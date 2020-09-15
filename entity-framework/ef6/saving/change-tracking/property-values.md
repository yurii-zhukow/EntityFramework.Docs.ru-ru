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
# <a name="working-with-property-values"></a><span data-ttu-id="346b0-103">Работа со значениями свойств</span><span class="sxs-lookup"><span data-stu-id="346b0-103">Working with property values</span></span>
<span data-ttu-id="346b0-104">Для большей части Entity Framework будет отслеживать состояние, исходные значения и текущие значения свойств экземпляров сущности.</span><span class="sxs-lookup"><span data-stu-id="346b0-104">For the most part Entity Framework will take care of tracking the state, original values, and current values of the properties of your entity instances.</span></span> <span data-ttu-id="346b0-105">Однако могут возникнуть некоторые случаи, например сценарии с отключением, в которых необходимо просмотреть сведения о свойствах EF и управлять ими.</span><span class="sxs-lookup"><span data-stu-id="346b0-105">However, there may be some cases - such as disconnected scenarios - where you want to view or manipulate the information EF has about the properties.</span></span> <span data-ttu-id="346b0-106">Методы, представленные в этом разделе, также применимы к моделям, созданным с помощью Code First и конструктора EF.</span><span class="sxs-lookup"><span data-stu-id="346b0-106">The techniques shown in this topic apply equally to models created with Code First and the EF Designer.</span></span>  

<span data-ttu-id="346b0-107">Entity Framework отслеживает два значения для каждого свойства отслеживающей сущности.</span><span class="sxs-lookup"><span data-stu-id="346b0-107">Entity Framework keeps track of two values for each property of a tracked entity.</span></span> <span data-ttu-id="346b0-108">Текущее значение равно, так как имя указывает текущее значение свойства в сущности.</span><span class="sxs-lookup"><span data-stu-id="346b0-108">The current value is, as the name indicates, the current value of the property in the entity.</span></span> <span data-ttu-id="346b0-109">Исходное значение — это значение, которое имело свойство при запросе сущности из базы данных или присоединено к контексту.</span><span class="sxs-lookup"><span data-stu-id="346b0-109">The original value is the value that the property had when the entity was queried from the database or attached to the context.</span></span>  

<span data-ttu-id="346b0-110">Существуют два общих механизма работы со значениями свойств.</span><span class="sxs-lookup"><span data-stu-id="346b0-110">There are two general mechanisms for working with property values:</span></span>  

- <span data-ttu-id="346b0-111">Значение одного свойства можно получить строго типизированным способом с помощью метода Property.</span><span class="sxs-lookup"><span data-stu-id="346b0-111">The value of a single property can be obtained in a strongly typed way using the Property method.</span></span>  
- <span data-ttu-id="346b0-112">Значения для всех свойств сущности могут быть считаны в объект DbPropertyValues.</span><span class="sxs-lookup"><span data-stu-id="346b0-112">Values for all properties of an entity can be read into a DbPropertyValues object.</span></span> <span data-ttu-id="346b0-113">Затем DbPropertyValues выступает в качестве объекта, подобного словарю, чтобы разрешить чтение и установку значений свойств.</span><span class="sxs-lookup"><span data-stu-id="346b0-113">DbPropertyValues then acts as a dictionary-like object to allow property values to be read and set.</span></span> <span data-ttu-id="346b0-114">Значения в объекте DbPropertyValues могут быть заданы из значений другого объекта DbPropertyValues или из значений в каком-либо другом объекте, например в другой копии сущности или в простом объекте передачи данных (DTO).</span><span class="sxs-lookup"><span data-stu-id="346b0-114">The values in a DbPropertyValues object can be set from values in another DbPropertyValues object or from values in some other object, such as another copy of the entity or a simple data transfer object (DTO).</span></span>  

<span data-ttu-id="346b0-115">В следующих разделах приведены примеры использования обоих описанных выше механизмов.</span><span class="sxs-lookup"><span data-stu-id="346b0-115">The sections below show examples of using both of the above mechanisms.</span></span>  

