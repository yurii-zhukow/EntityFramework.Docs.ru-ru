---
title: Тестирование компонентов с помощью EF Core — EF Core
description: Различные подходы к тестированию приложений, использующих EF Core.
author: ajcvickers
ms.date: 03/23/2020
uid: core/miscellaneous/testing/index
ms.openlocfilehash: b1ab37ebb0a3aae09d5d5b225f746cf83dfba170
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/07/2020
ms.locfileid: "80634255"
---
# <a name="testing-code-that-uses-ef-core"></a><span data-ttu-id="e0028-103">Тестирование кода, использующего EF Core</span><span class="sxs-lookup"><span data-stu-id="e0028-103">Testing code that uses EF Core</span></span>

<span data-ttu-id="e0028-104">Тестирование кода, обращающегося к базе данных, требует одного из следующих вариантов:</span><span class="sxs-lookup"><span data-stu-id="e0028-104">Testing code that accesses a database requires either:</span></span>
* <span data-ttu-id="e0028-105">Выполнение запросов и обновлений в той же СУБД, что используется в рабочей среде.</span><span class="sxs-lookup"><span data-stu-id="e0028-105">Running queries and updates against the same database system used in production.</span></span>
* <span data-ttu-id="e0028-106">Выполнение запросов и обновлений в другой СУБД, более простой для управления.</span><span class="sxs-lookup"><span data-stu-id="e0028-106">Running queries and updates against some other easier to manage database system.</span></span>
* <span data-ttu-id="e0028-107">Применение дублеров теста или иного механизма, позволяющего не использовать базу данных вообще.</span><span class="sxs-lookup"><span data-stu-id="e0028-107">Using test doubles or some other mechanism to avoid using a database at all.</span></span>

<span data-ttu-id="e0028-108">В этом документе описаны компромиссы, связанные с каждым из этих вариантов, и показано использование EF Core в каждом подходе.</span><span class="sxs-lookup"><span data-stu-id="e0028-108">This document outlines the trade offs involved in each of these choices and shows how EF Core can be used with each approach.</span></span>  

## <a name="all-database-providers-are-not-equal"></a><span data-ttu-id="e0028-109">Поставщики баз данных неодинаковы</span><span class="sxs-lookup"><span data-stu-id="e0028-109">All database providers are not equal</span></span>

<span data-ttu-id="e0028-110">Важно понимать, что платформа EF Core не предназначена для абстрагирования всех аспектов используемой СУБД.</span><span class="sxs-lookup"><span data-stu-id="e0028-110">It is very important to understand that EF Core is not designed to abstract every aspect of the underlying database system.</span></span>
<span data-ttu-id="e0028-111">Напротив, она предлагает общий набор шаблонов и понятий, которые можно использовать с любыми системами.</span><span class="sxs-lookup"><span data-stu-id="e0028-111">Instead, EF Core is a common set of patterns and concepts that can be used with any database system.</span></span>
<span data-ttu-id="e0028-112">Поставщики баз данных EF Core затем реализуют конкретные действия и функции как следующий уровень, используя эту общую платформу в качестве фундамента.</span><span class="sxs-lookup"><span data-stu-id="e0028-112">EF Core database providers then layer database-specific behavior and functionality over this common framework.</span></span>
<span data-ttu-id="e0028-113">В результате каждая СУБД может выполнять именно те задачи, с которыми она справляется лучше всего, сохраняя при необходимости общность с другими системами.</span><span class="sxs-lookup"><span data-stu-id="e0028-113">This allows each database system to do what it does best while still maintaining commonality, where appropriate, with other database systems.</span></span> 

