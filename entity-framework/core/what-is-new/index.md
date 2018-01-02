---
title: "Новые возможности в EF Core 2.0 — EF Core"
author: divega
ms.author: divega
ms.date: 10/27/2016
ms.assetid: C21F89EE-FB08-4ED9-A2A0-76CB7656E6E4
ms.technology: entity-framework-core
uid: core/what-is-new/index
ms.openlocfilehash: a0e54fe85e810793913b9c5fb5e745419a0983b2
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/27/2017
---
# <a name="new-features-in-ef-core-20"></a><span data-ttu-id="8f6aa-102">Новые возможности в EF Core 2.0</span><span class="sxs-lookup"><span data-stu-id="8f6aa-102">New features in EF Core 2.0</span></span>

## <a name="net-standard-20"></a><span data-ttu-id="8f6aa-103">.NET Standard 2.0</span><span class="sxs-lookup"><span data-stu-id="8f6aa-103">.NET Standard 2.0</span></span>
<span data-ttu-id="8f6aa-104">Сейчас компонент EF Core ориентирован на .NET Standard 2.0, что означает, что он может работать с .NET Core 2.0, .NET Framework 4.6.1 и другими библиотеками, реализующими .NET Standard 2.0.</span><span class="sxs-lookup"><span data-stu-id="8f6aa-104">EF Core now targets .NET Standard 2.0, which means it can work with .NET Core 2.0, .NET Framework 4.6.1, and other libraries that implement .NET Standard 2.0.</span></span>
<span data-ttu-id="8f6aa-105">Дополнительные сведения о поддерживаемых компонентах см. в разделе [Поддерживаемые реализации .NET](../platforms/index.md) .</span><span class="sxs-lookup"><span data-stu-id="8f6aa-105">See [Supported .NET Implementations](../platforms/index.md) for more details on what is supported.</span></span>

## <a name="modeling"></a><span data-ttu-id="8f6aa-106">Моделирование</span><span class="sxs-lookup"><span data-stu-id="8f6aa-106">Modeling</span></span>

### <a name="table-splitting"></a><span data-ttu-id="8f6aa-107">Разбиение таблиц</span><span class="sxs-lookup"><span data-stu-id="8f6aa-107">Table splitting</span></span>

<span data-ttu-id="8f6aa-108">Теперь можно сопоставить два типа сущностей с одной таблицей и более, где столбцы первичного ключа будут общими, а каждая строка будет соответствовать двум сущностям или более.</span><span class="sxs-lookup"><span data-stu-id="8f6aa-108">It is now possible to map two or more entity types to the same table where the primary key column(s) will be shared and each row will correspond to two or more entities.</span></span>

<span data-ttu-id="8f6aa-109">Чтобы использовать разбиение таблиц, нужно настроить идентифицирующее отношение (где свойства внешнего ключа формируют первичный ключ) между всеми типами сущностей, осуществляющими общий доступ к таблице.</span><span class="sxs-lookup"><span data-stu-id="8f6aa-109">To use table splitting an identifying relationship (where foreign key properties form the primary key) must be configured between all of the entity types sharing the table:</span></span>

``` csharp
modelBuilder.Entity<Product>()
    .HasOne(e => e.Details).WithOne(e => e.Product)
    .HasForeignKey<ProductDetails>(e => e.Id);
modelBuilder.Entity<Product>().ToTable("Products");
modelBuilder.Entity<ProductDetails>().ToTable("Products");
```

### <a name="owned-types"></a><span data-ttu-id="8f6aa-110">Принадлежащие типы</span><span class="sxs-lookup"><span data-stu-id="8f6aa-110">Owned types</span></span>

<span data-ttu-id="8f6aa-111">Принадлежащий тип сущности может использовать один тип среды CLR совместно с другим принадлежащим типом сущности, но так как его невозможно идентифицировать лишь по типу среды CLR, требуется переход к нему от другого типа сущности.</span><span class="sxs-lookup"><span data-stu-id="8f6aa-111">An owned entity type can share the same CLR type with another owned entity type, but since it cannot be identified just by the CLR type there must be a navigation to it from another entity type.</span></span> <span data-ttu-id="8f6aa-112">Сущность, содержащая определяющий переход, является владельцем.</span><span class="sxs-lookup"><span data-stu-id="8f6aa-112">The entity containing the defining navigation is the owner.</span></span> <span data-ttu-id="8f6aa-113">При запросе владельца принадлежащие типы включаются по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="8f6aa-113">When querying the owner the owned types will be included by default.</span></span>

