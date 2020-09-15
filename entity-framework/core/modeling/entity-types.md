---
title: Типы сущностей — EF Core
description: Как настроить и сопоставлять типы сущностей с помощью Entity Framework Core
author: roji
ms.date: 12/03/2019
uid: core/modeling/entity-types
ms.openlocfilehash: fead7f9e37efb7f674f429acbfd16c2ca78480d4
ms.sourcegitcommit: abda0872f86eefeca191a9a11bfca976bc14468b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/14/2020
ms.locfileid: "90071515"
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
