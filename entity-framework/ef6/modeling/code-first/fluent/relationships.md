---
title: Fluent API - связей — EF6
author: divega
ms.date: 2016-10-23
ms.assetid: fd73b4f8-16d5-40f1-9640-885ceafe67a1
ms.openlocfilehash: e13a1370f46362e0f2ca3743ec5b063ce6f87cbe
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/27/2018
ms.locfileid: "42994766"
---
# <a name="fluent-api---relationships"></a><span data-ttu-id="ce935-102">Fluent API - связи</span><span class="sxs-lookup"><span data-stu-id="ce935-102">Fluent API - Relationships</span></span>
> [!NOTE]
> <span data-ttu-id="ce935-103">Эта страница содержит сведения о настройке связи в модели Code First с помощью текучего API.</span><span class="sxs-lookup"><span data-stu-id="ce935-103">This page provides information about setting up relationships in your Code First model using the fluent API.</span></span> <span data-ttu-id="ce935-104">Общие сведения о связях в EF и способах доступа к данных и управления ими с помощью связей см. в разделе [связей и свойств навигации](~/ef6/fundamentals/relationships.md).</span><span class="sxs-lookup"><span data-stu-id="ce935-104">For general information about relationships in EF and how to access and manipulate data using relationships, see [Relationships & Navigation Properties](~/ef6/fundamentals/relationships.md).</span></span>  

<span data-ttu-id="ce935-105">При работе в режиме Code First, можно определить модель, определение классов CLR вашего домена.</span><span class="sxs-lookup"><span data-stu-id="ce935-105">When working with Code First, you define your model by defining your domain CLR classes.</span></span> <span data-ttu-id="ce935-106">По умолчанию Entity Framework использует соглашения Code First для сопоставления классов со схемой базы данных.</span><span class="sxs-lookup"><span data-stu-id="ce935-106">By default, Entity Framework uses the Code First conventions to map your classes to the database schema.</span></span> <span data-ttu-id="ce935-107">При использовании Code First соглашения об именовании, в большинстве случаев можно положиться на Code First, устанавливать отношения между таблицами на основе внешних ключей и свойства навигации, которые определяются в классах.</span><span class="sxs-lookup"><span data-stu-id="ce935-107">If you use the Code First naming conventions, in most cases you can rely on Code First to set up relationships between your tables based on the foreign keys and navigation properties that you define on the classes.</span></span> <span data-ttu-id="ce935-108">Если не использовать соглашения при определении классов, если вы хотите изменить способ правила работают, можно использовать fluent API или заметки к данным для настройки классов, поэтому Code First можно сопоставить связи между таблицами.</span><span class="sxs-lookup"><span data-stu-id="ce935-108">If you do not follow the conventions when defining your classes, or if you want to change the way the conventions work, you can use the fluent API or data annotations to configure your classes so Code First can map the relationships between your tables.</span></span>  

## <a name="introduction"></a><span data-ttu-id="ce935-109">Вступление</span><span class="sxs-lookup"><span data-stu-id="ce935-109">Introduction</span></span>  

<span data-ttu-id="ce935-110">При настройке связи с помощью текучего API, сначала с экземпляром EntityTypeConfiguration и затем используется метод HasRequired, HasOptional или HasMany для указания типа отношения, в которых участвует эта сущность.</span><span class="sxs-lookup"><span data-stu-id="ce935-110">When configuring a relationship with the fluent API, you start with the EntityTypeConfiguration instance and then use the HasRequired, HasOptional, or HasMany method to specify the type of relationship this entity participates in.</span></span> <span data-ttu-id="ce935-111">Методы HasRequired и HasOptional принимают лямбда-выражение, представляющее свойство навигации ссылки.</span><span class="sxs-lookup"><span data-stu-id="ce935-111">The HasRequired and HasOptional methods take a lambda expression that represents a reference navigation property.</span></span> <span data-ttu-id="ce935-112">Метод HasMany принимает лямбда-выражение, представляющее свойство навигации по коллекции.</span><span class="sxs-lookup"><span data-stu-id="ce935-112">The HasMany method takes a lambda expression that represents a collection navigation property.</span></span> <span data-ttu-id="ce935-113">Свойство обратной навигации, которое затем можно настроить с помощью методов WithRequired WithOptional и WithMany.</span><span class="sxs-lookup"><span data-stu-id="ce935-113">You can then configure an inverse navigation property by using the WithRequired, WithOptional, and WithMany methods.</span></span> <span data-ttu-id="ce935-114">Эти методы имеют перегрузки, которые не принимают аргументы и может использоваться для указания количества элементов с однонаправленным переходов.</span><span class="sxs-lookup"><span data-stu-id="ce935-114">These methods have overloads that do not take arguments and can be used to specify cardinality with unidirectional navigations.</span></span>  

