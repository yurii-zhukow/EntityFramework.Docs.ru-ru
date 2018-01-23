---
title: "Выполняется загрузка связанных данных — основной EF"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: f9fb64e2-6699-4d70-a773-592918c04c19
ms.technology: entity-framework-core
uid: core/querying/related-data
ms.openlocfilehash: ec69bb128890a1e0b72fe77014f37747585bb5a5
ms.sourcegitcommit: 3b21a7fdeddc7b3c70d9b7777b72bef61f59216c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/22/2018
---
# <a name="loading-related-data"></a><span data-ttu-id="e7462-102">Загрузка связанных данных</span><span class="sxs-lookup"><span data-stu-id="e7462-102">Loading Related Data</span></span>

<span data-ttu-id="e7462-103">Entity Framework Core позволяет использовать свойства навигации в модели для загрузки связанных сущностей.</span><span class="sxs-lookup"><span data-stu-id="e7462-103">Entity Framework Core allows you to use the navigation properties in your model to load related entities.</span></span> <span data-ttu-id="e7462-104">Существует три общих шаблонов O/надежный обмен Сообщениями, используемые для загрузки связанных данных.</span><span class="sxs-lookup"><span data-stu-id="e7462-104">There are three common O/RM patterns used to load related data.</span></span>
* <span data-ttu-id="e7462-105">**Безотложная загрузка** означает, что данные загружены из базы данных как часть исходного запроса.</span><span class="sxs-lookup"><span data-stu-id="e7462-105">**Eager loading** means that the related data is loaded from the database as part of the initial query.</span></span>
* <span data-ttu-id="e7462-106">**Явная загрузка** означает, что связанные данные явно загружен из базы данных в дальнейшем.</span><span class="sxs-lookup"><span data-stu-id="e7462-106">**Explicit loading** means that the related data is explicitly loaded from the database at a later time.</span></span>
* <span data-ttu-id="e7462-107">**Отложенная загрузка** означает, что связанные прозрачно загрузки данных из базы данных при доступе к свойству навигации.</span><span class="sxs-lookup"><span data-stu-id="e7462-107">**Lazy loading** means that the related data is transparently loaded from the database when the navigation property is accessed.</span></span> <span data-ttu-id="e7462-108">Отложенная загрузка не возможно при EF Core.</span><span class="sxs-lookup"><span data-stu-id="e7462-108">Lazy loading is not yet possible with EF Core.</span></span>

> [!TIP]  
> <span data-ttu-id="e7462-109">Для этой статьи вы можете скачать [пример](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) из репозитория GitHub.</span><span class="sxs-lookup"><span data-stu-id="e7462-109">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) on GitHub.</span></span>

## <a name="eager-loading"></a><span data-ttu-id="e7462-110">Безотложная загрузка</span><span class="sxs-lookup"><span data-stu-id="e7462-110">Eager loading</span></span>

<span data-ttu-id="e7462-111">Можно использовать `Include` метод, чтобы задать связанные данные, которые будут включены в результаты запроса.</span><span class="sxs-lookup"><span data-stu-id="e7462-111">You can use the `Include` method to specify related data to be included in query results.</span></span> <span data-ttu-id="e7462-112">В следующем примере, блоги, которые возвращаются в результатах будут иметь их `Posts` свойство заполняется связанных сообщений.</span><span class="sxs-lookup"><span data-stu-id="e7462-112">In the following example, the blogs that are returned in the results will have their `Posts` property populated with the related posts.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#SingleInclude)]

> [!TIP]  
> <span data-ttu-id="e7462-113">Entity Framework Core будет свойства навигации автоматически исправить вверх с другими сущностями, которые были ранее загружены в экземпляр контекста.</span><span class="sxs-lookup"><span data-stu-id="e7462-113">Entity Framework Core will automatically fix-up navigation properties to any other entities that were previously loaded into the context instance.</span></span> <span data-ttu-id="e7462-114">Поэтому даже если данные для свойства навигации не включить явным образом, свойство по-прежнему могут быть заполнены, если некоторые или все связанные сущности были ранее загружены.</span><span class="sxs-lookup"><span data-stu-id="e7462-114">So even if you don't explicitly include the data for a navigation property, the property may still be populated if some or all of the related entities were previously loaded.</span></span>


