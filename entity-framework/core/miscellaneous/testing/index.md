---
title: Тестирование кода, использующего EF Core (EF Core)
description: Различные подходы к тестированию приложений, использующих EF Core.
author: ajcvickers
ms.date: 04/22/2020
uid: core/miscellaneous/testing/index
ms.openlocfilehash: 415769e9c3c664ce49c9308740d39a65a10807ba
ms.sourcegitcommit: 59e3d5ce7dfb284457cf1c991091683b2d1afe9d
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/20/2020
ms.locfileid: "83672900"
---
# <a name="testing-code-that-uses-ef-core"></a><span data-ttu-id="4bfae-103">Тестирование кода, использующего EF Core</span><span class="sxs-lookup"><span data-stu-id="4bfae-103">Testing code that uses EF Core</span></span>

<span data-ttu-id="4bfae-104">Тестирование кода, обращающегося к базе данных, требует одного из следующих вариантов:</span><span class="sxs-lookup"><span data-stu-id="4bfae-104">Testing code that accesses a database requires either:</span></span>
* <span data-ttu-id="4bfae-105">Выполнение запросов и обновлений в той же СУБД, что используется в рабочей среде.</span><span class="sxs-lookup"><span data-stu-id="4bfae-105">Running queries and updates against the same database system used in production.</span></span>
* <span data-ttu-id="4bfae-106">Выполнение запросов и обновлений в другой СУБД, более простой для управления.</span><span class="sxs-lookup"><span data-stu-id="4bfae-106">Running queries and updates against some other easier to manage database system.</span></span>
* <span data-ttu-id="4bfae-107">Применение дублеров теста или иного механизма, позволяющего не использовать базу данных вообще.</span><span class="sxs-lookup"><span data-stu-id="4bfae-107">Using test doubles or some other mechanism to avoid using a database at all.</span></span>

<span data-ttu-id="4bfae-108">В этом документе описаны компромиссы, связанные с каждым из этих вариантов, и показано использование EF Core при каждом подходе.</span><span class="sxs-lookup"><span data-stu-id="4bfae-108">This document outlines the trade-offs involved in each of these choices and shows how EF Core can be used with each approach.</span></span>  

> [!TIP]
> <span data-ttu-id="4bfae-109">Пример кода, в котором демонстрируются описанные здесь понятия, см. в статье [Пример тестирования EF Core](xref:core/miscellaneous/testing/testing-sample).</span><span class="sxs-lookup"><span data-stu-id="4bfae-109">See [EF Core testing sample](xref:core/miscellaneous/testing/testing-sample) for code demonstrating the concepts introduced here.</span></span> 

## <a name="all-database-providers-are-not-equal"></a><span data-ttu-id="4bfae-110">Поставщики баз данных неодинаковы</span><span class="sxs-lookup"><span data-stu-id="4bfae-110">All database providers are not equal</span></span>

<span data-ttu-id="4bfae-111">Важно понимать, что платформа EF Core не предназначена для абстрагирования всех аспектов используемой СУБД.</span><span class="sxs-lookup"><span data-stu-id="4bfae-111">It is very important to understand that EF Core is not designed to abstract every aspect of the underlying database system.</span></span>
<span data-ttu-id="4bfae-112">Напротив, она предлагает общий набор шаблонов и понятий, которые можно использовать с любыми системами.</span><span class="sxs-lookup"><span data-stu-id="4bfae-112">Instead, EF Core is a common set of patterns and concepts that can be used with any database system.</span></span>
<span data-ttu-id="4bfae-113">Поставщики баз данных EF Core затем реализуют конкретные действия и функции как следующий уровень, используя эту общую платформу в качестве фундамента.</span><span class="sxs-lookup"><span data-stu-id="4bfae-113">EF Core database providers then layer database-specific behavior and functionality over this common framework.</span></span>
<span data-ttu-id="4bfae-114">В результате каждая СУБД может выполнять именно те задачи, с которыми она справляется лучше всего, сохраняя при необходимости общность с другими системами.</span><span class="sxs-lookup"><span data-stu-id="4bfae-114">This allows each database system to do what it does best while still maintaining commonality, where appropriate, with other database systems.</span></span> 

