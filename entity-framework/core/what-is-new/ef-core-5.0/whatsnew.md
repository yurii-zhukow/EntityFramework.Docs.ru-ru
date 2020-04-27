---
title: Новые возможности EF Core 5.0
description: Обзор новых возможностей в EF Core 5.0
author: ajcvickers
ms.date: 03/30/2020
uid: core/what-is-new/ef-core-5.0/whatsnew.md
ms.openlocfilehash: c902988920e3b1a6039808fe0658fc19dee2728a
ms.sourcegitcommit: 387cbd8109c0fc5ce6bdc85d0dec1aed72ad4c33
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/23/2020
ms.locfileid: "82103078"
---
# <a name="whats-new-in-ef-core-50"></a><span data-ttu-id="12fe6-103">Новые возможности EF Core 5.0</span><span class="sxs-lookup"><span data-stu-id="12fe6-103">What's New in EF Core 5.0</span></span>

<span data-ttu-id="12fe6-104">Сейчас EF Core 5.0 находится на стадии разработки.</span><span class="sxs-lookup"><span data-stu-id="12fe6-104">EF Core 5.0 is currently in development.</span></span>
<span data-ttu-id="12fe6-105">На этой странице представлен обзор интересных изменений, появившихся в каждой предварительной версии.</span><span class="sxs-lookup"><span data-stu-id="12fe6-105">This page will contain an overview of interesting changes introduced in each preview.</span></span>

<span data-ttu-id="12fe6-106">Эта страница не является аналогом статьи о [планировании для EF Core 5.0](plan.md).</span><span class="sxs-lookup"><span data-stu-id="12fe6-106">This page does not duplicate the [plan for EF Core 5.0](plan.md).</span></span>
<span data-ttu-id="12fe6-107">В плане описываются общие темы для EF Core 5.0, в том числе все, что планируется включить перед выпуском окончательной версии.</span><span class="sxs-lookup"><span data-stu-id="12fe6-107">The plan describes the overall themes for EF Core 5.0, including everything we are planning to include before shipping the final release.</span></span>

<span data-ttu-id="12fe6-108">Мы будем добавлять ссылки на официальную документацию по мере ее публикации.</span><span class="sxs-lookup"><span data-stu-id="12fe6-108">We will add links from here to the official documentation as it is published.</span></span>

## <a name="preview-3"></a><span data-ttu-id="12fe6-109">Предварительная версия 3</span><span class="sxs-lookup"><span data-stu-id="12fe6-109">Preview 3</span></span>

### <a name="filtered-include"></a><span data-ttu-id="12fe6-110">Включение с фильтрацией</span><span class="sxs-lookup"><span data-stu-id="12fe6-110">Filtered Include</span></span>

<span data-ttu-id="12fe6-111">Метод Include теперь поддерживает фильтрацию включенных сущностей.</span><span class="sxs-lookup"><span data-stu-id="12fe6-111">The Include method now supports filtering of the entities included.</span></span>
<span data-ttu-id="12fe6-112">Пример:</span><span class="sxs-lookup"><span data-stu-id="12fe6-112">For example:</span></span>

```CSharp
var blogs = context.Blogs
    .Include(e => e.Posts.Where(p => p.Title.Contains("Cheese")))
    .ToList();
```

<span data-ttu-id="12fe6-113">Этот запрос возвращает блоги со всеми связанными записями, но только в том случае, если заголовок записи содержит Cheese.</span><span class="sxs-lookup"><span data-stu-id="12fe6-113">This query will return blogs together with each associated post, but only when the post title contains "Cheese".</span></span>

<span data-ttu-id="12fe6-114">Для сокращения числа включаемых сущностей также можно использовать методы Skip и Take.</span><span class="sxs-lookup"><span data-stu-id="12fe6-114">Skip and Take can also be used to reduce the number of included entities.</span></span>
<span data-ttu-id="12fe6-115">Пример:</span><span class="sxs-lookup"><span data-stu-id="12fe6-115">For example:</span></span>
 
```CSharp
var blogs = context.Blogs
    .Include(e => e.Posts.OrderByDescending(post => post.Title).Take(5)))
    .ToList();
```
<span data-ttu-id="12fe6-116">Этот запрос возвратит блоги, включив не более пяти записей для каждого блога.</span><span class="sxs-lookup"><span data-stu-id="12fe6-116">This query will return blogs with at most five posts included on each blog.</span></span>

