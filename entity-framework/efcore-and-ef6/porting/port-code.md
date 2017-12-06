---
title: "Перенос приложений из EF6 в основные EF – перенос модель на основе кода"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 2dce1a50-7d84-4856-abf6-2763dd9be99d
uid: efcore-and-ef6/porting/port-code
ms.openlocfilehash: a0fa4f9a7028f56666fb993185cb03eddb9a2cd1
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/27/2017
---
# <a name="porting-an-ef6-code-based-model-to-ef-core"></a><span data-ttu-id="c7139-102">Перенос EF6 модели на основе кода EF ядра</span><span class="sxs-lookup"><span data-stu-id="c7139-102">Porting an EF6 Code-Based Model to EF Core</span></span>

<span data-ttu-id="c7139-103">Если вы прочитали все предупреждения, и вы готовы к порту, Вот некоторые рекомендации, которые помогут приступить к работе.</span><span class="sxs-lookup"><span data-stu-id="c7139-103">If you've read all the caveats and you are ready to port, then here are some guidelines to help you get started.</span></span>

## <a name="install-ef-core-nuget-packages"></a><span data-ttu-id="c7139-104">Установить пакеты EF Core NuGet</span><span class="sxs-lookup"><span data-stu-id="c7139-104">Install EF Core NuGet packages</span></span>

<span data-ttu-id="c7139-105">Для использования основных EF, установите пакет NuGet для поставщика базы данных, которые вы хотите использовать.</span><span class="sxs-lookup"><span data-stu-id="c7139-105">To use EF Core, you install the NuGet package for the database provider you want to use.</span></span> <span data-ttu-id="c7139-106">Например, если для различных версий SQL Server, необходимо установить `Microsoft.EntityFrameworkCore.SqlServer`.</span><span class="sxs-lookup"><span data-stu-id="c7139-106">For example, when targeting SQL Server, you would install `Microsoft.EntityFrameworkCore.SqlServer`.</span></span> <span data-ttu-id="c7139-107">В разделе [поставщиков базы данных](../../core/providers/index.md) подробные сведения.</span><span class="sxs-lookup"><span data-stu-id="c7139-107">See [Database Providers](../../core/providers/index.md) for details.</span></span>

<span data-ttu-id="c7139-108">Если вы планируете использовать миграции, то необходимо также установить `Microsoft.EntityFrameworkCore.Tools` пакет.</span><span class="sxs-lookup"><span data-stu-id="c7139-108">If you are planning to use migrations, then you should also install the `Microsoft.EntityFrameworkCore.Tools` package.</span></span>

<span data-ttu-id="c7139-109">Это нормально оставить пакет EF6 NuGet (EntityFramework) установлен, как ядро EF и EF6 может быть используется side-by-side в одном приложении.</span><span class="sxs-lookup"><span data-stu-id="c7139-109">It is fine to leave the EF6 NuGet package (EntityFramework) installed, as EF Core and EF6 can be used side-by-side in the same application.</span></span> <span data-ttu-id="c7139-110">Тем не менее если вы не собираетесь использовать EF6 в любой области приложения, затем при удалении пакета поможет выдают ошибки компиляции на элементы кода, требующие внимания.</span><span class="sxs-lookup"><span data-stu-id="c7139-110">However, if you aren't intending to use EF6 in any areas of your application, then uninstalling the package will help give compile errors on pieces of code that need attention.</span></span>

## <a name="swap-namespaces"></a><span data-ttu-id="c7139-111">Поменять местами пространства имен</span><span class="sxs-lookup"><span data-stu-id="c7139-111">Swap namespaces</span></span>

<span data-ttu-id="c7139-112">Большинство интерфейсов API, которые используются в EF6 находятся в `System.Data.Entity` пространства имен (и связанные вложенные пространства имен).</span><span class="sxs-lookup"><span data-stu-id="c7139-112">Most APIs that you use in EF6 are in the `System.Data.Entity` namespace (and related sub-namespaces).</span></span> <span data-ttu-id="c7139-113">Первое изменение кода — для замены для `Microsoft.EntityFrameworkCore` пространства имен.</span><span class="sxs-lookup"><span data-stu-id="c7139-113">The first code change is to swap to the `Microsoft.EntityFrameworkCore` namespace.</span></span> <span data-ttu-id="c7139-114">Следует начинать с помощью файла кода в контексте производной и затем составить оттуда адресации ошибки компиляции, как только они происходят.</span><span class="sxs-lookup"><span data-stu-id="c7139-114">You would typically start with your derived context code file and then work out from there, addressing compilation errors as they occur.</span></span>

## <a name="context-configuration-connection-etc"></a><span data-ttu-id="c7139-115">Контекст конфигурации (подключение и т. д.)</span><span class="sxs-lookup"><span data-stu-id="c7139-115">Context configuration (connection etc.)</span></span>

