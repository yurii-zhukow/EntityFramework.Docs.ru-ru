---
title: Перенос из EF6 в EF Core — перенос модели на основе кода — EF
description: Конкретные сведения о переносе приложения модели на основе кода Entity Framework 6 в Entity Framework Core
author: rowanmiller
ms.date: 10/27/2016
uid: efcore-and-ef6/porting/port-code
ms.openlocfilehash: a5bbdc2ee95ea6bea96e24bee4588b524e0ffc58
ms.sourcegitcommit: abda0872f86eefeca191a9a11bfca976bc14468b
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/14/2020
ms.locfileid: "90073582"
---
# <a name="porting-an-ef6-code-based-model-to-ef-core"></a><span data-ttu-id="46dbf-103">Перенос модели на основе кода EF6 в EF Core</span><span class="sxs-lookup"><span data-stu-id="46dbf-103">Porting an EF6 Code-Based Model to EF Core</span></span>

<span data-ttu-id="46dbf-104">Если вы прочитали все предостережения и готовы к переносу, приведенные ниже рекомендации помогут вам начать.</span><span class="sxs-lookup"><span data-stu-id="46dbf-104">If you've read all the caveats and you are ready to port, then here are some guidelines to help you get started.</span></span>

## <a name="install-ef-core-nuget-packages"></a><span data-ttu-id="46dbf-105">Установка пакетов NuGet EF Core</span><span class="sxs-lookup"><span data-stu-id="46dbf-105">Install EF Core NuGet packages</span></span>

<span data-ttu-id="46dbf-106">Чтобы использовать EF Core, установите пакет NuGet для нужного поставщика базы данных.</span><span class="sxs-lookup"><span data-stu-id="46dbf-106">To use EF Core, you install the NuGet package for the database provider you want to use.</span></span> <span data-ttu-id="46dbf-107">Например, при ориентации на SQL Server нужно установить `Microsoft.EntityFrameworkCore.SqlServer`.</span><span class="sxs-lookup"><span data-stu-id="46dbf-107">For example, when targeting SQL Server, you would install `Microsoft.EntityFrameworkCore.SqlServer`.</span></span> <span data-ttu-id="46dbf-108">Дополнительные сведения см. в разделе [Поставщики баз данных](xref:core/providers/index).</span><span class="sxs-lookup"><span data-stu-id="46dbf-108">See [Database Providers](xref:core/providers/index) for details.</span></span>

<span data-ttu-id="46dbf-109">Если планируется использовать миграции, следует также установить пакет `Microsoft.EntityFrameworkCore.Tools`.</span><span class="sxs-lookup"><span data-stu-id="46dbf-109">If you are planning to use migrations, then you should also install the `Microsoft.EntityFrameworkCore.Tools` package.</span></span>

<span data-ttu-id="46dbf-110">Можно оставить установленный пакет NuGet EF6 (EntityFramework), так как EF Core и EF6 могут использоваться параллельно в одном приложении.</span><span class="sxs-lookup"><span data-stu-id="46dbf-110">It is fine to leave the EF6 NuGet package (EntityFramework) installed, as EF Core and EF6 can be used side-by-side in the same application.</span></span> <span data-ttu-id="46dbf-111">Однако если вы не планируете использовать EF6 в каких-либо областях приложения, то удаление пакета поможет вывести ошибки компиляции для фрагментов кода, требующих внимания.</span><span class="sxs-lookup"><span data-stu-id="46dbf-111">However, if you aren't intending to use EF6 in any areas of your application, then uninstalling the package will help give compile errors on pieces of code that need attention.</span></span>

## <a name="swap-namespaces"></a><span data-ttu-id="46dbf-112">Переключение пространств имен</span><span class="sxs-lookup"><span data-stu-id="46dbf-112">Swap namespaces</span></span>

<span data-ttu-id="46dbf-113">Большинство API, используемых в EF6, находятся в пространстве имен `System.Data.Entity` (и связанных подпространствах имен).</span><span class="sxs-lookup"><span data-stu-id="46dbf-113">Most APIs that you use in EF6 are in the `System.Data.Entity` namespace (and related sub-namespaces).</span></span> <span data-ttu-id="46dbf-114">Первое изменение кода заключается в переключении на пространство имен `Microsoft.EntityFrameworkCore`.</span><span class="sxs-lookup"><span data-stu-id="46dbf-114">The first code change is to swap to the `Microsoft.EntityFrameworkCore` namespace.</span></span> <span data-ttu-id="46dbf-115">В общем случае вам следует начать с производного файла кода контекста, а затем действовать из него, устраняя ошибки компиляции по мере их возникновения.</span><span class="sxs-lookup"><span data-stu-id="46dbf-115">You would typically start with your derived context code file and then work out from there, addressing compilation errors as they occur.</span></span>

## <a name="context-configuration-connection-etc"></a><span data-ttu-id="46dbf-116">Конфигурация контекста (подключение и т. п.)</span><span class="sxs-lookup"><span data-stu-id="46dbf-116">Context configuration (connection etc.)</span></span>

