---
title: Тестирование с помощью инфраструктуры макетирования — EF6
author: divega
ms.date: 10/23/2016
ms.assetid: bd66a638-d245-44d4-8e71-b9c6cb335cc7
ms.openlocfilehash: 790e077c5b30c4a68a96b3c1a99b40893b2bbe55
ms.sourcegitcommit: 708b18520321c587b2046ad2ea9fa7c48aeebfe5
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/09/2019
ms.locfileid: "72181569"
---
# <a name="testing-with-a-mocking-framework"></a><span data-ttu-id="b6e42-102">Тестирование с помощью инфраструктуры макетирования</span><span class="sxs-lookup"><span data-stu-id="b6e42-102">Testing with a mocking framework</span></span>
> [!NOTE]
> <span data-ttu-id="b6e42-103">**Только в EF6 и более поздних версиях**. Функции, API и другие возможности, описанные на этой странице, появились в Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="b6e42-103">**EF6 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 6.</span></span> <span data-ttu-id="b6e42-104">При использовании более ранней версии могут быть неприменимы некоторые или все сведения.</span><span class="sxs-lookup"><span data-stu-id="b6e42-104">If you are using an earlier version, some or all of the information does not apply.</span></span>  

<span data-ttu-id="b6e42-105">При написании тестов для приложения часто желательно избежать попадания в базу данных.</span><span class="sxs-lookup"><span data-stu-id="b6e42-105">When writing tests for your application it is often desirable to avoid hitting the database.</span></span>  <span data-ttu-id="b6e42-106">Entity Framework позволяет добиться этого, создавая контекст с последующим поведением, определенным в тестах. Это позволяет использовать данные в памяти.</span><span class="sxs-lookup"><span data-stu-id="b6e42-106">Entity Framework allows you to achieve this by creating a context – with behavior defined by your tests – that makes use of in-memory data.</span></span>  

## <a name="options-for-creating-test-doubles"></a><span data-ttu-id="b6e42-107">Параметры для создания тестовых значений типа Double</span><span class="sxs-lookup"><span data-stu-id="b6e42-107">Options for creating test doubles</span></span>  

<span data-ttu-id="b6e42-108">Существует два разных подхода, которые можно использовать для создания версии контекста в памяти.</span><span class="sxs-lookup"><span data-stu-id="b6e42-108">There are two different approaches that can be used to create an in-memory version of your context.</span></span>  

- <span data-ttu-id="b6e42-109">**Создание собственных тестовых двойных** значений. Этот подход включает написание собственной реализации контекста и дбсетс в памяти.</span><span class="sxs-lookup"><span data-stu-id="b6e42-109">**Create your own test doubles** – This approach involves writing your own in-memory implementation of your context and DbSets.</span></span> <span data-ttu-id="b6e42-110">Это дает множество контроля над поведением классов, но может задействовать написание и владельца разумного объема кода.</span><span class="sxs-lookup"><span data-stu-id="b6e42-110">This gives you a lot of control over how the classes behave but can involve writing and owning a reasonable amount of code.</span></span>  
- <span data-ttu-id="b6e42-111">**Использование инфраструктуры макетирования для создания тестовых значений Double** . с помощью инфраструктуры макетирования (например, MOQ) можно реализовать в памяти реализации контекста и динамически создаваемые во время выполнения.</span><span class="sxs-lookup"><span data-stu-id="b6e42-111">**Use a mocking framework to create test doubles** – Using a mocking framework (such as Moq) you can have the in-memory implementations of your context and sets created dynamically at runtime for you.</span></span>  

<span data-ttu-id="b6e42-112">В этой статье рассматривается использование инфраструктуры макетирования.</span><span class="sxs-lookup"><span data-stu-id="b6e42-112">This article will deal with using a mocking framework.</span></span> <span data-ttu-id="b6e42-113">Для создания собственных тестовых двойной точности см. раздел [тестирование с помощью пользовательских значений Double](writing-test-doubles.md).</span><span class="sxs-lookup"><span data-stu-id="b6e42-113">For creating your own test doubles see [Testing with Your Own Test Doubles](writing-test-doubles.md).</span></span>  