<span data-ttu-id="4bfae-115">По сути это означает, что смена поставщика базы данных приведет к изменению поведения EF Core и приложение не будет работать должным образом, если только оно явным образом не учитывает все различия.</span><span class="sxs-lookup"><span data-stu-id="4bfae-115">Fundamentally, this means that switching out the database provider will change EF Core behavior and the application can't be expected to function correctly unless it explicitly accounts for any differences in behavior.</span></span>
<span data-ttu-id="4bfae-116">Тем не менее, во многих случаях такая смена возможна из-за высокой степени схожести между различными реляционными базами.</span><span class="sxs-lookup"><span data-stu-id="4bfae-116">That being said, in many cases doing this will work because there is a high degree of commonality amongst relational databases.</span></span>
<span data-ttu-id="4bfae-117">Это имеет и преимущества, и недостатки.</span><span class="sxs-lookup"><span data-stu-id="4bfae-117">This is good and bad.</span></span>
<span data-ttu-id="4bfae-118">Преимущество состоит в том, что переход между системами баз данных может быть относительно простым.</span><span class="sxs-lookup"><span data-stu-id="4bfae-118">Good because moving between database systems can be relatively easy.</span></span>
<span data-ttu-id="4bfae-119">Недостатком является возможное ложное чувство безопасности, ведь приложение может быть не полностью протестировано в новой СУБД.</span><span class="sxs-lookup"><span data-stu-id="4bfae-119">Bad because it can give a false sense of security if the application is not fully tested against the new database system.</span></span>  

## <a name="approach-1-production-database-system"></a><span data-ttu-id="4bfae-120">Подход 1. СУБД рабочей среды</span><span class="sxs-lookup"><span data-stu-id="4bfae-120">Approach 1: Production database system</span></span>

<span data-ttu-id="4bfae-121">Как описано в разделе выше, единственным способом тестирования компонентов, применяемых в рабочей среде, является использование той же системы баз данных.</span><span class="sxs-lookup"><span data-stu-id="4bfae-121">As described in the previous section, the only way to be sure you are testing what runs in production is to use the same database system.</span></span>
<span data-ttu-id="4bfae-122">Например, если развернутое приложение использует SQL в Azure, то тестирование также должно выполняться для SQL в Azure.</span><span class="sxs-lookup"><span data-stu-id="4bfae-122">For example, if the deployed application uses SQL Azure, then testing should also be done against SQL Azure.</span></span>

<span data-ttu-id="4bfae-123">Однако если каждый разработчик будет запускать тесты в SQL Azure и одновременно активно работать над кодом, это потребует значительного времени и затрат.</span><span class="sxs-lookup"><span data-stu-id="4bfae-123">However, having every developer run tests against SQL Azure while actively working on the code would be both slow and expensive.</span></span>
<span data-ttu-id="4bfae-124">В этом и заключается основной компромисс при таких подходах: когда будет уместно отклониться от рабочей СУБД, чтобы повысить эффективность тестирования?</span><span class="sxs-lookup"><span data-stu-id="4bfae-124">This illustrates the main trade-off involved throughout these approaches: when is it appropriate to deviate from the production database system so as to improve test efficiency?</span></span>

<span data-ttu-id="4bfae-125">К счастью, в этом случае ответ довольно прост: для тестирования разработчиками необходимо использовать локальную среду SQL Server.</span><span class="sxs-lookup"><span data-stu-id="4bfae-125">Luckily, in this case the answer is quite easy: use local or on-premises SQL Server for developer testing.</span></span>
<span data-ttu-id="4bfae-126">SQL Server и SQL в Azure практически идентичны, поэтому тестирование в SQL Server обычно является разумным компромиссом.</span><span class="sxs-lookup"><span data-stu-id="4bfae-126">SQL Azure and SQL Server are extremely similar, so testing against SQL Server is usually a reasonable trade-off.</span></span>
<span data-ttu-id="4bfae-127">Вместе с тем все-таки будет целесообразно провести тесты непосредственно в SQL Azure, прежде чем переходить к рабочей среде.</span><span class="sxs-lookup"><span data-stu-id="4bfae-127">That being said, it is still wise to run tests against SQL Azure itself before going into production.</span></span>
 
### <a name="localdb"></a><span data-ttu-id="4bfae-128">LocalDB</span><span class="sxs-lookup"><span data-stu-id="4bfae-128">LocalDB</span></span> 