<span data-ttu-id="ce935-115">Затем можно настроить свойства внешнего ключа с помощью метода HasForeignKey.</span><span class="sxs-lookup"><span data-stu-id="ce935-115">You can then configure foreign key properties by using the HasForeignKey method.</span></span> <span data-ttu-id="ce935-116">Этот метод принимает лямбда-выражение, представляющее свойство для использования в качестве внешнего ключа.</span><span class="sxs-lookup"><span data-stu-id="ce935-116">This method takes a lambda expression that represents the property to be used as the foreign key.</span></span>  

## <a name="configuring-a-required-to-optional-relationship-one-tozero-or-one"></a><span data-ttu-id="ce935-117">Настройка необходимых к дополнительную связь (к нулю или одному)</span><span class="sxs-lookup"><span data-stu-id="ce935-117">Configuring a Required-to-Optional Relationship (One-to–Zero-or-One)</span></span>  

<span data-ttu-id="ce935-118">В следующем примере настраивается отношение один к нулю или одному.</span><span class="sxs-lookup"><span data-stu-id="ce935-118">The following example configures a one-to-zero-or-one relationship.</span></span> <span data-ttu-id="ce935-119">OfficeAssignment имеет InstructorID свойства, которое является первичным ключом и внешнего ключа, так как имя свойства не соответствует соглашение, которое метод HasKey используется для настройки первичного ключа.</span><span class="sxs-lookup"><span data-stu-id="ce935-119">The OfficeAssignment has the InstructorID property that is a primary key and a foreign key, because the name of the property does not follow the convention the HasKey method is used to configure the primary key.</span></span>  

``` csharp
// Configure the primary key for the OfficeAssignment
modelBuilder.Entity<OfficeAssignment>()
    .HasKey(t => t.InstructorID);

// Map one-to-zero or one relationship
modelBuilder.Entity<OfficeAssignment>()
    .HasRequired(t => t.Instructor)
    .WithOptional(t => t.OfficeAssignment);
```  

## <a name="configuring-a-relationship-where-both-ends-are-required-one-to-one"></a><span data-ttu-id="ce935-120">Настройка связи, где требуются (один к одному) обоих концах</span><span class="sxs-lookup"><span data-stu-id="ce935-120">Configuring a Relationship Where Both Ends Are Required (One-to-One)</span></span>  

<span data-ttu-id="ce935-121">В большинстве случаев Entity Framework может определить, какой тип является зависимым и который является основным для связи.</span><span class="sxs-lookup"><span data-stu-id="ce935-121">In most cases Entity Framework can infer which type is the dependent and which is the principal in a relationship.</span></span> <span data-ttu-id="ce935-122">Тем не менее при оба конца связи являются обязательными или обеих сторон необязательно Entity Framework не может идентифицировать зависимым и участника.</span><span class="sxs-lookup"><span data-stu-id="ce935-122">However, when both ends of the relationship are required or both sides are optional Entity Framework cannot identify the dependent and principal.</span></span> <span data-ttu-id="ce935-123">Если требуются оба конца связи, используйте WithRequiredPrincipal или WithRequiredDependent после метода HasRequired.</span><span class="sxs-lookup"><span data-stu-id="ce935-123">When both ends of the relationship are required, use WithRequiredPrincipal or WithRequiredDependent after the HasRequired method.</span></span> <span data-ttu-id="ce935-124">Когда оба конца связи являются необязательными, используйте WithOptionalPrincipal или WithOptionalDependent после метода HasOptional.</span><span class="sxs-lookup"><span data-stu-id="ce935-124">When both ends of the relationship are optional, use WithOptionalPrincipal or WithOptionalDependent after the HasOptional method.</span></span>  

