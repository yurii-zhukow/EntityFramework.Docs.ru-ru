---
title: Соглашения о настраиваемых Code Firstах — EF6
author: divega
ms.date: 10/23/2016
ms.assetid: dd2bdbd9-ae9e-470a-aeb8-d0ba160499b7
ms.openlocfilehash: cfd7f7cad532dca5227793c04d7d91e977ea5e4e
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/06/2020
ms.locfileid: "78415895"
---
# <a name="custom-code-first-conventions"></a><span data-ttu-id="81f89-102">Пользовательские соглашения Code First</span><span class="sxs-lookup"><span data-stu-id="81f89-102">Custom Code First Conventions</span></span>
> [!NOTE]
> <span data-ttu-id="81f89-103">**Только в EF6 и более поздних версиях**. Функции, API и другие возможности, описанные на этой странице, появились в Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="81f89-103">**EF6 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 6.</span></span> <span data-ttu-id="81f89-104">При использовании более ранней версии могут быть неприменимы некоторые или все сведения.</span><span class="sxs-lookup"><span data-stu-id="81f89-104">If you are using an earlier version, some or all of the information does not apply.</span></span>

<span data-ttu-id="81f89-105">При использовании Code First модель вычисляется из классов с помощью набора соглашений.</span><span class="sxs-lookup"><span data-stu-id="81f89-105">When using Code First your model is calculated from your classes using a set of conventions.</span></span> <span data-ttu-id="81f89-106">[Соглашения Code First](~/ef6/modeling/code-first/conventions/built-in.md) по умолчанию определяют, как свойство станет первичным ключом сущности, имя таблицы, которой сопоставлена сущность, и точность и Масштаб десятичного столбца по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="81f89-106">The default [Code First Conventions](~/ef6/modeling/code-first/conventions/built-in.md) determine things like which property becomes the primary key of an entity, the name of the table an entity maps to, and what precision and scale a decimal column has by default.</span></span>

<span data-ttu-id="81f89-107">Иногда эти соглашения по умолчанию не идеально подходят для вашей модели, и их необходимо обойти, настроив множество отдельных сущностей с помощью заметок к данным или API-интерфейса Fluent.</span><span class="sxs-lookup"><span data-stu-id="81f89-107">Sometimes these default conventions are not ideal for your model, and you have to work around them by configuring many individual entities using Data Annotations or the Fluent API.</span></span> <span data-ttu-id="81f89-108">Настраиваемые соглашения о Code First позволяют определять собственные соглашения, предоставляющие параметры конфигурации по умолчанию для вашей модели.</span><span class="sxs-lookup"><span data-stu-id="81f89-108">Custom Code First Conventions let you define your own conventions that provide configuration defaults for your model.</span></span> <span data-ttu-id="81f89-109">В этом пошаговом руководстве будут рассмотрены различные типы пользовательских соглашений и способы их создания.</span><span class="sxs-lookup"><span data-stu-id="81f89-109">In this walkthrough, we will explore the different types of custom conventions and how to create each of them.</span></span>


## <a name="model-based-conventions"></a><span data-ttu-id="81f89-110">Соглашения на основе модели</span><span class="sxs-lookup"><span data-stu-id="81f89-110">Model-Based Conventions</span></span>

<span data-ttu-id="81f89-111">На этой странице рассматривается API Дбмоделбуилдер для пользовательских соглашений.</span><span class="sxs-lookup"><span data-stu-id="81f89-111">This page covers the DbModelBuilder API for custom conventions.</span></span> <span data-ttu-id="81f89-112">Этот API должен быть достаточным для создания наиболее настраиваемых соглашений.</span><span class="sxs-lookup"><span data-stu-id="81f89-112">This API should be sufficient for authoring most custom conventions.</span></span> <span data-ttu-id="81f89-113">Однако существует также возможность создавать соглашения на основе модели, которые управляют завершающей моделью после ее создания — для обработки сложных сценариев.</span><span class="sxs-lookup"><span data-stu-id="81f89-113">However, there is also the ability to author model-based conventions - conventions that manipulate the final model once it is created - to handle advanced scenarios.</span></span> <span data-ttu-id="81f89-114">Дополнительные сведения см. в разделе [соглашения на основе модели](~/ef6/modeling/code-first/conventions/model.md).</span><span class="sxs-lookup"><span data-stu-id="81f89-114">For more information, see [Model-Based Conventions](~/ef6/modeling/code-first/conventions/model.md).</span></span>

 

