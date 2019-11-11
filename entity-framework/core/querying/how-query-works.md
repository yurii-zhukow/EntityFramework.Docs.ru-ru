---
title: Как работают запросы — EF Core
author: rowanmiller
ms.date: 09/26/2018
ms.assetid: de2e34cd-659b-4cab-b5ed-7a979c6bf120
uid: core/querying/how-query-works
ms.openlocfilehash: ba0d68469530e6272ffbb51946d7856122a261c7
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/06/2019
ms.locfileid: "73656241"
---
# <a name="how-queries-work"></a><span data-ttu-id="d99c8-102">Как работают запросы</span><span class="sxs-lookup"><span data-stu-id="d99c8-102">How Queries Work</span></span>

<span data-ttu-id="d99c8-103">Entity Framework Core использует LINQ для запроса данных из базы данных.</span><span class="sxs-lookup"><span data-stu-id="d99c8-103">Entity Framework Core uses Language Integrated Query (LINQ) to query data from the database.</span></span> <span data-ttu-id="d99c8-104">LINQ позволяет использовать C# (или предпочитаемый вами язык .NET) для написания строго типизированных запросов, основанных на производном контексте и классах сущностей.</span><span class="sxs-lookup"><span data-stu-id="d99c8-104">LINQ allows you to use C# (or your .NET language of choice) to write strongly typed queries based on your derived context and entity classes.</span></span>

## <a name="the-life-of-a-query"></a><span data-ttu-id="d99c8-105">Срок действия запроса</span><span class="sxs-lookup"><span data-stu-id="d99c8-105">The life of a query</span></span>

<span data-ttu-id="d99c8-106">Ниже приведен общий обзор процесса, через который проходит каждый запрос.</span><span class="sxs-lookup"><span data-stu-id="d99c8-106">The following is a high level overview of the process each query goes through.</span></span>

1. <span data-ttu-id="d99c8-107">LINQ-запрос обрабатывается Entity Framework Core для создания представления, которое готово для обработки поставщиком данных.</span><span class="sxs-lookup"><span data-stu-id="d99c8-107">The LINQ query is processed by Entity Framework Core to build a representation that is ready to be processed by the database provider</span></span>
   1. <span data-ttu-id="d99c8-108">Результат кэшируется, чтобы не выполнять эту обработку каждый раз при выполнении запроса.</span><span class="sxs-lookup"><span data-stu-id="d99c8-108">The result is cached so that this processing does not need to be done every time the query is executed</span></span>
2. <span data-ttu-id="d99c8-109">Результат передается поставщику базы данных.</span><span class="sxs-lookup"><span data-stu-id="d99c8-109">The result is passed to the database provider</span></span>
   1. <span data-ttu-id="d99c8-110">Поставщик базы данных определяет, какие части запроса могут выполняться в базе данных.</span><span class="sxs-lookup"><span data-stu-id="d99c8-110">The database provider identifies which parts of the query can be evaluated in the database</span></span>
   2. <span data-ttu-id="d99c8-111">Эти части запроса преобразуются в язык запроса базы данных (например, SQL для реляционной базы данных).</span><span class="sxs-lookup"><span data-stu-id="d99c8-111">These parts of the query are translated to database specific query language (for example, SQL for a relational database)</span></span>
   3. <span data-ttu-id="d99c8-112">Один или несколько запросов отправляются в базу данных, и возвращается результирующий набор (результатами являются значения из базы данных, а не экземпляры сущностей).</span><span class="sxs-lookup"><span data-stu-id="d99c8-112">One or more queries are sent to the database and the result set returned (results are values from the database, not entity instances)</span></span>
3. <span data-ttu-id="d99c8-113">Для каждого элемента в результирующем наборе выполняются следующие действия:</span><span class="sxs-lookup"><span data-stu-id="d99c8-113">For each item in the result set</span></span>
   1. <span data-ttu-id="d99c8-114">Если это запрос отслеживания, EF проверяет, не предоставляют ли данные сущность, которая уже находится в объекте отслеживания изменений для экземпляра контекста.</span><span class="sxs-lookup"><span data-stu-id="d99c8-114">If this is a tracking query, EF checks if the data represents an entity already in the change tracker for the context instance</span></span>
      * <span data-ttu-id="d99c8-115">Если да, возвращается имеющаяся сущность.</span><span class="sxs-lookup"><span data-stu-id="d99c8-115">If so, the existing entity is returned</span></span>
      * <span data-ttu-id="d99c8-116">Если нет, создается другая сущность, устанавливается объект отслеживания изменений и возвращается новая сущность.</span><span class="sxs-lookup"><span data-stu-id="d99c8-116">If not, a new entity is created, change tracking is setup, and the new entity is returned</span></span>
   2. <span data-ttu-id="d99c8-117">Если это не запрос отслеживания, EF проверяет, не предоставляют ли данные сущность, которая уже находится в результирующем наборе для этого запроса.</span><span class="sxs-lookup"><span data-stu-id="d99c8-117">If this is a no-tracking query, EF checks if the data represents an entity already in the result set for this query</span></span>
      * <span data-ttu-id="d99c8-118">Если да, возвращается имеющаяся сущность<sup>(1)</sup>.</span><span class="sxs-lookup"><span data-stu-id="d99c8-118">If so, the existing entity is returned <sup>(1)</sup></span></span>
      * <span data-ttu-id="d99c8-119">Если нет, создается и возвращается другая сущность.</span><span class="sxs-lookup"><span data-stu-id="d99c8-119">If not, a new entity is created and returned</span></span>

