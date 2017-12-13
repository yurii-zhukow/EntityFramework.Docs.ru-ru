---
title: "Начало работы в .NET Framework — существующая база данных — EF Core"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: a29a3d97-b2d8-4d33-9475-40ac67b3b2c6
ms.technology: entity-framework-core
uid: core/get-started/full-dotnet/existing-db
ms.openlocfilehash: 3cd69109e3cf8dbc103f9eea6e2553df17f29a98
ms.sourcegitcommit: 5e2d97e731f975cf3405ff3deab2a3c75ad1b969
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/15/2017
---
# <a name="getting-started-with-ef-core-on-net-framework-with-an-existing-database"></a><span data-ttu-id="40f3c-102">Начало работы с EF Core в .NET Framework с существующей базой данных</span><span class="sxs-lookup"><span data-stu-id="40f3c-102">Getting started with EF Core on .NET Framework with an Existing Database</span></span>

<span data-ttu-id="40f3c-103">В этом пошаговом руководстве вы создадите консольное приложение, которое осуществляет базовые операции доступа к базе данных Microsoft SQL Server с помощью Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="40f3c-103">In this walkthrough, you will build a console application that performs basic data access against a Microsoft SQL Server database using Entity Framework.</span></span> <span data-ttu-id="40f3c-104">Чтобы создать модель Entity Framework для существующей базы данных, мы применим метод реконструирования.</span><span class="sxs-lookup"><span data-stu-id="40f3c-104">You will use reverse engineering to create an Entity Framework model based on an existing database.</span></span>

> [!TIP]  
> <span data-ttu-id="40f3c-105">Для этой статьи вы можете скачать [пример](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/FullNet/ConsoleApp.ExistingDb) из репозитория GitHub.</span><span class="sxs-lookup"><span data-stu-id="40f3c-105">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/FullNet/ConsoleApp.ExistingDb) on GitHub.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="40f3c-106">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="40f3c-106">Prerequisites</span></span>

<span data-ttu-id="40f3c-107">Для прохождения этого пошагового руководства нужны следующие элементы:</span><span class="sxs-lookup"><span data-stu-id="40f3c-107">The following prerequisites are needed to complete this walkthrough:</span></span>

