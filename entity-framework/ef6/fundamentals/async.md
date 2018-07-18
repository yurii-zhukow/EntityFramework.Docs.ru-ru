---
title: Асинхронные запросы и сохраняет - EF6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: d56e6f1d-4bd1-4b50-9558-9a30e04a8ec3
caps.latest.revision: 3
ms.openlocfilehash: 655676029b3a4e42bbe257ff6091179930b1814e
ms.sourcegitcommit: 390f3a37bc55105ed7cc5b0e0925b7f9c9e80ba6
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/09/2018
ms.locfileid: "39122558"
---
# <a name="async-query-and-save"></a>Асинхронный запрос и сохраните
> [!NOTE]
> **Только в EF6 и более поздних версиях**. Функции, API и другие возможности, описанные на этой странице, появились в Entity Framework 6. При использовании более ранней версии могут быть неприменимы некоторые или все сведения.

EF6 появилась поддержка для асинхронного запроса и сохраните файл с [async и await ключевые слова](https://msdn.microsoft.com/library/vstudio/hh191443.aspx) , которые появились в .NET 4.5. Не все приложения могут выиграть от асинхронность, его можно использовать для повышения масштабируемости клиента времени отклика и сервера при обработке долго выполняющихся, сети или задачи ввода-вывода.

## <a name="when-to-really-use-async"></a>Когда действительно использовать асинхронный

В этом пошаговом руководстве предназначено для ознакомления с основными понятиями async способом, который позволяет легко определить разницу между выполнениями программы асинхронные и синхронные. В этом пошаговом руководстве не предназначен для иллюстрации этих основных сценариев где асинхронного программирования предоставляет преимущества.

Асинхронное программирование в основном ориентировано на освобождение текущему управляемому потоку (поток выполнение кода .NET) может выполнять другие задачи при ожидании операции, которая не требует любое время вычислений из управляемого потока. Например в процессе ядра СУБД обработки запроса нет ничего выполнять код .NET.

В клиентских приложениях (WinForms, WPF и т.д.) текущего потока используется для сохранения отклика пользовательского интерфейса во время выполнения асинхронной операции. В потоке позволяют обрабатывать другие входящие запросы - серверных приложений (ASP.NET и т.д.) это может уменьшить уровень использования памяти и/или увеличить пропускную способность сервера.

В большинстве приложений использование метода async даст без заметного преимущества и даже может привести к ухудшению результатов. Использование тестов, профилирования и здравый смысл, чтобы оценить влияние асинхронного программирования в конкретной ситуации, прежде чем зафиксировать его.

Ниже приведены некоторые дополнительные ресурсы, чтобы узнать о async:

-   [Общие сведения о Brandon Bray async/await в .NET 4.5](http://blogs.msdn.com/b/dotnet/archive/2012/04/03/async-in-4-5-worth-the-await.aspx)
-   [Асинхронное программирование](https://msdn.microsoft.com/library/hh191443.aspx) страниц в библиотеке MSDN
-   [Как построить ASP.NET Web приложений с помощью Async](http://channel9.msdn.com/events/teched/northamerica/2013/dev-b337) (включает в себя демонстрацию пропускной способности сервера)

## <a name="create-the-model"></a>Создание модели

Мы будем использовать [код первого рабочего процесса](~/ef6/modeling/code-first/workflows/new-database.md) для создания нашей модели и создания базы данных, однако асинхронные функции будет работать со всеми моделями EF, включая созданные в конструкторе EF.

-   Создайте консольное приложение и присвойте ему **AsyncDemo**
-   Добавление пакета EntityFramework NuGet
    -   В обозревателе решений щелкните правой кнопкой мыши **AsyncDemo** проекта
    -   Выберите **управление пакетами NuGet...**
    -   В диалоговом окне «Управление пакетами NuGet» выберите **Online** вкладку и выберите **EntityFramework** пакета
    -   Нажмите кнопку **установки**
-   Добавить **Model.cs** класс следующей реализацией

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

 

## <a name="create-a-synchronous-program"></a>Создание синхронная программа

Теперь, когда у нас есть модель EF, давайте напишем код, использует его для выполнения некоторых доступа к данным.

-   Замените содержимое файла **Program.cs** следующим кодом

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

                Console.WriteLine();
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
                    db.SaveChanges();

                    // Query for all blogs ordered by name
                    var blogs = (from b in db.Blogs
                                orderby b.Name
                                select b).ToList();

                    // Write all blogs out to Console
                    Console.WriteLine();
                    Console.WriteLine("All blogs:");
                    foreach (var blog in blogs)
                    {
                        Console.WriteLine(" " + blog.Name);
                    }
                }
            }
        }
    }
