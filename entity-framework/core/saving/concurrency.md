---
title: Обработка конфликтов параллелизма — EF Core
author: rowanmiller
ms.date: 03/03/2018
uid: core/saving/concurrency
ms.openlocfilehash: b72fa472698e76e18f155cf96b738b0e193eee0f
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/06/2019
ms.locfileid: "73654618"
---
# <a name="handling-concurrency-conflicts"></a><span data-ttu-id="00ac1-102">Обработка конфликтов параллелизма</span><span class="sxs-lookup"><span data-stu-id="00ac1-102">Handling Concurrency Conflicts</span></span>

> [!NOTE]
> <span data-ttu-id="00ac1-103">На этой странице описывается, как работает параллелизм в EF Core и как обрабатывать конфликты параллелизма в вашем приложении.</span><span class="sxs-lookup"><span data-stu-id="00ac1-103">This page documents how concurrency works in EF Core and how to handle concurrency conflicts in your application.</span></span> <span data-ttu-id="00ac1-104">Подробнее о том, как настроить маркеры параллелизма в вашей модели, см. в [этой статье](xref:core/modeling/concurrency).</span><span class="sxs-lookup"><span data-stu-id="00ac1-104">See [Concurrency Tokens](xref:core/modeling/concurrency) for details on how to configure concurrency tokens in your model.</span></span>

> [!TIP]
> <span data-ttu-id="00ac1-105">Для этой статьи вы можете скачать [пример](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Concurrency/) из репозитория GitHub.</span><span class="sxs-lookup"><span data-stu-id="00ac1-105">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Concurrency/) on GitHub.</span></span>

<span data-ttu-id="00ac1-106">_Параллелизм базы данных_ относится к ситуациям, в которых несколько процессов или пользователей получают доступ или одновременно изменяют одни и те же данные в базе данных.</span><span class="sxs-lookup"><span data-stu-id="00ac1-106">_Database concurrency_ refers to situations in which multiple processes or users access or change the same data in a database at the same time.</span></span> <span data-ttu-id="00ac1-107">_Управление параллелизмом_ относится к конкретным механизмам, используемым для обеспечения согласованности данных при наличии одновременных изменений.</span><span class="sxs-lookup"><span data-stu-id="00ac1-107">_Concurrency control_ refers to specific mechanisms used to ensure data consistency in presence of concurrent changes.</span></span>

<span data-ttu-id="00ac1-108">EF Core реализует _оптимистичный контроль параллелизма_, который позволяет нескольким процессам или пользователям самостоятельно вносить изменения без накладных расходов на синхронизацию или блокировку.</span><span class="sxs-lookup"><span data-stu-id="00ac1-108">EF Core implements _optimistic concurrency control_, meaning that it will let multiple processes or users make changes independently without the overhead of synchronization or locking.</span></span> <span data-ttu-id="00ac1-109">В идеальной ситуации эти изменения не будут мешать друг другу и, следовательно, смогут завершиться успешно.</span><span class="sxs-lookup"><span data-stu-id="00ac1-109">In the ideal situation, these changes will not interfere with each other and therefore will be able to succeed.</span></span> <span data-ttu-id="00ac1-110">В худшем случае два или более процесса будут пытаться вносить конфликтующие изменения, и только один из них сможет преуспеть.</span><span class="sxs-lookup"><span data-stu-id="00ac1-110">In the worst case scenario, two or more processes will attempt to make conflicting changes, and only one of them should succeed.</span></span>

## <a name="how-concurrency-control-works-in-ef-core"></a><span data-ttu-id="00ac1-111">Как работает управление параллелизмом в EF Core</span><span class="sxs-lookup"><span data-stu-id="00ac1-111">How concurrency control works in EF Core</span></span>

<span data-ttu-id="00ac1-112">Свойства, настроенные как маркеры параллелизма, используются для реализации оптимистического контроля параллелизма: всякий раз, когда операция обновления или удаления выполняется во время `SaveChanges`, значение маркера параллелизма в базе данных сравнивается с исходным значением, считанным EF Core.</span><span class="sxs-lookup"><span data-stu-id="00ac1-112">Properties configured as concurrency tokens are used to implement optimistic concurrency control: whenever an update or delete operation is performed during `SaveChanges`, the value of the concurrency token on the database is compared against the original value read by EF Core.</span></span>

