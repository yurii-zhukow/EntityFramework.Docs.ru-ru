---
title: Работа со значениями свойств - EF6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: e3278b4b-9378-4fdb-923d-f64d80aaae70
caps.latest.revision: 3
ms.openlocfilehash: 07b71c9efe4e1fc3fd25a52c9cfb25f61e92f859
ms.sourcegitcommit: f05e7b62584cf228f17390bb086a61d505712e1b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/08/2018
ms.locfileid: "39121918"
---
# <a name="working-with-property-values"></a><span data-ttu-id="38fe8-102">Работа со значениями свойств</span><span class="sxs-lookup"><span data-stu-id="38fe8-102">Working with property values</span></span>
<span data-ttu-id="38fe8-103">В большинстве случаев платформа Entity Framework позаботится о отслеживания состояния, исходные значения и текущие значения свойств экземпляров сущности.</span><span class="sxs-lookup"><span data-stu-id="38fe8-103">For the most part Entity Framework will take care of tracking the state, original values, and current values of the properties of your entity instances.</span></span> <span data-ttu-id="38fe8-104">Тем не менее возможны некоторые ситуации — например без подключения к сети -, где вы хотите просмотреть или изменить сведения, которые EF о свойствах.</span><span class="sxs-lookup"><span data-stu-id="38fe8-104">However, there may be some cases - such as disconnected scenarios - where you want to view or manipulate the information EF has about the properties.</span></span> <span data-ttu-id="38fe8-105">Методы, представленные в этом разделе, также применимы к моделям, созданным с помощью Code First и конструктора EF.</span><span class="sxs-lookup"><span data-stu-id="38fe8-105">The techniques shown in this topic apply equally to models created with Code First and the EF Designer.</span></span>  

<span data-ttu-id="38fe8-106">Платформа Entity Framework хранит информацию о двух значений для каждого свойства отслеживаемой сущности.</span><span class="sxs-lookup"><span data-stu-id="38fe8-106">Entity Framework keeps track of two values for each property of a tracked entity.</span></span> <span data-ttu-id="38fe8-107">Текущее значение является, как видно из названия, текущее значение свойства в сущности.</span><span class="sxs-lookup"><span data-stu-id="38fe8-107">The current value is, as the name indicates, the current value of the property in the entity.</span></span> <span data-ttu-id="38fe8-108">Исходное значение является значением, которое свойство имело при запросе из базы данных или присоединен к контексту сущности.</span><span class="sxs-lookup"><span data-stu-id="38fe8-108">The original value is the value that the property had when the entity was queried from the database or attached to the context.</span></span>  

<span data-ttu-id="38fe8-109">Существует два механизма, общие для работы со значениями свойств:</span><span class="sxs-lookup"><span data-stu-id="38fe8-109">There are two general mechanisms for working with property values:</span></span>  

- <span data-ttu-id="38fe8-110">Значение одного свойства можно получить в строго типизированным способом с помощью метода свойства.</span><span class="sxs-lookup"><span data-stu-id="38fe8-110">The value of a single property can be obtained in a strongly typed way using the Property method.</span></span>  
- <span data-ttu-id="38fe8-111">Значения для всех свойств сущности можно считать в объект DbPropertyValues.</span><span class="sxs-lookup"><span data-stu-id="38fe8-111">Values for all properties of an entity can be read into a DbPropertyValues object.</span></span> <span data-ttu-id="38fe8-112">DbPropertyValues действует как объект подобная словарю, чтобы разрешить читать и устанавливать значения свойств.</span><span class="sxs-lookup"><span data-stu-id="38fe8-112">DbPropertyValues then acts as a dictionary-like object to allow property values to be read and set.</span></span> <span data-ttu-id="38fe8-113">Можно задать значения в объекте DbPropertyValues, на основе значений в другой объект DbPropertyValues или на основе значений в другой объект, например другую копию сущности или объект передачи данных (DTO).</span><span class="sxs-lookup"><span data-stu-id="38fe8-113">The values in a DbPropertyValues object can be set from values in another DbPropertyValues object or from values in some other object, such as another copy of the entity or a simple data transfer object (DTO).</span></span>  

