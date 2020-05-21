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
# <a name="connection-strings"></a>Строки соединения

Большинству поставщиков баз данных для подключения к базе данных требуется определенная форма строки подключения. Иногда эта строка подключения содержит конфиденциальную информацию, которую необходимо защитить. Также может потребоваться изменить строку подключения при перемещении приложения между средами, такими как разработка, тестирование и производство.

## <a name="winforms--wpf-applications"></a>Приложения WinForms & WPF

В приложениях WinForms, WPF и ASP.NET 4 есть предпринятая и проверенная схема строки подключения. Строка подключения должна быть добавлена в файл App. config приложения (Web. config, если используется ASP.NET). Если строка подключения содержит конфиденциальную информацию, например имя пользователя и пароль, содержимое файла конфигурации можно защитить с помощью [диспетчера секретов](/aspnet/core/security/app-secrets#secret-manager).

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
> `providerName`Параметр не требуется для EF Core строк подключения, хранящихся в файле App. config, так как поставщик базы данных настраивается с помощью кода.

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

Строки подключения в приложении UWP обычно являются подключениями SQLite, которые просто указывают локальное имя файла. Обычно они не содержат конфиденциальных сведений, и их не нужно изменять при развертывании приложения. Таким образом, эти строки подключения обычно можно оставить в коде, как показано ниже. Если вы хотите переместить их из кода, то UWP поддерживает концепцию параметров. Дополнительные сведения см. в [разделе "Параметры приложения" документации по UWP](/windows/uwp/app-settings/store-and-retrieve-app-data) .

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

## <a name="aspnet-core"></a>ASP.NET Core

В ASP.NET Core система конфигурации очень гибка, и строка подключения может храниться в `appsettings.json` среде, в переменной среды, в хранилище секретов пользователя или в другом источнике конфигурации. Дополнительные сведения см. в [разделе "Конфигурация" документации по ASP.NET Core](/aspnet/core/fundamentals/configuration) .

Например, можно использовать [Диспетчер секретов](/aspnet/core/security/app-secrets#secret-manager) для хранения пароля базы данных, а затем в формировании шаблонов использовать строку подключения, которая просто состоит из `Name=<database-alias>` .

```dotnetcli
dotnet user-secrets set ConnectionStrings.YourDatabaseAlias "Data Source=(localdb)\MSSQLLocalDB;Initial Catalog=YourDatabase"
dotnet ef dbcontext scaffold Name=YourDatabaseAlias Microsoft.EntityFrameworkCore.SqlServer
```

В следующем примере показана строка подключения, хранимая в `appsettings.json` .

``` json
{
  "ConnectionStrings": {
    "BloggingDatabase": "Server=(localdb)\\mssqllocaldb;Database=EFGetStarted.ConsoleApp.NewDb;Trusted_Connection=True;"
  },
}
```

Затем контекст обычно настраивается в `Startup.cs` со строкой подключения, которая считывается из конфигурации. Обратите внимание, что `GetConnectionString()` метод выполняет поиск значения конфигурации, раздел которого имеет значение `ConnectionStrings:<connection string name>` . Чтобы использовать этот метод расширения, необходимо импортировать пространство имен [Microsoft. Extensions. Configuration](/dotnet/api/microsoft.extensions.configuration) .

``` csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDbContext<BloggingContext>(options =>
        options.UseSqlServer(Configuration.GetConnectionString("BloggingDatabase")));
}
```
