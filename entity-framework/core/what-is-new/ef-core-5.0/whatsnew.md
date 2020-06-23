---
title: Новые возможности EF Core 5.0
description: Обзор новых возможностей в EF Core 5.0
author: ajcvickers
ms.date: 06/02/2020
uid: core/what-is-new/ef-core-5.0/whatsnew.md
ms.openlocfilehash: 45d851a4b08a26dda0c24e20c79f42964fa4fae4
ms.sourcegitcommit: 1f0f93c66b2b50e03fcbed90260e94faa0279c46
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/04/2020
ms.locfileid: "84418949"
---
# <a name="whats-new-in-ef-core-50"></a><span data-ttu-id="f5fbc-103">Новые возможности EF Core 5.0</span><span class="sxs-lookup"><span data-stu-id="f5fbc-103">What's New in EF Core 5.0</span></span>

<span data-ttu-id="f5fbc-104">Сейчас EF Core 5.0 находится на стадии разработки.</span><span class="sxs-lookup"><span data-stu-id="f5fbc-104">EF Core 5.0 is currently in development.</span></span>
<span data-ttu-id="f5fbc-105">На этой странице представлен обзор интересных изменений, появившихся в каждой предварительной версии.</span><span class="sxs-lookup"><span data-stu-id="f5fbc-105">This page will contain an overview of interesting changes introduced in each preview.</span></span>

<span data-ttu-id="f5fbc-106">Эта страница не является аналогом статьи о [планировании для EF Core 5.0](plan.md).</span><span class="sxs-lookup"><span data-stu-id="f5fbc-106">This page does not duplicate the [plan for EF Core 5.0](plan.md).</span></span>
<span data-ttu-id="f5fbc-107">В плане описываются общие темы для EF Core 5.0, в том числе все, что планируется включить перед выпуском окончательной версии.</span><span class="sxs-lookup"><span data-stu-id="f5fbc-107">The plan describes the overall themes for EF Core 5.0, including everything we are planning to include before shipping the final release.</span></span>

<span data-ttu-id="f5fbc-108">Мы будем добавлять ссылки на официальную документацию по мере ее публикации.</span><span class="sxs-lookup"><span data-stu-id="f5fbc-108">We will add links from here to the official documentation as it is published.</span></span>

## <a name="preview-5"></a><span data-ttu-id="f5fbc-109">Предварительная версия 5</span><span class="sxs-lookup"><span data-stu-id="f5fbc-109">Preview 5</span></span>

### <a name="database-collations"></a><span data-ttu-id="f5fbc-110">Параметры сортировки баз данных</span><span class="sxs-lookup"><span data-stu-id="f5fbc-110">Database collations</span></span>

<span data-ttu-id="f5fbc-111">Модель EF теперь позволяет указывать параметры сортировки по умолчанию для базы данных.</span><span class="sxs-lookup"><span data-stu-id="f5fbc-111">The default collation for a database can now be specified in the EF model.</span></span>
<span data-ttu-id="f5fbc-112">Это будет передаваться в создаваемые миграции для задания параметров сортировки при создании базы данных.</span><span class="sxs-lookup"><span data-stu-id="f5fbc-112">This will flow through to generated migrations to set the collation when the database is created.</span></span>
<span data-ttu-id="f5fbc-113">Пример:</span><span class="sxs-lookup"><span data-stu-id="f5fbc-113">For example:</span></span>

```CSharp
modelBuilder.UseCollation("German_PhoneBook_CI_AS");
```

<span data-ttu-id="f5fbc-114">Затем функция миграции формирует следующий код для создания базы данных в SQL Server:</span><span class="sxs-lookup"><span data-stu-id="f5fbc-114">Migrations then generates the following to create the database on SQL Server:</span></span>

```sql
CREATE DATABASE [Test]
COLLATE German_PhoneBook_CI_AS;
```

<span data-ttu-id="f5fbc-115">Можно также указать параметры сортировки, которые будут использоваться для конкретных столбцов.</span><span class="sxs-lookup"><span data-stu-id="f5fbc-115">The collation to use for specific database columns can also be specified.</span></span>
<span data-ttu-id="f5fbc-116">Пример:</span><span class="sxs-lookup"><span data-stu-id="f5fbc-116">For example:</span></span>

```CSharp
 modelBuilder
     .Entity<User>()
     .Property(e => e.Name)
     .UseCollation("German_PhoneBook_CI_AS");
```

<span data-ttu-id="f5fbc-117">Если функция миграции не используется, параметры сортировки реконструируются из базы данных при формировании шаблонов DbContext.</span><span class="sxs-lookup"><span data-stu-id="f5fbc-117">For those not using migrations, collations are now reverse-engineered from the database when scaffolding a DbContext.</span></span>

<span data-ttu-id="f5fbc-118">Наконец, `EF.Functions.Collate()` позволяет использовать различные параметры сортировки для нерегламентированных запросов.</span><span class="sxs-lookup"><span data-stu-id="f5fbc-118">Finally, the `EF.Functions.Collate()` allows for ad-hoc queries using different collations.</span></span>
<span data-ttu-id="f5fbc-119">Пример:</span><span class="sxs-lookup"><span data-stu-id="f5fbc-119">For example:</span></span>

```CSharp
context.Users.Single(e => EF.Functions.Collate(e.Name, "French_CI_AS") == "Jean-Michel Jarre");
```

<span data-ttu-id="f5fbc-120">Будет создан следующий запрос для SQL Server:</span><span class="sxs-lookup"><span data-stu-id="f5fbc-120">This will generate the following query for SQL Server:</span></span>

```sql
SELECT TOP(2) [u].[Id], [u].[Name]
FROM [Users] AS [u]
WHERE [u].[Name] COLLATE French_CI_AS = N'Jean-Michel Jarre'
```

<span data-ttu-id="f5fbc-121">Обратите внимание, что нерегламентированные параметры сортировки следует использовать с осторожностью, так как они могут негативно повлиять на производительность базы данных.</span><span class="sxs-lookup"><span data-stu-id="f5fbc-121">Note that ad-hoc collations should be used with care as they can negatively impact database performance.</span></span>

