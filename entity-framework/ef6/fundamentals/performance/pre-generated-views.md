---
title: Предварительно созданные представления сопоставления — EF6
description: Предварительно созданные представления сопоставления в Entity Framework 6
author: divega
ms.date: 10/23/2016
uid: ef6/fundamentals/performance/pre-generated-views
ms.openlocfilehash: b69c1eea1e219d76a8de1b2cf3b88de2cf74c8f0
ms.sourcegitcommit: abda0872f86eefeca191a9a11bfca976bc14468b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/14/2020
ms.locfileid: "90073556"
---
# <a name="pre-generated-mapping-views"></a>Предварительно созданные представления сопоставления
Прежде чем Entity Framework сможет выполнить запрос или сохранить изменения в источнике данных, он должен создать набор представлений сопоставления для доступа к базе данных. Эти представления сопоставления представляют собой набор Entity SQL инструкций, которые представляют базу данных абстрактным способом и являются частью метаданных, которые кэшируются для каждого домена приложения. При создании нескольких экземпляров одного и того же контекста в одном домене приложения они будут повторно использовать представления сопоставления из кэшированных метаданных, а не воссоздавать их. Поскольку создание представления сопоставления является важной частью общей стоимости выполнения первого запроса, Entity Framework позволяет предварительно создавать представления сопоставления и включать их в скомпилированный проект.Дополнительные сведения см. в разделе  [вопросы производительности (Entity Framework)](xref:ef6/fundamentals/performance/perf-whitepaper).

## <a name="generating-mapping-views-with-the-ef-power-tools-community-edition"></a>Создание представлений сопоставления с помощью EF Power Tools Community Edition

Самый простой способ предварительно создать представления — использовать [EF Power Tools Community Edition](https://marketplace.visualstudio.com/items?itemName=ErikEJ.EntityFramework6PowerToolsCommunityEdition). После установки Power Tools появится пункт меню для создания представлений, как показано ниже.

-   Для **Code First** моделей щелкните правой кнопкой мыши файл кода, содержащий класс DbContext.
-   Для моделей **конструктора EF** щелкните правой кнопкой мыши EDMX-файл.

![Создание представлений](~/ef6/media/generateviews.png)

По завершении процесса будет создан класс, аналогичный приведенному ниже.

![созданные представления](~/ef6/media/generatedviews.png)

Теперь при запуске приложения EF будет использовать этот класс для загрузки представлений по мере необходимости. Если модель изменяется и вы не создаете этот класс повторно, EF выдаст исключение.

## <a name="generating-mapping-views-from-code---ef6-onwards"></a>Создание представлений сопоставления из Code-EF6/назад

Другим способом создания представлений является использование API, предоставляемых EF. При использовании этого метода у вас есть возможность сериализовать представления, но вам необходимо также загрузить представления самостоятельно.

> [!NOTE]
> **EF6 только** для чтения. API-интерфейсы, показанные в этом разделе, появились в Entity Framework 6. Если вы используете более раннюю версию, эти сведения не применяются.

### <a name="generating-views"></a>Создание представлений

API-интерфейсы для создания представлений находятся в классе System. Data. Entity. Core. mapping. Сторажемаппингитемколлектион. Вы можете получить Сторажемаппингколлектион для контекста с помощью области MetadataWorkspace контекста ObjectContext. Если вы используете новый API DbContext, вы можете получить к нему доступ с помощью Иобжектконтекстадаптер, как показано ниже, в этом коде имеется экземпляр производного DbContext с именем dbContext:

``` csharp
    var objectContext = ((IObjectContextAdapter) dbContext).ObjectContext;
    var  mappingCollection = (StorageMappingItemCollection)objectContext.MetadataWorkspace
                                                                        .GetItemCollection(DataSpace.CSSpace);
```

После получения Сторажемаппингитемколлектион можно получить доступ к методам Женератевиевс и Компутемаппингхашвалуе.

``` csharp
    public Dictionary\<EntitySetBase, DbMappingView> GenerateViews(IList<EdmSchemaError> errors)
    public string ComputeMappingHashValue()
```

Первый метод создает словарь с записью для каждого представления в сопоставлении контейнеров. Второй метод выполняет вычисление хэш-значения для одного сопоставления контейнера и используется во время выполнения для проверки того, что модель не изменилась с момента предварительного создания представлений. Переопределения двух методов предоставляются для сложных сценариев, включающих несколько сопоставлений контейнеров.

Создавая представления, вы вызываете метод Женератевиевс, а затем записываете результирующие Ентитисетбасе и Дбмаппингвиев. Также необходимо сохранить хэш, созданный методом Компутемаппингхашвалуе.

### <a name="loading-views"></a>Загрузка представлений

Чтобы загрузить представления, созданные методом Женератевиевс, можно предоставить EF классу, наследуемому от абстрактного класса DbMappingViewCache. DbMappingViewCache указывает два метода, которые необходимо реализовать:

``` csharp
    public abstract string MappingHashValue { get; }
    public abstract DbMappingView GetView(EntitySetBase extent);
```

Свойство Маппингхашвалуе должно возвращать хэш, созданный методом Компутемаппингхашвалуе. Когда EF собирается запросить представления, он сначала создает и сравнивает хэш-значение модели с хэшем, возвращенным этим свойством. Если они не совпадают, EF вызовет исключение Ентитикоммандкомпилатионексцептион.

Метод noreturn принимает Ентитисетбасе, и необходимо вернуть Дбмаппингвиев, содержащий Ентитискл, который был создан для, который был связан с заданным Ентитисетбасе в словаре, созданном с помощью метода Женератевиевс. Если EF запрашивает представление, которое у вас нет, то оператор onview должен вернуть значение null.

Ниже приведено извлечение из DbMappingViewCache, созданного с помощью Power Tools, как описано выше. в нем мы видим один из способов хранения и извлечения необходимых Ентитискл.

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

Чтобы элемент EF использовал ваш DbMappingViewCache, добавьте параметр Дбмаппингвиевкачетипеаттрибуте, указав контекст, для которого он был создан. В приведенном ниже коде мы связываем BlogContext с классом Мимаппингвиевкаче.

``` csharp
    [assembly: DbMappingViewCacheType(typeof(BlogContext), typeof(MyMappingViewCache))]
```

Для более сложных сценариев можно предоставить экземпляры кэша представления сопоставления, указав фабрику кэша представления сопоставления. Это можно сделать, реализовав абстрактный класс System. Data. Entity. Infrastructure. Маппингвиевс. Дбмаппингвиевкачефактори. Экземпляр используемой фабрики кэша представления сопоставления можно получить или задать с помощью Сторажемаппингитемколлектион. Маппингвиевкачефакторипроперти.
