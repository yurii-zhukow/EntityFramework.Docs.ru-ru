---
title: Работа с DbContext-EF6
description: Работа с DbContext в Entity Framework 6
author: ajcvickers
ms.date: 10/23/2016
uid: ef6/fundamentals/working-with-dbcontext
ms.openlocfilehash: aa980e102862b559c8f38418cf90a11f284cc48c
ms.sourcegitcommit: 0a25c03fa65ae6e0e0e3f66bac48d59eceb96a5a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/14/2020
ms.locfileid: "92062806"
---
# <a name="working-with-dbcontext"></a><span data-ttu-id="aa6fa-103">Работа с DbContext</span><span class="sxs-lookup"><span data-stu-id="aa6fa-103">Working with DbContext</span></span>

<span data-ttu-id="aa6fa-104">Чтобы использовать Entity Framework для запроса, вставки, обновления и удаления данных с помощью объектов .NET, необходимо сначала [создать модель](xref:ef6/modeling/index) , которая сопоставляет сущности и связи, определенные в модели, с таблицами в базе данных.</span><span class="sxs-lookup"><span data-stu-id="aa6fa-104">In order to use Entity Framework to query, insert, update, and delete data using .NET objects, you first need to [Create a Model](xref:ef6/modeling/index) which maps the entities and relationships that are defined in your model to tables in a database.</span></span>

<span data-ttu-id="aa6fa-105">После создания модели основной класс, с которым взаимодействует приложение, — это `System.Data.Entity.DbContext` (часто называется классом контекста).</span><span class="sxs-lookup"><span data-stu-id="aa6fa-105">Once you have a model, the primary class your application interacts with is `System.Data.Entity.DbContext` (often referred to as the context class).</span></span> <span data-ttu-id="aa6fa-106">DbContext, связанный с моделью, можно использовать для:</span><span class="sxs-lookup"><span data-stu-id="aa6fa-106">You can use a DbContext associated to a model to:</span></span>
- <span data-ttu-id="aa6fa-107">Создание и выполнение запросов</span><span class="sxs-lookup"><span data-stu-id="aa6fa-107">Write and execute queries</span></span>   
- <span data-ttu-id="aa6fa-108">Материализация результатов запросов в виде объектов сущностей</span><span class="sxs-lookup"><span data-stu-id="aa6fa-108">Materialize query results as entity objects</span></span>
- <span data-ttu-id="aa6fa-109">Отслеживание изменений, внесенных в эти объекты</span><span class="sxs-lookup"><span data-stu-id="aa6fa-109">Track changes that are made to those objects</span></span>
- <span data-ttu-id="aa6fa-110">Сохранить изменения объекта обратно в базе данных</span><span class="sxs-lookup"><span data-stu-id="aa6fa-110">Persist object changes back on the database</span></span>
- <span data-ttu-id="aa6fa-111">Привязка объектов в памяти к элементам управления пользовательского интерфейса</span><span class="sxs-lookup"><span data-stu-id="aa6fa-111">Bind objects in memory to UI controls</span></span>

<span data-ttu-id="aa6fa-112">На этой странице приводятся некоторые рекомендации по управлению классом контекста.</span><span class="sxs-lookup"><span data-stu-id="aa6fa-112">This page gives some guidance on how to manage the context class.</span></span>  

## <a name="defining-a-dbcontext-derived-class"></a><span data-ttu-id="aa6fa-113">Определение производного класса DbContext</span><span class="sxs-lookup"><span data-stu-id="aa6fa-113">Defining a DbContext derived class</span></span>  

<span data-ttu-id="aa6fa-114">Для работы с контекстом рекомендуется определить класс, производный от DbContext и предоставляющий свойства DbSet, представляющие коллекции указанных сущностей в контексте.</span><span class="sxs-lookup"><span data-stu-id="aa6fa-114">The recommended way to work with context is to define a class that derives from DbContext and exposes DbSet properties that represent collections of the specified entities in the context.</span></span> <span data-ttu-id="aa6fa-115">При работе с конструктором EF этот контекст будет создан автоматически.</span><span class="sxs-lookup"><span data-stu-id="aa6fa-115">If you are working with the EF Designer, the context will be generated for you.</span></span> <span data-ttu-id="aa6fa-116">Если вы работаете с Code First, вы обычно пишете контекст самостоятельно.</span><span class="sxs-lookup"><span data-stu-id="aa6fa-116">If you are working with Code First, you will typically write the context yourself.</span></span>  

``` csharp
public class ProductContext : DbContext
{
    public DbSet<Category> Categories { get; set; }
    public DbSet<Product> Products { get; set; }
}
```  

