---
title: Работа с DbContext-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: b0e6bddc-8a87-4d51-b1cb-7756df938c23
ms.openlocfilehash: d961ffd8bed7f5b2f82dcfa30fc0241b7437be50
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/06/2020
ms.locfileid: "78413885"
---
# <a name="working-with-dbcontext"></a><span data-ttu-id="82929-102">Работа с DbContext</span><span class="sxs-lookup"><span data-stu-id="82929-102">Working with DbContext</span></span>

<span data-ttu-id="82929-103">Чтобы использовать Entity Framework для запроса, вставки, обновления и удаления данных с помощью объектов .NET, необходимо сначала [создать модель](~/ef6/modeling/index.md) , которая сопоставляет сущности и связи, определенные в модели, с таблицами в базе данных.</span><span class="sxs-lookup"><span data-stu-id="82929-103">In order to use Entity Framework to query, insert, update, and delete data using .NET objects, you first need to [Create a Model](~/ef6/modeling/index.md) which maps the entities and relationships that are defined in your model to tables in a database.</span></span>

<span data-ttu-id="82929-104">После создания модели основной класс, с которым взаимодействует приложение, является `System.Data.Entity.DbContext` (часто называется классом контекста).</span><span class="sxs-lookup"><span data-stu-id="82929-104">Once you have a model, the primary class your application interacts with is `System.Data.Entity.DbContext` (often referred to as the context class).</span></span> <span data-ttu-id="82929-105">DbContext, связанный с моделью, можно использовать для:</span><span class="sxs-lookup"><span data-stu-id="82929-105">You can use a DbContext associated to a model to:</span></span>
- <span data-ttu-id="82929-106">Создание и выполнение запросов</span><span class="sxs-lookup"><span data-stu-id="82929-106">Write and execute queries</span></span>   
- <span data-ttu-id="82929-107">Материализация результатов запросов в виде объектов сущностей</span><span class="sxs-lookup"><span data-stu-id="82929-107">Materialize query results as entity objects</span></span>
- <span data-ttu-id="82929-108">Отслеживание изменений, внесенных в эти объекты</span><span class="sxs-lookup"><span data-stu-id="82929-108">Track changes that are made to those objects</span></span>
- <span data-ttu-id="82929-109">Сохранить изменения объекта обратно в базе данных</span><span class="sxs-lookup"><span data-stu-id="82929-109">Persist object changes back on the database</span></span>
- <span data-ttu-id="82929-110">Привязка объектов в памяти к элементам управления пользовательского интерфейса</span><span class="sxs-lookup"><span data-stu-id="82929-110">Bind objects in memory to UI controls</span></span>

<span data-ttu-id="82929-111">На этой странице приводятся некоторые рекомендации по управлению классом контекста.</span><span class="sxs-lookup"><span data-stu-id="82929-111">This page gives some guidance on how to manage the context class.</span></span>  

## <a name="defining-a-dbcontext-derived-class"></a><span data-ttu-id="82929-112">Определение производного класса DbContext</span><span class="sxs-lookup"><span data-stu-id="82929-112">Defining a DbContext derived class</span></span>  

<span data-ttu-id="82929-113">Для работы с контекстом рекомендуется определить класс, производный от DbContext и предоставляющий свойства DbSet, представляющие коллекции указанных сущностей в контексте.</span><span class="sxs-lookup"><span data-stu-id="82929-113">The recommended way to work with context is to define a class that derives from DbContext and exposes DbSet properties that represent collections of the specified entities in the context.</span></span> <span data-ttu-id="82929-114">При работе с конструктором EF этот контекст будет создан автоматически.</span><span class="sxs-lookup"><span data-stu-id="82929-114">If you are working with the EF Designer, the context will be generated for you.</span></span> <span data-ttu-id="82929-115">Если вы работаете с Code First, вы обычно пишете контекст самостоятельно.</span><span class="sxs-lookup"><span data-stu-id="82929-115">If you are working with Code First, you will typically write the context yourself.</span></span>  

``` csharp
public class ProductContext : DbContext
{
    public DbSet<Category> Categories { get; set; }
    public DbSet<Product> Products { get; set; }
}
```  

