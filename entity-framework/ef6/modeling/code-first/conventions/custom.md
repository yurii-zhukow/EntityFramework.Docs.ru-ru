---
title: Пользовательский код соглашения First - EF6
author: divega
ms.date: 2016-10-23
ms.assetid: dd2bdbd9-ae9e-470a-aeb8-d0ba160499b7
ms.openlocfilehash: 79450790c6d3c8ce7fad209e3946e81d3fad4b75
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/27/2018
ms.locfileid: "42995832"
---
# <a name="custom-code-first-conventions"></a><span data-ttu-id="b3878-102">Первый соглашения о написании пользовательского кода</span><span class="sxs-lookup"><span data-stu-id="b3878-102">Custom Code First Conventions</span></span>
> [!NOTE]
> <span data-ttu-id="b3878-103">**Только в EF6 и более поздних версиях**. Функции, API и другие возможности, описанные на этой странице, появились в Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="b3878-103">**EF6 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 6.</span></span> <span data-ttu-id="b3878-104">При использовании более ранней версии могут быть неприменимы некоторые или все сведения.</span><span class="sxs-lookup"><span data-stu-id="b3878-104">If you are using an earlier version, some or all of the information does not apply.</span></span>

<span data-ttu-id="b3878-105">При использовании Code First модели вычисляется на основе классов с помощью набора соглашений.</span><span class="sxs-lookup"><span data-stu-id="b3878-105">When using Code First your model is calculated from your classes using a set of conventions.</span></span> <span data-ttu-id="b3878-106">Значение по умолчанию [первый соглашения о коде](~/ef6/modeling/code-first/conventions/built-in.md) определить, как и в которых свойство становится первичный ключ сущности, имя таблицы, сопоставляет сущности и какие точность и масштаб столбца decimal имеет по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="b3878-106">The default [Code First Conventions](~/ef6/modeling/code-first/conventions/built-in.md) determine things like which property becomes the primary key of an entity, the name of the table an entity maps to, and what precision and scale a decimal column has by default.</span></span>

<span data-ttu-id="b3878-107">Иногда эти соглашения по умолчанию не идеально подходят для вашей модели, и вам нужно решить их, настроив множество отдельных сущностей, с помощью заметок к данным или Fluent API.</span><span class="sxs-lookup"><span data-stu-id="b3878-107">Sometimes these default conventions are not ideal for your model, and you have to work around them by configuring many individual entities using Data Annotations or the Fluent API.</span></span> <span data-ttu-id="b3878-108">Пользовательские соглашения о коде первый позволяют определять собственные соглашения, которые предоставляют значения конфигурации по умолчанию для модели.</span><span class="sxs-lookup"><span data-stu-id="b3878-108">Custom Code First Conventions let you define your own conventions that provide configuration defaults for your model.</span></span> <span data-ttu-id="b3878-109">В этом пошаговом руководстве мы рассмотрим различные типы соглашения об именовании и создание каждого из них.</span><span class="sxs-lookup"><span data-stu-id="b3878-109">In this walkthrough, we will explore the different types of custom conventions and how to create each of them.</span></span>


## <a name="model-based-conventions"></a><span data-ttu-id="b3878-110">Правила на основе моделей</span><span class="sxs-lookup"><span data-stu-id="b3878-110">Model-Based Conventions</span></span>

<span data-ttu-id="b3878-111">В этой статье описываются API-интерфейса DbModelBuilder для соглашения об именовании.</span><span class="sxs-lookup"><span data-stu-id="b3878-111">This page covers the DbModelBuilder API for custom conventions.</span></span> <span data-ttu-id="b3878-112">Этот API должно быть достаточно для создания большинства соглашения об именовании.</span><span class="sxs-lookup"><span data-stu-id="b3878-112">This API should be sufficient for authoring most custom conventions.</span></span> <span data-ttu-id="b3878-113">Тем не менее есть также возможность создавать модели с использованием соглашений - соглашения, которые управляют окончательная версия модели, после ее создания — для обработки более сложных сценариев.</span><span class="sxs-lookup"><span data-stu-id="b3878-113">However, there is also the ability to author model-based conventions - conventions that manipulate the final model once it is created - to handle advanced scenarios.</span></span> <span data-ttu-id="b3878-114">Дополнительные сведения см. в разделе [модели с использованием соглашений](~/ef6/modeling/code-first/conventions/model.md).</span><span class="sxs-lookup"><span data-stu-id="b3878-114">For more information, see [Model-Based Conventions](~/ef6/modeling/code-first/conventions/model.md).</span></span>

 

## <a name="our-model"></a><span data-ttu-id="b3878-115">Наша модель</span><span class="sxs-lookup"><span data-stu-id="b3878-115">Our Model</span></span>

