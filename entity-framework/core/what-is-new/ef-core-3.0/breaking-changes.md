---
title: Критические изменения в EF Core 3.0 — EF Core
author: divega
ms.date: 02/19/2019
ms.assetid: EE2878C9-71F9-4FA5-9BC4-60517C7C9830
uid: core/what-is-new/ef-core-3.0/breaking-changes
ms.openlocfilehash: 7ed55d4cae36f6b25059a5b218db4b0d5e2fb266
ms.sourcegitcommit: 645785187ae23ddf7d7b0642c7a4da5ffb0c7f30
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/25/2019
ms.locfileid: "58419748"
---
# <a name="breaking-changes-included-in-ef-core-30-currently-in-preview"></a><span data-ttu-id="20cbd-102">Критические изменения в EF Core 3.0 (сейчас предоставляются в виде предварительной версии)</span><span class="sxs-lookup"><span data-stu-id="20cbd-102">Breaking changes included in EF Core 3.0 (currently in preview)</span></span>

> [!IMPORTANT]
> <span data-ttu-id="20cbd-103">Обратите внимание на то, что наборы возможностей и расписания будущих выпусков могут быть изменены, и, несмотря на то, что мы стараемся поддерживать эту страницу в актуальном состоянии, в определенные моменты времени содержание страницы может не отражать наши текущие планы.</span><span class="sxs-lookup"><span data-stu-id="20cbd-103">Please note that the feature sets and schedules of future releases are always subject to change, and although we will try to keep this page up to date, it may not reflect our latest plans at all times.</span></span>

<span data-ttu-id="20cbd-104">Указанные ниже изменения API и поведения могут нарушать работу приложений, разработанных для EF Core 2.2.x, при их обновлении до версии 3.0.0.</span><span class="sxs-lookup"><span data-stu-id="20cbd-104">The following API and behavior changes have the potential to break applications developed for EF Core 2.2.x when upgrading them to 3.0.0.</span></span>
<span data-ttu-id="20cbd-105">Изменения, которые, по нашим ожиданиям, повлияют только на поставщиков баз данных, описаны в разделе [изменений для поставщиков](../../providers/provider-log.md).</span><span class="sxs-lookup"><span data-stu-id="20cbd-105">Changes that we expect to only impact database providers are documented under [provider changes](../../providers/provider-log.md).</span></span>
<span data-ttu-id="20cbd-106">Здесь не описаны критические изменения в новых функциях, добавленных между разными предварительными версиями 3.0.</span><span class="sxs-lookup"><span data-stu-id="20cbd-106">Breaks in new features introduced from one 3.0 preview to another 3.0 preview aren't documented here.</span></span>

## <a name="linq-queries-are-no-longer-evaluated-on-the-client"></a><span data-ttu-id="20cbd-107">Запросы LINQ больше не вычисляются на клиенте</span><span class="sxs-lookup"><span data-stu-id="20cbd-107">LINQ queries are no longer evaluated on the client</span></span>

