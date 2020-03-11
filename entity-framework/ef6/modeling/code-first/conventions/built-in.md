---
title: Соглашения Code First — EF6
author: divega
ms.date: 10/23/2016
ms.assetid: bc644573-c2b2-4ed7-8745-3c37c41058ad
ms.openlocfilehash: 4d03a32db5d84eb37c22617a95005b272172a65d
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/06/2020
ms.locfileid: "78415949"
---
# <a name="code-first-conventions"></a><span data-ttu-id="7508d-102">Соглашения Code First</span><span class="sxs-lookup"><span data-stu-id="7508d-102">Code First Conventions</span></span>
<span data-ttu-id="7508d-103">Code First позволяет описать модель с помощью C# среды или Visual Basic классов .NET.</span><span class="sxs-lookup"><span data-stu-id="7508d-103">Code First enables you to describe a model by using C# or Visual Basic .NET classes.</span></span> <span data-ttu-id="7508d-104">Базовая форма модели определяется с помощью соглашений.</span><span class="sxs-lookup"><span data-stu-id="7508d-104">The basic shape of the model is detected by using conventions.</span></span> <span data-ttu-id="7508d-105">Соглашения — это наборы правил, которые используются для автоматической настройки концептуальной модели на основе определений классов при работе с Code First.</span><span class="sxs-lookup"><span data-stu-id="7508d-105">Conventions are sets of rules that are used to automatically configure a conceptual model based on class definitions when working with Code First.</span></span> <span data-ttu-id="7508d-106">Соглашения определяются в пространстве имен System. Data. Entity. Моделконфигуратион. Conventions.</span><span class="sxs-lookup"><span data-stu-id="7508d-106">The conventions are defined in the System.Data.Entity.ModelConfiguration.Conventions namespace.</span></span>  

<span data-ttu-id="7508d-107">Вы можете дополнительно настроить модель с помощью заметок к данным или API Fluent.</span><span class="sxs-lookup"><span data-stu-id="7508d-107">You can further configure your model by using data annotations or the fluent API.</span></span> <span data-ttu-id="7508d-108">Приоритет предоставляется в конфигурации через API Fluent, за которым следуют заметки к данным, а затем соглашения.</span><span class="sxs-lookup"><span data-stu-id="7508d-108">Precedence is given to configuration through the fluent API followed by data annotations and then conventions.</span></span> <span data-ttu-id="7508d-109">Дополнительные сведения см. в статье [заметки к данным](~/ef6/modeling/code-first/data-annotations.md), [интерфейсы API Fluent](~/ef6/modeling/code-first/fluent/relationships.md), [типы API Fluent & свойства](~/ef6/modeling/code-first/fluent/types-and-properties.md) и [fluent API с помощью VB.NET](~/ef6/modeling/code-first/fluent/vb.md).</span><span class="sxs-lookup"><span data-stu-id="7508d-109">For more information see [Data Annotations](~/ef6/modeling/code-first/data-annotations.md), [Fluent API - Relationships](~/ef6/modeling/code-first/fluent/relationships.md), [Fluent API - Types & Properties](~/ef6/modeling/code-first/fluent/types-and-properties.md) and [Fluent API with VB.NET](~/ef6/modeling/code-first/fluent/vb.md).</span></span>  

