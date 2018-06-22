---
title: Настройка DbContext - EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: d7a22b5a-4c5b-4e3b-9897-4d7320fcd13f
ms.technology: entity-framework-core
uid: core/miscellaneous/configuring-dbcontext
ms.openlocfilehash: 6980acd53b0a74055af7a1e04b476f4625c327c9
ms.sourcegitcommit: d2434edbfa6fbcee7287e33b4915033b796e417e
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/12/2018
ms.locfileid: "29152394"
---
# <a name="configuring-a-dbcontext"></a>Настройка DbContext

В этой статье показаны основные шаблоны для настройки `DbContext` через `DbContextOptions` для подключения к базе данных с помощью конкретного поставщика EF Core и необязательного поведения.

## <a name="design-time-dbcontext-configuration"></a>Конфигурация DbContext во время разработки

EF основных компонентов во время разработки средств, таких как [миграций](xref:core/managing-schemas/migrations/index) необходимо иметь для обнаружения и создания экземпляра рабочего `DbContext` тип для сбора подробных сведений о типах сущностей и их сопоставлении схемы базы данных приложения. Этот процесс может выполняться автоматически, при условии, что можно легко создать средство `DbContext` таким образом, что он будет настроен точно так же как бы настраивается во время выполнения.

Хотя любой шаблон, предоставляющий сведения о конфигурации, необходимые для `DbContext` можно работать во время выполнения, которые требуют использования средств `DbContext` во время разработки можно работать только с ограниченным числом шаблонов. Они описаны более подробно в [создания контекста разработки](xref:core/miscellaneous/cli/dbcontext-creation) раздела.

## <a name="configuring-dbcontextoptions"></a>Настройка DbContextOptions

`DbContext` необходимо иметь установленный экземпляр `DbContextOptions` для выполнения какой-либо работы. `DbContextOptions` Экземпляр содержит конфиденциальную информацию конфигурации, такие как:

- Поставщик базы данных для использования, обычно выбирается путем вызова метода, такие как `UseSqlServer` или `UseSqlite`
- Любой необходимую строку подключения или идентификатор экземпляра базы данных, обычно передается в качестве аргумента указанного выше метода выбора поставщика
- Любого селектора уровня поставщика необязательно поведение также привязанные внутри вызова метода выбора поставщика
- Все общие селекторы поведение EF Core, обычно связанных данных после или до выбора метода поставщика

В следующем примере настраивается `DbContextOptions` использование поставщика SQL Server, подключение содержащихся в `connectionString` переменной, время ожидания команды на уровне поставщика и селектора поведение EF Core, который делает все запросы, выполняемые в `DbContext` [нет отслеживания](xref:core/querying/tracking#no-tracking-queries) по умолчанию:

``` csharp
optionsBuilder
    .UseSqlServer(connectionString, providerOptions=>providerOptions.CommandTimeout(60))
    .UseQueryTrackingBehavior(QueryTrackingBehavior.NoTracking);
```

> [!NOTE]  
> Методы выбора поставщика и другие методы выбора поведение вышеупомянутых являются методами расширения на `DbContextOptions` или классы поставщика параметров. Для доступа к эти методы расширения, необходимо иметь пространства имен (обычно `Microsoft.EntityFrameworkCore`) в области видимости и включить дополнительные зависимости в проект.

`DbContextOptions` Может быть задано для `DbContext` путем переопределения `OnConfiguring` метода или внешне через аргумент конструктора.

Если используются оба `OnConfiguring` применяется последним и перезаписывают значениями, заданными в аргументе конструктора.

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
> Базовый конструктор DbContext также принимает неуниверсальном версии `DbContextOptions`, но неуниверсальная версия не рекомендуется использовать в приложениях, где несколько типов контекста.

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
> Этот подход непригодны для тестирования, если тесты целевой базы данных.

### <a name="using-dbcontext-with-dependency-injection"></a>Внедрение зависимостей при помощи DbContext

EF Core поддерживает использование `DbContext` с контейнер внедрения зависимостей. Тип DbContext можно добавить в контейнер службы с помощью `AddDbContext<TContext>` метод.

`AddDbContext<TContext>` сделает как ваш тип DbContext `TContext`и соответствующий `DbContextOptions<TContext>` доступны для внедрения из контейнера службы.

В разделе [дополнительные чтения](#more-reading) ниже дополнительные сведения о внедрения зависимостей.

Добавление `Dbcontext` для внедрения зависимостей:

``` csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDbContext<BloggingContext>(options => options.UseSqlite("Data Source=blog.db"));
}
```

Это требует добавления [аргумента конструктора](#constructor-argument) типу DbContext, принимающий `DbContextOptions<TContext>`.

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

Код приложения (с использованием ServiceProvider напрямую, реже):

``` csharp
using (var context = serviceProvider.GetService<BloggingContext>())
{
  // do stuff
}

var options = serviceProvider.GetService<DbContextOptions<BloggingContext>>();
```

## <a name="more-reading"></a>Дополнительные чтения

* Чтение [Приступая к работе в ASP.NET Core](../get-started/aspnetcore/index.md) Дополнительные сведения об использовании EF с ASP.NET Core.
* Чтение [внедрения зависимостей](https://docs.microsoft.com/aspnet/core/fundamentals/dependency-injection) для получения дополнительных сведений об использовании DI.
* Чтение [тестирования](testing/index.md) для получения дополнительной информации.
