---
title: Тестирование с помощью собственных тестовых двойных значений — EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 16a8b7c0-2d23-47f4-9cc0-e2eb2e738ca3
ms.openlocfilehash: 3d8933fb5e17f8c01f3971495a1fcdb5b8cfab57
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/06/2020
ms.locfileid: "78413891"
---
# <a name="testing-with-your-own-test-doubles"></a>Тестирование с помощью пользовательских двойной проверки
> [!NOTE]
> **Только в EF6 и более поздних версиях**. Функции, API и другие возможности, описанные на этой странице, появились в Entity Framework 6. При использовании более ранней версии могут быть неприменимы некоторые или все сведения.  

При написании тестов для приложения часто желательно избежать попадания в базу данных.  Entity Framework позволяет добиться этого, создавая контекст с последующим поведением, определенным в тестах. Это позволяет использовать данные в памяти.  

## <a name="options-for-creating-test-doubles"></a>Параметры для создания тестовых значений типа Double  

Существует два разных подхода, которые можно использовать для создания версии контекста в памяти.  

- **Создание собственных тестовых двойных** значений. Этот подход включает написание собственной реализации контекста и дбсетс в памяти. Это дает множество контроля над поведением классов, но может задействовать написание и владельца разумного объема кода.  
- **Использование инфраструктуры макетирования для создания тестовых значений Double** . с помощью инфраструктуры макетирования (например, MOQ) можно реализовать в памяти реализации контекста и наборов, создаваемых динамически во время выполнения.  

В этой статье рассматривается создание собственного тестового типа Double. Сведения об использовании инфраструктуры макетирования см. в разделе [тестирование с использованием инфраструктуры макетирования](mocking.md).  

## <a name="testing-with-pre-ef6-versions"></a>Тестирование с использованием предварительно EF6 версий  

Код, приведенный в этой статье, совместим с EF6. Тестирование с помощью EF5 и более ранних версий см. в разделе [тестирование с имитацией контекста](https://romiller.com/2012/02/14/testing-with-a-fake-dbcontext/).  

## <a name="limitations-of-ef-in-memory-test-doubles"></a>Ограничения для теста EF в памяти, Double  

Числа двойной проверки в памяти могут быть хорошим способом обеспечения уровня модульного тестирования для битов приложения, использующих EF. Однако при этом вы используете LINQ to Objects для выполнения запросов к данным в памяти. Это может привести к различному поведению, чем использование поставщика LINQ (LINQ to Entities) EF для преобразования запросов в SQL, выполняемых в базе данных.  

Одним из примеров такого различия является загрузка связанных данных. Если вы создадите серию блогов, в каждой из которых есть связанные записи, то при использовании данных в памяти связанные записи всегда будут загружаться для каждого блога. Однако при работе с базой данных данные будут загружаться только при использовании метода Include.  

По этой причине рекомендуется всегда включать некоторый уровень сквозного тестирования (в дополнение к модульным тестам), чтобы обеспечить правильную работу приложения в базе данных.  

## <a name="following-along-with-this-article"></a>Далее вместе с этой статьей  

В этой статье приведены полные листинги кода, которые можно скопировать в Visual Studio, если хотите. Проще всего создать **проект модульного теста** , а для выполнения разделов, использующих async, необходимо выбрать **.NET Framework 4,5** .  

## <a name="creating-a-context-interface"></a>Создание интерфейса контекста  

Мы будем рассматривать тестирование службы, использующей модель EF. Чтобы иметь возможность заменить наш контекст EF на версию в памяти для тестирования, мы определим интерфейс, реализуемый нашим контекстом EF (и его в памяти Double).

Служба, которую мы собираемся тестировать, будет запрашивать и изменять данные с помощью свойств DbSet в нашем контексте, а также вызывать метод SaveChanges для отправки изменений в базу данных. Так что мы будем включать эти члены в интерфейс.  

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

Служба, которую мы собираемся тестировать, использует модель EF, состоящие из BloggingContext, а также классов блога и POST. Этот код может быть создан конструктором EF или моделью Code First.  

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

Обратите внимание, что наш контекст реализует интерфейс Иблоггингконтекст.  

При использовании Code First можно изменить контекст непосредственно для реализации интерфейса. При использовании конструктора EF необходимо изменить шаблон T4, который создает контекст. Откройте\>model_name \<. Context.tt файл, вложенный в EDMX-файл, найдите следующий фрагмент кода и добавьте его в интерфейс, как показано ниже.  

``` csharp  
<#=Accessibility.ForType(container)#> partial class <#=code.Escape(container)#> : DbContext, IBloggingContext
```  

## <a name="service-to-be-tested"></a>Тестируемая служба  

Чтобы продемонстрировать тестирование с двойными тестами в памяти, мы будем писать несколько тестов для BlogService. Служба может создавать новые блоги (Аддблог) и возвращать все блоги, упорядоченные по имени (Жеталлблогс). Кроме Жеталлблогс, мы также предоставили метод, который будет асинхронно получать все блоги, упорядоченные по имени (Жеталлблогсасинк).  

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

## <a name="creating-the-in-memory-test-doubles"></a>Создание двойных тестов в памяти  

Теперь, когда у нас есть реальная модель EF и служба, которая может использовать ее, пришло время создать тестовое в памяти значение Double, которое можно использовать для тестирования. Для нашего контекста мы создали тест TestContext Double. В тестах Double мы получаем выбор поведения для поддержки тестов, которые мы будем выполнять. В этом примере мы просто записываем количество вызовов SaveChanges, но можно включить любую логику, необходимую для проверки тестируемого сценария.  

Мы также создали Тестдбсет, который предоставляет реализацию DbSet в памяти. Мы предоставили полную реализацию всех методов в DbSet (за исключением Find), но достаточно реализовать только те элементы, которые будут использоваться сценарием тестирования.  

Тестдбсет использует некоторые другие классы инфраструктуры, которые мы включили для обеспечения возможности обработки асинхронных запросов.  

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

Метод Find трудно реализовать обычным способом. Если необходимо протестировать код, который использует метод Find, проще всего создать тестовый DbSet для каждого из типов сущностей, которые должны поддерживать поиск. Затем можно написать логику для поиска конкретного типа сущности, как показано ниже.  

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

Это все, что нужно сделать для начала тестирования. В следующем тесте создается TestContext, а затем служба на основе этого контекста. Затем служба используется для создания нового блога — с помощью метода Аддблог. Наконец, тест подтверждает, что служба добавила новый блог к свойству блогов в контексте и вызвал команду SaveChanges в контексте.  

Это лишь пример типов объектов, которые можно протестировать с помощью двойного теста в памяти, и можно настроить логику тестовых значений Double и проверку в соответствии с вашими требованиями.  

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

Ниже приведен еще один пример тестового времени, который выполняет запрос. Тест начинается с создания контекста теста с некоторыми данными в своем свойстве блога. Обратите внимание, что данные находятся не в алфавитном порядке. Затем можно создать BlogService на основе нашего контекста теста и убедиться, что данные, получаемые из Жеталлблогс, упорядочены по имени.  

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

Наконец, мы напишем еще один тест, использующий наш асинхронный метод, чтобы убедиться, что асинхронная инфраструктура, включенная в [тестдбсет](#creating-the-in-memory-test-doubles) , работает.  

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
