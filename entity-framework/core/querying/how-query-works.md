---
title: Как работают запросы — EF Core
description: Общие сведения о внутреннем механизме компиляции и выполнения запросов Entity Framework Core
author: ajcvickers
ms.date: 03/17/2020
uid: core/querying/how-query-works
ms.openlocfilehash: 7b3014cf64f8467ccbec10598ea1bb47304dfe43
ms.sourcegitcommit: f3512e3a98e685a3ba409c1d0157ce85cc390cf4
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2020
ms.locfileid: "94430473"
---
# <a name="how-queries-work"></a><span data-ttu-id="eeb35-103">Как работают запросы</span><span class="sxs-lookup"><span data-stu-id="eeb35-103">How Queries Work</span></span>

<span data-ttu-id="eeb35-104">Entity Framework Core использует LINQ для запроса данных из базы данных.</span><span class="sxs-lookup"><span data-stu-id="eeb35-104">Entity Framework Core uses Language Integrated Query (LINQ) to query data from the database.</span></span> <span data-ttu-id="eeb35-105">LINQ позволяет использовать C# (или предпочитаемый вами язык .NET) для написания строго типизированных запросов, основанных на производном контексте и классах сущностей.</span><span class="sxs-lookup"><span data-stu-id="eeb35-105">LINQ allows you to use C# (or your .NET language of choice) to write strongly typed queries based on your derived context and entity classes.</span></span>

