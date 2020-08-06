---
title: Новые возможности EF Core 5.0
description: Обзор новых возможностей в EF Core 5.0
author: ajcvickers
ms.date: 07/20/2020
uid: core/what-is-new/ef-core-5.0/whatsnew
ms.openlocfilehash: d7f5863e657e243ce733eda5dc8b40c1b92818ce
ms.sourcegitcommit: 949faaba02e07e44359e77d7935f540af5c32093
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/03/2020
ms.locfileid: "87526879"
---
# <a name="whats-new-in-ef-core-50"></a><span data-ttu-id="f6d06-103">Новые возможности EF Core 5.0</span><span class="sxs-lookup"><span data-stu-id="f6d06-103">What's New in EF Core 5.0</span></span>

<span data-ttu-id="f6d06-104">Сейчас EF Core 5.0 находится на стадии разработки.</span><span class="sxs-lookup"><span data-stu-id="f6d06-104">EF Core 5.0 is currently in development.</span></span> <span data-ttu-id="f6d06-105">На этой странице представлен обзор интересных изменений, появившихся в каждой предварительной версии.</span><span class="sxs-lookup"><span data-stu-id="f6d06-105">This page will contain an overview of interesting changes introduced in each preview.</span></span>

<span data-ttu-id="f6d06-106">Эта страница не является аналогом статьи о [планировании для EF Core 5.0](xref:core/what-is-new/ef-core-5.0/plan).</span><span class="sxs-lookup"><span data-stu-id="f6d06-106">This page does not duplicate the [plan for EF Core 5.0](xref:core/what-is-new/ef-core-5.0/plan).</span></span> <span data-ttu-id="f6d06-107">В плане описываются общие темы для EF Core 5.0, в том числе все, что планируется включить перед выпуском окончательной версии.</span><span class="sxs-lookup"><span data-stu-id="f6d06-107">The plan describes the overall themes for EF Core 5.0, including everything we are planning to include before shipping the final release.</span></span>

<span data-ttu-id="f6d06-108">Мы будем добавлять ссылки на официальную документацию по мере ее публикации.</span><span class="sxs-lookup"><span data-stu-id="f6d06-108">We will add links from here to the official documentation as it is published.</span></span>

## <a name="preview-7"></a><span data-ttu-id="f6d06-109">предварительная версия 7</span><span class="sxs-lookup"><span data-stu-id="f6d06-109">Preview 7</span></span>

### <a name="dbcontextfactory"></a><span data-ttu-id="f6d06-110">DbContextFactory</span><span class="sxs-lookup"><span data-stu-id="f6d06-110">DbContextFactory</span></span>

<span data-ttu-id="f6d06-111">В EF Core 5.0 введены методы `AddDbContextFactory` и `AddPooledDbContextFactory` для регистрации производства с целью создания экземпляров DbContext в контейнере внедрения зависимостей приложения.</span><span class="sxs-lookup"><span data-stu-id="f6d06-111">EF Core 5.0 introduces `AddDbContextFactory` and `AddPooledDbContextFactory` to register a factory for creating DbContext instances in the application's dependency injection (D.I.) container.</span></span> <span data-ttu-id="f6d06-112">Пример:</span><span class="sxs-lookup"><span data-stu-id="f6d06-112">For example:</span></span>

```csharp
services.AddDbContextFactory<SomeDbContext>(b =>
    b.UseSqlServer(@"Server=(localdb)\mssqllocaldb;Database=Test"));
```

<span data-ttu-id="f6d06-113">Службы приложений, такие как контроллеры ASP.NET Core, могут затем использовать `IDbContextFactory<TContext>` в конструкторе службы.</span><span class="sxs-lookup"><span data-stu-id="f6d06-113">Application services such as ASP.NET Core controllers can then depend on `IDbContextFactory<TContext>` in the service constructor.</span></span> <span data-ttu-id="f6d06-114">Пример:</span><span class="sxs-lookup"><span data-stu-id="f6d06-114">For example:</span></span>

```csharp
public class MyController
{
    private readonly IDbContextFactory<SomeDbContext> _contextFactory;

    public MyController(IDbContextFactory<SomeDbContext> contextFactory)
    {
        _contextFactory = contextFactory;
    }
}
```

<span data-ttu-id="f6d06-115">После этого по мере необходимости можно создавать и использовать экземпляры DbContext.</span><span class="sxs-lookup"><span data-stu-id="f6d06-115">DbContext instances can then be created and used as needed.</span></span> <span data-ttu-id="f6d06-116">Пример:</span><span class="sxs-lookup"><span data-stu-id="f6d06-116">For example:</span></span>

```csharp
public void DoSomeThing()
{
    using (var context = _contextFactory.CreateDbContext())
    {
        // ...            
    }
}
```

<span data-ttu-id="f6d06-117">Обратите внимание, что созданные таким образом экземпляры DbContext _не_ управляются поставщиком служб приложения и поэтому должны удаляться приложением.</span><span class="sxs-lookup"><span data-stu-id="f6d06-117">Note that the DbContext instances created in this way are _not_ managed by the application's service provider and therefore must be disposed by the application.</span></span> <span data-ttu-id="f6d06-118">Такое разделение очень полезно для приложений Blazor, в которых рекомендуется использовать `IDbContextFactory`, но может быть полезно и в других сценариях.</span><span class="sxs-lookup"><span data-stu-id="f6d06-118">This decoupling is very useful for Blazor applications, where using `IDbContextFactory` is recommended, but may also be useful in other scenarios.</span></span>

<span data-ttu-id="f6d06-119">Экземпляры DbContext можно объединять в пул путем вызова `AddPooledDbContextFactory`.</span><span class="sxs-lookup"><span data-stu-id="f6d06-119">DbContext instances can be pooled by calling `AddPooledDbContextFactory`.</span></span> <span data-ttu-id="f6d06-120">Объединение в пул выполняется так же, как для `AddDbContextPool`, и имеет те же ограничения.</span><span class="sxs-lookup"><span data-stu-id="f6d06-120">This pooling works the same way as for `AddDbContextPool`, and also has the same limitations.</span></span>

