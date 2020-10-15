---
title: Поставщик базы данных SQLite — ограничения — EF Core
description: Ограничения поставщика базы данных Entity Framework Core SQLite по сравнению с другими поставщиками
author: bricelam
ms.date: 09/24/2020
uid: core/providers/sqlite/limitations
ms.openlocfilehash: 3d696474d401e8fd6c26a78067d292f0bb97a457
ms.sourcegitcommit: 0a25c03fa65ae6e0e0e3f66bac48d59eceb96a5a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/14/2020
ms.locfileid: "92062741"
---
# <a name="sqlite-ef-core-database-provider-limitations"></a><span data-ttu-id="313c7-103">Ограничения поставщика базы данных SQLite EF Core</span><span class="sxs-lookup"><span data-stu-id="313c7-103">SQLite EF Core Database Provider Limitations</span></span>

<span data-ttu-id="313c7-104">Поставщик SQLite имеет ряд ограничений на миграции.</span><span class="sxs-lookup"><span data-stu-id="313c7-104">The SQLite provider has a number of migrations limitations.</span></span> <span data-ttu-id="313c7-105">Большинство этих ограничений являются следствием ограничений в базовом ядре базы данных SQLite и не относятся к EF.</span><span class="sxs-lookup"><span data-stu-id="313c7-105">Most of these limitations are a result of limitations in the underlying SQLite database engine and are not specific to EF.</span></span>

## <a name="modeling-limitations"></a><span data-ttu-id="313c7-106">Ограничения моделирования</span><span class="sxs-lookup"><span data-stu-id="313c7-106">Modeling limitations</span></span>

<span data-ttu-id="313c7-107">Общая реляционная библиотека (совместно используемая Entity Framework поставщиками реляционных баз данных) определяет интерфейсы API для концепций моделирования, общих для большинства ядер реляционных баз данных.</span><span class="sxs-lookup"><span data-stu-id="313c7-107">The common relational library (shared by Entity Framework relational database providers) defines APIs for modelling concepts that are common to most relational database engines.</span></span> <span data-ttu-id="313c7-108">Некоторые из этих концепций не поддерживаются поставщиком SQLite.</span><span class="sxs-lookup"><span data-stu-id="313c7-108">A couple of these concepts are not supported by the SQLite provider.</span></span>

* <span data-ttu-id="313c7-109">Схемы</span><span class="sxs-lookup"><span data-stu-id="313c7-109">Schemas</span></span>
* <span data-ttu-id="313c7-110">Последовательности</span><span class="sxs-lookup"><span data-stu-id="313c7-110">Sequences</span></span>

## <a name="query-limitations"></a><span data-ttu-id="313c7-111">Ограничения запросов</span><span class="sxs-lookup"><span data-stu-id="313c7-111">Query limitations</span></span>

<span data-ttu-id="313c7-112">SQLite изначально не поддерживает следующие типы данных.</span><span class="sxs-lookup"><span data-stu-id="313c7-112">SQLite doesn't natively support the following data types.</span></span> <span data-ttu-id="313c7-113">EF Core может считывать и записывать значения этих типов, также поддерживается запрос на равенство ( `where e.Property == value` ).</span><span class="sxs-lookup"><span data-stu-id="313c7-113">EF Core can read and write values of these types, and querying for equality (`where e.Property == value`) is also supported.</span></span> <span data-ttu-id="313c7-114">Однако для других операций, таких как сравнение и упорядочение, потребуется Оценка на клиенте.</span><span class="sxs-lookup"><span data-stu-id="313c7-114">Other operations, however, like comparison and ordering will require evaluation on the client.</span></span>

* <span data-ttu-id="313c7-115">DateTimeOffset</span><span class="sxs-lookup"><span data-stu-id="313c7-115">DateTimeOffset</span></span>
* <span data-ttu-id="313c7-116">Decimal</span><span class="sxs-lookup"><span data-stu-id="313c7-116">Decimal</span></span>
* <span data-ttu-id="313c7-117">TimeSpan</span><span class="sxs-lookup"><span data-stu-id="313c7-117">TimeSpan</span></span>
* <span data-ttu-id="313c7-118">UInt64</span><span class="sxs-lookup"><span data-stu-id="313c7-118">UInt64</span></span>

