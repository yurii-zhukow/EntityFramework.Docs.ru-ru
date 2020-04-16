---
title: Тестируемость и система сущности 4.0 - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 9430e2ab-261c-4e8e-8545-2ebc52d7a247
ms.openlocfilehash: 96b6b2791f12b7d60a233f7e6dc77e5a8579fb66
ms.sourcegitcommit: 144edccf9b29a7ffad119c235ac9808ec1a46193
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/16/2020
ms.locfileid: "81434317"
---
# <a name="testability-and-entity-framework-40"></a><span data-ttu-id="49dd3-102">Тестируемость и рамочная система сущности 4.0</span><span class="sxs-lookup"><span data-stu-id="49dd3-102">Testability and Entity Framework 4.0</span></span>
<span data-ttu-id="49dd3-103">Скотт Аллен</span><span class="sxs-lookup"><span data-stu-id="49dd3-103">Scott Allen</span></span>

<span data-ttu-id="49dd3-104">Опубликовано: Май 2010</span><span class="sxs-lookup"><span data-stu-id="49dd3-104">Published: May 2010</span></span>

## <a name="introduction"></a><span data-ttu-id="49dd3-105">Введение</span><span class="sxs-lookup"><span data-stu-id="49dd3-105">Introduction</span></span>

<span data-ttu-id="49dd3-106">В этом документе описывается и демонстрируется, как писать тестируемый код с ADO.NET Entity Framework 4.0 и Visual Studio 2010.</span><span class="sxs-lookup"><span data-stu-id="49dd3-106">This white paper describes and demonstrates how to write testable code with the ADO.NET Entity Framework 4.0 and Visual Studio 2010.</span></span> <span data-ttu-id="49dd3-107">В настоящем документе не делается попытка сосредоточиться на конкретной методологии тестирования, такой как тест-ориентированный дизайн (TDD) или дизайн, управляемый поведением (BDD).</span><span class="sxs-lookup"><span data-stu-id="49dd3-107">This paper does not try to focus on a specific testing methodology, like test-driven design (TDD) or behavior-driven design (BDD).</span></span> <span data-ttu-id="49dd3-108">Вместо этого в настоящем документе основное внимание будет уделено тому, как писать код, который использует рамки ADO.NET entity, но остается легко изолировать и тестировать в автоматическом режиме.</span><span class="sxs-lookup"><span data-stu-id="49dd3-108">Instead this paper will focus on how to write code that uses the ADO.NET Entity Framework yet remains easy to isolate and test in an automated fashion.</span></span> <span data-ttu-id="49dd3-109">Мы рассмотрим общие шаблоны проектирования, которые облегчают тестирование в сценариях доступа к данным, и посмотрим, как применять эти шаблоны при использовании платформы.</span><span class="sxs-lookup"><span data-stu-id="49dd3-109">We’ll look at common design patterns that facilitate testing in data access scenarios and see how to apply those patterns when using the framework.</span></span> <span data-ttu-id="49dd3-110">Мы также рассмотрим конкретные особенности платформы, чтобы увидеть, как эти функции могут работать в тестируемом коде.</span><span class="sxs-lookup"><span data-stu-id="49dd3-110">We’ll also look at specific features of the framework to see how those features can work in testable code.</span></span>

## <a name="what-is-testable-code"></a><span data-ttu-id="49dd3-111">Что такое тестируемый код?</span><span class="sxs-lookup"><span data-stu-id="49dd3-111">What Is Testable Code?</span></span>

<span data-ttu-id="49dd3-112">Возможность проверить часть программного обеспечения с помощью автоматизированных модульных тестов предлагает много желательных преимуществ.</span><span class="sxs-lookup"><span data-stu-id="49dd3-112">The ability to verify a piece of software using automated unit tests offers many desirable benefits.</span></span> <span data-ttu-id="49dd3-113">Всем известно, что хорошие тесты уменьшит количество программных дефектов в приложении и увеличат качество приложения, но наличие модульных тестов выходит далеко за рамки простого поиска ошибок.</span><span class="sxs-lookup"><span data-stu-id="49dd3-113">Everyone knows that good tests will reduce the number of software defects in an application and increase the application’s quality - but having unit tests in place goes far beyond just finding bugs.</span></span>

<span data-ttu-id="49dd3-114">Хороший набор модульных тестов позволяет команде разработчиков сэкономить время и сохранить контроль над программным обеспечением, создаваемого ими.</span><span class="sxs-lookup"><span data-stu-id="49dd3-114">A good unit test suite allows a development team to save time and remain in control of the software they create.</span></span> <span data-ttu-id="49dd3-115">Команда может вносить изменения в существующий код, рефакторировать, редизайн и реструктурировать программное обеспечение в соответствии с новыми требованиями, а также добавлять новые компоненты в приложение, зная, что набор тестов может проверить поведение приложения.</span><span class="sxs-lookup"><span data-stu-id="49dd3-115">A team can make changes to existing code, refactor, redesign, and restructure software to meet new requirements, and add new components into an application all while knowing the test suite can verify the application’s behavior.</span></span> <span data-ttu-id="49dd3-116">Единичное тестирование является частью быстрого цикла обратной связи для облегчения изменений и сохранения работоспособности программного обеспечения по мере увеличения сложности.</span><span class="sxs-lookup"><span data-stu-id="49dd3-116">Unit tests are part of a quick feedback cycle to facilitate change and preserve the maintainability of software as complexity increases.</span></span>

<span data-ttu-id="49dd3-117">Модульное тестирование поставляется с ценой, однако.</span><span class="sxs-lookup"><span data-stu-id="49dd3-117">Unit testing comes with a price, however.</span></span> <span data-ttu-id="49dd3-118">Команда должна инвестировать время для создания и поддержания модульных тестов.</span><span class="sxs-lookup"><span data-stu-id="49dd3-118">A team has to invest the time to create and maintain unit tests.</span></span> <span data-ttu-id="49dd3-119">Объем усилий, необходимых для создания этих тестов, напрямую связан с **тестируемостью** базового программного обеспечения.</span><span class="sxs-lookup"><span data-stu-id="49dd3-119">The amount of effort required to create these tests is directly related to the **testability** of the underlying software.</span></span> <span data-ttu-id="49dd3-120">Насколько легко тестировать программное обеспечение?</span><span class="sxs-lookup"><span data-stu-id="49dd3-120">How easy is the software to test?</span></span> <span data-ttu-id="49dd3-121">Команда, разрабатывающая программное обеспечение с учетом тестируемости, создаст эффективные тесты быстрее, чем команда, работающая с нетестируемым программным обеспечением.</span><span class="sxs-lookup"><span data-stu-id="49dd3-121">A team designing software with testability in mind will create effective tests faster than the team working with un-testable software.</span></span>

<span data-ttu-id="49dd3-122">Корпорация Майкрософт разработала ADO.NET Entity Framework 4.0 (EF4) с учетом тестируемости.</span><span class="sxs-lookup"><span data-stu-id="49dd3-122">Microsoft designed the ADO.NET Entity Framework 4.0 (EF4) with testability in mind.</span></span> <span data-ttu-id="49dd3-123">Это не означает, что разработчики будут писать модульные тесты против самого фреймворчного кода.</span><span class="sxs-lookup"><span data-stu-id="49dd3-123">This doesn’t mean developers will be writing unit tests against framework code itself.</span></span> <span data-ttu-id="49dd3-124">Вместо этого цели тестируемости EF4 упрощает создание тестируемого кода, который строится поверх платформы.</span><span class="sxs-lookup"><span data-stu-id="49dd3-124">Instead, the testability goals for EF4 make it easy to create testable code that builds on top of the framework.</span></span> <span data-ttu-id="49dd3-125">Прежде чем мы рассмотрим конкретные примеры, стоит понять качества проверяемого кода.</span><span class="sxs-lookup"><span data-stu-id="49dd3-125">Before we look at specific examples, it’s worthwhile to understand the qualities of testable code.</span></span>

### <a name="the-qualities-of-testable-code"></a><span data-ttu-id="49dd3-126">Качества проверяемого кода</span><span class="sxs-lookup"><span data-stu-id="49dd3-126">The Qualities of Testable Code</span></span>

<span data-ttu-id="49dd3-127">Код, который легко проверить, всегда будет демонстрировать по крайней мере две черты.</span><span class="sxs-lookup"><span data-stu-id="49dd3-127">Code that is easy to test will always exhibit at least two traits.</span></span> <span data-ttu-id="49dd3-128">Во-первых, тестируемый код легко **наблюдать.**</span><span class="sxs-lookup"><span data-stu-id="49dd3-128">First, testable code is easy to **observe**.</span></span> <span data-ttu-id="49dd3-129">Учитывая некоторый набор входов, должно быть легко наблюдать выход кода.</span><span class="sxs-lookup"><span data-stu-id="49dd3-129">Given some set of inputs, it should be easy to observe the output of the code.</span></span> <span data-ttu-id="49dd3-130">Например, тестирование следующего метода легко, потому что метод непосредственно возвращает результат вычисления.</span><span class="sxs-lookup"><span data-stu-id="49dd3-130">For example, testing the following method is easy because the method directly returns the result of a calculation.</span></span>

``` csharp
    public int Add(int x, int y) {
        return x + y;
    }
```

<span data-ttu-id="49dd3-131">Тестирование метода затруднено, если метод записывает вычисленные значения в сетевой розетку, таблицу баз данных или файл, подобный следующему коду.</span><span class="sxs-lookup"><span data-stu-id="49dd3-131">Testing a method is difficult if the method writes the computed value into a network socket, a database table, or a file like the following code.</span></span> <span data-ttu-id="49dd3-132">Тест должен выполнить дополнительную работу для получения значения.</span><span class="sxs-lookup"><span data-stu-id="49dd3-132">The test has to perform additional work to retrieve the value.</span></span>

``` csharp
    public void AddAndSaveToFile(int x, int y) {
         var results = string.Format("The answer is {0}", x + y);
         File.WriteAllText("results.txt", results);
    }
```

<span data-ttu-id="49dd3-133">Во-вторых, тестируемый код легко **изолировать.**</span><span class="sxs-lookup"><span data-stu-id="49dd3-133">Secondly, testable code is easy to **isolate**.</span></span> <span data-ttu-id="49dd3-134">Давайте использовать следующий псевдо-код как плохой пример проверяемого кода.</span><span class="sxs-lookup"><span data-stu-id="49dd3-134">Let’s use the following pseudo-code as a bad example of testable code.</span></span>

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

<span data-ttu-id="49dd3-135">Метод легко наблюдать – мы можем пройти в страховой полис и проверить, что возвратная стоимость соответствует ожидаемому результату.</span><span class="sxs-lookup"><span data-stu-id="49dd3-135">The method is easy to observe – we can pass in an insurance policy and verify the return value matches an expected result.</span></span> <span data-ttu-id="49dd3-136">Однако для тестирования метода нам необходимо установить базу данных с правильной схемой и настроить сервер SMTP на случай, если метод попытается отправить электронное письмо.</span><span class="sxs-lookup"><span data-stu-id="49dd3-136">However, to test the method we’ll need to have a database installed with the correct schema, and configure the SMTP server in case the method tries to send an email.</span></span>

<span data-ttu-id="49dd3-137">Модульный тест требует проверить логику вычисления внутри метода, но тест может выйти из строя, поскольку сервер электронной почты находится в автономном режиме, или из-за перемещения сервера базы данных.</span><span class="sxs-lookup"><span data-stu-id="49dd3-137">The unit test only wants to verify the calculation logic inside the method, but the test might fail because the email server is offline, or because the database server moved.</span></span> <span data-ttu-id="49dd3-138">Оба эти сбоя не связаны с поведением, которые тест хочет проверить.</span><span class="sxs-lookup"><span data-stu-id="49dd3-138">Both of these failures are unrelated to the behavior the test wants to verify.</span></span> <span data-ttu-id="49dd3-139">Поведение трудно изолировать.</span><span class="sxs-lookup"><span data-stu-id="49dd3-139">The behavior is difficult to isolate.</span></span>

<span data-ttu-id="49dd3-140">Разработчики программного обеспечения, которые стремятся писать тестируемый код, часто стремятся сохранить разделение проблем в коде, который они пишут.</span><span class="sxs-lookup"><span data-stu-id="49dd3-140">Software developers who strive to write testable code often strive to maintain a separation of concerns in the code they write.</span></span> <span data-ttu-id="49dd3-141">Вышеупомянутый метод должен быть сосредоточен на бизнес-расчетах и делегировать информацию о реализации базы данных и электронной почты другим компонентам.</span><span class="sxs-lookup"><span data-stu-id="49dd3-141">The above method should focus on the business calculations and delegate the database and email implementation details to other components.</span></span> <span data-ttu-id="49dd3-142">Роберт К. Мартин называет это принципом единой ответственности.</span><span class="sxs-lookup"><span data-stu-id="49dd3-142">Robert C. Martin calls this the Single Responsibility Principle.</span></span> <span data-ttu-id="49dd3-143">Объект должен инкапсулировать одну узкую ответственность, например, расчет стоимости политики.</span><span class="sxs-lookup"><span data-stu-id="49dd3-143">An object should encapsulate a single, narrow responsibility, like calculating the value of a policy.</span></span> <span data-ttu-id="49dd3-144">За все остальные работы по базе данных и уведомлению должна отвечать некий другой объект.</span><span class="sxs-lookup"><span data-stu-id="49dd3-144">All other database and notification work should be the responsibility of some other object.</span></span> <span data-ttu-id="49dd3-145">Код, написанный таким образом, легче изолировать, потому что он ориентирован на одну задачу.</span><span class="sxs-lookup"><span data-stu-id="49dd3-145">Code written in this fashion is easier to isolate because it is focused on a single task.</span></span>

<span data-ttu-id="49dd3-146">В .NET есть абстракции, которые мы должны следовать принципу единой ответственности и достичь изоляции.</span><span class="sxs-lookup"><span data-stu-id="49dd3-146">In .NET we have the abstractions we need to follow the Single Responsibility Principle and achieve isolation.</span></span> <span data-ttu-id="49dd3-147">Мы можем использовать определения интерфейса и заставить код использовать абстракцию интерфейса вместо конкретного типа.</span><span class="sxs-lookup"><span data-stu-id="49dd3-147">We can use interface definitions and force the code to use the interface abstraction instead of a concrete type.</span></span> <span data-ttu-id="49dd3-148">Позже в этой статье мы увидим, как метод, подобный плохому примеру, представленному выше, может работать с интерфейсами, которые *выглядят* так, как будто они будут говорить с базой данных.</span><span class="sxs-lookup"><span data-stu-id="49dd3-148">Later in this paper we’ll see how a method like the bad example presented above can work with interfaces that *look* like they will talk to the database.</span></span> <span data-ttu-id="49dd3-149">Однако в тестовое время мы можем заменить фиктивную реализацию, которая не разговаривает с базой данных, а вместо этого хранит данные в памяти.</span><span class="sxs-lookup"><span data-stu-id="49dd3-149">At test time, however, we can substitute a dummy implementation that doesn’t talk to the database but instead holds data in memory.</span></span> <span data-ttu-id="49dd3-150">Эта фиктивная реализация изолирует код от несвязанных проблем в коде доступа к данным или конфигурации базы данных.</span><span class="sxs-lookup"><span data-stu-id="49dd3-150">This dummy implementation will isolate the code from unrelated problems in the data access code or database configuration.</span></span>

<span data-ttu-id="49dd3-151">Есть дополнительные преимущества изоляции.</span><span class="sxs-lookup"><span data-stu-id="49dd3-151">There are additional benefits to isolation.</span></span> <span data-ttu-id="49dd3-152">Вычисление бизнеса в последнем методе должно занять всего несколько миллисекунд, но сам тест может выполняться в течение нескольких секунд, поскольку код перемещается по сети и разговаривает с различными серверами.</span><span class="sxs-lookup"><span data-stu-id="49dd3-152">The business calculation in the last method should only take a few milliseconds to execute, but the test itself might run for several seconds as the code hops around the network and talks to various servers.</span></span> <span data-ttu-id="49dd3-153">Единие тесты должны работать быстро, чтобы облегчить небольшие изменения.</span><span class="sxs-lookup"><span data-stu-id="49dd3-153">Unit tests should run fast to facilitate small changes.</span></span> <span data-ttu-id="49dd3-154">Единие тесты также должны быть повторяемыми и не проваливаться, поскольку у компонента, не связанного с тестом, есть проблема.</span><span class="sxs-lookup"><span data-stu-id="49dd3-154">Unit tests should also be repeatable and not fail because a component unrelated to the test has a problem.</span></span> <span data-ttu-id="49dd3-155">Написание кода, который легко наблюдать и изолировать, означает, что разработчикам будет легче писать тесты для кода, тратить меньше времени на ожидание выполнения тестов и, что более важно, тратить меньше времени на отслеживание ошибок, которых не существует.</span><span class="sxs-lookup"><span data-stu-id="49dd3-155">Writing code that is easy to observe and to isolate means developers will have an easier time writing tests for the code, spend less time waiting for tests to execute, and more importantly, spend less time tracking down bugs that do not exist.</span></span>

<span data-ttu-id="49dd3-156">Надеюсь, вы сможете оценить преимущества тестирования и понять качества, которые демонстрирует проверяемый код.</span><span class="sxs-lookup"><span data-stu-id="49dd3-156">Hopefully you can appreciate the benefits of testing and understand the qualities that testable code exhibits.</span></span> <span data-ttu-id="49dd3-157">Мы собираемся рассмотреть вопрос о том, как писать код, который работает с EF4, чтобы сохранить данные в базе данных, оставаясь при этом наблюдаемым и легко изолировать, но сначала мы сузим наше внимание, чтобы обсудить тестируемые проекты для доступа к данным.</span><span class="sxs-lookup"><span data-stu-id="49dd3-157">We are about to address how to write code that works with EF4 to save data into a database while remaining observable and easy to isolate, but first we’ll narrow our focus to discuss testable designs for data access.</span></span>

