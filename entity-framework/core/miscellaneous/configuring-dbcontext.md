---
title: "Настройка DbContext - EF Core"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: d7a22b5a-4c5b-4e3b-9897-4d7320fcd13f
ms.technology: entity-framework-core
uid: core/miscellaneous/configuring-dbcontext
ms.openlocfilehash: 96abf3b94be3e1d19f833644f1c2f6f13fe0e730
ms.sourcegitcommit: 860ec5d047342fbc4063a0de881c9861cc1f8813
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/05/2017
---
# <a name="configuring-a-dbcontext"></a>Настройка DbContext

В этой статье показано шаблоны для настройки `DbContext` с `DbContextOptions`. Параметры в основном используются для выбора и настройки хранилища данных.

## <a name="configuring-dbcontextoptions"></a>Настройка DbContextOptions

`DbContext`необходимо иметь установленный экземпляр `DbContextOptions` для выполнения. Это можно сделать путем переопределения `OnConfiguring`, или извне, предоставленных через аргумент конструктора.

Если используются оба `OnConfiguring` выполняется на предоставленных параметрах, а значит она — друг к другу и может перезаписать значениями, заданными в аргументе конструктора.

### <a name="constructor-argument"></a>Аргумент конструктора

Контекст кода с помощью конструктора

``` csharp
public class BloggingContext : DbContext
{
    public BloggingContext(DbContextOptions<BloggingContext> options)
        : base(options)
    { }

    public DbSet<Blog> Blogs { get; set; }
}
```

> [!TIP]  
> Базовый конструктор DbContext также принимает неуниверсальном версии `DbContextOptions`. С помощью неуниверсальная версия не рекомендуется для приложений с несколькими типами контекста.

Код приложения для инициализации из аргумента конструктора

``` csharp
var optionsBuilder = new DbContextOptionsBuilder<BloggingContext>();
optionsBuilder.UseSqlite("Data Source=blog.db");

using (var context = new BloggingContext(optionsBuilder.Options))
{
  // do stuff
}
```

### <a name="onconfiguring"></a>OnConfiguring

> [!WARNING]  
> `OnConfiguring`происходит в последнюю очередь и перезаписывают параметры, полученные из DI или конструктора. Этот подход непригодны для тестирования (если целевая базы данных).

Контекст кода с помощью `OnConfiguring`:

``` csharp
public class BloggingContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }

    protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    {
        optionsBuilder.UseSqlite("Data Source=blog.db");
    }
}
```

Код приложения для инициализации с `OnConfiguring`:

``` csharp
using (var context = new BloggingContext())
{
  // do stuff
}
```

## <a name="using-dbcontext-with-dependency-injection"></a>Внедрение зависимостей при помощи DbContext

EF поддерживает использование `DbContext` с контейнер внедрения зависимостей. Тип DbContext можно добавить в контейнер службы с помощью `AddDbContext<TContext>`.

`AddDbContext`сделает как ваш тип DbContext `TContext`, и `DbContextOptions<TContext>` для внедрения из контейнера службы.

В разделе [дополнительные чтения](#more-reading) ниже сведения о внедрения зависимостей.

Добавление dbcontext внедрения зависимости

``` csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDbContext<BloggingContext>(options => options.UseSqlite("Data Source=blog.db"));
}
```

Это требует добавления [аргумента конструктора](#constructor-argument) типу DbContext, принимающий `DbContextOptions`.

Контекст кода:

``` csharp
public class BloggingContext : DbContext
{
    public BloggingContext(DbContextOptions<BloggingContext> options)
      :base(options)
    { }

    public DbSet<Blog> Blogs { get; set; }
}
```

Код приложения (в ASP.NET Core):

``` csharp
public MyController(BloggingContext context)
```

Код приложения (с использованием ServiceProvider напрямую, реже):

``` csharp
using (var context = serviceProvider.GetService<BloggingContext>())
{
  // do stuff
}

var options = serviceProvider.GetService<DbContextOptions<BloggingContext>>();
```

## <a name="using-idesigntimedbcontextfactorytcontext"></a>С помощью`IDesignTimeDbContextFactory<TContext>`

В качестве альтернативы для вариантов, представленных выше, может также предоставлять реализацию метода `IDesignTimeDbContextFactory<TContext>`. EF средства могут использовать эту фабрику для создания экземпляра вашей DbContext. Это может потребоваться для включения определенных взаимодействия времени разработки, такие как миграция.

Реализуйте этот интерфейс для службы времени проектирования для типов контекста, у которых нет открытого конструктора по умолчанию. Реализации этого интерфейса, которые находятся в той же сборке, как производный контекст автоматически обнаруживает использование службы во время разработки.

Пример

``` csharp
using Microsoft.EntityFrameworkCore;
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

## <a name="more-reading"></a>Дополнительные чтения

* Чтение [Приступая к работе в ASP.NET Core](../get-started/aspnetcore/index.md) Дополнительные сведения об использовании EF с ASP.NET Core.
* Чтение [внедрения зависимостей](https://docs.asp.net/en/latest/fundamentals/dependency-injection.html) для получения дополнительных сведений об использовании DI.
* Чтение [тестирования](testing/index.md) для получения дополнительной информации.
