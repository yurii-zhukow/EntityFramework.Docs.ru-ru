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
# <a name="testing-with-a-mocking-framework"></a>Тестирование с помощью инфраструктуры макетирования
> [!NOTE]
> **Только в EF6 и более поздних версиях**. Функции, API и другие возможности, описанные на этой странице, появились в Entity Framework 6. При использовании более ранней версии могут быть неприменимы некоторые или все сведения.  

При написании тестов для приложения часто желательно избежать попадания в базу данных.  Entity Framework позволяет добиться этого, создавая контекст с последующим поведением, определенным в тестах. Это позволяет использовать данные в памяти.  

## <a name="options-for-creating-test-doubles"></a>Параметры для создания тестовых значений типа Double  

Существует два разных подхода, которые можно использовать для создания версии контекста в памяти.  

- **Создание собственных тестовых двойных** значений. Этот подход включает написание собственной реализации контекста и дбсетс в памяти. Это дает множество контроля над поведением классов, но может задействовать написание и владельца разумного объема кода.  
- **Использование инфраструктуры макетирования для создания тестовых значений Double** . с помощью инфраструктуры макетирования (например, MOQ) можно реализовать в памяти реализации контекста и динамически создаваемые во время выполнения.  

В этой статье рассматривается использование инфраструктуры макетирования. Для создания собственных тестовых двойной точности см. раздел [тестирование с помощью пользовательских значений Double](xref:ef6/fundamentals/testing/writing-test-doubles).  

Чтобы продемонстрировать использование EF с инфраструктурой макетирования, мы будем использовать MOQ. Самый простой способ получить MOQ — установить [пакет MOQ из NuGet](https://nuget.org/packages/Moq/).  

## <a name="testing-with-pre-ef6-versions"></a>Тестирование с использованием предварительно EF6 версий  

Сценарий, приведенный в этой статье, зависит от некоторых изменений, внесенных в DbSet в EF6. Тестирование с помощью EF5 и более ранних версий см. в разделе [тестирование с имитацией контекста](https://romiller.com/2012/02/14/testing-with-a-fake-dbcontext/).  

## <a name="limitations-of-ef-in-memory-test-doubles"></a>Ограничения для теста EF в памяти, Double  

Числа двойной проверки в памяти могут быть хорошим способом обеспечения уровня модульного тестирования для битов приложения, использующих EF. Однако при этом вы используете LINQ to Objects для выполнения запросов к данным в памяти. Это может привести к различному поведению, чем использование поставщика LINQ (LINQ to Entities) EF для преобразования запросов в SQL, выполняемых в базе данных.  

Одним из примеров такого различия является загрузка связанных данных. Если вы создадите серию блогов, в каждой из которых есть связанные записи, то при использовании данных в памяти связанные записи всегда будут загружаться для каждого блога. Однако при работе с базой данных данные будут загружаться только при использовании метода Include.  

По этой причине рекомендуется всегда включать некоторый уровень сквозного тестирования (в дополнение к модульным тестам), чтобы обеспечить правильную работу приложения в базе данных.  

## <a name="following-along-with-this-article"></a>Далее вместе с этой статьей  

В этой статье приведены полные листинги кода, которые можно скопировать в Visual Studio, если хотите. Проще всего создать **проект модульного теста** , а для выполнения разделов, использующих async, необходимо выбрать **.NET Framework 4,5** .  

## <a name="the-ef-model"></a>Модель EF  

Служба, которую мы собираемся тестировать, использует модель EF, состоящие из BloggingContext, а также классов блога и POST. Этот код может быть создан конструктором EF или моделью Code First.  

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

### <a name="virtual-dbset-properties-with-ef-designer"></a>Свойства виртуальных DbSet с помощью конструктора EF  

Обратите внимание, что свойства DbSet в контексте помечаются как виртуальные. Это позволит инфраструктуре макетирования получить производный от нашего контекста и переопределить эти свойства с помощью макетной реализации.  

При использовании Code First можно изменить классы напрямую. При использовании конструктора EF необходимо изменить шаблон T4, который создает контекст. Откройте \<model_name\> . Context.tt файл, вложенный в EDMX-файл, найдите следующий фрагмент кода и добавьте ключевое слово virtual, как показано ниже.  

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

## <a name="testing-non-query-scenarios"></a>Тестирование сценариев, не использующих запросы  

Это все, что нужно сделать, чтобы начать тестирование методов, не относящихся к запросам. В следующем тесте для создания контекста используется MOQ. Затем он создает DbSet \<Blog\> и подсоединяет его, чтобы он возвращался из свойства blogs в контексте. Далее контекст используется для создания нового BlogService, который затем используется для создания нового блога — с помощью метода Аддблог. Наконец, тест подтверждает, что служба добавила новый блог и вызвала команду SaveChanges в контексте.  

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

## <a name="testing-query-scenarios"></a>Тестирование сценариев запросов  

Чтобы иметь возможность выполнять запросы к DbSet тесту Double, необходимо настроить реализацию IQueryable. Первым шагом является создание некоторых данных в памяти — мы используем список \<Blog\> . Далее мы создаем контекст, а \<Blog\> затем DBSet подсоединить реализацию IQueryable для DBSet — они просто делегируются поставщику LINQ to Objects, который работает со списком \<T\> .  

Затем можно создать BlogService на основе наших тестовых значений типа Double и убедиться, что данные, получаемые из Жеталлблогс, упорядочены по именам.  

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

### <a name="testing-with-async-queries"></a>Тестирование с помощью асинхронных запросов

В Entity Framework 6 появился набор методов расширения, которые можно использовать для асинхронного выполнения запроса. Примеры этих методов: Толистасинк, Фирстасинк, Фореачасинк и т. д.  

Поскольку запросы Entity Framework используют LINQ, методы расширения определяются в IQueryable и IEnumerable. Однако, поскольку они предназначены только для использования с Entity Framework может возникнуть следующая ошибка при попытке использовать их в запросе LINQ, который не является Entity Frameworkным запросом:

> Источник IQueryable не реализует Идбасинценумерабле {0} . Для Entity Framework асинхронных операций можно использовать только те источники, которые реализуют Идбасинценумерабле. Дополнительные сведения см. в разделе [http://go.microsoft.com/fwlink/?LinkId=287068](https://go.microsoft.com/fwlink/?LinkId=287068) .  

В то время как асинхронные методы поддерживаются только при выполнении запроса EF, их можно использовать в модульном тесте при выполнении теста в памяти с двойной точностью DbSet.  

Чтобы использовать асинхронные методы, необходимо создать Дбасинккуерипровидер в памяти для обработки асинхронного запроса. В то же самое, что можно было бы настроить поставщик запросов с помощью MOQ, гораздо проще создать тестовую реализацию двойной реализации в коде. Код для этой реализации выглядит следующим образом:  

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

Теперь, когда у нас есть поставщик асинхронных запросов, можно написать модульный тест для нашего нового метода Жеталлблогсасинк.  

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
