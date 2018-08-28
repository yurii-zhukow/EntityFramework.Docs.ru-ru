---
title: Работа с DbContext - EF6
author: divega
ms.date: 2016-10-23
ms.assetid: b0e6bddc-8a87-4d51-b1cb-7756df938c23
ms.openlocfilehash: f95f503c4e40e65503d5af0c1b686d0055728bfe
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/27/2018
ms.locfileid: "42998150"
---
# <a name="working-with-dbcontext"></a><span data-ttu-id="c9377-102">Работа с DbContext</span><span class="sxs-lookup"><span data-stu-id="c9377-102">Working with DbContext</span></span>

<span data-ttu-id="c9377-103">Чтобы использовать Entity Framework для запроса, вставки, обновления и удаления данных, используя объекты .NET, необходимо сначала [создать модель](~/ef6/modeling/index.md) который сопоставляет сущности и связи, которые определены в модели с таблицами в базе данных.</span><span class="sxs-lookup"><span data-stu-id="c9377-103">In order to use Entity Framework to query, insert, update, and delete data using .NET objects, you first need to [Create a Model](~/ef6/modeling/index.md) which maps the entities and relationships that are defined in your model to tables in a database.</span></span>

<span data-ttu-id="c9377-104">При наличии модели является основным классом, ваше приложение взаимодействует с `System.Data.Entity.DbContext` (часто обозначается как класс контекста).</span><span class="sxs-lookup"><span data-stu-id="c9377-104">Once you have a model, the primary class your application interacts with is `System.Data.Entity.DbContext` (often referred to as the context class).</span></span> <span data-ttu-id="c9377-105">Можно использовать связанные модели для DbContext:</span><span class="sxs-lookup"><span data-stu-id="c9377-105">You can use a DbContext associated to a model to:</span></span>
- <span data-ttu-id="c9377-106">Запись и выполнение запросов</span><span class="sxs-lookup"><span data-stu-id="c9377-106">Write and execute queries</span></span>   
- <span data-ttu-id="c9377-107">Материализовать результаты запроса в форме объектов сущности</span><span class="sxs-lookup"><span data-stu-id="c9377-107">Materialize query results as entity objects</span></span>
- <span data-ttu-id="c9377-108">Отслеживать изменения, внесенные в эти объекты</span><span class="sxs-lookup"><span data-stu-id="c9377-108">Track changes that are made to those objects</span></span>
- <span data-ttu-id="c9377-109">Сохранить изменения объектов обратно в базе данных</span><span class="sxs-lookup"><span data-stu-id="c9377-109">Persist object changes back on the database</span></span>
- <span data-ttu-id="c9377-110">Привязка объектов в памяти к элементам управления пользовательского интерфейса</span><span class="sxs-lookup"><span data-stu-id="c9377-110">Bind objects in memory to UI controls</span></span>

<span data-ttu-id="c9377-111">На этой странице приведены некоторые рекомендации по управлению класс контекста.</span><span class="sxs-lookup"><span data-stu-id="c9377-111">This page gives some guidance on how to manage the context class.</span></span>  

## <a name="defining-a-dbcontext-derived-class"></a><span data-ttu-id="c9377-112">Определение класса, производного DbContext</span><span class="sxs-lookup"><span data-stu-id="c9377-112">Defining a DbContext derived class</span></span>  

<span data-ttu-id="c9377-113">Для работы с контекстом рекомендуется определить класс, производный от DbContext и обеспечивает доступ к свойствам DbSet, представляющие коллекцию сущностей, указанный в контексте.</span><span class="sxs-lookup"><span data-stu-id="c9377-113">The recommended way to work with context is to define a class that derives from DbContext and exposes DbSet properties that represent collections of the specified entities in the context.</span></span> <span data-ttu-id="c9377-114">При работе с конструктором, EF, контекст будет создаваться автоматически.</span><span class="sxs-lookup"><span data-stu-id="c9377-114">If you are working with the EF Designer, the context will be generated for you.</span></span> <span data-ttu-id="c9377-115">Если вы работаете в режиме Code First, будут обычно контексте самостоятельном написании.</span><span class="sxs-lookup"><span data-stu-id="c9377-115">If you are working with Code First, you will typically write the context yourself.</span></span>  

``` csharp
public class ProductContext : DbContext
{
    public DbSet<Category> Categories { get; set; }
    public DbSet<Product> Products { get; set; }
}
```  

