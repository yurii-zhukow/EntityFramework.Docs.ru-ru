---
title: Строки подключения — EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: aeb0f5f8-b212-4f89-ae83-c642a5190ba0
uid: core/miscellaneous/connection-strings
ms.openlocfilehash: ed89d6d09b15b0dea7fd8bc3ff3e3f631495ecb7
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/06/2020
ms.locfileid: "78414095"
---
# <a name="connection-strings"></a><span data-ttu-id="0ad04-102">Строки соединения</span><span class="sxs-lookup"><span data-stu-id="0ad04-102">Connection Strings</span></span>

<span data-ttu-id="0ad04-103">Большинству поставщиков баз данных для подключения к базе данных требуется определенная форма строки подключения.</span><span class="sxs-lookup"><span data-stu-id="0ad04-103">Most database providers require some form of connection string to connect to the database.</span></span> <span data-ttu-id="0ad04-104">Иногда эта строка подключения содержит конфиденциальную информацию, которую необходимо защитить.</span><span class="sxs-lookup"><span data-stu-id="0ad04-104">Sometimes this connection string contains sensitive information that needs to be protected.</span></span> <span data-ttu-id="0ad04-105">Также может потребоваться изменить строку подключения при перемещении приложения между средами, такими как разработка, тестирование и производство.</span><span class="sxs-lookup"><span data-stu-id="0ad04-105">You may also need to change the connection string as you move your application between environments, such as development, testing, and production.</span></span>

## <a name="winforms--wpf-applications"></a><span data-ttu-id="0ad04-106">Приложения WinForms & WPF</span><span class="sxs-lookup"><span data-stu-id="0ad04-106">WinForms & WPF Applications</span></span>

<span data-ttu-id="0ad04-107">В приложениях WinForms, WPF и ASP.NET 4 есть предпринятая и проверенная схема строки подключения.</span><span class="sxs-lookup"><span data-stu-id="0ad04-107">WinForms, WPF, and ASP.NET 4 applications have a tried and tested connection string pattern.</span></span> <span data-ttu-id="0ad04-108">Строка подключения должна быть добавлена в файл App. config приложения (Web. config, если используется ASP.NET).</span><span class="sxs-lookup"><span data-stu-id="0ad04-108">The connection string should be added to your application's App.config file (Web.config if you are using ASP.NET).</span></span> <span data-ttu-id="0ad04-109">Если строка подключения содержит конфиденциальную информацию, например имя пользователя и пароль, содержимое файла конфигурации можно защитить с помощью [диспетчера секретов](https://docs.microsoft.com/aspnet/core/security/app-secrets#secret-manager).</span><span class="sxs-lookup"><span data-stu-id="0ad04-109">If your connection string contains sensitive information, such as username and password, you can protect the contents of the configuration file using the [Secret Manager tool](https://docs.microsoft.com/aspnet/core/security/app-secrets#secret-manager).</span></span>

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
> <span data-ttu-id="0ad04-110">Параметр `providerName` не требуется для строк подключения EF Core, хранящихся в файле App. config, так как поставщик базы данных настраивается с помощью кода.</span><span class="sxs-lookup"><span data-stu-id="0ad04-110">The `providerName` setting is not required on EF Core connection strings stored in App.config because the database provider is configured via code.</span></span>

<span data-ttu-id="0ad04-111">Затем можно прочитать строку подключения с помощью `ConfigurationManager` API в методе `OnConfiguring` контекста.</span><span class="sxs-lookup"><span data-stu-id="0ad04-111">You can then read the connection string using the `ConfigurationManager` API in your context's `OnConfiguring` method.</span></span> <span data-ttu-id="0ad04-112">Чтобы использовать этот API, может потребоваться добавить ссылку на сборку `System.Configuration` Framework.</span><span class="sxs-lookup"><span data-stu-id="0ad04-112">You may need to add a reference to the `System.Configuration` framework assembly to be able to use this API.</span></span>

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

## <a name="universal-windows-platform-uwp"></a><span data-ttu-id="0ad04-113">Универсальная платформа Windows (UWP)</span><span class="sxs-lookup"><span data-stu-id="0ad04-113">Universal Windows Platform (UWP)</span></span>