## <a name="our-model"></a><span data-ttu-id="81f89-115">Наша модель</span><span class="sxs-lookup"><span data-stu-id="81f89-115">Our Model</span></span>

<span data-ttu-id="81f89-116">Начнем с определения простой модели, которую мы можем использовать с нашими соглашениями.</span><span class="sxs-lookup"><span data-stu-id="81f89-116">Let's start by defining a simple model that we can use with our conventions.</span></span> <span data-ttu-id="81f89-117">Добавьте в проект следующие классы.</span><span class="sxs-lookup"><span data-stu-id="81f89-117">Add the following classes to your project.</span></span>

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

 

## <a name="introducing-custom-conventions"></a><span data-ttu-id="81f89-118">Введение в пользовательские соглашения</span><span class="sxs-lookup"><span data-stu-id="81f89-118">Introducing Custom Conventions</span></span>

<span data-ttu-id="81f89-119">Давайте напишем соглашение, которое настраивает любое свойство с именем KEY в качестве первичного ключа для типа сущности.</span><span class="sxs-lookup"><span data-stu-id="81f89-119">Let’s write a convention that configures any property named Key to be the primary key for its entity type.</span></span>

<span data-ttu-id="81f89-120">Соглашения включены в построителе моделей, доступ к которым можно получить путем переопределения OnModelCreating в контексте.</span><span class="sxs-lookup"><span data-stu-id="81f89-120">Conventions are enabled on the model builder, which can be accessed by overriding OnModelCreating in the context.</span></span> <span data-ttu-id="81f89-121">Обновите класс Продуктконтекст следующим образом:</span><span class="sxs-lookup"><span data-stu-id="81f89-121">Update the ProductContext class as follows:</span></span>

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

<span data-ttu-id="81f89-122">Теперь любое свойство в нашей модели с именем Key будет настроено как первичный ключ любой сущности.</span><span class="sxs-lookup"><span data-stu-id="81f89-122">Now, any property in our model named Key will be configured as the primary key of whatever entity its part of.</span></span>

<span data-ttu-id="81f89-123">Мы также можем сделать наши соглашения более конкретными путем фильтрации по типу свойства, которое мы собираемся настроить:</span><span class="sxs-lookup"><span data-stu-id="81f89-123">We could also make our conventions more specific by filtering on the type of property that we are going to configure:</span></span>

``` csharp
    modelBuilder.Properties<int>()
                .Where(p => p.Name == "Key")
                .Configure(p => p.IsKey());
```

<span data-ttu-id="81f89-124">Это приведет к настройке всех свойств, именуемых Key, на первичный ключ своей сущности, но только в том случае, если они являются целым числом.</span><span class="sxs-lookup"><span data-stu-id="81f89-124">This will configure all properties called Key to be the primary key of their entity, but only if they are an integer.</span></span>

<span data-ttu-id="81f89-125">Интересная особенность метода IsKey заключается в том, что он является аддитивным.</span><span class="sxs-lookup"><span data-stu-id="81f89-125">An interesting feature of the IsKey method is that it is additive.</span></span> <span data-ttu-id="81f89-126">Это означает, что при вызове IsKey на нескольких свойствах все они становятся частью составного ключа.</span><span class="sxs-lookup"><span data-stu-id="81f89-126">Which means that if you call IsKey on multiple properties and they will all become part of a composite key.</span></span> <span data-ttu-id="81f89-127">Одним из этих особенностей является то, что при указании нескольких свойств для ключа необходимо также указать порядок этих свойств.</span><span class="sxs-lookup"><span data-stu-id="81f89-127">The one caveat for this is that when you specify multiple properties for a key you must also specify an order for those properties.</span></span> <span data-ttu-id="81f89-128">Это можно сделать, вызвав метод Хасколумнордер, как показано ниже:</span><span class="sxs-lookup"><span data-stu-id="81f89-128">You can do this by calling the HasColumnOrder method like below:</span></span>

``` csharp
    modelBuilder.Properties<int>()
                .Where(x => x.Name == "Key")
                .Configure(x => x.IsKey().HasColumnOrder(1));

    modelBuilder.Properties()
                .Where(x => x.Name == "Name")
                .Configure(x => x.IsKey().HasColumnOrder(2));
```

