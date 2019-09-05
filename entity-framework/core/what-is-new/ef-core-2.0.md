---
title: Новые возможности в EF Core 2.0 — EF Core
author: divega
ms.date: 02/20/2018
ms.assetid: 2CB5809E-0EFB-44F6-AF14-9D5BFFFBFF9D
uid: core/what-is-new/ef-core-2.0
ms.openlocfilehash: 28b2180e898b91d233b590b1639674a464f8c679
ms.sourcegitcommit: 0cc9578fd49802789a00c0044b4e57325476ca2e
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/04/2019
ms.locfileid: "70271435"
---
# <a name="new-features-in-ef-core-20"></a><span data-ttu-id="55749-102">Новые возможности в EF Core 2.0</span><span class="sxs-lookup"><span data-stu-id="55749-102">New features in EF Core 2.0</span></span>

## <a name="net-standard-20"></a><span data-ttu-id="55749-103">.NET Standard 2.0</span><span class="sxs-lookup"><span data-stu-id="55749-103">.NET Standard 2.0</span></span>
<span data-ttu-id="55749-104">Сейчас компонент EF Core ориентирован на .NET Standard 2.0, что означает, что он может работать с .NET Core 2.0, .NET Framework 4.6.1 и другими библиотеками, реализующими .NET Standard 2.0.</span><span class="sxs-lookup"><span data-stu-id="55749-104">EF Core now targets .NET Standard 2.0, which means it can work with .NET Core 2.0, .NET Framework 4.6.1, and other libraries that implement .NET Standard 2.0.</span></span>
<span data-ttu-id="55749-105">Дополнительные сведения о поддерживаемых компонентах см. в разделе [Поддерживаемые реализации .NET](../platforms/index.md) .</span><span class="sxs-lookup"><span data-stu-id="55749-105">See [Supported .NET Implementations](../platforms/index.md) for more details on what is supported.</span></span>

## <a name="modeling"></a><span data-ttu-id="55749-106">Моделирование</span><span class="sxs-lookup"><span data-stu-id="55749-106">Modeling</span></span>

### <a name="table-splitting"></a><span data-ttu-id="55749-107">Разбиение таблиц</span><span class="sxs-lookup"><span data-stu-id="55749-107">Table splitting</span></span>

<span data-ttu-id="55749-108">Теперь можно сопоставить два типа сущностей с одной таблицей и более, где столбцы первичного ключа будут общими, а каждая строка будет соответствовать двум сущностям или более.</span><span class="sxs-lookup"><span data-stu-id="55749-108">It is now possible to map two or more entity types to the same table where the primary key column(s) will be shared and each row will correspond to two or more entities.</span></span>

<span data-ttu-id="55749-109">Чтобы использовать разбиение таблиц, нужно настроить идентифицирующее отношение (где свойства внешнего ключа формируют первичный ключ) между всеми типами сущностей, осуществляющими общий доступ к таблице.</span><span class="sxs-lookup"><span data-stu-id="55749-109">To use table splitting an identifying relationship (where foreign key properties form the primary key) must be configured between all of the entity types sharing the table:</span></span>

``` csharp
modelBuilder.Entity<Product>()
    .HasOne(e => e.Details).WithOne(e => e.Product)
    .HasForeignKey<ProductDetails>(e => e.Id);
modelBuilder.Entity<Product>().ToTable("Products");
modelBuilder.Entity<ProductDetails>().ToTable("Products");
```
<span data-ttu-id="55749-110">Дополнительные сведения об этой возможности см. в [разделе, посвященном разделению таблиц](xref:core/modeling/table-splitting).</span><span class="sxs-lookup"><span data-stu-id="55749-110">Read the [section on table splitting](xref:core/modeling/table-splitting) for more information on this feature.</span></span>

### <a name="owned-types"></a><span data-ttu-id="55749-111">Принадлежащие типы</span><span class="sxs-lookup"><span data-stu-id="55749-111">Owned types</span></span>

<span data-ttu-id="55749-112">Принадлежащий тип сущности может использовать один тип среды CLR совместно с другим принадлежащим типом сущности, но так как его невозможно идентифицировать лишь по типу среды CLR, требуется переход к нему от другого типа сущности.</span><span class="sxs-lookup"><span data-stu-id="55749-112">An owned entity type can share the same CLR type with another owned entity type, but since it cannot be identified just by the CLR type there must be a navigation to it from another entity type.</span></span> <span data-ttu-id="55749-113">Сущность, содержащая определяющий переход, является владельцем.</span><span class="sxs-lookup"><span data-stu-id="55749-113">The entity containing the defining navigation is the owner.</span></span> <span data-ttu-id="55749-114">При запросе владельца принадлежащие типы включаются по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="55749-114">When querying the owner the owned types will be included by default.</span></span>

