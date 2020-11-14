---
title: Настройка DbContext в EF Core
description: Принципы настройки экземпляров DbContext с помощью Entity Framework Core
author: ajcvickers
ms.date: 10/27/2016
uid: core/dbcontext-configuration/index
ms.openlocfilehash: 179cb1e4ff7a433c13677ec3f1e457de96004489
ms.sourcegitcommit: f3512e3a98e685a3ba409c1d0157ce85cc390cf4
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2020
ms.locfileid: "94431148"
---
# <a name="configuring-a-dbcontext"></a>Настройка DbContext

В этой статье показаны основные шаблоны настройки `DbContext` с помощью `DbContextOptions` для подключения к базе данных с использованием определенного поставщика EF Core и необязательных расширений функциональности.

## <a name="design-time-dbcontext-configuration"></a>Настройка DbContext во время разработки

Средства разработки EF Core, такие как средства [миграции](xref:core/managing-schemas/migrations/index), должны иметь возможность обнаруживать и создавать рабочие экземпляры типа `DbContext` для сбора сведений о типах сущностей приложения и их сопоставления со схемой базы данных. Этот процесс можно автоматизировать, так как средство может без труда создать `DbContext` с теми же настройками, которые заданы во время выполнения.

Хотя любой шаблон, предоставляющий `DbContext` необходимые сведения о конфигурации, может работать во время выполнения, средства, требующие использования `DbContext` во время разработки, можно использовать только с ограниченным количеством шаблонов. Дополнительные сведения об этом см. в статье [Создание DbContext во время разработки](xref:core/cli/dbcontext-creation).

## <a name="configuring-dbcontextoptions"></a>Настройка DbContextOptions

`DbContext` для выполнения любых операций требуется экземпляр `DbContextOptions`. Экземпляр `DbContextOptions` содержит сведения о конфигурации, например:

- Сведения об используемом поставщике базы данных. Как правило, для выбора поставщика вызывается метод, например `UseSqlServer` или `UseSqlite`. Для этих методов расширения требуется соответствующий пакет поставщика, например `Microsoft.EntityFrameworkCore.SqlServer` или `Microsoft.EntityFrameworkCore.Sqlite`. Эти методы определены в пространстве имен `Microsoft.EntityFrameworkCore`.
- Необходимая строка подключения или идентификатор экземпляра базы данных, которые обычно передаются в виде аргумента методу выбора поставщика, упомянутого выше.
- Сведения обо всех необязательных селекторах поведения на уровне поставщика, которые обычно также объединены в цепочку внутри вызова метода выбора поставщика.
- Сведения обо всех общих селекторах поведения EF Core, которые обычно объединяются в цепочку после метода выбора поставщика или перед ним.

