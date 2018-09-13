---
title: Тестирование с помощью собственных тестовых дублеров - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 16a8b7c0-2d23-47f4-9cc0-e2eb2e738ca3
ms.openlocfilehash: 2158dc73585c2720e7293096b0478c73edf522d9
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/13/2018
ms.locfileid: "45490913"
---
# <a name="testing-with-your-own-test-doubles"></a>Тестирование с помощью собственных тестовых дублеров
> [!NOTE]
> **Только в EF6 и более поздних версиях**. Функции, API и другие возможности, описанные на этой странице, появились в Entity Framework 6. При использовании более ранней версии могут быть неприменимы некоторые или все сведения.  

При написании тестов для приложения часто желательно избежать обращения к базе данных.  Платформа Entity Framework позволяет добиться этого путем создания контекста — с поведение, определяемое тестов —, в котором используется данных в памяти.  

## <a name="options-for-creating-test-doubles"></a>Параметры для создания тестовых дублеров  

Существует два различных подхода, которые могут использоваться для создания в памяти версии контекста.  

- **Создать свои собственные тестовые дублеры** — этот подход подразумевает написание собственной реализации в памяти DbSets и данные контекста. Это обеспечивает большую контроль над тем, как ведут себя классы, но может включать в себя написание и владельцев разумный объем кода.  
- **Использовать платформа имитации для создания тестовых дублеров** — использование платформы имитированной (например, Moq) может иметь реализаций в памяти из вас контекст и наборов, созданных динамически во время выполнения для вас.  

В этой статье будет иметь дело с созданием собственных тестов double. Сведения об использовании платформа имитации см. в разделе [тестирование на платформе, макетирование](mocking.md).  

## <a name="testing-with-pre-ef6-versions"></a>Тестирование с помощью EF6 предварительной версии  

