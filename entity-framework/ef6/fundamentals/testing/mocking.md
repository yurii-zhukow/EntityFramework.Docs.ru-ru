---
title: Тестирование с помощью инфраструктуру макетирования - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: bd66a638-d245-44d4-8e71-b9c6cb335cc7
ms.openlocfilehash: 3d39b41018beb70b72105dfb2fe4d61afc0b0525
ms.sourcegitcommit: eb8359b7ab3b0a1a08522faf67b703a00ecdcefd
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/21/2019
ms.locfileid: "58319209"
---
# <a name="testing-with-a-mocking-framework"></a><span data-ttu-id="42042-102">Тестирование с помощью макетирования</span><span class="sxs-lookup"><span data-stu-id="42042-102">Testing with a mocking framework</span></span>
> [!NOTE]
> <span data-ttu-id="42042-103">**Только в EF6 и более поздних версиях**. Функции, API и другие возможности, описанные на этой странице, появились в Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="42042-103">**EF6 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 6.</span></span> <span data-ttu-id="42042-104">При использовании более ранней версии могут быть неприменимы некоторые или все сведения.</span><span class="sxs-lookup"><span data-stu-id="42042-104">If you are using an earlier version, some or all of the information does not apply.</span></span>  

<span data-ttu-id="42042-105">При написании тестов для приложения часто желательно избежать обращения к базе данных.</span><span class="sxs-lookup"><span data-stu-id="42042-105">When writing tests for your application it is often desirable to avoid hitting the database.</span></span>  <span data-ttu-id="42042-106">Платформа Entity Framework позволяет добиться этого путем создания контекста — с поведение, определяемое тестов —, в котором используется данных в памяти.</span><span class="sxs-lookup"><span data-stu-id="42042-106">Entity Framework allows you to achieve this by creating a context – with behavior defined by your tests – that makes use of in-memory data.</span></span>  

## <a name="options-for-creating-test-doubles"></a><span data-ttu-id="42042-107">Параметры для создания тестовых дублеров</span><span class="sxs-lookup"><span data-stu-id="42042-107">Options for creating test doubles</span></span>  

<span data-ttu-id="42042-108">Существует два различных подхода, которые могут использоваться для создания в памяти версии контекста.</span><span class="sxs-lookup"><span data-stu-id="42042-108">There are two different approaches that can be used to create an in-memory version of your context.</span></span>  

- <span data-ttu-id="42042-109">**Создать свои собственные тестовые дублеры** — этот подход подразумевает написание собственной реализации в памяти DbSets и данные контекста.</span><span class="sxs-lookup"><span data-stu-id="42042-109">**Create your own test doubles** – This approach involves writing your own in-memory implementation of your context and DbSets.</span></span> <span data-ttu-id="42042-110">Это обеспечивает большую контроль над тем, как ведут себя классы, но может включать в себя написание и владельцев разумный объем кода.</span><span class="sxs-lookup"><span data-stu-id="42042-110">This gives you a lot of control over how the classes behave but can involve writing and owning a reasonable amount of code.</span></span>  
- <span data-ttu-id="42042-111">**Использовать платформа имитации для создания тестовых дублеров** — использование платформы имитированной (например, Moq) может иметь реализации в памяти наборы, созданные динамически во время выполнения для вас и данные контекста.</span><span class="sxs-lookup"><span data-stu-id="42042-111">**Use a mocking framework to create test doubles** – Using a mocking framework (such as Moq) you can have the in-memory implementations of your context and sets created dynamically at runtime for you.</span></span>  

<span data-ttu-id="42042-112">В этой статье будет работать при использовании платформа имитации.</span><span class="sxs-lookup"><span data-stu-id="42042-112">This article will deal with using a mocking framework.</span></span> <span data-ttu-id="42042-113">Для создания собственных тестовых дублеров см. в разделе [тестирование с помощью вашей собственной тестирования двойной точности](writing-test-doubles.md).</span><span class="sxs-lookup"><span data-stu-id="42042-113">For creating your own test doubles see [Testing with Your Own Test Doubles](writing-test-doubles.md).</span></span>  

