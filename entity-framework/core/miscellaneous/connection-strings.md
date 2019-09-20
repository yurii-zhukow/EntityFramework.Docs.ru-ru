---
title: Строки подключения — EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: aeb0f5f8-b212-4f89-ae83-c642a5190ba0
uid: core/miscellaneous/connection-strings
ms.openlocfilehash: ed89d6d09b15b0dea7fd8bc3ff3e3f631495ecb7
ms.sourcegitcommit: cbaa6cc89bd71d5e0bcc891e55743f0e8ea3393b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/20/2019
ms.locfileid: "71149105"
---
# <a name="connection-strings"></a>Строки подключения

Большинству поставщиков баз данных для подключения к базе данных требуется определенная форма строки подключения. Иногда эта строка подключения содержит конфиденциальную информацию, которую необходимо защитить. Также может потребоваться изменить строку подключения при перемещении приложения между средами, такими как разработка, тестирование и производство.

## <a name="winforms--wpf-applications"></a>Приложения WinForms & WPF

В приложениях WinForms, WPF и ASP.NET 4 есть предпринятая и проверенная схема строки подключения. Строка подключения должна быть добавлена в файл App. config приложения (Web. config, если используется ASP.NET). Если строка подключения содержит конфиденциальную информацию, например имя пользователя и пароль, содержимое файла конфигурации можно защитить с помощью [диспетчера секретов](https://docs.microsoft.com/aspnet/core/security/app-secrets#secret-manager).

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
> `providerName` Параметр не требуется для EF Core строк подключения, хранящихся в файле App. config, так как поставщик базы данных настраивается с помощью кода.

Затем можно прочитать строку подключения с помощью `ConfigurationManager` API в `OnConfiguring` методе контекста. Может потребоваться добавить ссылку на `System.Configuration` сборку платформы, чтобы иметь возможность использовать этот API.

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

Строки подключения в приложении UWP обычно являются подключениями SQLite, которые просто указывают локальное имя файла. Обычно они не содержат конфиденциальных сведений, и их не нужно изменять при развертывании приложения. Таким образом, эти строки подключения обычно можно оставить в коде, как показано ниже. Если вы хотите переместить их из кода, то UWP поддерживает концепцию параметров. Дополнительные сведения см. в [разделе "Параметры приложения" документации по UWP](https://docs.microsoft.com/windows/uwp/app-settings/store-and-retrieve-app-data) .

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

В ASP.NET Core система конфигурации очень гибка, и строка подключения может храниться в среде, `appsettings.json`в переменной среды, в хранилище секретов пользователя или в другом источнике конфигурации. Дополнительные сведения см. в [разделе "Конфигурация" документации по ASP.NET Core](https://docs.asp.net/en/latest/fundamentals/configuration.html) . В следующем примере показана строка подключения, хранимая в `appsettings.json`.

``` json
{
  "ConnectionStrings": {
    "BloggingDatabase": "Server=(localdb)\\mssqllocaldb;Database=EFGetStarted.ConsoleApp.NewDb;Trusted_Connection=True;"
  },
}
```

Контекст обычно настраивается в `Startup.cs` со строкой подключения, которая считывается из конфигурации. Обратите `GetConnectionString()` внимание, что метод выполняет поиск значения конфигурации, `ConnectionStrings:<connection string name>`раздел которого имеет значение. Чтобы использовать этот метод расширения, необходимо импортировать пространство имен [Microsoft. Extensions. Configuration](https://docs.microsoft.com/dotnet/api/microsoft.extensions.configuration) .

``` csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDbContext<BloggingContext>(options =>
        options.UseSqlServer(Configuration.GetConnectionString("BloggingDatabase")));
}
```
