---
title: Пример EF Core тестирования — EF Core
description: Пример, демонстрирующий тестирование приложений, использующих EF Core
author: ajcvickers
ms.date: 04/22/2020
uid: core/miscellaneous/testing/testing-sample
no-loc:
- Item
- Tag
- Items
- Tags
- items
- tags
ms.openlocfilehash: dda7191df7646aa06aab51d8d7891bd0ba155674
ms.sourcegitcommit: 79e460f76b6664e1da5886d102bd97f651d2ffff
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/29/2020
ms.locfileid: "82564262"
---
# <a name="ef-core-testing-sample"></a><span data-ttu-id="1941c-103">Пример EF Core тестирования</span><span class="sxs-lookup"><span data-stu-id="1941c-103">EF Core testing sample</span></span>

> [!TIP]
> <span data-ttu-id="1941c-104">Код в этом документе можно найти на сайте GitHub в качестве [готового к запуску примера](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Testing/ItemsWebApi/).</span><span class="sxs-lookup"><span data-stu-id="1941c-104">The code in this document can be found on GitHub as a [runnable sample](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Testing/ItemsWebApi/).</span></span>
> <span data-ttu-id="1941c-105">Обратите внимание, что некоторые из этих тестов должны **завершаться ошибкой**.</span><span class="sxs-lookup"><span data-stu-id="1941c-105">Note that some of these tests **are expected to fail**.</span></span> <span data-ttu-id="1941c-106">Причины этого описаны ниже.</span><span class="sxs-lookup"><span data-stu-id="1941c-106">The reasons for this are explained below.</span></span> 

<span data-ttu-id="1941c-107">В этом документе рассматривается пример для тестирования кода, который использует EF Core.</span><span class="sxs-lookup"><span data-stu-id="1941c-107">This doc walks through a sample for testing code that uses EF Core.</span></span>

## <a name="the-application"></a><span data-ttu-id="1941c-108">Приложение</span><span class="sxs-lookup"><span data-stu-id="1941c-108">The application</span></span>

