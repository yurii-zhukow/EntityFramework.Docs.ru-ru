---
title: "Каким образом запросы трудозатраты - EF Core"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: de2e34cd-659b-4cab-b5ed-7a979c6bf120
ms.technology: entity-framework-core
uid: core/querying/overview
ms.openlocfilehash: 7fd2940d559f82016d7a8fc3fdcf3af0d5b8bc8f
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/27/2017
---
# <a name="how-queries-work"></a><span data-ttu-id="8169f-102">Как работают запросы</span><span class="sxs-lookup"><span data-stu-id="8169f-102">How Queries Work</span></span>

<span data-ttu-id="8169f-103">Entity Framework Core использует интегрировать запросы языка (LINQ) для запроса данных из базы данных.</span><span class="sxs-lookup"><span data-stu-id="8169f-103">Entity Framework Core uses Language Integrate Query (LINQ) to query data from the database.</span></span> <span data-ttu-id="8169f-104">LINQ позволяет использовать C# (или выбранный язык .NET) для записи строго типизированные запросы, основанные на производных классах, контекст и сущности.</span><span class="sxs-lookup"><span data-stu-id="8169f-104">LINQ allows you to use C# (or your .NET language of choice) to write strongly typed queries based on your derived context and entity classes.</span></span>

## <a name="the-life-of-a-query"></a><span data-ttu-id="8169f-105">Срока действия запроса</span><span class="sxs-lookup"><span data-stu-id="8169f-105">The life of a query</span></span>

<span data-ttu-id="8169f-106">Ниже приведен общий обзор процесса, который проходит каждого запроса.</span><span class="sxs-lookup"><span data-stu-id="8169f-106">The following is a high level overview of the process each query goes through.</span></span>

1. <span data-ttu-id="8169f-107">Запрос LINQ, обрабатывается Entity Framework Core для построения в представление, готов к обработке поставщиком базы данных</span><span class="sxs-lookup"><span data-stu-id="8169f-107">The LINQ query is processed by Entity Framework Core to build a representation that is ready to be processed by the database provider</span></span>
   1. <span data-ttu-id="8169f-108">Результат в кэш, чтобы эта обработка не требуется выполнять при каждом выполнении запроса</span><span class="sxs-lookup"><span data-stu-id="8169f-108">The result is cached so that this processing does not need to be done every time the query is executed</span></span>
2. <span data-ttu-id="8169f-109">Результат передается поставщику базы данных</span><span class="sxs-lookup"><span data-stu-id="8169f-109">The result is passed to the database provider</span></span>
   1. <span data-ttu-id="8169f-110">Поставщик баз данных определяет, какие части запроса может вычисляться в базе данных</span><span class="sxs-lookup"><span data-stu-id="8169f-110">The database provider identifies which parts of the query can be evaluated in the database</span></span>
   2. <span data-ttu-id="8169f-111">Эти части запроса преобразуются в язык запроса базы данных (например SQL для реляционной базы данных)</span><span class="sxs-lookup"><span data-stu-id="8169f-111">These parts of the query are translated to database specific query language (e.g. SQL for a relational database)</span></span>
   3. <span data-ttu-id="8169f-112">Один или несколько запросы отправляются в базу данных и результирующий набор (результатов являются значениями из базы данных, не экземпляров сущностей)</span><span class="sxs-lookup"><span data-stu-id="8169f-112">One or more queries are sent to the database and the result set returned (results are values from the database, not entity instances)</span></span>
3. <span data-ttu-id="8169f-113">Для каждого элемента в результирующем наборе</span><span class="sxs-lookup"><span data-stu-id="8169f-113">For each item in the result set</span></span>
   1. <span data-ttu-id="8169f-114">Если это запрос отслеживания EF проверяет, если данные представляют сущности уже в объекте отслеживания изменений для экземпляра контекста</span><span class="sxs-lookup"><span data-stu-id="8169f-114">If this is a tracking query, EF checks if the data represents an entity already in the change tracker for the context instance</span></span>
      * <span data-ttu-id="8169f-115">В этом случае возвращается существующей сущности</span><span class="sxs-lookup"><span data-stu-id="8169f-115">If so, the existing entity is returned</span></span>
      * <span data-ttu-id="8169f-116">В противном случае создается новая сущность, отслеживание изменений, установки и возвращается новая сущность</span><span class="sxs-lookup"><span data-stu-id="8169f-116">If not, a new entity is created, change tracking is setup, and the new entity is returned</span></span>
   2. <span data-ttu-id="8169f-117">Если это запрос нет отслеживания EF проверяет, если данных представляет собой сущность, уже в результирующем наборе для этого запроса</span><span class="sxs-lookup"><span data-stu-id="8169f-117">If this is a no-tracking query, EF checks if the data represents an entity already in the result set for this query</span></span>
      * <span data-ttu-id="8169f-118">Если Да, возвращается существующей сущности <sup>(1)</sup></span><span class="sxs-lookup"><span data-stu-id="8169f-118">If so, the existing entity is returned <sup>(1)</sup></span></span>
      * <span data-ttu-id="8169f-119">В противном случае создается и возвращается новая сущность</span><span class="sxs-lookup"><span data-stu-id="8169f-119">If not, a new entity is created and returned</span></span>

