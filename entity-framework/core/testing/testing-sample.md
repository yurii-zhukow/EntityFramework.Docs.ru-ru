---
title: Пример EF Core тестирования — EF Core
description: Пример, демонстрирующий тестирование приложений, использующих Entity Framework Core
author: ajcvickers
ms.date: 04/22/2020
uid: core/testing/testing-sample
no-loc:
- ':::no-loc(Item):::'
- ':::no-loc(Tag):::'
- ':::no-loc(Items):::'
- ':::no-loc(Tags):::'
- ':::no-loc(items):::'
- ':::no-loc(tags):::'
ms.openlocfilehash: 9666bbde8ae9608dcebbea3ad37c51883960a942
ms.sourcegitcommit: f3512e3a98e685a3ba409c1d0157ce85cc390cf4
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2020
ms.locfileid: "94431371"
---
# <a name="ef-core-testing-sample"></a><span data-ttu-id="9c81d-103">Пример тестирования EF Core</span><span class="sxs-lookup"><span data-stu-id="9c81d-103">EF Core testing sample</span></span>

> [!TIP]
> <span data-ttu-id="9c81d-104">Код в этом документе можно найти на сайте GitHub в качестве [готового к запуску примера](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Testing/:::no-loc(Items):::WebApi/).</span><span class="sxs-lookup"><span data-stu-id="9c81d-104">The code in this document can be found on GitHub as a [runnable sample](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Testing/:::no-loc(Items):::WebApi/).</span></span>
> <span data-ttu-id="9c81d-105">Обратите внимание, что некоторые из этих тестов должны **завершаться ошибкой**.</span><span class="sxs-lookup"><span data-stu-id="9c81d-105">Note that some of these tests **are expected to fail**.</span></span> <span data-ttu-id="9c81d-106">Причины этого описаны ниже.</span><span class="sxs-lookup"><span data-stu-id="9c81d-106">The reasons for this are explained below.</span></span>

<span data-ttu-id="9c81d-107">В этом документе рассматривается пример для тестирования кода, который использует EF Core.</span><span class="sxs-lookup"><span data-stu-id="9c81d-107">This doc walks through a sample for testing code that uses EF Core.</span></span>

## <a name="the-application"></a><span data-ttu-id="9c81d-108">Приложение</span><span class="sxs-lookup"><span data-stu-id="9c81d-108">The application</span></span>

<span data-ttu-id="9c81d-109">[Пример](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Testing/:::no-loc(Items):::WebApi/) содержит два проекта:</span><span class="sxs-lookup"><span data-stu-id="9c81d-109">The [sample](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Testing/:::no-loc(Items):::WebApi/) contains two projects:</span></span>

