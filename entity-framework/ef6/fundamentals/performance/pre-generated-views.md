---
title: Сопоставление заранее созданные представления — EF6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: 917ba9c8-6ddf-4631-ab8c-c4fb378c2fcd
caps.latest.revision: 3
ms.openlocfilehash: 9e74176d02afc424118219eec8e016843333cbb8
ms.sourcegitcommit: 390f3a37bc55105ed7cc5b0e0925b7f9c9e80ba6
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/09/2018
ms.locfileid: "39122590"
---
# <a name="pre-generated-mapping-views"></a><span data-ttu-id="6bf0c-102">Сопоставление заранее созданные представления</span><span class="sxs-lookup"><span data-stu-id="6bf0c-102">Pre-generated mapping views</span></span>
<span data-ttu-id="6bf0c-103">Прежде чем платформа Entity Framework можно выполнить запрос или сохранить изменения в источнике данных, ей необходимо создать набор сопоставления представлений для доступа к базе данных.</span><span class="sxs-lookup"><span data-stu-id="6bf0c-103">Before the Entity Framework can execute a query or save changes to the data source, it must generate a set of mapping views to access the database.</span></span> <span data-ttu-id="6bf0c-104">Эти представления сопоставления представляют собой набор оператор Entity SQL, отображают состояние базы данных отвлеченно и являются частью метаданных, кэшируемых для каждого домена приложения.</span><span class="sxs-lookup"><span data-stu-id="6bf0c-104">These mapping views are a set of Entity SQL statement that represent the database in an abstract way, and are part of the metadata which is cached per application domain.</span></span> <span data-ttu-id="6bf0c-105">При создании нескольких экземпляров того же контекста, в том же домене приложения, они будут повторно использовать сопоставление представления из кэшированных метаданных, а не формируются заново.</span><span class="sxs-lookup"><span data-stu-id="6bf0c-105">If you create multiple instances of the same context in the same application domain, they will reuse mapping views from the cached metadata rather than regenerating them.</span></span> <span data-ttu-id="6bf0c-106">Сопоставление создание представлений составляет значительную долю общей стоимости выполнения первого запроса, Entity Framework позволяет заранее создавать представления сопоставления и включать их в скомпилированный проект.</span><span class="sxs-lookup"><span data-stu-id="6bf0c-106">Because mapping view generation is a significant part of the overall cost of executing the first query, the Entity Framework enables you to pre-generate mapping views and include them in the compiled project.</span></span> <span data-ttu-id="6bf0c-107">Дополнительные сведения см. в разделе [вопросы производительности (Entity Framework)](~/ef6/fundamentals/performance/perf-whitepaper.md).</span><span class="sxs-lookup"><span data-stu-id="6bf0c-107">For more information, see  [Performance Considerations (Entity Framework)](~/ef6/fundamentals/performance/perf-whitepaper.md).</span></span>

## <a name="generating-mapping-views-with-the-ef-power-tools"></a><span data-ttu-id="6bf0c-108">Создание сопоставления представлений с помощью EF Power Tools</span><span class="sxs-lookup"><span data-stu-id="6bf0c-108">Generating Mapping Views with the EF Power Tools</span></span>