<span data-ttu-id="e0028-114">По сути это означает, что смена поставщика базы данных приведет к изменению поведения EF Core и приложение не будет работать должным образом, если только оно явным образом не учитывает все различия.</span><span class="sxs-lookup"><span data-stu-id="e0028-114">Fundamentally, this means that switching out the database provider will change EF Core behavior and the application can't be expected to function correctly unless it explicitly accounts for all differences in behavior.</span></span>
<span data-ttu-id="e0028-115">Тем не менее, во многих случаях такая смена возможна из-за высокой степени схожести между различными реляционными базами.</span><span class="sxs-lookup"><span data-stu-id="e0028-115">That being said, in many cases doing this will work because there is a high degree of commonality amongst relational databases.</span></span>
<span data-ttu-id="e0028-116">Это имеет и преимущества, и недостатки.</span><span class="sxs-lookup"><span data-stu-id="e0028-116">This is good and bad.</span></span>
<span data-ttu-id="e0028-117">Преимущество состоит в том, что переход между базами данных может быть относительно простым.</span><span class="sxs-lookup"><span data-stu-id="e0028-117">Good because moving between databases can be relatively easy.</span></span>
<span data-ttu-id="e0028-118">Недостатком является возможное ложное чувство безопасности, ведь приложение может быть не полностью протестировано в новой СУБД.</span><span class="sxs-lookup"><span data-stu-id="e0028-118">Bad because it can give a false sense of security if the application is not fully tested against the new database system.</span></span>  

## <a name="approach-1-production-database-system"></a><span data-ttu-id="e0028-119">Подход 1. СУБД рабочей среды</span><span class="sxs-lookup"><span data-stu-id="e0028-119">Approach 1: Production database system</span></span>

<span data-ttu-id="e0028-120">Как описано в разделе выше, единственным способом тестирования компонентов, применяемых в рабочей среде, является использование той же системы баз данных.</span><span class="sxs-lookup"><span data-stu-id="e0028-120">As described in the previous section, the only way to be sure you are testing what runs in production is to use the same database system.</span></span>
<span data-ttu-id="e0028-121">Например, если развернутое приложение использует SQL в Azure, то тестирование также должно выполняться для SQL в Azure.</span><span class="sxs-lookup"><span data-stu-id="e0028-121">For example, if the deployed application uses SQL Azure, then testing should also be done against SQL Azure.</span></span>

<span data-ttu-id="e0028-122">Однако если каждый разработчик будет запускать тесты в SQL Azure и одновременно активно работать над кодом, это потребует значительного времени и затрат.</span><span class="sxs-lookup"><span data-stu-id="e0028-122">However, having every developer run tests against SQL Azure while actively working on the code would be both slow and expensive.</span></span>
<span data-ttu-id="e0028-123">В этом и заключается основной компромисс при таких подходах: когда будет уместно отклониться от рабочей СУБД, чтобы повысить эффективность тестирования?</span><span class="sxs-lookup"><span data-stu-id="e0028-123">This illustrates the main trade off involved throughout these approaches: when is it appropriate to deviate from the production database system so as to improve test efficiency?</span></span>

<span data-ttu-id="e0028-124">К счастью, в этом случае ответ довольно прост: для тестирования разработчиками необходимо использовать локальную среду SQL Server.</span><span class="sxs-lookup"><span data-stu-id="e0028-124">Luckily, in this case the answer is quite easy: use local or on-premises SQL Server for developer testing.</span></span>
<span data-ttu-id="e0028-125">SQL Server и SQL в Azure практически идентичны, поэтому тестирование в SQL Server обычно является разумным компромиссом.</span><span class="sxs-lookup"><span data-stu-id="e0028-125">SQL Azure and SQL Server are extremely similar, so testing against SQL Server is usually a reasonable trade off.</span></span>
<span data-ttu-id="e0028-126">Вместе с тем все-таки будет целесообразно провести тесты непосредственно в SQL Azure, прежде чем переходить к рабочей среде.</span><span class="sxs-lookup"><span data-stu-id="e0028-126">That being said, it is still wise to run tests against SQL Azure itself before going into production.</span></span>
 
### <a name="localdb"></a><span data-ttu-id="e0028-127">LocalDB</span><span class="sxs-lookup"><span data-stu-id="e0028-127">LocalDb</span></span> 