## <a name="getting-and-setting-the-current-or-original-value-of-an-individual-property"></a><span data-ttu-id="346b0-116">Получение и установка текущего или исходного значения отдельного свойства</span><span class="sxs-lookup"><span data-stu-id="346b0-116">Getting and setting the current or original value of an individual property</span></span>  

<span data-ttu-id="346b0-117">В приведенном ниже примере показано, как можно прочитать текущее значение свойства, а затем задать новое значение.</span><span class="sxs-lookup"><span data-stu-id="346b0-117">The example below shows how the current value of a property can be read and then set to a new value:</span></span>  

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

<span data-ttu-id="346b0-118">Используйте свойство OriginalValue вместо свойства Куррентвалуе для чтения или установки исходного значения.</span><span class="sxs-lookup"><span data-stu-id="346b0-118">Use the OriginalValue property instead of the CurrentValue property to read or set the original value.</span></span>  

<span data-ttu-id="346b0-119">Обратите внимание, что возвращаемое значение вводится как "Object", если строка используется для указания имени свойства.</span><span class="sxs-lookup"><span data-stu-id="346b0-119">Note that the returned value is typed as “object” when a string is used to specify the property name.</span></span> <span data-ttu-id="346b0-120">С другой стороны, возвращаемое значение является строго типизированным, если используется лямбда-выражение.</span><span class="sxs-lookup"><span data-stu-id="346b0-120">On the other hand, the returned value is strongly typed if a lambda expression is used.</span></span>  

<span data-ttu-id="346b0-121">Если задать значение свойства Like, свойство будет помечено как измененное, только если новое значение отличается от старого значения.</span><span class="sxs-lookup"><span data-stu-id="346b0-121">Setting the property value like this will only mark the property as modified if the new value is different from the old value.</span></span>  

<span data-ttu-id="346b0-122">Если значение свойства задано таким образом, это изменение определяется автоматически, даже если Аутодетектчанжес выключен.</span><span class="sxs-lookup"><span data-stu-id="346b0-122">When a property value is set in this way the change is automatically detected even if AutoDetectChanges is turned off.</span></span>  

## <a name="getting-and-setting-the-current-value-of-an-unmapped-property"></a><span data-ttu-id="346b0-123">Получение и установка текущего значения несопоставленного свойства</span><span class="sxs-lookup"><span data-stu-id="346b0-123">Getting and setting the current value of an unmapped property</span></span>  

<span data-ttu-id="346b0-124">Также можно прочитать текущее значение свойства, которое не сопоставлено с базой данных.</span><span class="sxs-lookup"><span data-stu-id="346b0-124">The current value of a property that is not mapped to the database can also be read.</span></span> <span data-ttu-id="346b0-125">Примером несопоставленного свойства может быть свойство Рсслинк в блоге.</span><span class="sxs-lookup"><span data-stu-id="346b0-125">An example of an unmapped property could be an RssLink property on Blog.</span></span> <span data-ttu-id="346b0-126">Это значение может быть вычислено на основе Блогид и, следовательно, не должно храниться в базе данных.</span><span class="sxs-lookup"><span data-stu-id="346b0-126">This value may be calculated based on the BlogId, and therefore doesn't need to be stored in the database.</span></span> <span data-ttu-id="346b0-127">Пример:</span><span class="sxs-lookup"><span data-stu-id="346b0-127">For example:</span></span>  

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

<span data-ttu-id="346b0-128">Текущее значение также может быть задано, если свойство предоставляет метод задания.</span><span class="sxs-lookup"><span data-stu-id="346b0-128">The current value can also be set if the property exposes a setter.</span></span>  

<span data-ttu-id="346b0-129">Чтение значений несопоставленных свойств полезно при выполнении Entity Framework проверки несопоставленных свойств.</span><span class="sxs-lookup"><span data-stu-id="346b0-129">Reading the values of unmapped properties is useful when performing Entity Framework validation of unmapped properties.</span></span> <span data-ttu-id="346b0-130">По той же причине текущие значения можно считывать и задавать для свойств сущностей, которые в настоящее время не отслеживается контекстом.</span><span class="sxs-lookup"><span data-stu-id="346b0-130">For the same reason current values can be read and set for properties of entities that are not currently being tracked by the context.</span></span> <span data-ttu-id="346b0-131">Пример:</span><span class="sxs-lookup"><span data-stu-id="346b0-131">For example:</span></span>  

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