<span data-ttu-id="12fe6-117">Подробные сведения см. в [документации по методу Include](xref:core/querying/related-data#filtered-include).</span><span class="sxs-lookup"><span data-stu-id="12fe6-117">See the [Include documentation](xref:core/querying/related-data#filtered-include) for full details.</span></span>

### <a name="new-modelbuilder-api-for-navigation-properties"></a><span data-ttu-id="12fe6-118">Новый API ModelBuilder для свойств навигации</span><span class="sxs-lookup"><span data-stu-id="12fe6-118">New ModelBuilder API for navigation properties</span></span>

<span data-ttu-id="12fe6-119">Свойства навигации главным образом настраиваются при [определении связей](xref:core/modeling/relationships).</span><span class="sxs-lookup"><span data-stu-id="12fe6-119">Navigation properties are primarily configured when [defining relationships](xref:core/modeling/relationships).</span></span>
<span data-ttu-id="12fe6-120">Однако новый метод `Navigation` можно использовать в случаях, когда свойства навигации требуют дополнительной настройки.</span><span class="sxs-lookup"><span data-stu-id="12fe6-120">However, the new `Navigation` method can be used in the cases where navigation properties need additional configuration.</span></span>
<span data-ttu-id="12fe6-121">Например, чтобы задать резервное поле для навигации, если не удалось найти поле по соглашению:</span><span class="sxs-lookup"><span data-stu-id="12fe6-121">For example, to set a backing field for the navigation when the field would not be found by convention:</span></span>

```CSharp
modelBuilder.Entity<Blog>().Navigation(e => e.Posts).HasField("_myposts");
```

<span data-ttu-id="12fe6-122">Обратите внимание, что API `Navigation` не является заменой для конфигурации связи.</span><span class="sxs-lookup"><span data-stu-id="12fe6-122">Note that the `Navigation` API does not replace relationship configuration.</span></span>
<span data-ttu-id="12fe6-123">Вместо этого он допускает дополнительную настройку свойств навигации в уже обнаруженных или определенных связях.</span><span class="sxs-lookup"><span data-stu-id="12fe6-123">Instead it allows additional configuration of navigation properties in already discovered or defined relationships.</span></span>

<span data-ttu-id="12fe6-124">Документация отслеживается по проблеме [#2302](https://github.com/dotnet/EntityFramework.Docs/issues/2302).</span><span class="sxs-lookup"><span data-stu-id="12fe6-124">Documentation is tracked by issue [#2302](https://github.com/dotnet/EntityFramework.Docs/issues/2302).</span></span>

### <a name="new-command-line-parameters-for-namespaces-and-connection-strings"></a><span data-ttu-id="12fe6-125">Новые параметры командной строки для пространств имен и строк подключения</span><span class="sxs-lookup"><span data-stu-id="12fe6-125">New command-line parameters for namespaces and connection strings</span></span> 

<span data-ttu-id="12fe6-126">Теперь можно указывать пространства имен в командной строке во время миграции и формирования шаблонов.</span><span class="sxs-lookup"><span data-stu-id="12fe6-126">Migrations and scaffolding now allow namespaces to be specified on the command line.</span></span>
<span data-ttu-id="12fe6-127">Это можно сделать, например, затем, чтобы реконструировать базу данных, поместив в нее классы контекста и модели в различных пространствах имен:</span><span class="sxs-lookup"><span data-stu-id="12fe6-127">For example, to reverse engineer a database putting the context and model classes in different namespaces:</span></span> 

```
dotnet ef dbcontext scaffold "connection string" Microsoft.EntityFrameworkCore.SqlServer --context-namespace "My.Context" --namespace "My.Model"
```

<span data-ttu-id="12fe6-128">Кроме того, теперь можно передать строку подключения команде `database-update`.</span><span class="sxs-lookup"><span data-stu-id="12fe6-128">Also, a connection string can now be passed to the `database-update` command:</span></span>

```
dotnet ef database update --connection "connection string"
```

<span data-ttu-id="12fe6-129">Аналогичные параметры также были добавлены в команды PowerShell, используемые в консоли диспетчера пакетов VS.</span><span class="sxs-lookup"><span data-stu-id="12fe6-129">Equivalent parameters have also been added to the PowerShell commands used in the VS Package Manager Console.</span></span>

<span data-ttu-id="12fe6-130">Документация отслеживается по проблеме [#2303](https://github.com/dotnet/EntityFramework.Docs/issues/2303).</span><span class="sxs-lookup"><span data-stu-id="12fe6-130">Documentation is tracked by issue [#2303](https://github.com/dotnet/EntityFramework.Docs/issues/2303).</span></span>

### <a name="enabledetailederrors-has-returned"></a><span data-ttu-id="12fe6-131">Возвращение EnableDetailedErrors</span><span class="sxs-lookup"><span data-stu-id="12fe6-131">EnableDetailedErrors has returned</span></span>

<span data-ttu-id="12fe6-132">По соображениям производительности EF не выполняет дополнительные проверки на NULL при считывании значений из базы данных.</span><span class="sxs-lookup"><span data-stu-id="12fe6-132">For performance reasons, EF doesn't do additional null-checks when reading values from the database.</span></span>
<span data-ttu-id="12fe6-133">Это может привести к возникновению исключений при обнаружении непредвиденных значений NULL, для которых будет сложно определить причину.</span><span class="sxs-lookup"><span data-stu-id="12fe6-133">This can result in exceptions that are hard to root-cause when an unexpected null is encountered.</span></span>

<span data-ttu-id="12fe6-134">При использовании `EnableDetailedErrors` будет добавлена дополнительная проверка на NULL для запросов, чтобы облегчить определение причин этих ошибок за счет небольшого снижения производительности.</span><span class="sxs-lookup"><span data-stu-id="12fe6-134">Using `EnableDetailedErrors` will add extra null checking to queries such that, for a small performance overhead, these errors are easier to trace back to a root cause.</span></span>  

<span data-ttu-id="12fe6-135">Пример:</span><span class="sxs-lookup"><span data-stu-id="12fe6-135">For example:</span></span>
```CSharp
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    => optionsBuilder
        .EnableDetailedErrors()
        .EnableSensitiveDataLogging() // Often also useful with EnableDetailedErrors 
        .UseSqlServer(Your.SqlServerConnectionString);
```

<span data-ttu-id="12fe6-136">Документация отслеживается по проблеме [#955](https://github.com/dotnet/EntityFramework.Docs/issues/955).</span><span class="sxs-lookup"><span data-stu-id="12fe6-136">Documentation is tracked by issue [#955](https://github.com/dotnet/EntityFramework.Docs/issues/955).</span></span>

### <a name="cosmos-partition-keys"></a><span data-ttu-id="12fe6-137">Ключи секций Cosmos</span><span class="sxs-lookup"><span data-stu-id="12fe6-137">Cosmos partition keys</span></span>

<span data-ttu-id="12fe6-138">Теперь можно указать в запросе ключ секции, используемый для данного запроса.</span><span class="sxs-lookup"><span data-stu-id="12fe6-138">The partition key to use for a given query can now be specified in the query.</span></span>
<span data-ttu-id="12fe6-139">Пример:</span><span class="sxs-lookup"><span data-stu-id="12fe6-139">For example:</span></span>

```CSharp
await context.Set<Customer>()
             .WithPartitionKey(myPartitionKey)
             .FirstAsync();
```

<span data-ttu-id="12fe6-140">Документация отслеживается по проблеме [#2199](https://github.com/dotnet/EntityFramework.Docs/issues/2199).</span><span class="sxs-lookup"><span data-stu-id="12fe6-140">Documentation is tracked by issue [#2199](https://github.com/dotnet/EntityFramework.Docs/issues/2199).</span></span>

### <a name="support-for-the-sql-server-datalength-function"></a><span data-ttu-id="12fe6-141">Поддержка функции DATALENGTH SQL Server</span><span class="sxs-lookup"><span data-stu-id="12fe6-141">Support for the SQL Server DATALENGTH function</span></span>

<span data-ttu-id="12fe6-142">К ней можно обратиться с помощью нового метода `EF.Functions.DataLength`.</span><span class="sxs-lookup"><span data-stu-id="12fe6-142">This can be accessed using the new `EF.Functions.DataLength` method.</span></span>
<span data-ttu-id="12fe6-143">Пример:</span><span class="sxs-lookup"><span data-stu-id="12fe6-143">For example:</span></span>
```CSharp
var count = context.Orders.Count(c => 100 < EF.Functions.DataLength(c.OrderDate));
``` 

## <a name="preview-2"></a><span data-ttu-id="12fe6-144">Предварительная версия 2</span><span class="sxs-lookup"><span data-stu-id="12fe6-144">Preview 2</span></span>

### <a name="use-a-c-attribute-to-specify-a-property-backing-field"></a><span data-ttu-id="12fe6-145">Использование атрибута C# для указания резервного поля свойств</span><span class="sxs-lookup"><span data-stu-id="12fe6-145">Use a C# attribute to specify a property backing field</span></span>

<span data-ttu-id="12fe6-146">Теперь атрибут C# можно использовать для указания резервного поля для свойства.</span><span class="sxs-lookup"><span data-stu-id="12fe6-146">A C# attribute can now be used to specify the backing field for a property.</span></span>
<span data-ttu-id="12fe6-147">Этот атрибут позволяет EF Core в обычном порядке выполнять запись и чтение из резервного поля, даже если резервное поле не может быть найдено автоматически.</span><span class="sxs-lookup"><span data-stu-id="12fe6-147">This attribute allows EF Core to still write to and read from the backing field as would normally happen, even when the backing field cannot be found automatically.</span></span>
<span data-ttu-id="12fe6-148">Пример:</span><span class="sxs-lookup"><span data-stu-id="12fe6-148">For example:</span></span>

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

<span data-ttu-id="12fe6-149">Документация отслеживается по проблеме [#2230](https://github.com/dotnet/EntityFramework.Docs/issues/2230).</span><span class="sxs-lookup"><span data-stu-id="12fe6-149">Documentation is tracked by issue [#2230](https://github.com/dotnet/EntityFramework.Docs/issues/2230).</span></span>

### <a name="complete-discriminator-mapping"></a><span data-ttu-id="12fe6-150">Завершение сопоставления дискриминатора</span><span class="sxs-lookup"><span data-stu-id="12fe6-150">Complete discriminator mapping</span></span>

<span data-ttu-id="12fe6-151">EF Core использует столбец дискриминатора для [сопоставления TPH в иерархии наследования](/ef/core/modeling/inheritance).</span><span class="sxs-lookup"><span data-stu-id="12fe6-151">EF Core uses a discriminator column for [TPH mapping of an inheritance hierarchy](/ef/core/modeling/inheritance).</span></span>
<span data-ttu-id="12fe6-152">Некоторые улучшения производительности будут поддерживаться до тех пор, пока EF Core будут известны все возможные значения для дискриминатора.</span><span class="sxs-lookup"><span data-stu-id="12fe6-152">Some performance enhancements are possible so long as EF Core knows all possible values for the discriminator.</span></span>
<span data-ttu-id="12fe6-153">Теперь эти улучшения реализованы в EF Core 5.0.</span><span class="sxs-lookup"><span data-stu-id="12fe6-153">EF Core 5.0 now implements these enhancements.</span></span>

<span data-ttu-id="12fe6-154">Например, в предыдущих версиях EF Core всегда создавался следующий код SQL для запроса, возвращающего все типы в иерархии:</span><span class="sxs-lookup"><span data-stu-id="12fe6-154">For example, previous versions of EF Core would always generate this SQL for a query returning all types in a hierarchy:</span></span>

```sql
SELECT [a].[Id], [a].[Discriminator], [a].[Name]
FROM [Animal] AS [a]
WHERE [a].[Discriminator] IN (N'Animal', N'Cat', N'Dog', N'Human')
```

<span data-ttu-id="12fe6-155">Теперь при настройке полного сопоставления дискриминатора EF Core 5.0 будет формировать следующее:</span><span class="sxs-lookup"><span data-stu-id="12fe6-155">EF Core 5.0 will now generate the following when a complete discriminator mapping is configured:</span></span>

```sql
SELECT [a].[Id], [a].[Discriminator], [a].[Name]
FROM [Animal] AS [a]
```

<span data-ttu-id="12fe6-156">Начиная с предварительной версии 3 это поведение будет доступно по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="12fe6-156">It will be the default behavior starting with preview 3.</span></span>

### <a name="performance-improvements-in-microsoftdatasqlite"></a><span data-ttu-id="12fe6-157">Улучшения производительности в Microsoft.Data.Sqlite</span><span class="sxs-lookup"><span data-stu-id="12fe6-157">Performance improvements in Microsoft.Data.Sqlite</span></span>

<span data-ttu-id="12fe6-158">В производительность SQLIte были внесены два улучшения:</span><span class="sxs-lookup"><span data-stu-id="12fe6-158">We have made two performance improvements for SQLIte:</span></span>

* <span data-ttu-id="12fe6-159">Теперь получение двоичных и строковых данных с помощью GetBytes, GetChars и GetTextReader осуществляется более эффективно за счет использования SqliteBlob и потоков.</span><span class="sxs-lookup"><span data-stu-id="12fe6-159">Retrieving binary and string data with GetBytes, GetChars, and GetTextReader is now more efficient by making use of SqliteBlob and streams.</span></span>
* <span data-ttu-id="12fe6-160">Инициализация SqliteConnection выполняется в отложенном режиме.</span><span class="sxs-lookup"><span data-stu-id="12fe6-160">Initialization of SqliteConnection is now lazy.</span></span>

<span data-ttu-id="12fe6-161">Эти улучшения добавлены в поставщик Microsoft.Data.Sqlite ADO.NET и, следовательно, позволяют повысить производительность за пределами EF Core.</span><span class="sxs-lookup"><span data-stu-id="12fe6-161">These improvements are in the ADO.NET Microsoft.Data.Sqlite provider and hence also improve performance outside of EF Core.</span></span>

## <a name="preview-1"></a><span data-ttu-id="12fe6-162">Предварительная версия 1</span><span class="sxs-lookup"><span data-stu-id="12fe6-162">Preview 1</span></span>

### <a name="simple-logging"></a><span data-ttu-id="12fe6-163">Простое ведение журнала</span><span class="sxs-lookup"><span data-stu-id="12fe6-163">Simple logging</span></span>

<span data-ttu-id="12fe6-164">Эта функция добавляет возможность, аналогичную `Database.Log` в EF6.</span><span class="sxs-lookup"><span data-stu-id="12fe6-164">This feature adds functionality similar to `Database.Log` in EF6.</span></span>
<span data-ttu-id="12fe6-165">Это значит, что она позволяет легко получать журналы из EF Core без настройки какой-либо внешней платформы ведения журналов.</span><span class="sxs-lookup"><span data-stu-id="12fe6-165">That is, it provides a simple way to get logs from EF Core without the need to configure any kind of external logging framework.</span></span>

<span data-ttu-id="12fe6-166">Предварительная документация включена в раздел с описанием [еженедельного состояния EF на 5 декабря 2019 г](https://github.com/dotnet/efcore/issues/15403#issuecomment-562332863).</span><span class="sxs-lookup"><span data-stu-id="12fe6-166">Preliminary documentation is included in the [EF weekly status for December 5, 2019](https://github.com/dotnet/efcore/issues/15403#issuecomment-562332863).</span></span>

<span data-ttu-id="12fe6-167">Дополнительная документация отслеживается по проблеме [#2085](https://github.com/dotnet/EntityFramework.Docs/issues/2085).</span><span class="sxs-lookup"><span data-stu-id="12fe6-167">Additional documentation is tracked by issue [#2085](https://github.com/dotnet/EntityFramework.Docs/issues/2085).</span></span>

### <a name="simple-way-to-get-generated-sql"></a><span data-ttu-id="12fe6-168">Простой способ получения созданного кода SQL</span><span class="sxs-lookup"><span data-stu-id="12fe6-168">Simple way to get generated SQL</span></span>

<span data-ttu-id="12fe6-169">В EF Core 5.0 представлен метод расширения `ToQueryString`, который будет возвращать код SQL, создаваемый в EF Core при выполнении запроса LINQ.</span><span class="sxs-lookup"><span data-stu-id="12fe6-169">EF Core 5.0 introduces the `ToQueryString` extension method, which will return the SQL that EF Core will generate when executing a LINQ query.</span></span>

<span data-ttu-id="12fe6-170">Предварительная документация включена в раздел с описанием [еженедельного состояния EF на 9 января 2020 г](https://github.com/dotnet/efcore/issues/19549#issuecomment-572823246).</span><span class="sxs-lookup"><span data-stu-id="12fe6-170">Preliminary documentation is included in the [EF weekly status for January 9, 2020](https://github.com/dotnet/efcore/issues/19549#issuecomment-572823246).</span></span>

<span data-ttu-id="12fe6-171">Дополнительная документация отслеживается по проблеме [#1331](https://github.com/dotnet/EntityFramework.Docs/issues/1331).</span><span class="sxs-lookup"><span data-stu-id="12fe6-171">Additional documentation is tracked by issue [#1331](https://github.com/dotnet/EntityFramework.Docs/issues/1331).</span></span>

### <a name="use-a-c-attribute-to-indicate-that-an-entity-has-no-key"></a><span data-ttu-id="12fe6-172">Использование атрибута C# для указания того, что сущность не имеет ключа</span><span class="sxs-lookup"><span data-stu-id="12fe6-172">Use a C# attribute to indicate that an entity has no key</span></span>

<span data-ttu-id="12fe6-173">Теперь можно настроить тип сущности, не имеющей ключа, используя новый атрибут `KeylessAttribute`.</span><span class="sxs-lookup"><span data-stu-id="12fe6-173">An entity type can now be configured as having no key using the new `KeylessAttribute`.</span></span>
<span data-ttu-id="12fe6-174">Пример:</span><span class="sxs-lookup"><span data-stu-id="12fe6-174">For example:</span></span>

```CSharp
[Keyless]
public class Address
{
    public string Street { get; set; }
    public string City { get; set; }
    public int Zip { get; set; }
}
```

<span data-ttu-id="12fe6-175">Документация отслеживается по проблеме [#2186](https://github.com/dotnet/EntityFramework.Docs/issues/2186).</span><span class="sxs-lookup"><span data-stu-id="12fe6-175">Documentation is tracked by issue [#2186](https://github.com/dotnet/EntityFramework.Docs/issues/2186).</span></span>

### <a name="connection-or-connection-string-can-be-changed-on-initialized-dbcontext"></a><span data-ttu-id="12fe6-176">Возможность изменения подключения или строки подключения в инициализированном DbContext</span><span class="sxs-lookup"><span data-stu-id="12fe6-176">Connection or connection string can be changed on initialized DbContext</span></span>

<span data-ttu-id="12fe6-177">Теперь создание экземпляра DbContext без подключения или строки подключения стало еще проще.</span><span class="sxs-lookup"><span data-stu-id="12fe6-177">It is now easier to create a DbContext instance without any connection or connection string.</span></span>
<span data-ttu-id="12fe6-178">Кроме того, подключение или строку подключения можно изменить в экземпляре контекста.</span><span class="sxs-lookup"><span data-stu-id="12fe6-178">Also, the connection or connection string can now be mutated on the context instance.</span></span>
<span data-ttu-id="12fe6-179">Эта возможность позволяет одному и тому же экземпляру контекста динамически подключаться к разным базам данных.</span><span class="sxs-lookup"><span data-stu-id="12fe6-179">This feature allows the same context instance to dynamically connect to different databases.</span></span>

<span data-ttu-id="12fe6-180">Документация отслеживается по проблеме [#2075](https://github.com/dotnet/EntityFramework.Docs/issues/2075).</span><span class="sxs-lookup"><span data-stu-id="12fe6-180">Documentation is tracked by issue [#2075](https://github.com/dotnet/EntityFramework.Docs/issues/2075).</span></span>

### <a name="change-tracking-proxies"></a><span data-ttu-id="12fe6-181">Прокси-серверы отслеживания изменений</span><span class="sxs-lookup"><span data-stu-id="12fe6-181">Change-tracking proxies</span></span>

<span data-ttu-id="12fe6-182">Теперь EF Core может создавать прокси-серверы среды выполнения, которые автоматически реализуют интерфейсы [INotifyPropertyChanging](https://docs.microsoft.com/dotnet/api/system.componentmodel.inotifypropertychanging?view=netcore-3.1) и [INotifyPropertyChanged](https://docs.microsoft.com/dotnet/api/system.componentmodel.inotifypropertychanged?view=netcore-3.1).</span><span class="sxs-lookup"><span data-stu-id="12fe6-182">EF Core can now generate runtime proxies that automatically implement [INotifyPropertyChanging](https://docs.microsoft.com/dotnet/api/system.componentmodel.inotifypropertychanging?view=netcore-3.1) and [INotifyPropertyChanged](https://docs.microsoft.com/dotnet/api/system.componentmodel.inotifypropertychanged?view=netcore-3.1).</span></span>
<span data-ttu-id="12fe6-183">Затем они сообщают об изменениях значений свойств сущности напрямую в EF Core, что позволяет не выполнять проверку на наличие изменений.</span><span class="sxs-lookup"><span data-stu-id="12fe6-183">These then report value changes on entity properties directly to EF Core, avoiding the need to scan for changes.</span></span>
<span data-ttu-id="12fe6-184">Однако прокси-серверы имеют собственный набор ограничений, поэтому они предназначены не для всех.</span><span class="sxs-lookup"><span data-stu-id="12fe6-184">However, proxies come with their own set of limitations, so they are not for everyone.</span></span>

<span data-ttu-id="12fe6-185">Документация отслеживается по проблеме [#2076](https://github.com/dotnet/EntityFramework.Docs/issues/2076).</span><span class="sxs-lookup"><span data-stu-id="12fe6-185">Documentation is tracked by issue [#2076](https://github.com/dotnet/EntityFramework.Docs/issues/2076).</span></span>

### <a name="enhanced-debug-views"></a><span data-ttu-id="12fe6-186">Улучшенные представления отладки</span><span class="sxs-lookup"><span data-stu-id="12fe6-186">Enhanced debug views</span></span>

<span data-ttu-id="12fe6-187">В представлениях отладки удобно просматривать внутренние компоненты EF Core при отладке.</span><span class="sxs-lookup"><span data-stu-id="12fe6-187">Debug views are an easy way to look at the internals of EF Core when debugging issues.</span></span>
<span data-ttu-id="12fe6-188">Представление отладки для модели было реализовано некоторое время назад.</span><span class="sxs-lookup"><span data-stu-id="12fe6-188">A debug view for the Model was implemented some time ago.</span></span>
<span data-ttu-id="12fe6-189">В EF Core 5.0 представление модели стало более удобочитаемым. Кроме того, добавлено новое представление отладки для отслеживаемых сущностей в диспетчере состояний.</span><span class="sxs-lookup"><span data-stu-id="12fe6-189">For EF Core 5.0, we have made the model view easier to read and added a new debug view for tracked entities in the state manager.</span></span>

<span data-ttu-id="12fe6-190">Предварительная документация включена в раздел с описанием [еженедельного состояния EF на 12 декабря 2019 г](https://github.com/dotnet/efcore/issues/15403#issuecomment-565196206).</span><span class="sxs-lookup"><span data-stu-id="12fe6-190">Preliminary documentation is included in the [EF weekly status for December 12, 2019](https://github.com/dotnet/efcore/issues/15403#issuecomment-565196206).</span></span>

<span data-ttu-id="12fe6-191">Дополнительная документация отслеживается по проблеме [#2086](https://github.com/dotnet/EntityFramework.Docs/issues/2086).</span><span class="sxs-lookup"><span data-stu-id="12fe6-191">Additional documentation is tracked by issue [#2086](https://github.com/dotnet/EntityFramework.Docs/issues/2086).</span></span>

### <a name="improved-handling-of-database-null-semantics"></a><span data-ttu-id="12fe6-192">Улучшенная обработка семантики значений NULL в базе данных</span><span class="sxs-lookup"><span data-stu-id="12fe6-192">Improved handling of database null semantics</span></span>

<span data-ttu-id="12fe6-193">Реляционные базы данных обычно обрабатывают NULL как неизвестное значение и, следовательно, как не равное любому другому значению NULL.</span><span class="sxs-lookup"><span data-stu-id="12fe6-193">Relational databases typically treat NULL as an unknown value and therefore not equal to any other NULL.</span></span>
<span data-ttu-id="12fe6-194">C# рассматривает NULL как определенное значение, которое аналогично любому другому значению NULL.</span><span class="sxs-lookup"><span data-stu-id="12fe6-194">While C# treats null as a defined value, which compares equal to any other null.</span></span>
<span data-ttu-id="12fe6-195">EF Core по умолчанию преобразует запросы так, чтобы они использовали семантику значений NULL C#.</span><span class="sxs-lookup"><span data-stu-id="12fe6-195">EF Core by default translates queries so that they use C# null semantics.</span></span>
<span data-ttu-id="12fe6-196">В EF Core 5.0 эффективность таких преобразований значительно повышена.</span><span class="sxs-lookup"><span data-stu-id="12fe6-196">EF Core 5.0 greatly improves the efficiency of these translations.</span></span>

<span data-ttu-id="12fe6-197">Документация отслеживается по проблеме [#1612](https://github.com/dotnet/EntityFramework.Docs/issues/1612).</span><span class="sxs-lookup"><span data-stu-id="12fe6-197">Documentation is tracked by issue [#1612](https://github.com/dotnet/EntityFramework.Docs/issues/1612).</span></span>

### <a name="indexer-properties"></a><span data-ttu-id="12fe6-198">Свойства индексатора</span><span class="sxs-lookup"><span data-stu-id="12fe6-198">Indexer properties</span></span>

<span data-ttu-id="12fe6-199">EF Core 5.0 поддерживает сопоставление свойств индексатора C#.</span><span class="sxs-lookup"><span data-stu-id="12fe6-199">EF Core 5.0 supports mapping of C# indexer properties.</span></span>
<span data-ttu-id="12fe6-200">Эти свойства позволяют сущностям выступать в качестве контейнеров свойств, в которых столбцы сопоставляются с именованными свойствами в контейнере.</span><span class="sxs-lookup"><span data-stu-id="12fe6-200">These properties allow entities to act as property bags where columns are mapped to named properties in the bag.</span></span>

<span data-ttu-id="12fe6-201">Документация отслеживается по проблеме [#2018](https://github.com/dotnet/EntityFramework.Docs/issues/2018).</span><span class="sxs-lookup"><span data-stu-id="12fe6-201">Documentation is tracked by issue [#2018](https://github.com/dotnet/EntityFramework.Docs/issues/2018).</span></span>

### <a name="generation-of-check-constraints-for-enum-mappings"></a><span data-ttu-id="12fe6-202">Создание ограничений CHECK для сопоставлений перечислений</span><span class="sxs-lookup"><span data-stu-id="12fe6-202">Generation of check constraints for enum mappings</span></span>

<span data-ttu-id="12fe6-203">Функция миграции в EF Core 5.0 теперь может создавать ограничения CHECK для сопоставлений свойств перечисления.</span><span class="sxs-lookup"><span data-stu-id="12fe6-203">EF Core 5.0 Migrations can now generate CHECK constraints for enum property mappings.</span></span>
<span data-ttu-id="12fe6-204">Пример:</span><span class="sxs-lookup"><span data-stu-id="12fe6-204">For example:</span></span>

```SQL
MyEnumColumn VARCHAR(10) NOT NULL CHECK (MyEnumColumn IN ('Useful', 'Useless', 'Unknown'))
```

<span data-ttu-id="12fe6-205">Документация отслеживается по проблеме [#2082](https://github.com/dotnet/EntityFramework.Docs/issues/2082).</span><span class="sxs-lookup"><span data-stu-id="12fe6-205">Documentation is tracked by issue [#2082](https://github.com/dotnet/EntityFramework.Docs/issues/2082).</span></span>

### <a name="isrelational"></a><span data-ttu-id="12fe6-206">Является реляционным</span><span class="sxs-lookup"><span data-stu-id="12fe6-206">IsRelational</span></span>

<span data-ttu-id="12fe6-207">В дополнение к существующим методам `IsSqlServer`, `IsSqlite` и `IsInMemory` был добавлен новый метод `IsRelational`.</span><span class="sxs-lookup"><span data-stu-id="12fe6-207">A new `IsRelational` method has been added in addition to the existing `IsSqlServer`, `IsSqlite`, and `IsInMemory`.</span></span>
<span data-ttu-id="12fe6-208">Этот метод можно использовать для проверки того, использует ли DbContext какой-либо поставщик реляционной базы данных.</span><span class="sxs-lookup"><span data-stu-id="12fe6-208">This method can be used to test if the DbContext is using any relational database provider.</span></span>
<span data-ttu-id="12fe6-209">Пример:</span><span class="sxs-lookup"><span data-stu-id="12fe6-209">For example:</span></span>

```CSharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    if (Database.IsRelational())
    {
        // Do relational-specific model configuration.
    }
}
```

<span data-ttu-id="12fe6-210">Документация отслеживается по проблеме [#2185](https://github.com/dotnet/EntityFramework.Docs/issues/2185).</span><span class="sxs-lookup"><span data-stu-id="12fe6-210">Documentation is tracked by issue [#2185](https://github.com/dotnet/EntityFramework.Docs/issues/2185).</span></span>

### <a name="cosmos-optimistic-concurrency-with-etags"></a><span data-ttu-id="12fe6-211">Оптимистический параллелизм Cosmos с тегами ETag</span><span class="sxs-lookup"><span data-stu-id="12fe6-211">Cosmos optimistic concurrency with ETags</span></span>

<span data-ttu-id="12fe6-212">Поставщик базы данных Azure Cosmos DB теперь поддерживает оптимистичный параллелизм с использованием тегов ETag.</span><span class="sxs-lookup"><span data-stu-id="12fe6-212">The Azure Cosmos DB database provider now supports optimistic concurrency using ETags.</span></span>
<span data-ttu-id="12fe6-213">Используйте построитель моделей в OnModelCreating для настройки ETag:</span><span class="sxs-lookup"><span data-stu-id="12fe6-213">Use the model builder in OnModelCreating to configure an ETag:</span></span>

```CSharp
builder.Entity<Customer>().Property(c => c.ETag).IsEtagConcurrency();
```

<span data-ttu-id="12fe6-214">Затем SaveChanges генерирует исключение `DbUpdateConcurrencyException` при конфликте параллелизма, которое [можно обработать](https://docs.microsoft.com/ef/core/saving/concurrency) для реализации повторных попыток и т. д.</span><span class="sxs-lookup"><span data-stu-id="12fe6-214">SaveChanges will then throw an `DbUpdateConcurrencyException` on a concurrency conflict, which [can be handled](https://docs.microsoft.com/ef/core/saving/concurrency) to implement retries, etc.</span></span>

<span data-ttu-id="12fe6-215">Документация отслеживается по проблеме [#2099](https://github.com/dotnet/EntityFramework.Docs/issues/2099).</span><span class="sxs-lookup"><span data-stu-id="12fe6-215">Documentation is tracked by issue [#2099](https://github.com/dotnet/EntityFramework.Docs/issues/2099).</span></span>

### <a name="query-translations-for-more-datetime-constructs"></a><span data-ttu-id="12fe6-216">Преобразования запросов для дополнительных конструкций DateTime</span><span class="sxs-lookup"><span data-stu-id="12fe6-216">Query translations for more DateTime constructs</span></span>

<span data-ttu-id="12fe6-217">Теперь запросы, содержащие новую конструкцию DateTime, поддерживают преобразование.</span><span class="sxs-lookup"><span data-stu-id="12fe6-217">Queries containing new DateTime construction are now translated.</span></span>

<span data-ttu-id="12fe6-218">Кроме того, теперь сопоставлены следующие функции SQL Server:</span><span class="sxs-lookup"><span data-stu-id="12fe6-218">In addition, the following SQL Server functions are now mapped:</span></span>

* <span data-ttu-id="12fe6-219">DateDiffWeek</span><span class="sxs-lookup"><span data-stu-id="12fe6-219">DateDiffWeek</span></span>
* <span data-ttu-id="12fe6-220">DateFromParts</span><span class="sxs-lookup"><span data-stu-id="12fe6-220">DateFromParts</span></span>

<span data-ttu-id="12fe6-221">Пример:</span><span class="sxs-lookup"><span data-stu-id="12fe6-221">For example:</span></span>

```CSharp
var count = context.Orders.Count(c => date > EF.Functions.DateFromParts(DateTime.Now.Year, 12, 25));

```

<span data-ttu-id="12fe6-222">Документация отслеживается по проблеме [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079).</span><span class="sxs-lookup"><span data-stu-id="12fe6-222">Documentation is tracked by issue [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079).</span></span>

### <a name="query-translations-for-more-byte-array-constructs"></a><span data-ttu-id="12fe6-223">Преобразования запросов для дополнительных конструкций массивов байтов</span><span class="sxs-lookup"><span data-stu-id="12fe6-223">Query translations for more byte array constructs</span></span>

<span data-ttu-id="12fe6-224">Запросы, использующие свойства Contains, Length, SequenceEqual и т. д. в массиве byte[], преобразуются в SQL.</span><span class="sxs-lookup"><span data-stu-id="12fe6-224">Queries using Contains, Length, SequenceEqual, etc. on byte[] properties are now translated to SQL.</span></span>

<span data-ttu-id="12fe6-225">Предварительная документация включена в раздел с описанием [еженедельного состояния EF на 5 декабря 2019 г](https://github.com/dotnet/efcore/issues/15403#issuecomment-562332863).</span><span class="sxs-lookup"><span data-stu-id="12fe6-225">Preliminary documentation is included in the [EF weekly status for December 5, 2019](https://github.com/dotnet/efcore/issues/15403#issuecomment-562332863).</span></span>

<span data-ttu-id="12fe6-226">Дополнительная документация отслеживается по проблеме [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079).</span><span class="sxs-lookup"><span data-stu-id="12fe6-226">Additional documentation is tracked by issue [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079).</span></span>

### <a name="query-translation-for-reverse"></a><span data-ttu-id="12fe6-227">Преобразование запросов, использующих Reverse</span><span class="sxs-lookup"><span data-stu-id="12fe6-227">Query translation for Reverse</span></span>

<span data-ttu-id="12fe6-228">Теперь можно преобразовывать запросы, использующие `Reverse`.</span><span class="sxs-lookup"><span data-stu-id="12fe6-228">Queries using `Reverse` are now translated.</span></span>
<span data-ttu-id="12fe6-229">Пример:</span><span class="sxs-lookup"><span data-stu-id="12fe6-229">For example:</span></span>

```CSharp
context.Employees.OrderBy(e => e.EmployeeID).Reverse()
```

<span data-ttu-id="12fe6-230">Документация отслеживается по проблеме [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079).</span><span class="sxs-lookup"><span data-stu-id="12fe6-230">Documentation is tracked by issue [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079).</span></span>

### <a name="query-translation-for-bitwise-operators"></a><span data-ttu-id="12fe6-231">Преобразование запросов для побитовых операторов</span><span class="sxs-lookup"><span data-stu-id="12fe6-231">Query translation for bitwise operators</span></span>

<span data-ttu-id="12fe6-232">Запросы, использующие побитовые операторы, теперь преобразуются в ряде дополнительных случаев, например:</span><span class="sxs-lookup"><span data-stu-id="12fe6-232">Queries using bitwise operators are now translated in more cases For example:</span></span>

```CSharp
context.Orders.Where(o => ~o.OrderID == negatedId)
```

<span data-ttu-id="12fe6-233">Документация отслеживается по проблеме [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079).</span><span class="sxs-lookup"><span data-stu-id="12fe6-233">Documentation is tracked by issue [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079).</span></span>

### <a name="query-translation-for-strings-on-cosmos"></a><span data-ttu-id="12fe6-234">Преобразование запросов для строк в Cosmos</span><span class="sxs-lookup"><span data-stu-id="12fe6-234">Query translation for strings on Cosmos</span></span>

<span data-ttu-id="12fe6-235">Запросы, использующие строковые методы Contains, StartsWith и EndsWith, теперь преобразуются при использовании поставщика Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="12fe6-235">Queries that use the string methods Contains, StartsWith, and EndsWith are now translated when using the Azure Cosmos DB provider.</span></span>

<span data-ttu-id="12fe6-236">Документация отслеживается по проблеме [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079).</span><span class="sxs-lookup"><span data-stu-id="12fe6-236">Documentation is tracked by issue [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079).</span></span>
