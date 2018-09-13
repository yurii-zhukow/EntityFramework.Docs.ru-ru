---
title: Пригодность для тестирования и Entity Framework 4.0
author: divega
ms.date: 10/23/2016
ms.assetid: 9430e2ab-261c-4e8e-8545-2ebc52d7a247
ms.openlocfilehash: 0ddf72ab46e2d67dc8a9cf75cbd40430352c5210
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/13/2018
ms.locfileid: "45490536"
---
# <a name="testability-and-entity-framework-40"></a><span data-ttu-id="2682c-102">Пригодность для тестирования и Entity Framework 4.0</span><span class="sxs-lookup"><span data-stu-id="2682c-102">Testability and Entity Framework 4.0</span></span>
<span data-ttu-id="2682c-103">Скотт Аллен</span><span class="sxs-lookup"><span data-stu-id="2682c-103">Scott Allen</span></span>

<span data-ttu-id="2682c-104">Опубликовано: Май 2010 г.</span><span class="sxs-lookup"><span data-stu-id="2682c-104">Published: May 2010</span></span>

## <a name="introduction"></a><span data-ttu-id="2682c-105">Вступление</span><span class="sxs-lookup"><span data-stu-id="2682c-105">Introduction</span></span>

<span data-ttu-id="2682c-106">В этом техническом документе описывается и демонстрируется способ записи тестируемого кода с помощью ADO.NET Entity Framework 4.0 и Visual Studio 2010.</span><span class="sxs-lookup"><span data-stu-id="2682c-106">This white paper describes and demonstrates how to write testable code with the ADO.NET Entity Framework 4.0 and Visual Studio 2010.</span></span> <span data-ttu-id="2682c-107">В этом документе не пытается сосредоточиться на конкретных тестирования методологии разработки через тестирование (TDD) или разработки на основе поведения (BDD).</span><span class="sxs-lookup"><span data-stu-id="2682c-107">This paper does not try to focus on a specific testing methodology, like test-driven design (TDD) or behavior-driven design (BDD).</span></span> <span data-ttu-id="2682c-108">Вместо этого в этом документе основное внимание уделяется как написать код, который использует ADO.NET Entity Framework, но остается легко изолировать и проверить в автоматическом режиме.</span><span class="sxs-lookup"><span data-stu-id="2682c-108">Instead this paper will focus on how to write code that uses the ADO.NET Entity Framework yet remains easy to isolate and test in an automated fashion.</span></span> <span data-ttu-id="2682c-109">Мы рассмотрим распространенные шаблоны разработки, упростить тестирование в данных сценариях доступа и применение этих шаблонов, при использовании платформы.</span><span class="sxs-lookup"><span data-stu-id="2682c-109">We’ll look at common design patterns that facilitate testing in data access scenarios and see how to apply those patterns when using the framework.</span></span> <span data-ttu-id="2682c-110">Мы также рассмотрим особенности платформы framework, чтобы увидеть работу этих возможностей в тестируемого кода.</span><span class="sxs-lookup"><span data-stu-id="2682c-110">We’ll also look at specific features of the framework to see how those features can work in testable code.</span></span>

## <a name="what-is-testable-code"></a><span data-ttu-id="2682c-111">Что такое тестируемого кода?</span><span class="sxs-lookup"><span data-stu-id="2682c-111">What Is Testable Code?</span></span>

<span data-ttu-id="2682c-112">Возможность проверки компонента с помощью автоматических модульных тестов по предоставляет множество преимуществ нежелательно.</span><span class="sxs-lookup"><span data-stu-id="2682c-112">The ability to verify a piece of software using automated unit tests offers many desirable benefits.</span></span> <span data-ttu-id="2682c-113">Все знают, что хорошие тесты сократит число дефектов программного обеспечения в приложения и увеличение, качество приложения -, однако наличие модульных тестов в месте далеко выходит за рамки, просто обнаружения ошибок.</span><span class="sxs-lookup"><span data-stu-id="2682c-113">Everyone knows that good tests will reduce the number of software defects in an application and increase the application’s quality - but having unit tests in place goes far beyond just finding bugs.</span></span>

<span data-ttu-id="2682c-114">Набор хороших модульных тестов позволяет группе разработчиков для экономии времени и сохраняете контроль над создаваемое ими программное обеспечение.</span><span class="sxs-lookup"><span data-stu-id="2682c-114">A good unit test suite allows a development team to save time and remain in control of the software they create.</span></span> <span data-ttu-id="2682c-115">Команды можно внести изменения существующего кода, рефакторинг и переработки и реструктуризации программного обеспечения к новым требованиям и добавить новые компоненты в приложение при этом знать набор тестов можно проверить поведение приложения.</span><span class="sxs-lookup"><span data-stu-id="2682c-115">A team can make changes to existing code, refactor, redesign, and restructure software to meet new requirements, and add new components into an application all while knowing the test suite can verify the application’s behavior.</span></span> <span data-ttu-id="2682c-116">Модульные тесты являются частью цикла быструю обратную связь для упрощения изменений и сохранить удобства обслуживания программного обеспечения, при увеличении сложности.</span><span class="sxs-lookup"><span data-stu-id="2682c-116">Unit tests are part of a quick feedback cycle to facilitate change and preserve the maintainability of software as complexity increases.</span></span>

<span data-ttu-id="2682c-117">Модульное тестирование в состав цену, тем не менее.</span><span class="sxs-lookup"><span data-stu-id="2682c-117">Unit testing comes with a price, however.</span></span> <span data-ttu-id="2682c-118">Проектной группе приходится вкладывать время для создания и обслуживания модульных тестов.</span><span class="sxs-lookup"><span data-stu-id="2682c-118">A team has to invest the time to create and maintain unit tests.</span></span> <span data-ttu-id="2682c-119">Трудозатраты, необходимые для создания этих тестов непосредственно связано с **пригодности для тестирования** базового программного обеспечения.</span><span class="sxs-lookup"><span data-stu-id="2682c-119">The amount of effort required to create these tests is directly related to the **testability** of the underlying software.</span></span> <span data-ttu-id="2682c-120">Насколько просто программное обеспечение для тестирования?</span><span class="sxs-lookup"><span data-stu-id="2682c-120">How easy is the software to test?</span></span> <span data-ttu-id="2682c-121">Группы разработки программного обеспечения учитывались возможности тестирования эффективных тестов создается быстрее, чем группа, работающая с нетестируемого программного обеспечения.</span><span class="sxs-lookup"><span data-stu-id="2682c-121">A team designing software with testability in mind will create effective tests faster than the team working with un-testable software.</span></span>

<span data-ttu-id="2682c-122">Корпорация Microsoft разработала ADO.NET Entity Framework 4.0 (EF4) учитывались возможности тестирования.</span><span class="sxs-lookup"><span data-stu-id="2682c-122">Microsoft designed the ADO.NET Entity Framework 4.0 (EF4) with testability in mind.</span></span> <span data-ttu-id="2682c-123">Это не означает, что разработчики будет записывать модульные тесты для сам код framework.</span><span class="sxs-lookup"><span data-stu-id="2682c-123">This doesn’t mean developers will be writing unit tests against framework code itself.</span></span> <span data-ttu-id="2682c-124">Вместо этого цели тестирования EF4 упрощают создание тестируемого кода, который создает на основе framework.</span><span class="sxs-lookup"><span data-stu-id="2682c-124">Instead, the testability goals for EF4 make it easy to create testable code that builds on top of the framework.</span></span> <span data-ttu-id="2682c-125">Прежде чем мы рассмотрим конкретные примеры, стоит понять качества кода для тестирования.</span><span class="sxs-lookup"><span data-stu-id="2682c-125">Before we look at specific examples, it’s worthwhile to understand the qualities of testable code.</span></span>

### <a name="the-qualities-of-testable-code"></a><span data-ttu-id="2682c-126">Качество тестируемого кода</span><span class="sxs-lookup"><span data-stu-id="2682c-126">The Qualities of Testable Code</span></span>

<span data-ttu-id="2682c-127">Код, который легко тестировать всегда будет обладать по крайней мере два признаков.</span><span class="sxs-lookup"><span data-stu-id="2682c-127">Code that is easy to test will always exhibit at least two traits.</span></span> <span data-ttu-id="2682c-128">Во-первых, пригодного для тестирования кода легко **наблюдать за**.</span><span class="sxs-lookup"><span data-stu-id="2682c-128">First, testable code is easy to **observe**.</span></span> <span data-ttu-id="2682c-129">Учитывая некоторый набор входных данных, должно быть легко наблюдать за результат выполнения кода.</span><span class="sxs-lookup"><span data-stu-id="2682c-129">Given some set of inputs, it should be easy to observe the output of the code.</span></span> <span data-ttu-id="2682c-130">Например тестирование следующий метод несложно, так как метод напрямую возвращает результат вычисления.</span><span class="sxs-lookup"><span data-stu-id="2682c-130">For example, testing the following method is easy because the method directly returns the result of a calculation.</span></span>

``` csharp
    public int Add(int x, int y) {
        return x + y;
    }
```

<span data-ttu-id="2682c-131">Тестирование метода является сложной задачей, если метод записывает вычисляемое значение в к сетевому сокету, таблицы базы данных или файла аналогично следующему коду.</span><span class="sxs-lookup"><span data-stu-id="2682c-131">Testing a method is difficult if the method writes the computed value into a network socket, a database table, or a file like the following code.</span></span> <span data-ttu-id="2682c-132">Тест должен выполнить дополнительную работу, чтобы получить значение.</span><span class="sxs-lookup"><span data-stu-id="2682c-132">The test has to perform additional work to retrieve the value.</span></span>

``` csharp
    public void AddAndSaveToFile(int x, int y) {
         var results = string.Format("The answer is {0}", x + y);
         File.WriteAllText("results.txt", results);
    }
```

<span data-ttu-id="2682c-133">Во-вторых, тестируемого кода легко **изолировать**.</span><span class="sxs-lookup"><span data-stu-id="2682c-133">Secondly, testable code is easy to **isolate**.</span></span> <span data-ttu-id="2682c-134">Давайте используем следующий псевдокод неверный пример кода для тестирования.</span><span class="sxs-lookup"><span data-stu-id="2682c-134">Let’s use the following pseudo-code as a bad example of testable code.</span></span>

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

<span data-ttu-id="2682c-135">Метод легко наблюдать за — мы можно передать в страховой полис и убедитесь, что возвращаемое значение совпадает с ожидаемым результатом.</span><span class="sxs-lookup"><span data-stu-id="2682c-135">The method is easy to observe – we can pass in an insurance policy and verify the return value matches an expected result.</span></span> <span data-ttu-id="2682c-136">Тем не менее для тестирования метода необходимо установить базу данных с помощью правильную схему и настроить SMTP-сервера в случае, если метод пытается отправить сообщение электронной почты.</span><span class="sxs-lookup"><span data-stu-id="2682c-136">However, to test the method we’ll need to have a database installed with the correct schema, and configure the SMTP server in case the method tries to send an email.</span></span>

<span data-ttu-id="2682c-137">Только хочет проверить логику вычислений внутри метода модульного теста, но тест может завершиться ошибкой, поскольку серверу электронной почты находится в автономном режиме, или из-за перемещения сервера базы данных.</span><span class="sxs-lookup"><span data-stu-id="2682c-137">The unit test only wants to verify the calculation logic inside the method, but the test might fail because the email server is offline, or because the database server moved.</span></span> <span data-ttu-id="2682c-138">Оба эти ошибки связаны с поведение, которое желает проверить теста.</span><span class="sxs-lookup"><span data-stu-id="2682c-138">Both of these failures are unrelated to the behavior the test wants to verify.</span></span> <span data-ttu-id="2682c-139">Поведение трудно локализовать.</span><span class="sxs-lookup"><span data-stu-id="2682c-139">The behavior is difficult to isolate.</span></span>

<span data-ttu-id="2682c-140">Разработчики программного обеспечения, стремиться к созданию кода для тестирования часто стремятся Ведение разделения проблем в коде, что они записывают.</span><span class="sxs-lookup"><span data-stu-id="2682c-140">Software developers who strive to write testable code often strive to maintain a separation of concerns in the code they write.</span></span> <span data-ttu-id="2682c-141">Приведенный выше метод необходимо сосредоточиться на бизнес-расчетов и делегировать детали реализации базы данных и адрес электронной почты для других компонентов.</span><span class="sxs-lookup"><span data-stu-id="2682c-141">The above method should focus on the business calculations and delegate the database and email implementation details to other components.</span></span> <span data-ttu-id="2682c-142">Роберт с. Мартин вызывает этот принципа персональной ответственности.</span><span class="sxs-lookup"><span data-stu-id="2682c-142">Robert C. Martin calls this the Single Responsibility Principle.</span></span> <span data-ttu-id="2682c-143">Объект следует инкапсулировать один узкий ответственности, скажем, расчет значение политики.</span><span class="sxs-lookup"><span data-stu-id="2682c-143">An object should encapsulate a single, narrow responsibility, like calculating the value of a policy.</span></span> <span data-ttu-id="2682c-144">Вся остальная работа базы данных и уведомления должно быть ответственность за некоторому другому объекту.</span><span class="sxs-lookup"><span data-stu-id="2682c-144">All other database and notification work should be the responsibility of some other object.</span></span> <span data-ttu-id="2682c-145">Код, написанный таким образом, проще ее изолировать, так как она сосредоточена на одну задачу.</span><span class="sxs-lookup"><span data-stu-id="2682c-145">Code written in this fashion is easier to isolate because it is focused on a single task.</span></span>

<span data-ttu-id="2682c-146">В .NET есть абстракции, нам нужно следовать принципа персональной ответственности и добиться изоляции.</span><span class="sxs-lookup"><span data-stu-id="2682c-146">In .NET we have the abstractions we need to follow the Single Responsibility Principle and achieve isolation.</span></span> <span data-ttu-id="2682c-147">Можно использовать интерфейс определения и принудительного кода, чтобы использовать абстракции интерфейс вместо конкретного типа.</span><span class="sxs-lookup"><span data-stu-id="2682c-147">We can use interface definitions and force the code to use the interface abstraction instead of a concrete type.</span></span> <span data-ttu-id="2682c-148">Далее в этом документе мы рассмотрим, как метод как неправильный приведенного выше примера можно работать с интерфейсы, *выглядеть* , как они будут обмениваться данными к базе данных.</span><span class="sxs-lookup"><span data-stu-id="2682c-148">Later in this paper we’ll see how a method like the bad example presented above can work with interfaces that *look* like they will talk to the database.</span></span> <span data-ttu-id="2682c-149">Во время тестирования мы может заменить фиктивные реализация, которая не обращаться к базе данных, но вместо этого хранит данные в памяти.</span><span class="sxs-lookup"><span data-stu-id="2682c-149">At test time, however, we can substitute a dummy implementation that doesn’t talk to the database but instead holds data in memory.</span></span> <span data-ttu-id="2682c-150">Это фиктивная реализация будет изолировать код из несвязанных проблем в модуле кода доступа к данным или базы данных конфигурации.</span><span class="sxs-lookup"><span data-stu-id="2682c-150">This dummy implementation will isolate the code from unrelated problems in the data access code or database configuration.</span></span>

<span data-ttu-id="2682c-151">Существуют дополнительные преимущества изоляции.</span><span class="sxs-lookup"><span data-stu-id="2682c-151">There are additional benefits to isolation.</span></span> <span data-ttu-id="2682c-152">Бизнес-расчета в последний метод обычно занимает лишь несколько миллисекунд для выполнения, но сам тест могут работать на несколько секунд в качестве кода прыжков в сети и взаимодействует с различными серверами.</span><span class="sxs-lookup"><span data-stu-id="2682c-152">The business calculation in the last method should only take a few milliseconds to execute, but the test itself might run for several seconds as the code hops around the network and talks to various servers.</span></span> <span data-ttu-id="2682c-153">Модульные тесты должны выполняться fast, чтобы упростить небольшие изменения.</span><span class="sxs-lookup"><span data-stu-id="2682c-153">Unit tests should run fast to facilitate small changes.</span></span> <span data-ttu-id="2682c-154">Модульные тесты должны также быть повторяемым и не сбоем, так как возникла проблема с компонентом, не имеющих отношения к теста.</span><span class="sxs-lookup"><span data-stu-id="2682c-154">Unit tests should also be repeatable and not fail because a component unrelated to the test has a problem.</span></span> <span data-ttu-id="2682c-155">Создание кода, легко наблюдать и изолировать означает, что разработчики получают это облегчит написание тестов для кода, тратить меньше времени на ожидание исполнение тестов и более важно, тратят меньше времени, ошибок, которые не существуют.</span><span class="sxs-lookup"><span data-stu-id="2682c-155">Writing code that is easy to observe and to isolate means developers will have an easier time writing tests for the code, spend less time waiting for tests to execute, and more importantly, spend less time tracking down bugs that do not exist.</span></span>

<span data-ttu-id="2682c-156">Будем надеяться, что можно оценить преимущества тестирования и понять качеств, которые видно, тестируемого кода.</span><span class="sxs-lookup"><span data-stu-id="2682c-156">Hopefully you can appreciate the benefits of testing and understand the qualities that testable code exhibits.</span></span> <span data-ttu-id="2682c-157">Мы должны решить, как написать код, который работает с помощью EF4 для сохранения данных в базу данных, сохраняя при этом наблюдаемых и легко изолировать, но сначала мы будем сузить основное внимание обсудить тестируемые архитектуры для доступа к данным.</span><span class="sxs-lookup"><span data-stu-id="2682c-157">We are about to address how to write code that works with EF4 to save data into a database while remaining observable and easy to isolate, but first we’ll narrow our focus to discuss testable designs for data access.</span></span>

