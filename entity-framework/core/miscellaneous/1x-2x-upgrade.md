---
title: Обновление предыдущих версий до EF Core 2 — EF Core
author: divega
ms.date: 08/13/2017
ms.assetid: 8BD43C8C-63D9-4F3A-B954-7BC518A1B7DB
uid: core/miscellaneous/1x-2x-upgrade
ms.openlocfilehash: b27c09fdb6210dd7c6aa0c8bc912a8bd183c16b9
ms.sourcegitcommit: 7a709ce4f77134782393aa802df5ab2718714479
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/04/2019
ms.locfileid: "74824428"
---
# <a name="upgrading-applications-from-previous-versions-to-ef-core-20"></a>Обновление приложений с предыдущих версий до EF Core 2,0

Мы предпользовались возможностью значительно усовершенствовать существующие API и поведения в 2,0. Существует несколько усовершенствований, которые могут потребовать изменения существующего кода приложения, хотя мы считаем, что большинство приложений оказываются низкими, в большинстве случаев требуется только повторная компиляция и минимальные изменения, позволяющие заменить устаревшие API.

Для обновления существующего приложения до EF Core 2,0 может потребоваться:

1. Обновление целевой реализации .NET приложения до версии, поддерживающей .NET Standard 2,0. Дополнительные сведения см. в разделе [Поддерживаемые реализации .NET](xref:core/platforms/index) .