<span data-ttu-id="81f89-129">Этот код настраивает типы в нашей модели для использования составного ключа, состоящего из ключевого столбца int и столбца строкового имени.</span><span class="sxs-lookup"><span data-stu-id="81f89-129">This code will configure the types in our model to have a composite key consisting of the int Key column and the string Name column.</span></span> <span data-ttu-id="81f89-130">При просмотре модели в конструкторе она будет выглядеть следующим образом:</span><span class="sxs-lookup"><span data-stu-id="81f89-130">If we view the model in the designer it would look like this:</span></span>

![составной ключ](~/ef6/media/compositekey.png)

<span data-ttu-id="81f89-132">Еще один пример соглашений о свойствах — Настройка всех свойств DateTime в модели для соответствия типу datetime2 в SQL Server вместо DateTime.</span><span class="sxs-lookup"><span data-stu-id="81f89-132">Another example of property conventions is to configure all DateTime properties in my model to map to the datetime2 type in SQL Server instead of datetime.</span></span> <span data-ttu-id="81f89-133">Это можно сделать следующим образом:</span><span class="sxs-lookup"><span data-stu-id="81f89-133">You can achieve this with the following:</span></span>

``` csharp
    modelBuilder.Properties<DateTime>()
                .Configure(c => c.HasColumnType("datetime2"));
```

 

## <a name="convention-classes"></a><span data-ttu-id="81f89-134">Классы соглашений</span><span class="sxs-lookup"><span data-stu-id="81f89-134">Convention Classes</span></span>

<span data-ttu-id="81f89-135">Другим способом определения соглашений является использование класса соглашения для инкапсуляции вашего соглашения.</span><span class="sxs-lookup"><span data-stu-id="81f89-135">Another way of defining conventions is to use a Convention Class to encapsulate your convention.</span></span> <span data-ttu-id="81f89-136">При использовании класса соглашения создается тип, который наследуется от класса соглашения в пространстве имен System. Data. Entity. Моделконфигуратион. соглашений.</span><span class="sxs-lookup"><span data-stu-id="81f89-136">When using a Convention Class then you create a type that inherits from the Convention class in the System.Data.Entity.ModelConfiguration.Conventions namespace.</span></span>

<span data-ttu-id="81f89-137">Мы можем создать класс соглашения с соглашением datetime2, которое мы показали ранее, выполнив следующие действия.</span><span class="sxs-lookup"><span data-stu-id="81f89-137">We can create a Convention Class with the datetime2 convention that we showed earlier by doing the following:</span></span>

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

<span data-ttu-id="81f89-138">Чтобы сообщить EF об использовании этого соглашения, добавьте его в коллекцию соглашений в OnModelCreating, что, если вы уже работали с пошаговым руководством, будет выглядеть следующим образом:</span><span class="sxs-lookup"><span data-stu-id="81f89-138">To tell EF to use this convention you add it to the Conventions collection in OnModelCreating, which if you’ve been following along with the walkthrough will look like this:</span></span>

``` csharp
    protected override void OnModelCreating(DbModelBuilder modelBuilder)
    {
        modelBuilder.Properties<int>()
                    .Where(p => p.Name.EndsWith("Key"))
                    .Configure(p => p.IsKey());

        modelBuilder.Conventions.Add(new DateTime2Convention());
    }
```

<span data-ttu-id="81f89-139">Как можно увидеть, мы добавим экземпляр нашего соглашения в коллекцию соглашений.</span><span class="sxs-lookup"><span data-stu-id="81f89-139">As you can see we add an instance of our convention to the conventions collection.</span></span> <span data-ttu-id="81f89-140">Наследование от соглашения предоставляет удобный способ группирования и совместного использования соглашений в командах или проектах.</span><span class="sxs-lookup"><span data-stu-id="81f89-140">Inheriting from Convention provides a convenient way of grouping and sharing conventions across teams or projects.</span></span> <span data-ttu-id="81f89-141">Например, можно создать библиотеку классов с общим набором соглашений, используемых всеми проектами вашей организации.</span><span class="sxs-lookup"><span data-stu-id="81f89-141">You could, for example, have a class library with a common set of conventions that all of your organizations projects use.</span></span>

 

## <a name="custom-attributes"></a><span data-ttu-id="81f89-142">Настраиваемые атрибуты</span><span class="sxs-lookup"><span data-stu-id="81f89-142">Custom Attributes</span></span>

