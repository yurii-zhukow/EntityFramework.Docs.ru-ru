---
title: Необработанные SQL-запросы - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 9e1ee76e-2499-408c-81e8-9b6c5d1945a0
ms.openlocfilehash: 6b00648939ccedffeed09b4e1d6e8d70fa262a36
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/13/2018
ms.locfileid: "45490588"
---
# <a name="raw-sql-queries"></a><span data-ttu-id="e2d4c-102">Необработанные SQL-запросы</span><span class="sxs-lookup"><span data-stu-id="e2d4c-102">Raw SQL Queries</span></span>
<span data-ttu-id="e2d4c-103">Платформа Entity Framework позволяет выполнять запросы с помощью LINQ с помощью классов сущностей.</span><span class="sxs-lookup"><span data-stu-id="e2d4c-103">Entity Framework allows you to query using LINQ with your entity classes.</span></span> <span data-ttu-id="e2d4c-104">Тем не менее могут возникнуть ситуации, которые вы хотите выполнять запросы с помощью необработанных SQL непосредственно в базе данных.</span><span class="sxs-lookup"><span data-stu-id="e2d4c-104">However, there may be times that you want to run queries using raw SQL directly against the database.</span></span> <span data-ttu-id="e2d4c-105">Это включает в себя вызов хранимых процедур, которые могут оказаться полезными для Code First моделей, которые в настоящее время не поддерживает сопоставление с хранимыми процедурами.</span><span class="sxs-lookup"><span data-stu-id="e2d4c-105">This includes calling stored procedures, which can be helpful for Code First models that currently do not support mapping to stored procedures.</span></span> <span data-ttu-id="e2d4c-106">Методы, представленные в этом разделе, также применимы к моделям, созданным с помощью Code First и конструктора EF.</span><span class="sxs-lookup"><span data-stu-id="e2d4c-106">The techniques shown in this topic apply equally to models created with Code First and the EF Designer.</span></span>  

## <a name="writing-sql-queries-for-entities"></a><span data-ttu-id="e2d4c-107">Написание SQL-запросов для сущностей</span><span class="sxs-lookup"><span data-stu-id="e2d4c-107">Writing SQL queries for entities</span></span>  

<span data-ttu-id="e2d4c-108">Метод SQL-запрос на DbSet позволяет необработанный SQL-запрос для записи, который возвращает экземпляры сущностей.</span><span class="sxs-lookup"><span data-stu-id="e2d4c-108">The SqlQuery method on DbSet allows a raw SQL query to be written that will return entity instances.</span></span> <span data-ttu-id="e2d4c-109">Возвращаемые объекты будут отслеживаемые по контексту, так же, как было бы, если они были возвращены запросом LINQ.</span><span class="sxs-lookup"><span data-stu-id="e2d4c-109">The returned objects will be tracked by the context just as they would be if they were returned by a LINQ query.</span></span> <span data-ttu-id="e2d4c-110">Пример:</span><span class="sxs-lookup"><span data-stu-id="e2d4c-110">For example:</span></span>  

``` csharp  
using (var context = new BloggingContext())
{
    var blogs = context.Blogs.SqlQuery("SELECT * FROM dbo.Blogs").ToList();
}
```  

<span data-ttu-id="e2d4c-111">Обратите внимание, что, так же как и запросы LINQ, запрос не выполняется до перечисляются результаты — в приведенном выше примере это делается с помощью вызова ToList.</span><span class="sxs-lookup"><span data-stu-id="e2d4c-111">Note that, just as for LINQ queries, the query is not executed until the results are enumerated—in the example above this is done with the call to ToList.</span></span>  

<span data-ttu-id="e2d4c-112">Следует соблюдать осторожность, каждый раз, когда необработанные SQL-запросы записываются по двум причинам.</span><span class="sxs-lookup"><span data-stu-id="e2d4c-112">Care should be taken whenever raw SQL queries are written for two reasons.</span></span> <span data-ttu-id="e2d4c-113">Во-первых запрос должны быть написаны так, чтобы убедиться, что он только возвращает сущности, которые действительно запрошенного типа.</span><span class="sxs-lookup"><span data-stu-id="e2d4c-113">First, the query should be written to ensure that it only returns entities that are really of the requested type.</span></span> <span data-ttu-id="e2d4c-114">Например при использовании механизмы вроде наследования можно легко написать запрос, который будет создание сущностей, которые имеют неправильный тип среды CLR.</span><span class="sxs-lookup"><span data-stu-id="e2d4c-114">For example, when using features such as inheritance it is easy to write a query that will create entities that are of the wrong CLR type.</span></span>  

<span data-ttu-id="e2d4c-115">Во-вторых некоторые виды необработанный SQL-запрос предоставлять потенциальные угрозы безопасности, особенно в отношении атаки путем внедрения кода SQL.</span><span class="sxs-lookup"><span data-stu-id="e2d4c-115">Second, some types of raw SQL query expose potential security risks, especially around SQL injection attacks.</span></span> <span data-ttu-id="e2d4c-116">Убедитесь в том, с использованием параметров в запросе в правильный способ защититься от таких атак.</span><span class="sxs-lookup"><span data-stu-id="e2d4c-116">Make sure that you use parameters in your query in the correct way to guard against such attacks.</span></span>  

