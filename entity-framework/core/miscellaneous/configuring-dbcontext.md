---
title: Настройка DbContext-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: d7a22b5a-4c5b-4e3b-9897-4d7320fcd13f
uid: core/miscellaneous/configuring-dbcontext
ms.openlocfilehash: e04c1b65df96819f3493e0ed34ccf26609511f6a
ms.sourcegitcommit: 37d0e0fd1703467918665a64837dc54ad2ec7484
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/16/2019
ms.locfileid: "72445908"
---
# <a name="configuring-a-dbcontext"></a>Настройка DbContext

В этой статье показаны основные шаблоны для настройки `DbContext` через `DbContextOptions` для подключения к базе данных с помощью определенного поставщика EF Core и необязательных поведений.

## <a name="design-time-dbcontext-configuration"></a>Конфигурация DbContext во время разработки

EF Core средства разработки, такие как [Миграция](xref:core/managing-schemas/migrations/index) , должны иметь возможность обнаружения и создания рабочего экземпляра типа `DbContext` для сбора сведений о типах сущностей приложения и их сопоставлении со схемой базы данных. Этот процесс может быть автоматическим, если средство может легко создать `DbContext` таким образом, что оно будет настроено аналогично настройке во время выполнения.

Хотя любой шаблон, предоставляющий необходимые сведения о конфигурации для `DbContext`, может работать во время выполнения, средства, требующие использования `DbContext` во время разработки, могут работать только с ограниченным количеством шаблонов. Они подробно рассматриваются в разделе [Создание контекста во время разработки](xref:core/miscellaneous/cli/dbcontext-creation) .

## <a name="configuring-dbcontextoptions"></a>Настройка DbContextOptions

`DbContext` должен иметь экземпляр `DbContextOptions` для выполнения любой работы. Экземпляр `DbContextOptions` содержит сведения о конфигурации, такие как:

- Используемый поставщик базы данных, который обычно выбирается путем вызова метода, например `UseSqlServer` или `UseSqlite`. Для этих методов расширения требуется соответствующий пакет поставщика, например `Microsoft.EntityFrameworkCore.SqlServer` или `Microsoft.EntityFrameworkCore.Sqlite`. Методы определены в пространстве имен `Microsoft.EntityFrameworkCore`.
- Все необходимые строки подключения или идентификаторы экземпляра базы данных, обычно передаваемые в качестве аргумента методу выбора поставщика, упомянутого выше.
- Любые необязательные селекторы поведения на уровне поставщика, обычно также связанные внутри вызова метода выбора поставщика
- Все общие EF Core селекторы поведения, обычно связанные в цепочке после или перед методом выбора поставщика