<span data-ttu-id="1941c-109">[Пример](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Testing/ItemsWebApi/) содержит два проекта:</span><span class="sxs-lookup"><span data-stu-id="1941c-109">The [sample](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Testing/ItemsWebApi/) contains two projects:</span></span>
- <span data-ttu-id="1941c-110">Итемсвебапи: очень простой [веб-API, поддерживаемый ASP.NET Core](/aspnet/core/tutorials/first-web-api?view=aspnetcore-3.1&tabs=visual-studio) с одним контроллером</span><span class="sxs-lookup"><span data-stu-id="1941c-110">ItemsWebApi: A very simple [Web API backed by ASP.NET Core](/aspnet/core/tutorials/first-web-api?view=aspnetcore-3.1&tabs=visual-studio) with a single controller</span></span>
- <span data-ttu-id="1941c-111">Тесты: тестовый проект [xUnit](https://xunit.net/) для тестирования контроллера</span><span class="sxs-lookup"><span data-stu-id="1941c-111">Tests: An [XUnit](https://xunit.net/) test project to test the controller</span></span>

### <a name="the-model-and-business-rules"></a><span data-ttu-id="1941c-112">Модель и бизнес-правила</span><span class="sxs-lookup"><span data-stu-id="1941c-112">The model and business rules</span></span>

<span data-ttu-id="1941c-113">Модель, которая включает этот API, имеет два типа Items сущностей Tags: и.</span><span class="sxs-lookup"><span data-stu-id="1941c-113">The model backing this API has two entity types: Items and Tags.</span></span>

* Items<span data-ttu-id="1941c-114">зависят от регистра имя и коллекцию Tags.</span><span class="sxs-lookup"><span data-stu-id="1941c-114"> have a case-sensitive name and a collection of Tags.</span></span>
* <span data-ttu-id="1941c-115">Каждый Tag имеет метку и число, представляющее количество применений к Item.</span><span class="sxs-lookup"><span data-stu-id="1941c-115">Each Tag has a label and a count representing the number of times it has been applied to the Item.</span></span>
* <span data-ttu-id="1941c-116">У Item каждого из них должен Tag быть только один с заданной меткой.</span><span class="sxs-lookup"><span data-stu-id="1941c-116">Each Item should only have one Tag with with a given label.</span></span>
  * <span data-ttu-id="1941c-117">Если элемент помечен одной и той же меткой более одного раза, то число в существующем теге с этой меткой увеличивается вместо создания нового тега.</span><span class="sxs-lookup"><span data-stu-id="1941c-117">If an item is tagged with the same label more than once, then the count on the existing tag with that label is incremented instead of a new tag being created.</span></span> 
* <span data-ttu-id="1941c-118">Удаление Item должно удалить все связанные Tags.</span><span class="sxs-lookup"><span data-stu-id="1941c-118">Deleting an Item should delete all associated Tags.</span></span>

#### <a name="the-item-entity-type"></a><span data-ttu-id="1941c-119">Тип Item сущности</span><span class="sxs-lookup"><span data-stu-id="1941c-119">The Item entity type</span></span>

<span data-ttu-id="1941c-120">Тип `Item` сущности:</span><span class="sxs-lookup"><span data-stu-id="1941c-120">The `Item` entity type:</span></span>

[!code-csharp[ItemEntityType](../../../../samples/core/Miscellaneous/Testing/ItemsWebApi/ItemsWebApi/Item.cs?name=ItemEntityType)]

<span data-ttu-id="1941c-121">И его конфигурация в `DbContext.OnModelCreating`:</span><span class="sxs-lookup"><span data-stu-id="1941c-121">And its configuration in `DbContext.OnModelCreating`:</span></span>

[!code-csharp[ConfigureItem](../../../../samples/core/Miscellaneous/Testing/ItemsWebApi/ItemsWebApi/ItemsContext.cs?name=ConfigureItem)]

<span data-ttu-id="1941c-122">Обратите внимание, что тип сущности ограничивается способом, который можно использовать для отражения модели предметной области и бизнес-правил.</span><span class="sxs-lookup"><span data-stu-id="1941c-122">Notice that entity type constrains the way it can be used to reflect the domain model and business rules.</span></span> <span data-ttu-id="1941c-123">В частности:</span><span class="sxs-lookup"><span data-stu-id="1941c-123">In particular:</span></span>
- <span data-ttu-id="1941c-124">Первичный ключ сопоставляется непосредственно с `_id` полем и не предоставляется публично</span><span class="sxs-lookup"><span data-stu-id="1941c-124">The primary key is mapped directly to the `_id` field and not exposed publicly</span></span>
  - <span data-ttu-id="1941c-125">EF обнаруживает и использует закрытый конструктор, принимающий значение и имя первичного ключа.</span><span class="sxs-lookup"><span data-stu-id="1941c-125">EF detects and uses the private constructor accepting the primary key value and name.</span></span>
- <span data-ttu-id="1941c-126">`Name` Свойство доступно только для чтения и задается только в конструкторе.</span><span class="sxs-lookup"><span data-stu-id="1941c-126">The `Name` property is read-only and set only in the constructor.</span></span> 
- Tags<span data-ttu-id="1941c-127">предоставляются в виде, `IReadOnlyList<Tag>` чтобы предотвратить произвольное изменение.</span><span class="sxs-lookup"><span data-stu-id="1941c-127"> are exposed as a `IReadOnlyList<Tag>` to prevent arbitrary modification.</span></span>
  - <span data-ttu-id="1941c-128">EF связывает `Tags` свойство с `_tags` резервным полем, сопоставляя их имена.</span><span class="sxs-lookup"><span data-stu-id="1941c-128">EF associates the `Tags` property with the `_tags` backing field by matching their names.</span></span> 
  - <span data-ttu-id="1941c-129">`AddTag` Метод принимает метку тега и реализует бизнес-правило, описанное выше.</span><span class="sxs-lookup"><span data-stu-id="1941c-129">The `AddTag` method takes a tag label and implements the business rule described above.</span></span>
    <span data-ttu-id="1941c-130">То есть, тег добавляется только для новых меток.</span><span class="sxs-lookup"><span data-stu-id="1941c-130">That is, a tag is only added for new labels.</span></span>
    <span data-ttu-id="1941c-131">В противном случае значение счетчика в существующей метке увеличивается.</span><span class="sxs-lookup"><span data-stu-id="1941c-131">Otherwise the count on an existing label is incremented.</span></span>
- <span data-ttu-id="1941c-132">Свойство `Tags` навигации настроено для связи "многие к одному"</span><span class="sxs-lookup"><span data-stu-id="1941c-132">The `Tags` navigation property is configured for a many-to-one relationship</span></span>
  - <span data-ttu-id="1941c-133">Нет необходимости в свойстве навигации из Tag Item, поэтому оно не включено.</span><span class="sxs-lookup"><span data-stu-id="1941c-133">There is no need for a navigation property from Tag to Item, so it is not included.</span></span>
  - <span data-ttu-id="1941c-134">Кроме того Tag , не определяет свойство внешнего ключа.</span><span class="sxs-lookup"><span data-stu-id="1941c-134">Also, Tag does not define a foreign key property.</span></span>
    <span data-ttu-id="1941c-135">Вместо этого EF будет создавать свойство в теневом состоянии и управлять им.</span><span class="sxs-lookup"><span data-stu-id="1941c-135">Instead, EF will create and manage a property in shadow-state.</span></span>

#### <a name="the-tag-entity-type"></a><span data-ttu-id="1941c-136">Тип Tag сущности</span><span class="sxs-lookup"><span data-stu-id="1941c-136">The Tag entity type</span></span>

<span data-ttu-id="1941c-137">Тип `Tag` сущности:</span><span class="sxs-lookup"><span data-stu-id="1941c-137">The `Tag` entity type:</span></span>

[!code-csharp[TagEntityType](../../../../samples/core/Miscellaneous/Testing/ItemsWebApi/ItemsWebApi/Tag.cs?name=TagEntityType)]

<span data-ttu-id="1941c-138">И его конфигурация в `DbContext.OnModelCreating`:</span><span class="sxs-lookup"><span data-stu-id="1941c-138">And its configuration in `DbContext.OnModelCreating`:</span></span>

[!code-csharp[ConfigureTag](../../../../samples/core/Miscellaneous/Testing/ItemsWebApi/ItemsWebApi/ItemsContext.cs?name=ConfigureTag)]

<span data-ttu-id="1941c-139">ItemАналогично Tag скрывает его первичный ключ и делает `Label` свойство доступным только для чтения.</span><span class="sxs-lookup"><span data-stu-id="1941c-139">Similarly to Item, Tag hides its primary key and makes the `Label` property read-only.</span></span>

### <a name="the-itemscontroller"></a><span data-ttu-id="1941c-140">Итемсконтроллер</span><span class="sxs-lookup"><span data-stu-id="1941c-140">The ItemsController</span></span>

<span data-ttu-id="1941c-141">Контроллер веб-API довольно прост.</span><span class="sxs-lookup"><span data-stu-id="1941c-141">The Web API controller is pretty basic.</span></span>
<span data-ttu-id="1941c-142">Он получает `DbContext` из контейнера внедрения зависимостей посредством внедрения конструктора:</span><span class="sxs-lookup"><span data-stu-id="1941c-142">It gets a `DbContext` from the dependency injection container through constructor injection:</span></span>

[!code-csharp[Constructor](../../../../samples/core/Miscellaneous/Testing/ItemsWebApi/ItemsWebApi/Controllers/ItemsController.cs?name=Constructor)]

<span data-ttu-id="1941c-143">Он имеет методы для получения всех Items или Item с заданным именем:</span><span class="sxs-lookup"><span data-stu-id="1941c-143">It has methods to get all Items or an Item with a given name:</span></span>

[!code-csharp[Get](../../../../samples/core/Miscellaneous/Testing/ItemsWebApi/ItemsWebApi/Controllers/ItemsController.cs?name=Get)]

<span data-ttu-id="1941c-144">В нем имеется метод для добавления нового Item:</span><span class="sxs-lookup"><span data-stu-id="1941c-144">It has a method to add a new Item:</span></span>

[!code-csharp[PostItem](../../../../samples/core/Miscellaneous/Testing/ItemsWebApi/ItemsWebApi/Controllers/ItemsController.cs?name=PostItem)]

<span data-ttu-id="1941c-145">Метод для добавления метки к Item тегу с меткой:</span><span class="sxs-lookup"><span data-stu-id="1941c-145">A method to tag an Item with a label:</span></span>

[!code-csharp[PostTag](../../../../samples/core/Miscellaneous/Testing/ItemsWebApi/ItemsWebApi/Controllers/ItemsController.cs?name=PostTag)]

<span data-ttu-id="1941c-146">И метод для удаления Item и все связанные: Tags</span><span class="sxs-lookup"><span data-stu-id="1941c-146">And a method to delete an Item and all associated Tags:</span></span>

[!code-csharp[DeleteItem](../../../../samples/core/Miscellaneous/Testing/ItemsWebApi/ItemsWebApi/Controllers/ItemsController.cs?name=DeleteItem)]

<span data-ttu-id="1941c-147">Большинство проверок и обработка ошибок были удалены для уменьшения перегруженности.</span><span class="sxs-lookup"><span data-stu-id="1941c-147">Most validation and error handling has been removed to reduce clutter.</span></span>

## <a name="the-tests"></a><span data-ttu-id="1941c-148">Тесты</span><span class="sxs-lookup"><span data-stu-id="1941c-148">The Tests</span></span>

<span data-ttu-id="1941c-149">Тесты организованы для выполнения с несколькими конфигурациями поставщика базы данных:</span><span class="sxs-lookup"><span data-stu-id="1941c-149">The tests are organized to run with multiple database provider configurations:</span></span>
* <span data-ttu-id="1941c-150">Поставщик SQL Server, который является поставщиком, используемым приложением.</span><span class="sxs-lookup"><span data-stu-id="1941c-150">The SQL Server provider, which is the provider used by the application</span></span>
* <span data-ttu-id="1941c-151">Поставщик SQLite</span><span class="sxs-lookup"><span data-stu-id="1941c-151">The SQLite provider</span></span>
* <span data-ttu-id="1941c-152">Поставщик SQLite, использующий базы данных SQLite в памяти</span><span class="sxs-lookup"><span data-stu-id="1941c-152">The SQLite provider using in-memory SQLite databases</span></span>
* <span data-ttu-id="1941c-153">Поставщик базы данных EF в памяти</span><span class="sxs-lookup"><span data-stu-id="1941c-153">The EF in-memory database provider</span></span>

<span data-ttu-id="1941c-154">Это достигается путем размещения всех тестов в базовом классе и последующего наследования от него для тестирования с каждым поставщиком.</span><span class="sxs-lookup"><span data-stu-id="1941c-154">This is achieved by putting all the tests in a base class, then inheriting from this to test with each provider.</span></span>

> [!TIP]
> <span data-ttu-id="1941c-155">Если вы не используете LocalDB, необходимо изменить строку подключения SQL Server.</span><span class="sxs-lookup"><span data-stu-id="1941c-155">You will need to change the SQL Server connection string if you're not using LocalDB.</span></span>

> [!TIP]
> <span data-ttu-id="1941c-156">Рекомендации по использованию SQLite для тестирования в памяти см. в разделе [тестирование с помощью Sqlite](xref:core/miscellaneous/testing/sqlite) .</span><span class="sxs-lookup"><span data-stu-id="1941c-156">See [Testing with SQLite](xref:core/miscellaneous/testing/sqlite) for guidance on using SQLite for in-memory testing.</span></span> 

<span data-ttu-id="1941c-157">Ожидается, что следующие два теста завершаются ошибкой:</span><span class="sxs-lookup"><span data-stu-id="1941c-157">The following two tests are expected to fail:</span></span>
* <span data-ttu-id="1941c-158">`Can_remove_item_and_all_associated_tags`При работе с поставщиком базы данных EF в памяти</span><span class="sxs-lookup"><span data-stu-id="1941c-158">`Can_remove_item_and_all_associated_tags` when running with the EF in-memory database provider</span></span>
* <span data-ttu-id="1941c-159">`Can_add_item_differing_only_by_case`При работе с поставщиком SQL Server</span><span class="sxs-lookup"><span data-stu-id="1941c-159">`Can_add_item_differing_only_by_case` when running with the SQL Server provider</span></span>

<span data-ttu-id="1941c-160">Более подробно эти сведения рассматриваются ниже.</span><span class="sxs-lookup"><span data-stu-id="1941c-160">This is covered in more detail below.</span></span>

### <a name="setting-up-and-seeding-the-database"></a><span data-ttu-id="1941c-161">Настройка и заполнение базы данных</span><span class="sxs-lookup"><span data-stu-id="1941c-161">Setting up and seeding the database</span></span>

<span data-ttu-id="1941c-162">XUnit, как и большинство платформ тестирования, создаст новый экземпляр тестового класса для каждого тестового запуска.</span><span class="sxs-lookup"><span data-stu-id="1941c-162">XUnit, like most testing frameworks, will create a new test class instance for each test run.</span></span>
<span data-ttu-id="1941c-163">Кроме того, XUnit не будет выполнять тесты внутри данного тестового класса параллельно.</span><span class="sxs-lookup"><span data-stu-id="1941c-163">Also, XUnit will not run tests within a given test class in parallel.</span></span> <span data-ttu-id="1941c-164">Это означает, что мы можем установить и настроить базу данных в конструкторе тестов, и она будет находиться в известном состоянии для каждого теста.</span><span class="sxs-lookup"><span data-stu-id="1941c-164">This means that we can setup and configure the database in the test constructor and it will be in a well-known state for each test.</span></span>

> [!TIP]
> <span data-ttu-id="1941c-165">Этот пример повторно создает базу данных для каждого теста.</span><span class="sxs-lookup"><span data-stu-id="1941c-165">This sample recreates the database for each test.</span></span>
> <span data-ttu-id="1941c-166">Это хорошо подходит для тестирования SQLite и EF в базе данных в памяти, но может заключаться в значительной нагрузке на другие системы баз данных, в том числе SQL Server.</span><span class="sxs-lookup"><span data-stu-id="1941c-166">This works well for SQLite and EF in-memory database testing, but can involve significant overhead with other database systems, including SQL Server.</span></span>
> <span data-ttu-id="1941c-167">Подходы к сокращению этих затрат рассматриваются в разделе [совместное использование баз данных в разных тестах](xref:core/miscellaneous/testing/sharing-databases).</span><span class="sxs-lookup"><span data-stu-id="1941c-167">Approaches for reducing this overhead are covered in [Sharing databases across tests](xref:core/miscellaneous/testing/sharing-databases).</span></span>

<span data-ttu-id="1941c-168">При выполнении каждого теста:</span><span class="sxs-lookup"><span data-stu-id="1941c-168">When each test is run:</span></span>
* <span data-ttu-id="1941c-169">DbContextOptions настроены для используемого поставщика и передаются конструктору базового класса</span><span class="sxs-lookup"><span data-stu-id="1941c-169">DbContextOptions are configured for the provider in use and passed to the base class constructor</span></span>
  * <span data-ttu-id="1941c-170">Эти параметры хранятся в свойстве и используются во всех тестах для создания экземпляров DbContext.</span><span class="sxs-lookup"><span data-stu-id="1941c-170">These options are stored in a property and used throughout the tests for creating DbContext instances</span></span>
* <span data-ttu-id="1941c-171">Для создания и заполнения базы данных вызывается метод SEED</span><span class="sxs-lookup"><span data-stu-id="1941c-171">A Seed method is called to create and seed the database</span></span>
  * <span data-ttu-id="1941c-172">Метод SEED гарантирует, что база данных будет очищена, удалив ее, а затем повторно создав ее.</span><span class="sxs-lookup"><span data-stu-id="1941c-172">The Seed method ensures the database is clean by deleting it and then re-creating it</span></span>
  * <span data-ttu-id="1941c-173">Некоторые хорошо известные тестовые сущности создаются и сохраняются в базе данных.</span><span class="sxs-lookup"><span data-stu-id="1941c-173">Some well-known test entities are created and saved to the database</span></span>

[!code-csharp[Seeding](../../../../samples/core/Miscellaneous/Testing/ItemsWebApi/Tests/ItemsControllerTest.cs?name=Seeding)]

<span data-ttu-id="1941c-174">Затем каждый конкретный тестовый класс наследуется от этого.</span><span class="sxs-lookup"><span data-stu-id="1941c-174">Each concrete test class then inherits from this.</span></span>
<span data-ttu-id="1941c-175">Пример:</span><span class="sxs-lookup"><span data-stu-id="1941c-175">For example:</span></span>

[!code-csharp[SqliteItemsControllerTest](../../../../samples/core/Miscellaneous/Testing/ItemsWebApi/Tests/SqliteItemsControllerTest.cs?name=SqliteItemsControllerTest)]

### <a name="test-structure"></a><span data-ttu-id="1941c-176">Структура теста</span><span class="sxs-lookup"><span data-stu-id="1941c-176">Test structure</span></span>

<span data-ttu-id="1941c-177">Несмотря на то, что приложение использует внедрение зависимостей, тесты не имеют.</span><span class="sxs-lookup"><span data-stu-id="1941c-177">Even though the application uses dependency injection, the tests do not.</span></span>
<span data-ttu-id="1941c-178">Здесь было бы неплохо использовать внедрение зависимостей, но дополнительный код, который он требует, имеет небольшую ценность.</span><span class="sxs-lookup"><span data-stu-id="1941c-178">It would be fine to use dependency injection here, but the additional code it requires has little value.</span></span>
<span data-ttu-id="1941c-179">Вместо этого DbContext создается с помощью `new` , а затем напрямую передается в качестве зависимости контроллеру.</span><span class="sxs-lookup"><span data-stu-id="1941c-179">Instead, a DbContext is created using `new` and then directly passed as the dependency to the controller.</span></span>

<span data-ttu-id="1941c-180">Затем каждый тест выполняет тестируемый метод на контроллере и утверждает результаты ожидаемым образом.</span><span class="sxs-lookup"><span data-stu-id="1941c-180">Each test then executes the method under test on the controller and asserts the results are as expected.</span></span>
<span data-ttu-id="1941c-181">Пример:</span><span class="sxs-lookup"><span data-stu-id="1941c-181">For example:</span></span>

[!code-csharp[CanGetItems](../../../../samples/core/Miscellaneous/Testing/ItemsWebApi/Tests/ItemsControllerTest.cs?name=CanGetItems)]

<span data-ttu-id="1941c-182">Обратите внимание, что для заполнения базы данных и выполнения тестов используются различные экземпляры DbContext.</span><span class="sxs-lookup"><span data-stu-id="1941c-182">Notice that different DbContext instances are used to seed the database and run the tests.</span></span> <span data-ttu-id="1941c-183">Это гарантирует, что тест не использует (или не переходит) сущности, которые отслеживает контекст при заполнении.</span><span class="sxs-lookup"><span data-stu-id="1941c-183">This ensures that test is not using (or tripping over) entities tracked by the context when seeding.</span></span>
<span data-ttu-id="1941c-184">Он также лучше соответствует тому, что происходит в веб-приложениях и службах.</span><span class="sxs-lookup"><span data-stu-id="1941c-184">It also better matches what happens in web apps and services.</span></span>

<span data-ttu-id="1941c-185">Тесты, изменяющие базу данных, создают второй экземпляр DbContext в тесте по тем же причинам.</span><span class="sxs-lookup"><span data-stu-id="1941c-185">Tests that mutate the database create a second DbContext instance in the test for similar reasons.</span></span>
<span data-ttu-id="1941c-186">Это значит, что создание нового, чистого и последующего контекста производится из базы данных, чтобы убедиться, что изменения действительно были сохранены в базе данных.</span><span class="sxs-lookup"><span data-stu-id="1941c-186">That is, creating a new, clean, context and then reading into it from the database to ensure that the changes really were saved to the database.</span></span> <span data-ttu-id="1941c-187">Пример:</span><span class="sxs-lookup"><span data-stu-id="1941c-187">For example:</span></span>

[!code-csharp[CanAddItem](../../../../samples/core/Miscellaneous/Testing/ItemsWebApi/Tests/ItemsControllerTest.cs?name=CanAddItem)]

<span data-ttu-id="1941c-188">Два еще более задействованных теста охватывают бизнес-логику вокруг добавления тегов.</span><span class="sxs-lookup"><span data-stu-id="1941c-188">Two slightly more involved tests cover the business logic around adding tags.</span></span>

[!code-csharp[CanAddTag](../../../../samples/core/Miscellaneous/Testing/ItemsWebApi/Tests/ItemsControllerTest.cs?name=CanAddTag)]

[!code-csharp[CanUpTagCount](../../../../samples/core/Miscellaneous/Testing/ItemsWebApi/Tests/ItemsControllerTest.cs?name=CanUpTagCount)]

## <a name="issues-using-different-database-providers"></a><span data-ttu-id="1941c-189">Проблемы с использованием разных поставщиков баз данных</span><span class="sxs-lookup"><span data-stu-id="1941c-189">Issues using different database providers</span></span>

<span data-ttu-id="1941c-190">Тестирование с использованием другой системы баз данных, чем используется в рабочем приложении, может привести к проблемам.</span><span class="sxs-lookup"><span data-stu-id="1941c-190">Testing with a different database system than is used in the production application can lead to problems.</span></span>
<span data-ttu-id="1941c-191">Они описаны на концептуальном уровне в [тестовом коде, который использует EF Core](xref:core/miscellaneous/testing/index).</span><span class="sxs-lookup"><span data-stu-id="1941c-191">These are covered at the conceptual level in [Testing code that uses EF Core](xref:core/miscellaneous/testing/index).</span></span>  
<span data-ttu-id="1941c-192">В следующих разделах рассматриваются два примера таких проблем, продемонстрированных в тестах в этом примере.</span><span class="sxs-lookup"><span data-stu-id="1941c-192">The sections below cover two examples of such issues demonstrated by the tests in this sample.</span></span>

### <a name="test-passes-when-application-is-broken"></a><span data-ttu-id="1941c-193">Тест проходит успешно при нарушении приложения</span><span class="sxs-lookup"><span data-stu-id="1941c-193">Test passes when application is broken</span></span>

<span data-ttu-id="1941c-194">Одно из требований для нашего приложения заключается в том, чтоItems "у вас есть имя с учетом регистра и Tagsколлекция".</span><span class="sxs-lookup"><span data-stu-id="1941c-194">One of the requirements for our application is that, "Items have a case-sensitive name and a collection of Tags."</span></span>
<span data-ttu-id="1941c-195">Это довольно просто для тестирования:</span><span class="sxs-lookup"><span data-stu-id="1941c-195">This is pretty simple to test:</span></span>

[!code-csharp[CanAddItemCaseInsensitive](../../../../samples/core/Miscellaneous/Testing/ItemsWebApi/Tests/ItemsControllerTest.cs?name=CanAddItemCaseInsensitive)]

<span data-ttu-id="1941c-196">Выполнение этого теста для базы данных EF в памяти означает, что все правильно.</span><span class="sxs-lookup"><span data-stu-id="1941c-196">Running this test against the EF in-memory database indicates that everything is fine.</span></span>
<span data-ttu-id="1941c-197">При использовании SQLite все еще выглядит нормально.</span><span class="sxs-lookup"><span data-stu-id="1941c-197">Everything still looks fine when using SQLite.</span></span>
<span data-ttu-id="1941c-198">Но тест завершается ошибкой при запуске SQL Server!</span><span class="sxs-lookup"><span data-stu-id="1941c-198">But the test fails when run against SQL Server!</span></span>

```
System.InvalidOperationException : Sequence contains more than one element
   at System.Linq.ThrowHelper.ThrowMoreThanOneElementException()
   at System.Linq.Enumerable.Single[TSource](IEnumerable`1 source)
   at Microsoft.EntityFrameworkCore.Query.Internal.QueryCompiler.Execute[TResult](Expression query)
   at Microsoft.EntityFrameworkCore.Query.Internal.EntityQueryProvider.Execute[TResult](Expression expression)
   at System.Linq.Queryable.Single[TSource](IQueryable`1 source, Expression`1 predicate)
   at Tests.ItemsControllerTest.Can_add_item_differing_only_by_case()
