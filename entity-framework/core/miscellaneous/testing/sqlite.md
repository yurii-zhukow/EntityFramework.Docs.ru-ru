---
title: Тестирование с помощью SQLite — EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 7a2b75e2-1875-4487-9877-feff0651b5a6
uid: core/miscellaneous/testing/sqlite
ms.openlocfilehash: bc9d6768a90ce17160c4126d2a68fddaa30d63de
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/27/2018
ms.locfileid: "42996872"
---
# <a name="testing-with-sqlite"></a><span data-ttu-id="7c3d8-102">Тестирование с помощью SQLite</span><span class="sxs-lookup"><span data-stu-id="7c3d8-102">Testing with SQLite</span></span>

<span data-ttu-id="7c3d8-103">SQLite имеет режим в памяти, можно использовать для написания тестов с реляционной базой данных, без издержек на фактических операций базы данных SQLite.</span><span class="sxs-lookup"><span data-stu-id="7c3d8-103">SQLite has an in-memory mode that allows you to use SQLite to write tests against a relational database, without the overhead of actual database operations.</span></span>

> [!TIP]  
> <span data-ttu-id="7c3d8-104">Можно просмотреть в этой статье [пример](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Testing) на GitHub</span><span class="sxs-lookup"><span data-stu-id="7c3d8-104">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Testing) on GitHub</span></span>

## <a name="example-testing-scenario"></a><span data-ttu-id="7c3d8-105">Пример сценария тестирования</span><span class="sxs-lookup"><span data-stu-id="7c3d8-105">Example testing scenario</span></span>

<span data-ttu-id="7c3d8-106">Предположим, что благодаря которой код приложения для выполнения некоторых операций, связанных с блоги.</span><span class="sxs-lookup"><span data-stu-id="7c3d8-106">Consider the following service that allows application code to perform some operations related to blogs.</span></span> <span data-ttu-id="7c3d8-107">Внутри она использует `DbContext` , соединяется с базой данных SQL Server.</span><span class="sxs-lookup"><span data-stu-id="7c3d8-107">Internally it uses a `DbContext` that connects to a SQL Server database.</span></span> <span data-ttu-id="7c3d8-108">Оно полезно для обмена данном контексте для подключения к базе данных SQLite в памяти, таким образом, мы может создавать эффективные тесты для этой службы без необходимости изменять код или выполнить большой объем работы по созданию теста double контекста.</span><span class="sxs-lookup"><span data-stu-id="7c3d8-108">It would be useful to swap this context to connect to an in-memory SQLite database so that we can write efficient tests for this service without having to modify the code, or do a lot of work to create a test double of the context.</span></span>

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BlogService.cs)]

## <a name="get-your-context-ready"></a><span data-ttu-id="7c3d8-109">Подготовка к контексту</span><span class="sxs-lookup"><span data-stu-id="7c3d8-109">Get your context ready</span></span>

### <a name="avoid-configuring-two-database-providers"></a><span data-ttu-id="7c3d8-110">Не настраивать два поставщика базы данных</span><span class="sxs-lookup"><span data-stu-id="7c3d8-110">Avoid configuring two database providers</span></span>

<span data-ttu-id="7c3d8-111">В тестах вы собираетесь извне настроить контекст, используемый поставщик InMemory.</span><span class="sxs-lookup"><span data-stu-id="7c3d8-111">In your tests you are going to externally configure the context to use the InMemory provider.</span></span> <span data-ttu-id="7c3d8-112">При настройке поставщика базы данных путем переопределения `OnConfiguring` в контексте, затем необходимо добавить некоторые условный код, чтобы убедиться, только настроить поставщик базы данных, если уже не был настроен.</span><span class="sxs-lookup"><span data-stu-id="7c3d8-112">If you are configuring a database provider by overriding `OnConfiguring` in your context, then you need to add some conditional code to ensure that you only configure the database provider if one has not already been configured.</span></span>

> [!TIP]  
> <span data-ttu-id="7c3d8-113">Если вы используете ASP.NET Core, то нет необходимости этот код с момента настройки поставщика базы данных вне контекста (в файле Startup.cs).</span><span class="sxs-lookup"><span data-stu-id="7c3d8-113">If you are using ASP.NET Core, then you should not need this code since your database provider is configured outside of the context (in Startup.cs).</span></span>

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BloggingContext.cs#OnConfiguring)]

### <a name="add-a-constructor-for-testing"></a><span data-ttu-id="7c3d8-114">Добавьте конструктор для тестирования</span><span class="sxs-lookup"><span data-stu-id="7c3d8-114">Add a constructor for testing</span></span>

<span data-ttu-id="7c3d8-115">Чтобы обеспечить возможность тестирования в другой базе данных проще всего изменить текущий контекст предоставляют конструктор, принимающий `DbContextOptions<TContext>`.</span><span class="sxs-lookup"><span data-stu-id="7c3d8-115">The simplest way to enable testing against a different database is to modify your context to expose a constructor that accepts a `DbContextOptions<TContext>`.</span></span>

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BloggingContext.cs#Constructors)]

> [!TIP]  
> <span data-ttu-id="7c3d8-116">`DbContextOptions<TContext>` Указывает контексту на все его параметры, например базу данных для подключения к.</span><span class="sxs-lookup"><span data-stu-id="7c3d8-116">`DbContextOptions<TContext>` tells the context all of its settings, such as which database to connect to.</span></span> <span data-ttu-id="7c3d8-117">Это тот же объект, в которой создано, выполнив метод OnConfiguring в вашем контексте.</span><span class="sxs-lookup"><span data-stu-id="7c3d8-117">This is the same object that is built by running the OnConfiguring method in your context.</span></span>

## <a name="writing-tests"></a><span data-ttu-id="7c3d8-118">Написание тестов</span><span class="sxs-lookup"><span data-stu-id="7c3d8-118">Writing tests</span></span>

<span data-ttu-id="7c3d8-119">Ключ для тестирования с этим поставщиком является возможность определить контекст SQLite и управлять областью базы данных в памяти.</span><span class="sxs-lookup"><span data-stu-id="7c3d8-119">The key to testing with this provider is the ability to tell the context to use SQLite, and control the scope of the in-memory database.</span></span> <span data-ttu-id="7c3d8-120">Область базы данных управляется Открытие и закрытие соединения.</span><span class="sxs-lookup"><span data-stu-id="7c3d8-120">The scope of the database is controlled by opening and closing the connection.</span></span> <span data-ttu-id="7c3d8-121">Базы данных, ограничиваются длительность, при открытом соединении.</span><span class="sxs-lookup"><span data-stu-id="7c3d8-121">The database is scoped to the duration that the connection is open.</span></span> <span data-ttu-id="7c3d8-122">Обычно требуется очистить базу данных для каждого метода теста.</span><span class="sxs-lookup"><span data-stu-id="7c3d8-122">Typically you want a clean database for each test method.</span></span>

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/TestProject/SQLite/BlogServiceTests.cs)]
