---
title: Async Query и Save-EF6
description: Асинхронный запрос и сохранение в Entity Framework 6
author: ajcvickers
ms.date: 10/23/2016
uid: ef6/fundamentals/async
ms.openlocfilehash: 77204f56e4dca63322c8ae2e1117318262f16f83
ms.sourcegitcommit: 4860d036ea0fb392c28799907bcc924c987d2d7b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/17/2020
ms.locfileid: "97635735"
---
# <a name="async-query-and-save"></a>Асинхронный запрос и сохранение
> [!NOTE]
> **Только в EF6 и более поздних версиях**. Функции, API и другие возможности, описанные на этой странице, появились в Entity Framework 6. При использовании более ранней версии могут быть неприменимы некоторые или все сведения.

В EF6 появилась поддержка асинхронных запросов и сохранен с использованием [ключевых слов Async и await](https://msdn.microsoft.com/library/vstudio/hh191443.aspx) , которые были введены в .NET 4,5. Хотя не все приложения могут воспользоваться преимуществами асинхронность, его можно использовать для повышения скорости реагирования клиента и масштабируемости сервера при обработке длительных, сетевых или операций ввода-вывода задач.

## <a name="when-to-really-use-async"></a>Когда следует действительно использовать Async

В этом пошаговом руководстве рассматриваются асинхронные концепции, которые позволяют легко отслеживать разницу между асинхронным и синхронным выполнением программы. Это пошаговое руководство не предназначено для иллюстрации основных сценариев, в которых асинхронное программирование дает преимущества.

Асинхронное программирование в основном сосредоточено на высвобождении текущего управляемого потока (потока, выполняющего код .NET) для выполнения другой работы, пока она ожидает операции, не требующей какого-либо времени вычислений из управляемого потока. Например, несмотря на то, что ядро СУБД обрабатывает запрос, в коде .NET ничего делать не нужно.

В клиентских приложениях (WinForms, WPF и т. д.) текущий поток можно использовать для сохранения скорости реагирования пользовательского интерфейса во время выполнения асинхронной операции. В серверных приложениях (ASP.NET и т. д.) поток можно использовать для обработки других входящих запросов. это может сократить использование памяти и (или) увеличить пропускную способность сервера.

В большинстве приложений, использующих async, не имеет заметных преимуществ и даже может быть негативно. Используйте тесты, профилирование и общий смысл для измерения воздействия Async в конкретном сценарии перед фиксацией в нем.

Ниже приведены некоторые дополнительные ресурсы, которые можно изучить асинхронно:

-   [Общие сведения о async/await в Брэндон Брей в .NET 4,5](https://devblogs.microsoft.com/dotnet/async-in-4-5-worth-the-await/)
-   Страницы [асинхронного программирования](https://msdn.microsoft.com/library/hh191443.aspx) в библиотеке MSDN
-   [Создание веб-приложений ASP.NET с помощью Async](https://channel9.msdn.com/events/teched/northamerica/2013/dev-b337) (содержит демонстрацию повышенной пропускной способности сервера)

## <a name="create-the-model"></a>Создание модели

Мы будем использовать [Рабочий процесс Code First](xref:ef6/modeling/code-first/workflows/new-database) для создания нашей модели и создания базы данных, однако асинхронные функции будут работать со всеми моделями EF, включая созданные с помощью конструктора EF.

-   Создание консольного приложения и вызов его **асинкдемо**
-   Добавление пакета NuGet EntityFramework
    -   В обозреватель решений щелкните правой кнопкой мыши проект **асинкдемо**
    -   Выберите **Управление пакетами NuGet...**
    -   В диалоговом окне Управление пакетами NuGet выберите вкладку в **сети** и выберите пакет **EntityFramework** .
    -   Щелкните **Установить**.
-   Добавьте класс **model.CS** со следующей реализацией

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

Теперь, когда у нас есть модель EF, напишем код, который использует ее для доступа к данным.

-   Замените содержимое **Program.CS** следующим кодом.

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

Этот код вызывает `PerformDatabaseOperations` метод, который сохраняет новый **блог** в базе данных, а затем извлекает все **блоги** из базы данных и выводит их на **консоль**. После этого программа записывает в **консоль** кавычки дня.

Так как код является синхронным, при запуске программы мы можем наблюдать за выполнением следующего потока:

1.  `SaveChanges` начинает передачу нового **блога** в базу данных
2.  `SaveChanges` завершения
3.  Запрос всех **блогов** отправлен в базу данных
4.  Возвращаемые запросы и результаты записываются в **консоль**
5.  Цитата дня записывается в **консоль**

![Выходные данные синхронизации](~/ef6/media/syncoutput.png) 

 

## <a name="making-it-asynchronous"></a>Асинхронное выполнение

Теперь, когда наша программа работает, мы можем приступить к использованию новых ключевых слов Async и await. Мы внесли следующие изменения в Program.cs.

1.  Строка 2. оператор using для `System.Data.Entity` пространства имен предоставляет нам доступ к методам расширения EF Async.
2.  Строка 4. оператор using для `System.Threading.Tasks` пространства имен позволяет нам использовать `Task` тип.
3.  Строка 12 & 18. запись выполняется в виде задачи, которая отслеживает ход выполнения `PerformSomeDatabaseOperations` (строка 12), а затем блокирует выполнение программы для выполнения этой задачи после завершения всей работы по программе (строка 18).
4.  Строка 25. Мы получили обновление, `PerformSomeDatabaseOperations` чтобы оно было помечено как `async` и возвращать `Task` .
5.  Строка 35: теперь вызывается асинхронная версия `SaveChanges` и ожидается ее завершение.
6.  Строка 42: теперь вызывается асинхронная версия `ToList` и ожидается результат.

Полный список доступных методов расширения в пространстве имен см. в описании `System.Data.Entity` `QueryableExtensions` класса. *Также необходимо добавить `using System.Data.Entity` в операторы using.*

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

Теперь, когда код является асинхронным, мы можем наблюдать за другим потоком выполнения при запуске программы:

1. `SaveChanges` начинает передачу нового **блога** в базу данных  
    *После отправки команды в базу данных больше не требуется времени вычислений в текущем управляемом потоке. `PerformDatabaseOperations` Метод возвращает значение (несмотря на то, что он не завершил выполнение) и выполнение программы в методе Main продолжится.*
2. **Цитата дня записывается в консоль**  
    *Поскольку больше нет работы в методе Main, управляемый поток блокируется в `Wait` вызове до тех пор, пока не завершится операция базы данных. После завершения работы будет выполнен остальная часть нашего `PerformDatabaseOperations` .*
3.  `SaveChanges` завершения  
4.  Запрос всех **блогов** отправлен в базу данных  
    *Опять же, управляемый поток свободен для выполнения других задач, пока запрос обрабатывается в базе данных. Так как все остальные выполнения завершены, поток просто останавливается на вызове Wait.*
5.  Возвращаемые запросы и результаты записываются в **консоль**  

![Асинхронный вывод](~/ef6/media/asyncoutput.png) 

 

## <a name="the-takeaway"></a>Мысль

Теперь мы увидели, насколько просто использовать асинхронные методы EF. Несмотря на то, что преимущества асинхронного выполнения могут быть не очень очевидны при использовании простого консольного приложения, эти же стратегии могут применяться в ситуациях, когда длительные или сетевые действия могут в противном случае блокировать приложение или вызвать большое количество потоков для увеличения объема памяти.
