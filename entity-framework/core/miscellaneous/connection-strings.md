---
title: Строки подключения — EF Core
description: Управление строками подключения в разных средах с помощью Entity Framework Core
author: bricelam
ms.date: 10/27/2016
uid: core/miscellaneous/connection-strings
ms.openlocfilehash: f657d39f66e6a757380ca25436a638b47c11cd12
ms.sourcegitcommit: 0a25c03fa65ae6e0e0e3f66bac48d59eceb96a5a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/14/2020
ms.locfileid: "92062325"
---
# <a name="connection-strings"></a>Строки соединения

Большинству поставщиков баз данных для подключения к базе данных требуется определенная форма строки подключения. Иногда эта строка подключения содержит конфиденциальную информацию, которую необходимо защитить. Также может потребоваться изменить строку подключения при перемещении приложения между средами, такими как разработка, тестирование и производство.

## <a name="winforms--wpf-applications"></a>Приложения WinForms & WPF

В приложениях WinForms, WPF и ASP.NET 4 есть предпринятая и проверенная схема строки подключения. Строка подключения должна быть добавлена в файл App.config приложения (Web.config при использовании ASP.NET). Если строка подключения содержит конфиденциальную информацию, например имя пользователя и пароль, содержимое файла конфигурации можно защитить с помощью [диспетчера секретов](/aspnet/core/security/app-secrets#secret-manager).

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>

  <connectionStrings>
    <add name="BloggingDatabase"
         connectionString="Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" />
  </connectionStrings>
</configuration>
```

> [!TIP]
> `providerName`Параметр не требуется для EF Core строк подключения, хранящихся в App.config, так как поставщик базы данных настраивается с помощью кода.

Затем можно прочитать строку подключения с помощью `ConfigurationManager` API в `OnConfiguring` методе контекста. Чтобы использовать этот API, может потребоваться добавить ссылку на сборку платформы `System.Configuration`.

```csharp
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

```csharp
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

В ASP.NET Core система конфигурации очень гибка, и строка подключения может храниться в `appsettings.json` среде, в переменной среды, в хранилище секретов пользователя или в другом источнике конфигурации. Дополнительные сведения см. в [разделе "Конфигурация" документации по ASP.NET Core](/aspnet/core/fundamentals/configuration) .

Например, можно использовать [Диспетчер секретов](/aspnet/core/security/app-secrets#secret-manager) для хранения пароля базы данных, а затем в формировании шаблонов использовать строку подключения, которая просто состоит из `Name=<database-alias>` .

```dotnetcli
dotnet user-secrets set ConnectionStrings.YourDatabaseAlias "Data Source=(localdb)\MSSQLLocalDB;Initial Catalog=YourDatabase"
dotnet ef dbcontext scaffold Name=ConnectionStrings.YourDatabaseAlias Microsoft.EntityFrameworkCore.SqlServer
```

В следующем примере показана строка подключения, хранимая в `appsettings.json` .

```json
{
  "ConnectionStrings": {
    "BloggingDatabase": "Server=(localdb)\\mssqllocaldb;Database=EFGetStarted.ConsoleApp.NewDb;Trusted_Connection=True;"
  },
}
```

Затем контекст обычно настраивается в `Startup.cs` со строкой подключения, которая считывается из конфигурации. Обратите внимание, что `GetConnectionString()` метод выполняет поиск значения конфигурации, раздел которого имеет значение `ConnectionStrings:<connection string name>` . Чтобы использовать этот метод расширения, необходимо импортировать пространство имен [Microsoft.Extensions.Configфигурации](/dotnet/api/microsoft.extensions.configuration) .

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDbContext<BloggingContext>(options =>
        options.UseSqlServer(Configuration.GetConnectionString("BloggingDatabase")));
}
```
