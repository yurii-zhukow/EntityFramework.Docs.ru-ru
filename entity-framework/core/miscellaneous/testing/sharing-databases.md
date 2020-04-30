---
title: Совместное использование баз данных между тестами — EF Core
description: Пример, демонстрирующий совместное использование базы данных несколькими тестами
author: ajcvickers
ms.date: 04/25/2020
uid: core/miscellaneous/testing/sharing-databases
ms.openlocfilehash: 96216fb8afd6916402637de3ffab04b79d37e11c
ms.sourcegitcommit: 79e460f76b6664e1da5886d102bd97f651d2ffff
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/29/2020
ms.locfileid: "82564256"
---
# <a name="sharing-databases-between-tests"></a><span data-ttu-id="cbcc6-103">Совместное использование баз данных между тестами</span><span class="sxs-lookup"><span data-stu-id="cbcc6-103">Sharing databases between tests</span></span>

<span data-ttu-id="cbcc6-104">[Пример EF Core тестирования](xref:core/miscellaneous/testing/testing-sample) показал, как тестировать приложения в разных системах баз данных.</span><span class="sxs-lookup"><span data-stu-id="cbcc6-104">The [EF Core testing sample](xref:core/miscellaneous/testing/testing-sample) showed how to test applications against different database systems.</span></span>
<span data-ttu-id="cbcc6-105">Для этого примера каждый тест создал новую базу данных.</span><span class="sxs-lookup"><span data-stu-id="cbcc6-105">For that sample, each test created a new database.</span></span>
<span data-ttu-id="cbcc6-106">Это хороший шаблон при использовании SQLite или базы данных EF в памяти, но при использовании других систем баз данных может воздействовать значительная нагрузка.</span><span class="sxs-lookup"><span data-stu-id="cbcc6-106">This is a good pattern when using SQLite or the EF in-memory database, but it can involve significant overhead when using other database systems.</span></span>

<span data-ttu-id="cbcc6-107">Этот пример строится на предыдущем примере путем перемещения создания базы данных в средство тестирования.</span><span class="sxs-lookup"><span data-stu-id="cbcc6-107">This sample builds on the previous sample by moving database creation into a test fixture.</span></span>
<span data-ttu-id="cbcc6-108">Это позволяет создать одну SQL Server базу данных и заполнить ее только один раз для всех тестов.</span><span class="sxs-lookup"><span data-stu-id="cbcc6-108">This allows a single SQL Server database to be created and seeded only once for all tests.</span></span>

> [!TIP]
> <span data-ttu-id="cbcc6-109">Перед продолжением работы ознакомьтесь с [примером EF Core тестирования](xref:core/miscellaneous/testing/testing-sample) .</span><span class="sxs-lookup"><span data-stu-id="cbcc6-109">Make sure to work through the [EF Core testing sample](xref:core/miscellaneous/testing/testing-sample) before continuing here.</span></span>

<span data-ttu-id="cbcc6-110">Писать несколько тестов для одной и той же базы данных несложно.</span><span class="sxs-lookup"><span data-stu-id="cbcc6-110">It's not difficult to write multiple tests against the same database.</span></span>
<span data-ttu-id="cbcc6-111">Хитрость заключается в том, что тесты не переносятся друг на друга по мере выполнения.</span><span class="sxs-lookup"><span data-stu-id="cbcc6-111">The trick is doing it in a way that the tests don't trip over each other as they run.</span></span>
<span data-ttu-id="cbcc6-112">Для этого необходимо понимание:</span><span class="sxs-lookup"><span data-stu-id="cbcc6-112">This requires understanding:</span></span>
* <span data-ttu-id="cbcc6-113">Безопасное совместное использование объектов между тестами</span><span class="sxs-lookup"><span data-stu-id="cbcc6-113">How to safely share objects between tests</span></span>
* <span data-ttu-id="cbcc6-114">Когда платформа тестирования выполняет тесты в параллельном режиме</span><span class="sxs-lookup"><span data-stu-id="cbcc6-114">When the test framework runs tests in parallel</span></span>
* <span data-ttu-id="cbcc6-115">Сохранение базы данных в чистом состоянии для каждого теста</span><span class="sxs-lookup"><span data-stu-id="cbcc6-115">How to keep the database in a clean state for every test</span></span>  

## <a name="the-fixture"></a><span data-ttu-id="cbcc6-116">Основы</span><span class="sxs-lookup"><span data-stu-id="cbcc6-116">The fixture</span></span>