<span data-ttu-id="55749-115">По соглашению, для принадлежащего типа создается теневой первичный ключ, и этот тип сопоставляется с той же таблицей, что и владелец, с помощью разбиения таблицы.</span><span class="sxs-lookup"><span data-stu-id="55749-115">By convention a shadow primary key will be created for the owned type and it will be mapped to the same table as the owner by using table splitting.</span></span> <span data-ttu-id="55749-116">Это позволяет использовать принадлежащие типы аналогично сложным типам в EF6.</span><span class="sxs-lookup"><span data-stu-id="55749-116">This allows to use owned types similarly to how complex types are used in EF6:</span></span>

``` csharp
modelBuilder.Entity<Order>().OwnsOne(p => p.OrderDetails, cb =>
    {
        cb.OwnsOne(c => c.BillingAddress);
        cb.OwnsOne(c => c.ShippingAddress);
    });

public class Order
{
    public int Id { get; set; }
    public OrderDetails OrderDetails { get; set; }
}

public class OrderDetails
{
    public StreetAddress BillingAddress { get; set; }
    public StreetAddress ShippingAddress { get; set; }
}

public class StreetAddress
{
    public string Street { get; set; }
    public string City { get; set; }
}
```
<span data-ttu-id="55749-117">Дополнительные сведения об этой возможности см. в [разделе, посвященном собственным типам сущностей](xref:core/modeling/owned-entities).</span><span class="sxs-lookup"><span data-stu-id="55749-117">Read the [section on owned entity types](xref:core/modeling/owned-entities) for more information on this feature.</span></span>

### <a name="model-level-query-filters"></a><span data-ttu-id="55749-118">Фильтры запросов на уровне модели</span><span class="sxs-lookup"><span data-stu-id="55749-118">Model-level query filters</span></span>

<span data-ttu-id="55749-119">EF Core 2.0 включает новую функцию, которую мы назвали фильтром запросов на уровне модели.</span><span class="sxs-lookup"><span data-stu-id="55749-119">EF Core 2.0 includes a new feature we call Model-level query filters.</span></span> <span data-ttu-id="55749-120">Она позволяет определить предикаты запросов LINQ (логическое выражение, которое обычно передается в оператор запроса LINQ) непосредственно в типах сущностей внутри модели метаданных (обычно в OnModelCreating).</span><span class="sxs-lookup"><span data-stu-id="55749-120">This feature allows LINQ query predicates (a boolean expression typically passed to the LINQ Where query operator) to be defined directly on Entity Types in the metadata model (usually in OnModelCreating).</span></span> <span data-ttu-id="55749-121">Такие фильтры автоматически применяются к любым запросам LINQ, связанным с этими типами сущностей, включая типы сущностей, указанные косвенно, например, с помощью оператора Include или ссылок на свойства прямой навигации.</span><span class="sxs-lookup"><span data-stu-id="55749-121">Such filters are automatically applied to any LINQ queries involving those Entity Types, including Entity Types referenced indirectly, such as through the use of Include or direct navigation property references.</span></span> <span data-ttu-id="55749-122">Ниже приведены некоторые типичные способы применения этой функции.</span><span class="sxs-lookup"><span data-stu-id="55749-122">Some common applications of this feature are:</span></span>

- <span data-ttu-id="55749-123">Обратимое удаление: тип сущности определяет свойство IsDeleted.</span><span class="sxs-lookup"><span data-stu-id="55749-123">Soft delete - An Entity Types defines an IsDeleted property.</span></span>
- <span data-ttu-id="55749-124">Мультитенантность: тип сущности определяет свойство TenantId.</span><span class="sxs-lookup"><span data-stu-id="55749-124">Multi-tenancy - An Entity Type defines a TenantId property.</span></span>

<span data-ttu-id="55749-125">Ниже приведен простой пример, демонстрирующий использование этой функции в двух указанных выше сценариях.</span><span class="sxs-lookup"><span data-stu-id="55749-125">Here is a simple example demonstrating the feature for the two scenarios listed above:</span></span>