<span data-ttu-id="d99c8-120"><sup>(1) </sup> В запросах, отличных от запросов отслеживания, используются слабые ссылки для отслеживания сущностей, которые уже были возвращены.</span><span class="sxs-lookup"><span data-stu-id="d99c8-120"><sup>(1)</sup> No-tracking queries use weak references to keep track of entities that have already been returned.</span></span> <span data-ttu-id="d99c8-121">Если предыдущий результат с тем же идентификатором выходит за пределы области, а сборка мусора выполняется, вы можете получить новый экземпляр сущности.</span><span class="sxs-lookup"><span data-stu-id="d99c8-121">If a previous result with the same identity goes out of scope, and garbage collection runs, you may get a new entity instance.</span></span>

## <a name="when-queries-are-executed"></a><span data-ttu-id="d99c8-122">Когда выполняются запросы</span><span class="sxs-lookup"><span data-stu-id="d99c8-122">When queries are executed</span></span>

<span data-ttu-id="d99c8-123">При вызове операторов LINQ вы просто создаете представление запроса в памяти.</span><span class="sxs-lookup"><span data-stu-id="d99c8-123">When you call LINQ operators, you are simply building up an in-memory representation of the query.</span></span> <span data-ttu-id="d99c8-124">Запрос отправляется в базу данных только после обработки результатов.</span><span class="sxs-lookup"><span data-stu-id="d99c8-124">The query is only sent to the database when the results are consumed.</span></span>

<span data-ttu-id="d99c8-125">Ниже приведены наиболее распространенные операции, которые приводят к отправке запроса в базу данных.</span><span class="sxs-lookup"><span data-stu-id="d99c8-125">The most common operations that result in the query being sent to the database are:</span></span>

* <span data-ttu-id="d99c8-126">Итерация результатов в цикле `for`.</span><span class="sxs-lookup"><span data-stu-id="d99c8-126">Iterating the results in a `for` loop</span></span>
* <span data-ttu-id="d99c8-127">Использование оператора, например `ToList`, `ToArray`, `Single`, `Count`.</span><span class="sxs-lookup"><span data-stu-id="d99c8-127">Using an operator such as `ToList`, `ToArray`, `Single`, `Count`</span></span>
* <span data-ttu-id="d99c8-128">Привязка данных результатов запроса к пользовательскому интерфейсу.</span><span class="sxs-lookup"><span data-stu-id="d99c8-128">Databinding the results of a query to a UI</span></span>

> [!WARNING]  
> <span data-ttu-id="d99c8-129">**Всегда проверяйте входные данные пользователя**. Так как платформа EF Core обеспечивает защиту от атак путем внедрения кода SQL с помощью параметров и экранирования литералов в запросах, она не проверяет входные данные.</span><span class="sxs-lookup"><span data-stu-id="d99c8-129">**Always validate user input:** While EF Core protects against SQL injection attacks by using parameters and escaping literals in queries, it does not validate inputs.</span></span> <span data-ttu-id="d99c8-130">Поэтому необходимо выполнять проверку в соответствии с требованиями отдельного приложения, прежде чем значения из непроверенного источника будут использоваться в LINQ-запросах, назначенные свойствам сущности или передаваемые другим API EF Core.</span><span class="sxs-lookup"><span data-stu-id="d99c8-130">Appropriate validation, per the application's requirements, should be performed before values from untrusted sources are used in LINQ queries, assigned to entity properties, or passed to other EF Core APIs.</span></span> <span data-ttu-id="d99c8-131">Сюда входят все входные данные пользователя, используемые для динамического формирования запросов.</span><span class="sxs-lookup"><span data-stu-id="d99c8-131">This includes any user input used to dynamically construct queries.</span></span> <span data-ttu-id="d99c8-132">Даже при использовании LINQ, если вы принимаете входные данные пользователя для создания выражений, необходимо убедиться, что можно создать только предполагаемые выражения.</span><span class="sxs-lookup"><span data-stu-id="d99c8-132">Even when using LINQ, if you are accepting user input to build expressions, you need to make sure that only intended expressions can be constructed.</span></span>