<span data-ttu-id="e7462-115">Связанные данные из нескольких связей может включать в одном запросе.</span><span class="sxs-lookup"><span data-stu-id="e7462-115">You can include related data from multiple relationships in a single query.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#MultipleIncludes)]

### <a name="including-multiple-levels"></a><span data-ttu-id="e7462-116">Включая несколько уровней</span><span class="sxs-lookup"><span data-stu-id="e7462-116">Including multiple levels</span></span>

<span data-ttu-id="e7462-117">Можно выполнить детализацию до связи для включения нескольких уровней связанных данных с помощью `ThenInclude` метод.</span><span class="sxs-lookup"><span data-stu-id="e7462-117">You can drill down thru relationships to include multiple levels of related data using the `ThenInclude` method.</span></span> <span data-ttu-id="e7462-118">В следующем примере загружаются все блоги, их связанных сообщений и автор каждой отправки.</span><span class="sxs-lookup"><span data-stu-id="e7462-118">The following example loads all blogs, their related posts, and the author of each post.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#SingleThenInclude)]

> [!NOTE]  
> <span data-ttu-id="e7462-119">Текущие версии Visual Studio имеются средства завершения неверного кода и может привести к правильные выражения устанавливается флаг синтаксические ошибки при использовании `ThenInclude` метод после свойства навигации коллекции.</span><span class="sxs-lookup"><span data-stu-id="e7462-119">Current versions of Visual Studio offer incorrect code completion options and can cause correct expressions to be flagged with syntax errors when using the `ThenInclude` method after a collection navigation property.</span></span> <span data-ttu-id="e7462-120">Это является признаком ошибка в IntelliSense, отслеживаются в https://github.com/dotnet/roslyn/issues/8237.</span><span class="sxs-lookup"><span data-stu-id="e7462-120">This is a symptom of an IntelliSense bug tracked at https://github.com/dotnet/roslyn/issues/8237.</span></span> <span data-ttu-id="e7462-121">Его можно игнорировать эти ложные синтаксических ошибок, при условии, что код указано правильно и успешно скомпилирован.</span><span class="sxs-lookup"><span data-stu-id="e7462-121">It is safe to ignore these spurious syntax errors as long as the code is correct and can be compiled successfully.</span></span> 

<span data-ttu-id="e7462-122">Можно соединить в цепочку несколько вызовов `ThenInclude` чтобы продолжить, включая дополнительные уровни взаимосвязанных данных.</span><span class="sxs-lookup"><span data-stu-id="e7462-122">You can chain multiple calls to `ThenInclude` to continue including further levels of related data.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#MultipleThenIncludes)]

<span data-ttu-id="e7462-123">Можно объединить все это для включения связанных данных из нескольких уровней и нескольких корней в одном запросе.</span><span class="sxs-lookup"><span data-stu-id="e7462-123">You can combine all of this to include related data from multiple levels and multiple roots in the same query.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#IncludeTree)]

<span data-ttu-id="e7462-124">Может потребоваться включить несколько связанных сущностей для одной сущности, включенного.</span><span class="sxs-lookup"><span data-stu-id="e7462-124">You may want to include multiple related entities for one of the entities that is being included.</span></span> <span data-ttu-id="e7462-125">Например, при запросе `Blog`s, включают `Posts` и затем необходимо включить оба `Author` и `Tags` из `Posts`.</span><span class="sxs-lookup"><span data-stu-id="e7462-125">For example, when querying `Blog`s, you include `Posts` and then want to include both the `Author` and `Tags` of the `Posts`.</span></span> <span data-ttu-id="e7462-126">Чтобы сделать это, необходимо указать, имеют путь, начиная с корневого каталога.</span><span class="sxs-lookup"><span data-stu-id="e7462-126">To do this, you need to specify each include path starting at the root.</span></span> <span data-ttu-id="e7462-127">Например `Blog -> Posts -> Author` и `Blog -> Posts -> Tags`.</span><span class="sxs-lookup"><span data-stu-id="e7462-127">For example, `Blog -> Posts -> Author` and `Blog -> Posts -> Tags`.</span></span> <span data-ttu-id="e7462-128">Это значит, что вы получите избыточных соединений, в большинстве случаев будет консолидировать EF соединений при создании кода SQL.</span><span class="sxs-lookup"><span data-stu-id="e7462-128">This does not mean you will get redundant joins, in most cases EF will consolidate the joins when generating SQL.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#MultipleLeafIncludes)]

