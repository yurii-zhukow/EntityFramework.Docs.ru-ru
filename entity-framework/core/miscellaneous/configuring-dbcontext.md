---
title: Настройка DbContext-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: d7a22b5a-4c5b-4e3b-9897-4d7320fcd13f
uid: core/miscellaneous/configuring-dbcontext
ms.openlocfilehash: 734acad86e364abbfd1522fe79d4a847b1acfb52
ms.sourcegitcommit: cbaa6cc89bd71d5e0bcc891e55743f0e8ea3393b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/20/2019
ms.locfileid: "71149029"
---
# <a name="configuring-a-dbcontext"></a>Настройка DbContext

В этой статье показаны основные шаблоны для настройки `DbContext` с `DbContextOptions` помощью для подключения к базе данных с помощью определенного поставщика EF Core и необязательных поведений.

## <a name="design-time-dbcontext-configuration"></a>Конфигурация DbContext во время разработки

EF Core средства разработки, такие как [Миграция](xref:core/managing-schemas/migrations/index) , должны иметь возможность обнаружения и создания рабочего экземпляра `DbContext` типа для сбора сведений о типах сущностей приложения и их сопоставлении со схемой базы данных. Этот процесс может быть автоматическим, если средство может легко создать объект `DbContext` таким образом, чтобы он настроился точно так же, как он будет настроен во время выполнения.

Хотя любой шаблон, предоставляющий необходимые сведения о `DbContext` конфигурации, может работать во время выполнения, средства, требующие `DbContext` использования во время разработки, могут работать только с ограниченным количеством шаблонов. Они подробно рассматриваются в разделе [Создание контекста во время разработки](xref:core/miscellaneous/cli/dbcontext-creation) .

## <a name="configuring-dbcontextoptions"></a>Настройка DbContextOptions

`DbContext`должен иметь экземпляр `DbContextOptions` для выполнения любой работы. `DbContextOptions` Экземпляр содержит сведения о конфигурации, такие как:

- Используемый поставщик базы данных, который обычно выбирается путем вызова метода, например `UseSqlServer` или. `UseSqlite` Для этих методов расширения требуется соответствующий пакет поставщика, например `Microsoft.EntityFrameworkCore.SqlServer` или. `Microsoft.EntityFrameworkCore.Sqlite` Методы определены в `Microsoft.EntityFrameworkCore` пространстве имен.
- Все необходимые строки подключения или идентификаторы экземпляра базы данных, обычно передаваемые в качестве аргумента методу выбора поставщика, упомянутого выше.
- Любые необязательные селекторы поведения на уровне поставщика, обычно также связанные внутри вызова метода выбора поставщика
- Все общие EF Core селекторы поведения, обычно связанные в цепочке после или перед методом выбора поставщика

