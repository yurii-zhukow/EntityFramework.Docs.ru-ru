---
title: Перенос из EF6 в EF Core — перенос модели на основе кода — EF
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 2dce1a50-7d84-4856-abf6-2763dd9be99d
uid: efcore-and-ef6/porting/port-code
ms.openlocfilehash: 0a99eac2091c07d8bcf7d4e5e4bdc2afcaeee810
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/07/2020
ms.locfileid: "78413861"
---
# <a name="porting-an-ef6-code-based-model-to-ef-core"></a><span data-ttu-id="bb05e-102">Перенос модели на основе кода EF6 в EF Core</span><span class="sxs-lookup"><span data-stu-id="bb05e-102">Porting an EF6 Code-Based Model to EF Core</span></span>

<span data-ttu-id="bb05e-103">Если вы прочитали все предостережения и готовы к переносу, приведенные ниже рекомендации помогут вам начать.</span><span class="sxs-lookup"><span data-stu-id="bb05e-103">If you've read all the caveats and you are ready to port, then here are some guidelines to help you get started.</span></span>

## <a name="install-ef-core-nuget-packages"></a><span data-ttu-id="bb05e-104">Установка пакетов NuGet EF Core</span><span class="sxs-lookup"><span data-stu-id="bb05e-104">Install EF Core NuGet packages</span></span>

<span data-ttu-id="bb05e-105">Чтобы использовать EF Core, установите пакет NuGet для нужного поставщика базы данных.</span><span class="sxs-lookup"><span data-stu-id="bb05e-105">To use EF Core, you install the NuGet package for the database provider you want to use.</span></span> <span data-ttu-id="bb05e-106">Например, при ориентации на SQL Server нужно установить `Microsoft.EntityFrameworkCore.SqlServer`.</span><span class="sxs-lookup"><span data-stu-id="bb05e-106">For example, when targeting SQL Server, you would install `Microsoft.EntityFrameworkCore.SqlServer`.</span></span> <span data-ttu-id="bb05e-107">Дополнительные сведения см. в разделе [Поставщики баз данных](../../core/providers/index.md).</span><span class="sxs-lookup"><span data-stu-id="bb05e-107">See [Database Providers](../../core/providers/index.md) for details.</span></span>

<span data-ttu-id="bb05e-108">Если планируется использовать миграции, следует также установить пакет `Microsoft.EntityFrameworkCore.Tools`.</span><span class="sxs-lookup"><span data-stu-id="bb05e-108">If you are planning to use migrations, then you should also install the `Microsoft.EntityFrameworkCore.Tools` package.</span></span>

<span data-ttu-id="bb05e-109">Можно оставить установленный пакет NuGet EF6 (EntityFramework), так как EF Core и EF6 могут использоваться параллельно в одном приложении.</span><span class="sxs-lookup"><span data-stu-id="bb05e-109">It is fine to leave the EF6 NuGet package (EntityFramework) installed, as EF Core and EF6 can be used side-by-side in the same application.</span></span> <span data-ttu-id="bb05e-110">Однако если вы не планируете использовать EF6 в каких-либо областях приложения, то удаление пакета поможет вывести ошибки компиляции для фрагментов кода, требующих внимания.</span><span class="sxs-lookup"><span data-stu-id="bb05e-110">However, if you aren't intending to use EF6 in any areas of your application, then uninstalling the package will help give compile errors on pieces of code that need attention.</span></span>

## <a name="swap-namespaces"></a><span data-ttu-id="bb05e-111">Переключение пространств имен</span><span class="sxs-lookup"><span data-stu-id="bb05e-111">Swap namespaces</span></span>

<span data-ttu-id="bb05e-112">Большинство API, используемых в EF6, находятся в пространстве имен `System.Data.Entity` (и связанных подпространствах имен).</span><span class="sxs-lookup"><span data-stu-id="bb05e-112">Most APIs that you use in EF6 are in the `System.Data.Entity` namespace (and related sub-namespaces).</span></span> <span data-ttu-id="bb05e-113">Первое изменение кода заключается в переключении на пространство имен `Microsoft.EntityFrameworkCore`.</span><span class="sxs-lookup"><span data-stu-id="bb05e-113">The first code change is to swap to the `Microsoft.EntityFrameworkCore` namespace.</span></span> <span data-ttu-id="bb05e-114">В общем случае вам следует начать с производного файла кода контекста, а затем действовать из него, устраняя ошибки компиляции по мере их возникновения.</span><span class="sxs-lookup"><span data-stu-id="bb05e-114">You would typically start with your derived context code file and then work out from there, addressing compilation errors as they occur.</span></span>

## <a name="context-configuration-connection-etc"></a><span data-ttu-id="bb05e-115">Конфигурация контекста (подключение и т. п.)</span><span class="sxs-lookup"><span data-stu-id="bb05e-115">Context configuration (connection etc.)</span></span>

