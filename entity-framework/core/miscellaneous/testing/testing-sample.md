---
title: ''
description: ''
author: ''
ms.date: ''
uid: ''
no-loc:
- Item
- Tag
- Items
- Tags
ms.openlocfilehash: ae073fc0b3a99fb9de07a3e0a42c638fe0838a5a
ms.sourcegitcommit: 59e3d5ce7dfb284457cf1c991091683b2d1afe9d
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/20/2020
ms.locfileid: "83672812"
---
# <a name="ef-core-testing-sample"></a><span data-ttu-id="5a848-101">Пример тестирования EF Core</span><span class="sxs-lookup"><span data-stu-id="5a848-101">EF Core testing sample</span></span>

> [!TIP]
> <span data-ttu-id="5a848-102">Код в этом документе можно найти на сайте GitHub в качестве [готового к запуску примера](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Testing/ItemsWebApi/).</span><span class="sxs-lookup"><span data-stu-id="5a848-102">The code in this document can be found on GitHub as a [runnable sample](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Testing/ItemsWebApi/).</span></span>
> <span data-ttu-id="5a848-103">Обратите внимание, что некоторые из этих тестов должны **завершаться ошибкой**.</span><span class="sxs-lookup"><span data-stu-id="5a848-103">Note that some of these tests **are expected to fail**.</span></span> <span data-ttu-id="5a848-104">Причины этого описаны ниже.</span><span class="sxs-lookup"><span data-stu-id="5a848-104">The reasons for this are explained below.</span></span> 

<span data-ttu-id="5a848-105">В этом документе рассматривается пример для тестирования кода, который использует EF Core.</span><span class="sxs-lookup"><span data-stu-id="5a848-105">This doc walks through a sample for testing code that uses EF Core.</span></span>

## <a name="the-application"></a><span data-ttu-id="5a848-106">Приложение</span><span class="sxs-lookup"><span data-stu-id="5a848-106">The application</span></span>