<span data-ttu-id="38fe8-114">В следующих разделах приведены примеры использования обоих указанных выше механизмов.</span><span class="sxs-lookup"><span data-stu-id="38fe8-114">The sections below show examples of using both of the above mechanisms.</span></span>  

## <a name="getting-and-setting-the-current-or-original-value-of-an-individual-property"></a><span data-ttu-id="38fe8-115">Получение и задание текущее или исходное значение отдельного свойства</span><span class="sxs-lookup"><span data-stu-id="38fe8-115">Getting and setting the current or original value of an individual property</span></span>  

<span data-ttu-id="38fe8-116">В приведенном ниже примере показано, как текущее значение свойства можно считывать и задайте новое значение:</span><span class="sxs-lookup"><span data-stu-id="38fe8-116">The example below shows how the current value of a property can be read and then set to a new value:</span></span>  

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

<span data-ttu-id="38fe8-117">Свойство OriginalValue вместо свойства CurrentValue прочитать или задать исходное значение.</span><span class="sxs-lookup"><span data-stu-id="38fe8-117">Use the OriginalValue property instead of the CurrentValue property to read or set the original value.</span></span>  

<span data-ttu-id="38fe8-118">Обратите внимание, что возвращаемое значение типизируется как «object», когда строка используется для указания имени свойства.</span><span class="sxs-lookup"><span data-stu-id="38fe8-118">Note that the returned value is typed as “object” when a string is used to specify the property name.</span></span> <span data-ttu-id="38fe8-119">С другой стороны возвращаемое значение является строго типизированным, если используется лямбда-выражение.</span><span class="sxs-lookup"><span data-stu-id="38fe8-119">On the other hand, the returned value is strongly typed if a lambda expression is used.</span></span>  

<span data-ttu-id="38fe8-120">Установка значения свойства следующим образом только отмечает свойство, как изменить значение, если новое значение отличается от предыдущего значения.</span><span class="sxs-lookup"><span data-stu-id="38fe8-120">Setting the property value like this will only mark the property as modified if the new value is different from the old value.</span></span>  

<span data-ttu-id="38fe8-121">Если значение свойства имеет значение таким образом изменение обнаруживается автоматически даже если AutoDetectChanges находится в отключенном состоянии.</span><span class="sxs-lookup"><span data-stu-id="38fe8-121">When a property value is set in this way the change is automatically detected even if AutoDetectChanges is turned off.</span></span>  

## <a name="getting-and-setting-the-current-value-of-an-unmapped-property"></a><span data-ttu-id="38fe8-122">Получение и задание текущее значение несопоставленного свойства</span><span class="sxs-lookup"><span data-stu-id="38fe8-122">Getting and setting the current value of an unmapped property</span></span>  

<span data-ttu-id="38fe8-123">Текущее значение свойства, которое не сопоставлен с базы данных также могут считываться.</span><span class="sxs-lookup"><span data-stu-id="38fe8-123">The current value of a property that is not mapped to the database can also be read.</span></span> <span data-ttu-id="38fe8-124">Пример несопоставленного свойства может быть свойством RssLink в блоге.</span><span class="sxs-lookup"><span data-stu-id="38fe8-124">An example of an unmapped property could be an RssLink property on Blog.</span></span> <span data-ttu-id="38fe8-125">Это значение может быть вычислены на основании BlogId и поэтому не должны храниться в базе данных.</span><span class="sxs-lookup"><span data-stu-id="38fe8-125">This value may be calculated based on the BlogId, and therefore doesn't need to be stored in the database.</span></span> <span data-ttu-id="38fe8-126">Пример:</span><span class="sxs-lookup"><span data-stu-id="38fe8-126">For example:</span></span>  

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

<span data-ttu-id="38fe8-127">Текущее значение можно задать также в том случае, если свойство предоставляет метод задания.</span><span class="sxs-lookup"><span data-stu-id="38fe8-127">The current value can also be set if the property exposes a setter.</span></span>  