<span data-ttu-id="42042-114">Чтобы продемонстрировать использование EF с макетирования мы собираемся использовать Moq.</span><span class="sxs-lookup"><span data-stu-id="42042-114">To demonstrate using EF with a mocking framework we are going to use Moq.</span></span> <span data-ttu-id="42042-115">Самый простой способ получить Moq является установка [Moq пакет из NuGet](http://nuget.org/packages/Moq/).</span><span class="sxs-lookup"><span data-stu-id="42042-115">The easiest way to get Moq is to install the [Moq package from NuGet](http://nuget.org/packages/Moq/).</span></span>  

## <a name="testing-with-pre-ef6-versions"></a><span data-ttu-id="42042-116">Тестирование с помощью EF6 предварительной версии</span><span class="sxs-lookup"><span data-stu-id="42042-116">Testing with pre-EF6 versions</span></span>  

<span data-ttu-id="42042-117">Сценарии, приведенном в этой статье, зависит от некоторых изменений, внесенных в DbSet в EF6.</span><span class="sxs-lookup"><span data-stu-id="42042-117">The scenario shown in this article is dependent on some changes we made to DbSet in EF6.</span></span> <span data-ttu-id="42042-118">Тестирование с помощью EF5 и более ранней версии см. в разделе [тестирования с контекстом имитировать](http://romiller.com/2012/02/14/testing-with-a-fake-dbcontext/).</span><span class="sxs-lookup"><span data-stu-id="42042-118">For testing with EF5 and earlier version see [Testing with a Fake Context](http://romiller.com/2012/02/14/testing-with-a-fake-dbcontext/).</span></span>  

## <a name="limitations-of-ef-in-memory-test-doubles"></a><span data-ttu-id="42042-119">Ограничения EF in-Memory тестовых дублеров</span><span class="sxs-lookup"><span data-stu-id="42042-119">Limitations of EF in-memory test doubles</span></span>  

<span data-ttu-id="42042-120">В памяти тестовых дублеров может быть хорошим средством предоставления уровня покрытия битов приложения, использующие EF модульного теста.</span><span class="sxs-lookup"><span data-stu-id="42042-120">In-memory test doubles can be a good way to provide unit test level coverage of bits of your application that use EF.</span></span> <span data-ttu-id="42042-121">Тем не менее при этом при использовании LINQ to Objects для выполнения запросов к данным в памяти.</span><span class="sxs-lookup"><span data-stu-id="42042-121">However, when doing this you are using LINQ to Objects to execute queries against in-memory data.</span></span> <span data-ttu-id="42042-122">Это может привести другое поведение, чем при использовании поставщика платформы EF LINQ (LINQ to Entities) для преобразования запросов в SQL, которая выполняется в базе данных.</span><span class="sxs-lookup"><span data-stu-id="42042-122">This can result in different behavior than using EF’s LINQ provider (LINQ to Entities) to translate queries into SQL that is run against your database.</span></span>  

<span data-ttu-id="42042-123">Примером такой разницы загрузка связанных данных.</span><span class="sxs-lookup"><span data-stu-id="42042-123">One example of such a difference is loading related data.</span></span> <span data-ttu-id="42042-124">Если создать серию блогов, в которых тематических записях блога, то при использовании данных в памяти связанных сообщений всегда будут загружаться для каждого блога.</span><span class="sxs-lookup"><span data-stu-id="42042-124">If you create a series of Blogs that each have related Posts, then when using in-memory data the related Posts will always be loaded for each Blog.</span></span> <span data-ttu-id="42042-125">Тем не менее при выполнении в базе данных будет загружаться только при использовании метода Include.</span><span class="sxs-lookup"><span data-stu-id="42042-125">However, when running against a database the data will only be loaded if you use the Include method.</span></span>  

<span data-ttu-id="42042-126">По этой причине рекомендуется всегда включать некоторый уровень end-to-end тестирования (в дополнение к модульные тесты), чтобы ваше приложение работает правильно для базы данных.</span><span class="sxs-lookup"><span data-stu-id="42042-126">For this reason, it is recommended to always include some level of end-to-end testing (in addition to your unit tests) to ensure your application works correctly against a database.</span></span>  

## <a name="following-along-with-this-article"></a><span data-ttu-id="42042-127">Следуя указаниям в этой статье</span><span class="sxs-lookup"><span data-stu-id="42042-127">Following along with this article</span></span>  

<span data-ttu-id="42042-128">В этой статье приводятся полные Листинги кода, которые можно скопировать в Visual Studio для выполнения этой процедуры при необходимости.</span><span class="sxs-lookup"><span data-stu-id="42042-128">This article gives complete code listings that you can copy into Visual Studio to follow along if you wish.</span></span> <span data-ttu-id="42042-129">Проще всего создать **проект модульного теста** и требуется целевой объект **.NET Framework 4.5** для выполнения в разделах, которые используют асинхронный.</span><span class="sxs-lookup"><span data-stu-id="42042-129">It's easiest to create a **Unit Test Project** and you will need to target **.NET Framework 4.5** to complete the sections that use async.</span></span>  

## <a name="the-ef-model"></a><span data-ttu-id="42042-130">Модель EF</span><span class="sxs-lookup"><span data-stu-id="42042-130">The EF model</span></span>  

<span data-ttu-id="42042-131">Службы, мы собираемся тестирования использует EF модели BloggingContext и классы блог и Post.</span><span class="sxs-lookup"><span data-stu-id="42042-131">The service we're going to test makes use of an EF model made up of the BloggingContext and the Blog and Post classes.</span></span> <span data-ttu-id="42042-132">Этот код был создан в конструкторе EF или быть модели Code First.</span><span class="sxs-lookup"><span data-stu-id="42042-132">This code may have been generated by the EF Designer or be a Code First model.</span></span>  

``` csharp
using System.Collections.Generic;
using System.Data.Entity;

namespace TestingDemo
{
    public class BloggingContext : DbContext
    {
        public virtual DbSet<Blog> Blogs { get; set; }
        public virtual DbSet<Post> Posts { get; set; }
    }

    public class Blog
    {
        public int BlogId { get; set; }
        public string Name { get; set; }
        public string Url { get; set; }

        public virtual List<Post> Posts { get; set; }
    }

    public class Post
    {
        public int PostId { get; set; }
        public string Title { get; set; }
        public string Content { get; set; }

        public int BlogId { get; set; }
        public virtual Blog Blog { get; set; }
    }
}
```  

### <a name="virtual-dbset-properties-with-ef-designer"></a><span data-ttu-id="42042-133">Виртуальные свойства DbSet, с помощью конструктора EF</span><span class="sxs-lookup"><span data-stu-id="42042-133">Virtual DbSet properties with EF Designer</span></span>  

<span data-ttu-id="42042-134">Обратите внимание на то, что свойства DbSet в контексте помечаются как виртуальные.</span><span class="sxs-lookup"><span data-stu-id="42042-134">Note that the DbSet properties on the context are marked as virtual.</span></span> <span data-ttu-id="42042-135">Таким образом, платформа имитации для наследования от наш контекст и переопределение этих свойств с реализацией макеты.</span><span class="sxs-lookup"><span data-stu-id="42042-135">This will allow the mocking framework to derive from our context and overriding these properties with a mocked implementation.</span></span>  

<span data-ttu-id="42042-136">Если вы используете Code First, то ваши классы можно изменить напрямую.</span><span class="sxs-lookup"><span data-stu-id="42042-136">If you are using Code First then you can edit your classes directly.</span></span> <span data-ttu-id="42042-137">Если вы используете конструктор EF необходимо изменить шаблон T4, который создает контекст.</span><span class="sxs-lookup"><span data-stu-id="42042-137">If you are using the EF Designer then you’ll need to edit the T4 template that generates your context.</span></span> <span data-ttu-id="42042-138">Откройте \<model_name\>. Context.tt файл, который вложен в вы EDMX-файла, найдите следующий фрагмент кода и добавьте в ключевое слово virtual, как показано.</span><span class="sxs-lookup"><span data-stu-id="42042-138">Open up the \<model_name\>.Context.tt file that is nested under you edmx file, find the following fragment of code and add in the virtual keyword as shown.</span></span>  

``` csharp
public string DbSet(EntitySet entitySet)
{
    return string.Format(
        CultureInfo.InvariantCulture,
        "{0} virtual DbSet\<{1}> {2} {{ get; set; }}",
        Accessibility.ForReadOnlyProperty(entitySet),
        _typeMapper.GetTypeName(entitySet.ElementType),
        _code.Escape(entitySet));
}
```  

## <a name="service-to-be-tested"></a><span data-ttu-id="42042-139">Служба проверяемый</span><span class="sxs-lookup"><span data-stu-id="42042-139">Service to be tested</span></span>  

<span data-ttu-id="42042-140">Для демонстрации тестирование с помощью выполняющейся в памяти тестовых дублеров мы хотим написание несколько тестов для BlogService.</span><span class="sxs-lookup"><span data-stu-id="42042-140">To demonstrate testing with in-memory test doubles we are going to be writing a couple of tests for a BlogService.</span></span> <span data-ttu-id="42042-141">Служба, которая позволяет создавать новые веб-дневники (AddBlog) и возвращает все блоги упорядоченные по имени (GetAllBlogs).</span><span class="sxs-lookup"><span data-stu-id="42042-141">The service is capable of creating new blogs (AddBlog) and returning all Blogs ordered by name (GetAllBlogs).</span></span> <span data-ttu-id="42042-142">В дополнение к GetAllBlogs мы также предоставляем метод, который получит асинхронно все блоги, упорядоченные по имени (GetAllBlogsAsync).</span><span class="sxs-lookup"><span data-stu-id="42042-142">In addition to GetAllBlogs, we’ve also provided a method that will asynchronously get all blogs ordered by name (GetAllBlogsAsync).</span></span>  

``` csharp
using System.Collections.Generic;
using System.Data.Entity;
using System.Linq;
using System.Threading.Tasks;

namespace TestingDemo
{
    public class BlogService
    {
        private BloggingContext _context;

        public BlogService(BloggingContext context)
        {
            _context = context;
        }

        public Blog AddBlog(string name, string url)
        {
            var blog = _context.Blogs.Add(new Blog { Name = name, Url = url });
            _context.SaveChanges();

            return blog;
        }

        public List<Blog> GetAllBlogs()
        {
            var query = from b in _context.Blogs
                        orderby b.Name
                        select b;

            return query.ToList();
        }

        public async Task<List<Blog>> GetAllBlogsAsync()
        {
            var query = from b in _context.Blogs
                        orderby b.Name
                        select b;

            return await query.ToListAsync();
        }
    }
}
```  

## <a name="testing-non-query-scenarios"></a><span data-ttu-id="42042-143">Сценарии тестирования не связанным с запросом</span><span class="sxs-lookup"><span data-stu-id="42042-143">Testing non-query scenarios</span></span>  

<span data-ttu-id="42042-144">Это все, что необходимо сделать, чтобы начать тестирование методы без запросов.</span><span class="sxs-lookup"><span data-stu-id="42042-144">That’s all we need to do to start testing non-query methods.</span></span> <span data-ttu-id="42042-145">Следующий тест использует Moq для создания контекста.</span><span class="sxs-lookup"><span data-stu-id="42042-145">The following test uses Moq to create a context.</span></span> <span data-ttu-id="42042-146">Затем он создает коллекцию DbSet\<блог\> и настраивающий возвращается из свойства контекста блоги.</span><span class="sxs-lookup"><span data-stu-id="42042-146">It then creates a DbSet\<Blog\> and wires it up to be returned from the context’s Blogs property.</span></span> <span data-ttu-id="42042-147">Затем контекст используется для создания новых BlogService, который затем используется для создания нового блога — с помощью метода AddBlog.</span><span class="sxs-lookup"><span data-stu-id="42042-147">Next, the context is used to create a new BlogService which is then used to create a new blog – using the AddBlog method.</span></span> <span data-ttu-id="42042-148">Наконец тест проверяет, что служба добавлен новый блог и вызове метода SaveChanges в контексте.</span><span class="sxs-lookup"><span data-stu-id="42042-148">Finally, the test verifies that the service added a new Blog and called SaveChanges on the context.</span></span>  

``` csharp
using Microsoft.VisualStudio.TestTools.UnitTesting;
using Moq;
using System.Data.Entity;

namespace TestingDemo
{
    [TestClass]
    public class NonQueryTests
    {
        [TestMethod]
        public void CreateBlog_saves_a_blog_via_context()
        {
            var mockSet = new Mock<DbSet<Blog>>();

            var mockContext = new Mock<BloggingContext>();
            mockContext.Setup(m => m.Blogs).Returns(mockSet.Object);

            var service = new BlogService(mockContext.Object);
            service.AddBlog("ADO.NET Blog", "http://blogs.msdn.com/adonet");

            mockSet.Verify(m => m.Add(It.IsAny<Blog>()), Times.Once());
            mockContext.Verify(m => m.SaveChanges(), Times.Once());
        }
    }
}
```  

## <a name="testing-query-scenarios"></a><span data-ttu-id="42042-149">Сценарии тестирования запроса</span><span class="sxs-lookup"><span data-stu-id="42042-149">Testing query scenarios</span></span>  

<span data-ttu-id="42042-150">Чтобы иметь возможность выполнять запросы к нашей DbSet тестовый дублер нам нужно настроить реализации IQueryable.</span><span class="sxs-lookup"><span data-stu-id="42042-150">In order to be able to execute queries against our DbSet test double we need to setup an implementation of IQueryable.</span></span> <span data-ttu-id="42042-151">Первым шагом является создание некоторых данных в памяти — мы используем список\<блог\>.</span><span class="sxs-lookup"><span data-stu-id="42042-151">The first step is to create some in-memory data – we’re using a List\<Blog\>.</span></span> <span data-ttu-id="42042-152">Теперь создадим контекст и DBSet\<блог\> затем подключите реализации IQueryable DbSet — они просто делегировании в поставщике LINQ to Objects, работающий со списком\<T\>.</span><span class="sxs-lookup"><span data-stu-id="42042-152">Next, we create a context and DBSet\<Blog\> then wire up the IQueryable implementation for the DbSet – they’re just delegating to the LINQ to Objects provider that works with List\<T\>.</span></span>  

<span data-ttu-id="42042-153">Затем можно создать BlogService, в зависимости от наших тестовых дублеров и убедитесь, что данные, которые мы получаем от GetAllBlogs упорядочен по имени.</span><span class="sxs-lookup"><span data-stu-id="42042-153">We can then create a BlogService based on our test doubles and ensure that the data we get back from GetAllBlogs is ordered by name.</span></span>  

``` csharp
using Microsoft.VisualStudio.TestTools.UnitTesting;
using Moq;
using System.Collections.Generic;
using System.Data.Entity;
using System.Linq;

namespace TestingDemo
{
    [TestClass]
    public class QueryTests
    {
        [TestMethod]
        public void GetAllBlogs_orders_by_name()
        {
            var data = new List<Blog>
            {
                new Blog { Name = "BBB" },
                new Blog { Name = "ZZZ" },
                new Blog { Name = "AAA" },
            }.AsQueryable();

            var mockSet = new Mock<DbSet<Blog>>();
            mockSet.As<IQueryable<Blog>>().Setup(m => m.Provider).Returns(data.Provider);
            mockSet.As<IQueryable<Blog>>().Setup(m => m.Expression).Returns(data.Expression);
            mockSet.As<IQueryable<Blog>>().Setup(m => m.ElementType).Returns(data.ElementType);
            mockSet.As<IQueryable<Blog>>().Setup(m => m.GetEnumerator()).Returns(data.GetEnumerator());

            var mockContext = new Mock<BloggingContext>();
            mockContext.Setup(c => c.Blogs).Returns(mockSet.Object);

            var service = new BlogService(mockContext.Object);
            var blogs = service.GetAllBlogs();

            Assert.AreEqual(3, blogs.Count);
            Assert.AreEqual("AAA", blogs[0].Name);
            Assert.AreEqual("BBB", blogs[1].Name);
            Assert.AreEqual("ZZZ", blogs[2].Name);
        }
    }
}
```  

### <a name="testing-with-async-queries"></a><span data-ttu-id="42042-154">Тестирование с помощью асинхронных запросов</span><span class="sxs-lookup"><span data-stu-id="42042-154">Testing with async queries</span></span>

<span data-ttu-id="42042-155">Entity Framework 6 был представлен набор методов расширения, которые могут использоваться для асинхронного выполнения запроса.</span><span class="sxs-lookup"><span data-stu-id="42042-155">Entity Framework 6 introduced a set of extension methods that can be used to asynchronously execute a query.</span></span> <span data-ttu-id="42042-156">Эти методы примеры ToListAsync, FirstAsync, ForEachAsync и т. д.</span><span class="sxs-lookup"><span data-stu-id="42042-156">Examples of these methods include ToListAsync, FirstAsync, ForEachAsync, etc.</span></span>  

<span data-ttu-id="42042-157">Так как запросы Entity Framework использовать LINQ, методы расширения определяются в IQueryable и IEnumerable.</span><span class="sxs-lookup"><span data-stu-id="42042-157">Because Entity Framework queries make use of LINQ, the extension methods are defined on IQueryable and IEnumerable.</span></span> <span data-ttu-id="42042-158">Тем не менее так как они предназначены только для использования с Entity Framework может появиться следующая ошибка при попытке использовать их в запрос LINQ, который не запрос Entity Framework:</span><span class="sxs-lookup"><span data-stu-id="42042-158">However, because they are only designed to be used with Entity Framework you may receive the following error if you try to use them on a LINQ query that isn’t an Entity Framework query:</span></span>

> <span data-ttu-id="42042-159">Источник IQueryable не реализует IDbAsyncEnumerable{0}.</span><span class="sxs-lookup"><span data-stu-id="42042-159">The source IQueryable doesn't implement IDbAsyncEnumerable{0}.</span></span> <span data-ttu-id="42042-160">Можно использовать только те источники, которые реализуют IDbAsyncEnumerable для асинхронных операций Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="42042-160">Only sources that implement IDbAsyncEnumerable can be used for Entity Framework asynchronous operations.</span></span> <span data-ttu-id="42042-161">Дополнительные сведения см. [ http://go.microsoft.com/fwlink/?LinkId=287068 ](https://go.microsoft.com/fwlink/?LinkId=287068).</span><span class="sxs-lookup"><span data-stu-id="42042-161">For more details see [http://go.microsoft.com/fwlink/?LinkId=287068](https://go.microsoft.com/fwlink/?LinkId=287068).</span></span>  

<span data-ttu-id="42042-162">Пока асинхронные методы поддерживаются только в том случае, при запуске для запроса на EF, можно использовать их выполнения с в памяти тестирования двойной из DbSet в модульном тесте.</span><span class="sxs-lookup"><span data-stu-id="42042-162">Whilst the async methods are only supported when running against an EF query, you may want to use them in your unit test when running against an in-memory test double of a DbSet.</span></span>  

<span data-ttu-id="42042-163">Чтобы использовать асинхронные методы, нам нужно создать DbAsyncQueryProvider в памяти для обработки асинхронного запроса.</span><span class="sxs-lookup"><span data-stu-id="42042-163">In order to use the async methods we need to create an in-memory DbAsyncQueryProvider to process the async query.</span></span> <span data-ttu-id="42042-164">Хотя можно было бы настроить поставщик запросов, с помощью Moq, гораздо проще создать реализацию double теста в коде.</span><span class="sxs-lookup"><span data-stu-id="42042-164">Whilst it would be possible to setup a query provider using Moq, it is much easier to create a test double implementation in code.</span></span> <span data-ttu-id="42042-165">Ниже приведен код для этой реализации:</span><span class="sxs-lookup"><span data-stu-id="42042-165">The code for this implementation is as follows:</span></span>  

``` csharp
using System.Collections.Generic;
using System.Data.Entity.Infrastructure;
using System.Linq;
using System.Linq.Expressions;
using System.Threading;
using System.Threading.Tasks;

namespace TestingDemo
{
    internal class TestDbAsyncQueryProvider<TEntity> : IDbAsyncQueryProvider
    {
        private readonly IQueryProvider _inner;

        internal TestDbAsyncQueryProvider(IQueryProvider inner)
        {
            _inner = inner;
        }

        public IQueryable CreateQuery(Expression expression)
        {
            return new TestDbAsyncEnumerable<TEntity>(expression);
        }

        public IQueryable<TElement> CreateQuery<TElement>(Expression expression)
        {
            return new TestDbAsyncEnumerable<TElement>(expression);
        }

        public object Execute(Expression expression)
        {
            return _inner.Execute(expression);
        }

        public TResult Execute<TResult>(Expression expression)
        {
            return _inner.Execute<TResult>(expression);
        }

        public Task<object> ExecuteAsync(Expression expression, CancellationToken cancellationToken)
        {
            return Task.FromResult(Execute(expression));
        }

        public Task<TResult> ExecuteAsync<TResult>(Expression expression, CancellationToken cancellationToken)
        {
            return Task.FromResult(Execute<TResult>(expression));
        }
    }

    internal class TestDbAsyncEnumerable<T> : EnumerableQuery<T>, IDbAsyncEnumerable<T>, IQueryable<T>
    {
        public TestDbAsyncEnumerable(IEnumerable<T> enumerable)
            : base(enumerable)
        { }

        public TestDbAsyncEnumerable(Expression expression)
            : base(expression)
        { }

        public IDbAsyncEnumerator<T> GetAsyncEnumerator()
        {
            return new TestDbAsyncEnumerator<T>(this.AsEnumerable().GetEnumerator());
        }

        IDbAsyncEnumerator IDbAsyncEnumerable.GetAsyncEnumerator()
        {
            return GetAsyncEnumerator();
        }

        IQueryProvider IQueryable.Provider
        {
            get { return new TestDbAsyncQueryProvider<T>(this); }
        }
    }

    internal class TestDbAsyncEnumerator<T> : IDbAsyncEnumerator<T>
    {
        private readonly IEnumerator<T> _inner;

        public TestDbAsyncEnumerator(IEnumerator<T> inner)
        {
            _inner = inner;
        }

        public void Dispose()
        {
            _inner.Dispose();
        }

        public Task<bool> MoveNextAsync(CancellationToken cancellationToken)
        {
            return Task.FromResult(_inner.MoveNext());
        }

        public T Current
        {
            get { return _inner.Current; }
        }

        object IDbAsyncEnumerator.Current
        {
            get { return Current; }
        }
    }
}
```  

<span data-ttu-id="42042-166">Теперь, когда у нас есть поставщик асинхронный запрос можно написать модульный тест для нашего нового метода GetAllBlogsAsync.</span><span class="sxs-lookup"><span data-stu-id="42042-166">Now that we have an async query provider we can write a unit test for our new GetAllBlogsAsync method.</span></span>  

``` csharp
using Microsoft.VisualStudio.TestTools.UnitTesting;
using Moq;
using System.Collections.Generic;
using System.Data.Entity;
using System.Data.Entity.Infrastructure;
using System.Linq;
using System.Threading.Tasks;

namespace TestingDemo
{
    [TestClass]
    public class AsyncQueryTests
    {
        [TestMethod]
        public async Task GetAllBlogsAsync_orders_by_name()
        {

            var data = new List<Blog>
            {
                new Blog { Name = "BBB" },
                new Blog { Name = "ZZZ" },
                new Blog { Name = "AAA" },
            }.AsQueryable();

            var mockSet = new Mock<DbSet<Blog>>();
            mockSet.As<IDbAsyncEnumerable<Blog>>()
                .Setup(m => m.GetAsyncEnumerator())
                .Returns(new TestDbAsyncEnumerator<Blog>(data.GetEnumerator()));

            mockSet.As<IQueryable<Blog>>()
                .Setup(m => m.Provider)
                .Returns(new TestDbAsyncQueryProvider<Blog>(data.Provider));

            mockSet.As<IQueryable<Blog>>().Setup(m => m.Expression).Returns(data.Expression);
            mockSet.As<IQueryable<Blog>>().Setup(m => m.ElementType).Returns(data.ElementType);
            mockSet.As<IQueryable<Blog>>().Setup(m => m.GetEnumerator()).Returns(data.GetEnumerator());

            var mockContext = new Mock<BloggingContext>();
            mockContext.Setup(c => c.Blogs).Returns(mockSet.Object);

            var service = new BlogService(mockContext.Object);
            var blogs = await service.GetAllBlogsAsync();

            Assert.AreEqual(3, blogs.Count);
            Assert.AreEqual("AAA", blogs[0].Name);
            Assert.AreEqual("BBB", blogs[1].Name);
            Assert.AreEqual("ZZZ", blogs[2].Name);
        }
    }
}
```  
