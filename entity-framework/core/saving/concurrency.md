---
title: Обработка конфликтов параллелизма — EF Core
description: Управление конфликтами при одновременном обновлении одних и тех же данных при использовании Entity Framework Core
author: ajcvickers
ms.date: 03/03/2018
uid: core/saving/concurrency
ms.openlocfilehash: 290fc3f68e71e4112891a4963f361506e95db17c
ms.sourcegitcommit: 4798ab8d04c1fdbe6dd204d94d770fcbf309d09b
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/11/2021
ms.locfileid: "103023553"
---
# <a name="handling-concurrency-conflicts"></a><span data-ttu-id="8c956-103">Обработка конфликтов параллелизма</span><span class="sxs-lookup"><span data-stu-id="8c956-103">Handling Concurrency Conflicts</span></span>

> [!NOTE]
> <span data-ttu-id="8c956-104">На этой странице описывается, как работает параллелизм в EF Core и как обрабатывать конфликты параллелизма в вашем приложении.</span><span class="sxs-lookup"><span data-stu-id="8c956-104">This page documents how concurrency works in EF Core and how to handle concurrency conflicts in your application.</span></span> <span data-ttu-id="8c956-105">Подробнее о том, как настроить маркеры параллелизма в вашей модели, см. в [этой статье](xref:core/modeling/concurrency).</span><span class="sxs-lookup"><span data-stu-id="8c956-105">See [Concurrency Tokens](xref:core/modeling/concurrency) for details on how to configure concurrency tokens in your model.</span></span>

