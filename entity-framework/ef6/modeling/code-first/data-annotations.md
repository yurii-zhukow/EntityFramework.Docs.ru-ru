---
title: Заметки к данным Code First — EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 80abefbd-23c9-4fce-9cd3-520e5df9856e
ms.openlocfilehash: 17370b83aee9974161b1771e8a1dc99c2de27f0f
ms.sourcegitcommit: 6f7af3f138bf7c724cbdda261f97e5cf7035e8d7
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/25/2020
ms.locfileid: "88847622"
---
# <a name="code-first-data-annotations"></a><span data-ttu-id="ded6d-102">Заметки к данным Code First</span><span class="sxs-lookup"><span data-stu-id="ded6d-102">Code First Data Annotations</span></span>
> [!NOTE]
> <span data-ttu-id="ded6d-103">**EF 4.1 только в сторону** — функции, API и т. д., обсуждаемые на этой странице, появились в Entity Framework 4,1.</span><span class="sxs-lookup"><span data-stu-id="ded6d-103">**EF4.1 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 4.1.</span></span> <span data-ttu-id="ded6d-104">Если вы используете более раннюю версию, некоторые или все эти сведения не применяются.</span><span class="sxs-lookup"><span data-stu-id="ded6d-104">If you are using an earlier version, some or all of this information does not apply.</span></span>

