---
title: Новые возможности EF Core 5.0
description: Обзор новых возможностей в EF Core 5.0
author: ajcvickers
ms.date: 03/30/2020
uid: core/what-is-new/ef-core-5.0/whatsnew.md
ms.openlocfilehash: c047a308cadf44eea577dcab29b68b36942a50df
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/07/2020
ms.locfileid: "80634280"
---
# <a name="whats-new-in-ef-core-50"></a><span data-ttu-id="a55fb-103">Новые возможности EF Core 5.0</span><span class="sxs-lookup"><span data-stu-id="a55fb-103">What's New in EF Core 5.0</span></span>

<span data-ttu-id="a55fb-104">Сейчас EF Core 5.0 находится на стадии разработки.</span><span class="sxs-lookup"><span data-stu-id="a55fb-104">EF Core 5.0 is currently in development.</span></span>
<span data-ttu-id="a55fb-105">На этой странице представлен обзор интересных изменений, появившихся в каждой предварительной версии.</span><span class="sxs-lookup"><span data-stu-id="a55fb-105">This page will contain an overview of interesting changes introduced in each preview.</span></span>

<span data-ttu-id="a55fb-106">Эта страница не является аналогом статьи о [планировании для EF Core 5.0](plan.md).</span><span class="sxs-lookup"><span data-stu-id="a55fb-106">This page does not duplicate the [plan for EF Core 5.0](plan.md).</span></span>
<span data-ttu-id="a55fb-107">В плане описываются общие темы для EF Core 5.0, в том числе все, что планируется включить перед выпуском окончательной версии.</span><span class="sxs-lookup"><span data-stu-id="a55fb-107">The plan describes the overall themes for EF Core 5.0, including everything we are planning to include before shipping the final release.</span></span>

<span data-ttu-id="a55fb-108">Мы будем добавлять ссылки на официальную документацию по мере ее публикации.</span><span class="sxs-lookup"><span data-stu-id="a55fb-108">We will add links from here to the official documentation as it is published.</span></span>

## <a name="preview-2"></a><span data-ttu-id="a55fb-109">Предварительная версия 2</span><span class="sxs-lookup"><span data-stu-id="a55fb-109">Preview 2</span></span>

### <a name="use-a-c-attribute-to-specify-a-property-backing-field"></a><span data-ttu-id="a55fb-110">Использование атрибута C# для указания резервного поля свойств</span><span class="sxs-lookup"><span data-stu-id="a55fb-110">Use a C# attribute to specify a property backing field</span></span>

<span data-ttu-id="a55fb-111">Теперь атрибут C# можно использовать для указания резервного поля для свойства.</span><span class="sxs-lookup"><span data-stu-id="a55fb-111">A C# attribute can now be used to specify the backing field for a property.</span></span>
<span data-ttu-id="a55fb-112">Этот атрибут позволяет EF Core в обычном порядке выполнять запись и чтение из резервного поля, даже если резервное поле не может быть найдено автоматически.</span><span class="sxs-lookup"><span data-stu-id="a55fb-112">This attribute allows EF Core to still write to and read from the backing field as would normally happen, even when the backing field cannot be found automatically.</span></span>
<span data-ttu-id="a55fb-113">Пример:</span><span class="sxs-lookup"><span data-stu-id="a55fb-113">For example:</span></span>

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