<span data-ttu-id="81f89-143">Еще одним отличным применением соглашений является включение новых атрибутов для использования при настройке модели.</span><span class="sxs-lookup"><span data-stu-id="81f89-143">Another great use of conventions is to enable new attributes to be used when configuring a model.</span></span> <span data-ttu-id="81f89-144">Чтобы проиллюстрировать это, создадим атрибут, который можно использовать для пометки свойств строки как не поддерживающих Юникод.</span><span class="sxs-lookup"><span data-stu-id="81f89-144">To illustrate this, let’s create an attribute that we can use to mark String properties as non-Unicode.</span></span>

``` csharp
    [AttributeUsage(AttributeTargets.Property, AllowMultiple = false)]
    public class NonUnicode : Attribute
    {
    }
```

<span data-ttu-id="81f89-145">Теперь создадим соглашение, чтобы применить этот атрибут к нашей модели:</span><span class="sxs-lookup"><span data-stu-id="81f89-145">Now, let’s create a convention to apply this attribute to our model:</span></span>

``` csharp
    modelBuilder.Properties()
                .Where(x => x.GetCustomAttributes(false).OfType<NonUnicode>().Any())
                .Configure(c => c.IsUnicode(false));
```

<span data-ttu-id="81f89-146">В соответствии с этим соглашением мы можем добавить атрибут не в Юникоде к любому из наших строковых свойств, что означает, что столбец в базе данных будет храниться как varchar, а не nvarchar.</span><span class="sxs-lookup"><span data-stu-id="81f89-146">With this convention we can add the NonUnicode attribute to any of our string properties, which means the column in the database will be stored as varchar instead of nvarchar.</span></span>

<span data-ttu-id="81f89-147">Обратите внимание на то, что при помещении атрибута не в Юникоде для любого, кроме строкового свойства, будет выдано исключение.</span><span class="sxs-lookup"><span data-stu-id="81f89-147">One thing to note about this convention is that if you put the NonUnicode attribute on anything other than a string property then it will throw an exception.</span></span> <span data-ttu-id="81f89-148">Это происходит потому, что нельзя настроить параметр GetString в Юникоде для любого типа, кроме строки.</span><span class="sxs-lookup"><span data-stu-id="81f89-148">It does this because you cannot configure IsUnicode on any type other than a string.</span></span> <span data-ttu-id="81f89-149">В этом случае вы можете сделать соглашение более конкретным, чтобы оно отфильтровывао все, что не является строкой.</span><span class="sxs-lookup"><span data-stu-id="81f89-149">If this happens, then you can make your convention more specific, so that it filters out anything that isn’t a string.</span></span>

<span data-ttu-id="81f89-150">Хотя приведенное выше соглашение работает при определении пользовательских атрибутов, существует еще один API, который гораздо проще в использовании, особенно если требуется использовать свойства из класса Attribute.</span><span class="sxs-lookup"><span data-stu-id="81f89-150">While the above convention works for defining custom attributes there is another API that can be much easier to use, especially when you want to use properties from the attribute class.</span></span>

<span data-ttu-id="81f89-151">В этом примере мы будем обновлять наш атрибут и изменим его на атрибут an в Юникоде, поэтому он выглядит следующим образом:</span><span class="sxs-lookup"><span data-stu-id="81f89-151">For this example we are going to update our attribute and change it to an IsUnicode attribute, so it looks like this:</span></span>

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

<span data-ttu-id="81f89-152">После этого можно задать логическое значение для нашего атрибута, чтобы указать, что свойство должно быть в Юникоде.</span><span class="sxs-lookup"><span data-stu-id="81f89-152">Once we have this, we can set a bool on our attribute to tell the convention whether or not a property should be Unicode.</span></span> <span data-ttu-id="81f89-153">Это можно сделать в соответствии с соглашением, которое мы уже получили, обращаясь к Клрпроперти класса конфигурации следующим образом:</span><span class="sxs-lookup"><span data-stu-id="81f89-153">We could do this in the convention we have already by accessing the ClrProperty of the configuration class like this:</span></span>

``` csharp
    modelBuilder.Properties()
                .Where(x => x.GetCustomAttributes(false).OfType<IsUnicode>().Any())
                .Configure(c => c.IsUnicode(c.ClrPropertyInfo.GetCustomAttribute<IsUnicode>().Unicode));
```

