---
title: Пригодность для тестирования и Entity Framework 4,0-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 9430e2ab-261c-4e8e-8545-2ebc52d7a247
ms.openlocfilehash: 28ec5446ce9faf98fb8fff141832236d70b29daf
ms.sourcegitcommit: 708b18520321c587b2046ad2ea9fa7c48aeebfe5
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/09/2019
ms.locfileid: "72181578"
---
# <a name="testability-and-entity-framework-40"></a><span data-ttu-id="442fe-102">Пригодность для тестирования и Entity Framework 4,0</span><span class="sxs-lookup"><span data-stu-id="442fe-102">Testability and Entity Framework 4.0</span></span>
<span data-ttu-id="442fe-103">Скотт Аллен</span><span class="sxs-lookup"><span data-stu-id="442fe-103">Scott Allen</span></span>

<span data-ttu-id="442fe-104">Выпущен Май 2010 г.</span><span class="sxs-lookup"><span data-stu-id="442fe-104">Published: May 2010</span></span>

## <a name="introduction"></a><span data-ttu-id="442fe-105">Вступление</span><span class="sxs-lookup"><span data-stu-id="442fe-105">Introduction</span></span>

<span data-ttu-id="442fe-106">В этом техническом документе описывается и демонстрируется написание тестируемого кода с помощью ADO.NET Entity Framework 4,0 и Visual Studio 2010.</span><span class="sxs-lookup"><span data-stu-id="442fe-106">This white paper describes and demonstrates how to write testable code with the ADO.NET Entity Framework 4.0 and Visual Studio 2010.</span></span> <span data-ttu-id="442fe-107">Этот документ не пытается сосредоточиться на конкретной методологии тестирования, такой как проектирование на основе тестирования (TDD) или проектирование на основе поведения (BDD).</span><span class="sxs-lookup"><span data-stu-id="442fe-107">This paper does not try to focus on a specific testing methodology, like test-driven design (TDD) or behavior-driven design (BDD).</span></span> <span data-ttu-id="442fe-108">В этом документе основное внимание уделяется тому, как писать код, использующий ADO.NET Entity Framework, но легко изолировать и протестировать в автоматическом режиме.</span><span class="sxs-lookup"><span data-stu-id="442fe-108">Instead this paper will focus on how to write code that uses the ADO.NET Entity Framework yet remains easy to isolate and test in an automated fashion.</span></span> <span data-ttu-id="442fe-109">Мы рассмотрим общие шаблоны разработки, которые облегчают тестирование в сценариях доступа к данным, и посмотрим, как применять эти шаблоны при использовании платформы.</span><span class="sxs-lookup"><span data-stu-id="442fe-109">We’ll look at common design patterns that facilitate testing in data access scenarios and see how to apply those patterns when using the framework.</span></span> <span data-ttu-id="442fe-110">Мы также рассмотрим определенные функции платформы, чтобы увидеть, как эти функции могут работать в тестируемом коде.</span><span class="sxs-lookup"><span data-stu-id="442fe-110">We’ll also look at specific features of the framework to see how those features can work in testable code.</span></span>

## <a name="what-is-testable-code"></a><span data-ttu-id="442fe-111">Что такое тестируемый код?</span><span class="sxs-lookup"><span data-stu-id="442fe-111">What Is Testable Code?</span></span>

<span data-ttu-id="442fe-112">Возможность проверки части программного обеспечения с помощью автоматизированных модульных тестов предоставляет множество преимуществ.</span><span class="sxs-lookup"><span data-stu-id="442fe-112">The ability to verify a piece of software using automated unit tests offers many desirable benefits.</span></span> <span data-ttu-id="442fe-113">Все знают, что хорошие тесты уменьшают количество дефектов программного обеспечения в приложении и увеличивают качество приложения, но наличие модульных тестов далеко не просто найдет ошибки.</span><span class="sxs-lookup"><span data-stu-id="442fe-113">Everyone knows that good tests will reduce the number of software defects in an application and increase the application’s quality - but having unit tests in place goes far beyond just finding bugs.</span></span>

<span data-ttu-id="442fe-114">Хороший набор модульных тестов позволяет команде разработчиков экономить время и сохранять контроль над создаваемым им программным обеспечением.</span><span class="sxs-lookup"><span data-stu-id="442fe-114">A good unit test suite allows a development team to save time and remain in control of the software they create.</span></span> <span data-ttu-id="442fe-115">Команда может вносить изменения в существующий код, выполнять рефакторинг, реконструировать и изменять структуру программного обеспечения для удовлетворения новых требований и добавлять новые компоненты в приложение, в то время как знание набора тестов может проверить поведение приложения.</span><span class="sxs-lookup"><span data-stu-id="442fe-115">A team can make changes to existing code, refactor, redesign, and restructure software to meet new requirements, and add new components into an application all while knowing the test suite can verify the application’s behavior.</span></span> <span data-ttu-id="442fe-116">Модульные тесты являются частью цикла быстрого отзыва, чтобы упростить изменение и сохранить сопровождение программного обеспечения по мере роста сложности.</span><span class="sxs-lookup"><span data-stu-id="442fe-116">Unit tests are part of a quick feedback cycle to facilitate change and preserve the maintainability of software as complexity increases.</span></span>

<span data-ttu-id="442fe-117">Однако модульное тестирование имеет цену.</span><span class="sxs-lookup"><span data-stu-id="442fe-117">Unit testing comes with a price, however.</span></span> <span data-ttu-id="442fe-118">Команда должна вкладывать время в создание и обслуживание модульных тестов.</span><span class="sxs-lookup"><span data-stu-id="442fe-118">A team has to invest the time to create and maintain unit tests.</span></span> <span data-ttu-id="442fe-119">Объем усилий, необходимых для создания этих тестов, напрямую связан с **тестированием** базового программного обеспечения.</span><span class="sxs-lookup"><span data-stu-id="442fe-119">The amount of effort required to create these tests is directly related to the **testability** of the underlying software.</span></span> <span data-ttu-id="442fe-120">Насколько легко тестировать программное обеспечение?</span><span class="sxs-lookup"><span data-stu-id="442fe-120">How easy is the software to test?</span></span> <span data-ttu-id="442fe-121">Группа разработки программного обеспечения с учетом пригодности для тестирования создаст эффективные тесты быстрее, чем команда, работающая с нетестируемым программным обеспечением.</span><span class="sxs-lookup"><span data-stu-id="442fe-121">A team designing software with testability in mind will create effective tests faster than the team working with un-testable software.</span></span>

<span data-ttu-id="442fe-122">Корпорация Майкрософт разработала ADO.NET Entity Framework 4,0 (EF4) с учетом пригодности для тестирования.</span><span class="sxs-lookup"><span data-stu-id="442fe-122">Microsoft designed the ADO.NET Entity Framework 4.0 (EF4) with testability in mind.</span></span> <span data-ttu-id="442fe-123">Это не означает, что разработчики будут писать модульные тесты для самого кода платформы.</span><span class="sxs-lookup"><span data-stu-id="442fe-123">This doesn’t mean developers will be writing unit tests against framework code itself.</span></span> <span data-ttu-id="442fe-124">Вместо этого цели тестирования для EF4 упрощают создание тестируемого кода, который строится на основе платформы.</span><span class="sxs-lookup"><span data-stu-id="442fe-124">Instead, the testability goals for EF4 make it easy to create testable code that builds on top of the framework.</span></span> <span data-ttu-id="442fe-125">Прежде чем приступать к рассмотрению конкретных примеров, важно понимать качество тестируемого кода.</span><span class="sxs-lookup"><span data-stu-id="442fe-125">Before we look at specific examples, it’s worthwhile to understand the qualities of testable code.</span></span>

### <a name="the-qualities-of-testable-code"></a><span data-ttu-id="442fe-126">Качество тестируемого кода</span><span class="sxs-lookup"><span data-stu-id="442fe-126">The Qualities of Testable Code</span></span>

<span data-ttu-id="442fe-127">Код, который легко протестировать, всегда будет иметь по крайней мере две особенности.</span><span class="sxs-lookup"><span data-stu-id="442fe-127">Code that is easy to test will always exhibit at least two traits.</span></span> <span data-ttu-id="442fe-128">Во-первых, можно легко **наблюдать**за тестируемым кодом.</span><span class="sxs-lookup"><span data-stu-id="442fe-128">First, testable code is easy to **observe**.</span></span> <span data-ttu-id="442fe-129">При наличии некоторого набора входных данных должно быть легко наблюдать за выходом кода.</span><span class="sxs-lookup"><span data-stu-id="442fe-129">Given some set of inputs, it should be easy to observe the output of the code.</span></span> <span data-ttu-id="442fe-130">Например, проверка следующего метода проста, поскольку метод непосредственно возвращает результат вычисления.</span><span class="sxs-lookup"><span data-stu-id="442fe-130">For example, testing the following method is easy because the method directly returns the result of a calculation.</span></span>

``` csharp
    public int Add(int x, int y) {
        return x + y;
    }
```

<span data-ttu-id="442fe-131">Тестирование метода сложно, если метод записывает вычисленное значение в сетевой сокет, таблицу базы данных или файл, как в приведенном ниже коде.</span><span class="sxs-lookup"><span data-stu-id="442fe-131">Testing a method is difficult if the method writes the computed value into a network socket, a database table, or a file like the following code.</span></span> <span data-ttu-id="442fe-132">Тест должен выполнить дополнительную работу, чтобы получить значение.</span><span class="sxs-lookup"><span data-stu-id="442fe-132">The test has to perform additional work to retrieve the value.</span></span>

``` csharp
    public void AddAndSaveToFile(int x, int y) {
         var results = string.Format("The answer is {0}", x + y);
         File.WriteAllText("results.txt", results);
    }
```

<span data-ttu-id="442fe-133">Во-вторых, легко **изолировать**тестируемый код.</span><span class="sxs-lookup"><span data-stu-id="442fe-133">Secondly, testable code is easy to **isolate**.</span></span> <span data-ttu-id="442fe-134">Давайте воспользуемся следующим псевдокодом в качестве неправильного примера тестируемого кода.</span><span class="sxs-lookup"><span data-stu-id="442fe-134">Let’s use the following pseudo-code as a bad example of testable code.</span></span>

``` csharp
    public int ComputePolicyValue(InsurancePolicy policy) {
        using (var connection = new SqlConnection("dbConnection"))
        using (var command = new SqlCommand(query, connection)) {

            // business calculations omitted ...               

            if (totalValue > notificationThreshold) {
                var message = new MailMessage();
                message.Subject = "Warning!";
                var client = new SmtpClient();
                client.Send(message);
            }
        }
        return totalValue;
    }
```

<span data-ttu-id="442fe-135">Этот метод прост в наблюдении — можно передать страховую полис и проверить, что возвращаемое значение соответствует ожидаемому результату.</span><span class="sxs-lookup"><span data-stu-id="442fe-135">The method is easy to observe – we can pass in an insurance policy and verify the return value matches an expected result.</span></span> <span data-ttu-id="442fe-136">Однако для тестирования метода необходимо установить базу данных с правильной схемой и настроить SMTP-сервер на случай, если метод попытается отправить сообщение электронной почты.</span><span class="sxs-lookup"><span data-stu-id="442fe-136">However, to test the method we’ll need to have a database installed with the correct schema, and configure the SMTP server in case the method tries to send an email.</span></span>

<span data-ttu-id="442fe-137">Модульный тест хочет проверить логику вычисления внутри метода, но тест может завершиться ошибкой из-за того, что сервер электронной почты находится в автономном режиме или если сервер базы данных был перемещен.</span><span class="sxs-lookup"><span data-stu-id="442fe-137">The unit test only wants to verify the calculation logic inside the method, but the test might fail because the email server is offline, or because the database server moved.</span></span> <span data-ttu-id="442fe-138">Обе эти ошибки не связаны с поведением, которое тест желает проверить.</span><span class="sxs-lookup"><span data-stu-id="442fe-138">Both of these failures are unrelated to the behavior the test wants to verify.</span></span> <span data-ttu-id="442fe-139">Такое поведение сложно изолировать.</span><span class="sxs-lookup"><span data-stu-id="442fe-139">The behavior is difficult to isolate.</span></span>

<span data-ttu-id="442fe-140">Разработчики программного обеспечения, стремящиеся написать тестируемый код, часто стремятся поддерживать разделение проблем в написанном им коде.</span><span class="sxs-lookup"><span data-stu-id="442fe-140">Software developers who strive to write testable code often strive to maintain a separation of concerns in the code they write.</span></span> <span data-ttu-id="442fe-141">Приведенный выше метод должен сосредоточиться на бизнес-вычислениях и делегировать сведения о реализации базы данных и электронной почты другим компонентам.</span><span class="sxs-lookup"><span data-stu-id="442fe-141">The above method should focus on the business calculations and delegate the database and email implementation details to other components.</span></span> <span data-ttu-id="442fe-142">Роберт C. Мартен вызывает этот принцип единой ответственности.</span><span class="sxs-lookup"><span data-stu-id="442fe-142">Robert C. Martin calls this the Single Responsibility Principle.</span></span> <span data-ttu-id="442fe-143">Объект должен инкапсулировать одну, более узкую ответственность, например вычисление значения политики.</span><span class="sxs-lookup"><span data-stu-id="442fe-143">An object should encapsulate a single, narrow responsibility, like calculating the value of a policy.</span></span> <span data-ttu-id="442fe-144">Все остальные работы с базами данных и уведомлениями должны быть ответственными за другой объект.</span><span class="sxs-lookup"><span data-stu-id="442fe-144">All other database and notification work should be the responsibility of some other object.</span></span> <span data-ttu-id="442fe-145">Код, написанный таким способом, проще изолировать, поскольку он сосредоточен на одной задаче.</span><span class="sxs-lookup"><span data-stu-id="442fe-145">Code written in this fashion is easier to isolate because it is focused on a single task.</span></span>

<span data-ttu-id="442fe-146">В .NET есть абстракции, которые необходимо соблюдать принципу единой ответственности и достичь изоляции.</span><span class="sxs-lookup"><span data-stu-id="442fe-146">In .NET we have the abstractions we need to follow the Single Responsibility Principle and achieve isolation.</span></span> <span data-ttu-id="442fe-147">Мы можем использовать определения интерфейсов и заставить код использовать абстракцию интерфейса вместо конкретного типа.</span><span class="sxs-lookup"><span data-stu-id="442fe-147">We can use interface definitions and force the code to use the interface abstraction instead of a concrete type.</span></span> <span data-ttu-id="442fe-148">Далее в этом документе мы посмотрим, как метод, похожий на приведенный выше, может работать с интерфейсами, которые *выглядят* так, как они будут взаимодействовать с базой данных.</span><span class="sxs-lookup"><span data-stu-id="442fe-148">Later in this paper we’ll see how a method like the bad example presented above can work with interfaces that *look* like they will talk to the database.</span></span> <span data-ttu-id="442fe-149">Однако во время тестирования можно заменить фиктивную реализацию, которая не будет обращаться к базе данных, а хранить данные в памяти.</span><span class="sxs-lookup"><span data-stu-id="442fe-149">At test time, however, we can substitute a dummy implementation that doesn’t talk to the database but instead holds data in memory.</span></span> <span data-ttu-id="442fe-150">Эта фиктивная реализация будет изолировать код от несвязанных проблем в коде доступа к данным или конфигурации базы данных.</span><span class="sxs-lookup"><span data-stu-id="442fe-150">This dummy implementation will isolate the code from unrelated problems in the data access code or database configuration.</span></span>

<span data-ttu-id="442fe-151">Существуют дополнительные преимущества изоляции.</span><span class="sxs-lookup"><span data-stu-id="442fe-151">There are additional benefits to isolation.</span></span> <span data-ttu-id="442fe-152">Вычисление бизнеса в последнем методе должно выполняться всего несколько миллисекунд, но сам тест может выполняться в течение нескольких секунд, так как код переключается по сети и обращается к различным серверам.</span><span class="sxs-lookup"><span data-stu-id="442fe-152">The business calculation in the last method should only take a few milliseconds to execute, but the test itself might run for several seconds as the code hops around the network and talks to various servers.</span></span> <span data-ttu-id="442fe-153">Модульные тесты должны выполняться быстро, чтобы упростить небольшие изменения.</span><span class="sxs-lookup"><span data-stu-id="442fe-153">Unit tests should run fast to facilitate small changes.</span></span> <span data-ttu-id="442fe-154">Модульные тесты также должны быть повторяемыми и не завершатся, так как в компоненте, не связанном с тестом, возникла проблема.</span><span class="sxs-lookup"><span data-stu-id="442fe-154">Unit tests should also be repeatable and not fail because a component unrelated to the test has a problem.</span></span> <span data-ttu-id="442fe-155">Написание кода, который легко наблюдать и изолировать, означает, что разработчики будут более простым написанием тестов для кода, тратить меньше времени на ожидание выполнения тестов и, что более важно, тратить меньше времени на отслеживание ошибок, которые не существуют.</span><span class="sxs-lookup"><span data-stu-id="442fe-155">Writing code that is easy to observe and to isolate means developers will have an easier time writing tests for the code, spend less time waiting for tests to execute, and more importantly, spend less time tracking down bugs that do not exist.</span></span>

<span data-ttu-id="442fe-156">Надеюсь, что вы можете оценить преимущества тестирования и понять, какие качества проверяются в коде.</span><span class="sxs-lookup"><span data-stu-id="442fe-156">Hopefully you can appreciate the benefits of testing and understand the qualities that testable code exhibits.</span></span> <span data-ttu-id="442fe-157">Мы будем обращаться к написанию кода, который работает с EF4, чтобы сохранить данные в базе данных, оставаясь видимой и легко изолированной, но сначала мы будем ограничивать наши усилия, чтобы обсудить тестируемые схемы для доступа к данным.</span><span class="sxs-lookup"><span data-stu-id="442fe-157">We are about to address how to write code that works with EF4 to save data into a database while remaining observable and easy to isolate, but first we’ll narrow our focus to discuss testable designs for data access.</span></span>

## <a name="design-patterns-for-data-persistence"></a><span data-ttu-id="442fe-158">Шаблоны проектирования для сохраняемости данных</span><span class="sxs-lookup"><span data-stu-id="442fe-158">Design Patterns for Data Persistence</span></span>

<span data-ttu-id="442fe-159">В обоих неверных примерах, приведенных выше, имелось слишком много обязанностей.</span><span class="sxs-lookup"><span data-stu-id="442fe-159">Both of the bad examples presented earlier had too many responsibilities.</span></span> <span data-ttu-id="442fe-160">Первому неверному примеру пришлось выполнить вычисление *и* записать в файл.</span><span class="sxs-lookup"><span data-stu-id="442fe-160">The first bad example had to perform a calculation *and* write to a file.</span></span> <span data-ttu-id="442fe-161">Во втором неверном примере пришлось считывать данные из базы данных *и* выполнять бизнес-расчет *и* отправлять электронную почту.</span><span class="sxs-lookup"><span data-stu-id="442fe-161">The second bad example had to read data from a database *and* perform a business calculation *and* send email.</span></span> <span data-ttu-id="442fe-162">Разрабатывая небольшие методы, которые разделяют проблемы и делегируют ответственность другим компонентам, вы сделаете отличный шаг для написания тестируемого кода.</span><span class="sxs-lookup"><span data-stu-id="442fe-162">By designing smaller methods that separate concerns and delegate responsibility to other components you’ll make great strides towards writing testable code.</span></span> <span data-ttu-id="442fe-163">Целью является создание функциональных возможностей путем составления действий из небольших и специализированных абстракций.</span><span class="sxs-lookup"><span data-stu-id="442fe-163">The goal is to build functionality by composing actions from small and focused abstractions.</span></span>