- <span data-ttu-id="9c81d-110">:::no-loc(Items):::WebApi: очень простой [веб-API, поддерживаемый ASP.NET Core](/aspnet/core/tutorials/first-web-api) с одним контроллером</span><span class="sxs-lookup"><span data-stu-id="9c81d-110">:::no-loc(Items):::WebApi: A very simple [Web API backed by ASP.NET Core](/aspnet/core/tutorials/first-web-api) with a single controller</span></span>
- <span data-ttu-id="9c81d-111">Тесты: тестовый проект [xUnit](https://xunit.net/) для тестирования контроллера</span><span class="sxs-lookup"><span data-stu-id="9c81d-111">Tests: An [XUnit](https://xunit.net/) test project to test the controller</span></span>

### <a name="the-model-and-business-rules"></a><span data-ttu-id="9c81d-112">Модель и бизнес-правила</span><span class="sxs-lookup"><span data-stu-id="9c81d-112">The model and business rules</span></span>

<span data-ttu-id="9c81d-113">Модель, которая включает этот API, имеет два типа сущностей: :::no-loc(Items)::: и :::no-loc(Tags)::: .</span><span class="sxs-lookup"><span data-stu-id="9c81d-113">The model backing this API has two entity types: :::no-loc(Items)::: and :::no-loc(Tags):::.</span></span>

- <span data-ttu-id="9c81d-114">:::no-loc(Items)::: зависят от регистра имя и коллекцию :::no-loc(Tags)::: .</span><span class="sxs-lookup"><span data-stu-id="9c81d-114">:::no-loc(Items)::: have a case-sensitive name and a collection of :::no-loc(Tags):::.</span></span>
- <span data-ttu-id="9c81d-115">Каждый :::no-loc(Tag)::: имеет метку и число, представляющее количество применений к :::no-loc(Item)::: .</span><span class="sxs-lookup"><span data-stu-id="9c81d-115">Each :::no-loc(Tag)::: has a label and a count representing the number of times it has been applied to the :::no-loc(Item):::.</span></span>
- <span data-ttu-id="9c81d-116">Каждый :::no-loc(Item)::: должен иметь только один :::no-loc(Tag)::: с заданной меткой.</span><span class="sxs-lookup"><span data-stu-id="9c81d-116">Each :::no-loc(Item)::: should only have one :::no-loc(Tag)::: with a given label.</span></span>
  - <span data-ttu-id="9c81d-117">Если элемент помечен одной и той же меткой более одного раза, то число в существующем теге с этой меткой увеличивается вместо создания нового тега.</span><span class="sxs-lookup"><span data-stu-id="9c81d-117">If an item is tagged with the same label more than once, then the count on the existing tag with that label is incremented instead of a new tag being created.</span></span>
- <span data-ttu-id="9c81d-118">Удаление :::no-loc(Item)::: должно удалить все связанные :::no-loc(Tags)::: .</span><span class="sxs-lookup"><span data-stu-id="9c81d-118">Deleting an :::no-loc(Item)::: should delete all associated :::no-loc(Tags):::.</span></span>

#### <a name="the-no-locitem-entity-type"></a><span data-ttu-id="9c81d-119">:::no-loc(Item):::Тип сущности</span><span class="sxs-lookup"><span data-stu-id="9c81d-119">The :::no-loc(Item)::: entity type</span></span>

<span data-ttu-id="9c81d-120">`:::no-loc(Item):::`Тип сущности:</span><span class="sxs-lookup"><span data-stu-id="9c81d-120">The `:::no-loc(Item):::` entity type:</span></span>

[!code-csharp[:::no-loc(Item):::EntityType](../../../samples/core/Miscellaneous/Testing/:::no-loc(Items):::WebApi/:::no-loc(Items):::WebApi/:::no-loc(Item):::.cs?name=:::no-loc(Item):::EntityType)]

<span data-ttu-id="9c81d-121">И его конфигурация в `DbContext.OnModelCreating` :</span><span class="sxs-lookup"><span data-stu-id="9c81d-121">And its configuration in `DbContext.OnModelCreating`:</span></span>

[!code-csharp[Configure:::no-loc(Item):::](../../../samples/core/Miscellaneous/Testing/:::no-loc(Items):::WebApi/:::no-loc(Items):::WebApi/:::no-loc(Items):::Context.cs?name=Configure:::no-loc(Item):::)]

<span data-ttu-id="9c81d-122">Обратите внимание, что тип сущности ограничивается способом, который можно использовать для отражения модели предметной области и бизнес-правил.</span><span class="sxs-lookup"><span data-stu-id="9c81d-122">Notice that entity type constrains the way it can be used to reflect the domain model and business rules.</span></span> <span data-ttu-id="9c81d-123">В частности:</span><span class="sxs-lookup"><span data-stu-id="9c81d-123">In particular:</span></span>

- <span data-ttu-id="9c81d-124">Первичный ключ сопоставляется непосредственно с `_id` полем и не предоставляется публично</span><span class="sxs-lookup"><span data-stu-id="9c81d-124">The primary key is mapped directly to the `_id` field and not exposed publicly</span></span>
  - <span data-ttu-id="9c81d-125">EF обнаруживает и использует закрытый конструктор, принимающий значение и имя первичного ключа.</span><span class="sxs-lookup"><span data-stu-id="9c81d-125">EF detects and uses the private constructor accepting the primary key value and name.</span></span>
- <span data-ttu-id="9c81d-126">`Name`Свойство доступно только для чтения и задается только в конструкторе.</span><span class="sxs-lookup"><span data-stu-id="9c81d-126">The `Name` property is read-only and set only in the constructor.</span></span>
- <span data-ttu-id="9c81d-127">:::no-loc(Tags)::: предоставляются в виде, `IReadOnlyList<:::no-loc(Tag):::>` чтобы предотвратить произвольное изменение.</span><span class="sxs-lookup"><span data-stu-id="9c81d-127">:::no-loc(Tags)::: are exposed as a `IReadOnlyList<:::no-loc(Tag):::>` to prevent arbitrary modification.</span></span>
  - <span data-ttu-id="9c81d-128">EF связывает `:::no-loc(Tags):::` свойство с `_:::no-loc(tags):::` резервным полем, сопоставляя их имена.</span><span class="sxs-lookup"><span data-stu-id="9c81d-128">EF associates the `:::no-loc(Tags):::` property with the `_:::no-loc(tags):::` backing field by matching their names.</span></span>
  - <span data-ttu-id="9c81d-129">`Add:::no-loc(Tag):::`Метод принимает метку тега и реализует бизнес-правило, описанное выше.</span><span class="sxs-lookup"><span data-stu-id="9c81d-129">The `Add:::no-loc(Tag):::` method takes a tag label and implements the business rule described above.</span></span>
    <span data-ttu-id="9c81d-130">То есть, тег добавляется только для новых меток.</span><span class="sxs-lookup"><span data-stu-id="9c81d-130">That is, a tag is only added for new labels.</span></span>
    <span data-ttu-id="9c81d-131">В противном случае значение счетчика в существующей метке увеличивается.</span><span class="sxs-lookup"><span data-stu-id="9c81d-131">Otherwise the count on an existing label is incremented.</span></span>
- <span data-ttu-id="9c81d-132">`:::no-loc(Tags):::`Свойство навигации настроено для связи "многие к одному"</span><span class="sxs-lookup"><span data-stu-id="9c81d-132">The `:::no-loc(Tags):::` navigation property is configured for a many-to-one relationship</span></span>
  - <span data-ttu-id="9c81d-133">Нет необходимости в свойстве навигации из :::no-loc(Tag)::: :::no-loc(Item)::: , поэтому оно не включено.</span><span class="sxs-lookup"><span data-stu-id="9c81d-133">There is no need for a navigation property from :::no-loc(Tag)::: to :::no-loc(Item):::, so it is not included.</span></span>
  - <span data-ttu-id="9c81d-134">Кроме того, не :::no-loc(Tag)::: определяет свойство внешнего ключа.</span><span class="sxs-lookup"><span data-stu-id="9c81d-134">Also, :::no-loc(Tag)::: does not define a foreign key property.</span></span>
    <span data-ttu-id="9c81d-135">Вместо этого EF будет создавать свойство в теневом состоянии и управлять им.</span><span class="sxs-lookup"><span data-stu-id="9c81d-135">Instead, EF will create and manage a property in shadow-state.</span></span>

#### <a name="the-no-loctag-entity-type"></a><span data-ttu-id="9c81d-136">:::no-loc(Tag):::Тип сущности</span><span class="sxs-lookup"><span data-stu-id="9c81d-136">The :::no-loc(Tag)::: entity type</span></span>

<span data-ttu-id="9c81d-137">`:::no-loc(Tag):::`Тип сущности:</span><span class="sxs-lookup"><span data-stu-id="9c81d-137">The `:::no-loc(Tag):::` entity type:</span></span>

[!code-csharp[:::no-loc(Tag):::EntityType](../../../samples/core/Miscellaneous/Testing/:::no-loc(Items):::WebApi/:::no-loc(Items):::WebApi/:::no-loc(Tag):::.cs?name=:::no-loc(Tag):::EntityType)]

<span data-ttu-id="9c81d-138">И его конфигурация в `DbContext.OnModelCreating` :</span><span class="sxs-lookup"><span data-stu-id="9c81d-138">And its configuration in `DbContext.OnModelCreating`:</span></span>

[!code-csharp[Configure:::no-loc(Tag):::](../../../samples/core/Miscellaneous/Testing/:::no-loc(Items):::WebApi/:::no-loc(Items):::WebApi/:::no-loc(Items):::Context.cs?name=Configure:::no-loc(Tag):::)]

<span data-ttu-id="9c81d-139">Аналогично :::no-loc(Item)::: :::no-loc(Tag)::: скрывает его первичный ключ и делает `Label` свойство доступным только для чтения.</span><span class="sxs-lookup"><span data-stu-id="9c81d-139">Similarly to :::no-loc(Item):::, :::no-loc(Tag)::: hides its primary key and makes the `Label` property read-only.</span></span>

### <a name="the-no-locitemscontroller"></a><span data-ttu-id="9c81d-140">:::no-loc(Items):::Контроллер</span><span class="sxs-lookup"><span data-stu-id="9c81d-140">The :::no-loc(Items):::Controller</span></span>

<span data-ttu-id="9c81d-141">Контроллер веб-API довольно прост.</span><span class="sxs-lookup"><span data-stu-id="9c81d-141">The Web API controller is pretty basic.</span></span>
<span data-ttu-id="9c81d-142">Он получает `DbContext` из контейнера внедрения зависимостей посредством внедрения конструктора:</span><span class="sxs-lookup"><span data-stu-id="9c81d-142">It gets a `DbContext` from the dependency injection container through constructor injection:</span></span>

[!code-csharp[Constructor](../../../samples/core/Miscellaneous/Testing/:::no-loc(Items):::WebApi/:::no-loc(Items):::WebApi/Controllers/:::no-loc(Items):::Controller.cs?name=Constructor)]

<span data-ttu-id="9c81d-143">Он имеет методы для получения всех :::no-loc(Items)::: или :::no-loc(Item)::: с заданным именем:</span><span class="sxs-lookup"><span data-stu-id="9c81d-143">It has methods to get all :::no-loc(Items)::: or an :::no-loc(Item)::: with a given name:</span></span>

[!code-csharp[Get](../../../samples/core/Miscellaneous/Testing/:::no-loc(Items):::WebApi/:::no-loc(Items):::WebApi/Controllers/:::no-loc(Items):::Controller.cs?name=Get)]

<span data-ttu-id="9c81d-144">В нем имеется метод для добавления нового :::no-loc(Item)::: :</span><span class="sxs-lookup"><span data-stu-id="9c81d-144">It has a method to add a new :::no-loc(Item)::::</span></span>

[!code-csharp[Post:::no-loc(Item):::](../../../samples/core/Miscellaneous/Testing/:::no-loc(Items):::WebApi/:::no-loc(Items):::WebApi/Controllers/:::no-loc(Items):::Controller.cs?name=Post:::no-loc(Item):::)]

<span data-ttu-id="9c81d-145">Метод для добавления метки к тегу :::no-loc(Item)::: с меткой:</span><span class="sxs-lookup"><span data-stu-id="9c81d-145">A method to tag an :::no-loc(Item)::: with a label:</span></span>

[!code-csharp[Post:::no-loc(Tag):::](../../../samples/core/Miscellaneous/Testing/:::no-loc(Items):::WebApi/:::no-loc(Items):::WebApi/Controllers/:::no-loc(Items):::Controller.cs?name=Post:::no-loc(Tag):::)]

<span data-ttu-id="9c81d-146">И метод для удаления :::no-loc(Item)::: и все связанные :::no-loc(Tags)::: :</span><span class="sxs-lookup"><span data-stu-id="9c81d-146">And a method to delete an :::no-loc(Item)::: and all associated :::no-loc(Tags)::::</span></span>

[!code-csharp[Delete:::no-loc(Item):::](../../../samples/core/Miscellaneous/Testing/:::no-loc(Items):::WebApi/:::no-loc(Items):::WebApi/Controllers/:::no-loc(Items):::Controller.cs?name=Delete:::no-loc(Item):::)]

<span data-ttu-id="9c81d-147">Большинство проверок и обработка ошибок были удалены для уменьшения перегруженности.</span><span class="sxs-lookup"><span data-stu-id="9c81d-147">Most validation and error handling have been removed to reduce clutter.</span></span>

## <a name="the-tests"></a><span data-ttu-id="9c81d-148">Тесты</span><span class="sxs-lookup"><span data-stu-id="9c81d-148">The Tests</span></span>

<span data-ttu-id="9c81d-149">Тесты организованы для выполнения с несколькими конфигурациями поставщика базы данных:</span><span class="sxs-lookup"><span data-stu-id="9c81d-149">The tests are organized to run with multiple database provider configurations:</span></span>

- <span data-ttu-id="9c81d-150">Поставщик SQL Server, который является поставщиком, используемым приложением.</span><span class="sxs-lookup"><span data-stu-id="9c81d-150">The SQL Server provider, which is the provider used by the application</span></span>
- <span data-ttu-id="9c81d-151">Поставщик SQLite</span><span class="sxs-lookup"><span data-stu-id="9c81d-151">The SQLite provider</span></span>
- <span data-ttu-id="9c81d-152">Поставщик SQLite, использующий базы данных SQLite в памяти</span><span class="sxs-lookup"><span data-stu-id="9c81d-152">The SQLite provider using in-memory SQLite databases</span></span>
- <span data-ttu-id="9c81d-153">Поставщик базы данных EF в памяти</span><span class="sxs-lookup"><span data-stu-id="9c81d-153">The EF in-memory database provider</span></span>

<span data-ttu-id="9c81d-154">Это достигается путем размещения всех тестов в базовом классе и последующего наследования от него для тестирования с каждым поставщиком.</span><span class="sxs-lookup"><span data-stu-id="9c81d-154">This is achieved by putting all the tests in a base class, then inheriting from this to test with each provider.</span></span>

> [!TIP]
> <span data-ttu-id="9c81d-155">Если вы не используете LocalDB, необходимо изменить строку подключения SQL Server.</span><span class="sxs-lookup"><span data-stu-id="9c81d-155">You will need to change the SQL Server connection string if you're not using LocalDB.</span></span>
> <span data-ttu-id="9c81d-156">Рекомендации по использованию SQLite для тестирования в памяти см. в разделе [тестирование с помощью Sqlite](xref:core/testing/sqlite) .</span><span class="sxs-lookup"><span data-stu-id="9c81d-156">See [Testing with SQLite](xref:core/testing/sqlite) for guidance on using SQLite for in-memory testing.</span></span>

<span data-ttu-id="9c81d-157">Ожидается, что следующие два теста завершаются ошибкой:</span><span class="sxs-lookup"><span data-stu-id="9c81d-157">The following two tests are expected to fail:</span></span>

- <span data-ttu-id="9c81d-158">`Can_remove_item_and_all_associated_:::no-loc(tags):::` При работе с поставщиком базы данных EF в памяти</span><span class="sxs-lookup"><span data-stu-id="9c81d-158">`Can_remove_item_and_all_associated_:::no-loc(tags):::` when running with the EF in-memory database provider</span></span>
- <span data-ttu-id="9c81d-159">`Can_add_item_differing_only_by_case` При работе с поставщиком SQL Server</span><span class="sxs-lookup"><span data-stu-id="9c81d-159">`Can_add_item_differing_only_by_case` when running with the SQL Server provider</span></span>

<span data-ttu-id="9c81d-160">Более подробно эти сведения рассматриваются ниже.</span><span class="sxs-lookup"><span data-stu-id="9c81d-160">This is covered in more detail below.</span></span>

### <a name="setting-up-and-seeding-the-database"></a><span data-ttu-id="9c81d-161">Настройка и заполнение базы данных</span><span class="sxs-lookup"><span data-stu-id="9c81d-161">Setting up and seeding the database</span></span>

<span data-ttu-id="9c81d-162">XUnit, как и большинство платформ тестирования, создаст новый экземпляр тестового класса для каждого тестового запуска.</span><span class="sxs-lookup"><span data-stu-id="9c81d-162">XUnit, like most testing frameworks, will create a new test class instance for each test run.</span></span>
<span data-ttu-id="9c81d-163">Кроме того, XUnit не будет выполнять тесты внутри данного тестового класса параллельно.</span><span class="sxs-lookup"><span data-stu-id="9c81d-163">Also, XUnit will not run tests within a given test class in parallel.</span></span>
<span data-ttu-id="9c81d-164">Это означает, что мы можем установить и настроить базу данных в конструкторе тестов, и она будет находиться в известном состоянии для каждого теста.</span><span class="sxs-lookup"><span data-stu-id="9c81d-164">This means that we can setup and configure the database in the test constructor and it will be in a well-known state for each test.</span></span>

> [!TIP]
> <span data-ttu-id="9c81d-165">Этот пример повторно создает базу данных для каждого теста.</span><span class="sxs-lookup"><span data-stu-id="9c81d-165">This sample recreates the database for each test.</span></span>
> <span data-ttu-id="9c81d-166">Это хорошо подходит для тестирования SQLite и EF в памяти, но может включать значительные издержки, связанные с другими системами баз данных, включая SQL Server.</span><span class="sxs-lookup"><span data-stu-id="9c81d-166">This works well for SQLite and EF in-memory database testing but can involve significant overhead with other database systems, including SQL Server.</span></span>
> <span data-ttu-id="9c81d-167">Подходы к сокращению этих затрат рассматриваются в разделе [совместное использование баз данных в разных тестах](xref:core/testing/sharing-databases).</span><span class="sxs-lookup"><span data-stu-id="9c81d-167">Approaches for reducing this overhead are covered in [Sharing databases across tests](xref:core/testing/sharing-databases).</span></span>

<span data-ttu-id="9c81d-168">При выполнении каждого теста:</span><span class="sxs-lookup"><span data-stu-id="9c81d-168">When each test is run:</span></span>

- <span data-ttu-id="9c81d-169">DbContextOptions настроены для используемого поставщика и передаются конструктору базового класса</span><span class="sxs-lookup"><span data-stu-id="9c81d-169">DbContextOptions are configured for the provider in use and passed to the base class constructor</span></span>
  - <span data-ttu-id="9c81d-170">Эти параметры хранятся в свойстве и используются во всех тестах для создания экземпляров DbContext.</span><span class="sxs-lookup"><span data-stu-id="9c81d-170">These options are stored in a property and used throughout the tests for creating DbContext instances</span></span>
- <span data-ttu-id="9c81d-171">Для создания и заполнения базы данных вызывается метод SEED</span><span class="sxs-lookup"><span data-stu-id="9c81d-171">A Seed method is called to create and seed the database</span></span>
  - <span data-ttu-id="9c81d-172">Метод SEED гарантирует, что база данных будет очищена, удалив ее, а затем повторно создав ее.</span><span class="sxs-lookup"><span data-stu-id="9c81d-172">The Seed method ensures the database is clean by deleting it and then re-creating it</span></span>
  - <span data-ttu-id="9c81d-173">Некоторые хорошо известные тестовые сущности создаются и сохраняются в базе данных.</span><span class="sxs-lookup"><span data-stu-id="9c81d-173">Some well-known test entities are created and saved to the database</span></span>

[!code-csharp[Seeding](../../../samples/core/Miscellaneous/Testing/:::no-loc(Items):::WebApi/Tests/:::no-loc(Items):::ControllerTest.cs?name=Seeding)]

<span data-ttu-id="9c81d-174">Затем каждый конкретный тестовый класс наследуется от этого.</span><span class="sxs-lookup"><span data-stu-id="9c81d-174">Each concrete test class then inherits from this.</span></span>
<span data-ttu-id="9c81d-175">Пример:</span><span class="sxs-lookup"><span data-stu-id="9c81d-175">For example:</span></span>

[!code-csharp[Sqlite:::no-loc(Items):::ControllerTest](../../../samples/core/Miscellaneous/Testing/:::no-loc(Items):::WebApi/Tests/Sqlite:::no-loc(Items):::ControllerTest.cs?name=Sqlite:::no-loc(Items):::ControllerTest)]

### <a name="test-structure"></a><span data-ttu-id="9c81d-176">Структура теста</span><span class="sxs-lookup"><span data-stu-id="9c81d-176">Test structure</span></span>

<span data-ttu-id="9c81d-177">Несмотря на то, что приложение использует внедрение зависимостей, тесты не имеют.</span><span class="sxs-lookup"><span data-stu-id="9c81d-177">Even though the application uses dependency injection, the tests do not.</span></span>
<span data-ttu-id="9c81d-178">Здесь было бы неплохо использовать внедрение зависимостей, но дополнительный код, который он требует, имеет небольшую ценность.</span><span class="sxs-lookup"><span data-stu-id="9c81d-178">It would be fine to use dependency injection here, but the additional code it requires has little value.</span></span>
<span data-ttu-id="9c81d-179">Вместо этого DbContext создается с помощью `new` , а затем напрямую передается в качестве зависимости контроллеру.</span><span class="sxs-lookup"><span data-stu-id="9c81d-179">Instead, a DbContext is created using `new` and then directly passed as the dependency to the controller.</span></span>

<span data-ttu-id="9c81d-180">Затем каждый тест выполняет тестируемый метод на контроллере и утверждает результаты ожидаемым образом.</span><span class="sxs-lookup"><span data-stu-id="9c81d-180">Each test then executes the method under test on the controller and asserts the results are as expected.</span></span>
<span data-ttu-id="9c81d-181">Пример:</span><span class="sxs-lookup"><span data-stu-id="9c81d-181">For example:</span></span>

[!code-csharp[CanGet:::no-loc(Items):::](../../../samples/core/Miscellaneous/Testing/:::no-loc(Items):::WebApi/Tests/:::no-loc(Items):::ControllerTest.cs?name=CanGet:::no-loc(Items):::)]

<span data-ttu-id="9c81d-182">Обратите внимание, что для заполнения базы данных и выполнения тестов используются различные экземпляры DbContext.</span><span class="sxs-lookup"><span data-stu-id="9c81d-182">Notice that different DbContext instances are used to seed the database and run the tests.</span></span>
<span data-ttu-id="9c81d-183">Это гарантирует, что тест не использует (или не переходит) сущности, которые отслеживает контекст при заполнении.</span><span class="sxs-lookup"><span data-stu-id="9c81d-183">This ensures that the test is not using (or tripping over) entities tracked by the context when seeding.</span></span>
<span data-ttu-id="9c81d-184">Он также лучше соответствует тому, что происходит в веб-приложениях и службах.</span><span class="sxs-lookup"><span data-stu-id="9c81d-184">It also better matches what happens in web apps and services.</span></span>

<span data-ttu-id="9c81d-185">Тесты, изменяющие базу данных, создают второй экземпляр DbContext в тесте по тем же причинам.</span><span class="sxs-lookup"><span data-stu-id="9c81d-185">Tests that mutate the database create a second DbContext instance in the test for similar reasons.</span></span>
<span data-ttu-id="9c81d-186">То есть создание нового, чистого и последующего контекста, а затем чтение из базы данных, чтобы убедиться, что изменения были сохранены в базе данных.</span><span class="sxs-lookup"><span data-stu-id="9c81d-186">That is, creating a new, clean, context and then reading into it from the database to ensure that the changes were saved to the database.</span></span>
<span data-ttu-id="9c81d-187">Пример:</span><span class="sxs-lookup"><span data-stu-id="9c81d-187">For example:</span></span>

[!code-csharp[CanAdd:::no-loc(Item):::](../../../samples/core/Miscellaneous/Testing/:::no-loc(Items):::WebApi/Tests/:::no-loc(Items):::ControllerTest.cs?name=CanAdd:::no-loc(Item):::)]

<span data-ttu-id="9c81d-188">Два еще более важных тестирования охватывают бизнес-логику, связанную с добавлением :::no-loc(tags)::: .</span><span class="sxs-lookup"><span data-stu-id="9c81d-188">Two slightly more involved tests cover the business logic around adding :::no-loc(tags):::.</span></span>

[!code-csharp[CanAdd:::no-loc(Tag):::](../../../samples/core/Miscellaneous/Testing/:::no-loc(Items):::WebApi/Tests/:::no-loc(Items):::ControllerTest.cs?name=CanAdd:::no-loc(Tag):::)]

[!code-csharp[CanUp:::no-loc(Tag):::Count](../../../samples/core/Miscellaneous/Testing/:::no-loc(Items):::WebApi/Tests/:::no-loc(Items):::ControllerTest.cs?name=CanUp:::no-loc(Tag):::Count)]

## <a name="issues-using-different-database-providers"></a><span data-ttu-id="9c81d-189">Проблемы с использованием разных поставщиков баз данных</span><span class="sxs-lookup"><span data-stu-id="9c81d-189">Issues using different database providers</span></span>

<span data-ttu-id="9c81d-190">Тестирование с использованием другой системы баз данных, чем используется в рабочем приложении, может привести к проблемам.</span><span class="sxs-lookup"><span data-stu-id="9c81d-190">Testing with a different database system than is used in the production application can lead to problems.</span></span>
<span data-ttu-id="9c81d-191">Они описаны на концептуальном уровне в [тестовом коде, который использует EF Core](xref:core/testing/index).</span><span class="sxs-lookup"><span data-stu-id="9c81d-191">These are covered at the conceptual level in [Testing code that uses EF Core](xref:core/testing/index).</span></span>
<span data-ttu-id="9c81d-192">В следующих разделах рассматриваются два примера таких проблем, продемонстрированных в тестах в этом примере.</span><span class="sxs-lookup"><span data-stu-id="9c81d-192">The sections below cover two examples of such issues demonstrated by the tests in this sample.</span></span>

### <a name="test-passes-when-the-application-is-broken"></a><span data-ttu-id="9c81d-193">Тест проходит успешно при нарушении приложения</span><span class="sxs-lookup"><span data-stu-id="9c81d-193">Test passes when the application is broken</span></span>

<span data-ttu-id="9c81d-194">Одно из требований для нашего приложения заключается в том, что для этого используется :::no-loc(Items)::: имя с учетом регистра и коллекция :::no-loc(Tags)::: .</span><span class="sxs-lookup"><span data-stu-id="9c81d-194">One of the requirements for our application is that ":::no-loc(Items)::: have a case-sensitive name and a collection of :::no-loc(Tags):::."</span></span>
<span data-ttu-id="9c81d-195">Это довольно просто для тестирования:</span><span class="sxs-lookup"><span data-stu-id="9c81d-195">This is pretty simple to test:</span></span>

[!code-csharp[CanAdd:::no-loc(Item):::CaseInsensitive](../../../samples/core/Miscellaneous/Testing/:::no-loc(Items):::WebApi/Tests/:::no-loc(Items):::ControllerTest.cs?name=CanAdd:::no-loc(Item):::CaseInsensitive)]

<span data-ttu-id="9c81d-196">Выполнение этого теста для базы данных EF в памяти означает, что все правильно.</span><span class="sxs-lookup"><span data-stu-id="9c81d-196">Running this test against the EF in-memory database indicates that everything is fine.</span></span>
<span data-ttu-id="9c81d-197">При использовании SQLite все еще выглядит нормально.</span><span class="sxs-lookup"><span data-stu-id="9c81d-197">Everything still looks fine when using SQLite.</span></span>
<span data-ttu-id="9c81d-198">Но тест завершается ошибкой при запуске SQL Server!</span><span class="sxs-lookup"><span data-stu-id="9c81d-198">But the test fails when run against SQL Server!</span></span>

```output
System.InvalidOperationException : Sequence contains more than one element
   at System.Linq.ThrowHelper.ThrowMoreThanOneElementException()
   at System.Linq.Enumerable.Single[TSource](IEnumerable`1 source)
   at Microsoft.EntityFrameworkCore.Query.Internal.QueryCompiler.Execute[TResult](Expression query)
   at Microsoft.EntityFrameworkCore.Query.Internal.EntityQueryProvider.Execute[TResult](Expression expression)
   at System.Linq.Queryable.Single[TSource](IQueryable`1 source, Expression`1 predicate)
   at Tests.:::no-loc(Items):::ControllerTest.Can_add_item_differing_only_by_case()