### <a name="ignored-includes"></a><span data-ttu-id="e7462-129">Учитывается включает</span><span class="sxs-lookup"><span data-stu-id="e7462-129">Ignored includes</span></span>

<span data-ttu-id="e7462-130">Если изменить запрос, чтобы он больше не возвращает экземпляры типа сущности, запрос начинающийся с, операторы include игнорируются.</span><span class="sxs-lookup"><span data-stu-id="e7462-130">If you change the query so that it no longer returns instances of the entity type that the query began with, then the include operators are ignored.</span></span>

<span data-ttu-id="e7462-131">В следующем примере операторы include основаны на `Blog`, а затем `Select` оператор используется для изменения запроса для возврата анонимного типа.</span><span class="sxs-lookup"><span data-stu-id="e7462-131">In the following example, the include operators are based on the `Blog`, but then the `Select` operator is used to change the query to return an anonymous type.</span></span> <span data-ttu-id="e7462-132">В этом случае операторы include не действуют.</span><span class="sxs-lookup"><span data-stu-id="e7462-132">In this case, the include operators have no effect.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#IgnoredInclude)]

<span data-ttu-id="e7462-133">По умолчанию основные EF регистрируют предупреждения при включении операторы игнорируются.</span><span class="sxs-lookup"><span data-stu-id="e7462-133">By default, EF Core will log a warning when include operators are ignored.</span></span> <span data-ttu-id="e7462-134">В разделе [входа](../miscellaneous/logging.md) Дополнительные сведения о просмотре выходных данных ведения журнала.</span><span class="sxs-lookup"><span data-stu-id="e7462-134">See [Logging](../miscellaneous/logging.md) for more information on viewing logging output.</span></span> <span data-ttu-id="e7462-135">Можно изменить поведение при оператор include игнорируется выдать или не выполняют никаких действий.</span><span class="sxs-lookup"><span data-stu-id="e7462-135">You can change the behavior when an include operator is ignored to either throw or do nothing.</span></span> <span data-ttu-id="e7462-136">Это делается при настройке параметров для текущего контекста — обычно в `DbContext.OnConfiguring`, или в `Startup.cs` при использовании ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="e7462-136">This is done when setting up the options for your context - typically in `DbContext.OnConfiguring`, or in `Startup.cs` if you are using ASP.NET Core.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/ThrowOnIgnoredInclude/BloggingContext.cs#OnConfiguring)]

## <a name="explicit-loading"></a><span data-ttu-id="e7462-137">Явная загрузка</span><span class="sxs-lookup"><span data-stu-id="e7462-137">Explicit loading</span></span>

> [!NOTE]  
> <span data-ttu-id="e7462-138">Эта функция появилась в версии 1.1 EF Core.</span><span class="sxs-lookup"><span data-stu-id="e7462-138">This feature was introduced in EF Core 1.1.</span></span>

<span data-ttu-id="e7462-139">Можно явно загрузить через свойство навигации `DbContext.Entry(...)` API.</span><span class="sxs-lookup"><span data-stu-id="e7462-139">You can explicitly load a navigation property via the `DbContext.Entry(...)` API.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#Eager)]

<span data-ttu-id="e7462-140">Можно также явно загрузить свойство навигации путем выполнения отдельного запроса, который возвращает связанные сущности.</span><span class="sxs-lookup"><span data-stu-id="e7462-140">You can also explicitly load a navigation property by executing a seperate query that returns the related entities.</span></span> <span data-ttu-id="e7462-141">Если включено отслеживание изменений, то при загрузке сущности, EF Core будет автоматически задать свойства навигации объектов загруженных для ссылки на все сущности, которые уже загружены и задать свойства навигации для ссылки на сущности уже загружен загруженных сущность.</span><span class="sxs-lookup"><span data-stu-id="e7462-141">If change tracking is enabled, then when loading an entity, EF Core will automatically set the navigation properties of the newly-loaded entitiy to refer to any entities already loaded, and set the navigation properties of the already-loaded entities to refer to the newly-loaded entity.</span></span>

### <a name="querying-related-entities"></a><span data-ttu-id="e7462-142">Запрос связанных сущностей</span><span class="sxs-lookup"><span data-stu-id="e7462-142">Querying related entities</span></span>

<span data-ttu-id="e7462-143">Можно также получить запрос LINQ, который представляет содержимое свойства навигации.</span><span class="sxs-lookup"><span data-stu-id="e7462-143">You can also get a LINQ query that represents the contents of a navigation property.</span></span>

