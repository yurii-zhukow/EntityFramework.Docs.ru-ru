---
title: Новые возможности в EF Core 2.0 — EF Core
author: divega
ms.date: 02/20/2018
ms.assetid: 2CB5809E-0EFB-44F6-AF14-9D5BFFFBFF9D
uid: core/what-is-new/ef-core-2.0
ms.openlocfilehash: 72393e96c195af1df5a169025ca2ce7a7acb16bb
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/06/2019
ms.locfileid: "73656213"
---
# <a name="new-features-in-ef-core-20"></a>Новые возможности в EF Core 2.0

## <a name="net-standard-20"></a>.NET Standard 2.0

Сейчас компонент EF Core ориентирован на .NET Standard 2.0, что означает, что он может работать с .NET Core 2.0, .NET Framework 4.6.1 и другими библиотеками, реализующими .NET Standard 2.0.
Дополнительные сведения о поддерживаемых компонентах см. в разделе [Поддерживаемые реализации .NET](../platforms/index.md) .

## <a name="modeling"></a>Моделирование

### <a name="table-splitting"></a>Разбиение таблиц

Теперь можно сопоставить два типа сущностей с одной таблицей и более, где столбцы первичного ключа будут общими, а каждая строка будет соответствовать двум сущностям или более.

Чтобы использовать разбиение таблиц, нужно настроить идентифицирующее отношение (где свойства внешнего ключа формируют первичный ключ) между всеми типами сущностей, осуществляющими общий доступ к таблице.

``` csharp
modelBuilder.Entity<Product>()
    .HasOne(e => e.Details).WithOne(e => e.Product)
    .HasForeignKey<ProductDetails>(e => e.Id);
modelBuilder.Entity<Product>().ToTable("Products");
modelBuilder.Entity<ProductDetails>().ToTable("Products");
```

Дополнительные сведения об этой возможности см. в [разделе, посвященном разделению таблиц](xref:core/modeling/table-splitting).

### <a name="owned-types"></a>Принадлежащие типы

Принадлежащий тип сущности может использовать один тип .NET совместно с другим принадлежащим типом сущности, но, так как его невозможно идентифицировать лишь по типу .NET, требуется переход к нему от другого типа сущности. Сущность, содержащая определяющий переход, является владельцем. При запросе владельца принадлежащие типы включаются по умолчанию.

По соглашению, для принадлежащего типа создается теневой первичный ключ, и этот тип сопоставляется с той же таблицей, что и владелец, с помощью разбиения таблицы. Это позволяет использовать принадлежащие типы аналогично сложным типам в EF6.

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

Дополнительные сведения об этой возможности см. в [разделе, посвященном собственным типам сущностей](xref:core/modeling/owned-entities).

### <a name="model-level-query-filters"></a>Фильтры запросов на уровне модели

EF Core 2.0 включает новую функцию, которую мы назвали фильтром запросов на уровне модели. Она позволяет определить предикаты запросов LINQ (логическое выражение, которое обычно передается в оператор запроса LINQ) непосредственно в типах сущностей внутри модели метаданных (обычно в OnModelCreating). Такие фильтры автоматически применяются к любым запросам LINQ, связанным с этими типами сущностей, включая типы сущностей, указанные косвенно, например, с помощью оператора Include или ссылок на свойства прямой навигации. Ниже приведены некоторые типичные способы применения этой функции.

- Обратимое удаление: тип сущности определяет свойство IsDeleted.
- Мультитенантность: тип сущности определяет свойство TenantId.

Ниже приведен простой пример, демонстрирующий использование этой функции в двух указанных выше сценариях.

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

Мы определяем фильтр на уровне модели, который реализует мультитенантность и обратимое удаление для экземпляров типа сущности `Post`. Обратите внимание на использование свойства уровня экземпляра DbContext: `TenantId`. Фильтры на уровне модели будут использовать значение из правильного экземпляра контекста (то есть экземпляра контекста, выполняющего запрос).

Можно отключить фильтры для отдельных запросов LINQ с помощью оператора IgnoreQueryFilters().

#### <a name="limitations"></a>Ограничения

- Ссылки навигации запрещены. Эта функция может быть добавлена на основе отзывов.
- Фильтры можно определить только для корневого типа сущности в иерархии.

### <a name="database-scalar-function-mapping"></a>Сопоставление скалярных функций базы данных

