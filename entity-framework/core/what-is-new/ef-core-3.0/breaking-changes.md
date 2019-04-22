---
title: Критические изменения в EF Core 3.0 — EF Core
author: divega
ms.date: 02/19/2019
ms.assetid: EE2878C9-71F9-4FA5-9BC4-60517C7C9830
uid: core/what-is-new/ef-core-3.0/breaking-changes
ms.openlocfilehash: 4b251638de43af6525f3e6faa0bd4113ab1714b9
ms.sourcegitcommit: 5280dcac4423acad8b440143433459b18886115b
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/16/2019
ms.locfileid: "59619263"
---
# <a name="breaking-changes-included-in-ef-core-30-currently-in-preview"></a><span data-ttu-id="d5515-102">Критические изменения в EF Core 3.0 (сейчас предоставляются в виде предварительной версии)</span><span class="sxs-lookup"><span data-stu-id="d5515-102">Breaking changes included in EF Core 3.0 (currently in preview)</span></span>

> [!IMPORTANT]
> <span data-ttu-id="d5515-103">Обратите внимание на то, что наборы возможностей и расписания будущих выпусков могут быть изменены, и, несмотря на то, что мы стараемся поддерживать эту страницу в актуальном состоянии, в определенные моменты времени содержание страницы может не отражать наши текущие планы.</span><span class="sxs-lookup"><span data-stu-id="d5515-103">Please note that the feature sets and schedules of future releases are always subject to change, and although we will try to keep this page up to date, it may not reflect our latest plans at all times.</span></span>

<span data-ttu-id="d5515-104">Указанные ниже изменения API и поведения могут нарушать работу приложений, разработанных для EF Core 2.2.x, при их обновлении до версии 3.0.0.</span><span class="sxs-lookup"><span data-stu-id="d5515-104">The following API and behavior changes have the potential to break applications developed for EF Core 2.2.x when upgrading them to 3.0.0.</span></span>
<span data-ttu-id="d5515-105">Изменения, которые, по нашим ожиданиям, повлияют только на поставщиков баз данных, описаны в разделе [изменений для поставщиков](../../providers/provider-log.md).</span><span class="sxs-lookup"><span data-stu-id="d5515-105">Changes that we expect to only impact database providers are documented under [provider changes](../../providers/provider-log.md).</span></span>
<span data-ttu-id="d5515-106">Здесь не описаны критические изменения в новых функциях, добавленных между разными предварительными версиями 3.0.</span><span class="sxs-lookup"><span data-stu-id="d5515-106">Breaks in new features introduced from one 3.0 preview to another 3.0 preview aren't documented here.</span></span>

## <a name="linq-queries-are-no-longer-evaluated-on-the-client"></a><span data-ttu-id="d5515-107">Запросы LINQ больше не вычисляются на клиенте</span><span class="sxs-lookup"><span data-stu-id="d5515-107">LINQ queries are no longer evaluated on the client</span></span>