<span data-ttu-id="b3878-116">Начнем с определения простой модели, который можно использовать с помощью наших соглашений.</span><span class="sxs-lookup"><span data-stu-id="b3878-116">Let's start by defining a simple model that we can use with our conventions.</span></span> <span data-ttu-id="b3878-117">Добавьте следующие классы в проект.</span><span class="sxs-lookup"><span data-stu-id="b3878-117">Add the following classes to your project.</span></span>

``` csharp
    using System;
    using System.Collections.Generic;
    using System.Data.Entity;
    using System.Linq;

    public class ProductContext : DbContext
    {
        static ProductContext()
        {
            Database.SetInitializer(new DropCreateDatabaseIfModelChanges<ProductContext>());
        }

        public DbSet<Product> Products { get; set; }
    }

    public class Product
    {
        public int Key { get; set; }
        public string Name { get; set; }
        public decimal? Price { get; set; }
        public DateTime? ReleaseDate { get; set; }
        public ProductCategory Category { get; set; }
    }

    public class ProductCategory
    {
        public int Key { get; set; }
        public string Name { get; set; }
        public List<Product> Products { get; set; }
    }
```

 

## <a name="introducing-custom-conventions"></a><span data-ttu-id="b3878-118">Знакомство с соглашения об именовании</span><span class="sxs-lookup"><span data-stu-id="b3878-118">Introducing Custom Conventions</span></span>

<span data-ttu-id="b3878-119">Давайте напишем соглашение, которое настраивает любое свойство с именем ключа в качестве первичного ключа для своего типа сущности.</span><span class="sxs-lookup"><span data-stu-id="b3878-119">Let’s write a convention that configures any property named Key to be the primary key for its entity type.</span></span>

<span data-ttu-id="b3878-120">Соглашения о включены построитель модели, к которому можно получить путем переопределения OnModelCreating в контексте.</span><span class="sxs-lookup"><span data-stu-id="b3878-120">Conventions are enabled on the model builder, which can be accessed by overriding OnModelCreating in the context.</span></span> <span data-ttu-id="b3878-121">Обновите класс ProductContext следующим образом:</span><span class="sxs-lookup"><span data-stu-id="b3878-121">Update the ProductContext class as follows:</span></span>

``` csharp
    public class ProductContext : DbContext
    {
        static ProductContext()
        {
            Database.SetInitializer(new DropCreateDatabaseIfModelChanges<ProductContext>());
        }

        public DbSet<Product> Products { get; set; }

        protected override void OnModelCreating(DbModelBuilder modelBuilder)
        {
            modelBuilder.Properties()
                        .Where(p => p.Name == "Key")
                        .Configure(p => p.IsKey());
        }
    }
```

<span data-ttu-id="b3878-122">Теперь, любое свойство в нашей модели, с именем ключа будет настроен в качестве первичного ключа сущности, независимо от его частью.</span><span class="sxs-lookup"><span data-stu-id="b3878-122">Now, any property in our model named Key will be configured as the primary key of whatever entity its part of.</span></span>

<span data-ttu-id="b3878-123">Можно также сделать соглашения более точным, фильтруя данные по тип свойства, который будет использоваться для настройки:</span><span class="sxs-lookup"><span data-stu-id="b3878-123">We could also make our conventions more specific by filtering on the type of property that we are going to configure:</span></span>

``` csharp
    modelBuilder.Properties<int>()
                .Where(p => p.Name == "Key")
                .Configure(p => p.IsKey());
```

<span data-ttu-id="b3878-124">Будут настроены все свойства, называемые ключ основного ключа их сущности, но только в том случае, если это целое число.</span><span class="sxs-lookup"><span data-stu-id="b3878-124">This will configure all properties called Key to be the primary key of their entity, but only if they are an integer.</span></span>

<span data-ttu-id="b3878-125">Интересной особенностью метод IsKey является его аддитивный характер.</span><span class="sxs-lookup"><span data-stu-id="b3878-125">An interesting feature of the IsKey method is that it is additive.</span></span> <span data-ttu-id="b3878-126">То есть, если вы вызываете IsKey на несколько свойств, и все они станут частью составного ключа.</span><span class="sxs-lookup"><span data-stu-id="b3878-126">Which means that if you call IsKey on multiple properties and they will all become part of a composite key.</span></span> <span data-ttu-id="b3878-127">Единственным условием для этого является то, что при указании нескольких свойств для ключа также необходимо указать, заказ для этих свойств.</span><span class="sxs-lookup"><span data-stu-id="b3878-127">The one caveat for this is that when you specify multiple properties for a key you must also specify an order for those properties.</span></span> <span data-ttu-id="b3878-128">Это можно сделать, вызвав HasColumnOrder, аналогичный метод:</span><span class="sxs-lookup"><span data-stu-id="b3878-128">You can do this by calling the HasColumnOrder method like below:</span></span>