<span data-ttu-id="e0028-128">Все популярные СУБД имеют некий вариант "выпуска для разработчиков" (Developer Edition) для локального тестирования.</span><span class="sxs-lookup"><span data-stu-id="e0028-128">All the major database systems have some form of "Developer Edition" for local testing.</span></span>
<span data-ttu-id="e0028-129">Кроме того, в SQL Server есть компонент под названием [LocalDB](/sql/database-engine/configure-windows/sql-server-express-localdb?view=sql-server-ver15).</span><span class="sxs-lookup"><span data-stu-id="e0028-129">SQL Server also also has a feature called [LocalDb](/sql/database-engine/configure-windows/sql-server-express-localdb?view=sql-server-ver15).</span></span>
<span data-ttu-id="e0028-130">Основным преимуществом LocalDB является возможность запуска экземпляра базы данных по запросу.</span><span class="sxs-lookup"><span data-stu-id="e0028-130">The primary advantage of LocalDb is that it spins up the database instance on demand.</span></span>
<span data-ttu-id="e0028-131">За счет этого не требуется, чтобы на компьютере постоянно работала служба баз данных, даже когда тесты не выполняются.</span><span class="sxs-lookup"><span data-stu-id="e0028-131">This avoids having a database service running on your machine even when you're not running tests.</span></span>

<span data-ttu-id="e0028-132">Однако LocalDB имеет ряд ограничений:</span><span class="sxs-lookup"><span data-stu-id="e0028-132">LocalDb is not without it's issues:</span></span>
* <span data-ttu-id="e0028-133">Этот компонент не поддерживает полный функционал выпуска [SQL Server Developer Edition](/sql/sql-server/editions-and-components-of-sql-server-2016?view=sql-server-ver15).</span><span class="sxs-lookup"><span data-stu-id="e0028-133">It doesn't support everything that [SQL Server Developer Edition](/sql/sql-server/editions-and-components-of-sql-server-2016?view=sql-server-ver15) does.</span></span>
* <span data-ttu-id="e0028-134">Он недоступен для Linux.</span><span class="sxs-lookup"><span data-stu-id="e0028-134">It isn't available on Linux.</span></span>
* <span data-ttu-id="e0028-135">Он может вызывать задержку при первом тестовом запуске из-за развертывания службы.</span><span class="sxs-lookup"><span data-stu-id="e0028-135">It can cause lag on first test run as the service is spun up.</span></span>

<span data-ttu-id="e0028-136">Автор этой статьи никогда не видел проблем в постоянной работе службы баз данных у себя на компьютере, поэтому в большинстве случаев рекомендует использовать Developer Edition, а не LocalDB.</span><span class="sxs-lookup"><span data-stu-id="e0028-136">Personally, I've never found it a problem having a database service running on my dev machine and I would generally recommend using Developer Edition instead.</span></span>
<span data-ttu-id="e0028-137">Однако последний вариант может подойти некоторым разработчикам с более слабыми машинами.</span><span class="sxs-lookup"><span data-stu-id="e0028-137">However, it may be appropriate for some people, especially on less powerful dev machines.</span></span>  

## <a name="approach-2-sqlite"></a><span data-ttu-id="e0028-138">Подход 2. SQLite</span><span class="sxs-lookup"><span data-stu-id="e0028-138">Approach 2: SQLite</span></span>

<span data-ttu-id="e0028-139">EF Core тестирует поставщик SQL Server, главным образом запуская его в локальном экземпляре SQL Server.</span><span class="sxs-lookup"><span data-stu-id="e0028-139">EF Core tests the SQL Server provider primarily by running it against a local SQL Server instance.</span></span>
<span data-ttu-id="e0028-140">На быстродействующем компьютере эти тесты выполняют десятки тысяч запросов всего за несколько минут.</span><span class="sxs-lookup"><span data-stu-id="e0028-140">These tests run tens of thousands of queries in a couple of minutes on a fast machine.</span></span>
<span data-ttu-id="e0028-141">Это доказывает, что реальная СУБД может оказаться высокоэффективным решением.</span><span class="sxs-lookup"><span data-stu-id="e0028-141">This illustrates that using the real database system can be a performant solution.</span></span>
<span data-ttu-id="e0028-142">То, что для быстрого тестирования обязательно необходима небольшая база данных, является мифом.</span><span class="sxs-lookup"><span data-stu-id="e0028-142">It is a myth that using some lighter-weight database is the only way to run tests quickly.</span></span>