<span data-ttu-id="38fe8-128">Чтение значений параметров несопоставленные свойства полезно в том случае, при проверке платформы Entity Framework несопоставленные свойства.</span><span class="sxs-lookup"><span data-stu-id="38fe8-128">Reading the values of unmapped properties is useful when performing Entity Framework validation of unmapped properties.</span></span> <span data-ttu-id="38fe8-129">По этой же причине текущие значения можно читать и задавать для свойства сущностей, которые не не отслеживаются контекстом.</span><span class="sxs-lookup"><span data-stu-id="38fe8-129">For the same reason current values can be read and set for properties of entities that are not currently being tracked by the context.</span></span> <span data-ttu-id="38fe8-130">Пример:</span><span class="sxs-lookup"><span data-stu-id="38fe8-130">For example:</span></span>  

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

<span data-ttu-id="38fe8-131">Обратите внимание на то, что исходные значения недоступны для несопоставленные свойства или свойства сущностей, которые не отслеживаются контекстом.</span><span class="sxs-lookup"><span data-stu-id="38fe8-131">Note that original values are not available for unmapped properties or for properties of entities that are not being tracked by the context.</span></span>  

## <a name="checking-whether-a-property-is-marked-as-modified"></a><span data-ttu-id="38fe8-132">Проверка, помечен ли свойство как измененное</span><span class="sxs-lookup"><span data-stu-id="38fe8-132">Checking whether a property is marked as modified</span></span>  

<span data-ttu-id="38fe8-133">В приведенном ниже примере показано, как проверить ли отдельное свойство отмечается как измененное:</span><span class="sxs-lookup"><span data-stu-id="38fe8-133">The example below shows how to check whether or not an individual property is marked as modified:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Find(1);

    var nameIsModified1 = context.Entry(blog).Property(u => u.Name).IsModified;

    // Use a string for the property name
    var nameIsModified2 = context.Entry(blog).Property("Name").IsModified;
}
```  

<span data-ttu-id="38fe8-134">Значения измененных свойств, отправляются в виде обновлений в базу данных при вызове метода SaveChanges.</span><span class="sxs-lookup"><span data-stu-id="38fe8-134">The values of modified properties are sent as updates to the database when SaveChanges is called.</span></span>  

##  <a name="marking-a-property-as-modified"></a><span data-ttu-id="38fe8-135">Пометить как измененное свойство</span><span class="sxs-lookup"><span data-stu-id="38fe8-135">Marking a property as modified</span></span>  

<span data-ttu-id="38fe8-136">В приведенном ниже примере показано, как заставить отдельное свойство помечается как измененный:</span><span class="sxs-lookup"><span data-stu-id="38fe8-136">The example below shows how to force an individual property to be marked as modified:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Find(1);

    context.Entry(blog).Property(u => u.Name).IsModified = true;

    // Use a string for the property name
    context.Entry(blog).Property("Name").IsModified = true;
}
```  

<span data-ttu-id="38fe8-137">Пометка измененный принудительное обновление быть отправки в базу данных для свойства при вызове метода SaveChanges, даже если текущее значение свойства является таким же, как исходное значение свойства.</span><span class="sxs-lookup"><span data-stu-id="38fe8-137">Marking a property as modified forces an update to be send to the database for the property when SaveChanges is called even if the current value of the property is the same as its original value.</span></span>  

<span data-ttu-id="38fe8-138">Не сейчас невозможно сбросить отдельного свойства, которые не будут изменяться после он был помечен как измененный.</span><span class="sxs-lookup"><span data-stu-id="38fe8-138">It is not currently possible to reset an individual property to be not modified after it has been marked as modified.</span></span> <span data-ttu-id="38fe8-139">Это то, что мы планируем реализовать поддержку в будущем выпуске.</span><span class="sxs-lookup"><span data-stu-id="38fe8-139">This is something we plan to support in a future release.</span></span>  

## <a name="reading-current-original-and-database-values-for-all-properties-of-an-entity"></a><span data-ttu-id="38fe8-140">Считывание текущего, исходные и значения базы данных для всех свойств сущности</span><span class="sxs-lookup"><span data-stu-id="38fe8-140">Reading current, original, and database values for all properties of an entity</span></span>  

<span data-ttu-id="38fe8-141">В приведенном ниже примере показано, как считывать значения фактически в базе данных для всех сопоставленных свойств сущности, исходные значения и текущие значения.</span><span class="sxs-lookup"><span data-stu-id="38fe8-141">The example below shows how to read the current values, the original values, and the values actually in the database for all mapped properties of an entity.</span></span>  

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