<span data-ttu-id="d5515-108">[Отслеживание вопроса № 14935](https://github.com/aspnet/EntityFrameworkCore/issues/14935)
[Также см. вопрос № 12795](https://github.com/aspnet/EntityFrameworkCore/issues/12795)</span><span class="sxs-lookup"><span data-stu-id="d5515-108">[Tracking Issue #14935](https://github.com/aspnet/EntityFrameworkCore/issues/14935)
[Also see issue #12795](https://github.com/aspnet/EntityFrameworkCore/issues/12795)</span></span>

<span data-ttu-id="d5515-109">Это изменение будет внесено в предварительную версию 4 EF Core 3.0.</span><span class="sxs-lookup"><span data-stu-id="d5515-109">This change will be introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="d5515-110">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="d5515-110">**Old behavior**</span></span>

<span data-ttu-id="d5515-111">До выхода версии 3.0, если системе EF Core не удавалось преобразовать выражение, являющееся частью запроса, в код SQL или параметр, она автоматически рассчитывала это выражение на клиенте.</span><span class="sxs-lookup"><span data-stu-id="d5515-111">Before 3.0, when EF Core couldn't convert an expression that was part of a query to either SQL or a parameter, it automatically evaluated the expression on the client.</span></span>
<span data-ttu-id="d5515-112">По умолчанию вычисление на клиенте потенциально ресурсоемких выражений вызывало лишь отображение предупреждения.</span><span class="sxs-lookup"><span data-stu-id="d5515-112">By default, client evaluation of potentially expensive expressions only triggered a warning.</span></span>

<span data-ttu-id="d5515-113">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="d5515-113">**New behavior**</span></span>

<span data-ttu-id="d5515-114">Начиная с версии 3.0 система EF Core разрешает вычислять на клиенте только выражения в высокоуровневой проекции (последний вызов `Select()` в запросе).</span><span class="sxs-lookup"><span data-stu-id="d5515-114">Starting with 3.0, EF Core only allows expressions in the top-level projection (the last `Select()` call in the query) to be evaluated on the client.</span></span>
<span data-ttu-id="d5515-115">Если выражения в любой другой части запроса невозможно преобразовать в код SQL или параметр, возникает исключение.</span><span class="sxs-lookup"><span data-stu-id="d5515-115">When expressions in any other part of the query can't be converted to either SQL or a parameter, an exception is thrown.</span></span>

<span data-ttu-id="d5515-116">**Причина**</span><span class="sxs-lookup"><span data-stu-id="d5515-116">**Why**</span></span>

<span data-ttu-id="d5515-117">Автоматическое вычисление запросов на клиенте позволяет выполнять многие запросы, даже если не удается преобразовать их важные части.</span><span class="sxs-lookup"><span data-stu-id="d5515-117">Automatic client evaluation of queries allows many queries to be executed even if important parts of them can't be translated.</span></span>
<span data-ttu-id="d5515-118">Это может привести к непредвиденному и потенциально опасному поведению, которое может стать заметным только в рабочей среде.</span><span class="sxs-lookup"><span data-stu-id="d5515-118">This behavior can result in unexpected and potentially damaging behavior that may only become evident in production.</span></span>
<span data-ttu-id="d5515-119">Например, условие в вызове `Where()`, которое невозможно преобразовать, может привести к передаче всех строк из таблицы с сервера базы данных и к применению фильтра на клиенте.</span><span class="sxs-lookup"><span data-stu-id="d5515-119">For example, a condition in a `Where()` call which can't be translated can cause all rows from the table to be transferred from the database server, and the filter to be applied on the client.</span></span>
<span data-ttu-id="d5515-120">Такая ситуация легко может остаться незамеченной, если таблица содержит небольшое число строк в разработке, и дать резкие отрицательные последствия при переносе приложения в рабочую среду, когда таблица может содержать миллионы строк.</span><span class="sxs-lookup"><span data-stu-id="d5515-120">This situation can easily go undetected if the table contains only a few rows in development, but hit hard when the application moves to production, where the table may contain millions of rows.</span></span>
<span data-ttu-id="d5515-121">Практика показала, что предупреждения о вычислении на клиенте также часто игнорируются во время разработки.</span><span class="sxs-lookup"><span data-stu-id="d5515-121">Client evaluation warnings also proved too easy to ignore during development.</span></span>

<span data-ttu-id="d5515-122">Кроме того, автоматическое вычисление на клиенте может привести к проблемам, из-за которых улучшение преобразования запросов для определенных выражений приводит к возникновению непреднамеренных критических изменений между выпусками.</span><span class="sxs-lookup"><span data-stu-id="d5515-122">Besides this, automatic client evaluation can lead to issues in which improving query translation for specific expressions caused unintended breaking changes between releases.</span></span>

<span data-ttu-id="d5515-123">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="d5515-123">**Mitigations**</span></span>

<span data-ttu-id="d5515-124">Если запрос невозможно преобразовать полностью, то перепишите его в форме, которую можно преобразовать, либо используйте `AsEnumerable()`, `ToList()` или аналогичный элемент, чтобы явно перенести данные обратно на клиент, где можно произвести их дальнейшую обработку с помощью LINQ-to-Objects.</span><span class="sxs-lookup"><span data-stu-id="d5515-124">If a query can't be fully translated, then either rewrite the query in a form that can be translated, or use `AsEnumerable()`, `ToList()`, or similar to explicitly bring data back to the client where it can then be further processed using LINQ-to-Objects.</span></span>

## <a name="entity-framework-core-is-no-longer-part-of-the-aspnet-core-shared-framework"></a><span data-ttu-id="d5515-125">Entity Framework Core больше не является частью общей платформы ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="d5515-125">Entity Framework Core is no longer part of the ASP.NET Core shared framework</span></span>

[<span data-ttu-id="d5515-126">Отслеживание объявлений о вопросе 325</span><span class="sxs-lookup"><span data-stu-id="d5515-126">Tracking Issue Announcements#325</span></span>](https://github.com/aspnet/Announcements/issues/325)

<span data-ttu-id="d5515-127">Это изменение было внесено в предварительную версию 1 ASP.NET Core 3.0.</span><span class="sxs-lookup"><span data-stu-id="d5515-127">This change was introduced in ASP.NET Core 3.0-preview 1.</span></span> 

<span data-ttu-id="d5515-128">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="d5515-128">**Old behavior**</span></span>

<span data-ttu-id="d5515-129">До выхода ASP.NET Core 3.0 при добавлении ссылки на пакет для `Microsoft.AspNetCore.App` или `Microsoft.AspNetCore.All` она включала в себя EF Core и некоторые поставщики данных EF Core, например поставщик SQL Server.</span><span class="sxs-lookup"><span data-stu-id="d5515-129">Before ASP.NET Core 3.0, when you added a package reference to `Microsoft.AspNetCore.App` or `Microsoft.AspNetCore.All`, it would include EF Core and some of the EF Core data providers like the SQL Server provider.</span></span>

<span data-ttu-id="d5515-130">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="d5515-130">**New behavior**</span></span>

<span data-ttu-id="d5515-131">Начиная с версии 3.0 общая платформа ASP.NET Core не включает в себя EF Core или поставщики данных EF Core.</span><span class="sxs-lookup"><span data-stu-id="d5515-131">Starting in 3.0, the ASP.NET Core shared framework doesn't include EF Core or any EF Core data providers.</span></span>

<span data-ttu-id="d5515-132">**Причина**</span><span class="sxs-lookup"><span data-stu-id="d5515-132">**Why**</span></span>

<span data-ttu-id="d5515-133">До этого изменения для получения EF Core требовались различные действия в зависимости от того, ориентировано ли приложение на ASP.NET Core и SQL Server.</span><span class="sxs-lookup"><span data-stu-id="d5515-133">Before this change, getting EF Core required different steps depending on whether the application targeted ASP.NET Core and SQL Server or not.</span></span> <span data-ttu-id="d5515-134">Кроме того, обновление ASP.NET Core приводило к принудительному обновлению EF Core и поставщика SQL Server, что иногда нежелательно.</span><span class="sxs-lookup"><span data-stu-id="d5515-134">Also, upgrading ASP.NET Core forced the upgrade of EF Core and the SQL Server provider, which isn't always desirable.</span></span>

<span data-ttu-id="d5515-135">Благодаря этому изменению процесс получения EF Core стал одинаковым для всех поставщиков, поддерживаемых реализаций .NET и типов приложений.</span><span class="sxs-lookup"><span data-stu-id="d5515-135">With this change, the experience of getting EF Core is the same across all providers, supported .NET implementations and application types.</span></span>
<span data-ttu-id="d5515-136">Кроме того, теперь разработчики могут точно управлять временем обновления EF Core и поставщиков данных EF Core.</span><span class="sxs-lookup"><span data-stu-id="d5515-136">Developers can also now control exactly when EF Core and EF Core data providers are upgraded.</span></span>

<span data-ttu-id="d5515-137">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="d5515-137">**Mitigations**</span></span>

<span data-ttu-id="d5515-138">Чтобы использовать EF Core в приложении ASP.NET Core 3.0 или любом другом поддерживаемом приложении, нужно явно добавить ссылку на пакет в поставщик базы данных EF Core, который ваше приложение будет использовать.</span><span class="sxs-lookup"><span data-stu-id="d5515-138">To use EF Core in an ASP.NET Core 3.0 application or any other supported application, explicitly add a package reference to the EF Core database provider that your application will use.</span></span>

## <a name="fromsql-executesql-and-executesqlasync-have-been-renamed"></a><span data-ttu-id="d5515-139">FromSql, ExecuteSql и ExecuteSqlAsync были переименованы</span><span class="sxs-lookup"><span data-stu-id="d5515-139">FromSql, ExecuteSql, and ExecuteSqlAsync have been renamed</span></span>

[<span data-ttu-id="d5515-140">Отслеживание вопроса № 10996</span><span class="sxs-lookup"><span data-stu-id="d5515-140">Tracking Issue #10996</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10996)

<span data-ttu-id="d5515-141">Это изменение было внесено в предварительную версию 4 EF Core 3.0.</span><span class="sxs-lookup"><span data-stu-id="d5515-141">This change was introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="d5515-142">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="d5515-142">**Old behavior**</span></span>

<span data-ttu-id="d5515-143">До выхода EF Core 3.0 имена этих методов были перегружены для работы с обычной строкой или строкой, которая должна быть интерполирована в SQL и параметры.</span><span class="sxs-lookup"><span data-stu-id="d5515-143">Before EF Core 3.0, these method names were overloaded to work with either a normal string or a string that should be interpolated into SQL and parameters.</span></span>

<span data-ttu-id="d5515-144">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="d5515-144">**New behavior**</span></span>

<span data-ttu-id="d5515-145">Начиная с EF Core 3.0 используйте `FromSqlRaw`, `ExecuteSqlRaw` и `ExecuteSqlRawAsync` для создания параметризованного запроса, где параметры передаются отдельно из строки запроса.</span><span class="sxs-lookup"><span data-stu-id="d5515-145">Starting with EF Core 3.0, use `FromSqlRaw`, `ExecuteSqlRaw`, and `ExecuteSqlRawAsync` to create a parameterized query where the parameters are passed separately from the query string.</span></span>
<span data-ttu-id="d5515-146">Например:</span><span class="sxs-lookup"><span data-stu-id="d5515-146">For example:</span></span>

```C#
context.Products.FromSqlRaw(
    "SELECT * FROM Products WHERE Name = {0}",
    product.Name);
```

<span data-ttu-id="d5515-147">Используйте `FromSqlInterpolated`, `ExecuteSqlInterpolated` и `ExecuteSqlInterpolatedAsync` для создания параметризованного запроса, где параметры передаются как часть интерполированной строки запроса.</span><span class="sxs-lookup"><span data-stu-id="d5515-147">Use `FromSqlInterpolated`, `ExecuteSqlInterpolated`, and `ExecuteSqlInterpolatedAsync` to create a parameterized query where the parameters are passed as part of an interpolated query string.</span></span>
<span data-ttu-id="d5515-148">Например:</span><span class="sxs-lookup"><span data-stu-id="d5515-148">For example:</span></span>

```C#
context.Products.FromSqlInterpolated(
    $"SELECT * FROM Products WHERE Name = {product.Name}");
```

<span data-ttu-id="d5515-149">Обратите внимание, что оба упомянутых выше запроса будут возвращать одинаковый параметризованный SQL с одинаковыми параметрами SQL.</span><span class="sxs-lookup"><span data-stu-id="d5515-149">Note that both of the queries above will produce the same parameterized SQL with the same SQL parameters.</span></span>

<span data-ttu-id="d5515-150">**Причина**</span><span class="sxs-lookup"><span data-stu-id="d5515-150">**Why**</span></span>

<span data-ttu-id="d5515-151">Подобные перегрузки методов могут случайно вызывать метод необработанной строки в тех случаях, когда требовалось вызвать метод интерполированной строки, и наоборот.</span><span class="sxs-lookup"><span data-stu-id="d5515-151">Method overloads like this make it very easy to accidentally call the raw srting method when the intent was to call the interpolated string method, and the other way around.</span></span>
<span data-ttu-id="d5515-152">Это может привести к тому, что запросы не параметризуются, хотя они должны параметризоваться.</span><span class="sxs-lookup"><span data-stu-id="d5515-152">This could result in queries not being parameterized when they should have been.</span></span>

<span data-ttu-id="d5515-153">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="d5515-153">**Mitigations**</span></span>

<span data-ttu-id="d5515-154">Перейдите на использование новых имен методов.</span><span class="sxs-lookup"><span data-stu-id="d5515-154">Switch to use the new method names.</span></span>

## <a name="query-execution-is-logged-at-debug-level"></a><span data-ttu-id="d5515-155">Выполнение запроса регистрируется на уровне отладки Debug</span><span class="sxs-lookup"><span data-stu-id="d5515-155">Query execution is logged at Debug level</span></span>

[<span data-ttu-id="d5515-156">Отслеживание вопроса 14523</span><span class="sxs-lookup"><span data-stu-id="d5515-156">Tracking Issue #14523</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14523)

<span data-ttu-id="d5515-157">Это изменение было внесено в предварительную версию 3 EF Core 3.0.</span><span class="sxs-lookup"><span data-stu-id="d5515-157">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="d5515-158">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="d5515-158">**Old behavior**</span></span>

<span data-ttu-id="d5515-159">До выхода EF Core 3.0 выполнение запросов и других команд зарегистрировалось на уровне `Info`.</span><span class="sxs-lookup"><span data-stu-id="d5515-159">Before EF Core 3.0, execution of queries and other commands was logged at the `Info` level.</span></span>

<span data-ttu-id="d5515-160">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="d5515-160">**New behavior**</span></span>

<span data-ttu-id="d5515-161">Начиная с EF Core 3.0 регистрация выполнения команды или кода SQL осуществляется на уровне `Debug`.</span><span class="sxs-lookup"><span data-stu-id="d5515-161">Starting with EF Core 3.0, logging of command/SQL execution is at the `Debug` level.</span></span>

<span data-ttu-id="d5515-162">**Причина**</span><span class="sxs-lookup"><span data-stu-id="d5515-162">**Why**</span></span>

<span data-ttu-id="d5515-163">Это изменение было внесено для снижения шума на уровне ведения журнала `Info`.</span><span class="sxs-lookup"><span data-stu-id="d5515-163">This change was made to reduce the noise at the `Info` log level.</span></span>

<span data-ttu-id="d5515-164">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="d5515-164">**Mitigations**</span></span>

<span data-ttu-id="d5515-165">Это событие ведения журнала определено `RelationalEventId.CommandExecuting` с идентификатором события 20100.</span><span class="sxs-lookup"><span data-stu-id="d5515-165">This logging event is defined by `RelationalEventId.CommandExecuting` with event ID 20100.</span></span>
<span data-ttu-id="d5515-166">Чтобы снова начать ведение журнала SQL на уровне `Info`, явно настройте уровень в методе `OnConfiguring` или `AddDbContext`.</span><span class="sxs-lookup"><span data-stu-id="d5515-166">To log SQL at the `Info` level again, explicitly configure the level in `OnConfiguring` or `AddDbContext`.</span></span>
<span data-ttu-id="d5515-167">Например:</span><span class="sxs-lookup"><span data-stu-id="d5515-167">For example:</span></span>
```C#
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    => optionsBuilder
        .UseSqlServer(connectionString)
        .ConfigureWarnings(c => c.Log((RelationalEventId.CommandExecuting, LogLevel.Info)));
```

## <a name="temporary-key-values-are-no-longer-set-onto-entity-instances"></a><span data-ttu-id="d5515-168">Временные значения ключа больше не устанавливаются для экземпляров сущностей</span><span class="sxs-lookup"><span data-stu-id="d5515-168">Temporary key values are no longer set onto entity instances</span></span>

[<span data-ttu-id="d5515-169">Отслеживание вопроса 12378</span><span class="sxs-lookup"><span data-stu-id="d5515-169">Tracking Issue #12378</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12378)

<span data-ttu-id="d5515-170">Это изменение было внесено в предварительную версию 2 EF Core 3.0.</span><span class="sxs-lookup"><span data-stu-id="d5515-170">This change was introduced in EF Core 3.0-preview 2.</span></span>

<span data-ttu-id="d5515-171">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="d5515-171">**Old behavior**</span></span>

<span data-ttu-id="d5515-172">До выхода EF Core 3.0 временные значения назначались всем свойствам ключей, которые позднее получили бы реальное значение, созданное базой данных.</span><span class="sxs-lookup"><span data-stu-id="d5515-172">Before EF Core 3.0, temporary values were assigned to all key properties that would later have a real value generated by the database.</span></span>
<span data-ttu-id="d5515-173">Обычно эти временные значения были большими отрицательными числами.</span><span class="sxs-lookup"><span data-stu-id="d5515-173">Usually these temporary values were large negative numbers.</span></span>

<span data-ttu-id="d5515-174">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="d5515-174">**New behavior**</span></span>

<span data-ttu-id="d5515-175">Начиная с версии 3.0 EF Core сохраняет временное значение ключа как часть сведений об отслеживании сущности, оставляя само свойство ключа без изменений.</span><span class="sxs-lookup"><span data-stu-id="d5515-175">Starting with 3.0, EF Core stores the temporary key value as part of the entity's tracking information, and leaves the key property itself unchanged.</span></span>

<span data-ttu-id="d5515-176">**Причина**</span><span class="sxs-lookup"><span data-stu-id="d5515-176">**Why**</span></span>

<span data-ttu-id="d5515-177">Это изменение было внесено для ситуации, когда временные значения ключей ошибочно становились постоянными из-за перемещения сущности, которая ранее отслеживалась некоторым экземпляром `DbContext`, в другой экземпляр `DbContext`.</span><span class="sxs-lookup"><span data-stu-id="d5515-177">This change was made to prevent temporary key values from erroneously becoming permanent when an entity that has been previously tracked by some `DbContext` instance is moved to a different `DbContext` instance.</span></span> 

<span data-ttu-id="d5515-178">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="d5515-178">**Mitigations**</span></span>

<span data-ttu-id="d5515-179">Приложения, которые назначают значения первичного ключа внешним ключам для формирования ассоциаций между сущностями, могут использовать старое поведение, если первичные ключи сформированы хранилищем и принадлежат сущностям в состоянии `Added`.</span><span class="sxs-lookup"><span data-stu-id="d5515-179">Applications that assign primary key values onto foreign keys to form associations between entities may depend on the old behavior if the primary keys are store-generated and belong to entities in the `Added` state.</span></span>
<span data-ttu-id="d5515-180">Этого можно избежать следующим образом:</span><span class="sxs-lookup"><span data-stu-id="d5515-180">This can be avoided by:</span></span>
* <span data-ttu-id="d5515-181">Отказ от использования ключей, сформированных хранилищем.</span><span class="sxs-lookup"><span data-stu-id="d5515-181">Not using store-generated keys.</span></span>
* <span data-ttu-id="d5515-182">Настройка свойств навигации для формирования отношений вместо задания значений внешнего ключа.</span><span class="sxs-lookup"><span data-stu-id="d5515-182">Setting navigation properties to form relationships instead of setting foreign key values.</span></span>
* <span data-ttu-id="d5515-183">Получение фактических временных значений ключа из сведений об отслеживании сущности.</span><span class="sxs-lookup"><span data-stu-id="d5515-183">Obtain the actual temporary key values from the entity's tracking information.</span></span>
<span data-ttu-id="d5515-184">Например, `context.Entry(blog).Property(e => e.Id).CurrentValue` возвратит временное значение, даже если сам `blog.Id` не был задан.</span><span class="sxs-lookup"><span data-stu-id="d5515-184">For example, `context.Entry(blog).Property(e => e.Id).CurrentValue` will return the temporary value even though `blog.Id` itself hasn't been set.</span></span>

## <a name="detectchanges-honors-store-generated-key-values"></a><span data-ttu-id="d5515-185">DetectChanges учитывает значения ключей, сформированные хранилищем</span><span class="sxs-lookup"><span data-stu-id="d5515-185">DetectChanges honors store-generated key values</span></span>

[<span data-ttu-id="d5515-186">Отслеживание вопроса 14616</span><span class="sxs-lookup"><span data-stu-id="d5515-186">Tracking Issue #14616</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14616)

<span data-ttu-id="d5515-187">Это изменение было внесено в предварительную версию 3 EF Core 3.0.</span><span class="sxs-lookup"><span data-stu-id="d5515-187">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="d5515-188">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="d5515-188">**Old behavior**</span></span>

<span data-ttu-id="d5515-189">До выпуска EF Core 3.0 неотслеживаемая сущность, обнаруженная `DetectChanges`, отслеживалась в состоянии `Added` и вставлялась в новую строку при вызове `SaveChanges`.</span><span class="sxs-lookup"><span data-stu-id="d5515-189">Before EF Core 3.0, an untracked entity found by `DetectChanges` would be tracked in the `Added` state and inserted as a new row when `SaveChanges` is called.</span></span>

<span data-ttu-id="d5515-190">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="d5515-190">**New behavior**</span></span>

<span data-ttu-id="d5515-191">Начиная с EF Core 3.0, если сущность использует сформированные значения ключа и какое-либо значение ключа задано, то она будет отслеживаться в состоянии `Modified`.</span><span class="sxs-lookup"><span data-stu-id="d5515-191">Starting with EF Core 3.0, if an entity is using generated key values and some key value is set, then the entity will be tracked in the `Modified` state.</span></span>
<span data-ttu-id="d5515-192">Это означает, что предполагается наличие строки для сущности, и она будет обновлена при вызове `SaveChanges`.</span><span class="sxs-lookup"><span data-stu-id="d5515-192">This means that a row for the entity is assumed to exist and it will be updated when `SaveChanges` is called.</span></span>
<span data-ttu-id="d5515-193">Если значение ключа не задано или тип сущности не использует сформированные ключи, то новая сущность все равно будет отслеживаться в состоянии `Added`, как в предыдущих версиях.</span><span class="sxs-lookup"><span data-stu-id="d5515-193">If the key value isn't set, or if the entity type isn't using generated keys, then the new entity will still be tracked as `Added` as in previous versions.</span></span>

<span data-ttu-id="d5515-194">**Причина**</span><span class="sxs-lookup"><span data-stu-id="d5515-194">**Why**</span></span>

<span data-ttu-id="d5515-195">Это изменение было внесено, чтобы сделать работу с несвязными графами сущностей при использовании ключей, сформированных хранилищем, более простой и согласованной.</span><span class="sxs-lookup"><span data-stu-id="d5515-195">This change was made to make it easier and more consistent to work with disconnected entity graphs while using store-generated keys.</span></span>

<span data-ttu-id="d5515-196">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="d5515-196">**Mitigations**</span></span>

<span data-ttu-id="d5515-197">Это изменение может нарушить работу приложения, если тип сущности должен использовать сформированные ключи, однако значения ключей явно заданы для новых экземпляров.</span><span class="sxs-lookup"><span data-stu-id="d5515-197">This change can break an application if an entity type is configured to use generated keys but key values are explicitly set for new instances.</span></span>
<span data-ttu-id="d5515-198">Решение проблемы заключается в явном запрете свойствам ключей использовать сформированные значения.</span><span class="sxs-lookup"><span data-stu-id="d5515-198">The fix is to explicitly configure the key properties to not use generated values.</span></span>
<span data-ttu-id="d5515-199">Например, с помощью текучего API:</span><span class="sxs-lookup"><span data-stu-id="d5515-199">For example, with the fluent API:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .ValueGeneratedNever();
```

<span data-ttu-id="d5515-200">Или с помощью заметок к данным:</span><span class="sxs-lookup"><span data-stu-id="d5515-200">Or with data annotations:</span></span>

```C#
[DatabaseGenerated(DatabaseGeneratedOption.None)]
public string Id { get; set; }
```

## <a name="cascade-deletions-now-happen-immediately-by-default"></a><span data-ttu-id="d5515-201">Каскадные удаления теперь по умолчанию выполняются немедленно</span><span class="sxs-lookup"><span data-stu-id="d5515-201">Cascade deletions now happen immediately by default</span></span>

[<span data-ttu-id="d5515-202">Отслеживание вопроса 10114</span><span class="sxs-lookup"><span data-stu-id="d5515-202">Tracking Issue #10114</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10114)

<span data-ttu-id="d5515-203">Это изменение было внесено в предварительную версию 3 EF Core 3.0.</span><span class="sxs-lookup"><span data-stu-id="d5515-203">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="d5515-204">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="d5515-204">**Old behavior**</span></span>

<span data-ttu-id="d5515-205">До выхода версии 3.0 применяемые EF Core каскадные действия (удаление зависимых сущностей, когда требуемый субъект удален либо отношение с ним было разорвано) не выполнялись до вызова SaveChanges.</span><span class="sxs-lookup"><span data-stu-id="d5515-205">Before 3.0, EF Core applied cascading actions (deleting dependent entities when a required principal is deleted or when the relationship to a required principal is severed) did not happen until SaveChanges was called.</span></span>

<span data-ttu-id="d5515-206">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="d5515-206">**New behavior**</span></span>

<span data-ttu-id="d5515-207">Начиная с версии 3.0 EF Core применяет каскадные действия сразу после обнаружения условия триггера.</span><span class="sxs-lookup"><span data-stu-id="d5515-207">Starting with 3.0, EF Core applies cascading actions as soon as the triggering condition is detected.</span></span>
<span data-ttu-id="d5515-208">Например, вызов `context.Remove()` для удаления сущности субъекта приведет к немедленной установке всех отслеживаемых связанных необходимых зависимых объектов в состояние `Deleted`.</span><span class="sxs-lookup"><span data-stu-id="d5515-208">For example, calling `context.Remove()` to delete a principal entity will result in all tracked related required dependents also being set to `Deleted` immediately.</span></span>

<span data-ttu-id="d5515-209">**Причина**</span><span class="sxs-lookup"><span data-stu-id="d5515-209">**Why**</span></span>

<span data-ttu-id="d5515-210">Это изменение было внесено, чтобы улучшить работу в сценариях аудита и привязки данных, где важно понимать, какие сущности будут удалены _перед_ вызовом `SaveChanges`.</span><span class="sxs-lookup"><span data-stu-id="d5515-210">This change was made to improve the experience for data binding and auditing scenarios where it is important to understand which entities will be deleted _before_ `SaveChanges` is called.</span></span>

<span data-ttu-id="d5515-211">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="d5515-211">**Mitigations**</span></span>

<span data-ttu-id="d5515-212">Для восстановления прежнего поведения можно использовать параметры `context.ChangedTracker`.</span><span class="sxs-lookup"><span data-stu-id="d5515-212">The previous behavior can be restored through settings on `context.ChangedTracker`.</span></span>
<span data-ttu-id="d5515-213">Например:</span><span class="sxs-lookup"><span data-stu-id="d5515-213">For example:</span></span>

```C#
context.ChangeTracker.CascadeDeleteTiming = CascadeTiming.OnSaveChanges;
context.ChangeTracker.DeleteOrphansTiming = CascadeTiming.OnSaveChanges;
```

## <a name="deletebehaviorrestrict-has-cleaner-semantics"></a><span data-ttu-id="d5515-214">более четкая семантика DeleteBehavior.Restrict</span><span class="sxs-lookup"><span data-stu-id="d5515-214">DeleteBehavior.Restrict has cleaner semantics</span></span>

[<span data-ttu-id="d5515-215">Отслеживание вопроса № 12661</span><span class="sxs-lookup"><span data-stu-id="d5515-215">Tracking Issue #12661</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12661)

<span data-ttu-id="d5515-216">Это изменение будет внесено в предварительную версию 5 EF Core 3.0.</span><span class="sxs-lookup"><span data-stu-id="d5515-216">This change will be introduced in EF Core 3.0-preview 5.</span></span>

<span data-ttu-id="d5515-217">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="d5515-217">**Old behavior**</span></span>

<span data-ttu-id="d5515-218">До версии 3.0 `DeleteBehavior.Restrict` создавал внешние ключи в базе данных с помощью семантики `Restrict`, но также изменял внутреннее исправление неочевидным образом.</span><span class="sxs-lookup"><span data-stu-id="d5515-218">Before 3.0, `DeleteBehavior.Restrict` created foreign keys in the database with `Restrict` semantics, but also changed internal fixup in a non-obvious way.</span></span>

<span data-ttu-id="d5515-219">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="d5515-219">**New behavior**</span></span>

<span data-ttu-id="d5515-220">Начиная с версии 3.0 `DeleteBehavior.Restrict` обеспечивает создание внешних ключей с помощью семантики `Restrict`, то есть без каскадов; создается при нарушении ограничения, не влияя на внутреннее исправление EF.</span><span class="sxs-lookup"><span data-stu-id="d5515-220">Starting with 3.0, `DeleteBehavior.Restrict` ensures that foreign keys are created with `Restrict` semantics--that is, no cascades; throw on constraint violation--without also impacting EF internal fixup.</span></span>

<span data-ttu-id="d5515-221">**Причина**</span><span class="sxs-lookup"><span data-stu-id="d5515-221">**Why**</span></span>

<span data-ttu-id="d5515-222">Это изменение было внесено, чтобы упростить работу с помощью более интуитивного использования `DeleteBehavior` без непредвиденных побочных эффектов.</span><span class="sxs-lookup"><span data-stu-id="d5515-222">This change was made to improve the experience for using `DeleteBehavior` in an intuitive manner, without unexpected side-effects.</span></span>

<span data-ttu-id="d5515-223">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="d5515-223">**Mitigations**</span></span>

<span data-ttu-id="d5515-224">Для восстановления прежнего поведения можно использовать `DeleteBehavior.ClientNoAction`.</span><span class="sxs-lookup"><span data-stu-id="d5515-224">The previous behavior can be restored by using `DeleteBehavior.ClientNoAction`.</span></span>

## <a name="query-types-are-consolidated-with-entity-types"></a><span data-ttu-id="d5515-225">Типы запросов объединяются с типами сущностей</span><span class="sxs-lookup"><span data-stu-id="d5515-225">Query types are consolidated with entity types</span></span>

[<span data-ttu-id="d5515-226">Отслеживание вопроса 14194</span><span class="sxs-lookup"><span data-stu-id="d5515-226">Tracking Issue #14194</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14194)

<span data-ttu-id="d5515-227">Это изменение было внесено в предварительную версию 3 EF Core 3.0.</span><span class="sxs-lookup"><span data-stu-id="d5515-227">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="d5515-228">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="d5515-228">**Old behavior**</span></span>

<span data-ttu-id="d5515-229">До выпуска EF Core 3.0 [типы запросов](xref:core/modeling/query-types) представляли собой средства для запроса данных, не определяющие первичный ключ структурированным образом.</span><span class="sxs-lookup"><span data-stu-id="d5515-229">Before EF Core 3.0, [query types](xref:core/modeling/query-types) were a means to query data that doesn't define a primary key in a structured way.</span></span>
<span data-ttu-id="d5515-230">То есть тип запроса использовался для сопоставления типов сущностей без ключей (скорее всего, из представления, но, возможно, и из таблицы), а обычный тип сущности использовался при наличии ключа (скорее всего, из таблицы, но, возможно, и из представления).</span><span class="sxs-lookup"><span data-stu-id="d5515-230">That is, a query type was used for mapping entity types without keys (more likely from a view, but possibly from a table) while a regular entity type was used when a key was available (more likely from a table, but possibly from a view).</span></span>

<span data-ttu-id="d5515-231">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="d5515-231">**New behavior**</span></span>

<span data-ttu-id="d5515-232">Теперь тип запроса стал просто типом сущности без первичного ключа.</span><span class="sxs-lookup"><span data-stu-id="d5515-232">A query type now becomes just an entity type without a primary key.</span></span>
<span data-ttu-id="d5515-233">Типы сущностей без ключей имеют ту же функциональность, что типы запросов в предыдущих версиях.</span><span class="sxs-lookup"><span data-stu-id="d5515-233">Keyless entity types have the same functionality as query types in previous versions.</span></span>

<span data-ttu-id="d5515-234">**Причина**</span><span class="sxs-lookup"><span data-stu-id="d5515-234">**Why**</span></span>

<span data-ttu-id="d5515-235">Это изменение было внесено, чтобы избежать путаницы с назначением типов запросов.</span><span class="sxs-lookup"><span data-stu-id="d5515-235">This change was made to reduce the confusion around the purpose of query types.</span></span>
<span data-ttu-id="d5515-236">В частности, они представляют собой типы сущностей без ключей и поэтому по определению доступны только для чтения, однако их не следует использовать только потому, что тип сущности должен быть доступен только для чтения.</span><span class="sxs-lookup"><span data-stu-id="d5515-236">Specifically, they are keyless entity types and they are inherently read-only because of this, but they should not be used just because an entity type needs to be read-only.</span></span>
<span data-ttu-id="d5515-237">Аналогично, их часто сопоставляют с представлениями, но только потому, что представления часто не определяют ключи.</span><span class="sxs-lookup"><span data-stu-id="d5515-237">Likewise, they are often mapped to views, but this is only because views often don't define keys.</span></span>

<span data-ttu-id="d5515-238">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="d5515-238">**Mitigations**</span></span>

<span data-ttu-id="d5515-239">Следующие компоненты этого API теперь являются устаревшими:</span><span class="sxs-lookup"><span data-stu-id="d5515-239">The following parts of the API are now obsolete:</span></span>
* <span data-ttu-id="d5515-240">**`ModelBuilder.Query<>()`** — вместо него следует вызвать `ModelBuilder.Entity<>().HasNoKey()`, чтобы пометить тип сущности как не имеющий ключей.</span><span class="sxs-lookup"><span data-stu-id="d5515-240">**`ModelBuilder.Query<>()`** - Instead `ModelBuilder.Entity<>().HasNoKey()` needs to be called to mark an entity type as having no keys.</span></span>
<span data-ttu-id="d5515-241">Это по-прежнему не следует настраивать посредством соглашения, чтобы избежать ошибки, когда первичный ключ ожидается, но не соответствует соглашению.</span><span class="sxs-lookup"><span data-stu-id="d5515-241">This would still not be configured by convention to avoid misconfiguration when a primary key is expected, but doesn't match the convention.</span></span>
* <span data-ttu-id="d5515-242">**`DbQuery<>`** — вместо него следует использовать `DbSet<>`.</span><span class="sxs-lookup"><span data-stu-id="d5515-242">**`DbQuery<>`** - Instead `DbSet<>` should be used.</span></span>
* <span data-ttu-id="d5515-243">**`DbContext.Query<>()`** — вместо него следует использовать `DbContext.Set<>()`.</span><span class="sxs-lookup"><span data-stu-id="d5515-243">**`DbContext.Query<>()`** - Instead `DbContext.Set<>()` should be used.</span></span>

## <a name="configuration-api-for-owned-type-relationships-has-changed"></a><span data-ttu-id="d5515-244">Изменение API конфигурации для отношений принадлежащего типа</span><span class="sxs-lookup"><span data-stu-id="d5515-244">Configuration API for owned type relationships has changed</span></span>

<span data-ttu-id="d5515-245">[Отслеживание вопроса 12444](https://github.com/aspnet/EntityFrameworkCore/issues/12444)
[Отслеживание вопроса 9148](https://github.com/aspnet/EntityFrameworkCore/issues/9148)
[Отслеживание вопроса 14153](https://github.com/aspnet/EntityFrameworkCore/issues/14153)</span><span class="sxs-lookup"><span data-stu-id="d5515-245">[Tracking Issue #12444](https://github.com/aspnet/EntityFrameworkCore/issues/12444)
[Tracking Issue #9148](https://github.com/aspnet/EntityFrameworkCore/issues/9148)
[Tracking Issue #14153](https://github.com/aspnet/EntityFrameworkCore/issues/14153)</span></span>

<span data-ttu-id="d5515-246">Это изменение было внесено в предварительную версию 3 EF Core 3.0.</span><span class="sxs-lookup"><span data-stu-id="d5515-246">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="d5515-247">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="d5515-247">**Old behavior**</span></span>

<span data-ttu-id="d5515-248">До выхода EF Core 3.0 конфигурация принадлежащего отношения выполнялась непосредственно после вызова `OwnsOne` или `OwnsMany`.</span><span class="sxs-lookup"><span data-stu-id="d5515-248">Before EF Core 3.0, configuration of the owned relationship was performed directly after the `OwnsOne` or `OwnsMany` call.</span></span> 

<span data-ttu-id="d5515-249">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="d5515-249">**New behavior**</span></span>

<span data-ttu-id="d5515-250">Начиная с EF Core 3.0 существует текучий API для настройки свойства навигации для владельца с помощью `WithOwner()`.</span><span class="sxs-lookup"><span data-stu-id="d5515-250">Starting with EF Core 3.0, there is now fluent API to configure a navigation property to the owner using `WithOwner()`.</span></span>
<span data-ttu-id="d5515-251">Например:</span><span class="sxs-lookup"><span data-stu-id="d5515-251">For example:</span></span>

```C#
modelBuilder.Entity<Order>.OwnsOne(e => e.Details).WithOwner(e => e.Order);
```

<span data-ttu-id="d5515-252">Конфигурация, связанная с отношением между владельцем и принадлежащим объектом, теперь должна быть включена в цепочку после `WithOwner()` по аналогии с настройкой других отношений.</span><span class="sxs-lookup"><span data-stu-id="d5515-252">The configuration related to the relationship between owner and owned should now be chained after `WithOwner()` similarly to how other relationships are configured.</span></span>
<span data-ttu-id="d5515-253">При этом конфигурация для принадлежащего типа сама будет включена в цепочку после `OwnsOne()/OwnsMany()`.</span><span class="sxs-lookup"><span data-stu-id="d5515-253">While the configuration for the owned type itself would still be chained after `OwnsOne()/OwnsMany()`.</span></span>
<span data-ttu-id="d5515-254">Например:</span><span class="sxs-lookup"><span data-stu-id="d5515-254">For example:</span></span>

```C#
modelBuilder.Entity<Order>.OwnsOne(e => e.Details, eb =>
    {
        eb.WithOwner()
            .HasForeignKey(e => e.AlternateId)
            .HasConstraintName("FK_OrderDetails");
            
        eb.ToTable("OrderDetails");
        eb.HasKey(e => e.AlternateId);
        eb.HasIndex(e => e.Id);

        eb.HasOne(e => e.Customer).WithOne();

        eb.HasData(
            new OrderDetails
            {
                AlternateId = 1,
                Id = -1
            });
    });
```

<span data-ttu-id="d5515-255">Кроме того, вызов `Entity()`, `HasOne()` или `Set()` с целевым объектом принадлежащего типа теперь приведет к возникновению исключения.</span><span class="sxs-lookup"><span data-stu-id="d5515-255">Additionally calling `Entity()`, `HasOne()`, or `Set()` with an owned type target will now throw an exception.</span></span>

<span data-ttu-id="d5515-256">**Причина**</span><span class="sxs-lookup"><span data-stu-id="d5515-256">**Why**</span></span>

<span data-ttu-id="d5515-257">Это изменение было внесено для более четкого разграничения между настройкой самого принадлежащего типа и _отношения_ с ним.</span><span class="sxs-lookup"><span data-stu-id="d5515-257">This change was made to create a cleaner separation between configuring the owned type itself and the _relationship to_ the owned type.</span></span>
<span data-ttu-id="d5515-258">Это, в свою очередь, устраняет неоднозначность и путаницу при использовании таких методов, как `HasForeignKey`.</span><span class="sxs-lookup"><span data-stu-id="d5515-258">This in turn removes ambiguity and confusion around methods like `HasForeignKey`.</span></span>

<span data-ttu-id="d5515-259">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="d5515-259">**Mitigations**</span></span>

<span data-ttu-id="d5515-260">Измените конфигурацию отношений принадлежащего типа, чтобы использовать новую поверхность API, как показано в приведенном выше примере.</span><span class="sxs-lookup"><span data-stu-id="d5515-260">Change configuration of owned type relationships to use the new API surface as shown in the example above.</span></span>

## <a name="dependent-entities-sharing-the-table-with-the-principal-are-now-optional"></a><span data-ttu-id="d5515-261">Зависимые сущности, имеющие общую с субъектом таблицу, теперь являются необязательными</span><span class="sxs-lookup"><span data-stu-id="d5515-261">Dependent entities sharing the table with the principal are now optional</span></span>

[<span data-ttu-id="d5515-262">Отслеживание вопроса № 9005</span><span class="sxs-lookup"><span data-stu-id="d5515-262">Tracking Issue #9005</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9005)

<span data-ttu-id="d5515-263">Это изменение будет внесено в предварительную версию 4 EF Core 3.0.</span><span class="sxs-lookup"><span data-stu-id="d5515-263">This change will be introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="d5515-264">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="d5515-264">**Old behavior**</span></span>

<span data-ttu-id="d5515-265">Рассмотрим следующую модель:</span><span class="sxs-lookup"><span data-stu-id="d5515-265">Consider the following model:</span></span>
```C#
public class Order
{
    public int Id { get; set; }
    public int CustomerId { get; set; }
    public OrderDetails Details { get; set; }
}

public class OrderDetails
{
    public int Id { get; set; }
    public string ShippingAddress { get; set; }
}
```
<span data-ttu-id="d5515-266">До выхода EF Core 3.0, если класс `OrderDetails` принадлежал классу `Order` или явно сопоставлялся с этой же таблицей, при добавлении нового класса `Order` всегда требовался экземпляр `OrderDetails`.</span><span class="sxs-lookup"><span data-stu-id="d5515-266">Before EF Core 3.0, if `OrderDetails` is owned by `Order` or explicitly mapped to the same table then an `OrderDetails` instance was always required when adding a new `Order`.</span></span>


<span data-ttu-id="d5515-267">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="d5515-267">**New behavior**</span></span>

<span data-ttu-id="d5515-268">Начиная с версии 3.0 система EF Core позволяет добавлять класс `Order` без класса `OrderDetails` и сопоставляет все свойства `OrderDetails`, за исключением первичного ключа, со столбцами, допускающими значение NULL.</span><span class="sxs-lookup"><span data-stu-id="d5515-268">Starting with 3.0, EF Core allows to add an `Order` without an `OrderDetails` and maps all of the `OrderDetails` properties except the primary key to nullable columns.</span></span>
<span data-ttu-id="d5515-269">При отправке запроса EF Core задает классу `OrderDetails` значение `null`, если какому-либо его обязательному свойству не задано значение или если отсутствуют необходимые свойства помимо первичного ключа и все свойства имеют значение `null`.</span><span class="sxs-lookup"><span data-stu-id="d5515-269">When querying EF Core sets `OrderDetails` to `null` if any of its required properties doesn't have a value or if it has no required properties besides the primary key and all properties are `null`.</span></span>

<span data-ttu-id="d5515-270">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="d5515-270">**Mitigations**</span></span>

<span data-ttu-id="d5515-271">Если модель содержит общую таблицу со всеми необязательными столбцами, но указывающее на нее свойство навигации не должно иметь значение `null`, приложение следует изменить для обработки случаев, когда свойству навигации задано значение `null`.</span><span class="sxs-lookup"><span data-stu-id="d5515-271">If your model has a table sharing dependent with all optional columns, but the navigation pointing to it is not expected to be `null` then the application should be modified to handle cases when the navigation is `null`.</span></span> <span data-ttu-id="d5515-272">Если это невозможно, необходимо добавить обязательное свойство к типу сущности либо по меньшей мере одному свойству должно быть задано значение, отличное от `null`.</span><span class="sxs-lookup"><span data-stu-id="d5515-272">If this is not possible a required property should be added to the entity type or at least one property should have a non-`null` value assigned to it.</span></span>

## <a name="all-entities-sharing-a-table-with-a-concurrency-token-column-have-to-map-it-to-a-property"></a><span data-ttu-id="d5515-273">Все сущности, имеющие общую таблицу со столбцом маркера параллелизма, должны сопоставлять ее со свойством</span><span class="sxs-lookup"><span data-stu-id="d5515-273">All entities sharing a table with a concurrency token column have to map it to a property</span></span>

[<span data-ttu-id="d5515-274">Отслеживание вопроса № 14154</span><span class="sxs-lookup"><span data-stu-id="d5515-274">Tracking Issue #14154</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14154)

<span data-ttu-id="d5515-275">Это изменение будет внесено в предварительную версию 4 EF Core 3.0.</span><span class="sxs-lookup"><span data-stu-id="d5515-275">This change will be introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="d5515-276">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="d5515-276">**Old behavior**</span></span>

<span data-ttu-id="d5515-277">Рассмотрим следующую модель:</span><span class="sxs-lookup"><span data-stu-id="d5515-277">Consider the following model:</span></span>
```C#
public class Order
{
    public int Id { get; set; }
    public int CustomerId { get; set; }
    public byte[] Version { get; set; }
    public OrderDetails Details { get; set; }
}

public class OrderDetails
{
    public int Id { get; set; }
    public string ShippingAddress { get; set; }
}

protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<Order>()
        .Property(o => o.Version).IsRowVersion().HasColumnName("Version");
}
```
<span data-ttu-id="d5515-278">До выхода EF Core 3.0, если класс `OrderDetails` принадлежал классу `Order` или явно сопоставлялся с той же таблицей, то обновление только класса `OrderDetails` не приводило к обновлению значения `Version` на клиенте и следующее обновление завершалось ошибкой.</span><span class="sxs-lookup"><span data-stu-id="d5515-278">Before EF Core 3.0, if `OrderDetails` is owned by `Order` or explicitly mapped to the same table then updating just `OrderDetails` will not update `Version` value on client and the next update will fail.</span></span>


<span data-ttu-id="d5515-279">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="d5515-279">**New behavior**</span></span>

<span data-ttu-id="d5515-280">Начиная с версии 3.0 система EF Core распространяет новое значение `Version` для класса `Order`, если ему принадлежит класс `OrderDetails`.</span><span class="sxs-lookup"><span data-stu-id="d5515-280">Starting with 3.0, EF Core propagates the new `Version` value to `Order` if it owns `OrderDetails`.</span></span> <span data-ttu-id="d5515-281">В противном случае во время проверки модели создается исключение.</span><span class="sxs-lookup"><span data-stu-id="d5515-281">Otherwise an exception is thrown during model validation.</span></span>

<span data-ttu-id="d5515-282">**Причина**</span><span class="sxs-lookup"><span data-stu-id="d5515-282">**Why**</span></span>

<span data-ttu-id="d5515-283">Это изменение было внесено, чтобы избежать получения устаревшего значения маркера параллелизма при обновлении только одной сущности, сопоставленной с той же таблицей.</span><span class="sxs-lookup"><span data-stu-id="d5515-283">This change was made to avoid a stale concurrency token value when only one of the entities mapped to the same table is updated.</span></span>

<span data-ttu-id="d5515-284">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="d5515-284">**Mitigations**</span></span>

<span data-ttu-id="d5515-285">Все сущности, имеющие общую таблицу, должны включать в себя свойство, сопоставленное со столбцом маркера параллелизма.</span><span class="sxs-lookup"><span data-stu-id="d5515-285">All entities sharing the table have to include a property that is mapped to the concurrency token column.</span></span> <span data-ttu-id="d5515-286">Можно создать свойство в теневом состоянии:</span><span class="sxs-lookup"><span data-stu-id="d5515-286">It's possible the create one in shadow-state:</span></span>
```C#
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<OrderDetails>()
        .Property<byte[]>("Version").IsRowVersion().HasColumnName("Version");
}
```

## <a name="inherited-properties-from-unmapped-types-are-now-mapped-to-a-single-column-for-all-derived-types"></a><span data-ttu-id="d5515-287">Наследуемые свойства из несопоставленных типов теперь сопоставляются с одним столбцом для всех производных типов</span><span class="sxs-lookup"><span data-stu-id="d5515-287">Inherited properties from unmapped types are now mapped to a single column for all derived types</span></span>

[<span data-ttu-id="d5515-288">Отслеживание вопроса № 13998</span><span class="sxs-lookup"><span data-stu-id="d5515-288">Tracking Issue #13998</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13998)

<span data-ttu-id="d5515-289">Это изменение будет внесено в предварительную версию 4 EF Core 3.0.</span><span class="sxs-lookup"><span data-stu-id="d5515-289">This change will be introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="d5515-290">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="d5515-290">**Old behavior**</span></span>

<span data-ttu-id="d5515-291">Рассмотрим следующую модель:</span><span class="sxs-lookup"><span data-stu-id="d5515-291">Consider the following model:</span></span>
```C#
public abstract class EntityBase
{
    public int Id { get; set; }
}

public abstract class OrderBase : EntityBase
{
    public int ShippingAddress { get; set; }
}

public class BulkOrder : OrderBase
{
}

public class Order : OrderBase
{
}

protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Ignore<OrderBase>();
    modelBuilder.Entity<EntityBase>();
    modelBuilder.Entity<BulkOrder>();
    modelBuilder.Entity<Order>();
}
```

<span data-ttu-id="d5515-292">До выхода EF Core 3.0 свойство `ShippingAddress` сопоставлялось с отдельными столбцами для классов `BulkOrder` и `Order` по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="d5515-292">Before EF Core 3.0, the `ShippingAddress` property would be mapped to separate columns for `BulkOrder` and `Order` by default.</span></span>

<span data-ttu-id="d5515-293">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="d5515-293">**New behavior**</span></span>

<span data-ttu-id="d5515-294">Начиная с версии 3.0 система EF Core создает только один столбец для класса `ShippingAddress`.</span><span class="sxs-lookup"><span data-stu-id="d5515-294">Starting with 3.0, EF Core only creates one column for `ShippingAddress`.</span></span>

<span data-ttu-id="d5515-295">**Причина**</span><span class="sxs-lookup"><span data-stu-id="d5515-295">**Why**</span></span>

<span data-ttu-id="d5515-296">Старое поведение было непредвиденным.</span><span class="sxs-lookup"><span data-stu-id="d5515-296">The old behavoir was unexpected.</span></span>

<span data-ttu-id="d5515-297">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="d5515-297">**Mitigations**</span></span>

<span data-ttu-id="d5515-298">Свойство можно по-прежнему явным образом сопоставлять с отдельным столбцом для производных типов.</span><span class="sxs-lookup"><span data-stu-id="d5515-298">The property can still be explicitly mapped to separate column on the derived types:</span></span>

```C#
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Ignore<OrderBase>();
    modelBuilder.Entity<EntityBase>();
    modelBuilder.Entity<BulkOrder>()
        .Property(o => o.ShippingAddress).HasColumnName("BulkShippingAddress");
    modelBuilder.Entity<Order>()
        .Property(o => o.ShippingAddress).HasColumnName("ShippingAddress");
}
```

## <a name="the-foreign-key-property-convention-no-longer-matches-same-name-as-the-principal-property"></a><span data-ttu-id="d5515-299">Соглашение для свойства внешнего ключа больше не совпадает со свойством субъекта по имени</span><span class="sxs-lookup"><span data-stu-id="d5515-299">The foreign key property convention no longer matches same name as the principal property</span></span>

[<span data-ttu-id="d5515-300">Отслеживание вопроса 13274</span><span class="sxs-lookup"><span data-stu-id="d5515-300">Tracking Issue #13274</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13274)

<span data-ttu-id="d5515-301">Это изменение было внесено в предварительную версию 3 EF Core 3.0.</span><span class="sxs-lookup"><span data-stu-id="d5515-301">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="d5515-302">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="d5515-302">**Old behavior**</span></span>

<span data-ttu-id="d5515-303">Рассмотрим следующую модель:</span><span class="sxs-lookup"><span data-stu-id="d5515-303">Consider the following model:</span></span>
```C#
public class Customer
{
    public int CustomerId { get; set; }
    public ICollection<Order> Orders { get; set; }
}

public class Order
{
    public int Id { get; set; }
    public int CustomerId { get; set; }
}
```
<span data-ttu-id="d5515-304">До выхода EF Core 3.0 свойство `CustomerId` использовалось для внешнего ключа по соглашению.</span><span class="sxs-lookup"><span data-stu-id="d5515-304">Before EF Core 3.0, the `CustomerId` property would be used for the foreign key by convention.</span></span>
<span data-ttu-id="d5515-305">Однако если `Order` является принадлежащим типом, это также делает `CustomerId` первичным ключом, что обычно не соответствует ожидаемому результату.</span><span class="sxs-lookup"><span data-stu-id="d5515-305">However, if `Order` is an owned type, then this would also make `CustomerId` the primary key and this isn't usually the expectation.</span></span>

<span data-ttu-id="d5515-306">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="d5515-306">**New behavior**</span></span>

<span data-ttu-id="d5515-307">Начиная с версии 3.0 система EF Core не будет пытаться использовать свойства для внешних ключей по соглашению, если они имеют такое же имя, что и свойство субъекта.</span><span class="sxs-lookup"><span data-stu-id="d5515-307">Starting with 3.0, EF Core doesn't try to use properties for foreign keys by convention if they have the same name as the principal property.</span></span>
<span data-ttu-id="d5515-308">Шаблоны имени типа субъекта, сцепленного с именем свойства субъекта, и имени навигации, сцепленного с именем свойства субъекта, по-прежнему совпадают.</span><span class="sxs-lookup"><span data-stu-id="d5515-308">Principal type name concatenated with principal property name, and navigation name concatenated with principal property name patterns are still matched.</span></span>
<span data-ttu-id="d5515-309">Например:</span><span class="sxs-lookup"><span data-stu-id="d5515-309">For example:</span></span>

```C#
public class Customer
{
    public int Id { get; set; }
    public ICollection<Order> Orders { get; set; }
}

public class Order
{
    public int Id { get; set; }
    public int CustomerId { get; set; }
}
```

```C#
public class Customer
{
    public int Id { get; set; }
    public ICollection<Order> Orders { get; set; }
}

public class Order
{
    public int Id { get; set; }
    public int BuyerId { get; set; }
    public Customer Buyer { get; set; }
}
```

<span data-ttu-id="d5515-310">**Причина**</span><span class="sxs-lookup"><span data-stu-id="d5515-310">**Why**</span></span>

<span data-ttu-id="d5515-311">Это изменение было внесено во избежание ошибочного определения свойств первичного ключа для принадлежащего типа.</span><span class="sxs-lookup"><span data-stu-id="d5515-311">This change was made to avoid erroneously defining a primary key property on the owned type.</span></span>

<span data-ttu-id="d5515-312">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="d5515-312">**Mitigations**</span></span>

<span data-ttu-id="d5515-313">Если свойство должно быть внешним ключом и, следовательно, частью первичного ключа, следует явно указать это при его настройке.</span><span class="sxs-lookup"><span data-stu-id="d5515-313">If the property was intended to be the foreign key, and hence part of the primary key, then explicitly configure it as such.</span></span>

## <a name="database-connection-is-now-closed-if-not-used-anymore-before-the-transactionscope-has-been-completed"></a><span data-ttu-id="d5515-314">Соединение с базой данных теперь закрывается, если оно больше не используется до завершения TransactionScope</span><span class="sxs-lookup"><span data-stu-id="d5515-314">Database connection is now closed if not used anymore before the TransactionScope has been completed</span></span>

[<span data-ttu-id="d5515-315">Отслеживание вопроса № 14218</span><span class="sxs-lookup"><span data-stu-id="d5515-315">Tracking Issue #14218</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14218)

<span data-ttu-id="d5515-316">Это изменение будет внесено в предварительную версию 4 EF Core 3.0.</span><span class="sxs-lookup"><span data-stu-id="d5515-316">This change will be introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="d5515-317">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="d5515-317">**Old behavior**</span></span>

<span data-ttu-id="d5515-318">До выхода EF Core 3.0, если контекст открывает соединение внутри класса `TransactionScope`, оно остается открытым пока активен текущий класс `TransactionScope`.</span><span class="sxs-lookup"><span data-stu-id="d5515-318">Before EF Core 3.0, if the context opens the connection inside a `TransactionScope`, the connection remains open while the current `TransactionScope` is active.</span></span>

```C#
using (new TransactionScope())
{
    using (AdventureWorks context = new AdventureWorks())
    {
        context.ProductCategories.Add(new ProductCategory());
        context.SaveChanges();

        // Old behavior: Connection is still open at this point
        
        var categories = context.ProductCategories().ToList();
    }
}
```

<span data-ttu-id="d5515-319">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="d5515-319">**New behavior**</span></span>

<span data-ttu-id="d5515-320">Начиная с версии 3.0 система EF Core закрывает соединение сразу же после прекращения его использования.</span><span class="sxs-lookup"><span data-stu-id="d5515-320">Starting with 3.0, EF Core closes the connection as soon as it's done using it.</span></span>

<span data-ttu-id="d5515-321">**Причина**</span><span class="sxs-lookup"><span data-stu-id="d5515-321">**Why**</span></span>

<span data-ttu-id="d5515-322">Это изменение позволяет использовать несколько контекстов в одном классе `TransactionScope`.</span><span class="sxs-lookup"><span data-stu-id="d5515-322">This change allows to use multiple contexts in the same `TransactionScope`.</span></span> <span data-ttu-id="d5515-323">Новое поведение также соответствует EF6.</span><span class="sxs-lookup"><span data-stu-id="d5515-323">The new behavior alose matches EF6.</span></span>

<span data-ttu-id="d5515-324">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="d5515-324">**Mitigations**</span></span>

<span data-ttu-id="d5515-325">Если соединение должно оставаться открытым, явный вызов метода `OpenConnection()` гарантирует, что EF Core не закроет его преждевременно.</span><span class="sxs-lookup"><span data-stu-id="d5515-325">If the connection needs to remain open explicit call to `OpenConnection()` will ensure that EF Core doesn't close it prematurely:</span></span>

```C#
using (new TransactionScope())
{
    using (AdventureWorks context = new AdventureWorks())
    {
        context.Database.OpenConnection();
        context.ProductCategories.Add(new ProductCategory());
        context.SaveChanges();
        
        var categories = context.ProductCategories().ToList();
        context.Database.CloseConnection();
    }
}
```

## <a name="each-property-uses-independent-in-memory-integer-key-generation"></a><span data-ttu-id="d5515-326">Каждое свойство использует создание независимых целочисленных ключей в памяти</span><span class="sxs-lookup"><span data-stu-id="d5515-326">Each property uses independent in-memory integer key generation</span></span>

[<span data-ttu-id="d5515-327">Отслеживание вопроса 6872</span><span class="sxs-lookup"><span data-stu-id="d5515-327">Tracking Issue #6872</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/6872)

<span data-ttu-id="d5515-328">Это изменение будет внесено в предварительную версию 4 EF Core 3.0.</span><span class="sxs-lookup"><span data-stu-id="d5515-328">This change will be introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="d5515-329">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="d5515-329">**Old behavior**</span></span>

<span data-ttu-id="d5515-330">До выхода EF Core 3.0 для всех свойств целочисленных ключей в памяти использовался один общий генератор значений.</span><span class="sxs-lookup"><span data-stu-id="d5515-330">Before EF Core 3.0, one shared value generator was used for all in-memory integer key properties.</span></span>

<span data-ttu-id="d5515-331">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="d5515-331">**New behavior**</span></span>

<span data-ttu-id="d5515-332">Начиная с EF Core 3.0 при использовании выполняющейся в памяти базы данных каждое свойство целочисленного ключа получает свой собственный генератор значений.</span><span class="sxs-lookup"><span data-stu-id="d5515-332">Starting with EF Core 3.0, each integer key property gets its own value generator when using the in-memory database.</span></span>
<span data-ttu-id="d5515-333">Кроме того, при удалении базы данных формирование ключа сбрасывается для всех таблиц.</span><span class="sxs-lookup"><span data-stu-id="d5515-333">Also, if the database is deleted, then key generation is reset for all tables.</span></span>

<span data-ttu-id="d5515-334">**Причина**</span><span class="sxs-lookup"><span data-stu-id="d5515-334">**Why**</span></span>

<span data-ttu-id="d5515-335">Это изменение было внесено, чтобы лучше согласовать формирование ключей в памяти с реальным процессом в базе данных, а также улучшить возможность изоляции тестов друг от друга при использовании выполняющейся в памяти базы данных.</span><span class="sxs-lookup"><span data-stu-id="d5515-335">This change was made to align in-memory key generation more closely to real database key generation and to improve the ability to isolate tests from each other when using the in-memory database.</span></span>

<span data-ttu-id="d5515-336">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="d5515-336">**Mitigations**</span></span>

<span data-ttu-id="d5515-337">Это изменение может нарушить работу приложения, полагающегося на задание определенных значений ключа в памяти.</span><span class="sxs-lookup"><span data-stu-id="d5515-337">This can break an application that is relying on specific in-memory key values to be set.</span></span>
<span data-ttu-id="d5515-338">Рекомендуется не полагаться на конкретные значения ключей либо реализовать новое поведение.</span><span class="sxs-lookup"><span data-stu-id="d5515-338">Consider instead not relying on specific key values, or updating to match the new behavior.</span></span>

## <a name="backing-fields-are-used-by-default"></a><span data-ttu-id="d5515-339">По умолчанию используются резервные поля</span><span class="sxs-lookup"><span data-stu-id="d5515-339">Backing fields are used by default</span></span>

[<span data-ttu-id="d5515-340">Отслеживание вопроса 12430</span><span class="sxs-lookup"><span data-stu-id="d5515-340">Tracking Issue #12430</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12430)

<span data-ttu-id="d5515-341">Это изменение было внесено в предварительную версию 2 EF Core 3.0.</span><span class="sxs-lookup"><span data-stu-id="d5515-341">This change was introduced in EF Core 3.0-preview 2.</span></span>

<span data-ttu-id="d5515-342">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="d5515-342">**Old behavior**</span></span>

<span data-ttu-id="d5515-343">До выхода версии 3.0, даже если резервное поле для свойства было известно, система EF Core все равно по умолчанию считывала и записывала значение свойства с помощью методов получения и задания свойства.</span><span class="sxs-lookup"><span data-stu-id="d5515-343">Before 3.0, even if the backing field for a property was known, EF Core would still by default read and write the property value using the property getter and setter methods.</span></span>
<span data-ttu-id="d5515-344">Исключением из этого было выполнение запроса, где известное резервное поле устанавливается напрямую.</span><span class="sxs-lookup"><span data-stu-id="d5515-344">The exception to this was query execution, where the backing field would be set directly if known.</span></span>

<span data-ttu-id="d5515-345">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="d5515-345">**New behavior**</span></span>

<span data-ttu-id="d5515-346">Начиная с EF Core 3.0, если резервное поле для свойства известно, то чтение и запись этого свойства всегда осуществляются с его помощью.</span><span class="sxs-lookup"><span data-stu-id="d5515-346">Starting with EF Core 3.0, if the backing field for a property is known, then will always read and write that property using the backing field.</span></span>
<span data-ttu-id="d5515-347">Это может нарушить работу приложения, если оно полагается на дополнительное поведение, закодированное в методы получения и задания.</span><span class="sxs-lookup"><span data-stu-id="d5515-347">This could cause an application break if the application is relying on additional behavior coded into the getter or setter methods.</span></span>

<span data-ttu-id="d5515-348">**Причина**</span><span class="sxs-lookup"><span data-stu-id="d5515-348">**Why**</span></span>

<span data-ttu-id="d5515-349">Это изменение было внесено, чтобы не позволить EF Core ошибочно активировать бизнес-логику по умолчанию при выполнении операций базы данных, затрагивающих сущности.</span><span class="sxs-lookup"><span data-stu-id="d5515-349">This change was made to prevent EF Core from erroneously triggering business logic by default when performing database operations involving the entities.</span></span>

<span data-ttu-id="d5515-350">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="d5515-350">**Mitigations**</span></span>

<span data-ttu-id="d5515-351">Поведение, предшествовавшее выходу версии 3.0, можно восстановить, настроив режим доступа к свойству в текучем API modelBuilder.</span><span class="sxs-lookup"><span data-stu-id="d5515-351">The pre-3.0 behavior can be restored through configuration of the property access mode in the modelBuilder fluent API.</span></span>
<span data-ttu-id="d5515-352">Например:</span><span class="sxs-lookup"><span data-stu-id="d5515-352">For example:</span></span>

```C#
modelBuilder.UsePropertyAccessMode(PropertyAccessMode.PreferFieldDuringConstruction);
```

## <a name="throw-if-multiple-compatible-backing-fields-are-found"></a><span data-ttu-id="d5515-353">Исключение при обнаружении нескольких совместимых резервных полей</span><span class="sxs-lookup"><span data-stu-id="d5515-353">Throw if multiple compatible backing fields are found</span></span>

[<span data-ttu-id="d5515-354">Отслеживание вопроса 12523</span><span class="sxs-lookup"><span data-stu-id="d5515-354">Tracking Issue #12523</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12523)

<span data-ttu-id="d5515-355">Это изменение будет внесено в предварительную версию 4 EF Core 3.0.</span><span class="sxs-lookup"><span data-stu-id="d5515-355">This change will be introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="d5515-356">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="d5515-356">**Old behavior**</span></span>

<span data-ttu-id="d5515-357">До выхода EF Core 3.0, если правила, обеспечивающие поиск резервного поля для свойства, находили несколько полей, выбиралось одно из них на основе определенной очередности.</span><span class="sxs-lookup"><span data-stu-id="d5515-357">Before EF Core 3.0, if multiple fields matched the rules for finding the backing field of a property, then one field would be chosen based on some precedence order.</span></span>
<span data-ttu-id="d5515-358">В неоднозначных случаях это могло привести к использованию неправильного поля.</span><span class="sxs-lookup"><span data-stu-id="d5515-358">This could cause the wrong field to be used in ambiguous cases.</span></span>

<span data-ttu-id="d5515-359">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="d5515-359">**New behavior**</span></span>

<span data-ttu-id="d5515-360">Начиная с EF Core 3.0 при обнаружении нескольких полей для одного свойства возникает исключение.</span><span class="sxs-lookup"><span data-stu-id="d5515-360">Starting with EF Core 3.0, if multiple fields are matched to the same property, then an exception is thrown.</span></span>

<span data-ttu-id="d5515-361">**Причина**</span><span class="sxs-lookup"><span data-stu-id="d5515-361">**Why**</span></span>

<span data-ttu-id="d5515-362">Это изменение было внесено во избежание автоматического использования одного поля вместо другого, когда правильным может быть только одно из них.</span><span class="sxs-lookup"><span data-stu-id="d5515-362">This change was made to avoid silently using one field over another when only one can be correct.</span></span>

<span data-ttu-id="d5515-363">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="d5515-363">**Mitigations**</span></span>

<span data-ttu-id="d5515-364">Для свойств с неоднозначными резервными полями используемое поле должно быть задано явным образом.</span><span class="sxs-lookup"><span data-stu-id="d5515-364">Properties with ambiguous backing fields must have the field to use specified explicitly.</span></span>
<span data-ttu-id="d5515-365">Например, с помощью текучего API:</span><span class="sxs-lookup"><span data-stu-id="d5515-365">For example, using the fluent API:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .HasField("_id");
```

## <a name="field-only-property-names-should-match-the-field-name"></a><span data-ttu-id="d5515-366">Имена свойств, доступных только для полей, должны совпадать с именем поля.</span><span class="sxs-lookup"><span data-stu-id="d5515-366">Field-only property names should match the field name</span></span>

<span data-ttu-id="d5515-367">Это изменение будет внесено в предварительную версию 4 EF Core 3.0.</span><span class="sxs-lookup"><span data-stu-id="d5515-367">This change will be introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="d5515-368">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="d5515-368">**Old behavior**</span></span>

<span data-ttu-id="d5515-369">До EF Core 3.0 свойство могло быть указано с помощью строкового значения, и если свойство с таким именем не было найдено в типе CLR, компонент EF Core пытался сопоставить его с полем, используя правила соглашения.</span><span class="sxs-lookup"><span data-stu-id="d5515-369">Before EF Core 3.0, a property could be specified by a string value and if no property with that name was found on the CLR type then EF Core would try to match it to a field using convetion rules.</span></span>
```C#
private class Blog
{
    private int _id;
    public string Name { get; set; }
}
```
```C#
modelBuilder
    .Entity<Blog>()
    .Property("Id");
```

<span data-ttu-id="d5515-370">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="d5515-370">**New behavior**</span></span>

<span data-ttu-id="d5515-371">Начиная с EF Core 3.0 имя свойства, доступного только для полей, должно точно соответствовать имени поля.</span><span class="sxs-lookup"><span data-stu-id="d5515-371">Starting with EF Core 3.0, a field-only property must match the field name exactly.</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property("_id");
```

<span data-ttu-id="d5515-372">**Причина**</span><span class="sxs-lookup"><span data-stu-id="d5515-372">**Why**</span></span>

<span data-ttu-id="d5515-373">Это изменение было внесено, чтобы избежать использования одного поля для двух свойств с похожими именами, кроме того, правила сопоставления для свойств, доступных только для полей, теперь соответствуют правилам для свойств, сопоставленных со свойствами CLR.</span><span class="sxs-lookup"><span data-stu-id="d5515-373">This change was made to avoid using the same field for two properties named similarly, it also makes the matching rules for field-only properties the same as for properties mapped to CLR properties.</span></span>

<span data-ttu-id="d5515-374">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="d5515-374">**Mitigations**</span></span>

<span data-ttu-id="d5515-375">Имена свойств, доступных только для полей, должны совпадать с именем поля, с которым они сопоставляются.</span><span class="sxs-lookup"><span data-stu-id="d5515-375">Field-only properties must be named the same as the field they are mapped to.</span></span>
<span data-ttu-id="d5515-376">В последующей предварительной версии EF Core 3.0 мы планируем снова включить явную настройку имени поля, которое отличается от имени свойства:</span><span class="sxs-lookup"><span data-stu-id="d5515-376">In a later preview of EF Core 3.0, we plan to re-enable explicitly configuring a field name that is different from the property name:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property("Id")
    .HasField("_id");
```

## <a name="adddbcontextadddbcontextpool-no-longer-call-addlogging-and-addmemorycache"></a><span data-ttu-id="d5515-377">AddDbContext/AddDbContextPool больше не вызывает метод AddLogging и AddMemoryCache</span><span class="sxs-lookup"><span data-stu-id="d5515-377">AddDbContext/AddDbContextPool no longer call AddLogging and AddMemoryCache</span></span>

[<span data-ttu-id="d5515-378">Отслеживание вопроса № 14756</span><span class="sxs-lookup"><span data-stu-id="d5515-378">Tracking Issue #14756</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14756)

<span data-ttu-id="d5515-379">Это изменение будет внесено в предварительную версию 4 EF Core 3.0.</span><span class="sxs-lookup"><span data-stu-id="d5515-379">This change will be introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="d5515-380">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="d5515-380">**Old behavior**</span></span>

<span data-ttu-id="d5515-381">До EF Core 3.0 вызов метода `AddDbContext` или `AddDbContextPool` также приводил к регистрации служб ведения журналов и кэширования памяти с внедрением зависимостей с помощью вызовов к методам [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) и [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span><span class="sxs-lookup"><span data-stu-id="d5515-381">Before EF Core 3.0, calling `AddDbContext` or `AddDbContextPool` would also register logging and memory caching services with D.I through calls to [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) and [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span></span>

<span data-ttu-id="d5515-382">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="d5515-382">**New behavior**</span></span>

<span data-ttu-id="d5515-383">Начиная с версии EF Core 3.0 методы `AddDbContext` и `AddDbContextPool` больше не регистрируют такие службы с внедрением зависимостей.</span><span class="sxs-lookup"><span data-stu-id="d5515-383">Starting with EF Core 3.0, `AddDbContext` and `AddDbContextPool` will no longer register these services with Dependency Injection (DI).</span></span>

<span data-ttu-id="d5515-384">**Причина**</span><span class="sxs-lookup"><span data-stu-id="d5515-384">**Why**</span></span>

<span data-ttu-id="d5515-385">EF Core 3.0 не требует, чтобы такие службы присутствовали в контейнере внедрения зависимостей приложения.</span><span class="sxs-lookup"><span data-stu-id="d5515-385">EF Core 3.0 does not require that these services are in the application's DI cotainer.</span></span> <span data-ttu-id="d5515-386">Но если интерфейс `ILoggerFactory` зарегистрирован в контейнере внедрения зависимостей приложения, он будет по-прежнему использоваться EF Core.</span><span class="sxs-lookup"><span data-stu-id="d5515-386">However, if `ILoggerFactory` is registered in the application's DI container, then it will still be used by EF Core.</span></span>

<span data-ttu-id="d5515-387">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="d5515-387">**Mitigations**</span></span>

<span data-ttu-id="d5515-388">Если для работы вашего приложения нужны такие службы, явно зарегистрируйте их в контейнере внедрения зависимостей с помощью метода [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) или [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span><span class="sxs-lookup"><span data-stu-id="d5515-388">If your application needs these services, then register them explicitly with the DI container using  [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) or [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span></span>

## <a name="dbcontextentry-now-performs-a-local-detectchanges"></a><span data-ttu-id="d5515-389">DbContext.Entry теперь выполняет локальную процедуру DetectChanges</span><span class="sxs-lookup"><span data-stu-id="d5515-389">DbContext.Entry now performs a local DetectChanges</span></span>

[<span data-ttu-id="d5515-390">Отслеживание вопроса 13552</span><span class="sxs-lookup"><span data-stu-id="d5515-390">Tracking Issue #13552</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13552)

<span data-ttu-id="d5515-391">Это изменение было внесено в предварительную версию 3 EF Core 3.0.</span><span class="sxs-lookup"><span data-stu-id="d5515-391">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="d5515-392">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="d5515-392">**Old behavior**</span></span>

<span data-ttu-id="d5515-393">До выхода EF Core 3.0 вызов `DbContext.Entry` приводил к обнаружению изменений для всех отслеживаемых сущностей.</span><span class="sxs-lookup"><span data-stu-id="d5515-393">Before EF Core 3.0, calling `DbContext.Entry` would cause changes to be detected for all tracked entities.</span></span>
<span data-ttu-id="d5515-394">Это гарантировало, что состояние, представленное в `EntityEntry`, было актуальным.</span><span class="sxs-lookup"><span data-stu-id="d5515-394">This ensured that the state exposed in the `EntityEntry` was up-to-date.</span></span>

<span data-ttu-id="d5515-395">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="d5515-395">**New behavior**</span></span>

<span data-ttu-id="d5515-396">Начиная с EF Core 3.0 при вызове `DbContext.Entry` будет предприниматься попытка обнаружить изменения только в заданной сущности и всех связанных с ней отслеживаемых сущностях субъектов.</span><span class="sxs-lookup"><span data-stu-id="d5515-396">Starting with EF Core 3.0, calling `DbContext.Entry` will now only attempt to detect changes in the given entity and any tracked principal entities related to it.</span></span>
<span data-ttu-id="d5515-397">Это означает, что изменения в другом месте могут не обнаруживаться при вызове этого метода, что может негативно повлиять на состояние приложения.</span><span class="sxs-lookup"><span data-stu-id="d5515-397">This means that changes elsewhere may not have been detected by calling this method, which could have implications on application state.</span></span>

<span data-ttu-id="d5515-398">Обратите внимание, что если для `ChangeTracker.AutoDetectChangesEnabled` задано значение `false`, то будет отключено даже такое локальное обнаружение изменений.</span><span class="sxs-lookup"><span data-stu-id="d5515-398">Note that if `ChangeTracker.AutoDetectChangesEnabled` is set to `false` then even this local change detection will be disabled.</span></span>

<span data-ttu-id="d5515-399">Другие методы, вызывающие обнаружение изменений, например `ChangeTracker.Entries` и `SaveChanges`, по-прежнему подразумевают полную процедуру `DetectChanges` для всех отслеживаемых сущностей.</span><span class="sxs-lookup"><span data-stu-id="d5515-399">Other methods that cause change detection--for example `ChangeTracker.Entries` and `SaveChanges`--still cause a full `DetectChanges` of all tracked entities.</span></span>

<span data-ttu-id="d5515-400">**Причина**</span><span class="sxs-lookup"><span data-stu-id="d5515-400">**Why**</span></span>

<span data-ttu-id="d5515-401">Это изменение было внесено для повышения производительности по умолчанию при использовании `context.Entry`.</span><span class="sxs-lookup"><span data-stu-id="d5515-401">This change was made to improve the default performance of using `context.Entry`.</span></span>

<span data-ttu-id="d5515-402">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="d5515-402">**Mitigations**</span></span>

<span data-ttu-id="d5515-403">Вызовите `ChgangeTracker.DetectChanges()` явным образом перед вызовом `Entry`, чтобы обеспечить поведение, предшествовавшее выходу версии 3.0.</span><span class="sxs-lookup"><span data-stu-id="d5515-403">Call `ChgangeTracker.DetectChanges()` explicitly before calling `Entry` to ensure the pre-3.0 behavior.</span></span>

## <a name="string-and-byte-array-keys-are-not-client-generated-by-default"></a><span data-ttu-id="d5515-404">Ключи массива строк и байтов не формируются клиентом по умолчанию</span><span class="sxs-lookup"><span data-stu-id="d5515-404">String and byte array keys are not client-generated by default</span></span>

[<span data-ttu-id="d5515-405">Отслеживание вопроса 14617</span><span class="sxs-lookup"><span data-stu-id="d5515-405">Tracking Issue #14617</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14617)

<span data-ttu-id="d5515-406">Это изменение будет внесено в предварительную версию 4 EF Core 3.0.</span><span class="sxs-lookup"><span data-stu-id="d5515-406">This change will be introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="d5515-407">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="d5515-407">**Old behavior**</span></span>

<span data-ttu-id="d5515-408">До выхода EF Core 3.0 свойства ключей `string` и `byte[]` можно было использовать без явного указания значения, отличного от NULL.</span><span class="sxs-lookup"><span data-stu-id="d5515-408">Before EF Core 3.0, `string` and `byte[]` key properties could be used without explicitly setting a non-null value.</span></span>
<span data-ttu-id="d5515-409">В этом случае значение ключа создается на клиенте в виде GUID, сериализованного до байтов для `byte[]`.</span><span class="sxs-lookup"><span data-stu-id="d5515-409">In such a case, the key value would be generated on the client as a GUID, serialized to bytes for `byte[]`.</span></span>

<span data-ttu-id="d5515-410">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="d5515-410">**New behavior**</span></span>

<span data-ttu-id="d5515-411">Начиная с EF Core 3.0 возникает исключение, указывающее на то, что значение ключа не задано.</span><span class="sxs-lookup"><span data-stu-id="d5515-411">Starting with EF Core 3.0 an exception will be thrown indicating that no key value has been set.</span></span>

<span data-ttu-id="d5515-412">**Причина**</span><span class="sxs-lookup"><span data-stu-id="d5515-412">**Why**</span></span>

<span data-ttu-id="d5515-413">Это изменение было внесено, так как формируемые клиентом значения `string`/`byte[]` обычно бесполезны, а поведение по умолчанию затрудняло типовое планирование использования этих значений ключей.</span><span class="sxs-lookup"><span data-stu-id="d5515-413">This change was made because client-generated `string`/`byte[]` values generally aren't useful, and the default behavior made it hard to reason about generated key values in a common way.</span></span>

<span data-ttu-id="d5515-414">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="d5515-414">**Mitigations**</span></span>

<span data-ttu-id="d5515-415">Поведение, предшествовавшее выходу версии 3.0, можно получить, явно указав, что свойства ключей должны использовать формируемые значения, если не задано никакое другое значение, отличное от NULL.</span><span class="sxs-lookup"><span data-stu-id="d5515-415">The pre-3.0 behavior can be obtained by explicitly specifying that the key properties should use generated values if no other non-null value is set.</span></span>
<span data-ttu-id="d5515-416">Например, с помощью текучего API:</span><span class="sxs-lookup"><span data-stu-id="d5515-416">For example, with the fluent API:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .ValueGeneratedOnAdd();
```

<span data-ttu-id="d5515-417">Или с помощью заметок к данным:</span><span class="sxs-lookup"><span data-stu-id="d5515-417">Or with data annotations:</span></span>

```C#
[DatabaseGenerated(DatabaseGeneratedOption.Identity)]
public string Id { get; set; }
```

## <a name="iloggerfactory-is-now-a-scoped-service"></a><span data-ttu-id="d5515-418">ILoggerFactory теперь является службой с ограниченной областью действия</span><span class="sxs-lookup"><span data-stu-id="d5515-418">ILoggerFactory is now a scoped service</span></span>

[<span data-ttu-id="d5515-419">Отслеживание вопроса 14698</span><span class="sxs-lookup"><span data-stu-id="d5515-419">Tracking Issue #14698</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14698)

<span data-ttu-id="d5515-420">Это изменение было внесено в предварительную версию 3 EF Core 3.0.</span><span class="sxs-lookup"><span data-stu-id="d5515-420">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="d5515-421">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="d5515-421">**Old behavior**</span></span>

<span data-ttu-id="d5515-422">До выхода EF Core 3.0 `ILoggerFactory` регистрировалась в качестве отдельной службы.</span><span class="sxs-lookup"><span data-stu-id="d5515-422">Before EF Core 3.0, `ILoggerFactory` was registered as a singleton service.</span></span>

<span data-ttu-id="d5515-423">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="d5515-423">**New behavior**</span></span>

<span data-ttu-id="d5515-424">Начиная с EF Core 3.0 `ILoggerFactory` регистрируется в качестве службы с ограниченной областью действия.</span><span class="sxs-lookup"><span data-stu-id="d5515-424">Starting with EF Core 3.0, `ILoggerFactory` is now registered as scoped.</span></span>

<span data-ttu-id="d5515-425">**Причина**</span><span class="sxs-lookup"><span data-stu-id="d5515-425">**Why**</span></span>

<span data-ttu-id="d5515-426">Это изменение было внесено, чтобы разрешить сопоставление средства ведения журнала с экземпляром `DbContext`, что обеспечивает другие функции и устраняет некоторые патологические варианты поведения, такие как взрывной рост внутренних поставщиков служб.</span><span class="sxs-lookup"><span data-stu-id="d5515-426">This change was made to allow association of a logger with a `DbContext` instance, which enables other functionality and removes some cases of pathological behavior such as an explosion of internal service providers.</span></span>

<span data-ttu-id="d5515-427">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="d5515-427">**Mitigations**</span></span>

<span data-ttu-id="d5515-428">Это изменение не должно затрагивать код приложения, если только он не регистрирует и использует настраиваемые службы во внутреннем поставщике служб EF Core.</span><span class="sxs-lookup"><span data-stu-id="d5515-428">This change should not impact application code unless it is registering and using custom services on the EF Core internal service provider.</span></span>
<span data-ttu-id="d5515-429">Это встречается довольно редко.</span><span class="sxs-lookup"><span data-stu-id="d5515-429">This isn't common.</span></span>
<span data-ttu-id="d5515-430">В таких случаях большинство возможностей продолжают работать, но потребуется изменить все отдельные службы, которые зависели от `ILoggerFactory`, чтобы получить `ILoggerFactory` другим способом.</span><span class="sxs-lookup"><span data-stu-id="d5515-430">In these cases, most things will still work, but any singleton service that was depending on `ILoggerFactory` will need to be changed to obtain the `ILoggerFactory` in a different way.</span></span>

<span data-ttu-id="d5515-431">При возникновении подобных ситуаций зарегистрируйте проблему в [средстве отслеживания вопросов EF Core на сайте GitHub](https://github.com/aspnet/EntityFrameworkCore/issues) и сообщите нам, как вы используете `ILoggerFactory`, чтобы мы могли лучше понять, как избежать подобных ошибок в будущем.</span><span class="sxs-lookup"><span data-stu-id="d5515-431">If you run into situations like this, please file an issue at on the [EF Core GitHub issue tracker](https://github.com/aspnet/EntityFrameworkCore/issues) to let us know how you are using `ILoggerFactory` such that we can better understand how not to break this again in the future.</span></span>

## <a name="idbcontextoptionsextensionwithdebuginfo-merged-into-idbcontextoptionsextension"></a><span data-ttu-id="d5515-432">IDbContextOptionsExtensionWithDebugInfo объединен с IDbContextOptionsExtension</span><span class="sxs-lookup"><span data-stu-id="d5515-432">IDbContextOptionsExtensionWithDebugInfo merged into IDbContextOptionsExtension</span></span>

[<span data-ttu-id="d5515-433">Отслеживание вопроса 13552</span><span class="sxs-lookup"><span data-stu-id="d5515-433">Tracking Issue #13552</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13552)

<span data-ttu-id="d5515-434">Это изменение было внесено в предварительную версию 3 EF Core 3.0.</span><span class="sxs-lookup"><span data-stu-id="d5515-434">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="d5515-435">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="d5515-435">**Old behavior**</span></span>

<span data-ttu-id="d5515-436">`IDbContextOptionsExtensionWithDebugInfo` был дополнительным необязательным интерфейсом, расширяемым из `IDbContextOptionsExtension`, для предотвращения критического изменения в интерфейсе в рамках выпуска версии 2.x.</span><span class="sxs-lookup"><span data-stu-id="d5515-436">`IDbContextOptionsExtensionWithDebugInfo` was an additional optional interface extended from `IDbContextOptionsExtension` to avoid making a breaking change to the interface during the 2.x release cycle.</span></span>

<span data-ttu-id="d5515-437">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="d5515-437">**New behavior**</span></span>

<span data-ttu-id="d5515-438">Теперь эти интерфейсы объединяются в `IDbContextOptionsExtension`.</span><span class="sxs-lookup"><span data-stu-id="d5515-438">The interfaces are now merged together into `IDbContextOptionsExtension`.</span></span>

<span data-ttu-id="d5515-439">**Причина**</span><span class="sxs-lookup"><span data-stu-id="d5515-439">**Why**</span></span>

<span data-ttu-id="d5515-440">Это изменение было внесено, так как интерфейсы представляют собой единое целое.</span><span class="sxs-lookup"><span data-stu-id="d5515-440">This change was made because the interfaces are conceptually one.</span></span>

<span data-ttu-id="d5515-441">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="d5515-441">**Mitigations**</span></span>

<span data-ttu-id="d5515-442">Нужно изменить все реализации `IDbContextOptionsExtension`, чтобы обеспечить поддержку нового члена.</span><span class="sxs-lookup"><span data-stu-id="d5515-442">Any implementations of `IDbContextOptionsExtension` will need to be updated to support the new member.</span></span>

## <a name="lazy-loading-proxies-no-longer-assume-navigation-properties-are-fully-loaded"></a><span data-ttu-id="d5515-443">Прокси с отложенной загрузкой больше не предполагают полную загрузку свойств навигации</span><span class="sxs-lookup"><span data-stu-id="d5515-443">Lazy-loading proxies no longer assume navigation properties are fully loaded</span></span>

[<span data-ttu-id="d5515-444">Отслеживание вопроса 12780</span><span class="sxs-lookup"><span data-stu-id="d5515-444">Tracking Issue #12780</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12780)

<span data-ttu-id="d5515-445">Это изменение будет внесено в предварительную версию 4 EF Core 3.0.</span><span class="sxs-lookup"><span data-stu-id="d5515-445">This change will be introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="d5515-446">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="d5515-446">**Old behavior**</span></span>

<span data-ttu-id="d5515-447">До выхода EF Core 3.0 после удаления `DbContext` не существовало способа узнать, было ли указанное свойство навигации для сущности, полученной из этого контекста, загружено полностью.</span><span class="sxs-lookup"><span data-stu-id="d5515-447">Before EF Core 3.0, once a `DbContext` was disposed there was no way of knowing if a given navigation property on an entity obtained from that context was fully loaded or not.</span></span>
<span data-ttu-id="d5515-448">Вместо это прокси предполагали, что навигация по ссылке загружена, если она имеет отличное NULL значение, а навигация по коллекции загружена, если она не является пустой.</span><span class="sxs-lookup"><span data-stu-id="d5515-448">Proxies would instead assume that a reference navigation is loaded if it has a non-null value, and that a collection navigation is loaded if it isn't empty.</span></span>
<span data-ttu-id="d5515-449">В этом случае попытка отложенной загрузки приводила к холостой операции.</span><span class="sxs-lookup"><span data-stu-id="d5515-449">In these cases, attempting to lazy-load would be a no-op.</span></span>

<span data-ttu-id="d5515-450">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="d5515-450">**New behavior**</span></span>

<span data-ttu-id="d5515-451">Начиная с EF Core 3.0 прокси отслеживают, загружено ли свойство навигации.</span><span class="sxs-lookup"><span data-stu-id="d5515-451">Starting with EF Core 3.0, proxies keep track of whether or not a navigation property is loaded.</span></span>
<span data-ttu-id="d5515-452">Это означает, что попытка обращения к свойству навигации, загружаемому после удаления контекста, всегда будет давать холостую операцию, даже если загруженное свойство навигации пусто или равно NULL.</span><span class="sxs-lookup"><span data-stu-id="d5515-452">This means attempting to access a navigation property that is loaded after the context has been disposed will always be a no-op, even when the loaded navigation is empty or null.</span></span>
<span data-ttu-id="d5515-453">И наоборот, при попытке обращения к незагруженному свойству навигации, когда контекст удален, возникает исключение, даже если это свойство навигации является непустой коллекцией.</span><span class="sxs-lookup"><span data-stu-id="d5515-453">Conversely, attempting to access a navigation property that isn't loaded will throw an exception if the context is disposed even if the navigation property is a non-empty collection.</span></span>
<span data-ttu-id="d5515-454">Подобная ситуация указывает на то, что код приложения пытается использовать отложенную загрузку в недопустимое время, и нужно запретить это, изменив приложение.</span><span class="sxs-lookup"><span data-stu-id="d5515-454">If this situation arises, it means the application code is attempting to use lazy-loading at an invalid time, and the application should be changed to not do this.</span></span>

<span data-ttu-id="d5515-455">**Причина**</span><span class="sxs-lookup"><span data-stu-id="d5515-455">**Why**</span></span>

<span data-ttu-id="d5515-456">Это изменение было внесено, чтобы обеспечить правильное и согласованное поведение при попытке отложенной загрузки для удаленного экземпляра `DbContext`.</span><span class="sxs-lookup"><span data-stu-id="d5515-456">This change was made to make the behavior consistent and correct when attempting to lazy-load on a disposed `DbContext` instance.</span></span>

<span data-ttu-id="d5515-457">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="d5515-457">**Mitigations**</span></span>

<span data-ttu-id="d5515-458">Обновите код приложения, чтобы не выполнять попытку отложенной загрузки с удаленным контекстом, либо настройте соответствующую холостую операцию, как описано в сообщении об исключении.</span><span class="sxs-lookup"><span data-stu-id="d5515-458">Update application code to not attempt lazy-loading with a disposed context, or configure this to be a no-op as described in the exception message.</span></span>

## <a name="excessive-creation-of-internal-service-providers-is-now-an-error-by-default"></a><span data-ttu-id="d5515-459">Чрезмерное создание внутренних поставщиков служб теперь является ошибкой по умолчанию</span><span class="sxs-lookup"><span data-stu-id="d5515-459">Excessive creation of internal service providers is now an error by default</span></span>

[<span data-ttu-id="d5515-460">Отслеживание вопроса 10236</span><span class="sxs-lookup"><span data-stu-id="d5515-460">Tracking Issue #10236</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10236)

<span data-ttu-id="d5515-461">Это изменение было внесено в предварительную версию 3 EF Core 3.0.</span><span class="sxs-lookup"><span data-stu-id="d5515-461">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="d5515-462">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="d5515-462">**Old behavior**</span></span>

<span data-ttu-id="d5515-463">До выхода EF Core 3.0 для приложения, создающего чрезмерное количество внутренних поставщиков служб, регистрировалось предупреждение.</span><span class="sxs-lookup"><span data-stu-id="d5515-463">Before EF Core 3.0, a warning would be logged for an application creating a pathological number of internal service providers.</span></span>

<span data-ttu-id="d5515-464">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="d5515-464">**New behavior**</span></span>

<span data-ttu-id="d5515-465">Начиная с EF Core 3.0 это предупреждение считается ошибкой и возникает исключение.</span><span class="sxs-lookup"><span data-stu-id="d5515-465">Starting with EF Core 3.0, this warning is now considered and error and an exception is thrown.</span></span> 

<span data-ttu-id="d5515-466">**Причина**</span><span class="sxs-lookup"><span data-stu-id="d5515-466">**Why**</span></span>

<span data-ttu-id="d5515-467">Это изменение было внесено для улучшения кода приложения посредством явного указания на подобный патологический случай.</span><span class="sxs-lookup"><span data-stu-id="d5515-467">This change was made to drive better application code through exposing this pathological case more explicitly.</span></span>

<span data-ttu-id="d5515-468">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="d5515-468">**Mitigations**</span></span>

<span data-ttu-id="d5515-469">При возникновении этой ошибки правильнее всего выявить первопричину и остановить создание такого большого числа внутренних поставщиков служб.</span><span class="sxs-lookup"><span data-stu-id="d5515-469">The most appropriate cause of action on encountering this error is to understand the root cause and stop creating so many internal service providers.</span></span>
<span data-ttu-id="d5515-470">Тем не менее эту ошибку можно преобразовать обратно в предупреждение (или игнорировать) с помощью конфигурации для `DbContextOptionsBuilder`.</span><span class="sxs-lookup"><span data-stu-id="d5515-470">However, the error can be converted back to a warning (or ignored) via configuration on the `DbContextOptionsBuilder`.</span></span>
<span data-ttu-id="d5515-471">Например:</span><span class="sxs-lookup"><span data-stu-id="d5515-471">For example:</span></span>

```C#
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
{
    optionsBuilder
        .ConfigureWarnings(w => w.Log(CoreEventId.ManyServiceProvidersCreatedWarning));
}
```

## <a name="new-behavior-for-hasonehasmany-called-with-a-single-string"></a><span data-ttu-id="d5515-472">Новое поведение для вызова HasOne/HasMany с одной строкой</span><span class="sxs-lookup"><span data-stu-id="d5515-472">New behavior for HasOne/HasMany called with a single string</span></span>

[<span data-ttu-id="d5515-473">Отслеживание вопроса № 9171</span><span class="sxs-lookup"><span data-stu-id="d5515-473">Tracking Issue #9171</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9171)

<span data-ttu-id="d5515-474">Это изменение будет внесено в предварительную версию 4 EF Core 3.0.</span><span class="sxs-lookup"><span data-stu-id="d5515-474">This change will be introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="d5515-475">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="d5515-475">**Old behavior**</span></span>

<span data-ttu-id="d5515-476">До версии EF Core 3.0 интерпретация кода с вызовом `HasOne` или `HasMany` с одной строкой была очень запутанной.</span><span class="sxs-lookup"><span data-stu-id="d5515-476">Before EF Core 3.0, code calling `HasOne` or `HasMany` with a single string was interpretted in a confusing way.</span></span>
<span data-ttu-id="d5515-477">Например:</span><span class="sxs-lookup"><span data-stu-id="d5515-477">For example:</span></span>
```C#
modelBuilder.Entity<Samurai>().HasOne("Entrance").WithOne();
```

<span data-ttu-id="d5515-478">Складывается впечатление, что код связывает `Samurai` с другим типом сущности с помощью свойства навигации `Entrance`, которое может быть закрытым.</span><span class="sxs-lookup"><span data-stu-id="d5515-478">The code looks like it is relating `Samurai` to some other entity type using the `Entrance` navigation property, which may be private.</span></span>

<span data-ttu-id="d5515-479">На самом деле этот код пытается создать отношение с некоторым типом сущности под именем `Entrance`, который не имеет свойства навигации.</span><span class="sxs-lookup"><span data-stu-id="d5515-479">In reality, this code attempts to create a relationship to some entity type called `Entrance` with no navigation property.</span></span>

<span data-ttu-id="d5515-480">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="d5515-480">**New behavior**</span></span>

<span data-ttu-id="d5515-481">Начиная с EF Core 3.0, указанный выше код выполняет те действия, которые должен был выполнять раньше.</span><span class="sxs-lookup"><span data-stu-id="d5515-481">Starting with EF Core 3.0, the code above now does what it looked like it should have been doing before.</span></span>

<span data-ttu-id="d5515-482">**Причина**</span><span class="sxs-lookup"><span data-stu-id="d5515-482">**Why**</span></span>

<span data-ttu-id="d5515-483">Старое поведение создавало большую путаницу, особенно при считывании кода конфигурации и при поиске ошибок.</span><span class="sxs-lookup"><span data-stu-id="d5515-483">The old behavior was very confusing, especially when reading the configuration code and looking for errors.</span></span>

<span data-ttu-id="d5515-484">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="d5515-484">**Mitigations**</span></span>

<span data-ttu-id="d5515-485">Это приведет к нарушению работы только тех приложений, которые явно настраивают отношения с помощью строк для имен типов и не указывают явно свойство навигации.</span><span class="sxs-lookup"><span data-stu-id="d5515-485">This will only break applications that are explicitly configuring relationships using strings for type names and without specifying the navigation property explicitly.</span></span>
<span data-ttu-id="d5515-486">Такой подход используется нечасто.</span><span class="sxs-lookup"><span data-stu-id="d5515-486">This is not common.</span></span>
<span data-ttu-id="d5515-487">Прежнее поведение можно реализовать с помощью явной передачи значения `null` для имени свойства навигации.</span><span class="sxs-lookup"><span data-stu-id="d5515-487">The previous behavior can be obtained through explicitly passing `null` for the navigation property name.</span></span>
<span data-ttu-id="d5515-488">Например:</span><span class="sxs-lookup"><span data-stu-id="d5515-488">For example:</span></span>

```C#
modelBuilder.Entity<Samurai>().HasOne("Some.Entity.Type.Name", null).WithOne();
```

## <a name="the-return-type-for-several-async-methods-has-been-changed-from-task-to-valuetask"></a><span data-ttu-id="d5515-489">Тип возвращаемого значения для нескольких асинхронных методов изменен с Task на ValueTask</span><span class="sxs-lookup"><span data-stu-id="d5515-489">The return type for several async methods has been changed from Task to ValueTask</span></span>

[<span data-ttu-id="d5515-490">Отслеживание вопроса № 15184</span><span class="sxs-lookup"><span data-stu-id="d5515-490">Tracking Issue #15184</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15184)

<span data-ttu-id="d5515-491">Это изменение будет внесено в предварительную версию 4 EF Core 3.0.</span><span class="sxs-lookup"><span data-stu-id="d5515-491">This change will be introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="d5515-492">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="d5515-492">**Old behavior**</span></span>

<span data-ttu-id="d5515-493">Следующие асинхронные методы ранее возвращали `Task<T>`:</span><span class="sxs-lookup"><span data-stu-id="d5515-493">The following async methods previously returned a `Task<T>`:</span></span>

* `DbContext.FindAsync()`
* `DbSet.FindAsync()`
* `DbContext.AddAsync()`
* `DbSet.AddAsync()`
* <span data-ttu-id="d5515-494">`ValueGenerator.NextValueAsync()` (и производные классы)</span><span class="sxs-lookup"><span data-stu-id="d5515-494">`ValueGenerator.NextValueAsync()` (and deriving classes)</span></span>

<span data-ttu-id="d5515-495">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="d5515-495">**New behavior**</span></span>

<span data-ttu-id="d5515-496">Упомянутые выше методы теперь возвращают `ValueTask<T>` над тем же `T`, что и раньше.</span><span class="sxs-lookup"><span data-stu-id="d5515-496">The aforementioned methods now return a `ValueTask<T>` over the same `T` as before.</span></span>

<span data-ttu-id="d5515-497">**Причина**</span><span class="sxs-lookup"><span data-stu-id="d5515-497">**Why**</span></span>

<span data-ttu-id="d5515-498">Это изменение уменьшает число выделений кучи, возникающих при вызове этих методов, что способствует повышению общей производительности.</span><span class="sxs-lookup"><span data-stu-id="d5515-498">This change reduces the number of heap allocations incurred when invoking these methods, improving general performance.</span></span>

<span data-ttu-id="d5515-499">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="d5515-499">**Mitigations**</span></span>

<span data-ttu-id="d5515-500">Приложения, просто ожидающие приведенные выше API, нужно просто перекомпилировать — изменять источник не требуется.</span><span class="sxs-lookup"><span data-stu-id="d5515-500">Applications simply awaiting the above APIs only need to be recompiled - no source changes are necessary.</span></span>
<span data-ttu-id="d5515-501">Для более сложного варианта использования (например, передачи возвращаемого класса `Task` методу `Task.WhenAny()`) обычно требуется, чтобы возвращаемый класс `ValueTask<T>` был преобразован в класс `Task<T>` путем вызова в нем метода `AsTask()`.</span><span class="sxs-lookup"><span data-stu-id="d5515-501">A more complex usage (e.g. passing the returned `Task` to `Task.WhenAny()`) typically require that the returned `ValueTask<T>` be converted to a `Task<T>` by calling `AsTask()` on it.</span></span>
<span data-ttu-id="d5515-502">Обратите внимание, что это сводит к нулю сокращение числа выделений, которое возможно в рамках этого изменения.</span><span class="sxs-lookup"><span data-stu-id="d5515-502">Note that this negates the allocation reduction that this change brings.</span></span>

## <a name="the-relationaltypemapping-annotation-is-now-just-typemapping"></a><span data-ttu-id="d5515-503">Заметка Relational:TypeMapping теперь является просто TypeMapping</span><span class="sxs-lookup"><span data-stu-id="d5515-503">The Relational:TypeMapping annotation is now just TypeMapping</span></span>

[<span data-ttu-id="d5515-504">Отслеживание вопроса 9913</span><span class="sxs-lookup"><span data-stu-id="d5515-504">Tracking Issue #9913</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9913)

<span data-ttu-id="d5515-505">Это изменение было внесено в предварительную версию 2 EF Core 3.0.</span><span class="sxs-lookup"><span data-stu-id="d5515-505">This change was introduced in EF Core 3.0-preview 2.</span></span>

<span data-ttu-id="d5515-506">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="d5515-506">**Old behavior**</span></span>

<span data-ttu-id="d5515-507">Для заметок сопоставлений типов использовалось имя "Relational:TypeMapping".</span><span class="sxs-lookup"><span data-stu-id="d5515-507">The annotation name for type mapping annotations was "Relational:TypeMapping".</span></span>

<span data-ttu-id="d5515-508">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="d5515-508">**New behavior**</span></span>

<span data-ttu-id="d5515-509">Теперь для заметок сопоставлений типов используется имя "TypeMapping".</span><span class="sxs-lookup"><span data-stu-id="d5515-509">The annotation name for type mapping annotations is now "TypeMapping".</span></span>

<span data-ttu-id="d5515-510">**Причина**</span><span class="sxs-lookup"><span data-stu-id="d5515-510">**Why**</span></span>

<span data-ttu-id="d5515-511">Сопоставления типов теперь используются не только для поставщиков реляционных баз данных.</span><span class="sxs-lookup"><span data-stu-id="d5515-511">Type mappings are now used for more than just relational database providers.</span></span>

<span data-ttu-id="d5515-512">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="d5515-512">**Mitigations**</span></span>

<span data-ttu-id="d5515-513">Это изменение нарушит работу только тех приложений, которые обращаются к сопоставлению типов непосредственно по заметке, что встречается довольно редко.</span><span class="sxs-lookup"><span data-stu-id="d5515-513">This will only break applications that access the type mapping directly as an annotation, which isn't common.</span></span>
<span data-ttu-id="d5515-514">Наиболее подходящий способ для устранения этой проблемы заключается в использовании поверхности API для доступа к сопоставлениям типов вместо прямого использования заметки.</span><span class="sxs-lookup"><span data-stu-id="d5515-514">The most appropriate action to fix is to use API surface to access type mappings rather than using the annotation directly.</span></span>

## <a name="totable-on-a-derived-type-throws-an-exception"></a><span data-ttu-id="d5515-515">ToTable для производного типа выдает исключение</span><span class="sxs-lookup"><span data-stu-id="d5515-515">ToTable on a derived type throws an exception</span></span> 

[<span data-ttu-id="d5515-516">Отслеживание вопроса 11811</span><span class="sxs-lookup"><span data-stu-id="d5515-516">Tracking Issue #11811</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/11811)

<span data-ttu-id="d5515-517">Это изменение было внесено в предварительную версию 3 EF Core 3.0.</span><span class="sxs-lookup"><span data-stu-id="d5515-517">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="d5515-518">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="d5515-518">**Old behavior**</span></span>

<span data-ttu-id="d5515-519">До выхода EF Core 3.0 вызов `ToTable()` для производного типа игнорировался, так как единственной стратегией сопоставления наследования был метод "одна таблица на иерархию", где такая операция недействительна.</span><span class="sxs-lookup"><span data-stu-id="d5515-519">Before EF Core 3.0, `ToTable()` called on a derived type would be ignored since only inheritance mapping strategy was TPH where this isn't valid.</span></span> 

<span data-ttu-id="d5515-520">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="d5515-520">**New behavior**</span></span>

<span data-ttu-id="d5515-521">Начиная с EF Core 3.0 и в рамках подготовки для добавления поддержки методов "одна таблица на тип" и "одна таблица на каждый отдельный тип/класс" в последующем выпуске вызов `ToTable()` для производного типа теперь будет выдавать исключение, чтобы избежать непредвиденного изменения сопоставления в будущем.</span><span class="sxs-lookup"><span data-stu-id="d5515-521">Starting with EF Core 3.0 and in preparation for adding TPT and TPC support in a later release, `ToTable()` called on a derived type will now throw an exception to avoid an unexpected mapping change in the future.</span></span>

<span data-ttu-id="d5515-522">**Причина**</span><span class="sxs-lookup"><span data-stu-id="d5515-522">**Why**</span></span>

<span data-ttu-id="d5515-523">Сейчас сопоставлять производный тип с другой таблицей запрещено.</span><span class="sxs-lookup"><span data-stu-id="d5515-523">Currently it isn't valid to map a derived type to a different table.</span></span>
<span data-ttu-id="d5515-524">Это изменение позволяет избежать нарушения работы в будущем, когда подобная операция будет разрешена.</span><span class="sxs-lookup"><span data-stu-id="d5515-524">This change avoids breaking in the future when it becomes a valid thing to do.</span></span>

<span data-ttu-id="d5515-525">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="d5515-525">**Mitigations**</span></span>

<span data-ttu-id="d5515-526">Удалите все попытки сопоставить производные типы с другими таблицами.</span><span class="sxs-lookup"><span data-stu-id="d5515-526">Remove any attempts to map derived types to other tables.</span></span>

## <a name="forsqlserverhasindex-replaced-with-hasindex"></a><span data-ttu-id="d5515-527">ForSqlServerHasIndex заменен на HasIndex</span><span class="sxs-lookup"><span data-stu-id="d5515-527">ForSqlServerHasIndex replaced with HasIndex</span></span> 

[<span data-ttu-id="d5515-528">Отслеживание вопроса 12366</span><span class="sxs-lookup"><span data-stu-id="d5515-528">Tracking Issue #12366</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12366)

<span data-ttu-id="d5515-529">Это изменение было внесено в предварительную версию 3 EF Core 3.0.</span><span class="sxs-lookup"><span data-stu-id="d5515-529">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="d5515-530">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="d5515-530">**Old behavior**</span></span>

<span data-ttu-id="d5515-531">До выхода EF Core 3.0 `ForSqlServerHasIndex().ForSqlServerInclude()` предоставлял способ настройки столбцов, используемых с `INCLUDE`.</span><span class="sxs-lookup"><span data-stu-id="d5515-531">Before EF Core 3.0, `ForSqlServerHasIndex().ForSqlServerInclude()` provided a way to configure columns used with `INCLUDE`.</span></span>

<span data-ttu-id="d5515-532">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="d5515-532">**New behavior**</span></span>

<span data-ttu-id="d5515-533">Начиная с EF Core 3.0 использование `Include` для индекса теперь поддерживается на реляционном уровне.</span><span class="sxs-lookup"><span data-stu-id="d5515-533">Starting with EF Core 3.0, using `Include` on an index is now supported at the relational level.</span></span>
<span data-ttu-id="d5515-534">Используйте ключевое слово `HasIndex().ForSqlServerInclude()`.</span><span class="sxs-lookup"><span data-stu-id="d5515-534">Use `HasIndex().ForSqlServerInclude()`.</span></span>

<span data-ttu-id="d5515-535">**Причина**</span><span class="sxs-lookup"><span data-stu-id="d5515-535">**Why**</span></span>

<span data-ttu-id="d5515-536">Это изменение было внесено, чтобы консолидировать API для индексов с `Include` в одном месте для всех поставщиков баз данных.</span><span class="sxs-lookup"><span data-stu-id="d5515-536">This change was made to consolidate the API for indexes with `Include` into one place for all database providers.</span></span>

<span data-ttu-id="d5515-537">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="d5515-537">**Mitigations**</span></span>

<span data-ttu-id="d5515-538">Используйте новый API, как показано выше.</span><span class="sxs-lookup"><span data-stu-id="d5515-538">Use the new API, as shown above.</span></span>

## <a name="metadata-api-changes"></a><span data-ttu-id="d5515-539">Изменения API метаданных</span><span class="sxs-lookup"><span data-stu-id="d5515-539">Metadata API changes</span></span>

[<span data-ttu-id="d5515-540">Отслеживание вопроса № 214</span><span class="sxs-lookup"><span data-stu-id="d5515-540">Tracking Issue #214</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/214)

<span data-ttu-id="d5515-541">Это изменение будет внесено в предварительную версию 4 EF Core 3.0.</span><span class="sxs-lookup"><span data-stu-id="d5515-541">This change will be introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="d5515-542">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="d5515-542">**New behavior**</span></span>

<span data-ttu-id="d5515-543">Следующие свойства были преобразованы в методы расширения:</span><span class="sxs-lookup"><span data-stu-id="d5515-543">The following properties were converted to extension methods:</span></span>

* `IEntityType.QueryFilter` -> `GetQueryFilter()`
* `IEntityType.DefiningQuery` -> `GetDefiningQuery()`
* `IProperty.IsShadowProperty` -> `IsShadowProperty()`
* `IProperty.BeforeSaveBehavior` -> `GetBeforeSaveBehavior()`
* `IProperty.AfterSaveBehavior` -> `GetAfterSaveBehavior()`

<span data-ttu-id="d5515-544">**Причина**</span><span class="sxs-lookup"><span data-stu-id="d5515-544">**Why**</span></span>

<span data-ttu-id="d5515-545">Это изменение упрощает реализацию упомянутых выше интерфейсов.</span><span class="sxs-lookup"><span data-stu-id="d5515-545">This change simplifies the implementation of the aforementioned interfaces.</span></span>

<span data-ttu-id="d5515-546">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="d5515-546">**Mitigations**</span></span>

<span data-ttu-id="d5515-547">Используйте новые методы расширения.</span><span class="sxs-lookup"><span data-stu-id="d5515-547">Use the new extension methods.</span></span>

## <a name="ef-core-no-longer-sends-pragma-for-sqlite-fk-enforcement"></a><span data-ttu-id="d5515-548">EF Core больше не отправляет pragma для принудительного применения FK SQLite</span><span class="sxs-lookup"><span data-stu-id="d5515-548">EF Core no longer sends pragma for SQLite FK enforcement</span></span>

[<span data-ttu-id="d5515-549">Отслеживание вопроса 12151</span><span class="sxs-lookup"><span data-stu-id="d5515-549">Tracking Issue #12151</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12151)

<span data-ttu-id="d5515-550">Это изменение было внесено в предварительную версию 3 EF Core 3.0.</span><span class="sxs-lookup"><span data-stu-id="d5515-550">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="d5515-551">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="d5515-551">**Old behavior**</span></span>

<span data-ttu-id="d5515-552">До выхода версии 3.0 система EF Core отправляла `PRAGMA foreign_keys = 1` при открытии соединения с SQLite.</span><span class="sxs-lookup"><span data-stu-id="d5515-552">Before EF Core 3.0, EF Core would send `PRAGMA foreign_keys = 1` when a connection to SQLite is opened.</span></span>

<span data-ttu-id="d5515-553">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="d5515-553">**New behavior**</span></span>

<span data-ttu-id="d5515-554">Начиная с версии 3.0 система EF Core больше не отправляет `PRAGMA foreign_keys = 1` при открытии соединения с SQLite.</span><span class="sxs-lookup"><span data-stu-id="d5515-554">Starting with EF Core 3.0, EF Core no longer sends `PRAGMA foreign_keys = 1` when a connection to SQLite is opened.</span></span>

<span data-ttu-id="d5515-555">**Причина**</span><span class="sxs-lookup"><span data-stu-id="d5515-555">**Why**</span></span>

<span data-ttu-id="d5515-556">Это изменение было внесено, так как EF Core по умолчанию использует `SQLitePCLRaw.bundle_e_sqlite3`, что, в свою очередь, означает, что принудительное применение FK по умолчанию включено и его не требуется включать каждый раз при открытии соединения.</span><span class="sxs-lookup"><span data-stu-id="d5515-556">This change was made because EF Core uses `SQLitePCLRaw.bundle_e_sqlite3` by default, which in turn means that FK enforcement is switched on by default and doesn't need to be explicitly enabled each time a connection is opened.</span></span>

<span data-ttu-id="d5515-557">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="d5515-557">**Mitigations**</span></span>

<span data-ttu-id="d5515-558">Внешние ключи включены по умолчанию в SQLitePCLRaw.bundle_e_sqlite3, который по умолчанию используется для EF Core.</span><span class="sxs-lookup"><span data-stu-id="d5515-558">Foreign keys are enabled by default in SQLitePCLRaw.bundle_e_sqlite3, which is used by default for EF Core.</span></span>
<span data-ttu-id="d5515-559">В других случаях внешние ключи можно включить, указав `Foreign Keys=True` в строке подключения.</span><span class="sxs-lookup"><span data-stu-id="d5515-559">For other cases, foreign keys can be enabled by specifying `Foreign Keys=True` in your connection string.</span></span>

## <a name="microsoftentityframeworkcoresqlite-now-depends-on-sqlitepclrawbundleesqlite3"></a><span data-ttu-id="d5515-560">Теперь Microsoft.EntityFrameworkCore.Sqlite зависит от SQLitePCLRaw.bundle_e_sqlite3</span><span class="sxs-lookup"><span data-stu-id="d5515-560">Microsoft.EntityFrameworkCore.Sqlite now depends on SQLitePCLRaw.bundle_e_sqlite3</span></span>

<span data-ttu-id="d5515-561">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="d5515-561">**Old behavior**</span></span>

<span data-ttu-id="d5515-562">До выхода версии 3.0 система EF Core использовала `SQLitePCLRaw.bundle_green`.</span><span class="sxs-lookup"><span data-stu-id="d5515-562">Before EF Core 3.0, EF Core used `SQLitePCLRaw.bundle_green`.</span></span>

<span data-ttu-id="d5515-563">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="d5515-563">**New behavior**</span></span>

<span data-ttu-id="d5515-564">Начиная с версии 3.0 система EF Core использует `SQLitePCLRaw.bundle_e_sqlite3`.</span><span class="sxs-lookup"><span data-stu-id="d5515-564">Starting with EF Core 3.0, EF Core uses `SQLitePCLRaw.bundle_e_sqlite3`.</span></span>

<span data-ttu-id="d5515-565">**Причина**</span><span class="sxs-lookup"><span data-stu-id="d5515-565">**Why**</span></span>

<span data-ttu-id="d5515-566">Это изменение было внесено, чтобы версия SQLite, используемая в iOS, была согласована с другими платформами.</span><span class="sxs-lookup"><span data-stu-id="d5515-566">This change was made so that the version of SQLite used on iOS consistent with other platforms.</span></span>

<span data-ttu-id="d5515-567">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="d5515-567">**Mitigations**</span></span>

<span data-ttu-id="d5515-568">Чтобы использовать собственную версию SQLite в iOS, настройте `Microsoft.Data.Sqlite` для использования другого пакета `SQLitePCLRaw`.</span><span class="sxs-lookup"><span data-stu-id="d5515-568">To use the native SQLite version on iOS, configure `Microsoft.Data.Sqlite` to use a different `SQLitePCLRaw` bundle.</span></span>

## <a name="guid-values-are-now-stored-as-text-on-sqlite"></a><span data-ttu-id="d5515-569">Теперь значения Guid хранятся в SQLite в виде значений типа TEXT</span><span class="sxs-lookup"><span data-stu-id="d5515-569">Guid values are now stored as TEXT on SQLite</span></span>

[<span data-ttu-id="d5515-570">Отслеживание вопроса № 15078</span><span class="sxs-lookup"><span data-stu-id="d5515-570">Tracking Issue #15078</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15078)

<span data-ttu-id="d5515-571">Это изменение было внесено в предварительную версию 4 EF Core 3.0.</span><span class="sxs-lookup"><span data-stu-id="d5515-571">This change was introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="d5515-572">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="d5515-572">**Old behavior**</span></span>

<span data-ttu-id="d5515-573">Ранее значения типа Guid хранились в SQLite в виде значений типа BLOB.</span><span class="sxs-lookup"><span data-stu-id="d5515-573">Guid values were previously sored as BLOB values on SQLite.</span></span>

<span data-ttu-id="d5515-574">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="d5515-574">**New behavior**</span></span>

<span data-ttu-id="d5515-575">Теперь значения типа Guid хранятся в виде значений типа TEXT.</span><span class="sxs-lookup"><span data-stu-id="d5515-575">Guid values are now sotred as TEXT.</span></span>

<span data-ttu-id="d5515-576">**Причина**</span><span class="sxs-lookup"><span data-stu-id="d5515-576">**Why**</span></span>

<span data-ttu-id="d5515-577">Двоичный формат типа Guid не стандартизирован.</span><span class="sxs-lookup"><span data-stu-id="d5515-577">The binary format of Guids is not standardized.</span></span> <span data-ttu-id="d5515-578">Хранение значений в виде значений типа TEXT повышает уровень совместимости базы данных с другими технологиями.</span><span class="sxs-lookup"><span data-stu-id="d5515-578">Storing the values as TEXT makes the database more compatible with other technologies.</span></span>

<span data-ttu-id="d5515-579">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="d5515-579">**Mitigations**</span></span>

<span data-ttu-id="d5515-580">Существующие базы данных можно перенести в новый формат, выполнив код SQL следующим образом.</span><span class="sxs-lookup"><span data-stu-id="d5515-580">You can migrate existing databases to the new format by executing SQL like the following.</span></span>

``` sql
UPDATE MyTable
SET GuidColumn = hex(substr(GuidColumn, 4, 1)) ||
                 hex(substr(GuidColumn, 3, 1)) ||
                 hex(substr(GuidColumn, 2, 1)) ||
                 hex(substr(GuidColumn, 1, 1)) || '-' ||
                 hex(substr(GuidColumn, 6, 1)) ||
                 hex(substr(GuidColumn, 5, 1)) || '-' ||
                 hex(substr(GuidColumn, 8, 1)) ||
                 hex(substr(GuidColumn, 7, 1)) || '-' ||
                 hex(substr(GuidColumn, 9, 2)) || '-' ||
                 hex(substr(GuidColumn, 11, 6))
WHERE typeof(GuidColumn) == 'blob';
```

<span data-ttu-id="d5515-581">В EF Core можно продолжить использовать прежнее поведение, настроив преобразователь значений для этих свойств.</span><span class="sxs-lookup"><span data-stu-id="d5515-581">In EF Core, you could also continue using the previous behavior by configuirng a value converter on these properties.</span></span>

``` csharp
modelBuilder
    .Entity<MyEntity>()
    .Property(e => e.GuidProperty)
    .HasConversion(
        g => g.ToByteArray(),
        b => new Guid(b));
```

<span data-ttu-id="d5515-582">Microsoft.Data.Sqlite по-прежнему может читать значения Guid из столбцов BLOB и TEXT. Но поскольку изменился формат по умолчанию для параметров и констант, скорее всего, вам потребуется выполнить некоторые действия в большинстве сценариев, где используются значения Guid.</span><span class="sxs-lookup"><span data-stu-id="d5515-582">Microsoft.Data.Sqlite remains capable of reading Guid values from both BLOB and TEXT columns; however, since the default format for parameters and constants has changed you'll likely need to take action for most scenarios involving Guids.</span></span>

## <a name="char-values-are-now-stored-as-text-on-sqlite"></a><span data-ttu-id="d5515-583">Теперь значения типа Char хранятся в SQLite в виде значений типа TEXT</span><span class="sxs-lookup"><span data-stu-id="d5515-583">Char values are now stored as TEXT on SQLite</span></span>

[<span data-ttu-id="d5515-584">Отслеживание вопроса № 15020</span><span class="sxs-lookup"><span data-stu-id="d5515-584">Tracking Issue #15020</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15020)

<span data-ttu-id="d5515-585">Это изменение было внесено в предварительную версию 4 EF Core 3.0.</span><span class="sxs-lookup"><span data-stu-id="d5515-585">This change was introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="d5515-586">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="d5515-586">**Old behavior**</span></span>

<span data-ttu-id="d5515-587">Ранее значения типа Char хранились в SQLite в виде значений типа INTEGER.</span><span class="sxs-lookup"><span data-stu-id="d5515-587">Char values were previously sored as INTEGER values on SQLite.</span></span> <span data-ttu-id="d5515-588">Например, значение типа Char *A* хранилось как целочисленное значение 65.</span><span class="sxs-lookup"><span data-stu-id="d5515-588">For example, a char value of *A* was stored as the integer value 65.</span></span>

<span data-ttu-id="d5515-589">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="d5515-589">**New behavior**</span></span>

<span data-ttu-id="d5515-590">Теперь значения типа Char хранятся в виде значений типа TEXT.</span><span class="sxs-lookup"><span data-stu-id="d5515-590">Char values are now sotred as TEXT.</span></span>

<span data-ttu-id="d5515-591">**Причина**</span><span class="sxs-lookup"><span data-stu-id="d5515-591">**Why**</span></span>

<span data-ttu-id="d5515-592">Хранение значений в виде значений типа TEXT является более естественным вариантом и повышает уровень совместимости базы данных с другими технологиями.</span><span class="sxs-lookup"><span data-stu-id="d5515-592">Storing the values as TEXT is more natural and makes the database more compatible with other technologies.</span></span>

<span data-ttu-id="d5515-593">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="d5515-593">**Mitigations**</span></span>

<span data-ttu-id="d5515-594">Существующие базы данных можно перенести в новый формат, выполнив код SQL следующим образом.</span><span class="sxs-lookup"><span data-stu-id="d5515-594">You can migrate existing databases to the new format by executing SQL like the following.</span></span>

``` sql
UPDATE MyTable
SET CharColumn = char(CharColumn)
WHERE typeof(CharColumn) = 'integer';
```

<span data-ttu-id="d5515-595">В EF Core можно продолжить использовать прежнее поведение, настроив преобразователь значений для этих свойств.</span><span class="sxs-lookup"><span data-stu-id="d5515-595">In EF Core, you could also continue using the previous behavior by configuirng a value converter on these properties.</span></span>

``` csharp
modelBuilder
    .Entity<MyEntity>()
    .Property(e => e.CharProperty)
    .HasConversion(
        c => (long)c,
        i => (char)i);
```

<span data-ttu-id="d5515-596">Microsoft.Data.Sqlite также сохраняет возможность чтения значений символов из столбцов INTEGER и TEXT, поэтому для реализации определенных сценариев может не потребоваться никаких действий.</span><span class="sxs-lookup"><span data-stu-id="d5515-596">Microsoft.Data.Sqlite also remains capable of reading character values from both INTEGER and TEXT columns, so certain scenarios may not require any action.</span></span>

## <a name="migration-ids-are-now-generated-using-the-invariant-cultures-calendar"></a><span data-ttu-id="d5515-597">Идентификаторы миграции теперь создаются с использованием календаря инвариантного языка и региональных параметров</span><span class="sxs-lookup"><span data-stu-id="d5515-597">Migration IDs are now generated using the invariant culture's calendar</span></span>

[<span data-ttu-id="d5515-598">Отслеживание вопроса № 12978</span><span class="sxs-lookup"><span data-stu-id="d5515-598">Tracking Issue #12978</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12978)

<span data-ttu-id="d5515-599">Это изменение было внесено в предварительную версию 4 EF Core 3.0.</span><span class="sxs-lookup"><span data-stu-id="d5515-599">This change was introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="d5515-600">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="d5515-600">**Old behavior**</span></span>

<span data-ttu-id="d5515-601">Идентификаторы миграции непреднамеренно создавались с использованием календаря текущего языка и региональных параметров.</span><span class="sxs-lookup"><span data-stu-id="d5515-601">Migration IDs were inadvertantly generated using the currret culture's calendar.</span></span>

<span data-ttu-id="d5515-602">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="d5515-602">**New behavior**</span></span>

<span data-ttu-id="d5515-603">Теперь идентификаторы миграций всегда создаются с использованием календаря инвариантного языка и региональных параметров (григорианского).</span><span class="sxs-lookup"><span data-stu-id="d5515-603">Migration IDs are now always generated using the invariant culture's calendar (Gregorian).</span></span>

<span data-ttu-id="d5515-604">**Причина**</span><span class="sxs-lookup"><span data-stu-id="d5515-604">**Why**</span></span>

<span data-ttu-id="d5515-605">Порядок миграций играет важную роль при обновлении базы данных или разрешении конфликтов слияния.</span><span class="sxs-lookup"><span data-stu-id="d5515-605">The order of migrations is important when updating the database or resolving merge conflicts.</span></span> <span data-ttu-id="d5515-606">Использование инвариантного календаря позволяет избежать проблем с порядком выполнения, которые могут быть связаны с наличием разных системных календарей у членов группы.</span><span class="sxs-lookup"><span data-stu-id="d5515-606">Using the invariant calendar avoids ordering issues that can result from team members having different system calendars.</span></span>

<span data-ttu-id="d5515-607">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="d5515-607">**Mitigations**</span></span>

<span data-ttu-id="d5515-608">Это изменение затрагивает всех, кто использует календари, отличные от григорианского, в которых значение года больше, чем в григорианском (например, тай-буддистский календарь).</span><span class="sxs-lookup"><span data-stu-id="d5515-608">This change affects anyone using a non-Gregorian calender where the year is greater than the Gregorian calendar (like the Thai Buddhist calendar).</span></span> <span data-ttu-id="d5515-609">Имеющиеся идентификаторы миграций следует обновить, чтобы новые миграции выполнялись после существующих.</span><span class="sxs-lookup"><span data-stu-id="d5515-609">Existing migration IDs will need to be updated so that new migrations are ordered after existing migrations.</span></span>

<span data-ttu-id="d5515-610">Идентификатор миграции можно найти в атрибуте Migration в файлы конструктора миграций.</span><span class="sxs-lookup"><span data-stu-id="d5515-610">The migration ID can be found in the Migration attribute in the migrations' designer files.</span></span>

``` diff
 [DbContext(typeof(MyDbContext))]
-[Migration("25620318122820_MyMigration")]
+[Migration("20190318122820_MyMigration")]
 partial class MyMigration
 {
```

<span data-ttu-id="d5515-611">Кроме того, потребуется обновить таблицу журнала миграций.</span><span class="sxs-lookup"><span data-stu-id="d5515-611">The Migrations history table also needs to be updated.</span></span>

``` sql
UPDATE __EFMigrationsHistory
SET MigrationId = CONCAT(LEFT(MigrationId, 4)  - 543, SUBSTRING(MigrationId, 4, 150))
```

## <a name="logquerypossibleexceptionwithaggregateoperator-has-been-renamed"></a><span data-ttu-id="d5515-612">Оператор LogQueryPossibleExceptionWithAggregateOperator был переименован</span><span class="sxs-lookup"><span data-stu-id="d5515-612">LogQueryPossibleExceptionWithAggregateOperator has been renamed</span></span>

[<span data-ttu-id="d5515-613">Отслеживание проблемы № 10985</span><span class="sxs-lookup"><span data-stu-id="d5515-613">Tracking Issue #10985</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10985)

<span data-ttu-id="d5515-614">Это изменение было внесено в предварительную версию 4 EF Core 3.0.</span><span class="sxs-lookup"><span data-stu-id="d5515-614">This change was introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="d5515-615">**Изменение**</span><span class="sxs-lookup"><span data-stu-id="d5515-615">**Change**</span></span>

<span data-ttu-id="d5515-616">`RelationalEventId.LogQueryPossibleExceptionWithAggregateOperator` переименован в `RelationalEventId.LogQueryPossibleExceptionWithAggregateOperatorWarning`.</span><span class="sxs-lookup"><span data-stu-id="d5515-616">`RelationalEventId.LogQueryPossibleExceptionWithAggregateOperator` has been renamed to `RelationalEventId.LogQueryPossibleExceptionWithAggregateOperatorWarning`.</span></span>

<span data-ttu-id="d5515-617">**Причина**</span><span class="sxs-lookup"><span data-stu-id="d5515-617">**Why**</span></span>

<span data-ttu-id="d5515-618">Выравнивает именование этого события предупреждения с другими событиями предупреждения.</span><span class="sxs-lookup"><span data-stu-id="d5515-618">Aligns the naming of this warning event with all other warning events.</span></span>

<span data-ttu-id="d5515-619">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="d5515-619">**Mitigations**</span></span>

<span data-ttu-id="d5515-620">Используйте новое имя.</span><span class="sxs-lookup"><span data-stu-id="d5515-620">Use the new name.</span></span> <span data-ttu-id="d5515-621">(Обратите внимание, что номер события не изменен.)</span><span class="sxs-lookup"><span data-stu-id="d5515-621">(Note that the event ID number has not changed.)</span></span>

## <a name="clarify-api-for-foreign-key-constraint-names"></a><span data-ttu-id="d5515-622">Уточнение API для имен ограничений внешнего ключа</span><span class="sxs-lookup"><span data-stu-id="d5515-622">Clarify API for foreign key constraint names</span></span>

[<span data-ttu-id="d5515-623">Отслеживание проблемы № 10730</span><span class="sxs-lookup"><span data-stu-id="d5515-623">Tracking Issue #10730</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10730)

<span data-ttu-id="d5515-624">Это изменение было внесено в предварительную версию 4 EF Core 3.0.</span><span class="sxs-lookup"><span data-stu-id="d5515-624">This change was introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="d5515-625">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="d5515-625">**Old behavior**</span></span>

<span data-ttu-id="d5515-626">До EF Core 3.0 имена ограничений внешнего ключа назывались просто именами.</span><span class="sxs-lookup"><span data-stu-id="d5515-626">Before EF Core 3.0, foreign key constraint names were referred to as simply the "name".</span></span> <span data-ttu-id="d5515-627">Например:</span><span class="sxs-lookup"><span data-stu-id="d5515-627">For example:</span></span>

```C#
var constraintName = myForeignKey.Name;
```

<span data-ttu-id="d5515-628">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="d5515-628">**New behavior**</span></span>

<span data-ttu-id="d5515-629">Начиная с EF Core 3.0 имена ограничений внешнего ключа называются "имя ограничения".</span><span class="sxs-lookup"><span data-stu-id="d5515-629">Starting with EF Core 3.0, foreign key constraint names are now referred to as the "constaint name".</span></span> <span data-ttu-id="d5515-630">Например:</span><span class="sxs-lookup"><span data-stu-id="d5515-630">For example:</span></span>

```C#
var constraintName = myForeignKey.ConstraintName;
```

<span data-ttu-id="d5515-631">**Причина**</span><span class="sxs-lookup"><span data-stu-id="d5515-631">**Why**</span></span>

<span data-ttu-id="d5515-632">Это изменение обеспечивает согласованность именования в этой области, а также проясняет, что это имя ограничения внешнего ключа, а не имя столбца или свойства, для которого определен внешний ключ.</span><span class="sxs-lookup"><span data-stu-id="d5515-632">This change brings consistency to naming in this area, and also clarifies that this is the name of the foreign key constaint, and not the column or property name that the foreign key is defined on.</span></span>

<span data-ttu-id="d5515-633">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="d5515-633">**Mitigations**</span></span>

<span data-ttu-id="d5515-634">Используйте новое имя.</span><span class="sxs-lookup"><span data-stu-id="d5515-634">Use the new name.</span></span>