<span data-ttu-id="8169f-120"><sup>(1) </sup> Отсутствуют запросы отслеживания использования слабых ссылок для отслеживания объектов, которые уже были возвращены.</span><span class="sxs-lookup"><span data-stu-id="8169f-120"><sup>(1)</sup> No tracking queries use weak references to keep track of entities that have already been returned.</span></span> <span data-ttu-id="8169f-121">Если предыдущий результат с тем же удостоверением выходит за пределы области, а сборка мусора выполняется, может получить новый экземпляр сущности.</span><span class="sxs-lookup"><span data-stu-id="8169f-121">If a previous result with the same identity goes out of scope, and garbage collection runs, you may get a new entity instance.</span></span>

## <a name="when-queries-are-executed"></a><span data-ttu-id="8169f-122">При выполнении запросов</span><span class="sxs-lookup"><span data-stu-id="8169f-122">When queries are executed</span></span>

<span data-ttu-id="8169f-123">При вызове операторы LINQ, вы просто строите в памяти представление запроса.</span><span class="sxs-lookup"><span data-stu-id="8169f-123">When you call LINQ operators, you are simply building up an in-memory representation of the query.</span></span> <span data-ttu-id="8169f-124">Запрос отправляется в базу данных только после обработки результатов.</span><span class="sxs-lookup"><span data-stu-id="8169f-124">The query is only sent to the database when the results are consumed.</span></span>

<span data-ttu-id="8169f-125">Ниже приведены наиболее распространенных операций, которые приводят в запросе, отправляемых в базе данных.</span><span class="sxs-lookup"><span data-stu-id="8169f-125">The most common operations that result in the query being sent to the database are:</span></span>
* <span data-ttu-id="8169f-126">Итерация результаты в `for` цикла</span><span class="sxs-lookup"><span data-stu-id="8169f-126">Iterating the results in a `for` loop</span></span>
* <span data-ttu-id="8169f-127">С помощью оператора, например `ToList`, `ToArray`, `Single`,`Count`</span><span class="sxs-lookup"><span data-stu-id="8169f-127">Using an operator such as `ToList`, `ToArray`, `Single`, `Count`</span></span>
* <span data-ttu-id="8169f-128">Привязка данных результаты запроса к пользовательскому Интерфейсу</span><span class="sxs-lookup"><span data-stu-id="8169f-128">Databinding the results of a query to a UI</span></span>

> [!WARNING]  
> <span data-ttu-id="8169f-129">**Всегда проверять вводимые:** пока EF обеспечивают защиту от атак путем внедрения кода SQL, он не включает общие проверку входных данных.</span><span class="sxs-lookup"><span data-stu-id="8169f-129">**Always validate user input:** While EF does provide protection from SQL injection attacks, it does not do any general validation of input.</span></span> <span data-ttu-id="8169f-130">Поэтому если значения, передаваемые в API-интерфейсы, используемые в запросах LINQ, назначенные свойства сущности, т. д., берутся из ненадежного источника, а затем соответствующую проверку, в соответствии с требованиями вашего приложения, необходимо выполнить.</span><span class="sxs-lookup"><span data-stu-id="8169f-130">Therefore if values being passed to APIs, used in LINQ queries, assigned to entity properties, etc., come from an untrusted source then appropriate validation, per your application requirements, should be performed.</span></span> <span data-ttu-id="8169f-131">Сюда входят вводимых пользователем, используемый для динамического построения запросов.</span><span class="sxs-lookup"><span data-stu-id="8169f-131">This includes any user input used to dynamically construct queries.</span></span> <span data-ttu-id="8169f-132">Даже при использовании LINQ, если вы принимаете ввода данных пользователя, для построения выражений, необходимые для убедитесь в том, чем только нужные выражения может быть создан.</span><span class="sxs-lookup"><span data-stu-id="8169f-132">Even when using LINQ, if you are accepting user input to build expressions you need to make sure than only intended expressions can be constructed.</span></span>
