---
title: Необработанные запросы SQL — EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 9e1ee76e-2499-408c-81e8-9b6c5d1945a0
ms.openlocfilehash: 168aee67186535bf2a50705e86bfc5a88147e369
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/06/2020
ms.locfileid: "78414473"
---
# <a name="raw-sql-queries"></a><span data-ttu-id="ae284-102">Необработанные SQL-запросы</span><span class="sxs-lookup"><span data-stu-id="ae284-102">Raw SQL Queries</span></span>
<span data-ttu-id="ae284-103">Entity Framework позволяет выполнять запросы с помощью LINQ к классам сущностей.</span><span class="sxs-lookup"><span data-stu-id="ae284-103">Entity Framework allows you to query using LINQ with your entity classes.</span></span> <span data-ttu-id="ae284-104">Однако могут возникнуть ситуации, когда требуется выполнить запросы с использованием необработанного SQL непосредственно к базе данных.</span><span class="sxs-lookup"><span data-stu-id="ae284-104">However, there may be times that you want to run queries using raw SQL directly against the database.</span></span> <span data-ttu-id="ae284-105">К ним относятся вызов хранимых процедур, которые могут быть полезны для Code First моделей, которые в настоящее время не поддерживают сопоставление с хранимыми процедурами.</span><span class="sxs-lookup"><span data-stu-id="ae284-105">This includes calling stored procedures, which can be helpful for Code First models that currently do not support mapping to stored procedures.</span></span> <span data-ttu-id="ae284-106">Методы, представленные в этом разделе, также применимы к моделям, созданным с помощью Code First и конструктора EF.</span><span class="sxs-lookup"><span data-stu-id="ae284-106">The techniques shown in this topic apply equally to models created with Code First and the EF Designer.</span></span>  

## <a name="writing-sql-queries-for-entities"></a><span data-ttu-id="ae284-107">Написание запросов SQL для сущностей</span><span class="sxs-lookup"><span data-stu-id="ae284-107">Writing SQL queries for entities</span></span>  

<span data-ttu-id="ae284-108">Метод SqlQuery в DbSet позволяет написать необработанный SQL-запрос, который будет возвращать экземпляры сущности.</span><span class="sxs-lookup"><span data-stu-id="ae284-108">The SqlQuery method on DbSet allows a raw SQL query to be written that will return entity instances.</span></span> <span data-ttu-id="ae284-109">Возвращаемые объекты будут относиться по контексту так же, как если бы они возвращались запросом LINQ.</span><span class="sxs-lookup"><span data-stu-id="ae284-109">The returned objects will be tracked by the context just as they would be if they were returned by a LINQ query.</span></span> <span data-ttu-id="ae284-110">Пример:</span><span class="sxs-lookup"><span data-stu-id="ae284-110">For example:</span></span>  

``` csharp  
using (var context = new BloggingContext())
{
    var blogs = context.Blogs.SqlQuery("SELECT * FROM dbo.Blogs").ToList();
}
```  

<span data-ttu-id="ae284-111">Обратите внимание, что, как и для запросов LINQ, запрос не выполняется до перечисления результатов, в приведенном выше примере это выполняется с помощью вызова ToList.</span><span class="sxs-lookup"><span data-stu-id="ae284-111">Note that, just as for LINQ queries, the query is not executed until the results are enumerated—in the example above this is done with the call to ToList.</span></span>  

<span data-ttu-id="ae284-112">Следует соблюдать осторожность при написании необработанных запросов SQL по двум причинам.</span><span class="sxs-lookup"><span data-stu-id="ae284-112">Care should be taken whenever raw SQL queries are written for two reasons.</span></span> <span data-ttu-id="ae284-113">Сначала необходимо написать запрос, чтобы убедиться, что он возвращает только те сущности, которые действительно относятся к запрошенному типу.</span><span class="sxs-lookup"><span data-stu-id="ae284-113">First, the query should be written to ensure that it only returns entities that are really of the requested type.</span></span> <span data-ttu-id="ae284-114">Например, при использовании таких функций, как наследование, легко написать запрос, который будет создавать сущности с неправильным типом CLR.</span><span class="sxs-lookup"><span data-stu-id="ae284-114">For example, when using features such as inheritance it is easy to write a query that will create entities that are of the wrong CLR type.</span></span>  

<span data-ttu-id="ae284-115">Во вторых, некоторые типы необработанных запросов SQL предоставляют потенциальные риски безопасности, особенно при атаках путем внедрения кода SQL.</span><span class="sxs-lookup"><span data-stu-id="ae284-115">Second, some types of raw SQL query expose potential security risks, especially around SQL injection attacks.</span></span> <span data-ttu-id="ae284-116">Убедитесь, что в запросе используются параметры для защиты от таких атак.</span><span class="sxs-lookup"><span data-stu-id="ae284-116">Make sure that you use parameters in your query in the correct way to guard against such attacks.</span></span>  