<span data-ttu-id="46dbf-117">Как описано в процедуре [проверки работы EF Core для приложения](xref:efcore-and-ef6/porting/index), EF Core использует меньше логики при обнаружении базы данных для подключения.</span><span class="sxs-lookup"><span data-stu-id="46dbf-117">As described in [Ensure EF Core Will Work for Your Application](xref:efcore-and-ef6/porting/index), EF Core has less magic around detecting the database to connect to.</span></span> <span data-ttu-id="46dbf-118">Вам потребуется переопределить метод `OnConfiguring` в производном контексте и использовать API для конкретного поставщика базы данных для настройки подключения к базе данных.</span><span class="sxs-lookup"><span data-stu-id="46dbf-118">You will need to override the `OnConfiguring` method on your derived context, and use the database provider specific API to setup the connection to the database.</span></span>

<span data-ttu-id="46dbf-119">Большинство приложений EF6 хранят строку подключения в файле `App/Web.config` приложения.</span><span class="sxs-lookup"><span data-stu-id="46dbf-119">Most EF6 applications store the connection string in the applications `App/Web.config` file.</span></span> <span data-ttu-id="46dbf-120">В EF Core для считывания этой строки подключения используется API `ConfigurationManager`.</span><span class="sxs-lookup"><span data-stu-id="46dbf-120">In EF Core, you read this connection string using the `ConfigurationManager` API.</span></span> <span data-ttu-id="46dbf-121">Чтобы использовать этот API, может потребоваться добавить ссылку на сборку платформы `System.Configuration`.</span><span class="sxs-lookup"><span data-stu-id="46dbf-121">You may need to add a reference to the `System.Configuration` framework assembly to be able to use this API.</span></span>

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

## <a name="update-your-code"></a><span data-ttu-id="46dbf-122">Обновление кода</span><span class="sxs-lookup"><span data-stu-id="46dbf-122">Update your code</span></span>

<span data-ttu-id="46dbf-123">На этом этапе осуществляется устранение ошибок компиляции и проверка кода, чтобы узнать, затронут ли вас изменения поведения.</span><span class="sxs-lookup"><span data-stu-id="46dbf-123">At this point, it's a matter of addressing compilation errors and reviewing code to see if the behavior changes will impact you.</span></span>

## <a name="existing-migrations"></a><span data-ttu-id="46dbf-124">Существующие миграции</span><span class="sxs-lookup"><span data-stu-id="46dbf-124">Existing migrations</span></span>

<span data-ttu-id="46dbf-125">Фактически не существует практичного способа перенести имеющиеся миграции EF6 в EF Core.</span><span class="sxs-lookup"><span data-stu-id="46dbf-125">There isn't really a feasible way to port existing EF6 migrations to EF Core.</span></span>

<span data-ttu-id="46dbf-126">По возможности лучше всего предположить, что все предыдущие миграции из EF6 были применены к базе данных, а затем начать миграцию схемы с этой точки с помощью EF Core.</span><span class="sxs-lookup"><span data-stu-id="46dbf-126">If possible, it is best to assume that all previous migrations from EF6 have been applied to the database and then start migrating the schema from that point using EF Core.</span></span> <span data-ttu-id="46dbf-127">Для этого используйте команду `Add-Migration`, чтобы добавить миграцию после переноса модели в EF Core.</span><span class="sxs-lookup"><span data-stu-id="46dbf-127">To do this, you would use the `Add-Migration` command to add a migration once the model is ported to EF Core.</span></span> <span data-ttu-id="46dbf-128">Затем следует удалить весь код из методов `Up` и `Down` шаблонной миграции.</span><span class="sxs-lookup"><span data-stu-id="46dbf-128">You would then remove all code from the `Up` and `Down` methods of the scaffolded migration.</span></span> <span data-ttu-id="46dbf-129">Последующие миграции будут сравниваться с моделью после создания шаблона первоначальной миграции.</span><span class="sxs-lookup"><span data-stu-id="46dbf-129">Subsequent migrations will compare to the model when that initial migration was scaffolded.</span></span>

## <a name="test-the-port"></a><span data-ttu-id="46dbf-130">Тестирование переноса</span><span class="sxs-lookup"><span data-stu-id="46dbf-130">Test the port</span></span>

<span data-ttu-id="46dbf-131">Даже если приложение компилируется, это не означает, что оно успешно перенесено в EF Core.</span><span class="sxs-lookup"><span data-stu-id="46dbf-131">Just because your application compiles, does not mean it is successfully ported to EF Core.</span></span> <span data-ttu-id="46dbf-132">Необходимо протестировать все области приложения, чтобы предотвратить отсутствие неблагоприятного воздействия каких-либо изменений поведения.</span><span class="sxs-lookup"><span data-stu-id="46dbf-132">You will need to test all areas of your application to ensure that none of the behavior changes have adversely impacted your application.</span></span>
