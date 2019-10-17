---
title: Тестирование с помощью собственных тестовых двойных значений — EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 16a8b7c0-2d23-47f4-9cc0-e2eb2e738ca3
ms.openlocfilehash: 3d8933fb5e17f8c01f3971495a1fcdb5b8cfab57
ms.sourcegitcommit: 37d0e0fd1703467918665a64837dc54ad2ec7484
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/16/2019
ms.locfileid: "72446024"
---
# <a name="testing-with-your-own-test-doubles"></a><span data-ttu-id="c64e3-102">Тестирование с помощью пользовательских двойной проверки</span><span class="sxs-lookup"><span data-stu-id="c64e3-102">Testing with your own test doubles</span></span>
> [!NOTE]
> <span data-ttu-id="c64e3-103">**Только в EF6 и более поздних версиях**. Функции, API и другие возможности, описанные на этой странице, появились в Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="c64e3-103">**EF6 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 6.</span></span> <span data-ttu-id="c64e3-104">При использовании более ранней версии могут быть неприменимы некоторые или все сведения.</span><span class="sxs-lookup"><span data-stu-id="c64e3-104">If you are using an earlier version, some or all of the information does not apply.</span></span>  

<span data-ttu-id="c64e3-105">При написании тестов для приложения часто желательно избежать попадания в базу данных.</span><span class="sxs-lookup"><span data-stu-id="c64e3-105">When writing tests for your application it is often desirable to avoid hitting the database.</span></span>  <span data-ttu-id="c64e3-106">Entity Framework позволяет добиться этого, создавая контекст с последующим поведением, определенным в тестах. Это позволяет использовать данные в памяти.</span><span class="sxs-lookup"><span data-stu-id="c64e3-106">Entity Framework allows you to achieve this by creating a context – with behavior defined by your tests – that makes use of in-memory data.</span></span>  

## <a name="options-for-creating-test-doubles"></a><span data-ttu-id="c64e3-107">Параметры для создания тестовых значений типа Double</span><span class="sxs-lookup"><span data-stu-id="c64e3-107">Options for creating test doubles</span></span>  

<span data-ttu-id="c64e3-108">Существует два разных подхода, которые можно использовать для создания версии контекста в памяти.</span><span class="sxs-lookup"><span data-stu-id="c64e3-108">There are two different approaches that can be used to create an in-memory version of your context.</span></span>  

- <span data-ttu-id="c64e3-109">**Создание собственных тестовых двойных** значений. Этот подход включает написание собственной реализации контекста и дбсетс в памяти.</span><span class="sxs-lookup"><span data-stu-id="c64e3-109">**Create your own test doubles** – This approach involves writing your own in-memory implementation of your context and DbSets.</span></span> <span data-ttu-id="c64e3-110">Это дает множество контроля над поведением классов, но может задействовать написание и владельца разумного объема кода.</span><span class="sxs-lookup"><span data-stu-id="c64e3-110">This gives you a lot of control over how the classes behave but can involve writing and owning a reasonable amount of code.</span></span>  
- <span data-ttu-id="c64e3-111">**Использование инфраструктуры макетирования для создания тестовых значений Double** . с помощью инфраструктуры макетирования (например, MOQ) можно реализовать в памяти реализации контекста и наборов, создаваемых динамически во время выполнения.</span><span class="sxs-lookup"><span data-stu-id="c64e3-111">**Use a mocking framework to create test doubles** – Using a mocking framework (such as Moq) you can have the in-memory implementations of you context and sets created dynamically at runtime for you.</span></span>  

<span data-ttu-id="c64e3-112">В этой статье рассматривается создание собственного тестового типа Double.</span><span class="sxs-lookup"><span data-stu-id="c64e3-112">This article will deal with creating your own test double.</span></span> <span data-ttu-id="c64e3-113">Сведения об использовании инфраструктуры макетирования см. в разделе [тестирование с использованием инфраструктуры макетирования](mocking.md).</span><span class="sxs-lookup"><span data-stu-id="c64e3-113">For information on using a mocking framework see [Testing with a Mocking Framework](mocking.md).</span></span>  

## <a name="testing-with-pre-ef6-versions"></a><span data-ttu-id="c64e3-114">Тестирование с использованием предварительно EF6 версий</span><span class="sxs-lookup"><span data-stu-id="c64e3-114">Testing with pre-EF6 versions</span></span>  