``` csharp
    modelBuilder.Properties<int>()
                .Where(x => x.Name == "Key")
                .Configure(x => x.IsKey().HasColumnOrder(1));

    modelBuilder.Properties()
                .Where(x => x.Name == "Name")
                .Configure(x => x.IsKey().HasColumnOrder(2));
```

<span data-ttu-id="b3878-129">Этот код будет настроить типы в нашей модели иметь составной ключ, состоящий из столбца Key int и строкового имени столбца.</span><span class="sxs-lookup"><span data-stu-id="b3878-129">This code will configure the types in our model to have a composite key consisting of the int Key column and the string Name column.</span></span> <span data-ttu-id="b3878-130">Если просмотреть модель в конструкторе оно выглядело следующим образом:</span><span class="sxs-lookup"><span data-stu-id="b3878-130">If we view the model in the designer it would look like this:</span></span>

![compositeKey](~/ef6/media/compositekey.png)

<span data-ttu-id="b3878-132">Другой пример соглашений свойство — настроить все свойства даты и времени в моей модели для сопоставления с типом datetime2 в SQL Server вместо даты и времени.</span><span class="sxs-lookup"><span data-stu-id="b3878-132">Another example of property conventions is to configure all DateTime properties in my model to map to the datetime2 type in SQL Server instead of datetime.</span></span> <span data-ttu-id="b3878-133">Это можно сделать с помощью следующих:</span><span class="sxs-lookup"><span data-stu-id="b3878-133">You can achieve this with the following:</span></span>

``` csharp
    modelBuilder.Properties<DateTime>()
                .Configure(c => c.HasColumnType("datetime2"));
```

 

## <a name="convention-classes"></a><span data-ttu-id="b3878-134">Соглашение о классах</span><span class="sxs-lookup"><span data-stu-id="b3878-134">Convention Classes</span></span>

<span data-ttu-id="b3878-135">Определение соглашения еще один способ — использовать соглашение о класс для инкапсуляции соглашению об.</span><span class="sxs-lookup"><span data-stu-id="b3878-135">Another way of defining conventions is to use a Convention Class to encapsulate your convention.</span></span> <span data-ttu-id="b3878-136">При использовании класса соглашение о создании типа, наследуемого от класса соглашение в пространстве имен System.Data.Entity.ModelConfiguration.Conventions.</span><span class="sxs-lookup"><span data-stu-id="b3878-136">When using a Convention Class then you create a type that inherits from the Convention class in the System.Data.Entity.ModelConfiguration.Conventions namespace.</span></span>

<span data-ttu-id="b3878-137">Мы можем создать класс соглашение об с datetime2 соглашение, которое мы продемонстрировали ранее, сделав следующее:</span><span class="sxs-lookup"><span data-stu-id="b3878-137">We can create a Convention Class with the datetime2 convention that we showed earlier by doing the following:</span></span>

``` csharp
    public class DateTime2Convention : Convention
    {
        public DateTime2Convention()
        {
            this.Properties<DateTime>()
                .Configure(c => c.HasColumnType("datetime2"));        
        }
    }
```

<span data-ttu-id="b3878-138">Чтобы сообщить EF, чтобы использовать это соглашение, добавить его в коллекцию соглашений в OnModelCreating, что если вы выполняли с пошаговым руководством, будет выглядеть следующим образом:</span><span class="sxs-lookup"><span data-stu-id="b3878-138">To tell EF to use this convention you add it to the Conventions collection in OnModelCreating, which if you’ve been following along with the walkthrough will look like this:</span></span>

``` csharp
    protected override void OnModelCreating(DbModelBuilder modelBuilder)
    {
        modelBuilder.Properties<int>()
                    .Where(p => p.Name.EndsWith("Key"))
                    .Configure(p => p.IsKey());

        modelBuilder.Conventions.Add(new DateTime2Convention());
    }
```

<span data-ttu-id="b3878-139">Как вы видите, мы добавим экземпляр нашей соглашение в коллекцию соглашений.</span><span class="sxs-lookup"><span data-stu-id="b3878-139">As you can see we add an instance of our convention to the conventions collection.</span></span> <span data-ttu-id="b3878-140">Наследование от соглашение предоставляет удобный способ группировки и совместное использование соглашения несколькими командами или проектов.</span><span class="sxs-lookup"><span data-stu-id="b3878-140">Inheriting from Convention provides a convenient way of grouping and sharing conventions across teams or projects.</span></span> <span data-ttu-id="b3878-141">Например, можно создать общий набор соглашений, что все организации проекты, использующие библиотеки классов.</span><span class="sxs-lookup"><span data-stu-id="b3878-141">You could, for example, have a class library with a common set of conventions that all of your organizations projects use.</span></span>

 