<span data-ttu-id="5a848-107">[Пример](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Testing/ItemsWebApi/) содержит два проекта:</span><span class="sxs-lookup"><span data-stu-id="5a848-107">The [sample](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Testing/ItemsWebApi/) contains two projects:</span></span>
- <span data-ttu-id="5a848-108">Итемсвебапи: очень простой [веб-API, поддерживаемый ASP.NET Core](/aspnet/core/tutorials/first-web-api?view=aspnetcore-3.1&tabs=visual-studio) с одним контроллером</span><span class="sxs-lookup"><span data-stu-id="5a848-108">ItemsWebApi: A very simple [Web API backed by ASP.NET Core](/aspnet/core/tutorials/first-web-api?view=aspnetcore-3.1&tabs=visual-studio) with a single controller</span></span>
- <span data-ttu-id="5a848-109">Тесты: тестовый проект [xUnit](https://xunit.net/) для тестирования контроллера</span><span class="sxs-lookup"><span data-stu-id="5a848-109">Tests: An [XUnit](https://xunit.net/) test project to test the controller</span></span>

### <a name="the-model-and-business-rules"></a><span data-ttu-id="5a848-110">Модель и бизнес-правила</span><span class="sxs-lookup"><span data-stu-id="5a848-110">The model and business rules</span></span>

<span data-ttu-id="5a848-111">Модель, которая включает этот API, имеет два типа сущностей: Items и Tags .</span><span class="sxs-lookup"><span data-stu-id="5a848-111">The model backing this API has two entity types: Items and Tags.</span></span>

* Items<span data-ttu-id="5a848-112">зависят от регистра имя и коллекцию Tags .</span><span class="sxs-lookup"><span data-stu-id="5a848-112"> have a case-sensitive name and a collection of Tags.</span></span>
* <span data-ttu-id="5a848-113">Каждый Tag имеет метку и число, представляющее количество применений к Item .</span><span class="sxs-lookup"><span data-stu-id="5a848-113">Each Tag has a label and a count representing the number of times it has been applied to the Item.</span></span>
* <span data-ttu-id="5a848-114">Каждый Item должен иметь только один Tag с заданной меткой.</span><span class="sxs-lookup"><span data-stu-id="5a848-114">Each Item should only have one Tag with a given label.</span></span>
  * <span data-ttu-id="5a848-115">Если элемент помечен одной и той же меткой более одного раза, то число в существующем теге с этой меткой увеличивается вместо создания нового тега.</span><span class="sxs-lookup"><span data-stu-id="5a848-115">If an item is tagged with the same label more than once, then the count on the existing tag with that label is incremented instead of a new tag being created.</span></span> 
* <span data-ttu-id="5a848-116">Удаление Item должно удалить все связанные Tags .</span><span class="sxs-lookup"><span data-stu-id="5a848-116">Deleting an Item should delete all associated Tags.</span></span>

#### <a name="the-item-entity-type"></a><span data-ttu-id="5a848-117">ItemТип сущности</span><span class="sxs-lookup"><span data-stu-id="5a848-117">The Item entity type</span></span>

<span data-ttu-id="5a848-118">`Item`Тип сущности:</span><span class="sxs-lookup"><span data-stu-id="5a848-118">The `Item` entity type:</span></span>

[!code-csharp[ItemEntityType](../../../../samples/core/Miscellaneous/Testing/ItemsWebApi/ItemsWebApi/Item.cs?name=ItemEntityType)]

<span data-ttu-id="5a848-119">И его конфигурация в `DbContext.OnModelCreating` :</span><span class="sxs-lookup"><span data-stu-id="5a848-119">And its configuration in `DbContext.OnModelCreating`:</span></span>

[!code-csharp[ConfigureItem](../../../../samples/core/Miscellaneous/Testing/ItemsWebApi/ItemsWebApi/ItemsContext.cs?name=ConfigureItem)]

<span data-ttu-id="5a848-120">Обратите внимание, что тип сущности ограничивается способом, который можно использовать для отражения модели предметной области и бизнес-правил.</span><span class="sxs-lookup"><span data-stu-id="5a848-120">Notice that entity type constrains the way it can be used to reflect the domain model and business rules.</span></span> <span data-ttu-id="5a848-121">В частности:</span><span class="sxs-lookup"><span data-stu-id="5a848-121">In particular:</span></span>
- <span data-ttu-id="5a848-122">Первичный ключ сопоставляется непосредственно с `_id` полем и не предоставляется публично</span><span class="sxs-lookup"><span data-stu-id="5a848-122">The primary key is mapped directly to the `_id` field and not exposed publicly</span></span>
  - <span data-ttu-id="5a848-123">EF обнаруживает и использует закрытый конструктор, принимающий значение и имя первичного ключа.</span><span class="sxs-lookup"><span data-stu-id="5a848-123">EF detects and uses the private constructor accepting the primary key value and name.</span></span>
- <span data-ttu-id="5a848-124">`Name`Свойство доступно только для чтения и задается только в конструкторе.</span><span class="sxs-lookup"><span data-stu-id="5a848-124">The `Name` property is read-only and set only in the constructor.</span></span> 
- Tags<span data-ttu-id="5a848-125">предоставляются в виде, `IReadOnlyList<Tag>` чтобы предотвратить произвольное изменение.</span><span class="sxs-lookup"><span data-stu-id="5a848-125"> are exposed as a `IReadOnlyList<Tag>` to prevent arbitrary modification.</span></span>
  - <span data-ttu-id="5a848-126">EF связывает `Tags` свойство с `_tags` резервным полем, сопоставляя их имена.</span><span class="sxs-lookup"><span data-stu-id="5a848-126">EF associates the `Tags` property with the `_tags` backing field by matching their names.</span></span> 
  - <span data-ttu-id="5a848-127">`AddTag`Метод принимает метку тега и реализует бизнес-правило, описанное выше.</span><span class="sxs-lookup"><span data-stu-id="5a848-127">The `AddTag` method takes a tag label and implements the business rule described above.</span></span>
    <span data-ttu-id="5a848-128">То есть, тег добавляется только для новых меток.</span><span class="sxs-lookup"><span data-stu-id="5a848-128">That is, a tag is only added for new labels.</span></span>
    <span data-ttu-id="5a848-129">В противном случае значение счетчика в существующей метке увеличивается.</span><span class="sxs-lookup"><span data-stu-id="5a848-129">Otherwise the count on an existing label is incremented.</span></span>
- <span data-ttu-id="5a848-130">`Tags`Свойство навигации настроено для связи "многие к одному"</span><span class="sxs-lookup"><span data-stu-id="5a848-130">The `Tags` navigation property is configured for a many-to-one relationship</span></span>
  - <span data-ttu-id="5a848-131">Нет необходимости в свойстве навигации из Tag Item , поэтому оно не включено.</span><span class="sxs-lookup"><span data-stu-id="5a848-131">There is no need for a navigation property from Tag to Item, so it is not included.</span></span>
  - <span data-ttu-id="5a848-132">Кроме того, не Tag определяет свойство внешнего ключа.</span><span class="sxs-lookup"><span data-stu-id="5a848-132">Also, Tag does not define a foreign key property.</span></span>
    <span data-ttu-id="5a848-133">Вместо этого EF будет создавать свойство в теневом состоянии и управлять им.</span><span class="sxs-lookup"><span data-stu-id="5a848-133">Instead, EF will create and manage a property in shadow-state.</span></span>

#### <a name="the-tag-entity-type"></a><span data-ttu-id="5a848-134">TagТип сущности</span><span class="sxs-lookup"><span data-stu-id="5a848-134">The Tag entity type</span></span>

<span data-ttu-id="5a848-135">`Tag`Тип сущности:</span><span class="sxs-lookup"><span data-stu-id="5a848-135">The `Tag` entity type:</span></span>

[!code-csharp[TagEntityType](../../../../samples/core/Miscellaneous/Testing/ItemsWebApi/ItemsWebApi/Tag.cs?name=TagEntityType)]

<span data-ttu-id="5a848-136">И его конфигурация в `DbContext.OnModelCreating` :</span><span class="sxs-lookup"><span data-stu-id="5a848-136">And its configuration in `DbContext.OnModelCreating`:</span></span>

[!code-csharp[ConfigureTag](../../../../samples/core/Miscellaneous/Testing/ItemsWebApi/ItemsWebApi/ItemsContext.cs?name=ConfigureTag)]

<span data-ttu-id="5a848-137">Аналогично Item Tag скрывает его первичный ключ и делает `Label` свойство доступным только для чтения.</span><span class="sxs-lookup"><span data-stu-id="5a848-137">Similarly to Item, Tag hides its primary key and makes the `Label` property read-only.</span></span>

### <a name="the-itemscontroller"></a><span data-ttu-id="5a848-138">Итемсконтроллер</span><span class="sxs-lookup"><span data-stu-id="5a848-138">The ItemsController</span></span>

<span data-ttu-id="5a848-139">Контроллер веб-API довольно прост.</span><span class="sxs-lookup"><span data-stu-id="5a848-139">The Web API controller is pretty basic.</span></span>
<span data-ttu-id="5a848-140">Он получает `DbContext` из контейнера внедрения зависимостей посредством внедрения конструктора:</span><span class="sxs-lookup"><span data-stu-id="5a848-140">It gets a `DbContext` from the dependency injection container through constructor injection:</span></span>

[!code-csharp[Constructor](../../../../samples/core/Miscellaneous/Testing/ItemsWebApi/ItemsWebApi/Controllers/ItemsController.cs?name=Constructor)]

<span data-ttu-id="5a848-141">Он имеет методы для получения всех Items или Item с заданным именем:</span><span class="sxs-lookup"><span data-stu-id="5a848-141">It has methods to get all Items or an Item with a given name:</span></span>

[!code-csharp[Get](../../../../samples/core/Miscellaneous/Testing/ItemsWebApi/ItemsWebApi/Controllers/ItemsController.cs?name=Get)]

<span data-ttu-id="5a848-142">В нем имеется метод для добавления нового Item :</span><span class="sxs-lookup"><span data-stu-id="5a848-142">It has a method to add a new Item:</span></span>

[!code-csharp[PostItem](../../../../samples/core/Miscellaneous/Testing/ItemsWebApi/ItemsWebApi/Controllers/ItemsController.cs?name=PostItem)]

<span data-ttu-id="5a848-143">Метод для добавления метки к тегу Item с меткой:</span><span class="sxs-lookup"><span data-stu-id="5a848-143">A method to tag an Item with a label:</span></span>

[!code-csharp[PostTag](../../../../samples/core/Miscellaneous/Testing/ItemsWebApi/ItemsWebApi/Controllers/ItemsController.cs?name=PostTag)]

<span data-ttu-id="5a848-144">И метод для удаления Item и все связанные Tags :</span><span class="sxs-lookup"><span data-stu-id="5a848-144">And a method to delete an Item and all associated Tags:</span></span>

[!code-csharp[DeleteItem](../../../../samples/core/Miscellaneous/Testing/ItemsWebApi/ItemsWebApi/Controllers/ItemsController.cs?name=DeleteItem)]

<span data-ttu-id="5a848-145">Большинство проверок и обработка ошибок были удалены для уменьшения перегруженности.</span><span class="sxs-lookup"><span data-stu-id="5a848-145">Most validation and error handling have been removed to reduce clutter.</span></span>

## <a name="the-tests"></a><span data-ttu-id="5a848-146">Тесты</span><span class="sxs-lookup"><span data-stu-id="5a848-146">The Tests</span></span>

<span data-ttu-id="5a848-147">Тесты организованы для выполнения с несколькими конфигурациями поставщика базы данных:</span><span class="sxs-lookup"><span data-stu-id="5a848-147">The tests are organized to run with multiple database provider configurations:</span></span>
* <span data-ttu-id="5a848-148">Поставщик SQL Server, который является поставщиком, используемым приложением.</span><span class="sxs-lookup"><span data-stu-id="5a848-148">The SQL Server provider, which is the provider used by the application</span></span>
* <span data-ttu-id="5a848-149">Поставщик SQLite</span><span class="sxs-lookup"><span data-stu-id="5a848-149">The SQLite provider</span></span>
* <span data-ttu-id="5a848-150">Поставщик SQLite, использующий базы данных SQLite в памяти</span><span class="sxs-lookup"><span data-stu-id="5a848-150">The SQLite provider using in-memory SQLite databases</span></span>
* <span data-ttu-id="5a848-151">Поставщик базы данных EF в памяти</span><span class="sxs-lookup"><span data-stu-id="5a848-151">The EF in-memory database provider</span></span>

<span data-ttu-id="5a848-152">Это достигается путем размещения всех тестов в базовом классе и последующего наследования от него для тестирования с каждым поставщиком.</span><span class="sxs-lookup"><span data-stu-id="5a848-152">This is achieved by putting all the tests in a base class, then inheriting from this to test with each provider.</span></span>

> [!TIP]
> <span data-ttu-id="5a848-153">Если вы не используете LocalDB, необходимо изменить строку подключения SQL Server.</span><span class="sxs-lookup"><span data-stu-id="5a848-153">You will need to change the SQL Server connection string if you're not using LocalDB.</span></span>

> [!TIP]
> <span data-ttu-id="5a848-154">Рекомендации по использованию SQLite для тестирования в памяти см. в разделе [тестирование с помощью Sqlite](xref:core/miscellaneous/testing/sqlite) .</span><span class="sxs-lookup"><span data-stu-id="5a848-154">See [Testing with SQLite](xref:core/miscellaneous/testing/sqlite) for guidance on using SQLite for in-memory testing.</span></span> 

<span data-ttu-id="5a848-155">Ожидается, что следующие два теста завершаются ошибкой:</span><span class="sxs-lookup"><span data-stu-id="5a848-155">The following two tests are expected to fail:</span></span>
* <span data-ttu-id="5a848-156">`Can_remove_item_and_all_associated_tags`При работе с поставщиком базы данных EF в памяти</span><span class="sxs-lookup"><span data-stu-id="5a848-156">`Can_remove_item_and_all_associated_tags` when running with the EF in-memory database provider</span></span>
* <span data-ttu-id="5a848-157">`Can_add_item_differing_only_by_case`При работе с поставщиком SQL Server</span><span class="sxs-lookup"><span data-stu-id="5a848-157">`Can_add_item_differing_only_by_case` when running with the SQL Server provider</span></span>

<span data-ttu-id="5a848-158">Более подробно эти сведения рассматриваются ниже.</span><span class="sxs-lookup"><span data-stu-id="5a848-158">This is covered in more detail below.</span></span>

### <a name="setting-up-and-seeding-the-database"></a><span data-ttu-id="5a848-159">Настройка и заполнение базы данных</span><span class="sxs-lookup"><span data-stu-id="5a848-159">Setting up and seeding the database</span></span>

<span data-ttu-id="5a848-160">XUnit, как и большинство платформ тестирования, создаст новый экземпляр тестового класса для каждого тестового запуска.</span><span class="sxs-lookup"><span data-stu-id="5a848-160">XUnit, like most testing frameworks, will create a new test class instance for each test run.</span></span>
<span data-ttu-id="5a848-161">Кроме того, XUnit не будет выполнять тесты внутри данного тестового класса параллельно.</span><span class="sxs-lookup"><span data-stu-id="5a848-161">Also, XUnit will not run tests within a given test class in parallel.</span></span> <span data-ttu-id="5a848-162">Это означает, что мы можем установить и настроить базу данных в конструкторе тестов, и она будет находиться в известном состоянии для каждого теста.</span><span class="sxs-lookup"><span data-stu-id="5a848-162">This means that we can setup and configure the database in the test constructor and it will be in a well-known state for each test.</span></span>

> [!TIP]
> <span data-ttu-id="5a848-163">Этот пример повторно создает базу данных для каждого теста.</span><span class="sxs-lookup"><span data-stu-id="5a848-163">This sample recreates the database for each test.</span></span>
> <span data-ttu-id="5a848-164">Это хорошо подходит для тестирования SQLite и EF в памяти, но может включать значительные издержки, связанные с другими системами баз данных, включая SQL Server.</span><span class="sxs-lookup"><span data-stu-id="5a848-164">This works well for SQLite and EF in-memory database testing but can involve significant overhead with other database systems, including SQL Server.</span></span>
> <span data-ttu-id="5a848-165">Подходы к сокращению этих затрат рассматриваются в разделе [совместное использование баз данных в разных тестах](xref:core/miscellaneous/testing/sharing-databases).</span><span class="sxs-lookup"><span data-stu-id="5a848-165">Approaches for reducing this overhead are covered in [Sharing databases across tests](xref:core/miscellaneous/testing/sharing-databases).</span></span>

<span data-ttu-id="5a848-166">При выполнении каждого теста:</span><span class="sxs-lookup"><span data-stu-id="5a848-166">When each test is run:</span></span>
* <span data-ttu-id="5a848-167">DbContextOptions настроены для используемого поставщика и передаются конструктору базового класса</span><span class="sxs-lookup"><span data-stu-id="5a848-167">DbContextOptions are configured for the provider in use and passed to the base class constructor</span></span>
  * <span data-ttu-id="5a848-168">Эти параметры хранятся в свойстве и используются во всех тестах для создания экземпляров DbContext.</span><span class="sxs-lookup"><span data-stu-id="5a848-168">These options are stored in a property and used throughout the tests for creating DbContext instances</span></span>
* <span data-ttu-id="5a848-169">Для создания и заполнения базы данных вызывается метод SEED</span><span class="sxs-lookup"><span data-stu-id="5a848-169">A Seed method is called to create and seed the database</span></span>
  * <span data-ttu-id="5a848-170">Метод SEED гарантирует, что база данных будет очищена, удалив ее, а затем повторно создав ее.</span><span class="sxs-lookup"><span data-stu-id="5a848-170">The Seed method ensures the database is clean by deleting it and then re-creating it</span></span>
  * <span data-ttu-id="5a848-171">Некоторые хорошо известные тестовые сущности создаются и сохраняются в базе данных.</span><span class="sxs-lookup"><span data-stu-id="5a848-171">Some well-known test entities are created and saved to the database</span></span>

[!code-csharp[Seeding](../../../../samples/core/Miscellaneous/Testing/ItemsWebApi/Tests/ItemsControllerTest.cs?name=Seeding)]

<span data-ttu-id="5a848-172">Затем каждый конкретный тестовый класс наследуется от этого.</span><span class="sxs-lookup"><span data-stu-id="5a848-172">Each concrete test class then inherits from this.</span></span>
<span data-ttu-id="5a848-173">Пример:</span><span class="sxs-lookup"><span data-stu-id="5a848-173">For example:</span></span>

[!code-csharp[SqliteItemsControllerTest](../../../../samples/core/Miscellaneous/Testing/ItemsWebApi/Tests/SqliteItemsControllerTest.cs?name=SqliteItemsControllerTest)]

### <a name="test-structure"></a><span data-ttu-id="5a848-174">Структура теста</span><span class="sxs-lookup"><span data-stu-id="5a848-174">Test structure</span></span>

<span data-ttu-id="5a848-175">Несмотря на то, что приложение использует внедрение зависимостей, тесты не имеют.</span><span class="sxs-lookup"><span data-stu-id="5a848-175">Even though the application uses dependency injection, the tests do not.</span></span>
<span data-ttu-id="5a848-176">Здесь было бы неплохо использовать внедрение зависимостей, но дополнительный код, который он требует, имеет небольшую ценность.</span><span class="sxs-lookup"><span data-stu-id="5a848-176">It would be fine to use dependency injection here, but the additional code it requires has little value.</span></span>
<span data-ttu-id="5a848-177">Вместо этого DbContext создается с помощью `new` , а затем напрямую передается в качестве зависимости контроллеру.</span><span class="sxs-lookup"><span data-stu-id="5a848-177">Instead, a DbContext is created using `new` and then directly passed as the dependency to the controller.</span></span>

<span data-ttu-id="5a848-178">Затем каждый тест выполняет тестируемый метод на контроллере и утверждает результаты ожидаемым образом.</span><span class="sxs-lookup"><span data-stu-id="5a848-178">Each test then executes the method under test on the controller and asserts the results are as expected.</span></span>
<span data-ttu-id="5a848-179">Пример:</span><span class="sxs-lookup"><span data-stu-id="5a848-179">For example:</span></span>

[!code-csharp[CanGetItems](../../../../samples/core/Miscellaneous/Testing/ItemsWebApi/Tests/ItemsControllerTest.cs?name=CanGetItems)]

<span data-ttu-id="5a848-180">Обратите внимание, что для заполнения базы данных и выполнения тестов используются различные экземпляры DbContext.</span><span class="sxs-lookup"><span data-stu-id="5a848-180">Notice that different DbContext instances are used to seed the database and run the tests.</span></span> <span data-ttu-id="5a848-181">Это гарантирует, что тест не использует (или не переходит) сущности, которые отслеживает контекст при заполнении.</span><span class="sxs-lookup"><span data-stu-id="5a848-181">This ensures that the test is not using (or tripping over) entities tracked by the context when seeding.</span></span>
<span data-ttu-id="5a848-182">Он также лучше соответствует тому, что происходит в веб-приложениях и службах.</span><span class="sxs-lookup"><span data-stu-id="5a848-182">It also better matches what happens in web apps and services.</span></span>

<span data-ttu-id="5a848-183">Тесты, изменяющие базу данных, создают второй экземпляр DbContext в тесте по тем же причинам.</span><span class="sxs-lookup"><span data-stu-id="5a848-183">Tests that mutate the database create a second DbContext instance in the test for similar reasons.</span></span>
<span data-ttu-id="5a848-184">То есть создание нового, чистого и последующего контекста, а затем чтение из базы данных, чтобы убедиться, что изменения были сохранены в базе данных.</span><span class="sxs-lookup"><span data-stu-id="5a848-184">That is, creating a new, clean, context and then reading into it from the database to ensure that the changes were saved to the database.</span></span> <span data-ttu-id="5a848-185">Пример:</span><span class="sxs-lookup"><span data-stu-id="5a848-185">For example:</span></span>

[!code-csharp[CanAddItem](../../../../samples/core/Miscellaneous/Testing/ItemsWebApi/Tests/ItemsControllerTest.cs?name=CanAddItem)]

<span data-ttu-id="5a848-186">Два еще более задействованных теста охватывают бизнес-логику вокруг добавления тегов.</span><span class="sxs-lookup"><span data-stu-id="5a848-186">Two slightly more involved tests cover the business logic around adding tags.</span></span>

[!code-csharp[CanAddTag](../../../../samples/core/Miscellaneous/Testing/ItemsWebApi/Tests/ItemsControllerTest.cs?name=CanAddTag)]

[!code-csharp[CanUpTagCount](../../../../samples/core/Miscellaneous/Testing/ItemsWebApi/Tests/ItemsControllerTest.cs?name=CanUpTagCount)]

## <a name="issues-using-different-database-providers"></a><span data-ttu-id="5a848-187">Проблемы с использованием разных поставщиков баз данных</span><span class="sxs-lookup"><span data-stu-id="5a848-187">Issues using different database providers</span></span>

<span data-ttu-id="5a848-188">Тестирование с использованием другой системы баз данных, чем используется в рабочем приложении, может привести к проблемам.</span><span class="sxs-lookup"><span data-stu-id="5a848-188">Testing with a different database system than is used in the production application can lead to problems.</span></span>
<span data-ttu-id="5a848-189">Они описаны на концептуальном уровне в [тестовом коде, который использует EF Core](xref:core/miscellaneous/testing/index).</span><span class="sxs-lookup"><span data-stu-id="5a848-189">These are covered at the conceptual level in [Testing code that uses EF Core](xref:core/miscellaneous/testing/index).</span></span>  
<span data-ttu-id="5a848-190">В следующих разделах рассматриваются два примера таких проблем, продемонстрированных в тестах в этом примере.</span><span class="sxs-lookup"><span data-stu-id="5a848-190">The sections below cover two examples of such issues demonstrated by the tests in this sample.</span></span>

### <a name="test-passes-when-the-application-is-broken"></a><span data-ttu-id="5a848-191">Тест проходит успешно при нарушении приложения</span><span class="sxs-lookup"><span data-stu-id="5a848-191">Test passes when the application is broken</span></span>

<span data-ttu-id="5a848-192">Одно из требований для нашего приложения заключается в том, что для этого используется Items имя с учетом регистра и коллекция Tags .</span><span class="sxs-lookup"><span data-stu-id="5a848-192">One of the requirements for our application is that "Items have a case-sensitive name and a collection of Tags."</span></span>
<span data-ttu-id="5a848-193">Это довольно просто для тестирования:</span><span class="sxs-lookup"><span data-stu-id="5a848-193">This is pretty simple to test:</span></span>

[!code-csharp[CanAddItemCaseInsensitive](../../../../samples/core/Miscellaneous/Testing/ItemsWebApi/Tests/ItemsControllerTest.cs?name=CanAddItemCaseInsensitive)]

<span data-ttu-id="5a848-194">Выполнение этого теста для базы данных EF в памяти означает, что все правильно.</span><span class="sxs-lookup"><span data-stu-id="5a848-194">Running this test against the EF in-memory database indicates that everything is fine.</span></span>
<span data-ttu-id="5a848-195">При использовании SQLite все еще выглядит нормально.</span><span class="sxs-lookup"><span data-stu-id="5a848-195">Everything still looks fine when using SQLite.</span></span>
<span data-ttu-id="5a848-196">Но тест завершается ошибкой при запуске SQL Server!</span><span class="sxs-lookup"><span data-stu-id="5a848-196">But the test fails when run against SQL Server!</span></span>

```
System.InvalidOperationException : Sequence contains more than one element
   at System.Linq.ThrowHelper.ThrowMoreThanOneElementException()
   at System.Linq.Enumerable.Single[TSource](IEnumerable`1 source)
   at Microsoft.EntityFrameworkCore.Query.Internal.QueryCompiler.Execute[TResult](Expression query)
   at Microsoft.EntityFrameworkCore.Query.Internal.EntityQueryProvider.Execute[TResult](Expression expression)
   at System.Linq.Queryable.Single[TSource](IQueryable`1 source, Expression`1 predicate)
   at Tests.ItemsControllerTest.Can_add_item_differing_only_by_case()