<span data-ttu-id="6bf0c-109">Чтобы заранее создать представления проще всего использовать [EF Power Tools](http://visualstudiogallery.msdn.microsoft.com/72a60b14-1581-4b9b-89f2-846072eff19d).</span><span class="sxs-lookup"><span data-stu-id="6bf0c-109">The easiest way to pre-generate views is to use the [EF Power Tools](http://visualstudiogallery.msdn.microsoft.com/72a60b14-1581-4b9b-89f2-846072eff19d).</span></span> <span data-ttu-id="6bf0c-110">Если у вас установлены инструменты Power имеется пункт меню, чтобы создать представления, как показано ниже.</span><span class="sxs-lookup"><span data-stu-id="6bf0c-110">Once you have the Power Tools installed you will have a menu option to Generate Views, as below.</span></span>

-   <span data-ttu-id="6bf0c-111">Для **Code First** моделей, щелкните правой кнопкой мыши на файл кода, который содержит ваш класс DbContext.</span><span class="sxs-lookup"><span data-stu-id="6bf0c-111">For **Code First** models right-click on the code file that contains your DbContext class.</span></span>
-   <span data-ttu-id="6bf0c-112">Для **конструктор EF** моделей, щелкните правой кнопкой мыши на файле EDMX.</span><span class="sxs-lookup"><span data-stu-id="6bf0c-112">For **EF Designer** models right-click on your EDMX file.</span></span>

![generateViews](~/ef6/media/generateviews.png)

<span data-ttu-id="6bf0c-114">После завершения процесса вы получите следующую созданный класс</span><span class="sxs-lookup"><span data-stu-id="6bf0c-114">Once the process is finished you will have a class similar to the following generated</span></span>

![generatedViews](~/ef6/media/generatedviews.png)

<span data-ttu-id="6bf0c-116">Теперь при запуске приложения EF будет использовать этот класс для загрузки представления при необходимости.</span><span class="sxs-lookup"><span data-stu-id="6bf0c-116">Now when you run your application EF will use this class to load views as required.</span></span> <span data-ttu-id="6bf0c-117">Если изменения модели и вы не повторно создают этот класс EF вызовет исключение.</span><span class="sxs-lookup"><span data-stu-id="6bf0c-117">If your model changes and you do not re-generate this class then EF will throw an exception.</span></span>

## <a name="generating-mapping-views-from-code---ef6-onwards"></a><span data-ttu-id="6bf0c-118">Создание представления сопоставления из кода - EF6 и выше</span><span class="sxs-lookup"><span data-stu-id="6bf0c-118">Generating Mapping Views from Code - EF6 Onwards</span></span>

<span data-ttu-id="6bf0c-119">Еще одним способом создавать представления является использование API-интерфейсов с EF.</span><span class="sxs-lookup"><span data-stu-id="6bf0c-119">The other way to generate views is to use the APIs that EF provides.</span></span> <span data-ttu-id="6bf0c-120">При использовании этого метода, вы можете свободно для сериализации в представлениях, однако вам удобно, но необходимо также загрузить представления самостоятельно.</span><span class="sxs-lookup"><span data-stu-id="6bf0c-120">When using this method you have the freedom to serialize the views however you like, but you also need to load the views yourself.</span></span>

> [!NOTE]
> <span data-ttu-id="6bf0c-121">**EF6 и выше только** -API, как показано в этом разделе были представлены в Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="6bf0c-121">**EF6 Onwards Only** - The APIs shown in this section were introduced in Entity Framework 6.</span></span> <span data-ttu-id="6bf0c-122">При использовании более ранней версии эти сведения не применяется.</span><span class="sxs-lookup"><span data-stu-id="6bf0c-122">If you are using an earlier version this information does not apply.</span></span>

### <a name="generating-views"></a><span data-ttu-id="6bf0c-123">Создание представлений</span><span class="sxs-lookup"><span data-stu-id="6bf0c-123">Generating Views</span></span>

<span data-ttu-id="6bf0c-124">API-интерфейсы для создания представлений, находятся в классе System.Data.Entity.Core.Mapping.StorageMappingItemCollection.</span><span class="sxs-lookup"><span data-stu-id="6bf0c-124">The APIs to generate views are on the System.Data.Entity.Core.Mapping.StorageMappingItemCollection class.</span></span> <span data-ttu-id="6bf0c-125">StorageMappingCollection для контекста, можно получить с помощью MetadataWorkspace ObjectContext.</span><span class="sxs-lookup"><span data-stu-id="6bf0c-125">You can retrieve a StorageMappingCollection for a Context by using the MetadataWorkspace of an ObjectContext.</span></span> <span data-ttu-id="6bf0c-126">Если вы используете более новый API DbContext, а затем можно получить доступ к этому с помощью IObjectContextAdapter, например ниже, в этом коде у нас есть экземпляр вашем производном DbContext называется dbContext:</span><span class="sxs-lookup"><span data-stu-id="6bf0c-126">If you are using the newer DbContext API then you can access this by using the IObjectContextAdapter like below, in this code we have an instance of your derived DbContext called dbContext:</span></span>

``` csharp
    var objectContext = ((IObjectContextAdapter) dbContext).ObjectContext;
    var  mappingCollection = (StorageMappingItemCollection)objectContext.MetadataWorkspace
                                                                        .GetItemCollection(DataSpace.CSSpace);
```

<span data-ttu-id="6bf0c-127">При наличии StorageMappingItemCollection можно получить доступ к методам GenerateViews и ComputeMappingHashValue.</span><span class="sxs-lookup"><span data-stu-id="6bf0c-127">Once you have the StorageMappingItemCollection then you can get access to the GenerateViews and ComputeMappingHashValue methods.</span></span>

``` csharp
    public Dictionary\<EntitySetBase, DbMappingView> GenerateViews(IList<EdmSchemaError> errors)
    public string ComputeMappingHashValue()
```

<span data-ttu-id="6bf0c-128">Первый метод создает словарь с записью для каждого представления в сопоставлении контейнеров.</span><span class="sxs-lookup"><span data-stu-id="6bf0c-128">The first method creates a dictionary with an entry for each view in the container mapping.</span></span> <span data-ttu-id="6bf0c-129">Второй метод вычисляет значение хэша для сопоставления одного контейнера и используется во время выполнения для проверки что модель не был изменен, так как представления были предварительно созданных.</span><span class="sxs-lookup"><span data-stu-id="6bf0c-129">The second method computes a hash value for the single container mapping and is used at runtime to validate that the model has not changed since the views were pre-generated.</span></span> <span data-ttu-id="6bf0c-130">Для сложных сценариев, включающих несколько сопоставлений контейнера предоставляются переопределения двух методов.</span><span class="sxs-lookup"><span data-stu-id="6bf0c-130">Overrides of the two methods are provided for complex scenarios involving multiple container mappings.</span></span>

<span data-ttu-id="6bf0c-131">При создании представления будет вызывать метод GenerateViews и затем записать итоговый EntitySetBase и DbMappingView.</span><span class="sxs-lookup"><span data-stu-id="6bf0c-131">When generating views you will call the GenerateViews method and then write out the resulting EntitySetBase and DbMappingView.</span></span> <span data-ttu-id="6bf0c-132">Также необходимо будет хранить хэша, созданного с помощью метода ComputeMappingHashValue.</span><span class="sxs-lookup"><span data-stu-id="6bf0c-132">You will also need to store the hash generated by the ComputeMappingHashValue method.</span></span>

### <a name="loading-views"></a><span data-ttu-id="6bf0c-133">Загрузка представления</span><span class="sxs-lookup"><span data-stu-id="6bf0c-133">Loading Views</span></span>

<span data-ttu-id="6bf0c-134">Чтобы загрузить представления, сформированные с помощью метода GenerateViews, вы можете предоставить EF класс, наследующий от абстрактного класса DbMappingViewCache.</span><span class="sxs-lookup"><span data-stu-id="6bf0c-134">In order to load the views generated by the GenerateViews method, you can provide EF with a class that inherits from the DbMappingViewCache abstract class.</span></span> <span data-ttu-id="6bf0c-135">DbMappingViewCache определяет два метода, которые необходимо реализовать:</span><span class="sxs-lookup"><span data-stu-id="6bf0c-135">DbMappingViewCache specifies two methods that you must implement:</span></span>

``` csharp
    public abstract string MappingHashValue { get; }
    public abstract DbMappingView GetView(EntitySetBase extent);
```

<span data-ttu-id="6bf0c-136">Свойство MappingHashValue должно возвращать хэша, созданного с помощью метода ComputeMappingHashValue.</span><span class="sxs-lookup"><span data-stu-id="6bf0c-136">The MappingHashValue property must return the hash generated by the ComputeMappingHashValue method.</span></span> <span data-ttu-id="6bf0c-137">Когда EF, которая будет служит для представления сначала создаст и сравнивать хэш-значение модели с хэшем, возвращаемый этим свойством.</span><span class="sxs-lookup"><span data-stu-id="6bf0c-137">When EF is going to ask for views it will first generate and compare the hash value of the model with the hash returned by this property.</span></span> <span data-ttu-id="6bf0c-138">Если они не совпадают EF вызовет исключение EntityCommandCompilationException.</span><span class="sxs-lookup"><span data-stu-id="6bf0c-138">If they do not match then EF will throw an EntityCommandCompilationException exception.</span></span>

<span data-ttu-id="6bf0c-139">Метод GetView примет EntitySetBase и требуется возвращать что DbMappingVIew, содержащий язык EntitySql, который был создан для этого был связан с заданной EntitySetBase в словаре, созданной методом GenerateViews.</span><span class="sxs-lookup"><span data-stu-id="6bf0c-139">The GetView method will accept an EntitySetBase and you need to return a DbMappingVIew containing the EntitySql that was generated for that was associated with the given EntitySetBase in the dictionary generated by the GenerateViews method.</span></span> <span data-ttu-id="6bf0c-140">Если EF запрашивает представление, что у вас нет затем GetView должен возвращать значение null.</span><span class="sxs-lookup"><span data-stu-id="6bf0c-140">If EF asks for a view that you do not have then GetView should return null.</span></span>

<span data-ttu-id="6bf0c-141">Ниже приведен фрагмент DbMappingViewCache, который создается с помощью Power Tools, как описано выше, в нем мы видим, один из способов для хранения и извлечения язык EntitySql, которые требуется.</span><span class="sxs-lookup"><span data-stu-id="6bf0c-141">The following is an extract from the DbMappingViewCache that is generated with the Power Tools as described above, in it we see one way to store and retrieve the EntitySql required.</span></span>

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

<span data-ttu-id="6bf0c-142">Чтобы использовать EF вашей DbMappingViewCache, добавлении используйте DbMappingViewCacheTypeAttribute, указав контекст, в котором он был создан для.</span><span class="sxs-lookup"><span data-stu-id="6bf0c-142">To have EF use your DbMappingViewCache you add use the DbMappingViewCacheTypeAttribute, specifying the context that it was created for.</span></span> <span data-ttu-id="6bf0c-143">В приведенном ниже коде мы связываем с классом MyMappingViewCache BlogContext.</span><span class="sxs-lookup"><span data-stu-id="6bf0c-143">In the code below we associate the BlogContext with the MyMappingViewCache class.</span></span>

``` csharp
    [assembly: DbMappingViewCacheType(typeof(BlogContext), typeof(MyMappingViewCache))]
```

<span data-ttu-id="6bf0c-144">Для более сложных сценариев можно указать экземпляров кэша представление сопоставления, задайте фабрику кэша представление сопоставления.</span><span class="sxs-lookup"><span data-stu-id="6bf0c-144">For more complex scenarios, mapping view cache instances can be provided by specifying a mapping view cache factory.</span></span> <span data-ttu-id="6bf0c-145">Это можно сделать путем реализации абстрактного класса System.Data.Entity.Infrastructure.MappingViews.DbMappingViewCacheFactory.</span><span class="sxs-lookup"><span data-stu-id="6bf0c-145">This can be done by implementing the abstract class System.Data.Entity.Infrastructure.MappingViews.DbMappingViewCacheFactory.</span></span> <span data-ttu-id="6bf0c-146">Экземпляр фабрики кэша представление сопоставления, используемый можно извлечь или задать с помощью StorageMappingItemCollection.MappingViewCacheFactoryproperty.</span><span class="sxs-lookup"><span data-stu-id="6bf0c-146">The instance of the mapping view cache factory that is used can be retrieved or set using the StorageMappingItemCollection.MappingViewCacheFactoryproperty.</span></span>