## <a name="design-patterns-for-data-persistence"></a><span data-ttu-id="2682c-158">Шаблоны разработки для постоянного хранения</span><span class="sxs-lookup"><span data-stu-id="2682c-158">Design Patterns for Data Persistence</span></span>

<span data-ttu-id="2682c-159">В обоих примерах неправильный, представленного выше было слишком большого количества обязанностей.</span><span class="sxs-lookup"><span data-stu-id="2682c-159">Both of the bad examples presented earlier had too many responsibilities.</span></span> <span data-ttu-id="2682c-160">Первый пример неправильный пришлось выполнить расчет *и* записи в файл.</span><span class="sxs-lookup"><span data-stu-id="2682c-160">The first bad example had to perform a calculation *and* write to a file.</span></span> <span data-ttu-id="2682c-161">Во втором примере неправильный требовалось считывать данные из базы данных *и* выполнения расчетов *и* Отправка сообщения электронной почты.</span><span class="sxs-lookup"><span data-stu-id="2682c-161">The second bad example had to read data from a database *and* perform a business calculation *and* send email.</span></span> <span data-ttu-id="2682c-162">Создав небольших методов, которые разделения функций и делегировать ответственность за другими компонентами вносятся большой шаг вперед для написания тестируемого кода.</span><span class="sxs-lookup"><span data-stu-id="2682c-162">By designing smaller methods that separate concerns and delegate responsibility to other components you’ll make great strides towards writing testable code.</span></span> <span data-ttu-id="2682c-163">Целью является построение функциональные возможности путем составления действий с небольшим и направленным абстракции.</span><span class="sxs-lookup"><span data-stu-id="2682c-163">The goal is to build functionality by composing actions from small and focused abstractions.</span></span>

<span data-ttu-id="2682c-164">Когда дело доходит до постоянного хранения небольшую и фокусом абстракции, которые мы ищем настолько распространено, они были задокументированы как шаблоны проектирования.</span><span class="sxs-lookup"><span data-stu-id="2682c-164">When it comes to data persistence the small and focused abstractions we are looking for are so common they’ve been documented as design patterns.</span></span> <span data-ttu-id="2682c-165">Книги (Martin Fowler) Patterns of Enterprise Application Architecture была первой работой для описания этих шаблонов в печати.</span><span class="sxs-lookup"><span data-stu-id="2682c-165">Martin Fowler’s book Patterns of Enterprise Application Architecture was the first work to describe these patterns in print.</span></span> <span data-ttu-id="2682c-166">Прежде чем мы покажем, как эти платформы ADO.NET Entity Framework реализует и работает с этими шаблонами, мы предоставим краткое описание этих шаблонов в следующих разделах.</span><span class="sxs-lookup"><span data-stu-id="2682c-166">We’ll provide a brief description of these patterns in the following sections before we show how these ADO.NET Entity Framework implements and works with these patterns.</span></span>

### <a name="the-repository-pattern"></a><span data-ttu-id="2682c-167">Шаблон репозитория</span><span class="sxs-lookup"><span data-stu-id="2682c-167">The Repository Pattern</span></span>

<span data-ttu-id="2682c-168">Фаулер, — говорит репозиторием «является посредником между доменом и данных уровней сопоставления, с помощью интерфейса обобщенное для доступа к объектам домена».</span><span class="sxs-lookup"><span data-stu-id="2682c-168">Fowler says a repository “mediates between the domain and data mapping layers using a collection-like interface for accessing domain objects”.</span></span> <span data-ttu-id="2682c-169">Шаблон репозитория призвано изолировать код из рутинной работе доступа к данным, и как мы видели ранее изоляции является обязательным характерной для тестирования.</span><span class="sxs-lookup"><span data-stu-id="2682c-169">The goal of the repository pattern is to isolate code from the minutiae of data access, and as we saw earlier isolation is a required trait for testability.</span></span>

<span data-ttu-id="2682c-170">Ключом к изоляции является, как хранилище предоставляет объекты, с помощью интерфейса коллекции по принципу.</span><span class="sxs-lookup"><span data-stu-id="2682c-170">The key to the isolation is how the repository exposes objects using a collection-like interface.</span></span> <span data-ttu-id="2682c-171">Записи можно использовать хранилище имеет ни малейшего представления как логика репозитории материализуется объекты, которые можно запросить.</span><span class="sxs-lookup"><span data-stu-id="2682c-171">The logic you write to use the repository has no idea how the repository will materialize the objects you request.</span></span> <span data-ttu-id="2682c-172">Хранилище может взаимодействовать с базой данных, или он может просто возвращает объекты из коллекции в памяти.</span><span class="sxs-lookup"><span data-stu-id="2682c-172">The repository might talk to a database, or it might just return objects from an in-memory collection.</span></span> <span data-ttu-id="2682c-173">Ваш код должен знать всего лишь репозитории отображается для обслуживания коллекции, что можно получить, добавление и удаление объектов из коллекции.</span><span class="sxs-lookup"><span data-stu-id="2682c-173">All your code needs to know is that the repository appears to maintain the collection, and you can retrieve, add, and delete objects from the collection.</span></span>

<span data-ttu-id="2682c-174">Конкретный репозиторий в существующих приложений .NET часто наследуется от универсального интерфейса следующим образом:</span><span class="sxs-lookup"><span data-stu-id="2682c-174">In existing .NET applications a concrete repository often inherits from a generic interface like the following:</span></span>

``` csharp
    public interface IRepository<T> {       
        IEnumerable<T> FindAll();
        IEnumerable<T> FindBy(Expression<Func\<T, bool>> predicate);
        T FindById(int id);
        void Add(T newEntity);
        void Remove(T entity);
    }
```

<span data-ttu-id="2682c-175">Сделаем некоторые изменения в определение интерфейса мы предоставлять реализацию для EF4, когда основной принцип остается неизменным.</span><span class="sxs-lookup"><span data-stu-id="2682c-175">We’ll make a few changes to the interface definition when we provide an implementation for EF4, but the basic concept remains the same.</span></span> <span data-ttu-id="2682c-176">Код с помощью репозиторием конкретные реализации этого интерфейса можно получить сущность по значению первичного ключа, для получения коллекции сущностей, по результатам вычисления предиката, или просто получить все доступные сущности.</span><span class="sxs-lookup"><span data-stu-id="2682c-176">Code can use a concrete repository implementing this interface to retrieve an entity by its primary key value, to retrieve a collection of entities based on the evaluation of a predicate, or simply retrieve all available entities.</span></span> <span data-ttu-id="2682c-177">Код можно также добавлять и удалять сущности через интерфейс репозитория.</span><span class="sxs-lookup"><span data-stu-id="2682c-177">The code can also add and remove entities through the repository interface.</span></span>

<span data-ttu-id="2682c-178">Заданных объектов IRepository Employee, код может выполнить следующие операции.</span><span class="sxs-lookup"><span data-stu-id="2682c-178">Given an IRepository of Employee objects, code can perform the following operations.</span></span>

``` csharp
    var employeesNamedScott =
        repository
            .FindBy(e => e.Name == "Scott")
            .OrderBy(e => e.HireDate);
    var firstEmployee = repository.FindById(1);
    var newEmployee = new Employee() {/*... */};
    repository.Add(newEmployee);
```

<span data-ttu-id="2682c-179">Поскольку код использует интерфейс (IRepository сотрудника), мы можем предоставить код с помощью различных реализаций интерфейса.</span><span class="sxs-lookup"><span data-stu-id="2682c-179">Since the code is using an interface (IRepository of Employee), we can provide the code with different implementations of the interface.</span></span> <span data-ttu-id="2682c-180">Одна реализация может быть реализацию, предлагая EF4 и сохранение объектов в базу данных Microsoft SQL Server.</span><span class="sxs-lookup"><span data-stu-id="2682c-180">One implementation might be an implementation backed by EF4 and persisting objects into a Microsoft SQL Server database.</span></span> <span data-ttu-id="2682c-181">Другую реализацию (один, который используется во время тестирования) может быть подкреплен сотрудника из списка объектов в памяти.</span><span class="sxs-lookup"><span data-stu-id="2682c-181">A different implementation (one we use during testing) might be backed by an in-memory List of Employee objects.</span></span> <span data-ttu-id="2682c-182">Интерфейс поможет добиться изоляции в коде.</span><span class="sxs-lookup"><span data-stu-id="2682c-182">The interface will help to achieve isolation in the code.</span></span>

<span data-ttu-id="2682c-183">Обратите внимание, что IRepository&lt;T&gt; интерфейс не предоставляет операции сохранения.</span><span class="sxs-lookup"><span data-stu-id="2682c-183">Notice the IRepository&lt;T&gt; interface does not expose a Save operation.</span></span> <span data-ttu-id="2682c-184">Как обновить существующие объекты?</span><span class="sxs-lookup"><span data-stu-id="2682c-184">How do we update existing objects?</span></span> <span data-ttu-id="2682c-185">Часто встречаются IRepository определений, которые включают операции сохранения и реализации этих репозиториев необходимо немедленно сохранить объект в базу данных.</span><span class="sxs-lookup"><span data-stu-id="2682c-185">You might come across IRepository definitions that do include the Save operation, and implementations of these repositories will need to immediately persist an object into the database.</span></span> <span data-ttu-id="2682c-186">Тем не менее во многих приложениях мы не хотим сохранять объекты по отдельности.</span><span class="sxs-lookup"><span data-stu-id="2682c-186">However, in many applications we don’t want to persist objects individually.</span></span> <span data-ttu-id="2682c-187">Вместо этого мы хотим вдохните жизнь в объекты, возможно из разных репозиториев, изменить эти объекты как часть бизнес-деятельности и затем сохранить все объекты в рамках одной атомарной операции.</span><span class="sxs-lookup"><span data-stu-id="2682c-187">Instead, we want to bring objects to life, perhaps from different repositories, modify those objects as part of a business activity, and then persist all the objects as part of a single, atomic operation.</span></span> <span data-ttu-id="2682c-188">К счастью есть шаблон, чтобы разрешить этот тип поведения.</span><span class="sxs-lookup"><span data-stu-id="2682c-188">Fortunately, there is a pattern to allow this type of behavior.</span></span>

### <a name="the-unit-of-work-pattern"></a><span data-ttu-id="2682c-189">Шаблон единицы работы</span><span class="sxs-lookup"><span data-stu-id="2682c-189">The Unit of Work Pattern</span></span>

<span data-ttu-id="2682c-190">Фаулер, — говорит единица работы «сохранит список объектов зависит от бизнес-транзакции и координирует записи изменений и разрешение проблем параллелизма».</span><span class="sxs-lookup"><span data-stu-id="2682c-190">Fowler says a unit of work will “maintain a list of objects affected by a business transaction and coordinates the writing out of changes and the resolution of concurrency problems”.</span></span> <span data-ttu-id="2682c-191">Это ответственность за единицу работы для отслеживания изменений объектов мы воплотить в жизнь из репозитория и сохранять любые изменения, внесенных в объекты, когда сообщается о единицу работы для фиксации изменений.</span><span class="sxs-lookup"><span data-stu-id="2682c-191">It is the responsibility of the unit of work to track changes to the objects we bring to life from a repository and persist any changes we’ve made to the objects when we tell the unit of work to commit the changes.</span></span> <span data-ttu-id="2682c-192">Это также отвечают за единицу работы для новых объектов, мы добавили ко всем репозиториям и вставить объекты в базе данных, а также управление ими удаления.</span><span class="sxs-lookup"><span data-stu-id="2682c-192">It’s also the responsibility of the unit of work to take the new objects we’ve added to all repositories and insert the objects into a database, and also mange deletion.</span></span>

<span data-ttu-id="2682c-193">Если вы выполняли никакой работы с наборами данных ADO.NET затем вы будете уметь шаблон единицы работы.</span><span class="sxs-lookup"><span data-stu-id="2682c-193">If you’ve ever done any work with ADO.NET DataSets then you’ll already be familiar with the unit of work pattern.</span></span> <span data-ttu-id="2682c-194">Наборы данных ADO.NET имел возможность отслеживать наши обновления, удаления и вставки объектов DataRow и удалось (с помощью адаптера таблицы) выверить все изменения в базе данных.</span><span class="sxs-lookup"><span data-stu-id="2682c-194">ADO.NET DataSets had the ability to track our updates, deletions, and insertion of DataRow objects and could (with the help of a TableAdapter) reconcile all our changes to a database.</span></span> <span data-ttu-id="2682c-195">Тем не менее объекты DataSet модель отключенных подмножество основной базе данных.</span><span class="sxs-lookup"><span data-stu-id="2682c-195">However, DataSet objects model a disconnected subset of the underlying database.</span></span> <span data-ttu-id="2682c-196">Шаблон единицы работы выполняет то же поведение, но работает с бизнес-объекты и объекты домена, которые изолированы от кода доступа к данным и без базы данных.</span><span class="sxs-lookup"><span data-stu-id="2682c-196">The unit of work pattern exhibits the same behavior, but works with business objects and domain objects that are isolated from data access code and unaware of the database.</span></span>

<span data-ttu-id="2682c-197">Абстракция для моделирования единицу работы, в коде .NET может выглядеть следующим образом:</span><span class="sxs-lookup"><span data-stu-id="2682c-197">An abstraction to model the unit of work in .NET code might look like the following:</span></span>

``` csharp
    public interface IUnitOfWork {
        IRepository<Employee> Employees { get; }
        IRepository<Order> Orders { get; }
        IRepository<Customer> Customers { get; }
        void Commit();
    }
```

<span data-ttu-id="2682c-198">Предоставляя ссылки репозитория из единицу работы, можно обеспечить единый рабочий объект имеет возможность отслеживать все сущности, материализуется во время бизнес-транзакции.</span><span class="sxs-lookup"><span data-stu-id="2682c-198">By exposing repository references from the unit of work we can ensure a single unit of work object has the ability to track all entities materialized during a business transaction.</span></span> <span data-ttu-id="2682c-199">Метод Commit для реальных единицы работы реализуется все Магия для согласования изменений в памяти с базой данных.</span><span class="sxs-lookup"><span data-stu-id="2682c-199">The implementation of the Commit method for a real unit of work is where all the magic happens to reconcile in-memory changes with the database.</span></span> 

<span data-ttu-id="2682c-200">Получает ссылку на IUnitOfWork, код можно внести изменения в бизнес-объекты, полученные из одного или нескольких репозиториев и сохранить все изменения, с помощью атомарные операции фиксации.</span><span class="sxs-lookup"><span data-stu-id="2682c-200">Given an IUnitOfWork reference, code can make changes to business objects retrieved from one or more repositories and save all the changes using the atomic Commit operation.</span></span>

``` csharp
    var firstEmployee = unitofWork.Employees.FindById(1);
    var firstCustomer = unitofWork.Customers.FindById(1);
    firstEmployee.Name = "Alex";
    firstCustomer.Name = "Christopher";
    unitofWork.Commit();
```

### <a name="the-lazy-load-pattern"></a><span data-ttu-id="2682c-201">Шаблон отложенной загрузки</span><span class="sxs-lookup"><span data-stu-id="2682c-201">The Lazy Load Pattern</span></span>

<span data-ttu-id="2682c-202">Фаулер использует имя отложенной загрузки для описания «объект, который не содержит все данные, вам требуется, но знает, как его получить».</span><span class="sxs-lookup"><span data-stu-id="2682c-202">Fowler uses the name lazy load to describe “an object that doesn’t contain all of the data you need but knows how to get it”.</span></span> <span data-ttu-id="2682c-203">Прозрачный отложенная загрузка является важной характеристикой для после написания кода, пригодного для тестирования бизнеса и работа в реляционной базе данных.</span><span class="sxs-lookup"><span data-stu-id="2682c-203">Transparent lazy loading is an important feature to have when writing testable business code and working with a relational database.</span></span> <span data-ttu-id="2682c-204">Например рассмотрим следующий код.</span><span class="sxs-lookup"><span data-stu-id="2682c-204">As an example, consider the following code.</span></span>

``` csharp
    var employee = repository.FindById(id);
    // ... and later ...
    foreach(var timeCard in employee.TimeCards) {
        // .. manipulate the timeCard
    }
```

<span data-ttu-id="2682c-205">Как табелях коллекция заполняется?</span><span class="sxs-lookup"><span data-stu-id="2682c-205">How is the TimeCards collection populated?</span></span> <span data-ttu-id="2682c-206">Существует два возможных варианта ответа.</span><span class="sxs-lookup"><span data-stu-id="2682c-206">There are two possible answers.</span></span> <span data-ttu-id="2682c-207">Один ответ – что хранилище сотрудника, в ответ на запрос для получения сотрудника, выдает запрос для получения сотрудника вместе с сотрудника связанные сведения.</span><span class="sxs-lookup"><span data-stu-id="2682c-207">One answer is that the employee repository, when asked to fetch an employee, issues a query to retrieve both the employee along with the employee’s associated time card information.</span></span> <span data-ttu-id="2682c-208">В реляционных базах данных, это обычно требуется запрос с предложением JOIN и может повлечь за извлечение больше информации, чем приложения должен.</span><span class="sxs-lookup"><span data-stu-id="2682c-208">In relational databases this generally requires a query with a JOIN clause and may result in retrieving more information than an application needs.</span></span> <span data-ttu-id="2682c-209">Что делать, если приложение никогда не должно touch свойство табелях?</span><span class="sxs-lookup"><span data-stu-id="2682c-209">What if the application never needs to touch the TimeCards property?</span></span>