```

<span data-ttu-id="9c81d-199">Это происходит потому, что по умолчанию учитывается регистр в базе данных EF в памяти и в базе данных SQLite.</span><span class="sxs-lookup"><span data-stu-id="9c81d-199">This is because both the EF in-memory database and the SQLite database are case-sensitive by default.</span></span>
<span data-ttu-id="9c81d-200">SQL Server, с другой стороны, не учитывает регистр.</span><span class="sxs-lookup"><span data-stu-id="9c81d-200">SQL Server, on the other hand, is case-insensitive!</span></span>

<span data-ttu-id="9c81d-201">EF Core, по дизайну, не изменяет эти поведения, поскольку принудительное изменение чувствительности к регистру может существенно повлиять на производительность.</span><span class="sxs-lookup"><span data-stu-id="9c81d-201">EF Core, by design, does not change these behaviors because forcing a change in case-sensitivity can have a big performance impact.</span></span>

<span data-ttu-id="9c81d-202">Когда мы понимаем, что это проблема, можно исправить приложение и компенсировать тесты.</span><span class="sxs-lookup"><span data-stu-id="9c81d-202">Once we know this is a problem we can fix the application and compensate in tests.</span></span>
<span data-ttu-id="9c81d-203">Однако в этом случае эта ошибка может быть пропущена, если только тестирование выполняется с использованием в базе данных EF в памяти или поставщиков SQLite.</span><span class="sxs-lookup"><span data-stu-id="9c81d-203">However, the point here is that this bug could be missed if only testing with the EF in-memory database or SQLite providers.</span></span>

### <a name="test-fails-when-the-application-is-correct"></a><span data-ttu-id="9c81d-204">Проверка завершается ошибкой, если приложение является правильным</span><span class="sxs-lookup"><span data-stu-id="9c81d-204">Test fails when the application is correct</span></span>

<span data-ttu-id="9c81d-205">Другим требованием для нашего приложения является то, что "Удаление :::no-loc(Item)::: должно удалить все связанные :::no-loc(Tags)::: ".</span><span class="sxs-lookup"><span data-stu-id="9c81d-205">Another of the requirements for our application is that "deleting an :::no-loc(Item)::: should delete all associated :::no-loc(Tags):::."</span></span>
<span data-ttu-id="9c81d-206">Опять же, легко тестировать:</span><span class="sxs-lookup"><span data-stu-id="9c81d-206">Again, easy to test:</span></span>

[!code-csharp[Delete:::no-loc(Item):::](../../../samples/core/Miscellaneous/Testing/:::no-loc(Items):::WebApi/Tests/:::no-loc(Items):::ControllerTest.cs?name=Delete:::no-loc(Item):::)]

<span data-ttu-id="9c81d-207">Этот тест передается в SQL Server и SQLite, но завершается сбоем с базой данных EF в памяти!</span><span class="sxs-lookup"><span data-stu-id="9c81d-207">This test passes on SQL Server and SQLite, but fails with the EF in-memory database!</span></span>

```output
Assert.False() Failure
Expected: False
Actual:   True
   at Tests.:::no-loc(Items):::ControllerTest.Can_remove_item_and_all_associated_:::no-loc(tags):::()
