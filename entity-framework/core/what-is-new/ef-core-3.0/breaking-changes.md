---
title: Критические изменения в EF Core 3.0 — EF Core
author: divega
ms.date: 02/19/2019
ms.assetid: EE2878C9-71F9-4FA5-9BC4-60517C7C9830
uid: core/what-is-new/ef-core-3.0/breaking-changes
ms.openlocfilehash: 96586808862c4373168dcd34a5f00c9f2f7563c3
ms.sourcegitcommit: 9bd64a1a71b7f7aeb044aeecc7c4785b57db1ec9
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/26/2019
ms.locfileid: "67394831"
---
# <a name="breaking-changes-included-in-ef-core-30-currently-in-preview"></a><span data-ttu-id="11eba-102">Критические изменения в EF Core 3.0 (сейчас предоставляются в виде предварительной версии)</span><span class="sxs-lookup"><span data-stu-id="11eba-102">Breaking changes included in EF Core 3.0 (currently in preview)</span></span>

> [!IMPORTANT]
> <span data-ttu-id="11eba-103">Обратите внимание на то, что наборы возможностей и расписания будущих выпусков могут быть изменены, и, несмотря на то, что мы стараемся поддерживать эту страницу в актуальном состоянии, в определенные моменты времени содержание страницы может не отражать наши текущие планы.</span><span class="sxs-lookup"><span data-stu-id="11eba-103">Please note that the feature sets and schedules of future releases are always subject to change, and although we will try to keep this page up to date, it may not reflect our latest plans at all times.</span></span>

<span data-ttu-id="11eba-104">Указанные ниже изменения API и поведения могут нарушать работу приложений, разработанных для EF Core 2.2.x, при их обновлении до версии 3.0.0.</span><span class="sxs-lookup"><span data-stu-id="11eba-104">The following API and behavior changes have the potential to break applications developed for EF Core 2.2.x when upgrading them to 3.0.0.</span></span>
<span data-ttu-id="11eba-105">Изменения, которые, по нашим ожиданиям, повлияют только на поставщиков баз данных, описаны в разделе [изменений для поставщиков](../../providers/provider-log.md).</span><span class="sxs-lookup"><span data-stu-id="11eba-105">Changes that we expect to only impact database providers are documented under [provider changes](../../providers/provider-log.md).</span></span>
<span data-ttu-id="11eba-106">Здесь не описаны критические изменения в новых функциях, добавленных между разными предварительными версиями 3.0.</span><span class="sxs-lookup"><span data-stu-id="11eba-106">Breaks in new features introduced from one 3.0 preview to another 3.0 preview aren't documented here.</span></span>

## <a name="linq-queries-are-no-longer-evaluated-on-the-client"></a><span data-ttu-id="11eba-107">Запросы LINQ больше не вычисляются на клиенте</span><span class="sxs-lookup"><span data-stu-id="11eba-107">LINQ queries are no longer evaluated on the client</span></span>

