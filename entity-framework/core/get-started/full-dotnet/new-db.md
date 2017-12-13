---
title: "Начало работы в .NET Framework — новая база данных — Core EF"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 52b69727-ded9-4a7b-b8d5-73f3acfbbad3
ms.technology: entity-framework-core
uid: core/get-started/full-dotnet/new-db
ms.openlocfilehash: bd7054c6834ae11bfdc352d63654e4304771e432
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/27/2017
---
# <a name="getting-started-with-ef-core-on-net-framework-with-a-new-database"></a>Начало работы с EF Core в .NET Framework с новой базой данных

В этом пошаговом руководстве вы создадите консольное приложение, которое осуществляет базовые операции доступа к базе данных Microsoft SQL Server с помощью Entity Framework. Вы примените процесс миграций, чтобы создать базу данных из модели.

> [!TIP]  
> Для этой статьи вы можете скачать [пример](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/FullNet/ConsoleApp.NewDb) из репозитория GitHub.

## <a name="prerequisites"></a>Предварительные требования

Для прохождения этого пошагового руководства нужны следующие элементы:

* [Visual Studio 2017](https://www.visualstudio.com/downloads/);

* [последняя версия диспетчера пакетов NuGet](https://dist.nuget.org/index.html);

* [последняя версия Windows PowerShell](https://docs.microsoft.com/powershell/scripting/setup/installing-windows-powershell).

## <a name="create-a-new-project"></a>Создание нового проекта

* Откройте Visual Studio.

* Последовательно выберите "Файл" > "Создать" > "Проект".

* В меню слева выберите элементы "Шаблоны" > "Visual C#" > "Классический рабочий стол Windows".

* Выберите шаблон проекта **Консольное приложение (.NET Framework)**.

* Проверьте, что настроено нацеливание на **.NET Framework 4.5.1** или более поздней версии.

* Присвойте проекту имя и щелкните **ОК**.

## <a name="install-entity-framework"></a>Установка Entity Framework

Чтобы использовать EF Core, установите пакеты для поставщиков базы данных, с которыми вы будете работать. В этом пошаговом руководстве используется SQL Server. Список доступных поставщиков вы найдете в разделе [Database Providers](../../providers/index.md) (Поставщики базы данных).

* Последовательно выберите пункты "Средства" -> "Диспетчер пакетов NuGet" -> "Консоль диспетчера пакетов".

* Выполните `Install-Package Microsoft.EntityFrameworkCore.SqlServer`.

Далее в этом пошаговом руководстве мы будем использовать некоторые средства платформы Entity Framework для обслуживания базы данных. Поэтому мы установим пакет средств:

* Выполните `Install-Package Microsoft.EntityFrameworkCore.Tools`.

## <a name="create-your-model"></a>Создание модели

Теперь нам нужно определить контекст и классы сущности, которые входят в модель:

* Выберите элементы "Проект" > "Добавить класс".

* Введите имя класса *Model.cs* и щелкните **ОК**.

* Замените все содержимое этого файла следующим кодом:

<!-- [!code-csharp[Main](samples/core/GetStarted/FullNet/ConsoleApp.NewDb/Model.cs)] -->
``` csharp
using Microsoft.EntityFrameworkCore;
using System.Collections.Generic;

namespace EFGetStarted.ConsoleApp
{
    public class BloggingContext : DbContext
    {
        public DbSet<Blog> Blogs { get; set; }
        public DbSet<Post> Posts { get; set; }

        protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
        {
            optionsBuilder.UseSqlServer(@"Server=(localdb)\mssqllocaldb;Database=EFGetStarted.ConsoleApp.NewDb;Trusted_Connection=True;");
        }
    }

    public class Blog
    {
        public int BlogId { get; set; }
        public string Url { get; set; }

        public List<Post> Posts { get; set; }
    }

    public class Post
    {
        public int PostId { get; set; }
        public string Title { get; set; }
        public string Content { get; set; }

        public int BlogId { get; set; }
        public Blog Blog { get; set; }
    }
}
```

> [!TIP]  
> В реальном приложении каждый класс помещается в отдельный файл, а строка подключения — в файл `App.Config`, откуда она считывается с помощью `ConfigurationManager`. В этом руководстве мы для простоты заносим все данные в один файл кода.

## <a name="create-your-database"></a>Создание базы данных

Итак, модель будет готова, и вы можете создать из нее базу данных с помощью миграций:

* Последовательно выберите пункты "Средства" -> "Диспетчер пакетов NuGet" -> "Консоль диспетчера пакетов".

* Запустите `Add-Migration MyFirstMigration`, чтобы сформировать шаблон миграции для создания начального набора таблиц для модели.

* Запустите `Update-Database`, чтобы применить созданные миграции к базе данных. Поскольку база данных еще не существует, она будет автоматически создана до применения миграции.

> [!TIP]  
> Если позднее вы внесете изменения в эту модель, с помощью команды `Add-Migration` вы сможете сформировать новую миграцию, которая вносит необходимые изменения в схему базы данных. Проверьте сформированный код шаблона (и внесите правки, если потребуется), а затем выполните команду `Update-Database`, чтобы применить изменения к базе данных.
>
>EF использует таблицу `__EFMigrationsHistory` в базе данных, чтобы отслеживать уже примененные к базе данных миграции.

## <a name="use-your-model"></a>Использование модели

Теперь вы можете использовать созданную модель для доступа к данным.

* Откройте файл *Program.cs*.

* Замените все содержимое этого файла следующим кодом:

<!-- [!code-csharp[Main](samples/core/GetStarted/FullNet/ConsoleApp.NewDb/Program.cs)] -->
``` csharp
using System;

namespace EFGetStarted.ConsoleApp
{
    class Program
    {
        static void Main(string[] args)
        {
            using (var db = new BloggingContext())
            {
                db.Blogs.Add(new Blog { Url = "http://blogs.msdn.com/adonet" });
                var count = db.SaveChanges();
                Console.WriteLine("{0} records saved to database", count);

                Console.WriteLine();
                Console.WriteLine("All blogs in database:");
                foreach (var blog in db.Blogs)
                {
                    Console.WriteLine(" - {0}", blog.Url);
                }
            }
        }
    }
}
```

* Выберите "Отладка" -> "Запустить без отладки".

Вы увидите, как в базе данных создается один блог, а затем в консоль выводятся сведения обо всех блогах.

![image](_static/output-new-db.png)