<span data-ttu-id="b6e42-114">Чтобы продемонстрировать использование EF с инфраструктурой макетирования, мы будем использовать MOQ.</span><span class="sxs-lookup"><span data-stu-id="b6e42-114">To demonstrate using EF with a mocking framework we are going to use Moq.</span></span> <span data-ttu-id="b6e42-115">Самый простой способ получить MOQ — установить [пакет MOQ из NuGet](https://nuget.org/packages/Moq/).</span><span class="sxs-lookup"><span data-stu-id="b6e42-115">The easiest way to get Moq is to install the [Moq package from NuGet](https://nuget.org/packages/Moq/).</span></span>  

## <a name="testing-with-pre-ef6-versions"></a><span data-ttu-id="b6e42-116">Тестирование с использованием предварительно EF6 версий</span><span class="sxs-lookup"><span data-stu-id="b6e42-116">Testing with pre-EF6 versions</span></span>  

<span data-ttu-id="b6e42-117">Сценарий, приведенный в этой статье, зависит от некоторых изменений, внесенных в DbSet в EF6.</span><span class="sxs-lookup"><span data-stu-id="b6e42-117">The scenario shown in this article is dependent on some changes we made to DbSet in EF6.</span></span> <span data-ttu-id="b6e42-118">Тестирование с помощью EF5 и более ранних версий см. в разделе [тестирование с имитацией контекста](https://romiller.com/2012/02/14/testing-with-a-fake-dbcontext/).</span><span class="sxs-lookup"><span data-stu-id="b6e42-118">For testing with EF5 and earlier version see [Testing with a Fake Context](https://romiller.com/2012/02/14/testing-with-a-fake-dbcontext/).</span></span>  

## <a name="limitations-of-ef-in-memory-test-doubles"></a><span data-ttu-id="b6e42-119">Ограничения для теста EF в памяти, Double</span><span class="sxs-lookup"><span data-stu-id="b6e42-119">Limitations of EF in-memory test doubles</span></span>  

<span data-ttu-id="b6e42-120">Числа двойной проверки в памяти могут быть хорошим способом обеспечения уровня модульного тестирования для битов приложения, использующих EF.</span><span class="sxs-lookup"><span data-stu-id="b6e42-120">In-memory test doubles can be a good way to provide unit test level coverage of bits of your application that use EF.</span></span> <span data-ttu-id="b6e42-121">Однако при этом вы используете LINQ to Objects для выполнения запросов к данным в памяти.</span><span class="sxs-lookup"><span data-stu-id="b6e42-121">However, when doing this you are using LINQ to Objects to execute queries against in-memory data.</span></span> <span data-ttu-id="b6e42-122">Это может привести к различному поведению, чем использование поставщика LINQ (LINQ to Entities) EF для преобразования запросов в SQL, выполняемых в базе данных.</span><span class="sxs-lookup"><span data-stu-id="b6e42-122">This can result in different behavior than using EF’s LINQ provider (LINQ to Entities) to translate queries into SQL that is run against your database.</span></span>  

<span data-ttu-id="b6e42-123">Одним из примеров такого различия является загрузка связанных данных.</span><span class="sxs-lookup"><span data-stu-id="b6e42-123">One example of such a difference is loading related data.</span></span> <span data-ttu-id="b6e42-124">Если вы создадите серию блогов, в каждой из которых есть связанные записи, то при использовании данных в памяти связанные записи всегда будут загружаться для каждого блога.</span><span class="sxs-lookup"><span data-stu-id="b6e42-124">If you create a series of Blogs that each have related Posts, then when using in-memory data the related Posts will always be loaded for each Blog.</span></span> <span data-ttu-id="b6e42-125">Однако при работе с базой данных данные будут загружаться только при использовании метода Include.</span><span class="sxs-lookup"><span data-stu-id="b6e42-125">However, when running against a database the data will only be loaded if you use the Include method.</span></span>  

<span data-ttu-id="b6e42-126">По этой причине рекомендуется всегда включать некоторый уровень сквозного тестирования (в дополнение к модульным тестам), чтобы обеспечить правильную работу приложения в базе данных.</span><span class="sxs-lookup"><span data-stu-id="b6e42-126">For this reason, it is recommended to always include some level of end-to-end testing (in addition to your unit tests) to ensure your application works correctly against a database.</span></span>  

## <a name="following-along-with-this-article"></a><span data-ttu-id="b6e42-127">Далее вместе с этой статьей</span><span class="sxs-lookup"><span data-stu-id="b6e42-127">Following along with this article</span></span>  

<span data-ttu-id="b6e42-128">В этой статье приведены полные листинги кода, которые можно скопировать в Visual Studio, если хотите.</span><span class="sxs-lookup"><span data-stu-id="b6e42-128">This article gives complete code listings that you can copy into Visual Studio to follow along if you wish.</span></span> <span data-ttu-id="b6e42-129">Проще всего создать **проект модульного теста** , а для выполнения разделов, использующих async, необходимо выбрать **.NET Framework 4,5** .</span><span class="sxs-lookup"><span data-stu-id="b6e42-129">It's easiest to create a **Unit Test Project** and you will need to target **.NET Framework 4.5** to complete the sections that use async.</span></span>  

## <a name="the-ef-model"></a><span data-ttu-id="b6e42-130">Модель EF</span><span class="sxs-lookup"><span data-stu-id="b6e42-130">The EF model</span></span>  

<span data-ttu-id="b6e42-131">Служба, которую мы собираемся тестировать, использует модель EF, состоящие из BloggingContext, а также классов блога и POST.</span><span class="sxs-lookup"><span data-stu-id="b6e42-131">The service we're going to test makes use of an EF model made up of the BloggingContext and the Blog and Post classes.</span></span> <span data-ttu-id="b6e42-132">Этот код может быть создан конструктором EF или моделью Code First.</span><span class="sxs-lookup"><span data-stu-id="b6e42-132">This code may have been generated by the EF Designer or be a Code First model.</span></span>  

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

### <a name="virtual-dbset-properties-with-ef-designer"></a><span data-ttu-id="b6e42-133">Свойства виртуальных DbSet с помощью конструктора EF</span><span class="sxs-lookup"><span data-stu-id="b6e42-133">Virtual DbSet properties with EF Designer</span></span>  

<span data-ttu-id="b6e42-134">Обратите внимание, что свойства DbSet в контексте помечаются как виртуальные.</span><span class="sxs-lookup"><span data-stu-id="b6e42-134">Note that the DbSet properties on the context are marked as virtual.</span></span> <span data-ttu-id="b6e42-135">Это позволит инфраструктуре макетирования получить производный от нашего контекста и переопределить эти свойства с помощью макетной реализации.</span><span class="sxs-lookup"><span data-stu-id="b6e42-135">This will allow the mocking framework to derive from our context and overriding these properties with a mocked implementation.</span></span>  

<span data-ttu-id="b6e42-136">При использовании Code First можно изменить классы напрямую.</span><span class="sxs-lookup"><span data-stu-id="b6e42-136">If you are using Code First then you can edit your classes directly.</span></span> <span data-ttu-id="b6e42-137">При использовании конструктора EF необходимо изменить шаблон T4, который создает контекст.</span><span class="sxs-lookup"><span data-stu-id="b6e42-137">If you are using the EF Designer then you’ll need to edit the T4 template that generates your context.</span></span> <span data-ttu-id="b6e42-138">Откройте \<model_name @ no__t-1. Context.tt файл, вложенный в EDMX-файл, найдите следующий фрагмент кода и добавьте ключевое слово virtual, как показано ниже.</span><span class="sxs-lookup"><span data-stu-id="b6e42-138">Open up the \<model_name\>.Context.tt file that is nested under you edmx file, find the following fragment of code and add in the virtual keyword as shown.</span></span>  

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

## <a name="service-to-be-tested"></a><span data-ttu-id="b6e42-139">Тестируемая служба</span><span class="sxs-lookup"><span data-stu-id="b6e42-139">Service to be tested</span></span>  

<span data-ttu-id="b6e42-140">Чтобы продемонстрировать тестирование с двойными тестами в памяти, мы будем писать несколько тестов для BlogService.</span><span class="sxs-lookup"><span data-stu-id="b6e42-140">To demonstrate testing with in-memory test doubles we are going to be writing a couple of tests for a BlogService.</span></span> <span data-ttu-id="b6e42-141">Служба может создавать новые блоги (Аддблог) и возвращать все блоги, упорядоченные по имени (Жеталлблогс).</span><span class="sxs-lookup"><span data-stu-id="b6e42-141">The service is capable of creating new blogs (AddBlog) and returning all Blogs ordered by name (GetAllBlogs).</span></span> <span data-ttu-id="b6e42-142">Кроме Жеталлблогс, мы также предоставили метод, который будет асинхронно получать все блоги, упорядоченные по имени (Жеталлблогсасинк).</span><span class="sxs-lookup"><span data-stu-id="b6e42-142">In addition to GetAllBlogs, we’ve also provided a method that will asynchronously get all blogs ordered by name (GetAllBlogsAsync).</span></span>  

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

## <a name="testing-non-query-scenarios"></a><span data-ttu-id="b6e42-143">Тестирование сценариев, не использующих запросы</span><span class="sxs-lookup"><span data-stu-id="b6e42-143">Testing non-query scenarios</span></span>  

<span data-ttu-id="b6e42-144">Это все, что нужно сделать, чтобы начать тестирование методов, не относящихся к запросам.</span><span class="sxs-lookup"><span data-stu-id="b6e42-144">That’s all we need to do to start testing non-query methods.</span></span> <span data-ttu-id="b6e42-145">В следующем тесте для создания контекста используется MOQ.</span><span class="sxs-lookup"><span data-stu-id="b6e42-145">The following test uses Moq to create a context.</span></span> <span data-ttu-id="b6e42-146">Затем он создает DbSet @ no__t-0Blog @ no__t-1 и подсоединяет его, чтобы он возвращался из свойства blogs в контексте.</span><span class="sxs-lookup"><span data-stu-id="b6e42-146">It then creates a DbSet\<Blog\> and wires it up to be returned from the context’s Blogs property.</span></span> <span data-ttu-id="b6e42-147">Далее контекст используется для создания нового BlogService, который затем используется для создания нового блога — с помощью метода Аддблог.</span><span class="sxs-lookup"><span data-stu-id="b6e42-147">Next, the context is used to create a new BlogService which is then used to create a new blog – using the AddBlog method.</span></span> <span data-ttu-id="b6e42-148">Наконец, тест подтверждает, что служба добавила новый блог и вызвала команду SaveChanges в контексте.</span><span class="sxs-lookup"><span data-stu-id="b6e42-148">Finally, the test verifies that the service added a new Blog and called SaveChanges on the context.</span></span>  

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

## <a name="testing-query-scenarios"></a><span data-ttu-id="b6e42-149">Тестирование сценариев запросов</span><span class="sxs-lookup"><span data-stu-id="b6e42-149">Testing query scenarios</span></span>  

<span data-ttu-id="b6e42-150">Чтобы иметь возможность выполнять запросы к DbSet тесту Double, необходимо настроить реализацию IQueryable.</span><span class="sxs-lookup"><span data-stu-id="b6e42-150">In order to be able to execute queries against our DbSet test double we need to setup an implementation of IQueryable.</span></span> <span data-ttu-id="b6e42-151">Первым шагом является создание некоторых данных в памяти — мы используем List @ no__t-0Blog @ no__t-1.</span><span class="sxs-lookup"><span data-stu-id="b6e42-151">The first step is to create some in-memory data – we’re using a List\<Blog\>.</span></span> <span data-ttu-id="b6e42-152">Далее мы создадим контекст и DBSet @ no__t-0Blog @ no__t-1, а затем попытаемся реализовать реализацию IQueryable для DbSet – они просто делегируются поставщику LINQ to Objects, который работает с List @ no__t-2T @ no__t-3.</span><span class="sxs-lookup"><span data-stu-id="b6e42-152">Next, we create a context and DBSet\<Blog\> then wire up the IQueryable implementation for the DbSet – they’re just delegating to the LINQ to Objects provider that works with List\<T\>.</span></span>  

<span data-ttu-id="b6e42-153">Затем можно создать BlogService на основе наших тестовых значений типа Double и убедиться, что данные, получаемые из Жеталлблогс, упорядочены по именам.</span><span class="sxs-lookup"><span data-stu-id="b6e42-153">We can then create a BlogService based on our test doubles and ensure that the data we get back from GetAllBlogs is ordered by name.</span></span>  

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

### <a name="testing-with-async-queries"></a><span data-ttu-id="b6e42-154">Тестирование с помощью асинхронных запросов</span><span class="sxs-lookup"><span data-stu-id="b6e42-154">Testing with async queries</span></span>

<span data-ttu-id="b6e42-155">В Entity Framework 6 появился набор методов расширения, которые можно использовать для асинхронного выполнения запроса.</span><span class="sxs-lookup"><span data-stu-id="b6e42-155">Entity Framework 6 introduced a set of extension methods that can be used to asynchronously execute a query.</span></span> <span data-ttu-id="b6e42-156">Примеры этих методов: Толистасинк, Фирстасинк, Фореачасинк и т. д.</span><span class="sxs-lookup"><span data-stu-id="b6e42-156">Examples of these methods include ToListAsync, FirstAsync, ForEachAsync, etc.</span></span>  

<span data-ttu-id="b6e42-157">Поскольку запросы Entity Framework используют LINQ, методы расширения определяются в IQueryable и IEnumerable.</span><span class="sxs-lookup"><span data-stu-id="b6e42-157">Because Entity Framework queries make use of LINQ, the extension methods are defined on IQueryable and IEnumerable.</span></span> <span data-ttu-id="b6e42-158">Однако, поскольку они предназначены только для использования с Entity Framework может возникнуть следующая ошибка при попытке использовать их в запросе LINQ, который не является Entity Frameworkным запросом:</span><span class="sxs-lookup"><span data-stu-id="b6e42-158">However, because they are only designed to be used with Entity Framework you may receive the following error if you try to use them on a LINQ query that isn’t an Entity Framework query:</span></span>

> <span data-ttu-id="b6e42-159">Источник IQueryable не реализует Идбасинценумерабле @ no__t-0.</span><span class="sxs-lookup"><span data-stu-id="b6e42-159">The source IQueryable doesn't implement IDbAsyncEnumerable{0}.</span></span> <span data-ttu-id="b6e42-160">Для Entity Framework асинхронных операций можно использовать только те источники, которые реализуют Идбасинценумерабле.</span><span class="sxs-lookup"><span data-stu-id="b6e42-160">Only sources that implement IDbAsyncEnumerable can be used for Entity Framework asynchronous operations.</span></span> <span data-ttu-id="b6e42-161">Дополнительные сведения см. в разделе [http://go.microsoft.com/fwlink/?LinkId=287068](https://go.microsoft.com/fwlink/?LinkId=287068).</span><span class="sxs-lookup"><span data-stu-id="b6e42-161">For more details see [http://go.microsoft.com/fwlink/?LinkId=287068](https://go.microsoft.com/fwlink/?LinkId=287068).</span></span>  

<span data-ttu-id="b6e42-162">В то время как асинхронные методы поддерживаются только при выполнении запроса EF, их можно использовать в модульном тесте при выполнении теста в памяти с двойной точностью DbSet.</span><span class="sxs-lookup"><span data-stu-id="b6e42-162">Whilst the async methods are only supported when running against an EF query, you may want to use them in your unit test when running against an in-memory test double of a DbSet.</span></span>  

<span data-ttu-id="b6e42-163">Чтобы использовать асинхронные методы, необходимо создать Дбасинккуерипровидер в памяти для обработки асинхронного запроса.</span><span class="sxs-lookup"><span data-stu-id="b6e42-163">In order to use the async methods we need to create an in-memory DbAsyncQueryProvider to process the async query.</span></span> <span data-ttu-id="b6e42-164">В то же самое, что можно было бы настроить поставщик запросов с помощью MOQ, гораздо проще создать тестовую реализацию двойной реализации в коде.</span><span class="sxs-lookup"><span data-stu-id="b6e42-164">Whilst it would be possible to setup a query provider using Moq, it is much easier to create a test double implementation in code.</span></span> <span data-ttu-id="b6e42-165">Код для этой реализации выглядит следующим образом:</span><span class="sxs-lookup"><span data-stu-id="b6e42-165">The code for this implementation is as follows:</span></span>  

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

<span data-ttu-id="b6e42-166">Теперь, когда у нас есть поставщик асинхронных запросов, можно написать модульный тест для нашего нового метода Жеталлблогсасинк.</span><span class="sxs-lookup"><span data-stu-id="b6e42-166">Now that we have an async query provider we can write a unit test for our new GetAllBlogsAsync method.</span></span>  

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
