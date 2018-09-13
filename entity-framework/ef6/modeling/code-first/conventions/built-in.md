---
title: Соглашения Code First - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: bc644573-c2b2-4ed7-8745-3c37c41058ad
ms.openlocfilehash: 4d03a32db5d84eb37c22617a95005b272172a65d
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/13/2018
ms.locfileid: "45491004"
---
# <a name="code-first-conventions"></a><span data-ttu-id="88f44-102">Первый соглашения о коде</span><span class="sxs-lookup"><span data-stu-id="88f44-102">Code First Conventions</span></span>
<span data-ttu-id="88f44-103">Во-первых, код позволяет описания модели с помощью классов C# или Visual Basic .NET.</span><span class="sxs-lookup"><span data-stu-id="88f44-103">Code First enables you to describe a model by using C# or Visual Basic .NET classes.</span></span> <span data-ttu-id="88f44-104">Основные фигуры модели определяется с использованием соглашений.</span><span class="sxs-lookup"><span data-stu-id="88f44-104">The basic shape of the model is detected by using conventions.</span></span> <span data-ttu-id="88f44-105">Соглашения являются наборы правил, которые используются для автоматической настройки концептуальной модели на основе определений класса при работе в режиме Code First.</span><span class="sxs-lookup"><span data-stu-id="88f44-105">Conventions are sets of rules that are used to automatically configure a conceptual model based on class definitions when working with Code First.</span></span> <span data-ttu-id="88f44-106">Правила определяются в пространстве имен System.Data.Entity.ModelConfiguration.Conventions.</span><span class="sxs-lookup"><span data-stu-id="88f44-106">The conventions are defined in the System.Data.Entity.ModelConfiguration.Conventions namespace.</span></span>  

<span data-ttu-id="88f44-107">Можно продолжить настройку модели с помощью заметок к данным или текучего API.</span><span class="sxs-lookup"><span data-stu-id="88f44-107">You can further configure your model by using data annotations or the fluent API.</span></span> <span data-ttu-id="88f44-108">Преимущество отдается конфигурацию с помощью текучего API, следуют заметок к данным и соглашения.</span><span class="sxs-lookup"><span data-stu-id="88f44-108">Precedence is given to configuration through the fluent API followed by data annotations and then conventions.</span></span> <span data-ttu-id="88f44-109">Дополнительные сведения см. в разделе [заметок к данным](~/ef6/modeling/code-first/data-annotations.md), [Fluent API - связи](~/ef6/modeling/code-first/fluent/relationships.md), [Fluent API - типов и свойств,](~/ef6/modeling/code-first/fluent/types-and-properties.md) и [Fluent API с использованием VB.NET](~/ef6/modeling/code-first/fluent/vb.md).</span><span class="sxs-lookup"><span data-stu-id="88f44-109">For more information see [Data Annotations](~/ef6/modeling/code-first/data-annotations.md), [Fluent API - Relationships](~/ef6/modeling/code-first/fluent/relationships.md), [Fluent API - Types & Properties](~/ef6/modeling/code-first/fluent/types-and-properties.md) and [Fluent API with VB.NET](~/ef6/modeling/code-first/fluent/vb.md).</span></span>  