<span data-ttu-id="346b0-132">Обратите внимание, что исходные значения недоступны для несопоставленных свойств или для свойств сущностей, которые не отслеживается контекстом.</span><span class="sxs-lookup"><span data-stu-id="346b0-132">Note that original values are not available for unmapped properties or for properties of entities that are not being tracked by the context.</span></span>  

## <a name="checking-whether-a-property-is-marked-as-modified"></a><span data-ttu-id="346b0-133">Проверка того, помечено ли свойство как измененное</span><span class="sxs-lookup"><span data-stu-id="346b0-133">Checking whether a property is marked as modified</span></span>  

<span data-ttu-id="346b0-134">В приведенном ниже примере показано, как проверить, помечено ли отдельное свойство как измененное.</span><span class="sxs-lookup"><span data-stu-id="346b0-134">The example below shows how to check whether or not an individual property is marked as modified:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Find(1);

    var nameIsModified1 = context.Entry(blog).Property(u => u.Name).IsModified;

    // Use a string for the property name
    var nameIsModified2 = context.Entry(blog).Property("Name").IsModified;
}
```  

<span data-ttu-id="346b0-135">Значения измененных свойств отправляются в виде обновлений в базу данных при вызове метода SaveChanges.</span><span class="sxs-lookup"><span data-stu-id="346b0-135">The values of modified properties are sent as updates to the database when SaveChanges is called.</span></span>  

##  <a name="marking-a-property-as-modified"></a><span data-ttu-id="346b0-136">Пометка свойства как измененного</span><span class="sxs-lookup"><span data-stu-id="346b0-136">Marking a property as modified</span></span>  

<span data-ttu-id="346b0-137">В приведенном ниже примере показано, как принудительно пометить отдельное свойство как измененное.</span><span class="sxs-lookup"><span data-stu-id="346b0-137">The example below shows how to force an individual property to be marked as modified:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Find(1);

    context.Entry(blog).Property(u => u.Name).IsModified = true;

    // Use a string for the property name
    context.Entry(blog).Property("Name").IsModified = true;
}
```  

<span data-ttu-id="346b0-138">Пометка свойства как измененного приводит к тому, что обновление будет отправлено в базу данных для свойства при вызове SaveChanges, даже если текущее значение свойства совпадает с исходным значением.</span><span class="sxs-lookup"><span data-stu-id="346b0-138">Marking a property as modified forces an update to be send to the database for the property when SaveChanges is called even if the current value of the property is the same as its original value.</span></span>  

<span data-ttu-id="346b0-139">Сейчас невозможно сбросить отдельное свойство, чтобы оно не было изменено после того, как оно было помечено как измененное.</span><span class="sxs-lookup"><span data-stu-id="346b0-139">It is not currently possible to reset an individual property to be not modified after it has been marked as modified.</span></span> <span data-ttu-id="346b0-140">Это то, что мы планируем поддерживать в будущем выпуске.</span><span class="sxs-lookup"><span data-stu-id="346b0-140">This is something we plan to support in a future release.</span></span>  

## <a name="reading-current-original-and-database-values-for-all-properties-of-an-entity"></a><span data-ttu-id="346b0-141">Чтение текущих, исходных и значений баз данных для всех свойств сущности</span><span class="sxs-lookup"><span data-stu-id="346b0-141">Reading current, original, and database values for all properties of an entity</span></span>  

<span data-ttu-id="346b0-142">В приведенном ниже примере показано, как считывать текущие значения, исходные значения и значения, фактически находящиеся в базе данных для всех сопоставленных свойств сущности.</span><span class="sxs-lookup"><span data-stu-id="346b0-142">The example below shows how to read the current values, the original values, and the values actually in the database for all mapped properties of an entity.</span></span>  

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