<span data-ttu-id="2682c-210">Ответ на второй вопрос — загрузка свойство табелях «по требованию».</span><span class="sxs-lookup"><span data-stu-id="2682c-210">A second answer is to load the TimeCards property “on demand”.</span></span> <span data-ttu-id="2682c-211">Эта отложенная загрузка прозрачно для бизнес-логики и неявные потому, что код не вызывает специальные API, чтобы получить сведения.</span><span class="sxs-lookup"><span data-stu-id="2682c-211">This lazy loading is implicit and transparent to the business logic because the code does not invoke special APIs to retrieve time card information.</span></span> <span data-ttu-id="2682c-212">В коде предполагается, что сведения о карте время присутствует, при необходимости.</span><span class="sxs-lookup"><span data-stu-id="2682c-212">The code assumes the time card information is present when needed.</span></span> <span data-ttu-id="2682c-213">Чудеса с отложенной загрузки, который обычно включает перехват вызовов методов среды выполнения.</span><span class="sxs-lookup"><span data-stu-id="2682c-213">There is some magic involved with lazy loading that generally involves runtime interception of method invocations.</span></span> <span data-ttu-id="2682c-214">Перехват кода несет ответственность за обращения к базе данных и получение сведений о времени карты оставив бизнес-логика может быть бизнес-логики.</span><span class="sxs-lookup"><span data-stu-id="2682c-214">The intercepting code is responsible for talking to the database and retrieving time card information while leaving the business logic free to be business logic.</span></span> <span data-ttu-id="2682c-215">Эти невероятные вещи отложенную загрузку позволяет бизнес-кода для изоляции сам с операциями получения данных и чему больший объем кода, пригодного для тестирования.</span><span class="sxs-lookup"><span data-stu-id="2682c-215">This lazy load magic allows the business code to isolate itself from data retrieval operations and results in more testable code.</span></span>

<span data-ttu-id="2682c-216">Недостатком вызывает отложенную загрузку является, когда приложение *does* необходимые сведения о карте времени, код будет выполняться дополнительный запрос.</span><span class="sxs-lookup"><span data-stu-id="2682c-216">The drawback to a lazy load is that when an application *does* need the time card information the code will execute an additional query.</span></span> <span data-ttu-id="2682c-217">Это не является проблемой для многих приложений, но для производительности уязвимых приложений или приложений цикла по несколько сотрудников объектов и выполнения запроса для получения карточек времени во время каждой итерации цикла (проблемы часто называют N + 1 задачи с запросами), отложенная загрузка — это перетаскивания.</span><span class="sxs-lookup"><span data-stu-id="2682c-217">This isn’t a concern for many applications, but for performance sensitive applications or applications looping through a number of employee objects and executing a query to retrieve time cards during each iteration of the loop (a problem often referred to as the N+1 query problem), lazy loading is a drag.</span></span> <span data-ttu-id="2682c-218">В этих сценариях приложение может потребоваться заблаговременной загрузки сведения в наиболее эффективным способом невозможно.</span><span class="sxs-lookup"><span data-stu-id="2682c-218">In these scenarios an application might want to eagerly load time card information in the most efficient manner possible.</span></span>

<span data-ttu-id="2682c-219">К счастью мы увидим, как EF4 поддерживает оба неявное отложенной загрузки и эффективный интенсивно загружается информация по мере перемещения в следующем разделе и реализации этих шаблонов.</span><span class="sxs-lookup"><span data-stu-id="2682c-219">Fortunately, we’ll see how EF4 supports both implicit lazy loads and efficient eager loads as we move into the next section and implement these patterns.</span></span>

## <a name="implementing-patterns-with-the-entity-framework"></a><span data-ttu-id="2682c-220">Реализация шаблонов с платформой Entity Framework</span><span class="sxs-lookup"><span data-stu-id="2682c-220">Implementing Patterns with the Entity Framework</span></span>

<span data-ttu-id="2682c-221">Хорошо то, что все шаблоны разработки, описанные в предыдущем разделе, просто реализуется с помощью EF4.</span><span class="sxs-lookup"><span data-stu-id="2682c-221">The good news is that all of the design patterns we described in the last section are straightforward to implement with EF4.</span></span> <span data-ttu-id="2682c-222">Для демонстрации мы собираемся использовать простое приложение ASP.NET MVC для редактирования и отображения сотрудников и их связанные сведения.</span><span class="sxs-lookup"><span data-stu-id="2682c-222">To demonstrate we are going to use a simple ASP.NET MVC application to edit and display Employees and their associated time card information.</span></span> <span data-ttu-id="2682c-223">Мы начнем с помощью следующих «старые объекты CLR» (POCO).</span><span class="sxs-lookup"><span data-stu-id="2682c-223">We’ll start by using the following “plain old CLR objects” (POCOs).</span></span> 

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

<span data-ttu-id="2682c-224">Эти определения классов немного меняется, как мы рассмотрим различные подходы и особенностей EF4, но цель состоит в том, чтобы сохранить эти классы как игнорирующих сохраняемость (PI) максимально.</span><span class="sxs-lookup"><span data-stu-id="2682c-224">These class definitions will change slightly as we explore different approaches and features of EF4, but the intent is to keep these classes as persistence ignorant (PI) as possible.</span></span> <span data-ttu-id="2682c-225">Объект PI не знает *как*, или даже *Если*, состояние, в ней находится внутри базы данных.</span><span class="sxs-lookup"><span data-stu-id="2682c-225">A PI object doesn’t know *how*, or even *if*, the state it holds lives inside a database.</span></span> <span data-ttu-id="2682c-226">PI и POCO идут рука об руку с тестируемого программного обеспечения.</span><span class="sxs-lookup"><span data-stu-id="2682c-226">PI and POCOs go hand in hand with testable software.</span></span> <span data-ttu-id="2682c-227">Объекты, с использованием подхода POCO являются менее ограниченного более гибким и проще тестировать, поскольку они могут работать без базы данных существует.</span><span class="sxs-lookup"><span data-stu-id="2682c-227">Objects using a POCO approach are less constrained, more flexible, and easier to test because they can operate without a database present.</span></span>

<span data-ttu-id="2682c-228">С помощью POCO на месте мы можем создать Entity Data Model (EDM) в Visual Studio (см. рис. 1).</span><span class="sxs-lookup"><span data-stu-id="2682c-228">With the POCOs in place we can create an Entity Data Model (EDM) in Visual Studio (see figure 1).</span></span> <span data-ttu-id="2682c-229">Мы не будем использовать для создания кода для наших сущностей модели EDM.</span><span class="sxs-lookup"><span data-stu-id="2682c-229">We will not use the EDM to generate code for our entities.</span></span> <span data-ttu-id="2682c-230">Вместо этого мы хотим использовать сущностей, которые мы любовно создавать вручную.</span><span class="sxs-lookup"><span data-stu-id="2682c-230">Instead, we want to use the entities we lovingly craft by hand.</span></span> <span data-ttu-id="2682c-231">Модель EDM используется только для создания схемы базы данных и введите метаданные EF4 должен сопоставить объекты в базе данных.</span><span class="sxs-lookup"><span data-stu-id="2682c-231">We will only use the EDM to generate our database schema and provide the metadata EF4 needs to map objects into the database.</span></span>

![EF test_01](~/ef6/media/eftest-01.jpg)

<span data-ttu-id="2682c-233">**Рис. 1**</span><span class="sxs-lookup"><span data-stu-id="2682c-233">**Figure 1**</span></span>

<span data-ttu-id="2682c-234">Примечание: Если вы хотите разрабатывать модель EDM, во-первых, это невозможно создать очистки код POCO из модели EDM.</span><span class="sxs-lookup"><span data-stu-id="2682c-234">Note: if you want to develop the EDM model first, it is possible to generate clean, POCO code from the EDM.</span></span> <span data-ttu-id="2682c-235">Это можно сделать с помощью расширения Visual Studio 2010, предоставляемые группе программирования данных.</span><span class="sxs-lookup"><span data-stu-id="2682c-235">You can do this with a Visual Studio 2010 extension provided by the Data Programmability team.</span></span> <span data-ttu-id="2682c-236">Чтобы скачать расширение, запустите диспетчер расширений из меню "Сервис" в Visual Studio и выполните поиск веб-коллекции шаблонов «POCO» (см. рис. 2).</span><span class="sxs-lookup"><span data-stu-id="2682c-236">To download the extension, launch the Extension Manager from the Tools menu in Visual Studio and search the online gallery of templates for “POCO” (See figure 2).</span></span> <span data-ttu-id="2682c-237">Существует несколько шаблонов POCO для Entity FRAMEWORK.</span><span class="sxs-lookup"><span data-stu-id="2682c-237">There are several POCO templates available for EF.</span></span> <span data-ttu-id="2682c-238">Дополнительные сведения об использовании шаблона см. в разделе " [Пошаговое руководство: шаблон POCO для Entity Framework](http://blogs.msdn.com/adonet/pages/walkthrough-poco-template-for-the-entity-framework.aspx)«.</span><span class="sxs-lookup"><span data-stu-id="2682c-238">For more information on using the template, see “ [Walkthrough: POCO Template for the Entity Framework](http://blogs.msdn.com/adonet/pages/walkthrough-poco-template-for-the-entity-framework.aspx)”.</span></span>

![EF test_02](~/ef6/media/eftest-02.png)

<span data-ttu-id="2682c-240">**Рис. 2**</span><span class="sxs-lookup"><span data-stu-id="2682c-240">**Figure 2**</span></span>

<span data-ttu-id="2682c-241">Из этого POCO, начальная точка мы рассмотрим два различных подхода для тестируемого кода.</span><span class="sxs-lookup"><span data-stu-id="2682c-241">From this POCO starting point we will explore two different approaches to testable code.</span></span> <span data-ttu-id="2682c-242">Первый подход, я вызываю метод EF, так как он использует абстракции от Entity Framework API для реализации единицы работы и репозиториев.</span><span class="sxs-lookup"><span data-stu-id="2682c-242">The first approach I call the EF approach because it leverages abstractions from the Entity Framework API to implement units of work and repositories.</span></span> <span data-ttu-id="2682c-243">Второй способ мы создадим собственных абстракций пользовательский репозиторий и затем см. в разделе преимущества и недостатки каждого подхода.</span><span class="sxs-lookup"><span data-stu-id="2682c-243">In the second approach we will create our own custom repository abstractions and then see the advantages and disadvantages of each approach.</span></span> <span data-ttu-id="2682c-244">Мы начнем с изучения подход EF.</span><span class="sxs-lookup"><span data-stu-id="2682c-244">We’ll start by exploring the EF approach.</span></span>  

### <a name="an-ef-centric-implementation"></a><span data-ttu-id="2682c-245">Ориентированные на реализацию EF</span><span class="sxs-lookup"><span data-stu-id="2682c-245">An EF Centric Implementation</span></span>

<span data-ttu-id="2682c-246">Рассмотрим следующее действие контроллера из проекта ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="2682c-246">Consider the following controller action from an ASP.NET MVC project.</span></span> <span data-ttu-id="2682c-247">Действие извлекает объект Employee и возвращает результат, чтобы отобразить подробное представление сотрудника.</span><span class="sxs-lookup"><span data-stu-id="2682c-247">The action retrieves an Employee object and returns a result to display a detailed view of the employee.</span></span>

``` csharp
    public ViewResult Details(int id) {
        var employee = _unitOfWork.Employees
                                  .Single(e => e.Id == id);
        return View(employee);
    }
```

<span data-ttu-id="2682c-248">Можно протестировать код?</span><span class="sxs-lookup"><span data-stu-id="2682c-248">Is the code testable?</span></span> <span data-ttu-id="2682c-249">Существует по крайней мере два теста, необходимо проверить его поведение.</span><span class="sxs-lookup"><span data-stu-id="2682c-249">There are at least two tests we’d need to verify the action’s behavior.</span></span> <span data-ttu-id="2682c-250">Во-первых мы бы хотели убедиться, что это действие возвращает правильный режим — простым тестом.</span><span class="sxs-lookup"><span data-stu-id="2682c-250">First, we’d like to verify the action returns the correct view – an easy test.</span></span> <span data-ttu-id="2682c-251">Мы бы также требуется написать тест, чтобы проверить действие извлекает правильный сотрудника, и мы бы хотели сделать это без выполнения кода в базе данных.</span><span class="sxs-lookup"><span data-stu-id="2682c-251">We’d also want to write a test to verify the action retrieves the correct employee, and we’d like to do this without executing code to query the database.</span></span> <span data-ttu-id="2682c-252">Помните, что нам нужно изолировать тестируемый код.</span><span class="sxs-lookup"><span data-stu-id="2682c-252">Remember we want to isolate the code under test.</span></span> <span data-ttu-id="2682c-253">Изоляция будет убедитесь, что тест не произошел сбой из-за ошибки в модуле кода доступа к данным или базы данных конфигурации.</span><span class="sxs-lookup"><span data-stu-id="2682c-253">Isolation will ensure the test doesn’t fail because of a bug in the data access code or database configuration.</span></span> <span data-ttu-id="2682c-254">Если тест не пройден, мы знаем, что у нас есть ошибки в логике контроллера, а не в некоторых нижнего уровня системного компонента.</span><span class="sxs-lookup"><span data-stu-id="2682c-254">If the test fails, we will know we have a bug in the controller logic, and not in some lower level system component.</span></span>

<span data-ttu-id="2682c-255">Для обеспечения изоляции, потребуются некоторые абстракции, как и интерфейсы, которые мы представленные выше для репозиториев и единиц работы.</span><span class="sxs-lookup"><span data-stu-id="2682c-255">To achieve isolation we’ll need some abstractions like the interfaces we presented earlier for repositories and units of work.</span></span> <span data-ttu-id="2682c-256">Помните, что шаблон репозитория предназначен в качестве посредника между объектами домена и уровень сопоставления данных.</span><span class="sxs-lookup"><span data-stu-id="2682c-256">Remember the repository pattern is designed to mediate between domain objects and the data mapping layer.</span></span> <span data-ttu-id="2682c-257">В этом сценарии EF4 *—* слоя сопоставления данных и уже предоставляет абстракцию хранилища с именем помощью IObjectSet&lt;T&gt; (из пространства имен System.Data.Objects).</span><span class="sxs-lookup"><span data-stu-id="2682c-257">In this scenario EF4 *is* the data mapping layer, and already provides a repository-like abstraction named IObjectSet&lt;T&gt; (from the System.Data.Objects namespace).</span></span> <span data-ttu-id="2682c-258">Определение интерфейса выглядит следующим образом.</span><span class="sxs-lookup"><span data-stu-id="2682c-258">The interface definition looks like the following.</span></span>

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

<span data-ttu-id="2682c-259">Помощью IObjectSet&lt;T&gt; соответствует требованиям для репозитория, так как он напоминает коллекции объектов (через интерфейс IEnumerable&lt;T&gt;) и предоставляет методы для добавления и удаления объектов из коллекции имитации.</span><span class="sxs-lookup"><span data-stu-id="2682c-259">IObjectSet&lt;T&gt; meets the requirements for a repository because it resembles a collection of objects (via IEnumerable&lt;T&gt;) and provides methods to add and remove objects from the simulated collection.</span></span> <span data-ttu-id="2682c-260">Методы присоединения и отсоединения предоставляют дополнительные возможности EF4 API.</span><span class="sxs-lookup"><span data-stu-id="2682c-260">The Attach and Detach methods expose additional capabilities of the EF4 API.</span></span> <span data-ttu-id="2682c-261">Для использования с помощью IObjectSet&lt;T&gt; как интерфейс для репозиториев, мы должны единицу работы абстракции для связать репозитории вместе.</span><span class="sxs-lookup"><span data-stu-id="2682c-261">To use IObjectSet&lt;T&gt; as the interface for repositories we need a unit of work abstraction to bind repositories together.</span></span>

``` csharp
    public interface IUnitOfWork {
        IObjectSet<Employee> Employees { get; }
        IObjectSet<TimeCard> TimeCards { get; }
        void Commit();
    }
```

<span data-ttu-id="2682c-262">Одну конкретную реализацию этого интерфейса будет обращаться к SQL Server и можно легко создать с помощью класса ObjectContext с помощью EF4.</span><span class="sxs-lookup"><span data-stu-id="2682c-262">One concrete implementation of this interface will talk to SQL Server and is easy to create using the ObjectContext class from EF4.</span></span> <span data-ttu-id="2682c-263">Класс ObjectContext является реальной единицей работы в EF4 API.</span><span class="sxs-lookup"><span data-stu-id="2682c-263">The ObjectContext class is the real unit of work in the EF4 API.</span></span>

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

<span data-ttu-id="2682c-264">Перенос с помощью IObjectSet&lt;T&gt; в жизнь так же прост, что и вызов метод CreateObjectSet объекта ObjectContext.</span><span class="sxs-lookup"><span data-stu-id="2682c-264">Bringing an IObjectSet&lt;T&gt; to life is as easy as invoking the CreateObjectSet method of the ObjectContext object.</span></span> <span data-ttu-id="2682c-265">За кулисами платформа будет использовать метаданные мы предоставили в модели EDM для создания конкретных ObjectSet&lt;T&gt;.</span><span class="sxs-lookup"><span data-stu-id="2682c-265">Behind the scenes the framework will use the metadata we provided in the EDM to produce a concrete ObjectSet&lt;T&gt;.</span></span> <span data-ttu-id="2682c-266">Поместим при возврате помощью IObjectSet&lt;T&gt; интерфейс, так как он поможет сохранить пригодности для тестирования в клиентском коде.</span><span class="sxs-lookup"><span data-stu-id="2682c-266">We’ll stick with returning the IObjectSet&lt;T&gt; interface because it will help preserve testability in client code.</span></span>

