---
title: Тестирование с помощью InMemory — EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 0d0590f1-1ea3-4d5c-8f44-db17395cd3f3
uid: core/miscellaneous/testing/in-memory
ms.openlocfilehash: 8aaea52f22954ef6a2b7d9b9c5627597c61ac644
ms.sourcegitcommit: 8f801993c9b8cd8a8fbfa7134818a8edca79e31a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/14/2019
ms.locfileid: "59562550"
---
# <a name="testing-with-inmemory"></a><span data-ttu-id="ef125-102">Тестирование с помощью InMemory</span><span class="sxs-lookup"><span data-stu-id="ef125-102">Testing with InMemory</span></span>

<span data-ttu-id="ef125-103">Поставщик InMemory полезно в тех случаях, когда вы хотите протестировать компоненты, используя средство, имитирующее подключение к реальной базе данных, без издержек на фактических операций базы данных.</span><span class="sxs-lookup"><span data-stu-id="ef125-103">The InMemory provider is useful when you want to test components using something that approximates connecting to the real database, without the overhead of actual database operations.</span></span>

> [!TIP]  
> <span data-ttu-id="ef125-104">Для этой статьи вы можете скачать [пример](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Testing) из репозитория GitHub.</span><span class="sxs-lookup"><span data-stu-id="ef125-104">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Testing) on GitHub.</span></span>

## <a name="inmemory-is-not-a-relational-database"></a><span data-ttu-id="ef125-105">InMemory не является реляционной базы данных</span><span class="sxs-lookup"><span data-stu-id="ef125-105">InMemory is not a relational database</span></span>

<span data-ttu-id="ef125-106">Поставщики базы данных EF Core не быть реляционных баз данных.</span><span class="sxs-lookup"><span data-stu-id="ef125-106">EF Core database providers do not have to be relational databases.</span></span> <span data-ttu-id="ef125-107">InMemory предназначен для базы данных общего назначения для тестирования и не предназначен для имитации реляционной базы данных.</span><span class="sxs-lookup"><span data-stu-id="ef125-107">InMemory is designed to be a general purpose database for testing, and is not designed to mimic a relational database.</span></span>

<span data-ttu-id="ef125-108">Некоторые примеры включают:</span><span class="sxs-lookup"><span data-stu-id="ef125-108">Some examples of this include:</span></span>

* <span data-ttu-id="ef125-109">InMemory позволит вам сохранить данные, которые нарушают ограничения ссылочной целостности в реляционной базе данных.</span><span class="sxs-lookup"><span data-stu-id="ef125-109">InMemory will allow you to save data that would violate referential integrity constraints in a relational database.</span></span>
* <span data-ttu-id="ef125-110">Если вы используете DefaultValueSql(string) для свойства в модели, это — это реляционная база данных API и не имеет смысла при работе с InMemory.</span><span class="sxs-lookup"><span data-stu-id="ef125-110">If you use DefaultValueSql(string) for a property in your model, this is a relational database API and will have no effect when running against InMemory.</span></span>
* <span data-ttu-id="ef125-111">[Параллелизм с помощью метки времени и строк версии](xref:core/modeling/concurrency#timestamprow-version) (`[Timestamp]` или `IsRowVersion`) не поддерживается.</span><span class="sxs-lookup"><span data-stu-id="ef125-111">[Concurrency via Timestamp/row version](xref:core/modeling/concurrency#timestamprow-version) (`[Timestamp]` or `IsRowVersion`) is not supported.</span></span> <span data-ttu-id="ef125-112">Не [DbUpdateConcurrencyException](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.dbupdateconcurrencyexception) возникает, если обновление выполняется с помощью старого маркера параллелизма.</span><span class="sxs-lookup"><span data-stu-id="ef125-112">No [DbUpdateConcurrencyException](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.dbupdateconcurrencyexception) will be thrown if an update is done using an old concurrency token.</span></span>

> [!TIP]  
> <span data-ttu-id="ef125-113">Для многих тестовых целей эти различия будут имеет значения.</span><span class="sxs-lookup"><span data-stu-id="ef125-113">For many test purposes these differences will not matter.</span></span> <span data-ttu-id="ef125-114">Тем не менее, если вы хотите проверить то, что поведение во многом напоминает true реляционной базы данных, рассмотрите возможность использования [режиме in-memory SQLite](sqlite.md).</span><span class="sxs-lookup"><span data-stu-id="ef125-114">However, if you want to test against something that behaves more like a true relational database, then consider using [SQLite in-memory mode](sqlite.md).</span></span>

## <a name="example-testing-scenario"></a><span data-ttu-id="ef125-115">Пример сценария тестирования</span><span class="sxs-lookup"><span data-stu-id="ef125-115">Example testing scenario</span></span>

<span data-ttu-id="ef125-116">Предположим, что благодаря которой код приложения для выполнения некоторых операций, связанных с блоги.</span><span class="sxs-lookup"><span data-stu-id="ef125-116">Consider the following service that allows application code to perform some operations related to blogs.</span></span> <span data-ttu-id="ef125-117">Внутри она использует `DbContext` , соединяется с базой данных SQL Server.</span><span class="sxs-lookup"><span data-stu-id="ef125-117">Internally it uses a `DbContext` that connects to a SQL Server database.</span></span> <span data-ttu-id="ef125-118">Оно полезно для обмена данном контексте для подключения к базе данных InMemory, таким образом, мы может создавать эффективные тесты для этой службы без необходимости изменять код или выполнить большой объем работы по созданию теста double контекста.</span><span class="sxs-lookup"><span data-stu-id="ef125-118">It would be useful to swap this context to connect to an InMemory database so that we can write efficient tests for this service without having to modify the code, or do a lot of work to create a test double of the context.</span></span>

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BlogService.cs)]

