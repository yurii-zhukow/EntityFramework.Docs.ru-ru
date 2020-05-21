---
title: Строки подключения — EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: aeb0f5f8-b212-4f89-ae83-c642a5190ba0
uid: core/miscellaneous/connection-strings
ms.openlocfilehash: 062a7f292d16deb3840fd116f270edb11c6e0687
ms.sourcegitcommit: 59e3d5ce7dfb284457cf1c991091683b2d1afe9d
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/20/2020
ms.locfileid: "83672919"
---
# <a name="connection-strings"></a><span data-ttu-id="c240c-102">Строки соединения</span><span class="sxs-lookup"><span data-stu-id="c240c-102">Connection Strings</span></span>

<span data-ttu-id="c240c-103">Большинству поставщиков баз данных для подключения к базе данных требуется определенная форма строки подключения.</span><span class="sxs-lookup"><span data-stu-id="c240c-103">Most database providers require some form of connection string to connect to the database.</span></span> <span data-ttu-id="c240c-104">Иногда эта строка подключения содержит конфиденциальную информацию, которую необходимо защитить.</span><span class="sxs-lookup"><span data-stu-id="c240c-104">Sometimes this connection string contains sensitive information that needs to be protected.</span></span> <span data-ttu-id="c240c-105">Также может потребоваться изменить строку подключения при перемещении приложения между средами, такими как разработка, тестирование и производство.</span><span class="sxs-lookup"><span data-stu-id="c240c-105">You may also need to change the connection string as you move your application between environments, such as development, testing, and production.</span></span>

## <a name="winforms--wpf-applications"></a><span data-ttu-id="c240c-106">Приложения WinForms & WPF</span><span class="sxs-lookup"><span data-stu-id="c240c-106">WinForms & WPF Applications</span></span>

<span data-ttu-id="c240c-107">В приложениях WinForms, WPF и ASP.NET 4 есть предпринятая и проверенная схема строки подключения.</span><span class="sxs-lookup"><span data-stu-id="c240c-107">WinForms, WPF, and ASP.NET 4 applications have a tried and tested connection string pattern.</span></span> <span data-ttu-id="c240c-108">Строка подключения должна быть добавлена в файл App. config приложения (Web. config, если используется ASP.NET).</span><span class="sxs-lookup"><span data-stu-id="c240c-108">The connection string should be added to your application's App.config file (Web.config if you are using ASP.NET).</span></span> <span data-ttu-id="c240c-109">Если строка подключения содержит конфиденциальную информацию, например имя пользователя и пароль, содержимое файла конфигурации можно защитить с помощью [диспетчера секретов](/aspnet/core/security/app-secrets#secret-manager).</span><span class="sxs-lookup"><span data-stu-id="c240c-109">If your connection string contains sensitive information, such as username and password, you can protect the contents of the configuration file using the [Secret Manager tool](/aspnet/core/security/app-secrets#secret-manager).</span></span>

``` xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>

  <connectionStrings>
    <add name="BloggingDatabase"
         connectionString="Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" />
  </connectionStrings>
</configuration>
```

> [!TIP]  
> <span data-ttu-id="c240c-110">`providerName`Параметр не требуется для EF Core строк подключения, хранящихся в файле App. config, так как поставщик базы данных настраивается с помощью кода.</span><span class="sxs-lookup"><span data-stu-id="c240c-110">The `providerName` setting is not required on EF Core connection strings stored in App.config because the database provider is configured via code.</span></span>

<span data-ttu-id="c240c-111">Затем можно прочитать строку подключения с помощью `ConfigurationManager` API в `OnConfiguring` методе контекста.</span><span class="sxs-lookup"><span data-stu-id="c240c-111">You can then read the connection string using the `ConfigurationManager` API in your context's `OnConfiguring` method.</span></span> <span data-ttu-id="c240c-112">Может потребоваться добавить ссылку на `System.Configuration` сборку платформы, чтобы иметь возможность использовать этот API.</span><span class="sxs-lookup"><span data-stu-id="c240c-112">You may need to add a reference to the `System.Configuration` framework assembly to be able to use this API.</span></span>

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

## <a name="universal-windows-platform-uwp"></a><span data-ttu-id="c240c-113">Универсальная платформа Windows (UWP)</span><span class="sxs-lookup"><span data-stu-id="c240c-113">Universal Windows Platform (UWP)</span></span>