``` csharp
// Configure the primary key for the OfficeAssignment
modelBuilder.Entity<OfficeAssignment>()
    .HasKey(t => t.InstructorID);

modelBuilder.Entity<Instructor>()
    .HasRequired(t => t.OfficeAssignment)
    .WithRequiredPrincipal(t => t.Instructor);
```  

## <a name="configuring-a-many-to-many-relationship"></a><span data-ttu-id="ce935-125">Настройка связи многие ко многим</span><span class="sxs-lookup"><span data-stu-id="ce935-125">Configuring a Many-to-Many Relationship</span></span>  

<span data-ttu-id="ce935-126">Приведенный ниже код настраивает отношение "многие ко многим" между типами Course и Instructor.</span><span class="sxs-lookup"><span data-stu-id="ce935-126">The following code configures a many-to-many relationship between the Course and Instructor types.</span></span> <span data-ttu-id="ce935-127">В следующем примере Чтобы создать таблицу соединения используются соглашения Code First по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="ce935-127">In the following example, the default Code First conventions are used to create a join table.</span></span> <span data-ttu-id="ce935-128">В результате создается таблица CourseInstructor со столбцами Course_CourseID и Instructor_InstructorID.</span><span class="sxs-lookup"><span data-stu-id="ce935-128">As a result the CourseInstructor table is created with Course_CourseID and Instructor_InstructorID columns.</span></span>  

``` csharp
modelBuilder.Entity<Course>()
    .HasMany(t => t.Instructors)
    .WithMany(t => t.Courses)
```  

<span data-ttu-id="ce935-129">Если вы хотите указать имя таблицы соединения и имена столбцов в таблице, необходимо выполнить дополнительную настройку с помощью метода карты.</span><span class="sxs-lookup"><span data-stu-id="ce935-129">If you want to specify the join table name and the names of the columns in the table you need to do additional configuration by using the Map method.</span></span> <span data-ttu-id="ce935-130">Следующий код приводит к возникновению ошибки CourseInstructor таблицу со столбцами, CourseID и InstructorID.</span><span class="sxs-lookup"><span data-stu-id="ce935-130">The following code generates the CourseInstructor table with CourseID and InstructorID columns.</span></span>  

``` csharp
modelBuilder.Entity<Course>()
    .HasMany(t => t.Instructors)
    .WithMany(t => t.Courses)
    .Map(m =>
    {
        m.ToTable("CourseInstructor");
        m.MapLeftKey("CourseID");
        m.MapRightKey("InstructorID");
    });
```  

## <a name="configuring-a-relationship-with-one-navigation-property"></a><span data-ttu-id="ce935-131">Настройка связи с одним свойством навигации</span><span class="sxs-lookup"><span data-stu-id="ce935-131">Configuring a Relationship with One Navigation Property</span></span>  

<span data-ttu-id="ce935-132">Однонаправленное (также называемый однонаправленные) связи выполняется во время определения только на одном из конечных точках, а не на оба свойства навигации.</span><span class="sxs-lookup"><span data-stu-id="ce935-132">A one-directional (also called unidirectional) relationship is when a navigation property is defined on only one of the relationship ends and not on both.</span></span> <span data-ttu-id="ce935-133">По соглашению Code First всегда интерпретирует Однонаправленная связь как один ко многим.</span><span class="sxs-lookup"><span data-stu-id="ce935-133">By convention, Code First always interprets a unidirectional relationship as one-to-many.</span></span> <span data-ttu-id="ce935-134">Например если вы хотите, чтобы однозначное соответствие между Instructor и OfficeAssignment, при наличии свойства навигации на только тип данных Instructor, необходимо использовать текучий API для настройки этой связи.</span><span class="sxs-lookup"><span data-stu-id="ce935-134">For example, if you want a one-to-one relationship between Instructor and OfficeAssignment, where you have a navigation property on only the Instructor type, you need to use the fluent API to configure this relationship.</span></span>  

``` csharp
// Configure the primary Key for the OfficeAssignment
modelBuilder.Entity<OfficeAssignment>()
    .HasKey(t => t.InstructorID);

modelBuilder.Entity<Instructor>()
    .HasRequired(t => t.OfficeAssignment)
    .WithRequiredPrincipal();
```  