## <a name="design-patterns-for-data-persistence"></a><span data-ttu-id="49dd3-158">Шаблоны проектирования для сохранения данных</span><span class="sxs-lookup"><span data-stu-id="49dd3-158">Design Patterns for Data Persistence</span></span>

<span data-ttu-id="49dd3-159">Оба плохих примера, представленные ранее, имели слишком много обязанностей.</span><span class="sxs-lookup"><span data-stu-id="49dd3-159">Both of the bad examples presented earlier had too many responsibilities.</span></span> <span data-ttu-id="49dd3-160">Первый плохой пример должен был выполнить расчет *и* записать в файл.</span><span class="sxs-lookup"><span data-stu-id="49dd3-160">The first bad example had to perform a calculation *and* write to a file.</span></span> <span data-ttu-id="49dd3-161">Второй плохой пример должен был считывать данные из базы данных *и* выполнять бизнес-расчет *и* отправлять электронную почту.</span><span class="sxs-lookup"><span data-stu-id="49dd3-161">The second bad example had to read data from a database *and* perform a business calculation *and* send email.</span></span> <span data-ttu-id="49dd3-162">Разрабатывая более мелкие методы, разделяющие проблемы и делегирующих ответственность другим компонентам, вы сделаете большие шаги в написании проверяемого кода.</span><span class="sxs-lookup"><span data-stu-id="49dd3-162">By designing smaller methods that separate concerns and delegate responsibility to other components you’ll make great strides towards writing testable code.</span></span> <span data-ttu-id="49dd3-163">Цель состоит в том, чтобы построить функциональность путем составления действий из небольших и целенаправленных абстракций.</span><span class="sxs-lookup"><span data-stu-id="49dd3-163">The goal is to build functionality by composing actions from small and focused abstractions.</span></span>

<span data-ttu-id="49dd3-164">Когда дело доходит до сохранения данных, небольшие и целенаправленные абстракции, которые мы ищем, настолько распространены, что они были задокументированы как шаблоны проектирования.</span><span class="sxs-lookup"><span data-stu-id="49dd3-164">When it comes to data persistence the small and focused abstractions we are looking for are so common they’ve been documented as design patterns.</span></span> <span data-ttu-id="49dd3-165">Книга Мартина Фаулера «Шаблоны архитектуры корпоративных приложений» стала первой работой, описывающей эти закономерности в печатном виде.</span><span class="sxs-lookup"><span data-stu-id="49dd3-165">Martin Fowler’s book Patterns of Enterprise Application Architecture was the first work to describe these patterns in print.</span></span> <span data-ttu-id="49dd3-166">Мы предоставим краткое описание этих шаблонов в следующих разделах, прежде чем мы покажем, как эти ADO.NET Entity Framework реализует и работает с этими шаблонами.</span><span class="sxs-lookup"><span data-stu-id="49dd3-166">We’ll provide a brief description of these patterns in the following sections before we show how these ADO.NET Entity Framework implements and works with these patterns.</span></span>

### <a name="the-repository-pattern"></a><span data-ttu-id="49dd3-167">Шаблон репозитория</span><span class="sxs-lookup"><span data-stu-id="49dd3-167">The Repository Pattern</span></span>

<span data-ttu-id="49dd3-168">Фаулер говорит, что репозиторий "опосредует между доменом и слоями отображения данных с помощью коллекционного интерфейса для доступа к доменным объектам".</span><span class="sxs-lookup"><span data-stu-id="49dd3-168">Fowler says a repository “mediates between the domain and data mapping layers using a collection-like interface for accessing domain objects”.</span></span> <span data-ttu-id="49dd3-169">Цель шаблона репозитория состоит в том, чтобы изолировать код от мелочей доступа к данным, и, как мы видели ранее изоляции является необходимой чертой для тестируемости.</span><span class="sxs-lookup"><span data-stu-id="49dd3-169">The goal of the repository pattern is to isolate code from the minutiae of data access, and as we saw earlier isolation is a required trait for testability.</span></span>

<span data-ttu-id="49dd3-170">Ключом к изоляции является то, как репозиторий предоставляет объекты с помощью интерфейса, похожего на сбор.</span><span class="sxs-lookup"><span data-stu-id="49dd3-170">The key to the isolation is how the repository exposes objects using a collection-like interface.</span></span> <span data-ttu-id="49dd3-171">Логика, которую вы пишете для использования репозитория, не имеет ни малейшего представления о том, как репозиторий материализует объекты, которые вы запрашиваете.</span><span class="sxs-lookup"><span data-stu-id="49dd3-171">The logic you write to use the repository has no idea how the repository will materialize the objects you request.</span></span> <span data-ttu-id="49dd3-172">Репозиторий может говорить с базой данных, или он может просто вернуть объекты из коллекции в памяти.</span><span class="sxs-lookup"><span data-stu-id="49dd3-172">The repository might talk to a database, or it might just return objects from an in-memory collection.</span></span> <span data-ttu-id="49dd3-173">Все, что нужно знать, это то, что репозиторий, как представляется, поддерживает коллекцию, и вы можете получить, добавить и удалить объекты из коллекции.</span><span class="sxs-lookup"><span data-stu-id="49dd3-173">All your code needs to know is that the repository appears to maintain the collection, and you can retrieve, add, and delete objects from the collection.</span></span>

<span data-ttu-id="49dd3-174">В существующих приложениях .NET конкретный репозиторий часто наследует от общего интерфейса, как следующее:</span><span class="sxs-lookup"><span data-stu-id="49dd3-174">In existing .NET applications a concrete repository often inherits from a generic interface like the following:</span></span>

``` csharp
    public interface IRepository<T> {       
        IEnumerable<T> FindAll();
        IEnumerable<T> FindBy(Expression<Func\<T, bool>> predicate);
        T FindById(int id);
        void Add(T newEntity);
        void Remove(T entity);
    }
```

<span data-ttu-id="49dd3-175">Мы внедрим несколько изменений в определение интерфейса, когда предоставим реализацию для EF4, но основная концепция останется прежней.</span><span class="sxs-lookup"><span data-stu-id="49dd3-175">We’ll make a few changes to the interface definition when we provide an implementation for EF4, but the basic concept remains the same.</span></span> <span data-ttu-id="49dd3-176">Код может использовать конкретный репозиторий, реализующий этот интерфейс, для извлечения сущности по ее основному ключевому значению, для получения коллекции сущностей на основе оценки предиката или просто для получения всех доступных сущностей.</span><span class="sxs-lookup"><span data-stu-id="49dd3-176">Code can use a concrete repository implementing this interface to retrieve an entity by its primary key value, to retrieve a collection of entities based on the evaluation of a predicate, or simply retrieve all available entities.</span></span> <span data-ttu-id="49dd3-177">Код также может добавлять и удалять сущности через интерфейс репозитория.</span><span class="sxs-lookup"><span data-stu-id="49dd3-177">The code can also add and remove entities through the repository interface.</span></span>

<span data-ttu-id="49dd3-178">С учетом IRepository объектов сотрудника код может выполнять следующие операции.</span><span class="sxs-lookup"><span data-stu-id="49dd3-178">Given an IRepository of Employee objects, code can perform the following operations.</span></span>

``` csharp
    var employeesNamedScott =
        repository
            .FindBy(e => e.Name == "Scott")
            .OrderBy(e => e.HireDate);
    var firstEmployee = repository.FindById(1);
    var newEmployee = new Employee() {/*... */};
    repository.Add(newEmployee);
```

<span data-ttu-id="49dd3-179">Поскольку код использует интерфейс (IRepository of Employee), мы можем предоставить код с различными реализациями интерфейса.</span><span class="sxs-lookup"><span data-stu-id="49dd3-179">Since the code is using an interface (IRepository of Employee), we can provide the code with different implementations of the interface.</span></span> <span data-ttu-id="49dd3-180">Одной из реализаций может быть реализация, поддерживаемая EF4, и сохраняющиеся объекты в базе данных Microsoft S'L Server.</span><span class="sxs-lookup"><span data-stu-id="49dd3-180">One implementation might be an implementation backed by EF4 and persisting objects into a Microsoft SQL Server database.</span></span> <span data-ttu-id="49dd3-181">Другая реализация (та, которую мы используем во время тестирования), может быть подкреплена списком объектов сотрудника в памяти.</span><span class="sxs-lookup"><span data-stu-id="49dd3-181">A different implementation (one we use during testing) might be backed by an in-memory List of Employee objects.</span></span> <span data-ttu-id="49dd3-182">Интерфейс поможет достичь изоляции в коде.</span><span class="sxs-lookup"><span data-stu-id="49dd3-182">The interface will help to achieve isolation in the code.</span></span>

<span data-ttu-id="49dd3-183">Обратите внимание, что&lt;интерфейс&gt; IRepository T не предоставляет операцию «Сохранение».</span><span class="sxs-lookup"><span data-stu-id="49dd3-183">Notice the IRepository&lt;T&gt; interface does not expose a Save operation.</span></span> <span data-ttu-id="49dd3-184">Как обновить существующие объекты?</span><span class="sxs-lookup"><span data-stu-id="49dd3-184">How do we update existing objects?</span></span> <span data-ttu-id="49dd3-185">Вы можете встретить определения IRepository, которые включают операцию «Сохранение», и реализация этих репозиторий должна будет немедленно упорствовать объекта в базу данных.</span><span class="sxs-lookup"><span data-stu-id="49dd3-185">You might come across IRepository definitions that do include the Save operation, and implementations of these repositories will need to immediately persist an object into the database.</span></span> <span data-ttu-id="49dd3-186">Однако во многих приложениях мы не хотим сохранять объекты по отдельности.</span><span class="sxs-lookup"><span data-stu-id="49dd3-186">However, in many applications we don’t want to persist objects individually.</span></span> <span data-ttu-id="49dd3-187">Вместо этого мы хотим оживить объекты, возможно, из различных хранилищ, изменить эти объекты как часть деловой деятельности, а затем сохранить все объекты в рамках одной атомной операции.</span><span class="sxs-lookup"><span data-stu-id="49dd3-187">Instead, we want to bring objects to life, perhaps from different repositories, modify those objects as part of a business activity, and then persist all the objects as part of a single, atomic operation.</span></span> <span data-ttu-id="49dd3-188">К счастью, существует шаблон, позволяющий такого рода поведение.</span><span class="sxs-lookup"><span data-stu-id="49dd3-188">Fortunately, there is a pattern to allow this type of behavior.</span></span>

### <a name="the-unit-of-work-pattern"></a><span data-ttu-id="49dd3-189">Единица рабочей модели</span><span class="sxs-lookup"><span data-stu-id="49dd3-189">The Unit of Work Pattern</span></span>

<span data-ttu-id="49dd3-190">Фаулер говорит, что единица работы будет "поддерживать список объектов, пострадавших от бизнес-транзакции и координирует написание из изменений и решение проблем с параллелью".</span><span class="sxs-lookup"><span data-stu-id="49dd3-190">Fowler says a unit of work will “maintain a list of objects affected by a business transaction and coordinates the writing out of changes and the resolution of concurrency problems”.</span></span> <span data-ttu-id="49dd3-191">Это ответственность единицы работы, чтобы отслеживать изменения объектов, которые мы вора в жизнь из репозитория и сохранять любые изменения, которые мы внесли в объекты, когда мы говорим единицы работы, чтобы совершить изменения.</span><span class="sxs-lookup"><span data-stu-id="49dd3-191">It is the responsibility of the unit of work to track changes to the objects we bring to life from a repository and persist any changes we’ve made to the objects when we tell the unit of work to commit the changes.</span></span> <span data-ttu-id="49dd3-192">Кроме того, подразделение работы несет ответственность за удаление новых объектов, которые мы добавили во все репозитории, и вставить объекты в базу данных, а также удаление mange.</span><span class="sxs-lookup"><span data-stu-id="49dd3-192">It’s also the responsibility of the unit of work to take the new objects we’ve added to all repositories and insert the objects into a database, and also mange deletion.</span></span>

<span data-ttu-id="49dd3-193">Если вы когда-либо делали какую-либо работу с ADO.NET DataSets, то вы уже знакомы с единицей шаблона работы.</span><span class="sxs-lookup"><span data-stu-id="49dd3-193">If you’ve ever done any work with ADO.NET DataSets then you’ll already be familiar with the unit of work pattern.</span></span> <span data-ttu-id="49dd3-194">ADO.NET DataSets имел возможность отслеживать наши обновления, удаления и вставки объектов DataRow и мог (с помощью TableAdapter) согласовать все наши изменения в базе данных.</span><span class="sxs-lookup"><span data-stu-id="49dd3-194">ADO.NET DataSets had the ability to track our updates, deletions, and insertion of DataRow objects and could (with the help of a TableAdapter) reconcile all our changes to a database.</span></span> <span data-ttu-id="49dd3-195">Однако объекты DataSet моделируются в отключенном подмножестве базовой базы данных.</span><span class="sxs-lookup"><span data-stu-id="49dd3-195">However, DataSet objects model a disconnected subset of the underlying database.</span></span> <span data-ttu-id="49dd3-196">Единица рабочего шаблона демонстрирует одно и то же поведение, но работает с бизнес-объектами и доменными объектами, которые изолированы от кода доступа к данным и не знают о базе данных.</span><span class="sxs-lookup"><span data-stu-id="49dd3-196">The unit of work pattern exhibits the same behavior, but works with business objects and domain objects that are isolated from data access code and unaware of the database.</span></span>

<span data-ttu-id="49dd3-197">Абстракция для моделирования единицы работы в коде .NET может выглядеть следующим образом:</span><span class="sxs-lookup"><span data-stu-id="49dd3-197">An abstraction to model the unit of work in .NET code might look like the following:</span></span>

``` csharp
    public interface IUnitOfWork {
        IRepository<Employee> Employees { get; }
        IRepository<Order> Orders { get; }
        IRepository<Customer> Customers { get; }
        void Commit();
    }
```

<span data-ttu-id="49dd3-198">Разоблачая ссылки репозитория из единицы работы, мы можем обеспечить, чтобы единая единица рабочего объекта имеет возможность отслеживать все сущности, материализованные во время бизнес-транзакции.</span><span class="sxs-lookup"><span data-stu-id="49dd3-198">By exposing repository references from the unit of work we can ensure a single unit of work object has the ability to track all entities materialized during a business transaction.</span></span> <span data-ttu-id="49dd3-199">Реализация метода Commit для реальной единицы работы, где все волшебство происходит, чтобы примирить в памяти изменения с базой данных.</span><span class="sxs-lookup"><span data-stu-id="49dd3-199">The implementation of the Commit method for a real unit of work is where all the magic happens to reconcile in-memory changes with the database.</span></span> 

<span data-ttu-id="49dd3-200">С учетом ссылки IUnitOfWork код может вносить изменения в бизнес-объекты, извлеченные из одного или нескольких репозиторий, и сохранять все изменения с помощью атомной операции Commit.</span><span class="sxs-lookup"><span data-stu-id="49dd3-200">Given an IUnitOfWork reference, code can make changes to business objects retrieved from one or more repositories and save all the changes using the atomic Commit operation.</span></span>

``` csharp
    var firstEmployee = unitofWork.Employees.FindById(1);
    var firstCustomer = unitofWork.Customers.FindById(1);
    firstEmployee.Name = "Alex";
    firstCustomer.Name = "Christopher";
    unitofWork.Commit();
```

### <a name="the-lazy-load-pattern"></a><span data-ttu-id="49dd3-201">Шаблон ленивой нагрузки</span><span class="sxs-lookup"><span data-stu-id="49dd3-201">The Lazy Load Pattern</span></span>

<span data-ttu-id="49dd3-202">Фаулер использует название ленивая нагрузка для описания "объект, который не содержит все данные, которые вам нужны, но знает, как получить его".</span><span class="sxs-lookup"><span data-stu-id="49dd3-202">Fowler uses the name lazy load to describe “an object that doesn’t contain all of the data you need but knows how to get it”.</span></span> <span data-ttu-id="49dd3-203">Прозрачная ленивая загрузка является важной особенностью при написании проверяемого бизнес-кода и работе с реляционной базой данных.</span><span class="sxs-lookup"><span data-stu-id="49dd3-203">Transparent lazy loading is an important feature to have when writing testable business code and working with a relational database.</span></span> <span data-ttu-id="49dd3-204">В качестве примера рассмотрим следующий код.</span><span class="sxs-lookup"><span data-stu-id="49dd3-204">As an example, consider the following code.</span></span>

``` csharp
    var employee = repository.FindById(id);
    // ... and later ...
    foreach(var timeCard in employee.TimeCards) {
        // .. manipulate the timeCard
    }
```

<span data-ttu-id="49dd3-205">Как заполняется коллекция TimeCards?</span><span class="sxs-lookup"><span data-stu-id="49dd3-205">How is the TimeCards collection populated?</span></span> <span data-ttu-id="49dd3-206">Есть два возможных ответа.</span><span class="sxs-lookup"><span data-stu-id="49dd3-206">There are two possible answers.</span></span> <span data-ttu-id="49dd3-207">Один из ответов заключается в том, что репозиторий сотрудника, когда его просят забрать сотрудника, выдает запрос для получения как сотрудника, так и связанной с сотрудником информации о временной карте.</span><span class="sxs-lookup"><span data-stu-id="49dd3-207">One answer is that the employee repository, when asked to fetch an employee, issues a query to retrieve both the employee along with the employee’s associated time card information.</span></span> <span data-ttu-id="49dd3-208">В реляционных базах данных это обычно требует запроса с оговоркой JOIN и может привести к получению больше информации, чем требует приложение.</span><span class="sxs-lookup"><span data-stu-id="49dd3-208">In relational databases this generally requires a query with a JOIN clause and may result in retrieving more information than an application needs.</span></span> <span data-ttu-id="49dd3-209">Что делать, если приложение никогда не нужно касаться свойства TimeCards?</span><span class="sxs-lookup"><span data-stu-id="49dd3-209">What if the application never needs to touch the TimeCards property?</span></span>

