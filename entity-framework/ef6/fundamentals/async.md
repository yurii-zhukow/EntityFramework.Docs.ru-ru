---
title: Async запрос и сохранение - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: d56e6f1d-4bd1-4b50-9558-9a30e04a8ec3
ms.openlocfilehash: a001df726b7cd3342fcca566c1373f452811d07d
ms.sourcegitcommit: 144edccf9b29a7ffad119c235ac9808ec1a46193
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/16/2020
ms.locfileid: "81434244"
---
# <a name="async-query-and-save"></a><span data-ttu-id="9369f-102">Запрос и сохранение Async</span><span class="sxs-lookup"><span data-stu-id="9369f-102">Async query and save</span></span>
> [!NOTE]
> <span data-ttu-id="9369f-103">**Только в EF6 и более поздних версиях**. Функции, API и другие возможности, описанные на этой странице, появились в Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="9369f-103">**EF6 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 6.</span></span> <span data-ttu-id="9369f-104">При использовании более ранней версии могут быть неприменимы некоторые или все сведения.</span><span class="sxs-lookup"><span data-stu-id="9369f-104">If you are using an earlier version, some or all of the information does not apply.</span></span>

<span data-ttu-id="9369f-105">EF6 ввела поддержку асинхронного запроса и сохранения с помощью [async и ждать ключевых слов,](https://msdn.microsoft.com/library/vstudio/hh191443.aspx) которые были введены в .NET 4.5.</span><span class="sxs-lookup"><span data-stu-id="9369f-105">EF6 introduced support for asynchronous query and save using the [async and await keywords](https://msdn.microsoft.com/library/vstudio/hh191443.aspx) that were introduced in .NET 4.5.</span></span> <span data-ttu-id="9369f-106">Хотя не все приложения могут извлечь выгоду из асинхронности, он может быть использован для улучшения отзывчивости клиентов и масштабируемости сервера при обработке длительных, сетевых или вводом-связанных задач.</span><span class="sxs-lookup"><span data-stu-id="9369f-106">While not all applications may benefit from asynchrony, it can be used to improve client responsiveness and server scalability when handling long-running, network or I/O-bound tasks.</span></span>

## <a name="when-to-really-use-async"></a><span data-ttu-id="9369f-107">Когда действительно использовать async</span><span class="sxs-lookup"><span data-stu-id="9369f-107">When to really use async</span></span>

<span data-ttu-id="9369f-108">Цель этого пошагового представления состоит в том, чтобы ввести понятия async таким образом, чтобы можно было легко наблюдать разницу между асинхронным и синхронным выполнением программы.</span><span class="sxs-lookup"><span data-stu-id="9369f-108">The purpose of this walkthrough is to introduce the async concepts in a way that makes it easy to observe the difference between asynchronous and synchronous program execution.</span></span> <span data-ttu-id="9369f-109">Это пошаговое решение не предназначено для иллюстрации любого из ключевых сценариев, в которых программирование async дает преимущества.</span><span class="sxs-lookup"><span data-stu-id="9369f-109">This walkthrough is not intended to illustrate any of the key scenarios where async programming provides benefits.</span></span>

<span data-ttu-id="9369f-110">Программирование Async в первую очередь сосредоточено на высвобождении текущего управляемого потока (поток, выполняющий код .NET) для выполнения другой работы, пока он ждет операции, не требующей времени вычисления от управляемого потока.</span><span class="sxs-lookup"><span data-stu-id="9369f-110">Async programming is primarily focused on freeing up the current managed thread (thread running .NET code) to do other work while it waits for an operation that does not require any compute time from a managed thread.</span></span> <span data-ttu-id="9369f-111">Например, в то время как движок базы данных обрабатывает запрос, код .NET ничего не может сделать.</span><span class="sxs-lookup"><span data-stu-id="9369f-111">For example, whilst the database engine is processing a query there is nothing to be done by .NET code.</span></span>

<span data-ttu-id="9369f-112">В клиентских приложениях (WinForms, WPF и т.д.) текущий поток может использоваться для поддержания реакции в ашего мониторе во время выполнения операции async.</span><span class="sxs-lookup"><span data-stu-id="9369f-112">In client applications (WinForms, WPF, etc.) the current thread can be used to keep the UI responsive while the async operation is performed.</span></span> <span data-ttu-id="9369f-113">В серверных приложениях (ASP.NET и т.д.) поток может использоваться для обработки других входящих запросов - это может уменьшить использование памяти и/или увеличить пропускную стоимость сервера.</span><span class="sxs-lookup"><span data-stu-id="9369f-113">In server applications (ASP.NET etc.) the thread can be used to process other incoming requests - this can reduce memory usage and/or increase throughput of the server.</span></span>

<span data-ttu-id="9369f-114">В большинстве приложений с использованием async не будет иметь заметных преимуществ и даже может быть пагубным.</span><span class="sxs-lookup"><span data-stu-id="9369f-114">In most applications using async will have no noticeable benefits and even could be detrimental.</span></span> <span data-ttu-id="9369f-115">Используйте тесты, профилирование и здравый смысл для измерения влияния async в вашем конкретном сценарии, прежде чем совершить его.</span><span class="sxs-lookup"><span data-stu-id="9369f-115">Use tests, profiling and common sense to measure the impact of async in your particular scenario before committing to it.</span></span>

<span data-ttu-id="9369f-116">Вот еще несколько ресурсов, чтобы узнать о async:</span><span class="sxs-lookup"><span data-stu-id="9369f-116">Here are some more resources to learn about async:</span></span>

-   [<span data-ttu-id="9369f-117">Обзор Брэндон Брей async / ждать в .NET 4.5</span><span class="sxs-lookup"><span data-stu-id="9369f-117">Brandon Bray’s overview of async/await in .NET 4.5</span></span>](https://devblogs.microsoft.com/dotnet/async-in-4-5-worth-the-await/)
-   <span data-ttu-id="9369f-118">[Асинхронные страницы программирования](https://msdn.microsoft.com/library/hh191443.aspx) в библиотеке MSDN</span><span class="sxs-lookup"><span data-stu-id="9369f-118">[Asynchronous Programming](https://msdn.microsoft.com/library/hh191443.aspx) pages in the MSDN Library</span></span>
-   <span data-ttu-id="9369f-119">[Как построить ASP.NET веб-приложений с помощью Async](https://channel9.msdn.com/events/teched/northamerica/2013/dev-b337) (включает демонстрацию увеличенной пропускной записи сервера)</span><span class="sxs-lookup"><span data-stu-id="9369f-119">[How to Build ASP.NET Web Applications Using Async](https://channel9.msdn.com/events/teched/northamerica/2013/dev-b337) (includes a demo of increased server throughput)</span></span>

## <a name="create-the-model"></a><span data-ttu-id="9369f-120">Создание модели</span><span class="sxs-lookup"><span data-stu-id="9369f-120">Create the model</span></span>

<span data-ttu-id="9369f-121">Мы будем использовать [рабочий процесс Code First](~/ef6/modeling/code-first/workflows/new-database.md) для создания нашей модели и создания базы данных, однако асинхронная функциональность будет работать со всеми моделями EF, включая те, которые созданы с EF Designer.</span><span class="sxs-lookup"><span data-stu-id="9369f-121">We’ll be using the [Code First workflow](~/ef6/modeling/code-first/workflows/new-database.md) to create our model and generate the database, however the asynchronous functionality will work with all EF models including those created with the EF Designer.</span></span>

-   <span data-ttu-id="9369f-122">Создайте консольное приложение и назовите его **AsyncDemo**</span><span class="sxs-lookup"><span data-stu-id="9369f-122">Create a Console Application and call it **AsyncDemo**</span></span>
-   <span data-ttu-id="9369f-123">Добавить пакет EntityFramework NuGet</span><span class="sxs-lookup"><span data-stu-id="9369f-123">Add the EntityFramework NuGet package</span></span>
    -   <span data-ttu-id="9369f-124">В Solution Explorer нажмите правой кнопкой мыши на **проекте AsyncDemo**</span><span class="sxs-lookup"><span data-stu-id="9369f-124">In Solution Explorer, right-click on the **AsyncDemo** project</span></span>
    -   <span data-ttu-id="9369f-125">Выберите **Пакеты Управления NuGet...**</span><span class="sxs-lookup"><span data-stu-id="9369f-125">Select **Manage NuGet Packages…**</span></span>
    -   <span data-ttu-id="9369f-126">В диалоге пакетов Управления NuGet выберите вкладку **Online** и выберите пакет **EntityFramework**</span><span class="sxs-lookup"><span data-stu-id="9369f-126">In the Manage NuGet Packages dialog, Select the **Online** tab and choose the **EntityFramework** package</span></span>
    -   <span data-ttu-id="9369f-127">Нажмите **Установить**</span><span class="sxs-lookup"><span data-stu-id="9369f-127">Click **Install**</span></span>
-   <span data-ttu-id="9369f-128">Добавьте **Model.cs** класс со следующей реализацией</span><span class="sxs-lookup"><span data-stu-id="9369f-128">Add a **Model.cs** class with the following implementation</span></span>

``` csharp
    using System.Collections.Generic;
    using System.Data.Entity;

    namespace AsyncDemo
    {
        public class BloggingContext : DbContext
        {
            public DbSet<Blog> Blogs { get; set; }
            public DbSet<Post> Posts { get; set; }
        }

        public class Blog
        {
            public int BlogId { get; set; }
            public string Name { get; set; }

            public virtual List<Post> Posts { get; set; }
        }

        public class Post
        {
            public int PostId { get; set; }
            public string Title { get; set; }
            public string Content { get; set; }

            public int BlogId { get; set; }
            public virtual Blog Blog { get; set; }
        }
    }
```

 

## <a name="create-a-synchronous-program"></a><span data-ttu-id="9369f-129">Создание синхронной программы</span><span class="sxs-lookup"><span data-stu-id="9369f-129">Create a synchronous program</span></span>

<span data-ttu-id="9369f-130">Теперь, когда у нас есть модель EF, давайте напишем код, который использует ее для выполнения некоторых данных.</span><span class="sxs-lookup"><span data-stu-id="9369f-130">Now that we have an EF model, let's write some code that uses it to perform some data access.</span></span>

-   <span data-ttu-id="9369f-131">Замените содержимое **Program.cs** следующим кодом</span><span class="sxs-lookup"><span data-stu-id="9369f-131">Replace the contents of **Program.cs** with the following code</span></span>

``` csharp
    using System;
    using System.Linq;

    namespace AsyncDemo
    {
        class Program
        {
            static void Main(string[] args)
            {
                PerformDatabaseOperations();

                Console.WriteLine("Quote of the day");
                Console.WriteLine(" Don't worry about the world coming to an end today... ");
                Console.WriteLine(" It's already tomorrow in Australia.");

                Console.WriteLine();
                Console.WriteLine("Press any key to exit...");
                Console.ReadKey();
            }

            public static void PerformDatabaseOperations()
            {
                using (var db = new BloggingContext())
                {
                    // Create a new blog and save it
                    db.Blogs.Add(new Blog
                    {
                        Name = "Test Blog #" + (db.Blogs.Count() + 1)
                    });
                    Console.WriteLine("Calling SaveChanges.");
                    db.SaveChanges();
                    Console.WriteLine("SaveChanges completed.");

                    // Query for all blogs ordered by name
                    Console.WriteLine("Executing query.");
                    var blogs = (from b in db.Blogs
                                orderby b.Name
                                select b).ToList();

                    // Write all blogs out to Console
                    Console.WriteLine("Query completed with following results:");
                    foreach (var blog in blogs)
                    {
                        Console.WriteLine(" " + blog.Name);
                    }
                }
            }
        }
    }
```

<span data-ttu-id="9369f-132">Этот код вызывает метод **PerformDatabaseOperations,** который сохраняет новый **блог** в базу данных, а затем извлекает все **блоги** из базы данных и печатает их на **консоль.**</span><span class="sxs-lookup"><span data-stu-id="9369f-132">This code calls the **PerformDatabaseOperations** method which saves a new **Blog** to the database and then retrieves all **Blogs** from the database and prints them to the **Console**.</span></span> <span data-ttu-id="9369f-133">После этого программа пишет цитату дня на **консоль**.</span><span class="sxs-lookup"><span data-stu-id="9369f-133">After this, the program writes a quote of the day to the **Console**.</span></span>

<span data-ttu-id="9369f-134">Поскольку код синхронный, мы можем наблюдать следующий поток выполнения при запуске программы:</span><span class="sxs-lookup"><span data-stu-id="9369f-134">Since the code is synchronous, we can observe the following execution flow when we run the program:</span></span>

1.  <span data-ttu-id="9369f-135">**SaveChanges** начинает толкать новый **блог** в базу данных</span><span class="sxs-lookup"><span data-stu-id="9369f-135">**SaveChanges** begins to push the new **Blog** to the database</span></span>
2.  <span data-ttu-id="9369f-136">**SaveChanges** завершается</span><span class="sxs-lookup"><span data-stu-id="9369f-136">**SaveChanges** completes</span></span>
3.  <span data-ttu-id="9369f-137">Запрос для всех **блогов** отправляется в базу данных</span><span class="sxs-lookup"><span data-stu-id="9369f-137">Query for all **Blogs** is sent to the database</span></span>
4.  <span data-ttu-id="9369f-138">Возвраты запросов и результаты записываются в **Console**</span><span class="sxs-lookup"><span data-stu-id="9369f-138">Query returns and results are written to **Console**</span></span>
5.  <span data-ttu-id="9369f-139">Цитата дня написана на **консоли**</span><span class="sxs-lookup"><span data-stu-id="9369f-139">Quote of the day is written to **Console**</span></span>

![Выход синхронизации](~/ef6/media/syncoutput.png) 

 

## <a name="making-it-asynchronous"></a><span data-ttu-id="9369f-141">Создание асинхронного</span><span class="sxs-lookup"><span data-stu-id="9369f-141">Making it asynchronous</span></span>

<span data-ttu-id="9369f-142">Теперь, когда у нас есть наша программа и работает, мы можем начать использовать новые async и ждать ключевых слов.</span><span class="sxs-lookup"><span data-stu-id="9369f-142">Now that we have our program up and running, we can begin making use of the new async and await keywords.</span></span> <span data-ttu-id="9369f-143">Мы внесли следующие изменения в Program.cs</span><span class="sxs-lookup"><span data-stu-id="9369f-143">We've made the following changes to Program.cs</span></span>

1.  <span data-ttu-id="9369f-144">Строка 2: Использование оператора для пространства имен **System.Data.Entity** дает нам доступ к методам расширения EF async.</span><span class="sxs-lookup"><span data-stu-id="9369f-144">Line 2: The using statement for the **System.Data.Entity** namespace gives us access to the EF async extension methods.</span></span>
2.  <span data-ttu-id="9369f-145">Строка 4: Использование оператора для пространства имен **System.Threading.Tasks** позволяет нам использовать тип **задачи.**</span><span class="sxs-lookup"><span data-stu-id="9369f-145">Line 4: The using statement for the **System.Threading.Tasks** namespace allows us to use the **Task** type.</span></span>
3.  <span data-ttu-id="9369f-146">Строка 12 & 18: Мы захватываем как задачу, которая отслеживает ход **ВыполненияSomeDatabaseOperations** (строка 12), а затем блокируем выполнение программы для выполнения этой задачи после выполнения всей работы для программы (линия 18).</span><span class="sxs-lookup"><span data-stu-id="9369f-146">Line 12 & 18: We are capturing as task that monitors the progress of **PerformSomeDatabaseOperations** (line 12) and then blocking program execution for this task to complete once all the work for the program is done (line 18).</span></span>
4.  <span data-ttu-id="9369f-147">Строка 25: Мы обновляем **PerformSomeDatabaseOperations,** чтобы быть помечены как **async** и вернуть **задачу**.</span><span class="sxs-lookup"><span data-stu-id="9369f-147">Line 25: We've update **PerformSomeDatabaseOperations** to be marked as **async** and return a **Task**.</span></span>
5.  <span data-ttu-id="9369f-148">Строка 35: Мы сейчас вызываем версию Async SaveChanges и ожидаем ее завершения.</span><span class="sxs-lookup"><span data-stu-id="9369f-148">Line 35: We're now calling the Async version of SaveChanges and awaiting it's completion.</span></span>
6.  <span data-ttu-id="9369f-149">Строка 42: Мы сейчас вызываем версию Async ToList и ждем результата.</span><span class="sxs-lookup"><span data-stu-id="9369f-149">Line 42: We're now calling the Async version of ToList and awaiting on the result.</span></span>

<span data-ttu-id="9369f-150">Для получения полного списка доступных методов расширения в пространстве имен System.Data.Entity обратитесь к классу queryableExtensions.</span><span class="sxs-lookup"><span data-stu-id="9369f-150">For a comprehensive list of available extension methods in the System.Data.Entity namespace, refer to the QueryableExtensions class.</span></span> <span data-ttu-id="9369f-151">*Вам также нужно будет добавить "использование System.Data.Entity" к вашим сиюиным заявлениям.*</span><span class="sxs-lookup"><span data-stu-id="9369f-151">*You’ll also need to add “using System.Data.Entity” to your using statements.*</span></span>

``` csharp
    using System;
    using System.Data.Entity;
    using System.Linq;
    using System.Threading.Tasks;

    namespace AsyncDemo
    {
        class Program
        {
            static void Main(string[] args)
            {
                var task = PerformDatabaseOperations();

                Console.WriteLine("Quote of the day");
                Console.WriteLine(" Don't worry about the world coming to an end today... ");
                Console.WriteLine(" It's already tomorrow in Australia.");

                task.Wait();

                Console.WriteLine();
                Console.WriteLine("Press any key to exit...");
                Console.ReadKey();
            }

            public static async Task PerformDatabaseOperations()
            {
                using (var db = new BloggingContext())
                {
                    // Create a new blog and save it
                    db.Blogs.Add(new Blog
                    {
                        Name = "Test Blog #" + (db.Blogs.Count() + 1)
                    });
                    Console.WriteLine("Calling SaveChanges.");
                    await db.SaveChangesAsync();
                    Console.WriteLine("SaveChanges completed.");

                    // Query for all blogs ordered by name
                    Console.WriteLine("Executing query.");
                    var blogs = await (from b in db.Blogs
                                orderby b.Name
                                select b).ToListAsync();

                    // Write all blogs out to Console
                    Console.WriteLine("Query completed with following results:");
                    foreach (var blog in blogs)
                    {
                        Console.WriteLine(" - " + blog.Name);
                    }
                }
            }
        }
    }
```

<span data-ttu-id="9369f-152">Теперь, когда код асинхронный, мы можем наблюдать другой поток выполнения при запуске программы:</span><span class="sxs-lookup"><span data-stu-id="9369f-152">Now that the code is asynchronous, we can observe a different execution flow when we run the program:</span></span>

1. <span data-ttu-id="9369f-153">**SaveChanges** начинает толкать новый **блог** в базу данных</span><span class="sxs-lookup"><span data-stu-id="9369f-153">**SaveChanges** begins to push the new **Blog** to the database</span></span>  
    <span data-ttu-id="9369f-154">*После отправки команды в базу данных больше времени вычислений в текущем управляемом потоке не требуется. Возвращается метод **PerformDatabaseOperations** (даже если он еще не закончил выполнение) и поток программы в основном методе продолжается.*</span><span class="sxs-lookup"><span data-stu-id="9369f-154">*Once the command is sent to the database no more compute time is needed on the current managed thread. The **PerformDatabaseOperations** method returns (even though it hasn't finished executing) and program flow in the Main method continues.*</span></span>
2. <span data-ttu-id="9369f-155">**Цитата дня написана на консоли**</span><span class="sxs-lookup"><span data-stu-id="9369f-155">**Quote of the day is written to Console**</span></span>  
    <span data-ttu-id="9369f-156">*Поскольку в основном методе больше нет работы, управляемый поток блокируется на вызове Ожидания до завершения операции базы данных. Как только он завершится, остальная часть наших **операций PerformDatabaseбудет** будет выполнена.*</span><span class="sxs-lookup"><span data-stu-id="9369f-156">*Since there is no more work to do in the Main method, the managed thread is blocked on the Wait call until the database operation completes. Once it completes, the remainder of our **PerformDatabaseOperations** will be executed.*</span></span>
3.  <span data-ttu-id="9369f-157">**SaveChanges** завершается</span><span class="sxs-lookup"><span data-stu-id="9369f-157">**SaveChanges** completes</span></span>  
4.  <span data-ttu-id="9369f-158">Запрос для всех **блогов** отправляется в базу данных</span><span class="sxs-lookup"><span data-stu-id="9369f-158">Query for all **Blogs** is sent to the database</span></span>  
    <span data-ttu-id="9369f-159">*Опять же, управляемый поток может выполнять другую работу, пока запрос обрабатывается в базе данных. Так как все другие выполнения завершена, поток будет просто остановить на вызов ожидания, хотя.*</span><span class="sxs-lookup"><span data-stu-id="9369f-159">*Again, the managed thread is free to do other work while the query is processed in the database. Since all other execution has completed, the thread will just halt on the Wait call though.*</span></span>
5.  <span data-ttu-id="9369f-160">Возвраты запросов и результаты записываются в **Console**</span><span class="sxs-lookup"><span data-stu-id="9369f-160">Query returns and results are written to **Console**</span></span>  

![Выход Async](~/ef6/media/asyncoutput.png) 

 

## <a name="the-takeaway"></a><span data-ttu-id="9369f-162">Вынос</span><span class="sxs-lookup"><span data-stu-id="9369f-162">The takeaway</span></span>

<span data-ttu-id="9369f-163">Теперь мы увидели, как легко использовать асинхронные методы EF.</span><span class="sxs-lookup"><span data-stu-id="9369f-163">We now saw how easy it is to make use of EF’s asynchronous methods.</span></span> <span data-ttu-id="9369f-164">Хотя преимущества async могут быть не очень очевидными с помощью простого консольного приложения, эти же стратегии могут применяться в ситуациях, когда длительные или сетевые действия могут в противном случае заблокировать приложение или привести к увеличению количества потоков для увеличения памяти.</span><span class="sxs-lookup"><span data-stu-id="9369f-164">Although the advantages of async may not be very apparent with a simple console app, these same strategies can be applied in situations where long-running or network-bound activities might otherwise block the application, or cause a large number of threads to increase the memory footprint.</span></span>
