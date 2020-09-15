---
title: Как работают запросы — EF Core
description: Общие сведения о внутреннем механизме компиляции и выполнения запросов Entity Framework Core
author: ajcvickers
ms.date: 03/17/2020
ms.assetid: de2e34cd-659b-4cab-b5ed-7a979c6bf120
uid: core/querying/how-query-works
ms.openlocfilehash: e0e489f5f797b6f7d8f4860539a538dba90bd1c2
ms.sourcegitcommit: 7c3939504bb9da3f46bea3443638b808c04227c2
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/09/2020
ms.locfileid: "89616364"
---
# <a name="how-queries-work"></a><span data-ttu-id="5ed71-103">Как работают запросы</span><span class="sxs-lookup"><span data-stu-id="5ed71-103">How Queries Work</span></span>

<span data-ttu-id="5ed71-104">Entity Framework Core использует LINQ для запроса данных из базы данных.</span><span class="sxs-lookup"><span data-stu-id="5ed71-104">Entity Framework Core uses Language Integrated Query (LINQ) to query data from the database.</span></span> <span data-ttu-id="5ed71-105">LINQ позволяет использовать C# (или предпочитаемый вами язык .NET) для написания строго типизированных запросов, основанных на производном контексте и классах сущностей.</span><span class="sxs-lookup"><span data-stu-id="5ed71-105">LINQ allows you to use C# (or your .NET language of choice) to write strongly typed queries based on your derived context and entity classes.</span></span>

## <a name="the-life-of-a-query"></a><span data-ttu-id="5ed71-106">Срок действия запроса</span><span class="sxs-lookup"><span data-stu-id="5ed71-106">The life of a query</span></span>

<span data-ttu-id="5ed71-107">Ниже приведено общее описание процесса, через который проходит каждый запрос.</span><span class="sxs-lookup"><span data-stu-id="5ed71-107">The following description is a high-level overview of the process each query goes through.</span></span>

1. <span data-ttu-id="5ed71-108">LINQ-запрос обрабатывается Entity Framework Core для создания представления, которое готово для обработки поставщиком данных.</span><span class="sxs-lookup"><span data-stu-id="5ed71-108">The LINQ query is processed by Entity Framework Core to build a representation that is ready to be processed by the database provider</span></span>
   1. <span data-ttu-id="5ed71-109">Результат кэшируется, чтобы не выполнять эту обработку каждый раз при выполнении запроса.</span><span class="sxs-lookup"><span data-stu-id="5ed71-109">The result is cached so that this processing does not need to be done every time the query is executed</span></span>
2. <span data-ttu-id="5ed71-110">Результат передается поставщику базы данных.</span><span class="sxs-lookup"><span data-stu-id="5ed71-110">The result is passed to the database provider</span></span>
   1. <span data-ttu-id="5ed71-111">Поставщик базы данных определяет, какие части запроса могут выполняться в базе данных.</span><span class="sxs-lookup"><span data-stu-id="5ed71-111">The database provider identifies which parts of the query can be evaluated in the database</span></span>
   2. <span data-ttu-id="5ed71-112">Эти части запроса преобразуются в язык запроса базы данных (например, SQL для реляционной базы данных).</span><span class="sxs-lookup"><span data-stu-id="5ed71-112">These parts of the query are translated to database-specific query language (for example, SQL for a relational database)</span></span>
   3. <span data-ttu-id="5ed71-113">Запрос отправляется в базу данных, и возвращается результирующий набор (результатами являются значения из базы данных, а не экземпляры сущностей)</span><span class="sxs-lookup"><span data-stu-id="5ed71-113">A query is sent to the database and the result set returned (results are values from the database, not entity instances)</span></span>