<span data-ttu-id="2682c-267">Это конкретная реализация полезно в рабочей среде, но нам нужно сосредоточиться на том, как мы будем использовать наши IUnitOfWork абстракции для упрощения тестирования.</span><span class="sxs-lookup"><span data-stu-id="2682c-267">This concrete implementation is useful in production, but we need to focus on how we’ll use our IUnitOfWork abstraction to facilitate testing.</span></span>

### <a name="the-test-doubles"></a><span data-ttu-id="2682c-268">Тестовых дублеров</span><span class="sxs-lookup"><span data-stu-id="2682c-268">The Test Doubles</span></span>

<span data-ttu-id="2682c-269">Чтобы изолировать действия контроллера нам потребуется возможность переключаться между реальных единицу работы (поддерживаемый ObjectContext) и тест double или «фальшивая» единицы работы (выполнение операций в памяти).</span><span class="sxs-lookup"><span data-stu-id="2682c-269">To isolate the controller action we’ll need the ability to switch between the real unit of work (backed by an ObjectContext) and a test double or “fake” unit of work (performing in-memory operations).</span></span> <span data-ttu-id="2682c-270">Распространенный подход для выполнения такого рода переключение является не могут создать экземпляр единицу работы, но вместо этого передайте единицу работы в качестве параметра конструктора контроллер контроллера MVC.</span><span class="sxs-lookup"><span data-stu-id="2682c-270">The common approach to perform this type of switching is to not let the MVC controller instantiate a unit of work, but instead pass the unit of work into the controller as a constructor parameter.</span></span>

``` csharp
    class EmployeeController : Controller {
      publicEmployeeController(IUnitOfWork unitOfWork)  {
          _unitOfWork = unitOfWork;
      }
      ...
    }
```

<span data-ttu-id="2682c-271">Приведенный выше код является примером внедрения зависимостей.</span><span class="sxs-lookup"><span data-stu-id="2682c-271">The above code is an example of dependency injection.</span></span> <span data-ttu-id="2682c-272">Мы не позволяем на контроллере, чтобы создать его зависимостей (единица работы), но внедрения зависимости в контроллер.</span><span class="sxs-lookup"><span data-stu-id="2682c-272">We don’t allow the controller to create it’s dependency (the unit of work) but inject the dependency into the controller.</span></span> <span data-ttu-id="2682c-273">В проекте MVC довольно часто использовать фабрику настраиваемый контроллер в сочетании с инверсией управления (IoC) контейнера для автоматизации внедрения зависимостей.</span><span class="sxs-lookup"><span data-stu-id="2682c-273">In an MVC project it is common to use a custom controller factory in combination with an inversion of control (IoC) container to automate dependency injection.</span></span> <span data-ttu-id="2682c-274">Эти темы выходят за рамки данной статьи, но можно прочитать по более следующие ссылки в конце этой статьи.</span><span class="sxs-lookup"><span data-stu-id="2682c-274">These topics are beyond the scope of this article, but you can read more by following the references at the end of this article.</span></span>

<span data-ttu-id="2682c-275">Единое фиктивные реализации работы, который можно использовать для тестирования может выглядеть следующим образом.</span><span class="sxs-lookup"><span data-stu-id="2682c-275">A fake unit of work implementation that we can use for testing might look like the following.</span></span>

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

<span data-ttu-id="2682c-276">Обратите внимание, что фиктивные единицу работы предоставляет свойство были применены.</span><span class="sxs-lookup"><span data-stu-id="2682c-276">Notice the fake unit of work exposes a Commited property.</span></span> <span data-ttu-id="2682c-277">Иногда полезно добавить компоненты в класс фиктивными, упростить тестирование.</span><span class="sxs-lookup"><span data-stu-id="2682c-277">It’s sometimes useful to add features to a fake class that facilitate testing.</span></span> <span data-ttu-id="2682c-278">В этом случае проще наблюдать, если код фиксирует единицу работы, путем проверки свойства были применены.</span><span class="sxs-lookup"><span data-stu-id="2682c-278">In this case it is easy to observe if code commits a unit of work by checking the Commited property.</span></span>

<span data-ttu-id="2682c-279">Нам также потребуется фальшивые помощью IObjectSet&lt;T&gt; для хранения сотрудников и TimeCard объектов в памяти.</span><span class="sxs-lookup"><span data-stu-id="2682c-279">We’ll also need a fake IObjectSet&lt;T&gt; to hold Employee and TimeCard objects in memory.</span></span> <span data-ttu-id="2682c-280">Мы можем предоставить единую реализацию использовании универсальных шаблонов.</span><span class="sxs-lookup"><span data-stu-id="2682c-280">We can provide a single implementation using generics.</span></span>

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

<span data-ttu-id="2682c-281">Этот тестовый дублер делегирует большинство операций на базовый класс HashSet&lt;T&gt; объекта.</span><span class="sxs-lookup"><span data-stu-id="2682c-281">This test double delegates most of its work to an underlying HashSet&lt;T&gt; object.</span></span> <span data-ttu-id="2682c-282">Обратите внимание, помощью IObjectSet&lt;T&gt; требует универсального ограничения применения T в качестве класса (ссылочный тип), а также заставляет нас реализацию IQueryable&lt;T&gt;.</span><span class="sxs-lookup"><span data-stu-id="2682c-282">Note that IObjectSet&lt;T&gt; requires a generic constraint enforcing T as a class (a reference type), and also forces us to implement IQueryable&lt;T&gt;.</span></span> <span data-ttu-id="2682c-283">Это легко сделать в коллекцию в памяти, которые отображаются как IQueryable&lt;T&gt; с помощью стандартного оператора LINQ AsQueryable.</span><span class="sxs-lookup"><span data-stu-id="2682c-283">It is easy to make an in-memory collection appear as an IQueryable&lt;T&gt; using the standard LINQ operator AsQueryable.</span></span>

### <a name="the-tests"></a><span data-ttu-id="2682c-284">Тесты</span><span class="sxs-lookup"><span data-stu-id="2682c-284">The Tests</span></span>

<span data-ttu-id="2682c-285">Традиционные модульные тесты будут использовать одного класса тестов для хранения всех тестов для всех действий в один контроллер MVC.</span><span class="sxs-lookup"><span data-stu-id="2682c-285">Traditional unit tests will use a single test class to hold all of the tests for all of the actions in a single MVC controller.</span></span> <span data-ttu-id="2682c-286">Мы можем написать эти тесты, или любого типа модульного теста, используя в памяти имитаций мы создали.</span><span class="sxs-lookup"><span data-stu-id="2682c-286">We can write these tests, or any type of unit test, using the in memory fakes we’ve built.</span></span> <span data-ttu-id="2682c-287">Однако в этой статье, в которой мы будет избегать монолитного приложения тестирования подход на основе класса и сгруппировать связанные наших тестов, чтобы сосредоточиться на определенной части функциональных возможностей.</span><span class="sxs-lookup"><span data-stu-id="2682c-287">However, for this article we will avoid the monolithic test class approach and instead group our tests to focus on a specific piece of functionality.</span></span>  <span data-ttu-id="2682c-288">Например, «создание нового сотрудника» может быть функциональные возможности, которые мы хотим проверить, поэтому мы будем использовать одного класса тестов, чтобы проверить, как одного контроллера работает отвечает за создание нового сотрудника.</span><span class="sxs-lookup"><span data-stu-id="2682c-288">For example, “create new employee” might be the functionality we want to test, so we will use a single test class to verify the single controller action responsible for creating a new employee.</span></span>

<span data-ttu-id="2682c-289">Есть некоторые распространенные код настройки, необходимые для все эти детально тестовых классов.</span><span class="sxs-lookup"><span data-stu-id="2682c-289">There is some common setup code we need for all these fine grained test classes.</span></span> <span data-ttu-id="2682c-290">Например необходимо всегда создать наших репозиториях в памяти и фальшивые единицу работы.</span><span class="sxs-lookup"><span data-stu-id="2682c-290">For example, we always need to create our in-memory repositories and fake unit of work.</span></span> <span data-ttu-id="2682c-291">Необходимо также экземпляра контроллера сотрудника с фиктивными единицу работы вставлен.</span><span class="sxs-lookup"><span data-stu-id="2682c-291">We also need an instance of the employee controller with the fake unit of work injected.</span></span> <span data-ttu-id="2682c-292">Мы будем совместного использования общего кода установки тестовых классов с помощью базового класса.</span><span class="sxs-lookup"><span data-stu-id="2682c-292">We’ll share this common setup code across test classes by using a base class.</span></span>

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

<span data-ttu-id="2682c-293">Мы используем в базовом классе «объект мама» — один из распространенных шаблонов для создания тестовых данных.</span><span class="sxs-lookup"><span data-stu-id="2682c-293">The “object mother” we use in the base class is one common pattern for creating test data.</span></span> <span data-ttu-id="2682c-294">Мама объект содержит фабричные методы для создания экземпляра сущности теста для использования в нескольких средства тестирования.</span><span class="sxs-lookup"><span data-stu-id="2682c-294">An object mother contains factory methods to instantiate test entities for use across multiple test fixtures.</span></span>

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

<span data-ttu-id="2682c-295">Мы используем EmployeeControllerTestBase как базовый класс для числа средства тестирования (см. рис. 3).</span><span class="sxs-lookup"><span data-stu-id="2682c-295">We can use the EmployeeControllerTestBase as the base class for a number of test fixtures (see figure 3).</span></span> <span data-ttu-id="2682c-296">Каждый средство тестирования тестирование определенное действие контроллера.</span><span class="sxs-lookup"><span data-stu-id="2682c-296">Each test fixture will test a specific controller action.</span></span> <span data-ttu-id="2682c-297">Например, одно средство тестирования основное внимание уделяется тестирование создать действие, используемое при выполнении запроса HTTP GET (чтобы отобразить представление для создания сотрудника), и другой тестовый стенд основное внимание уделяется создать действие, используемое в запрос HTTP POST (будет извлекать сведения, отправленные пользователю создать сотрудника).</span><span class="sxs-lookup"><span data-stu-id="2682c-297">For example, one test fixture will focus on testing the Create action used during an HTTP GET request (to display the view for creating an employee), and a different fixture will focus on the Create action used in an HTTP POST request (to take information submitted by the user to create an employee).</span></span> <span data-ttu-id="2682c-298">Каждый производный класс отвечает только за настройки, необходимый в его определенном контексте, а также для предоставления утверждения, необходимые для проверки результатов для контекст конкретного теста.</span><span class="sxs-lookup"><span data-stu-id="2682c-298">Each derived class is only responsible for the setup needed in its specific context, and to provide the assertions needed to verify the outcomes for its specific test context.</span></span>

![EF test_03](~/ef6/media/eftest-03.png)

<span data-ttu-id="2682c-300">**Рис. 3**</span><span class="sxs-lookup"><span data-stu-id="2682c-300">**Figure 3**</span></span>

<span data-ttu-id="2682c-301">Соглашение о вызовах и тестирования стиль именования представленные здесь, не является обязательным для тестируемого кода — это лишь одним из подходов.</span><span class="sxs-lookup"><span data-stu-id="2682c-301">The naming convention and test style presented here isn’t required for testable code – it’s just one approach.</span></span> <span data-ttu-id="2682c-302">Рис. 4 показано, что тесты, выполняемые в Resharper полушарие Jet тестов средство запуска подключаемого модуля для Visual Studio 2010.</span><span class="sxs-lookup"><span data-stu-id="2682c-302">Figure 4 shows the tests running in the Jet Brains Resharper test runner plugin for Visual Studio 2010.</span></span>

![EF test_04](~/ef6/media/eftest-04.png)

<span data-ttu-id="2682c-304">**Рис. 4**</span><span class="sxs-lookup"><span data-stu-id="2682c-304">**Figure 4**</span></span>

<span data-ttu-id="2682c-305">С базовым классом для обработки ее кода сетевую установку модульные тесты для каждого действия контроллера небольшим и простым для записи.</span><span class="sxs-lookup"><span data-stu-id="2682c-305">With a base class to handle the shared setup code, the unit tests for each controller action are small and easy to write.</span></span> <span data-ttu-id="2682c-306">Тесты будут выполняться быстро (поскольку мы занимаемся операций в памяти) и не должны завершаться сбоем из-за несвязанных инфраструктуры или охраной окружающей среды (поскольку мы определили, что модульного теста).</span><span class="sxs-lookup"><span data-stu-id="2682c-306">The tests will execute quickly (since we are performing in-memory operations), and shouldn’t fail because of unrelated infrastructure or environmental concerns (because we’ve isolated the unit under test).</span></span>

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

<span data-ttu-id="2682c-307">В этих тестах базовый класс не выполняет большую часть работы программы установки.</span><span class="sxs-lookup"><span data-stu-id="2682c-307">In these tests, the base class does most of the setup work.</span></span> <span data-ttu-id="2682c-308">Помните, что конструктор базового класса создает хранилище в памяти, фиктивный единицу работы и экземпляр класса EmployeeController.</span><span class="sxs-lookup"><span data-stu-id="2682c-308">Remember the base class constructor creates the in-memory repository, a fake unit of work, and an instance of the EmployeeController class.</span></span> <span data-ttu-id="2682c-309">Тестовый класс является производным от этого базового класса и фокусируется на особенности тестирование метода Create.</span><span class="sxs-lookup"><span data-stu-id="2682c-309">The test class derives from this base class and focuses on the specifics of testing the Create method.</span></span> <span data-ttu-id="2682c-310">В этом случае особенности подразделяются на «упорядочить, действовать и утвердить» шаги, которые можно будет увидеть в любой модульное тестирование процедуры:</span><span class="sxs-lookup"><span data-stu-id="2682c-310">In this case the specifics boil down to the “arrange, act, and assert” steps you’ll see in any unit testing procedure:</span></span>

-   <span data-ttu-id="2682c-311">Создайте объект newEmployee для имитации входящих данных.</span><span class="sxs-lookup"><span data-stu-id="2682c-311">Create a newEmployee object to simulate incoming data.</span></span>
-   <span data-ttu-id="2682c-312">Вызвать действие Create EmployeeController и передайте newEmployee.</span><span class="sxs-lookup"><span data-stu-id="2682c-312">Invoke the Create action of the EmployeeController and pass in the newEmployee.</span></span>
-   <span data-ttu-id="2682c-313">Убедитесь, что действие создания получить желаемые результаты, (сотрудника отображается в репозитории).</span><span class="sxs-lookup"><span data-stu-id="2682c-313">Verify the Create action produces the expected results (the employee appears in the repository).</span></span>

<span data-ttu-id="2682c-314">Мы создали позволяет нам протестировать EmployeeController действия.</span><span class="sxs-lookup"><span data-stu-id="2682c-314">What we’ve built allows us to test any of the EmployeeController actions.</span></span> <span data-ttu-id="2682c-315">Например при написании тестов для действии Index контроллера сотрудника мы может наследовать от базового класса теста, чтобы установить эту же базовая установка для наших тестов.</span><span class="sxs-lookup"><span data-stu-id="2682c-315">For example, when we write tests for the Index action of the Employee controller we can inherit from the test base class to establish the same base setup for our tests.</span></span> <span data-ttu-id="2682c-316">Еще раз базовый класс создаст хранилище в памяти, фиктивный единица работы и экземпляр EmployeeController.</span><span class="sxs-lookup"><span data-stu-id="2682c-316">Again the base class will create the in-memory repository, the fake unit of work, and an instance of the EmployeeController.</span></span> <span data-ttu-id="2682c-317">Тесты для действия индекса только необходимо сосредоточиться на вызове действия индекса и тестирование качества модели действие возвращает.</span><span class="sxs-lookup"><span data-stu-id="2682c-317">The tests for the Index action only need to focus on invoking the Index action and testing the qualities of the model the action returns.</span></span>

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

<span data-ttu-id="2682c-318">Тесты создаются с помощью fakes в памяти на достижение тестирования *состояние* программного обеспечения.</span><span class="sxs-lookup"><span data-stu-id="2682c-318">The tests we are creating with in-memory fakes are oriented towards testing the *state* of the software.</span></span> <span data-ttu-id="2682c-319">Например при тестировании действия создания, нам нужно проверять состояние репозитория, после выполнения действия создания — репозиторий хранит новый сотрудник?</span><span class="sxs-lookup"><span data-stu-id="2682c-319">For example, when testing the Create action we want to inspect the state of the repository after the create action executes – does the repository hold the new employee?</span></span>

``` csharp
    [TestMethod]
    public void ShouldAddNewEmployeeToRepository() {
        _controller.Create(_newEmployee);
        Assert.IsTrue(_repository.Contains(_newEmployee));
    }
```

<span data-ttu-id="2682c-320">Позже мы рассмотрим взаимодействие на основе тестирования.</span><span class="sxs-lookup"><span data-stu-id="2682c-320">Later we’ll look at interaction based testing.</span></span> <span data-ttu-id="2682c-321">Тестирование взаимодействия на основе запрашивает Если тестируемый код вызывается соответствующие методы объектов и получает правильные параметры.</span><span class="sxs-lookup"><span data-stu-id="2682c-321">Interaction based testing will ask if the code under test invoked the proper methods on our objects and passed the correct parameters.</span></span> <span data-ttu-id="2682c-322">Сейчас мы перейдем на обложке другой design pattern — отложенной загрузки.</span><span class="sxs-lookup"><span data-stu-id="2682c-322">For now we’ll move on the cover another design pattern – the lazy load.</span></span>