<span data-ttu-id="38fe8-142">Текущие значения — это значения, в настоящий момент содержат свойства сущности.</span><span class="sxs-lookup"><span data-stu-id="38fe8-142">The current values are the values that the properties of the entity currently contain.</span></span> <span data-ttu-id="38fe8-143">Исходные значения — это значения, считанных из базы данных, на которые запросила сущности.</span><span class="sxs-lookup"><span data-stu-id="38fe8-143">The original values are the values that were read from the database when the entity was queried.</span></span> <span data-ttu-id="38fe8-144">Значения базы данных, значения в настоящее время хранятся в базе данных.</span><span class="sxs-lookup"><span data-stu-id="38fe8-144">The database values are the values as they are currently stored in the database.</span></span> <span data-ttu-id="38fe8-145">Получение значений базы данных полезно в том случае, когда значения в базе данных мог быть изменен, так как сущность запросила, такие как стала при одновременное изменение в базе данных другим пользователем.</span><span class="sxs-lookup"><span data-stu-id="38fe8-145">Getting the database values is useful when the values in the database may have changed since the entity was queried such as when a concurrent edit to the database has been made by another user.</span></span>  

## <a name="setting-current-or-original-values-from-another-object"></a><span data-ttu-id="38fe8-146">Текущее и исходное значения из другого объекта</span><span class="sxs-lookup"><span data-stu-id="38fe8-146">Setting current or original values from another object</span></span>  

<span data-ttu-id="38fe8-147">Текущее и исходное значения отслеживаемой сущности можно обновить путем копирования значения из другого объекта.</span><span class="sxs-lookup"><span data-stu-id="38fe8-147">The current or original values of a tracked entity can be updated by copying values from another object.</span></span> <span data-ttu-id="38fe8-148">Пример:</span><span class="sxs-lookup"><span data-stu-id="38fe8-148">For example:</span></span>  

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

<span data-ttu-id="38fe8-149">Под управлением приведенный выше код выводит на экран:</span><span class="sxs-lookup"><span data-stu-id="38fe8-149">Running the code above will print out:</span></span>  

```  
Current values:
Property Id has value 1
Property Name has value My Cool Blog

Original values:
Property Id has value 1
Property Name has value My Boring Blog
```  

<span data-ttu-id="38fe8-150">Этот способ иногда используется при обновлении сущности с помощью значений, полученных из вызова службы или клиента в n уровневого приложения.</span><span class="sxs-lookup"><span data-stu-id="38fe8-150">This technique is sometimes used when updating an entity with values obtained from a service call or a client in an n-tier application.</span></span> <span data-ttu-id="38fe8-151">Обратите внимание, что объект используется не быть того же типа как сущность, до тех пор, пока он имеет свойства, имена которых совпадают с сущности.</span><span class="sxs-lookup"><span data-stu-id="38fe8-151">Note that the object used does not have to be of the same type as the entity so long as it has properties whose names match those of the entity.</span></span> <span data-ttu-id="38fe8-152">В приведенном выше примере экземпляр BlogDTO используется для обновления исходных значений.</span><span class="sxs-lookup"><span data-stu-id="38fe8-152">In the example above, an instance of BlogDTO is used to update the original values.</span></span>  

<span data-ttu-id="38fe8-153">Обратите внимание на то, что только те свойства, которые заданы разные значения, при копировании с другой объект будет помечен как измененный.</span><span class="sxs-lookup"><span data-stu-id="38fe8-153">Note that only properties that are set to different values when copied from the other object will be marked as modified.</span></span>  

## <a name="setting-current-or-original-values-from-a-dictionary"></a><span data-ttu-id="38fe8-154">Параметр текущее и исходное значения из словаря</span><span class="sxs-lookup"><span data-stu-id="38fe8-154">Setting current or original values from a dictionary</span></span>  