## <a name="custom-attributes"></a><span data-ttu-id="b3878-142">Настраиваемые атрибуты</span><span class="sxs-lookup"><span data-stu-id="b3878-142">Custom Attributes</span></span>

<span data-ttu-id="b3878-143">Чтобы включить новые атрибуты для использования при настройке модели — еще одно отличное применение соглашений.</span><span class="sxs-lookup"><span data-stu-id="b3878-143">Another great use of conventions is to enable new attributes to be used when configuring a model.</span></span> <span data-ttu-id="b3878-144">Чтобы проиллюстрировать это, давайте создадим атрибут, который можно использовать для пометки свойств строки как Юникод.</span><span class="sxs-lookup"><span data-stu-id="b3878-144">To illustrate this, let’s create an attribute that we can use to mark String properties as non-Unicode.</span></span>

``` csharp
    [AttributeUsage(AttributeTargets.Property, AllowMultiple = false)]
    public class NonUnicode : Attribute
    {
    }
```

<span data-ttu-id="b3878-145">Теперь давайте создадим соглашение о этот атрибут применяется к нашей модели:</span><span class="sxs-lookup"><span data-stu-id="b3878-145">Now, let’s create a convention to apply this attribute to our model:</span></span>

``` csharp
    modelBuilder.Properties()
                .Where(x => x.GetCustomAttributes(false).OfType<NonUnicode>().Any())
                .Configure(c => c.IsUnicode(false));
```

<span data-ttu-id="b3878-146">Данное соглашение мы можно добавить атрибут NonUnicode к любому из наших свойства строки, что означает столбец в базе данных будет храниться как varchar, а не nvarchar.</span><span class="sxs-lookup"><span data-stu-id="b3878-146">With this convention we can add the NonUnicode attribute to any of our string properties, which means the column in the database will be stored as varchar instead of nvarchar.</span></span>

<span data-ttu-id="b3878-147">Следует отметить о это соглашение о том, что если поместить атрибут NonUnicode на ничего, кроме строковое свойство, а затем он вызовет исключение.</span><span class="sxs-lookup"><span data-stu-id="b3878-147">One thing to note about this convention is that if you put the NonUnicode attribute on anything other than a string property then it will throw an exception.</span></span> <span data-ttu-id="b3878-148">Это связано с IsUnicode невозможно настроить для любого типа, кроме строки.</span><span class="sxs-lookup"><span data-stu-id="b3878-148">It does this because you cannot configure IsUnicode on any type other than a string.</span></span> <span data-ttu-id="b3878-149">В этом случае после этого можно выполнять соглашению об более точным, позволяя отфильтровывает все, что не является строкой.</span><span class="sxs-lookup"><span data-stu-id="b3878-149">If this happens, then you can make your convention more specific, so that it filters out anything that isn’t a string.</span></span>

<span data-ttu-id="b3878-150">Хотя выше соглашение работает для определения настраиваемых атрибутов нет другого API, который может быть гораздо проще в использовании, особенно если вы хотите использовать свойства класса атрибутов.</span><span class="sxs-lookup"><span data-stu-id="b3878-150">While the above convention works for defining custom attributes there is another API that can be much easier to use, especially when you want to use properties from the attribute class.</span></span>

<span data-ttu-id="b3878-151">В этом примере мы собираемся обновить наш атрибут и измените его атрибут IsUnicode, чтобы он выглядел следующим образом:</span><span class="sxs-lookup"><span data-stu-id="b3878-151">For this example we are going to update our attribute and change it to an IsUnicode attribute, so it looks like this:</span></span>

``` csharp
    [AttributeUsage(AttributeTargets.Property, AllowMultiple = false)]
    internal class IsUnicode : Attribute
    {
        public bool Unicode { get; set; }

        public IsUnicode(bool isUnicode)
        {
            Unicode = isUnicode;
        }
    }
```

<span data-ttu-id="b3878-152">Получив это, можно установить логическое значение для наших атрибут, чтобы понять в соответствии с соглашением, ли свойство должно быть Юникода.</span><span class="sxs-lookup"><span data-stu-id="b3878-152">Once we have this, we can set a bool on our attribute to tell the convention whether or not a property should be Unicode.</span></span> <span data-ttu-id="b3878-153">Это можно делать в соглашение, которое уже имеется, обратившись к ClrProperty класса конфигурации следующим образом:</span><span class="sxs-lookup"><span data-stu-id="b3878-153">We could do this in the convention we have already by accessing the ClrProperty of the configuration class like this:</span></span>