<span data-ttu-id="442fe-164">Когда дело доходит до сохранения данных, немалые и незначительные абстракции, которые мы будем искать, настолько распространены, что они были описаны как конструктивные шаблоны.</span><span class="sxs-lookup"><span data-stu-id="442fe-164">When it comes to data persistence the small and focused abstractions we are looking for are so common they’ve been documented as design patterns.</span></span> <span data-ttu-id="442fe-165">Для описания этих шаблонов в выпуске использовалась первая работа с книгами по архитектуре корпоративных приложений Фаулер.</span><span class="sxs-lookup"><span data-stu-id="442fe-165">Martin Fowler’s book Patterns of Enterprise Application Architecture was the first work to describe these patterns in print.</span></span> <span data-ttu-id="442fe-166">Мы предоставим краткое описание этих шаблонов в следующих разделах, прежде чем мы покажем, как эти ADO.NET Entity Framework реализованы и работают с этими шаблонами.</span><span class="sxs-lookup"><span data-stu-id="442fe-166">We’ll provide a brief description of these patterns in the following sections before we show how these ADO.NET Entity Framework implements and works with these patterns.</span></span>

### <a name="the-repository-pattern"></a><span data-ttu-id="442fe-167">Шаблон репозитория</span><span class="sxs-lookup"><span data-stu-id="442fe-167">The Repository Pattern</span></span>

<span data-ttu-id="442fe-168">Фаулер говорит о том, что между уровнями домена и сопоставления данных в репозитории используется интерфейс, подобный коллекции, для доступа к объектам домена.</span><span class="sxs-lookup"><span data-stu-id="442fe-168">Fowler says a repository “mediates between the domain and data mapping layers using a collection-like interface for accessing domain objects”.</span></span> <span data-ttu-id="442fe-169">Цель шаблона репозитория — изолировать код от минутиае доступа к данным, и как мы видели более раннюю изоляцию, является обязательной характеристикой для тестирования.</span><span class="sxs-lookup"><span data-stu-id="442fe-169">The goal of the repository pattern is to isolate code from the minutiae of data access, and as we saw earlier isolation is a required trait for testability.</span></span>

<span data-ttu-id="442fe-170">Ключом к изоляции является то, как репозиторий предоставляет объекты, используя интерфейс, подобный коллекции.</span><span class="sxs-lookup"><span data-stu-id="442fe-170">The key to the isolation is how the repository exposes objects using a collection-like interface.</span></span> <span data-ttu-id="442fe-171">Логика, которую вы пишете для использования в репозитории, не имеет представления о том, как репозиторий будет материализовать запрашиваемые объекты.</span><span class="sxs-lookup"><span data-stu-id="442fe-171">The logic you write to use the repository has no idea how the repository will materialize the objects you request.</span></span> <span data-ttu-id="442fe-172">Репозиторий может взаимодействовать с базой данных или просто возвращать объекты из коллекции в памяти.</span><span class="sxs-lookup"><span data-stu-id="442fe-172">The repository might talk to a database, or it might just return objects from an in-memory collection.</span></span> <span data-ttu-id="442fe-173">Весь код должен быть уверен в том, что репозиторий поддерживает коллекцию, и вы можете получать, добавлять и удалять объекты из коллекции.</span><span class="sxs-lookup"><span data-stu-id="442fe-173">All your code needs to know is that the repository appears to maintain the collection, and you can retrieve, add, and delete objects from the collection.</span></span>

<span data-ttu-id="442fe-174">В существующих приложениях .NET конкретный репозиторий часто наследуется от универсального интерфейса, подобного следующему:</span><span class="sxs-lookup"><span data-stu-id="442fe-174">In existing .NET applications a concrete repository often inherits from a generic interface like the following:</span></span>

``` csharp
    public interface IRepository<T> {       
        IEnumerable<T> FindAll();
        IEnumerable<T> FindBy(Expression<Func\<T, bool>> predicate);
        T FindById(int id);
        void Add(T newEntity);
        void Remove(T entity);
    }
```

<span data-ttu-id="442fe-175">Мы сделаем несколько изменений в определении интерфейса, когда мы предоставляем реализацию для EF4, но основная концепция остается неизменной.</span><span class="sxs-lookup"><span data-stu-id="442fe-175">We’ll make a few changes to the interface definition when we provide an implementation for EF4, but the basic concept remains the same.</span></span> <span data-ttu-id="442fe-176">Код может использовать конкретный репозиторий, реализующий этот интерфейс, чтобы получить сущность по значению первичного ключа, получить коллекцию сущностей на основе вычисления предиката или просто получить все доступные сущности.</span><span class="sxs-lookup"><span data-stu-id="442fe-176">Code can use a concrete repository implementing this interface to retrieve an entity by its primary key value, to retrieve a collection of entities based on the evaluation of a predicate, or simply retrieve all available entities.</span></span> <span data-ttu-id="442fe-177">Код также может добавлять и удалять сущности через интерфейс репозитория.</span><span class="sxs-lookup"><span data-stu-id="442fe-177">The code can also add and remove entities through the repository interface.</span></span>

<span data-ttu-id="442fe-178">Учитывая IRepository объектов Employee, код может выполнять следующие операции.</span><span class="sxs-lookup"><span data-stu-id="442fe-178">Given an IRepository of Employee objects, code can perform the following operations.</span></span>

``` csharp
    var employeesNamedScott =
        repository
            .FindBy(e => e.Name == "Scott")
            .OrderBy(e => e.HireDate);
    var firstEmployee = repository.FindById(1);
    var newEmployee = new Employee() {/*... */};
    repository.Add(newEmployee);
```

<span data-ttu-id="442fe-179">Поскольку код использует интерфейс (IRepository для Employee), мы можем предоставить код с различными реализациями интерфейса.</span><span class="sxs-lookup"><span data-stu-id="442fe-179">Since the code is using an interface (IRepository of Employee), we can provide the code with different implementations of the interface.</span></span> <span data-ttu-id="442fe-180">Одна из реализаций может быть реализацией, поддерживаемой EF4, и сохранять объекты в базе данных Microsoft SQL Server.</span><span class="sxs-lookup"><span data-stu-id="442fe-180">One implementation might be an implementation backed by EF4 and persisting objects into a Microsoft SQL Server database.</span></span> <span data-ttu-id="442fe-181">Другая реализация (используемая во время тестирования) может быть создана в виде списка объектов Employee в памяти.</span><span class="sxs-lookup"><span data-stu-id="442fe-181">A different implementation (one we use during testing) might be backed by an in-memory List of Employee objects.</span></span> <span data-ttu-id="442fe-182">Интерфейс позволяет достичь изоляции в коде.</span><span class="sxs-lookup"><span data-stu-id="442fe-182">The interface will help to achieve isolation in the code.</span></span>

<span data-ttu-id="442fe-183">Обратите внимание, что интерфейс IRepository @ no__t-0T @ no__t-1 не предоставляет операции сохранения.</span><span class="sxs-lookup"><span data-stu-id="442fe-183">Notice the IRepository&lt;T&gt; interface does not expose a Save operation.</span></span> <span data-ttu-id="442fe-184">Как обновить существующие объекты?</span><span class="sxs-lookup"><span data-stu-id="442fe-184">How do we update existing objects?</span></span> <span data-ttu-id="442fe-185">Вы можете использовать определения IRepository, которые включают операцию сохранения, и реализации этих репозиториев должны немедленно сохранить объект в базе данных.</span><span class="sxs-lookup"><span data-stu-id="442fe-185">You might come across IRepository definitions that do include the Save operation, and implementations of these repositories will need to immediately persist an object into the database.</span></span> <span data-ttu-id="442fe-186">Однако во многих приложениях не нужно сохранять объекты по отдельности.</span><span class="sxs-lookup"><span data-stu-id="442fe-186">However, in many applications we don’t want to persist objects individually.</span></span> <span data-ttu-id="442fe-187">Вместо этого мы хотим перенести объекты в жизнь, возможно, из разных репозиториев, изменить эти объекты как часть бизнес-деятельности, а затем сохранить все объекты в рамках одной атомарной операции.</span><span class="sxs-lookup"><span data-stu-id="442fe-187">Instead, we want to bring objects to life, perhaps from different repositories, modify those objects as part of a business activity, and then persist all the objects as part of a single, atomic operation.</span></span> <span data-ttu-id="442fe-188">К счастью, существует шаблон, позволяющий использовать этот тип поведения.</span><span class="sxs-lookup"><span data-stu-id="442fe-188">Fortunately, there is a pattern to allow this type of behavior.</span></span>

### <a name="the-unit-of-work-pattern"></a><span data-ttu-id="442fe-189">Шаблон единицы работы</span><span class="sxs-lookup"><span data-stu-id="442fe-189">The Unit of Work Pattern</span></span>

<span data-ttu-id="442fe-190">Фаулер говорит, что единица работы будет поддерживать список объектов, затронутых бизнес-транзакцией, и координирует запись изменений и разрешение проблем параллелизма.</span><span class="sxs-lookup"><span data-stu-id="442fe-190">Fowler says a unit of work will “maintain a list of objects affected by a business transaction and coordinates the writing out of changes and the resolution of concurrency problems”.</span></span> <span data-ttu-id="442fe-191">Ответственность за отработку изменений объектов, переносимых в жизнь из репозитория, и сохранение изменений, внесенных в объекты при указании единицы работы для фиксации изменений, лежит за единицу работы.</span><span class="sxs-lookup"><span data-stu-id="442fe-191">It is the responsibility of the unit of work to track changes to the objects we bring to life from a repository and persist any changes we’ve made to the objects when we tell the unit of work to commit the changes.</span></span> <span data-ttu-id="442fe-192">Кроме того, эта единица отвечает за то, чтобы новые объекты, добавленные во все репозитории, были вставлены в базу данных, а также операции удаления.</span><span class="sxs-lookup"><span data-stu-id="442fe-192">It’s also the responsibility of the unit of work to take the new objects we’ve added to all repositories and insert the objects into a database, and also mange deletion.</span></span>

<span data-ttu-id="442fe-193">Если вы когда-либо работали с наборами данных ADO.NET, вы уже знакомы с шаблоном единицы работы.</span><span class="sxs-lookup"><span data-stu-id="442fe-193">If you’ve ever done any work with ADO.NET DataSets then you’ll already be familiar with the unit of work pattern.</span></span> <span data-ttu-id="442fe-194">Наборы данных ADO.NET имели возможность отслеживании обновлений, удалений и вставки объектов DataRow и могли бы (с помощью TableAdapter) согласовать все наши изменения с базой.</span><span class="sxs-lookup"><span data-stu-id="442fe-194">ADO.NET DataSets had the ability to track our updates, deletions, and insertion of DataRow objects and could (with the help of a TableAdapter) reconcile all our changes to a database.</span></span> <span data-ttu-id="442fe-195">Однако объекты DataSet моделируют отключенное подмножество базовой базы данных.</span><span class="sxs-lookup"><span data-stu-id="442fe-195">However, DataSet objects model a disconnected subset of the underlying database.</span></span> <span data-ttu-id="442fe-196">Шаблон единицы работы имеет такое же поведение, но работает с бизнес-объектами и объектами предметной области, изолированными от кода доступа к данным и не осведомленных о базе данных.</span><span class="sxs-lookup"><span data-stu-id="442fe-196">The unit of work pattern exhibits the same behavior, but works with business objects and domain objects that are isolated from data access code and unaware of the database.</span></span>

<span data-ttu-id="442fe-197">Абстракция для моделирования единицы работы в коде .NET может выглядеть следующим образом:</span><span class="sxs-lookup"><span data-stu-id="442fe-197">An abstraction to model the unit of work in .NET code might look like the following:</span></span>

``` csharp
    public interface IUnitOfWork {
        IRepository<Employee> Employees { get; }
        IRepository<Order> Orders { get; }
        IRepository<Customer> Customers { get; }
        void Commit();
    }
```

<span data-ttu-id="442fe-198">Предоставляя ссылки на репозитории из единицы работы, можно гарантировать, что один объект единицы работы может следить за всеми сущностями, материализованными во время бизнес-транзакции.</span><span class="sxs-lookup"><span data-stu-id="442fe-198">By exposing repository references from the unit of work we can ensure a single unit of work object has the ability to track all entities materialized during a business transaction.</span></span> <span data-ttu-id="442fe-199">Реализация метода Commit для реальной единицы работы заключается в том, что все волшебные ситуации позволяют согласовать изменения в памяти с базой данных.</span><span class="sxs-lookup"><span data-stu-id="442fe-199">The implementation of the Commit method for a real unit of work is where all the magic happens to reconcile in-memory changes with the database.</span></span> 

<span data-ttu-id="442fe-200">При наличии ссылки на IUnitOfWork код может вносить изменения в бизнес-объекты, полученные из одного или нескольких репозиториев, и сохранять все изменения с помощью операции атомарной фиксации.</span><span class="sxs-lookup"><span data-stu-id="442fe-200">Given an IUnitOfWork reference, code can make changes to business objects retrieved from one or more repositories and save all the changes using the atomic Commit operation.</span></span>

``` csharp
    var firstEmployee = unitofWork.Employees.FindById(1);
    var firstCustomer = unitofWork.Customers.FindById(1);
    firstEmployee.Name = "Alex";
    firstCustomer.Name = "Christopher";
    unitofWork.Commit();
```

### <a name="the-lazy-load-pattern"></a><span data-ttu-id="442fe-201">Шаблон отложенной загрузки</span><span class="sxs-lookup"><span data-stu-id="442fe-201">The Lazy Load Pattern</span></span>

<span data-ttu-id="442fe-202">Фаулер использует имя "Lazy Load" для описания "объекта, который не содержит все необходимые данные, но знает, как его получить".</span><span class="sxs-lookup"><span data-stu-id="442fe-202">Fowler uses the name lazy load to describe “an object that doesn’t contain all of the data you need but knows how to get it”.</span></span> <span data-ttu-id="442fe-203">Прозрачная отложенная загрузка является важной функцией при написании тестируемого бизнес-кода и работе с реляционной базой данных.</span><span class="sxs-lookup"><span data-stu-id="442fe-203">Transparent lazy loading is an important feature to have when writing testable business code and working with a relational database.</span></span> <span data-ttu-id="442fe-204">В качестве примера рассмотрим следующий код.</span><span class="sxs-lookup"><span data-stu-id="442fe-204">As an example, consider the following code.</span></span>

``` csharp
    var employee = repository.FindById(id);
    // ... and later ...
    foreach(var timeCard in employee.TimeCards) {
        // .. manipulate the timeCard
    }
```

<span data-ttu-id="442fe-205">Как заполняется коллекция Тимекардс?</span><span class="sxs-lookup"><span data-stu-id="442fe-205">How is the TimeCards collection populated?</span></span> <span data-ttu-id="442fe-206">Есть два возможных ответа.</span><span class="sxs-lookup"><span data-stu-id="442fe-206">There are two possible answers.</span></span> <span data-ttu-id="442fe-207">Один из ответов заключается в том, что при запросе на получение сотрудника в репозитории сотрудников отправляется запрос для получения сотрудника вместе со сведениями о карте времени, связанными с сотрудниками.</span><span class="sxs-lookup"><span data-stu-id="442fe-207">One answer is that the employee repository, when asked to fetch an employee, issues a query to retrieve both the employee along with the employee’s associated time card information.</span></span> <span data-ttu-id="442fe-208">В реляционных базах данных это обычно требует запроса с предложением JOIN и может привести к извлечению дополнительных сведений, чем требуется приложению.</span><span class="sxs-lookup"><span data-stu-id="442fe-208">In relational databases this generally requires a query with a JOIN clause and may result in retrieving more information than an application needs.</span></span> <span data-ttu-id="442fe-209">Что делать, если приложению не нужно касаться свойства Тимекардс?</span><span class="sxs-lookup"><span data-stu-id="442fe-209">What if the application never needs to touch the TimeCards property?</span></span>

<span data-ttu-id="442fe-210">Вторым ответом является загрузка свойства Тимекардс "по требованию".</span><span class="sxs-lookup"><span data-stu-id="442fe-210">A second answer is to load the TimeCards property “on demand”.</span></span> <span data-ttu-id="442fe-211">Эта отложенная загрузка является неявной и прозрачной для бизнес-логики, так как код не вызывает специальные интерфейсы API для получения сведений о карте времени.</span><span class="sxs-lookup"><span data-stu-id="442fe-211">This lazy loading is implicit and transparent to the business logic because the code does not invoke special APIs to retrieve time card information.</span></span> <span data-ttu-id="442fe-212">В коде предполагается, что сведения о карте времени имеются при необходимости.</span><span class="sxs-lookup"><span data-stu-id="442fe-212">The code assumes the time card information is present when needed.</span></span> <span data-ttu-id="442fe-213">Существует несколько волшебных усилий, связанных с отложенной загрузкой, которая обычно включает Перехват вызовов методов в среде выполнения.</span><span class="sxs-lookup"><span data-stu-id="442fe-213">There is some magic involved with lazy loading that generally involves runtime interception of method invocations.</span></span> <span data-ttu-id="442fe-214">Перехват кода отвечает за общение с базой данных и получение сведений о карте времени, при этом бизнес-логика может быть бизнес-логикой.</span><span class="sxs-lookup"><span data-stu-id="442fe-214">The intercepting code is responsible for talking to the database and retrieving time card information while leaving the business logic free to be business logic.</span></span> <span data-ttu-id="442fe-215">Этот инструмент «ленивая» загрузка позволяет бизнес-коду изолировать себя от операций извлечения данных и приводить к более тестируемому коду.</span><span class="sxs-lookup"><span data-stu-id="442fe-215">This lazy load magic allows the business code to isolate itself from data retrieval operations and results in more testable code.</span></span>

<span data-ttu-id="442fe-216">Недостатком отложенной нагрузки является то, что если приложению *требуется информация* о карте времени, код будет выполнять дополнительный запрос.</span><span class="sxs-lookup"><span data-stu-id="442fe-216">The drawback to a lazy load is that when an application *does* need the time card information the code will execute an additional query.</span></span> <span data-ttu-id="442fe-217">Это не имеет значения для многих приложений, но для приложений, зависящих от производительности, или приложений, которые проходят через несколько объектов Employee и выполняют запрос для получения карт времени во время каждой итерации цикла (проблема часто называется N + 1 проблема с запросом), отложенная загрузка является перетаскиванием.</span><span class="sxs-lookup"><span data-stu-id="442fe-217">This isn’t a concern for many applications, but for performance sensitive applications or applications looping through a number of employee objects and executing a query to retrieve time cards during each iteration of the loop (a problem often referred to as the N+1 query problem), lazy loading is a drag.</span></span> <span data-ttu-id="442fe-218">В этих сценариях приложение может захотеть заранее загрузить сведения о карте времени с максимально эффективным способом.</span><span class="sxs-lookup"><span data-stu-id="442fe-218">In these scenarios an application might want to eagerly load time card information in the most efficient manner possible.</span></span>

