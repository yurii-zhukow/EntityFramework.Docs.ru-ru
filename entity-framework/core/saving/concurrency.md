---
title: "Обработка конфликтов параллелизма - EF Core"
author: rowanmiller
ms.author: divega
ms.date: 03/03/2018
ms.technology: entity-framework-core
uid: core/saving/concurrency
ms.openlocfilehash: 288d9c6fced5ebbaa2c366248c68547502c3698e
ms.sourcegitcommit: 8f3be0a2a394253efb653388ec66bda964e5ee1b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/05/2018
---
# <a name="handling-concurrency-conflicts"></a><span data-ttu-id="38253-102">Обработка конфликтов параллелизма</span><span class="sxs-lookup"><span data-stu-id="38253-102">Handling Concurrency Conflicts</span></span>

> [!NOTE]
> <span data-ttu-id="38253-103">Этой странице документах как параллелизма работает в ядре EF и способов обработки конфликтов параллелизма в приложении.</span><span class="sxs-lookup"><span data-stu-id="38253-103">This page documents how concurrency works in EF Core and how to handle concurrency conflicts in your application.</span></span> <span data-ttu-id="38253-104">В разделе [маркеры параллелизма](xref:core/modeling/concurrency) подробные сведения о настройке маркеры параллелизма в модели.</span><span class="sxs-lookup"><span data-stu-id="38253-104">See [Concurrency Tokens](xref:core/modeling/concurrency) for details on how to configure concurrency tokens in your model.</span></span>

> [!TIP]
> <span data-ttu-id="38253-105">Для этой статьи вы можете скачать [пример](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/Concurrency/) из репозитория GitHub.</span><span class="sxs-lookup"><span data-stu-id="38253-105">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/Concurrency/) on GitHub.</span></span>

<span data-ttu-id="38253-106">_Параллелизм баз данных_ относится к ситуации, в которых несколько процессов или пользователям доступ, или изменить те же данные в базе данных в то же время.</span><span class="sxs-lookup"><span data-stu-id="38253-106">_Database concurrency_ refers to situations in which multiple processes or users access or change the same data in a database at the same time.</span></span> <span data-ttu-id="38253-107">_Управление параллелизмом_ ссылается на определенные механизмами для обеспечения согласованности данных при наличии параллельных изменений.</span><span class="sxs-lookup"><span data-stu-id="38253-107">_Concurrency control_ refers to specific mechanisms used to ensure data consistency in presence of concurrent changes.</span></span>

<span data-ttu-id="38253-108">Реализует основные EF _управления оптимистичным параллелизмом_, означающего, что оно позволит несколько процессов и пользователей изменять независимо друг от друга без необходимости проведения синхронизации или блокировки.</span><span class="sxs-lookup"><span data-stu-id="38253-108">EF Core implements _optimistic concurrency control_, meaning that it will let multiple processes or users make changes independently without the overhead of synchronization or locking.</span></span> <span data-ttu-id="38253-109">В идеальном случае эти изменения не будут конфликтовать друг с другом и поэтому сможет завершиться успешно.</span><span class="sxs-lookup"><span data-stu-id="38253-109">In the ideal situation, these changes will not interfere with each other and therefore will be able to succeed.</span></span> <span data-ttu-id="38253-110">В худшем случае двух или более процессов попытается установить конфликтующих изменений, и только один из них должны выполняться успешно.</span><span class="sxs-lookup"><span data-stu-id="38253-110">In the worst case scenario, two or more processes will attempt to make conflicting changes, and only one of them should succeed.</span></span>

## <a name="how-concurrency-control-works-in-ef-core"></a><span data-ttu-id="38253-111">Как работает управление параллелизмом в EF Core</span><span class="sxs-lookup"><span data-stu-id="38253-111">How concurrency control works in EF Core</span></span>

<span data-ttu-id="38253-112">Свойства, настроенные как маркеры параллелизма используются для реализации управления оптимистичным параллелизмом: каждый раз, когда выполняется операция обновления или удаления во время `SaveChanges`, маркер параллелизма для базы данных сравнивается с исходным значение, считываемых EF Core.</span><span class="sxs-lookup"><span data-stu-id="38253-112">Properties configured as concurrency tokens are used to implement optimistic concurrency control: whenever an update or delete operation is performed during `SaveChanges`, the value of the concurrency token on the database is compared against the original value read by EF Core.</span></span>

