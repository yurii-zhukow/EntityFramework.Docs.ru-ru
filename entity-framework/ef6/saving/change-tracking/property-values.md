---
title: Работа со значениями свойств — EF6
description: Работа со значениями свойств в Entity Framework 6
author: divega
ms.date: 10/23/2016
uid: ef6/saving/change-tracking/property-values
ms.openlocfilehash: 30c8e7dbd59f0eb3ec15c0f57f022afd90fd80f8
ms.sourcegitcommit: abda0872f86eefeca191a9a11bfca976bc14468b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/14/2020
ms.locfileid: "90073760"
---
# <a name="working-with-property-values"></a>Работа со значениями свойств
Для большей части Entity Framework будет отслеживать состояние, исходные значения и текущие значения свойств экземпляров сущности. Однако могут возникнуть некоторые случаи, например сценарии с отключением, в которых необходимо просмотреть сведения о свойствах EF и управлять ими. Методы, представленные в этом разделе, также применимы к моделям, созданным с помощью Code First и конструктора EF.  

Entity Framework отслеживает два значения для каждого свойства отслеживающей сущности. Текущее значение равно, так как имя указывает текущее значение свойства в сущности. Исходное значение — это значение, которое имело свойство при запросе сущности из базы данных или присоединено к контексту.  

Существуют два общих механизма работы со значениями свойств.  

- Значение одного свойства можно получить строго типизированным способом с помощью метода Property.  
- Значения для всех свойств сущности могут быть считаны в объект DbPropertyValues. Затем DbPropertyValues выступает в качестве объекта, подобного словарю, чтобы разрешить чтение и установку значений свойств. Значения в объекте DbPropertyValues могут быть заданы из значений другого объекта DbPropertyValues или из значений в каком-либо другом объекте, например в другой копии сущности или в простом объекте передачи данных (DTO).  

В следующих разделах приведены примеры использования обоих описанных выше механизмов.  

## <a name="getting-and-setting-the-current-or-original-value-of-an-individual-property"></a>Получение и установка текущего или исходного значения отдельного свойства  

В приведенном ниже примере показано, как можно прочитать текущее значение свойства, а затем задать новое значение.  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Find(3);

    // Read the current value of the Name property
    string currentName1 = context.Entry(blog).Property(u => u.Name).CurrentValue;

    // Set the Name property to a new value
    context.Entry(blog).Property(u => u.Name).CurrentValue = "My Fancy Blog";

    // Read the current value of the Name property using a string for the property name
    object currentName2 = context.Entry(blog).Property("Name").CurrentValue;

    // Set the Name property to a new value using a string for the property name
    context.Entry(blog).Property("Name").CurrentValue = "My Boring Blog";
}
```  

Используйте свойство OriginalValue вместо свойства Куррентвалуе для чтения или установки исходного значения.  

Обратите внимание, что возвращаемое значение вводится как "Object", если строка используется для указания имени свойства. С другой стороны, возвращаемое значение является строго типизированным, если используется лямбда-выражение.  

Если задать значение свойства Like, свойство будет помечено как измененное, только если новое значение отличается от старого значения.  

Если значение свойства задано таким образом, это изменение определяется автоматически, даже если Аутодетектчанжес выключен.  

## <a name="getting-and-setting-the-current-value-of-an-unmapped-property"></a>Получение и установка текущего значения несопоставленного свойства  

Также можно прочитать текущее значение свойства, которое не сопоставлено с базой данных. Примером несопоставленного свойства может быть свойство Рсслинк в блоге. Это значение может быть вычислено на основе Блогид и, следовательно, не должно храниться в базе данных. Пример:  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Find(1);
    // Read the current value of an unmapped property
    var rssLink = context.Entry(blog).Property(p => p.RssLink).CurrentValue;

    // Use a string to specify the property name
    var rssLinkAgain = context.Entry(blog).Property("RssLink").CurrentValue;
}
```  