<span data-ttu-id="ded6d-105">Содержимое этой страницы адаптируется из статьи, первоначально написанной Джулия Лерман ( \<http://thedatafarm.com> ).</span><span class="sxs-lookup"><span data-stu-id="ded6d-105">The content on this page is adapted from an article originally written by Julie Lerman (\<http://thedatafarm.com>).</span></span>

<span data-ttu-id="ded6d-106">Entity Framework Code First позволяет использовать собственные доменные классы для представления модели, которую использует EF для выполнения запросов, отслеживания изменений и обновления функций.</span><span class="sxs-lookup"><span data-stu-id="ded6d-106">Entity Framework Code First allows you to use your own domain classes to represent the model that EF relies on to perform querying, change tracking, and updating functions.</span></span> <span data-ttu-id="ded6d-107">Code First использует шаблон программирования, называемый "соглашение по конфигурации".</span><span class="sxs-lookup"><span data-stu-id="ded6d-107">Code First leverages a programming pattern referred to as 'convention over configuration.'</span></span> <span data-ttu-id="ded6d-108">Code First предполагает, что классы соответствуют соглашениям Entity Framework, и в этом случае автоматически выполнит действия по выполнению своей работы.</span><span class="sxs-lookup"><span data-stu-id="ded6d-108">Code First will assume that your classes follow the conventions of Entity Framework, and in that case, will automatically work out how to perform its job.</span></span> <span data-ttu-id="ded6d-109">Однако если ваши классы не соответствуют этим соглашениям, вы можете добавлять конфигурации в классы для предоставления EF с информацией о необходимых параметрах.</span><span class="sxs-lookup"><span data-stu-id="ded6d-109">However, if your classes do not follow those conventions, you have the ability to add configurations to your classes to provide EF with the requisite information.</span></span>

<span data-ttu-id="ded6d-110">Code First предоставляет два способа добавления этих конфигураций в классы.</span><span class="sxs-lookup"><span data-stu-id="ded6d-110">Code First gives you two ways to add these configurations to your classes.</span></span> <span data-ttu-id="ded6d-111">В одном из них используются простые атрибуты, называемые заметками, а во второй — API-интерфейс Fluent Code First, который предоставляет способ описания конфигураций императивно в коде.</span><span class="sxs-lookup"><span data-stu-id="ded6d-111">One is using simple attributes called DataAnnotations, and the second is using Code First’s Fluent API, which provides you with a way to describe configurations imperatively, in code.</span></span>

<span data-ttu-id="ded6d-112">В этой статье основное внимание уделяется использованию аннотаций (в пространстве имен System. ComponentModel. заметок к данным) для настройки классов: выделение наиболее часто используемых конфигураций.</span><span class="sxs-lookup"><span data-stu-id="ded6d-112">This article will focus on using DataAnnotations (in the System.ComponentModel.DataAnnotations namespace) to configure your classes – highlighting the most commonly needed configurations.</span></span> <span data-ttu-id="ded6d-113">Аннотации данных также понятны в ряде приложений .NET, таких как ASP.NET MVC, что позволяет этим приложениям использовать те же заметки для проверок на стороне клиента.</span><span class="sxs-lookup"><span data-stu-id="ded6d-113">DataAnnotations are also understood by a number of .NET applications, such as ASP.NET MVC which allows these applications to leverage the same annotations for client-side validations.</span></span>


## <a name="the-model"></a><span data-ttu-id="ded6d-114">Модель</span><span class="sxs-lookup"><span data-stu-id="ded6d-114">The model</span></span>

<span data-ttu-id="ded6d-115">Я продемонстрирую Code First аннотации с помощью простой пары классов: blog и POST.</span><span class="sxs-lookup"><span data-stu-id="ded6d-115">I’ll demonstrate Code First DataAnnotations with a simple pair of classes: Blog and Post.</span></span>

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

<span data-ttu-id="ded6d-116">Как и в случае, классы блога и POST посвящены первому соглашению о коде и не нуждаются в корректировках для включения совместимости с EF.</span><span class="sxs-lookup"><span data-stu-id="ded6d-116">As they are, the Blog and Post classes conveniently follow code first convention and require no tweaks to enable EF compatability.</span></span> <span data-ttu-id="ded6d-117">Однако заметки также можно использовать для предоставления в EF дополнительных сведений о классах и базе данных, с которой они сопоставляются.</span><span class="sxs-lookup"><span data-stu-id="ded6d-117">However, you can also use the annotations to provide more information to EF about the classes and the database to which they map.</span></span>

 

## <a name="key"></a><span data-ttu-id="ded6d-118">Ключ</span><span class="sxs-lookup"><span data-stu-id="ded6d-118">Key</span></span>

<span data-ttu-id="ded6d-119">Entity Framework полагается на каждую сущность, имеющую значение ключа, используемое для отслеживания сущностей.</span><span class="sxs-lookup"><span data-stu-id="ded6d-119">Entity Framework relies on every entity having a key value that is used for entity tracking.</span></span> <span data-ttu-id="ded6d-120">Одно из соглашений Code First является неявными свойствами ключа; Code First будет искать свойство с именем "ID" или сочетание имени класса и "ID", например "Блогид".</span><span class="sxs-lookup"><span data-stu-id="ded6d-120">One convention of Code First is implicit key properties; Code First will look for a property named “Id”, or a combination of class name and “Id”, such as “BlogId”.</span></span> <span data-ttu-id="ded6d-121">Это свойство будет сопоставляться с первичным ключевым столбцом в базе данных.</span><span class="sxs-lookup"><span data-stu-id="ded6d-121">This property will map to a primary key column in the database.</span></span>

<span data-ttu-id="ded6d-122">Классы blog и POST соответствуют этому соглашению.</span><span class="sxs-lookup"><span data-stu-id="ded6d-122">The Blog and Post classes both follow this convention.</span></span> <span data-ttu-id="ded6d-123">Что делать, если это не так?</span><span class="sxs-lookup"><span data-stu-id="ded6d-123">What if they didn’t?</span></span> <span data-ttu-id="ded6d-124">Что если в блоге использовалось имя *примаритраккингкэй* вместо или даже *foo*?</span><span class="sxs-lookup"><span data-stu-id="ded6d-124">What if Blog used the name *PrimaryTrackingKey* instead, or even *foo*?</span></span> <span data-ttu-id="ded6d-125">Если код сначала не находит свойство, которое соответствует этому соглашению, это вызовет исключение из-за того, что Entity Framework требуется ключевое свойство.</span><span class="sxs-lookup"><span data-stu-id="ded6d-125">If code first does not find a property that matches this convention it will throw an exception because of Entity Framework’s requirement that you must have a key property.</span></span> <span data-ttu-id="ded6d-126">Можно использовать ключевую аннотацию, чтобы указать, какое свойство будет использоваться в качестве EntityKey.</span><span class="sxs-lookup"><span data-stu-id="ded6d-126">You can use the key annotation to specify which property is to be used as the EntityKey.</span></span>

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

<span data-ttu-id="ded6d-127">Если вы используете функцию создания базы данных в первую очередь, таблица блога будет иметь первичный ключевой столбец с именем Примаритраккингкэй, который также определен как Identity по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="ded6d-127">If you are using code first’s database generation feature, the Blog table will have a primary key column named PrimaryTrackingKey, which is also defined as Identity by default.</span></span>

![Таблица блога с первичным ключом](~/ef6/media/jj591583-figure01.png)

### <a name="composite-keys"></a><span data-ttu-id="ded6d-129">Составные ключи</span><span class="sxs-lookup"><span data-stu-id="ded6d-129">Composite keys</span></span>

<span data-ttu-id="ded6d-130">Entity Framework поддерживает составные ключи — Первичные ключи, состоящие более чем из одного свойства.</span><span class="sxs-lookup"><span data-stu-id="ded6d-130">Entity Framework supports composite keys - primary keys that consist of more than one property.</span></span> <span data-ttu-id="ded6d-131">Например, у вас может быть класс Passport, первичный ключ которого является сочетанием Пасспортнумбер и Иссуингкаунтри.</span><span class="sxs-lookup"><span data-stu-id="ded6d-131">For example, you could have a Passport class whose primary key is a combination of PassportNumber and IssuingCountry.</span></span>

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

<span data-ttu-id="ded6d-132">Попытка использовать приведенный выше класс в модели EF приведет к `InvalidOperationException` следующим результатам:</span><span class="sxs-lookup"><span data-stu-id="ded6d-132">Attempting to use the above class in your EF model would result in an `InvalidOperationException`:</span></span>

<span data-ttu-id="ded6d-133">*Не удалось определить порядок составного первичного ключа для типа "паспорт". Используйте метод Колумнаттрибуте или HasKey, чтобы указать порядок для составных первичных ключей.*</span><span class="sxs-lookup"><span data-stu-id="ded6d-133">*Unable to determine composite primary key ordering for type 'Passport'. Use the ColumnAttribute or the HasKey method to specify an order for composite primary keys.*</span></span>

<span data-ttu-id="ded6d-134">Чтобы использовать составные ключи, Entity Framework необходимо определить порядок для ключевых свойств.</span><span class="sxs-lookup"><span data-stu-id="ded6d-134">In order to use composite keys, Entity Framework requires you to define an order for the key properties.</span></span> <span data-ttu-id="ded6d-135">Это можно сделать, используя заметку столбца для указания порядка.</span><span class="sxs-lookup"><span data-stu-id="ded6d-135">You can do this by using the Column annotation to specify an order.</span></span>

>[!NOTE]
> <span data-ttu-id="ded6d-136">Значение порядка является относительным (а не основанным на индексе), чтобы можно было использовать любые значения.</span><span class="sxs-lookup"><span data-stu-id="ded6d-136">The order value is relative (rather than index based) so any values can be used.</span></span> <span data-ttu-id="ded6d-137">Например, 100 и 200 будут приемлемыми вместо 1 и 2.</span><span class="sxs-lookup"><span data-stu-id="ded6d-137">For example, 100 and 200 would be acceptable in place of 1 and 2.</span></span>

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

<span data-ttu-id="ded6d-138">При наличии сущностей с составными внешними ключами необходимо указать тот же порядок столбцов, который использовался для соответствующих свойств первичного ключа.</span><span class="sxs-lookup"><span data-stu-id="ded6d-138">If you have entities with composite foreign keys, then you must specify the same column ordering that you used for the corresponding primary key properties.</span></span>

<span data-ttu-id="ded6d-139">Только относительный порядок в свойствах внешнего ключа должен быть одинаковым, точные значения, назначенные **заказу** , не должны совпадать.</span><span class="sxs-lookup"><span data-stu-id="ded6d-139">Only the relative ordering within the foreign key properties needs to be the same, the exact values assigned to **Order** do not need to match.</span></span> <span data-ttu-id="ded6d-140">Например, в следующем классе можно использовать 3 и 4 вместо 1 и 2.</span><span class="sxs-lookup"><span data-stu-id="ded6d-140">For example, in the following class, 3 and 4 could be used in place of 1 and 2.</span></span>

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

## <a name="required"></a><span data-ttu-id="ded6d-141">Обязательно</span><span class="sxs-lookup"><span data-stu-id="ded6d-141">Required</span></span>

<span data-ttu-id="ded6d-142">`Required`Заметка сообщает EF, что требуется определенное свойство.</span><span class="sxs-lookup"><span data-stu-id="ded6d-142">The `Required` annotation tells EF that a particular property is required.</span></span>

<span data-ttu-id="ded6d-143">Добавление, обязательное к свойству Title, заставит EF (и MVC) убедиться, что свойство содержит данные.</span><span class="sxs-lookup"><span data-stu-id="ded6d-143">Adding Required to the Title property will force EF (and MVC) to ensure that the property has data in it.</span></span>

``` csharp
    [Required]
    public string Title { get; set; }
```

<span data-ttu-id="ded6d-144">При отсутствии в приложении дополнительного кода или изменений разметки приложение MVC выполняет проверку на стороне клиента, даже динамически создавая сообщение, используя имена свойств и заметок.</span><span class="sxs-lookup"><span data-stu-id="ded6d-144">With no additional code or markup changes in the application, an MVC application will perform client side validation, even dynamically building a message using the property and annotation names.</span></span>

![Ошибка при создании страницы с заголовком](~/ef6/media/jj591583-figure02.png)

<span data-ttu-id="ded6d-146">Обязательный атрибут также влияет на созданную базу данных, делая сопоставленное свойство не допускающим значение null.</span><span class="sxs-lookup"><span data-stu-id="ded6d-146">The Required attribute will also affect the generated database by making the mapped property non-nullable.</span></span> <span data-ttu-id="ded6d-147">Обратите внимание, что поле заголовка изменено на «NOT NULL».</span><span class="sxs-lookup"><span data-stu-id="ded6d-147">Notice that the Title field has changed to “not null”.</span></span>

>[!NOTE]
> <span data-ttu-id="ded6d-148">В некоторых случаях для столбца в базе данных может оказаться невозможным значение null, даже если свойство является обязательным.</span><span class="sxs-lookup"><span data-stu-id="ded6d-148">In some cases it may not be possible for the column in the database to be non-nullable even though the property is required.</span></span> <span data-ttu-id="ded6d-149">Например, при использовании ИЕРАРХИческих данных стратегии наследования для нескольких типов хранится в одной таблице.</span><span class="sxs-lookup"><span data-stu-id="ded6d-149">For example, when using a TPH inheritance strategy data for multiple types is stored in a single table.</span></span> <span data-ttu-id="ded6d-150">Если производный тип содержит обязательное свойство, столбец не может быть сделан пустым, так как не все типы в иерархии будут иметь это свойство.</span><span class="sxs-lookup"><span data-stu-id="ded6d-150">If a derived type includes a required property the column cannot be made non-nullable since not all types in the hierarchy will have this property.</span></span>

 

![Таблица блогов](~/ef6/media/jj591583-figure03.png)

 

## <a name="maxlength-and-minlength"></a><span data-ttu-id="ded6d-152">MaxLength и MinLength</span><span class="sxs-lookup"><span data-stu-id="ded6d-152">MaxLength and MinLength</span></span>

<span data-ttu-id="ded6d-153">`MaxLength`Атрибуты и `MinLength` позволяют указать дополнительные проверки свойств так же, как и в случае с `Required` .</span><span class="sxs-lookup"><span data-stu-id="ded6d-153">The `MaxLength` and `MinLength` attributes allow you to specify additional property validations, just as you did with `Required`.</span></span>

<span data-ttu-id="ded6d-154">Ниже приведены Блогжернаме требования к длине.</span><span class="sxs-lookup"><span data-stu-id="ded6d-154">Here is the BloggerName with length requirements.</span></span> <span data-ttu-id="ded6d-155">В примере также показано, как комбинировать атрибуты.</span><span class="sxs-lookup"><span data-stu-id="ded6d-155">The example also demonstrates how to combine attributes.</span></span>

``` csharp
    [MaxLength(10),MinLength(5)]
    public string BloggerName { get; set; }
```

<span data-ttu-id="ded6d-156">Аннотация MaxLength повлияет на базу данных, устанавливая для свойства длину значение 10.</span><span class="sxs-lookup"><span data-stu-id="ded6d-156">The MaxLength annotation will impact the database by setting the property’s length to 10.</span></span>

![Таблица блогов, в которой отображается максимальная длина столбца Блогжернаме](~/ef6/media/jj591583-figure04.png)

<span data-ttu-id="ded6d-158">В аннотации на стороне клиента MVC и на стороне сервера EF 4,1 будет выполняться проверка, а также динамическое создание сообщения об ошибке: "поле Блогжернаме должно быть строкой или типом массива с максимальной длиной" 10 ". Это сообщение имеет небольшой формат.</span><span class="sxs-lookup"><span data-stu-id="ded6d-158">MVC client-side annotation and EF 4.1 server-side annotation will both honor this validation, again dynamically building an error message: “The field BloggerName must be a string or array type with a maximum length of '10'.” That message is a little long.</span></span> <span data-ttu-id="ded6d-159">Многие аннотации позволяют указать сообщение об ошибке с атрибутом ErrorMessage.</span><span class="sxs-lookup"><span data-stu-id="ded6d-159">Many annotations let you specify an error message with the ErrorMessage attribute.</span></span>

``` csharp
    [MaxLength(10, ErrorMessage="BloggerName must be 10 characters or less"),MinLength(5)]
    public string BloggerName { get; set; }
```

<span data-ttu-id="ded6d-160">Можно также указать ErrorMessage в требуемой аннотации.</span><span class="sxs-lookup"><span data-stu-id="ded6d-160">You can also specify ErrorMessage in the Required annotation.</span></span>

![Страница создания с настраиваемым сообщением об ошибке](~/ef6/media/jj591583-figure05.png)

 

## <a name="notmapped"></a><span data-ttu-id="ded6d-162">нотмаппед</span><span class="sxs-lookup"><span data-stu-id="ded6d-162">NotMapped</span></span>

<span data-ttu-id="ded6d-163">Первое соглашение Code определяет, что каждое свойство, имеющее поддерживаемый тип данных, представлено в базе данных.</span><span class="sxs-lookup"><span data-stu-id="ded6d-163">Code first convention dictates that every property that is of a supported data type is represented in the database.</span></span> <span data-ttu-id="ded6d-164">Но это не всегда так в приложениях.</span><span class="sxs-lookup"><span data-stu-id="ded6d-164">But this isn’t always the case in your applications.</span></span> <span data-ttu-id="ded6d-165">Например, в классе блога может быть свойство, которое создает код на основе полей Title и Блогжернаме.</span><span class="sxs-lookup"><span data-stu-id="ded6d-165">For example you might have a property in the Blog class that creates a code based on the Title and BloggerName fields.</span></span> <span data-ttu-id="ded6d-166">Это свойство может быть создано динамически и не должно сохраняться.</span><span class="sxs-lookup"><span data-stu-id="ded6d-166">That property can be created dynamically and does not need to be stored.</span></span> <span data-ttu-id="ded6d-167">Можно пометить все свойства, которые не сопоставляются с базой данных, с аннотацией Нотмаппед, например с этим свойством Блогкоде.</span><span class="sxs-lookup"><span data-stu-id="ded6d-167">You can mark any properties that do not map to the database with the NotMapped annotation such as this BlogCode property.</span></span>

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

 

## <a name="complextype"></a><span data-ttu-id="ded6d-168">ComplexType</span><span class="sxs-lookup"><span data-stu-id="ded6d-168">ComplexType</span></span>

<span data-ttu-id="ded6d-169">Нередко описать сущности предметной области в наборе классов, а затем послойировать эти классы для описания готовой сущности.</span><span class="sxs-lookup"><span data-stu-id="ded6d-169">It’s not uncommon to describe your domain entities across a set of classes and then layer those classes to describe a complete entity.</span></span> <span data-ttu-id="ded6d-170">Например, в модель можно добавить класс с именем Блогдетаилс.</span><span class="sxs-lookup"><span data-stu-id="ded6d-170">For example, you may add a class called BlogDetails to your model.</span></span>

``` csharp
    public class BlogDetails
    {
        public DateTime? DateCreated { get; set; }

        [MaxLength(250)]
        public string Description { get; set; }
    }
```

<span data-ttu-id="ded6d-171">Обратите внимание, что не `BlogDetails` имеет какого либо типа ключевого свойства.</span><span class="sxs-lookup"><span data-stu-id="ded6d-171">Notice that `BlogDetails` does not have any type of key property.</span></span> <span data-ttu-id="ded6d-172">При проектировании на основе домена `BlogDetails` называется объектом значения.</span><span class="sxs-lookup"><span data-stu-id="ded6d-172">In domain driven design, `BlogDetails` is referred to as a value object.</span></span> <span data-ttu-id="ded6d-173">Entity Framework относится к объектам значений как к сложным типам.</span><span class="sxs-lookup"><span data-stu-id="ded6d-173">Entity Framework refers to value objects as complex types.</span></span><span data-ttu-id="ded6d-174">Сложные типы не могут быть записаны самостоятельно.</span><span class="sxs-lookup"><span data-stu-id="ded6d-174">  Complex types cannot be tracked on their own.</span></span>

<span data-ttu-id="ded6d-175">Однако, как свойство в `Blog` классе, `BlogDetails` будет относиться к объекту в виде части `Blog` объекта.</span><span class="sxs-lookup"><span data-stu-id="ded6d-175">However as a property in the `Blog` class, `BlogDetails` will be tracked as part of a `Blog` object.</span></span> <span data-ttu-id="ded6d-176">Чтобы код сначала распознал это, необходимо пометить `BlogDetails` класс как `ComplexType` .</span><span class="sxs-lookup"><span data-stu-id="ded6d-176">In order for code first to recognize this, you must mark the `BlogDetails` class as a `ComplexType`.</span></span>

``` csharp
    [ComplexType]
    public class BlogDetails
    {
        public DateTime? DateCreated { get; set; }

        [MaxLength(250)]
        public string Description { get; set; }
    }
```

<span data-ttu-id="ded6d-177">Теперь можно добавить свойство в `Blog` класс для представления `BlogDetails` для этого блога.</span><span class="sxs-lookup"><span data-stu-id="ded6d-177">Now you can add a property in the `Blog` class to represent the `BlogDetails` for that blog.</span></span>

``` csharp
        public BlogDetails BlogDetail { get; set; }
```

<span data-ttu-id="ded6d-178">В базе данных `Blog` таблица будет содержать все свойства блога, включая свойства, содержащиеся в его `BlogDetail` свойстве.</span><span class="sxs-lookup"><span data-stu-id="ded6d-178">In the database, the `Blog` table will contain all of the properties of the blog including the properties contained in its `BlogDetail` property.</span></span> <span data-ttu-id="ded6d-179">По умолчанию каждому из них предшествуют имя сложного типа «Блогдетаил».</span><span class="sxs-lookup"><span data-stu-id="ded6d-179">By default, each one is preceded with the name of the complex type, "BlogDetail".</span></span>

![Таблица блога со сложным типом](~/ef6/media/jj591583-figure06.png)


## <a name="concurrencycheck"></a><span data-ttu-id="ded6d-181">ConcurrencyCheck</span><span class="sxs-lookup"><span data-stu-id="ded6d-181">ConcurrencyCheck</span></span>

<span data-ttu-id="ded6d-182">`ConcurrencyCheck`Заметка позволяет пометить одно или несколько свойств, которые будут использоваться для проверки параллелизма в базе данных, когда пользователь редактирует или удаляет сущность.</span><span class="sxs-lookup"><span data-stu-id="ded6d-182">The `ConcurrencyCheck` annotation allows you to flag one or more properties to be used for concurrency checking in the database when a user edits or deletes an entity.</span></span> <span data-ttu-id="ded6d-183">Если вы работали с конструктором EF, то при этом выполняется сопоставление с установкой свойства в значение `ConcurrencyMode` `Fixed` .</span><span class="sxs-lookup"><span data-stu-id="ded6d-183">If you've been working with the EF Designer, this aligns with setting a property's `ConcurrencyMode` to `Fixed`.</span></span>

<span data-ttu-id="ded6d-184">Давайте посмотрим, как `ConcurrencyCheck` работает, добавив его в `BloggerName` свойство.</span><span class="sxs-lookup"><span data-stu-id="ded6d-184">Let’s see how `ConcurrencyCheck` works by adding it to the `BloggerName` property.</span></span>

``` csharp
    [ConcurrencyCheck, MaxLength(10, ErrorMessage="BloggerName must be 10 characters or less"),MinLength(5)]
    public string BloggerName { get; set; }
```

<span data-ttu-id="ded6d-185">При `SaveChanges` вызове метода из-за `ConcurrencyCheck` аннотации `BloggerName` поля исходное значение этого свойства будет использоваться в обновлении.</span><span class="sxs-lookup"><span data-stu-id="ded6d-185">When `SaveChanges` is called, because of the `ConcurrencyCheck` annotation on the `BloggerName` field, the original value of that property will be used in the update.</span></span> <span data-ttu-id="ded6d-186">Команда попытается выбрать правильную строку, отфильтровывая не только значение ключа, но и исходное значение `BloggerName` .</span><span class="sxs-lookup"><span data-stu-id="ded6d-186">The command will attempt to locate the correct row by filtering not only on the key value but also on the original value of `BloggerName`.</span></span><span data-ttu-id="ded6d-187">Ниже приведены критические части команды UPDATE, отправленной в базу данных, на которой можно увидеть, что команда обновит строку, имеющую значение, `PrimaryTrackingKey` равную 1, и значение `BloggerName` «Юлия», которое было исходным значением при извлечении этого блога из базы данных.</span><span class="sxs-lookup"><span data-stu-id="ded6d-187">  Here are the critical parts of the UPDATE command sent to the database, where you can see the command will update the row that has a `PrimaryTrackingKey` is 1 and a `BloggerName` of “Julie” which was the original value when that blog was retrieved from the database.</span></span>

``` SQL
    where (([PrimaryTrackingKey] = @4) and ([BloggerName] = @5))
    @4=1,@5=N'Julie'
```

<span data-ttu-id="ded6d-188">Если кто-то изменил имя блоггера для этого блога, это обновление завершится ошибкой, и вы получите **дбупдатеконкурренциексцептион** , который вам потребуется справиться.</span><span class="sxs-lookup"><span data-stu-id="ded6d-188">If someone has changed the blogger name for that blog in the meantime, this update will fail and you’ll get a **DbUpdateConcurrencyException** that you'll need to handle.</span></span>

 

## <a name="timestamp"></a><span data-ttu-id="ded6d-189">TimeStamp</span><span class="sxs-lookup"><span data-stu-id="ded6d-189">TimeStamp</span></span>

<span data-ttu-id="ded6d-190">Чаще всего для проверки параллелизма используются поля rowversion или timestamp.</span><span class="sxs-lookup"><span data-stu-id="ded6d-190">It's more common to use rowversion or timestamp fields for concurrency checking.</span></span> <span data-ttu-id="ded6d-191">Но вместо использования `ConcurrencyCheck` аннотации можно использовать более конкретную `TimeStamp` аннотацию, если тип свойства — байтовый массив.</span><span class="sxs-lookup"><span data-stu-id="ded6d-191">But rather than using the `ConcurrencyCheck` annotation, you can use the more specific `TimeStamp` annotation as long as the type of the property is byte array.</span></span> <span data-ttu-id="ded6d-192">Code First будет рассматривать `Timestamp` свойства так же `ConcurrencyCheck` , как свойства, но также гарантирует, что поле базы данных, которое создает код, не допускает значения NULL.</span><span class="sxs-lookup"><span data-stu-id="ded6d-192">Code first will treat `Timestamp` properties the same as `ConcurrencyCheck` properties, but it will also ensure that the database field that code first generates is non-nullable.</span></span> <span data-ttu-id="ded6d-193">В данном классе может быть только одно свойство timestamp.</span><span class="sxs-lookup"><span data-stu-id="ded6d-193">You can only have one timestamp property in a given class.</span></span>

<span data-ttu-id="ded6d-194">Добавление следующего свойства в класс Blog:</span><span class="sxs-lookup"><span data-stu-id="ded6d-194">Adding the following property to the Blog class:</span></span>

``` csharp
    [Timestamp]
    public Byte[] TimeStamp { get; set; }
```

<span data-ttu-id="ded6d-195">в коде сначала создается столбец метки времени, не допускающий значения NULL, в таблице базы данных.</span><span class="sxs-lookup"><span data-stu-id="ded6d-195">results in code first creating a non-nullable timestamp column in the database table.</span></span>

![Таблица блогов со столбцом временной метки](~/ef6/media/jj591583-figure07.png)

 

## <a name="table-and-column"></a><span data-ttu-id="ded6d-197">Таблица и столбец</span><span class="sxs-lookup"><span data-stu-id="ded6d-197">Table and Column</span></span>

<span data-ttu-id="ded6d-198">Если вы разрешаете Code First создать базу данных, может потребоваться изменить имя создаваемых таблиц и столбцов.</span><span class="sxs-lookup"><span data-stu-id="ded6d-198">If you are letting Code First create the database, you may want to change the name of the tables and columns it is creating.</span></span> <span data-ttu-id="ded6d-199">Можно также использовать Code First с существующей базой данных.</span><span class="sxs-lookup"><span data-stu-id="ded6d-199">You can also use Code First with an existing database.</span></span> <span data-ttu-id="ded6d-200">Но в этом случае имена классов и свойств в домене не всегда совпадают с именами таблиц и столбцов в базе данных.</span><span class="sxs-lookup"><span data-stu-id="ded6d-200">But it's not always the case that the names of the classes and properties in your domain match the names of the tables and columns in your database.</span></span>

<span data-ttu-id="ded6d-201">Мой класс имеет имя `Blog` и по соглашению, код First предполагает, что это будет сопоставляться с таблицей с именем `Blogs` .</span><span class="sxs-lookup"><span data-stu-id="ded6d-201">My class is named `Blog` and by convention, code first presumes this will map to a table named `Blogs`.</span></span> <span data-ttu-id="ded6d-202">Если это не так, можно указать имя таблицы с `Table` атрибутом.</span><span class="sxs-lookup"><span data-stu-id="ded6d-202">If that's not the case you can specify the name of the table with the `Table` attribute.</span></span> <span data-ttu-id="ded6d-203">Например, заметка указывает, что имя таблицы — **интерналблогс**.</span><span class="sxs-lookup"><span data-stu-id="ded6d-203">Here for example, the annotation is specifying that the table name is **InternalBlogs**.</span></span>

``` csharp
    [Table("InternalBlogs")]
    public class Blog
```

<span data-ttu-id="ded6d-204">`Column`Заметка является более умелыми при указании атрибутов сопоставленного столбца.</span><span class="sxs-lookup"><span data-stu-id="ded6d-204">The `Column` annotation is a more adept in specifying the attributes of a mapped column.</span></span> <span data-ttu-id="ded6d-205">Можно указать имя, тип данных или даже порядок, в котором столбец отображается в таблице.</span><span class="sxs-lookup"><span data-stu-id="ded6d-205">You can stipulate a name, data type or even the order in which a column appears in the table.</span></span> <span data-ttu-id="ded6d-206">Ниже приведен пример `Column` атрибута.</span><span class="sxs-lookup"><span data-stu-id="ded6d-206">Here is an example of the `Column` attribute.</span></span>

``` csharp
    [Column("BlogDescription", TypeName="ntext")]
    public String Description {get;set;}
```

<span data-ttu-id="ded6d-207">Не путайте атрибут столбца `TypeName` с аннотацией DataType.</span><span class="sxs-lookup"><span data-stu-id="ded6d-207">Don’t confuse Column’s `TypeName` attribute with the DataType DataAnnotation.</span></span> <span data-ttu-id="ded6d-208">DataType — это заметка, используемая в пользовательском интерфейсе и игнорируемая Code First.</span><span class="sxs-lookup"><span data-stu-id="ded6d-208">DataType is an annotation used for the UI and is ignored by Code First.</span></span>

<span data-ttu-id="ded6d-209">Ниже приведена таблица, созданная после повторного создания.</span><span class="sxs-lookup"><span data-stu-id="ded6d-209">Here is the table after it’s been regenerated.</span></span> <span data-ttu-id="ded6d-210">Имя таблицы изменилось на **интерналблогс** , а `Description` столбец из сложного типа теперь имеет значение `BlogDescription` .</span><span class="sxs-lookup"><span data-stu-id="ded6d-210">The table name has changed to **InternalBlogs** and `Description` column from the complex type is now `BlogDescription`.</span></span> <span data-ttu-id="ded6d-211">Поскольку имя было указано в аннотации, код First не будет использовать соглашение о начале имени столбца с именем сложного типа.</span><span class="sxs-lookup"><span data-stu-id="ded6d-211">Because the name was specified in the annotation, code first will not use the convention of starting the column name with the name of the complex type.</span></span>

![Таблица и столбец блогов переименованы](~/ef6/media/jj591583-figure08.png)

 

## <a name="databasegenerated"></a><span data-ttu-id="ded6d-213">датабасеженератед</span><span class="sxs-lookup"><span data-stu-id="ded6d-213">DatabaseGenerated</span></span>

<span data-ttu-id="ded6d-214">Важным компонентом базы данных является возможность иметь вычисленные свойства.</span><span class="sxs-lookup"><span data-stu-id="ded6d-214">An important database features is the ability to have computed properties.</span></span> <span data-ttu-id="ded6d-215">При сопоставлении классов Code First с таблицами, содержащими вычисленные столбцы, не нужно Entity Framework пытаться обновить эти столбцы.</span><span class="sxs-lookup"><span data-stu-id="ded6d-215">If you're mapping your Code First classes to tables that contain computed columns, you don't want Entity Framework to try to update those columns.</span></span> <span data-ttu-id="ded6d-216">Однако необходимо, чтобы EF возвращал эти значения из базы данных после вставки или обновления данных.</span><span class="sxs-lookup"><span data-stu-id="ded6d-216">But you do want EF to return those values from the database after you've inserted or updated data.</span></span> <span data-ttu-id="ded6d-217">Заметку можно использовать `DatabaseGenerated` для пометки этих свойств в классе вместе с `Computed` перечислением.</span><span class="sxs-lookup"><span data-stu-id="ded6d-217">You can use the `DatabaseGenerated` annotation to flag those properties in your class along with the `Computed` enum.</span></span> <span data-ttu-id="ded6d-218">Другие перечисления — `None` и `Identity` .</span><span class="sxs-lookup"><span data-stu-id="ded6d-218">Other enums are `None` and `Identity`.</span></span>

``` csharp
    [DatabaseGenerated(DatabaseGeneratedOption.Computed)]
    public DateTime DateCreated { get; set; }
```

<span data-ttu-id="ded6d-219">Базу данных, созданную для столбцов типа Byte или timestamp, можно использовать при первом создании базы данных с помощью Code. в противном случае ее следует использовать только при указании существующих баз данных, так как код сначала не сможет определить формулу для вычисляемого столбца.</span><span class="sxs-lookup"><span data-stu-id="ded6d-219">You can use database generated on byte or timestamp columns when code first is generating the database, otherwise you should only use this when pointing to existing databases because code first won't be able to determine the formula for the computed column.</span></span>

<span data-ttu-id="ded6d-220">По умолчанию, ключевое свойство, которое является целым числом, станет ключом удостоверения в базе данных.</span><span class="sxs-lookup"><span data-stu-id="ded6d-220">You read above that by default, a key property that is an integer will become an identity key in the database.</span></span> <span data-ttu-id="ded6d-221">Это будет то же самое, что и параметр `DatabaseGenerated` `DatabaseGeneratedOption.Identity` .</span><span class="sxs-lookup"><span data-stu-id="ded6d-221">That would be the same as setting `DatabaseGenerated` to `DatabaseGeneratedOption.Identity`.</span></span> <span data-ttu-id="ded6d-222">Если вы не хотите, чтобы он был ключом удостоверения, можно задать значение `DatabaseGeneratedOption.None` .</span><span class="sxs-lookup"><span data-stu-id="ded6d-222">If you do not want it to be an identity key, you can set the value to `DatabaseGeneratedOption.None`.</span></span>

 

## <a name="index"></a><span data-ttu-id="ded6d-223">Индекс</span><span class="sxs-lookup"><span data-stu-id="ded6d-223">Index</span></span>

> [!NOTE]
> <span data-ttu-id="ded6d-224">**EF 6.1 только в сторону** — `Index` атрибут был введен в Entity Framework 6,1.</span><span class="sxs-lookup"><span data-stu-id="ded6d-224">**EF6.1 Onwards Only** - The `Index` attribute was introduced in Entity Framework 6.1.</span></span> <span data-ttu-id="ded6d-225">Если вы используете более раннюю версию, сведения в этом разделе не применяются.</span><span class="sxs-lookup"><span data-stu-id="ded6d-225">If you are using an earlier version the information in this section does not apply.</span></span>

<span data-ttu-id="ded6d-226">Можно создать индекс для одного или нескольких столбцов с помощью **индексаттрибуте**.</span><span class="sxs-lookup"><span data-stu-id="ded6d-226">You can create an index on one or more columns using the **IndexAttribute**.</span></span> <span data-ttu-id="ded6d-227">Добавление атрибута в одно или несколько свойств приведет к тому, что EF создаст соответствующий индекс в базе данных при создании базы данных, или сформировать шаблон для соответствующих вызовов **CreateIndex** , если используется Code First migrations.</span><span class="sxs-lookup"><span data-stu-id="ded6d-227">Adding the attribute to one or more properties will cause EF to create the corresponding index in the database when it creates the database, or scaffold the corresponding **CreateIndex** calls if you are using Code First Migrations.</span></span>

<span data-ttu-id="ded6d-228">Например, следующий код приведет к созданию индекса для `Rating` столбца `Posts` таблицы в базе данных.</span><span class="sxs-lookup"><span data-stu-id="ded6d-228">For example, the following code will result in an index being created on the `Rating` column of the `Posts` table in the database.</span></span>

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

<span data-ttu-id="ded6d-229">По умолчанию индекс будет называться \*\* \_ &lt; имя &gt; Свойства IX\*\* (рейтинг IX \_ в приведенном выше примере).</span><span class="sxs-lookup"><span data-stu-id="ded6d-229">By default, the index will be named **IX\_&lt;property name&gt;** (IX\_Rating in the above example).</span></span> <span data-ttu-id="ded6d-230">Однако можно также указать имя для индекса.</span><span class="sxs-lookup"><span data-stu-id="ded6d-230">You can also specify a name for the index though.</span></span> <span data-ttu-id="ded6d-231">В следующем примере указывается, что индекс должен иметь имя `PostRatingIndex` .</span><span class="sxs-lookup"><span data-stu-id="ded6d-231">The following example specifies that the index should be named `PostRatingIndex`.</span></span>

``` csharp
    [Index("PostRatingIndex")]
    public int Rating { get; set; }
```

<span data-ttu-id="ded6d-232">По умолчанию индексы не являются уникальными, но можно использовать `IsUnique` именованный параметр, чтобы указать, что индекс должен быть уникальным.</span><span class="sxs-lookup"><span data-stu-id="ded6d-232">By default, indexes are non-unique, but you can use the `IsUnique` named parameter to specify that an index should be unique.</span></span> <span data-ttu-id="ded6d-233">В следующем примере представлен уникальный индекс для `User` имени входа.</span><span class="sxs-lookup"><span data-stu-id="ded6d-233">The following example introduces a unique index on a `User`'s login name.</span></span>

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

### <a name="multiple-column-indexes"></a><span data-ttu-id="ded6d-234">Индексы с несколькими столбцами</span><span class="sxs-lookup"><span data-stu-id="ded6d-234">Multiple-Column Indexes</span></span>

<span data-ttu-id="ded6d-235">Индексы, охватывающие несколько столбцов, задаются с одним и тем же именем в нескольких аннотациях индекса для данной таблицы.</span><span class="sxs-lookup"><span data-stu-id="ded6d-235">Indexes that span multiple columns are specified by using the same name in multiple Index annotations for a given table.</span></span> <span data-ttu-id="ded6d-236">При создании индексов с несколькими столбцами необходимо указать порядок столбцов в индексе.</span><span class="sxs-lookup"><span data-stu-id="ded6d-236">When you create multi-column indexes, you need to specify an order for the columns in the index.</span></span> <span data-ttu-id="ded6d-237">Например, следующий код создает индекс с несколькими столбцами для `Rating` и, `BlogId` именуемый **IX \_ блогидандратинг**.</span><span class="sxs-lookup"><span data-stu-id="ded6d-237">For example, the following code creates a multi-column index on `Rating` and `BlogId` called **IX\_BlogIdAndRating**.</span></span> <span data-ttu-id="ded6d-238">`BlogId` — Это первый столбец в индексе, а `Rating` — второй.</span><span class="sxs-lookup"><span data-stu-id="ded6d-238">`BlogId` is the first column in the index and `Rating` is the second.</span></span>

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

 

## <a name="relationship-attributes-inverseproperty-and-foreignkey"></a><span data-ttu-id="ded6d-239">Атрибуты отношений: Инверсепроперти и Фореигнкэй</span><span class="sxs-lookup"><span data-stu-id="ded6d-239">Relationship Attributes: InverseProperty and ForeignKey</span></span>

> [!NOTE]
> <span data-ttu-id="ded6d-240">На этой странице содержатся сведения о настройке связей в модели Code First с помощью заметок к данным.</span><span class="sxs-lookup"><span data-stu-id="ded6d-240">This page provides information about setting up relationships in your Code First model using Data Annotations.</span></span> <span data-ttu-id="ded6d-241">Общие сведения о связях в EF и доступе к данным и управлении ими с помощью связей см. в разделе [связи & свойства навигации](~/ef6/fundamentals/relationships.md). \*</span><span class="sxs-lookup"><span data-stu-id="ded6d-241">For general information about relationships in EF and how to access and manipulate data using relationships, see [Relationships & Navigation Properties](~/ef6/fundamentals/relationships.md).\*</span></span>

<span data-ttu-id="ded6d-242">Первое соглашение Code позаботится о наиболее распространенных отношениях в модели, но в некоторых случаях требуется помощь.</span><span class="sxs-lookup"><span data-stu-id="ded6d-242">Code first convention will take care of the most common relationships in your model, but there are some cases where it needs help.</span></span>

<span data-ttu-id="ded6d-243">При изменении имени ключевого свойства в `Blog` классе возникла проблема со связью `Post` .</span><span class="sxs-lookup"><span data-stu-id="ded6d-243">Changing the name of the key property in the `Blog` class created a problem with its relationship to `Post`.</span></span> 

<span data-ttu-id="ded6d-244">При формировании базы данных код сначала видит `BlogId` свойство в классе Post и распознает его, согласно соглашению, что оно соответствует имени класса плюс **идентификатор**, в качестве внешнего ключа для `Blog` класса.</span><span class="sxs-lookup"><span data-stu-id="ded6d-244">When generating the database, code first sees the `BlogId` property in the Post class and recognizes it, by the convention that it matches a class name plus **Id**, as a foreign key to the `Blog` class.</span></span> <span data-ttu-id="ded6d-245">Но `BlogId` в классе блога нет свойства.</span><span class="sxs-lookup"><span data-stu-id="ded6d-245">But there is no `BlogId` property in the blog class.</span></span> <span data-ttu-id="ded6d-246">Решением этой проблемы является создание свойства навигации в `Post` и использование `ForeignKey` аннотации данных для того, чтобы понять, как создать связь между двумя классами (с помощью `Post.BlogId` Свойства), а также как задать ограничения в базе.</span><span class="sxs-lookup"><span data-stu-id="ded6d-246">The solution for this is to create a navigation property in the `Post` and use the `ForeignKey` DataAnnotation to help code first understand how to build the relationship between the two classes (using the `Post.BlogId` property) as well as how to specify constraints in the database.</span></span>

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

<span data-ttu-id="ded6d-247">Ограничение в базе данных показывает связь между `InternalBlogs.PrimaryTrackingKey` и `Posts.BlogId` .</span><span class="sxs-lookup"><span data-stu-id="ded6d-247">The constraint in the database shows a relationship between `InternalBlogs.PrimaryTrackingKey` and `Posts.BlogId`.</span></span> 

![отношение между Интерналблогс. Примаритраккингкэй и posts. Блогид](~/ef6/media/jj591583-figure09.png)

<span data-ttu-id="ded6d-249">`InverseProperty`Используется при наличии нескольких связей между классами.</span><span class="sxs-lookup"><span data-stu-id="ded6d-249">The `InverseProperty` is used when you have multiple relationships between classes.</span></span>

<span data-ttu-id="ded6d-250">В `Post` классе может быть необходимо отследить, кто написал запись блога, а также кто ее редактировал.</span><span class="sxs-lookup"><span data-stu-id="ded6d-250">In the `Post` class, you may want to keep track of who wrote a blog post as well as who edited it.</span></span> <span data-ttu-id="ded6d-251">Ниже приведены два новых свойства навигации для класса Post.</span><span class="sxs-lookup"><span data-stu-id="ded6d-251">Here are two new navigation properties for the Post class.</span></span>

``` csharp
    public Person CreatedBy { get; set; }
    public Person UpdatedBy { get; set; }
```

<span data-ttu-id="ded6d-252">Также необходимо добавить в `Person` класс, на который ссылаются эти свойства.</span><span class="sxs-lookup"><span data-stu-id="ded6d-252">You’ll also need to add in the `Person` class referenced by these properties.</span></span> <span data-ttu-id="ded6d-253">`Person`Класс имеет свойства навигации обратно `Post` , один для всех записей, записанных человеком, а другой — для всех записей, обновленных этим человеком.</span><span class="sxs-lookup"><span data-stu-id="ded6d-253">The `Person` class has navigation properties back to the `Post`, one for all of the posts written by the person and one for all of the posts updated by that person.</span></span>

``` csharp
    public class Person
    {
            public int Id { get; set; }
            public string Name { get; set; }
            public List<Post> PostsWritten { get; set; }
            public List<Post> PostsUpdated { get; set; }
    }
```

<span data-ttu-id="ded6d-254">Код сначала не способен сопоставлять свойства в двух классах самостоятельно.</span><span class="sxs-lookup"><span data-stu-id="ded6d-254">Code first is not able to match up the properties in the two classes on its own.</span></span> <span data-ttu-id="ded6d-255">Таблица базы данных для `Posts` пользователя должна иметь один внешний ключ `CreatedBy` и один для `UpdatedBy` пользователя, но сначала код создаст четыре свойства внешнего ключа: \*\* \_ идентификатор пользователя**, **пользователь \_ id1**, \*\* \_ идентификатор CreatedBy** и \*\* \_ идентификатор упдатедби\*\*.</span><span class="sxs-lookup"><span data-stu-id="ded6d-255">The database table for `Posts` should have one foreign key for the `CreatedBy` person and one for the `UpdatedBy` person but code first will create four foreign key properties: **Person\_Id**, **Person\_Id1**, **CreatedBy\_Id** and **UpdatedBy\_Id**.</span></span>

![Отправляет таблицу с дополнительными внешними ключами](~/ef6/media/jj591583-figure10.png)

<span data-ttu-id="ded6d-257">Чтобы устранить эти проблемы, можно использовать `InverseProperty` заметку для указания выравнивания свойств.</span><span class="sxs-lookup"><span data-stu-id="ded6d-257">To fix these problems, you can use the `InverseProperty` annotation to specify the alignment of the properties.</span></span>

``` csharp
    [InverseProperty("CreatedBy")]
    public List<Post> PostsWritten { get; set; }

    [InverseProperty("UpdatedBy")]
    public List<Post> PostsUpdated { get; set; }
```

<span data-ttu-id="ded6d-258">Так как `PostsWritten` это свойство знает, что это относится к `Post` типу, он создает связь с `Post.CreatedBy` .</span><span class="sxs-lookup"><span data-stu-id="ded6d-258">Because the `PostsWritten` property in Person knows that this refers to the `Post` type, it will build the relationship to `Post.CreatedBy`.</span></span> <span data-ttu-id="ded6d-259">Аналогично, `PostsUpdated` будет подключен к `Post.UpdatedBy` .</span><span class="sxs-lookup"><span data-stu-id="ded6d-259">Similarly, `PostsUpdated` will be connected to `Post.UpdatedBy`.</span></span> <span data-ttu-id="ded6d-260">И Code First не будет создавать дополнительные внешние ключи.</span><span class="sxs-lookup"><span data-stu-id="ded6d-260">And code first will not create the extra foreign keys.</span></span>

![Таблица posts без дополнительных внешних ключей](~/ef6/media/jj591583-figure11.png)

 

## <a name="summary"></a><span data-ttu-id="ded6d-262">Итоги</span><span class="sxs-lookup"><span data-stu-id="ded6d-262">Summary</span></span>

<span data-ttu-id="ded6d-263">Аннотации данных не только позволяют описать клиентскую и серверную проверку в первых классах кода, но они также позволяют улучшать и даже исправлять предположения о том, что код сначала будет делаться с вашими классами в соответствии с его соглашениями.</span><span class="sxs-lookup"><span data-stu-id="ded6d-263">DataAnnotations not only let you describe client and server side validation in your code first classes, but they also allow you to enhance and even correct the assumptions that code first will make about your classes based on its conventions.</span></span> <span data-ttu-id="ded6d-264">С помощью аннотаций к данным можно не только создать схему базы данных, но и связать сначала классы Code с уже существующей базой данных.</span><span class="sxs-lookup"><span data-stu-id="ded6d-264">With DataAnnotations you can not only drive database schema generation, but you can also map your code first classes to a pre-existing database.</span></span>

<span data-ttu-id="ded6d-265">Хотя они являются очень гибкими, помните, что аннотации в виде примечаний предоставляют только наиболее распространенные изменения конфигурации, которые можно внести в первые классы кода.</span><span class="sxs-lookup"><span data-stu-id="ded6d-265">While they are very flexible, keep in mind that DataAnnotations provide only the most commonly needed configuration changes you can make on your code first classes.</span></span> <span data-ttu-id="ded6d-266">Чтобы настроить классы для некоторых из пограничных вариантов, следует обратить внимание на альтернативный механизм настройки, API-интерфейс Code First Fluent.</span><span class="sxs-lookup"><span data-stu-id="ded6d-266">To configure your classes for some of the edge cases, you should look to the alternate configuration mechanism, Code First’s Fluent API .</span></span>