> [!NOTE]
> <span data-ttu-id="eeb35-106">Эта статья устарела, и некоторые ее части необходимо обновить с учетом изменений, внесенных при создании конвейера запросов.</span><span class="sxs-lookup"><span data-stu-id="eeb35-106">This article is out of date and some parts of it needs to be updated to account for changes happened in design of query pipeline.</span></span> <span data-ttu-id="eeb35-107">Если у вас есть какие-либо сомнения о поведении, описанном здесь, [задайте вопрос](https://github.com/dotnet/efcore/issues/new/choose).</span><span class="sxs-lookup"><span data-stu-id="eeb35-107">If you have any doubts about any behavior mentioned here, please [ask a question](https://github.com/dotnet/efcore/issues/new/choose).</span></span>

## <a name="the-life-of-a-query"></a><span data-ttu-id="eeb35-108">Срок действия запроса</span><span class="sxs-lookup"><span data-stu-id="eeb35-108">The life of a query</span></span>

<span data-ttu-id="eeb35-109">Ниже приведено общее описание процесса, через который проходит каждый запрос.</span><span class="sxs-lookup"><span data-stu-id="eeb35-109">The following description is a high-level overview of the process each query goes through.</span></span>

1. <span data-ttu-id="eeb35-110">LINQ-запрос обрабатывается Entity Framework Core для создания представления, которое готово для обработки поставщиком данных.</span><span class="sxs-lookup"><span data-stu-id="eeb35-110">The LINQ query is processed by Entity Framework Core to build a representation that is ready to be processed by the database provider</span></span>
   1. <span data-ttu-id="eeb35-111">Результат кэшируется, чтобы не выполнять эту обработку каждый раз при выполнении запроса.</span><span class="sxs-lookup"><span data-stu-id="eeb35-111">The result is cached so that this processing does not need to be done every time the query is executed</span></span>
2. <span data-ttu-id="eeb35-112">Результат передается поставщику базы данных.</span><span class="sxs-lookup"><span data-stu-id="eeb35-112">The result is passed to the database provider</span></span>
   1. <span data-ttu-id="eeb35-113">Поставщик базы данных определяет, какие части запроса могут выполняться в базе данных.</span><span class="sxs-lookup"><span data-stu-id="eeb35-113">The database provider identifies which parts of the query can be evaluated in the database</span></span>
   2. <span data-ttu-id="eeb35-114">Эти части запроса преобразуются в язык запроса базы данных (например, SQL для реляционной базы данных).</span><span class="sxs-lookup"><span data-stu-id="eeb35-114">These parts of the query are translated to database-specific query language (for example, SQL for a relational database)</span></span>
   3. <span data-ttu-id="eeb35-115">Запрос отправляется в базу данных, и возвращается результирующий набор (результатами являются значения из базы данных, а не экземпляры сущностей)</span><span class="sxs-lookup"><span data-stu-id="eeb35-115">A query is sent to the database and the result set returned (results are values from the database, not entity instances)</span></span>
3. <span data-ttu-id="eeb35-116">Для каждого элемента в результирующем наборе выполняются следующие действия:</span><span class="sxs-lookup"><span data-stu-id="eeb35-116">For each item in the result set</span></span>
   1. <span data-ttu-id="eeb35-117">Если запрос является запросом отслеживания, EF проверяет, не предоставляют ли данные сущность, которая уже находится в объекте отслеживания изменений для экземпляра контекста.</span><span class="sxs-lookup"><span data-stu-id="eeb35-117">If the query is a tracking query, EF checks if the data represents an entity already in the change tracker for the context instance</span></span>
      * <span data-ttu-id="eeb35-118">Если да, возвращается имеющаяся сущность.</span><span class="sxs-lookup"><span data-stu-id="eeb35-118">If so, the existing entity is returned</span></span>
      * <span data-ttu-id="eeb35-119">Если нет, создается другая сущность, устанавливается объект отслеживания изменений и возвращается новая сущность.</span><span class="sxs-lookup"><span data-stu-id="eeb35-119">If not, a new entity is created, change tracking is set up, and the new entity is returned</span></span>
   2. <span data-ttu-id="eeb35-120">Если запрос не является отслеживаемым запросом, то всегда создается и возвращается новая сущность.</span><span class="sxs-lookup"><span data-stu-id="eeb35-120">If the query is a no-tracking query, then a new entity is always created and returned</span></span>

## <a name="when-queries-are-executed"></a><span data-ttu-id="eeb35-121">Когда выполняются запросы</span><span class="sxs-lookup"><span data-stu-id="eeb35-121">When queries are executed</span></span>

<span data-ttu-id="eeb35-122">При вызове операторов LINQ вы просто создаете представление запроса в памяти.</span><span class="sxs-lookup"><span data-stu-id="eeb35-122">When you call LINQ operators, you're simply building up an in-memory representation of the query.</span></span> <span data-ttu-id="eeb35-123">Запрос отправляется в базу данных только после обработки результатов.</span><span class="sxs-lookup"><span data-stu-id="eeb35-123">The query is only sent to the database when the results are consumed.</span></span>

<span data-ttu-id="eeb35-124">Ниже приведены наиболее распространенные операции, которые приводят к отправке запроса в базу данных.</span><span class="sxs-lookup"><span data-stu-id="eeb35-124">The most common operations that result in the query being sent to the database are:</span></span>

* <span data-ttu-id="eeb35-125">Итерация результатов в цикле `for`.</span><span class="sxs-lookup"><span data-stu-id="eeb35-125">Iterating the results in a `for` loop</span></span>
* <span data-ttu-id="eeb35-126">Использование операторов, таких как `ToList`, `ToArray`, `Single`, `Count`, или эквивалентных асинхронных перегрузок</span><span class="sxs-lookup"><span data-stu-id="eeb35-126">Using an operator such as `ToList`, `ToArray`, `Single`, `Count`, or the equivalent async overloads</span></span>

> [!WARNING]  
> <span data-ttu-id="eeb35-127">**Всегда проверяйте входные данные пользователя.** Так как платформа EF Core с помощью параметров и экранирования литералов в запросах обеспечивает защиту от атак путем внедрения кода SQL, она не проверяет входные данные.</span><span class="sxs-lookup"><span data-stu-id="eeb35-127">**Always validate user input:** While EF Core protects against SQL injection attacks by using parameters and escaping literals in queries, it does not validate inputs.</span></span> <span data-ttu-id="eeb35-128">Поэтому необходимо выполнять проверку в соответствии с требованиями отдельного приложения, прежде чем значения из непроверенных источников будут использоваться в LINQ-запросах, назначенные свойствам сущности или передаваемые другим API EF Core.</span><span class="sxs-lookup"><span data-stu-id="eeb35-128">Appropriate validation, per the application's requirements, should be performed before values from un-trusted sources are used in LINQ queries, assigned to entity properties, or passed to other EF Core APIs.</span></span> <span data-ttu-id="eeb35-129">Сюда входят все входные данные пользователя, используемые для динамического формирования запросов.</span><span class="sxs-lookup"><span data-stu-id="eeb35-129">This includes any user input used to dynamically construct queries.</span></span> <span data-ttu-id="eeb35-130">Даже при использовании LINQ, если вы принимаете входные данные пользователя для создания выражений, необходимо убедиться, что можно создать только предполагаемые выражения.</span><span class="sxs-lookup"><span data-stu-id="eeb35-130">Even when using LINQ, if you are accepting user input to build expressions, you need to make sure that only intended expressions can be constructed.</span></span>
