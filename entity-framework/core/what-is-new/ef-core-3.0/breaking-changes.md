---
title: Критические изменения в EF Core 3.0 — EF Core
author: divega
ms.date: 02/19/2019
ms.assetid: EE2878C9-71F9-4FA5-9BC4-60517C7C9830
uid: core/what-is-new/ef-core-3.0/breaking-changes
ms.openlocfilehash: 748db8a71a04a2d696ef21a03319906b9fc776be
ms.sourcegitcommit: a709054b2bc7a8365201d71f59325891aacd315f
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/14/2019
ms.locfileid: "57829230"
---
# <a name="breaking-changes-included-in-ef-core-30-currently-in-preview"></a><span data-ttu-id="85e61-102">Критические изменения в EF Core 3.0 (сейчас предоставляются в виде предварительной версии)</span><span class="sxs-lookup"><span data-stu-id="85e61-102">Breaking changes included in EF Core 3.0 (currently in preview)</span></span>

> [!IMPORTANT]
> <span data-ttu-id="85e61-103">Обратите внимание на то, что наборы возможностей и расписания будущих выпусков могут быть изменены, и, несмотря на то, что мы стараемся поддерживать эту страницу в актуальном состоянии, в определенные моменты времени содержание страницы может не отражать наши текущие планы.</span><span class="sxs-lookup"><span data-stu-id="85e61-103">Please note that the feature sets and schedules of future releases are always subject to change, and although we will try to keep this page up to date, it may not reflect our latest plans at all times.</span></span>

<span data-ttu-id="85e61-104">Указанные ниже изменения API и поведения могут нарушать работу приложений, разработанных для EF Core 2.2.x, при их обновлении до версии 3.0.0.</span><span class="sxs-lookup"><span data-stu-id="85e61-104">The following API and behavior changes have the potential to break applications developed for EF Core 2.2.x when upgrading them to 3.0.0.</span></span>
<span data-ttu-id="85e61-105">Изменения, которые, по нашим ожиданиям, повлияют только на поставщиков баз данных, описаны в разделе [изменений для поставщиков](../../providers/provider-log.md).</span><span class="sxs-lookup"><span data-stu-id="85e61-105">Changes that we expect to only impact database providers are documented under [provider changes](../../providers/provider-log.md).</span></span>
<span data-ttu-id="85e61-106">Здесь не описаны критические изменения в новых функциях, добавленных между разными предварительными версиями 3.0.</span><span class="sxs-lookup"><span data-stu-id="85e61-106">Breaks in new features introduced from one 3.0 preview to another 3.0 preview aren't documented here.</span></span>

## <a name="linq-queries-are-no-longer-evaluated-on-the-client"></a><span data-ttu-id="85e61-107">Запросы LINQ больше не вычисляются на клиенте</span><span class="sxs-lookup"><span data-stu-id="85e61-107">LINQ queries are no longer evaluated on the client</span></span>

