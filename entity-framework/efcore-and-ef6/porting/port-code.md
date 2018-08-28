---
title: Перенос приложений из EF6 в EF Core — перенос модели на основе кода
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 2dce1a50-7d84-4856-abf6-2763dd9be99d
uid: efcore-and-ef6/porting/port-code
ms.openlocfilehash: 2484b681d71ae8711b1b3a59bc274a0b2e403294
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/27/2018
ms.locfileid: "42997053"
---
# <a name="porting-an-ef6-code-based-model-to-ef-core"></a><span data-ttu-id="5a05a-102">Перенос модель на основе кода EF6 в EF Core</span><span class="sxs-lookup"><span data-stu-id="5a05a-102">Porting an EF6 Code-Based Model to EF Core</span></span>

<span data-ttu-id="5a05a-103">Если вы прочитали все предупреждения, и вы готовы к порту, Вот несколько рекомендаций, которые помогут вам приступить к работе.</span><span class="sxs-lookup"><span data-stu-id="5a05a-103">If you've read all the caveats and you are ready to port, then here are some guidelines to help you get started.</span></span>

## <a name="install-ef-core-nuget-packages"></a><span data-ttu-id="5a05a-104">Установить пакеты EF Core NuGet</span><span class="sxs-lookup"><span data-stu-id="5a05a-104">Install EF Core NuGet packages</span></span>

<span data-ttu-id="5a05a-105">Чтобы использовать EF Core, установите пакет NuGet для поставщика базы данных, которую вы хотите использовать.</span><span class="sxs-lookup"><span data-stu-id="5a05a-105">To use EF Core, you install the NuGet package for the database provider you want to use.</span></span> <span data-ttu-id="5a05a-106">Например, при разработке для SQL Server, требуется установить `Microsoft.EntityFrameworkCore.SqlServer`.</span><span class="sxs-lookup"><span data-stu-id="5a05a-106">For example, when targeting SQL Server, you would install `Microsoft.EntityFrameworkCore.SqlServer`.</span></span> <span data-ttu-id="5a05a-107">См. в разделе [поставщики баз данных](../../core/providers/index.md) подробные сведения.</span><span class="sxs-lookup"><span data-stu-id="5a05a-107">See [Database Providers](../../core/providers/index.md) for details.</span></span>

<span data-ttu-id="5a05a-108">Если вы планируете использовать migrations, то также следует установить `Microsoft.EntityFrameworkCore.Tools` пакета.</span><span class="sxs-lookup"><span data-stu-id="5a05a-108">If you are planning to use migrations, then you should also install the `Microsoft.EntityFrameworkCore.Tools` package.</span></span>

<span data-ttu-id="5a05a-109">Это можно оставить EF6 установленный пакет NuGet (EntityFramework), как EF Core и EF6 можно использовать side-by-side в одном приложении.</span><span class="sxs-lookup"><span data-stu-id="5a05a-109">It is fine to leave the EF6 NuGet package (EntityFramework) installed, as EF Core and EF6 can be used side-by-side in the same application.</span></span> <span data-ttu-id="5a05a-110">Тем не менее если вы не собираетесь использовать EF6 в любой области приложения, то при удалении пакета поможет выдают ошибки компиляции на фрагменты кода, требуют внимания.</span><span class="sxs-lookup"><span data-stu-id="5a05a-110">However, if you aren't intending to use EF6 in any areas of your application, then uninstalling the package will help give compile errors on pieces of code that need attention.</span></span>

## <a name="swap-namespaces"></a><span data-ttu-id="5a05a-111">Переключить пространств имен</span><span class="sxs-lookup"><span data-stu-id="5a05a-111">Swap namespaces</span></span>

<span data-ttu-id="5a05a-112">Большинство интерфейсов API, используемых в EF6 находятся в `System.Data.Entity` пространства имен (и связанные с ней подпространства имен).</span><span class="sxs-lookup"><span data-stu-id="5a05a-112">Most APIs that you use in EF6 are in the `System.Data.Entity` namespace (and related sub-namespaces).</span></span> <span data-ttu-id="5a05a-113">Первое изменение кода, поменяйте местами для `Microsoft.EntityFrameworkCore` пространства имен.</span><span class="sxs-lookup"><span data-stu-id="5a05a-113">The first code change is to swap to the `Microsoft.EntityFrameworkCore` namespace.</span></span> <span data-ttu-id="5a05a-114">Вы бы, обычно начинается с файла кода производного контекста и затем разработать оттуда адресации ошибки компиляции по мере их появления.</span><span class="sxs-lookup"><span data-stu-id="5a05a-114">You would typically start with your derived context code file and then work out from there, addressing compilation errors as they occur.</span></span>

## <a name="context-configuration-connection-etc"></a><span data-ttu-id="5a05a-115">Конфигурацию контекста (соединение д.)</span><span class="sxs-lookup"><span data-stu-id="5a05a-115">Context configuration (connection etc.)</span></span>

