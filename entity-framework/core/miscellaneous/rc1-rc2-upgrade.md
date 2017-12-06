---
title: "Обновление до версии-кандидата 2 - EF Core EF Core 1.0 RC1"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 6d75b229-cc79-4d08-88cd-3a1c1b24d88f
ms.technology: entity-framework-core
uid: core/miscellaneous/rc1-rc2-upgrade
ms.openlocfilehash: ae5077c30642e3f40f51adee429821978f194460
ms.sourcegitcommit: 5e2d97e731f975cf3405ff3deab2a3c75ad1b969
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/15/2017
---
# <a name="upgrading-from-ef-core-10-rc1-to-10-rc2"></a>Обновление до версии 1.0 RC2 EF Core 1.0 RC1

Эта статья содержит рекомендации по переносу приложения, построенные с помощью версии-кандидата 1 пакеты для версии-кандидата 2.

## <a name="package-names-and-versions"></a>Имена пакетов и версий

Между RC1 и RC2 мы изменено с «Entity Framework 7» на «Entity Framework Core». Вы можете прочитать больше о причинах изменение [эту запись Скотт Хансельман](http://www.hanselman.com/blog/ASPNET5IsDeadIntroducingASPNETCore10AndNETCore10.aspx). Из-за этого изменения наших имена пакетов изменилось с `EntityFramework.*` для `Microsoft.EntityFrameworkCore.*` и наши версий от `7.0.0-rc1-final` для `1.0.0-rc2-final` (или `1.0.0-preview1-final` для средств).

**Необходимо полностью удалить пакеты версии-кандидата 1, а затем установить RC2 областей.** Вот сопоставления для некоторых распространенных пакетов.

| Пакет для версии-кандидата 1                                               | Эквивалент RC2                                                       |
| --------------------------------------------------------- | -------------------------------------------------------------------- |
| EntityFramework.MicrosoftSqlServer 7.0.0-rc1-final | Microsoft.EntityFrameworkCore.SqlServer 1.0.0-rc2-final      |
| EntityFramework.SQLite 7.0.0-rc1-final | Microsoft.EntityFrameworkCore.Sqlite 1.0.0-rc2-final      |
| EntityFramework7.Npgsql 3.1.0-rc1-3     | NpgSql.EntityFrameworkCore.Postgres<to be advised>      |
| EntityFramework.SqlServerCompact35 7.0.0-rc1-final | EntityFrameworkCore.SqlServerCompact35 1.0.0-rc2-final      |
| EntityFramework.SqlServerCompact40 7.0.0-rc1-final | EntityFrameworkCore.SqlServerCompact40 1.0.0-rc2-final      |
| EntityFramework.InMemory 7.0.0-rc1-final | Microsoft.EntityFrameworkCore.InMemory 1.0.0-rc2-final      |
| EntityFramework.IBMDataServer 7.0.0-beta1     | На данный момент недоступно для RC2                                            |
| EntityFramework.Commands 7.0.0-rc1-final | Microsoft.EntityFrameworkCore.Tools 1.0.0-preview1-final |
| EntityFramework.MicrosoftSqlServer.Design 7.0.0-rc1-final | Microsoft.EntityFrameworkCore.SqlServer.Design 1.0.0-rc2-final      |

## <a name="namespaces"></a>Пространства имен

А также имена пакетов, пространства имен изменилось с `Microsoft.Data.Entity.*` для `Microsoft.EntityFrameworkCore.*`. Можно обработать это изменение с поиск и замена `using Microsoft.Data.Entity` с `using Microsoft.EntityFrameworkCore`.

## <a name="table-naming-convention-changes"></a>Именование изменений в таблице

Значительное изменение функциональных мы воспользовались в версии-кандидата 2 было принято использовать имя `DbSet<TEntity>` свойство для заданной сущности в качестве имени таблицы оно сопоставлено, а не только имя класса. Можно прочитать подробнее об этом изменении в [проблему связанные объявления](https://github.com/aspnet/Announcements/issues/167).

Для существующих приложений, RC1, мы рекомендуем, добавив следующий код в начало вашей `OnModelCreating` методом обновления стратегия именования RC1:

``` csharp
foreach (var entity in modelBuilder.Model.GetEntityTypes())
{
    entity.Relational().TableName = entity.DisplayName();
}
```

Если вы хотите использовать новый стратегия именования, будет рекомендуется успешно переименовывает завершение остальные шаги обновления и затем удаления кода и создания миграции для применения в таблице.

## <a name="adddbcontext--startupcs-changes-aspnet-core-projects-only"></a>AddDbContext / файле Startup.cs изменяет (только для проектов ASP.NET Core)

В версии RC1, необходимо добавить службы платформы Entity Framework для поставщика службы приложений — в `Startup.ConfigureServices(...)`:

``` csharp
services.AddEntityFramework()
  .AddSqlServer()
  .AddDbContext<ApplicationDbContext>(options =>
    options.UseSqlServer(Configuration["ConnectionStrings:DefaultConnection"]));
```

В версии-кандидата 2, можно удалить вызовы `AddEntityFramework()`, `AddSqlServer()`и т. п.:

``` csharp
services.AddDbContext<ApplicationDbContext>(options =>
  options.UseSqlServer(Configuration["ConnectionStrings:DefaultConnection"]));
```

Необходимо также добавить конструктор производного контекст, который принимает параметры контекста и передает их в конструкторе базового класса. Это необходимо, поскольку мы удалили некоторые пугающие магическое значение, которое использовали их в фоновом режиме:

``` csharp
public ApplicationDbContext(DbContextOptions<ApplicationDbContext> options)
    : base(options)
{
}
```

## <a name="passing-in-an-iserviceprovider"></a>Передача в IServiceProvider

При наличии версии-кандидата 1 код, который передает `IServiceProvider` в контекст это теперь перемещен `DbContextOptions`, а не параметра отдельных конструктора. Используйте `DbContextOptionsBuilder.UseInternalServiceProvider(...)` для задания поставщика услуг.

### <a name="testing"></a>Тестирование

Наиболее распространенным сценарием для этого можно было управлять областью InMemory базы данных при тестировании. В разделе обновленный [тестирования](testing/index.md) статьи, например, делать это с помощью версии-кандидата 2.

### <a name="resolving-internal-services-from-application-service-provider-aspnet-core-projects-only"></a>Разрешение внутренних служб от поставщика службы приложений (только для проектов ASP.NET Core)

В приложении ASP.NET Core, и требуется EF для разрешения внутренних служб от поставщика службы приложения, существует ли перегрузка `AddDbContext` , можно настроить это:

``` csharp
services.AddEntityFrameworkSqlServer()
  .AddDbContext<ApplicationDbContext>((serviceProvider, options) =>
    options.UseSqlServer(Configuration["ConnectionStrings:DefaultConnection"])
           .UseInternalServiceProvider(serviceProvider)); );
```

> [!WARNING]  
> Мы рекомендуем, позволяя EF для внутренних целей управления собственных служб, если нет причины для объединения внутренних служб EF в поставщику службы приложения. Основной причиной, что хотите сделать это заключается в использовании поставщику службы приложения для замены служб, которые внутренне использует EF

## <a name="dnx-commands--net-cli-aspnet-core-projects-only"></a>Команды DNX = > .NET CLI (только для проектов ASP.NET Core)

Если вы ранее использовали `dnx ef` команд для проектов ASP.NET 5, они теперь были перемещены в `dotnet ef` команд. По-прежнему применяется тот же синтаксис команд. Можно использовать `dotnet ef --help` сведения о синтаксисе.

Способ регистрации команды изменилось RC2, из-за DNX, заменяемое .NET CLI. Команды зарегистрированы в `tools` статьи `project.json`:

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
> Если вы используете Visual Studio, теперь можно использовать консоль диспетчера пакетов для выполнения команд EF для проектов ASP.NET Core (такой синтаксис не поддерживается в версии-кандидата 1). По-прежнему необходимо зарегистрировать команды в `tools` раздел `project.json` это сделать.

## <a name="package-manager-commands-require-powershell-5"></a>Диспетчер пакетов команд требуется PowerShell 5

При использовании команды Entity Framework в консоли диспетчера пакетов в Visual Studio, затем будет необходимо убедиться, что у вас установлен PowerShell 5. Это требование временный, будут удалены в следующей версии (см. [выдавать #5327](https://github.com/aspnet/EntityFramework/issues/5327) Дополнительные сведения).

## <a name="using-imports-in-projectjson"></a>С помощью «imports» в project.json

Некоторые из основных EF зависимостей еще не поддерживает .NET Standard. Основные EF в проектах .NET Standard и .NET Core может потребоваться добавление «Импорт» в файл project.json в качестве временного решения.

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

Это решение подходит в том, чтобы вручную импортировать профиль переносимой «portable net451 + win8». Это заставляет NuGet обрабатывать этот двоичные файлы, которые соответствуют это предоставить в качестве совместимая среда .NET стандарт, даже если они не являются. Несмотря на то, что «portable net451 + win8» не совместима с .NET Standard 100%, он совместим достаточно для перехода со переносимой библиотеке Классов .NET Standard. Операции импорта можно удалить, когда зависимости EF со временем обновления до .NET Standard.

Различных платформ может быть добавлен в «импорт» в синтаксис массива. Другие операции импорта может оказаться необходимым, если добавить дополнительные библиотеки в проект.

``` json
{
  "frameworks": {
    "netcoreapp1.0": {
      "imports": ["dnxcore50", "portable-net451+win8"]
    }
  }
}
```

В разделе [выдавать #5176](https://github.com/aspnet/EntityFramework/issues/5176).