### <a name="loading-entities-from-stored-procedures"></a><span data-ttu-id="e2d4c-117">Загрузка сущностей из хранимых процедур</span><span class="sxs-lookup"><span data-stu-id="e2d4c-117">Loading entities from stored procedures</span></span>  

<span data-ttu-id="e2d4c-118">DbSet.SqlQuery можно использовать для загрузки сущностей из результатов хранимой процедуры.</span><span class="sxs-lookup"><span data-stu-id="e2d4c-118">You can use DbSet.SqlQuery to load entities from the results of a stored procedure.</span></span> <span data-ttu-id="e2d4c-119">Например следующий код вызывает dbo. Процедура GetBlogs в базе данных.</span><span class="sxs-lookup"><span data-stu-id="e2d4c-119">For example, the following code calls the dbo.GetBlogs procedure in the database:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    var blogs = context.Blogs.SqlQuery("dbo.GetBlogs").ToList();
}
```  

<span data-ttu-id="e2d4c-120">Вы также можете передавать параметры в хранимую процедуру, используя следующий синтаксис:</span><span class="sxs-lookup"><span data-stu-id="e2d4c-120">You can also pass parameters to a stored procedure using the following syntax:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    var blogId = 1;

    var blogs = context.Blogs.SqlQuery("dbo.GetBlogById @p0", blogId).Single();
}
```  

## <a name="writing-sql-queries-for-non-entity-types"></a><span data-ttu-id="e2d4c-121">Написание SQL-запросов для несущностным типам</span><span class="sxs-lookup"><span data-stu-id="e2d4c-121">Writing SQL queries for non-entity types</span></span>  

<span data-ttu-id="e2d4c-122">SQL-запроса возвращаются экземпляры любого типа, включая типы-примитивы, могут создаваться с помощью метода SQL-запрос к классу базы данных.</span><span class="sxs-lookup"><span data-stu-id="e2d4c-122">A SQL query returning instances of any type, including primitive types, can be created using the SqlQuery method on the Database class.</span></span> <span data-ttu-id="e2d4c-123">Пример:</span><span class="sxs-lookup"><span data-stu-id="e2d4c-123">For example:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    var blogNames = context.Database.SqlQuery<string>(
                       "SELECT Name FROM dbo.Blogs").ToList();
}
```  

<span data-ttu-id="e2d4c-124">Результаты, возвращенные SQL-запрос в базе данных никогда не быть отслеживаемые по контексту, даже если объекты являются экземплярами типа сущности.</span><span class="sxs-lookup"><span data-stu-id="e2d4c-124">The results returned from SqlQuery on Database will never be tracked by the context even if the objects are instances of an entity type.</span></span>  

## <a name="sending-raw-commands-to-the-database"></a><span data-ttu-id="e2d4c-125">Отправка необработанных команды в базу данных</span><span class="sxs-lookup"><span data-stu-id="e2d4c-125">Sending raw commands to the database</span></span>  

<span data-ttu-id="e2d4c-126">Команды запросов не могут отправляться в базу данных, используя метод ExecuteSqlCommand в базе данных.</span><span class="sxs-lookup"><span data-stu-id="e2d4c-126">Non-query commands can be sent to the database using the ExecuteSqlCommand method on Database.</span></span> <span data-ttu-id="e2d4c-127">Пример:</span><span class="sxs-lookup"><span data-stu-id="e2d4c-127">For example:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    context.Database.ExecuteSqlCommand(
        "UPDATE dbo.Blogs SET Name = 'Another Name' WHERE BlogId = 1");
}
```  

<span data-ttu-id="e2d4c-128">Обратите внимание, что любые изменения, внесенные в данные в базе данных с помощью ExecuteSqlCommand непрозрачности в контекст пока сущностей не будут загружены или перезагружена из базы данных.</span><span class="sxs-lookup"><span data-stu-id="e2d4c-128">Note that any changes made to data in the database using ExecuteSqlCommand are opaque to the context until entities are loaded or reloaded from the database.</span></span>  

### <a name="output-parameters"></a><span data-ttu-id="e2d4c-129">Выходные параметры</span><span class="sxs-lookup"><span data-stu-id="e2d4c-129">Output Parameters</span></span>  

<span data-ttu-id="e2d4c-130">Если используются выходные параметры, их значения не будут доступны, пока не полностью считать результаты.</span><span class="sxs-lookup"><span data-stu-id="e2d4c-130">If output parameters are used, their values will not be available until the results have been read completely.</span></span> <span data-ttu-id="e2d4c-131">Это происходит из-за особенностей DbDataReader, см. в разделе [получение данных с помощью объекта DataReader](http://go.microsoft.com/fwlink/?LinkID=398589) для получения дополнительных сведений.</span><span class="sxs-lookup"><span data-stu-id="e2d4c-131">This is due to the underlying behavior of DbDataReader, see [Retrieving Data Using a DataReader](http://go.microsoft.com/fwlink/?LinkID=398589) for more details.</span></span>  
