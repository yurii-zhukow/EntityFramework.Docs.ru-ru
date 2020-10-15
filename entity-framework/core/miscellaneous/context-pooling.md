---
title: Пулы DbContext
description: DbContext пулов в Entity Framework Core
author: rick-anderson
ms.author: riande
ms.date: 9/19/2020
uid: core/miscellaneous/context-pooling
ms.openlocfilehash: 8638c838511be85bd994751b9911b107974dfe2f
ms.sourcegitcommit: 0a25c03fa65ae6e0e0e3f66bac48d59eceb96a5a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/14/2020
ms.locfileid: "92061961"
---
# <a name="dbcontext-pooling"></a><span data-ttu-id="6fc82-103">Создание пулов DbContext</span><span class="sxs-lookup"><span data-stu-id="6fc82-103">DbContext pooling</span></span>

<span data-ttu-id="6fc82-104">`AddDbContextPool` включает создание пулов `DbContext` экземпляров.</span><span class="sxs-lookup"><span data-stu-id="6fc82-104">`AddDbContextPool` enables pooling of `DbContext` instances.</span></span> <span data-ttu-id="6fc82-105">Пул контекста может увеличить пропускную способность в крупномасштабных сценариях, таких как веб-серверы, путем повторного использования экземпляров контекста вместо создания новых экземпляров для каждого запроса.</span><span class="sxs-lookup"><span data-stu-id="6fc82-105">Context pooling can increase throughput in high-scale scenarios such as web servers by reusing context instances, rather than creating new instances for each request.</span></span>

<span data-ttu-id="6fc82-106">Типичный шаблон в ASP.NET Core приложении, использующем EF Core, включает регистрацию пользовательского <xref:Microsoft.EntityFrameworkCore.DbContext> типа в контейнер [внедрения зависимостей](/aspnet/core/fundamentals/dependency-injection) и получение экземпляров этого типа через параметры конструктора в контроллерах или Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="6fc82-106">The typical pattern in an ASP.NET Core app using EF Core involves registering a custom <xref:Microsoft.EntityFrameworkCore.DbContext> type into the [dependency injection](/aspnet/core/fundamentals/dependency-injection) container and obtaining instances of that type through constructor parameters in controllers or Razor Pages.</span></span> <span data-ttu-id="6fc82-107">При использовании внедрения конструктора для каждого запроса создается новый экземпляр контекста.</span><span class="sxs-lookup"><span data-stu-id="6fc82-107">Using constructor injection, a new context instance is created for each request.</span></span>

<span data-ttu-id="6fc82-108"><xref:Microsoft.Extensions.DependencyInjection.EntityFrameworkServiceCollectionExtensions.AddDbContextPool%2A> включает пул экземпляров контекста для повторного использования.</span><span class="sxs-lookup"><span data-stu-id="6fc82-108"><xref:Microsoft.Extensions.DependencyInjection.EntityFrameworkServiceCollectionExtensions.AddDbContextPool%2A> enables a pool of reusable context instances.</span></span> <span data-ttu-id="6fc82-109">Чтобы использовать пулы контекста, используйте `AddDbContextPool` метод вместо `AddDbContext` во время регистрации службы:</span><span class="sxs-lookup"><span data-stu-id="6fc82-109">To use context pooling, use the `AddDbContextPool` method instead of `AddDbContext` during service registration:</span></span>

```csharp
services.AddDbContextPool<BloggingContext>(
    options => options.UseSqlServer(connectionString));
```

<span data-ttu-id="6fc82-110">Если `AddDbContextPool` используется, то при запросе экземпляра контекста EF сначала проверяет, доступен ли экземпляр в пуле.</span><span class="sxs-lookup"><span data-stu-id="6fc82-110">When `AddDbContextPool` is used, at the time a context instance is requested, EF first checks if there is an instance available in the pool.</span></span> <span data-ttu-id="6fc82-111">После завершения обработки запроса любое состояние экземпляра сбрасывается, а сам экземпляр возвращается в пул.</span><span class="sxs-lookup"><span data-stu-id="6fc82-111">Once the request processing finalizes, any state on the instance is reset and the instance is itself returned to the pool.</span></span>

<span data-ttu-id="6fc82-112">Это концептуально похоже на то, как пулы соединений работают в поставщиках ADO.NET и имеют преимущество при сохранении некоторых затрат на инициализацию экземпляра контекста.</span><span class="sxs-lookup"><span data-stu-id="6fc82-112">This is conceptually similar to how connection pooling operates in ADO.NET providers and has the advantage of saving some of the cost of initialization of the context instance.</span></span>

