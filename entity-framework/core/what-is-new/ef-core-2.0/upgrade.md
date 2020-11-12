---
title: Обновление предыдущих версий до EF Core 2 — EF Core
description: Инструкции и замечания по обновлению до Entity Framework Core 2.0
author: ajcvickers
ms.date: 08/13/2017
uid: core/what-is-new/ef-core-2.0/upgrade
ms.openlocfilehash: 5054661d308e7ea6acd433981dfb2af6026b7765
ms.sourcegitcommit: f3512e3a98e685a3ba409c1d0157ce85cc390cf4
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2020
ms.locfileid: "94430096"
---
# <a name="upgrading-applications-from-previous-versions-to-ef-core-20"></a>Обновление приложений с предыдущих версий до EF Core 2.0

В версии 2.0 мы воспользовались возможностью значительно усовершенствовать существующие API и поведение. Есть несколько нововведений, которые могут потребовать изменения существующего кода приложения, хотя мы надеемся, что влияние на большинство приложений окажется незначительным. В большинстве случаев требуется только повторная компиляция и минимальные документированные изменения по замене устаревших API.

Для обновления существующего приложения до EF Core 2.0 может потребоваться:

1. Обновление целевой реализации .NET для приложения до версии, поддерживающей .NET Standard 2.0. Дополнительные сведения см. в разделе [Поддерживаемые реализации .NET](xref:core/miscellaneous/platforms).