``` csharp
    modelBuilder.Properties()
                .Where(x => x.GetCustomAttributes(false).OfType<IsUnicode>().Any())
                .Configure(c => c.IsUnicode(c.ClrPropertyInfo.GetCustomAttribute<IsUnicode>().Unicode));
```

<span data-ttu-id="b3878-154">Это достаточно просто, но есть более краткий способ сделать это с помощью Having метод соглашений API.</span><span class="sxs-lookup"><span data-stu-id="b3878-154">This is easy enough, but there is a more succinct way of achieving this by using the Having method of the conventions API.</span></span> <span data-ttu-id="b3878-155">Having, метод имеет параметр типа Func&lt;PropertyInfo, T&gt; , принимающий класс PropertyInfo так же, как Where метод, но должен вернуть объект.</span><span class="sxs-lookup"><span data-stu-id="b3878-155">The Having method has a parameter of type Func&lt;PropertyInfo, T&gt; which accepts the PropertyInfo the same as the Where method, but is expected to return an object.</span></span> <span data-ttu-id="b3878-156">Если возвращаемый объект имеет значение null, то свойство не будет настроен, что может сделать возможной фильтрацию свойств с ним так же, как Where, но он отличается тем, что он также помещается возвращаемый объект и передать его в метод Configure.</span><span class="sxs-lookup"><span data-stu-id="b3878-156">If the returned object is null then the property will not be configured, which means you can filter out properties with it just like Where, but it is different in that it will also capture the returned object and pass it to the Configure method.</span></span> <span data-ttu-id="b3878-157">Это работает следующим образом:</span><span class="sxs-lookup"><span data-stu-id="b3878-157">This works like the following:</span></span>

``` csharp
    modelBuilder.Properties()
                .Having(x =>x.GetCustomAttributes(false).OfType<IsUnicode>().FirstOrDefault())
                .Configure((config, att) => config.IsUnicode(att.Unicode));
```

<span data-ttu-id="b3878-158">Пользовательские атрибуты не единственная причина для использования Having метод, полезно в любом месте, необходимо делать выводы о том, что фильтрации по при настройке типами или свойствами.</span><span class="sxs-lookup"><span data-stu-id="b3878-158">Custom attributes are not the only reason to use the Having method, it is useful anywhere that you need to reason about something that you are filtering on when configuring your types or properties.</span></span>

 

## <a name="configuring-types"></a><span data-ttu-id="b3878-159">Настройка типов</span><span class="sxs-lookup"><span data-stu-id="b3878-159">Configuring Types</span></span>

<span data-ttu-id="b3878-160">До сих всех соглашений были для свойств, но существует еще одна область API соглашений для настройки типов в модели.</span><span class="sxs-lookup"><span data-stu-id="b3878-160">So far all of our conventions have been for properties, but there is another area of the conventions API for configuring the types in your model.</span></span> <span data-ttu-id="b3878-161">Процесс выполняется аналогично соглашения об именах, который мы видели в данный момент, но параметры настройки внутри будет объекты вместо свойства уровня.</span><span class="sxs-lookup"><span data-stu-id="b3878-161">The experience is similar to the conventions we have seen so far, but the options inside configure will be at the entity instead of property level.</span></span>

<span data-ttu-id="b3878-162">Среди прочего, соглашения об уровне типов может быть очень полезно при изменении таблицы соглашение об именовании, сопоставляемый существующую схему, отличную от EF по умолчанию или создать новую базу данных с различных соглашения об именовании.</span><span class="sxs-lookup"><span data-stu-id="b3878-162">One of the things that Type level conventions can be really useful for is changing the table naming convention, either to map to an existing schema that differs from the EF default or to create a new database with a different naming convention.</span></span> <span data-ttu-id="b3878-163">Для этого необходимо сначала метод, который может принимать TypeInfo типа в нашей модели и возвращать имя таблицы для этого типа, которое должно быть:</span><span class="sxs-lookup"><span data-stu-id="b3878-163">To do this we first need a method that can accept the TypeInfo for a type in our model and return what the table name for that type should be:</span></span>

``` csharp
    private string GetTableName(Type type)
    {
        var result = Regex.Replace(type.Name, ".[A-Z]", m => m.Value[0] + "_" + m.Value[1]);

        return result.ToLower();
    }
```

<span data-ttu-id="b3878-164">Этот метод принимает значение типа и возвращает строку, которая используется нижний регистр с символами подчеркивания, а не CamelCase.</span><span class="sxs-lookup"><span data-stu-id="b3878-164">This method takes a type and returns a string that uses lower case with underscores instead of CamelCase.</span></span> <span data-ttu-id="b3878-165">В нашей модели это означает, что таблица с именем продукта будет сопоставлено класс ProductCategory\_категории вместо ProductCategories.</span><span class="sxs-lookup"><span data-stu-id="b3878-165">In our model this means that the ProductCategory class will be mapped to a table called product\_category instead of ProductCategories.</span></span>

