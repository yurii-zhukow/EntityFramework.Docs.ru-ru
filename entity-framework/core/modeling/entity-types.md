---
title: Типы сущностей — EF Core
description: Как настроить и сопоставлять типы сущностей с помощью Entity Framework Core
author: roji
ms.date: 10/06/2020
uid: core/modeling/entity-types
ms.openlocfilehash: 6c8029b64210dd9bc69fd53d9a3b4aa06bf519e0
ms.sourcegitcommit: 4798ab8d04c1fdbe6dd204d94d770fcbf309d09b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/11/2021
ms.locfileid: "103023995"
---
# <a name="entity-types"></a>Типы сущностей

Включение DbSet типа в контекст означает, что он включен в модель EF Core; как правило, тип сущности упоминается как *сущность*. EF Core может считывать и записывать экземпляры сущностей из базы данных, а также, если используется реляционная база данных, EF Core может создавать таблицы для сущностей с помощью миграций.

## <a name="including-types-in-the-model"></a>Включение типов в модель

По соглашению типы, предоставляемые в свойствах DbSet в контексте, включаются в модель в качестве сущностей. Также включаются типы сущностей, указанные в `OnModelCreating` методе, как и любые типы, которые обнаруживаются рекурсивным просмотром свойств навигации других обнаруженных типов сущностей.

В приведенном ниже примере кода включены все типы:

* `Blog` включен, так как он предоставляется в свойстве DbSet в контексте.
* `Post` включен, так как он обнаруживается через `Blog.Posts` свойство навигации.
* `AuditEntry` так как он указан в `OnModelCreating` .

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/EntityTypes.cs?name=EntityTypes&highlight=3,7,16)]

## <a name="excluding-types-from-the-model"></a>Исключение типов из модели

Если вы не хотите включать тип в модель, его можно исключить:

### <a name="data-annotations"></a>[Заметки к данным](#tab/data-annotations)

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/IgnoreType.cs?name=IgnoreType&highlight=1)]

### <a name="fluent-api"></a>[Текучий API](#tab/fluent-api)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/IgnoreType.cs?name=IgnoreType&highlight=3)]

***

### <a name="excluding-from-migrations"></a>Исключение из миграции

> [!NOTE]
> Возможность исключения таблиц из миграции была введена в EF Core 5,0.

Иногда бывает полезно иметь один и тот же тип сущности, сопоставленный в нескольких `DbContext` типах. Это особенно справедливо при использовании [ограниченных](https://www.martinfowler.com/bliki/BoundedContext.html)контекстов, для которых обычно используется отдельный `DbContext` тип для каждого ограниченного контекста.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/TableExcludeFromMigrations.cs?name=TableExcludeFromMigrations&highlight=4)]

При такой миграции конфигурации таблица не создается `AspNetUsers` , но `IdentityUser` по-прежнему включается в модель и может использоваться в обычном режиме.

Если необходимо приступить к управлению таблицей с помощью миграции, то следует создать новую миграцию, если `AspNetUsers` она не исключена. Следующая миграция теперь будет содержать все изменения, внесенные в таблицу.

## <a name="table-name"></a>Имя таблицы

По соглашению каждый тип сущности будет настроен для соотнесения с таблицей базы данных с тем же именем, что и свойство DbSet, предоставляющее сущность. Если DbSet для данной сущности не существует, используется имя класса.

Вы можете вручную настроить имя таблицы:

### <a name="data-annotations"></a>[Заметки к данным](#tab/data-annotations)

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/TableName.cs?Name=TableName&highlight=1)]

### <a name="fluent-api"></a>[Текучий API](#tab/fluent-api)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/TableName.cs?Name=TableName&highlight=3-4)]

***

## <a name="table-schema"></a>Схема таблицы

При использовании реляционной базы данных таблицы — это соглашение, созданное в схеме базы данных по умолчанию. Например, Microsoft SQL Server будет использовать `dbo` схему (SQLite не поддерживает схемы).

Можно настроить создание таблиц в определенной схеме следующим образом.

### <a name="data-annotations"></a>[Заметки к данным](#tab/data-annotations)

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/TableNameAndSchema.cs?name=TableNameAndSchema&highlight=1)]

