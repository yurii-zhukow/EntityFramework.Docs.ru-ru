---
title: Новые возможности в EF Core 3.0 — EF Core
author: divega
ms.date: 02/19/2019
ms.assetid: 2EBE2CCC-E52D-483F-834C-8877F5EB0C0C
uid: core/what-is-new/ef-core-3.0/features
ms.openlocfilehash: b6774f615b04bf9579aac5dea217e7321631da0c
ms.sourcegitcommit: a709054b2bc7a8365201d71f59325891aacd315f
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/14/2019
ms.locfileid: "57829191"
---
# <a name="new-features-included-in-ef-core-30-currently-in-preview"></a><span data-ttu-id="8890b-102">Новые функции в EF Core 3.0 (сейчас предоставляются в виде предварительной версии)</span><span class="sxs-lookup"><span data-stu-id="8890b-102">New features included in EF Core 3.0 (currently in preview)</span></span>

> [!IMPORTANT]
> <span data-ttu-id="8890b-103">Обратите внимание на то, что наборы возможностей и расписания будущих выпусков могут быть изменены, и, несмотря на то, что мы стараемся поддерживать эту страницу в актуальном состоянии, в определенные моменты времени содержание страницы может не отражать наши текущие планы.</span><span class="sxs-lookup"><span data-stu-id="8890b-103">Please note that the feature sets and schedules of future releases are always subject to change, and although we will try to keep this page up to date, it may not reflect our latest plans at all times.</span></span>

<span data-ttu-id="8890b-104">В следующем списке приведены ключевые новые функции, запланированные для реализации в EF Core 3.0.</span><span class="sxs-lookup"><span data-stu-id="8890b-104">The following list includes the major new features planned for EF Core 3.0.</span></span>
<span data-ttu-id="8890b-105">Большинство этих функций не включаются в текущую предварительную версию, но будут доступны по мере продвижения к RTM.</span><span class="sxs-lookup"><span data-stu-id="8890b-105">Most of these features are not included in the current preview, but will become available as we make progress towards RTM.</span></span>

<span data-ttu-id="8890b-106">Это вызвано тем, что в начале работы над выпуском мы сосредоточились на внедрении запланированных [критических изменений](xref:core/what-is-new/ef-core-3.0/breaking-changes).</span><span class="sxs-lookup"><span data-stu-id="8890b-106">The reason is that at the beginning of the release we are focusing on implementing planned [breaking changes](xref:core/what-is-new/ef-core-3.0/breaking-changes).</span></span>
<span data-ttu-id="8890b-107">Многие из этих критических изменений сами по себе являются усовершенствованиями EF Core.</span><span class="sxs-lookup"><span data-stu-id="8890b-107">Many of these breaking changes are improvements to EF Core on their own.</span></span>
<span data-ttu-id="8890b-108">Многие другие необходимы для реализации дополнительных улучшений.</span><span class="sxs-lookup"><span data-stu-id="8890b-108">Many others are required to unblock further improvements.</span></span> 