<span data-ttu-id="b3878-166">После получения этого метода можно обращаться к нему в соглашении следующим образом:</span><span class="sxs-lookup"><span data-stu-id="b3878-166">Once we have that method we can call it in a convention like this:</span></span>

``` csharp
    modelBuilder.Types()
                .Configure(c => c.ToTable(GetTableName(c.ClrType)));
```

<span data-ttu-id="b3878-167">Это соглашение настраивает каждый тип в нашей модели, которое будет сопоставлено имя таблицы, которое возвращается из нашего метода GetTableName.</span><span class="sxs-lookup"><span data-stu-id="b3878-167">This convention configures every type in our model to map to the table name that is returned from our GetTableName method.</span></span> <span data-ttu-id="b3878-168">Это соглашение эквивалентен вызову totable-метод для каждой сущности в модели с помощью Fluent API.</span><span class="sxs-lookup"><span data-stu-id="b3878-168">This convention is the equivalent to calling the ToTable method for each entity in the model using the Fluent API.</span></span>

<span data-ttu-id="b3878-169">Следует заметить, об этом, что при вызове totable-EF будет иметь строку, указанных в качестве точное имя таблицы, без каких-либо преобразования во множественную форму, что обычно делается при определении имен таблиц.</span><span class="sxs-lookup"><span data-stu-id="b3878-169">One thing to note about this is that when you call ToTable EF will take the string that you provide as the exact table name, without any of the pluralization that it would normally do when determining table names.</span></span> <span data-ttu-id="b3878-170">Вот почему имя таблицы из наших соглашение — продукт\_категории вместо продукта\_категории.</span><span class="sxs-lookup"><span data-stu-id="b3878-170">This is why the table name from our convention is product\_category instead of product\_categories.</span></span> <span data-ttu-id="b3878-171">Чтобы устранить, в нашем соглашении, делая вызов к службе преобразования во множественную форму, сами.</span><span class="sxs-lookup"><span data-stu-id="b3878-171">We can resolve that in our convention by making a call to the pluralization service ourselves.</span></span>

<span data-ttu-id="b3878-172">В следующем коде мы будем использовать [разрешение зависимостей](~/ef6/fundamentals/configuring/dependency-resolution.md) компонент, добавленный в EF6, обращение к службе преобразования во множественную форму, приходилось использовать EF и pluralize наших имя таблицы.</span><span class="sxs-lookup"><span data-stu-id="b3878-172">In the following code we will use the [Dependency Resolution](~/ef6/fundamentals/configuring/dependency-resolution.md) feature added in EF6 to retrieve the pluralization service that EF would have used and pluralize our table name.</span></span>

``` csharp
    private string GetTableName(Type type)
    {
        var pluralizationService = DbConfiguration.DependencyResolver.GetService<IPluralizationService>();

        var result = pluralizationService.Pluralize(type.Name);

        result = Regex.Replace(result, ".[A-Z]", m => m.Value[0] + "_" + m.Value[1]);

        return result.ToLower();
    }
```

> [!NOTE]
> <span data-ttu-id="b3878-173">Универсальная версия GetService является методом расширения в пространстве имен System.Data.Entity.Infrastructure.DependencyResolution, необходимо добавить с помощью инструкции к контексту, чтобы их использовать.</span><span class="sxs-lookup"><span data-stu-id="b3878-173">The generic version of GetService is an extension method in the System.Data.Entity.Infrastructure.DependencyResolution namespace, you will need to add a using statement to your context in order to use it.</span></span>

### <a name="totable-and-inheritance"></a><span data-ttu-id="b3878-174">Totable- и наследование</span><span class="sxs-lookup"><span data-stu-id="b3878-174">ToTable and Inheritance</span></span>

<span data-ttu-id="b3878-175">Еще один важный аспект ToTable том, что если вы явным образом сопоставить тип для данной таблицы, а затем его можно изменить стратегию сопоставления, который будет использовать EF.</span><span class="sxs-lookup"><span data-stu-id="b3878-175">Another important aspect of ToTable is that if you explicitly map a type to a given table, then you can alter the mapping strategy that EF will use.</span></span> <span data-ttu-id="b3878-176">Если вы вызываете метод ToTable для каждого типа в иерархии наследования, передавая имя типа как имя таблицы, как это делалось выше, затем мы изменим стратегию сопоставления по умолчанию таблица на иерархию (TPH) таблица на тип (TPT).</span><span class="sxs-lookup"><span data-stu-id="b3878-176">If you call ToTable for every type in an inheritance hierarchy, passing the type name as the name of the table like we did above, then you will change the default Table-Per-Hierarchy (TPH) mapping strategy to Table-Per-Type (TPT).</span></span> <span data-ttu-id="b3878-177">Лучший способ описания — whith конкретный пример:</span><span class="sxs-lookup"><span data-stu-id="b3878-177">The best way to describe this is whith a concrete example:</span></span>