- <span data-ttu-id="00ac1-113">Если значения совпадают, операция может завершиться.</span><span class="sxs-lookup"><span data-stu-id="00ac1-113">If the values match, the operation can complete.</span></span>
- <span data-ttu-id="00ac1-114">Если значения не совпадают, EF Core предполагает, что другой пользователь выполнил конфликтную операцию и прервал текущую транзакцию.</span><span class="sxs-lookup"><span data-stu-id="00ac1-114">If the values do not match, EF Core assumes that another user has performed a conflicting operation and aborts the current transaction.</span></span>

<span data-ttu-id="00ac1-115">Ситуация, когда другой пользователь выполнил операцию, конфликтующую с текущей операцией, называется _конфликтом параллелизма_.</span><span class="sxs-lookup"><span data-stu-id="00ac1-115">The situation when another user has performed an operation that conflicts with the current operation is known as _concurrency conflict_.</span></span>

<span data-ttu-id="00ac1-116">Поставщики баз данных отвечают за реализацию сравнения значений маркеров параллелизма.</span><span class="sxs-lookup"><span data-stu-id="00ac1-116">Database providers are responsible for implementing the comparison of concurrency token values.</span></span>

<span data-ttu-id="00ac1-117">В реляционных базах данных EF Core включает проверку значения маркера параллелизма в предложении `WHERE` любых инструкций с `UPDATE` или `DELETE`.</span><span class="sxs-lookup"><span data-stu-id="00ac1-117">On relational databases EF Core includes a check for the value of the concurrency token in the `WHERE` clause of any `UPDATE` or `DELETE` statements.</span></span> <span data-ttu-id="00ac1-118">После выполнения операторов EF Core считывает количество затронутых строк.</span><span class="sxs-lookup"><span data-stu-id="00ac1-118">After executing the statements, EF Core reads the number of rows that were affected.</span></span>

<span data-ttu-id="00ac1-119">Если никакие строки не затронуты, конфликт параллелизма обнаруживается и EF Core вызывает `DbUpdateConcurrencyException`.</span><span class="sxs-lookup"><span data-stu-id="00ac1-119">If no rows are affected, a concurrency conflict is detected, and EF Core throws `DbUpdateConcurrencyException`.</span></span>

<span data-ttu-id="00ac1-120">Например, мы можем настроить `LastName` в `Person` как маркер параллелизма.</span><span class="sxs-lookup"><span data-stu-id="00ac1-120">For example, we may want to configure `LastName` on `Person` to be a concurrency token.</span></span> <span data-ttu-id="00ac1-121">Тогда любая операция обновления для пользователя будет содержать проверку параллелизма в предложении `WHERE`:</span><span class="sxs-lookup"><span data-stu-id="00ac1-121">Then any update operation on Person will include the concurrency check in the `WHERE` clause:</span></span>

``` sql
UPDATE [Person] SET [FirstName] = @p1
WHERE [PersonId] = @p0 AND [LastName] = @p2;
```

## <a name="resolving-concurrency-conflicts"></a><span data-ttu-id="00ac1-122">Разрешение конфликтов параллелизма</span><span class="sxs-lookup"><span data-stu-id="00ac1-122">Resolving concurrency conflicts</span></span>

<span data-ttu-id="00ac1-123">Продолжая рассматривать предыдущий пример, если один пользователь пытается сохранить некоторые изменения в `Person`, но другой пользователь уже изменил `LastName`, будет вызвано исключение.</span><span class="sxs-lookup"><span data-stu-id="00ac1-123">Continuing with the previous example, if one user tries to save some changes to a `Person`, but another user has already changed the `LastName`, then an exception will be thrown.</span></span>

<span data-ttu-id="00ac1-124">На этом этапе приложение может просто проинформировать пользователя о том, что обновление не было успешным из-за противоречивых изменений, и продолжить выполнение.</span><span class="sxs-lookup"><span data-stu-id="00ac1-124">At this point, the application could simply inform the user that the update was not successful due to conflicting changes and move on.</span></span> <span data-ttu-id="00ac1-125">Но может быть желательно запросить пользователя убедиться, что запись по-прежнему представляет одного и того же фактического пользователя, и повторить операцию.</span><span class="sxs-lookup"><span data-stu-id="00ac1-125">But it may be desirable to prompt the user to ensure this record still represents the same actual person and to retry the operation.</span></span>

