---
title: Создание DbContext во время разработки — EF Core
description: Стратегии создания DbContext времени разработки с Entity Framework Core
author: bricelam
ms.date: 09/16/2019
uid: core/miscellaneous/cli/dbcontext-creation
ms.openlocfilehash: a9d7da2bb76d60ca63eb0dc189f924df125f0a7d
ms.sourcegitcommit: 0a25c03fa65ae6e0e0e3f66bac48d59eceb96a5a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/14/2020
ms.locfileid: "92062013"
---
# <a name="design-time-dbcontext-creation"></a>Создание DbContext во время разработки

Для некоторых команд EF Core инструментов (например, для команд [миграции][1] ) `DbContext` во время разработки необходимо создать производный экземпляр, чтобы получить сведения о типах сущностей приложения и их сопоставлении со схемой базы данных. В большинстве случаев желательно, `DbContext` чтобы созданный таким образом объект был настроен аналогично тому, как он будет [настроен во время выполнения][2].

Существует несколько способов, с помощью которых средства пытаются создать `DbContext` :

## <a name="from-application-services"></a>Из служб приложений

Если запускаемый проект использует [ASP.NET Core веб-узел][3] или [универсальный узел .NET Core][4], средства пытаются получить объект DbContext от поставщика услуг приложения.

Сначала средства пытаются получить поставщик услуг путем вызова `Program.CreateHostBuilder()` , вызова `Build()` и доступа к `Services` свойству.

[!code-csharp[Main](../../../../samples/core/Miscellaneous/CommandLine/ApplicationService.cs)]

> [!NOTE]
> При создании нового ASP.NET Core приложения этот обработчик включается по умолчанию.

`DbContext`Сам по себе и все зависимости в конструкторе должны быть зарегистрированы в качестве служб в поставщике услуг приложения. Это можно легко сделать, используя [конструктор `DbContext` , который принимает экземпляр `DbContextOptions<TContext>` в качестве аргумента][5] и использует [ `AddDbContext<TContext>` метод][6].

## <a name="using-a-constructor-with-no-parameters"></a>Использование конструктора без параметров

Если DbContext невозможно получить от поставщика службы приложений, средства ищут производный `DbContext` тип внутри проекта. Затем они пытаются создать экземпляр с помощью конструктора без параметров. Это может быть конструктор по умолчанию, если `DbContext` настроен с помощью [`OnConfiguring`][7] метода.

## <a name="from-a-design-time-factory"></a>Из фабрики времени разработки

Вы также можете сообщить средствам, как создать DbContext, реализовав `IDesignTimeDbContextFactory<TContext>` интерфейс: Если класс, реализующий этот интерфейс, находится в том же проекте, что и производный, `DbContext` или в проекте запуска приложения, средства обходят другие способы создания DbContext и используют вместо этого фабрику времени разработки.

[!code-csharp[Main](../../../../samples/core/Miscellaneous/CommandLine/BloggingContextFactory.cs)]

> [!NOTE]
> До Ефкоре 5,0 `args` параметр был неиспользован (см. [эту ошибку][8]).
> Это исправлено в Ефкоре 5,0, а все дополнительные аргументы времени разработки передаются в приложение через этот параметр.

Фабрика времени разработки может быть особенно полезной, если необходимо настроить DbContext по-разному для времени разработки, чем во время выполнения, если `DbContext` конструктор принимает дополнительные параметры, не зарегистрированные в di, если вы не используете функцию Onon или по какой-либо причине не хотите использовать `BuildWebHost` метод в классе ASP.NET Core приложения `Main` .

  [1]: xref:core/managing-schemas/migrations/index
  [2]: xref:core/miscellaneous/configuring-dbcontext
  [3]: /aspnet/core/fundamentals/host/web-host
  [4]: /aspnet/core/fundamentals/host/generic-host
  [5]: xref:core/miscellaneous/configuring-dbcontext#constructor-argument
  [6]: xref:core/miscellaneous/configuring-dbcontext#using-dbcontext-with-dependency-injection
  [7]: xref:core/miscellaneous/configuring-dbcontext#onconfiguring
  [8]: https://github.com/aspnet/EntityFrameworkCore/issues/8332