<span data-ttu-id="7508d-110">Подробный список соглашений Code First можно найти в документации по [API](https://msdn.microsoft.com/library/system.data.entity.modelconfiguration.conventions.aspx).</span><span class="sxs-lookup"><span data-stu-id="7508d-110">A detailed list of Code First conventions is available in the [API Documentation](https://msdn.microsoft.com/library/system.data.entity.modelconfiguration.conventions.aspx).</span></span> <span data-ttu-id="7508d-111">В этом разделе приводятся общие сведения о соглашениях, используемых Code First.</span><span class="sxs-lookup"><span data-stu-id="7508d-111">This topic provides an overview of the conventions used by Code First.</span></span>  

## <a name="type-discovery"></a><span data-ttu-id="7508d-112">Обнаружение типов</span><span class="sxs-lookup"><span data-stu-id="7508d-112">Type Discovery</span></span>  

<span data-ttu-id="7508d-113">При использовании Code First разработки обычно начинается написание .NET Framework классов, определяющих концептуальную (доменную) модель.</span><span class="sxs-lookup"><span data-stu-id="7508d-113">When using Code First development you usually begin by writing .NET Framework classes that define your conceptual (domain) model.</span></span> <span data-ttu-id="7508d-114">Помимо определения классов необходимо также разрешить **DbContext** знать, какие типы необходимо включить в модель.</span><span class="sxs-lookup"><span data-stu-id="7508d-114">In addition to defining the classes, you also need to let **DbContext** know which types you want to include in the model.</span></span> <span data-ttu-id="7508d-115">Для этого необходимо определить класс контекста, который является производным от **DbContext** , и предоставляет свойства **DbSet** для типов, которые должны быть частью модели.</span><span class="sxs-lookup"><span data-stu-id="7508d-115">To do this, you define a context class that derives from **DbContext** and exposes **DbSet** properties for the types that you want to be part of the model.</span></span> <span data-ttu-id="7508d-116">Code First будут включать эти типы, а также будут запрашивать ссылочные типы, даже если типы, на которые имеются ссылки, определены в другой сборке.</span><span class="sxs-lookup"><span data-stu-id="7508d-116">Code First will include these types and also will pull in any referenced types, even if the referenced types are defined in a different assembly.</span></span>  

<span data-ttu-id="7508d-117">Если типы участвуют в иерархии наследования, достаточно определить свойство **DbSet** для базового класса, и производные типы будут автоматически включены в сборку, если они находятся в той же сборке, что и базовый класс.</span><span class="sxs-lookup"><span data-stu-id="7508d-117">If your types participate in an inheritance hierarchy, it is enough to define a **DbSet** property for the base class, and the derived types will be automatically included, if they are in the same assembly as the base class.</span></span>  

<span data-ttu-id="7508d-118">В следующем примере для класса **пункт SchoolEntities** определено только одно свойство **DbSet** (**Departments**).</span><span class="sxs-lookup"><span data-stu-id="7508d-118">In the following example, there is only one **DbSet** property defined on the **SchoolEntities** class (**Departments**).</span></span> <span data-ttu-id="7508d-119">Code First использует это свойство для обнаружения и извлечения любых ссылочных типов.</span><span class="sxs-lookup"><span data-stu-id="7508d-119">Code First uses this property to discover and pull in any referenced types.</span></span>  

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

<span data-ttu-id="7508d-120">Если необходимо исключить тип из модели, используйте атрибут **нотмаппед** или API **дбмоделбуилдер. Ignore** Fluent.</span><span class="sxs-lookup"><span data-stu-id="7508d-120">If you want to exclude a type from the model, use the **NotMapped** attribute or the **DbModelBuilder.Ignore** fluent API.</span></span>  

```  csharp
modelBuilder.Ignore<Department>();
```  

## <a name="primary-key-convention"></a><span data-ttu-id="7508d-121">Соглашение о первичном ключе</span><span class="sxs-lookup"><span data-stu-id="7508d-121">Primary Key Convention</span></span>  

<span data-ttu-id="7508d-122">Code First выводит, что свойство является первичным ключом, если свойство класса имеет имя "ID" (без учета регистра), или имя класса, за которым следует "ID".</span><span class="sxs-lookup"><span data-stu-id="7508d-122">Code First infers that a property is a primary key if a property on a class is named “ID” (not case sensitive), or the class name followed by "ID".</span></span> <span data-ttu-id="7508d-123">Если тип свойства первичного ключа является числовым или идентификатором GUID, он будет настроен в качестве столбца идентификаторов.</span><span class="sxs-lookup"><span data-stu-id="7508d-123">If the type of the primary key property is numeric or GUID it will be configured as an identity column.</span></span>  

``` csharp
public class Department
{
    // Primary key
    public int DepartmentID { get; set; }

    . . .  

}
```  

## <a name="relationship-convention"></a><span data-ttu-id="7508d-124">Соглашение о связях</span><span class="sxs-lookup"><span data-stu-id="7508d-124">Relationship Convention</span></span>  

<span data-ttu-id="7508d-125">В Entity Framework свойства навигации предоставляют способ навигации по связям между двумя типами сущностей.</span><span class="sxs-lookup"><span data-stu-id="7508d-125">In Entity Framework, navigation properties provide a way to navigate a relationship between two entity types.</span></span> <span data-ttu-id="7508d-126">Каждый объект может обладать свойством навигации для каждого отношения, в котором участвует.</span><span class="sxs-lookup"><span data-stu-id="7508d-126">Every object can have a navigation property for every relationship in which it participates.</span></span> <span data-ttu-id="7508d-127">Свойства навигации позволяют перемещать связи и управлять ими в обоих направлениях, возвращая либо ссылочный объект (если кратность — либо одна, либо нулевая или-одна), либо коллекция (если количество элементов равно многим).</span><span class="sxs-lookup"><span data-stu-id="7508d-127">Navigation properties allow you to navigate and manage relationships in both directions, returning either a reference object (if the multiplicity is either one or zero-or-one) or a collection (if the multiplicity is many).</span></span> <span data-ttu-id="7508d-128">Code First определяет связи на основе свойств навигации, определенных в типах.</span><span class="sxs-lookup"><span data-stu-id="7508d-128">Code First infers relationships based on the navigation properties defined on your types.</span></span>  

<span data-ttu-id="7508d-129">В дополнение к свойствам навигации рекомендуется включить свойства внешнего ключа для типов, представляющих зависимые объекты.</span><span class="sxs-lookup"><span data-stu-id="7508d-129">In addition to navigation properties, we recommend that you include foreign key properties on the types that represent dependent objects.</span></span> <span data-ttu-id="7508d-130">Любое свойство с тем же типом данных, что и основное свойство первичного ключа, и имя, следующее за одним из следующих форматов, представляют внешний ключ для связи: "\<имя свойства навигации\>\<имя свойства основного ключа\>", "\<имя основного класса\>\<имя свойства первичного ключа\>", или "\<имя свойства основного ключа\>".</span><span class="sxs-lookup"><span data-stu-id="7508d-130">Any property with the same data type as the principal primary key property and with a name that follows one of the following formats represents a foreign key for the relationship: '\<navigation property name\>\<principal primary key property name\>', '\<principal class name\>\<primary key property name\>', or '\<principal primary key property name\>'.</span></span> <span data-ttu-id="7508d-131">Если найдено несколько совпадений, приоритет передается в указанном выше порядке.</span><span class="sxs-lookup"><span data-stu-id="7508d-131">If multiple matches are found then precedence is given in the order listed above.</span></span> <span data-ttu-id="7508d-132">При обнаружении внешнего ключа не учитывается регистр.</span><span class="sxs-lookup"><span data-stu-id="7508d-132">Foreign key detection is not case sensitive.</span></span> <span data-ttu-id="7508d-133">При обнаружении свойства внешнего ключа Code First определяет количество элементов связи на основе допустимости значений NULL внешнего ключа.</span><span class="sxs-lookup"><span data-stu-id="7508d-133">When a foreign key property is detected, Code First infers the multiplicity of the relationship based on the nullability of the foreign key.</span></span> <span data-ttu-id="7508d-134">Если свойство допускает значение null, то связь регистрируется как необязательнаая. в противном случае связь регистрируется по мере необходимости.</span><span class="sxs-lookup"><span data-stu-id="7508d-134">If the property is nullable then the relationship is registered as optional; otherwise the relationship is registered as required.</span></span>  

<span data-ttu-id="7508d-135">Если внешний ключ в зависимой сущности не допускает значения NULL, то Code First задает каскадное удаление для связи.</span><span class="sxs-lookup"><span data-stu-id="7508d-135">If a foreign key on the dependent entity is not nullable, then Code First sets cascade delete on the relationship.</span></span> <span data-ttu-id="7508d-136">Если внешний ключ в зависимой сущности допускает значение null, Code First не устанавливает каскадное удаление для связи, и при удалении участника внешний ключ будет установлен в значение null.</span><span class="sxs-lookup"><span data-stu-id="7508d-136">If a foreign key on the dependent entity is nullable, Code First does not set cascade delete on the relationship, and when the principal is deleted the foreign key will be set to null.</span></span> <span data-ttu-id="7508d-137">Поведение количества элементов и каскадного удаления, обнаруженное соглашением, может быть переопределено с помощью API Fluent.</span><span class="sxs-lookup"><span data-stu-id="7508d-137">The multiplicity and cascade delete behavior detected by convention can be overridden by using the fluent API.</span></span>  

<span data-ttu-id="7508d-138">В следующем примере свойства навигации и внешний ключ используются для определения связи между классами Department и Course.</span><span class="sxs-lookup"><span data-stu-id="7508d-138">In the following example the navigation properties and a foreign key are used to define the relationship between the Department and Course classes.</span></span>  

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
> <span data-ttu-id="7508d-139">При наличии нескольких связей между одними и теми же типами (например, предположим, что вы определили классы **Person** и **Book** , где класс **Person** содержит свойства навигации **ревиеведбукс** и **Аусоредбукс** , а класс **Book** содержит свойства навигации " **Автор** " и " **проверяющий** "), необходимо вручную настроить связи с помощью заметок к данным или API-интерфейса Fluent.</span><span class="sxs-lookup"><span data-stu-id="7508d-139">If you have multiple relationships between the same types (for example, suppose you define the **Person** and **Book** classes, where the **Person** class contains the **ReviewedBooks** and **AuthoredBooks** navigation properties and the **Book** class contains the **Author** and **Reviewer** navigation properties) you need to manually configure the relationships by using Data Annotations or the fluent API.</span></span> <span data-ttu-id="7508d-140">Дополнительные сведения см. в статье связи [аннотаций данных](~/ef6/modeling/code-first/data-annotations.md) и [интерфейсов API Fluent](~/ef6/modeling/code-first/fluent/relationships.md).</span><span class="sxs-lookup"><span data-stu-id="7508d-140">For more information, see [Data Annotations - Relationships](~/ef6/modeling/code-first/data-annotations.md) and [Fluent API - Relationships](~/ef6/modeling/code-first/fluent/relationships.md).</span></span>  

## <a name="complex-types-convention"></a><span data-ttu-id="7508d-141">Соглашение о сложных типах</span><span class="sxs-lookup"><span data-stu-id="7508d-141">Complex Types Convention</span></span>  

<span data-ttu-id="7508d-142">Когда Code First обнаруживает определение класса, в котором не удается вывести первичный ключ и не зарегистрирован первичный ключ с помощью аннотаций данных или API-интерфейса Fluent, тип автоматически регистрируется как сложный тип.</span><span class="sxs-lookup"><span data-stu-id="7508d-142">When Code First discovers a class definition where a primary key cannot be inferred, and no primary key is registered through data annotations or the fluent API, then the type is automatically registered as a complex type.</span></span> <span data-ttu-id="7508d-143">Для обнаружения сложного типа также требуется, чтобы тип не имел свойств, ссылающихся на типы сущностей и на которые нет ссылок из свойства коллекции другого типа.</span><span class="sxs-lookup"><span data-stu-id="7508d-143">Complex type detection also requires that the type does not have properties that reference entity types and is not referenced from a collection property on another type.</span></span> <span data-ttu-id="7508d-144">Учитывая следующие определения классов Code First выводит, что **сведения** являются сложным типом, так как у него нет первичного ключа.</span><span class="sxs-lookup"><span data-stu-id="7508d-144">Given the following class definitions Code First would infer that **Details** is a complex type because it has no primary key.</span></span>  

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

## <a name="connection-string-convention"></a><span data-ttu-id="7508d-145">Соглашение строки соединения</span><span class="sxs-lookup"><span data-stu-id="7508d-145">Connection String Convention</span></span>  

<span data-ttu-id="7508d-146">Чтобы узнать о соглашениях, которые DbContext использует для обнаружения подключения для использования, см. раздел [подключения и модели](~/ef6/fundamentals/configuring/connection-strings.md).</span><span class="sxs-lookup"><span data-stu-id="7508d-146">To learn about the conventions that DbContext uses to discover the connection to use see [Connections and Models](~/ef6/fundamentals/configuring/connection-strings.md).</span></span>  

## <a name="removing-conventions"></a><span data-ttu-id="7508d-147">Удаление соглашений</span><span class="sxs-lookup"><span data-stu-id="7508d-147">Removing Conventions</span></span>  

<span data-ttu-id="7508d-148">Можно удалить любое из соглашений, определенных в пространстве имен System. Data. Entity. Моделконфигуратион. Conventions.</span><span class="sxs-lookup"><span data-stu-id="7508d-148">You can remove any of the conventions defined in the System.Data.Entity.ModelConfiguration.Conventions namespace.</span></span> <span data-ttu-id="7508d-149">В следующем примере удаляется **плурализингтабленамеконвентион**.</span><span class="sxs-lookup"><span data-stu-id="7508d-149">The following example removes **PluralizingTableNameConvention**.</span></span>  

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

## <a name="custom-conventions"></a><span data-ttu-id="7508d-150">Настраиваемые соглашения</span><span class="sxs-lookup"><span data-stu-id="7508d-150">Custom Conventions</span></span>  

<span data-ttu-id="7508d-151">Пользовательские соглашения поддерживаются в EF6.</span><span class="sxs-lookup"><span data-stu-id="7508d-151">Custom conventions are supported in EF6 onwards.</span></span> <span data-ttu-id="7508d-152">Дополнительные сведения см. в разделе [Custom Code First Conventions](~/ef6/modeling/code-first/conventions/custom.md).</span><span class="sxs-lookup"><span data-stu-id="7508d-152">For more information see [Custom Code First Conventions](~/ef6/modeling/code-first/conventions/custom.md).</span></span>
