---
title: "Тестирование с помощью SQLite - EF Core"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 7a2b75e2-1875-4487-9877-feff0651b5a6
ms.technology: entity-framework-core
uid: core/miscellaneous/testing/sqlite
ms.openlocfilehash: 8fcb4b1a37da6f52219ebe3c672160627fe28fab
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/27/2017
---
# <a name="testing-with-sqlite"></a><span data-ttu-id="a5c93-102">Тестирование с помощью SQLite</span><span class="sxs-lookup"><span data-stu-id="a5c93-102">Testing with SQLite</span></span>

<span data-ttu-id="a5c93-103">SQLite имеет режим в памяти, который можно использовать для написания тестов с реляционной базой данных, одновременно снижая издержки фактических операций базы данных SQLite.</span><span class="sxs-lookup"><span data-stu-id="a5c93-103">SQLite has an in-memory mode that allows you to use SQLite to write tests against a relational database, without the overhead of actual database operations.</span></span>

> [!TIP]  
> <span data-ttu-id="a5c93-104">Можно просмотреть в этой статье [пример](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Testing) на GitHub</span><span class="sxs-lookup"><span data-stu-id="a5c93-104">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Testing) on GitHub</span></span>

## <a name="example-testing-scenario"></a><span data-ttu-id="a5c93-105">Пример сценария тестирования</span><span class="sxs-lookup"><span data-stu-id="a5c93-105">Example testing scenario</span></span>

<span data-ttu-id="a5c93-106">Рассмотрим следующую службу, благодаря которой код приложения для выполнения некоторых операций, связанных с блоги.</span><span class="sxs-lookup"><span data-stu-id="a5c93-106">Consider the following service that allows application code to perform some operations related to blogs.</span></span> <span data-ttu-id="a5c93-107">Внутренне используется `DbContext` , подключается к базе данных SQL Server.</span><span class="sxs-lookup"><span data-stu-id="a5c93-107">Internally it uses a `DbContext` that connects to a SQL Server database.</span></span> <span data-ttu-id="a5c93-108">Оно полезно для замены этот контекст для подключения к базе данных SQLite в памяти, чтобы мы писать эффективный тесты для этой службы без необходимости изменения кода, либо выполнить большой объем работы, чтобы создать тестовый двойные контекста.</span><span class="sxs-lookup"><span data-stu-id="a5c93-108">It would be useful to swap this context to connect to an in-memory SQLite database so that we can write efficient tests for this service without having to modify the code, or do a lot of work to create a test double of the context.</span></span>

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BlogService.cs)]

## <a name="get-your-context-ready"></a><span data-ttu-id="a5c93-109">Подготовьтесь к контексту</span><span class="sxs-lookup"><span data-stu-id="a5c93-109">Get your context ready</span></span>

### <a name="avoid-configuring-two-database-providers"></a><span data-ttu-id="a5c93-110">Не настраивать двух поставщиков базы данных</span><span class="sxs-lookup"><span data-stu-id="a5c93-110">Avoid configuring two database providers</span></span>

<span data-ttu-id="a5c93-111">В тестах вы собираетесь настроить контекст для использования поставщика InMemory извне.</span><span class="sxs-lookup"><span data-stu-id="a5c93-111">In your tests you are going to externally configure the context to use the InMemory provider.</span></span> <span data-ttu-id="a5c93-112">При настройке поставщика базы данных путем переопределения `OnConfiguring` в контексте текущего пользователя, затем необходимо добавить некоторые условного коде, чтобы гарантировать только Настройка базы данных поставщика, если уже не был настроен.</span><span class="sxs-lookup"><span data-stu-id="a5c93-112">If you are configuring a database provider by overriding `OnConfiguring` in your context, then you need to add some conditional code to ensure that you only configure the database provider if one has not already been configured.</span></span>

> [!TIP]  
> <span data-ttu-id="a5c93-113">При использовании ASP.NET Core затем нет необходимости этот код так как настроен поставщик базы данных вне контекста (в файле Startup.cs).</span><span class="sxs-lookup"><span data-stu-id="a5c93-113">If you are using ASP.NET Core, then you should not need this code since your database provider is configured outside of the context (in Startup.cs).</span></span>

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BloggingContext.cs#OnConfiguring)]

### <a name="add-a-constructor-for-testing"></a><span data-ttu-id="a5c93-114">Добавьте конструктор для тестирования</span><span class="sxs-lookup"><span data-stu-id="a5c93-114">Add a constructor for testing</span></span>

<span data-ttu-id="a5c93-115">Самый простой способ тестирования в другой базе данных требуется изменить контекст для предоставления конструктор, принимающий `DbContextOptions<TContext>`.</span><span class="sxs-lookup"><span data-stu-id="a5c93-115">The simplest way to enable testing against a different database is to modify your context to expose a constructor that accepts a `DbContextOptions<TContext>`.</span></span>

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BloggingContext.cs#Constructors)]

> [!TIP]  
> <span data-ttu-id="a5c93-116">`DbContextOptions<TContext>`Указывает контекст, все его параметры, такие как базы данных для подключения к.</span><span class="sxs-lookup"><span data-stu-id="a5c93-116">`DbContextOptions<TContext>` tells the context all of its settings, such as which database to connect to.</span></span> <span data-ttu-id="a5c93-117">Это тот же объект, который создается при выполнении метода OnConfiguring в контексте текущего пользователя.</span><span class="sxs-lookup"><span data-stu-id="a5c93-117">This is the same object that is built by running the OnConfiguring method in your context.</span></span>

## <a name="writing-tests"></a><span data-ttu-id="a5c93-118">Написание тестов</span><span class="sxs-lookup"><span data-stu-id="a5c93-118">Writing tests</span></span>

<span data-ttu-id="a5c93-119">Ключ для тестирования с этим поставщиком является возможность определить контекст SQLite и управлять областью базы данных в памяти.</span><span class="sxs-lookup"><span data-stu-id="a5c93-119">The key to testing with this provider is the ability to tell the context to use SQLite, and control the scope of the in-memory database.</span></span> <span data-ttu-id="a5c93-120">Открытие и закрытие соединения управляется области базы данных.</span><span class="sxs-lookup"><span data-stu-id="a5c93-120">The scope of the database is controlled by opening and closing the connection.</span></span> <span data-ttu-id="a5c93-121">Базы данных, ограничиваются продолжительность времени, которая при открытом соединении.</span><span class="sxs-lookup"><span data-stu-id="a5c93-121">The database is scoped to the duration that the connection is open.</span></span> <span data-ttu-id="a5c93-122">Обычно требуется чистую базу данных для каждого метода теста.</span><span class="sxs-lookup"><span data-stu-id="a5c93-122">Typically you want a clean database for each test method.</span></span>

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/TestProject/SQLite/BlogServiceTests.cs)]
