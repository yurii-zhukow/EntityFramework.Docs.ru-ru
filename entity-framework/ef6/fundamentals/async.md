---
title: Асинхронные запросы и сохраняет - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: d56e6f1d-4bd1-4b50-9558-9a30e04a8ec3
ms.openlocfilehash: 89c7b9d533d37b4c9e123f37d8ab27c67ba26cc8
ms.sourcegitcommit: 159c2e9afed7745e7512730ffffaf154bcf2ff4a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/03/2019
ms.locfileid: "55668717"
---
# <a name="async-query-and-save"></a><span data-ttu-id="fb174-102">Асинхронный запрос и сохраните</span><span class="sxs-lookup"><span data-stu-id="fb174-102">Async query and save</span></span>
> [!NOTE]
> <span data-ttu-id="fb174-103">**Только в EF6 и более поздних версиях**. Функции, API и другие возможности, описанные на этой странице, появились в Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="fb174-103">**EF6 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 6.</span></span> <span data-ttu-id="fb174-104">При использовании более ранней версии могут быть неприменимы некоторые или все сведения.</span><span class="sxs-lookup"><span data-stu-id="fb174-104">If you are using an earlier version, some or all of the information does not apply.</span></span>

<span data-ttu-id="fb174-105">EF6 появилась поддержка для асинхронного запроса и сохраните файл с [async и await ключевые слова](https://msdn.microsoft.com/library/vstudio/hh191443.aspx) , которые появились в .NET 4.5.</span><span class="sxs-lookup"><span data-stu-id="fb174-105">EF6 introduced support for asynchronous query and save using the [async and await keywords](https://msdn.microsoft.com/library/vstudio/hh191443.aspx) that were introduced in .NET 4.5.</span></span> <span data-ttu-id="fb174-106">Не все приложения могут выиграть от асинхронность, его можно использовать для повышения масштабируемости клиента времени отклика и сервера при обработке долго выполняющихся, сети или задачи ввода-вывода.</span><span class="sxs-lookup"><span data-stu-id="fb174-106">While not all applications may benefit from asynchrony, it can be used to improve client responsiveness and server scalability when handling long-running, network or I/O-bound tasks.</span></span>

## <a name="when-to-really-use-async"></a><span data-ttu-id="fb174-107">Когда действительно использовать асинхронный</span><span class="sxs-lookup"><span data-stu-id="fb174-107">When to really use async</span></span>

<span data-ttu-id="fb174-108">В этом пошаговом руководстве предназначено для ознакомления с основными понятиями async способом, который позволяет легко определить разницу между выполнениями программы асинхронные и синхронные.</span><span class="sxs-lookup"><span data-stu-id="fb174-108">The purpose of this walkthrough is to introduce the async concepts in a way that makes it easy to observe the difference between asynchronous and synchronous program execution.</span></span> <span data-ttu-id="fb174-109">В этом пошаговом руководстве не предназначен для иллюстрации этих основных сценариев где асинхронного программирования предоставляет преимущества.</span><span class="sxs-lookup"><span data-stu-id="fb174-109">This walkthrough is not intended to illustrate any of the key scenarios where async programming provides benefits.</span></span>

<span data-ttu-id="fb174-110">Асинхронное программирование в основном ориентировано на освобождение текущему управляемому потоку (поток выполнение кода .NET) может выполнять другие задачи при ожидании операции, которая не требует любое время вычислений из управляемого потока.</span><span class="sxs-lookup"><span data-stu-id="fb174-110">Async programming is primarily focused on freeing up the current managed thread (thread running .NET code) to do other work while it waits for an operation that does not require any compute time from a managed thread.</span></span> <span data-ttu-id="fb174-111">Например в процессе ядра СУБД обработки запроса нет ничего выполнять код .NET.</span><span class="sxs-lookup"><span data-stu-id="fb174-111">For example, whilst the database engine is processing a query there is nothing to be done by .NET code.</span></span>

<span data-ttu-id="fb174-112">В клиентских приложениях (WinForms, WPF и т.д.) текущего потока используется для сохранения отклика пользовательского интерфейса во время выполнения асинхронной операции.</span><span class="sxs-lookup"><span data-stu-id="fb174-112">In client applications (WinForms, WPF, etc.) the current thread can be used to keep the UI responsive while the async operation is performed.</span></span> <span data-ttu-id="fb174-113">В потоке позволяют обрабатывать другие входящие запросы - серверных приложений (ASP.NET и т.д.) это может уменьшить уровень использования памяти и/или увеличить пропускную способность сервера.</span><span class="sxs-lookup"><span data-stu-id="fb174-113">In server applications (ASP.NET etc.) the thread can be used to process other incoming requests - this can reduce memory usage and/or increase throughput of the server.</span></span>

<span data-ttu-id="fb174-114">В большинстве приложений использование метода async даст без заметного преимущества и даже может привести к ухудшению результатов.</span><span class="sxs-lookup"><span data-stu-id="fb174-114">In most applications using async will have no noticeable benefits and even could be detrimental.</span></span> <span data-ttu-id="fb174-115">Использование тестов, профилирования и здравый смысл, чтобы оценить влияние асинхронного программирования в конкретной ситуации, прежде чем зафиксировать его.</span><span class="sxs-lookup"><span data-stu-id="fb174-115">Use tests, profiling and common sense to measure the impact of async in your particular scenario before committing to it.</span></span>

<span data-ttu-id="fb174-116">Ниже приведены некоторые дополнительные ресурсы, чтобы узнать о async:</span><span class="sxs-lookup"><span data-stu-id="fb174-116">Here are some more resources to learn about async:</span></span>

-   [<span data-ttu-id="fb174-117">Общие сведения о Brandon Bray async/await в .NET 4.5</span><span class="sxs-lookup"><span data-stu-id="fb174-117">Brandon Bray’s overview of async/await in .NET 4.5</span></span>](https://blogs.msdn.com/b/dotnet/archive/2012/04/03/async-in-4-5-worth-the-await.aspx)
-   <span data-ttu-id="fb174-118">[Асинхронное программирование](https://msdn.microsoft.com/library/hh191443.aspx) страниц в библиотеке MSDN</span><span class="sxs-lookup"><span data-stu-id="fb174-118">[Asynchronous Programming](https://msdn.microsoft.com/library/hh191443.aspx) pages in the MSDN Library</span></span>
-   <span data-ttu-id="fb174-119">[Как построить ASP.NET Web приложений с помощью Async](http://channel9.msdn.com/events/teched/northamerica/2013/dev-b337) (включает в себя демонстрацию пропускной способности сервера)</span><span class="sxs-lookup"><span data-stu-id="fb174-119">[How to Build ASP.NET Web Applications Using Async](http://channel9.msdn.com/events/teched/northamerica/2013/dev-b337) (includes a demo of increased server throughput)</span></span>

## <a name="create-the-model"></a><span data-ttu-id="fb174-120">Создание модели</span><span class="sxs-lookup"><span data-stu-id="fb174-120">Create the model</span></span>

<span data-ttu-id="fb174-121">Мы будем использовать [код первого рабочего процесса](~/ef6/modeling/code-first/workflows/new-database.md) для создания нашей модели и создания базы данных, однако асинхронные функции будет работать со всеми моделями EF, включая созданные в конструкторе EF.</span><span class="sxs-lookup"><span data-stu-id="fb174-121">We’ll be using the [Code First workflow](~/ef6/modeling/code-first/workflows/new-database.md) to create our model and generate the database, however the asynchronous functionality will work with all EF models including those created with the EF Designer.</span></span>

-   <span data-ttu-id="fb174-122">Создайте консольное приложение и присвойте ему **AsyncDemo**</span><span class="sxs-lookup"><span data-stu-id="fb174-122">Create a Console Application and call it **AsyncDemo**</span></span>
-   <span data-ttu-id="fb174-123">Добавление пакета EntityFramework NuGet</span><span class="sxs-lookup"><span data-stu-id="fb174-123">Add the EntityFramework NuGet package</span></span>
    -   <span data-ttu-id="fb174-124">В обозревателе решений щелкните правой кнопкой мыши **AsyncDemo** проекта</span><span class="sxs-lookup"><span data-stu-id="fb174-124">In Solution Explorer, right-click on the **AsyncDemo** project</span></span>
    -   <span data-ttu-id="fb174-125">Выберите **управление пакетами NuGet...**</span><span class="sxs-lookup"><span data-stu-id="fb174-125">Select **Manage NuGet Packages…**</span></span>
    -   <span data-ttu-id="fb174-126">В диалоговом окне «Управление пакетами NuGet» выберите **Online** вкладку и выберите **EntityFramework** пакета</span><span class="sxs-lookup"><span data-stu-id="fb174-126">In the Manage NuGet Packages dialog, Select the **Online** tab and choose the **EntityFramework** package</span></span>
    -   <span data-ttu-id="fb174-127">Нажмите кнопку **Установить**</span><span class="sxs-lookup"><span data-stu-id="fb174-127">Click **Install**</span></span>
-   <span data-ttu-id="fb174-128">Добавить **Model.cs** класс следующей реализацией</span><span class="sxs-lookup"><span data-stu-id="fb174-128">Add a **Model.cs** class with the following implementation</span></span>

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

 

## <a name="create-a-synchronous-program"></a><span data-ttu-id="fb174-129">Создание синхронная программа</span><span class="sxs-lookup"><span data-stu-id="fb174-129">Create a synchronous program</span></span>

<span data-ttu-id="fb174-130">Теперь, когда у нас есть модель EF, давайте напишем код, использует его для выполнения некоторых доступа к данным.</span><span class="sxs-lookup"><span data-stu-id="fb174-130">Now that we have an EF model, let's write some code that uses it to perform some data access.</span></span>

-   <span data-ttu-id="fb174-131">Замените содержимое файла **Program.cs** следующим кодом</span><span class="sxs-lookup"><span data-stu-id="fb174-131">Replace the contents of **Program.cs** with the following code</span></span>

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

<span data-ttu-id="fb174-132">Этот код вызывает **PerformDatabaseOperations** метод, который сохраняет новый **блог** в базу данных и затем извлекает все **блоги** из базы данных и выводит их на **Консоли**.</span><span class="sxs-lookup"><span data-stu-id="fb174-132">This code calls the **PerformDatabaseOperations** method which saves a new **Blog** to the database and then retrieves all **Blogs** from the database and prints them to the **Console**.</span></span> <span data-ttu-id="fb174-133">После этого программа записывает знак кавычек дня для **консоли**.</span><span class="sxs-lookup"><span data-stu-id="fb174-133">After this, the program writes a quote of the day to the **Console**.</span></span>

<span data-ttu-id="fb174-134">Так как код является синхронным, мы заметили следующий поток выполнения, при запуске программы:</span><span class="sxs-lookup"><span data-stu-id="fb174-134">Since the code is synchronous, we can observe the following execution flow when we run the program:</span></span>

1.  <span data-ttu-id="fb174-135">**SaveChanges** начинает отправлять новый **блог** к базе данных</span><span class="sxs-lookup"><span data-stu-id="fb174-135">**SaveChanges** begins to push the new **Blog** to the database</span></span>
2.  <span data-ttu-id="fb174-136">**SaveChanges** завершения</span><span class="sxs-lookup"><span data-stu-id="fb174-136">**SaveChanges** completes</span></span>
3.  <span data-ttu-id="fb174-137">Запрос для всех **блоги** отправляется в базу данных</span><span class="sxs-lookup"><span data-stu-id="fb174-137">Query for all **Blogs** is sent to the database</span></span>
4.  <span data-ttu-id="fb174-138">Запрос возвращает и результаты записываются в **консоли**</span><span class="sxs-lookup"><span data-stu-id="fb174-138">Query returns and results are written to **Console**</span></span>
5.  <span data-ttu-id="fb174-139">Цитата дня записывается **консоли**</span><span class="sxs-lookup"><span data-stu-id="fb174-139">Quote of the day is written to **Console**</span></span>

![Выходные данные для синхронизации](~/ef6/media/syncoutput.png) 

 

## <a name="making-it-asynchronous"></a><span data-ttu-id="fb174-141">Воплощение в асинхронный</span><span class="sxs-lookup"><span data-stu-id="fb174-141">Making it asynchronous</span></span>

<span data-ttu-id="fb174-142">Теперь, когда у нас есть нашей программы работу программ, мы можем начать использовать новый асинхронного программирования и ключевые слова ожидания.</span><span class="sxs-lookup"><span data-stu-id="fb174-142">Now that we have our program up and running, we can begin making use of the new async and await keywords.</span></span> <span data-ttu-id="fb174-143">Мы внесли следующие изменения в файл Program.cs</span><span class="sxs-lookup"><span data-stu-id="fb174-143">We've made the following changes to Program.cs</span></span>

1.  <span data-ttu-id="fb174-144">Строка 2: Оператор using для **System.Data.Entity** пространство имен дает доступ к методам расширения async EF.</span><span class="sxs-lookup"><span data-stu-id="fb174-144">Line 2: The using statement for the **System.Data.Entity** namespace gives us access to the EF async extension methods.</span></span>
2.  <span data-ttu-id="fb174-145">Строка 4: Оператор using для **System.Threading.Tasks** пространства имен позволяет использовать **задачи** типа.</span><span class="sxs-lookup"><span data-stu-id="fb174-145">Line 4: The using statement for the **System.Threading.Tasks** namespace allows us to use the **Task** type.</span></span>
3.  <span data-ttu-id="fb174-146">Строка 12 & 18: Мы записи как задачу, которая отслеживает ход выполнения **PerformSomeDatabaseOperations** (строка 12) и затем блокирует выполнение программы для этой задачи до завершения одного раза всю работу для программы создается (строка 18).</span><span class="sxs-lookup"><span data-stu-id="fb174-146">Line 12 & 18: We are capturing as task that monitors the progress of **PerformSomeDatabaseOperations** (line 12) and then blocking program execution for this task to complete once all the work for the program is done (line 18).</span></span>
4.  <span data-ttu-id="fb174-147">Строка 25: Мы улучшили обновления **PerformSomeDatabaseOperations** были помечены как **async** и вернуть **задачи**.</span><span class="sxs-lookup"><span data-stu-id="fb174-147">Line 25: We've update **PerformSomeDatabaseOperations** to be marked as **async** and return a **Task**.</span></span>
5.  <span data-ttu-id="fb174-148">Строка 35: Теперь мы вызова асинхронная версия метода SaveChanges и ожидает его завершения.</span><span class="sxs-lookup"><span data-stu-id="fb174-148">Line 35: We're now calling the Async version of SaveChanges and awaiting it's completion.</span></span>
6.  <span data-ttu-id="fb174-149">Строки 42: Теперь мы звоним асинхронная версия ToList и ожидает от результата.</span><span class="sxs-lookup"><span data-stu-id="fb174-149">Line 42: We're now calling hte Async version of ToList and awaiting on the result.</span></span>

<span data-ttu-id="fb174-150">Полный список методов доступные расширения в пространстве имен System.Data.Entity ссылки на класс QueryableExtensions.</span><span class="sxs-lookup"><span data-stu-id="fb174-150">For a comprehensive list of available extension methods in the System.Data.Entity namespace, refer to the QueryableExtensions class.</span></span> <span data-ttu-id="fb174-151">*Также необходимо добавить «использование System.Data.Entity» с помощью инструкций.*</span><span class="sxs-lookup"><span data-stu-id="fb174-151">*You’ll also need to add “using System.Data.Entity” to your using statements.*</span></span>

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

<span data-ttu-id="fb174-152">Теперь, когда код выполняется асинхронно, мы можно наблюдать потоку выполнения при запуске программы:</span><span class="sxs-lookup"><span data-stu-id="fb174-152">Now that the code is asyncronous, we can observe a different execution flow when we run the program:</span></span>

1.  <span data-ttu-id="fb174-153">**SaveChanges** начинает отправлять новый **блог** к базе данных *после отправки команды в базу данных больше нет вычислений, время, необходимое на текущего управляемого потока. **PerformDatabaseOperations** метод возвращает (даже если он еще не завершено выполнение) и продолжается выполнением программы в методе Main.*</span><span class="sxs-lookup"><span data-stu-id="fb174-153">**SaveChanges** begins to push the new **Blog** to the database *Once the command is sent to the database no more compute time is needed on the current managed thread. The **PerformDatabaseOperations** method returns (even though it hasn't finished executing) and program flow in the Main method continues.*</span></span>
2.  <span data-ttu-id="fb174-154">**Цитата дня записывается в консоль**
    *поскольку больше работы в методе Main, управляемый поток заблокирован на время ожидания вызова до завершения операции базы данных. После завершения оставшейся части нашей **PerformDatabaseOperations** будет выполняться.*</span><span class="sxs-lookup"><span data-stu-id="fb174-154">**Quote of the day is written to Console**
*Since there is no more work to do in the Main method, the managed thread is blocked on the Wait call until the database operation completes. Once it completes, the remainder of our **PerformDatabaseOperations** will be executed.*</span></span>
3.  <span data-ttu-id="fb174-155">**SaveChanges** завершения</span><span class="sxs-lookup"><span data-stu-id="fb174-155">**SaveChanges** completes</span></span>
4.  <span data-ttu-id="fb174-156">Запрос для всех **блоги** отправляется в базу данных *Опять же, управляемый поток может выполнять другую работу, пока запрос обрабатывается в базе данных. Так как все другие выполнения потока программа просто приостанавливает работу во время ожидания вызова хотя.*</span><span class="sxs-lookup"><span data-stu-id="fb174-156">Query for all **Blogs** is sent to the database *Again, the managed thread is free to do other work while the query is processed in the database. Since all other execution has completed, the thread will just halt on the Wait call though.*</span></span>
5.  <span data-ttu-id="fb174-157">Запрос возвращает и результаты записываются в **консоли**</span><span class="sxs-lookup"><span data-stu-id="fb174-157">Query returns and results are written to **Console**</span></span>

![Выходные данные Async](~/ef6/media/asyncoutput.png) 

 

## <a name="the-takeaway"></a><span data-ttu-id="fb174-159">Отсюда вывод:</span><span class="sxs-lookup"><span data-stu-id="fb174-159">The takeaway</span></span>

<span data-ttu-id="fb174-160">Теперь мы увидели, насколько это просто чтобы сделать использование асинхронных методов платформы EF.</span><span class="sxs-lookup"><span data-stu-id="fb174-160">We now saw how easy it is to make use of EF’s asynchronous methods.</span></span> <span data-ttu-id="fb174-161">Несмотря на то, что преимущества async не может быть ясно, с помощью простого консольного приложения, эти же стратегии можно применять в ситуациях, где долго выполняющиеся или привязкой сети действий может в противном случае блокировать приложение или привести к большим числом потоков Увеличьте объем памяти.</span><span class="sxs-lookup"><span data-stu-id="fb174-161">Although the advantages of async may not be very apparent with a simple console app, these same strategies can be applied in situations where long-running or network-bound activities might otherwise block the application, or cause a large number of threads to increase the memory footprint.</span></span>
