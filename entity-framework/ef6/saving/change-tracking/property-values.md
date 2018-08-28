---
title: Работа со значениями свойств - EF6
author: divega
ms.date: 2016-10-23
ms.assetid: e3278b4b-9378-4fdb-923d-f64d80aaae70
ms.openlocfilehash: a9b969950ec7dcfb86a2abc9c8bd6cc24899948c
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/27/2018
ms.locfileid: "42998307"
---
# <a name="working-with-property-values"></a>Работа со значениями свойств
В большинстве случаев платформа Entity Framework позаботится о отслеживания состояния, исходные значения и текущие значения свойств экземпляров сущности. Тем не менее возможны некоторые ситуации — например без подключения к сети -, где вы хотите просмотреть или изменить сведения, которые EF о свойствах. Методы, представленные в этом разделе, также применимы к моделям, созданным с помощью Code First и конструктора EF.  

Платформа Entity Framework хранит информацию о двух значений для каждого свойства отслеживаемой сущности. Текущее значение является, как видно из названия, текущее значение свойства в сущности. Исходное значение является значением, которое свойство имело при запросе из базы данных или присоединен к контексту сущности.  

Существует два механизма, общие для работы со значениями свойств:  

- Значение одного свойства можно получить в строго типизированным способом с помощью метода свойства.  
- Значения для всех свойств сущности можно считать в объект DbPropertyValues. DbPropertyValues действует как объект подобная словарю, чтобы разрешить читать и устанавливать значения свойств. Можно задать значения в объекте DbPropertyValues, на основе значений в другой объект DbPropertyValues или на основе значений в другой объект, например другую копию сущности или объект передачи данных (DTO).  

В следующих разделах приведены примеры использования обоих указанных выше механизмов.  

## <a name="getting-and-setting-the-current-or-original-value-of-an-individual-property"></a>Получение и задание текущее или исходное значение отдельного свойства  

В приведенном ниже примере показано, как текущее значение свойства можно считывать и задайте новое значение:  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Find(3);

    // Read the current value of the Name property
    string currentName1 = context.Entry(blog).Property(u => u.Name).CurrentValue;

    // Set the Name property to a new value
    context.Entry(name).Property(u => u.Name).CurrentValue = "My Fancy Blog";

    // Read the current value of the Name property using a string for the property name
    object currentName2 = context.Entry(blog).Property("Name").CurrentValue;

    // Set the Name property to a new value using a string for the property name
    context.Entry(blog).Property("Name").CurrentValue = "My Boring Blog";
}
```  

Свойство OriginalValue вместо свойства CurrentValue прочитать или задать исходное значение.  

Обратите внимание, что возвращаемое значение типизируется как «object», когда строка используется для указания имени свойства. С другой стороны возвращаемое значение является строго типизированным, если используется лямбда-выражение.  

Установка значения свойства следующим образом только отмечает свойство, как изменить значение, если новое значение отличается от предыдущего значения.  

Если значение свойства имеет значение таким образом изменение обнаруживается автоматически даже если AutoDetectChanges находится в отключенном состоянии.  

## <a name="getting-and-setting-the-current-value-of-an-unmapped-property"></a>Получение и задание текущее значение несопоставленного свойства  

Текущее значение свойства, которое не сопоставлен с базы данных также могут считываться. Пример несопоставленного свойства может быть свойством RssLink в блоге. Это значение может быть вычислены на основании BlogId и поэтому не должны храниться в базе данных. Пример:  

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

Текущее значение можно задать также в том случае, если свойство предоставляет метод задания.  

Чтение значений параметров несопоставленные свойства полезно в том случае, при проверке платформы Entity Framework несопоставленные свойства. По этой же причине текущие значения можно читать и задавать для свойства сущностей, которые не не отслеживаются контекстом. Пример:  

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

Обратите внимание на то, что исходные значения недоступны для несопоставленные свойства или свойства сущностей, которые не отслеживаются контекстом.  

## <a name="checking-whether-a-property-is-marked-as-modified"></a>Проверка, помечен ли свойство как измененное  

В приведенном ниже примере показано, как проверить ли отдельное свойство отмечается как измененное:  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Find(1);

    var nameIsModified1 = context.Entry(blog).Property(u => u.Name).IsModified;

    // Use a string for the property name
    var nameIsModified2 = context.Entry(blog).Property("Name").IsModified;
}
```  

Значения измененных свойств, отправляются в виде обновлений в базу данных при вызове метода SaveChanges.  

##  <a name="marking-a-property-as-modified"></a>Пометить как измененное свойство  