<span data-ttu-id="81f89-154">Это достаточно просто, но существует более лаконичный способ достижения этого, используя метод HAVING API соглашений.</span><span class="sxs-lookup"><span data-stu-id="81f89-154">This is easy enough, but there is a more succinct way of achieving this by using the Having method of the conventions API.</span></span> <span data-ttu-id="81f89-155">Метод HAVING имеет параметр типа Func&lt;PropertyInfo, T&gt; который принимает объект PropertyInfo так же, как и метод Where, но Ожидался возврат объекта.</span><span class="sxs-lookup"><span data-stu-id="81f89-155">The Having method has a parameter of type Func&lt;PropertyInfo, T&gt; which accepts the PropertyInfo the same as the Where method, but is expected to return an object.</span></span> <span data-ttu-id="81f89-156">Если возвращенный объект имеет значение null, свойство не будет настроено. Это означает, что вы можете отфильтровывать свойства с ним, как и там, но при этом он также будет захватывать возвращаемый объект и передавать его в метод configure.</span><span class="sxs-lookup"><span data-stu-id="81f89-156">If the returned object is null then the property will not be configured, which means you can filter out properties with it just like Where, but it is different in that it will also capture the returned object and pass it to the Configure method.</span></span> <span data-ttu-id="81f89-157">Это работает следующим образом:</span><span class="sxs-lookup"><span data-stu-id="81f89-157">This works like the following:</span></span>

``` csharp
    modelBuilder.Properties()
                .Having(x =>x.GetCustomAttributes(false).OfType<IsUnicode>().FirstOrDefault())
                .Configure((config, att) => config.IsUnicode(att.Unicode));
```

<span data-ttu-id="81f89-158">Пользовательские атрибуты — это не единственная причина использования метода HAVING. Это полезно в любом месте, где необходимо указать, что вы фильтруете при настройке типов или свойств.</span><span class="sxs-lookup"><span data-stu-id="81f89-158">Custom attributes are not the only reason to use the Having method, it is useful anywhere that you need to reason about something that you are filtering on when configuring your types or properties.</span></span>

 

## <a name="configuring-types"></a><span data-ttu-id="81f89-159">Настройка типов</span><span class="sxs-lookup"><span data-stu-id="81f89-159">Configuring Types</span></span>

<span data-ttu-id="81f89-160">Пока все наши соглашения были для свойств, но есть другая область API соглашений для настройки типов в модели.</span><span class="sxs-lookup"><span data-stu-id="81f89-160">So far all of our conventions have been for properties, but there is another area of the conventions API for configuring the types in your model.</span></span> <span data-ttu-id="81f89-161">Опыт похож на те, которые мы видели до сих пор, но параметры внутри настройки будут находиться в сущности, а не на уровне свойств.</span><span class="sxs-lookup"><span data-stu-id="81f89-161">The experience is similar to the conventions we have seen so far, but the options inside configure will be at the entity instead of property level.</span></span>

<span data-ttu-id="81f89-162">Одна из моментов, с которой соглашения об уровне типа может быть действительно полезна для, — изменение соглашения об именовании таблиц для соответствия существующей схеме, которая отличается от EF Default, или создание новой базы данных с другим соглашением об именовании.</span><span class="sxs-lookup"><span data-stu-id="81f89-162">One of the things that Type level conventions can be really useful for is changing the table naming convention, either to map to an existing schema that differs from the EF default or to create a new database with a different naming convention.</span></span> <span data-ttu-id="81f89-163">Для этого нам нужен метод, который может принять TypeInfo для типа в нашей модели и вернуть имя таблицы для этого типа:</span><span class="sxs-lookup"><span data-stu-id="81f89-163">To do this we first need a method that can accept the TypeInfo for a type in our model and return what the table name for that type should be:</span></span>

``` csharp
    private string GetTableName(Type type)
    {
        var result = Regex.Replace(type.Name, ".[A-Z]", m => m.Value[0] + "_" + m.Value[1]);

        return result.ToLower();
    }
```

<span data-ttu-id="81f89-164">Этот метод принимает тип и возвращает строку, в которой используется нижний регистр с символами подчеркивания вместо CamelCase.</span><span class="sxs-lookup"><span data-stu-id="81f89-164">This method takes a type and returns a string that uses lower case with underscores instead of CamelCase.</span></span> <span data-ttu-id="81f89-165">В нашей модели это означает, что класс ProductCategory будет сопоставлен с таблицей с именем Product\_Category вместо ProductCategories.</span><span class="sxs-lookup"><span data-stu-id="81f89-165">In our model this means that the ProductCategory class will be mapped to a table called product\_category instead of ProductCategories.</span></span>

