---
title: Критические изменения в EF Core 3.0 — EF Core
author: divega
ms.date: 02/19/2019
ms.assetid: EE2878C9-71F9-4FA5-9BC4-60517C7C9830
uid: core/what-is-new/ef-core-3.0/breaking-changes
ms.openlocfilehash: b1b5e286e08a8b6b4efe225a176e76023f9fdd20
ms.sourcegitcommit: 960e42a01b3a2f76da82e074f64f52252a8afecc
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/08/2019
ms.locfileid: "65405230"
---
# <a name="breaking-changes-included-in-ef-core-30-currently-in-preview"></a><span data-ttu-id="1dc3d-102">Критические изменения в EF Core 3.0 (сейчас предоставляются в виде предварительной версии)</span><span class="sxs-lookup"><span data-stu-id="1dc3d-102">Breaking changes included in EF Core 3.0 (currently in preview)</span></span>

> [!IMPORTANT]
> <span data-ttu-id="1dc3d-103">Обратите внимание на то, что наборы возможностей и расписания будущих выпусков могут быть изменены, и, несмотря на то, что мы стараемся поддерживать эту страницу в актуальном состоянии, в определенные моменты времени содержание страницы может не отражать наши текущие планы.</span><span class="sxs-lookup"><span data-stu-id="1dc3d-103">Please note that the feature sets and schedules of future releases are always subject to change, and although we will try to keep this page up to date, it may not reflect our latest plans at all times.</span></span>

<span data-ttu-id="1dc3d-104">Указанные ниже изменения API и поведения могут нарушать работу приложений, разработанных для EF Core 2.2.x, при их обновлении до версии 3.0.0.</span><span class="sxs-lookup"><span data-stu-id="1dc3d-104">The following API and behavior changes have the potential to break applications developed for EF Core 2.2.x when upgrading them to 3.0.0.</span></span>
<span data-ttu-id="1dc3d-105">Изменения, которые, по нашим ожиданиям, повлияют только на поставщиков баз данных, описаны в разделе [изменений для поставщиков](../../providers/provider-log.md).</span><span class="sxs-lookup"><span data-stu-id="1dc3d-105">Changes that we expect to only impact database providers are documented under [provider changes](../../providers/provider-log.md).</span></span>
<span data-ttu-id="1dc3d-106">Здесь не описаны критические изменения в новых функциях, добавленных между разными предварительными версиями 3.0.</span><span class="sxs-lookup"><span data-stu-id="1dc3d-106">Breaks in new features introduced from one 3.0 preview to another 3.0 preview aren't documented here.</span></span>

## <a name="linq-queries-are-no-longer-evaluated-on-the-client"></a><span data-ttu-id="1dc3d-107">Запросы LINQ больше не вычисляются на клиенте</span><span class="sxs-lookup"><span data-stu-id="1dc3d-107">LINQ queries are no longer evaluated on the client</span></span>