```

<span data-ttu-id="1941c-199">Это происходит потому, что по умолчанию учитывается регистр в базе данных EF в памяти и в базе данных SQLite.</span><span class="sxs-lookup"><span data-stu-id="1941c-199">This is because both the EF in-memory database and the SQLite database are case-sensitive by default.</span></span>
<span data-ttu-id="1941c-200">SQL Server, с другой стороны, не учитывает регистр.</span><span class="sxs-lookup"><span data-stu-id="1941c-200">SQL Server, on the other hand, is case-insensitive!</span></span> 

<span data-ttu-id="1941c-201">EF Core, по дизайну, не изменяет эти поведения, поскольку принудительное изменение чувствительности к регистру может существенно повлиять на производительность.</span><span class="sxs-lookup"><span data-stu-id="1941c-201">EF Core, by design, does not change these behaviors because forcing a change in case-sensitivity can have big performance impact.</span></span>

<span data-ttu-id="1941c-202">Когда мы понимаем, что это проблема, можно исправить приложение и компенсировать тесты.</span><span class="sxs-lookup"><span data-stu-id="1941c-202">Once we know this is a problem we can fix the application and compensate in tests.</span></span>
<span data-ttu-id="1941c-203">Однако в этом случае эта ошибка может быть пропущена, если только тестирование выполняется с использованием в базе данных EF в памяти или поставщиков SQLite.</span><span class="sxs-lookup"><span data-stu-id="1941c-203">However, the point here is that this bug could be missed if only testing with the EF in-memory database or SQLite providers.</span></span>

### <a name="test-fails-when-application-is-correct"></a><span data-ttu-id="1941c-204">Проверка завершается ошибкой, если приложение является правильным</span><span class="sxs-lookup"><span data-stu-id="1941c-204">Test fails when application is correct</span></span> 

<span data-ttu-id="1941c-205">Другим требованием для нашего приложения является то, что "Удаление Item должно удалить все связанные Tags".</span><span class="sxs-lookup"><span data-stu-id="1941c-205">Another of the requirements for our application is that, "deleting an Item should delete all associated Tags."</span></span>
<span data-ttu-id="1941c-206">Опять же, легко тестировать:</span><span class="sxs-lookup"><span data-stu-id="1941c-206">Again, easy to test:</span></span>

[!code-csharp[DeleteItem](../../../../samples/core/Miscellaneous/Testing/ItemsWebApi/Tests/ItemsControllerTest.cs?name=DeleteItem)]

<span data-ttu-id="1941c-207">Этот тест передается в SQL Server и SQLite, но завершается сбоем с базой данных EF в памяти!</span><span class="sxs-lookup"><span data-stu-id="1941c-207">This test passes on SQL Server and SQLite, but fails with the EF in-memory database!</span></span>

```
Assert.False() Failure
Expected: False
Actual:   True
   at Tests.ItemsControllerTest.Can_remove_item_and_all_associated_tags()