<span data-ttu-id="81f89-166">После этого метод можно вызвать в соответствии со следующим соглашением:</span><span class="sxs-lookup"><span data-stu-id="81f89-166">Once we have that method we can call it in a convention like this:</span></span>

``` csharp
    modelBuilder.Types()
                .Configure(c => c.ToTable(GetTableName(c.ClrType)));
```

<span data-ttu-id="81f89-167">Это соглашение настраивает каждый тип в нашей модели для соотнесения с именем таблицы, возвращаемым методомического TableName.</span><span class="sxs-lookup"><span data-stu-id="81f89-167">This convention configures every type in our model to map to the table name that is returned from our GetTableName method.</span></span> <span data-ttu-id="81f89-168">Это соглашение эквивалентно вызову метода ToTable для каждой сущности в модели с помощью API Fluent.</span><span class="sxs-lookup"><span data-stu-id="81f89-168">This convention is the equivalent to calling the ToTable method for each entity in the model using the Fluent API.</span></span>

<span data-ttu-id="81f89-169">Обратите внимание, что при вызове ToTable EF будет принимать строку, которую вы задаете как точное имя таблицы, без какого-либо множественного преобразования, которое обычно выполняется при определении имен таблиц.</span><span class="sxs-lookup"><span data-stu-id="81f89-169">One thing to note about this is that when you call ToTable EF will take the string that you provide as the exact table name, without any of the pluralization that it would normally do when determining table names.</span></span> <span data-ttu-id="81f89-170">Именно поэтому имя таблицы в нашем соглашении — это\_категории «продукт» вместо категорий «\_продукта».</span><span class="sxs-lookup"><span data-stu-id="81f89-170">This is why the table name from our convention is product\_category instead of product\_categories.</span></span> <span data-ttu-id="81f89-171">Мы можем разрешить это в нашем соглашении, сделав вызов службы во множественном числе.</span><span class="sxs-lookup"><span data-stu-id="81f89-171">We can resolve that in our convention by making a call to the pluralization service ourselves.</span></span>

<span data-ttu-id="81f89-172">В следующем коде мы будем использовать функцию [разрешения зависимостей](~/ef6/fundamentals/configuring/dependency-resolution.md) , добавленную в EF6, чтобы получить службу множественного преобразования, которая использовалась EF и создания множественного наше имя таблицы.</span><span class="sxs-lookup"><span data-stu-id="81f89-172">In the following code we will use the [Dependency Resolution](~/ef6/fundamentals/configuring/dependency-resolution.md) feature added in EF6 to retrieve the pluralization service that EF would have used and pluralize our table name.</span></span>

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
> <span data-ttu-id="81f89-173">Универсальная версия метода WebService является методом расширения в пространстве имен System. Data. Entity. Infrastructure. Депенденциресолутион. для его использования необходимо добавить инструкцию using в контекст.</span><span class="sxs-lookup"><span data-stu-id="81f89-173">The generic version of GetService is an extension method in the System.Data.Entity.Infrastructure.DependencyResolution namespace, you will need to add a using statement to your context in order to use it.</span></span>

### <a name="totable-and-inheritance"></a><span data-ttu-id="81f89-174">ToTable и наследование</span><span class="sxs-lookup"><span data-stu-id="81f89-174">ToTable and Inheritance</span></span>

<span data-ttu-id="81f89-175">Еще один важный аспект ToTable заключается в том, что при явном сопоставлении типа с заданной таблицей можно изменить стратегию сопоставления, которую будет использовать EF.</span><span class="sxs-lookup"><span data-stu-id="81f89-175">Another important aspect of ToTable is that if you explicitly map a type to a given table, then you can alter the mapping strategy that EF will use.</span></span> <span data-ttu-id="81f89-176">Если вы вызываете ToTable для каждого типа в иерархии наследования, передавая имя типа в качестве имени таблицы, как мы делали ранее, то изменим стратегию сопоставления таблиц по иерархии (иерархия) по умолчанию до типа "одна таблица на тип" (TPT).</span><span class="sxs-lookup"><span data-stu-id="81f89-176">If you call ToTable for every type in an inheritance hierarchy, passing the type name as the name of the table like we did above, then you will change the default Table-Per-Hierarchy (TPH) mapping strategy to Table-Per-Type (TPT).</span></span> <span data-ttu-id="81f89-177">Лучший способ описать это — вхис конкретный пример:</span><span class="sxs-lookup"><span data-stu-id="81f89-177">The best way to describe this is whith a concrete example:</span></span>

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

