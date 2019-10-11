---
title: Async Query и Save-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: d56e6f1d-4bd1-4b50-9558-9a30e04a8ec3
ms.openlocfilehash: 0642dc13e7aa3906fa1495031c62701fc16f0192
ms.sourcegitcommit: 708b18520321c587b2046ad2ea9fa7c48aeebfe5
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/09/2019
ms.locfileid: "72181848"
---
# <a name="async-query-and-save"></a><span data-ttu-id="c8f1b-102">Асинхронный запрос и сохранение</span><span class="sxs-lookup"><span data-stu-id="c8f1b-102">Async query and save</span></span>
> [!NOTE]
> <span data-ttu-id="c8f1b-103">**Только в EF6 и более поздних версиях**. Функции, API и другие возможности, описанные на этой странице, появились в Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="c8f1b-103">**EF6 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 6.</span></span> <span data-ttu-id="c8f1b-104">При использовании более ранней версии могут быть неприменимы некоторые или все сведения.</span><span class="sxs-lookup"><span data-stu-id="c8f1b-104">If you are using an earlier version, some or all of the information does not apply.</span></span>

<span data-ttu-id="c8f1b-105">В EF6 появилась поддержка асинхронных запросов и сохранен с использованием [ключевых слов Async и await](https://msdn.microsoft.com/library/vstudio/hh191443.aspx) , которые были введены в .NET 4,5.</span><span class="sxs-lookup"><span data-stu-id="c8f1b-105">EF6 introduced support for asynchronous query and save using the [async and await keywords](https://msdn.microsoft.com/library/vstudio/hh191443.aspx) that were introduced in .NET 4.5.</span></span> <span data-ttu-id="c8f1b-106">Хотя не все приложения могут воспользоваться преимуществами асинхронность, его можно использовать для повышения скорости реагирования клиента и масштабируемости сервера при обработке длительных, сетевых или операций ввода-вывода задач.</span><span class="sxs-lookup"><span data-stu-id="c8f1b-106">While not all applications may benefit from asynchrony, it can be used to improve client responsiveness and server scalability when handling long-running, network or I/O-bound tasks.</span></span>

## <a name="when-to-really-use-async"></a><span data-ttu-id="c8f1b-107">Когда следует действительно использовать Async</span><span class="sxs-lookup"><span data-stu-id="c8f1b-107">When to really use async</span></span>

<span data-ttu-id="c8f1b-108">В этом пошаговом руководстве рассматриваются асинхронные концепции, которые позволяют легко отслеживать разницу между асинхронным и синхронным выполнением программы.</span><span class="sxs-lookup"><span data-stu-id="c8f1b-108">The purpose of this walkthrough is to introduce the async concepts in a way that makes it easy to observe the difference between asynchronous and synchronous program execution.</span></span> <span data-ttu-id="c8f1b-109">Это пошаговое руководство не предназначено для иллюстрации основных сценариев, в которых асинхронное программирование дает преимущества.</span><span class="sxs-lookup"><span data-stu-id="c8f1b-109">This walkthrough is not intended to illustrate any of the key scenarios where async programming provides benefits.</span></span>

<span data-ttu-id="c8f1b-110">Асинхронное программирование в основном сосредоточено на высвобождении текущего управляемого потока (потока, выполняющего код .NET) для выполнения другой работы, пока она ожидает операции, не требующей какого-либо времени вычислений из управляемого потока.</span><span class="sxs-lookup"><span data-stu-id="c8f1b-110">Async programming is primarily focused on freeing up the current managed thread (thread running .NET code) to do other work while it waits for an operation that does not require any compute time from a managed thread.</span></span> <span data-ttu-id="c8f1b-111">Например, несмотря на то, что ядро СУБД обрабатывает запрос, в коде .NET ничего делать не нужно.</span><span class="sxs-lookup"><span data-stu-id="c8f1b-111">For example, whilst the database engine is processing a query there is nothing to be done by .NET code.</span></span>

<span data-ttu-id="c8f1b-112">В клиентских приложениях (WinForms, WPF и т. д.) текущий поток можно использовать для сохранения скорости реагирования пользовательского интерфейса во время выполнения асинхронной операции.</span><span class="sxs-lookup"><span data-stu-id="c8f1b-112">In client applications (WinForms, WPF, etc.) the current thread can be used to keep the UI responsive while the async operation is performed.</span></span> <span data-ttu-id="c8f1b-113">В серверных приложениях (ASP.NET и т. д.) поток можно использовать для обработки других входящих запросов. это может сократить использование памяти и (или) увеличить пропускную способность сервера.</span><span class="sxs-lookup"><span data-stu-id="c8f1b-113">In server applications (ASP.NET etc.) the thread can be used to process other incoming requests - this can reduce memory usage and/or increase throughput of the server.</span></span>

<span data-ttu-id="c8f1b-114">В большинстве приложений, использующих async, не имеет заметных преимуществ и даже может быть негативно.</span><span class="sxs-lookup"><span data-stu-id="c8f1b-114">In most applications using async will have no noticeable benefits and even could be detrimental.</span></span> <span data-ttu-id="c8f1b-115">Используйте тесты, профилирование и общий смысл для измерения воздействия Async в конкретном сценарии перед фиксацией в нем.</span><span class="sxs-lookup"><span data-stu-id="c8f1b-115">Use tests, profiling and common sense to measure the impact of async in your particular scenario before committing to it.</span></span>

<span data-ttu-id="c8f1b-116">Ниже приведены некоторые дополнительные ресурсы, которые можно изучить асинхронно:</span><span class="sxs-lookup"><span data-stu-id="c8f1b-116">Here are some more resources to learn about async:</span></span>

-   [<span data-ttu-id="c8f1b-117">Общие сведения о async/await в Брэндон Брей в .NET 4,5</span><span class="sxs-lookup"><span data-stu-id="c8f1b-117">Brandon Bray’s overview of async/await in .NET 4.5</span></span>](https://blogs.msdn.com/b/dotnet/archive/2012/04/03/async-in-4-5-worth-the-await.aspx)
-   <span data-ttu-id="c8f1b-118">Страницы [асинхронного программирования](https://msdn.microsoft.com/library/hh191443.aspx) в библиотеке MSDN</span><span class="sxs-lookup"><span data-stu-id="c8f1b-118">[Asynchronous Programming](https://msdn.microsoft.com/library/hh191443.aspx) pages in the MSDN Library</span></span>
-   <span data-ttu-id="c8f1b-119">[Создание веб-приложений ASP.NET с помощью Async](https://channel9.msdn.com/events/teched/northamerica/2013/dev-b337) (содержит демонстрацию повышенной пропускной способности сервера)</span><span class="sxs-lookup"><span data-stu-id="c8f1b-119">[How to Build ASP.NET Web Applications Using Async](https://channel9.msdn.com/events/teched/northamerica/2013/dev-b337) (includes a demo of increased server throughput)</span></span>

## <a name="create-the-model"></a><span data-ttu-id="c8f1b-120">Создание модели</span><span class="sxs-lookup"><span data-stu-id="c8f1b-120">Create the model</span></span>

<span data-ttu-id="c8f1b-121">Мы будем использовать [Рабочий процесс Code First](~/ef6/modeling/code-first/workflows/new-database.md) для создания нашей модели и создания базы данных, однако асинхронные функции будут работать со всеми моделями EF, включая созданные с помощью конструктора EF.</span><span class="sxs-lookup"><span data-stu-id="c8f1b-121">We’ll be using the [Code First workflow](~/ef6/modeling/code-first/workflows/new-database.md) to create our model and generate the database, however the asynchronous functionality will work with all EF models including those created with the EF Designer.</span></span>

-   <span data-ttu-id="c8f1b-122">Создание консольного приложения и вызов его **асинкдемо**</span><span class="sxs-lookup"><span data-stu-id="c8f1b-122">Create a Console Application and call it **AsyncDemo**</span></span>
-   <span data-ttu-id="c8f1b-123">Добавление пакета NuGet EntityFramework</span><span class="sxs-lookup"><span data-stu-id="c8f1b-123">Add the EntityFramework NuGet package</span></span>
    -   <span data-ttu-id="c8f1b-124">В обозреватель решений щелкните правой кнопкой мыши проект **асинкдемо**</span><span class="sxs-lookup"><span data-stu-id="c8f1b-124">In Solution Explorer, right-click on the **AsyncDemo** project</span></span>
    -   <span data-ttu-id="c8f1b-125">Выберите **Управление пакетами NuGet...**</span><span class="sxs-lookup"><span data-stu-id="c8f1b-125">Select **Manage NuGet Packages…**</span></span>
    -   <span data-ttu-id="c8f1b-126">В диалоговом окне Управление пакетами NuGet выберите вкладку в **сети** и выберите пакет **EntityFramework** .</span><span class="sxs-lookup"><span data-stu-id="c8f1b-126">In the Manage NuGet Packages dialog, Select the **Online** tab and choose the **EntityFramework** package</span></span>
    -   <span data-ttu-id="c8f1b-127">Нажмите кнопку **Установить**</span><span class="sxs-lookup"><span data-stu-id="c8f1b-127">Click **Install**</span></span>
-   <span data-ttu-id="c8f1b-128">Добавьте класс **model.CS** со следующей реализацией</span><span class="sxs-lookup"><span data-stu-id="c8f1b-128">Add a **Model.cs** class with the following implementation</span></span>

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

 

## <a name="create-a-synchronous-program"></a><span data-ttu-id="c8f1b-129">Создание синхронной программы</span><span class="sxs-lookup"><span data-stu-id="c8f1b-129">Create a synchronous program</span></span>

<span data-ttu-id="c8f1b-130">Теперь, когда у нас есть модель EF, напишем код, который использует ее для доступа к данным.</span><span class="sxs-lookup"><span data-stu-id="c8f1b-130">Now that we have an EF model, let's write some code that uses it to perform some data access.</span></span>

-   <span data-ttu-id="c8f1b-131">Замените содержимое **Program.CS** следующим кодом.</span><span class="sxs-lookup"><span data-stu-id="c8f1b-131">Replace the contents of **Program.cs** with the following code</span></span>

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

<span data-ttu-id="c8f1b-132">Этот код вызывает метод **перформдатабасеоператионс** , который сохраняет новый **блог** в базе данных, а затем извлекает все **блоги** из базы данных и выводит их на **консоль**.</span><span class="sxs-lookup"><span data-stu-id="c8f1b-132">This code calls the **PerformDatabaseOperations** method which saves a new **Blog** to the database and then retrieves all **Blogs** from the database and prints them to the **Console**.</span></span> <span data-ttu-id="c8f1b-133">После этого программа записывает в **консоль**кавычки дня.</span><span class="sxs-lookup"><span data-stu-id="c8f1b-133">After this, the program writes a quote of the day to the **Console**.</span></span>

<span data-ttu-id="c8f1b-134">Так как код является синхронным, при запуске программы мы можем наблюдать за выполнением следующего потока:</span><span class="sxs-lookup"><span data-stu-id="c8f1b-134">Since the code is synchronous, we can observe the following execution flow when we run the program:</span></span>

1.  <span data-ttu-id="c8f1b-135">Параметр **SaveChanges** начинает отправлять новый **блог** в базу данных</span><span class="sxs-lookup"><span data-stu-id="c8f1b-135">**SaveChanges** begins to push the new **Blog** to the database</span></span>
2.  <span data-ttu-id="c8f1b-136">**SaveChanges** завершается</span><span class="sxs-lookup"><span data-stu-id="c8f1b-136">**SaveChanges** completes</span></span>
3.  <span data-ttu-id="c8f1b-137">Запрос всех **блогов** отправлен в базу данных</span><span class="sxs-lookup"><span data-stu-id="c8f1b-137">Query for all **Blogs** is sent to the database</span></span>
4.  <span data-ttu-id="c8f1b-138">Возвращаемые запросы и результаты записываются в **консоль**</span><span class="sxs-lookup"><span data-stu-id="c8f1b-138">Query returns and results are written to **Console**</span></span>
5.  <span data-ttu-id="c8f1b-139">Цитата дня записывается в **консоль**</span><span class="sxs-lookup"><span data-stu-id="c8f1b-139">Quote of the day is written to **Console**</span></span>

![Выходные данные синхронизации](~/ef6/media/syncoutput.png) 

 

## <a name="making-it-asynchronous"></a><span data-ttu-id="c8f1b-141">Асинхронное выполнение</span><span class="sxs-lookup"><span data-stu-id="c8f1b-141">Making it asynchronous</span></span>

<span data-ttu-id="c8f1b-142">Теперь, когда наша программа работает, мы можем приступить к использованию новых ключевых слов Async и await.</span><span class="sxs-lookup"><span data-stu-id="c8f1b-142">Now that we have our program up and running, we can begin making use of the new async and await keywords.</span></span> <span data-ttu-id="c8f1b-143">Мы внесли следующие изменения в Program.cs.</span><span class="sxs-lookup"><span data-stu-id="c8f1b-143">We've made the following changes to Program.cs</span></span>

1.  <span data-ttu-id="c8f1b-144">Строка 2: Инструкция using для пространства имен **System. Data. Entity** предоставляет нам доступ к методам расширения EF Async.</span><span class="sxs-lookup"><span data-stu-id="c8f1b-144">Line 2: The using statement for the **System.Data.Entity** namespace gives us access to the EF async extension methods.</span></span>
2.  <span data-ttu-id="c8f1b-145">Строка 4: Инструкция using для пространства имен **System. Threading. Tasks** позволяет нам использовать тип **задачи** .</span><span class="sxs-lookup"><span data-stu-id="c8f1b-145">Line 4: The using statement for the **System.Threading.Tasks** namespace allows us to use the **Task** type.</span></span>
3.  <span data-ttu-id="c8f1b-146">Строка 12 & 18: Мы собираемся как задача, которая отслеживает ход выполнения **перформсомедатабасеоператионс** (строка 12), а затем блокирует выполнение программы для выполнения этой задачи после завершения всей работы по программе (строка 18).</span><span class="sxs-lookup"><span data-stu-id="c8f1b-146">Line 12 & 18: We are capturing as task that monitors the progress of **PerformSomeDatabaseOperations** (line 12) and then blocking program execution for this task to complete once all the work for the program is done (line 18).</span></span>
4.  <span data-ttu-id="c8f1b-147">Строка 25: Мы обновляем **перформсомедатабасеоператионс** , чтобы они были помечены как **асинхронные** и возвращали **задачу**.</span><span class="sxs-lookup"><span data-stu-id="c8f1b-147">Line 25: We've update **PerformSomeDatabaseOperations** to be marked as **async** and return a **Task**.</span></span>
5.  <span data-ttu-id="c8f1b-148">Строка 35: Теперь мы вызываем асинхронную версию функции SaveChanges и ожидаем ее завершения.</span><span class="sxs-lookup"><span data-stu-id="c8f1b-148">Line 35: We're now calling the Async version of SaveChanges and awaiting it's completion.</span></span>
6.  <span data-ttu-id="c8f1b-149">Строка 42: Теперь мы вызываем асинхронную версию ToList и ожидаем к результату.</span><span class="sxs-lookup"><span data-stu-id="c8f1b-149">Line 42: We're now calling the Async version of ToList and awaiting on the result.</span></span>

<span data-ttu-id="c8f1b-150">Полный список доступных методов расширения в пространстве имен System. Data. Entity см. в классе Куерябликстенсионс.</span><span class="sxs-lookup"><span data-stu-id="c8f1b-150">For a comprehensive list of available extension methods in the System.Data.Entity namespace, refer to the QueryableExtensions class.</span></span> <span data-ttu-id="c8f1b-151">*Кроме того, необходимо добавить "использование System. Data. Entity" в операторы using.*</span><span class="sxs-lookup"><span data-stu-id="c8f1b-151">*You’ll also need to add “using System.Data.Entity” to your using statements.*</span></span>

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

<span data-ttu-id="c8f1b-152">Теперь, когда код является асинхронным, мы можем наблюдать за другим потоком выполнения при запуске программы:</span><span class="sxs-lookup"><span data-stu-id="c8f1b-152">Now that the code is asynchronous, we can observe a different execution flow when we run the program:</span></span>

1. <span data-ttu-id="c8f1b-153">Параметр **SaveChanges** начинает отправлять новый **блог** в базу данных</span><span class="sxs-lookup"><span data-stu-id="c8f1b-153">**SaveChanges** begins to push the new **Blog** to the database</span></span>  
    <span data-ttu-id="c8f1b-154">*Once. команда отправляется в базу данных. в текущем управляемом потоке не требуется больше времени для вычислений. Метод **перформдатабасеоператионс** возвращает значение (несмотря на то, что он не завершил выполнение) и выполнение программы в методе Main.*</span><span class="sxs-lookup"><span data-stu-id="c8f1b-154">*Once the command is sent to the database no more compute time is needed on the current managed thread. The **PerformDatabaseOperations** method returns (even though it hasn't finished executing) and program flow in the Main method continues.*</span></span>
2. <span data-ttu-id="c8f1b-155">**Цитата дня записывается в консоль**</span><span class="sxs-lookup"><span data-stu-id="c8f1b-155">**Quote of the day is written to Console**</span></span>  
    <span data-ttu-id="c8f1b-156">*Since в методе Main больше не работает, управляемый поток блокируется в вызове Wait до тех пор, пока не завершится операция базы данных. После завершения этого действия будет выполнено оставшаяся часть нашего **перформдатабасеоператионс** .*</span><span class="sxs-lookup"><span data-stu-id="c8f1b-156">*Since there is no more work to do in the Main method, the managed thread is blocked on the Wait call until the database operation completes. Once it completes, the remainder of our **PerformDatabaseOperations** will be executed.*</span></span>
3.  <span data-ttu-id="c8f1b-157">**SaveChanges** завершается</span><span class="sxs-lookup"><span data-stu-id="c8f1b-157">**SaveChanges** completes</span></span>  
4.  <span data-ttu-id="c8f1b-158">Запрос всех **блогов** отправлен в базу данных</span><span class="sxs-lookup"><span data-stu-id="c8f1b-158">Query for all **Blogs** is sent to the database</span></span>  
    <span data-ttu-id="c8f1b-159">@no__t 0Again, управляемый поток может выполнять другие действия, пока запрос обрабатывается в базе данных. Так как все остальные выполнения завершены, поток просто останавливается на вызове Wait. \*</span><span class="sxs-lookup"><span data-stu-id="c8f1b-159">*Again, the managed thread is free to do other work while the query is processed in the database. Since all other execution has completed, the thread will just halt on the Wait call though.*</span></span>
5.  <span data-ttu-id="c8f1b-160">Возвращаемые запросы и результаты записываются в **консоль**</span><span class="sxs-lookup"><span data-stu-id="c8f1b-160">Query returns and results are written to **Console**</span></span>  

![Асинхронный вывод](~/ef6/media/asyncoutput.png) 

 

## <a name="the-takeaway"></a><span data-ttu-id="c8f1b-162">Мысль</span><span class="sxs-lookup"><span data-stu-id="c8f1b-162">The takeaway</span></span>

<span data-ttu-id="c8f1b-163">Теперь мы увидели, насколько просто использовать асинхронные методы EF.</span><span class="sxs-lookup"><span data-stu-id="c8f1b-163">We now saw how easy it is to make use of EF’s asynchronous methods.</span></span> <span data-ttu-id="c8f1b-164">Несмотря на то, что преимущества асинхронного выполнения могут быть не очень очевидны при использовании простого консольного приложения, эти же стратегии могут применяться в ситуациях, когда длительные или сетевые действия могут в противном случае блокировать приложение или вызвать большое количество потоков. Увеличьте объем памяти.</span><span class="sxs-lookup"><span data-stu-id="c8f1b-164">Although the advantages of async may not be very apparent with a simple console app, these same strategies can be applied in situations where long-running or network-bound activities might otherwise block the application, or cause a large number of threads to increase the memory footprint.</span></span>
