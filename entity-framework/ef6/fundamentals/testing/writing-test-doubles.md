---
title: Тестирование с помощью собственных тестовых дублеров - EF6
author: divega
ms.date: 2016-10-23
ms.assetid: 16a8b7c0-2d23-47f4-9cc0-e2eb2e738ca3
ms.openlocfilehash: fa63c483e0a55b6cbd43382f68ab9592f3c1768b
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/27/2018
ms.locfileid: "42997221"
---
# <a name="testing-with-your-own-test-doubles"></a><span data-ttu-id="960c7-102">Тестирование с помощью собственных тестовых дублеров</span><span class="sxs-lookup"><span data-stu-id="960c7-102">Testing with your own test doubles</span></span>
> [!NOTE]
> <span data-ttu-id="960c7-103">**Только в EF6 и более поздних версиях**. Функции, API и другие возможности, описанные на этой странице, появились в Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="960c7-103">**EF6 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 6.</span></span> <span data-ttu-id="960c7-104">При использовании более ранней версии могут быть неприменимы некоторые или все сведения.</span><span class="sxs-lookup"><span data-stu-id="960c7-104">If you are using an earlier version, some or all of the information does not apply.</span></span>  

<span data-ttu-id="960c7-105">При написании тестов для приложения часто желательно избежать обращения к базе данных.</span><span class="sxs-lookup"><span data-stu-id="960c7-105">When writing tests for your application it is often desirable to avoid hitting the database.</span></span>  <span data-ttu-id="960c7-106">Платформа Entity Framework позволяет добиться этого путем создания контекста — с поведение, определяемое тестов —, в котором используется данных в памяти.</span><span class="sxs-lookup"><span data-stu-id="960c7-106">Entity Framework allows you to achieve this by creating a context – with behavior defined by your tests – that makes use of in-memory data.</span></span>  

## <a name="options-for-creating-test-doubles"></a><span data-ttu-id="960c7-107">Параметры для создания тестовых дублеров</span><span class="sxs-lookup"><span data-stu-id="960c7-107">Options for creating test doubles</span></span>  

<span data-ttu-id="960c7-108">Существует два различных подхода, которые могут использоваться для создания в памяти версии контекста.</span><span class="sxs-lookup"><span data-stu-id="960c7-108">There are two different approaches that can be used to create an in-memory version of your context.</span></span>  

- <span data-ttu-id="960c7-109">**Создать свои собственные тестовые дублеры** — этот подход подразумевает написание собственной реализации в памяти DbSets и данные контекста.</span><span class="sxs-lookup"><span data-stu-id="960c7-109">**Create your own test doubles** – This approach involves writing your own in-memory implementation of your context and DbSets.</span></span> <span data-ttu-id="960c7-110">Это обеспечивает большую контроль над тем, как ведут себя классы, но может включать в себя написание и владельцев разумный объем кода.</span><span class="sxs-lookup"><span data-stu-id="960c7-110">This gives you a lot of control over how the classes behave but can involve writing and owning a reasonable amount of code.</span></span>  
- <span data-ttu-id="960c7-111">**Использовать платформа имитации для создания тестовых дублеров** — использование платформы имитированной (например, Moq) может иметь реализаций в памяти из вас контекст и наборов, созданных динамически во время выполнения для вас.</span><span class="sxs-lookup"><span data-stu-id="960c7-111">**Use a mocking framework to create test doubles** – Using a mocking framework (such as Moq) you can have the in-memory implementations of you context and sets created dynamically at runtime for you.</span></span>  

<span data-ttu-id="960c7-112">В этой статье будет иметь дело с созданием собственных тестов double.</span><span class="sxs-lookup"><span data-stu-id="960c7-112">This article will deal with creating your own test double.</span></span> <span data-ttu-id="960c7-113">Сведения об использовании платформа имитации см. в разделе [тестирование на платформе, макетирование](mocking.md).</span><span class="sxs-lookup"><span data-stu-id="960c7-113">For information on using a mocking framework see [Testing with a Mocking Framework](mocking.md).</span></span>  