В следующем примере `DbContextOptions` настраивается для использования поставщика SQL Server, соединения, содержащегося в переменной `connectionString`, времени ожидания команды на уровне поставщика, и селектора поведения EF Core, который делает все запросы, выполняемые в `DbContext` [без отслеживания](xref:core/querying/tracking#no-tracking-queries) . по умолчанию:

``` csharp
optionsBuilder
    .UseSqlServer(connectionString, providerOptions=>providerOptions.CommandTimeout(60))
    .UseQueryTrackingBehavior(QueryTrackingBehavior.NoTracking);
```

> [!NOTE]  
> Методы выбора поставщика и другие методы выбора поведения, упомянутые выше, являются методами расширения для `DbContextOptions` или классов параметров, зависящих от поставщика. Чтобы получить доступ к этим методам расширения, необходимо иметь пространство имен (обычно `Microsoft.EntityFrameworkCore`) в области и включить дополнительные зависимости пакетов в проект.

@No__t-0 может быть предоставлено `DbContext` путем переопределения метода `OnConfiguring` или извне с помощью аргумента конструктора.

При использовании обоих параметров `OnConfiguring` применяется последним и может перезаписывать параметры, передаваемые в аргумент конструктора.

### <a name="constructor-argument"></a>Аргумент конструктора

Конструктор может просто принять `DbContextOptions` следующим образом:

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
> Базовый конструктор DbContext также принимает неуниверсальную версию `DbContextOptions`, но использование неуниверсальной версии не рекомендуется для приложений с несколькими типами контекста.

Теперь приложение может передать `DbContextOptions` при создании экземпляра контекста следующим образом:

``` csharp
var optionsBuilder = new DbContextOptionsBuilder<BloggingContext>();
optionsBuilder.UseSqlite("Data Source=blog.db");

using (var context = new BloggingContext(optionsBuilder.Options))
{
  // do stuff
}
```

### <a name="onconfiguring"></a>Настройка

Можно также инициализировать `DbContextOptions` в самом контексте. Хотя этот метод можно использовать для базовой конфигурации, вам, как правило, необходимо получить определенные сведения о конфигурации извне, например строку подключения к базе данных. Это можно сделать с помощью API конфигурации или любого другого средства.

Чтобы инициализировать `DbContextOptions` в контексте, переопределите метод `OnConfiguring` и вызовите методы для предоставленного `DbContextOptionsBuilder`:

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

Приложение может просто создать экземпляр такого контекста, не передавая что-либо в свой конструктор:

``` csharp
using (var context = new BloggingContext())
{
  // do stuff
}
```

> [!TIP]
> Этот подход не предназначен для тестирования, если только тесты не нацелены на полную базу данных.

### <a name="using-dbcontext-with-dependency-injection"></a>Использование DbContext с внедрением зависимостей

EF Core поддерживает использование `DbContext` с контейнером внедрения зависимостей. Тип DbContext можно добавить в контейнер службы с помощью метода `AddDbContext<TContext>`.

`AddDbContext<TContext>` сделает тип DbContext, `TContext`, а соответствующий `DbContextOptions<TContext>` доступен для внедрения из контейнера службы.

Дополнительные сведения о внедрении [зависимостей см.](#more-reading) ниже.

Добавление `DbContext` к внедрению зависимостей:

``` csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDbContext<BloggingContext>(options => options.UseSqlite("Data Source=blog.db"));
}
```

Для этого необходимо добавить [аргумент конструктора](#constructor-argument) к типу DbContext, который принимает `DbContextOptions<TContext>`.

Код контекста:

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

Код приложения (с использованием метода ServiceProvider напрямую, менее распространенный):

``` csharp
using (var context = serviceProvider.GetService<BloggingContext>())
{
  // do stuff
}

var options = serviceProvider.GetService<DbContextOptions<BloggingContext>>();
```
## <a name="avoiding-dbcontext-threading-issues"></a>Предотвращение проблем с многопоточной обDbContextкой

Entity Framework Core не поддерживает выполнение нескольких параллельных операций на одном экземпляре `DbContext`. Сюда входит параллельное выполнение асинхронных запросов и любое явное одновременное использование из нескольких потоков. Таким образом, всегда `await` асинхронные вызовы немедленно или используют отдельные экземпляры `DbContext` для операций, выполняемых параллельно.

Когда EF Core обнаруживает попытку одновременного использования экземпляра `DbContext`, вы увидите `InvalidOperationException` с сообщением следующего вида: 

> Вторая операция запущена в этом контексте до завершения предыдущей операции. Обычно это вызвано различными потоками, использующими один и тот же экземпляр DbContext, однако не гарантируется, что члены экземпляров будут потокобезопасными.

Если одновременный доступ не обнаруживается, это может привести к неопределенному поведению, сбоям приложений и повреждению данных.

Существуют распространенные ошибки, которые могут случайно вызвать одновременный доступ к одному экземпляру `DbContext`:

### <a name="forgetting-to-await-the-completion-of-an-asynchronous-operation-before-starting-any-other-operation-on-the-same-dbcontext"></a>Невозможность ожидания завершения асинхронной операции перед запуском любой другой операции с тем же DbContext

Асинхронные методы позволяют EF Core инициировать операции, обращающиеся к базе данных без блокировки. Но если вызывающий объект не ожидает завершения одного из этих методов и переходит к выполнению других операций с `DbContext`, состояние `DbContext` может быть, (и очень вероятно, будет) повреждено. 

Всегда ожидайте EF Core асинхронных методов немедленно.  

### <a name="implicitly-sharing-dbcontext-instances-across-multiple-threads-via-dependency-injection"></a>Неявное совместное использование экземпляров DbContext в нескольких потоках с помощью внедрения зависимостей

Метод расширения [`AddDbContext`](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.entityframeworkservicecollectionextensions.adddbcontext) регистрирует типы `DbContext` с [временем существования в области](https://docs.microsoft.com/aspnet/core/fundamentals/dependency-injection#service-lifetimes) по умолчанию. 

Это может быть защищено от проблем с одновременным доступом в ASP.NET Core приложениях, так как существует только один поток, выполняющий каждый запрос клиента в определенный момент времени, и поскольку каждый запрос получает отдельную область внедрения зависимостей (и, следовательно, отдельный экземпляр `DbContext`).

Однако любой код, явно выполняющий несколько потоков в параллельном режиме, должен обеспечить одновременное обращение к экземплярам `DbContext`.

С помощью внедрения зависимостей можно либо зарегистрировать контекст в области видимости, либо создать области (с помощью `IServiceScopeFactory`) для каждого потока или зарегистрировать `DbContext` как временный (с помощью перегрузки `AddDbContext`, которая принимает параметр `ServiceLifetime`).

## <a name="more-reading"></a>Дополнительные материалы

* Дополнительные сведения об использовании DI см. в статье [внедрение зависимостей](https://docs.microsoft.com/aspnet/core/fundamentals/dependency-injection) .
* Дополнительные сведения см. в статье [тестирование](testing/index.md) .
