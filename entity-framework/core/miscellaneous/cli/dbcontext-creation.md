---
title: Создание DbContext во время разработки — EF Core
author: bricelam
ms.author: bricelam
ms.date: 09/16/2019
uid: core/miscellaneous/cli/dbcontext-creation
ms.openlocfilehash: f83d4b16227d114a1cac1514667484a908fea4ac
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/23/2019
ms.locfileid: "71197573"
---
<a name="design-time-dbcontext-creation"></a>Создание DbContext во время разработки
==============================
Для некоторых команд EF Core инструментов (например, для команд [миграции][1] ) во время разработки необходимо создать производный `DbContext` экземпляр, чтобы получить сведения о типах сущностей приложения и их сопоставлении со схемой базы данных. В большинстве случаев желательно, `DbContext` чтобы созданный таким образом объект был настроен аналогично тому, как он будет [настроен во время выполнения][2].

Существует несколько способов, с помощью которых средства пытаются `DbContext`создать:

<a name="from-application-services"></a>Из служб приложений
-------------------------
Если запускаемый проект использует [ASP.NET Core веб-узел][3] или [универсальный узел .NET Core][4], средства пытаются получить объект DbContext от поставщика услуг приложения.

Сначала средства пытаются получить поставщик услуг путем вызова `Program.CreateHostBuilder()`, вызова `Build()`и доступа к `Services` свойству.

``` csharp
public class Program
{
    public static void Main(string[] args)
        => CreateHostBuilder(args).Build().Run();

    // EF Core uses this method at design time to access the DbContext
    public static IHostBuilder CreateHostBuilder(string[] args)
        => Host.CreateDefaultBuilder(args)
            .ConfigureWebHostDefaults(
                webBuilder => webBuilder.UseStartup<Startup>());
}

public class Startup
{
    public void ConfigureServices(IServiceCollection services)
        => services.AddDbContext<ApplicationDbContext>();
}

public class ApplicationDbContext : DbContext
{
    public ApplicationDbContext(DbContextOptions<ApplicationDbContext> options)
        : base(options)
    {
    }
}
```

> [!NOTE]
> При создании нового ASP.NET Core приложения этот обработчик включается по умолчанию.

Сам `DbContext` по себе и все зависимости в конструкторе должны быть зарегистрированы в качестве служб в поставщике услуг приложения. Это можно легко сделать, используя [конструктор `DbContext` , который принимает экземпляр `DbContextOptions<TContext>` ][5] [ `AddDbContext<TContext>` ][6]в качестве аргумента и использует метод.

<a name="using-a-constructor-with-no-parameters"></a>Использование конструктора без параметров
--------------------------------------
Если DbContext невозможно получить от поставщика службы приложений, средства ищут производный `DbContext` тип внутри проекта. Затем они пытаются создать экземпляр с помощью конструктора без параметров. Это может быть конструктор по умолчанию, `DbContext` если настроен [`OnConfiguring`][7] с помощью метода.

<a name="from-a-design-time-factory"></a>Из фабрики времени разработки
--------------------------
Вы также можете сообщить средствам, как создать DbContext, реализовав `IDesignTimeDbContextFactory<TContext>` интерфейс: Если класс, реализующий этот интерфейс, находится в том же проекте, что и `DbContext` производный, или в проекте запуска приложения, средства обходят другие способы создания DbContext и используют вместо нее фабрику времени разработки.

``` csharp
using Microsoft.EntityFrameworkCore;
using Microsoft.EntityFrameworkCore.Design;
using Microsoft.EntityFrameworkCore.Infrastructure;

namespace MyProject
{
    public class BloggingContextFactory : IDesignTimeDbContextFactory<BloggingContext>
    {
        public BloggingContext CreateDbContext(string[] args)
        {
            var optionsBuilder = new DbContextOptionsBuilder<BloggingContext>();
            optionsBuilder.UseSqlite("Data Source=blog.db");

            return new BloggingContext(optionsBuilder.Options);
        }
    }
}
```

> [!NOTE]
> `args` Параметр в настоящее время не используется. Существует ошибка [при отслеживании][8] возможности указания аргументов времени разработки из средств.

Фабрика времени разработки может быть особенно полезной, если необходимо настроить DbContext по-разному для времени разработки, чем во время выполнения, если `DbContext` конструктор принимает дополнительные параметры, не зарегистрированные в di, если вы не используете di вообще или для некоторых Причина, по которой не требуется использовать `BuildWebHost` метод в `Main` классе ASP.NET Core приложения.

  [1]: xref:core/managing-schemas/migrations/index
  [2]: xref:core/miscellaneous/configuring-dbcontext
  [3]: /aspnet/core/fundamentals/host/web-host
  [4]: /aspnet/core/fundamentals/host/generic-host
  [5]: xref:core/miscellaneous/configuring-dbcontext#constructor-argument
  [6]: xref:core/miscellaneous/configuring-dbcontext#using-dbcontext-with-dependency-injection
  [7]: xref:core/miscellaneous/configuring-dbcontext#onconfiguring
  [8]: https://github.com/aspnet/EntityFrameworkCore/issues/8332
