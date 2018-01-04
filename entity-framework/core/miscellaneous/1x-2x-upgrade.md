---
title: "Обновление с предыдущих версий до 2 Core EF - EF Core"
author: divega
ms.author: divega
ms.date: 8/13/2017
ms.assetid: 8BD43C8C-63D9-4F3A-B954-7BC518A1B7DB
ms.technology: entity-framework-core
uid: core/miscellaneous/1x-2x-upgrade
ms.openlocfilehash: 380f27c9f00943a2909ec7b876e151572a67dc37
ms.sourcegitcommit: ced2637bf8cc5964c6daa6c7fcfce501bf9ef6e8
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/22/2017
---
# <a name="upgrading-applications-from-previous-versions-to-ef-core-20"></a>Обновление приложений предыдущих версий 2.0 EF Core

## <a name="procedures-common-to-all-applications"></a>Процедуры, общие для всех приложений

Может потребоваться обновление существующего приложения в EF Core 2.0:

1. Обновление приложения, поддерживающего .NET 2.0 стандартная целевой платформы .NET. В разделе [поддерживаемые платформы](../platforms/index.md) для получения дополнительных сведений.

2. Определения поставщика для целевой базы данных, совместимый с EF Core 2.0. В разделе [EF Core 2.0 требуется поставщик 2.0 базы данных](#ef-core-20-requires-a-20-database-provider) ниже.

3. Обновление всех пакетов EF Core (среда выполнения и инструментарий) 2.0. Ссылаться на [Установка основных компонентов EF](../get-started/install/index.md) для получения дополнительных сведений.

4. Внесите изменения необходимый код для компенсации критические изменения. В разделе [критические изменения](#breaking-changes) ниже, в разделе для получения дополнительных сведений.

## <a name="aspnet-core-applications"></a>Приложения ASP.NET Core

1. См. в частности [новый шаблон для инициализации поставщика службы приложения](#new-way-of-getting-application-services) описано ниже.

> [!TIP]  
> По внедрению этого нового шаблона. при обновлении приложений до версии 2.0 настоятельно рекомендуется необходима функций продукта, как Entity Framework Core миграции для работы. Типичная альтернатива — [реализовать *IDesignTimeDbContextFactory\<TContext >*](xref:core/miscellaneous/cli/dbcontext-creation#from-a-design-time-factory).

2. Приложения, предназначенные для ASP.NET Core 2.0, могут использовать EF Core 2.0 без дополнительных зависимостей, кроме сторонних поставщиков базы данных. Тем не менее приложения, предназначенные для предыдущих версий ASP.NET Core нужно обновлять до Core ASP.NET 2.0 для использования EF Core 2.0. Подробнее об обновлении приложений ASP.NET Core 2.0 [ASP.NET основной документацией по этой теме](https://docs.microsoft.com/aspnet/core/migration/1x-to-2x/).

## <a name="breaking-changes"></a>Критические изменения

Мы предприняли возможности, чтобы значительно уточнить наши существующие интерфейсы и поведения в версии 2.0. Есть несколько улучшений, что может потребовать изменения существующего кода приложений, несмотря на то, что мы считаем, что для большинства приложений влияние будет низким, в большинстве случаев требуется просто повторной компиляции и минимальными изменениями интерактивная заменить устаревшие API-интерфейсы.

### <a name="new-way-of-getting-application-services"></a>Новый способ начала работы службы приложений

Рекомендуемый шаблон для веб-приложений ASP.NET Core был обновлен для 2.0, в результате которого было передано EF Core, используемой в 1.x логики во время разработки. Ранее во время разработки EF Core будет выполнена попытка вызвать `Startup.ConfigureServices` напрямую, чтобы получить доступ к поставщику приложения службы. В ASP.NET 2.0 основной конфигурации инициализируется за пределами `Startup` класса. Приложения, использующие EF Core обычно доступа их строка подключения от конфигурации, `Startup` сам по себе не является достаточной мерой. При обновлении приложения ASP.NET Core 1.x, с помощью EF основных средств может появиться следующая ошибка.

> Конструктор без параметров не найден на «ApplicationContext». Добавьте конструктор для «ApplicationContext» или добавьте реализацию "IDesignTimeDbContextFactory&lt;ApplicationContext&gt;" в сборке «ApplicationContext»

Новый обработчик разработки был добавлен в шаблон ASP.NET Core 2.0 по умолчанию. Статический `Program.BuildWebHost` метод, который позволяет EF ядро, что позволяет получить доступ к поставщику службы приложения во время разработки. Если вы обновляете приложение ASP.NET Core 1.x, будет необходимо обновить вы `Program` класс выглядеть следующим образом.

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

### <a name="idbcontextfactory-renamed"></a>Переименовать IDbContextFactory

Чтобы поддерживать шаблоны для различных приложений и предоставлять пользователям больший контроль над тем, как их `DbContext` используется во время разработки, предоставлены, в прошлом `IDbContextFactory<TContext>` интерфейса. Во время разработки EF основных средств будет обнаруживать реализации этого интерфейса в проекте и использовать его для создания `DbContext` объектов.

Этот интерфейс имеет очень стандартное имя, что некоторые заблуждение попробуйте повторно использовать его для других `DbContext`-Создание сценариев. Они были перехвачено отключение защиты при средства EF попытался использовать свою реализацию во время разработки и причиной подобного команды `Update-Database` или `dotnet ef database update` к сбою.

Для связи семантика строгого во время разработки этот интерфейс, чтобы была переименована `IDesignTimeDbContextFactory<TContext>`.

2.0 выпуска `IDbContextFactory<TContext>` по-прежнему существует, но помечен как устаревший.

### <a name="dbcontextfactoryoptions-removed"></a>Удалить DbContextFactoryOptions

Из-за изменения основных ASP.NET 2.0, описанные выше, мы обнаружили, что `DbContextFactoryOptions` был больше не нужны на новом `IDesignTimeDbContextFactory<TContext>` интерфейса. Ниже приведены альтернативные варианты, вместо этого следует использовать.

DbContextFactoryOptions | Альтернатива
--- | ---
ApplicationBasePath | AppContext.BaseDirectory
ContentRootPath | Directory.GetCurrentDirectory()
EnvironmentName | Environment.GetEnvironmentVariable("ASPNETCORE_ENVIRONMENT")

### <a name="design-time-working-directory-changed"></a>Рабочий каталог во время разработки изменен

ASP.NET 2.0 основных изменений также требуется рабочий каталог, используемый `dotnet ef` выравнивается рабочий каталог, используемый в Visual Studio при запуске приложения. Наблюдаемый побочным эффектом того, это является SQLite, что имена файлов теперь являются относительно каталога проекта, а не в выходной каталог, как был.

### <a name="ef-core-20-requires-a-20-database-provider"></a>EF Core 2.0 требует поставщика 2.0 базы данных

EF Core 2.0 мы внесли многие упрощения и усовершенствования в базы данных поставщиков способом работы. Это означает, 1.0.x и 1.1.x поставщики не будет работать с EF Core 2.0.

Поставщики SQL Server и SQLite поставляются командой EF и версии 2.0, будут доступны как часть 2.0 выпуска. Открытая сторонних поставщиков для [SQL Compact](https://github.com/ErikEJ/EntityFramework.SqlServerCompact), [PostgreSQL](https://github.com/npgsql/Npgsql.EntityFrameworkCore.PostgreSQL), и [MySQL](https://github.com/PomeloFoundation/Pomelo.EntityFrameworkCore.MySql) обновляются для 2.0. Для других поставщиков обратитесь в службу модуля записи поставщика.

### <a name="logging-and-diagnostics-events-have-changed"></a>События ведения журнала и диагностики были изменены.

Примечание: эти изменения не должен влиять большая часть кода приложения.

Коды событий для отправки сообщений на [ILogger](https://github.com/aspnet/Logging/blob/dev/src/Microsoft.Extensions.Logging.Abstractions/ILogger.cs) были изменены в версии 2.0. Теперь эти идентификаторы событий являются уникальными в рамках всего кода EF Core. Эти сообщения теперь соответствуют стандартному шаблону для структурированного ведения журнала, используемому, например, MVC.

Кроме того, были изменены категории средства ведения журнала. Теперь используется известный набор категорий, доступных через [DbLoggerCategory](https://github.com/aspnet/EntityFramework/blob/dev/src/EFCore/DbLoggerCategory.cs).

[DiagnosticSource](https://github.com/dotnet/corefx/blob/master/src/System.Diagnostics.DiagnosticSource/src/DiagnosticSourceUsersGuide.md) события теперь используют те же имена идентификатор события как соответствующие `ILogger` сообщений. Полезные данные события будут все номинальные типы, производные от [EventData](https://github.com/aspnet/EntityFramework/blob/dev/src/EFCore/Diagnostics/EventData.cs).

Идентификаторы событий, категории и типы полезных данных описаны в [CoreEventId](https://github.com/aspnet/EntityFramework/blob/dev/src/EFCore/Diagnostics/CoreEventId.cs) и [RelationalEventId](https://github.com/aspnet/EntityFramework/blob/dev/src/EFCore.Relational/Diagnostics/RelationalEventId.cs) классы.

Идентификаторы также были перемещены из Microsoft.EntityFrameworkCore.Infraestructure Microsoft.EntityFrameworkCore.Diagnostics пространство имен.

### <a name="ef-core-relational-metadata-api-changes"></a>Изменения реляционных метаданных API Core EF

EF Core 2.0 теперь будет создавать отдельный объект [IModel](https://github.com/aspnet/EntityFramework/blob/dev/src/EFCore/Metadata/IModel.cs) для каждого из используемых поставщиков. Обычно это является прозрачным для приложения. Это позволило упростить работу с API метаданных более низкого уровня, например, добиться того, что любое обращение к _основным концепциям реляционных метаданных_ всегда осуществляется путем вызова `.Relational` вместо `.SqlServer`, `.Sqlite` и т. д. Например код 1.1.x следующим образом:

``` csharp
var tableName = context.Model.FindEntityType(typeof(User)).SqlServer().TableName;
```

Теперь должен быть записан следующим образом:

``` csharp
var tableName = context.Model.FindEntityType(typeof(User)).Relational().TableName;
```

Вместо того чтобы использовать методы, такие как `ForSqlServerToTable`, методы расширения теперь доступны для записи условного кода на основании текущего поставщика используется. Пример:

```C#
modelBuilder.Entity<User>().ToTable(
    Database.IsSqlServer() ? "SqlServerName" : "OtherName");
```

Обратите внимание, что это изменение применяется только к API-интерфейсов и метаданные, определенные для _все_ реляционных поставщиков. API и метаданных не меняется при он определяется только один поставщик. Например, кластеризованные индексы относятся к SQL Sever, поэтому `ForSqlServerIsClustered` и `.SqlServer().IsClustered()` необходимо использовать.

### <a name="dont-take-control-of-the-ef-service-provider"></a>Не берет на себя управление EF поставщика услуг

EF Core использует внутреннюю `IServiceProvider` (т. е. контейнер внедрения зависимостей) для своей внутренней реализации. Приложения следует разрешать EF основных компонентов для создания и управления этим поставщиком, за исключением того, в особых случаях. Настоятельно рекомендуется удалить все вызовы `UseInternalServiceProvider`. Если приложению требуется вызывать `UseInternalServiceProvider`, затем попробуйте [регистрации проблемы](https://github.com/aspnet/EntityFramework/Issues) так мы можно исследовать другие способы обработки вашего сценария.

Вызов `AddEntityFramework`, `AddEntityFrameworkSqlServer`, т. д. требуется только в коде приложения `UseInternalServiceProvider` также называется. Удалите все существующие вызовы `AddEntityFramework` или `AddEntityFrameworkSqlServer`, т. д. `AddDbContext` следует по-прежнему использовать так же как и раньше.

### <a name="in-memory-databases-must-be-named"></a>Должен иметь имя базы данных в памяти

Глобальные без имени базы данных в памяти было удалено, а вместо этого должны иметь имена всех баз данных в памяти. Пример:

``` csharp
optionsBuilder.UseInMemoryDatabase("MyDatabase");
```

Это создает и использует базу данных с именем «MyDatabase». Если `UseInMemoryDatabase` будет вызван повторно с тем же именем, то будет использоваться одну и ту же базу данных в памяти, что позволяет совместно использоваться несколькими экземплярами контекста.

### <a name="read-only-api-changes"></a>Изменения API-Интерфейс только для чтения

`IsReadOnlyBeforeSave`, `IsReadOnlyAferSave`, и `IsStoreGeneratedAlways` были устарел и заменен [BeforeSaveBehavior](https://github.com/aspnet/EntityFramework/blob/dev/src/EFCore/Metadata/IProperty.cs#L39) и [AfterSaveBehavior](https://github.com/aspnet/EntityFramework/blob/dev/src/EFCore/Metadata/IProperty.cs#L55). Эти поведения применяются к любому свойству (не только свойства, формируемым хранилищем) и определите, каким образом следует использовать значение свойства при вставке в строку базы данных (`BeforeSaveBehavior`) или при обновлении существующей базы данных строки (`AfterSaveBehavior`).

Свойства помечены как [ValueGenerated.OnAddOrUpdate](https://github.com/aspnet/EntityFramework/blob/dev/src/EFCore/Metadata/ValueGenerated.cs) (например, для вычисляемых столбцов) по умолчанию игнорирует любые текущее значение для свойства. Это означает, что сформированное хранилищем значение будет получаться независимо от того задать или изменить на Отслеживаемая сущность любое значение. Это можно изменить, задав другой `Before\AfterSaveBehavior`.

### <a name="new-clientsetnull-delete-behavior"></a>Новое поведение удаления ClientSetNull

В предыдущих выпусках [DeleteBehavior.Restrict](https://github.com/aspnet/EntityFramework/blob/dev/src/EFCore/Metadata/DeleteBehavior.cs) бы поведение для сущностей отслеживаются контекстом нескольких закрытых сопоставленная `SetNull` семантику. В версии 2.0 основные EF новый `ClientSetNull` представлена поведение по умолчанию для связей, необязательно. Такое поведение влечет `SetNull` семантику для отслеживаемых объектов и `Restrict` поведение для баз данных, созданных с помощью EF Core. Опыт это ожидалось или чаще поведения для отслеживаемых объектов и базы данных. `DeleteBehavior.Restrict`Теперь учитывается для отслеживаемых объектов, при установке для связи необязательно.

### <a name="provider-design-time-packages-removed"></a>Удалить пакетов во время разработки поставщика

`Microsoft.EntityFrameworkCore.Relational.Design` Пакет был удален. Ее содержимое был объединен в `Microsoft.EntityFrameworkCore.Relational` и `Microsoft.EntityFrameworkCore.Design`.

Это распространяется на пакетов во время разработки поставщика. Эти пакеты (`Microsoft.EntityFrameworkCore.Sqlite.Design`, `Microsoft.EntityFrameworkCore.SqlServer.Design`, т. д.) были удалены и их содержимое, объединяются в пакеты основного поставщика.

Чтобы включить `Scaffold-DbContext` или `dotnet ef dbcontext scaffold` в EF Core 2.0, достаточно для ссылки на пакет одного поставщика:

``` xml
<PackageReference Include="Microsoft.EntityFrameworkCore.SqlServer"
    Version="2.0.0" />
<PackageReference Include="Microsoft.EntityFrameworkCore.Tools"
    Version="2.0.0"
    PrivateAssets="All" />
<DotNetCliToolReference Include="Microsoft.EntityFrameworkCore.Tools.DotNet"
    Version="2.0.0" />
```
