---
title: Обновление с EF Core 1,0 RC1 до RC2-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 6d75b229-cc79-4d08-88cd-3a1c1b24d88f
uid: core/miscellaneous/rc1-rc2-upgrade
ms.openlocfilehash: 887b7cd539b9c0f5a680398f5039757420228710
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/06/2020
ms.locfileid: "78414113"
---
# <a name="upgrading-from-ef-core-10-rc1-to-10-rc2"></a>Обновление с EF Core 1,0 RC1 до 1,0 RC2

В этой статье приводятся рекомендации по перемещению приложения, созданного с помощью пакетов RC1, в RC2.

## <a name="package-names-and-versions"></a>Имена и версии пакетов

Между RC1 и RC2 мы изменили с "Entity Framework 7" на "Entity Framework Core". Дополнительные сведения о причинах изменений в этой записи можно узнать [по Скотт Hanselman](https://www.hanselman.com/blog/ASPNET5IsDeadIntroducingASPNETCore10AndNETCore10.aspx). Из-за этого изменения имена пакетов изменились с `EntityFramework.*` на `Microsoft.EntityFrameworkCore.*` и наши версии с `7.0.0-rc1-final` на `1.0.0-rc2-final` (или `1.0.0-preview1-final` для средств).

**Необходимо полностью удалить пакеты RC1, а затем установить RC2.** Ниже приведено сопоставление для некоторых распространенных пакетов.

| Пакет RC1                                               | Эквивалент RC2                                                       |
|:----------------------------------------------------------|:---------------------------------------------------------------------|
| EntityFramework. Микрософтсклсервер 7.0.0-RC1-Final | Microsoft. EntityFrameworkCore. SqlServer 1.0.0-RC2-Final      |
| EntityFramework. SQLite 7.0.0-RC1-Final | Microsoft. EntityFrameworkCore. SQLite 1.0.0-RC2-Final      |
| EntityFramework7. Npgsql 3.1.0-RC1-3     | NpgSql. EntityFrameworkCore. postgres <to be advised>      |
| EntityFramework. SqlServerCompact35 7.0.0-RC1-Final | EntityFrameworkCore. SqlServerCompact35 1.0.0-RC2-Final      |
| EntityFramework. SqlServerCompact40 7.0.0-RC1-Final | EntityFrameworkCore. SqlServerCompact40 1.0.0-RC2-Final      |
| EntityFramework. 7.0.0-RC1-Final | Microsoft. EntityFrameworkCore. Memory 1.0.0 — RC2-Final      |
| EntityFramework. Ибмдатасервер 7.0.0 — Beta1     | Еще не доступно для RC2                                            |
| EntityFramework. Commands 7.0.0-RC1-Final | Microsoft. EntityFrameworkCore. Tools 1.0.0-preview1:-Final |
| EntityFramework. Микрософтсклсервер. Design 7.0.0 — RC1-Final | Microsoft. EntityFrameworkCore. SqlServer. Design 1.0.0 — RC2-Final      |

## <a name="namespaces"></a>Пространства имен

Наряду с именами пакетов пространства имен изменились с `Microsoft.Data.Entity.*` на `Microsoft.EntityFrameworkCore.*`. Это изменение можно решить с помощью поиска и замены `using Microsoft.Data.Entity` с `using Microsoft.EntityFrameworkCore`.

## <a name="table-naming-convention-changes"></a>Изменения в соглашении об именовании таблиц

Значительным функциональным изменением, которое мы использовали в RC2, было использование имени свойства `DbSet<TEntity>` для данной сущности в качестве имени таблицы, к которой она сопоставляется, а не просто имени класса. Дополнительные сведения об этом изменении см. в описании [проблемы, связанной с объявлением](https://github.com/aspnet/Announcements/issues/167).

Для существующих приложений RC1 рекомендуется добавить следующий код в начало метода `OnModelCreating` для сохранения стратегии именования RC1:

``` csharp
foreach (var entity in modelBuilder.Model.GetEntityTypes())
{
    entity.Relational().TableName = entity.DisplayName();
}
```

Если вы хотите применить новую стратегию именования, мы рекомендуем успешно выполнить оставшиеся действия по обновлению, а затем удалить код и создать миграцию для применения переименований таблиц.

## <a name="adddbcontext--startupcs-changes-aspnet-core-projects-only"></a>Изменения AddDbContext и Startup.cs (только для проектов ASP.NET Core)

В RC1 необходимо было добавить Entity Framework служб в `Startup.ConfigureServices(...)`поставщика службы приложений:

``` csharp
services.AddEntityFramework()
  .AddSqlServer()
  .AddDbContext<ApplicationDbContext>(options =>
    options.UseSqlServer(Configuration["ConnectionStrings:DefaultConnection"]));
```

В RC2 можно удалить вызовы `AddEntityFramework()`, `AddSqlServer()`и т. д.:

``` csharp
services.AddDbContext<ApplicationDbContext>(options =>
  options.UseSqlServer(Configuration["ConnectionStrings:DefaultConnection"]));
```

Также необходимо добавить конструктор в производный контекст, который принимает параметры контекста и передает их в базовый конструктор. Это необходимо, поскольку мы удалили некоторые из самых страшного волшебства, снукк их в фоновом режиме:

``` csharp
public ApplicationDbContext(DbContextOptions<ApplicationDbContext> options)
    : base(options)
{
}
```

## <a name="passing-in-an-iserviceprovider"></a>Передача IServiceProvider

Если у вас есть код RC1, который передает `IServiceProvider` в контекст, теперь он перемещается в `DbContextOptions`, а не в отдельный параметр конструктора. Для задания поставщика услуг используйте `DbContextOptionsBuilder.UseInternalServiceProvider(...)`.

### <a name="testing"></a>Тестирование

Наиболее распространенным сценарием этого является управление областью базы данных в памяти при тестировании. Пример этого с помощью RC2 см. в обновленной статье о [тестировании](testing/index.md) .

### <a name="resolving-internal-services-from-application-service-provider-aspnet-core-projects-only"></a>Разрешение внутренних служб от поставщика службы приложений (только ASP.NET Core проектов)

Если у вас есть ASP.NET Coreное приложение и требуется, чтобы EF разрешил внутренние службы от поставщика службы приложений, существует перегрузка `AddDbContext`, которая позволяет настроить это:

``` csharp
services.AddEntityFrameworkSqlServer()
  .AddDbContext<ApplicationDbContext>((serviceProvider, options) =>
    options.UseSqlServer(Configuration["ConnectionStrings:DefaultConnection"])
           .UseInternalServiceProvider(serviceProvider));
```

> [!WARNING]  
> Рекомендуется разрешить EF управлять собственными службами, если нет причины объединить внутренние службы EF в поставщик службы приложений. Основная причина, по которой это можно сделать, — использование поставщика службы приложений для замены служб, используемых EF внутри.

## <a name="dnx-commands--net-cli-aspnet-core-projects-only"></a>Команды DNX = > .NET CLI (только проекты ASP.NET Core)

Если ранее вы использовали `dnx ef` команды для проектов ASP.NET 5, теперь они перемещены в `dotnet ef` команды. По-прежнему применяется тот же синтаксис команды. Для получения сведений о синтаксисе можно использовать `dotnet ef --help`.

Способ регистрации команд изменился в RC2, так как DNX заменяется интерфейсом командной строки .NET. Команды теперь регистрируются в `tools` разделе `project.json`.

``` json
"tools": {
  "Microsoft.EntityFrameworkCore.Tools": {
    "version": "1.0.0-preview1-final",
    "imports": [
      "portable-net45+win8+dnxcore50",
      "portable-net45+win8"
    ]
  }
}
```

> [!TIP]  
> Если вы используете Visual Studio, теперь можно использовать консоль диспетчера пакетов для выполнения команд EF для ASP.NET Core проектов (это не поддерживается в RC1). Для этого по-прежнему необходимо зарегистрировать команды в `tools` разделе `project.json`.

## <a name="package-manager-commands-require-powershell-5"></a>Для команд диспетчера пакетов требуется PowerShell 5

При использовании команд Entity Framework в консоли диспетчера пакетов в Visual Studio необходимо убедиться, что установлен PowerShell 5. Это временное требование, которое будет удалено в следующем выпуске (Дополнительные сведения см. в [#5327 выпуска](https://github.com/aspnet/EntityFramework/issues/5327) ).

## <a name="using-imports-in-projectjson"></a>Использование "Imports" в Project. JSON

Некоторые зависимости EF Core не поддерживают .NET Standard. Для EF Core в .NET Standard и проектах .NET Core может потребоваться добавить "Imports" в Project. JSON в качестве временного решения.

При добавлении EF в Restore NuGet отображается следующее сообщение об ошибке:

``` Console
Package Ix-Async 1.2.5 is not compatible with netcoreapp1.0 (.NETCoreApp,Version=v1.0). Package Ix-Async 1.2.5 supports:
  - net40 (.NETFramework,Version=v4.0)
  - net45 (.NETFramework,Version=v4.5)
  - portable-net45+win8+wp8 (.NETPortable,Version=v0.0,Profile=Profile78)
Package Remotion.Linq 2.0.2 is not compatible with netcoreapp1.0 (.NETCoreApp,Version=v1.0). Package Remotion.Linq 2.0.2 supports:
  - net35 (.NETFramework,Version=v3.5)
  - net40 (.NETFramework,Version=v4.0)
  - net45 (.NETFramework,Version=v4.5)
  - portable-net45+win8+wp8+wpa81 (.NETPortable,Version=v0.0,Profile=Profile259)
```

Чтобы устранить эту проблему, вручную импортируйте переносимый профиль "Portable-net451 + Win8". Это заставляет NuGet обрабатывать эти двоичные файлы, соответствующие этой платформе, в качестве совместимой платформы с .NET Standard, даже если они не являются. Несмотря на то, что "Portable-net451 + Win8 100" несовместим с .NET Standard, он достаточно совместим для перехода с PCL на .NET Standard. Импорт может быть удален, когда зависимости EF в конечном итоге обновляются до .NET Standard.

В синтаксисе массива можно добавить несколько платформ в "Imports". Если в проект добавляются дополнительные библиотеки, могут потребоваться другие операции импорта.

``` json
{
  "frameworks": {
    "netcoreapp1.0": {
      "imports": ["dnxcore50", "portable-net451+win8"]
    }
  }
}
```

См. раздел [Issue #5176](https://github.com/aspnet/EntityFramework/issues/5176).