<span data-ttu-id="49dd3-210">Второй ответ заключается в загрузке свойства TimeCards "по требованию".</span><span class="sxs-lookup"><span data-stu-id="49dd3-210">A second answer is to load the TimeCards property “on demand”.</span></span> <span data-ttu-id="49dd3-211">Эта ленивая загрузка неясна и прозрачна для бизнес-логики, поскольку код не вызывает специальные AA для получения информации о карточке времени.</span><span class="sxs-lookup"><span data-stu-id="49dd3-211">This lazy loading is implicit and transparent to the business logic because the code does not invoke special APIs to retrieve time card information.</span></span> <span data-ttu-id="49dd3-212">Код предполагает, что информация о карте времени присутствует при необходимости.</span><span class="sxs-lookup"><span data-stu-id="49dd3-212">The code assumes the time card information is present when needed.</span></span> <span data-ttu-id="49dd3-213">Существует некоторая магия, связанная с ленивым погрузки, которая обычно включает в себя перехват времени выполнения метода вызовов.</span><span class="sxs-lookup"><span data-stu-id="49dd3-213">There is some magic involved with lazy loading that generally involves runtime interception of method invocations.</span></span> <span data-ttu-id="49dd3-214">Код перехвата отвечает за разговор с базой данных и получение информации о карточке времени, оставляя бизнес-логику свободной для бизнес-логики.</span><span class="sxs-lookup"><span data-stu-id="49dd3-214">The intercepting code is responsible for talking to the database and retrieving time card information while leaving the business logic free to be business logic.</span></span> <span data-ttu-id="49dd3-215">Эта ленивая магия нагрузки позволяет бизнес-коду изолировать себя от операций поиска данных и приводит к более проверяемому коду.</span><span class="sxs-lookup"><span data-stu-id="49dd3-215">This lazy load magic allows the business code to isolate itself from data retrieval operations and results in more testable code.</span></span>

<span data-ttu-id="49dd3-216">Недостатком ленивой нагрузки является то, что, когда приложению *действительно* нужна информация о карточке времени, код выполняет дополнительный запрос.</span><span class="sxs-lookup"><span data-stu-id="49dd3-216">The drawback to a lazy load is that when an application *does* need the time card information the code will execute an additional query.</span></span> <span data-ttu-id="49dd3-217">Это не является проблемой для многих приложений, но для чувствительных к производительности приложений или приложений цикл через ряд объектов сотрудника и выполнения запроса для получения временных карт во время каждой итерации цикла (проблема часто называют n-1 проблемы запроса), ленивая загрузка перетаскивания.</span><span class="sxs-lookup"><span data-stu-id="49dd3-217">This isn’t a concern for many applications, but for performance sensitive applications or applications looping through a number of employee objects and executing a query to retrieve time cards during each iteration of the loop (a problem often referred to as the N+1 query problem), lazy loading is a drag.</span></span> <span data-ttu-id="49dd3-218">В этих сценариях приложение может захотеть загрузить информацию о карточке времени наиболее эффективным способом.</span><span class="sxs-lookup"><span data-stu-id="49dd3-218">In these scenarios an application might want to eagerly load time card information in the most efficient manner possible.</span></span>

<span data-ttu-id="49dd3-219">К счастью, мы увидим, как EF4 поддерживает как неявные ленивые нагрузки, так и эффективные нагрузки, которые мы перейдем к следующему разделу и реализуем эти шаблоны.</span><span class="sxs-lookup"><span data-stu-id="49dd3-219">Fortunately, we’ll see how EF4 supports both implicit lazy loads and efficient eager loads as we move into the next section and implement these patterns.</span></span>

## <a name="implementing-patterns-with-the-entity-framework"></a><span data-ttu-id="49dd3-220">Внедрение шаблонов с рамочной системой образований</span><span class="sxs-lookup"><span data-stu-id="49dd3-220">Implementing Patterns with the Entity Framework</span></span>

<span data-ttu-id="49dd3-221">Хорошей новостью является то, что все шаблоны проектирования, описанные в последнем разделе, просты в реализации с EF4.</span><span class="sxs-lookup"><span data-stu-id="49dd3-221">The good news is that all of the design patterns we described in the last section are straightforward to implement with EF4.</span></span> <span data-ttu-id="49dd3-222">Чтобы продемонстрировать, что мы собираемся использовать простое приложение ASP.NET MVC для отображения и отображения информации о сотрудниках и связанных с ними данных о карточках времени.</span><span class="sxs-lookup"><span data-stu-id="49dd3-222">To demonstrate we are going to use a simple ASP.NET MVC application to edit and display Employees and their associated time card information.</span></span> <span data-ttu-id="49dd3-223">Начнем с использования следующих "обычных старых объектов CLR" (POCOs).</span><span class="sxs-lookup"><span data-stu-id="49dd3-223">We’ll start by using the following “plain old CLR objects” (POCOs).</span></span> 

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

<span data-ttu-id="49dd3-224">Эти определения классов немного изменятся по мере того, как мы исследуем различные подходы и особенности EF4, но цель состоит в том, чтобы сохранить эти классы как сохранение невежественным (PI), насколько это возможно.</span><span class="sxs-lookup"><span data-stu-id="49dd3-224">These class definitions will change slightly as we explore different approaches and features of EF4, but the intent is to keep these classes as persistence ignorant (PI) as possible.</span></span> <span data-ttu-id="49dd3-225">Объект PI не *знает, как*или даже *если*состояние, в которое он содержится, живет внутри базы данных.</span><span class="sxs-lookup"><span data-stu-id="49dd3-225">A PI object doesn’t know *how*, or even *if*, the state it holds lives inside a database.</span></span> <span data-ttu-id="49dd3-226">PI и POCOs идут рука об руку с тестируемым программным обеспечением.</span><span class="sxs-lookup"><span data-stu-id="49dd3-226">PI and POCOs go hand in hand with testable software.</span></span> <span data-ttu-id="49dd3-227">Объекты, использующие подход POCO, менее ограничены, более гибки и легче тестируются, поскольку они могут работать без присутствия базы данных.</span><span class="sxs-lookup"><span data-stu-id="49dd3-227">Objects using a POCO approach are less constrained, more flexible, and easier to test because they can operate without a database present.</span></span>

<span data-ttu-id="49dd3-228">С POCOs на месте мы можем создать модель данных entity (EDM) в Visual Studio (см. рисунок 1).</span><span class="sxs-lookup"><span data-stu-id="49dd3-228">With the POCOs in place we can create an Entity Data Model (EDM) in Visual Studio (see figure 1).</span></span> <span data-ttu-id="49dd3-229">Мы не будем использовать EDM для создания кода для наших сущностей.</span><span class="sxs-lookup"><span data-stu-id="49dd3-229">We will not use the EDM to generate code for our entities.</span></span> <span data-ttu-id="49dd3-230">Вместо этого, мы хотим использовать сущности, которые мы с любовью ремесла вручную.</span><span class="sxs-lookup"><span data-stu-id="49dd3-230">Instead, we want to use the entities we lovingly craft by hand.</span></span> <span data-ttu-id="49dd3-231">Мы будем использовать EDM только для создания схемы базы данных и предоставления метаданных EF4, необходимых для картирования объектов в базе данных.</span><span class="sxs-lookup"><span data-stu-id="49dd3-231">We will only use the EDM to generate our database schema and provide the metadata EF4 needs to map objects into the database.</span></span>

![ef test_01](~/ef6/media/eftest-01.jpg)

<span data-ttu-id="49dd3-233">**Рисунок 1**</span><span class="sxs-lookup"><span data-stu-id="49dd3-233">**Figure 1**</span></span>