<span data-ttu-id="20cbd-108">[Отслеживание вопроса № 14935](https://github.com/aspnet/EntityFrameworkCore/issues/14935)
[Также см. вопрос № 12795](https://github.com/aspnet/EntityFrameworkCore/issues/12795)</span><span class="sxs-lookup"><span data-stu-id="20cbd-108">[Tracking Issue #14935](https://github.com/aspnet/EntityFrameworkCore/issues/14935)
[Also see issue #12795](https://github.com/aspnet/EntityFrameworkCore/issues/12795)</span></span>

<span data-ttu-id="20cbd-109">Это изменение будет внесено в предварительную версию 4 EF Core 3.0.</span><span class="sxs-lookup"><span data-stu-id="20cbd-109">This change will be introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="20cbd-110">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="20cbd-110">**Old behavior**</span></span>

<span data-ttu-id="20cbd-111">До выхода версии 3.0, если системе EF Core не удавалось преобразовать выражение, являющееся частью запроса, в код SQL или параметр, она автоматически рассчитывала это выражение на клиенте.</span><span class="sxs-lookup"><span data-stu-id="20cbd-111">Before 3.0, when EF Core couldn't convert an expression that was part of a query to either SQL or a parameter, it automatically evaluated the expression on the client.</span></span>
<span data-ttu-id="20cbd-112">По умолчанию вычисление на клиенте потенциально ресурсоемких выражений вызывало лишь отображение предупреждения.</span><span class="sxs-lookup"><span data-stu-id="20cbd-112">By default, client evaluation of potentially expensive expressions only triggered a warning.</span></span>

<span data-ttu-id="20cbd-113">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="20cbd-113">**New behavior**</span></span>

<span data-ttu-id="20cbd-114">Начиная с версии 3.0 система EF Core разрешает вычислять на клиенте только выражения в высокоуровневой проекции (последний вызов `Select()` в запросе).</span><span class="sxs-lookup"><span data-stu-id="20cbd-114">Starting with 3.0, EF Core only allows expressions in the top-level projection (the last `Select()` call in the query) to be evaluated on the client.</span></span>
<span data-ttu-id="20cbd-115">Если выражения в любой другой части запроса невозможно преобразовать в код SQL или параметр, возникает исключение.</span><span class="sxs-lookup"><span data-stu-id="20cbd-115">When expressions in any other part of the query can't be converted to either SQL or a parameter, an exception is thrown.</span></span>

<span data-ttu-id="20cbd-116">**Причина**</span><span class="sxs-lookup"><span data-stu-id="20cbd-116">**Why**</span></span>

<span data-ttu-id="20cbd-117">Автоматическое вычисление запросов на клиенте позволяет выполнять многие запросы, даже если не удается преобразовать их важные части.</span><span class="sxs-lookup"><span data-stu-id="20cbd-117">Automatic client evaluation of queries allows many queries to be executed even if important parts of them can't be translated.</span></span>
<span data-ttu-id="20cbd-118">Это может привести к непредвиденному и потенциально опасному поведению, которое может стать заметным только в рабочей среде.</span><span class="sxs-lookup"><span data-stu-id="20cbd-118">This behavior can result in unexpected and potentially damaging behavior that may only become evident in production.</span></span>
<span data-ttu-id="20cbd-119">Например, условие в вызове `Where()`, которое невозможно преобразовать, может привести к передаче всех строк из таблицы с сервера базы данных и к применению фильтра на клиенте.</span><span class="sxs-lookup"><span data-stu-id="20cbd-119">For example, a condition in a `Where()` call which can't be translated can cause all rows from the table to be transferred from the database server, and the filter to be applied on the client.</span></span>
<span data-ttu-id="20cbd-120">Такая ситуация легко может остаться незамеченной, если таблица содержит небольшое число строк в разработке, и дать резкие отрицательные последствия при переносе приложения в рабочую среду, когда таблица может содержать миллионы строк.</span><span class="sxs-lookup"><span data-stu-id="20cbd-120">This situation can easily go undetected if the table contains only a few rows in development, but hit hard when the application moves to production, where the table may contain millions of rows.</span></span>
<span data-ttu-id="20cbd-121">Практика показала, что предупреждения о вычислении на клиенте также часто игнорируются во время разработки.</span><span class="sxs-lookup"><span data-stu-id="20cbd-121">Client evaluation warnings also proved too easy to ignore during development.</span></span>

<span data-ttu-id="20cbd-122">Кроме того, автоматическое вычисление на клиенте может привести к проблемам, из-за которых улучшение преобразования запросов для определенных выражений приводит к возникновению непреднамеренных критических изменений между выпусками.</span><span class="sxs-lookup"><span data-stu-id="20cbd-122">Besides this, automatic client evaluation can lead to issues in which improving query translation for specific expressions caused unintended breaking changes between releases.</span></span>

<span data-ttu-id="20cbd-123">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="20cbd-123">**Mitigations**</span></span>

<span data-ttu-id="20cbd-124">Если запрос невозможно преобразовать полностью, то перепишите его в форме, которую можно преобразовать, либо используйте `AsEnumerable()`, `ToList()` или аналогичный элемент, чтобы явно перенести данные обратно на клиент, где можно произвести их дальнейшую обработку с помощью LINQ-to-Objects.</span><span class="sxs-lookup"><span data-stu-id="20cbd-124">If a query can't be fully translated, then either rewrite the query in a form that can be translated, or use `AsEnumerable()`, `ToList()`, or similar to explicitly bring data back to the client where it can then be further processed using LINQ-to-Objects.</span></span>

## <a name="entity-framework-core-is-no-longer-part-of-the-aspnet-core-shared-framework"></a><span data-ttu-id="20cbd-125">Entity Framework Core больше не является частью общей платформы ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="20cbd-125">Entity Framework Core is no longer part of the ASP.NET Core shared framework</span></span>

[<span data-ttu-id="20cbd-126">Отслеживание объявлений о вопросе 325</span><span class="sxs-lookup"><span data-stu-id="20cbd-126">Tracking Issue Announcements#325</span></span>](https://github.com/aspnet/Announcements/issues/325)

<span data-ttu-id="20cbd-127">Это изменение было внесено в предварительную версию 1 ASP.NET Core 3.0.</span><span class="sxs-lookup"><span data-stu-id="20cbd-127">This change was introduced in ASP.NET Core 3.0-preview 1.</span></span> 

<span data-ttu-id="20cbd-128">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="20cbd-128">**Old behavior**</span></span>

<span data-ttu-id="20cbd-129">До выхода ASP.NET Core 3.0 при добавлении ссылки на пакет для `Microsoft.AspNetCore.App` или `Microsoft.AspNetCore.All` она включала в себя EF Core и некоторые поставщики данных EF Core, например поставщик SQL Server.</span><span class="sxs-lookup"><span data-stu-id="20cbd-129">Before ASP.NET Core 3.0, when you added a package reference to `Microsoft.AspNetCore.App` or `Microsoft.AspNetCore.All`, it would include EF Core and some of the EF Core data providers like the SQL Server provider.</span></span>

<span data-ttu-id="20cbd-130">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="20cbd-130">**New behavior**</span></span>

<span data-ttu-id="20cbd-131">Начиная с версии 3.0 общая платформа ASP.NET Core не включает в себя EF Core или поставщики данных EF Core.</span><span class="sxs-lookup"><span data-stu-id="20cbd-131">Starting in 3.0, the ASP.NET Core shared framework doesn't include EF Core or any EF Core data providers.</span></span>

<span data-ttu-id="20cbd-132">**Причина**</span><span class="sxs-lookup"><span data-stu-id="20cbd-132">**Why**</span></span>

<span data-ttu-id="20cbd-133">До этого изменения для получения EF Core требовались различные действия в зависимости от того, ориентировано ли приложение на ASP.NET Core и SQL Server.</span><span class="sxs-lookup"><span data-stu-id="20cbd-133">Before this change, getting EF Core required different steps depending on whether the application targeted ASP.NET Core and SQL Server or not.</span></span> <span data-ttu-id="20cbd-134">Кроме того, обновление ASP.NET Core приводило к принудительному обновлению EF Core и поставщика SQL Server, что иногда нежелательно.</span><span class="sxs-lookup"><span data-stu-id="20cbd-134">Also, upgrading ASP.NET Core forced the upgrade of EF Core and the SQL Server provider, which isn't always desirable.</span></span>

<span data-ttu-id="20cbd-135">Благодаря этому изменению процесс получения EF Core стал одинаковым для всех поставщиков, поддерживаемых реализаций .NET и типов приложений.</span><span class="sxs-lookup"><span data-stu-id="20cbd-135">With this change, the experience of getting EF Core is the same across all providers, supported .NET implementations and application types.</span></span>
<span data-ttu-id="20cbd-136">Кроме того, теперь разработчики могут точно управлять временем обновления EF Core и поставщиков данных EF Core.</span><span class="sxs-lookup"><span data-stu-id="20cbd-136">Developers can also now control exactly when EF Core and EF Core data providers are upgraded.</span></span>

<span data-ttu-id="20cbd-137">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="20cbd-137">**Mitigations**</span></span>

<span data-ttu-id="20cbd-138">Чтобы использовать EF Core в приложении ASP.NET Core 3.0 или любом другом поддерживаемом приложении, нужно явно добавить ссылку на пакет в поставщик базы данных EF Core, который ваше приложение будет использовать.</span><span class="sxs-lookup"><span data-stu-id="20cbd-138">To use EF Core in an ASP.NET Core 3.0 application or any other supported application, explicitly add a package reference to the EF Core database provider that your application will use.</span></span>

## <a name="query-execution-is-logged-at-debug-level"></a><span data-ttu-id="20cbd-139">Выполнение запроса регистрируется на уровне отладки Debug</span><span class="sxs-lookup"><span data-stu-id="20cbd-139">Query execution is logged at Debug level</span></span>

[<span data-ttu-id="20cbd-140">Отслеживание вопроса 14523</span><span class="sxs-lookup"><span data-stu-id="20cbd-140">Tracking Issue #14523</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14523)

<span data-ttu-id="20cbd-141">Это изменение было внесено в предварительную версию 3 EF Core 3.0.</span><span class="sxs-lookup"><span data-stu-id="20cbd-141">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="20cbd-142">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="20cbd-142">**Old behavior**</span></span>

<span data-ttu-id="20cbd-143">До выхода EF Core 3.0 выполнение запросов и других команд зарегистрировалось на уровне `Info`.</span><span class="sxs-lookup"><span data-stu-id="20cbd-143">Before EF Core 3.0, execution of queries and other commands was logged at the `Info` level.</span></span>

<span data-ttu-id="20cbd-144">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="20cbd-144">**New behavior**</span></span>

<span data-ttu-id="20cbd-145">Начиная с EF Core 3.0 регистрация выполнения команды или кода SQL осуществляется на уровне `Debug`.</span><span class="sxs-lookup"><span data-stu-id="20cbd-145">Starting with EF Core 3.0, logging of command/SQL execution is at the `Debug` level.</span></span>

<span data-ttu-id="20cbd-146">**Причина**</span><span class="sxs-lookup"><span data-stu-id="20cbd-146">**Why**</span></span>

<span data-ttu-id="20cbd-147">Это изменение было внесено для снижения шума на уровне ведения журнала `Info`.</span><span class="sxs-lookup"><span data-stu-id="20cbd-147">This change was made to reduce the noise at the `Info` log level.</span></span>

<span data-ttu-id="20cbd-148">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="20cbd-148">**Mitigations**</span></span>

<span data-ttu-id="20cbd-149">Это событие ведения журнала определено `RelationalEventId.CommandExecuting` с идентификатором события 20100.</span><span class="sxs-lookup"><span data-stu-id="20cbd-149">This logging event is defined by `RelationalEventId.CommandExecuting` with event ID 20100.</span></span>
<span data-ttu-id="20cbd-150">Чтобы снова начать ведение журнала SQL на уровне `Info`, явно настройте уровень в методе `OnConfiguring` или `AddDbContext`.</span><span class="sxs-lookup"><span data-stu-id="20cbd-150">To log SQL at the `Info` level again, explicitly configure the level in `OnConfiguring` or `AddDbContext`.</span></span>
<span data-ttu-id="20cbd-151">Например:</span><span class="sxs-lookup"><span data-stu-id="20cbd-151">For example:</span></span>
```C#
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    => optionsBuilder
        .UseSqlServer(connectionString)
        .ConfigureWarnings(c => c.Log((RelationalEventId.CommandExecuting, LogLevel.Info)));
```

## <a name="temporary-key-values-are-no-longer-set-onto-entity-instances"></a><span data-ttu-id="20cbd-152">Временные значения ключа больше не устанавливаются для экземпляров сущностей</span><span class="sxs-lookup"><span data-stu-id="20cbd-152">Temporary key values are no longer set onto entity instances</span></span>

[<span data-ttu-id="20cbd-153">Отслеживание вопроса 12378</span><span class="sxs-lookup"><span data-stu-id="20cbd-153">Tracking Issue #12378</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12378)

<span data-ttu-id="20cbd-154">Это изменение было внесено в предварительную версию 2 EF Core 3.0.</span><span class="sxs-lookup"><span data-stu-id="20cbd-154">This change was introduced in EF Core 3.0-preview 2.</span></span>

<span data-ttu-id="20cbd-155">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="20cbd-155">**Old behavior**</span></span>

<span data-ttu-id="20cbd-156">До выхода EF Core 3.0 временные значения назначались всем свойствам ключей, которые позднее получили бы реальное значение, созданное базой данных.</span><span class="sxs-lookup"><span data-stu-id="20cbd-156">Before EF Core 3.0, temporary values were assigned to all key properties that would later have a real value generated by the database.</span></span>
<span data-ttu-id="20cbd-157">Обычно эти временные значения были большими отрицательными числами.</span><span class="sxs-lookup"><span data-stu-id="20cbd-157">Usually these temporary values were large negative numbers.</span></span>

<span data-ttu-id="20cbd-158">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="20cbd-158">**New behavior**</span></span>

<span data-ttu-id="20cbd-159">Начиная с версии 3.0 EF Core сохраняет временное значение ключа как часть сведений об отслеживании сущности, оставляя само свойство ключа без изменений.</span><span class="sxs-lookup"><span data-stu-id="20cbd-159">Starting with 3.0, EF Core stores the temporary key value as part of the entity's tracking information, and leaves the key property itself unchanged.</span></span>

<span data-ttu-id="20cbd-160">**Причина**</span><span class="sxs-lookup"><span data-stu-id="20cbd-160">**Why**</span></span>

<span data-ttu-id="20cbd-161">Это изменение было внесено для ситуации, когда временные значения ключей ошибочно становились постоянными из-за перемещения сущности, которая ранее отслеживалась некоторым экземпляром `DbContext`, в другой экземпляр `DbContext`.</span><span class="sxs-lookup"><span data-stu-id="20cbd-161">This change was made to prevent temporary key values from erroneously becoming permanent when an entity that has been previously tracked by some `DbContext` instance is moved to a different `DbContext` instance.</span></span> 

<span data-ttu-id="20cbd-162">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="20cbd-162">**Mitigations**</span></span>

<span data-ttu-id="20cbd-163">Приложения, которые назначают значения первичного ключа внешним ключам для формирования ассоциаций между сущностями, могут использовать старое поведение, если первичные ключи сформированы хранилищем и принадлежат сущностям в состоянии `Added`.</span><span class="sxs-lookup"><span data-stu-id="20cbd-163">Applications that assign primary key values onto foreign keys to form associations between entities may depend on the old behavior if the primary keys are store-generated and belong to entities in the `Added` state.</span></span>
<span data-ttu-id="20cbd-164">Этого можно избежать следующим образом:</span><span class="sxs-lookup"><span data-stu-id="20cbd-164">This can be avoided by:</span></span>
* <span data-ttu-id="20cbd-165">Отказ от использования ключей, сформированных хранилищем.</span><span class="sxs-lookup"><span data-stu-id="20cbd-165">Not using store-generated keys.</span></span>
* <span data-ttu-id="20cbd-166">Настройка свойств навигации для формирования отношений вместо задания значений внешнего ключа.</span><span class="sxs-lookup"><span data-stu-id="20cbd-166">Setting navigation properties to form relationships instead of setting foreign key values.</span></span>
* <span data-ttu-id="20cbd-167">Получение фактических временных значений ключа из сведений об отслеживании сущности.</span><span class="sxs-lookup"><span data-stu-id="20cbd-167">Obtain the actual temporary key values from the entity's tracking information.</span></span>
<span data-ttu-id="20cbd-168">Например, `context.Entry(blog).Property(e => e.Id).CurrentValue` возвратит временное значение, даже если сам `blog.Id` не был задан.</span><span class="sxs-lookup"><span data-stu-id="20cbd-168">For example, `context.Entry(blog).Property(e => e.Id).CurrentValue` will return the temporary value even though `blog.Id` itself hasn't been set.</span></span>

## <a name="detectchanges-honors-store-generated-key-values"></a><span data-ttu-id="20cbd-169">DetectChanges учитывает значения ключей, сформированные хранилищем</span><span class="sxs-lookup"><span data-stu-id="20cbd-169">DetectChanges honors store-generated key values</span></span>

[<span data-ttu-id="20cbd-170">Отслеживание вопроса 14616</span><span class="sxs-lookup"><span data-stu-id="20cbd-170">Tracking Issue #14616</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14616)

<span data-ttu-id="20cbd-171">Это изменение было внесено в предварительную версию 3 EF Core 3.0.</span><span class="sxs-lookup"><span data-stu-id="20cbd-171">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="20cbd-172">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="20cbd-172">**Old behavior**</span></span>

<span data-ttu-id="20cbd-173">До выпуска EF Core 3.0 неотслеживаемая сущность, обнаруженная `DetectChanges`, отслеживалась в состоянии `Added` и вставлялась в новую строку при вызове `SaveChanges`.</span><span class="sxs-lookup"><span data-stu-id="20cbd-173">Before EF Core 3.0, an untracked entity found by `DetectChanges` would be tracked in the `Added` state and inserted as a new row when `SaveChanges` is called.</span></span>

<span data-ttu-id="20cbd-174">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="20cbd-174">**New behavior**</span></span>

<span data-ttu-id="20cbd-175">Начиная с EF Core 3.0, если сущность использует сформированные значения ключа и какое-либо значение ключа задано, то она будет отслеживаться в состоянии `Modified`.</span><span class="sxs-lookup"><span data-stu-id="20cbd-175">Starting with EF Core 3.0, if an entity is using generated key values and some key value is set, then the entity will be tracked in the `Modified` state.</span></span>
<span data-ttu-id="20cbd-176">Это означает, что предполагается наличие строки для сущности, и она будет обновлена при вызове `SaveChanges`.</span><span class="sxs-lookup"><span data-stu-id="20cbd-176">This means that a row for the entity is assumed to exist and it will be updated when `SaveChanges` is called.</span></span>
<span data-ttu-id="20cbd-177">Если значение ключа не задано или тип сущности не использует сформированные ключи, то новая сущность все равно будет отслеживаться в состоянии `Added`, как в предыдущих версиях.</span><span class="sxs-lookup"><span data-stu-id="20cbd-177">If the key value isn't set, or if the entity type isn't using generated keys, then the new entity will still be tracked as `Added` as in previous versions.</span></span>

<span data-ttu-id="20cbd-178">**Причина**</span><span class="sxs-lookup"><span data-stu-id="20cbd-178">**Why**</span></span>

<span data-ttu-id="20cbd-179">Это изменение было внесено, чтобы сделать работу с несвязными графами сущностей при использовании ключей, сформированных хранилищем, более простой и согласованной.</span><span class="sxs-lookup"><span data-stu-id="20cbd-179">This change was made to make it easier and more consistent to work with disconnected entity graphs while using store-generated keys.</span></span>

<span data-ttu-id="20cbd-180">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="20cbd-180">**Mitigations**</span></span>

<span data-ttu-id="20cbd-181">Это изменение может нарушить работу приложения, если тип сущности должен использовать сформированные ключи, однако значения ключей явно заданы для новых экземпляров.</span><span class="sxs-lookup"><span data-stu-id="20cbd-181">This change can break an application if an entity type is configured to use generated keys but key values are explicitly set for new instances.</span></span>
<span data-ttu-id="20cbd-182">Решение проблемы заключается в явном запрете свойствам ключей использовать сформированные значения.</span><span class="sxs-lookup"><span data-stu-id="20cbd-182">The fix is to explicitly configure the key properties to not use generated values.</span></span>
<span data-ttu-id="20cbd-183">Например, с помощью текучего API:</span><span class="sxs-lookup"><span data-stu-id="20cbd-183">For example, with the fluent API:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .ValueGeneratedNever();
```

<span data-ttu-id="20cbd-184">Или с помощью заметок к данным:</span><span class="sxs-lookup"><span data-stu-id="20cbd-184">Or with data annotations:</span></span>

```C#
[DatabaseGenerated(DatabaseGeneratedOption.None)]
public string Id { get; set; }
```

## <a name="cascade-deletions-now-happen-immediately-by-default"></a><span data-ttu-id="20cbd-185">Каскадные удаления теперь по умолчанию выполняются немедленно</span><span class="sxs-lookup"><span data-stu-id="20cbd-185">Cascade deletions now happen immediately by default</span></span>

[<span data-ttu-id="20cbd-186">Отслеживание вопроса 10114</span><span class="sxs-lookup"><span data-stu-id="20cbd-186">Tracking Issue #10114</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10114)

<span data-ttu-id="20cbd-187">Это изменение было внесено в предварительную версию 3 EF Core 3.0.</span><span class="sxs-lookup"><span data-stu-id="20cbd-187">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="20cbd-188">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="20cbd-188">**Old behavior**</span></span>

<span data-ttu-id="20cbd-189">До выхода версии 3.0 применяемые EF Core каскадные действия (удаление зависимых сущностей, когда требуемый субъект удален либо отношение с ним было разорвано) не выполнялись до вызова SaveChanges.</span><span class="sxs-lookup"><span data-stu-id="20cbd-189">Before 3.0, EF Core applied cascading actions (deleting dependent entities when a required principal is deleted or when the relationship to a required principal is severed) did not happen until SaveChanges was called.</span></span>

<span data-ttu-id="20cbd-190">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="20cbd-190">**New behavior**</span></span>

<span data-ttu-id="20cbd-191">Начиная с версии 3.0 EF Core применяет каскадные действия сразу после обнаружения условия триггера.</span><span class="sxs-lookup"><span data-stu-id="20cbd-191">Starting with 3.0, EF Core applies cascading actions as soon as the triggering condition is detected.</span></span>
<span data-ttu-id="20cbd-192">Например, вызов `context.Remove()` для удаления сущности субъекта приведет к немедленной установке всех отслеживаемых связанных необходимых зависимых объектов в состояние `Deleted`.</span><span class="sxs-lookup"><span data-stu-id="20cbd-192">For example, calling `context.Remove()` to delete a principal entity will result in all tracked related required dependents also being set to `Deleted` immediately.</span></span>

<span data-ttu-id="20cbd-193">**Причина**</span><span class="sxs-lookup"><span data-stu-id="20cbd-193">**Why**</span></span>

<span data-ttu-id="20cbd-194">Это изменение было внесено, чтобы улучшить работу в сценариях аудита и привязки данных, где важно понимать, какие сущности будут удалены _перед_ вызовом `SaveChanges`.</span><span class="sxs-lookup"><span data-stu-id="20cbd-194">This change was made to improve the experience for data binding and auditing scenarios where it is important to understand which entities will be deleted _before_ `SaveChanges` is called.</span></span>

<span data-ttu-id="20cbd-195">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="20cbd-195">**Mitigations**</span></span>

<span data-ttu-id="20cbd-196">Для восстановления прежнего поведения можно использовать параметры `context.ChangedTracker`.</span><span class="sxs-lookup"><span data-stu-id="20cbd-196">The previous behavior can be restored through settings on `context.ChangedTracker`.</span></span>
<span data-ttu-id="20cbd-197">Например:</span><span class="sxs-lookup"><span data-stu-id="20cbd-197">For example:</span></span>

```C#
context.ChangeTracker.CascadeDeleteTiming = CascadeTiming.OnSaveChanges;
context.ChangeTracker.DeleteOrphansTiming = CascadeTiming.OnSaveChanges;
```

## <a name="query-types-are-consolidated-with-entity-types"></a><span data-ttu-id="20cbd-198">Типы запросов объединяются с типами сущностей</span><span class="sxs-lookup"><span data-stu-id="20cbd-198">Query types are consolidated with entity types</span></span>

[<span data-ttu-id="20cbd-199">Отслеживание вопроса 14194</span><span class="sxs-lookup"><span data-stu-id="20cbd-199">Tracking Issue #14194</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14194)

<span data-ttu-id="20cbd-200">Это изменение было внесено в предварительную версию 3 EF Core 3.0.</span><span class="sxs-lookup"><span data-stu-id="20cbd-200">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="20cbd-201">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="20cbd-201">**Old behavior**</span></span>

<span data-ttu-id="20cbd-202">До выпуска EF Core 3.0 [типы запросов](xref:core/modeling/query-types) представляли собой средства для запроса данных, не определяющие первичный ключ структурированным образом.</span><span class="sxs-lookup"><span data-stu-id="20cbd-202">Before EF Core 3.0, [query types](xref:core/modeling/query-types) were a means to query data that doesn't define a primary key in a structured way.</span></span>
<span data-ttu-id="20cbd-203">То есть тип запроса использовался для сопоставления типов сущностей без ключей (скорее всего, из представления, но, возможно, и из таблицы), а обычный тип сущности использовался при наличии ключа (скорее всего, из таблицы, но, возможно, и из представления).</span><span class="sxs-lookup"><span data-stu-id="20cbd-203">That is, a query type was used for mapping entity types without keys (more likely from a view, but possibly from a table) while a regular entity type was used when a key was available (more likely from a table, but possibly from a view).</span></span>

<span data-ttu-id="20cbd-204">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="20cbd-204">**New behavior**</span></span>

<span data-ttu-id="20cbd-205">Теперь тип запроса стал просто типом сущности без первичного ключа.</span><span class="sxs-lookup"><span data-stu-id="20cbd-205">A query type now becomes just an entity type without a primary key.</span></span>
<span data-ttu-id="20cbd-206">Типы сущностей без ключей имеют ту же функциональность, что типы запросов в предыдущих версиях.</span><span class="sxs-lookup"><span data-stu-id="20cbd-206">Keyless entity types have the same functionality as query types in previous versions.</span></span>

<span data-ttu-id="20cbd-207">**Причина**</span><span class="sxs-lookup"><span data-stu-id="20cbd-207">**Why**</span></span>

<span data-ttu-id="20cbd-208">Это изменение было внесено, чтобы избежать путаницы с назначением типов запросов.</span><span class="sxs-lookup"><span data-stu-id="20cbd-208">This change was made to reduce the confusion around the purpose of query types.</span></span>
<span data-ttu-id="20cbd-209">В частности, они представляют собой типы сущностей без ключей и поэтому по определению доступны только для чтения, однако их не следует использовать только потому, что тип сущности должен быть доступен только для чтения.</span><span class="sxs-lookup"><span data-stu-id="20cbd-209">Specifically, they are keyless entity types and they are inherently read-only because of this, but they should not be used just because an entity type needs to be read-only.</span></span>
<span data-ttu-id="20cbd-210">Аналогично, их часто сопоставляют с представлениями, но только потому, что представления часто не определяют ключи.</span><span class="sxs-lookup"><span data-stu-id="20cbd-210">Likewise, they are often mapped to views, but this is only because views often don't define keys.</span></span>

<span data-ttu-id="20cbd-211">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="20cbd-211">**Mitigations**</span></span>

<span data-ttu-id="20cbd-212">Следующие компоненты этого API теперь являются устаревшими:</span><span class="sxs-lookup"><span data-stu-id="20cbd-212">The following parts of the API are now obsolete:</span></span>
* <span data-ttu-id="20cbd-213">**`ModelBuilder.Query<>()`** — вместо него следует вызвать `ModelBuilder.Entity<>().HasNoKey()`, чтобы пометить тип сущности как не имеющий ключей.</span><span class="sxs-lookup"><span data-stu-id="20cbd-213">**`ModelBuilder.Query<>()`** - Instead `ModelBuilder.Entity<>().HasNoKey()` needs to be called to mark an entity type as having no keys.</span></span>
<span data-ttu-id="20cbd-214">Это по-прежнему не следует настраивать посредством соглашения, чтобы избежать ошибки, когда первичный ключ ожидается, но не соответствует соглашению.</span><span class="sxs-lookup"><span data-stu-id="20cbd-214">This would still not be configured by convention to avoid misconfiguration when a primary key is expected, but doesn't match the convention.</span></span>
* <span data-ttu-id="20cbd-215">**`DbQuery<>`** — вместо него следует использовать `DbSet<>`.</span><span class="sxs-lookup"><span data-stu-id="20cbd-215">**`DbQuery<>`** - Instead `DbSet<>` should be used.</span></span>
* <span data-ttu-id="20cbd-216">**`DbContext.Query<>()`** — вместо него следует использовать `DbContext.Set<>()`.</span><span class="sxs-lookup"><span data-stu-id="20cbd-216">**`DbContext.Query<>()`** - Instead `DbContext.Set<>()` should be used.</span></span>

## <a name="configuration-api-for-owned-type-relationships-has-changed"></a><span data-ttu-id="20cbd-217">Изменение API конфигурации для отношений принадлежащего типа</span><span class="sxs-lookup"><span data-stu-id="20cbd-217">Configuration API for owned type relationships has changed</span></span>

<span data-ttu-id="20cbd-218">[Отслеживание вопроса 12444](https://github.com/aspnet/EntityFrameworkCore/issues/12444)
[Отслеживание вопроса 9148](https://github.com/aspnet/EntityFrameworkCore/issues/9148)
[Отслеживание вопроса 14153](https://github.com/aspnet/EntityFrameworkCore/issues/14153)</span><span class="sxs-lookup"><span data-stu-id="20cbd-218">[Tracking Issue #12444](https://github.com/aspnet/EntityFrameworkCore/issues/12444)
[Tracking Issue #9148](https://github.com/aspnet/EntityFrameworkCore/issues/9148)
[Tracking Issue #14153](https://github.com/aspnet/EntityFrameworkCore/issues/14153)</span></span>

<span data-ttu-id="20cbd-219">Это изменение было внесено в предварительную версию 3 EF Core 3.0.</span><span class="sxs-lookup"><span data-stu-id="20cbd-219">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="20cbd-220">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="20cbd-220">**Old behavior**</span></span>

<span data-ttu-id="20cbd-221">До выхода EF Core 3.0 конфигурация принадлежащего отношения выполнялась непосредственно после вызова `OwnsOne` или `OwnsMany`.</span><span class="sxs-lookup"><span data-stu-id="20cbd-221">Before EF Core 3.0, configuration of the owned relationship was performed directly after the `OwnsOne` or `OwnsMany` call.</span></span> 

<span data-ttu-id="20cbd-222">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="20cbd-222">**New behavior**</span></span>

<span data-ttu-id="20cbd-223">Начиная с EF Core 3.0 существует текучий API для настройки свойства навигации для владельца с помощью `WithOwner()`.</span><span class="sxs-lookup"><span data-stu-id="20cbd-223">Starting with EF Core 3.0, there is now fluent API to configure a navigation property to the owner using `WithOwner()`.</span></span>
<span data-ttu-id="20cbd-224">Например:</span><span class="sxs-lookup"><span data-stu-id="20cbd-224">For example:</span></span>

```C#
modelBuilder.Entity<Order>.OwnsOne(e => e.Details).WithOwner(e => e.Order);
```

<span data-ttu-id="20cbd-225">Конфигурация, связанная с отношением между владельцем и принадлежащим объектом, теперь должна быть включена в цепочку после `WithOwner()` по аналогии с настройкой других отношений.</span><span class="sxs-lookup"><span data-stu-id="20cbd-225">The configuration related to the relationship between owner and owned should now be chained after `WithOwner()` similarly to how other relationships are configured.</span></span>
<span data-ttu-id="20cbd-226">При этом конфигурация для принадлежащего типа сама будет включена в цепочку после `OwnsOne()/OwnsMany()`.</span><span class="sxs-lookup"><span data-stu-id="20cbd-226">While the configuration for the owned type itself would still be chained after `OwnsOne()/OwnsMany()`.</span></span>
<span data-ttu-id="20cbd-227">Например:</span><span class="sxs-lookup"><span data-stu-id="20cbd-227">For example:</span></span>

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

<span data-ttu-id="20cbd-228">Кроме того, вызов `Entity()`, `HasOne()` или `Set()` с целевым объектом принадлежащего типа теперь приведет к возникновению исключения.</span><span class="sxs-lookup"><span data-stu-id="20cbd-228">Additionally calling `Entity()`, `HasOne()`, or `Set()` with an owned type target will now throw an exception.</span></span>

<span data-ttu-id="20cbd-229">**Причина**</span><span class="sxs-lookup"><span data-stu-id="20cbd-229">**Why**</span></span>

<span data-ttu-id="20cbd-230">Это изменение было внесено для более четкого разграничения между настройкой самого принадлежащего типа и _отношения_ с ним.</span><span class="sxs-lookup"><span data-stu-id="20cbd-230">This change was made to create a cleaner separation between configuring the owned type itself and the _relationship to_ the owned type.</span></span>
<span data-ttu-id="20cbd-231">Это, в свою очередь, устраняет неоднозначность и путаницу при использовании таких методов, как `HasForeignKey`.</span><span class="sxs-lookup"><span data-stu-id="20cbd-231">This in turn removes ambiguity and confusion around methods like `HasForeignKey`.</span></span>

<span data-ttu-id="20cbd-232">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="20cbd-232">**Mitigations**</span></span>

<span data-ttu-id="20cbd-233">Измените конфигурацию отношений принадлежащего типа, чтобы использовать новую поверхность API, как показано в приведенном выше примере.</span><span class="sxs-lookup"><span data-stu-id="20cbd-233">Change configuration of owned type relationships to use the new API surface as shown in the example above.</span></span>

## <a name="the-foreign-key-property-convention-no-longer-matches-same-name-as-the-principal-property"></a><span data-ttu-id="20cbd-234">Соглашение для свойства внешнего ключа больше не совпадает со свойством субъекта по имени</span><span class="sxs-lookup"><span data-stu-id="20cbd-234">The foreign key property convention no longer matches same name as the principal property</span></span>

[<span data-ttu-id="20cbd-235">Отслеживание вопроса 13274</span><span class="sxs-lookup"><span data-stu-id="20cbd-235">Tracking Issue #13274</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13274)

<span data-ttu-id="20cbd-236">Это изменение было внесено в предварительную версию 3 EF Core 3.0.</span><span class="sxs-lookup"><span data-stu-id="20cbd-236">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="20cbd-237">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="20cbd-237">**Old behavior**</span></span>

<span data-ttu-id="20cbd-238">Рассмотрим следующую модель:</span><span class="sxs-lookup"><span data-stu-id="20cbd-238">Consider the following model:</span></span>
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
<span data-ttu-id="20cbd-239">До выхода EF Core 3.0 свойство `CustomerId` использовалось для внешнего ключа по соглашению.</span><span class="sxs-lookup"><span data-stu-id="20cbd-239">Before EF Core 3.0, the `CustomerId` property would be used for the foreign key by convention.</span></span>
<span data-ttu-id="20cbd-240">Однако если `Order` является принадлежащим типом, это также делает `CustomerId` первичным ключом, что обычно не соответствует ожидаемому результату.</span><span class="sxs-lookup"><span data-stu-id="20cbd-240">However, if `Order` is an owned type, then this would also make `CustomerId` the primary key and this isn't usually the expectation.</span></span>

<span data-ttu-id="20cbd-241">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="20cbd-241">**New behavior**</span></span>

<span data-ttu-id="20cbd-242">Начиная с версии 3.0 EF Core не будет пытаться использовать свойства для внешних ключей по соглашению, если они имеют такое же имя, что и свойство субъекта.</span><span class="sxs-lookup"><span data-stu-id="20cbd-242">Starting with 3.0, EF Core won't try to use properties for foreign keys by convention if they have the same name as the principal property.</span></span>
<span data-ttu-id="20cbd-243">Шаблоны имени типа субъекта, сцепленного с именем свойства субъекта, и имени навигации, сцепленного с именем свойства субъекта, по-прежнему совпадают.</span><span class="sxs-lookup"><span data-stu-id="20cbd-243">Principal type name concatenated with principal property name, and navigation name concatenated with principal property name patterns are still matched.</span></span>
<span data-ttu-id="20cbd-244">Например:</span><span class="sxs-lookup"><span data-stu-id="20cbd-244">For example:</span></span>

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

<span data-ttu-id="20cbd-245">**Причина**</span><span class="sxs-lookup"><span data-stu-id="20cbd-245">**Why**</span></span>

<span data-ttu-id="20cbd-246">Это изменение было внесено во избежание ошибочного определения свойств первичного ключа для принадлежащего типа.</span><span class="sxs-lookup"><span data-stu-id="20cbd-246">This change was made to avoid erroneously defining a primary key property on the owned type.</span></span>

<span data-ttu-id="20cbd-247">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="20cbd-247">**Mitigations**</span></span>

<span data-ttu-id="20cbd-248">Если свойство должно быть внешним ключом и, следовательно, частью первичного ключа, следует явно указать это при его настройке.</span><span class="sxs-lookup"><span data-stu-id="20cbd-248">If the property was intended to be the foreign key, and hence part of the primary key, then explicitly configure it as such.</span></span>

## <a name="each-property-uses-independent-in-memory-integer-key-generation"></a><span data-ttu-id="20cbd-249">Каждое свойство использует создание независимых целочисленных ключей в памяти</span><span class="sxs-lookup"><span data-stu-id="20cbd-249">Each property uses independent in-memory integer key generation</span></span>

[<span data-ttu-id="20cbd-250">Отслеживание вопроса 6872</span><span class="sxs-lookup"><span data-stu-id="20cbd-250">Tracking Issue #6872</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/6872)

<span data-ttu-id="20cbd-251">Это изменение будет внесено в предварительную версию 4 EF Core 3.0.</span><span class="sxs-lookup"><span data-stu-id="20cbd-251">This change will be introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="20cbd-252">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="20cbd-252">**Old behavior**</span></span>

<span data-ttu-id="20cbd-253">До выхода EF Core 3.0 для всех свойств целочисленных ключей в памяти использовался один общий генератор значений.</span><span class="sxs-lookup"><span data-stu-id="20cbd-253">Before EF Core 3.0, one shared value generator was used for all in-memory integer key properties.</span></span>

<span data-ttu-id="20cbd-254">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="20cbd-254">**New behavior**</span></span>

<span data-ttu-id="20cbd-255">Начиная с EF Core 3.0 при использовании выполняющейся в памяти базы данных каждое свойство целочисленного ключа получает свой собственный генератор значений.</span><span class="sxs-lookup"><span data-stu-id="20cbd-255">Starting with EF Core 3.0, each integer key property gets its own value generator when using the in-memory database.</span></span>
<span data-ttu-id="20cbd-256">Кроме того, при удалении базы данных формирование ключа сбрасывается для всех таблиц.</span><span class="sxs-lookup"><span data-stu-id="20cbd-256">Also, if the database is deleted, then key generation is reset for all tables.</span></span>

<span data-ttu-id="20cbd-257">**Причина**</span><span class="sxs-lookup"><span data-stu-id="20cbd-257">**Why**</span></span>

<span data-ttu-id="20cbd-258">Это изменение было внесено, чтобы лучше согласовать формирование ключей в памяти с реальным процессом в базе данных, а также улучшить возможность изоляции тестов друг от друга при использовании выполняющейся в памяти базы данных.</span><span class="sxs-lookup"><span data-stu-id="20cbd-258">This change was made to align in-memory key generation more closely to real database key generation and to improve the ability to isolate tests from each other when using the in-memory database.</span></span>

<span data-ttu-id="20cbd-259">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="20cbd-259">**Mitigations**</span></span>

<span data-ttu-id="20cbd-260">Это изменение может нарушить работу приложения, полагающегося на задание определенных значений ключа в памяти.</span><span class="sxs-lookup"><span data-stu-id="20cbd-260">This can break an application that is relying on specific in-memory key values to be set.</span></span>
<span data-ttu-id="20cbd-261">Рекомендуется не полагаться на конкретные значения ключей либо реализовать новое поведение.</span><span class="sxs-lookup"><span data-stu-id="20cbd-261">Consider instead not relying on specific key values, or updating to match the new behavior.</span></span>

## <a name="backing-fields-are-used-by-default"></a><span data-ttu-id="20cbd-262">По умолчанию используются резервные поля</span><span class="sxs-lookup"><span data-stu-id="20cbd-262">Backing fields are used by default</span></span>

[<span data-ttu-id="20cbd-263">Отслеживание вопроса 12430</span><span class="sxs-lookup"><span data-stu-id="20cbd-263">Tracking Issue #12430</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12430)

<span data-ttu-id="20cbd-264">Это изменение было внесено в предварительную версию 2 EF Core 3.0.</span><span class="sxs-lookup"><span data-stu-id="20cbd-264">This change was introduced in EF Core 3.0-preview 2.</span></span>

<span data-ttu-id="20cbd-265">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="20cbd-265">**Old behavior**</span></span>

<span data-ttu-id="20cbd-266">До выхода версии 3.0, даже если резервное поле для свойства было известно, система EF Core все равно по умолчанию считывала и записывала значение свойства с помощью методов получения и задания свойства.</span><span class="sxs-lookup"><span data-stu-id="20cbd-266">Before 3.0, even if the backing field for a property was known, EF Core would still by default read and write the property value using the property getter and setter methods.</span></span>
<span data-ttu-id="20cbd-267">Исключением из этого было выполнение запроса, где известное резервное поле устанавливается напрямую.</span><span class="sxs-lookup"><span data-stu-id="20cbd-267">The exception to this was query execution, where the backing field would be set directly if known.</span></span>

<span data-ttu-id="20cbd-268">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="20cbd-268">**New behavior**</span></span>

<span data-ttu-id="20cbd-269">Начиная с EF Core 3.0, если резервное поле для свойства известно, то чтение и запись этого свойства всегда осуществляются с его помощью.</span><span class="sxs-lookup"><span data-stu-id="20cbd-269">Starting with EF Core 3.0, if the backing field for a property is known, then will always read and write that property using the backing field.</span></span>
<span data-ttu-id="20cbd-270">Это может нарушить работу приложения, если оно полагается на дополнительное поведение, закодированное в методы получения и задания.</span><span class="sxs-lookup"><span data-stu-id="20cbd-270">This could cause an application break if the application is relying on additional behavior coded into the getter or setter methods.</span></span>

<span data-ttu-id="20cbd-271">**Причина**</span><span class="sxs-lookup"><span data-stu-id="20cbd-271">**Why**</span></span>

<span data-ttu-id="20cbd-272">Это изменение было внесено, чтобы не позволить EF Core ошибочно активировать бизнес-логику по умолчанию при выполнении операций базы данных, затрагивающих сущности.</span><span class="sxs-lookup"><span data-stu-id="20cbd-272">This change was made to prevent EF Core from erroneously triggering business logic by default when performing database operations involving the entities.</span></span>

<span data-ttu-id="20cbd-273">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="20cbd-273">**Mitigations**</span></span>

<span data-ttu-id="20cbd-274">Поведение, предшествовавшее выходу версии 3.0, можно восстановить, настроив режим доступа к свойству в текучем API modelBuilder.</span><span class="sxs-lookup"><span data-stu-id="20cbd-274">The pre-3.0 behavior can be restored through configuration of the property access mode in the modelBuilder fluent API.</span></span>
<span data-ttu-id="20cbd-275">Например:</span><span class="sxs-lookup"><span data-stu-id="20cbd-275">For example:</span></span>

```C#
modelBuilder.UsePropertyAccessMode(PropertyAccessMode.PreferFieldDuringConstruction);
```

## <a name="throw-if-multiple-compatible-backing-fields-are-found"></a><span data-ttu-id="20cbd-276">Исключение при обнаружении нескольких совместимых резервных полей</span><span class="sxs-lookup"><span data-stu-id="20cbd-276">Throw if multiple compatible backing fields are found</span></span>

[<span data-ttu-id="20cbd-277">Отслеживание вопроса 12523</span><span class="sxs-lookup"><span data-stu-id="20cbd-277">Tracking Issue #12523</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12523)

<span data-ttu-id="20cbd-278">Это изменение будет внесено в предварительную версию 4 EF Core 3.0.</span><span class="sxs-lookup"><span data-stu-id="20cbd-278">This change will be introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="20cbd-279">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="20cbd-279">**Old behavior**</span></span>

<span data-ttu-id="20cbd-280">До выхода EF Core 3.0, если правила, обеспечивающие поиск резервного поля для свойства, находили несколько полей, выбиралось одно из них на основе определенной очередности.</span><span class="sxs-lookup"><span data-stu-id="20cbd-280">Before EF Core 3.0, if multiple fields matched the rules for finding the backing field of a property, then one field would be chosen based on some precedence order.</span></span>
<span data-ttu-id="20cbd-281">В неоднозначных случаях это могло привести к использованию неправильного поля.</span><span class="sxs-lookup"><span data-stu-id="20cbd-281">This could cause the wrong field to be used in ambiguous cases.</span></span>

<span data-ttu-id="20cbd-282">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="20cbd-282">**New behavior**</span></span>

<span data-ttu-id="20cbd-283">Начиная с EF Core 3.0 при обнаружении нескольких полей для одного свойства возникает исключение.</span><span class="sxs-lookup"><span data-stu-id="20cbd-283">Starting with EF Core 3.0, if multiple fields are matched to the same property, then an exception is thrown.</span></span>

<span data-ttu-id="20cbd-284">**Причина**</span><span class="sxs-lookup"><span data-stu-id="20cbd-284">**Why**</span></span>

<span data-ttu-id="20cbd-285">Это изменение было внесено во избежание автоматического использования одного поля вместо другого, когда правильным может быть только одно из них.</span><span class="sxs-lookup"><span data-stu-id="20cbd-285">This change was made to avoid silently using one field over another when only one can be correct.</span></span>

<span data-ttu-id="20cbd-286">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="20cbd-286">**Mitigations**</span></span>

<span data-ttu-id="20cbd-287">Для свойств с неоднозначными резервными полями используемое поле должно быть задано явным образом.</span><span class="sxs-lookup"><span data-stu-id="20cbd-287">Properties with ambiguous backing fields must have the field to use specified explicitly.</span></span>
<span data-ttu-id="20cbd-288">Например, с помощью текучего API:</span><span class="sxs-lookup"><span data-stu-id="20cbd-288">For example, using the fluent API:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .HasField("_id");
```

## <a name="adddbcontextadddbcontextpool-no-longer-call-addlogging-and-addmemorycache"></a><span data-ttu-id="20cbd-289">AddDbContext/AddDbContextPool больше не вызывает метод AddLogging и AddMemoryCache</span><span class="sxs-lookup"><span data-stu-id="20cbd-289">AddDbContext/AddDbContextPool no longer call AddLogging and AddMemoryCache</span></span>

[<span data-ttu-id="20cbd-290">Отслеживание вопроса № 14756</span><span class="sxs-lookup"><span data-stu-id="20cbd-290">Tracking Issue #14756</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14756)

<span data-ttu-id="20cbd-291">Это изменение будет внесено в предварительную версию 4 EF Core 3.0.</span><span class="sxs-lookup"><span data-stu-id="20cbd-291">This change will be introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="20cbd-292">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="20cbd-292">**Old behavior**</span></span>

<span data-ttu-id="20cbd-293">До EF Core 3.0 вызов метода `AddDbContext` или `AddDbContextPool` также приводил к регистрации служб ведения журналов и кэширования памяти с внедрением зависимостей с помощью вызовов к методам [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) и [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span><span class="sxs-lookup"><span data-stu-id="20cbd-293">Before EF Core 3.0, calling `AddDbContext` or `AddDbContextPool` would also register logging and memory caching services with D.I through calls to [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) and [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span></span>

<span data-ttu-id="20cbd-294">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="20cbd-294">**New behavior**</span></span>

<span data-ttu-id="20cbd-295">Начиная с версии EF Core 3.0 методы `AddDbContext` и `AddDbContextPool` больше не регистрируют такие службы с внедрением зависимостей.</span><span class="sxs-lookup"><span data-stu-id="20cbd-295">Starting with EF Core 3.0, `AddDbContext` and `AddDbContextPool` will no longer register these services with Dependency Injection (DI).</span></span>

<span data-ttu-id="20cbd-296">**Причина**</span><span class="sxs-lookup"><span data-stu-id="20cbd-296">**Why**</span></span>

<span data-ttu-id="20cbd-297">EF Core 3.0 не требует, чтобы такие службы присутствовали в контейнере внедрения зависимостей приложения.</span><span class="sxs-lookup"><span data-stu-id="20cbd-297">EF Core 3.0 does not require that these services are in the application's DI cotainer.</span></span> <span data-ttu-id="20cbd-298">Но если интерфейс `ILoggerFactory` зарегистрирован в контейнере внедрения зависимостей приложения, он будет по-прежнему использоваться EF Core.</span><span class="sxs-lookup"><span data-stu-id="20cbd-298">However, if `ILoggerFactory` is registered in the application's DI container, then it will still be used by EF Core.</span></span>

<span data-ttu-id="20cbd-299">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="20cbd-299">**Mitigations**</span></span>

<span data-ttu-id="20cbd-300">Если для работы вашего приложения нужны такие службы, явно зарегистрируйте их в контейнере внедрения зависимостей с помощью метода [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) или [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span><span class="sxs-lookup"><span data-stu-id="20cbd-300">If your application needs these services, then register them explicitly with the DI container using  [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) or [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span></span>

## <a name="dbcontextentry-now-performs-a-local-detectchanges"></a><span data-ttu-id="20cbd-301">DbContext.Entry теперь выполняет локальную процедуру DetectChanges</span><span class="sxs-lookup"><span data-stu-id="20cbd-301">DbContext.Entry now performs a local DetectChanges</span></span>

[<span data-ttu-id="20cbd-302">Отслеживание вопроса 13552</span><span class="sxs-lookup"><span data-stu-id="20cbd-302">Tracking Issue #13552</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13552)

<span data-ttu-id="20cbd-303">Это изменение было внесено в предварительную версию 3 EF Core 3.0.</span><span class="sxs-lookup"><span data-stu-id="20cbd-303">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="20cbd-304">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="20cbd-304">**Old behavior**</span></span>

<span data-ttu-id="20cbd-305">До выхода EF Core 3.0 вызов `DbContext.Entry` приводил к обнаружению изменений для всех отслеживаемых сущностей.</span><span class="sxs-lookup"><span data-stu-id="20cbd-305">Before EF Core 3.0, calling `DbContext.Entry` would cause changes to be detected for all tracked entities.</span></span>
<span data-ttu-id="20cbd-306">Это гарантировало, что состояние, представленное в `EntityEntry`, было актуальным.</span><span class="sxs-lookup"><span data-stu-id="20cbd-306">This ensured that the state exposed in the `EntityEntry` was up-to-date.</span></span>

<span data-ttu-id="20cbd-307">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="20cbd-307">**New behavior**</span></span>

<span data-ttu-id="20cbd-308">Начиная с EF Core 3.0 при вызове `DbContext.Entry` будет предприниматься попытка обнаружить изменения только в заданной сущности и всех связанных с ней отслеживаемых сущностях субъектов.</span><span class="sxs-lookup"><span data-stu-id="20cbd-308">Starting with EF Core 3.0, calling `DbContext.Entry` will now only attempt to detect changes in the given entity and any tracked principal entities related to it.</span></span>
<span data-ttu-id="20cbd-309">Это означает, что изменения в другом месте могут не обнаруживаться при вызове этого метода, что может негативно повлиять на состояние приложения.</span><span class="sxs-lookup"><span data-stu-id="20cbd-309">This means that changes elsewhere may not have been detected by calling this method, which could have implications on application state.</span></span>

<span data-ttu-id="20cbd-310">Обратите внимание, что если для `ChangeTracker.AutoDetectChangesEnabled` задано значение `false`, то будет отключено даже такое локальное обнаружение изменений.</span><span class="sxs-lookup"><span data-stu-id="20cbd-310">Note that if `ChangeTracker.AutoDetectChangesEnabled` is set to `false` then even this local change detection will be disabled.</span></span>

<span data-ttu-id="20cbd-311">Другие методы, вызывающие обнаружение изменений, например `ChangeTracker.Entries` и `SaveChanges`, по-прежнему подразумевают полную процедуру `DetectChanges` для всех отслеживаемых сущностей.</span><span class="sxs-lookup"><span data-stu-id="20cbd-311">Other methods that cause change detection--for example `ChangeTracker.Entries` and `SaveChanges`--still cause a full `DetectChanges` of all tracked entities.</span></span>

<span data-ttu-id="20cbd-312">**Причина**</span><span class="sxs-lookup"><span data-stu-id="20cbd-312">**Why**</span></span>

<span data-ttu-id="20cbd-313">Это изменение было внесено для повышения производительности по умолчанию при использовании `context.Entry`.</span><span class="sxs-lookup"><span data-stu-id="20cbd-313">This change was made to improve the default performance of using `context.Entry`.</span></span>

<span data-ttu-id="20cbd-314">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="20cbd-314">**Mitigations**</span></span>

<span data-ttu-id="20cbd-315">Вызовите `ChgangeTracker.DetectChanges()` явным образом перед вызовом `Entry`, чтобы обеспечить поведение, предшествовавшее выходу версии 3.0.</span><span class="sxs-lookup"><span data-stu-id="20cbd-315">Call `ChgangeTracker.DetectChanges()` explicitly before calling `Entry` to ensure the pre-3.0 behavior.</span></span>

## <a name="string-and-byte-array-keys-are-not-client-generated-by-default"></a><span data-ttu-id="20cbd-316">Ключи массива строк и байтов не формируются клиентом по умолчанию</span><span class="sxs-lookup"><span data-stu-id="20cbd-316">String and byte array keys are not client-generated by default</span></span>

[<span data-ttu-id="20cbd-317">Отслеживание вопроса 14617</span><span class="sxs-lookup"><span data-stu-id="20cbd-317">Tracking Issue #14617</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14617)

<span data-ttu-id="20cbd-318">Это изменение будет внесено в предварительную версию 4 EF Core 3.0.</span><span class="sxs-lookup"><span data-stu-id="20cbd-318">This change will be introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="20cbd-319">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="20cbd-319">**Old behavior**</span></span>

<span data-ttu-id="20cbd-320">До выхода EF Core 3.0 свойства ключей `string` и `byte[]` можно было использовать без явного указания значения, отличного от NULL.</span><span class="sxs-lookup"><span data-stu-id="20cbd-320">Before EF Core 3.0, `string` and `byte[]` key properties could be used without explicitly setting a non-null value.</span></span>
<span data-ttu-id="20cbd-321">В этом случае значение ключа создается на клиенте в виде GUID, сериализованного до байтов для `byte[]`.</span><span class="sxs-lookup"><span data-stu-id="20cbd-321">In such a case, the key value would be generated on the client as a GUID, serialized to bytes for `byte[]`.</span></span>

<span data-ttu-id="20cbd-322">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="20cbd-322">**New behavior**</span></span>

<span data-ttu-id="20cbd-323">Начиная с EF Core 3.0 возникает исключение, указывающее на то, что значение ключа не задано.</span><span class="sxs-lookup"><span data-stu-id="20cbd-323">Starting with EF Core 3.0 an exception will be thrown indicating that no key value has been set.</span></span>

<span data-ttu-id="20cbd-324">**Причина**</span><span class="sxs-lookup"><span data-stu-id="20cbd-324">**Why**</span></span>

<span data-ttu-id="20cbd-325">Это изменение было внесено, так как формируемые клиентом значения `string`/`byte[]` обычно бесполезны, а поведение по умолчанию затрудняло типовое планирование использования этих значений ключей.</span><span class="sxs-lookup"><span data-stu-id="20cbd-325">This change was made because client-generated `string`/`byte[]` values generally aren't useful, and the default behavior made it hard to reason about generated key values in a common way.</span></span>

<span data-ttu-id="20cbd-326">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="20cbd-326">**Mitigations**</span></span>

<span data-ttu-id="20cbd-327">Поведение, предшествовавшее выходу версии 3.0, можно получить, явно указав, что свойства ключей должны использовать формируемые значения, если не задано никакое другое значение, отличное от NULL.</span><span class="sxs-lookup"><span data-stu-id="20cbd-327">The pre-3.0 behavior can be obtained by explicitly specifying that the key properties should use generated values if no other non-null value is set.</span></span>
<span data-ttu-id="20cbd-328">Например, с помощью текучего API:</span><span class="sxs-lookup"><span data-stu-id="20cbd-328">For example, with the fluent API:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .ValueGeneratedOnAdd();
```

<span data-ttu-id="20cbd-329">Или с помощью заметок к данным:</span><span class="sxs-lookup"><span data-stu-id="20cbd-329">Or with data annotations:</span></span>

```C#
[DatabaseGenerated(DatabaseGeneratedOption.Identity)]
public string Id { get; set; }
```

## <a name="iloggerfactory-is-now-a-scoped-service"></a><span data-ttu-id="20cbd-330">ILoggerFactory теперь является службой с ограниченной областью действия</span><span class="sxs-lookup"><span data-stu-id="20cbd-330">ILoggerFactory is now a scoped service</span></span>

[<span data-ttu-id="20cbd-331">Отслеживание вопроса 14698</span><span class="sxs-lookup"><span data-stu-id="20cbd-331">Tracking Issue #14698</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14698)

<span data-ttu-id="20cbd-332">Это изменение было внесено в предварительную версию 3 EF Core 3.0.</span><span class="sxs-lookup"><span data-stu-id="20cbd-332">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="20cbd-333">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="20cbd-333">**Old behavior**</span></span>

<span data-ttu-id="20cbd-334">До выхода EF Core 3.0 `ILoggerFactory` регистрировалась в качестве отдельной службы.</span><span class="sxs-lookup"><span data-stu-id="20cbd-334">Before EF Core 3.0, `ILoggerFactory` was registered as a singleton service.</span></span>

<span data-ttu-id="20cbd-335">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="20cbd-335">**New behavior**</span></span>

<span data-ttu-id="20cbd-336">Начиная с EF Core 3.0 `ILoggerFactory` регистрируется в качестве службы с ограниченной областью действия.</span><span class="sxs-lookup"><span data-stu-id="20cbd-336">Starting with EF Core 3.0, `ILoggerFactory` is now registered as scoped.</span></span>

<span data-ttu-id="20cbd-337">**Причина**</span><span class="sxs-lookup"><span data-stu-id="20cbd-337">**Why**</span></span>

<span data-ttu-id="20cbd-338">Это изменение было внесено, чтобы разрешить сопоставление средства ведения журнала с экземпляром `DbContext`, что обеспечивает другие функции и устраняет некоторые патологические варианты поведения, такие как взрывной рост внутренних поставщиков служб.</span><span class="sxs-lookup"><span data-stu-id="20cbd-338">This change was made to allow association of a logger with a `DbContext` instance, which enables other functionality and removes some cases of pathological behavior such as an explosion of internal service providers.</span></span>

<span data-ttu-id="20cbd-339">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="20cbd-339">**Mitigations**</span></span>

<span data-ttu-id="20cbd-340">Это изменение не должно затрагивать код приложения, если только он не регистрирует и использует настраиваемые службы во внутреннем поставщике служб EF Core.</span><span class="sxs-lookup"><span data-stu-id="20cbd-340">This change should not impact application code unless it is registering and using custom services on the EF Core internal service provider.</span></span>
<span data-ttu-id="20cbd-341">Это встречается довольно редко.</span><span class="sxs-lookup"><span data-stu-id="20cbd-341">This isn't common.</span></span>
<span data-ttu-id="20cbd-342">В таких случаях большинство возможностей продолжают работать, но потребуется изменить все отдельные службы, которые зависели от `ILoggerFactory`, чтобы получить `ILoggerFactory` другим способом.</span><span class="sxs-lookup"><span data-stu-id="20cbd-342">In these cases, most things will still work, but any singleton service that was depending on `ILoggerFactory` will need to be changed to obtain the `ILoggerFactory` in a different way.</span></span>

<span data-ttu-id="20cbd-343">При возникновении подобных ситуаций зарегистрируйте проблему в [средстве отслеживания вопросов EF Core на сайте GitHub](https://github.com/aspnet/EntityFrameworkCore/issues) и сообщите нам, как вы используете `ILoggerFactory`, чтобы мы могли лучше понять, как избежать подобных ошибок в будущем.</span><span class="sxs-lookup"><span data-stu-id="20cbd-343">If you run into situations like this, please file an issue at on the [EF Core GitHub issue tracker](https://github.com/aspnet/EntityFrameworkCore/issues) to let us know how you are using `ILoggerFactory` such that we can better understand how not to break this again in the future.</span></span>

## <a name="idbcontextoptionsextensionwithdebuginfo-merged-into-idbcontextoptionsextension"></a><span data-ttu-id="20cbd-344">IDbContextOptionsExtensionWithDebugInfo объединен с IDbContextOptionsExtension</span><span class="sxs-lookup"><span data-stu-id="20cbd-344">IDbContextOptionsExtensionWithDebugInfo merged into IDbContextOptionsExtension</span></span>

[<span data-ttu-id="20cbd-345">Отслеживание вопроса 13552</span><span class="sxs-lookup"><span data-stu-id="20cbd-345">Tracking Issue #13552</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13552)

<span data-ttu-id="20cbd-346">Это изменение было внесено в предварительную версию 3 EF Core 3.0.</span><span class="sxs-lookup"><span data-stu-id="20cbd-346">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="20cbd-347">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="20cbd-347">**Old behavior**</span></span>

<span data-ttu-id="20cbd-348">`IDbContextOptionsExtensionWithDebugInfo` был дополнительным необязательным интерфейсом, расширяемым из `IDbContextOptionsExtension`, для предотвращения критического изменения в интерфейсе в рамках выпуска версии 2.x.</span><span class="sxs-lookup"><span data-stu-id="20cbd-348">`IDbContextOptionsExtensionWithDebugInfo` was an additional optional interface extended from `IDbContextOptionsExtension` to avoid making a breaking change to the interface during the 2.x release cycle.</span></span>

<span data-ttu-id="20cbd-349">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="20cbd-349">**New behavior**</span></span>

<span data-ttu-id="20cbd-350">Теперь эти интерфейсы объединяются в `IDbContextOptionsExtension`.</span><span class="sxs-lookup"><span data-stu-id="20cbd-350">The interfaces are now merged together into `IDbContextOptionsExtension`.</span></span>

<span data-ttu-id="20cbd-351">**Причина**</span><span class="sxs-lookup"><span data-stu-id="20cbd-351">**Why**</span></span>

<span data-ttu-id="20cbd-352">Это изменение было внесено, так как интерфейсы представляют собой единое целое.</span><span class="sxs-lookup"><span data-stu-id="20cbd-352">This change was made because the interfaces are conceptually one.</span></span>

<span data-ttu-id="20cbd-353">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="20cbd-353">**Mitigations**</span></span>

<span data-ttu-id="20cbd-354">Нужно изменить все реализации `IDbContextOptionsExtension`, чтобы обеспечить поддержку нового члена.</span><span class="sxs-lookup"><span data-stu-id="20cbd-354">Any implementations of `IDbContextOptionsExtension` will need to be updated to support the new member.</span></span>

## <a name="lazy-loading-proxies-no-longer-assume-navigation-properties-are-fully-loaded"></a><span data-ttu-id="20cbd-355">Прокси с отложенной загрузкой больше не предполагают полную загрузку свойств навигации</span><span class="sxs-lookup"><span data-stu-id="20cbd-355">Lazy-loading proxies no longer assume navigation properties are fully loaded</span></span>

[<span data-ttu-id="20cbd-356">Отслеживание вопроса 12780</span><span class="sxs-lookup"><span data-stu-id="20cbd-356">Tracking Issue #12780</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12780)

<span data-ttu-id="20cbd-357">Это изменение будет внесено в предварительную версию 4 EF Core 3.0.</span><span class="sxs-lookup"><span data-stu-id="20cbd-357">This change will be introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="20cbd-358">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="20cbd-358">**Old behavior**</span></span>

<span data-ttu-id="20cbd-359">До выхода EF Core 3.0 после удаления `DbContext` не существовало способа узнать, было ли указанное свойство навигации для сущности, полученной из этого контекста, загружено полностью.</span><span class="sxs-lookup"><span data-stu-id="20cbd-359">Before EF Core 3.0, once a `DbContext` was disposed there was no way of knowing if a given navigation property on an entity obtained from that context was fully loaded or not.</span></span>
<span data-ttu-id="20cbd-360">Вместо это прокси предполагали, что навигация по ссылке загружена, если она имеет отличное NULL значение, а навигация по коллекции загружена, если она не является пустой.</span><span class="sxs-lookup"><span data-stu-id="20cbd-360">Proxies would instead assume that a reference navigation is loaded if it has a non-null value, and that a collection navigation is loaded if it isn't empty.</span></span>
<span data-ttu-id="20cbd-361">В этом случае попытка отложенной загрузки приводила к холостой операции.</span><span class="sxs-lookup"><span data-stu-id="20cbd-361">In these cases, attempting to lazy-load would be a no-op.</span></span>

<span data-ttu-id="20cbd-362">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="20cbd-362">**New behavior**</span></span>

<span data-ttu-id="20cbd-363">Начиная с EF Core 3.0 прокси отслеживают, загружено ли свойство навигации.</span><span class="sxs-lookup"><span data-stu-id="20cbd-363">Starting with EF Core 3.0, proxies keep track of whether or not a navigation property is loaded.</span></span>
<span data-ttu-id="20cbd-364">Это означает, что попытка обращения к свойству навигации, загружаемому после удаления контекста, всегда будет давать холостую операцию, даже если загруженное свойство навигации пусто или равно NULL.</span><span class="sxs-lookup"><span data-stu-id="20cbd-364">This means attempting to access a navigation property that is loaded after the context has been disposed will always be a no-op, even when the loaded navigation is empty or null.</span></span>
<span data-ttu-id="20cbd-365">И наоборот, при попытке обращения к незагруженному свойству навигации, когда контекст удален, возникает исключение, даже если это свойство навигации является непустой коллекцией.</span><span class="sxs-lookup"><span data-stu-id="20cbd-365">Conversely, attempting to access a navigation property that isn't loaded will throw an exception if the context is disposed even if the navigation property is a non-empty collection.</span></span>
<span data-ttu-id="20cbd-366">Подобная ситуация указывает на то, что код приложения пытается использовать отложенную загрузку в недопустимое время, и нужно запретить это, изменив приложение.</span><span class="sxs-lookup"><span data-stu-id="20cbd-366">If this situation arises, it means the application code is attempting to use lazy-loading at an invalid time, and the application should be changed to not do this.</span></span>

<span data-ttu-id="20cbd-367">**Причина**</span><span class="sxs-lookup"><span data-stu-id="20cbd-367">**Why**</span></span>

<span data-ttu-id="20cbd-368">Это изменение было внесено, чтобы обеспечить правильное и согласованное поведение при попытке отложенной загрузки для удаленного экземпляра `DbContext`.</span><span class="sxs-lookup"><span data-stu-id="20cbd-368">This change was made to make the behavior consistent and correct when attempting to lazy-load on a disposed `DbContext` instance.</span></span>

<span data-ttu-id="20cbd-369">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="20cbd-369">**Mitigations**</span></span>

<span data-ttu-id="20cbd-370">Обновите код приложения, чтобы не выполнять попытку отложенной загрузки с удаленным контекстом, либо настройте соответствующую холостую операцию, как описано в сообщении об исключении.</span><span class="sxs-lookup"><span data-stu-id="20cbd-370">Update application code to not attempt lazy-loading with a disposed context, or configure this to be a no-op as described in the exception message.</span></span>

## <a name="excessive-creation-of-internal-service-providers-is-now-an-error-by-default"></a><span data-ttu-id="20cbd-371">Чрезмерное создание внутренних поставщиков служб теперь является ошибкой по умолчанию</span><span class="sxs-lookup"><span data-stu-id="20cbd-371">Excessive creation of internal service providers is now an error by default</span></span>

[<span data-ttu-id="20cbd-372">Отслеживание вопроса 10236</span><span class="sxs-lookup"><span data-stu-id="20cbd-372">Tracking Issue #10236</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10236)

<span data-ttu-id="20cbd-373">Это изменение было внесено в предварительную версию 3 EF Core 3.0.</span><span class="sxs-lookup"><span data-stu-id="20cbd-373">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="20cbd-374">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="20cbd-374">**Old behavior**</span></span>

<span data-ttu-id="20cbd-375">До выхода EF Core 3.0 для приложения, создающего чрезмерное количество внутренних поставщиков служб, регистрировалось предупреждение.</span><span class="sxs-lookup"><span data-stu-id="20cbd-375">Before EF Core 3.0, a warning would be logged for an application creating a pathological number of internal service providers.</span></span>

<span data-ttu-id="20cbd-376">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="20cbd-376">**New behavior**</span></span>

<span data-ttu-id="20cbd-377">Начиная с EF Core 3.0 это предупреждение считается ошибкой и возникает исключение.</span><span class="sxs-lookup"><span data-stu-id="20cbd-377">Starting with EF Core 3.0, this warning is now considered and error and an exception is thrown.</span></span> 

<span data-ttu-id="20cbd-378">**Причина**</span><span class="sxs-lookup"><span data-stu-id="20cbd-378">**Why**</span></span>

<span data-ttu-id="20cbd-379">Это изменение было внесено для улучшения кода приложения посредством явного указания на подобный патологический случай.</span><span class="sxs-lookup"><span data-stu-id="20cbd-379">This change was made to drive better application code through exposing this pathological case more explicitly.</span></span>

<span data-ttu-id="20cbd-380">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="20cbd-380">**Mitigations**</span></span>

<span data-ttu-id="20cbd-381">При возникновении этой ошибки правильнее всего выявить первопричину и остановить создание такого большого числа внутренних поставщиков служб.</span><span class="sxs-lookup"><span data-stu-id="20cbd-381">The most appropriate cause of action on encountering this error is to understand the root cause and stop creating so many internal service providers.</span></span>
<span data-ttu-id="20cbd-382">Тем не менее эту ошибку можно преобразовать обратно в предупреждение (или игнорировать) с помощью конфигурации для `DbContextOptionsBuilder`.</span><span class="sxs-lookup"><span data-stu-id="20cbd-382">However, the error can be converted back to a warning (or ignored) via configuration on the `DbContextOptionsBuilder`.</span></span>
<span data-ttu-id="20cbd-383">Например:</span><span class="sxs-lookup"><span data-stu-id="20cbd-383">For example:</span></span>

```C#
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
{
    optionsBuilder
        .ConfigureWarnings(w => w.Log(CoreEventId.ManyServiceProvidersCreatedWarning));
}
```

## <a name="new-behavior-for-hasonehasmany-called-with-a-single-string"></a><span data-ttu-id="20cbd-384">Новое поведение для вызова HasOne/HasMany с одной строкой</span><span class="sxs-lookup"><span data-stu-id="20cbd-384">New behavior for HasOne/HasMany called with a single string</span></span>

[<span data-ttu-id="20cbd-385">Отслеживание вопроса № 9171</span><span class="sxs-lookup"><span data-stu-id="20cbd-385">Tracking Issue #9171</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9171)

<span data-ttu-id="20cbd-386">Это изменение будет внесено в предварительную версию 4 EF Core 3.0.</span><span class="sxs-lookup"><span data-stu-id="20cbd-386">This change will be introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="20cbd-387">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="20cbd-387">**Old behavior**</span></span>

<span data-ttu-id="20cbd-388">До версии EF Core 3.0 интерпретация кода с вызовом `HasOne` или `HasMany` с одной строкой была очень запутанной.</span><span class="sxs-lookup"><span data-stu-id="20cbd-388">Before EF Core 3.0, code calling `HasOne` or `HasMany` with a single string was interpretted in a confusing way.</span></span>
<span data-ttu-id="20cbd-389">Например:</span><span class="sxs-lookup"><span data-stu-id="20cbd-389">For example:</span></span>
```C#
modelBuilder.Entity<Samurai>().HasOne("Entrance").WithOne();
```

<span data-ttu-id="20cbd-390">Складывается впечатление, что код связывает `Samurai` с другим типом сущности с помощью свойства навигации `Entrance`, которое может быть закрытым.</span><span class="sxs-lookup"><span data-stu-id="20cbd-390">The code looks like it is relating `Samurai` to some other entity type using the `Entrance` navigation property, which may be private.</span></span>

<span data-ttu-id="20cbd-391">На самом деле этот код пытается создать отношение с некоторым типом сущности под именем `Entrance`, который не имеет свойства навигации.</span><span class="sxs-lookup"><span data-stu-id="20cbd-391">In reality, this code attempts to create a relationship to some entity type called `Entrance` with no navigation property.</span></span>

<span data-ttu-id="20cbd-392">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="20cbd-392">**New behavior**</span></span>

<span data-ttu-id="20cbd-393">Начиная с EF Core 3.0, указанный выше код выполняет те действия, которые должен был выполнять раньше.</span><span class="sxs-lookup"><span data-stu-id="20cbd-393">Starting with EF Core 3.0, the code above now does what it looked like it should have been doing before.</span></span>

<span data-ttu-id="20cbd-394">**Причина**</span><span class="sxs-lookup"><span data-stu-id="20cbd-394">**Why**</span></span>

<span data-ttu-id="20cbd-395">Старое поведение создавало большую путаницу, особенно при считывании кода конфигурации и при поиске ошибок.</span><span class="sxs-lookup"><span data-stu-id="20cbd-395">The old behavior was very confusing, especially when reading the configuration code and looking for errors.</span></span>

<span data-ttu-id="20cbd-396">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="20cbd-396">**Mitigations**</span></span>

<span data-ttu-id="20cbd-397">Это приведет к нарушению работы только тех приложений, которые явно настраивают отношения с помощью строк для имен типов и не указывают явно свойство навигации.</span><span class="sxs-lookup"><span data-stu-id="20cbd-397">This will only break applications that are explicitly configuring relationships using strings for type names and without specifying the navigation property explicitly.</span></span>
<span data-ttu-id="20cbd-398">Такой подход используется нечасто.</span><span class="sxs-lookup"><span data-stu-id="20cbd-398">This is not common.</span></span>
<span data-ttu-id="20cbd-399">Прежнее поведение можно реализовать с помощью явной передачи значения `null` для имени свойства навигации.</span><span class="sxs-lookup"><span data-stu-id="20cbd-399">The previous behavior can be obtained through explicitly passing `null` for the navigation property name.</span></span>
<span data-ttu-id="20cbd-400">Например:</span><span class="sxs-lookup"><span data-stu-id="20cbd-400">For example:</span></span>

```C#
modelBuilder.Entity<Samurai>().HasOne("Some.Entity.Type.Name", null).WithOne();
```

## <a name="the-relationaltypemapping-annotation-is-now-just-typemapping"></a><span data-ttu-id="20cbd-401">Заметка Relational:TypeMapping теперь является просто TypeMapping</span><span class="sxs-lookup"><span data-stu-id="20cbd-401">The Relational:TypeMapping annotation is now just TypeMapping</span></span>

[<span data-ttu-id="20cbd-402">Отслеживание вопроса 9913</span><span class="sxs-lookup"><span data-stu-id="20cbd-402">Tracking Issue #9913</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9913)

<span data-ttu-id="20cbd-403">Это изменение было внесено в предварительную версию 2 EF Core 3.0.</span><span class="sxs-lookup"><span data-stu-id="20cbd-403">This change was introduced in EF Core 3.0-preview 2.</span></span>

<span data-ttu-id="20cbd-404">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="20cbd-404">**Old behavior**</span></span>

<span data-ttu-id="20cbd-405">Для заметок сопоставлений типов использовалось имя "Relational:TypeMapping".</span><span class="sxs-lookup"><span data-stu-id="20cbd-405">The annotation name for type mapping annotations was "Relational:TypeMapping".</span></span>

<span data-ttu-id="20cbd-406">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="20cbd-406">**New behavior**</span></span>

<span data-ttu-id="20cbd-407">Теперь для заметок сопоставлений типов используется имя "TypeMapping".</span><span class="sxs-lookup"><span data-stu-id="20cbd-407">The annotation name for type mapping annotations is now "TypeMapping".</span></span>

<span data-ttu-id="20cbd-408">**Причина**</span><span class="sxs-lookup"><span data-stu-id="20cbd-408">**Why**</span></span>

<span data-ttu-id="20cbd-409">Сопоставления типов теперь используются не только для поставщиков реляционных баз данных.</span><span class="sxs-lookup"><span data-stu-id="20cbd-409">Type mappings are now used for more than just relational database providers.</span></span>

<span data-ttu-id="20cbd-410">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="20cbd-410">**Mitigations**</span></span>

<span data-ttu-id="20cbd-411">Это изменение нарушит работу только тех приложений, которые обращаются к сопоставлению типов непосредственно по заметке, что встречается довольно редко.</span><span class="sxs-lookup"><span data-stu-id="20cbd-411">This will only break applications that access the type mapping directly as an annotation, which isn't common.</span></span>
<span data-ttu-id="20cbd-412">Наиболее подходящий способ для устранения этой проблемы заключается в использовании поверхности API для доступа к сопоставлениям типов вместо прямого использования заметки.</span><span class="sxs-lookup"><span data-stu-id="20cbd-412">The most appropriate action to fix is to use API surface to access type mappings rather than using the annotation directly.</span></span>

## <a name="totable-on-a-derived-type-throws-an-exception"></a><span data-ttu-id="20cbd-413">ToTable для производного типа выдает исключение</span><span class="sxs-lookup"><span data-stu-id="20cbd-413">ToTable on a derived type throws an exception</span></span> 

[<span data-ttu-id="20cbd-414">Отслеживание вопроса 11811</span><span class="sxs-lookup"><span data-stu-id="20cbd-414">Tracking Issue #11811</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/11811)

<span data-ttu-id="20cbd-415">Это изменение было внесено в предварительную версию 3 EF Core 3.0.</span><span class="sxs-lookup"><span data-stu-id="20cbd-415">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="20cbd-416">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="20cbd-416">**Old behavior**</span></span>

<span data-ttu-id="20cbd-417">До выхода EF Core 3.0 вызов `ToTable()` для производного типа игнорировался, так как единственной стратегией сопоставления наследования был метод "одна таблица на иерархию", где такая операция недействительна.</span><span class="sxs-lookup"><span data-stu-id="20cbd-417">Before EF Core 3.0, `ToTable()` called on a derived type would be ignored since only inheritance mapping strategy was TPH where this isn't valid.</span></span> 

<span data-ttu-id="20cbd-418">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="20cbd-418">**New behavior**</span></span>

<span data-ttu-id="20cbd-419">Начиная с EF Core 3.0 и в рамках подготовки для добавления поддержки методов "одна таблица на тип" и "одна таблица на каждый отдельный тип/класс" в последующем выпуске вызов `ToTable()` для производного типа теперь будет выдавать исключение, чтобы избежать непредвиденного изменения сопоставления в будущем.</span><span class="sxs-lookup"><span data-stu-id="20cbd-419">Starting with EF Core 3.0 and in preparation for adding TPT and TPC support in a later release, `ToTable()` called on a derived type will now throw an exception to avoid an unexpected mapping change in the future.</span></span>

<span data-ttu-id="20cbd-420">**Причина**</span><span class="sxs-lookup"><span data-stu-id="20cbd-420">**Why**</span></span>

<span data-ttu-id="20cbd-421">Сейчас сопоставлять производный тип с другой таблицей запрещено.</span><span class="sxs-lookup"><span data-stu-id="20cbd-421">Currently it isn't valid to map a derived type to a different table.</span></span>
<span data-ttu-id="20cbd-422">Это изменение позволяет избежать нарушения работы в будущем, когда подобная операция будет разрешена.</span><span class="sxs-lookup"><span data-stu-id="20cbd-422">This change avoids breaking in the future when it becomes a valid thing to do.</span></span>

<span data-ttu-id="20cbd-423">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="20cbd-423">**Mitigations**</span></span>

<span data-ttu-id="20cbd-424">Удалите все попытки сопоставить производные типы с другими таблицами.</span><span class="sxs-lookup"><span data-stu-id="20cbd-424">Remove any attempts to map derived types to other tables.</span></span>

## <a name="forsqlserverhasindex-replaced-with-hasindex"></a><span data-ttu-id="20cbd-425">ForSqlServerHasIndex заменен на HasIndex</span><span class="sxs-lookup"><span data-stu-id="20cbd-425">ForSqlServerHasIndex replaced with HasIndex</span></span> 

[<span data-ttu-id="20cbd-426">Отслеживание вопроса 12366</span><span class="sxs-lookup"><span data-stu-id="20cbd-426">Tracking Issue #12366</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12366)

<span data-ttu-id="20cbd-427">Это изменение было внесено в предварительную версию 3 EF Core 3.0.</span><span class="sxs-lookup"><span data-stu-id="20cbd-427">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="20cbd-428">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="20cbd-428">**Old behavior**</span></span>

<span data-ttu-id="20cbd-429">До выхода EF Core 3.0 `ForSqlServerHasIndex().ForSqlServerInclude()` предоставлял способ настройки столбцов, используемых с `INCLUDE`.</span><span class="sxs-lookup"><span data-stu-id="20cbd-429">Before EF Core 3.0, `ForSqlServerHasIndex().ForSqlServerInclude()` provided a way to configure columns used with `INCLUDE`.</span></span>

<span data-ttu-id="20cbd-430">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="20cbd-430">**New behavior**</span></span>

<span data-ttu-id="20cbd-431">Начиная с EF Core 3.0 использование `Include` для индекса теперь поддерживается на реляционном уровне.</span><span class="sxs-lookup"><span data-stu-id="20cbd-431">Starting with EF Core 3.0, using `Include` on an index is now supported at the relational level.</span></span>
<span data-ttu-id="20cbd-432">Используйте ключевое слово `HasIndex().ForSqlServerInclude()`.</span><span class="sxs-lookup"><span data-stu-id="20cbd-432">Use `HasIndex().ForSqlServerInclude()`.</span></span>

<span data-ttu-id="20cbd-433">**Причина**</span><span class="sxs-lookup"><span data-stu-id="20cbd-433">**Why**</span></span>

<span data-ttu-id="20cbd-434">Это изменение было внесено, чтобы консолидировать API для индексов с `Includes` в одном месте для всех поставщиков баз данных.</span><span class="sxs-lookup"><span data-stu-id="20cbd-434">This change was made to consolidate the API for indexes with `Includes` into one place for all database providers.</span></span>

<span data-ttu-id="20cbd-435">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="20cbd-435">**Mitigations**</span></span>

<span data-ttu-id="20cbd-436">Используйте новый API, как показано выше.</span><span class="sxs-lookup"><span data-stu-id="20cbd-436">Use the new API, as shown above.</span></span>

## <a name="ef-core-no-longer-sends-pragma-for-sqlite-fk-enforcement"></a><span data-ttu-id="20cbd-437">EF Core больше не отправляет pragma для принудительного применения FK SQLite</span><span class="sxs-lookup"><span data-stu-id="20cbd-437">EF Core no longer sends pragma for SQLite FK enforcement</span></span>

[<span data-ttu-id="20cbd-438">Отслеживание вопроса 12151</span><span class="sxs-lookup"><span data-stu-id="20cbd-438">Tracking Issue #12151</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12151)

<span data-ttu-id="20cbd-439">Это изменение было внесено в предварительную версию 3 EF Core 3.0.</span><span class="sxs-lookup"><span data-stu-id="20cbd-439">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="20cbd-440">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="20cbd-440">**Old behavior**</span></span>

<span data-ttu-id="20cbd-441">До выхода версии 3.0 система EF Core отправляла `PRAGMA foreign_keys = 1` при открытии соединения с SQLite.</span><span class="sxs-lookup"><span data-stu-id="20cbd-441">Before EF Core 3.0, EF Core would send `PRAGMA foreign_keys = 1` when a connection to SQLite is opened.</span></span>

<span data-ttu-id="20cbd-442">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="20cbd-442">**New behavior**</span></span>

<span data-ttu-id="20cbd-443">Начиная с версии 3.0 система EF Core больше не отправляет `PRAGMA foreign_keys = 1` при открытии соединения с SQLite.</span><span class="sxs-lookup"><span data-stu-id="20cbd-443">Starting with EF Core 3.0, EF Core no longer sends `PRAGMA foreign_keys = 1` when a connection to SQLite is opened.</span></span>

<span data-ttu-id="20cbd-444">**Причина**</span><span class="sxs-lookup"><span data-stu-id="20cbd-444">**Why**</span></span>

<span data-ttu-id="20cbd-445">Это изменение было внесено, так как EF Core по умолчанию использует `SQLitePCLRaw.bundle_e_sqlite3`, что, в свою очередь, означает, что принудительное применение FK по умолчанию включено и его не требуется включать каждый раз при открытии соединения.</span><span class="sxs-lookup"><span data-stu-id="20cbd-445">This change was made because EF Core uses `SQLitePCLRaw.bundle_e_sqlite3` by default, which in turn means that FK enforcement is switched on by default and doesn't need to be explicitly enabled each time a connection is opened.</span></span>

<span data-ttu-id="20cbd-446">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="20cbd-446">**Mitigations**</span></span>

<span data-ttu-id="20cbd-447">Внешние ключи включены по умолчанию в SQLitePCLRaw.bundle_e_sqlite3, который по умолчанию используется для EF Core.</span><span class="sxs-lookup"><span data-stu-id="20cbd-447">Foreign keys are enabled by default in SQLitePCLRaw.bundle_e_sqlite3, which is used by default for EF Core.</span></span>
<span data-ttu-id="20cbd-448">В других случаях внешние ключи можно включить, указав `Foreign Keys=True` в строке подключения.</span><span class="sxs-lookup"><span data-stu-id="20cbd-448">For other cases, foreign keys can be enabled by specifying `Foreign Keys=True` in your connection string.</span></span>

## <a name="microsoftentityframeworkcoresqlite-now-depends-on-sqlitepclrawbundleesqlite3"></a><span data-ttu-id="20cbd-449">Теперь Microsoft.EntityFrameworkCore.Sqlite зависит от SQLitePCLRaw.bundle_e_sqlite3</span><span class="sxs-lookup"><span data-stu-id="20cbd-449">Microsoft.EntityFrameworkCore.Sqlite now depends on SQLitePCLRaw.bundle_e_sqlite3</span></span>

<span data-ttu-id="20cbd-450">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="20cbd-450">**Old behavior**</span></span>

<span data-ttu-id="20cbd-451">До выхода версии 3.0 система EF Core использовала `SQLitePCLRaw.bundle_green`.</span><span class="sxs-lookup"><span data-stu-id="20cbd-451">Before EF Core 3.0, EF Core used `SQLitePCLRaw.bundle_green`.</span></span>

<span data-ttu-id="20cbd-452">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="20cbd-452">**New behavior**</span></span>

<span data-ttu-id="20cbd-453">Начиная с версии 3.0 система EF Core использует `SQLitePCLRaw.bundle_e_sqlite3`.</span><span class="sxs-lookup"><span data-stu-id="20cbd-453">Starting with EF Core 3.0, EF Core uses `SQLitePCLRaw.bundle_e_sqlite3`.</span></span>

<span data-ttu-id="20cbd-454">**Причина**</span><span class="sxs-lookup"><span data-stu-id="20cbd-454">**Why**</span></span>

<span data-ttu-id="20cbd-455">Это изменение было внесено, чтобы версия SQLite, используемая в iOS, была согласована с другими платформами.</span><span class="sxs-lookup"><span data-stu-id="20cbd-455">This change was made so that the version of SQLite used on iOS consistent with other platforms.</span></span>

<span data-ttu-id="20cbd-456">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="20cbd-456">**Mitigations**</span></span>

<span data-ttu-id="20cbd-457">Чтобы использовать собственную версию SQLite в iOS, настройте `Microsoft.Data.Sqlite` для использования другого пакета `SQLitePCLRaw`.</span><span class="sxs-lookup"><span data-stu-id="20cbd-457">To use the native SQLite version on iOS, configure `Microsoft.Data.Sqlite` to use a different `SQLitePCLRaw` bundle.</span></span>

## <a name="guid-values-are-now-stored-as-text-on-sqlite"></a><span data-ttu-id="20cbd-458">Теперь значения Guid хранятся в SQLite в виде значений типа TEXT</span><span class="sxs-lookup"><span data-stu-id="20cbd-458">Guid values are now stored as TEXT on SQLite</span></span>

[<span data-ttu-id="20cbd-459">Отслеживание вопроса № 15078</span><span class="sxs-lookup"><span data-stu-id="20cbd-459">Tracking Issue #15078</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15078)

<span data-ttu-id="20cbd-460">Это изменение было внесено в предварительную версию 4 EF Core 3.0.</span><span class="sxs-lookup"><span data-stu-id="20cbd-460">This change was introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="20cbd-461">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="20cbd-461">**Old behavior**</span></span>

<span data-ttu-id="20cbd-462">Ранее значения типа Guid хранились в SQLite в виде значений типа BLOB.</span><span class="sxs-lookup"><span data-stu-id="20cbd-462">Guid values were previously sored as BLOB values on SQLite.</span></span>

<span data-ttu-id="20cbd-463">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="20cbd-463">**New behavior**</span></span>

<span data-ttu-id="20cbd-464">Теперь значения типа Guid хранятся в виде значений типа TEXT.</span><span class="sxs-lookup"><span data-stu-id="20cbd-464">Guid values are now sotred as TEXT.</span></span>

<span data-ttu-id="20cbd-465">**Причина**</span><span class="sxs-lookup"><span data-stu-id="20cbd-465">**Why**</span></span>

<span data-ttu-id="20cbd-466">Двоичный формат типа Guid не стандартизирован.</span><span class="sxs-lookup"><span data-stu-id="20cbd-466">The binary format of Guids is not standardized.</span></span> <span data-ttu-id="20cbd-467">Хранение значений в виде значений типа TEXT повышает уровень совместимости базы данных с другими технологиями.</span><span class="sxs-lookup"><span data-stu-id="20cbd-467">Storing the values as TEXT makes the database more compatible with other technologies.</span></span>

<span data-ttu-id="20cbd-468">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="20cbd-468">**Mitigations**</span></span>

<span data-ttu-id="20cbd-469">Существующие базы данных можно перенести в новый формат, выполнив код SQL следующим образом.</span><span class="sxs-lookup"><span data-stu-id="20cbd-469">You can migrate existing databases to the new format by executing SQL like the following.</span></span>

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

<span data-ttu-id="20cbd-470">В EF Core можно продолжить использовать прежнее поведение, настроив преобразователь значений для этих свойств.</span><span class="sxs-lookup"><span data-stu-id="20cbd-470">In EF Core, you could also continue using the previous behavior by configuirng a value converter on these properties.</span></span>

``` csharp
modelBuilder
    .Entity<MyEntity>()
    .Property(e => e.GuidProperty)
    .HasConversion(
        g => g.ToByteArray(),
        b => new Guid(b));
```

<span data-ttu-id="20cbd-471">Microsoft.Data.Sqlite по-прежнему может читать значения Guid из столбцов BLOB и TEXT. Но поскольку изменился формат по умолчанию для параметров и констант, скорее всего, вам потребуется выполнить некоторые действия в большинстве сценариев, где используются значения Guid.</span><span class="sxs-lookup"><span data-stu-id="20cbd-471">Microsoft.Data.Sqlite remains capable of reading Guid values from both BLOB and TEXT columns; however, since the default format for parameters and constants has changed you'll likely need to take action for most scenarios involving Guids.</span></span>

## <a name="char-values-are-now-stored-as-text-on-sqlite"></a><span data-ttu-id="20cbd-472">Теперь значения типа Char хранятся в SQLite в виде значений типа TEXT</span><span class="sxs-lookup"><span data-stu-id="20cbd-472">Char values are now stored as TEXT on SQLite</span></span>

[<span data-ttu-id="20cbd-473">Отслеживание вопроса № 15020</span><span class="sxs-lookup"><span data-stu-id="20cbd-473">Tracking Issue #15020</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/15020)

<span data-ttu-id="20cbd-474">Это изменение было внесено в предварительную версию 4 EF Core 3.0.</span><span class="sxs-lookup"><span data-stu-id="20cbd-474">This change was introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="20cbd-475">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="20cbd-475">**Old behavior**</span></span>

<span data-ttu-id="20cbd-476">Ранее значения типа Char хранились в SQLite в виде значений типа INTEGER.</span><span class="sxs-lookup"><span data-stu-id="20cbd-476">Char values were previously sored as INTEGER values on SQLite.</span></span> <span data-ttu-id="20cbd-477">Например, значение типа Char *A* хранилось как целочисленное значение 65.</span><span class="sxs-lookup"><span data-stu-id="20cbd-477">For example, a char value of *A* was stored as the integer value 65.</span></span>

<span data-ttu-id="20cbd-478">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="20cbd-478">**New behavior**</span></span>

<span data-ttu-id="20cbd-479">Теперь значения типа Char хранятся в виде значений типа TEXT.</span><span class="sxs-lookup"><span data-stu-id="20cbd-479">Char values are now sotred as TEXT.</span></span>

<span data-ttu-id="20cbd-480">**Причина**</span><span class="sxs-lookup"><span data-stu-id="20cbd-480">**Why**</span></span>

<span data-ttu-id="20cbd-481">Хранение значений в виде значений типа TEXT является более естественным вариантом и повышает уровень совместимости базы данных с другими технологиями.</span><span class="sxs-lookup"><span data-stu-id="20cbd-481">Storing the values as TEXT is more natural and makes the database more compatible with other technologies.</span></span>

<span data-ttu-id="20cbd-482">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="20cbd-482">**Mitigations**</span></span>

<span data-ttu-id="20cbd-483">Существующие базы данных можно перенести в новый формат, выполнив код SQL следующим образом.</span><span class="sxs-lookup"><span data-stu-id="20cbd-483">You can migrate existing databases to the new format by executing SQL like the following.</span></span>

``` sql
UPDATE MyTable
SET CharColumn = char(CharColumn)
WHERE typeof(CharColumn) = 'integer';
```

<span data-ttu-id="20cbd-484">В EF Core можно продолжить использовать прежнее поведение, настроив преобразователь значений для этих свойств.</span><span class="sxs-lookup"><span data-stu-id="20cbd-484">In EF Core, you could also continue using the previous behavior by configuirng a value converter on these properties.</span></span>

``` csharp
modelBuilder
    .Entity<MyEntity>()
    .Property(e => e.CharProperty)
    .HasConversion(
        c => (long)c,
        i => (char)i);
```

<span data-ttu-id="20cbd-485">Microsoft.Data.Sqlite также сохраняет возможность чтения значений символов из столбцов INTEGER и TEXT, поэтому для реализации определенных сценариев может не потребоваться никаких действий.</span><span class="sxs-lookup"><span data-stu-id="20cbd-485">Microsoft.Data.Sqlite also remains capable of reading character values from both INTEGER and TEXT columns, so certain scenarios may not require any action.</span></span>

## <a name="migration-ids-are-now-generated-using-the-invariant-cultures-calendar"></a><span data-ttu-id="20cbd-486">Идентификаторы миграции теперь создаются с использованием календаря инвариантного языка и региональных параметров</span><span class="sxs-lookup"><span data-stu-id="20cbd-486">Migration IDs are now generated using the invariant culture's calendar</span></span>

[<span data-ttu-id="20cbd-487">Отслеживание вопроса № 12978</span><span class="sxs-lookup"><span data-stu-id="20cbd-487">Tracking Issue #12978</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12978)

<span data-ttu-id="20cbd-488">Это изменение было внесено в предварительную версию 4 EF Core 3.0.</span><span class="sxs-lookup"><span data-stu-id="20cbd-488">This change was introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="20cbd-489">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="20cbd-489">**Old behavior**</span></span>

<span data-ttu-id="20cbd-490">Идентификаторы миграции непреднамеренно создавались с использованием календаря текущего языка и региональных параметров.</span><span class="sxs-lookup"><span data-stu-id="20cbd-490">Migration IDs were inadvertantly generated using the currret culture's calendar.</span></span>

<span data-ttu-id="20cbd-491">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="20cbd-491">**New behavior**</span></span>

<span data-ttu-id="20cbd-492">Теперь идентификаторы миграций всегда создаются с использованием календаря инвариантного языка и региональных параметров (григорианского).</span><span class="sxs-lookup"><span data-stu-id="20cbd-492">Migration IDs are now always generated using the invariant culture's calendar (Gregorian).</span></span>

<span data-ttu-id="20cbd-493">**Причина**</span><span class="sxs-lookup"><span data-stu-id="20cbd-493">**Why**</span></span>

<span data-ttu-id="20cbd-494">Порядок миграций играет важную роль при обновлении базы данных или разрешении конфликтов слияния.</span><span class="sxs-lookup"><span data-stu-id="20cbd-494">The order of migrations is important when updating the database or resolving merge conflicts.</span></span> <span data-ttu-id="20cbd-495">Использование инвариантного календаря позволяет избежать проблем с порядком выполнения, которые могут быть связаны с наличием разных системных календарей у членов группы.</span><span class="sxs-lookup"><span data-stu-id="20cbd-495">Using the invariant calendar avoids ordering issues that can result from team members having different system calendars.</span></span>

<span data-ttu-id="20cbd-496">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="20cbd-496">**Mitigations**</span></span>

<span data-ttu-id="20cbd-497">Это изменение затрагивает всех, кто использует календари, отличные от григорианского, в которых значение года больше, чем в григорианском (например, тай-буддистский календарь).</span><span class="sxs-lookup"><span data-stu-id="20cbd-497">This change affects anyone using a non-Gregorian calender where the year is greater than the Gregorian calendar (like the Thai Buddhist calendar).</span></span> <span data-ttu-id="20cbd-498">Имеющиеся идентификаторы миграций следует обновить, чтобы новые миграции выполнялись после существующих.</span><span class="sxs-lookup"><span data-stu-id="20cbd-498">Existing migration IDs will need to be updated so that new migrations are ordered after existing migrations.</span></span>

<span data-ttu-id="20cbd-499">Идентификатор миграции можно найти в атрибуте Migration в файлы конструктора миграций.</span><span class="sxs-lookup"><span data-stu-id="20cbd-499">The migration ID can be found in the Migration attribute in the migrations' designer files.</span></span>

``` diff
 [DbContext(typeof(MyDbContext))]
-[Migration("25620318122820_MyMigration")]
+[Migration("20190318122820_MyMigration")]
 partial class MyMigration
 {
```

<span data-ttu-id="20cbd-500">Кроме того, потребуется обновить таблицу журнала миграций.</span><span class="sxs-lookup"><span data-stu-id="20cbd-500">The Migrations history table also needs to be updated.</span></span>

``` sql
UPDATE __EFMigrationsHistory
SET MigrationId = CONCAT(LEFT(MigrationId, 4)  - 543, SUBSTRING(MigrationId, 4, 150))
```

## <a name="logquerypossibleexceptionwithaggregateoperator-has-been-renamed"></a><span data-ttu-id="20cbd-501">Оператор LogQueryPossibleExceptionWithAggregateOperator был переименован</span><span class="sxs-lookup"><span data-stu-id="20cbd-501">LogQueryPossibleExceptionWithAggregateOperator has been renamed</span></span>

[<span data-ttu-id="20cbd-502">Отслеживание проблемы № 10985</span><span class="sxs-lookup"><span data-stu-id="20cbd-502">Tracking Issue #10985</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10985)

<span data-ttu-id="20cbd-503">Это изменение было внесено в предварительную версию 4 EF Core 3.0.</span><span class="sxs-lookup"><span data-stu-id="20cbd-503">This change was introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="20cbd-504">**Изменение**</span><span class="sxs-lookup"><span data-stu-id="20cbd-504">**Change**</span></span>

<span data-ttu-id="20cbd-505">`RelationalEventId.LogQueryPossibleExceptionWithAggregateOperator` переименован в `RelationalEventId.LogQueryPossibleExceptionWithAggregateOperatorWarning`.</span><span class="sxs-lookup"><span data-stu-id="20cbd-505">`RelationalEventId.LogQueryPossibleExceptionWithAggregateOperator` has been renamed to `RelationalEventId.LogQueryPossibleExceptionWithAggregateOperatorWarning`.</span></span>

<span data-ttu-id="20cbd-506">**Причина**</span><span class="sxs-lookup"><span data-stu-id="20cbd-506">**Why**</span></span>

<span data-ttu-id="20cbd-507">Выравнивает именование этого события предупреждения с другими событиями предупреждения.</span><span class="sxs-lookup"><span data-stu-id="20cbd-507">Aligns the naming of this warning event with all other warning events.</span></span>

<span data-ttu-id="20cbd-508">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="20cbd-508">**Mitigations**</span></span>

<span data-ttu-id="20cbd-509">Используйте новое имя.</span><span class="sxs-lookup"><span data-stu-id="20cbd-509">Use the new name.</span></span> <span data-ttu-id="20cbd-510">(Обратите внимание, что номер события не изменен.)</span><span class="sxs-lookup"><span data-stu-id="20cbd-510">(Note that the event ID number has not changed.)</span></span>

## <a name="clarify-api-for-foreign-key-constraint-names"></a><span data-ttu-id="20cbd-511">Уточнение API для имен ограничений внешнего ключа</span><span class="sxs-lookup"><span data-stu-id="20cbd-511">Clarify API for foreign key constraint names</span></span>

[<span data-ttu-id="20cbd-512">Отслеживание проблемы № 10730</span><span class="sxs-lookup"><span data-stu-id="20cbd-512">Tracking Issue #10730</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10730)

<span data-ttu-id="20cbd-513">Это изменение было внесено в предварительную версию 4 EF Core 3.0.</span><span class="sxs-lookup"><span data-stu-id="20cbd-513">This change was introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="20cbd-514">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="20cbd-514">**Old behavior**</span></span>

<span data-ttu-id="20cbd-515">До EF Core 3.0 имена ограничений внешнего ключа назывались просто именами.</span><span class="sxs-lookup"><span data-stu-id="20cbd-515">Before EF Core 3.0, foreign key constraint names were referred to as simply the "name".</span></span> <span data-ttu-id="20cbd-516">Например:</span><span class="sxs-lookup"><span data-stu-id="20cbd-516">For example:</span></span>

```C#
var constraintName = myForeignKey.Name;
```

<span data-ttu-id="20cbd-517">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="20cbd-517">**New behavior**</span></span>

<span data-ttu-id="20cbd-518">Начиная с EF Core 3.0 имена ограничений внешнего ключа называются "имя ограничения".</span><span class="sxs-lookup"><span data-stu-id="20cbd-518">Starting with EF Core 3.0, foreign key constraint names are now referred to as the "constaint name".</span></span> <span data-ttu-id="20cbd-519">Например:</span><span class="sxs-lookup"><span data-stu-id="20cbd-519">For example:</span></span>

```C#
var constraintName = myForeignKey.ConstraintName;
```

<span data-ttu-id="20cbd-520">**Причина**</span><span class="sxs-lookup"><span data-stu-id="20cbd-520">**Why**</span></span>

<span data-ttu-id="20cbd-521">Это изменение обеспечивает согласованность именования в этой области, а также проясняет, что это имя ограничения внешнего ключа, а не имя столбца или свойства, для которого определен внешний ключ.</span><span class="sxs-lookup"><span data-stu-id="20cbd-521">This change brings consistency to naming in this area, and also clarifies that this is the name of the foreign key constaint, and not the column or property name that the foreign key is defined on.</span></span>

<span data-ttu-id="20cbd-522">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="20cbd-522">**Mitigations**</span></span>

<span data-ttu-id="20cbd-523">Используйте новое имя.</span><span class="sxs-lookup"><span data-stu-id="20cbd-523">Use the new name.</span></span>