<span data-ttu-id="346b0-143">Текущие значения — это значения, которые в настоящее время содержатся в свойствах сущности.</span><span class="sxs-lookup"><span data-stu-id="346b0-143">The current values are the values that the properties of the entity currently contain.</span></span> <span data-ttu-id="346b0-144">Исходные значения — это значения, которые были считаны из базы данных при запросе сущности.</span><span class="sxs-lookup"><span data-stu-id="346b0-144">The original values are the values that were read from the database when the entity was queried.</span></span> <span data-ttu-id="346b0-145">Значения базы данных — это значения, которые в настоящее время хранятся в базе данных.</span><span class="sxs-lookup"><span data-stu-id="346b0-145">The database values are the values as they are currently stored in the database.</span></span> <span data-ttu-id="346b0-146">Получение значений базы данных полезно, когда значения в базе данных могли измениться с момента запроса сущности, например, когда одновременное изменение базы данных выполнено другим пользователем.</span><span class="sxs-lookup"><span data-stu-id="346b0-146">Getting the database values is useful when the values in the database may have changed since the entity was queried such as when a concurrent edit to the database has been made by another user.</span></span>  

## <a name="setting-current-or-original-values-from-another-object"></a><span data-ttu-id="346b0-147">Установка текущих или исходных значений из другого объекта</span><span class="sxs-lookup"><span data-stu-id="346b0-147">Setting current or original values from another object</span></span>  

<span data-ttu-id="346b0-148">Текущие или исходные значения отслеживающей сущности могут быть обновлены путем копирования значений из другого объекта.</span><span class="sxs-lookup"><span data-stu-id="346b0-148">The current or original values of a tracked entity can be updated by copying values from another object.</span></span> <span data-ttu-id="346b0-149">Пример:</span><span class="sxs-lookup"><span data-stu-id="346b0-149">For example:</span></span>  

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

<span data-ttu-id="346b0-150">При выполнении приведенного выше кода выводится следующее:</span><span class="sxs-lookup"><span data-stu-id="346b0-150">Running the code above will print out:</span></span>  

```console
Current values:
Property Id has value 1
Property Name has value My Cool Blog

Original values:
Property Id has value 1
Property Name has value My Boring Blog
```  

<span data-ttu-id="346b0-151">Этот метод иногда используется при обновлении сущности со значениями, полученными от вызова службы или клиентом в n-уровневых приложении.</span><span class="sxs-lookup"><span data-stu-id="346b0-151">This technique is sometimes used when updating an entity with values obtained from a service call or a client in an n-tier application.</span></span> <span data-ttu-id="346b0-152">Обратите внимание, что используемый объект не должен быть того же типа, что и сущность, пока он имеет свойства, имена которых совпадают с именами сущности.</span><span class="sxs-lookup"><span data-stu-id="346b0-152">Note that the object used does not have to be of the same type as the entity so long as it has properties whose names match those of the entity.</span></span> <span data-ttu-id="346b0-153">В приведенном выше примере для обновления исходных значений используется экземпляр Блогдто.</span><span class="sxs-lookup"><span data-stu-id="346b0-153">In the example above, an instance of BlogDTO is used to update the original values.</span></span>  

<span data-ttu-id="346b0-154">Обратите внимание, что только свойства, для которых заданы разные значения при копировании из другого объекта, будут помечены как измененные.</span><span class="sxs-lookup"><span data-stu-id="346b0-154">Note that only properties that are set to different values when copied from the other object will be marked as modified.</span></span>  

## <a name="setting-current-or-original-values-from-a-dictionary"></a><span data-ttu-id="346b0-155">Установка текущих или исходных значений из словаря</span><span class="sxs-lookup"><span data-stu-id="346b0-155">Setting current or original values from a dictionary</span></span>  

<span data-ttu-id="346b0-156">Текущие или исходные значения отслеживающей сущности могут быть обновлены путем копирования значений из словаря или какой-либо другой структуры данных.</span><span class="sxs-lookup"><span data-stu-id="346b0-156">The current or original values of a tracked entity can be updated by copying values from a dictionary or some other data structure.</span></span> <span data-ttu-id="346b0-157">Пример:</span><span class="sxs-lookup"><span data-stu-id="346b0-157">For example:</span></span>  

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