<span data-ttu-id="38fe8-155">Текущее и исходное значения отслеживаемой сущности можно обновить путем копирования значения из словаря или некоторые другие структуры данных.</span><span class="sxs-lookup"><span data-stu-id="38fe8-155">The current or original values of a tracked entity can be updated by copying values from a dictionary or some other data structure.</span></span> <span data-ttu-id="38fe8-156">Пример:</span><span class="sxs-lookup"><span data-stu-id="38fe8-156">For example:</span></span>  

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

<span data-ttu-id="38fe8-157">Используйте свойство OriginalValues вместо свойства CurrentValues для установки исходных значений.</span><span class="sxs-lookup"><span data-stu-id="38fe8-157">Use the OriginalValues property instead of the CurrentValues property to set original values.</span></span>  

## <a name="setting-current-or-original-values-from-a-dictionary-using-property"></a><span data-ttu-id="38fe8-158">Параметр текущее и исходное значения из словаря с помощью свойства</span><span class="sxs-lookup"><span data-stu-id="38fe8-158">Setting current or original values from a dictionary using Property</span></span>  

<span data-ttu-id="38fe8-159">Использование метода свойство для задания значения каждого свойства является альтернативой использованию CurrentValues или OriginalValues, как показано выше.</span><span class="sxs-lookup"><span data-stu-id="38fe8-159">An alternative to using CurrentValues or OriginalValues as shown above is to use the Property method to set the value of each property.</span></span> <span data-ttu-id="38fe8-160">Это может быть более предпочтительным, чем при необходимости задайте значения свойств сложных свойств.</span><span class="sxs-lookup"><span data-stu-id="38fe8-160">This can be preferable when you need to set the values of complex properties.</span></span> <span data-ttu-id="38fe8-161">Пример:</span><span class="sxs-lookup"><span data-stu-id="38fe8-161">For example:</span></span>  

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

<span data-ttu-id="38fe8-162">В примере выше сложных свойств осуществляется с помощью точечно имена.</span><span class="sxs-lookup"><span data-stu-id="38fe8-162">In the example above complex properties are accessed using dotted names.</span></span> <span data-ttu-id="38fe8-163">Другие способы доступа к сложным свойствам см. в двух разделах далее в этом разделе, в частности о сложных свойств.</span><span class="sxs-lookup"><span data-stu-id="38fe8-163">For other ways to access complex properties see the two sections later in this topic specifically about complex properties.</span></span>  

## <a name="creating-a-cloned-object-containing-current-original-or-database-values"></a><span data-ttu-id="38fe8-164">Создание клонированный объект, содержащий текущий, исходные или значения базы данных</span><span class="sxs-lookup"><span data-stu-id="38fe8-164">Creating a cloned object containing current, original, or database values</span></span>  