Код, показанный в этой статье совместим с EF6. Тестирование с помощью EF5 и более ранней версии см. в разделе [тестирования с контекстом имитировать](http://romiller.com/2012/02/14/testing-with-a-fake-dbcontext/).  

## <a name="limitations-of-ef-in-memory-test-doubles"></a>Ограничения EF in-Memory тестовых дублеров  

В памяти тестовых дублеров может быть хорошим средством предоставления уровня покрытия битов приложения, использующие EF модульного теста. Тем не менее при этом при использовании LINQ to Objects для выполнения запросов к данным в памяти. Это может привести другое поведение, чем при использовании поставщика платформы EF LINQ (LINQ to Entities) для преобразования запросов в SQL, которая выполняется в базе данных.  

Примером такой разницы загрузка связанных данных. Если создать серию блогов, в которых тематических записях блога, то при использовании данных в памяти связанных сообщений всегда будут загружаться для каждого блога. Тем не менее при выполнении в базе данных будет загружаться только при использовании метода Include.  

По этой причине рекомендуется всегда включать некоторый уровень end-to-end тестирования (в дополнение к модульные тесты), чтобы ваше приложение работает правильно для базы данных.  

## <a name="following-along-with-this-article"></a>Следуя указаниям в этой статье  

В этой статье приводятся полные Листинги кода, которые можно скопировать в Visual Studio для выполнения этой процедуры при необходимости. Проще всего создать **проект модульного теста** и требуется целевой объект **.NET Framework 4.5** для выполнения в разделах, которые используют асинхронный.  

## <a name="creating-a-context-interface"></a>Создание интерфейса контекста  

Мы собираемся тестирования службы, который использует EF модели. Чтобы иметь возможность заменять наш контекст EF с версией в памяти для тестирования, мы определим интерфейс что наш контекст EF (и double в памяти) будет imeplement.  

Службы, с которой мы хотим проверить запрос и изменение данных с помощью свойства DbSet наш контекст и также вызывать метод SaveChanges для внесения изменений в базу данных. Поэтому мы добавим эти члены в интерфейсе.  

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

## <a name="the-ef-model"></a>Модель EF  

Службы, мы собираемся тестирования использует EF модели BloggingContext и классы блог и Post. Этот код был создан в конструкторе EF или быть модели Code First.  

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

### <a name="implementing-the-context-interface-with-the-ef-designer"></a>Реализация интерфейса контекста с помощью конструктора EF  

Обратите внимание на то, что наш контекст реализует интерфейс IBloggingContext.  

Если вы используете Code First можно изменить текущий контекст непосредственно реализуют интерфейс. Если вы используете конструктор EF необходимо изменить шаблон T4, который создает контекст. Откройте \<model_name\>. Context.tt файл, который вложен в вы EDMX-файла, найдите следующий фрагмент кода и добавьте в интерфейсе, как показано.  

``` csharp  
<#=Accessibility.ForType(container)#> partial class <#=code.Escape(container)#> : DbContext, IBloggingContext
```  

## <a name="service-to-be-tested"></a>Служба проверяемый  

Для демонстрации тестирование с помощью выполняющейся в памяти тестовых дублеров мы хотим написание несколько тестов для BlogService. Служба, которая позволяет создавать новые веб-дневники (AddBlog) и возвращает все блоги упорядоченные по имени (GetAllBlogs). В дополнение к GetAllBlogs мы также предоставляем метод, который получит асинхронно все блоги, упорядоченные по имени (GetAllBlogsAsync).  

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

<a name="creating-the-in-memory-test-doubles"/> ## Создание теста в памяти удваивает  

Теперь, когда у нас есть реальной модели EF и службой, можно использовать его, пора создать тест в памяти, double, можно использовать для тестирования. Мы создали теста TestContext double для наш контекст. В тестовых дублеров, которые мы приступим к выберите поведение, мы хотим, чтобы обеспечить поддержку тестов мы собираемся выполнить. В этом примере мы просто захватываются количество раз, когда в вызове метода SaveChanges, но можно включить любую логику, необходимым для проверки сценария, который вы тестируете.  

Мы также создали TestDbSet, который предоставляет реализацию в памяти DbSet. Мы предоставляем полный реализации для всех методов на DbSet (за исключением находит), но необходимо реализовать члены, которые будут использовать сценария тестирования.  

TestDbSet использует некоторые другие классы инфраструктуры, которые мы включили, чтобы убедиться, что обработку асинхронных запросов.  

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

### <a name="implementing-find"></a>Реализация поиска  

Метод Find трудно реализовать в первоначальном виде. Если необходимо протестировать код, который делает использование метода Find, проще всего создать тест DbSet для каждого из типов сущностей, которые должны поддерживать поиск. После этого можно написать логику для поиска конкретного типа сущности, как показано ниже.  

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

## <a name="writing-some-tests"></a>Написание некоторых тестов  

Это все, что необходимо сделать, чтобы начать тестирование. Следующий тест создает TestContext, а затем — это служба, исходя из этого контекста. Служба затем используется для создания нового блога — с помощью метода AddBlog. Наконец тест проверяет, что служба добавлен новый блог свойства блоги контекста и вызове метода SaveChanges в контексте.  

Это всего лишь примером типа из вещей, которые можно проверить с помощью двойных теста в памяти, и можно настроить логику тестовых дублеров и проверку в соответствии с требованиями.  

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

Вот еще один пример теста — это время, который выполняет запрос. Тест начинается с создания контекста теста с данными в его блоге свойство — Обратите внимание, что данные не в алфавитном порядке. Затем можно создать BlogService, исходя из наших контекста теста и убедитесь, что данные, которые мы получаем от GetAllBlogs упорядочен по имени.  

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

Наконец, мы напишем еще один тест, который использует наших асинхронный метод, чтобы убедиться, что инфраструктура async, мы включили в [TestDbSet](#creating-the-in-memory-test-doubles) работает.  

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
