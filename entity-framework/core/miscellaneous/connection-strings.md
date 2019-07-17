---
title: Строки подключения — EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: aeb0f5f8-b212-4f89-ae83-c642a5190ba0
uid: core/miscellaneous/connection-strings
ms.openlocfilehash: c306f9ca7a51fc9e3db18e883fd44f56dd1a3cb4
ms.sourcegitcommit: e90d6cfa3e96f10b8b5275430759a66a0c714ed1
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/17/2019
ms.locfileid: "68286449"
---
# <a name="connection-strings"></a><span data-ttu-id="49088-102">Строки подключения</span><span class="sxs-lookup"><span data-stu-id="49088-102">Connection Strings</span></span>

<span data-ttu-id="49088-103">Большинство поставщиков баз данных требуется какая-либо разновидность строку подключения для подключения к базе данных.</span><span class="sxs-lookup"><span data-stu-id="49088-103">Most database providers require some form of connection string to connect to the database.</span></span> <span data-ttu-id="49088-104">Иногда эта строка подключения содержит конфиденциальные сведения, которые необходимо защитить.</span><span class="sxs-lookup"><span data-stu-id="49088-104">Sometimes this connection string contains sensitive information that needs to be protected.</span></span> <span data-ttu-id="49088-105">Кроме того, может потребоваться изменить строку подключения, при перемещении приложения между средами, например, разработки, тестирования и эксплуатации.</span><span class="sxs-lookup"><span data-stu-id="49088-105">You may also need to change the connection string as you move your application between environments, such as development, testing, and production.</span></span>

## <a name="net-framework-applications"></a><span data-ttu-id="49088-106">Приложения .NET framework</span><span class="sxs-lookup"><span data-stu-id="49088-106">.NET Framework Applications</span></span>

<span data-ttu-id="49088-107">Приложения .NET framework, такие как WinForms, WPF, консоли и ASP.NET 4, имеют строковый шаблон с помощью проверенная подключения.</span><span class="sxs-lookup"><span data-stu-id="49088-107">.NET Framework applications, such as WinForms, WPF, Console, and ASP.NET 4, have a tried and tested connection string pattern.</span></span> <span data-ttu-id="49088-108">Строка подключения должны добавляться в файл App.config приложения (Web.config при использовании ASP.NET).</span><span class="sxs-lookup"><span data-stu-id="49088-108">The connection string should be added to your application's App.config file (Web.config if you are using ASP.NET).</span></span> <span data-ttu-id="49088-109">Если строка подключения содержит конфиденциальные сведения, такие как имя пользователя и пароль, чтобы защитить содержимое файла конфигурации с помощью [защищенной конфигурации](https://docs.microsoft.com/dotnet/framework/data/adonet/connection-strings-and-configuration-files#encrypting-configuration-file-sections-using-protected-configuration).</span><span class="sxs-lookup"><span data-stu-id="49088-109">If your connection string contains sensitive information, such as username and password, you can protect the contents of the configuration file using [Protected Configuration](https://docs.microsoft.com/dotnet/framework/data/adonet/connection-strings-and-configuration-files#encrypting-configuration-file-sections-using-protected-configuration).</span></span>

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
> <span data-ttu-id="49088-110">`providerName` Параметр не является обязательным для строки подключения EF Core, хранятся в файле App.config, так как поставщик базы данных настраивается с помощью кода.</span><span class="sxs-lookup"><span data-stu-id="49088-110">The `providerName` setting is not required on EF Core connection strings stored in App.config because the database provider is configured via code.</span></span>

<span data-ttu-id="49088-111">Вы сможете прочесть строку соединения с помощью `ConfigurationManager` API в вашем контексте `OnConfiguring` метод.</span><span class="sxs-lookup"><span data-stu-id="49088-111">You can then read the connection string using the `ConfigurationManager` API in your context's `OnConfiguring` method.</span></span> <span data-ttu-id="49088-112">Может потребоваться добавить ссылку на `System.Configuration` framework сборки, чтобы иметь возможность использовать этот API.</span><span class="sxs-lookup"><span data-stu-id="49088-112">You may need to add a reference to the `System.Configuration` framework assembly to be able to use this API.</span></span>

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

## <a name="universal-windows-platform-uwp"></a><span data-ttu-id="49088-113">Универсальная платформа Windows (UWP)</span><span class="sxs-lookup"><span data-stu-id="49088-113">Universal Windows Platform (UWP)</span></span>

