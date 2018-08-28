---
title: Обновление с предыдущих версий до EF Core 2 — EF Core
author: divega
ms.date: 8/13/2017
ms.assetid: 8BD43C8C-63D9-4F3A-B954-7BC518A1B7DB
uid: core/miscellaneous/1x-2x-upgrade
ms.openlocfilehash: 6df57b04808307238287094c285727ececc6c18d
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/27/2018
ms.locfileid: "42996818"
---
# <a name="upgrading-applications-from-previous-versions-to-ef-core-20"></a>Обновление приложений из предыдущих версий до EF Core 2.0

## <a name="procedures-common-to-all-applications"></a>Процедуры, общие для всех приложений

Может потребоваться обновление существующего приложения до EF Core 2.0:

1. Обновление целевой платформы .NET приложения, которая поддерживает .NET Standard 2.0. См. в разделе [поддерживаемые платформы](../platforms/index.md) для получения дополнительных сведений.

2. Определения поставщика для целевой базы данных, которая совместима с EF Core 2.0. См. в разделе [EF Core 2.0 не требуется поставщик базы данных 2.0](#ef-core-20-requires-a-20-database-provider) ниже.

3. Обновление всех пакетов EF Core (среды выполнения и инструментов) 2.0. Ссылаться на [Установка EF Core](../get-started/install/index.md) для получения дополнительных сведений.

4. Внесите изменения необходимый код для компенсации критические изменения. См. в разделе [критические изменения](#breaking-changes) Дополнительные сведения в приведенном ниже разделе.

## <a name="aspnet-core-applications"></a>Приложения ASP.NET Core

1. См. в частности [новый шаблон для инициализации поставщика службы приложения](#new-way-of-getting-application-services) описано ниже.

> [!TIP]  
> Внедрение этого нового шаблона, при обновлении приложений 2.0 настоятельно рекомендуется и необходимо для таких функций, как миграция Entity Framework Core для работы. Типичная альтернатива — [реализовать *IDesignTimeDbContextFactory\<TContext >*](xref:core/miscellaneous/cli/dbcontext-creation#from-a-design-time-factory).

2. Приложения, предназначенные для ASP.NET Core 2.0, могут использовать EF Core 2.0 без дополнительных зависимостей, кроме сторонних поставщиков базы данных. Тем не менее приложения, предназначенные для предыдущих версий ASP.NET Core должны выполнить обновление до ASP.NET Core 2.0, чтобы использовать EF Core 2.0. Дополнительные сведения об обновлении приложений ASP.NET Core 2.0 см. [документации по ASP.NET Core по этой теме](https://docs.microsoft.com/aspnet/core/migration/1x-to-2x/).

## <a name="breaking-changes"></a>Критические изменения

Мы предприняли возможность значительно улучшать наши существующие API-интерфейсы и поведения в версии 2.0. Существует несколько улучшений, что может потребовать изменения в существующий программный код, несмотря на то, что мы считаем, что для большинства приложений влияние будет низкой, в большинстве случаев, требующих просто перекомпиляции и интерактивная незначительно замените устаревшие API.

### <a name="new-way-of-getting-application-services"></a>Новый способ получения службы приложений

Рекомендуемый шаблон для веб-приложений ASP.NET Core была обновлена для 2.0, в результате которого было передано логику во время разработки, используемые EF Core в версии 1.x. Ранее во время разработки, EF Core будет выполнена попытка вызвать `Startup.ConfigureServices` напрямую, чтобы получить доступ к поставщику службы приложения. В ASP.NET Core 2.0 инициализации конфигурации используется за пределами `Startup` класса. Приложения, использующие EF Core обычно доступ к своей строке подключения из конфигурации, поэтому `Startup` сама по себе больше не достаточно. Если вы обновляете приложение ASP.NET Core 1.x, при использовании средства EF Core может появиться следующая ошибка.

> Конструктор без параметров не найден на «ApplicationContext». Добавьте конструктор без параметров «ApplicationContext» или добавьте реализацию "IDesignTimeDbContextFactory&lt;ApplicationContext&gt;" в сборке «ApplicationContext»

В шаблоне по умолчанию ASP.NET Core 2.0 был добавлен новый обработчик во время разработки. Статический `Program.BuildWebHost` метод, который позволяет EF Core для доступа к поставщику службы приложения во время разработки. Если вы обновляете приложение ASP.NET Core 1.x, необходимо будет обновить вы `Program` класс следующим образом:.

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

### <a name="idbcontextfactory-renamed"></a>IDbContextFactory переименован

Чтобы поддерживать шаблоны для различных приложений и предоставление пользователям больший контроль над тем, как их `DbContext` используется во время разработки, у нас есть, в прошлом, предоставляемых `IDbContextFactory<TContext>` интерфейс. Во время разработки, средства EF Core найдет реализации этого интерфейса в проекте и использовать его для создания `DbContext` объектов.

Этот интерфейс был очень общее имя, которое ввести в заблуждение нескольких пользователей, чтобы попробовать повторно использовать его для других `DbContext`-Создание сценариев. Они были застигнутым врасплох средствам EF попытался использовать свою реализацию во время разработки и вызвало команд, таких как `Update-Database` или `dotnet ef database update` переход на другой.

Для связи семантика строгого во время разработки этот интерфейс, мы переименовали его `IDesignTimeDbContextFactory<TContext>`.

2.0 release `IDbContextFactory<TContext>` по-прежнему существует, но помечен как устаревший.

### <a name="dbcontextfactoryoptions-removed"></a>Удалить DbContextFactoryOptions

Из-за изменения ASP.NET Core 2.0, описанные выше, мы обнаружили, что `DbContextFactoryOptions` был больше не нужны на новом `IDesignTimeDbContextFactory<TContext>` интерфейс. Ниже приведены альтернативные варианты, которые следует использовать вместо этого.

| DbContextFactoryOptions | Альтернатива                                                  |
|:------------------------|:-------------------------------------------------------------|
| ApplicationBasePath     | AppContext.BaseDirectory                                     |
| Атрибут ContentRootPath         | Directory.GetCurrentDirectory()                              |
| EnvironmentName         | Environment.GetEnvironmentVariable("ASPNETCORE_ENVIRONMENT") |

### <a name="design-time-working-directory-changed"></a>Во время разработки рабочий каталог изменен

ASP.NET Core 2.0 изменений также требуется рабочий каталог, используемый `dotnet ef` выравнивается рабочий каталог, используемый в Visual Studio при работе с приложением. Один наблюдаемый побочный эффект этого заключается в SQLite, что имена файлов теперь являются относительно каталога проекта, а не в выходной каталог, как они привыкли быть.

### <a name="ef-core-20-requires-a-20-database-provider"></a>EF Core 2.0 не требуется поставщик 2.0 базы данных

Для EF Core 2.0 мы внесли множество упрощения и улучшениях в поставщики баз данных способ работы. Это означает, что поставщики 1.0.x и 1.1.x, не будут работать с EF Core 2.0.

Поставщики SQL Server и SQLite поставляются группой EF и 2.0 версии будут доступны как часть 2.0 release. Открытый сторонних поставщиков для [SQL Compact](https://github.com/ErikEJ/EntityFramework.SqlServerCompact), [PostgreSQL](https://github.com/npgsql/Npgsql.EntityFrameworkCore.PostgreSQL), и [MySQL](https://github.com/PomeloFoundation/Pomelo.EntityFrameworkCore.MySql) обновляются 2.0. Для всех остальных поставщиков обратитесь в службу модуля записи поставщика.

### <a name="logging-and-diagnostics-events-have-changed"></a>События диагностики и ведения журнала были изменены

Примечание: эти изменения не должны повлиять большая часть кода приложения.

ИД событий для сообщений, отправляемых [ILogger](https://github.com/aspnet/Logging/blob/dev/src/Microsoft.Extensions.Logging.Abstractions/ILogger.cs) были изменены в версии 2.0. Теперь эти идентификаторы событий являются уникальными в рамках всего кода EF Core. Эти сообщения теперь соответствуют стандартному шаблону для структурированного ведения журнала, используемому, например, MVC.

Кроме того, были изменены категории средства ведения журнала. Теперь используется известный набор категорий, доступных через [DbLoggerCategory](https://github.com/aspnet/EntityFramework/blob/dev/src/EFCore/DbLoggerCategory.cs).

[DiagnosticSource](https://github.com/dotnet/corefx/blob/master/src/System.Diagnostics.DiagnosticSource/src/DiagnosticSourceUsersGuide.md) события теперь используют те же имена идентификатор события как соответствующие `ILogger` сообщений. Полезные данные события являются все номинальные типы, производные от [EventData](https://github.com/aspnet/EntityFramework/blob/dev/src/EFCore/Diagnostics/EventData.cs).

Идентификаторы событий, категории и типы полезных данных описаны в [CoreEventId](https://github.com/aspnet/EntityFramework/blob/dev/src/EFCore/Diagnostics/CoreEventId.cs) и [RelationalEventId](https://github.com/aspnet/EntityFramework/blob/dev/src/EFCore.Relational/Diagnostics/RelationalEventId.cs) классы.

Идентификаторы также перенесены из Microsoft.EntityFrameworkCore.Infraestructure Microsoft.EntityFrameworkCore.Diagnostics пространство имен.

### <a name="ef-core-relational-metadata-api-changes"></a>EF Core реляционных метаданных API изменения

EF Core 2.0 теперь будет создавать отдельный объект [IModel](https://github.com/aspnet/EntityFramework/blob/dev/src/EFCore/Metadata/IModel.cs) для каждого из используемых поставщиков. Обычно это является прозрачным для приложения. Это позволило упростить работу с API метаданных более низкого уровня, например, добиться того, что любое обращение к _основным концепциям реляционных метаданных_ всегда осуществляется путем вызова `.Relational` вместо `.SqlServer`, `.Sqlite` и т. д. Например код 1.1.x следующим образом:

``` csharp
var tableName = context.Model.FindEntityType(typeof(User)).SqlServer().TableName;
```

Теперь должен быть записан следующим образом:

``` csharp
var tableName = context.Model.FindEntityType(typeof(User)).Relational().TableName;
```

Вместо того чтобы использовать методы, такие как `ForSqlServerToTable`, методы расширения теперь доступны для записи на основе текущего поставщика используется условный код. Пример:

```C#
modelBuilder.Entity<User>().ToTable(
    Database.IsSqlServer() ? "SqlServerName" : "OtherName");
```

Обратите внимание, что это изменение применяется только к API-интерфейсы и метаданных, который определен для _все_ реляционных поставщиков. API и метаданные остается неизменным, если он определяется только один поставщик. Например, кластеризованные индексы относятся к SQL Sever, поэтому `ForSqlServerIsClustered` и `.SqlServer().IsClustered()` по-прежнему должны использоваться.

### <a name="dont-take-control-of-the-ef-service-provider"></a>Не принимают управления поставщика услуг EF

EF Core использует внутренний `IServiceProvider` (контейнер внедрения зависимостей) для его внутренней реализации. Приложения должны поддерживать EF Core для создания и управления этот поставщик, за исключением в особых случаях. Настоятельно рекомендуется удалить все вызовы `UseInternalServiceProvider`. Если в приложении нужно вызвать `UseInternalServiceProvider`, затем попробуйте [отправьте проблему](https://github.com/aspnet/EntityFramework/Issues) чтобы мы могли изучить другие способы обработки вашего сценария.

Вызов `AddEntityFramework`, `AddEntityFrameworkSqlServer`, и т.д. требуется только в коде приложения `UseInternalServiceProvider` также называется. Удалить все существующие вызовы `AddEntityFramework` или `AddEntityFrameworkSqlServer`т. д. `AddDbContext` следует по-прежнему использовать так же как и раньше.

### <a name="in-memory-databases-must-be-named"></a>Должен иметь имя базы данных в памяти

Глобальные без имени базы данных в памяти была удалена, и вместо этого необходимо присвоить имя всех баз данных в памяти. Пример:

``` csharp
optionsBuilder.UseInMemoryDatabase("MyDatabase");
```

Это создает и использует базу данных с именем «MyDatabase». Если `UseInMemoryDatabase` вызывается снова с тем же именем, то будет использоваться одну и ту же базу данных в памяти, что позволяет совместно использоваться несколькими экземплярами контекста.

### <a name="read-only-api-changes"></a>Изменения API только для чтения

`IsReadOnlyBeforeSave`, `IsReadOnlyAfterSave`, и `IsStoreGeneratedAlways` устарел и заменены [BeforeSaveBehavior](https://github.com/aspnet/EntityFramework/blob/dev/src/EFCore/Metadata/IProperty.cs#L39) и [AfterSaveBehavior](https://github.com/aspnet/EntityFramework/blob/dev/src/EFCore/Metadata/IProperty.cs#L55). Такое поведение, применяются к любому свойству (не только свойства, сформированное хранилищем) и определить, каким образом следует использовать значение свойства при вставке в строку базы данных (`BeforeSaveBehavior`) или при обновлении существующей базы данных строки (`AfterSaveBehavior`).

Свойства помечены как [ValueGenerated.OnAddOrUpdate](https://github.com/aspnet/EntityFramework/blob/dev/src/EFCore/Metadata/ValueGenerated.cs) (например, для вычисляемых столбцов) по умолчанию игнорирует любые текущее значение свойства. Это означает, что сформированное хранилищем значение будет получаться независимо от того задать или изменить на отслеживаемой сущности любое значение. Это можно изменить, задав другой `Before\AfterSaveBehavior`.

### <a name="new-clientsetnull-delete-behavior"></a>Новое поведение удаления ClientSetNull

В предыдущих выпусках [DeleteBehavior.Restrict](https://github.com/aspnet/EntityFramework/blob/dev/src/EFCore/Metadata/DeleteBehavior.cs) бы поведение для сущностей, отслеживаемые по контексту нескольких закрытых сопоставленная `SetNull` семантику. В EF Core 2.0 новый `ClientSetNull` представлена поведение по умолчанию для связи необязательно. Такое поведение влечет `SetNull` семантику для отслеживаемых сущностей и `Restrict` поведение для баз данных, созданных с помощью EF Core. Наш опыт показывает это ожидалось/важнейших поведений отслеживаемых сущностей и базой данных. `DeleteBehavior.Restrict` Теперь учитывается для отслеживаемых сущностей, при установке для связи необязательно.

### <a name="provider-design-time-packages-removed"></a>Удалить пакеты разработки поставщиков

`Microsoft.EntityFrameworkCore.Relational.Design` Пакет был удален. Ее содержимое были объединены в `Microsoft.EntityFrameworkCore.Relational` и `Microsoft.EntityFrameworkCore.Design`.

Это распространяется на поставщика во время разработки пакетов. Эти пакеты (`Microsoft.EntityFrameworkCore.Sqlite.Design`, `Microsoft.EntityFrameworkCore.SqlServer.Design`т. д.) были удалены и их содержимое объединяется в пакеты основным поставщиком.

Чтобы включить `Scaffold-DbContext` или `dotnet ef dbcontext scaffold` в EF Core 2.0, требуется только для ссылки на пакет одного поставщика:

``` xml
<PackageReference Include="Microsoft.EntityFrameworkCore.SqlServer"
    Version="2.0.0" />
<PackageReference Include="Microsoft.EntityFrameworkCore.Tools"
    Version="2.0.0"
    PrivateAssets="All" />
<DotNetCliToolReference Include="Microsoft.EntityFrameworkCore.Tools.DotNet"
    Version="2.0.0" />
```
