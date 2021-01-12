---
title: Использование прослушивателей диагностики — EF Core
description: Использование Диагностиклистенер для глобального потребления EF Core диагностики
author: ajcvickers
ms.date: 10/16/2020
uid: core/logging-events-diagnostics/diagnostic-listeners
ms.openlocfilehash: afb80aa8f05f70761e423f58653f681938079858
ms.sourcegitcommit: 032a1767d7a6e42052a005f660b80372c6521e7e
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/12/2021
ms.locfileid: "98129269"
---
# <a name="using-diagnostic-listeners-in-ef-core"></a><span data-ttu-id="93942-103">Использование прослушивателей диагностики в EF Core</span><span class="sxs-lookup"><span data-stu-id="93942-103">Using Diagnostic Listeners in EF Core</span></span>

> [!TIP]
> <span data-ttu-id="93942-104">Вы можете [скачать пример этой статьи](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/DiagnosticListeners) с сайта GitHub.</span><span class="sxs-lookup"><span data-stu-id="93942-104">You can [download this article's sample](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/DiagnosticListeners) from GitHub.</span></span>

<span data-ttu-id="93942-105">Прослушиватели диагностики позволяют прослушивать любое событие EF Core, возникающее в текущем процессе .NET.</span><span class="sxs-lookup"><span data-stu-id="93942-105">Diagnostic listeners allow listening for any EF Core event that occurs in the current .NET process.</span></span> <span data-ttu-id="93942-106"><xref:System.Diagnostics.DiagnosticListener>Класс является частью [общего механизма в .NET](https://github.com/dotnet/runtime/blob/master/src/libraries/System.Diagnostics.DiagnosticSource/src/DiagnosticSourceUsersGuide.md) для получения диагностических сведений от выполняющихся приложений.</span><span class="sxs-lookup"><span data-stu-id="93942-106">The <xref:System.Diagnostics.DiagnosticListener> class is a part of a [common mechanism across .NET](https://github.com/dotnet/runtime/blob/master/src/libraries/System.Diagnostics.DiagnosticSource/src/DiagnosticSourceUsersGuide.md) for obtaining diagnostic information from running applications.</span></span>

<span data-ttu-id="93942-107">Прослушиватели диагностики не подходят для получения событий из одного экземпляра DbContext.</span><span class="sxs-lookup"><span data-stu-id="93942-107">Diagnostic listeners are not suitable for getting events from a single DbContext instance.</span></span> <span data-ttu-id="93942-108">[Перехватчики](xref:core/logging-events-diagnostics/interceptors) EF Core предоставляют доступ к одним и тем же событиям с регистрацией по контексту.</span><span class="sxs-lookup"><span data-stu-id="93942-108">EF Core [interceptors](xref:core/logging-events-diagnostics/interceptors) provide access to the same events with per-context registration.</span></span>

<span data-ttu-id="93942-109">Прослушиватели диагностики не предназначены для ведения журнала.</span><span class="sxs-lookup"><span data-stu-id="93942-109">Diagnostic listeners are not designed for logging.</span></span> <span data-ttu-id="93942-110">Рассмотрите возможность использования [простого ведения журнала](xref:core/logging-events-diagnostics/simple-logging) или [Microsoft. Extensions. Logging](xref:core/logging-events-diagnostics/extensions-logging) для ведения журнала.</span><span class="sxs-lookup"><span data-stu-id="93942-110">Consider using [simple logging](xref:core/logging-events-diagnostics/simple-logging) or [Microsoft.Extensions.Logging](xref:core/logging-events-diagnostics/extensions-logging) for logging.</span></span>

## <a name="example-observing-diagnostic-events"></a><span data-ttu-id="93942-111">Пример. наблюдение за событиями диагностики</span><span class="sxs-lookup"><span data-stu-id="93942-111">Example: Observing diagnostic events</span></span>

<span data-ttu-id="93942-112">Разрешение EF Core событий состоит из двух этапов.</span><span class="sxs-lookup"><span data-stu-id="93942-112">Resolving EF Core events is a two-step process.</span></span> <span data-ttu-id="93942-113">Сначала необходимо создать [наблюдатель](/dotnet/standard/events/observer-design-pattern) для `DiagnosticListener` самого себя:</span><span class="sxs-lookup"><span data-stu-id="93942-113">First, an [observer](/dotnet/standard/events/observer-design-pattern) for `DiagnosticListener` itself must be created:</span></span>

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

<span data-ttu-id="93942-114">`OnNext`Метод ищет диагностиклистенер, который поступает из EF Core.</span><span class="sxs-lookup"><span data-stu-id="93942-114">The `OnNext` method looks for the DiagnosticListener that comes from EF Core.</span></span> <span data-ttu-id="93942-115">Этот прослушиватель имеет имя Microsoft. EntityFrameworkCore, которое можно получить из <xref:Microsoft.EntityFrameworkCore.DbLoggerCategory> класса, как показано ниже.</span><span class="sxs-lookup"><span data-stu-id="93942-115">This listener has the name "Microsoft.EntityFrameworkCore", which can be obtained from the <xref:Microsoft.EntityFrameworkCore.DbLoggerCategory> class as shown.</span></span>

<span data-ttu-id="93942-116">Затем этот наблюдатель должен быть зарегистрирован глобально, например в `Main` методе приложения:</span><span class="sxs-lookup"><span data-stu-id="93942-116">This observer must then be registered globally, for example in the application's `Main` method:</span></span>

<!--
        DiagnosticListener.AllListeners.Subscribe(new DiagnosticObserver());
-->
[!code-csharp[RegisterDiagnosticListener](../../../samples/core/Miscellaneous/DiagnosticListeners/Program.cs?name=RegisterDiagnosticListener)]

<span data-ttu-id="93942-117">Во-вторых, после обнаружения EF Core Диагностиклистенер создается новый наблюдатель "ключ — значение", который подписывается на фактические события EF Core.</span><span class="sxs-lookup"><span data-stu-id="93942-117">Second, once the EF Core DiagnosticListener is found, a new key-value observer is created to subscribe to the actual EF Core events.</span></span> <span data-ttu-id="93942-118">Пример:</span><span class="sxs-lookup"><span data-stu-id="93942-118">For example:</span></span>

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

<span data-ttu-id="93942-119">`OnNext`Метод — это время, которое вызывается с парой "ключ-значение" для каждого EF Core события.</span><span class="sxs-lookup"><span data-stu-id="93942-119">The `OnNext` method is this time called with a key/value pair for each EF Core event.</span></span> <span data-ttu-id="93942-120">Ключ — это имя события, которое можно получить одним из следующих:</span><span class="sxs-lookup"><span data-stu-id="93942-120">The key is the name of the event, which can be obtained from one of:</span></span>

* <span data-ttu-id="93942-121"><xref:Microsoft.EntityFrameworkCore.Diagnostics.CoreEventId> для событий, общих для всех поставщиков EF Core баз данных</span><span class="sxs-lookup"><span data-stu-id="93942-121"><xref:Microsoft.EntityFrameworkCore.Diagnostics.CoreEventId> for events common to all EF Core database providers</span></span>
* <span data-ttu-id="93942-122"><xref:Microsoft.EntityFrameworkCore.Diagnostics.RelationalEventId> для событий, общих для всех поставщиков реляционных баз данных</span><span class="sxs-lookup"><span data-stu-id="93942-122"><xref:Microsoft.EntityFrameworkCore.Diagnostics.RelationalEventId> for events common to all relational database providers</span></span>
* <span data-ttu-id="93942-123">Аналогичный класс для событий, относящихся к текущему поставщику базы данных.</span><span class="sxs-lookup"><span data-stu-id="93942-123">A similar class for events specific to the current database provider.</span></span> <span data-ttu-id="93942-124">Например, <xref:Microsoft.EntityFrameworkCore.Diagnostics.SqlServerEventId> для поставщика SQL Server.</span><span class="sxs-lookup"><span data-stu-id="93942-124">For example, <xref:Microsoft.EntityFrameworkCore.Diagnostics.SqlServerEventId> for the SQL Server provider.</span></span>

<span data-ttu-id="93942-125">Значение пары "ключ-значение" — это тип полезной нагрузки, относящийся к событию.</span><span class="sxs-lookup"><span data-stu-id="93942-125">The value of the key/value pair is a payload type specific to the event.</span></span> <span data-ttu-id="93942-126">Тип полезных данных задокументирован в каждом событии, определенном в этих классах событий.</span><span class="sxs-lookup"><span data-stu-id="93942-126">The type of payload to expect is documented on each event defined in these event classes.</span></span>

<span data-ttu-id="93942-127">Например, приведенный выше код обрабатывает <xref:Microsoft.EntityFrameworkCore.Diagnostics.CoreEventId.ContextInitialized> <xref:Microsoft.EntityFrameworkCore.Diagnostics.RelationalEventId.ConnectionOpening> события и.</span><span class="sxs-lookup"><span data-stu-id="93942-127">For example, the code above handles the <xref:Microsoft.EntityFrameworkCore.Diagnostics.CoreEventId.ContextInitialized> and the <xref:Microsoft.EntityFrameworkCore.Diagnostics.RelationalEventId.ConnectionOpening> events.</span></span> <span data-ttu-id="93942-128">Для первого из них полезная нагрузка: <xref:Microsoft.EntityFrameworkCore.Diagnostics.ContextInitializedEventData> .</span><span class="sxs-lookup"><span data-stu-id="93942-128">For the first of these, the payload is <xref:Microsoft.EntityFrameworkCore.Diagnostics.ContextInitializedEventData>.</span></span> <span data-ttu-id="93942-129">Во-вторых, это <xref:Microsoft.EntityFrameworkCore.Diagnostics.ConnectionEventData> .</span><span class="sxs-lookup"><span data-stu-id="93942-129">For the second, it is <xref:Microsoft.EntityFrameworkCore.Diagnostics.ConnectionEventData>.</span></span>

> [!TIP]
> <span data-ttu-id="93942-130">ToString переопределяется в каждом классе данных EF Core событий для создания эквивалентного сообщения журнала для события.</span><span class="sxs-lookup"><span data-stu-id="93942-130">ToString is overridden in every EF Core event data class to generate the equivalent log message for the event.</span></span> <span data-ttu-id="93942-131">Например, при вызове метода `ContextInitializedEventData.ToString` создается "Entity Framework Core 5.0.0 Initialized" блогсконтекст "с помощью поставщика" Microsoft. EntityFrameworkCore. SQLite "с параметрами: None".</span><span class="sxs-lookup"><span data-stu-id="93942-131">For example, calling `ContextInitializedEventData.ToString` generates "Entity Framework Core 5.0.0 initialized 'BlogsContext' using provider 'Microsoft.EntityFrameworkCore.Sqlite' with options: None".</span></span>

<span data-ttu-id="93942-132">[Пример](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/DiagnosticListeners) содержит простое консольное приложение, которое вносит изменения в базу данных блогов и выводит обнаруженные диагностические события.</span><span class="sxs-lookup"><span data-stu-id="93942-132">The [sample](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/DiagnosticListeners) contains a simple console application that makes changes to the blogging database and prints out the diagnostic events encountered.</span></span>

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

<span data-ttu-id="93942-133">В выходных данных этого кода показаны события, обнаруженные:</span><span class="sxs-lookup"><span data-stu-id="93942-133">The output from this code shows the events detected:</span></span>

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