<span data-ttu-id="313c7-119">Вместо рекомендуется `DateTimeOffset` использовать значения типа DateTime.</span><span class="sxs-lookup"><span data-stu-id="313c7-119">Instead of `DateTimeOffset`, we recommend using DateTime values.</span></span> <span data-ttu-id="313c7-120">При обработке нескольких часовых поясов рекомендуется преобразовать значения в формат UTC перед сохранением, а затем выполнить преобразование в соответствующий часовой пояс.</span><span class="sxs-lookup"><span data-stu-id="313c7-120">When handling multiple time zones, we recommend converting the values to UTC before saving and then converting back to the appropriate time zone.</span></span>

<span data-ttu-id="313c7-121">`Decimal`Тип обеспечивает высокий уровень точности.</span><span class="sxs-lookup"><span data-stu-id="313c7-121">The `Decimal` type provides a high level of precision.</span></span> <span data-ttu-id="313c7-122">Однако если такой уровень точности не требуется, вместо него рекомендуется использовать Double.</span><span class="sxs-lookup"><span data-stu-id="313c7-122">If you don't need that level of precision, however, we recommend using double instead.</span></span> <span data-ttu-id="313c7-123">Можно использовать [преобразователь значений](xref:core/modeling/value-conversions) , чтобы продолжить использование десятичного разделителя в классах.</span><span class="sxs-lookup"><span data-stu-id="313c7-123">You can use a [value converter](xref:core/modeling/value-conversions) to continue using decimal in your classes.</span></span>

```csharp
modelBuilder.Entity<MyEntity>()
    .Property(e => e.DecimalProperty)
    .HasConversion<double>();
```

## <a name="migrations-limitations"></a><span data-ttu-id="313c7-124">Ограничения миграции</span><span class="sxs-lookup"><span data-stu-id="313c7-124">Migrations limitations</span></span>

<span data-ttu-id="313c7-125">Ядро базы данных SQLite не поддерживает ряд операций с схемами, которые поддерживаются большинством других реляционных баз данных.</span><span class="sxs-lookup"><span data-stu-id="313c7-125">The SQLite database engine does not support a number of schema operations that are supported by the majority of other relational databases.</span></span> <span data-ttu-id="313c7-126">При попытке применить одну из неподдерживаемых операций к базе данных SQLite `NotSupportedException` будет выдано исключение.</span><span class="sxs-lookup"><span data-stu-id="313c7-126">If you attempt to apply one of the unsupported operations to a SQLite database then a `NotSupportedException` will be thrown.</span></span>

<span data-ttu-id="313c7-127">Будет предпринята попытка перестроения для выполнения определенных операций.</span><span class="sxs-lookup"><span data-stu-id="313c7-127">A rebuild will be attempted in order to perform certain operations.</span></span> <span data-ttu-id="313c7-128">Перестроение возможно только для артефактов базы данных, которые являются частью модели EF Core.</span><span class="sxs-lookup"><span data-stu-id="313c7-128">Rebuilds are only possible for database artifacts that are part of your EF Core model.</span></span> <span data-ttu-id="313c7-129">Если артефакт базы данных не является частью модели, например, если он был создан вручную внутри миграции, то создается `NotSupportedException` исключение.</span><span class="sxs-lookup"><span data-stu-id="313c7-129">If a database artifact isn't part of the model--for example, if it was created manually inside a migration--then a `NotSupportedException` is still thrown.</span></span>