<span data-ttu-id="85e61-108">[Отслеживание вопроса № 14935](https://github.com/aspnet/EntityFrameworkCore/issues/14935)
[Также см. вопрос № 12795](https://github.com/aspnet/EntityFrameworkCore/issues/12795)</span><span class="sxs-lookup"><span data-stu-id="85e61-108">[Tracking Issue #14935](https://github.com/aspnet/EntityFrameworkCore/issues/14935)
[Also see issue #12795](https://github.com/aspnet/EntityFrameworkCore/issues/12795)</span></span>

<span data-ttu-id="85e61-109">Это изменение будет внесено в предварительную версию 4 EF Core 3.0.</span><span class="sxs-lookup"><span data-stu-id="85e61-109">This change will be introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="85e61-110">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="85e61-110">**Old behavior**</span></span>

<span data-ttu-id="85e61-111">До выхода версии 3.0, если системе EF Core не удавалось преобразовать выражение, являющееся частью запроса, в код SQL или параметр, она автоматически рассчитывала это выражение на клиенте.</span><span class="sxs-lookup"><span data-stu-id="85e61-111">Before 3.0, when EF Core couldn't convert an expression that was part of a query to either SQL or a parameter, it automatically evaluated the expression on the client.</span></span>
<span data-ttu-id="85e61-112">По умолчанию вычисление на клиенте потенциально ресурсоемких выражений вызывало лишь отображение предупреждения.</span><span class="sxs-lookup"><span data-stu-id="85e61-112">By default, client evaluation of potentially expensive expressions only triggered a warning.</span></span>

<span data-ttu-id="85e61-113">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="85e61-113">**New behavior**</span></span>

<span data-ttu-id="85e61-114">Начиная с версии 3.0 система EF Core разрешает вычислять на клиенте только выражения в высокоуровневой проекции (последний вызов `Select()` в запросе).</span><span class="sxs-lookup"><span data-stu-id="85e61-114">Starting with 3.0, EF Core only allows expressions in the top-level projection (the last `Select()` call in the query) to be evaluated on the client.</span></span>
<span data-ttu-id="85e61-115">Если выражения в любой другой части запроса невозможно преобразовать в код SQL или параметр, возникает исключение.</span><span class="sxs-lookup"><span data-stu-id="85e61-115">When expressions in any other part of the query can't be converted to either SQL or a parameter, an exception is thrown.</span></span>

<span data-ttu-id="85e61-116">**Причина**</span><span class="sxs-lookup"><span data-stu-id="85e61-116">**Why**</span></span>

<span data-ttu-id="85e61-117">Автоматическое вычисление запросов на клиенте позволяет выполнять многие запросы, даже если не удается преобразовать их важные части.</span><span class="sxs-lookup"><span data-stu-id="85e61-117">Automatic client evaluation of queries allows many queries to be executed even if important parts of them can't be translated.</span></span>
<span data-ttu-id="85e61-118">Это может привести к непредвиденному и потенциально опасному поведению, которое может стать заметным только в рабочей среде.</span><span class="sxs-lookup"><span data-stu-id="85e61-118">This behavior can result in unexpected and potentially damaging behavior that may only become evident in production.</span></span>
<span data-ttu-id="85e61-119">Например, условие в вызове `Where()`, которое невозможно преобразовать, может привести к передаче всех строк из таблицы с сервера базы данных и к применению фильтра на клиенте.</span><span class="sxs-lookup"><span data-stu-id="85e61-119">For example, a condition in a `Where()` call which can't be translated can cause all rows from the table to be transferred from the database server, and the filter to be applied on the client.</span></span>
<span data-ttu-id="85e61-120">Такая ситуация легко может остаться незамеченной, если таблица содержит небольшое число строк в разработке, и дать резкие отрицательные последствия при переносе приложения в рабочую среду, когда таблица может содержать миллионы строк.</span><span class="sxs-lookup"><span data-stu-id="85e61-120">This situation can easily go undetected if the table contains only a few rows in development, but hit hard when the application moves to production, where the table may contain millions of rows.</span></span>
<span data-ttu-id="85e61-121">Практика показала, что предупреждения о вычислении на клиенте также часто игнорируются во время разработки.</span><span class="sxs-lookup"><span data-stu-id="85e61-121">Client evaluation warnings also proved too easy to ignore during development.</span></span>

<span data-ttu-id="85e61-122">Кроме того, автоматическое вычисление на клиенте может привести к проблемам, из-за которых улучшение преобразования запросов для определенных выражений приводит к возникновению непреднамеренных критических изменений между выпусками.</span><span class="sxs-lookup"><span data-stu-id="85e61-122">Besides this, automatic client evaluation can lead to issues in which improving query translation for specific expressions caused unintended breaking changes between releases.</span></span>

<span data-ttu-id="85e61-123">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="85e61-123">**Mitigations**</span></span>

<span data-ttu-id="85e61-124">Если запрос невозможно преобразовать полностью, то перепишите его в форме, которую можно преобразовать, либо используйте `AsEnumerable()`, `ToList()` или аналогичный элемент, чтобы явно перенести данные обратно на клиент, где можно произвести их дальнейшую обработку с помощью LINQ-to-Objects.</span><span class="sxs-lookup"><span data-stu-id="85e61-124">If a query can't be fully translated, then either rewrite the query in a form that can be translated, or use `AsEnumerable()`, `ToList()`, or similar to explicitly bring data back to the client where it can then be further processed using LINQ-to-Objects.</span></span>

## <a name="entity-framework-core-is-no-longer-part-of-the-aspnet-core-shared-framework"></a><span data-ttu-id="85e61-125">Entity Framework Core больше не является частью общей платформы ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="85e61-125">Entity Framework Core is no longer part of the ASP.NET Core shared framework</span></span>

[<span data-ttu-id="85e61-126">Отслеживание объявлений о вопросе 325</span><span class="sxs-lookup"><span data-stu-id="85e61-126">Tracking Issue Announcements#325</span></span>](https://github.com/aspnet/Announcements/issues/325)

<span data-ttu-id="85e61-127">Это изменение было внесено в предварительную версию 1 ASP.NET Core 3.0.</span><span class="sxs-lookup"><span data-stu-id="85e61-127">This change was introduced in ASP.NET Core 3.0-preview 1.</span></span> 

<span data-ttu-id="85e61-128">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="85e61-128">**Old behavior**</span></span>

<span data-ttu-id="85e61-129">До выхода ASP.NET Core 3.0 при добавлении ссылки на пакет для `Microsoft.AspNetCore.App` или `Microsoft.AspNetCore.All` она включала в себя EF Core и некоторые поставщики данных EF Core, например поставщик SQL Server.</span><span class="sxs-lookup"><span data-stu-id="85e61-129">Before ASP.NET Core 3.0, when you added a package reference to `Microsoft.AspNetCore.App` or `Microsoft.AspNetCore.All`, it would include EF Core and some of the EF Core data providers like the SQL Server provider.</span></span>

<span data-ttu-id="85e61-130">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="85e61-130">**New behavior**</span></span>

<span data-ttu-id="85e61-131">Начиная с версии 3.0 общая платформа ASP.NET Core не включает в себя EF Core или поставщики данных EF Core.</span><span class="sxs-lookup"><span data-stu-id="85e61-131">Starting in 3.0, the ASP.NET Core shared framework doesn't include EF Core or any EF Core data providers.</span></span>

<span data-ttu-id="85e61-132">**Причина**</span><span class="sxs-lookup"><span data-stu-id="85e61-132">**Why**</span></span>

<span data-ttu-id="85e61-133">До этого изменения для получения EF Core требовались различные действия в зависимости от того, ориентировано ли приложение на ASP.NET Core и SQL Server.</span><span class="sxs-lookup"><span data-stu-id="85e61-133">Before this change, getting EF Core required different steps depending on whether the application targeted ASP.NET Core and SQL Server or not.</span></span> <span data-ttu-id="85e61-134">Кроме того, обновление ASP.NET Core приводило к принудительному обновлению EF Core и поставщика SQL Server, что иногда нежелательно.</span><span class="sxs-lookup"><span data-stu-id="85e61-134">Also, upgrading ASP.NET Core forced the upgrade of EF Core and the SQL Server provider, which isn't always desirable.</span></span>

<span data-ttu-id="85e61-135">Благодаря этому изменению процесс получения EF Core стал одинаковым для всех поставщиков, поддерживаемых реализаций .NET и типов приложений.</span><span class="sxs-lookup"><span data-stu-id="85e61-135">With this change, the experience of getting EF Core is the same across all providers, supported .NET implementations and application types.</span></span>
<span data-ttu-id="85e61-136">Кроме того, теперь разработчики могут точно управлять временем обновления EF Core и поставщиков данных EF Core.</span><span class="sxs-lookup"><span data-stu-id="85e61-136">Developers can also now control exactly when EF Core and EF Core data providers are upgraded.</span></span>

<span data-ttu-id="85e61-137">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="85e61-137">**Mitigations**</span></span>

<span data-ttu-id="85e61-138">Чтобы использовать EF Core в приложении ASP.NET Core 3.0 или любом другом поддерживаемом приложении, нужно явно добавить ссылку на пакет в поставщик базы данных EF Core, который ваше приложение будет использовать.</span><span class="sxs-lookup"><span data-stu-id="85e61-138">To use EF Core in an ASP.NET Core 3.0 application or any other supported application, explicitly add a package reference to the EF Core database provider that your application will use.</span></span>

## <a name="query-execution-is-logged-at-debug-level"></a><span data-ttu-id="85e61-139">Выполнение запроса регистрируется на уровне отладки Debug</span><span class="sxs-lookup"><span data-stu-id="85e61-139">Query execution is logged at Debug level</span></span>

[<span data-ttu-id="85e61-140">Отслеживание вопроса 14523</span><span class="sxs-lookup"><span data-stu-id="85e61-140">Tracking Issue #14523</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14523)

<span data-ttu-id="85e61-141">Это изменение было внесено в предварительную версию 3 EF Core 3.0.</span><span class="sxs-lookup"><span data-stu-id="85e61-141">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="85e61-142">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="85e61-142">**Old behavior**</span></span>

<span data-ttu-id="85e61-143">До выхода EF Core 3.0 выполнение запросов и других команд зарегистрировалось на уровне `Info`.</span><span class="sxs-lookup"><span data-stu-id="85e61-143">Before EF Core 3.0, execution of queries and other commands was logged at the `Info` level.</span></span>

<span data-ttu-id="85e61-144">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="85e61-144">**New behavior**</span></span>

<span data-ttu-id="85e61-145">Начиная с EF Core 3.0 регистрация выполнения команды или кода SQL осуществляется на уровне `Debug`.</span><span class="sxs-lookup"><span data-stu-id="85e61-145">Starting with EF Core 3.0, logging of command/SQL execution is at the `Debug` level.</span></span>

<span data-ttu-id="85e61-146">**Причина**</span><span class="sxs-lookup"><span data-stu-id="85e61-146">**Why**</span></span>

<span data-ttu-id="85e61-147">Это изменение было внесено для снижения шума на уровне ведения журнала `Info`.</span><span class="sxs-lookup"><span data-stu-id="85e61-147">This change was made to reduce the noise at the `Info` log level.</span></span>

<span data-ttu-id="85e61-148">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="85e61-148">**Mitigations**</span></span>

<span data-ttu-id="85e61-149">Это событие ведения журнала определено `RelationalEventId.CommandExecuting` с идентификатором события 20100.</span><span class="sxs-lookup"><span data-stu-id="85e61-149">This logging event is defined by `RelationalEventId.CommandExecuting` with event ID 20100.</span></span>
<span data-ttu-id="85e61-150">Чтобы снова начать ведение журнала SQL на уровне `Info`, явно настройте уровень в методе `OnConfiguring` или `AddDbContext`.</span><span class="sxs-lookup"><span data-stu-id="85e61-150">To log SQL at the `Info` level again, explicitly configure the level in `OnConfiguring` or `AddDbContext`.</span></span>
<span data-ttu-id="85e61-151">Например:</span><span class="sxs-lookup"><span data-stu-id="85e61-151">For example:</span></span>
```C#
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    => optionsBuilder
        .UseSqlServer(connectionString)
        .ConfigureWarnings(c => c.Log((RelationalEventId.CommandExecuting, LogLevel.Info)));
```

## <a name="temporary-key-values-are-no-longer-set-onto-entity-instances"></a><span data-ttu-id="85e61-152">Временные значения ключа больше не устанавливаются для экземпляров сущностей</span><span class="sxs-lookup"><span data-stu-id="85e61-152">Temporary key values are no longer set onto entity instances</span></span>

[<span data-ttu-id="85e61-153">Отслеживание вопроса 12378</span><span class="sxs-lookup"><span data-stu-id="85e61-153">Tracking Issue #12378</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12378)

<span data-ttu-id="85e61-154">Это изменение было внесено в предварительную версию 2 EF Core 3.0.</span><span class="sxs-lookup"><span data-stu-id="85e61-154">This change was introduced in EF Core 3.0-preview 2.</span></span>

<span data-ttu-id="85e61-155">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="85e61-155">**Old behavior**</span></span>

<span data-ttu-id="85e61-156">До выхода EF Core 3.0 временные значения назначались всем свойствам ключей, которые позднее получили бы реальное значение, созданное базой данных.</span><span class="sxs-lookup"><span data-stu-id="85e61-156">Before EF Core 3.0, temporary values were assigned to all key properties that would later have a real value generated by the database.</span></span>
<span data-ttu-id="85e61-157">Обычно эти временные значения были большими отрицательными числами.</span><span class="sxs-lookup"><span data-stu-id="85e61-157">Usually these temporary values were large negative numbers.</span></span>

<span data-ttu-id="85e61-158">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="85e61-158">**New behavior**</span></span>

<span data-ttu-id="85e61-159">Начиная с версии 3.0 EF Core сохраняет временное значение ключа как часть сведений об отслеживании сущности, оставляя само свойство ключа без изменений.</span><span class="sxs-lookup"><span data-stu-id="85e61-159">Starting with 3.0, EF Core stores the temporary key value as part of the entity's tracking information, and leaves the key property itself unchanged.</span></span>

<span data-ttu-id="85e61-160">**Причина**</span><span class="sxs-lookup"><span data-stu-id="85e61-160">**Why**</span></span>

<span data-ttu-id="85e61-161">Это изменение было внесено для ситуации, когда временные значения ключей ошибочно становились постоянными из-за перемещения сущности, которая ранее отслеживалась некоторым экземпляром `DbContext`, в другой экземпляр `DbContext`.</span><span class="sxs-lookup"><span data-stu-id="85e61-161">This change was made to prevent temporary key values from erroneously becoming permanent when an entity that has been previously tracked by some `DbContext` instance is moved to a different `DbContext` instance.</span></span> 

<span data-ttu-id="85e61-162">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="85e61-162">**Mitigations**</span></span>

<span data-ttu-id="85e61-163">Приложения, которые назначают значения первичного ключа внешним ключам для формирования ассоциаций между сущностями, могут использовать старое поведение, если первичные ключи сформированы хранилищем и принадлежат сущностям в состоянии `Added`.</span><span class="sxs-lookup"><span data-stu-id="85e61-163">Applications that assign primary key values onto foreign keys to form associations between entities may depend on the old behavior if the primary keys are store-generated and belong to entities in the `Added` state.</span></span>
<span data-ttu-id="85e61-164">Этого можно избежать следующим образом:</span><span class="sxs-lookup"><span data-stu-id="85e61-164">This can be avoided by:</span></span>
* <span data-ttu-id="85e61-165">Отказ от использования ключей, сформированных хранилищем.</span><span class="sxs-lookup"><span data-stu-id="85e61-165">Not using store-generated keys.</span></span>
* <span data-ttu-id="85e61-166">Настройка свойств навигации для формирования отношений вместо задания значений внешнего ключа.</span><span class="sxs-lookup"><span data-stu-id="85e61-166">Setting navigation properties to form relationships instead of setting foreign key values.</span></span>
* <span data-ttu-id="85e61-167">Получение фактических временных значений ключа из сведений об отслеживании сущности.</span><span class="sxs-lookup"><span data-stu-id="85e61-167">Obtain the actual temporary key values from the entity's tracking information.</span></span>
<span data-ttu-id="85e61-168">Например, `context.Entry(blog).Property(e => e.Id).CurrentValue` возвратит временное значение, даже если сам `blog.Id` не был задан.</span><span class="sxs-lookup"><span data-stu-id="85e61-168">For example, `context.Entry(blog).Property(e => e.Id).CurrentValue` will return the temporary value even though `blog.Id` itself hasn't been set.</span></span>

## <a name="detectchanges-honors-store-generated-key-values"></a><span data-ttu-id="85e61-169">DetectChanges учитывает значения ключей, сформированные хранилищем</span><span class="sxs-lookup"><span data-stu-id="85e61-169">DetectChanges honors store-generated key values</span></span>

[<span data-ttu-id="85e61-170">Отслеживание вопроса 14616</span><span class="sxs-lookup"><span data-stu-id="85e61-170">Tracking Issue #14616</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14616)

<span data-ttu-id="85e61-171">Это изменение было внесено в предварительную версию 3 EF Core 3.0.</span><span class="sxs-lookup"><span data-stu-id="85e61-171">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="85e61-172">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="85e61-172">**Old behavior**</span></span>

<span data-ttu-id="85e61-173">До выпуска EF Core 3.0 неотслеживаемая сущность, обнаруженная `DetectChanges`, отслеживалась в состоянии `Added` и вставлялась в новую строку при вызове `SaveChanges`.</span><span class="sxs-lookup"><span data-stu-id="85e61-173">Before EF Core 3.0, an untracked entity found by `DetectChanges` would be tracked in the `Added` state and inserted as a new row when `SaveChanges` is called.</span></span>

<span data-ttu-id="85e61-174">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="85e61-174">**New behavior**</span></span>

<span data-ttu-id="85e61-175">Начиная с EF Core 3.0, если сущность использует сформированные значения ключа и какое-либо значение ключа задано, то она будет отслеживаться в состоянии `Modified`.</span><span class="sxs-lookup"><span data-stu-id="85e61-175">Starting with EF Core 3.0, if an entity is using generated key values and some key value is set, then the entity will be tracked in the `Modified` state.</span></span>
<span data-ttu-id="85e61-176">Это означает, что предполагается наличие строки для сущности, и она будет обновлена при вызове `SaveChanges`.</span><span class="sxs-lookup"><span data-stu-id="85e61-176">This means that a row for the entity is assumed to exist and it will be updated when `SaveChanges` is called.</span></span>
<span data-ttu-id="85e61-177">Если значение ключа не задано или тип сущности не использует сформированные ключи, то новая сущность все равно будет отслеживаться в состоянии `Added`, как в предыдущих версиях.</span><span class="sxs-lookup"><span data-stu-id="85e61-177">If the key value isn't set, or if the entity type isn't using generated keys, then the new entity will still be tracked as `Added` as in previous versions.</span></span>

<span data-ttu-id="85e61-178">**Причина**</span><span class="sxs-lookup"><span data-stu-id="85e61-178">**Why**</span></span>

<span data-ttu-id="85e61-179">Это изменение было внесено, чтобы сделать работу с несвязными графами сущностей при использовании ключей, сформированных хранилищем, более простой и согласованной.</span><span class="sxs-lookup"><span data-stu-id="85e61-179">This change was made to make it easier and more consistent to work with disconnected entity graphs while using store-generated keys.</span></span>

<span data-ttu-id="85e61-180">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="85e61-180">**Mitigations**</span></span>

<span data-ttu-id="85e61-181">Это изменение может нарушить работу приложения, если тип сущности должен использовать сформированные ключи, однако значения ключей явно заданы для новых экземпляров.</span><span class="sxs-lookup"><span data-stu-id="85e61-181">This change can break an application if an entity type is configured to use generated keys but key values are explicitly set for new instances.</span></span>
<span data-ttu-id="85e61-182">Решение проблемы заключается в явном запрете свойствам ключей использовать сформированные значения.</span><span class="sxs-lookup"><span data-stu-id="85e61-182">The fix is to explicitly configure the key properties to not use generated values.</span></span>
<span data-ttu-id="85e61-183">Например, с помощью текучего API:</span><span class="sxs-lookup"><span data-stu-id="85e61-183">For example, with the fluent API:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .ValueGeneratedNever();
```

<span data-ttu-id="85e61-184">Или с помощью заметок к данным:</span><span class="sxs-lookup"><span data-stu-id="85e61-184">Or with data annotations:</span></span>

```C#
[DatabaseGenerated(DatabaseGeneratedOption.None)]
public string Id { get; set; }
```

## <a name="cascade-deletions-now-happen-immediately-by-default"></a><span data-ttu-id="85e61-185">Каскадные удаления теперь по умолчанию выполняются немедленно</span><span class="sxs-lookup"><span data-stu-id="85e61-185">Cascade deletions now happen immediately by default</span></span>

[<span data-ttu-id="85e61-186">Отслеживание вопроса 10114</span><span class="sxs-lookup"><span data-stu-id="85e61-186">Tracking Issue #10114</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10114)

<span data-ttu-id="85e61-187">Это изменение было внесено в предварительную версию 3 EF Core 3.0.</span><span class="sxs-lookup"><span data-stu-id="85e61-187">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="85e61-188">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="85e61-188">**Old behavior**</span></span>

<span data-ttu-id="85e61-189">До выхода версии 3.0 применяемые EF Core каскадные действия (удаление зависимых сущностей, когда требуемый субъект удален либо отношение с ним было разорвано) не выполнялись до вызова SaveChanges.</span><span class="sxs-lookup"><span data-stu-id="85e61-189">Before 3.0, EF Core applied cascading actions (deleting dependent entities when a required principal is deleted or when the relationship to a required principal is severed) did not happen until SaveChanges was called.</span></span>

<span data-ttu-id="85e61-190">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="85e61-190">**New behavior**</span></span>

<span data-ttu-id="85e61-191">Начиная с версии 3.0 EF Core применяет каскадные действия сразу после обнаружения условия триггера.</span><span class="sxs-lookup"><span data-stu-id="85e61-191">Starting with 3.0, EF Core applies cascading actions as soon as the triggering condition is detected.</span></span>
<span data-ttu-id="85e61-192">Например, вызов `context.Remove()` для удаления сущности субъекта приведет к немедленной установке всех отслеживаемых связанных необходимых зависимых объектов в состояние `Deleted`.</span><span class="sxs-lookup"><span data-stu-id="85e61-192">For example, calling `context.Remove()` to delete a principal entity will result in all tracked related required dependents also being set to `Deleted` immediately.</span></span>

<span data-ttu-id="85e61-193">**Причина**</span><span class="sxs-lookup"><span data-stu-id="85e61-193">**Why**</span></span>

<span data-ttu-id="85e61-194">Это изменение было внесено, чтобы улучшить работу в сценариях аудита и привязки данных, где важно понимать, какие сущности будут удалены _перед_ вызовом `SaveChanges`.</span><span class="sxs-lookup"><span data-stu-id="85e61-194">This change was made to improve the experience for data binding and auditing scenarios where it is important to understand which entities will be deleted _before_ `SaveChanges` is called.</span></span>

<span data-ttu-id="85e61-195">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="85e61-195">**Mitigations**</span></span>

<span data-ttu-id="85e61-196">Для восстановления прежнего поведения можно использовать параметры `context.ChangedTracker`.</span><span class="sxs-lookup"><span data-stu-id="85e61-196">The previous behavior can be restored through settings on `context.ChangedTracker`.</span></span>
<span data-ttu-id="85e61-197">Например:</span><span class="sxs-lookup"><span data-stu-id="85e61-197">For example:</span></span>

```C#
context.ChangeTracker.CascadeDeleteTiming = CascadeTiming.OnSaveChanges;
context.ChangeTracker.DeleteOrphansTiming = CascadeTiming.OnSaveChanges;
```

## <a name="query-types-are-consolidated-with-entity-types"></a><span data-ttu-id="85e61-198">Типы запросов объединяются с типами сущностей</span><span class="sxs-lookup"><span data-stu-id="85e61-198">Query types are consolidated with entity types</span></span>

[<span data-ttu-id="85e61-199">Отслеживание вопроса 14194</span><span class="sxs-lookup"><span data-stu-id="85e61-199">Tracking Issue #14194</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14194)

<span data-ttu-id="85e61-200">Это изменение было внесено в предварительную версию 3 EF Core 3.0.</span><span class="sxs-lookup"><span data-stu-id="85e61-200">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="85e61-201">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="85e61-201">**Old behavior**</span></span>

<span data-ttu-id="85e61-202">До выпуска EF Core 3.0 [типы запросов](xref:core/modeling/query-types) представляли собой средства для запроса данных, не определяющие первичный ключ структурированным образом.</span><span class="sxs-lookup"><span data-stu-id="85e61-202">Before EF Core 3.0, [query types](xref:core/modeling/query-types) were a means to query data that doesn't define a primary key in a structured way.</span></span>
<span data-ttu-id="85e61-203">То есть тип запроса использовался для сопоставления типов сущностей без ключей (скорее всего, из представления, но, возможно, и из таблицы), а обычный тип сущности использовался при наличии ключа (скорее всего, из таблицы, но, возможно, и из представления).</span><span class="sxs-lookup"><span data-stu-id="85e61-203">That is, a query type was used for mapping entity types without keys (more likely from a view, but possibly from a table) while a regular entity type was used when a key was available (more likely from a table, but possibly from a view).</span></span>

<span data-ttu-id="85e61-204">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="85e61-204">**New behavior**</span></span>

<span data-ttu-id="85e61-205">Теперь тип запроса стал просто типом сущности без первичного ключа.</span><span class="sxs-lookup"><span data-stu-id="85e61-205">A query type now becomes just an entity type without a primary key.</span></span>
<span data-ttu-id="85e61-206">Типы сущностей без ключей имеют ту же функциональность, что типы запросов в предыдущих версиях.</span><span class="sxs-lookup"><span data-stu-id="85e61-206">Keyless entity types have the same functionality as query types in previous versions.</span></span>

<span data-ttu-id="85e61-207">**Причина**</span><span class="sxs-lookup"><span data-stu-id="85e61-207">**Why**</span></span>

<span data-ttu-id="85e61-208">Это изменение было внесено, чтобы избежать путаницы с назначением типов запросов.</span><span class="sxs-lookup"><span data-stu-id="85e61-208">This change was made to reduce the confusion around the purpose of query types.</span></span>
<span data-ttu-id="85e61-209">В частности, они представляют собой типы сущностей без ключей и поэтому по определению доступны только для чтения, однако их не следует использовать только потому, что тип сущности должен быть доступен только для чтения.</span><span class="sxs-lookup"><span data-stu-id="85e61-209">Specifically, they are keyless entity types and they are inherently read-only because of this, but they should not be used just because an entity type needs to be read-only.</span></span>
<span data-ttu-id="85e61-210">Аналогично, их часто сопоставляют с представлениями, но только потому, что представления часто не определяют ключи.</span><span class="sxs-lookup"><span data-stu-id="85e61-210">Likewise, they are often mapped to views, but this is only because views often don't define keys.</span></span>

<span data-ttu-id="85e61-211">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="85e61-211">**Mitigations**</span></span>

<span data-ttu-id="85e61-212">Следующие компоненты этого API теперь являются устаревшими:</span><span class="sxs-lookup"><span data-stu-id="85e61-212">The following parts of the API are now obsolete:</span></span>
* <span data-ttu-id="85e61-213">**`ModelBuilder.Query<>()`** — вместо него следует вызвать `ModelBuilder.Entity<>().HasNoKey()`, чтобы пометить тип сущности как не имеющий ключей.</span><span class="sxs-lookup"><span data-stu-id="85e61-213">**`ModelBuilder.Query<>()`** - Instead `ModelBuilder.Entity<>().HasNoKey()` needs to be called to mark an entity type as having no keys.</span></span>
<span data-ttu-id="85e61-214">Это по-прежнему не следует настраивать посредством соглашения, чтобы избежать ошибки, когда первичный ключ ожидается, но не соответствует соглашению.</span><span class="sxs-lookup"><span data-stu-id="85e61-214">This would still not be configured by convention to avoid misconfiguration when a primary key is expected, but doesn't match the convention.</span></span>
* <span data-ttu-id="85e61-215">**`DbQuery<>`** — вместо него следует использовать `DbSet<>`.</span><span class="sxs-lookup"><span data-stu-id="85e61-215">**`DbQuery<>`** - Instead `DbSet<>` should be used.</span></span>
* <span data-ttu-id="85e61-216">**`DbContext.Query<>()`** — вместо него следует использовать `DbContext.Set<>()`.</span><span class="sxs-lookup"><span data-stu-id="85e61-216">**`DbContext.Query<>()`** - Instead `DbContext.Set<>()` should be used.</span></span>

## <a name="configuration-api-for-owned-type-relationships-has-changed"></a><span data-ttu-id="85e61-217">Изменение API конфигурации для отношений принадлежащего типа</span><span class="sxs-lookup"><span data-stu-id="85e61-217">Configuration API for owned type relationships has changed</span></span>

<span data-ttu-id="85e61-218">[Отслеживание вопроса 12444](https://github.com/aspnet/EntityFrameworkCore/issues/12444)
[Отслеживание вопроса 9148](https://github.com/aspnet/EntityFrameworkCore/issues/9148)
[Отслеживание вопроса 14153](https://github.com/aspnet/EntityFrameworkCore/issues/14153)</span><span class="sxs-lookup"><span data-stu-id="85e61-218">[Tracking Issue #12444](https://github.com/aspnet/EntityFrameworkCore/issues/12444)
[Tracking Issue #9148](https://github.com/aspnet/EntityFrameworkCore/issues/9148)
[Tracking Issue #14153](https://github.com/aspnet/EntityFrameworkCore/issues/14153)</span></span>

<span data-ttu-id="85e61-219">Это изменение было внесено в предварительную версию 3 EF Core 3.0.</span><span class="sxs-lookup"><span data-stu-id="85e61-219">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="85e61-220">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="85e61-220">**Old behavior**</span></span>

<span data-ttu-id="85e61-221">До выхода EF Core 3.0 конфигурация принадлежащего отношения выполнялась непосредственно после вызова `OwnsOne` или `OwnsMany`.</span><span class="sxs-lookup"><span data-stu-id="85e61-221">Before EF Core 3.0, configuration of the owned relationship was performed directly after the `OwnsOne` or `OwnsMany` call.</span></span> 

<span data-ttu-id="85e61-222">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="85e61-222">**New behavior**</span></span>

<span data-ttu-id="85e61-223">Начиная с EF Core 3.0 существует текучий API для настройки свойства навигации для владельца с помощью `WithOwner()`.</span><span class="sxs-lookup"><span data-stu-id="85e61-223">Starting with EF Core 3.0, there is now fluent API to configure a navigation property to the owner using `WithOwner()`.</span></span>
<span data-ttu-id="85e61-224">Например:</span><span class="sxs-lookup"><span data-stu-id="85e61-224">For example:</span></span>

```C#
modelBuilder.Entity<Order>.OwnsOne(e => e.Details).WithOwner(e => e.Order);
```

<span data-ttu-id="85e61-225">Конфигурация, связанная с отношением между владельцем и принадлежащим объектом, теперь должна быть включена в цепочку после `WithOwner()` по аналогии с настройкой других отношений.</span><span class="sxs-lookup"><span data-stu-id="85e61-225">The configuration related to the relationship between owner and owned should now be chained after `WithOwner()` similarly to how other relationships are configured.</span></span>
<span data-ttu-id="85e61-226">При этом конфигурация для принадлежащего типа сама будет включена в цепочку после `OwnsOne()/OwnsMany()`.</span><span class="sxs-lookup"><span data-stu-id="85e61-226">While the configuration for the owned type itself would still be chained after `OwnsOne()/OwnsMany()`.</span></span>
<span data-ttu-id="85e61-227">Например:</span><span class="sxs-lookup"><span data-stu-id="85e61-227">For example:</span></span>

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

<span data-ttu-id="85e61-228">Кроме того, вызов `Entity()`, `HasOne()` или `Set()` с целевым объектом принадлежащего типа теперь приведет к возникновению исключения.</span><span class="sxs-lookup"><span data-stu-id="85e61-228">Additionally calling `Entity()`, `HasOne()`, or `Set()` with an owned type target will now throw an exception.</span></span>

<span data-ttu-id="85e61-229">**Причина**</span><span class="sxs-lookup"><span data-stu-id="85e61-229">**Why**</span></span>

<span data-ttu-id="85e61-230">Это изменение было внесено для более четкого разграничения между настройкой самого принадлежащего типа и _отношения_ с ним.</span><span class="sxs-lookup"><span data-stu-id="85e61-230">This change was made to create a cleaner separation between configuring the owned type itself and the _relationship to_ the owned type.</span></span>
<span data-ttu-id="85e61-231">Это, в свою очередь, устраняет неоднозначность и путаницу при использовании таких методов, как `HasForeignKey`.</span><span class="sxs-lookup"><span data-stu-id="85e61-231">This in turn removes ambiguity and confusion around methods like `HasForeignKey`.</span></span>

<span data-ttu-id="85e61-232">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="85e61-232">**Mitigations**</span></span>

<span data-ttu-id="85e61-233">Измените конфигурацию отношений принадлежащего типа, чтобы использовать новую поверхность API, как показано в приведенном выше примере.</span><span class="sxs-lookup"><span data-stu-id="85e61-233">Change configuration of owned type relationships to use the new API surface as shown in the example above.</span></span>

## <a name="the-foreign-key-property-convention-no-longer-matches-same-name-as-the-principal-property"></a><span data-ttu-id="85e61-234">Соглашение для свойства внешнего ключа больше не совпадает со свойством субъекта по имени</span><span class="sxs-lookup"><span data-stu-id="85e61-234">The foreign key property convention no longer matches same name as the principal property</span></span>

[<span data-ttu-id="85e61-235">Отслеживание вопроса 13274</span><span class="sxs-lookup"><span data-stu-id="85e61-235">Tracking Issue #13274</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13274)

<span data-ttu-id="85e61-236">Это изменение было внесено в предварительную версию 3 EF Core 3.0.</span><span class="sxs-lookup"><span data-stu-id="85e61-236">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="85e61-237">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="85e61-237">**Old behavior**</span></span>

<span data-ttu-id="85e61-238">Рассмотрим следующую модель:</span><span class="sxs-lookup"><span data-stu-id="85e61-238">Consider the following model:</span></span>
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
<span data-ttu-id="85e61-239">До выхода EF Core 3.0 свойство `CustomerId` использовалось для внешнего ключа по соглашению.</span><span class="sxs-lookup"><span data-stu-id="85e61-239">Before EF Core 3.0, the `CustomerId` property would be used for the foreign key by convention.</span></span>
<span data-ttu-id="85e61-240">Однако если `Order` является принадлежащим типом, это также делает `CustomerId` первичным ключом, что обычно не соответствует ожидаемому результату.</span><span class="sxs-lookup"><span data-stu-id="85e61-240">However, if `Order` is an owned type, then this would also make `CustomerId` the primary key and this isn't usually the expectation.</span></span>

<span data-ttu-id="85e61-241">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="85e61-241">**New behavior**</span></span>

<span data-ttu-id="85e61-242">Начиная с версии 3.0 EF Core не будет пытаться использовать свойства для внешних ключей по соглашению, если они имеют такое же имя, что и свойство субъекта.</span><span class="sxs-lookup"><span data-stu-id="85e61-242">Starting with 3.0, EF Core won't try to use properties for foreign keys by convention if they have the same name as the principal property.</span></span>
<span data-ttu-id="85e61-243">Шаблоны имени типа субъекта, сцепленного с именем свойства субъекта, и имени навигации, сцепленного с именем свойства субъекта, по-прежнему совпадают.</span><span class="sxs-lookup"><span data-stu-id="85e61-243">Principal type name concatenated with principal property name, and navigation name concatenated with principal property name patterns are still matched.</span></span>
<span data-ttu-id="85e61-244">Например:</span><span class="sxs-lookup"><span data-stu-id="85e61-244">For example:</span></span>

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

<span data-ttu-id="85e61-245">**Причина**</span><span class="sxs-lookup"><span data-stu-id="85e61-245">**Why**</span></span>

<span data-ttu-id="85e61-246">Это изменение было внесено во избежание ошибочного определения свойств первичного ключа для принадлежащего типа.</span><span class="sxs-lookup"><span data-stu-id="85e61-246">This change was made to avoid erroneously defining a primary key property on the owned type.</span></span>

<span data-ttu-id="85e61-247">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="85e61-247">**Mitigations**</span></span>

<span data-ttu-id="85e61-248">Если свойство должно быть внешним ключом и, следовательно, частью первичного ключа, следует явно указать это при его настройке.</span><span class="sxs-lookup"><span data-stu-id="85e61-248">If the property was intended to be the foreign key, and hence part of the primary key, then explicitly configure it as such.</span></span>

## <a name="each-property-uses-independent-in-memory-integer-key-generation"></a><span data-ttu-id="85e61-249">Каждое свойство использует создание независимых целочисленных ключей в памяти</span><span class="sxs-lookup"><span data-stu-id="85e61-249">Each property uses independent in-memory integer key generation</span></span>

[<span data-ttu-id="85e61-250">Отслеживание вопроса 6872</span><span class="sxs-lookup"><span data-stu-id="85e61-250">Tracking Issue #6872</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/6872)

<span data-ttu-id="85e61-251">Это изменение будет внесено в предварительную версию 4 EF Core 3.0.</span><span class="sxs-lookup"><span data-stu-id="85e61-251">This change will be introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="85e61-252">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="85e61-252">**Old behavior**</span></span>

<span data-ttu-id="85e61-253">До выхода EF Core 3.0 для всех свойств целочисленных ключей в памяти использовался один общий генератор значений.</span><span class="sxs-lookup"><span data-stu-id="85e61-253">Before EF Core 3.0, one shared value generator was used for all in-memory integer key properties.</span></span>

<span data-ttu-id="85e61-254">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="85e61-254">**New behavior**</span></span>

<span data-ttu-id="85e61-255">Начиная с EF Core 3.0 при использовании выполняющейся в памяти базы данных каждое свойство целочисленного ключа получает свой собственный генератор значений.</span><span class="sxs-lookup"><span data-stu-id="85e61-255">Starting with EF Core 3.0, each integer key property gets its own value generator when using the in-memory database.</span></span>
<span data-ttu-id="85e61-256">Кроме того, при удалении базы данных формирование ключа сбрасывается для всех таблиц.</span><span class="sxs-lookup"><span data-stu-id="85e61-256">Also, if the database is deleted, then key generation is reset for all tables.</span></span>

<span data-ttu-id="85e61-257">**Причина**</span><span class="sxs-lookup"><span data-stu-id="85e61-257">**Why**</span></span>

<span data-ttu-id="85e61-258">Это изменение было внесено, чтобы лучше согласовать формирование ключей в памяти с реальным процессом в базе данных, а также улучшить возможность изоляции тестов друг от друга при использовании выполняющейся в памяти базы данных.</span><span class="sxs-lookup"><span data-stu-id="85e61-258">This change was made to align in-memory key generation more closely to real database key generation and to improve the ability to isolate tests from each other when using the in-memory database.</span></span>

<span data-ttu-id="85e61-259">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="85e61-259">**Mitigations**</span></span>

<span data-ttu-id="85e61-260">Это изменение может нарушить работу приложения, полагающегося на задание определенных значений ключа в памяти.</span><span class="sxs-lookup"><span data-stu-id="85e61-260">This can break an application that is relying on specific in-memory key values to be set.</span></span>
<span data-ttu-id="85e61-261">Рекомендуется не полагаться на конкретные значения ключей либо реализовать новое поведение.</span><span class="sxs-lookup"><span data-stu-id="85e61-261">Consider instead not relying on specific key values, or updating to match the new behavior.</span></span>

## <a name="backing-fields-are-used-by-default"></a><span data-ttu-id="85e61-262">По умолчанию используются резервные поля</span><span class="sxs-lookup"><span data-stu-id="85e61-262">Backing fields are used by default</span></span>

[<span data-ttu-id="85e61-263">Отслеживание вопроса 12430</span><span class="sxs-lookup"><span data-stu-id="85e61-263">Tracking Issue #12430</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12430)

<span data-ttu-id="85e61-264">Это изменение было внесено в предварительную версию 2 EF Core 3.0.</span><span class="sxs-lookup"><span data-stu-id="85e61-264">This change was introduced in EF Core 3.0-preview 2.</span></span>

<span data-ttu-id="85e61-265">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="85e61-265">**Old behavior**</span></span>

<span data-ttu-id="85e61-266">До выхода версии 3.0, даже если резервное поле для свойства было известно, система EF Core все равно по умолчанию считывала и записывала значение свойства с помощью методов получения и задания свойства.</span><span class="sxs-lookup"><span data-stu-id="85e61-266">Before 3.0, even if the backing field for a property was known, EF Core would still by default read and write the property value using the property getter and setter methods.</span></span>
<span data-ttu-id="85e61-267">Исключением из этого было выполнение запроса, где известное резервное поле устанавливается напрямую.</span><span class="sxs-lookup"><span data-stu-id="85e61-267">The exception to this was query execution, where the backing field would be set directly if known.</span></span>

<span data-ttu-id="85e61-268">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="85e61-268">**New behavior**</span></span>

<span data-ttu-id="85e61-269">Начиная с EF Core 3.0, если резервное поле для свойства известно, то чтение и запись этого свойства всегда осуществляются с его помощью.</span><span class="sxs-lookup"><span data-stu-id="85e61-269">Starting with EF Core 3.0, if the backing field for a property is known, then will always read and write that property using the backing field.</span></span>
<span data-ttu-id="85e61-270">Это может нарушить работу приложения, если оно полагается на дополнительное поведение, закодированное в методы получения и задания.</span><span class="sxs-lookup"><span data-stu-id="85e61-270">This could cause an application break if the application is relying on additional behavior coded into the getter or setter methods.</span></span>

<span data-ttu-id="85e61-271">**Причина**</span><span class="sxs-lookup"><span data-stu-id="85e61-271">**Why**</span></span>

<span data-ttu-id="85e61-272">Это изменение было внесено, чтобы не позволить EF Core ошибочно активировать бизнес-логику по умолчанию при выполнении операций базы данных, затрагивающих сущности.</span><span class="sxs-lookup"><span data-stu-id="85e61-272">This change was made to prevent EF Core from erroneously triggering business logic by default when performing database operations involving the entities.</span></span>

<span data-ttu-id="85e61-273">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="85e61-273">**Mitigations**</span></span>

<span data-ttu-id="85e61-274">Поведение, предшествовавшее выходу версии 3.0, можно восстановить, настроив режим доступа к свойству в текучем API modelBuilder.</span><span class="sxs-lookup"><span data-stu-id="85e61-274">The pre-3.0 behavior can be restored through configuration of the property access mode in the modelBuilder fluent API.</span></span>
<span data-ttu-id="85e61-275">Например:</span><span class="sxs-lookup"><span data-stu-id="85e61-275">For example:</span></span>

```C#
modelBuilder.UsePropertyAccessMode(PropertyAccessMode.PreferFieldDuringConstruction);
```

## <a name="throw-if-multiple-compatible-backing-fields-are-found"></a><span data-ttu-id="85e61-276">Исключение при обнаружении нескольких совместимых резервных полей</span><span class="sxs-lookup"><span data-stu-id="85e61-276">Throw if multiple compatible backing fields are found</span></span>

[<span data-ttu-id="85e61-277">Отслеживание вопроса 12523</span><span class="sxs-lookup"><span data-stu-id="85e61-277">Tracking Issue #12523</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12523)

<span data-ttu-id="85e61-278">Это изменение будет внесено в предварительную версию 4 EF Core 3.0.</span><span class="sxs-lookup"><span data-stu-id="85e61-278">This change will be introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="85e61-279">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="85e61-279">**Old behavior**</span></span>

<span data-ttu-id="85e61-280">До выхода EF Core 3.0, если правила, обеспечивающие поиск резервного поля для свойства, находили несколько полей, выбиралось одно из них на основе определенной очередности.</span><span class="sxs-lookup"><span data-stu-id="85e61-280">Before EF Core 3.0, if multiple fields matched the rules for finding the backing field of a property, then one field would be chosen based on some precedence order.</span></span>
<span data-ttu-id="85e61-281">В неоднозначных случаях это могло привести к использованию неправильного поля.</span><span class="sxs-lookup"><span data-stu-id="85e61-281">This could cause the wrong field to be used in ambiguous cases.</span></span>

<span data-ttu-id="85e61-282">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="85e61-282">**New behavior**</span></span>

<span data-ttu-id="85e61-283">Начиная с EF Core 3.0 при обнаружении нескольких полей для одного свойства возникает исключение.</span><span class="sxs-lookup"><span data-stu-id="85e61-283">Starting with EF Core 3.0, if multiple fields are matched to the same property, then an exception is thrown.</span></span>

<span data-ttu-id="85e61-284">**Причина**</span><span class="sxs-lookup"><span data-stu-id="85e61-284">**Why**</span></span>

<span data-ttu-id="85e61-285">Это изменение было внесено во избежание автоматического использования одного поля вместо другого, когда правильным может быть только одно из них.</span><span class="sxs-lookup"><span data-stu-id="85e61-285">This change was made to avoid silently using one field over another when only one can be correct.</span></span>

<span data-ttu-id="85e61-286">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="85e61-286">**Mitigations**</span></span>

<span data-ttu-id="85e61-287">Для свойств с неоднозначными резервными полями используемое поле должно быть задано явным образом.</span><span class="sxs-lookup"><span data-stu-id="85e61-287">Properties with ambiguous backing fields must have the field to use specified explicitly.</span></span>
<span data-ttu-id="85e61-288">Например, с помощью текучего API:</span><span class="sxs-lookup"><span data-stu-id="85e61-288">For example, using the fluent API:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .HasField("_id");
```

## <a name="adddbcontextadddbcontextpool-no-longer-call-addlogging-and-addmemorycache"></a><span data-ttu-id="85e61-289">AddDbContext/AddDbContextPool больше не вызывает метод AddLogging и AddMemoryCache</span><span class="sxs-lookup"><span data-stu-id="85e61-289">AddDbContext/AddDbContextPool no longer call AddLogging and AddMemoryCache</span></span>

[<span data-ttu-id="85e61-290">Отслеживание вопроса № 14756</span><span class="sxs-lookup"><span data-stu-id="85e61-290">Tracking Issue #14756</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14756)

<span data-ttu-id="85e61-291">Это изменение будет внесено в предварительную версию 4 EF Core 3.0.</span><span class="sxs-lookup"><span data-stu-id="85e61-291">This change will be introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="85e61-292">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="85e61-292">**Old behavior**</span></span>

<span data-ttu-id="85e61-293">До EF Core 3.0 вызов метода `AddDbContext` или `AddDbContextPool` также приводил к регистрации служб ведения журналов и кэширования памяти с внедрением зависимостей с помощью вызовов к методам [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) и [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span><span class="sxs-lookup"><span data-stu-id="85e61-293">Before EF Core 3.0, calling `AddDbContext` or `AddDbContextPool` would also register logging and memory caching services with D.I through calls to [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) and [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span></span>

<span data-ttu-id="85e61-294">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="85e61-294">**New behavior**</span></span>

<span data-ttu-id="85e61-295">Начиная с версии EF Core 3.0 методы `AddDbContext` и `AddDbContextPool` больше не регистрируют такие службы с внедрением зависимостей.</span><span class="sxs-lookup"><span data-stu-id="85e61-295">Starting with EF Core 3.0, `AddDbContext` and `AddDbContextPool` will no longer register these services with Dependency Injection (DI).</span></span>

<span data-ttu-id="85e61-296">**Причина**</span><span class="sxs-lookup"><span data-stu-id="85e61-296">**Why**</span></span>

<span data-ttu-id="85e61-297">EF Core 3.0 не требует, чтобы такие службы присутствовали в контейнере внедрения зависимостей приложения.</span><span class="sxs-lookup"><span data-stu-id="85e61-297">EF Core 3.0 does not require that these services are in the application's DI cotainer.</span></span> <span data-ttu-id="85e61-298">Но если интерфейс `ILoggerFactory` зарегистрирован в контейнере внедрения зависимостей приложения, он будет по-прежнему использоваться EF Core.</span><span class="sxs-lookup"><span data-stu-id="85e61-298">However, if `ILoggerFactory` is registered in the application's DI container, then it will still be used by EF Core.</span></span>

<span data-ttu-id="85e61-299">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="85e61-299">**Mitigations**</span></span>

<span data-ttu-id="85e61-300">Если для работы вашего приложения нужны такие службы, явно зарегистрируйте их в контейнере внедрения зависимостей с помощью метода [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) или [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span><span class="sxs-lookup"><span data-stu-id="85e61-300">If your application needs these services, then register them explicitly with the DI container using  [AddLogging](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.loggingservicecollectionextensions.addlogging) or [AddMemoryCache](https://docs.microsoft.com/dotnet/api/microsoft.extensions.dependencyinjection.memorycacheservicecollectionextensions.addmemorycache).</span></span>

## <a name="dbcontextentry-now-performs-a-local-detectchanges"></a><span data-ttu-id="85e61-301">DbContext.Entry теперь выполняет локальную процедуру DetectChanges</span><span class="sxs-lookup"><span data-stu-id="85e61-301">DbContext.Entry now performs a local DetectChanges</span></span>

[<span data-ttu-id="85e61-302">Отслеживание вопроса 13552</span><span class="sxs-lookup"><span data-stu-id="85e61-302">Tracking Issue #13552</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13552)

<span data-ttu-id="85e61-303">Это изменение было внесено в предварительную версию 3 EF Core 3.0.</span><span class="sxs-lookup"><span data-stu-id="85e61-303">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="85e61-304">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="85e61-304">**Old behavior**</span></span>

<span data-ttu-id="85e61-305">До выхода EF Core 3.0 вызов `DbContext.Entry` приводил к обнаружению изменений для всех отслеживаемых сущностей.</span><span class="sxs-lookup"><span data-stu-id="85e61-305">Before EF Core 3.0, calling `DbContext.Entry` would cause changes to be detected for all tracked entities.</span></span>
<span data-ttu-id="85e61-306">Это гарантировало, что состояние, представленное в `EntityEntry`, было актуальным.</span><span class="sxs-lookup"><span data-stu-id="85e61-306">This ensured that the state exposed in the `EntityEntry` was up-to-date.</span></span>

<span data-ttu-id="85e61-307">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="85e61-307">**New behavior**</span></span>

<span data-ttu-id="85e61-308">Начиная с EF Core 3.0 при вызове `DbContext.Entry` будет предприниматься попытка обнаружить изменения только в заданной сущности и всех связанных с ней отслеживаемых сущностях субъектов.</span><span class="sxs-lookup"><span data-stu-id="85e61-308">Starting with EF Core 3.0, calling `DbContext.Entry` will now only attempt to detect changes in the given entity and any tracked principal entities related to it.</span></span>
<span data-ttu-id="85e61-309">Это означает, что изменения в другом месте могут не обнаруживаться при вызове этого метода, что может негативно повлиять на состояние приложения.</span><span class="sxs-lookup"><span data-stu-id="85e61-309">This means that changes elsewhere may not have been detected by calling this method, which could have implications on application state.</span></span>

<span data-ttu-id="85e61-310">Обратите внимание, что если для `ChangeTracker.AutoDetectChangesEnabled` задано значение `false`, то будет отключено даже такое локальное обнаружение изменений.</span><span class="sxs-lookup"><span data-stu-id="85e61-310">Note that if `ChangeTracker.AutoDetectChangesEnabled` is set to `false` then even this local change detection will be disabled.</span></span>

<span data-ttu-id="85e61-311">Другие методы, вызывающие обнаружение изменений, например `ChangeTracker.Entries` и `SaveChanges`, по-прежнему подразумевают полную процедуру `DetectChanges` для всех отслеживаемых сущностей.</span><span class="sxs-lookup"><span data-stu-id="85e61-311">Other methods that cause change detection--for example `ChangeTracker.Entries` and `SaveChanges`--still cause a full `DetectChanges` of all tracked entities.</span></span>

<span data-ttu-id="85e61-312">**Причина**</span><span class="sxs-lookup"><span data-stu-id="85e61-312">**Why**</span></span>

<span data-ttu-id="85e61-313">Это изменение было внесено для повышения производительности по умолчанию при использовании `context.Entry`.</span><span class="sxs-lookup"><span data-stu-id="85e61-313">This change was made to improve the default performance of using `context.Entry`.</span></span>

<span data-ttu-id="85e61-314">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="85e61-314">**Mitigations**</span></span>

<span data-ttu-id="85e61-315">Вызовите `ChgangeTracker.DetectChanges()` явным образом перед вызовом `Entry`, чтобы обеспечить поведение, предшествовавшее выходу версии 3.0.</span><span class="sxs-lookup"><span data-stu-id="85e61-315">Call `ChgangeTracker.DetectChanges()` explicitly before calling `Entry` to ensure the pre-3.0 behavior.</span></span>

## <a name="string-and-byte-array-keys-are-not-client-generated-by-default"></a><span data-ttu-id="85e61-316">Ключи массива строк и байтов не формируются клиентом по умолчанию</span><span class="sxs-lookup"><span data-stu-id="85e61-316">String and byte array keys are not client-generated by default</span></span>

[<span data-ttu-id="85e61-317">Отслеживание вопроса 14617</span><span class="sxs-lookup"><span data-stu-id="85e61-317">Tracking Issue #14617</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14617)

<span data-ttu-id="85e61-318">Это изменение будет внесено в предварительную версию 4 EF Core 3.0.</span><span class="sxs-lookup"><span data-stu-id="85e61-318">This change will be introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="85e61-319">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="85e61-319">**Old behavior**</span></span>

<span data-ttu-id="85e61-320">До выхода EF Core 3.0 свойства ключей `string` и `byte[]` можно было использовать без явного указания значения, отличного от NULL.</span><span class="sxs-lookup"><span data-stu-id="85e61-320">Before EF Core 3.0, `string` and `byte[]` key properties could be used without explicitly setting a non-null value.</span></span>
<span data-ttu-id="85e61-321">В этом случае значение ключа создается на клиенте в виде GUID, сериализованного до байтов для `byte[]`.</span><span class="sxs-lookup"><span data-stu-id="85e61-321">In such a case, the key value would be generated on the client as a GUID, serialized to bytes for `byte[]`.</span></span>

<span data-ttu-id="85e61-322">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="85e61-322">**New behavior**</span></span>

<span data-ttu-id="85e61-323">Начиная с EF Core 3.0 возникает исключение, указывающее на то, что значение ключа не задано.</span><span class="sxs-lookup"><span data-stu-id="85e61-323">Starting with EF Core 3.0 an exception will be thrown indicating that no key value has been set.</span></span>

<span data-ttu-id="85e61-324">**Причина**</span><span class="sxs-lookup"><span data-stu-id="85e61-324">**Why**</span></span>

<span data-ttu-id="85e61-325">Это изменение было внесено, так как формируемые клиентом значения `string`/`byte[]` обычно бесполезны, а поведение по умолчанию затрудняло типовое планирование использования этих значений ключей.</span><span class="sxs-lookup"><span data-stu-id="85e61-325">This change was made because client-generated `string`/`byte[]` values generally aren't useful, and the default behavior made it hard to reason about generated key values in a common way.</span></span>

<span data-ttu-id="85e61-326">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="85e61-326">**Mitigations**</span></span>

<span data-ttu-id="85e61-327">Поведение, предшествовавшее выходу версии 3.0, можно получить, явно указав, что свойства ключей должны использовать формируемые значения, если не задано никакое другое значение, отличное от NULL.</span><span class="sxs-lookup"><span data-stu-id="85e61-327">The pre-3.0 behavior can be obtained by explicitly specifying that the key properties should use generated values if no other non-null value is set.</span></span>
<span data-ttu-id="85e61-328">Например, с помощью текучего API:</span><span class="sxs-lookup"><span data-stu-id="85e61-328">For example, with the fluent API:</span></span>

```C#
modelBuilder
    .Entity<Blog>()
    .Property(e => e.Id)
    .ValueGeneratedOnAdd();
```

<span data-ttu-id="85e61-329">Или с помощью заметок к данным:</span><span class="sxs-lookup"><span data-stu-id="85e61-329">Or with data annotations:</span></span>

```C#
[DatabaseGenerated(DatabaseGeneratedOption.Identity)]
public string Id { get; set; }
```

## <a name="iloggerfactory-is-now-a-scoped-service"></a><span data-ttu-id="85e61-330">ILoggerFactory теперь является службой с ограниченной областью действия</span><span class="sxs-lookup"><span data-stu-id="85e61-330">ILoggerFactory is now a scoped service</span></span>

[<span data-ttu-id="85e61-331">Отслеживание вопроса 14698</span><span class="sxs-lookup"><span data-stu-id="85e61-331">Tracking Issue #14698</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14698)

<span data-ttu-id="85e61-332">Это изменение было внесено в предварительную версию 3 EF Core 3.0.</span><span class="sxs-lookup"><span data-stu-id="85e61-332">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="85e61-333">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="85e61-333">**Old behavior**</span></span>

<span data-ttu-id="85e61-334">До выхода EF Core 3.0 `ILoggerFactory` регистрировалась в качестве отдельной службы.</span><span class="sxs-lookup"><span data-stu-id="85e61-334">Before EF Core 3.0, `ILoggerFactory` was registered as a singleton service.</span></span>

<span data-ttu-id="85e61-335">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="85e61-335">**New behavior**</span></span>

<span data-ttu-id="85e61-336">Начиная с EF Core 3.0 `ILoggerFactory` регистрируется в качестве службы с ограниченной областью действия.</span><span class="sxs-lookup"><span data-stu-id="85e61-336">Starting with EF Core 3.0, `ILoggerFactory` is now registered as scoped.</span></span>

<span data-ttu-id="85e61-337">**Причина**</span><span class="sxs-lookup"><span data-stu-id="85e61-337">**Why**</span></span>

<span data-ttu-id="85e61-338">Это изменение было внесено, чтобы разрешить сопоставление средства ведения журнала с экземпляром `DbContext`, что обеспечивает другие функции и устраняет некоторые патологические варианты поведения, такие как взрывной рост внутренних поставщиков служб.</span><span class="sxs-lookup"><span data-stu-id="85e61-338">This change was made to allow association of a logger with a `DbContext` instance, which enables other functionality and removes some cases of pathological behavior such as an explosion of internal service providers.</span></span>

<span data-ttu-id="85e61-339">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="85e61-339">**Mitigations**</span></span>

<span data-ttu-id="85e61-340">Это изменение не должно затрагивать код приложения, если только он не регистрирует и использует настраиваемые службы во внутреннем поставщике служб EF Core.</span><span class="sxs-lookup"><span data-stu-id="85e61-340">This change should not impact application code unless it is registering and using custom services on the EF Core internal service provider.</span></span>
<span data-ttu-id="85e61-341">Это встречается довольно редко.</span><span class="sxs-lookup"><span data-stu-id="85e61-341">This isn't common.</span></span>
<span data-ttu-id="85e61-342">В таких случаях большинство возможностей продолжают работать, но потребуется изменить все отдельные службы, которые зависели от `ILoggerFactory`, чтобы получить `ILoggerFactory` другим способом.</span><span class="sxs-lookup"><span data-stu-id="85e61-342">In these cases, most things will still work, but any singleton service that was depending on `ILoggerFactory` will need to be changed to obtain the `ILoggerFactory` in a different way.</span></span>

<span data-ttu-id="85e61-343">При возникновении подобных ситуаций зарегистрируйте проблему в [средстве отслеживания вопросов EF Core на сайте GitHub](https://github.com/aspnet/EntityFrameworkCore/issues) и сообщите нам, как вы используете `ILoggerFactory`, чтобы мы могли лучше понять, как избежать подобных ошибок в будущем.</span><span class="sxs-lookup"><span data-stu-id="85e61-343">If you run into situations like this, please file an issue at on the [EF Core GitHub issue tracker](https://github.com/aspnet/EntityFrameworkCore/issues) to let us know how you are using `ILoggerFactory` such that we can better understand how not to break this again in the future.</span></span>

## <a name="idbcontextoptionsextensionwithdebuginfo-merged-into-idbcontextoptionsextension"></a><span data-ttu-id="85e61-344">IDbContextOptionsExtensionWithDebugInfo объединен с IDbContextOptionsExtension</span><span class="sxs-lookup"><span data-stu-id="85e61-344">IDbContextOptionsExtensionWithDebugInfo merged into IDbContextOptionsExtension</span></span>

[<span data-ttu-id="85e61-345">Отслеживание вопроса 13552</span><span class="sxs-lookup"><span data-stu-id="85e61-345">Tracking Issue #13552</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/13552)

<span data-ttu-id="85e61-346">Это изменение было внесено в предварительную версию 3 EF Core 3.0.</span><span class="sxs-lookup"><span data-stu-id="85e61-346">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="85e61-347">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="85e61-347">**Old behavior**</span></span>

<span data-ttu-id="85e61-348">`IDbContextOptionsExtensionWithDebugInfo` был дополнительным необязательным интерфейсом, расширяемым из `IDbContextOptionsExtension`, для предотвращения критического изменения в интерфейсе в рамках выпуска версии 2.x.</span><span class="sxs-lookup"><span data-stu-id="85e61-348">`IDbContextOptionsExtensionWithDebugInfo` was an additional optional interface extended from `IDbContextOptionsExtension` to avoid making a breaking change to the interface during the 2.x release cycle.</span></span>

<span data-ttu-id="85e61-349">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="85e61-349">**New behavior**</span></span>

<span data-ttu-id="85e61-350">Теперь эти интерфейсы объединяются в `IDbContextOptionsExtension`.</span><span class="sxs-lookup"><span data-stu-id="85e61-350">The interfaces are now merged together into `IDbContextOptionsExtension`.</span></span>

<span data-ttu-id="85e61-351">**Причина**</span><span class="sxs-lookup"><span data-stu-id="85e61-351">**Why**</span></span>

<span data-ttu-id="85e61-352">Это изменение было внесено, так как интерфейсы представляют собой единое целое.</span><span class="sxs-lookup"><span data-stu-id="85e61-352">This change was made because the interfaces are conceptually one.</span></span>

<span data-ttu-id="85e61-353">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="85e61-353">**Mitigations**</span></span>

<span data-ttu-id="85e61-354">Нужно изменить все реализации `IDbContextOptionsExtension`, чтобы обеспечить поддержку нового члена.</span><span class="sxs-lookup"><span data-stu-id="85e61-354">Any implementations of `IDbContextOptionsExtension` will need to be updated to support the new member.</span></span>

## <a name="lazy-loading-proxies-no-longer-assume-navigation-properties-are-fully-loaded"></a><span data-ttu-id="85e61-355">Прокси с отложенной загрузкой больше не предполагают полную загрузку свойств навигации</span><span class="sxs-lookup"><span data-stu-id="85e61-355">Lazy-loading proxies no longer assume navigation properties are fully loaded</span></span>

[<span data-ttu-id="85e61-356">Отслеживание вопроса 12780</span><span class="sxs-lookup"><span data-stu-id="85e61-356">Tracking Issue #12780</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12780)

<span data-ttu-id="85e61-357">Это изменение будет внесено в предварительную версию 4 EF Core 3.0.</span><span class="sxs-lookup"><span data-stu-id="85e61-357">This change will be introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="85e61-358">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="85e61-358">**Old behavior**</span></span>

<span data-ttu-id="85e61-359">До выхода EF Core 3.0 после удаления `DbContext` не существовало способа узнать, было ли указанное свойство навигации для сущности, полученной из этого контекста, загружено полностью.</span><span class="sxs-lookup"><span data-stu-id="85e61-359">Before EF Core 3.0, once a `DbContext` was disposed there was no way of knowing if a given navigation property on an entity obtained from that context was fully loaded or not.</span></span>
<span data-ttu-id="85e61-360">Вместо это прокси предполагали, что навигация по ссылке загружена, если она имеет отличное NULL значение, а навигация по коллекции загружена, если она не является пустой.</span><span class="sxs-lookup"><span data-stu-id="85e61-360">Proxies would instead assume that a reference navigation is loaded if it has a non-null value, and that a collection navigation is loaded if it isn't empty.</span></span>
<span data-ttu-id="85e61-361">В этом случае попытка отложенной загрузки приводила к холостой операции.</span><span class="sxs-lookup"><span data-stu-id="85e61-361">In these cases, attempting to lazy-load would be a no-op.</span></span>

<span data-ttu-id="85e61-362">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="85e61-362">**New behavior**</span></span>

<span data-ttu-id="85e61-363">Начиная с EF Core 3.0 прокси отслеживают, загружено ли свойство навигации.</span><span class="sxs-lookup"><span data-stu-id="85e61-363">Starting with EF Core 3.0, proxies keep track of whether or not a navigation property is loaded.</span></span>
<span data-ttu-id="85e61-364">Это означает, что попытка обращения к свойству навигации, загружаемому после удаления контекста, всегда будет давать холостую операцию, даже если загруженное свойство навигации пусто или равно NULL.</span><span class="sxs-lookup"><span data-stu-id="85e61-364">This means attempting to access a navigation property that is loaded after the context has been disposed will always be a no-op, even when the loaded navigation is empty or null.</span></span>
<span data-ttu-id="85e61-365">И наоборот, при попытке обращения к незагруженному свойству навигации, когда контекст удален, возникает исключение, даже если это свойство навигации является непустой коллекцией.</span><span class="sxs-lookup"><span data-stu-id="85e61-365">Conversely, attempting to access a navigation property that isn't loaded will throw an exception if the context is disposed even if the navigation property is a non-empty collection.</span></span>
<span data-ttu-id="85e61-366">Подобная ситуация указывает на то, что код приложения пытается использовать отложенную загрузку в недопустимое время, и нужно запретить это, изменив приложение.</span><span class="sxs-lookup"><span data-stu-id="85e61-366">If this situation arises, it means the application code is attempting to use lazy-loading at an invalid time, and the application should be changed to not do this.</span></span>

<span data-ttu-id="85e61-367">**Причина**</span><span class="sxs-lookup"><span data-stu-id="85e61-367">**Why**</span></span>

<span data-ttu-id="85e61-368">Это изменение было внесено, чтобы обеспечить правильное и согласованное поведение при попытке отложенной загрузки для удаленного экземпляра `DbContext`.</span><span class="sxs-lookup"><span data-stu-id="85e61-368">This change was made to make the behavior consistent and correct when attempting to lazy-load on a disposed `DbContext` instance.</span></span>

<span data-ttu-id="85e61-369">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="85e61-369">**Mitigations**</span></span>

<span data-ttu-id="85e61-370">Обновите код приложения, чтобы не выполнять попытку отложенной загрузки с удаленным контекстом, либо настройте соответствующую холостую операцию, как описано в сообщении об исключении.</span><span class="sxs-lookup"><span data-stu-id="85e61-370">Update application code to not attempt lazy-loading with a disposed context, or configure this to be a no-op as described in the exception message.</span></span>

## <a name="excessive-creation-of-internal-service-providers-is-now-an-error-by-default"></a><span data-ttu-id="85e61-371">Чрезмерное создание внутренних поставщиков служб теперь является ошибкой по умолчанию</span><span class="sxs-lookup"><span data-stu-id="85e61-371">Excessive creation of internal service providers is now an error by default</span></span>

[<span data-ttu-id="85e61-372">Отслеживание вопроса 10236</span><span class="sxs-lookup"><span data-stu-id="85e61-372">Tracking Issue #10236</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/10236)

<span data-ttu-id="85e61-373">Это изменение было внесено в предварительную версию 3 EF Core 3.0.</span><span class="sxs-lookup"><span data-stu-id="85e61-373">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="85e61-374">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="85e61-374">**Old behavior**</span></span>

<span data-ttu-id="85e61-375">До выхода EF Core 3.0 для приложения, создающего чрезмерное количество внутренних поставщиков служб, регистрировалось предупреждение.</span><span class="sxs-lookup"><span data-stu-id="85e61-375">Before EF Core 3.0, a warning would be logged for an application creating a pathological number of internal service providers.</span></span>

<span data-ttu-id="85e61-376">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="85e61-376">**New behavior**</span></span>

<span data-ttu-id="85e61-377">Начиная с EF Core 3.0 это предупреждение считается ошибкой и возникает исключение.</span><span class="sxs-lookup"><span data-stu-id="85e61-377">Starting with EF Core 3.0, this warning is now considered and error and an exception is thrown.</span></span> 

<span data-ttu-id="85e61-378">**Причина**</span><span class="sxs-lookup"><span data-stu-id="85e61-378">**Why**</span></span>

<span data-ttu-id="85e61-379">Это изменение было внесено для улучшения кода приложения посредством явного указания на подобный патологический случай.</span><span class="sxs-lookup"><span data-stu-id="85e61-379">This change was made to drive better application code through exposing this pathological case more explicitly.</span></span>

<span data-ttu-id="85e61-380">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="85e61-380">**Mitigations**</span></span>

<span data-ttu-id="85e61-381">При возникновении этой ошибки правильнее всего выявить первопричину и остановить создание такого большого числа внутренних поставщиков служб.</span><span class="sxs-lookup"><span data-stu-id="85e61-381">The most appropriate cause of action on encountering this error is to understand the root cause and stop creating so many internal service providers.</span></span>
<span data-ttu-id="85e61-382">Тем не менее эту ошибку можно преобразовать обратно в предупреждение (или игнорировать) с помощью конфигурации для `DbContextOptionsBuilder`.</span><span class="sxs-lookup"><span data-stu-id="85e61-382">However, the error can be converted back to a warning (or ignored) via configuration on the `DbContextOptionsBuilder`.</span></span>
<span data-ttu-id="85e61-383">Например:</span><span class="sxs-lookup"><span data-stu-id="85e61-383">For example:</span></span>

```C#
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
{
    optionsBuilder
        .ConfigureWarnings(w => w.Log(CoreEventId.ManyServiceProvidersCreatedWarning));
}
```

## <a name="new-behavior-for-hasonehasmany-called-with-a-single-string"></a><span data-ttu-id="85e61-384">Новое поведение для вызова HasOne/HasMany с одной строкой</span><span class="sxs-lookup"><span data-stu-id="85e61-384">New behavior for HasOne/HasMany called with a single string</span></span>

[<span data-ttu-id="85e61-385">Отслеживание вопроса № 9171</span><span class="sxs-lookup"><span data-stu-id="85e61-385">Tracking Issue #9171</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9171)

<span data-ttu-id="85e61-386">Это изменение будет внесено в предварительную версию 4 EF Core 3.0.</span><span class="sxs-lookup"><span data-stu-id="85e61-386">This change will be introduced in EF Core 3.0-preview 4.</span></span>

<span data-ttu-id="85e61-387">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="85e61-387">**Old behavior**</span></span>

<span data-ttu-id="85e61-388">До версии EF Core 3.0 интерпретация кода с вызовом `HasOne` или `HasMany` с одной строкой была очень запутанной.</span><span class="sxs-lookup"><span data-stu-id="85e61-388">Before EF Core 3.0, code calling `HasOne` or `HasMany` with a single string was interpretted in a confusing way.</span></span>
<span data-ttu-id="85e61-389">Например:</span><span class="sxs-lookup"><span data-stu-id="85e61-389">For example:</span></span>
```C#
modelBuilder.Entity<Samurai>().HasOne("Entrance").WithOne();
```

<span data-ttu-id="85e61-390">Складывается впечатление, что код связывает `Samuri` с другим типом сущности с помощью свойства навигации `Entrance`, которое может быть закрытым.</span><span class="sxs-lookup"><span data-stu-id="85e61-390">The code looks like it is relating `Samuri` to some other entity type using the `Entrance` navigation property, which may be private.</span></span>

<span data-ttu-id="85e61-391">На самом деле этот код пытается создать отношение с некоторым типом сущности под именем `Entrance`, который не имеет свойства навигации.</span><span class="sxs-lookup"><span data-stu-id="85e61-391">In reality, this code attempts to create a relationship to some entity type called `Entrance` with no navigation property.</span></span>

<span data-ttu-id="85e61-392">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="85e61-392">**New behavior**</span></span>

<span data-ttu-id="85e61-393">Начиная с EF Core 3.0, указанный выше код выполняет те действия, которые должен был выполнять раньше.</span><span class="sxs-lookup"><span data-stu-id="85e61-393">Starting with EF Core 3.0, the code above now does what it looked like it should have been doing before.</span></span>

<span data-ttu-id="85e61-394">**Причина**</span><span class="sxs-lookup"><span data-stu-id="85e61-394">**Why**</span></span>

<span data-ttu-id="85e61-395">Старое поведение создавало большую путаницу, особенно при считывании кода конфигурации и при поиске ошибок.</span><span class="sxs-lookup"><span data-stu-id="85e61-395">The old behavior was very confusing, especially when reading the configuration code and looking for errors.</span></span>

<span data-ttu-id="85e61-396">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="85e61-396">**Mitigations**</span></span>

<span data-ttu-id="85e61-397">Это приведет к нарушению работы только тех приложений, которые явно настраивают отношения с помощью строк для имен типов и не указывают явно свойство навигации.</span><span class="sxs-lookup"><span data-stu-id="85e61-397">This will only break applications that are explicitly configuring relationships using strings for type names and without specifying the navigation property explicitly.</span></span>
<span data-ttu-id="85e61-398">Такой подход используется нечасто.</span><span class="sxs-lookup"><span data-stu-id="85e61-398">This is not common.</span></span>
<span data-ttu-id="85e61-399">Прежнее поведение можно реализовать с помощью явной передачи значения `null` для имени свойства навигации.</span><span class="sxs-lookup"><span data-stu-id="85e61-399">The previous behavior can be obtained through explicitly passing `null` for the navigation property name.</span></span>
<span data-ttu-id="85e61-400">Например:</span><span class="sxs-lookup"><span data-stu-id="85e61-400">For example:</span></span>

```C#
modelBuilder.Entity<Samurai>().HasOne("Some.Entity.Type.Name", null).WithOne();
```

## <a name="the-relationaltypemapping-annotation-is-now-just-typemapping"></a><span data-ttu-id="85e61-401">Заметка Relational:TypeMapping теперь является просто TypeMapping</span><span class="sxs-lookup"><span data-stu-id="85e61-401">The Relational:TypeMapping annotation is now just TypeMapping</span></span>

[<span data-ttu-id="85e61-402">Отслеживание вопроса 9913</span><span class="sxs-lookup"><span data-stu-id="85e61-402">Tracking Issue #9913</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/9913)

<span data-ttu-id="85e61-403">Это изменение было внесено в предварительную версию 2 EF Core 3.0.</span><span class="sxs-lookup"><span data-stu-id="85e61-403">This change was introduced in EF Core 3.0-preview 2.</span></span>

<span data-ttu-id="85e61-404">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="85e61-404">**Old behavior**</span></span>

<span data-ttu-id="85e61-405">Для заметок сопоставлений типов использовалось имя "Relational:TypeMapping".</span><span class="sxs-lookup"><span data-stu-id="85e61-405">The annotation name for type mapping annotations was "Relational:TypeMapping".</span></span>

<span data-ttu-id="85e61-406">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="85e61-406">**New behavior**</span></span>

<span data-ttu-id="85e61-407">Теперь для заметок сопоставлений типов используется имя "TypeMapping".</span><span class="sxs-lookup"><span data-stu-id="85e61-407">The annotation name for type mapping annotations is now "TypeMapping".</span></span>

<span data-ttu-id="85e61-408">**Причина**</span><span class="sxs-lookup"><span data-stu-id="85e61-408">**Why**</span></span>

<span data-ttu-id="85e61-409">Сопоставления типов теперь используются не только для поставщиков реляционных баз данных.</span><span class="sxs-lookup"><span data-stu-id="85e61-409">Type mappings are now used for more than just relational database providers.</span></span>

<span data-ttu-id="85e61-410">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="85e61-410">**Mitigations**</span></span>

<span data-ttu-id="85e61-411">Это изменение нарушит работу только тех приложений, которые обращаются к сопоставлению типов непосредственно по заметке, что встречается довольно редко.</span><span class="sxs-lookup"><span data-stu-id="85e61-411">This will only break applications that access the type mapping directly as an annotation, which isn't common.</span></span>
<span data-ttu-id="85e61-412">Наиболее подходящий способ для устранения этой проблемы заключается в использовании поверхности API для доступа к сопоставлениям типов вместо прямого использования заметки.</span><span class="sxs-lookup"><span data-stu-id="85e61-412">The most appropriate action to fix is to use API surface to access type mappings rather than using the annotation directly.</span></span>

## <a name="totable-on-a-derived-type-throws-an-exception"></a><span data-ttu-id="85e61-413">ToTable для производного типа выдает исключение</span><span class="sxs-lookup"><span data-stu-id="85e61-413">ToTable on a derived type throws an exception</span></span> 

[<span data-ttu-id="85e61-414">Отслеживание вопроса 11811</span><span class="sxs-lookup"><span data-stu-id="85e61-414">Tracking Issue #11811</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/11811)

<span data-ttu-id="85e61-415">Это изменение было внесено в предварительную версию 3 EF Core 3.0.</span><span class="sxs-lookup"><span data-stu-id="85e61-415">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="85e61-416">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="85e61-416">**Old behavior**</span></span>

<span data-ttu-id="85e61-417">До выхода EF Core 3.0 вызов `ToTable()` для производного типа игнорировался, так как единственной стратегией сопоставления наследования был метод "одна таблица на иерархию", где такая операция недействительна.</span><span class="sxs-lookup"><span data-stu-id="85e61-417">Before EF Core 3.0, `ToTable()` called on a derived type would be ignored since only inheritance mapping strategy was TPH where this isn't valid.</span></span> 

<span data-ttu-id="85e61-418">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="85e61-418">**New behavior**</span></span>

<span data-ttu-id="85e61-419">Начиная с EF Core 3.0 и в рамках подготовки для добавления поддержки методов "одна таблица на тип" и "одна таблица на каждый отдельный тип/класс" в последующем выпуске вызов `ToTable()` для производного типа теперь будет выдавать исключение, чтобы избежать непредвиденного изменения сопоставления в будущем.</span><span class="sxs-lookup"><span data-stu-id="85e61-419">Starting with EF Core 3.0 and in preparation for adding TPT and TPC support in a later release, `ToTable()` called on a derived type will now throw an exception to avoid an unexpected mapping change in the future.</span></span>

<span data-ttu-id="85e61-420">**Причина**</span><span class="sxs-lookup"><span data-stu-id="85e61-420">**Why**</span></span>

<span data-ttu-id="85e61-421">Сейчас сопоставлять производный тип с другой таблицей запрещено.</span><span class="sxs-lookup"><span data-stu-id="85e61-421">Currently it isn't valid to map a derived type to a different table.</span></span>
<span data-ttu-id="85e61-422">Это изменение позволяет избежать нарушения работы в будущем, когда подобная операция будет разрешена.</span><span class="sxs-lookup"><span data-stu-id="85e61-422">This change avoids breaking in the future when it becomes a valid thing to do.</span></span>

<span data-ttu-id="85e61-423">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="85e61-423">**Mitigations**</span></span>

<span data-ttu-id="85e61-424">Удалите все попытки сопоставить производные типы с другими таблицами.</span><span class="sxs-lookup"><span data-stu-id="85e61-424">Remove any attempts to map derived types to other tables.</span></span>

## <a name="forsqlserverhasindex-replaced-with-hasindex"></a><span data-ttu-id="85e61-425">ForSqlServerHasIndex заменен на HasIndex</span><span class="sxs-lookup"><span data-stu-id="85e61-425">ForSqlServerHasIndex replaced with HasIndex</span></span> 

[<span data-ttu-id="85e61-426">Отслеживание вопроса 12366</span><span class="sxs-lookup"><span data-stu-id="85e61-426">Tracking Issue #12366</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12366)

<span data-ttu-id="85e61-427">Это изменение было внесено в предварительную версию 3 EF Core 3.0.</span><span class="sxs-lookup"><span data-stu-id="85e61-427">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="85e61-428">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="85e61-428">**Old behavior**</span></span>

<span data-ttu-id="85e61-429">До выхода EF Core 3.0 `ForSqlServerHasIndex().ForSqlServerInclude()` предоставлял способ настройки столбцов, используемых с `INCLUDE`.</span><span class="sxs-lookup"><span data-stu-id="85e61-429">Before EF Core 3.0, `ForSqlServerHasIndex().ForSqlServerInclude()` provided a way to configure columns used with `INCLUDE`.</span></span>

<span data-ttu-id="85e61-430">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="85e61-430">**New behavior**</span></span>

<span data-ttu-id="85e61-431">Начиная с EF Core 3.0 использование `Include` для индекса теперь поддерживается на реляционном уровне.</span><span class="sxs-lookup"><span data-stu-id="85e61-431">Starting with EF Core 3.0, using `Include` on an index is now supported at the relational level.</span></span>
<span data-ttu-id="85e61-432">Используйте ключевое слово `HasIndex().ForSqlServerInclude()`.</span><span class="sxs-lookup"><span data-stu-id="85e61-432">Use `HasIndex().ForSqlServerInclude()`.</span></span>

<span data-ttu-id="85e61-433">**Причина**</span><span class="sxs-lookup"><span data-stu-id="85e61-433">**Why**</span></span>

<span data-ttu-id="85e61-434">Это изменение было внесено, чтобы консолидировать API для индексов с `Includes` в одном месте для всех поставщиков баз данных.</span><span class="sxs-lookup"><span data-stu-id="85e61-434">This change was made to consolidate the API for indexes with `Includes` into one place for all database providers.</span></span>

<span data-ttu-id="85e61-435">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="85e61-435">**Mitigations**</span></span>

<span data-ttu-id="85e61-436">Используйте новый API, как показано выше.</span><span class="sxs-lookup"><span data-stu-id="85e61-436">Use the new API, as shown above.</span></span>

## <a name="ef-core-no-longer-sends-pragma-for-sqlite-fk-enforcement"></a><span data-ttu-id="85e61-437">EF Core больше не отправляет pragma для принудительного применения FK SQLite</span><span class="sxs-lookup"><span data-stu-id="85e61-437">EF Core no longer sends pragma for SQLite FK enforcement</span></span>

[<span data-ttu-id="85e61-438">Отслеживание вопроса 12151</span><span class="sxs-lookup"><span data-stu-id="85e61-438">Tracking Issue #12151</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12151)

<span data-ttu-id="85e61-439">Это изменение было внесено в предварительную версию 3 EF Core 3.0.</span><span class="sxs-lookup"><span data-stu-id="85e61-439">This change was introduced in EF Core 3.0-preview 3.</span></span>

<span data-ttu-id="85e61-440">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="85e61-440">**Old behavior**</span></span>

<span data-ttu-id="85e61-441">До выхода версии 3.0 система EF Core отправляла `PRAGMA foreign_keys = 1` при открытии соединения с SQLite.</span><span class="sxs-lookup"><span data-stu-id="85e61-441">Before EF Core 3.0, EF Core would send `PRAGMA foreign_keys = 1` when a connection to SQLite is opened.</span></span>

<span data-ttu-id="85e61-442">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="85e61-442">**New behavior**</span></span>

<span data-ttu-id="85e61-443">Начиная с версии 3.0 система EF Core больше не отправляет `PRAGMA foreign_keys = 1` при открытии соединения с SQLite.</span><span class="sxs-lookup"><span data-stu-id="85e61-443">Starting with EF Core 3.0, EF Core no longer sends `PRAGMA foreign_keys = 1` when a connection to SQLite is opened.</span></span>

<span data-ttu-id="85e61-444">**Причина**</span><span class="sxs-lookup"><span data-stu-id="85e61-444">**Why**</span></span>

<span data-ttu-id="85e61-445">Это изменение было внесено, так как EF Core по умолчанию использует `SQLitePCLRaw.bundle_e_sqlite3`, что, в свою очередь, означает, что принудительное применение FK по умолчанию включено и его не требуется включать каждый раз при открытии соединения.</span><span class="sxs-lookup"><span data-stu-id="85e61-445">This change was made because EF Core uses `SQLitePCLRaw.bundle_e_sqlite3` by default, which in turn means that FK enforcement is switched on by default and doesn't need to be explicitly enabled each time a connection is opened.</span></span>

<span data-ttu-id="85e61-446">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="85e61-446">**Mitigations**</span></span>

<span data-ttu-id="85e61-447">Внешние ключи включены по умолчанию в SQLitePCLRaw.bundle_e_sqlite3, который по умолчанию используется для EF Core.</span><span class="sxs-lookup"><span data-stu-id="85e61-447">Foreign keys are enabled by default in SQLitePCLRaw.bundle_e_sqlite3, which is used by default for EF Core.</span></span>
<span data-ttu-id="85e61-448">В других случаях внешние ключи можно включить, указав `Foreign Keys=True` в строке подключения.</span><span class="sxs-lookup"><span data-stu-id="85e61-448">For other cases, foreign keys can be enabled by specifying `Foreign Keys=True` in your connection string.</span></span>

## <a name="microsoftentityframeworkcoresqlite-now-depends-on-sqlitepclrawbundleesqlite3"></a><span data-ttu-id="85e61-449">Теперь Microsoft.EntityFrameworkCore.Sqlite зависит от SQLitePCLRaw.bundle_e_sqlite3</span><span class="sxs-lookup"><span data-stu-id="85e61-449">Microsoft.EntityFrameworkCore.Sqlite now depends on SQLitePCLRaw.bundle_e_sqlite3</span></span>

<span data-ttu-id="85e61-450">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="85e61-450">**Old behavior**</span></span>

<span data-ttu-id="85e61-451">До выхода версии 3.0 система EF Core использовала `SQLitePCLRaw.bundle_green`.</span><span class="sxs-lookup"><span data-stu-id="85e61-451">Before EF Core 3.0, EF Core used `SQLitePCLRaw.bundle_green`.</span></span>

<span data-ttu-id="85e61-452">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="85e61-452">**New behavior**</span></span>

<span data-ttu-id="85e61-453">Начиная с версии 3.0 система EF Core использует `SQLitePCLRaw.bundle_e_sqlite3`.</span><span class="sxs-lookup"><span data-stu-id="85e61-453">Starting with EF Core 3.0, EF Core uses `SQLitePCLRaw.bundle_e_sqlite3`.</span></span>

<span data-ttu-id="85e61-454">**Причина**</span><span class="sxs-lookup"><span data-stu-id="85e61-454">**Why**</span></span>

<span data-ttu-id="85e61-455">Это изменение было внесено, чтобы версия SQLite, используемая в iOS, была согласована с другими платформами.</span><span class="sxs-lookup"><span data-stu-id="85e61-455">This change was made so that the version of SQLite used on iOS consistent with other platforms.</span></span>

<span data-ttu-id="85e61-456">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="85e61-456">**Mitigations**</span></span>

<span data-ttu-id="85e61-457">Чтобы использовать собственную версию SQLite в iOS, настройте `Microsoft.Data.Sqlite` для использования другого пакета `SQLitePCLRaw`.</span><span class="sxs-lookup"><span data-stu-id="85e61-457">To use the native SQLite version on iOS, configure `Microsoft.Data.Sqlite` to use a different `SQLitePCLRaw` bundle.</span></span>
