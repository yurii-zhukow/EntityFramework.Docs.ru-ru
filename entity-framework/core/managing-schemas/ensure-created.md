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
# <a name="create-and-drop-apis"></a><span data-ttu-id="8d0bb-103">API создания и удаления</span><span class="sxs-lookup"><span data-stu-id="8d0bb-103">Create and Drop APIs</span></span>

<span data-ttu-id="8d0bb-104">Методы Енсурекреатед и Енсуределетед предоставляют упрощенную альтернативу [миграции](xref:core/managing-schemas/migrations/index) для управления схемой базы данных.</span><span class="sxs-lookup"><span data-stu-id="8d0bb-104">The EnsureCreated and EnsureDeleted methods provide a lightweight alternative to [Migrations](xref:core/managing-schemas/migrations/index) for managing the database schema.</span></span> <span data-ttu-id="8d0bb-105">Эти методы полезны в сценариях, когда данные являются временными и могут быть удалены при изменении схемы.</span><span class="sxs-lookup"><span data-stu-id="8d0bb-105">These methods are useful in scenarios when the data is transient and can be dropped when the schema changes.</span></span> <span data-ttu-id="8d0bb-106">Например, во время создания прототипов, в тестах или для локальных кэшей.</span><span class="sxs-lookup"><span data-stu-id="8d0bb-106">For example during prototyping, in tests, or for local caches.</span></span>

<span data-ttu-id="8d0bb-107">Некоторые поставщики (особенно нереляционные) не поддерживают миграцию.</span><span class="sxs-lookup"><span data-stu-id="8d0bb-107">Some providers (especially non-relational ones) don't support Migrations.</span></span> <span data-ttu-id="8d0bb-108">Для этих поставщиков Енсурекреатед часто является самым простым способом инициализации схемы базы данных.</span><span class="sxs-lookup"><span data-stu-id="8d0bb-108">For these providers, EnsureCreated is often the easiest way to initialize the database schema.</span></span>

> [!WARNING]
> <span data-ttu-id="8d0bb-109">Енсурекреатед и миграция не работают вместе.</span><span class="sxs-lookup"><span data-stu-id="8d0bb-109">EnsureCreated and Migrations don't work well together.</span></span> <span data-ttu-id="8d0bb-110">Если вы используете миграции, не используйте Енсурекреатед для инициализации схемы.</span><span class="sxs-lookup"><span data-stu-id="8d0bb-110">If you're using Migrations, don't use EnsureCreated to initialize the schema.</span></span>

<span data-ttu-id="8d0bb-111">Переход от Енсурекреатед к миграции не является простым интерфейсом.</span><span class="sxs-lookup"><span data-stu-id="8d0bb-111">Transitioning from EnsureCreated to Migrations is not a seamless experience.</span></span> <span data-ttu-id="8d0bb-112">Самый простой способ сделать это — удалить базу данных и создать ее повторно с помощью миграции.</span><span class="sxs-lookup"><span data-stu-id="8d0bb-112">The simplest way to do it is to drop the database and re-create it using Migrations.</span></span> <span data-ttu-id="8d0bb-113">Если предполагается использование миграций в будущем, лучше всего начать с миграции, а не использовать Енсурекреатед.</span><span class="sxs-lookup"><span data-stu-id="8d0bb-113">If you anticipate using migrations in the future, it's best to just start with Migrations instead of using EnsureCreated.</span></span>

## <a name="ensuredeleted"></a><span data-ttu-id="8d0bb-114">енсуределетед</span><span class="sxs-lookup"><span data-stu-id="8d0bb-114">EnsureDeleted</span></span>

<span data-ttu-id="8d0bb-115">Метод Енсуределетед удаляет базу данных, если она существует.</span><span class="sxs-lookup"><span data-stu-id="8d0bb-115">The EnsureDeleted method will drop the database if it exists.</span></span> <span data-ttu-id="8d0bb-116">Если у вас нет соответствующих разрешений, возникает исключение.</span><span class="sxs-lookup"><span data-stu-id="8d0bb-116">If you don't have the appropriate permissions, an exception is thrown.</span></span>

```csharp
// Drop the database if it exists
dbContext.Database.EnsureDeleted();
```

## <a name="ensurecreated"></a><span data-ttu-id="8d0bb-117">енсурекреатед</span><span class="sxs-lookup"><span data-stu-id="8d0bb-117">EnsureCreated</span></span>

<span data-ttu-id="8d0bb-118">Енсурекреатед создаст базу данных, если она не существует, и инициализировать схему базы данных.</span><span class="sxs-lookup"><span data-stu-id="8d0bb-118">EnsureCreated will create the database if it doesn't exist and initialize the database schema.</span></span> <span data-ttu-id="8d0bb-119">Если существуют какие-либо таблицы (включая таблицы для другого класса DbContext), схема не будет инициализирована.</span><span class="sxs-lookup"><span data-stu-id="8d0bb-119">If any tables exist (including tables for another DbContext class), the schema won't be initialized.</span></span>

```csharp
// Create the database if it doesn't exist
dbContext.Database.EnsureCreated();
```

> [!TIP]
> <span data-ttu-id="8d0bb-120">Также доступны асинхронные версии этих методов.</span><span class="sxs-lookup"><span data-stu-id="8d0bb-120">Async versions of these methods are also available.</span></span>

## <a name="sql-script"></a><span data-ttu-id="8d0bb-121">Скрипт SQL</span><span class="sxs-lookup"><span data-stu-id="8d0bb-121">SQL Script</span></span>

<span data-ttu-id="8d0bb-122">Чтобы получить SQL, используемый Енсурекреатед, можно использовать метод Женератекреатескрипт.</span><span class="sxs-lookup"><span data-stu-id="8d0bb-122">To get the SQL used by EnsureCreated, you can use the GenerateCreateScript method.</span></span>

```csharp
var sql = dbContext.Database.GenerateCreateScript();
```

## <a name="multiple-dbcontext-classes"></a><span data-ttu-id="8d0bb-123">Несколько классов DbContext</span><span class="sxs-lookup"><span data-stu-id="8d0bb-123">Multiple DbContext classes</span></span>

<span data-ttu-id="8d0bb-124">Енсурекреатед работает, только если в базе данных нет таблиц.</span><span class="sxs-lookup"><span data-stu-id="8d0bb-124">EnsureCreated only works when no tables are present in the database.</span></span> <span data-ttu-id="8d0bb-125">При необходимости можно написать собственную проверку, чтобы определить, нужно ли инициализировать схему, и использовать базовую службу Ирелатионалдатабасекреатор для инициализации схемы.</span><span class="sxs-lookup"><span data-stu-id="8d0bb-125">If needed, you can write your own check to see if the schema needs to be initialized, and use the underlying IRelationalDatabaseCreator service to initialize the schema.</span></span>

```csharp
// TODO: Check whether the schema needs to be initialized

// Initialize the schema for this DbContext
var databaseCreator = dbContext.GetService<IRelationalDatabaseCreator>();
databaseCreator.CreateTables();
```
