---
title: "Строки подключения - EF Core"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: aeb0f5f8-b212-4f89-ae83-c642a5190ba0
ms.technology: entity-framework-core
uid: core/miscellaneous/connection-strings
ms.openlocfilehash: b4ed01f0452d74ac49d3fde780caa5f1b25a6e97
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/27/2017
---
# <a name="connection-strings"></a>Строки подключения

Большинство поставщиков базы данных требуют отдельной строки подключения к базе данных. Иногда эта строка подключения содержит конфиденциальные сведения, которые необходимо защитить. Также может потребоваться изменить строку подключения при переходах средах, таких как разработка, тестирование и производственного приложения.

## <a name="net-framework-applications"></a>Приложения .NET framework

Приложения .NET framework, такие как WinForms, WPF, консоли и ASP.NET 4, будет иметь подключения попытка и проверенный строки. Строка подключения должна быть добавлена в файл App.config приложения (Web.config при использовании ASP.NET). Если строка подключения содержит конфиденциальные сведения, например имя пользователя и пароль, можно защитить содержимое файла конфигурации с помощью [защищенной конфигурации](https://docs.microsoft.com/dotnet/framework/data/adonet/connection-strings-and-configuration-files#encrypting-configuration-file-sections-using-protected-configuration).

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
> `providerName` Параметр не является обязательным в строках подключения EF Core хранятся в файле App.config, так как поставщик базы данных настраивается с помощью кода.

Затем можно прочитать строку соединения с помощью `ConfigurationManager` API в его контексте `OnConfiguring` метод. Может потребоваться добавить ссылку на `System.Configuration` framework сборки, чтобы иметь возможность использовать этот API.

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

Строки подключения в приложении UWP обычно являются SQLite соединение, просто задает имя локального файла. Обычно они не содержат конфиденциальной информации и не нужно изменить, так как приложение развертывается. Таким образом эти строки подключения обычно нормально, которое должно остаться в коде, как показано ниже. Если вы хотите переместить их в коде UWP поддерживает концепцию параметров см. в разделе [разделе параметров приложения UWP](https://docs.microsoft.com/windows/uwp/app-settings/store-and-retrieve-app-data) подробные сведения.

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

В ASP.NET Core системы конфигурации является более гибким и строки подключения могут храниться в `appsettings.json`, переменную среды, хранилище секрета пользователя или другой источник конфигурации. В разделе [раздела конфигурации ASP.NET Core документации](https://docs.asp.net/en/latest/fundamentals/configuration.html) для получения дополнительных сведений. В следующем примере показано строку подключения, сохраненную в `appsettings.json`.

``` json
{
  "ConnectionStrings": {
    "BloggingDatabase": "Server=(localdb)\\mssqllocaldb;Database=EFGetStarted.ConsoleApp.NewDb;Trusted_Connection=True;"
  },
}
```

Контекст обычно настраивается в `Startup.cs` со строкой соединения, считываемое из конфигурации. Примечание `GetConnectionString()` метод ищет значение конфигурации, ключ которого является `ConnectionStrings:<connection string name>`.

``` csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDbContext<BloggingContext>(options =>
        options.UseSqlServer(Configuration.GetConnectionString("BloggingDatabase")));
}
```