<span data-ttu-id="6fc82-113">`poolSize`Параметр <xref:Microsoft.Extensions.DependencyInjection.EntityFrameworkServiceCollectionExtensions.AddDbContextPool%2A> задает максимальное число экземпляров, хранящихся в пуле.</span><span class="sxs-lookup"><span data-stu-id="6fc82-113">The `poolSize` parameter of <xref:Microsoft.Extensions.DependencyInjection.EntityFrameworkServiceCollectionExtensions.AddDbContextPool%2A> sets the maximum number of instances retained by the pool.</span></span> <span data-ttu-id="6fc82-114">После этого `poolSize` новые экземпляры контекста не кэшируются, а EF возвращается к поведению без пула, создающего экземпляры по запросу.</span><span class="sxs-lookup"><span data-stu-id="6fc82-114">Once `poolSize` is exceeded, new context instances are not cached and  EF falls back to the non-pooling behavior of creating instances on demand.</span></span>

## <a name="limitations"></a><span data-ttu-id="6fc82-115">Ограничения</span><span class="sxs-lookup"><span data-stu-id="6fc82-115">Limitations</span></span>

<span data-ttu-id="6fc82-116">Приложения должны быть профилированы и протестированы для демонстрации того, что инициализация контекста является значительной ценой.</span><span class="sxs-lookup"><span data-stu-id="6fc82-116">Apps should be profiled and tested to show that context initialization is a significant cost.</span></span>

<span data-ttu-id="6fc82-117">`AddDbContextPool` имеет несколько ограничений на то, что может быть сделано в `OnConfiguring` методе контекста.</span><span class="sxs-lookup"><span data-stu-id="6fc82-117">`AddDbContextPool` has a few limitations on what can be done in the `OnConfiguring` method of the context.</span></span>

> [!WARNING]
> <span data-ttu-id="6fc82-118">Избегайте использования контекстного пула в приложениях, которые поддерживают состояние.</span><span class="sxs-lookup"><span data-stu-id="6fc82-118">Avoid using context pooling in apps that maintain state.</span></span> <span data-ttu-id="6fc82-119">Например, закрытые поля в контексте, которые не должны совместно использоваться в запросах.</span><span class="sxs-lookup"><span data-stu-id="6fc82-119">For example, private fields in the context that shouldn't be shared across requests.</span></span> <span data-ttu-id="6fc82-120">EF Core только сбрасывает состояние, о котором оно известно, перед добавлением экземпляра контекста в пул.</span><span class="sxs-lookup"><span data-stu-id="6fc82-120">EF Core only resets the state that it is aware of before adding a context instance to the pool.</span></span>

<span data-ttu-id="6fc82-121">Пул контекста работает путем повторного использования одного и того же экземпляра контекста в запросах.</span><span class="sxs-lookup"><span data-stu-id="6fc82-121">Context pooling works by reusing the same context instance across requests.</span></span> <span data-ttu-id="6fc82-122">Это означает, что оно эффективно регистрируется как [Singleton](/aspnet/core/fundamentals/dependency-injection#service-lifetimes) в терминах самого экземпляра, чтобы его можно было сохранить.</span><span class="sxs-lookup"><span data-stu-id="6fc82-122">This means that it's effectively registered as a [Singleton](/aspnet/core/fundamentals/dependency-injection#service-lifetimes) in terms of the instance itself so that it's able to persist.</span></span>

<span data-ttu-id="6fc82-123">Контекстное группирование предназначено для сценариев, в которых конфигурация контекста, включая разрешенные службы, фиксируется между запросами.</span><span class="sxs-lookup"><span data-stu-id="6fc82-123">Context pooling is intended for scenarios where the context configuration, which includes services resolved, is fixed between requests.</span></span> <span data-ttu-id="6fc82-124">Для случаев, когда требуются службы с заданной [областью](/aspnet/core/fundamentals/dependency-injection#service-lifetimes) или необходимо изменить конфигурацию, не используйте пулы.</span><span class="sxs-lookup"><span data-stu-id="6fc82-124">For cases where [Scoped](/aspnet/core/fundamentals/dependency-injection#service-lifetimes) services are required, or configuration needs to be changed, don't use pooling.</span></span> <span data-ttu-id="6fc82-125">Повышение производительности при использовании пулов обычно незначительно, за исключением сценариев с высоким уровнем оптимизации.</span><span class="sxs-lookup"><span data-stu-id="6fc82-125">The performance gain from pooling is usually negligible except in highly optimized scenarios.</span></span>