<span data-ttu-id="f5fbc-122">Это документируется в проблеме [#2273](https://github.com/dotnet/EntityFramework.Docs/issues/2273).</span><span class="sxs-lookup"><span data-stu-id="f5fbc-122">Documentation is tracked by issue [#2273](https://github.com/dotnet/EntityFramework.Docs/issues/2273).</span></span>

### <a name="flow-arguments-into-idesigntimedbcontextfactory"></a><span data-ttu-id="f5fbc-123">Передача аргументов в интерфейс IDesignTimeDbContextFactory</span><span class="sxs-lookup"><span data-stu-id="f5fbc-123">Flow arguments into IDesignTimeDbContextFactory</span></span>

<span data-ttu-id="f5fbc-124">Теперь можно передавать аргументы из командной строки в метод `CreateDbContext` интерфейса [IDesignTimeDbContextFactory](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.design.idesigntimedbcontextfactory-1?view=efcore-3.1).</span><span class="sxs-lookup"><span data-stu-id="f5fbc-124">Arguments are now flowed from the command line into the `CreateDbContext` method of [IDesignTimeDbContextFactory](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.design.idesigntimedbcontextfactory-1?view=efcore-3.1).</span></span> <span data-ttu-id="f5fbc-125">Например, чтобы указать, что используется сборка для разработки, можно передать в командной строке настраиваемый аргумент, например `dev`:</span><span class="sxs-lookup"><span data-stu-id="f5fbc-125">For example, to indicate this is a dev build, a custom argument (e.g. `dev`) can passed on the command line:</span></span>

```
dotnet ef migrations add two --verbose --dev
``` 

<span data-ttu-id="f5fbc-126">Затем этот аргумент будет передан в фабрику, где его можно использовать для управления созданием и инициализацией контекста.</span><span class="sxs-lookup"><span data-stu-id="f5fbc-126">This argument will then flow into the factory, where it can be used to control how the context is created and initialized.</span></span>
<span data-ttu-id="f5fbc-127">Пример:</span><span class="sxs-lookup"><span data-stu-id="f5fbc-127">For example:</span></span>

```CSharp
public class MyDbContextFactory : IDesignTimeDbContextFactory<SomeDbContext>
{
    public SomeDbContext CreateDbContext(string[] args) 
        => new SomeDbContext(args.Contains("--dev"));
}
```

<span data-ttu-id="f5fbc-128">Это документируется в проблеме [#2419](https://github.com/dotnet/EntityFramework.Docs/issues/2419).</span><span class="sxs-lookup"><span data-stu-id="f5fbc-128">Documentation is tracked by issue [#2419](https://github.com/dotnet/EntityFramework.Docs/issues/2419).</span></span>

### <a name="no-tracking-queries-with-identity-resolution"></a><span data-ttu-id="f5fbc-129">Запросы без отслеживания с разрешением идентификаторов</span><span class="sxs-lookup"><span data-stu-id="f5fbc-129">No-tracking queries with identity resolution</span></span>

<span data-ttu-id="f5fbc-130">Теперь можно настроить запросы без отслеживания на выполнение разрешения идентификаторов.</span><span class="sxs-lookup"><span data-stu-id="f5fbc-130">No-tracking queries can now be configured to perform identity resolution.</span></span>
<span data-ttu-id="f5fbc-131">Например, следующий запрос будет создавать новый экземпляр Blog для каждого объекта Post, даже если у каждого Blog один и тот же первичный ключ.</span><span class="sxs-lookup"><span data-stu-id="f5fbc-131">For example, the following query will create a new Blog instance for each Post, even if each Blog has the same primary key.</span></span> 

```CSharp
context.Posts.AsNoTracking().Include(e => e.Blog).ToList();
```

<span data-ttu-id="f5fbc-132">Однако за счет того, что этот запрос обычно выполняется немного медленнее и всегда использует больше памяти, его можно изменить так, чтобы создавался только один экземпляр Blog:</span><span class="sxs-lookup"><span data-stu-id="f5fbc-132">However, at the expense of usually being slightly slower and always using more memory, this query can be changed to ensure only a single Blog instance is created:</span></span>

```CSharp
context.Posts.AsNoTracking().PerformIdentityResolution().Include(e => e.Blog).ToList();
```

<span data-ttu-id="f5fbc-133">Обратите внимание, что этот вариант подходит только для запросов без отслеживания, так как все запросы с отслеживанием уже действуют таким образом.</span><span class="sxs-lookup"><span data-stu-id="f5fbc-133">Note that this is only useful for no-tracking queries since all tracking queries already exhibit this behavior.</span></span> <span data-ttu-id="f5fbc-134">Кроме того, после проверки API будет изменен синтаксис `PerformIdentityResolution`.</span><span class="sxs-lookup"><span data-stu-id="f5fbc-134">Also, following API review, the `PerformIdentityResolution` syntax will be changed.</span></span>
<span data-ttu-id="f5fbc-135">См. [#19877](https://github.com/dotnet/efcore/issues/19877#issuecomment-637371073).</span><span class="sxs-lookup"><span data-stu-id="f5fbc-135">See [#19877](https://github.com/dotnet/efcore/issues/19877#issuecomment-637371073).</span></span>

<span data-ttu-id="f5fbc-136">Это документируется в проблеме [#1895](https://github.com/dotnet/EntityFramework.Docs/issues/1895).</span><span class="sxs-lookup"><span data-stu-id="f5fbc-136">Documentation is tracked by issue [#1895](https://github.com/dotnet/EntityFramework.Docs/issues/1895).</span></span>

### <a name="stored-persisted-computed-columns"></a><span data-ttu-id="f5fbc-137">Сохраняемые вычисляемые столбцы</span><span class="sxs-lookup"><span data-stu-id="f5fbc-137">Stored (persisted) computed columns</span></span>

<span data-ttu-id="f5fbc-138">Большинство баз данных позволяют сохранять значения вычисляемых столбцов после вычислений.</span><span class="sxs-lookup"><span data-stu-id="f5fbc-138">Most databases allow computed column values to be stored after computation.</span></span>
<span data-ttu-id="f5fbc-139">Хотя для этого требуется место на диске, вычисляемый столбец вычисляется только один раз при обновлении, а не при каждом получении его значения.</span><span class="sxs-lookup"><span data-stu-id="f5fbc-139">While this takes up disk space, the computed column is calculated only once on update, instead of each time its value is retrieved.</span></span>
<span data-ttu-id="f5fbc-140">Это также позволяет индексировать столбец для некоторых баз данных.</span><span class="sxs-lookup"><span data-stu-id="f5fbc-140">This also allows the column to be indexed for some databases.</span></span>

<span data-ttu-id="f5fbc-141">EF Core 5.0 поддерживает настройку вычисляемых столбцов как сохраняемых.</span><span class="sxs-lookup"><span data-stu-id="f5fbc-141">EF Core 5.0 allows computed columns to be configured as stored.</span></span>
<span data-ttu-id="f5fbc-142">Пример:</span><span class="sxs-lookup"><span data-stu-id="f5fbc-142">For example:</span></span>
 
```CSharp
modelBuilder
    .Entity<User>()
    .Property(e => e.SomethingComputed)
    .HasComputedColumnSql("my sql", stored: true);
```

### <a name="sqlite-computed-columns"></a><span data-ttu-id="f5fbc-143">Вычисляемые столбцы SQLite</span><span class="sxs-lookup"><span data-stu-id="f5fbc-143">SQLite computed columns</span></span>

<span data-ttu-id="f5fbc-144">EF Core теперь поддерживает вычисляемые столбцы в базах данных SQLite.</span><span class="sxs-lookup"><span data-stu-id="f5fbc-144">EF Core now supports computed columns in SQLite databases.</span></span>

## <a name="preview-4"></a><span data-ttu-id="f5fbc-145">Предварительная версия 4</span><span class="sxs-lookup"><span data-stu-id="f5fbc-145">Preview 4</span></span>

### <a name="configure-database-precisionscale-in-model"></a><span data-ttu-id="f5fbc-146">Настройка точности и масштаба для базы данных в модели</span><span class="sxs-lookup"><span data-stu-id="f5fbc-146">Configure database precision/scale in model</span></span>

<span data-ttu-id="f5fbc-147">Теперь можно указать точность и масштаб для свойства с помощью построителя модели.</span><span class="sxs-lookup"><span data-stu-id="f5fbc-147">Precision and scale for a property can now be specified using the model builder.</span></span>
<span data-ttu-id="f5fbc-148">Пример:</span><span class="sxs-lookup"><span data-stu-id="f5fbc-148">For example:</span></span>

```CSharp
modelBuilder
    .Entity<Blog>()
    .Property(b => b.Numeric)
    .HasPrecision(16, 4);
```

<span data-ttu-id="f5fbc-149">Точность и масштаб по-прежнему можно задать, используя полный тип базы данных, например "decimal(16,4)".</span><span class="sxs-lookup"><span data-stu-id="f5fbc-149">Precision and scale can still be set via the full database type, such as "decimal(16,4)".</span></span> 

<span data-ttu-id="f5fbc-150">Документация отслеживается по проблеме [#527](https://github.com/dotnet/EntityFramework.Docs/issues/527).</span><span class="sxs-lookup"><span data-stu-id="f5fbc-150">Documentation is tracked by issue [#527](https://github.com/dotnet/EntityFramework.Docs/issues/527).</span></span>

### <a name="specify-sql-server-index-fill-factor"></a><span data-ttu-id="f5fbc-151">Указание коэффициента заполнения индекса SQL Server</span><span class="sxs-lookup"><span data-stu-id="f5fbc-151">Specify SQL Server index fill factor</span></span>

<span data-ttu-id="f5fbc-152">Теперь при создании индекса в SQL Server можно указать коэффициент заполнения.</span><span class="sxs-lookup"><span data-stu-id="f5fbc-152">The fill factor can now be specified when creating an index on SQL Server.</span></span>
<span data-ttu-id="f5fbc-153">Пример:</span><span class="sxs-lookup"><span data-stu-id="f5fbc-153">For example:</span></span>

```CSharp
modelBuilder
    .Entity<Customer>()
    .HasIndex(e => e.Name)
    .HasFillFactor(90);
```

## <a name="preview-3"></a><span data-ttu-id="f5fbc-154">Предварительная версия 3</span><span class="sxs-lookup"><span data-stu-id="f5fbc-154">Preview 3</span></span>

### <a name="filtered-include"></a><span data-ttu-id="f5fbc-155">Включение с фильтрацией</span><span class="sxs-lookup"><span data-stu-id="f5fbc-155">Filtered Include</span></span>

<span data-ttu-id="f5fbc-156">Метод Include теперь поддерживает фильтрацию включенных сущностей.</span><span class="sxs-lookup"><span data-stu-id="f5fbc-156">The Include method now supports filtering of the entities included.</span></span>
<span data-ttu-id="f5fbc-157">Пример:</span><span class="sxs-lookup"><span data-stu-id="f5fbc-157">For example:</span></span>

```CSharp
var blogs = context.Blogs
    .Include(e => e.Posts.Where(p => p.Title.Contains("Cheese")))
    .ToList();
```

<span data-ttu-id="f5fbc-158">Этот запрос возвращает блоги со всеми связанными записями, но только в том случае, если заголовок записи содержит Cheese.</span><span class="sxs-lookup"><span data-stu-id="f5fbc-158">This query will return blogs together with each associated post, but only when the post title contains "Cheese".</span></span>

<span data-ttu-id="f5fbc-159">Для сокращения числа включаемых сущностей также можно использовать методы Skip и Take.</span><span class="sxs-lookup"><span data-stu-id="f5fbc-159">Skip and Take can also be used to reduce the number of included entities.</span></span>
<span data-ttu-id="f5fbc-160">Пример:</span><span class="sxs-lookup"><span data-stu-id="f5fbc-160">For example:</span></span>
 
```CSharp
var blogs = context.Blogs
    .Include(e => e.Posts.OrderByDescending(post => post.Title).Take(5)))
    .ToList();
```
<span data-ttu-id="f5fbc-161">Этот запрос возвратит блоги, включив не более пяти записей для каждого блога.</span><span class="sxs-lookup"><span data-stu-id="f5fbc-161">This query will return blogs with at most five posts included on each blog.</span></span>

<span data-ttu-id="f5fbc-162">Подробные сведения см. в [документации по методу Include](xref:core/querying/related-data#filtered-include).</span><span class="sxs-lookup"><span data-stu-id="f5fbc-162">See the [Include documentation](xref:core/querying/related-data#filtered-include) for full details.</span></span>

### <a name="new-modelbuilder-api-for-navigation-properties"></a><span data-ttu-id="f5fbc-163">Новый API ModelBuilder для свойств навигации</span><span class="sxs-lookup"><span data-stu-id="f5fbc-163">New ModelBuilder API for navigation properties</span></span>

<span data-ttu-id="f5fbc-164">Свойства навигации главным образом настраиваются при [определении связей](xref:core/modeling/relationships).</span><span class="sxs-lookup"><span data-stu-id="f5fbc-164">Navigation properties are primarily configured when [defining relationships](xref:core/modeling/relationships).</span></span>
<span data-ttu-id="f5fbc-165">Однако новый метод `Navigation` можно использовать в случаях, когда свойства навигации требуют дополнительной настройки.</span><span class="sxs-lookup"><span data-stu-id="f5fbc-165">However, the new `Navigation` method can be used in the cases where navigation properties need additional configuration.</span></span>
<span data-ttu-id="f5fbc-166">Например, чтобы задать резервное поле для навигации, если не удалось найти поле по соглашению:</span><span class="sxs-lookup"><span data-stu-id="f5fbc-166">For example, to set a backing field for the navigation when the field would not be found by convention:</span></span>

```CSharp
modelBuilder.Entity<Blog>().Navigation(e => e.Posts).HasField("_myposts");
```

<span data-ttu-id="f5fbc-167">Обратите внимание, что API `Navigation` не является заменой для конфигурации связи.</span><span class="sxs-lookup"><span data-stu-id="f5fbc-167">Note that the `Navigation` API does not replace relationship configuration.</span></span>
<span data-ttu-id="f5fbc-168">Вместо этого он допускает дополнительную настройку свойств навигации в уже обнаруженных или определенных связях.</span><span class="sxs-lookup"><span data-stu-id="f5fbc-168">Instead it allows additional configuration of navigation properties in already discovered or defined relationships.</span></span>

<span data-ttu-id="f5fbc-169">См. [документацию по настройке свойств навигации](xref:core/modeling/relationships#configuring-navigation-properties).</span><span class="sxs-lookup"><span data-stu-id="f5fbc-169">See the [Configuring Navigation Properties documentation](xref:core/modeling/relationships#configuring-navigation-properties).</span></span>

### <a name="new-command-line-parameters-for-namespaces-and-connection-strings"></a><span data-ttu-id="f5fbc-170">Новые параметры командной строки для пространств имен и строк подключения</span><span class="sxs-lookup"><span data-stu-id="f5fbc-170">New command-line parameters for namespaces and connection strings</span></span> 

<span data-ttu-id="f5fbc-171">Теперь можно указывать пространства имен в командной строке во время миграции и формирования шаблонов.</span><span class="sxs-lookup"><span data-stu-id="f5fbc-171">Migrations and scaffolding now allow namespaces to be specified on the command line.</span></span>
<span data-ttu-id="f5fbc-172">Это можно сделать, например, затем, чтобы реконструировать базу данных, поместив в нее классы контекста и модели в различных пространствах имен:</span><span class="sxs-lookup"><span data-stu-id="f5fbc-172">For example, to reverse engineer a database putting the context and model classes in different namespaces:</span></span> 

```
dotnet ef dbcontext scaffold "connection string" Microsoft.EntityFrameworkCore.SqlServer --context-namespace "My.Context" --namespace "My.Model"
```

<span data-ttu-id="f5fbc-173">Подробные сведения см. в документации по [миграции](xref:core/managing-schemas/migrations/index#namespaces) и [реконструированию](xref:core/managing-schemas/scaffolding#directories-and-namespaces).</span><span class="sxs-lookup"><span data-stu-id="f5fbc-173">See the [Migrations](xref:core/managing-schemas/migrations/index#namespaces) and [Reverse Engineering](xref:core/managing-schemas/scaffolding#directories-and-namespaces) documentation for full details.</span></span>

---
<span data-ttu-id="f5fbc-174">Кроме того, теперь можно передать строку подключения команде `database-update`.</span><span class="sxs-lookup"><span data-stu-id="f5fbc-174">Also, a connection string can now be passed to the `database-update` command:</span></span>

```
dotnet ef database update --connection "connection string"
```

<span data-ttu-id="f5fbc-175">Подробные сведения см. в [документации по средствам](xref:core/miscellaneous/cli/dotnet#dotnet-ef-database-update).</span><span class="sxs-lookup"><span data-stu-id="f5fbc-175">See the [Tools documentation](xref:core/miscellaneous/cli/dotnet#dotnet-ef-database-update) for full details.</span></span>

<span data-ttu-id="f5fbc-176">Аналогичные параметры также были добавлены в команды PowerShell, используемые в консоли диспетчера пакетов VS.</span><span class="sxs-lookup"><span data-stu-id="f5fbc-176">Equivalent parameters have also been added to the PowerShell commands used in the VS Package Manager Console.</span></span>

### <a name="enabledetailederrors-has-returned"></a><span data-ttu-id="f5fbc-177">Возвращение EnableDetailedErrors</span><span class="sxs-lookup"><span data-stu-id="f5fbc-177">EnableDetailedErrors has returned</span></span>

<span data-ttu-id="f5fbc-178">По соображениям производительности EF не выполняет дополнительные проверки на NULL при считывании значений из базы данных.</span><span class="sxs-lookup"><span data-stu-id="f5fbc-178">For performance reasons, EF doesn't do additional null-checks when reading values from the database.</span></span>
<span data-ttu-id="f5fbc-179">Это может привести к возникновению исключений при обнаружении непредвиденных значений NULL, для которых будет сложно определить причину.</span><span class="sxs-lookup"><span data-stu-id="f5fbc-179">This can result in exceptions that are hard to root-cause when an unexpected null is encountered.</span></span>

<span data-ttu-id="f5fbc-180">При использовании `EnableDetailedErrors` будет добавлена дополнительная проверка на NULL для запросов, чтобы облегчить определение причин этих ошибок за счет небольшого снижения производительности.</span><span class="sxs-lookup"><span data-stu-id="f5fbc-180">Using `EnableDetailedErrors` will add extra null checking to queries such that, for a small performance overhead, these errors are easier to trace back to a root cause.</span></span>  

<span data-ttu-id="f5fbc-181">Пример:</span><span class="sxs-lookup"><span data-stu-id="f5fbc-181">For example:</span></span>
```CSharp
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    => optionsBuilder
        .EnableDetailedErrors()
        .EnableSensitiveDataLogging() // Often also useful with EnableDetailedErrors 
        .UseSqlServer(Your.SqlServerConnectionString);
```

<span data-ttu-id="f5fbc-182">Документация отслеживается по проблеме [#955](https://github.com/dotnet/EntityFramework.Docs/issues/955).</span><span class="sxs-lookup"><span data-stu-id="f5fbc-182">Documentation is tracked by issue [#955](https://github.com/dotnet/EntityFramework.Docs/issues/955).</span></span>

### <a name="cosmos-partition-keys"></a><span data-ttu-id="f5fbc-183">Ключи секций Cosmos</span><span class="sxs-lookup"><span data-stu-id="f5fbc-183">Cosmos partition keys</span></span>

<span data-ttu-id="f5fbc-184">Теперь можно указать в запросе ключ секции, используемый для данного запроса.</span><span class="sxs-lookup"><span data-stu-id="f5fbc-184">The partition key to use for a given query can now be specified in the query.</span></span>
<span data-ttu-id="f5fbc-185">Пример:</span><span class="sxs-lookup"><span data-stu-id="f5fbc-185">For example:</span></span>

```CSharp
await context.Set<Customer>()
             .WithPartitionKey(myPartitionKey)
             .FirstAsync();
```

<span data-ttu-id="f5fbc-186">Документация отслеживается по проблеме [#2199](https://github.com/dotnet/EntityFramework.Docs/issues/2199).</span><span class="sxs-lookup"><span data-stu-id="f5fbc-186">Documentation is tracked by issue [#2199](https://github.com/dotnet/EntityFramework.Docs/issues/2199).</span></span>

### <a name="support-for-the-sql-server-datalength-function"></a><span data-ttu-id="f5fbc-187">Поддержка функции DATALENGTH SQL Server</span><span class="sxs-lookup"><span data-stu-id="f5fbc-187">Support for the SQL Server DATALENGTH function</span></span>

<span data-ttu-id="f5fbc-188">К ней можно обратиться с помощью нового метода `EF.Functions.DataLength`.</span><span class="sxs-lookup"><span data-stu-id="f5fbc-188">This can be accessed using the new `EF.Functions.DataLength` method.</span></span>
<span data-ttu-id="f5fbc-189">Пример:</span><span class="sxs-lookup"><span data-stu-id="f5fbc-189">For example:</span></span>
```CSharp
var count = context.Orders.Count(c => 100 < EF.Functions.DataLength(c.OrderDate));
``` 

## <a name="preview-2"></a><span data-ttu-id="f5fbc-190">Предварительная версия 2</span><span class="sxs-lookup"><span data-stu-id="f5fbc-190">Preview 2</span></span>

### <a name="use-a-c-attribute-to-specify-a-property-backing-field"></a><span data-ttu-id="f5fbc-191">Использование атрибута C# для указания резервного поля свойств</span><span class="sxs-lookup"><span data-stu-id="f5fbc-191">Use a C# attribute to specify a property backing field</span></span>

<span data-ttu-id="f5fbc-192">Теперь атрибут C# можно использовать для указания резервного поля для свойства.</span><span class="sxs-lookup"><span data-stu-id="f5fbc-192">A C# attribute can now be used to specify the backing field for a property.</span></span>
<span data-ttu-id="f5fbc-193">Этот атрибут позволяет EF Core в обычном порядке выполнять запись и чтение из резервного поля, даже если резервное поле не может быть найдено автоматически.</span><span class="sxs-lookup"><span data-stu-id="f5fbc-193">This attribute allows EF Core to still write to and read from the backing field as would normally happen, even when the backing field cannot be found automatically.</span></span>
<span data-ttu-id="f5fbc-194">Пример:</span><span class="sxs-lookup"><span data-stu-id="f5fbc-194">For example:</span></span>

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

<span data-ttu-id="f5fbc-195">Документация отслеживается по проблеме [#2230](https://github.com/dotnet/EntityFramework.Docs/issues/2230).</span><span class="sxs-lookup"><span data-stu-id="f5fbc-195">Documentation is tracked by issue [#2230](https://github.com/dotnet/EntityFramework.Docs/issues/2230).</span></span>

### <a name="complete-discriminator-mapping"></a><span data-ttu-id="f5fbc-196">Завершение сопоставления дискриминатора</span><span class="sxs-lookup"><span data-stu-id="f5fbc-196">Complete discriminator mapping</span></span>

<span data-ttu-id="f5fbc-197">EF Core использует столбец дискриминатора для [сопоставления TPH в иерархии наследования](/ef/core/modeling/inheritance).</span><span class="sxs-lookup"><span data-stu-id="f5fbc-197">EF Core uses a discriminator column for [TPH mapping of an inheritance hierarchy](/ef/core/modeling/inheritance).</span></span>
<span data-ttu-id="f5fbc-198">Некоторые улучшения производительности будут поддерживаться до тех пор, пока EF Core будут известны все возможные значения для дискриминатора.</span><span class="sxs-lookup"><span data-stu-id="f5fbc-198">Some performance enhancements are possible so long as EF Core knows all possible values for the discriminator.</span></span>
<span data-ttu-id="f5fbc-199">Теперь эти улучшения реализованы в EF Core 5.0.</span><span class="sxs-lookup"><span data-stu-id="f5fbc-199">EF Core 5.0 now implements these enhancements.</span></span>

<span data-ttu-id="f5fbc-200">Например, в предыдущих версиях EF Core всегда создавался следующий код SQL для запроса, возвращающего все типы в иерархии:</span><span class="sxs-lookup"><span data-stu-id="f5fbc-200">For example, previous versions of EF Core would always generate this SQL for a query returning all types in a hierarchy:</span></span>

```sql
SELECT [a].[Id], [a].[Discriminator], [a].[Name]
FROM [Animal] AS [a]
WHERE [a].[Discriminator] IN (N'Animal', N'Cat', N'Dog', N'Human')
```

<span data-ttu-id="f5fbc-201">Теперь при настройке полного сопоставления дискриминатора EF Core 5.0 будет формировать следующее:</span><span class="sxs-lookup"><span data-stu-id="f5fbc-201">EF Core 5.0 will now generate the following when a complete discriminator mapping is configured:</span></span>

```sql
SELECT [a].[Id], [a].[Discriminator], [a].[Name]
FROM [Animal] AS [a]
```

<span data-ttu-id="f5fbc-202">Начиная с предварительной версии 3 это поведение будет доступно по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="f5fbc-202">It will be the default behavior starting with preview 3.</span></span>

### <a name="performance-improvements-in-microsoftdatasqlite"></a><span data-ttu-id="f5fbc-203">Улучшения производительности в Microsoft.Data.Sqlite</span><span class="sxs-lookup"><span data-stu-id="f5fbc-203">Performance improvements in Microsoft.Data.Sqlite</span></span>

<span data-ttu-id="f5fbc-204">В производительность SQLIte были внесены два улучшения:</span><span class="sxs-lookup"><span data-stu-id="f5fbc-204">We have made two performance improvements for SQLIte:</span></span>

* <span data-ttu-id="f5fbc-205">Теперь получение двоичных и строковых данных с помощью GetBytes, GetChars и GetTextReader осуществляется более эффективно за счет использования SqliteBlob и потоков.</span><span class="sxs-lookup"><span data-stu-id="f5fbc-205">Retrieving binary and string data with GetBytes, GetChars, and GetTextReader is now more efficient by making use of SqliteBlob and streams.</span></span>
* <span data-ttu-id="f5fbc-206">Инициализация SqliteConnection выполняется в отложенном режиме.</span><span class="sxs-lookup"><span data-stu-id="f5fbc-206">Initialization of SqliteConnection is now lazy.</span></span>

<span data-ttu-id="f5fbc-207">Эти улучшения добавлены в поставщик Microsoft.Data.Sqlite ADO.NET и, следовательно, позволяют повысить производительность за пределами EF Core.</span><span class="sxs-lookup"><span data-stu-id="f5fbc-207">These improvements are in the ADO.NET Microsoft.Data.Sqlite provider and hence also improve performance outside of EF Core.</span></span>

## <a name="preview-1"></a><span data-ttu-id="f5fbc-208">Предварительная версия 1</span><span class="sxs-lookup"><span data-stu-id="f5fbc-208">Preview 1</span></span>

### <a name="simple-logging"></a><span data-ttu-id="f5fbc-209">Простое ведение журнала</span><span class="sxs-lookup"><span data-stu-id="f5fbc-209">Simple logging</span></span>

<span data-ttu-id="f5fbc-210">Эта функция добавляет возможность, аналогичную `Database.Log` в EF6.</span><span class="sxs-lookup"><span data-stu-id="f5fbc-210">This feature adds functionality similar to `Database.Log` in EF6.</span></span>
<span data-ttu-id="f5fbc-211">Это значит, что она позволяет легко получать журналы из EF Core без настройки какой-либо внешней платформы ведения журналов.</span><span class="sxs-lookup"><span data-stu-id="f5fbc-211">That is, it provides a simple way to get logs from EF Core without the need to configure any kind of external logging framework.</span></span>

<span data-ttu-id="f5fbc-212">Предварительная документация включена в раздел с описанием [еженедельного состояния EF на 5 декабря 2019 г](https://github.com/dotnet/efcore/issues/15403#issuecomment-562332863).</span><span class="sxs-lookup"><span data-stu-id="f5fbc-212">Preliminary documentation is included in the [EF weekly status for December 5, 2019](https://github.com/dotnet/efcore/issues/15403#issuecomment-562332863).</span></span>

<span data-ttu-id="f5fbc-213">Дополнительная документация отслеживается по проблеме [#2085](https://github.com/dotnet/EntityFramework.Docs/issues/2085).</span><span class="sxs-lookup"><span data-stu-id="f5fbc-213">Additional documentation is tracked by issue [#2085](https://github.com/dotnet/EntityFramework.Docs/issues/2085).</span></span>

### <a name="simple-way-to-get-generated-sql"></a><span data-ttu-id="f5fbc-214">Простой способ получения созданного кода SQL</span><span class="sxs-lookup"><span data-stu-id="f5fbc-214">Simple way to get generated SQL</span></span>

<span data-ttu-id="f5fbc-215">В EF Core 5.0 представлен метод расширения `ToQueryString`, который будет возвращать код SQL, создаваемый в EF Core при выполнении запроса LINQ.</span><span class="sxs-lookup"><span data-stu-id="f5fbc-215">EF Core 5.0 introduces the `ToQueryString` extension method, which will return the SQL that EF Core will generate when executing a LINQ query.</span></span>

<span data-ttu-id="f5fbc-216">Предварительная документация включена в раздел с описанием [еженедельного состояния EF на 9 января 2020 г](https://github.com/dotnet/efcore/issues/19549#issuecomment-572823246).</span><span class="sxs-lookup"><span data-stu-id="f5fbc-216">Preliminary documentation is included in the [EF weekly status for January 9, 2020](https://github.com/dotnet/efcore/issues/19549#issuecomment-572823246).</span></span>

<span data-ttu-id="f5fbc-217">Дополнительная документация отслеживается по проблеме [#1331](https://github.com/dotnet/EntityFramework.Docs/issues/1331).</span><span class="sxs-lookup"><span data-stu-id="f5fbc-217">Additional documentation is tracked by issue [#1331](https://github.com/dotnet/EntityFramework.Docs/issues/1331).</span></span>

### <a name="use-a-c-attribute-to-indicate-that-an-entity-has-no-key"></a><span data-ttu-id="f5fbc-218">Использование атрибута C# для указания того, что сущность не имеет ключа</span><span class="sxs-lookup"><span data-stu-id="f5fbc-218">Use a C# attribute to indicate that an entity has no key</span></span>

<span data-ttu-id="f5fbc-219">Теперь можно настроить тип сущности, не имеющей ключа, используя новый атрибут `KeylessAttribute`.</span><span class="sxs-lookup"><span data-stu-id="f5fbc-219">An entity type can now be configured as having no key using the new `KeylessAttribute`.</span></span>
<span data-ttu-id="f5fbc-220">Пример:</span><span class="sxs-lookup"><span data-stu-id="f5fbc-220">For example:</span></span>

```CSharp
[Keyless]
public class Address
{
    public string Street { get; set; }
    public string City { get; set; }
    public int Zip { get; set; }
}
```

<span data-ttu-id="f5fbc-221">Документация отслеживается по проблеме [#2186](https://github.com/dotnet/EntityFramework.Docs/issues/2186).</span><span class="sxs-lookup"><span data-stu-id="f5fbc-221">Documentation is tracked by issue [#2186](https://github.com/dotnet/EntityFramework.Docs/issues/2186).</span></span>

### <a name="connection-or-connection-string-can-be-changed-on-initialized-dbcontext"></a><span data-ttu-id="f5fbc-222">Возможность изменения подключения или строки подключения в инициализированном DbContext</span><span class="sxs-lookup"><span data-stu-id="f5fbc-222">Connection or connection string can be changed on initialized DbContext</span></span>

<span data-ttu-id="f5fbc-223">Теперь создание экземпляра DbContext без подключения или строки подключения стало еще проще.</span><span class="sxs-lookup"><span data-stu-id="f5fbc-223">It is now easier to create a DbContext instance without any connection or connection string.</span></span>
<span data-ttu-id="f5fbc-224">Кроме того, подключение или строку подключения можно изменить в экземпляре контекста.</span><span class="sxs-lookup"><span data-stu-id="f5fbc-224">Also, the connection or connection string can now be mutated on the context instance.</span></span>
<span data-ttu-id="f5fbc-225">Эта возможность позволяет одному и тому же экземпляру контекста динамически подключаться к разным базам данных.</span><span class="sxs-lookup"><span data-stu-id="f5fbc-225">This feature allows the same context instance to dynamically connect to different databases.</span></span>

<span data-ttu-id="f5fbc-226">Документация отслеживается по проблеме [#2075](https://github.com/dotnet/EntityFramework.Docs/issues/2075).</span><span class="sxs-lookup"><span data-stu-id="f5fbc-226">Documentation is tracked by issue [#2075](https://github.com/dotnet/EntityFramework.Docs/issues/2075).</span></span>

### <a name="change-tracking-proxies"></a><span data-ttu-id="f5fbc-227">Прокси-серверы отслеживания изменений</span><span class="sxs-lookup"><span data-stu-id="f5fbc-227">Change-tracking proxies</span></span>

<span data-ttu-id="f5fbc-228">Теперь EF Core может создавать прокси-серверы среды выполнения, которые автоматически реализуют интерфейсы [INotifyPropertyChanging](https://docs.microsoft.com/dotnet/api/system.componentmodel.inotifypropertychanging?view=netcore-3.1) и [INotifyPropertyChanged](https://docs.microsoft.com/dotnet/api/system.componentmodel.inotifypropertychanged?view=netcore-3.1).</span><span class="sxs-lookup"><span data-stu-id="f5fbc-228">EF Core can now generate runtime proxies that automatically implement [INotifyPropertyChanging](https://docs.microsoft.com/dotnet/api/system.componentmodel.inotifypropertychanging?view=netcore-3.1) and [INotifyPropertyChanged](https://docs.microsoft.com/dotnet/api/system.componentmodel.inotifypropertychanged?view=netcore-3.1).</span></span>
<span data-ttu-id="f5fbc-229">Затем они сообщают об изменениях значений свойств сущности напрямую в EF Core, что позволяет не выполнять проверку на наличие изменений.</span><span class="sxs-lookup"><span data-stu-id="f5fbc-229">These then report value changes on entity properties directly to EF Core, avoiding the need to scan for changes.</span></span>
<span data-ttu-id="f5fbc-230">Однако прокси-серверы имеют собственный набор ограничений, поэтому они предназначены не для всех.</span><span class="sxs-lookup"><span data-stu-id="f5fbc-230">However, proxies come with their own set of limitations, so they are not for everyone.</span></span>

<span data-ttu-id="f5fbc-231">Документация отслеживается по проблеме [#2076](https://github.com/dotnet/EntityFramework.Docs/issues/2076).</span><span class="sxs-lookup"><span data-stu-id="f5fbc-231">Documentation is tracked by issue [#2076](https://github.com/dotnet/EntityFramework.Docs/issues/2076).</span></span>

### <a name="enhanced-debug-views"></a><span data-ttu-id="f5fbc-232">Улучшенные представления отладки</span><span class="sxs-lookup"><span data-stu-id="f5fbc-232">Enhanced debug views</span></span>

<span data-ttu-id="f5fbc-233">В представлениях отладки удобно просматривать внутренние компоненты EF Core при отладке.</span><span class="sxs-lookup"><span data-stu-id="f5fbc-233">Debug views are an easy way to look at the internals of EF Core when debugging issues.</span></span>
<span data-ttu-id="f5fbc-234">Представление отладки для модели было реализовано некоторое время назад.</span><span class="sxs-lookup"><span data-stu-id="f5fbc-234">A debug view for the Model was implemented some time ago.</span></span>
<span data-ttu-id="f5fbc-235">В EF Core 5.0 представление модели стало более удобочитаемым. Кроме того, добавлено новое представление отладки для отслеживаемых сущностей в диспетчере состояний.</span><span class="sxs-lookup"><span data-stu-id="f5fbc-235">For EF Core 5.0, we have made the model view easier to read and added a new debug view for tracked entities in the state manager.</span></span>

<span data-ttu-id="f5fbc-236">Предварительная документация включена в раздел с описанием [еженедельного состояния EF на 12 декабря 2019 г](https://github.com/dotnet/efcore/issues/15403#issuecomment-565196206).</span><span class="sxs-lookup"><span data-stu-id="f5fbc-236">Preliminary documentation is included in the [EF weekly status for December 12, 2019](https://github.com/dotnet/efcore/issues/15403#issuecomment-565196206).</span></span>

<span data-ttu-id="f5fbc-237">Дополнительная документация отслеживается по проблеме [#2086](https://github.com/dotnet/EntityFramework.Docs/issues/2086).</span><span class="sxs-lookup"><span data-stu-id="f5fbc-237">Additional documentation is tracked by issue [#2086](https://github.com/dotnet/EntityFramework.Docs/issues/2086).</span></span>

### <a name="improved-handling-of-database-null-semantics"></a><span data-ttu-id="f5fbc-238">Улучшенная обработка семантики значений NULL в базе данных</span><span class="sxs-lookup"><span data-stu-id="f5fbc-238">Improved handling of database null semantics</span></span>

<span data-ttu-id="f5fbc-239">Реляционные базы данных обычно обрабатывают NULL как неизвестное значение и, следовательно, как не равное любому другому значению NULL.</span><span class="sxs-lookup"><span data-stu-id="f5fbc-239">Relational databases typically treat NULL as an unknown value and therefore not equal to any other NULL.</span></span>
<span data-ttu-id="f5fbc-240">C# рассматривает NULL как определенное значение, которое аналогично любому другому значению NULL.</span><span class="sxs-lookup"><span data-stu-id="f5fbc-240">While C# treats null as a defined value, which compares equal to any other null.</span></span>
<span data-ttu-id="f5fbc-241">EF Core по умолчанию преобразует запросы так, чтобы они использовали семантику значений NULL C#.</span><span class="sxs-lookup"><span data-stu-id="f5fbc-241">EF Core by default translates queries so that they use C# null semantics.</span></span>
<span data-ttu-id="f5fbc-242">В EF Core 5.0 эффективность таких преобразований значительно повышена.</span><span class="sxs-lookup"><span data-stu-id="f5fbc-242">EF Core 5.0 greatly improves the efficiency of these translations.</span></span>

<span data-ttu-id="f5fbc-243">Документация отслеживается по проблеме [#1612](https://github.com/dotnet/EntityFramework.Docs/issues/1612).</span><span class="sxs-lookup"><span data-stu-id="f5fbc-243">Documentation is tracked by issue [#1612](https://github.com/dotnet/EntityFramework.Docs/issues/1612).</span></span>

### <a name="indexer-properties"></a><span data-ttu-id="f5fbc-244">Свойства индексатора</span><span class="sxs-lookup"><span data-stu-id="f5fbc-244">Indexer properties</span></span>

<span data-ttu-id="f5fbc-245">EF Core 5.0 поддерживает сопоставление свойств индексатора C#.</span><span class="sxs-lookup"><span data-stu-id="f5fbc-245">EF Core 5.0 supports mapping of C# indexer properties.</span></span>
<span data-ttu-id="f5fbc-246">Эти свойства позволяют сущностям выступать в качестве контейнеров свойств, в которых столбцы сопоставляются с именованными свойствами в контейнере.</span><span class="sxs-lookup"><span data-stu-id="f5fbc-246">These properties allow entities to act as property bags where columns are mapped to named properties in the bag.</span></span>

<span data-ttu-id="f5fbc-247">Документация отслеживается по проблеме [#2018](https://github.com/dotnet/EntityFramework.Docs/issues/2018).</span><span class="sxs-lookup"><span data-stu-id="f5fbc-247">Documentation is tracked by issue [#2018](https://github.com/dotnet/EntityFramework.Docs/issues/2018).</span></span>

### <a name="generation-of-check-constraints-for-enum-mappings"></a><span data-ttu-id="f5fbc-248">Создание ограничений CHECK для сопоставлений перечислений</span><span class="sxs-lookup"><span data-stu-id="f5fbc-248">Generation of check constraints for enum mappings</span></span>

<span data-ttu-id="f5fbc-249">Функция миграции в EF Core 5.0 теперь может создавать ограничения CHECK для сопоставлений свойств перечисления.</span><span class="sxs-lookup"><span data-stu-id="f5fbc-249">EF Core 5.0 Migrations can now generate CHECK constraints for enum property mappings.</span></span>
<span data-ttu-id="f5fbc-250">Пример:</span><span class="sxs-lookup"><span data-stu-id="f5fbc-250">For example:</span></span>

```SQL
MyEnumColumn VARCHAR(10) NOT NULL CHECK (MyEnumColumn IN ('Useful', 'Useless', 'Unknown'))
```

<span data-ttu-id="f5fbc-251">Документация отслеживается по проблеме [#2082](https://github.com/dotnet/EntityFramework.Docs/issues/2082).</span><span class="sxs-lookup"><span data-stu-id="f5fbc-251">Documentation is tracked by issue [#2082](https://github.com/dotnet/EntityFramework.Docs/issues/2082).</span></span>

### <a name="isrelational"></a><span data-ttu-id="f5fbc-252">Является реляционным</span><span class="sxs-lookup"><span data-stu-id="f5fbc-252">IsRelational</span></span>

<span data-ttu-id="f5fbc-253">В дополнение к существующим методам `IsSqlServer`, `IsSqlite` и `IsInMemory` был добавлен новый метод `IsRelational`.</span><span class="sxs-lookup"><span data-stu-id="f5fbc-253">A new `IsRelational` method has been added in addition to the existing `IsSqlServer`, `IsSqlite`, and `IsInMemory`.</span></span>
<span data-ttu-id="f5fbc-254">Этот метод можно использовать для проверки того, использует ли DbContext какой-либо поставщик реляционной базы данных.</span><span class="sxs-lookup"><span data-stu-id="f5fbc-254">This method can be used to test if the DbContext is using any relational database provider.</span></span>
<span data-ttu-id="f5fbc-255">Пример:</span><span class="sxs-lookup"><span data-stu-id="f5fbc-255">For example:</span></span>

```CSharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    if (Database.IsRelational())
    {
        // Do relational-specific model configuration.
    }
}
```

<span data-ttu-id="f5fbc-256">Документация отслеживается по проблеме [#2185](https://github.com/dotnet/EntityFramework.Docs/issues/2185).</span><span class="sxs-lookup"><span data-stu-id="f5fbc-256">Documentation is tracked by issue [#2185](https://github.com/dotnet/EntityFramework.Docs/issues/2185).</span></span>

### <a name="cosmos-optimistic-concurrency-with-etags"></a><span data-ttu-id="f5fbc-257">Оптимистический параллелизм Cosmos с тегами ETag</span><span class="sxs-lookup"><span data-stu-id="f5fbc-257">Cosmos optimistic concurrency with ETags</span></span>

<span data-ttu-id="f5fbc-258">Поставщик базы данных Azure Cosmos DB теперь поддерживает оптимистичный параллелизм с использованием тегов ETag.</span><span class="sxs-lookup"><span data-stu-id="f5fbc-258">The Azure Cosmos DB database provider now supports optimistic concurrency using ETags.</span></span>
<span data-ttu-id="f5fbc-259">Используйте построитель моделей в OnModelCreating для настройки ETag:</span><span class="sxs-lookup"><span data-stu-id="f5fbc-259">Use the model builder in OnModelCreating to configure an ETag:</span></span>

```CSharp
builder.Entity<Customer>().Property(c => c.ETag).IsEtagConcurrency();
```

<span data-ttu-id="f5fbc-260">Затем SaveChanges генерирует исключение `DbUpdateConcurrencyException` при конфликте параллелизма, которое [можно обработать](https://docs.microsoft.com/ef/core/saving/concurrency) для реализации повторных попыток и т. д.</span><span class="sxs-lookup"><span data-stu-id="f5fbc-260">SaveChanges will then throw an `DbUpdateConcurrencyException` on a concurrency conflict, which [can be handled](https://docs.microsoft.com/ef/core/saving/concurrency) to implement retries, etc.</span></span>

<span data-ttu-id="f5fbc-261">Документация отслеживается по проблеме [#2099](https://github.com/dotnet/EntityFramework.Docs/issues/2099).</span><span class="sxs-lookup"><span data-stu-id="f5fbc-261">Documentation is tracked by issue [#2099](https://github.com/dotnet/EntityFramework.Docs/issues/2099).</span></span>

### <a name="query-translations-for-more-datetime-constructs"></a><span data-ttu-id="f5fbc-262">Преобразования запросов для дополнительных конструкций DateTime</span><span class="sxs-lookup"><span data-stu-id="f5fbc-262">Query translations for more DateTime constructs</span></span>

<span data-ttu-id="f5fbc-263">Теперь запросы, содержащие новую конструкцию DateTime, поддерживают преобразование.</span><span class="sxs-lookup"><span data-stu-id="f5fbc-263">Queries containing new DateTime construction are now translated.</span></span>

<span data-ttu-id="f5fbc-264">Кроме того, теперь сопоставлены следующие функции SQL Server:</span><span class="sxs-lookup"><span data-stu-id="f5fbc-264">In addition, the following SQL Server functions are now mapped:</span></span>

* <span data-ttu-id="f5fbc-265">DateDiffWeek</span><span class="sxs-lookup"><span data-stu-id="f5fbc-265">DateDiffWeek</span></span>
* <span data-ttu-id="f5fbc-266">DateFromParts</span><span class="sxs-lookup"><span data-stu-id="f5fbc-266">DateFromParts</span></span>

<span data-ttu-id="f5fbc-267">Пример:</span><span class="sxs-lookup"><span data-stu-id="f5fbc-267">For example:</span></span>

```CSharp
var count = context.Orders.Count(c => date > EF.Functions.DateFromParts(DateTime.Now.Year, 12, 25));

```

<span data-ttu-id="f5fbc-268">Документация отслеживается по проблеме [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079).</span><span class="sxs-lookup"><span data-stu-id="f5fbc-268">Documentation is tracked by issue [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079).</span></span>

### <a name="query-translations-for-more-byte-array-constructs"></a><span data-ttu-id="f5fbc-269">Преобразования запросов для дополнительных конструкций массивов байтов</span><span class="sxs-lookup"><span data-stu-id="f5fbc-269">Query translations for more byte array constructs</span></span>

<span data-ttu-id="f5fbc-270">Запросы, использующие свойства Contains, Length, SequenceEqual и т. д. в массиве byte[], преобразуются в SQL.</span><span class="sxs-lookup"><span data-stu-id="f5fbc-270">Queries using Contains, Length, SequenceEqual, etc. on byte[] properties are now translated to SQL.</span></span>

<span data-ttu-id="f5fbc-271">Предварительная документация включена в раздел с описанием [еженедельного состояния EF на 5 декабря 2019 г](https://github.com/dotnet/efcore/issues/15403#issuecomment-562332863).</span><span class="sxs-lookup"><span data-stu-id="f5fbc-271">Preliminary documentation is included in the [EF weekly status for December 5, 2019](https://github.com/dotnet/efcore/issues/15403#issuecomment-562332863).</span></span>

<span data-ttu-id="f5fbc-272">Дополнительная документация отслеживается по проблеме [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079).</span><span class="sxs-lookup"><span data-stu-id="f5fbc-272">Additional documentation is tracked by issue [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079).</span></span>

### <a name="query-translation-for-reverse"></a><span data-ttu-id="f5fbc-273">Преобразование запросов, использующих Reverse</span><span class="sxs-lookup"><span data-stu-id="f5fbc-273">Query translation for Reverse</span></span>

<span data-ttu-id="f5fbc-274">Теперь можно преобразовывать запросы, использующие `Reverse`.</span><span class="sxs-lookup"><span data-stu-id="f5fbc-274">Queries using `Reverse` are now translated.</span></span>
<span data-ttu-id="f5fbc-275">Пример:</span><span class="sxs-lookup"><span data-stu-id="f5fbc-275">For example:</span></span>

```CSharp
context.Employees.OrderBy(e => e.EmployeeID).Reverse()
```

<span data-ttu-id="f5fbc-276">Документация отслеживается по проблеме [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079).</span><span class="sxs-lookup"><span data-stu-id="f5fbc-276">Documentation is tracked by issue [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079).</span></span>

### <a name="query-translation-for-bitwise-operators"></a><span data-ttu-id="f5fbc-277">Преобразование запросов для побитовых операторов</span><span class="sxs-lookup"><span data-stu-id="f5fbc-277">Query translation for bitwise operators</span></span>

<span data-ttu-id="f5fbc-278">Запросы, использующие побитовые операторы, теперь преобразуются в ряде дополнительных случаев, например:</span><span class="sxs-lookup"><span data-stu-id="f5fbc-278">Queries using bitwise operators are now translated in more cases For example:</span></span>

```CSharp
context.Orders.Where(o => ~o.OrderID == negatedId)
```

<span data-ttu-id="f5fbc-279">Документация отслеживается по проблеме [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079).</span><span class="sxs-lookup"><span data-stu-id="f5fbc-279">Documentation is tracked by issue [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079).</span></span>

### <a name="query-translation-for-strings-on-cosmos"></a><span data-ttu-id="f5fbc-280">Преобразование запросов для строк в Cosmos</span><span class="sxs-lookup"><span data-stu-id="f5fbc-280">Query translation for strings on Cosmos</span></span>

<span data-ttu-id="f5fbc-281">Запросы, использующие строковые методы Contains, StartsWith и EndsWith, теперь преобразуются при использовании поставщика Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="f5fbc-281">Queries that use the string methods Contains, StartsWith, and EndsWith are now translated when using the Azure Cosmos DB provider.</span></span>

<span data-ttu-id="f5fbc-282">Документация отслеживается по проблеме [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079).</span><span class="sxs-lookup"><span data-stu-id="f5fbc-282">Documentation is tracked by issue [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079).</span></span>