<span data-ttu-id="38fe8-165">DbPropertyValues объект, возвращенный CurrentValues OriginalValues, или GetDatabaseValues может использоваться для создания клона сущности.</span><span class="sxs-lookup"><span data-stu-id="38fe8-165">The DbPropertyValues object returned from CurrentValues, OriginalValues, or GetDatabaseValues can be used to create a clone of the entity.</span></span> <span data-ttu-id="38fe8-166">Этот клон будет содержать значения свойств из объекта DbPropertyValues, использованного для его создания.</span><span class="sxs-lookup"><span data-stu-id="38fe8-166">This clone will contain the property values from the DbPropertyValues object used to create it.</span></span> <span data-ttu-id="38fe8-167">Пример:</span><span class="sxs-lookup"><span data-stu-id="38fe8-167">For example:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Find(1);

    var clonedBlog = context.Entry(blog).GetDatabaseValues().ToObject();
}
```  

<span data-ttu-id="38fe8-168">Обратите внимание, что возвращенный объект не является ни сущностью и не отслеживается контекстом.</span><span class="sxs-lookup"><span data-stu-id="38fe8-168">Note that the object returned is not the entity and is not being tracked by the context.</span></span> <span data-ttu-id="38fe8-169">Возвращаемый объект также имеет все связи, задайте для других объектов.</span><span class="sxs-lookup"><span data-stu-id="38fe8-169">The returned object also does not have any relationships set to other objects.</span></span>  

<span data-ttu-id="38fe8-170">Клонированный объект может быть полезно при решении проблем, связанных с одновременных обновлений в базу данных, особенно когда используется пользовательский Интерфейс, который включает в себя привязку данных к объектам определенного типа.</span><span class="sxs-lookup"><span data-stu-id="38fe8-170">The cloned object can be useful for resolving issues related to concurrent updates to the database, especially where a UI that involves data binding to objects of a certain type is being used.</span></span>  

## <a name="getting-and-setting-the-current-or-original-values-of-complex-properties"></a><span data-ttu-id="38fe8-171">Получение и установка значения текущей или исходной сложных свойств</span><span class="sxs-lookup"><span data-stu-id="38fe8-171">Getting and setting the current or original values of complex properties</span></span>  

<span data-ttu-id="38fe8-172">Чтение и задается с помощью метода свойства так же, как это может быть простое свойство, допускается значение всего сложного объекта.</span><span class="sxs-lookup"><span data-stu-id="38fe8-172">The value of an entire complex object can be read and set using the Property method just as it can be for a primitive property.</span></span> <span data-ttu-id="38fe8-173">Кроме того можно выполнить детализацию в сложного объекта и чтения или заданы свойства этого объекта, или даже вложенного объекта.</span><span class="sxs-lookup"><span data-stu-id="38fe8-173">In addition you can drill down into the complex object and read or set properties of that object, or even a nested object.</span></span> <span data-ttu-id="38fe8-174">Далее приводятся некоторые примеры.</span><span class="sxs-lookup"><span data-stu-id="38fe8-174">Here are some examples:</span></span>  

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

<span data-ttu-id="38fe8-175">Свойство OriginalValue вместо свойства CurrentValue требуется получить или задать исходное значение.</span><span class="sxs-lookup"><span data-stu-id="38fe8-175">Use the OriginalValue property instead of the CurrentValue property to get or set an original value.</span></span>  

<span data-ttu-id="38fe8-176">Обратите внимание на то, что свойство или метод ComplexProperty может использоваться для доступа к сложного свойства.</span><span class="sxs-lookup"><span data-stu-id="38fe8-176">Note that either the Property or the ComplexProperty method can be used to access a complex property.</span></span> <span data-ttu-id="38fe8-177">Тем не менее метод ComplexProperty должен использоваться, если вы хотите углубиться в сложного объекта с дополнительным свойством или вызывает ComplexProperty.</span><span class="sxs-lookup"><span data-stu-id="38fe8-177">However, the ComplexProperty method must be used if you wish to drill down into the complex object with additional Property or ComplexProperty calls.</span></span>  

## <a name="using-dbpropertyvalues-to-access-complex-properties"></a><span data-ttu-id="38fe8-178">Использование DbPropertyValues для доступа к сложным свойствам</span><span class="sxs-lookup"><span data-stu-id="38fe8-178">Using DbPropertyValues to access complex properties</span></span>  

<span data-ttu-id="38fe8-179">При использовании CurrentValues, OriginalValues или GetDatabaseValues для получения исходного все текущие, или значения базы данных для сущности, как вложенные объекты DbPropertyValues возвращаются значения любого из сложных свойств.</span><span class="sxs-lookup"><span data-stu-id="38fe8-179">When you use CurrentValues, OriginalValues, or GetDatabaseValues to get all the current, original, or database values for an entity, the values of any complex properties are returned as nested DbPropertyValues objects.</span></span> <span data-ttu-id="38fe8-180">Эти вложенные объекты могут затем использоваться для получения значения сложного объекта.</span><span class="sxs-lookup"><span data-stu-id="38fe8-180">These nested objects can then be used to get values of the complex object.</span></span> <span data-ttu-id="38fe8-181">Например следующий метод напечатает значения всех свойств, включая значения любого сложных свойств и вложенных составных свойств.</span><span class="sxs-lookup"><span data-stu-id="38fe8-181">For example, the following method will print out the values of all properties, including values of any complex properties and nested complex properties.</span></span>  

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

<span data-ttu-id="38fe8-182">Чтобы распечатать все текущие значения свойств, метод будет вызываться следующим образом:</span><span class="sxs-lookup"><span data-stu-id="38fe8-182">To print out all current property values the method would be called like this:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    var user = context.Users.Find("johndoe1987");

    WritePropertyValues("", context.Entry(user).CurrentValues);
}
```  