* [<span data-ttu-id="40f3c-108">Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="40f3c-108">Visual Studio 2017</span></span>](https://www.visualstudio.com/downloads/)

* <span data-ttu-id="40f3c-109">[последняя версия диспетчера пакетов NuGet](https://dist.nuget.org/index.html);</span><span class="sxs-lookup"><span data-stu-id="40f3c-109">[Latest version of NuGet Package Manager](https://dist.nuget.org/index.html)</span></span>

* <span data-ttu-id="40f3c-110">[последняя версия Windows PowerShell](https://docs.microsoft.com/powershell/scripting/setup/installing-windows-powershell);</span><span class="sxs-lookup"><span data-stu-id="40f3c-110">[Latest version of Windows PowerShell](https://docs.microsoft.com/powershell/scripting/setup/installing-windows-powershell)</span></span>

* <span data-ttu-id="40f3c-111">[база данных для ведения блогов](#blogging-database).</span><span class="sxs-lookup"><span data-stu-id="40f3c-111">[Blogging database](#blogging-database)</span></span>

### <a name="blogging-database"></a><span data-ttu-id="40f3c-112">База данных для ведения блогов</span><span class="sxs-lookup"><span data-stu-id="40f3c-112">Blogging database</span></span>

<span data-ttu-id="40f3c-113">В этом руководстве в качестве существующей базы данных используется база данных **для ведения блогов**, размещенная на локальном экземпляре LocalDb.</span><span class="sxs-lookup"><span data-stu-id="40f3c-113">This tutorial uses a **Blogging** database on your LocalDb instance as the existing database.</span></span>

> [!TIP]  
> <span data-ttu-id="40f3c-114">Если вы уже создали эту базу данных **для ведения блогов**, работая с другим руководством, можете пропустить эти шаги.</span><span class="sxs-lookup"><span data-stu-id="40f3c-114">If you have already created the **Blogging** database as part of another tutorial, you can skip these steps.</span></span>

* <span data-ttu-id="40f3c-115">Откройте Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="40f3c-115">Open Visual Studio</span></span>

* <span data-ttu-id="40f3c-116">Выберите "Инструменты" -> "Подключиться к базе данных...".</span><span class="sxs-lookup"><span data-stu-id="40f3c-116">Tools > Connect to Database...</span></span>

* <span data-ttu-id="40f3c-117">Выберите **Microsoft SQL Server** и щелкните **Продолжить**.</span><span class="sxs-lookup"><span data-stu-id="40f3c-117">Select **Microsoft SQL Server** and click **Continue**</span></span>

* <span data-ttu-id="40f3c-118">Введите значение **(localdb)\mssqllocaldb** для параметра **Имя сервера**.</span><span class="sxs-lookup"><span data-stu-id="40f3c-118">Enter **(localdb)\mssqllocaldb** as the **Server Name**</span></span>

* <span data-ttu-id="40f3c-119">Введите значение **master** для параметра **Имя базы данных**, затем щелкните **ОК**.</span><span class="sxs-lookup"><span data-stu-id="40f3c-119">Enter **master** as the **Database Name** and click **OK**</span></span>

* <span data-ttu-id="40f3c-120">Теперь база данных master появится в разделе **Подключения к данным** в **обозревателе сервера**.</span><span class="sxs-lookup"><span data-stu-id="40f3c-120">The master database is now displayed under **Data Connections** in **Server Explorer**</span></span>

* <span data-ttu-id="40f3c-121">Щелкните правой кнопкой мыши базу данных в **обозревателе сервера** и выберите действие **Создать запрос**.</span><span class="sxs-lookup"><span data-stu-id="40f3c-121">Right-click on the database in **Server Explorer** and select **New Query**</span></span>

* <span data-ttu-id="40f3c-122">Скопируйте представленный ниже скрипт и вставьте его в редактор запросов.</span><span class="sxs-lookup"><span data-stu-id="40f3c-122">Copy the script, listed below, into the query editor</span></span>

* <span data-ttu-id="40f3c-123">Щелкните область редактора запросов правой кнопкой мыши и выберите действие **Выполнить**.</span><span class="sxs-lookup"><span data-stu-id="40f3c-123">Right-click on the query editor and select **Execute**</span></span>

[!code-sql[Main](../_shared/create-blogging-database-script.sql)]

## <a name="create-a-new-project"></a><span data-ttu-id="40f3c-124">Создание нового проекта</span><span class="sxs-lookup"><span data-stu-id="40f3c-124">Create a new project</span></span>

* <span data-ttu-id="40f3c-125">Откройте Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="40f3c-125">Open Visual Studio</span></span>

* <span data-ttu-id="40f3c-126">Последовательно выберите "Файл" > "Создать" > "Проект".</span><span class="sxs-lookup"><span data-stu-id="40f3c-126">File > New > Project...</span></span>

* <span data-ttu-id="40f3c-127">В меню слева выберите "Шаблоны" > Visual C# > Windows.</span><span class="sxs-lookup"><span data-stu-id="40f3c-127">From the left menu select Templates > Visual C# > Windows</span></span>

* <span data-ttu-id="40f3c-128">Выберите шаблон проекта **Консольное приложение**.</span><span class="sxs-lookup"><span data-stu-id="40f3c-128">Select the **Console Application** project template</span></span>

* <span data-ttu-id="40f3c-129">Проверьте, что настроено нацеливание на **.NET Framework 4.5.1** или более поздней версии.</span><span class="sxs-lookup"><span data-stu-id="40f3c-129">Ensure you are targeting **.NET Framework 4.5.1** or later</span></span>

* <span data-ttu-id="40f3c-130">Присвойте проекту имя и щелкните **ОК**.</span><span class="sxs-lookup"><span data-stu-id="40f3c-130">Give the project a name and click **OK**</span></span>

## <a name="install-entity-framework"></a><span data-ttu-id="40f3c-131">Установка Entity Framework</span><span class="sxs-lookup"><span data-stu-id="40f3c-131">Install Entity Framework</span></span>

<span data-ttu-id="40f3c-132">Чтобы использовать EF Core, установите пакеты для поставщиков базы данных, с которыми вы будете работать.</span><span class="sxs-lookup"><span data-stu-id="40f3c-132">To use EF Core, install the package for the database provider(s) you want to target.</span></span> <span data-ttu-id="40f3c-133">В этом пошаговом руководстве используется SQL Server.</span><span class="sxs-lookup"><span data-stu-id="40f3c-133">This walkthrough uses SQL Server.</span></span> <span data-ttu-id="40f3c-134">Список доступных поставщиков вы найдете в разделе [Database Providers](../../providers/index.md) (Поставщики базы данных).</span><span class="sxs-lookup"><span data-stu-id="40f3c-134">For a list of available providers see [Database Providers](../../providers/index.md).</span></span>

* <span data-ttu-id="40f3c-135">Последовательно выберите пункты "Средства" -> "Диспетчер пакетов NuGet" -> "Консоль диспетчера пакетов".</span><span class="sxs-lookup"><span data-stu-id="40f3c-135">Tools > NuGet Package Manager > Package Manager Console</span></span>

* <span data-ttu-id="40f3c-136">Выполните `Install-Package Microsoft.EntityFrameworkCore.SqlServer`.</span><span class="sxs-lookup"><span data-stu-id="40f3c-136">Run `Install-Package Microsoft.EntityFrameworkCore.SqlServer`</span></span>

<span data-ttu-id="40f3c-137">Чтобы применить реконструирование из существующей базы данных, нам нужно установить еще несколько пакетов.</span><span class="sxs-lookup"><span data-stu-id="40f3c-137">To enable reverse engineering from an existing database we need to install a couple of other packages too.</span></span>

* <span data-ttu-id="40f3c-138">Выполните `Install-Package Microsoft.EntityFrameworkCore.Tools`.</span><span class="sxs-lookup"><span data-stu-id="40f3c-138">Run `Install-Package Microsoft.EntityFrameworkCore.Tools`</span></span>

## <a name="reverse-engineer-your-model"></a><span data-ttu-id="40f3c-139">Реконструирование модели</span><span class="sxs-lookup"><span data-stu-id="40f3c-139">Reverse engineer your model</span></span>

<span data-ttu-id="40f3c-140">Теперь пора создать модель EF на основе существующей базы данных:</span><span class="sxs-lookup"><span data-stu-id="40f3c-140">Now it's time to create the EF model based on your existing database.</span></span>

* <span data-ttu-id="40f3c-141">Последовательно выберите пункты "Средства" -> "Диспетчер пакетов NuGet" -> "Консоль диспетчера пакетов".</span><span class="sxs-lookup"><span data-stu-id="40f3c-141">Tools –> NuGet Package Manager –> Package Manager Console</span></span>

* <span data-ttu-id="40f3c-142">Выполните следующую команду, чтобы создать модель на основе существующей базы данных.</span><span class="sxs-lookup"><span data-stu-id="40f3c-142">Run the following command to create a model from the existing database</span></span>

``` powershell
Scaffold-DbContext "Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer
```

<span data-ttu-id="40f3c-143">Процесс реконструирования создает классы сущностей и производный контекст на основе схемы существующей базы данных.</span><span class="sxs-lookup"><span data-stu-id="40f3c-143">The reverse engineer process created entity classes and a derived context based on the schema of the existing database.</span></span> <span data-ttu-id="40f3c-144">Классы сущностей — это простые объекты C#, которые представляют данные для использования в запросах и командах сохранения.</span><span class="sxs-lookup"><span data-stu-id="40f3c-144">The entity classes are simple C# objects that represent the data you will be querying and saving.</span></span>

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

<span data-ttu-id="40f3c-145">Контекст соответствует сеансу взаимодействия с базой данных и позволяет запрашивать и сохранять экземпляры классов сущностей.</span><span class="sxs-lookup"><span data-stu-id="40f3c-145">The context represents a session with the database and allows you to query and save instances of the entity classes.</span></span>

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

## <a name="use-your-model"></a><span data-ttu-id="40f3c-146">Использование модели</span><span class="sxs-lookup"><span data-stu-id="40f3c-146">Use your model</span></span>

<span data-ttu-id="40f3c-147">Теперь вы можете использовать созданную модель для доступа к данным.</span><span class="sxs-lookup"><span data-stu-id="40f3c-147">You can now use your model to perform data access.</span></span>

* <span data-ttu-id="40f3c-148">Откройте файл *Program.cs*.</span><span class="sxs-lookup"><span data-stu-id="40f3c-148">Open *Program.cs*</span></span>

* <span data-ttu-id="40f3c-149">Замените все содержимое этого файла следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="40f3c-149">Replace the contents of the file with the following code</span></span>

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

* <span data-ttu-id="40f3c-150">Выберите "Отладка" -> "Запустить без отладки".</span><span class="sxs-lookup"><span data-stu-id="40f3c-150">Debug > Start Without Debugging</span></span>

<span data-ttu-id="40f3c-151">Вы увидите, как в базе данных создается один блог, а затем в консоль выводятся сведения обо всех блогах.</span><span class="sxs-lookup"><span data-stu-id="40f3c-151">You will see that one blog is saved to the database and then the details of all blogs are printed to the console.</span></span>

![image](_static/output-existing-db.png)