<span data-ttu-id="5a05a-116">Как описано в разделе [обеспечить EF Core будет действий по обеспечению приложения](ensure-requirements.md), EF Core имеет менее magic вокруг обнаружение для подключения к базе данных.</span><span class="sxs-lookup"><span data-stu-id="5a05a-116">As described in [Ensure EF Core Will Work for Your Application](ensure-requirements.md), EF Core has less magic around detecting the database to connect to.</span></span> <span data-ttu-id="5a05a-117">Необходимо переопределить `OnConfiguring` метод в производном контексте и используйте поставщик конкретный API базы данных для настройки подключения к базе данных.</span><span class="sxs-lookup"><span data-stu-id="5a05a-117">You will need to override the `OnConfiguring` method on your derived context, and use the database provider specific API to setup the connection to the database.</span></span>

<span data-ttu-id="5a05a-118">Большинство приложений EF6 хранить строки подключения в приложениях `App/Web.config` файл.</span><span class="sxs-lookup"><span data-stu-id="5a05a-118">Most EF6 applications store the connection string in the applications `App/Web.config` file.</span></span> <span data-ttu-id="5a05a-119">В EF Core прочтением этой строки подключения с помощью `ConfigurationManager` API.</span><span class="sxs-lookup"><span data-stu-id="5a05a-119">In EF Core, you read this connection string using the `ConfigurationManager` API.</span></span> <span data-ttu-id="5a05a-120">Может потребоваться добавить ссылку на `System.Configuration` framework сборки, чтобы иметь возможность использовать этот API.</span><span class="sxs-lookup"><span data-stu-id="5a05a-120">You may need to add a reference to the `System.Configuration` framework assembly to be able to use this API.</span></span>

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

## <a name="update-your-code"></a><span data-ttu-id="5a05a-121">Обновление кода</span><span class="sxs-lookup"><span data-stu-id="5a05a-121">Update your code</span></span>

<span data-ttu-id="5a05a-122">На этом этапе это вопрос адресации ошибки компиляции и проверки кода, чтобы увидеть, если изменения в работе повлияет на вас.</span><span class="sxs-lookup"><span data-stu-id="5a05a-122">At this point, it's a matter of addressing compilation errors and reviewing code to see if the behavior changes will impact you.</span></span>

## <a name="existing-migrations"></a><span data-ttu-id="5a05a-123">Существующие миграции</span><span class="sxs-lookup"><span data-stu-id="5a05a-123">Existing migrations</span></span>

<span data-ttu-id="5a05a-124">Действительно не представляется возможным способ переноса имеющихся миграций EF6 в EF Core.</span><span class="sxs-lookup"><span data-stu-id="5a05a-124">There isn't really a feasible way to port existing EF6 migrations to EF Core.</span></span>

<span data-ttu-id="5a05a-125">Если это возможно лучше всего предполагают, что всех предыдущих операций миграции из EF6 были применены к базе данных, а затем start, миграция схемы из этой точки, с использованием EF Core.</span><span class="sxs-lookup"><span data-stu-id="5a05a-125">If possible, it is best to assume that all previous migrations from EF6 have been applied to the database and then start migrating the schema from that point using EF Core.</span></span> <span data-ttu-id="5a05a-126">Чтобы сделать это, используйте `Add-Migration` команду, чтобы добавить миграцию, когда модель будет перенесена в EF Core.</span><span class="sxs-lookup"><span data-stu-id="5a05a-126">To do this, you would use the `Add-Migration` command to add a migration once the model is ported to EF Core.</span></span> <span data-ttu-id="5a05a-127">Затем, вам нужно удалить весь код из `Up` и `Down` методы шаблонный миграции.</span><span class="sxs-lookup"><span data-stu-id="5a05a-127">You would then remove all code from the `Up` and `Down` methods of the scaffolded migration.</span></span> <span data-ttu-id="5a05a-128">Последующей миграции сравниваются в модель при был сформированные для первоначальной миграции.</span><span class="sxs-lookup"><span data-stu-id="5a05a-128">Subsequent migrations will compare to the model when that initial migration was scaffolded.</span></span>

## <a name="test-the-port"></a><span data-ttu-id="5a05a-129">Протестировать порт</span><span class="sxs-lookup"><span data-stu-id="5a05a-129">Test the port</span></span>

<span data-ttu-id="5a05a-130">Компилирует приложение, не означает, что он успешно перенесена в EF Core.</span><span class="sxs-lookup"><span data-stu-id="5a05a-130">Just because your application compiles, does not mean it is successfully ported to EF Core.</span></span> <span data-ttu-id="5a05a-131">Необходимо будет проверить все области приложения, чтобы убедиться, что ни одно из изменений поведение негативно повлиять на приложения.</span><span class="sxs-lookup"><span data-stu-id="5a05a-131">You will need to test all areas of your application to ensure that none of the behavior changes have adversely impacted your application.</span></span>