<span data-ttu-id="8f6aa-114">По соглашению, для принадлежащего типа создается теневой первичный ключ, и этот тип сопоставляется с той же таблицей, что и владелец, с помощью разбиения таблицы.</span><span class="sxs-lookup"><span data-stu-id="8f6aa-114">By convention a shadow primary key will be created for the owned type and it will be mapped to the same table as the owner by using table splitting.</span></span> <span data-ttu-id="8f6aa-115">Это позволяет использовать принадлежащие типы аналогично сложным типам в EF6.</span><span class="sxs-lookup"><span data-stu-id="8f6aa-115">This allows to use owned types similarly to how complex types are used in EF6:</span></span>

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

### <a name="model-level-query-filters"></a><span data-ttu-id="8f6aa-116">Фильтры запросов на уровне модели</span><span class="sxs-lookup"><span data-stu-id="8f6aa-116">Model-level query filters</span></span>

<span data-ttu-id="8f6aa-117">EF Core 2.0 включает новую функцию, которую мы назвали фильтром запросов на уровне модели.</span><span class="sxs-lookup"><span data-stu-id="8f6aa-117">EF Core 2.0 includes a new feature we call Model-level query filters.</span></span> <span data-ttu-id="8f6aa-118">Она позволяет определить предикаты запросов LINQ (логическое выражение, которое обычно передается в оператор запроса LINQ) непосредственно в типах сущностей внутри модели метаданных (обычно в OnModelCreating).</span><span class="sxs-lookup"><span data-stu-id="8f6aa-118">This feature allows LINQ query predicates (a boolean expression typically passed to the LINQ Where query operator) to be defined directly on Entity Types in the metadata model (usually in OnModelCreating).</span></span> <span data-ttu-id="8f6aa-119">Такие фильтры автоматически применяются к любым запросам LINQ, связанным с этими типами сущностей, включая типы сущностей, указанные косвенно, например, с помощью оператора Include или ссылок на свойства прямой навигации.</span><span class="sxs-lookup"><span data-stu-id="8f6aa-119">Such filters are automatically applied to any LINQ queries involving those Entity Types, including Entity Types referenced indirectly, such as through the use of Include or direct navigation property references.</span></span> <span data-ttu-id="8f6aa-120">Ниже приведены некоторые типичные способы применения этой функции.</span><span class="sxs-lookup"><span data-stu-id="8f6aa-120">Some common applications of this feature are:</span></span>

- <span data-ttu-id="8f6aa-121">Обратимое удаление: тип сущности определяет свойство IsDeleted.</span><span class="sxs-lookup"><span data-stu-id="8f6aa-121">Soft delete - An Entity Types defines an IsDeleted property.</span></span>
- <span data-ttu-id="8f6aa-122">Мультитенантность: тип сущности определяет свойство TenantId.</span><span class="sxs-lookup"><span data-stu-id="8f6aa-122">Multi-tenancy - An Entity Type defines a TenantId property.</span></span>

<span data-ttu-id="8f6aa-123">Ниже приведен простой пример, демонстрирующий использование этой функции в двух указанных выше сценариях.</span><span class="sxs-lookup"><span data-stu-id="8f6aa-123">Here is a simple example demonstrating the feature for the two scenarios listed above:</span></span>

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
<span data-ttu-id="8f6aa-124">Мы определяем фильтр на уровне модели, который реализует мультитенантность и обратимое удаление для экземпляров типа сущности ```Post```.</span><span class="sxs-lookup"><span data-stu-id="8f6aa-124">We define a model-level filter that implements multi-tenancy and soft-delete for instances of the ```Post``` Entity Type.</span></span> <span data-ttu-id="8f6aa-125">Обратите внимание на использование свойства уровня экземпляра DbContext: ```TenantId```.</span><span class="sxs-lookup"><span data-stu-id="8f6aa-125">Note the use of a DbContext instance level property: ```TenantId```.</span></span> <span data-ttu-id="8f6aa-126">Фильтры на уровне модели будут использовать значение из правильного экземпляра контекста.</span><span class="sxs-lookup"><span data-stu-id="8f6aa-126">Model-level filters will use the value from the correct context instance.</span></span> <span data-ttu-id="8f6aa-127">То есть того, который выполняет этот запрос.</span><span class="sxs-lookup"><span data-stu-id="8f6aa-127">I.e. the one that is executing the query.</span></span>