```

<span data-ttu-id="5a848-197">Это происходит потому, что по умолчанию учитывается регистр в базе данных EF в памяти и в базе данных SQLite.</span><span class="sxs-lookup"><span data-stu-id="5a848-197">This is because both the EF in-memory database and the SQLite database are case-sensitive by default.</span></span>
<span data-ttu-id="5a848-198">SQL Server, с другой стороны, не учитывает регистр.</span><span class="sxs-lookup"><span data-stu-id="5a848-198">SQL Server, on the other hand, is case-insensitive!</span></span> 

<span data-ttu-id="5a848-199">EF Core, по дизайну, не изменяет эти поведения, поскольку принудительное изменение чувствительности к регистру может существенно повлиять на производительность.</span><span class="sxs-lookup"><span data-stu-id="5a848-199">EF Core, by design, does not change these behaviors because forcing a change in case-sensitivity can have a big performance impact.</span></span>

<span data-ttu-id="5a848-200">Когда мы понимаем, что это проблема, можно исправить приложение и компенсировать тесты.</span><span class="sxs-lookup"><span data-stu-id="5a848-200">Once we know this is a problem we can fix the application and compensate in tests.</span></span>
<span data-ttu-id="5a848-201">Однако в этом случае эта ошибка может быть пропущена, если только тестирование выполняется с использованием в базе данных EF в памяти или поставщиков SQLite.</span><span class="sxs-lookup"><span data-stu-id="5a848-201">However, the point here is that this bug could be missed if only testing with the EF in-memory database or SQLite providers.</span></span>

### <a name="test-fails-when-the-application-is-correct"></a><span data-ttu-id="5a848-202">Проверка завершается ошибкой, если приложение является правильным</span><span class="sxs-lookup"><span data-stu-id="5a848-202">Test fails when the application is correct</span></span> 

<span data-ttu-id="5a848-203">Другим требованием для нашего приложения является то, что "Удаление Item должно удалить все связанные Tags ".</span><span class="sxs-lookup"><span data-stu-id="5a848-203">Another of the requirements for our application is that "deleting an Item should delete all associated Tags."</span></span>
<span data-ttu-id="5a848-204">Опять же, легко тестировать:</span><span class="sxs-lookup"><span data-stu-id="5a848-204">Again, easy to test:</span></span>

[!code-csharp[DeleteItem](../../../../samples/core/Miscellaneous/Testing/ItemsWebApi/Tests/ItemsControllerTest.cs?name=DeleteItem)]

<span data-ttu-id="5a848-205">Этот тест передается в SQL Server и SQLite, но завершается сбоем с базой данных EF в памяти!</span><span class="sxs-lookup"><span data-stu-id="5a848-205">This test passes on SQL Server and SQLite, but fails with the EF in-memory database!</span></span>

```
Assert.False() Failure
Expected: False
Actual:   True
   at Tests.ItemsControllerTest.Can_remove_item_and_all_associated_tags()
