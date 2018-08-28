---
title: Настройка DbContext — EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: d7a22b5a-4c5b-4e3b-9897-4d7320fcd13f
uid: core/miscellaneous/configuring-dbcontext
ms.openlocfilehash: 393349c05ffaf42c6d2520e73abce23def6becc0
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/27/2018
ms.locfileid: "42995942"
---
# <a name="configuring-a-dbcontext"></a>Настройка DbContext

В этой статье показаны базовые шаблоны для настройки `DbContext` через `DbContextOptions` для подключения к базе данных с помощью определенного поставщика EF Core и необязательного поведения.

## <a name="design-time-dbcontext-configuration"></a>Конфигурация DbContext во время разработки

Средства EF Core во время разработки, такие как [миграций](xref:core/managing-schemas/migrations/index) должны иметь возможность находить и создать экземпляр рабочего `DbContext` тип для сбора сведений о типах сущностей и их сопоставлении схеме базы данных приложения. Этот процесс может выполняться автоматически, до тех пор, пока средство может легко создавать `DbContext` таким образом, что он будет назначен точно так же как будет настраиваться во время выполнения.

Хотя любой шаблон, который предоставляет сведения о конфигурации, необходимые для `DbContext` можно работать во время выполнения, средства, которые требуют использования `DbContext` во время разработки можно работать только с ограниченный набор шаблонов. Эти файлы рассматриваются более подробно в [создание контекста разработки](xref:core/miscellaneous/cli/dbcontext-creation) раздел.

## <a name="configuring-dbcontextoptions"></a>Настройка DbContextOptions

`DbContext` необходимо иметь экземпляр `DbContextOptions` для какой-либо работы. `DbContextOptions` Экземпляр содержит сведения о конфигурации, такие как:

- Поставщик базы данных для использования, обычно выбирается путем вызова метода, такие как `UseSqlServer` или `UseSqlite`
- Любой необходимую строку подключения или идентификатор экземпляра базы данных, обычно передается в качестве аргумента в метод выбора поставщика, упомянутых выше
- Любое необязательное поведение уровня поставщика селекторы, также цепочку внутри вызова метода выбора поставщика
- Все общие селекторы поведение EF Core, обычно связанных после или до метода выбора поставщика

В следующем примере настраивается `DbContextOptions` использование поставщика SQL Server, подключение содержащихся в `connectionString` переменную, время ожидания команды уровня поставщика и селектор поведения EF Core, который делает все запросы, выполняемые в `DbContext` [Отключение отслеживания](xref:core/querying/tracking#no-tracking-queries) по умолчанию:

``` csharp
optionsBuilder
    .UseSqlServer(connectionString, providerOptions=>providerOptions.CommandTimeout(60))
    .UseQueryTrackingBehavior(QueryTrackingBehavior.NoTracking);
```

> [!NOTE]  
> Методы выбора поставщика и другие методы выбора поведение, упомянутых выше являются методами расширений в `DbContextOptions` или классы поставщика параметров. Чтобы получить доступ к эти методы расширения, может потребоваться пространство имен (обычно `Microsoft.EntityFrameworkCore`) в области видимости и включить зависимости дополнительных пакетов в проект.

`DbContextOptions` Может быть задано для `DbContext` путем переопределения `OnConfiguring` метод или извне через аргумент конструктора.

Если указаны оба, `OnConfiguring` применяется последним и перезаписывают параметры, передаваемое аргументом конструктора.

### <a name="constructor-argument"></a>Аргумент конструктора

Контекст кода с помощью конструктора:

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
> Базовый конструктор DbContext также принимает нестандартную версию `DbContextOptions`, но неуниверсальная версия не рекомендуется использовать для приложений с несколькими типами контекста.

Код приложения для инициализации из аргумента конструктора:

``` csharp
var optionsBuilder = new DbContextOptionsBuilder<BloggingContext>();
optionsBuilder.UseSqlite("Data Source=blog.db");

using (var context = new BloggingContext(optionsBuilder.Options))
{
  // do stuff
}
```

### <a name="onconfiguring"></a>OnConfiguring

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

Код приложения для инициализации `DbContext` , использующий `OnConfiguring`:

``` csharp
using (var context = new BloggingContext())
{
  // do stuff
}
```

> [!TIP]
> Этот подход не применяться для тестирования, если тесты предназначенных для всей базы данных.

### <a name="using-dbcontext-with-dependency-injection"></a>С помощью DbContext с помощью внедрения зависимостей

EF Core поддерживает использование `DbContext` с контейнер внедрения зависимостей. Ваш тип DbContext можно добавить в контейнер службы с помощью `AddDbContext<TContext>` метод.

`AddDbContext<TContext>` сделает оба ваш тип DbContext `TContext`и соответствующих `DbContextOptions<TContext>` для внедрения из контейнера служб.

См. в разделе [дополнительные чтения](#more-reading) ниже дополнительные сведения о внедрении зависимостей.

Добавление `Dbcontext` для внедрения зависимостей:

``` csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDbContext<BloggingContext>(options => options.UseSqlite("Data Source=blog.db"));
}
```

Это требует добавления [аргумент конструктора](#constructor-argument) для вашего типа DbContext, который принимает `DbContextOptions<TContext>`.

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
public class MyController
{
    private readonly BloggingContext _context;

    public MyController(BloggingContext context)
    {
      _context = context;
    }

    ...
}
```

Код приложения (с использованием ServiceProvider напрямую, менее распространено):

``` csharp
using (var context = serviceProvider.GetService<BloggingContext>())
{
  // do stuff
}

var options = serviceProvider.GetService<DbContextOptions<BloggingContext>>();
```

## <a name="more-reading"></a>Дополнительные материалы

* Чтение [начало работы в ASP.NET Core](../get-started/aspnetcore/index.md) Дополнительные сведения об использовании EF с ASP.NET Core.
* Чтение [внедрения зависимостей](https://docs.microsoft.com/aspnet/core/fundamentals/dependency-injection) Дополнительные сведения об использовании внедрения Зависимостей.
* Чтение [тестирования](testing/index.md) Дополнительные сведения.