<span data-ttu-id="e0028-143">Но что если по какой-то причине вы не можете запускать тесты в среде, близкой к рабочей СУБД?</span><span class="sxs-lookup"><span data-stu-id="e0028-143">That being said, what if for whatever reason you can't run tests against something close to your production database system?</span></span>
<span data-ttu-id="e0028-144">Следующий по оптимальности вариант заключается в использовании решения с похожими функциями.</span><span class="sxs-lookup"><span data-stu-id="e0028-144">The next best choice is to use something with similar functionality.</span></span>
<span data-ttu-id="e0028-145">Обычно это другая реляционная база данных, и очевидным выбором здесь будет [SQLite](https://sqlite.org/index.html).</span><span class="sxs-lookup"><span data-stu-id="e0028-145">This usually means another relational database, for which [SQLite](https://sqlite.org/index.html) is the obvious choice.</span></span>

<span data-ttu-id="e0028-146">Система SQLite имеет следующие преимущества:</span><span class="sxs-lookup"><span data-stu-id="e0028-146">SQLite is a good choice because:</span></span>
* <span data-ttu-id="e0028-147">Она выполняется внутри процессов приложения, поэтому создает незначительную нагрузку.</span><span class="sxs-lookup"><span data-stu-id="e0028-147">It runs in-process with your application and so has low overhead.</span></span>
* <span data-ttu-id="e0028-148">SQLite использует для баз данных простые, автоматически создаваемые файлы и поэтому не требует управления базами.</span><span class="sxs-lookup"><span data-stu-id="e0028-148">It uses simple, automatically created files for databases, and so doesn't require database management.</span></span>
* <span data-ttu-id="e0028-149">Она имеет режим выполнения в памяти, позволяющий даже не создавать файлы.</span><span class="sxs-lookup"><span data-stu-id="e0028-149">It has an in-memory mode that avoids even the file creation.</span></span>

<span data-ttu-id="e0028-150">Однако следует помнить следующее:</span><span class="sxs-lookup"><span data-stu-id="e0028-150">However, remember that:</span></span>
* <span data-ttu-id="e0028-151">SQLite безусловно не поддерживает полный функционал вашей рабочей СУБД.</span><span class="sxs-lookup"><span data-stu-id="e0028-151">SQLite inevitability doesn't support everything that your production database system does.</span></span>
* <span data-ttu-id="e0028-152">Для некоторых запросов система SQLite и ваша рабочая СУБД будут работать по-разному.</span><span class="sxs-lookup"><span data-stu-id="e0028-152">SQLite will behave differently than your production database system for some queries.</span></span>

<span data-ttu-id="e0028-153">Поэтому если вы используете SQLite для тестирования, обязательно запускайте тесты и в реальной системе баз данных.</span><span class="sxs-lookup"><span data-stu-id="e0028-153">So if you do use SQLite for some testing, make sure to also test against your real database system.</span></span>

<span data-ttu-id="e0028-154">Конкретные рекомендации для EF Core см. в статье [Тестирование с помощью SQLite](xref:core/miscellaneous/testing/sqlite).</span><span class="sxs-lookup"><span data-stu-id="e0028-154">See [Testing with SQLite](xref:core/miscellaneous/testing/sqlite) for EF Core specific guidance.</span></span> 

## <a name="approach-3-the-ef-core-in-memory-database"></a><span data-ttu-id="e0028-155">Подход 3. Выполняющаяся в памяти база данных EF Core</span><span class="sxs-lookup"><span data-stu-id="e0028-155">Approach 3: The EF Core in-memory database</span></span>

<span data-ttu-id="e0028-156">В состав EF Core входит выполняющаяся в памяти база данных, которая используется для внутреннего тестирования самой платформы EF Core.</span><span class="sxs-lookup"><span data-stu-id="e0028-156">EF Core comes with an in-memory database that we use for internal testing of EF Core itself.</span></span>
<span data-ttu-id="e0028-157">Эта база в целом **не подходит в качестве замены для тестирования использующих EF Core приложений**.</span><span class="sxs-lookup"><span data-stu-id="e0028-157">This database is in general **not suitable as a substitute for testing applications that use EF Core**.</span></span> <span data-ttu-id="e0028-158">В частности:</span><span class="sxs-lookup"><span data-stu-id="e0028-158">Specifically:</span></span>
* <span data-ttu-id="e0028-159">Она не является реляционной базой данных.</span><span class="sxs-lookup"><span data-stu-id="e0028-159">It is not a relational database</span></span>
* <span data-ttu-id="e0028-160">Она не поддерживает транзакции.</span><span class="sxs-lookup"><span data-stu-id="e0028-160">It doesn't support transactions</span></span>
* <span data-ttu-id="e0028-161">Она не оптимизирована для производительности.</span><span class="sxs-lookup"><span data-stu-id="e0028-161">It is not optimized for performance</span></span>

<span data-ttu-id="e0028-162">Эти факторы не имеют большого значения при тестировании внутренних компонентов EF Core, так как мы используем базу данных именно там, где она не важна для теста.</span><span class="sxs-lookup"><span data-stu-id="e0028-162">None of this is very important when testing EF Core internals because we use it specifically where the database is irrelevant to the test.</span></span>
<span data-ttu-id="e0028-163">Однако эти аспекты становятся крайне важными при тестировании использующего EF Core приложения.</span><span class="sxs-lookup"><span data-stu-id="e0028-163">On the other hand, these things tend to be very important when testing an application that uses EF Core.</span></span>

## <a name="unit-testing"></a><span data-ttu-id="e0028-164">Модульное тестирование</span><span class="sxs-lookup"><span data-stu-id="e0028-164">Unit testing</span></span>

<span data-ttu-id="e0028-165">Предположим, мы тестируем часть бизнес-логики, для которой могут потребоваться какие-то данные из базы, но при этом мы не тестируем работу с базой как таковую.</span><span class="sxs-lookup"><span data-stu-id="e0028-165">Consider testing a piece of business logic that might need to use some data from a database, but is not inherently testing the database interactions.</span></span>
<span data-ttu-id="e0028-166">В этом случае один из вариантов — использовать [дублер теста](https://en.wikipedia.org/wiki/Test_double), например фиктивную реализацию или макет.</span><span class="sxs-lookup"><span data-stu-id="e0028-166">One option is to use a [test double](https://en.wikipedia.org/wiki/Test_double) such as a mock or fake.</span></span>

<span data-ttu-id="e0028-167">Дублеры теста применяются для внутреннего тестирования EF Core.</span><span class="sxs-lookup"><span data-stu-id="e0028-167">We use test doubles for internal testing of EF Core.</span></span>
<span data-ttu-id="e0028-168">Однако мы никогда не используем макеты DbContext или IQueryable.</span><span class="sxs-lookup"><span data-stu-id="e0028-168">However, we never try to mock DbContext or IQueryable.</span></span>
<span data-ttu-id="e0028-169">Это сложный, громоздкий и нестабильный процесс.</span><span class="sxs-lookup"><span data-stu-id="e0028-169">Doing so is difficult, cumbersome, and fragile.</span></span>
<span data-ttu-id="e0028-170">**Не делайте этого.**</span><span class="sxs-lookup"><span data-stu-id="e0028-170">**Don't do it.**</span></span>

<span data-ttu-id="e0028-171">Вместо этого при модульном тестировании решений, применяющих DbContext, мы используем выполняющуюся в памяти базу данных.</span><span class="sxs-lookup"><span data-stu-id="e0028-171">Instead we use the in-memory database when unit testing something that uses DbContext.</span></span>
<span data-ttu-id="e0028-172">В этом случае ее использование целесообразно, так как тест не зависит от работы базы.</span><span class="sxs-lookup"><span data-stu-id="e0028-172">In this case using the in-memory database is appropriate because the test is not dependent on database behavior.</span></span>
<span data-ttu-id="e0028-173">Только не прибегайте к этому варианту для тестирования реальных запросов и операций обновления.</span><span class="sxs-lookup"><span data-stu-id="e0028-173">Just don't do this to test actual database queries or updates.</span></span>   

<span data-ttu-id="e0028-174">Конкретные рекомендации для EF Core по модульному тестированию с использованием выполняющейся в памяти базы данных см. в статье [Тестирование с помощью InMemory](xref:core/miscellaneous/testing/in-memory).</span><span class="sxs-lookup"><span data-stu-id="e0028-174">See [Testing with the in-memory provider](xref:core/miscellaneous/testing/in-memory) for EF Core specific guidance on using the in-memory database for unit testing.</span></span>