<span data-ttu-id="1dc3d-108">[Отслеживание вопроса № 14935](https://github.com/aspnet/EntityFrameworkCore/issues/14935)
[Также см. вопрос № 12795](https://github.com/aspnet/EntityFrameworkCore/issues/12795)</span><span class="sxs-lookup"><span data-stu-id="1dc3d-108">[Tracking Issue #14935](https://github.com/aspnet/EntityFrameworkCore/issues/14935)
[Also see issue #12795](https://github.com/aspnet/EntityFrameworkCore/issues/12795)</span></span>

<span data-ttu-id="1dc3d-109">Это изменение будет внесено в предварительную версию 4 EF Core 3.0.</span><span class="sxs-lookup"><span data-stu-id="1dc3d-109">This change will be introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="1dc3d-110">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="1dc3d-110">**Old behavior**</span></span>

<span data-ttu-id="1dc3d-111">До выхода версии 3.0, если системе EF Core не удавалось преобразовать выражение, являющееся частью запроса, в код SQL или параметр, она автоматически рассчитывала это выражение на клиенте.</span><span class="sxs-lookup"><span data-stu-id="1dc3d-111">Before 3.0, when EF Core couldn't convert an expression that was part of a query to either SQL or a parameter, it automatically evaluated the expression on the client.</span></span>
<span data-ttu-id="1dc3d-112">По умолчанию вычисление на клиенте потенциально ресурсоемких выражений вызывало лишь отображение предупреждения.</span><span class="sxs-lookup"><span data-stu-id="1dc3d-112">By default, client evaluation of potentially expensive expressions only triggered a warning.</span></span>

<span data-ttu-id="1dc3d-113">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="1dc3d-113">**New behavior**</span></span>

<span data-ttu-id="1dc3d-114">Начиная с версии 3.0 система EF Core разрешает вычислять на клиенте только выражения в высокоуровневой проекции (последний вызов `Select()` в запросе).</span><span class="sxs-lookup"><span data-stu-id="1dc3d-114">Starting with 3.0, EF Core only allows expressions in the top-level projection (the last `Select()` call in the query) to be evaluated on the client.</span></span>
<span data-ttu-id="1dc3d-115">Если выражения в любой другой части запроса невозможно преобразовать в код SQL или параметр, возникает исключение.</span><span class="sxs-lookup"><span data-stu-id="1dc3d-115">When expressions in any other part of the query can't be converted to either SQL or a parameter, an exception is thrown.</span></span>

<span data-ttu-id="1dc3d-116">**Причина**</span><span class="sxs-lookup"><span data-stu-id="1dc3d-116">**Why**</span></span>

<span data-ttu-id="1dc3d-117">Автоматическое вычисление запросов на клиенте позволяет выполнять многие запросы, даже если не удается преобразовать их важные части.</span><span class="sxs-lookup"><span data-stu-id="1dc3d-117">Automatic client evaluation of queries allows many queries to be executed even if important parts of them can't be translated.</span></span>
<span data-ttu-id="1dc3d-118">Это может привести к непредвиденному и потенциально опасному поведению, которое может стать заметным только в рабочей среде.</span><span class="sxs-lookup"><span data-stu-id="1dc3d-118">This behavior can result in unexpected and potentially damaging behavior that may only become evident in production.</span></span>
<span data-ttu-id="1dc3d-119">Например, условие в вызове `Where()`, которое невозможно преобразовать, может привести к передаче всех строк из таблицы с сервера базы данных и к применению фильтра на клиенте.</span><span class="sxs-lookup"><span data-stu-id="1dc3d-119">For example, a condition in a `Where()` call which can't be translated can cause all rows from the table to be transferred from the database server, and the filter to be applied on the client.</span></span>
<span data-ttu-id="1dc3d-120">Такая ситуация легко может остаться незамеченной, если таблица содержит небольшое число строк в разработке, и дать резкие отрицательные последствия при переносе приложения в рабочую среду, когда таблица может содержать миллионы строк.</span><span class="sxs-lookup"><span data-stu-id="1dc3d-120">This situation can easily go undetected if the table contains only a few rows in development, but hit hard when the application moves to production, where the table may contain millions of rows.</span></span>
<span data-ttu-id="1dc3d-121">Практика показала, что предупреждения о вычислении на клиенте также часто игнорируются во время разработки.</span><span class="sxs-lookup"><span data-stu-id="1dc3d-121">Client evaluation warnings also proved too easy to ignore during development.</span></span>

<span data-ttu-id="1dc3d-122">Кроме того, автоматическое вычисление на клиенте может привести к проблемам, из-за которых улучшение преобразования запросов для определенных выражений приводит к возникновению непреднамеренных критических изменений между выпусками.</span><span class="sxs-lookup"><span data-stu-id="1dc3d-122">Besides this, automatic client evaluation can lead to issues in which improving query translation for specific expressions caused unintended breaking changes between releases.</span></span>

<span data-ttu-id="1dc3d-123">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="1dc3d-123">**Mitigations**</span></span>

<span data-ttu-id="1dc3d-124">Если запрос невозможно преобразовать полностью, то перепишите его в форме, которую можно преобразовать, либо используйте `AsEnumerable()`, `ToList()` или аналогичный элемент, чтобы явно перенести данные обратно на клиент, где можно произвести их дальнейшую обработку с помощью LINQ-to-Objects.</span><span class="sxs-lookup"><span data-stu-id="1dc3d-124">If a query can't be fully translated, then either rewrite the query in a form that can be translated, or use `AsEnumerable()`, `ToList()`, or similar to explicitly bring data back to the client where it can then be further processed using LINQ-to-Objects.</span></span>

## <a name="entity-framework-core-is-no-longer-part-of-the-aspnet-core-shared-framework"></a><span data-ttu-id="1dc3d-125">Entity Framework Core больше не является частью общей платформы ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="1dc3d-125">Entity Framework Core is no longer part of the ASP.NET Core shared framework</span></span>

[<span data-ttu-id="1dc3d-126">Отслеживание объявлений о вопросе 325</span><span class="sxs-lookup"><span data-stu-id="1dc3d-126">Tracking Issue Announcements#325</span></span>](https://github.com/aspnet/Announcements/issues/325)

<span data-ttu-id="1dc3d-127">Это изменение было внесено в предварительную версию 1 ASP.NET Core 3.0.</span><span class="sxs-lookup"><span data-stu-id="1dc3d-127">This change was introduced in ASP.NET Core 3.0-preview 1.</span></span> 

<span data-ttu-id="1dc3d-128">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="1dc3d-128">**Old behavior**</span></span>

<span data-ttu-id="1dc3d-129">До выхода ASP.NET Core 3.0 при добавлении ссылки на пакет для `Microsoft.AspNetCore.App` или `Microsoft.AspNetCore.All` она включала в себя EF Core и некоторые поставщики данных EF Core, например поставщик SQL Server.</span><span class="sxs-lookup"><span data-stu-id="1dc3d-129">Before ASP.NET Core 3.0, when you added a package reference to `Microsoft.AspNetCore.App` or `Microsoft.AspNetCore.All`, it would include EF Core and some of the EF Core data providers like the SQL Server provider.</span></span>

<span data-ttu-id="1dc3d-130">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="1dc3d-130">**New behavior**</span></span>

<span data-ttu-id="1dc3d-131">Начиная с версии 3.0 общая платформа ASP.NET Core не включает в себя EF Core или поставщики данных EF Core.</span><span class="sxs-lookup"><span data-stu-id="1dc3d-131">Starting in 3.0, the ASP.NET Core shared framework doesn't include EF Core or any EF Core data providers.</span></span>

<span data-ttu-id="1dc3d-132">**Причина**</span><span class="sxs-lookup"><span data-stu-id="1dc3d-132">**Why**</span></span>

<span data-ttu-id="1dc3d-133">До этого изменения для получения EF Core требовались различные действия в зависимости от того, ориентировано ли приложение на ASP.NET Core и SQL Server.</span><span class="sxs-lookup"><span data-stu-id="1dc3d-133">Before this change, getting EF Core required different steps depending on whether the application targeted ASP.NET Core and SQL Server or not.</span></span> <span data-ttu-id="1dc3d-134">Кроме того, обновление ASP.NET Core приводило к принудительному обновлению EF Core и поставщика SQL Server, что иногда нежелательно.</span><span class="sxs-lookup"><span data-stu-id="1dc3d-134">Also, upgrading ASP.NET Core forced the upgrade of EF Core and the SQL Server provider, which isn't always desirable.</span></span>

<span data-ttu-id="1dc3d-135">Благодаря этому изменению процесс получения EF Core стал одинаковым для всех поставщиков, поддерживаемых реализаций .NET и типов приложений.</span><span class="sxs-lookup"><span data-stu-id="1dc3d-135">With this change, the experience of getting EF Core is the same across all providers, supported .NET implementations and application types.</span></span>
<span data-ttu-id="1dc3d-136">Кроме того, теперь разработчики могут точно управлять временем обновления EF Core и поставщиков данных EF Core.</span><span class="sxs-lookup"><span data-stu-id="1dc3d-136">Developers can also now control exactly when EF Core and EF Core data providers are upgraded.</span></span>

<span data-ttu-id="1dc3d-137">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="1dc3d-137">**Mitigations**</span></span>

<span data-ttu-id="1dc3d-138">Чтобы использовать EF Core в приложении ASP.NET Core 3.0 или любом другом поддерживаемом приложении, нужно явно добавить ссылку на пакет в поставщик базы данных EF Core, который ваше приложение будет использовать.</span><span class="sxs-lookup"><span data-stu-id="1dc3d-138">To use EF Core in an ASP.NET Core 3.0 application or any other supported application, explicitly add a package reference to the EF Core database provider that your application will use.</span></span>

## <a name="the-ef-core-command-line-tool-dotnet-ef-is-no-longer-part-of-the-net-core-sdk"></a><span data-ttu-id="1dc3d-139">Средство командной строки EF Core, dotnet ef больше не входит в пакет SDK для .NET Core</span><span class="sxs-lookup"><span data-stu-id="1dc3d-139">The EF Core command-line tool, dotnet ef, is no longer part of the .NET Core SDK</span></span>

[<span data-ttu-id="1dc3d-140">Отслеживание вопроса № 14016</span><span class="sxs-lookup"><span data-stu-id="1dc3d-140">Tracking Issue #14016</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14016)

<span data-ttu-id="1dc3d-141">Это изменение было представлено в EF Core 3.0 (предварительная версия 4) и соответствующей версии пакета SDK для .NET Core.</span><span class="sxs-lookup"><span data-stu-id="1dc3d-141">This change was introduced in EF Core 3.0-preview 4 and the corresponding version of the .NET Core SDK.</span></span>

<span data-ttu-id="1dc3d-142">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="1dc3d-142">**Old behavior**</span></span>

<span data-ttu-id="1dc3d-143">До версии 3.0 средство `dotnet ef` входило в состав пакета SDK для .NET Core и было доступно для использования из командной строки любого проекта без дополнительных действий.</span><span class="sxs-lookup"><span data-stu-id="1dc3d-143">Before 3.0, the `dotnet ef` tool was included in the .NET Core SDK and was readily available to use from the command line from any project without requiring extra steps.</span></span> 

<span data-ttu-id="1dc3d-144">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="1dc3d-144">**New behavior**</span></span>

<span data-ttu-id="1dc3d-145">Начиная с версии 3.0 средство `dotnet ef` не входит в состав пакета SDK для .NET, поэтому если вы хотите его использовать, установите его явным образом как локальное или глобальное средство.</span><span class="sxs-lookup"><span data-stu-id="1dc3d-145">Starting in 3.0, the .NET SDK does not incude the `dotnet ef` tool, so before you can use it you have to explicitly install it as a local or global tool.</span></span> 

<span data-ttu-id="1dc3d-146">**Причина**</span><span class="sxs-lookup"><span data-stu-id="1dc3d-146">**Why**</span></span>

<span data-ttu-id="1dc3d-147">Это изменение позволяет распространять и обновлять `dotnet ef` как обычное средство .NET CLI в NuGet в соответствии с тем, что EF Core 3.0 также всегда распространяется в виде пакета NuGet.</span><span class="sxs-lookup"><span data-stu-id="1dc3d-147">This change allows us to distribute and update `dotnet ef` as a regular .NET CLI tool on NuGet, consistent with the fact that the EF Core 3.0 is also always distributed as a NuGet package.</span></span>

<span data-ttu-id="1dc3d-148">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="1dc3d-148">**Mitigations**</span></span>

<span data-ttu-id="1dc3d-149">Чтобы управлять миграциями или сформировать шаблон `DbContext`, установите `dotnet-ef` с помощью команды `dotnet tool install`.</span><span class="sxs-lookup"><span data-stu-id="1dc3d-149">To be able to manage migrations or scaffold a `DbContext`, install `dotnet-ef` using the `dotnet tool install` command.</span></span>
<span data-ttu-id="1dc3d-150">Например, чтобы установить это средство как глобальное, можно ввести эту команду:</span><span class="sxs-lookup"><span data-stu-id="1dc3d-150">For example, to install it as a global tool, you can type this command:</span></span>

  ``` console
  $ dotnet tool install --global dotnet-ef --version <exact-version>
  ```

<span data-ttu-id="1dc3d-151">Это средство можно получить в виде локального инструмента при восстановлении зависимостей проекта, в котором оно объявляется как соответствующая зависимость, с помощью [файла манифеста средства](https://github.com/dotnet/cli/issues/10288).</span><span class="sxs-lookup"><span data-stu-id="1dc3d-151">You can also obtain it a local tool when you restore the dependencies of a project that declares it as a tooling dependency using a [tool manifest file](https://github.com/dotnet/cli/issues/10288).</span></span>

## <a name="fromsql-executesql-and-executesqlasync-have-been-renamed"></a><span data-ttu-id="1dc3d-152">FromSql, ExecuteSql и ExecuteSqlAsync были переименованы</span><span class="sxs-lookup"><span data-stu-id="1dc3d-152">FromSql, ExecuteSql, and ExecuteSqlAsync have been renamed</span></span>

[<span data-ttu-id="1dc3d-153">Отслеживание вопроса № 10996</span><span class="sxs-lookup"><span data-stu-id="1dc3d-153">Tracking Issue #10996</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10996)

<span data-ttu-id="1dc3d-154">Это изменение было внесено в предварительную версию 4 EF Core 3.0.</span><span class="sxs-lookup"><span data-stu-id="1dc3d-154">This change was introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="1dc3d-155">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="1dc3d-155">**Old behavior**</span></span>

<span data-ttu-id="1dc3d-156">До выхода EF Core 3.0 имена этих методов были перегружены для работы с обычной строкой или строкой, которая должна быть интерполирована в SQL и параметры.</span><span class="sxs-lookup"><span data-stu-id="1dc3d-156">Before EF Core 3.0, these method names were overloaded to work with either a normal string or a string that should be interpolated into SQL and parameters.</span></span>

<span data-ttu-id="1dc3d-157">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="1dc3d-157">**New behavior**</span></span>

<span data-ttu-id="1dc3d-158">Начиная с EF Core 3.0 используйте `FromSqlRaw`, `ExecuteSqlRaw` и `ExecuteSqlRawAsync` для создания параметризованного запроса, где параметры передаются отдельно из строки запроса.</span><span class="sxs-lookup"><span data-stu-id="1dc3d-158">Starting with EF Core 3.0, use `FromSqlRaw`, `ExecuteSqlRaw`, and `ExecuteSqlRawAsync` to create a parameterized query where the parameters are passed separately from the query string.</span></span>
<span data-ttu-id="1dc3d-159">Например:</span><span class="sxs-lookup"><span data-stu-id="1dc3d-159">For example:</span></span>

```C#
context.Products.FromSqlRaw(
    "SELECT * FROM Products WHERE Name = {0}",
    product.Name);
```

<span data-ttu-id="1dc3d-160">Используйте `FromSqlInterpolated`, `ExecuteSqlInterpolated` и `ExecuteSqlInterpolatedAsync` для создания параметризованного запроса, где параметры передаются как часть интерполированной строки запроса.</span><span class="sxs-lookup"><span data-stu-id="1dc3d-160">Use `FromSqlInterpolated`, `ExecuteSqlInterpolated`, and `ExecuteSqlInterpolatedAsync` to create a parameterized query where the parameters are passed as part of an interpolated query string.</span></span>
<span data-ttu-id="1dc3d-161">Например:</span><span class="sxs-lookup"><span data-stu-id="1dc3d-161">For example:</span></span>

```C#
context.Products.FromSqlInterpolated(
    $"SELECT * FROM Products WHERE Name = {product.Name}");
```

<span data-ttu-id="1dc3d-162">Обратите внимание, что оба упомянутых выше запроса будут возвращать одинаковый параметризованный SQL с одинаковыми параметрами SQL.</span><span class="sxs-lookup"><span data-stu-id="1dc3d-162">Note that both of the queries above will produce the same parameterized SQL with the same SQL parameters.</span></span>

<span data-ttu-id="1dc3d-163">**Причина**</span><span class="sxs-lookup"><span data-stu-id="1dc3d-163">**Why**</span></span>

<span data-ttu-id="1dc3d-164">Подобные перегрузки методов могут случайно вызывать метод необработанной строки в тех случаях, когда требовалось вызвать метод интерполированной строки, и наоборот.</span><span class="sxs-lookup"><span data-stu-id="1dc3d-164">Method overloads like this make it very easy to accidentally call the raw string method when the intent was to call the interpolated string method, and the other way around.</span></span>
<span data-ttu-id="1dc3d-165">Это может привести к тому, что запросы не параметризуются, хотя они должны параметризоваться.</span><span class="sxs-lookup"><span data-stu-id="1dc3d-165">This could result in queries not being parameterized when they should have been.</span></span>

<span data-ttu-id="1dc3d-166">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="1dc3d-166">**Mitigations**</span></span>

<span data-ttu-id="1dc3d-167">Перейдите на использование новых имен методов.</span><span class="sxs-lookup"><span data-stu-id="1dc3d-167">Switch to use the new method names.</span></span>

## <a name="query-execution-is-logged-at-debug-level"></a><span data-ttu-id="1dc3d-168">Выполнение запроса регистрируется на уровне отладки Debug</span><span class="sxs-lookup"><span data-stu-id="1dc3d-168">Query execution is logged at Debug level</span></span>

[<span data-ttu-id="1dc3d-169">Отслеживание вопроса 14523</span><span class="sxs-lookup"><span data-stu-id="1dc3d-169">Tracking Issue #14523</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14523)

<span data-ttu-id="1dc3d-170">Это изменение было внесено в предварительную версию 3 EF Core 3.0.</span><span class="sxs-lookup"><span data-stu-id="1dc3d-170">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="1dc3d-171">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="1dc3d-171">**Old behavior**</span></span>

<span data-ttu-id="1dc3d-172">До выхода EF Core 3.0 выполнение запросов и других команд зарегистрировалось на уровне `Info`.</span><span class="sxs-lookup"><span data-stu-id="1dc3d-172">Before EF Core 3.0, execution of queries and other commands was logged at the `Info` level.</span></span>

<span data-ttu-id="1dc3d-173">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="1dc3d-173">**New behavior**</span></span>

<span data-ttu-id="1dc3d-174">Начиная с EF Core 3.0 регистрация выполнения команды или кода SQL осуществляется на уровне `Debug`.</span><span class="sxs-lookup"><span data-stu-id="1dc3d-174">Starting with EF Core 3.0, logging of command/SQL execution is at the `Debug` level.</span></span>

<span data-ttu-id="1dc3d-175">**Причина**</span><span class="sxs-lookup"><span data-stu-id="1dc3d-175">**Why**</span></span>

<span data-ttu-id="1dc3d-176">Это изменение было внесено для снижения шума на уровне ведения журнала `Info`.</span><span class="sxs-lookup"><span data-stu-id="1dc3d-176">This change was made to reduce the noise at the `Info` log level.</span></span>

<span data-ttu-id="1dc3d-177">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="1dc3d-177">**Mitigations**</span></span>

<span data-ttu-id="1dc3d-178">Это событие ведения журнала определено `RelationalEventId.CommandExecuting` с идентификатором события 20100.</span><span class="sxs-lookup"><span data-stu-id="1dc3d-178">This logging event is defined by `RelationalEventId.CommandExecuting` with event ID 20100.</span></span>
<span data-ttu-id="1dc3d-179">Чтобы снова начать ведение журнала SQL на уровне `Info`, явно настройте уровень в методе `OnConfiguring` или `AddDbContext`.</span><span class="sxs-lookup"><span data-stu-id="1dc3d-179">To log SQL at the `Info` level again, explicitly configure the level in `OnConfiguring` or `AddDbContext`.</span></span>
<span data-ttu-id="1dc3d-180">Например:</span><span class="sxs-lookup"><span data-stu-id="1dc3d-180">For example:</span></span>
```C#
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    => optionsBuilder
        .UseSqlServer(connectionString)
        .ConfigureWarnings(c => c.Log((RelationalEventId.CommandExecuting, LogLevel.Info)));
```

## <a name="temporary-key-values-are-no-longer-set-onto-entity-instances"></a><span data-ttu-id="1dc3d-181">Временные значения ключа больше не устанавливаются для экземпляров сущностей</span><span class="sxs-lookup"><span data-stu-id="1dc3d-181">Temporary key values are no longer set onto entity instances</span></span>

[<span data-ttu-id="1dc3d-182">Отслеживание вопроса 12378</span><span class="sxs-lookup"><span data-stu-id="1dc3d-182">Tracking Issue #12378</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12378)

<span data-ttu-id="1dc3d-183">Это изменение было внесено в предварительную версию 2 EF Core 3.0.</span><span class="sxs-lookup"><span data-stu-id="1dc3d-183">This change was introduced in EF Core 3.0-preview 2.</span></span>

<span data-ttu-id="1dc3d-184">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="1dc3d-184">**Old behavior**</span></span>

<span data-ttu-id="1dc3d-185">До выхода EF Core 3.0 временные значения назначались всем свойствам ключей, которые позднее получили бы реальное значение, созданное базой данных.</span><span class="sxs-lookup"><span data-stu-id="1dc3d-185">Before EF Core 3.0, temporary values were assigned to all key properties that would later have a real value generated by the database.</span></span>
<span data-ttu-id="1dc3d-186">Обычно эти временные значения были большими отрицательными числами.</span><span class="sxs-lookup"><span data-stu-id="1dc3d-186">Usually these temporary values were large negative numbers.</span></span>

<span data-ttu-id="1dc3d-187">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="1dc3d-187">**New behavior**</span></span>

<span data-ttu-id="1dc3d-188">Начиная с версии 3.0 EF Core сохраняет временное значение ключа как часть сведений об отслеживании сущности, оставляя само свойство ключа без изменений.</span><span class="sxs-lookup"><span data-stu-id="1dc3d-188">Starting with 3.0, EF Core stores the temporary key value as part of the entity's tracking information, and leaves the key property itself unchanged.</span></span>

<span data-ttu-id="1dc3d-189">**Причина**</span><span class="sxs-lookup"><span data-stu-id="1dc3d-189">**Why**</span></span>

<span data-ttu-id="1dc3d-190">Это изменение было внесено для ситуации, когда временные значения ключей ошибочно становились постоянными из-за перемещения сущности, которая ранее отслеживалась некоторым экземпляром `DbContext`, в другой экземпляр `DbContext`.</span><span class="sxs-lookup"><span data-stu-id="1dc3d-190">This change was made to prevent temporary key values from erroneously becoming permanent when an entity that has been previously tracked by some `DbContext` instance is moved to a different `DbContext` instance.</span></span> 

<span data-ttu-id="1dc3d-191">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="1dc3d-191">**Mitigations**</span></span>

<span data-ttu-id="1dc3d-192">Приложения, которые назначают значения первичного ключа внешним ключам для формирования ассоциаций между сущностями, могут использовать старое поведение, если первичные ключи сформированы хранилищем и принадлежат сущностям в состоянии `Added`.</span><span class="sxs-lookup"><span data-stu-id="1dc3d-192">Applications that assign primary key values onto foreign keys to form associations between entities may depend on the old behavior if the primary keys are store-generated and belong to entities in the `Added` state.</span></span>
<span data-ttu-id="1dc3d-193">Этого можно избежать следующим образом:</span><span class="sxs-lookup"><span data-stu-id="1dc3d-193">This can be avoided by:</span></span>
* <span data-ttu-id="1dc3d-194">Отказ от использования ключей, сформированных хранилищем.</span><span class="sxs-lookup"><span data-stu-id="1dc3d-194">Not using store-generated keys.</span></span>
* <span data-ttu-id="1dc3d-195">Настройка свойств навигации для формирования отношений вместо задания значений внешнего ключа.</span><span class="sxs-lookup"><span data-stu-id="1dc3d-195">Setting navigation properties to form relationships instead of setting foreign key values.</span></span>
* <span data-ttu-id="1dc3d-196">Получение фактических временных значений ключа из сведений об отслеживании сущности.</span><span class="sxs-lookup"><span data-stu-id="1dc3d-196">Obtain the actual temporary key values from the entity's tracking information.</span></span>
<span data-ttu-id="1dc3d-197">Например, `context.Entry(blog).Property(e => e.Id).CurrentValue` возвратит временное значение, даже если сам `blog.Id` не был задан.</span><span class="sxs-lookup"><span data-stu-id="1dc3d-197">For example, `context.Entry(blog).Property(e => e.Id).CurrentValue` will return the temporary value even though `blog.Id` itself hasn't been set.</span></span>

## <a name="detectchanges-honors-store-generated-key-values"></a><span data-ttu-id="1dc3d-198">DetectChanges учитывает значения ключей, сформированные хранилищем</span><span class="sxs-lookup"><span data-stu-id="1dc3d-198">DetectChanges honors store-generated key values</span></span>

[<span data-ttu-id="1dc3d-199">Отслеживание вопроса 14616</span><span class="sxs-lookup"><span data-stu-id="1dc3d-199">Tracking Issue #14616</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14616)

<span data-ttu-id="1dc3d-200">Это изменение было внесено в предварительную версию 3 EF Core 3.0.</span><span class="sxs-lookup"><span data-stu-id="1dc3d-200">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="1dc3d-201">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="1dc3d-201">**Old behavior**</span></span>

<span data-ttu-id="1dc3d-202">До выпуска EF Core 3.0 неотслеживаемая сущность, обнаруженная `DetectChanges`, отслеживалась в состоянии `Added` и вставлялась в новую строку при вызове `SaveChanges`.</span><span class="sxs-lookup"><span data-stu-id="1dc3d-202">Before EF Core 3.0, an untracked entity found by `DetectChanges` would be tracked in the `Added` state and inserted as a new row when `SaveChanges` is called.</span></span>

<span data-ttu-id="1dc3d-203">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="1dc3d-203">**New behavior**</span></span>

<span data-ttu-id="1dc3d-204">Начиная с EF Core 3.0, если сущность использует сформированные значения ключа и какое-либо значение ключа задано, то она будет отслеживаться в состоянии `Modified`.</span><span class="sxs-lookup"><span data-stu-id="1dc3d-204">Starting with EF Core 3.0, if an entity is using generated key values and some key value is set, then the entity will be tracked in the `Modified` state.</span></span>
<span data-ttu-id="1dc3d-205">Это означает, что предполагается наличие строки для сущности, и она будет обновлена при вызове `SaveChanges`.</span><span class="sxs-lookup"><span data-stu-id="1dc3d-205">This means that a row for the entity is assumed to exist and it will be updated when `SaveChanges` is called.</span></span>
<span data-ttu-id="1dc3d-206">Если значение ключа не задано или тип сущности не использует сформированные ключи, то новая сущность все равно будет отслеживаться в состоянии `Added`, как в предыдущих версиях.</span><span class="sxs-lookup"><span data-stu-id="1dc3d-206">If the key value isn't set, or if the entity type isn't using generated keys, then the new entity will still be tracked as `Added` as in previous versions.</span></span>

<span data-ttu-id="1dc3d-207">**Причина**</span><span class="sxs-lookup"><span data-stu-id="1dc3d-207">**Why**</span></span>

<span data-ttu-id="1dc3d-208">Это изменение было внесено, чтобы сделать работу с несвязными графами сущностей при использовании ключей, сформированных хранилищем, более простой и согласованной.</span><span class="sxs-lookup"><span data-stu-id="1dc3d-208">This change was made to make it easier and more consistent to work with disconnected entity graphs while using store-generated keys.</span></span>

<span data-ttu-id="1dc3d-209">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="1dc3d-209">**Mitigations**</span></span>

<span data-ttu-id="1dc3d-210">Это изменение может нарушить работу приложения, если тип сущности должен использовать сформированные ключи, однако значения ключей явно заданы для новых экземпляров.</span><span class="sxs-lookup"><span data-stu-id="1dc3d-210">This change can break an application if an entity type is configured to use generated keys but key values are explicitly set for new instances.</span></span>
<span data-ttu-id="1dc3d-211">Решение проблемы заключается в явном запрете свойствам ключей использовать сформированные значения.</span><span class="sxs-lookup"><span data-stu-id="1dc3d-211">The fix is to explicitly configure the key properties to not use generated values.</span></span>
<span data-ttu-id="1dc3d-212">Например, с помощью текучего API:</span><span class="sxs-lookup"><span data-stu-id="1dc3d-212">For example, with the fluent API:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .ValueGeneratedNever();
```

<span data-ttu-id="1dc3d-213">Или с помощью заметок к данным:</span><span class="sxs-lookup"><span data-stu-id="1dc3d-213">Or with data annotations:</span></span>

```C#
[DatabaseGenerated(DatabaseGeneratedOption.None)]
public string Id { get; set; }
```

## <a name="cascade-deletions-now-happen-immediately-by-default"></a><span data-ttu-id="1dc3d-214">Каскадные удаления теперь по умолчанию выполняются немедленно</span><span class="sxs-lookup"><span data-stu-id="1dc3d-214">Cascade deletions now happen immediately by default</span></span>

[<span data-ttu-id="1dc3d-215">Отслеживание вопроса 10114</span><span class="sxs-lookup"><span data-stu-id="1dc3d-215">Tracking Issue #10114</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10114)

<span data-ttu-id="1dc3d-216">Это изменение было внесено в предварительную версию 3 EF Core 3.0.</span><span class="sxs-lookup"><span data-stu-id="1dc3d-216">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="1dc3d-217">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="1dc3d-217">**Old behavior**</span></span>

<span data-ttu-id="1dc3d-218">До выхода версии 3.0 применяемые EF Core каскадные действия (удаление зависимых сущностей, когда требуемый субъект удален либо отношение с ним было разорвано) не выполнялись до вызова SaveChanges.</span><span class="sxs-lookup"><span data-stu-id="1dc3d-218">Before 3.0, EF Core applied cascading actions (deleting dependent entities when a required principal is deleted or when the relationship to a required principal is severed) did not happen until SaveChanges was called.</span></span>

<span data-ttu-id="1dc3d-219">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="1dc3d-219">**New behavior**</span></span>

<span data-ttu-id="1dc3d-220">Начиная с версии 3.0 EF Core применяет каскадные действия сразу после обнаружения условия триггера.</span><span class="sxs-lookup"><span data-stu-id="1dc3d-220">Starting with 3.0, EF Core applies cascading actions as soon as the triggering condition is detected.</span></span>
<span data-ttu-id="1dc3d-221">Например, вызов `context.Remove()` для удаления сущности субъекта приведет к немедленной установке всех отслеживаемых связанных необходимых зависимых объектов в состояние `Deleted`.</span><span class="sxs-lookup"><span data-stu-id="1dc3d-221">For example, calling `context.Remove()` to delete a principal entity will result in all tracked related required dependents also being set to `Deleted` immediately.</span></span>

<span data-ttu-id="1dc3d-222">**Причина**</span><span class="sxs-lookup"><span data-stu-id="1dc3d-222">**Why**</span></span>

<span data-ttu-id="1dc3d-223">Это изменение было внесено, чтобы улучшить работу в сценариях аудита и привязки данных, где важно понимать, какие сущности будут удалены _перед_ вызовом `SaveChanges`.</span><span class="sxs-lookup"><span data-stu-id="1dc3d-223">This change was made to improve the experience for data binding and auditing scenarios where it is important to understand which entities will be deleted _before_ `SaveChanges` is called.</span></span>

<span data-ttu-id="1dc3d-224">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="1dc3d-224">**Mitigations**</span></span>

<span data-ttu-id="1dc3d-225">Для восстановления прежнего поведения можно использовать параметры `context.ChangedTracker`.</span><span class="sxs-lookup"><span data-stu-id="1dc3d-225">The previous behavior can be restored through settings on `context.ChangedTracker`.</span></span>
<span data-ttu-id="1dc3d-226">Например:</span><span class="sxs-lookup"><span data-stu-id="1dc3d-226">For example:</span></span>

```C#
context.ChangeTracker.CascadeDeleteTiming = CascadeTiming.OnSaveChanges;
context.ChangeTracker.DeleteOrphansTiming = CascadeTiming.OnSaveChanges;
```

## <a name="deletebehaviorrestrict-has-cleaner-semantics"></a><span data-ttu-id="1dc3d-227">более четкая семантика DeleteBehavior.Restrict</span><span class="sxs-lookup"><span data-stu-id="1dc3d-227">DeleteBehavior.Restrict has cleaner semantics</span></span>

[<span data-ttu-id="1dc3d-228">Отслеживание вопроса № 12661</span><span class="sxs-lookup"><span data-stu-id="1dc3d-228">Tracking Issue #12661</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12661)

<span data-ttu-id="1dc3d-229">Это изменение будет внесено в предварительную версию 5 EF Core 3.0.</span><span class="sxs-lookup"><span data-stu-id="1dc3d-229">This change will be introduced in EF Core 3.0-preview 5.</span></span>

<span data-ttu-id="1dc3d-230">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="1dc3d-230">**Old behavior**</span></span>

<span data-ttu-id="1dc3d-231">До версии 3.0 `DeleteBehavior.Restrict` создавал внешние ключи в базе данных с помощью семантики `Restrict`, но также изменял внутреннее исправление неочевидным образом.</span><span class="sxs-lookup"><span data-stu-id="1dc3d-231">Before 3.0, `DeleteBehavior.Restrict` created foreign keys in the database with `Restrict` semantics, but also changed internal fixup in a non-obvious way.</span></span>

<span data-ttu-id="1dc3d-232">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="1dc3d-232">**New behavior**</span></span>

<span data-ttu-id="1dc3d-233">Начиная с версии 3.0 `DeleteBehavior.Restrict` обеспечивает создание внешних ключей с помощью семантики `Restrict`, то есть без каскадов; создается при нарушении ограничения, не влияя на внутреннее исправление EF.</span><span class="sxs-lookup"><span data-stu-id="1dc3d-233">Starting with 3.0, `DeleteBehavior.Restrict` ensures that foreign keys are created with `Restrict` semantics--that is, no cascades; throw on constraint violation--without also impacting EF internal fixup.</span></span>

<span data-ttu-id="1dc3d-234">**Причина**</span><span class="sxs-lookup"><span data-stu-id="1dc3d-234">**Why**</span></span>

<span data-ttu-id="1dc3d-235">Это изменение было внесено, чтобы упростить работу с помощью более интуитивного использования `DeleteBehavior` без непредвиденных побочных эффектов.</span><span class="sxs-lookup"><span data-stu-id="1dc3d-235">This change was made to improve the experience for using `DeleteBehavior` in an intuitive manner, without unexpected side-effects.</span></span>

<span data-ttu-id="1dc3d-236">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="1dc3d-236">**Mitigations**</span></span>

<span data-ttu-id="1dc3d-237">Для восстановления прежнего поведения можно использовать `DeleteBehavior.ClientNoAction`.</span><span class="sxs-lookup"><span data-stu-id="1dc3d-237">The previous behavior can be restored by using `DeleteBehavior.ClientNoAction`.</span></span>

## <a name="query-types-are-consolidated-with-entity-types"></a><span data-ttu-id="1dc3d-238">Типы запросов объединяются с типами сущностей</span><span class="sxs-lookup"><span data-stu-id="1dc3d-238">Query types are consolidated with entity types</span></span>

[<span data-ttu-id="1dc3d-239">Отслеживание вопроса 14194</span><span class="sxs-lookup"><span data-stu-id="1dc3d-239">Tracking Issue #14194</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14194)

<span data-ttu-id="1dc3d-240">Это изменение было внесено в предварительную версию 3 EF Core 3.0.</span><span class="sxs-lookup"><span data-stu-id="1dc3d-240">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="1dc3d-241">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="1dc3d-241">**Old behavior**</span></span>

<span data-ttu-id="1dc3d-242">До выпуска EF Core 3.0 [типы запросов](xref:core/modeling/query-types) представляли собой средства для запроса данных, не определяющие первичный ключ структурированным образом.</span><span class="sxs-lookup"><span data-stu-id="1dc3d-242">Before EF Core 3.0, [query types](xref:core/modeling/query-types) were a means to query data that doesn't define a primary key in a structured way.</span></span>
<span data-ttu-id="1dc3d-243">То есть тип запроса использовался для сопоставления типов сущностей без ключей (скорее всего, из представления, но, возможно, и из таблицы), а обычный тип сущности использовался при наличии ключа (скорее всего, из таблицы, но, возможно, и из представления).</span><span class="sxs-lookup"><span data-stu-id="1dc3d-243">That is, a query type was used for mapping entity types without keys (more likely from a view, but possibly from a table) while a regular entity type was used when a key was available (more likely from a table, but possibly from a view).</span></span>

<span data-ttu-id="1dc3d-244">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="1dc3d-244">**New behavior**</span></span>

<span data-ttu-id="1dc3d-245">Теперь тип запроса стал просто типом сущности без первичного ключа.</span><span class="sxs-lookup"><span data-stu-id="1dc3d-245">A query type now becomes just an entity type without a primary key.</span></span>
<span data-ttu-id="1dc3d-246">Типы сущностей без ключей имеют ту же функциональность, что типы запросов в предыдущих версиях.</span><span class="sxs-lookup"><span data-stu-id="1dc3d-246">Keyless entity types have the same functionality as query types in previous versions.</span></span>

<span data-ttu-id="1dc3d-247">**Причина**</span><span class="sxs-lookup"><span data-stu-id="1dc3d-247">**Why**</span></span>

<span data-ttu-id="1dc3d-248">Это изменение было внесено, чтобы избежать путаницы с назначением типов запросов.</span><span class="sxs-lookup"><span data-stu-id="1dc3d-248">This change was made to reduce the confusion around the purpose of query types.</span></span>
<span data-ttu-id="1dc3d-249">В частности, они представляют собой типы сущностей без ключей и поэтому по определению доступны только для чтения, однако их не следует использовать только потому, что тип сущности должен быть доступен только для чтения.</span><span class="sxs-lookup"><span data-stu-id="1dc3d-249">Specifically, they are keyless entity types and they are inherently read-only because of this, but they should not be used just because an entity type needs to be read-only.</span></span>
<span data-ttu-id="1dc3d-250">Аналогично, их часто сопоставляют с представлениями, но только потому, что представления часто не определяют ключи.</span><span class="sxs-lookup"><span data-stu-id="1dc3d-250">Likewise, they are often mapped to views, but this is only because views often don't define keys.</span></span>

<span data-ttu-id="1dc3d-251">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="1dc3d-251">**Mitigations**</span></span>

<span data-ttu-id="1dc3d-252">Следующие компоненты этого API теперь являются устаревшими:</span><span class="sxs-lookup"><span data-stu-id="1dc3d-252">The following parts of the API are now obsolete:</span></span>
* <span data-ttu-id="1dc3d-253">**`ModelBuilder.Query<>()`** — вместо него следует вызвать `ModelBuilder.Entity<>().HasNoKey()`, чтобы пометить тип сущности как не имеющий ключей.</span><span class="sxs-lookup"><span data-stu-id="1dc3d-253">**`ModelBuilder.Query<>()`** - Instead `ModelBuilder.Entity<>().HasNoKey()` needs to be called to mark an entity type as having no keys.</span></span>
<span data-ttu-id="1dc3d-254">Это по-прежнему не следует настраивать посредством соглашения, чтобы избежать ошибки, когда первичный ключ ожидается, но не соответствует соглашению.</span><span class="sxs-lookup"><span data-stu-id="1dc3d-254">This would still not be configured by convention to avoid misconfiguration when a primary key is expected, but doesn't match the convention.</span></span>
* <span data-ttu-id="1dc3d-255">**`DbQuery<>`** — вместо него следует использовать `DbSet<>`.</span><span class="sxs-lookup"><span data-stu-id="1dc3d-255">**`DbQuery<>`** - Instead `DbSet<>` should be used.</span></span>
* <span data-ttu-id="1dc3d-256">**`DbContext.Query<>()`** — вместо него следует использовать `DbContext.Set<>()`.</span><span class="sxs-lookup"><span data-stu-id="1dc3d-256">**`DbContext.Query<>()`** - Instead `DbContext.Set<>()` should be used.</span></span>

## <a name="configuration-api-for-owned-type-relationships-has-changed"></a><span data-ttu-id="1dc3d-257">Изменение API конфигурации для отношений принадлежащего типа</span><span class="sxs-lookup"><span data-stu-id="1dc3d-257">Configuration API for owned type relationships has changed</span></span>

<span data-ttu-id="1dc3d-258">[Отслеживание вопроса 12444](https://github.com/aspnet/EntityFrameworkCore/issues/12444)
[Отслеживание вопроса 9148](https://github.com/aspnet/EntityFrameworkCore/issues/9148)
[Отслеживание вопроса 14153](https://github.com/aspnet/EntityFrameworkCore/issues/14153)</span><span class="sxs-lookup"><span data-stu-id="1dc3d-258">[Tracking Issue #12444](https://github.com/aspnet/EntityFrameworkCore/issues/12444)
[Tracking Issue #9148](https://github.com/aspnet/EntityFrameworkCore/issues/9148)
[Tracking Issue #14153](https://github.com/aspnet/EntityFrameworkCore/issues/14153)</span></span>

<span data-ttu-id="1dc3d-259">Это изменение было внесено в предварительную версию 3 EF Core 3.0.</span><span class="sxs-lookup"><span data-stu-id="1dc3d-259">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="1dc3d-260">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="1dc3d-260">**Old behavior**</span></span>

<span data-ttu-id="1dc3d-261">До выхода EF Core 3.0 конфигурация принадлежащего отношения выполнялась непосредственно после вызова `OwnsOne` или `OwnsMany`.</span><span class="sxs-lookup"><span data-stu-id="1dc3d-261">Before EF Core 3.0, configuration of the owned relationship was performed directly after the `OwnsOne` or `OwnsMany` call.</span></span> 

<span data-ttu-id="1dc3d-262">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="1dc3d-262">**New behavior**</span></span>

<span data-ttu-id="1dc3d-263">Начиная с EF Core 3.0 существует текучий API для настройки свойства навигации для владельца с помощью `WithOwner()`.</span><span class="sxs-lookup"><span data-stu-id="1dc3d-263">Starting with EF Core 3.0, there is now fluent API to configure a navigation property to the owner using `WithOwner()`.</span></span>
<span data-ttu-id="1dc3d-264">Например:</span><span class="sxs-lookup"><span data-stu-id="1dc3d-264">For example:</span></span>

```C#
modelBuilder.Entity<Order>.OwnsOne(e => e.Details).WithOwner(e => e.Order);
```

<span data-ttu-id="1dc3d-265">Конфигурация, связанная с отношением между владельцем и принадлежащим объектом, теперь должна быть включена в цепочку после `WithOwner()` по аналогии с настройкой других отношений.</span><span class="sxs-lookup"><span data-stu-id="1dc3d-265">The configuration related to the relationship between owner and owned should now be chained after `WithOwner()` similarly to how other relationships are configured.</span></span>
<span data-ttu-id="1dc3d-266">При этом конфигурация для принадлежащего типа сама будет включена в цепочку после `OwnsOne()/OwnsMany()`.</span><span class="sxs-lookup"><span data-stu-id="1dc3d-266">While the configuration for the owned type itself would still be chained after `OwnsOne()/OwnsMany()`.</span></span>
<span data-ttu-id="1dc3d-267">Например:</span><span class="sxs-lookup"><span data-stu-id="1dc3d-267">For example:</span></span>

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

<span data-ttu-id="1dc3d-268">Кроме того, вызов `Entity()`, `HasOne()` или `Set()` с целевым объектом принадлежащего типа теперь приведет к возникновению исключения.</span><span class="sxs-lookup"><span data-stu-id="1dc3d-268">Additionally calling `Entity()`, `HasOne()`, or `Set()` with an owned type target will now throw an exception.</span></span>

<span data-ttu-id="1dc3d-269">**Причина**</span><span class="sxs-lookup"><span data-stu-id="1dc3d-269">**Why**</span></span>

<span data-ttu-id="1dc3d-270">Это изменение было внесено для более четкого разграничения между настройкой самого принадлежащего типа и _отношения_ с ним.</span><span class="sxs-lookup"><span data-stu-id="1dc3d-270">This change was made to create a cleaner separation between configuring the owned type itself and the _relationship to_ the owned type.</span></span>
<span data-ttu-id="1dc3d-271">Это, в свою очередь, устраняет неоднозначность и путаницу при использовании таких методов, как `HasForeignKey`.</span><span class="sxs-lookup"><span data-stu-id="1dc3d-271">This in turn removes ambiguity and confusion around methods like `HasForeignKey`.</span></span>

<span data-ttu-id="1dc3d-272">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="1dc3d-272">**Mitigations**</span></span>

<span data-ttu-id="1dc3d-273">Измените конфигурацию отношений принадлежащего типа, чтобы использовать новую поверхность API, как показано в приведенном выше примере.</span><span class="sxs-lookup"><span data-stu-id="1dc3d-273">Change configuration of owned type relationships to use the new API surface as shown in the example above.</span></span>

## <a name="dependent-entities-sharing-the-table-with-the-principal-are-now-optional"></a><span data-ttu-id="1dc3d-274">Зависимые сущности, имеющие общую с субъектом таблицу, теперь являются необязательными</span><span class="sxs-lookup"><span data-stu-id="1dc3d-274">Dependent entities sharing the table with the principal are now optional</span></span>

[<span data-ttu-id="1dc3d-275">Отслеживание вопроса № 9005</span><span class="sxs-lookup"><span data-stu-id="1dc3d-275">Tracking Issue #9005</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9005)

<span data-ttu-id="1dc3d-276">Это изменение будет внесено в предварительную версию 4 EF Core 3.0.</span><span class="sxs-lookup"><span data-stu-id="1dc3d-276">This change will be introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="1dc3d-277">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="1dc3d-277">**Old behavior**</span></span>

<span data-ttu-id="1dc3d-278">Рассмотрим следующую модель:</span><span class="sxs-lookup"><span data-stu-id="1dc3d-278">Consider the following model:</span></span>
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
<span data-ttu-id="1dc3d-279">До выхода EF Core 3.0, если класс `OrderDetails` принадлежал классу `Order` или явно сопоставлялся с этой же таблицей, при добавлении нового класса `Order` всегда требовался экземпляр `OrderDetails`.</span><span class="sxs-lookup"><span data-stu-id="1dc3d-279">Before EF Core 3.0, if `OrderDetails` is owned by `Order` or explicitly mapped to the same table then an `OrderDetails` instance was always required when adding a new `Order`.</span></span>


<span data-ttu-id="1dc3d-280">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="1dc3d-280">**New behavior**</span></span>

<span data-ttu-id="1dc3d-281">Начиная с версии 3.0 система EF Core позволяет добавлять класс `Order` без класса `OrderDetails` и сопоставляет все свойства `OrderDetails`, за исключением первичного ключа, со столбцами, допускающими значение NULL.</span><span class="sxs-lookup"><span data-stu-id="1dc3d-281">Starting with 3.0, EF Core allows to add an `Order` without an `OrderDetails` and maps all of the `OrderDetails` properties except the primary key to nullable columns.</span></span>
<span data-ttu-id="1dc3d-282">При отправке запроса EF Core задает классу `OrderDetails` значение `null`, если какому-либо его обязательному свойству не задано значение или если отсутствуют необходимые свойства помимо первичного ключа и все свойства имеют значение `null`.</span><span class="sxs-lookup"><span data-stu-id="1dc3d-282">When querying EF Core sets `OrderDetails` to `null` if any of its required properties doesn't have a value or if it has no required properties besides the primary key and all properties are `null`.</span></span>

<span data-ttu-id="1dc3d-283">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="1dc3d-283">**Mitigations**</span></span>

<span data-ttu-id="1dc3d-284">Если модель содержит общую таблицу со всеми необязательными столбцами, но указывающее на нее свойство навигации не должно иметь значение `null`, приложение следует изменить для обработки случаев, когда свойству навигации задано значение `null`.</span><span class="sxs-lookup"><span data-stu-id="1dc3d-284">If your model has a table sharing dependent with all optional columns, but the navigation pointing to it is not expected to be `null` then the application should be modified to handle cases when the navigation is `null`.</span></span> <span data-ttu-id="1dc3d-285">Если это невозможно, необходимо добавить обязательное свойство к типу сущности либо по меньшей мере одному свойству должно быть задано значение, отличное от `null`.</span><span class="sxs-lookup"><span data-stu-id="1dc3d-285">If this is not possible a required property should be added to the entity type or at least one property should have a non-`null` value assigned to it.</span></span>

## <a name="all-entities-sharing-a-table-with-a-concurrency-token-column-have-to-map-it-to-a-property"></a><span data-ttu-id="1dc3d-286">Все сущности, имеющие общую таблицу со столбцом маркера параллелизма, должны сопоставлять ее со свойством</span><span class="sxs-lookup"><span data-stu-id="1dc3d-286">All entities sharing a table with a concurrency token column have to map it to a property</span></span>

[<span data-ttu-id="1dc3d-287">Отслеживание вопроса № 14154</span><span class="sxs-lookup"><span data-stu-id="1dc3d-287">Tracking Issue #14154</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14154)

<span data-ttu-id="1dc3d-288">Это изменение будет внесено в предварительную версию 4 EF Core 3.0.</span><span class="sxs-lookup"><span data-stu-id="1dc3d-288">This change will be introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="1dc3d-289">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="1dc3d-289">**Old behavior**</span></span>

<span data-ttu-id="1dc3d-290">Рассмотрим следующую модель:</span><span class="sxs-lookup"><span data-stu-id="1dc3d-290">Consider the following model:</span></span>
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
<span data-ttu-id="1dc3d-291">До выхода EF Core 3.0, если класс `OrderDetails` принадлежал классу `Order` или явно сопоставлялся с той же таблицей, то обновление только класса `OrderDetails` не приводило к обновлению значения `Version` на клиенте и следующее обновление завершалось ошибкой.</span><span class="sxs-lookup"><span data-stu-id="1dc3d-291">Before EF Core 3.0, if `OrderDetails` is owned by `Order` or explicitly mapped to the same table then updating just `OrderDetails` will not update `Version` value on client and the next update will fail.</span></span>


<span data-ttu-id="1dc3d-292">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="1dc3d-292">**New behavior**</span></span>

<span data-ttu-id="1dc3d-293">Начиная с версии 3.0 система EF Core распространяет новое значение `Version` для класса `Order`, если ему принадлежит класс `OrderDetails`.</span><span class="sxs-lookup"><span data-stu-id="1dc3d-293">Starting with 3.0, EF Core propagates the new `Version` value to `Order` if it owns `OrderDetails`.</span></span> <span data-ttu-id="1dc3d-294">В противном случае во время проверки модели создается исключение.</span><span class="sxs-lookup"><span data-stu-id="1dc3d-294">Otherwise an exception is thrown during model validation.</span></span>

<span data-ttu-id="1dc3d-295">**Причина**</span><span class="sxs-lookup"><span data-stu-id="1dc3d-295">**Why**</span></span>

<span data-ttu-id="1dc3d-296">Это изменение было внесено, чтобы избежать получения устаревшего значения маркера параллелизма при обновлении только одной сущности, сопоставленной с той же таблицей.</span><span class="sxs-lookup"><span data-stu-id="1dc3d-296">This change was made to avoid a stale concurrency token value when only one of the entities mapped to the same table is updated.</span></span>

<span data-ttu-id="1dc3d-297">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="1dc3d-297">**Mitigations**</span></span>

<span data-ttu-id="1dc3d-298">Все сущности, имеющие общую таблицу, должны включать в себя свойство, сопоставленное со столбцом маркера параллелизма.</span><span class="sxs-lookup"><span data-stu-id="1dc3d-298">All entities sharing the table have to include a property that is mapped to the concurrency token column.</span></span> <span data-ttu-id="1dc3d-299">Можно создать свойство в теневом состоянии:</span><span class="sxs-lookup"><span data-stu-id="1dc3d-299">It's possible the create one in shadow-state:</span></span>
```C#
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.Entity<OrderDetails>()
        .Property<byte[]>("Version").IsRowVersion().HasColumnName("Version");
}
```

## <a name="inherited-properties-from-unmapped-types-are-now-mapped-to-a-single-column-for-all-derived-types"></a><span data-ttu-id="1dc3d-300">Наследуемые свойства из несопоставленных типов теперь сопоставляются с одним столбцом для всех производных типов</span><span class="sxs-lookup"><span data-stu-id="1dc3d-300">Inherited properties from unmapped types are now mapped to a single column for all derived types</span></span>

[<span data-ttu-id="1dc3d-301">Отслеживание вопроса № 13998</span><span class="sxs-lookup"><span data-stu-id="1dc3d-301">Tracking Issue #13998</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13998)

<span data-ttu-id="1dc3d-302">Это изменение будет внесено в предварительную версию 4 EF Core 3.0.</span><span class="sxs-lookup"><span data-stu-id="1dc3d-302">This change will be introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="1dc3d-303">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="1dc3d-303">**Old behavior**</span></span>

<span data-ttu-id="1dc3d-304">Рассмотрим следующую модель:</span><span class="sxs-lookup"><span data-stu-id="1dc3d-304">Consider the following model:</span></span>
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

<span data-ttu-id="1dc3d-305">До выхода EF Core 3.0 свойство `ShippingAddress` сопоставлялось с отдельными столбцами для классов `BulkOrder` и `Order` по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="1dc3d-305">Before EF Core 3.0, the `ShippingAddress` property would be mapped to separate columns for `BulkOrder` and `Order` by default.</span></span>

<span data-ttu-id="1dc3d-306">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="1dc3d-306">**New behavior**</span></span>

<span data-ttu-id="1dc3d-307">Начиная с версии 3.0 система EF Core создает только один столбец для класса `ShippingAddress`.</span><span class="sxs-lookup"><span data-stu-id="1dc3d-307">Starting with 3.0, EF Core only creates one column for `ShippingAddress`.</span></span>

<span data-ttu-id="1dc3d-308">**Причина**</span><span class="sxs-lookup"><span data-stu-id="1dc3d-308">**Why**</span></span>

<span data-ttu-id="1dc3d-309">Старое поведение было непредвиденным.</span><span class="sxs-lookup"><span data-stu-id="1dc3d-309">The old behavoir was unexpected.</span></span>

<span data-ttu-id="1dc3d-310">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="1dc3d-310">**Mitigations**</span></span>

<span data-ttu-id="1dc3d-311">Свойство можно по-прежнему явным образом сопоставлять с отдельным столбцом для производных типов.</span><span class="sxs-lookup"><span data-stu-id="1dc3d-311">The property can still be explicitly mapped to separate column on the derived types:</span></span>

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

## <a name="the-foreign-key-property-convention-no-longer-matches-same-name-as-the-principal-property"></a><span data-ttu-id="1dc3d-312">Соглашение для свойства внешнего ключа больше не совпадает со свойством субъекта по имени</span><span class="sxs-lookup"><span data-stu-id="1dc3d-312">The foreign key property convention no longer matches same name as the principal property</span></span>

[<span data-ttu-id="1dc3d-313">Отслеживание вопроса 13274</span><span class="sxs-lookup"><span data-stu-id="1dc3d-313">Tracking Issue #13274</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13274)

<span data-ttu-id="1dc3d-314">Это изменение было внесено в предварительную версию 3 EF Core 3.0.</span><span class="sxs-lookup"><span data-stu-id="1dc3d-314">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="1dc3d-315">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="1dc3d-315">**Old behavior**</span></span>

<span data-ttu-id="1dc3d-316">Рассмотрим следующую модель:</span><span class="sxs-lookup"><span data-stu-id="1dc3d-316">Consider the following model:</span></span>
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
<span data-ttu-id="1dc3d-317">До выхода EF Core 3.0 свойство `CustomerId` использовалось для внешнего ключа по соглашению.</span><span class="sxs-lookup"><span data-stu-id="1dc3d-317">Before EF Core 3.0, the `CustomerId` property would be used for the foreign key by convention.</span></span>
<span data-ttu-id="1dc3d-318">Однако если `Order` является принадлежащим типом, это также делает `CustomerId` первичным ключом, что обычно не соответствует ожидаемому результату.</span><span class="sxs-lookup"><span data-stu-id="1dc3d-318">However, if `Order` is an owned type, then this would also make `CustomerId` the primary key and this isn't usually the expectation.</span></span>

<span data-ttu-id="1dc3d-319">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="1dc3d-319">**New behavior**</span></span>

<span data-ttu-id="1dc3d-320">Начиная с версии 3.0 система EF Core не будет пытаться использовать свойства для внешних ключей по соглашению, если они имеют такое же имя, что и свойство субъекта.</span><span class="sxs-lookup"><span data-stu-id="1dc3d-320">Starting with 3.0, EF Core doesn't try to use properties for foreign keys by convention if they have the same name as the principal property.</span></span>
<span data-ttu-id="1dc3d-321">Шаблоны имени типа субъекта, сцепленного с именем свойства субъекта, и имени навигации, сцепленного с именем свойства субъекта, по-прежнему совпадают.</span><span class="sxs-lookup"><span data-stu-id="1dc3d-321">Principal type name concatenated with principal property name, and navigation name concatenated with principal property name patterns are still matched.</span></span>
<span data-ttu-id="1dc3d-322">Например:</span><span class="sxs-lookup"><span data-stu-id="1dc3d-322">For example:</span></span>

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

<span data-ttu-id="1dc3d-323">**Причина**</span><span class="sxs-lookup"><span data-stu-id="1dc3d-323">**Why**</span></span>

<span data-ttu-id="1dc3d-324">Это изменение было внесено во избежание ошибочного определения свойств первичного ключа для принадлежащего типа.</span><span class="sxs-lookup"><span data-stu-id="1dc3d-324">This change was made to avoid erroneously defining a primary key property on the owned type.</span></span>

<span data-ttu-id="1dc3d-325">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="1dc3d-325">**Mitigations**</span></span>

<span data-ttu-id="1dc3d-326">Если свойство должно быть внешним ключом и, следовательно, частью первичного ключа, следует явно указать это при его настройке.</span><span class="sxs-lookup"><span data-stu-id="1dc3d-326">If the property was intended to be the foreign key, and hence part of the primary key, then explicitly configure it as such.</span></span>

## <a name="database-connection-is-now-closed-if-not-used-anymore-before-the-transactionscope-has-been-completed"></a><span data-ttu-id="1dc3d-327">Соединение с базой данных теперь закрывается, если оно больше не используется до завершения TransactionScope</span><span class="sxs-lookup"><span data-stu-id="1dc3d-327">Database connection is now closed if not used anymore before the TransactionScope has been completed</span></span>

[<span data-ttu-id="1dc3d-328">Отслеживание вопроса № 14218</span><span class="sxs-lookup"><span data-stu-id="1dc3d-328">Tracking Issue #14218</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14218)

<span data-ttu-id="1dc3d-329">Это изменение будет внесено в предварительную версию 4 EF Core 3.0.</span><span class="sxs-lookup"><span data-stu-id="1dc3d-329">This change will be introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="1dc3d-330">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="1dc3d-330">**Old behavior**</span></span>

<span data-ttu-id="1dc3d-331">До выхода EF Core 3.0, если контекст открывает соединение внутри класса `TransactionScope`, оно остается открытым пока активен текущий класс `TransactionScope`.</span><span class="sxs-lookup"><span data-stu-id="1dc3d-331">Before EF Core 3.0, if the context opens the connection inside a `TransactionScope`, the connection remains open while the current `TransactionScope` is active.</span></span>

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

<span data-ttu-id="1dc3d-332">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="1dc3d-332">**New behavior**</span></span>

<span data-ttu-id="1dc3d-333">Начиная с версии 3.0 система EF Core закрывает соединение сразу же после прекращения его использования.</span><span class="sxs-lookup"><span data-stu-id="1dc3d-333">Starting with 3.0, EF Core closes the connection as soon as it's done using it.</span></span>

<span data-ttu-id="1dc3d-334">**Причина**</span><span class="sxs-lookup"><span data-stu-id="1dc3d-334">**Why**</span></span>

<span data-ttu-id="1dc3d-335">Это изменение позволяет использовать несколько контекстов в одном классе `TransactionScope`.</span><span class="sxs-lookup"><span data-stu-id="1dc3d-335">This change allows to use multiple contexts in the same `TransactionScope`.</span></span> <span data-ttu-id="1dc3d-336">Новое поведение также соответствует EF6.</span><span class="sxs-lookup"><span data-stu-id="1dc3d-336">The new behavior also matches EF6.</span></span>

<span data-ttu-id="1dc3d-337">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="1dc3d-337">**Mitigations**</span></span>

<span data-ttu-id="1dc3d-338">Если соединение должно оставаться открытым, явный вызов метода `OpenConnection()` гарантирует, что EF Core не закроет его преждевременно.</span><span class="sxs-lookup"><span data-stu-id="1dc3d-338">If the connection needs to remain open explicit call to `OpenConnection()` will ensure that EF Core doesn't close it prematurely:</span></span>

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

## <a name="each-property-uses-independent-in-memory-integer-key-generation"></a><span data-ttu-id="1dc3d-339">Каждое свойство использует создание независимых целочисленных ключей в памяти</span><span class="sxs-lookup"><span data-stu-id="1dc3d-339">Each property uses independent in-memory integer key generation</span></span>

[<span data-ttu-id="1dc3d-340">Отслеживание вопроса 6872</span><span class="sxs-lookup"><span data-stu-id="1dc3d-340">Tracking Issue #6872</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/6872)

<span data-ttu-id="1dc3d-341">Это изменение будет внесено в предварительную версию 4 EF Core 3.0.</span><span class="sxs-lookup"><span data-stu-id="1dc3d-341">This change will be introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="1dc3d-342">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="1dc3d-342">**Old behavior**</span></span>

<span data-ttu-id="1dc3d-343">До выхода EF Core 3.0 для всех свойств целочисленных ключей в памяти использовался один общий генератор значений.</span><span class="sxs-lookup"><span data-stu-id="1dc3d-343">Before EF Core 3.0, one shared value generator was used for all in-memory integer key properties.</span></span>

<span data-ttu-id="1dc3d-344">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="1dc3d-344">**New behavior**</span></span>

<span data-ttu-id="1dc3d-345">Начиная с EF Core 3.0 при использовании выполняющейся в памяти базы данных каждое свойство целочисленного ключа получает свой собственный генератор значений.</span><span class="sxs-lookup"><span data-stu-id="1dc3d-345">Starting with EF Core 3.0, each integer key property gets its own value generator when using the in-memory database.</span></span>
<span data-ttu-id="1dc3d-346">Кроме того, при удалении базы данных формирование ключа сбрасывается для всех таблиц.</span><span class="sxs-lookup"><span data-stu-id="1dc3d-346">Also, if the database is deleted, then key generation is reset for all tables.</span></span>

<span data-ttu-id="1dc3d-347">**Причина**</span><span class="sxs-lookup"><span data-stu-id="1dc3d-347">**Why**</span></span>

<span data-ttu-id="1dc3d-348">Это изменение было внесено, чтобы лучше согласовать формирование ключей в памяти с реальным процессом в базе данных, а также улучшить возможность изоляции тестов друг от друга при использовании выполняющейся в памяти базы данных.</span><span class="sxs-lookup"><span data-stu-id="1dc3d-348">This change was made to align in-memory key generation more closely to real database key generation and to improve the ability to isolate tests from each other when using the in-memory database.</span></span>

<span data-ttu-id="1dc3d-349">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="1dc3d-349">**Mitigations**</span></span>

<span data-ttu-id="1dc3d-350">Это изменение может нарушить работу приложения, полагающегося на задание определенных значений ключа в памяти.</span><span class="sxs-lookup"><span data-stu-id="1dc3d-350">This can break an application that is relying on specific in-memory key values to be set.</span></span>
<span data-ttu-id="1dc3d-351">Рекомендуется не полагаться на конкретные значения ключей либо реализовать новое поведение.</span><span class="sxs-lookup"><span data-stu-id="1dc3d-351">Consider instead not relying on specific key values, or updating to match the new behavior.</span></span>

## <a name="backing-fields-are-used-by-default"></a><span data-ttu-id="1dc3d-352">По умолчанию используются резервные поля</span><span class="sxs-lookup"><span data-stu-id="1dc3d-352">Backing fields are used by default</span></span>

[<span data-ttu-id="1dc3d-353">Отслеживание вопроса 12430</span><span class="sxs-lookup"><span data-stu-id="1dc3d-353">Tracking Issue #12430</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12430)

<span data-ttu-id="1dc3d-354">Это изменение было внесено в предварительную версию 2 EF Core 3.0.</span><span class="sxs-lookup"><span data-stu-id="1dc3d-354">This change was introduced in EF Core 3.0-preview 2.</span></span>

<span data-ttu-id="1dc3d-355">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="1dc3d-355">**Old behavior**</span></span>

<span data-ttu-id="1dc3d-356">До выхода версии 3.0, даже если резервное поле для свойства было известно, система EF Core все равно по умолчанию считывала и записывала значение свойства с помощью методов получения и задания свойства.</span><span class="sxs-lookup"><span data-stu-id="1dc3d-356">Before 3.0, even if the backing field for a property was known, EF Core would still by default read and write the property value using the property getter and setter methods.</span></span>
<span data-ttu-id="1dc3d-357">Исключением из этого было выполнение запроса, где известное резервное поле устанавливается напрямую.</span><span class="sxs-lookup"><span data-stu-id="1dc3d-357">The exception to this was query execution, where the backing field would be set directly if known.</span></span>

<span data-ttu-id="1dc3d-358">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="1dc3d-358">**New behavior**</span></span>

<span data-ttu-id="1dc3d-359">Начиная с EF Core 3.0, если резервное поле для свойства известно, то EF Core всегда будет использовать его для чтения и записи этого свойства.</span><span class="sxs-lookup"><span data-stu-id="1dc3d-359">Starting with EF Core 3.0, if the backing field for a property is known, then EF Core will always read and write that property using the backing field.</span></span>
<span data-ttu-id="1dc3d-360">Это может нарушить работу приложения, если оно полагается на дополнительное поведение, закодированное в методы получения и задания.</span><span class="sxs-lookup"><span data-stu-id="1dc3d-360">This could cause an application break if the application is relying on additional behavior coded into the getter or setter methods.</span></span>

<span data-ttu-id="1dc3d-361">**Причина**</span><span class="sxs-lookup"><span data-stu-id="1dc3d-361">**Why**</span></span>

<span data-ttu-id="1dc3d-362">Это изменение было внесено, чтобы не позволить EF Core ошибочно активировать бизнес-логику по умолчанию при выполнении операций базы данных, затрагивающих сущности.</span><span class="sxs-lookup"><span data-stu-id="1dc3d-362">This change was made to prevent EF Core from erroneously triggering business logic by default when performing database operations involving the entities.</span></span>

<span data-ttu-id="1dc3d-363">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="1dc3d-363">**Mitigations**</span></span>

<span data-ttu-id="1dc3d-364">Поведение, предшествовавшее выходу версии 3.0, можно восстановить, настроив режим доступа к свойству в текучем API modelBuilder.</span><span class="sxs-lookup"><span data-stu-id="1dc3d-364">The pre-3.0 behavior can be restored through configuration of the property access mode in the modelBuilder fluent API.</span></span>
<span data-ttu-id="1dc3d-365">Например:</span><span class="sxs-lookup"><span data-stu-id="1dc3d-365">For example:</span></span>

```C#
modelBuilder.UsePropertyAccessMode(PropertyAccessMode.PreferFieldDuringConstruction);
```

## <a name="throw-if-multiple-compatible-backing-fields-are-found"></a><span data-ttu-id="1dc3d-366">Исключение при обнаружении нескольких совместимых резервных полей</span><span class="sxs-lookup"><span data-stu-id="1dc3d-366">Throw if multiple compatible backing fields are found</span></span>

[<span data-ttu-id="1dc3d-367">Отслеживание вопроса 12523</span><span class="sxs-lookup"><span data-stu-id="1dc3d-367">Tracking Issue #12523</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12523)

<span data-ttu-id="1dc3d-368">Это изменение будет внесено в предварительную версию 4 EF Core 3.0.</span><span class="sxs-lookup"><span data-stu-id="1dc3d-368">This change will be introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="1dc3d-369">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="1dc3d-369">**Old behavior**</span></span>

<span data-ttu-id="1dc3d-370">До выхода EF Core 3.0, если правила, обеспечивающие поиск резервного поля для свойства, находили несколько полей, выбиралось одно из них на основе определенной очередности.</span><span class="sxs-lookup"><span data-stu-id="1dc3d-370">Before EF Core 3.0, if multiple fields matched the rules for finding the backing field of a property, then one field would be chosen based on some precedence order.</span></span>
<span data-ttu-id="1dc3d-371">В неоднозначных случаях это могло привести к использованию неправильного поля.</span><span class="sxs-lookup"><span data-stu-id="1dc3d-371">This could cause the wrong field to be used in ambiguous cases.</span></span>

<span data-ttu-id="1dc3d-372">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="1dc3d-372">**New behavior**</span></span>

<span data-ttu-id="1dc3d-373">Начиная с EF Core 3.0 при обнаружении нескольких полей для одного свойства возникает исключение.</span><span class="sxs-lookup"><span data-stu-id="1dc3d-373">Starting with EF Core 3.0, if multiple fields are matched to the same property, then an exception is thrown.</span></span>

<span data-ttu-id="1dc3d-374">**Причина**</span><span class="sxs-lookup"><span data-stu-id="1dc3d-374">**Why**</span></span>

<span data-ttu-id="1dc3d-375">Это изменение было внесено во избежание автоматического использования одного поля вместо другого, когда правильным может быть только одно из них.</span><span class="sxs-lookup"><span data-stu-id="1dc3d-375">This change was made to avoid silently using one field over another when only one can be correct.</span></span>

<span data-ttu-id="1dc3d-376">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="1dc3d-376">**Mitigations**</span></span>

<span data-ttu-id="1dc3d-377">Для свойств с неоднозначными резервными полями используемое поле должно быть задано явным образом.</span><span class="sxs-lookup"><span data-stu-id="1dc3d-377">Properties with ambiguous backing fields must have the field to use specified explicitly.</span></span>
<span data-ttu-id="1dc3d-378">Например, с помощью текучего API:</span><span class="sxs-lookup"><span data-stu-id="1dc3d-378">For example, using the fluent API:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .HasField("_id");
```

## <a name="field-only-property-names-should-match-the-field-name"></a><span data-ttu-id="1dc3d-379">Имена свойств, доступных только для полей, должны совпадать с именем поля.</span><span class="sxs-lookup"><span data-stu-id="1dc3d-379">Field-only property names should match the field name</span></span>

<span data-ttu-id="1dc3d-380">Это изменение будет внесено в предварительную версию 4 EF Core 3.0.</span><span class="sxs-lookup"><span data-stu-id="1dc3d-380">This change will be introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="1dc3d-381">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="1dc3d-381">**Old behavior**</span></span>

<span data-ttu-id="1dc3d-382">До EF Core 3.0 свойство могло быть указано с помощью строкового значения, и если свойство с таким именем не было найдено в типе CLR, компонент EF Core пытался сопоставить его с полем, используя правила соглашения.</span><span class="sxs-lookup"><span data-stu-id="1dc3d-382">Before EF Core 3.0, a property could be specified by a string value and if no property with that name was found on the CLR type then EF Core would try to match it to a field using convetion rules.</span></span>
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

<span data-ttu-id="1dc3d-383">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="1dc3d-383">**New behavior**</span></span>

<span data-ttu-id="1dc3d-384">Начиная с EF Core 3.0 имя свойства, доступного только для полей, должно точно соответствовать имени поля.</span><span class="sxs-lookup"><span data-stu-id="1dc3d-384">Starting with EF Core 3.0, a field-only property must match the field name exactly.</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property("_id");
```

<span data-ttu-id="1dc3d-385">**Причина**</span><span class="sxs-lookup"><span data-stu-id="1dc3d-385">**Why**</span></span>

<span data-ttu-id="1dc3d-386">Это изменение было внесено, чтобы избежать использования одного поля для двух свойств с похожими именами, кроме того, правила сопоставления для свойств, доступных только для полей, теперь соответствуют правилам для свойств, сопоставленных со свойствами CLR.</span><span class="sxs-lookup"><span data-stu-id="1dc3d-386">This change was made to avoid using the same field for two properties named similarly, it also makes the matching rules for field-only properties the same as for properties mapped to CLR properties.</span></span>

<span data-ttu-id="1dc3d-387">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="1dc3d-387">**Mitigations**</span></span>

<span data-ttu-id="1dc3d-388">Имена свойств, доступных только для полей, должны совпадать с именем поля, с которым они сопоставляются.</span><span class="sxs-lookup"><span data-stu-id="1dc3d-388">Field-only properties must be named the same as the field they are mapped to.</span></span>
<span data-ttu-id="1dc3d-389">В последующей предварительной версии EF Core 3.0 мы планируем снова включить явную настройку имени поля, которое отличается от имени свойства:</span><span class="sxs-lookup"><span data-stu-id="1dc3d-389">In a later preview of EF Core 3.0, we plan to re-enable explicitly configuring a field name that is different from the property name:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property("Id")
    .HasField("_id");
```

## <a name="adddbcontextadddbcontextpool-no-longer-call-addlogging-and-addmemorycache"></a><span data-ttu-id="1dc3d-390">AddDbContext/AddDbContextPool больше не вызывает метод AddLogging и AddMemoryCache</span><span class="sxs-lookup"><span data-stu-id="1dc3d-390">AddDbContext/AddDbContextPool no longer call AddLogging and AddMemoryCache</span></span>

[<span data-ttu-id="1dc3d-391">Отслеживание вопроса № 14756</span><span class="sxs-lookup"><span data-stu-id="1dc3d-391">Tracking Issue #14756</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14756)

<span data-ttu-id="1dc3d-392">Это изменение будет внесено в предварительную версию 4 EF Core 3.0.</span><span class="sxs-lookup"><span data-stu-id="1dc3d-392">This change will be introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="1dc3d-393">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="1dc3d-393">**Old behavior**</span></span>

<span data-ttu-id="1dc3d-394">До EF Core 3.0 вызов метода `AddDbContext` или `AddDbContextPool` также приводил к регистрации служб ведения журналов и кэширования памяти с внедрением зависимостей с помощью вызовов к методам [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) и [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span><span class="sxs-lookup"><span data-stu-id="1dc3d-394">Before EF Core 3.0, calling `AddDbContext` or `AddDbContextPool` would also register logging and memory caching services with D.I through calls to [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) and [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span></span>

<span data-ttu-id="1dc3d-395">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="1dc3d-395">**New behavior**</span></span>

<span data-ttu-id="1dc3d-396">Начиная с версии EF Core 3.0 методы `AddDbContext` и `AddDbContextPool` больше не регистрируют такие службы с внедрением зависимостей.</span><span class="sxs-lookup"><span data-stu-id="1dc3d-396">Starting with EF Core 3.0, `AddDbContext` and `AddDbContextPool` will no longer register these services with Dependency Injection (DI).</span></span>

<span data-ttu-id="1dc3d-397">**Причина**</span><span class="sxs-lookup"><span data-stu-id="1dc3d-397">**Why**</span></span>

<span data-ttu-id="1dc3d-398">EF Core 3.0 не требует, чтобы такие службы присутствовали в контейнере внедрения зависимостей приложения.</span><span class="sxs-lookup"><span data-stu-id="1dc3d-398">EF Core 3.0 does not require that these services are in the application's DI cotainer.</span></span> <span data-ttu-id="1dc3d-399">Но если интерфейс `ILoggerFactory` зарегистрирован в контейнере внедрения зависимостей приложения, он будет по-прежнему использоваться EF Core.</span><span class="sxs-lookup"><span data-stu-id="1dc3d-399">However, if `ILoggerFactory` is registered in the application's DI container, then it will still be used by EF Core.</span></span>

<span data-ttu-id="1dc3d-400">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="1dc3d-400">**Mitigations**</span></span>

<span data-ttu-id="1dc3d-401">Если для работы вашего приложения нужны такие службы, явно зарегистрируйте их в контейнере внедрения зависимостей с помощью метода [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) или [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span><span class="sxs-lookup"><span data-stu-id="1dc3d-401">If your application needs these services, then register them explicitly with the DI container using  [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) or [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span></span>

## <a name="dbcontextentry-now-performs-a-local-detectchanges"></a><span data-ttu-id="1dc3d-402">DbContext.Entry теперь выполняет локальную процедуру DetectChanges</span><span class="sxs-lookup"><span data-stu-id="1dc3d-402">DbContext.Entry now performs a local DetectChanges</span></span>

[<span data-ttu-id="1dc3d-403">Отслеживание вопроса 13552</span><span class="sxs-lookup"><span data-stu-id="1dc3d-403">Tracking Issue #13552</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13552)

<span data-ttu-id="1dc3d-404">Это изменение было внесено в предварительную версию 3 EF Core 3.0.</span><span class="sxs-lookup"><span data-stu-id="1dc3d-404">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="1dc3d-405">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="1dc3d-405">**Old behavior**</span></span>

<span data-ttu-id="1dc3d-406">До выхода EF Core 3.0 вызов `DbContext.Entry` приводил к обнаружению изменений для всех отслеживаемых сущностей.</span><span class="sxs-lookup"><span data-stu-id="1dc3d-406">Before EF Core 3.0, calling `DbContext.Entry` would cause changes to be detected for all tracked entities.</span></span>
<span data-ttu-id="1dc3d-407">Это гарантировало, что состояние, представленное в `EntityEntry`, было актуальным.</span><span class="sxs-lookup"><span data-stu-id="1dc3d-407">This ensured that the state exposed in the `EntityEntry` was up-to-date.</span></span>

<span data-ttu-id="1dc3d-408">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="1dc3d-408">**New behavior**</span></span>

<span data-ttu-id="1dc3d-409">Начиная с EF Core 3.0 при вызове `DbContext.Entry` будет предприниматься попытка обнаружить изменения только в заданной сущности и всех связанных с ней отслеживаемых сущностях субъектов.</span><span class="sxs-lookup"><span data-stu-id="1dc3d-409">Starting with EF Core 3.0, calling `DbContext.Entry` will now only attempt to detect changes in the given entity and any tracked principal entities related to it.</span></span>
<span data-ttu-id="1dc3d-410">Это означает, что изменения в другом месте могут не обнаруживаться при вызове этого метода, что может негативно повлиять на состояние приложения.</span><span class="sxs-lookup"><span data-stu-id="1dc3d-410">This means that changes elsewhere may not have been detected by calling this method, which could have implications on application state.</span></span>

<span data-ttu-id="1dc3d-411">Обратите внимание, что если для `ChangeTracker.AutoDetectChangesEnabled` задано значение `false`, то будет отключено даже такое локальное обнаружение изменений.</span><span class="sxs-lookup"><span data-stu-id="1dc3d-411">Note that if `ChangeTracker.AutoDetectChangesEnabled` is set to `false` then even this local change detection will be disabled.</span></span>

<span data-ttu-id="1dc3d-412">Другие методы, вызывающие обнаружение изменений, например `ChangeTracker.Entries` и `SaveChanges`, по-прежнему подразумевают полную процедуру `DetectChanges` для всех отслеживаемых сущностей.</span><span class="sxs-lookup"><span data-stu-id="1dc3d-412">Other methods that cause change detection--for example `ChangeTracker.Entries` and `SaveChanges`--still cause a full `DetectChanges` of all tracked entities.</span></span>

<span data-ttu-id="1dc3d-413">**Причина**</span><span class="sxs-lookup"><span data-stu-id="1dc3d-413">**Why**</span></span>

<span data-ttu-id="1dc3d-414">Это изменение было внесено для повышения производительности по умолчанию при использовании `context.Entry`.</span><span class="sxs-lookup"><span data-stu-id="1dc3d-414">This change was made to improve the default performance of using `context.Entry`.</span></span>

<span data-ttu-id="1dc3d-415">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="1dc3d-415">**Mitigations**</span></span>

<span data-ttu-id="1dc3d-416">Вызовите `ChgangeTracker.DetectChanges()` явным образом перед вызовом `Entry`, чтобы обеспечить поведение, предшествовавшее выходу версии 3.0.</span><span class="sxs-lookup"><span data-stu-id="1dc3d-416">Call `ChgangeTracker.DetectChanges()` explicitly before calling `Entry` to ensure the pre-3.0 behavior.</span></span>

## <a name="string-and-byte-array-keys-are-not-client-generated-by-default"></a><span data-ttu-id="1dc3d-417">Ключи массива строк и байтов не формируются клиентом по умолчанию</span><span class="sxs-lookup"><span data-stu-id="1dc3d-417">String and byte array keys are not client-generated by default</span></span>

[<span data-ttu-id="1dc3d-418">Отслеживание вопроса 14617</span><span class="sxs-lookup"><span data-stu-id="1dc3d-418">Tracking Issue #14617</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14617)

<span data-ttu-id="1dc3d-419">Это изменение будет внесено в предварительную версию 4 EF Core 3.0.</span><span class="sxs-lookup"><span data-stu-id="1dc3d-419">This change will be introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="1dc3d-420">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="1dc3d-420">**Old behavior**</span></span>

<span data-ttu-id="1dc3d-421">До выхода EF Core 3.0 свойства ключей `string` и `byte[]` можно было использовать без явного указания значения, отличного от NULL.</span><span class="sxs-lookup"><span data-stu-id="1dc3d-421">Before EF Core 3.0, `string` and `byte[]` key properties could be used without explicitly setting a non-null value.</span></span>
<span data-ttu-id="1dc3d-422">В этом случае значение ключа создается на клиенте в виде GUID, сериализованного до байтов для `byte[]`.</span><span class="sxs-lookup"><span data-stu-id="1dc3d-422">In such a case, the key value would be generated on the client as a GUID, serialized to bytes for `byte[]`.</span></span>

<span data-ttu-id="1dc3d-423">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="1dc3d-423">**New behavior**</span></span>

<span data-ttu-id="1dc3d-424">Начиная с EF Core 3.0 возникает исключение, указывающее на то, что значение ключа не задано.</span><span class="sxs-lookup"><span data-stu-id="1dc3d-424">Starting with EF Core 3.0 an exception will be thrown indicating that no key value has been set.</span></span>

<span data-ttu-id="1dc3d-425">**Причина**</span><span class="sxs-lookup"><span data-stu-id="1dc3d-425">**Why**</span></span>

<span data-ttu-id="1dc3d-426">Это изменение было внесено, так как формируемые клиентом значения `string`/`byte[]` обычно бесполезны, а поведение по умолчанию затрудняло типовое планирование использования этих значений ключей.</span><span class="sxs-lookup"><span data-stu-id="1dc3d-426">This change was made because client-generated `string`/`byte[]` values generally aren't useful, and the default behavior made it hard to reason about generated key values in a common way.</span></span>

<span data-ttu-id="1dc3d-427">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="1dc3d-427">**Mitigations**</span></span>

<span data-ttu-id="1dc3d-428">Поведение, предшествовавшее выходу версии 3.0, можно получить, явно указав, что свойства ключей должны использовать формируемые значения, если не задано никакое другое значение, отличное от NULL.</span><span class="sxs-lookup"><span data-stu-id="1dc3d-428">The pre-3.0 behavior can be obtained by explicitly specifying that the key properties should use generated values if no other non-null value is set.</span></span>
<span data-ttu-id="1dc3d-429">Например, с помощью текучего API:</span><span class="sxs-lookup"><span data-stu-id="1dc3d-429">For example, with the fluent API:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .ValueGeneratedOnAdd();
```

<span data-ttu-id="1dc3d-430">Или с помощью заметок к данным:</span><span class="sxs-lookup"><span data-stu-id="1dc3d-430">Or with data annotations:</span></span>

```C#
[DatabaseGenerated(DatabaseGeneratedOption.Identity)]
public string Id { get; set; }
```

## <a name="iloggerfactory-is-now-a-scoped-service"></a><span data-ttu-id="1dc3d-431">ILoggerFactory теперь является службой с ограниченной областью действия</span><span class="sxs-lookup"><span data-stu-id="1dc3d-431">ILoggerFactory is now a scoped service</span></span>

[<span data-ttu-id="1dc3d-432">Отслеживание вопроса 14698</span><span class="sxs-lookup"><span data-stu-id="1dc3d-432">Tracking Issue #14698</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14698)

<span data-ttu-id="1dc3d-433">Это изменение было внесено в предварительную версию 3 EF Core 3.0.</span><span class="sxs-lookup"><span data-stu-id="1dc3d-433">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="1dc3d-434">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="1dc3d-434">**Old behavior**</span></span>

<span data-ttu-id="1dc3d-435">До выхода EF Core 3.0 `ILoggerFactory` регистрировалась в качестве отдельной службы.</span><span class="sxs-lookup"><span data-stu-id="1dc3d-435">Before EF Core 3.0, `ILoggerFactory` was registered as a singleton service.</span></span>

<span data-ttu-id="1dc3d-436">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="1dc3d-436">**New behavior**</span></span>

<span data-ttu-id="1dc3d-437">Начиная с EF Core 3.0 `ILoggerFactory` регистрируется в качестве службы с ограниченной областью действия.</span><span class="sxs-lookup"><span data-stu-id="1dc3d-437">Starting with EF Core 3.0, `ILoggerFactory` is now registered as scoped.</span></span>

<span data-ttu-id="1dc3d-438">**Причина**</span><span class="sxs-lookup"><span data-stu-id="1dc3d-438">**Why**</span></span>

<span data-ttu-id="1dc3d-439">Это изменение было внесено, чтобы разрешить сопоставление средства ведения журнала с экземпляром `DbContext`, что обеспечивает другие функции и устраняет некоторые патологические варианты поведения, такие как взрывной рост внутренних поставщиков служб.</span><span class="sxs-lookup"><span data-stu-id="1dc3d-439">This change was made to allow association of a logger with a `DbContext` instance, which enables other functionality and removes some cases of pathological behavior such as an explosion of internal service providers.</span></span>

<span data-ttu-id="1dc3d-440">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="1dc3d-440">**Mitigations**</span></span>

<span data-ttu-id="1dc3d-441">Это изменение не должно затрагивать код приложения, если только он не регистрирует и использует настраиваемые службы во внутреннем поставщике служб EF Core.</span><span class="sxs-lookup"><span data-stu-id="1dc3d-441">This change should not impact application code unless it is registering and using custom services on the EF Core internal service provider.</span></span>
<span data-ttu-id="1dc3d-442">Это встречается довольно редко.</span><span class="sxs-lookup"><span data-stu-id="1dc3d-442">This isn't common.</span></span>
<span data-ttu-id="1dc3d-443">В таких случаях большинство возможностей продолжают работать, но потребуется изменить все отдельные службы, которые зависели от `ILoggerFactory`, чтобы получить `ILoggerFactory` другим способом.</span><span class="sxs-lookup"><span data-stu-id="1dc3d-443">In these cases, most things will still work, but any singleton service that was depending on `ILoggerFactory` will need to be changed to obtain the `ILoggerFactory` in a different way.</span></span>

<span data-ttu-id="1dc3d-444">При возникновении подобных ситуаций зарегистрируйте проблему в [средстве отслеживания вопросов EF Core на сайте GitHub](https://github.com/aspnet/EntityFrameworkCore/issues) и сообщите нам, как вы используете `ILoggerFactory`, чтобы мы могли лучше понять, как избежать подобных ошибок в будущем.</span><span class="sxs-lookup"><span data-stu-id="1dc3d-444">If you run into situations like this, please file an issue at on the [EF Core GitHub issue tracker](https://github.com/aspnet/EntityFrameworkCore/issues) to let us know how you are using `ILoggerFactory` such that we can better understand how not to break this again in the future.</span></span>

## <a name="idbcontextoptionsextensionwithdebuginfo-merged-into-idbcontextoptionsextension"></a><span data-ttu-id="1dc3d-445">IDbContextOptionsExtensionWithDebugInfo объединен с IDbContextOptionsExtension</span><span class="sxs-lookup"><span data-stu-id="1dc3d-445">IDbContextOptionsExtensionWithDebugInfo merged into IDbContextOptionsExtension</span></span>

[<span data-ttu-id="1dc3d-446">Отслеживание вопроса 13552</span><span class="sxs-lookup"><span data-stu-id="1dc3d-446">Tracking Issue #13552</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13552)

<span data-ttu-id="1dc3d-447">Это изменение было внесено в предварительную версию 3 EF Core 3.0.</span><span class="sxs-lookup"><span data-stu-id="1dc3d-447">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="1dc3d-448">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="1dc3d-448">**Old behavior**</span></span>

<span data-ttu-id="1dc3d-449">`IDbContextOptionsExtensionWithDebugInfo` был дополнительным необязательным интерфейсом, расширяемым из `IDbContextOptionsExtension`, для предотвращения критического изменения в интерфейсе в рамках выпуска версии 2.x.</span><span class="sxs-lookup"><span data-stu-id="1dc3d-449">`IDbContextOptionsExtensionWithDebugInfo` was an additional optional interface extended from `IDbContextOptionsExtension` to avoid making a breaking change to the interface during the 2.x release cycle.</span></span>

<span data-ttu-id="1dc3d-450">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="1dc3d-450">**New behavior**</span></span>

<span data-ttu-id="1dc3d-451">Теперь эти интерфейсы объединяются в `IDbContextOptionsExtension`.</span><span class="sxs-lookup"><span data-stu-id="1dc3d-451">The interfaces are now merged together into `IDbContextOptionsExtension`.</span></span>

<span data-ttu-id="1dc3d-452">**Причина**</span><span class="sxs-lookup"><span data-stu-id="1dc3d-452">**Why**</span></span>

<span data-ttu-id="1dc3d-453">Это изменение было внесено, так как интерфейсы представляют собой единое целое.</span><span class="sxs-lookup"><span data-stu-id="1dc3d-453">This change was made because the interfaces are conceptually one.</span></span>

<span data-ttu-id="1dc3d-454">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="1dc3d-454">**Mitigations**</span></span>

<span data-ttu-id="1dc3d-455">Нужно изменить все реализации `IDbContextOptionsExtension`, чтобы обеспечить поддержку нового члена.</span><span class="sxs-lookup"><span data-stu-id="1dc3d-455">Any implementations of `IDbContextOptionsExtension` will need to be updated to support the new member.</span></span>

## <a name="lazy-loading-proxies-no-longer-assume-navigation-properties-are-fully-loaded"></a><span data-ttu-id="1dc3d-456">Прокси с отложенной загрузкой больше не предполагают полную загрузку свойств навигации</span><span class="sxs-lookup"><span data-stu-id="1dc3d-456">Lazy-loading proxies no longer assume navigation properties are fully loaded</span></span>

[<span data-ttu-id="1dc3d-457">Отслеживание вопроса 12780</span><span class="sxs-lookup"><span data-stu-id="1dc3d-457">Tracking Issue #12780</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12780)

<span data-ttu-id="1dc3d-458">Это изменение будет внесено в предварительную версию 4 EF Core 3.0.</span><span class="sxs-lookup"><span data-stu-id="1dc3d-458">This change will be introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="1dc3d-459">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="1dc3d-459">**Old behavior**</span></span>

<span data-ttu-id="1dc3d-460">До выхода EF Core 3.0 после удаления `DbContext` не существовало способа узнать, было ли указанное свойство навигации для сущности, полученной из этого контекста, загружено полностью.</span><span class="sxs-lookup"><span data-stu-id="1dc3d-460">Before EF Core 3.0, once a `DbContext` was disposed there was no way of knowing if a given navigation property on an entity obtained from that context was fully loaded or not.</span></span>
<span data-ttu-id="1dc3d-461">Вместо это прокси предполагали, что навигация по ссылке загружена, если она имеет отличное NULL значение, а навигация по коллекции загружена, если она не является пустой.</span><span class="sxs-lookup"><span data-stu-id="1dc3d-461">Proxies would instead assume that a reference navigation is loaded if it has a non-null value, and that a collection navigation is loaded if it isn't empty.</span></span>
<span data-ttu-id="1dc3d-462">В этом случае попытка отложенной загрузки приводила к холостой операции.</span><span class="sxs-lookup"><span data-stu-id="1dc3d-462">In these cases, attempting to lazy-load would be a no-op.</span></span>

<span data-ttu-id="1dc3d-463">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="1dc3d-463">**New behavior**</span></span>

<span data-ttu-id="1dc3d-464">Начиная с EF Core 3.0 прокси отслеживают, загружено ли свойство навигации.</span><span class="sxs-lookup"><span data-stu-id="1dc3d-464">Starting with EF Core 3.0, proxies keep track of whether or not a navigation property is loaded.</span></span>
<span data-ttu-id="1dc3d-465">Это означает, что попытка обращения к свойству навигации, загружаемому после удаления контекста, всегда будет давать холостую операцию, даже если загруженное свойство навигации пусто или равно NULL.</span><span class="sxs-lookup"><span data-stu-id="1dc3d-465">This means attempting to access a navigation property that is loaded after the context has been disposed will always be a no-op, even when the loaded navigation is empty or null.</span></span>
<span data-ttu-id="1dc3d-466">И наоборот, при попытке обращения к незагруженному свойству навигации, когда контекст удален, возникает исключение, даже если это свойство навигации является непустой коллекцией.</span><span class="sxs-lookup"><span data-stu-id="1dc3d-466">Conversely, attempting to access a navigation property that isn't loaded will throw an exception if the context is disposed even if the navigation property is a non-empty collection.</span></span>
<span data-ttu-id="1dc3d-467">Подобная ситуация указывает на то, что код приложения пытается использовать отложенную загрузку в недопустимое время, и нужно запретить это, изменив приложение.</span><span class="sxs-lookup"><span data-stu-id="1dc3d-467">If this situation arises, it means the application code is attempting to use lazy-loading at an invalid time, and the application should be changed to not do this.</span></span>

<span data-ttu-id="1dc3d-468">**Причина**</span><span class="sxs-lookup"><span data-stu-id="1dc3d-468">**Why**</span></span>

<span data-ttu-id="1dc3d-469">Это изменение было внесено, чтобы обеспечить правильное и согласованное поведение при попытке отложенной загрузки для удаленного экземпляра `DbContext`.</span><span class="sxs-lookup"><span data-stu-id="1dc3d-469">This change was made to make the behavior consistent and correct when attempting to lazy-load on a disposed `DbContext` instance.</span></span>

<span data-ttu-id="1dc3d-470">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="1dc3d-470">**Mitigations**</span></span>

<span data-ttu-id="1dc3d-471">Обновите код приложения, чтобы не выполнять попытку отложенной загрузки с удаленным контекстом, либо настройте соответствующую холостую операцию, как описано в сообщении об исключении.</span><span class="sxs-lookup"><span data-stu-id="1dc3d-471">Update application code to not attempt lazy-loading with a disposed context, or configure this to be a no-op as described in the exception message.</span></span>

## <a name="excessive-creation-of-internal-service-providers-is-now-an-error-by-default"></a><span data-ttu-id="1dc3d-472">Чрезмерное создание внутренних поставщиков служб теперь является ошибкой по умолчанию</span><span class="sxs-lookup"><span data-stu-id="1dc3d-472">Excessive creation of internal service providers is now an error by default</span></span>

[<span data-ttu-id="1dc3d-473">Отслеживание вопроса 10236</span><span class="sxs-lookup"><span data-stu-id="1dc3d-473">Tracking Issue #10236</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10236)

<span data-ttu-id="1dc3d-474">Это изменение было внесено в предварительную версию 3 EF Core 3.0.</span><span class="sxs-lookup"><span data-stu-id="1dc3d-474">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="1dc3d-475">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="1dc3d-475">**Old behavior**</span></span>

<span data-ttu-id="1dc3d-476">До выхода EF Core 3.0 для приложения, создающего чрезмерное количество внутренних поставщиков служб, регистрировалось предупреждение.</span><span class="sxs-lookup"><span data-stu-id="1dc3d-476">Before EF Core 3.0, a warning would be logged for an application creating a pathological number of internal service providers.</span></span>

<span data-ttu-id="1dc3d-477">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="1dc3d-477">**New behavior**</span></span>

<span data-ttu-id="1dc3d-478">Начиная с EF Core 3.0 это предупреждение считается ошибкой и возникает исключение.</span><span class="sxs-lookup"><span data-stu-id="1dc3d-478">Starting with EF Core 3.0, this warning is now considered and error and an exception is thrown.</span></span> 

<span data-ttu-id="1dc3d-479">**Причина**</span><span class="sxs-lookup"><span data-stu-id="1dc3d-479">**Why**</span></span>

<span data-ttu-id="1dc3d-480">Это изменение было внесено для улучшения кода приложения посредством явного указания на подобный патологический случай.</span><span class="sxs-lookup"><span data-stu-id="1dc3d-480">This change was made to drive better application code through exposing this pathological case more explicitly.</span></span>

<span data-ttu-id="1dc3d-481">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="1dc3d-481">**Mitigations**</span></span>

<span data-ttu-id="1dc3d-482">При возникновении этой ошибки правильнее всего выявить первопричину и остановить создание такого большого числа внутренних поставщиков служб.</span><span class="sxs-lookup"><span data-stu-id="1dc3d-482">The most appropriate cause of action on encountering this error is to understand the root cause and stop creating so many internal service providers.</span></span>
<span data-ttu-id="1dc3d-483">Тем не менее эту ошибку можно преобразовать обратно в предупреждение (или игнорировать) с помощью конфигурации для `DbContextOptionsBuilder`.</span><span class="sxs-lookup"><span data-stu-id="1dc3d-483">However, the error can be converted back to a warning (or ignored) via configuration on the `DbContextOptionsBuilder`.</span></span>
<span data-ttu-id="1dc3d-484">Например:</span><span class="sxs-lookup"><span data-stu-id="1dc3d-484">For example:</span></span>

```C#
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
{
    optionsBuilder
        .ConfigureWarnings(w => w.Log(CoreEventId.ManyServiceProvidersCreatedWarning));
}
```

## <a name="new-behavior-for-hasonehasmany-called-with-a-single-string"></a><span data-ttu-id="1dc3d-485">Новое поведение для вызова HasOne/HasMany с одной строкой</span><span class="sxs-lookup"><span data-stu-id="1dc3d-485">New behavior for HasOne/HasMany called with a single string</span></span>

[<span data-ttu-id="1dc3d-486">Отслеживание вопроса № 9171</span><span class="sxs-lookup"><span data-stu-id="1dc3d-486">Tracking Issue #9171</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9171)

<span data-ttu-id="1dc3d-487">Это изменение будет внесено в предварительную версию 4 EF Core 3.0.</span><span class="sxs-lookup"><span data-stu-id="1dc3d-487">This change will be introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="1dc3d-488">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="1dc3d-488">**Old behavior**</span></span>

<span data-ttu-id="1dc3d-489">До версии EF Core 3.0 интерпретация кода с вызовом `HasOne` или `HasMany` с одной строкой была очень запутанной.</span><span class="sxs-lookup"><span data-stu-id="1dc3d-489">Before EF Core 3.0, code calling `HasOne` or `HasMany` with a single string was interpretted in a confusing way.</span></span>
<span data-ttu-id="1dc3d-490">Например:</span><span class="sxs-lookup"><span data-stu-id="1dc3d-490">For example:</span></span>
```C#
modelBuilder.Entity<Samurai>().HasOne("Entrance").WithOne();
```

<span data-ttu-id="1dc3d-491">Складывается впечатление, что код связывает `Samurai` с другим типом сущности с помощью свойства навигации `Entrance`, которое может быть закрытым.</span><span class="sxs-lookup"><span data-stu-id="1dc3d-491">The code looks like it is relating `Samurai` to some other entity type using the `Entrance` navigation property, which may be private.</span></span>

<span data-ttu-id="1dc3d-492">На самом деле этот код пытается создать отношение с некоторым типом сущности под именем `Entrance`, который не имеет свойства навигации.</span><span class="sxs-lookup"><span data-stu-id="1dc3d-492">In reality, this code attempts to create a relationship to some entity type called `Entrance` with no navigation property.</span></span>

<span data-ttu-id="1dc3d-493">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="1dc3d-493">**New behavior**</span></span>

<span data-ttu-id="1dc3d-494">Начиная с EF Core 3.0, указанный выше код выполняет те действия, которые должен был выполнять раньше.</span><span class="sxs-lookup"><span data-stu-id="1dc3d-494">Starting with EF Core 3.0, the code above now does what it looked like it should have been doing before.</span></span>

<span data-ttu-id="1dc3d-495">**Причина**</span><span class="sxs-lookup"><span data-stu-id="1dc3d-495">**Why**</span></span>

<span data-ttu-id="1dc3d-496">Старое поведение создавало большую путаницу, особенно при считывании кода конфигурации и при поиске ошибок.</span><span class="sxs-lookup"><span data-stu-id="1dc3d-496">The old behavior was very confusing, especially when reading the configuration code and looking for errors.</span></span>

<span data-ttu-id="1dc3d-497">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="1dc3d-497">**Mitigations**</span></span>

<span data-ttu-id="1dc3d-498">Это приведет к нарушению работы только тех приложений, которые явно настраивают отношения с помощью строк для имен типов и не указывают явно свойство навигации.</span><span class="sxs-lookup"><span data-stu-id="1dc3d-498">This will only break applications that are explicitly configuring relationships using strings for type names and without specifying the navigation property explicitly.</span></span>
<span data-ttu-id="1dc3d-499">Такой подход используется нечасто.</span><span class="sxs-lookup"><span data-stu-id="1dc3d-499">This is not common.</span></span>
<span data-ttu-id="1dc3d-500">Прежнее поведение можно реализовать с помощью явной передачи значения `null` для имени свойства навигации.</span><span class="sxs-lookup"><span data-stu-id="1dc3d-500">The previous behavior can be obtained through explicitly passing `null` for the navigation property name.</span></span>
<span data-ttu-id="1dc3d-501">Например:</span><span class="sxs-lookup"><span data-stu-id="1dc3d-501">For example:</span></span>

```C#
modelBuilder.Entity<Samurai>().HasOne("Some.Entity.Type.Name", null).WithOne();
```

## <a name="the-return-type-for-several-async-methods-has-been-changed-from-task-to-valuetask"></a><span data-ttu-id="1dc3d-502">Тип возвращаемого значения для нескольких асинхронных методов изменен с Task на ValueTask</span><span class="sxs-lookup"><span data-stu-id="1dc3d-502">The return type for several async methods has been changed from Task to ValueTask</span></span>

[<span data-ttu-id="1dc3d-503">Отслеживание вопроса № 15184</span><span class="sxs-lookup"><span data-stu-id="1dc3d-503">Tracking Issue #15184</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15184)

<span data-ttu-id="1dc3d-504">Это изменение будет внесено в предварительную версию 4 EF Core 3.0.</span><span class="sxs-lookup"><span data-stu-id="1dc3d-504">This change will be introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="1dc3d-505">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="1dc3d-505">**Old behavior**</span></span>

<span data-ttu-id="1dc3d-506">Следующие асинхронные методы ранее возвращали `Task<T>`:</span><span class="sxs-lookup"><span data-stu-id="1dc3d-506">The following async methods previously returned a `Task<T>`:</span></span>

* `DbContext.FindAsync()`
* `DbSet.FindAsync()`
* `DbContext.AddAsync()`
* `DbSet.AddAsync()`
* <span data-ttu-id="1dc3d-507">`ValueGenerator.NextValueAsync()` (и производные классы)</span><span class="sxs-lookup"><span data-stu-id="1dc3d-507">`ValueGenerator.NextValueAsync()` (and deriving classes)</span></span>

<span data-ttu-id="1dc3d-508">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="1dc3d-508">**New behavior**</span></span>

<span data-ttu-id="1dc3d-509">Упомянутые выше методы теперь возвращают `ValueTask<T>` над тем же `T`, что и раньше.</span><span class="sxs-lookup"><span data-stu-id="1dc3d-509">The aforementioned methods now return a `ValueTask<T>` over the same `T` as before.</span></span>

<span data-ttu-id="1dc3d-510">**Причина**</span><span class="sxs-lookup"><span data-stu-id="1dc3d-510">**Why**</span></span>

<span data-ttu-id="1dc3d-511">Это изменение уменьшает число выделений кучи, возникающих при вызове этих методов, что способствует повышению общей производительности.</span><span class="sxs-lookup"><span data-stu-id="1dc3d-511">This change reduces the number of heap allocations incurred when invoking these methods, improving general performance.</span></span>

<span data-ttu-id="1dc3d-512">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="1dc3d-512">**Mitigations**</span></span>

<span data-ttu-id="1dc3d-513">Приложения, просто ожидающие приведенные выше API, нужно просто перекомпилировать — изменять источник не требуется.</span><span class="sxs-lookup"><span data-stu-id="1dc3d-513">Applications simply awaiting the above APIs only need to be recompiled - no source changes are necessary.</span></span>
<span data-ttu-id="1dc3d-514">Для более сложного варианта использования (например, передачи возвращаемого класса `Task` методу `Task.WhenAny()`) обычно требуется, чтобы возвращаемый класс `ValueTask<T>` был преобразован в класс `Task<T>` путем вызова в нем метода `AsTask()`.</span><span class="sxs-lookup"><span data-stu-id="1dc3d-514">A more complex usage (e.g. passing the returned `Task` to `Task.WhenAny()`) typically require that the returned `ValueTask<T>` be converted to a `Task<T>` by calling `AsTask()` on it.</span></span>
<span data-ttu-id="1dc3d-515">Обратите внимание, что это сводит к нулю сокращение числа выделений, которое возможно в рамках этого изменения.</span><span class="sxs-lookup"><span data-stu-id="1dc3d-515">Note that this negates the allocation reduction that this change brings.</span></span>

## <a name="the-relationaltypemapping-annotation-is-now-just-typemapping"></a><span data-ttu-id="1dc3d-516">Заметка Relational:TypeMapping теперь является просто TypeMapping</span><span class="sxs-lookup"><span data-stu-id="1dc3d-516">The Relational:TypeMapping annotation is now just TypeMapping</span></span>

[<span data-ttu-id="1dc3d-517">Отслеживание вопроса 9913</span><span class="sxs-lookup"><span data-stu-id="1dc3d-517">Tracking Issue #9913</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9913)

<span data-ttu-id="1dc3d-518">Это изменение было внесено в предварительную версию 2 EF Core 3.0.</span><span class="sxs-lookup"><span data-stu-id="1dc3d-518">This change was introduced in EF Core 3.0-preview 2.</span></span>

<span data-ttu-id="1dc3d-519">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="1dc3d-519">**Old behavior**</span></span>

<span data-ttu-id="1dc3d-520">Для заметок сопоставлений типов использовалось имя "Relational:TypeMapping".</span><span class="sxs-lookup"><span data-stu-id="1dc3d-520">The annotation name for type mapping annotations was "Relational:TypeMapping".</span></span>

<span data-ttu-id="1dc3d-521">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="1dc3d-521">**New behavior**</span></span>

<span data-ttu-id="1dc3d-522">Теперь для заметок сопоставлений типов используется имя "TypeMapping".</span><span class="sxs-lookup"><span data-stu-id="1dc3d-522">The annotation name for type mapping annotations is now "TypeMapping".</span></span>

<span data-ttu-id="1dc3d-523">**Причина**</span><span class="sxs-lookup"><span data-stu-id="1dc3d-523">**Why**</span></span>

<span data-ttu-id="1dc3d-524">Сопоставления типов теперь используются не только для поставщиков реляционных баз данных.</span><span class="sxs-lookup"><span data-stu-id="1dc3d-524">Type mappings are now used for more than just relational database providers.</span></span>

<span data-ttu-id="1dc3d-525">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="1dc3d-525">**Mitigations**</span></span>

<span data-ttu-id="1dc3d-526">Это изменение нарушит работу только тех приложений, которые обращаются к сопоставлению типов непосредственно по заметке, что встречается довольно редко.</span><span class="sxs-lookup"><span data-stu-id="1dc3d-526">This will only break applications that access the type mapping directly as an annotation, which isn't common.</span></span>
<span data-ttu-id="1dc3d-527">Наиболее подходящий способ для устранения этой проблемы заключается в использовании поверхности API для доступа к сопоставлениям типов вместо прямого использования заметки.</span><span class="sxs-lookup"><span data-stu-id="1dc3d-527">The most appropriate action to fix is to use API surface to access type mappings rather than using the annotation directly.</span></span>

## <a name="totable-on-a-derived-type-throws-an-exception"></a><span data-ttu-id="1dc3d-528">ToTable для производного типа выдает исключение</span><span class="sxs-lookup"><span data-stu-id="1dc3d-528">ToTable on a derived type throws an exception</span></span> 

[<span data-ttu-id="1dc3d-529">Отслеживание вопроса 11811</span><span class="sxs-lookup"><span data-stu-id="1dc3d-529">Tracking Issue #11811</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/11811)

<span data-ttu-id="1dc3d-530">Это изменение было внесено в предварительную версию 3 EF Core 3.0.</span><span class="sxs-lookup"><span data-stu-id="1dc3d-530">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="1dc3d-531">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="1dc3d-531">**Old behavior**</span></span>

<span data-ttu-id="1dc3d-532">До выхода EF Core 3.0 вызов `ToTable()` для производного типа игнорировался, так как единственной стратегией сопоставления наследования был метод "одна таблица на иерархию", где такая операция недействительна.</span><span class="sxs-lookup"><span data-stu-id="1dc3d-532">Before EF Core 3.0, `ToTable()` called on a derived type would be ignored since only inheritance mapping strategy was TPH where this isn't valid.</span></span> 

<span data-ttu-id="1dc3d-533">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="1dc3d-533">**New behavior**</span></span>

<span data-ttu-id="1dc3d-534">Начиная с EF Core 3.0 и в рамках подготовки для добавления поддержки методов "одна таблица на тип" и "одна таблица на каждый отдельный тип/класс" в последующем выпуске вызов `ToTable()` для производного типа теперь будет выдавать исключение, чтобы избежать непредвиденного изменения сопоставления в будущем.</span><span class="sxs-lookup"><span data-stu-id="1dc3d-534">Starting with EF Core 3.0 and in preparation for adding TPT and TPC support in a later release, `ToTable()` called on a derived type will now throw an exception to avoid an unexpected mapping change in the future.</span></span>

<span data-ttu-id="1dc3d-535">**Причина**</span><span class="sxs-lookup"><span data-stu-id="1dc3d-535">**Why**</span></span>

<span data-ttu-id="1dc3d-536">Сейчас сопоставлять производный тип с другой таблицей запрещено.</span><span class="sxs-lookup"><span data-stu-id="1dc3d-536">Currently it isn't valid to map a derived type to a different table.</span></span>
<span data-ttu-id="1dc3d-537">Это изменение позволяет избежать нарушения работы в будущем, когда подобная операция будет разрешена.</span><span class="sxs-lookup"><span data-stu-id="1dc3d-537">This change avoids breaking in the future when it becomes a valid thing to do.</span></span>

<span data-ttu-id="1dc3d-538">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="1dc3d-538">**Mitigations**</span></span>

<span data-ttu-id="1dc3d-539">Удалите все попытки сопоставить производные типы с другими таблицами.</span><span class="sxs-lookup"><span data-stu-id="1dc3d-539">Remove any attempts to map derived types to other tables.</span></span>

## <a name="forsqlserverhasindex-replaced-with-hasindex"></a><span data-ttu-id="1dc3d-540">ForSqlServerHasIndex заменен на HasIndex</span><span class="sxs-lookup"><span data-stu-id="1dc3d-540">ForSqlServerHasIndex replaced with HasIndex</span></span> 

[<span data-ttu-id="1dc3d-541">Отслеживание вопроса 12366</span><span class="sxs-lookup"><span data-stu-id="1dc3d-541">Tracking Issue #12366</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12366)

<span data-ttu-id="1dc3d-542">Это изменение было внесено в предварительную версию 3 EF Core 3.0.</span><span class="sxs-lookup"><span data-stu-id="1dc3d-542">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="1dc3d-543">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="1dc3d-543">**Old behavior**</span></span>

<span data-ttu-id="1dc3d-544">До выхода EF Core 3.0 `ForSqlServerHasIndex().ForSqlServerInclude()` предоставлял способ настройки столбцов, используемых с `INCLUDE`.</span><span class="sxs-lookup"><span data-stu-id="1dc3d-544">Before EF Core 3.0, `ForSqlServerHasIndex().ForSqlServerInclude()` provided a way to configure columns used with `INCLUDE`.</span></span>

<span data-ttu-id="1dc3d-545">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="1dc3d-545">**New behavior**</span></span>

<span data-ttu-id="1dc3d-546">Начиная с EF Core 3.0 использование `Include` для индекса теперь поддерживается на реляционном уровне.</span><span class="sxs-lookup"><span data-stu-id="1dc3d-546">Starting with EF Core 3.0, using `Include` on an index is now supported at the relational level.</span></span>
<span data-ttu-id="1dc3d-547">Используйте ключевое слово `HasIndex().ForSqlServerInclude()`.</span><span class="sxs-lookup"><span data-stu-id="1dc3d-547">Use `HasIndex().ForSqlServerInclude()`.</span></span>

<span data-ttu-id="1dc3d-548">**Причина**</span><span class="sxs-lookup"><span data-stu-id="1dc3d-548">**Why**</span></span>

<span data-ttu-id="1dc3d-549">Это изменение было внесено, чтобы консолидировать API для индексов с `Include` в одном месте для всех поставщиков баз данных.</span><span class="sxs-lookup"><span data-stu-id="1dc3d-549">This change was made to consolidate the API for indexes with `Include` into one place for all database providers.</span></span>

<span data-ttu-id="1dc3d-550">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="1dc3d-550">**Mitigations**</span></span>

<span data-ttu-id="1dc3d-551">Используйте новый API, как показано выше.</span><span class="sxs-lookup"><span data-stu-id="1dc3d-551">Use the new API, as shown above.</span></span>

## <a name="metadata-api-changes"></a><span data-ttu-id="1dc3d-552">Изменения API метаданных</span><span class="sxs-lookup"><span data-stu-id="1dc3d-552">Metadata API changes</span></span>

[<span data-ttu-id="1dc3d-553">Отслеживание вопроса № 214</span><span class="sxs-lookup"><span data-stu-id="1dc3d-553">Tracking Issue #214</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/214)

<span data-ttu-id="1dc3d-554">Это изменение будет внесено в предварительную версию 4 EF Core 3.0.</span><span class="sxs-lookup"><span data-stu-id="1dc3d-554">This change will be introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="1dc3d-555">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="1dc3d-555">**New behavior**</span></span>

<span data-ttu-id="1dc3d-556">Следующие свойства были преобразованы в методы расширения:</span><span class="sxs-lookup"><span data-stu-id="1dc3d-556">The following properties were converted to extension methods:</span></span>

* `IEntityType.QueryFilter` -> `GetQueryFilter()`
* `IEntityType.DefiningQuery` -> `GetDefiningQuery()`
* `IProperty.IsShadowProperty` -> `IsShadowProperty()`
* `IProperty.BeforeSaveBehavior` -> `GetBeforeSaveBehavior()`
* `IProperty.AfterSaveBehavior` -> `GetAfterSaveBehavior()`

<span data-ttu-id="1dc3d-557">**Причина**</span><span class="sxs-lookup"><span data-stu-id="1dc3d-557">**Why**</span></span>

<span data-ttu-id="1dc3d-558">Это изменение упрощает реализацию упомянутых выше интерфейсов.</span><span class="sxs-lookup"><span data-stu-id="1dc3d-558">This change simplifies the implementation of the aforementioned interfaces.</span></span>

<span data-ttu-id="1dc3d-559">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="1dc3d-559">**Mitigations**</span></span>

<span data-ttu-id="1dc3d-560">Используйте новые методы расширения.</span><span class="sxs-lookup"><span data-stu-id="1dc3d-560">Use the new extension methods.</span></span>

## <a name="ef-core-no-longer-sends-pragma-for-sqlite-fk-enforcement"></a><span data-ttu-id="1dc3d-561">EF Core больше не отправляет pragma для принудительного применения FK SQLite</span><span class="sxs-lookup"><span data-stu-id="1dc3d-561">EF Core no longer sends pragma for SQLite FK enforcement</span></span>

[<span data-ttu-id="1dc3d-562">Отслеживание вопроса 12151</span><span class="sxs-lookup"><span data-stu-id="1dc3d-562">Tracking Issue #12151</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12151)

<span data-ttu-id="1dc3d-563">Это изменение было внесено в предварительную версию 3 EF Core 3.0.</span><span class="sxs-lookup"><span data-stu-id="1dc3d-563">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="1dc3d-564">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="1dc3d-564">**Old behavior**</span></span>

<span data-ttu-id="1dc3d-565">До выхода версии 3.0 система EF Core отправляла `PRAGMA foreign_keys = 1` при открытии соединения с SQLite.</span><span class="sxs-lookup"><span data-stu-id="1dc3d-565">Before EF Core 3.0, EF Core would send `PRAGMA foreign_keys = 1` when a connection to SQLite is opened.</span></span>

<span data-ttu-id="1dc3d-566">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="1dc3d-566">**New behavior**</span></span>

<span data-ttu-id="1dc3d-567">Начиная с версии 3.0 система EF Core больше не отправляет `PRAGMA foreign_keys = 1` при открытии соединения с SQLite.</span><span class="sxs-lookup"><span data-stu-id="1dc3d-567">Starting with EF Core 3.0, EF Core no longer sends `PRAGMA foreign_keys = 1` when a connection to SQLite is opened.</span></span>

<span data-ttu-id="1dc3d-568">**Причина**</span><span class="sxs-lookup"><span data-stu-id="1dc3d-568">**Why**</span></span>

<span data-ttu-id="1dc3d-569">Это изменение было внесено, так как EF Core по умолчанию использует `SQLitePCLRaw.bundle_e_sqlite3`, что, в свою очередь, означает, что принудительное применение FK по умолчанию включено и его не требуется включать каждый раз при открытии соединения.</span><span class="sxs-lookup"><span data-stu-id="1dc3d-569">This change was made because EF Core uses `SQLitePCLRaw.bundle_e_sqlite3` by default, which in turn means that FK enforcement is switched on by default and doesn't need to be explicitly enabled each time a connection is opened.</span></span>

<span data-ttu-id="1dc3d-570">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="1dc3d-570">**Mitigations**</span></span>

<span data-ttu-id="1dc3d-571">Внешние ключи включены по умолчанию в SQLitePCLRaw.bundle_e_sqlite3, который по умолчанию используется для EF Core.</span><span class="sxs-lookup"><span data-stu-id="1dc3d-571">Foreign keys are enabled by default in SQLitePCLRaw.bundle_e_sqlite3, which is used by default for EF Core.</span></span>
<span data-ttu-id="1dc3d-572">В других случаях внешние ключи можно включить, указав `Foreign Keys=True` в строке подключения.</span><span class="sxs-lookup"><span data-stu-id="1dc3d-572">For other cases, foreign keys can be enabled by specifying `Foreign Keys=True` in your connection string.</span></span>

## <a name="microsoftentityframeworkcoresqlite-now-depends-on-sqlitepclrawbundleesqlite3"></a><span data-ttu-id="1dc3d-573">Теперь Microsoft.EntityFrameworkCore.Sqlite зависит от SQLitePCLRaw.bundle_e_sqlite3</span><span class="sxs-lookup"><span data-stu-id="1dc3d-573">Microsoft.EntityFrameworkCore.Sqlite now depends on SQLitePCLRaw.bundle_e_sqlite3</span></span>

<span data-ttu-id="1dc3d-574">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="1dc3d-574">**Old behavior**</span></span>

<span data-ttu-id="1dc3d-575">До выхода версии 3.0 система EF Core использовала `SQLitePCLRaw.bundle_green`.</span><span class="sxs-lookup"><span data-stu-id="1dc3d-575">Before EF Core 3.0, EF Core used `SQLitePCLRaw.bundle_green`.</span></span>

<span data-ttu-id="1dc3d-576">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="1dc3d-576">**New behavior**</span></span>

<span data-ttu-id="1dc3d-577">Начиная с версии 3.0 система EF Core использует `SQLitePCLRaw.bundle_e_sqlite3`.</span><span class="sxs-lookup"><span data-stu-id="1dc3d-577">Starting with EF Core 3.0, EF Core uses `SQLitePCLRaw.bundle_e_sqlite3`.</span></span>

<span data-ttu-id="1dc3d-578">**Причина**</span><span class="sxs-lookup"><span data-stu-id="1dc3d-578">**Why**</span></span>

<span data-ttu-id="1dc3d-579">Это изменение было внесено, чтобы версия SQLite, используемая в iOS, была согласована с другими платформами.</span><span class="sxs-lookup"><span data-stu-id="1dc3d-579">This change was made so that the version of SQLite used on iOS consistent with other platforms.</span></span>

<span data-ttu-id="1dc3d-580">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="1dc3d-580">**Mitigations**</span></span>

<span data-ttu-id="1dc3d-581">Чтобы использовать собственную версию SQLite в iOS, настройте `Microsoft.Data.Sqlite` для использования другого пакета `SQLitePCLRaw`.</span><span class="sxs-lookup"><span data-stu-id="1dc3d-581">To use the native SQLite version on iOS, configure `Microsoft.Data.Sqlite` to use a different `SQLitePCLRaw` bundle.</span></span>

## <a name="guid-values-are-now-stored-as-text-on-sqlite"></a><span data-ttu-id="1dc3d-582">Теперь значения Guid хранятся в SQLite в виде значений типа TEXT</span><span class="sxs-lookup"><span data-stu-id="1dc3d-582">Guid values are now stored as TEXT on SQLite</span></span>

[<span data-ttu-id="1dc3d-583">Отслеживание вопроса № 15078</span><span class="sxs-lookup"><span data-stu-id="1dc3d-583">Tracking Issue #15078</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15078)

<span data-ttu-id="1dc3d-584">Это изменение было внесено в предварительную версию 4 EF Core 3.0.</span><span class="sxs-lookup"><span data-stu-id="1dc3d-584">This change was introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="1dc3d-585">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="1dc3d-585">**Old behavior**</span></span>

<span data-ttu-id="1dc3d-586">Ранее значения типа Guid хранились в SQLite в виде значений типа BLOB.</span><span class="sxs-lookup"><span data-stu-id="1dc3d-586">Guid values were previously sored as BLOB values on SQLite.</span></span>

<span data-ttu-id="1dc3d-587">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="1dc3d-587">**New behavior**</span></span>

<span data-ttu-id="1dc3d-588">Теперь значения типа Guid хранятся в виде значений типа TEXT.</span><span class="sxs-lookup"><span data-stu-id="1dc3d-588">Guid values are now sotred as TEXT.</span></span>

<span data-ttu-id="1dc3d-589">**Причина**</span><span class="sxs-lookup"><span data-stu-id="1dc3d-589">**Why**</span></span>

<span data-ttu-id="1dc3d-590">Двоичный формат типа Guid не стандартизирован.</span><span class="sxs-lookup"><span data-stu-id="1dc3d-590">The binary format of Guids is not standardized.</span></span> <span data-ttu-id="1dc3d-591">Хранение значений в виде значений типа TEXT повышает уровень совместимости базы данных с другими технологиями.</span><span class="sxs-lookup"><span data-stu-id="1dc3d-591">Storing the values as TEXT makes the database more compatible with other technologies.</span></span>

<span data-ttu-id="1dc3d-592">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="1dc3d-592">**Mitigations**</span></span>

<span data-ttu-id="1dc3d-593">Существующие базы данных можно перенести в новый формат, выполнив код SQL следующим образом.</span><span class="sxs-lookup"><span data-stu-id="1dc3d-593">You can migrate existing databases to the new format by executing SQL like the following.</span></span>

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

<span data-ttu-id="1dc3d-594">В EF Core можно продолжить использовать прежнее поведение, настроив преобразователь значений для этих свойств.</span><span class="sxs-lookup"><span data-stu-id="1dc3d-594">In EF Core, you could also continue using the previous behavior by configuirng a value converter on these properties.</span></span>

``` csharp
modelBuilder
    .Entity<MyEntity>()
    .Property(e => e.GuidProperty)
    .HasConversion(
        g => g.ToByteArray(),
        b => new Guid(b));
```

<span data-ttu-id="1dc3d-595">Microsoft.Data.Sqlite по-прежнему может читать значения Guid из столбцов BLOB и TEXT. Но поскольку изменился формат по умолчанию для параметров и констант, скорее всего, вам потребуется выполнить некоторые действия в большинстве сценариев, где используются значения Guid.</span><span class="sxs-lookup"><span data-stu-id="1dc3d-595">Microsoft.Data.Sqlite remains capable of reading Guid values from both BLOB and TEXT columns; however, since the default format for parameters and constants has changed you'll likely need to take action for most scenarios involving Guids.</span></span>

## <a name="char-values-are-now-stored-as-text-on-sqlite"></a><span data-ttu-id="1dc3d-596">Теперь значения типа Char хранятся в SQLite в виде значений типа TEXT</span><span class="sxs-lookup"><span data-stu-id="1dc3d-596">Char values are now stored as TEXT on SQLite</span></span>

[<span data-ttu-id="1dc3d-597">Отслеживание вопроса № 15020</span><span class="sxs-lookup"><span data-stu-id="1dc3d-597">Tracking Issue #15020</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15020)

<span data-ttu-id="1dc3d-598">Это изменение было внесено в предварительную версию 4 EF Core 3.0.</span><span class="sxs-lookup"><span data-stu-id="1dc3d-598">This change was introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="1dc3d-599">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="1dc3d-599">**Old behavior**</span></span>

<span data-ttu-id="1dc3d-600">Ранее значения типа Char хранились в SQLite в виде значений типа INTEGER.</span><span class="sxs-lookup"><span data-stu-id="1dc3d-600">Char values were previously sored as INTEGER values on SQLite.</span></span> <span data-ttu-id="1dc3d-601">Например, значение типа Char *A* хранилось как целочисленное значение 65.</span><span class="sxs-lookup"><span data-stu-id="1dc3d-601">For example, a char value of *A* was stored as the integer value 65.</span></span>

<span data-ttu-id="1dc3d-602">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="1dc3d-602">**New behavior**</span></span>

<span data-ttu-id="1dc3d-603">Теперь значения типа Char хранятся в виде значений типа TEXT.</span><span class="sxs-lookup"><span data-stu-id="1dc3d-603">Char values are now sotred as TEXT.</span></span>

<span data-ttu-id="1dc3d-604">**Причина**</span><span class="sxs-lookup"><span data-stu-id="1dc3d-604">**Why**</span></span>

<span data-ttu-id="1dc3d-605">Хранение значений в виде значений типа TEXT является более естественным вариантом и повышает уровень совместимости базы данных с другими технологиями.</span><span class="sxs-lookup"><span data-stu-id="1dc3d-605">Storing the values as TEXT is more natural and makes the database more compatible with other technologies.</span></span>

<span data-ttu-id="1dc3d-606">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="1dc3d-606">**Mitigations**</span></span>

<span data-ttu-id="1dc3d-607">Существующие базы данных можно перенести в новый формат, выполнив код SQL следующим образом.</span><span class="sxs-lookup"><span data-stu-id="1dc3d-607">You can migrate existing databases to the new format by executing SQL like the following.</span></span>

``` sql
UPDATE MyTable
SET CharColumn = char(CharColumn)
WHERE typeof(CharColumn) = 'integer';
```

<span data-ttu-id="1dc3d-608">В EF Core можно продолжить использовать прежнее поведение, настроив преобразователь значений для этих свойств.</span><span class="sxs-lookup"><span data-stu-id="1dc3d-608">In EF Core, you could also continue using the previous behavior by configuirng a value converter on these properties.</span></span>

``` csharp
modelBuilder
    .Entity<MyEntity>()
    .Property(e => e.CharProperty)
    .HasConversion(
        c => (long)c,
        i => (char)i);
```

<span data-ttu-id="1dc3d-609">Microsoft.Data.Sqlite также сохраняет возможность чтения значений символов из столбцов INTEGER и TEXT, поэтому для реализации определенных сценариев может не потребоваться никаких действий.</span><span class="sxs-lookup"><span data-stu-id="1dc3d-609">Microsoft.Data.Sqlite also remains capable of reading character values from both INTEGER and TEXT columns, so certain scenarios may not require any action.</span></span>

## <a name="migration-ids-are-now-generated-using-the-invariant-cultures-calendar"></a><span data-ttu-id="1dc3d-610">Идентификаторы миграции теперь создаются с использованием календаря инвариантного языка и региональных параметров</span><span class="sxs-lookup"><span data-stu-id="1dc3d-610">Migration IDs are now generated using the invariant culture's calendar</span></span>

[<span data-ttu-id="1dc3d-611">Отслеживание вопроса № 12978</span><span class="sxs-lookup"><span data-stu-id="1dc3d-611">Tracking Issue #12978</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12978)

<span data-ttu-id="1dc3d-612">Это изменение было внесено в предварительную версию 4 EF Core 3.0.</span><span class="sxs-lookup"><span data-stu-id="1dc3d-612">This change was introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="1dc3d-613">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="1dc3d-613">**Old behavior**</span></span>

<span data-ttu-id="1dc3d-614">Идентификаторы миграции непреднамеренно создавались с использованием календаря текущего языка и региональных параметров.</span><span class="sxs-lookup"><span data-stu-id="1dc3d-614">Migration IDs were inadvertantly generated using the currret culture's calendar.</span></span>

<span data-ttu-id="1dc3d-615">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="1dc3d-615">**New behavior**</span></span>

<span data-ttu-id="1dc3d-616">Теперь идентификаторы миграций всегда создаются с использованием календаря инвариантного языка и региональных параметров (григорианского).</span><span class="sxs-lookup"><span data-stu-id="1dc3d-616">Migration IDs are now always generated using the invariant culture's calendar (Gregorian).</span></span>

<span data-ttu-id="1dc3d-617">**Причина**</span><span class="sxs-lookup"><span data-stu-id="1dc3d-617">**Why**</span></span>

<span data-ttu-id="1dc3d-618">Порядок миграций играет важную роль при обновлении базы данных или разрешении конфликтов слияния.</span><span class="sxs-lookup"><span data-stu-id="1dc3d-618">The order of migrations is important when updating the database or resolving merge conflicts.</span></span> <span data-ttu-id="1dc3d-619">Использование инвариантного календаря позволяет избежать проблем с порядком выполнения, которые могут быть связаны с наличием разных системных календарей у членов группы.</span><span class="sxs-lookup"><span data-stu-id="1dc3d-619">Using the invariant calendar avoids ordering issues that can result from team members having different system calendars.</span></span>

<span data-ttu-id="1dc3d-620">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="1dc3d-620">**Mitigations**</span></span>

<span data-ttu-id="1dc3d-621">Это изменение затрагивает всех, кто использует календари, отличные от григорианского, в которых значение года больше, чем в григорианском (например, тай-буддистский календарь).</span><span class="sxs-lookup"><span data-stu-id="1dc3d-621">This change affects anyone using a non-Gregorian calender where the year is greater than the Gregorian calendar (like the Thai Buddhist calendar).</span></span> <span data-ttu-id="1dc3d-622">Имеющиеся идентификаторы миграций следует обновить, чтобы новые миграции выполнялись после существующих.</span><span class="sxs-lookup"><span data-stu-id="1dc3d-622">Existing migration IDs will need to be updated so that new migrations are ordered after existing migrations.</span></span>

<span data-ttu-id="1dc3d-623">Идентификатор миграции можно найти в атрибуте Migration в файлы конструктора миграций.</span><span class="sxs-lookup"><span data-stu-id="1dc3d-623">The migration ID can be found in the Migration attribute in the migrations' designer files.</span></span>

``` diff
 [DbContext(typeof(MyDbContext))]
-[Migration("25620318122820_MyMigration")]
+[Migration("20190318122820_MyMigration")]
 partial class MyMigration
 {
```

<span data-ttu-id="1dc3d-624">Кроме того, потребуется обновить таблицу журнала миграций.</span><span class="sxs-lookup"><span data-stu-id="1dc3d-624">The Migrations history table also needs to be updated.</span></span>

``` sql
UPDATE __EFMigrationsHistory
SET MigrationId = CONCAT(LEFT(MigrationId, 4)  - 543, SUBSTRING(MigrationId, 4, 150))
```

## <a name="logquerypossibleexceptionwithaggregateoperator-has-been-renamed"></a><span data-ttu-id="1dc3d-625">Оператор LogQueryPossibleExceptionWithAggregateOperator был переименован</span><span class="sxs-lookup"><span data-stu-id="1dc3d-625">LogQueryPossibleExceptionWithAggregateOperator has been renamed</span></span>

[<span data-ttu-id="1dc3d-626">Отслеживание проблемы № 10985</span><span class="sxs-lookup"><span data-stu-id="1dc3d-626">Tracking Issue #10985</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10985)

<span data-ttu-id="1dc3d-627">Это изменение было внесено в предварительную версию 4 EF Core 3.0.</span><span class="sxs-lookup"><span data-stu-id="1dc3d-627">This change was introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="1dc3d-628">**Изменение**</span><span class="sxs-lookup"><span data-stu-id="1dc3d-628">**Change**</span></span>

<span data-ttu-id="1dc3d-629">`RelationalEventId.LogQueryPossibleExceptionWithAggregateOperator` переименован в `RelationalEventId.LogQueryPossibleExceptionWithAggregateOperatorWarning`.</span><span class="sxs-lookup"><span data-stu-id="1dc3d-629">`RelationalEventId.LogQueryPossibleExceptionWithAggregateOperator` has been renamed to `RelationalEventId.LogQueryPossibleExceptionWithAggregateOperatorWarning`.</span></span>

<span data-ttu-id="1dc3d-630">**Причина**</span><span class="sxs-lookup"><span data-stu-id="1dc3d-630">**Why**</span></span>

<span data-ttu-id="1dc3d-631">Выравнивает именование этого события предупреждения с другими событиями предупреждения.</span><span class="sxs-lookup"><span data-stu-id="1dc3d-631">Aligns the naming of this warning event with all other warning events.</span></span>

<span data-ttu-id="1dc3d-632">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="1dc3d-632">**Mitigations**</span></span>

<span data-ttu-id="1dc3d-633">Используйте новое имя.</span><span class="sxs-lookup"><span data-stu-id="1dc3d-633">Use the new name.</span></span> <span data-ttu-id="1dc3d-634">(Обратите внимание, что номер события не изменен.)</span><span class="sxs-lookup"><span data-stu-id="1dc3d-634">(Note that the event ID number has not changed.)</span></span>

## <a name="clarify-api-for-foreign-key-constraint-names"></a><span data-ttu-id="1dc3d-635">Уточнение API для имен ограничений внешнего ключа</span><span class="sxs-lookup"><span data-stu-id="1dc3d-635">Clarify API for foreign key constraint names</span></span>

[<span data-ttu-id="1dc3d-636">Отслеживание проблемы № 10730</span><span class="sxs-lookup"><span data-stu-id="1dc3d-636">Tracking Issue #10730</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10730)

<span data-ttu-id="1dc3d-637">Это изменение было внесено в предварительную версию 4 EF Core 3.0.</span><span class="sxs-lookup"><span data-stu-id="1dc3d-637">This change was introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="1dc3d-638">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="1dc3d-638">**Old behavior**</span></span>

<span data-ttu-id="1dc3d-639">До EF Core 3.0 имена ограничений внешнего ключа назывались просто именами.</span><span class="sxs-lookup"><span data-stu-id="1dc3d-639">Before EF Core 3.0, foreign key constraint names were referred to as simply the "name".</span></span> <span data-ttu-id="1dc3d-640">Например:</span><span class="sxs-lookup"><span data-stu-id="1dc3d-640">For example:</span></span>

```C#
var constraintName = myForeignKey.Name;
```

<span data-ttu-id="1dc3d-641">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="1dc3d-641">**New behavior**</span></span>

<span data-ttu-id="1dc3d-642">Начиная с EF Core 3.0 имена ограничений внешнего ключа называются "имя ограничения".</span><span class="sxs-lookup"><span data-stu-id="1dc3d-642">Starting with EF Core 3.0, foreign key constraint names are now referred to as the "constaint name".</span></span> <span data-ttu-id="1dc3d-643">Например:</span><span class="sxs-lookup"><span data-stu-id="1dc3d-643">For example:</span></span>

```C#
var constraintName = myForeignKey.ConstraintName;
```

<span data-ttu-id="1dc3d-644">**Причина**</span><span class="sxs-lookup"><span data-stu-id="1dc3d-644">**Why**</span></span>

<span data-ttu-id="1dc3d-645">Это изменение обеспечивает согласованность именования в этой области, а также проясняет, что это имя ограничения внешнего ключа, а не имя столбца или свойства, для которого определен внешний ключ.</span><span class="sxs-lookup"><span data-stu-id="1dc3d-645">This change brings consistency to naming in this area, and also clarifies that this is the name of the foreign key constaint, and not the column or property name that the foreign key is defined on.</span></span>

<span data-ttu-id="1dc3d-646">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="1dc3d-646">**Mitigations**</span></span>

<span data-ttu-id="1dc3d-647">Используйте новое имя.</span><span class="sxs-lookup"><span data-stu-id="1dc3d-647">Use the new name.</span></span>