2. Определение поставщика для целевой базы данных, совместимого с EF Core 2.0. См. раздел [EF Core 2.0 требует поставщика базы данных версии 2.0](#ef-core-20-requires-a-20-database-provider) ниже.

3. Обновление всех пакетов EF Core (сред выполнения и инструментария) до версии 2.0. Дополнительные сведения см. в разделе [Установка EF Core](xref:core/get-started/overview/install).

4. Внесение необходимых изменений в код, чтобы компенсировать критические изменения, описанные в оставшейся части этого документа.

## <a name="aspnet-core-now-includes-ef-core"></a>ASP.NET Core теперь включает EF Core

Приложения, предназначенные для ASP.NET Core 2.0, могут использовать EF Core 2.0 без дополнительных зависимостей, кроме сторонних поставщиков базы данных. Однако приложения, предназначенные для предыдущих версий ASP.NET Core, нужно обновить до ASP.NET Core 2.0, чтобы они могли использовать EF Core 2.0. Дополнительные сведения об обновлении приложений ASP.NET Core до версии 2.0 см. в [соответствующем разделе документации по ASP.NET Core](/aspnet/core/migration/1x-to-2x/).

## <a name="new-way-of-getting-application-services-in-aspnet-core"></a>Новый способ получения служб приложений в ASP.NET Core

Рекомендуемый шаблон для веб-приложений ASP.NET Core был обновлен для версии 2.0, что нарушило логику времени разработки EF Core, используемую в 1.x. Ранее во время разработки EF Core пытался вызвать `Startup.ConfigureServices` напрямую, чтобы получить доступ к поставщику служб приложения. В ASP.NET Core 2.0 конфигурация инициализируется за пределами класса `Startup`. Приложения, использующие EF Core, обычно обращаются к своей строке подключения из конфигурации, поэтому одного класса `Startup` уже недостаточно. При обновлении приложения ASP.NET Core 1. x может появиться следующая ошибка при использовании средств EF Core.

> В "ApplicationContext" не найден конструктор без параметров. Либо добавьте конструктор без параметров в "ApplicationContext", либо добавьте реализацию "IDesignTimeDbContextFactory&lt;ApplicationContext&gt;" в той же сборке, что и "ApplicationContext"

В шаблоне по умолчанию для ASP.NET Core 2.0 добавлен новый обработчик времени разработки. Статический метод `Program.BuildWebHost` позволяет EF Core получить доступ к поставщику службы приложения во время разработки. При обновлении приложения ASP.NET Core 1. x необходимо обновить класс `Program`, чтобы он выглядел следующим образом.

```csharp
using Microsoft.AspNetCore;
using Microsoft.AspNetCore.Hosting;

namespace AspNetCoreDotNetCore2._0App
{
    public class Program
    {
        public static void Main(string[] args)
        {
            BuildWebHost(args).Run();
        }

        public static IWebHost BuildWebHost(string[] args) =>
            WebHost.CreateDefaultBuilder(args)
                .UseStartup<Startup>()
                .Build();
    }
}
```

Внедрение этого нового шаблона при обновлении приложений до версии 2.0 настоятельно рекомендуется и необходимо для работы таких функций продукта, как миграция Entity Framework Core. Другой распространенной альтернативой является [реализация *IDesignTimeDbContextFactory\<TContext>*](xref:core/cli/dbcontext-creation#from-a-design-time-factory).

## <a name="idbcontextfactory-renamed"></a>IDbContextFactory переименован

Чтобы обеспечить поддержку разнообразных шаблонов приложений и предоставить пользователям больший контроль над использованием `DbContext` во время разработки, мы, в прошлом, предоставляли интерфейс `IDbContextFactory<TContext>`. Во время разработки средства EF Core обнаруживали реализации этого интерфейса в проекте и использовали его для создания объектов `DbContext`.

Этот интерфейс имел очень общее имя, которое вводило некоторых пользователей в заблуждение, и они пытались использовать его повторно в других сценариях создания `DbContext`. Они были застигнуты врасплох, когда средства EF пытались использовать их реализацию во время разработки и вызвали сбой таких команд, как `Update-Database` или `dotnet ef database update`.

Чтобы сообщить о строгой семантике этого интерфейса во время разработки, мы переименовали его в `IDesignTimeDbContextFactory<TContext>`.

В версии 2.0 `IDbContextFactory<TContext>` по-прежнему существует, но помечен как устаревший.

## <a name="dbcontextfactoryoptions-removed"></a>DbContextFactoryOptions удален

Из-за описанных выше изменений ASP.NET Core 2.0 мы обнаружили, что `DbContextFactoryOptions` больше не требуется в новом интерфейсе `IDesignTimeDbContextFactory<TContext>`. Вместо этого следует использовать альтернативные варианты.

| DbContextFactoryOptions | Альтернатива                                                  |
|:------------------------|:-------------------------------------------------------------|
| ApplicationBasePath     | AppContext.BaseDirectory                                     |
| ContentRootPath         | Directory.GetCurrentDirectory()                              |
| EnvironmentName         | Environment.GetEnvironmentVariable("ASPNETCORE_ENVIRONMENT") |

## <a name="design-time-working-directory-changed"></a>Рабочий каталог времени разработки изменен

Изменения в ASP.NET Core 2.0 также требуют, чтобы рабочий каталог, используемый `dotnet ef`, согласовывался с рабочим каталогом, используемым Visual Studio при запуске приложения. Одним из наблюдаемых побочных эффектов является то, что имена файлов SQLite теперь относятся к каталогу проекта, а не к выходному каталогу, как было раньше.

## <a name="ef-core-20-requires-a-20-database-provider"></a>EF Core 2.0 требует поставщика базы данных версии 2.0

В EF Core 2.0 мы внесли много упрощений и улучшений в механизм работы поставщиков баз данных. Это означает, что поставщики 1.0.x и 1.1.x не будут работать с EF Core 2.0.

Поставщики SQL Server и SQLite поставляются командой разработчиков EF, и версии 2.0 будут доступны в рамках выпуска EF Core 2.0. Сторонние поставщики с открытым кодом для [SQL Compact](https://github.com/ErikEJ/EntityFramework.SqlServerCompact), [PostgreSQL](https://github.com/npgsql/Npgsql.EntityFrameworkCore.PostgreSQL) и [MySQL](https://github.com/PomeloFoundation/Pomelo.EntityFrameworkCore.MySql) находятся в процессе обновления до версии 2.0. Для всех других поставщиков обратитесь к автору поставщика.

## <a name="logging-and-diagnostics-events-have-changed"></a>События журнала и диагностики изменены

Примечание. Эти изменения не должны повлиять на большую часть кода приложений.

В версии 2.0 были изменены идентификаторы событий для сообщений, отправляемых в [ILogger](/dotnet/api/microsoft.extensions.logging.ilogger). Теперь эти идентификаторы событий являются уникальными в рамках всего кода EF Core. Эти сообщения теперь соответствуют стандартному шаблону для структурированного ведения журнала, используемому, например, MVC.

Кроме того, были изменены категории средства ведения журнала. Теперь используется известный набор категорий, доступных через [DbLoggerCategory](https://github.com/dotnet/efcore/blob/rel/2.0.0/src/EFCore/DbLoggerCategory.cs).

События [DiagnosticSource](https://github.com/dotnet/corefx/blob/master/src/System.Diagnostics.DiagnosticSource/src/DiagnosticSourceUsersGuide.md) теперь используют те же имена идентификаторов событий, что и соответствующие сообщения `ILogger`. Все полезные данные события представляют собой номинальные типы, производные от [EventData](/dotnet/api/microsoft.entityframeworkcore.diagnostics.eventdata).

Идентификаторы событий, типы полезных данных и категории задокументированы в описаниях классов [CoreEventId](/dotnet/api/microsoft.entityframeworkcore.diagnostics.coreeventid) и [RelationalEventId](/dotnet/api/microsoft.entityframeworkcore.diagnostics.relationaleventid).

Идентификаторы также перемещены из Microsoft.EntityFrameworkCore.Infrastructure в новое пространство имен Microsoft.EntityFrameworkCore.Diagnostics.

## <a name="ef-core-relational-metadata-api-changes"></a>Изменения API реляционных метаданных EF Core

EF Core 2.0 теперь будет создавать отдельный объект [IModel](/dotnet/api/microsoft.entityframeworkcore.metadata.imodel) для каждого из используемых поставщиков. Обычно это является прозрачным для приложения. Это позволило упростить работу с API метаданных более низкого уровня, например, добиться того, что любое обращение к _основным концепциям реляционных метаданных_ всегда осуществляется путем вызова `.Relational` вместо `.SqlServer`, `.Sqlite` и т. д. Поэтому следующий код 1.1.x

```csharp
var tableName = context.Model.FindEntityType(typeof(User)).SqlServer().TableName;
```

теперь следует писать так:

```csharp
var tableName = context.Model.FindEntityType(typeof(User)).Relational().TableName;
```

Вместо использования таких методов, как `ForSqlServerToTable`, теперь доступны методы расширения для написания условного кода на основе текущего используемого поставщика. Пример:

```csharp
modelBuilder.Entity<User>().ToTable(
    Database.IsSqlServer() ? "SqlServerName" : "OtherName");
```

Обратите внимание, что это изменение применяется только к API-интерфейсам и метаданным, определенным для _всех_ реляционных поставщиков. API и метаданные остаются неизменными, если они относятся только к одному поставщику. Например, кластеризованные индексы относятся к SQL Sever, поэтому должны по-прежнему использоваться `ForSqlServerIsClustered` и `.SqlServer().IsClustered()`.

## <a name="dont-take-control-of-the-ef-service-provider"></a>Не контролируйте поставщика служб EF

EF Core использует внутренний `IServiceProvider` (контейнер внедрения зависимостей) для своей внутренней реализации. Приложения должны позволить EF Core создавать и управлять этим поставщиком, за исключением особых случаев. Настоятельно рекомендуем удалить все вызовы `UseInternalServiceProvider`. Если приложению необходимо вызвать `UseInternalServiceProvider`, рассмотрите возможность [регистрации проблемы](https://github.com/dotnet/efcore/Issues), чтобы мы могли изучить другие способы решения вашего вопроса.

Вызов `AddEntityFramework`, `AddEntityFrameworkSqlServer` и т. д. не требуется для кода приложения, если только `UseInternalServiceProvider` также не вызывается. Удалите все существующие вызовы `AddEntityFramework`, `AddEntityFrameworkSqlServer` и т. д. `AddDbContext` должен использоваться так же, как и раньше.

## <a name="in-memory-databases-must-be-named"></a>Базы данных в памяти должны иметь имена

Глобальные неименованные базы данных в памяти были удалены, и теперь все базы данных в памяти должны иметь имя. Пример:

```csharp
optionsBuilder.UseInMemoryDatabase("MyDatabase");
```

При этом создается или используется база данных с именем "MyDatabase". Если `UseInMemoryDatabase` вызывается снова с тем же именем, будет использоваться та же база данных в памяти, что позволяет совместно использовать в нескольких экземплярах контекста.

## <a name="read-only-api-changes"></a>Изменения API только для чтения

`IsReadOnlyBeforeSave`, `IsReadOnlyAfterSave`и `IsStoreGeneratedAlways` были устаревшими и заменены на [BeforeSaveBehavior](/dotnet/api/microsoft.entityframeworkcore.metadata.iproperty.beforesavebehavior) и [AfterSaveBehavior](/dotnet/api/microsoft.entityframeworkcore.metadata.iproperty.aftersavebehavior). Это поведение применяется к любому свойству (не только к свойствам, созданным хранилищем) и определяет, как значение свойства должно использоваться при вставке в строку базы данных (`BeforeSaveBehavior`) или при обновлении существующей строки базы данных (`AfterSaveBehavior`).

Свойства, помеченные как [ValueGenerated.OnAddOrUpdate](/dotnet/api/microsoft.entityframeworkcore.metadata.valuegenerated) (например, для вычисляемых столбцов), по умолчанию будут игнорировать любые значения, заданные в данный момент в свойстве. Это означает, что созданное хранилищем значение всегда будет получено независимо от того, было ли какое-либо значение установлено или изменено для отслеживаемого объекта. Это поведение можно изменить, задав другое значение `Before\AfterSaveBehavior`.

## <a name="new-clientsetnull-delete-behavior"></a>Новое поведение ClientSetNull при удалении

В предыдущих выпусках [DeleteBehavior.Restrict](/dotnet/api/microsoft.entityframeworkcore.deletebehavior) имело для сущностей, отслеживаемых контекстом, поведение, которое ближе соответствовало семантике `SetNull`. В EF Core 2.0 было введено новое поведение по умолчанию `ClientSetNull` для дополнительных связей. Это поведение имеет семантику `SetNull` для отслеживаемых сущностей и поведение `Restrict` для баз данных, созданных с помощью EF Core. Из нашего опыта, это является наиболее ожидаемым и полезным поведением для отслеживаемых сущностей и баз данных. `DeleteBehavior.Restrict` теперь учитывается для отслеживаемых сущностей, если заданы дополнительные связи.

## <a name="provider-design-time-packages-removed"></a>Пакеты времени разработки поставщика удалены

Пакет `Microsoft.EntityFrameworkCore.Relational.Design` был удален. Его содержимое было объединено в `Microsoft.EntityFrameworkCore.Relational` и `Microsoft.EntityFrameworkCore.Design`.

Это распространяется на пакеты времени разработки поставщика. Эти пакеты (`Microsoft.EntityFrameworkCore.Sqlite.Design`, `Microsoft.EntityFrameworkCore.SqlServer.Design` и т. д.) были удалены, а их содержимое объединено в основные пакеты поставщиков.

Чтобы включить `Scaffold-DbContext` или `dotnet ef dbcontext scaffold` в EF Core 2.0, достаточно сослаться только на один пакет поставщика.

```xml
<PackageReference Include="Microsoft.EntityFrameworkCore.SqlServer"
    Version="2.0.0" />
<PackageReference Include="Microsoft.EntityFrameworkCore.Tools"
    Version="2.0.0"
    PrivateAssets="All" />
<DotNetCliToolReference Include="Microsoft.EntityFrameworkCore.Tools.DotNet"
    Version="2.0.0" />
```