<span data-ttu-id="82929-116">После создания контекста необходимо запросить, добавить (с помощью `Add` или `Attach` методов) или удалить (с помощью `Remove`) сущности в контексте с помощью этих свойств.</span><span class="sxs-lookup"><span data-stu-id="82929-116">Once you have a context, you would query for, add (using `Add` or `Attach` methods ) or remove (using `Remove`) entities in the context through these properties.</span></span> <span data-ttu-id="82929-117">Доступ к свойству `DbSet` объекта контекста представляет собой начальный запрос, возвращающий все сущности указанного типа.</span><span class="sxs-lookup"><span data-stu-id="82929-117">Accessing a `DbSet` property on a context object represent a starting query that returns all entities of the specified type.</span></span> <span data-ttu-id="82929-118">Обратите внимание, что только доступ к свойству не приведет к выполнению запроса.</span><span class="sxs-lookup"><span data-stu-id="82929-118">Note that just accessing a property will not execute the query.</span></span> <span data-ttu-id="82929-119">Запрос выполняется в следующих случаях:</span><span class="sxs-lookup"><span data-stu-id="82929-119">A query is executed when:</span></span>  

- <span data-ttu-id="82929-120">Запрос перечисляется с помощью инструкции `foreach` (C#) или `For Each` (Visual Basic).</span><span class="sxs-lookup"><span data-stu-id="82929-120">It is enumerated by a `foreach` (C#) or `For Each` (Visual Basic) statement.</span></span>  
- <span data-ttu-id="82929-121">Он перечисляется операцией сбора, например `ToArray`, `ToDictionary`или `ToList`.</span><span class="sxs-lookup"><span data-stu-id="82929-121">It is enumerated by a collection operation such as `ToArray`, `ToDictionary`, or `ToList`.</span></span>  
- <span data-ttu-id="82929-122">Операторы LINQ, такие как `First` или `Any`, указываются в самой внешней части запроса.</span><span class="sxs-lookup"><span data-stu-id="82929-122">LINQ operators such as `First` or `Any` are specified in the outermost part of the query.</span></span>  
- <span data-ttu-id="82929-123">Вызывается один из следующих методов: метод расширения `Load`, `DbEntityEntry.Reload`, `Database.ExecuteSqlCommand`и `DbSet<T>.Find`, если сущность с указанным ключом не найдена в контексте уже загружена.</span><span class="sxs-lookup"><span data-stu-id="82929-123">One of the following methods are called: the `Load` extension method, `DbEntityEntry.Reload`,  `Database.ExecuteSqlCommand`, and `DbSet<T>.Find`, if an entity with the specified key is not found already loaded in the context.</span></span>  

## <a name="lifetime"></a><span data-ttu-id="82929-124">Срок действия</span><span class="sxs-lookup"><span data-stu-id="82929-124">Lifetime</span></span>  

<span data-ttu-id="82929-125">Время существования контекста начинается при создании экземпляра и завершается, когда экземпляр удаляется или уничтожается сборщиком мусора.</span><span class="sxs-lookup"><span data-stu-id="82929-125">The lifetime of the context begins when the instance is created and ends when the instance is either disposed or garbage-collected.</span></span> <span data-ttu-id="82929-126">Используйте **Использование** , если требуется, чтобы все ресурсы, которые элемент управления контекста удалялись в конце блока.</span><span class="sxs-lookup"><span data-stu-id="82929-126">Use **using** if you want all the resources that the context controls to be disposed at the end of the block.</span></span> <span data-ttu-id="82929-127">При использовании **с помощью**компилятор автоматически создает блок try/finally и вызывает Dispose в блоке **finally** .</span><span class="sxs-lookup"><span data-stu-id="82929-127">When you use **using**, the compiler automatically creates a try/finally block and calls dispose in the **finally** block.</span></span>  

``` csharp
public void UseProducts()
{
    using (var context = new ProductContext())
    {     
        // Perform data access using the context
    }
}
```  

<span data-ttu-id="82929-128">Ниже приведены некоторые общие рекомендации, которые следует учитывать при выборе времени существования контекста.</span><span class="sxs-lookup"><span data-stu-id="82929-128">Here are some general guidelines when deciding on the lifetime of the context:</span></span>  

- <span data-ttu-id="82929-129">При работе с веб-приложениями используйте экземпляр контекста для каждого запроса.</span><span class="sxs-lookup"><span data-stu-id="82929-129">When working with Web applications, use a context instance per request.</span></span>  
- <span data-ttu-id="82929-130">При работе с Windows Presentation Foundation (WPF) или Windows Forms используйте экземпляр контекста для каждой формы.</span><span class="sxs-lookup"><span data-stu-id="82929-130">When working with Windows Presentation Foundation (WPF) or Windows Forms, use a context instance per form.</span></span> <span data-ttu-id="82929-131">Это позволяет использовать функциональные возможности отслеживания изменений, предоставляемые контекстом.</span><span class="sxs-lookup"><span data-stu-id="82929-131">This lets you use change-tracking functionality that context provides.</span></span>  
- <span data-ttu-id="82929-132">Если экземпляр контекста создается с помощью контейнера внедрения зависимостей, обычно он является обязанностью контейнера для удаления контекста.</span><span class="sxs-lookup"><span data-stu-id="82929-132">If the context instance is created by a dependency injection container, it is usually the responsibility of the container to dispose the context.</span></span>
- <span data-ttu-id="82929-133">Если контекст создается в коде приложения, не забудьте удалить контекст, если он больше не требуется.</span><span class="sxs-lookup"><span data-stu-id="82929-133">If the context is created in application code, remember to dispose of the context when it is no longer required.</span></span>  
- <span data-ttu-id="82929-134">При работе с долго выполняемым контекстом учитывайте следующее.</span><span class="sxs-lookup"><span data-stu-id="82929-134">When working with long-running context consider the following:</span></span>  
    - <span data-ttu-id="82929-135">Когда вы загружаете больше объектов и их ссылок в память, использование памяти контекста может быстро увеличиться.</span><span class="sxs-lookup"><span data-stu-id="82929-135">As you load more objects and their references into memory, the memory consumption of the context may increase rapidly.</span></span> <span data-ttu-id="82929-136">Это может вызвать снижение производительности.</span><span class="sxs-lookup"><span data-stu-id="82929-136">This may cause performance issues.</span></span>  
    - <span data-ttu-id="82929-137">Контекст не является потокобезопасным, поэтому он не должен совместно использоваться несколькими потоками, работающими над ним одновременно.</span><span class="sxs-lookup"><span data-stu-id="82929-137">The context is not thread-safe, therefore it should not be shared across multiple threads doing work on it concurrently.</span></span>
    - <span data-ttu-id="82929-138">Если исключение приводит к тому, что контекст находится в невосстанавливаемом состоянии, все приложение может завершиться.</span><span class="sxs-lookup"><span data-stu-id="82929-138">If an exception causes the context to be in an unrecoverable state, the whole application may terminate.</span></span>  
    - <span data-ttu-id="82929-139">Вероятность возникновения проблем с параллелизмом возрастает по мере увеличения разрыва между временем запроса и временем обновления данных.</span><span class="sxs-lookup"><span data-stu-id="82929-139">The chances of running into concurrency-related issues increase as the gap between the time when the data is queried and updated grows.</span></span>  

## <a name="connections"></a><span data-ttu-id="82929-140">Соединения</span><span class="sxs-lookup"><span data-stu-id="82929-140">Connections</span></span>  

<span data-ttu-id="82929-141">По умолчанию контекст управляет соединениями с базой данных.</span><span class="sxs-lookup"><span data-stu-id="82929-141">By default, the context manages connections to the database.</span></span> <span data-ttu-id="82929-142">При необходимости контекст открывает и закрывает подключения.</span><span class="sxs-lookup"><span data-stu-id="82929-142">The context opens and closes connections as needed.</span></span> <span data-ttu-id="82929-143">Например, контекст открывает соединение для выполнения запроса, а затем закрывает соединение после обработки всех результирующих наборов.</span><span class="sxs-lookup"><span data-stu-id="82929-143">For example, the context opens a connection to execute a query, and then closes the connection when all the result sets have been processed.</span></span>  

<span data-ttu-id="82929-144">В некоторых случаях необходимо больше контроля над открытием и закрытием соединения.</span><span class="sxs-lookup"><span data-stu-id="82929-144">There are cases when you want to have more control over when the connection opens and closes.</span></span> <span data-ttu-id="82929-145">Например, при работе с SQL Server Compact часто рекомендуется поддерживать отдельное открытое соединение с базой данных в течение всего времени существования приложения, чтобы повысить производительность.</span><span class="sxs-lookup"><span data-stu-id="82929-145">For example, when working with SQL Server Compact, it is often recommended to maintain a separate open connection to the database for the lifetime of the application to improve performance.</span></span> <span data-ttu-id="82929-146">Процессом можно управлять вручную с помощью свойства `Connection`.</span><span class="sxs-lookup"><span data-stu-id="82929-146">You can manage this process manually by using the `Connection` property.</span></span>  
