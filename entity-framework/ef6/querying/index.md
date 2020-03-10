---
title: Запросы и поиск сущностей — EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 65bb3db2-2226-44af-8864-caa575cf1b46
ms.openlocfilehash: 29a86817e250a2f53ecaa73e8fa4bf93452f0497
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/06/2020
ms.locfileid: "78412979"
---
# <a name="querying-and-finding-entities"></a>Запросы и поиск сущностей
В этом разделе рассматриваются различные способы запроса данных с помощью Entity Framework, включая LINQ и метод Find. Методы, представленные в этом разделе, также применимы к моделям, созданным с помощью Code First и конструктора EF.  

## <a name="finding-entities-using-a-query"></a>Поиск сущностей с помощью запроса  

DbSet и IDbSet реализуют IQueryable, поэтому их можно использовать как начальную точку для написания запроса LINQ к базе данных. Здесь мы не будем подробно рассматривать LINQ, но приведем несколько простых примеров:  

``` csharp
using (var context = new BloggingContext())
{
    // Query for all blogs with names starting with B
    var blogs = from b in context.Blogs
                   where b.Name.StartsWith("B")
                   select b;

    // Query for the Blog named ADO.NET Blog
    var blog = context.Blogs
                    .Where(b => b.Name == "ADO.NET Blog")
                    .FirstOrDefault();
}
```  

Обратите внимание, что DbSet и IDbSet всегда создают запросы к базе данных и всегда используют круговой путь к базе данных, даже если возвращаемые сущности уже присутствуют в контексте. Запрос выполняется в базе данных, если:  

- Он перечисляется с помощью инструкции **foreach** (C#) или **For Each** (Visual Basic).  
- Он перечисляется операцией коллекции, например [ToArray](https://msdn.microsoft.com/library/bb298736), [ToDictionary](https://msdn.microsoft.com/library/system.linq.enumerable.todictionary) или [ToList](https://msdn.microsoft.com/library/bb342261).  
- Во внешней части запроса заданы операторы LINQ, например [First](https://msdn.microsoft.com/library/bb291976) или [Any](https://msdn.microsoft.com/library/bb337697).  
- Вызываются следующие методы: метод расширения [Load](https://msdn.microsoft.com/library/system.data.entity.dbextensions.load) в DbSet, [DbEntityEntry.Reload](https://msdn.microsoft.com/library/system.data.entity.infrastructure.dbentityentry.reload.aspx) и Database.ExecuteSqlCommand.  

Когда из базы данных возвращаются результаты, объекты, отсутствующие в контексте, присоединяются к контексту. Если объект уже есть в контексте, возвращается существующий объект (текущие и исходные значения свойств объекта в записи **не** переписываются значениями из базы данных).  

Когда вы выполняете запрос, сущности, которые были добавлены в контекст, но еще не были сохранены в базе данных, не возвращаются в составе результирующего набора. Чтобы получить данные из контекста, см. раздел [Локальные данные](~/ef6/querying/local-data.md).  

Если запрос не возвращает строки из базы данных, результатом будет пустая коллекция, а не **NULL**.  

## <a name="finding-entities-using-primary-keys"></a>Поиск сущностей с помощью первичных ключей  

Метод Find в классе DbSet использует значение первичного ключа, чтобы найти сущность, отслеживаемую контекстом. Если сущность не найдена в контексте, запрос отправляется в базу данных для поиска сущности там. Если сущность не найдена в контексте или в базе данных, возвращается значение NULL.  

Метод Find имеет два важных отличия от запроса:  

- Круговой путь к базе данных будет использоваться только в том случае, если сущность с указанным ключом не найдена в контексте.  
- Метод Find возвращает сущности в состоянии "Добавлено". Это значит, что метод Find возвращает сущности, которые были добавлены в контекст, но еще не были сохранены в базе данных.  
### <a name="finding-an-entity-by-primary-key"></a>Поиск сущности по первичному ключу  

В коде ниже приведено несколько примеров использования метода Find.  

``` csharp
using (var context = new BloggingContext())
{
    // Will hit the database
    var blog = context.Blogs.Find(3);

    // Will return the same instance without hitting the database
    var blogAgain = context.Blogs.Find(3);

    context.Blogs.Add(new Blog { Id = -1 });

    // Will find the new blog even though it does not exist in the database
    var newBlog = context.Blogs.Find(-1);

    // Will find a User which has a string primary key
    var user = context.Users.Find("johndoe1987");
}
```  

### <a name="finding-an-entity-by-composite-primary-key"></a>Поиск сущности по составному первичному ключу  

Платформа Entity Framework позволяет сущностям иметь составные ключи, то есть ключи, состоящие из нескольких свойств. Например, вы можете создать сущность BlogSettings, которая представляет собой параметры пользователей для конкретного блога. Так как пользователю необходима только одна сущность BlogSettings для каждого блога, первичный ключ для BlogSettings может состоять из комбинации идентификатора блога и имени пользователя. Следующий код пытается найти BlogSettings по идентификатору = 3 и имени пользователя = johndoe1987:  

``` csharp  
using (var context = new BloggingContext())
{
    var settings = context.BlogSettings.Find(3, "johndoe1987");
}
```  

Если у вас есть составные ключи, вам нужно использовать ColumnAttribute или текучий API, чтобы указать порядок свойств составного ключа. В вызове метода Find эти значения ключа должны указываться в том же порядке.  
