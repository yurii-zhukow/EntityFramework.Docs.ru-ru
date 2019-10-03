---
title: Интерфейсы API создания и удаления — EF Core
author: bricelam
ms.author: bricelam
ms.date: 11/07/2018
ms.openlocfilehash: 88c1403d2fae740ad78bb7c41d404b0dd91e86ae
ms.sourcegitcommit: 6c28926a1e35e392b198a8729fc13c1c1968a27b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/02/2019
ms.locfileid: "71813440"
---
# <a name="create-and-drop-apis"></a><span data-ttu-id="94aeb-102">API создания и удаления</span><span class="sxs-lookup"><span data-stu-id="94aeb-102">Create and Drop APIs</span></span>

<span data-ttu-id="94aeb-103">Методы Енсурекреатед и Енсуределетед предоставляют упрощенную альтернативу [миграции](migrations/index.md) для управления схемой базы данных.</span><span class="sxs-lookup"><span data-stu-id="94aeb-103">The EnsureCreated and EnsureDeleted methods provide a lightweight alternative to [Migrations](migrations/index.md) for managing the database schema.</span></span> <span data-ttu-id="94aeb-104">Эти методы полезны в сценариях, когда данные являются временными и могут быть удалены при изменении схемы.</span><span class="sxs-lookup"><span data-stu-id="94aeb-104">These methods are useful in scenarios when the data is transient and can be dropped when the schema changes.</span></span> <span data-ttu-id="94aeb-105">Например, во время создания прототипов, в тестах или для локальных кэшей.</span><span class="sxs-lookup"><span data-stu-id="94aeb-105">For example during prototyping, in tests, or for local caches.</span></span>

<span data-ttu-id="94aeb-106">Некоторые поставщики (особенно нереляционные) не поддерживают миграцию.</span><span class="sxs-lookup"><span data-stu-id="94aeb-106">Some providers (especially non-relational ones) don't support Migrations.</span></span> <span data-ttu-id="94aeb-107">Для этих поставщиков Енсурекреатед часто является самым простым способом инициализации схемы базы данных.</span><span class="sxs-lookup"><span data-stu-id="94aeb-107">For these providers, EnsureCreated is often the easiest way to initialize the database schema.</span></span>

> [!WARNING]
> <span data-ttu-id="94aeb-108">Енсурекреатед и миграция не работают вместе.</span><span class="sxs-lookup"><span data-stu-id="94aeb-108">EnsureCreated and Migrations don't work well together.</span></span> <span data-ttu-id="94aeb-109">Если вы используете миграции, не используйте Енсурекреатед для инициализации схемы.</span><span class="sxs-lookup"><span data-stu-id="94aeb-109">If you're using Migrations, don't use EnsureCreated to initialize the schema.</span></span>

<span data-ttu-id="94aeb-110">Переход от Енсурекреатед к миграции не является простым интерфейсом.</span><span class="sxs-lookup"><span data-stu-id="94aeb-110">Transitioning from EnsureCreated to Migrations is not a seamless experience.</span></span> <span data-ttu-id="94aeb-111">Самый простой способ сделать это — удалить базу данных и создать ее повторно с помощью миграции.</span><span class="sxs-lookup"><span data-stu-id="94aeb-111">The simplest way to do it is to drop the database and re-create it using Migrations.</span></span> <span data-ttu-id="94aeb-112">Если предполагается использование миграций в будущем, лучше всего начать с миграции, а не использовать Енсурекреатед.</span><span class="sxs-lookup"><span data-stu-id="94aeb-112">If you anticipate using migrations in the future, it's best to just start with Migrations instead of using EnsureCreated.</span></span>

## <a name="ensuredeleted"></a><span data-ttu-id="94aeb-113">енсуределетед</span><span class="sxs-lookup"><span data-stu-id="94aeb-113">EnsureDeleted</span></span>

<span data-ttu-id="94aeb-114">Метод Енсуределетед удаляет базу данных, если она существует.</span><span class="sxs-lookup"><span data-stu-id="94aeb-114">The EnsureDeleted method will drop the database if it exists.</span></span> <span data-ttu-id="94aeb-115">Если у вас нет соответствующих разрешений, возникает исключение.</span><span class="sxs-lookup"><span data-stu-id="94aeb-115">If you don't have the appropriate permissions, an exception is thrown.</span></span>

``` csharp
// Drop the database if it exists
dbContext.Database.EnsureDeleted();
```

## <a name="ensurecreated"></a><span data-ttu-id="94aeb-116">енсурекреатед</span><span class="sxs-lookup"><span data-stu-id="94aeb-116">EnsureCreated</span></span>

<span data-ttu-id="94aeb-117">Енсурекреатед создаст базу данных, если она не существует, и инициализировать схему базы данных.</span><span class="sxs-lookup"><span data-stu-id="94aeb-117">EnsureCreated will create the database if it doesn't exist and initialize the database schema.</span></span> <span data-ttu-id="94aeb-118">Если существуют какие-либо таблицы (включая таблицы для другого класса DbContext), схема не будет инициализирована.</span><span class="sxs-lookup"><span data-stu-id="94aeb-118">If any tables exist (including tables for another DbContext class), the schema won't be initialized.</span></span>

``` csharp
// Create the database if it doesn't exist
dbContext.Database.EnsureCreated();
```

> [!TIP]
> <span data-ttu-id="94aeb-119">Также доступны асинхронные версии этих методов.</span><span class="sxs-lookup"><span data-stu-id="94aeb-119">Async versions of these methods are also available.</span></span>

## <a name="sql-script"></a><span data-ttu-id="94aeb-120">Скрипт SQL</span><span class="sxs-lookup"><span data-stu-id="94aeb-120">SQL Script</span></span>

<span data-ttu-id="94aeb-121">Чтобы получить SQL, используемый Енсурекреатед, можно использовать метод Женератекреатескрипт.</span><span class="sxs-lookup"><span data-stu-id="94aeb-121">To get the SQL used by EnsureCreated, you can use the GenerateCreateScript method.</span></span>

``` csharp
var sql = dbContext.Database.GenerateCreateScript();
```

## <a name="multiple-dbcontext-classes"></a><span data-ttu-id="94aeb-122">Несколько классов DbContext</span><span class="sxs-lookup"><span data-stu-id="94aeb-122">Multiple DbContext classes</span></span>

<span data-ttu-id="94aeb-123">Енсурекреатед работает, только если в базе данных нет таблиц.</span><span class="sxs-lookup"><span data-stu-id="94aeb-123">EnsureCreated only works when no tables are present in the database.</span></span> <span data-ttu-id="94aeb-124">При необходимости можно написать собственную проверку, чтобы определить, нужно ли инициализировать схему, и использовать базовую службу Ирелатионалдатабасекреатор для инициализации схемы.</span><span class="sxs-lookup"><span data-stu-id="94aeb-124">If needed, you can write your own check to see if the schema needs to be initialized, and use the underlying IRelationalDatabaseCreator service to initialize the schema.</span></span>

``` csharp
// TODO: Check whether the schema needs to be initialized

// Initialize the schema for this DbContext
var databaseCreator = dbContext.GetService<IRelationalDatabaseCreator>();
databaseCreator.CreateTables();
```