<span data-ttu-id="49dd3-234">Примечание: если вы хотите сначала разработать модель EDM, можно создать чистый код POCO из EDM.</span><span class="sxs-lookup"><span data-stu-id="49dd3-234">Note: if you want to develop the EDM model first, it is possible to generate clean, POCO code from the EDM.</span></span> <span data-ttu-id="49dd3-235">Вы можете сделать это с помощью расширения Visual Studio 2010, предоставленного командой Data Programmability.</span><span class="sxs-lookup"><span data-stu-id="49dd3-235">You can do this with a Visual Studio 2010 extension provided by the Data Programmability team.</span></span> <span data-ttu-id="49dd3-236">Чтобы загрузить расширение, запустите менеджер расширения из меню Tools в Visual Studio и ищите онлайн-галерею шаблонов для "POCO" (см. рисунок 2).</span><span class="sxs-lookup"><span data-stu-id="49dd3-236">To download the extension, launch the Extension Manager from the Tools menu in Visual Studio and search the online gallery of templates for “POCO” (See figure 2).</span></span> <span data-ttu-id="49dd3-237">Есть несколько шаблонов POCO, доступных для EF.</span><span class="sxs-lookup"><span data-stu-id="49dd3-237">There are several POCO templates available for EF.</span></span> <span data-ttu-id="49dd3-238">Для получения дополнительной информации об [Walkthrough: POCO Template for the Entity Framework](https://docs.microsoft.com/archive/blogs/adonet/walkthrough-poco-template-for-the-entity-framework)использовании шаблона см.</span><span class="sxs-lookup"><span data-stu-id="49dd3-238">For more information on using the template, see “ [Walkthrough: POCO Template for the Entity Framework](https://docs.microsoft.com/archive/blogs/adonet/walkthrough-poco-template-for-the-entity-framework)”.</span></span>

![ef test_02](~/ef6/media/eftest-02.png)

<span data-ttu-id="49dd3-240">**Рисунок 2**</span><span class="sxs-lookup"><span data-stu-id="49dd3-240">**Figure 2**</span></span>

<span data-ttu-id="49dd3-241">С этой отправной точки POCO мы рассмотрим два различных подхода к тестируемому коду.</span><span class="sxs-lookup"><span data-stu-id="49dd3-241">From this POCO starting point we will explore two different approaches to testable code.</span></span> <span data-ttu-id="49dd3-242">Первый подход я называю подходом EF, поскольку он использует абстракции из API Системы сущностей для реализации единиц работы и репозиторий.</span><span class="sxs-lookup"><span data-stu-id="49dd3-242">The first approach I call the EF approach because it leverages abstractions from the Entity Framework API to implement units of work and repositories.</span></span> <span data-ttu-id="49dd3-243">Во втором подходе мы создадим наши собственные абстракции репозитория, а затем увидим преимущества и недостатки каждого подхода.</span><span class="sxs-lookup"><span data-stu-id="49dd3-243">In the second approach we will create our own custom repository abstractions and then see the advantages and disadvantages of each approach.</span></span> <span data-ttu-id="49dd3-244">Начнем с изучения подхода EF.</span><span class="sxs-lookup"><span data-stu-id="49dd3-244">We’ll start by exploring the EF approach.</span></span>  

### <a name="an-ef-centric-implementation"></a><span data-ttu-id="49dd3-245">Реализация ef Centric</span><span class="sxs-lookup"><span data-stu-id="49dd3-245">An EF Centric Implementation</span></span>

<span data-ttu-id="49dd3-246">Рассмотрим следующее действие контроллера из проекта ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="49dd3-246">Consider the following controller action from an ASP.NET MVC project.</span></span> <span data-ttu-id="49dd3-247">Действие извлекает объект сотрудника и возвращает результат для отображения подробного представления сотрудника.</span><span class="sxs-lookup"><span data-stu-id="49dd3-247">The action retrieves an Employee object and returns a result to display a detailed view of the employee.</span></span>

``` csharp
    public ViewResult Details(int id) {
        var employee = _unitOfWork.Employees
                                  .Single(e => e.Id == id);
        return View(employee);
    }
```

<span data-ttu-id="49dd3-248">Тестируемый ли код?</span><span class="sxs-lookup"><span data-stu-id="49dd3-248">Is the code testable?</span></span> <span data-ttu-id="49dd3-249">Есть по крайней мере два теста, которые нам нужно проверить поведение действия.</span><span class="sxs-lookup"><span data-stu-id="49dd3-249">There are at least two tests we’d need to verify the action’s behavior.</span></span> <span data-ttu-id="49dd3-250">Во-первых, мы хотели бы проверить, что действие возвращает правильное представление – простой тест.</span><span class="sxs-lookup"><span data-stu-id="49dd3-250">First, we’d like to verify the action returns the correct view – an easy test.</span></span> <span data-ttu-id="49dd3-251">Мы также хотим написать тест для проверки того, что действие получает правильного сотрудника, и мы хотели бы сделать это без выполнения кода для запроса базы данных.</span><span class="sxs-lookup"><span data-stu-id="49dd3-251">We’d also want to write a test to verify the action retrieves the correct employee, and we’d like to do this without executing code to query the database.</span></span> <span data-ttu-id="49dd3-252">Помните, что мы хотим изолировать тестированный код.</span><span class="sxs-lookup"><span data-stu-id="49dd3-252">Remember we want to isolate the code under test.</span></span> <span data-ttu-id="49dd3-253">Изоляция гарантирует, что тест не выйдет из строя из-за ошибки в коде доступа к данным или конфигурации базы данных.</span><span class="sxs-lookup"><span data-stu-id="49dd3-253">Isolation will ensure the test doesn’t fail because of a bug in the data access code or database configuration.</span></span> <span data-ttu-id="49dd3-254">Если тест не удается, мы будем знать, что у нас есть ошибка в логике контроллера, а не в каком-то компоненте системы более низкого уровня.</span><span class="sxs-lookup"><span data-stu-id="49dd3-254">If the test fails, we will know we have a bug in the controller logic, and not in some lower level system component.</span></span>

<span data-ttu-id="49dd3-255">Для достижения изоляции нам понадобятся некоторые абстракции, такие как интерфейсы, которые мы представили ранее для репозиторий и единиц работы.</span><span class="sxs-lookup"><span data-stu-id="49dd3-255">To achieve isolation we’ll need some abstractions like the interfaces we presented earlier for repositories and units of work.</span></span> <span data-ttu-id="49dd3-256">Помните, что шаблон репозитория предназначен для посредничества между объектами домена и слоем картирования данных.</span><span class="sxs-lookup"><span data-stu-id="49dd3-256">Remember the repository pattern is designed to mediate between domain objects and the data mapping layer.</span></span> <span data-ttu-id="49dd3-257">В этом сценарии EF4 *является* слоем картирования данных и уже предоставляет абстракцию типа репозитория iObjectSet&lt;T&gt; (из пространства имен System.Data.Objects).</span><span class="sxs-lookup"><span data-stu-id="49dd3-257">In this scenario EF4 *is* the data mapping layer, and already provides a repository-like abstraction named IObjectSet&lt;T&gt; (from the System.Data.Objects namespace).</span></span> <span data-ttu-id="49dd3-258">Определение интерфейса выглядит следующим образом.</span><span class="sxs-lookup"><span data-stu-id="49dd3-258">The interface definition looks like the following.</span></span>

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

<span data-ttu-id="49dd3-259">IObjectSet&lt;&gt; T отвечает требованиям к репозиторию, поскольку он напоминает коллекцию&lt;&gt;объектов (через IEnumerable T) и предоставляет методы добавления и удаления объектов из смоделированной коллекции.</span><span class="sxs-lookup"><span data-stu-id="49dd3-259">IObjectSet&lt;T&gt; meets the requirements for a repository because it resembles a collection of objects (via IEnumerable&lt;T&gt;) and provides methods to add and remove objects from the simulated collection.</span></span> <span data-ttu-id="49dd3-260">Методы присоединения и отъема предоставляют дополнительные возможности API EF4.</span><span class="sxs-lookup"><span data-stu-id="49dd3-260">The Attach and Detach methods expose additional capabilities of the EF4 API.</span></span> <span data-ttu-id="49dd3-261">Для использования IObjectSet&lt;T&gt; в качестве интерфейса для репозиториев нам нужна единица рабочей абстракции для связывания репозиториев.</span><span class="sxs-lookup"><span data-stu-id="49dd3-261">To use IObjectSet&lt;T&gt; as the interface for repositories we need a unit of work abstraction to bind repositories together.</span></span>

``` csharp
    public interface IUnitOfWork {
        IObjectSet<Employee> Employees { get; }
        IObjectSet<TimeCard> TimeCards { get; }
        void Commit();
    }
```

<span data-ttu-id="49dd3-262">Одна конкретная реализация этого интерфейса будет говорить с сервером S'L и легко создать с помощью класса ObjectContext от EF4.</span><span class="sxs-lookup"><span data-stu-id="49dd3-262">One concrete implementation of this interface will talk to SQL Server and is easy to create using the ObjectContext class from EF4.</span></span> <span data-ttu-id="49dd3-263">Класс ObjectContext является реальной единицей работы в API EF4.</span><span class="sxs-lookup"><span data-stu-id="49dd3-263">The ObjectContext class is the real unit of work in the EF4 API.</span></span>

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

<span data-ttu-id="49dd3-264">Оживлять&lt;IObjectSet T&gt; так же просто, как ссылаться на метод CreateObjectSet объекта ObjectContext.</span><span class="sxs-lookup"><span data-stu-id="49dd3-264">Bringing an IObjectSet&lt;T&gt; to life is as easy as invoking the CreateObjectSet method of the ObjectContext object.</span></span> <span data-ttu-id="49dd3-265">За кулисами рамки будут использовать метаданные, которые мы предоставили&lt;в&gt;EDM для производства конкретных ObjectSet T .</span><span class="sxs-lookup"><span data-stu-id="49dd3-265">Behind the scenes the framework will use the metadata we provided in the EDM to produce a concrete ObjectSet&lt;T&gt;.</span></span> <span data-ttu-id="49dd3-266">Мы будем придерживаться возвращения интерфейса IObjectSet&lt;T,&gt; потому что это поможет сохранить тестируемость в клиентском коде.</span><span class="sxs-lookup"><span data-stu-id="49dd3-266">We’ll stick with returning the IObjectSet&lt;T&gt; interface because it will help preserve testability in client code.</span></span>

<span data-ttu-id="49dd3-267">Эта конкретная реализация полезна в производстве, но мы должны сосредоточиться на том, как мы будем использовать нашу абстракцию IUnitOfWork для облегчения тестирования.</span><span class="sxs-lookup"><span data-stu-id="49dd3-267">This concrete implementation is useful in production, but we need to focus on how we’ll use our IUnitOfWork abstraction to facilitate testing.</span></span>

### <a name="the-test-doubles"></a><span data-ttu-id="49dd3-268">Тестовый парный разряд</span><span class="sxs-lookup"><span data-stu-id="49dd3-268">The Test Doubles</span></span>

<span data-ttu-id="49dd3-269">Чтобы изолировать действие контроллера, нам понадобится возможность переключения между реальной единицей работы (при поддержке ObjectContext) и тестовой двойной или "поддельной" единицей работы (выполняя операции в памяти).</span><span class="sxs-lookup"><span data-stu-id="49dd3-269">To isolate the controller action we’ll need the ability to switch between the real unit of work (backed by an ObjectContext) and a test double or “fake” unit of work (performing in-memory operations).</span></span> <span data-ttu-id="49dd3-270">Общий подход для выполнения этого типа переключения заключается в том, чтобы не позволить контроллеру MVC мгновенно гостевого блока работы, а вместо этого передать единицу работы в контроллер в качестве параметра конструктора.</span><span class="sxs-lookup"><span data-stu-id="49dd3-270">The common approach to perform this type of switching is to not let the MVC controller instantiate a unit of work, but instead pass the unit of work into the controller as a constructor parameter.</span></span>

``` csharp
    class EmployeeController : Controller {
      publicEmployeeController(IUnitOfWork unitOfWork)  {
          _unitOfWork = unitOfWork;
      }
      ...
    }
```

<span data-ttu-id="49dd3-271">Вышеуказанный код является примером инъекции зависимости.</span><span class="sxs-lookup"><span data-stu-id="49dd3-271">The above code is an example of dependency injection.</span></span> <span data-ttu-id="49dd3-272">Мы не позволяем контроллеру создавать его зависимость (единицу работы), но вводим зависимость в контроллер.</span><span class="sxs-lookup"><span data-stu-id="49dd3-272">We don’t allow the controller to create it’s dependency (the unit of work) but inject the dependency into the controller.</span></span> <span data-ttu-id="49dd3-273">В проекте MVC обычно используется фабрика пользовательских контроллеров в сочетании с инверсией элемента управления (IoC) контейнера для автоматизации впрыска зависимости.</span><span class="sxs-lookup"><span data-stu-id="49dd3-273">In an MVC project it is common to use a custom controller factory in combination with an inversion of control (IoC) container to automate dependency injection.</span></span> <span data-ttu-id="49dd3-274">Эти темы выходят за рамки данной статьи, но вы можете прочитать больше, следуя ссылкам в конце этой статьи.</span><span class="sxs-lookup"><span data-stu-id="49dd3-274">These topics are beyond the scope of this article, but you can read more by following the references at the end of this article.</span></span>

<span data-ttu-id="49dd3-275">Поддельная единица реализации работы, которую мы можем использовать для тестирования, может выглядеть следующим образом.</span><span class="sxs-lookup"><span data-stu-id="49dd3-275">A fake unit of work implementation that we can use for testing might look like the following.</span></span>

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

<span data-ttu-id="49dd3-276">Обратите внимание, поддельные единицы работы подвергает совершается собственности.</span><span class="sxs-lookup"><span data-stu-id="49dd3-276">Notice the fake unit of work exposes a Commited property.</span></span> <span data-ttu-id="49dd3-277">Иногда полезно добавлять функции в поддельный класс, облегчающие тестирование.</span><span class="sxs-lookup"><span data-stu-id="49dd3-277">It’s sometimes useful to add features to a fake class that facilitate testing.</span></span> <span data-ttu-id="49dd3-278">В этом случае легко наблюдать, если код совершает единицу работы, проверяя свойство Commited.</span><span class="sxs-lookup"><span data-stu-id="49dd3-278">In this case it is easy to observe if code commits a unit of work by checking the Commited property.</span></span>

<span data-ttu-id="49dd3-279">Нам также понадобится поддельный&lt;IObjectSet T&gt; для хранения объектов сотрудников и TimeCard в памяти.</span><span class="sxs-lookup"><span data-stu-id="49dd3-279">We’ll also need a fake IObjectSet&lt;T&gt; to hold Employee and TimeCard objects in memory.</span></span> <span data-ttu-id="49dd3-280">Мы можем обеспечить единую реализацию с использованием генериков.</span><span class="sxs-lookup"><span data-stu-id="49dd3-280">We can provide a single implementation using generics.</span></span>

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

<span data-ttu-id="49dd3-281">Этот тестовый двойник делегирует большую&lt;&gt; часть своей работы базовому объекту HashSet T.</span><span class="sxs-lookup"><span data-stu-id="49dd3-281">This test double delegates most of its work to an underlying HashSet&lt;T&gt; object.</span></span> <span data-ttu-id="49dd3-282">Обратите внимание,&lt;что&gt; IObjectSet T требует общего ограничения, обеспечивающего T как класс (тип&lt;ссылки), а также заставляет нас реализовывать I'Eeryable T&gt;.</span><span class="sxs-lookup"><span data-stu-id="49dd3-282">Note that IObjectSet&lt;T&gt; requires a generic constraint enforcing T as a class (a reference type), and also forces us to implement IQueryable&lt;T&gt;.</span></span> <span data-ttu-id="49dd3-283">С помощью стандартного оператора LIN' As'queryable легко сделать так, чтобы коллекция в памяти выглядела как I'I-Образный&lt;T.&gt;</span><span class="sxs-lookup"><span data-stu-id="49dd3-283">It is easy to make an in-memory collection appear as an IQueryable&lt;T&gt; using the standard LINQ operator AsQueryable.</span></span>

### <a name="the-tests"></a><span data-ttu-id="49dd3-284">Тесты</span><span class="sxs-lookup"><span data-stu-id="49dd3-284">The Tests</span></span>

<span data-ttu-id="49dd3-285">Традиционные модульные тесты будут использовать один класс испытаний для проведения всех тестов для всех действий в одном контроллере MVC.</span><span class="sxs-lookup"><span data-stu-id="49dd3-285">Traditional unit tests will use a single test class to hold all of the tests for all of the actions in a single MVC controller.</span></span> <span data-ttu-id="49dd3-286">Мы можем написать эти тесты, или любой тип модульного теста, используя подделки в памяти, которые мы создали.</span><span class="sxs-lookup"><span data-stu-id="49dd3-286">We can write these tests, or any type of unit test, using the in memory fakes we’ve built.</span></span> <span data-ttu-id="49dd3-287">Однако для этой статьи мы избежим монолитного подхода класса тестов и вместо этого сгруппируем наши тесты, чтобы сосредоточиться на определенной функциональности.</span><span class="sxs-lookup"><span data-stu-id="49dd3-287">However, for this article we will avoid the monolithic test class approach and instead group our tests to focus on a specific piece of functionality.</span></span><span data-ttu-id="49dd3-288">Например, "создать нового сотрудника" может быть функциональность, которая мы хотим проверить, поэтому мы будем использовать один класс тестирования для проверки одного действия контроллера, ответственного за создание нового сотрудника.</span><span class="sxs-lookup"><span data-stu-id="49dd3-288">  For example, “create new employee” might be the functionality we want to test, so we will use a single test class to verify the single controller action responsible for creating a new employee.</span></span>

<span data-ttu-id="49dd3-289">Существует некоторый общий код настройки, который нам нужен для всех этих классов тонкозернистого теста.</span><span class="sxs-lookup"><span data-stu-id="49dd3-289">There is some common setup code we need for all these fine grained test classes.</span></span> <span data-ttu-id="49dd3-290">Например, нам всегда нужно создавать наши хранилища памяти и поддельную единицу работы.</span><span class="sxs-lookup"><span data-stu-id="49dd3-290">For example, we always need to create our in-memory repositories and fake unit of work.</span></span> <span data-ttu-id="49dd3-291">Нам также нужен экземпляр контроллера сотрудника с поддельным блоком работы вводили.</span><span class="sxs-lookup"><span data-stu-id="49dd3-291">We also need an instance of the employee controller with the fake unit of work injected.</span></span> <span data-ttu-id="49dd3-292">Мы поделимся этим общим кодом настройки в тестовых классах с помощью базового класса.</span><span class="sxs-lookup"><span data-stu-id="49dd3-292">We’ll share this common setup code across test classes by using a base class.</span></span>

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

<span data-ttu-id="49dd3-293">"Объектная мать", используемая в базовом классе, является одним из распространенных шаблонов для создания тестовых данных.</span><span class="sxs-lookup"><span data-stu-id="49dd3-293">The “object mother” we use in the base class is one common pattern for creating test data.</span></span> <span data-ttu-id="49dd3-294">Мать объекта содержит заводские методы для мгновенного распознавания объектов тестирования для использования в нескольких тестовых приборах.</span><span class="sxs-lookup"><span data-stu-id="49dd3-294">An object mother contains factory methods to instantiate test entities for use across multiple test fixtures.</span></span>

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

<span data-ttu-id="49dd3-295">Мы можем использовать EmployeeControllerTestBase в качестве базового класса для ряда тестовых приборов (см. рисунок 3).</span><span class="sxs-lookup"><span data-stu-id="49dd3-295">We can use the EmployeeControllerTestBase as the base class for a number of test fixtures (see figure 3).</span></span> <span data-ttu-id="49dd3-296">Каждый тестовый прибор будет тестировать определенное действие контроллера.</span><span class="sxs-lookup"><span data-stu-id="49dd3-296">Each test fixture will test a specific controller action.</span></span> <span data-ttu-id="49dd3-297">Например, один тестовый прибор будет сосредоточен на тестировании действия Create, используемого во время запроса HTTP GET (для отображения представления для создания сотрудника), а другой прибор будет сосредоточен на действии Create, используемом в запросе HTTP POST (для получения информации, представленной пользователем для создания сотрудника).</span><span class="sxs-lookup"><span data-stu-id="49dd3-297">For example, one test fixture will focus on testing the Create action used during an HTTP GET request (to display the view for creating an employee), and a different fixture will focus on the Create action used in an HTTP POST request (to take information submitted by the user to create an employee).</span></span> <span data-ttu-id="49dd3-298">Каждый полученный класс отвечает только за настройку, необходимую в его конкретном контексте, и для предоставления утверждений, необходимых для проверки результатов для его конкретного контекста тестирования.</span><span class="sxs-lookup"><span data-stu-id="49dd3-298">Each derived class is only responsible for the setup needed in its specific context, and to provide the assertions needed to verify the outcomes for its specific test context.</span></span>

![ef test_03](~/ef6/media/eftest-03.png)

<span data-ttu-id="49dd3-300">**Рисунок 3**</span><span class="sxs-lookup"><span data-stu-id="49dd3-300">**Figure 3**</span></span>

<span data-ttu-id="49dd3-301">Представленный здесь тип именования и стиль тестирования не требуются для тестируемого кода – это всего лишь один подход.</span><span class="sxs-lookup"><span data-stu-id="49dd3-301">The naming convention and test style presented here isn’t required for testable code – it’s just one approach.</span></span> <span data-ttu-id="49dd3-302">Рисунок 4 показывает тесты, проводимые в Jet Brains Resharper тест бегун плагин для Visual Studio 2010.</span><span class="sxs-lookup"><span data-stu-id="49dd3-302">Figure 4 shows the tests running in the Jet Brains Resharper test runner plugin for Visual Studio 2010.</span></span>

![ef test_04](~/ef6/media/eftest-04.png)

<span data-ttu-id="49dd3-304">**Рисунок 4**</span><span class="sxs-lookup"><span data-stu-id="49dd3-304">**Figure 4**</span></span>

<span data-ttu-id="49dd3-305">При базовом классе для обработки общего кода настройки модульные тесты для каждого действия контроллера малы и просты в записи.</span><span class="sxs-lookup"><span data-stu-id="49dd3-305">With a base class to handle the shared setup code, the unit tests for each controller action are small and easy to write.</span></span> <span data-ttu-id="49dd3-306">Тесты будут выполняться быстро (так как мы выполняем операции в памяти), и не должны потерпеть неудачу из-за несвязанных инфраструктуры или экологических проблем (потому что мы изолировали блок, наданный тест).</span><span class="sxs-lookup"><span data-stu-id="49dd3-306">The tests will execute quickly (since we are performing in-memory operations), and shouldn’t fail because of unrelated infrastructure or environmental concerns (because we’ve isolated the unit under test).</span></span>

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

<span data-ttu-id="49dd3-307">В этих тестах базовый класс выполняет большую часть работы по настройке.</span><span class="sxs-lookup"><span data-stu-id="49dd3-307">In these tests, the base class does most of the setup work.</span></span> <span data-ttu-id="49dd3-308">Помните, что конструктор базового класса создает репозиторий памяти, поддельную единицу работы и экземпляр класса EmployeeController.</span><span class="sxs-lookup"><span data-stu-id="49dd3-308">Remember the base class constructor creates the in-memory repository, a fake unit of work, and an instance of the EmployeeController class.</span></span> <span data-ttu-id="49dd3-309">Тестовый класс происходит от этого базового класса и фокусируется на специфике тестирования метода Создания.</span><span class="sxs-lookup"><span data-stu-id="49dd3-309">The test class derives from this base class and focuses on the specifics of testing the Create method.</span></span> <span data-ttu-id="49dd3-310">В этом случае специфика сводится к шагам «обустроить, действовать и утверждать», которые вы увидите в любой процедуре модульного тестирования:</span><span class="sxs-lookup"><span data-stu-id="49dd3-310">In this case the specifics boil down to the “arrange, act, and assert” steps you’ll see in any unit testing procedure:</span></span>

-   <span data-ttu-id="49dd3-311">Создание нового объекта-сотрудника для имитации входящих данных.</span><span class="sxs-lookup"><span data-stu-id="49dd3-311">Create a newEmployee object to simulate incoming data.</span></span>
-   <span data-ttu-id="49dd3-312">Вызвать действие Create employeeController и пройти в newEmployee.</span><span class="sxs-lookup"><span data-stu-id="49dd3-312">Invoke the Create action of the EmployeeController and pass in the newEmployee.</span></span>
-   <span data-ttu-id="49dd3-313">Проверка действия Create создает ожидаемые результаты (сотрудник отображается в репозитории).</span><span class="sxs-lookup"><span data-stu-id="49dd3-313">Verify the Create action produces the expected results (the employee appears in the repository).</span></span>

<span data-ttu-id="49dd3-314">То, что мы создали, позволяет нам протестировать любое из действий EmployeeController.</span><span class="sxs-lookup"><span data-stu-id="49dd3-314">What we’ve built allows us to test any of the EmployeeController actions.</span></span> <span data-ttu-id="49dd3-315">Например, когда мы пишем тесты для действия Индекса контроллера сотрудника, мы можем наследовать из класса тестовой базы, чтобы установить ту же базовую настройку для наших тестов.</span><span class="sxs-lookup"><span data-stu-id="49dd3-315">For example, when we write tests for the Index action of the Employee controller we can inherit from the test base class to establish the same base setup for our tests.</span></span> <span data-ttu-id="49dd3-316">Снова базовый класс создаст хранилище в памяти, поддельную единицу работы и экземпляр EmployeeController.</span><span class="sxs-lookup"><span data-stu-id="49dd3-316">Again the base class will create the in-memory repository, the fake unit of work, and an instance of the EmployeeController.</span></span> <span data-ttu-id="49dd3-317">Тесты для действия Индекса должны быть направлены только на то, чтобы ссылаться на действие Индекса и тестировать качества модели, которая возвращается к действию.</span><span class="sxs-lookup"><span data-stu-id="49dd3-317">The tests for the Index action only need to focus on invoking the Index action and testing the qualities of the model the action returns.</span></span>

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

<span data-ttu-id="49dd3-318">Тесты, которые мы создаем с подделками в памяти, ориентированы на тестирование *состояния* программного обеспечения.</span><span class="sxs-lookup"><span data-stu-id="49dd3-318">The tests we are creating with in-memory fakes are oriented towards testing the *state* of the software.</span></span> <span data-ttu-id="49dd3-319">Например, при тестировании действия «Создание» необходимо проверить состояние репозитория после выполнения действия «Создание» — удерживает ли репозиторий нового сотрудника?</span><span class="sxs-lookup"><span data-stu-id="49dd3-319">For example, when testing the Create action we want to inspect the state of the repository after the create action executes – does the repository hold the new employee?</span></span>

``` csharp
    [TestMethod]
    public void ShouldAddNewEmployeeToRepository() {
        _controller.Create(_newEmployee);
        Assert.IsTrue(_repository.Contains(_newEmployee));
    }
```

<span data-ttu-id="49dd3-320">Позже мы рассмотрим тестирование на основе взаимодействия.</span><span class="sxs-lookup"><span data-stu-id="49dd3-320">Later we’ll look at interaction based testing.</span></span> <span data-ttu-id="49dd3-321">Тестирование на основе взаимодействия спросит, ссылается ли тестовый код на надлежащие методы на наши объекты и передает правильные параметры.</span><span class="sxs-lookup"><span data-stu-id="49dd3-321">Interaction based testing will ask if the code under test invoked the proper methods on our objects and passed the correct parameters.</span></span> <span data-ttu-id="49dd3-322">Сейчас мы будем двигаться по крышке другой шаблон дизайна - ленивый нагрузки.</span><span class="sxs-lookup"><span data-stu-id="49dd3-322">For now we’ll move on the cover another design pattern – the lazy load.</span></span>

## <a name="eager-loading-and-lazy-loading"></a><span data-ttu-id="49dd3-323">Стремясь загрузки и ленивые загрузки</span><span class="sxs-lookup"><span data-stu-id="49dd3-323">Eager Loading and Lazy Loading</span></span>

<span data-ttu-id="49dd3-324">В какой-то момент в веб-приложении mVC ASP.NET мы, возможно, пожелаем отобразить информацию сотрудника и включить связанные с сотрудником временные карты.</span><span class="sxs-lookup"><span data-stu-id="49dd3-324">At some point in the ASP.NET  MVC web application we might wish to display an employee’s information and include the employee’s associated time cards.</span></span> <span data-ttu-id="49dd3-325">Например, у нас может быть сводный дисплей скарты времени, который показывает имя сотрудника и общее количество карт времени в системе.</span><span class="sxs-lookup"><span data-stu-id="49dd3-325">For example, we might have a time card summary display that shows the employee’s name and the total number of time cards in the system.</span></span> <span data-ttu-id="49dd3-326">Существует несколько подходов, которые мы можем предпринять для реализации этой функции.</span><span class="sxs-lookup"><span data-stu-id="49dd3-326">There are several approaches we can take to implement this feature.</span></span>

### <a name="projection"></a><span data-ttu-id="49dd3-327">Проекция</span><span class="sxs-lookup"><span data-stu-id="49dd3-327">Projection</span></span>

<span data-ttu-id="49dd3-328">Один простой подход к созданию резюме заключается в создании модели, посвященной информации, которая мы хотим отображать в представлении.</span><span class="sxs-lookup"><span data-stu-id="49dd3-328">One easy approach to create the summary is to construct a model dedicated to the information we want to display in the view.</span></span> <span data-ttu-id="49dd3-329">В этом сценарии модель может выглядеть следующим образом.</span><span class="sxs-lookup"><span data-stu-id="49dd3-329">In this scenario the model might look like the following.</span></span>

``` csharp
    public class EmployeeSummaryViewModel {
        public string Name { get; set; }
        public int TotalTimeCards { get; set; }
    }
```

<span data-ttu-id="49dd3-330">Обратите внимание, что EmployeeSummaryViewModel не является сущностью - другими словами, это не то, что мы хотим сохранить в базе данных.</span><span class="sxs-lookup"><span data-stu-id="49dd3-330">Note that the EmployeeSummaryViewModel is not an entity – in other words it is not something we want to persist in the database.</span></span> <span data-ttu-id="49dd3-331">Мы будем использовать этот класс только для перетасовки данных в представление в сильно набранном виде.</span><span class="sxs-lookup"><span data-stu-id="49dd3-331">We are only going to use this class to shuffle data into the view in a strongly typed manner.</span></span> <span data-ttu-id="49dd3-332">Модель представления подобна объекту передачи данных (DTO), поскольку она не содержит поведения (без методов) - только свойства.</span><span class="sxs-lookup"><span data-stu-id="49dd3-332">The view model is like a data transfer object (DTO) because it contains no behavior (no methods) – only properties.</span></span> <span data-ttu-id="49dd3-333">Свойства будут хранить данные, необходимые для перемещения.</span><span class="sxs-lookup"><span data-stu-id="49dd3-333">The properties will hold the data we need to move.</span></span> <span data-ttu-id="49dd3-334">С помощью стандартного проекционного оператора - оператора Select можно легко мгновенно извнести эту модель представления.</span><span class="sxs-lookup"><span data-stu-id="49dd3-334">It is easy to instantiate this view model using LINQ’s standard projection operator – the Select operator.</span></span>

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

<span data-ttu-id="49dd3-335">В приведенном выше коде есть две примечательные особенности.</span><span class="sxs-lookup"><span data-stu-id="49dd3-335">There are two notable features to the above code.</span></span> <span data-ttu-id="49dd3-336">Во-первых – код легко тестировать, потому что его по-прежнему легко наблюдать и изолировать.</span><span class="sxs-lookup"><span data-stu-id="49dd3-336">First – the code is easy to test because it is still easy to observe and isolate.</span></span> <span data-ttu-id="49dd3-337">Оператор Select работает так же хорошо против наших подделок в памяти, как и против реальной единицы работы.</span><span class="sxs-lookup"><span data-stu-id="49dd3-337">The Select operator works just as well against our in-memory fakes as it does against the real unit of work.</span></span>

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

<span data-ttu-id="49dd3-338">Вторая примечательная особенность заключается в том, как код позволяет EF4 генерировать единый, эффективный запрос для совместной сборки информации о сотрудниках и временных картах.</span><span class="sxs-lookup"><span data-stu-id="49dd3-338">The second notable feature is how the code allows EF4 to generate a single, efficient query to assemble employee and time card information together.</span></span> <span data-ttu-id="49dd3-339">Мы загрузили информацию о сотрудниках и информацию о временных картах в один и тот же объект без использования специальных AA.</span><span class="sxs-lookup"><span data-stu-id="49dd3-339">We’ve loaded employee information and time card information into the same object without using any special APIs.</span></span> <span data-ttu-id="49dd3-340">В коде просто выражается информация, необходимая для использования стандартных операторов LIN, которые работают против источников данных в памяти, а также удаленных источников данных.</span><span class="sxs-lookup"><span data-stu-id="49dd3-340">The code merely expressed the information it requires using standard LINQ operators that work against in-memory data sources as well as remote data sources.</span></span> <span data-ttu-id="49dd3-341">EF4 удалось перевести деревья выражения, генерируемые запросом\# LIN и C компилятором, в единый и эффективный запрос T-S'L.</span><span class="sxs-lookup"><span data-stu-id="49dd3-341">EF4 was able to translate the expression trees generated by the LINQ query and C\# compiler into a single and efficient T-SQL query.</span></span>

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

<span data-ttu-id="49dd3-342">Есть и другие случаи, когда мы не хотим работать с моделью представления или объектом DTO, но с реальными сущностями.</span><span class="sxs-lookup"><span data-stu-id="49dd3-342">There are other times when we don’t want to work with a view model or DTO object, but with real entities.</span></span> <span data-ttu-id="49dd3-343">Когда мы знаем, что нам нужен сотрудник *и* карты времени сотрудника, мы можем с нетерпением загружать соответствующие данные ненавязчивым и эффективным образом.</span><span class="sxs-lookup"><span data-stu-id="49dd3-343">When we know we need an employee *and* the employee’s time cards, we can eagerly load the related data in an unobtrusive and efficient manner.</span></span>

### <a name="explicit-eager-loading"></a><span data-ttu-id="49dd3-344">Явная стремящаяся загрузка</span><span class="sxs-lookup"><span data-stu-id="49dd3-344">Explicit Eager Loading</span></span>

<span data-ttu-id="49dd3-345">Когда мы хотим с готовностью загрузить связанную информацию о сущности, нам нужен механизм бизнес-логики (или в этом сценарии, логика действия контроллера), чтобы выразить свое желание репозиторию.</span><span class="sxs-lookup"><span data-stu-id="49dd3-345">When we want to eagerly load related entity information we need some mechanism for business logic (or in this scenario, controller action logic) to express its desire to the repository.</span></span> <span data-ttu-id="49dd3-346">Класс ef4 Object'ery&lt;T&gt; определяет метод включения для указания связанных объектов для извлечения во время запроса.</span><span class="sxs-lookup"><span data-stu-id="49dd3-346">The EF4 ObjectQuery&lt;T&gt; class defines an Include method to specify the related objects to retrieve during a query.</span></span> <span data-ttu-id="49dd3-347">Помните, что EF4 ObjectContext разоблачает сущности через конкретный класс ObjectSet&lt;T,&gt; который наследует от Object's.a.&lt;&gt;</span><span class="sxs-lookup"><span data-stu-id="49dd3-347">Remember the EF4 ObjectContext exposes entities via the concrete ObjectSet&lt;T&gt; class which inherits from ObjectQuery&lt;T&gt;.</span></span><span data-ttu-id="49dd3-348">Если бы мы&lt;использовали&gt; ссылки ObjectSet T в нашем действии контроллера, мы могли бы написать следующий код, чтобы указать несбывную нагрузку информации о карточке времени для каждого сотрудника.</span><span class="sxs-lookup"><span data-stu-id="49dd3-348">  If we were using ObjectSet&lt;T&gt; references in our controller action we could write the following code to specify an eager load of time card information for each employee.</span></span>

``` csharp
    _employees.Include("TimeCards")
              .Where(e => e.HireDate.Year > 2009);
```

<span data-ttu-id="49dd3-349">Однако, поскольку мы стараемся, чтобы наш код&lt;&gt; проверяемый, мы не подвергаем ObjectSet T из-за пределов реальной единицы рабочего класса.</span><span class="sxs-lookup"><span data-stu-id="49dd3-349">However, since we are trying to keep our code testable we are not exposing ObjectSet&lt;T&gt; from outside the real unit of work class.</span></span> <span data-ttu-id="49dd3-350">Вместо этого мы полагаемся&lt;на&gt; интерфейс IObjectSet T, который&lt;&gt; легче подделать, но IObjectSet T не определяет метод включения.</span><span class="sxs-lookup"><span data-stu-id="49dd3-350">Instead, we rely on the IObjectSet&lt;T&gt; interface which is easier to fake, but IObjectSet&lt;T&gt; does not define an Include method.</span></span> <span data-ttu-id="49dd3-351">Прелесть LIN'а заключается в том, что мы можем создать собственного оператора Include.</span><span class="sxs-lookup"><span data-stu-id="49dd3-351">The beauty of LINQ is that we can create our own Include operator.</span></span>

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

<span data-ttu-id="49dd3-352">Обратите внимание, что этот оператор Включает в&lt;себя&gt; метод расширения&lt;для&gt;I'E'E'Eiryable T вместо IObjectSet T .</span><span class="sxs-lookup"><span data-stu-id="49dd3-352">Notice this Include operator is defined as an extension method for IQueryable&lt;T&gt; instead of IObjectSet&lt;T&gt;.</span></span> <span data-ttu-id="49dd3-353">Это дает нам возможность использовать метод с более широким диапазоном возможных&gt;типов,&lt;в&gt;том числе I'I'ryable&lt;&lt;T , IObjectSet T , ОбъектКуи T&gt;, и ObjectSet&lt;T&gt;.</span><span class="sxs-lookup"><span data-stu-id="49dd3-353">This gives us the ability to use the method with a wider range of possible types, including IQueryable&lt;T&gt;, IObjectSet&lt;T&gt;, ObjectQuery&lt;T&gt;, and ObjectSet&lt;T&gt;.</span></span> <span data-ttu-id="49dd3-354">В случае, если основная последовательность не является&lt;подлинным EF4 Object's,s,&gt;то нет никакого вреда, и оператор Include не является op.</span><span class="sxs-lookup"><span data-stu-id="49dd3-354">In the event the underlying sequence is not a genuine EF4 ObjectQuery&lt;T&gt;, then there is no harm done and the Include operator is a no-op.</span></span> <span data-ttu-id="49dd3-355">Если основной последовательностью *является* &lt;Object'ery T&gt; (или&lt;&gt;выведенный из Object'ery T), то EF4 увидит наше требование о дополнительных данных и сформулирует надлежащий запрос S'L.</span><span class="sxs-lookup"><span data-stu-id="49dd3-355">If the underlying sequence *is* an ObjectQuery&lt;T&gt; (or derived from ObjectQuery&lt;T&gt;), then EF4 will see our requirement for additional data and formulate the proper SQL query.</span></span>

