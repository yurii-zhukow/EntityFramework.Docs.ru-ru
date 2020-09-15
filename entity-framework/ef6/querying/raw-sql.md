---
title: Необработанные запросы SQL — EF6
description: Необработанные запросы SQL в Entity Framework 6
author: divega
ms.date: 10/23/2016
uid: ef6/querying/raw-sql
ms.openlocfilehash: 30fab3029d71fb560493af279231e46828aa43d4
ms.sourcegitcommit: abda0872f86eefeca191a9a11bfca976bc14468b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/14/2020
ms.locfileid: "90073890"
---
# <a name="raw-sql-queries-ef6"></a><span data-ttu-id="04344-103">Необработанные запросы SQL (EF6)</span><span class="sxs-lookup"><span data-stu-id="04344-103">Raw SQL Queries (EF6)</span></span>

<span data-ttu-id="04344-104">Entity Framework позволяет выполнять запросы с помощью LINQ к классам сущностей.</span><span class="sxs-lookup"><span data-stu-id="04344-104">Entity Framework allows you to query using LINQ with your entity classes.</span></span> <span data-ttu-id="04344-105">Однако могут возникнуть ситуации, когда требуется выполнить запросы с использованием необработанного SQL непосредственно к базе данных.</span><span class="sxs-lookup"><span data-stu-id="04344-105">However, there may be times that you want to run queries using raw SQL directly against the database.</span></span> <span data-ttu-id="04344-106">К ним относятся вызов хранимых процедур, которые могут быть полезны для Code First моделей, которые в настоящее время не поддерживают сопоставление с хранимыми процедурами.</span><span class="sxs-lookup"><span data-stu-id="04344-106">This includes calling stored procedures, which can be helpful for Code First models that currently do not support mapping to stored procedures.</span></span> <span data-ttu-id="04344-107">Методы, представленные в этом разделе, также применимы к моделям, созданным с помощью Code First и конструктора EF.</span><span class="sxs-lookup"><span data-stu-id="04344-107">The techniques shown in this topic apply equally to models created with Code First and the EF Designer.</span></span>  

## <a name="writing-sql-queries-for-entities"></a><span data-ttu-id="04344-108">Написание запросов SQL для сущностей</span><span class="sxs-lookup"><span data-stu-id="04344-108">Writing SQL queries for entities</span></span>  

<span data-ttu-id="04344-109">Метод SqlQuery в DbSet позволяет написать необработанный SQL-запрос, который будет возвращать экземпляры сущности.</span><span class="sxs-lookup"><span data-stu-id="04344-109">The SqlQuery method on DbSet allows a raw SQL query to be written that will return entity instances.</span></span> <span data-ttu-id="04344-110">Возвращаемые объекты будут относиться по контексту так же, как если бы они возвращались запросом LINQ.</span><span class="sxs-lookup"><span data-stu-id="04344-110">The returned objects will be tracked by the context just as they would be if they were returned by a LINQ query.</span></span> <span data-ttu-id="04344-111">Пример:</span><span class="sxs-lookup"><span data-stu-id="04344-111">For example:</span></span>  

``` csharp  
using (var context = new BloggingContext())
{
    var blogs = context.Blogs.SqlQuery("SELECT * FROM dbo.Blogs").ToList();
}
```  

<span data-ttu-id="04344-112">Обратите внимание, что, как и для запросов LINQ, запрос не выполняется до перечисления результатов, в приведенном выше примере это выполняется с помощью вызова ToList.</span><span class="sxs-lookup"><span data-stu-id="04344-112">Note that, just as for LINQ queries, the query is not executed until the results are enumerated—in the example above this is done with the call to ToList.</span></span>  

<span data-ttu-id="04344-113">Следует соблюдать осторожность при написании необработанных запросов SQL по двум причинам.</span><span class="sxs-lookup"><span data-stu-id="04344-113">Care should be taken whenever raw SQL queries are written for two reasons.</span></span> <span data-ttu-id="04344-114">Сначала необходимо написать запрос, чтобы убедиться, что он возвращает только те сущности, которые действительно относятся к запрошенному типу.</span><span class="sxs-lookup"><span data-stu-id="04344-114">First, the query should be written to ensure that it only returns entities that are really of the requested type.</span></span> <span data-ttu-id="04344-115">Например, при использовании таких функций, как наследование, легко написать запрос, который будет создавать сущности с неправильным типом CLR.</span><span class="sxs-lookup"><span data-stu-id="04344-115">For example, when using features such as inheritance it is easy to write a query that will create entities that are of the wrong CLR type.</span></span>  

<span data-ttu-id="04344-116">Во вторых, некоторые типы необработанных запросов SQL предоставляют потенциальные риски безопасности, особенно при атаках путем внедрения кода SQL.</span><span class="sxs-lookup"><span data-stu-id="04344-116">Second, some types of raw SQL query expose potential security risks, especially around SQL injection attacks.</span></span> <span data-ttu-id="04344-117">Убедитесь, что в запросе используются параметры для защиты от таких атак.</span><span class="sxs-lookup"><span data-stu-id="04344-117">Make sure that you use parameters in your query in the correct way to guard against such attacks.</span></span>  