<span data-ttu-id="c9377-116">Получив контекст, нужно запросить, добавить (с помощью `Add` или `Attach` методы) или удалить (с помощью `Remove`) сущностей в контексте через эти свойства.</span><span class="sxs-lookup"><span data-stu-id="c9377-116">Once you have a context, you would query for, add (using `Add` or `Attach` methods ) or remove (using `Remove`) entities in the context through these properties.</span></span> <span data-ttu-id="c9377-117">Доступ к `DbSet` свойство в объекте контекста представляет начальный запрос, возвращающий все сущности заданного типа.</span><span class="sxs-lookup"><span data-stu-id="c9377-117">Accessing a `DbSet` property on a context object represent a starting query that returns all entities of the specified type.</span></span> <span data-ttu-id="c9377-118">Обратите внимание на то, что только доступ к свойству не будет выполняться запрос.</span><span class="sxs-lookup"><span data-stu-id="c9377-118">Note that just accessing a property will not execute the query.</span></span> <span data-ttu-id="c9377-119">Запрос выполняется при:</span><span class="sxs-lookup"><span data-stu-id="c9377-119">A query is executed when:</span></span>  

- <span data-ttu-id="c9377-120">Запрос перечисляется с помощью инструкции `foreach` (C#) или `For Each` (Visual Basic).</span><span class="sxs-lookup"><span data-stu-id="c9377-120">It is enumerated by a `foreach` (C#) or `For Each` (Visual Basic) statement.</span></span>  
- <span data-ttu-id="c9377-121">Запрос перечисляется операцией коллекции, такие как `ToArray`, `ToDictionary`, или `ToList`.</span><span class="sxs-lookup"><span data-stu-id="c9377-121">It is enumerated by a collection operation such as `ToArray`, `ToDictionary`, or `ToList`.</span></span>  
- <span data-ttu-id="c9377-122">Операторы LINQ, такие как `First` или `Any` указываются в внешней части запроса.</span><span class="sxs-lookup"><span data-stu-id="c9377-122">LINQ operators such as `First` or `Any` are specified in the outermost part of the query.</span></span>  
- <span data-ttu-id="c9377-123">Вызван один из следующих методов: `Load` метод расширения, `DbEntityEntry.Reload`, `Database.ExecuteSqlCommand`, и `DbSet<T>.Find`, если сущность с указанным ключом отсутствует уже загружены в контекст.</span><span class="sxs-lookup"><span data-stu-id="c9377-123">One of the following methods are called: the `Load` extension method, `DbEntityEntry.Reload`,  `Database.ExecuteSqlCommand`, and `DbSet<T>.Find`, if an entity with the specified key is not found already loaded in the context.</span></span>  

## <a name="lifetime"></a><span data-ttu-id="c9377-124">Время существования</span><span class="sxs-lookup"><span data-stu-id="c9377-124">Lifetime</span></span>  

<span data-ttu-id="c9377-125">Время существования контекста начинается, когда создается экземпляр и заканчивается, когда экземпляр удаляется или сборщиком мусора.</span><span class="sxs-lookup"><span data-stu-id="c9377-125">The lifetime of the context begins when the instance is created and ends when the instance is either disposed or garbage-collected.</span></span> <span data-ttu-id="c9377-126">Используйте **с помощью** Если необходимо, чтобы все ресурсы, контекст управляет непосредственно перед удалением в конце блока.</span><span class="sxs-lookup"><span data-stu-id="c9377-126">Use **using** if you want all the resources that the context controls to be disposed at the end of the block.</span></span> <span data-ttu-id="c9377-127">При использовании **с помощью**, компилятор автоматически создает блок try/finally и вызывает удаление в **наконец** блока.</span><span class="sxs-lookup"><span data-stu-id="c9377-127">When you use **using**, the compiler automatically creates a try/finally block and calls dispose in the **finally** block.</span></span>  

``` csharp
public void UseProducts()
{
    using (var context = new ProductContext())
    {     
        // Perform data access using the context
    }
}
```  

<span data-ttu-id="c9377-128">Ниже приведены некоторые общие рекомендации при принятии решения о времени существования контекста.</span><span class="sxs-lookup"><span data-stu-id="c9377-128">Here are some general guidelines when deciding on the lifetime of the context:</span></span>  

- <span data-ttu-id="c9377-129">При работе с веб-приложений, используйте экземпляр контекста каждого запроса.</span><span class="sxs-lookup"><span data-stu-id="c9377-129">When working with Web applications, use a context instance per request.</span></span>  
- <span data-ttu-id="c9377-130">При работе с Windows Presentation Foundation (WPF) или Windows Forms, используйте экземпляр контекста на каждую форму.</span><span class="sxs-lookup"><span data-stu-id="c9377-130">When working with Windows Presentation Foundation (WPF) or Windows Forms, use a context instance per form.</span></span> <span data-ttu-id="c9377-131">Это позволяет использовать функции отслеживания изменений предоставляет этот контекст.</span><span class="sxs-lookup"><span data-stu-id="c9377-131">This lets you use change-tracking functionality that context provides.</span></span>  
- <span data-ttu-id="c9377-132">Если создан экземпляр контекста с контейнер внедрения зависимостей, он отвечает обычно контейнера высвобождение контекста.</span><span class="sxs-lookup"><span data-stu-id="c9377-132">If the context instance is created by a dependency injection container, it is usually the responsibility of the container to dispose the context.</span></span>
- <span data-ttu-id="c9377-133">Если контекст создается в коде приложения, не забывайте удалять контекст, когда он больше не требуется.</span><span class="sxs-lookup"><span data-stu-id="c9377-133">If the context is created in application code, remember to dispose of the context when it is no longer required.</span></span>  
- <span data-ttu-id="c9377-134">При работе с длительно существующем контексте учитывайте следующее:</span><span class="sxs-lookup"><span data-stu-id="c9377-134">When working with long-running context consider the following:</span></span>  
    - <span data-ttu-id="c9377-135">Несколько объектов и их ссылок при загрузке в память, может быстро увеличить потребление памяти контекста.</span><span class="sxs-lookup"><span data-stu-id="c9377-135">As you load more objects and their references into memory, the memory consumption of the context may increase rapidly.</span></span> <span data-ttu-id="c9377-136">Это может вызвать снижение производительности.</span><span class="sxs-lookup"><span data-stu-id="c9377-136">This may cause performance issues.</span></span>  
    - <span data-ttu-id="c9377-137">Контекст не является потокобезопасным, поэтому его не следует использовать совместно в нескольких потоках одновременно выполняют его работу.</span><span class="sxs-lookup"><span data-stu-id="c9377-137">The context is not thread-safe, therefore it should not be shared across multiple threads doing work on it concurrently.</span></span>
    - <span data-ttu-id="c9377-138">Если исключение вызывает контекст быть в состоянии неустранимых, всего приложения может завершиться.</span><span class="sxs-lookup"><span data-stu-id="c9377-138">If an exception causes the context to be in an unrecoverable state, the whole application may terminate.</span></span>  
    - <span data-ttu-id="c9377-139">Вероятность возникновения проблем с параллелизмом возрастает по мере увеличения разрыва между временем запроса и временем обновления данных.</span><span class="sxs-lookup"><span data-stu-id="c9377-139">The chances of running into concurrency-related issues increase as the gap between the time when the data is queried and updated grows.</span></span>  

## <a name="connections"></a><span data-ttu-id="c9377-140">Подключения</span><span class="sxs-lookup"><span data-stu-id="c9377-140">Connections</span></span>  

<span data-ttu-id="c9377-141">По умолчанию контекст управляет подключениями к базе данных.</span><span class="sxs-lookup"><span data-stu-id="c9377-141">By default, the context manages connections to the database.</span></span> <span data-ttu-id="c9377-142">Контекст открывает и закрывает соединения по мере необходимости.</span><span class="sxs-lookup"><span data-stu-id="c9377-142">The context opens and closes connections as needed.</span></span> <span data-ttu-id="c9377-143">Например контекст открывает соединение для выполнения запроса и затем закрывает соединение, когда будут обработаны все результирующие наборы.</span><span class="sxs-lookup"><span data-stu-id="c9377-143">For example, the context opens a connection to execute a query, and then closes the connection when all the result sets have been processed.</span></span>  

<span data-ttu-id="c9377-144">В некоторых случаях необходимо больше контроля над открытием и закрытием соединения.</span><span class="sxs-lookup"><span data-stu-id="c9377-144">There are cases when you want to have more control over when the connection opens and closes.</span></span> <span data-ttu-id="c9377-145">Например при работе с SQL Server Compact, часто рекомендуется поддерживать отдельные откройте подключение к базе данных в течение времени существования приложения для повышения производительности.</span><span class="sxs-lookup"><span data-stu-id="c9377-145">For example, when working with SQL Server Compact, it is often recommended to maintain a separate open connection to the database for the lifetime of the application to improve performance.</span></span> <span data-ttu-id="c9377-146">Процессом можно управлять вручную с помощью свойства `Connection`.</span><span class="sxs-lookup"><span data-stu-id="c9377-146">You can manage this process manually by using the `Connection` property.</span></span>  