``` csharp
public class BloggingContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }
    public DbSet<Post> Posts { get; set; }

    public int TenantId { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Post>().HasQueryFilter(
            p => !p.IsDeleted
            && p.TenantId == this.TenantId );
    }
}
```
<span data-ttu-id="55749-126">Мы определяем фильтр на уровне модели, который реализует мультитенантность и обратимое удаление для экземпляров типа сущности `Post`.</span><span class="sxs-lookup"><span data-stu-id="55749-126">We define a model-level filter that implements multi-tenancy and soft-delete for instances of the `Post` Entity Type.</span></span> <span data-ttu-id="55749-127">Обратите внимание на использование свойства уровня экземпляра DbContext: `TenantId`.</span><span class="sxs-lookup"><span data-stu-id="55749-127">Note the use of a DbContext instance level property: `TenantId`.</span></span> <span data-ttu-id="55749-128">Фильтры на уровне модели будут использовать значение из правильного экземпляра контекста (то есть экземпляра контекста, выполняющего запрос).</span><span class="sxs-lookup"><span data-stu-id="55749-128">Model-level filters will use the value from the correct context instance (that is, the context instance that is executing the query).</span></span>

<span data-ttu-id="55749-129">Можно отключить фильтры для отдельных запросов LINQ с помощью оператора IgnoreQueryFilters().</span><span class="sxs-lookup"><span data-stu-id="55749-129">Filters may be disabled for individual LINQ queries using the IgnoreQueryFilters() operator.</span></span>

#### <a name="limitations"></a><span data-ttu-id="55749-130">Ограничения</span><span class="sxs-lookup"><span data-stu-id="55749-130">Limitations</span></span>

- <span data-ttu-id="55749-131">Ссылки навигации запрещены.</span><span class="sxs-lookup"><span data-stu-id="55749-131">Navigation references are not allowed.</span></span> <span data-ttu-id="55749-132">Эта функция может быть добавлена на основе отзывов.</span><span class="sxs-lookup"><span data-stu-id="55749-132">This feature may be added based on feedback.</span></span>
- <span data-ttu-id="55749-133">Фильтры можно определить только для корневого типа сущности в иерархии.</span><span class="sxs-lookup"><span data-stu-id="55749-133">Filters can only be defined on the root Entity Type of a hierarchy.</span></span>

### <a name="database-scalar-function-mapping"></a><span data-ttu-id="55749-134">Сопоставление скалярных функций базы данных</span><span class="sxs-lookup"><span data-stu-id="55749-134">Database scalar function mapping</span></span>

