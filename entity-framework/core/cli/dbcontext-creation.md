---
title: Создание DbContext во время разработки — EF Core
description: Стратегии создания DbContext времени разработки с Entity Framework Core
author: bricelam
ms.date: 10/27/2020
uid: core/cli/dbcontext-creation
ms.openlocfilehash: 1a2c0e853047cf4ab54a320d0bef413a114e90bc
ms.sourcegitcommit: 704240349e18b6404e5a809f5b7c9d365b152e2e
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/16/2021
ms.locfileid: "100543410"
---
# <a name="design-time-dbcontext-creation"></a>Создание DbContext во время разработки

Для некоторых команд EF Core инструментов (например, для команд [миграции][1] ) `DbContext` во время разработки необходимо создать производный экземпляр, чтобы получить сведения о типах сущностей приложения и их сопоставлении со схемой базы данных. В большинстве случаев желательно, `DbContext` чтобы созданный таким образом объект был настроен аналогично тому, как он будет [настроен во время выполнения][2].

Существует несколько способов, с помощью которых средства пытаются создать `DbContext` :

## <a name="from-application-services"></a>Из служб приложений

Если запускаемый проект использует [ASP.NET Core веб-узел][3] или [универсальный узел .NET Core][4], средства пытаются получить объект DbContext от поставщика услуг приложения.

Сначала средства пытаются получить поставщик услуг путем вызова `Program.CreateHostBuilder()` , вызова `Build()` и доступа к `Services` свойству.

[!code-csharp[Main](../../../samples/core/Miscellaneous/CommandLine/ApplicationService.cs#ApplicationService)]

> [!NOTE]
> При создании нового ASP.NET Core приложения этот обработчик включается по умолчанию.

`DbContext`Сам по себе и все зависимости в конструкторе должны быть зарегистрированы в качестве служб в поставщике услуг приложения. Это можно легко сделать, используя [конструктор `DbContext` , который принимает экземпляр `DbContextOptions<TContext>` в качестве аргумента][5] и использует [ `AddDbContext<TContext>` метод][6].

## <a name="using-a-constructor-with-no-parameters"></a>Использование конструктора без параметров

Если DbContext невозможно получить от поставщика службы приложений, средства ищут производный `DbContext` тип внутри проекта. Затем они пытаются создать экземпляр с помощью конструктора без параметров. Это может быть конструктор по умолчанию, если `DbContext` настроен с помощью [`OnConfiguring`][7] метода.

## <a name="from-a-design-time-factory"></a>Из фабрики времени разработки

Вы также можете сообщить средствам, как создать DbContext, реализовав `IDesignTimeDbContextFactory<TContext>` интерфейс: Если класс, реализующий этот интерфейс, находится в том же проекте, что и производный, `DbContext` или в проекте запуска приложения, средства обходят другие способы создания DbContext и используют вместо этого фабрику времени разработки.

[!code-csharp[Main](../../../samples/core/Miscellaneous/CommandLine/BloggingContextFactory.cs#BloggingContextFactory)]

> [!NOTE]
> До Ефкоре 5,0 `args` параметр был неиспользован (см. [эту ошибку][8]).
> Это исправлено в Ефкоре 5,0, а все дополнительные аргументы времени разработки передаются в приложение через этот параметр.

Фабрика времени разработки может быть особенно полезной, если необходимо настроить `DbContext` разное время разработки, чем во время выполнения, если `DbContext` конструктор принимает дополнительные параметры, не зарегистрированные на сайте di, если вы не используете функцию Onon или по какой-либо причине не хотите использовать `CreateHostBuilder` метод в классе ASP.NET Core приложения `Main` .

## <a name="args"></a>Args

Оба <xref:Microsoft.EntityFrameworkCore.Design.IDesignTimeDbContextFactory%601.CreateDbContext%2A?displayProperty=nameWithType> `Program.CreateHostBuilder` аргумента и принимают аргументы командной строки.

Начиная с EF Core 5,0, можно указать следующие аргументы в средствах:

### <a name="net-core-cli"></a>[Интерфейс командной строки .NET Core](#tab/dotnet-core-cli)

```dotnetcli
dotnet ef database update -- --environment Production
```

`--`Токен направляет `dotnet ef` все, что следует за аргументом, и не пытается проанализировать их как параметры. Все дополнительные аргументы, не используемые, `dotnet ef` пересылаются в приложение.

### <a name="visual-studio"></a>[Visual Studio](#tab/vs)

```powershell
Update-Database -Args '--environment Production'
```

***

  [1]: xref:core/managing-schemas/migrations/index
  [2]: xref:core/dbcontext-configuration/index
  [3]: /aspnet/core/fundamentals/host/web-host
  [4]: /aspnet/core/fundamentals/host/generic-host
  [5]: xref:core/dbcontext-configuration/index#constructor-argument
  [6]: xref:core/dbcontext-configuration/index#using-dbcontext-with-dependency-injection
  [7]: xref:core/dbcontext-configuration/index#onconfiguring
  [8]: https://github.com/dotnet/efcore/issues/8332