Текущее значение также может быть задано, если свойство предоставляет метод задания.  

Чтение значений несопоставленных свойств полезно при выполнении Entity Framework проверки несопоставленных свойств. По той же причине текущие значения можно считывать и задавать для свойств сущностей, которые в настоящее время не отслеживается контекстом. Пример:  

``` csharp
using (var context = new BloggingContext())
{
    // Create an entity that is not being tracked
    var blog = new Blog { Name = "ADO.NET Blog" };

    // Read and set the current value of Name as before
    var currentName1 = context.Entry(blog).Property(u => u.Name).CurrentValue;
    context.Entry(blog).Property(u => u.Name).CurrentValue = "My Fancy Blog";
    var currentName2 = context.Entry(blog).Property("Name").CurrentValue;
    context.Entry(blog).Property("Name").CurrentValue = "My Boring Blog";
}
```  

Обратите внимание, что исходные значения недоступны для несопоставленных свойств или для свойств сущностей, которые не отслеживается контекстом.  

## <a name="checking-whether-a-property-is-marked-as-modified"></a>Проверка того, помечено ли свойство как измененное  

В приведенном ниже примере показано, как проверить, помечено ли отдельное свойство как измененное.  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Find(1);

    var nameIsModified1 = context.Entry(blog).Property(u => u.Name).IsModified;

    // Use a string for the property name
    var nameIsModified2 = context.Entry(blog).Property("Name").IsModified;
}
```  

Значения измененных свойств отправляются в виде обновлений в базу данных при вызове метода SaveChanges.  

##  <a name="marking-a-property-as-modified"></a>Пометка свойства как измененного  

В приведенном ниже примере показано, как принудительно пометить отдельное свойство как измененное.  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Find(1);

    context.Entry(blog).Property(u => u.Name).IsModified = true;

    // Use a string for the property name
    context.Entry(blog).Property("Name").IsModified = true;
}
```  

Пометка свойства как измененного приводит к тому, что обновление будет отправлено в базу данных для свойства при вызове SaveChanges, даже если текущее значение свойства совпадает с исходным значением.  

Сейчас невозможно сбросить отдельное свойство, чтобы оно не было изменено после того, как оно было помечено как измененное. Это то, что мы планируем поддерживать в будущем выпуске.  

## <a name="reading-current-original-and-database-values-for-all-properties-of-an-entity"></a>Чтение текущих, исходных и значений баз данных для всех свойств сущности  

В приведенном ниже примере показано, как считывать текущие значения, исходные значения и значения, фактически находящиеся в базе данных для всех сопоставленных свойств сущности.  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Find(1);

    // Make a modification to Name in the tracked entity
    blog.Name = "My Cool Blog";

    // Make a modification to Name in the database
    context.Database.SqlCommand("update dbo.Blogs set Name = 'My Boring Blog' where Id = 1");

    // Print out current, original, and database values
    Console.WriteLine("Current values:");
    PrintValues(context.Entry(blog).CurrentValues);

    Console.WriteLine("\nOriginal values:");
    PrintValues(context.Entry(blog).OriginalValues);

    Console.WriteLine("\nDatabase values:");
    PrintValues(context.Entry(blog).GetDatabaseValues());
}