<span data-ttu-id="cbcc6-117">Мы будем использовать средство тестирования для совместного использования объектов между тестами.</span><span class="sxs-lookup"><span data-stu-id="cbcc6-117">We will use a test fixture for sharing objects between tests.</span></span>
<span data-ttu-id="cbcc6-118">В [документации xUnit](https://xunit.net/docs/shared-context.html) указано, что следует использовать средство, если необходимо создать один контекст теста и предоставить его для всех тестов в классе, а затем очистить его после завершения всех тестов в классе. "</span><span class="sxs-lookup"><span data-stu-id="cbcc6-118">The [XUnit documentation](https://xunit.net/docs/shared-context.html) states that a fixture should be used "when you want to create a single test context and share it among all the tests in the class, and have it cleaned up after all the tests in the class have finished."</span></span>

> [!TIP]
> <span data-ttu-id="cbcc6-119">В этом примере используется [xUnit](https://xunit.net/), но аналогичные концепции существуют в других платформах тестирования, включая [NUnit](https://nunit.org/).</span><span class="sxs-lookup"><span data-stu-id="cbcc6-119">This sample uses [XUnit](https://xunit.net/), but similar concepts exist in other testing frameworks, including [NUnit](https://nunit.org/).</span></span>

<span data-ttu-id="cbcc6-120">Это означает, что необходимо переместить создание и заполнение базы данных в класс основы.</span><span class="sxs-lookup"><span data-stu-id="cbcc6-120">This means that we need to move database creation and seeding to a fixture class.</span></span>
<span data-ttu-id="cbcc6-121">Вот как оно выглядит:</span><span class="sxs-lookup"><span data-stu-id="cbcc6-121">Here's what it looks like:</span></span>

[!code-csharp[SharedDatabaseFixture](../../../../samples/core/Miscellaneous/Testing/ItemsWebApi/SharedDatabaseTests/SharedDatabaseFixture.cs?name=SharedDatabaseFixture)]

<span data-ttu-id="cbcc6-122">Теперь обратите внимание на то, как конструктор:</span><span class="sxs-lookup"><span data-stu-id="cbcc6-122">For now, notice how the constructor:</span></span>
* <span data-ttu-id="cbcc6-123">Создает одно подключение к базе данных в течение времени существования основы.</span><span class="sxs-lookup"><span data-stu-id="cbcc6-123">Creates a single database connection for the lifetime of the fixture</span></span>
* <span data-ttu-id="cbcc6-124">Создает и заполняет эту базу данных путем вызова `Seed` метода.</span><span class="sxs-lookup"><span data-stu-id="cbcc6-124">Creates and seeds that database by calling the `Seed` method</span></span> 

<span data-ttu-id="cbcc6-125">Игнорировать блокировку сейчас; Мы вернемся к нему позже.</span><span class="sxs-lookup"><span data-stu-id="cbcc6-125">Ignore the locking for now; we will come back to it later.</span></span>

> [!TIP]
> <span data-ttu-id="cbcc6-126">Код создания и заполнения не обязательно должен быть асинхронным.</span><span class="sxs-lookup"><span data-stu-id="cbcc6-126">The creation and seeding code does not need to be async.</span></span>
> <span data-ttu-id="cbcc6-127">Его асинхронное выполнение усложняет код и не повышает производительность и пропускную способность тестов.</span><span class="sxs-lookup"><span data-stu-id="cbcc6-127">Making it async will complicate the code and will not improve performance or throughput of tests.</span></span>

<span data-ttu-id="cbcc6-128">База данных создается при первом удалении любой существующей базы данных, а затем при создании новой базы данных.</span><span class="sxs-lookup"><span data-stu-id="cbcc6-128">The database is created by first deleting any existing database and then creating a new database.</span></span>
<span data-ttu-id="cbcc6-129">Это гарантирует, что база данных будет соответствовать текущей модели EF, даже если она была изменена с момента последнего тестового запуска.</span><span class="sxs-lookup"><span data-stu-id="cbcc6-129">This ensures that the database matches the current EF model even if it has been changed since the last test run.</span></span>

> [!TIP]
> <span data-ttu-id="cbcc6-130">Можно быстрее «очищать» существующую базу данных, используя нечто вроде перезапуска [, а не](https://jimmybogard.com/tag/respawn/) повторно создавать ее каждый раз.</span><span class="sxs-lookup"><span data-stu-id="cbcc6-130">It can be faster to "clean" the existing database using something like [respawn](https://jimmybogard.com/tag/respawn/) rather than re-create it each time.</span></span>
> <span data-ttu-id="cbcc6-131">Однако необходимо соблюдать осторожность, чтобы обеспечить актуальность схемы базы данных с помощью модели EF.</span><span class="sxs-lookup"><span data-stu-id="cbcc6-131">However, care must be taken to ensure that the database schema is up-to-date with the EF model when doing this.</span></span>

<span data-ttu-id="cbcc6-132">Подключение к базе данных удаляется при удалении основы.</span><span class="sxs-lookup"><span data-stu-id="cbcc6-132">The database connection is disposed when the fixture is disposed.</span></span>
<span data-ttu-id="cbcc6-133">На этом этапе также можно удалить тестовую базу данных.</span><span class="sxs-lookup"><span data-stu-id="cbcc6-133">You may also consider deleting the test database at this point.</span></span>
<span data-ttu-id="cbcc6-134">Однако это потребует дополнительной блокировки и подсчета ссылок, если оборудование является общим для нескольких тестовых классов.</span><span class="sxs-lookup"><span data-stu-id="cbcc6-134">However, this will require additional locking and reference counting if the fixture is being shared by multiple test classes.</span></span>
<span data-ttu-id="cbcc6-135">Кроме того, часто бывает полезно, чтобы тестовая база данных по-прежнему была доступна для отладки неудачных тестов.</span><span class="sxs-lookup"><span data-stu-id="cbcc6-135">Also, it is often useful to have the test database still available for debugging failed tests.</span></span>  

## <a name="using-the-fixture"></a><span data-ttu-id="cbcc6-136">Использование основы</span><span class="sxs-lookup"><span data-stu-id="cbcc6-136">Using the fixture</span></span>

<span data-ttu-id="cbcc6-137">XUnit имеет общий шаблон для связывания средства тестирования с классом тестов:</span><span class="sxs-lookup"><span data-stu-id="cbcc6-137">XUnit has a common pattern for associating a test fixture with a class of tests:</span></span>

[!code-csharp[UsingTheFixture](../../../../samples/core/Miscellaneous/Testing/ItemsWebApi/SharedDatabaseTests/SharedDatabaseTest.cs?name=UsingTheFixture)]

<span data-ttu-id="cbcc6-138">XUnit теперь создаст один экземпляр основы и передаст его каждому экземпляру тестового класса.</span><span class="sxs-lookup"><span data-stu-id="cbcc6-138">XUnit will now create a single fixture instance and pass it to each instance of the test class.</span></span>
<span data-ttu-id="cbcc6-139">(Помните из первого [примера тестирования](xref:core/miscellaneous/testing/testing-sample) , который xUnit создает новый экземпляр тестового класса при каждом выполнении теста.) Это означает, что база данных будет создана и заполнена один раз, а затем каждый тест будет использовать эту базу данных.</span><span class="sxs-lookup"><span data-stu-id="cbcc6-139">(Remember from the first [testing sample](xref:core/miscellaneous/testing/testing-sample) that XUnit creates a new test class instance every time it runs a test.) This means that the database will be created and seeded once and then each test will use this database.</span></span>

<span data-ttu-id="cbcc6-140">Обратите внимание, что тесты внутри одного класса не будут выполняться параллельно.</span><span class="sxs-lookup"><span data-stu-id="cbcc6-140">Note that tests within a single class will not be run in parallel.</span></span>
<span data-ttu-id="cbcc6-141">Это означает, что для каждого теста можно использовать одно и то же подключение к базе данных, `DbConnection` даже если объект не является потокобезопасным.</span><span class="sxs-lookup"><span data-stu-id="cbcc6-141">This means it is safe for each test to use the same database connection, even though the `DbConnection` object is not thread-safe.</span></span>

## <a name="maintaining-database-state"></a><span data-ttu-id="cbcc6-142">Обслуживание состояния базы данных</span><span class="sxs-lookup"><span data-stu-id="cbcc6-142">Maintaining database state</span></span>

<span data-ttu-id="cbcc6-143">Тесты часто требуют изменения тестовых данных с помощью вставок, обновлений и удалений.</span><span class="sxs-lookup"><span data-stu-id="cbcc6-143">Tests often need to mutate the test data with inserts, updates, and deletes.</span></span>
<span data-ttu-id="cbcc6-144">Но эти изменения повлияют на другие тесты, в которых ожидается чистая заполненная база данных.</span><span class="sxs-lookup"><span data-stu-id="cbcc6-144">But these changes will then impact other tests which are expecting a clean, seeded database.</span></span>

<span data-ttu-id="cbcc6-145">Это может быть вызвано выполнением изменения тестов внутри транзакции.</span><span class="sxs-lookup"><span data-stu-id="cbcc6-145">This can be dealt with by running mutating tests inside a transaction.</span></span>
<span data-ttu-id="cbcc6-146">Пример:</span><span class="sxs-lookup"><span data-stu-id="cbcc6-146">For example:</span></span>

[!code-csharp[CanAddItem](../../../../samples/core/Miscellaneous/Testing/ItemsWebApi/SharedDatabaseTests/SharedDatabaseTest.cs?name=CanAddItem)]

<span data-ttu-id="cbcc6-147">Обратите внимание, что транзакция создается по мере начала и удаления теста по завершении.</span><span class="sxs-lookup"><span data-stu-id="cbcc6-147">Notice that the transaction is created as the test starts and disposed when it is finished.</span></span>
<span data-ttu-id="cbcc6-148">Удаление транзакции приводит к ее откату, поэтому никакие изменения не будут видны другим тестам.</span><span class="sxs-lookup"><span data-stu-id="cbcc6-148">Disposing the transaction causes it to be rolled back, so none of the changes will be seen by other tests.</span></span>

<span data-ttu-id="cbcc6-149">Вспомогательный метод для создания контекста (см. Приведенный выше код основы) принимает эту транзакцию и разрешает DbContext использовать ее.</span><span class="sxs-lookup"><span data-stu-id="cbcc6-149">The helper method for creating a context (see the fixture code above) accepts this transaction and opts the DbContext into using it.</span></span> 

## <a name="sharing-the-fixture"></a><span data-ttu-id="cbcc6-150">Совместное использование основы</span><span class="sxs-lookup"><span data-stu-id="cbcc6-150">Sharing the fixture</span></span>

<span data-ttu-id="cbcc6-151">Возможно, вы обратили внимание на код блокировки создания и заполнения базы данных.</span><span class="sxs-lookup"><span data-stu-id="cbcc6-151">You may have noticed locking code around database creation and seeding.</span></span>
<span data-ttu-id="cbcc6-152">Это не требуется для этого примера, так как только один класс тестов использует средство, поэтому создается только один экземпляр основы.</span><span class="sxs-lookup"><span data-stu-id="cbcc6-152">This is not needed for this sample since only one class of tests use the fixture, so only a single fixture instance is created.</span></span>

<span data-ttu-id="cbcc6-153">Тем не менее может потребоваться использовать один и тот же набор с несколькими классами тестов.</span><span class="sxs-lookup"><span data-stu-id="cbcc6-153">However, you may want to use the same fixture with multiple classes of tests.</span></span>
<span data-ttu-id="cbcc6-154">XUnit создаст один экземпляр основы для каждого из этих классов.</span><span class="sxs-lookup"><span data-stu-id="cbcc6-154">XUnit will create one fixture instance for each of these classes.</span></span>
<span data-ttu-id="cbcc6-155">Они могут использоваться различными потоками, выполняющими тесты параллельно.</span><span class="sxs-lookup"><span data-stu-id="cbcc6-155">These may be used by different threads running tests in parallel.</span></span>
<span data-ttu-id="cbcc6-156">Поэтому важно иметь подходящую блокировку, чтобы гарантировать создание и заполнение базы данных только одним потоком.</span><span class="sxs-lookup"><span data-stu-id="cbcc6-156">Therefore, it is important to have appropriate locking to ensure only one thread does the database creation and seeding.</span></span>

> [!TIP]
> <span data-ttu-id="cbcc6-157">Здесь очень `lock` просто.</span><span class="sxs-lookup"><span data-stu-id="cbcc6-157">A simple `lock` is fine here.</span></span>
> <span data-ttu-id="cbcc6-158">Нет необходимости предпринимать какие-либо более сложные действия, например шаблоны без блокировки.</span><span class="sxs-lookup"><span data-stu-id="cbcc6-158">There is no need to attempt anything more complex, such as any lock-free patterns.</span></span>