## <a name="eager-loading-and-lazy-loading"></a><span data-ttu-id="2682c-323">Безотложная загрузка и отложенная загрузка</span><span class="sxs-lookup"><span data-stu-id="2682c-323">Eager Loading and Lazy Loading</span></span>

<span data-ttu-id="2682c-324">В определенный момент в ASP.NET MVC веб-приложение, которое мы может потребоваться просмотреть информацию сотрудника, а также включать сотрудника связанные время карты.</span><span class="sxs-lookup"><span data-stu-id="2682c-324">At some point in the ASP.NET  MVC web application we might wish to display an employee’s information and include the employee’s associated time cards.</span></span> <span data-ttu-id="2682c-325">Например может использоваться карта времени отображения сводки, который отображает имя сотрудника и общее количество времени карт в системе.</span><span class="sxs-lookup"><span data-stu-id="2682c-325">For example, we might have a time card summary display that shows the employee’s name and the total number of time cards in the system.</span></span> <span data-ttu-id="2682c-326">Существует несколько подходов, которые мы можно предпринять для реализации этой возможности.</span><span class="sxs-lookup"><span data-stu-id="2682c-326">There are several approaches we can take to implement this feature.</span></span>

### <a name="projection"></a><span data-ttu-id="2682c-327">Проекция</span><span class="sxs-lookup"><span data-stu-id="2682c-327">Projection</span></span>

<span data-ttu-id="2682c-328">— Это один из простых способов создания сводки для построения модели, выделенные с данными, которые нужно отобразить в представлении.</span><span class="sxs-lookup"><span data-stu-id="2682c-328">One easy approach to create the summary is to construct a model dedicated to the information we want to display in the view.</span></span> <span data-ttu-id="2682c-329">В этом случае модель может выглядеть следующим образом.</span><span class="sxs-lookup"><span data-stu-id="2682c-329">In this scenario the model might look like the following.</span></span>

``` csharp
    public class EmployeeSummaryViewModel {
        public string Name { get; set; }
        public int TotalTimeCards { get; set; }
    }
```

<span data-ttu-id="2682c-330">Обратите внимание, что EmployeeSummaryViewModel не является сущностью, — другими словами это не то, что мы должны сохраняться в базе данных.</span><span class="sxs-lookup"><span data-stu-id="2682c-330">Note that the EmployeeSummaryViewModel is not an entity – in other words it is not something we want to persist in the database.</span></span> <span data-ttu-id="2682c-331">Только мы собираемся использовать этот класс распределяет данные в представление в режиме строгой типизации.</span><span class="sxs-lookup"><span data-stu-id="2682c-331">We are only going to use this class to shuffle data into the view in a strongly typed manner.</span></span> <span data-ttu-id="2682c-332">Передача данных объекта (DTO), так как в нем нет поведения (методы не) — только свойства аналогична модели представления.</span><span class="sxs-lookup"><span data-stu-id="2682c-332">The view model is like a data transfer object (DTO) because it contains no behavior (no methods) – only properties.</span></span> <span data-ttu-id="2682c-333">Свойства будет содержать данные, необходимые для перемещения.</span><span class="sxs-lookup"><span data-stu-id="2682c-333">The properties will hold the data we need to move.</span></span> <span data-ttu-id="2682c-334">Его можно легко создать экземпляр этой модели представления, с помощью LINQ проекции стандартный оператор — оператор Select.</span><span class="sxs-lookup"><span data-stu-id="2682c-334">It is easy to instantiate this view model using LINQ’s standard projection operator – the Select operator.</span></span>

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

<span data-ttu-id="2682c-335">Существуют две важные функции в приведенный выше код.</span><span class="sxs-lookup"><span data-stu-id="2682c-335">There are two notable features to the above code.</span></span> <span data-ttu-id="2682c-336">Сначала — код можно легко проверить, так как он по-прежнему позволяет легко наблюдать и изолировать.</span><span class="sxs-lookup"><span data-stu-id="2682c-336">First – the code is easy to test because it is still easy to observe and isolate.</span></span> <span data-ttu-id="2682c-337">Оператор Select точно так же работает с нашей fakes в памяти, как в случае с реальной единицу работы.</span><span class="sxs-lookup"><span data-stu-id="2682c-337">The Select operator works just as well against our in-memory fakes as it does against the real unit of work.</span></span>

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

<span data-ttu-id="2682c-338">Вторая функция, важные заключается в том, как код позволяет EF4 для формирования запроса единый, эффективных сборка сотрудника и времени сведения о карте друг с другом.</span><span class="sxs-lookup"><span data-stu-id="2682c-338">The second notable feature is how the code allows EF4 to generate a single, efficient query to assemble employee and time card information together.</span></span> <span data-ttu-id="2682c-339">Мы загрузили данные о сотрудниках и сведения в тот же объект без использования любые специальные интерфейсы API.</span><span class="sxs-lookup"><span data-stu-id="2682c-339">We’ve loaded employee information and time card information into the same object without using any special APIs.</span></span> <span data-ttu-id="2682c-340">Код просто выражен сведения, что он требует использования стандартных операторов LINQ, которые работают с источниками данных в памяти, а также удаленным источникам данных.</span><span class="sxs-lookup"><span data-stu-id="2682c-340">The code merely expressed the information it requires using standard LINQ operators that work against in-memory data sources as well as remote data sources.</span></span> <span data-ttu-id="2682c-341">EF4 была возможность перевода деревья выражений, создаваемый запрос LINQ и C\# компилятора в единый и эффективный запрос T-SQL.</span><span class="sxs-lookup"><span data-stu-id="2682c-341">EF4 was able to translate the expression trees generated by the LINQ query and C\# compiler into a single and efficient T-SQL query.</span></span>

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
         )  AS [Project1]
    )  AS [Limit1]
```

<span data-ttu-id="2682c-342">Существуют в других случаях, когда мы не хотим работать с модели представления или объект DTO, но реальные сущности.</span><span class="sxs-lookup"><span data-stu-id="2682c-342">There are other times when we don’t want to work with a view model or DTO object, but with real entities.</span></span> <span data-ttu-id="2682c-343">Когда мы знаем, нам нужно сотрудник *и* карт время сотрудника, мы можем заранее загрузить связанные данные ненавязчивого и эффективным образом.</span><span class="sxs-lookup"><span data-stu-id="2682c-343">When we know we need an employee *and* the employee’s time cards, we can eagerly load the related data in an unobtrusive and efficient manner.</span></span>

### <a name="explicit-eager-loading"></a><span data-ttu-id="2682c-344">Явная Безотложная загрузка</span><span class="sxs-lookup"><span data-stu-id="2682c-344">Explicit Eager Loading</span></span>

<span data-ttu-id="2682c-345">Когда мы хотим заранее загрузить сведения о связанной сущности, необходимо, чтобы какой-либо механизм для бизнес-логики (или в этом случае логика действия контроллера) для выражения необходимость в репозиторий.</span><span class="sxs-lookup"><span data-stu-id="2682c-345">When we want to eagerly load related entity information we need some mechanism for business logic (or in this scenario, controller action logic) to express its desire to the repository.</span></span> <span data-ttu-id="2682c-346">Для класса ObjectQuery EF4&lt;T&gt; класс определяет метод Include, чтобы задать связанные объекты должны быть получены при запросе.</span><span class="sxs-lookup"><span data-stu-id="2682c-346">The EF4 ObjectQuery&lt;T&gt; class defines an Include method to specify the related objects to retrieve during a query.</span></span> <span data-ttu-id="2682c-347">Помните, EF4 ObjectContext предоставляет сущности через конкретный ObjectSet&lt;T&gt; класс, который наследуется от ObjectQuery&lt;T&gt;.</span><span class="sxs-lookup"><span data-stu-id="2682c-347">Remember the EF4 ObjectContext exposes entities via the concrete ObjectSet&lt;T&gt; class which inherits from ObjectQuery&lt;T&gt;.</span></span>  <span data-ttu-id="2682c-348">Если мы использовали ObjectSet&lt;T&gt; ссылок в наши действия контроллера, можно было бы написать следующий код, чтобы указать диапазон безотложной загрузки сведения для каждого сотрудника.</span><span class="sxs-lookup"><span data-stu-id="2682c-348">If we were using ObjectSet&lt;T&gt; references in our controller action we could write the following code to specify an eager load of time card information for each employee.</span></span>

``` csharp
    _employees.Include("TimeCards")
              .Where(e => e.HireDate.Year > 2009);
```

<span data-ttu-id="2682c-349">Тем не менее, так как мы пытаемся хранить наш код пригодного для тестирования мы не предоставляете ObjectSet&lt;T&gt; из за пределами реальных класс рабочих единиц.</span><span class="sxs-lookup"><span data-stu-id="2682c-349">However, since we are trying to keep our code testable we are not exposing ObjectSet&lt;T&gt; from outside the real unit of work class.</span></span> <span data-ttu-id="2682c-350">Вместо этого мы полагаемся на помощью IObjectSet&lt;T&gt; интерфейс, который гораздо проще подделки, но помощью IObjectSet&lt;T&gt; не определяет метод Include.</span><span class="sxs-lookup"><span data-stu-id="2682c-350">Instead, we rely on the IObjectSet&lt;T&gt; interface which is easier to fake, but IObjectSet&lt;T&gt; does not define an Include method.</span></span> <span data-ttu-id="2682c-351">Красота LINQ — это, можно создать собственный оператор Include.</span><span class="sxs-lookup"><span data-stu-id="2682c-351">The beauty of LINQ is that we can create our own Include operator.</span></span>

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

<span data-ttu-id="2682c-352">Обратите внимание на то, как метод расширения определен оператор Include для IQueryable&lt;T&gt; вместо помощью IObjectSet&lt;T&gt;.</span><span class="sxs-lookup"><span data-stu-id="2682c-352">Notice this Include operator is defined as an extension method for IQueryable&lt;T&gt; instead of IObjectSet&lt;T&gt;.</span></span> <span data-ttu-id="2682c-353">Это дает возможность использовать метод с более широкий диапазон возможных типов, включая IQueryable&lt;T&gt;, помощью IObjectSet&lt;T&gt;, ObjectQuery&lt;T&gt;и ObjectSet&lt;T&gt;.</span><span class="sxs-lookup"><span data-stu-id="2682c-353">This gives us the ability to use the method with a wider range of possible types, including IQueryable&lt;T&gt;, IObjectSet&lt;T&gt;, ObjectQuery&lt;T&gt;, and ObjectSet&lt;T&gt;.</span></span> <span data-ttu-id="2682c-354">В случае базовой последовательности, не является подлинной ObjectQuery EF4&lt;T&gt;, то нет никакого вреда Готово, и оператор Include является холостой.</span><span class="sxs-lookup"><span data-stu-id="2682c-354">In the event the underlying sequence is not a genuine EF4 ObjectQuery&lt;T&gt;, then there is no harm done and the Include operator is a no-op.</span></span> <span data-ttu-id="2682c-355">Если основной последовательности *—* ObjectQuery&lt;T&gt; (или производный от ObjectQuery&lt;T&gt;), то EF4 будет см. в разделе требования дополнительные данные и формулировка правильного SQL запрос.</span><span class="sxs-lookup"><span data-stu-id="2682c-355">If the underlying sequence *is* an ObjectQuery&lt;T&gt; (or derived from ObjectQuery&lt;T&gt;), then EF4 will see our requirement for additional data and formulate the proper SQL query.</span></span>

<span data-ttu-id="2682c-356">С помощью этого нового оператора, в месте мы явно запросить диапазон безотложной загрузки сведения из репозитория.</span><span class="sxs-lookup"><span data-stu-id="2682c-356">With this new operator in place we can explicitly request an eager load of time card information from the repository.</span></span>

``` csharp
    public ViewResult Index() {
        var model = _unitOfWork.Employees
                               .Include("TimeCards")
                               .OrderBy(e => e.HireDate);
        return View(model);
    }
```

<span data-ttu-id="2682c-357">При выполнении для реальных ObjectContext, код создает следующий один запрос.</span><span class="sxs-lookup"><span data-stu-id="2682c-357">When run against a real ObjectContext, the code produces the following single query.</span></span> <span data-ttu-id="2682c-358">Запрос собирает достаточно информации из базы данных в один обращения к материализации объектов employee и полностью заполнить свойству их труда.</span><span class="sxs-lookup"><span data-stu-id="2682c-358">The query gathers enough information from the database in one trip to materialize the employee objects and fully populate their TimeCards property.</span></span>

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
         FROM  [dbo].[Employees] AS [Extent1]
         LEFT OUTER JOIN [dbo].[TimeCards] AS [Extent2] ON [Extent1].[Id] = [Extent2].[EmployeeTimeCard_TimeCard_Id]
    )  AS [Project1]
    ORDER BY [Project1].[HireDate] ASC,
             [Project1].[Id] ASC, [Project1].[C1] ASC
```

<span data-ttu-id="2682c-359">Хорошая новость в том, остается полностью тестируемый код внутри метода действия.</span><span class="sxs-lookup"><span data-stu-id="2682c-359">The great news is the code inside the action method remains fully testable.</span></span> <span data-ttu-id="2682c-360">Не требуется предоставить дополнительные компоненты для наших fakes для поддержки оператор Include.</span><span class="sxs-lookup"><span data-stu-id="2682c-360">We don’t need to provide any additional features for our fakes to support the Include operator.</span></span> <span data-ttu-id="2682c-361">Плохая новость заключается в что нам было необходимо использовать оператор Include внутри кода, мы хотели бы сохранить игнорирующих сохраняемость.</span><span class="sxs-lookup"><span data-stu-id="2682c-361">The bad news is we had to use the Include operator inside of the code we wanted to keep persistence ignorant.</span></span> <span data-ttu-id="2682c-362">Это — отличный пример типа необходимых действиях, которые необходимо оценить при построении тестируемого кода.</span><span class="sxs-lookup"><span data-stu-id="2682c-362">This is a prime example of the type of tradeoffs you’ll need to evaluate when building testable code.</span></span> <span data-ttu-id="2682c-363">Бывают случаи, когда необходимо разрешить сохраняемости проблемы утечки за пределами абстракции репозитория в соответствии с целевыми показателями производительности.</span><span class="sxs-lookup"><span data-stu-id="2682c-363">There are times when you need to let persistence concerns leak outside the repository abstraction to meet performance goals.</span></span>

<span data-ttu-id="2682c-364">Альтернативой Безотложная загрузка является отложенной загрузки.</span><span class="sxs-lookup"><span data-stu-id="2682c-364">The alternative to eager loading is lazy loading.</span></span> <span data-ttu-id="2682c-365">Отложенная загрузка означает, что мы делаем *не* требуется код нашей бизнес явно объявить о требование для связанных данных.</span><span class="sxs-lookup"><span data-stu-id="2682c-365">Lazy loading means we do *not* need our business code to explicitly announce the requirement for associated data.</span></span> <span data-ttu-id="2682c-366">Вместо этого мы используем наш сущностей в приложении и ли дополнительные данные, необходимые Entity Framework будет загружать данные по запросу.</span><span class="sxs-lookup"><span data-stu-id="2682c-366">Instead, we use our entities in the application and if additional data is needed Entity Framework will load the data on demand.</span></span>

### <a name="lazy-loading"></a><span data-ttu-id="2682c-367">Отложенная загрузка</span><span class="sxs-lookup"><span data-stu-id="2682c-367">Lazy Loading</span></span>

<span data-ttu-id="2682c-368">Очень просто Представьте себе ситуацию, где мы не знаем, что потребуется данных часть бизнес-логики.</span><span class="sxs-lookup"><span data-stu-id="2682c-368">It’s easy to imagine a scenario where we don’t know what data a piece of business logic will need.</span></span> <span data-ttu-id="2682c-369">Мы знаем, логика должна объект employee, но мы может выполнять переход в разные пути выполнения которых некоторые из этих путей требуют сведения от сотрудника, а некоторые нет.</span><span class="sxs-lookup"><span data-stu-id="2682c-369">We might know the logic needs an employee object, but we may branch into different execution paths where some of those paths require time card information from the employee, and some do not.</span></span> <span data-ttu-id="2682c-370">Такие сценарии, как это идеально подходят для неявного отложенной загрузки, так как данные волшебным образом отображаются по мере необходимости.</span><span class="sxs-lookup"><span data-stu-id="2682c-370">Scenarios like this are perfect for implicit lazy loading because data magically appears on an as-needed basis.</span></span>

<span data-ttu-id="2682c-371">Отложенная загрузка, также известный как отложенная загрузка, поместите некоторые требования объектов сущности.</span><span class="sxs-lookup"><span data-stu-id="2682c-371">Lazy loading, also known as deferred loading, does place some requirements on our entity objects.</span></span> <span data-ttu-id="2682c-372">Любые требования из уровня сохраняемости не сопряжено с POCO с обеспечением сохраняемости значение true, но true сохраняемости практически невозможно достичь.</span><span class="sxs-lookup"><span data-stu-id="2682c-372">POCOs with true persistence ignorance would not face any requirements from the persistence layer, but true persistence ignorance is practically impossible to achieve.</span></span>  <span data-ttu-id="2682c-373">Вместо этого мы измеряем сохраняемости в градусах относительный.</span><span class="sxs-lookup"><span data-stu-id="2682c-373">Instead we measure persistence ignorance in relative degrees.</span></span> <span data-ttu-id="2682c-374">Было бы нежелательным, если нужно наследовать от базового класса ориентированного сохраняемости или использовать специализированные коллекции для достижения отложенной загрузки в POCO.</span><span class="sxs-lookup"><span data-stu-id="2682c-374">It would be unfortunate if we needed to inherit from a persistence oriented base class or use a specialized collection to achieve lazy loading in POCOs.</span></span> <span data-ttu-id="2682c-375">К счастью EF4 содержит щадящим решения.</span><span class="sxs-lookup"><span data-stu-id="2682c-375">Fortunately, EF4 has a less intrusive solution.</span></span>

