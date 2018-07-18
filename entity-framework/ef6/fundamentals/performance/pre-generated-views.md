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
# <a name="pre-generated-mapping-views"></a>Сопоставление заранее созданные представления
Прежде чем платформа Entity Framework можно выполнить запрос или сохранить изменения в источнике данных, ей необходимо создать набор сопоставления представлений для доступа к базе данных. Эти представления сопоставления представляют собой набор оператор Entity SQL, отображают состояние базы данных отвлеченно и являются частью метаданных, кэшируемых для каждого домена приложения. При создании нескольких экземпляров того же контекста, в том же домене приложения, они будут повторно использовать сопоставление представления из кэшированных метаданных, а не формируются заново. Сопоставление создание представлений составляет значительную долю общей стоимости выполнения первого запроса, Entity Framework позволяет заранее создавать представления сопоставления и включать их в скомпилированный проект. Дополнительные сведения см. в разделе [вопросы производительности (Entity Framework)](~/ef6/fundamentals/performance/perf-whitepaper.md).

## <a name="generating-mapping-views-with-the-ef-power-tools"></a>Создание сопоставления представлений с помощью EF Power Tools

Чтобы заранее создать представления проще всего использовать [EF Power Tools](http://visualstudiogallery.msdn.microsoft.com/72a60b14-1581-4b9b-89f2-846072eff19d). Если у вас установлены инструменты Power имеется пункт меню, чтобы создать представления, как показано ниже.

-   Для **Code First** моделей, щелкните правой кнопкой мыши на файл кода, который содержит ваш класс DbContext.
-   Для **конструктор EF** моделей, щелкните правой кнопкой мыши на файле EDMX.

![generateViews](~/ef6/media/generateviews.png)

После завершения процесса вы получите следующую созданный класс

![generatedViews](~/ef6/media/generatedviews.png)

Теперь при запуске приложения EF будет использовать этот класс для загрузки представления при необходимости. Если изменения модели и вы не повторно создают этот класс EF вызовет исключение.

## <a name="generating-mapping-views-from-code---ef6-onwards"></a>Создание представления сопоставления из кода - EF6 и выше

Еще одним способом создавать представления является использование API-интерфейсов с EF. При использовании этого метода, вы можете свободно для сериализации в представлениях, однако вам удобно, но необходимо также загрузить представления самостоятельно.

> [!NOTE]
> **EF6 и выше только** -API, как показано в этом разделе были представлены в Entity Framework 6. При использовании более ранней версии эти сведения не применяется.

### <a name="generating-views"></a>Создание представлений

API-интерфейсы для создания представлений, находятся в классе System.Data.Entity.Core.Mapping.StorageMappingItemCollection. StorageMappingCollection для контекста, можно получить с помощью MetadataWorkspace ObjectContext. Если вы используете более новый API DbContext, а затем можно получить доступ к этому с помощью IObjectContextAdapter, например ниже, в этом коде у нас есть экземпляр вашем производном DbContext называется dbContext:

``` csharp
    var objectContext = ((IObjectContextAdapter) dbContext).ObjectContext;
    var  mappingCollection = (StorageMappingItemCollection)objectContext.MetadataWorkspace
                                                                        .GetItemCollection(DataSpace.CSSpace);
```

При наличии StorageMappingItemCollection можно получить доступ к методам GenerateViews и ComputeMappingHashValue.

``` csharp
    public Dictionary\<EntitySetBase, DbMappingView> GenerateViews(IList<EdmSchemaError> errors)
    public string ComputeMappingHashValue()
```

Первый метод создает словарь с записью для каждого представления в сопоставлении контейнеров. Второй метод вычисляет значение хэша для сопоставления одного контейнера и используется во время выполнения для проверки что модель не был изменен, так как представления были предварительно созданных. Для сложных сценариев, включающих несколько сопоставлений контейнера предоставляются переопределения двух методов.

При создании представления будет вызывать метод GenerateViews и затем записать итоговый EntitySetBase и DbMappingView. Также необходимо будет хранить хэша, созданного с помощью метода ComputeMappingHashValue.

### <a name="loading-views"></a>Загрузка представления

Чтобы загрузить представления, сформированные с помощью метода GenerateViews, вы можете предоставить EF класс, наследующий от абстрактного класса DbMappingViewCache. DbMappingViewCache определяет два метода, которые необходимо реализовать:

``` csharp
    public abstract string MappingHashValue { get; }
    public abstract DbMappingView GetView(EntitySetBase extent);
```

Свойство MappingHashValue должно возвращать хэша, созданного с помощью метода ComputeMappingHashValue. Когда EF, которая будет служит для представления сначала создаст и сравнивать хэш-значение модели с хэшем, возвращаемый этим свойством. Если они не совпадают EF вызовет исключение EntityCommandCompilationException.

Метод GetView примет EntitySetBase и требуется возвращать что DbMappingVIew, содержащий язык EntitySql, который был создан для этого был связан с заданной EntitySetBase в словаре, созданной методом GenerateViews. Если EF запрашивает представление, что у вас нет затем GetView должен возвращать значение null.

Ниже приведен фрагмент DbMappingViewCache, который создается с помощью Power Tools, как описано выше, в нем мы видим, один из способов для хранения и извлечения язык EntitySql, которые требуется.

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

Чтобы использовать EF вашей DbMappingViewCache, добавлении используйте DbMappingViewCacheTypeAttribute, указав контекст, в котором он был создан для. В приведенном ниже коде мы связываем с классом MyMappingViewCache BlogContext.

``` csharp
    [assembly: DbMappingViewCacheType(typeof(BlogContext), typeof(MyMappingViewCache))]
```

Для более сложных сценариев можно указать экземпляров кэша представление сопоставления, задайте фабрику кэша представление сопоставления. Это можно сделать путем реализации абстрактного класса System.Data.Entity.Infrastructure.MappingViews.DbMappingViewCacheFactory. Экземпляр фабрики кэша представление сопоставления, используемый можно извлечь или задать с помощью StorageMappingItemCollection.MappingViewCacheFactoryproperty.
