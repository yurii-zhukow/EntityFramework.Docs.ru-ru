---
title: "Тестирование с помощью InMemory - EF Core"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 0d0590f1-1ea3-4d5c-8f44-db17395cd3f3
ms.technology: entity-framework-core
uid: core/miscellaneous/testing/in-memory
ms.openlocfilehash: c5c48c575e9fd693d1f28d1a6d10eb83ebbc9d70
ms.sourcegitcommit: 860ec5d047342fbc4063a0de881c9861cc1f8813
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/05/2017
---
# <a name="testing-with-inmemory"></a><span data-ttu-id="0432e-102">Тестирование с использованием памяти</span><span class="sxs-lookup"><span data-stu-id="0432e-102">Testing with InMemory</span></span>

<span data-ttu-id="0432e-103">Поставщик InMemory полезен в тех случаях, когда для проверки компонентов с помощью то, что приблизительно подключение к базе данных real, одновременно снижая издержки фактических операций базы данных.</span><span class="sxs-lookup"><span data-stu-id="0432e-103">The InMemory provider is useful when you want to test components using something that approximates connecting to the real database, without the overhead of actual database operations.</span></span>

> [!TIP]  
> <span data-ttu-id="0432e-104">Можно просмотреть в этой статье [пример](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Testing) на GitHub.</span><span class="sxs-lookup"><span data-stu-id="0432e-104">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Testing) on GitHub.</span></span>

## <a name="inmemory-is-not-a-relational-database"></a><span data-ttu-id="0432e-105">InMemory не является реляционной базы данных</span><span class="sxs-lookup"><span data-stu-id="0432e-105">InMemory is not a relational database</span></span>

<span data-ttu-id="0432e-106">Поставщики базы данных основной EF не обязательно реляционных баз данных.</span><span class="sxs-lookup"><span data-stu-id="0432e-106">EF Core database providers do not have to be relational databases.</span></span> <span data-ttu-id="0432e-107">InMemory предназначен для базы данных общего назначения для тестирования и не предназначен для имитации реляционной базы данных.</span><span class="sxs-lookup"><span data-stu-id="0432e-107">InMemory is designed to be a general purpose database for testing, and is not designed to mimic a relational database.</span></span>

<span data-ttu-id="0432e-108">Некоторые примеры включают:</span><span class="sxs-lookup"><span data-stu-id="0432e-108">Some examples of this include:</span></span>
* <span data-ttu-id="0432e-109">InMemory дает возможность сохранения данных, нарушающих ограничения ссылочной целостности в реляционной базе данных.</span><span class="sxs-lookup"><span data-stu-id="0432e-109">InMemory will allow you to save data that would violate referential integrity constraints in a relational database.</span></span>

* <span data-ttu-id="0432e-110">Если вы используете DefaultValueSql(string) для свойства в модели, это — это реляционная база данных API и не будет действовать при выполнении для InMemory.</span><span class="sxs-lookup"><span data-stu-id="0432e-110">If you use DefaultValueSql(string) for a property in your model, this is a relational database API and will have no effect when running against InMemory.</span></span>

> [!TIP]  
> <span data-ttu-id="0432e-111">Многие теста для целей эти различия не будет иметь значения.</span><span class="sxs-lookup"><span data-stu-id="0432e-111">For many test purposes these differences will not matter.</span></span> <span data-ttu-id="0432e-112">Тем не менее, если вы хотите протестировать то, что во многом напоминает true реляционной базы данных, рассмотрите возможность использования [режиме in-memory SQLite](sqlite.md).</span><span class="sxs-lookup"><span data-stu-id="0432e-112">However, if you want to test against something that behaves more like a true relational database, then consider using [SQLite in-memory mode](sqlite.md).</span></span>

## <a name="example-testing-scenario"></a><span data-ttu-id="0432e-113">Пример сценария тестирования</span><span class="sxs-lookup"><span data-stu-id="0432e-113">Example testing scenario</span></span>

<span data-ttu-id="0432e-114">Рассмотрим следующую службу, благодаря которой код приложения для выполнения некоторых операций, связанных с блоги.</span><span class="sxs-lookup"><span data-stu-id="0432e-114">Consider the following service that allows application code to perform some operations related to blogs.</span></span> <span data-ttu-id="0432e-115">Внутренне используется `DbContext` , подключается к базе данных SQL Server.</span><span class="sxs-lookup"><span data-stu-id="0432e-115">Internally it uses a `DbContext` that connects to a SQL Server database.</span></span> <span data-ttu-id="0432e-116">Оно полезно для замены этот контекст для подключения к базе данных InMemory таким образом, мы писать эффективный тесты для этой службы без необходимости изменения кода или выполнить большой объем работы, чтобы создать тестовый двойные контекста.</span><span class="sxs-lookup"><span data-stu-id="0432e-116">It would be useful to swap this context to connect to an InMemory database so that we can write efficient tests for this service without having to modify the code, or do a lot of work to create a test double of the context.</span></span>

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BlogService.cs)]