<span data-ttu-id="c64e3-115">Код, приведенный в этой статье, совместим с EF6.</span><span class="sxs-lookup"><span data-stu-id="c64e3-115">The code shown in this article is compatible with EF6.</span></span> <span data-ttu-id="c64e3-116">Тестирование с помощью EF5 и более ранних версий см. в разделе [тестирование с имитацией контекста](https://romiller.com/2012/02/14/testing-with-a-fake-dbcontext/).</span><span class="sxs-lookup"><span data-stu-id="c64e3-116">For testing with EF5 and earlier version see [Testing with a Fake Context](https://romiller.com/2012/02/14/testing-with-a-fake-dbcontext/).</span></span>  

## <a name="limitations-of-ef-in-memory-test-doubles"></a><span data-ttu-id="c64e3-117">Ограничения для теста EF в памяти, Double</span><span class="sxs-lookup"><span data-stu-id="c64e3-117">Limitations of EF in-memory test doubles</span></span>  

<span data-ttu-id="c64e3-118">Числа двойной проверки в памяти могут быть хорошим способом обеспечения уровня модульного тестирования для битов приложения, использующих EF.</span><span class="sxs-lookup"><span data-stu-id="c64e3-118">In-memory test doubles can be a good way to provide unit test level coverage of bits of your application that use EF.</span></span> <span data-ttu-id="c64e3-119">Однако при этом вы используете LINQ to Objects для выполнения запросов к данным в памяти.</span><span class="sxs-lookup"><span data-stu-id="c64e3-119">However, when doing this you are using LINQ to Objects to execute queries against in-memory data.</span></span> <span data-ttu-id="c64e3-120">Это может привести к различному поведению, чем использование поставщика LINQ (LINQ to Entities) EF для преобразования запросов в SQL, выполняемых в базе данных.</span><span class="sxs-lookup"><span data-stu-id="c64e3-120">This can result in different behavior than using EF’s LINQ provider (LINQ to Entities) to translate queries into SQL that is run against your database.</span></span>  

<span data-ttu-id="c64e3-121">Одним из примеров такого различия является загрузка связанных данных.</span><span class="sxs-lookup"><span data-stu-id="c64e3-121">One example of such a difference is loading related data.</span></span> <span data-ttu-id="c64e3-122">Если вы создадите серию блогов, в каждой из которых есть связанные записи, то при использовании данных в памяти связанные записи всегда будут загружаться для каждого блога.</span><span class="sxs-lookup"><span data-stu-id="c64e3-122">If you create a series of Blogs that each have related Posts, then when using in-memory data the related Posts will always be loaded for each Blog.</span></span> <span data-ttu-id="c64e3-123">Однако при работе с базой данных данные будут загружаться только при использовании метода Include.</span><span class="sxs-lookup"><span data-stu-id="c64e3-123">However, when running against a database the data will only be loaded if you use the Include method.</span></span>  

<span data-ttu-id="c64e3-124">По этой причине рекомендуется всегда включать некоторый уровень сквозного тестирования (в дополнение к модульным тестам), чтобы обеспечить правильную работу приложения в базе данных.</span><span class="sxs-lookup"><span data-stu-id="c64e3-124">For this reason, it is recommended to always include some level of end-to-end testing (in addition to your unit tests) to ensure your application works correctly against a database.</span></span>  

## <a name="following-along-with-this-article"></a><span data-ttu-id="c64e3-125">Далее вместе с этой статьей</span><span class="sxs-lookup"><span data-stu-id="c64e3-125">Following along with this article</span></span>  

<span data-ttu-id="c64e3-126">В этой статье приведены полные листинги кода, которые можно скопировать в Visual Studio, если хотите.</span><span class="sxs-lookup"><span data-stu-id="c64e3-126">This article gives complete code listings that you can copy into Visual Studio to follow along if you wish.</span></span> <span data-ttu-id="c64e3-127">Проще всего создать **проект модульного теста** , а для выполнения разделов, использующих async, необходимо выбрать **.NET Framework 4,5** .</span><span class="sxs-lookup"><span data-stu-id="c64e3-127">It's easiest to create a **Unit Test Project** and you will need to target **.NET Framework 4.5** to complete the sections that use async.</span></span>  

## <a name="creating-a-context-interface"></a><span data-ttu-id="c64e3-128">Создание интерфейса контекста</span><span class="sxs-lookup"><span data-stu-id="c64e3-128">Creating a context interface</span></span>  

<span data-ttu-id="c64e3-129">Мы будем рассматривать тестирование службы, использующей модель EF.</span><span class="sxs-lookup"><span data-stu-id="c64e3-129">We're going to look at testing a service that makes use of an EF model.</span></span> <span data-ttu-id="c64e3-130">Чтобы иметь возможность заменить наш контекст EF на версию в памяти для тестирования, мы определим интерфейс, реализуемый нашим контекстом EF (и его в памяти Double).</span><span class="sxs-lookup"><span data-stu-id="c64e3-130">In order to be able to replace our EF context with an in-memory version for testing, we'll define an interface that our EF context (and it's in-memory double) will implement.</span></span>

<span data-ttu-id="c64e3-131">Служба, которую мы собираемся тестировать, будет запрашивать и изменять данные с помощью свойств DbSet в нашем контексте, а также вызывать метод SaveChanges для отправки изменений в базу данных.</span><span class="sxs-lookup"><span data-stu-id="c64e3-131">The service we are going to test will query and modify data using the DbSet properties of our context and also call SaveChanges to push changes to the database.</span></span> <span data-ttu-id="c64e3-132">Так что мы будем включать эти члены в интерфейс.</span><span class="sxs-lookup"><span data-stu-id="c64e3-132">So we're including these members on the interface.</span></span>  

``` csharp
using System.Data.Entity;

namespace TestingDemo
{
    public interface IBloggingContext
    {
        DbSet<Blog> Blogs { get; }
        DbSet<Post> Posts { get; }
        int SaveChanges();
    }
}
```  

## <a name="the-ef-model"></a><span data-ttu-id="c64e3-133">Модель EF</span><span class="sxs-lookup"><span data-stu-id="c64e3-133">The EF model</span></span>  

<span data-ttu-id="c64e3-134">Служба, которую мы собираемся тестировать, использует модель EF, состоящие из BloggingContext, а также классов блога и POST.</span><span class="sxs-lookup"><span data-stu-id="c64e3-134">The service we're going to test makes use of an EF model made up of the BloggingContext and the Blog and Post classes.</span></span> <span data-ttu-id="c64e3-135">Этот код может быть создан конструктором EF или моделью Code First.</span><span class="sxs-lookup"><span data-stu-id="c64e3-135">This code may have been generated by the EF Designer or be a Code First model.</span></span>  

``` csharp
using System.Collections.Generic;
using System.Data.Entity;

namespace TestingDemo
{
    public class BloggingContext : DbContext, IBloggingContext
    {
        public DbSet<Blog> Blogs { get; set; }
        public DbSet<Post> Posts { get; set; }
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

### <a name="implementing-the-context-interface-with-the-ef-designer"></a><span data-ttu-id="c64e3-136">Реализация интерфейса контекста с помощью конструктора EF</span><span class="sxs-lookup"><span data-stu-id="c64e3-136">Implementing the context interface with the EF Designer</span></span>  

<span data-ttu-id="c64e3-137">Обратите внимание, что наш контекст реализует интерфейс Иблоггингконтекст.</span><span class="sxs-lookup"><span data-stu-id="c64e3-137">Note that our context implements the IBloggingContext interface.</span></span>  

<span data-ttu-id="c64e3-138">При использовании Code First можно изменить контекст непосредственно для реализации интерфейса.</span><span class="sxs-lookup"><span data-stu-id="c64e3-138">If you are using Code First then you can edit your context directly to implement the interface.</span></span> <span data-ttu-id="c64e3-139">При использовании конструктора EF необходимо изменить шаблон T4, который создает контекст.</span><span class="sxs-lookup"><span data-stu-id="c64e3-139">If you are using the EF Designer then you’ll need to edit the T4 template that generates your context.</span></span> <span data-ttu-id="c64e3-140">Откройте \<model_name @ no__t-1. Context.tt файл, вложенный в EDMX-файл, найдите следующий фрагмент кода и добавьте его в интерфейс, как показано ниже.</span><span class="sxs-lookup"><span data-stu-id="c64e3-140">Open up the \<model_name\>.Context.tt file that is nested under you edmx file, find the following fragment of code and add in the interface as shown.</span></span>  

``` csharp  
<#=Accessibility.ForType(container)#> partial class <#=code.Escape(container)#> : DbContext, IBloggingContext
```  

## <a name="service-to-be-tested"></a><span data-ttu-id="c64e3-141">Тестируемая служба</span><span class="sxs-lookup"><span data-stu-id="c64e3-141">Service to be tested</span></span>  

<span data-ttu-id="c64e3-142">Чтобы продемонстрировать тестирование с двойными тестами в памяти, мы будем писать несколько тестов для BlogService.</span><span class="sxs-lookup"><span data-stu-id="c64e3-142">To demonstrate testing with in-memory test doubles we are going to be writing a couple of tests for a BlogService.</span></span> <span data-ttu-id="c64e3-143">Служба может создавать новые блоги (Аддблог) и возвращать все блоги, упорядоченные по имени (Жеталлблогс).</span><span class="sxs-lookup"><span data-stu-id="c64e3-143">The service is capable of creating new blogs (AddBlog) and returning all Blogs ordered by name (GetAllBlogs).</span></span> <span data-ttu-id="c64e3-144">Кроме Жеталлблогс, мы также предоставили метод, который будет асинхронно получать все блоги, упорядоченные по имени (Жеталлблогсасинк).</span><span class="sxs-lookup"><span data-stu-id="c64e3-144">In addition to GetAllBlogs, we’ve also provided a method that will asynchronously get all blogs ordered by name (GetAllBlogsAsync).</span></span>  

``` csharp
using System.Collections.Generic;
using System.Data.Entity;
using System.Linq;
using System.Threading.Tasks;

namespace TestingDemo
{
    public class BlogService
    {
        private IBloggingContext _context;

        public BlogService(IBloggingContext context)
        {
            _context = context;
        }

        public Blog AddBlog(string name, string url)
        {
            var blog = new Blog { Name = name, Url = url };
            _context.Blogs.Add(blog);
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

## <a name="creating-the-in-memory-test-doubles"></a><span data-ttu-id="c64e3-145">Создание двойных тестов в памяти</span><span class="sxs-lookup"><span data-stu-id="c64e3-145">Creating the in-memory test doubles</span></span>  

<span data-ttu-id="c64e3-146">Теперь, когда у нас есть реальная модель EF и служба, которая может использовать ее, пришло время создать тестовое в памяти значение Double, которое можно использовать для тестирования.</span><span class="sxs-lookup"><span data-stu-id="c64e3-146">Now that we have the real EF model and the service that can use it, it's time to create the in-memory test double that we can use for testing.</span></span> <span data-ttu-id="c64e3-147">Для нашего контекста мы создали тест TestContext Double.</span><span class="sxs-lookup"><span data-stu-id="c64e3-147">We've created a TestContext test double for our context.</span></span> <span data-ttu-id="c64e3-148">В тестах Double мы получаем выбор поведения для поддержки тестов, которые мы будем выполнять.</span><span class="sxs-lookup"><span data-stu-id="c64e3-148">In test doubles we get to choose the behavior we want in order to support the tests we are going to run.</span></span> <span data-ttu-id="c64e3-149">В этом примере мы просто записываем количество вызовов SaveChanges, но можно включить любую логику, необходимую для проверки тестируемого сценария.</span><span class="sxs-lookup"><span data-stu-id="c64e3-149">In this example we're just capturing the number of times SaveChanges is called, but you can include whatever logic is needed to verify the scenario you are testing.</span></span>  

<span data-ttu-id="c64e3-150">Мы также создали Тестдбсет, который предоставляет реализацию DbSet в памяти.</span><span class="sxs-lookup"><span data-stu-id="c64e3-150">We've also created a TestDbSet that provides an in-memory implementation of DbSet.</span></span> <span data-ttu-id="c64e3-151">Мы предоставили полную реализацию всех методов в DbSet (за исключением Find), но достаточно реализовать только те элементы, которые будут использоваться сценарием тестирования.</span><span class="sxs-lookup"><span data-stu-id="c64e3-151">We've provided a complete implemention for all the methods on DbSet (except for Find), but you only need to implement the members that your test scenario will use.</span></span>  

<span data-ttu-id="c64e3-152">Тестдбсет использует некоторые другие классы инфраструктуры, которые мы включили для обеспечения возможности обработки асинхронных запросов.</span><span class="sxs-lookup"><span data-stu-id="c64e3-152">TestDbSet makes use of some other infrastructure classes that we've included to ensure that async queries can be processed.</span></span>  

``` csharp
using System;
using System.Collections.Generic;
using System.Collections.ObjectModel;
using System.Data.Entity;
using System.Data.Entity.Infrastructure;
using System.Linq;
using System.Linq.Expressions;
using System.Threading;
using System.Threading.Tasks;

namespace TestingDemo
{
    public class TestContext : IBloggingContext
    {
        public TestContext()
        {
            this.Blogs = new TestDbSet<Blog>();
            this.Posts = new TestDbSet<Post>();
        }

        public DbSet<Blog> Blogs { get; set; }
        public DbSet<Post> Posts { get; set; }
        public int SaveChangesCount { get; private set; }
        public int SaveChanges()
        {
            this.SaveChangesCount++;
            return 1;
        }
    }

    public class TestDbSet<TEntity> : DbSet<TEntity>, IQueryable, IEnumerable<TEntity>, IDbAsyncEnumerable<TEntity>
        where TEntity : class
    {
        ObservableCollection<TEntity> _data;
        IQueryable _query;

        public TestDbSet()
        {
            _data = new ObservableCollection<TEntity>();
            _query = _data.AsQueryable();
        }

        public override TEntity Add(TEntity item)
        {
            _data.Add(item);
            return item;
        }

        public override TEntity Remove(TEntity item)
        {
            _data.Remove(item);
            return item;
        }

        public override TEntity Attach(TEntity item)
        {
            _data.Add(item);
            return item;
        }

        public override TEntity Create()
        {
            return Activator.CreateInstance<TEntity>();
        }

        public override TDerivedEntity Create<TDerivedEntity>()
        {
            return Activator.CreateInstance<TDerivedEntity>();
        }

        public override ObservableCollection<TEntity> Local
        {
            get { return _data; }
        }

        Type IQueryable.ElementType
        {
            get { return _query.ElementType; }
        }

        Expression IQueryable.Expression
        {
            get { return _query.Expression; }
        }

        IQueryProvider IQueryable.Provider
        {
            get { return new TestDbAsyncQueryProvider<TEntity>(_query.Provider); }
        }

        System.Collections.IEnumerator System.Collections.IEnumerable.GetEnumerator()
        {
            return _data.GetEnumerator();
        }

        IEnumerator<TEntity> IEnumerable<TEntity>.GetEnumerator()
        {
            return _data.GetEnumerator();
        }

        IDbAsyncEnumerator<TEntity> IDbAsyncEnumerable<TEntity>.GetAsyncEnumerator()
        {
            return new TestDbAsyncEnumerator<TEntity>(_data.GetEnumerator());
        }
    }

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

### <a name="implementing-find"></a><span data-ttu-id="c64e3-153">Реализация поиска</span><span class="sxs-lookup"><span data-stu-id="c64e3-153">Implementing Find</span></span>  

<span data-ttu-id="c64e3-154">Метод Find трудно реализовать обычным способом.</span><span class="sxs-lookup"><span data-stu-id="c64e3-154">The Find method is difficult to implement in a generic fashion.</span></span> <span data-ttu-id="c64e3-155">Если необходимо протестировать код, который использует метод Find, проще всего создать тестовый DbSet для каждого из типов сущностей, которые должны поддерживать поиск.</span><span class="sxs-lookup"><span data-stu-id="c64e3-155">If you need to test code that makes use of the Find method it is easiest to create a test DbSet for each of the entity types that need to support find.</span></span> <span data-ttu-id="c64e3-156">Затем можно написать логику для поиска конкретного типа сущности, как показано ниже.</span><span class="sxs-lookup"><span data-stu-id="c64e3-156">You can then write logic to find that particular type of entity, as shown below.</span></span>  

``` csharp
using System.Linq;

namespace TestingDemo
{
    class TestBlogDbSet : TestDbSet<Blog>
    {
        public override Blog Find(params object[] keyValues)
        {
            var id = (int)keyValues.Single();
            return this.SingleOrDefault(b => b.BlogId == id);
        }
    }
}
```  

## <a name="writing-some-tests"></a><span data-ttu-id="c64e3-157">Написание некоторых тестов</span><span class="sxs-lookup"><span data-stu-id="c64e3-157">Writing some tests</span></span>  

<span data-ttu-id="c64e3-158">Это все, что нужно сделать для начала тестирования.</span><span class="sxs-lookup"><span data-stu-id="c64e3-158">That’s all we need to do to start testing.</span></span> <span data-ttu-id="c64e3-159">В следующем тесте создается TestContext, а затем служба на основе этого контекста.</span><span class="sxs-lookup"><span data-stu-id="c64e3-159">The following test creates a TestContext and then a service based on this context.</span></span> <span data-ttu-id="c64e3-160">Затем служба используется для создания нового блога — с помощью метода Аддблог.</span><span class="sxs-lookup"><span data-stu-id="c64e3-160">The service is then used to create a new blog – using the AddBlog method.</span></span> <span data-ttu-id="c64e3-161">Наконец, тест подтверждает, что служба добавила новый блог к свойству блогов в контексте и вызвал команду SaveChanges в контексте.</span><span class="sxs-lookup"><span data-stu-id="c64e3-161">Finally, the test verifies that the service added a new Blog to the context's Blogs property and called SaveChanges on the context.</span></span>  

<span data-ttu-id="c64e3-162">Это лишь пример типов объектов, которые можно протестировать с помощью двойного теста в памяти, и можно настроить логику тестовых значений Double и проверку в соответствии с вашими требованиями.</span><span class="sxs-lookup"><span data-stu-id="c64e3-162">This is just an example of the types of things you can test with an in-memory test double and you can adjust the logic of the test doubles and the verification to meet your requirements.</span></span>  

``` csharp
using Microsoft.VisualStudio.TestTools.UnitTesting;
using System.Linq;

namespace TestingDemo
{
    [TestClass]
    public class NonQueryTests
    {
        [TestMethod]
        public void CreateBlog_saves_a_blog_via_context()
        {
            var context = new TestContext();

            var service = new BlogService(context);
            service.AddBlog("ADO.NET Blog", "http://blogs.msdn.com/adonet");

            Assert.AreEqual(1, context.Blogs.Count());
            Assert.AreEqual("ADO.NET Blog", context.Blogs.Single().Name);
            Assert.AreEqual("http://blogs.msdn.com/adonet", context.Blogs.Single().Url);
            Assert.AreEqual(1, context.SaveChangesCount);
        }
    }
}
```  

<span data-ttu-id="c64e3-163">Ниже приведен еще один пример тестового времени, который выполняет запрос.</span><span class="sxs-lookup"><span data-stu-id="c64e3-163">Here is another example of a test - this time one that performs a query.</span></span> <span data-ttu-id="c64e3-164">Тест начинается с создания контекста теста с некоторыми данными в своем свойстве блога. Обратите внимание, что данные находятся не в алфавитном порядке.</span><span class="sxs-lookup"><span data-stu-id="c64e3-164">The test starts by creating a test context with some data in its Blog property - note that the data is not in alphabetical order.</span></span> <span data-ttu-id="c64e3-165">Затем можно создать BlogService на основе нашего контекста теста и убедиться, что данные, получаемые из Жеталлблогс, упорядочены по имени.</span><span class="sxs-lookup"><span data-stu-id="c64e3-165">We can then create a BlogService based on our test context and ensure that the data we get back from GetAllBlogs is ordered by name.</span></span>  

``` csharp
using Microsoft.VisualStudio.TestTools.UnitTesting;

namespace TestingDemo
{
    [TestClass]
    public class QueryTests
    {
        [TestMethod]
        public void GetAllBlogs_orders_by_name()
        {
            var context = new TestContext();
            context.Blogs.Add(new Blog { Name = "BBB" });
            context.Blogs.Add(new Blog { Name = "ZZZ" });
            context.Blogs.Add(new Blog { Name = "AAA" });

            var service = new BlogService(context);
            var blogs = service.GetAllBlogs();

            Assert.AreEqual(3, blogs.Count);
            Assert.AreEqual("AAA", blogs[0].Name);
            Assert.AreEqual("BBB", blogs[1].Name);
            Assert.AreEqual("ZZZ", blogs[2].Name);
        }
    }
}
```  

<span data-ttu-id="c64e3-166">Наконец, мы напишем еще один тест, использующий наш асинхронный метод, чтобы убедиться, что асинхронная инфраструктура, включенная в [тестдбсет](#creating-the-in-memory-test-doubles) , работает.</span><span class="sxs-lookup"><span data-stu-id="c64e3-166">Finally, we'll write one more test that uses our async method to ensure that the async infrastructure we included in [TestDbSet](#creating-the-in-memory-test-doubles) is working.</span></span>  

``` csharp
using Microsoft.VisualStudio.TestTools.UnitTesting;
using System.Collections.Generic;
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
            var context = new TestContext();
            context.Blogs.Add(new Blog { Name = "BBB" });
            context.Blogs.Add(new Blog { Name = "ZZZ" });
            context.Blogs.Add(new Blog { Name = "AAA" });

            var service = new BlogService(context);
            var blogs = await service.GetAllBlogsAsync();

            Assert.AreEqual(3, blogs.Count);
            Assert.AreEqual("AAA", blogs[0].Name);
            Assert.AreEqual("BBB", blogs[1].Name);
            Assert.AreEqual("ZZZ", blogs[2].Name);
        }
    }
}
```  