<span data-ttu-id="88f44-110">Подробный список соглашения Code First доступен в [документации по API](https://msdn.microsoft.com/library/system.data.entity.modelconfiguration.conventions.aspx).</span><span class="sxs-lookup"><span data-stu-id="88f44-110">A detailed list of Code First conventions is available in the [API Documentation](https://msdn.microsoft.com/library/system.data.entity.modelconfiguration.conventions.aspx).</span></span> <span data-ttu-id="88f44-111">Здесь представлен обзор соглашения, используемые механизмом Code First.</span><span class="sxs-lookup"><span data-stu-id="88f44-111">This topic provides an overview of the conventions used by Code First.</span></span>  

## <a name="type-discovery"></a><span data-ttu-id="88f44-112">Обнаружение типа</span><span class="sxs-lookup"><span data-stu-id="88f44-112">Type Discovery</span></span>  

<span data-ttu-id="88f44-113">При использовании шаблона разработки Code First обычно начинается с написания классов .NET Framework, которые определяют модель концептуальный (домен).</span><span class="sxs-lookup"><span data-stu-id="88f44-113">When using Code First development you usually begin by writing .NET Framework classes that define your conceptual (domain) model.</span></span> <span data-ttu-id="88f44-114">Помимо определения классов, необходимо также разрешить **DbContext** знать, какие типы, которые вы хотите включить в модель.</span><span class="sxs-lookup"><span data-stu-id="88f44-114">In addition to defining the classes, you also need to let **DbContext** know which types you want to include in the model.</span></span> <span data-ttu-id="88f44-115">Чтобы сделать это, вы определяется класс контекста, производный от **DbContext** и предоставляет **DbSet** свойств для типов, которые должны быть частью модели.</span><span class="sxs-lookup"><span data-stu-id="88f44-115">To do this, you define a context class that derives from **DbContext** and exposes **DbSet** properties for the types that you want to be part of the model.</span></span> <span data-ttu-id="88f44-116">Код сначала будет включать эти типы и также будет получать в любой ссылочных типов, даже если ссылочных типов определяются в другой сборке.</span><span class="sxs-lookup"><span data-stu-id="88f44-116">Code First will include these types and also will pull in any referenced types, even if the referenced types are defined in a different assembly.</span></span>  

<span data-ttu-id="88f44-117">Если типы участвуют в иерархии наследования, его достаточно, чтобы определить **DbSet** свойство для базового класса и производных типов, будут автоматически включены, если они находятся в той же сборке, в качестве базового класса.</span><span class="sxs-lookup"><span data-stu-id="88f44-117">If your types participate in an inheritance hierarchy, it is enough to define a **DbSet** property for the base class, and the derived types will be automatically included, if they are in the same assembly as the base class.</span></span>  

<span data-ttu-id="88f44-118">В следующем примере имеется только один **DbSet** свойство, определенное для **SchoolEntities** класс (**отделов**).</span><span class="sxs-lookup"><span data-stu-id="88f44-118">In the following example, there is only one **DbSet** property defined on the **SchoolEntities** class (**Departments**).</span></span> <span data-ttu-id="88f44-119">Код сначала использует это свойство для обнаружения и по запросу в любой ссылочных типов.</span><span class="sxs-lookup"><span data-stu-id="88f44-119">Code First uses this property to discover and pull in any referenced types.</span></span>  

``` csharp
public class SchoolEntities : DbContext
{
    public DbSet<Department> Departments { get; set; }
}

public class Department
{
    // Primary key
    public int DepartmentID { get; set; }
    public string Name { get; set; }

    // Navigation property
    public virtual ICollection<Course> Courses { get; set; }
}

public class Course
{
    // Primary key
    public int CourseID { get; set; }

    public string Title { get; set; }
    public int Credits { get; set; }

    // Foreign key
    public int DepartmentID { get; set; }

    // Navigation properties
    public virtual Department Department { get; set; }
}

public partial class OnlineCourse : Course
{
    public string URL { get; set; }
}

public partial class OnsiteCourse : Course
{
    public string Location { get; set; }
    public string Days { get; set; }
    public System.DateTime Time { get; set; }
}
```  

<span data-ttu-id="88f44-120">Если вы хотите исключить тип из модели, используйте **NotMapped** атрибут или **DbModelBuilder.Ignore** текучего API.</span><span class="sxs-lookup"><span data-stu-id="88f44-120">If you want to exclude a type from the model, use the **NotMapped** attribute or the **DbModelBuilder.Ignore** fluent API.</span></span>  

```  csharp
modelBuilder.Ignore<Department>();
```  

## <a name="primary-key-convention"></a><span data-ttu-id="88f44-121">Соглашение первичного ключа</span><span class="sxs-lookup"><span data-stu-id="88f44-121">Primary Key Convention</span></span>  

<span data-ttu-id="88f44-122">Код сначала определяет, что свойство является первичным ключом, если свойство класса, называется «ID» (без учета регистра), или имя класса, добавив «ID».</span><span class="sxs-lookup"><span data-stu-id="88f44-122">Code First infers that a property is a primary key if a property on a class is named “ID” (not case sensitive), or the class name followed by "ID".</span></span> <span data-ttu-id="88f44-123">Если тип со свойством первичного ключа является числом или GUID будет настроен в качестве столбца идентификаторов.</span><span class="sxs-lookup"><span data-stu-id="88f44-123">If the type of the primary key property is numeric or GUID it will be configured as an identity column.</span></span>  

``` csharp
public class Department
{
    // Primary key
    public int DepartmentID { get; set; }

    . . .  

}
```  

## <a name="relationship-convention"></a><span data-ttu-id="88f44-124">Соглашение о связи</span><span class="sxs-lookup"><span data-stu-id="88f44-124">Relationship Convention</span></span>  

<span data-ttu-id="88f44-125">В Entity Framework свойства навигации позволяют для перехода по связи между двумя типами сущностей.</span><span class="sxs-lookup"><span data-stu-id="88f44-125">In Entity Framework, navigation properties provide a way to navigate a relationship between two entity types.</span></span> <span data-ttu-id="88f44-126">Каждый объект может обладать свойством навигации для каждого отношения, в котором участвует.</span><span class="sxs-lookup"><span data-stu-id="88f44-126">Every object can have a navigation property for every relationship in which it participates.</span></span> <span data-ttu-id="88f44-127">Свойства навигации позволяют перейти связей и управление ими в обоих направлениях, возвращая объект ссылки (если атрибут кратности имеет один или ноль или один) или коллекции (Если кратность равна много).</span><span class="sxs-lookup"><span data-stu-id="88f44-127">Navigation properties allow you to navigate and manage relationships in both directions, returning either a reference object (if the multiplicity is either one or zero-or-one) or a collection (if the multiplicity is many).</span></span> <span data-ttu-id="88f44-128">Код сначала определяет связи, основанные на свойства навигации, определенные для типов.</span><span class="sxs-lookup"><span data-stu-id="88f44-128">Code First infers relationships based on the navigation properties defined on your types.</span></span>  

<span data-ttu-id="88f44-129">В дополнение к свойствам навигации рекомендуется включить свойства внешнего ключа на типы, представляющие зависимые объекты.</span><span class="sxs-lookup"><span data-stu-id="88f44-129">In addition to navigation properties, we recommend that you include foreign key properties on the types that represent dependent objects.</span></span> <span data-ttu-id="88f44-130">Любое свойство с тем же типом данных, как основной свойство первичного ключа и с именем, которое следует одной из следующих форматов представляет внешний ключ для связи: "\<имя свойства навигации\>\<участника Имя свойство первичного ключа\>','\<имя основного класса\>\<имя свойство первичного ключа\>", или"\<имя участника-свойство первичного ключа\>".</span><span class="sxs-lookup"><span data-stu-id="88f44-130">Any property with the same data type as the principal primary key property and with a name that follows one of the following formats represents a foreign key for the relationship: '\<navigation property name\>\<principal primary key property name\>', '\<principal class name\>\<primary key property name\>', or '\<principal primary key property name\>'.</span></span> <span data-ttu-id="88f44-131">Если найдено несколько совпадений, преимущество отдается в порядке, указанном выше.</span><span class="sxs-lookup"><span data-stu-id="88f44-131">If multiple matches are found then precedence is given in the order listed above.</span></span> <span data-ttu-id="88f44-132">Обнаружение внешнего ключа не учитывается регистр символов.</span><span class="sxs-lookup"><span data-stu-id="88f44-132">Foreign key detection is not case sensitive.</span></span> <span data-ttu-id="88f44-133">При обнаружении свойство внешнего ключа, Code First определяет кратность связи, в зависимости от того, поддерживает ли внешний ключ.</span><span class="sxs-lookup"><span data-stu-id="88f44-133">When a foreign key property is detected, Code First infers the multiplicity of the relationship based on the nullability of the foreign key.</span></span> <span data-ttu-id="88f44-134">Если свойство имеет значение NULL затем связь регистрируется как необязательный; в противном случае — регистрируется связи при необходимости.</span><span class="sxs-lookup"><span data-stu-id="88f44-134">If the property is nullable then the relationship is registered as optional; otherwise the relationship is registered as required.</span></span>  

<span data-ttu-id="88f44-135">Если внешний ключ для зависимой сущности не допускает значения NULL, то Code First устанавливает каскадное удаление в отношении.</span><span class="sxs-lookup"><span data-stu-id="88f44-135">If a foreign key on the dependent entity is not nullable, then Code First sets cascade delete on the relationship.</span></span> <span data-ttu-id="88f44-136">Если внешний ключ для зависимой сущности допускает значения NULL, Code First не устанавливает каскадное удаление для связи, и при удалении участника внешний ключ устанавливается в значение null.</span><span class="sxs-lookup"><span data-stu-id="88f44-136">If a foreign key on the dependent entity is nullable, Code First does not set cascade delete on the relationship, and when the principal is deleted the foreign key will be set to null.</span></span> <span data-ttu-id="88f44-137">Поведение, обнаруживаемые при удалении кратность и cascade соглашение можно переопределить с помощью текучего API.</span><span class="sxs-lookup"><span data-stu-id="88f44-137">The multiplicity and cascade delete behavior detected by convention can be overridden by using the fluent API.</span></span>  

<span data-ttu-id="88f44-138">В следующем примере свойства навигации и внешний ключ используются для определения связи между классами отдела и курса.</span><span class="sxs-lookup"><span data-stu-id="88f44-138">In the following example the navigation properties and a foreign key are used to define the relationship between the Department and Course classes.</span></span>  

``` csharp
public class Department
{
    // Primary key
    public int DepartmentID { get; set; }
    public string Name { get; set; }

    // Navigation property
    public virtual ICollection<Course> Courses { get; set; }
}

public class Course
{
    // Primary key
    public int CourseID { get; set; }

    public string Title { get; set; }
    public int Credits { get; set; }

    // Foreign key
    public int DepartmentID { get; set; }

    // Navigation properties
    public virtual Department Department { get; set; }
}
```  

> [!NOTE]
> <span data-ttu-id="88f44-139">Если у вас есть несколько связей между теми же типами (предположим, например, можно определить **Person** и **книги** классы, где **Person** класс содержит  **ReviewedBooks** и **AuthoredBooks** свойства навигации и **книги** класс содержит **автор** и  **Редактор** свойства навигации) необходимо вручную настроить отношения с помощью заметок к данным или текучего API.</span><span class="sxs-lookup"><span data-stu-id="88f44-139">If you have multiple relationships between the same types (for example, suppose you define the **Person** and **Book** classes, where the **Person** class contains the **ReviewedBooks** and **AuthoredBooks** navigation properties and the **Book** class contains the **Author** and **Reviewer** navigation properties) you need to manually configure the relationships by using Data Annotations or the fluent API.</span></span> <span data-ttu-id="88f44-140">Дополнительные сведения см. в разделе [заметок к данным - связи](~/ef6/modeling/code-first/data-annotations.md) и [Fluent API - связи](~/ef6/modeling/code-first/fluent/relationships.md).</span><span class="sxs-lookup"><span data-stu-id="88f44-140">For more information, see [Data Annotations - Relationships](~/ef6/modeling/code-first/data-annotations.md) and [Fluent API - Relationships](~/ef6/modeling/code-first/fluent/relationships.md).</span></span>  

## <a name="complex-types-convention"></a><span data-ttu-id="88f44-141">Соглашение о сложных типов</span><span class="sxs-lookup"><span data-stu-id="88f44-141">Complex Types Convention</span></span>  

<span data-ttu-id="88f44-142">Когда Code First обнаруживает определение класса, где не может быть определен первичный ключ, а первичный ключ не зарегистрирован при помощи заметки к данным или fluent API, тип автоматически регистрируется как сложный тип.</span><span class="sxs-lookup"><span data-stu-id="88f44-142">When Code First discovers a class definition where a primary key cannot be inferred, and no primary key is registered through data annotations or the fluent API, then the type is automatically registered as a complex type.</span></span> <span data-ttu-id="88f44-143">Определение сложных типов также требует, что тип не имеет свойства, которые ссылаются на типы сущностей и нет ссылок из свойства коллекции другого типа.</span><span class="sxs-lookup"><span data-stu-id="88f44-143">Complex type detection also requires that the type does not have properties that reference entity types and is not referenced from a collection property on another type.</span></span> <span data-ttu-id="88f44-144">Учитывая следующие определения класса Code First бы получен, **сведения** — это сложный тип, так как он не имеет первичного ключа.</span><span class="sxs-lookup"><span data-stu-id="88f44-144">Given the following class definitions Code First would infer that **Details** is a complex type because it has no primary key.</span></span>  

``` csharp
public partial class OnsiteCourse : Course
{
    public OnsiteCourse()
    {
        Details = new Details();
    }

    public Details Details { get; set; }
}

public class Details
{
    public System.DateTime Time { get; set; }
    public string Location { get; set; }
    public string Days { get; set; }
}
```  

## <a name="connection-string-convention"></a><span data-ttu-id="88f44-145">Соглашение о строке подключения</span><span class="sxs-lookup"><span data-stu-id="88f44-145">Connection String Convention</span></span>  

<span data-ttu-id="88f44-146">Дополнительные сведения о соглашениях об что DbContext использует для обнаружения соединение для использования см. в разделе [подключений и модели](~/ef6/fundamentals/configuring/connection-strings.md).</span><span class="sxs-lookup"><span data-stu-id="88f44-146">To learn about the conventions that DbContext uses to discover the connection to use see [Connections and Models](~/ef6/fundamentals/configuring/connection-strings.md).</span></span>  

## <a name="removing-conventions"></a><span data-ttu-id="88f44-147">Удаление соглашения</span><span class="sxs-lookup"><span data-stu-id="88f44-147">Removing Conventions</span></span>  

<span data-ttu-id="88f44-148">Вы можете удалить правилам, определенным в пространстве имен System.Data.Entity.ModelConfiguration.Conventions.</span><span class="sxs-lookup"><span data-stu-id="88f44-148">You can remove any of the conventions defined in the System.Data.Entity.ModelConfiguration.Conventions namespace.</span></span> <span data-ttu-id="88f44-149">В следующем примере удаляется **PluralizingTableNameConvention**.</span><span class="sxs-lookup"><span data-stu-id="88f44-149">The following example removes **PluralizingTableNameConvention**.</span></span>  

``` csharp
public class SchoolEntities : DbContext
{
     . . .

    protected override void OnModelCreating(DbModelBuilder modelBuilder)
    {
        // Configure Code First to ignore PluralizingTableName convention
        // If you keep this convention, the generated tables  
        // will have pluralized names.
        modelBuilder.Conventions.Remove<PluralizingTableNameConvention>();
    }
}
```  

## <a name="custom-conventions"></a><span data-ttu-id="88f44-150">Соглашения об именовании</span><span class="sxs-lookup"><span data-stu-id="88f44-150">Custom Conventions</span></span>  

<span data-ttu-id="88f44-151">Соглашения об именовании, поддерживаются в EF6 и выше.</span><span class="sxs-lookup"><span data-stu-id="88f44-151">Custom conventions are supported in EF6 onwards.</span></span> <span data-ttu-id="88f44-152">Дополнительные сведения см. в разделе [первый соглашения о написании пользовательского кода](~/ef6/modeling/code-first/conventions/custom.md).</span><span class="sxs-lookup"><span data-stu-id="88f44-152">For more information see [Custom Code First Conventions](~/ef6/modeling/code-first/conventions/custom.md).</span></span>