<span data-ttu-id="81f89-178">По умолчанию сотрудники и менеджер сопоставляются с одной и той же таблицей в базе данных.</span><span class="sxs-lookup"><span data-stu-id="81f89-178">By default both employee and manager are mapped to the same table (Employees) in the database.</span></span> <span data-ttu-id="81f89-179">Таблица будет содержать как сотрудников, так и руководителей со столбцом дискриминатора, который сообщит, какой тип экземпляра хранится в каждой строке.</span><span class="sxs-lookup"><span data-stu-id="81f89-179">The table will contain both employees and managers with a discriminator column that will tell you what type of instance is stored in each row.</span></span> <span data-ttu-id="81f89-180">Это сопоставление с иерархической таблицей, так как для иерархии существует одна таблица.</span><span class="sxs-lookup"><span data-stu-id="81f89-180">This is TPH mapping as there is a single table for the hierarchy.</span></span> <span data-ttu-id="81f89-181">Однако при вызове ToTable для обоих классе каждый тип будет сопоставлен с собственной таблицей, также известной как TPT, так как каждый тип имеет свою собственную таблицу.</span><span class="sxs-lookup"><span data-stu-id="81f89-181">However, if you call ToTable on both classe then each type will instead be mapped to its own table, also known as TPT since each type has its own table.</span></span>

``` csharp
    modelBuilder.Types()
                .Configure(c=>c.ToTable(c.ClrType.Name));
```

<span data-ttu-id="81f89-182">Приведенный выше код будет сопоставляться со структурой таблицы, которая выглядит следующим образом:</span><span class="sxs-lookup"><span data-stu-id="81f89-182">The code above will map to a table structure that looks like the following:</span></span>

![Пример TPT](~/ef6/media/tptexample.jpg)

<span data-ttu-id="81f89-184">Вы можете избежать этого и сохранить сопоставление по умолчанию, выполняйте несколько следующих действий:</span><span class="sxs-lookup"><span data-stu-id="81f89-184">You can avoid this, and maintain the default TPH mapping, in a couple ways:</span></span>

1.  <span data-ttu-id="81f89-185">Вызовите ToTable с тем же именем таблицы для каждого типа в иерархии.</span><span class="sxs-lookup"><span data-stu-id="81f89-185">Call ToTable with the same table name for each type in the hierarchy.</span></span>
2.  <span data-ttu-id="81f89-186">Вызовите ToTable только для базового класса иерархии, в нашем примере — Employee.</span><span class="sxs-lookup"><span data-stu-id="81f89-186">Call ToTable only on the base class of the hierarchy, in our example that would be employee.</span></span>

 

## <a name="execution-order"></a><span data-ttu-id="81f89-187">Порядок выполнения</span><span class="sxs-lookup"><span data-stu-id="81f89-187">Execution Order</span></span>

<span data-ttu-id="81f89-188">Соглашения работают в режиме последнего WINS-сервера, то же, что и API Fluent.</span><span class="sxs-lookup"><span data-stu-id="81f89-188">Conventions operate in a last wins manner, the same as the Fluent API.</span></span> <span data-ttu-id="81f89-189">Это означает, что при написании двух соглашений, которые настраивают один и тот же параметр одного и того же свойства, последний для выполнения WINS.</span><span class="sxs-lookup"><span data-stu-id="81f89-189">What this means is that if you write two conventions that configure the same option of the same property, then the last one to execute wins.</span></span> <span data-ttu-id="81f89-190">Например, в коде ниже максимальной длины всех строк задано значение 500, но затем все свойства, имена которых называются в модели, будут иметь максимальную длину 250.</span><span class="sxs-lookup"><span data-stu-id="81f89-190">As an example, in the code below the max length of all strings is set to 500 but we then configure all properties called Name in the model to have a max length of 250.</span></span>

``` csharp
    modelBuilder.Properties<string>()
                .Configure(c => c.HasMaxLength(500));

    modelBuilder.Properties<string>()
                .Where(x => x.Name == "Name")
                .Configure(c => c.HasMaxLength(250));
```