<span data-ttu-id="a55fb-114">Документация отслеживается по проблеме [#2230](https://github.com/dotnet/EntityFramework.Docs/issues/2230).</span><span class="sxs-lookup"><span data-stu-id="a55fb-114">Documentation is tracked by issue [#2230](https://github.com/dotnet/EntityFramework.Docs/issues/2230).</span></span>

### <a name="complete-discriminator-mapping"></a><span data-ttu-id="a55fb-115">Завершение сопоставления дискриминатора</span><span class="sxs-lookup"><span data-stu-id="a55fb-115">Complete discriminator mapping</span></span>

<span data-ttu-id="a55fb-116">EF Core использует столбец дискриминатора для [сопоставления TPH в иерархии наследования](/ef/core/modeling/inheritance).</span><span class="sxs-lookup"><span data-stu-id="a55fb-116">EF Core uses a discriminator column for [TPH mapping of an inheritance hierarchy](/ef/core/modeling/inheritance).</span></span>
<span data-ttu-id="a55fb-117">Некоторые улучшения производительности будут поддерживаться до тех пор, пока EF Core будут известны все возможные значения для дискриминатора.</span><span class="sxs-lookup"><span data-stu-id="a55fb-117">Some performance enhancements are possible so long as EF Core knows all possible values for the discriminator.</span></span>
<span data-ttu-id="a55fb-118">Теперь эти улучшения реализованы в EF Core 5.0.</span><span class="sxs-lookup"><span data-stu-id="a55fb-118">EF Core 5.0 now implements these enhancements.</span></span>

<span data-ttu-id="a55fb-119">Например, в предыдущих версиях EF Core всегда создавался следующий код SQL для запроса, возвращающего все типы в иерархии:</span><span class="sxs-lookup"><span data-stu-id="a55fb-119">For example, previous versions of EF Core would always generate this SQL for a query returning all types in a hierarchy:</span></span>

```sql
SELECT [a].[Id], [a].[Discriminator], [a].[Name]
FROM [Animal] AS [a]
WHERE [a].[Discriminator] IN (N'Animal', N'Cat', N'Dog', N'Human')
```

<span data-ttu-id="a55fb-120">Теперь при настройке полного сопоставления дискриминатора EF Core 5.0 будет формировать следующее:</span><span class="sxs-lookup"><span data-stu-id="a55fb-120">EF Core 5.0 will now generate the following when a complete discriminator mapping is configured:</span></span>

```sql
SELECT [a].[Id], [a].[Discriminator], [a].[Name]
FROM [Animal] AS [a]
```

<span data-ttu-id="a55fb-121">Начиная с предварительной версии 3 это поведение будет доступно по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="a55fb-121">It will be the default behavior starting with preview 3.</span></span>

### <a name="performance-improvements-in-microsoftdatasqlite"></a><span data-ttu-id="a55fb-122">Улучшения производительности в Microsoft.Data.Sqlite</span><span class="sxs-lookup"><span data-stu-id="a55fb-122">Performance improvements in Microsoft.Data.Sqlite</span></span>

<span data-ttu-id="a55fb-123">В производительность SQLIte были внесены два улучшения:</span><span class="sxs-lookup"><span data-stu-id="a55fb-123">We have made two performance improvements for SQLIte:</span></span>

* <span data-ttu-id="a55fb-124">Теперь получение двоичных и строковых данных с помощью GetBytes, GetChars и GetTextReader осуществляется более эффективно за счет использования SqliteBlob и потоков.</span><span class="sxs-lookup"><span data-stu-id="a55fb-124">Retrieving binary and string data with GetBytes, GetChars, and GetTextReader is now more efficient by making use of SqliteBlob and streams.</span></span>
* <span data-ttu-id="a55fb-125">Инициализация SqliteConnection выполняется в отложенном режиме.</span><span class="sxs-lookup"><span data-stu-id="a55fb-125">Initialization of SqliteConnection is now lazy.</span></span>

<span data-ttu-id="a55fb-126">Эти улучшения добавлены в поставщик Microsoft.Data.Sqlite ADO.NET и, следовательно, позволяют повысить производительность за пределами EF Core.</span><span class="sxs-lookup"><span data-stu-id="a55fb-126">These improvements are in the ADO.NET Microsoft.Data.Sqlite provider and hence also improve performance outside of EF Core.</span></span>

## <a name="preview-1"></a><span data-ttu-id="a55fb-127">Предварительная версия 1</span><span class="sxs-lookup"><span data-stu-id="a55fb-127">Preview 1</span></span>

### <a name="simple-logging"></a><span data-ttu-id="a55fb-128">Простое ведение журнала</span><span class="sxs-lookup"><span data-stu-id="a55fb-128">Simple logging</span></span>

<span data-ttu-id="a55fb-129">Эта функция добавляет возможность, аналогичную `Database.Log` в EF6.</span><span class="sxs-lookup"><span data-stu-id="a55fb-129">This feature adds functionality similar to `Database.Log` in EF6.</span></span>
<span data-ttu-id="a55fb-130">Это значит, что она позволяет легко получать журналы из EF Core без настройки какой-либо внешней платформы ведения журналов.</span><span class="sxs-lookup"><span data-stu-id="a55fb-130">That is, it provides a simple way to get logs from EF Core without the need to configure any kind of external logging framework.</span></span>

<span data-ttu-id="a55fb-131">Предварительная документация включена в раздел с описанием [еженедельного состояния EF на 5 декабря 2019 г](https://github.com/dotnet/efcore/issues/15403#issuecomment-562332863).</span><span class="sxs-lookup"><span data-stu-id="a55fb-131">Preliminary documentation is included in the [EF weekly status for December 5, 2019](https://github.com/dotnet/efcore/issues/15403#issuecomment-562332863).</span></span>

<span data-ttu-id="a55fb-132">Дополнительная документация отслеживается по проблеме [#2085](https://github.com/dotnet/EntityFramework.Docs/issues/2085).</span><span class="sxs-lookup"><span data-stu-id="a55fb-132">Additional documentation is tracked by issue [#2085](https://github.com/dotnet/EntityFramework.Docs/issues/2085).</span></span>

### <a name="simple-way-to-get-generated-sql"></a><span data-ttu-id="a55fb-133">Простой способ получения созданного кода SQL</span><span class="sxs-lookup"><span data-stu-id="a55fb-133">Simple way to get generated SQL</span></span>

<span data-ttu-id="a55fb-134">В EF Core 5.0 представлен метод расширения `ToQueryString`, который будет возвращать код SQL, создаваемый в EF Core при выполнении запроса LINQ.</span><span class="sxs-lookup"><span data-stu-id="a55fb-134">EF Core 5.0 introduces the `ToQueryString` extension method, which will return the SQL that EF Core will generate when executing a LINQ query.</span></span>

<span data-ttu-id="a55fb-135">Предварительная документация включена в раздел с описанием [еженедельного состояния EF на 9 января 2020 г](https://github.com/dotnet/efcore/issues/19549#issuecomment-572823246).</span><span class="sxs-lookup"><span data-stu-id="a55fb-135">Preliminary documentation is included in the [EF weekly status for January 9, 2020](https://github.com/dotnet/efcore/issues/19549#issuecomment-572823246).</span></span>

<span data-ttu-id="a55fb-136">Дополнительная документация отслеживается по проблеме [#1331](https://github.com/dotnet/EntityFramework.Docs/issues/1331).</span><span class="sxs-lookup"><span data-stu-id="a55fb-136">Additional documentation is tracked by issue [#1331](https://github.com/dotnet/EntityFramework.Docs/issues/1331).</span></span>

### <a name="use-a-c-attribute-to-indicate-that-an-entity-has-no-key"></a><span data-ttu-id="a55fb-137">Использование атрибута C# для указания того, что сущность не имеет ключа</span><span class="sxs-lookup"><span data-stu-id="a55fb-137">Use a C# attribute to indicate that an entity has no key</span></span>

<span data-ttu-id="a55fb-138">Теперь можно настроить тип сущности, не имеющей ключа, используя новый атрибут `KeylessAttribute`.</span><span class="sxs-lookup"><span data-stu-id="a55fb-138">An entity type can now be configured as having no key using the new `KeylessAttribute`.</span></span>
<span data-ttu-id="a55fb-139">Пример:</span><span class="sxs-lookup"><span data-stu-id="a55fb-139">For example:</span></span>

```CSharp
[Keyless]
public class Address
{
    public string Street { get; set; }
    public string City { get; set; }
    public int Zip { get; set; }
}
```

<span data-ttu-id="a55fb-140">Документация отслеживается по проблеме [#2186](https://github.com/dotnet/EntityFramework.Docs/issues/2186).</span><span class="sxs-lookup"><span data-stu-id="a55fb-140">Documentation is tracked by issue [#2186](https://github.com/dotnet/EntityFramework.Docs/issues/2186).</span></span>

### <a name="connection-or-connection-string-can-be-changed-on-initialized-dbcontext"></a><span data-ttu-id="a55fb-141">Возможность изменения подключения или строки подключения в инициализированном DbContext</span><span class="sxs-lookup"><span data-stu-id="a55fb-141">Connection or connection string can be changed on initialized DbContext</span></span>

<span data-ttu-id="a55fb-142">Теперь создание экземпляра DbContext без подключения или строки подключения стало еще проще.</span><span class="sxs-lookup"><span data-stu-id="a55fb-142">It is now easier to create a DbContext instance without any connection or connection string.</span></span>
<span data-ttu-id="a55fb-143">Кроме того, подключение или строку подключения можно изменить в экземпляре контекста.</span><span class="sxs-lookup"><span data-stu-id="a55fb-143">Also, the connection or connection string can now be mutated on the context instance.</span></span>
<span data-ttu-id="a55fb-144">Эта возможность позволяет одному и тому же экземпляру контекста динамически подключаться к разным базам данных.</span><span class="sxs-lookup"><span data-stu-id="a55fb-144">This feature allows the same context instance to dynamically connect to different databases.</span></span>

<span data-ttu-id="a55fb-145">Документация отслеживается по проблеме [#2075](https://github.com/dotnet/EntityFramework.Docs/issues/2075).</span><span class="sxs-lookup"><span data-stu-id="a55fb-145">Documentation is tracked by issue [#2075](https://github.com/dotnet/EntityFramework.Docs/issues/2075).</span></span>

### <a name="change-tracking-proxies"></a><span data-ttu-id="a55fb-146">Прокси-серверы отслеживания изменений</span><span class="sxs-lookup"><span data-stu-id="a55fb-146">Change-tracking proxies</span></span>

<span data-ttu-id="a55fb-147">Теперь EF Core может создавать прокси-серверы среды выполнения, которые автоматически реализуют интерфейсы [INotifyPropertyChanging](https://docs.microsoft.com/dotnet/api/system.componentmodel.inotifypropertychanging?view=netcore-3.1) и [INotifyPropertyChanged](https://docs.microsoft.com/dotnet/api/system.componentmodel.inotifypropertychanged?view=netcore-3.1).</span><span class="sxs-lookup"><span data-stu-id="a55fb-147">EF Core can now generate runtime proxies that automatically implement [INotifyPropertyChanging](https://docs.microsoft.com/dotnet/api/system.componentmodel.inotifypropertychanging?view=netcore-3.1) and [INotifyPropertyChanged](https://docs.microsoft.com/dotnet/api/system.componentmodel.inotifypropertychanged?view=netcore-3.1).</span></span>
<span data-ttu-id="a55fb-148">Затем они сообщают об изменениях значений свойств сущности напрямую в EF Core, что позволяет не выполнять проверку на наличие изменений.</span><span class="sxs-lookup"><span data-stu-id="a55fb-148">These then report value changes on entity properties directly to EF Core, avoiding the need to scan for changes.</span></span>
<span data-ttu-id="a55fb-149">Однако прокси-серверы имеют собственный набор ограничений, поэтому они предназначены не для всех.</span><span class="sxs-lookup"><span data-stu-id="a55fb-149">However, proxies come with their own set of limitations, so they are not for everyone.</span></span>

<span data-ttu-id="a55fb-150">Документация отслеживается по проблеме [#2076](https://github.com/dotnet/EntityFramework.Docs/issues/2076).</span><span class="sxs-lookup"><span data-stu-id="a55fb-150">Documentation is tracked by issue [#2076](https://github.com/dotnet/EntityFramework.Docs/issues/2076).</span></span>

### <a name="enhanced-debug-views"></a><span data-ttu-id="a55fb-151">Улучшенные представления отладки</span><span class="sxs-lookup"><span data-stu-id="a55fb-151">Enhanced debug views</span></span>

<span data-ttu-id="a55fb-152">В представлениях отладки удобно просматривать внутренние компоненты EF Core при отладке.</span><span class="sxs-lookup"><span data-stu-id="a55fb-152">Debug views are an easy way to look at the internals of EF Core when debugging issues.</span></span>
<span data-ttu-id="a55fb-153">Представление отладки для модели было реализовано некоторое время назад.</span><span class="sxs-lookup"><span data-stu-id="a55fb-153">A debug view for the Model was implemented some time ago.</span></span>
<span data-ttu-id="a55fb-154">В EF Core 5.0 представление модели стало более удобочитаемым. Кроме того, добавлено новое представление отладки для отслеживаемых сущностей в диспетчере состояний.</span><span class="sxs-lookup"><span data-stu-id="a55fb-154">For EF Core 5.0, we have made the model view easier to read and added a new debug view for tracked entities in the state manager.</span></span>

<span data-ttu-id="a55fb-155">Предварительная документация включена в раздел с описанием [еженедельного состояния EF на 12 декабря 2019 г](https://github.com/dotnet/efcore/issues/15403#issuecomment-565196206).</span><span class="sxs-lookup"><span data-stu-id="a55fb-155">Preliminary documentation is included in the [EF weekly status for December 12, 2019](https://github.com/dotnet/efcore/issues/15403#issuecomment-565196206).</span></span>

<span data-ttu-id="a55fb-156">Дополнительная документация отслеживается по проблеме [#2086](https://github.com/dotnet/EntityFramework.Docs/issues/2086).</span><span class="sxs-lookup"><span data-stu-id="a55fb-156">Additional documentation is tracked by issue [#2086](https://github.com/dotnet/EntityFramework.Docs/issues/2086).</span></span>

### <a name="improved-handling-of-database-null-semantics"></a><span data-ttu-id="a55fb-157">Улучшенная обработка семантики значений NULL в базе данных</span><span class="sxs-lookup"><span data-stu-id="a55fb-157">Improved handling of database null semantics</span></span>

<span data-ttu-id="a55fb-158">Реляционные базы данных обычно обрабатывают NULL как неизвестное значение и, следовательно, как не равное любому другому значению NULL.</span><span class="sxs-lookup"><span data-stu-id="a55fb-158">Relational databases typically treat NULL as an unknown value and therefore not equal to any other NULL.</span></span>
<span data-ttu-id="a55fb-159">C# рассматривает NULL как определенное значение, которое аналогично любому другому значению NULL.</span><span class="sxs-lookup"><span data-stu-id="a55fb-159">While C# treats null as a defined value, which compares equal to any other null.</span></span>
<span data-ttu-id="a55fb-160">EF Core по умолчанию преобразует запросы так, чтобы они использовали семантику значений NULL C#.</span><span class="sxs-lookup"><span data-stu-id="a55fb-160">EF Core by default translates queries so that they use C# null semantics.</span></span>
<span data-ttu-id="a55fb-161">В EF Core 5.0 эффективность таких преобразований значительно повышена.</span><span class="sxs-lookup"><span data-stu-id="a55fb-161">EF Core 5.0 greatly improves the efficiency of these translations.</span></span>

<span data-ttu-id="a55fb-162">Документация отслеживается по проблеме [#1612](https://github.com/dotnet/EntityFramework.Docs/issues/1612).</span><span class="sxs-lookup"><span data-stu-id="a55fb-162">Documentation is tracked by issue [#1612](https://github.com/dotnet/EntityFramework.Docs/issues/1612).</span></span>

### <a name="indexer-properties"></a><span data-ttu-id="a55fb-163">Свойства индексатора</span><span class="sxs-lookup"><span data-stu-id="a55fb-163">Indexer properties</span></span>

<span data-ttu-id="a55fb-164">EF Core 5.0 поддерживает сопоставление свойств индексатора C#.</span><span class="sxs-lookup"><span data-stu-id="a55fb-164">EF Core 5.0 supports mapping of C# indexer properties.</span></span>
<span data-ttu-id="a55fb-165">Эти свойства позволяют сущностям выступать в качестве контейнеров свойств, в которых столбцы сопоставляются с именованными свойствами в контейнере.</span><span class="sxs-lookup"><span data-stu-id="a55fb-165">These properties allow entities to act as property bags where columns are mapped to named properties in the bag.</span></span>

<span data-ttu-id="a55fb-166">Документация отслеживается по проблеме [#2018](https://github.com/dotnet/EntityFramework.Docs/issues/2018).</span><span class="sxs-lookup"><span data-stu-id="a55fb-166">Documentation is tracked by issue [#2018](https://github.com/dotnet/EntityFramework.Docs/issues/2018).</span></span>

### <a name="generation-of-check-constraints-for-enum-mappings"></a><span data-ttu-id="a55fb-167">Создание ограничений CHECK для сопоставлений перечислений</span><span class="sxs-lookup"><span data-stu-id="a55fb-167">Generation of check constraints for enum mappings</span></span>

<span data-ttu-id="a55fb-168">Функция миграции в EF Core 5.0 теперь может создавать ограничения CHECK для сопоставлений свойств перечисления.</span><span class="sxs-lookup"><span data-stu-id="a55fb-168">EF Core 5.0 Migrations can now generate CHECK constraints for enum property mappings.</span></span>
<span data-ttu-id="a55fb-169">Пример:</span><span class="sxs-lookup"><span data-stu-id="a55fb-169">For example:</span></span>

```SQL
MyEnumColumn VARCHAR(10) NOT NULL CHECK (MyEnumColumn IN ('Useful', 'Useless', 'Unknown'))
```

<span data-ttu-id="a55fb-170">Документация отслеживается по проблеме [#2082](https://github.com/dotnet/EntityFramework.Docs/issues/2082).</span><span class="sxs-lookup"><span data-stu-id="a55fb-170">Documentation is tracked by issue [#2082](https://github.com/dotnet/EntityFramework.Docs/issues/2082).</span></span>

### <a name="isrelational"></a><span data-ttu-id="a55fb-171">Является реляционным</span><span class="sxs-lookup"><span data-stu-id="a55fb-171">IsRelational</span></span>

<span data-ttu-id="a55fb-172">В дополнение к существующим методам `IsSqlServer`, `IsSqlite` и `IsInMemory` был добавлен новый метод `IsRelational`.</span><span class="sxs-lookup"><span data-stu-id="a55fb-172">A new `IsRelational` method has been added in addition to the existing `IsSqlServer`, `IsSqlite`, and `IsInMemory`.</span></span>
<span data-ttu-id="a55fb-173">Этот метод можно использовать для проверки того, использует ли DbContext какой-либо поставщик реляционной базы данных.</span><span class="sxs-lookup"><span data-stu-id="a55fb-173">This method can be used to test if the DbContext is using any relational database provider.</span></span>
<span data-ttu-id="a55fb-174">Пример:</span><span class="sxs-lookup"><span data-stu-id="a55fb-174">For example:</span></span>

```CSharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    if (Database.IsRelational())
    {
        // Do relational-specific model configuration.
    }
}
```

<span data-ttu-id="a55fb-175">Документация отслеживается по проблеме [#2185](https://github.com/dotnet/EntityFramework.Docs/issues/2185).</span><span class="sxs-lookup"><span data-stu-id="a55fb-175">Documentation is tracked by issue [#2185](https://github.com/dotnet/EntityFramework.Docs/issues/2185).</span></span>

### <a name="cosmos-optimistic-concurrency-with-etags"></a><span data-ttu-id="a55fb-176">Оптимистический параллелизм Cosmos с тегами ETag</span><span class="sxs-lookup"><span data-stu-id="a55fb-176">Cosmos optimistic concurrency with ETags</span></span>

<span data-ttu-id="a55fb-177">Поставщик базы данных Azure Cosmos DB теперь поддерживает оптимистичный параллелизм с использованием тегов ETag.</span><span class="sxs-lookup"><span data-stu-id="a55fb-177">The Azure Cosmos DB database provider now supports optimistic concurrency using ETags.</span></span>
<span data-ttu-id="a55fb-178">Используйте построитель моделей в OnModelCreating для настройки ETag:</span><span class="sxs-lookup"><span data-stu-id="a55fb-178">Use the model builder in OnModelCreating to configure an ETag:</span></span>

```CSharp
builder.Entity<Customer>().Property(c => c.ETag).IsEtagConcurrency();
```

<span data-ttu-id="a55fb-179">Затем SaveChanges генерирует исключение `DbUpdateConcurrencyException` при конфликте параллелизма, которое [можно обработать](https://docs.microsoft.com/ef/core/saving/concurrency) для реализации повторных попыток и т. д.</span><span class="sxs-lookup"><span data-stu-id="a55fb-179">SaveChanges will then throw an `DbUpdateConcurrencyException` on a concurrency conflict, which [can be handled](https://docs.microsoft.com/ef/core/saving/concurrency) to implement retries, etc.</span></span>

<span data-ttu-id="a55fb-180">Документация отслеживается по проблеме [#2099](https://github.com/dotnet/EntityFramework.Docs/issues/2099).</span><span class="sxs-lookup"><span data-stu-id="a55fb-180">Documentation is tracked by issue [#2099](https://github.com/dotnet/EntityFramework.Docs/issues/2099).</span></span>

### <a name="query-translations-for-more-datetime-constructs"></a><span data-ttu-id="a55fb-181">Преобразования запросов для дополнительных конструкций DateTime</span><span class="sxs-lookup"><span data-stu-id="a55fb-181">Query translations for more DateTime constructs</span></span>

<span data-ttu-id="a55fb-182">Теперь запросы, содержащие новую конструкцию DateTime, поддерживают преобразование.</span><span class="sxs-lookup"><span data-stu-id="a55fb-182">Queries containing new DateTime construction are now translated.</span></span>

<span data-ttu-id="a55fb-183">Кроме того, теперь сопоставлены следующие функции SQL Server:</span><span class="sxs-lookup"><span data-stu-id="a55fb-183">In addition, the following SQL Server functions are now mapped:</span></span>

* <span data-ttu-id="a55fb-184">DateDiffWeek</span><span class="sxs-lookup"><span data-stu-id="a55fb-184">DateDiffWeek</span></span>
* <span data-ttu-id="a55fb-185">DateFromParts</span><span class="sxs-lookup"><span data-stu-id="a55fb-185">DateFromParts</span></span>

<span data-ttu-id="a55fb-186">Пример:</span><span class="sxs-lookup"><span data-stu-id="a55fb-186">For example:</span></span>

```CSharp
var count = context.Orders.Count(c => date > EF.Functions.DateFromParts(DateTime.Now.Year, 12, 25));

```

<span data-ttu-id="a55fb-187">Документация отслеживается по проблеме [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079).</span><span class="sxs-lookup"><span data-stu-id="a55fb-187">Documentation is tracked by issue [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079).</span></span>

### <a name="query-translations-for-more-byte-array-constructs"></a><span data-ttu-id="a55fb-188">Преобразования запросов для дополнительных конструкций массивов байтов</span><span class="sxs-lookup"><span data-stu-id="a55fb-188">Query translations for more byte array constructs</span></span>

<span data-ttu-id="a55fb-189">Запросы, использующие свойства Contains, Length, SequenceEqual и т. д. в массиве byte[], преобразуются в SQL.</span><span class="sxs-lookup"><span data-stu-id="a55fb-189">Queries using Contains, Length, SequenceEqual, etc. on byte[] properties are now translated to SQL.</span></span>

<span data-ttu-id="a55fb-190">Предварительная документация включена в раздел с описанием [еженедельного состояния EF на 5 декабря 2019 г](https://github.com/dotnet/efcore/issues/15403#issuecomment-562332863).</span><span class="sxs-lookup"><span data-stu-id="a55fb-190">Preliminary documentation is included in the [EF weekly status for December 5, 2019](https://github.com/dotnet/efcore/issues/15403#issuecomment-562332863).</span></span>

<span data-ttu-id="a55fb-191">Дополнительная документация отслеживается по проблеме [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079).</span><span class="sxs-lookup"><span data-stu-id="a55fb-191">Additional documentation is tracked by issue [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079).</span></span>

### <a name="query-translation-for-reverse"></a><span data-ttu-id="a55fb-192">Преобразование запросов, использующих Reverse</span><span class="sxs-lookup"><span data-stu-id="a55fb-192">Query translation for Reverse</span></span>

<span data-ttu-id="a55fb-193">Теперь можно преобразовывать запросы, использующие `Reverse`.</span><span class="sxs-lookup"><span data-stu-id="a55fb-193">Queries using `Reverse` are now translated.</span></span>
<span data-ttu-id="a55fb-194">Пример:</span><span class="sxs-lookup"><span data-stu-id="a55fb-194">For example:</span></span>

```CSharp
context.Employees.OrderBy(e => e.EmployeeID).Reverse()
```

<span data-ttu-id="a55fb-195">Документация отслеживается по проблеме [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079).</span><span class="sxs-lookup"><span data-stu-id="a55fb-195">Documentation is tracked by issue [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079).</span></span>

### <a name="query-translation-for-bitwise-operators"></a><span data-ttu-id="a55fb-196">Преобразование запросов для побитовых операторов</span><span class="sxs-lookup"><span data-stu-id="a55fb-196">Query translation for bitwise operators</span></span>

<span data-ttu-id="a55fb-197">Запросы, использующие побитовые операторы, теперь преобразуются в ряде дополнительных случаев, например:</span><span class="sxs-lookup"><span data-stu-id="a55fb-197">Queries using bitwise operators are now translated in more cases For example:</span></span>

```CSharp
context.Orders.Where(o => ~o.OrderID == negatedId)
```

<span data-ttu-id="a55fb-198">Документация отслеживается по проблеме [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079).</span><span class="sxs-lookup"><span data-stu-id="a55fb-198">Documentation is tracked by issue [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079).</span></span>

### <a name="query-translation-for-strings-on-cosmos"></a><span data-ttu-id="a55fb-199">Преобразование запросов для строк в Cosmos</span><span class="sxs-lookup"><span data-stu-id="a55fb-199">Query translation for strings on Cosmos</span></span>

<span data-ttu-id="a55fb-200">Запросы, использующие строковые методы Contains, StartsWith и EndsWith, теперь преобразуются при использовании поставщика Azure Cosmos DB.</span><span class="sxs-lookup"><span data-stu-id="a55fb-200">Queries that use the string methods Contains, StartsWith, and EndsWith are now translated when using the Azure Cosmos DB provider.</span></span>

<span data-ttu-id="a55fb-201">Документация отслеживается по проблеме [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079).</span><span class="sxs-lookup"><span data-stu-id="a55fb-201">Documentation is tracked by issue [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079).</span></span>