<span data-ttu-id="49dd3-356">С этим новым оператором на месте мы можем явно запросить стремятся нагрузки информации карты времени из репозитория.</span><span class="sxs-lookup"><span data-stu-id="49dd3-356">With this new operator in place we can explicitly request an eager load of time card information from the repository.</span></span>

``` csharp
    public ViewResult Index() {
        var model = _unitOfWork.Employees
                               .Include("TimeCards")
                               .OrderBy(e => e.HireDate);
        return View(model);
    }
```

<span data-ttu-id="49dd3-357">При запуске в отношении реального ObjectContext код создает следующий один запрос.</span><span class="sxs-lookup"><span data-stu-id="49dd3-357">When run against a real ObjectContext, the code produces the following single query.</span></span> <span data-ttu-id="49dd3-358">Запрос собирает достаточно информации из базы данных за одну поездку, чтобы материализовать объекты сотрудников и полностью заполнить их свойство TimeCards.</span><span class="sxs-lookup"><span data-stu-id="49dd3-358">The query gathers enough information from the database in one trip to materialize the employee objects and fully populate their TimeCards property.</span></span>

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

<span data-ttu-id="49dd3-359">Хорошей новостью является то, что код внутри метода действия остается полностью проверяемым.</span><span class="sxs-lookup"><span data-stu-id="49dd3-359">The great news is the code inside the action method remains fully testable.</span></span> <span data-ttu-id="49dd3-360">Нам не нужно предоставлять какие-либо дополнительные функции для наших подделок для поддержки оператора Include.</span><span class="sxs-lookup"><span data-stu-id="49dd3-360">We don’t need to provide any additional features for our fakes to support the Include operator.</span></span> <span data-ttu-id="49dd3-361">Плохая новость заключается в том, что мы должны были использовать оператора Включить внутри кода, который мы хотели сохранить настойчивость невежественным.</span><span class="sxs-lookup"><span data-stu-id="49dd3-361">The bad news is we had to use the Include operator inside of the code we wanted to keep persistence ignorant.</span></span> <span data-ttu-id="49dd3-362">Это наиосновной пример типа компромиссов, которые необходимо оценить при создании проверяемого кода.</span><span class="sxs-lookup"><span data-stu-id="49dd3-362">This is a prime example of the type of tradeoffs you’ll need to evaluate when building testable code.</span></span> <span data-ttu-id="49dd3-363">Есть моменты, когда вам нужно, чтобы настойчивость проблемы утечки за пределами репозитория абстракции для достижения целей производительности.</span><span class="sxs-lookup"><span data-stu-id="49dd3-363">There are times when you need to let persistence concerns leak outside the repository abstraction to meet performance goals.</span></span>

<span data-ttu-id="49dd3-364">Альтернативой нетерпеливой загрузке является ленивая загрузка.</span><span class="sxs-lookup"><span data-stu-id="49dd3-364">The alternative to eager loading is lazy loading.</span></span> <span data-ttu-id="49dd3-365">Ленивая загрузка означает, что нам *не* нужен наш бизнес-код, чтобы явно объявить требование о связанных данных.</span><span class="sxs-lookup"><span data-stu-id="49dd3-365">Lazy loading means we do *not* need our business code to explicitly announce the requirement for associated data.</span></span> <span data-ttu-id="49dd3-366">Вместо этого мы используем наши сущности в приложении, и если необходимы дополнительные данные, система entity загружает данные по требованию.</span><span class="sxs-lookup"><span data-stu-id="49dd3-366">Instead, we use our entities in the application and if additional data is needed Entity Framework will load the data on demand.</span></span>

### <a name="lazy-loading"></a><span data-ttu-id="49dd3-367">Отложенная загрузка</span><span class="sxs-lookup"><span data-stu-id="49dd3-367">Lazy Loading</span></span>

<span data-ttu-id="49dd3-368">Легко представить себе сценарий, при котором мы не знаем, какие данные понадобятся бизнес-логике.</span><span class="sxs-lookup"><span data-stu-id="49dd3-368">It’s easy to imagine a scenario where we don’t know what data a piece of business logic will need.</span></span> <span data-ttu-id="49dd3-369">Возможно, мы знаем, что логике нужен объект сотрудника, но мы можем влиться в различные пути выполнения, где некоторые из этих путей требуют от сотрудника данных временных карт, а некоторые нет.</span><span class="sxs-lookup"><span data-stu-id="49dd3-369">We might know the logic needs an employee object, but we may branch into different execution paths where some of those paths require time card information from the employee, and some do not.</span></span> <span data-ttu-id="49dd3-370">Сценарии, подобные этому, идеально подходят для неявной ленивой загрузки, потому что данные волшебным образом появляются по мере необходимости.</span><span class="sxs-lookup"><span data-stu-id="49dd3-370">Scenarios like this are perfect for implicit lazy loading because data magically appears on an as-needed basis.</span></span>