``` csharp
    public class Employee
    {
        public int Id { get; set; }
        public string Name { get; set; }
    }

    public class Manager : Employee
    {
        public string SectionManaged { get; set; }
    }
```

<span data-ttu-id="b3878-178">По умолчанию сотрудника и руководителя сопоставляются с той же таблицей (сотрудники) в базе данных.</span><span class="sxs-lookup"><span data-stu-id="b3878-178">By default both employee and manager are mapped to the same table (Employees) in the database.</span></span> <span data-ttu-id="b3878-179">Таблица будет содержать сотрудники и менеджеры со столбцом дискриминатора, который сообщит, какой тип экземпляра хранится в каждой строке.</span><span class="sxs-lookup"><span data-stu-id="b3878-179">The table will contain both employees and managers with a discriminator column that will tell you what type of instance is stored in each row.</span></span> <span data-ttu-id="b3878-180">Такое сопоставление TPH, так как для одной таблицы для иерархии.</span><span class="sxs-lookup"><span data-stu-id="b3878-180">This is TPH mapping as there is a single table for the hierarchy.</span></span> <span data-ttu-id="b3878-181">Тем не менее если вызвать метод ToTable в обоих classe затем каждого типа будет вместо этого можно сопоставить с собственную таблицу, а также называется TPT, так как каждый тип имеет собственную таблицу.</span><span class="sxs-lookup"><span data-stu-id="b3878-181">However, if you call ToTable on both classe then each type will instead be mapped to its own table, also known as TPT since each type has its own table.</span></span>

``` csharp
    modelBuilder.Types()
                .Configure(c=>c.ToTable(c.ClrType.Name));
```

<span data-ttu-id="b3878-182">Приведенный выше код будет сопоставлен структуру таблицы, которая выглядит следующим образом:</span><span class="sxs-lookup"><span data-stu-id="b3878-182">The code above will map to a table structure that looks like the following:</span></span>

![tptExample](~/ef6/media/tptexample.jpg)

<span data-ttu-id="b3878-184">Можно избежать этого и поддерживать TPH сопоставление по умолчанию, несколькими способами:</span><span class="sxs-lookup"><span data-stu-id="b3878-184">You can avoid this, and maintain the default TPH mapping, in a couple ways:</span></span>

1.  <span data-ttu-id="b3878-185">Вызовите метод ToTable с тем же именем таблицы, для каждого типа в иерархии.</span><span class="sxs-lookup"><span data-stu-id="b3878-185">Call ToTable with the same table name for each type in the hierarchy.</span></span>
2.  <span data-ttu-id="b3878-186">Вызовите метод ToTable только на базовый класс для иерархии, в нашем примере, которая была бы сотрудника.</span><span class="sxs-lookup"><span data-stu-id="b3878-186">Call ToTable only on the base class of the hierarchy, in our example that would be employee.</span></span>

 

## <a name="execution-order"></a><span data-ttu-id="b3878-187">Порядок выполнения</span><span class="sxs-lookup"><span data-stu-id="b3878-187">Execution Order</span></span>

<span data-ttu-id="b3878-188">Соглашения о работают в виде последнего wins, так же, как Fluent API.</span><span class="sxs-lookup"><span data-stu-id="b3878-188">Conventions operate in a last wins manner, the same as the Fluent API.</span></span> <span data-ttu-id="b3878-189">Это означает, что при написании два соглашения, настройте один и тот же параметр этого свойства, а затем последнее из них для выполнения wins.</span><span class="sxs-lookup"><span data-stu-id="b3878-189">What this means is that if you write two conventions that configure the same option of the same property, then the last one to execute wins.</span></span> <span data-ttu-id="b3878-190">Например в приведенном ниже коде Максимальная длина для всех строк имеет значение 500, но мы затем настройте все свойства «Name» в модели, чтобы максимальная длина 250.</span><span class="sxs-lookup"><span data-stu-id="b3878-190">As an example, in the code below the max length of all strings is set to 500 but we then configure all properties called Name in the model to have a max length of 250.</span></span>

``` csharp
    modelBuilder.Properties<string>()
                .Configure(c => c.HasMaxLength(500));

    modelBuilder.Properties<string>()
                .Where(x => x.Name == "Name")
                .Configure(c => c.HasMaxLength(250));
```

