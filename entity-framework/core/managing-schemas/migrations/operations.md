---
title: Операции с настраиваемыми миграциями — EF Core
description: Управление пользовательскими и необработанными миграциями SQL для управления схемой базы данных с помощью Entity Framework Core
author: bricelam
ms.date: 10/27/2020
uid: core/managing-schemas/migrations/operations
ms.openlocfilehash: 2abde4d5eac977a746863dcfd77bc85a34e2166c
ms.sourcegitcommit: f3512e3a98e685a3ba409c1d0157ce85cc390cf4
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2020
ms.locfileid: "94429836"
---
# <a name="custom-migrations-operations"></a>Операции пользовательской миграции

API Мигратионбуилдер позволяет выполнять различные виды операций во время миграции, но это далеко не все. Однако API также является расширяемым, позволяя определять собственные операции. Существует два способа расширения API: с помощью `Sql()` метода или путем определения пользовательских `MigrationOperation` объектов.

Чтобы проиллюстрировать это, давайте взглянем на реализацию операции, которая создает пользователя базы данных, используя каждый из подходов. В наших переносах мы хотим включить написание следующего кода:

```csharp
migrationBuilder.CreateUser("SQLUser1", "Password");
```

## <a name="using-migrationbuildersql"></a>Использование Мигратионбуилдер. SQL ()

Самый простой способ реализовать пользовательскую операцию — определить метод расширения, который вызывает `MigrationBuilder.Sql()` . Ниже приведен пример, в котором создается соответствующий Transact-SQL.

[!code-csharp[](../../../../samples/core/Schemas/Migrations/CustomOperationSql.cs#snippet_CustomOperationSql)]

> [!TIP]
> Используйте `EXEC` функцию, если инструкция должна быть первой или единственной в пакете SQL. Также может потребоваться обойти ошибки синтаксического анализатора в сценариях миграции идемпотентными, которые могут возникать, если в таблице не существует столбцов, на которые имеются ссылки.

Если для миграции требуется поддержка нескольких поставщиков баз данных, можно использовать `MigrationBuilder.ActiveProvider` свойство. Ниже приведен пример, поддерживающий как Microsoft SQL Server, так и PostgreSQL.

[!code-csharp[](../../../../samples/core/Schemas/Migrations/CustomOperationMultiSql.cs#snippet_CustomOperationMultiSql)]

Этот подход работает только в том случае, если вы знакомы с каждым поставщиком, к которому будет применена пользовательская операция.

## <a name="using-a-migrationoperation"></a>Использование Мигратионоператион

Чтобы отделить пользовательскую операцию от SQL, можно определить собственную `MigrationOperation` для ее представления. Затем операция передается поставщику, чтобы он мог определить соответствующий SQL.

[!code-csharp[](../../../../samples/core/Schemas/Migrations/CustomOperation.cs#snippet_CreateUserOperation)]

При таком подходе методу расширения просто нужно добавить одну из этих операций в `MigrationBuilder.Operations` .

[!code-csharp[](../../../../samples/core/Schemas/Migrations/CustomOperation.cs#snippet_MigrationBuilderExtension)]

Этот подход требует, чтобы каждый поставщик знал, как создать SQL для этой операции в своей `IMigrationsSqlGenerator` службе. Ниже приведен пример переопределения генератора SQL Server для управления новой операцией.

[!code-csharp[](../../../../samples/core/Schemas/Migrations/CustomOperation.cs#snippet_MigrationsSqlGenerator)]

Замените службу генератора SQL по умолчанию новой.

[!code-csharp[](../../../../samples/core/Schemas/Migrations/CustomOperation.cs#snippet_OnConfiguring)]
