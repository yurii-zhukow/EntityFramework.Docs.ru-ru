---
title: "Выполняется загрузка связанных данных — основной EF"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: f9fb64e2-6699-4d70-a773-592918c04c19
ms.technology: entity-framework-core
uid: core/querying/related-data
ms.openlocfilehash: cd26bd2e6f85083f73d97b1356d0ba38f53e0b8f
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/27/2017
---
# <a name="loading-related-data"></a>Загрузка связанных данных

Entity Framework Core позволяет использовать свойства навигации в модели для загрузки связанных сущностей. Существует три общих шаблонов O/надежный обмен Сообщениями, используемые для загрузки связанных данных.
* **Безотложная загрузка** означает, что данные загружены из базы данных как часть исходного запроса.
* **Явная загрузка** означает, что связанные данные явно загружен из базы данных в дальнейшем.
* **Отложенная загрузка** означает, что связанные прозрачно загрузки данных из базы данных при доступе к свойству навигации. Отложенная загрузка не возможно при EF Core.

> [!TIP]  
> Можно просмотреть в этой статье [пример](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) на GitHub.

## <a name="eager-loading"></a>Активная загрузка

Можно использовать `Include` метод, чтобы задать связанные данные, которые будут включены в результаты запроса. В следующем примере, блоги, которые возвращаются в результатах будут иметь их `Posts` свойство заполняется связанных сообщений.

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#SingleInclude)]

> [!TIP]  
> Entity Framework Core будет свойства навигации автоматически исправить вверх с другими сущностями, которые были ранее загружены в экземпляр контекста. Поэтому даже если данные для свойства навигации не включить явным образом, свойство по-прежнему могут быть заполнены, если некоторые или все связанные сущности были ранее загружены.


Связанные данные из нескольких связей может включать в одном запросе.

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#MultipleIncludes)]

### <a name="including-multiple-levels"></a>Включая несколько уровней

Можно выполнить детализацию до связи для включения нескольких уровней связанных данных с помощью `ThenInclude` метод. В следующем примере загружаются все блоги, их связанных сообщений и автор каждой отправки.

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#SingleThenInclude)]

Можно соединить в цепочку несколько вызовов `ThenInclude` чтобы продолжить, включая дополнительные уровни взаимосвязанных данных.

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#MultipleThenIncludes)]

Можно объединить все это для включения связанных данных из нескольких уровней и нескольких корней в одном запросе.

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#IncludeTree)]

Может потребоваться включить несколько связанных сущностей для одной сущности, включенного. Например, при запросе `Blog`s, включают `Posts` и затем необходимо включить оба `Author` и `Tags` из `Posts`. Чтобы сделать это, необходимо указать, имеют путь, начиная с корневого каталога. Например, `Blog -> Posts -> Author` или `Blog -> Posts -> Tags`. Это значит, что вы получите избыточных соединений, в большинстве случаев будет консолидировать EF соединений при создании кода SQL.

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#MultipleLeafIncludes)]

### <a name="ignored-includes"></a>Учитывается включает

Если изменить запрос, чтобы он больше не возвращает экземпляры типа сущности, запрос начинающийся с, операторы include игнорируются.

В следующем примере операторы include основаны на `Blog`, а затем `Select` оператор используется для изменения запроса для возврата анонимного типа. В этом случае операторы include не действуют.

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#IgnoredInclude)]

По умолчанию основные EF регистрируют предупреждения при включении операторы игнорируются. В разделе [входа](../miscellaneous/logging.md) Дополнительные сведения о просмотре выходных данных ведения журнала. Можно изменить поведение при оператор include игнорируется выдать или не выполняют никаких действий. Это делается при настройке параметров для текущего контекста — обычно в `DbContext.OnConfiguring`, или в `Startup.cs` при использовании ASP.NET Core.

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/ThrowOnIgnoredInclude/BloggingContext.cs#OnConfiguring)]

## <a name="explicit-loading"></a>Явная загрузка

> [!NOTE]  
> Эта функция появилась в версии 1.1 EF Core.

Можно явно загрузить через свойство навигации `DbContext.Entry(...)` API.

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#Eager)]

Можно также явно загрузить свойство навигации путем выполнения отдельного запроса, который возвращает связанные сущности. Если включено отслеживание изменений, то при загрузке сущности, EF Core будет автоматически задать свойства навигации объектов загруженных для ссылки на все сущности, которые уже загружены и задать свойства навигации для ссылки на сущности уже загружен загруженных сущность.

### <a name="querying-related-entities"></a>Запрос связанных сущностей

Можно также получить запрос LINQ, который представляет содержимое свойства навигации.

Это дает возможность выполнения действий, таких как запуск статистических операторов для связанных сущностей без их загрузки в память.

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#NavQueryAggregate)]

Можно также фильтровать связанные сущности загружаются в память.

[!code-csharp[Main](../../../samples/core/Querying/Querying/RelatedData/Sample.cs#NavQueryFiltered)]

## <a name="lazy-loading"></a>«Неспешная» загрузка

Отложенная загрузка ядром EF еще не поддерживается. Можно просмотреть [отложенную загрузку элемента в невыполненной работой](https://github.com/aspnet/EntityFramework/issues/3797) для отслеживания этой функции.

## <a name="related-data-and-serialization"></a>Связанные данные и сериализации

Поскольку свойства навигации автоматически исправить вверх будет EF Core, можно получить с циклами в граф объекта. Например загрузка блог и он не связан приведет к записи в блоге объект, который ссылается на коллекцию записей. Каждый из этих записей будет ссылкой на блога.

Некоторые платформы сериализации не допускают такие циклы. Например Json.NET вызовет следующее исключение, если цикл встречено.

> Newtonsoft.Json.JsonSerializationException: Self ссылается обнаружен для свойства «Блог» с типом «MyApplication.Models.Blog» цикл.

При использовании ASP.NET Core можно настроить Json.NET игнорировать циклов, найденные в графе объекта. Это можно сделать в `ConfigureServices(...)` метод в `Startup.cs`.

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
