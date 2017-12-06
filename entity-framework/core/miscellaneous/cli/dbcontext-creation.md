---
title: "Создание DbContext во время разработки - EF Core"
author: bricelam
ms.author: bricelam
ms.date: 10/27/2017
ms.technology: entity-framework-core
ms.openlocfilehash: 5fcd9e362d76127e7acadd9e552ef3ac90967a37
ms.sourcegitcommit: 5e2d97e731f975cf3405ff3deab2a3c75ad1b969
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/15/2017
---
<a name="design-time-dbcontext-creation"></a><span data-ttu-id="2a5c4-102">Создание DbContext во время разработки</span><span class="sxs-lookup"><span data-stu-id="2a5c4-102">Design-time DbContext Creation</span></span>
==============================
<span data-ttu-id="2a5c4-103">Некоторые средства EF команд требуется экземпляр DbContext, созданные на этапе разработки времени (например, при выполнении команды миграции).</span><span class="sxs-lookup"><span data-stu-id="2a5c4-103">Some of the EF Tools commands require a DbContext instance to be created at design time (for example, when running Migrations commands).</span></span> <span data-ttu-id="2a5c4-104">Существует несколько способов, попытайтесь создать ее средств.</span><span class="sxs-lookup"><span data-stu-id="2a5c4-104">There are various ways the tools try to create it.</span></span>

<a name="from-application-services"></a><span data-ttu-id="2a5c4-105">Из служб приложений</span><span class="sxs-lookup"><span data-stu-id="2a5c4-105">From application services</span></span>
-------------------------
<span data-ttu-id="2a5c4-106">Если автоматически запускаемый проект приложения ASP.NET Core, средства попытайтесь получить объект DbContext приложения поставщика услуг.</span><span class="sxs-lookup"><span data-stu-id="2a5c4-106">If your startup project is an ASP.NET Core app, the tools try to obtain the DbContext object from the application's service provider.</span></span> <span data-ttu-id="2a5c4-107">Они получают путем вызова `Program.BuildWebHost()` и доступ к `IWebHost.Services` свойство.</span><span class="sxs-lookup"><span data-stu-id="2a5c4-107">They obtain it by invoking `Program.BuildWebHost()` and accessing the `IWebHost.Services` property.</span></span> <span data-ttu-id="2a5c4-108">Любой DbContext зарегистрировать с помощью `IServiceCollection.AddDbContext<TContext>()` могут быть найдены и созданный таким способом.</span><span class="sxs-lookup"><span data-stu-id="2a5c4-108">Any DbContext registered using `IServiceCollection.AddDbContext<TContext>()` can be found and created this way.</span></span> <span data-ttu-id="2a5c4-109">Этот шаблон был [появилась в ASP.NET Core 2.0][1]</span><span class="sxs-lookup"><span data-stu-id="2a5c4-109">This pattern was [introduced in ASP.NET Core 2.0][1]</span></span>

<a name="using-the-default-constructor"></a><span data-ttu-id="2a5c4-110">С помощью конструктора по умолчанию</span><span class="sxs-lookup"><span data-stu-id="2a5c4-110">Using the default constructor</span></span>
-----------------------------
<span data-ttu-id="2a5c4-111">Если класс DbContext нельзя получить от поставщика службы приложений, средства поиска типа DbContext внутри проекта.</span><span class="sxs-lookup"><span data-stu-id="2a5c4-111">If the DbContext can't be obtained from the application service provider, the tools look for the DbContext type inside the project.</span></span> <span data-ttu-id="2a5c4-112">Они пытаются создать его с помощью его конструктора по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="2a5c4-112">They try to create it using its default constructor.</span></span>

<a name="from-a-design-time-factory"></a><span data-ttu-id="2a5c4-113">Из фабрики во время разработки</span><span class="sxs-lookup"><span data-stu-id="2a5c4-113">From a design-time factory</span></span>
--------------------------
<span data-ttu-id="2a5c4-114">Можно также сказать, средства Создание вашей DbContext путем реализации `IDesignTimeDbContextFactory`.</span><span class="sxs-lookup"><span data-stu-id="2a5c4-114">You can also tell the tools how to create your DbContext by implementing `IDesignTimeDbContextFactory`.</span></span> <span data-ttu-id="2a5c4-115">Если класс, реализующий этот интерфейс располагается в пределах проекта, средства обойти другие способы создания класс DbContext.</span><span class="sxs-lookup"><span data-stu-id="2a5c4-115">If a class implementing this interface is found inside your project, the tools bypass the other ways of creating the DbContext.</span></span>
<span data-ttu-id="2a5c4-116">Фабрика всегда используют во время разработки.</span><span class="sxs-lookup"><span data-stu-id="2a5c4-116">They always use the factory at design time.</span></span> <span data-ttu-id="2a5c4-117">Фабрика особенно полезен, если необходимо настроить класс DbContext по-разному для времени разработки, чем во время выполнения.</span><span class="sxs-lookup"><span data-stu-id="2a5c4-117">A factory is especially useful if you need to configure the DbContext differently for design time than at runtime.</span></span>

``` csharp
using Microsoft.EntityFrameworkCore;
using Microsoft.EntityFrameworkCore.Infrastructure;

namespace MyProject
{
    public class BloggingContextFactory : IDesignTimeDbContextFactory<BloggingContext>
    {
        public BloggingContext CreateDbContext(string[] args)
        {
            var optionsBuilder = new DbContextOptionsBuilder<BloggingContext>();
            optionsBuilder.UseSqlite("Data Source=blog.db");

            return new BloggingContext(optionsBuilder.Options);
        }
    }
}
```

> [!NOTE]
> <span data-ttu-id="2a5c4-118">`args` Параметр не в настоящее время используется.</span><span class="sxs-lookup"><span data-stu-id="2a5c4-118">The `args` parameter is currently unused.</span></span> <span data-ttu-id="2a5c4-119">Отсутствует [проблему] [ 2] возможность указывать аргументы во время разработки с помощью средств отслеживания.</span><span class="sxs-lookup"><span data-stu-id="2a5c4-119">There is [an issue][2] tracking the ability to specify design-time arguments from the tools.</span></span>

  [1]: https://docs.microsoft.com/aspnet/core/migration/1x-to-2x/#update-main-method-in-programcs
  [2]: https://github.com/aspnet/EntityFrameworkCore/issues/8332