## <a name="get-your-context-ready"></a><span data-ttu-id="ef125-119">Подготовка к контексту</span><span class="sxs-lookup"><span data-stu-id="ef125-119">Get your context ready</span></span>

### <a name="avoid-configuring-two-database-providers"></a><span data-ttu-id="ef125-120">Не настраивать два поставщика базы данных</span><span class="sxs-lookup"><span data-stu-id="ef125-120">Avoid configuring two database providers</span></span>

<span data-ttu-id="ef125-121">В тестах вы собираетесь извне настроить контекст, используемый поставщик InMemory.</span><span class="sxs-lookup"><span data-stu-id="ef125-121">In your tests you are going to externally configure the context to use the InMemory provider.</span></span> <span data-ttu-id="ef125-122">При настройке поставщика базы данных путем переопределения `OnConfiguring` в контексте, затем необходимо добавить некоторые условный код, чтобы убедиться, только настроить поставщик базы данных, если уже не был настроен.</span><span class="sxs-lookup"><span data-stu-id="ef125-122">If you are configuring a database provider by overriding `OnConfiguring` in your context, then you need to add some conditional code to ensure that you only configure the database provider if one has not already been configured.</span></span>

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BloggingContext.cs#OnConfiguring)]

> [!TIP]  
> <span data-ttu-id="ef125-123">Если вы используете ASP.NET Core, то нет необходимости этот код так как поставщик базы данных уже настроена вне контекста (в файле Startup.cs).</span><span class="sxs-lookup"><span data-stu-id="ef125-123">If you are using ASP.NET Core, then you should not need this code since your database provider is already configured outside of the context (in Startup.cs).</span></span>

### <a name="add-a-constructor-for-testing"></a><span data-ttu-id="ef125-124">Добавьте конструктор для тестирования</span><span class="sxs-lookup"><span data-stu-id="ef125-124">Add a constructor for testing</span></span>

<span data-ttu-id="ef125-125">Чтобы обеспечить возможность тестирования в другой базе данных проще всего изменить текущий контекст предоставляют конструктор, принимающий `DbContextOptions<TContext>`.</span><span class="sxs-lookup"><span data-stu-id="ef125-125">The simplest way to enable testing against a different database is to modify your context to expose a constructor that accepts a `DbContextOptions<TContext>`.</span></span>

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BloggingContext.cs#Constructors)]

> [!TIP]  
> <span data-ttu-id="ef125-126">`DbContextOptions<TContext>` Указывает контексту на все его параметры, например базу данных для подключения к.</span><span class="sxs-lookup"><span data-stu-id="ef125-126">`DbContextOptions<TContext>` tells the context all of its settings, such as which database to connect to.</span></span> <span data-ttu-id="ef125-127">Это тот же объект, в которой создано, выполнив метод OnConfiguring в вашем контексте.</span><span class="sxs-lookup"><span data-stu-id="ef125-127">This is the same object that is built by running the OnConfiguring method in your context.</span></span>

## <a name="writing-tests"></a><span data-ttu-id="ef125-128">Написание тестов</span><span class="sxs-lookup"><span data-stu-id="ef125-128">Writing tests</span></span>

<span data-ttu-id="ef125-129">Ключ для тестирования с этим поставщиком является возможность определить контекст для использования поставщика InMemory и управлять областью базы данных в памяти.</span><span class="sxs-lookup"><span data-stu-id="ef125-129">The key to testing with this provider is the ability to tell the context to use the InMemory provider, and control the scope of the in-memory database.</span></span> <span data-ttu-id="ef125-130">Обычно требуется очистить базу данных для каждого метода теста.</span><span class="sxs-lookup"><span data-stu-id="ef125-130">Typically you want a clean database for each test method.</span></span>

<span data-ttu-id="ef125-131">Ниже приведен пример тестового класса, использующего базу данных InMemory.</span><span class="sxs-lookup"><span data-stu-id="ef125-131">Here is an example of a test class that uses the InMemory database.</span></span> <span data-ttu-id="ef125-132">Каждый метод теста указывает уникальное имя базы данных, это означает, что каждый метод имеет свою собственную базу данных InMemory.</span><span class="sxs-lookup"><span data-stu-id="ef125-132">Each test method specifies a unique database name, meaning each method has its own InMemory database.</span></span>

>[!TIP]
> <span data-ttu-id="ef125-133">Чтобы использовать `.UseInMemoryDatabase()` метод расширения, ссылка на пакет NuGet [Microsoft.EntityFrameworkCore.InMemory](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.InMemory/).</span><span class="sxs-lookup"><span data-stu-id="ef125-133">To use the `.UseInMemoryDatabase()` extension method, reference the NuGet package [Microsoft.EntityFrameworkCore.InMemory](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.InMemory/).</span></span>

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/TestProject/InMemory/BlogServiceTests.cs)]
