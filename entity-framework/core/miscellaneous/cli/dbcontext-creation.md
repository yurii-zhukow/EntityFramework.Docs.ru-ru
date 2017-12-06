---
title: "Создание DbContext во время разработки - EF Core"
author: bricelam
ms.author: bricelam
ms.date: 10/27/2017
ms.technology: entity-framework-core
ms.openlocfilehash: 5fcd9e362d76127e7acadd9e552ef3ac90967a37
ms.sourcegitcommit: 5e2d97e731f975cf3405ff3deab2a3c75ad1b969
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/15/2017
---
<a name="design-time-dbcontext-creation"></a>Создание DbContext во время разработки
==============================
Некоторые средства EF команд требуется экземпляр DbContext, созданные на этапе разработки времени (например, при выполнении команды миграции). Существует несколько способов, попытайтесь создать ее средств.

<a name="from-application-services"></a>Из служб приложений
-------------------------
Если автоматически запускаемый проект приложения ASP.NET Core, средства попытайтесь получить объект DbContext приложения поставщика услуг. Они получают путем вызова `Program.BuildWebHost()` и доступ к `IWebHost.Services` свойство. Любой DbContext зарегистрировать с помощью `IServiceCollection.AddDbContext<TContext>()` могут быть найдены и созданный таким способом. Этот шаблон был [появилась в ASP.NET Core 2.0][1]

<a name="using-the-default-constructor"></a>С помощью конструктора по умолчанию
-----------------------------
Если класс DbContext нельзя получить от поставщика службы приложений, средства поиска типа DbContext внутри проекта. Они пытаются создать его с помощью его конструктора по умолчанию.

<a name="from-a-design-time-factory"></a>Из фабрики во время разработки
--------------------------
Можно также сказать, средства Создание вашей DbContext путем реализации `IDesignTimeDbContextFactory`. Если класс, реализующий этот интерфейс располагается в пределах проекта, средства обойти другие способы создания класс DbContext.
Фабрика всегда используют во время разработки. Фабрика особенно полезен, если необходимо настроить класс DbContext по-разному для времени разработки, чем во время выполнения.

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
> `args` Параметр не в настоящее время используется. Отсутствует [проблему] [ 2] возможность указывать аргументы во время разработки с помощью средств отслеживания.

  [1]: https://docs.microsoft.com/aspnet/core/migration/1x-to-2x/#update-main-method-in-programcs
  [2]: https://github.com/aspnet/EntityFrameworkCore/issues/8332