2. Определение поставщика для целевой базы данных, совместимого с EF Core 2,0. См. [EF Core 2,0 требуется поставщик базы данных 2,0](#ef-core-20-requires-a-20-database-provider) ниже.

3. Обновление всех пакетов EF Core (сред выполнения и средств) до 2,0. Дополнительные сведения см. в разделе об [установке EF Core](xref:core/get-started/install/index) .

4. Внесите необходимые изменения в код, чтобы компенсировать критические изменения, описанные в оставшейся части этого документа.

## <a name="aspnet-core-now-includes-ef-core"></a>ASP.NET Core теперь включает EF Core

Приложения, предназначенные для ASP.NET Core 2.0, могут использовать EF Core 2.0 без дополнительных зависимостей, кроме сторонних поставщиков базы данных. Однако для использования EF Core 2,0 в приложениях, предназначенных для предыдущих версий ASP.NET Core требуется обновление до ASP.NET Core 2,0. Дополнительные сведения об обновлении ASP.NET Core приложений до 2,0 см. в [ASP.NET Core документации по теме](/aspnet/core/migration/1x-to-2x/).

## <a name="new-way-of-getting-application-services-in-aspnet-core"></a>Новый способ получения служб приложений в ASP.NET Core

Рекомендуемый шаблон для ASP.NET Core веб-приложений был обновлен для 2,0 таким образом, что логика времени разработки EF Core использовалась в 1. x. Ранее во время разработки EF Core попытается вызвать `Startup.ConfigureServices` напрямую, чтобы получить доступ к поставщику услуг приложения. В ASP.NET Core 2,0 конфигурация инициализируется за пределами класса `Startup`. Приложения, использующие EF Core, обычно обращаются к своей строке подключения из конфигурации, поэтому `Startup` сама по себе больше не достаточно. При обновлении приложения ASP.NET Core 1. x может появиться следующая ошибка при использовании средств EF Core.

> В "Аппликатионконтекст" не найден конструктор без параметров. Либо добавьте конструктор без параметров в "Аппликатионконтекст", либо добавьте реализацию "Идесигнтимедбконтекстфактори&lt;Аппликатионконтекст&gt;" в той же сборке, что и "Аппликатионконтекст"

В шаблоне по умолчанию для ASP.NET Core 2.0 добавлен новый обработчик времени разработки. Статический метод `Program.BuildWebHost` позволяет EF Core доступ к поставщику службы приложения во время разработки. При обновлении приложения ASP.NET Core 1. x необходимо обновить класс `Program`, чтобы он наглядел следующим образом.

``` csharp
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

Внедрение этого нового шаблона при обновлении приложений до версии 2,0 настоятельно рекомендуется и необходимо для того, чтобы такие функции продукта, как Entity Framework Core миграции работали, были доступны. Другой распространенной альтернативой является [реализация *Идесигнтимедбконтекстфактори\<тконтекст >* ](xref:core/miscellaneous/cli/dbcontext-creation#from-a-design-time-factory).

## <a name="idbcontextfactory-renamed"></a>Идбконтекстфактори переименован

Чтобы обеспечить поддержку разнообразных шаблонов приложений и предоставить пользователям больший контроль над использованием `DbContext` во время разработки, мы, в прошлом, указали интерфейс `IDbContextFactory<TContext>`. Во время разработки EF Core средства будут обнаруживать реализации этого интерфейса в проекте и использовать его для создания объектов `DbContext`.

Этот интерфейс имеет очень общее имя, что позволяет некоторым пользователям использовать его повторно для других сценариев создания `DbContext`. Они были перехвачены, когда средства EF попытались использовать их реализацию во время разработки и вызвали сбой таких команд, как `Update-Database` или `dotnet ef database update`.

Чтобы сообщить о строгой семантике этого интерфейса во время разработки, мы переименовали его на `IDesignTimeDbContextFactory<TContext>`.

Для выпуска 2,0 `IDbContextFactory<TContext>` по-прежнему существует, но помечена как устаревшая.

## <a name="dbcontextfactoryoptions-removed"></a>Дбконтекстфакторйоптионс удален

Из-за изменений ASP.NET Core 2,0, описанных выше, мы обнаружили, что `DbContextFactoryOptions` больше не требовалось в новом интерфейсе `IDesignTimeDbContextFactory<TContext>`. Вместо этого следует использовать альтернативные варианты.

| дбконтекстфакторйоптионс | Альтернатива                                                  |
|:------------------------|:-------------------------------------------------------------|
| аппликатионбасепас     | AppContext. Баседиректори                                     |
| ContentRootPath         | Directory. GetCurrentDirectory ()                              |
| EnvironmentName         | Environment. GetEnvironmentVariable ("ASPNETCORE_ENVIRONMENT") |

## <a name="design-time-working-directory-changed"></a>Рабочий каталог времени разработки изменен

В ASP.NET Core 2,0 также требуется рабочий каталог, используемый `dotnet ef` для согласования с рабочим каталогом, используемым Visual Studio при запуске приложения. Одним из наблюдаемых побочных эффектов является то, что файлы SQLite теперь относятся к каталогу проекта, а не к выходному каталогу, который они используют.

## <a name="ef-core-20-requires-a-20-database-provider"></a>Для EF Core 2,0 требуется поставщик базы данных 2,0

Для EF Core 2,0 мы внесли много упрощений и улучшений в способ работы поставщиков баз данных. Это означает, что поставщики 1.0. x и 1.1. x не будут работать с EF Core 2,0.

Поставщики SQL Server и SQLite поставляются группой EF и версии 2,0 будут доступны в рамках выпуска 2,0. Сторонние поставщики с открытым кодом для [SQL Compact](https://github.com/ErikEJ/EntityFramework.SqlServerCompact), [PostgreSQL](https://github.com/npgsql/Npgsql.EntityFrameworkCore.PostgreSQL)и [MySQL](https://github.com/PomeloFoundation/Pomelo.EntityFrameworkCore.MySql) обновляются для 2,0. Для всех других поставщиков обратитесь к модулю записи поставщика.

## <a name="logging-and-diagnostics-events-have-changed"></a>События ведения журнала и диагностики изменены

Примечание. Эти изменения не должны влиять на большую часть кода приложения.

Идентификаторы событий для сообщений, отправленных в [ILogger](/dotnet/api/microsoft.extensions.logging.ilogger) , были изменены в 2,0. Теперь эти идентификаторы событий являются уникальными в рамках всего кода EF Core. Эти сообщения теперь соответствуют стандартному шаблону для структурированного ведения журнала, используемому, например, MVC.

Кроме того, были изменены категории средства ведения журнала. Теперь используется известный набор категорий, доступных через [DbLoggerCategory](https://github.com/aspnet/EntityFrameworkCore/blob/rel/2.0.0/src/EFCore/DbLoggerCategory.cs).

События [DiagnosticSource](https://github.com/dotnet/corefx/blob/master/src/System.Diagnostics.DiagnosticSource/src/DiagnosticSourceUsersGuide.md) теперь используют те же имена идентификаторов событий, что и соответствующие сообщения `ILogger`. Полезные данные события — это все номинальные типы, производные от [EVENTDATA](/dotnet/api/microsoft.entityframeworkcore.diagnostics.eventdata).

Идентификаторы событий, типы полезных данных и категории описаны в классах [коривентид](/dotnet/api/microsoft.entityframeworkcore.diagnostics.coreeventid) и [релатионалевентид](/dotnet/api/microsoft.entityframeworkcore.diagnostics.relationaleventid) .

Идентификаторы также перемещены из Microsoft. EntityFrameworkCore. Infrastructure в новое пространство имен Microsoft. EntityFrameworkCore. Diagnostics.

## <a name="ef-core-relational-metadata-api-changes"></a>EF Core изменения API реляционных метаданных

EF Core 2.0 теперь будет создавать отдельный объект [IModel](/dotnet/api/microsoft.entityframeworkcore.metadata.imodel) для каждого из используемых поставщиков. Обычно это является прозрачным для приложения. Это упрощает упрощение интерфейсов API метаданных более низкого уровня, так что любой доступ к _общим концепциям реляционных метаданных_ всегда осуществляется с помощью вызова `.Relational` вместо `.SqlServer`, `.Sqlite`и т. д. Например, код 1.1. x выглядит следующим образом:

``` csharp
var tableName = context.Model.FindEntityType(typeof(User)).SqlServer().TableName;
```

Теперь должен быть написан следующим образом:

``` csharp
var tableName = context.Model.FindEntityType(typeof(User)).Relational().TableName;
```

Вместо использования методов, таких как `ForSqlServerToTable`, методы расширения теперь доступны для написания условного кода на основе текущего используемого поставщика. Например:

```csharp
modelBuilder.Entity<User>().ToTable(
    Database.IsSqlServer() ? "SqlServerName" : "OtherName");
```

Обратите внимание, что это изменение применяется только к API-интерфейсам и метаданным, определенным для _всех_ реляционных поставщиков. API и метаданные остаются неизменными, если они относятся только к одному поставщику. Например, кластеризованные индексы относятся к SQL-серверу, поэтому `ForSqlServerIsClustered` и `.SqlServer().IsClustered()` должны по-прежнему использоваться.

## <a name="dont-take-control-of-the-ef-service-provider"></a>Не контролируйте поставщик служб EF

EF Core использует внутреннее `IServiceProvider` (контейнер внедрения зависимостей) для своей внутренней реализации. Приложения должны разрешать EF Core создавать и управлять этим поставщиком, за исключением особых случаев. Настоятельно рекомендуем удалить все вызовы `UseInternalServiceProvider`. Если приложению необходимо вызвать `UseInternalServiceProvider`, попробуйте устранить [проблему](https://github.com/aspnet/EntityFramework/Issues) , чтобы мы могли исследовать другие способы решения вашей ситуации.

Вызов `AddEntityFramework`, `AddEntityFrameworkSqlServer`и т. д. не требуется для кода приложения, если не вызывается `UseInternalServiceProvider`. Удалите все существующие вызовы `AddEntityFramework` или `AddEntityFrameworkSqlServer`и т. д. `AddDbContext` должны использоваться так же, как и раньше.

## <a name="in-memory-databases-must-be-named"></a>Базы данных в памяти должны иметь имена

Глобальная неименованная база данных в памяти была удалена, а все базы данных в памяти должны иметь имя. Например:

``` csharp
optionsBuilder.UseInMemoryDatabase("MyDatabase");
```

При этом создается или используется база данных с именем "MyDatabase". Если `UseInMemoryDatabase` вызывается снова с тем же именем, будет использоваться та же база данных в памяти, что позволяет совместно использовать несколько экземпляров контекста.

## <a name="read-only-api-changes"></a>Изменения API только для чтения

`IsReadOnlyBeforeSave`, `IsReadOnlyAfterSave`и `IsStoreGeneratedAlways` были устаревшими и заменены на [бефоресавебехавиор](/dotnet/api/microsoft.entityframeworkcore.metadata.iproperty.beforesavebehavior) и [афтерсавебехавиор](/dotnet/api/microsoft.entityframeworkcore.metadata.iproperty.aftersavebehavior). Эти поведения применяются к любому свойству (не только к созданным хранилищем свойствам) и определяют, как значение свойства должно использоваться при вставке в строку базы данных (`BeforeSaveBehavior`) или при обновлении существующей строки базы данных (`AfterSaveBehavior`).

Свойства, помеченные как [валуеженератед. онаддорупдате](/dotnet/api/microsoft.entityframeworkcore.metadata.valuegenerated) (например, для "вычисленных столбцов"), по умолчанию будут игнорировать любые значения, заданные в данный момент в свойстве. Это означает, что созданное хранилищем значение всегда будет получено независимо от того, было ли значение установлено или изменено для объекта Tracked. Это можно изменить, задав другой `Before\AfterSaveBehavior`.

## <a name="new-clientsetnull-delete-behavior"></a>Новое поведение при удалении Клиентсетнулл

В предыдущих выпусках [делетебехавиор. restrict](/dotnet/api/microsoft.entityframeworkcore.deletebehavior) имел поведение для сущностей, которые были отслеживанием в контексте, более закрытым соответствующей семантикой `SetNull`. В EF Core 2,0 в качестве по умолчанию для дополнительных связей было введено новое поведение `ClientSetNull`. Это поведение имеет `SetNull`ую семантику для отслеживания сущностей и поведения `Restrict` для баз данных, созданных с помощью EF Core. В нашем опыте это наиболее ожидаемое и полезное поведение для отслеживания сущностей и базы данных. `DeleteBehavior.Restrict` теперь учитывается для отслеживания сущностей, если заданы дополнительные связи.

## <a name="provider-design-time-packages-removed"></a>Пакеты времени разработки поставщика удалены

Пакет `Microsoft.EntityFrameworkCore.Relational.Design` был удален. Его содержимое было объединено в `Microsoft.EntityFrameworkCore.Relational` и `Microsoft.EntityFrameworkCore.Design`.

Это распространяется на пакеты времени разработки поставщика. Эти пакеты (`Microsoft.EntityFrameworkCore.Sqlite.Design`, `Microsoft.EntityFrameworkCore.SqlServer.Design`и т. д.) были удалены, а их содержимое объединено в основные пакеты поставщиков.

Чтобы включить `Scaffold-DbContext` или `dotnet ef dbcontext scaffold` в EF Core 2,0, необходимо ссылаться только на один пакет поставщика:

``` xml
<PackageReference Include="Microsoft.EntityFrameworkCore.SqlServer"
    Version="2.0.0" />
<PackageReference Include="Microsoft.EntityFrameworkCore.Tools"
    Version="2.0.0"
    PrivateAssets="All" />
<DotNetCliToolReference Include="Microsoft.EntityFrameworkCore.Tools.DotNet"
    Version="2.0.0" />
```
