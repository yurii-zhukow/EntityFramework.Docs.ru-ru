---
title: Тестирование с помощью SQLite-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 7a2b75e2-1875-4487-9877-feff0651b5a6
uid: core/miscellaneous/testing/sqlite
ms.openlocfilehash: f7f847d8c766c0d4d7577ea6760ee72a17f84933
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/06/2020
ms.locfileid: "78414635"
---
# <a name="testing-with-sqlite"></a><span data-ttu-id="e80fb-102">Тестирование с помощью SQLite</span><span class="sxs-lookup"><span data-stu-id="e80fb-102">Testing with SQLite</span></span>

<span data-ttu-id="e80fb-103">SQLite имеет режим в памяти, который позволяет использовать SQLite для записи тестов в реляционную базу данных без затрат на реальные операции с базой данных.</span><span class="sxs-lookup"><span data-stu-id="e80fb-103">SQLite has an in-memory mode that allows you to use SQLite to write tests against a relational database, without the overhead of actual database operations.</span></span>

> [!TIP]  
> <span data-ttu-id="e80fb-104">Вы можете просмотреть [Пример](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Testing) этой статьи на сайте GitHub.</span><span class="sxs-lookup"><span data-stu-id="e80fb-104">You can view this article's [sample](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Testing) on GitHub</span></span>

## <a name="example-testing-scenario"></a><span data-ttu-id="e80fb-105">Пример сценария тестирования</span><span class="sxs-lookup"><span data-stu-id="e80fb-105">Example testing scenario</span></span>

<span data-ttu-id="e80fb-106">Рассмотрим следующую службу, которая позволяет коду приложения выполнять некоторые операции, связанные с блогами.</span><span class="sxs-lookup"><span data-stu-id="e80fb-106">Consider the following service that allows application code to perform some operations related to blogs.</span></span> <span data-ttu-id="e80fb-107">На внутреннем уровне используется `DbContext`, который подключается к базе данных SQL Server.</span><span class="sxs-lookup"><span data-stu-id="e80fb-107">Internally it uses a `DbContext` that connects to a SQL Server database.</span></span> <span data-ttu-id="e80fb-108">Было бы полезно переключить этот контекст для подключения к базе данных SQLite в памяти, чтобы мы могли писать эффективные тесты для этой службы, не изменяя код, или выполнять массовую работу по созданию тестовой двойной части контекста.</span><span class="sxs-lookup"><span data-stu-id="e80fb-108">It would be useful to swap this context to connect to an in-memory SQLite database so that we can write efficient tests for this service without having to modify the code, or do a lot of work to create a test double of the context.</span></span>

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BlogService.cs)]

## <a name="get-your-context-ready"></a><span data-ttu-id="e80fb-109">Подготовка контекста</span><span class="sxs-lookup"><span data-stu-id="e80fb-109">Get your context ready</span></span>

### <a name="avoid-configuring-two-database-providers"></a><span data-ttu-id="e80fb-110">Избегайте настройки двух поставщиков баз данных</span><span class="sxs-lookup"><span data-stu-id="e80fb-110">Avoid configuring two database providers</span></span>

<span data-ttu-id="e80fb-111">В тестах вы собираетесь извне настроить контекст для использования поставщика памяти.</span><span class="sxs-lookup"><span data-stu-id="e80fb-111">In your tests you are going to externally configure the context to use the InMemory provider.</span></span> <span data-ttu-id="e80fb-112">При настройке поставщика базы данных путем переопределения `OnConfiguring` в контексте необходимо добавить некоторый условный код, чтобы убедиться, что поставщик базы данных настроен только в том случае, если он еще не настроен.</span><span class="sxs-lookup"><span data-stu-id="e80fb-112">If you are configuring a database provider by overriding `OnConfiguring` in your context, then you need to add some conditional code to ensure that you only configure the database provider if one has not already been configured.</span></span>

> [!TIP]  
> <span data-ttu-id="e80fb-113">Если вы используете ASP.NET Core, этот код не нужен, так как поставщик базы данных настроен вне контекста (в Startup.cs).</span><span class="sxs-lookup"><span data-stu-id="e80fb-113">If you are using ASP.NET Core, then you should not need this code since your database provider is configured outside of the context (in Startup.cs).</span></span>

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BloggingContext.cs#OnConfiguring)]

### <a name="add-a-constructor-for-testing"></a><span data-ttu-id="e80fb-114">Добавление конструктора для тестирования</span><span class="sxs-lookup"><span data-stu-id="e80fb-114">Add a constructor for testing</span></span>

<span data-ttu-id="e80fb-115">Самый простой способ включить тестирование в другой базе данных — изменить контекст, чтобы предоставить конструктор, принимающий `DbContextOptions<TContext>`.</span><span class="sxs-lookup"><span data-stu-id="e80fb-115">The simplest way to enable testing against a different database is to modify your context to expose a constructor that accepts a `DbContextOptions<TContext>`.</span></span>

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BloggingContext.cs#Constructors)]

> [!TIP]  
> <span data-ttu-id="e80fb-116">`DbContextOptions<TContext>` сообщает контексту все параметры, такие как база данных для подключения.</span><span class="sxs-lookup"><span data-stu-id="e80fb-116">`DbContextOptions<TContext>` tells the context all of its settings, such as which database to connect to.</span></span> <span data-ttu-id="e80fb-117">Это тот же объект, который создается путем запуска метода onconfiguring в контексте.</span><span class="sxs-lookup"><span data-stu-id="e80fb-117">This is the same object that is built by running the OnConfiguring method in your context.</span></span>

## <a name="writing-tests"></a><span data-ttu-id="e80fb-118">Написание тестов</span><span class="sxs-lookup"><span data-stu-id="e80fb-118">Writing tests</span></span>

<span data-ttu-id="e80fb-119">Ключом к тестированию с помощью этого поставщика является возможность указать контексту использовать SQLite и контролировать область базы данных в памяти.</span><span class="sxs-lookup"><span data-stu-id="e80fb-119">The key to testing with this provider is the ability to tell the context to use SQLite, and control the scope of the in-memory database.</span></span> <span data-ttu-id="e80fb-120">Областью действия базы данных управляет, открывая и закрывая соединение.</span><span class="sxs-lookup"><span data-stu-id="e80fb-120">The scope of the database is controlled by opening and closing the connection.</span></span> <span data-ttu-id="e80fb-121">Областью действия базы данных является длительность открытия соединения.</span><span class="sxs-lookup"><span data-stu-id="e80fb-121">The database is scoped to the duration that the connection is open.</span></span> <span data-ttu-id="e80fb-122">Обычно требуется чистая база данных для каждого метода теста.</span><span class="sxs-lookup"><span data-stu-id="e80fb-122">Typically you want a clean database for each test method.</span></span>

>[!TIP]
> <span data-ttu-id="e80fb-123">Чтобы использовать `SqliteConnection()` и метод расширения `.UseSqlite()`, укажите ссылку на пакет NuGet [Microsoft. EntityFrameworkCore. SQLite](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Sqlite/).</span><span class="sxs-lookup"><span data-stu-id="e80fb-123">To use `SqliteConnection()` and the `.UseSqlite()` extension method, reference the NuGet package [Microsoft.EntityFrameworkCore.Sqlite](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Sqlite/).</span></span>

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/TestProject/SQLite/BlogServiceTests.cs)]