<span data-ttu-id="442fe-219">К счастью, мы увидим, как EF4 поддерживает неявные отложенные и эффективные загрузки, так как мы перейдем к следующему разделу и реализуем эти закономерности.</span><span class="sxs-lookup"><span data-stu-id="442fe-219">Fortunately, we’ll see how EF4 supports both implicit lazy loads and efficient eager loads as we move into the next section and implement these patterns.</span></span>

## <a name="implementing-patterns-with-the-entity-framework"></a><span data-ttu-id="442fe-220">Реализация шаблонов с помощью Entity Framework</span><span class="sxs-lookup"><span data-stu-id="442fe-220">Implementing Patterns with the Entity Framework</span></span>

<span data-ttu-id="442fe-221">Хорошая новость состоит в том, что все шаблоны проектирования, описанные в последнем разделе, легко реализовать с помощью EF4.</span><span class="sxs-lookup"><span data-stu-id="442fe-221">The good news is that all of the design patterns we described in the last section are straightforward to implement with EF4.</span></span> <span data-ttu-id="442fe-222">Чтобы продемонстрировать, что мы будем использовать простое ASP.NET приложение MVC для редактирования и показа сотрудников и связанных с ними сведений о времени.</span><span class="sxs-lookup"><span data-stu-id="442fe-222">To demonstrate we are going to use a simple ASP.NET MVC application to edit and display Employees and their associated time card information.</span></span> <span data-ttu-id="442fe-223">Начнем с использования следующих "простых старых объектов CLR" (POCO).</span><span class="sxs-lookup"><span data-stu-id="442fe-223">We’ll start by using the following “plain old CLR objects” (POCOs).</span></span> 

``` csharp
    public class Employee {
        public int Id { get; set; }
        public string Name { get; set; }
        public DateTime HireDate { get; set; }
        public ICollection<TimeCard> TimeCards { get; set; }
    }

    public class TimeCard {
        public int Id { get; set; }
        public int Hours { get; set; }
        public DateTime EffectiveDate { get; set; }
    }
```

<span data-ttu-id="442fe-224">Эти определения классов немного изменяются, так как мы рассмотрим различные подходы и функции EF4, но цель заключается в том, чтобы эти классы всегда были как можно устойчивыми игнорирующих (PI).</span><span class="sxs-lookup"><span data-stu-id="442fe-224">These class definitions will change slightly as we explore different approaches and features of EF4, but the intent is to keep these classes as persistence ignorant (PI) as possible.</span></span> <span data-ttu-id="442fe-225">Объект PI не знает, *как*или даже *Если*состояние, которое он хранит, находится внутри базы данных.</span><span class="sxs-lookup"><span data-stu-id="442fe-225">A PI object doesn’t know *how*, or even *if*, the state it holds lives inside a database.</span></span> <span data-ttu-id="442fe-226">Устройства PI и POCO переходят в руки с тестируемым программным обеспечением.</span><span class="sxs-lookup"><span data-stu-id="442fe-226">PI and POCOs go hand in hand with testable software.</span></span> <span data-ttu-id="442fe-227">Объекты, использующие алгоритм POCO, менее ограничены, более гибкы и проще в тестировании, так как они могут работать без базы данных.</span><span class="sxs-lookup"><span data-stu-id="442fe-227">Objects using a POCO approach are less constrained, more flexible, and easier to test because they can operate without a database present.</span></span>

<span data-ttu-id="442fe-228">С помощью POCO можно создать EDM (EDM) в Visual Studio (см. рис. 1).</span><span class="sxs-lookup"><span data-stu-id="442fe-228">With the POCOs in place we can create an Entity Data Model (EDM) in Visual Studio (see figure 1).</span></span> <span data-ttu-id="442fe-229">Мы не будем использовать EDM для создания кода для наших сущностей.</span><span class="sxs-lookup"><span data-stu-id="442fe-229">We will not use the EDM to generate code for our entities.</span></span> <span data-ttu-id="442fe-230">Вместо этого мы хотим использовать сущности, которые мы ловингли вручную.</span><span class="sxs-lookup"><span data-stu-id="442fe-230">Instead, we want to use the entities we lovingly craft by hand.</span></span> <span data-ttu-id="442fe-231">Мы будем использовать EDM только для создания нашей схемы базы данных и предоставления метаданных EF4, необходимых для отображения объектов в базе данных.</span><span class="sxs-lookup"><span data-stu-id="442fe-231">We will only use the EDM to generate our database schema and provide the metadata EF4 needs to map objects into the database.</span></span>

![EF Test_01](~/ef6/media/eftest-01.jpg)

<span data-ttu-id="442fe-233">**Рис. 1**</span><span class="sxs-lookup"><span data-stu-id="442fe-233">**Figure 1**</span></span>