<span data-ttu-id="c240c-114">Строки подключения в приложении UWP обычно являются подключениями SQLite, которые просто указывают локальное имя файла.</span><span class="sxs-lookup"><span data-stu-id="c240c-114">Connection strings in a UWP application are typically a SQLite connection that just specifies a local filename.</span></span> <span data-ttu-id="c240c-115">Обычно они не содержат конфиденциальных сведений, и их не нужно изменять при развертывании приложения.</span><span class="sxs-lookup"><span data-stu-id="c240c-115">They typically do not contain sensitive information, and do not need to be changed as an application is deployed.</span></span> <span data-ttu-id="c240c-116">Таким образом, эти строки подключения обычно можно оставить в коде, как показано ниже.</span><span class="sxs-lookup"><span data-stu-id="c240c-116">As such, these connection strings are usually fine to be left in code, as shown below.</span></span> <span data-ttu-id="c240c-117">Если вы хотите переместить их из кода, то UWP поддерживает концепцию параметров. Дополнительные сведения см. в [разделе "Параметры приложения" документации по UWP](/windows/uwp/app-settings/store-and-retrieve-app-data) .</span><span class="sxs-lookup"><span data-stu-id="c240c-117">If you wish to move them out of code then UWP supports the concept of settings, see the [App Settings section of the UWP documentation](/windows/uwp/app-settings/store-and-retrieve-app-data) for details.</span></span>

``` csharp
public class BloggingContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }
    public DbSet<Post> Posts { get; set; }

    protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    {
            optionsBuilder.UseSqlite("Data Source=blogging.db");
    }
}
```

## <a name="aspnet-core"></a><span data-ttu-id="c240c-118">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="c240c-118">ASP.NET Core</span></span>

<span data-ttu-id="c240c-119">В ASP.NET Core система конфигурации очень гибка, и строка подключения может храниться в `appsettings.json` среде, в переменной среды, в хранилище секретов пользователя или в другом источнике конфигурации.</span><span class="sxs-lookup"><span data-stu-id="c240c-119">In ASP.NET Core the configuration system is very flexible, and the connection string could be stored in `appsettings.json`, an environment variable, the user secret store, or another configuration source.</span></span> <span data-ttu-id="c240c-120">Дополнительные сведения см. в [разделе "Конфигурация" документации по ASP.NET Core](/aspnet/core/fundamentals/configuration) .</span><span class="sxs-lookup"><span data-stu-id="c240c-120">See the [Configuration section of the ASP.NET Core documentation](/aspnet/core/fundamentals/configuration) for more details.</span></span>

<span data-ttu-id="c240c-121">Например, можно использовать [Диспетчер секретов](/aspnet/core/security/app-secrets#secret-manager) для хранения пароля базы данных, а затем в формировании шаблонов использовать строку подключения, которая просто состоит из `Name=<database-alias>` .</span><span class="sxs-lookup"><span data-stu-id="c240c-121">For instance, you can use the [Secret Manager tool](/aspnet/core/security/app-secrets#secret-manager) to store your database password and then, in scaffolding, use a connection string that simply consists of `Name=<database-alias>`.</span></span>

```dotnetcli
dotnet user-secrets set ConnectionStrings.YourDatabaseAlias "Data Source=(localdb)\MSSQLLocalDB;Initial Catalog=YourDatabase"
dotnet ef dbcontext scaffold Name=YourDatabaseAlias Microsoft.EntityFrameworkCore.SqlServer
```

<span data-ttu-id="c240c-122">В следующем примере показана строка подключения, хранимая в `appsettings.json` .</span><span class="sxs-lookup"><span data-stu-id="c240c-122">Or the following example shows the connection string stored in `appsettings.json`.</span></span>

``` json
{
  "ConnectionStrings": {
    "BloggingDatabase": "Server=(localdb)\\mssqllocaldb;Database=EFGetStarted.ConsoleApp.NewDb;Trusted_Connection=True;"
  },
}
```

<span data-ttu-id="c240c-123">Затем контекст обычно настраивается в `Startup.cs` со строкой подключения, которая считывается из конфигурации.</span><span class="sxs-lookup"><span data-stu-id="c240c-123">Then the context is typically configured in `Startup.cs` with the connection string being read from configuration.</span></span> <span data-ttu-id="c240c-124">Обратите внимание, что `GetConnectionString()` метод выполняет поиск значения конфигурации, раздел которого имеет значение `ConnectionStrings:<connection string name>` .</span><span class="sxs-lookup"><span data-stu-id="c240c-124">Note the `GetConnectionString()` method looks for a configuration value whose key is `ConnectionStrings:<connection string name>`.</span></span> <span data-ttu-id="c240c-125">Чтобы использовать этот метод расширения, необходимо импортировать пространство имен [Microsoft. Extensions. Configuration](/dotnet/api/microsoft.extensions.configuration) .</span><span class="sxs-lookup"><span data-stu-id="c240c-125">You need to import the [Microsoft.Extensions.Configuration](/dotnet/api/microsoft.extensions.configuration) namespace to use this extension method.</span></span>

``` csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDbContext<BloggingContext>(options =>
        options.UseSqlServer(Configuration.GetConnectionString("BloggingDatabase")));
}
```