## <a name="testing-with-pre-ef6-versions"></a><span data-ttu-id="960c7-114">Тестирование с помощью EF6 предварительной версии</span><span class="sxs-lookup"><span data-stu-id="960c7-114">Testing with pre-EF6 versions</span></span>  

<span data-ttu-id="960c7-115">Код, показанный в этой статье совместим с EF6.</span><span class="sxs-lookup"><span data-stu-id="960c7-115">The code shown in this article is compatible with EF6.</span></span> <span data-ttu-id="960c7-116">Тестирование с помощью EF5 и более ранней версии см. в разделе [тестирования с контекстом имитировать](http://romiller.com/2012/02/14/testing-with-a-fake-dbcontext/).</span><span class="sxs-lookup"><span data-stu-id="960c7-116">For testing with EF5 and earlier version see [Testing with a Fake Context](http://romiller.com/2012/02/14/testing-with-a-fake-dbcontext/).</span></span>  

## <a name="limitations-of-ef-in-memory-test-doubles"></a><span data-ttu-id="960c7-117">Ограничения EF in-Memory тестовых дублеров</span><span class="sxs-lookup"><span data-stu-id="960c7-117">Limitations of EF in-memory test doubles</span></span>  

<span data-ttu-id="960c7-118">В памяти тестовых дублеров может быть хорошим средством предоставления уровня покрытия битов приложения, использующие EF модульного теста.</span><span class="sxs-lookup"><span data-stu-id="960c7-118">In-memory test doubles can be a good way to provide unit test level coverage of bits of your application that use EF.</span></span> <span data-ttu-id="960c7-119">Тем не менее при этом при использовании LINQ to Objects для выполнения запросов к данным в памяти.</span><span class="sxs-lookup"><span data-stu-id="960c7-119">However, when doing this you are using LINQ to Objects to execute queries against in-memory data.</span></span> <span data-ttu-id="960c7-120">Это может привести другое поведение, чем при использовании поставщика платформы EF LINQ (LINQ to Entities) для преобразования запросов в SQL, которая выполняется в базе данных.</span><span class="sxs-lookup"><span data-stu-id="960c7-120">This can result in different behavior than using EF’s LINQ provider (LINQ to Entities) to translate queries into SQL that is run against your database.</span></span>  

<span data-ttu-id="960c7-121">Примером такой разницы загрузка связанных данных.</span><span class="sxs-lookup"><span data-stu-id="960c7-121">One example of such a difference is loading related data.</span></span> <span data-ttu-id="960c7-122">Если создать серию блогов, в которых тематических записях блога, то при использовании данных в памяти связанных сообщений всегда будут загружаться для каждого блога.</span><span class="sxs-lookup"><span data-stu-id="960c7-122">If you create a series of Blogs that each have related Posts, then when using in-memory data the related Posts will always be loaded for each Blog.</span></span> <span data-ttu-id="960c7-123">Тем не менее при выполнении в базе данных будет загружаться только при использовании метода Include.</span><span class="sxs-lookup"><span data-stu-id="960c7-123">However, when running against a database the data will only be loaded if you use the Include method.</span></span>  

<span data-ttu-id="960c7-124">По этой причине рекомендуется всегда включать некоторый уровень end-to-end тестирования (в дополнение к модульные тесты), чтобы ваше приложение работает правильно для базы данных.</span><span class="sxs-lookup"><span data-stu-id="960c7-124">For this reason, it is recommended to always include some level of end-to-end testing (in addition to your unit tests) to ensure your application works correctly against a database.</span></span>  

## <a name="following-along-with-this-article"></a><span data-ttu-id="960c7-125">Следуя указаниям в этой статье</span><span class="sxs-lookup"><span data-stu-id="960c7-125">Following along with this article</span></span>  

<span data-ttu-id="960c7-126">В этой статье приводятся полные Листинги кода, которые можно скопировать в Visual Studio для выполнения этой процедуры при необходимости.</span><span class="sxs-lookup"><span data-stu-id="960c7-126">This article gives complete code listings that you can copy into Visual Studio to follow along if you wish.</span></span> <span data-ttu-id="960c7-127">Проще всего создать **проект модульного теста** и требуется целевой объект **.NET Framework 4.5** для выполнения в разделах, которые используют асинхронный.</span><span class="sxs-lookup"><span data-stu-id="960c7-127">It's easiest to create a **Unit Test Project** and you will need to target **.NET Framework 4.5** to complete the sections that use async.</span></span>  

## <a name="creating-a-context-interface"></a><span data-ttu-id="960c7-128">Создание интерфейса контекста</span><span class="sxs-lookup"><span data-stu-id="960c7-128">Creating a context interface</span></span>  

<span data-ttu-id="960c7-129">Мы собираемся тестирования службы, который использует EF модели.</span><span class="sxs-lookup"><span data-stu-id="960c7-129">We're going to look at testing a service that makes use of an EF model.</span></span> <span data-ttu-id="960c7-130">Чтобы иметь возможность заменять наш контекст EF с версией в памяти для тестирования, мы определим интерфейс что наш контекст EF (и double в памяти) будет imeplement.</span><span class="sxs-lookup"><span data-stu-id="960c7-130">In order to be able to replace our EF context with an in-memory version for testing, we'll define an interface that our EF context (and it's in-memory double) will imeplement.</span></span>  

<span data-ttu-id="960c7-131">Службы, с которой мы хотим проверить запрос и изменение данных с помощью свойства DbSet наш контекст и также вызывать метод SaveChanges для внесения изменений в базу данных.</span><span class="sxs-lookup"><span data-stu-id="960c7-131">The service we are going to test will query and modify data using the DbSet properties of our context and also call SaveChanges to push changes to the database.</span></span> <span data-ttu-id="960c7-132">Поэтому мы добавим эти члены в интерфейсе.</span><span class="sxs-lookup"><span data-stu-id="960c7-132">So we're including these members on the interface.</span></span>  

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

## <a name="the-ef-model"></a><span data-ttu-id="960c7-133">Модель EF</span><span class="sxs-lookup"><span data-stu-id="960c7-133">The EF model</span></span>  

<span data-ttu-id="960c7-134">Службы, мы собираемся тестирования использует EF модели BloggingContext и классы блог и Post.</span><span class="sxs-lookup"><span data-stu-id="960c7-134">The service we're going to test makes use of an EF model made up of the BloggingContext and the Blog and Post classes.</span></span> <span data-ttu-id="960c7-135">Этот код был создан в конструкторе EF или быть модели Code First.</span><span class="sxs-lookup"><span data-stu-id="960c7-135">This code may have been generated by the EF Designer or be a Code First model.</span></span>  

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

### <a name="implementing-the-context-interface-with-the-ef-designer"></a><span data-ttu-id="960c7-136">Реализация интерфейса контекста с помощью конструктора EF</span><span class="sxs-lookup"><span data-stu-id="960c7-136">Implementing the context interface with the EF Designer</span></span>  

<span data-ttu-id="960c7-137">Обратите внимание на то, что наш контекст реализует интерфейс IBloggingContext.</span><span class="sxs-lookup"><span data-stu-id="960c7-137">Note that our context implements the IBloggingContext interface.</span></span>  

<span data-ttu-id="960c7-138">Если вы используете Code First можно изменить текущий контекст непосредственно реализуют интерфейс.</span><span class="sxs-lookup"><span data-stu-id="960c7-138">If you are using Code First then you can edit your context directly to implement the interface.</span></span> <span data-ttu-id="960c7-139">Если вы используете конструктор EF необходимо изменить шаблон T4, который создает контекст.</span><span class="sxs-lookup"><span data-stu-id="960c7-139">If you are using the EF Designer then you’ll need to edit the T4 template that generates your context.</span></span> <span data-ttu-id="960c7-140">Откройте \<model_name\>. Context.tt файл, который вложен в вы EDMX-файла, найдите следующий фрагмент кода и добавьте в интерфейсе, как показано.</span><span class="sxs-lookup"><span data-stu-id="960c7-140">Open up the \<model_name\>.Context.tt file that is nested under you edmx file, find the following fragment of code and add in the interface as shown.</span></span>  

``` csharp  
<#=Accessibility.ForType(container)#> partial class <#=code.Escape(container)#> : DbContext, IBloggingContext
```  

## <a name="service-to-be-tested"></a><span data-ttu-id="960c7-141">Служба проверяемый</span><span class="sxs-lookup"><span data-stu-id="960c7-141">Service to be tested</span></span>  

<span data-ttu-id="960c7-142">Для демонстрации тестирование с помощью выполняющейся в памяти тестовых дублеров мы хотим написание несколько тестов для BlogService.</span><span class="sxs-lookup"><span data-stu-id="960c7-142">To demonstrate testing with in-memory test doubles we are going to be writing a couple of tests for a BlogService.</span></span> <span data-ttu-id="960c7-143">Служба, которая позволяет создавать новые веб-дневники (AddBlog) и возвращает все блоги упорядоченные по имени (GetAllBlogs).</span><span class="sxs-lookup"><span data-stu-id="960c7-143">The service is capable of creating new blogs (AddBlog) and returning all Blogs ordered by name (GetAllBlogs).</span></span> <span data-ttu-id="960c7-144">В дополнение к GetAllBlogs мы также предоставляем метод, который получит асинхронно все блоги, упорядоченные по имени (GetAllBlogsAsync).</span><span class="sxs-lookup"><span data-stu-id="960c7-144">In addition to GetAllBlogs, we’ve also provided a method that will asynchronously get all blogs ordered by name (GetAllBlogsAsync).</span></span>  

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

<span data-ttu-id="960c7-145"><a name="creating-the-in-memory-test-doubles"/> ## Создание теста в памяти удваивает</span><span class="sxs-lookup"><span data-stu-id="960c7-145"><a name="creating-the-in-memory-test-doubles"/> ## Creating the in-memory test doubles</span></span>  

<span data-ttu-id="960c7-146">Теперь, когда у нас есть реальной модели EF и службой, можно использовать его, пора создать тест в памяти, double, можно использовать для тестирования.</span><span class="sxs-lookup"><span data-stu-id="960c7-146">Now that we have the real EF model and the service that can use it, it's time to create the in-memory test double that we can use for testing.</span></span> <span data-ttu-id="960c7-147">Мы создали теста TestContext double для наш контекст.</span><span class="sxs-lookup"><span data-stu-id="960c7-147">We've created a TestContext test double for our context.</span></span> <span data-ttu-id="960c7-148">В тестовых дублеров, которые мы приступим к выберите поведение, мы хотим, чтобы обеспечить поддержку тестов мы собираемся выполнить.</span><span class="sxs-lookup"><span data-stu-id="960c7-148">In test doubles we get to choose the behavior we want in order to support the tests we are going to run.</span></span> <span data-ttu-id="960c7-149">В этом примере мы просто захватываются количество раз, когда в вызове метода SaveChanges, но можно включить любую логику, необходимым для проверки сценария, который вы тестируете.</span><span class="sxs-lookup"><span data-stu-id="960c7-149">In this example we're just capturing the number of times SaveChanges is called, but you can include whatever logic is needed to verify the scenario you are testing.</span></span>  

<span data-ttu-id="960c7-150">Мы также создали TestDbSet, который предоставляет реализацию в памяти DbSet.</span><span class="sxs-lookup"><span data-stu-id="960c7-150">We've also created a TestDbSet that provides an in-memory implementation of DbSet.</span></span> <span data-ttu-id="960c7-151">Мы предоставляем полный реализации для всех методов на DbSet (за исключением находит), но необходимо реализовать члены, которые будут использовать сценария тестирования.</span><span class="sxs-lookup"><span data-stu-id="960c7-151">We've provided a complete implemention for all the methods on DbSet (except for Find), but you only need to implement the members that your test scenario will use.</span></span>  

<span data-ttu-id="960c7-152">TestDbSet использует некоторые другие классы инфраструктуры, которые мы включили, чтобы убедиться, что обработку асинхронных запросов.</span><span class="sxs-lookup"><span data-stu-id="960c7-152">TestDbSet makes use of some other infrastructure classes that we've included to ensure that async queries can be processed.</span></span>  

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

### <a name="implementing-find"></a><span data-ttu-id="960c7-153">Реализация поиска</span><span class="sxs-lookup"><span data-stu-id="960c7-153">Implementing Find</span></span>  

<span data-ttu-id="960c7-154">Метод Find трудно реализовать в первоначальном виде.</span><span class="sxs-lookup"><span data-stu-id="960c7-154">The Find method is difficult to implement in a generic fashion.</span></span> <span data-ttu-id="960c7-155">Если необходимо протестировать код, который делает использование метода Find, проще всего создать тест DbSet для каждого из типов сущностей, которые должны поддерживать поиск.</span><span class="sxs-lookup"><span data-stu-id="960c7-155">If you need to test code that makes use of the Find method it is easiest to create a test DbSet for each of the entity types that need to support find.</span></span> <span data-ttu-id="960c7-156">После этого можно написать логику для поиска конкретного типа сущности, как показано ниже.</span><span class="sxs-lookup"><span data-stu-id="960c7-156">You can then write logic to find that particular type of entity, as shown below.</span></span>  

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

## <a name="writing-some-tests"></a><span data-ttu-id="960c7-157">Написание некоторых тестов</span><span class="sxs-lookup"><span data-stu-id="960c7-157">Writing some tests</span></span>  

<span data-ttu-id="960c7-158">Это все, что необходимо сделать, чтобы начать тестирование.</span><span class="sxs-lookup"><span data-stu-id="960c7-158">That’s all we need to do to start testing.</span></span> <span data-ttu-id="960c7-159">Следующий тест создает TestContext, а затем — это служба, исходя из этого контекста.</span><span class="sxs-lookup"><span data-stu-id="960c7-159">The following test creates a TestContext and then a service based on this context.</span></span> <span data-ttu-id="960c7-160">Служба затем используется для создания нового блога — с помощью метода AddBlog.</span><span class="sxs-lookup"><span data-stu-id="960c7-160">The service is then used to create a new blog – using the AddBlog method.</span></span> <span data-ttu-id="960c7-161">Наконец тест проверяет, что служба добавлен новый блог свойства блоги контекста и вызове метода SaveChanges в контексте.</span><span class="sxs-lookup"><span data-stu-id="960c7-161">Finally, the test verifies that the service added a new Blog to the context's Blogs property and called SaveChanges on the context.</span></span>  

<span data-ttu-id="960c7-162">Это всего лишь примером типа из вещей, которые можно проверить с помощью двойных теста в памяти, и можно настроить логику тестовых дублеров и проверку в соответствии с требованиями.</span><span class="sxs-lookup"><span data-stu-id="960c7-162">This is just an example of the types of things you can test with an in-memory test double and you can adjust the logic of the test doubles and the verification to meet your requirements.</span></span>  

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

<span data-ttu-id="960c7-163">Вот еще один пример теста — это время, который выполняет запрос.</span><span class="sxs-lookup"><span data-stu-id="960c7-163">Here is another example of a test - this time one that performs a query.</span></span> <span data-ttu-id="960c7-164">Тест начинается с создания контекста теста с данными в его блоге свойство — Обратите внимание, что данные не в алфавитном порядке.</span><span class="sxs-lookup"><span data-stu-id="960c7-164">The test starts by creating a test context with some data in its Blog property - note that the data is not in alphabetical order.</span></span> <span data-ttu-id="960c7-165">Затем можно создать BlogService, исходя из наших контекста теста и убедитесь, что данные, которые мы получаем от GetAllBlogs упорядочен по имени.</span><span class="sxs-lookup"><span data-stu-id="960c7-165">We can then create a BlogService based on our test context and ensure that the data we get back from GetAllBlogs is ordered by name.</span></span>  

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

<span data-ttu-id="960c7-166">Наконец, мы напишем еще один тест, который использует наших асинхронный метод, чтобы убедиться, что инфраструктура async, мы включили в [TestDbSet](#creating-the-in-memory-test-doubles) работает.</span><span class="sxs-lookup"><span data-stu-id="960c7-166">Finally, we'll write one more test that uses our async method to ensure that the async infrastructure we included in [TestDbSet](#creating-the-in-memory-test-doubles) is working.</span></span>  

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