| <span data-ttu-id="313c7-130">Операция</span><span class="sxs-lookup"><span data-stu-id="313c7-130">Operation</span></span>            | <span data-ttu-id="313c7-131">Поддержка</span><span class="sxs-lookup"><span data-stu-id="313c7-131">Supported?</span></span>  | <span data-ttu-id="313c7-132">Требуется версия</span><span class="sxs-lookup"><span data-stu-id="313c7-132">Requires version</span></span> |
|:---------------------|:------------|:-----------------|
| <span data-ttu-id="313c7-133">аддчеккконстраинт</span><span class="sxs-lookup"><span data-stu-id="313c7-133">AddCheckConstraint</span></span>   | <span data-ttu-id="313c7-134">✔ (Перестроение)</span><span class="sxs-lookup"><span data-stu-id="313c7-134">✔ (rebuild)</span></span> | <span data-ttu-id="313c7-135">5.0</span><span class="sxs-lookup"><span data-stu-id="313c7-135">5.0</span></span>              |
| <span data-ttu-id="313c7-136">AddColumn</span><span class="sxs-lookup"><span data-stu-id="313c7-136">AddColumn</span></span>            | <span data-ttu-id="313c7-137">✔</span><span class="sxs-lookup"><span data-stu-id="313c7-137">✔</span></span>           |                  |
| <span data-ttu-id="313c7-138">аддфореигнкэй</span><span class="sxs-lookup"><span data-stu-id="313c7-138">AddForeignKey</span></span>        | <span data-ttu-id="313c7-139">✔ (Перестроение)</span><span class="sxs-lookup"><span data-stu-id="313c7-139">✔ (rebuild)</span></span> | <span data-ttu-id="313c7-140">5.0</span><span class="sxs-lookup"><span data-stu-id="313c7-140">5.0</span></span>              |
| <span data-ttu-id="313c7-141">аддпримарикэй</span><span class="sxs-lookup"><span data-stu-id="313c7-141">AddPrimaryKey</span></span>        | <span data-ttu-id="313c7-142">✔ (Перестроение)</span><span class="sxs-lookup"><span data-stu-id="313c7-142">✔ (rebuild)</span></span> | <span data-ttu-id="313c7-143">5.0</span><span class="sxs-lookup"><span data-stu-id="313c7-143">5.0</span></span>              |
| <span data-ttu-id="313c7-144">аддуникуеконстраинт</span><span class="sxs-lookup"><span data-stu-id="313c7-144">AddUniqueConstraint</span></span>  | <span data-ttu-id="313c7-145">✔ (Перестроение)</span><span class="sxs-lookup"><span data-stu-id="313c7-145">✔ (rebuild)</span></span> | <span data-ttu-id="313c7-146">5.0</span><span class="sxs-lookup"><span data-stu-id="313c7-146">5.0</span></span>              |
| <span data-ttu-id="313c7-147">алтерколумн</span><span class="sxs-lookup"><span data-stu-id="313c7-147">AlterColumn</span></span>          | <span data-ttu-id="313c7-148">✔ (Перестроение)</span><span class="sxs-lookup"><span data-stu-id="313c7-148">✔ (rebuild)</span></span> | <span data-ttu-id="313c7-149">5.0</span><span class="sxs-lookup"><span data-stu-id="313c7-149">5.0</span></span>              |
| <span data-ttu-id="313c7-150">CreateIndex</span><span class="sxs-lookup"><span data-stu-id="313c7-150">CreateIndex</span></span>          | <span data-ttu-id="313c7-151">✔</span><span class="sxs-lookup"><span data-stu-id="313c7-151">✔</span></span>           |                  |
| <span data-ttu-id="313c7-152">CreateTable</span><span class="sxs-lookup"><span data-stu-id="313c7-152">CreateTable</span></span>          | <span data-ttu-id="313c7-153">✔</span><span class="sxs-lookup"><span data-stu-id="313c7-153">✔</span></span>           |                  |
| <span data-ttu-id="313c7-154">дропчеккконстраинт</span><span class="sxs-lookup"><span data-stu-id="313c7-154">DropCheckConstraint</span></span>  | <span data-ttu-id="313c7-155">✔ (Перестроение)</span><span class="sxs-lookup"><span data-stu-id="313c7-155">✔ (rebuild)</span></span> | <span data-ttu-id="313c7-156">5.0</span><span class="sxs-lookup"><span data-stu-id="313c7-156">5.0</span></span>              |
| <span data-ttu-id="313c7-157">DropColumn</span><span class="sxs-lookup"><span data-stu-id="313c7-157">DropColumn</span></span>           | <span data-ttu-id="313c7-158">✔ (Перестроение)</span><span class="sxs-lookup"><span data-stu-id="313c7-158">✔ (rebuild)</span></span> | <span data-ttu-id="313c7-159">5.0</span><span class="sxs-lookup"><span data-stu-id="313c7-159">5.0</span></span>              |
| <span data-ttu-id="313c7-160">дропфореигнкэй</span><span class="sxs-lookup"><span data-stu-id="313c7-160">DropForeignKey</span></span>       | <span data-ttu-id="313c7-161">✔ (Перестроение)</span><span class="sxs-lookup"><span data-stu-id="313c7-161">✔ (rebuild)</span></span> | <span data-ttu-id="313c7-162">5.0</span><span class="sxs-lookup"><span data-stu-id="313c7-162">5.0</span></span>              |
| <span data-ttu-id="313c7-163">DropIndex</span><span class="sxs-lookup"><span data-stu-id="313c7-163">DropIndex</span></span>            | <span data-ttu-id="313c7-164">✔</span><span class="sxs-lookup"><span data-stu-id="313c7-164">✔</span></span>           |                  |
| <span data-ttu-id="313c7-165">дроппримарикэй</span><span class="sxs-lookup"><span data-stu-id="313c7-165">DropPrimaryKey</span></span>       | <span data-ttu-id="313c7-166">✔ (Перестроение)</span><span class="sxs-lookup"><span data-stu-id="313c7-166">✔ (rebuild)</span></span> | <span data-ttu-id="313c7-167">5.0</span><span class="sxs-lookup"><span data-stu-id="313c7-167">5.0</span></span>              |
| <span data-ttu-id="313c7-168">DropTable</span><span class="sxs-lookup"><span data-stu-id="313c7-168">DropTable</span></span>            | <span data-ttu-id="313c7-169">✔</span><span class="sxs-lookup"><span data-stu-id="313c7-169">✔</span></span>           |                  |
| <span data-ttu-id="313c7-170">дропуникуеконстраинт</span><span class="sxs-lookup"><span data-stu-id="313c7-170">DropUniqueConstraint</span></span> | <span data-ttu-id="313c7-171">✔ (Перестроение)</span><span class="sxs-lookup"><span data-stu-id="313c7-171">✔ (rebuild)</span></span> | <span data-ttu-id="313c7-172">5.0</span><span class="sxs-lookup"><span data-stu-id="313c7-172">5.0</span></span>              |
| <span data-ttu-id="313c7-173">ренамеколумн</span><span class="sxs-lookup"><span data-stu-id="313c7-173">RenameColumn</span></span>         | <span data-ttu-id="313c7-174">✔</span><span class="sxs-lookup"><span data-stu-id="313c7-174">✔</span></span>           | <span data-ttu-id="313c7-175">2.2</span><span class="sxs-lookup"><span data-stu-id="313c7-175">2.2</span></span>              |
| <span data-ttu-id="313c7-176">ренамеиндекс</span><span class="sxs-lookup"><span data-stu-id="313c7-176">RenameIndex</span></span>          | <span data-ttu-id="313c7-177">✔ (Перестроение)</span><span class="sxs-lookup"><span data-stu-id="313c7-177">✔ (rebuild)</span></span> |                  |
| <span data-ttu-id="313c7-178">ренаметабле</span><span class="sxs-lookup"><span data-stu-id="313c7-178">RenameTable</span></span>          | <span data-ttu-id="313c7-179">✔</span><span class="sxs-lookup"><span data-stu-id="313c7-179">✔</span></span>           |                  |
| <span data-ttu-id="313c7-180">енсуресчема</span><span class="sxs-lookup"><span data-stu-id="313c7-180">EnsureSchema</span></span>         | <span data-ttu-id="313c7-181">✔ (без op)</span><span class="sxs-lookup"><span data-stu-id="313c7-181">✔ (no-op)</span></span>   |                  |
| <span data-ttu-id="313c7-182">DropSchema</span><span class="sxs-lookup"><span data-stu-id="313c7-182">DropSchema</span></span>           | <span data-ttu-id="313c7-183">✔ (без op)</span><span class="sxs-lookup"><span data-stu-id="313c7-183">✔ (no-op)</span></span>   |                  |
| <span data-ttu-id="313c7-184">Вставить</span><span class="sxs-lookup"><span data-stu-id="313c7-184">Insert</span></span>               | <span data-ttu-id="313c7-185">✔</span><span class="sxs-lookup"><span data-stu-id="313c7-185">✔</span></span>           |                  |
| <span data-ttu-id="313c7-186">Update</span><span class="sxs-lookup"><span data-stu-id="313c7-186">Update</span></span>               | <span data-ttu-id="313c7-187">✔</span><span class="sxs-lookup"><span data-stu-id="313c7-187">✔</span></span>           |                  |
| <span data-ttu-id="313c7-188">DELETE</span><span class="sxs-lookup"><span data-stu-id="313c7-188">Delete</span></span>               | <span data-ttu-id="313c7-189">✔</span><span class="sxs-lookup"><span data-stu-id="313c7-189">✔</span></span>           |                  |