> [!TIP]
> <span data-ttu-id="8c956-106">Для этой статьи вы можете скачать [пример](https://github.com/dotnet/EntityFramework.Docs/tree/main/samples/core/Saving/Concurrency/) из репозитория GitHub.</span><span class="sxs-lookup"><span data-stu-id="8c956-106">You can view this article's [sample](https://github.com/dotnet/EntityFramework.Docs/tree/main/samples/core/Saving/Concurrency/) on GitHub.</span></span>

<span data-ttu-id="8c956-107">_Параллелизм базы данных_ относится к ситуациям, в которых несколько процессов или пользователей получают доступ или одновременно изменяют одни и те же данные в базе данных.</span><span class="sxs-lookup"><span data-stu-id="8c956-107">_Database concurrency_ refers to situations in which multiple processes or users access or change the same data in a database at the same time.</span></span> <span data-ttu-id="8c956-108">_Управление параллелизмом_ относится к конкретным механизмам, используемым для обеспечения согласованности данных при наличии одновременных изменений.</span><span class="sxs-lookup"><span data-stu-id="8c956-108">_Concurrency control_ refers to specific mechanisms used to ensure data consistency in presence of concurrent changes.</span></span>

<span data-ttu-id="8c956-109">EF Core реализует _оптимистичный контроль параллелизма_, который позволяет нескольким процессам или пользователям самостоятельно вносить изменения без накладных расходов на синхронизацию или блокировку.</span><span class="sxs-lookup"><span data-stu-id="8c956-109">EF Core implements _optimistic concurrency control_, meaning that it will let multiple processes or users make changes independently without the overhead of synchronization or locking.</span></span> <span data-ttu-id="8c956-110">В идеальной ситуации эти изменения не будут мешать друг другу и, следовательно, смогут завершиться успешно.</span><span class="sxs-lookup"><span data-stu-id="8c956-110">In the ideal situation, these changes will not interfere with each other and therefore will be able to succeed.</span></span> <span data-ttu-id="8c956-111">В худшем случае два или более процесса будут пытаться вносить конфликтующие изменения, и только один из них сможет преуспеть.</span><span class="sxs-lookup"><span data-stu-id="8c956-111">In the worst case scenario, two or more processes will attempt to make conflicting changes, and only one of them should succeed.</span></span>

## <a name="how-concurrency-control-works-in-ef-core"></a><span data-ttu-id="8c956-112">Как работает управление параллелизмом в EF Core</span><span class="sxs-lookup"><span data-stu-id="8c956-112">How concurrency control works in EF Core</span></span>

<span data-ttu-id="8c956-113">Свойства, настроенные как маркеры параллелизма, используются для реализации оптимистического контроля параллелизма: всякий раз, когда операция обновления или удаления выполняется во время `SaveChanges`, значение маркера параллелизма в базе данных сравнивается с исходным значением, считанным EF Core.</span><span class="sxs-lookup"><span data-stu-id="8c956-113">Properties configured as concurrency tokens are used to implement optimistic concurrency control: whenever an update or delete operation is performed during `SaveChanges`, the value of the concurrency token on the database is compared against the original value read by EF Core.</span></span>

- <span data-ttu-id="8c956-114">Если значения совпадают, операция может завершиться.</span><span class="sxs-lookup"><span data-stu-id="8c956-114">If the values match, the operation can complete.</span></span>
- <span data-ttu-id="8c956-115">Если значения не совпадают, EF Core предполагает, что другой пользователь выполнил конфликтную операцию и прервал текущую транзакцию.</span><span class="sxs-lookup"><span data-stu-id="8c956-115">If the values do not match, EF Core assumes that another user has performed a conflicting operation and aborts the current transaction.</span></span>

<span data-ttu-id="8c956-116">Ситуация, когда другой пользователь выполнил операцию, конфликтующую с текущей операцией, называется _конфликтом параллелизма_.</span><span class="sxs-lookup"><span data-stu-id="8c956-116">The situation when another user has performed an operation that conflicts with the current operation is known as _concurrency conflict_.</span></span>

<span data-ttu-id="8c956-117">Поставщики баз данных отвечают за реализацию сравнения значений маркеров параллелизма.</span><span class="sxs-lookup"><span data-stu-id="8c956-117">Database providers are responsible for implementing the comparison of concurrency token values.</span></span>

<span data-ttu-id="8c956-118">В реляционных базах данных EF Core включает проверку значения маркера параллелизма в предложении `WHERE` любых инструкций с `UPDATE` или `DELETE`.</span><span class="sxs-lookup"><span data-stu-id="8c956-118">On relational databases EF Core includes a check for the value of the concurrency token in the `WHERE` clause of any `UPDATE` or `DELETE` statements.</span></span> <span data-ttu-id="8c956-119">После выполнения операторов EF Core считывает количество затронутых строк.</span><span class="sxs-lookup"><span data-stu-id="8c956-119">After executing the statements, EF Core reads the number of rows that were affected.</span></span>

<span data-ttu-id="8c956-120">Если никакие строки не затронуты, конфликт параллелизма обнаруживается и EF Core вызывает `DbUpdateConcurrencyException`.</span><span class="sxs-lookup"><span data-stu-id="8c956-120">If no rows are affected, a concurrency conflict is detected, and EF Core throws `DbUpdateConcurrencyException`.</span></span>

<span data-ttu-id="8c956-121">Например, мы можем настроить `LastName` в `Person` как маркер параллелизма.</span><span class="sxs-lookup"><span data-stu-id="8c956-121">For example, we may want to configure `LastName` on `Person` to be a concurrency token.</span></span> <span data-ttu-id="8c956-122">Тогда любая операция обновления для пользователя будет содержать проверку параллелизма в предложении `WHERE`:</span><span class="sxs-lookup"><span data-stu-id="8c956-122">Then any update operation on Person will include the concurrency check in the `WHERE` clause:</span></span>

```sql
UPDATE [Person] SET [FirstName] = @p1
WHERE [PersonId] = @p0 AND [LastName] = @p2;
```

## <a name="resolving-concurrency-conflicts"></a><span data-ttu-id="8c956-123">Разрешение конфликтов параллелизма</span><span class="sxs-lookup"><span data-stu-id="8c956-123">Resolving concurrency conflicts</span></span>

<span data-ttu-id="8c956-124">Продолжая рассматривать предыдущий пример, если один пользователь пытается сохранить некоторые изменения в `Person`, но другой пользователь уже изменил `LastName`, будет вызвано исключение.</span><span class="sxs-lookup"><span data-stu-id="8c956-124">Continuing with the previous example, if one user tries to save some changes to a `Person`, but another user has already changed the `LastName`, then an exception will be thrown.</span></span>

<span data-ttu-id="8c956-125">На этом этапе приложение может просто проинформировать пользователя о том, что обновление не было успешным из-за противоречивых изменений, и продолжить выполнение.</span><span class="sxs-lookup"><span data-stu-id="8c956-125">At this point, the application could simply inform the user that the update was not successful due to conflicting changes and move on.</span></span> <span data-ttu-id="8c956-126">Но может быть желательно запросить пользователя убедиться, что запись по-прежнему представляет одного и того же фактического пользователя, и повторить операцию.</span><span class="sxs-lookup"><span data-stu-id="8c956-126">But it may be desirable to prompt the user to ensure this record still represents the same actual person and to retry the operation.</span></span>

<span data-ttu-id="8c956-127">Этот процесс является примером _разрешения конфликта параллелизма_.</span><span class="sxs-lookup"><span data-stu-id="8c956-127">This process is an example of _resolving a concurrency conflict_.</span></span>

<span data-ttu-id="8c956-128">Разрешение конфликта параллелизма включает в себя объединение ожидающих изменений из текущего контекста `DbContext` со значениями в базе данных.</span><span class="sxs-lookup"><span data-stu-id="8c956-128">Resolving a concurrency conflict involves merging the pending changes from the current `DbContext` with the values in the database.</span></span> <span data-ttu-id="8c956-129">Объединяемые значения отличаются в зависимости от приложения, и их может указать пользователь, введя входные данные.</span><span class="sxs-lookup"><span data-stu-id="8c956-129">What values get merged will vary based on the application and may be directed by user input.</span></span>

<span data-ttu-id="8c956-130">**Доступны три набора значений для разрешения конфликта параллелизма:**</span><span class="sxs-lookup"><span data-stu-id="8c956-130">**There are three sets of values available to help resolve a concurrency conflict:**</span></span>

- <span data-ttu-id="8c956-131">**Текущие значения** — это значения, которые приложение пыталось записать в базу данных.</span><span class="sxs-lookup"><span data-stu-id="8c956-131">**Current values** are the values that the application was attempting to write to the database.</span></span>
- <span data-ttu-id="8c956-132">**Исходные значения** — это значения, которые изначально были получены из базы данных до того, как были сделаны какие-либо изменения.</span><span class="sxs-lookup"><span data-stu-id="8c956-132">**Original values** are the values that were originally retrieved from the database, before any edits were made.</span></span>
- <span data-ttu-id="8c956-133">**Значения базы данных** — это значения, которые в настоящее время хранятся в базе данных.</span><span class="sxs-lookup"><span data-stu-id="8c956-133">**Database values** are the values currently stored in the database.</span></span>

<span data-ttu-id="8c956-134">Общий подход к обработке конфликтов параллелизма:</span><span class="sxs-lookup"><span data-stu-id="8c956-134">The general approach to handle a concurrency conflicts is:</span></span>

1. <span data-ttu-id="8c956-135">Получите `DbUpdateConcurrencyException` в процессе `SaveChanges`.</span><span class="sxs-lookup"><span data-stu-id="8c956-135">Catch `DbUpdateConcurrencyException` during `SaveChanges`.</span></span>
2. <span data-ttu-id="8c956-136">Используйте `DbUpdateConcurrencyException.Entries`, чтобы подготовить новый набор изменений для затронутых объектов.</span><span class="sxs-lookup"><span data-stu-id="8c956-136">Use `DbUpdateConcurrencyException.Entries` to prepare a new set of changes for the affected entities.</span></span>
3. <span data-ttu-id="8c956-137">Обновите исходные значения маркера параллелизма, чтобы отразить текущие значения в базе данных.</span><span class="sxs-lookup"><span data-stu-id="8c956-137">Refresh the original values of the concurrency token to reflect the current values in the database.</span></span>
4. <span data-ttu-id="8c956-138">Повторяйте процесс, пока не возникнут конфликты.</span><span class="sxs-lookup"><span data-stu-id="8c956-138">Retry the process until no conflicts occur.</span></span>

<span data-ttu-id="8c956-139">В следующем примере `Person.FirstName` и `Person.LastName` устанавливаются как маркеры параллелизма.</span><span class="sxs-lookup"><span data-stu-id="8c956-139">In the following example, `Person.FirstName` and `Person.LastName` are set up as concurrency tokens.</span></span> <span data-ttu-id="8c956-140">В месте, где вы указываете конкретную логику приложения, есть комментарий `// TODO:`, позволяющий выбрать значение, которое нужно сохранить.</span><span class="sxs-lookup"><span data-stu-id="8c956-140">There is a `// TODO:` comment in the location where you include application specific logic to choose the value to be saved.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Concurrency/Sample.cs?name=ConcurrencyHandlingCode&highlight=33-34)]
