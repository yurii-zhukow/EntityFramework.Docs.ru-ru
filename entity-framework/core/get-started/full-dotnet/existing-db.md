---
title: Начало работы в .NET Framework — существующая база данных — EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: a29a3d97-b2d8-4d33-9475-40ac67b3b2c6
ms.technology: entity-framework-core
uid: core/get-started/full-dotnet/existing-db
ms.openlocfilehash: 39e77ab8c124df67458cc5fa6db2882b65943ebe
ms.sourcegitcommit: 4467032fd6ca223e5965b59912d74cf88a1dd77f
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/01/2018
ms.locfileid: "39388472"
---
# <a name="getting-started-with-ef-core-on-net-framework-with-an-existing-database"></a>Начало работы с EF Core в .NET Framework с существующей базой данных

В этом пошаговом руководстве вы создадите консольное приложение, которое осуществляет базовые операции доступа к базе данных Microsoft SQL Server с помощью Entity Framework. Чтобы создать модель Entity Framework для существующей базы данных, мы применим метод реконструирования.

> [!TIP]  
> Для этой статьи вы можете скачать [пример](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/FullNet/ConsoleApp.ExistingDb) из репозитория GitHub.

## <a name="prerequisites"></a>Предварительные требования

Для прохождения этого пошагового руководства нужны следующие элементы:

* [Visual Studio 2017](https://www.visualstudio.com/downloads/) — по крайней мере версии 15.3

* [последняя версия диспетчера пакетов NuGet](https://dist.nuget.org/index.html);

* [последняя версия Windows PowerShell](https://docs.microsoft.com/powershell/scripting/setup/installing-windows-powershell);

* [база данных для ведения блогов](#blogging-database).

### <a name="blogging-database"></a>База данных для ведения блогов

В этом руководстве в качестве существующей базы данных используется база данных **для ведения блогов**, размещенная на локальном экземпляре LocalDb.

> [!TIP]  
> Если вы уже создали эту базу данных **для ведения блогов**, работая с другим руководством, можете пропустить эти шаги.

* Открытие Visual Studio

* Выберите "Инструменты" -> "Подключиться к базе данных...".

* Выберите **Microsoft SQL Server** и щелкните **Продолжить**.

* Введите значение **(localdb)\mssqllocaldb** для параметра **Имя сервера**.

* Введите значение **master** для параметра **Имя базы данных**, затем щелкните **ОК**.

* Теперь база данных master появится в разделе **Подключения к данным** в **обозревателе сервера**.

* Щелкните правой кнопкой мыши базу данных в **обозревателе сервера** и выберите действие **Создать запрос**.

* Скопируйте представленный ниже скрипт и вставьте его в редактор запросов.

* Щелкните область редактора запросов правой кнопкой мыши и выберите действие **Выполнить**.

[!code-sql[Main](../_shared/create-blogging-database-script.sql)]

## <a name="create-a-new-project"></a>Создание нового проекта

* Открытие Visual Studio

* Последовательно выберите "Файл" > "Создать" > "Проект".

* В меню слева выберите "Шаблоны" > Visual C# > Windows.

* Выберите шаблон проекта **Консольное приложение**.

* Проверьте, что настроено нацеливание на **.NET Framework 4.6.1** или более поздней версии

* Присвойте проекту имя и щелкните **ОК**.

## <a name="install-entity-framework"></a>Установка Entity Framework

Чтобы использовать EF Core, установите пакеты для поставщиков базы данных, с которыми вы будете работать. В этом пошаговом руководстве используется SQL Server. Список доступных поставщиков вы найдете в разделе [Database Providers](../../providers/index.md) (Поставщики базы данных).

* Последовательно выберите пункты "Средства" -> "Диспетчер пакетов NuGet" -> "Консоль диспетчера пакетов".

* Запуск `Install-Package Microsoft.EntityFrameworkCore.SqlServer`

Чтобы применить реконструирование из существующей базы данных, нам нужно установить еще несколько пакетов.

* Запуск `Install-Package Microsoft.EntityFrameworkCore.Tools`

## <a name="reverse-engineer-your-model"></a>Реконструирование модели

Теперь пора создать модель EF на основе существующей базы данных:

* Последовательно выберите пункты "Средства" -> "Диспетчер пакетов NuGet" -> "Консоль диспетчера пакетов".

* Выполните следующую команду, чтобы создать модель на основе существующей базы данных.

``` powershell
Scaffold-DbContext "Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer
```

Процесс реконструирования создает классы сущностей и производный контекст на основе схемы существующей базы данных. Классы сущностей — это простые объекты C#, которые представляют данные для использования в запросах и командах сохранения.

<!-- [!code-csharp[Main](samples/core/GetStarted/FullNet/ConsoleApp.ExistingDb/Blog.cs)] -->
``` csharp
using System;
using System.Collections.Generic;

namespace EFGetStarted.ConsoleApp.ExistingDb
{
    public partial class Blog
    {
        public Blog()
        {
            Post = new HashSet<Post>();
        }

        public int BlogId { get; set; }
        public string Url { get; set; }

        public virtual ICollection<Post> Post { get; set; }
    }
}
```

Контекст соответствует сеансу взаимодействия с базой данных и позволяет запрашивать и сохранять экземпляры классов сущностей.

<!-- [!code-csharp[Main](samples/core/GetStarted/FullNet/ConsoleApp.ExistingDb/BloggingContext.cs)] -->
``` csharp
using Microsoft.EntityFrameworkCore;
using Microsoft.EntityFrameworkCore.Metadata;

namespace EFGetStarted.ConsoleApp.ExistingDb
{
    public partial class BloggingContext : DbContext
    {
        protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
        {
            #warning To protect potentially sensitive information in your connection string, you should move it out of source code. See http://go.microsoft.com/fwlink/?LinkId=723263 for guidance on storing connection strings.
            optionsBuilder.UseSqlServer(@"Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;");
        }

        protected override void OnModelCreating(ModelBuilder modelBuilder)
        {
            modelBuilder.Entity<Blog>(entity =>
            {
                entity.Property(e => e.Url).IsRequired();
            });

            modelBuilder.Entity<Post>(entity =>
            {
                entity.HasOne(d => d.Blog)
                    .WithMany(p => p.Post)
                    .HasForeignKey(d => d.BlogId);
            });
        }

        public virtual DbSet<Blog> Blog { get; set; }
        public virtual DbSet<Post> Post { get; set; }
    }
}
```

## <a name="use-your-model"></a>Использование модели

Теперь вы можете использовать созданную модель для доступа к данным.

* Откройте файл *Program.cs*.

* Замените все содержимое этого файла следующим кодом:

<!-- [!code-csharp[Main](samples/core/GetStarted/FullNet/ConsoleApp.ExistingDb/Program.cs)] -->
``` csharp
using System;

namespace EFGetStarted.ConsoleApp.ExistingDb
{
    class Program
    {
        static void Main(string[] args)
        {
            using (var db = new BloggingContext())
            {
                db.Blog.Add(new Blog { Url = "http://blogs.msdn.com/adonet" });
                var count = db.SaveChanges();
                Console.WriteLine("{0} records saved to database", count);

                Console.WriteLine();
                Console.WriteLine("All blogs in database:");
                foreach (var blog in db.Blog)
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

![изображение](_static/output-existing-db.png)
