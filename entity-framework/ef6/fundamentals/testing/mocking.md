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
# <a name="testing-with-a-mocking-framework"></a>Тестирование с помощью макетирования
> [!NOTE]
> **Только в EF6 и более поздних версиях**. Функции, API и другие возможности, описанные на этой странице, появились в Entity Framework 6. При использовании более ранней версии могут быть неприменимы некоторые или все сведения.  

При написании тестов для приложения часто желательно избежать обращения к базе данных.  Платформа Entity Framework позволяет добиться этого путем создания контекста — с поведение, определяемое тестов —, в котором используется данных в памяти.  

## <a name="options-for-creating-test-doubles"></a>Параметры для создания тестовых дублеров  

Существует два различных подхода, которые могут использоваться для создания в памяти версии контекста.  

- **Создать свои собственные тестовые дублеры** — этот подход подразумевает написание собственной реализации в памяти DbSets и данные контекста. Это обеспечивает большую контроль над тем, как ведут себя классы, но может включать в себя написание и владельцев разумный объем кода.  
- **Использовать платформа имитации для создания тестовых дублеров** — использование платформы имитированной (например, Moq) может иметь реализации в памяти наборы, созданные динамически во время выполнения для вас и данные контекста.  

В этой статье будет работать при использовании платформа имитации. Для создания собственных тестовых дублеров см. в разделе [тестирование с помощью вашей собственной тестирования двойной точности](writing-test-doubles.md).  

Чтобы продемонстрировать использование EF с макетирования мы собираемся использовать Moq. Самый простой способ получить Moq является установка [Moq пакет из NuGet](http://nuget.org/packages/Moq/).  

## <a name="testing-with-pre-ef6-versions"></a>Тестирование с помощью EF6 предварительной версии  

Сценарии, приведенном в этой статье, зависит от некоторых изменений, внесенных в DbSet в EF6. Тестирование с помощью EF5 и более ранней версии см. в разделе [тестирования с контекстом имитировать](http://romiller.com/2012/02/14/testing-with-a-fake-dbcontext/).  

## <a name="limitations-of-ef-in-memory-test-doubles"></a>Ограничения EF in-Memory тестовых дублеров  

В памяти тестовых дублеров может быть хорошим средством предоставления уровня покрытия битов приложения, использующие EF модульного теста. Тем не менее при этом при использовании LINQ to Objects для выполнения запросов к данным в памяти. Это может привести другое поведение, чем при использовании поставщика платформы EF LINQ (LINQ to Entities) для преобразования запросов в SQL, которая выполняется в базе данных.  

Примером такой разницы загрузка связанных данных. Если создать серию блогов, в которых тематических записях блога, то при использовании данных в памяти связанных сообщений всегда будут загружаться для каждого блога. Тем не менее при выполнении в базе данных будет загружаться только при использовании метода Include.  

По этой причине рекомендуется всегда включать некоторый уровень end-to-end тестирования (в дополнение к модульные тесты), чтобы ваше приложение работает правильно для базы данных.  

## <a name="following-along-with-this-article"></a>Следуя указаниям в этой статье  

В этой статье приводятся полные Листинги кода, которые можно скопировать в Visual Studio для выполнения этой процедуры при необходимости. Проще всего создать **проект модульного теста** и требуется целевой объект **.NET Framework 4.5** для выполнения в разделах, которые используют асинхронный.  

## <a name="the-ef-model"></a>Модель EF  

Службы, мы собираемся тестирования использует EF модели BloggingContext и классы блог и Post. Этот код был создан в конструкторе EF или быть модели Code First.  

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

### <a name="virtual-dbset-properties-with-ef-designer"></a>Виртуальные свойства DbSet, с помощью конструктора EF  

Обратите внимание на то, что свойства DbSet в контексте помечаются как виртуальные. Таким образом, платформа имитации для наследования от наш контекст и переопределение этих свойств с реализацией макеты.  

Если вы используете Code First, то ваши классы можно изменить напрямую. Если вы используете конструктор EF необходимо изменить шаблон T4, который создает контекст. Откройте \<model_name\>. Context.tt файл, который вложен в вы EDMX-файла, найдите следующий фрагмент кода и добавьте в ключевое слово virtual, как показано.  

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

## <a name="testing-non-query-scenarios"></a>Сценарии тестирования не связанным с запросом  

Это все, что необходимо сделать, чтобы начать тестирование методы без запросов. Следующий тест использует Moq для создания контекста. Затем он создает коллекцию DbSet\<блог\> и настраивающий возвращается из свойства контекста блоги. Затем контекст используется для создания новых BlogService, который затем используется для создания нового блога — с помощью метода AddBlog. Наконец тест проверяет, что служба добавлен новый блог и вызове метода SaveChanges в контексте.  

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

## <a name="testing-query-scenarios"></a>Сценарии тестирования запроса  

Чтобы иметь возможность выполнять запросы к нашей DbSet тестовый дублер нам нужно настроить реализации IQueryable. Первым шагом является создание некоторых данных в памяти — мы используем список\<блог\>. Теперь создадим контекст и DBSet\<блог\> затем подключите реализации IQueryable DbSet — они просто делегировании в поставщике LINQ to Objects, работающий со списком\<T\>.  

Затем можно создать BlogService, в зависимости от наших тестовых дублеров и убедитесь, что данные, которые мы получаем от GetAllBlogs упорядочен по имени.  

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

Entity Framework 6 был представлен набор методов расширения, которые могут использоваться для асинхронного выполнения запроса. Эти методы примеры ToListAsync, FirstAsync, ForEachAsync и т. д.  

Так как запросы Entity Framework использовать LINQ, методы расширения определяются в IQueryable и IEnumerable. Тем не менее так как они предназначены только для использования с Entity Framework может появиться следующая ошибка при попытке использовать их в запрос LINQ, который не запрос Entity Framework:

> Источник IQueryable не реализует IDbAsyncEnumerable{0}. Можно использовать только те источники, которые реализуют IDbAsyncEnumerable для асинхронных операций Entity Framework. Дополнительные сведения см. [ http://go.microsoft.com/fwlink/?LinkId=287068 ](https://go.microsoft.com/fwlink/?LinkId=287068).  

Пока асинхронные методы поддерживаются только в том случае, при запуске для запроса на EF, можно использовать их выполнения с в памяти тестирования двойной из DbSet в модульном тесте.  

Чтобы использовать асинхронные методы, нам нужно создать DbAsyncQueryProvider в памяти для обработки асинхронного запроса. Хотя можно было бы настроить поставщик запросов, с помощью Moq, гораздо проще создать реализацию double теста в коде. Ниже приведен код для этой реализации:  

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

Теперь, когда у нас есть поставщик асинхронный запрос можно написать модульный тест для нашего нового метода GetAllBlogsAsync.  

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
