---
title: Создание и удаление интерфейсы API — EF Core
author: bricelam
ms.author: bricelam
ms.date: 11/7/2018
ms.openlocfilehash: 40d9e3aa0aba1bf2bc341f01dd815ed7cb7b48fa
ms.sourcegitcommit: b3c2b34d5f006ee3b41d6668f16fe7dcad1b4317
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/15/2018
ms.locfileid: "51688633"
---
# <a name="create-and-drop-apis"></a><span data-ttu-id="595d4-102">API создания и удаления</span><span class="sxs-lookup"><span data-stu-id="595d4-102">Create and Drop APIs</span></span>

<span data-ttu-id="595d4-103">Методы EnsureCreated и EnsureDeleted предоставляют упрощенную альтернативу семафору [миграций](migrations/index.md) управления схемой базы данных.</span><span class="sxs-lookup"><span data-stu-id="595d4-103">The EnsureCreated and EnsureDeleted methods provide a lightweight alternative to [Migrations](migrations/index.md) for managing the database schema.</span></span> <span data-ttu-id="595d4-104">Эти методы полезны в сценариях, когда данные являются временными и могут быть удалены, при изменении схемы.</span><span class="sxs-lookup"><span data-stu-id="595d4-104">These methods are useful in scenarios when the data is transient and can be dropped when the schema changes.</span></span> <span data-ttu-id="595d4-105">Например, во время создания прототипов, тесты, или в локальных кэшах.</span><span class="sxs-lookup"><span data-stu-id="595d4-105">For example during prototyping, in tests, or for local caches.</span></span>

<span data-ttu-id="595d4-106">Некоторые поставщики (особенно нереляционных тех) не поддерживается перенос.</span><span class="sxs-lookup"><span data-stu-id="595d4-106">Some providers (especially non-relational ones) don't support Migrations.</span></span> <span data-ttu-id="595d4-107">Для этих поставщиков EnsureCreated часто является самым простым способом инициализации схемы базы данных.</span><span class="sxs-lookup"><span data-stu-id="595d4-107">For these providers, EnsureCreated is often the easiest way to initialize the database schema.</span></span>

> [!WARNING]
> <span data-ttu-id="595d4-108">EnsureCreated и миграции не работают вместе.</span><span class="sxs-lookup"><span data-stu-id="595d4-108">EnsureCreated and Migrations don't work well together.</span></span> <span data-ttu-id="595d4-109">Если вы используете миграций, не используйте EnsureCreated инициализировать схему.</span><span class="sxs-lookup"><span data-stu-id="595d4-109">If you're using Migrations, don't use EnsureCreated to initialize the schema.</span></span>

<span data-ttu-id="595d4-110">Переход от EnsureCreated к миграции не удобную работу.</span><span class="sxs-lookup"><span data-stu-id="595d4-110">Transitioning from EnsureCreated to Migrations is not a seamless experience.</span></span> <span data-ttu-id="595d4-111">Самый простой способ сделать это является удаление базы данных и повторно создать его с помощью миграций.</span><span class="sxs-lookup"><span data-stu-id="595d4-111">The simplest way to do it is to drop the database and re-create it using Migrations.</span></span> <span data-ttu-id="595d4-112">Если ожидается в будущем с помощью миграций, лучше просто начните с Миграциями, вместо использования EnsureCreated.</span><span class="sxs-lookup"><span data-stu-id="595d4-112">If you anticipate using migrations in the future, it's best to just start with Migrations instead of using EnsureCreated.</span></span>

## <a name="ensuredeleted"></a><span data-ttu-id="595d4-113">EnsureDeleted</span><span class="sxs-lookup"><span data-stu-id="595d4-113">EnsureDeleted</span></span>

<span data-ttu-id="595d4-114">Метод EnsureDeleted будет удалить базу данных, если он существует.</span><span class="sxs-lookup"><span data-stu-id="595d4-114">The EnsureDeleted method will drop the database if it exists.</span></span> <span data-ttu-id="595d4-115">Если у вас нет соответствующих разрешений, создается исключение.</span><span class="sxs-lookup"><span data-stu-id="595d4-115">If you don't have the appropriate permissions, an exception is thrown.</span></span>

``` csharp
// Drop the database if it exists
dbContext.Database.EnsureDeleted();
```

## <a name="ensurecreated"></a><span data-ttu-id="595d4-116">EnsureCreated</span><span class="sxs-lookup"><span data-stu-id="595d4-116">EnsureCreated</span></span>

<span data-ttu-id="595d4-117">EnsureCreated создаст базу данных, если он не существует и инициализировать схему базы данных.</span><span class="sxs-lookup"><span data-stu-id="595d4-117">EnsureCreated will create the database if it doesn't exist and initialize the database schema.</span></span> <span data-ttu-id="595d4-118">Если существует каких-либо таблиц (включая таблицы, для другого класса DbContext), схема не будет инициализироваться.</span><span class="sxs-lookup"><span data-stu-id="595d4-118">If any tables exist (including tables for another DbContext class), the schema won't be initialized.</span></span>

``` csharp
// Create the database if it doesn't exist
dbContext.Database.EnsureCreated();
```

> [!TIP]
> <span data-ttu-id="595d4-119">Асинхронные версии этих методов также доступны.</span><span class="sxs-lookup"><span data-stu-id="595d4-119">Async versions of these methods are also available.</span></span>

## <a name="sql-script"></a><span data-ttu-id="595d4-120">Скрипт SQL</span><span class="sxs-lookup"><span data-stu-id="595d4-120">SQL Script</span></span>

<span data-ttu-id="595d4-121">Чтобы получить код SQL, используемые EnsureCreated, можно использовать метод GenerateCreateScript.</span><span class="sxs-lookup"><span data-stu-id="595d4-121">To get the SQL used by EnsureCreated, you can use the GenerateCreateScript method.</span></span>

``` csharp
var sql = dbContext.Database.GenerateCreateScript();
```

## <a name="multiple-dbcontext-classes"></a><span data-ttu-id="595d4-122">Несколько классов DbContext</span><span class="sxs-lookup"><span data-stu-id="595d4-122">Multiple DbContext classes</span></span>

<span data-ttu-id="595d4-123">EnsureCreated работает только в том случае, если таблицы не присутствуют в базе данных.</span><span class="sxs-lookup"><span data-stu-id="595d4-123">EnsureCreated only works when no tables are present in the database.</span></span> <span data-ttu-id="595d4-124">При необходимости можно написать собственную проверку для см. в разделе, необходимо инициализировать схему и использовать базовой службой IRelationalDatabaseCreator для инициализации схемы.</span><span class="sxs-lookup"><span data-stu-id="595d4-124">If needed, you can write your own check to see if the schema needs to be initialized, and use the underlying IRelationalDatabaseCreator service to initialize the schema.</span></span>

``` csharp
// TODO: Check whether the schema needs to be initialized

// Initialize the schema for this DbContext
var databaseCreator = dbContext.GetService<IRelationalDatabaseCreator>();
databaseCreator.CreateTables();
```