В приведенном ниже примере для `DbContextOptions` заданы такие настройки: поставщик SQL Server; подключение, переданное в переменной `connectionString`; время ожидания для команды на уровне поставщика и селектор поведения EF Core, который делает все запросы, выполняемые в `DbContext`, [неотслеживаемыми](xref:core/querying/tracking#no-tracking-queries) по умолчанию.

```csharp
optionsBuilder
    .UseSqlServer(connectionString, providerOptions=>providerOptions.CommandTimeout(60))
    .UseQueryTrackingBehavior(QueryTrackingBehavior.NoTracking);
```

> [!NOTE]
> Методы селектора поставщика и другие методы селектора поведения, упомянутые выше, являются методами расширения в `DbContextOptions` или классах параметров определенного поставщика. Чтобы получить доступ к этим методам расширения, возможно, вам потребуется добавить пространство имен (обычно `Microsoft.EntityFrameworkCore`) в область и включить в проект дополнительные зависимости пакетов.

`DbContextOptions` можно предоставить `DbContext`, переопределив метод `OnConfiguring`, или предоставить извне с помощью аргумента конструктора.

Если используются оба способа, `OnConfiguring` применяется последним и может перезаписывать параметры, передаваемые в аргумент конструктора.

### <a name="constructor-argument"></a>Аргумент конструктора

Конструктор может просто принять `DbContextOptions` следующим образом:

```csharp
public class BloggingContext : DbContext
{
    public BloggingContext(DbContextOptions<BloggingContext> options)
        : base(options)
    { }

    public DbSet<Blog> Blogs { get; set; }
}
```

> [!TIP]
> Базовый конструктор DbContext также принимает неуниверсальную версию `DbContextOptions`. Но не следует использовать такую версию для приложений с несколькими типами контекста.

Теперь приложение может передать `DbContextOptions` при создании экземпляра контекста следующим образом:

```csharp
var optionsBuilder = new DbContextOptionsBuilder<BloggingContext>();
optionsBuilder.UseSqlite("Data Source=blog.db");

using (var context = new BloggingContext(optionsBuilder.Options))
{
  // do stuff
}
```

### <a name="onconfiguring"></a>OnConfiguring

Кроме того, вы можете инициализировать `DbContextOptions` в самом контексте. Этот способ можно применить для базовой конфигурации но, как правило, все равно потребуется получить определенные сведения о конфигурации извне, например строку подключения к базе данных. Это можно сделать с помощью API конфигурации или любого другого средства.

Чтобы инициализировать `DbContextOptions` в контексте, переопределите метод `OnConfiguring` и вызовите методы для предоставленного `DbContextOptionsBuilder`:

```csharp
public class BloggingContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }

    protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    {
        optionsBuilder.UseSqlite("Data Source=blog.db");
    }
}
```

Приложение может без труда создать экземпляр такого контекста, не передавая какие-либо данные в свой конструктор:

```csharp
using (var context = new BloggingContext())
{
  // do stuff
}
```

> [!TIP]
> Этот подход не предназначен для тестирования, если только тесты не ориентированы на проверку работы всей базы данных.

### <a name="using-dbcontext-with-dependency-injection"></a>Использование DbContext с внедрением зависимостей

EF Core поддерживает использование `DbContext` с контейнером внедрения зависимостей. Тип DbContext можно добавить в контейнер службы с помощью метода `AddDbContext<TContext>`.

`AddDbContext<TContext>` сделает тип DbContext (`TContext`) и соответствующий тип `DbContextOptions<TContext>` доступными для внедрения из контейнера службы.

Дополнительные сведения о внедрении зависимостей см. в статьях, ссылки на которые приведены в разделе [Дополнительные материалы](#more-reading).

Добавление `DbContext` в код внедрения зависимостей:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDbContext<BloggingContext>(options => options.UseSqlite("Data Source=blog.db"));
}
```

Вам потребуется добавить [аргумент конструктора](#constructor-argument) в тип DbContext, который принимает `DbContextOptions<TContext>`.

Код контекста:

```csharp
public class BloggingContext : DbContext
{
    public BloggingContext(DbContextOptions<BloggingContext> options)
      :base(options)
    { }

    public DbSet<Blog> Blogs { get; set; }
}
```

Код приложения (в ASP.NET Core):

```csharp
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

Код приложения с использованием ServiceProvider напрямую (менее распространенный способ):

```csharp
using (var context = serviceProvider.GetService<BloggingContext>())
{
  // do stuff
}

var options = serviceProvider.GetService<DbContextOptions<BloggingContext>>();
```

## <a name="avoiding-dbcontext-threading-issues"></a>Предотвращение проблем с потоками DbContext

Entity Framework Core не поддерживает выполнение нескольких параллельных операций в одном экземпляре `DbContext`, включая параллельное выполнение асинхронных запросов и любое явное использование экземпляра из нескольких потоков одновременно. Поэтому обязательно сразу же применяйте `await` к асинхронным вызовам или используйте отдельные экземпляры `DbContext`, выполняемые параллельно.

Когда EF Core обнаруживает попытку одновременного использования экземпляра `DbContext`, отобразится `InvalidOperationException` с примерно таким сообщением:

> В этом контексте была начата вторая операция до завершения предыдущей операции. Как правило, так происходит, когда несколько потоков используют один и тот же экземпляр DbContext. Но потокобезопасность членов экземпляров не гарантируется.

Если одновременный доступ не обнаруживается, это может привести к неопределенному поведению, аварийному завершению работы приложения и повреждению данных.

Есть распространенные ошибки, из-за которых может случайно возникнуть одновременный доступ к одному и тому же экземпляру `DbContext`.

### <a name="forgetting-to-await-the-completion-of-an-asynchronous-operation-before-starting-any-other-operation-on-the-same-dbcontext"></a>Запуск другой асинхронной операции с одним и тем же экземпляром до завершения предыдущей операции

Асинхронные методы позволяют EF Core инициировать операции, обращающиеся к базе данных, без блокировки. Но если вызывающий объект не ожидает завершения одного из этих методов и переходит к выполнению других операций с `DbContext`, структура `DbContext` может быть (и, скорее всего, будет) повреждена.

Обязательно сразу же применяйте await к асинхронным методам EF Core.

### <a name="implicitly-sharing-dbcontext-instances-across-multiple-threads-via-dependency-injection"></a>Неявное совместное использование экземпляров DbContext в нескольких потоках путем внедрения зависимостей

Метод расширения [`AddDbContext`](/dotnet/api/microsoft.extensions.dependencyinjection.entityframeworkservicecollectionextensions.adddbcontext) по умолчанию регистрирует типы `DbContext` с заданной [областью времени существования](/aspnet/core/fundamentals/dependency-injection#service-lifetimes).

Это обеспечивает защиту от проблем с одновременным доступом в большинстве приложений ASP.NET Core, так как есть только один поток, в котором каждый запрос клиента выполняется в определенный момент времени, и каждый такой запрос получает отдельную область внедрения зависимостей (и, следовательно, отдельный экземпляр `DbContext`). В модели размещения Blazor Server используется один логический запрос для обслуживания пользовательского канала Blazor. Поэтому при использовании области внедрения по умолчанию для каждого пользовательского канала доступен только один экземпляр DbContext с заданной областью.

Если код явно выполняет несколько потоков в параллельном режиме, нужно исключить возможность одновременного доступа к экземплярам `DbContext`.

При использовании внедрения зависимостей это можно реализовать путем регистрации контекста как ограниченного областью, создания области (с помощью `IServiceScopeFactory`) для каждого потока или регистрации экземпляра `DbContext` как временного (с помощью перегрузки `AddDbContext`, при которой принимается параметр `ServiceLifetime`).

## <a name="more-reading"></a>Дополнительные материалы

- Дополнительные сведения о внедрении зависимостей см. [здесь](/aspnet/core/fundamentals/dependency-injection).
- Подробную информацию см. в статье [Тестирование кода, использующего EF Core](xref:core/testing/index).
