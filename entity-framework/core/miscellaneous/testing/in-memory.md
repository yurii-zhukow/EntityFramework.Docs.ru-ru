---
title: Тестирование с использованием EF Core памяти
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 0d0590f1-1ea3-4d5c-8f44-db17395cd3f3
uid: core/miscellaneous/testing/in-memory
ms.openlocfilehash: 18641677098c20d9172136b07868dcb647d189c6
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/06/2020
ms.locfileid: "78414029"
---
# <a name="testing-with-inmemory"></a><span data-ttu-id="c1d63-102">Тестирование с помощью InMemory</span><span class="sxs-lookup"><span data-stu-id="c1d63-102">Testing with InMemory</span></span>

<span data-ttu-id="c1d63-103">Поставщик нехватки памяти полезен, если нужно протестировать компоненты с помощью чего-то, которое приблизительно подключается к реальной базе данных, без издержек на выполнение фактических операций с базой данных.</span><span class="sxs-lookup"><span data-stu-id="c1d63-103">The InMemory provider is useful when you want to test components using something that approximates connecting to the real database, without the overhead of actual database operations.</span></span>

> [!TIP]  
> <span data-ttu-id="c1d63-104">Для этой статьи вы можете скачать [пример](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Testing) из репозитория GitHub.</span><span class="sxs-lookup"><span data-stu-id="c1d63-104">You can view this article's [sample](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Testing) on GitHub.</span></span>

## <a name="inmemory-is-not-a-relational-database"></a><span data-ttu-id="c1d63-105">Неиспользуемая память не является реляционной базой данных</span><span class="sxs-lookup"><span data-stu-id="c1d63-105">InMemory is not a relational database</span></span>

<span data-ttu-id="c1d63-106">Поставщики баз данных EF Core не обязательно должны быть реляционными базами данных.</span><span class="sxs-lookup"><span data-stu-id="c1d63-106">EF Core database providers do not have to be relational databases.</span></span> <span data-ttu-id="c1d63-107">Параметр "память" предназначен для использования в качестве базы данных общего назначения для тестирования и не предназначен для имитации реляционной базы данных.</span><span class="sxs-lookup"><span data-stu-id="c1d63-107">InMemory is designed to be a general purpose database for testing, and is not designed to mimic a relational database.</span></span>

<span data-ttu-id="c1d63-108">Ниже приведены некоторые примеры.</span><span class="sxs-lookup"><span data-stu-id="c1d63-108">Some examples of this include:</span></span>

* <span data-ttu-id="c1d63-109">Параметр "память" позволяет сохранять данные, нарушающие ограничения ссылочной целостности в реляционной базе данных.</span><span class="sxs-lookup"><span data-stu-id="c1d63-109">InMemory will allow you to save data that would violate referential integrity constraints in a relational database.</span></span>
* <span data-ttu-id="c1d63-110">Если для свойства в модели используется Дефаултвалуескл (строка), это API-интерфейс реляционной базы данных и не будет оказывать влияния при выполнении в памяти.</span><span class="sxs-lookup"><span data-stu-id="c1d63-110">If you use DefaultValueSql(string) for a property in your model, this is a relational database API and will have no effect when running against InMemory.</span></span>
* <span data-ttu-id="c1d63-111">[Параллелизм через отметку времени или версию строки](xref:core/modeling/concurrency#timestamprowversion) (`[Timestamp]` или `IsRowVersion`) не поддерживается.</span><span class="sxs-lookup"><span data-stu-id="c1d63-111">[Concurrency via Timestamp/row version](xref:core/modeling/concurrency#timestamprowversion) (`[Timestamp]` or `IsRowVersion`) is not supported.</span></span> <span data-ttu-id="c1d63-112">Если обновление выполняется с помощью старого маркера параллелизма, [дбупдатеконкурренциексцептион](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.dbupdateconcurrencyexception) не создается.</span><span class="sxs-lookup"><span data-stu-id="c1d63-112">No [DbUpdateConcurrencyException](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.dbupdateconcurrencyexception) will be thrown if an update is done using an old concurrency token.</span></span>

> [!TIP]  
> <span data-ttu-id="c1d63-113">Для многих целей тестирования эти различия не имеют значения.</span><span class="sxs-lookup"><span data-stu-id="c1d63-113">For many test purposes these differences will not matter.</span></span> <span data-ttu-id="c1d63-114">Тем не менее, если нужно протестировать нечто вроде реальной реляционной базы данных, рассмотрите возможность использования [SQLite в памяти](sqlite.md).</span><span class="sxs-lookup"><span data-stu-id="c1d63-114">However, if you want to test against something that behaves more like a true relational database, then consider using [SQLite in-memory mode](sqlite.md).</span></span>

## <a name="example-testing-scenario"></a><span data-ttu-id="c1d63-115">Пример сценария тестирования</span><span class="sxs-lookup"><span data-stu-id="c1d63-115">Example testing scenario</span></span>

<span data-ttu-id="c1d63-116">Рассмотрим следующую службу, которая позволяет коду приложения выполнять некоторые операции, связанные с блогами.</span><span class="sxs-lookup"><span data-stu-id="c1d63-116">Consider the following service that allows application code to perform some operations related to blogs.</span></span> <span data-ttu-id="c1d63-117">На внутреннем уровне используется `DbContext`, который подключается к базе данных SQL Server.</span><span class="sxs-lookup"><span data-stu-id="c1d63-117">Internally it uses a `DbContext` that connects to a SQL Server database.</span></span> <span data-ttu-id="c1d63-118">Было бы полезно переключить этот контекст для подключения к базе данных в памяти, чтобы мы могли написать эффективные тесты для этой службы, не изменяя код, или выполнить массовую работу по созданию тестовой двойной части контекста.</span><span class="sxs-lookup"><span data-stu-id="c1d63-118">It would be useful to swap this context to connect to an InMemory database so that we can write efficient tests for this service without having to modify the code, or do a lot of work to create a test double of the context.</span></span>

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BlogService.cs)]

