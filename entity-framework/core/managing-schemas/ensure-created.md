---
title: Создание и удаление интерфейсы API — EF Core
author: bricelam
ms.author: bricelam
ms.date: 11/10/2017
ms.openlocfilehash: 336f6fd655603a2474a58dfef377e121d9b04c3a
ms.sourcegitcommit: a088421ecac4f5dc5213208170490181ae2f5f0f
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/08/2018
ms.locfileid: "51285643"
---
# <a name="create-and-drop-apis"></a>API создания и удаления

Методы EnsureCreated и EnsureDeleted предоставляют упрощенную альтернативу семафору [миграций](migrations/index.md) управления схемой базы данных. Это полезно в сценариях, когда данные являются временными и могут быть удалены, при изменении схемы. Например, во время создания прототипов, тесты, или в локальных кэшах.

Некоторые поставщики (особенно нереляционных тех) не поддерживается перенос. Для этого EnsureCreated часто является самым простым способом инициализации схемы базы данных.

> [!WARNING]
> EnsureCreated и миграции не работают вместе. Если вы используете миграций, не используйте EnsureCreated инициализировать схему.

Переход от EnsureCreated к миграции не удобную работу. Simpelest способ добиться этого — удалить базу данных и повторно создать его с помощью миграций. Если ожидается в будущем с помощью миграций, лучше просто начните с Миграциями, вместо использования EnsureCreated.

## <a name="ensuredeleted"></a>EnsureDeleted

Метод EnsureDeleted будет удалить базу данных, если он существует. Если у вас нет разрешения недопустимым, создается исключение.

``` csharp
// Drop the database if it exists
dbContext.Database.EnsureDeleted();
```

## <a name="ensurecreated"></a>EnsureCreated

EnsureCreated создаст базу данных, если он не существует и инициализировать схему базы данных. Если существует каких-либо таблиц (включая таблицы, для другого класса DbContext), схема не будет инициализироваться.

``` csharp
// Create the database if it doesn't exist
dbContext.Database.EnsureCreated();
```

> [!TIP]
> Асинхронные версии этих методов также доступны.

## <a name="sql-script"></a>Скрипт SQL

Чтобы получить код SQL, используемые EnsureCreated, можно использовать метод GenerateCreateScript.

``` csharp
var sql = dbContext.Database.GenerateCreateScript();
```

## <a name="multiple-dbcontext-classes"></a>Несколько классов DbContext

EnsureCreated работает только в том случае, если таблицы не присутствуют в базе данных. При необходимости можно написать собственную проверку для см. в разделе, необходимо инициализировать схему и использовать базовой службой IRelationalDatabaseCreator для инициализации схемы.

``` csharp
// TODO: Check whether the schema needs to be initialized

// Initialize the schema for this DbContext
var databaseCreator = dbContext.GetService<IRelationalDatabaseCreator>();
databaseCreator.CreateTables();
```
