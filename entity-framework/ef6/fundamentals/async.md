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
# <a name="async-query-and-save"></a>Запрос и сохранение Async
> [!NOTE]
> **Только в EF6 и более поздних версиях**. Функции, API и другие возможности, описанные на этой странице, появились в Entity Framework 6. При использовании более ранней версии могут быть неприменимы некоторые или все сведения.

EF6 ввела поддержку асинхронного запроса и сохранения с помощью [async и ждать ключевых слов,](https://msdn.microsoft.com/library/vstudio/hh191443.aspx) которые были введены в .NET 4.5. Хотя не все приложения могут извлечь выгоду из асинхронности, он может быть использован для улучшения отзывчивости клиентов и масштабируемости сервера при обработке длительных, сетевых или вводом-связанных задач.

## <a name="when-to-really-use-async"></a>Когда действительно использовать async

Цель этого пошагового представления состоит в том, чтобы ввести понятия async таким образом, чтобы можно было легко наблюдать разницу между асинхронным и синхронным выполнением программы. Это пошаговое решение не предназначено для иллюстрации любого из ключевых сценариев, в которых программирование async дает преимущества.

Программирование Async в первую очередь сосредоточено на высвобождении текущего управляемого потока (поток, выполняющий код .NET) для выполнения другой работы, пока он ждет операции, не требующей времени вычисления от управляемого потока. Например, в то время как движок базы данных обрабатывает запрос, код .NET ничего не может сделать.

В клиентских приложениях (WinForms, WPF и т.д.) текущий поток может использоваться для поддержания реакции в ашего мониторе во время выполнения операции async. В серверных приложениях (ASP.NET и т.д.) поток может использоваться для обработки других входящих запросов - это может уменьшить использование памяти и/или увеличить пропускную стоимость сервера.

В большинстве приложений с использованием async не будет иметь заметных преимуществ и даже может быть пагубным. Используйте тесты, профилирование и здравый смысл для измерения влияния async в вашем конкретном сценарии, прежде чем совершить его.

Вот еще несколько ресурсов, чтобы узнать о async:

-   [Обзор Брэндон Брей async / ждать в .NET 4.5](https://devblogs.microsoft.com/dotnet/async-in-4-5-worth-the-await/)
-   [Асинхронные страницы программирования](https://msdn.microsoft.com/library/hh191443.aspx) в библиотеке MSDN
-   [Как построить ASP.NET веб-приложений с помощью Async](https://channel9.msdn.com/events/teched/northamerica/2013/dev-b337) (включает демонстрацию увеличенной пропускной записи сервера)

## <a name="create-the-model"></a>Создание модели

Мы будем использовать [рабочий процесс Code First](~/ef6/modeling/code-first/workflows/new-database.md) для создания нашей модели и создания базы данных, однако асинхронная функциональность будет работать со всеми моделями EF, включая те, которые созданы с EF Designer.

-   Создайте консольное приложение и назовите его **AsyncDemo**
-   Добавить пакет EntityFramework NuGet
    -   В Solution Explorer нажмите правой кнопкой мыши на **проекте AsyncDemo**
    -   Выберите **Пакеты Управления NuGet...**
    -   В диалоге пакетов Управления NuGet выберите вкладку **Online** и выберите пакет **EntityFramework**
    -   Нажмите **Установить**
-   Добавьте **Model.cs** класс со следующей реализацией

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

 

## <a name="create-a-synchronous-program"></a>Создание синхронной программы

Теперь, когда у нас есть модель EF, давайте напишем код, который использует ее для выполнения некоторых данных.

-   Замените содержимое **Program.cs** следующим кодом

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

Этот код вызывает метод **PerformDatabaseOperations,** который сохраняет новый **блог** в базу данных, а затем извлекает все **блоги** из базы данных и печатает их на **консоль.** После этого программа пишет цитату дня на **консоль**.

Поскольку код синхронный, мы можем наблюдать следующий поток выполнения при запуске программы:

1.  **SaveChanges** начинает толкать новый **блог** в базу данных
2.  **SaveChanges** завершается
3.  Запрос для всех **блогов** отправляется в базу данных
4.  Возвраты запросов и результаты записываются в **Console**
5.  Цитата дня написана на **консоли**

![Выход синхронизации](~/ef6/media/syncoutput.png) 

 

## <a name="making-it-asynchronous"></a>Создание асинхронного

Теперь, когда у нас есть наша программа и работает, мы можем начать использовать новые async и ждать ключевых слов. Мы внесли следующие изменения в Program.cs

1.  Строка 2: Использование оператора для пространства имен **System.Data.Entity** дает нам доступ к методам расширения EF async.
2.  Строка 4: Использование оператора для пространства имен **System.Threading.Tasks** позволяет нам использовать тип **задачи.**
3.  Строка 12 & 18: Мы захватываем как задачу, которая отслеживает ход **ВыполненияSomeDatabaseOperations** (строка 12), а затем блокируем выполнение программы для выполнения этой задачи после выполнения всей работы для программы (линия 18).
4.  Строка 25: Мы обновляем **PerformSomeDatabaseOperations,** чтобы быть помечены как **async** и вернуть **задачу**.
5.  Строка 35: Мы сейчас вызываем версию Async SaveChanges и ожидаем ее завершения.
6.  Строка 42: Мы сейчас вызываем версию Async ToList и ждем результата.

Для получения полного списка доступных методов расширения в пространстве имен System.Data.Entity обратитесь к классу queryableExtensions. *Вам также нужно будет добавить "использование System.Data.Entity" к вашим сиюиным заявлениям.*

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

Теперь, когда код асинхронный, мы можем наблюдать другой поток выполнения при запуске программы:

1. **SaveChanges** начинает толкать новый **блог** в базу данных  
    *После отправки команды в базу данных больше времени вычислений в текущем управляемом потоке не требуется. Возвращается метод **PerformDatabaseOperations** (даже если он еще не закончил выполнение) и поток программы в основном методе продолжается.*
2. **Цитата дня написана на консоли**  
    *Поскольку в основном методе больше нет работы, управляемый поток блокируется на вызове Ожидания до завершения операции базы данных. Как только он завершится, остальная часть наших **операций PerformDatabaseбудет** будет выполнена.*
3.  **SaveChanges** завершается  
4.  Запрос для всех **блогов** отправляется в базу данных  
    *Опять же, управляемый поток может выполнять другую работу, пока запрос обрабатывается в базе данных. Так как все другие выполнения завершена, поток будет просто остановить на вызов ожидания, хотя.*
5.  Возвраты запросов и результаты записываются в **Console**  

![Выход Async](~/ef6/media/asyncoutput.png) 

 

## <a name="the-takeaway"></a>Вынос

Теперь мы увидели, как легко использовать асинхронные методы EF. Хотя преимущества async могут быть не очень очевидными с помощью простого консольного приложения, эти же стратегии могут применяться в ситуациях, когда длительные или сетевые действия могут в противном случае заблокировать приложение или привести к увеличению количества потоков для увеличения памяти.
