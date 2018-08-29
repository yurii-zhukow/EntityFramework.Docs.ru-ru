---
title: Создание DbContext во время разработки — EF Core
author: bricelam
ms.author: bricelam
ms.date: 10/27/2017
uid: core/miscellaneous/cli/dbcontext-creation
ms.openlocfilehash: 66fec7605b6ac2da0af1e801f8a1dca0789aea35
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/27/2018
ms.locfileid: "42993722"
---
<a name="design-time-dbcontext-creation"></a>Создание DbContext во время разработки
==============================
Некоторые команды Инструменты EF Core (например, [миграций] [ 1] команды) требуют производный `DbContext` экземпляра во время разработки для сбора сведений о приложении типы сущностей и их сопоставлении схемы базы данных. В большинстве случаев желательно, `DbContext` тем самым созданный настраивается в том, как было бы так же, как [настроен во время выполнения][2].

Существует несколько способов, попробуйте создать средства `DbContext`:

<a name="from-application-services"></a>Из служб приложений
-------------------------
Если ваш запускаемый проект приложения ASP.NET Core, инструменты попытаться получить объект DbContext у поставщика услуг приложения.

Средства сначала пытаются получить доступ к службе, вызвав `Program.BuildWebHost()` и доступ к `IWebHost.Services` свойство.

> [!NOTE]
> При создании нового приложения ASP.NET Core 2.0, этот обработчик включается по умолчанию. В предыдущих версиях EF Core и ASP.NET Core, инструменты попытке вызвать `Startup.ConfigureServices` напрямую, чтобы получить поставщик службы приложения, но этот шаблон больше не работает правильно в приложениях ASP.NET Core 2.0. Если вы обновляете приложение ASP.NET Core 1.x на 2.0, вы можете [изменить ваш `Program` класс стоит следовать этому шаблону новый][3].

`DbContext` Сам и все зависимости в своем конструкторе должны быть зарегистрированы как службы в поставщике служб приложения. Это легко достигается за счет [конструктор в `DbContext` , принимающий экземпляр `DbContextOptions<TContext>` как аргумент] [ 4] и с помощью [ `AddDbContext<TContext>` метод] [5].

<a name="using-a-constructor-with-no-parameters"></a>С помощью конструктора без параметров
--------------------------------------
DbContext не может быть получен от поставщика службы приложения, средства поиска производные `DbContext` тип внутри проекта. Затем они пытаются создать экземпляр с помощью конструктора без параметров. Это может быть конструктор по умолчанию, если `DbContext` настраивается с помощью [ `OnConfiguring` ] [ 6] метод.

<a name="from-a-design-time-factory"></a>Из фабрики во время разработки
--------------------------
Вы также можете перенаправить средства Создание DbContext, реализовав `IDesignTimeDbContextFactory<TContext>` интерфейса: Если класс, реализующий этот интерфейс находится в любом проекте, производные `DbContext` или в проекте запуска приложения, средства обхода другие способы вместо создания DbContext и использование разработки фабрики.

``` csharp
using Microsoft.EntityFrameworkCore;
using Microsoft.EntityFrameworkCore.Design;
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
> `args` Параметра, используемого в данный момент. Существует [проблема] [ 7] отслеживания возможность указывать аргументы во время разработки с помощью средств.

Во время разработки фабрика может быть особенно полезно в том случае, если вам нужно по-разному настроить DbContext для времени разработки, чем во время выполнения, если `DbContext` конструктор принимает дополнительные параметры не зарегистрированы в функции внедрения Зависимостей, если вы не используете DI вообще или, если для некоторых -либо причине вы не хотите иметь `BuildWebHost` метод в приложении ASP.NET Core `Main` класса.

  [1]: xref:core/managing-schemas/migrations/index
  [2]: xref:core/miscellaneous/configuring-dbcontext
  [3]: https://docs.microsoft.com/aspnet/core/migration/1x-to-2x/#update-main-method-in-programcs
  [4]: xref:core/miscellaneous/configuring-dbcontext#constructor-argument
  [5]: xref:core/miscellaneous/configuring-dbcontext#using-dbcontext-with-dependency-injection
  [6]: xref:core/miscellaneous/configuring-dbcontext#onconfiguring
  [7]: https://github.com/aspnet/EntityFrameworkCore/issues/8332