```

<span data-ttu-id="5a848-206">В этом случае приложение работает правильно, так как SQL Server поддерживает [каскадное удаление](xref:core/saving/cascade-delete).</span><span class="sxs-lookup"><span data-stu-id="5a848-206">In this case, the application is working correctly because SQL Server supports [cascade deletes](xref:core/saving/cascade-delete).</span></span> <span data-ttu-id="5a848-207">SQLite также поддерживает каскадное удаление, как и большинство реляционных баз данных, поэтому тестирование этого файла в SQLite работает.</span><span class="sxs-lookup"><span data-stu-id="5a848-207">SQLite also supports cascade deletes, as do most relational databases, so testing this on SQLite works.</span></span>
<span data-ttu-id="5a848-208">С другой стороны, база данных EF в памяти [не поддерживает каскадное удаление](https://github.com/dotnet/efcore/issues/3924).</span><span class="sxs-lookup"><span data-stu-id="5a848-208">On the other hand, the EF in-memory database [does not support cascade deletes](https://github.com/dotnet/efcore/issues/3924).</span></span>
<span data-ttu-id="5a848-209">Это означает, что эта часть приложения не может быть протестирована с помощью поставщика базы данных EF в памяти.</span><span class="sxs-lookup"><span data-stu-id="5a848-209">This means that this part of the application cannot be tested with the EF in-memory database provider.</span></span>