<span data-ttu-id="8890b-109">Полный список исправлений ошибок и усовершенствований, над которыми ведется работа, можно просмотреть в [этом запросе в средстве отслеживания вопросов](https://github.com/aspnet/EntityFrameworkCore/issues?q=is%3Aopen+is%3Aissue+milestone%3A3.0.0+sort%3Areactions-%2B1-desc).</span><span class="sxs-lookup"><span data-stu-id="8890b-109">For a complete list of bug fixes and enhancements underway, you can see [this query in our issue tracker](https://github.com/aspnet/EntityFrameworkCore/issues?q=is%3Aopen+is%3Aissue+milestone%3A3.0.0+sort%3Areactions-%2B1-desc).</span></span>

## <a name="linq-improvements"></a><span data-ttu-id="8890b-110">Улучшения LINQ</span><span class="sxs-lookup"><span data-stu-id="8890b-110">LINQ improvements</span></span> 

[<span data-ttu-id="8890b-111">Отслеживание вопроса 12795</span><span class="sxs-lookup"><span data-stu-id="8890b-111">Tracking Issue #12795</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/12795)

<span data-ttu-id="8890b-112">Работа над этой функцией началась, однако она еще не включена в текущую предварительную версию.</span><span class="sxs-lookup"><span data-stu-id="8890b-112">Work on this feature has started but it isn't included in the current preview.</span></span>

<span data-ttu-id="8890b-113">LINQ позволяет писать запросы к базам данных прямо в коде вашего языка программирования, используя различные типы форматированных данных, функции IntelliSense и проверку типа во время компиляции.</span><span class="sxs-lookup"><span data-stu-id="8890b-113">LINQ enables you to write database queries without leaving your language of choice, taking advantage of rich type information to get IntelliSense and compile-time type checking.</span></span>
<span data-ttu-id="8890b-114">Однако LINQ также поддерживает написание неограниченного числа сложных запросов, и поставщикам LINQ всегда было сложно обеспечить работу этой функции.</span><span class="sxs-lookup"><span data-stu-id="8890b-114">But LINQ also enables you to write an unlimited number of complicated queries, and that has always been a huge challenge for LINQ providers.</span></span>
<span data-ttu-id="8890b-115">В ранних версиях EF Core эта проблема в какой-то мере решалась переводом определенных частей запроса на язык SQL и выполнением остальных частей в памяти на клиенте.</span><span class="sxs-lookup"><span data-stu-id="8890b-115">In the first few versions of EF Core, we solved that in part by figuring out what portions of a query could be translated to SQL, and then by allowing the rest of the query to execute in memory on the client.</span></span>
<span data-ttu-id="8890b-116">Выполнение на стороне клиента в некоторых ситуациях дает хороший результат, но во многих случаях могут создаваться неэффективные запросы, которые будут выявлены только при развертывании приложения в рабочей среде.</span><span class="sxs-lookup"><span data-stu-id="8890b-116">This client-side execution can be desirable in some situations, but in many other cases it can result in inefficient queries that may not be identified until an application is deployed to production.</span></span>
<span data-ttu-id="8890b-117">В EF Core 3.0 мы планируем основательно изменить нашу реализацию LINQ и принципы ее тестирования.</span><span class="sxs-lookup"><span data-stu-id="8890b-117">In EF Core 3.0, we're planning to make profound changes to how our LINQ implementation works, and how we test it.</span></span>
<span data-ttu-id="8890b-118">Мы хотим сделать ее более надежной (например, чтобы выпуски исправлений не нарушали работу старых запросов), обеспечить корректный перевод на язык SQL еще большего числа выражений, реализовать создание эффективных запросов для еще большего числа сценариев, а также улучшить обнаружение неэффективных запросов.</span><span class="sxs-lookup"><span data-stu-id="8890b-118">The goals are to make it more robust (for example, to avoid breaking queries in patch releases), to enable translating more expressions correctly into SQL, to generate efficient queries in more cases, and to prevent inefficient queries from going undetected.</span></span>

## <a name="cosmos-db-support"></a><span data-ttu-id="8890b-119">Поддержка Cosmos DB</span><span class="sxs-lookup"><span data-stu-id="8890b-119">Cosmos DB support</span></span> 

[<span data-ttu-id="8890b-120">Отслеживание вопроса 8443</span><span class="sxs-lookup"><span data-stu-id="8890b-120">Tracking Issue #8443</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/8443)

<span data-ttu-id="8890b-121">Эта функция включена в текущую предварительную версию, но еще не готова.</span><span class="sxs-lookup"><span data-stu-id="8890b-121">This feature is included in the current preview, but isn't complete yet.</span></span> 

<span data-ttu-id="8890b-122">Мы создаем для EF Core поставщик Cosmos DB, который позволит разработчикам, знакомым с моделью программирования EF, легко использовать Azure Cosmos DB в качестве базы данных приложения.</span><span class="sxs-lookup"><span data-stu-id="8890b-122">We're working on a Cosmos DB provider for EF Core, to enable developers familiar with the EF programing model to easily target Azure Cosmos DB as an application database.</span></span>
<span data-ttu-id="8890b-123">Мы хотим, чтобы такие преимущества Cosmos DB, как глобальное распределение, постоянная готовность, эластичная масштабируемость и низкая задержка, стали еще доступнее .NET-разработчикам.</span><span class="sxs-lookup"><span data-stu-id="8890b-123">The goal is to make some of the advantages of Cosmos DB, like global distribution, "always on" availability, elastic scalability, and low latency, even more accessible to .NET developers.</span></span>
<span data-ttu-id="8890b-124">Поставщик позволит использовать API-интерфейс SQL в Cosmos DB с большинством функций EF Core, включая автоматическое отслеживание изменений, LINQ и преобразование значений.</span><span class="sxs-lookup"><span data-stu-id="8890b-124">The provider will enable most EF Core features, like automatic change tracking, LINQ, and value conversions, against the SQL API in Cosmos DB.</span></span>
<span data-ttu-id="8890b-125">Подготовка этих возможностей началась еще до выпуска EF Core 2.2, и [мы уже публиковали предварительные версии поставщика](https://blogs.msdn.microsoft.com/dotnet/2018/10/17/announcing-entity-framework-core-2-2-preview-3/).</span><span class="sxs-lookup"><span data-stu-id="8890b-125">We started this effort before EF Core 2.2, and [we have made some preview versions of the provider available](https://blogs.msdn.microsoft.com/dotnet/2018/10/17/announcing-entity-framework-core-2-2-preview-3/).</span></span>
<span data-ttu-id="8890b-126">Сейчас мы планируем продолжать его разработку вместе с разработкой EF Core 3.0.</span><span class="sxs-lookup"><span data-stu-id="8890b-126">The new plan is to continue developing the provider alongside EF Core 3.0.</span></span> 

## <a name="c-80-support"></a><span data-ttu-id="8890b-127">Поддержка C# 8.0</span><span class="sxs-lookup"><span data-stu-id="8890b-127">C# 8.0 support</span></span>

<span data-ttu-id="8890b-128">[Отслеживание вопроса 12047](https://github.com/aspnet/EntityFrameworkCore/issues/12047)
[Отслеживание вопроса 10347](https://github.com/aspnet/EntityFrameworkCore/issues/10347)</span><span class="sxs-lookup"><span data-stu-id="8890b-128">[Tracking Issue #12047](https://github.com/aspnet/EntityFrameworkCore/issues/12047)
[Tracking Issue #10347](https://github.com/aspnet/EntityFrameworkCore/issues/10347)</span></span>

<span data-ttu-id="8890b-129">Работа над этой функцией началась, однако она еще не включена в текущую предварительную версию.</span><span class="sxs-lookup"><span data-stu-id="8890b-129">Work on this feature has started but it isn't included in the current preview.</span></span>

<span data-ttu-id="8890b-130">Мы хотим предоставить клиентам возможность использовать в EF Core некоторые [будущие функции C# 8.0](https://blogs.msdn.microsoft.com/dotnet/2018/11/12/building-c-8-0/), например асинхронные потоки (включая `await foreach`) и ссылочные типы, допускающие значение null.</span><span class="sxs-lookup"><span data-stu-id="8890b-130">We want our customers to take advantage of some of the [new features coming in C# 8.0](https://blogs.msdn.microsoft.com/dotnet/2018/11/12/building-c-8-0/) like async streams (including `await foreach`) and nullable reference types while using EF Core.</span></span>

## <a name="reverse-engineering-of-database-views"></a><span data-ttu-id="8890b-131">Реконструирование представлений базы данных</span><span class="sxs-lookup"><span data-stu-id="8890b-131">Reverse engineering of database views</span></span>

[<span data-ttu-id="8890b-132">Отслеживание вопроса 1679</span><span class="sxs-lookup"><span data-stu-id="8890b-132">Tracking Issue #1679</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/1679)

<span data-ttu-id="8890b-133">Эта функция не включена в текущую предварительную версию.</span><span class="sxs-lookup"><span data-stu-id="8890b-133">This feature isn't included in the current preview.</span></span>

<span data-ttu-id="8890b-134">[Типы запросов](xref:core/modeling/query-types), представленные в EF Core 2.1 и учитывающие типы сущностей без ключей в EF Core 3.0, представляют данные, которые можно считать из базы данных, но невозможно изменить.</span><span class="sxs-lookup"><span data-stu-id="8890b-134">[Query types](xref:core/modeling/query-types), introduced in EF Core 2.1 and considered entity types without keys in EF Core 3.0, represent data that can be read from the database, but cannot be updated.</span></span>
<span data-ttu-id="8890b-135">Благодаря этой особенности они отлично подходят для представлений базы данных в большинстве сценариев, поэтому мы планируем автоматизировать создание типов сущностей без ключей при реконструировании представлений базы данных.</span><span class="sxs-lookup"><span data-stu-id="8890b-135">This characteristic makes them an excellent fit for database views in most scenarios, so we plan to automate the creation of entity types without keys when reverse engineering database views.</span></span>

## <a name="property-bag-entities"></a><span data-ttu-id="8890b-136">Сущности контейнера свойств</span><span class="sxs-lookup"><span data-stu-id="8890b-136">Property bag entities</span></span> 

<span data-ttu-id="8890b-137">[Отслеживание вопроса 13610](https://github.com/aspnet/EntityFrameworkCore/issues/13610) и [9914](https://github.com/aspnet/EntityFrameworkCore/issues/9914)</span><span class="sxs-lookup"><span data-stu-id="8890b-137">[Tracking Issue #13610](https://github.com/aspnet/EntityFrameworkCore/issues/13610) and [#9914](https://github.com/aspnet/EntityFrameworkCore/issues/9914)</span></span>

<span data-ttu-id="8890b-138">Работа над этой функцией началась, однако она еще не включена в текущую предварительную версию.</span><span class="sxs-lookup"><span data-stu-id="8890b-138">Work on this feature has started but it isn't included in the current preview.</span></span> 

<span data-ttu-id="8890b-139">Эта функция позволяет использовать сущности, хранящие данные в свойствах с индексацией, а также представлять в модели EF Core разные типы сущностей при помощи экземпляров одного и того же класса .NET (потенциально в простейшем виде `Dictionary<string, object>`).</span><span class="sxs-lookup"><span data-stu-id="8890b-139">This feature is about enabling entities that store data in indexed properties instead of regular properties, and also about being able to use instances of the same .NET class (potentially something as simple as a `Dictionary<string, object>`) to represent different entity types in the same EF Core model.</span></span>
<span data-ttu-id="8890b-140">В будущем эта функция позволит реализовать связи "многие ко многим" без сущности объединения ([вопрос № 1368](https://github.com/aspnet/EntityFrameworkCore/issues/1368)). Это одна из тех возможностей, о которых нас чаще всего просят пользователи EF Core.</span><span class="sxs-lookup"><span data-stu-id="8890b-140">This feature is a stepping stone to support many-to-many relationships without a join entity ([issue #1368](https://github.com/aspnet/EntityFrameworkCore/issues/1368)), which is one of the most requested improvements for EF Core.</span></span>

## <a name="ef-63-on-net-core"></a><span data-ttu-id="8890b-141">EF 6.3 на платформе .NET Core</span><span class="sxs-lookup"><span data-stu-id="8890b-141">EF 6.3 on .NET Core</span></span> 

[<span data-ttu-id="8890b-142">Отслеживание вопроса EF6#271</span><span class="sxs-lookup"><span data-stu-id="8890b-142">Tracking Issue EF6#271</span></span>](https://github.com/aspnet/EntityFramework6/issues/271)

<span data-ttu-id="8890b-143">Работа над этой функцией началась, однако она еще не включена в текущую предварительную версию.</span><span class="sxs-lookup"><span data-stu-id="8890b-143">Work on this feature has started but it isn't included in the current preview.</span></span> 

<span data-ttu-id="8890b-144">Мы понимаем, что многие существующие приложения используют предыдущие версии EF и перенос этих приложений на EF Core исключительно для поддержки функций .NET Core может оказаться очень трудоемким.</span><span class="sxs-lookup"><span data-stu-id="8890b-144">We understand that many existing applications use previous versions of EF, and that porting them to EF Core only to take advantage of .NET Core can sometimes require a significant effort.</span></span>
<span data-ttu-id="8890b-145">По этой причине мы планируем адаптировать следующую версию EF 6 к работе на платформе .NET Core 3.0.</span><span class="sxs-lookup"><span data-stu-id="8890b-145">For that reason, we will be adapting the next version of EF 6 to run on .NET Core 3.0.</span></span>
<span data-ttu-id="8890b-146">Это делается для того, чтобы перенос существующих приложений был прост и требовал лишь минимальных изменений.</span><span class="sxs-lookup"><span data-stu-id="8890b-146">We are doing this to facilitate porting existing applications with minimal changes.</span></span>
<span data-ttu-id="8890b-147">Однако имеются и некоторые ограничения.</span><span class="sxs-lookup"><span data-stu-id="8890b-147">There are going to be some limitations.</span></span> <span data-ttu-id="8890b-148">Например:</span><span class="sxs-lookup"><span data-stu-id="8890b-148">For example:</span></span>
- <span data-ttu-id="8890b-149">Потребность в новых поставщиках для работы с другими базами данных, кроме предусмотренной в .NET Core поддержки SQL Server.</span><span class="sxs-lookup"><span data-stu-id="8890b-149">It will require new providers to work with other databases besides the included SQL Server support on .NET Core</span></span>
- <span data-ttu-id="8890b-150">Поддержка пространственных индексов в SQL Server не будет реализована.</span><span class="sxs-lookup"><span data-stu-id="8890b-150">Spatial support with SQL Server won't be enabled</span></span>

<span data-ttu-id="8890b-151">Кроме того, обратите внимание, что никаких новых функций для EF 6 сейчас не запланировано.</span><span class="sxs-lookup"><span data-stu-id="8890b-151">Note also that there are no new features planned for EF 6 at this point.</span></span>