### <a name="loading-entities-from-stored-procedures"></a><span data-ttu-id="04344-118">Загрузка сущностей из хранимых процедур</span><span class="sxs-lookup"><span data-stu-id="04344-118">Loading entities from stored procedures</span></span>  

<span data-ttu-id="04344-119">DbSet. SqlQuery можно использовать для загрузки сущностей из результатов хранимой процедуры.</span><span class="sxs-lookup"><span data-stu-id="04344-119">You can use DbSet.SqlQuery to load entities from the results of a stored procedure.</span></span> <span data-ttu-id="04344-120">Например, следующий код вызывает dbo. Процедура обработки и записи в базе данных:</span><span class="sxs-lookup"><span data-stu-id="04344-120">For example, the following code calls the dbo.GetBlogs procedure in the database:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    var blogs = context.Blogs.SqlQuery("dbo.GetBlogs").ToList();
}
```  

<span data-ttu-id="04344-121">Можно также передать параметры в хранимую процедуру, используя следующий синтаксис:</span><span class="sxs-lookup"><span data-stu-id="04344-121">You can also pass parameters to a stored procedure using the following syntax:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    var blogId = 1;

    var blogs = context.Blogs.SqlQuery("dbo.GetBlogById @p0", blogId).Single();
}
```  

## <a name="writing-sql-queries-for-non-entity-types"></a><span data-ttu-id="04344-122">Написание SQL-запросов для типов, не являющихся сущностями</span><span class="sxs-lookup"><span data-stu-id="04344-122">Writing SQL queries for non-entity types</span></span>  

<span data-ttu-id="04344-123">SQL-запрос, возвращающий экземпляры любого типа, включая типы-примитивы, можно создать с помощью метода SqlQuery в классе базы данных.</span><span class="sxs-lookup"><span data-stu-id="04344-123">A SQL query returning instances of any type, including primitive types, can be created using the SqlQuery method on the Database class.</span></span> <span data-ttu-id="04344-124">Пример:</span><span class="sxs-lookup"><span data-stu-id="04344-124">For example:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    var blogNames = context.Database.SqlQuery<string>(
                       "SELECT Name FROM dbo.Blogs").ToList();
}
```  

<span data-ttu-id="04344-125">Результаты, возвращаемые из SqlQuery в базе данных, никогда не будут контролироваться контекстом, даже если объекты являются экземплярами типа сущности.</span><span class="sxs-lookup"><span data-stu-id="04344-125">The results returned from SqlQuery on Database will never be tracked by the context even if the objects are instances of an entity type.</span></span>  

## <a name="sending-raw-commands-to-the-database"></a><span data-ttu-id="04344-126">Отправка необработанных команд в базу данных</span><span class="sxs-lookup"><span data-stu-id="04344-126">Sending raw commands to the database</span></span>  

<span data-ttu-id="04344-127">Команды, не связанные с запросами, можно отправлять в базу данных с помощью метода Ексекутесклкомманд в базе данных.</span><span class="sxs-lookup"><span data-stu-id="04344-127">Non-query commands can be sent to the database using the ExecuteSqlCommand method on Database.</span></span> <span data-ttu-id="04344-128">Пример:</span><span class="sxs-lookup"><span data-stu-id="04344-128">For example:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    context.Database.ExecuteSqlCommand(
        "UPDATE dbo.Blogs SET Name = 'Another Name' WHERE BlogId = 1");
}
```  

<span data-ttu-id="04344-129">Обратите внимание, что любые изменения, внесенные в данные в базе данных с помощью Ексекутесклкомманд, непрозрачны для контекста до тех пор, пока сущности не будут загружены или выгружены из базы данных.</span><span class="sxs-lookup"><span data-stu-id="04344-129">Note that any changes made to data in the database using ExecuteSqlCommand are opaque to the context until entities are loaded or reloaded from the database.</span></span>  

### <a name="output-parameters"></a><span data-ttu-id="04344-130">Выходные параметры</span><span class="sxs-lookup"><span data-stu-id="04344-130">Output Parameters</span></span>  

<span data-ttu-id="04344-131">Если используются выходные параметры, их значения будут недоступны до тех пор, пока результаты не считаны полностью.</span><span class="sxs-lookup"><span data-stu-id="04344-131">If output parameters are used, their values will not be available until the results have been read completely.</span></span> <span data-ttu-id="04344-132">Это обусловлено поведением DbDataReader. Дополнительные сведения см. в разделе [Получение данных с помощью DataReader](https://go.microsoft.com/fwlink/?LinkID=398589) .</span><span class="sxs-lookup"><span data-stu-id="04344-132">This is due to the underlying behavior of DbDataReader, see [Retrieving Data Using a DataReader](https://go.microsoft.com/fwlink/?LinkID=398589) for more details.</span></span>  