### <a name="virtually-undetectable"></a><span data-ttu-id="2682c-376">Практически невозможно обнаружить</span><span class="sxs-lookup"><span data-stu-id="2682c-376">Virtually Undetectable</span></span>

<span data-ttu-id="2682c-377">При работе с объектами POCO, EF4 может динамически создавать прокси-серверы среды выполнения для сущностей.</span><span class="sxs-lookup"><span data-stu-id="2682c-377">When using POCO objects, EF4 can dynamically generate runtime proxies for entities.</span></span> <span data-ttu-id="2682c-378">Эти учетные записи-посредники незаметно wrap материализованные POCO и предоставить дополнительные службы, перехватывая каждого свойства, которые получают операций установки и выполнить дополнительные действия.</span><span class="sxs-lookup"><span data-stu-id="2682c-378">These proxies invisibly wrap the materialized POCOs and provide additional services by intercepting each property get and set operation to perform additional work.</span></span> <span data-ttu-id="2682c-379">Из таких служб — это возможность отложенной загрузки, которую мы ищем.</span><span class="sxs-lookup"><span data-stu-id="2682c-379">One such service is the lazy loading feature we are looking for.</span></span> <span data-ttu-id="2682c-380">Другая служба — это эффективный механизм, который можно записать, когда она изменяет значения свойств сущности отслеживания изменений.</span><span class="sxs-lookup"><span data-stu-id="2682c-380">Another service is an efficient change tracking mechanism which can record when the program changes the property values of an entity.</span></span> <span data-ttu-id="2682c-381">Список изменений используется контекст ObjectContext во время выполнения метода SaveChanges для сохранения любого измененных сущностей, с помощью команды обновления.</span><span class="sxs-lookup"><span data-stu-id="2682c-381">The list of changes is used by the ObjectContext during the SaveChanges method to persist any modified entities using UPDATE commands.</span></span>

<span data-ttu-id="2682c-382">Для этих учетных записей-посредников для работы Однако они нужен способ обработки свойства get и набора операций на сущности и прокси-серверы достижения этой цели путем переопределения виртуальных членов.</span><span class="sxs-lookup"><span data-stu-id="2682c-382">For these proxies to work, however, they need a way to hook into property get and set operations on an entity, and the proxies achieve this goal by overriding virtual members.</span></span> <span data-ttu-id="2682c-383">Таким образом Если мы хотим использовать неявную отложенную загрузку и эффективное отслеживание изменений необходимо вернуться к нашей определения классов POCO и пометки свойства как виртуальный.</span><span class="sxs-lookup"><span data-stu-id="2682c-383">Thus, if we want to have implicit lazy loading and efficient change tracking we need to go back to our POCO class definitions and mark properties as virtual.</span></span>

``` csharp
    public class Employee {
        public virtual int Id { get; set; }
        public virtual string Name { get; set; }
        public virtual DateTime HireDate { get; set; }
        public virtual ICollection<TimeCard> TimeCards { get; set; }
    }
```

<span data-ttu-id="2682c-384">Мы по-прежнему может сообщать, что сущность сотрудника является главным образом игнорирующих сохраняемость.</span><span class="sxs-lookup"><span data-stu-id="2682c-384">We can still say the Employee entity is mostly persistence ignorant.</span></span> <span data-ttu-id="2682c-385">Единственное требование заключается в том, чтобы использовать виртуальные члены, и это не влияет на пригодность для тестирования кода.</span><span class="sxs-lookup"><span data-stu-id="2682c-385">The only requirement is to use virtual members and this does not impact the testability of the code.</span></span> <span data-ttu-id="2682c-386">Мы не должны наследовать от любого специального базового класса, или даже использовать специальную подборку, выделенных для отложенной загрузки.</span><span class="sxs-lookup"><span data-stu-id="2682c-386">We don’t need to derive from any special base class, or even use a special collection dedicated to lazy loading.</span></span> <span data-ttu-id="2682c-387">Как показано в коде, любой класс, реализующий интерфейс ICollection&lt;T&gt; доступен для хранения связанных сущностей.</span><span class="sxs-lookup"><span data-stu-id="2682c-387">As the code demonstrates, any class implementing ICollection&lt;T&gt; is available to hold related entities.</span></span>

<span data-ttu-id="2682c-388">Имеется также одним небольшим изменением, что необходимо сделать внутри нашей единицей работы.</span><span class="sxs-lookup"><span data-stu-id="2682c-388">There is also one minor change we need to make inside our unit of work.</span></span> <span data-ttu-id="2682c-389">Отложенная загрузка — это *off* по умолчанию при работе непосредственно с объекта ObjectContext.</span><span class="sxs-lookup"><span data-stu-id="2682c-389">Lazy loading is *off* by default when working directly with an ObjectContext object.</span></span> <span data-ttu-id="2682c-390">Свойства, которые можно установить для свойства ContextOptions для включения отложенной загрузки, и мы можно установить это свойство внутри наших реальных единица работы, если необходимо, чтобы везде отложенной загрузки.</span><span class="sxs-lookup"><span data-stu-id="2682c-390">There is a property we can set on the ContextOptions property to enable deferred loading, and we can set this property inside our real unit of work if we want to enable lazy loading everywhere.</span></span>

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

<span data-ttu-id="2682c-391">Неявные отложенная загрузка включена, код приложения можно использовать сотрудника и сотрудника связанных карт время сохраняя при этом не нужно знать о работы, необходимой для Entity FRAMEWORK загрузить дополнительные данные.</span><span class="sxs-lookup"><span data-stu-id="2682c-391">With implicit lazy loading enabled, application code can use an employee and the employee’s associated time cards while remaining blissfully unaware of the work required for EF to load the extra data.</span></span>

``` csharp
    var employee = _unitOfWork.Employees
                              .Single(e => e.Id == id);
    foreach (var card in employee.TimeCards) {
        // ...
    }
```

<span data-ttu-id="2682c-392">Отложенная загрузка упрощает код приложения для записи и с помощью волшебной команды прокси-сервера остается полностью пригодного для тестирования кода.</span><span class="sxs-lookup"><span data-stu-id="2682c-392">Lazy loading makes the application code easier to write, and with the proxy magic the code remains completely testable.</span></span> <span data-ttu-id="2682c-393">Fakes в памяти, единицы работы просто можно предварительно загрузить фиктивные сущности с связанные данные, при необходимости во время теста.</span><span class="sxs-lookup"><span data-stu-id="2682c-393">In-memory fakes of the unit of work can simply preload fake entities with associated data when needed during a test.</span></span>

<span data-ttu-id="2682c-394">На этом этапе мы обратим наше внимание от создания репозиториев с помощью помощью IObjectSet&lt;T&gt; и посмотрите на абстракции, чтобы скрыть все знаки платформы сохраняемости.</span><span class="sxs-lookup"><span data-stu-id="2682c-394">At this point we’ll turn our attention from building repositories using IObjectSet&lt;T&gt; and look at abstractions to hide all signs of the persistence framework.</span></span>

## <a name="custom-repositories"></a><span data-ttu-id="2682c-395">Пользовательские репозитории</span><span class="sxs-lookup"><span data-stu-id="2682c-395">Custom Repositories</span></span>

<span data-ttu-id="2682c-396">Когда мы впервые представлен шаблон единицы работы конструктора в этой статье мы предоставили пример кода для как может выглядеть единицу работы.</span><span class="sxs-lookup"><span data-stu-id="2682c-396">When we first presented the unit of work design pattern in this article we provided some sample code for what the unit of work might look like.</span></span> <span data-ttu-id="2682c-397">Давайте снова представить эту идею исходного, с помощью сотрудников и карта времени ситуацию с сотрудниками, мы работали с.</span><span class="sxs-lookup"><span data-stu-id="2682c-397">Let’s re-present this original idea using the employee and employee time card scenario we’ve been working with.</span></span>

``` csharp
    public interface IUnitOfWork {
        IRepository<Employee> Employees { get; }
        IRepository<TimeCard> TimeCards { get;  }
        void Commit();
    }
```

<span data-ttu-id="2682c-398">— Это основное различие между этой единицы работы и единицу работы, мы создали в предыдущем разделе, как эта единица работы не использовать такие абстракции, с помощью EF4 framework (имеется не помощью IObjectSet&lt;T&gt;).</span><span class="sxs-lookup"><span data-stu-id="2682c-398">The primary difference between this unit of work and the unit of work we created in the last section is how this unit of work does not use any abstractions from the EF4 framework (there is no IObjectSet&lt;T&gt;).</span></span> <span data-ttu-id="2682c-399">Помощью IObjectSet&lt;T&gt; работает, а также интерфейс репозитория, но он предоставляет API могут быть не согласованы полностью с потребностями приложения.</span><span class="sxs-lookup"><span data-stu-id="2682c-399">IObjectSet&lt;T&gt; works well as a repository interface, but the API it exposes might not perfectly align with our application’s needs.</span></span> <span data-ttu-id="2682c-400">При таком подходе предстоящих будем представлять репозитории, с помощью пользовательских IRepository&lt;T&gt; абстракции.</span><span class="sxs-lookup"><span data-stu-id="2682c-400">In this upcoming approach we will represent repositories using a custom IRepository&lt;T&gt; abstraction.</span></span>

<span data-ttu-id="2682c-401">Многие разработчики, которым следуют разработки через тестирование, проектирование на основе поведения и методологии разработки предметно-ориентированное предпочитают IRepository&lt;T&gt; подход по следующим причинам.</span><span class="sxs-lookup"><span data-stu-id="2682c-401">Many developers who follow test-driven design, behavior-driven design, and domain driven methodologies design prefer the IRepository&lt;T&gt; approach for several reasons.</span></span> <span data-ttu-id="2682c-402">Во-первых, IRepository&lt;T&gt; интерфейс представляет слой «защиты от повреждения».</span><span class="sxs-lookup"><span data-stu-id="2682c-402">First, the IRepository&lt;T&gt; interface represents an “anti-corruption” layer.</span></span> <span data-ttu-id="2682c-403">Как описано в своей книге проблемно-ориентированное проектирование (Eric Evans) уровень защиты от повреждения хранит код предметной области от инфраструктуры API-интерфейсы, такие как API сохраняемости.</span><span class="sxs-lookup"><span data-stu-id="2682c-403">As described by Eric Evans in his Domain Driven Design book an anti-corruption layer keeps your domain code away from infrastructure APIs, like a persistence API.</span></span> <span data-ttu-id="2682c-404">Во-вторых разработчики могут создавать методы в репозиторий, отвечают потребностям приложения, (, обнаруженные во время записи тестов).</span><span class="sxs-lookup"><span data-stu-id="2682c-404">Secondly, developers can build methods into the repository that meet the exact needs of an application (as discovered while writing tests).</span></span> <span data-ttu-id="2682c-405">Например мы часто может потребоваться найти одну сущность, с помощью значений Идентификаторов, поэтому мы можем добавить метод FindById интерфейс репозитория.</span><span class="sxs-lookup"><span data-stu-id="2682c-405">For example, we might frequently need to locate a single entity using an ID value, so we can add a FindById method to the repository interface.</span></span>  <span data-ttu-id="2682c-406">Наши IRepository&lt;T&gt; определение будет выглядеть следующим образом.</span><span class="sxs-lookup"><span data-stu-id="2682c-406">Our IRepository&lt;T&gt; definition will look like the following.</span></span>

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

<span data-ttu-id="2682c-407">Обратите внимание на то, будет отправлено обратно с помощью IQueryable&lt;T&gt; интерфейс для предоставления коллекции сущностей.</span><span class="sxs-lookup"><span data-stu-id="2682c-407">Notice we’ll drop back to using an IQueryable&lt;T&gt; interface to expose entity collections.</span></span> <span data-ttu-id="2682c-408">IQueryable&lt;T&gt; позволяет деревьях выражений LINQ, поступающие в EF4 поставщика и предоставление поставщика целостное представление запроса.</span><span class="sxs-lookup"><span data-stu-id="2682c-408">IQueryable&lt;T&gt; allows LINQ expression trees to flow into the EF4 provider and give the provider a holistic view of the query.</span></span> <span data-ttu-id="2682c-409">Второй вариант будет возвращать объект IEnumerable&lt;T&gt;, это означает, поставщик EF4 LINQ будет видеть только выражения, построенные в репозитории.</span><span class="sxs-lookup"><span data-stu-id="2682c-409">A second option would be to return IEnumerable&lt;T&gt;, which means the EF4 LINQ provider will only see the expressions built inside of the repository.</span></span> <span data-ttu-id="2682c-410">Любой группирования, сортировки и проекции, вне репозитория будет состоять не в команду SQL, отправленную в базу данных, которая может вызвать снижение производительности.</span><span class="sxs-lookup"><span data-stu-id="2682c-410">Any grouping, ordering, and projection done outside of the repository will not be composed into the SQL command sent to the database, which can hurt performance.</span></span> <span data-ttu-id="2682c-411">С другой стороны, хранилище, возвращая только IEnumerable&lt;T&gt; результаты будут никогда не удивляет с помощью новой команды SQL.</span><span class="sxs-lookup"><span data-stu-id="2682c-411">On the other hand, a repository returning only IEnumerable&lt;T&gt; results will never surprise you with a new SQL command.</span></span> <span data-ttu-id="2682c-412">Оба подхода будут работать, и оба подхода остаются пригодного для тестирования.</span><span class="sxs-lookup"><span data-stu-id="2682c-412">Both approaches will work, and both approaches remain testable.</span></span>

<span data-ttu-id="2682c-413">Это просто для предоставления единую реализацию IRepository&lt;T&gt; интерфейс, с помощью универсальных типов и EF4 ObjectContext API.</span><span class="sxs-lookup"><span data-stu-id="2682c-413">It’s straightforward to provide a single implementation of the IRepository&lt;T&gt; interface using generics and the EF4 ObjectContext API.</span></span>

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

<span data-ttu-id="2682c-414">IRepository&lt;T&gt; подход дает нам некоторые дополнительный контроль над наши запросы так, как клиент должен вызвать метод, чтобы перейти к сущности.</span><span class="sxs-lookup"><span data-stu-id="2682c-414">The IRepository&lt;T&gt; approach gives us some additional control over our queries because a client has to invoke a method to get to an entity.</span></span> <span data-ttu-id="2682c-415">Внутри метода мы предоставляем дополнительные проверки и операторы LINQ для принудительного применения ограничения для приложения.</span><span class="sxs-lookup"><span data-stu-id="2682c-415">Inside the method we could provide additional checks and LINQ operators to enforce application constraints.</span></span> <span data-ttu-id="2682c-416">Обратите внимание, что интерфейс имеет два ограничения для параметра универсального типа.</span><span class="sxs-lookup"><span data-stu-id="2682c-416">Notice the interface has two constraints on the generic type parameter.</span></span> <span data-ttu-id="2682c-417">Первое ограничение – недостатки класс нарушить проверку необходимых ObjectSet&lt;T&gt;, и второе ограничение принудительно сущностями для реализации IEntity — это абстракция, созданного для приложения.</span><span class="sxs-lookup"><span data-stu-id="2682c-417">The first constraint is the class cons taint required by ObjectSet&lt;T&gt;, and the second constraint forces our entities to implement IEntity – an abstraction created for the application.</span></span> <span data-ttu-id="2682c-418">Интерфейс IEntity заставляет сущностей в удобном для чтения свойств Id, и можно затем использовать это свойство в метод FindById.</span><span class="sxs-lookup"><span data-stu-id="2682c-418">The IEntity interface forces entities to have a readable Id property, and we can then use this property in the FindById method.</span></span> <span data-ttu-id="2682c-419">IEntity определяется следующим кодом.</span><span class="sxs-lookup"><span data-stu-id="2682c-419">IEntity is defined with the following code.</span></span>

``` csharp
    public interface IEntity {
        int Id { get; }
    }
```

<span data-ttu-id="2682c-420">IEntity можно считать небольшой нарушение сохраняемости, поскольку сущностями необходимы для реализации этого интерфейса.</span><span class="sxs-lookup"><span data-stu-id="2682c-420">IEntity could be considered a small violation of persistence ignorance since our entities are required to implement this interface.</span></span> <span data-ttu-id="2682c-421">Помните, принцип независимости сохраняемости очень о необходимости поиска компромиссов, и для многих функций FindById перевешивают ограничение, наложенное интерфейс.</span><span class="sxs-lookup"><span data-stu-id="2682c-421">Remember persistence ignorance is about tradeoffs, and for many the FindById functionality will outweigh the constraint imposed by the interface.</span></span> <span data-ttu-id="2682c-422">Интерфейс не оказывает влияния на пригодность для тестирования.</span><span class="sxs-lookup"><span data-stu-id="2682c-422">The interface has no impact on testability.</span></span>

<span data-ttu-id="2682c-423">Создание экземпляра динамической IRepository&lt;T&gt; требуется контекст ObjectContext EF4, поэтому конкретные единицы реализации рабочих должен управлять создание экземпляра.</span><span class="sxs-lookup"><span data-stu-id="2682c-423">Instantiating a live IRepository&lt;T&gt; requires an EF4 ObjectContext, so a concrete unit of work implementation should manage the instantiation.</span></span>

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