<span data-ttu-id="f6d06-121">Документация отслеживается по проблеме [#2523](https://github.com/dotnet/EntityFramework.Docs/issues/2523).</span><span class="sxs-lookup"><span data-stu-id="f6d06-121">Documentation is tracked by issue [#2523](https://github.com/dotnet/EntityFramework.Docs/issues/2523).</span></span>

### <a name="reset-dbcontext-state"></a><span data-ttu-id="f6d06-122">Сброс состояния DbContext</span><span class="sxs-lookup"><span data-stu-id="f6d06-122">Reset DbContext state</span></span>

<span data-ttu-id="f6d06-123">В EF Core 5.0 появился метод `ChangeTracker.Clear()`, который очищает DbContext всех отслеживаемых сущностей.</span><span class="sxs-lookup"><span data-stu-id="f6d06-123">EF Core 5.0 introduces `ChangeTracker.Clear()` which clears the DbContext of all tracked entities.</span></span> <span data-ttu-id="f6d06-124">Обычно это не требуется, если выполняется рекомендация по созданию нового экземпляра контекста с кратким сроком существования для каждой единицы работы.</span><span class="sxs-lookup"><span data-stu-id="f6d06-124">This should usually not be needed when using the best practice of creating a new, short-lived context instance for each unit-of-work.</span></span> <span data-ttu-id="f6d06-125">Однако если необходимо сбросить состояние экземпляра DbContext, то использование нового метода `Clear()` является более производительным и надежным подходом, чем массовое отсоединение всех сущностей.</span><span class="sxs-lookup"><span data-stu-id="f6d06-125">However, if there is a need to reset the state of a DbContext instance, then using the new `Clear()` method is more performant and robust than mass-detaching all entities.</span></span>  

<span data-ttu-id="f6d06-126">Документация отслеживается по проблеме [#2524](https://github.com/dotnet/EntityFramework.Docs/issues/2524).</span><span class="sxs-lookup"><span data-stu-id="f6d06-126">Documentation is tracked by issue [#2524](https://github.com/dotnet/EntityFramework.Docs/issues/2524).</span></span>

### <a name="new-pattern-for-store-generated-defaults"></a><span data-ttu-id="f6d06-127">Новый шаблон для значений по умолчанию, создаваемых хранилищем</span><span class="sxs-lookup"><span data-stu-id="f6d06-127">New pattern for store-generated defaults</span></span>

<span data-ttu-id="f6d06-128">EF Core позволяет явно задать значение для столбца, у которого может быть ограничение на значение по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="f6d06-128">EF Core allows an explicit value to be set for a column that may also have default value constraint.</span></span> <span data-ttu-id="f6d06-129">В качестве индикатора для этого в EF Core используется значение, принятое по умолчанию для типа свойства в CLR. Если значение отлично от значения CLR по умолчанию, оно будет вставлено. В противном случае используется значение по умолчанию для базы данных.</span><span class="sxs-lookup"><span data-stu-id="f6d06-129">EF Core uses the CLR default of type property type as a sentinel for this; if the value is not the CLR default, then it is inserted, otherwise the database default is used.</span></span>

<span data-ttu-id="f6d06-130">Это создает проблемы для типов, для которых значение CLR по умолчанию является неподходящим индикатором, особенно для свойств `bool`.</span><span class="sxs-lookup"><span data-stu-id="f6d06-130">This creates problems for types where the CLR default is not a good sentinel--most notably, `bool` properties.</span></span> <span data-ttu-id="f6d06-131">В таких случаях EF Core 5.0 теперь допускает значение NULL для резервного поля.</span><span class="sxs-lookup"><span data-stu-id="f6d06-131">EF Core 5.0 now allows the backing field to be nullable for cases like this.</span></span> <span data-ttu-id="f6d06-132">Пример:</span><span class="sxs-lookup"><span data-stu-id="f6d06-132">For example:</span></span>

```csharp
public class Blog
{
    private bool? _isValid;

    public bool IsValid
    {
        get => _isValid ?? false;
        set => _isValid = value;
    }
}
```

<span data-ttu-id="f6d06-133">Обратите внимание, что резервное поле допускает значение NULL, а общедоступное свойство — нет.</span><span class="sxs-lookup"><span data-stu-id="f6d06-133">Note that the backing field is nullable, but the publicly exposed property is not.</span></span> <span data-ttu-id="f6d06-134">В результате значение индикатора может быть `null`, и это не влияет на общую контактную зону типа сущности.</span><span class="sxs-lookup"><span data-stu-id="f6d06-134">This allows the sentinel value to be `null` without impacting the public surface of the entity type.</span></span> <span data-ttu-id="f6d06-135">Если `IsValid` не задается, используется значение по умолчанию для базы данных, так как значением резервного поля остается NULL.</span><span class="sxs-lookup"><span data-stu-id="f6d06-135">In this case, if the `IsValid` is never set, then the database default will be used since the backing field remains null.</span></span> <span data-ttu-id="f6d06-136">Если задается `true` или `false`, это значение сохраняется явным образом в базе данных.</span><span class="sxs-lookup"><span data-stu-id="f6d06-136">If either `true` or `false` are set, then this value is saved explicitly to the database.</span></span>

<span data-ttu-id="f6d06-137">Документация отслеживается по проблеме [#2525](https://github.com/dotnet/EntityFramework.Docs/issues/2525).</span><span class="sxs-lookup"><span data-stu-id="f6d06-137">Documentation is tracked by issue [#2525](https://github.com/dotnet/EntityFramework.Docs/issues/2525).</span></span>

### <a name="cosmos-partition-keys"></a><span data-ttu-id="f6d06-138">Ключи секций Cosmos</span><span class="sxs-lookup"><span data-stu-id="f6d06-138">Cosmos partition keys</span></span>

<span data-ttu-id="f6d06-139">EF Core позволяет включать ключ секции Cosmos в модель EF.</span><span class="sxs-lookup"><span data-stu-id="f6d06-139">EF Core allows the Cosmos partition key is included in the EF model.</span></span> <span data-ttu-id="f6d06-140">Пример:</span><span class="sxs-lookup"><span data-stu-id="f6d06-140">For example:</span></span>

```csharp
modelBuilder.Entity<Customer>().HasPartitionKey(b => b.AlternateKey)
```

<span data-ttu-id="f6d06-141">Начиная с предварительной версии 7 ключ секции включается в первичный ключ типа сущности и используется для повышения производительности некоторых запросов.</span><span class="sxs-lookup"><span data-stu-id="f6d06-141">Starting with preview 7, the partition key is included in the entity type's PK and is used to improved performance in some queries.</span></span>

<span data-ttu-id="f6d06-142">Документация отслеживается по проблеме [#2471](https://github.com/dotnet/EntityFramework.Docs/issues/2471).</span><span class="sxs-lookup"><span data-stu-id="f6d06-142">Documentation is tracked by issue [#2471](https://github.com/dotnet/EntityFramework.Docs/issues/2471).</span></span>

### <a name="cosmos-configuration"></a><span data-ttu-id="f6d06-143">Конфигурация Cosmos</span><span class="sxs-lookup"><span data-stu-id="f6d06-143">Cosmos configuration</span></span>

<span data-ttu-id="f6d06-144">В EF Core 5.0 улучшена настройка Cosmos и подключений Cosmos.</span><span class="sxs-lookup"><span data-stu-id="f6d06-144">EF Core 5.0 improves configuration of Cosmos and Cosmos connections.</span></span>

<span data-ttu-id="f6d06-145">Ранее в EF Core требовалось явно указывать конечную точку и ключ при подключении к базе данных Cosmos.</span><span class="sxs-lookup"><span data-stu-id="f6d06-145">Previously, EF Core required the end-point and key to be specified explicitly when connecting to a Cosmos database.</span></span> <span data-ttu-id="f6d06-146">В EF Core 5.0 вместо этого можно использовать строку подключения.</span><span class="sxs-lookup"><span data-stu-id="f6d06-146">EF Core 5.0 allows use of a connection string instead.</span></span> <span data-ttu-id="f6d06-147">Кроме того, в EF Core 5.0 можно явно задавать экземпляр класса WebProxy.</span><span class="sxs-lookup"><span data-stu-id="f6d06-147">In addition, EF Core 5.0 allows the WebProxy instance to be explicitly set.</span></span> <span data-ttu-id="f6d06-148">Пример:</span><span class="sxs-lookup"><span data-stu-id="f6d06-148">For example:</span></span>

```csharp
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    => optionsBuilder
        .UseCosmos("my-cosmos-connection-string", "MyDb",
            cosmosOptionsBuilder =>
            {
                cosmosOptionsBuilder.WebProxy(myProxyInstance);
            });
```

<span data-ttu-id="f6d06-149">Кроме того, теперь также можно настраивать множество других значений времени ожидания, ограничений и т. д.</span><span class="sxs-lookup"><span data-stu-id="f6d06-149">Many other timeout values, limits, etc. can now also be configured.</span></span> <span data-ttu-id="f6d06-150">Пример:</span><span class="sxs-lookup"><span data-stu-id="f6d06-150">For example:</span></span>

```csharp
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    => optionsBuilder
        .UseCosmos("my-cosmos-connection-string", "MyDb",
            cosmosOptionsBuilder =>
            {
                cosmosOptionsBuilder.LimitToEndpoint();
                cosmosOptionsBuilder.RequestTimeout(requestTimeout);
                cosmosOptionsBuilder.OpenTcpConnectionTimeout(timeout);
                cosmosOptionsBuilder.IdleTcpConnectionTimeout(timeout);
                cosmosOptionsBuilder.GatewayModeMaxConnectionLimit(connectionLimit);
                cosmosOptionsBuilder.MaxTcpConnectionsPerEndpoint(connectionLimit);
                cosmosOptionsBuilder.MaxRequestsPerTcpConnection(requestLimit);
            });
```

<span data-ttu-id="f6d06-151">Наконец, режимом соединения по умолчанию теперь является `ConnectionMode.Gateway`, что, как правило, обеспечивает большую совместимость.</span><span class="sxs-lookup"><span data-stu-id="f6d06-151">Finally, the default connection mode is now `ConnectionMode.Gateway`, which is generally more compatible.</span></span>

<span data-ttu-id="f6d06-152">Документация отслеживается по проблеме [#2471](https://github.com/dotnet/EntityFramework.Docs/issues/2471).</span><span class="sxs-lookup"><span data-stu-id="f6d06-152">Documentation is tracked by issue [#2471](https://github.com/dotnet/EntityFramework.Docs/issues/2471).</span></span>

### <a name="scaffold-dbcontext-now-singularizes"></a><span data-ttu-id="f6d06-153">Формирование шаблонов DbContext теперь производится в форме единственного числа</span><span class="sxs-lookup"><span data-stu-id="f6d06-153">Scaffold-DbContext now singularizes</span></span>

<span data-ttu-id="f6d06-154">Ранее при формировании шаблона DbContext на основе существующей базы данных в EF Core создавались имена типов сущностей, совпадающие с именами таблиц в базе данных.</span><span class="sxs-lookup"><span data-stu-id="f6d06-154">Previously when scaffolding a DbContext from an existing database, EF Core will create entity type names that match the table names in the database.</span></span> <span data-ttu-id="f6d06-155">Например, для таблиц `People` и `Addresses` создавались типы сущностей с именами `People` и `Addresses`.</span><span class="sxs-lookup"><span data-stu-id="f6d06-155">For example, tables `People` and `Addresses` resulted in entity types named `People` and `Addresses`.</span></span>

<span data-ttu-id="f6d06-156">В предыдущих выпусках это поведение можно было настраивать путем регистрации службы преобразования во множественную форму.</span><span class="sxs-lookup"><span data-stu-id="f6d06-156">In previous releases, this behavior was configurable through registration of a pluralization service.</span></span> <span data-ttu-id="f6d06-157">Теперь в EF Core 5.0 в качестве службы преобразования во множественную форму по умолчанию используется пакет [Humanizer](https://www.nuget.org/packages/Humanizer.Core/).</span><span class="sxs-lookup"><span data-stu-id="f6d06-157">Now in EF Core 5.0, the [Humanizer](https://www.nuget.org/packages/Humanizer.Core/) package is used as a default pluralization service.</span></span> <span data-ttu-id="f6d06-158">Это означает, что таблицы `People` и `Addresses` теперь будут реконструированы в типы сущностей с именами `Person` и `Address`.</span><span class="sxs-lookup"><span data-stu-id="f6d06-158">This means tables `People` and `Addresses` will now be reverse engineered to entity types named `Person` and `Address`.</span></span>

### <a name="savepoints"></a><span data-ttu-id="f6d06-159">Точки сохранения</span><span class="sxs-lookup"><span data-stu-id="f6d06-159">Savepoints</span></span>

<span data-ttu-id="f6d06-160">EF Core теперь поддерживает [точки сохранения](/SQL/t-sql/language-elements/save-transaction-transact-sql?view=sql-server-ver15#remarks) для лучшего контроля над транзакциями, выполняющими несколько операций.</span><span class="sxs-lookup"><span data-stu-id="f6d06-160">EF Core now supports [savepoints](/SQL/t-sql/language-elements/save-transaction-transact-sql?view=sql-server-ver15#remarks) for greater control over transactions that execute multiple operations.</span></span>

<span data-ttu-id="f6d06-161">Точки сохранения можно создавать, освобождать и откатывать вручную.</span><span class="sxs-lookup"><span data-stu-id="f6d06-161">Savepoints can be manually created, released, and rolled back.</span></span> <span data-ttu-id="f6d06-162">Пример:</span><span class="sxs-lookup"><span data-stu-id="f6d06-162">For example:</span></span>

```csharp
context.Database.CreateSavepoint("MySavePoint"); 
```

<span data-ttu-id="f6d06-163">Кроме того, при неудачном выполнении `SaveChanges` в EF Core теперь выполняется откат к последней точке сохранения.</span><span class="sxs-lookup"><span data-stu-id="f6d06-163">In addition, EF Core will now roll back to the last savepoint when executing `SaveChanges` fails.</span></span> <span data-ttu-id="f6d06-164">Это позволяет повторно выполнять вызов SaveChanges без повторного выполнения всей транзакции.</span><span class="sxs-lookup"><span data-stu-id="f6d06-164">This allows SaveChanges to be re-tried without re-trying the entire transaction.</span></span>

<span data-ttu-id="f6d06-165">Документация отслеживается по проблеме [#2429](https://github.com/dotnet/EntityFramework.Docs/issues/2429).</span><span class="sxs-lookup"><span data-stu-id="f6d06-165">Documentation is tracked by issue [#2429](https://github.com/dotnet/EntityFramework.Docs/issues/2429).</span></span>

## <a name="preview-6"></a><span data-ttu-id="f6d06-166">предварительная версия 6</span><span class="sxs-lookup"><span data-stu-id="f6d06-166">Preview 6</span></span>

### <a name="split-queries-for-related-collections"></a><span data-ttu-id="f6d06-167">Разделение запросов для связанных коллекций</span><span class="sxs-lookup"><span data-stu-id="f6d06-167">Split queries for related collections</span></span>

<span data-ttu-id="f6d06-168">Начиная с EF Core 3.0, EF Core всегда создает один SQL-запрос для каждого LINQ-запроса.</span><span class="sxs-lookup"><span data-stu-id="f6d06-168">Starting with EF Core 3.0, EF Core always generates a single SQL query for each LINQ query.</span></span> <span data-ttu-id="f6d06-169">Это обеспечивает согласованность данных, возвращаемых в рамках ограничений используемого режима транзакций.</span><span class="sxs-lookup"><span data-stu-id="f6d06-169">This ensures consistency of the data returned within the constraints of the transaction mode in use.</span></span> <span data-ttu-id="f6d06-170">Однако если запрос использует `Include` или проекцию для возврата нескольких связанных коллекций, это может привести к снижению производительности.</span><span class="sxs-lookup"><span data-stu-id="f6d06-170">However, this can become very slow when the query uses `Include` or a projection to bring back multiple related collections.</span></span>

<span data-ttu-id="f6d06-171">Теперь EF Core 5.0 позволяет разделять один LINQ-запрос, включая связанные коллекции, на несколько SQL-запросов.</span><span class="sxs-lookup"><span data-stu-id="f6d06-171">EF Core 5.0 now allows a single LINQ query including related collections to be split into multiple SQL queries.</span></span> <span data-ttu-id="f6d06-172">Это может значительно повысить производительность, но может также привести к несогласованности возвращаемых результатов в случае изменения данных между двумя запросами.</span><span class="sxs-lookup"><span data-stu-id="f6d06-172">This can significantly improve performance, but can result in inconsistency in the results returned if the data changes between the two queries.</span></span> <span data-ttu-id="f6d06-173">Для устранения этой проблемы и обеспечения согласованности с разделенными запросами можно использовать сериализуемые транзакции или транзакции с моментальными снимками, но это может привести к дополнительным затратам на производительность и вызвать различия в поведении.</span><span class="sxs-lookup"><span data-stu-id="f6d06-173">Serializable or snapshot transactions can be used to mitigate this and achieve consistency with split queries, but that may bring other performance costs and behavioral difference.</span></span>

#### <a name="split-queries-with-include"></a><span data-ttu-id="f6d06-174">Разделение запросов с помощью Include</span><span class="sxs-lookup"><span data-stu-id="f6d06-174">Split queries with Include</span></span>

<span data-ttu-id="f6d06-175">Рассмотрим запрос, который извлекает два уровня связанных коллекций с помощью `Include`:</span><span class="sxs-lookup"><span data-stu-id="f6d06-175">For example, consider a query that pulls in two levels of related collections using `Include`:</span></span>

```CSharp
var artists = context.Artists
    .Include(e => e.Albums).ThenInclude(e => e.Tags)
    .ToList();
```

<span data-ttu-id="f6d06-176">Если используется поставщик SQLite, EF Core по умолчанию создаст следующий SQL:</span><span class="sxs-lookup"><span data-stu-id="f6d06-176">By default, EF Core will generate the following SQL when using the SQLite provider:</span></span>

```sql
SELECT "a"."Id", "a"."Name", "t0"."Id", "t0"."ArtistId", "t0"."Title", "t0"."Id0", "t0"."AlbumId", "t0"."Name"
FROM "Artists" AS "a"
LEFT JOIN (
    SELECT "a0"."Id", "a0"."ArtistId", "a0"."Title", "t"."Id" AS "Id0", "t"."AlbumId", "t"."Name"
    FROM "Album" AS "a0"
    LEFT JOIN "Tag" AS "t" ON "a0"."Id" = "t"."AlbumId"
) AS "t0" ON "a"."Id" = "t0"."ArtistId"
ORDER BY "a"."Id", "t0"."Id", "t0"."Id0"
```

<span data-ttu-id="f6d06-177">Для изменения этого поведения можно использовать новый API `AsSplitQuery`.</span><span class="sxs-lookup"><span data-stu-id="f6d06-177">The new `AsSplitQuery` API can be used to change this behavior.</span></span> <span data-ttu-id="f6d06-178">Пример:</span><span class="sxs-lookup"><span data-stu-id="f6d06-178">For example:</span></span>

```CSharp
var artists = context.Artists
    .AsSplitQuery()
    .Include(e => e.Albums).ThenInclude(e => e.Tags)
    .ToList();
```

<span data-ttu-id="f6d06-179">AsSplitQuery доступен для всех поставщиков реляционных баз данных и может использоваться в любом месте запроса так же, как AsNoTracking.</span><span class="sxs-lookup"><span data-stu-id="f6d06-179">AsSplitQuery is available for all relational database providers and can be used anywhere in the query, just like AsNoTracking.</span></span> <span data-ttu-id="f6d06-180">Теперь EF Core будет создавать следующие три SQL-запроса:</span><span class="sxs-lookup"><span data-stu-id="f6d06-180">EF Core will now generate the following three SQL queries:</span></span>

```sql
SELECT "a"."Id", "a"."Name"
FROM "Artists" AS "a"
ORDER BY "a"."Id"

SELECT "a0"."Id", "a0"."ArtistId", "a0"."Title", "a"."Id"
FROM "Artists" AS "a"
INNER JOIN "Album" AS "a0" ON "a"."Id" = "a0"."ArtistId"
ORDER BY "a"."Id", "a0"."Id"

SELECT "t"."Id", "t"."AlbumId", "t"."Name", "a"."Id", "a0"."Id"
FROM "Artists" AS "a"
INNER JOIN "Album" AS "a0" ON "a"."Id" = "a0"."ArtistId"
INNER JOIN "Tag" AS "t" ON "a0"."Id" = "t"."AlbumId"
ORDER BY "a"."Id", "a0"."Id"
```

<span data-ttu-id="f6d06-181">Поддерживаются все операции в корне запроса.</span><span class="sxs-lookup"><span data-stu-id="f6d06-181">All operations on the query root are supported.</span></span> <span data-ttu-id="f6d06-182">В их число входят операции OrderBy/Skip/Take, Join, FirstOrDefault и аналогичные операции выбора одного результата.</span><span class="sxs-lookup"><span data-stu-id="f6d06-182">This includes OrderBy/Skip/Take, Join operations, FirstOrDefault and similar single result selecting operations.</span></span>

<span data-ttu-id="f6d06-183">Обратите внимание, что включения с фильтрацией с операциями OrderBy/Skip/Take не поддерживаются в предварительной версии 6, но они доступны в ежедневных сборках и будут включены в предварительную версию 7.</span><span class="sxs-lookup"><span data-stu-id="f6d06-183">Note that filtered Includes with OrderBy/Skip/Take are not supported in preview 6, but are available in the daily builds and will be included in preview 7.</span></span>

#### <a name="split-queries-with-collection-projections"></a><span data-ttu-id="f6d06-184">Разделение запросов с помощью проекций коллекции</span><span class="sxs-lookup"><span data-stu-id="f6d06-184">Split queries with collection projections</span></span>

<span data-ttu-id="f6d06-185">`AsSplitQuery` можно также использовать при загрузке коллекций в проекции.</span><span class="sxs-lookup"><span data-stu-id="f6d06-185">`AsSplitQuery` can also be used when collections are loaded in projections.</span></span> <span data-ttu-id="f6d06-186">Пример:</span><span class="sxs-lookup"><span data-stu-id="f6d06-186">For example:</span></span>

```CSharp
context.Artists
    .AsSplitQuery()
    .Select(e => new
    {
        Artist = e,
        Albums = e.Albums,
    }).ToList();
```

<span data-ttu-id="f6d06-187">При использовании поставщика SQLite этот LINQ-запрос создает следующие два SQL-запроса:</span><span class="sxs-lookup"><span data-stu-id="f6d06-187">This LINQ query generates the following two SQL queries when using the SQLite provider:</span></span>

```sql
SELECT "a"."Id", "a"."Name"
FROM "Artists" AS "a"
ORDER BY "a"."Id"

SELECT "a0"."Id", "a0"."ArtistId", "a0"."Title", "a"."Id"
FROM "Artists" AS "a"
INNER JOIN "Album" AS "a0" ON "a"."Id" = "a0"."ArtistId"
ORDER BY "a"."Id"
```

<span data-ttu-id="f6d06-188">Обратите внимание, что поддерживается только материализация коллекции.</span><span class="sxs-lookup"><span data-stu-id="f6d06-188">Note that only materialization of the collection is supported.</span></span> <span data-ttu-id="f6d06-189">Любая композиция после `e.Albums` в приведенном выше случае не приведет к разделению запроса.</span><span class="sxs-lookup"><span data-stu-id="f6d06-189">Any composition after `e.Albums` in above case won't result in a split query.</span></span> <span data-ttu-id="f6d06-190">Улучшения в этой области отслеживаются по проблеме [#21234](https://github.com/dotnet/efcore/issues/21234).</span><span class="sxs-lookup"><span data-stu-id="f6d06-190">Improvements in this area are tracked by [#21234](https://github.com/dotnet/efcore/issues/21234).</span></span>

### <a name="indexattribute"></a><span data-ttu-id="f6d06-191">IndexAttribute</span><span class="sxs-lookup"><span data-stu-id="f6d06-191">IndexAttribute</span></span>

<span data-ttu-id="f6d06-192">Новый IndexAttribute можно поместить в тип сущности, чтобы указать индекс для одного столбца.</span><span class="sxs-lookup"><span data-stu-id="f6d06-192">The new IndexAttribute can be placed on an entity type to specify an index for a single column.</span></span> <span data-ttu-id="f6d06-193">Пример:</span><span class="sxs-lookup"><span data-stu-id="f6d06-193">For example:</span></span>

```CSharp
[Index(nameof(FullName), IsUnique = true)]
public class User
{
    public int Id { get; set; }
    
    [MaxLength(128)]
    public string FullName { get; set; }
}
```

<span data-ttu-id="f6d06-194">Для SQL Server функция миграции создаст следующий SQL:</span><span class="sxs-lookup"><span data-stu-id="f6d06-194">For SQL Server, Migrations will then generate the following SQL:</span></span>

```sql
CREATE UNIQUE INDEX [IX_Users_FullName]
    ON [Users] ([FullName])
    WHERE [FullName] IS NOT NULL;
```

<span data-ttu-id="f6d06-195">IndexAttribute можно также использовать для указания индекса, охватывающего несколько столбцов.</span><span class="sxs-lookup"><span data-stu-id="f6d06-195">IndexAttribute can also be used to specify an index spanning multiple columns.</span></span> <span data-ttu-id="f6d06-196">Пример:</span><span class="sxs-lookup"><span data-stu-id="f6d06-196">For example:</span></span>

```CSharp
[Index(nameof(FirstName), nameof(LastName), IsUnique = true)]
public class User
{
    public int Id { get; set; }
    
    [MaxLength(64)]
    public string FirstName { get; set; }

    [MaxLength(64)]
    public string LastName { get; set; }
}
```

<span data-ttu-id="f6d06-197">В SQL Server будет выведен следующий результат:</span><span class="sxs-lookup"><span data-stu-id="f6d06-197">For SQL Server, this results in:</span></span>

```sql
CREATE UNIQUE INDEX [IX_Users_FirstName_LastName]
    ON [Users] ([FirstName], [LastName])
    WHERE [FirstName] IS NOT NULL AND [LastName] IS NOT NULL;
```

<span data-ttu-id="f6d06-198">Документация отслеживается по проблеме [#2407](https://github.com/dotnet/EntityFramework.Docs/issues/2407).</span><span class="sxs-lookup"><span data-stu-id="f6d06-198">Documentation is tracked by issue [#2407](https://github.com/dotnet/EntityFramework.Docs/issues/2407).</span></span>

### <a name="improved-query-translation-exceptions"></a><span data-ttu-id="f6d06-199">Улучшенные исключения преобразования запросов</span><span class="sxs-lookup"><span data-stu-id="f6d06-199">Improved query translation exceptions</span></span>

<span data-ttu-id="f6d06-200">Мы продолжаем улучшать сообщения об исключениях, создаваемые при сбое преобразования запроса.</span><span class="sxs-lookup"><span data-stu-id="f6d06-200">We are continuing to improve the exception messages generated when query translation fails.</span></span> <span data-ttu-id="f6d06-201">Например, в этом запросе используется несопоставленное свойство `IsSigned`:</span><span class="sxs-lookup"><span data-stu-id="f6d06-201">For example, this query uses the unmapped property `IsSigned`:</span></span>

```CSharp
var artists = context.Artists.Where(e => e.IsSigned).ToList();
```

<span data-ttu-id="f6d06-202">EF Core выдаст следующее исключение, указывающее на сбой преобразования из-за несопоставленного `IsSigned`:</span><span class="sxs-lookup"><span data-stu-id="f6d06-202">EF Core will throw the following exception indicating that translation failed because `IsSigned` is not mapped:</span></span>

> <span data-ttu-id="f6d06-203">Необработанное исключение.</span><span class="sxs-lookup"><span data-stu-id="f6d06-203">Unhandled exception.</span></span> <span data-ttu-id="f6d06-204">"System.InvalidOperationException: не удалось преобразовать выражение LINQ "DbSet<Artist>() .Where(a => a.IsSigned)".</span><span class="sxs-lookup"><span data-stu-id="f6d06-204">System.InvalidOperationException: The LINQ expression 'DbSet<Artist>() .Where(a => a.IsSigned)' could not be translated.</span></span> <span data-ttu-id="f6d06-205">Дополнительные сведения: не удалось преобразовать член "IsSigned" для типа сущности "Artist".</span><span class="sxs-lookup"><span data-stu-id="f6d06-205">Additional information: Translation of member 'IsSigned' on entity type 'Artist' failed.</span></span> <span data-ttu-id="f6d06-206">Возможно, указанный элемент не сопоставлен.</span><span class="sxs-lookup"><span data-stu-id="f6d06-206">Possibly the specified member is not mapped.</span></span> <span data-ttu-id="f6d06-207">Перепишите запрос в поддерживающей преобразование форме или явно переключитесь на оценку клиента, вставив вызов либо AsEnumerable(), AsAsyncEnumerable(), ToList(), либо ToListAsync().</span><span class="sxs-lookup"><span data-stu-id="f6d06-207">Either rewrite the query in a form that can be translated, or switch to client evaluation explicitly by inserting a call to either AsEnumerable(), AsAsyncEnumerable(), ToList(), or ToListAsync().</span></span> <span data-ttu-id="f6d06-208">Дополнительные сведения см. в разделе https://go.microsoft.com/fwlink/?linkid=2101038.</span><span class="sxs-lookup"><span data-stu-id="f6d06-208">See https://go.microsoft.com/fwlink/?linkid=2101038 for more information.</span></span>

<span data-ttu-id="f6d06-209">Аналогично, при попытке преобразовать сравнения строк с семантикой, зависящей от языка и региональных параметров, теперь создаются более понятные сообщения об исключениях.</span><span class="sxs-lookup"><span data-stu-id="f6d06-209">Similarly, better exception messages are now generated when attempting to translate string comparisons with culture-dependent semantics.</span></span> <span data-ttu-id="f6d06-210">Например, этот запрос пытается использовать `StringComparison.CurrentCulture`:</span><span class="sxs-lookup"><span data-stu-id="f6d06-210">For example, this query attempts to use `StringComparison.CurrentCulture`:</span></span>

```CSharp
var artists = context.Artists
    .Where(e => e.Name.Equals("The Unicorns", StringComparison.CurrentCulture))
    .ToList();
```

<span data-ttu-id="f6d06-211">Теперь EF Core выдаст следующее исключение:</span><span class="sxs-lookup"><span data-stu-id="f6d06-211">EF Core will now throw the following exception:</span></span>

> <span data-ttu-id="f6d06-212">Необработанное исключение.</span><span class="sxs-lookup"><span data-stu-id="f6d06-212">Unhandled exception.</span></span> <span data-ttu-id="f6d06-213">"System.InvalidOperationException: не удается преобразовать выражение LINQ "DbSet<Artist>() .Where(a => a.Name.Equals( value: "The Unicorns", comparisonType: CurrentCulture))".</span><span class="sxs-lookup"><span data-stu-id="f6d06-213">System.InvalidOperationException: The LINQ expression 'DbSet<Artist>() .Where(a => a.Name.Equals( value: "The Unicorns", comparisonType: CurrentCulture))' could not be translated.</span></span> <span data-ttu-id="f6d06-214">Дополнительные сведения: преобразование метода "string.Equals", принимающего аргумент "StringComparison", не поддерживается.</span><span class="sxs-lookup"><span data-stu-id="f6d06-214">Additional information: Translation of 'string.Equals' method which takes 'StringComparison' argument is not supported.</span></span> <span data-ttu-id="f6d06-215">Дополнительные сведения см. в разделе https://go.microsoft.com/fwlink/?linkid=2129535.</span><span class="sxs-lookup"><span data-stu-id="f6d06-215">See https://go.microsoft.com/fwlink/?linkid=2129535 for more information.</span></span> <span data-ttu-id="f6d06-216">Перепишите запрос в поддерживающей преобразование форме или явно переключитесь на оценку клиента, вставив вызов либо AsEnumerable(), AsAsyncEnumerable(), ToList(), либо ToListAsync().</span><span class="sxs-lookup"><span data-stu-id="f6d06-216">Either rewrite the query in a form that can be translated, or switch to client evaluation explicitly by inserting a call to either AsEnumerable(), AsAsyncEnumerable(), ToList(), or ToListAsync().</span></span> <span data-ttu-id="f6d06-217">Дополнительные сведения см. в разделе https://go.microsoft.com/fwlink/?linkid=2101038.</span><span class="sxs-lookup"><span data-stu-id="f6d06-217">See https://go.microsoft.com/fwlink/?linkid=2101038 for more information.</span></span>

### <a name="specify-transaction-id"></a><span data-ttu-id="f6d06-218">Указание идентификатора транзакции</span><span class="sxs-lookup"><span data-stu-id="f6d06-218">Specify transaction ID</span></span>

<span data-ttu-id="f6d06-219">Эту функцию предоставил участник сообщества [@Marusyk](https://github.com/Marusyk).</span><span class="sxs-lookup"><span data-stu-id="f6d06-219">This feature was contributed from the community by [@Marusyk](https://github.com/Marusyk).</span></span> <span data-ttu-id="f6d06-220">Большое спасибо за публикацию!</span><span class="sxs-lookup"><span data-stu-id="f6d06-220">Many thanks for the contribution!</span></span>

<span data-ttu-id="f6d06-221">EF Core предоставляет идентификатор транзакции для корреляции транзакций между вызовами.</span><span class="sxs-lookup"><span data-stu-id="f6d06-221">EF Core exposes a transaction ID for correlation of transactions across calls.</span></span> <span data-ttu-id="f6d06-222">Этот идентификатор обычно задается в EF Core при запуске транзакции.</span><span class="sxs-lookup"><span data-stu-id="f6d06-222">This ID typically set by EF Core when a transaction is started.</span></span> <span data-ttu-id="f6d06-223">Если транзакцию запускает приложение, эта функция позволяет приложению явно задать идентификатор транзакции для его правильной корреляции везде, где она используется.</span><span class="sxs-lookup"><span data-stu-id="f6d06-223">If the application starts the transaction instead, then this feature allows the application to explicitly set the transaction ID so it is correlated correctly everywhere it is used.</span></span> <span data-ttu-id="f6d06-224">Пример:</span><span class="sxs-lookup"><span data-stu-id="f6d06-224">For example:</span></span>

```CSharp
using (context.Database.UseTransaction(myTransaction, myId))
{
   ...
}
```

### <a name="ipaddress-mapping"></a><span data-ttu-id="f6d06-225">Сопоставление IPAddress</span><span class="sxs-lookup"><span data-stu-id="f6d06-225">IPAddress mapping</span></span>

<span data-ttu-id="f6d06-226">Эту функцию предоставил участник сообщества [@ralmsdeveloper](https://github.com/ralmsdeveloper).</span><span class="sxs-lookup"><span data-stu-id="f6d06-226">This feature was contributed from the community by [@ralmsdeveloper](https://github.com/ralmsdeveloper).</span></span> <span data-ttu-id="f6d06-227">Большое спасибо за публикацию!</span><span class="sxs-lookup"><span data-stu-id="f6d06-227">Many thanks for the contribution!</span></span>

<span data-ttu-id="f6d06-228">Стандартный класс [IPAddress](/dotnet/api/system.net.ipaddress) .NET теперь автоматически сопоставляется со строковым столбцом для баз данных, у которых пока нет собственной поддержки.</span><span class="sxs-lookup"><span data-stu-id="f6d06-228">The standard .NET [IPAddress class](/dotnet/api/system.net.ipaddress) is now automatically mapped to a string column for databases that do not already have native support.</span></span> <span data-ttu-id="f6d06-229">Например, рассмотрим сопоставление этого типа сущности:</span><span class="sxs-lookup"><span data-stu-id="f6d06-229">For example, consider mapping this entity type:</span></span>

```CSharp
public class Host
{
    public int Id { get; set; }
    public IPAddress Address { get; set; }
}
```

<span data-ttu-id="f6d06-230">В SQL Server функция миграции создаст следующую таблицу:</span><span class="sxs-lookup"><span data-stu-id="f6d06-230">On SQL Server, this will result in Migrations creating the following table:</span></span>

```sql
CREATE TABLE [Host] (
    [Id] int NOT NULL,
    [Address] nvarchar(45) NULL,
    CONSTRAINT [PK_Host] PRIMARY KEY ([Id]));
``` 

<span data-ttu-id="f6d06-231">Затем можно добавить сущности обычным образом:</span><span class="sxs-lookup"><span data-stu-id="f6d06-231">Entities can then be added in the normal way:</span></span>

```CSharp
context.AddRange(
    new Host { Address = IPAddress.Parse("127.0.0.1")},
    new Host { Address = IPAddress.Parse("0000:0000:0000:0000:0000:0000:0000:0001")});
``` 

<span data-ttu-id="f6d06-232">Результирующий SQL вставит нормализованный IPv4- или IPv6-адрес:</span><span class="sxs-lookup"><span data-stu-id="f6d06-232">And the resulting SQL will insert the normalized IPv4 or IPv6 address:</span></span>

```sql
Executed DbCommand (14ms) [Parameters=[@p0='1', @p1='127.0.0.1' (Size = 45), @p2='2', @p3='::1' (Size = 45)], CommandType='Text', CommandTimeout='30']
      SET NOCOUNT ON;
      INSERT INTO [Host] ([Id], [Address])
      VALUES (@p0, @p1), (@p2, @p3);
```

### <a name="exclude-onconfiguring-when-scaffolding"></a><span data-ttu-id="f6d06-233">Исключение OnConfiguring при формировании шаблонов</span><span class="sxs-lookup"><span data-stu-id="f6d06-233">Exclude OnConfiguring when scaffolding</span></span>

<span data-ttu-id="f6d06-234">При формировании DbContext из существующей базы данных EF Core по умолчанию создает перегрузку OnConfiguring со строкой подключения для немедленного использования контекста.</span><span class="sxs-lookup"><span data-stu-id="f6d06-234">When a DbContext is scaffolded from an existing database, EF Core by default creates an OnConfiguring overload with a connection string so that the context is immediately usable.</span></span> <span data-ttu-id="f6d06-235">Однако это бесполезно, если у вас уже есть разделяемый класс с OnConfiguring, или если вы настраиваете контекст другим способом.</span><span class="sxs-lookup"><span data-stu-id="f6d06-235">However, this is not useful if you already have a partial class with OnConfiguring, or if you are configuring the context some other way.</span></span>

<span data-ttu-id="f6d06-236">Теперь командам формирования шаблонов можно указать пропустить создание OnConfiguring.</span><span class="sxs-lookup"><span data-stu-id="f6d06-236">To address this, the scaffolding commands can now be instructed to omit generation of OnConfiguring.</span></span> <span data-ttu-id="f6d06-237">Пример:</span><span class="sxs-lookup"><span data-stu-id="f6d06-237">For example:</span></span>

```
dotnet ef dbcontext scaffold "Data Source=(localdb)\MSSQLLocalDB;Initial Catalog=Chinook" Microsoft.EntityFrameworkCore.SqlServer --no-onconfiguring
```

<span data-ttu-id="f6d06-238">Или в консоли диспетчера пакетов:</span><span class="sxs-lookup"><span data-stu-id="f6d06-238">Or in the Package Manager Console:</span></span>

```
Scaffold-DbContext 'Data Source=(localdb)\MSSQLLocalDB;Initial Catalog=Chinook' Microsoft.EntityFrameworkCore.SqlServer -NoOnConfiguring 
``` 

<span data-ttu-id="f6d06-239">Обратите внимание, что рекомендуется использовать [именованную строку подключения и защищенное хранилище, такое как секреты пользователей](/core/managing-schemas/scaffolding?tabs=vs#configuration-and-user-secrets).</span><span class="sxs-lookup"><span data-stu-id="f6d06-239">Note that we recommend using [a named connection string and secure storage like User Secrets](/core/managing-schemas/scaffolding?tabs=vs#configuration-and-user-secrets).</span></span>

### <a name="translations-for-firstordefault-on-strings"></a><span data-ttu-id="f6d06-240">Преобразования для FirstOrDefault в строках</span><span class="sxs-lookup"><span data-stu-id="f6d06-240">Translations for FirstOrDefault on strings</span></span>

<span data-ttu-id="f6d06-241">Эту функцию предоставил участник сообщества [@dvoreckyaa](https://github.com/dvoreckyaa).</span><span class="sxs-lookup"><span data-stu-id="f6d06-241">This feature was contributed from the community by [@dvoreckyaa](https://github.com/dvoreckyaa).</span></span> <span data-ttu-id="f6d06-242">Большое спасибо за публикацию!</span><span class="sxs-lookup"><span data-stu-id="f6d06-242">Many thanks for the contribution!</span></span>

<span data-ttu-id="f6d06-243">FirstOrDefault и аналогичные операторы для символов в строках теперь можно преобразовывать.</span><span class="sxs-lookup"><span data-stu-id="f6d06-243">FirstOrDefault and similar operators for characters in strings are now translated.</span></span> <span data-ttu-id="f6d06-244">Например, следующий LINQ-запрос:</span><span class="sxs-lookup"><span data-stu-id="f6d06-244">For example, this LINQ query:</span></span>

```CSharp
context.Customers.Where(c => c.ContactName.FirstOrDefault() == 'A').ToList();
```

<span data-ttu-id="f6d06-245">будет преобразован в следующий SQL при использовании SQL Server:</span><span class="sxs-lookup"><span data-stu-id="f6d06-245">Will be translated to the following SQL when using SQL Server:</span></span>

```sql
SELECT [c].[Id], [c].[ContactName]
FROM [Customer] AS [c]
WHERE SUBSTRING([c].[ContactName], 1, 1) = N'A'
```

### <a name="simplify-case-blocks"></a><span data-ttu-id="f6d06-246">Упрощение блоков case</span><span class="sxs-lookup"><span data-stu-id="f6d06-246">Simplify case blocks</span></span>

<span data-ttu-id="f6d06-247">Теперь EF Core создает улучшенные запросы с блоками case.</span><span class="sxs-lookup"><span data-stu-id="f6d06-247">EF Core now generates better queries with CASE blocks.</span></span> <span data-ttu-id="f6d06-248">Например, следующий LINQ-запрос:</span><span class="sxs-lookup"><span data-stu-id="f6d06-248">For example, this LINQ query:</span></span> 

```CSharp
context.Weapons
    .OrderBy(w => w.Name.CompareTo("Marcus' Lancer") == 0)
    .ThenBy(w => w.Id)
```

<span data-ttu-id="f6d06-249">раньше преобразовывался в SQL Server в:</span><span class="sxs-lookup"><span data-stu-id="f6d06-249">Was on SQL Server formally translated to:</span></span>

```sql
SELECT [w].[Id], [w].[AmmunitionType], [w].[IsAutomatic], [w].[Name], [w].[OwnerFullName], [w].[SynergyWithId]
FROM [Weapons] AS [w]
ORDER BY CASE
    WHEN (CASE
        WHEN [w].[Name] = N'Marcus'' Lancer' THEN 0
        WHEN [w].[Name] > N'Marcus'' Lancer' THEN 1
        WHEN [w].[Name] < N'Marcus'' Lancer' THEN -1
    END = 0) AND CASE
        WHEN [w].[Name] = N'Marcus'' Lancer' THEN 0
        WHEN [w].[Name] > N'Marcus'' Lancer' THEN 1
        WHEN [w].[Name] < N'Marcus'' Lancer' THEN -1
    END IS NOT NULL THEN CAST(1 AS bit)
    ELSE CAST(0 AS bit)
END, [w].[Id]");
``` 

<span data-ttu-id="f6d06-250">Но теперь преобразуется в:</span><span class="sxs-lookup"><span data-stu-id="f6d06-250">But is now translated to:</span></span>

```sql
SELECT [w].[Id], [w].[AmmunitionType], [w].[IsAutomatic], [w].[Name], [w].[OwnerFullName], [w].[SynergyWithId]
FROM [Weapons] AS [w]
ORDER BY CASE
    WHEN ([w].[Name] = N'Marcus'' Lancer') AND [w].[Name] IS NOT NULL THEN CAST(1 AS bit)
    ELSE CAST(0 AS bit)
END, [w].[Id]");
``` 

## <a name="preview-5"></a><span data-ttu-id="f6d06-251">Предварительная версия 5</span><span class="sxs-lookup"><span data-stu-id="f6d06-251">Preview 5</span></span>

### <a name="database-collations"></a><span data-ttu-id="f6d06-252">Параметры сортировки баз данных</span><span class="sxs-lookup"><span data-stu-id="f6d06-252">Database collations</span></span>

<span data-ttu-id="f6d06-253">Модель EF теперь позволяет указывать параметры сортировки по умолчанию для базы данных.</span><span class="sxs-lookup"><span data-stu-id="f6d06-253">The default collation for a database can now be specified in the EF model.</span></span> <span data-ttu-id="f6d06-254">Это будет передаваться в создаваемые миграции для задания параметров сортировки при создании базы данных.</span><span class="sxs-lookup"><span data-stu-id="f6d06-254">This will flow through to generated migrations to set the collation when the database is created.</span></span> <span data-ttu-id="f6d06-255">Пример:</span><span class="sxs-lookup"><span data-stu-id="f6d06-255">For example:</span></span>

```CSharp
modelBuilder.UseCollation("German_PhoneBook_CI_AS");
```

<span data-ttu-id="f6d06-256">Затем функция миграции формирует следующий код для создания базы данных в SQL Server:</span><span class="sxs-lookup"><span data-stu-id="f6d06-256">Migrations then generates the following to create the database on SQL Server:</span></span>

```sql
CREATE DATABASE [Test]
COLLATE German_PhoneBook_CI_AS;
```

<span data-ttu-id="f6d06-257">Можно также указать параметры сортировки, которые будут использоваться для конкретных столбцов.</span><span class="sxs-lookup"><span data-stu-id="f6d06-257">The collation to use for specific database columns can also be specified.</span></span> <span data-ttu-id="f6d06-258">Пример:</span><span class="sxs-lookup"><span data-stu-id="f6d06-258">For example:</span></span>

```CSharp
 modelBuilder
     .Entity<User>()
     .Property(e => e.Name)
     .UseCollation("German_PhoneBook_CI_AS");
```

<span data-ttu-id="f6d06-259">Если функция миграции не используется, параметры сортировки реконструируются из базы данных при формировании шаблонов DbContext.</span><span class="sxs-lookup"><span data-stu-id="f6d06-259">For those not using migrations, collations are now reverse-engineered from the database when scaffolding a DbContext.</span></span>

<span data-ttu-id="f6d06-260">Наконец, `EF.Functions.Collate()` позволяет использовать различные параметры сортировки для нерегламентированных запросов.</span><span class="sxs-lookup"><span data-stu-id="f6d06-260">Finally, the `EF.Functions.Collate()` allows for ad-hoc queries using different collations.</span></span> <span data-ttu-id="f6d06-261">Пример:</span><span class="sxs-lookup"><span data-stu-id="f6d06-261">For example:</span></span>

```CSharp
context.Users.Single(e => EF.Functions.Collate(e.Name, "French_CI_AS") == "Jean-Michel Jarre");
```

<span data-ttu-id="f6d06-262">Будет создан следующий запрос для SQL Server:</span><span class="sxs-lookup"><span data-stu-id="f6d06-262">This will generate the following query for SQL Server:</span></span>

```sql
SELECT TOP(2) [u].[Id], [u].[Name]
FROM [Users] AS [u]
WHERE [u].[Name] COLLATE French_CI_AS = N'Jean-Michel Jarre'
```

<span data-ttu-id="f6d06-263">Обратите внимание, что нерегламентированные параметры сортировки следует использовать с осторожностью, так как они могут негативно повлиять на производительность базы данных.</span><span class="sxs-lookup"><span data-stu-id="f6d06-263">Note that ad-hoc collations should be used with care as they can negatively impact database performance.</span></span>

<span data-ttu-id="f6d06-264">Это документируется в проблеме [#2273](https://github.com/dotnet/EntityFramework.Docs/issues/2273).</span><span class="sxs-lookup"><span data-stu-id="f6d06-264">Documentation is tracked by issue [#2273](https://github.com/dotnet/EntityFramework.Docs/issues/2273).</span></span>

### <a name="flow-arguments-into-idesigntimedbcontextfactory"></a><span data-ttu-id="f6d06-265">Передача аргументов в интерфейс IDesignTimeDbContextFactory</span><span class="sxs-lookup"><span data-stu-id="f6d06-265">Flow arguments into IDesignTimeDbContextFactory</span></span>

<span data-ttu-id="f6d06-266">Теперь можно передавать аргументы из командной строки в метод `CreateDbContext` интерфейса [IDesignTimeDbContextFactory](/dotnet/api/microsoft.entityframeworkcore.design.idesigntimedbcontextfactory-1?view=efcore-3.1).</span><span class="sxs-lookup"><span data-stu-id="f6d06-266">Arguments are now flowed from the command line into the `CreateDbContext` method of [IDesignTimeDbContextFactory](/dotnet/api/microsoft.entityframeworkcore.design.idesigntimedbcontextfactory-1?view=efcore-3.1).</span></span> <span data-ttu-id="f6d06-267">Например, чтобы указать, что используется сборка для разработки, можно передать в командной строке настраиваемый аргумент, например `dev`:</span><span class="sxs-lookup"><span data-stu-id="f6d06-267">For example, to indicate this is a dev build, a custom argument (e.g. `dev`) can be passed on the command line:</span></span>

```
dotnet ef migrations add two --verbose --dev
``` 

<span data-ttu-id="f6d06-268">Затем этот аргумент будет передан в фабрику, где его можно использовать для управления созданием и инициализацией контекста.</span><span class="sxs-lookup"><span data-stu-id="f6d06-268">This argument will then flow into the factory, where it can be used to control how the context is created and initialized.</span></span> <span data-ttu-id="f6d06-269">Пример:</span><span class="sxs-lookup"><span data-stu-id="f6d06-269">For example:</span></span>

```CSharp
public class MyDbContextFactory : IDesignTimeDbContextFactory<SomeDbContext>
{
    public SomeDbContext CreateDbContext(string[] args) 
        => new SomeDbContext(args.Contains("--dev"));
}
```

<span data-ttu-id="f6d06-270">Это документируется в проблеме [#2419](https://github.com/dotnet/EntityFramework.Docs/issues/2419).</span><span class="sxs-lookup"><span data-stu-id="f6d06-270">Documentation is tracked by issue [#2419](https://github.com/dotnet/EntityFramework.Docs/issues/2419).</span></span>

### <a name="no-tracking-queries-with-identity-resolution"></a><span data-ttu-id="f6d06-271">Запросы без отслеживания с разрешением идентификаторов</span><span class="sxs-lookup"><span data-stu-id="f6d06-271">No-tracking queries with identity resolution</span></span>

<span data-ttu-id="f6d06-272">Теперь можно настроить запросы без отслеживания на выполнение разрешения идентификаторов.</span><span class="sxs-lookup"><span data-stu-id="f6d06-272">No-tracking queries can now be configured to perform identity resolution.</span></span> <span data-ttu-id="f6d06-273">Например, следующий запрос будет создавать новый экземпляр Blog для каждого объекта Post, даже если у каждого Blog один и тот же первичный ключ.</span><span class="sxs-lookup"><span data-stu-id="f6d06-273">For example, the following query will create a new Blog instance for each Post, even if each Blog has the same primary key.</span></span> 

```CSharp
context.Posts.AsNoTracking().Include(e => e.Blog).ToList();
```

<span data-ttu-id="f6d06-274">Однако за счет того, что этот запрос обычно выполняется немного медленнее и всегда использует больше памяти, его можно изменить так, чтобы создавался только один экземпляр Blog:</span><span class="sxs-lookup"><span data-stu-id="f6d06-274">However, at the expense of usually being slightly slower and always using more memory, this query can be changed to ensure only a single Blog instance is created:</span></span>

```CSharp
context.Posts.AsNoTracking().PerformIdentityResolution().Include(e => e.Blog).ToList();
```

<span data-ttu-id="f6d06-275">Обратите внимание, что этот вариант подходит только для запросов без отслеживания, так как все запросы с отслеживанием уже действуют таким образом.</span><span class="sxs-lookup"><span data-stu-id="f6d06-275">Note that this is only useful for no-tracking queries since all tracking queries already exhibit this behavior.</span></span> <span data-ttu-id="f6d06-276">Кроме того, после проверки API будет изменен синтаксис `PerformIdentityResolution`.</span><span class="sxs-lookup"><span data-stu-id="f6d06-276">Also, following API review, the `PerformIdentityResolution` syntax will be changed.</span></span> <span data-ttu-id="f6d06-277">См. [#19877](https://github.com/dotnet/efcore/issues/19877#issuecomment-637371073).</span><span class="sxs-lookup"><span data-stu-id="f6d06-277">See [#19877](https://github.com/dotnet/efcore/issues/19877#issuecomment-637371073).</span></span>

<span data-ttu-id="f6d06-278">Это документируется в проблеме [#1895](https://github.com/dotnet/EntityFramework.Docs/issues/1895).</span><span class="sxs-lookup"><span data-stu-id="f6d06-278">Documentation is tracked by issue [#1895](https://github.com/dotnet/EntityFramework.Docs/issues/1895).</span></span>

### <a name="stored-persisted-computed-columns"></a><span data-ttu-id="f6d06-279">Сохраняемые вычисляемые столбцы</span><span class="sxs-lookup"><span data-stu-id="f6d06-279">Stored (persisted) computed columns</span></span>

<span data-ttu-id="f6d06-280">Большинство баз данных позволяют сохранять значения вычисляемых столбцов после вычислений.</span><span class="sxs-lookup"><span data-stu-id="f6d06-280">Most databases allow computed column values to be stored after computation.</span></span> <span data-ttu-id="f6d06-281">Хотя для этого требуется место на диске, вычисляемый столбец вычисляется только один раз при обновлении, а не при каждом получении его значения.</span><span class="sxs-lookup"><span data-stu-id="f6d06-281">While this takes up disk space, the computed column is calculated only once on update, instead of each time its value is retrieved.</span></span> <span data-ttu-id="f6d06-282">Это также позволяет индексировать столбец для некоторых баз данных.</span><span class="sxs-lookup"><span data-stu-id="f6d06-282">This also allows the column to be indexed for some databases.</span></span>

<span data-ttu-id="f6d06-283">EF Core 5.0 поддерживает настройку вычисляемых столбцов как сохраняемых.</span><span class="sxs-lookup"><span data-stu-id="f6d06-283">EF Core 5.0 allows computed columns to be configured as stored.</span></span> <span data-ttu-id="f6d06-284">Пример:</span><span class="sxs-lookup"><span data-stu-id="f6d06-284">For example:</span></span>
 
```CSharp
modelBuilder
    .Entity<User>()
    .Property(e => e.SomethingComputed)
    .HasComputedColumnSql("my sql", stored: true);
```

### <a name="sqlite-computed-columns"></a><span data-ttu-id="f6d06-285">Вычисляемые столбцы SQLite</span><span class="sxs-lookup"><span data-stu-id="f6d06-285">SQLite computed columns</span></span>

<span data-ttu-id="f6d06-286">EF Core теперь поддерживает вычисляемые столбцы в базах данных SQLite.</span><span class="sxs-lookup"><span data-stu-id="f6d06-286">EF Core now supports computed columns in SQLite databases.</span></span>

## <a name="preview-4"></a><span data-ttu-id="f6d06-287">Предварительная версия 4</span><span class="sxs-lookup"><span data-stu-id="f6d06-287">Preview 4</span></span>

### <a name="configure-database-precisionscale-in-model"></a><span data-ttu-id="f6d06-288">Настройка точности и масштаба для базы данных в модели</span><span class="sxs-lookup"><span data-stu-id="f6d06-288">Configure database precision/scale in model</span></span>

<span data-ttu-id="f6d06-289">Теперь можно указать точность и масштаб для свойства с помощью построителя модели.</span><span class="sxs-lookup"><span data-stu-id="f6d06-289">Precision and scale for a property can now be specified using the model builder.</span></span> <span data-ttu-id="f6d06-290">Пример:</span><span class="sxs-lookup"><span data-stu-id="f6d06-290">For example:</span></span>

```CSharp
modelBuilder
    .Entity<Blog>()
    .Property(b => b.Numeric)
    .HasPrecision(16, 4);
```

<span data-ttu-id="f6d06-291">Точность и масштаб по-прежнему можно задать, используя полный тип базы данных, например "decimal(16,4)".</span><span class="sxs-lookup"><span data-stu-id="f6d06-291">Precision and scale can still be set via the full database type, such as "decimal(16,4)".</span></span> 

<span data-ttu-id="f6d06-292">Документация отслеживается по проблеме [#527](https://github.com/dotnet/EntityFramework.Docs/issues/527).</span><span class="sxs-lookup"><span data-stu-id="f6d06-292">Documentation is tracked by issue [#527](https://github.com/dotnet/EntityFramework.Docs/issues/527).</span></span>

### <a name="specify-sql-server-index-fill-factor"></a><span data-ttu-id="f6d06-293">Указание коэффициента заполнения индекса SQL Server</span><span class="sxs-lookup"><span data-stu-id="f6d06-293">Specify SQL Server index fill factor</span></span>

<span data-ttu-id="f6d06-294">Теперь при создании индекса в SQL Server можно указать коэффициент заполнения.</span><span class="sxs-lookup"><span data-stu-id="f6d06-294">The fill factor can now be specified when creating an index on SQL Server.</span></span> <span data-ttu-id="f6d06-295">Пример:</span><span class="sxs-lookup"><span data-stu-id="f6d06-295">For example:</span></span>

```CSharp
modelBuilder
    .Entity<Customer>()
    .HasIndex(e => e.Name)
    .HasFillFactor(90);
```

## <a name="preview-3"></a><span data-ttu-id="f6d06-296">Предварительная версия 3</span><span class="sxs-lookup"><span data-stu-id="f6d06-296">Preview 3</span></span>

### <a name="filtered-include"></a><span data-ttu-id="f6d06-297">Включение с фильтрацией</span><span class="sxs-lookup"><span data-stu-id="f6d06-297">Filtered Include</span></span>

<span data-ttu-id="f6d06-298">Метод Include теперь поддерживает фильтрацию включенных сущностей.</span><span class="sxs-lookup"><span data-stu-id="f6d06-298">The Include method now supports filtering of the entities included.</span></span> <span data-ttu-id="f6d06-299">Пример:</span><span class="sxs-lookup"><span data-stu-id="f6d06-299">For example:</span></span>

```CSharp
var blogs = context.Blogs
    .Include(e => e.Posts.Where(p => p.Title.Contains("Cheese")))
    .ToList();
```

<span data-ttu-id="f6d06-300">Этот запрос возвращает блоги со всеми связанными записями, но только в том случае, если заголовок записи содержит Cheese.</span><span class="sxs-lookup"><span data-stu-id="f6d06-300">This query will return blogs together with each associated post, but only when the post title contains "Cheese".</span></span>

<span data-ttu-id="f6d06-301">Для сокращения числа включаемых сущностей также можно использовать методы Skip и Take.</span><span class="sxs-lookup"><span data-stu-id="f6d06-301">Skip and Take can also be used to reduce the number of included entities.</span></span> <span data-ttu-id="f6d06-302">Пример:</span><span class="sxs-lookup"><span data-stu-id="f6d06-302">For example:</span></span>
 
```CSharp
var blogs = context.Blogs
    .Include(e => e.Posts.OrderByDescending(post => post.Title).Take(5)))
    .ToList();
```
<span data-ttu-id="f6d06-303">Этот запрос возвратит блоги, включив не более пяти записей для каждого блога.</span><span class="sxs-lookup"><span data-stu-id="f6d06-303">This query will return blogs with at most five posts included on each blog.</span></span>

<span data-ttu-id="f6d06-304">Подробные сведения см. в [документации по методу Include](xref:core/querying/related-data#filtered-include).</span><span class="sxs-lookup"><span data-stu-id="f6d06-304">See the [Include documentation](xref:core/querying/related-data#filtered-include) for full details.</span></span>

### <a name="new-modelbuilder-api-for-navigation-properties"></a><span data-ttu-id="f6d06-305">Новый API ModelBuilder для свойств навигации</span><span class="sxs-lookup"><span data-stu-id="f6d06-305">New ModelBuilder API for navigation properties</span></span>

<span data-ttu-id="f6d06-306">Свойства навигации главным образом настраиваются при [определении связей](xref:core/modeling/relationships).</span><span class="sxs-lookup"><span data-stu-id="f6d06-306">Navigation properties are primarily configured when [defining relationships](xref:core/modeling/relationships).</span></span> <span data-ttu-id="f6d06-307">Однако новый метод `Navigation` можно использовать в случаях, когда свойства навигации требуют дополнительной настройки.</span><span class="sxs-lookup"><span data-stu-id="f6d06-307">However, the new `Navigation` method can be used in the cases where navigation properties need additional configuration.</span></span> <span data-ttu-id="f6d06-308">Например, чтобы задать резервное поле для навигации, если не удалось найти поле по соглашению:</span><span class="sxs-lookup"><span data-stu-id="f6d06-308">For example, to set a backing field for the navigation when the field would not be found by convention:</span></span>

```CSharp
modelBuilder.Entity<Blog>().Navigation(e => e.Posts).HasField("_myposts");
```

<span data-ttu-id="f6d06-309">Обратите внимание, что API `Navigation` не является заменой для конфигурации связи.</span><span class="sxs-lookup"><span data-stu-id="f6d06-309">Note that the `Navigation` API does not replace relationship configuration.</span></span> <span data-ttu-id="f6d06-310">Вместо этого он допускает дополнительную настройку свойств навигации в уже обнаруженных или определенных связях.</span><span class="sxs-lookup"><span data-stu-id="f6d06-310">Instead it allows additional configuration of navigation properties in already discovered or defined relationships.</span></span>

<span data-ttu-id="f6d06-311">См. [документацию по настройке свойств навигации](xref:core/modeling/relationships#configuring-navigation-properties).</span><span class="sxs-lookup"><span data-stu-id="f6d06-311">See the [Configuring Navigation Properties documentation](xref:core/modeling/relationships#configuring-navigation-properties).</span></span>

### <a name="new-command-line-parameters-for-namespaces-and-connection-strings"></a><span data-ttu-id="f6d06-312">Новые параметры командной строки для пространств имен и строк подключения</span><span class="sxs-lookup"><span data-stu-id="f6d06-312">New command-line parameters for namespaces and connection strings</span></span> 

<span data-ttu-id="f6d06-313">Теперь можно указывать пространства имен в командной строке во время миграции и формирования шаблонов.</span><span class="sxs-lookup"><span data-stu-id="f6d06-313">Migrations and scaffolding now allow namespaces to be specified on the command line.</span></span> <span data-ttu-id="f6d06-314">Это можно сделать, например, затем, чтобы реконструировать базу данных, поместив в нее классы контекста и модели в различных пространствах имен:</span><span class="sxs-lookup"><span data-stu-id="f6d06-314">For example, to reverse engineer a database putting the context and model classes in different namespaces:</span></span> 

```
dotnet ef dbcontext scaffold "connection string" Microsoft.EntityFrameworkCore.SqlServer --context-namespace "My.Context" --namespace "My.Model"
```

<span data-ttu-id="f6d06-315">Подробные сведения см. в документации по [миграции](xref:core/managing-schemas/migrations/index#namespaces) и [реконструированию](xref:core/managing-schemas/scaffolding#directories-and-namespaces).</span><span class="sxs-lookup"><span data-stu-id="f6d06-315">See the [Migrations](xref:core/managing-schemas/migrations/index#namespaces) and [Reverse Engineering](xref:core/managing-schemas/scaffolding#directories-and-namespaces) documentation for full details.</span></span>

---
<span data-ttu-id="f6d06-316">Кроме того, теперь можно передать строку подключения команде `database-update`.</span><span class="sxs-lookup"><span data-stu-id="f6d06-316">Also, a connection string can now be passed to the `database-update` command:</span></span>

```
dotnet ef database update --connection "connection string"
```

<span data-ttu-id="f6d06-317">Подробные сведения см. в [документации по средствам](xref:core/miscellaneous/cli/dotnet#dotnet-ef-database-update).</span><span class="sxs-lookup"><span data-stu-id="f6d06-317">See the [Tools documentation](xref:core/miscellaneous/cli/dotnet#dotnet-ef-database-update) for full details.</span></span>

<span data-ttu-id="f6d06-318">Аналогичные параметры также были добавлены в команды PowerShell, используемые в консоли диспетчера пакетов VS.</span><span class="sxs-lookup"><span data-stu-id="f6d06-318">Equivalent parameters have also been added to the PowerShell commands used in the VS Package Manager Console.</span></span>

### <a name="enabledetailederrors-has-returned"></a><span data-ttu-id="f6d06-319">Возвращение EnableDetailedErrors</span><span class="sxs-lookup"><span data-stu-id="f6d06-319">EnableDetailedErrors has returned</span></span>

<span data-ttu-id="f6d06-320">По соображениям производительности EF не выполняет дополнительные проверки на NULL при считывании значений из базы данных.</span><span class="sxs-lookup"><span data-stu-id="f6d06-320">For performance reasons, EF doesn't do additional null-checks when reading values from the database.</span></span> <span data-ttu-id="f6d06-321">Это может привести к возникновению исключений при обнаружении непредвиденных значений NULL, для которых будет сложно определить причину.</span><span class="sxs-lookup"><span data-stu-id="f6d06-321">This can result in exceptions that are hard to root-cause when an unexpected null is encountered.</span></span>

<span data-ttu-id="f6d06-322">При использовании `EnableDetailedErrors` будет добавлена дополнительная проверка на NULL для запросов, чтобы облегчить определение причин этих ошибок за счет небольшого снижения производительности.</span><span class="sxs-lookup"><span data-stu-id="f6d06-322">Using `EnableDetailedErrors` will add extra null checking to queries such that, for a small performance overhead, these errors are easier to trace back to a root cause.</span></span>  

<span data-ttu-id="f6d06-323">Пример:</span><span class="sxs-lookup"><span data-stu-id="f6d06-323">For example:</span></span>
```CSharp
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    => optionsBuilder
        .EnableDetailedErrors()
        .EnableSensitiveDataLogging() // Often also useful with EnableDetailedErrors 
        .UseSqlServer(Your.SqlServerConnectionString);
```

<span data-ttu-id="f6d06-324">Документация отслеживается по проблеме [#955](https://github.com/dotnet/EntityFramework.Docs/issues/955).</span><span class="sxs-lookup"><span data-stu-id="f6d06-324">Documentation is tracked by issue [#955](https://github.com/dotnet/EntityFramework.Docs/issues/955).</span></span>

### <a name="cosmos-partition-keys"></a><span data-ttu-id="f6d06-325">Ключи секций Cosmos</span><span class="sxs-lookup"><span data-stu-id="f6d06-325">Cosmos partition keys</span></span>

<span data-ttu-id="f6d06-326">Теперь можно указать в запросе ключ секции, используемый для данного запроса.</span><span class="sxs-lookup"><span data-stu-id="f6d06-326">The partition key to use for a given query can now be specified in the query.</span></span> <span data-ttu-id="f6d06-327">Пример:</span><span class="sxs-lookup"><span data-stu-id="f6d06-327">For example:</span></span>

```CSharp
await context.Set<Customer>()
             .WithPartitionKey(myPartitionKey)
             .FirstAsync();
```

<span data-ttu-id="f6d06-328">Документация отслеживается по проблеме [#2199](https://github.com/dotnet/EntityFramework.Docs/issues/2199).</span><span class="sxs-lookup"><span data-stu-id="f6d06-328">Documentation is tracked by issue [#2199](https://github.com/dotnet/EntityFramework.Docs/issues/2199).</span></span>

### <a name="support-for-the-sql-server-datalength-function"></a><span data-ttu-id="f6d06-329">Поддержка функции DATALENGTH SQL Server</span><span class="sxs-lookup"><span data-stu-id="f6d06-329">Support for the SQL Server DATALENGTH function</span></span>

<span data-ttu-id="f6d06-330">К ней можно обратиться с помощью нового метода `EF.Functions.DataLength`.</span><span class="sxs-lookup"><span data-stu-id="f6d06-330">This can be accessed using the new `EF.Functions.DataLength` method.</span></span> <span data-ttu-id="f6d06-331">Пример:</span><span class="sxs-lookup"><span data-stu-id="f6d06-331">For example:</span></span>
```CSharp
var count = context.Orders.Count(c => 100 < EF.Functions.DataLength(c.OrderDate));
``` 

## <a name="preview-2"></a><span data-ttu-id="f6d06-332">Предварительная версия 2</span><span class="sxs-lookup"><span data-stu-id="f6d06-332">Preview 2</span></span>

### <a name="use-a-c-attribute-to-specify-a-property-backing-field"></a><span data-ttu-id="f6d06-333">Использование атрибута C# для указания резервного поля свойств</span><span class="sxs-lookup"><span data-stu-id="f6d06-333">Use a C# attribute to specify a property backing field</span></span>

<span data-ttu-id="f6d06-334">Теперь атрибут C# можно использовать для указания резервного поля для свойства.</span><span class="sxs-lookup"><span data-stu-id="f6d06-334">A C# attribute can now be used to specify the backing field for a property.</span></span> <span data-ttu-id="f6d06-335">Этот атрибут позволяет EF Core в обычном порядке выполнять запись и чтение из резервного поля, даже если резервное поле не может быть найдено автоматически.</span><span class="sxs-lookup"><span data-stu-id="f6d06-335">This attribute allows EF Core to still write to and read from the backing field as would normally happen, even when the backing field cannot be found automatically.</span></span> <span data-ttu-id="f6d06-336">Пример:</span><span class="sxs-lookup"><span data-stu-id="f6d06-336">For example:</span></span>

```CSharp
public class Blog
{
    private string _mainTitle;

    public int Id { get; set; }

    [BackingField(nameof(_mainTitle))]
    public string Title
    {
        get => _mainTitle;
        set => _mainTitle = value;
    }
}
```

<span data-ttu-id="f6d06-337">Документация отслеживается по проблеме [#2230](https://github.com/dotnet/EntityFramework.Docs/issues/2230).</span><span class="sxs-lookup"><span data-stu-id="f6d06-337">Documentation is tracked by issue [#2230](https://github.com/dotnet/EntityFramework.Docs/issues/2230).</span></span>

### <a name="complete-discriminator-mapping"></a><span data-ttu-id="f6d06-338">Завершение сопоставления дискриминатора</span><span class="sxs-lookup"><span data-stu-id="f6d06-338">Complete discriminator mapping</span></span>

<span data-ttu-id="f6d06-339">EF Core использует столбец дискриминатора для [сопоставления TPH в иерархии наследования](/ef/core/modeling/inheritance).</span><span class="sxs-lookup"><span data-stu-id="f6d06-339">EF Core uses a discriminator column for [TPH mapping of an inheritance hierarchy](/ef/core/modeling/inheritance).</span></span> <span data-ttu-id="f6d06-340">Некоторые улучшения производительности будут поддерживаться до тех пор, пока EF Core будут известны все возможные значения для дискриминатора.</span><span class="sxs-lookup"><span data-stu-id="f6d06-340">Some performance enhancements are possible so long as EF Core knows all possible values for the discriminator.</span></span> <span data-ttu-id="f6d06-341">Теперь эти улучшения реализованы в EF Core 5.0.</span><span class="sxs-lookup"><span data-stu-id="f6d06-341">EF Core 5.0 now implements these enhancements.</span></span>

<span data-ttu-id="f6d06-342">Например, в предыдущих версиях EF Core всегда создавался следующий код SQL для запроса, возвращающего все типы в иерархии:</span><span class="sxs-lookup"><span data-stu-id="f6d06-342">For example, previous versions of EF Core would always generate this SQL for a query returning all types in a hierarchy:</span></span>

```sql
SELECT [a].[Id], [a].[Discriminator], [a].[Name]
FROM [Animal] AS [a]
WHERE [a].[Discriminator] IN (N'Animal', N'Cat', N'Dog', N'Human')
```

<span data-ttu-id="f6d06-343">Теперь при настройке полного сопоставления дискриминатора EF Core 5.0 будет формировать следующее:</span><span class="sxs-lookup"><span data-stu-id="f6d06-343">EF Core 5.0 will now generate the following when a complete discriminator mapping is configured:</span></span>

```sql
SELECT [a].[Id], [a].[Discriminator], [a].[Name]
FROM [Animal] AS [a]
```

<span data-ttu-id="f6d06-344">Начиная с предварительной версии 3 это поведение будет доступно по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="f6d06-344">It will be the default behavior starting with preview 3.</span></span>

### <a name="performance-improvements-in-microsoftdatasqlite"></a><span data-ttu-id="f6d06-345">Улучшения производительности в Microsoft.Data.Sqlite</span><span class="sxs-lookup"><span data-stu-id="f6d06-345">Performance improvements in Microsoft.Data.Sqlite</span></span>

<span data-ttu-id="f6d06-346">В производительность SQLIte были внесены два улучшения:</span><span class="sxs-lookup"><span data-stu-id="f6d06-346">We have made two performance improvements for SQLIte:</span></span>

* <span data-ttu-id="f6d06-347">Теперь получение двоичных и строковых данных с помощью GetBytes, GetChars и GetTextReader осуществляется более эффективно за счет использования SqliteBlob и потоков.</span><span class="sxs-lookup"><span data-stu-id="f6d06-347">Retrieving binary and string data with GetBytes, GetChars, and GetTextReader is now more efficient by making use of SqliteBlob and streams.</span></span>
* <span data-ttu-id="f6d06-348">Инициализация SqliteConnection выполняется в отложенном режиме.</span><span class="sxs-lookup"><span data-stu-id="f6d06-348">Initialization of SqliteConnection is now lazy.</span></span>

<span data-ttu-id="f6d06-349">Эти улучшения добавлены в поставщик Microsoft.Data.Sqlite ADO.NET и, следовательно, позволяют повысить производительность за пределами EF Core.</span><span class="sxs-lookup"><span data-stu-id="f6d06-349">These improvements are in the ADO.NET Microsoft.Data.Sqlite provider and hence also improve performance outside of EF Core.</span></span>

## <a name="preview-1"></a><span data-ttu-id="f6d06-350">Предварительная версия 1</span><span class="sxs-lookup"><span data-stu-id="f6d06-350">Preview 1</span></span>

### <a name="simple-logging"></a><span data-ttu-id="f6d06-351">Простое ведение журнала</span><span class="sxs-lookup"><span data-stu-id="f6d06-351">Simple logging</span></span>

<span data-ttu-id="f6d06-352">Эта функция добавляет возможность, аналогичную `Database.Log` в EF6.</span><span class="sxs-lookup"><span data-stu-id="f6d06-352">This feature adds functionality similar to `Database.Log` in EF6.</span></span> <span data-ttu-id="f6d06-353">Это значит, что она позволяет легко получать журналы из EF Core без настройки какой-либо внешней платформы ведения журналов.</span><span class="sxs-lookup"><span data-stu-id="f6d06-353">That is, it provides a simple way to get logs from EF Core without the need to configure any kind of external logging framework.</span></span>

<span data-ttu-id="f6d06-354">Предварительная документация включена в раздел с описанием [еженедельного состояния EF на 5 декабря 2019 г](https://github.com/dotnet/efcore/issues/15403#issuecomment-562332863).</span><span class="sxs-lookup"><span data-stu-id="f6d06-354">Preliminary documentation is included in the [EF weekly status for December 5, 2019](https://github.com/dotnet/efcore/issues/15403#issuecomment-562332863).</span></span>

<span data-ttu-id="f6d06-355">Дополнительная документация отслеживается по проблеме [#2085](https://github.com/dotnet/EntityFramework.Docs/issues/2085).</span><span class="sxs-lookup"><span data-stu-id="f6d06-355">Additional documentation is tracked by issue [#2085](https://github.com/dotnet/EntityFramework.Docs/issues/2085).</span></span>

### <a name="simple-way-to-get-generated-sql"></a><span data-ttu-id="f6d06-356">Простой способ получения созданного кода SQL</span><span class="sxs-lookup"><span data-stu-id="f6d06-356">Simple way to get generated SQL</span></span>

<span data-ttu-id="f6d06-357">В EF Core 5.0 представлен метод расширения `ToQueryString`, который будет возвращать код SQL, создаваемый в EF Core при выполнении запроса LINQ.</span><span class="sxs-lookup"><span data-stu-id="f6d06-357">EF Core 5.0 introduces the `ToQueryString` extension method, which will return the SQL that EF Core will generate when executing a LINQ query.</span></span>

<span data-ttu-id="f6d06-358">Предварительная документация включена в раздел с описанием [еженедельного состояния EF на 9 января 2020 г](https://github.com/dotnet/efcore/issues/19549#issuecomment-572823246).</span><span class="sxs-lookup"><span data-stu-id="f6d06-358">Preliminary documentation is included in the [EF weekly status for January 9, 2020](https://github.com/dotnet/efcore/issues/19549#issuecomment-572823246).</span></span>

<span data-ttu-id="f6d06-359">Дополнительная документация отслеживается по проблеме [#1331](https://github.com/dotnet/EntityFramework.Docs/issues/1331).</span><span class="sxs-lookup"><span data-stu-id="f6d06-359">Additional documentation is tracked by issue [#1331](https://github.com/dotnet/EntityFramework.Docs/issues/1331).</span></span>

### <a name="use-a-c-attribute-to-indicate-that-an-entity-has-no-key"></a><span data-ttu-id="f6d06-360">Использование атрибута C# для указания того, что сущность не имеет ключа</span><span class="sxs-lookup"><span data-stu-id="f6d06-360">Use a C# attribute to indicate that an entity has no key</span></span>

<span data-ttu-id="f6d06-361">Теперь можно настроить тип сущности, не имеющей ключа, используя новый атрибут `KeylessAttribute`.</span><span class="sxs-lookup"><span data-stu-id="f6d06-361">An entity type can now be configured as having no key using the new `KeylessAttribute`.</span></span> <span data-ttu-id="f6d06-362">Пример:</span><span class="sxs-lookup"><span data-stu-id="f6d06-362">For example:</span></span>

```CSharp
[Keyless]
public class Address
{
    public string Street { get; set; }
    public string City { get; set; }
    public int Zip { get; set; }
}
```

<span data-ttu-id="f6d06-363">Документация отслеживается по проблеме [#2186](https://github.com/dotnet/EntityFramework.Docs/issues/2186).</span><span class="sxs-lookup"><span data-stu-id="f6d06-363">Documentation is tracked by issue [#2186](https://github.com/dotnet/EntityFramework.Docs/issues/2186).</span></span>

### <a name="connection-or-connection-string-can-be-changed-on-initialized-dbcontext"></a><span data-ttu-id="f6d06-364">Возможность изменения подключения или строки подключения в инициализированном DbContext</span><span class="sxs-lookup"><span data-stu-id="f6d06-364">Connection or connection string can be changed on initialized DbContext</span></span>

<span data-ttu-id="f6d06-365">Теперь создание экземпляра DbContext без подключения или строки подключения стало еще проще.</span><span class="sxs-lookup"><span data-stu-id="f6d06-365">It is now easier to create a DbContext instance without any connection or connection string.</span></span> <span data-ttu-id="f6d06-366">Кроме того, подключение или строку подключения можно изменить в экземпляре контекста.</span><span class="sxs-lookup"><span data-stu-id="f6d06-366">Also, the connection or connection string can now be mutated on the context instance.</span></span> <span data-ttu-id="f6d06-367">Эта возможность позволяет одному и тому же экземпляру контекста динамически подключаться к разным базам данных.</span><span class="sxs-lookup"><span data-stu-id="f6d06-367">This feature allows the same context instance to dynamically connect to different databases.</span></span>

<span data-ttu-id="f6d06-368">Документация отслеживается по проблеме [#2075](https://github.com/dotnet/EntityFramework.Docs/issues/2075).</span><span class="sxs-lookup"><span data-stu-id="f6d06-368">Documentation is tracked by issue [#2075](https://github.com/dotnet/EntityFramework.Docs/issues/2075).</span></span>

### <a name="change-tracking-proxies"></a><span data-ttu-id="f6d06-369">Прокси-серверы отслеживания изменений</span><span class="sxs-lookup"><span data-stu-id="f6d06-369">Change-tracking proxies</span></span>

<span data-ttu-id="f6d06-370">Теперь EF Core может создавать прокси-серверы среды выполнения, которые автоматически реализуют интерфейсы [INotifyPropertyChanging](/dotnet/api/system.componentmodel.inotifypropertychanging?view=netcore-3.1) и [INotifyPropertyChanged](/dotnet/api/system.componentmodel.inotifypropertychanged?view=netcore-3.1).</span><span class="sxs-lookup"><span data-stu-id="f6d06-370">EF Core can now generate runtime proxies that automatically implement [INotifyPropertyChanging](/dotnet/api/system.componentmodel.inotifypropertychanging?view=netcore-3.1) and [INotifyPropertyChanged](/dotnet/api/system.componentmodel.inotifypropertychanged?view=netcore-3.1).</span></span> <span data-ttu-id="f6d06-371">Затем они сообщают об изменениях значений свойств сущности напрямую в EF Core, что позволяет не выполнять проверку на наличие изменений.</span><span class="sxs-lookup"><span data-stu-id="f6d06-371">These then report value changes on entity properties directly to EF Core, avoiding the need to scan for changes.</span></span> <span data-ttu-id="f6d06-372">Однако прокси-серверы имеют собственный набор ограничений, поэтому они предназначены не для всех.</span><span class="sxs-lookup"><span data-stu-id="f6d06-372">However, proxies come with their own set of limitations, so they are not for everyone.</span></span>

<span data-ttu-id="f6d06-373">Документация отслеживается по проблеме [#2076](https://github.com/dotnet/EntityFramework.Docs/issues/2076).</span><span class="sxs-lookup"><span data-stu-id="f6d06-373">Documentation is tracked by issue [#2076](https://github.com/dotnet/EntityFramework.Docs/issues/2076).</span></span>

### <a name="enhanced-debug-views"></a><span data-ttu-id="f6d06-374">Улучшенные представления отладки</span><span class="sxs-lookup"><span data-stu-id="f6d06-374">Enhanced debug views</span></span>

<span data-ttu-id="f6d06-375">В представлениях отладки удобно просматривать внутренние компоненты EF Core при отладке.</span><span class="sxs-lookup"><span data-stu-id="f6d06-375">Debug views are an easy way to look at the internals of EF Core when debugging issues.</span></span> <span data-ttu-id="f6d06-376">Представление отладки для модели было реализовано некоторое время назад.</span><span class="sxs-lookup"><span data-stu-id="f6d06-376">A debug view for the Model was implemented some time ago.</span></span> <span data-ttu-id="f6d06-377">В EF Core 5.0 представление модели стало более удобочитаемым. Кроме того, добавлено новое представление отладки для отслеживаемых сущностей в диспетчере состояний.</span><span class="sxs-lookup"><span data-stu-id="f6d06-377">For EF Core 5.0, we have made the model view easier to read and added a new debug view for tracked entities in the state manager.</span></span>

<span data-ttu-id="f6d06-378">Предварительная документация включена в раздел с описанием [еженедельного состояния EF на 12 декабря 2019 г](https://github.com/dotnet/efcore/issues/15403#issuecomment-565196206).</span><span class="sxs-lookup"><span data-stu-id="f6d06-378">Preliminary documentation is included in the [EF weekly status for December 12, 2019](https://github.com/dotnet/efcore/issues/15403#issuecomment-565196206).</span></span>

<span data-ttu-id="f6d06-379">Дополнительная документация отслеживается по проблеме [#2086](https://github.com/dotnet/EntityFramework.Docs/issues/2086).</span><span class="sxs-lookup"><span data-stu-id="f6d06-379">Additional documentation is tracked by issue [#2086](https://github.com/dotnet/EntityFramework.Docs/issues/2086).</span></span>

### <a name="improved-handling-of-database-null-semantics"></a><span data-ttu-id="f6d06-380">Улучшенная обработка семантики значений NULL в базе данных</span><span class="sxs-lookup"><span data-stu-id="f6d06-380">Improved handling of database null semantics</span></span>

<span data-ttu-id="f6d06-381">Реляционные базы данных обычно обрабатывают NULL как неизвестное значение и, следовательно, как не равное любому другому значению NULL.</span><span class="sxs-lookup"><span data-stu-id="f6d06-381">Relational databases typically treat NULL as an unknown value and therefore not equal to any other NULL.</span></span> <span data-ttu-id="f6d06-382">C# рассматривает NULL как определенное значение, которое аналогично любому другому значению NULL.</span><span class="sxs-lookup"><span data-stu-id="f6d06-382">While C# treats null as a defined value, which compares equal to any other null.</span></span> <span data-ttu-id="f6d06-383">EF Core по умолчанию преобразует запросы так, чтобы они использовали семантику значений NULL C#.</span><span class="sxs-lookup"><span data-stu-id="f6d06-383">EF Core by default translates queries so that they use C# null semantics.</span></span> <span data-ttu-id="f6d06-384">В EF Core 5.0 эффективность таких преобразований значительно повышена.</span><span class="sxs-lookup"><span data-stu-id="f6d06-384">EF Core 5.0 greatly improves the efficiency of these translations.</span></span>

<span data-ttu-id="f6d06-385">Документация отслеживается по проблеме [#1612](https://github.com/dotnet/EntityFramework.Docs/issues/1612).</span><span class="sxs-lookup"><span data-stu-id="f6d06-385">Documentation is tracked by issue [#1612](https://github.com/dotnet/EntityFramework.Docs/issues/1612).</span></span>

### <a name="indexer-properties"></a><span data-ttu-id="f6d06-386">Свойства индексатора</span><span class="sxs-lookup"><span data-stu-id="f6d06-386">Indexer properties</span></span>

<span data-ttu-id="f6d06-387">EF Core 5.0 поддерживает сопоставление свойств индексатора C#.</span><span class="sxs-lookup"><span data-stu-id="f6d06-387">EF Core 5.0 supports mapping of C# indexer properties.</span></span> <span data-ttu-id="f6d06-388">Эти свойства позволяют сущностям выступать в качестве контейнеров свойств, в которых столбцы сопоставляются с именованными свойствами в контейнере.</span><span class="sxs-lookup"><span data-stu-id="f6d06-388">These properties allow entities to act as property bags where columns are mapped to named properties in the bag.</span></span>

<span data-ttu-id="f6d06-389">Документация отслеживается по проблеме [#2018](https://github.com/dotnet/EntityFramework.Docs/issues/2018).</span><span class="sxs-lookup"><span data-stu-id="f6d06-389">Documentation is tracked by issue [#2018](https://github.com/dotnet/EntityFramework.Docs/issues/2018).</span></span>

### <a name="generation-of-check-constraints-for-enum-mappings"></a><span data-ttu-id="f6d06-390">Создание ограничений CHECK для сопоставлений перечислений</span><span class="sxs-lookup"><span data-stu-id="f6d06-390">Generation of check constraints for enum mappings</span></span>

<span data-ttu-id="f6d06-391">Функция миграции в EF Core 5.0 теперь может создавать ограничения CHECK для сопоставлений свойств перечисления.</span><span class="sxs-lookup"><span data-stu-id="f6d06-391">EF Core 5.0 Migrations can now generate CHECK constraints for enum property mappings.</span></span> <span data-ttu-id="f6d06-392">Пример:</span><span class="sxs-lookup"><span data-stu-id="f6d06-392">For example:</span></span>

```SQL
MyEnumColumn VARCHAR(10) NOT NULL CHECK (MyEnumColumn IN ('Useful', 'Useless', 'Unknown'))
```

<span data-ttu-id="f6d06-393">Документация отслеживается по проблеме [#2082](https://github.com/dotnet/EntityFramework.Docs/issues/2082).</span><span class="sxs-lookup"><span data-stu-id="f6d06-393">Documentation is tracked by issue [#2082](https://github.com/dotnet/EntityFramework.Docs/issues/2082).</span></span>

### <a name="isrelational"></a><span data-ttu-id="f6d06-394">Является реляционным</span><span class="sxs-lookup"><span data-stu-id="f6d06-394">IsRelational</span></span>

<span data-ttu-id="f6d06-395">В дополнение к существующим методам `IsSqlServer`, `IsSqlite` и `IsInMemory` был добавлен новый метод `IsRelational`.</span><span class="sxs-lookup"><span data-stu-id="f6d06-395">A new `IsRelational` method has been added in addition to the existing `IsSqlServer`, `IsSqlite`, and `IsInMemory`.</span></span> <span data-ttu-id="f6d06-396">Этот метод можно использовать для проверки того, использует ли DbContext какой-либо поставщик реляционной базы данных.</span><span class="sxs-lookup"><span data-stu-id="f6d06-396">This method can be used to test if the DbContext is using any relational database provider.</span></span> <span data-ttu-id="f6d06-397">Пример:</span><span class="sxs-lookup"><span data-stu-id="f6d06-397">For example:</span></span>

```CSharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    if (Database.IsRelational())
    {
        // Do relational-specific model configuration.
    }
}
```

<span data-ttu-id="f6d06-398">Документация отслеживается по проблеме [#2185](https://github.com/dotnet/EntityFramework.Docs/issues/2185).</span><span class="sxs-lookup"><span data-stu-id="f6d06-398">Documentation is tracked by issue [#2185](https://github.com/dotnet/EntityFramework.Docs/issues/2185).</span></span>

### <a name="cosmos-optimistic-concurrency-with-etags"></a><span data-ttu-id="f6d06-399">Оптимистический параллелизм Cosmos с тегами ETag</span><span class="sxs-lookup"><span data-stu-id="f6d06-399">Cosmos optimistic concurrency with ETags</span></span>

<span data-ttu-id="f6d06-400">Поставщик базы данных Azure Cosmos DB теперь поддерживает оптимистичный параллелизм с использованием тегов ETag.</span><span class="sxs-lookup"><span data-stu-id="f6d06-400">The Azure Cosmos DB database provider now supports optimistic concurrency using ETags.</span></span> <span data-ttu-id="f6d06-401">Используйте построитель моделей в OnModelCreating для настройки ETag:</span><span class="sxs-lookup"><span data-stu-id="f6d06-401">Use the model builder in OnModelCreating to configure an ETag:</span></span>

```CSharp
builder.Entity<Customer>().Property(c => c.ETag).IsEtagConcurrency();
```

<span data-ttu-id="f6d06-402">Затем SaveChanges генерирует исключение `DbUpdateConcurrencyException` при конфликте параллелизма, которое [можно обработать](/ef/core/saving/concurrency) для реализации повторных попыток и т. д.</span><span class="sxs-lookup"><span data-stu-id="f6d06-402">SaveChanges will then throw an `DbUpdateConcurrencyException` on a concurrency conflict, which [can be handled](/ef/core/saving/concurrency) to implement retries, etc.</span></span>

<span data-ttu-id="f6d06-403">Документация отслеживается по проблеме [#2099](https://github.com/dotnet/EntityFramework.Docs/issues/2099).</span><span class="sxs-lookup"><span data-stu-id="f6d06-403">Documentation is tracked by issue [#2099](https://github.com/dotnet/EntityFramework.Docs/issues/2099).</span></span>

### <a name="query-translations-for-more-datetime-constructs"></a><span data-ttu-id="f6d06-404">Преобразования запросов для дополнительных конструкций DateTime</span><span class="sxs-lookup"><span data-stu-id="f6d06-404">Query translations for more DateTime constructs</span></span>

<span data-ttu-id="f6d06-405">Теперь запросы, содержащие новую конструкцию DateTime, поддерживают преобразование.</span><span class="sxs-lookup"><span data-stu-id="f6d06-405">Queries containing new DateTime construction are now translated.</span></span>

<span data-ttu-id="f6d06-406">Кроме того, теперь сопоставлены следующие функции SQL Server:</span><span class="sxs-lookup"><span data-stu-id="f6d06-406">In addition, the following SQL Server functions are now mapped:</span></span>

* <span data-ttu-id="f6d06-407">DateDiffWeek</span><span class="sxs-lookup"><span data-stu-id="f6d06-407">DateDiffWeek</span></span>
* <span data-ttu-id="f6d06-408">DateFromParts</span><span class="sxs-lookup"><span data-stu-id="f6d06-408">DateFromParts</span></span>

<span data-ttu-id="f6d06-409">Пример:</span><span class="sxs-lookup"><span data-stu-id="f6d06-409">For example:</span></span>

```CSharp
var count = context.Orders.Count(c => date > EF.Functions.DateFromParts(DateTime.Now.Year, 12, 25));

```

<span data-ttu-id="f6d06-410">Документация отслеживается по проблеме [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079).</span><span class="sxs-lookup"><span data-stu-id="f6d06-410">Documentation is tracked by issue [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079).</span></span>

### <a name="query-translations-for-more-byte-array-constructs"></a><span data-ttu-id="f6d06-411">Преобразования запросов для дополнительных конструкций массивов байтов</span><span class="sxs-lookup"><span data-stu-id="f6d06-411">Query translations for more byte array constructs</span></span>

<span data-ttu-id="f6d06-412">Запросы, использующие свойства Contains, Length, SequenceEqual и т. д. в массиве byte[], преобразуются в SQL.</span><span class="sxs-lookup"><span data-stu-id="f6d06-412">Queries using Contains, Length, SequenceEqual, etc. on byte[] properties are now translated to SQL.</span></span>

<span data-ttu-id="f6d06-413">Предварительная документация включена в раздел с описанием [еженедельного состояния EF на 5 декабря 2019 г](https://github.com/dotnet/efcore/issues/15403#issuecomment-562332863).</span><span class="sxs-lookup"><span data-stu-id="f6d06-413">Preliminary documentation is included in the [EF weekly status for December 5, 2019](https://github.com/dotnet/efcore/issues/15403#issuecomment-562332863).</span></span>

<span data-ttu-id="f6d06-414">Дополнительная документация отслеживается по проблеме [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079).</span><span class="sxs-lookup"><span data-stu-id="f6d06-414">Additional documentation is tracked by issue [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079).</span></span>

### <a name="query-translation-for-reverse"></a><span data-ttu-id="f6d06-415">Преобразование запросов, использующих Reverse</span><span class="sxs-lookup"><span data-stu-id="f6d06-415">Query translation for Reverse</span></span>

<span data-ttu-id="f6d06-416">Теперь можно преобразовывать запросы, использующие `Reverse`.</span><span class="sxs-lookup"><span data-stu-id="f6d06-416">Queries using `Reverse` are now translated.</span></span> <span data-ttu-id="f6d06-417">Пример:</span><span class="sxs-lookup"><span data-stu-id="f6d06-417">For example:</span></span>

```CSharp
context.Employees.OrderBy(e => e.EmployeeID).Reverse()
```

<span data-ttu-id="f6d06-418">Документация отслеживается по проблеме [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079).</span><span class="sxs-lookup"><span data-stu-id="f6d06-418">Documentation is tracked by issue [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079).</span></span>

### <a name="query-translation-for-bitwise-operators"></a><span data-ttu-id="f6d06-419">Преобразование запросов для побитовых операторов</span><span class="sxs-lookup"><span data-stu-id="f6d06-419">Query translation for bitwise operators</span></span>

<span data-ttu-id="f6d06-420">Запросы, использующие побитовые операторы, теперь преобразуются в ряде дополнительных случаев, например:</span><span class="sxs-lookup"><span data-stu-id="f6d06-420">Queries using bitwise operators are now translated in more cases For example:</span></span>

```CSharp
context.Orders.Where(o => ~o.OrderID == negatedId)
```

<span data-ttu-id="f6d06-421">Документация отслеживается по проблеме [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079).</span><span class="sxs-lookup"><span data-stu-id="f6d06-421">Documentation is tracked by issue [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079).</span></span>

### <a name="query-translation-for-strings-on-cosmos"></a><span data-ttu-id="f6d06-422">Преобразование запросов для строк в Cosmos</span><span class="sxs-lookup"><span data-stu-id="f6d06-422">Query translation for strings on Cosmos</span></span>

<span data-ttu-id="f6d06-423">Запросы, использующие строковые методы Contains, StartsWith и EndsWith, теперь преобразуются при использовании поставщика Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="f6d06-423">Queries that use the string methods Contains, StartsWith, and EndsWith are now translated when using the Azure Cosmos DB provider.</span></span>

<span data-ttu-id="f6d06-424">Документация отслеживается по проблеме [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079).</span><span class="sxs-lookup"><span data-stu-id="f6d06-424">Documentation is tracked by issue [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079).</span></span>
