---
title: Заметки к данным First - EF6 Code
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: 80abefbd-23c9-4fce-9cd3-520e5df9856e
caps.latest.revision: 3
ms.openlocfilehash: 91d1d8c2608df8f7b38e70b565a4225cf10ae21f
ms.sourcegitcommit: bdd06c9a591ba5e6d6a3ec046c80de98f598f3f3
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/10/2018
ms.locfileid: "39122804"
---
# <a name="code-first-data-annotations"></a><span data-ttu-id="f4951-102">Заметки данных Code First</span><span class="sxs-lookup"><span data-stu-id="f4951-102">Code First Data Annotations</span></span>
> [!NOTE]
> <span data-ttu-id="f4951-103">**EF4.1 и более поздних версий только** -функции, интерфейсы API, и т.д., описанных на этой странице появились в версии 4.1 платформы Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="f4951-103">**EF4.1 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 4.1.</span></span> <span data-ttu-id="f4951-104">При использовании более ранней версии могут быть неприменимы некоторые или все сведения.</span><span class="sxs-lookup"><span data-stu-id="f4951-104">If you are using an earlier version, some or all of the information does not apply.</span></span>

<span data-ttu-id="f4951-105">Содержимое на этой странице написана по материалам, а статья задумано, Джули Лерман (\<http://thedatafarm.com>).</span><span class="sxs-lookup"><span data-stu-id="f4951-105">The content on this page is adapted from and article originally written by Julie Lerman (\<http://thedatafarm.com>).</span></span>

<span data-ttu-id="f4951-106">Entity Framework Code First позволяет использовать собственные классы домена для представления модели, который зависит от EF для выполнения запросов, изменение отслеживания и обновления функций.</span><span class="sxs-lookup"><span data-stu-id="f4951-106">Entity Framework Code First allows you to use your own domain classes to represent the model which EF relies on to perform querying, change tracking and updating functions.</span></span> <span data-ttu-id="f4951-107">Код сначала использует шаблон программирования, называется соглашение относительно настройки.</span><span class="sxs-lookup"><span data-stu-id="f4951-107">Code first leverages a programming pattern referred to as convention over configuration.</span></span> <span data-ttu-id="f4951-108">Это означает, что код сначала предположит, что ваши классы соответствуют соглашениям об используемые EF.</span><span class="sxs-lookup"><span data-stu-id="f4951-108">What this means is that code first will assume that your classes follow the conventions that EF uses.</span></span> <span data-ttu-id="f4951-109">В этом случае EF будет иметь возможность работать с деталями, ему необходимо выполнять свою работу.</span><span class="sxs-lookup"><span data-stu-id="f4951-109">In that case, EF will be able to work out the details it needs to do its job.</span></span> <span data-ttu-id="f4951-110">Тем не менее если ваши классы не выполняйте эти соглашения, у вас есть возможность добавления конфигураций в пользовательские классы для предоставления EF с необходимой ему информации.</span><span class="sxs-lookup"><span data-stu-id="f4951-110">However, if your classes do not follow those conventions, you have the ability to add configurations to your classes to provide EF with the information it needs.</span></span>

<span data-ttu-id="f4951-111">Во-первых, код предоставляет два способа добавления этих конфигураций в пользовательские классы.</span><span class="sxs-lookup"><span data-stu-id="f4951-111">Code first gives you two ways to add these configurations to your classes.</span></span> <span data-ttu-id="f4951-112">Один с помощью простых атрибутов, вызывается DataAnnotations, а другой — сначала с помощью кода является Fluent API, который предоставляет способ описания конфигураций, принудительно, в коде.</span><span class="sxs-lookup"><span data-stu-id="f4951-112">One is using simple attributes called DataAnnotations and the other is using code first’s Fluent API, which provides you with a way to describe configurations imperatively, in code.</span></span>

<span data-ttu-id="f4951-113">В этой статье основное внимание уделяется с помощью DataAnnotations (в пространстве имен System.ComponentModel.DataAnnotations) для настройки классов — выделение наиболее общие конфигурации.</span><span class="sxs-lookup"><span data-stu-id="f4951-113">This article will focus on using DataAnnotations (in the System.ComponentModel.DataAnnotations namespace) to configure your classes – highlighting the most commonly needed configurations.</span></span> <span data-ttu-id="f4951-114">DataAnnotations также понимает ряд приложений .NET, таких как ASP.NET MVC, который позволяет этим приложениям использовать одинаковые примечания для проверки на стороне клиента.</span><span class="sxs-lookup"><span data-stu-id="f4951-114">DataAnnotations are also understood by a number of .NET applications, such as ASP.NET MVC which allows these applications to leverage the same annotations for client-side validations.</span></span>


## <a name="the-model"></a><span data-ttu-id="f4951-115">Модель</span><span class="sxs-lookup"><span data-stu-id="f4951-115">The model</span></span>

<span data-ttu-id="f4951-116">Я продемонстрирую кода первого DataAnnotations с парой простых классов: блог и Post.</span><span class="sxs-lookup"><span data-stu-id="f4951-116">I’ll demonstrate code first DataAnnotations with a simple pair of classes: Blog and Post.</span></span>

``` csharp
    public class Blog
    {
        public int Id { get; set; }
        public string Title { get; set; }
        public string BloggerName { get; set;}
        public virtual ICollection<Post> Posts { get; set; }
    }

    public class Post
    {
        public int Id { get; set; }
        public string Title { get; set; }
        public DateTime DateCreated { get; set; }
        public string Content { get; set; }
        public int BlogId { get; set; }
        public ICollection<Comment> Comments { get; set; }
    }
```

<span data-ttu-id="f4951-117">Значения, блог и Post классы удобно соответствуют соглашению об первый код и необходимые без модификации для работы с ними EF.</span><span class="sxs-lookup"><span data-stu-id="f4951-117">As they are, the Blog and Post classes conveniently follow code first convention and required no tweaks to help EF work with them.</span></span> <span data-ttu-id="f4951-118">Но можно также использовать заметки для предоставления EF Дополнительные сведения о классах и базу данных, они сопоставляются.</span><span class="sxs-lookup"><span data-stu-id="f4951-118">But you can also use the annotations to provide more information to EF about the classes and the database that they map to.</span></span>

 

## <a name="key"></a><span data-ttu-id="f4951-119">Ключ</span><span class="sxs-lookup"><span data-stu-id="f4951-119">Key</span></span>

<span data-ttu-id="f4951-120">Entity Framework использует каждая сущность с ключом, который используется для отслеживания сущности.</span><span class="sxs-lookup"><span data-stu-id="f4951-120">Entity Framework relies on every entity having a key value that it uses for tracking entities.</span></span> <span data-ttu-id="f4951-121">Одно из правил, которые код сначала зависит от является, как подразумевается, какое свойство является ключом в каждый из классов первый код.</span><span class="sxs-lookup"><span data-stu-id="f4951-121">One of the conventions that code first depends on is how it implies which property is the key in each of the code first classes.</span></span> <span data-ttu-id="f4951-122">Что по соглашению, найдите свойство с именем «Id» или сочетает имя класса и «Id», например «BlogId».</span><span class="sxs-lookup"><span data-stu-id="f4951-122">That convention is to look for a property named “Id” or one that combines the class name and “Id”, such as “BlogId”.</span></span> <span data-ttu-id="f4951-123">Свойство сопоставляется столбцом первичного ключа в базе данных.</span><span class="sxs-lookup"><span data-stu-id="f4951-123">The property will map to a primary key column in the database.</span></span>

<span data-ttu-id="f4951-124">Блог и Post классов следуют соглашению.</span><span class="sxs-lookup"><span data-stu-id="f4951-124">The Blog and Post classes both follow this convention.</span></span> <span data-ttu-id="f4951-125">Но что делать, если они не?</span><span class="sxs-lookup"><span data-stu-id="f4951-125">But what if they didn’t?</span></span> <span data-ttu-id="f4951-126">Что делать, если имя используется блог *PrimaryTrackingKey* вместо или даже *foo*?</span><span class="sxs-lookup"><span data-stu-id="f4951-126">What if Blog used the name *PrimaryTrackingKey* instead or even *foo*?</span></span> <span data-ttu-id="f4951-127">Если код сначала не удается найти свойство, которое соответствует этому соглашению вызовет исключение из-за требования платформы Entity Framework, что необходимо иметь ключевое свойство.</span><span class="sxs-lookup"><span data-stu-id="f4951-127">If code first does not find a property that matches this convention it will throw an exception because of Entity Framework’s requirement that you must have a key property.</span></span> <span data-ttu-id="f4951-128">Ключа заметки можно использовать для указания того, какое свойство будет использоваться в качестве EntityKey.</span><span class="sxs-lookup"><span data-stu-id="f4951-128">You can use the key annotation to specify which property is to be used as the EntityKey.</span></span>

``` csharp
    public class Blog
    {
        [Key]
        public int PrimaryTrackingKey { get; set; }
        public string Title { get; set; }
        public string BloggerName { get; set;}
        public virtual ICollection<Post> Posts { get; set; }
    }
```

<span data-ttu-id="f4951-129">Если вы являетесь сначала с помощью кода — использовать функцию создания базы данных, в блоге таблице будет иметь столбец первичного ключа с именем PrimaryTrackingKey, который также определен как удостоверение по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="f4951-129">If you are using code first’s database generation feature, the Blog table will have a primary key column named PrimaryTrackingKey which is also defined as Identity by default.</span></span>

![jj591583_figure01](~/ef6/media/jj591583-figure01.png)

### <a name="composite-keys"></a><span data-ttu-id="f4951-131">Составные ключи</span><span class="sxs-lookup"><span data-stu-id="f4951-131">Composite keys</span></span>

<span data-ttu-id="f4951-132">Платформа Entity Framework поддерживает составные ключи - первичные ключи, которые состоят из нескольких свойств.</span><span class="sxs-lookup"><span data-stu-id="f4951-132">Entity Framework supports composite keys - primary keys that consist of more than one property.</span></span> <span data-ttu-id="f4951-133">Например ваш может иметь первичный ключ которой представляет собой сочетание PassportNumber и IssuingCountry класс Passport.</span><span class="sxs-lookup"><span data-stu-id="f4951-133">For example, your could have a Passport class whose primary key is a combination of PassportNumber and IssuingCountry.</span></span>

``` csharp
    public class Passport
    {
        [Key]
        public int PassportNumber { get; set; }
        [Key]
        public string IssuingCountry { get; set; }
        public DateTime Issued { get; set; }
        public DateTime Expires { get; set; }
    }
```

<span data-ttu-id="f4951-134">Если использовать приведенного выше класса в модели EF получится сообщение исключения InvalidOperationException;</span><span class="sxs-lookup"><span data-stu-id="f4951-134">If you were to try and use the above class in your EF model you wuld get an InvalidOperationExceptions stating;</span></span>

<span data-ttu-id="f4951-135">*Не удалось определить составного первичного ключа упорядочения для типа «Passport». Чтобы указать порядок для составные первичные ключи, используя ColumnAttribute или метод HasKey.*</span><span class="sxs-lookup"><span data-stu-id="f4951-135">*Unable to determine composite primary key ordering for type 'Passport'. Use the ColumnAttribute or the HasKey method to specify an order for composite primary keys.*</span></span>

<span data-ttu-id="f4951-136">Если у вас есть составные ключи, Entity Framework необходимо определять порядок ключевых свойств.</span><span class="sxs-lookup"><span data-stu-id="f4951-136">When you have composite keys, Entity Framework requires you to define an order of the key properties.</span></span> <span data-ttu-id="f4951-137">Это можно сделать с помощью заметок к столбцам, чтобы задать порядок.</span><span class="sxs-lookup"><span data-stu-id="f4951-137">You can do this using the Column annotation to specify an order.</span></span>

>[!NOTE]
> <span data-ttu-id="f4951-138">Порядок сортировки является относительным (а не на базе индекса), чтобы использовать все значения.</span><span class="sxs-lookup"><span data-stu-id="f4951-138">The order value is relative (rather than index based) so any values can be used.</span></span> <span data-ttu-id="f4951-139">Например 100 и 200 подойдет вместо 1 и 2.</span><span class="sxs-lookup"><span data-stu-id="f4951-139">For example, 100 and 200 would be acceptable in place of 1 and 2.</span></span>

``` csharp
    public class Passport
    {
        [Key]
        [Column(Order=1)]
        public int PassportNumber { get; set; }
        [Key]
        [Column(Order = 2)]
        public string IssuingCountry { get; set; }
        public DateTime Issued { get; set; }
        public DateTime Expires { get; set; }
    }
```

<span data-ttu-id="f4951-140">При наличии сущностей с помощью составного внешних ключей необходимо указать тот же столбец, упорядочение, которое использовалось для соответствующего свойства первичного ключа.</span><span class="sxs-lookup"><span data-stu-id="f4951-140">If you have entities with composite foreign keys then you must specify the same column ordering that you used for the corresponding primary key properties.</span></span>

<span data-ttu-id="f4951-141">Только относительный порядок в пределах свойств внешнего ключа должен быть тем же, точные значения, присвоенные **порядок** не обязательно должны совпадать.</span><span class="sxs-lookup"><span data-stu-id="f4951-141">Only the relative ordering within the foreign key properties needs to be the same, the exact values assigned to **Order** do not need to match.</span></span> <span data-ttu-id="f4951-142">Например в следующем классе, 3 и 4 может использоваться вместо 1 и 2.</span><span class="sxs-lookup"><span data-stu-id="f4951-142">For example, in the following class, 3 and 4 could be used in place of 1 and 2.</span></span>

``` csharp
    public class PassportStamp
    {
        [Key]
        public int StampId { get; set; }
        public DateTime Stamped { get; set; }
        public string StampingCountry { get; set; }

        [ForeignKey("Passport")]
        [Column(Order = 1)]
        public int PassportNumber { get; set; }

        [ForeignKey("Passport")]
        [Column(Order = 2)]
        public string IssuingCountry { get; set; }

        public Passport Passport { get; set; }
    }
```

## <a name="required"></a><span data-ttu-id="f4951-143">Обязательно</span><span class="sxs-lookup"><span data-stu-id="f4951-143">Required</span></span>

<span data-ttu-id="f4951-144">Требуется аннотация указывает EF, что определенное свойство является обязательным.</span><span class="sxs-lookup"><span data-stu-id="f4951-144">The Required annotation tells EF that a particular property is required.</span></span>

<span data-ttu-id="f4951-145">Добавление необходимых для свойства Title приведет к EF (и MVC), чтобы убедиться, что свойство имеет данные в ней.</span><span class="sxs-lookup"><span data-stu-id="f4951-145">Adding Required to the Title property will force EF (and MVC) to ensure that the property has data in it.</span></span>

``` csharp
    [Required]
    public string Title { get; set; }
```

<span data-ttu-id="f4951-146">Нет дополнительных нет изменений кода или разметки в приложении, MVC-приложении будет выполнять проверки на стороне клиента, даже динамическое создание сообщения с использованием имени свойства и заметки.</span><span class="sxs-lookup"><span data-stu-id="f4951-146">With no additional no code or markup changes in the application, an MVC application will perform client side validation, even dynamically building a message using the property and annotation names.</span></span>

![jj591583_figure02](~/ef6/media/jj591583-figure02.png)

<span data-ttu-id="f4951-148">Обязательный атрибут также повлияет на создаваемой базы данных, сделав сопоставленного свойства не допускающие значения NULL.</span><span class="sxs-lookup"><span data-stu-id="f4951-148">The Required attribute will also affect the generated database by making the mapped property non-nullable.</span></span> <span data-ttu-id="f4951-149">Обратите внимание на то, что поле заголовка изменилось на «не null».</span><span class="sxs-lookup"><span data-stu-id="f4951-149">Notice that the Title field has changed to “not null”.</span></span>

>[!NOTE]
> <span data-ttu-id="f4951-150">В некоторых случаях может оказаться невозможным для столбца в базе данных и не допускало, несмотря на то, что свойство является обязательным.</span><span class="sxs-lookup"><span data-stu-id="f4951-150">In some cases it may not be possible for the column in the database to be non-nullable even though the property is required.</span></span> <span data-ttu-id="f4951-151">Например, когда с помощью данных стратегии наследование TPH для нескольких типов хранится в одной таблице.</span><span class="sxs-lookup"><span data-stu-id="f4951-151">For example, when using a TPH inheritance strategy data for multiple types is stored in a single table.</span></span> <span data-ttu-id="f4951-152">Если производный тип содержит свойство столбца невозможно не допускающие значения NULL, так как не все типы в иерархии, это свойство будет задано.</span><span class="sxs-lookup"><span data-stu-id="f4951-152">If a derived type includes a required property the column cannot be made non-nullable since not all types in the hierarchy will have this property.</span></span>

 

![jj591583_figure03](~/ef6/media/jj591583-figure03.png)

 

## <a name="maxlength-and-minlength"></a><span data-ttu-id="f4951-154">MaxLength и MinLength</span><span class="sxs-lookup"><span data-stu-id="f4951-154">MaxLength and MinLength</span></span>

<span data-ttu-id="f4951-155">MaxLength и MinLength атрибуты позволяют пользователю указать дополнительных свойств в комплексе, как это делалось с обязательным.</span><span class="sxs-lookup"><span data-stu-id="f4951-155">The MaxLength and MinLength attributes allow you to specify additional property validations, just as you did with Required.</span></span>

<span data-ttu-id="f4951-156">Вот BloggerName с требования к длине.</span><span class="sxs-lookup"><span data-stu-id="f4951-156">Here is the BloggerName with length requirements.</span></span> <span data-ttu-id="f4951-157">Пример также демонстрирует использование атрибутов.</span><span class="sxs-lookup"><span data-stu-id="f4951-157">The example also demonstrates how to combine attributes.</span></span>

``` csharp
    [MaxLength(10),MinLength(5)]
    public string BloggerName { get; set; }
```

<span data-ttu-id="f4951-158">Заметка MaxLength повлияет на базе данных путем задания свойства длины 10.</span><span class="sxs-lookup"><span data-stu-id="f4951-158">The MaxLength annotation will impact the database by setting the property’s length to 10.</span></span>

![jj591583_figure04](~/ef6/media/jj591583-figure04.png)

<span data-ttu-id="f4951-160">Заметки клиентские MVC и EF 4.1 заметки на сервере оба учитывает проверки снова динамическое создание сообщение об ошибке: «поле BloggerName должно быть типом строки или массива с максимальной длиной "10".» Это сообщение имеет несколько большую длину.</span><span class="sxs-lookup"><span data-stu-id="f4951-160">MVC client-side annotation and EF 4.1 server-side annotation will both honor this validation, again dynamically building an error message: “The field BloggerName must be a string or array type with a maximum length of '10'.” That message is a little long.</span></span> <span data-ttu-id="f4951-161">Многие заметки позволяют указать сообщение об ошибке с атрибутом ErrorMessage.</span><span class="sxs-lookup"><span data-stu-id="f4951-161">Many annotations let you specify an error message with the ErrorMessage attribute.</span></span>

``` csharp
    [MaxLength(10, ErrorMessage="BloggerName must be 10 characters or less"),MinLength(5)]
    public string BloggerName { get; set; }
```

<span data-ttu-id="f4951-162">Сообщение об ошибке можно также указать необходимые заметки.</span><span class="sxs-lookup"><span data-stu-id="f4951-162">You can also specify ErrorMessage in the Required annotation.</span></span>

![jj591583_figure05](~/ef6/media/jj591583-figure05.png)

 

## <a name="notmapped"></a><span data-ttu-id="f4951-164">NotMapped</span><span class="sxs-lookup"><span data-stu-id="f4951-164">NotMapped</span></span>

<span data-ttu-id="f4951-165">Соглашение об первый код определяет, что каждое свойство, которое имеет тип данных, поддерживаемых представляется в базе данных.</span><span class="sxs-lookup"><span data-stu-id="f4951-165">Code first convention dictates that every property that is of a supported data type is represented in the database.</span></span> <span data-ttu-id="f4951-166">Но это не всегда так, в ваших приложениях.</span><span class="sxs-lookup"><span data-stu-id="f4951-166">But this isn’t always the case in your applications.</span></span> <span data-ttu-id="f4951-167">Например можно использовать свойство в классе блог, который создает код, основанный на поля Title и BloggerName.</span><span class="sxs-lookup"><span data-stu-id="f4951-167">For example you might have a property in the Blog class that creates a code based on the Title and BloggerName fields.</span></span> <span data-ttu-id="f4951-168">Это свойство можно создавать динамически и не должны храниться.</span><span class="sxs-lookup"><span data-stu-id="f4951-168">That property can be created dynamically and does not need to be stored.</span></span> <span data-ttu-id="f4951-169">Можно пометить все свойства, которые не сопоставляются с аннотацией NotMapped, например, это свойство BlogCode базы данных.</span><span class="sxs-lookup"><span data-stu-id="f4951-169">You can mark any properties that do not map to the database with the NotMapped annotation such as this BlogCode property.</span></span>

``` csharp
    [NotMapped]
    public string BlogCode
    {
        get
        {
            return Title.Substring(0, 1) + ":" + BloggerName.Substring(0, 1);
        }
    }
```

 

## <a name="complextype"></a><span data-ttu-id="f4951-170">ComplexType</span><span class="sxs-lookup"><span data-stu-id="f4951-170">ComplexType</span></span>

<span data-ttu-id="f4951-171">Нередко для описания сущности предметной области через набор классов и затем уровня этих классов для описания в завершенную сущность.</span><span class="sxs-lookup"><span data-stu-id="f4951-171">It’s not uncommon to describe your domain entities across a set of classes and then layer those classes to describe a complete entity.</span></span> <span data-ttu-id="f4951-172">Например может добавьте класс с именем BlogDetails в модель.</span><span class="sxs-lookup"><span data-stu-id="f4951-172">For example, you may add a class called BlogDetails to your model.</span></span>

``` csharp
    public class BlogDetails
    {
        public DateTime? DateCreated { get; set; }

        [MaxLength(250)]
        public string Description { get; set; }
    }
```

<span data-ttu-id="f4951-173">Обратите внимание на то, что BlogDetails не имеет ключевого свойства любого типа.</span><span class="sxs-lookup"><span data-stu-id="f4951-173">Notice that BlogDetails does not have any type of key property.</span></span> <span data-ttu-id="f4951-174">В предметно-ориентированное проектирование BlogDetails упоминается как объект значения.</span><span class="sxs-lookup"><span data-stu-id="f4951-174">In domain driven design, BlogDetails is referred to as a value object.</span></span> <span data-ttu-id="f4951-175">Платформа Entity Framework ссылается на объекты-значения как сложные типы.</span><span class="sxs-lookup"><span data-stu-id="f4951-175">Entity Framework refers to value objects as complex types.</span></span>  <span data-ttu-id="f4951-176">Сложные типы не отслеживается сами по себе.</span><span class="sxs-lookup"><span data-stu-id="f4951-176">Complex types cannot be tracked on their own.</span></span>

<span data-ttu-id="f4951-177">Тем не менее как свойство в классе блог BlogDetails, он будет отслеживаться как часть объекта блога.</span><span class="sxs-lookup"><span data-stu-id="f4951-177">However as a property in the Blog class, BlogDetails it will be tracked as part of a Blog object.</span></span> <span data-ttu-id="f4951-178">В порядке для code first для распознавания это BlogDetails класс необходимо пометить как ComplexType.</span><span class="sxs-lookup"><span data-stu-id="f4951-178">In order for code first to recognize this, you must mark the BlogDetails class as a ComplexType.</span></span>

``` csharp
    [ComplexType]
    public class BlogDetails
    {
        public DateTime? DateCreated { get; set; }

        [MaxLength(250)]
        public string Description { get; set; }
    }
```

<span data-ttu-id="f4951-179">Теперь можно добавить свойство в блоге классу для представления BlogDetails для этого блога.</span><span class="sxs-lookup"><span data-stu-id="f4951-179">Now you can add a property in the Blog class to represent the BlogDetails for that blog.</span></span>

``` csharp
        public BlogDetails BlogDetail { get; set; }
```

<span data-ttu-id="f4951-180">В базе данных блог таблица будет содержать все свойства блога, включая свойства, содержащиеся в его свойстве BlogDetail.</span><span class="sxs-lookup"><span data-stu-id="f4951-180">In the database, the Blog table will contain all of the properties of the blog including the properties contained in its BlogDetail property.</span></span> <span data-ttu-id="f4951-181">По умолчанию каждый из них предшествует имя сложного типа, BlogDetail.</span><span class="sxs-lookup"><span data-stu-id="f4951-181">By default, each one is preceded with the name of the complex type, BlogDetail.</span></span>

![jj591583_figure06](~/ef6/media/jj591583-figure06.png)

<span data-ttu-id="f4951-183">Другой Интересно отметить, что несмотря на то, что свойство DateCreated было определено как не допускающие значения NULL даты и времени в классе, поле соответствующей базы данных допускает значения NULL.</span><span class="sxs-lookup"><span data-stu-id="f4951-183">Another interesting note is that although the DateCreated property was defined as a non-nullable DateTime in the class, the relevant database field is nullable.</span></span> <span data-ttu-id="f4951-184">Если вы хотите повлиять на схему базы данных, необходимо использовать необходимые заметки.</span><span class="sxs-lookup"><span data-stu-id="f4951-184">You must use the Required annotation if you wish to affect the database schema.</span></span>

 

## <a name="concurrencycheck"></a><span data-ttu-id="f4951-185">ConcurrencyCheck</span><span class="sxs-lookup"><span data-stu-id="f4951-185">ConcurrencyCheck</span></span>

<span data-ttu-id="f4951-186">Заметка ConcurrencyCheck позволяет пометить одно или несколько свойств, используемый для проверки в базе данных, когда пользователь изменяет или удаляет сущность параллелизма.</span><span class="sxs-lookup"><span data-stu-id="f4951-186">The ConcurrencyCheck annotation allows you to flag one or more properties to be used for concurrency checking in the database when a user edits or deletes an entity.</span></span> <span data-ttu-id="f4951-187">Если вы работали с конструктором, EF, это соответствует соглашению присвоить свойство ConcurrencyMode значение Fixed.</span><span class="sxs-lookup"><span data-stu-id="f4951-187">If you've been working with the EF Designer, this aligns with setting a property's ConcurrencyMode to Fixed.</span></span>

<span data-ttu-id="f4951-188">Давайте посмотрим, как работает ConcurrencyCheck, добавьте его в свойство BloggerName.</span><span class="sxs-lookup"><span data-stu-id="f4951-188">Let’s see how ConcurrencyCheck works by adding it to the BloggerName property.</span></span>

``` csharp
    [ConcurrencyCheck, MaxLength(10, ErrorMessage="BloggerName must be 10 characters or less"),MinLength(5)]
    public string BloggerName { get; set; }
```

<span data-ttu-id="f4951-189">При вызове метода SaveChanges, из-за Аннотация ConcurrencyCheck в поле BloggerName исходное значение этого свойства будет использоваться в обновлении.</span><span class="sxs-lookup"><span data-stu-id="f4951-189">When SaveChanges is called, because of the ConcurrencyCheck annotation on the BloggerName field, the original value of that property will be used in the update.</span></span> <span data-ttu-id="f4951-190">Команда попытается найти правильные строки путем фильтрации не только на значение ключа, но также и на ее оригинальном значении BloggerName.</span><span class="sxs-lookup"><span data-stu-id="f4951-190">The command will attempt to locate the correct row by filtering not only on the key value but also on the original value of BloggerName.</span></span>  <span data-ttu-id="f4951-191">Ниже приведены важные обновления команда, Посланная в базу данных, где вы увидите, команда будет обновить строку, которая имеет PrimaryTrackingKey — 1 и BloggerName «Julie», который был исходное значение, при блоге был извлечен из базы данных.</span><span class="sxs-lookup"><span data-stu-id="f4951-191">Here are the critical parts of the UPDATE command sent to the database, where you can see the command will update the row that has a PrimaryTrackingKey is 1 and a BloggerName of “Julie” which was the original value when that blog was retrieved from the database.</span></span>

``` SQL
    where (([PrimaryTrackingKey] = @4) and ([BloggerName] = @5))
    @4=1,@5=N'Julie'
```

<span data-ttu-id="f4951-192">Если кто-то изменил название блоггер для блога, в то же время, это обновление завершится сбоем, и вы получите DbUpdateConcurrencyException, которые потребуются для обработки.</span><span class="sxs-lookup"><span data-stu-id="f4951-192">If someone has changed the blogger name for that blog in the meantime, this update will fail and you’ll get a DbUpdateConcurrencyException that you'll need to handle.</span></span>

 

## <a name="timestamp"></a><span data-ttu-id="f4951-193">Метка времени</span><span class="sxs-lookup"><span data-stu-id="f4951-193">TimeStamp</span></span>

<span data-ttu-id="f4951-194">Это чаще всего используется поля rowversion или метку времени для проверки параллелизма.</span><span class="sxs-lookup"><span data-stu-id="f4951-194">It's more common to use rowversion or timestamp fields for concurrency checking.</span></span> <span data-ttu-id="f4951-195">Но вместо использования ConcurrencyCheck заметки, можно использовать более конкретные заметки отметки времени, до тех пор, пока тип свойства — массив байтов.</span><span class="sxs-lookup"><span data-stu-id="f4951-195">But rather than using the ConcurrencyCheck annotation, you can use the more specific TimeStamp annotation as long as the type of the property is byte array.</span></span> <span data-ttu-id="f4951-196">Код сначала будет обрабатывать Timestamp свойства так же, как свойства ConcurrencyCheck, но также гарантирует, что поле базы данных, код сначала создает не допускающие значения NULL.</span><span class="sxs-lookup"><span data-stu-id="f4951-196">Code first will treat Timestamp properties the same as ConcurrencyCheck properties, but it will also ensure that the database field that code first generates is non-nullable.</span></span> <span data-ttu-id="f4951-197">В одном классе может иметь только одно свойство метки времени.</span><span class="sxs-lookup"><span data-stu-id="f4951-197">You can only have one timestamp property in a given class.</span></span>

<span data-ttu-id="f4951-198">Добавление в класс блог следующее свойство:</span><span class="sxs-lookup"><span data-stu-id="f4951-198">Adding the following property to the Blog class:</span></span>

``` csharp
    [Timestamp]
    public Byte[] TimeStamp { get; set; }
```

<span data-ttu-id="f4951-199">в результате получается код сначала создается столбец типа timestamp, не допускающие значения NULL в таблице базы данных.</span><span class="sxs-lookup"><span data-stu-id="f4951-199">results in code first creating a non-nullable timestamp column in the database table.</span></span>

![jj591583_figure07](~/ef6/media/jj591583-figure07.png)

 

## <a name="table-and-column"></a><span data-ttu-id="f4951-201">Таблицы и столбца</span><span class="sxs-lookup"><span data-stu-id="f4951-201">Table and Column</span></span>

<span data-ttu-id="f4951-202">Если вы предоставляете Code First создания базы данных, можно изменить имя таблицы и столбцы, которые он создает.</span><span class="sxs-lookup"><span data-stu-id="f4951-202">If you are letting Code First create the database, you may want to change the name of the tables and columns it is creating.</span></span> <span data-ttu-id="f4951-203">Можно также использовать Code First с существующей базы данных.</span><span class="sxs-lookup"><span data-stu-id="f4951-203">You can also use Code First with an existing database.</span></span> <span data-ttu-id="f4951-204">Но это не всегда так, что имена классов и свойств в вашем домене соответствуют именам таблиц и столбцов базы данных.</span><span class="sxs-lookup"><span data-stu-id="f4951-204">But it's not always the case that the names of the classes and properties in your domain match the names of the tables and columns in your database.</span></span>

<span data-ttu-id="f4951-205">Мой класс называется блог и по соглашению, код сначала рассматривает это приведет к сопоставлению в таблицу с именем блоги.</span><span class="sxs-lookup"><span data-stu-id="f4951-205">My class is named Blog and by convention, code first presumes this will map to a table named Blogs.</span></span> <span data-ttu-id="f4951-206">Если это не так с атрибутом таблицы можно указать имя таблицы.</span><span class="sxs-lookup"><span data-stu-id="f4951-206">If that's not the case you can specify the name of the table with the Table attribute.</span></span> <span data-ttu-id="f4951-207">Здесь к примеру, заметка указывается, что имя таблицы является InternalBlogs.</span><span class="sxs-lookup"><span data-stu-id="f4951-207">Here for example, the annotation is specifying that the table name is InternalBlogs.</span></span>

``` csharp
    [Table("InternalBlogs")]
    public class Blog
```

<span data-ttu-id="f4951-208">Пометка столбца является более детальное представление при указании атрибутов сопоставленного столбца.</span><span class="sxs-lookup"><span data-stu-id="f4951-208">The Column annotation is a more adept in specifying the attributes of a mapped column.</span></span> <span data-ttu-id="f4951-209">Можно указать имя, тип данных или даже порядок, в котором отображается столбец в таблице.</span><span class="sxs-lookup"><span data-stu-id="f4951-209">You can stipulate a name, data type or even the order in which a column appears in the table.</span></span> <span data-ttu-id="f4951-210">Вот пример атрибута.</span><span class="sxs-lookup"><span data-stu-id="f4951-210">Here is an example of the Column attribute.</span></span>

``` csharp
    [Column(“BlogDescription", TypeName="ntext")]
    public String Description {get;set;}
```

<span data-ttu-id="f4951-211">Не путайте атрибут TypeName столбца с типом данных DataAnnotation.</span><span class="sxs-lookup"><span data-stu-id="f4951-211">Don’t confuse Column’s TypeName attribute with the DataType DataAnnotation.</span></span> <span data-ttu-id="f4951-212">Тип данных является заметки, используемый для пользовательского интерфейса и поэтому пропускается при Code First.</span><span class="sxs-lookup"><span data-stu-id="f4951-212">DataType is an annotation used for the UI and is ignored by Code First.</span></span>

<span data-ttu-id="f4951-213">Здесь приведена таблица после его генерируется.</span><span class="sxs-lookup"><span data-stu-id="f4951-213">Here is the table after it’s been regenerated.</span></span> <span data-ttu-id="f4951-214">Имя таблицы изменилось на InternalBlogs и описание столбца из сложного типа теперь BlogDescription.</span><span class="sxs-lookup"><span data-stu-id="f4951-214">The table name has changed to InternalBlogs and Description column from the complex type is now BlogDescription.</span></span> <span data-ttu-id="f4951-215">Так как имя указано в аннотации, код сначала не будет использовать соглашение для имени столбца, начиная с имени сложного типа.</span><span class="sxs-lookup"><span data-stu-id="f4951-215">Because the name was specified in the annotation, code first will not use the convention of starting the column name with the name of the complex type.</span></span>

![jj591583_figure08](~/ef6/media/jj591583-figure08.png)

 

## <a name="databasegenerated"></a><span data-ttu-id="f4951-217">DatabaseGenerated</span><span class="sxs-lookup"><span data-stu-id="f4951-217">DatabaseGenerated</span></span>

<span data-ttu-id="f4951-218">Функции важные базы данных является возможность вычисляемые свойства.</span><span class="sxs-lookup"><span data-stu-id="f4951-218">An important database features is the ability to have computed properties.</span></span> <span data-ttu-id="f4951-219">При сопоставлении Code First классы для таблиц, содержащих вычисляемые столбцы, Entity Framework, чтобы попытаться обновить эти столбцы не нужно.</span><span class="sxs-lookup"><span data-stu-id="f4951-219">If you're mapping your Code First classes to tables that contain computed columns, you don't want Entity Framework to try to update those columns.</span></span> <span data-ttu-id="f4951-220">Но вы хотите EF для получения этих значений из базы данных после были вставлены или обновлены данные.</span><span class="sxs-lookup"><span data-stu-id="f4951-220">But you do want EF to return those values from the database after you've inserted or updated data.</span></span> <span data-ttu-id="f4951-221">DatabaseGenerated заметки можно использовать для пометки этих свойств в классе, а также перечисления вычисленные.</span><span class="sxs-lookup"><span data-stu-id="f4951-221">You can use the DatabaseGenerated annotation to flag those properties in your class along with the Computed enum.</span></span> <span data-ttu-id="f4951-222">Другие перечисления являются None и удостоверений.</span><span class="sxs-lookup"><span data-stu-id="f4951-222">Other enums are None and Identity.</span></span>

``` csharp
    [DatabaseGenerated(DatabaseGenerationOption.Computed)]
    public DateTime DateCreated { get; set; }
```

<span data-ttu-id="f4951-223">Можно использовать базу данных, создается байтов или метки времени столбцов при код сначала создает базу данных, в противном случае вам следует использовать только при указывает на существующие базы данных, так как код сначала не сможет определить формулу для данного вычисляемого столбца.</span><span class="sxs-lookup"><span data-stu-id="f4951-223">You can use database generated on byte or timestamp columns when code first is generating the database, otherwise you should only use this when pointing to existing databases because code first won't be able to determine the formula for the computed column.</span></span>

<span data-ttu-id="f4951-224">Ключевое свойство, которое должно быть целым числом, уже говорилось, по умолчанию станет ключа удостоверения в базе данных.</span><span class="sxs-lookup"><span data-stu-id="f4951-224">You read above that by default, a key property that is an integer will become an identity key in the database.</span></span> <span data-ttu-id="f4951-225">Что бы так же, как присвоить DatabaseGenerated DatabaseGenerationOption.Identity.</span><span class="sxs-lookup"><span data-stu-id="f4951-225">That would be the same as setting DatabaseGenerated to DatabaseGenerationOption.Identity.</span></span> <span data-ttu-id="f4951-226">Если вы не хотите его ключ удостоверения, можно задать значение для DatabaseGenerationOption.None.</span><span class="sxs-lookup"><span data-stu-id="f4951-226">If you do not want it to be an identity key, you can set the value to DatabaseGenerationOption.None.</span></span>

 

## <a name="index"></a><span data-ttu-id="f4951-227">Индекс</span><span class="sxs-lookup"><span data-stu-id="f4951-227">Index</span></span>

> [!NOTE]
> <span data-ttu-id="f4951-228">**EF6.1 и более поздних версий только** -индекс, представляющий атрибут появился в Entity Framework 6.1.</span><span class="sxs-lookup"><span data-stu-id="f4951-228">**EF6.1 Onwards Only** - The Index attribute was introduced in Entity Framework 6.1.</span></span> <span data-ttu-id="f4951-229">При использовании более ранней версии сведения в этом разделе не применяется.</span><span class="sxs-lookup"><span data-stu-id="f4951-229">If you are using an earlier version the information in this section does not apply.</span></span>

<span data-ttu-id="f4951-230">Можно создать индекс для одного или нескольких столбцов с помощью **IndexAttribute**.</span><span class="sxs-lookup"><span data-stu-id="f4951-230">You can create an index on one or more columns using the **IndexAttribute**.</span></span> <span data-ttu-id="f4951-231">Добавление атрибута в один или несколько свойств будет приводит к EF для создания соответствующего индекса в базе данных, при создании базы данных, или сформировать шаблон соответствующего **CreateIndex** вызывает при использовании Code First Migrations.</span><span class="sxs-lookup"><span data-stu-id="f4951-231">Adding the attribute to one or more properties will cause EF to create the corresponding index in the database when it creates the database, or scaffold the corresponding **CreateIndex** calls if you are using Code First Migrations.</span></span>

<span data-ttu-id="f4951-232">Например, следующий код приведет к индекса при создании на основе **Оценка** столбец **сообщения** таблицы в базе данных.</span><span class="sxs-lookup"><span data-stu-id="f4951-232">For example, the following code will result in an index being created on the **Rating** column of the **Posts** table in the database.</span></span>

``` csharp
    public class Post
    {
        public int Id { get; set; }
        public string Title { get; set; }
        public string Content { get; set; }
        [Index]
        public int Rating { get; set; }
        public int BlogId { get; set; }
    }
```

<span data-ttu-id="f4951-233">По умолчанию будут присваиваться индекс **IX\_&lt;имя свойства&gt;**  (IX\_оценка в приведенном выше примере).</span><span class="sxs-lookup"><span data-stu-id="f4951-233">By default, the index will be named **IX\_&lt;property name&gt;** (IX\_Rating in the above example).</span></span> <span data-ttu-id="f4951-234">Можно также указать имя индекса хотя.</span><span class="sxs-lookup"><span data-stu-id="f4951-234">You can also specify a name for the index though.</span></span> <span data-ttu-id="f4951-235">В следующем примере указывается, что индекс должен быть назван **PostRatingIndex**.</span><span class="sxs-lookup"><span data-stu-id="f4951-235">The following example specifies that the index should be named **PostRatingIndex**.</span></span>

``` csharp
    [Index("PostRatingIndex")]
    public int Rating { get; set; }
```

<span data-ttu-id="f4951-236">По умолчанию, неуникальных индексов, но можно использовать **IsUnique** именованный параметр, чтобы указать, что индекс должен быть уникальным.</span><span class="sxs-lookup"><span data-stu-id="f4951-236">By default, indexes are non-unique, but you can use the **IsUnique** named parameter to specify that an index should be unique.</span></span> <span data-ttu-id="f4951-237">В следующем примере представлены уникальный индекс на **пользователя**на имя входа.</span><span class="sxs-lookup"><span data-stu-id="f4951-237">The following example introduces a unique index on a **User**'s login name.</span></span>

``` csharp
    public class User
    {
        public int UserId { get; set; }

        [Index(IsUnique = true)]
        [StringLength(200)]
        public string Username { get; set; }

        public string DisplayName { get; set; }
    }
```

### <a name="multiple-column-indexes"></a><span data-ttu-id="f4951-238">Индексы с несколькими столбцами</span><span class="sxs-lookup"><span data-stu-id="f4951-238">Multiple-Column Indexes</span></span>

<span data-ttu-id="f4951-239">Индексы, которые охватывают несколько столбцов, указываются с помощью тем же именем в несколько заметок индекса для данной таблицы.</span><span class="sxs-lookup"><span data-stu-id="f4951-239">Indexes that span multiple columns are specified by using the same name in multiple Index annotations for a given table.</span></span> <span data-ttu-id="f4951-240">Создавая индексы с несколькими столбцами, необходимо указать порядок для столбцов в индексе.</span><span class="sxs-lookup"><span data-stu-id="f4951-240">When you create multi-column indexes, you need to specify an order for the columns in the index.</span></span> <span data-ttu-id="f4951-241">Например, следующий код создает многостолбцовый индекс для **Оценка** и **BlogId** вызывается **IX\_BlogAndRating**.</span><span class="sxs-lookup"><span data-stu-id="f4951-241">For example, the following code creates a multi-column index on **Rating** and **BlogId** called **IX\_BlogAndRating**.</span></span> <span data-ttu-id="f4951-242">**BlogId** является первым столбцом в индексе и **Оценка** является вторым.</span><span class="sxs-lookup"><span data-stu-id="f4951-242">**BlogId** is the first column in the index and **Rating** is the second.</span></span>

``` csharp
    public class Post
    {
        public int Id { get; set; }
        public string Title { get; set; }
        public string Content { get; set; }
        [Index("IX_BlogIdAndRating", 2)]
        public int Rating { get; set; }
        [Index("IX_BlogIdAndRating", 1)]
        public int BlogId { get; set; }
    }
```

 

## <a name="relationship-attributes-inverseproperty-and-foreignkey"></a><span data-ttu-id="f4951-243">Связь атрибутов: InverseProperty и ForeignKey</span><span class="sxs-lookup"><span data-stu-id="f4951-243">Relationship Attributes: InverseProperty and ForeignKey</span></span>

> [!NOTE]
> <span data-ttu-id="f4951-244">Эта страница содержит сведения о настройке связи в модели Code First с помощью заметок к данным.</span><span class="sxs-lookup"><span data-stu-id="f4951-244">This page provides information about setting up relationships in your Code First model using Data Annotations.</span></span> <span data-ttu-id="f4951-245">Общие сведения о связях в EF и способах доступа к данных и управления ими с помощью связей см. в разделе [связей и свойств навигации](~/ef6/fundamentals/relationships.md). \*</span><span class="sxs-lookup"><span data-stu-id="f4951-245">For general information about relationships in EF and how to access and manipulate data using relationships, see [Relationships & Navigation Properties](~/ef6/fundamentals/relationships.md).\*</span></span>

<span data-ttu-id="f4951-246">Соглашение об первый кода позаботится о наиболее распространенных связи в модели, но существуют случаи, где он нуждается в помощи.</span><span class="sxs-lookup"><span data-stu-id="f4951-246">Code first convention will take care of the most common relationships in your model, but there are some cases where it needs help.</span></span>

<span data-ttu-id="f4951-247">Изменение имени ключевого свойства в класс блог, созданный на проблему с его связь с Post.</span><span class="sxs-lookup"><span data-stu-id="f4951-247">Changing the name of the key property in the Blog class created a problem with its relationship to Post.</span></span> 

<span data-ttu-id="f4951-248">При создании базы данных, код сначала увидит BlogId свойства в классе Post и распознает ее, по соглашению, что он соответствует имени класса, а также «Id», как внешний ключ к классу блога.</span><span class="sxs-lookup"><span data-stu-id="f4951-248">When generating the database, code first sees the BlogId property in the Post class and recognizes it, by the convention that it matches a class name plus “Id”, as a foreign key to the Blog class.</span></span> <span data-ttu-id="f4951-249">Но нет BlogId свойства в классе блога.</span><span class="sxs-lookup"><span data-stu-id="f4951-249">But there is no BlogId property in the blog class.</span></span> <span data-ttu-id="f4951-250">Решением будет создать свойство навигации в блога и использовать внешний DataAnnotation для кода сначала понять, как создать связь между двумя классами, используя свойство Post.BlogId — а также способы указания ограничений в База данных.</span><span class="sxs-lookup"><span data-stu-id="f4951-250">The solution for this is to create a navigation property in the Post and use the Foreign DataAnnotation to help code first understand how to build the relationship between the two classes —using the Post.BlogId property — as well as how to specify constraints in the database.</span></span>

``` csharp
    public class Post
    {
            public int Id { get; set; }
            public string Title { get; set; }
            public DateTime DateCreated { get; set; }
            public string Content { get; set; }
            public int BlogId { get; set; }
            [ForeignKey("BlogId")]
            public Blog Blog { get; set; }
            public ICollection<Comment> Comments { get; set; }
    }
```

<span data-ttu-id="f4951-251">Ограничение в базе данных отображает связь между InternalBlogs.PrimaryTrackingKey и Posts.BlogId.</span><span class="sxs-lookup"><span data-stu-id="f4951-251">The constraint in the database shows a relationship between InternalBlogs.PrimaryTrackingKey and Posts.BlogId.</span></span> 

![jj591583_figure09](~/ef6/media/jj591583-figure09.png)

<span data-ttu-id="f4951-253">InverseProperty используется в том случае, если у вас есть несколько связей между классами.</span><span class="sxs-lookup"><span data-stu-id="f4951-253">The InverseProperty is used when you have multiple relationships between classes.</span></span>

<span data-ttu-id="f4951-254">В классе Post, может потребоваться хранить список кто написал в блоге и кто его изменить.</span><span class="sxs-lookup"><span data-stu-id="f4951-254">In the Post class, you may want to keep track of who wrote a blog post as well as who edited it.</span></span> <span data-ttu-id="f4951-255">Ниже приведены два новых свойства навигации для класса Post.</span><span class="sxs-lookup"><span data-stu-id="f4951-255">Here are two new navigation properties for the Post class.</span></span>

``` csharp
    public Person CreatedBy { get; set; }
    public Person UpdatedBy { get; set; }
```

<span data-ttu-id="f4951-256">Также необходимо добавить в класс Person, ссылается на эти свойства.</span><span class="sxs-lookup"><span data-stu-id="f4951-256">You’ll also need to add in the Person class referenced by these properties.</span></span> <span data-ttu-id="f4951-257">Класс Person имеет свойства навигации обратно к записи, одну для всех записей, написанной person и одну для всех записей, обновлены этим пользователем.</span><span class="sxs-lookup"><span data-stu-id="f4951-257">The Person class has navigation properties back to the Post, one for all of the posts written by the person and one for all of the posts updated by that person.</span></span>

``` csharp
    public class Person
    {
            public int Id { get; set; }
            public string Name { get; set; }
            public List<Post> PostsWritten { get; set; }
            public List<Post> PostsUpdated { get; set; }
    }
```

<span data-ttu-id="f4951-258">Сначала код не сможет сопоставлять свойства в класс сам по себе.</span><span class="sxs-lookup"><span data-stu-id="f4951-258">Code first is not able to match up the properties in the two classes on its own.</span></span> <span data-ttu-id="f4951-259">В таблице базы данных для записей должны иметь один внешний ключ для пользователя, Кем создано и один для UpdatedBy человека, но код сначала создать свойства внешнего ключа четыре будет: Person\_идентификатор, Person\_Id1, Кем создано\_идентификатор и UpdatedBy\_идентификатор.</span><span class="sxs-lookup"><span data-stu-id="f4951-259">The database table for Posts should have one foreign key for the CreatedBy person and one for the UpdatedBy person but code first will create four will foreign key properties: Person\_Id, Person\_Id1, CreatedBy\_Id and UpdatedBy\_Id.</span></span>

![jj591583_figure10](~/ef6/media/jj591583-figure10.png)

<span data-ttu-id="f4951-261">Чтобы устранить эти проблемы, InverseProperty заметки можно использовать для указания выравнивания свойств.</span><span class="sxs-lookup"><span data-stu-id="f4951-261">To fix these problems, you can use the InverseProperty annotation to specify the alignment of the properties.</span></span>

``` csharp
    [InverseProperty("CreatedBy")]
    public List<Post> PostsWritten { get; set; }

    [InverseProperty("UpdatedBy")]
    public List<Post> PostsUpdated { get; set; }
```

<span data-ttu-id="f4951-262">Так как свойство PostsWritten лично знает, что это относится к типу Post, сборка будет связи Post.CreatedBy.</span><span class="sxs-lookup"><span data-stu-id="f4951-262">Because the PostsWritten property in Person knows that this refers to the Post type, it will build the relationship to Post.CreatedBy.</span></span> <span data-ttu-id="f4951-263">Аналогичным образом PostsUpdated будут подключены к Post.UpdatedBy.</span><span class="sxs-lookup"><span data-stu-id="f4951-263">Similarly, PostsUpdated will be connected to Post.UpdatedBy.</span></span> <span data-ttu-id="f4951-264">И код сначала не создает дополнительных внешних ключей.</span><span class="sxs-lookup"><span data-stu-id="f4951-264">And code first will not create the extra foreign keys.</span></span>

![jj591583_figure11](~/ef6/media/jj591583-figure11.png)

 

## <a name="summary"></a><span data-ttu-id="f4951-266">Сводка</span><span class="sxs-lookup"><span data-stu-id="f4951-266">Summary</span></span>

<span data-ttu-id="f4951-267">DataAnnotations не только позволяют описывают проверки на стороне клиента и сервера в классах первый код, но они также позволяют улучшать и даже устранить предположения, которые код сначала облегчат о классов на основе его соглашений.</span><span class="sxs-lookup"><span data-stu-id="f4951-267">DataAnnotations not only let you describe client and server side validation in your code first classes, but they also allow you to enhance and even correct the assumptions that code first will make about your classes based on its conventions.</span></span> <span data-ttu-id="f4951-268">С помощью DataAnnotations вы можете не только обеспечивать создание схемы базы данных, но также можно сопоставить классы первый код и уже существующей базы данных.</span><span class="sxs-lookup"><span data-stu-id="f4951-268">With DataAnnotations you can not only drive database schema generation, but you can also map your code first classes to a pre-existing database.</span></span>

<span data-ttu-id="f4951-269">Хотя они являются достаточно гибкими, помните, что DataAnnotations предоставляют только наиболее часто используемые конфигурации изменения, внесенные в классах первого кода.</span><span class="sxs-lookup"><span data-stu-id="f4951-269">While they are very flexible, keep in mind that DataAnnotations provide only the most commonly needed configuration changes you can make on your code first classes.</span></span> <span data-ttu-id="f4951-270">Чтобы настроить свои классы для некоторых крайних случаев, следует проверить механизм альтернативная конфигурация, Code First Fluent API.</span><span class="sxs-lookup"><span data-stu-id="f4951-270">To configure your classes for some of the edge cases, you should look to the alternate configuration mechanism, Code First’s Fluent API .</span></span>