<span data-ttu-id="e7462-144">Это дает возможность выполнения действий, таких как запуск статистических операторов для связанных сущностей без их загрузки в память.</span><span class="sxs-lookup"><span data-stu-id="e7462-144">This allows you to do things such as running an aggregate operator over the related entities without loading them into memory.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#NavQueryAggregate)]

<span data-ttu-id="e7462-145">Можно также фильтровать связанные сущности загружаются в память.</span><span class="sxs-lookup"><span data-stu-id="e7462-145">You can also filter which related entities are loaded into memory.</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#NavQueryFiltered)]

## <a name="lazy-loading"></a><span data-ttu-id="e7462-146">«Неспешная» загрузка</span><span class="sxs-lookup"><span data-stu-id="e7462-146">Lazy loading</span></span>

<span data-ttu-id="e7462-147">Отложенная загрузка ядром EF еще не поддерживается.</span><span class="sxs-lookup"><span data-stu-id="e7462-147">Lazy loading is not yet supported by EF Core.</span></span> <span data-ttu-id="e7462-148">Можно просмотреть [отложенную загрузку элемента в невыполненной работой](https://github.com/aspnet/EntityFramework/issues/3797) для отслеживания этой функции.</span><span class="sxs-lookup"><span data-stu-id="e7462-148">You can view the [lazy loading item on our backlog](https://github.com/aspnet/EntityFramework/issues/3797) to track this feature.</span></span>

## <a name="related-data-and-serialization"></a><span data-ttu-id="e7462-149">Связанные данные и сериализации</span><span class="sxs-lookup"><span data-stu-id="e7462-149">Related data and serialization</span></span>

<span data-ttu-id="e7462-150">Поскольку свойства навигации автоматически исправить вверх будет EF Core, можно получить с циклами в граф объекта.</span><span class="sxs-lookup"><span data-stu-id="e7462-150">Because EF Core will automatically fix-up navigation properties, you can end up with cycles in your object graph.</span></span> <span data-ttu-id="e7462-151">Например загрузка блог и он не связан приведет к записи в блоге объект, который ссылается на коллекцию записей.</span><span class="sxs-lookup"><span data-stu-id="e7462-151">For example, Loading a blog and it's related posts will result in a blog object that references a collection of posts.</span></span> <span data-ttu-id="e7462-152">Каждый из этих записей будет ссылкой на блога.</span><span class="sxs-lookup"><span data-stu-id="e7462-152">Each of those posts will have a reference back to the blog.</span></span>

<span data-ttu-id="e7462-153">Некоторые платформы сериализации не допускают такие циклы.</span><span class="sxs-lookup"><span data-stu-id="e7462-153">Some serialization frameworks do not allow such cycles.</span></span> <span data-ttu-id="e7462-154">Например Json.NET вызовет следующее исключение, если цикл встречено.</span><span class="sxs-lookup"><span data-stu-id="e7462-154">For example, Json.NET will throw the following exception if a cycle is encoutered.</span></span>

> <span data-ttu-id="e7462-155">Newtonsoft.Json.JsonSerializationException: Self ссылается обнаружен для свойства «Блог» с типом «MyApplication.Models.Blog» цикл.</span><span class="sxs-lookup"><span data-stu-id="e7462-155">Newtonsoft.Json.JsonSerializationException: Self referencing loop detected for property 'Blog' with type 'MyApplication.Models.Blog'.</span></span>

<span data-ttu-id="e7462-156">При использовании ASP.NET Core можно настроить Json.NET игнорировать циклов, найденные в графе объекта.</span><span class="sxs-lookup"><span data-stu-id="e7462-156">If you are using ASP.NET Core, you can configure Json.NET to ignore cycles that it finds in the object graph.</span></span> <span data-ttu-id="e7462-157">Это можно сделать в `ConfigureServices(...)` метод в `Startup.cs`.</span><span class="sxs-lookup"><span data-stu-id="e7462-157">This is done in the `ConfigureServices(...)` method in `Startup.cs`.</span></span>

``` csharp
public void ConfigureServices(IServiceCollection services)
{
    ...

    services.AddMvc()
        .AddJsonOptions(
            options => options.SerializerSettings.ReferenceLoopHandling = Newtonsoft.Json.ReferenceLoopHandling.Ignore
        );

    ...
}
```