<span data-ttu-id="81f89-191">Поскольку в соответствии с соглашением для установки параметра Max length равным 250, после того, как для всех строк задается значение 500, все свойства, имена которых называются в нашей модели, будут иметь значение MaxLength, равное 250, а любые другие строки, например описания, будут состоять из 500.</span><span class="sxs-lookup"><span data-stu-id="81f89-191">Because the convention to set max length to 250 is after the one that sets all strings to 500, all the properties called Name in our model will have a MaxLength of 250 while any other strings, such as descriptions, would be 500.</span></span> <span data-ttu-id="81f89-192">Использование соглашений таким образом означает, что вы можете предоставить общее соглашение для типов или свойств в модели, а затем переопределить их для поднаборов, которые отличаются.</span><span class="sxs-lookup"><span data-stu-id="81f89-192">Using conventions in this way means that you can provide a general convention for types or properties in your model and then overide them for subsets that are different.</span></span>

<span data-ttu-id="81f89-193">API-интерфейс Fluent и заметки к данным также можно использовать для переопределения соглашения в конкретных случаях.</span><span class="sxs-lookup"><span data-stu-id="81f89-193">The Fluent API and Data Annotations can also be used to override a convention in specific cases.</span></span> <span data-ttu-id="81f89-194">В нашем примере выше, если мы использовали API Fluent для установки максимальной длины свойства, мы могли бы разместить его до или после соглашения, поскольку более конкретный API-интерфейс Fluent будет выиграть по более общему соглашению о конфигурации.</span><span class="sxs-lookup"><span data-stu-id="81f89-194">In our example above if we had used the Fluent API to set the max length of a property then we could have put it before or after the convention, because the more specific Fluent API will win over the more general Configuration Convention.</span></span>

 

## <a name="built-in-conventions"></a><span data-ttu-id="81f89-195">Встроенные соглашения</span><span class="sxs-lookup"><span data-stu-id="81f89-195">Built-in Conventions</span></span>

<span data-ttu-id="81f89-196">Так как соглашения Code First по умолчанию могут повлиять на пользовательские соглашения, может быть полезно добавить соглашения, которые будут выполняться до или после другого соглашения.</span><span class="sxs-lookup"><span data-stu-id="81f89-196">Because custom conventions could be affected by the default Code First conventions, it can be useful to add conventions to run before or after another convention.</span></span> <span data-ttu-id="81f89-197">Для этого можно использовать методы Аддбефоре и Аддафтер коллекции соглашений для производного DbContext.</span><span class="sxs-lookup"><span data-stu-id="81f89-197">To do this you can use the AddBefore and AddAfter methods of the Conventions collection on your derived DbContext.</span></span> <span data-ttu-id="81f89-198">Следующий код добавляет созданный ранее класс соглашения, чтобы он выполнялся перед встроенным соглашением об обнаружении ключа.</span><span class="sxs-lookup"><span data-stu-id="81f89-198">The following code would add the convention class we created earlier so that it will run before the built in key discovery convention.</span></span>

``` csharp
    modelBuilder.Conventions.AddBefore<IdKeyDiscoveryConvention>(new DateTime2Convention());
```

<span data-ttu-id="81f89-199">Это будет наиболее частое применение при добавлении соглашений, которые должны выполняться до или после встроенных соглашений. список встроенных соглашений можно найти здесь: [System. Data. Entity. моделконфигуратион. Conventions Namespace](https://msdn.microsoft.com/library/system.data.entity.modelconfiguration.conventions.aspx).</span><span class="sxs-lookup"><span data-stu-id="81f89-199">This is going to be of the most use when adding conventions that need to run before or after the built in conventions, a list of the built in conventions can be found here: [System.Data.Entity.ModelConfiguration.Conventions Namespace](https://msdn.microsoft.com/library/system.data.entity.modelconfiguration.conventions.aspx).</span></span>

<span data-ttu-id="81f89-200">Можно также удалить соглашения, которые не должны применяться к модели.</span><span class="sxs-lookup"><span data-stu-id="81f89-200">You can also remove conventions that you do not want applied to your model.</span></span> <span data-ttu-id="81f89-201">Чтобы удалить соглашение, используйте метод Remove.</span><span class="sxs-lookup"><span data-stu-id="81f89-201">To remove a convention, use the Remove method.</span></span> <span data-ttu-id="81f89-202">Ниже приведен пример удаления Плурализингтабленамеконвентион.</span><span class="sxs-lookup"><span data-stu-id="81f89-202">Here is an example of removing the PluralizingTableNameConvention.</span></span>

``` csharp
    protected override void OnModelCreating(DbModelBuilder modelBuilder)
    {
        modelBuilder.Conventions.Remove<PluralizingTableNameConvention>();
    }
```