<span data-ttu-id="0ad04-114">Строки подключения в приложении UWP обычно являются подключениями SQLite, которые просто указывают локальное имя файла.</span><span class="sxs-lookup"><span data-stu-id="0ad04-114">Connection strings in a UWP application are typically a SQLite connection that just specifies a local filename.</span></span> <span data-ttu-id="0ad04-115">Обычно они не содержат конфиденциальных сведений, и их не нужно изменять при развертывании приложения.</span><span class="sxs-lookup"><span data-stu-id="0ad04-115">They typically do not contain sensitive information, and do not need to be changed as an application is deployed.</span></span> <span data-ttu-id="0ad04-116">Таким образом, эти строки подключения обычно можно оставить в коде, как показано ниже.</span><span class="sxs-lookup"><span data-stu-id="0ad04-116">As such, these connection strings are usually fine to be left in code, as shown below.</span></span> <span data-ttu-id="0ad04-117">Если вы хотите переместить их из кода, то UWP поддерживает концепцию параметров. Дополнительные сведения см. в [разделе "Параметры приложения" документации по UWP](https://docs.microsoft.com/windows/uwp/app-settings/store-and-retrieve-app-data) .</span><span class="sxs-lookup"><span data-stu-id="0ad04-117">If you wish to move them out of code then UWP supports the concept of settings, see the [App Settings section of the UWP documentation](https://docs.microsoft.com/windows/uwp/app-settings/store-and-retrieve-app-data) for details.</span></span>

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

## <a name="aspnet-core"></a><span data-ttu-id="0ad04-118">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="0ad04-118">ASP.NET Core</span></span>

<span data-ttu-id="0ad04-119">В ASP.NET Core система конфигурации очень гибка, и строка подключения может храниться в `appsettings.json`, переменной среды, хранилище секретов пользователя или другом источнике конфигурации.</span><span class="sxs-lookup"><span data-stu-id="0ad04-119">In ASP.NET Core the configuration system is very flexible, and the connection string could be stored in `appsettings.json`, an environment variable, the user secret store, or another configuration source.</span></span> <span data-ttu-id="0ad04-120">Дополнительные сведения см. в [разделе "Конфигурация" документации по ASP.NET Core](https://docs.asp.net/en/latest/fundamentals/configuration.html) .</span><span class="sxs-lookup"><span data-stu-id="0ad04-120">See the [Configuration section of the ASP.NET Core documentation](https://docs.asp.net/en/latest/fundamentals/configuration.html) for more details.</span></span> <span data-ttu-id="0ad04-121">В следующем примере показана строка подключения, хранящаяся в `appsettings.json`.</span><span class="sxs-lookup"><span data-stu-id="0ad04-121">The following example shows the connection string stored in `appsettings.json`.</span></span>

``` json
{
  "ConnectionStrings": {
    "BloggingDatabase": "Server=(localdb)\\mssqllocaldb;Database=EFGetStarted.ConsoleApp.NewDb;Trusted_Connection=True;"
  },
}
```

<span data-ttu-id="0ad04-122">Контекст обычно настраивается в `Startup.cs` со строкой подключения, которая считывается из конфигурации.</span><span class="sxs-lookup"><span data-stu-id="0ad04-122">The context is typically configured in `Startup.cs` with the connection string being read from configuration.</span></span> <span data-ttu-id="0ad04-123">Обратите внимание, что метод `GetConnectionString()` ищет значение конфигурации, ключ которого `ConnectionStrings:<connection string name>`.</span><span class="sxs-lookup"><span data-stu-id="0ad04-123">Note the `GetConnectionString()` method looks for a configuration value whose key is `ConnectionStrings:<connection string name>`.</span></span> <span data-ttu-id="0ad04-124">Чтобы использовать этот метод расширения, необходимо импортировать пространство имен [Microsoft. Extensions. Configuration](https://docs.microsoft.com/dotnet/api/microsoft.extensions.configuration) .</span><span class="sxs-lookup"><span data-stu-id="0ad04-124">You need to import the [Microsoft.Extensions.Configuration](https://docs.microsoft.com/dotnet/api/microsoft.extensions.configuration) namespace to use this extension method.</span></span>

``` csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDbContext<BloggingContext>(options =>
        options.UseSqlServer(Configuration.GetConnectionString("BloggingDatabase")));
}
```
