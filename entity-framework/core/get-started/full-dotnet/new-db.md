---
title: Начало работы в .NET Framework — новая база данных — Core EF
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 52b69727-ded9-4a7b-b8d5-73f3acfbbad3
ms.technology: entity-framework-core
uid: core/get-started/full-dotnet/new-db
ms.openlocfilehash: bd7054c6834ae11bfdc352d63654e4304771e432
ms.sourcegitcommit: 507a40ed050fee957bcf8cf05f6e0ec8a3b1a363
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/26/2018
ms.locfileid: "31812525"
---
# <a name="getting-started-with-ef-core-on-net-framework-with-a-new-database"></a><span data-ttu-id="82aa4-102">Начало работы с EF Core в .NET Framework с новой базой данных</span><span class="sxs-lookup"><span data-stu-id="82aa4-102">Getting started with EF Core on .NET Framework with a New Database</span></span>

<span data-ttu-id="82aa4-103">В этом пошаговом руководстве вы создадите консольное приложение, которое осуществляет базовые операции доступа к базе данных Microsoft SQL Server с помощью Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="82aa4-103">In this walkthrough, you will build a console application that performs basic data access against a Microsoft SQL Server database using Entity Framework.</span></span> <span data-ttu-id="82aa4-104">Вы примените процесс миграций, чтобы создать базу данных из модели.</span><span class="sxs-lookup"><span data-stu-id="82aa4-104">You will use migrations to create the database from your model.</span></span>

> [!TIP]  
> <span data-ttu-id="82aa4-105">Для этой статьи вы можете скачать [пример](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/FullNet/ConsoleApp.NewDb) из репозитория GitHub.</span><span class="sxs-lookup"><span data-stu-id="82aa4-105">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/FullNet/ConsoleApp.NewDb) on GitHub.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="82aa4-106">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="82aa4-106">Prerequisites</span></span>

<span data-ttu-id="82aa4-107">Для прохождения этого пошагового руководства нужны следующие элементы:</span><span class="sxs-lookup"><span data-stu-id="82aa4-107">The following prerequisites are needed to complete this walkthrough:</span></span>