- <span data-ttu-id="38253-113">Если значения совпадают, операция может завершиться.</span><span class="sxs-lookup"><span data-stu-id="38253-113">If the values match, the operation can complete.</span></span>
- <span data-ttu-id="38253-114">Если значения не совпадают, EF Core предполагается, что другой пользователь выполнил конфликтует с операцией и прерывает текущую транзакцию.</span><span class="sxs-lookup"><span data-stu-id="38253-114">If the values do not match, EF Core assumes that another user has performed a conflicting operation and aborts the current transaction.</span></span>

<span data-ttu-id="38253-115">Ситуации, когда другой пользователь выполнил операцию, которая конфликтует с текущей операцией называется _конфликт параллелизма_.</span><span class="sxs-lookup"><span data-stu-id="38253-115">The situation when another user has performed an operation that conflicts with the current operation is known as _concurrency conflict_.</span></span>

<span data-ttu-id="38253-116">Поставщики базы данных — за реализацию сравнения значений маркера параллелизма.</span><span class="sxs-lookup"><span data-stu-id="38253-116">Database providers are responsible for implementing the comparison of concurrency token values.</span></span>

<span data-ttu-id="38253-117">В реляционных базах данных EF Core включает проверку для значения маркера параллелизма в `WHERE` предложение любого `UPDATE` или `DELETE` инструкции.</span><span class="sxs-lookup"><span data-stu-id="38253-117">On relational databases EF Core includes a check for the value of the concurrency token in the `WHERE` clause of any `UPDATE` or `DELETE` statements.</span></span> <span data-ttu-id="38253-118">После выполнения инструкций, EF Core считывает число строк, которые были затронуты.</span><span class="sxs-lookup"><span data-stu-id="38253-118">After executing the statements, EF Core reads the number of rows that were affected.</span></span>

<span data-ttu-id="38253-119">Если не задействована, обнаруживается конфликт параллелизма и вызывает EF Core `DbUpdateConcurrencyException`.</span><span class="sxs-lookup"><span data-stu-id="38253-119">If no rows are affected, a concurrency conflict is detected, and EF Core throws `DbUpdateConcurrencyException`.</span></span>

<span data-ttu-id="38253-120">Например, следует настроить `LastName` на `Person` быть маркера параллелизма.</span><span class="sxs-lookup"><span data-stu-id="38253-120">For example, we may want to configure `LastName` on `Person` to be a concurrency token.</span></span> <span data-ttu-id="38253-121">Все операции обновления на пользователя будет включать проверка параллелизма в `WHERE` предложения:</span><span class="sxs-lookup"><span data-stu-id="38253-121">Then any update operation on Person will include the concurrency check in the `WHERE` clause:</span></span>

``` sql
UPDATE [Person] SET [FirstName] = @p1
WHERE [PersonId] = @p0 AND [LastName] = @p2;
```

## <a name="resolving-concurrency-conflicts"></a><span data-ttu-id="38253-122">Разрешение конфликтов параллелизма</span><span class="sxs-lookup"><span data-stu-id="38253-122">Resolving concurrency conflicts</span></span>

<span data-ttu-id="38253-123">Продолжение предыдущего примера, если один пользователь пытается сохранить некоторые изменения в `Person`, но уже была изменена другим пользователем `LastName` будет вызвано исключение.</span><span class="sxs-lookup"><span data-stu-id="38253-123">Continuing with the previous example, if one user tries to save some changes to a `Person`, but another user has already changed the `LastName` the an exception will be thrown.</span></span>

<span data-ttu-id="38253-124">На этом этапе приложение может просто информировать пользователей о том, что обновление не выполнена из-за конфликтующих изменений и перейти.</span><span class="sxs-lookup"><span data-stu-id="38253-124">At this point, the application could simply inform the user that the update was not successful due to conflicting changes and move on.</span></span> <span data-ttu-id="38253-125">Однако она может возникнуть необходимость запрашивать пользователя Убедитесь, что эта запись по-прежнему представляет фактический неизменность и повторите операцию.</span><span class="sxs-lookup"><span data-stu-id="38253-125">But it may be desirable to prompt the user to ensure this record still represents the same actual person and to retry the operation.</span></span>