public static void PrintValues(DbPropertyValues values)
{
    foreach (var propertyName in values.PropertyNames)
    {
        Console.WriteLine("Property {0} has value {1}",
                          propertyName, values[propertyName]);
    }
}
```  

Текущие значения — это значения, которые в настоящее время содержатся в свойствах сущности. Исходные значения — это значения, которые были считаны из базы данных при запросе сущности. Значения базы данных — это значения, которые в настоящее время хранятся в базе данных. Получение значений базы данных полезно, когда значения в базе данных могли измениться с момента запроса сущности, например, когда одновременное изменение базы данных выполнено другим пользователем.  

## <a name="setting-current-or-original-values-from-another-object"></a>Установка текущих или исходных значений из другого объекта  

Текущие или исходные значения отслеживающей сущности могут быть обновлены путем копирования значений из другого объекта. Пример:  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Find(1);
    var coolBlog = new Blog { Id = 1, Name = "My Cool Blog" };
    var boringBlog = new BlogDto { Id = 1, Name = "My Boring Blog" };

    // Change the current and original values by copying the values from other objects
    var entry = context.Entry(blog);
    entry.CurrentValues.SetValues(coolBlog);
    entry.OriginalValues.SetValues(boringBlog);

    // Print out current and original values
    Console.WriteLine("Current values:");
    PrintValues(entry.CurrentValues);

    Console.WriteLine("\nOriginal values:");
    PrintValues(entry.OriginalValues);
}

public class BlogDto
{
    public int Id { get; set; }
    public string Name { get; set; }
}
```  

При выполнении приведенного выше кода выводится следующее:  

```console
Current values:
Property Id has value 1
Property Name has value My Cool Blog

Original values:
Property Id has value 1
Property Name has value My Boring Blog
```  

Этот метод иногда используется при обновлении сущности со значениями, полученными от вызова службы или клиентом в n-уровневых приложении. Обратите внимание, что используемый объект не должен быть того же типа, что и сущность, пока он имеет свойства, имена которых совпадают с именами сущности. В приведенном выше примере для обновления исходных значений используется экземпляр Блогдто.  

Обратите внимание, что только свойства, для которых заданы разные значения при копировании из другого объекта, будут помечены как измененные.  

## <a name="setting-current-or-original-values-from-a-dictionary"></a>Установка текущих или исходных значений из словаря  

Текущие или исходные значения отслеживающей сущности могут быть обновлены путем копирования значений из словаря или какой-либо другой структуры данных. Пример:  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Find(1);

    var newValues = new Dictionary\<string, object>
    {
        { "Name", "The New ADO.NET Blog" },
        { "Url", "blogs.msdn.com/adonet" },
    };

    var currentValues = context.Entry(blog).CurrentValues;

    foreach (var propertyName in newValues.Keys)
    {
        currentValues[propertyName] = newValues[propertyName];
    }

    PrintValues(currentValues);
}
```  

Для установки исходных значений используйте свойство Оригиналвалуес вместо свойства Куррентвалуес.  

## <a name="setting-current-or-original-values-from-a-dictionary-using-property"></a>Установка текущих или исходных значений из словаря с помощью свойства  

Альтернативой использованию Куррентвалуес или Оригиналвалуес, как показано выше, является использование метода Property для задания значения каждого свойства. Это может быть предпочтительнее, если необходимо задать значения сложных свойств. Пример:  

``` csharp
using (var context = new BloggingContext())
{
    var user = context.Users.Find("johndoe1987");

    var newValues = new Dictionary\<string, object>
    {
        { "Name", "John Doe" },
        { "Location.City", "Redmond" },
        { "Location.State.Name", "Washington" },
        { "Location.State.Code", "WA" },
    };

    var entry = context.Entry(user);

    foreach (var propertyName in newValues.Keys)
    {
        entry.Property(propertyName).CurrentValue = newValues[propertyName];
    }
}
```  

В примере выше сложные свойства обращаются по точечным именам. Другие способы доступа к сложным свойствам см. в двух подразделах этого раздела, особенно о сложных свойствах.  

## <a name="creating-a-cloned-object-containing-current-original-or-database-values"></a>Создание клонированного объекта, содержащего текущие, исходные значения или базы данных  

Объект DbPropertyValues, возвращаемый из Куррентвалуес, Оригиналвалуес или Жетдатабасевалуес, можно использовать для создания клона сущности. Этот клон будет содержать значения свойств из объекта DbPropertyValues, используемого для его создания. Пример:  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Find(1);

    var clonedBlog = context.Entry(blog).GetDatabaseValues().ToObject();
}
```  