<span data-ttu-id="49dd3-371">Ленивая загрузка, также известная как отложенная загрузка, действительно предъявляет некоторые требования к объектам сущности.</span><span class="sxs-lookup"><span data-stu-id="49dd3-371">Lazy loading, also known as deferred loading, does place some requirements on our entity objects.</span></span> <span data-ttu-id="49dd3-372">POOS с истинным незнанием настойчивости не будет сталкиваться с какими-либо требованиями со стороны настойчивости слоя, но истинное сохранение невежества практически невозможно достичь.</span><span class="sxs-lookup"><span data-stu-id="49dd3-372">POCOs with true persistence ignorance would not face any requirements from the persistence layer, but true persistence ignorance is practically impossible to achieve.</span></span><span data-ttu-id="49dd3-373">Вместо этого мы измеряем упорство невежество в относительной степени.</span><span class="sxs-lookup"><span data-stu-id="49dd3-373">  Instead we measure persistence ignorance in relative degrees.</span></span> <span data-ttu-id="49dd3-374">Было бы прискорбно, если бы нам нужно было наследовать от настойчивости ориентированный базовый класс или использовать специализированную коллекцию для достижения ленивых загрузки в POOS.</span><span class="sxs-lookup"><span data-stu-id="49dd3-374">It would be unfortunate if we needed to inherit from a persistence oriented base class or use a specialized collection to achieve lazy loading in POCOs.</span></span> <span data-ttu-id="49dd3-375">К счастью, EF4 имеет менее навязчивое решение.</span><span class="sxs-lookup"><span data-stu-id="49dd3-375">Fortunately, EF4 has a less intrusive solution.</span></span>

### <a name="virtually-undetectable"></a><span data-ttu-id="49dd3-376">Практически необнаруживаемые</span><span class="sxs-lookup"><span data-stu-id="49dd3-376">Virtually Undetectable</span></span>

<span data-ttu-id="49dd3-377">При использовании объектов POCO EF4 может динамически генерировать прокси-подобие времени выполнения для сущностей.</span><span class="sxs-lookup"><span data-stu-id="49dd3-377">When using POCO objects, EF4 can dynamically generate runtime proxies for entities.</span></span> <span data-ttu-id="49dd3-378">Эти прокси невидимо обернуть материализовались POOS и предоставлять дополнительные услуги путем перехвата каждого свойства получить и установить операцию для выполнения дополнительной работы.</span><span class="sxs-lookup"><span data-stu-id="49dd3-378">These proxies invisibly wrap the materialized POCOs and provide additional services by intercepting each property get and set operation to perform additional work.</span></span> <span data-ttu-id="49dd3-379">Одним из таких услуг является ленивый функции загрузки мы ищем.</span><span class="sxs-lookup"><span data-stu-id="49dd3-379">One such service is the lazy loading feature we are looking for.</span></span> <span data-ttu-id="49dd3-380">Другой службой является эффективный механизм отслеживания изменений, который может записывать, когда программа изменяет значения свойств объекта.</span><span class="sxs-lookup"><span data-stu-id="49dd3-380">Another service is an efficient change tracking mechanism which can record when the program changes the property values of an entity.</span></span> <span data-ttu-id="49dd3-381">Список изменений используется ObjectContext во время метода SaveChanges для сохранения любых измененных сущностей с использованием команд UPDATE.</span><span class="sxs-lookup"><span data-stu-id="49dd3-381">The list of changes is used by the ObjectContext during the SaveChanges method to persist any modified entities using UPDATE commands.</span></span>

<span data-ttu-id="49dd3-382">Для того, чтобы эти прокси-компании работали, им нужен способ зацепиться за свойство получить и установить операции на сущности, и прокси достичь этой цели, переопределяя виртуальных членов.</span><span class="sxs-lookup"><span data-stu-id="49dd3-382">For these proxies to work, however, they need a way to hook into property get and set operations on an entity, and the proxies achieve this goal by overriding virtual members.</span></span> <span data-ttu-id="49dd3-383">Таким образом, если мы хотим иметь неявную ленивую загрузку и эффективное отслеживание изменений, мы должны вернуться к нашим определениям класса POCO и пометить свойства как виртуальные.</span><span class="sxs-lookup"><span data-stu-id="49dd3-383">Thus, if we want to have implicit lazy loading and efficient change tracking we need to go back to our POCO class definitions and mark properties as virtual.</span></span>

``` csharp
    public class Employee {
        public virtual int Id { get; set; }
        public virtual string Name { get; set; }
        public virtual DateTime HireDate { get; set; }
        public virtual ICollection<TimeCard> TimeCards { get; set; }
    }
```

<span data-ttu-id="49dd3-384">Мы все еще можем сказать, что организация Employee в основном является неосведомленной.</span><span class="sxs-lookup"><span data-stu-id="49dd3-384">We can still say the Employee entity is mostly persistence ignorant.</span></span> <span data-ttu-id="49dd3-385">Единственным требованием является использование виртуальных членов, и это не влияет на тестируемость кода.</span><span class="sxs-lookup"><span data-stu-id="49dd3-385">The only requirement is to use virtual members and this does not impact the testability of the code.</span></span> <span data-ttu-id="49dd3-386">Нам не нужно получать от какого-либо специального базового класса, или даже использовать специальную коллекцию, посвященную ленивым загрузки.</span><span class="sxs-lookup"><span data-stu-id="49dd3-386">We don’t need to derive from any special base class, or even use a special collection dedicated to lazy loading.</span></span> <span data-ttu-id="49dd3-387">Как показывает код, любой класс,&lt;&gt; реализующий ICollection T, доступен для хранения связанных сущностей.</span><span class="sxs-lookup"><span data-stu-id="49dd3-387">As the code demonstrates, any class implementing ICollection&lt;T&gt; is available to hold related entities.</span></span>

<span data-ttu-id="49dd3-388">Существует также одно незначительное изменение, мы должны сделать внутри нашей единицы работы.</span><span class="sxs-lookup"><span data-stu-id="49dd3-388">There is also one minor change we need to make inside our unit of work.</span></span> <span data-ttu-id="49dd3-389">Ленивая загрузка *отключена* по умолчанию при работе непосредственно с объектом ObjectContext.</span><span class="sxs-lookup"><span data-stu-id="49dd3-389">Lazy loading is *off* by default when working directly with an ObjectContext object.</span></span> <span data-ttu-id="49dd3-390">Существует свойство, мы можем установить на свойство ContextOptions для включения отложенной загрузки, и мы можем установить это свойство внутри нашей реальной единицы работы, если мы хотим, чтобы ленивый загрузки во всем мире.</span><span class="sxs-lookup"><span data-stu-id="49dd3-390">There is a property we can set on the ContextOptions property to enable deferred loading, and we can set this property inside our real unit of work if we want to enable lazy loading everywhere.</span></span>

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

<span data-ttu-id="49dd3-391">При неявной ленивой загрузке включена, код приложения может использовать сотрудников и связанные карты времени сотрудника, оставаясь в блаженном неведении о работе, необходимой для загрузки дополнительных данных EF.</span><span class="sxs-lookup"><span data-stu-id="49dd3-391">With implicit lazy loading enabled, application code can use an employee and the employee’s associated time cards while remaining blissfully unaware of the work required for EF to load the extra data.</span></span>

``` csharp
    var employee = _unitOfWork.Employees
                              .Single(e => e.Id == id);
    foreach (var card in employee.TimeCards) {
        // ...
    }
```

<span data-ttu-id="49dd3-392">Ленивая загрузка упрощает запись кода приложения, а с помощью прокси-магии код остается полностью проверяемым.</span><span class="sxs-lookup"><span data-stu-id="49dd3-392">Lazy loading makes the application code easier to write, and with the proxy magic the code remains completely testable.</span></span> <span data-ttu-id="49dd3-393">Подделки в памяти рабочего блока могут просто предзагружать поддельные объекты со связанными данными, когда это необходимо во время теста.</span><span class="sxs-lookup"><span data-stu-id="49dd3-393">In-memory fakes of the unit of work can simply preload fake entities with associated data when needed during a test.</span></span>

<span data-ttu-id="49dd3-394">На этом этапе мы обратим наше внимание от создания&lt;репозиторий с помощью IObjectSet T&gt; и рассмотрим абстракции, чтобы скрыть все признаки инфраструктуры сохранения.</span><span class="sxs-lookup"><span data-stu-id="49dd3-394">At this point we’ll turn our attention from building repositories using IObjectSet&lt;T&gt; and look at abstractions to hide all signs of the persistence framework.</span></span>

## <a name="custom-repositories"></a><span data-ttu-id="49dd3-395">Пользовательские репозитории</span><span class="sxs-lookup"><span data-stu-id="49dd3-395">Custom Repositories</span></span>

<span data-ttu-id="49dd3-396">Когда мы впервые представили в этой статье шаблон проектирования единицы работы, мы предоставили некоторый пример кода для того, как может выглядеть единица работы.</span><span class="sxs-lookup"><span data-stu-id="49dd3-396">When we first presented the unit of work design pattern in this article we provided some sample code for what the unit of work might look like.</span></span> <span data-ttu-id="49dd3-397">Давайте представим эту оригинальную идею, используя сценарий карты времени сотрудника и сотрудника, с которым мы работали.</span><span class="sxs-lookup"><span data-stu-id="49dd3-397">Let’s re-present this original idea using the employee and employee time card scenario we’ve been working with.</span></span>

``` csharp
    public interface IUnitOfWork {
        IRepository<Employee> Employees { get; }
        IRepository<TimeCard> TimeCards { get;  }
        void Commit();
    }
```

<span data-ttu-id="49dd3-398">Основное различие между этой единицей работы и единицей работы, которую мы создали в последнем разделе, заключается в&lt;том, как эта единица работы не использует никаких абстракций из фреймворка EF4 (нет IObjectSet T).&gt;</span><span class="sxs-lookup"><span data-stu-id="49dd3-398">The primary difference between this unit of work and the unit of work we created in the last section is how this unit of work does not use any abstractions from the EF4 framework (there is no IObjectSet&lt;T&gt;).</span></span> <span data-ttu-id="49dd3-399">IObjectSet&lt;&gt; T хорошо работает как интерфейс репозитория, но API, который он предоставляет, может не соответствовать потребностям нашего приложения.</span><span class="sxs-lookup"><span data-stu-id="49dd3-399">IObjectSet&lt;T&gt; works well as a repository interface, but the API it exposes might not perfectly align with our application’s needs.</span></span> <span data-ttu-id="49dd3-400">В этом предстоящем подходе мы будем представлять репозитории, используя пользовательские абстракции IRepository&lt;T.&gt;</span><span class="sxs-lookup"><span data-stu-id="49dd3-400">In this upcoming approach we will represent repositories using a custom IRepository&lt;T&gt; abstraction.</span></span>

<span data-ttu-id="49dd3-401">Многие разработчики, которые следуют тест-ориентированному дизайну, дизайну, управляемому поведением, и методологиям домена, предпочитают подход IRepository&lt;T&gt; по нескольким причинам.</span><span class="sxs-lookup"><span data-stu-id="49dd3-401">Many developers who follow test-driven design, behavior-driven design, and domain driven methodologies design prefer the IRepository&lt;T&gt; approach for several reasons.</span></span> <span data-ttu-id="49dd3-402">Во-первых, интерфейс IRepository&lt;T&gt; представляет собой слой «антикоррупционного» уровня.</span><span class="sxs-lookup"><span data-stu-id="49dd3-402">First, the IRepository&lt;T&gt; interface represents an “anti-corruption” layer.</span></span> <span data-ttu-id="49dd3-403">Как описано Эриком Эвансом в его книге Domain Driven Design, антикоррупционный слой удерживает ваш код домена от API инфраструктуры, как ИП.</span><span class="sxs-lookup"><span data-stu-id="49dd3-403">As described by Eric Evans in his Domain Driven Design book an anti-corruption layer keeps your domain code away from infrastructure APIs, like a persistence API.</span></span> <span data-ttu-id="49dd3-404">Во-вторых, разработчики могут создавать методы в репозиторий, которые отвечают точным потребностям приложения (как обнаружено при написании тестов).</span><span class="sxs-lookup"><span data-stu-id="49dd3-404">Secondly, developers can build methods into the repository that meet the exact needs of an application (as discovered while writing tests).</span></span> <span data-ttu-id="49dd3-405">Например, нам часто приходится находить одну сущность с помощью значения идентификатора, чтобы мы могли добавить метод FindById в интерфейс репозитория.</span><span class="sxs-lookup"><span data-stu-id="49dd3-405">For example, we might frequently need to locate a single entity using an ID value, so we can add a FindById method to the repository interface.</span></span><span data-ttu-id="49dd3-406">Наше определение IRepository&lt;T&gt; будет выглядеть следующим образом.</span><span class="sxs-lookup"><span data-stu-id="49dd3-406">  Our IRepository&lt;T&gt; definition will look like the following.</span></span>

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

<span data-ttu-id="49dd3-407">Обратите внимание, что мы вернемся к&lt;&gt; использованию интерфейса I'Siryable T для разоблачения коллекций сущностей.</span><span class="sxs-lookup"><span data-stu-id="49dd3-407">Notice we’ll drop back to using an IQueryable&lt;T&gt; interface to expose entity collections.</span></span> <span data-ttu-id="49dd3-408">Иски&lt;T&gt; позволяют деревьям выражения LIN'а вливаться в провайдер EF4 и дать поставщику целостное представление о запросе.</span><span class="sxs-lookup"><span data-stu-id="49dd3-408">IQueryable&lt;T&gt; allows LINQ expression trees to flow into the EF4 provider and give the provider a holistic view of the query.</span></span> <span data-ttu-id="49dd3-409">Вторым вариантом было бы возвращение IEnumerable&lt;T,&gt;что означает, что поставщик EF4 LIN' будет видеть только выражения, встроенные внутри репозитория.</span><span class="sxs-lookup"><span data-stu-id="49dd3-409">A second option would be to return IEnumerable&lt;T&gt;, which means the EF4 LINQ provider will only see the expressions built inside of the repository.</span></span> <span data-ttu-id="49dd3-410">Любая группировка, заказ и проекция, выполненная за пределами репозитория, не будут входить в команду S'L, отправленную в базу данных, что может повредить производительности.</span><span class="sxs-lookup"><span data-stu-id="49dd3-410">Any grouping, ordering, and projection done outside of the repository will not be composed into the SQL command sent to the database, which can hurt performance.</span></span> <span data-ttu-id="49dd3-411">С другой стороны, репозиторий, возвращающийся&lt;&gt; только с iEnumerable T-результатов, никогда не удивит вас новой командой S'L.</span><span class="sxs-lookup"><span data-stu-id="49dd3-411">On the other hand, a repository returning only IEnumerable&lt;T&gt; results will never surprise you with a new SQL command.</span></span> <span data-ttu-id="49dd3-412">Оба подхода будут работать, и оба подхода остаются проверяемыми.</span><span class="sxs-lookup"><span data-stu-id="49dd3-412">Both approaches will work, and both approaches remain testable.</span></span>

<span data-ttu-id="49dd3-413">Обеспечить единую реализацию интерфейса IRepository&lt;&gt; T с использованием генериков и API EF4 ObjectContext.</span><span class="sxs-lookup"><span data-stu-id="49dd3-413">It’s straightforward to provide a single implementation of the IRepository&lt;T&gt; interface using generics and the EF4 ObjectContext API.</span></span>

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

<span data-ttu-id="49dd3-414">Подход&lt;IRepository&gt; T дает нам дополнительный контроль над нашими запросами, потому что клиент должен вызвать метод, чтобы добраться до сущности.</span><span class="sxs-lookup"><span data-stu-id="49dd3-414">The IRepository&lt;T&gt; approach gives us some additional control over our queries because a client has to invoke a method to get to an entity.</span></span> <span data-ttu-id="49dd3-415">Внутри метода мы могли бы предоставить дополнительные проверки и операторов LIN'а для обеспечения ограничений приложений.</span><span class="sxs-lookup"><span data-stu-id="49dd3-415">Inside the method we could provide additional checks and LINQ operators to enforce application constraints.</span></span> <span data-ttu-id="49dd3-416">Обратите внимание, что интерфейс имеет два ограничения на общий параметр типа.</span><span class="sxs-lookup"><span data-stu-id="49dd3-416">Notice the interface has two constraints on the generic type parameter.</span></span> <span data-ttu-id="49dd3-417">Первым ограничением является запятнание класса&lt;конс, требуемое ObjectSet T,&gt;а второе ограничение вынуждает наши сущности реализовывать IEntity – абстракцию, созданную для приложения.</span><span class="sxs-lookup"><span data-stu-id="49dd3-417">The first constraint is the class cons taint required by ObjectSet&lt;T&gt;, and the second constraint forces our entities to implement IEntity – an abstraction created for the application.</span></span> <span data-ttu-id="49dd3-418">Интерфейс IEntity заставляет сущности иметь свойство читаемого идона, и мы можем использовать это свойство в методе FindById.</span><span class="sxs-lookup"><span data-stu-id="49dd3-418">The IEntity interface forces entities to have a readable Id property, and we can then use this property in the FindById method.</span></span> <span data-ttu-id="49dd3-419">IEntity определяется со следующим кодом.</span><span class="sxs-lookup"><span data-stu-id="49dd3-419">IEntity is defined with the following code.</span></span>

``` csharp
    public interface IEntity {
        int Id { get; }
    }
```

<span data-ttu-id="49dd3-420">IEntity можно считать небольшим нарушением незнания настойчивости, поскольку наши организации обязаны реализовать этот интерфейс.</span><span class="sxs-lookup"><span data-stu-id="49dd3-420">IEntity could be considered a small violation of persistence ignorance since our entities are required to implement this interface.</span></span> <span data-ttu-id="49dd3-421">Помните, что сохранение невежества о компромиссов, и для многих функции FindById будет перевешивать ограничения, введенные интерфейсом.</span><span class="sxs-lookup"><span data-stu-id="49dd3-421">Remember persistence ignorance is about tradeoffs, and for many the FindById functionality will outweigh the constraint imposed by the interface.</span></span> <span data-ttu-id="49dd3-422">Интерфейс не влияет на тестируемость.</span><span class="sxs-lookup"><span data-stu-id="49dd3-422">The interface has no impact on testability.</span></span>

