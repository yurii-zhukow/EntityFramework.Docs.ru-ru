---
title: Время существования, настройка и инициализация DbContext (EF Core)
description: Шаблоны для создания экземпляров DbContext с внедрением зависимостей или без него, а также управление такими экземплярами
author: ajcvickers
ms.date: 11/07/2020
uid: core/dbcontext-configuration/index
ms.openlocfilehash: 10ed474df2c4c52e61083d9d671909be02cd8cef
ms.sourcegitcommit: 032a1767d7a6e42052a005f660b80372c6521e7e
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/12/2021
ms.locfileid: "98129035"
---
# <a name="dbcontext-lifetime-configuration-and-initialization"></a>Время существования, настройка и инициализация DbContext

В этой статье описаны основные шаблоны для инициализации и настройки экземпляра <xref:Microsoft.EntityFrameworkCore.DbContext>.

## <a name="the-dbcontext-lifetime"></a>Время существования DbContext

Время существования `DbContext` начинается, когда создается экземпляр, и заканчивается, когда экземпляр [удаляется](/dotnet/standard/garbage-collection/unmanaged). Экземпляр `DbContext` предназначен для выполнения _одной_ [единицы работы](https://www.martinfowler.com/eaaCatalog/unitOfWork.html). Это означает, что время существования экземпляра `DbContext` обычно невелико.

> [!TIP]
> Цитируя Мартина Фаулера (по ссылке выше): "С помощью единицы работы отслеживаются все операции во время бизнес-транзакции, которые могут повлиять на базу данных. Когда вы закончите работу, такая единица определяет, что нужно сделать, чтобы изменить базу данных в результате ваших действий".

Обычная единица работы при использовании Entity Framework Core (EF Core) включает следующее:

- Создание экземпляра `DbContext`.
- Отслеживание экземпляров сущности по контексту. Сущности отслеживаются, когда они:
  - [возвращаются запросом;](xref:core/querying/tracking)
  - [добавляются или присоединяются к контексту.](xref:core/saving/disconnected-entities)
- При необходимости отслеживаемые сущности изменяются для реализации бизнес-правила.
- Вызывается <xref:Microsoft.EntityFrameworkCore.DbContext.SaveChanges%2A> или <xref:Microsoft.EntityFrameworkCore.DbContext.SaveChangesAsync%2A>. EF Core обнаруживает внесенные изменения и записывает их в базу данных.
- Экземпляр `DbContext` удаляется.

> [!IMPORTANT]
>
> - Очень важно также удалить <xref:Microsoft.EntityFrameworkCore.DbContext> после использования. Это гарантирует, что все неуправляемые ресурсы будут освобождены и что регистрация всех событий или других перехватчиков будет отменена. Так можно предотвратить утечки памяти в тех случаях, если на экземпляр сохраняются ссылки.
> - [DbContext **не является потокобезопасным**](#avoiding-dbcontext-threading-issues). Не передавайте контексты в другие потоки. Выполняйте оператор [await](/dotnet/csharp/language-reference/operators/await) для всех вызовов синхронизации, прежде чем продолжать использование экземпляра контекста.
> - Исключение <xref:System.InvalidOperationException>, генероируемое кодом EF Core, может перевести контекст в невосстанавливаемое состояние. Такие исключения указывают на ошибку в программе и не допускают восстановление.

## <a name="dbcontext-in-dependency-injection-for-aspnet-core"></a>DbContext во внедрении зависимостей для ASP.NET Core

Во многих веб-приложениях каждый HTTP-запрос соответствует одной единице работы. По этой причине в веб-приложениях время существования контекста по умолчанию привязывается ко времени существования запроса.

Приложения ASP.NET Core [настраиваются с использованием внедрения зависимостей](/aspnet/core/fundamentals/startup). Вы можете добавить EF Core в такую конфигурацию с помощью <xref:Microsoft.Extensions.DependencyInjection.EntityFrameworkServiceCollectionExtensions.AddDbContext%2A> в методе [`ConfigureServices`](/aspnet/core/fundamentals/startup#the-configureservices-method) файла `Startup.cs`. Пример:

<!--
        public void ConfigureServices(IServiceCollection services)
        {
            services.AddControllers();

            services.AddDbContext<ApplicationDbContext>(
                options => options.UseSqlServer("name=ConnectionStrings:DefaultConnection"));
        }
-->
[!code-csharp[ConfigureServices](../../../samples/core/Miscellaneous/ConfiguringDbContext/WebApp/Startup.cs?name=ConfigureServices)]

В этом примере регистрируется подкласс `DbContext` с именем `ApplicationDbContext` в качестве службы с заданной областью в поставщике службы приложений ASP.NET Core (т. е. в контейнере внедрения зависимостей). Контекст при этом настраивается для использования поставщика базы данных SQL Server и считывания строки подключения из конфигурации ASP.NET Core. Обычно не имеет значения, _где_ в `ConfigureServices` выполняется вызов к `AddDbContext`.

Класс `ApplicationDbContext` должен предоставить доступ к общедоступному конструктору с помощью параметра `DbContextOptions<ApplicationDbContext>`. Именно так передается конфигурация контекста из `AddDbContext` в `DbContext`. Пример:

<!--
    public class ApplicationDbContext : DbContext
    {
        public ApplicationDbContext(DbContextOptions<ApplicationDbContext> options)
            : base(options)
        {
        }
    }
-->
[!code-csharp[ApplicationDbContext](../../../samples/core/Miscellaneous/ConfiguringDbContext/WebApp/ApplicationDbContext.cs?name=ApplicationDbContext)]

Затем `ApplicationDbContext` можно использовать в контроллерах ASP.NET Core или других службах посредством внедрения конструктора. Пример:

<!--
    public class MyController
    {
        private readonly ApplicationDbContext _context;

        public MyController(ApplicationDbContext context)
        {
            _context = context;
        }
    }
-->
[!code-csharp[MyController](../../../samples/core/Miscellaneous/ConfiguringDbContext/WebApp/Controllers/MyController.cs?name=MyController)]

Результатом будет экземпляр `ApplicationDbContext`, созданный для каждого запроса и переданный контроллеру для выполнения единицы работы до удаления при завершении запроса.

Дополнительные сведения о параметрах конфигурации см. далее в этой статье. Кроме того, дополнительные сведения о конфигурации и внедрении зависимостей можно найти в статьях [Запуск приложения в ASP.NET Core](/aspnet/core/fundamentals/startup) и [Внедрение зависимостей в ASP.NET Core](/aspnet/core/fundamentals/dependency-injection).

<!-- See also [Using Dependency Injection](TODO) for advanced dependency injection configuration with EF Core. -->

## <a name="simple-dbcontext-initialization-with-new"></a>Простая инициализация DbContext с помощью оператора new

Экземпляры `DbContext` можно создать средствами .NET, например с помощью оператора `new` в C#. Настройку можно выполнить путем переопределения метода `OnConfiguring` или передачи параметров в конструктор. Пример:

<!--
    public class ApplicationDbContext : DbContext
    {
        protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
        {
            optionsBuilder.UseSqlServer(@"Server=(localdb)\mssqllocaldb;Database=Test");
        }
    }
-->
[!code-csharp[ApplicationDbContext](../../../samples/core/Miscellaneous/ConfiguringDbContext/WithNew/ApplicationDbContext.cs?name=ApplicationDbContext)]

Этот шаблон также упрощает передачу сведений о конфигурации, например строки подключения, через конструктор `DbContext`. Пример:

<!--
    public class ApplicationDbContext : DbContext
    {
        private readonly string _connectionString;

        public ApplicationDbContext(string connectionString)
        {
            _connectionString = connectionString;
        }

        protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
        {
            optionsBuilder.UseSqlServer(_connectionString);
        }
    }
-->
[!code-csharp[ApplicationDbContext](../../../samples/core/Miscellaneous/ConfiguringDbContext/WithNewAndArgs/ApplicationDbContext.cs?name=ApplicationDbContext)]

Кроме того, вы можете использовать `DbContextOptionsBuilder` для создания объекта `DbContextOptions`, который затем передается в конструктор `DbContext`. Это позволяет явно создать `DbContext`, настроенный для внедрения зависимостей. Например, при использовании `ApplicationDbContext`, определенного для указанных выше веб-приложений ASP.NET Core:

<!--
    public class ApplicationDbContext : DbContext
    {
        public ApplicationDbContext(DbContextOptions<ApplicationDbContext> options)
            : base(options)
        {
        }
    }
-->
[!code-csharp[ApplicationDbContext](../../../samples/core/Miscellaneous/ConfiguringDbContext/WebApp/ApplicationDbContext.cs?name=ApplicationDbContext)]

Вы можете создать `DbContextOptions`, а конструктор может быть вызван явно:

<!--
            var contextOptions = new DbContextOptionsBuilder<ApplicationDbContext>()
                .UseSqlServer(@"Server=(localdb)\mssqllocaldb;Database=Test")
                .Options;

            using var context = new ApplicationDbContext(contextOptions);
-->
[!code-csharp[UseNewForWebApp](../../../samples/core/Miscellaneous/ConfiguringDbContext/WebApp/UseNewForWebApp.cs?name=UseNewForWebApp)]

## <a name="using-a-dbcontext-factory-eg-for-blazor"></a>Использование фабрики DbContext (например, для Blazor)

Некоторые типы приложений (например, [ASP.NET Core Blazor](/aspnet/core/blazor/)) используют внедрение зависимостей, но не создают область службы, соответствующую нужному времени существования `DbContext`. Даже если такое соответствие отсутствует, приложению, возможно, потребуется выполнять несколько единиц работы в рамках такой области, например в одном HTTP-запросе.

В таких случаях можно использовать <xref:Microsoft.Extensions.DependencyInjection.EntityFrameworkServiceCollectionExtensions.AddDbContextFactory%2A> для регистрации фабрики для создания экземпляров `DbContext`. Пример:

<!--
        public void ConfigureServices(IServiceCollection services)
        {
            services.AddDbContextFactory<ApplicationDbContext>(options =>
                options.UseSqlServer(@"Server=(localdb)\mssqllocaldb;Database=Test"));
        }
-->
[!code-csharp[ConfigureServices](../../../samples/core/Miscellaneous/ConfiguringDbContext/WithContextFactory/FactoryServicesExample.cs?name=ConfigureServices)]

Класс `ApplicationDbContext` должен предоставить доступ к общедоступному конструктору с помощью параметра `DbContextOptions<ApplicationDbContext>`. Такой же шаблон используется в традиционных приложениях ASP.NET Core (см. раздел выше).

<!--
    public class ApplicationDbContext : DbContext
    {
        public ApplicationDbContext(DbContextOptions<ApplicationDbContext> options)
            : base(options)
        {
        }
    }
-->
[!code-csharp[ApplicationDbContext](../../../samples/core/Miscellaneous/ConfiguringDbContext/WebApp/ApplicationDbContext.cs?name=ApplicationDbContext)]

Затем фабрику `DbContextFactory` можно использовать в других службах посредством внедрения конструктора. Пример:

<!--
        private readonly IDbContextFactory<ApplicationDbContext> _contextFactory;

        public MyController(IDbContextFactory<ApplicationDbContext> contextFactory)
        {
            _contextFactory = contextFactory;
        }
-->
[!code-csharp[Construct](../../../samples/core/Miscellaneous/ConfiguringDbContext/WithContextFactory/MyController.cs?name=Construct)]

Внедренную фабрику затем можно использовать для создания экземпляров DbContext в коде службы. Пример:

<!--
        public void DoSomething()
        {
            using (var context = _contextFactory.CreateDbContext())
            {
                // ...
            }
        }
-->
[!code-csharp[DoSomething](../../../samples/core/Miscellaneous/ConfiguringDbContext/WithContextFactory/MyController.cs?name=DoSomething)]

Обратите внимание, что созданными таким образом экземплярами `DbContext` **не** управляет поставщик служб приложения. Поэтому приложение должно удалять их.

Дополнительные сведения об использовании EF Core с Blazor см. в статье [ASP.NET Core Blazor Server с Entity Framework Core](/aspnet/core/blazor/blazor-server-ef-core).

## <a name="dbcontextoptions"></a>DbContextOptions

Начальная точка для всех конфигураций `DbContext` — это <xref:Microsoft.EntityFrameworkCore.DbContextOptionsBuilder>. Есть три способа получения этого построителя:

- В `AddDbContext` и связанных методах.
- В `OnConfiguring`
- При явном создании с помощью оператора `new`.

Примеры всех этих вариантов приведены в предыдущих разделах. Такую же конфигурацию можно применить независимо от того, каким способом получен построитель. Кроме того, `OnConfiguring` вызывается в любом случае и независимо от способа создания контекста. Это означает, что `OnConfiguring` можно использовать для выполнения дополнительной настройки даже при использовании `AddDbContext`.

### <a name="configuring-the-database-provider"></a>Настройка поставщика базы данных

Каждый экземпляр `DbContext` необходимо настроить для использования только одного поставщика баз данных. Разные экземпляры подтипа `DbContext` можно использовать с разными поставщиками баз данных, но для отдельного экземпляра нужно назначить только один поставщик. Поставщик базы данных настраивается с помощью специального вызова `Use*`. Например, следующий код позволяет использовать поставщик базы данных SQL Server:

<!--
    public class ApplicationDbContext : DbContext
    {
        protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
        {
            optionsBuilder.UseSqlServer(@"Server=(localdb)\mssqllocaldb;Database=Test");
        }
    }
-->
[!code-csharp[ApplicationDbContext](../../../samples/core/Miscellaneous/ConfiguringDbContext/WithNew/ApplicationDbContext.cs?name=ApplicationDbContext)]

Такие методы `Use*` являются методами расширения, реализованными поставщиком базы данных. Это означает, что пакет NuGet поставщика базы данных нужно установить до использования метода расширения.

> [!TIP]
> Поставщики базы данных EF Core активно используют [методы расширения](/dotnet/csharp/programming-guide/classes-and-structs/extension-methods). Если компилятор указывает, что метод не найден, убедитесь, что пакет NuGet поставщика установлен и что ваш код включает `using Microsoft.EntityFrameworkCore;`.

В представленной ниже таблице приведены примеры для популярных поставщиков баз данных.

| Система базы данных              | Пример конфигурации                         | Пакет NuGet
|:-----------------------------|-----------------------------------------------|--------------
| SQL Server или Azure SQL      | `.UseSqlServer(connectionString)`             | [Microsoft.EntityFrameworkCore.SqlServer](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer/)
| Azure Cosmos DB              | `.UseCosmos(connectionString, databaseName)`  | [Microsoft.EntityFrameworkCore.Cosmos](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Cosmos/)
| SQLite                       | `.UseSqlite(connectionString)`                | [Microsoft.EntityFrameworkCore.Sqlite](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Sqlite/)
| Выполняющаяся в памяти база данных EF Core   | `.UseInMemoryDatabase(databaseName)`          | [Microsoft.EntityFrameworkCore.InMemory](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.InMemory/)
| PostgreSQL*                  | `.UseNpgsql(connectionString)`                | [Npgsql.EntityFrameworkCore.PostgreSQL](https://www.nuget.org/packages/Npgsql.EntityFrameworkCore.PostgreSQL/)
| MySQL или MariaDB*               | `.UseMySql((connectionString)`                | [Pomelo.EntityFrameworkCore.MySql](https://www.nuget.org/packages/Pomelo.EntityFrameworkCore.MySql/)
| Oracle*                      | `.UseOracle(connectionString)`                | [Oracle.EntityFrameworkCore](https://www.nuget.org/packages/Oracle.EntityFrameworkCore/)

* Корпорация Майкрософт не предоставляет эти поставщики баз данных. Дополнительные сведения можно найти в статье [Поставщики баз данных](xref:core/providers/index).

> [!WARNING]
> Выполняющаяся в памяти база данных EF Core не предназначена для использования в рабочей среде. Кроме того, это не самый лучший вариант даже для тестирования. Дополнительные сведения см. в статье [Тестирование кода, использующего EF Core](xref:core/testing/index).

Дополнительные сведения об использовании строк подключения с EF Core см. в статье [Строки подключения](xref:core/miscellaneous/connection-strings).

Необязательная настройка, зависящая от поставщика базы данных, выполняется в дополнительном построителе, предоставляемом поставщиком. Например, вы можете использовать <xref:Microsoft.EntityFrameworkCore.Infrastructure.SqlServerDbContextOptionsBuilder.EnableRetryOnFailure%2A> для настройки повторных попыток для обеспечения устойчивости при подключении к Azure SQL:

<!--
    public class ApplicationDbContext : DbContext
    {
        protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
        {
            optionsBuilder
                .UseSqlServer(
                    @"Server=(localdb)\mssqllocaldb;Database=Test",
                    providerOptions =>
                        {
                            providerOptions.EnableRetryOnFailure();
                        });
        }
    }
-->
[!code-csharp[ApplicationDbContext](../../../samples/core/Miscellaneous/ConfiguringDbContext/AdditionalProviderConfiguration/ApplicationDbContext.cs?name=ApplicationDbContext)]

> [!TIP]
> Этот же поставщик базы данных используется для SQL Server и Azure SQL. Но мы рекомендуем обеспечить [устойчивость подключения](xref:core/miscellaneous/connection-resiliency) к Azure SQL.

Подробные сведения о настройке для конкретных поставщиков см. в статье [Поставщики базы данных](xref:core/providers/index).

### <a name="other-dbcontext-configuration"></a>Другая конфигурация DbContext

Другую конфигурацию `DbContext` можно связать до или после (это не играет роли) вызова`Use*`. Например, чтобы включить ведение журнала для конфиденциальных данных:

<!--
    public class ApplicationDbContext : DbContext
    {
        protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
        {
            optionsBuilder
                .EnableSensitiveDataLogging()
                .UseSqlServer(@"Server=(localdb)\mssqllocaldb;Database=Test");
        }
    }
-->
[!code-csharp[ApplicationDbContext](../../../samples/core/Miscellaneous/ConfiguringDbContext/AdditionalConfiguration/ApplicationDbContext.cs?name=ApplicationDbContext)]

В следующей таблице приведены примеры популярных методов, вызываемых для `DbContextOptionsBuilder`.

| Метод DbContextOptionsBuilder                                                             | Действие                                                | Дополнительные сведения
|:-------------------------------------------------------------------------------------------|-------------------------------------------------------------|--------------
| <xref:Microsoft.EntityFrameworkCore.DbContextOptionsBuilder.UseQueryTrackingBehavior%2A>   | Задает поведение отслеживания по умолчанию для запросов.              | [Поведение отслеживания запросов](xref:core/querying/tracking)
| <xref:Microsoft.EntityFrameworkCore.DbContextOptionsBuilder.LogTo%2A>                      | Простой способ для получения журналов EF Core (для EF Core 5.0 и более поздних версий).    | [Ведение журналов, регистрация событий и диагностика](xref:core/logging-events-diagnostics/index)
| <xref:Microsoft.EntityFrameworkCore.DbContextOptionsBuilder.UseLoggerFactory%2A>           | Регистрирует фабрику `Microsoft.Extensions.Logging`.         | [Ведение журналов, регистрация событий и диагностика](xref:core/logging-events-diagnostics/index)
| <xref:Microsoft.EntityFrameworkCore.DbContextOptionsBuilder.EnableSensitiveDataLogging%2A> | Отвечает за включение данных приложения в исключениях и ведение журналов.         | [Ведение журналов, регистрация событий и диагностика](xref:core/logging-events-diagnostics/index)
| <xref:Microsoft.EntityFrameworkCore.DbContextOptionsBuilder.EnableDetailedErrors%2A>       | Подробные ошибки запросов (в ущерб производительности).  | [Ведение журналов, регистрация событий и диагностика](xref:core/logging-events-diagnostics/index)
| <xref:Microsoft.EntityFrameworkCore.DbContextOptionsBuilder.ConfigureWarnings%2A>          | Позволяет игнорировать или генерировать предупреждения и другие события.               | [Ведение журналов, регистрация событий и диагностика](xref:core/logging-events-diagnostics/index)
| <xref:Microsoft.EntityFrameworkCore.DbContextOptionsBuilder.AddInterceptors%2A>            | Регистрирует перехватчики EF Core.                              | [Ведение журналов, регистрация событий и диагностика](xref:core/logging-events-diagnostics/index)
| <xref:Microsoft.EntityFrameworkCore.ProxiesExtensions.UseLazyLoadingProxies%2A>            | Позволяет использовать динамические прокси-серверы для отложенной загрузки.                        | [Отложенная загрузка](xref:core/querying/related-data/lazy)
| <xref:Microsoft.EntityFrameworkCore.ProxiesExtensions.UseChangeTrackingProxies%2A>         | Позволяет использовать динамические прокси-серверы для отслеживания изменений.                     | Ожидается в ближайшее время...

> [!NOTE]
> <xref:Microsoft.EntityFrameworkCore.ProxiesExtensions.UseLazyLoadingProxies%2A> и <xref:Microsoft.EntityFrameworkCore.ProxiesExtensions.UseChangeTrackingProxies%2A> — это методы расширения из пакета NuGet [Microsoft.EntityFrameworkCore.Proxies](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Proxies/). Такой тип вызова ".UseSomething()" рекомендуется для настройки и (или) использования расширений EF Core в других пакетах.

### <a name="dbcontextoptions-verses-dbcontextoptionstcontext"></a>`DbContextOptions` и `DbContextOptions<TContext>`

Большинство подклассов `DbContext`, которые принимают `DbContextOptions`, должны использовать [универсальный](/dotnet/csharp/programming-guide/generics/) вариант `DbContextOptions<TContext>`. Пример:

<!--
    public sealed class SealedApplicationDbContext : DbContext
    {
        public SealedApplicationDbContext(DbContextOptions<SealedApplicationDbContext> contextOptions)
            : base(contextOptions)
        {
        }
    }
-->
[!code-csharp[SealedApplicationDbContext](../../../samples/core/Miscellaneous/ConfiguringDbContext/InheritDbContext/ApplicationDbContext.cs?name=SealedApplicationDbContext)]

Это гарантирует, что для конкретного подтипа `DbContext` будут разрешены правильные параметры из внедрения зависимостей, даже если зарегистрировано несколько подтипов `DbContext`.

> [!TIP]
> Запечатывать DbContext не обязательно, но рекомендуется для классов, от которых не будут осуществляться наследование.

Но если сам подтип `DbContext` предполагает наследование, он должен предоставить доступ к защищенному конструктору, принимающему неуниверсальный `DbContextOptions`. Пример:

<!--
    public abstract class ApplicationDbContextBase : DbContext
    {
        protected ApplicationDbContextBase(DbContextOptions contextOptions)
            : base(contextOptions)
        {
        }
    }
-->
[!code-csharp[ApplicationDbContextBase](../../../samples/core/Miscellaneous/ConfiguringDbContext/InheritDbContext/ApplicationDbContext.cs?name=ApplicationDbContextBase)]

Это позволяет нескольким конкретным подклассам вызвать такой базовый конструктор с помощью разных универсальных экземпляров `DbContextOptions<TContext>`. Пример:

<!--
    public sealed class ApplicationDbContext1 : ApplicationDbContextBase
    {
        public ApplicationDbContext1(DbContextOptions<ApplicationDbContext1> contextOptions)
            : base(contextOptions)
        {
        }
    }

    public sealed class ApplicationDbContext2 : ApplicationDbContextBase
    {
        public ApplicationDbContext2(DbContextOptions<ApplicationDbContext2> contextOptions)
            : base(contextOptions)
        {
        }
    }
-->
[!code-csharp[ApplicationDbContext12](../../../samples/core/Miscellaneous/ConfiguringDbContext/InheritDbContext/ApplicationDbContext.cs?name=ApplicationDbContext12)]

Обратите внимание, что этот шаблон аналогичен наследованию непосредственно от `DbContext`. То есть конструктор `DbContext` принимает неуниверсальные `DbContextOptions` по этой причине.

Подкласс `DbContext`, который предусматривает создание экземпляра и от которого будет осуществляться наследование, должен предоставить доступ к обеим формам конструктора. Пример:

<!--
    public class ApplicationDbContext : DbContext
    {
        public ApplicationDbContext(DbContextOptions<ApplicationDbContext> contextOptions)
            : base(contextOptions)
        {
        }

        protected ApplicationDbContext(DbContextOptions contextOptions)
            : base(contextOptions)
        {
        }
    }
-->
[!code-csharp[ApplicationDbContext](../../../samples/core/Miscellaneous/ConfiguringDbContext/InheritDbContext/ApplicationDbContext.cs?name=ApplicationDbContext)]

## <a name="design-time-dbcontext-configuration"></a>Настройка DbContext во время разработки

Средства разработки EF Core, такие как средства для [миграции EF Core](xref:core/managing-schemas/migrations/index), должны иметь возможность обнаруживать и создавать рабочие экземпляры типа `DbContext` для сбора сведений о типах сущностей приложения и их сопоставления со схемой базы данных. Этот процесс можно автоматизировать, так как средство может без труда создать `DbContext` с теми же настройками, которые заданы во время выполнения.

Хотя любой шаблон, предоставляющий `DbContext` необходимые сведения о конфигурации, может работать во время выполнения, средства, требующие использования `DbContext` во время разработки, можно использовать только с ограниченным количеством шаблонов. Дополнительные сведения об этом см. в статье [Создание DbContext во время разработки](xref:core/cli/dbcontext-creation).

## <a name="avoiding-dbcontext-threading-issues"></a>Предотвращение проблем с потоками DbContext

Entity Framework Core не поддерживает выполнение нескольких параллельных операций в одном экземпляре `DbContext`, включая параллельное выполнение асинхронных запросов и любое явное использование экземпляра из нескольких потоков одновременно. Поэтому обязательно сразу же применяйте `await` к асинхронным вызовам или используйте отдельные экземпляры `DbContext`, выполняемые параллельно.

Когда EF Core обнаруживает попытку одновременного использования экземпляра `DbContext`, отобразится `InvalidOperationException` с примерно таким сообщением:

> В этом контексте была начата вторая операция до завершения предыдущей операции. Как правило, так происходит, когда несколько потоков используют один и тот же экземпляр DbContext. Но потокобезопасность членов экземпляров не гарантируется.

Если одновременный доступ не обнаруживается, это может привести к неопределенному поведению, аварийному завершению работы приложения и повреждению данных.

Есть распространенные ошибки, из-за которых может случайно возникнуть одновременный доступ к одному и тому же экземпляру `DbContext`.

### <a name="asynchronous-operation-pitfalls"></a>Ошибки, связанные с асинхронными операциями

Асинхронные методы позволяют EF Core инициировать операции, обращающиеся к базе данных, без блокировки. Но если вызывающий объект не ожидает завершения одного из этих методов и переходит к выполнению других операций с `DbContext`, структура `DbContext` может быть (и, скорее всего, будет) повреждена.

Обязательно сразу же применяйте await к асинхронным методам EF Core.

### <a name="implicitly-sharing-dbcontext-instances-via-dependency-injection"></a>Неявное совместное использование экземпляров DbContext путем внедрения зависимостей

Метод расширения [`AddDbContext`](/dotnet/api/microsoft.extensions.dependencyinjection.entityframeworkservicecollectionextensions.adddbcontext) по умолчанию регистрирует типы `DbContext` с заданной [областью времени существования](/aspnet/core/fundamentals/dependency-injection#service-lifetimes).

Это обеспечивает защиту от проблем с одновременным доступом в большинстве приложений ASP.NET Core, так как есть только один поток, в котором каждый запрос клиента выполняется в определенный момент времени, и каждый такой запрос получает отдельную область внедрения зависимостей (и, следовательно, отдельный экземпляр `DbContext`). В модели размещения Blazor Server используется один логический запрос для обслуживания пользовательского канала Blazor. Поэтому при использовании области внедрения по умолчанию для каждого пользовательского канала доступен только один экземпляр DbContext с заданной областью.

Если код явно выполняет несколько потоков в параллельном режиме, нужно исключить возможность одновременного доступа к экземплярам `DbContext`.

При использовании внедрения зависимостей это можно реализовать путем регистрации контекста как ограниченного областью, создания области (с помощью `IServiceScopeFactory`) для каждого потока или регистрации экземпляра `DbContext` как временного (с помощью перегрузки `AddDbContext`, при которой принимается параметр `ServiceLifetime`).

## <a name="more-reading"></a>Дополнительные материалы

- Дополнительные сведения о внедрении зависимостей см. [здесь](/aspnet/core/fundamentals/dependency-injection).
- Подробную информацию см. в статье [Тестирование кода, использующего EF Core](xref:core/testing/index).