## <a name="get-your-context-ready"></a><span data-ttu-id="0432e-117">Подготовьтесь к контексту</span><span class="sxs-lookup"><span data-stu-id="0432e-117">Get your context ready</span></span>

### <a name="avoid-configuring-two-database-providers"></a><span data-ttu-id="0432e-118">Не настраивать двух поставщиков базы данных</span><span class="sxs-lookup"><span data-stu-id="0432e-118">Avoid configuring two database providers</span></span>

<span data-ttu-id="0432e-119">В тестах вы собираетесь настроить контекст для использования поставщика InMemory извне.</span><span class="sxs-lookup"><span data-stu-id="0432e-119">In your tests you are going to externally configure the context to use the InMemory provider.</span></span> <span data-ttu-id="0432e-120">При настройке поставщика базы данных путем переопределения `OnConfiguring` в контексте текущего пользователя, затем необходимо добавить некоторые условного коде, чтобы гарантировать только Настройка базы данных поставщика, если уже не был настроен.</span><span class="sxs-lookup"><span data-stu-id="0432e-120">If you are configuring a database provider by overriding `OnConfiguring` in your context, then you need to add some conditional code to ensure that you only configure the database provider if one has not already been configured.</span></span>

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BloggingContext.cs#OnConfiguring)]

> [!TIP]  
> <span data-ttu-id="0432e-121">При использовании ASP.NET Core затем нет необходимости этот код, так как поставщик базы данных уже настроены вне контекста (в файле Startup.cs).</span><span class="sxs-lookup"><span data-stu-id="0432e-121">If you are using ASP.NET Core, then you should not need this code since your database provider is already configured outside of the context (in Startup.cs).</span></span>

### <a name="add-a-constructor-for-testing"></a><span data-ttu-id="0432e-122">Добавьте конструктор для тестирования</span><span class="sxs-lookup"><span data-stu-id="0432e-122">Add a constructor for testing</span></span>

<span data-ttu-id="0432e-123">Самый простой способ тестирования в другой базе данных требуется изменить контекст для предоставления конструктор, принимающий `DbContextOptions<TContext>`.</span><span class="sxs-lookup"><span data-stu-id="0432e-123">The simplest way to enable testing against a different database is to modify your context to expose a constructor that accepts a `DbContextOptions<TContext>`.</span></span>

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BloggingContext.cs#Constructors)]

> [!TIP]  
> <span data-ttu-id="0432e-124">`DbContextOptions<TContext>`Указывает контекст, все его параметры, такие как базы данных для подключения к.</span><span class="sxs-lookup"><span data-stu-id="0432e-124">`DbContextOptions<TContext>` tells the context all of its settings, such as which database to connect to.</span></span> <span data-ttu-id="0432e-125">Это тот же объект, который создается при выполнении метода OnConfiguring в контексте текущего пользователя.</span><span class="sxs-lookup"><span data-stu-id="0432e-125">This is the same object that is built by running the OnConfiguring method in your context.</span></span>

## <a name="writing-tests"></a><span data-ttu-id="0432e-126">Написание тестов</span><span class="sxs-lookup"><span data-stu-id="0432e-126">Writing tests</span></span>

<span data-ttu-id="0432e-127">Ключ для тестирования с этим поставщиком является возможность определить контекст для использования поставщика InMemory и управлять областью базы данных в памяти.</span><span class="sxs-lookup"><span data-stu-id="0432e-127">The key to testing with this provider is the ability to tell the context to use the InMemory provider, and control the scope of the in-memory database.</span></span> <span data-ttu-id="0432e-128">Обычно требуется чистую базу данных для каждого метода теста.</span><span class="sxs-lookup"><span data-stu-id="0432e-128">Typically you want a clean database for each test method.</span></span>

<span data-ttu-id="0432e-129">Ниже приведен пример тестового класса, использующего InMemory базы данных.</span><span class="sxs-lookup"><span data-stu-id="0432e-129">Here is an example of a test class that uses the InMemory database.</span></span> <span data-ttu-id="0432e-130">Каждый метод теста указывает уникальное имя базы данных, это означает, что каждый метод имеет собственную базу данных в памяти.</span><span class="sxs-lookup"><span data-stu-id="0432e-130">Each test method specifies a unique database name, meaning each method has its own InMemory database.</span></span>

>[!TIP]
> <span data-ttu-id="0432e-131">Для использования `.UseInMemoryDatabase()` метод расширения, ссылка пакета Nuget `Microsoft.EntityFrameworkCore.InMemory`.</span><span class="sxs-lookup"><span data-stu-id="0432e-131">To use the `.UseInMemoryDatabase()` extension method, reference the Nuget package `Microsoft.EntityFrameworkCore.InMemory`.</span></span>

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/TestProject/InMemory/BlogServiceTests.cs)]
