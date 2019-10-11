---
title: Перенос из EF6 в EF Core-перенос модели на основе кода — EF
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 2dce1a50-7d84-4856-abf6-2763dd9be99d
uid: efcore-and-ef6/porting/port-code
ms.openlocfilehash: 0a99eac2091c07d8bcf7d4e5e4bdc2afcaeee810
ms.sourcegitcommit: 708b18520321c587b2046ad2ea9fa7c48aeebfe5
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/09/2019
ms.locfileid: "72181218"
---
# <a name="porting-an-ef6-code-based-model-to-ef-core"></a><span data-ttu-id="7a72a-102">Перенос модели на основе кода EF6 в EF Core</span><span class="sxs-lookup"><span data-stu-id="7a72a-102">Porting an EF6 Code-Based Model to EF Core</span></span>

<span data-ttu-id="7a72a-103">Если вы прочитали все предостережения и готовы к переносу, то ниже приведены некоторые рекомендации, которые помогут приступить к работе.</span><span class="sxs-lookup"><span data-stu-id="7a72a-103">If you've read all the caveats and you are ready to port, then here are some guidelines to help you get started.</span></span>

## <a name="install-ef-core-nuget-packages"></a><span data-ttu-id="7a72a-104">Установка EF Core пакетов NuGet</span><span class="sxs-lookup"><span data-stu-id="7a72a-104">Install EF Core NuGet packages</span></span>

<span data-ttu-id="7a72a-105">Чтобы использовать EF Core, необходимо установить пакет NuGet для поставщика базы данных, который вы хотите использовать.</span><span class="sxs-lookup"><span data-stu-id="7a72a-105">To use EF Core, you install the NuGet package for the database provider you want to use.</span></span> <span data-ttu-id="7a72a-106">Например, при нацеливании на SQL Server можно установить `Microsoft.EntityFrameworkCore.SqlServer`.</span><span class="sxs-lookup"><span data-stu-id="7a72a-106">For example, when targeting SQL Server, you would install `Microsoft.EntityFrameworkCore.SqlServer`.</span></span> <span data-ttu-id="7a72a-107">Дополнительные сведения см. в разделе [поставщики баз данных](../../core/providers/index.md) .</span><span class="sxs-lookup"><span data-stu-id="7a72a-107">See [Database Providers](../../core/providers/index.md) for details.</span></span>

<span data-ttu-id="7a72a-108">Если планируется использовать миграции, следует также установить пакет `Microsoft.EntityFrameworkCore.Tools`.</span><span class="sxs-lookup"><span data-stu-id="7a72a-108">If you are planning to use migrations, then you should also install the `Microsoft.EntityFrameworkCore.Tools` package.</span></span>

<span data-ttu-id="7a72a-109">Можно оставить установленный пакет NuGet EF6 (EntityFramework), так как EF Core и EF6 могут использоваться параллельно в одном приложении.</span><span class="sxs-lookup"><span data-stu-id="7a72a-109">It is fine to leave the EF6 NuGet package (EntityFramework) installed, as EF Core and EF6 can be used side-by-side in the same application.</span></span> <span data-ttu-id="7a72a-110">Однако если вы не планируете использовать EF6 в каких бы то ни было областях приложения, то удаление пакета поможет предоставить ошибки компиляции для фрагментов кода, требующих внимания.</span><span class="sxs-lookup"><span data-stu-id="7a72a-110">However, if you aren't intending to use EF6 in any areas of your application, then uninstalling the package will help give compile errors on pieces of code that need attention.</span></span>

## <a name="swap-namespaces"></a><span data-ttu-id="7a72a-111">Переключить пространства имен</span><span class="sxs-lookup"><span data-stu-id="7a72a-111">Swap namespaces</span></span>

<span data-ttu-id="7a72a-112">Большинство интерфейсов API, используемых в EF6, находятся в пространстве имен `System.Data.Entity` (и связанных подпространствах имен).</span><span class="sxs-lookup"><span data-stu-id="7a72a-112">Most APIs that you use in EF6 are in the `System.Data.Entity` namespace (and related sub-namespaces).</span></span> <span data-ttu-id="7a72a-113">Первое изменение кода заключается в переключении на пространство имен `Microsoft.EntityFrameworkCore`.</span><span class="sxs-lookup"><span data-stu-id="7a72a-113">The first code change is to swap to the `Microsoft.EntityFrameworkCore` namespace.</span></span> <span data-ttu-id="7a72a-114">Как правило, вы начинаете с исходного файла кода контекста, а затем выйдете из него, устраняя ошибки компиляции по мере их возникновения.</span><span class="sxs-lookup"><span data-stu-id="7a72a-114">You would typically start with your derived context code file and then work out from there, addressing compilation errors as they occur.</span></span>

## <a name="context-configuration-connection-etc"></a><span data-ttu-id="7a72a-115">Контекстная конфигурация (подключение и т. д.)</span><span class="sxs-lookup"><span data-stu-id="7a72a-115">Context configuration (connection etc.)</span></span>