Обратите внимание, что возвращаемый объект не является сущностью и не отслеживается контекстом. Возвращаемый объект также не имеет связей, настроенных для других объектов.  

Клонированный объект может быть полезен для устранения проблем, связанных с параллельными обновлениями базы данных, особенно в том случае, когда используется пользовательский интерфейс, включающий привязку данных к объектам определенного типа.  

## <a name="getting-and-setting-the-current-or-original-values-of-complex-properties"></a>Получение и установка текущих или исходных значений сложных свойств  

Значение всего сложного объекта можно считывать и задавать с помощью метода Property точно так же, как это может быть для примитивного свойства. Кроме того, можно детализировать сложный объект и считывать или задавать свойства этого объекта или даже вложенный объект. Ниже приводится несколько примеров.  

``` csharp
using (var context = new BloggingContext())
{
    var user = context.Users.Find("johndoe1987");

    // Get the Location complex object
    var location = context.Entry(user)
                       .Property(u => u.Location)
                       .CurrentValue;

    // Get the nested State complex object using chained calls
    var state1 = context.Entry(user)
                     .ComplexProperty(u => u.Location)
                     .Property(l => l.State)
                     .CurrentValue;

    // Get the nested State complex object using a single lambda expression
    var state2 = context.Entry(user)
                     .Property(u => u.Location.State)
                     .CurrentValue;

    // Get the nested State complex object using a dotted string
    var state3 = context.Entry(user)
                     .Property("Location.State")
                     .CurrentValue;

    // Get the value of the Name property on the nested State complex object using chained calls
    var name1 = context.Entry(user)
                       .ComplexProperty(u => u.Location)
                       .ComplexProperty(l => l.State)
                       .Property(s => s.Name)
                       .CurrentValue;

    // Get the value of the Name property on the nested State complex object using a single lambda expression
    var name2 = context.Entry(user)
                       .Property(u => u.Location.State.Name)
                       .CurrentValue;

    // Get the value of the Name property on the nested State complex object using a dotted string
    var name3 = context.Entry(user)
                       .Property("Location.State.Name")
                       .CurrentValue;
}
```  

Чтобы получить или задать исходное значение, используйте свойство OriginalValue вместо свойства Куррентвалуе.  

Обратите внимание, что для доступа к сложному свойству можно использовать либо свойство, либо метод Комплекспроперти. Однако необходимо использовать метод Комплекспроперти, если вы хотите детализировать сложный объект с дополнительными вызовами Property или Комплекспроперти.  

## <a name="using-dbpropertyvalues-to-access-complex-properties"></a>Использование DbPropertyValues для доступа к сложным свойствам  

При использовании Куррентвалуес, Оригиналвалуес или Жетдатабасевалуес для получения всех текущих, исходных или значений базы данных для сущности значения любых сложных свойств возвращаются как вложенные объекты DbPropertyValues. Эти вложенные объекты затем можно использовать для получения значений сложного объекта. Например, следующий метод выведет на печать значения всех свойств, включая значения сложных свойств и вложенных сложных свойств.  

``` csharp
public static void WritePropertyValues(string parentPropertyName, DbPropertyValues propertyValues)
{
    foreach (var propertyName in propertyValues.PropertyNames)
    {
        var nestedValues = propertyValues[propertyName] as DbPropertyValues;
        if (nestedValues != null)
        {
            WritePropertyValues(parentPropertyName + propertyName + ".", nestedValues);
        }
        else
        {
            Console.WriteLine("Property {0}{1} has value {2}",
                              parentPropertyName, propertyName,
                              propertyValues[propertyName]);
        }
    }
}
```  

Для вывода всех текущих значений свойств метод будет вызываться следующим образом:  

``` csharp
using (var context = new BloggingContext())
{
    var user = context.Users.Find("johndoe1987");

    WritePropertyValues("", context.Entry(user).CurrentValues);
}
```  