<span data-ttu-id="49dd3-423">Мгновенное воспроизведение&lt;iRepository&gt; T требует EF4 ObjectContext, поэтому конкретная единица реализации работы должна управлять моментом.</span><span class="sxs-lookup"><span data-stu-id="49dd3-423">Instantiating a live IRepository&lt;T&gt; requires an EF4 ObjectContext, so a concrete unit of work implementation should manage the instantiation.</span></span>

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

### <a name="using-the-custom-repository"></a><span data-ttu-id="49dd3-424">Использование пользовательского репозитория</span><span class="sxs-lookup"><span data-stu-id="49dd3-424">Using the Custom Repository</span></span>

<span data-ttu-id="49dd3-425">Использование нашего пользовательского репозитория существенно не отличается от использования&lt;&gt;репозитория на основе IObjectSet T .</span><span class="sxs-lookup"><span data-stu-id="49dd3-425">Using our custom repository is not significantly different from using the repository based on IObjectSet&lt;T&gt;.</span></span> <span data-ttu-id="49dd3-426">Вместо того, чтобы применять операторы LIN'а непосредственно к свойству, нам сначала нужно вызвать один метод репозитория, чтобы захватить ссылку на I'Eeryable&lt;T.&gt;</span><span class="sxs-lookup"><span data-stu-id="49dd3-426">Instead of applying LINQ operators directly to a property, we’ll first need to invoke one the repository’s methods to grab an IQueryable&lt;T&gt; reference.</span></span>

``` csharp
    public ViewResult Index() {
        var model = _repository.FindAll()
                               .Include("TimeCards")
                               .OrderBy(e => e.HireDate);
        return View(model);
    }
```

<span data-ttu-id="49dd3-427">Обратите внимание, пользовательский Оператор Включить мы реализовали ранее будет работать без изменений.</span><span class="sxs-lookup"><span data-stu-id="49dd3-427">Notice the custom Include operator we implemented previously will work without change.</span></span> <span data-ttu-id="49dd3-428">Метод FindById репозитория удаляет дублированную логику из действий, пытающихся получить одну сущность.</span><span class="sxs-lookup"><span data-stu-id="49dd3-428">The repository’s FindById method removes duplicated logic from actions trying to retrieve a single entity.</span></span>

``` csharp
    public ViewResult Details(int id) {
        var model = _repository.FindById(id);
        return View(model);
    }
```

<span data-ttu-id="49dd3-429">Существенной разницы в тестируемости двух рассмотренных подходов нет.</span><span class="sxs-lookup"><span data-stu-id="49dd3-429">There is no significant difference in the testability of the two approaches we’ve examined.</span></span> <span data-ttu-id="49dd3-430">Мы могли бы предоставить поддельные&lt;реализации IRepository&gt; T&lt;&gt; путем создания конкретных классов при поддержке сотрудника HashSet - так же, как то, что мы сделали в последнем разделе.</span><span class="sxs-lookup"><span data-stu-id="49dd3-430">We could provide fake implementations of IRepository&lt;T&gt; by building concrete classes backed by HashSet&lt;Employee&gt; - just like what we did in the last section.</span></span> <span data-ttu-id="49dd3-431">Однако некоторые разработчики предпочитают использовать макеты объектов и макет объектов, а не строить подделки.</span><span class="sxs-lookup"><span data-stu-id="49dd3-431">However, some developers prefer to use mock objects and mock object frameworks instead of building fakes.</span></span> <span data-ttu-id="49dd3-432">Мы рассмотрим использование макетов для проверки нашей реализации и обсуждения различий между макетами и подделками в следующем разделе.</span><span class="sxs-lookup"><span data-stu-id="49dd3-432">We’ll look at using mocks to test our implementation and discuss the differences between mocks and fakes in the next section.</span></span>

### <a name="testing-with-mocks"></a><span data-ttu-id="49dd3-433">Тестирование с помощью моков</span><span class="sxs-lookup"><span data-stu-id="49dd3-433">Testing with Mocks</span></span>

<span data-ttu-id="49dd3-434">Существуют различные подходы к созданию того, что Мартин Фаулер называет "тест-двойником".</span><span class="sxs-lookup"><span data-stu-id="49dd3-434">There are different approaches to building what Martin Fowler calls a “test double”.</span></span> <span data-ttu-id="49dd3-435">Тест овый (например, трюк трюк фильм двойной) является объектом, который вы строите, чтобы "стоять в" для реальных, производственных объектов во время испытаний.</span><span class="sxs-lookup"><span data-stu-id="49dd3-435">A test double (like a movie stunt double) is an object you build to “stand in” for real, production objects during tests.</span></span> <span data-ttu-id="49dd3-436">Созданные нами хранилища в памяти — это тестовые двойники для репозиториев, которые разговаривают с сервером S'L.</span><span class="sxs-lookup"><span data-stu-id="49dd3-436">The in-memory repositories we created are test doubles for the repositories that talk to SQL Server.</span></span> <span data-ttu-id="49dd3-437">Мы видели, как использовать эти тестовые двойники во время модульных тестов для изоляции кода и быстрого выполнения тестов.</span><span class="sxs-lookup"><span data-stu-id="49dd3-437">We’ve seen how to use these test-doubles during the unit tests to isolate code and keep tests running fast.</span></span>

<span data-ttu-id="49dd3-438">Тестовые двойники, которые мы создали, имеют реальные рабочие реализации.</span><span class="sxs-lookup"><span data-stu-id="49dd3-438">The test doubles we’ve built have real, working implementations.</span></span> <span data-ttu-id="49dd3-439">За кулисами каждый из них хранит конкретную коллекцию объектов, и они будут добавлять и удалять объекты из этой коллекции, как мы манипулируем репозиторий во время теста.</span><span class="sxs-lookup"><span data-stu-id="49dd3-439">Behind the scenes each one stores a concrete collection of objects, and they will add and remove objects from this collection as we manipulate the repository during a test.</span></span> <span data-ttu-id="49dd3-440">Некоторым разработчикам нравится создавать свои тесты в два раза таким образом – с реальным кодом и рабочими реализациями.</span><span class="sxs-lookup"><span data-stu-id="49dd3-440">Some developers like to build their test doubles this way – with real code and working implementations.</span></span><span data-ttu-id="49dd3-441">Эти тест удваивает это то, что мы называем *подделками*.</span><span class="sxs-lookup"><span data-stu-id="49dd3-441">  These test doubles are what we call *fakes*.</span></span> <span data-ttu-id="49dd3-442">У них есть рабочие реализации, но они не являются достаточно реальными для использования в производстве.</span><span class="sxs-lookup"><span data-stu-id="49dd3-442">They have working implementations, but they aren’t real enough for production use.</span></span> <span data-ttu-id="49dd3-443">Поддельный репозиторий на самом деле не записывается в базу данных.</span><span class="sxs-lookup"><span data-stu-id="49dd3-443">The fake repository doesn’t actually write to the database.</span></span> <span data-ttu-id="49dd3-444">Поддельный сервер SMTP на самом деле не отправить сообщение электронной почты по сети.</span><span class="sxs-lookup"><span data-stu-id="49dd3-444">The fake SMTP server doesn’t actually send an email message over the network.</span></span>

### <a name="mocks-versus-fakes"></a><span data-ttu-id="49dd3-445">Макеты против подделок</span><span class="sxs-lookup"><span data-stu-id="49dd3-445">Mocks versus Fakes</span></span>

<span data-ttu-id="49dd3-446">Существует еще один тип теста двойной известный как *макет*.</span><span class="sxs-lookup"><span data-stu-id="49dd3-446">There is another type of test double known as a *mock*.</span></span> <span data-ttu-id="49dd3-447">В то время как подделки имеют рабочие реализации, макеты поставляются без реализации.</span><span class="sxs-lookup"><span data-stu-id="49dd3-447">While fakes have working implementations, mocks come with no implementation.</span></span> <span data-ttu-id="49dd3-448">С помощью макета инфраструктуры объекта мы строим эти макетные объекты во время выполнения и используем их в качестве тестового двойника.</span><span class="sxs-lookup"><span data-stu-id="49dd3-448">With the help of a mock object framework we construct these mock objects at run time and use them as test doubles.</span></span> <span data-ttu-id="49dd3-449">В этом разделе мы будем использовать платформу с открытым исходным кодом Moq.</span><span class="sxs-lookup"><span data-stu-id="49dd3-449">In this section we’ll be using the open source mocking framework Moq.</span></span> <span data-ttu-id="49dd3-450">Вот простой пример использования Moq для динамического создания тестового дубля для репозитория сотрудников.</span><span class="sxs-lookup"><span data-stu-id="49dd3-450">Here is a simple example of using Moq to dynamically create a test double for an employee repository.</span></span>

``` csharp
    Mock<IRepository<Employee>> mock =
        new Mock<IRepository<Employee>>();
    IRepository<Employee> repository = mock.Object;
    repository.Add(new Employee());
    var employee = repository.FindById(1);
```

<span data-ttu-id="49dd3-451">Мы просим Moq для реализации&lt;&gt; IRepository сотрудника, и он строит один динамически.</span><span class="sxs-lookup"><span data-stu-id="49dd3-451">We ask Moq for an IRepository&lt;Employee&gt; implementation and it builds one dynamically.</span></span> <span data-ttu-id="49dd3-452">Мы можем добраться до объекта,&lt;реализующего IRepository Employee,&gt; приостановив доступ к свойству объекта объекта Mock&lt;T.&gt;</span><span class="sxs-lookup"><span data-stu-id="49dd3-452">We can get to the object implementing IRepository&lt;Employee&gt; by accessing the Object property of the Mock&lt;T&gt; object.</span></span> <span data-ttu-id="49dd3-453">Именно этот внутренний объект мы можем передать в наши контроллеры, и они не будут знать, если это тест двойной или реальный репозиторий.</span><span class="sxs-lookup"><span data-stu-id="49dd3-453">It is this inner object we can pass into our controllers, and they won’t know if this is a test double or the real repository.</span></span> <span data-ttu-id="49dd3-454">Мы можем ссылаться на методы на объекте так же, как мы хотели бы вызвать методы на объекте с реальной реализацией.</span><span class="sxs-lookup"><span data-stu-id="49dd3-454">We can invoke methods on the object just like we would invoke methods on an object with a real implementation.</span></span>

<span data-ttu-id="49dd3-455">Вы должны быть удивлены, что макет репозитория будет делать, когда мы ссылаемся на метод добавления.</span><span class="sxs-lookup"><span data-stu-id="49dd3-455">You must be wondering what the mock repository will do when we invoke the Add method.</span></span> <span data-ttu-id="49dd3-456">Поскольку за объектом макета нет реализации, Добавить ничего не делает.</span><span class="sxs-lookup"><span data-stu-id="49dd3-456">Since there is no implementation behind the mock object, Add does nothing.</span></span> <span data-ttu-id="49dd3-457">Существует нет конкретной коллекции за кулисами, как мы имели с подделками мы написали, так что сотрудник отбрасывается.</span><span class="sxs-lookup"><span data-stu-id="49dd3-457">There is no concrete collection behind the scenes like we had with the fakes we wrote, so the employee is discarded.</span></span> <span data-ttu-id="49dd3-458">Как насчет возврата стоимости FindById?</span><span class="sxs-lookup"><span data-stu-id="49dd3-458">What about the return value of FindById?</span></span> <span data-ttu-id="49dd3-459">В этом случае объект макета делает единственное, что он может сделать, то есть вернуть значение по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="49dd3-459">In this case the mock object does the only thing it can do, which is return a default value.</span></span> <span data-ttu-id="49dd3-460">Поскольку мы возвращаем тип ссылки (сотрудник), значение возврата является нулевым значением.</span><span class="sxs-lookup"><span data-stu-id="49dd3-460">Since we are returning a reference type (an Employee), the return value is a null value.</span></span>

<span data-ttu-id="49dd3-461">Mocks может показаться бесполезным; однако, есть еще две особенности макетов, о которых мы не говорили.</span><span class="sxs-lookup"><span data-stu-id="49dd3-461">Mocks might sound worthless; however, there are two more features of mocks we haven’t talked about.</span></span> <span data-ttu-id="49dd3-462">Во-первых, фреймворк Moq записывает все вызовы, сделанные на макет объекта.</span><span class="sxs-lookup"><span data-stu-id="49dd3-462">First, the Moq framework records all the calls made on the mock object.</span></span> <span data-ttu-id="49dd3-463">Позже в коде мы можем спросить Moq, ссылался ли кто-либо на метод добавления, или если кто-то сослался на метод FindById.</span><span class="sxs-lookup"><span data-stu-id="49dd3-463">Later in the code we can ask Moq if anyone invoked the Add method, or if anyone invoked the FindById method.</span></span> <span data-ttu-id="49dd3-464">Мы увидим позже, как мы можем использовать этот "черный ящик" функцию записи в тестах.</span><span class="sxs-lookup"><span data-stu-id="49dd3-464">We’ll see later how we can use this “black box” recording feature in tests.</span></span>

<span data-ttu-id="49dd3-465">Вторая большая особенность заключается в том, как мы можем использовать Moq для программы макет объекта с *ожиданиями*.</span><span class="sxs-lookup"><span data-stu-id="49dd3-465">The second great feature is how we can use Moq to program a mock object with *expectations*.</span></span> <span data-ttu-id="49dd3-466">Ожидание говорит макет объект, как реагировать на любое взаимодействие.</span><span class="sxs-lookup"><span data-stu-id="49dd3-466">An expectation tells the mock object how to respond to any given interaction.</span></span> <span data-ttu-id="49dd3-467">Например, мы можем запрограммировать ожидание в наш макет и сказать ему, чтобы вернуть объект сотрудника, когда кто-то вызывает FindById.</span><span class="sxs-lookup"><span data-stu-id="49dd3-467">For example, we can program an expectation into our mock and tell it to return an employee object when someone invokes FindById.</span></span> <span data-ttu-id="49dd3-468">Для программы этих ожиданий в фрейм-системе Moq используется API Setup API и lambda.</span><span class="sxs-lookup"><span data-stu-id="49dd3-468">The Moq framework uses a Setup API and lambda expressions to program these expectations.</span></span>

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

<span data-ttu-id="49dd3-469">В этом примере мы просим Moq динамически построить репозиторий, а затем мы программировать репозиторий с ожиданием.</span><span class="sxs-lookup"><span data-stu-id="49dd3-469">In this sample we ask Moq to dynamically build a repository, and then we program the repository with an expectation.</span></span> <span data-ttu-id="49dd3-470">Ожидание говорит объекту макета, чтобы вернуть новый объект сотрудника со значением Id 5, когда кто-то вызывает метод FindById, проходящий значение 5.</span><span class="sxs-lookup"><span data-stu-id="49dd3-470">The expectation tells the mock object to return a new employee object with an Id value of 5 when someone invokes the FindById method passing a value of 5.</span></span> <span data-ttu-id="49dd3-471">Этот тест проходит, и нам не нужно было строить полную&lt;реализацию, чтобы подделать IRepository T&gt;.</span><span class="sxs-lookup"><span data-stu-id="49dd3-471">This test passes, and we didn’t need to build a full implementation to fake IRepository&lt;T&gt;.</span></span>

<span data-ttu-id="49dd3-472">Давайте вернемся к тестам, которые мы писали ранее, и переработаем их, чтобы использовать макеты вместо подделок.</span><span class="sxs-lookup"><span data-stu-id="49dd3-472">Let’s revisit the tests we wrote earlier and rework them to use mocks instead of fakes.</span></span> <span data-ttu-id="49dd3-473">Как и раньше, мы будем использовать базовый класс для настройки общих элементов инфраструктуры, необходимых для всех тестов контроллера.</span><span class="sxs-lookup"><span data-stu-id="49dd3-473">Just like before, we’ll use a base class to setup the common pieces of infrastructure we need for all of the controller’s tests.</span></span>

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

<span data-ttu-id="49dd3-474">Код настройки остается в основном неизменным.</span><span class="sxs-lookup"><span data-stu-id="49dd3-474">The setup code remains mostly the same.</span></span> <span data-ttu-id="49dd3-475">Вместо того, чтобы использовать подделки, мы будем использовать Moq для построения макет объектов.</span><span class="sxs-lookup"><span data-stu-id="49dd3-475">Instead of using fakes, we’ll use Moq to construct mock objects.</span></span> <span data-ttu-id="49dd3-476">Базовый класс организует для макет ампорированной единицы работы, чтобы вернуть макет репозитория, когда код вызывает свойство сотрудников.</span><span class="sxs-lookup"><span data-stu-id="49dd3-476">The base class arranges for the mock unit of work to return a mock repository when code invokes the Employees property.</span></span> <span data-ttu-id="49dd3-477">Остальная часть установки макета будет проходить внутри тестовых светильников, посвященных каждому конкретному сценарию.</span><span class="sxs-lookup"><span data-stu-id="49dd3-477">The rest of the mock setup will take place inside the test fixtures dedicated to each specific scenario.</span></span> <span data-ttu-id="49dd3-478">Например, тестовый прибор для действия Индекса настраивает макет репозитория, чтобы вернуть список сотрудников, когда действие вызывает метод FindAll в репозитории макета.</span><span class="sxs-lookup"><span data-stu-id="49dd3-478">For example, the test fixture for the Index action will setup the mock repository to return a list of employees when the action invokes the FindAll method of the mock repository.</span></span>

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