<span data-ttu-id="8f6aa-128">Можно отключить фильтры для отдельных запросов LINQ с помощью оператора IgnoreQueryFilters().</span><span class="sxs-lookup"><span data-stu-id="8f6aa-128">Filters may be disabled for individual LINQ queries using the IgnoreQueryFilters() operator.</span></span>

#### <a name="limitations"></a><span data-ttu-id="8f6aa-129">Ограничения</span><span class="sxs-lookup"><span data-stu-id="8f6aa-129">Limitations</span></span>

- <span data-ttu-id="8f6aa-130">Ссылки навигации запрещены.</span><span class="sxs-lookup"><span data-stu-id="8f6aa-130">Navigation references are not allowed.</span></span> <span data-ttu-id="8f6aa-131">Эта функция может быть добавлена на основе отзывов.</span><span class="sxs-lookup"><span data-stu-id="8f6aa-131">This feature may be added based on feedback.</span></span>
- <span data-ttu-id="8f6aa-132">Фильтры можно определить только для корневого типа сущности в иерархии.</span><span class="sxs-lookup"><span data-stu-id="8f6aa-132">Filters can only be defined on the root Entity Type of a hierarchy.</span></span>

### <a name="database-scalar-function-mapping"></a><span data-ttu-id="8f6aa-133">Сопоставление скалярных функций базы данных</span><span class="sxs-lookup"><span data-stu-id="8f6aa-133">Database scalar function mapping</span></span>