### <a name="migrations-limitations-workaround"></a><span data-ttu-id="313c7-190">Обходные ограничения для миграции</span><span class="sxs-lookup"><span data-stu-id="313c7-190">Migrations limitations workaround</span></span>

<span data-ttu-id="313c7-191">Некоторые из этих ограничений можно решить, вручную записав код в миграции для выполнения перестроения.</span><span class="sxs-lookup"><span data-stu-id="313c7-191">You can workaround some of these limitations by manually writing code in your migrations to perform a rebuild.</span></span> <span data-ttu-id="313c7-192">Перестроение таблиц подразумевает создание новой таблицы, копирование данных в новую таблицу, удаление старой таблицы и переименование новой таблицы.</span><span class="sxs-lookup"><span data-stu-id="313c7-192">Table rebuilds involve creating a new table, copying data to the new table, dropping the old table, renaming the new table.</span></span> <span data-ttu-id="313c7-193">`Sql(string)`Для выполнения некоторых из этих действий потребуется использовать метод.</span><span class="sxs-lookup"><span data-stu-id="313c7-193">You will need to use the `Sql(string)` method to perform some of these steps.</span></span>

<span data-ttu-id="313c7-194">Дополнительные сведения см. в разделе [внесение других изменений схемы таблицы](https://sqlite.org/lang_altertable.html#otheralter) в документацию по SQLite.</span><span class="sxs-lookup"><span data-stu-id="313c7-194">See [Making Other Kinds Of Table Schema Changes](https://sqlite.org/lang_altertable.html#otheralter) in the SQLite documentation for more details.</span></span>

### <a name="idempotent-script-limitations"></a><span data-ttu-id="313c7-195">Ограничения сценария идемпотентными</span><span class="sxs-lookup"><span data-stu-id="313c7-195">Idempotent script limitations</span></span>

<span data-ttu-id="313c7-196">В отличие от других баз данных, SQLite не включает процедурный язык.</span><span class="sxs-lookup"><span data-stu-id="313c7-196">Unlike other databases, SQLite doesn't include a procedural language.</span></span> <span data-ttu-id="313c7-197">Поэтому не существует способа создания логики if-then, необходимой для сценариев миграции идемпотентными.</span><span class="sxs-lookup"><span data-stu-id="313c7-197">Because of this, there is no way to generate the if-then logic required by the idempotent migration scripts.</span></span>

<span data-ttu-id="313c7-198">Если вы знакомы с последней миграцией, примененной к базе данных, можно создать сценарий из этой миграции на последнюю миграцию.</span><span class="sxs-lookup"><span data-stu-id="313c7-198">If you know the last migration applied to a database, you can generate a script from that migration to the latest migration.</span></span>

```dotnetcli
dotnet ef migrations script CurrentMigration
```

<span data-ttu-id="313c7-199">В противном случае мы рекомендуем использовать `dotnet ef database update` для применения миграций.</span><span class="sxs-lookup"><span data-stu-id="313c7-199">Otherwise, we recommend using `dotnet ef database update` to apply migrations.</span></span> <span data-ttu-id="313c7-200">Начиная с EF Core 5,0, можно указать файл базы данных при выполнении команды.</span><span class="sxs-lookup"><span data-stu-id="313c7-200">Starting in EF Core 5.0, you can specify the database file when running the command.</span></span>

```dotnetcli
dotnet ef database update --connection "Data Source=My.db"
```

## <a name="see-also"></a><span data-ttu-id="313c7-201">См. также статью</span><span class="sxs-lookup"><span data-stu-id="313c7-201">See also</span></span>

* [<span data-ttu-id="313c7-202">Ограничения Async Microsoft. Data. SQLite</span><span class="sxs-lookup"><span data-stu-id="313c7-202">Microsoft.Data.Sqlite Async Limitations</span></span>](/dotnet/standard/data/sqlite/async)
* [<span data-ttu-id="313c7-203">Ограничения Microsoft. Data. SQLite ADO.NET</span><span class="sxs-lookup"><span data-stu-id="313c7-203">Microsoft.Data.Sqlite ADO.NET Limitations</span></span>](/dotnet/standard/data/sqlite/adonet-limitations)
