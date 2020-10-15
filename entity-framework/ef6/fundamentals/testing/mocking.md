---
title: Тестирование с помощью инфраструктуры макетирования — EF6
description: Тестирование с помощью инфраструктуры макетирования в Entity Framework 6
author: ajcvickers
ms.date: 10/23/2016
uid: ef6/fundamentals/testing/mocking
ms.openlocfilehash: 893d9f921adc148465830dee5dbfebca4d4c8f50
ms.sourcegitcommit: 0a25c03fa65ae6e0e0e3f66bac48d59eceb96a5a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/14/2020
ms.locfileid: "92062923"
---
# <a name="testing-with-a-mocking-framework"></a><span data-ttu-id="f1b0d-103">Тестирование с помощью инфраструктуры макетирования</span><span class="sxs-lookup"><span data-stu-id="f1b0d-103">Testing with a mocking framework</span></span>
> [!NOTE]
> <span data-ttu-id="f1b0d-104">**Только в EF6 и более поздних версиях**. Функции, API и другие возможности, описанные на этой странице, появились в Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="f1b0d-104">**EF6 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 6.</span></span> <span data-ttu-id="f1b0d-105">При использовании более ранней версии могут быть неприменимы некоторые или все сведения.</span><span class="sxs-lookup"><span data-stu-id="f1b0d-105">If you are using an earlier version, some or all of the information does not apply.</span></span>  

<span data-ttu-id="f1b0d-106">При написании тестов для приложения часто желательно избежать попадания в базу данных.</span><span class="sxs-lookup"><span data-stu-id="f1b0d-106">When writing tests for your application it is often desirable to avoid hitting the database.</span></span>  <span data-ttu-id="f1b0d-107">Entity Framework позволяет добиться этого, создавая контекст с последующим поведением, определенным в тестах. Это позволяет использовать данные в памяти.</span><span class="sxs-lookup"><span data-stu-id="f1b0d-107">Entity Framework allows you to achieve this by creating a context – with behavior defined by your tests – that makes use of in-memory data.</span></span>  

## <a name="options-for-creating-test-doubles"></a><span data-ttu-id="f1b0d-108">Параметры для создания тестовых значений типа Double</span><span class="sxs-lookup"><span data-stu-id="f1b0d-108">Options for creating test doubles</span></span>  

<span data-ttu-id="f1b0d-109">Существует два разных подхода, которые можно использовать для создания версии контекста в памяти.</span><span class="sxs-lookup"><span data-stu-id="f1b0d-109">There are two different approaches that can be used to create an in-memory version of your context.</span></span>  

- <span data-ttu-id="f1b0d-110">**Создание собственных тестовых двойных** значений. Этот подход включает написание собственной реализации контекста и дбсетс в памяти.</span><span class="sxs-lookup"><span data-stu-id="f1b0d-110">**Create your own test doubles** – This approach involves writing your own in-memory implementation of your context and DbSets.</span></span> <span data-ttu-id="f1b0d-111">Это дает множество контроля над поведением классов, но может задействовать написание и владельца разумного объема кода.</span><span class="sxs-lookup"><span data-stu-id="f1b0d-111">This gives you a lot of control over how the classes behave but can involve writing and owning a reasonable amount of code.</span></span>  
- <span data-ttu-id="f1b0d-112">**Использование инфраструктуры макетирования для создания тестовых значений Double** . с помощью инфраструктуры макетирования (например, MOQ) можно реализовать в памяти реализации контекста и динамически создаваемые во время выполнения.</span><span class="sxs-lookup"><span data-stu-id="f1b0d-112">**Use a mocking framework to create test doubles** – Using a mocking framework (such as Moq) you can have the in-memory implementations of your context and sets created dynamically at runtime for you.</span></span>  

<span data-ttu-id="f1b0d-113">В этой статье рассматривается использование инфраструктуры макетирования.</span><span class="sxs-lookup"><span data-stu-id="f1b0d-113">This article will deal with using a mocking framework.</span></span> <span data-ttu-id="f1b0d-114">Для создания собственных тестовых двойной точности см. раздел [тестирование с помощью пользовательских значений Double](xref:ef6/fundamentals/testing/writing-test-doubles).</span><span class="sxs-lookup"><span data-stu-id="f1b0d-114">For creating your own test doubles see [Testing with Your Own Test Doubles](xref:ef6/fundamentals/testing/writing-test-doubles).</span></span>  