В следующем примере выполняется настройка `DbContextOptions` для использования поставщика SQL Server, соединения, содержащегося `connectionString` в переменной, времени ожидания команды на уровне поставщика, а также селектора поведения EF Core, который делает все запросы, `DbContext` выполняемые в [без отслеживания](xref:core/querying/tracking#no-tracking-queries) по умолчанию:

``` csharp
optionsBuilder
    .UseSqlServer(connectionString, providerOptions=>providerOptions.CommandTimeout(60))
    .UseQueryTrackingBehavior(QueryTrackingBehavior.NoTracking);
```

> [!NOTE]  
> Методы выбора поставщика и другие методы выбора поведения, упомянутые выше, являются `DbContextOptions` методами расширения для классов параметров или, зависящих от поставщика. Чтобы получить доступ к этим методам расширения, вам может потребоваться пространство имен (обычно `Microsoft.EntityFrameworkCore`) в области и включить в проект дополнительные зависимости пакетов.

Можно передать в, `DbContext` переопределив `OnConfiguring` метод или извне с помощью аргумента конструктора. `DbContextOptions`

Если используются оба параметра, `OnConfiguring` применяется последняя и могут перезаписываться параметры, передаваемые в аргумент конструктора.

### <a name="constructor-argument"></a>Аргумент конструктора

Контекстный код с конструктором:

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
> Базовый конструктор DbContext также принимает не универсальную версию `DbContextOptions`, но использование неуниверсальной версии не рекомендуется для приложений с несколькими типами контекста.

Код приложения для инициализации из аргумента конструктора:

``` csharp
var optionsBuilder = new DbContextOptionsBuilder<BloggingContext>();
optionsBuilder.UseSqlite("Data Source=blog.db");

using (var context = new BloggingContext(optionsBuilder.Options))
{
  // do stuff
}
```

### <a name="onconfiguring"></a>Настройка

Контекстный код `OnConfiguring`:

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

Код приложения для инициализации, `DbContext` в котором `OnConfiguring`используется:

``` csharp
using (var context = new BloggingContext())
{
  // do stuff
}
```

> [!TIP]
> Этот подход не предназначен для тестирования, если только тесты не нацелены на полную базу данных.

### <a name="using-dbcontext-with-dependency-injection"></a>Использование DbContext с внедрением зависимостей

EF Core поддерживает использование `DbContext` с контейнером внедрения зависимостей. Тип DbContext можно добавить в контейнер службы с помощью `AddDbContext<TContext>` метода.

`AddDbContext<TContext>`сделает тип DbContext, `TContext`и соответствующий `DbContextOptions<TContext>` объект доступным для внедрения из контейнера службы.

Дополнительные сведения о внедрении [зависимостей см.](#more-reading) ниже.

Добавление в `DbContext` встраивание зависимостей:

``` csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddDbContext<BloggingContext>(options => options.UseSqlite("Data Source=blog.db"));
}
```

Для этого необходимо добавить [аргумент конструктора](#constructor-argument) в тип DbContext, который принимает `DbContextOptions<TContext>`.

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

Entity Framework Core не поддерживает выполнение нескольких параллельных операций на одном и том `DbContext` же экземпляре. Сюда входит параллельное выполнение асинхронных запросов и любое явное одновременное использование из нескольких потоков. Таким образом, `await` всегда асинхронные вызовы выполняются немедленно или `DbContext` используют отдельные экземпляры для операций, выполняемых параллельно.

Когда EF Core обнаруживает одновременную попытку `DbContext` использования экземпляра, вы `InvalidOperationException` увидите сообщение с таким образом: 

> Вторая операция запущена в этом контексте до завершения предыдущей операции. Обычно это вызвано различными потоками, использующими один и тот же экземпляр DbContext, однако не гарантируется, что члены экземпляров будут потокобезопасными.

Если одновременный доступ не обнаруживается, это может привести к неопределенному поведению, сбоям приложений и повреждению данных.

Существуют распространенные ошибки, которые могут случайно вызвать параллельный доступ к одному и `DbContext` тому же экземпляру:

### <a name="forgetting-to-await-the-completion-of-an-asynchronous-operation-before-starting-any-other-operation-on-the-same-dbcontext"></a>Невозможность ожидания завершения асинхронной операции перед запуском любой другой операции с тем же DbContext

Асинхронные методы позволяют EF Core инициировать операции, обращающиеся к базе данных без блокировки. Но если вызывающий объект не ожидает завершения одного из этих методов и переходит к выполнению других операций `DbContext`в, состояние `DbContext` может быть (и, скорее всего, будет) повреждено. 

Всегда ожидайте EF Core асинхронных методов немедленно.  

### <a name="implicitly-sharing-dbcontext-instances-across-multiple-threads-via-dependency-injection"></a>Неявное совместное использование экземпляров DbContext в нескольких потоках с помощью внедрения зависимостей

Метод расширения регистрирует `DbContext` типы с [жизненным циклом в области действия](https://docs.microsoft.com/aspnet/core/fundamentals/dependency-injection#service-lifetimes) по умолчанию. [`AddDbContext`](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.entityframeworkservicecollectionextensions.adddbcontext) 

Это может быть связано с проблемами параллельного доступа в ASP.NET Core приложениях, поскольку существует только один поток, выполняющий каждый клиентский запрос в определенный момент времени, и поскольку каждый запрос получает отдельную область внедрения зависимостей ( `DbContext` и, следовательно, отдельный элемент экземпляр).

Однако любой код, который явным образом выполняет несколько потоков в параллельном режиме `DbContext` , должен обеспечить одновременное обращение к экземплярам.

С помощью внедрения зависимостей это можно сделать, зарегистрировав контекст в области видимости и создавая области (с помощью `IServiceScopeFactory`) для каждого потока или `DbContext` зарегистрировав как временную `AddDbContext` (с помощью перегрузки, которая принимает `ServiceLifetime` параметр).

## <a name="more-reading"></a>Дополнительные материалы

* Дополнительные сведения об использовании DI см. в статье [внедрение зависимостей](https://docs.microsoft.com/aspnet/core/fundamentals/dependency-injection) .
* Дополнительные сведения см. в статье [тестирование](testing/index.md) .