<span data-ttu-id="4bfae-129">Все популярные СУБД имеют некий вариант "выпуска для разработчиков" (Developer Edition) для локального тестирования.</span><span class="sxs-lookup"><span data-stu-id="4bfae-129">All the major database systems have some form of "Developer Edition" for local testing.</span></span>
<span data-ttu-id="4bfae-130">В SQL Server также есть компонент под названием [LocalDB](/sql/database-engine/configure-windows/sql-server-express-localdb?view=sql-server-ver15).</span><span class="sxs-lookup"><span data-stu-id="4bfae-130">SQL Server also has a feature called [LocalDB](/sql/database-engine/configure-windows/sql-server-express-localdb?view=sql-server-ver15).</span></span>
<span data-ttu-id="4bfae-131">Основным преимуществом LocalDB является возможность запуска экземпляра базы данных по запросу.</span><span class="sxs-lookup"><span data-stu-id="4bfae-131">The primary advantage of LocalDB is that it spins up the database instance on demand.</span></span>
<span data-ttu-id="4bfae-132">За счет этого не требуется, чтобы на компьютере постоянно работала служба баз данных, даже когда тесты не выполняются.</span><span class="sxs-lookup"><span data-stu-id="4bfae-132">This avoids having a database service running on your machine even when you're not running tests.</span></span>

<span data-ttu-id="4bfae-133">Но LocalDB имеет ряд ограничений:</span><span class="sxs-lookup"><span data-stu-id="4bfae-133">LocalDB is not without its issues:</span></span>
* <span data-ttu-id="4bfae-134">Этот компонент не поддерживает полный функционал выпуска [SQL Server Developer Edition](/sql/sql-server/editions-and-components-of-sql-server-2016?view=sql-server-ver15).</span><span class="sxs-lookup"><span data-stu-id="4bfae-134">It doesn't support everything that [SQL Server Developer Edition](/sql/sql-server/editions-and-components-of-sql-server-2016?view=sql-server-ver15) does.</span></span>
* <span data-ttu-id="4bfae-135">Он недоступен для Linux.</span><span class="sxs-lookup"><span data-stu-id="4bfae-135">It isn't available on Linux.</span></span>
* <span data-ttu-id="4bfae-136">Он может вызывать задержку при первом тестовом запуске из-за развертывания службы.</span><span class="sxs-lookup"><span data-stu-id="4bfae-136">It can cause lag on first test run as the service is spun up.</span></span>

<span data-ttu-id="4bfae-137">Автор этой статьи никогда не видел проблем в постоянной работе службы баз данных у себя на компьютере, поэтому в большинстве случаев рекомендует использовать Developer Edition, а не LocalDB.</span><span class="sxs-lookup"><span data-stu-id="4bfae-137">Personally, I've never found it a problem having a database service running on my dev machine and I would generally recommend using Developer Edition instead.</span></span>
<span data-ttu-id="4bfae-138">Но LocalDB может подойти некоторым разработчикам с более слабыми компьютерами.</span><span class="sxs-lookup"><span data-stu-id="4bfae-138">However, LocalDB may be appropriate for some people, especially on less powerful dev machines.</span></span>

<span data-ttu-id="4bfae-139">Запуск SQL Server (или любой другой системы базы данных) в контейнере Docker (или в подобной системе) — это еще один способ избежать запуска системы базы данных непосредственно на компьютере разработки.</span><span class="sxs-lookup"><span data-stu-id="4bfae-139">Running SQL Server (or any other database system) in a Docker container (or similar) is another way to avoid running the database system directly on your development machine.</span></span>  

## <a name="approach-2-sqlite"></a><span data-ttu-id="4bfae-140">Подход 2. SQLite</span><span class="sxs-lookup"><span data-stu-id="4bfae-140">Approach 2: SQLite</span></span>

<span data-ttu-id="4bfae-141">EF Core тестирует поставщик SQL Server, главным образом запуская его в локальном экземпляре SQL Server.</span><span class="sxs-lookup"><span data-stu-id="4bfae-141">EF Core tests the SQL Server provider primarily by running it against a local SQL Server instance.</span></span>
<span data-ttu-id="4bfae-142">На быстродействующем компьютере эти тесты выполняют десятки тысяч запросов всего за несколько минут.</span><span class="sxs-lookup"><span data-stu-id="4bfae-142">These tests run tens of thousands of queries in a couple of minutes on a fast machine.</span></span>
<span data-ttu-id="4bfae-143">Это доказывает, что реальная СУБД может оказаться высокоэффективным решением.</span><span class="sxs-lookup"><span data-stu-id="4bfae-143">This illustrates that using the real database system can be a performant solution.</span></span>
<span data-ttu-id="4bfae-144">То, что для быстрого тестирования обязательно необходима небольшая база данных, является мифом.</span><span class="sxs-lookup"><span data-stu-id="4bfae-144">It is a myth that using some lighter-weight database is the only way to run tests quickly.</span></span>