<span data-ttu-id="49088-114">Строки подключения в приложении универсальной платформы Windows обычно являются SQLite подключение, которое просто задает имя локального файла.</span><span class="sxs-lookup"><span data-stu-id="49088-114">Connection strings in a UWP application are typically a SQLite connection that just specifies a local filename.</span></span> <span data-ttu-id="49088-115">Обычно они не содержат конфиденциальной информации и не нужно изменить, так как приложение развертывается.</span><span class="sxs-lookup"><span data-stu-id="49088-115">They typically do not contain sensitive information, and do not need to be changed as an application is deployed.</span></span> <span data-ttu-id="49088-116">Таким образом эти строки подключения являются обычно нормально, которое должно остаться в коде, как показано ниже.</span><span class="sxs-lookup"><span data-stu-id="49088-116">As such, these connection strings are usually fine to be left in code, as shown below.</span></span> <span data-ttu-id="49088-117">Если вы хотите переместить их за пределы кода универсальной платформы Windows поддерживает концепцию параметров, см. в разделе [разделе параметров приложения UWP](https://docs.microsoft.com/windows/uwp/app-settings/store-and-retrieve-app-data) сведения.</span><span class="sxs-lookup"><span data-stu-id="49088-117">If you wish to move them out of code then UWP supports the concept of settings, see the [App Settings section of the UWP documentation](https://docs.microsoft.com/windows/uwp/app-settings/store-and-retrieve-app-data) for details.</span></span>

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

## <a name="aspnet-core"></a><span data-ttu-id="49088-118">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="49088-118">ASP.NET Core</span></span>

<span data-ttu-id="49088-119">В ASP.NET Core в системе конфигурации является очень гибкой, а строка подключения, которые могут храниться в `appsettings.json`, переменную среды, хранилище секретов пользователя или другого источника конфигурации.</span><span class="sxs-lookup"><span data-stu-id="49088-119">In ASP.NET Core the configuration system is very flexible, and the connection string could be stored in `appsettings.json`, an environment variable, the user secret store, or another configuration source.</span></span> <span data-ttu-id="49088-120">См. в разделе [раздел конфигурации из документации по ASP.NET Core](https://docs.asp.net/en/latest/fundamentals/configuration.html) для получения дополнительных сведений.</span><span class="sxs-lookup"><span data-stu-id="49088-120">See the [Configuration section of the ASP.NET Core documentation](https://docs.asp.net/en/latest/fundamentals/configuration.html) for more details.</span></span> <span data-ttu-id="49088-121">В следующем примере показано строку подключения, сохраненную в `appsettings.json`.</span><span class="sxs-lookup"><span data-stu-id="49088-121">The following example shows the connection string stored in `appsettings.json`.</span></span>

``` json
{
  "ConnectionStrings": {
    "BloggingDatabase": "Server=(localdb)\\mssqllocaldb;Database=EFGetStarted.ConsoleApp.NewDb;Trusted_Connection=True;"
  },
}
```

<span data-ttu-id="49088-122">Контекст обычно настраивается в `Startup.cs` строкой подключения, считываемого из конфигурации.</span><span class="sxs-lookup"><span data-stu-id="49088-122">The context is typically configured in `Startup.cs` with the connection string being read from configuration.</span></span> <span data-ttu-id="49088-123">Примечание `GetConnectionString()` метод ищет значение конфигурации, ключ которого `ConnectionStrings:<connection string name>`.</span><span class="sxs-lookup"><span data-stu-id="49088-123">Note the `GetConnectionString()` method looks for a configuration value whose key is `ConnectionStrings:<connection string name>`.</span></span> <span data-ttu-id="49088-124">Необходимо импортировать [Microsoft.Extensions.Configuration](https://docs.microsoft.com/dotnet/api/microsoft.extensions.configuration) пространство имен для использования этого метода расширения.</span><span class="sxs-lookup"><span data-stu-id="49088-124">You need to import the [Microsoft.Extensions.Configuration](https://docs.microsoft.com/dotnet/api/microsoft.extensions.configuration) namespace to use this extension method.</span></span>

``` csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDbContext<BloggingContext>(options =>
        options.UseSqlServer(Configuration.GetConnectionString("BloggingDatabase")));
}
```