```

<span data-ttu-id="9c81d-208">В этом случае приложение работает правильно, так как SQL Server поддерживает [каскадное удаление](xref:core/saving/cascade-delete).</span><span class="sxs-lookup"><span data-stu-id="9c81d-208">In this case, the application is working correctly because SQL Server supports [cascade deletes](xref:core/saving/cascade-delete).</span></span>
<span data-ttu-id="9c81d-209">SQLite также поддерживает каскадное удаление, как и большинство реляционных баз данных, поэтому тестирование этого файла в SQLite работает.</span><span class="sxs-lookup"><span data-stu-id="9c81d-209">SQLite also supports cascade deletes, as do most relational databases, so testing this on SQLite works.</span></span>
<span data-ttu-id="9c81d-210">С другой стороны, база данных EF в памяти [не поддерживает каскадное удаление](https://github.com/dotnet/efcore/issues/3924).</span><span class="sxs-lookup"><span data-stu-id="9c81d-210">On the other hand, the EF in-memory database [does not support cascade deletes](https://github.com/dotnet/efcore/issues/3924).</span></span>
<span data-ttu-id="9c81d-211">Это означает, что эта часть приложения не может быть протестирована с помощью поставщика базы данных EF в памяти.</span><span class="sxs-lookup"><span data-stu-id="9c81d-211">This means that this part of the application cannot be tested with the EF in-memory database provider.</span></span>