<span data-ttu-id="11eba-108">[Отслеживание вопроса № 14935](https://github.com/aspnet/EntityFrameworkCore/issues/14935)
[Также см. вопрос № 12795](https://github.com/aspnet/EntityFrameworkCore/issues/12795)</span><span class="sxs-lookup"><span data-stu-id="11eba-108">[Tracking Issue #14935](https://github.com/aspnet/EntityFrameworkCore/issues/14935)
[Also see issue #12795](https://github.com/aspnet/EntityFrameworkCore/issues/12795)</span></span>

<span data-ttu-id="11eba-109">Это изменение реализовано в EF Core 3.0, предварительная версия 4.</span><span class="sxs-lookup"><span data-stu-id="11eba-109">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="11eba-110">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="11eba-110">**Old behavior**</span></span>

<span data-ttu-id="11eba-111">До выхода версии 3.0, если системе EF Core не удавалось преобразовать выражение, являющееся частью запроса, в код SQL или параметр, она автоматически рассчитывала это выражение на клиенте.</span><span class="sxs-lookup"><span data-stu-id="11eba-111">Before 3.0, when EF Core couldn't convert an expression that was part of a query to either SQL or a parameter, it automatically evaluated the expression on the client.</span></span>
<span data-ttu-id="11eba-112">По умолчанию вычисление на клиенте потенциально ресурсоемких выражений вызывало лишь отображение предупреждения.</span><span class="sxs-lookup"><span data-stu-id="11eba-112">By default, client evaluation of potentially expensive expressions only triggered a warning.</span></span>

<span data-ttu-id="11eba-113">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="11eba-113">**New behavior**</span></span>

<span data-ttu-id="11eba-114">Начиная с версии 3.0 система EF Core разрешает вычислять на клиенте только выражения в высокоуровневой проекции (последний вызов `Select()` в запросе).</span><span class="sxs-lookup"><span data-stu-id="11eba-114">Starting with 3.0, EF Core only allows expressions in the top-level projection (the last `Select()` call in the query) to be evaluated on the client.</span></span>
<span data-ttu-id="11eba-115">Если выражения в любой другой части запроса невозможно преобразовать в код SQL или параметр, возникает исключение.</span><span class="sxs-lookup"><span data-stu-id="11eba-115">When expressions in any other part of the query can't be converted to either SQL or a parameter, an exception is thrown.</span></span>

<span data-ttu-id="11eba-116">**Причина**</span><span class="sxs-lookup"><span data-stu-id="11eba-116">**Why**</span></span>

<span data-ttu-id="11eba-117">Автоматическое вычисление запросов на клиенте позволяет выполнять многие запросы, даже если не удается преобразовать их важные части.</span><span class="sxs-lookup"><span data-stu-id="11eba-117">Automatic client evaluation of queries allows many queries to be executed even if important parts of them can't be translated.</span></span>
<span data-ttu-id="11eba-118">Это может привести к непредвиденному и потенциально опасному поведению, которое может стать заметным только в рабочей среде.</span><span class="sxs-lookup"><span data-stu-id="11eba-118">This behavior can result in unexpected and potentially damaging behavior that may only become evident in production.</span></span>
<span data-ttu-id="11eba-119">Например, условие в вызове `Where()`, которое невозможно преобразовать, может привести к передаче всех строк из таблицы с сервера базы данных и к применению фильтра на клиенте.</span><span class="sxs-lookup"><span data-stu-id="11eba-119">For example, a condition in a `Where()` call which can't be translated can cause all rows from the table to be transferred from the database server, and the filter to be applied on the client.</span></span>
<span data-ttu-id="11eba-120">Такая ситуация легко может остаться незамеченной, если таблица содержит небольшое число строк в разработке, и дать резкие отрицательные последствия при переносе приложения в рабочую среду, когда таблица может содержать миллионы строк.</span><span class="sxs-lookup"><span data-stu-id="11eba-120">This situation can easily go undetected if the table contains only a few rows in development, but hit hard when the application moves to production, where the table may contain millions of rows.</span></span>
<span data-ttu-id="11eba-121">Практика показала, что предупреждения о вычислении на клиенте также часто игнорируются во время разработки.</span><span class="sxs-lookup"><span data-stu-id="11eba-121">Client evaluation warnings also proved too easy to ignore during development.</span></span>

<span data-ttu-id="11eba-122">Кроме того, автоматическое вычисление на клиенте может привести к проблемам, из-за которых улучшение преобразования запросов для определенных выражений приводит к возникновению непреднамеренных критических изменений между выпусками.</span><span class="sxs-lookup"><span data-stu-id="11eba-122">Besides this, automatic client evaluation can lead to issues in which improving query translation for specific expressions caused unintended breaking changes between releases.</span></span>

<span data-ttu-id="11eba-123">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="11eba-123">**Mitigations**</span></span>

<span data-ttu-id="11eba-124">Если запрос невозможно преобразовать полностью, то перепишите его в форме, которую можно преобразовать, либо используйте `AsEnumerable()`, `ToList()` или аналогичный элемент, чтобы явно перенести данные обратно на клиент, где можно произвести их дальнейшую обработку с помощью LINQ-to-Objects.</span><span class="sxs-lookup"><span data-stu-id="11eba-124">If a query can't be fully translated, then either rewrite the query in a form that can be translated, or use `AsEnumerable()`, `ToList()`, or similar to explicitly bring data back to the client where it can then be further processed using LINQ-to-Objects.</span></span>

## <a name="entity-framework-core-is-no-longer-part-of-the-aspnet-core-shared-framework"></a><span data-ttu-id="11eba-125">Entity Framework Core больше не является частью общей платформы ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="11eba-125">Entity Framework Core is no longer part of the ASP.NET Core shared framework</span></span>

[<span data-ttu-id="11eba-126">Отслеживание объявлений о вопросе 325</span><span class="sxs-lookup"><span data-stu-id="11eba-126">Tracking Issue Announcements#325</span></span>](https://github.com/aspnet/Announcements/issues/325)

<span data-ttu-id="11eba-127">Это изменение реализовано в ASP.NET Core 3.0, предварительная версия 1.</span><span class="sxs-lookup"><span data-stu-id="11eba-127">This change is introduced in ASP.NET Core 3.0-preview 1.</span></span> 

<span data-ttu-id="11eba-128">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="11eba-128">**Old behavior**</span></span>

<span data-ttu-id="11eba-129">До выхода ASP.NET Core 3.0 при добавлении ссылки на пакет для `Microsoft.AspNetCore.App` или `Microsoft.AspNetCore.All` она включала в себя EF Core и некоторые поставщики данных EF Core, например поставщик SQL Server.</span><span class="sxs-lookup"><span data-stu-id="11eba-129">Before ASP.NET Core 3.0, when you added a package reference to `Microsoft.AspNetCore.App` or `Microsoft.AspNetCore.All`, it would include EF Core and some of the EF Core data providers like the SQL Server provider.</span></span>

<span data-ttu-id="11eba-130">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="11eba-130">**New behavior**</span></span>

<span data-ttu-id="11eba-131">Начиная с версии 3.0 общая платформа ASP.NET Core не включает в себя EF Core или поставщики данных EF Core.</span><span class="sxs-lookup"><span data-stu-id="11eba-131">Starting in 3.0, the ASP.NET Core shared framework doesn't include EF Core or any EF Core data providers.</span></span>

<span data-ttu-id="11eba-132">**Причина**</span><span class="sxs-lookup"><span data-stu-id="11eba-132">**Why**</span></span>

<span data-ttu-id="11eba-133">До этого изменения для получения EF Core требовались различные действия в зависимости от того, ориентировано ли приложение на ASP.NET Core и SQL Server.</span><span class="sxs-lookup"><span data-stu-id="11eba-133">Before this change, getting EF Core required different steps depending on whether the application targeted ASP.NET Core and SQL Server or not.</span></span> <span data-ttu-id="11eba-134">Кроме того, обновление ASP.NET Core приводило к принудительному обновлению EF Core и поставщика SQL Server, что иногда нежелательно.</span><span class="sxs-lookup"><span data-stu-id="11eba-134">Also, upgrading ASP.NET Core forced the upgrade of EF Core and the SQL Server provider, which isn't always desirable.</span></span>

<span data-ttu-id="11eba-135">Благодаря этому изменению процесс получения EF Core стал одинаковым для всех поставщиков, поддерживаемых реализаций .NET и типов приложений.</span><span class="sxs-lookup"><span data-stu-id="11eba-135">With this change, the experience of getting EF Core is the same across all providers, supported .NET implementations and application types.</span></span>
<span data-ttu-id="11eba-136">Кроме того, теперь разработчики могут точно управлять временем обновления EF Core и поставщиков данных EF Core.</span><span class="sxs-lookup"><span data-stu-id="11eba-136">Developers can also now control exactly when EF Core and EF Core data providers are upgraded.</span></span>

<span data-ttu-id="11eba-137">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="11eba-137">**Mitigations**</span></span>

<span data-ttu-id="11eba-138">Чтобы использовать EF Core в приложении ASP.NET Core 3.0 или любом другом поддерживаемом приложении, нужно явно добавить ссылку на пакет в поставщик базы данных EF Core, который ваше приложение будет использовать.</span><span class="sxs-lookup"><span data-stu-id="11eba-138">To use EF Core in an ASP.NET Core 3.0 application or any other supported application, explicitly add a package reference to the EF Core database provider that your application will use.</span></span>

## <a name="the-ef-core-command-line-tool-dotnet-ef-is-no-longer-part-of-the-net-core-sdk"></a><span data-ttu-id="11eba-139">Средство командной строки EF Core, dotnet ef больше не входит в пакет SDK для .NET Core</span><span class="sxs-lookup"><span data-stu-id="11eba-139">The EF Core command-line tool, dotnet ef, is no longer part of the .NET Core SDK</span></span>

[<span data-ttu-id="11eba-140">Отслеживание вопроса № 14016</span><span class="sxs-lookup"><span data-stu-id="11eba-140">Tracking Issue #14016</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14016)

<span data-ttu-id="11eba-141">Это изменение реализовано в EF Core 3.0, предварительная версия 4, и соответствующей версии пакета SDK для .NET Core.</span><span class="sxs-lookup"><span data-stu-id="11eba-141">This change is introduced in EF Core 3.0-preview 4 and the corresponding version of the .NET Core SDK.</span></span>

<span data-ttu-id="11eba-142">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="11eba-142">**Old behavior**</span></span>

<span data-ttu-id="11eba-143">До версии 3.0 средство `dotnet ef` входило в состав пакета SDK для .NET Core и было доступно для использования из командной строки любого проекта без дополнительных действий.</span><span class="sxs-lookup"><span data-stu-id="11eba-143">Before 3.0, the `dotnet ef` tool was included in the .NET Core SDK and was readily available to use from the command line from any project without requiring extra steps.</span></span> 

<span data-ttu-id="11eba-144">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="11eba-144">**New behavior**</span></span>

<span data-ttu-id="11eba-145">Начиная с версии 3.0 средство `dotnet ef` не входит в состав пакета SDK для .NET, поэтому если вам нужно его использовать, явно установите его как локальное или глобальное средство.</span><span class="sxs-lookup"><span data-stu-id="11eba-145">Starting in 3.0, the .NET SDK does not include the `dotnet ef` tool, so before you can use it you have to explicitly install it as a local or global tool.</span></span> 

<span data-ttu-id="11eba-146">**Причина**</span><span class="sxs-lookup"><span data-stu-id="11eba-146">**Why**</span></span>

<span data-ttu-id="11eba-147">Это изменение позволяет распространять и обновлять `dotnet ef` как обычное средство .NET CLI в NuGet в соответствии с тем, что EF Core 3.0 также всегда распространяется в виде пакета NuGet.</span><span class="sxs-lookup"><span data-stu-id="11eba-147">This change allows us to distribute and update `dotnet ef` as a regular .NET CLI tool on NuGet, consistent with the fact that the EF Core 3.0 is also always distributed as a NuGet package.</span></span>

<span data-ttu-id="11eba-148">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="11eba-148">**Mitigations**</span></span>

<span data-ttu-id="11eba-149">Чтобы управлять миграциями или сформировать шаблон `DbContext`, установите `dotnet-ef` с помощью команды `dotnet tool install`.</span><span class="sxs-lookup"><span data-stu-id="11eba-149">To be able to manage migrations or scaffold a `DbContext`, install `dotnet-ef` using the `dotnet tool install` command.</span></span>
<span data-ttu-id="11eba-150">Например, чтобы установить это средство как глобальное, можно ввести эту команду:</span><span class="sxs-lookup"><span data-stu-id="11eba-150">For example, to install it as a global tool, you can type this command:</span></span>

  ``` console
  $ dotnet tool install --global dotnet-ef --version <exact-version>
  ```

<span data-ttu-id="11eba-151">Это средство можно получить в виде локального инструмента при восстановлении зависимостей проекта, в котором оно объявляется как соответствующая зависимость, с помощью [файла манифеста средства](https://github.com/dotnet/cli/issues/10288).</span><span class="sxs-lookup"><span data-stu-id="11eba-151">You can also obtain it a local tool when you restore the dependencies of a project that declares it as a tooling dependency using a [tool manifest file](https://github.com/dotnet/cli/issues/10288).</span></span>

## <a name="fromsql-executesql-and-executesqlasync-have-been-renamed"></a><span data-ttu-id="11eba-152">FromSql, ExecuteSql и ExecuteSqlAsync были переименованы</span><span class="sxs-lookup"><span data-stu-id="11eba-152">FromSql, ExecuteSql, and ExecuteSqlAsync have been renamed</span></span>

[<span data-ttu-id="11eba-153">Отслеживание вопроса № 10996</span><span class="sxs-lookup"><span data-stu-id="11eba-153">Tracking Issue #10996</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10996)

<span data-ttu-id="11eba-154">Это изменение реализовано в EF Core 3.0, предварительная версия 4.</span><span class="sxs-lookup"><span data-stu-id="11eba-154">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="11eba-155">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="11eba-155">**Old behavior**</span></span>

<span data-ttu-id="11eba-156">До выхода EF Core 3.0 имена этих методов были перегружены для работы с обычной строкой или строкой, которая должна быть интерполирована в SQL и параметры.</span><span class="sxs-lookup"><span data-stu-id="11eba-156">Before EF Core 3.0, these method names were overloaded to work with either a normal string or a string that should be interpolated into SQL and parameters.</span></span>

<span data-ttu-id="11eba-157">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="11eba-157">**New behavior**</span></span>

<span data-ttu-id="11eba-158">Начиная с EF Core 3.0 используйте `FromSqlRaw`, `ExecuteSqlRaw` и `ExecuteSqlRawAsync` для создания параметризованного запроса, где параметры передаются отдельно из строки запроса.</span><span class="sxs-lookup"><span data-stu-id="11eba-158">Starting with EF Core 3.0, use `FromSqlRaw`, `ExecuteSqlRaw`, and `ExecuteSqlRawAsync` to create a parameterized query where the parameters are passed separately from the query string.</span></span>
<span data-ttu-id="11eba-159">Например:</span><span class="sxs-lookup"><span data-stu-id="11eba-159">For example:</span></span>

```C#
context.Products.FromSqlRaw(
    "SELECT * FROM Products WHERE Name = {0}",
    product.Name);
```

<span data-ttu-id="11eba-160">Используйте `FromSqlInterpolated`, `ExecuteSqlInterpolated` и `ExecuteSqlInterpolatedAsync` для создания параметризованного запроса, где параметры передаются как часть интерполированной строки запроса.</span><span class="sxs-lookup"><span data-stu-id="11eba-160">Use `FromSqlInterpolated`, `ExecuteSqlInterpolated`, and `ExecuteSqlInterpolatedAsync` to create a parameterized query where the parameters are passed as part of an interpolated query string.</span></span>
<span data-ttu-id="11eba-161">Например:</span><span class="sxs-lookup"><span data-stu-id="11eba-161">For example:</span></span>

```C#
context.Products.FromSqlInterpolated(
    $"SELECT * FROM Products WHERE Name = {product.Name}");
```

<span data-ttu-id="11eba-162">Обратите внимание, что оба упомянутых выше запроса будут возвращать одинаковый параметризованный SQL с одинаковыми параметрами SQL.</span><span class="sxs-lookup"><span data-stu-id="11eba-162">Note that both of the queries above will produce the same parameterized SQL with the same SQL parameters.</span></span>

<span data-ttu-id="11eba-163">**Причина**</span><span class="sxs-lookup"><span data-stu-id="11eba-163">**Why**</span></span>

<span data-ttu-id="11eba-164">Подобные перегрузки методов могут случайно вызывать метод необработанной строки в тех случаях, когда требовалось вызвать метод интерполированной строки, и наоборот.</span><span class="sxs-lookup"><span data-stu-id="11eba-164">Method overloads like this make it very easy to accidentally call the raw string method when the intent was to call the interpolated string method, and the other way around.</span></span>
<span data-ttu-id="11eba-165">Это может привести к тому, что запросы не параметризуются, хотя они должны параметризоваться.</span><span class="sxs-lookup"><span data-stu-id="11eba-165">This could result in queries not being parameterized when they should have been.</span></span>

<span data-ttu-id="11eba-166">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="11eba-166">**Mitigations**</span></span>

<span data-ttu-id="11eba-167">Перейдите на использование новых имен методов.</span><span class="sxs-lookup"><span data-stu-id="11eba-167">Switch to use the new method names.</span></span>

## <a name="fromsql-methods-can-only-be-specified-on-query-roots"></a><span data-ttu-id="11eba-168">Методы FromSql можно указать только в корневых элементах запроса.</span><span class="sxs-lookup"><span data-stu-id="11eba-168">FromSql methods can only be specified on query roots</span></span>

[<span data-ttu-id="11eba-169">Отслеживание вопроса № 15704</span><span class="sxs-lookup"><span data-stu-id="11eba-169">Tracking Issue #15704</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15704)

<span data-ttu-id="11eba-170">Это изменение реализовано в EF Core 3.0, предварительная версия 6.</span><span class="sxs-lookup"><span data-stu-id="11eba-170">This change is introduced in EF Core 3.0-preview 6.</span></span>

<span data-ttu-id="11eba-171">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="11eba-171">**Old behavior**</span></span>

<span data-ttu-id="11eba-172">До EF Core версии 3.0 метод `FromSql` можно было определять в любом месте в запросе.</span><span class="sxs-lookup"><span data-stu-id="11eba-172">Before EF Core 3.0, the `FromSql` method could be specified anywhere in the query.</span></span>

<span data-ttu-id="11eba-173">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="11eba-173">**New behavior**</span></span>

<span data-ttu-id="11eba-174">Начиная с EF Core версии 3.0, новые методы `FromSqlRaw` и `FromSqlInterpolated` (которые заменяют `FromSql`) можно указывать только в корневых элементах запроса, т. е. непосредственно в `DbSet<>`.</span><span class="sxs-lookup"><span data-stu-id="11eba-174">Starting with EF Core 3.0, the new `FromSqlRaw` and `FromSqlInterpolated` methods (which replace `FromSql`) can only be specified on query roots, i.e. directly on the `DbSet<>`.</span></span> <span data-ttu-id="11eba-175">Попытка указать их в любом другом месте приведет к ошибке компиляции.</span><span class="sxs-lookup"><span data-stu-id="11eba-175">Attempting to specify them anywhere else will result in a compilation error.</span></span>

<span data-ttu-id="11eba-176">**Причина**</span><span class="sxs-lookup"><span data-stu-id="11eba-176">**Why**</span></span>

<span data-ttu-id="11eba-177">Определение `FromSql` в любом месте за пределами `DbSet` не предоставляет никаких преимуществ и может привести к неоднозначности в некоторых сценариях.</span><span class="sxs-lookup"><span data-stu-id="11eba-177">Specifying `FromSql` anywhere other than on a `DbSet` had no added meaning or added value, and could cause ambiguity in certain scenarios.</span></span>

<span data-ttu-id="11eba-178">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="11eba-178">**Mitigations**</span></span>

<span data-ttu-id="11eba-179">Вызовы `FromSql` нужно поместить непосредственно в элемент `DbSet`, к которому они применяются.</span><span class="sxs-lookup"><span data-stu-id="11eba-179">`FromSql` invocations should be moved to be directly on the `DbSet` to which they apply.</span></span>

## <a name="query-execution-is-logged-at-debug-level-reverted"></a><span data-ttu-id="11eba-180">~~Выполнение запроса с ведением журнала на уровне отладки Debug~~ отменено</span><span class="sxs-lookup"><span data-stu-id="11eba-180">~~Query execution is logged at Debug level~~ Reverted</span></span>

[<span data-ttu-id="11eba-181">Отслеживание вопроса 14523</span><span class="sxs-lookup"><span data-stu-id="11eba-181">Tracking Issue #14523</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14523)

<span data-ttu-id="11eba-182">Это изменение отменено в выпуске EF Core 3.0, предварительная версия 7.</span><span class="sxs-lookup"><span data-stu-id="11eba-182">This change is reverted in EF Core 3.0-preview 7.</span></span>

<span data-ttu-id="11eba-183">Причиной послужило то, что новая конфигурация EF Core 3.0 позволяет приложению указывать уровень ведения журнала для любого события.</span><span class="sxs-lookup"><span data-stu-id="11eba-183">We reverted this change because new configuration in EF Core 3.0 allows the log level for any event to be specified by the application.</span></span> <span data-ttu-id="11eba-184">Например, чтобы переключить ведение журналов с уровня SQL на `Debug`, явно настройте уровень в `OnConfiguring` или `AddDbContext`:</span><span class="sxs-lookup"><span data-stu-id="11eba-184">For example, to switch logging of SQL to `Debug`, explicitly configure the level in `OnConfiguring` or `AddDbContext`:</span></span>
```C#
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    => optionsBuilder
        .UseSqlServer(connectionString)
        .ConfigureWarnings(c => c.Log((RelationalEventId.CommandExecuting, LogLevel.Debug)));
```

## <a name="temporary-key-values-are-no-longer-set-onto-entity-instances"></a><span data-ttu-id="11eba-185">Временные значения ключа больше не устанавливаются для экземпляров сущностей</span><span class="sxs-lookup"><span data-stu-id="11eba-185">Temporary key values are no longer set onto entity instances</span></span>

[<span data-ttu-id="11eba-186">Отслеживание вопроса 12378</span><span class="sxs-lookup"><span data-stu-id="11eba-186">Tracking Issue #12378</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12378)

<span data-ttu-id="11eba-187">Это изменение реализовано в EF Core 3.0, предварительная версия 2.</span><span class="sxs-lookup"><span data-stu-id="11eba-187">This change is introduced in EF Core 3.0-preview 2.</span></span>

<span data-ttu-id="11eba-188">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="11eba-188">**Old behavior**</span></span>

<span data-ttu-id="11eba-189">До выхода EF Core 3.0 временные значения назначались всем свойствам ключей, которые позднее получили бы реальное значение, созданное базой данных.</span><span class="sxs-lookup"><span data-stu-id="11eba-189">Before EF Core 3.0, temporary values were assigned to all key properties that would later have a real value generated by the database.</span></span>
<span data-ttu-id="11eba-190">Обычно эти временные значения были большими отрицательными числами.</span><span class="sxs-lookup"><span data-stu-id="11eba-190">Usually these temporary values were large negative numbers.</span></span>

<span data-ttu-id="11eba-191">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="11eba-191">**New behavior**</span></span>

<span data-ttu-id="11eba-192">Начиная с версии 3.0 EF Core сохраняет временное значение ключа как часть сведений об отслеживании сущности, оставляя само свойство ключа без изменений.</span><span class="sxs-lookup"><span data-stu-id="11eba-192">Starting with 3.0, EF Core stores the temporary key value as part of the entity's tracking information, and leaves the key property itself unchanged.</span></span>

<span data-ttu-id="11eba-193">**Причина**</span><span class="sxs-lookup"><span data-stu-id="11eba-193">**Why**</span></span>

<span data-ttu-id="11eba-194">Это изменение было внесено для ситуации, когда временные значения ключей ошибочно становились постоянными из-за перемещения сущности, которая ранее отслеживалась некоторым экземпляром `DbContext`, в другой экземпляр `DbContext`.</span><span class="sxs-lookup"><span data-stu-id="11eba-194">This change was made to prevent temporary key values from erroneously becoming permanent when an entity that has been previously tracked by some `DbContext` instance is moved to a different `DbContext` instance.</span></span> 

<span data-ttu-id="11eba-195">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="11eba-195">**Mitigations**</span></span>

<span data-ttu-id="11eba-196">Приложения, которые назначают значения первичного ключа внешним ключам для формирования ассоциаций между сущностями, могут использовать старое поведение, если первичные ключи сформированы хранилищем и принадлежат сущностям в состоянии `Added`.</span><span class="sxs-lookup"><span data-stu-id="11eba-196">Applications that assign primary key values onto foreign keys to form associations between entities may depend on the old behavior if the primary keys are store-generated and belong to entities in the `Added` state.</span></span>
<span data-ttu-id="11eba-197">Этого можно избежать следующим образом:</span><span class="sxs-lookup"><span data-stu-id="11eba-197">This can be avoided by:</span></span>
* <span data-ttu-id="11eba-198">Отказ от использования ключей, сформированных хранилищем.</span><span class="sxs-lookup"><span data-stu-id="11eba-198">Not using store-generated keys.</span></span>
* <span data-ttu-id="11eba-199">Настройка свойств навигации для формирования отношений вместо задания значений внешнего ключа.</span><span class="sxs-lookup"><span data-stu-id="11eba-199">Setting navigation properties to form relationships instead of setting foreign key values.</span></span>
* <span data-ttu-id="11eba-200">Получение фактических временных значений ключа из сведений об отслеживании сущности.</span><span class="sxs-lookup"><span data-stu-id="11eba-200">Obtain the actual temporary key values from the entity's tracking information.</span></span>
<span data-ttu-id="11eba-201">Например, `context.Entry(blog).Property(e => e.Id).CurrentValue` возвратит временное значение, даже если сам `blog.Id` не был задан.</span><span class="sxs-lookup"><span data-stu-id="11eba-201">For example, `context.Entry(blog).Property(e => e.Id).CurrentValue` will return the temporary value even though `blog.Id` itself hasn't been set.</span></span>

## <a name="detectchanges-honors-store-generated-key-values"></a><span data-ttu-id="11eba-202">DetectChanges учитывает значения ключей, сформированные хранилищем</span><span class="sxs-lookup"><span data-stu-id="11eba-202">DetectChanges honors store-generated key values</span></span>

[<span data-ttu-id="11eba-203">Отслеживание вопроса 14616</span><span class="sxs-lookup"><span data-stu-id="11eba-203">Tracking Issue #14616</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14616)

<span data-ttu-id="11eba-204">Это изменение реализовано в EF Core 3.0, предварительная версия 3.</span><span class="sxs-lookup"><span data-stu-id="11eba-204">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="11eba-205">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="11eba-205">**Old behavior**</span></span>

<span data-ttu-id="11eba-206">До выпуска EF Core 3.0 неотслеживаемая сущность, обнаруженная `DetectChanges`, отслеживалась в состоянии `Added` и вставлялась в новую строку при вызове `SaveChanges`.</span><span class="sxs-lookup"><span data-stu-id="11eba-206">Before EF Core 3.0, an untracked entity found by `DetectChanges` would be tracked in the `Added` state and inserted as a new row when `SaveChanges` is called.</span></span>

<span data-ttu-id="11eba-207">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="11eba-207">**New behavior**</span></span>

<span data-ttu-id="11eba-208">Начиная с EF Core 3.0, если сущность использует сформированные значения ключа и какое-либо значение ключа задано, то она будет отслеживаться в состоянии `Modified`.</span><span class="sxs-lookup"><span data-stu-id="11eba-208">Starting with EF Core 3.0, if an entity is using generated key values and some key value is set, then the entity will be tracked in the `Modified` state.</span></span>
<span data-ttu-id="11eba-209">Это означает, что предполагается наличие строки для сущности, и она будет обновлена при вызове `SaveChanges`.</span><span class="sxs-lookup"><span data-stu-id="11eba-209">This means that a row for the entity is assumed to exist and it will be updated when `SaveChanges` is called.</span></span>
<span data-ttu-id="11eba-210">Если значение ключа не задано или тип сущности не использует сформированные ключи, то новая сущность все равно будет отслеживаться в состоянии `Added`, как в предыдущих версиях.</span><span class="sxs-lookup"><span data-stu-id="11eba-210">If the key value isn't set, or if the entity type isn't using generated keys, then the new entity will still be tracked as `Added` as in previous versions.</span></span>

<span data-ttu-id="11eba-211">**Причина**</span><span class="sxs-lookup"><span data-stu-id="11eba-211">**Why**</span></span>

<span data-ttu-id="11eba-212">Это изменение было внесено, чтобы сделать работу с несвязными графами сущностей при использовании ключей, сформированных хранилищем, более простой и согласованной.</span><span class="sxs-lookup"><span data-stu-id="11eba-212">This change was made to make it easier and more consistent to work with disconnected entity graphs while using store-generated keys.</span></span>

<span data-ttu-id="11eba-213">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="11eba-213">**Mitigations**</span></span>

<span data-ttu-id="11eba-214">Это изменение может нарушить работу приложения, если тип сущности должен использовать сформированные ключи, однако значения ключей явно заданы для новых экземпляров.</span><span class="sxs-lookup"><span data-stu-id="11eba-214">This change can break an application if an entity type is configured to use generated keys but key values are explicitly set for new instances.</span></span>
<span data-ttu-id="11eba-215">Решение проблемы заключается в явном запрете свойствам ключей использовать сформированные значения.</span><span class="sxs-lookup"><span data-stu-id="11eba-215">The fix is to explicitly configure the key properties to not use generated values.</span></span>
<span data-ttu-id="11eba-216">Например, с помощью текучего API:</span><span class="sxs-lookup"><span data-stu-id="11eba-216">For example, with the fluent API:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .ValueGeneratedNever();
```

<span data-ttu-id="11eba-217">Или с помощью заметок к данным:</span><span class="sxs-lookup"><span data-stu-id="11eba-217">Or with data annotations:</span></span>

```C#
[DatabaseGenerated(DatabaseGeneratedOption.None)]
public string Id { get; set; }
```

## <a name="cascade-deletions-now-happen-immediately-by-default"></a><span data-ttu-id="11eba-218">Каскадные удаления теперь по умолчанию выполняются немедленно</span><span class="sxs-lookup"><span data-stu-id="11eba-218">Cascade deletions now happen immediately by default</span></span>

[<span data-ttu-id="11eba-219">Отслеживание вопроса 10114</span><span class="sxs-lookup"><span data-stu-id="11eba-219">Tracking Issue #10114</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10114)

<span data-ttu-id="11eba-220">Это изменение реализовано в EF Core 3.0, предварительная версия 3.</span><span class="sxs-lookup"><span data-stu-id="11eba-220">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="11eba-221">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="11eba-221">**Old behavior**</span></span>

<span data-ttu-id="11eba-222">До выхода версии 3.0 применяемые EF Core каскадные действия (удаление зависимых сущностей, когда требуемый субъект удален либо отношение с ним было разорвано) не выполнялись до вызова SaveChanges.</span><span class="sxs-lookup"><span data-stu-id="11eba-222">Before 3.0, EF Core applied cascading actions (deleting dependent entities when a required principal is deleted or when the relationship to a required principal is severed) did not happen until SaveChanges was called.</span></span>

<span data-ttu-id="11eba-223">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="11eba-223">**New behavior**</span></span>

<span data-ttu-id="11eba-224">Начиная с версии 3.0 EF Core применяет каскадные действия сразу после обнаружения условия триггера.</span><span class="sxs-lookup"><span data-stu-id="11eba-224">Starting with 3.0, EF Core applies cascading actions as soon as the triggering condition is detected.</span></span>
<span data-ttu-id="11eba-225">Например, вызов `context.Remove()` для удаления сущности субъекта приведет к немедленной установке всех отслеживаемых связанных необходимых зависимых объектов в состояние `Deleted`.</span><span class="sxs-lookup"><span data-stu-id="11eba-225">For example, calling `context.Remove()` to delete a principal entity will result in all tracked related required dependents also being set to `Deleted` immediately.</span></span>

<span data-ttu-id="11eba-226">**Причина**</span><span class="sxs-lookup"><span data-stu-id="11eba-226">**Why**</span></span>

<span data-ttu-id="11eba-227">Это изменение было внесено, чтобы улучшить работу в сценариях аудита и привязки данных, где важно понимать, какие сущности будут удалены _перед_ вызовом `SaveChanges`.</span><span class="sxs-lookup"><span data-stu-id="11eba-227">This change was made to improve the experience for data binding and auditing scenarios where it is important to understand which entities will be deleted _before_ `SaveChanges` is called.</span></span>

<span data-ttu-id="11eba-228">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="11eba-228">**Mitigations**</span></span>

<span data-ttu-id="11eba-229">Для восстановления прежнего поведения можно использовать параметры `context.ChangedTracker`.</span><span class="sxs-lookup"><span data-stu-id="11eba-229">The previous behavior can be restored through settings on `context.ChangedTracker`.</span></span>
<span data-ttu-id="11eba-230">Например:</span><span class="sxs-lookup"><span data-stu-id="11eba-230">For example:</span></span>

```C#
context.ChangeTracker.CascadeDeleteTiming = CascadeTiming.OnSaveChanges;
context.ChangeTracker.DeleteOrphansTiming = CascadeTiming.OnSaveChanges;
```

## <a name="deletebehaviorrestrict-has-cleaner-semantics"></a><span data-ttu-id="11eba-231">более четкая семантика DeleteBehavior.Restrict</span><span class="sxs-lookup"><span data-stu-id="11eba-231">DeleteBehavior.Restrict has cleaner semantics</span></span>

[<span data-ttu-id="11eba-232">Отслеживание вопроса № 12661</span><span class="sxs-lookup"><span data-stu-id="11eba-232">Tracking Issue #12661</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12661)

<span data-ttu-id="11eba-233">Это изменение реализовано в EF Core 3.0, предварительная версия 5.</span><span class="sxs-lookup"><span data-stu-id="11eba-233">This change is introduced in EF Core 3.0-preview 5.</span></span>

<span data-ttu-id="11eba-234">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="11eba-234">**Old behavior**</span></span>

<span data-ttu-id="11eba-235">До версии 3.0 `DeleteBehavior.Restrict` создавал внешние ключи в базе данных с помощью семантики `Restrict`, но также изменял внутреннее исправление неочевидным образом.</span><span class="sxs-lookup"><span data-stu-id="11eba-235">Before 3.0, `DeleteBehavior.Restrict` created foreign keys in the database with `Restrict` semantics, but also changed internal fixup in a non-obvious way.</span></span>

<span data-ttu-id="11eba-236">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="11eba-236">**New behavior**</span></span>

<span data-ttu-id="11eba-237">Начиная с версии 3.0 `DeleteBehavior.Restrict` обеспечивает создание внешних ключей с помощью семантики `Restrict`, то есть без каскадов; создается при нарушении ограничения, не влияя на внутреннее исправление EF.</span><span class="sxs-lookup"><span data-stu-id="11eba-237">Starting with 3.0, `DeleteBehavior.Restrict` ensures that foreign keys are created with `Restrict` semantics--that is, no cascades; throw on constraint violation--without also impacting EF internal fixup.</span></span>

<span data-ttu-id="11eba-238">**Причина**</span><span class="sxs-lookup"><span data-stu-id="11eba-238">**Why**</span></span>

<span data-ttu-id="11eba-239">Это изменение было внесено, чтобы упростить работу с помощью более интуитивного использования `DeleteBehavior` без непредвиденных побочных эффектов.</span><span class="sxs-lookup"><span data-stu-id="11eba-239">This change was made to improve the experience for using `DeleteBehavior` in an intuitive manner, without unexpected side-effects.</span></span>

<span data-ttu-id="11eba-240">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="11eba-240">**Mitigations**</span></span>

<span data-ttu-id="11eba-241">Для восстановления прежнего поведения можно использовать `DeleteBehavior.ClientNoAction`.</span><span class="sxs-lookup"><span data-stu-id="11eba-241">The previous behavior can be restored by using `DeleteBehavior.ClientNoAction`.</span></span>

## <a name="query-types-are-consolidated-with-entity-types"></a><span data-ttu-id="11eba-242">Типы запросов объединяются с типами сущностей</span><span class="sxs-lookup"><span data-stu-id="11eba-242">Query types are consolidated with entity types</span></span>

[<span data-ttu-id="11eba-243">Отслеживание вопроса 14194</span><span class="sxs-lookup"><span data-stu-id="11eba-243">Tracking Issue #14194</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14194)

<span data-ttu-id="11eba-244">Это изменение реализовано в EF Core 3.0, предварительная версия 3.</span><span class="sxs-lookup"><span data-stu-id="11eba-244">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="11eba-245">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="11eba-245">**Old behavior**</span></span>

<span data-ttu-id="11eba-246">До выпуска EF Core 3.0 [типы запросов](xref:core/modeling/query-types) представляли собой средства для запроса данных, не определяющие первичный ключ структурированным образом.</span><span class="sxs-lookup"><span data-stu-id="11eba-246">Before EF Core 3.0, [query types](xref:core/modeling/query-types) were a means to query data that doesn't define a primary key in a structured way.</span></span>
<span data-ttu-id="11eba-247">То есть тип запроса использовался для сопоставления типов сущностей без ключей (скорее всего, из представления, но, возможно, и из таблицы), а обычный тип сущности использовался при наличии ключа (скорее всего, из таблицы, но, возможно, и из представления).</span><span class="sxs-lookup"><span data-stu-id="11eba-247">That is, a query type was used for mapping entity types without keys (more likely from a view, but possibly from a table) while a regular entity type was used when a key was available (more likely from a table, but possibly from a view).</span></span>

<span data-ttu-id="11eba-248">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="11eba-248">**New behavior**</span></span>

<span data-ttu-id="11eba-249">Теперь тип запроса стал просто типом сущности без первичного ключа.</span><span class="sxs-lookup"><span data-stu-id="11eba-249">A query type now becomes just an entity type without a primary key.</span></span>
<span data-ttu-id="11eba-250">Типы сущностей без ключей имеют ту же функциональность, что типы запросов в предыдущих версиях.</span><span class="sxs-lookup"><span data-stu-id="11eba-250">Keyless entity types have the same functionality as query types in previous versions.</span></span>

<span data-ttu-id="11eba-251">**Причина**</span><span class="sxs-lookup"><span data-stu-id="11eba-251">**Why**</span></span>

<span data-ttu-id="11eba-252">Это изменение было внесено, чтобы избежать путаницы с назначением типов запросов.</span><span class="sxs-lookup"><span data-stu-id="11eba-252">This change was made to reduce the confusion around the purpose of query types.</span></span>
<span data-ttu-id="11eba-253">В частности, они представляют собой типы сущностей без ключей и поэтому по определению доступны только для чтения, однако их не следует использовать только потому, что тип сущности должен быть доступен только для чтения.</span><span class="sxs-lookup"><span data-stu-id="11eba-253">Specifically, they are keyless entity types and they are inherently read-only because of this, but they should not be used just because an entity type needs to be read-only.</span></span>
<span data-ttu-id="11eba-254">Аналогично, их часто сопоставляют с представлениями, но только потому, что представления часто не определяют ключи.</span><span class="sxs-lookup"><span data-stu-id="11eba-254">Likewise, they are often mapped to views, but this is only because views often don't define keys.</span></span>

<span data-ttu-id="11eba-255">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="11eba-255">**Mitigations**</span></span>

<span data-ttu-id="11eba-256">Следующие компоненты этого API теперь являются устаревшими:</span><span class="sxs-lookup"><span data-stu-id="11eba-256">The following parts of the API are now obsolete:</span></span>
* <span data-ttu-id="11eba-257">**`ModelBuilder.Query<>()`** — вместо него следует вызвать `ModelBuilder.Entity<>().HasNoKey()`, чтобы пометить тип сущности как не имеющий ключей.</span><span class="sxs-lookup"><span data-stu-id="11eba-257">**`ModelBuilder.Query<>()`** - Instead `ModelBuilder.Entity<>().HasNoKey()` needs to be called to mark an entity type as having no keys.</span></span>
<span data-ttu-id="11eba-258">Это по-прежнему не следует настраивать посредством соглашения, чтобы избежать ошибки, когда первичный ключ ожидается, но не соответствует соглашению.</span><span class="sxs-lookup"><span data-stu-id="11eba-258">This would still not be configured by convention to avoid misconfiguration when a primary key is expected, but doesn't match the convention.</span></span>
* <span data-ttu-id="11eba-259">**`DbQuery<>`** — вместо него следует использовать `DbSet<>`.</span><span class="sxs-lookup"><span data-stu-id="11eba-259">**`DbQuery<>`** - Instead `DbSet<>` should be used.</span></span>
* <span data-ttu-id="11eba-260">**`DbContext.Query<>()`** — вместо него следует использовать `DbContext.Set<>()`.</span><span class="sxs-lookup"><span data-stu-id="11eba-260">**`DbContext.Query<>()`** - Instead `DbContext.Set<>()` should be used.</span></span>

## <a name="configuration-api-for-owned-type-relationships-has-changed"></a><span data-ttu-id="11eba-261">Изменение API конфигурации для отношений принадлежащего типа</span><span class="sxs-lookup"><span data-stu-id="11eba-261">Configuration API for owned type relationships has changed</span></span>

<span data-ttu-id="11eba-262">[Отслеживание вопроса 12444](https://github.com/aspnet/EntityFrameworkCore/issues/12444)
[Отслеживание вопроса 9148](https://github.com/aspnet/EntityFrameworkCore/issues/9148)
[Отслеживание вопроса 14153](https://github.com/aspnet/EntityFrameworkCore/issues/14153)</span><span class="sxs-lookup"><span data-stu-id="11eba-262">[Tracking Issue #12444](https://github.com/aspnet/EntityFrameworkCore/issues/12444)
[Tracking Issue #9148](https://github.com/aspnet/EntityFrameworkCore/issues/9148)
[Tracking Issue #14153](https://github.com/aspnet/EntityFrameworkCore/issues/14153)</span></span>

<span data-ttu-id="11eba-263">Это изменение реализовано в EF Core 3.0, предварительная версия 3.</span><span class="sxs-lookup"><span data-stu-id="11eba-263">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="11eba-264">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="11eba-264">**Old behavior**</span></span>

<span data-ttu-id="11eba-265">До выхода EF Core 3.0 конфигурация принадлежащего отношения выполнялась непосредственно после вызова `OwnsOne` или `OwnsMany`.</span><span class="sxs-lookup"><span data-stu-id="11eba-265">Before EF Core 3.0, configuration of the owned relationship was performed directly after the `OwnsOne` or `OwnsMany` call.</span></span> 

<span data-ttu-id="11eba-266">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="11eba-266">**New behavior**</span></span>

<span data-ttu-id="11eba-267">Начиная с EF Core 3.0 существует текучий API для настройки свойства навигации для владельца с помощью `WithOwner()`.</span><span class="sxs-lookup"><span data-stu-id="11eba-267">Starting with EF Core 3.0, there is now fluent API to configure a navigation property to the owner using `WithOwner()`.</span></span>
<span data-ttu-id="11eba-268">Например:</span><span class="sxs-lookup"><span data-stu-id="11eba-268">For example:</span></span>

```C#
modelBuilder.Entity<Order>.OwnsOne(e => e.Details).WithOwner(e => e.Order);
```

<span data-ttu-id="11eba-269">Конфигурация, связанная с отношением между владельцем и принадлежащим объектом, теперь должна быть включена в цепочку после `WithOwner()` по аналогии с настройкой других отношений.</span><span class="sxs-lookup"><span data-stu-id="11eba-269">The configuration related to the relationship between owner and owned should now be chained after `WithOwner()` similarly to how other relationships are configured.</span></span>
<span data-ttu-id="11eba-270">При этом конфигурация для принадлежащего типа сама будет включена в цепочку после `OwnsOne()/OwnsMany()`.</span><span class="sxs-lookup"><span data-stu-id="11eba-270">While the configuration for the owned type itself would still be chained after `OwnsOne()/OwnsMany()`.</span></span>
<span data-ttu-id="11eba-271">Например:</span><span class="sxs-lookup"><span data-stu-id="11eba-271">For example:</span></span>

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

<span data-ttu-id="11eba-272">Кроме того, вызов `Entity()`, `HasOne()` или `Set()` с целевым объектом принадлежащего типа теперь приведет к возникновению исключения.</span><span class="sxs-lookup"><span data-stu-id="11eba-272">Additionally calling `Entity()`, `HasOne()`, or `Set()` with an owned type target will now throw an exception.</span></span>

<span data-ttu-id="11eba-273">**Причина**</span><span class="sxs-lookup"><span data-stu-id="11eba-273">**Why**</span></span>

<span data-ttu-id="11eba-274">Это изменение было внесено для более четкого разграничения между настройкой самого принадлежащего типа и _отношения_ с ним.</span><span class="sxs-lookup"><span data-stu-id="11eba-274">This change was made to create a cleaner separation between configuring the owned type itself and the _relationship to_ the owned type.</span></span>
<span data-ttu-id="11eba-275">Это, в свою очередь, устраняет неоднозначность и путаницу при использовании таких методов, как `HasForeignKey`.</span><span class="sxs-lookup"><span data-stu-id="11eba-275">This in turn removes ambiguity and confusion around methods like `HasForeignKey`.</span></span>

<span data-ttu-id="11eba-276">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="11eba-276">**Mitigations**</span></span>

<span data-ttu-id="11eba-277">Измените конфигурацию отношений принадлежащего типа, чтобы использовать новую поверхность API, как показано в приведенном выше примере.</span><span class="sxs-lookup"><span data-stu-id="11eba-277">Change configuration of owned type relationships to use the new API surface as shown in the example above.</span></span>

## <a name="dependent-entities-sharing-the-table-with-the-principal-are-now-optional"></a><span data-ttu-id="11eba-278">Зависимые сущности, имеющие общую с субъектом таблицу, теперь являются необязательными</span><span class="sxs-lookup"><span data-stu-id="11eba-278">Dependent entities sharing the table with the principal are now optional</span></span>

[<span data-ttu-id="11eba-279">Отслеживание вопроса № 9005</span><span class="sxs-lookup"><span data-stu-id="11eba-279">Tracking Issue #9005</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9005)

<span data-ttu-id="11eba-280">Это изменение реализовано в EF Core 3.0, предварительная версия 4.</span><span class="sxs-lookup"><span data-stu-id="11eba-280">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="11eba-281">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="11eba-281">**Old behavior**</span></span>

<span data-ttu-id="11eba-282">Рассмотрим следующую модель:</span><span class="sxs-lookup"><span data-stu-id="11eba-282">Consider the following model:</span></span>
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
<span data-ttu-id="11eba-283">До выхода EF Core 3.0, если класс `OrderDetails` принадлежал классу `Order` или явно сопоставлялся с этой же таблицей, при добавлении нового класса `Order` всегда требовался экземпляр `OrderDetails`.</span><span class="sxs-lookup"><span data-stu-id="11eba-283">Before EF Core 3.0, if `OrderDetails` is owned by `Order` or explicitly mapped to the same table then an `OrderDetails` instance was always required when adding a new `Order`.</span></span>


<span data-ttu-id="11eba-284">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="11eba-284">**New behavior**</span></span>

<span data-ttu-id="11eba-285">Начиная с версии 3.0 система EF Core позволяет добавлять класс `Order` без класса `OrderDetails` и сопоставляет все свойства `OrderDetails`, за исключением первичного ключа, со столбцами, допускающими значение NULL.</span><span class="sxs-lookup"><span data-stu-id="11eba-285">Starting with 3.0, EF Core allows to add an `Order` without an `OrderDetails` and maps all of the `OrderDetails` properties except the primary key to nullable columns.</span></span>
<span data-ttu-id="11eba-286">При отправке запроса EF Core задает классу `OrderDetails` значение `null`, если какому-либо его обязательному свойству не задано значение или если отсутствуют необходимые свойства помимо первичного ключа и все свойства имеют значение `null`.</span><span class="sxs-lookup"><span data-stu-id="11eba-286">When querying EF Core sets `OrderDetails` to `null` if any of its required properties doesn't have a value or if it has no required properties besides the primary key and all properties are `null`.</span></span>

<span data-ttu-id="11eba-287">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="11eba-287">**Mitigations**</span></span>

<span data-ttu-id="11eba-288">Если модель содержит общую таблицу со всеми необязательными столбцами, но указывающее на нее свойство навигации не должно иметь значение `null`, приложение следует изменить для обработки случаев, когда свойству навигации задано значение `null`.</span><span class="sxs-lookup"><span data-stu-id="11eba-288">If your model has a table sharing dependent with all optional columns, but the navigation pointing to it is not expected to be `null` then the application should be modified to handle cases when the navigation is `null`.</span></span> <span data-ttu-id="11eba-289">Если это невозможно, необходимо добавить обязательное свойство к типу сущности либо по меньшей мере одному свойству должно быть задано значение, отличное от `null`.</span><span class="sxs-lookup"><span data-stu-id="11eba-289">If this is not possible a required property should be added to the entity type or at least one property should have a non-`null` value assigned to it.</span></span>

## <a name="all-entities-sharing-a-table-with-a-concurrency-token-column-have-to-map-it-to-a-property"></a><span data-ttu-id="11eba-290">Все сущности, имеющие общую таблицу со столбцом маркера параллелизма, должны сопоставлять ее со свойством</span><span class="sxs-lookup"><span data-stu-id="11eba-290">All entities sharing a table with a concurrency token column have to map it to a property</span></span>

[<span data-ttu-id="11eba-291">Отслеживание вопроса № 14154</span><span class="sxs-lookup"><span data-stu-id="11eba-291">Tracking Issue #14154</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14154)

<span data-ttu-id="11eba-292">Это изменение реализовано в EF Core 3.0, предварительная версия 4.</span><span class="sxs-lookup"><span data-stu-id="11eba-292">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="11eba-293">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="11eba-293">**Old behavior**</span></span>

<span data-ttu-id="11eba-294">Рассмотрим следующую модель:</span><span class="sxs-lookup"><span data-stu-id="11eba-294">Consider the following model:</span></span>
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
<span data-ttu-id="11eba-295">До выхода EF Core 3.0, если класс `OrderDetails` принадлежал классу `Order` или явно сопоставлялся с той же таблицей, то обновление только класса `OrderDetails` не приводило к обновлению значения `Version` на клиенте и следующее обновление завершалось ошибкой.</span><span class="sxs-lookup"><span data-stu-id="11eba-295">Before EF Core 3.0, if `OrderDetails` is owned by `Order` or explicitly mapped to the same table then updating just `OrderDetails` will not update `Version` value on client and the next update will fail.</span></span>


<span data-ttu-id="11eba-296">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="11eba-296">**New behavior**</span></span>

<span data-ttu-id="11eba-297">Начиная с версии 3.0 система EF Core распространяет новое значение `Version` для класса `Order`, если ему принадлежит класс `OrderDetails`.</span><span class="sxs-lookup"><span data-stu-id="11eba-297">Starting with 3.0, EF Core propagates the new `Version` value to `Order` if it owns `OrderDetails`.</span></span> <span data-ttu-id="11eba-298">В противном случае во время проверки модели создается исключение.</span><span class="sxs-lookup"><span data-stu-id="11eba-298">Otherwise an exception is thrown during model validation.</span></span>

<span data-ttu-id="11eba-299">**Причина**</span><span class="sxs-lookup"><span data-stu-id="11eba-299">**Why**</span></span>

<span data-ttu-id="11eba-300">Это изменение было внесено, чтобы избежать получения устаревшего значения маркера параллелизма при обновлении только одной сущности, сопоставленной с той же таблицей.</span><span class="sxs-lookup"><span data-stu-id="11eba-300">This change was made to avoid a stale concurrency token value when only one of the entities mapped to the same table is updated.</span></span>

<span data-ttu-id="11eba-301">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="11eba-301">**Mitigations**</span></span>

<span data-ttu-id="11eba-302">Все сущности, имеющие общую таблицу, должны включать в себя свойство, сопоставленное со столбцом маркера параллелизма.</span><span class="sxs-lookup"><span data-stu-id="11eba-302">All entities sharing the table have to include a property that is mapped to the concurrency token column.</span></span> <span data-ttu-id="11eba-303">Можно создать свойство в теневом состоянии:</span><span class="sxs-lookup"><span data-stu-id="11eba-303">It's possible the create one in shadow-state:</span></span>
```C#
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<OrderDetails>()
        .Property<byte[]>("Version").IsRowVersion().HasColumnName("Version");
}
```

## <a name="inherited-properties-from-unmapped-types-are-now-mapped-to-a-single-column-for-all-derived-types"></a><span data-ttu-id="11eba-304">Наследуемые свойства из несопоставленных типов теперь сопоставляются с одним столбцом для всех производных типов</span><span class="sxs-lookup"><span data-stu-id="11eba-304">Inherited properties from unmapped types are now mapped to a single column for all derived types</span></span>

[<span data-ttu-id="11eba-305">Отслеживание вопроса № 13998</span><span class="sxs-lookup"><span data-stu-id="11eba-305">Tracking Issue #13998</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13998)

<span data-ttu-id="11eba-306">Это изменение реализовано в EF Core 3.0, предварительная версия 4.</span><span class="sxs-lookup"><span data-stu-id="11eba-306">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="11eba-307">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="11eba-307">**Old behavior**</span></span>

<span data-ttu-id="11eba-308">Рассмотрим следующую модель:</span><span class="sxs-lookup"><span data-stu-id="11eba-308">Consider the following model:</span></span>
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

<span data-ttu-id="11eba-309">До выхода EF Core 3.0 свойство `ShippingAddress` сопоставлялось с отдельными столбцами для классов `BulkOrder` и `Order` по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="11eba-309">Before EF Core 3.0, the `ShippingAddress` property would be mapped to separate columns for `BulkOrder` and `Order` by default.</span></span>

<span data-ttu-id="11eba-310">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="11eba-310">**New behavior**</span></span>

<span data-ttu-id="11eba-311">Начиная с версии 3.0 система EF Core создает только один столбец для класса `ShippingAddress`.</span><span class="sxs-lookup"><span data-stu-id="11eba-311">Starting with 3.0, EF Core only creates one column for `ShippingAddress`.</span></span>

<span data-ttu-id="11eba-312">**Причина**</span><span class="sxs-lookup"><span data-stu-id="11eba-312">**Why**</span></span>

<span data-ttu-id="11eba-313">Старое поведение было непредвиденным.</span><span class="sxs-lookup"><span data-stu-id="11eba-313">The old behavoir was unexpected.</span></span>

<span data-ttu-id="11eba-314">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="11eba-314">**Mitigations**</span></span>

<span data-ttu-id="11eba-315">Свойство можно по-прежнему явным образом сопоставлять с отдельным столбцом для производных типов.</span><span class="sxs-lookup"><span data-stu-id="11eba-315">The property can still be explicitly mapped to separate column on the derived types:</span></span>

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

## <a name="the-foreign-key-property-convention-no-longer-matches-same-name-as-the-principal-property"></a><span data-ttu-id="11eba-316">Соглашение для свойства внешнего ключа больше не совпадает со свойством субъекта по имени</span><span class="sxs-lookup"><span data-stu-id="11eba-316">The foreign key property convention no longer matches same name as the principal property</span></span>

[<span data-ttu-id="11eba-317">Отслеживание вопроса 13274</span><span class="sxs-lookup"><span data-stu-id="11eba-317">Tracking Issue #13274</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13274)

<span data-ttu-id="11eba-318">Это изменение реализовано в EF Core 3.0, предварительная версия 3.</span><span class="sxs-lookup"><span data-stu-id="11eba-318">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="11eba-319">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="11eba-319">**Old behavior**</span></span>

<span data-ttu-id="11eba-320">Рассмотрим следующую модель:</span><span class="sxs-lookup"><span data-stu-id="11eba-320">Consider the following model:</span></span>
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
<span data-ttu-id="11eba-321">До выхода EF Core 3.0 свойство `CustomerId` использовалось для внешнего ключа по соглашению.</span><span class="sxs-lookup"><span data-stu-id="11eba-321">Before EF Core 3.0, the `CustomerId` property would be used for the foreign key by convention.</span></span>
<span data-ttu-id="11eba-322">Однако если `Order` является принадлежащим типом, это также делает `CustomerId` первичным ключом, что обычно не соответствует ожидаемому результату.</span><span class="sxs-lookup"><span data-stu-id="11eba-322">However, if `Order` is an owned type, then this would also make `CustomerId` the primary key and this isn't usually the expectation.</span></span>

<span data-ttu-id="11eba-323">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="11eba-323">**New behavior**</span></span>

<span data-ttu-id="11eba-324">Начиная с версии 3.0 система EF Core не будет пытаться использовать свойства для внешних ключей по соглашению, если они имеют такое же имя, что и свойство субъекта.</span><span class="sxs-lookup"><span data-stu-id="11eba-324">Starting with 3.0, EF Core doesn't try to use properties for foreign keys by convention if they have the same name as the principal property.</span></span>
<span data-ttu-id="11eba-325">Шаблоны имени типа субъекта, сцепленного с именем свойства субъекта, и имени навигации, сцепленного с именем свойства субъекта, по-прежнему совпадают.</span><span class="sxs-lookup"><span data-stu-id="11eba-325">Principal type name concatenated with principal property name, and navigation name concatenated with principal property name patterns are still matched.</span></span>
<span data-ttu-id="11eba-326">Например:</span><span class="sxs-lookup"><span data-stu-id="11eba-326">For example:</span></span>

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

<span data-ttu-id="11eba-327">**Причина**</span><span class="sxs-lookup"><span data-stu-id="11eba-327">**Why**</span></span>

<span data-ttu-id="11eba-328">Это изменение было внесено во избежание ошибочного определения свойств первичного ключа для принадлежащего типа.</span><span class="sxs-lookup"><span data-stu-id="11eba-328">This change was made to avoid erroneously defining a primary key property on the owned type.</span></span>

<span data-ttu-id="11eba-329">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="11eba-329">**Mitigations**</span></span>

<span data-ttu-id="11eba-330">Если свойство должно быть внешним ключом и, следовательно, частью первичного ключа, следует явно указать это при его настройке.</span><span class="sxs-lookup"><span data-stu-id="11eba-330">If the property was intended to be the foreign key, and hence part of the primary key, then explicitly configure it as such.</span></span>

## <a name="database-connection-is-now-closed-if-not-used-anymore-before-the-transactionscope-has-been-completed"></a><span data-ttu-id="11eba-331">Соединение с базой данных теперь закрывается, если оно больше не используется до завершения TransactionScope</span><span class="sxs-lookup"><span data-stu-id="11eba-331">Database connection is now closed if not used anymore before the TransactionScope has been completed</span></span>

[<span data-ttu-id="11eba-332">Отслеживание вопроса № 14218</span><span class="sxs-lookup"><span data-stu-id="11eba-332">Tracking Issue #14218</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14218)

<span data-ttu-id="11eba-333">Это изменение реализовано в EF Core 3.0, предварительная версия 4.</span><span class="sxs-lookup"><span data-stu-id="11eba-333">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="11eba-334">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="11eba-334">**Old behavior**</span></span>

<span data-ttu-id="11eba-335">До выхода EF Core 3.0, если контекст открывает соединение внутри класса `TransactionScope`, оно остается открытым пока активен текущий класс `TransactionScope`.</span><span class="sxs-lookup"><span data-stu-id="11eba-335">Before EF Core 3.0, if the context opens the connection inside a `TransactionScope`, the connection remains open while the current `TransactionScope` is active.</span></span>

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

<span data-ttu-id="11eba-336">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="11eba-336">**New behavior**</span></span>

<span data-ttu-id="11eba-337">Начиная с версии 3.0 система EF Core закрывает соединение сразу же после прекращения его использования.</span><span class="sxs-lookup"><span data-stu-id="11eba-337">Starting with 3.0, EF Core closes the connection as soon as it's done using it.</span></span>

<span data-ttu-id="11eba-338">**Причина**</span><span class="sxs-lookup"><span data-stu-id="11eba-338">**Why**</span></span>

<span data-ttu-id="11eba-339">Это изменение позволяет использовать несколько контекстов в одном классе `TransactionScope`.</span><span class="sxs-lookup"><span data-stu-id="11eba-339">This change allows to use multiple contexts in the same `TransactionScope`.</span></span> <span data-ttu-id="11eba-340">Новое поведение также соответствует EF6.</span><span class="sxs-lookup"><span data-stu-id="11eba-340">The new behavior also matches EF6.</span></span>

<span data-ttu-id="11eba-341">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="11eba-341">**Mitigations**</span></span>

<span data-ttu-id="11eba-342">Если соединение должно оставаться открытым, явный вызов метода `OpenConnection()` гарантирует, что EF Core не закроет его преждевременно.</span><span class="sxs-lookup"><span data-stu-id="11eba-342">If the connection needs to remain open explicit call to `OpenConnection()` will ensure that EF Core doesn't close it prematurely:</span></span>

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

## <a name="each-property-uses-independent-in-memory-integer-key-generation"></a><span data-ttu-id="11eba-343">Каждое свойство использует создание независимых целочисленных ключей в памяти</span><span class="sxs-lookup"><span data-stu-id="11eba-343">Each property uses independent in-memory integer key generation</span></span>

[<span data-ttu-id="11eba-344">Отслеживание вопроса 6872</span><span class="sxs-lookup"><span data-stu-id="11eba-344">Tracking Issue #6872</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/6872)

<span data-ttu-id="11eba-345">Это изменение реализовано в EF Core 3.0, предварительная версия 4.</span><span class="sxs-lookup"><span data-stu-id="11eba-345">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="11eba-346">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="11eba-346">**Old behavior**</span></span>

<span data-ttu-id="11eba-347">До выхода EF Core 3.0 для всех свойств целочисленных ключей в памяти использовался один общий генератор значений.</span><span class="sxs-lookup"><span data-stu-id="11eba-347">Before EF Core 3.0, one shared value generator was used for all in-memory integer key properties.</span></span>

<span data-ttu-id="11eba-348">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="11eba-348">**New behavior**</span></span>

<span data-ttu-id="11eba-349">Начиная с EF Core 3.0 при использовании выполняющейся в памяти базы данных каждое свойство целочисленного ключа получает свой собственный генератор значений.</span><span class="sxs-lookup"><span data-stu-id="11eba-349">Starting with EF Core 3.0, each integer key property gets its own value generator when using the in-memory database.</span></span>
<span data-ttu-id="11eba-350">Кроме того, при удалении базы данных формирование ключа сбрасывается для всех таблиц.</span><span class="sxs-lookup"><span data-stu-id="11eba-350">Also, if the database is deleted, then key generation is reset for all tables.</span></span>

<span data-ttu-id="11eba-351">**Причина**</span><span class="sxs-lookup"><span data-stu-id="11eba-351">**Why**</span></span>

<span data-ttu-id="11eba-352">Это изменение было внесено, чтобы лучше согласовать формирование ключей в памяти с реальным процессом в базе данных, а также улучшить возможность изоляции тестов друг от друга при использовании выполняющейся в памяти базы данных.</span><span class="sxs-lookup"><span data-stu-id="11eba-352">This change was made to align in-memory key generation more closely to real database key generation and to improve the ability to isolate tests from each other when using the in-memory database.</span></span>

<span data-ttu-id="11eba-353">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="11eba-353">**Mitigations**</span></span>

<span data-ttu-id="11eba-354">Это изменение может нарушить работу приложения, полагающегося на задание определенных значений ключа в памяти.</span><span class="sxs-lookup"><span data-stu-id="11eba-354">This can break an application that is relying on specific in-memory key values to be set.</span></span>
<span data-ttu-id="11eba-355">Рекомендуется не полагаться на конкретные значения ключей либо реализовать новое поведение.</span><span class="sxs-lookup"><span data-stu-id="11eba-355">Consider instead not relying on specific key values, or updating to match the new behavior.</span></span>

## <a name="backing-fields-are-used-by-default"></a><span data-ttu-id="11eba-356">По умолчанию используются резервные поля</span><span class="sxs-lookup"><span data-stu-id="11eba-356">Backing fields are used by default</span></span>

[<span data-ttu-id="11eba-357">Отслеживание вопроса 12430</span><span class="sxs-lookup"><span data-stu-id="11eba-357">Tracking Issue #12430</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12430)

<span data-ttu-id="11eba-358">Это изменение реализовано в EF Core 3.0, предварительная версия 2.</span><span class="sxs-lookup"><span data-stu-id="11eba-358">This change is introduced in EF Core 3.0-preview 2.</span></span>

<span data-ttu-id="11eba-359">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="11eba-359">**Old behavior**</span></span>

<span data-ttu-id="11eba-360">До выхода версии 3.0, даже если резервное поле для свойства было известно, система EF Core все равно по умолчанию считывала и записывала значение свойства с помощью методов получения и задания свойства.</span><span class="sxs-lookup"><span data-stu-id="11eba-360">Before 3.0, even if the backing field for a property was known, EF Core would still by default read and write the property value using the property getter and setter methods.</span></span>
<span data-ttu-id="11eba-361">Исключением из этого было выполнение запроса, где известное резервное поле устанавливается напрямую.</span><span class="sxs-lookup"><span data-stu-id="11eba-361">The exception to this was query execution, where the backing field would be set directly if known.</span></span>

<span data-ttu-id="11eba-362">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="11eba-362">**New behavior**</span></span>

<span data-ttu-id="11eba-363">Начиная с EF Core 3.0, если резервное поле для свойства известно, то EF Core всегда будет использовать его для чтения и записи этого свойства.</span><span class="sxs-lookup"><span data-stu-id="11eba-363">Starting with EF Core 3.0, if the backing field for a property is known, then EF Core will always read and write that property using the backing field.</span></span>
<span data-ttu-id="11eba-364">Это может нарушить работу приложения, если оно полагается на дополнительное поведение, закодированное в методы получения и задания.</span><span class="sxs-lookup"><span data-stu-id="11eba-364">This could cause an application break if the application is relying on additional behavior coded into the getter or setter methods.</span></span>

<span data-ttu-id="11eba-365">**Причина**</span><span class="sxs-lookup"><span data-stu-id="11eba-365">**Why**</span></span>

<span data-ttu-id="11eba-366">Это изменение было внесено, чтобы не позволить EF Core ошибочно активировать бизнес-логику по умолчанию при выполнении операций базы данных, затрагивающих сущности.</span><span class="sxs-lookup"><span data-stu-id="11eba-366">This change was made to prevent EF Core from erroneously triggering business logic by default when performing database operations involving the entities.</span></span>

<span data-ttu-id="11eba-367">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="11eba-367">**Mitigations**</span></span>

<span data-ttu-id="11eba-368">Поведение, характерное для версий до 3.0, можно восстановить, настроив режим доступа в `ModelBuilder`.</span><span class="sxs-lookup"><span data-stu-id="11eba-368">The pre-3.0 behavior can be restored through configuration of the property access mode on `ModelBuilder`.</span></span>
<span data-ttu-id="11eba-369">Например:</span><span class="sxs-lookup"><span data-stu-id="11eba-369">For example:</span></span>

```C#
modelBuilder.UsePropertyAccessMode(PropertyAccessMode.PreferFieldDuringConstruction);
```

## <a name="throw-if-multiple-compatible-backing-fields-are-found"></a><span data-ttu-id="11eba-370">Исключение при обнаружении нескольких совместимых резервных полей</span><span class="sxs-lookup"><span data-stu-id="11eba-370">Throw if multiple compatible backing fields are found</span></span>

[<span data-ttu-id="11eba-371">Отслеживание вопроса 12523</span><span class="sxs-lookup"><span data-stu-id="11eba-371">Tracking Issue #12523</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12523)

<span data-ttu-id="11eba-372">Это изменение реализовано в EF Core 3.0, предварительная версия 4.</span><span class="sxs-lookup"><span data-stu-id="11eba-372">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="11eba-373">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="11eba-373">**Old behavior**</span></span>

<span data-ttu-id="11eba-374">До выхода EF Core 3.0, если правила, обеспечивающие поиск резервного поля для свойства, находили несколько полей, выбиралось одно из них на основе определенной очередности.</span><span class="sxs-lookup"><span data-stu-id="11eba-374">Before EF Core 3.0, if multiple fields matched the rules for finding the backing field of a property, then one field would be chosen based on some precedence order.</span></span>
<span data-ttu-id="11eba-375">В неоднозначных случаях это могло привести к использованию неправильного поля.</span><span class="sxs-lookup"><span data-stu-id="11eba-375">This could cause the wrong field to be used in ambiguous cases.</span></span>

<span data-ttu-id="11eba-376">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="11eba-376">**New behavior**</span></span>

<span data-ttu-id="11eba-377">Начиная с EF Core 3.0 при обнаружении нескольких полей для одного свойства возникает исключение.</span><span class="sxs-lookup"><span data-stu-id="11eba-377">Starting with EF Core 3.0, if multiple fields are matched to the same property, then an exception is thrown.</span></span>

<span data-ttu-id="11eba-378">**Причина**</span><span class="sxs-lookup"><span data-stu-id="11eba-378">**Why**</span></span>

<span data-ttu-id="11eba-379">Это изменение было внесено во избежание автоматического использования одного поля вместо другого, когда правильным может быть только одно из них.</span><span class="sxs-lookup"><span data-stu-id="11eba-379">This change was made to avoid silently using one field over another when only one can be correct.</span></span>

<span data-ttu-id="11eba-380">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="11eba-380">**Mitigations**</span></span>

<span data-ttu-id="11eba-381">Для свойств с неоднозначными резервными полями используемое поле должно быть задано явным образом.</span><span class="sxs-lookup"><span data-stu-id="11eba-381">Properties with ambiguous backing fields must have the field to use specified explicitly.</span></span>
<span data-ttu-id="11eba-382">Например, с помощью текучего API:</span><span class="sxs-lookup"><span data-stu-id="11eba-382">For example, using the fluent API:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .HasField("_id");
```

## <a name="field-only-property-names-should-match-the-field-name"></a><span data-ttu-id="11eba-383">Имена свойств, доступных только для полей, должны совпадать с именем поля.</span><span class="sxs-lookup"><span data-stu-id="11eba-383">Field-only property names should match the field name</span></span>

<span data-ttu-id="11eba-384">Это изменение реализовано в EF Core 3.0, предварительная версия 4.</span><span class="sxs-lookup"><span data-stu-id="11eba-384">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="11eba-385">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="11eba-385">**Old behavior**</span></span>

<span data-ttu-id="11eba-386">До EF Core 3.0 свойство можно было указать с помощью строкового значения. Если свойство с таким именем не удавалось найти в типе CLR, компонент EF Core пытался сопоставить его с полем, используя правила соглашения.</span><span class="sxs-lookup"><span data-stu-id="11eba-386">Before EF Core 3.0, a property could be specified by a string value and if no property with that name was found on the CLR type then EF Core would try to match it to a field using convention rules.</span></span>
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

<span data-ttu-id="11eba-387">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="11eba-387">**New behavior**</span></span>

<span data-ttu-id="11eba-388">Начиная с EF Core 3.0 имя свойства, доступного только для полей, должно точно соответствовать имени поля.</span><span class="sxs-lookup"><span data-stu-id="11eba-388">Starting with EF Core 3.0, a field-only property must match the field name exactly.</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property("_id");
```

<span data-ttu-id="11eba-389">**Причина**</span><span class="sxs-lookup"><span data-stu-id="11eba-389">**Why**</span></span>

<span data-ttu-id="11eba-390">Это изменение было внесено, чтобы избежать использования одного поля для двух свойств с похожими именами, кроме того, правила сопоставления для свойств, доступных только для полей, теперь соответствуют правилам для свойств, сопоставленных со свойствами CLR.</span><span class="sxs-lookup"><span data-stu-id="11eba-390">This change was made to avoid using the same field for two properties named similarly, it also makes the matching rules for field-only properties the same as for properties mapped to CLR properties.</span></span>

<span data-ttu-id="11eba-391">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="11eba-391">**Mitigations**</span></span>

<span data-ttu-id="11eba-392">Имена свойств, доступных только для полей, должны совпадать с именем поля, с которым они сопоставляются.</span><span class="sxs-lookup"><span data-stu-id="11eba-392">Field-only properties must be named the same as the field they are mapped to.</span></span>
<span data-ttu-id="11eba-393">В последующей предварительной версии EF Core 3.0 мы планируем снова включить явную настройку имени поля, которое отличается от имени свойства:</span><span class="sxs-lookup"><span data-stu-id="11eba-393">In a later preview of EF Core 3.0, we plan to re-enable explicitly configuring a field name that is different from the property name:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property("Id")
    .HasField("_id");
```

## <a name="adddbcontextadddbcontextpool-no-longer-call-addlogging-and-addmemorycache"></a><span data-ttu-id="11eba-394">AddDbContext/AddDbContextPool больше не вызывает метод AddLogging и AddMemoryCache</span><span class="sxs-lookup"><span data-stu-id="11eba-394">AddDbContext/AddDbContextPool no longer call AddLogging and AddMemoryCache</span></span>

[<span data-ttu-id="11eba-395">Отслеживание вопроса № 14756</span><span class="sxs-lookup"><span data-stu-id="11eba-395">Tracking Issue #14756</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14756)

<span data-ttu-id="11eba-396">Это изменение реализовано в EF Core 3.0, предварительная версия 4.</span><span class="sxs-lookup"><span data-stu-id="11eba-396">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="11eba-397">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="11eba-397">**Old behavior**</span></span>

<span data-ttu-id="11eba-398">До EF Core 3.0 вызов метода `AddDbContext` или `AddDbContextPool` также приводил к регистрации служб ведения журналов и кэширования памяти с внедрением зависимостей с помощью вызовов к методам [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) и [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span><span class="sxs-lookup"><span data-stu-id="11eba-398">Before EF Core 3.0, calling `AddDbContext` or `AddDbContextPool` would also register logging and memory caching services with D.I through calls to [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) and [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span></span>

<span data-ttu-id="11eba-399">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="11eba-399">**New behavior**</span></span>

<span data-ttu-id="11eba-400">Начиная с версии EF Core 3.0 методы `AddDbContext` и `AddDbContextPool` больше не регистрируют такие службы с внедрением зависимостей.</span><span class="sxs-lookup"><span data-stu-id="11eba-400">Starting with EF Core 3.0, `AddDbContext` and `AddDbContextPool` will no longer register these services with Dependency Injection (DI).</span></span>

<span data-ttu-id="11eba-401">**Причина**</span><span class="sxs-lookup"><span data-stu-id="11eba-401">**Why**</span></span>

<span data-ttu-id="11eba-402">Для EF Core 3.0 не требуется наличие таких служб в контейнере внедрения зависимостей приложения.</span><span class="sxs-lookup"><span data-stu-id="11eba-402">EF Core 3.0 does not require that these services are in the application's DI container.</span></span> <span data-ttu-id="11eba-403">Но если интерфейс `ILoggerFactory` зарегистрирован в контейнере внедрения зависимостей приложения, он будет по-прежнему использоваться EF Core.</span><span class="sxs-lookup"><span data-stu-id="11eba-403">However, if `ILoggerFactory` is registered in the application's DI container, then it will still be used by EF Core.</span></span>

<span data-ttu-id="11eba-404">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="11eba-404">**Mitigations**</span></span>

<span data-ttu-id="11eba-405">Если для работы вашего приложения нужны такие службы, явно зарегистрируйте их в контейнере внедрения зависимостей с помощью метода [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) или [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span><span class="sxs-lookup"><span data-stu-id="11eba-405">If your application needs these services, then register them explicitly with the DI container using  [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) or [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span></span>

## <a name="dbcontextentry-now-performs-a-local-detectchanges"></a><span data-ttu-id="11eba-406">DbContext.Entry теперь выполняет локальную процедуру DetectChanges</span><span class="sxs-lookup"><span data-stu-id="11eba-406">DbContext.Entry now performs a local DetectChanges</span></span>

[<span data-ttu-id="11eba-407">Отслеживание вопроса 13552</span><span class="sxs-lookup"><span data-stu-id="11eba-407">Tracking Issue #13552</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13552)

<span data-ttu-id="11eba-408">Это изменение реализовано в EF Core 3.0, предварительная версия 3.</span><span class="sxs-lookup"><span data-stu-id="11eba-408">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="11eba-409">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="11eba-409">**Old behavior**</span></span>

<span data-ttu-id="11eba-410">До выхода EF Core 3.0 вызов `DbContext.Entry` приводил к обнаружению изменений для всех отслеживаемых сущностей.</span><span class="sxs-lookup"><span data-stu-id="11eba-410">Before EF Core 3.0, calling `DbContext.Entry` would cause changes to be detected for all tracked entities.</span></span>
<span data-ttu-id="11eba-411">Это гарантировало, что состояние, представленное в `EntityEntry`, было актуальным.</span><span class="sxs-lookup"><span data-stu-id="11eba-411">This ensured that the state exposed in the `EntityEntry` was up-to-date.</span></span>

<span data-ttu-id="11eba-412">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="11eba-412">**New behavior**</span></span>

<span data-ttu-id="11eba-413">Начиная с EF Core 3.0 при вызове `DbContext.Entry` будет предприниматься попытка обнаружить изменения только в заданной сущности и всех связанных с ней отслеживаемых сущностях субъектов.</span><span class="sxs-lookup"><span data-stu-id="11eba-413">Starting with EF Core 3.0, calling `DbContext.Entry` will now only attempt to detect changes in the given entity and any tracked principal entities related to it.</span></span>
<span data-ttu-id="11eba-414">Это означает, что изменения в другом месте могут не обнаруживаться при вызове этого метода, что может негативно повлиять на состояние приложения.</span><span class="sxs-lookup"><span data-stu-id="11eba-414">This means that changes elsewhere may not have been detected by calling this method, which could have implications on application state.</span></span>

<span data-ttu-id="11eba-415">Обратите внимание, что если для `ChangeTracker.AutoDetectChangesEnabled` задано значение `false`, то будет отключено даже такое локальное обнаружение изменений.</span><span class="sxs-lookup"><span data-stu-id="11eba-415">Note that if `ChangeTracker.AutoDetectChangesEnabled` is set to `false` then even this local change detection will be disabled.</span></span>

<span data-ttu-id="11eba-416">Другие методы, вызывающие обнаружение изменений, например `ChangeTracker.Entries` и `SaveChanges`, по-прежнему подразумевают полную процедуру `DetectChanges` для всех отслеживаемых сущностей.</span><span class="sxs-lookup"><span data-stu-id="11eba-416">Other methods that cause change detection--for example `ChangeTracker.Entries` and `SaveChanges`--still cause a full `DetectChanges` of all tracked entities.</span></span>

<span data-ttu-id="11eba-417">**Причина**</span><span class="sxs-lookup"><span data-stu-id="11eba-417">**Why**</span></span>

<span data-ttu-id="11eba-418">Это изменение было внесено для повышения производительности по умолчанию при использовании `context.Entry`.</span><span class="sxs-lookup"><span data-stu-id="11eba-418">This change was made to improve the default performance of using `context.Entry`.</span></span>

<span data-ttu-id="11eba-419">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="11eba-419">**Mitigations**</span></span>

<span data-ttu-id="11eba-420">Вызовите `ChgangeTracker.DetectChanges()` явным образом перед вызовом `Entry`, чтобы обеспечить поведение, предшествовавшее выходу версии 3.0.</span><span class="sxs-lookup"><span data-stu-id="11eba-420">Call `ChgangeTracker.DetectChanges()` explicitly before calling `Entry` to ensure the pre-3.0 behavior.</span></span>

## <a name="string-and-byte-array-keys-are-not-client-generated-by-default"></a><span data-ttu-id="11eba-421">Ключи массива строк и байтов не формируются клиентом по умолчанию</span><span class="sxs-lookup"><span data-stu-id="11eba-421">String and byte array keys are not client-generated by default</span></span>

[<span data-ttu-id="11eba-422">Отслеживание вопроса 14617</span><span class="sxs-lookup"><span data-stu-id="11eba-422">Tracking Issue #14617</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14617)

<span data-ttu-id="11eba-423">Это изменение реализовано в EF Core 3.0, предварительная версия 4.</span><span class="sxs-lookup"><span data-stu-id="11eba-423">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="11eba-424">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="11eba-424">**Old behavior**</span></span>

<span data-ttu-id="11eba-425">До выхода EF Core 3.0 свойства ключей `string` и `byte[]` можно было использовать без явного указания значения, отличного от NULL.</span><span class="sxs-lookup"><span data-stu-id="11eba-425">Before EF Core 3.0, `string` and `byte[]` key properties could be used without explicitly setting a non-null value.</span></span>
<span data-ttu-id="11eba-426">В этом случае значение ключа создается на клиенте в виде GUID, сериализованного до байтов для `byte[]`.</span><span class="sxs-lookup"><span data-stu-id="11eba-426">In such a case, the key value would be generated on the client as a GUID, serialized to bytes for `byte[]`.</span></span>

<span data-ttu-id="11eba-427">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="11eba-427">**New behavior**</span></span>

<span data-ttu-id="11eba-428">Начиная с EF Core 3.0 возникает исключение, указывающее на то, что значение ключа не задано.</span><span class="sxs-lookup"><span data-stu-id="11eba-428">Starting with EF Core 3.0 an exception will be thrown indicating that no key value has been set.</span></span>

<span data-ttu-id="11eba-429">**Причина**</span><span class="sxs-lookup"><span data-stu-id="11eba-429">**Why**</span></span>

<span data-ttu-id="11eba-430">Это изменение было внесено, так как формируемые клиентом значения `string`/`byte[]` обычно бесполезны, а поведение по умолчанию затрудняло типовое планирование использования этих значений ключей.</span><span class="sxs-lookup"><span data-stu-id="11eba-430">This change was made because client-generated `string`/`byte[]` values generally aren't useful, and the default behavior made it hard to reason about generated key values in a common way.</span></span>

<span data-ttu-id="11eba-431">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="11eba-431">**Mitigations**</span></span>

<span data-ttu-id="11eba-432">Поведение, предшествовавшее выходу версии 3.0, можно получить, явно указав, что свойства ключей должны использовать формируемые значения, если не задано никакое другое значение, отличное от NULL.</span><span class="sxs-lookup"><span data-stu-id="11eba-432">The pre-3.0 behavior can be obtained by explicitly specifying that the key properties should use generated values if no other non-null value is set.</span></span>
<span data-ttu-id="11eba-433">Например, с помощью текучего API:</span><span class="sxs-lookup"><span data-stu-id="11eba-433">For example, with the fluent API:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .ValueGeneratedOnAdd();
```

<span data-ttu-id="11eba-434">Или с помощью заметок к данным:</span><span class="sxs-lookup"><span data-stu-id="11eba-434">Or with data annotations:</span></span>

```C#
[DatabaseGenerated(DatabaseGeneratedOption.Identity)]
public string Id { get; set; }
```

## <a name="iloggerfactory-is-now-a-scoped-service"></a><span data-ttu-id="11eba-435">ILoggerFactory теперь является службой с ограниченной областью действия</span><span class="sxs-lookup"><span data-stu-id="11eba-435">ILoggerFactory is now a scoped service</span></span>

[<span data-ttu-id="11eba-436">Отслеживание вопроса 14698</span><span class="sxs-lookup"><span data-stu-id="11eba-436">Tracking Issue #14698</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14698)

<span data-ttu-id="11eba-437">Это изменение реализовано в EF Core 3.0, предварительная версия 3.</span><span class="sxs-lookup"><span data-stu-id="11eba-437">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="11eba-438">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="11eba-438">**Old behavior**</span></span>

<span data-ttu-id="11eba-439">До выхода EF Core 3.0 `ILoggerFactory` регистрировалась в качестве отдельной службы.</span><span class="sxs-lookup"><span data-stu-id="11eba-439">Before EF Core 3.0, `ILoggerFactory` was registered as a singleton service.</span></span>

<span data-ttu-id="11eba-440">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="11eba-440">**New behavior**</span></span>

<span data-ttu-id="11eba-441">Начиная с EF Core 3.0 `ILoggerFactory` регистрируется в качестве службы с ограниченной областью действия.</span><span class="sxs-lookup"><span data-stu-id="11eba-441">Starting with EF Core 3.0, `ILoggerFactory` is now registered as scoped.</span></span>

<span data-ttu-id="11eba-442">**Причина**</span><span class="sxs-lookup"><span data-stu-id="11eba-442">**Why**</span></span>

<span data-ttu-id="11eba-443">Это изменение было внесено, чтобы разрешить сопоставление средства ведения журнала с экземпляром `DbContext`, что обеспечивает другие функции и устраняет некоторые патологические варианты поведения, такие как взрывной рост внутренних поставщиков служб.</span><span class="sxs-lookup"><span data-stu-id="11eba-443">This change was made to allow association of a logger with a `DbContext` instance, which enables other functionality and removes some cases of pathological behavior such as an explosion of internal service providers.</span></span>

<span data-ttu-id="11eba-444">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="11eba-444">**Mitigations**</span></span>

<span data-ttu-id="11eba-445">Это изменение не должно затрагивать код приложения, если только он не регистрирует и использует настраиваемые службы во внутреннем поставщике служб EF Core.</span><span class="sxs-lookup"><span data-stu-id="11eba-445">This change should not impact application code unless it is registering and using custom services on the EF Core internal service provider.</span></span>
<span data-ttu-id="11eba-446">Это встречается довольно редко.</span><span class="sxs-lookup"><span data-stu-id="11eba-446">This isn't common.</span></span>
<span data-ttu-id="11eba-447">В таких случаях большинство возможностей продолжают работать, но потребуется изменить все отдельные службы, которые зависели от `ILoggerFactory`, чтобы получить `ILoggerFactory` другим способом.</span><span class="sxs-lookup"><span data-stu-id="11eba-447">In these cases, most things will still work, but any singleton service that was depending on `ILoggerFactory` will need to be changed to obtain the `ILoggerFactory` in a different way.</span></span>

<span data-ttu-id="11eba-448">При возникновении подобных ситуаций зарегистрируйте проблему в [средстве отслеживания вопросов EF Core на сайте GitHub](https://github.com/aspnet/EntityFrameworkCore/issues) и сообщите нам, как вы используете `ILoggerFactory`, чтобы мы могли лучше понять, как избежать подобных ошибок в будущем.</span><span class="sxs-lookup"><span data-stu-id="11eba-448">If you run into situations like this, please file an issue at on the [EF Core GitHub issue tracker](https://github.com/aspnet/EntityFrameworkCore/issues) to let us know how you are using `ILoggerFactory` such that we can better understand how not to break this again in the future.</span></span>

## <a name="lazy-loading-proxies-no-longer-assume-navigation-properties-are-fully-loaded"></a><span data-ttu-id="11eba-449">Прокси с отложенной загрузкой больше не предполагают полную загрузку свойств навигации</span><span class="sxs-lookup"><span data-stu-id="11eba-449">Lazy-loading proxies no longer assume navigation properties are fully loaded</span></span>

[<span data-ttu-id="11eba-450">Отслеживание вопроса 12780</span><span class="sxs-lookup"><span data-stu-id="11eba-450">Tracking Issue #12780</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12780)

<span data-ttu-id="11eba-451">Это изменение реализовано в EF Core 3.0, предварительная версия 4.</span><span class="sxs-lookup"><span data-stu-id="11eba-451">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="11eba-452">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="11eba-452">**Old behavior**</span></span>

<span data-ttu-id="11eba-453">До выхода EF Core 3.0 после удаления `DbContext` не существовало способа узнать, было ли указанное свойство навигации для сущности, полученной из этого контекста, загружено полностью.</span><span class="sxs-lookup"><span data-stu-id="11eba-453">Before EF Core 3.0, once a `DbContext` was disposed there was no way of knowing if a given navigation property on an entity obtained from that context was fully loaded or not.</span></span>
<span data-ttu-id="11eba-454">Вместо это прокси предполагали, что навигация по ссылке загружена, если она имеет отличное NULL значение, а навигация по коллекции загружена, если она не является пустой.</span><span class="sxs-lookup"><span data-stu-id="11eba-454">Proxies would instead assume that a reference navigation is loaded if it has a non-null value, and that a collection navigation is loaded if it isn't empty.</span></span>
<span data-ttu-id="11eba-455">В этом случае попытка отложенной загрузки приводила к холостой операции.</span><span class="sxs-lookup"><span data-stu-id="11eba-455">In these cases, attempting to lazy-load would be a no-op.</span></span>

<span data-ttu-id="11eba-456">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="11eba-456">**New behavior**</span></span>

<span data-ttu-id="11eba-457">Начиная с EF Core 3.0 прокси отслеживают, загружено ли свойство навигации.</span><span class="sxs-lookup"><span data-stu-id="11eba-457">Starting with EF Core 3.0, proxies keep track of whether or not a navigation property is loaded.</span></span>
<span data-ttu-id="11eba-458">Это означает, что попытка обращения к свойству навигации, загружаемому после удаления контекста, всегда будет давать холостую операцию, даже если загруженное свойство навигации пусто или равно NULL.</span><span class="sxs-lookup"><span data-stu-id="11eba-458">This means attempting to access a navigation property that is loaded after the context has been disposed will always be a no-op, even when the loaded navigation is empty or null.</span></span>
<span data-ttu-id="11eba-459">И наоборот, при попытке обращения к незагруженному свойству навигации, когда контекст удален, возникает исключение, даже если это свойство навигации является непустой коллекцией.</span><span class="sxs-lookup"><span data-stu-id="11eba-459">Conversely, attempting to access a navigation property that isn't loaded will throw an exception if the context is disposed even if the navigation property is a non-empty collection.</span></span>
<span data-ttu-id="11eba-460">Подобная ситуация указывает на то, что код приложения пытается использовать отложенную загрузку в недопустимое время, и нужно запретить это, изменив приложение.</span><span class="sxs-lookup"><span data-stu-id="11eba-460">If this situation arises, it means the application code is attempting to use lazy-loading at an invalid time, and the application should be changed to not do this.</span></span>

<span data-ttu-id="11eba-461">**Причина**</span><span class="sxs-lookup"><span data-stu-id="11eba-461">**Why**</span></span>

<span data-ttu-id="11eba-462">Это изменение было внесено, чтобы обеспечить правильное и согласованное поведение при попытке отложенной загрузки для удаленного экземпляра `DbContext`.</span><span class="sxs-lookup"><span data-stu-id="11eba-462">This change was made to make the behavior consistent and correct when attempting to lazy-load on a disposed `DbContext` instance.</span></span>

<span data-ttu-id="11eba-463">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="11eba-463">**Mitigations**</span></span>

<span data-ttu-id="11eba-464">Обновите код приложения, чтобы не выполнять попытку отложенной загрузки с удаленным контекстом, либо настройте соответствующую холостую операцию, как описано в сообщении об исключении.</span><span class="sxs-lookup"><span data-stu-id="11eba-464">Update application code to not attempt lazy-loading with a disposed context, or configure this to be a no-op as described in the exception message.</span></span>

## <a name="excessive-creation-of-internal-service-providers-is-now-an-error-by-default"></a><span data-ttu-id="11eba-465">Чрезмерное создание внутренних поставщиков служб теперь является ошибкой по умолчанию</span><span class="sxs-lookup"><span data-stu-id="11eba-465">Excessive creation of internal service providers is now an error by default</span></span>

[<span data-ttu-id="11eba-466">Отслеживание вопроса 10236</span><span class="sxs-lookup"><span data-stu-id="11eba-466">Tracking Issue #10236</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10236)

<span data-ttu-id="11eba-467">Это изменение реализовано в EF Core 3.0, предварительная версия 3.</span><span class="sxs-lookup"><span data-stu-id="11eba-467">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="11eba-468">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="11eba-468">**Old behavior**</span></span>

<span data-ttu-id="11eba-469">До выхода EF Core 3.0 для приложения, создающего чрезмерное количество внутренних поставщиков служб, регистрировалось предупреждение.</span><span class="sxs-lookup"><span data-stu-id="11eba-469">Before EF Core 3.0, a warning would be logged for an application creating a pathological number of internal service providers.</span></span>

<span data-ttu-id="11eba-470">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="11eba-470">**New behavior**</span></span>

<span data-ttu-id="11eba-471">Начиная с EF Core 3.0 это предупреждение считается ошибкой и возникает исключение.</span><span class="sxs-lookup"><span data-stu-id="11eba-471">Starting with EF Core 3.0, this warning is now considered and error and an exception is thrown.</span></span> 

<span data-ttu-id="11eba-472">**Причина**</span><span class="sxs-lookup"><span data-stu-id="11eba-472">**Why**</span></span>

<span data-ttu-id="11eba-473">Это изменение было внесено для улучшения кода приложения посредством явного указания на подобный патологический случай.</span><span class="sxs-lookup"><span data-stu-id="11eba-473">This change was made to drive better application code through exposing this pathological case more explicitly.</span></span>

<span data-ttu-id="11eba-474">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="11eba-474">**Mitigations**</span></span>

<span data-ttu-id="11eba-475">При возникновении этой ошибки правильнее всего выявить первопричину и остановить создание такого большого числа внутренних поставщиков служб.</span><span class="sxs-lookup"><span data-stu-id="11eba-475">The most appropriate cause of action on encountering this error is to understand the root cause and stop creating so many internal service providers.</span></span>
<span data-ttu-id="11eba-476">Тем не менее эту ошибку можно преобразовать обратно в предупреждение (или игнорировать) с помощью конфигурации для `DbContextOptionsBuilder`.</span><span class="sxs-lookup"><span data-stu-id="11eba-476">However, the error can be converted back to a warning (or ignored) via configuration on the `DbContextOptionsBuilder`.</span></span>
<span data-ttu-id="11eba-477">Например:</span><span class="sxs-lookup"><span data-stu-id="11eba-477">For example:</span></span>

```C#
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
{
    optionsBuilder
        .ConfigureWarnings(w => w.Log(CoreEventId.ManyServiceProvidersCreatedWarning));
}
```

## <a name="new-behavior-for-hasonehasmany-called-with-a-single-string"></a><span data-ttu-id="11eba-478">Новое поведение для вызова HasOne/HasMany с одной строкой</span><span class="sxs-lookup"><span data-stu-id="11eba-478">New behavior for HasOne/HasMany called with a single string</span></span>

[<span data-ttu-id="11eba-479">Отслеживание вопроса № 9171</span><span class="sxs-lookup"><span data-stu-id="11eba-479">Tracking Issue #9171</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9171)

<span data-ttu-id="11eba-480">Это изменение реализовано в EF Core 3.0, предварительная версия 4.</span><span class="sxs-lookup"><span data-stu-id="11eba-480">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="11eba-481">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="11eba-481">**Old behavior**</span></span>

<span data-ttu-id="11eba-482">До версии EF Core 3.0 интерпретация кода с вызовом `HasOne` или `HasMany` с одной строкой была очень нечеткой.</span><span class="sxs-lookup"><span data-stu-id="11eba-482">Before EF Core 3.0, code calling `HasOne` or `HasMany` with a single string was interpreted in a confusing way.</span></span>
<span data-ttu-id="11eba-483">Например:</span><span class="sxs-lookup"><span data-stu-id="11eba-483">For example:</span></span>
```C#
modelBuilder.Entity<Samurai>().HasOne("Entrance").WithOne();
```

<span data-ttu-id="11eba-484">Складывается впечатление, что код связывает `Samurai` с другим типом сущности с помощью свойства навигации `Entrance`, которое может быть закрытым.</span><span class="sxs-lookup"><span data-stu-id="11eba-484">The code looks like it is relating `Samurai` to some other entity type using the `Entrance` navigation property, which may be private.</span></span>

<span data-ttu-id="11eba-485">На самом деле этот код пытается создать отношение с некоторым типом сущности под именем `Entrance`, который не имеет свойства навигации.</span><span class="sxs-lookup"><span data-stu-id="11eba-485">In reality, this code attempts to create a relationship to some entity type called `Entrance` with no navigation property.</span></span>

<span data-ttu-id="11eba-486">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="11eba-486">**New behavior**</span></span>

<span data-ttu-id="11eba-487">Начиная с EF Core 3.0, указанный выше код выполняет те действия, которые должен был выполнять раньше.</span><span class="sxs-lookup"><span data-stu-id="11eba-487">Starting with EF Core 3.0, the code above now does what it looked like it should have been doing before.</span></span>

<span data-ttu-id="11eba-488">**Причина**</span><span class="sxs-lookup"><span data-stu-id="11eba-488">**Why**</span></span>

<span data-ttu-id="11eba-489">Старое поведение создавало большую путаницу, особенно при считывании кода конфигурации и при поиске ошибок.</span><span class="sxs-lookup"><span data-stu-id="11eba-489">The old behavior was very confusing, especially when reading the configuration code and looking for errors.</span></span>

<span data-ttu-id="11eba-490">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="11eba-490">**Mitigations**</span></span>

<span data-ttu-id="11eba-491">Это приведет к нарушению работы только тех приложений, которые явно настраивают отношения с помощью строк для имен типов и не указывают явно свойство навигации.</span><span class="sxs-lookup"><span data-stu-id="11eba-491">This will only break applications that are explicitly configuring relationships using strings for type names and without specifying the navigation property explicitly.</span></span>
<span data-ttu-id="11eba-492">Такой подход используется нечасто.</span><span class="sxs-lookup"><span data-stu-id="11eba-492">This is not common.</span></span>
<span data-ttu-id="11eba-493">Прежнее поведение можно реализовать с помощью явной передачи значения `null` для имени свойства навигации.</span><span class="sxs-lookup"><span data-stu-id="11eba-493">The previous behavior can be obtained through explicitly passing `null` for the navigation property name.</span></span>
<span data-ttu-id="11eba-494">Например:</span><span class="sxs-lookup"><span data-stu-id="11eba-494">For example:</span></span>

```C#
modelBuilder.Entity<Samurai>().HasOne("Some.Entity.Type.Name", null).WithOne();
```

## <a name="the-return-type-for-several-async-methods-has-been-changed-from-task-to-valuetask"></a><span data-ttu-id="11eba-495">Тип возвращаемого значения для нескольких асинхронных методов изменен с Task на ValueTask</span><span class="sxs-lookup"><span data-stu-id="11eba-495">The return type for several async methods has been changed from Task to ValueTask</span></span>

[<span data-ttu-id="11eba-496">Отслеживание вопроса № 15184</span><span class="sxs-lookup"><span data-stu-id="11eba-496">Tracking Issue #15184</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15184)

<span data-ttu-id="11eba-497">Это изменение реализовано в EF Core 3.0, предварительная версия 4.</span><span class="sxs-lookup"><span data-stu-id="11eba-497">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="11eba-498">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="11eba-498">**Old behavior**</span></span>

<span data-ttu-id="11eba-499">Следующие асинхронные методы ранее возвращали `Task<T>`:</span><span class="sxs-lookup"><span data-stu-id="11eba-499">The following async methods previously returned a `Task<T>`:</span></span>

* `DbContext.FindAsync()`
* `DbSet.FindAsync()`
* `DbContext.AddAsync()`
* `DbSet.AddAsync()`
* <span data-ttu-id="11eba-500">`ValueGenerator.NextValueAsync()` (и производные классы)</span><span class="sxs-lookup"><span data-stu-id="11eba-500">`ValueGenerator.NextValueAsync()` (and deriving classes)</span></span>

<span data-ttu-id="11eba-501">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="11eba-501">**New behavior**</span></span>

<span data-ttu-id="11eba-502">Упомянутые выше методы теперь возвращают `ValueTask<T>` над тем же `T`, что и раньше.</span><span class="sxs-lookup"><span data-stu-id="11eba-502">The aforementioned methods now return a `ValueTask<T>` over the same `T` as before.</span></span>

<span data-ttu-id="11eba-503">**Причина**</span><span class="sxs-lookup"><span data-stu-id="11eba-503">**Why**</span></span>

<span data-ttu-id="11eba-504">Это изменение уменьшает число выделений кучи, возникающих при вызове этих методов, что способствует повышению общей производительности.</span><span class="sxs-lookup"><span data-stu-id="11eba-504">This change reduces the number of heap allocations incurred when invoking these methods, improving general performance.</span></span>

<span data-ttu-id="11eba-505">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="11eba-505">**Mitigations**</span></span>

<span data-ttu-id="11eba-506">Приложения, просто ожидающие приведенные выше API, нужно просто перекомпилировать — изменять источник не требуется.</span><span class="sxs-lookup"><span data-stu-id="11eba-506">Applications simply awaiting the above APIs only need to be recompiled - no source changes are necessary.</span></span>
<span data-ttu-id="11eba-507">Для более сложного варианта использования (например, передачи возвращаемого класса `Task` методу `Task.WhenAny()`) обычно требуется, чтобы возвращаемый класс `ValueTask<T>` был преобразован в класс `Task<T>` путем вызова в нем метода `AsTask()`.</span><span class="sxs-lookup"><span data-stu-id="11eba-507">A more complex usage (e.g. passing the returned `Task` to `Task.WhenAny()`) typically require that the returned `ValueTask<T>` be converted to a `Task<T>` by calling `AsTask()` on it.</span></span>
<span data-ttu-id="11eba-508">Обратите внимание, что это сводит к нулю сокращение числа выделений, которое возможно в рамках этого изменения.</span><span class="sxs-lookup"><span data-stu-id="11eba-508">Note that this negates the allocation reduction that this change brings.</span></span>

## <a name="the-relationaltypemapping-annotation-is-now-just-typemapping"></a><span data-ttu-id="11eba-509">Заметка Relational:TypeMapping теперь является просто TypeMapping</span><span class="sxs-lookup"><span data-stu-id="11eba-509">The Relational:TypeMapping annotation is now just TypeMapping</span></span>

[<span data-ttu-id="11eba-510">Отслеживание вопроса 9913</span><span class="sxs-lookup"><span data-stu-id="11eba-510">Tracking Issue #9913</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9913)

<span data-ttu-id="11eba-511">Это изменение реализовано в EF Core 3.0, предварительная версия 2.</span><span class="sxs-lookup"><span data-stu-id="11eba-511">This change is introduced in EF Core 3.0-preview 2.</span></span>

<span data-ttu-id="11eba-512">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="11eba-512">**Old behavior**</span></span>

<span data-ttu-id="11eba-513">Для заметок сопоставлений типов использовалось имя "Relational:TypeMapping".</span><span class="sxs-lookup"><span data-stu-id="11eba-513">The annotation name for type mapping annotations was "Relational:TypeMapping".</span></span>

<span data-ttu-id="11eba-514">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="11eba-514">**New behavior**</span></span>

<span data-ttu-id="11eba-515">Теперь для заметок сопоставлений типов используется имя "TypeMapping".</span><span class="sxs-lookup"><span data-stu-id="11eba-515">The annotation name for type mapping annotations is now "TypeMapping".</span></span>

<span data-ttu-id="11eba-516">**Причина**</span><span class="sxs-lookup"><span data-stu-id="11eba-516">**Why**</span></span>

<span data-ttu-id="11eba-517">Сопоставления типов теперь используются не только для поставщиков реляционных баз данных.</span><span class="sxs-lookup"><span data-stu-id="11eba-517">Type mappings are now used for more than just relational database providers.</span></span>

<span data-ttu-id="11eba-518">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="11eba-518">**Mitigations**</span></span>

<span data-ttu-id="11eba-519">Это изменение нарушит работу только тех приложений, которые обращаются к сопоставлению типов непосредственно по заметке, что встречается довольно редко.</span><span class="sxs-lookup"><span data-stu-id="11eba-519">This will only break applications that access the type mapping directly as an annotation, which isn't common.</span></span>
<span data-ttu-id="11eba-520">Наиболее подходящий способ для устранения этой проблемы заключается в использовании поверхности API для доступа к сопоставлениям типов вместо прямого использования заметки.</span><span class="sxs-lookup"><span data-stu-id="11eba-520">The most appropriate action to fix is to use API surface to access type mappings rather than using the annotation directly.</span></span>

## <a name="totable-on-a-derived-type-throws-an-exception"></a><span data-ttu-id="11eba-521">ToTable для производного типа выдает исключение</span><span class="sxs-lookup"><span data-stu-id="11eba-521">ToTable on a derived type throws an exception</span></span> 

[<span data-ttu-id="11eba-522">Отслеживание вопроса 11811</span><span class="sxs-lookup"><span data-stu-id="11eba-522">Tracking Issue #11811</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/11811)

<span data-ttu-id="11eba-523">Это изменение реализовано в EF Core 3.0, предварительная версия 3.</span><span class="sxs-lookup"><span data-stu-id="11eba-523">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="11eba-524">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="11eba-524">**Old behavior**</span></span>

<span data-ttu-id="11eba-525">До выхода EF Core 3.0 вызов `ToTable()` для производного типа игнорировался, так как единственной стратегией сопоставления наследования был метод "одна таблица на иерархию", где такая операция недействительна.</span><span class="sxs-lookup"><span data-stu-id="11eba-525">Before EF Core 3.0, `ToTable()` called on a derived type would be ignored since only inheritance mapping strategy was TPH where this isn't valid.</span></span> 

<span data-ttu-id="11eba-526">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="11eba-526">**New behavior**</span></span>

<span data-ttu-id="11eba-527">Начиная с EF Core 3.0 и в рамках подготовки для добавления поддержки методов "одна таблица на тип" и "одна таблица на каждый отдельный тип/класс" в последующем выпуске вызов `ToTable()` для производного типа теперь будет выдавать исключение, чтобы избежать непредвиденного изменения сопоставления в будущем.</span><span class="sxs-lookup"><span data-stu-id="11eba-527">Starting with EF Core 3.0 and in preparation for adding TPT and TPC support in a later release, `ToTable()` called on a derived type will now throw an exception to avoid an unexpected mapping change in the future.</span></span>

<span data-ttu-id="11eba-528">**Причина**</span><span class="sxs-lookup"><span data-stu-id="11eba-528">**Why**</span></span>

<span data-ttu-id="11eba-529">Сейчас сопоставлять производный тип с другой таблицей запрещено.</span><span class="sxs-lookup"><span data-stu-id="11eba-529">Currently it isn't valid to map a derived type to a different table.</span></span>
<span data-ttu-id="11eba-530">Это изменение позволяет избежать нарушения работы в будущем, когда подобная операция будет разрешена.</span><span class="sxs-lookup"><span data-stu-id="11eba-530">This change avoids breaking in the future when it becomes a valid thing to do.</span></span>

<span data-ttu-id="11eba-531">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="11eba-531">**Mitigations**</span></span>

<span data-ttu-id="11eba-532">Удалите все попытки сопоставить производные типы с другими таблицами.</span><span class="sxs-lookup"><span data-stu-id="11eba-532">Remove any attempts to map derived types to other tables.</span></span>

## <a name="forsqlserverhasindex-replaced-with-hasindex"></a><span data-ttu-id="11eba-533">ForSqlServerHasIndex заменен на HasIndex</span><span class="sxs-lookup"><span data-stu-id="11eba-533">ForSqlServerHasIndex replaced with HasIndex</span></span> 

[<span data-ttu-id="11eba-534">Отслеживание вопроса 12366</span><span class="sxs-lookup"><span data-stu-id="11eba-534">Tracking Issue #12366</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12366)

<span data-ttu-id="11eba-535">Это изменение реализовано в EF Core 3.0, предварительная версия 3.</span><span class="sxs-lookup"><span data-stu-id="11eba-535">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="11eba-536">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="11eba-536">**Old behavior**</span></span>

<span data-ttu-id="11eba-537">До выхода EF Core 3.0 `ForSqlServerHasIndex().ForSqlServerInclude()` предоставлял способ настройки столбцов, используемых с `INCLUDE`.</span><span class="sxs-lookup"><span data-stu-id="11eba-537">Before EF Core 3.0, `ForSqlServerHasIndex().ForSqlServerInclude()` provided a way to configure columns used with `INCLUDE`.</span></span>

<span data-ttu-id="11eba-538">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="11eba-538">**New behavior**</span></span>

<span data-ttu-id="11eba-539">Начиная с EF Core 3.0 использование `Include` для индекса теперь поддерживается на реляционном уровне.</span><span class="sxs-lookup"><span data-stu-id="11eba-539">Starting with EF Core 3.0, using `Include` on an index is now supported at the relational level.</span></span>
<span data-ttu-id="11eba-540">Используйте ключевое слово `HasIndex().ForSqlServerInclude()`.</span><span class="sxs-lookup"><span data-stu-id="11eba-540">Use `HasIndex().ForSqlServerInclude()`.</span></span>

<span data-ttu-id="11eba-541">**Причина**</span><span class="sxs-lookup"><span data-stu-id="11eba-541">**Why**</span></span>

<span data-ttu-id="11eba-542">Это изменение было внесено, чтобы консолидировать API для индексов с `Include` в одном месте для всех поставщиков баз данных.</span><span class="sxs-lookup"><span data-stu-id="11eba-542">This change was made to consolidate the API for indexes with `Include` into one place for all database providers.</span></span>

<span data-ttu-id="11eba-543">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="11eba-543">**Mitigations**</span></span>

<span data-ttu-id="11eba-544">Используйте новый API, как показано выше.</span><span class="sxs-lookup"><span data-stu-id="11eba-544">Use the new API, as shown above.</span></span>

## <a name="metadata-api-changes"></a><span data-ttu-id="11eba-545">Изменения API метаданных</span><span class="sxs-lookup"><span data-stu-id="11eba-545">Metadata API changes</span></span>

[<span data-ttu-id="11eba-546">Отслеживание вопроса № 214</span><span class="sxs-lookup"><span data-stu-id="11eba-546">Tracking Issue #214</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/214)

<span data-ttu-id="11eba-547">Это изменение реализовано в EF Core 3.0, предварительная версия 4.</span><span class="sxs-lookup"><span data-stu-id="11eba-547">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="11eba-548">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="11eba-548">**New behavior**</span></span>

<span data-ttu-id="11eba-549">Следующие свойства были преобразованы в методы расширения:</span><span class="sxs-lookup"><span data-stu-id="11eba-549">The following properties were converted to extension methods:</span></span>

* `IEntityType.QueryFilter` -> `GetQueryFilter()`
* `IEntityType.DefiningQuery` -> `GetDefiningQuery()`
* `IProperty.IsShadowProperty` -> `IsShadowProperty()`
* `IProperty.BeforeSaveBehavior` -> `GetBeforeSaveBehavior()`
* `IProperty.AfterSaveBehavior` -> `GetAfterSaveBehavior()`

<span data-ttu-id="11eba-550">**Причина**</span><span class="sxs-lookup"><span data-stu-id="11eba-550">**Why**</span></span>

<span data-ttu-id="11eba-551">Это изменение упрощает реализацию упомянутых выше интерфейсов.</span><span class="sxs-lookup"><span data-stu-id="11eba-551">This change simplifies the implementation of the aforementioned interfaces.</span></span>

<span data-ttu-id="11eba-552">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="11eba-552">**Mitigations**</span></span>

<span data-ttu-id="11eba-553">Используйте новые методы расширения.</span><span class="sxs-lookup"><span data-stu-id="11eba-553">Use the new extension methods.</span></span>

## <a name="provider-specific-metadata-api-changes"></a><span data-ttu-id="11eba-554">Изменения API метаданных с учетом поставщика</span><span class="sxs-lookup"><span data-stu-id="11eba-554">Provider-specific Metadata API changes</span></span>

[<span data-ttu-id="11eba-555">Отслеживание вопроса № 214</span><span class="sxs-lookup"><span data-stu-id="11eba-555">Tracking Issue #214</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/214)

<span data-ttu-id="11eba-556">Это изменение реализовано в EF Core 3.0, предварительная версия 6.</span><span class="sxs-lookup"><span data-stu-id="11eba-556">This change is introduced in EF Core 3.0-preview 6.</span></span>

<span data-ttu-id="11eba-557">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="11eba-557">**New behavior**</span></span>

<span data-ttu-id="11eba-558">Методы расширения с учетом поставщика будут приведены в соответствие:</span><span class="sxs-lookup"><span data-stu-id="11eba-558">The provider-specific extension methods will be flattened out:</span></span>

* `IProperty.Relational().ColumnName` -> `IProperty.GetColumnName()`
* `IEntityType.SqlServer().IsMemoryOptimized` -> `IEntityType.GetSqlServerIsMemoryOptimized()`
* `PropertyBuilder.UseSqlServerIdentityColumn()` -> `PropertyBuilder.ForSqlServerUseIdentityColumn()`

<span data-ttu-id="11eba-559">**Причина**</span><span class="sxs-lookup"><span data-stu-id="11eba-559">**Why**</span></span>

<span data-ttu-id="11eba-560">Это изменение упрощает реализацию упомянутых выше методов расширения.</span><span class="sxs-lookup"><span data-stu-id="11eba-560">This change simplifies the implementation of the aforementioned extension methods.</span></span>

<span data-ttu-id="11eba-561">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="11eba-561">**Mitigations**</span></span>

<span data-ttu-id="11eba-562">Используйте новые методы расширения.</span><span class="sxs-lookup"><span data-stu-id="11eba-562">Use the new extension methods.</span></span>

## <a name="ef-core-no-longer-sends-pragma-for-sqlite-fk-enforcement"></a><span data-ttu-id="11eba-563">EF Core больше не отправляет pragma для принудительного применения FK SQLite</span><span class="sxs-lookup"><span data-stu-id="11eba-563">EF Core no longer sends pragma for SQLite FK enforcement</span></span>

[<span data-ttu-id="11eba-564">Отслеживание вопроса 12151</span><span class="sxs-lookup"><span data-stu-id="11eba-564">Tracking Issue #12151</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12151)

<span data-ttu-id="11eba-565">Это изменение реализовано в EF Core 3.0, предварительная версия 3.</span><span class="sxs-lookup"><span data-stu-id="11eba-565">This change is introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="11eba-566">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="11eba-566">**Old behavior**</span></span>

<span data-ttu-id="11eba-567">До выхода версии 3.0 система EF Core отправляла `PRAGMA foreign_keys = 1` при открытии соединения с SQLite.</span><span class="sxs-lookup"><span data-stu-id="11eba-567">Before EF Core 3.0, EF Core would send `PRAGMA foreign_keys = 1` when a connection to SQLite is opened.</span></span>

<span data-ttu-id="11eba-568">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="11eba-568">**New behavior**</span></span>

<span data-ttu-id="11eba-569">Начиная с версии 3.0 система EF Core больше не отправляет `PRAGMA foreign_keys = 1` при открытии соединения с SQLite.</span><span class="sxs-lookup"><span data-stu-id="11eba-569">Starting with EF Core 3.0, EF Core no longer sends `PRAGMA foreign_keys = 1` when a connection to SQLite is opened.</span></span>

<span data-ttu-id="11eba-570">**Причина**</span><span class="sxs-lookup"><span data-stu-id="11eba-570">**Why**</span></span>

<span data-ttu-id="11eba-571">Это изменение было внесено, так как EF Core по умолчанию использует `SQLitePCLRaw.bundle_e_sqlite3`, что, в свою очередь, означает, что принудительное применение FK по умолчанию включено и его не требуется включать каждый раз при открытии соединения.</span><span class="sxs-lookup"><span data-stu-id="11eba-571">This change was made because EF Core uses `SQLitePCLRaw.bundle_e_sqlite3` by default, which in turn means that FK enforcement is switched on by default and doesn't need to be explicitly enabled each time a connection is opened.</span></span>

<span data-ttu-id="11eba-572">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="11eba-572">**Mitigations**</span></span>

<span data-ttu-id="11eba-573">Внешние ключи включены по умолчанию в SQLitePCLRaw.bundle_e_sqlite3, который по умолчанию используется для EF Core.</span><span class="sxs-lookup"><span data-stu-id="11eba-573">Foreign keys are enabled by default in SQLitePCLRaw.bundle_e_sqlite3, which is used by default for EF Core.</span></span>
<span data-ttu-id="11eba-574">В других случаях внешние ключи можно включить, указав `Foreign Keys=True` в строке подключения.</span><span class="sxs-lookup"><span data-stu-id="11eba-574">For other cases, foreign keys can be enabled by specifying `Foreign Keys=True` in your connection string.</span></span>

## <a name="microsoftentityframeworkcoresqlite-now-depends-on-sqlitepclrawbundleesqlite3"></a><span data-ttu-id="11eba-575">Теперь Microsoft.EntityFrameworkCore.Sqlite зависит от SQLitePCLRaw.bundle_e_sqlite3</span><span class="sxs-lookup"><span data-stu-id="11eba-575">Microsoft.EntityFrameworkCore.Sqlite now depends on SQLitePCLRaw.bundle_e_sqlite3</span></span>

<span data-ttu-id="11eba-576">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="11eba-576">**Old behavior**</span></span>

<span data-ttu-id="11eba-577">До выхода версии 3.0 система EF Core использовала `SQLitePCLRaw.bundle_green`.</span><span class="sxs-lookup"><span data-stu-id="11eba-577">Before EF Core 3.0, EF Core used `SQLitePCLRaw.bundle_green`.</span></span>

<span data-ttu-id="11eba-578">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="11eba-578">**New behavior**</span></span>

<span data-ttu-id="11eba-579">Начиная с версии 3.0 система EF Core использует `SQLitePCLRaw.bundle_e_sqlite3`.</span><span class="sxs-lookup"><span data-stu-id="11eba-579">Starting with EF Core 3.0, EF Core uses `SQLitePCLRaw.bundle_e_sqlite3`.</span></span>

<span data-ttu-id="11eba-580">**Причина**</span><span class="sxs-lookup"><span data-stu-id="11eba-580">**Why**</span></span>

<span data-ttu-id="11eba-581">Это изменение было внесено, чтобы версия SQLite, используемая в iOS, была согласована с другими платформами.</span><span class="sxs-lookup"><span data-stu-id="11eba-581">This change was made so that the version of SQLite used on iOS consistent with other platforms.</span></span>

<span data-ttu-id="11eba-582">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="11eba-582">**Mitigations**</span></span>

<span data-ttu-id="11eba-583">Чтобы использовать собственную версию SQLite в iOS, настройте `Microsoft.Data.Sqlite` для использования другого пакета `SQLitePCLRaw`.</span><span class="sxs-lookup"><span data-stu-id="11eba-583">To use the native SQLite version on iOS, configure `Microsoft.Data.Sqlite` to use a different `SQLitePCLRaw` bundle.</span></span>

## <a name="guid-values-are-now-stored-as-text-on-sqlite"></a><span data-ttu-id="11eba-584">Теперь значения Guid хранятся в SQLite в виде значений типа TEXT</span><span class="sxs-lookup"><span data-stu-id="11eba-584">Guid values are now stored as TEXT on SQLite</span></span>

[<span data-ttu-id="11eba-585">Отслеживание вопроса № 15078</span><span class="sxs-lookup"><span data-stu-id="11eba-585">Tracking Issue #15078</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15078)

<span data-ttu-id="11eba-586">Это изменение реализовано в EF Core 3.0, предварительная версия 4.</span><span class="sxs-lookup"><span data-stu-id="11eba-586">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="11eba-587">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="11eba-587">**Old behavior**</span></span>

<span data-ttu-id="11eba-588">Ранее значения типа Guid хранились в SQLite в виде значений типа BLOB.</span><span class="sxs-lookup"><span data-stu-id="11eba-588">Guid values were previously sored as BLOB values on SQLite.</span></span>

<span data-ttu-id="11eba-589">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="11eba-589">**New behavior**</span></span>

<span data-ttu-id="11eba-590">Значения GUID теперь хранятся в виде значений типа TEXT.</span><span class="sxs-lookup"><span data-stu-id="11eba-590">Guid values are now stored as TEXT.</span></span>

<span data-ttu-id="11eba-591">**Причина**</span><span class="sxs-lookup"><span data-stu-id="11eba-591">**Why**</span></span>

<span data-ttu-id="11eba-592">Двоичный формат типа Guid не стандартизирован.</span><span class="sxs-lookup"><span data-stu-id="11eba-592">The binary format of Guids is not standardized.</span></span> <span data-ttu-id="11eba-593">Хранение значений в виде значений типа TEXT повышает уровень совместимости базы данных с другими технологиями.</span><span class="sxs-lookup"><span data-stu-id="11eba-593">Storing the values as TEXT makes the database more compatible with other technologies.</span></span>

<span data-ttu-id="11eba-594">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="11eba-594">**Mitigations**</span></span>

<span data-ttu-id="11eba-595">Существующие базы данных можно перенести в новый формат, выполнив код SQL следующим образом.</span><span class="sxs-lookup"><span data-stu-id="11eba-595">You can migrate existing databases to the new format by executing SQL like the following.</span></span>

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

<span data-ttu-id="11eba-596">В EF Core можно продолжить использовать прежнее поведение, настроив преобразователь величин для этих свойств.</span><span class="sxs-lookup"><span data-stu-id="11eba-596">In EF Core, you could also continue using the previous behavior by configuring a value converter on these properties.</span></span>

``` csharp
modelBuilder
    .Entity<MyEntity>()
    .Property(e => e.GuidProperty)
    .HasConversion(
        g => g.ToByteArray(),
        b => new Guid(b));
```

<span data-ttu-id="11eba-597">Microsoft.Data.Sqlite по-прежнему может читать значения Guid из столбцов BLOB и TEXT. Но поскольку изменился формат по умолчанию для параметров и констант, скорее всего, вам потребуется выполнить некоторые действия в большинстве сценариев, где используются значения Guid.</span><span class="sxs-lookup"><span data-stu-id="11eba-597">Microsoft.Data.Sqlite remains capable of reading Guid values from both BLOB and TEXT columns; however, since the default format for parameters and constants has changed you'll likely need to take action for most scenarios involving Guids.</span></span>

## <a name="char-values-are-now-stored-as-text-on-sqlite"></a><span data-ttu-id="11eba-598">Теперь значения типа Char хранятся в SQLite в виде значений типа TEXT</span><span class="sxs-lookup"><span data-stu-id="11eba-598">Char values are now stored as TEXT on SQLite</span></span>

[<span data-ttu-id="11eba-599">Отслеживание вопроса № 15020</span><span class="sxs-lookup"><span data-stu-id="11eba-599">Tracking Issue #15020</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15020)

<span data-ttu-id="11eba-600">Это изменение реализовано в EF Core 3.0, предварительная версия 4.</span><span class="sxs-lookup"><span data-stu-id="11eba-600">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="11eba-601">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="11eba-601">**Old behavior**</span></span>

<span data-ttu-id="11eba-602">Ранее значения типа Char хранились в SQLite в виде значений типа INTEGER.</span><span class="sxs-lookup"><span data-stu-id="11eba-602">Char values were previously sored as INTEGER values on SQLite.</span></span> <span data-ttu-id="11eba-603">Например, значение типа Char *A* хранилось как целочисленное значение 65.</span><span class="sxs-lookup"><span data-stu-id="11eba-603">For example, a char value of *A* was stored as the integer value 65.</span></span>

<span data-ttu-id="11eba-604">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="11eba-604">**New behavior**</span></span>

<span data-ttu-id="11eba-605">Теперь значения типа char хранятся в виде значений типа TEXT.</span><span class="sxs-lookup"><span data-stu-id="11eba-605">Char values are now stored as TEXT.</span></span>

<span data-ttu-id="11eba-606">**Причина**</span><span class="sxs-lookup"><span data-stu-id="11eba-606">**Why**</span></span>

<span data-ttu-id="11eba-607">Хранение значений в виде значений типа TEXT является более естественным вариантом и повышает уровень совместимости базы данных с другими технологиями.</span><span class="sxs-lookup"><span data-stu-id="11eba-607">Storing the values as TEXT is more natural and makes the database more compatible with other technologies.</span></span>

<span data-ttu-id="11eba-608">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="11eba-608">**Mitigations**</span></span>

<span data-ttu-id="11eba-609">Существующие базы данных можно перенести в новый формат, выполнив код SQL следующим образом.</span><span class="sxs-lookup"><span data-stu-id="11eba-609">You can migrate existing databases to the new format by executing SQL like the following.</span></span>

``` sql
UPDATE MyTable
SET CharColumn = char(CharColumn)
WHERE typeof(CharColumn) = 'integer';
```

<span data-ttu-id="11eba-610">В EF Core можно и дальше использовать прежнее поведение, настроив преобразователь величин для этих свойств.</span><span class="sxs-lookup"><span data-stu-id="11eba-610">In EF Core, you could also continue using the previous behavior by configuring a value converter on these properties.</span></span>

``` csharp
modelBuilder
    .Entity<MyEntity>()
    .Property(e => e.CharProperty)
    .HasConversion(
        c => (long)c,
        i => (char)i);
```

<span data-ttu-id="11eba-611">Microsoft.Data.Sqlite также сохраняет возможность чтения значений символов из столбцов INTEGER и TEXT, поэтому для реализации определенных сценариев может не потребоваться никаких действий.</span><span class="sxs-lookup"><span data-stu-id="11eba-611">Microsoft.Data.Sqlite also remains capable of reading character values from both INTEGER and TEXT columns, so certain scenarios may not require any action.</span></span>

## <a name="migration-ids-are-now-generated-using-the-invariant-cultures-calendar"></a><span data-ttu-id="11eba-612">Идентификаторы миграции теперь создаются с использованием календаря инвариантного языка и региональных параметров</span><span class="sxs-lookup"><span data-stu-id="11eba-612">Migration IDs are now generated using the invariant culture's calendar</span></span>

[<span data-ttu-id="11eba-613">Отслеживание вопроса № 12978</span><span class="sxs-lookup"><span data-stu-id="11eba-613">Tracking Issue #12978</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12978)

<span data-ttu-id="11eba-614">Это изменение реализовано в EF Core 3.0, предварительная версия 4.</span><span class="sxs-lookup"><span data-stu-id="11eba-614">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="11eba-615">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="11eba-615">**Old behavior**</span></span>

<span data-ttu-id="11eba-616">Идентификаторы миграции непреднамеренно создавались с использованием календаря текущего языка и региональных параметров.</span><span class="sxs-lookup"><span data-stu-id="11eba-616">Migration IDs were inadvertently generated using the current culture's calendar.</span></span>

<span data-ttu-id="11eba-617">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="11eba-617">**New behavior**</span></span>

<span data-ttu-id="11eba-618">Теперь идентификаторы миграций всегда создаются с использованием календаря инвариантного языка и региональных параметров (григорианского).</span><span class="sxs-lookup"><span data-stu-id="11eba-618">Migration IDs are now always generated using the invariant culture's calendar (Gregorian).</span></span>

<span data-ttu-id="11eba-619">**Причина**</span><span class="sxs-lookup"><span data-stu-id="11eba-619">**Why**</span></span>

<span data-ttu-id="11eba-620">Порядок миграций играет важную роль при обновлении базы данных или разрешении конфликтов слияния.</span><span class="sxs-lookup"><span data-stu-id="11eba-620">The order of migrations is important when updating the database or resolving merge conflicts.</span></span> <span data-ttu-id="11eba-621">Использование инвариантного календаря позволяет избежать проблем с порядком выполнения, которые могут быть связаны с наличием разных системных календарей у членов группы.</span><span class="sxs-lookup"><span data-stu-id="11eba-621">Using the invariant calendar avoids ordering issues that can result from team members having different system calendars.</span></span>

<span data-ttu-id="11eba-622">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="11eba-622">**Mitigations**</span></span>

<span data-ttu-id="11eba-623">Это изменение затрагивает всех, кто использует календари, отличные от григорианского, в которых значение года больше, чем в григорианском (например, тайский (буддистский) календарь).</span><span class="sxs-lookup"><span data-stu-id="11eba-623">This change affects anyone using a non-Gregorian calendar where the year is greater than the Gregorian calendar (like the Thai Buddhist calendar).</span></span> <span data-ttu-id="11eba-624">Имеющиеся идентификаторы миграций следует обновить, чтобы новые миграции выполнялись после существующих.</span><span class="sxs-lookup"><span data-stu-id="11eba-624">Existing migration IDs will need to be updated so that new migrations are ordered after existing migrations.</span></span>

<span data-ttu-id="11eba-625">Идентификатор миграции можно найти в атрибуте Migration в файлы конструктора миграций.</span><span class="sxs-lookup"><span data-stu-id="11eba-625">The migration ID can be found in the Migration attribute in the migrations' designer files.</span></span>

``` diff
 [DbContext(typeof(MyDbContext))]
-[Migration("25620318122820_MyMigration")]
+[Migration("20190318122820_MyMigration")]
 partial class MyMigration
 {
```

<span data-ttu-id="11eba-626">Кроме того, потребуется обновить таблицу журнала миграций.</span><span class="sxs-lookup"><span data-stu-id="11eba-626">The Migrations history table also needs to be updated.</span></span>

``` sql
UPDATE __EFMigrationsHistory
SET MigrationId = CONCAT(LEFT(MigrationId, 4)  - 543, SUBSTRING(MigrationId, 4, 150))
```

## <a name="extension-infometadata-has-been-removed-from-idbcontextoptionsextension"></a><span data-ttu-id="11eba-627">Сведения о расширении и его метаданные были удалены из интерфейса IDbContextOptionsExtension</span><span class="sxs-lookup"><span data-stu-id="11eba-627">Extension info/metadata has been removed from IDbContextOptionsExtension</span></span>

[<span data-ttu-id="11eba-628">Отслеживание вопроса № 16119</span><span class="sxs-lookup"><span data-stu-id="11eba-628">Tracking Issue #16119</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/16119)

<span data-ttu-id="11eba-629">Это изменение реализовано в EF Core 3.0, предварительная версия 7.</span><span class="sxs-lookup"><span data-stu-id="11eba-629">This change is introduced in EF Core 3.0-preview 7.</span></span>

<span data-ttu-id="11eba-630">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="11eba-630">**Old behavior**</span></span>

<span data-ttu-id="11eba-631">Интерфейс `IDbContextOptionsExtension` содержал методы для предоставления метаданных расширения.</span><span class="sxs-lookup"><span data-stu-id="11eba-631">`IDbContextOptionsExtension` contained methods for providing metadata about the extension.</span></span>

<span data-ttu-id="11eba-632">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="11eba-632">**New behavior**</span></span>

<span data-ttu-id="11eba-633">Эти методы были перемещены в новый абстрактный базовый класс `DbContextOptionsExtensionInfo`, который возвращается новым свойством `IDbContextOptionsExtension.Info`.</span><span class="sxs-lookup"><span data-stu-id="11eba-633">These methods have been moved onto a new `DbContextOptionsExtensionInfo` abstract base class, which is returned from a new `IDbContextOptionsExtension.Info` property.</span></span>

<span data-ttu-id="11eba-634">**Причина**</span><span class="sxs-lookup"><span data-stu-id="11eba-634">**Why**</span></span>

<span data-ttu-id="11eba-635">В период между выпусками 2.0 и 3.0 нам пришлось несколько раз добавлять и изменять эти методы.</span><span class="sxs-lookup"><span data-stu-id="11eba-635">Over the releases from 2.0 to 3.0 we needed to add to or change these methods several times.</span></span>
<span data-ttu-id="11eba-636">Их выделение в новый абстрактный базовый класс упростит реализацию таких изменений и позволит вносить их без нарушения работы существующих расширений.</span><span class="sxs-lookup"><span data-stu-id="11eba-636">Breaking them out into a new abstract base class will make it easier to make these kind of changes without breaking existing extensions.</span></span>

<span data-ttu-id="11eba-637">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="11eba-637">**Mitigations**</span></span>

<span data-ttu-id="11eba-638">Обновите расширения, чтобы применить новый шаблон.</span><span class="sxs-lookup"><span data-stu-id="11eba-638">Update extensions to follow the new pattern.</span></span>
<span data-ttu-id="11eba-639">Примеры доступны во множестве реализаций `IDbContextOptionsExtension` для разных типов расширений в исходном коде EF Core.</span><span class="sxs-lookup"><span data-stu-id="11eba-639">Examples are found in the many implementations of `IDbContextOptionsExtension` for different kinds of extensions in the EF Core source code.</span></span>

## <a name="logquerypossibleexceptionwithaggregateoperator-has-been-renamed"></a><span data-ttu-id="11eba-640">Оператор LogQueryPossibleExceptionWithAggregateOperator был переименован</span><span class="sxs-lookup"><span data-stu-id="11eba-640">LogQueryPossibleExceptionWithAggregateOperator has been renamed</span></span>

[<span data-ttu-id="11eba-641">Отслеживание проблемы № 10985</span><span class="sxs-lookup"><span data-stu-id="11eba-641">Tracking Issue #10985</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10985)

<span data-ttu-id="11eba-642">Это изменение реализовано в EF Core 3.0, предварительная версия 4.</span><span class="sxs-lookup"><span data-stu-id="11eba-642">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="11eba-643">**Изменение**</span><span class="sxs-lookup"><span data-stu-id="11eba-643">**Change**</span></span>

<span data-ttu-id="11eba-644">`RelationalEventId.LogQueryPossibleExceptionWithAggregateOperator` переименован в `RelationalEventId.LogQueryPossibleExceptionWithAggregateOperatorWarning`.</span><span class="sxs-lookup"><span data-stu-id="11eba-644">`RelationalEventId.LogQueryPossibleExceptionWithAggregateOperator` has been renamed to `RelationalEventId.LogQueryPossibleExceptionWithAggregateOperatorWarning`.</span></span>

<span data-ttu-id="11eba-645">**Причина**</span><span class="sxs-lookup"><span data-stu-id="11eba-645">**Why**</span></span>

<span data-ttu-id="11eba-646">Выравнивает именование этого события предупреждения с другими событиями предупреждения.</span><span class="sxs-lookup"><span data-stu-id="11eba-646">Aligns the naming of this warning event with all other warning events.</span></span>

<span data-ttu-id="11eba-647">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="11eba-647">**Mitigations**</span></span>

<span data-ttu-id="11eba-648">Используйте новое имя.</span><span class="sxs-lookup"><span data-stu-id="11eba-648">Use the new name.</span></span> <span data-ttu-id="11eba-649">(Обратите внимание, что номер события не изменен.)</span><span class="sxs-lookup"><span data-stu-id="11eba-649">(Note that the event ID number has not changed.)</span></span>

## <a name="clarify-api-for-foreign-key-constraint-names"></a><span data-ttu-id="11eba-650">Уточнение API для имен ограничений внешнего ключа</span><span class="sxs-lookup"><span data-stu-id="11eba-650">Clarify API for foreign key constraint names</span></span>

[<span data-ttu-id="11eba-651">Отслеживание проблемы № 10730</span><span class="sxs-lookup"><span data-stu-id="11eba-651">Tracking Issue #10730</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10730)

<span data-ttu-id="11eba-652">Это изменение реализовано в EF Core 3.0, предварительная версия 4.</span><span class="sxs-lookup"><span data-stu-id="11eba-652">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="11eba-653">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="11eba-653">**Old behavior**</span></span>

<span data-ttu-id="11eba-654">До EF Core 3.0 имена ограничений внешнего ключа назывались просто именами.</span><span class="sxs-lookup"><span data-stu-id="11eba-654">Before EF Core 3.0, foreign key constraint names were referred to as simply the "name".</span></span> <span data-ttu-id="11eba-655">Например:</span><span class="sxs-lookup"><span data-stu-id="11eba-655">For example:</span></span>

```C#
var constraintName = myForeignKey.Name;
```

<span data-ttu-id="11eba-656">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="11eba-656">**New behavior**</span></span>

<span data-ttu-id="11eba-657">Начиная с EF Core 3.0 имена ограничений внешнего ключа называются "имя ограничения".</span><span class="sxs-lookup"><span data-stu-id="11eba-657">Starting with EF Core 3.0, foreign key constraint names are now referred to as the "constraint name".</span></span> <span data-ttu-id="11eba-658">Например:</span><span class="sxs-lookup"><span data-stu-id="11eba-658">For example:</span></span>

```C#
var constraintName = myForeignKey.ConstraintName;
```

<span data-ttu-id="11eba-659">**Причина**</span><span class="sxs-lookup"><span data-stu-id="11eba-659">**Why**</span></span>

<span data-ttu-id="11eba-660">Это изменение обеспечивает согласованность именования в этой области, а также указывает, что это имя ограничения внешнего ключа, а не имя столбца или свойства, для которого определен внешний ключ.</span><span class="sxs-lookup"><span data-stu-id="11eba-660">This change brings consistency to naming in this area, and also clarifies that this is the name of the foreign key constraint, and not the column or property name that the foreign key is defined on.</span></span>

<span data-ttu-id="11eba-661">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="11eba-661">**Mitigations**</span></span>

<span data-ttu-id="11eba-662">Используйте новое имя.</span><span class="sxs-lookup"><span data-stu-id="11eba-662">Use the new name.</span></span>

## <a name="irelationaldatabasecreatorhastableshastablesasync-have-been-made-public"></a><span data-ttu-id="11eba-663">Методы IRelationalDatabaseCreator.HasTables/HasTablesAsync стали общедоступными</span><span class="sxs-lookup"><span data-stu-id="11eba-663">IRelationalDatabaseCreator.HasTables/HasTablesAsync have been made public</span></span>

[<span data-ttu-id="11eba-664">Отслеживание вопроса № 15997</span><span class="sxs-lookup"><span data-stu-id="11eba-664">Tracking Issue #15997</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15997)

<span data-ttu-id="11eba-665">Это изменение реализовано в EF Core 3.0, предварительная версия 7.</span><span class="sxs-lookup"><span data-stu-id="11eba-665">This change is introduced in EF Core 3.0-preview 7.</span></span>

<span data-ttu-id="11eba-666">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="11eba-666">**Old behavior**</span></span>

<span data-ttu-id="11eba-667">До выпуска версии EF Core 3.0 эти методы были защищены.</span><span class="sxs-lookup"><span data-stu-id="11eba-667">Before EF Core 3.0, these methods were protected.</span></span>

```C#
var constraintName = myForeignKey.Name;
```

<span data-ttu-id="11eba-668">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="11eba-668">**New behavior**</span></span>

<span data-ttu-id="11eba-669">Начиная с EF Core 3.0 эти методы стали общедоступными.</span><span class="sxs-lookup"><span data-stu-id="11eba-669">Starting with EF Core 3.0, these methods are public.</span></span>

<span data-ttu-id="11eba-670">**Причина**</span><span class="sxs-lookup"><span data-stu-id="11eba-670">**Why**</span></span>

<span data-ttu-id="11eba-671">Эти методы используются EF для определения того, была ли база данных создана пустой.</span><span class="sxs-lookup"><span data-stu-id="11eba-671">These methods are used by EF to determine if a database is created but empty.</span></span> <span data-ttu-id="11eba-672">Также они могут оказаться полезными при работе с другими решениями (не EF) для определения того, следует ли применять миграцию.</span><span class="sxs-lookup"><span data-stu-id="11eba-672">This can also be useful from outside EF when determining whether or not to apply migrations.</span></span>

<span data-ttu-id="11eba-673">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="11eba-673">**Mitigations**</span></span>

<span data-ttu-id="11eba-674">Измените уровень доступа для всех переопределений.</span><span class="sxs-lookup"><span data-stu-id="11eba-674">Change the accessibility of any overrides.</span></span>

## <a name="microsoftentityframeworkcoredesign-is-now-a-developmentdependency-package"></a><span data-ttu-id="11eba-675">Microsoft.EntityFrameworkCore.Design теперь является пакетом DevelopmentDependency</span><span class="sxs-lookup"><span data-stu-id="11eba-675">Microsoft.EntityFrameworkCore.Design is now a DevelopmentDependency package</span></span>

[<span data-ttu-id="11eba-676">Отслеживание вопроса № 11506</span><span class="sxs-lookup"><span data-stu-id="11eba-676">Tracking Issue #11506</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/11506)

<span data-ttu-id="11eba-677">Это изменение реализовано в EF Core 3.0, предварительная версия 4.</span><span class="sxs-lookup"><span data-stu-id="11eba-677">This change is introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="11eba-678">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="11eba-678">**Old behavior**</span></span>

<span data-ttu-id="11eba-679">До выпуска EF Core 3.0 мы предоставляли Microsoft.EntityFrameworkCore.Design в виде обычного пакета NuGet, на сборку которого могли ссылаться зависящие от него проекты.</span><span class="sxs-lookup"><span data-stu-id="11eba-679">Before EF Core 3.0, Microsoft.EntityFrameworkCore.Design was a regular NuGet package whose assembly could be referenced by projects that depended on it.</span></span>

<span data-ttu-id="11eba-680">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="11eba-680">**New behavior**</span></span>

<span data-ttu-id="11eba-681">Начиная версии с EF Core 3.0, используется пакет DevelopmentDependency.</span><span class="sxs-lookup"><span data-stu-id="11eba-681">Starting with EF Core 3.0, it is a DevelopmentDependency package.</span></span> <span data-ttu-id="11eba-682">Это означает, что зависимость не будет транзитивно передаваться в другие проекты и вы больше не сможете по умолчанию ссылаться на сборку пакета.</span><span class="sxs-lookup"><span data-stu-id="11eba-682">Which means that the dependency won't flow transitively into other projects, and that you can no longer, by default, reference its assembly.</span></span>

<span data-ttu-id="11eba-683">**Причина**</span><span class="sxs-lookup"><span data-stu-id="11eba-683">**Why**</span></span>

<span data-ttu-id="11eba-684">Этот пакет предназначен для использования только во время разработки.</span><span class="sxs-lookup"><span data-stu-id="11eba-684">This package is only intended to be used at design time.</span></span> <span data-ttu-id="11eba-685">Развернутые приложения не должны ссылаться на него.</span><span class="sxs-lookup"><span data-stu-id="11eba-685">Deployed applications shouldn't reference it.</span></span> <span data-ttu-id="11eba-686">Эта рекомендация основана на том, что теперь используется пакет DevelopmentDependency.</span><span class="sxs-lookup"><span data-stu-id="11eba-686">Making the package a DevelopmentDependency reinforces this recommendation.</span></span>

<span data-ttu-id="11eba-687">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="11eba-687">**Mitigations**</span></span>

<span data-ttu-id="11eba-688">Если вам необходимо создать ссылку на этот пакет, чтобы переопределить поведение EF Core во время разработки, вы можете обновить метаданные элемента PackageReference в своем проекте.</span><span class="sxs-lookup"><span data-stu-id="11eba-688">If you need to reference this package to override EF Core's design-time behavior, you can update update PackageReference item metadata in your project.</span></span> <span data-ttu-id="11eba-689">Если ссылка на пакет указывается транзитивно через Microsoft.EntityFrameworkCore.Tools, вам нужно добавить явную ссылку PackageReference на пакет для изменения его метаданных.</span><span class="sxs-lookup"><span data-stu-id="11eba-689">If the package is being referenced transitively via Microsoft.EntityFrameworkCore.Tools, you will need to add an explicit PackageReference to the package to change its metadata.</span></span>

``` xml
<PackageReference Include="Microsoft.EntityFrameworkCore.Design" Version="3.0.0-preview4.19216.3">
  <PrivateAssets>all</PrivateAssets>
  <!-- Remove IncludeAssets to allow compiling against the assembly -->
  <!--<IncludeAssets>runtime; build; native; contentfiles; analyzers; buildtransitive</IncludeAssets>-->
</PackageReference>
```

## <a name="sqlitepclraw-updated-to-version-200"></a><span data-ttu-id="11eba-690">Библиотека SQLitePCL.raw обновлена до версии 2.0.0</span><span class="sxs-lookup"><span data-stu-id="11eba-690">SQLitePCL.raw updated to version 2.0.0</span></span>

[<span data-ttu-id="11eba-691">Отслеживание вопроса № 14824</span><span class="sxs-lookup"><span data-stu-id="11eba-691">Tracking Issue #14824</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14824)

<span data-ttu-id="11eba-692">Это изменение реализовано в EF Core 3.0, предварительная версия 7.</span><span class="sxs-lookup"><span data-stu-id="11eba-692">This change is introduced in EF Core 3.0-preview 7.</span></span>

<span data-ttu-id="11eba-693">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="11eba-693">**Old behavior**</span></span>

<span data-ttu-id="11eba-694">Пакет Microsoft.EntityFrameworkCore.Sqlite ранее зависел от библиотеки SQLitePCL.raw версии 1.1.12.</span><span class="sxs-lookup"><span data-stu-id="11eba-694">Microsoft.EntityFrameworkCore.Sqlite previously depended on version 1.1.12 of SQLitePCL.raw.</span></span>

<span data-ttu-id="11eba-695">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="11eba-695">**New behavior**</span></span>

<span data-ttu-id="11eba-696">Мы обновили пакет, и теперь он зависит от версии 2.0.0.</span><span class="sxs-lookup"><span data-stu-id="11eba-696">We've update our package to depend on version 2.0.0.</span></span>

<span data-ttu-id="11eba-697">**Причина**</span><span class="sxs-lookup"><span data-stu-id="11eba-697">**Why**</span></span>

<span data-ttu-id="11eba-698">Версия 2.0.0 библиотеки SQLitePCL.raw работает с .NET Standard 2.0.</span><span class="sxs-lookup"><span data-stu-id="11eba-698">Version 2.0.0 of SQLitePCL.raw targets .NET Standard 2.0.</span></span> <span data-ttu-id="11eba-699">Ранее она работала с .NET Standard 1.1, для чего требовалось выполнять крупное закрытие транзитивных пакетов.</span><span class="sxs-lookup"><span data-stu-id="11eba-699">It previously targeted .NET Standard 1.1 which required a large closure of transitive packages to work.</span></span>

<span data-ttu-id="11eba-700">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="11eba-700">**Mitigations**</span></span>

<span data-ttu-id="11eba-701">SQLitePCL.raw версии 2.0.0 включает некоторые критические изменения.</span><span class="sxs-lookup"><span data-stu-id="11eba-701">SQLitePCL.raw version 2.0.0 includes some breaking changes.</span></span> <span data-ttu-id="11eba-702">Подробные сведения см. в [заметках о выпуске](https://github.com/ericsink/SQLitePCL.raw/blob/v2/v2.md).</span><span class="sxs-lookup"><span data-stu-id="11eba-702">See the [release notes](https://github.com/ericsink/SQLitePCL.raw/blob/v2/v2.md) for details.</span></span>