<span data-ttu-id="7a72a-116">Как описано в статье [обеспечение работы EF Core для вашего приложения](ensure-requirements.md), EF Core менее волшебны, чем обнаружение базы данных для подключения.</span><span class="sxs-lookup"><span data-stu-id="7a72a-116">As described in [Ensure EF Core Will Work for Your Application](ensure-requirements.md), EF Core has less magic around detecting the database to connect to.</span></span> <span data-ttu-id="7a72a-117">Необходимо переопределить метод `OnConfiguring` в производном контексте и использовать специальный API поставщика базы данных для настройки подключения к базе данных.</span><span class="sxs-lookup"><span data-stu-id="7a72a-117">You will need to override the `OnConfiguring` method on your derived context, and use the database provider specific API to setup the connection to the database.</span></span>

<span data-ttu-id="7a72a-118">Большинство приложений EF6 хранят строку подключения в файле Applications `App/Web.config`.</span><span class="sxs-lookup"><span data-stu-id="7a72a-118">Most EF6 applications store the connection string in the applications `App/Web.config` file.</span></span> <span data-ttu-id="7a72a-119">В EF Core вы читаете эту строку подключения с помощью API `ConfigurationManager`.</span><span class="sxs-lookup"><span data-stu-id="7a72a-119">In EF Core, you read this connection string using the `ConfigurationManager` API.</span></span> <span data-ttu-id="7a72a-120">Может потребоваться добавить ссылку на сборку платформы `System.Configuration`, чтобы иметь возможность использовать этот API.</span><span class="sxs-lookup"><span data-stu-id="7a72a-120">You may need to add a reference to the `System.Configuration` framework assembly to be able to use this API.</span></span>

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

## <a name="update-your-code"></a><span data-ttu-id="7a72a-121">Обновление кода</span><span class="sxs-lookup"><span data-stu-id="7a72a-121">Update your code</span></span>

<span data-ttu-id="7a72a-122">На этом этапе происходит устранение ошибок компиляции и просмотр кода, чтобы узнать, повлияет ли изменение поведения на вас.</span><span class="sxs-lookup"><span data-stu-id="7a72a-122">At this point, it's a matter of addressing compilation errors and reviewing code to see if the behavior changes will impact you.</span></span>

## <a name="existing-migrations"></a><span data-ttu-id="7a72a-123">Существующие миграции</span><span class="sxs-lookup"><span data-stu-id="7a72a-123">Existing migrations</span></span>

<span data-ttu-id="7a72a-124">На самом деле нет разумного способа перенести существующие EF6 миграции в EF Core.</span><span class="sxs-lookup"><span data-stu-id="7a72a-124">There isn't really a feasible way to port existing EF6 migrations to EF Core.</span></span>

<span data-ttu-id="7a72a-125">По возможности рекомендуется предположить, что все предыдущие миграции из EF6 были применены к базе данных, а затем начали перенос схемы с этой точки с помощью EF Core.</span><span class="sxs-lookup"><span data-stu-id="7a72a-125">If possible, it is best to assume that all previous migrations from EF6 have been applied to the database and then start migrating the schema from that point using EF Core.</span></span> <span data-ttu-id="7a72a-126">Для этого используйте команду `Add-Migration`, чтобы добавить миграцию после переноса модели в EF Core.</span><span class="sxs-lookup"><span data-stu-id="7a72a-126">To do this, you would use the `Add-Migration` command to add a migration once the model is ported to EF Core.</span></span> <span data-ttu-id="7a72a-127">Затем вы удалите весь код из методов `Up` и `Down` для формирования с шаблоном миграции.</span><span class="sxs-lookup"><span data-stu-id="7a72a-127">You would then remove all code from the `Up` and `Down` methods of the scaffolded migration.</span></span> <span data-ttu-id="7a72a-128">Последующие миграции будут сравниваться с моделью при создании шаблона первоначальной миграции.</span><span class="sxs-lookup"><span data-stu-id="7a72a-128">Subsequent migrations will compare to the model when that initial migration was scaffolded.</span></span>

## <a name="test-the-port"></a><span data-ttu-id="7a72a-129">Проверка порта</span><span class="sxs-lookup"><span data-stu-id="7a72a-129">Test the port</span></span>

<span data-ttu-id="7a72a-130">Так как приложение компилируется, не означает, что оно успешно перенесено в EF Core.</span><span class="sxs-lookup"><span data-stu-id="7a72a-130">Just because your application compiles, does not mean it is successfully ported to EF Core.</span></span> <span data-ttu-id="7a72a-131">Необходимо протестировать все области приложения, чтобы предотвратить неблагоприятное воздействие каких-либо изменений в поведение приложения.</span><span class="sxs-lookup"><span data-stu-id="7a72a-131">You will need to test all areas of your application to ensure that none of the behavior changes have adversely impacted your application.</span></span>