В приведенном ниже примере показано, как заставить отдельное свойство помечается как измененный:  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Find(1);

    context.Entry(blog).Property(u => u.Name).IsModified = true;

    // Use a string for the property name
    context.Entry(blog).Property("Name").IsModified = true;
}
```  

Пометка измененный принудительное обновление быть отправки в базу данных для свойства при вызове метода SaveChanges, даже если текущее значение свойства является таким же, как исходное значение свойства.  

Не сейчас невозможно сбросить отдельного свойства, которые не будут изменяться после он был помечен как измененный. Это то, что мы планируем реализовать поддержку в будущем выпуске.  

## <a name="reading-current-original-and-database-values-for-all-properties-of-an-entity"></a>Считывание текущего, исходные и значения базы данных для всех свойств сущности  

В приведенном ниже примере показано, как считывать значения фактически в базе данных для всех сопоставленных свойств сущности, исходные значения и текущие значения.  

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

Текущие значения — это значения, в настоящий момент содержат свойства сущности. Исходные значения — это значения, считанных из базы данных, на которые запросила сущности. Значения базы данных, значения в настоящее время хранятся в базе данных. Получение значений базы данных полезно в том случае, когда значения в базе данных мог быть изменен, так как сущность запросила, такие как стала при одновременное изменение в базе данных другим пользователем.  

## <a name="setting-current-or-original-values-from-another-object"></a>Текущее и исходное значения из другого объекта  

Текущее и исходное значения отслеживаемой сущности можно обновить путем копирования значения из другого объекта. Пример:  

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

Под управлением приведенный выше код выводит на экран:  

```  
Current values:
Property Id has value 1
Property Name has value My Cool Blog

Original values:
Property Id has value 1
Property Name has value My Boring Blog
```  

Этот способ иногда используется при обновлении сущности с помощью значений, полученных из вызова службы или клиента в n уровневого приложения. Обратите внимание, что объект используется не быть того же типа как сущность, до тех пор, пока он имеет свойства, имена которых совпадают с сущности. В приведенном выше примере экземпляр BlogDTO используется для обновления исходных значений.  

Обратите внимание на то, что только те свойства, которые заданы разные значения, при копировании с другой объект будет помечен как измененный.  

## <a name="setting-current-or-original-values-from-a-dictionary"></a>Параметр текущее и исходное значения из словаря  

Текущее и исходное значения отслеживаемой сущности можно обновить путем копирования значения из словаря или некоторые другие структуры данных. Пример:  

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

Используйте свойство OriginalValues вместо свойства CurrentValues для установки исходных значений.  

## <a name="setting-current-or-original-values-from-a-dictionary-using-property"></a>Параметр текущее и исходное значения из словаря с помощью свойства  

Использование метода свойство для задания значения каждого свойства является альтернативой использованию CurrentValues или OriginalValues, как показано выше. Это может быть более предпочтительным, чем при необходимости задайте значения свойств сложных свойств. Пример:  

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

В примере выше сложных свойств осуществляется с помощью точечно имена. Другие способы доступа к сложным свойствам см. в двух разделах далее в этом разделе, в частности о сложных свойств.  

## <a name="creating-a-cloned-object-containing-current-original-or-database-values"></a>Создание клонированный объект, содержащий текущий, исходные или значения базы данных  

DbPropertyValues объект, возвращенный CurrentValues OriginalValues, или GetDatabaseValues может использоваться для создания клона сущности. Этот клон будет содержать значения свойств из объекта DbPropertyValues, использованного для его создания. Пример:  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Find(1);

    var clonedBlog = context.Entry(blog).GetDatabaseValues().ToObject();
}
```  

Обратите внимание, что возвращенный объект не является ни сущностью и не отслеживается контекстом. Возвращаемый объект также имеет все связи, задайте для других объектов.  

Клонированный объект может быть полезно при решении проблем, связанных с одновременных обновлений в базу данных, особенно когда используется пользовательский Интерфейс, который включает в себя привязку данных к объектам определенного типа.  

## <a name="getting-and-setting-the-current-or-original-values-of-complex-properties"></a>Получение и установка значения текущей или исходной сложных свойств  

Чтение и задается с помощью метода свойства так же, как это может быть простое свойство, допускается значение всего сложного объекта. Кроме того можно выполнить детализацию в сложного объекта и чтения или заданы свойства этого объекта, или даже вложенного объекта. Далее приводятся некоторые примеры.  

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

Свойство OriginalValue вместо свойства CurrentValue требуется получить или задать исходное значение.  

Обратите внимание на то, что свойство или метод ComplexProperty может использоваться для доступа к сложного свойства. Тем не менее метод ComplexProperty должен использоваться, если вы хотите углубиться в сложного объекта с дополнительным свойством или вызывает ComplexProperty.  

## <a name="using-dbpropertyvalues-to-access-complex-properties"></a>Использование DbPropertyValues для доступа к сложным свойствам  

При использовании CurrentValues, OriginalValues или GetDatabaseValues для получения исходного все текущие, или значения базы данных для сущности, как вложенные объекты DbPropertyValues возвращаются значения любого из сложных свойств. Эти вложенные объекты могут затем использоваться для получения значения сложного объекта. Например следующий метод напечатает значения всех свойств, включая значения любого сложных свойств и вложенных составных свойств.  

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

Чтобы распечатать все текущие значения свойств, метод будет вызываться следующим образом:  

``` csharp
using (var context = new BloggingContext())
{
    var user = context.Users.Find("johndoe1987");

    WritePropertyValues("", context.Entry(user).CurrentValues);
}
```  
