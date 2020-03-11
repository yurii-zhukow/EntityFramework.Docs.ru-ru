---
title: Предварительно созданные представления сопоставления — EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 917ba9c8-6ddf-4631-ab8c-c4fb378c2fcd
ms.openlocfilehash: 1fda9fe9638adce9b24a6b81aa081effeb0def81
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/06/2020
ms.locfileid: "78416033"
---
# <a name="pre-generated-mapping-views"></a><span data-ttu-id="54810-102">Предварительно созданные представления сопоставления</span><span class="sxs-lookup"><span data-stu-id="54810-102">Pre-generated mapping views</span></span>
<span data-ttu-id="54810-103">Прежде чем Entity Framework сможет выполнить запрос или сохранить изменения в источнике данных, он должен создать набор представлений сопоставления для доступа к базе данных.</span><span class="sxs-lookup"><span data-stu-id="54810-103">Before the Entity Framework can execute a query or save changes to the data source, it must generate a set of mapping views to access the database.</span></span> <span data-ttu-id="54810-104">Эти представления сопоставления представляют собой набор Entity SQL инструкций, которые представляют базу данных абстрактным способом и являются частью метаданных, которые кэшируются для каждого домена приложения.</span><span class="sxs-lookup"><span data-stu-id="54810-104">These mapping views are a set of Entity SQL statement that represent the database in an abstract way, and are part of the metadata which is cached per application domain.</span></span> <span data-ttu-id="54810-105">При создании нескольких экземпляров одного и того же контекста в одном домене приложения они будут повторно использовать представления сопоставления из кэшированных метаданных, а не воссоздавать их.</span><span class="sxs-lookup"><span data-stu-id="54810-105">If you create multiple instances of the same context in the same application domain, they will reuse mapping views from the cached metadata rather than regenerating them.</span></span> <span data-ttu-id="54810-106">Поскольку создание представления сопоставления является важной частью общей стоимости выполнения первого запроса, Entity Framework позволяет предварительно создавать представления сопоставления и включать их в скомпилированный проект.</span><span class="sxs-lookup"><span data-stu-id="54810-106">Because mapping view generation is a significant part of the overall cost of executing the first query, the Entity Framework enables you to pre-generate mapping views and include them in the compiled project.</span></span><span data-ttu-id="54810-107"> Дополнительные сведения см. в разделе  [вопросы производительности (Entity Framework)](~/ef6/fundamentals/performance/perf-whitepaper.md).</span><span class="sxs-lookup"><span data-stu-id="54810-107"> For more information, see  [Performance Considerations (Entity Framework)](~/ef6/fundamentals/performance/perf-whitepaper.md).</span></span>

## <a name="generating-mapping-views-with-the-ef-power-tools-community-edition"></a><span data-ttu-id="54810-108">Создание представлений сопоставления с помощью EF Power Tools Community Edition</span><span class="sxs-lookup"><span data-stu-id="54810-108">Generating Mapping Views with the EF Power Tools Community Edition</span></span>