```

<span data-ttu-id="1941c-208">В этом случае приложение работает правильно, так как SQL Server поддерживает [каскадное удаление](xref:core/saving/cascade-delete).</span><span class="sxs-lookup"><span data-stu-id="1941c-208">In this case the application is working correctly because SQL Server supports [cascade deletes](xref:core/saving/cascade-delete).</span></span> <span data-ttu-id="1941c-209">SQLite также поддерживает каскадное удаление, как и большинство реляционных баз данных, поэтому тестирование этого файла в SQLite работает.</span><span class="sxs-lookup"><span data-stu-id="1941c-209">SQLite also supports cascade deletes, as do most relational databases, so testing this on SQLite works.</span></span>
<span data-ttu-id="1941c-210">С другой стороны, база данных EF в памяти [не поддерживает каскадное удаление](https://github.com/dotnet/efcore/issues/3924).</span><span class="sxs-lookup"><span data-stu-id="1941c-210">On the other hand, the EF in-memory database [does not support cascade deletes](https://github.com/dotnet/efcore/issues/3924).</span></span>
<span data-ttu-id="1941c-211">Это означает, что эта часть приложения не может быть протестирована с помощью поставщика базы данных EF в памяти.</span><span class="sxs-lookup"><span data-stu-id="1941c-211">This means that this part of the application cannot be tested with the EF in-memory database provider.</span></span>
