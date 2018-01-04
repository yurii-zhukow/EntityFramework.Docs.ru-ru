---
title: "Создание DbContext во время разработки - EF Core"
author: bricelam
ms.author: bricelam
ms.date: 10/27/2017
ms.technology: entity-framework-core
uid: core/miscellaneous/cli/dbcontext-creation
ms.openlocfilehash: a899c474cc45437bff7c82ce5bddeb915b15c3b0
ms.sourcegitcommit: ced2637bf8cc5964c6daa6c7fcfce501bf9ef6e8
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/22/2017
---
<a name="design-time-dbcontext-creation"></a>Создание DbContext во время разработки
==============================
Некоторые команды EF основные инструменты (например, [миграций] [ 1] команд) требуют производный `DbContext` экземпляра во время разработки для сбора подробных сведений о приложении типы сущностей и их сопоставлении схемы базы данных. В большинстве случаев желательно, `DbContext` тем самым созданный настраивается так же, как в том, как было бы [настроен во время выполнения][2].

Существует несколько способов, попытайтесь создать средства `DbContext`:

<a name="from-application-services"></a>Из служб приложений
-------------------------
Если автоматически запускаемый проект приложения ASP.NET Core, средства попытайтесь получить объект DbContext приложения поставщика услуг.

Средство сначала пытаются получить доступ к службе с помощью вызова `Program.BuildWebHost()` и доступ к `IWebHost.Services` свойство.

> [!NOTE]
> При создании нового приложения ASP.NET Core 2.0, этот обработчик включается по умолчанию. В предыдущих версиях EF Core и ASP.NET Core, попытка вызова средства `Startup.ConfigureServices` напрямую для получения поставщика службы приложения, но этот шаблон больше не работает правильно в приложениях ASP.NET Core 2.0. Если вы обновляете приложение ASP.NET Core 1.x 2.0, вы можете [изменить вашей `Program` класс, чтобы использовать этот новый подход][3].

`DbContext` Себя и все зависимости в конструкторе должны быть зарегистрированы как службы в приложение поставщика услуг. Это легко сделать, задав [конструктор `DbContext` , принимающий экземпляр `DbContextOptions<TContext>` как аргумент] [ 4] и с помощью [ `AddDbContext<TContext>` метод] [5].

<a name="using-a-constructor-with-no-parameters"></a>С помощью конструктора без параметров
--------------------------------------
Если класс DbContext нельзя получить от поставщика службы приложений, средства поиска производного `DbContext` типа внутри проекта. Затем они пытаются создать экземпляр с помощью конструктора без параметров. Это может быть конструктор по умолчанию, если `DbContext` настраивается с помощью [ `OnConfiguring` ] [ 6] метод.

<a name="from-a-design-time-factory"></a>Из фабрики во время разработки
--------------------------
Можно также сказать, средства Создание вашей DbContext путем реализации `IDesignTimeDbContextFactory<TContext>` интерфейса: Если класс, реализующий этот интерфейс находится в любом проекте, производным `DbContext` или в проекте при запуске приложения, обходить средства другие способы вместо создания DbContext и использование фабрики во время разработки.

``` csharp
using Microsoft.EntityFrameworkCore;
using Microsoft.EntityFrameworkCore.Infrastructure;

namespace MyProject
{
    public class BloggingContextFactory : IDesignTimeDbContextFactory<BloggingContext>
    {
        public BloggingContext CreateDbContext(string[] args)
        {
            var optionsBuilder = new DbContextOptionsBuilder<BloggingContext>();
            optionsBuilder.UseSqlite("Data Source=blog.db");

            return new BloggingContext(optionsBuilder.Options);
        }
    }
}
```

> [!NOTE]
> `args` Параметр не в настоящее время используется. Отсутствует [проблему] [ 7] возможность указывать аргументы во время разработки с помощью средств отслеживания.

Фабрика разработки может быть особенно полезно, если необходимо настроить класс DbContext по-разному для времени разработки, чем во время выполнения, если `DbContext` конструктор принимает дополнительные параметры не зарегистрированы в DI, если вы не используете DI вообще или, если для некоторых причины, не должны иметь `BuildWebHost` метода в приложении ASP.NET Core  
Класс `Main`.

  [1]: xref:core/managing-schemas/migrations/index
  [2]: xref:core/miscellaneous/configuring-dbcontext
  [3]: https://docs.microsoft.com/aspnet/core/migration/1x-to-2x/#update-main-method-in-programcs
  [4]: xref:core/miscellaneous/configuring-dbcontext#constructor-argument
  [5]: xref:core/miscellaneous/configuring-dbcontext#using-dbcontext-with-dependency-injection
  [6]: xref:core/miscellaneous/configuring-dbcontext#onconfiguring
  [7]: https://github.com/aspnet/EntityFrameworkCore/issues/8332