3. <span data-ttu-id="5ed71-114">Для каждого элемента в результирующем наборе выполняются следующие действия:</span><span class="sxs-lookup"><span data-stu-id="5ed71-114">For each item in the result set</span></span>
   1. <span data-ttu-id="5ed71-115">Если запрос является запросом отслеживания, EF проверяет, не предоставляют ли данные сущность, которая уже находится в объекте отслеживания изменений для экземпляра контекста.</span><span class="sxs-lookup"><span data-stu-id="5ed71-115">If the query is a tracking query, EF checks if the data represents an entity already in the change tracker for the context instance</span></span>
      * <span data-ttu-id="5ed71-116">Если да, возвращается имеющаяся сущность.</span><span class="sxs-lookup"><span data-stu-id="5ed71-116">If so, the existing entity is returned</span></span>
      * <span data-ttu-id="5ed71-117">Если нет, создается другая сущность, устанавливается объект отслеживания изменений и возвращается новая сущность.</span><span class="sxs-lookup"><span data-stu-id="5ed71-117">If not, a new entity is created, change tracking is set up, and the new entity is returned</span></span>
   2. <span data-ttu-id="5ed71-118">Если запрос не является отслеживаемым запросом, то всегда создается и возвращается новая сущность.</span><span class="sxs-lookup"><span data-stu-id="5ed71-118">If the query is a no-tracking query, then a new entity is always created and returned</span></span>

## <a name="when-queries-are-executed"></a><span data-ttu-id="5ed71-119">Когда выполняются запросы</span><span class="sxs-lookup"><span data-stu-id="5ed71-119">When queries are executed</span></span>

<span data-ttu-id="5ed71-120">При вызове операторов LINQ вы просто создаете представление запроса в памяти.</span><span class="sxs-lookup"><span data-stu-id="5ed71-120">When you call LINQ operators, you're simply building up an in-memory representation of the query.</span></span> <span data-ttu-id="5ed71-121">Запрос отправляется в базу данных только после обработки результатов.</span><span class="sxs-lookup"><span data-stu-id="5ed71-121">The query is only sent to the database when the results are consumed.</span></span>

<span data-ttu-id="5ed71-122">Ниже приведены наиболее распространенные операции, которые приводят к отправке запроса в базу данных.</span><span class="sxs-lookup"><span data-stu-id="5ed71-122">The most common operations that result in the query being sent to the database are:</span></span>

* <span data-ttu-id="5ed71-123">Итерация результатов в цикле `for`.</span><span class="sxs-lookup"><span data-stu-id="5ed71-123">Iterating the results in a `for` loop</span></span>
* <span data-ttu-id="5ed71-124">Использование операторов, таких как `ToList`, `ToArray`, `Single`, `Count`, или эквивалентных асинхронных перегрузок</span><span class="sxs-lookup"><span data-stu-id="5ed71-124">Using an operator such as `ToList`, `ToArray`, `Single`, `Count`, or the equivalent async overloads</span></span>

> [!WARNING]  
> <span data-ttu-id="5ed71-125">**Всегда проверяйте входные данные пользователя.** Так как платформа EF Core с помощью параметров и экранирования литералов в запросах обеспечивает защиту от атак путем внедрения кода SQL, она не проверяет входные данные.</span><span class="sxs-lookup"><span data-stu-id="5ed71-125">**Always validate user input:** While EF Core protects against SQL injection attacks by using parameters and escaping literals in queries, it does not validate inputs.</span></span> <span data-ttu-id="5ed71-126">Поэтому необходимо выполнять проверку в соответствии с требованиями отдельного приложения, прежде чем значения из непроверенных источников будут использоваться в LINQ-запросах, назначенные свойствам сущности или передаваемые другим API EF Core.</span><span class="sxs-lookup"><span data-stu-id="5ed71-126">Appropriate validation, per the application's requirements, should be performed before values from un-trusted sources are used in LINQ queries, assigned to entity properties, or passed to other EF Core APIs.</span></span> <span data-ttu-id="5ed71-127">Сюда входят все входные данные пользователя, используемые для динамического формирования запросов.</span><span class="sxs-lookup"><span data-stu-id="5ed71-127">This includes any user input used to dynamically construct queries.</span></span> <span data-ttu-id="5ed71-128">Даже при использовании LINQ, если вы принимаете входные данные пользователя для создания выражений, необходимо убедиться, что можно создать только предполагаемые выражения.</span><span class="sxs-lookup"><span data-stu-id="5ed71-128">Even when using LINQ, if you are accepting user input to build expressions, you need to make sure that only intended expressions can be constructed.</span></span>
