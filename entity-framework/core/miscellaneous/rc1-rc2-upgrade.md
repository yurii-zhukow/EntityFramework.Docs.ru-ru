---
title: Обновление до версии-кандидата 2 — EF Core EF Core 1.0 RC1
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 6d75b229-cc79-4d08-88cd-3a1c1b24d88f
uid: core/miscellaneous/rc1-rc2-upgrade
ms.openlocfilehash: 83b98fda5ac9491994b5b3fb333c9951ec01188a
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/27/2018
ms.locfileid: "42996901"
---
# <a name="upgrading-from-ef-core-10-rc1-to-10-rc2"></a>Обновление с версии-кандидата 1 EF Core 1.0 до 1.0 RC2

Эта статья содержит руководство по переносу приложения, созданного с помощью версии-кандидата 1 пакеты для версии-кандидата 2.

## <a name="package-names-and-versions"></a>Имена пакетов и версий

Между RC1 и RC2 изменено с «Entity Framework 7» на «Entity Framework Core». Дополнительные сведения о причинах возникновения изменение [блога, Скотт Хансельман](http://www.hanselman.com/blog/ASPNET5IsDeadIntroducingASPNETCore10AndNETCore10.aspx). Благодаря этому наши имена пакетов изменилось с `EntityFramework.*` для `Microsoft.EntityFrameworkCore.*` и наши версий от `7.0.0-rc1-final` для `1.0.0-rc2-final` (или `1.0.0-preview1-final` инструментов).

**Необходимо полностью удалить пакеты версии-кандидата 1, а затем установить RC2 объектам.** Вот некоторые распространенные пакеты сопоставление.

| Версия-кандидат 1 пакета                                               | Эквивалент RC2                                                       |
|:----------------------------------------------------------|:---------------------------------------------------------------------|
| EntityFramework.MicrosoftSqlServer 7.0.0-rc1-final | Microsoft.EntityFrameworkCore.SqlServer 1.0.0-rc2-final      |
| EntityFramework.SQLite 7.0.0-rc1-final | Microsoft.EntityFrameworkCore.Sqlite 1.0.0-rc2-final      |
| EntityFramework7.Npgsql 3.1.0-rc1-3     | NpgSql.EntityFrameworkCore.Postgres             <to be advised>      |
| EntityFramework.SqlServerCompact35 7.0.0-rc1-final | EntityFrameworkCore.SqlServerCompact35 1.0.0-rc2-final      |
| EntityFramework.SqlServerCompact40 7.0.0-rc1-final | EntityFrameworkCore.SqlServerCompact40 1.0.0-rc2-final      |
| EntityFramework.InMemory 7.0.0-rc1-final | Microsoft.EntityFrameworkCore.InMemory 1.0.0-rc2-final      |
| EntityFramework.IBMDataServer 7.0.0-beta1     | Еще не доступна для версии-кандидате 2                                            |
| EntityFramework.Commands 7.0.0-rc1-final | Microsoft.EntityFrameworkCore.Tools 1.0.0-preview1-final |
| EntityFramework.MicrosoftSqlServer.Design 7.0.0-rc1-final | Microsoft.EntityFrameworkCore.SqlServer.Design 1.0.0-rc2-final      |

## <a name="namespaces"></a>Пространства имен

Вместе с именами пакетов, пространства имен изменилось с `Microsoft.Data.Entity.*` для `Microsoft.EntityFrameworkCore.*`. Можно обработать это изменение с помощью поиска и замены `using Microsoft.Data.Entity` с `using Microsoft.EntityFrameworkCore`.

## <a name="table-naming-convention-changes"></a>Таблицы изменений в соглашениях именования

Значительное изменение функциональных мы воспользовались в версии-кандидата 2 мы хотели использовать имя `DbSet<TEntity>` свойство для заданной сущности в качестве имени таблицы оно сопоставлено, а не только имя класса. Дополнительные сведения об этом изменении в [проблему связанные объявления](https://github.com/aspnet/Announcements/issues/167).

Для существующих приложений RC1, рекомендуется добавить следующий код в начало вашей `OnModelCreating` методом обновления стратегия именования RC1:

``` csharp
foreach (var entity in modelBuilder.Model.GetEntityTypes())
{
    entity.Relational().TableName = entity.DisplayName();
}
```

Если вы хотите внедрить новую стратегию именования, рекомендуется успешно, выполнив остальные этапы обновления и затем удаления кода и создания перехода на применение таблицы переименовывает.

## <a name="adddbcontext--startupcs-changes-aspnet-core-projects-only"></a>AddDbContext / Startup.cs изменяет (только для проектов ASP.NET Core)

В RC1, приходилось добавлять Entity Framework службы поставщику услуг приложения - `Startup.ConfigureServices(...)`:

``` csharp
services.AddEntityFramework()
  .AddSqlServer()
  .AddDbContext<ApplicationDbContext>(options =>
    options.UseSqlServer(Configuration["ConnectionStrings:DefaultConnection"]));
```

В версии-кандидата 2, можно удалить вызовы `AddEntityFramework()`, `AddSqlServer()`и т. д.:

``` csharp
services.AddDbContext<ApplicationDbContext>(options =>
  options.UseSqlServer(Configuration["ConnectionStrings:DefaultConnection"]));
```

Необходимо также добавить конструктор, для вашего производного контекста, который принимает параметры контекста и передает их в базовый конструктор. Это необходимо, поскольку мы удалили некоторую часть scary волшебство, использовали их в фоновом режиме:

``` csharp
public ApplicationDbContext(DbContextOptions<ApplicationDbContext> options)
    : base(options)
{
}
```

## <a name="passing-in-an-iserviceprovider"></a>Передача IServiceProvider

Если у вас есть код версии-кандидата 1, который передает `IServiceProvider` к контексту, это теперь перемещена в `DbContextOptions`, а не параметра отдельных конструктора. Используйте `DbContextOptionsBuilder.UseInternalServiceProvider(...)` для задания поставщика услуг.

### <a name="testing"></a>Тестирование

Наиболее распространенным сценарием для этого было управлять областью InMemory базы данных, при тестировании. См. в разделе обновленный [тестирования](testing/index.md) статье пример делать это с помощью версии-кандидата 2.

### <a name="resolving-internal-services-from-application-service-provider-aspnet-core-projects-only"></a>Разрешение внутренних служб от поставщика службы приложений (только для проектов ASP.NET Core)

Если у вас есть приложение ASP.NET Core и хотите, чтобы EF для разрешения внутренних служб из поставщика служб приложений, является перегрузка метода `AddDbContext` , можно настроить это:

``` csharp
services.AddEntityFrameworkSqlServer()
  .AddDbContext<ApplicationDbContext>((serviceProvider, options) =>
    options.UseSqlServer(Configuration["ConnectionStrings:DefaultConnection"])
           .UseInternalServiceProvider(serviceProvider)); );
```

> [!WARNING]  
> Мы рекомендуем, позволяя EF для внутренних целей управления собственных служб, если нет причин для объединения внутренних служб EF в поставщику службы приложения. Основная причина, что вы можете сделать это является использование поставщику службы приложений для замены службы, которые внутренне использует EF

## <a name="dnx-commands--net-cli-aspnet-core-projects-only"></a>Команды DNX = > .NET CLI (только для проектов ASP.NET Core)

Если вы ранее использовали `dnx ef` команд для проектов ASP.NET 5, они уже перешли `dotnet ef` команды. По-прежнему применяется один и тот же синтаксис команды. Можно использовать `dotnet ef --help` сведения о синтаксисе.

Способ регистрации команд изменилось в версии-кандидата 2, из-за DNX, заменяемое интерфейса командной строки .NET. Команды зарегистрированы в `tools` статьи `project.json`:

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
> Если вы используете Visual Studio, теперь можно использовать консоль диспетчера пакетов для выполнения команд EF для проектов ASP.NET Core (это не поддерживается в версии-кандидата 1). По-прежнему необходимо зарегистрировать команды в `tools` раздел `project.json` это сделать.

## <a name="package-manager-commands-require-powershell-5"></a>Диспетчер пакетов команд требуется PowerShell 5

Если вы используете команды Entity Framework в консоли диспетчера пакетов в Visual Studio, затем необходимо будет убедиться, что у вас установлен PowerShell 5. Это временный требование, которое будет удален в следующем выпуске (см. в разделе [выдавать #5327](https://github.com/aspnet/EntityFramework/issues/5327) подробности).

## <a name="using-imports-in-projectjson"></a>С помощью «imports» в файле project.json

Некоторые из зависимостей EF Core не поддерживают .NET Standard еще. EF Core в проектах .NET Standard и .NET Core может потребоваться добавление «imports» в файл project.json в качестве временного решения.

При добавлении EF, восстановление NuGet будет отображаться это сообщение об ошибке:

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

Обойти это можно вручную импортировать на переносимый профиль «portable net451 + win8». Это заставляет NuGet обрабатывать это двоичные файлы, которые соответствуют это предоставить в качестве платформа, совместимая с .NET Standard, несмотря на то, что они не являются. Несмотря на то, что «portable net451 + win8» не равно 100% совместим с .NET Standard, он совместим достаточно для перехода из библиотеки PCL .NET Standard. Импорты могут быть удалены, когда зависимости платформы EF в конечном счете обновление до .NET Standard.

Несколько платформ могут добавляться к «imports» в синтаксис массива. Других команд импорта может возникнуть необходимость в том случае, если добавить дополнительные библиотеки в проект.

``` json
{
  "frameworks": {
    "netcoreapp1.0": {
      "imports": ["dnxcore50", "portable-net451+win8"]
    }
  }
}
```

См. в разделе [выдавать #5176](https://github.com/aspnet/EntityFramework/issues/5176).