<span data-ttu-id="8f6aa-134">В предварительной версии 2 представлено важное дополнение от [Пола Миддлтона (Paul Middleton)](https://github.com/pmiddleton), позволяющее сопоставлять скалярные функции базы данных с заглушками методов, чтобы их можно было использовать в запросах LINQ и преобразовывать в SQL.</span><span class="sxs-lookup"><span data-stu-id="8f6aa-134">Preview 2 includes an important contribution from [Paul Middleton](https://github.com/pmiddleton) which enables mapping database scalar functions to method stubs so that they can be used in LINQ queries and translated to SQL.</span></span>

<span data-ttu-id="8f6aa-135">Ниже приведено краткое описание использования этой функции.</span><span class="sxs-lookup"><span data-stu-id="8f6aa-135">Here is a brief description of how the feature can be used:</span></span>

<span data-ttu-id="8f6aa-136">Объявите статический метод для вашего `DbContext` и аннотируйте его с помощью `DbFunctionAttribute`.</span><span class="sxs-lookup"><span data-stu-id="8f6aa-136">Declare a static method on your `DbContext` and annotate it with `DbFunctionAttribute`:</span></span>

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

<span data-ttu-id="8f6aa-137">Подобные методы регистрируются автоматически.</span><span class="sxs-lookup"><span data-stu-id="8f6aa-137">Methods like this are automatically registered.</span></span> <span data-ttu-id="8f6aa-138">После регистрации методы вы можете использовать его в любом месте своих запросов.</span><span class="sxs-lookup"><span data-stu-id="8f6aa-138">Once a method has been registered you can use it anywhere in your queries:</span></span>

``` csharp
var query =
    from p in context.Posts
    where BloggingContext.PostReadCount(p.Id) > 5
    select p;
```

<span data-ttu-id="8f6aa-139">Следует отметить несколько аспектов.</span><span class="sxs-lookup"><span data-stu-id="8f6aa-139">A few things to note:</span></span>

- <span data-ttu-id="8f6aa-140">По соглашению, имя метода используется в качестве имени функции (в данном случае пользовательской) при создании кода SQL, но вы можете переопределить имя и схему во время регистрации метод.</span><span class="sxs-lookup"><span data-stu-id="8f6aa-140">By convention the name of the method is used as the name of a function (in this case a user defined function) when generating the SQL, but you can override the name and schema during method registration</span></span>
- <span data-ttu-id="8f6aa-141">Сейчас поддерживаются только скалярные функции.</span><span class="sxs-lookup"><span data-stu-id="8f6aa-141">Currently only scalar functions are supported</span></span>
- <span data-ttu-id="8f6aa-142">Вам нужно создать сопоставленную функцию в базе данных, так как миграции EF Core этого не делают.</span><span class="sxs-lookup"><span data-stu-id="8f6aa-142">You must create the mapped function in the database, e.g. EF Core migrations will not take care of creating it</span></span>

### <a name="self-contained-type-configuration-for-code-first"></a><span data-ttu-id="8f6aa-143">Настройка самодостаточных типов для Code First</span><span class="sxs-lookup"><span data-stu-id="8f6aa-143">Self-contained type configuration for code first</span></span>

<span data-ttu-id="8f6aa-144">В EF6 можно было инкапсулировать конфигурацию Code First для определенного типа сущности, создав объект, производный от *EntityTypeConfiguration*.</span><span class="sxs-lookup"><span data-stu-id="8f6aa-144">In EF6 it was possible to encapsulate the code first configuration of a specific entity type by deriving from *EntityTypeConfiguration*.</span></span> <span data-ttu-id="8f6aa-145">В EF Core 2.0 мы возвращаем эту возможность.</span><span class="sxs-lookup"><span data-stu-id="8f6aa-145">In EF Core 2.0 we are bringing this pattern back:</span></span>

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

## <a name="high-performance"></a><span data-ttu-id="8f6aa-146">Высокая производительность</span><span class="sxs-lookup"><span data-stu-id="8f6aa-146">High Performance</span></span>

### <a name="dbcontext-pooling"></a><span data-ttu-id="8f6aa-147">Создание пулов DbContext</span><span class="sxs-lookup"><span data-stu-id="8f6aa-147">DbContext pooling</span></span>

<span data-ttu-id="8f6aa-148">Типичное использование EF Core в приложении ASP.NET Core обычно включает в себя регистрацию пользовательского типа DbContext в системе внедрения зависимостей и последующее получение экземпляров этого типа через параметры конструктора в контроллерах.</span><span class="sxs-lookup"><span data-stu-id="8f6aa-148">The basic pattern for using EF Core in an ASP.NET Core application usually involves registering a custom DbContext type into the dependency injection system and later obtaining instances of that type through constructor parameters in controllers.</span></span> <span data-ttu-id="8f6aa-149">Это означает, что для каждого из запросов создается экземпляр DbContext.</span><span class="sxs-lookup"><span data-stu-id="8f6aa-149">This means a new instance of the DbContext is created for each requests.</span></span>

<span data-ttu-id="8f6aa-150">В версии 2.0 мы предлагаем новый способ регистрации пользовательских типов DbContext в системе внедрения зависимостей, который прозрачно вводит в обиход пул многоразовых экземпляров DbContext.</span><span class="sxs-lookup"><span data-stu-id="8f6aa-150">In version 2.0 we are introducing a new way to register custom DbContext types in dependency injection which transparently introduces a pool of reusable DbContext instances.</span></span> <span data-ttu-id="8f6aa-151">Для работы с пулами DbContext используйте ```AddDbContextPool``` вместо ```AddDbContext``` во время регистрации службы.</span><span class="sxs-lookup"><span data-stu-id="8f6aa-151">To use DbContext pooling, use the ```AddDbContextPool``` instead of ```AddDbContext``` during service registration:</span></span>

``` csharp
services.AddDbContextPool<BloggingContext>(
    options => options.UseSqlServer(connectionString));
```

<span data-ttu-id="8f6aa-152">Если применяется этот метод, при запросе экземпляра DbContext контроллером мы сначала проверим, доступен ли этот экземпляр в пуле.</span><span class="sxs-lookup"><span data-stu-id="8f6aa-152">If this method is used, at the time a DbContext instance is requested by a controller we will first check if there is an instance available in the pool.</span></span> <span data-ttu-id="8f6aa-153">После завершения обработки запроса любое состояние экземпляра сбрасывается, а сам экземпляр возвращается в пул.</span><span class="sxs-lookup"><span data-stu-id="8f6aa-153">Once the request processing finalizes, any state on the instance is reset and the instance is itself returned to the pool.</span></span>

<span data-ttu-id="8f6aa-154">По своему принципу этот подход похож на работу пулов подключений в поставщиках ADO.NET, кроме того, он выгоден с позиции снижения затрат на инициализацию экземпляра DbContext.</span><span class="sxs-lookup"><span data-stu-id="8f6aa-154">This is conceptually similar to how connection pooling operates in ADO.NET providers and has the advantage of saving some of the cost of initialization of DbContext instance.</span></span>

### <a name="limitations"></a><span data-ttu-id="8f6aa-155">Ограничения</span><span class="sxs-lookup"><span data-stu-id="8f6aa-155">Limitations</span></span>

<span data-ttu-id="8f6aa-156">Новый метод накладывает ряд ограничений на то, что можно сделать в методе ```OnConfiguring()``` DbContext.</span><span class="sxs-lookup"><span data-stu-id="8f6aa-156">The new method introduces a few limitations on what can be done in the ```OnConfiguring()``` method of the DbContext.</span></span>

> [!WARNING]  
> <span data-ttu-id="8f6aa-157">Избегайте создания пулов DbContext, если используете собственное состояние (например, закрытые поля) в производном классе DbContext, которое не должно совместно использоваться разными запросами.</span><span class="sxs-lookup"><span data-stu-id="8f6aa-157">Avoid using DbContext Pooling if you maintain your own state (e.g. private fields) in your derived DbContext class that should not be shared across requests.</span></span> <span data-ttu-id="8f6aa-158">EF Core сбросит только состояние, о котором известно перед добавлением экземпляра DbContext в пул.</span><span class="sxs-lookup"><span data-stu-id="8f6aa-158">EF Core will only reset the state that is aware of before adding a DbContext instance to the pool.</span></span>

### <a name="explicitly-compiled-queries"></a><span data-ttu-id="8f6aa-159">Явным образом скомпилированные запросы</span><span class="sxs-lookup"><span data-stu-id="8f6aa-159">Explicitly compiled queries</span></span>

<span data-ttu-id="8f6aa-160">Это вторя функция обеспечения производительности, предоставляющая преимущества в крупномасштабных сценариях.</span><span class="sxs-lookup"><span data-stu-id="8f6aa-160">This is the second opt-in performance features designed to offer benefits in high-scale scenarios.</span></span>

<span data-ttu-id="8f6aa-161">API вручную или явно скомпилированных запросов были доступны в предыдущих версиях EF, а также в LINQ to SQL, позволяя приложениям кэшировать преобразование запросов, чтобы их можно было вычислить всего один раз, а выполнять многократно.</span><span class="sxs-lookup"><span data-stu-id="8f6aa-161">Manual or explicitly compiled query APIs have been available in previous versions of EF and also in LINQ to SQL to allow applications to cache the translation of queries so that they can be computed only once and executed many times.</span></span>

<span data-ttu-id="8f6aa-162">Хотя в общем случае EF Core может автоматически компилировать и кэшировать запросы на основе хэшированного представления выражений запросов, этот механизм можно использовать для небольшого повышения производительности за счет обхода вычисления хэша и поиска по нему, что позволяет приложению использовать уже скомпилированный запрос с помощью вызова делегата.</span><span class="sxs-lookup"><span data-stu-id="8f6aa-162">Although in general EF Core can automatically compile and cache queries based on a hashed representation of the query expressions, this mechanism can be used to obtain a small performance gain by bypassing the computation of the hash and the cache lookup, allowing the application to use an already compiled query through the invocation of a delegate.</span></span>

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

## <a name="change-tracking"></a><span data-ttu-id="8f6aa-163">Отслеживание изменений</span><span class="sxs-lookup"><span data-stu-id="8f6aa-163">Change Tracking</span></span>

### <a name="attach-can-track-a-graph-of-new-and-existing-entities"></a><span data-ttu-id="8f6aa-164">Attach может отслеживать граф новых и существующих сущностей.</span><span class="sxs-lookup"><span data-stu-id="8f6aa-164">Attach can track a graph of new and existing entities</span></span>

<span data-ttu-id="8f6aa-165">EF Core поддерживает автоматическое формирование значений ключа посредством разнообразных механизмов.</span><span class="sxs-lookup"><span data-stu-id="8f6aa-165">EF Core supports automatic generation of key values through a variety of mechanisms.</span></span> <span data-ttu-id="8f6aa-166">При использовании этой функции значение создается, если для свойства ключа задано значение по умолчанию для среды CLR, которое обычно равно нулю или NULL.</span><span class="sxs-lookup"><span data-stu-id="8f6aa-166">When using this feature, a value is generated if the key property is the CLR default--usually zero or null.</span></span> <span data-ttu-id="8f6aa-167">Это означает, что граф сущностей можно передать в `DbContext.Attach` или `DbSet.Attach`, и EF Core пометит сущности, для которых ключ уже задан, как `Unchanged`, а сущности без набора ключей — как `Added`.</span><span class="sxs-lookup"><span data-stu-id="8f6aa-167">This means that a graph of entities can be passed to `DbContext.Attach` or `DbSet.Attach` and EF Core will mark those entities that have a key already set as `Unchanged` while those entities that do not have a key set will be marked as `Added`.</span></span> <span data-ttu-id="8f6aa-168">Это упрощает вложение смешанных графов из новых и существующих сущностей при использовании сформированных ключей.</span><span class="sxs-lookup"><span data-stu-id="8f6aa-168">This makes it easy to attach a graph of mixed new and existing entities when using generated keys.</span></span> <span data-ttu-id="8f6aa-169">`DbContext.Update`и `DbSet.Update` работают аналогично, за исключением того, что сущности с набором ключей помечаются как `Modified` вместо `Unchanged`.</span><span class="sxs-lookup"><span data-stu-id="8f6aa-169">`DbContext.Update` and `DbSet.Update` work in the same way, except that entities with a key set are marked as `Modified` instead of `Unchanged`.</span></span>

## <a name="query"></a><span data-ttu-id="8f6aa-170">Запрос</span><span class="sxs-lookup"><span data-stu-id="8f6aa-170">Query</span></span>

### <a name="improved-linq-translation"></a><span data-ttu-id="8f6aa-171">Усовершенствованное преобразование LINQ</span><span class="sxs-lookup"><span data-stu-id="8f6aa-171">Improved LINQ Translation</span></span>

<span data-ttu-id="8f6aa-172">Позволяет успешно выполнять больше запросов, увеличив при этом долю логики, вычисляемой в базе данных (а не в памяти) и уменьшив объем ненужных данных, извлекаемых из базы данных.</span><span class="sxs-lookup"><span data-stu-id="8f6aa-172">Enables more queries to successfully execute, with more logic being evaluated in the database (rather than in-memory) and less data unnecessarily being retrieved from the database.</span></span>

### <a name="groupjoin-improvements"></a><span data-ttu-id="8f6aa-173">Усовершенствования объединений групп</span><span class="sxs-lookup"><span data-stu-id="8f6aa-173">GroupJoin improvements</span></span>

<span data-ttu-id="8f6aa-174">Эта работа улучшает код SQL, создаваемый для объединений групп.</span><span class="sxs-lookup"><span data-stu-id="8f6aa-174">This work improves the SQL that is generated for group joins.</span></span> <span data-ttu-id="8f6aa-175">Чаще всего объединения групп возникают в результате выполнения вложенных запросов для необязательных свойств навигации.</span><span class="sxs-lookup"><span data-stu-id="8f6aa-175">Group joins are most often a result of sub-queries on optional navigation properties.</span></span>

### <a name="string-interpolation-in-fromsql-and-executesqlcommand"></a><span data-ttu-id="8f6aa-176">Интерполяция строк в FromSql и ExecuteSqlCommand</span><span class="sxs-lookup"><span data-stu-id="8f6aa-176">String interpolation in FromSql and ExecuteSqlCommand</span></span>

<span data-ttu-id="8f6aa-177">В C# 6 появилась интерполяция строк, позволяющая внедрять выражения C# непосредственно в строковые литералы, что очень удобно для создания строк во время выполнения.</span><span class="sxs-lookup"><span data-stu-id="8f6aa-177">C# 6 introduced String Interpolation, a feature that allows C# expressions to be directly embedded in string literals, providing a nice way of building strings at runtime.</span></span> <span data-ttu-id="8f6aa-178">В EF Core 2.0 мы добавили специальную поддержку интерполированных строк в два основных API, принимающих необработанные строки SQL: ```FromSql``` и ```ExecuteSqlCommand```.</span><span class="sxs-lookup"><span data-stu-id="8f6aa-178">In EF Core 2.0 we added special support for interpolated strings to our two primary APIs that accept raw SQL strings: ```FromSql``` and ```ExecuteSqlCommand```.</span></span> <span data-ttu-id="8f6aa-179">Это обеспечивает безопасное использование интерполяции строк C#.</span><span class="sxs-lookup"><span data-stu-id="8f6aa-179">This new support allows C# string interpolation to be used in a 'safe' manner.</span></span> <span data-ttu-id="8f6aa-180">То есть этот механизм защищает от типичных ошибок внедрения кода SQL, которые могут возникнуть при динамическом формировании кода SQL во время выполнения.</span><span class="sxs-lookup"><span data-stu-id="8f6aa-180">I.e. in a way that protects against common SQL injection mistakes that can occur when dynamically constructing SQL at runtime.</span></span>

<span data-ttu-id="8f6aa-181">Пример:</span><span class="sxs-lookup"><span data-stu-id="8f6aa-181">Here is an example:</span></span>

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

<span data-ttu-id="8f6aa-182">В этом примере в строку формата SQL внедрены две переменные.</span><span class="sxs-lookup"><span data-stu-id="8f6aa-182">In this example, there are two variables embedded in the SQL format string.</span></span> <span data-ttu-id="8f6aa-183">EF Core выдаст следующий код SQL:</span><span class="sxs-lookup"><span data-stu-id="8f6aa-183">EF Core will produce the following SQL:</span></span>

```sql
@p0='London' (Size = 4000)
@p1='Sales Representative' (Size = 4000)

SELECT *
FROM ""Customers""
WHERE ""City"" = @p0
    AND ""ContactTitle"" = @p1
```

### <a name="effunctionslike"></a><span data-ttu-id="8f6aa-184">EF.Functions.Like()</span><span class="sxs-lookup"><span data-stu-id="8f6aa-184">EF.Functions.Like()</span></span>

<span data-ttu-id="8f6aa-185">Мы добавили свойство EF.Functions, которое EF Core или поставщики могут использовать для определения методов, соответствующих операторам или функциям базы данных, чтобы их можно вызывать в запросах LINQ.</span><span class="sxs-lookup"><span data-stu-id="8f6aa-185">We have added the EF.Functions property which can be used by EF Core or providers to define methods that map to database functions or operators so that those can be invoked in LINQ queries.</span></span> <span data-ttu-id="8f6aa-186">Первым примером такого метода является Like().</span><span class="sxs-lookup"><span data-stu-id="8f6aa-186">The first example of such a method is Like():</span></span>

``` csharp
var aCustomers =
    from c in context.Customers
    where EF.Functions.Like(c.Name, "a%");
    select c;
```

<span data-ttu-id="8f6aa-187">Обратите внимание, что Like() предоставляется в реализации для выполнения в памяти, что может быть удобно при работе с выполняющейся в памяти базой данных или при вычислении предиката на стороне клиента.</span><span class="sxs-lookup"><span data-stu-id="8f6aa-187">Note that Like() comes with an in-memory implementation, which can be handy when working against an in-memory database or when evaluation of the predicate needs to occur con the client side.</span></span>

## <a name="database-management"></a><span data-ttu-id="8f6aa-188">Управление базами данных</span><span class="sxs-lookup"><span data-stu-id="8f6aa-188">Database management</span></span>

### <a name="pluralization-hook-for-dbcontext-scaffolding"></a><span data-ttu-id="8f6aa-189">Обработчик преобразования во множественную форму для формирования DbContext</span><span class="sxs-lookup"><span data-stu-id="8f6aa-189">Pluralization hook for DbContext Scaffolding</span></span>

<span data-ttu-id="8f6aa-190">В EF Core 2.0 появилась новая служба *IPluralizer*, используемая для преобразования имен типов сущности в единственное число и имен DbSet — во множественное.</span><span class="sxs-lookup"><span data-stu-id="8f6aa-190">EF Core 2.0 introduces a new *IPluralizer* service that is used to singularize entity type names and pluralize DbSet names.</span></span> <span data-ttu-id="8f6aa-191">Реализация по умолчанию является холостой, то есть это просто обработчик, куда можно подключить свой собственный преобразователь во множественное число.</span><span class="sxs-lookup"><span data-stu-id="8f6aa-191">The default implementation is a no-op, so this is just a hook where folks can easily plug in their own pluralizer.</span></span>

<span data-ttu-id="8f6aa-192">Вот как разработчик может подключить свой собственный преобразователь во множественное число.</span><span class="sxs-lookup"><span data-stu-id="8f6aa-192">Here is what it looks like for a developer to hook in their own pluralizer:</span></span>

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

## <a name="others"></a><span data-ttu-id="8f6aa-193">Другие</span><span class="sxs-lookup"><span data-stu-id="8f6aa-193">Others</span></span>

### <a name="move-adonet-sqlite-provider-to-sqlitepclraw"></a><span data-ttu-id="8f6aa-194">Перемещение поставщика ADO.NET SQLite в SQLitePCL.raw</span><span class="sxs-lookup"><span data-stu-id="8f6aa-194">Move ADO.NET SQLite provider to SQLitePCL.raw</span></span>
<span data-ttu-id="8f6aa-195">Это позволяет получить более надежное решение в Microsoft.Data.Sqlite для распространения собственных двоичных файлов SQLite на разных платформах.</span><span class="sxs-lookup"><span data-stu-id="8f6aa-195">This gives us a more robust solution in Microsoft.Data.Sqlite for distributing native SQLite binaries on different platforms.</span></span>

### <a name="only-one-provider-per-model"></a><span data-ttu-id="8f6aa-196">Всего один поставщик на модель</span><span class="sxs-lookup"><span data-stu-id="8f6aa-196">Only one provider per model</span></span>
<span data-ttu-id="8f6aa-197">Значительно расширяет возможности взаимодействия поставщиков с моделью и упрощает использование соглашений, заметок и текучих API с разными поставщиками.</span><span class="sxs-lookup"><span data-stu-id="8f6aa-197">Significantly augments how providers can interact with the model and simplifies how conventions, annotations and fluent APIs work with different providers.</span></span>

<span data-ttu-id="8f6aa-198">EF Core 2.0 теперь будет создавать отдельный объект [IModel](https://github.com/aspnet/EntityFramework/blob/dev/src/EFCore/Metadata/IModel.cs) для каждого из используемых поставщиков.</span><span class="sxs-lookup"><span data-stu-id="8f6aa-198">EF Core 2.0 will now build a different [IModel](https://github.com/aspnet/EntityFramework/blob/dev/src/EFCore/Metadata/IModel.cs) for each different provider being used.</span></span> <span data-ttu-id="8f6aa-199">Обычно это является прозрачным для приложения.</span><span class="sxs-lookup"><span data-stu-id="8f6aa-199">This is usually transparent to the application.</span></span> <span data-ttu-id="8f6aa-200">Это позволило упростить работу с API метаданных более низкого уровня, например, добиться того, что любое обращение к *основным концепциям реляционных метаданных* всегда осуществляется путем вызова `.Relational` вместо `.SqlServer`, `.Sqlite` и т. д.</span><span class="sxs-lookup"><span data-stu-id="8f6aa-200">This has facilitated a simplification of lower-level metadata APIs such that any access to *common relational metadata concepts* is always made through a call to `.Relational` instead of `.SqlServer`, `.Sqlite`, etc.</span></span>

### <a name="consolidated-logging-and-diagnostics"></a><span data-ttu-id="8f6aa-201">Консолидация ведения журнала и диагностики</span><span class="sxs-lookup"><span data-stu-id="8f6aa-201">Consolidated Logging and Diagnostics</span></span>

<span data-ttu-id="8f6aa-202">Механизмы ведения журнала (на основе ILogger) и диагностики (на основе DiagnosticSource) теперь используют еще больше общего кода.</span><span class="sxs-lookup"><span data-stu-id="8f6aa-202">Logging (based on ILogger) and Diagnostics (based on DiagnosticSource) mechanisms now share more code.</span></span>

<span data-ttu-id="8f6aa-203">В версии 2.0 были изменены идентификаторы событий для сообщений, отправляемых в ILogger.</span><span class="sxs-lookup"><span data-stu-id="8f6aa-203">The event IDs for messages sent to an ILogger have changed in 2.0.</span></span> <span data-ttu-id="8f6aa-204">Теперь эти идентификаторы событий являются уникальными в рамках всего кода EF Core.</span><span class="sxs-lookup"><span data-stu-id="8f6aa-204">The event IDs are now unique across EF Core code.</span></span> <span data-ttu-id="8f6aa-205">Эти сообщения теперь соответствуют стандартному шаблону для структурированного ведения журнала, используемому, например, MVC.</span><span class="sxs-lookup"><span data-stu-id="8f6aa-205">These messages now also follow the standard pattern for structured logging used by, for example, MVC.</span></span>

<span data-ttu-id="8f6aa-206">Кроме того, были изменены категории средства ведения журнала.</span><span class="sxs-lookup"><span data-stu-id="8f6aa-206">Logger categories have also changed.</span></span> <span data-ttu-id="8f6aa-207">Теперь используется известный набор категорий, доступных через [DbLoggerCategory](https://github.com/aspnet/EntityFramework/blob/dev/src/EFCore/DbLoggerCategory.cs).</span><span class="sxs-lookup"><span data-stu-id="8f6aa-207">There is now a well-known set of categories accessed through [DbLoggerCategory](https://github.com/aspnet/EntityFramework/blob/dev/src/EFCore/DbLoggerCategory.cs).</span></span>

<span data-ttu-id="8f6aa-208">События DiagnosticSource теперь используют те же имена идентификаторов событий, что и соответствующие сообщения `ILogger`.</span><span class="sxs-lookup"><span data-stu-id="8f6aa-208">DiagnosticSource events now use the same event ID names as the corresponding `ILogger` messages.</span></span>