<span data-ttu-id="c7139-116">Как описано в [обеспечить EF основная будет работа для вашего приложения](ensure-requirements.md), ядро EF имеет меньше magic вокруг обнаружение для подключения к базе данных.</span><span class="sxs-lookup"><span data-stu-id="c7139-116">As described in [Ensure EF Core Will Work for Your Application](ensure-requirements.md), EF Core has less magic around detecting the database to connect to.</span></span> <span data-ttu-id="c7139-117">Необходимо переопределить `OnConfiguring` метод в производном контекста и установить подключение к базе данных с помощью конкретного API поставщика базы данных.</span><span class="sxs-lookup"><span data-stu-id="c7139-117">You will need to override the `OnConfiguring` method on your derived context, and use the database provider specific API to setup the connection to the database.</span></span>

<span data-ttu-id="c7139-118">Большинство приложений EF6 Сохранение строки подключения в приложениях `App/Web.config` файла.</span><span class="sxs-lookup"><span data-stu-id="c7139-118">Most EF6 applications store the connection string in the applications `App/Web.config` file.</span></span> <span data-ttu-id="c7139-119">В ядре EF, чтение этой строки подключения с помощью `ConfigurationManager` API.</span><span class="sxs-lookup"><span data-stu-id="c7139-119">In EF Core, you read this connection string using the `ConfigurationManager` API.</span></span> <span data-ttu-id="c7139-120">Может потребоваться добавить ссылку на `System.Configuration` framework сборки, чтобы иметь возможность использовать этот API.</span><span class="sxs-lookup"><span data-stu-id="c7139-120">You may need to add a reference to the `System.Configuration` framework assembly to be able to use this API.</span></span>

``` csharp
public class BloggingContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }
    public DbSet<Post> Posts { get; set; }

    protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    {
      optionsBuilder.UseSqlServer(ConfigurationManager.ConnectionStrings["BloggingDatabase"].ConnectionString);
    }
}
```

## <a name="update-your-code"></a><span data-ttu-id="c7139-121">Обновите ваш код</span><span class="sxs-lookup"><span data-stu-id="c7139-121">Update your code</span></span>

<span data-ttu-id="c7139-122">На этом этапе это вопрос устранения ошибок компиляции и проверки кода, чтобы просмотреть, если вы влияют изменения в работе.</span><span class="sxs-lookup"><span data-stu-id="c7139-122">At this point, it's a matter of addressing compilation errors and reviewing code to see if the behavior changes will impact you.</span></span>

## <a name="existing-migrations"></a><span data-ttu-id="c7139-123">Существующие миграции</span><span class="sxs-lookup"><span data-stu-id="c7139-123">Existing migrations</span></span>

<span data-ttu-id="c7139-124">Действительно не подходящий способ переноса существующих EF6 миграций EF ядра.</span><span class="sxs-lookup"><span data-stu-id="c7139-124">There isn't really a feasible way to port existing EF6 migrations to EF Core.</span></span>

<span data-ttu-id="c7139-125">Если это возможно лучше предположить, что все предыдущие миграцию с EF6 были применены к базе данных, а затем с помощью EF Core миграции схемы, из начала.</span><span class="sxs-lookup"><span data-stu-id="c7139-125">If possible, it is best to assume that all previous migrations from EF6 have been applied to the database and then start migrating the schema from that point using EF Core.</span></span> <span data-ttu-id="c7139-126">Чтобы сделать это, используйте `Add-Migration` команду, чтобы добавить к миграции после модели, перенесена в EF Core.</span><span class="sxs-lookup"><span data-stu-id="c7139-126">To do this, you would use the `Add-Migration` command to add a migration once the model is ported to EF Core.</span></span> <span data-ttu-id="c7139-127">Затем нужно удалить весь код из `Up` и `Down` методы переноса формирования шаблонов.</span><span class="sxs-lookup"><span data-stu-id="c7139-127">You would then remove all code from the `Up` and `Down` methods of the scaffolded migration.</span></span> <span data-ttu-id="c7139-128">Последующей миграции будет сравнить модели при этой первоначальной миграции был формирования шаблонов.</span><span class="sxs-lookup"><span data-stu-id="c7139-128">Subsequent migrations will compare to the model when that initial migration was scaffolded.</span></span>

## <a name="test-the-port"></a><span data-ttu-id="c7139-129">Проверить порт</span><span class="sxs-lookup"><span data-stu-id="c7139-129">Test the port</span></span>

<span data-ttu-id="c7139-130">Только потому, что компилирует приложения, не означает, что он успешно перенесена на EF Core.</span><span class="sxs-lookup"><span data-stu-id="c7139-130">Just because your application compiles, does not mean it is successfully ported to EF Core.</span></span> <span data-ttu-id="c7139-131">Необходимо для проверки всех областях приложения, чтобы убедиться, что никакие изменения в поведении отрицательно повлиять приложения.</span><span class="sxs-lookup"><span data-stu-id="c7139-131">You will need to test all areas of your application to ensure that none of the behavior changes have adversely impacted your application.</span></span>
