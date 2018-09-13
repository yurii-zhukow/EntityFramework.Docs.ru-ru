---
title: Заметки к данным First - EF6 Code
author: divega
ms.date: 10/23/2016
ms.assetid: 80abefbd-23c9-4fce-9cd3-520e5df9856e
ms.openlocfilehash: 38ae52543ed99e5a1c1da7d19a2e15d168e3a1bd
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/13/2018
ms.locfileid: "45490119"
---
# <a name="code-first-data-annotations"></a><span data-ttu-id="cf3e2-102">Заметки данных Code First</span><span class="sxs-lookup"><span data-stu-id="cf3e2-102">Code First Data Annotations</span></span>
> [!NOTE]
> <span data-ttu-id="cf3e2-103">**EF4.1 и более поздних версий только** -функции, интерфейсы API, и т.д., описанных на этой странице появились в версии 4.1 платформы Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="cf3e2-103">**EF4.1 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 4.1.</span></span> <span data-ttu-id="cf3e2-104">При использовании более ранней версии, некоторые или все эти сведения не применяются.</span><span class="sxs-lookup"><span data-stu-id="cf3e2-104">If you are using an earlier version, some or all of this information does not apply.</span></span>

<span data-ttu-id="cf3e2-105">Содержимое на этой странице взят из статьи, первоначально написан Майклом Джули Лерман (\<http://thedatafarm.com>).</span><span class="sxs-lookup"><span data-stu-id="cf3e2-105">The content on this page is adapted from an article originally written by Julie Lerman (\<http://thedatafarm.com>).</span></span>

<span data-ttu-id="cf3e2-106">Entity Framework Code First позволяет использовать собственные классы домена для представления модели EF полагается на выполнять запросы, изменение отслеживания и обновления функций.</span><span class="sxs-lookup"><span data-stu-id="cf3e2-106">Entity Framework Code First allows you to use your own domain classes to represent the model that EF relies on to perform querying, change tracking, and updating functions.</span></span> <span data-ttu-id="cf3e2-107">Код сначала использует шаблон программирования, называется «соглашение относительно настройки».</span><span class="sxs-lookup"><span data-stu-id="cf3e2-107">Code First leverages a programming pattern referred to as 'convention over configuration.'</span></span> <span data-ttu-id="cf3e2-108">Код сначала будет предполагается, что ваши классы соответствуют соглашениям об платформы Entity Framework и в этом случае будут автоматически работать способа выполнения задания его.</span><span class="sxs-lookup"><span data-stu-id="cf3e2-108">Code First will assume that your classes follow the conventions of Entity Framework, and in that case, will automatically work out how to perform it's job.</span></span> <span data-ttu-id="cf3e2-109">Тем не менее если ваши классы не выполняйте эти соглашения, вы можете для добавления конфигурации к классам, чтобы предоставить EF требуемую информацию.</span><span class="sxs-lookup"><span data-stu-id="cf3e2-109">However, if your classes do not follow those conventions, you have the ability to add configurations to your classes to provide EF with the requisite information.</span></span>

<span data-ttu-id="cf3e2-110">Во-первых, код предоставляет два способа добавления этих конфигураций в пользовательские классы.</span><span class="sxs-lookup"><span data-stu-id="cf3e2-110">Code First gives you two ways to add these configurations to your classes.</span></span> <span data-ttu-id="cf3e2-111">Один с помощью простых атрибутов, вызывается DataAnnotations, а второй с помощью Code First Fluent API, который предоставляет способ описания конфигураций, принудительно, в коде.</span><span class="sxs-lookup"><span data-stu-id="cf3e2-111">One is using simple attributes called DataAnnotations, and the second is using Code First’s Fluent API, which provides you with a way to describe configurations imperatively, in code.</span></span>

<span data-ttu-id="cf3e2-112">В этой статье основное внимание уделяется с помощью DataAnnotations (в пространстве имен System.ComponentModel.DataAnnotations) для настройки классов — выделение наиболее общие конфигурации.</span><span class="sxs-lookup"><span data-stu-id="cf3e2-112">This article will focus on using DataAnnotations (in the System.ComponentModel.DataAnnotations namespace) to configure your classes – highlighting the most commonly needed configurations.</span></span> <span data-ttu-id="cf3e2-113">DataAnnotations также понимает ряд приложений .NET, таких как ASP.NET MVC, который позволяет этим приложениям использовать одинаковые примечания для проверки на стороне клиента.</span><span class="sxs-lookup"><span data-stu-id="cf3e2-113">DataAnnotations are also understood by a number of .NET applications, such as ASP.NET MVC which allows these applications to leverage the same annotations for client-side validations.</span></span>


## <a name="the-model"></a><span data-ttu-id="cf3e2-114">Модель</span><span class="sxs-lookup"><span data-stu-id="cf3e2-114">The model</span></span>

<span data-ttu-id="cf3e2-115">Я продемонстрирую DataAnnotations первого кода с парой простых классов: блог и Post.</span><span class="sxs-lookup"><span data-stu-id="cf3e2-115">I’ll demonstrate Code First DataAnnotations with a simple pair of classes: Blog and Post.</span></span>

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

<span data-ttu-id="cf3e2-116">Значения, блог и Post классы удобно соответствуют соглашению об первый код и требуют нет изменений для включения совместимости EF.</span><span class="sxs-lookup"><span data-stu-id="cf3e2-116">As they are, the Blog and Post classes conveniently follow code first convention and require no tweaks to enable EF compatability.</span></span> <span data-ttu-id="cf3e2-117">Тем не менее можно также использовать заметки для предоставления EF Дополнительные сведения о классах и базы данных, к которому они сопоставлены.</span><span class="sxs-lookup"><span data-stu-id="cf3e2-117">However, you can also use the annotations to provide more information to EF about the classes and the database to which they map.</span></span>

 

## <a name="key"></a><span data-ttu-id="cf3e2-118">Ключ</span><span class="sxs-lookup"><span data-stu-id="cf3e2-118">Key</span></span>

<span data-ttu-id="cf3e2-119">Платформа Entity Framework использует каждой сущности, который используется для отслеживания сущности значение ключа.</span><span class="sxs-lookup"><span data-stu-id="cf3e2-119">Entity Framework relies on every entity having a key value that is used for entity tracking.</span></span> <span data-ttu-id="cf3e2-120">Одно соглашение о Code First — неявное ключевых свойств; Код сначала ищет свойство с именем «Id», или сочетание имени класса и «Id», например «BlogId».</span><span class="sxs-lookup"><span data-stu-id="cf3e2-120">One convention of Code First is implicit key properties; Code First will look for a property named “Id”, or a combination of class name and “Id”, such as “BlogId”.</span></span> <span data-ttu-id="cf3e2-121">Это свойство сопоставляется столбцом первичного ключа в базе данных.</span><span class="sxs-lookup"><span data-stu-id="cf3e2-121">This property will map to a primary key column in the database.</span></span>

<span data-ttu-id="cf3e2-122">Блог и Post классов следуют соглашению.</span><span class="sxs-lookup"><span data-stu-id="cf3e2-122">The Blog and Post classes both follow this convention.</span></span> <span data-ttu-id="cf3e2-123">Что делать, если они не?</span><span class="sxs-lookup"><span data-stu-id="cf3e2-123">What if they didn’t?</span></span> <span data-ttu-id="cf3e2-124">Что делать, если имя используется блог *PrimaryTrackingKey* вместо, или даже *foo*?</span><span class="sxs-lookup"><span data-stu-id="cf3e2-124">What if Blog used the name *PrimaryTrackingKey* instead, or even *foo*?</span></span> <span data-ttu-id="cf3e2-125">Если код сначала не удается найти свойство, которое соответствует этому соглашению вызовет исключение из-за требования платформы Entity Framework, что необходимо иметь ключевое свойство.</span><span class="sxs-lookup"><span data-stu-id="cf3e2-125">If code first does not find a property that matches this convention it will throw an exception because of Entity Framework’s requirement that you must have a key property.</span></span> <span data-ttu-id="cf3e2-126">Ключа заметки можно использовать для указания того, какое свойство будет использоваться в качестве EntityKey.</span><span class="sxs-lookup"><span data-stu-id="cf3e2-126">You can use the key annotation to specify which property is to be used as the EntityKey.</span></span>

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

<span data-ttu-id="cf3e2-127">Если вы являетесь сначала с помощью кода — использовать функцию создания базы данных, в блоге таблице будет иметь столбец первичного ключа с именем PrimaryTrackingKey, который также определен как удостоверение по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="cf3e2-127">If you are using code first’s database generation feature, the Blog table will have a primary key column named PrimaryTrackingKey, which is also defined as Identity by default.</span></span>

![Блог таблицы с первичным ключом](~/ef6/media/jj591583-figure01.png)

### <a name="composite-keys"></a><span data-ttu-id="cf3e2-129">Составные ключи</span><span class="sxs-lookup"><span data-stu-id="cf3e2-129">Composite keys</span></span>

<span data-ttu-id="cf3e2-130">Платформа Entity Framework поддерживает составные ключи - первичные ключи, которые состоят из нескольких свойств.</span><span class="sxs-lookup"><span data-stu-id="cf3e2-130">Entity Framework supports composite keys - primary keys that consist of more than one property.</span></span> <span data-ttu-id="cf3e2-131">Например можно создать первичный ключ которой представляет собой сочетание PassportNumber и IssuingCountry класс Passport.</span><span class="sxs-lookup"><span data-stu-id="cf3e2-131">For example, you could have a Passport class whose primary key is a combination of PassportNumber and IssuingCountry.</span></span>

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

<span data-ttu-id="cf3e2-132">Попытка использовать приведенного выше класса в модели EF приведет к `InvalidOperationException`:</span><span class="sxs-lookup"><span data-stu-id="cf3e2-132">Attempting to use the above class in your EF model would result in an `InvalidOperationException`:</span></span>

<span data-ttu-id="cf3e2-133">*Не удалось определить составного первичного ключа упорядочения для типа «Passport». Чтобы указать порядок для составные первичные ключи, используя ColumnAttribute или метод HasKey.*</span><span class="sxs-lookup"><span data-stu-id="cf3e2-133">*Unable to determine composite primary key ordering for type 'Passport'. Use the ColumnAttribute or the HasKey method to specify an order for composite primary keys.*</span></span>

<span data-ttu-id="cf3e2-134">Чтобы использовать составные ключи, Entity Framework необходимо определять порядок ключевых свойств.</span><span class="sxs-lookup"><span data-stu-id="cf3e2-134">In order to use composite keys, Entity Framework requires you to define an order for the key properties.</span></span> <span data-ttu-id="cf3e2-135">Это можно сделать с помощью заметок к столбцам, чтобы задать порядок.</span><span class="sxs-lookup"><span data-stu-id="cf3e2-135">You can do this by using the Column annotation to specify an order.</span></span>

>[!NOTE]
> <span data-ttu-id="cf3e2-136">Порядок сортировки является относительным (а не на базе индекса), чтобы использовать все значения.</span><span class="sxs-lookup"><span data-stu-id="cf3e2-136">The order value is relative (rather than index based) so any values can be used.</span></span> <span data-ttu-id="cf3e2-137">Например 100 и 200 подойдет вместо 1 и 2.</span><span class="sxs-lookup"><span data-stu-id="cf3e2-137">For example, 100 and 200 would be acceptable in place of 1 and 2.</span></span>

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

<span data-ttu-id="cf3e2-138">При наличии сущностей с помощью составного внешние ключи, необходимо указать тот же столбец, упорядочение, которое использовалось для соответствующего свойства первичного ключа.</span><span class="sxs-lookup"><span data-stu-id="cf3e2-138">If you have entities with composite foreign keys, then you must specify the same column ordering that you used for the corresponding primary key properties.</span></span>

<span data-ttu-id="cf3e2-139">Только относительный порядок в пределах свойств внешнего ключа должен быть тем же, точные значения, присвоенные **порядок** не обязательно должны совпадать.</span><span class="sxs-lookup"><span data-stu-id="cf3e2-139">Only the relative ordering within the foreign key properties needs to be the same, the exact values assigned to **Order** do not need to match.</span></span> <span data-ttu-id="cf3e2-140">Например в следующем классе, 3 и 4 может использоваться вместо 1 и 2.</span><span class="sxs-lookup"><span data-stu-id="cf3e2-140">For example, in the following class, 3 and 4 could be used in place of 1 and 2.</span></span>

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

## <a name="required"></a><span data-ttu-id="cf3e2-141">Обязательно</span><span class="sxs-lookup"><span data-stu-id="cf3e2-141">Required</span></span>

<span data-ttu-id="cf3e2-142">Требуется аннотация указывает EF, что определенное свойство является обязательным.</span><span class="sxs-lookup"><span data-stu-id="cf3e2-142">The Required annotation tells EF that a particular property is required.</span></span>

<span data-ttu-id="cf3e2-143">Добавление необходимых для свойства Title приведет к EF (и MVC), чтобы убедиться, что свойство имеет данные в ней.</span><span class="sxs-lookup"><span data-stu-id="cf3e2-143">Adding Required to the Title property will force EF (and MVC) to ensure that the property has data in it.</span></span>

``` csharp
    [Required]
    public string Title { get; set; }
```

<span data-ttu-id="cf3e2-144">Нет дополнительных нет изменений кода или разметки в приложении, MVC-приложении будет выполнять проверки на стороне клиента, даже динамическое создание сообщения с использованием имени свойства и заметки.</span><span class="sxs-lookup"><span data-stu-id="cf3e2-144">With no additional no code or markup changes in the application, an MVC application will perform client side validation, even dynamically building a message using the property and annotation names.</span></span>

![Создать страницу с названием является обязательным ошибки](~/ef6/media/jj591583-figure02.png)

<span data-ttu-id="cf3e2-146">Обязательный атрибут также повлияет на создаваемой базы данных, сделав сопоставленного свойства не допускающие значения NULL.</span><span class="sxs-lookup"><span data-stu-id="cf3e2-146">The Required attribute will also affect the generated database by making the mapped property non-nullable.</span></span> <span data-ttu-id="cf3e2-147">Обратите внимание на то, что поле заголовка изменилось на «не null».</span><span class="sxs-lookup"><span data-stu-id="cf3e2-147">Notice that the Title field has changed to “not null”.</span></span>

>[!NOTE]
> <span data-ttu-id="cf3e2-148">В некоторых случаях может оказаться невозможным для столбца в базе данных и не допускало, несмотря на то, что свойство является обязательным.</span><span class="sxs-lookup"><span data-stu-id="cf3e2-148">In some cases it may not be possible for the column in the database to be non-nullable even though the property is required.</span></span> <span data-ttu-id="cf3e2-149">Например, когда с помощью данных стратегии наследование TPH для нескольких типов хранится в одной таблице.</span><span class="sxs-lookup"><span data-stu-id="cf3e2-149">For example, when using a TPH inheritance strategy data for multiple types is stored in a single table.</span></span> <span data-ttu-id="cf3e2-150">Если производный тип содержит свойство столбца невозможно не допускающие значения NULL, так как не все типы в иерархии, это свойство будет задано.</span><span class="sxs-lookup"><span data-stu-id="cf3e2-150">If a derived type includes a required property the column cannot be made non-nullable since not all types in the hierarchy will have this property.</span></span>

 

![Блоги по таблице](~/ef6/media/jj591583-figure03.png)

 

## <a name="maxlength-and-minlength"></a><span data-ttu-id="cf3e2-152">MaxLength и MinLength</span><span class="sxs-lookup"><span data-stu-id="cf3e2-152">MaxLength and MinLength</span></span>

<span data-ttu-id="cf3e2-153">MaxLength и MinLength атрибуты позволяют пользователю указать дополнительных свойств в комплексе, как это делалось с обязательным.</span><span class="sxs-lookup"><span data-stu-id="cf3e2-153">The MaxLength and MinLength attributes allow you to specify additional property validations, just as you did with Required.</span></span>

<span data-ttu-id="cf3e2-154">Вот BloggerName с требования к длине.</span><span class="sxs-lookup"><span data-stu-id="cf3e2-154">Here is the BloggerName with length requirements.</span></span> <span data-ttu-id="cf3e2-155">Пример также демонстрирует использование атрибутов.</span><span class="sxs-lookup"><span data-stu-id="cf3e2-155">The example also demonstrates how to combine attributes.</span></span>

``` csharp
    [MaxLength(10),MinLength(5)]
    public string BloggerName { get; set; }
```

<span data-ttu-id="cf3e2-156">Заметка MaxLength повлияет на базе данных путем задания свойства длины 10.</span><span class="sxs-lookup"><span data-stu-id="cf3e2-156">The MaxLength annotation will impact the database by setting the property’s length to 10.</span></span>

![Блоги таблица, показывающая Максимальная длина столбца BloggerName](~/ef6/media/jj591583-figure04.png)

<span data-ttu-id="cf3e2-158">Заметки клиентские MVC и EF 4.1 заметки на сервере оба учитывает проверки снова динамическое создание сообщение об ошибке: «поле BloggerName должно быть типом строки или массива с максимальной длиной "10".» Это сообщение имеет несколько большую длину.</span><span class="sxs-lookup"><span data-stu-id="cf3e2-158">MVC client-side annotation and EF 4.1 server-side annotation will both honor this validation, again dynamically building an error message: “The field BloggerName must be a string or array type with a maximum length of '10'.” That message is a little long.</span></span> <span data-ttu-id="cf3e2-159">Многие заметки позволяют указать сообщение об ошибке с атрибутом ErrorMessage.</span><span class="sxs-lookup"><span data-stu-id="cf3e2-159">Many annotations let you specify an error message with the ErrorMessage attribute.</span></span>

``` csharp
    [MaxLength(10, ErrorMessage="BloggerName must be 10 characters or less"),MinLength(5)]
    public string BloggerName { get; set; }
```

<span data-ttu-id="cf3e2-160">Сообщение об ошибке можно также указать необходимые заметки.</span><span class="sxs-lookup"><span data-stu-id="cf3e2-160">You can also specify ErrorMessage in the Required annotation.</span></span>

![Создание страницы с настраиваемым сообщением об ошибке](~/ef6/media/jj591583-figure05.png)

 

## <a name="notmapped"></a><span data-ttu-id="cf3e2-162">NotMapped</span><span class="sxs-lookup"><span data-stu-id="cf3e2-162">NotMapped</span></span>

<span data-ttu-id="cf3e2-163">Соглашение об первый код определяет, что каждое свойство, которое имеет тип данных, поддерживаемых представляется в базе данных.</span><span class="sxs-lookup"><span data-stu-id="cf3e2-163">Code first convention dictates that every property that is of a supported data type is represented in the database.</span></span> <span data-ttu-id="cf3e2-164">Но это не всегда так, в ваших приложениях.</span><span class="sxs-lookup"><span data-stu-id="cf3e2-164">But this isn’t always the case in your applications.</span></span> <span data-ttu-id="cf3e2-165">Например можно использовать свойство в классе блог, который создает код, основанный на поля Title и BloggerName.</span><span class="sxs-lookup"><span data-stu-id="cf3e2-165">For example you might have a property in the Blog class that creates a code based on the Title and BloggerName fields.</span></span> <span data-ttu-id="cf3e2-166">Это свойство можно создавать динамически и не должны храниться.</span><span class="sxs-lookup"><span data-stu-id="cf3e2-166">That property can be created dynamically and does not need to be stored.</span></span> <span data-ttu-id="cf3e2-167">Можно пометить все свойства, которые не сопоставляются с аннотацией NotMapped, например, это свойство BlogCode базы данных.</span><span class="sxs-lookup"><span data-stu-id="cf3e2-167">You can mark any properties that do not map to the database with the NotMapped annotation such as this BlogCode property.</span></span>

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

 

## <a name="complextype"></a><span data-ttu-id="cf3e2-168">ComplexType</span><span class="sxs-lookup"><span data-stu-id="cf3e2-168">ComplexType</span></span>

<span data-ttu-id="cf3e2-169">Нередко для описания сущности предметной области через набор классов и затем уровня этих классов для описания в завершенную сущность.</span><span class="sxs-lookup"><span data-stu-id="cf3e2-169">It’s not uncommon to describe your domain entities across a set of classes and then layer those classes to describe a complete entity.</span></span> <span data-ttu-id="cf3e2-170">Например может добавьте класс с именем BlogDetails в модель.</span><span class="sxs-lookup"><span data-stu-id="cf3e2-170">For example, you may add a class called BlogDetails to your model.</span></span>

``` csharp
    public class BlogDetails
    {
        public DateTime? DateCreated { get; set; }

        [MaxLength(250)]
        public string Description { get; set; }
    }
```

<span data-ttu-id="cf3e2-171">Обратите внимание на то, что BlogDetails не имеет ключевого свойства любого типа.</span><span class="sxs-lookup"><span data-stu-id="cf3e2-171">Notice that BlogDetails does not have any type of key property.</span></span> <span data-ttu-id="cf3e2-172">В предметно-ориентированное проектирование BlogDetails упоминается как объект значения.</span><span class="sxs-lookup"><span data-stu-id="cf3e2-172">In domain driven design, BlogDetails is referred to as a value object.</span></span> <span data-ttu-id="cf3e2-173">Платформа Entity Framework ссылается на объекты-значения как сложные типы.</span><span class="sxs-lookup"><span data-stu-id="cf3e2-173">Entity Framework refers to value objects as complex types.</span></span>  <span data-ttu-id="cf3e2-174">Сложные типы не отслеживается сами по себе.</span><span class="sxs-lookup"><span data-stu-id="cf3e2-174">Complex types cannot be tracked on their own.</span></span>

<span data-ttu-id="cf3e2-175">Тем не менее как свойство в классе блог BlogDetails, он будет отслеживаться как часть объекта блога.</span><span class="sxs-lookup"><span data-stu-id="cf3e2-175">However as a property in the Blog class, BlogDetails it will be tracked as part of a Blog object.</span></span> <span data-ttu-id="cf3e2-176">В порядке для code first для распознавания это BlogDetails класс необходимо пометить как ComplexType.</span><span class="sxs-lookup"><span data-stu-id="cf3e2-176">In order for code first to recognize this, you must mark the BlogDetails class as a ComplexType.</span></span>

``` csharp
    [ComplexType]
    public class BlogDetails
    {
        public DateTime? DateCreated { get; set; }

        [MaxLength(250)]
        public string Description { get; set; }
    }
```

<span data-ttu-id="cf3e2-177">Теперь можно добавить свойство в блоге классу для представления BlogDetails для этого блога.</span><span class="sxs-lookup"><span data-stu-id="cf3e2-177">Now you can add a property in the Blog class to represent the BlogDetails for that blog.</span></span>

``` csharp
        public BlogDetails BlogDetail { get; set; }
```

<span data-ttu-id="cf3e2-178">В базе данных блог таблица будет содержать все свойства блога, включая свойства, содержащиеся в его свойстве BlogDetail.</span><span class="sxs-lookup"><span data-stu-id="cf3e2-178">In the database, the Blog table will contain all of the properties of the blog including the properties contained in its BlogDetail property.</span></span> <span data-ttu-id="cf3e2-179">По умолчанию каждый из них предшествует имя сложного типа, BlogDetail.</span><span class="sxs-lookup"><span data-stu-id="cf3e2-179">By default, each one is preceded with the name of the complex type, BlogDetail.</span></span>

![Блог таблицы со сложным типом](~/ef6/media/jj591583-figure06.png)

<span data-ttu-id="cf3e2-181">Другой Интересно отметить, что несмотря на то, что свойство DateCreated было определено как не допускающие значения NULL даты и времени в классе, поле соответствующей базы данных допускает значения NULL.</span><span class="sxs-lookup"><span data-stu-id="cf3e2-181">Another interesting note is that although the DateCreated property was defined as a non-nullable DateTime in the class, the relevant database field is nullable.</span></span> <span data-ttu-id="cf3e2-182">Если вы хотите повлиять на схему базы данных, необходимо использовать необходимые заметки.</span><span class="sxs-lookup"><span data-stu-id="cf3e2-182">You must use the Required annotation if you wish to affect the database schema.</span></span>

 

## <a name="concurrencycheck"></a><span data-ttu-id="cf3e2-183">ConcurrencyCheck</span><span class="sxs-lookup"><span data-stu-id="cf3e2-183">ConcurrencyCheck</span></span>

<span data-ttu-id="cf3e2-184">Заметка ConcurrencyCheck позволяет пометить одно или несколько свойств, используемый для проверки в базе данных, когда пользователь изменяет или удаляет сущность параллелизма.</span><span class="sxs-lookup"><span data-stu-id="cf3e2-184">The ConcurrencyCheck annotation allows you to flag one or more properties to be used for concurrency checking in the database when a user edits or deletes an entity.</span></span> <span data-ttu-id="cf3e2-185">Если вы работали с конструктором, EF, это соответствует соглашению присвоить свойство ConcurrencyMode значение Fixed.</span><span class="sxs-lookup"><span data-stu-id="cf3e2-185">If you've been working with the EF Designer, this aligns with setting a property's ConcurrencyMode to Fixed.</span></span>

<span data-ttu-id="cf3e2-186">Давайте посмотрим, как работает ConcurrencyCheck, добавьте его в свойство BloggerName.</span><span class="sxs-lookup"><span data-stu-id="cf3e2-186">Let’s see how ConcurrencyCheck works by adding it to the BloggerName property.</span></span>

``` csharp
    [ConcurrencyCheck, MaxLength(10, ErrorMessage="BloggerName must be 10 characters or less"),MinLength(5)]
    public string BloggerName { get; set; }
```

<span data-ttu-id="cf3e2-187">При вызове метода SaveChanges, из-за Аннотация ConcurrencyCheck в поле BloggerName исходное значение этого свойства будет использоваться в обновлении.</span><span class="sxs-lookup"><span data-stu-id="cf3e2-187">When SaveChanges is called, because of the ConcurrencyCheck annotation on the BloggerName field, the original value of that property will be used in the update.</span></span> <span data-ttu-id="cf3e2-188">Команда попытается найти правильные строки путем фильтрации не только на значение ключа, но также и на ее оригинальном значении BloggerName.</span><span class="sxs-lookup"><span data-stu-id="cf3e2-188">The command will attempt to locate the correct row by filtering not only on the key value but also on the original value of BloggerName.</span></span>  <span data-ttu-id="cf3e2-189">Ниже приведены важные обновления команда, Посланная в базу данных, где вы увидите, команда будет обновить строку, которая имеет PrimaryTrackingKey — 1 и BloggerName «Julie», который был исходное значение, при блоге был извлечен из базы данных.</span><span class="sxs-lookup"><span data-stu-id="cf3e2-189">Here are the critical parts of the UPDATE command sent to the database, where you can see the command will update the row that has a PrimaryTrackingKey is 1 and a BloggerName of “Julie” which was the original value when that blog was retrieved from the database.</span></span>

``` SQL
    where (([PrimaryTrackingKey] = @4) and ([BloggerName] = @5))
    @4=1,@5=N'Julie'
```

<span data-ttu-id="cf3e2-190">Если кто-то изменил название блоггер для блога, в то же время, это обновление завершится сбоем, и вы получите DbUpdateConcurrencyException, которые потребуются для обработки.</span><span class="sxs-lookup"><span data-stu-id="cf3e2-190">If someone has changed the blogger name for that blog in the meantime, this update will fail and you’ll get a DbUpdateConcurrencyException that you'll need to handle.</span></span>

 

## <a name="timestamp"></a><span data-ttu-id="cf3e2-191">Метка времени</span><span class="sxs-lookup"><span data-stu-id="cf3e2-191">TimeStamp</span></span>

<span data-ttu-id="cf3e2-192">Это чаще всего используется поля rowversion или метку времени для проверки параллелизма.</span><span class="sxs-lookup"><span data-stu-id="cf3e2-192">It's more common to use rowversion or timestamp fields for concurrency checking.</span></span> <span data-ttu-id="cf3e2-193">Но вместо использования ConcurrencyCheck заметки, можно использовать более конкретные заметки отметки времени, до тех пор, пока тип свойства — массив байтов.</span><span class="sxs-lookup"><span data-stu-id="cf3e2-193">But rather than using the ConcurrencyCheck annotation, you can use the more specific TimeStamp annotation as long as the type of the property is byte array.</span></span> <span data-ttu-id="cf3e2-194">Код сначала будет обрабатывать Timestamp свойства так же, как свойства ConcurrencyCheck, но также гарантирует, что поле базы данных, код сначала создает не допускающие значения NULL.</span><span class="sxs-lookup"><span data-stu-id="cf3e2-194">Code first will treat Timestamp properties the same as ConcurrencyCheck properties, but it will also ensure that the database field that code first generates is non-nullable.</span></span> <span data-ttu-id="cf3e2-195">В одном классе может иметь только одно свойство метки времени.</span><span class="sxs-lookup"><span data-stu-id="cf3e2-195">You can only have one timestamp property in a given class.</span></span>

<span data-ttu-id="cf3e2-196">Добавление в класс блог следующее свойство:</span><span class="sxs-lookup"><span data-stu-id="cf3e2-196">Adding the following property to the Blog class:</span></span>

``` csharp
    [Timestamp]
    public Byte[] TimeStamp { get; set; }
```

<span data-ttu-id="cf3e2-197">в результате получается код сначала создается столбец типа timestamp, не допускающие значения NULL в таблице базы данных.</span><span class="sxs-lookup"><span data-stu-id="cf3e2-197">results in code first creating a non-nullable timestamp column in the database table.</span></span>

![Блоги по таблице с столбец временной метки](~/ef6/media/jj591583-figure07.png)

 

## <a name="table-and-column"></a><span data-ttu-id="cf3e2-199">Таблицы и столбца</span><span class="sxs-lookup"><span data-stu-id="cf3e2-199">Table and Column</span></span>

<span data-ttu-id="cf3e2-200">Если вы предоставляете Code First создания базы данных, можно изменить имя таблицы и столбцы, которые он создает.</span><span class="sxs-lookup"><span data-stu-id="cf3e2-200">If you are letting Code First create the database, you may want to change the name of the tables and columns it is creating.</span></span> <span data-ttu-id="cf3e2-201">Можно также использовать Code First с существующей базы данных.</span><span class="sxs-lookup"><span data-stu-id="cf3e2-201">You can also use Code First with an existing database.</span></span> <span data-ttu-id="cf3e2-202">Но это не всегда так, что имена классов и свойств в вашем домене соответствуют именам таблиц и столбцов базы данных.</span><span class="sxs-lookup"><span data-stu-id="cf3e2-202">But it's not always the case that the names of the classes and properties in your domain match the names of the tables and columns in your database.</span></span>

<span data-ttu-id="cf3e2-203">Мой класс называется блог и по соглашению, код сначала рассматривает это приведет к сопоставлению в таблицу с именем блоги.</span><span class="sxs-lookup"><span data-stu-id="cf3e2-203">My class is named Blog and by convention, code first presumes this will map to a table named Blogs.</span></span> <span data-ttu-id="cf3e2-204">Если это не так с атрибутом таблицы можно указать имя таблицы.</span><span class="sxs-lookup"><span data-stu-id="cf3e2-204">If that's not the case you can specify the name of the table with the Table attribute.</span></span> <span data-ttu-id="cf3e2-205">Здесь к примеру, заметка указывается, что имя таблицы является InternalBlogs.</span><span class="sxs-lookup"><span data-stu-id="cf3e2-205">Here for example, the annotation is specifying that the table name is InternalBlogs.</span></span>

``` csharp
    [Table("InternalBlogs")]
    public class Blog
```

<span data-ttu-id="cf3e2-206">Пометка столбца является более детальное представление при указании атрибутов сопоставленного столбца.</span><span class="sxs-lookup"><span data-stu-id="cf3e2-206">The Column annotation is a more adept in specifying the attributes of a mapped column.</span></span> <span data-ttu-id="cf3e2-207">Можно указать имя, тип данных или даже порядок, в котором отображается столбец в таблице.</span><span class="sxs-lookup"><span data-stu-id="cf3e2-207">You can stipulate a name, data type or even the order in which a column appears in the table.</span></span> <span data-ttu-id="cf3e2-208">Вот пример атрибута.</span><span class="sxs-lookup"><span data-stu-id="cf3e2-208">Here is an example of the Column attribute.</span></span>

``` csharp
    [Column(“BlogDescription", TypeName="ntext")]
    public String Description {get;set;}
```

<span data-ttu-id="cf3e2-209">Не путайте атрибут TypeName столбца с типом данных DataAnnotation.</span><span class="sxs-lookup"><span data-stu-id="cf3e2-209">Don’t confuse Column’s TypeName attribute with the DataType DataAnnotation.</span></span> <span data-ttu-id="cf3e2-210">Тип данных является заметки, используемый для пользовательского интерфейса и поэтому пропускается при Code First.</span><span class="sxs-lookup"><span data-stu-id="cf3e2-210">DataType is an annotation used for the UI and is ignored by Code First.</span></span>

<span data-ttu-id="cf3e2-211">Здесь приведена таблица после его генерируется.</span><span class="sxs-lookup"><span data-stu-id="cf3e2-211">Here is the table after it’s been regenerated.</span></span> <span data-ttu-id="cf3e2-212">Имя таблицы изменилось на InternalBlogs и описание столбца из сложного типа теперь BlogDescription.</span><span class="sxs-lookup"><span data-stu-id="cf3e2-212">The table name has changed to InternalBlogs and Description column from the complex type is now BlogDescription.</span></span> <span data-ttu-id="cf3e2-213">Так как имя указано в аннотации, код сначала не будет использовать соглашение для имени столбца, начиная с имени сложного типа.</span><span class="sxs-lookup"><span data-stu-id="cf3e2-213">Because the name was specified in the annotation, code first will not use the convention of starting the column name with the name of the complex type.</span></span>

![Блоги таблиц и столбцов, которые переименован](~/ef6/media/jj591583-figure08.png)

 

## <a name="databasegenerated"></a><span data-ttu-id="cf3e2-215">DatabaseGenerated</span><span class="sxs-lookup"><span data-stu-id="cf3e2-215">DatabaseGenerated</span></span>

<span data-ttu-id="cf3e2-216">Функции важные базы данных является возможность вычисляемые свойства.</span><span class="sxs-lookup"><span data-stu-id="cf3e2-216">An important database features is the ability to have computed properties.</span></span> <span data-ttu-id="cf3e2-217">При сопоставлении Code First классы для таблиц, содержащих вычисляемые столбцы, Entity Framework, чтобы попытаться обновить эти столбцы не нужно.</span><span class="sxs-lookup"><span data-stu-id="cf3e2-217">If you're mapping your Code First classes to tables that contain computed columns, you don't want Entity Framework to try to update those columns.</span></span> <span data-ttu-id="cf3e2-218">Но вы хотите EF для получения этих значений из базы данных после были вставлены или обновлены данные.</span><span class="sxs-lookup"><span data-stu-id="cf3e2-218">But you do want EF to return those values from the database after you've inserted or updated data.</span></span> <span data-ttu-id="cf3e2-219">DatabaseGenerated заметки можно использовать для пометки этих свойств в классе, а также перечисления вычисленные.</span><span class="sxs-lookup"><span data-stu-id="cf3e2-219">You can use the DatabaseGenerated annotation to flag those properties in your class along with the Computed enum.</span></span> <span data-ttu-id="cf3e2-220">Другие перечисления являются None и удостоверений.</span><span class="sxs-lookup"><span data-stu-id="cf3e2-220">Other enums are None and Identity.</span></span>

``` csharp
    [DatabaseGenerated(DatabaseGenerationOption.Computed)]
    public DateTime DateCreated { get; set; }
```

<span data-ttu-id="cf3e2-221">Можно использовать базу данных, создается байтов или метки времени столбцов при код сначала создает базу данных, в противном случае вам следует использовать только при указывает на существующие базы данных, так как код сначала не сможет определить формулу для данного вычисляемого столбца.</span><span class="sxs-lookup"><span data-stu-id="cf3e2-221">You can use database generated on byte or timestamp columns when code first is generating the database, otherwise you should only use this when pointing to existing databases because code first won't be able to determine the formula for the computed column.</span></span>

<span data-ttu-id="cf3e2-222">Ключевое свойство, которое должно быть целым числом, уже говорилось, по умолчанию станет ключа удостоверения в базе данных.</span><span class="sxs-lookup"><span data-stu-id="cf3e2-222">You read above that by default, a key property that is an integer will become an identity key in the database.</span></span> <span data-ttu-id="cf3e2-223">Что бы так же, как присвоить DatabaseGenerated DatabaseGenerationOption.Identity.</span><span class="sxs-lookup"><span data-stu-id="cf3e2-223">That would be the same as setting DatabaseGenerated to DatabaseGenerationOption.Identity.</span></span> <span data-ttu-id="cf3e2-224">Если вы не хотите его ключ удостоверения, можно задать значение для DatabaseGenerationOption.None.</span><span class="sxs-lookup"><span data-stu-id="cf3e2-224">If you do not want it to be an identity key, you can set the value to DatabaseGenerationOption.None.</span></span>

 

## <a name="index"></a><span data-ttu-id="cf3e2-225">Индекс</span><span class="sxs-lookup"><span data-stu-id="cf3e2-225">Index</span></span>

> [!NOTE]
> <span data-ttu-id="cf3e2-226">**EF6.1 и более поздних версий только** -индекс, представляющий атрибут появился в Entity Framework 6.1.</span><span class="sxs-lookup"><span data-stu-id="cf3e2-226">**EF6.1 Onwards Only** - The Index attribute was introduced in Entity Framework 6.1.</span></span> <span data-ttu-id="cf3e2-227">При использовании более ранней версии сведения в этом разделе не применяется.</span><span class="sxs-lookup"><span data-stu-id="cf3e2-227">If you are using an earlier version the information in this section does not apply.</span></span>

<span data-ttu-id="cf3e2-228">Можно создать индекс для одного или нескольких столбцов с помощью **IndexAttribute**.</span><span class="sxs-lookup"><span data-stu-id="cf3e2-228">You can create an index on one or more columns using the **IndexAttribute**.</span></span> <span data-ttu-id="cf3e2-229">Добавление атрибута в один или несколько свойств будет приводит к EF для создания соответствующего индекса в базе данных, при создании базы данных, или сформировать шаблон соответствующего **CreateIndex** вызывает при использовании Code First Migrations.</span><span class="sxs-lookup"><span data-stu-id="cf3e2-229">Adding the attribute to one or more properties will cause EF to create the corresponding index in the database when it creates the database, or scaffold the corresponding **CreateIndex** calls if you are using Code First Migrations.</span></span>

<span data-ttu-id="cf3e2-230">Например, следующий код приведет к индекса при создании на основе **Оценка** столбец **сообщения** таблицы в базе данных.</span><span class="sxs-lookup"><span data-stu-id="cf3e2-230">For example, the following code will result in an index being created on the **Rating** column of the **Posts** table in the database.</span></span>

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

<span data-ttu-id="cf3e2-231">По умолчанию будут присваиваться индекс **IX\_&lt;имя свойства&gt;**  (IX\_оценка в приведенном выше примере).</span><span class="sxs-lookup"><span data-stu-id="cf3e2-231">By default, the index will be named **IX\_&lt;property name&gt;** (IX\_Rating in the above example).</span></span> <span data-ttu-id="cf3e2-232">Можно также указать имя индекса хотя.</span><span class="sxs-lookup"><span data-stu-id="cf3e2-232">You can also specify a name for the index though.</span></span> <span data-ttu-id="cf3e2-233">В следующем примере указывается, что индекс должен быть назван **PostRatingIndex**.</span><span class="sxs-lookup"><span data-stu-id="cf3e2-233">The following example specifies that the index should be named **PostRatingIndex**.</span></span>

``` csharp
    [Index("PostRatingIndex")]
    public int Rating { get; set; }
```

<span data-ttu-id="cf3e2-234">По умолчанию, неуникальных индексов, но можно использовать **IsUnique** именованный параметр, чтобы указать, что индекс должен быть уникальным.</span><span class="sxs-lookup"><span data-stu-id="cf3e2-234">By default, indexes are non-unique, but you can use the **IsUnique** named parameter to specify that an index should be unique.</span></span> <span data-ttu-id="cf3e2-235">В следующем примере представлены уникальный индекс на **пользователя**на имя входа.</span><span class="sxs-lookup"><span data-stu-id="cf3e2-235">The following example introduces a unique index on a **User**'s login name.</span></span>

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

### <a name="multiple-column-indexes"></a><span data-ttu-id="cf3e2-236">Индексы с несколькими столбцами</span><span class="sxs-lookup"><span data-stu-id="cf3e2-236">Multiple-Column Indexes</span></span>

<span data-ttu-id="cf3e2-237">Индексы, которые охватывают несколько столбцов, указываются с помощью тем же именем в несколько заметок индекса для данной таблицы.</span><span class="sxs-lookup"><span data-stu-id="cf3e2-237">Indexes that span multiple columns are specified by using the same name in multiple Index annotations for a given table.</span></span> <span data-ttu-id="cf3e2-238">Создавая индексы с несколькими столбцами, необходимо указать порядок для столбцов в индексе.</span><span class="sxs-lookup"><span data-stu-id="cf3e2-238">When you create multi-column indexes, you need to specify an order for the columns in the index.</span></span> <span data-ttu-id="cf3e2-239">Например, следующий код создает многостолбцовый индекс для **Оценка** и **BlogId** вызывается **IX\_BlogAndRating**.</span><span class="sxs-lookup"><span data-stu-id="cf3e2-239">For example, the following code creates a multi-column index on **Rating** and **BlogId** called **IX\_BlogAndRating**.</span></span> <span data-ttu-id="cf3e2-240">**BlogId** является первым столбцом в индексе и **Оценка** является вторым.</span><span class="sxs-lookup"><span data-stu-id="cf3e2-240">**BlogId** is the first column in the index and **Rating** is the second.</span></span>

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

 

## <a name="relationship-attributes-inverseproperty-and-foreignkey"></a><span data-ttu-id="cf3e2-241">Связь атрибутов: InverseProperty и ForeignKey</span><span class="sxs-lookup"><span data-stu-id="cf3e2-241">Relationship Attributes: InverseProperty and ForeignKey</span></span>

> [!NOTE]
> <span data-ttu-id="cf3e2-242">Эта страница содержит сведения о настройке связи в модели Code First с помощью заметок к данным.</span><span class="sxs-lookup"><span data-stu-id="cf3e2-242">This page provides information about setting up relationships in your Code First model using Data Annotations.</span></span> <span data-ttu-id="cf3e2-243">Общие сведения о связях в EF и способах доступа к данных и управления ими с помощью связей см. в разделе [связей и свойств навигации](~/ef6/fundamentals/relationships.md). \*</span><span class="sxs-lookup"><span data-stu-id="cf3e2-243">For general information about relationships in EF and how to access and manipulate data using relationships, see [Relationships & Navigation Properties](~/ef6/fundamentals/relationships.md).\*</span></span>

<span data-ttu-id="cf3e2-244">Соглашение об первый кода позаботится о наиболее распространенных связи в модели, но существуют случаи, где он нуждается в помощи.</span><span class="sxs-lookup"><span data-stu-id="cf3e2-244">Code first convention will take care of the most common relationships in your model, but there are some cases where it needs help.</span></span>

<span data-ttu-id="cf3e2-245">Изменение имени ключевого свойства в класс блог, созданный на проблему с его связь с Post.</span><span class="sxs-lookup"><span data-stu-id="cf3e2-245">Changing the name of the key property in the Blog class created a problem with its relationship to Post.</span></span> 

<span data-ttu-id="cf3e2-246">При создании базы данных, код сначала увидит BlogId свойства в классе Post и распознает ее, по соглашению, что он соответствует имени класса, а также «Id», как внешний ключ к классу блога.</span><span class="sxs-lookup"><span data-stu-id="cf3e2-246">When generating the database, code first sees the BlogId property in the Post class and recognizes it, by the convention that it matches a class name plus “Id”, as a foreign key to the Blog class.</span></span> <span data-ttu-id="cf3e2-247">Но нет BlogId свойства в классе блога.</span><span class="sxs-lookup"><span data-stu-id="cf3e2-247">But there is no BlogId property in the blog class.</span></span> <span data-ttu-id="cf3e2-248">Решением будет создать свойство навигации в блога и использовать внешний DataAnnotation для кода сначала понять, как создать связь между двумя классами, используя свойство Post.BlogId — а также способы указания ограничений в База данных.</span><span class="sxs-lookup"><span data-stu-id="cf3e2-248">The solution for this is to create a navigation property in the Post and use the Foreign DataAnnotation to help code first understand how to build the relationship between the two classes —using the Post.BlogId property — as well as how to specify constraints in the database.</span></span>

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

<span data-ttu-id="cf3e2-249">Ограничение в базе данных отображает связь между InternalBlogs.PrimaryTrackingKey и Posts.BlogId.</span><span class="sxs-lookup"><span data-stu-id="cf3e2-249">The constraint in the database shows a relationship between InternalBlogs.PrimaryTrackingKey and Posts.BlogId.</span></span> 

![отношение между InternalBlogs.PrimaryTrackingKey и Posts.BlogId](~/ef6/media/jj591583-figure09.png)

<span data-ttu-id="cf3e2-251">InverseProperty используется в том случае, если у вас есть несколько связей между классами.</span><span class="sxs-lookup"><span data-stu-id="cf3e2-251">The InverseProperty is used when you have multiple relationships between classes.</span></span>

<span data-ttu-id="cf3e2-252">В классе Post, может потребоваться хранить список кто написал в блоге и кто его изменить.</span><span class="sxs-lookup"><span data-stu-id="cf3e2-252">In the Post class, you may want to keep track of who wrote a blog post as well as who edited it.</span></span> <span data-ttu-id="cf3e2-253">Ниже приведены два новых свойства навигации для класса Post.</span><span class="sxs-lookup"><span data-stu-id="cf3e2-253">Here are two new navigation properties for the Post class.</span></span>

``` csharp
    public Person CreatedBy { get; set; }
    public Person UpdatedBy { get; set; }
```

<span data-ttu-id="cf3e2-254">Также необходимо добавить в класс Person, ссылается на эти свойства.</span><span class="sxs-lookup"><span data-stu-id="cf3e2-254">You’ll also need to add in the Person class referenced by these properties.</span></span> <span data-ttu-id="cf3e2-255">Класс Person имеет свойства навигации обратно к записи, одну для всех записей, написанной person и одну для всех записей, обновлены этим пользователем.</span><span class="sxs-lookup"><span data-stu-id="cf3e2-255">The Person class has navigation properties back to the Post, one for all of the posts written by the person and one for all of the posts updated by that person.</span></span>

``` csharp
    public class Person
    {
            public int Id { get; set; }
            public string Name { get; set; }
            public List<Post> PostsWritten { get; set; }
            public List<Post> PostsUpdated { get; set; }
    }
```

<span data-ttu-id="cf3e2-256">Сначала код не сможет сопоставлять свойства в класс сам по себе.</span><span class="sxs-lookup"><span data-stu-id="cf3e2-256">Code first is not able to match up the properties in the two classes on its own.</span></span> <span data-ttu-id="cf3e2-257">В таблице базы данных для записей должны иметь один внешний ключ для пользователя, Кем создано и один для UpdatedBy человека, но код сначала создать свойства внешнего ключа четыре будет: Person\_идентификатор, Person\_Id1, Кем создано\_идентификатор и UpdatedBy\_идентификатор.</span><span class="sxs-lookup"><span data-stu-id="cf3e2-257">The database table for Posts should have one foreign key for the CreatedBy person and one for the UpdatedBy person but code first will create four will foreign key properties: Person\_Id, Person\_Id1, CreatedBy\_Id and UpdatedBy\_Id.</span></span>

![Таблице Posts с очень внешние ключи](~/ef6/media/jj591583-figure10.png)

<span data-ttu-id="cf3e2-259">Чтобы устранить эти проблемы, InverseProperty заметки можно использовать для указания выравнивания свойств.</span><span class="sxs-lookup"><span data-stu-id="cf3e2-259">To fix these problems, you can use the InverseProperty annotation to specify the alignment of the properties.</span></span>

``` csharp
    [InverseProperty("CreatedBy")]
    public List<Post> PostsWritten { get; set; }

    [InverseProperty("UpdatedBy")]
    public List<Post> PostsUpdated { get; set; }
```

<span data-ttu-id="cf3e2-260">Так как свойство PostsWritten лично знает, что это относится к типу Post, сборка будет связи Post.CreatedBy.</span><span class="sxs-lookup"><span data-stu-id="cf3e2-260">Because the PostsWritten property in Person knows that this refers to the Post type, it will build the relationship to Post.CreatedBy.</span></span> <span data-ttu-id="cf3e2-261">Аналогичным образом PostsUpdated будут подключены к Post.UpdatedBy.</span><span class="sxs-lookup"><span data-stu-id="cf3e2-261">Similarly, PostsUpdated will be connected to Post.UpdatedBy.</span></span> <span data-ttu-id="cf3e2-262">И код сначала не создает дополнительных внешних ключей.</span><span class="sxs-lookup"><span data-stu-id="cf3e2-262">And code first will not create the extra foreign keys.</span></span>

![Таблице Posts без дополнительных внешних ключей](~/ef6/media/jj591583-figure11.png)

 

## <a name="summary"></a><span data-ttu-id="cf3e2-264">Сводка</span><span class="sxs-lookup"><span data-stu-id="cf3e2-264">Summary</span></span>

<span data-ttu-id="cf3e2-265">DataAnnotations не только позволяют описывают проверки на стороне клиента и сервера в классах первый код, но они также позволяют улучшать и даже устранить предположения, которые код сначала облегчат о классов на основе его соглашений.</span><span class="sxs-lookup"><span data-stu-id="cf3e2-265">DataAnnotations not only let you describe client and server side validation in your code first classes, but they also allow you to enhance and even correct the assumptions that code first will make about your classes based on its conventions.</span></span> <span data-ttu-id="cf3e2-266">С помощью DataAnnotations вы можете не только обеспечивать создание схемы базы данных, но также можно сопоставить классы первый код и уже существующей базы данных.</span><span class="sxs-lookup"><span data-stu-id="cf3e2-266">With DataAnnotations you can not only drive database schema generation, but you can also map your code first classes to a pre-existing database.</span></span>

<span data-ttu-id="cf3e2-267">Хотя они являются достаточно гибкими, помните, что DataAnnotations предоставляют только наиболее часто используемые конфигурации изменения, внесенные в классах первого кода.</span><span class="sxs-lookup"><span data-stu-id="cf3e2-267">While they are very flexible, keep in mind that DataAnnotations provide only the most commonly needed configuration changes you can make on your code first classes.</span></span> <span data-ttu-id="cf3e2-268">Чтобы настроить свои классы для некоторых крайних случаев, следует проверить механизм альтернативная конфигурация, Code First Fluent API.</span><span class="sxs-lookup"><span data-stu-id="cf3e2-268">To configure your classes for some of the edge cases, you should look to the alternate configuration mechanism, Code First’s Fluent API .</span></span>