<span data-ttu-id="38253-126">Этот процесс является примером _разрешение конфликтов параллелизма_.</span><span class="sxs-lookup"><span data-stu-id="38253-126">This process is an example of _resolving a concurrency conflict_.</span></span>

<span data-ttu-id="38253-127">Разрешение конфликтов параллелизма включает в себя объединение ожидающие обработки изменения из текущего `DbContext` со значениями в базе данных.</span><span class="sxs-lookup"><span data-stu-id="38253-127">Resolving a concurrency conflict involves merging the pending changes from the current `DbContext` with the values in the database.</span></span> <span data-ttu-id="38253-128">Объединить какие значения будут зависеть от приложения и могут быть перенаправлены вводимыми пользователем.</span><span class="sxs-lookup"><span data-stu-id="38253-128">What values get merged will vary based on the application and may be directed by user input.</span></span>

<span data-ttu-id="38253-129">**Существуют три набора значений, который помогает разрешить конфликт параллелизма.**</span><span class="sxs-lookup"><span data-stu-id="38253-129">**There are three sets of values available to help resolve a concurrency conflict:**</span></span>

* <span data-ttu-id="38253-130">**Текущие значения** — это значения, при попытке приложения записать в базу данных.</span><span class="sxs-lookup"><span data-stu-id="38253-130">**Current values** are the values that the application was attempting to write to the database.</span></span>

* <span data-ttu-id="38253-131">**Исходные значения** — это значения, изначально извлеченные из базы данных, перед были внесены изменения.</span><span class="sxs-lookup"><span data-stu-id="38253-131">**Original values** are the values that were originally retrieved from the database, before any edits were made.</span></span>

* <span data-ttu-id="38253-132">**Базы данных значения** являются значения, которые хранятся в базе данных.</span><span class="sxs-lookup"><span data-stu-id="38253-132">**Database values** are the values currently stored in the database.</span></span>

<span data-ttu-id="38253-133">— Общий подход для обработки конфликтов параллелизма:</span><span class="sxs-lookup"><span data-stu-id="38253-133">The general approach to handle a concurrency conflicts is:</span></span>

1. <span data-ttu-id="38253-134">Catch `DbUpdateConcurrencyException` во время `SaveChanges`.</span><span class="sxs-lookup"><span data-stu-id="38253-134">Catch `DbUpdateConcurrencyException` during `SaveChanges`.</span></span>
2. <span data-ttu-id="38253-135">Используйте `DbUpdateConcurrencyException.Entries` подготовить новый набор изменений для затронутых объектов.</span><span class="sxs-lookup"><span data-stu-id="38253-135">Use `DbUpdateConcurrencyException.Entries` to prepare a new set of changes for the affected entities.</span></span>
3. <span data-ttu-id="38253-136">Обновите исходные значения маркера параллелизма в соответствии с текущими значениями в базе данных.</span><span class="sxs-lookup"><span data-stu-id="38253-136">Refresh the original values of the concurrency token to reflect the current values in the database.</span></span>
4. <span data-ttu-id="38253-137">Повторите процесс, пока не возникают.</span><span class="sxs-lookup"><span data-stu-id="38253-137">Retry the process until no conflicts occur.</span></span>

<span data-ttu-id="38253-138">В следующем примере `Person.FirstName` и `Person.LastName` маркеры параллелизма, которые являются программы установки.</span><span class="sxs-lookup"><span data-stu-id="38253-138">In the following example, `Person.FirstName` and `Person.LastName` are setup as concurrency tokens.</span></span> <span data-ttu-id="38253-139">Отсутствует `// TODO:` комментария в расположении, где содержат определенную логику приложения выберите значение для сохранения.</span><span class="sxs-lookup"><span data-stu-id="38253-139">There is a `// TODO:` comment in the location where you include application specific logic to choose the value to be saved.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Concurrency/Sample.cs?name=ConcurrencyHandlingCode&highlight=34-35)]