## <a name="get-your-context-ready"></a><span data-ttu-id="c1d63-119">Подготовка контекста</span><span class="sxs-lookup"><span data-stu-id="c1d63-119">Get your context ready</span></span>

### <a name="avoid-configuring-two-database-providers"></a><span data-ttu-id="c1d63-120">Избегайте настройки двух поставщиков баз данных</span><span class="sxs-lookup"><span data-stu-id="c1d63-120">Avoid configuring two database providers</span></span>

<span data-ttu-id="c1d63-121">В тестах вы собираетесь извне настроить контекст для использования поставщика памяти.</span><span class="sxs-lookup"><span data-stu-id="c1d63-121">In your tests you are going to externally configure the context to use the InMemory provider.</span></span> <span data-ttu-id="c1d63-122">При настройке поставщика базы данных путем переопределения `OnConfiguring` в контексте необходимо добавить некоторый условный код, чтобы убедиться, что поставщик базы данных настроен только в том случае, если он еще не настроен.</span><span class="sxs-lookup"><span data-stu-id="c1d63-122">If you are configuring a database provider by overriding `OnConfiguring` in your context, then you need to add some conditional code to ensure that you only configure the database provider if one has not already been configured.</span></span>

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BloggingContext.cs#OnConfiguring)]

> [!TIP]  
> <span data-ttu-id="c1d63-123">Если вы используете ASP.NET Core, этот код не нужен, так как поставщик базы данных уже настроен вне контекста (в Startup.cs).</span><span class="sxs-lookup"><span data-stu-id="c1d63-123">If you are using ASP.NET Core, then you should not need this code since your database provider is already configured outside of the context (in Startup.cs).</span></span>

### <a name="add-a-constructor-for-testing"></a><span data-ttu-id="c1d63-124">Добавление конструктора для тестирования</span><span class="sxs-lookup"><span data-stu-id="c1d63-124">Add a constructor for testing</span></span>

<span data-ttu-id="c1d63-125">Самый простой способ включить тестирование в другой базе данных — изменить контекст, чтобы предоставить конструктор, принимающий `DbContextOptions<TContext>`.</span><span class="sxs-lookup"><span data-stu-id="c1d63-125">The simplest way to enable testing against a different database is to modify your context to expose a constructor that accepts a `DbContextOptions<TContext>`.</span></span>

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BloggingContext.cs#Constructors)]

> [!TIP]  
> <span data-ttu-id="c1d63-126">`DbContextOptions<TContext>` сообщает контексту все параметры, такие как база данных для подключения.</span><span class="sxs-lookup"><span data-stu-id="c1d63-126">`DbContextOptions<TContext>` tells the context all of its settings, such as which database to connect to.</span></span> <span data-ttu-id="c1d63-127">Это тот же объект, который создается путем запуска метода onconfiguring в контексте.</span><span class="sxs-lookup"><span data-stu-id="c1d63-127">This is the same object that is built by running the OnConfiguring method in your context.</span></span>

## <a name="writing-tests"></a><span data-ttu-id="c1d63-128">Написание тестов</span><span class="sxs-lookup"><span data-stu-id="c1d63-128">Writing tests</span></span>

<span data-ttu-id="c1d63-129">Ключом к тестированию с помощью этого поставщика является возможность указать контексту использование поставщика памяти и управлять областью базы данных в памяти.</span><span class="sxs-lookup"><span data-stu-id="c1d63-129">The key to testing with this provider is the ability to tell the context to use the InMemory provider, and control the scope of the in-memory database.</span></span> <span data-ttu-id="c1d63-130">Обычно требуется чистая база данных для каждого метода теста.</span><span class="sxs-lookup"><span data-stu-id="c1d63-130">Typically you want a clean database for each test method.</span></span>

<span data-ttu-id="c1d63-131">Ниже приведен пример тестового класса, использующего базу данных в памяти.</span><span class="sxs-lookup"><span data-stu-id="c1d63-131">Here is an example of a test class that uses the InMemory database.</span></span> <span data-ttu-id="c1d63-132">Каждый метод теста задает уникальное имя базы данных, то есть каждый метод имеет собственную базу данных, используемую в памяти.</span><span class="sxs-lookup"><span data-stu-id="c1d63-132">Each test method specifies a unique database name, meaning each method has its own InMemory database.</span></span>

>[!TIP]
> <span data-ttu-id="c1d63-133">Чтобы использовать метод расширения `.UseInMemoryDatabase()`, укажите ссылку на пакет NuGet [Microsoft. EntityFrameworkCore. Memory](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.InMemory/).</span><span class="sxs-lookup"><span data-stu-id="c1d63-133">To use the `.UseInMemoryDatabase()` extension method, reference the NuGet package [Microsoft.EntityFrameworkCore.InMemory](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.InMemory/).</span></span>

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/TestProject/InMemory/BlogServiceTests.cs)]