<span data-ttu-id="346b0-158">Для установки исходных значений используйте свойство Оригиналвалуес вместо свойства Куррентвалуес.</span><span class="sxs-lookup"><span data-stu-id="346b0-158">Use the OriginalValues property instead of the CurrentValues property to set original values.</span></span>  

## <a name="setting-current-or-original-values-from-a-dictionary-using-property"></a><span data-ttu-id="346b0-159">Установка текущих или исходных значений из словаря с помощью свойства</span><span class="sxs-lookup"><span data-stu-id="346b0-159">Setting current or original values from a dictionary using Property</span></span>  

<span data-ttu-id="346b0-160">Альтернативой использованию Куррентвалуес или Оригиналвалуес, как показано выше, является использование метода Property для задания значения каждого свойства.</span><span class="sxs-lookup"><span data-stu-id="346b0-160">An alternative to using CurrentValues or OriginalValues as shown above is to use the Property method to set the value of each property.</span></span> <span data-ttu-id="346b0-161">Это может быть предпочтительнее, если необходимо задать значения сложных свойств.</span><span class="sxs-lookup"><span data-stu-id="346b0-161">This can be preferable when you need to set the values of complex properties.</span></span> <span data-ttu-id="346b0-162">Пример:</span><span class="sxs-lookup"><span data-stu-id="346b0-162">For example:</span></span>  

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

<span data-ttu-id="346b0-163">В примере выше сложные свойства обращаются по точечным именам.</span><span class="sxs-lookup"><span data-stu-id="346b0-163">In the example above complex properties are accessed using dotted names.</span></span> <span data-ttu-id="346b0-164">Другие способы доступа к сложным свойствам см. в двух подразделах этого раздела, особенно о сложных свойствах.</span><span class="sxs-lookup"><span data-stu-id="346b0-164">For other ways to access complex properties see the two sections later in this topic specifically about complex properties.</span></span>  

## <a name="creating-a-cloned-object-containing-current-original-or-database-values"></a><span data-ttu-id="346b0-165">Создание клонированного объекта, содержащего текущие, исходные значения или базы данных</span><span class="sxs-lookup"><span data-stu-id="346b0-165">Creating a cloned object containing current, original, or database values</span></span>  

