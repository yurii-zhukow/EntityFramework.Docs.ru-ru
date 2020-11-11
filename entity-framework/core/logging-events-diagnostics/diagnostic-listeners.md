---
title: Использование прослушивателей диагностики — EF Core
description: Использование Диагностиклистенер для глобального потребления EF Core диагностики
author: ajcvickers
ms.date: 10/16/2020
uid: core/logging-events-diagnostics/diagnostic-listeners
ms.openlocfilehash: a2a962ac714cf80c42c269cee3770699aaa4c0c9
ms.sourcegitcommit: 42bbf7f68e92c364c5fff63092d3eb02229f568d
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/11/2020
ms.locfileid: "94503232"
---
# <a name="using-diagnostic-listeners-in-ef-core"></a>Использование прослушивателей диагностики в EF Core

> [!TIP]  
> Вы можете [скачать пример этой статьи](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/DiagnosticListeners) с сайта GitHub.

Прослушиватели диагностики допускают прослушивание любого EF Core события, возникающего в текущем процессе .NET. <xref:System.Diagnostics.DiagnosticListener>Класс является частью [общего механизма в .NET](https://github.com/dotnet/runtime/blob/master/src/libraries/System.Diagnostics.DiagnosticSource/src/DiagnosticSourceUsersGuide.md) для получения диагностических сведений от выполняющихся приложений.

Прослушиватели диагностики не подходят для получения событий из одного экземпляра DbContext. [Перехватчики](xref:core/logging-events-diagnostics/interceptors) EF Core предоставляют доступ к одним и тем же событиям с регистрацией по контексту.

Прослушиватели диагностики не предназначены для ведения журнала. Рассмотрите возможность использования [простого ведения журнала](xref:core/logging-events-diagnostics/simple-logging) или [Microsoft. Extensions. Logging](xref:core/logging-events-diagnostics/extensions-logging) для ведения журнала.

## <a name="example-observing-diagnostic-events"></a>Пример. наблюдение за событиями диагностики

Разрешение EF Core событий состоит из двух этапов. Сначала необходимо создать [наблюдатель](/dotnet/standard/events/observer-design-pattern) для `DiagnosticListener` самого себя:

<!--
public class DiagnosticObserver : IObserver<DiagnosticListener>
{
    public void OnCompleted() 
        => throw new NotImplementedException();
    
    public void OnError(Exception error) 
        => throw new NotImplementedException();

    public void OnNext(DiagnosticListener value)
    {
        if (value.Name == DbLoggerCategory.Name) // "Microsoft.EntityFrameworkCore"
        {
            value.Subscribe(new KeyValueObserver());
        }
    }
}
-->
[!code-csharp[DiagnosticObserver](../../../samples/core/Miscellaneous/DiagnosticListeners/Program.cs?name=DiagnosticObserver)]

`OnNext`Метод ищет диагностиклистенер, который поступает из EF Core. Этот прослушиватель имеет имя Microsoft. EntityFrameworkCore, которое можно получить из <xref:Microsoft.EntityFrameworkCore.DbLoggerCategory> класса, как показано ниже.

Затем этот наблюдатель должен быть зарегистрирован глобально, например в `Main` методе приложения:

<!--
        DiagnosticListener.AllListeners.Subscribe(new DiagnosticObserver());
-->
[!code-csharp[RegisterDiagnosticListener](../../../samples/core/Miscellaneous/DiagnosticListeners/Program.cs?name=RegisterDiagnosticListener)]

Во-вторых, после обнаружения EF Core Диагностиклистенер создается новый наблюдатель "ключ — значение", который подписывается на фактические события EF Core. Пример:

<!--
public class KeyValueObserver : IObserver<KeyValuePair<string, object>>
{
    public void OnCompleted() 
        => throw new NotImplementedException();
    
    public void OnError(Exception error) 
        => throw new NotImplementedException();

    public void OnNext(KeyValuePair<string, object> value)
    {
        if (value.Key == CoreEventId.ContextInitialized.Name)
        {
            var payload = (ContextInitializedEventData)value.Value;
            Console.WriteLine($"EF is initializing {payload.Context.GetType().Name} ");
        }

        if (value.Key == RelationalEventId.ConnectionOpening.Name)
        {
            var payload = (ConnectionEventData)value.Value;
            Console.WriteLine($"EF is opening a connection to {payload.Connection.ConnectionString} ");
        }
    }
}
-->
[!code-csharp[KeyValueObserver](../../../samples/core/Miscellaneous/DiagnosticListeners/Program.cs?name=KeyValueObserver)]

`OnNext`Метод — это время, которое вызывается с парой "ключ-значение" для каждого EF Core события. Ключ — это имя события, которое можно получить одним из следующих:

* <xref:Microsoft.EntityFrameworkCore.Diagnostics.CoreEventId> для событий, общих для всех поставщиков EF Core баз данных
* <xref:Microsoft.EntityFrameworkCore.Diagnostics.RelationalEventId> для событий, общих для всех поставщиков реляционных баз данных
* Аналогичный класс для событий, относящихся к текущему поставщику базы данных. Например, <xref:Microsoft.EntityFrameworkCore.Diagnostics.SqlServerEventId> для поставщика SQL Server.

Значение пары "ключ-значение" — это тип полезной нагрузки, относящийся к событию. Тип полезных данных задокументирован в каждом событии, определенном в этих классах событий.

Например, приведенный выше код обрабатывает <xref:Microsoft.EntityFrameworkCore.Diagnostics.CoreEventId.ContextInitialized> <xref:Microsoft.EntityFrameworkCore.Diagnostics.RelationalEventId.ConnectionOpening> события и. Для первого из них полезная нагрузка: <xref:Microsoft.EntityFrameworkCore.Diagnostics.ContextInitializedEventData> . Во-вторых, это <xref:Microsoft.EntityFrameworkCore.Diagnostics.ConnectionEventData> .

> [!TIP]
> ToString переопределяется в каждом классе данных EF Core событий для создания эквивалентного сообщения журнала для события. Например, при вызове метода `ContextInitializedEventData.ToString` создается "Entity Framework Core 5.0.0 Initialized" блогсконтекст "с помощью поставщика" Microsoft. EntityFrameworkCore. SQLite "с параметрами: None".

[Пример](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/DiagnosticListeners) содержит простое консольное приложение, которое вносит изменения в базу данных блогов и выводит обнаруженные диагностические события.

<!--
    public static void Main()
    {
        #region RegisterDiagnosticListener
        DiagnosticListener.AllListeners.Subscribe(new DiagnosticObserver());
        #endregion
        
        using (var context = new BlogsContext())
        {
            context.Database.EnsureDeleted();
            context.Database.EnsureCreated();
            
            context.Add(
                new Blog
                {
                    Name = "EF Blog",
                    Posts =
                    {
                        new Post { Title = "EF Core 3.1!" },
                        new Post { Title = "EF Core 5.0!" }
                    }
                });

            context.SaveChanges();
        }

        using (var context = new BlogsContext())
        {
            var blog = context.Blogs.Include(e => e.Posts).Single();

            blog.Name = "EF Core Blog";
            context.Remove(blog.Posts.First());
            blog.Posts.Add(new Post { Title = "EF Core 6.0!" });

            context.SaveChanges();
        }
        #endregion
    }
-->
[!code-csharp[Program](../../../samples/core/Miscellaneous/DiagnosticListeners/Program.cs?name=Program)]

В выходных данных этого кода показаны события, обнаруженные:

```output
EF is initializing BlogsContext
EF is opening a connection to Data Source=blogs.db;Mode=ReadOnly
EF is opening a connection to DataSource=blogs.db
EF is opening a connection to Data Source=blogs.db;Mode=ReadOnly
EF is opening a connection to DataSource=blogs.db
EF is opening a connection to DataSource=blogs.db
EF is opening a connection to DataSource=blogs.db
EF is initializing BlogsContext
EF is opening a connection to DataSource=blogs.db
EF is opening a connection to DataSource=blogs.db
```