* [<span data-ttu-id="82aa4-108">Visual Studio 2017</span><span class="sxs-lookup"><span data-stu-id="82aa4-108">Visual Studio 2017</span></span>](https://www.visualstudio.com/downloads/)

* <span data-ttu-id="82aa4-109">[последняя версия диспетчера пакетов NuGet](https://dist.nuget.org/index.html);</span><span class="sxs-lookup"><span data-stu-id="82aa4-109">[Latest version of NuGet Package Manager](https://dist.nuget.org/index.html)</span></span>

* <span data-ttu-id="82aa4-110">[последняя версия Windows PowerShell](https://docs.microsoft.com/powershell/scripting/setup/installing-windows-powershell);</span><span class="sxs-lookup"><span data-stu-id="82aa4-110">[Latest version of Windows PowerShell](https://docs.microsoft.com/powershell/scripting/setup/installing-windows-powershell)</span></span>

## <a name="create-a-new-project"></a><span data-ttu-id="82aa4-111">Создание нового проекта</span><span class="sxs-lookup"><span data-stu-id="82aa4-111">Create a new project</span></span>

* <span data-ttu-id="82aa4-112">Открытие Visual Studio</span><span class="sxs-lookup"><span data-stu-id="82aa4-112">Open Visual Studio</span></span>

* <span data-ttu-id="82aa4-113">Последовательно выберите "Файл" > "Создать" > "Проект".</span><span class="sxs-lookup"><span data-stu-id="82aa4-113">File > New > Project...</span></span>

* <span data-ttu-id="82aa4-114">В меню слева выберите элементы "Шаблоны" > "Visual C#" > "Классический рабочий стол Windows".</span><span class="sxs-lookup"><span data-stu-id="82aa4-114">From the left menu select Templates > Visual C# > Windows Classic Desktop</span></span>

* <span data-ttu-id="82aa4-115">Выберите шаблон проекта **Консольное приложение (.NET Framework)**.</span><span class="sxs-lookup"><span data-stu-id="82aa4-115">Select the **Console App (.NET Framework)** project template</span></span>

* <span data-ttu-id="82aa4-116">Проверьте, что настроено нацеливание на **.NET Framework 4.5.1** или более поздней версии.</span><span class="sxs-lookup"><span data-stu-id="82aa4-116">Ensure you are targeting **.NET Framework 4.5.1** or later</span></span>

* <span data-ttu-id="82aa4-117">Присвойте проекту имя и щелкните **ОК**.</span><span class="sxs-lookup"><span data-stu-id="82aa4-117">Give the project a name and click **OK**</span></span>

## <a name="install-entity-framework"></a><span data-ttu-id="82aa4-118">Установка Entity Framework</span><span class="sxs-lookup"><span data-stu-id="82aa4-118">Install Entity Framework</span></span>

<span data-ttu-id="82aa4-119">Чтобы использовать EF Core, установите пакеты для поставщиков базы данных, с которыми вы будете работать.</span><span class="sxs-lookup"><span data-stu-id="82aa4-119">To use EF Core, install the package for the database provider(s) you want to target.</span></span> <span data-ttu-id="82aa4-120">В этом пошаговом руководстве используется SQL Server.</span><span class="sxs-lookup"><span data-stu-id="82aa4-120">This walkthrough uses SQL Server.</span></span> <span data-ttu-id="82aa4-121">Список доступных поставщиков вы найдете в разделе [Database Providers](../../providers/index.md) (Поставщики базы данных).</span><span class="sxs-lookup"><span data-stu-id="82aa4-121">For a list of available providers see [Database Providers](../../providers/index.md).</span></span>

* <span data-ttu-id="82aa4-122">Последовательно выберите пункты "Средства" -> "Диспетчер пакетов NuGet" -> "Консоль диспетчера пакетов".</span><span class="sxs-lookup"><span data-stu-id="82aa4-122">Tools > NuGet Package Manager > Package Manager Console</span></span>

* <span data-ttu-id="82aa4-123">Запуск `Install-Package Microsoft.EntityFrameworkCore.SqlServer`</span><span class="sxs-lookup"><span data-stu-id="82aa4-123">Run `Install-Package Microsoft.EntityFrameworkCore.SqlServer`</span></span>

<span data-ttu-id="82aa4-124">Далее в этом пошаговом руководстве мы будем использовать некоторые средства платформы Entity Framework для обслуживания базы данных.</span><span class="sxs-lookup"><span data-stu-id="82aa4-124">Later in this walkthrough we will also be using some Entity Framework Tools to maintain the database.</span></span> <span data-ttu-id="82aa4-125">Поэтому мы установим пакет средств:</span><span class="sxs-lookup"><span data-stu-id="82aa4-125">So we will install the tools package as well.</span></span>

* <span data-ttu-id="82aa4-126">Запуск `Install-Package Microsoft.EntityFrameworkCore.Tools`</span><span class="sxs-lookup"><span data-stu-id="82aa4-126">Run `Install-Package Microsoft.EntityFrameworkCore.Tools`</span></span>

## <a name="create-your-model"></a><span data-ttu-id="82aa4-127">Создание модели</span><span class="sxs-lookup"><span data-stu-id="82aa4-127">Create your model</span></span>

<span data-ttu-id="82aa4-128">Теперь нам нужно определить контекст и классы сущности, которые входят в модель:</span><span class="sxs-lookup"><span data-stu-id="82aa4-128">Now it's time to define a context and entity classes that make up your model.</span></span>

* <span data-ttu-id="82aa4-129">Выберите элементы "Проект" > "Добавить класс".</span><span class="sxs-lookup"><span data-stu-id="82aa4-129">Project > Add Class...</span></span>

* <span data-ttu-id="82aa4-130">Введите имя класса *Model.cs* и щелкните **ОК**.</span><span class="sxs-lookup"><span data-stu-id="82aa4-130">Enter *Model.cs* as the name and click **OK**</span></span>

* <span data-ttu-id="82aa4-131">Замените все содержимое этого файла следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="82aa4-131">Replace the contents of the file with the following code</span></span>

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
> <span data-ttu-id="82aa4-132">В реальном приложении каждый класс помещается в отдельный файл, а строка подключения — в файл `App.Config`, откуда она считывается с помощью `ConfigurationManager`.</span><span class="sxs-lookup"><span data-stu-id="82aa4-132">In a real application you would put each class in a separate file and put the connection string in the `App.Config` file and read it out using `ConfigurationManager`.</span></span> <span data-ttu-id="82aa4-133">В этом руководстве мы для простоты заносим все данные в один файл кода.</span><span class="sxs-lookup"><span data-stu-id="82aa4-133">For the sake of simplicity, we are putting everything in a single code file for this tutorial.</span></span>

## <a name="create-your-database"></a><span data-ttu-id="82aa4-134">Создание базы данных</span><span class="sxs-lookup"><span data-stu-id="82aa4-134">Create your database</span></span>

<span data-ttu-id="82aa4-135">Итак, модель будет готова, и вы можете создать из нее базу данных с помощью миграций:</span><span class="sxs-lookup"><span data-stu-id="82aa4-135">Now that you have a model, you can use migrations to create a database for you.</span></span>

* <span data-ttu-id="82aa4-136">Последовательно выберите пункты "Средства" -> "Диспетчер пакетов NuGet" -> "Консоль диспетчера пакетов".</span><span class="sxs-lookup"><span data-stu-id="82aa4-136">Tools –> NuGet Package Manager –> Package Manager Console</span></span>

* <span data-ttu-id="82aa4-137">Запустите `Add-Migration MyFirstMigration`, чтобы сформировать шаблон миграции для создания начального набора таблиц для модели.</span><span class="sxs-lookup"><span data-stu-id="82aa4-137">Run `Add-Migration MyFirstMigration` to scaffold a migration to create the initial set of tables for your model.</span></span>

* <span data-ttu-id="82aa4-138">Запустите `Update-Database`, чтобы применить созданные миграции к базе данных.</span><span class="sxs-lookup"><span data-stu-id="82aa4-138">Run `Update-Database` to apply the new migration to the database.</span></span> <span data-ttu-id="82aa4-139">Поскольку база данных еще не существует, она будет автоматически создана до применения миграции.</span><span class="sxs-lookup"><span data-stu-id="82aa4-139">Because your database doesn't exist yet, it will be created for you before the migration is applied.</span></span>

> [!TIP]  
> <span data-ttu-id="82aa4-140">Если позднее вы внесете изменения в эту модель, с помощью команды `Add-Migration` вы сможете сформировать новую миграцию, которая вносит необходимые изменения в схему базы данных.</span><span class="sxs-lookup"><span data-stu-id="82aa4-140">If you make future changes to your model, you can use the `Add-Migration` command to scaffold a new migration to make the corresponding schema changes to the database.</span></span> <span data-ttu-id="82aa4-141">Проверьте сформированный код шаблона (и внесите правки, если потребуется), а затем выполните команду `Update-Database`, чтобы применить изменения к базе данных.</span><span class="sxs-lookup"><span data-stu-id="82aa4-141">Once you have checked the scaffolded code (and made any required changes), you can use the `Update-Database` command to apply the changes to the database.</span></span>
>
><span data-ttu-id="82aa4-142">EF использует таблицу `__EFMigrationsHistory` в базе данных, чтобы отслеживать уже примененные к базе данных миграции.</span><span class="sxs-lookup"><span data-stu-id="82aa4-142">EF uses a `__EFMigrationsHistory` table in the database to keep track of which migrations have already been applied to the database.</span></span>

## <a name="use-your-model"></a><span data-ttu-id="82aa4-143">Использование модели</span><span class="sxs-lookup"><span data-stu-id="82aa4-143">Use your model</span></span>

<span data-ttu-id="82aa4-144">Теперь вы можете использовать созданную модель для доступа к данным.</span><span class="sxs-lookup"><span data-stu-id="82aa4-144">You can now use your model to perform data access.</span></span>

* <span data-ttu-id="82aa4-145">Откройте файл *Program.cs*.</span><span class="sxs-lookup"><span data-stu-id="82aa4-145">Open *Program.cs*</span></span>

* <span data-ttu-id="82aa4-146">Замените все содержимое этого файла следующим кодом:</span><span class="sxs-lookup"><span data-stu-id="82aa4-146">Replace the contents of the file with the following code</span></span>

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

* <span data-ttu-id="82aa4-147">Выберите "Отладка" -> "Запустить без отладки".</span><span class="sxs-lookup"><span data-stu-id="82aa4-147">Debug > Start Without Debugging</span></span>

<span data-ttu-id="82aa4-148">Вы увидите, как в базе данных создается один блог, а затем в консоль выводятся сведения обо всех блогах.</span><span class="sxs-lookup"><span data-stu-id="82aa4-148">You will see that one blog is saved to the database and then the details of all blogs are printed to the console.</span></span>

![изображение](_static/output-new-db.png)