<span data-ttu-id="f1b0d-115">Чтобы продемонстрировать использование EF с инфраструктурой макетирования, мы будем использовать MOQ.</span><span class="sxs-lookup"><span data-stu-id="f1b0d-115">To demonstrate using EF with a mocking framework we are going to use Moq.</span></span> <span data-ttu-id="f1b0d-116">Самый простой способ получить MOQ — установить [пакет MOQ из NuGet](https://nuget.org/packages/Moq/).</span><span class="sxs-lookup"><span data-stu-id="f1b0d-116">The easiest way to get Moq is to install the [Moq package from NuGet](https://nuget.org/packages/Moq/).</span></span>  

## <a name="testing-with-pre-ef6-versions"></a><span data-ttu-id="f1b0d-117">Тестирование с использованием предварительно EF6 версий</span><span class="sxs-lookup"><span data-stu-id="f1b0d-117">Testing with pre-EF6 versions</span></span>  

<span data-ttu-id="f1b0d-118">Сценарий, приведенный в этой статье, зависит от некоторых изменений, внесенных в DbSet в EF6.</span><span class="sxs-lookup"><span data-stu-id="f1b0d-118">The scenario shown in this article is dependent on some changes we made to DbSet in EF6.</span></span> <span data-ttu-id="f1b0d-119">Тестирование с помощью EF5 и более ранних версий см. в разделе [тестирование с имитацией контекста](https://romiller.com/2012/02/14/testing-with-a-fake-dbcontext/).</span><span class="sxs-lookup"><span data-stu-id="f1b0d-119">For testing with EF5 and earlier version see [Testing with a Fake Context](https://romiller.com/2012/02/14/testing-with-a-fake-dbcontext/).</span></span>  

## <a name="limitations-of-ef-in-memory-test-doubles"></a><span data-ttu-id="f1b0d-120">Ограничения для теста EF в памяти, Double</span><span class="sxs-lookup"><span data-stu-id="f1b0d-120">Limitations of EF in-memory test doubles</span></span>  

<span data-ttu-id="f1b0d-121">Числа двойной проверки в памяти могут быть хорошим способом обеспечения уровня модульного тестирования для битов приложения, использующих EF.</span><span class="sxs-lookup"><span data-stu-id="f1b0d-121">In-memory test doubles can be a good way to provide unit test level coverage of bits of your application that use EF.</span></span> <span data-ttu-id="f1b0d-122">Однако при этом вы используете LINQ to Objects для выполнения запросов к данным в памяти.</span><span class="sxs-lookup"><span data-stu-id="f1b0d-122">However, when doing this you are using LINQ to Objects to execute queries against in-memory data.</span></span> <span data-ttu-id="f1b0d-123">Это может привести к различному поведению, чем использование поставщика LINQ (LINQ to Entities) EF для преобразования запросов в SQL, выполняемых в базе данных.</span><span class="sxs-lookup"><span data-stu-id="f1b0d-123">This can result in different behavior than using EF’s LINQ provider (LINQ to Entities) to translate queries into SQL that is run against your database.</span></span>  

<span data-ttu-id="f1b0d-124">Одним из примеров такого различия является загрузка связанных данных.</span><span class="sxs-lookup"><span data-stu-id="f1b0d-124">One example of such a difference is loading related data.</span></span> <span data-ttu-id="f1b0d-125">Если вы создадите серию блогов, в каждой из которых есть связанные записи, то при использовании данных в памяти связанные записи всегда будут загружаться для каждого блога.</span><span class="sxs-lookup"><span data-stu-id="f1b0d-125">If you create a series of Blogs that each have related Posts, then when using in-memory data the related Posts will always be loaded for each Blog.</span></span> <span data-ttu-id="f1b0d-126">Однако при работе с базой данных данные будут загружаться только при использовании метода Include.</span><span class="sxs-lookup"><span data-stu-id="f1b0d-126">However, when running against a database the data will only be loaded if you use the Include method.</span></span>  

<span data-ttu-id="f1b0d-127">По этой причине рекомендуется всегда включать некоторый уровень сквозного тестирования (в дополнение к модульным тестам), чтобы обеспечить правильную работу приложения в базе данных.</span><span class="sxs-lookup"><span data-stu-id="f1b0d-127">For this reason, it is recommended to always include some level of end-to-end testing (in addition to your unit tests) to ensure your application works correctly against a database.</span></span>  

## <a name="following-along-with-this-article"></a><span data-ttu-id="f1b0d-128">Далее вместе с этой статьей</span><span class="sxs-lookup"><span data-stu-id="f1b0d-128">Following along with this article</span></span>  

<span data-ttu-id="f1b0d-129">В этой статье приведены полные листинги кода, которые можно скопировать в Visual Studio, если хотите.</span><span class="sxs-lookup"><span data-stu-id="f1b0d-129">This article gives complete code listings that you can copy into Visual Studio to follow along if you wish.</span></span> <span data-ttu-id="f1b0d-130">Проще всего создать **проект модульного теста** , а для выполнения разделов, использующих async, необходимо выбрать **.NET Framework 4,5** .</span><span class="sxs-lookup"><span data-stu-id="f1b0d-130">It's easiest to create a **Unit Test Project** and you will need to target **.NET Framework 4.5** to complete the sections that use async.</span></span>  

## <a name="the-ef-model"></a><span data-ttu-id="f1b0d-131">Модель EF</span><span class="sxs-lookup"><span data-stu-id="f1b0d-131">The EF model</span></span>  

<span data-ttu-id="f1b0d-132">Служба, которую мы собираемся тестировать, использует модель EF, состоящие из BloggingContext, а также классов блога и POST.</span><span class="sxs-lookup"><span data-stu-id="f1b0d-132">The service we're going to test makes use of an EF model made up of the BloggingContext and the Blog and Post classes.</span></span> <span data-ttu-id="f1b0d-133">Этот код может быть создан конструктором EF или моделью Code First.</span><span class="sxs-lookup"><span data-stu-id="f1b0d-133">This code may have been generated by the EF Designer or be a Code First model.</span></span>  

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

### <a name="virtual-dbset-properties-with-ef-designer"></a><span data-ttu-id="f1b0d-134">Свойства виртуальных DbSet с помощью конструктора EF</span><span class="sxs-lookup"><span data-stu-id="f1b0d-134">Virtual DbSet properties with EF Designer</span></span>  

<span data-ttu-id="f1b0d-135">Обратите внимание, что свойства DbSet в контексте помечаются как виртуальные.</span><span class="sxs-lookup"><span data-stu-id="f1b0d-135">Note that the DbSet properties on the context are marked as virtual.</span></span> <span data-ttu-id="f1b0d-136">Это позволит инфраструктуре макетирования получить производный от нашего контекста и переопределить эти свойства с помощью макетной реализации.</span><span class="sxs-lookup"><span data-stu-id="f1b0d-136">This will allow the mocking framework to derive from our context and overriding these properties with a mocked implementation.</span></span>  

<span data-ttu-id="f1b0d-137">При использовании Code First можно изменить классы напрямую.</span><span class="sxs-lookup"><span data-stu-id="f1b0d-137">If you are using Code First then you can edit your classes directly.</span></span> <span data-ttu-id="f1b0d-138">При использовании конструктора EF необходимо изменить шаблон T4, который создает контекст.</span><span class="sxs-lookup"><span data-stu-id="f1b0d-138">If you are using the EF Designer then you’ll need to edit the T4 template that generates your context.</span></span> <span data-ttu-id="f1b0d-139">Откройте \<model_name\> . Context.tt файл, вложенный в EDMX-файл, найдите следующий фрагмент кода и добавьте ключевое слово virtual, как показано ниже.</span><span class="sxs-lookup"><span data-stu-id="f1b0d-139">Open up the \<model_name\>.Context.tt file that is nested under you edmx file, find the following fragment of code and add in the virtual keyword as shown.</span></span>  

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

## <a name="service-to-be-tested"></a><span data-ttu-id="f1b0d-140">Тестируемая служба</span><span class="sxs-lookup"><span data-stu-id="f1b0d-140">Service to be tested</span></span>  

<span data-ttu-id="f1b0d-141">Чтобы продемонстрировать тестирование с двойными тестами в памяти, мы будем писать несколько тестов для BlogService.</span><span class="sxs-lookup"><span data-stu-id="f1b0d-141">To demonstrate testing with in-memory test doubles we are going to be writing a couple of tests for a BlogService.</span></span> <span data-ttu-id="f1b0d-142">Служба может создавать новые блоги (Аддблог) и возвращать все блоги, упорядоченные по имени (Жеталлблогс).</span><span class="sxs-lookup"><span data-stu-id="f1b0d-142">The service is capable of creating new blogs (AddBlog) and returning all Blogs ordered by name (GetAllBlogs).</span></span> <span data-ttu-id="f1b0d-143">Кроме Жеталлблогс, мы также предоставили метод, который будет асинхронно получать все блоги, упорядоченные по имени (Жеталлблогсасинк).</span><span class="sxs-lookup"><span data-stu-id="f1b0d-143">In addition to GetAllBlogs, we’ve also provided a method that will asynchronously get all blogs ordered by name (GetAllBlogsAsync).</span></span>  

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

## <a name="testing-non-query-scenarios"></a><span data-ttu-id="f1b0d-144">Тестирование сценариев, не использующих запросы</span><span class="sxs-lookup"><span data-stu-id="f1b0d-144">Testing non-query scenarios</span></span>  

<span data-ttu-id="f1b0d-145">Это все, что нужно сделать, чтобы начать тестирование методов, не относящихся к запросам.</span><span class="sxs-lookup"><span data-stu-id="f1b0d-145">That’s all we need to do to start testing non-query methods.</span></span> <span data-ttu-id="f1b0d-146">В следующем тесте для создания контекста используется MOQ.</span><span class="sxs-lookup"><span data-stu-id="f1b0d-146">The following test uses Moq to create a context.</span></span> <span data-ttu-id="f1b0d-147">Затем он создает DbSet \<Blog\> и подсоединяет его, чтобы он возвращался из свойства blogs в контексте.</span><span class="sxs-lookup"><span data-stu-id="f1b0d-147">It then creates a DbSet\<Blog\> and wires it up to be returned from the context’s Blogs property.</span></span> <span data-ttu-id="f1b0d-148">Далее контекст используется для создания нового BlogService, который затем используется для создания нового блога — с помощью метода Аддблог.</span><span class="sxs-lookup"><span data-stu-id="f1b0d-148">Next, the context is used to create a new BlogService which is then used to create a new blog – using the AddBlog method.</span></span> <span data-ttu-id="f1b0d-149">Наконец, тест подтверждает, что служба добавила новый блог и вызвала команду SaveChanges в контексте.</span><span class="sxs-lookup"><span data-stu-id="f1b0d-149">Finally, the test verifies that the service added a new Blog and called SaveChanges on the context.</span></span>  

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

## <a name="testing-query-scenarios"></a><span data-ttu-id="f1b0d-150">Тестирование сценариев запросов</span><span class="sxs-lookup"><span data-stu-id="f1b0d-150">Testing query scenarios</span></span>  

<span data-ttu-id="f1b0d-151">Чтобы иметь возможность выполнять запросы к DbSet тесту Double, необходимо настроить реализацию IQueryable.</span><span class="sxs-lookup"><span data-stu-id="f1b0d-151">In order to be able to execute queries against our DbSet test double we need to setup an implementation of IQueryable.</span></span> <span data-ttu-id="f1b0d-152">Первым шагом является создание некоторых данных в памяти — мы используем список \<Blog\> .</span><span class="sxs-lookup"><span data-stu-id="f1b0d-152">The first step is to create some in-memory data – we’re using a List\<Blog\>.</span></span> <span data-ttu-id="f1b0d-153">Далее мы создаем контекст, а \<Blog\> затем DBSet подсоединить реализацию IQueryable для DBSet — они просто делегируются поставщику LINQ to Objects, который работает со списком \<T\> .</span><span class="sxs-lookup"><span data-stu-id="f1b0d-153">Next, we create a context and DBSet\<Blog\> then wire up the IQueryable implementation for the DbSet – they’re just delegating to the LINQ to Objects provider that works with List\<T\>.</span></span>  

<span data-ttu-id="f1b0d-154">Затем можно создать BlogService на основе наших тестовых значений типа Double и убедиться, что данные, получаемые из Жеталлблогс, упорядочены по именам.</span><span class="sxs-lookup"><span data-stu-id="f1b0d-154">We can then create a BlogService based on our test doubles and ensure that the data we get back from GetAllBlogs is ordered by name.</span></span>  

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

### <a name="testing-with-async-queries"></a><span data-ttu-id="f1b0d-155">Тестирование с помощью асинхронных запросов</span><span class="sxs-lookup"><span data-stu-id="f1b0d-155">Testing with async queries</span></span>

<span data-ttu-id="f1b0d-156">В Entity Framework 6 появился набор методов расширения, которые можно использовать для асинхронного выполнения запроса.</span><span class="sxs-lookup"><span data-stu-id="f1b0d-156">Entity Framework 6 introduced a set of extension methods that can be used to asynchronously execute a query.</span></span> <span data-ttu-id="f1b0d-157">Примеры этих методов: Толистасинк, Фирстасинк, Фореачасинк и т. д.</span><span class="sxs-lookup"><span data-stu-id="f1b0d-157">Examples of these methods include ToListAsync, FirstAsync, ForEachAsync, etc.</span></span>  

<span data-ttu-id="f1b0d-158">Поскольку запросы Entity Framework используют LINQ, методы расширения определяются в IQueryable и IEnumerable.</span><span class="sxs-lookup"><span data-stu-id="f1b0d-158">Because Entity Framework queries make use of LINQ, the extension methods are defined on IQueryable and IEnumerable.</span></span> <span data-ttu-id="f1b0d-159">Однако, поскольку они предназначены только для использования с Entity Framework может возникнуть следующая ошибка при попытке использовать их в запросе LINQ, который не является Entity Frameworkным запросом:</span><span class="sxs-lookup"><span data-stu-id="f1b0d-159">However, because they are only designed to be used with Entity Framework you may receive the following error if you try to use them on a LINQ query that isn’t an Entity Framework query:</span></span>

> <span data-ttu-id="f1b0d-160">Источник IQueryable не реализует Идбасинценумерабле {0} .</span><span class="sxs-lookup"><span data-stu-id="f1b0d-160">The source IQueryable doesn't implement IDbAsyncEnumerable{0}.</span></span> <span data-ttu-id="f1b0d-161">Для Entity Framework асинхронных операций можно использовать только те источники, которые реализуют Идбасинценумерабле.</span><span class="sxs-lookup"><span data-stu-id="f1b0d-161">Only sources that implement IDbAsyncEnumerable can be used for Entity Framework asynchronous operations.</span></span> <span data-ttu-id="f1b0d-162">Дополнительные сведения см. в разделе [http://go.microsoft.com/fwlink/?LinkId=287068](https://go.microsoft.com/fwlink/?LinkId=287068) .</span><span class="sxs-lookup"><span data-stu-id="f1b0d-162">For more details see [http://go.microsoft.com/fwlink/?LinkId=287068](https://go.microsoft.com/fwlink/?LinkId=287068).</span></span>  

<span data-ttu-id="f1b0d-163">В то время как асинхронные методы поддерживаются только при выполнении запроса EF, их можно использовать в модульном тесте при выполнении теста в памяти с двойной точностью DbSet.</span><span class="sxs-lookup"><span data-stu-id="f1b0d-163">Whilst the async methods are only supported when running against an EF query, you may want to use them in your unit test when running against an in-memory test double of a DbSet.</span></span>  

<span data-ttu-id="f1b0d-164">Чтобы использовать асинхронные методы, необходимо создать Дбасинккуерипровидер в памяти для обработки асинхронного запроса.</span><span class="sxs-lookup"><span data-stu-id="f1b0d-164">In order to use the async methods we need to create an in-memory DbAsyncQueryProvider to process the async query.</span></span> <span data-ttu-id="f1b0d-165">В то же самое, что можно было бы настроить поставщик запросов с помощью MOQ, гораздо проще создать тестовую реализацию двойной реализации в коде.</span><span class="sxs-lookup"><span data-stu-id="f1b0d-165">Whilst it would be possible to setup a query provider using Moq, it is much easier to create a test double implementation in code.</span></span> <span data-ttu-id="f1b0d-166">Код для этой реализации выглядит следующим образом:</span><span class="sxs-lookup"><span data-stu-id="f1b0d-166">The code for this implementation is as follows:</span></span>  

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

<span data-ttu-id="f1b0d-167">Теперь, когда у нас есть поставщик асинхронных запросов, можно написать модульный тест для нашего нового метода Жеталлблогсасинк.</span><span class="sxs-lookup"><span data-stu-id="f1b0d-167">Now that we have an async query provider we can write a unit test for our new GetAllBlogsAsync method.</span></span>  

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
