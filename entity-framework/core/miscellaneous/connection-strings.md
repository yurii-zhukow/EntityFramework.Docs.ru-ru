---
title: Строки подключения — EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: aeb0f5f8-b212-4f89-ae83-c642a5190ba0
uid: core/miscellaneous/connection-strings
ms.openlocfilehash: 52a8527170845d3e73ebcec518713ade3f3844f0
ms.sourcegitcommit: 06073f8efde97dd5f540dbfb69f574d8380566fe
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/24/2019
ms.locfileid: "67333839"
---
# <a name="connection-strings"></a>Строки подключения

Большинство поставщиков баз данных требуется какая-либо разновидность строку подключения для подключения к базе данных. Иногда эта строка подключения содержит конфиденциальные сведения, которые необходимо защитить. Кроме того, может потребоваться изменить строку подключения, при перемещении приложения между средами, например, разработки, тестирования и эксплуатации.

## <a name="net-framework-applications"></a>Приложения .NET framework

Приложения .NET framework, такие как WinForms, WPF, консоли и ASP.NET 4, имеют строковый шаблон с помощью проверенная подключения. Строка подключения должны добавляться в файл App.config приложения (Web.config при использовании ASP.NET). Если строка подключения содержит конфиденциальные сведения, такие как имя пользователя и пароль, чтобы защитить содержимое файла конфигурации с помощью [защищенной конфигурации](https://docs.microsoft.com/dotnet/framework/data/adonet/connection-strings-and-configuration-files#encrypting-configuration-file-sections-using-protected-configuration).

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
> `providerName` Параметр не является обязательным для строки подключения EF Core, хранятся в файле App.config, так как поставщик базы данных настраивается с помощью кода.

Вы сможете прочесть строку соединения с помощью `ConfigurationManager` API в вашем контексте `OnConfiguring` метод. Может потребоваться добавить ссылку на `System.Configuration` framework сборки, чтобы иметь возможность использовать этот API.

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

## <a name="universal-windows-platform-uwp"></a>Универсальная платформа Windows (UWP)

Строки подключения в приложении универсальной платформы Windows обычно являются SQLite подключение, которое просто задает имя локального файла. Обычно они не содержат конфиденциальной информации и не нужно изменить, так как приложение развертывается. Таким образом эти строки подключения являются обычно нормально, которое должно остаться в коде, как показано ниже. Если вы хотите переместить их за пределы кода универсальной платформы Windows поддерживает концепцию параметров, см. в разделе [разделе параметров приложения UWP](https://docs.microsoft.com/windows/uwp/app-settings/store-and-retrieve-app-data) сведения.

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

## <a name="aspnet-core"></a>ASP.NET Core

В ASP.NET Core в системе конфигурации является очень гибкой, а строка подключения, которые могут храниться в `appsettings.json`, переменную среды, хранилище секретов пользователя или другого источника конфигурации. См. в разделе [раздел конфигурации из документации по ASP.NET Core](https://docs.asp.net/en/latest/fundamentals/configuration.html) для получения дополнительных сведений. В следующем примере показано строку подключения, сохраненную в `appsettings.json`.

``` json
{
  "ConnectionStrings": {
    "BloggingDatabase": "Server=(localdb)\\mssqllocaldb;Database=EFGetStarted.ConsoleApp.NewDb;Trusted_Connection=True;"
  },
}
```

Контекст обычно настраивается в `Startup.cs` строкой подключения, считываемого из конфигурации. Примечание `GetConnectionString()` метод ищет значение конфигурации, ключ которого `ConnectionStrings:<connection string name>`. Необходимо импортировать [Microsoft.Extensions.Configuration](https://docs.microsoft.com/dotnet/api/microsoft.extensions.configuration) пространство имен для использования этого метода расширения.

``` csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDbContext<BloggingContext>(options =>
        options.UseSqlServer(Configuration.GetConnectionString("BloggingDatabase")));
}
```