<span data-ttu-id="b3878-191">Так как соглашение об установке максимальной длины до 250 после того, который задает все строки до 500, все свойства «Name» в нашей модели будет иметь MaxLength 250 при других строк, например, описания, составит 500.</span><span class="sxs-lookup"><span data-stu-id="b3878-191">Because the convention to set max length to 250 is after the one that sets all strings to 500, all the properties called Name in our model will have a MaxLength of 250 while any other strings, such as descriptions, would be 500.</span></span> <span data-ttu-id="b3878-192">Подобное использование соглашения означает, что может предоставить общие соглашения для типов или свойств в модели и затем переопределить их подмножества, которые отличаются.</span><span class="sxs-lookup"><span data-stu-id="b3878-192">Using conventions in this way means that you can provide a general convention for types or properties in your model and then overide them for subsets that are different.</span></span>

<span data-ttu-id="b3878-193">Fluent API и заметки к данным может также использоваться для переопределения соглашения в конкретных случаях.</span><span class="sxs-lookup"><span data-stu-id="b3878-193">The Fluent API and Data Annotations can also be used to override a convention in specific cases.</span></span> <span data-ttu-id="b3878-194">В примере выше если бы мы использовали Fluent API для задания Максимальная длина свойства затем было бы поместить ее до или после соглашение, так как более конкретный Fluent API бьет соглашение по дополнительной конфигурации.</span><span class="sxs-lookup"><span data-stu-id="b3878-194">In our example above if we had used the Fluent API to set the max length of a property then we could have put it before or after the convention, because the more specific Fluent API will win over the more general Configuration Convention.</span></span>

 

## <a name="built-in-conventions"></a><span data-ttu-id="b3878-195">Встроенные соглашения</span><span class="sxs-lookup"><span data-stu-id="b3878-195">Built-in Conventions</span></span>

<span data-ttu-id="b3878-196">Так как соглашения об именовании может повлиять на соглашения Code First по умолчанию, может быть полезно добавить соглашения для выполнения до или после другим соглашением.</span><span class="sxs-lookup"><span data-stu-id="b3878-196">Because custom conventions could be affected by the default Code First conventions, it can be useful to add conventions to run before or after another convention.</span></span> <span data-ttu-id="b3878-197">Для этого можно использовать методы AddBefore и AddAfter коллекции соглашения в вашем производном DbContext.</span><span class="sxs-lookup"><span data-stu-id="b3878-197">To do this you can use the AddBefore and AddAfter methods of the Conventions collection on your derived DbContext.</span></span> <span data-ttu-id="b3878-198">Добавить приведенный ниже класс соглашение, созданную ранее будет выполняться перед встроенные в соглашении обнаруживать ключ.</span><span class="sxs-lookup"><span data-stu-id="b3878-198">The following code would add the convention class we created earlier so that it will run before the built in key discovery convention.</span></span>

``` csharp
    modelBuilder.Conventions.AddBefore<IdKeyDiscoveryConvention>(new DateTime2Convention());
```

<span data-ttu-id="b3878-199">Это будет наиболее полезны при добавлении соглашения, которые должны выполняться до или после встроенных правил, список встроенных соглашений можно найти здесь: [пространства имен System.Data.Entity.ModelConfiguration.Conventions](https://msdn.microsoft.com/library/system.data.entity.modelconfiguration.conventions.aspx) .</span><span class="sxs-lookup"><span data-stu-id="b3878-199">This is going to be of the most use when adding conventions that need to run before or after the built in conventions, a list of the built in conventions can be found here: [System.Data.Entity.ModelConfiguration.Conventions Namespace](https://msdn.microsoft.com/library/system.data.entity.modelconfiguration.conventions.aspx).</span></span>

<span data-ttu-id="b3878-200">Можно также удалить соглашения, которые необходимо применить к модели.</span><span class="sxs-lookup"><span data-stu-id="b3878-200">You can also remove conventions that you do not want applied to your model.</span></span> <span data-ttu-id="b3878-201">Чтобы удалить соглашение, используйте метод Remove.</span><span class="sxs-lookup"><span data-stu-id="b3878-201">To remove a convention, use the Remove method.</span></span> <span data-ttu-id="b3878-202">Ниже приведен пример удаления PluralizingTableNameConvention.</span><span class="sxs-lookup"><span data-stu-id="b3878-202">Here is an example of removing the PluralizingTableNameConvention.</span></span>

``` csharp
    protected override void OnModelCreating(DbModelBuilder modelBuilder)
    {
        modelBuilder.Conventions.Remove<PluralizingTableNameConvention>();
    }
```
