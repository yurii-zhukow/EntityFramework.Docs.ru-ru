---
title: Интерфейсы API создания и удаления — EF Core
description: API для создания и удаления баз данных с помощью Entity Framework Core
author: bricelam
ms.date: 11/07/2018
uid: core/managing-schemas/ensure-created
ms.openlocfilehash: c23311fbb8254ba183a6fd1661bba915aedc9a97
ms.sourcegitcommit: 0a25c03fa65ae6e0e0e3f66bac48d59eceb96a5a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/14/2020
ms.locfileid: "92062338"
---
# <a name="create-and-drop-apis"></a>API создания и удаления

Методы Енсурекреатед и Енсуределетед предоставляют упрощенную альтернативу [миграции](xref:core/managing-schemas/migrations/index) для управления схемой базы данных. Эти методы полезны в сценариях, когда данные являются временными и могут быть удалены при изменении схемы. Например, во время создания прототипов, в тестах или для локальных кэшей.

Некоторые поставщики (особенно нереляционные) не поддерживают миграцию. Для этих поставщиков Енсурекреатед часто является самым простым способом инициализации схемы базы данных.

> [!WARNING]
> Енсурекреатед и миграция не работают вместе. Если вы используете миграции, не используйте Енсурекреатед для инициализации схемы.

Переход от Енсурекреатед к миграции не является простым интерфейсом. Самый простой способ сделать это — удалить базу данных и создать ее повторно с помощью миграции. Если предполагается использование миграций в будущем, лучше всего начать с миграции, а не использовать Енсурекреатед.

## <a name="ensuredeleted"></a>енсуределетед

Метод Енсуределетед удаляет базу данных, если она существует. Если у вас нет соответствующих разрешений, возникает исключение.

```csharp
// Drop the database if it exists
dbContext.Database.EnsureDeleted();
```

## <a name="ensurecreated"></a>енсурекреатед

Енсурекреатед создаст базу данных, если она не существует, и инициализировать схему базы данных. Если существуют какие-либо таблицы (включая таблицы для другого класса DbContext), схема не будет инициализирована.

```csharp
// Create the database if it doesn't exist
dbContext.Database.EnsureCreated();
```

> [!TIP]
> Также доступны асинхронные версии этих методов.

## <a name="sql-script"></a>Скрипт SQL

Чтобы получить SQL, используемый Енсурекреатед, можно использовать метод Женератекреатескрипт.

```csharp
var sql = dbContext.Database.GenerateCreateScript();
```

## <a name="multiple-dbcontext-classes"></a>Несколько классов DbContext

Енсурекреатед работает, только если в базе данных нет таблиц. При необходимости можно написать собственную проверку, чтобы определить, нужно ли инициализировать схему, и использовать базовую службу Ирелатионалдатабасекреатор для инициализации схемы.

```csharp
// TODO: Check whether the schema needs to be initialized

// Initialize the schema for this DbContext
var databaseCreator = dbContext.GetService<IRelationalDatabaseCreator>();
databaseCreator.CreateTables();
```