<span data-ttu-id="bb05e-116">Как описано в процедуре [проверки работы EF Core для приложения](ensure-requirements.md), EF Core использует меньше логики при обнаружении базы данных для подключения.</span><span class="sxs-lookup"><span data-stu-id="bb05e-116">As described in [Ensure EF Core Will Work for Your Application](ensure-requirements.md), EF Core has less magic around detecting the database to connect to.</span></span> <span data-ttu-id="bb05e-117">Вам потребуется переопределить метод `OnConfiguring` в производном контексте и использовать API для конкретного поставщика базы данных для настройки подключения к базе данных.</span><span class="sxs-lookup"><span data-stu-id="bb05e-117">You will need to override the `OnConfiguring` method on your derived context, and use the database provider specific API to setup the connection to the database.</span></span>

<span data-ttu-id="bb05e-118">Большинство приложений EF6 хранят строку подключения в файле `App/Web.config` приложения.</span><span class="sxs-lookup"><span data-stu-id="bb05e-118">Most EF6 applications store the connection string in the applications `App/Web.config` file.</span></span> <span data-ttu-id="bb05e-119">В EF Core для считывания этой строки подключения используется API `ConfigurationManager`.</span><span class="sxs-lookup"><span data-stu-id="bb05e-119">In EF Core, you read this connection string using the `ConfigurationManager` API.</span></span> <span data-ttu-id="bb05e-120">Чтобы использовать этот API, может потребоваться добавить ссылку на сборку платформы `System.Configuration`.</span><span class="sxs-lookup"><span data-stu-id="bb05e-120">You may need to add a reference to the `System.Configuration` framework assembly to be able to use this API.</span></span>

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

## <a name="update-your-code"></a><span data-ttu-id="bb05e-121">Обновление кода</span><span class="sxs-lookup"><span data-stu-id="bb05e-121">Update your code</span></span>

<span data-ttu-id="bb05e-122">На этом этапе осуществляется устранение ошибок компиляции и проверка кода, чтобы узнать, затронут ли вас изменения поведения.</span><span class="sxs-lookup"><span data-stu-id="bb05e-122">At this point, it's a matter of addressing compilation errors and reviewing code to see if the behavior changes will impact you.</span></span>

## <a name="existing-migrations"></a><span data-ttu-id="bb05e-123">Существующие миграции</span><span class="sxs-lookup"><span data-stu-id="bb05e-123">Existing migrations</span></span>

<span data-ttu-id="bb05e-124">Фактически не существует практичного способа перенести имеющиеся миграции EF6 в EF Core.</span><span class="sxs-lookup"><span data-stu-id="bb05e-124">There isn't really a feasible way to port existing EF6 migrations to EF Core.</span></span>

<span data-ttu-id="bb05e-125">По возможности лучше всего предположить, что все предыдущие миграции из EF6 были применены к базе данных, а затем начать миграцию схемы с этой точки с помощью EF Core.</span><span class="sxs-lookup"><span data-stu-id="bb05e-125">If possible, it is best to assume that all previous migrations from EF6 have been applied to the database and then start migrating the schema from that point using EF Core.</span></span> <span data-ttu-id="bb05e-126">Для этого используйте команду `Add-Migration`, чтобы добавить миграцию после переноса модели в EF Core.</span><span class="sxs-lookup"><span data-stu-id="bb05e-126">To do this, you would use the `Add-Migration` command to add a migration once the model is ported to EF Core.</span></span> <span data-ttu-id="bb05e-127">Затем следует удалить весь код из методов `Up` и `Down` шаблонной миграции.</span><span class="sxs-lookup"><span data-stu-id="bb05e-127">You would then remove all code from the `Up` and `Down` methods of the scaffolded migration.</span></span> <span data-ttu-id="bb05e-128">Последующие миграции будут сравниваться с моделью после создания шаблона первоначальной миграции.</span><span class="sxs-lookup"><span data-stu-id="bb05e-128">Subsequent migrations will compare to the model when that initial migration was scaffolded.</span></span>

## <a name="test-the-port"></a><span data-ttu-id="bb05e-129">Тестирование переноса</span><span class="sxs-lookup"><span data-stu-id="bb05e-129">Test the port</span></span>

<span data-ttu-id="bb05e-130">Даже если приложение компилируется, это не означает, что оно успешно перенесено в EF Core.</span><span class="sxs-lookup"><span data-stu-id="bb05e-130">Just because your application compiles, does not mean it is successfully ported to EF Core.</span></span> <span data-ttu-id="bb05e-131">Необходимо протестировать все области приложения, чтобы предотвратить отсутствие неблагоприятного воздействия каких-либо изменений поведения.</span><span class="sxs-lookup"><span data-stu-id="bb05e-131">You will need to test all areas of your application to ensure that none of the behavior changes have adversely impacted your application.</span></span>
