---
title: Настройка DbContext-EF Core
description: Стратегии настройки DbContext с помощью Entity Framework Core
author: ajcvickers
ms.date: 10/27/2016
uid: core/miscellaneous/configuring-dbcontext
ms.openlocfilehash: 3afad8d220acecbb01b15bbb855b52a895e6eb66
ms.sourcegitcommit: 0a25c03fa65ae6e0e0e3f66bac48d59eceb96a5a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/14/2020
ms.locfileid: "92062026"
---
# <a name="configuring-a-dbcontext"></a>Настройка DbContext

В этой статье показаны основные шаблоны для настройки с помощью `DbContext` `DbContextOptions` для подключения к базе данных с помощью определенного поставщика EF Core и необязательных поведений.

## <a name="design-time-dbcontext-configuration"></a>Конфигурация DbContext во время разработки

EF Core средства разработки, такие как [Миграция](xref:core/managing-schemas/migrations/index) , должны иметь возможность обнаружения и создания рабочего экземпляра `DbContext` типа для сбора сведений о типах сущностей приложения и их сопоставлении со схемой базы данных. Этот процесс может быть автоматическим, если средство может легко создать объект таким `DbContext` образом, чтобы он настроился точно так же, как он будет настроен во время выполнения.

Хотя любой шаблон, предоставляющий необходимые сведения о конфигурации, `DbContext` может работать во время выполнения, средства, требующие использования `DbContext` во время разработки, могут работать только с ограниченным количеством шаблонов. Они подробно рассматриваются в разделе [Создание контекста во время разработки](xref:core/miscellaneous/cli/dbcontext-creation) .

## <a name="configuring-dbcontextoptions"></a>Настройка DbContextOptions

`DbContext` должен иметь экземпляр `DbContextOptions` для выполнения любой работы. `DbContextOptions`Экземпляр содержит сведения о конфигурации, такие как:

- Используемый поставщик базы данных, который обычно выбирается путем вызова метода, например `UseSqlServer` или `UseSqlite` . Для этих методов расширения требуется соответствующий пакет поставщика, например `Microsoft.EntityFrameworkCore.SqlServer` или `Microsoft.EntityFrameworkCore.Sqlite` . Методы определены в `Microsoft.EntityFrameworkCore` пространстве имен.
- Все необходимые строки подключения или идентификаторы экземпляра базы данных, обычно передаваемые в качестве аргумента методу выбора поставщика, упомянутого выше.
- Любые необязательные селекторы поведения на уровне поставщика, обычно также связанные внутри вызова метода выбора поставщика
- Все общие EF Core селекторы поведения, обычно связанные в цепочке после или перед методом выбора поставщика