<span data-ttu-id="aa6fa-117">После создания контекста необходимо запросить, добавить (использовать `Add` `Attach` методы или) или удалить (с помощью `Remove` ) сущности в контексте с помощью этих свойств.</span><span class="sxs-lookup"><span data-stu-id="aa6fa-117">Once you have a context, you would query for, add (using `Add` or `Attach` methods ) or remove (using `Remove`) entities in the context through these properties.</span></span> <span data-ttu-id="aa6fa-118">Доступ к `DbSet` свойству объекта контекста представляет собой начальный запрос, который возвращает все сущности указанного типа.</span><span class="sxs-lookup"><span data-stu-id="aa6fa-118">Accessing a `DbSet` property on a context object represent a starting query that returns all entities of the specified type.</span></span> <span data-ttu-id="aa6fa-119">Обратите внимание, что только доступ к свойству не приведет к выполнению запроса.</span><span class="sxs-lookup"><span data-stu-id="aa6fa-119">Note that just accessing a property will not execute the query.</span></span> <span data-ttu-id="aa6fa-120">Запрос выполняется в следующих случаях:</span><span class="sxs-lookup"><span data-stu-id="aa6fa-120">A query is executed when:</span></span>  

- <span data-ttu-id="aa6fa-121">Запрос перечисляется с помощью инструкции `foreach` (C#) или `For Each` (Visual Basic).</span><span class="sxs-lookup"><span data-stu-id="aa6fa-121">It is enumerated by a `foreach` (C#) or `For Each` (Visual Basic) statement.</span></span>  
- <span data-ttu-id="aa6fa-122">Он перечисляется операцией сбора, например `ToArray` , `ToDictionary` или `ToList` .</span><span class="sxs-lookup"><span data-stu-id="aa6fa-122">It is enumerated by a collection operation such as `ToArray`, `ToDictionary`, or `ToList`.</span></span>  
- <span data-ttu-id="aa6fa-123">Операторы LINQ, такие как `First` или `Any` , указываются в самой внешней части запроса.</span><span class="sxs-lookup"><span data-stu-id="aa6fa-123">LINQ operators such as `First` or `Any` are specified in the outermost part of the query.</span></span>  
- <span data-ttu-id="aa6fa-124">Вызывается один из следующих методов: `Load` метод расширения,, `DbEntityEntry.Reload`  `Database.ExecuteSqlCommand` и `DbSet<T>.Find` , если сущность с указанным ключом не найдена в контексте уже загружена.</span><span class="sxs-lookup"><span data-stu-id="aa6fa-124">One of the following methods are called: the `Load` extension method, `DbEntityEntry.Reload`,  `Database.ExecuteSqlCommand`, and `DbSet<T>.Find`, if an entity with the specified key is not found already loaded in the context.</span></span>  

## <a name="lifetime"></a><span data-ttu-id="aa6fa-125">Время существования</span><span class="sxs-lookup"><span data-stu-id="aa6fa-125">Lifetime</span></span>  

<span data-ttu-id="aa6fa-126">Время существования контекста начинается при создании экземпляра и завершается, когда экземпляр удаляется или уничтожается сборщиком мусора.</span><span class="sxs-lookup"><span data-stu-id="aa6fa-126">The lifetime of the context begins when the instance is created and ends when the instance is either disposed or garbage-collected.</span></span> <span data-ttu-id="aa6fa-127">Используйте **Использование** , если требуется, чтобы все ресурсы, которые элемент управления контекста удалялись в конце блока.</span><span class="sxs-lookup"><span data-stu-id="aa6fa-127">Use **using** if you want all the resources that the context controls to be disposed at the end of the block.</span></span> <span data-ttu-id="aa6fa-128">При использовании **с помощью**компилятор автоматически создает блок try/finally и вызывает Dispose в блоке **finally** .</span><span class="sxs-lookup"><span data-stu-id="aa6fa-128">When you use **using**, the compiler automatically creates a try/finally block and calls dispose in the **finally** block.</span></span>  

``` csharp
public void UseProducts()
{
    using (var context = new ProductContext())
    {     
        // Perform data access using the context
    }
}
```  

<span data-ttu-id="aa6fa-129">Ниже приведены некоторые общие рекомендации, которые следует учитывать при выборе времени существования контекста.</span><span class="sxs-lookup"><span data-stu-id="aa6fa-129">Here are some general guidelines when deciding on the lifetime of the context:</span></span>  

- <span data-ttu-id="aa6fa-130">При работе с веб-приложениями используйте экземпляр контекста для каждого запроса.</span><span class="sxs-lookup"><span data-stu-id="aa6fa-130">When working with Web applications, use a context instance per request.</span></span>  
- <span data-ttu-id="aa6fa-131">При работе с Windows Presentation Foundation (WPF) или Windows Forms используйте экземпляр контекста для каждой формы.</span><span class="sxs-lookup"><span data-stu-id="aa6fa-131">When working with Windows Presentation Foundation (WPF) or Windows Forms, use a context instance per form.</span></span> <span data-ttu-id="aa6fa-132">Это позволяет использовать функциональные возможности отслеживания изменений, предоставляемые контекстом.</span><span class="sxs-lookup"><span data-stu-id="aa6fa-132">This lets you use change-tracking functionality that context provides.</span></span>  
- <span data-ttu-id="aa6fa-133">Если экземпляр контекста создается с помощью контейнера внедрения зависимостей, обычно он является обязанностью контейнера для удаления контекста.</span><span class="sxs-lookup"><span data-stu-id="aa6fa-133">If the context instance is created by a dependency injection container, it is usually the responsibility of the container to dispose the context.</span></span>
- <span data-ttu-id="aa6fa-134">Если контекст создается в коде приложения, не забудьте удалить контекст, если он больше не требуется.</span><span class="sxs-lookup"><span data-stu-id="aa6fa-134">If the context is created in application code, remember to dispose of the context when it is no longer required.</span></span>  
- <span data-ttu-id="aa6fa-135">При работе с долго выполняемым контекстом учитывайте следующее.</span><span class="sxs-lookup"><span data-stu-id="aa6fa-135">When working with long-running context consider the following:</span></span>  
    - <span data-ttu-id="aa6fa-136">Когда вы загружаете больше объектов и их ссылок в память, использование памяти контекста может быстро увеличиться.</span><span class="sxs-lookup"><span data-stu-id="aa6fa-136">As you load more objects and their references into memory, the memory consumption of the context may increase rapidly.</span></span> <span data-ttu-id="aa6fa-137">Это может вызвать снижение производительности.</span><span class="sxs-lookup"><span data-stu-id="aa6fa-137">This may cause performance issues.</span></span>  
    - <span data-ttu-id="aa6fa-138">Контекст не является потокобезопасным, поэтому он не должен совместно использоваться несколькими потоками, работающими над ним одновременно.</span><span class="sxs-lookup"><span data-stu-id="aa6fa-138">The context is not thread-safe, therefore it should not be shared across multiple threads doing work on it concurrently.</span></span>
    - <span data-ttu-id="aa6fa-139">Если исключение приводит к тому, что контекст находится в невосстанавливаемом состоянии, все приложение может завершиться.</span><span class="sxs-lookup"><span data-stu-id="aa6fa-139">If an exception causes the context to be in an unrecoverable state, the whole application may terminate.</span></span>  
    - <span data-ttu-id="aa6fa-140">Вероятность возникновения проблем с параллелизмом возрастает по мере увеличения разрыва между временем запроса и временем обновления данных.</span><span class="sxs-lookup"><span data-stu-id="aa6fa-140">The chances of running into concurrency-related issues increase as the gap between the time when the data is queried and updated grows.</span></span>  

## <a name="connections"></a><span data-ttu-id="aa6fa-141">Соединения</span><span class="sxs-lookup"><span data-stu-id="aa6fa-141">Connections</span></span>  

<span data-ttu-id="aa6fa-142">По умолчанию контекст управляет соединениями с базой данных.</span><span class="sxs-lookup"><span data-stu-id="aa6fa-142">By default, the context manages connections to the database.</span></span> <span data-ttu-id="aa6fa-143">При необходимости контекст открывает и закрывает подключения.</span><span class="sxs-lookup"><span data-stu-id="aa6fa-143">The context opens and closes connections as needed.</span></span> <span data-ttu-id="aa6fa-144">Например, контекст открывает соединение для выполнения запроса, а затем закрывает соединение после обработки всех результирующих наборов.</span><span class="sxs-lookup"><span data-stu-id="aa6fa-144">For example, the context opens a connection to execute a query, and then closes the connection when all the result sets have been processed.</span></span>  

<span data-ttu-id="aa6fa-145">В некоторых случаях необходимо больше контроля над открытием и закрытием соединения.</span><span class="sxs-lookup"><span data-stu-id="aa6fa-145">There are cases when you want to have more control over when the connection opens and closes.</span></span> <span data-ttu-id="aa6fa-146">Например, при работе с SQL Server Compact часто рекомендуется поддерживать отдельное открытое соединение с базой данных в течение всего времени существования приложения, чтобы повысить производительность.</span><span class="sxs-lookup"><span data-stu-id="aa6fa-146">For example, when working with SQL Server Compact, it is often recommended to maintain a separate open connection to the database for the lifetime of the application to improve performance.</span></span> <span data-ttu-id="aa6fa-147">Процессом можно управлять вручную с помощью свойства `Connection`.</span><span class="sxs-lookup"><span data-stu-id="aa6fa-147">You can manage this process manually by using the `Connection` property.</span></span>  