<span data-ttu-id="54810-109">Самый простой способ предварительно создать представления — использовать [EF Power Tools Community Edition](https://marketplace.visualstudio.com/items?itemName=ErikEJ.EntityFramework6PowerToolsCommunityEdition).</span><span class="sxs-lookup"><span data-stu-id="54810-109">The easiest way to pre-generate views is to use the [EF Power Tools Community Edition](https://marketplace.visualstudio.com/items?itemName=ErikEJ.EntityFramework6PowerToolsCommunityEdition).</span></span> <span data-ttu-id="54810-110">После установки Power Tools появится пункт меню для создания представлений, как показано ниже.</span><span class="sxs-lookup"><span data-stu-id="54810-110">Once you have the Power Tools installed you will have a menu option to Generate Views, as below.</span></span>

-   <span data-ttu-id="54810-111">Для **Code First** моделей щелкните правой кнопкой мыши файл кода, содержащий класс DbContext.</span><span class="sxs-lookup"><span data-stu-id="54810-111">For **Code First** models right-click on the code file that contains your DbContext class.</span></span>
-   <span data-ttu-id="54810-112">Для моделей **конструктора EF** щелкните правой кнопкой мыши EDMX-файл.</span><span class="sxs-lookup"><span data-stu-id="54810-112">For **EF Designer** models right-click on your EDMX file.</span></span>

![Создание представлений](~/ef6/media/generateviews.png)

<span data-ttu-id="54810-114">По завершении процесса будет создан класс, аналогичный приведенному ниже.</span><span class="sxs-lookup"><span data-stu-id="54810-114">Once the process is finished you will have a class similar to the following generated</span></span>

![созданные представления](~/ef6/media/generatedviews.png)

<span data-ttu-id="54810-116">Теперь при запуске приложения EF будет использовать этот класс для загрузки представлений по мере необходимости.</span><span class="sxs-lookup"><span data-stu-id="54810-116">Now when you run your application EF will use this class to load views as required.</span></span> <span data-ttu-id="54810-117">Если модель изменяется и вы не создаете этот класс повторно, EF выдаст исключение.</span><span class="sxs-lookup"><span data-stu-id="54810-117">If your model changes and you do not re-generate this class then EF will throw an exception.</span></span>

## <a name="generating-mapping-views-from-code---ef6-onwards"></a><span data-ttu-id="54810-118">Создание представлений сопоставления из Code-EF6/назад</span><span class="sxs-lookup"><span data-stu-id="54810-118">Generating Mapping Views from Code - EF6 Onwards</span></span>

<span data-ttu-id="54810-119">Другим способом создания представлений является использование API, предоставляемых EF.</span><span class="sxs-lookup"><span data-stu-id="54810-119">The other way to generate views is to use the APIs that EF provides.</span></span> <span data-ttu-id="54810-120">При использовании этого метода у вас есть возможность сериализовать представления, но вам необходимо также загрузить представления самостоятельно.</span><span class="sxs-lookup"><span data-stu-id="54810-120">When using this method you have the freedom to serialize the views however you like, but you also need to load the views yourself.</span></span>

> [!NOTE]
> <span data-ttu-id="54810-121">**EF6 только** для чтения. API-интерфейсы, показанные в этом разделе, появились в Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="54810-121">**EF6 Onwards Only** - The APIs shown in this section were introduced in Entity Framework 6.</span></span> <span data-ttu-id="54810-122">Если вы используете более раннюю версию, эти сведения не применяются.</span><span class="sxs-lookup"><span data-stu-id="54810-122">If you are using an earlier version this information does not apply.</span></span>

### <a name="generating-views"></a><span data-ttu-id="54810-123">Создание представлений</span><span class="sxs-lookup"><span data-stu-id="54810-123">Generating Views</span></span>

<span data-ttu-id="54810-124">API-интерфейсы для создания представлений находятся в классе System. Data. Entity. Core. mapping. Сторажемаппингитемколлектион.</span><span class="sxs-lookup"><span data-stu-id="54810-124">The APIs to generate views are on the System.Data.Entity.Core.Mapping.StorageMappingItemCollection class.</span></span> <span data-ttu-id="54810-125">Вы можете получить Сторажемаппингколлектион для контекста с помощью области MetadataWorkspace контекста ObjectContext.</span><span class="sxs-lookup"><span data-stu-id="54810-125">You can retrieve a StorageMappingCollection for a Context by using the MetadataWorkspace of an ObjectContext.</span></span> <span data-ttu-id="54810-126">Если вы используете новый API DbContext, вы можете получить к нему доступ с помощью Иобжектконтекстадаптер, как показано ниже, в этом коде имеется экземпляр производного DbContext с именем dbContext:</span><span class="sxs-lookup"><span data-stu-id="54810-126">If you are using the newer DbContext API then you can access this by using the IObjectContextAdapter like below, in this code we have an instance of your derived DbContext called dbContext:</span></span>

``` csharp
    var objectContext = ((IObjectContextAdapter) dbContext).ObjectContext;
    var  mappingCollection = (StorageMappingItemCollection)objectContext.MetadataWorkspace
                                                                        .GetItemCollection(DataSpace.CSSpace);
```

<span data-ttu-id="54810-127">После получения Сторажемаппингитемколлектион можно получить доступ к методам Женератевиевс и Компутемаппингхашвалуе.</span><span class="sxs-lookup"><span data-stu-id="54810-127">Once you have the StorageMappingItemCollection then you can get access to the GenerateViews and ComputeMappingHashValue methods.</span></span>

``` csharp
    public Dictionary\<EntitySetBase, DbMappingView> GenerateViews(IList<EdmSchemaError> errors)
    public string ComputeMappingHashValue()
```

<span data-ttu-id="54810-128">Первый метод создает словарь с записью для каждого представления в сопоставлении контейнеров.</span><span class="sxs-lookup"><span data-stu-id="54810-128">The first method creates a dictionary with an entry for each view in the container mapping.</span></span> <span data-ttu-id="54810-129">Второй метод выполняет вычисление хэш-значения для одного сопоставления контейнера и используется во время выполнения для проверки того, что модель не изменилась с момента предварительного создания представлений.</span><span class="sxs-lookup"><span data-stu-id="54810-129">The second method computes a hash value for the single container mapping and is used at runtime to validate that the model has not changed since the views were pre-generated.</span></span> <span data-ttu-id="54810-130">Переопределения двух методов предоставляются для сложных сценариев, включающих несколько сопоставлений контейнеров.</span><span class="sxs-lookup"><span data-stu-id="54810-130">Overrides of the two methods are provided for complex scenarios involving multiple container mappings.</span></span>

<span data-ttu-id="54810-131">Создавая представления, вы вызываете метод Женератевиевс, а затем записываете результирующие Ентитисетбасе и Дбмаппингвиев.</span><span class="sxs-lookup"><span data-stu-id="54810-131">When generating views you will call the GenerateViews method and then write out the resulting EntitySetBase and DbMappingView.</span></span> <span data-ttu-id="54810-132">Также необходимо сохранить хэш, созданный методом Компутемаппингхашвалуе.</span><span class="sxs-lookup"><span data-stu-id="54810-132">You will also need to store the hash generated by the ComputeMappingHashValue method.</span></span>

### <a name="loading-views"></a><span data-ttu-id="54810-133">Загрузка представлений</span><span class="sxs-lookup"><span data-stu-id="54810-133">Loading Views</span></span>

<span data-ttu-id="54810-134">Чтобы загрузить представления, созданные методом Женератевиевс, можно предоставить EF классу, наследуемому от абстрактного класса DbMappingViewCache.</span><span class="sxs-lookup"><span data-stu-id="54810-134">In order to load the views generated by the GenerateViews method, you can provide EF with a class that inherits from the DbMappingViewCache abstract class.</span></span> <span data-ttu-id="54810-135">DbMappingViewCache указывает два метода, которые необходимо реализовать:</span><span class="sxs-lookup"><span data-stu-id="54810-135">DbMappingViewCache specifies two methods that you must implement:</span></span>

``` csharp
    public abstract string MappingHashValue { get; }
    public abstract DbMappingView GetView(EntitySetBase extent);
```

<span data-ttu-id="54810-136">Свойство Маппингхашвалуе должно возвращать хэш, созданный методом Компутемаппингхашвалуе.</span><span class="sxs-lookup"><span data-stu-id="54810-136">The MappingHashValue property must return the hash generated by the ComputeMappingHashValue method.</span></span> <span data-ttu-id="54810-137">Когда EF собирается запросить представления, он сначала создает и сравнивает хэш-значение модели с хэшем, возвращенным этим свойством.</span><span class="sxs-lookup"><span data-stu-id="54810-137">When EF is going to ask for views it will first generate and compare the hash value of the model with the hash returned by this property.</span></span> <span data-ttu-id="54810-138">Если они не совпадают, EF вызовет исключение Ентитикоммандкомпилатионексцептион.</span><span class="sxs-lookup"><span data-stu-id="54810-138">If they do not match then EF will throw an EntityCommandCompilationException exception.</span></span>

<span data-ttu-id="54810-139">Метод noreturn принимает Ентитисетбасе, и необходимо вернуть Дбмаппингвиев, содержащий Ентитискл, который был создан для, который был связан с заданным Ентитисетбасе в словаре, созданном с помощью метода Женератевиевс.</span><span class="sxs-lookup"><span data-stu-id="54810-139">The GetView method will accept an EntitySetBase and you need to return a DbMappingVIew containing the EntitySql that was generated for that was associated with the given EntitySetBase in the dictionary generated by the GenerateViews method.</span></span> <span data-ttu-id="54810-140">Если EF запрашивает представление, которое у вас нет, то оператор onview должен вернуть значение null.</span><span class="sxs-lookup"><span data-stu-id="54810-140">If EF asks for a view that you do not have then GetView should return null.</span></span>

<span data-ttu-id="54810-141">Ниже приведено извлечение из DbMappingViewCache, созданного с помощью Power Tools, как описано выше. в нем мы видим один из способов хранения и извлечения необходимых Ентитискл.</span><span class="sxs-lookup"><span data-stu-id="54810-141">The following is an extract from the DbMappingViewCache that is generated with the Power Tools as described above, in it we see one way to store and retrieve the EntitySql required.</span></span>

``` csharp
    public override string MappingHashValue
    {
        get { return "a0b843f03dd29abee99789e190a6fb70ce8e93dc97945d437d9a58fb8e2afd2e"; }
    }

    public override DbMappingView GetView(EntitySetBase extent)
    {
        if (extent == null)
        {
            throw new ArgumentNullException("extent");
        }

        var extentName = extent.EntityContainer.Name + "." + extent.Name;

        if (extentName == "BlogContext.Blogs")
        {
            return GetView2();
        }

        if (extentName == "BlogContext.Posts")
        {
            return GetView3();
        }

        return null;
    }

    private static DbMappingView GetView2()
    {
        return new DbMappingView(@"
            SELECT VALUE -- Constructing Blogs
            [BlogApp.Models.Blog](T1.Blog_BlogId, T1.Blog_Test, T1.Blog_title, T1.Blog_Active, T1.Blog_SomeDecimal)
            FROM (
            SELECT
                T.BlogId AS Blog_BlogId,
                T.Test AS Blog_Test,
                T.title AS Blog_title,
                T.Active AS Blog_Active,
                T.SomeDecimal AS Blog_SomeDecimal,
                True AS _from0
            FROM CodeFirstDatabase.Blog AS T
            ) AS T1");
    }
```

<span data-ttu-id="54810-142">Чтобы элемент EF использовал ваш DbMappingViewCache, добавьте параметр Дбмаппингвиевкачетипеаттрибуте, указав контекст, для которого он был создан.</span><span class="sxs-lookup"><span data-stu-id="54810-142">To have EF use your DbMappingViewCache you add use the DbMappingViewCacheTypeAttribute, specifying the context that it was created for.</span></span> <span data-ttu-id="54810-143">В приведенном ниже коде мы связываем BlogContext с классом Мимаппингвиевкаче.</span><span class="sxs-lookup"><span data-stu-id="54810-143">In the code below we associate the BlogContext with the MyMappingViewCache class.</span></span>

``` csharp
    [assembly: DbMappingViewCacheType(typeof(BlogContext), typeof(MyMappingViewCache))]
```

<span data-ttu-id="54810-144">Для более сложных сценариев можно предоставить экземпляры кэша представления сопоставления, указав фабрику кэша представления сопоставления.</span><span class="sxs-lookup"><span data-stu-id="54810-144">For more complex scenarios, mapping view cache instances can be provided by specifying a mapping view cache factory.</span></span> <span data-ttu-id="54810-145">Это можно сделать, реализовав абстрактный класс System. Data. Entity. Infrastructure. Маппингвиевс. Дбмаппингвиевкачефактори.</span><span class="sxs-lookup"><span data-stu-id="54810-145">This can be done by implementing the abstract class System.Data.Entity.Infrastructure.MappingViews.DbMappingViewCacheFactory.</span></span> <span data-ttu-id="54810-146">Экземпляр используемой фабрики кэша представления сопоставления можно получить или задать с помощью Сторажемаппингитемколлектион. Маппингвиевкачефакторипроперти.</span><span class="sxs-lookup"><span data-stu-id="54810-146">The instance of the mapping view cache factory that is used can be retrieved or set using the StorageMappingItemCollection.MappingViewCacheFactoryproperty.</span></span>