<span data-ttu-id="4bfae-145">Но что если по какой-то причине вы не можете запускать тесты в среде, близкой к рабочей СУБД?</span><span class="sxs-lookup"><span data-stu-id="4bfae-145">That being said, what if for whatever reason you can't run tests against something close to your production database system?</span></span>
<span data-ttu-id="4bfae-146">Следующий по оптимальности вариант заключается в использовании решения с похожими функциями.</span><span class="sxs-lookup"><span data-stu-id="4bfae-146">The next best choice is to use something with similar functionality.</span></span>
<span data-ttu-id="4bfae-147">Обычно это другая реляционная база данных, и очевидным выбором здесь будет [SQLite](https://sqlite.org/index.html).</span><span class="sxs-lookup"><span data-stu-id="4bfae-147">This usually means another relational database, for which [SQLite](https://sqlite.org/index.html) is the obvious choice.</span></span>

<span data-ttu-id="4bfae-148">Система SQLite имеет следующие преимущества:</span><span class="sxs-lookup"><span data-stu-id="4bfae-148">SQLite is a good choice because:</span></span>
* <span data-ttu-id="4bfae-149">Она выполняется внутри процессов приложения, поэтому создает незначительную нагрузку.</span><span class="sxs-lookup"><span data-stu-id="4bfae-149">It runs in-process with your application and so has low overhead.</span></span>
* <span data-ttu-id="4bfae-150">SQLite использует для баз данных простые, автоматически создаваемые файлы и поэтому не требует управления базами.</span><span class="sxs-lookup"><span data-stu-id="4bfae-150">It uses simple, automatically created files for databases, and so doesn't require database management.</span></span>
* <span data-ttu-id="4bfae-151">Она имеет режим выполнения в памяти, позволяющий даже не создавать файлы.</span><span class="sxs-lookup"><span data-stu-id="4bfae-151">It has an in-memory mode that avoids even the file creation.</span></span>

<span data-ttu-id="4bfae-152">Однако следует помнить следующее:</span><span class="sxs-lookup"><span data-stu-id="4bfae-152">However, remember that:</span></span>
* <span data-ttu-id="4bfae-153">SQLite безусловно не поддерживает полный функционал вашей рабочей СУБД.</span><span class="sxs-lookup"><span data-stu-id="4bfae-153">SQLite inevitability doesn't support everything that your production database system does.</span></span>
* <span data-ttu-id="4bfae-154">Для некоторых запросов система SQLite и ваша рабочая СУБД будут работать по-разному.</span><span class="sxs-lookup"><span data-stu-id="4bfae-154">SQLite will behave differently than your production database system for some queries.</span></span>

<span data-ttu-id="4bfae-155">Поэтому если вы используете SQLite для тестирования, обязательно запускайте тесты и в реальной системе баз данных.</span><span class="sxs-lookup"><span data-stu-id="4bfae-155">So if you do use SQLite for some testing, make sure to also test against your real database system.</span></span>

<span data-ttu-id="4bfae-156">Конкретные рекомендации для EF Core см. в статье [Тестирование с помощью SQLite](xref:core/miscellaneous/testing/sqlite).</span><span class="sxs-lookup"><span data-stu-id="4bfae-156">See [Testing with SQLite](xref:core/miscellaneous/testing/sqlite) for EF Core specific guidance.</span></span> 

## <a name="approach-3-the-ef-core-in-memory-database"></a><span data-ttu-id="4bfae-157">Подход 3. Выполняющаяся в памяти база данных EF Core</span><span class="sxs-lookup"><span data-stu-id="4bfae-157">Approach 3: The EF Core in-memory database</span></span>

<span data-ttu-id="4bfae-158">В состав EF Core входит выполняющаяся в памяти база данных, которая используется для внутреннего тестирования самой платформы EF Core.</span><span class="sxs-lookup"><span data-stu-id="4bfae-158">EF Core comes with an in-memory database that we use for internal testing of EF Core itself.</span></span>
<span data-ttu-id="4bfae-159">Эта база в целом **не подходит в качестве замены для тестирования использующих EF Core приложений**.</span><span class="sxs-lookup"><span data-stu-id="4bfae-159">This database is in general **not suitable as a substitute for testing applications that use EF Core**.</span></span> <span data-ttu-id="4bfae-160">В частности:</span><span class="sxs-lookup"><span data-stu-id="4bfae-160">Specifically:</span></span>
* <span data-ttu-id="4bfae-161">Она не является реляционной базой данных.</span><span class="sxs-lookup"><span data-stu-id="4bfae-161">It is not a relational database.</span></span>
* <span data-ttu-id="4bfae-162">Она не поддерживает транзакции.</span><span class="sxs-lookup"><span data-stu-id="4bfae-162">It doesn't support transactions.</span></span>
* <span data-ttu-id="4bfae-163">Она не оптимизирована для производительности.</span><span class="sxs-lookup"><span data-stu-id="4bfae-163">It is not optimized for performance.</span></span>

<span data-ttu-id="4bfae-164">Эти факторы не имеют большого значения при тестировании внутренних компонентов EF Core, так как мы используем базу данных именно там, где она не важна для теста.</span><span class="sxs-lookup"><span data-stu-id="4bfae-164">None of this is very important when testing EF Core internals because we use it specifically where the database is irrelevant to the test.</span></span>
<span data-ttu-id="4bfae-165">Однако эти аспекты становятся крайне важными при тестировании использующего EF Core приложения.</span><span class="sxs-lookup"><span data-stu-id="4bfae-165">On the other hand, these things tend to be very important when testing an application that uses EF Core.</span></span>

## <a name="unit-testing"></a><span data-ttu-id="4bfae-166">Модульное тестирование</span><span class="sxs-lookup"><span data-stu-id="4bfae-166">Unit testing</span></span>

<span data-ttu-id="4bfae-167">Предположим, мы тестируем часть бизнес-логики, для которой могут потребоваться какие-то данные из базы, но при этом мы не тестируем работу с базой как таковую.</span><span class="sxs-lookup"><span data-stu-id="4bfae-167">Consider testing a piece of business logic that might need to use some data from a database, but is not inherently testing the database interactions.</span></span>
<span data-ttu-id="4bfae-168">В этом случае один из вариантов — использовать [дублер теста](https://en.wikipedia.org/wiki/Test_double), например фиктивную реализацию или макет.</span><span class="sxs-lookup"><span data-stu-id="4bfae-168">One option is to use a [test double](https://en.wikipedia.org/wiki/Test_double) such as a mock or fake.</span></span>

<span data-ttu-id="4bfae-169">Дублеры теста применяются для внутреннего тестирования EF Core.</span><span class="sxs-lookup"><span data-stu-id="4bfae-169">We use test doubles for internal testing of EF Core.</span></span>
<span data-ttu-id="4bfae-170">Однако мы никогда не используем макеты DbContext или IQueryable.</span><span class="sxs-lookup"><span data-stu-id="4bfae-170">However, we never try to mock DbContext or IQueryable.</span></span>
<span data-ttu-id="4bfae-171">Это сложный, громоздкий и нестабильный процесс.</span><span class="sxs-lookup"><span data-stu-id="4bfae-171">Doing so is difficult, cumbersome, and fragile.</span></span>
<span data-ttu-id="4bfae-172">**Не делайте этого.**</span><span class="sxs-lookup"><span data-stu-id="4bfae-172">**Don't do it.**</span></span>

<span data-ttu-id="4bfae-173">Вместо этого при модульном тестировании решений, в которых применяется DbContext, мы используем выполняющуюся в памяти базу данных EF.</span><span class="sxs-lookup"><span data-stu-id="4bfae-173">Instead we use the EF in-memory database when unit testing something that uses DbContext.</span></span>
<span data-ttu-id="4bfae-174">В этом случае ее использование оправданно, так как тест не зависит от работы базы данных.</span><span class="sxs-lookup"><span data-stu-id="4bfae-174">In this case using the EF in-memory database is appropriate because the test is not dependent on database behavior.</span></span>
<span data-ttu-id="4bfae-175">Только не прибегайте к этому варианту для тестирования реальных запросов и операций обновления.</span><span class="sxs-lookup"><span data-stu-id="4bfae-175">Just don't do this to test actual database queries or updates.</span></span>   

<span data-ttu-id="4bfae-176">В статье [Пример тестирования EF Core](xref:core/miscellaneous/testing/testing-sample) демонстрируются тесты с использованием выполняющейся в памяти базы данных EF, а также SQL Server и SQLite.</span><span class="sxs-lookup"><span data-stu-id="4bfae-176">The [EF Core testing sample](xref:core/miscellaneous/testing/testing-sample) demonstrates tests using the EF in-memory database, as well as SQL Server and SQLite.</span></span> 