### <a name="using-the-custom-repository"></a><span data-ttu-id="2682c-424">С помощью пользовательского репозитория</span><span class="sxs-lookup"><span data-stu-id="2682c-424">Using the Custom Repository</span></span>

<span data-ttu-id="2682c-425">С помощью нашего пользовательского репозитория незначительно отличается от использования репозитория, в соответствии с помощью IObjectSet&lt;T&gt;.</span><span class="sxs-lookup"><span data-stu-id="2682c-425">Using our custom repository is not significantly different from using the repository based on IObjectSet&lt;T&gt;.</span></span> <span data-ttu-id="2682c-426">Вместо применения операторов LINQ непосредственно к свойству, мы сначала необходимо вызвать один методы репозитория, чтобы скопировать IQueryable&lt;T&gt; ссылки.</span><span class="sxs-lookup"><span data-stu-id="2682c-426">Instead of applying LINQ operators directly to a property, we’ll first need to invoke one the repository’s methods to grab an IQueryable&lt;T&gt; reference.</span></span>

``` csharp
    public ViewResult Index() {
        var model = _repository.FindAll()
                               .Include("TimeCards")
                               .OrderBy(e => e.HireDate);
        return View(model);
    }
```

<span data-ttu-id="2682c-427">Обратите внимание на то, что пользовательский оператор Include, которую мы применили ранее будет работать без изменений.</span><span class="sxs-lookup"><span data-stu-id="2682c-427">Notice the custom Include operator we implemented previously will work without change.</span></span> <span data-ttu-id="2682c-428">Метод FindById репозитория удаляет повторяющиеся логики из действия, при попытке извлечь одну сущность.</span><span class="sxs-lookup"><span data-stu-id="2682c-428">The repository’s FindById method removes duplicated logic from actions trying to retrieve a single entity.</span></span>

``` csharp
    public ViewResult Details(int id) {
        var model = _repository.FindById(id);
        return View(model);
    }
```

<span data-ttu-id="2682c-429">Нет нет существенной разницы в двух подходов, которые мы рассмотрели возможности тестирования.</span><span class="sxs-lookup"><span data-stu-id="2682c-429">There is no significant difference in the testability of the two approaches we’ve examined.</span></span> <span data-ttu-id="2682c-430">Мы можем решить фиктивных реализаций из IRepository&lt;T&gt; , создав конкретные классы, поддерживаемый HashSet&lt;сотрудника&gt; — так же, как мы сделали в последнем разделе.</span><span class="sxs-lookup"><span data-stu-id="2682c-430">We could provide fake implementations of IRepository&lt;T&gt; by building concrete classes backed by HashSet&lt;Employee&gt; - just like what we did in the last section.</span></span> <span data-ttu-id="2682c-431">Тем не менее некоторые разработчики предпочитают использовать макеты объектов и макетирования платформ объекта вместо сборки fakes.</span><span class="sxs-lookup"><span data-stu-id="2682c-431">However, some developers prefer to use mock objects and mock object frameworks instead of building fakes.</span></span> <span data-ttu-id="2682c-432">Мы рассмотрим использование макетов для тестирования нашей реализации и обсудим различия между макеты и имитаций в следующем разделе.</span><span class="sxs-lookup"><span data-stu-id="2682c-432">We’ll look at using mocks to test our implementation and discuss the differences between mocks and fakes in the next section.</span></span>

### <a name="testing-with-mocks"></a><span data-ttu-id="2682c-433">Тестирование с помощью макетов</span><span class="sxs-lookup"><span data-stu-id="2682c-433">Testing with Mocks</span></span>

<span data-ttu-id="2682c-434">Существуют различные способы создания какие вызовы (Martin Fowler) «тестовый дублер».</span><span class="sxs-lookup"><span data-stu-id="2682c-434">There are different approaches to building what Martin Fowler calls a “test double”.</span></span> <span data-ttu-id="2682c-435">Тестовый дублер (например, фильма stunt double) — это объект, создаваемых «вместо» для реальных рабочих объектов во время выполнения тестов.</span><span class="sxs-lookup"><span data-stu-id="2682c-435">A test double (like a movie stunt double) is an object you build to “stand in” for real, production objects during tests.</span></span> <span data-ttu-id="2682c-436">Репозитории в памяти, созданной нами — тестовых дублеров для репозиториев, которые обращаются к SQL Server.</span><span class="sxs-lookup"><span data-stu-id="2682c-436">The in-memory repositories we created are test doubles for the repositories that talk to SQL Server.</span></span> <span data-ttu-id="2682c-437">Мы узнали, как использовать эти тестовые дублеры во время модульных тестов для изоляции кода и сохранить быстрому запуску тестов.</span><span class="sxs-lookup"><span data-stu-id="2682c-437">We’ve seen how to use these test-doubles during the unit tests to isolate code and keep tests running fast.</span></span>

<span data-ttu-id="2682c-438">Тестовых дублеров, которые мы создали имеют реализации реальных, работа.</span><span class="sxs-lookup"><span data-stu-id="2682c-438">The test doubles we’ve built have real, working implementations.</span></span> <span data-ttu-id="2682c-439">За кулисами каждый из них хранит коллекцию объектов, и они будут добавлять и удалять объекты из данной коллекции, как мы управлять хранилище во время теста.</span><span class="sxs-lookup"><span data-stu-id="2682c-439">Behind the scenes each one stores a concrete collection of objects, and they will add and remove objects from this collection as we manipulate the repository during a test.</span></span> <span data-ttu-id="2682c-440">Некоторые разработчики предпочитаю создавать их тестовых дублеров таким образом — с реальный код и работу реализации.</span><span class="sxs-lookup"><span data-stu-id="2682c-440">Some developers like to build their test doubles this way – with real code and working implementations.</span></span>  <span data-ttu-id="2682c-441">Эти тестовые дублеры так называемые *fakes*.</span><span class="sxs-lookup"><span data-stu-id="2682c-441">These test doubles are what we call *fakes*.</span></span> <span data-ttu-id="2682c-442">Они имеют рабочей реализации, но они не реальных, для использования в рабочей среде.</span><span class="sxs-lookup"><span data-stu-id="2682c-442">They have working implementations, but they aren’t real enough for production use.</span></span> <span data-ttu-id="2682c-443">Имитацией хранилища фактически выполняет запись в базе данных.</span><span class="sxs-lookup"><span data-stu-id="2682c-443">The fake repository doesn’t actually write to the database.</span></span> <span data-ttu-id="2682c-444">Фиктивные SMTP-сервера не фактически отправляет сообщение электронной почты по сети.</span><span class="sxs-lookup"><span data-stu-id="2682c-444">The fake SMTP server doesn’t actually send an email message over the network.</span></span>

### <a name="mocks-versus-fakes"></a><span data-ttu-id="2682c-445">Макеты и имитаций</span><span class="sxs-lookup"><span data-stu-id="2682c-445">Mocks versus Fakes</span></span>

<span data-ttu-id="2682c-446">Есть другой тип теста, double, известный как *макетирование*.</span><span class="sxs-lookup"><span data-stu-id="2682c-446">There is another type of test double known as a *mock*.</span></span> <span data-ttu-id="2682c-447">Хотя fakes рабочий реализаций, макеты поставляются с нет.</span><span class="sxs-lookup"><span data-stu-id="2682c-447">While fakes have working implementations, mocks come with no implementation.</span></span> <span data-ttu-id="2682c-448">С помощью платформы макетов объектов создается во время выполнения эти макеты объектов и использовать их в качестве тестовых дублеров.</span><span class="sxs-lookup"><span data-stu-id="2682c-448">With the help of a mock object framework we construct these mock objects at run time and use them as test doubles.</span></span> <span data-ttu-id="2682c-449">В этом разделе мы будем использовать макетирование framework Moq открытым кодом.</span><span class="sxs-lookup"><span data-stu-id="2682c-449">In this section we’ll be using the open source mocking framework Moq.</span></span> <span data-ttu-id="2682c-450">Ниже приведен простой пример с помощью Moq для динамического создания теста double для репозиторием сотрудника.</span><span class="sxs-lookup"><span data-stu-id="2682c-450">Here is a simple example of using Moq to dynamically create a test double for an employee repository.</span></span>

``` csharp
    Mock<IRepository<Employee>> mock =
        new Mock<IRepository<Employee>>();
    IRepository<Employee> repository = mock.Object;
    repository.Add(new Employee());
    var employee = repository.FindById(1);
```

<span data-ttu-id="2682c-451">Мы запрашиваем IRepository Moq&lt;сотрудника&gt; реализации и он создает его динамически.</span><span class="sxs-lookup"><span data-stu-id="2682c-451">We ask Moq for an IRepository&lt;Employee&gt; implementation and it builds one dynamically.</span></span> <span data-ttu-id="2682c-452">Мы можем получить для объекта, реализующего IRepository&lt;сотрудника&gt; путем обращения к свойству объекта фиктивного объекта&lt;T&gt; объекта.</span><span class="sxs-lookup"><span data-stu-id="2682c-452">We can get to the object implementing IRepository&lt;Employee&gt; by accessing the Object property of the Mock&lt;T&gt; object.</span></span> <span data-ttu-id="2682c-453">Это этот внутренний объект, которые можно передать в наши контроллеры, и он может не знать, если это тестовый дублер или реальные репозитории.</span><span class="sxs-lookup"><span data-stu-id="2682c-453">It is this inner object we can pass into our controllers, and they won’t know if this is a test double or the real repository.</span></span> <span data-ttu-id="2682c-454">Мы можно вызывать методы в объекте, так же, как мы бы вызывать методы на объект с фактической реализации.</span><span class="sxs-lookup"><span data-stu-id="2682c-454">We can invoke methods on the object just like we would invoke methods on an object with a real implementation.</span></span>

<span data-ttu-id="2682c-455">Вы должны спросить, что будете выполнять фиктивного репозитория, когда мы вызываем метод Add.</span><span class="sxs-lookup"><span data-stu-id="2682c-455">You must be wondering what the mock repository will do when we invoke the Add method.</span></span> <span data-ttu-id="2682c-456">Так как не реализацию макета объекта, добавить не выполняет никаких действий.</span><span class="sxs-lookup"><span data-stu-id="2682c-456">Since there is no implementation behind the mock object, Add does nothing.</span></span> <span data-ttu-id="2682c-457">Нет конкретных коллекции за кулисами, как у нас было с помощью fakes, который был записан, чтобы сотрудник удаляется.</span><span class="sxs-lookup"><span data-stu-id="2682c-457">There is no concrete collection behind the scenes like we had with the fakes we wrote, so the employee is discarded.</span></span> <span data-ttu-id="2682c-458">Как насчет возвращаемое значение FindById?</span><span class="sxs-lookup"><span data-stu-id="2682c-458">What about the return value of FindById?</span></span> <span data-ttu-id="2682c-459">В этом случае макета объекта выполняет единственное, что его можно сделать, это возвращаемое значение по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="2682c-459">In this case the mock object does the only thing it can do, which is return a default value.</span></span> <span data-ttu-id="2682c-460">Так как мы возвращается ссылочным типом (сотрудник), возвращается значение null.</span><span class="sxs-lookup"><span data-stu-id="2682c-460">Since we are returning a reference type (an Employee), the return value is a null value.</span></span>

<span data-ttu-id="2682c-461">Макеты может показаться бесполезным; Тем не менее существуют две дополнительные возможности макетов, которые мы не затрагивали.</span><span class="sxs-lookup"><span data-stu-id="2682c-461">Mocks might sound worthless; however, there are two more features of mocks we haven’t talked about.</span></span> <span data-ttu-id="2682c-462">Во-первых Moq framework записывает все вызовы, выполняемые для макета объекта.</span><span class="sxs-lookup"><span data-stu-id="2682c-462">First, the Moq framework records all the calls made on the mock object.</span></span> <span data-ttu-id="2682c-463">Далее в коде мы просим Moq Если любой пользователь вызывается метод Add, или в том случае, если любой пользователь вызывает метод FindById.</span><span class="sxs-lookup"><span data-stu-id="2682c-463">Later in the code we can ask Moq if anyone invoked the Add method, or if anyone invoked the FindById method.</span></span> <span data-ttu-id="2682c-464">Мы рассмотрим, как можно использовать эту функцию «черного ящика» запись в тестах.</span><span class="sxs-lookup"><span data-stu-id="2682c-464">We’ll see later how we can use this “black box” recording feature in tests.</span></span>

<span data-ttu-id="2682c-465">Второй отличная возможность заключается в том, как можно использовать Moq программировать макет объекта с *ожидания*.</span><span class="sxs-lookup"><span data-stu-id="2682c-465">The second great feature is how we can use Moq to program a mock object with *expectations*.</span></span> <span data-ttu-id="2682c-466">Ожидание указывает макета объекта как отвечать на любые данного действия.</span><span class="sxs-lookup"><span data-stu-id="2682c-466">An expectation tells the mock object how to respond to any given interaction.</span></span> <span data-ttu-id="2682c-467">Например мы программировать ожидание в наш макет и сообщить его, чтобы вернуть объект employee, когда кто-то вызывает FindById.</span><span class="sxs-lookup"><span data-stu-id="2682c-467">For example, we can program an expectation into our mock and tell it to return an employee object when someone invokes FindById.</span></span> <span data-ttu-id="2682c-468">Moq инфраструктура использует API установки и лямбда-выражения программировать эти ожидания.</span><span class="sxs-lookup"><span data-stu-id="2682c-468">The Moq framework uses a Setup API and lambda expressions to program these expectations.</span></span>

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

<span data-ttu-id="2682c-469">В этом примере мы просим Moq динамически создать репозиторий, а затем мы используем при программировании репозиторий с Ожидание.</span><span class="sxs-lookup"><span data-stu-id="2682c-469">In this sample we ask Moq to dynamically build a repository, and then we program the repository with an expectation.</span></span> <span data-ttu-id="2682c-470">Исходя из предположения о том, макета объекта для возврата объекта сотрудника с идентификатором 5, когда кто-то вызывает метод FindById, передавая значение 5.</span><span class="sxs-lookup"><span data-stu-id="2682c-470">The expectation tells the mock object to return a new employee object with an Id value of 5 when someone invokes the FindById method passing a value of 5.</span></span> <span data-ttu-id="2682c-471">Этот тест проходит успешно, и нам не нужно создавать полную реализацию, чтобы поддельное IRepository&lt;T&gt;.</span><span class="sxs-lookup"><span data-stu-id="2682c-471">This test passes, and we didn’t need to build a full implementation to fake IRepository&lt;T&gt;.</span></span>

<span data-ttu-id="2682c-472">Давайте вернемся к тестов, который был записан ранее и переработать их на использование макетов вместо fakes.</span><span class="sxs-lookup"><span data-stu-id="2682c-472">Let’s revisit the tests we wrote earlier and rework them to use mocks instead of fakes.</span></span> <span data-ttu-id="2682c-473">Так же, как раньше, мы будем использовать базовый класс для настройки общие части инфраструктуры, которые нужны для всех тестов контроллера.</span><span class="sxs-lookup"><span data-stu-id="2682c-473">Just like before, we’ll use a base class to setup the common pieces of infrastructure we need for all of the controller’s tests.</span></span>

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

<span data-ttu-id="2682c-474">Программный код установки большей части остается неизменным.</span><span class="sxs-lookup"><span data-stu-id="2682c-474">The setup code remains mostly the same.</span></span> <span data-ttu-id="2682c-475">Вместо использования fakes, мы будем использовать Moq для создания макетов объектов.</span><span class="sxs-lookup"><span data-stu-id="2682c-475">Instead of using fakes, we’ll use Moq to construct mock objects.</span></span> <span data-ttu-id="2682c-476">Базовый класс упорядочивает макетов единицу работы для возврата фиктивного репозитория, когда код вызывает свойство сотрудников.</span><span class="sxs-lookup"><span data-stu-id="2682c-476">The base class arranges for the mock unit of work to return a mock repository when code invokes the Employees property.</span></span> <span data-ttu-id="2682c-477">Остальная часть макета настройки будет иметь место внутри средства тестирования, выделенных для каждого конкретного сценария.</span><span class="sxs-lookup"><span data-stu-id="2682c-477">The rest of the mock setup will take place inside the test fixtures dedicated to each specific scenario.</span></span> <span data-ttu-id="2682c-478">Например средство тестирования для действия индекса настроим фиктивного репозитория, возвращающий список сотрудников, когда действие вызывает метод FindAll фиктивного репозитория.</span><span class="sxs-lookup"><span data-stu-id="2682c-478">For example, the test fixture for the Index action will setup the mock repository to return a list of employees when the action invokes the FindAll method of the mock repository.</span></span>

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

<span data-ttu-id="2682c-479">За исключением ожиданий наши тесты выглядеть тесты, которые мы использовали ранее.</span><span class="sxs-lookup"><span data-stu-id="2682c-479">Except for the expectations, our tests look similar to the tests we had before.</span></span> <span data-ttu-id="2682c-480">Тем не менее с возможностью записи макетов платформы мы может приблизить тестирования с другого ракурса.</span><span class="sxs-lookup"><span data-stu-id="2682c-480">However, with the recording ability of a mock framework we can approach testing from a different angle.</span></span> <span data-ttu-id="2682c-481">Мы рассмотрим это новая Перспектива в следующем разделе.</span><span class="sxs-lookup"><span data-stu-id="2682c-481">We’ll look at this new perspective in the next section.</span></span>