```

Этот код вызывает **PerformDatabaseOperations** метод, который сохраняет новый **блог** в базу данных и затем извлекает все **блоги** из базы данных и выводит их на **Консоли**. После этого программа записывает знак кавычек дня для **консоли**.

Поскольку код syncronous, мы заметили следующий поток выполнения, при запуске программы:

1.  **SaveChanges** начинает отправлять новый **блог** к базе данных
2.  **SaveChanges** завершения
3.  Запрос для всех **блоги** отправляется в базу данных
4.  Запрос возвращает и результаты записываются в **консоли**
5.  Цитата дня записывается **консоли**

![SyncOutput](~/ef6/media/syncoutput.png) 

 

## <a name="making-it-asynchronous"></a>Воплощение в асинхронный

Теперь, когда у нас есть нашей программы работу программ, мы можем начать использовать новый асинхронного программирования и ключевые слова ожидания. Мы внесли следующие изменения в файл Program.cs

1.  Строка 2: С помощью инструкции для **System.Data.Entity** пространство имен дает доступ к методам расширения async EF.
2.  Строки 4: С помощью инструкции для **System.Threading.Tasks** пространства имен позволяет использовать **задачи** типа.
3.  Строка 12 & 18: мы записи как задачу, которая отслеживает ход выполнения **PerformSomeDatabaseOperations** (строка 12) и затем блокирует выполнение программы для этой задачи до завершения одного раза всю работу для программы создается (строка 18).
4.  Строка 25: Мы включили обновления **PerformSomeDatabaseOperations** были помечены как **async** и вернуть **задачи**.
5.  Строка 35: Мы теперь вызова асинхронная версия метода SaveChanges и ожидает его завершения.
6.  Строки 42: Мы теперь вызова асинхронная версия ToList и ожидает от результата.

Полный список методов доступные расширения в пространстве имен System.Data.Entity ссылки на класс QueryableExtensions. *Также необходимо добавить «использование System.Data.Entity» с помощью инструкций.*

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

Теперь, когда код выполняется асинхронно, мы можно наблюдать потоку выполнения при запуске программы:

1.  **SaveChanges** начинает отправлять новый **блог** к базе данных *после отправки команды в базу данных больше нет вычислений, время, необходимое на текущего управляемого потока. **PerformDatabaseOperations** метод возвращает (даже если он еще не завершено выполнение) и продолжается выполнением программы в методе Main.*
2.  **Цитата дня записывается в консоль**
    *поскольку больше работы в методе Main, управляемый поток заблокирован на время ожидания вызова до завершения операции базы данных. После завершения оставшейся части нашей **PerformDatabaseOperations** * будет выполняться.
3.  **SaveChanges** завершения
4.  Запрос для всех **блоги** отправляется в базу данных *Опять же, управляемый поток может выполнять другую работу, пока запрос обрабатывается в базе данных. Так как все другие выполнения потока программа просто приостанавливает работу во время ожидания вызова хотя.*
5.  Запрос возвращает и результаты записываются в **консоли**

![AsyncOutput](~/ef6/media/asyncoutput.png) 

 

## <a name="the-takeaway"></a>Отсюда вывод:

Теперь мы увидели, насколько это просто чтобы сделать использование асинхронных методов платформы EF. Несмотря на то, что преимущества async не может быть ясно, с помощью простого консольного приложения, эти же стратегии можно применять в ситуациях, где долго выполняющиеся или привязкой сети действий может в противном случае блокировать приложение или привести к большим числом потоков Увеличьте объем памяти.