<span data-ttu-id="00ac1-126">Этот процесс является примером _разрешения конфликта параллелизма_.</span><span class="sxs-lookup"><span data-stu-id="00ac1-126">This process is an example of _resolving a concurrency conflict_.</span></span>

<span data-ttu-id="00ac1-127">Разрешение конфликта параллелизма включает в себя объединение ожидающих изменений из текущего контекста `DbContext` со значениями в базе данных.</span><span class="sxs-lookup"><span data-stu-id="00ac1-127">Resolving a concurrency conflict involves merging the pending changes from the current `DbContext` with the values in the database.</span></span> <span data-ttu-id="00ac1-128">Объединяемые значения отличаются в зависимости от приложения, и их может указать пользователь, введя входные данные.</span><span class="sxs-lookup"><span data-stu-id="00ac1-128">What values get merged will vary based on the application and may be directed by user input.</span></span>

<span data-ttu-id="00ac1-129">**Доступны три набора значений для разрешения конфликта параллелизма:**</span><span class="sxs-lookup"><span data-stu-id="00ac1-129">**There are three sets of values available to help resolve a concurrency conflict:**</span></span>

- <span data-ttu-id="00ac1-130">**Текущие значения** — это значения, которые приложение пыталось записать в базу данных.</span><span class="sxs-lookup"><span data-stu-id="00ac1-130">**Current values** are the values that the application was attempting to write to the database.</span></span>
- <span data-ttu-id="00ac1-131">**Исходные значения** — это значения, которые изначально были получены из базы данных до того, как были сделаны какие-либо изменения.</span><span class="sxs-lookup"><span data-stu-id="00ac1-131">**Original values** are the values that were originally retrieved from the database, before any edits were made.</span></span>
- <span data-ttu-id="00ac1-132">**Значения базы данных** — это значения, которые в настоящее время хранятся в базе данных.</span><span class="sxs-lookup"><span data-stu-id="00ac1-132">**Database values** are the values currently stored in the database.</span></span>

<span data-ttu-id="00ac1-133">Общий подход к обработке конфликтов параллелизма:</span><span class="sxs-lookup"><span data-stu-id="00ac1-133">The general approach to handle a concurrency conflicts is:</span></span>

1. <span data-ttu-id="00ac1-134">Получите `DbUpdateConcurrencyException` в процессе `SaveChanges`.</span><span class="sxs-lookup"><span data-stu-id="00ac1-134">Catch `DbUpdateConcurrencyException` during `SaveChanges`.</span></span>
2. <span data-ttu-id="00ac1-135">Используйте `DbUpdateConcurrencyException.Entries`, чтобы подготовить новый набор изменений для затронутых объектов.</span><span class="sxs-lookup"><span data-stu-id="00ac1-135">Use `DbUpdateConcurrencyException.Entries` to prepare a new set of changes for the affected entities.</span></span>
3. <span data-ttu-id="00ac1-136">Обновите исходные значения маркера параллелизма, чтобы отразить текущие значения в базе данных.</span><span class="sxs-lookup"><span data-stu-id="00ac1-136">Refresh the original values of the concurrency token to reflect the current values in the database.</span></span>
4. <span data-ttu-id="00ac1-137">Повторяйте процесс, пока не возникнут конфликты.</span><span class="sxs-lookup"><span data-stu-id="00ac1-137">Retry the process until no conflicts occur.</span></span>

<span data-ttu-id="00ac1-138">В следующем примере `Person.FirstName` и `Person.LastName` устанавливаются как маркеры параллелизма.</span><span class="sxs-lookup"><span data-stu-id="00ac1-138">In the following example, `Person.FirstName` and `Person.LastName` are setup as concurrency tokens.</span></span> <span data-ttu-id="00ac1-139">В месте, где вы указываете конкретную логику приложения, есть комментарий `// TODO:`, позволяющий выбрать значение, которое нужно сохранить.</span><span class="sxs-lookup"><span data-stu-id="00ac1-139">There is a `// TODO:` comment in the location where you include application specific logic to choose the value to be saved.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Concurrency/Sample.cs?name=ConcurrencyHandlingCode&highlight=34-35)]