### <a name="state-versus-interaction-testing"></a><span data-ttu-id="2682c-482">Состояния и тестирование взаимодействия</span><span class="sxs-lookup"><span data-stu-id="2682c-482">State versus Interaction Testing</span></span>

<span data-ttu-id="2682c-483">Существуют различные методы, которые можно использовать для тестирования программного обеспечения макеты объектов.</span><span class="sxs-lookup"><span data-stu-id="2682c-483">There are different techniques you can use to test software with mock objects.</span></span> <span data-ttu-id="2682c-484">Одним из подходов является использование состояний на основе тестирования, это, что мы проделали в настоящей документации в данный момент.</span><span class="sxs-lookup"><span data-stu-id="2682c-484">One approach is to use state based testing, which is what we have done in this paper so far.</span></span> <span data-ttu-id="2682c-485">Состояние на основе тестирования делает утверждения о состоянии программного обеспечения.</span><span class="sxs-lookup"><span data-stu-id="2682c-485">State based testing makes assertions about the state of the software.</span></span> <span data-ttu-id="2682c-486">В последней проверки мы вызова метода действия контроллера и утверждение о модели, он должен быть построен.</span><span class="sxs-lookup"><span data-stu-id="2682c-486">In the last test we invoked an action method on the controller and made an assertion about the model it should build.</span></span> <span data-ttu-id="2682c-487">Ниже приведены некоторые другие примеры тестирования состояния.</span><span class="sxs-lookup"><span data-stu-id="2682c-487">Here are some other examples of testing state:</span></span>

-   <span data-ttu-id="2682c-488">Убедитесь, что хранилище содержит новый объект employee, после выполнения Create.</span><span class="sxs-lookup"><span data-stu-id="2682c-488">Verify the repository contains the new employee object after Create executes.</span></span>
-   <span data-ttu-id="2682c-489">Убедитесь, что модель содержит список всех сотрудников, после выполнения индекса.</span><span class="sxs-lookup"><span data-stu-id="2682c-489">Verify the model holds a list of all employees after Index executes.</span></span>
-   <span data-ttu-id="2682c-490">Убедитесь, что хранилище не содержит данного сотрудника после выполнения удаления.</span><span class="sxs-lookup"><span data-stu-id="2682c-490">Verify the repository does not contain a given employee after Delete executes.</span></span>

<span data-ttu-id="2682c-491">Другой подход, вы увидите с макеты объектов — для проверки *взаимодействия*.</span><span class="sxs-lookup"><span data-stu-id="2682c-491">Another approach you’ll see with mock objects is to verify *interactions*.</span></span> <span data-ttu-id="2682c-492">Хотя состояния на основе тестирования делает предположения о состоянии объектов, взаимодействие на основе тестирования делает утверждения о взаимодействии объектов.</span><span class="sxs-lookup"><span data-stu-id="2682c-492">While state based testing makes assertions about the state of objects, interaction based testing makes assertions about how objects interact.</span></span> <span data-ttu-id="2682c-493">Пример:</span><span class="sxs-lookup"><span data-stu-id="2682c-493">For example:</span></span>

-   <span data-ttu-id="2682c-494">Убедитесь, что контроллер вызывает метод Add репозитория при выполнении Create.</span><span class="sxs-lookup"><span data-stu-id="2682c-494">Verify the controller invokes the repository’s Add method when Create executes.</span></span>
-   <span data-ttu-id="2682c-495">Убедитесь, что контроллер вызывает метод FindAll репозитория, при выполнении индекса.</span><span class="sxs-lookup"><span data-stu-id="2682c-495">Verify the controller invokes the repository’s FindAll method when Index executes.</span></span>
-   <span data-ttu-id="2682c-496">Убедитесь, что контроллер вызывает единицы работы метода Commit для сохранения изменений, при выполнении редактирования.</span><span class="sxs-lookup"><span data-stu-id="2682c-496">Verify the controller invokes the unit of work’s Commit method to save changes when Edit executes.</span></span>

<span data-ttu-id="2682c-497">Тестирование взаимодействия часто требуется меньше проверочных данных, так как мы не поковыряться внутри коллекции, а также проверка счетчиков.</span><span class="sxs-lookup"><span data-stu-id="2682c-497">Interaction testing often requires less test data, because we aren’t poking inside of collections and verifying counts.</span></span> <span data-ttu-id="2682c-498">Например если мы знаем, что на действие Details вызывает метод FindById репозитория с правильным значением - нажмите действие, вероятно, работает правильно.</span><span class="sxs-lookup"><span data-stu-id="2682c-498">For example, if we know the Details action invokes a repository’s FindById method with the correct value - then the action is probably behaving correctly.</span></span> <span data-ttu-id="2682c-499">Мы можно проверить это без настройки любые тестовые данные для возврата из FindById.</span><span class="sxs-lookup"><span data-stu-id="2682c-499">We can verify this behavior without setting up any test data to return from FindById.</span></span>

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

<span data-ttu-id="2682c-500">Только установки, необходимый в выше средство тестирования состоит в настройке, предоставляемых базовым классом.</span><span class="sxs-lookup"><span data-stu-id="2682c-500">The only setup required in the above test fixture is the setup provided by the base class.</span></span> <span data-ttu-id="2682c-501">Когда вызывается действие контроллера, Moq приводит к записи взаимодействия с фиктивного репозитория.</span><span class="sxs-lookup"><span data-stu-id="2682c-501">When we invoke the controller action, Moq will record the interactions with the mock repository.</span></span> <span data-ttu-id="2682c-502">С помощью API проверки из Moq, мы можете попросить Moq Если контроллера вызываются FindById с правильным значением идентификатора.</span><span class="sxs-lookup"><span data-stu-id="2682c-502">Using the Verify API of Moq, we can ask Moq if the controller invoked FindById with the proper ID value.</span></span> <span data-ttu-id="2682c-503">Если контроллер не происходит вызов метода, или вызывается метод со значением непредвиденный параметр, проверьте метод вызовет исключение, и тест завершится ошибкой.</span><span class="sxs-lookup"><span data-stu-id="2682c-503">If the controller did not invoke the method, or invoked the method with an unexpected parameter value, the Verify method will throw an exception and the test will fail.</span></span>

<span data-ttu-id="2682c-504">Вот еще один пример, чтобы убедиться, что действие создания вызывает фиксации в текущей единице работы.</span><span class="sxs-lookup"><span data-stu-id="2682c-504">Here is another example to verify the Create action invokes Commit on the current unit of work.</span></span>

``` csharp
    [TestMethod]
    public void ShouldCommitUnitOfWork() {
        _controller.Create(_newEmployee);
        _unitOfWork.Verify(u => u.Commit());
    }
```

<span data-ttu-id="2682c-505">Один недостаток тестирование взаимодействия является тенденции к через определяют взаимодействие.</span><span class="sxs-lookup"><span data-stu-id="2682c-505">One danger with interaction testing is the tendency to over specify interactions.</span></span> <span data-ttu-id="2682c-506">Способность макета объекта для записи и убедитесь, что любое взаимодействие с макета объекта не значит, тест следует попытаться проверить каждого диалога.</span><span class="sxs-lookup"><span data-stu-id="2682c-506">The ability of the mock object to record and verify every interaction with the mock object doesn’t mean the test should try to verify every interaction.</span></span> <span data-ttu-id="2682c-507">Некоторые взаимодействия являются подробностями реализации и необходимо только проверить взаимодействия *требуется* для удовлетворения текущего теста.</span><span class="sxs-lookup"><span data-stu-id="2682c-507">Some interactions are implementation details and you should only verify the interactions *required* to satisfy the current test.</span></span>

<span data-ttu-id="2682c-508">Выбор между макеты и имитаций во многом зависит от системы, тестировании и личную (или объединении адаптеров) предпочтения.</span><span class="sxs-lookup"><span data-stu-id="2682c-508">The choice between mocks or fakes largely depends on the system you are testing and your personal (or team) preferences.</span></span> <span data-ttu-id="2682c-509">Макеты объектов может значительно сократить объем кода, необходимо реализовать тестовых дублеров, но не всем удобна, программирование ожиданиям и проверке взаимодействия.</span><span class="sxs-lookup"><span data-stu-id="2682c-509">Mock objects can drastically reduce the amount of code you need to implement test doubles, but not everyone is comfortable programming expectations and verifying interactions.</span></span>

## <a name="conclusions"></a><span data-ttu-id="2682c-510">Выводы</span><span class="sxs-lookup"><span data-stu-id="2682c-510">Conclusions</span></span>

<span data-ttu-id="2682c-511">В этом документе мы рассмотрели несколько подходов к созданию кода для тестирования при использовании ADO.NET Entity Framework для постоянного хранения.</span><span class="sxs-lookup"><span data-stu-id="2682c-511">In this paper we’ve demonstrated several approaches to creating testable code while using the ADO.NET Entity Framework for data persistence.</span></span> <span data-ttu-id="2682c-512">Можно использовать встроенные абстракций наподобие помощью IObjectSet&lt;T&gt;, или создания собственных абстракций наподобие IRepository&lt;T&gt;.</span><span class="sxs-lookup"><span data-stu-id="2682c-512">We can leverage built in abstractions like IObjectSet&lt;T&gt;, or create our own abstractions like IRepository&lt;T&gt;.</span></span>  <span data-ttu-id="2682c-513">В обоих случаях поддержка POCO в Entity Framework ADO.NET 4.0 позволяет пользователям эти абстракции сохраняются неведении и широкими возможностями тестирования.</span><span class="sxs-lookup"><span data-stu-id="2682c-513">In both cases, the POCO support in the ADO.NET Entity Framework 4.0 allows the consumers of these abstractions to remain persistent ignorant and highly testable.</span></span> <span data-ttu-id="2682c-514">Дополнительные возможности EF4 неявную отложенную загрузку позволяет приложению и бизнес-службы, как код работать, не заботясь о деталях реляционного хранилища данных.</span><span class="sxs-lookup"><span data-stu-id="2682c-514">Additional EF4 features like implicit lazy loading allows business and application service code to work without worrying about the details of a relational data store.</span></span> <span data-ttu-id="2682c-515">Наконец эти абстракции, которые будут созданы просты в макетируйте и не Имитируйте внутри модульных тестов, и мы можем использовать эти тестовые дублеры для достижения быстрое выполнение, высокой изолированным и надежные тесты.</span><span class="sxs-lookup"><span data-stu-id="2682c-515">Finally, the abstractions we create are easy to mock or fake inside of unit tests, and we can use these test doubles to achieve fast running, highly isolated, and reliable tests.</span></span>

### <a name="additional-resources"></a><span data-ttu-id="2682c-516">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="2682c-516">Additional Resources</span></span>

-   <span data-ttu-id="2682c-517">Роберт с. Мартин, " [принцип единственной обязанности](http://www.objectmentor.com/resources/articles/srp.pdf)"</span><span class="sxs-lookup"><span data-stu-id="2682c-517">Robert C. Martin, “ [The Single Responsibility Principle](http://www.objectmentor.com/resources/articles/srp.pdf)”</span></span>
-   <span data-ttu-id="2682c-518">(Martin Fowler) [каталог шаблонов](http://www.martinfowler.com/eaaCatalog/index.html) из *шаблоны Enterprise Application Architecture*</span><span class="sxs-lookup"><span data-stu-id="2682c-518">Martin Fowler, [Catalog of Patterns](http://www.martinfowler.com/eaaCatalog/index.html) from *Patterns of Enterprise Application Architecture*</span></span>
-   <span data-ttu-id="2682c-519">Caprio Гриффин " [внедрения зависимостей](https://msdn.microsoft.com/magazine/cc163739.aspx)"</span><span class="sxs-lookup"><span data-stu-id="2682c-519">Griffin Caprio, “ [Dependency Injection](https://msdn.microsoft.com/magazine/cc163739.aspx)”</span></span>
-   <span data-ttu-id="2682c-520">Блог программистов данных, « [Пошаговое руководство: разработка с помощью Entity Framework 4.0 на основе тестирования](http://blogs.msdn.com/adonet/pages/walkthrough-test-driven-development-with-the-entity-framework-4-0.aspx)«.</span><span class="sxs-lookup"><span data-stu-id="2682c-520">Data Programmability Blog, “ [Walkthrough: Test Driven Development with the Entity Framework 4.0](http://blogs.msdn.com/adonet/pages/walkthrough-test-driven-development-with-the-entity-framework-4-0.aspx)”.</span></span>
-   <span data-ttu-id="2682c-521">Блог программистов данных, « [шаблонов с помощью репозитория и единица работы с Entity Framework 4.0](http://blogs.msdn.com/adonet/archive/2009/06/16/using-repository-and-unit-of-work-patterns-with-entity-framework-4-0.aspx)"</span><span class="sxs-lookup"><span data-stu-id="2682c-521">Data Programmability Blog, “ [Using Repository and Unit of Work patterns with Entity Framework 4.0](http://blogs.msdn.com/adonet/archive/2009/06/16/using-repository-and-unit-of-work-patterns-with-entity-framework-4-0.aspx)”</span></span>
-   <span data-ttu-id="2682c-522">Дейв Astels, " [введение BDD](http://blog.daveastels.com/files/BDD_Intro.pdf)"</span><span class="sxs-lookup"><span data-stu-id="2682c-522">Dave Astels, “ [BDD Intro](http://blog.daveastels.com/files/BDD_Intro.pdf)”</span></span>
-   <span data-ttu-id="2682c-523">Йенсен Аарон " [Общие сведения о спецификации машин](http://codebetter.com/blogs/aaron.jensen/archive/2008/05/08/introducing-machine-specifications-or-mspec-for-short.aspx)"</span><span class="sxs-lookup"><span data-stu-id="2682c-523">Aaron Jensen, “ [Introducing Machine Specifications](http://codebetter.com/blogs/aaron.jensen/archive/2008/05/08/introducing-machine-specifications-or-mspec-for-short.aspx)”</span></span>
-   <span data-ttu-id="2682c-524">Эрик ли " [BDD с использованием MSTest](http://blogs.msdn.com/elee/archive/2009/01/20/bdd-with-mstest.aspx)"</span><span class="sxs-lookup"><span data-stu-id="2682c-524">Eric Lee, “ [BDD with MSTest](http://blogs.msdn.com/elee/archive/2009/01/20/bdd-with-mstest.aspx)”</span></span>
-   <span data-ttu-id="2682c-525">(Eric Evans), " [предметно-ориентированное проектирование](http://books.google.com/books?id=7dlaMs0SECsC&printsec=frontcover&dq=evans%20domain%20driven%20design&hl=en&ei=cHztS6C8KIaglAfA_dS1CA&sa=X&oi=book_result&ct=result&resnum=1&ved=0CCoQ6AEwAA)"</span><span class="sxs-lookup"><span data-stu-id="2682c-525">Eric Evans, “ [Domain Driven Design](http://books.google.com/books?id=7dlaMs0SECsC&printsec=frontcover&dq=evans%20domain%20driven%20design&hl=en&ei=cHztS6C8KIaglAfA_dS1CA&sa=X&oi=book_result&ct=result&resnum=1&ved=0CCoQ6AEwAA)”</span></span>
-   <span data-ttu-id="2682c-526">(Martin Fowler) « [макетов не только заглушки](http://martinfowler.com/articles/mocksArentStubs.html)"</span><span class="sxs-lookup"><span data-stu-id="2682c-526">Martin Fowler, “ [Mocks Aren’t Stubs](http://martinfowler.com/articles/mocksArentStubs.html)”</span></span>
-   <span data-ttu-id="2682c-527">(Martin Fowler) « [тестирования двойной](http://martinfowler.com/bliki/TestDouble.html)"</span><span class="sxs-lookup"><span data-stu-id="2682c-527">Martin Fowler, “ [Test Double](http://martinfowler.com/bliki/TestDouble.html)”</span></span>
-   <span data-ttu-id="2682c-528">Джереми Миллер " [состояние и тестирование взаимодействия](http://codebetter.com/blogs/jeremy.miller/articles/129544.aspx)"</span><span class="sxs-lookup"><span data-stu-id="2682c-528">Jeremy Miller, “ [State versus Interaction Testing](http://codebetter.com/blogs/jeremy.miller/articles/129544.aspx)”</span></span>
-   [<span data-ttu-id="2682c-529">MOQ</span><span class="sxs-lookup"><span data-stu-id="2682c-529">Moq</span></span>](http://code.google.com/p/moq/)

### <a name="biography"></a><span data-ttu-id="2682c-530">Биография</span><span class="sxs-lookup"><span data-stu-id="2682c-530">Biography</span></span>

<span data-ttu-id="2682c-531">Скотт Аллен является членом технического персонала в Pluralsight и учредитель OdeToCode.com.</span><span class="sxs-lookup"><span data-stu-id="2682c-531">Scott Allen is a member of the technical staff at Pluralsight and the founder of OdeToCode.com.</span></span> <span data-ttu-id="2682c-532">В 15-летним коммерческой разработки программных продуктов Скотт участвовал в решения для всех операций из 8-разрядных устройств embedded позволяет высокомасштабируемых веб-приложений ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="2682c-532">In 15 years of commercial software development, Scott has worked on solutions for everything from 8-bit embedded devices to highly scalable ASP.NET web applications.</span></span> <span data-ttu-id="2682c-533">Скоттом можно связаться на его блог по адресу OdeToCode или в Твиттере [ http://twitter.com/OdeToCode ](http://twitter.com/OdeToCode).</span><span class="sxs-lookup"><span data-stu-id="2682c-533">You can reach Scott on his blog at OdeToCode, or on Twitter at [http://twitter.com/OdeToCode](http://twitter.com/OdeToCode).</span></span>