В следующем примере выполняется настройка `DbContextOptions` для использования поставщика SQL Server, соединения, содержащегося в `connectionString` переменной, времени ожидания команды на уровне поставщика, и селектора поведения EF Core, который по умолчанию выполняет все запросы, выполняемые в `DbContext` [неотслеживаемом](xref:core/querying/tracking#no-tracking-queries) виде:

```csharp
optionsBuilder
    .UseSqlServer(connectionString, providerOptions=>providerOptions.CommandTimeout(60))
    .UseQueryTrackingBehavior(QueryTrackingBehavior.NoTracking);
```

> [!NOTE]
> Методы выбора поставщика и другие методы выбора поведения, упомянутые выше, являются методами расширения для `DbContextOptions` классов параметров или, зависящих от поставщика. Чтобы получить доступ к этим методам расширения, вам может потребоваться пространство имен (обычно `Microsoft.EntityFrameworkCore` ) в области и включить в проект дополнительные зависимости пакетов.

`DbContextOptions`Можно передать в, `DbContext` переопределив `OnConfiguring` метод или извне с помощью аргумента конструктора.

Если используются оба параметра, `OnConfiguring` применяется последняя и могут перезаписываться параметры, передаваемые в аргумент конструктора.

### <a name="constructor-argument"></a>Аргумент конструктора

Конструктор может просто принять, `DbContextOptions` как показано ниже.

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
> Базовый конструктор DbContext также принимает не универсальную версию `DbContextOptions` , но использование неуниверсальной версии не рекомендуется для приложений с несколькими типами контекста.

Теперь приложение может передавать `DbContextOptions` при создании экземпляра контекста следующим образом:

```csharp
var optionsBuilder = new DbContextOptionsBuilder<BloggingContext>();
optionsBuilder.UseSqlite("Data Source=blog.db");

using (var context = new BloggingContext(optionsBuilder.Options))
{
  // do stuff
}
```

### <a name="onconfiguring"></a>Настройка

Можно также инициализировать в `DbContextOptions` самом контексте. Хотя этот метод можно использовать для базовой конфигурации, вам, как правило, необходимо получить определенные сведения о конфигурации извне, например строку подключения к базе данных. Это можно сделать с помощью API конфигурации или любого другого средства.

Для инициализации `DbContextOptions` в контексте Переопределите `OnConfiguring` метод и вызовите методы для предоставленного `DbContextOptionsBuilder` :

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

Приложение может просто создать экземпляр такого контекста, не передавая что-либо в свой конструктор:

```csharp
using (var context = new BloggingContext())
{
  // do stuff
}
```

> [!TIP]
> Этот подход не предназначен для тестирования, если только тесты не нацелены на полную базу данных.

### <a name="using-dbcontext-with-dependency-injection"></a>Использование DbContext с внедрением зависимостей

EF Core поддерживает использование `DbContext` с контейнером внедрения зависимостей. Тип DbContext можно добавить в контейнер службы с помощью `AddDbContext<TContext>` метода.

`AddDbContext<TContext>` сделает тип DbContext, `TContext` и соответствующий объект `DbContextOptions<TContext>` доступным для внедрения из контейнера службы.

Дополнительные сведения о внедрении [зависимостей см.](#more-reading) ниже.

Добавление в `DbContext` встраивание зависимостей:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDbContext<BloggingContext>(options => options.UseSqlite("Data Source=blog.db"));
}
```

Для этого необходимо добавить [аргумент конструктора](#constructor-argument) в тип DbContext, который принимает `DbContextOptions<TContext>` .

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

Код приложения (с использованием метода ServiceProvider напрямую, менее распространенный):

```csharp
using (var context = serviceProvider.GetService<BloggingContext>())
{
  // do stuff
}

var options = serviceProvider.GetService<DbContextOptions<BloggingContext>>();
```

## <a name="avoiding-dbcontext-threading-issues"></a>Предотвращение проблем с многопоточной обDbContextкой

Entity Framework Core не поддерживает выполнение нескольких параллельных операций на одном и том же `DbContext` экземпляре. Сюда входит параллельное выполнение асинхронных запросов и любое явное одновременное использование из нескольких потоков. Таким образом, всегда `await` асинхронные вызовы выполняются немедленно или используют отдельные `DbContext` экземпляры для операций, выполняемых параллельно.

Когда EF Core обнаруживает `DbContext` одновременную попытку использования экземпляра, вы увидите `InvalidOperationException` сообщение с таким образом:

> Вторая операция запущена в этом контексте до завершения предыдущей операции. Обычно это вызвано различными потоками, использующими один и тот же экземпляр DbContext, однако не гарантируется, что члены экземпляров будут потокобезопасными.

Если одновременный доступ не обнаруживается, это может привести к неопределенному поведению, сбоям приложений и повреждению данных.

Существуют распространенные ошибки, которые могут случайно вызвать параллельный доступ к одному и тому же `DbContext` экземпляру:

### <a name="forgetting-to-await-the-completion-of-an-asynchronous-operation-before-starting-any-other-operation-on-the-same-dbcontext"></a>Невозможность ожидания завершения асинхронной операции перед запуском любой другой операции с тем же DbContext

Асинхронные методы позволяют EF Core инициировать операции, обращающиеся к базе данных без блокировки. Но если вызывающий объект не ожидает завершения одного из этих методов и переходит к выполнению других операций в `DbContext` , состояние `DbContext` может быть (и, скорее всего, будет) повреждено.

Всегда ожидайте EF Core асинхронных методов немедленно.

### <a name="implicitly-sharing-dbcontext-instances-across-multiple-threads-via-dependency-injection"></a>Неявное совместное использование экземпляров DbContext в нескольких потоках с помощью внедрения зависимостей

[`AddDbContext`](/dotnet/api/microsoft.extensions.dependencyinjection.entityframeworkservicecollectionextensions.adddbcontext)Метод расширения регистрирует `DbContext` типы с [жизненным циклом в области действия](/aspnet/core/fundamentals/dependency-injection#service-lifetimes) по умолчанию.

Это может быть защищено от проблем с одновременным доступом в большинстве ASP.NET Core приложений, поскольку существует только один поток, выполняющий каждый запрос клиента в определенный момент времени, и поскольку каждый запрос получает отдельную область внедрения зависимостей (и, следовательно, отдельный `DbContext` экземпляр). Для модели размещения сервера Блазор используется один логический запрос для обслуживания пользовательского канала Блазор, поэтому при использовании области введения по умолчанию для каждого пользовательского канала доступен только один экземпляр DbContext с заданной областью.

Любой код, явно выполняющий несколько потоков в параллельном режиме, должен обеспечить `DbContext` одновременное обращение к экземплярам.

С помощью внедрения зависимостей можно либо зарегистрировать контекст в области, либо создать области (с помощью `IServiceScopeFactory` ) для каждого потока, либо зарегистрировать `DbContext` как временную (с помощью перегрузки, `AddDbContext` которая принимает `ServiceLifetime` параметр).

## <a name="more-reading"></a>Дополнительные материалы

- Дополнительные сведения об использовании DI см. в статье [внедрение зависимостей](/aspnet/core/fundamentals/dependency-injection) .
- Дополнительные сведения см. в статье [тестирование](xref:core/miscellaneous/testing/index) .