В EF Core 2.0 представлено важное дополнение от [Пола Миддлтона (Paul Middleton)](https://github.com/pmiddleton), позволяющее сопоставлять скалярные функции базы данных с заглушками методов, чтобы их можно было использовать в запросах LINQ и преобразовывать в SQL.

Ниже приведено краткое описание использования этой функции.

Объявите статический метод для вашего `DbContext` и аннотируйте его с помощью `DbFunctionAttribute`.

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

Подобные методы регистрируются автоматически. После регистрации вызовы метода в запросе LINQ могут быть преобразованы в вызовы функций SQL:

``` csharp
var query =
    from p in context.Posts
    where BloggingContext.PostReadCount(p.Id) > 5
    select p;
```

Следует отметить несколько аспектов.

- По соглашению, имя метода используется в качестве имени функции (в данном случае пользовательской) при создании кода SQL, но вы можете переопределить имя и схему во время регистрации метод.
- Сейчас поддерживаются только скалярные функции.
- Необходимо создать сопоставимую функцию в базе данных. Она не создается при миграциях EF Core.

### <a name="self-contained-type-configuration-for-code-first"></a>Настройка самодостаточных типов для Code First

В EF6 можно было инкапсулировать конфигурацию Code First для определенного типа сущности, создав объект, производный от *EntityTypeConfiguration*. В EF Core 2.0 мы возвращаем эту возможность.

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

## <a name="high-performance"></a>Высокая производительность

### <a name="dbcontext-pooling"></a>Создание пулов DbContext

Типичное использование EF Core в приложении ASP.NET Core обычно включает в себя регистрацию пользовательского типа DbContext в системе внедрения зависимостей и последующее получение экземпляров этого типа через параметры конструктора в контроллерах. Это означает, что для каждого из запросов создается экземпляр DbContext.

В версии 2.0 мы предлагаем новый способ регистрации пользовательских типов DbContext в системе внедрения зависимостей, который прозрачно вводит в обиход пул многоразовых экземпляров DbContext. Для работы с пулами DbContext используйте `AddDbContextPool` вместо `AddDbContext` во время регистрации службы.

``` csharp
services.AddDbContextPool<BloggingContext>(
    options => options.UseSqlServer(connectionString));
```

Если применяется этот метод, при запросе экземпляра DbContext контроллером мы сначала проверим, доступен ли этот экземпляр в пуле. После завершения обработки запроса любое состояние экземпляра сбрасывается, а сам экземпляр возвращается в пул.

По своему принципу этот подход похож на работу пулов подключений в поставщиках ADO.NET, кроме того, он выгоден с позиции снижения затрат на инициализацию экземпляра DbContext.

### <a name="limitations"></a>Ограничения

Новый метод накладывает ряд ограничений на то, что можно сделать в методе `OnConfiguring()` DbContext.

> [!WARNING]  
> Избегайте создания пулов DbContext, если используете собственное состояние (например, закрытые поля) в производном классе DbContext, которое не должно совместно использоваться разными запросами. EF Core сбросит только состояние, о котором известно перед добавлением экземпляра DbContext в пул.

### <a name="explicitly-compiled-queries"></a>Явным образом скомпилированные запросы

Это вторя функция обеспечения производительности, предоставляющая преимущества в крупномасштабных сценариях.

API вручную или явно скомпилированных запросов были доступны в предыдущих версиях EF, а также в LINQ to SQL, позволяя приложениям кэшировать преобразование запросов, чтобы их можно было вычислить всего один раз, а выполнять многократно.

Хотя в общем случае EF Core может автоматически компилировать и кэшировать запросы на основе хэшированного представления выражений запросов, этот механизм можно использовать для небольшого повышения производительности за счет обхода вычисления хэша и поиска по нему, что позволяет приложению использовать уже скомпилированный запрос с помощью вызова делегата.

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

## <a name="change-tracking"></a>Отслеживание изменений

### <a name="attach-can-track-a-graph-of-new-and-existing-entities"></a>Attach может отслеживать граф новых и существующих сущностей.

EF Core поддерживает автоматическое формирование значений ключа посредством разнообразных механизмов. При использовании этой функции значение создается, если для свойства ключа задано значение по умолчанию для среды CLR, которое обычно равно нулю или NULL. Это означает, что граф сущностей можно передать в `DbContext.Attach` или `DbSet.Attach`, и EF Core пометит сущности, для которых ключ уже задан, как `Unchanged`, а сущности без набора ключей — как `Added`. Это упрощает вложение смешанных графов из новых и существующих сущностей при использовании сформированных ключей. `DbContext.Update`и `DbSet.Update` работают аналогично, за исключением того, что сущности с набором ключей помечаются как `Modified` вместо `Unchanged`.

## <a name="query"></a>query

### <a name="improved-linq-translation"></a>Усовершенствованное преобразование LINQ

Позволяет успешно выполнять больше запросов, увеличив при этом долю логики, вычисляемой в базе данных (а не в памяти) и уменьшив объем ненужных данных, извлекаемых из базы данных.

### <a name="groupjoin-improvements"></a>Усовершенствования объединений групп

Эта работа улучшает код SQL, создаваемый для объединений групп. Чаще всего объединения групп возникают в результате выполнения вложенных запросов для необязательных свойств навигации.

### <a name="string-interpolation-in-fromsql-and-executesqlcommand"></a>Интерполяция строк в FromSql и ExecuteSqlCommand

В C# 6 появилась интерполяция строк, позволяющая внедрять выражения C# непосредственно в строковые литералы, что очень удобно для создания строк во время выполнения. В EF Core 2.0 мы добавили специальную поддержку интерполированных строк в два основных API, принимающих необработанные строки SQL: `FromSql` и `ExecuteSqlCommand`. Это обеспечивает безопасное использование интерполяции строк C#. То есть этот механизм защищает от типичных ошибок внедрения кода SQL, которые могут возникнуть при динамическом формировании кода SQL во время выполнения.

Пример:

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

В этом примере в строку формата SQL внедрены две переменные. EF Core выдаст следующий код SQL:

```sql
@p0='London' (Size = 4000)
@p1='Sales Representative' (Size = 4000)

SELECT *
FROM ""Customers""
WHERE ""City"" = @p0
    AND ""ContactTitle"" = @p1
```

### <a name="effunctionslike"></a>EF.Functions.Like()

Мы добавили свойство EF.Functions, которое EF Core или поставщики могут использовать для определения методов, соответствующих операторам или функциям базы данных, чтобы их можно вызывать в запросах LINQ. Первым примером такого метода является Like().

``` csharp
var aCustomers =
    from c in context.Customers
    where EF.Functions.Like(c.Name, "a%")
    select c;
```

Обратите внимание, что Like() предоставляется в реализации для выполнения в памяти, что может быть удобно при работе с выполняющейся в памяти базой данных или при вычислении предиката на стороне клиента.

## <a name="database-management"></a>Управление базами данных

### <a name="pluralization-hook-for-dbcontext-scaffolding"></a>Обработчик преобразования во множественную форму для формирования DbContext

В EF Core 2.0 появилась новая служба *IPluralizer*, используемая для преобразования имен типов сущности в единственное число и имен DbSet — во множественное. Реализация по умолчанию является холостой, то есть это просто обработчик, куда можно подключить свой собственный преобразователь во множественное число.

Вот как разработчик может подключить свой собственный преобразователь во множественное число.

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

## <a name="others"></a>Другие

### <a name="move-adonet-sqlite-provider-to-sqlitepclraw"></a>Перемещение поставщика ADO.NET SQLite в SQLitePCL.raw

Это позволяет получить более надежное решение в Microsoft.Data.Sqlite для распространения собственных двоичных файлов SQLite на разных платформах.

### <a name="only-one-provider-per-model"></a>Всего один поставщик на модель

Значительно расширяет возможности взаимодействия поставщиков с моделью и упрощает использование соглашений, заметок и текучих API с разными поставщиками.

EF Core 2.0 теперь будет создавать отдельный объект [IModel](https://github.com/aspnet/EntityFramework/blob/master/src/EFCore/Metadata/IModel.cs) для каждого из используемых поставщиков. Обычно это является прозрачным для приложения. Это позволило упростить работу с API метаданных более низкого уровня, например, добиться того, что любое обращение к *основным концепциям реляционных метаданных* всегда осуществляется путем вызова `.Relational` вместо `.SqlServer`, `.Sqlite` и т. д.

### <a name="consolidated-logging-and-diagnostics"></a>Консолидация ведения журнала и диагностики

Механизмы ведения журнала (на основе ILogger) и диагностики (на основе DiagnosticSource) теперь используют еще больше общего кода.

В версии 2.0 были изменены идентификаторы событий для сообщений, отправляемых в ILogger. Теперь эти идентификаторы событий являются уникальными в рамках всего кода EF Core. Эти сообщения теперь соответствуют стандартному шаблону для структурированного ведения журнала, используемому, например, MVC.

Кроме того, были изменены категории средства ведения журнала. Теперь используется известный набор категорий, доступных через [DbLoggerCategory](https://github.com/aspnet/EntityFramework/blob/master/src/EFCore/DbLoggerCategory.cs).

События DiagnosticSource теперь используют те же имена идентификаторов событий, что и соответствующие сообщения `ILogger`.