<span data-ttu-id="442fe-234">Примечание. Если вы хотите сначала разработать модель EDM, можно создать чистый код POCO из EDM.</span><span class="sxs-lookup"><span data-stu-id="442fe-234">Note: if you want to develop the EDM model first, it is possible to generate clean, POCO code from the EDM.</span></span> <span data-ttu-id="442fe-235">Это можно сделать с помощью расширения Visual Studio 2010, предоставленного группой программирования данных.</span><span class="sxs-lookup"><span data-stu-id="442fe-235">You can do this with a Visual Studio 2010 extension provided by the Data Programmability team.</span></span> <span data-ttu-id="442fe-236">Чтобы скачать расширение, запустите Диспетчер расширений из меню "Сервис" в Visual Studio и выполните поиск в интерактивной коллекции шаблонов для "POCO" (см. рис. 2).</span><span class="sxs-lookup"><span data-stu-id="442fe-236">To download the extension, launch the Extension Manager from the Tools menu in Visual Studio and search the online gallery of templates for “POCO” (See figure 2).</span></span> <span data-ttu-id="442fe-237">Существует несколько шаблонов POCO, доступных для EF.</span><span class="sxs-lookup"><span data-stu-id="442fe-237">There are several POCO templates available for EF.</span></span> <span data-ttu-id="442fe-238">Дополнительные сведения об использовании шаблона см. в разделе "[Walkthrough: Шаблон POCO для Entity Framework @ no__t-0 ".</span><span class="sxs-lookup"><span data-stu-id="442fe-238">For more information on using the template, see “ [Walkthrough: POCO Template for the Entity Framework](https://blogs.msdn.com/adonet/pages/walkthrough-poco-template-for-the-entity-framework.aspx)”.</span></span>

![EF test_02](~/ef6/media/eftest-02.png)

<span data-ttu-id="442fe-240">**Рис. 2**</span><span class="sxs-lookup"><span data-stu-id="442fe-240">**Figure 2**</span></span>

<span data-ttu-id="442fe-241">Начиная с этой начальной точки POCO мы рассмотрим два разных подхода к тестируемому коду.</span><span class="sxs-lookup"><span data-stu-id="442fe-241">From this POCO starting point we will explore two different approaches to testable code.</span></span> <span data-ttu-id="442fe-242">Первый подход вызывает подход EF, так как он использует абстракции из API Entity Framework для реализации единиц работы и репозиториев.</span><span class="sxs-lookup"><span data-stu-id="442fe-242">The first approach I call the EF approach because it leverages abstractions from the Entity Framework API to implement units of work and repositories.</span></span> <span data-ttu-id="442fe-243">Во втором подходе мы создадим собственные абстракции пользовательского репозитория, а затем увидим преимущества и недостатки каждого подхода.</span><span class="sxs-lookup"><span data-stu-id="442fe-243">In the second approach we will create our own custom repository abstractions and then see the advantages and disadvantages of each approach.</span></span> <span data-ttu-id="442fe-244">Начнем с изучения подхода EF.</span><span class="sxs-lookup"><span data-stu-id="442fe-244">We’ll start by exploring the EF approach.</span></span>  

### <a name="an-ef-centric-implementation"></a><span data-ttu-id="442fe-245">Реализация, ориентированная на EF</span><span class="sxs-lookup"><span data-stu-id="442fe-245">An EF Centric Implementation</span></span>

<span data-ttu-id="442fe-246">Рассмотрим следующее действие контроллера из проекта MVC ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="442fe-246">Consider the following controller action from an ASP.NET MVC project.</span></span> <span data-ttu-id="442fe-247">Действие извлекает объект Employee и возвращает результат для отображения подробного представления сотрудника.</span><span class="sxs-lookup"><span data-stu-id="442fe-247">The action retrieves an Employee object and returns a result to display a detailed view of the employee.</span></span>

``` csharp
    public ViewResult Details(int id) {
        var employee = _unitOfWork.Employees
                                  .Single(e => e.Id == id);
        return View(employee);
    }
```

<span data-ttu-id="442fe-248">Тестируется ли код?</span><span class="sxs-lookup"><span data-stu-id="442fe-248">Is the code testable?</span></span> <span data-ttu-id="442fe-249">Существует по крайней мере два теста, которые необходимо проверить на поведении действия.</span><span class="sxs-lookup"><span data-stu-id="442fe-249">There are at least two tests we’d need to verify the action’s behavior.</span></span> <span data-ttu-id="442fe-250">Во-первых, мы хотим убедиться, что действие возвращает правильное представление — простой тест.</span><span class="sxs-lookup"><span data-stu-id="442fe-250">First, we’d like to verify the action returns the correct view – an easy test.</span></span> <span data-ttu-id="442fe-251">Мы также хотим написать тест для проверки того, что действие получает правильного сотрудника, и нам бы хотелось сделать это без выполнения кода для запроса к базе данных.</span><span class="sxs-lookup"><span data-stu-id="442fe-251">We’d also want to write a test to verify the action retrieves the correct employee, and we’d like to do this without executing code to query the database.</span></span> <span data-ttu-id="442fe-252">Помните, что мы хотим изолировать тестируемый код.</span><span class="sxs-lookup"><span data-stu-id="442fe-252">Remember we want to isolate the code under test.</span></span> <span data-ttu-id="442fe-253">Изоляция обеспечит сбой теста из-за ошибки в коде доступа к данным или конфигурации базы данных.</span><span class="sxs-lookup"><span data-stu-id="442fe-253">Isolation will ensure the test doesn’t fail because of a bug in the data access code or database configuration.</span></span> <span data-ttu-id="442fe-254">Если тест завершается неудачно, мы будем осведомлены об ошибке в логике контроллера, а не на каком-либо системном компоненте более низкого уровня.</span><span class="sxs-lookup"><span data-stu-id="442fe-254">If the test fails, we will know we have a bug in the controller logic, and not in some lower level system component.</span></span>

<span data-ttu-id="442fe-255">Для достижения изоляции нам потребуются некоторые абстракции, такие как интерфейсы, представленные ранее для репозиториев и единиц работы.</span><span class="sxs-lookup"><span data-stu-id="442fe-255">To achieve isolation we’ll need some abstractions like the interfaces we presented earlier for repositories and units of work.</span></span> <span data-ttu-id="442fe-256">Помните, что шаблон репозитория предназначен для исправлять между объектами домена и уровнем сопоставления данных.</span><span class="sxs-lookup"><span data-stu-id="442fe-256">Remember the repository pattern is designed to mediate between domain objects and the data mapping layer.</span></span> <span data-ttu-id="442fe-257">В этом сценарии EF4 *является* слоем сопоставления данных и уже предоставляет абстракцию, похожую на репозиторий с именем иобжектсет @ No__t-1T @ no__t-2 (из пространства имен System. Data. Objects).</span><span class="sxs-lookup"><span data-stu-id="442fe-257">In this scenario EF4 *is* the data mapping layer, and already provides a repository-like abstraction named IObjectSet&lt;T&gt; (from the System.Data.Objects namespace).</span></span> <span data-ttu-id="442fe-258">Определение интерфейса выглядит следующим образом.</span><span class="sxs-lookup"><span data-stu-id="442fe-258">The interface definition looks like the following.</span></span>

``` csharp
    public interface IObjectSet<TEntity> :
                     IQueryable<TEntity>,
                     IEnumerable<TEntity>,
                     IQueryable,
                     IEnumerable
                     where TEntity : class
    {
        void AddObject(TEntity entity);
        void Attach(TEntity entity);
        void DeleteObject(TEntity entity);
        void Detach(TEntity entity);
    }
```

<span data-ttu-id="442fe-259">Иобжектсет @ no__t-0T @ no__t-1 соответствует требованиям для репозитория, так как он напоминает коллекцию объектов (через IEnumerable @ no__t-2T @ no__t-3) и предоставляет методы для добавления и удаления объектов из имитации коллекции.</span><span class="sxs-lookup"><span data-stu-id="442fe-259">IObjectSet&lt;T&gt; meets the requirements for a repository because it resembles a collection of objects (via IEnumerable&lt;T&gt;) and provides methods to add and remove objects from the simulated collection.</span></span> <span data-ttu-id="442fe-260">Методы Attach и Detach предоставляют дополнительные возможности API EF4.</span><span class="sxs-lookup"><span data-stu-id="442fe-260">The Attach and Detach methods expose additional capabilities of the EF4 API.</span></span> <span data-ttu-id="442fe-261">Чтобы использовать Иобжектсет @ no__t-0T @ no__t-1 в качестве интерфейса для репозиториев, нам требуется абстракция единиц работы для привязки репозиториев вместе.</span><span class="sxs-lookup"><span data-stu-id="442fe-261">To use IObjectSet&lt;T&gt; as the interface for repositories we need a unit of work abstraction to bind repositories together.</span></span>

``` csharp
    public interface IUnitOfWork {
        IObjectSet<Employee> Employees { get; }
        IObjectSet<TimeCard> TimeCards { get; }
        void Commit();
    }
```

<span data-ttu-id="442fe-262">Одна конкретная реализация этого интерфейса будет обсуждаться с SQL Server и проста в создании с помощью класса ObjectContext из EF4.</span><span class="sxs-lookup"><span data-stu-id="442fe-262">One concrete implementation of this interface will talk to SQL Server and is easy to create using the ObjectContext class from EF4.</span></span> <span data-ttu-id="442fe-263">Класс ObjectContext — это реальная единица работы в API EF4.</span><span class="sxs-lookup"><span data-stu-id="442fe-263">The ObjectContext class is the real unit of work in the EF4 API.</span></span>

``` csharp
    public class SqlUnitOfWork : IUnitOfWork {
        public SqlUnitOfWork() {
            var connectionString =
                ConfigurationManager
                    .ConnectionStrings[ConnectionStringName]
                    .ConnectionString;
            _context = new ObjectContext(connectionString);
        }

        public IObjectSet<Employee> Employees {
            get { return _context.CreateObjectSet<Employee>(); }
        }

        public IObjectSet<TimeCard> TimeCards {
            get { return _context.CreateObjectSet<TimeCard>(); }
        }

        public void Commit() {
            _context.SaveChanges();
        }

        readonly ObjectContext _context;
        const string ConnectionStringName = "EmployeeDataModelContainer";
    }
```

<span data-ttu-id="442fe-264">Перевод Иобжектсет @ no__t-0T @ no__t-1 в жизнь — это так же просто, как вызов метода CreateObject объекта ObjectContext.</span><span class="sxs-lookup"><span data-stu-id="442fe-264">Bringing an IObjectSet&lt;T&gt; to life is as easy as invoking the CreateObjectSet method of the ObjectContext object.</span></span> <span data-ttu-id="442fe-265">В фоновом режиме платформа будет использовать метаданные, предоставленные в EDM, для создания конкретного объектного типа @ no__t-0T @ no__t-1.</span><span class="sxs-lookup"><span data-stu-id="442fe-265">Behind the scenes the framework will use the metadata we provided in the EDM to produce a concrete ObjectSet&lt;T&gt;.</span></span> <span data-ttu-id="442fe-266">Мы будем возвращать интерфейс Иобжектсет @ no__t-0T @ no__t-1, так как он поможет сохранить пригодность для тестирования в клиентском коде.</span><span class="sxs-lookup"><span data-stu-id="442fe-266">We’ll stick with returning the IObjectSet&lt;T&gt; interface because it will help preserve testability in client code.</span></span>

<span data-ttu-id="442fe-267">Эта конкретная реализация полезна в рабочей среде, но нам нужно сосредоточиться на том, как мы будем использовать нашу абстракцию IUnitOfWork для упрощения тестирования.</span><span class="sxs-lookup"><span data-stu-id="442fe-267">This concrete implementation is useful in production, but we need to focus on how we’ll use our IUnitOfWork abstraction to facilitate testing.</span></span>

### <a name="the-test-doubles"></a><span data-ttu-id="442fe-268">Двойная проверка</span><span class="sxs-lookup"><span data-stu-id="442fe-268">The Test Doubles</span></span>

<span data-ttu-id="442fe-269">Чтобы изолировать действие контроллера, потребуется возможность переключения между реальными единицами работы (с помощью ObjectContext) и тестовой двойной или «фиктивной» единицей работы (выполнение операций в памяти).</span><span class="sxs-lookup"><span data-stu-id="442fe-269">To isolate the controller action we’ll need the ability to switch between the real unit of work (backed by an ObjectContext) and a test double or “fake” unit of work (performing in-memory operations).</span></span> <span data-ttu-id="442fe-270">Распространенный подход к выполнению этого типа переключения заключается в том, чтобы не допустить создания экземпляром блока работы контроллером MVC, а передать единицу работы в контроллер в качестве параметра конструктора.</span><span class="sxs-lookup"><span data-stu-id="442fe-270">The common approach to perform this type of switching is to not let the MVC controller instantiate a unit of work, but instead pass the unit of work into the controller as a constructor parameter.</span></span>

``` csharp
    class EmployeeController : Controller {
      publicEmployeeController(IUnitOfWork unitOfWork)  {
          _unitOfWork = unitOfWork;
      }
      ...
    }
```

<span data-ttu-id="442fe-271">Приведенный выше код является примером внедрения зависимостей.</span><span class="sxs-lookup"><span data-stu-id="442fe-271">The above code is an example of dependency injection.</span></span> <span data-ttu-id="442fe-272">Мы не разрешают контроллеру создать зависимость (единицу работы), а затем внедрить зависимость в контроллер.</span><span class="sxs-lookup"><span data-stu-id="442fe-272">We don’t allow the controller to create it’s dependency (the unit of work) but inject the dependency into the controller.</span></span> <span data-ttu-id="442fe-273">В проекте MVC обычно используется настраиваемая фабрика контроллеров в сочетании с контейнером инверсии управления (IoC) для автоматизации внедрения зависимостей.</span><span class="sxs-lookup"><span data-stu-id="442fe-273">In an MVC project it is common to use a custom controller factory in combination with an inversion of control (IoC) container to automate dependency injection.</span></span> <span data-ttu-id="442fe-274">Эти разделы выходят за рамки этой статьи, но дополнительные сведения можно получить, выполнив ссылки в конце этой статьи.</span><span class="sxs-lookup"><span data-stu-id="442fe-274">These topics are beyond the scope of this article, but you can read more by following the references at the end of this article.</span></span>

<span data-ttu-id="442fe-275">Поддельная реализация блока работы, которую можно использовать для тестирования, может выглядеть следующим образом.</span><span class="sxs-lookup"><span data-stu-id="442fe-275">A fake unit of work implementation that we can use for testing might look like the following.</span></span>

``` csharp
    public class InMemoryUnitOfWork : IUnitOfWork {
        public InMemoryUnitOfWork() {
            Committed = false;
        }
        public IObjectSet<Employee> Employees {
            get;
            set;
        }

        public IObjectSet<TimeCard> TimeCards {
            get;
            set;
        }

        public bool Committed { get; set; }
        public void Commit() {
            Committed = true;
        }
    }
```

<span data-ttu-id="442fe-276">Обратите внимание, что поддельная единица работы предоставляет зафиксированное свойство.</span><span class="sxs-lookup"><span data-stu-id="442fe-276">Notice the fake unit of work exposes a Commited property.</span></span> <span data-ttu-id="442fe-277">Иногда бывает полезно добавить компоненты в имитацию класса, который упрощает тестирование.</span><span class="sxs-lookup"><span data-stu-id="442fe-277">It’s sometimes useful to add features to a fake class that facilitate testing.</span></span> <span data-ttu-id="442fe-278">В этом случае можно легко определить, фиксирует ли код единицу работы, проверив зафиксированное свойство.</span><span class="sxs-lookup"><span data-stu-id="442fe-278">In this case it is easy to observe if code commits a unit of work by checking the Commited property.</span></span>

<span data-ttu-id="442fe-279">Для хранения объектов Employee и табели в памяти также потребуется фиктивный Иобжектсет @ no__t-0T @ no__t-1.</span><span class="sxs-lookup"><span data-stu-id="442fe-279">We’ll also need a fake IObjectSet&lt;T&gt; to hold Employee and TimeCard objects in memory.</span></span> <span data-ttu-id="442fe-280">Мы можем предоставить единую реализацию, используя универсальные шаблоны.</span><span class="sxs-lookup"><span data-stu-id="442fe-280">We can provide a single implementation using generics.</span></span>

``` csharp
    public class InMemoryObjectSet<T> : IObjectSet<T> where T : class
        public InMemoryObjectSet()
            : this(Enumerable.Empty<T>()) {
        }
        public InMemoryObjectSet(IEnumerable<T> entities) {
            _set = new HashSet<T>();
            foreach (var entity in entities) {
                _set.Add(entity);
            }
            _queryableSet = _set.AsQueryable();
        }
        public void AddObject(T entity) {
            _set.Add(entity);
        }
        public void Attach(T entity) {
            _set.Add(entity);
        }
        public void DeleteObject(T entity) {
            _set.Remove(entity);
        }
        public void Detach(T entity) {
            _set.Remove(entity);
        }
        public Type ElementType {
            get { return _queryableSet.ElementType; }
        }
        public Expression Expression {
            get { return _queryableSet.Expression; }
        }
        public IQueryProvider Provider {
            get { return _queryableSet.Provider; }
        }
        public IEnumerator<T> GetEnumerator() {
            return _set.GetEnumerator();
        }
        IEnumerator IEnumerable.GetEnumerator() {
            return GetEnumerator();
        }

        readonly HashSet<T> _set;
        readonly IQueryable<T> _queryableSet;
    }
```

<span data-ttu-id="442fe-281">Этот тест дважды делегирует большую часть своей работы базовому объекту hash @ no__t-0T @ no__t-1.</span><span class="sxs-lookup"><span data-stu-id="442fe-281">This test double delegates most of its work to an underlying HashSet&lt;T&gt; object.</span></span> <span data-ttu-id="442fe-282">Обратите внимание, что Иобжектсет @ no__t-0T @ no__t-1 требует универсального ограничения, обеспечивающего применение T в качестве класса (ссылочный тип), а также заставляет нас реализовать IQueryable @ no__t-2T @ no__t-3.</span><span class="sxs-lookup"><span data-stu-id="442fe-282">Note that IObjectSet&lt;T&gt; requires a generic constraint enforcing T as a class (a reference type), and also forces us to implement IQueryable&lt;T&gt;.</span></span> <span data-ttu-id="442fe-283">Можно легко создать в памяти коллекцию IQueryable @ no__t-0T @ no__t-1 с помощью стандартного оператора LINQ Аскуерябле.</span><span class="sxs-lookup"><span data-stu-id="442fe-283">It is easy to make an in-memory collection appear as an IQueryable&lt;T&gt; using the standard LINQ operator AsQueryable.</span></span>

### <a name="the-tests"></a><span data-ttu-id="442fe-284">Тесты</span><span class="sxs-lookup"><span data-stu-id="442fe-284">The Tests</span></span>

<span data-ttu-id="442fe-285">Традиционные модульные тесты будут использовать один тестовый класс для хранения всех тестов для всех действий в одном контроллере MVC.</span><span class="sxs-lookup"><span data-stu-id="442fe-285">Traditional unit tests will use a single test class to hold all of the tests for all of the actions in a single MVC controller.</span></span> <span data-ttu-id="442fe-286">Можно написать эти тесты или любой тип модульного теста с помощью поддельных подразделений памяти, которые мы создали.</span><span class="sxs-lookup"><span data-stu-id="442fe-286">We can write these tests, or any type of unit test, using the in memory fakes we’ve built.</span></span> <span data-ttu-id="442fe-287">Тем не менее, в этой статье мы будем избегать подхода монолитного тестового класса, а вместо этого сгруппировать наши тесты, чтобы сосредоточиться на определенной части функциональности.</span><span class="sxs-lookup"><span data-stu-id="442fe-287">However, for this article we will avoid the monolithic test class approach and instead group our tests to focus on a specific piece of functionality.</span></span><span data-ttu-id="442fe-288">  Например, «создание нового сотрудника» — это функция, которую мы хотим протестировать, поэтому мы будем использовать один тестовый класс для проверки действия одного контроллера, ответственного за создание нового сотрудника.</span><span class="sxs-lookup"><span data-stu-id="442fe-288">  For example, “create new employee” might be the functionality we want to test, so we will use a single test class to verify the single controller action responsible for creating a new employee.</span></span>

<span data-ttu-id="442fe-289">Для всех этих детализированных тестовых классов необходимо выполнить некоторые распространенные коды настройки.</span><span class="sxs-lookup"><span data-stu-id="442fe-289">There is some common setup code we need for all these fine grained test classes.</span></span> <span data-ttu-id="442fe-290">Например, нам всегда нужно создавать репозитории в памяти и поддельную единицу работы.</span><span class="sxs-lookup"><span data-stu-id="442fe-290">For example, we always need to create our in-memory repositories and fake unit of work.</span></span> <span data-ttu-id="442fe-291">Кроме того, необходим экземпляр контроллера сотрудника с поддельной единицей работы.</span><span class="sxs-lookup"><span data-stu-id="442fe-291">We also need an instance of the employee controller with the fake unit of work injected.</span></span> <span data-ttu-id="442fe-292">Мы будем использовать этот распространенный код настройки в тестовых классах с помощью базового класса.</span><span class="sxs-lookup"><span data-stu-id="442fe-292">We’ll share this common setup code across test classes by using a base class.</span></span>

``` csharp
    public class EmployeeControllerTestBase {
        public EmployeeControllerTestBase() {
            _employeeData = EmployeeObjectMother.CreateEmployees()
                                                .ToList();
            _repository = new InMemoryObjectSet<Employee>(_employeeData);
            _unitOfWork = new InMemoryUnitOfWork();
            _unitOfWork.Employees = _repository;
            _controller = new EmployeeController(_unitOfWork);
        }

        protected IList<Employee> _employeeData;
        protected EmployeeController _controller;
        protected InMemoryObjectSet<Employee> _repository;
        protected InMemoryUnitOfWork _unitOfWork;
    }
```

<span data-ttu-id="442fe-293">Объект «мама», используемый в базовом классе, является одним из распространенных шаблонов для создания тестовых данных.</span><span class="sxs-lookup"><span data-stu-id="442fe-293">The “object mother” we use in the base class is one common pattern for creating test data.</span></span> <span data-ttu-id="442fe-294">Объект матери содержит заводские методы для создания экземпляров тестовых сущностей для использования в нескольких тестовых средствах тестирования.</span><span class="sxs-lookup"><span data-stu-id="442fe-294">An object mother contains factory methods to instantiate test entities for use across multiple test fixtures.</span></span>

``` csharp
    public static class EmployeeObjectMother {
        public static IEnumerable<Employee> CreateEmployees() {
            yield return new Employee() {
                Id = 1, Name = "Scott", HireDate=new DateTime(2002, 1, 1)
            };
            yield return new Employee() {
                Id = 2, Name = "Poonam", HireDate=new DateTime(2001, 1, 1)
            };
            yield return new Employee() {
                Id = 3, Name = "Simon", HireDate=new DateTime(2008, 1, 1)
            };
        }
        // ... more fake data for different scenarios
    }
```

<span data-ttu-id="442fe-295">Мы можем использовать Емплойиконтроллертестбасе в качестве базового класса для ряда тестовой основы (см. рис. 3).</span><span class="sxs-lookup"><span data-stu-id="442fe-295">We can use the EmployeeControllerTestBase as the base class for a number of test fixtures (see figure 3).</span></span> <span data-ttu-id="442fe-296">Каждый тестовый набор будет проверять конкретное действие контроллера.</span><span class="sxs-lookup"><span data-stu-id="442fe-296">Each test fixture will test a specific controller action.</span></span> <span data-ttu-id="442fe-297">Например, одно средство тестирования будет сосредоточиться на тестировании действия Create, используемого во время HTTP-запроса GET (для отображения представления для создания сотрудника), и в другом наборе основное внимание уделяется действию create, используемому в запросе HTTP POST (чтобы получить сведения, отправленные пользователь, чтобы создать сотрудника.</span><span class="sxs-lookup"><span data-stu-id="442fe-297">For example, one test fixture will focus on testing the Create action used during an HTTP GET request (to display the view for creating an employee), and a different fixture will focus on the Create action used in an HTTP POST request (to take information submitted by the user to create an employee).</span></span> <span data-ttu-id="442fe-298">Каждый производный класс несет ответственность за установку, необходимую в конкретном контексте, и на предоставление утверждений, необходимых для проверки результатов для конкретного контекста теста.</span><span class="sxs-lookup"><span data-stu-id="442fe-298">Each derived class is only responsible for the setup needed in its specific context, and to provide the assertions needed to verify the outcomes for its specific test context.</span></span>

![EF test_03](~/ef6/media/eftest-03.png)

<span data-ttu-id="442fe-300">**Рис. 3**</span><span class="sxs-lookup"><span data-stu-id="442fe-300">**Figure 3**</span></span>

<span data-ttu-id="442fe-301">Соглашение об именовании и стиль теста, представленные здесь, не требуются для тестируемого кода — это лишь один из подходов.</span><span class="sxs-lookup"><span data-stu-id="442fe-301">The naming convention and test style presented here isn’t required for testable code – it’s just one approach.</span></span> <span data-ttu-id="442fe-302">На рис. 4 показаны тесты, выполняемые в подключаемом модуле средства выполнения тестов нервной для Visual Studio 2010.</span><span class="sxs-lookup"><span data-stu-id="442fe-302">Figure 4 shows the tests running in the Jet Brains Resharper test runner plugin for Visual Studio 2010.</span></span>

![EF test_04](~/ef6/media/eftest-04.png)

<span data-ttu-id="442fe-304">**Рис. 4**</span><span class="sxs-lookup"><span data-stu-id="442fe-304">**Figure 4**</span></span>

<span data-ttu-id="442fe-305">При использовании базового класса для работы с общим кодом настройки модульные тесты для каждого действия контроллера являются небольшими и легко написанными.</span><span class="sxs-lookup"><span data-stu-id="442fe-305">With a base class to handle the shared setup code, the unit tests for each controller action are small and easy to write.</span></span> <span data-ttu-id="442fe-306">Тесты будут выполняться быстро (так как мы выполняем операции в памяти) и не должны завершаться сбоем из-за несвязанных инфраструктур или проблем с окружающей средой (так как мы разработали тестируемую единицу).</span><span class="sxs-lookup"><span data-stu-id="442fe-306">The tests will execute quickly (since we are performing in-memory operations), and shouldn’t fail because of unrelated infrastructure or environmental concerns (because we’ve isolated the unit under test).</span></span>

``` csharp
    [TestClass]
    public class EmployeeControllerCreateActionPostTests
               : EmployeeControllerTestBase {
        [TestMethod]
        public void ShouldAddNewEmployeeToRepository() {
            _controller.Create(_newEmployee);
            Assert.IsTrue(_repository.Contains(_newEmployee));
        }
        [TestMethod]
        public void ShouldCommitUnitOfWork() {
            _controller.Create(_newEmployee);
            Assert.IsTrue(_unitOfWork.Committed);
        }
        // ... more tests

        Employee _newEmployee = new Employee() {
            Name = "NEW EMPLOYEE",
            HireDate = new System.DateTime(2010, 1, 1)
        };
    }
```

<span data-ttu-id="442fe-307">В этих тестах базовый класс выполняет большую часть работы по установке.</span><span class="sxs-lookup"><span data-stu-id="442fe-307">In these tests, the base class does most of the setup work.</span></span> <span data-ttu-id="442fe-308">Помните, что конструктор базового класса создает репозиторий в памяти, поддельную единицу работы и экземпляр класса Емплойиконтроллер.</span><span class="sxs-lookup"><span data-stu-id="442fe-308">Remember the base class constructor creates the in-memory repository, a fake unit of work, and an instance of the EmployeeController class.</span></span> <span data-ttu-id="442fe-309">Тестовый класс является производным от этого базового класса и посвящен конкретным методам тестирования метода Create.</span><span class="sxs-lookup"><span data-stu-id="442fe-309">The test class derives from this base class and focuses on the specifics of testing the Create method.</span></span> <span data-ttu-id="442fe-310">В этом случае особенности сводится к этапам "организовать", "действовать" и "утверждение", которые вы увидите в любой процедуре модульного тестирования:</span><span class="sxs-lookup"><span data-stu-id="442fe-310">In this case the specifics boil down to the “arrange, act, and assert” steps you’ll see in any unit testing procedure:</span></span>

-   <span data-ttu-id="442fe-311">Создайте объект newEmployee для имитации входящих данных.</span><span class="sxs-lookup"><span data-stu-id="442fe-311">Create a newEmployee object to simulate incoming data.</span></span>
-   <span data-ttu-id="442fe-312">Вызовите действие Create объекта Емплойиконтроллер и передайте newEmployee.</span><span class="sxs-lookup"><span data-stu-id="442fe-312">Invoke the Create action of the EmployeeController and pass in the newEmployee.</span></span>
-   <span data-ttu-id="442fe-313">Убедитесь, что действие Create выдает ожидаемые результаты (сотрудник отображается в репозитории).</span><span class="sxs-lookup"><span data-stu-id="442fe-313">Verify the Create action produces the expected results (the employee appears in the repository).</span></span>

<span data-ttu-id="442fe-314">Созданная нами возможность протестировать любые действия Емплойиконтроллер.</span><span class="sxs-lookup"><span data-stu-id="442fe-314">What we’ve built allows us to test any of the EmployeeController actions.</span></span> <span data-ttu-id="442fe-315">Например, при написании тестов для действия индекса контроллера сотрудника можно наследовать от базового класса Test, чтобы установить ту же основную настройку для наших тестов.</span><span class="sxs-lookup"><span data-stu-id="442fe-315">For example, when we write tests for the Index action of the Employee controller we can inherit from the test base class to establish the same base setup for our tests.</span></span> <span data-ttu-id="442fe-316">В этом случае базовый класс создаст репозиторий в памяти, поддельную единицу работы и экземпляр Емплойиконтроллер.</span><span class="sxs-lookup"><span data-stu-id="442fe-316">Again the base class will create the in-memory repository, the fake unit of work, and an instance of the EmployeeController.</span></span> <span data-ttu-id="442fe-317">Тесты для действия с индексом должны сосредоточиться только на вызове действия индекса и проверке качеств модели, возвращаемой действием.</span><span class="sxs-lookup"><span data-stu-id="442fe-317">The tests for the Index action only need to focus on invoking the Index action and testing the qualities of the model the action returns.</span></span>

``` csharp
    [TestClass]
    public class EmployeeControllerIndexActionTests
               : EmployeeControllerTestBase {
        [TestMethod]
        public void ShouldBuildModelWithAllEmployees() {
            var result = _controller.Index();
            var model = result.ViewData.Model
                          as IEnumerable<Employee>;
            Assert.IsTrue(model.Count() == _employeeData.Count);
        }
        [TestMethod]
        public void ShouldOrderModelByHiredateAscending() {
            var result = _controller.Index();
            var model = result.ViewData.Model
                         as IEnumerable<Employee>;
            Assert.IsTrue(model.SequenceEqual(
                           _employeeData.OrderBy(e => e.HireDate)));
        }
        // ...
    }
```

<span data-ttu-id="442fe-318">Тесты, создаваемые с помощью имитаций в памяти, ориентированы на тестирование *состояния* программного обеспечения.</span><span class="sxs-lookup"><span data-stu-id="442fe-318">The tests we are creating with in-memory fakes are oriented towards testing the *state* of the software.</span></span> <span data-ttu-id="442fe-319">Например, при тестировании действия Create, которое мы хотим проверить состояние репозитория после выполнения действия Create, — сохраняет ли репозиторий новый сотрудник?</span><span class="sxs-lookup"><span data-stu-id="442fe-319">For example, when testing the Create action we want to inspect the state of the repository after the create action executes – does the repository hold the new employee?</span></span>

``` csharp
    [TestMethod]
    public void ShouldAddNewEmployeeToRepository() {
        _controller.Create(_newEmployee);
        Assert.IsTrue(_repository.Contains(_newEmployee));
    }
```

<span data-ttu-id="442fe-320">Далее мы рассмотрим тестирование на основе взаимодействия.</span><span class="sxs-lookup"><span data-stu-id="442fe-320">Later we’ll look at interaction based testing.</span></span> <span data-ttu-id="442fe-321">Тестирование на основе взаимодействия попросит вас, если тестируемый код вызвал правильные методы наших объектов и передал правильные параметры.</span><span class="sxs-lookup"><span data-stu-id="442fe-321">Interaction based testing will ask if the code under test invoked the proper methods on our objects and passed the correct parameters.</span></span> <span data-ttu-id="442fe-322">Сейчас мы будем переноситься на обложку другого шаблона разработки — отложенная загрузка.</span><span class="sxs-lookup"><span data-stu-id="442fe-322">For now we’ll move on the cover another design pattern – the lazy load.</span></span>

## <a name="eager-loading-and-lazy-loading"></a><span data-ttu-id="442fe-323">Упреждающая загрузка и отложенная загрузка</span><span class="sxs-lookup"><span data-stu-id="442fe-323">Eager Loading and Lazy Loading</span></span>

<span data-ttu-id="442fe-324">В некоторый момент в веб-приложении ASP.NET MVC может потребоваться отобразить сведения о сотруднике и включить связанные с ним карты времени.</span><span class="sxs-lookup"><span data-stu-id="442fe-324">At some point in the ASP.NET  MVC web application we might wish to display an employee’s information and include the employee’s associated time cards.</span></span> <span data-ttu-id="442fe-325">Например, у нас может быть отображение сводки по времени, в котором отображается имя сотрудника и общее количество карт времени в системе.</span><span class="sxs-lookup"><span data-stu-id="442fe-325">For example, we might have a time card summary display that shows the employee’s name and the total number of time cards in the system.</span></span> <span data-ttu-id="442fe-326">Для реализации этой функции можно применить несколько подходов.</span><span class="sxs-lookup"><span data-stu-id="442fe-326">There are several approaches we can take to implement this feature.</span></span>

### <a name="projection"></a><span data-ttu-id="442fe-327">Проекция</span><span class="sxs-lookup"><span data-stu-id="442fe-327">Projection</span></span>

<span data-ttu-id="442fe-328">Одним из простых подходов к созданию сводки является создание модели, выделенной для информации, которую нужно отобразить в представлении.</span><span class="sxs-lookup"><span data-stu-id="442fe-328">One easy approach to create the summary is to construct a model dedicated to the information we want to display in the view.</span></span> <span data-ttu-id="442fe-329">В этом сценарии модель может выглядеть следующим образом.</span><span class="sxs-lookup"><span data-stu-id="442fe-329">In this scenario the model might look like the following.</span></span>

``` csharp
    public class EmployeeSummaryViewModel {
        public string Name { get; set; }
        public int TotalTimeCards { get; set; }
    }
```

<span data-ttu-id="442fe-330">Обратите внимание, что Емплойисуммаривиевмодел не является сущностью, иными словами, это не то, что мы хотим сохранить в базе данных.</span><span class="sxs-lookup"><span data-stu-id="442fe-330">Note that the EmployeeSummaryViewModel is not an entity – in other words it is not something we want to persist in the database.</span></span> <span data-ttu-id="442fe-331">Мы будем использовать этот класс для случайного перебора данных в представление строго типизированным способом.</span><span class="sxs-lookup"><span data-stu-id="442fe-331">We are only going to use this class to shuffle data into the view in a strongly typed manner.</span></span> <span data-ttu-id="442fe-332">Модель представления похожа на объект передачи данных (DTO), так как он не содержит методов поведения (нет) — только свойств.</span><span class="sxs-lookup"><span data-stu-id="442fe-332">The view model is like a data transfer object (DTO) because it contains no behavior (no methods) – only properties.</span></span> <span data-ttu-id="442fe-333">Эти свойства будут содержать данные, которые необходимо переместить.</span><span class="sxs-lookup"><span data-stu-id="442fe-333">The properties will hold the data we need to move.</span></span> <span data-ttu-id="442fe-334">Создать эту модель представления легко с помощью стандартного оператора проекции LINQ — оператора SELECT.</span><span class="sxs-lookup"><span data-stu-id="442fe-334">It is easy to instantiate this view model using LINQ’s standard projection operator – the Select operator.</span></span>

``` csharp
    public ViewResult Summary(int id) {
        var model = _unitOfWork.Employees
                               .Where(e => e.Id == id)
                               .Select(e => new EmployeeSummaryViewModel
                                  {
                                    Name = e.Name,
                                    TotalTimeCards = e.TimeCards.Count()
                                  })
                               .Single();
        return View(model);
    }
```

<span data-ttu-id="442fe-335">Приведенный выше код имеет две важные функции.</span><span class="sxs-lookup"><span data-stu-id="442fe-335">There are two notable features to the above code.</span></span> <span data-ttu-id="442fe-336">Во-первых, код легко тестировать, так как его легко наблюдать и изолировать.</span><span class="sxs-lookup"><span data-stu-id="442fe-336">First – the code is easy to test because it is still easy to observe and isolate.</span></span> <span data-ttu-id="442fe-337">Оператор SELECT работает так же, как и для поддельных в памяти подразделений, как и для реальной единицы работы.</span><span class="sxs-lookup"><span data-stu-id="442fe-337">The Select operator works just as well against our in-memory fakes as it does against the real unit of work.</span></span>

``` csharp
    [TestClass]
    public class EmployeeControllerSummaryActionTests
               : EmployeeControllerTestBase {
        [TestMethod]
        public void ShouldBuildModelWithCorrectEmployeeSummary() {
            var id = 1;
            var result = _controller.Summary(id);
            var model = result.ViewData.Model as EmployeeSummaryViewModel;
            Assert.IsTrue(model.TotalTimeCards == 3);
        }
        // ...
    }
```

<span data-ttu-id="442fe-338">Вторая существенная особенность заключается в том, как код позволяет EF4 создавать единый, эффективный запрос для совместного сбора информации о сотрудниках и карточках времени.</span><span class="sxs-lookup"><span data-stu-id="442fe-338">The second notable feature is how the code allows EF4 to generate a single, efficient query to assemble employee and time card information together.</span></span> <span data-ttu-id="442fe-339">Мы загрузили сведения о сотрудниках и сведения о карте времени в один объект без использования каких-либо специальных API.</span><span class="sxs-lookup"><span data-stu-id="442fe-339">We’ve loaded employee information and time card information into the same object without using any special APIs.</span></span> <span data-ttu-id="442fe-340">Код просто выражает информацию, которую он требует, с помощью стандартных операторов LINQ, работающих с источниками данных в памяти, а также с удаленными источниками данных.</span><span class="sxs-lookup"><span data-stu-id="442fe-340">The code merely expressed the information it requires using standard LINQ operators that work against in-memory data sources as well as remote data sources.</span></span> <span data-ttu-id="442fe-341">EF4 удалось преобразовать деревья выражений, созданные с помощью запроса LINQ и компилятора C @ no__t-0, в единый и эффективный запрос T-SQL.</span><span class="sxs-lookup"><span data-stu-id="442fe-341">EF4 was able to translate the expression trees generated by the LINQ query and C\# compiler into a single and efficient T-SQL query.</span></span>

``` SQL
    SELECT
    [Limit1].[Id] AS [Id],
    [Limit1].[Name] AS [Name],
    [Limit1].[C1] AS [C1]
    FROM (SELECT TOP (2)
      [Project1].[Id] AS [Id],
      [Project1].[Name] AS [Name],
      [Project1].[C1] AS [C1]
      FROM (SELECT
        [Extent1].[Id] AS [Id],
        [Extent1].[Name] AS [Name],
        (SELECT COUNT(1) AS [A1]
         FROM [dbo].[TimeCards] AS [Extent2]
         WHERE [Extent1].[Id] =
               [Extent2].[EmployeeTimeCard_TimeCard_Id]) AS [C1]
              FROM [dbo].[Employees] AS [Extent1]
               WHERE [Extent1].[Id] = @p__linq__0
         )  AS [Project1]
    )  AS [Limit1]
```

<span data-ttu-id="442fe-342">Существуют и другие случаи, когда нам не нужно работать с моделью представления или объектом DTO, но с реальными сущностями.</span><span class="sxs-lookup"><span data-stu-id="442fe-342">There are other times when we don’t want to work with a view model or DTO object, but with real entities.</span></span> <span data-ttu-id="442fe-343">Когда нам известно, что нам нужен сотрудник *и* карты времени сотрудника, мы можем заранее загрузить связанные данные с помощью ненавязчивого и эффективного способа.</span><span class="sxs-lookup"><span data-stu-id="442fe-343">When we know we need an employee *and* the employee’s time cards, we can eagerly load the related data in an unobtrusive and efficient manner.</span></span>

### <a name="explicit-eager-loading"></a><span data-ttu-id="442fe-344">Явная упреждающая загрузка</span><span class="sxs-lookup"><span data-stu-id="442fe-344">Explicit Eager Loading</span></span>

<span data-ttu-id="442fe-345">Когда мы хотим заранее загрузить связанные сведения о сущности, требуется некоторый механизм для бизнес-логики (или в этом сценарии — логика действия контроллера), чтобы выразить свое желание в репозитории.</span><span class="sxs-lookup"><span data-stu-id="442fe-345">When we want to eagerly load related entity information we need some mechanism for business logic (or in this scenario, controller action logic) to express its desire to the repository.</span></span> <span data-ttu-id="442fe-346">Класс EF4 ObjectQuery @ no__t-0T @ no__t-1 определяет метод Include, указывающий связанные объекты для извлечения во время выполнения запроса.</span><span class="sxs-lookup"><span data-stu-id="442fe-346">The EF4 ObjectQuery&lt;T&gt; class defines an Include method to specify the related objects to retrieve during a query.</span></span> <span data-ttu-id="442fe-347">Помните, что EF4 ObjectContext предоставляет сущности через конкретный класс Objecting @ no__t-0T @ no__t-1, который наследует от ObjectQuery @ no__t-2T @ no__t-3.</span><span class="sxs-lookup"><span data-stu-id="442fe-347">Remember the EF4 ObjectContext exposes entities via the concrete ObjectSet&lt;T&gt; class which inherits from ObjectQuery&lt;T&gt;.</span></span><span data-ttu-id="442fe-348">  Если мы использовали набор объектов @ no__t-0T @ no__t-1 в действии контроллера, мы можем написать следующий код, чтобы указать безотлагательную загрузку сведений о карте времени для каждого сотрудника.</span><span class="sxs-lookup"><span data-stu-id="442fe-348">  If we were using ObjectSet&lt;T&gt; references in our controller action we could write the following code to specify an eager load of time card information for each employee.</span></span>

``` csharp
    _employees.Include("TimeCards")
              .Where(e => e.HireDate.Year > 2009);
```

<span data-ttu-id="442fe-349">Однако, поскольку мы пытаемся сделать наш код тестируемым, мы не предоставляем объект набора объектов @ no__t-0T @ no__t-1 извне класса реальных единиц работы.</span><span class="sxs-lookup"><span data-stu-id="442fe-349">However, since we are trying to keep our code testable we are not exposing ObjectSet&lt;T&gt; from outside the real unit of work class.</span></span> <span data-ttu-id="442fe-350">Вместо этого мы полагаетесь на интерфейс Иобжектсет @ no__t-0T @ no__t-1, который проще имитировать, но Иобжектсет @ no__t-2T @ no__t-3 не определяет метод Include.</span><span class="sxs-lookup"><span data-stu-id="442fe-350">Instead, we rely on the IObjectSet&lt;T&gt; interface which is easier to fake, but IObjectSet&lt;T&gt; does not define an Include method.</span></span> <span data-ttu-id="442fe-351">Прелесть LINQ заключается в том, что мы можем создать собственный оператор include.</span><span class="sxs-lookup"><span data-stu-id="442fe-351">The beauty of LINQ is that we can create our own Include operator.</span></span>

``` csharp
    public static class QueryableExtensions {
        public static IQueryable<T> Include<T>
                (this IQueryable<T> sequence, string path) {
            var objectQuery = sequence as ObjectQuery<T>;
            if(objectQuery != null)
            {
                return objectQuery.Include(path);
            }
            return sequence;
        }
    }
```

<span data-ttu-id="442fe-352">Обратите внимание, что этот оператор include определен как метод расширения для IQueryable @ no__t-0T @ no__t-1 вместо Иобжектсет @ no__t-2T @ no__t-3.</span><span class="sxs-lookup"><span data-stu-id="442fe-352">Notice this Include operator is defined as an extension method for IQueryable&lt;T&gt; instead of IObjectSet&lt;T&gt;.</span></span> <span data-ttu-id="442fe-353">Это дает нам возможность использовать метод с более широким диапазоном возможных типов, включая IQueryable @ no__t-0T @ no__t-1, Иобжектсет @ no__t-2T @ no__t-3, ObjectQuery @ no__t-4T поддерживается @ no__t-5, и Object @ no__t-6T @ no__t-7.</span><span class="sxs-lookup"><span data-stu-id="442fe-353">This gives us the ability to use the method with a wider range of possible types, including IQueryable&lt;T&gt;, IObjectSet&lt;T&gt;, ObjectQuery&lt;T&gt;, and ObjectSet&lt;T&gt;.</span></span> <span data-ttu-id="442fe-354">В случае, если базовая последовательность не является подлинной EF4 ObjectQuery @ no__t-0T @ no__t-1, то ничего не происходит, а оператор include — это No-Op.</span><span class="sxs-lookup"><span data-stu-id="442fe-354">In the event the underlying sequence is not a genuine EF4 ObjectQuery&lt;T&gt;, then there is no harm done and the Include operator is a no-op.</span></span> <span data-ttu-id="442fe-355">Если базовая последовательность является ObjectQuery @ no__t-1T @ no__t-2 (или *является* производной от ObjectQuery @ No__t-3T @ no__t-4), то EF4 будет видеть наше требование к дополнительным данным и СФОРМУЛИРОВАТЬ правильный SQL-запрос.</span><span class="sxs-lookup"><span data-stu-id="442fe-355">If the underlying sequence *is* an ObjectQuery&lt;T&gt; (or derived from ObjectQuery&lt;T&gt;), then EF4 will see our requirement for additional data and formulate the proper SQL query.</span></span>

<span data-ttu-id="442fe-356">С помощью этого нового оператора можно явным образом запросить неподготовленную загрузку сведений о карте времени из репозитория.</span><span class="sxs-lookup"><span data-stu-id="442fe-356">With this new operator in place we can explicitly request an eager load of time card information from the repository.</span></span>

``` csharp
    public ViewResult Index() {
        var model = _unitOfWork.Employees
                               .Include("TimeCards")
                               .OrderBy(e => e.HireDate);
        return View(model);
    }
```

<span data-ttu-id="442fe-357">При выполнении с реальным контекстом ObjectContext код создает следующий отдельный запрос.</span><span class="sxs-lookup"><span data-stu-id="442fe-357">When run against a real ObjectContext, the code produces the following single query.</span></span> <span data-ttu-id="442fe-358">Запрос собирает достаточно информации из базы данных за один проход для материализации объектов Employee и полностью заполняет их свойство Тимекардс.</span><span class="sxs-lookup"><span data-stu-id="442fe-358">The query gathers enough information from the database in one trip to materialize the employee objects and fully populate their TimeCards property.</span></span>

``` SQL
    SELECT
    [Project1].[Id] AS [Id],
    [Project1].[Name] AS [Name],
    [Project1].[HireDate] AS [HireDate],
    [Project1].[C1] AS [C1],
    [Project1].[Id1] AS [Id1],
    [Project1].[Hours] AS [Hours],
    [Project1].[EffectiveDate] AS [EffectiveDate],
    [Project1].[EmployeeTimeCard_TimeCard_Id] AS [EmployeeTimeCard_TimeCard_Id]
    FROM ( SELECT
         [Extent1].[Id] AS [Id],
         [Extent1].[Name] AS [Name],
         [Extent1].[HireDate] AS [HireDate],
         [Extent2].[Id] AS [Id1],
         [Extent2].[Hours] AS [Hours],
         [Extent2].[EffectiveDate] AS [EffectiveDate],
         [Extent2].[EmployeeTimeCard_TimeCard_Id] AS
                    [EmployeeTimeCard_TimeCard_Id],
         CASE WHEN ([Extent2].[Id] IS NULL) THEN CAST(NULL AS int)
         ELSE 1 END AS [C1]
         FROM  [dbo].[Employees] AS [Extent1]
         LEFT OUTER JOIN [dbo].[TimeCards] AS [Extent2] ON [Extent1].[Id] = [Extent2].[EmployeeTimeCard_TimeCard_Id]
    )  AS [Project1]
    ORDER BY [Project1].[HireDate] ASC,
             [Project1].[Id] ASC, [Project1].[C1] ASC
```

<span data-ttu-id="442fe-359">Отличная новость — код внутри метода действия остается полностью тестируемым.</span><span class="sxs-lookup"><span data-stu-id="442fe-359">The great news is the code inside the action method remains fully testable.</span></span> <span data-ttu-id="442fe-360">Для поддержки оператора include не нужно предоставлять дополнительные функции для наших имитаций.</span><span class="sxs-lookup"><span data-stu-id="442fe-360">We don’t need to provide any additional features for our fakes to support the Include operator.</span></span> <span data-ttu-id="442fe-361">Неплохая новость заключается в том, чтобы использовать оператор include внутри кода, который нам нужен для сохранения игнорирующих.</span><span class="sxs-lookup"><span data-stu-id="442fe-361">The bad news is we had to use the Include operator inside of the code we wanted to keep persistence ignorant.</span></span> <span data-ttu-id="442fe-362">Это простой пример типа компромиссов, которые необходимо оценить при создании тестируемого кода.</span><span class="sxs-lookup"><span data-stu-id="442fe-362">This is a prime example of the type of tradeoffs you’ll need to evaluate when building testable code.</span></span> <span data-ttu-id="442fe-363">Бывают случаи, когда необходимо разрешить устойчивую утечку данных вне абстракции репозитория, чтобы удовлетворить цели производительности.</span><span class="sxs-lookup"><span data-stu-id="442fe-363">There are times when you need to let persistence concerns leak outside the repository abstraction to meet performance goals.</span></span>

<span data-ttu-id="442fe-364">Альтернативой безотлагательной загрузке является отложенная загрузка.</span><span class="sxs-lookup"><span data-stu-id="442fe-364">The alternative to eager loading is lazy loading.</span></span> <span data-ttu-id="442fe-365">Отложенная загрузка означает, что наш бизнес-код *не* должен явно объявлять требование для связанных данных.</span><span class="sxs-lookup"><span data-stu-id="442fe-365">Lazy loading means we do *not* need our business code to explicitly announce the requirement for associated data.</span></span> <span data-ttu-id="442fe-366">Вместо этого мы используем наши сущности в приложении и, если требуются дополнительные данные, Entity Framework загрузит данные по запросу.</span><span class="sxs-lookup"><span data-stu-id="442fe-366">Instead, we use our entities in the application and if additional data is needed Entity Framework will load the data on demand.</span></span>

### <a name="lazy-loading"></a><span data-ttu-id="442fe-367">Отложенная загрузка</span><span class="sxs-lookup"><span data-stu-id="442fe-367">Lazy Loading</span></span>

<span data-ttu-id="442fe-368">Вы можете легко представить себе ситуацию, когда нам неизвестно, какие данные потребуются для части бизнес-логики.</span><span class="sxs-lookup"><span data-stu-id="442fe-368">It’s easy to imagine a scenario where we don’t know what data a piece of business logic will need.</span></span> <span data-ttu-id="442fe-369">Нам может быть известно, что логике требуется объект Employee, но мы можем выполнить ветвление по разным путям выполнения, в которых некоторые из этих путей требуют от сотрудника сведения о карте времени, а некоторые — нет.</span><span class="sxs-lookup"><span data-stu-id="442fe-369">We might know the logic needs an employee object, but we may branch into different execution paths where some of those paths require time card information from the employee, and some do not.</span></span> <span data-ttu-id="442fe-370">Такие сценарии идеально подходят для неявной отложенной загрузки, так как данные прекрасно отображаются по мере необходимости.</span><span class="sxs-lookup"><span data-stu-id="442fe-370">Scenarios like this are perfect for implicit lazy loading because data magically appears on an as-needed basis.</span></span>

<span data-ttu-id="442fe-371">Отложенная загрузка, также известная как отложенная загрузка, замещает некоторые требования к объектам сущности.</span><span class="sxs-lookup"><span data-stu-id="442fe-371">Lazy loading, also known as deferred loading, does place some requirements on our entity objects.</span></span> <span data-ttu-id="442fe-372">POCO с истинным игнорированияом сохраняемости не повлияет ни на какие требования с уровня сохраняемости, но истинность игнорирования сохранения практически невозможна.</span><span class="sxs-lookup"><span data-stu-id="442fe-372">POCOs with true persistence ignorance would not face any requirements from the persistence layer, but true persistence ignorance is practically impossible to achieve.</span></span><span data-ttu-id="442fe-373">  Вместо этого мы измеряем игнорирования сохраняемости в относительном градусах.</span><span class="sxs-lookup"><span data-stu-id="442fe-373">  Instead we measure persistence ignorance in relative degrees.</span></span> <span data-ttu-id="442fe-374">Это было бы неудачно, если бы нам пришлось наследовать от базового класса, ориентированного на сохраняемость, или использовать специализированную коллекцию для реализации отложенной загрузки в POCO.</span><span class="sxs-lookup"><span data-stu-id="442fe-374">It would be unfortunate if we needed to inherit from a persistence oriented base class or use a specialized collection to achieve lazy loading in POCOs.</span></span> <span data-ttu-id="442fe-375">К счастью, EF4 имеет менее агрессивное решение.</span><span class="sxs-lookup"><span data-stu-id="442fe-375">Fortunately, EF4 has a less intrusive solution.</span></span>

### <a name="virtually-undetectable"></a><span data-ttu-id="442fe-376">Практически необнаруживаемые</span><span class="sxs-lookup"><span data-stu-id="442fe-376">Virtually Undetectable</span></span>

<span data-ttu-id="442fe-377">При использовании объектов POCO EF4 может динамически создавать прокси-серверы среды выполнения для сущностей.</span><span class="sxs-lookup"><span data-stu-id="442fe-377">When using POCO objects, EF4 can dynamically generate runtime proxies for entities.</span></span> <span data-ttu-id="442fe-378">Эти прокси незаметно для упаковки материализованных POCO и предоставляют дополнительные службы путем перехвата каждой операции получения и установки свойства для выполнения дополнительной работы.</span><span class="sxs-lookup"><span data-stu-id="442fe-378">These proxies invisibly wrap the materialized POCOs and provide additional services by intercepting each property get and set operation to perform additional work.</span></span> <span data-ttu-id="442fe-379">Одна из таких услуг — это функция отложенной загрузки, которую мы ищем.</span><span class="sxs-lookup"><span data-stu-id="442fe-379">One such service is the lazy loading feature we are looking for.</span></span> <span data-ttu-id="442fe-380">Другая служба является эффективным механизмом отслеживания изменений, который может записывать, когда программа изменяет значения свойств сущности.</span><span class="sxs-lookup"><span data-stu-id="442fe-380">Another service is an efficient change tracking mechanism which can record when the program changes the property values of an entity.</span></span> <span data-ttu-id="442fe-381">Список изменений используется контекстом ObjectContext во время метода SaveChanges для сохранения измененных сущностей с помощью команд UPDATE.</span><span class="sxs-lookup"><span data-stu-id="442fe-381">The list of changes is used by the ObjectContext during the SaveChanges method to persist any modified entities using UPDATE commands.</span></span>

<span data-ttu-id="442fe-382">Однако для работы этих прокси-серверов требуется способ подключения к операциям get и Set в отношении сущности, а прокси для достижения этой цели — переопределения виртуальных членов.</span><span class="sxs-lookup"><span data-stu-id="442fe-382">For these proxies to work, however, they need a way to hook into property get and set operations on an entity, and the proxies achieve this goal by overriding virtual members.</span></span> <span data-ttu-id="442fe-383">Таким образом, если требуется неявная отложенная загрузка и эффективное отслеживание изменений, нам нужно вернуться к нашим определениям классов POCO и пометить свойства как виртуальные.</span><span class="sxs-lookup"><span data-stu-id="442fe-383">Thus, if we want to have implicit lazy loading and efficient change tracking we need to go back to our POCO class definitions and mark properties as virtual.</span></span>

``` csharp
    public class Employee {
        public virtual int Id { get; set; }
        public virtual string Name { get; set; }
        public virtual DateTime HireDate { get; set; }
        public virtual ICollection<TimeCard> TimeCards { get; set; }
    }
```

<span data-ttu-id="442fe-384">Мы по-прежнему можем сказать, что сущность сотрудника в основном является сохраняемостью игнорирующих.</span><span class="sxs-lookup"><span data-stu-id="442fe-384">We can still say the Employee entity is mostly persistence ignorant.</span></span> <span data-ttu-id="442fe-385">Единственным требованием является использование виртуальных членов, и это не влияет на тестируемость кода.</span><span class="sxs-lookup"><span data-stu-id="442fe-385">The only requirement is to use virtual members and this does not impact the testability of the code.</span></span> <span data-ttu-id="442fe-386">Не нужно создавать производный от какого-либо специального базового класса или даже использовать специальную коллекцию, выделенную для отложенной загрузки.</span><span class="sxs-lookup"><span data-stu-id="442fe-386">We don’t need to derive from any special base class, or even use a special collection dedicated to lazy loading.</span></span> <span data-ttu-id="442fe-387">Как показано в коде, любой класс, реализующий ICollection @ no__t-0T @ no__t-1, доступен для хранения связанных сущностей.</span><span class="sxs-lookup"><span data-stu-id="442fe-387">As the code demonstrates, any class implementing ICollection&lt;T&gt; is available to hold related entities.</span></span>

<span data-ttu-id="442fe-388">Существует также одно небольшое изменение, которое необходимо сделать в нашей единице работы.</span><span class="sxs-lookup"><span data-stu-id="442fe-388">There is also one minor change we need to make inside our unit of work.</span></span> <span data-ttu-id="442fe-389">Отложенная загрузка *отключена* по умолчанию при работе непосредственно с объектом ObjectContext.</span><span class="sxs-lookup"><span data-stu-id="442fe-389">Lazy loading is *off* by default when working directly with an ObjectContext object.</span></span> <span data-ttu-id="442fe-390">Существует свойство, которое можно задать для свойства Контекстоптионс, чтобы включить отложенную загрузку, и мы можем установить это свойство в реальной единице работы, если мы хотим включить отложенную загрузку везде.</span><span class="sxs-lookup"><span data-stu-id="442fe-390">There is a property we can set on the ContextOptions property to enable deferred loading, and we can set this property inside our real unit of work if we want to enable lazy loading everywhere.</span></span>

``` csharp
    public class SqlUnitOfWork : IUnitOfWork {
         public SqlUnitOfWork() {
             // ...
             _context = new ObjectContext(connectionString);
             _context.ContextOptions.LazyLoadingEnabled = true;
         }    
         // ...
     }
```

<span data-ttu-id="442fe-391">Если включена неявная отложенная загрузка, код приложения может использовать сотрудника и связанные с ним карты времени, в то время как оставшиеся счастливом неведении не знают о работе, необходимой EF для загрузки дополнительных данных.</span><span class="sxs-lookup"><span data-stu-id="442fe-391">With implicit lazy loading enabled, application code can use an employee and the employee’s associated time cards while remaining blissfully unaware of the work required for EF to load the extra data.</span></span>

``` csharp
    var employee = _unitOfWork.Employees
                              .Single(e => e.Id == id);
    foreach (var card in employee.TimeCards) {
        // ...
    }
```

<span data-ttu-id="442fe-392">Отложенная загрузка упрощает написание кода приложения, а с помощью прокси-сервера код остается полностью тестируемым.</span><span class="sxs-lookup"><span data-stu-id="442fe-392">Lazy loading makes the application code easier to write, and with the proxy magic the code remains completely testable.</span></span> <span data-ttu-id="442fe-393">Поддельные единицы работы в памяти позволяют просто предварительно загружать фиктивные сущности со связанными данными при необходимости во время тестирования.</span><span class="sxs-lookup"><span data-stu-id="442fe-393">In-memory fakes of the unit of work can simply preload fake entities with associated data when needed during a test.</span></span>

<span data-ttu-id="442fe-394">На этом этапе мы сделаем наш контроль над созданием репозиториев с помощью Иобжектсет @ no__t-0T @ no__t-1 и взглянем на абстракции, чтобы скрыть все знаки инфраструктуры сохраняемости.</span><span class="sxs-lookup"><span data-stu-id="442fe-394">At this point we’ll turn our attention from building repositories using IObjectSet&lt;T&gt; and look at abstractions to hide all signs of the persistence framework.</span></span>

## <a name="custom-repositories"></a><span data-ttu-id="442fe-395">Пользовательские репозитории</span><span class="sxs-lookup"><span data-stu-id="442fe-395">Custom Repositories</span></span>

<span data-ttu-id="442fe-396">Когда мы впервые предоставили шаблон проектирования единиц работы в этой статье, мы предоставляли пример кода для того, как может выглядеть единица работы.</span><span class="sxs-lookup"><span data-stu-id="442fe-396">When we first presented the unit of work design pattern in this article we provided some sample code for what the unit of work might look like.</span></span> <span data-ttu-id="442fe-397">Восстановите эту исходную идею с помощью сценария рабочего времени сотрудника и сотрудника, с которым мы работаем.</span><span class="sxs-lookup"><span data-stu-id="442fe-397">Let’s re-present this original idea using the employee and employee time card scenario we’ve been working with.</span></span>

``` csharp
    public interface IUnitOfWork {
        IRepository<Employee> Employees { get; }
        IRepository<TimeCard> TimeCards { get;  }
        void Commit();
    }
```

<span data-ttu-id="442fe-398">Основное различие между этой единицей работы и единицей работы, созданным в последнем разделе, заключается в том, как эта единица работы не использует абстракции из платформы EF4 (отсутствует Иобжектсет @ no__t-0T @ no__t-1).</span><span class="sxs-lookup"><span data-stu-id="442fe-398">The primary difference between this unit of work and the unit of work we created in the last section is how this unit of work does not use any abstractions from the EF4 framework (there is no IObjectSet&lt;T&gt;).</span></span> <span data-ttu-id="442fe-399">Иобжектсет @ no__t-0T @ no__t-1 работает в качестве интерфейса репозитория, но предоставляемый им API может быть не вполне согласован с потребностями нашего приложения.</span><span class="sxs-lookup"><span data-stu-id="442fe-399">IObjectSet&lt;T&gt; works well as a repository interface, but the API it exposes might not perfectly align with our application’s needs.</span></span> <span data-ttu-id="442fe-400">В этом предстоящем подходе мы будем представлять репозитории с помощью пользовательской абстракции IRepository @ no__t-0T @ no__t-1.</span><span class="sxs-lookup"><span data-stu-id="442fe-400">In this upcoming approach we will represent repositories using a custom IRepository&lt;T&gt; abstraction.</span></span>

<span data-ttu-id="442fe-401">Многие разработчики, которые следуют разработке на основе тестирования, проектирования на основе поведения и методологии, ориентированной на работу с доменами, предпочитают подход IRepository @ no__t-0T @ no__t-1 по нескольким причинам.</span><span class="sxs-lookup"><span data-stu-id="442fe-401">Many developers who follow test-driven design, behavior-driven design, and domain driven methodologies design prefer the IRepository&lt;T&gt; approach for several reasons.</span></span> <span data-ttu-id="442fe-402">Во-первых, интерфейс IRepository @ no__t-0T @ no__t-1 представляет уровень защиты от повреждений.</span><span class="sxs-lookup"><span data-stu-id="442fe-402">First, the IRepository&lt;T&gt; interface represents an “anti-corruption” layer.</span></span> <span data-ttu-id="442fe-403">Как описано в руководстве по созданию и Эванс в своей книге разработки, основанной на домене, уровень защиты от вредоносных программ удерживает ваш код домена от API-интерфейсов инфраструктуры, таких как API сохраняемости.</span><span class="sxs-lookup"><span data-stu-id="442fe-403">As described by Eric Evans in his Domain Driven Design book an anti-corruption layer keeps your domain code away from infrastructure APIs, like a persistence API.</span></span> <span data-ttu-id="442fe-404">Во-вторых, разработчики могут создавать в репозитории методы, соответствующие точным потребностям приложения (как было обнаружено при написании тестов).</span><span class="sxs-lookup"><span data-stu-id="442fe-404">Secondly, developers can build methods into the repository that meet the exact needs of an application (as discovered while writing tests).</span></span> <span data-ttu-id="442fe-405">Например, нам часто приходится искать одну сущность, используя значение идентификатора, поэтому мы можем добавить метод Финдбид в интерфейс репозитория.</span><span class="sxs-lookup"><span data-stu-id="442fe-405">For example, we might frequently need to locate a single entity using an ID value, so we can add a FindById method to the repository interface.</span></span><span data-ttu-id="442fe-406">  Наше определение IRepository @ no__t-0T @ no__t-1 будет выглядеть следующим образом.</span><span class="sxs-lookup"><span data-stu-id="442fe-406">  Our IRepository&lt;T&gt; definition will look like the following.</span></span>

``` csharp
    public interface IRepository<T>
                    where T : class, IEntity {
        IQueryable<T> FindAll();
        IQueryable<T> FindWhere(Expression<Func\<T, bool>> predicate);
        T FindById(int id);       
        void Add(T newEntity);
        void Remove(T entity);
    }
```

<span data-ttu-id="442fe-407">Обратите внимание, что мы вернемся к использованию интерфейса IQueryable @ no__t-0T @ no__t-1 для предоставления коллекций сущностей.</span><span class="sxs-lookup"><span data-stu-id="442fe-407">Notice we’ll drop back to using an IQueryable&lt;T&gt; interface to expose entity collections.</span></span> <span data-ttu-id="442fe-408">IQueryable @ no__t-0T @ no__t-1 позволяет деревьям выражений LINQ передаваться в поставщик EF4 и предоставлять поставщику целостное представление запроса.</span><span class="sxs-lookup"><span data-stu-id="442fe-408">IQueryable&lt;T&gt; allows LINQ expression trees to flow into the EF4 provider and give the provider a holistic view of the query.</span></span> <span data-ttu-id="442fe-409">Вторым вариантом будет возврат IEnumerable @ no__t-0T @ no__t-1, что означает, что поставщик EF4 LINQ будет видеть только выражения, собранные внутри репозитория.</span><span class="sxs-lookup"><span data-stu-id="442fe-409">A second option would be to return IEnumerable&lt;T&gt;, which means the EF4 LINQ provider will only see the expressions built inside of the repository.</span></span> <span data-ttu-id="442fe-410">Группирование, упорядочивание и проекция, выполненные за пределами репозитория, не будут содержаться в команде SQL, отправленной в базу данных, что может понизить производительность.</span><span class="sxs-lookup"><span data-stu-id="442fe-410">Any grouping, ordering, and projection done outside of the repository will not be composed into the SQL command sent to the database, which can hurt performance.</span></span> <span data-ttu-id="442fe-411">С другой стороны, репозиторий, возвращающий только результаты IEnumerable @ no__t-0T @ no__t-1, никогда не поудивит вас с новой командой SQL.</span><span class="sxs-lookup"><span data-stu-id="442fe-411">On the other hand, a repository returning only IEnumerable&lt;T&gt; results will never surprise you with a new SQL command.</span></span> <span data-ttu-id="442fe-412">Оба подхода будут работать, и оба подхода останутся тестируемыми.</span><span class="sxs-lookup"><span data-stu-id="442fe-412">Both approaches will work, and both approaches remain testable.</span></span>

<span data-ttu-id="442fe-413">Очень просто предоставить единую реализацию интерфейса IRepository @ no__t-0T @ no__t-1, используя универсальные шаблоны и API-интерфейс EF4 ObjectContext.</span><span class="sxs-lookup"><span data-stu-id="442fe-413">It’s straightforward to provide a single implementation of the IRepository&lt;T&gt; interface using generics and the EF4 ObjectContext API.</span></span>

``` csharp
    public class SqlRepository<T> : IRepository<T>
                                    where T : class, IEntity {
        public SqlRepository(ObjectContext context) {
            _objectSet = context.CreateObjectSet<T>();
        }
        public IQueryable<T> FindAll() {
            return _objectSet;
        }
        public IQueryable<T> FindWhere(
                               Expression<Func\<T, bool>> predicate) {
            return _objectSet.Where(predicate);
        }
        public T FindById(int id) {
            return _objectSet.Single(o => o.Id == id);
        }
        public void Add(T newEntity) {
            _objectSet.AddObject(newEntity);
        }
        public void Remove(T entity) {
            _objectSet.DeleteObject(entity);
        }
        protected ObjectSet<T> _objectSet;
    }
```

<span data-ttu-id="442fe-414">Подход IRepository @ no__t-0T @ no__t-1 предоставляет нам дополнительный контроль над нашими запросами, так как клиент должен вызвать метод для получения сущности.</span><span class="sxs-lookup"><span data-stu-id="442fe-414">The IRepository&lt;T&gt; approach gives us some additional control over our queries because a client has to invoke a method to get to an entity.</span></span> <span data-ttu-id="442fe-415">Внутри метода можно предоставить дополнительные проверки и операторы LINQ для применения ограничений приложения.</span><span class="sxs-lookup"><span data-stu-id="442fe-415">Inside the method we could provide additional checks and LINQ operators to enforce application constraints.</span></span> <span data-ttu-id="442fe-416">Обратите внимание, что интерфейс имеет два ограничения для параметра универсального типа.</span><span class="sxs-lookup"><span data-stu-id="442fe-416">Notice the interface has two constraints on the generic type parameter.</span></span> <span data-ttu-id="442fe-417">Первое ограничение — это класс таинт, необходимый для Objecting @ no__t-0T @ no__t-1, а второе ограничение заставляет наши сущности реализовать Иентити — абстракцию, созданной для приложения.</span><span class="sxs-lookup"><span data-stu-id="442fe-417">The first constraint is the class cons taint required by ObjectSet&lt;T&gt;, and the second constraint forces our entities to implement IEntity – an abstraction created for the application.</span></span> <span data-ttu-id="442fe-418">Интерфейс Иентити заставляет сущности иметь доступное для чтения свойство идентификатора, а затем это свойство можно использовать в методе Финдбид.</span><span class="sxs-lookup"><span data-stu-id="442fe-418">The IEntity interface forces entities to have a readable Id property, and we can then use this property in the FindById method.</span></span> <span data-ttu-id="442fe-419">Иентити определяется с помощью следующего кода.</span><span class="sxs-lookup"><span data-stu-id="442fe-419">IEntity is defined with the following code.</span></span>

``` csharp
    public interface IEntity {
        int Id { get; }
    }
```

<span data-ttu-id="442fe-420">Иентити можно считать небольшим нарушением игнорирования сохраняемости, так как наши сущности должны реализовать этот интерфейс.</span><span class="sxs-lookup"><span data-stu-id="442fe-420">IEntity could be considered a small violation of persistence ignorance since our entities are required to implement this interface.</span></span> <span data-ttu-id="442fe-421">Помните, что игнорирования сохраняемости имеет отношение к компромиссам, а многие функции Финдбид перевешивают ограничение, наложенное интерфейсом.</span><span class="sxs-lookup"><span data-stu-id="442fe-421">Remember persistence ignorance is about tradeoffs, and for many the FindById functionality will outweigh the constraint imposed by the interface.</span></span> <span data-ttu-id="442fe-422">Интерфейс не влияет на пригодность для тестирования.</span><span class="sxs-lookup"><span data-stu-id="442fe-422">The interface has no impact on testability.</span></span>

<span data-ttu-id="442fe-423">Для создания экземпляра Live IRepository @ no__t-0T @ no__t-1 требуется EF4 ObjectContext, поэтому конкретная реализация единиц работы должна управлять созданием экземпляра.</span><span class="sxs-lookup"><span data-stu-id="442fe-423">Instantiating a live IRepository&lt;T&gt; requires an EF4 ObjectContext, so a concrete unit of work implementation should manage the instantiation.</span></span>

``` csharp
    public class SqlUnitOfWork : IUnitOfWork {
        public SqlUnitOfWork() {
            var connectionString =
                ConfigurationManager
                    .ConnectionStrings[ConnectionStringName]
                    .ConnectionString;

            _context = new ObjectContext(connectionString);
            _context.ContextOptions.LazyLoadingEnabled = true;
        }

        public IRepository<Employee> Employees {
            get {
                if (_employees == null) {
                    _employees = new SqlRepository<Employee>(_context);
                }
                return _employees;
            }
        }

        public IRepository<TimeCard> TimeCards {
            get {
                if (_timeCards == null) {
                    _timeCards = new SqlRepository<TimeCard>(_context);
                }
                return _timeCards;
            }
        }

        public void Commit() {
            _context.SaveChanges();
        }

        SqlRepository<Employee> _employees = null;
        SqlRepository<TimeCard> _timeCards = null;
        readonly ObjectContext _context;
        const string ConnectionStringName = "EmployeeDataModelContainer";
    }
```

### <a name="using-the-custom-repository"></a><span data-ttu-id="442fe-424">Использование пользовательского репозитория</span><span class="sxs-lookup"><span data-stu-id="442fe-424">Using the Custom Repository</span></span>

<span data-ttu-id="442fe-425">Использование нашего пользовательского репозитория незначительно отличается от использования репозитория на основе Иобжектсет @ no__t-0T @ no__t-1.</span><span class="sxs-lookup"><span data-stu-id="442fe-425">Using our custom repository is not significantly different from using the repository based on IObjectSet&lt;T&gt;.</span></span> <span data-ttu-id="442fe-426">Вместо применения операторов LINQ непосредственно к свойству сначала необходимо вызвать один из методов репозитория, чтобы получить ссылку на IQueryable @ no__t-0T @ no__t-1.</span><span class="sxs-lookup"><span data-stu-id="442fe-426">Instead of applying LINQ operators directly to a property, we’ll first need to invoke one the repository’s methods to grab an IQueryable&lt;T&gt; reference.</span></span>

``` csharp
    public ViewResult Index() {
        var model = _repository.FindAll()
                               .Include("TimeCards")
                               .OrderBy(e => e.HireDate);
        return View(model);
    }
```

<span data-ttu-id="442fe-427">Обратите внимание, что ранее реализованный оператор include будет работать без изменений.</span><span class="sxs-lookup"><span data-stu-id="442fe-427">Notice the custom Include operator we implemented previously will work without change.</span></span> <span data-ttu-id="442fe-428">Метод Финдбид репозитория удаляет повторяющуюся логику из действий, пытающихся получить одну сущность.</span><span class="sxs-lookup"><span data-stu-id="442fe-428">The repository’s FindById method removes duplicated logic from actions trying to retrieve a single entity.</span></span>

``` csharp
    public ViewResult Details(int id) {
        var model = _repository.FindById(id);
        return View(model);
    }
```

<span data-ttu-id="442fe-429">Мы не имеем существенных различий в тестировании двух подходов, которые мы рассматривали.</span><span class="sxs-lookup"><span data-stu-id="442fe-429">There is no significant difference in the testability of the two approaches we’ve examined.</span></span> <span data-ttu-id="442fe-430">Мы можем предоставить фиктивные реализации IRepository @ no__t-0T @ no__t-1, создав конкретные классы, поддерживающие хэширование @ no__t-2Employee @ no__t-3 — как и в последнем разделе.</span><span class="sxs-lookup"><span data-stu-id="442fe-430">We could provide fake implementations of IRepository&lt;T&gt; by building concrete classes backed by HashSet&lt;Employee&gt; - just like what we did in the last section.</span></span> <span data-ttu-id="442fe-431">Однако некоторые разработчики предпочитают использовать макеты объектов и макеты объектных платформ вместо создания фиктивных.</span><span class="sxs-lookup"><span data-stu-id="442fe-431">However, some developers prefer to use mock objects and mock object frameworks instead of building fakes.</span></span> <span data-ttu-id="442fe-432">Мы рассмотрим использование макетов для тестирования нашей реализации и обсуждение различий между макетами и подложными в следующем разделе.</span><span class="sxs-lookup"><span data-stu-id="442fe-432">We’ll look at using mocks to test our implementation and discuss the differences between mocks and fakes in the next section.</span></span>

### <a name="testing-with-mocks"></a><span data-ttu-id="442fe-433">Тестирование с помощью макетов</span><span class="sxs-lookup"><span data-stu-id="442fe-433">Testing with Mocks</span></span>

<span data-ttu-id="442fe-434">Существует несколько подходов к созданию того, что Мартен Фаулер вызывает "Test Double".</span><span class="sxs-lookup"><span data-stu-id="442fe-434">There are different approaches to building what Martin Fowler calls a “test double”.</span></span> <span data-ttu-id="442fe-435">Тестовый двойной (например, ролик стунт Double) — это объект, который вы создаете для реальных, рабочих объектов во время тестов.</span><span class="sxs-lookup"><span data-stu-id="442fe-435">A test double (like a movie stunt double) is an object you build to “stand in” for real, production objects during tests.</span></span> <span data-ttu-id="442fe-436">Создаваемые нами репозитории в памяти — это тестовые Double для репозиториев, которые говорят SQL Server.</span><span class="sxs-lookup"><span data-stu-id="442fe-436">The in-memory repositories we created are test doubles for the repositories that talk to SQL Server.</span></span> <span data-ttu-id="442fe-437">Мы рассмотрели, как использовать эти тестовые-Double во время модульных тестов для изоляции кода и быстрого выполнения тестов.</span><span class="sxs-lookup"><span data-stu-id="442fe-437">We’ve seen how to use these test-doubles during the unit tests to isolate code and keep tests running fast.</span></span>

<span data-ttu-id="442fe-438">Создаваемые двойные числа тестов имеют реальную и рабочую реализацию.</span><span class="sxs-lookup"><span data-stu-id="442fe-438">The test doubles we’ve built have real, working implementations.</span></span> <span data-ttu-id="442fe-439">В фоновом режиме каждый из них хранит конкретную коллекцию объектов, и они будут добавлять и удалять объекты из этой коллекции по мере работы с репозиторием во время теста.</span><span class="sxs-lookup"><span data-stu-id="442fe-439">Behind the scenes each one stores a concrete collection of objects, and they will add and remove objects from this collection as we manipulate the repository during a test.</span></span> <span data-ttu-id="442fe-440">Некоторые разработчики, например, могут создавать свои тестовые двойные значения таким образом, с реальным кодом и рабочими реализациями.</span><span class="sxs-lookup"><span data-stu-id="442fe-440">Some developers like to build their test doubles this way – with real code and working implementations.</span></span><span data-ttu-id="442fe-441">  Эти тестовые удвоения — это то, что мы вызываем *поддельные*объекты.</span><span class="sxs-lookup"><span data-stu-id="442fe-441">  These test doubles are what we call *fakes*.</span></span> <span data-ttu-id="442fe-442">У них есть рабочие реализации, но они не настолько актуальны для использования в рабочей среде.</span><span class="sxs-lookup"><span data-stu-id="442fe-442">They have working implementations, but they aren’t real enough for production use.</span></span> <span data-ttu-id="442fe-443">Фиктивный репозиторий на самом деле не выполняет запись в базу данных.</span><span class="sxs-lookup"><span data-stu-id="442fe-443">The fake repository doesn’t actually write to the database.</span></span> <span data-ttu-id="442fe-444">Фальшивый SMTP-сервер на самом деле не отправляет электронное сообщение по сети.</span><span class="sxs-lookup"><span data-stu-id="442fe-444">The fake SMTP server doesn’t actually send an email message over the network.</span></span>

### <a name="mocks-versus-fakes"></a><span data-ttu-id="442fe-445">Макеты и имитации</span><span class="sxs-lookup"><span data-stu-id="442fe-445">Mocks versus Fakes</span></span>

<span data-ttu-id="442fe-446">Существует другой тип теста, который дважды известен как *Макет*.</span><span class="sxs-lookup"><span data-stu-id="442fe-446">There is another type of test double known as a *mock*.</span></span> <span data-ttu-id="442fe-447">Хотя имитации имеют рабочие реализации, макеты поставляются без реализации.</span><span class="sxs-lookup"><span data-stu-id="442fe-447">While fakes have working implementations, mocks come with no implementation.</span></span> <span data-ttu-id="442fe-448">С помощью макетной платформы объектов мы создаем эти макеты объектов во время выполнения и используем их в качестве тестовых значений Double.</span><span class="sxs-lookup"><span data-stu-id="442fe-448">With the help of a mock object framework we construct these mock objects at run time and use them as test doubles.</span></span> <span data-ttu-id="442fe-449">В этом разделе будет использоваться инфраструктура макетирования с открытым исходным кодом MOQ.</span><span class="sxs-lookup"><span data-stu-id="442fe-449">In this section we’ll be using the open source mocking framework Moq.</span></span> <span data-ttu-id="442fe-450">Ниже приведен простой пример использования MOQ для динамического создания тестового типа Double для репозитория сотрудников.</span><span class="sxs-lookup"><span data-stu-id="442fe-450">Here is a simple example of using Moq to dynamically create a test double for an employee repository.</span></span>

``` csharp
    Mock<IRepository<Employee>> mock =
        new Mock<IRepository<Employee>>();
    IRepository<Employee> repository = mock.Object;
    repository.Add(new Employee());
    var employee = repository.FindById(1);
```

<span data-ttu-id="442fe-451">Мы задаем MOQ для реализации IRepository @ no__t-0Employee @ no__t-1, и она создает ее динамически.</span><span class="sxs-lookup"><span data-stu-id="442fe-451">We ask Moq for an IRepository&lt;Employee&gt; implementation and it builds one dynamically.</span></span> <span data-ttu-id="442fe-452">Чтобы получить доступ к объекту, реализующему IRepository @ no__t-0Employee @ no__t-1, можно обратиться к свойству Object объекта макетирование @ no__t-2T @ no__t-3.</span><span class="sxs-lookup"><span data-stu-id="442fe-452">We can get to the object implementing IRepository&lt;Employee&gt; by accessing the Object property of the Mock&lt;T&gt; object.</span></span> <span data-ttu-id="442fe-453">Это внутренний объект, который мы можем передать в контроллеры, и они не будут уверены, что это тестовый двойной или реальный репозиторий.</span><span class="sxs-lookup"><span data-stu-id="442fe-453">It is this inner object we can pass into our controllers, and they won’t know if this is a test double or the real repository.</span></span> <span data-ttu-id="442fe-454">Мы можем вызывать методы для объекта точно так же, как мы бы вызывали методы для объекта с реальной реализацией.</span><span class="sxs-lookup"><span data-stu-id="442fe-454">We can invoke methods on the object just like we would invoke methods on an object with a real implementation.</span></span>

<span data-ttu-id="442fe-455">Необходимо знать, что делает макет репозитория при вызове метода Add.</span><span class="sxs-lookup"><span data-stu-id="442fe-455">You must be wondering what the mock repository will do when we invoke the Add method.</span></span> <span data-ttu-id="442fe-456">Поскольку нет реализации, основанной на макете объекта, Add не выполняет никаких действий.</span><span class="sxs-lookup"><span data-stu-id="442fe-456">Since there is no implementation behind the mock object, Add does nothing.</span></span> <span data-ttu-id="442fe-457">Не существует какой-то конкретной коллекции, как в случае с имитацией, которую мы написали, поэтому сотрудник отклоняется.</span><span class="sxs-lookup"><span data-stu-id="442fe-457">There is no concrete collection behind the scenes like we had with the fakes we wrote, so the employee is discarded.</span></span> <span data-ttu-id="442fe-458">Как насчет возвращаемого значения Финдбид?</span><span class="sxs-lookup"><span data-stu-id="442fe-458">What about the return value of FindById?</span></span> <span data-ttu-id="442fe-459">В этом случае макет объекта делает только то, что он может сделать, что возвращает значение по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="442fe-459">In this case the mock object does the only thing it can do, which is return a default value.</span></span> <span data-ttu-id="442fe-460">Так как мы возвращаем ссылочный тип (Employee), возвращаемое значение имеет значение null.</span><span class="sxs-lookup"><span data-stu-id="442fe-460">Since we are returning a reference type (an Employee), the return value is a null value.</span></span>

<span data-ttu-id="442fe-461">Макеты могут показаться бесполезнымиными; Однако есть еще две функции макетов, о которых мы не говорили.</span><span class="sxs-lookup"><span data-stu-id="442fe-461">Mocks might sound worthless; however, there are two more features of mocks we haven’t talked about.</span></span> <span data-ttu-id="442fe-462">Во первых, платформа MOQ записывает все вызовы, сделанные в макете объекта.</span><span class="sxs-lookup"><span data-stu-id="442fe-462">First, the Moq framework records all the calls made on the mock object.</span></span> <span data-ttu-id="442fe-463">Далее в коде мы можем спросить MOQ, если кто угодно вызвал метод Add, или если кто угодно вызвал метод Финдбид.</span><span class="sxs-lookup"><span data-stu-id="442fe-463">Later in the code we can ask Moq if anyone invoked the Add method, or if anyone invoked the FindById method.</span></span> <span data-ttu-id="442fe-464">Позже мы увидим, как можно использовать эту функцию записи "черного ящика" в тестах.</span><span class="sxs-lookup"><span data-stu-id="442fe-464">We’ll see later how we can use this “black box” recording feature in tests.</span></span>

<span data-ttu-id="442fe-465">Второй замечательной функцией является то, как мы можем использовать MOQ для программирования макета объекта с *ожиданиями*.</span><span class="sxs-lookup"><span data-stu-id="442fe-465">The second great feature is how we can use Moq to program a mock object with *expectations*.</span></span> <span data-ttu-id="442fe-466">Ожидание указывает макету объекта, как реагировать на любое заданное взаимодействие.</span><span class="sxs-lookup"><span data-stu-id="442fe-466">An expectation tells the mock object how to respond to any given interaction.</span></span> <span data-ttu-id="442fe-467">Например, можно запрограммировать ожидание в макете и сообщить ему, что он возвращает объект Employee, когда кто-то вызывает Финдбид.</span><span class="sxs-lookup"><span data-stu-id="442fe-467">For example, we can program an expectation into our mock and tell it to return an employee object when someone invokes FindById.</span></span> <span data-ttu-id="442fe-468">Платформа MOQ использует API установки и лямбда-выражения для программирования этих ожиданий.</span><span class="sxs-lookup"><span data-stu-id="442fe-468">The Moq framework uses a Setup API and lambda expressions to program these expectations.</span></span>

``` csharp
    [TestMethod]
    public void MockSample() {
        Mock<IRepository<Employee>> mock =
            new Mock<IRepository<Employee>>();
        mock.Setup(m => m.FindById(5))
            .Returns(new Employee {Id = 5});
        IRepository<Employee> repository = mock.Object;
        var employee = repository.FindById(5);
        Assert.IsTrue(employee.Id == 5);
    }
```

<span data-ttu-id="442fe-469">В этом примере мы просим MOQ динамически создавать репозиторий, а затем запрограммирован репозиторий с ожиданием.</span><span class="sxs-lookup"><span data-stu-id="442fe-469">In this sample we ask Moq to dynamically build a repository, and then we program the repository with an expectation.</span></span> <span data-ttu-id="442fe-470">Ожидание означает, что макет объекта возвращает новый объект Employee со значением идентификатора 5, когда кто-то вызывает метод Финдбид, передающий значение 5.</span><span class="sxs-lookup"><span data-stu-id="442fe-470">The expectation tells the mock object to return a new employee object with an Id value of 5 when someone invokes the FindById method passing a value of 5.</span></span> <span data-ttu-id="442fe-471">Этот тест пройден, и нам не нужно было создавать полную реализацию для имитации IRepository @ no__t-0T @ no__t-1.</span><span class="sxs-lookup"><span data-stu-id="442fe-471">This test passes, and we didn’t need to build a full implementation to fake IRepository&lt;T&gt;.</span></span>

<span data-ttu-id="442fe-472">Давайте вернемся к тестам, которые мы написали ранее, и переработали их, чтобы использовать макеты вместо имитаций.</span><span class="sxs-lookup"><span data-stu-id="442fe-472">Let’s revisit the tests we wrote earlier and rework them to use mocks instead of fakes.</span></span> <span data-ttu-id="442fe-473">Как и раньше, мы будем использовать базовый класс для настройки общих частей инфраструктуры, необходимых для всех тестов контроллера.</span><span class="sxs-lookup"><span data-stu-id="442fe-473">Just like before, we’ll use a base class to setup the common pieces of infrastructure we need for all of the controller’s tests.</span></span>

``` csharp
    public class EmployeeControllerTestBase {
        public EmployeeControllerTestBase() {
            _employeeData = EmployeeObjectMother.CreateEmployees()
                                                .AsQueryable();
            _repository = new Mock<IRepository<Employee>>();
            _unitOfWork = new Mock<IUnitOfWork>();
            _unitOfWork.Setup(u => u.Employees)
                       .Returns(_repository.Object);
            _controller = new EmployeeController(_unitOfWork.Object);
        }

        protected IQueryable<Employee> _employeeData;
        protected Mock<IUnitOfWork> _unitOfWork;
        protected EmployeeController _controller;
        protected Mock<IRepository<Employee>> _repository;
    }
```

<span data-ttu-id="442fe-474">Код установки остается главным образом.</span><span class="sxs-lookup"><span data-stu-id="442fe-474">The setup code remains mostly the same.</span></span> <span data-ttu-id="442fe-475">Вместо использования имитаций мы будем использовать MOQ для создания макетных объектов.</span><span class="sxs-lookup"><span data-stu-id="442fe-475">Instead of using fakes, we’ll use Moq to construct mock objects.</span></span> <span data-ttu-id="442fe-476">Базовый класс упорядочивает макетную единицу работы, чтобы вернуть макет репозитория, когда код вызывает свойство Employees.</span><span class="sxs-lookup"><span data-stu-id="442fe-476">The base class arranges for the mock unit of work to return a mock repository when code invokes the Employees property.</span></span> <span data-ttu-id="442fe-477">Оставшаяся часть установки макета будет выполняться внутри набора тестов, предназначенного для каждого конкретного сценария.</span><span class="sxs-lookup"><span data-stu-id="442fe-477">The rest of the mock setup will take place inside the test fixtures dedicated to each specific scenario.</span></span> <span data-ttu-id="442fe-478">Например, средство тестирования для действия с индексом настраивает макет репозитория для возврата списка сотрудников, когда действие вызывает метод FindAll макетного репозитория.</span><span class="sxs-lookup"><span data-stu-id="442fe-478">For example, the test fixture for the Index action will setup the mock repository to return a list of employees when the action invokes the FindAll method of the mock repository.</span></span>

``` csharp
    [TestClass]
    public class EmployeeControllerIndexActionTests
               : EmployeeControllerTestBase {
        public EmployeeControllerIndexActionTests() {
            _repository.Setup(r => r.FindAll())
                        .Returns(_employeeData);
        }
        // .. tests
        [TestMethod]
        public void ShouldBuildModelWithAllEmployees() {
            var result = _controller.Index();
            var model = result.ViewData.Model
                          as IEnumerable<Employee>;
            Assert.IsTrue(model.Count() == _employeeData.Count());
        }
        // .. and more tests
    }
```

<span data-ttu-id="442fe-479">За исключением ожиданий, наши тесты похожи на тесты, ранее имевшие место.</span><span class="sxs-lookup"><span data-stu-id="442fe-479">Except for the expectations, our tests look similar to the tests we had before.</span></span> <span data-ttu-id="442fe-480">Тем не менее, с возможностью записи структуры макета можно подходить к тестированию с другого угла.</span><span class="sxs-lookup"><span data-stu-id="442fe-480">However, with the recording ability of a mock framework we can approach testing from a different angle.</span></span> <span data-ttu-id="442fe-481">В следующем разделе мы рассмотрим эту новую перспективу.</span><span class="sxs-lookup"><span data-stu-id="442fe-481">We’ll look at this new perspective in the next section.</span></span>

### <a name="state-versus-interaction-testing"></a><span data-ttu-id="442fe-482">Тестирование состояния и взаимодействия</span><span class="sxs-lookup"><span data-stu-id="442fe-482">State versus Interaction Testing</span></span>

<span data-ttu-id="442fe-483">Существуют различные методы, которые можно использовать для тестирования программного обеспечения с помощью макетов объектов.</span><span class="sxs-lookup"><span data-stu-id="442fe-483">There are different techniques you can use to test software with mock objects.</span></span> <span data-ttu-id="442fe-484">Одним из подходов является использование тестирования на основе состояния, которое мы уже сделали в этой статье.</span><span class="sxs-lookup"><span data-stu-id="442fe-484">One approach is to use state based testing, which is what we have done in this paper so far.</span></span> <span data-ttu-id="442fe-485">Тестирование на основе состояния делает утверждения о состоянии программного обеспечения.</span><span class="sxs-lookup"><span data-stu-id="442fe-485">State based testing makes assertions about the state of the software.</span></span> <span data-ttu-id="442fe-486">В последнем тесте мы вызвали метод действия на контроллере и внесли утверждение о модели, которую она должна создать.</span><span class="sxs-lookup"><span data-stu-id="442fe-486">In the last test we invoked an action method on the controller and made an assertion about the model it should build.</span></span> <span data-ttu-id="442fe-487">Ниже приведены некоторые другие примеры состояния тестирования.</span><span class="sxs-lookup"><span data-stu-id="442fe-487">Here are some other examples of testing state:</span></span>

-   <span data-ttu-id="442fe-488">Убедитесь, что репозиторий содержит новый объект Employee после выполнения инструкции CREATE.</span><span class="sxs-lookup"><span data-stu-id="442fe-488">Verify the repository contains the new employee object after Create executes.</span></span>
-   <span data-ttu-id="442fe-489">Убедитесь, что модель содержит список всех сотрудников после выполнения индекса.</span><span class="sxs-lookup"><span data-stu-id="442fe-489">Verify the model holds a list of all employees after Index executes.</span></span>
-   <span data-ttu-id="442fe-490">Убедитесь, что репозиторий не содержит данного сотрудника после выполнения инструкции DELETE.</span><span class="sxs-lookup"><span data-stu-id="442fe-490">Verify the repository does not contain a given employee after Delete executes.</span></span>

<span data-ttu-id="442fe-491">Другим подходом, который вы увидите с макетами объектов, является проверка *взаимодействия*.</span><span class="sxs-lookup"><span data-stu-id="442fe-491">Another approach you’ll see with mock objects is to verify *interactions*.</span></span> <span data-ttu-id="442fe-492">Хотя тестирование на основе состояния делает утверждения о состоянии объектов, проверка на основе взаимодействия делает утверждения о взаимодействии объектов.</span><span class="sxs-lookup"><span data-stu-id="442fe-492">While state based testing makes assertions about the state of objects, interaction based testing makes assertions about how objects interact.</span></span> <span data-ttu-id="442fe-493">Пример:</span><span class="sxs-lookup"><span data-stu-id="442fe-493">For example:</span></span>

-   <span data-ttu-id="442fe-494">Проверьте, вызывает ли контроллер метод Add репозитория при выполнении инструкции CREATE.</span><span class="sxs-lookup"><span data-stu-id="442fe-494">Verify the controller invokes the repository’s Add method when Create executes.</span></span>
-   <span data-ttu-id="442fe-495">Убедитесь, что контроллер вызывает метод FindAll репозитория при выполнении индекса.</span><span class="sxs-lookup"><span data-stu-id="442fe-495">Verify the controller invokes the repository’s FindAll method when Index executes.</span></span>
-   <span data-ttu-id="442fe-496">Убедитесь, что контроллер вызывает метод фиксации единицы работы, чтобы сохранить изменения при выполнении команды Edit.</span><span class="sxs-lookup"><span data-stu-id="442fe-496">Verify the controller invokes the unit of work’s Commit method to save changes when Edit executes.</span></span>

<span data-ttu-id="442fe-497">Тестирование взаимодействия часто требует меньше тестовых данных, так как мы не знакомство в коллекциях и не проверяя счетчики.</span><span class="sxs-lookup"><span data-stu-id="442fe-497">Interaction testing often requires less test data, because we aren’t poking inside of collections and verifying counts.</span></span> <span data-ttu-id="442fe-498">Например, если известно, что действие Details вызывает метод Финдбид репозитория с правильным значением, то действие, вероятно, работает правильно.</span><span class="sxs-lookup"><span data-stu-id="442fe-498">For example, if we know the Details action invokes a repository’s FindById method with the correct value - then the action is probably behaving correctly.</span></span> <span data-ttu-id="442fe-499">Это поведение можно проверить без настройки тестовых данных, возвращаемых из Финдбид.</span><span class="sxs-lookup"><span data-stu-id="442fe-499">We can verify this behavior without setting up any test data to return from FindById.</span></span>

``` csharp
    [TestClass]
    public class EmployeeControllerDetailsActionTests
               : EmployeeControllerTestBase {
         // ...
        [TestMethod]
        public void ShouldInvokeRepositoryToFindEmployee() {
            var result = _controller.Details(_detailsId);
            _repository.Verify(r => r.FindById(_detailsId));
        }
        int _detailsId = 1;
    }
```

<span data-ttu-id="442fe-500">Единственная настройка, необходимая в вышеуказанном тесте, — это настройка, предоставляемая базовым классом.</span><span class="sxs-lookup"><span data-stu-id="442fe-500">The only setup required in the above test fixture is the setup provided by the base class.</span></span> <span data-ttu-id="442fe-501">При вызове действия контроллера MOQ будет записывать взаимодействия с макетом репозитория.</span><span class="sxs-lookup"><span data-stu-id="442fe-501">When we invoke the controller action, Moq will record the interactions with the mock repository.</span></span> <span data-ttu-id="442fe-502">С помощью API проверки MOQ можно запросить MOQ, если контроллер вызвал Финдбид с правильным значением идентификатора.</span><span class="sxs-lookup"><span data-stu-id="442fe-502">Using the Verify API of Moq, we can ask Moq if the controller invoked FindById with the proper ID value.</span></span> <span data-ttu-id="442fe-503">Если контроллер не вызвал метод или вызвал метод с непредвиденным значением параметра, метод Verify выдаст исключение, и тест завершится ошибкой.</span><span class="sxs-lookup"><span data-stu-id="442fe-503">If the controller did not invoke the method, or invoked the method with an unexpected parameter value, the Verify method will throw an exception and the test will fail.</span></span>

<span data-ttu-id="442fe-504">Ниже приведен еще один пример проверки того, что действие Create вызывает фиксацию в текущей единице работы.</span><span class="sxs-lookup"><span data-stu-id="442fe-504">Here is another example to verify the Create action invokes Commit on the current unit of work.</span></span>

``` csharp
    [TestMethod]
    public void ShouldCommitUnitOfWork() {
        _controller.Create(_newEmployee);
        _unitOfWork.Verify(u => u.Commit());
    }
```

<span data-ttu-id="442fe-505">Одна опасность, связанная с тестированием взаимодействия, — это тенденция для определения взаимодействий.</span><span class="sxs-lookup"><span data-stu-id="442fe-505">One danger with interaction testing is the tendency to over specify interactions.</span></span> <span data-ttu-id="442fe-506">Возможность макета объекта записывать и проверять каждое взаимодействие с макетом объекта не означает, что тест должен попытаться проверить каждое взаимодействие.</span><span class="sxs-lookup"><span data-stu-id="442fe-506">The ability of the mock object to record and verify every interaction with the mock object doesn’t mean the test should try to verify every interaction.</span></span> <span data-ttu-id="442fe-507">Некоторые взаимодействия представляют собой детали реализации, поэтому необходимо только проверить взаимодействия, *необходимые* для удовлетворения текущего теста.</span><span class="sxs-lookup"><span data-stu-id="442fe-507">Some interactions are implementation details and you should only verify the interactions *required* to satisfy the current test.</span></span>

<span data-ttu-id="442fe-508">Выбор между макетами или имитациями во многом зависит от тестируемой системы и личных настроек (или команды).</span><span class="sxs-lookup"><span data-stu-id="442fe-508">The choice between mocks or fakes largely depends on the system you are testing and your personal (or team) preferences.</span></span> <span data-ttu-id="442fe-509">Макетные объекты могут радикально уменьшить объем кода, необходимого для реализации тестовых значений Double, но не все это удобно в программировании и проверки взаимодействий.</span><span class="sxs-lookup"><span data-stu-id="442fe-509">Mock objects can drastically reduce the amount of code you need to implement test doubles, but not everyone is comfortable programming expectations and verifying interactions.</span></span>

## <a name="conclusions"></a><span data-ttu-id="442fe-510">Выводы</span><span class="sxs-lookup"><span data-stu-id="442fe-510">Conclusions</span></span>

<span data-ttu-id="442fe-511">В этом документе мы рассмотрели несколько подходов к созданию тестируемого кода при использовании Entity Framework ADO.NET для сохраняемости данных.</span><span class="sxs-lookup"><span data-stu-id="442fe-511">In this paper we’ve demonstrated several approaches to creating testable code while using the ADO.NET Entity Framework for data persistence.</span></span> <span data-ttu-id="442fe-512">Мы можем использовать встроенные абстракции, такие как Иобжектсет @ no__t-0T @ no__t-1, или создавать собственные абстракции, такие как IRepository @ no__t-2T @ no__t-3.</span><span class="sxs-lookup"><span data-stu-id="442fe-512">We can leverage built in abstractions like IObjectSet&lt;T&gt;, or create our own abstractions like IRepository&lt;T&gt;.</span></span><span data-ttu-id="442fe-513">  В обоих случаях поддержка POCO в ADO.NET Entity Framework 4,0 позволяет потребителям этих абстракций оставаться постоянной игнорирующих и высоко тестироваться.</span><span class="sxs-lookup"><span data-stu-id="442fe-513">  In both cases, the POCO support in the ADO.NET Entity Framework 4.0 allows the consumers of these abstractions to remain persistent ignorant and highly testable.</span></span> <span data-ttu-id="442fe-514">Дополнительные функции EF4, такие как неявная неактивная Загрузка, позволяют бизнес-приложениям и коду службы приложений работать, не беспокоясь о деталях реляционного хранилища данных.</span><span class="sxs-lookup"><span data-stu-id="442fe-514">Additional EF4 features like implicit lazy loading allows business and application service code to work without worrying about the details of a relational data store.</span></span> <span data-ttu-id="442fe-515">Наконец, создаваемые абстракции просты в макете или подмышлении в модульных тестах, и мы можем использовать эти тестовые удвоения для быстрого выполнения, высокой изолированности и надежности тестов.</span><span class="sxs-lookup"><span data-stu-id="442fe-515">Finally, the abstractions we create are easy to mock or fake inside of unit tests, and we can use these test doubles to achieve fast running, highly isolated, and reliable tests.</span></span>

### <a name="additional-resources"></a><span data-ttu-id="442fe-516">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="442fe-516">Additional Resources</span></span>

-   <span data-ttu-id="442fe-517">Роберт C. Мартен, " [принцип единой ответственности](https://www.objectmentor.com/resources/articles/srp.pdf)"</span><span class="sxs-lookup"><span data-stu-id="442fe-517">Robert C. Martin, “ [The Single Responsibility Principle](https://www.objectmentor.com/resources/articles/srp.pdf)”</span></span>
-   <span data-ttu-id="442fe-518">Мартен Фаулер, [Каталог шаблонов](https://www.martinfowler.com/eaaCatalog/index.html) из *шаблонов архитектуры корпоративных приложений*</span><span class="sxs-lookup"><span data-stu-id="442fe-518">Martin Fowler, [Catalog of Patterns](https://www.martinfowler.com/eaaCatalog/index.html) from *Patterns of Enterprise Application Architecture*</span></span>
-   <span data-ttu-id="442fe-519">Гриффин Каприо, « [внедрение зависимостей](https://msdn.microsoft.com/magazine/cc163739.aspx)»</span><span class="sxs-lookup"><span data-stu-id="442fe-519">Griffin Caprio, “ [Dependency Injection](https://msdn.microsoft.com/magazine/cc163739.aspx)”</span></span>
-   <span data-ttu-id="442fe-520">Блог по программированию данных "@no__t 0Walkthrough: Разработка на основе тестирования с помощью Entity Framework 4.0 @ no__t-0 ".</span><span class="sxs-lookup"><span data-stu-id="442fe-520">Data Programmability Blog, “ [Walkthrough: Test Driven Development with the Entity Framework 4.0](https://blogs.msdn.com/adonet/pages/walkthrough-test-driven-development-with-the-entity-framework-4-0.aspx)”.</span></span>
-   <span data-ttu-id="442fe-521">Блог по программированию данных: [Использование шаблонов репозитория и единиц работы с Entity Framework 4,0](https://blogs.msdn.com/adonet/archive/2009/06/16/using-repository-and-unit-of-work-patterns-with-entity-framework-4-0.aspx)»</span><span class="sxs-lookup"><span data-stu-id="442fe-521">Data Programmability Blog, “ [Using Repository and Unit of Work patterns with Entity Framework 4.0](https://blogs.msdn.com/adonet/archive/2009/06/16/using-repository-and-unit-of-work-patterns-with-entity-framework-4-0.aspx)”</span></span>
-   <span data-ttu-id="442fe-522">Аарон Йенсен, " [Введение в спецификации машин](http://codebetter.com/blogs/aaron.jensen/archive/2008/05/08/introducing-machine-specifications-or-mspec-for-short.aspx)"</span><span class="sxs-lookup"><span data-stu-id="442fe-522">Aaron Jensen, “ [Introducing Machine Specifications](http://codebetter.com/blogs/aaron.jensen/archive/2008/05/08/introducing-machine-specifications-or-mspec-for-short.aspx)”</span></span>
-   <span data-ttu-id="442fe-523">«Иванов», « [BDD с MSTest](https://blogs.msdn.com/elee/archive/2009/01/20/bdd-with-mstest.aspx)»</span><span class="sxs-lookup"><span data-stu-id="442fe-523">Eric Lee, “ [BDD with MSTest](https://blogs.msdn.com/elee/archive/2009/01/20/bdd-with-mstest.aspx)”</span></span>
-   <span data-ttu-id="442fe-524">«Эванс», « [Разработка на основе домена](https://books.google.com/books?id=7dlaMs0SECsC&printsec=frontcover&dq=evans%20domain%20driven%20design&hl=en&ei=cHztS6C8KIaglAfA_dS1CA&sa=X&oi=book_result&ct=result&resnum=1&ved=0CCoQ6AEwAA)»</span><span class="sxs-lookup"><span data-stu-id="442fe-524">Eric Evans, “ [Domain Driven Design](https://books.google.com/books?id=7dlaMs0SECsC&printsec=frontcover&dq=evans%20domain%20driven%20design&hl=en&ei=cHztS6C8KIaglAfA_dS1CA&sa=X&oi=book_result&ct=result&resnum=1&ved=0CCoQ6AEwAA)”</span></span>
-   <span data-ttu-id="442fe-525">Мартен Фаулер, " [макеты не являются заглушками](https://martinfowler.com/articles/mocksArentStubs.html)"</span><span class="sxs-lookup"><span data-stu-id="442fe-525">Martin Fowler, “ [Mocks Aren’t Stubs](https://martinfowler.com/articles/mocksArentStubs.html)”</span></span>
-   <span data-ttu-id="442fe-526">Мартен Фаулер, " [Test Double](https://martinfowler.com/bliki/TestDouble.html)"</span><span class="sxs-lookup"><span data-stu-id="442fe-526">Martin Fowler, “ [Test Double](https://martinfowler.com/bliki/TestDouble.html)”</span></span>
-   [<span data-ttu-id="442fe-527">MOQ</span><span class="sxs-lookup"><span data-stu-id="442fe-527">Moq</span></span>](https://code.google.com/p/moq/)

### <a name="biography"></a><span data-ttu-id="442fe-528">Текст биографии</span><span class="sxs-lookup"><span data-stu-id="442fe-528">Biography</span></span>

<span data-ttu-id="442fe-529">Скотт Аллен является участником технических специалистов по Pluralsight и основатель OdeToCode.com.</span><span class="sxs-lookup"><span data-stu-id="442fe-529">Scott Allen is a member of the technical staff at Pluralsight and the founder of OdeToCode.com.</span></span> <span data-ttu-id="442fe-530">В течение 15 лет коммерческой разработки программного обеспечения Скотт работал над решениями для всех, от 8-разрядных встраиваемых устройств до высокомасштабируемых веб-приложений ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="442fe-530">In 15 years of commercial software development, Scott has worked on solutions for everything from 8-bit embedded devices to highly scalable ASP.NET web applications.</span></span> <span data-ttu-id="442fe-531">Вы можете связаться с Скотт в своем блоге по адресу Одетокоде или в Twitter по адресу [https://twitter.com/OdeToCode](https://twitter.com/OdeToCode).</span><span class="sxs-lookup"><span data-stu-id="442fe-531">You can reach Scott on his blog at OdeToCode, or on Twitter at [https://twitter.com/OdeToCode](https://twitter.com/OdeToCode).</span></span>