<span data-ttu-id="55749-135">В EF Core 2.0 представлено важное дополнение от [Пола Миддлтона (Paul Middleton)](https://github.com/pmiddleton), позволяющее сопоставлять скалярные функции базы данных с заглушками методов, чтобы их можно было использовать в запросах LINQ и преобразовывать в SQL.</span><span class="sxs-lookup"><span data-stu-id="55749-135">EF Core 2.0 includes an important contribution from [Paul Middleton](https://github.com/pmiddleton) which enables mapping database scalar functions to method stubs so that they can be used in LINQ queries and translated to SQL.</span></span>

<span data-ttu-id="55749-136">Ниже приведено краткое описание использования этой функции.</span><span class="sxs-lookup"><span data-stu-id="55749-136">Here is a brief description of how the feature can be used:</span></span>

<span data-ttu-id="55749-137">Объявите статический метод для вашего `DbContext` и аннотируйте его с помощью `DbFunctionAttribute`.</span><span class="sxs-lookup"><span data-stu-id="55749-137">Declare a static method on your `DbContext` and annotate it with `DbFunctionAttribute`:</span></span>

``` csharp
public class BloggingContext : DbContext
{
    [DbFunction]
    public static int PostReadCount(int blogId)
    {
        throw new Exception();
    }
}
```

<span data-ttu-id="55749-138">Подобные методы регистрируются автоматически.</span><span class="sxs-lookup"><span data-stu-id="55749-138">Methods like this are automatically registered.</span></span> <span data-ttu-id="55749-139">После регистрации вызовы метода в запросе LINQ могут быть преобразованы в вызовы функций SQL:</span><span class="sxs-lookup"><span data-stu-id="55749-139">Once registered, calls to the method in a LINQ query can be translated to function calls in SQL:</span></span>

``` csharp
var query =
    from p in context.Posts
    where BloggingContext.PostReadCount(p.Id) > 5
    select p;
```

<span data-ttu-id="55749-140">Следует отметить несколько аспектов.</span><span class="sxs-lookup"><span data-stu-id="55749-140">A few things to note:</span></span>

- <span data-ttu-id="55749-141">По соглашению, имя метода используется в качестве имени функции (в данном случае пользовательской) при создании кода SQL, но вы можете переопределить имя и схему во время регистрации метод.</span><span class="sxs-lookup"><span data-stu-id="55749-141">By convention the name of the method is used as the name of a function (in this case a user defined function) when generating the SQL, but you can override the name and schema during method registration</span></span>
- <span data-ttu-id="55749-142">Сейчас поддерживаются только скалярные функции.</span><span class="sxs-lookup"><span data-stu-id="55749-142">Currently only scalar functions are supported</span></span>
- <span data-ttu-id="55749-143">Необходимо создать сопоставимую функцию в базе данных.</span><span class="sxs-lookup"><span data-stu-id="55749-143">You must create the mapped function in the database.</span></span> <span data-ttu-id="55749-144">Она не создается при миграциях EF Core.</span><span class="sxs-lookup"><span data-stu-id="55749-144">EF Core migrations will not take care of creating it</span></span>

### <a name="self-contained-type-configuration-for-code-first"></a><span data-ttu-id="55749-145">Настройка самодостаточных типов для Code First</span><span class="sxs-lookup"><span data-stu-id="55749-145">Self-contained type configuration for code first</span></span>

<span data-ttu-id="55749-146">В EF6 можно было инкапсулировать конфигурацию Code First для определенного типа сущности, создав объект, производный от *EntityTypeConfiguration*.</span><span class="sxs-lookup"><span data-stu-id="55749-146">In EF6 it was possible to encapsulate the code first configuration of a specific entity type by deriving from *EntityTypeConfiguration*.</span></span> <span data-ttu-id="55749-147">В EF Core 2.0 мы возвращаем эту возможность.</span><span class="sxs-lookup"><span data-stu-id="55749-147">In EF Core 2.0 we are bringing this pattern back:</span></span>

``` csharp
class CustomerConfiguration : IEntityTypeConfiguration<Customer>
{
  public void Configure(EntityTypeBuilder<Customer> builder)
  {
     builder.HasKey(c => c.AlternateKey);
     builder.Property(c => c.Name).HasMaxLength(200);
   }
}

...
// OnModelCreating
builder.ApplyConfiguration(new CustomerConfiguration());
```

## <a name="high-performance"></a><span data-ttu-id="55749-148">Высокая производительность</span><span class="sxs-lookup"><span data-stu-id="55749-148">High Performance</span></span>

### <a name="dbcontext-pooling"></a><span data-ttu-id="55749-149">Создание пулов DbContext</span><span class="sxs-lookup"><span data-stu-id="55749-149">DbContext pooling</span></span>

<span data-ttu-id="55749-150">Типичное использование EF Core в приложении ASP.NET Core обычно включает в себя регистрацию пользовательского типа DbContext в системе внедрения зависимостей и последующее получение экземпляров этого типа через параметры конструктора в контроллерах.</span><span class="sxs-lookup"><span data-stu-id="55749-150">The basic pattern for using EF Core in an ASP.NET Core application usually involves registering a custom DbContext type into the dependency injection system and later obtaining instances of that type through constructor parameters in controllers.</span></span> <span data-ttu-id="55749-151">Это означает, что для каждого из запросов создается экземпляр DbContext.</span><span class="sxs-lookup"><span data-stu-id="55749-151">This means a new instance of the DbContext is created for each requests.</span></span>

<span data-ttu-id="55749-152">В версии 2.0 мы предлагаем новый способ регистрации пользовательских типов DbContext в системе внедрения зависимостей, который прозрачно вводит в обиход пул многоразовых экземпляров DbContext.</span><span class="sxs-lookup"><span data-stu-id="55749-152">In version 2.0 we are introducing a new way to register custom DbContext types in dependency injection which transparently introduces a pool of reusable DbContext instances.</span></span> <span data-ttu-id="55749-153">Для работы с пулами DbContext используйте `AddDbContextPool` вместо `AddDbContext` во время регистрации службы.</span><span class="sxs-lookup"><span data-stu-id="55749-153">To use DbContext pooling, use the `AddDbContextPool` instead of `AddDbContext` during service registration:</span></span>

``` csharp
services.AddDbContextPool<BloggingContext>(
    options => options.UseSqlServer(connectionString));
```

<span data-ttu-id="55749-154">Если применяется этот метод, при запросе экземпляра DbContext контроллером мы сначала проверим, доступен ли этот экземпляр в пуле.</span><span class="sxs-lookup"><span data-stu-id="55749-154">If this method is used, at the time a DbContext instance is requested by a controller we will first check if there is an instance available in the pool.</span></span> <span data-ttu-id="55749-155">После завершения обработки запроса любое состояние экземпляра сбрасывается, а сам экземпляр возвращается в пул.</span><span class="sxs-lookup"><span data-stu-id="55749-155">Once the request processing finalizes, any state on the instance is reset and the instance is itself returned to the pool.</span></span>

<span data-ttu-id="55749-156">По своему принципу этот подход похож на работу пулов подключений в поставщиках ADO.NET, кроме того, он выгоден с позиции снижения затрат на инициализацию экземпляра DbContext.</span><span class="sxs-lookup"><span data-stu-id="55749-156">This is conceptually similar to how connection pooling operates in ADO.NET providers and has the advantage of saving some of the cost of initialization of DbContext instance.</span></span>

### <a name="limitations"></a><span data-ttu-id="55749-157">Ограничения</span><span class="sxs-lookup"><span data-stu-id="55749-157">Limitations</span></span>

<span data-ttu-id="55749-158">Новый метод накладывает ряд ограничений на то, что можно сделать в методе `OnConfiguring()` DbContext.</span><span class="sxs-lookup"><span data-stu-id="55749-158">The new method introduces a few limitations on what can be done in the `OnConfiguring()` method of the DbContext.</span></span>

> [!WARNING]  
> <span data-ttu-id="55749-159">Избегайте создания пулов DbContext, если используете собственное состояние (например, закрытые поля) в производном классе DbContext, которое не должно совместно использоваться разными запросами.</span><span class="sxs-lookup"><span data-stu-id="55749-159">Avoid using DbContext Pooling if you maintain your own state (for example, private fields) in your derived DbContext class that should not be shared across requests.</span></span> <span data-ttu-id="55749-160">EF Core сбросит только состояние, о котором известно перед добавлением экземпляра DbContext в пул.</span><span class="sxs-lookup"><span data-stu-id="55749-160">EF Core will only reset the state that is aware of before adding a DbContext instance to the pool.</span></span>

### <a name="explicitly-compiled-queries"></a><span data-ttu-id="55749-161">Явным образом скомпилированные запросы</span><span class="sxs-lookup"><span data-stu-id="55749-161">Explicitly compiled queries</span></span>

<span data-ttu-id="55749-162">Это вторя функция обеспечения производительности, предоставляющая преимущества в крупномасштабных сценариях.</span><span class="sxs-lookup"><span data-stu-id="55749-162">This is the second opt-in performance feature designed to offer benefits in high-scale scenarios.</span></span>

<span data-ttu-id="55749-163">API вручную или явно скомпилированных запросов были доступны в предыдущих версиях EF, а также в LINQ to SQL, позволяя приложениям кэшировать преобразование запросов, чтобы их можно было вычислить всего один раз, а выполнять многократно.</span><span class="sxs-lookup"><span data-stu-id="55749-163">Manual or explicitly compiled query APIs have been available in previous versions of EF and also in LINQ to SQL to allow applications to cache the translation of queries so that they can be computed only once and executed many times.</span></span>

<span data-ttu-id="55749-164">Хотя в общем случае EF Core может автоматически компилировать и кэшировать запросы на основе хэшированного представления выражений запросов, этот механизм можно использовать для небольшого повышения производительности за счет обхода вычисления хэша и поиска по нему, что позволяет приложению использовать уже скомпилированный запрос с помощью вызова делегата.</span><span class="sxs-lookup"><span data-stu-id="55749-164">Although in general EF Core can automatically compile and cache queries based on a hashed representation of the query expressions, this mechanism can be used to obtain a small performance gain by bypassing the computation of the hash and the cache lookup, allowing the application to use an already compiled query through the invocation of a delegate.</span></span>

``` csharp
// Create an explicitly compiled query
private static Func<CustomerContext, int, Customer> _customerById =
    EF.CompileQuery((CustomerContext db, int id) =>
        db.Customers
            .Include(c => c.Address)
            .Single(c => c.Id == id));

// Use the compiled query by invoking it
using (var db = new CustomerContext())
{
   var customer = _customerById(db, 147);
}
```

## <a name="change-tracking"></a><span data-ttu-id="55749-165">Отслеживание изменений</span><span class="sxs-lookup"><span data-stu-id="55749-165">Change Tracking</span></span>

### <a name="attach-can-track-a-graph-of-new-and-existing-entities"></a><span data-ttu-id="55749-166">Attach может отслеживать граф новых и существующих сущностей.</span><span class="sxs-lookup"><span data-stu-id="55749-166">Attach can track a graph of new and existing entities</span></span>

<span data-ttu-id="55749-167">EF Core поддерживает автоматическое формирование значений ключа посредством разнообразных механизмов.</span><span class="sxs-lookup"><span data-stu-id="55749-167">EF Core supports automatic generation of key values through a variety of mechanisms.</span></span> <span data-ttu-id="55749-168">При использовании этой функции значение создается, если для свойства ключа задано значение по умолчанию для среды CLR, которое обычно равно нулю или NULL.</span><span class="sxs-lookup"><span data-stu-id="55749-168">When using this feature, a value is generated if the key property is the CLR default--usually zero or null.</span></span> <span data-ttu-id="55749-169">Это означает, что граф сущностей можно передать в `DbContext.Attach` или `DbSet.Attach`, и EF Core пометит сущности, для которых ключ уже задан, как `Unchanged`, а сущности без набора ключей — как `Added`.</span><span class="sxs-lookup"><span data-stu-id="55749-169">This means that a graph of entities can be passed to `DbContext.Attach` or `DbSet.Attach` and EF Core will mark those entities that have a key already set as `Unchanged` while those entities that do not have a key set will be marked as `Added`.</span></span> <span data-ttu-id="55749-170">Это упрощает вложение смешанных графов из новых и существующих сущностей при использовании сформированных ключей.</span><span class="sxs-lookup"><span data-stu-id="55749-170">This makes it easy to attach a graph of mixed new and existing entities when using generated keys.</span></span> <span data-ttu-id="55749-171">`DbContext.Update`и `DbSet.Update` работают аналогично, за исключением того, что сущности с набором ключей помечаются как `Modified` вместо `Unchanged`.</span><span class="sxs-lookup"><span data-stu-id="55749-171">`DbContext.Update` and `DbSet.Update` work in the same way, except that entities with a key set are marked as `Modified` instead of `Unchanged`.</span></span>

## <a name="query"></a><span data-ttu-id="55749-172">Запрос</span><span class="sxs-lookup"><span data-stu-id="55749-172">Query</span></span>

### <a name="improved-linq-translation"></a><span data-ttu-id="55749-173">Усовершенствованное преобразование LINQ</span><span class="sxs-lookup"><span data-stu-id="55749-173">Improved LINQ Translation</span></span>

<span data-ttu-id="55749-174">Позволяет успешно выполнять больше запросов, увеличив при этом долю логики, вычисляемой в базе данных (а не в памяти) и уменьшив объем ненужных данных, извлекаемых из базы данных.</span><span class="sxs-lookup"><span data-stu-id="55749-174">Enables more queries to successfully execute, with more logic being evaluated in the database (rather than in-memory) and less data unnecessarily being retrieved from the database.</span></span>

### <a name="groupjoin-improvements"></a><span data-ttu-id="55749-175">Усовершенствования объединений групп</span><span class="sxs-lookup"><span data-stu-id="55749-175">GroupJoin improvements</span></span>

<span data-ttu-id="55749-176">Эта работа улучшает код SQL, создаваемый для объединений групп.</span><span class="sxs-lookup"><span data-stu-id="55749-176">This work improves the SQL that is generated for group joins.</span></span> <span data-ttu-id="55749-177">Чаще всего объединения групп возникают в результате выполнения вложенных запросов для необязательных свойств навигации.</span><span class="sxs-lookup"><span data-stu-id="55749-177">Group joins are most often a result of sub-queries on optional navigation properties.</span></span>

### <a name="string-interpolation-in-fromsql-and-executesqlcommand"></a><span data-ttu-id="55749-178">Интерполяция строк в FromSql и ExecuteSqlCommand</span><span class="sxs-lookup"><span data-stu-id="55749-178">String interpolation in FromSql and ExecuteSqlCommand</span></span>

<span data-ttu-id="55749-179">В C# 6 появилась интерполяция строк, позволяющая внедрять выражения C# непосредственно в строковые литералы, что очень удобно для создания строк во время выполнения.</span><span class="sxs-lookup"><span data-stu-id="55749-179">C# 6 introduced String Interpolation, a feature that allows C# expressions to be directly embedded in string literals, providing a nice way of building strings at runtime.</span></span> <span data-ttu-id="55749-180">В EF Core 2.0 мы добавили специальную поддержку интерполированных строк в два основных API, принимающих необработанные строки SQL: `FromSql` и `ExecuteSqlCommand`.</span><span class="sxs-lookup"><span data-stu-id="55749-180">In EF Core 2.0 we added special support for interpolated strings to our two primary APIs that accept raw SQL strings: `FromSql` and `ExecuteSqlCommand`.</span></span> <span data-ttu-id="55749-181">Это обеспечивает безопасное использование интерполяции строк C#.</span><span class="sxs-lookup"><span data-stu-id="55749-181">This new support allows C# string interpolation to be used in a 'safe' manner.</span></span> <span data-ttu-id="55749-182">То есть этот механизм защищает от типичных ошибок внедрения кода SQL, которые могут возникнуть при динамическом формировании кода SQL во время выполнения.</span><span class="sxs-lookup"><span data-stu-id="55749-182">That is, in a way that protects against common SQL injection mistakes that can occur when dynamically constructing SQL at runtime.</span></span>

<span data-ttu-id="55749-183">Пример:</span><span class="sxs-lookup"><span data-stu-id="55749-183">Here is an example:</span></span>

``` csharp
var city = "London";
var contactTitle = "Sales Representative";

using (var context = CreateContext())
{
    context.Set<Customer>()
        .FromSql($@"
            SELECT *
            FROM ""Customers""
            WHERE ""City"" = {city} AND
                ""ContactTitle"" = {contactTitle}")
            .ToArray();
  }
```

<span data-ttu-id="55749-184">В этом примере в строку формата SQL внедрены две переменные.</span><span class="sxs-lookup"><span data-stu-id="55749-184">In this example, there are two variables embedded in the SQL format string.</span></span> <span data-ttu-id="55749-185">EF Core выдаст следующий код SQL:</span><span class="sxs-lookup"><span data-stu-id="55749-185">EF Core will produce the following SQL:</span></span>

```sql
@p0='London' (Size = 4000)
@p1='Sales Representative' (Size = 4000)

SELECT *
FROM ""Customers""
WHERE ""City"" = @p0
    AND ""ContactTitle"" = @p1
```

### <a name="effunctionslike"></a><span data-ttu-id="55749-186">EF.Functions.Like()</span><span class="sxs-lookup"><span data-stu-id="55749-186">EF.Functions.Like()</span></span>

<span data-ttu-id="55749-187">Мы добавили свойство EF.Functions, которое EF Core или поставщики могут использовать для определения методов, соответствующих операторам или функциям базы данных, чтобы их можно вызывать в запросах LINQ.</span><span class="sxs-lookup"><span data-stu-id="55749-187">We have added the EF.Functions property which can be used by EF Core or providers to define methods that map to database functions or operators so that those can be invoked in LINQ queries.</span></span> <span data-ttu-id="55749-188">Первым примером такого метода является Like().</span><span class="sxs-lookup"><span data-stu-id="55749-188">The first example of such a method is Like():</span></span>

``` csharp
var aCustomers =
    from c in context.Customers
    where EF.Functions.Like(c.Name, "a%")
    select c;
```

<span data-ttu-id="55749-189">Обратите внимание, что Like() предоставляется в реализации для выполнения в памяти, что может быть удобно при работе с выполняющейся в памяти базой данных или при вычислении предиката на стороне клиента.</span><span class="sxs-lookup"><span data-stu-id="55749-189">Note that Like() comes with an in-memory implementation, which can be handy when working against an in-memory database or when evaluation of the predicate needs to occur on the client side.</span></span>

## <a name="database-management"></a><span data-ttu-id="55749-190">Управление базами данных</span><span class="sxs-lookup"><span data-stu-id="55749-190">Database management</span></span>

### <a name="pluralization-hook-for-dbcontext-scaffolding"></a><span data-ttu-id="55749-191">Обработчик преобразования во множественную форму для формирования DbContext</span><span class="sxs-lookup"><span data-stu-id="55749-191">Pluralization hook for DbContext Scaffolding</span></span>

<span data-ttu-id="55749-192">В EF Core 2.0 появилась новая служба *IPluralizer*, используемая для преобразования имен типов сущности в единственное число и имен DbSet — во множественное.</span><span class="sxs-lookup"><span data-stu-id="55749-192">EF Core 2.0 introduces a new *IPluralizer* service that is used to singularize entity type names and pluralize DbSet names.</span></span> <span data-ttu-id="55749-193">Реализация по умолчанию является холостой, то есть это просто обработчик, куда можно подключить свой собственный преобразователь во множественное число.</span><span class="sxs-lookup"><span data-stu-id="55749-193">The default implementation is a no-op, so this is just a hook where folks can easily plug in their own pluralizer.</span></span>

<span data-ttu-id="55749-194">Вот как разработчик может подключить свой собственный преобразователь во множественное число.</span><span class="sxs-lookup"><span data-stu-id="55749-194">Here is what it looks like for a developer to hook in their own pluralizer:</span></span>

``` csharp
public class MyDesignTimeServices : IDesignTimeServices
{
    public void ConfigureDesignTimeServices(IServiceCollection services)
    {
        services.AddSingleton<IPluralizer, MyPluralizer>();
    }
}

public class MyPluralizer : IPluralizer
{
    public string Pluralize(string name)
    {
        return Inflector.Inflector.Pluralize(name) ?? name;
    }

    public string Singularize(string name)
    {
        return Inflector.Inflector.Singularize(name) ?? name;
    }
}
```

## <a name="others"></a><span data-ttu-id="55749-195">Другие</span><span class="sxs-lookup"><span data-stu-id="55749-195">Others</span></span>

### <a name="move-adonet-sqlite-provider-to-sqlitepclraw"></a><span data-ttu-id="55749-196">Перемещение поставщика ADO.NET SQLite в SQLitePCL.raw</span><span class="sxs-lookup"><span data-stu-id="55749-196">Move ADO.NET SQLite provider to SQLitePCL.raw</span></span>
<span data-ttu-id="55749-197">Это позволяет получить более надежное решение в Microsoft.Data.Sqlite для распространения собственных двоичных файлов SQLite на разных платформах.</span><span class="sxs-lookup"><span data-stu-id="55749-197">This gives us a more robust solution in Microsoft.Data.Sqlite for distributing native SQLite binaries on different platforms.</span></span>

### <a name="only-one-provider-per-model"></a><span data-ttu-id="55749-198">Всего один поставщик на модель</span><span class="sxs-lookup"><span data-stu-id="55749-198">Only one provider per model</span></span>
<span data-ttu-id="55749-199">Значительно расширяет возможности взаимодействия поставщиков с моделью и упрощает использование соглашений, заметок и текучих API с разными поставщиками.</span><span class="sxs-lookup"><span data-stu-id="55749-199">Significantly augments how providers can interact with the model and simplifies how conventions, annotations and fluent APIs work with different providers.</span></span>

<span data-ttu-id="55749-200">EF Core 2.0 теперь будет создавать отдельный объект [IModel](https://github.com/aspnet/EntityFramework/blob/master/src/EFCore/Metadata/IModel.cs) для каждого из используемых поставщиков.</span><span class="sxs-lookup"><span data-stu-id="55749-200">EF Core 2.0 will now build a different [IModel](https://github.com/aspnet/EntityFramework/blob/master/src/EFCore/Metadata/IModel.cs) for each different provider being used.</span></span> <span data-ttu-id="55749-201">Обычно это является прозрачным для приложения.</span><span class="sxs-lookup"><span data-stu-id="55749-201">This is usually transparent to the application.</span></span> <span data-ttu-id="55749-202">Это позволило упростить работу с API метаданных более низкого уровня, например, добиться того, что любое обращение к *основным концепциям реляционных метаданных* всегда осуществляется путем вызова `.Relational` вместо `.SqlServer`, `.Sqlite` и т. д.</span><span class="sxs-lookup"><span data-stu-id="55749-202">This has facilitated a simplification of lower-level metadata APIs such that any access to *common relational metadata concepts* is always made through a call to `.Relational` instead of `.SqlServer`, `.Sqlite`, etc.</span></span>

### <a name="consolidated-logging-and-diagnostics"></a><span data-ttu-id="55749-203">Консолидация ведения журнала и диагностики</span><span class="sxs-lookup"><span data-stu-id="55749-203">Consolidated Logging and Diagnostics</span></span>

<span data-ttu-id="55749-204">Механизмы ведения журнала (на основе ILogger) и диагностики (на основе DiagnosticSource) теперь используют еще больше общего кода.</span><span class="sxs-lookup"><span data-stu-id="55749-204">Logging (based on ILogger) and Diagnostics (based on DiagnosticSource) mechanisms now share more code.</span></span>

<span data-ttu-id="55749-205">В версии 2.0 были изменены идентификаторы событий для сообщений, отправляемых в ILogger.</span><span class="sxs-lookup"><span data-stu-id="55749-205">The event IDs for messages sent to an ILogger have changed in 2.0.</span></span> <span data-ttu-id="55749-206">Теперь эти идентификаторы событий являются уникальными в рамках всего кода EF Core.</span><span class="sxs-lookup"><span data-stu-id="55749-206">The event IDs are now unique across EF Core code.</span></span> <span data-ttu-id="55749-207">Эти сообщения теперь соответствуют стандартному шаблону для структурированного ведения журнала, используемому, например, MVC.</span><span class="sxs-lookup"><span data-stu-id="55749-207">These messages now also follow the standard pattern for structured logging used by, for example, MVC.</span></span>

<span data-ttu-id="55749-208">Кроме того, были изменены категории средства ведения журнала.</span><span class="sxs-lookup"><span data-stu-id="55749-208">Logger categories have also changed.</span></span> <span data-ttu-id="55749-209">Теперь используется известный набор категорий, доступных через [DbLoggerCategory](https://github.com/aspnet/EntityFramework/blob/master/src/EFCore/DbLoggerCategory.cs).</span><span class="sxs-lookup"><span data-stu-id="55749-209">There is now a well-known set of categories accessed through [DbLoggerCategory](https://github.com/aspnet/EntityFramework/blob/master/src/EFCore/DbLoggerCategory.cs).</span></span>

<span data-ttu-id="55749-210">События DiagnosticSource теперь используют те же имена идентификаторов событий, что и соответствующие сообщения `ILogger`.</span><span class="sxs-lookup"><span data-stu-id="55749-210">DiagnosticSource events now use the same event ID names as the corresponding `ILogger` messages.</span></span>