## <a name="enabling-cascade-delete"></a><span data-ttu-id="ce935-135">Включение каскадное удаление</span><span class="sxs-lookup"><span data-stu-id="ce935-135">Enabling Cascade Delete</span></span>  

<span data-ttu-id="ce935-136">Можно настроить каскадное удаление для связи с помощью метода WillCascadeOnDelete.</span><span class="sxs-lookup"><span data-stu-id="ce935-136">You can configure cascade delete on a relationship by using the WillCascadeOnDelete method.</span></span> <span data-ttu-id="ce935-137">Если внешний ключ для зависимой сущности не допускает значения NULL, то Code First устанавливает каскадное удаление в отношении.</span><span class="sxs-lookup"><span data-stu-id="ce935-137">If a foreign key on the dependent entity is not nullable, then Code First sets cascade delete on the relationship.</span></span> <span data-ttu-id="ce935-138">Если внешний ключ для зависимой сущности допускает значения NULL, Code First не устанавливает каскадное удаление для связи, и при удалении участника внешний ключ устанавливается в значение null.</span><span class="sxs-lookup"><span data-stu-id="ce935-138">If a foreign key on the dependent entity is nullable, Code First does not set cascade delete on the relationship, and when the principal is deleted the foreign key will be set to null.</span></span>  

<span data-ttu-id="ce935-139">Эти правила cascade delete можно удалить с помощью:</span><span class="sxs-lookup"><span data-stu-id="ce935-139">You can remove these cascade delete conventions by using:</span></span>  

<span data-ttu-id="ce935-140">modelBuilder.Conventions.Remove\<OneToManyCascadeDeleteConvention\>)</span><span class="sxs-lookup"><span data-stu-id="ce935-140">modelBuilder.Conventions.Remove\<OneToManyCascadeDeleteConvention\>()</span></span>  
<span data-ttu-id="ce935-141">modelBuilder.Conventions.Remove\<ManyToManyCascadeDeleteConvention\>)</span><span class="sxs-lookup"><span data-stu-id="ce935-141">modelBuilder.Conventions.Remove\<ManyToManyCascadeDeleteConvention\>()</span></span>  

<span data-ttu-id="ce935-142">Следующий код настраивает требуемую связь, а затем отключает каскадное удаление.</span><span class="sxs-lookup"><span data-stu-id="ce935-142">The following code configures the relationship to be required and then disables cascade delete.</span></span>  

``` csharp
modelBuilder.Entity<Course>()
    .HasRequired(t => t.Department)
    .WithMany(t => t.Courses)
    .HasForeignKey(d => d.DepartmentID)
    .WillCascadeOnDelete(false);
```  

## <a name="configuring-a-composite-foreign-key"></a><span data-ttu-id="ce935-143">Настройка составного внешнего ключа</span><span class="sxs-lookup"><span data-stu-id="ce935-143">Configuring a Composite Foreign Key</span></span>  

<span data-ttu-id="ce935-144">Если первичный ключ типа отдела состоял из свойств DepartmentID и Name, вы бы настроить первичный ключ для подразделения и внешнего ключа типов курсов следующим образом:</span><span class="sxs-lookup"><span data-stu-id="ce935-144">If the primary key on the Department type consisted of DepartmentID and Name properties, you would configure the primary key for the Department and the foreign key on the Course types as follows:</span></span>  

``` csharp
// Composite primary key
modelBuilder.Entity<Department>()
.HasKey(d => new { d.DepartmentID, d.Name });

// Composite foreign key
modelBuilder.Entity<Course>()  
    .HasRequired(c => c.Department)  
    .WithMany(d => d.Courses)
    .HasForeignKey(d => new { d.DepartmentID, d.DepartmentName });
```  

## <a name="renaming-a-foreign-key-that-is-not-defined-in-the-model"></a><span data-ttu-id="ce935-145">Переименование внешний ключ, который не определен в модели</span><span class="sxs-lookup"><span data-stu-id="ce935-145">Renaming a Foreign Key That Is Not Defined in the Model</span></span>  

<span data-ttu-id="ce935-146">Если вы не захотите определения внешнего ключа в типе CLR, но, чтобы задать имя, она должна иметь в базе данных, сделайте следующее:</span><span class="sxs-lookup"><span data-stu-id="ce935-146">If you choose not to define a foreign key on the CLR type, but want to specify what name it should have in the database, do the following:</span></span>  