<span data-ttu-id="49dd3-479">За исключением ожиданий, наши тесты похожи на те тесты, которые мы проводили раньше.</span><span class="sxs-lookup"><span data-stu-id="49dd3-479">Except for the expectations, our tests look similar to the tests we had before.</span></span> <span data-ttu-id="49dd3-480">Однако, с возможностью записи макета фреймворка, мы можем подходить к тестированию под другим углом зрения.</span><span class="sxs-lookup"><span data-stu-id="49dd3-480">However, with the recording ability of a mock framework we can approach testing from a different angle.</span></span> <span data-ttu-id="49dd3-481">Мы рассмотрим эту новую перспективу в следующем разделе.</span><span class="sxs-lookup"><span data-stu-id="49dd3-481">We’ll look at this new perspective in the next section.</span></span>

### <a name="state-versus-interaction-testing"></a><span data-ttu-id="49dd3-482">Тестирование состояния против взаимодействия</span><span class="sxs-lookup"><span data-stu-id="49dd3-482">State versus Interaction Testing</span></span>

<span data-ttu-id="49dd3-483">Существуют различные методы, которые можно использовать для тестирования программного обеспечения с макет объектов.</span><span class="sxs-lookup"><span data-stu-id="49dd3-483">There are different techniques you can use to test software with mock objects.</span></span> <span data-ttu-id="49dd3-484">Один из подходов заключается в использовании государственного тестирования, что мы и сделали в настоящем документе до сих пор.</span><span class="sxs-lookup"><span data-stu-id="49dd3-484">One approach is to use state based testing, which is what we have done in this paper so far.</span></span> <span data-ttu-id="49dd3-485">Государственное тестирование делает утверждения о состоянии программного обеспечения.</span><span class="sxs-lookup"><span data-stu-id="49dd3-485">State based testing makes assertions about the state of the software.</span></span> <span data-ttu-id="49dd3-486">В последнем тесте мы сослались на метод действия на контроллере и сделали утверждение о модели, которую он должен построить.</span><span class="sxs-lookup"><span data-stu-id="49dd3-486">In the last test we invoked an action method on the controller and made an assertion about the model it should build.</span></span> <span data-ttu-id="49dd3-487">Вот некоторые другие примеры состояния тестирования:</span><span class="sxs-lookup"><span data-stu-id="49dd3-487">Here are some other examples of testing state:</span></span>

-   <span data-ttu-id="49dd3-488">Проверка репозитория содержит новый объект сотрудника после выполнения Create.</span><span class="sxs-lookup"><span data-stu-id="49dd3-488">Verify the repository contains the new employee object after Create executes.</span></span>
-   <span data-ttu-id="49dd3-489">Проверка модели содержит список всех сотрудников после выполнения Index.</span><span class="sxs-lookup"><span data-stu-id="49dd3-489">Verify the model holds a list of all employees after Index executes.</span></span>
-   <span data-ttu-id="49dd3-490">Проверка репозитория не содержит данного сотрудника после выполнения удаления.</span><span class="sxs-lookup"><span data-stu-id="49dd3-490">Verify the repository does not contain a given employee after Delete executes.</span></span>

<span data-ttu-id="49dd3-491">Другой подход, который вы увидите с объектами макета, заключается в проверке *взаимодействий.*</span><span class="sxs-lookup"><span data-stu-id="49dd3-491">Another approach you’ll see with mock objects is to verify *interactions*.</span></span> <span data-ttu-id="49dd3-492">В то время как тестирование на основе состояния делает утверждения о состоянии объектов, тестирование на основе взаимодействия делает утверждения о том, как объекты взаимодействуют.</span><span class="sxs-lookup"><span data-stu-id="49dd3-492">While state based testing makes assertions about the state of objects, interaction based testing makes assertions about how objects interact.</span></span> <span data-ttu-id="49dd3-493">Пример:</span><span class="sxs-lookup"><span data-stu-id="49dd3-493">For example:</span></span>

-   <span data-ttu-id="49dd3-494">Проверка контроллера вызывает метод добавления репозитория при выполнении create.</span><span class="sxs-lookup"><span data-stu-id="49dd3-494">Verify the controller invokes the repository’s Add method when Create executes.</span></span>
-   <span data-ttu-id="49dd3-495">Проверка контроллера вызывает метод FindAll репозитория при выполнении Индекса.</span><span class="sxs-lookup"><span data-stu-id="49dd3-495">Verify the controller invokes the repository’s FindAll method when Index executes.</span></span>
-   <span data-ttu-id="49dd3-496">Проверка контроллера вызывает блок выполнения блока работы для сохранения изменений при выполнении edit.</span><span class="sxs-lookup"><span data-stu-id="49dd3-496">Verify the controller invokes the unit of work’s Commit method to save changes when Edit executes.</span></span>

<span data-ttu-id="49dd3-497">Взаимодействие тестирования часто требует меньше тестовых данных, потому что мы не тыкать внутри коллекций и проверки количества.</span><span class="sxs-lookup"><span data-stu-id="49dd3-497">Interaction testing often requires less test data, because we aren’t poking inside of collections and verifying counts.</span></span> <span data-ttu-id="49dd3-498">Например, если мы знаем, что действие Детали вызывает метод FindById репозитория с правильным значением - то действие, вероятно, ведет себя правильно.</span><span class="sxs-lookup"><span data-stu-id="49dd3-498">For example, if we know the Details action invokes a repository’s FindById method with the correct value - then the action is probably behaving correctly.</span></span> <span data-ttu-id="49dd3-499">Мы можем проверить это поведение без настройки каких-либо тестовых данных для возвращения из FindById.</span><span class="sxs-lookup"><span data-stu-id="49dd3-499">We can verify this behavior without setting up any test data to return from FindById.</span></span>

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

<span data-ttu-id="49dd3-500">Единственной установкой, необходимой в вышеуказанном тестовом приборе, является установка, предоставляемая базовым классом.</span><span class="sxs-lookup"><span data-stu-id="49dd3-500">The only setup required in the above test fixture is the setup provided by the base class.</span></span> <span data-ttu-id="49dd3-501">Когда мы ссылаемся на действие контроллера, Moq записывает взаимодействия с репозиторием макета.</span><span class="sxs-lookup"><span data-stu-id="49dd3-501">When we invoke the controller action, Moq will record the interactions with the mock repository.</span></span> <span data-ttu-id="49dd3-502">Используя Verify API Moq, мы можем спросить Moq, если контроллер вызвал FindById с соответствующим значением id.</span><span class="sxs-lookup"><span data-stu-id="49dd3-502">Using the Verify API of Moq, we can ask Moq if the controller invoked FindById with the proper ID value.</span></span> <span data-ttu-id="49dd3-503">Если контроллер не ссылался на метод или ссылался на метод с неожиданным значением параметра, метод Verify будет завершать и тест не будет.</span><span class="sxs-lookup"><span data-stu-id="49dd3-503">If the controller did not invoke the method, or invoked the method with an unexpected parameter value, the Verify method will throw an exception and the test will fail.</span></span>

<span data-ttu-id="49dd3-504">Вот еще один пример для проверки действия Create вызывает commit на текущей единице работы.</span><span class="sxs-lookup"><span data-stu-id="49dd3-504">Here is another example to verify the Create action invokes Commit on the current unit of work.</span></span>

``` csharp
    [TestMethod]
    public void ShouldCommitUnitOfWork() {
        _controller.Create(_newEmployee);
        _unitOfWork.Verify(u => u.Commit());
    }
```

<span data-ttu-id="49dd3-505">Одной из опасностей при тестировании взаимодействия является тенденция к чрезмерному указанию взаимодействий.</span><span class="sxs-lookup"><span data-stu-id="49dd3-505">One danger with interaction testing is the tendency to over specify interactions.</span></span> <span data-ttu-id="49dd3-506">Способность макетного объекта записывать и проверять каждое взаимодействие с объектом макета не означает, что тест должен попытаться проверить каждое взаимодействие.</span><span class="sxs-lookup"><span data-stu-id="49dd3-506">The ability of the mock object to record and verify every interaction with the mock object doesn’t mean the test should try to verify every interaction.</span></span> <span data-ttu-id="49dd3-507">Некоторые взаимодействия являются деталями реализации, и вы должны только проверить взаимодействия, *необходимые* для удовлетворения текущего теста.</span><span class="sxs-lookup"><span data-stu-id="49dd3-507">Some interactions are implementation details and you should only verify the interactions *required* to satisfy the current test.</span></span>

<span data-ttu-id="49dd3-508">Выбор между макетами или подделками во многом зависит от системы, которая вы тестируете, и ваших личных (или командных) предпочтений.</span><span class="sxs-lookup"><span data-stu-id="49dd3-508">The choice between mocks or fakes largely depends on the system you are testing and your personal (or team) preferences.</span></span> <span data-ttu-id="49dd3-509">Объекты Mock могут значительно сократить количество кода, необходимого для реализации тестовых дублера, но не всем комфортно программировать ожидания и проверять взаимодействия.</span><span class="sxs-lookup"><span data-stu-id="49dd3-509">Mock objects can drastically reduce the amount of code you need to implement test doubles, but not everyone is comfortable programming expectations and verifying interactions.</span></span>

## <a name="conclusions"></a><span data-ttu-id="49dd3-510">Выводы</span><span class="sxs-lookup"><span data-stu-id="49dd3-510">Conclusions</span></span>

<span data-ttu-id="49dd3-511">В этой статье мы продемонстрировали несколько подходов к созданию тестируемого кода при использовании ADO.NET возможностей для сохранения данных.</span><span class="sxs-lookup"><span data-stu-id="49dd3-511">In this paper we’ve demonstrated several approaches to creating testable code while using the ADO.NET Entity Framework for data persistence.</span></span> <span data-ttu-id="49dd3-512">Мы можем использовать встроенные в&lt;абстракции, как IObjectSet T&gt;,&lt;&gt;или создать наши собственные абстракции, как IRepository T .</span><span class="sxs-lookup"><span data-stu-id="49dd3-512">We can leverage built in abstractions like IObjectSet&lt;T&gt;, or create our own abstractions like IRepository&lt;T&gt;.</span></span><span data-ttu-id="49dd3-513">В обоих случаях поддержка POCO в ADO.NET entity Framework 4.0 позволяет потребителям этих абстракций оставаться постоянными невежественным и высоко проверяемыми.</span><span class="sxs-lookup"><span data-stu-id="49dd3-513">  In both cases, the POCO support in the ADO.NET Entity Framework 4.0 allows the consumers of these abstractions to remain persistent ignorant and highly testable.</span></span> <span data-ttu-id="49dd3-514">Дополнительные функции EF4, такие как неявная ленивая загрузка, позволяют бизнесу и коду обслуживания приложений работать, не беспокоясь о деталях релятивного хранилища данных.</span><span class="sxs-lookup"><span data-stu-id="49dd3-514">Additional EF4 features like implicit lazy loading allows business and application service code to work without worrying about the details of a relational data store.</span></span> <span data-ttu-id="49dd3-515">Наконец, абстракции, которые мы создаем, легко насмехаются или подделывают внутри модульных тестов, и мы можем использовать эти двойные тесты для достижения быстрой работы, сильно изолированных и надежных тестов.</span><span class="sxs-lookup"><span data-stu-id="49dd3-515">Finally, the abstractions we create are easy to mock or fake inside of unit tests, and we can use these test doubles to achieve fast running, highly isolated, and reliable tests.</span></span>

### <a name="additional-resources"></a><span data-ttu-id="49dd3-516">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="49dd3-516">Additional Resources</span></span>

-   <span data-ttu-id="49dd3-517">Роберт К. Мартин, [«Принцип единой ответственности](https://www.objectmentor.com/resources/articles/srp.pdf)»</span><span class="sxs-lookup"><span data-stu-id="49dd3-517">Robert C. Martin, “ [The Single Responsibility Principle](https://www.objectmentor.com/resources/articles/srp.pdf)”</span></span>
-   <span data-ttu-id="49dd3-518">Мартин Фаулер, [Каталог шаблонов](https://www.martinfowler.com/eaaCatalog/index.html) из *шаблонов архитектуры корпоративных приложений*</span><span class="sxs-lookup"><span data-stu-id="49dd3-518">Martin Fowler, [Catalog of Patterns](https://www.martinfowler.com/eaaCatalog/index.html) from *Patterns of Enterprise Application Architecture*</span></span>
-   <span data-ttu-id="49dd3-519">Гриффин Каприо, " [Инъекции зависимостей](https://msdn.microsoft.com/magazine/cc163739.aspx)"</span><span class="sxs-lookup"><span data-stu-id="49dd3-519">Griffin Caprio, “ [Dependency Injection](https://msdn.microsoft.com/magazine/cc163739.aspx)”</span></span>
-   <span data-ttu-id="49dd3-520">Блог о программируемости данных, « [Прохождение: Разработка тестовых управляемых с помощью Entity Framework 4.0](https://docs.microsoft.com/archive/blogs/adonet/walkthrough-test-driven-development-with-the-entity-framework-4-0)».</span><span class="sxs-lookup"><span data-stu-id="49dd3-520">Data Programmability Blog, “ [Walkthrough: Test Driven Development with the Entity Framework 4.0](https://docs.microsoft.com/archive/blogs/adonet/walkthrough-test-driven-development-with-the-entity-framework-4-0)”.</span></span>
-   <span data-ttu-id="49dd3-521">Блог о программируемости данных, [«Использование репозитория и шаблонов единицы работы с рамками сущности 4.0](https://docs.microsoft.com/archive/blogs/adonet/using-repository-and-unit-of-work-patterns-with-entity-framework-4-0)»</span><span class="sxs-lookup"><span data-stu-id="49dd3-521">Data Programmability Blog, “ [Using Repository and Unit of Work patterns with Entity Framework 4.0](https://docs.microsoft.com/archive/blogs/adonet/using-repository-and-unit-of-work-patterns-with-entity-framework-4-0)”</span></span>
-   <span data-ttu-id="49dd3-522">Аарон Дженсен, ["Представляем технические характеристики машин"](http://codebetter.com/blogs/aaron.jensen/archive/2008/05/08/introducing-machine-specifications-or-mspec-for-short.aspx)</span><span class="sxs-lookup"><span data-stu-id="49dd3-522">Aaron Jensen, “ [Introducing Machine Specifications](http://codebetter.com/blogs/aaron.jensen/archive/2008/05/08/introducing-machine-specifications-or-mspec-for-short.aspx)”</span></span>
-   <span data-ttu-id="49dd3-523">Эрик Ли, ["BDD с MSTest](https://saintgimp.org/2009/01/20/bdd-with-mstest/)"</span><span class="sxs-lookup"><span data-stu-id="49dd3-523">Eric Lee, “ [BDD with MSTest](https://saintgimp.org/2009/01/20/bdd-with-mstest/)”</span></span>
-   <span data-ttu-id="49dd3-524">Эрик Эванс, " [Domain Driven Дизайн](https://books.google.com/books?id=7dlaMs0SECsC&printsec=frontcover&dq=evans%20domain%20driven%20design&hl=en&ei=cHztS6C8KIaglAfA_dS1CA&sa=X&oi=book_result&ct=result&resnum=1&ved=0CCoQ6AEwAA)"</span><span class="sxs-lookup"><span data-stu-id="49dd3-524">Eric Evans, “ [Domain Driven Design](https://books.google.com/books?id=7dlaMs0SECsC&printsec=frontcover&dq=evans%20domain%20driven%20design&hl=en&ei=cHztS6C8KIaglAfA_dS1CA&sa=X&oi=book_result&ct=result&resnum=1&ved=0CCoQ6AEwAA)”</span></span>
-   <span data-ttu-id="49dd3-525">Мартин Фаулер, ["Мокс не Стубс"](https://martinfowler.com/articles/mocksArentStubs.html)</span><span class="sxs-lookup"><span data-stu-id="49dd3-525">Martin Fowler, “ [Mocks Aren’t Stubs](https://martinfowler.com/articles/mocksArentStubs.html)”</span></span>
-   <span data-ttu-id="49dd3-526">Мартин Фаулер, " [Тест Двойной](https://martinfowler.com/bliki/TestDouble.html)"</span><span class="sxs-lookup"><span data-stu-id="49dd3-526">Martin Fowler, “ [Test Double](https://martinfowler.com/bliki/TestDouble.html)”</span></span>
-   [<span data-ttu-id="49dd3-527">Moq</span><span class="sxs-lookup"><span data-stu-id="49dd3-527">Moq</span></span>](https://code.google.com/p/moq/)

### <a name="biography"></a><span data-ttu-id="49dd3-528">Биография</span><span class="sxs-lookup"><span data-stu-id="49dd3-528">Biography</span></span>

<span data-ttu-id="49dd3-529">Скотт Аллен (Scott Allen) является членом технического персонала Pluralsight и основателем OdeToCode.com.</span><span class="sxs-lookup"><span data-stu-id="49dd3-529">Scott Allen is a member of the technical staff at Pluralsight and the founder of OdeToCode.com.</span></span> <span data-ttu-id="49dd3-530">За 15 лет коммерческой разработки программного обеспечения Скотт работал над решениями для всего, от 8-битных встроенных устройств до высокомасштабируемых ASP.NET веб-приложений.</span><span class="sxs-lookup"><span data-stu-id="49dd3-530">In 15 years of commercial software development, Scott has worked on solutions for everything from 8-bit embedded devices to highly scalable ASP.NET web applications.</span></span> <span data-ttu-id="49dd3-531">Вы можете связаться со Скоттом в своем блоге [https://twitter.com/OdeToCode](https://twitter.com/OdeToCode)на OdeToCode, или на Twitter в .</span><span class="sxs-lookup"><span data-stu-id="49dd3-531">You can reach Scott on his blog at OdeToCode, or on Twitter at [https://twitter.com/OdeToCode](https://twitter.com/OdeToCode).</span></span>