### <a name="loading-entities-from-stored-procedures"></a><span data-ttu-id="ae284-117">Загрузка сущностей из хранимых процедур</span><span class="sxs-lookup"><span data-stu-id="ae284-117">Loading entities from stored procedures</span></span>  

<span data-ttu-id="ae284-118">DbSet. SqlQuery можно использовать для загрузки сущностей из результатов хранимой процедуры.</span><span class="sxs-lookup"><span data-stu-id="ae284-118">You can use DbSet.SqlQuery to load entities from the results of a stored procedure.</span></span> <span data-ttu-id="ae284-119">Например, следующий код вызывает dbo. Процедура обработки и записи в базе данных:</span><span class="sxs-lookup"><span data-stu-id="ae284-119">For example, the following code calls the dbo.GetBlogs procedure in the database:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    var blogs = context.Blogs.SqlQuery("dbo.GetBlogs").ToList();
}
```  

<span data-ttu-id="ae284-120">Можно также передать параметры в хранимую процедуру, используя следующий синтаксис:</span><span class="sxs-lookup"><span data-stu-id="ae284-120">You can also pass parameters to a stored procedure using the following syntax:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    var blogId = 1;

    var blogs = context.Blogs.SqlQuery("dbo.GetBlogById @p0", blogId).Single();
}
```  

## <a name="writing-sql-queries-for-non-entity-types"></a><span data-ttu-id="ae284-121">Написание SQL-запросов для типов, не являющихся сущностями</span><span class="sxs-lookup"><span data-stu-id="ae284-121">Writing SQL queries for non-entity types</span></span>  

<span data-ttu-id="ae284-122">SQL-запрос, возвращающий экземпляры любого типа, включая типы-примитивы, можно создать с помощью метода SqlQuery в классе базы данных.</span><span class="sxs-lookup"><span data-stu-id="ae284-122">A SQL query returning instances of any type, including primitive types, can be created using the SqlQuery method on the Database class.</span></span> <span data-ttu-id="ae284-123">Пример:</span><span class="sxs-lookup"><span data-stu-id="ae284-123">For example:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    var blogNames = context.Database.SqlQuery<string>(
                       "SELECT Name FROM dbo.Blogs").ToList();
}
```  

<span data-ttu-id="ae284-124">Результаты, возвращаемые из SqlQuery в базе данных, никогда не будут контролироваться контекстом, даже если объекты являются экземплярами типа сущности.</span><span class="sxs-lookup"><span data-stu-id="ae284-124">The results returned from SqlQuery on Database will never be tracked by the context even if the objects are instances of an entity type.</span></span>  

## <a name="sending-raw-commands-to-the-database"></a><span data-ttu-id="ae284-125">Отправка необработанных команд в базу данных</span><span class="sxs-lookup"><span data-stu-id="ae284-125">Sending raw commands to the database</span></span>  

<span data-ttu-id="ae284-126">Команды, не связанные с запросами, можно отправлять в базу данных с помощью метода Ексекутесклкомманд в базе данных.</span><span class="sxs-lookup"><span data-stu-id="ae284-126">Non-query commands can be sent to the database using the ExecuteSqlCommand method on Database.</span></span> <span data-ttu-id="ae284-127">Пример:</span><span class="sxs-lookup"><span data-stu-id="ae284-127">For example:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    context.Database.ExecuteSqlCommand(
        "UPDATE dbo.Blogs SET Name = 'Another Name' WHERE BlogId = 1");
}
```  

<span data-ttu-id="ae284-128">Обратите внимание, что любые изменения, внесенные в данные в базе данных с помощью Ексекутесклкомманд, непрозрачны для контекста до тех пор, пока сущности не будут загружены или выгружены из базы данных.</span><span class="sxs-lookup"><span data-stu-id="ae284-128">Note that any changes made to data in the database using ExecuteSqlCommand are opaque to the context until entities are loaded or reloaded from the database.</span></span>  

### <a name="output-parameters"></a><span data-ttu-id="ae284-129">Выходные параметры</span><span class="sxs-lookup"><span data-stu-id="ae284-129">Output Parameters</span></span>  

<span data-ttu-id="ae284-130">Если используются выходные параметры, их значения будут недоступны до тех пор, пока результаты не считаны полностью.</span><span class="sxs-lookup"><span data-stu-id="ae284-130">If output parameters are used, their values will not be available until the results have been read completely.</span></span> <span data-ttu-id="ae284-131">Это обусловлено поведением DbDataReader. Дополнительные сведения см. в разделе [Получение данных с помощью DataReader](https://go.microsoft.com/fwlink/?LinkID=398589) .</span><span class="sxs-lookup"><span data-stu-id="ae284-131">This is due to the underlying behavior of DbDataReader, see [Retrieving Data Using a DataReader](https://go.microsoft.com/fwlink/?LinkID=398589) for more details.</span></span>  