``` csharp
modelBuilder.Entity<Course>()
    .HasRequired(c => c.Department)
    .WithMany(t => t.Courses)
    .Map(m => m.MapKey("ChangedDepartmentID"));
```  

## <a name="configuring-a-foreign-key-name-that-does-not-follow-the-code-first-convention"></a><span data-ttu-id="ce935-147">Настройка имя внешнего ключа, который является исключением из соглашения первого кода</span><span class="sxs-lookup"><span data-stu-id="ce935-147">Configuring a Foreign Key Name That Does Not Follow the Code First Convention</span></span>  

<span data-ttu-id="ce935-148">Если свойства внешнего ключа в классе курс называется SomeDepartmentID вместо DepartmentID, необходимо выполнить следующую команду, чтобы указать, что SomeDepartmentID в качестве внешнего ключа:</span><span class="sxs-lookup"><span data-stu-id="ce935-148">If the foreign key property on the Course class was called SomeDepartmentID instead of DepartmentID, you would need to do the following to specify that you want SomeDepartmentID to be the foreign key:</span></span>  

``` csharp
modelBuilder.Entity<Course>()
         .HasRequired(c => c.Department)
         .WithMany(d => d.Courses)
         .HasForeignKey(c => c.SomeDepartmentID);
```  

## <a name="model-used-in-samples"></a><span data-ttu-id="ce935-149">Модели, используемые в примерах</span><span class="sxs-lookup"><span data-stu-id="ce935-149">Model Used in Samples</span></span>  

<span data-ttu-id="ce935-150">Для примеров на этой странице используется следующая модель Code First.</span><span class="sxs-lookup"><span data-stu-id="ce935-150">The following Code First model is used for the samples on this page.</span></span>  

``` csharp
using System.Data.Entity;
using System.Data.Entity.ModelConfiguration.Conventions;
// add a reference to System.ComponentModel.DataAnnotations DLL
using System.ComponentModel.DataAnnotations;
using System.Collections.Generic;
using System;

public class SchoolEntities : DbContext
{
    public DbSet<Course> Courses { get; set; }
    public DbSet<Department> Departments { get; set; }
    public DbSet<Instructor> Instructors { get; set; }
    public DbSet<OfficeAssignment> OfficeAssignments { get; set; }

    protected override void OnModelCreating(DbModelBuilder modelBuilder)
    {
        // Configure Code First to ignore PluralizingTableName convention
        // If you keep this convention then the generated tables will have pluralized names.
        modelBuilder.Conventions.Remove<PluralizingTableNameConvention>();
    }
}

public class Department
{
    public Department()
    {
        this.Courses = new HashSet<Course>();
    }
    // Primary key
    public int DepartmentID { get; set; }
    public string Name { get; set; }
    public decimal Budget { get; set; }
    public System.DateTime StartDate { get; set; }
    public int? Administrator { get; set; }

    // Navigation property
    public virtual ICollection<Course> Courses { get; private set; }
}

public class Course
{
    public Course()
    {
        this.Instructors = new HashSet<Instructor>();
    }
    // Primary key
    public int CourseID { get; set; }

    public string Title { get; set; }
    public int Credits { get; set; }

    // Foreign key
    public int DepartmentID { get; set; }

    // Navigation properties
    public virtual Department Department { get; set; }
    public virtual ICollection<Instructor> Instructors { get; private set; }
}

public partial class OnlineCourse : Course
{
    public string URL { get; set; }
}

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

public class Instructor
{
    public Instructor()
    {
        this.Courses = new List<Course>();
    }

    // Primary key
    public int InstructorID { get; set; }
    public string LastName { get; set; }
    public string FirstName { get; set; }
    public System.DateTime HireDate { get; set; }

    // Navigation properties
    public virtual ICollection<Course> Courses { get; private set; }
}

public class OfficeAssignment
{
    // Specifying InstructorID as a primary
    [Key()]
    public Int32 InstructorID { get; set; }

    public string Location { get; set; }

    // When Entity Framework sees Timestamp attribute
    // it configures ConcurrencyCheck and DatabaseGeneratedPattern=Computed.
    [Timestamp]
    public Byte[] Timestamp { get; set; }

    // Navigation property
    public virtual Instructor Instructor { get; set; }
}
```  
