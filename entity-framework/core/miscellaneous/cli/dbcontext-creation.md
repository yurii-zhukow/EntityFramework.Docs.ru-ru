---
title: Создание DbContext во время разработки — EF Core
author: bricelam
ms.author: bricelam
ms.date: 09/16/2019
uid: core/miscellaneous/cli/dbcontext-creation
ms.openlocfilehash: c36dae150085b1ab509288f6fabfdd8ed7201ca8
ms.sourcegitcommit: 2355447d89496a8ca6bcbfc0a68a14a0bf7f0327
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/23/2019
ms.locfileid: "72812015"
---
# <a name="design-time-dbcontext-creation"></a>Создание DbContext во время разработки

Для некоторых команд EF Core инструментов (например, для команд [миграции][1] ) во время разработки требуется создать производный экземпляр `DbContext`, чтобы получить сведения о типах сущностей приложения и их сопоставлении со схемой базы данных. В большинстве случаев желательно, чтобы `DbContext`, созданный таким образом, был настроен аналогично тому, как он будет [настроен во время выполнения][2].

Существуют различные способы создания `DbContext`с помощью инструментов.

## <a name="from-application-services"></a>Из служб приложений

Если запускаемый проект использует [ASP.NET Core веб-узел][3] или [универсальный узел .NET Core][4], средства пытаются получить объект DbContext от поставщика услуг приложения.

Сначала средства пытаются получить поставщик услуг путем вызова `Program.CreateHostBuilder()`, вызова `Build()`, а затем доступа к свойству `Services`.

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

Сам `DbContext` и все зависимости в его конструкторе должны быть зарегистрированы в качестве служб в поставщике услуг приложения. Для этого можно легко получить [Конструктор на `DbContext`, который принимает экземпляр `DbContextOptions<TContext>` в качестве аргумента][5] и использует [метод`AddDbContext<TContext>`][6].

## <a name="using-a-constructor-with-no-parameters"></a>Использование конструктора без параметров

Если DbContext невозможно получить от поставщика службы приложений, средства ищут производный тип `DbContext` в проекте. Затем они пытаются создать экземпляр с помощью конструктора без параметров. Это может быть конструктор по умолчанию, если `DbContext` настроен с помощью метода [`OnConfiguring`][7] .

## <a name="from-a-design-time-factory"></a>Из фабрики времени разработки

Вы также можете сообщить средствам, как создать DbContext, реализовав интерфейс `IDesignTimeDbContextFactory<TContext>`: Если класс, реализующий этот интерфейс, находится в том же проекте, что и производный `DbContext`, или в проекте запуска приложения, средства обходят другие способы создания DbContext и использования фабрики времени разработки.

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
> Параметр `args` в настоящее время не используется. Существует ошибка [при отслеживании][8] возможности указания аргументов времени разработки из средств.

Фабрика времени разработки может быть особенно полезной, если необходимо настроить DbContext по-разному для времени разработки, чем во время выполнения, если `DbContext` конструктор принимает дополнительные параметры, не зарегистрированные в DI, если вы не используете DI вообще или по какой-то причине предпочитаете не использовать метод `BuildWebHost` в классе `Main` приложения ASP.NET Core.

  [1]: xref:core/managing-schemas/migrations/index
  [2]: xref:core/miscellaneous/configuring-dbcontext
  [3]: /aspnet/core/fundamentals/host/web-host
  [4]: /aspnet/core/fundamentals/host/generic-host
  [5]: xref:core/miscellaneous/configuring-dbcontext#constructor-argument
  [6]: xref:core/miscellaneous/configuring-dbcontext#using-dbcontext-with-dependency-injection
  [7]: xref:core/miscellaneous/configuring-dbcontext#onconfiguring
  [8]: https://github.com/aspnet/EntityFrameworkCore/issues/8332