<span data-ttu-id="346b0-166">Объект DbPropertyValues, возвращаемый из Куррентвалуес, Оригиналвалуес или Жетдатабасевалуес, можно использовать для создания клона сущности.</span><span class="sxs-lookup"><span data-stu-id="346b0-166">The DbPropertyValues object returned from CurrentValues, OriginalValues, or GetDatabaseValues can be used to create a clone of the entity.</span></span> <span data-ttu-id="346b0-167">Этот клон будет содержать значения свойств из объекта DbPropertyValues, используемого для его создания.</span><span class="sxs-lookup"><span data-stu-id="346b0-167">This clone will contain the property values from the DbPropertyValues object used to create it.</span></span> <span data-ttu-id="346b0-168">Пример:</span><span class="sxs-lookup"><span data-stu-id="346b0-168">For example:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Find(1);

    var clonedBlog = context.Entry(blog).GetDatabaseValues().ToObject();
}
```  

<span data-ttu-id="346b0-169">Обратите внимание, что возвращаемый объект не является сущностью и не отслеживается контекстом.</span><span class="sxs-lookup"><span data-stu-id="346b0-169">Note that the object returned is not the entity and is not being tracked by the context.</span></span> <span data-ttu-id="346b0-170">Возвращаемый объект также не имеет связей, настроенных для других объектов.</span><span class="sxs-lookup"><span data-stu-id="346b0-170">The returned object also does not have any relationships set to other objects.</span></span>  

<span data-ttu-id="346b0-171">Клонированный объект может быть полезен для устранения проблем, связанных с параллельными обновлениями базы данных, особенно в том случае, когда используется пользовательский интерфейс, включающий привязку данных к объектам определенного типа.</span><span class="sxs-lookup"><span data-stu-id="346b0-171">The cloned object can be useful for resolving issues related to concurrent updates to the database, especially where a UI that involves data binding to objects of a certain type is being used.</span></span>  

## <a name="getting-and-setting-the-current-or-original-values-of-complex-properties"></a><span data-ttu-id="346b0-172">Получение и установка текущих или исходных значений сложных свойств</span><span class="sxs-lookup"><span data-stu-id="346b0-172">Getting and setting the current or original values of complex properties</span></span>  

<span data-ttu-id="346b0-173">Значение всего сложного объекта можно считывать и задавать с помощью метода Property точно так же, как это может быть для примитивного свойства.</span><span class="sxs-lookup"><span data-stu-id="346b0-173">The value of an entire complex object can be read and set using the Property method just as it can be for a primitive property.</span></span> <span data-ttu-id="346b0-174">Кроме того, можно детализировать сложный объект и считывать или задавать свойства этого объекта или даже вложенный объект.</span><span class="sxs-lookup"><span data-stu-id="346b0-174">In addition you can drill down into the complex object and read or set properties of that object, or even a nested object.</span></span> <span data-ttu-id="346b0-175">Ниже приводится несколько примеров.</span><span class="sxs-lookup"><span data-stu-id="346b0-175">Here are some examples:</span></span>  

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

<span data-ttu-id="346b0-176">Чтобы получить или задать исходное значение, используйте свойство OriginalValue вместо свойства Куррентвалуе.</span><span class="sxs-lookup"><span data-stu-id="346b0-176">Use the OriginalValue property instead of the CurrentValue property to get or set an original value.</span></span>  

<span data-ttu-id="346b0-177">Обратите внимание, что для доступа к сложному свойству можно использовать либо свойство, либо метод Комплекспроперти.</span><span class="sxs-lookup"><span data-stu-id="346b0-177">Note that either the Property or the ComplexProperty method can be used to access a complex property.</span></span> <span data-ttu-id="346b0-178">Однако необходимо использовать метод Комплекспроперти, если вы хотите детализировать сложный объект с дополнительными вызовами Property или Комплекспроперти.</span><span class="sxs-lookup"><span data-stu-id="346b0-178">However, the ComplexProperty method must be used if you wish to drill down into the complex object with additional Property or ComplexProperty calls.</span></span>  

## <a name="using-dbpropertyvalues-to-access-complex-properties"></a><span data-ttu-id="346b0-179">Использование DbPropertyValues для доступа к сложным свойствам</span><span class="sxs-lookup"><span data-stu-id="346b0-179">Using DbPropertyValues to access complex properties</span></span>  

<span data-ttu-id="346b0-180">При использовании Куррентвалуес, Оригиналвалуес или Жетдатабасевалуес для получения всех текущих, исходных или значений базы данных для сущности значения любых сложных свойств возвращаются как вложенные объекты DbPropertyValues.</span><span class="sxs-lookup"><span data-stu-id="346b0-180">When you use CurrentValues, OriginalValues, or GetDatabaseValues to get all the current, original, or database values for an entity, the values of any complex properties are returned as nested DbPropertyValues objects.</span></span> <span data-ttu-id="346b0-181">Эти вложенные объекты затем можно использовать для получения значений сложного объекта.</span><span class="sxs-lookup"><span data-stu-id="346b0-181">These nested objects can then be used to get values of the complex object.</span></span> <span data-ttu-id="346b0-182">Например, следующий метод выведет на печать значения всех свойств, включая значения сложных свойств и вложенных сложных свойств.</span><span class="sxs-lookup"><span data-stu-id="346b0-182">For example, the following method will print out the values of all properties, including values of any complex properties and nested complex properties.</span></span>  

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

<span data-ttu-id="346b0-183">Для вывода всех текущих значений свойств метод будет вызываться следующим образом:</span><span class="sxs-lookup"><span data-stu-id="346b0-183">To print out all current property values the method would be called like this:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    var user = context.Users.Find("johndoe1987");

    WritePropertyValues("", context.Entry(user).CurrentValues);
}
```  