### <a name="fluent-api"></a>[Текучий API](#tab/fluent-api)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/TableNameAndSchema.cs?name=TableNameAndSchema&highlight=3-4)]

***

Вместо того чтобы указывать схему для каждой таблицы, можно также определить схему по умолчанию на уровне модели с помощью API-интерфейса Fluent:

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/DefaultSchema.cs?name=DefaultSchema&highlight=3)]

Обратите внимание, что установка схемы по умолчанию также влияет на другие объекты базы данных, например последовательности.

## <a name="view-mapping"></a>Просмотр сопоставления

Типы сущностей можно сопоставлять с представлениями базы данных с помощью API Fluent.

> [!Note]
> EF предполагает, что представление, на которое имеется ссылка, уже существует в базе данных, оно не будет автоматически создаваться при миграции.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/ViewNameAndSchema.cs?name=ViewNameAndSchema&highlight=1)]

 Сопоставление с представлением приведет к удалению сопоставления таблицы по умолчанию, но начиная с EF 5,0 тип сущности также можно сопоставить с таблицей явным образом. В этом случае для запросов будет использоваться сопоставление запросов, а для обновлений будут использоваться сопоставления таблиц.

> [!TIP]
> Чтобы проверить типы сущностей, сопоставленные с представлениями с помощью поставщика в памяти, сопоставьте их с запросом через `ToInMemoryQuery` . Дополнительные сведения см. в разделе [готовый к запуску пример](https://github.com/dotnet/EntityFramework.Docs/tree/main/samples/core/Miscellaneous/Testing/ItemsWebApi/) с использованием этого метода.

## <a name="table-valued-function-mapping"></a>Сопоставление функций, возвращающих табличное значение

Можно сопоставлять тип сущности с возвращающей табличное значение функцией вместо таблицы в базе данных. Чтобы проиллюстрировать это, давайте определим другую сущность, которая представляет блог с несколькими записями. В этом примере сущность [не имеет смысла, но](xref:core/modeling/keyless-entity-types)она не должна быть.

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/EntityTypes.cs#BlogWithMultiplePostsEntity)]

Затем создайте в базе данных следующую функцию с табличным значением, которая возвращает только блоги с несколькими записями, а также число записей, связанных с каждым из этих блогов:

```sql
CREATE FUNCTION dbo.BlogsWithMultiplePosts()
RETURNS TABLE
AS
RETURN
(
    SELECT b.Url, COUNT(p.BlogId) AS PostCount
    FROM Blogs AS b
    JOIN Posts AS p ON b.BlogId = p.BlogId
    GROUP BY b.BlogId, b.Url
    HAVING COUNT(p.BlogId) > 1
)
```

Теперь сущность `BlogWithMultiplePost` может быть сопоставлена с этой функцией следующим образом:

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/EntityTypes.cs#QueryableFunctionConfigurationToFunction)]

> [!NOTE]
> Чтобы связать сущность с возвращающей табличное значение функцией, функция должна быть без параметров.

Согласно соглашению, свойства сущности будут сопоставляться с соответствующими столбцами, возвращаемыми функцией. Если столбцы, возвращаемые функцией "функция", имеют разные имена, чем свойство сущности, то их можно настроить с помощью `HasColumnName` метода, как и при сопоставлении с обычной таблицей.

Если тип сущности сопоставляется с функцией, возвращающей табличное значение, запрос:

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Program.cs#ToFunctionQuery)]

Преобразуется в следующий запрос SQL:

```sql
SELECT [b].[Url], [b].[PostCount]
FROM [dbo].[BlogsWithMultiplePosts]() AS [b]
WHERE [b].[PostCount] > 3
```

## <a name="table-comments"></a>Комментарии к таблице

Можно задать произвольный текстовый комментарий, заданный для таблицы базы данных, что позволит документировать схему в базе данных:

### <a name="data-annotations"></a>[Заметки к данным](#tab/data-annotations)

> [!NOTE]
> Установка комментариев с помощью заметок к данным была представлена в EF Core 5,0.

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/TableComment.cs?name=TableComment&highlight=1)]

### <a name="fluent-api"></a>[Текучий API](#tab/fluent-api)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/TableComment.cs?name=TableComment&highlight=4)]

***
