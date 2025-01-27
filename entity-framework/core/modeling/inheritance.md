---
title: Наследование — EF Core
description: Настройка наследования типов сущностей с помощью Entity Framework Core
author: AndriySvyryd
ms.date: 10/01/2020
uid: core/modeling/inheritance
ms.openlocfilehash: 11bd653a53767aa732790b1222da1beff8ad26a9
ms.sourcegitcommit: 4860d036ea0fb392c28799907bcc924c987d2d7b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/17/2020
ms.locfileid: "97635761"
---
# <a name="inheritance"></a>Наследование

EF может сопоставлять иерархию типов .NET с базой данных. Это позволяет создавать сущности .NET в коде обычным образом, используя базовые и производные типы и позволяя EF легко создавать соответствующую схему базы данных, выдавать запросы и т. д. Фактические сведения о сопоставлении иерархии типов являются зависимыми от поставщика; на этой странице описывается поддержка наследования в контексте реляционной базы данных.

## <a name="entity-type-hierarchy-mapping"></a>Сопоставление иерархии типов сущностей

По соглашению EF не будет автоматически сканировать базовые или производные типы; Это означает, что если требуется сопоставить тип CLR в иерархии, необходимо явно указать этот тип в модели. Например, указание только базового типа иерархии не приведет к неявному включению всех его вложенных типов EF Core.

В следующем примере предоставляется DbSet для `Blog` и его подкласс `RssBlog` . Если `Blog` имеет любой другой подкласс, он не будет включаться в модель.

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/InheritanceDbSets.cs?name=InheritanceDbSets&highlight=3-4)]

> [!NOTE]
> Столбцы базы данных автоматически допускают значение null при необходимости при использовании сопоставления «одна таблица». Например, `RssUrl` столбец допускает значение null, так как у обычных `Blog` экземпляров нет этого свойства.

Если вы не хотите предоставлять `DbSet` для одной или нескольких сущностей в иерархии, можно также использовать API Fluent, чтобы убедиться, что они включены в модель.

> [!TIP]
> Если вы не полагаетесь на соглашения, можно явно указать базовый тип с помощью `HasBaseType` . Также можно использовать `.HasBaseType((Type)null)` для удаления типа сущности из иерархии.

## <a name="table-per-hierarchy-and-discriminator-configuration"></a>Конфигурация таблиц на иерархию и дискриминатор

По умолчанию EF сопоставляет наследование, используя шаблон «одна *таблица на иерархию* ». Функция «подтаблица» использует одну таблицу для хранения данных всех типов в иерархии, а столбец дискриминатора используется для указания типа, представляемого каждой строкой.

Приведенная выше модель сопоставлена со следующей схемой базы данных (Обратите внимание на `Discriminator` столбец, созданный неявно, который определяет, какой тип `Blog` хранится в каждой строке).

![Снимок экрана: результаты запроса иерархии сущностей блога с использованием шаблона «таблица на иерархию»](_static/inheritance-tph-data.png)

Можно настроить имя и тип столбца дискриминатора, а также значения, которые используются для обнаружения каждого типа в иерархии.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/DiscriminatorConfiguration.cs?name=DiscriminatorConfiguration&highlight=4-6)]

В приведенных выше примерах EF добавил дискриминатор неявно в качестве [Свойства теневой](xref:core/modeling/shadow-properties) копии базовой сущности иерархии. Это свойство можно настроить так же, как и любое другое:

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/DiscriminatorPropertyConfiguration.cs?name=DiscriminatorPropertyConfiguration&highlight=4-5)]

Наконец, дискриминатор также можно сопоставить с обычным свойством .NET в сущности:

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/NonShadowDiscriminator.cs?name=NonShadowDiscriminator&highlight=4)]

При запросе производных сущностей, использующих шаблон «подстановка», EF Core добавляет предикат к столбцу дискриминатора в запросе. Этот фильтр гарантирует, что мы не получаем дополнительные строки для базовых типов или родственных типов, отсутствующих в результатах. Этот предикат фильтра пропущен для базового типа сущности, так как при выполнении запроса для базовой сущности будут получены результаты для всех сущностей в иерархии. При материализации результатов из запроса, если мы получаем значение дискриминатора, которое не сопоставлено ни с одним из типов сущностей в модели, создается исключение, поскольку неизвестно, как материализовать результаты. Эта ошибка возникает только в том случае, если база данных содержит строки со значениями дискриминатора, которые не сопоставлены в модели EF. Если у вас есть такие данные, можно пометить дискриминатор в EF Core модели как неполное, чтобы указать, что необходимо всегда добавлять предикат фильтра для запроса любого типа в иерархии. `IsComplete(false)` вызов в конфигурации дискриминатора помечает сопоставление как неполное.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/DiscriminatorMappingIncomplete.cs?name=DiscriminatorMappingIncomplete&highlight=5)]

### <a name="shared-columns"></a>Общие столбцы

По умолчанию, если два одноуровневого типа сущности в иерархии имеют свойство с одинаковым именем, они будут сопоставлены с двумя отдельными столбцами. Однако, если их тип идентичен, они могут быть сопоставлены с одним столбцом базы данных:

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/SharedTPHColumns.cs?name=SharedTPHColumns&highlight=9,13)]

## <a name="table-per-type-configuration"></a>Конфигурация типа "один таблица на тип"

> [!NOTE]
> Функция "Таблица для каждого типа" (TPT) была представлена в EF Core 5,0. Тип данных "Таблица — конкретный" (TPC) поддерживается EF6, но еще не поддерживается EF Core.

В шаблоне сопоставления TPT все типы сопоставлены с отдельными таблицами. Как свойства базового типа, так и свойства производного типа хранятся в таблице, сопоставленной с этим типом. Таблицы, которые сопоставляются с производными типами, также хранят внешний ключ, который соединяет производную таблицу с базовой таблицей.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/TPTConfiguration.cs?name=TPTConfiguration)]

EF создаст следующую схему базы данных для указанной выше модели.

```sql
CREATE TABLE [Blogs] (
    [BlogId] int NOT NULL IDENTITY,
    [Url] nvarchar(max) NULL,
    CONSTRAINT [PK_Blogs] PRIMARY KEY ([BlogId])
);

CREATE TABLE [RssBlogs] (
    [BlogId] int NOT NULL,
    [RssUrl] nvarchar(max) NULL,
    CONSTRAINT [PK_RssBlogs] PRIMARY KEY ([BlogId]),
    CONSTRAINT [FK_RssBlogs_Blogs_BlogId] FOREIGN KEY ([BlogId]) REFERENCES [Blogs] ([BlogId]) ON DELETE NO ACTION
);
```

> [!NOTE]
> При переименовании ограничения PRIMARY KEY новое имя будет применено ко всем таблицам, сопоставленным с иерархией, а будущие версии EF будут разрешать Переименование ограничения только для конкретной таблицы, если [проблема 19970](https://github.com/dotnet/efcore/issues/19970) исправлена.

При использовании групповой конфигурации можно получить имя столбца для определенной таблицы, вызвав метод <xref:Microsoft.EntityFrameworkCore.RelationalPropertyExtensions.GetColumnName(Microsoft.EntityFrameworkCore.Metadata.IProperty,Microsoft.EntityFrameworkCore.Metadata.StoreObjectIdentifier@)> .

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/TPTConfiguration.cs?name=Metadata&highlight=10)]

> [!WARNING]
> Во многих случаях TPT отображает неограниченную производительность по сравнению с подиерархией. [Дополнительные сведения см. в документации по производительности](xref:core/performance/modeling-for-performance#inheritance-mapping).
