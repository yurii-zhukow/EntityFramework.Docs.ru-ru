---
title: Интерфейсы API Fluent — взаимосвязи — EF6
description: Связи API Fluent в Entity Framework 6
author: ajcvickers
ms.date: 10/23/2016
uid: ef6/modeling/code-first/fluent/relationships
ms.openlocfilehash: 7c62dfd788afda41f4c940836152b9114aa545e1
ms.sourcegitcommit: 0a25c03fa65ae6e0e0e3f66bac48d59eceb96a5a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/14/2020
ms.locfileid: "92065164"
---
# <a name="fluent-api---relationships"></a><span data-ttu-id="827fd-103">Взаимосвязи API Fluent</span><span class="sxs-lookup"><span data-stu-id="827fd-103">Fluent API - Relationships</span></span>
> [!NOTE]
> <span data-ttu-id="827fd-104">На этой странице содержатся сведения о настройке связей в модели Code First с помощью API Fluent.</span><span class="sxs-lookup"><span data-stu-id="827fd-104">This page provides information about setting up relationships in your Code First model using the fluent API.</span></span> <span data-ttu-id="827fd-105">Общие сведения о связях в EF и доступе к данным и управлении ими с помощью связей см. в разделе [связи & свойства навигации](xref:ef6/fundamentals/relationships).</span><span class="sxs-lookup"><span data-stu-id="827fd-105">For general information about relationships in EF and how to access and manipulate data using relationships, see [Relationships & Navigation Properties](xref:ef6/fundamentals/relationships).</span></span>  

<span data-ttu-id="827fd-106">При работе с Code First вы определяете модель, определяя классы CLR домена.</span><span class="sxs-lookup"><span data-stu-id="827fd-106">When working with Code First, you define your model by defining your domain CLR classes.</span></span> <span data-ttu-id="827fd-107">По умолчанию Entity Framework использует соглашения Code First, чтобы сопоставлять классы с схемой базы данных.</span><span class="sxs-lookup"><span data-stu-id="827fd-107">By default, Entity Framework uses the Code First conventions to map your classes to the database schema.</span></span> <span data-ttu-id="827fd-108">Если используются соглашения об именовании Code First, в большинстве случаев можно полагаться на Code First, чтобы настроить связи между таблицами на основе внешних ключей и свойств навигации, определенных в классах.</span><span class="sxs-lookup"><span data-stu-id="827fd-108">If you use the Code First naming conventions, in most cases you can rely on Code First to set up relationships between your tables based on the foreign keys and navigation properties that you define on the classes.</span></span> <span data-ttu-id="827fd-109">Если вы не соответствуете соглашениям при определении классов или хотите изменить способ работы соглашений, можно использовать API-интерфейс Fluent или заметки к данным для настройки классов таким образом, чтобы Code First могли сопоставлять связи между таблицами.</span><span class="sxs-lookup"><span data-stu-id="827fd-109">If you do not follow the conventions when defining your classes, or if you want to change the way the conventions work, you can use the fluent API or data annotations to configure your classes so Code First can map the relationships between your tables.</span></span>  

## <a name="introduction"></a><span data-ttu-id="827fd-110">Введение</span><span class="sxs-lookup"><span data-stu-id="827fd-110">Introduction</span></span>  

<span data-ttu-id="827fd-111">При настройке связи с API Fluent необходимо начать с экземпляра Ентититипеконфигуратион, а затем использовать метод Хасрекуиред, Хасоптионал или Хасмани для указания типа связи, в которой участвует эта сущность.</span><span class="sxs-lookup"><span data-stu-id="827fd-111">When configuring a relationship with the fluent API, you start with the EntityTypeConfiguration instance and then use the HasRequired, HasOptional, or HasMany method to specify the type of relationship this entity participates in.</span></span> <span data-ttu-id="827fd-112">Методы Хасрекуиред и Хасоптионал принимают лямбда-выражение, представляющее свойство навигации ссылки.</span><span class="sxs-lookup"><span data-stu-id="827fd-112">The HasRequired and HasOptional methods take a lambda expression that represents a reference navigation property.</span></span> <span data-ttu-id="827fd-113">Метод Хасмани принимает лямбда-выражение, представляющее свойство навигации коллекции.</span><span class="sxs-lookup"><span data-stu-id="827fd-113">The HasMany method takes a lambda expression that represents a collection navigation property.</span></span> <span data-ttu-id="827fd-114">Затем можно настроить обратное свойство навигации с помощью методов Висрекуиред, Висоптионал и Висмани.</span><span class="sxs-lookup"><span data-stu-id="827fd-114">You can then configure an inverse navigation property by using the WithRequired, WithOptional, and WithMany methods.</span></span> <span data-ttu-id="827fd-115">Эти методы имеют перегрузки, которые не принимают аргументы и могут использоваться для задания количества элементов с однонаправленными переходами.</span><span class="sxs-lookup"><span data-stu-id="827fd-115">These methods have overloads that do not take arguments and can be used to specify cardinality with unidirectional navigations.</span></span>  

<span data-ttu-id="827fd-116">Затем можно настроить свойства внешнего ключа с помощью метода Хасфореигнкэй.</span><span class="sxs-lookup"><span data-stu-id="827fd-116">You can then configure foreign key properties by using the HasForeignKey method.</span></span> <span data-ttu-id="827fd-117">Этот метод принимает лямбда-выражение, представляющее свойство, которое будет использоваться в качестве внешнего ключа.</span><span class="sxs-lookup"><span data-stu-id="827fd-117">This method takes a lambda expression that represents the property to be used as the foreign key.</span></span>  

## <a name="configuring-a-required-to-optional-relationship-one-tozero-or-one"></a><span data-ttu-id="827fd-118">Настройка связи «обязательно-ко-многим» (один к одному — ноль или один)</span><span class="sxs-lookup"><span data-stu-id="827fd-118">Configuring a Required-to-Optional Relationship (One-to–Zero-or-One)</span></span>  

<span data-ttu-id="827fd-119">В следующем примере настраивается связь «один к нулю» или «одна-одна».</span><span class="sxs-lookup"><span data-stu-id="827fd-119">The following example configures a one-to-zero-or-one relationship.</span></span> <span data-ttu-id="827fd-120">OfficeAssignment имеет свойство InstructorID, которое является первичным ключом и внешним ключом, поскольку имя свойства не соответствует соглашению, для настройки первичного ключа используется метод HasKey.</span><span class="sxs-lookup"><span data-stu-id="827fd-120">The OfficeAssignment has the InstructorID property that is a primary key and a foreign key, because the name of the property does not follow the convention the HasKey method is used to configure the primary key.</span></span>  

``` csharp
// Configure the primary key for the OfficeAssignment
modelBuilder.Entity<OfficeAssignment>()
    .HasKey(t => t.InstructorID);

// Map one-to-zero or one relationship
modelBuilder.Entity<OfficeAssignment>()
    .HasRequired(t => t.Instructor)
    .WithOptional(t => t.OfficeAssignment);
```  

## <a name="configuring-a-relationship-where-both-ends-are-required-one-to-one"></a><span data-ttu-id="827fd-121">Настройка связи, где требуются оба конца (один к одному)</span><span class="sxs-lookup"><span data-stu-id="827fd-121">Configuring a Relationship Where Both Ends Are Required (One-to-One)</span></span>  

<span data-ttu-id="827fd-122">В большинстве случаев Entity Framework может определить, какой из типов является зависимым и является участником в связи.</span><span class="sxs-lookup"><span data-stu-id="827fd-122">In most cases Entity Framework can infer which type is the dependent and which is the principal in a relationship.</span></span> <span data-ttu-id="827fd-123">Однако, если требуются оба конца связи, или обе стороны являются необязательными Entity Framework не может опознать зависимый и основной субъект.</span><span class="sxs-lookup"><span data-stu-id="827fd-123">However, when both ends of the relationship are required or both sides are optional Entity Framework cannot identify the dependent and principal.</span></span> <span data-ttu-id="827fd-124">Если требуются оба конца связи, используйте ВисрекуиредпринЦипал или Висрекуиреддепендент после метода Хасрекуиред.</span><span class="sxs-lookup"><span data-stu-id="827fd-124">When both ends of the relationship are required, use WithRequiredPrincipal or WithRequiredDependent after the HasRequired method.</span></span> <span data-ttu-id="827fd-125">Если оба конца связи являются необязательными, используйте ВисоптионалпринЦипал или Висоптионалдепендент после метода Хасоптионал.</span><span class="sxs-lookup"><span data-stu-id="827fd-125">When both ends of the relationship are optional, use WithOptionalPrincipal or WithOptionalDependent after the HasOptional method.</span></span>  

``` csharp
// Configure the primary key for the OfficeAssignment
modelBuilder.Entity<OfficeAssignment>()
    .HasKey(t => t.InstructorID);

modelBuilder.Entity<Instructor>()
    .HasRequired(t => t.OfficeAssignment)
    .WithRequiredPrincipal(t => t.Instructor);
```  

## <a name="configuring-a-many-to-many-relationship"></a><span data-ttu-id="827fd-126">Настройка связи «многие ко многим»</span><span class="sxs-lookup"><span data-stu-id="827fd-126">Configuring a Many-to-Many Relationship</span></span>  

<span data-ttu-id="827fd-127">Следующий код настраивает связь «многие ко многим» между типами курсов и преподавателей.</span><span class="sxs-lookup"><span data-stu-id="827fd-127">The following code configures a many-to-many relationship between the Course and Instructor types.</span></span> <span data-ttu-id="827fd-128">В следующем примере для создания таблицы соединений используются соглашения Code First по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="827fd-128">In the following example, the default Code First conventions are used to create a join table.</span></span> <span data-ttu-id="827fd-129">В результате таблица Каурсеинструктор создается с Course_CourseID и Instructor_InstructorID столбцами.</span><span class="sxs-lookup"><span data-stu-id="827fd-129">As a result the CourseInstructor table is created with Course_CourseID and Instructor_InstructorID columns.</span></span>  

``` csharp
modelBuilder.Entity<Course>()
    .HasMany(t => t.Instructors)
    .WithMany(t => t.Courses)
```  

<span data-ttu-id="827fd-130">Если необходимо указать имя таблицы соединений и имена столбцов в таблице, необходимо выполнить дополнительную настройку с помощью метода Map.</span><span class="sxs-lookup"><span data-stu-id="827fd-130">If you want to specify the join table name and the names of the columns in the table you need to do additional configuration by using the Map method.</span></span> <span data-ttu-id="827fd-131">Следующий код создает таблицу Каурсеинструктор со столбцами CourseID и InstructorID.</span><span class="sxs-lookup"><span data-stu-id="827fd-131">The following code generates the CourseInstructor table with CourseID and InstructorID columns.</span></span>  

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

## <a name="configuring-a-relationship-with-one-navigation-property"></a><span data-ttu-id="827fd-132">Настройка связи с одним свойством навигации</span><span class="sxs-lookup"><span data-stu-id="827fd-132">Configuring a Relationship with One Navigation Property</span></span>  

<span data-ttu-id="827fd-133">Однонаправленная (также называемая однонаправленной) связь — это то, что свойство навигации определено только для одного из сторон связи, а не для обоих.</span><span class="sxs-lookup"><span data-stu-id="827fd-133">A one-directional (also called unidirectional) relationship is when a navigation property is defined on only one of the relationship ends and not on both.</span></span> <span data-ttu-id="827fd-134">По соглашению Code First всегда интерпретирует однонаправленную связь как "один ко многим".</span><span class="sxs-lookup"><span data-stu-id="827fd-134">By convention, Code First always interprets a unidirectional relationship as one-to-many.</span></span> <span data-ttu-id="827fd-135">Например, если требуется связь «один к одному» между преподавателем и OfficeAssignment, в которой имеется свойство навигации только для типа инструктора, то для настройки этой связи необходимо использовать API-интерфейс Fluent.</span><span class="sxs-lookup"><span data-stu-id="827fd-135">For example, if you want a one-to-one relationship between Instructor and OfficeAssignment, where you have a navigation property on only the Instructor type, you need to use the fluent API to configure this relationship.</span></span>  

``` csharp
// Configure the primary Key for the OfficeAssignment
modelBuilder.Entity<OfficeAssignment>()
    .HasKey(t => t.InstructorID);

modelBuilder.Entity<Instructor>()
    .HasRequired(t => t.OfficeAssignment)
    .WithRequiredPrincipal();
```  

## <a name="enabling-cascade-delete"></a><span data-ttu-id="827fd-136">Включение каскадного удаления</span><span class="sxs-lookup"><span data-stu-id="827fd-136">Enabling Cascade Delete</span></span>  

<span data-ttu-id="827fd-137">Каскадное удаление для связи можно настроить с помощью метода Виллкаскадеонделете.</span><span class="sxs-lookup"><span data-stu-id="827fd-137">You can configure cascade delete on a relationship by using the WillCascadeOnDelete method.</span></span> <span data-ttu-id="827fd-138">Если внешний ключ в зависимой сущности не допускает значения NULL, то Code First задает каскадное удаление для связи.</span><span class="sxs-lookup"><span data-stu-id="827fd-138">If a foreign key on the dependent entity is not nullable, then Code First sets cascade delete on the relationship.</span></span> <span data-ttu-id="827fd-139">Если внешний ключ в зависимой сущности допускает значение null, Code First не устанавливает каскадное удаление для связи, и при удалении участника внешний ключ будет установлен в значение null.</span><span class="sxs-lookup"><span data-stu-id="827fd-139">If a foreign key on the dependent entity is nullable, Code First does not set cascade delete on the relationship, and when the principal is deleted the foreign key will be set to null.</span></span>  

<span data-ttu-id="827fd-140">Эти правила каскадного удаления можно удалить с помощью:</span><span class="sxs-lookup"><span data-stu-id="827fd-140">You can remove these cascade delete conventions by using:</span></span>  

<span data-ttu-id="827fd-141">modelBuilder. Conventions. Remove \<OneToManyCascadeDeleteConvention\> ()</span><span class="sxs-lookup"><span data-stu-id="827fd-141">modelBuilder.Conventions.Remove\<OneToManyCascadeDeleteConvention\>()</span></span>  
<span data-ttu-id="827fd-142">modelBuilder. Conventions. Remove \<ManyToManyCascadeDeleteConvention\> ()</span><span class="sxs-lookup"><span data-stu-id="827fd-142">modelBuilder.Conventions.Remove\<ManyToManyCascadeDeleteConvention\>()</span></span>  

<span data-ttu-id="827fd-143">Следующий код настраивает требуемую связь, а затем отключает каскадное удаление.</span><span class="sxs-lookup"><span data-stu-id="827fd-143">The following code configures the relationship to be required and then disables cascade delete.</span></span>  

``` csharp
modelBuilder.Entity<Course>()
    .HasRequired(t => t.Department)
    .WithMany(t => t.Courses)
    .HasForeignKey(d => d.DepartmentID)
    .WillCascadeOnDelete(false);
```  

## <a name="configuring-a-composite-foreign-key"></a><span data-ttu-id="827fd-144">Настройка составного внешнего ключа</span><span class="sxs-lookup"><span data-stu-id="827fd-144">Configuring a Composite Foreign Key</span></span>  

<span data-ttu-id="827fd-145">Если первичный ключ в типе Отдела состоит из свойств DepartmentID и Name, то необходимо настроить первичный ключ для отдела и внешний ключ в типах курсов следующим образом:</span><span class="sxs-lookup"><span data-stu-id="827fd-145">If the primary key on the Department type consisted of DepartmentID and Name properties, you would configure the primary key for the Department and the foreign key on the Course types as follows:</span></span>  

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

## <a name="renaming-a-foreign-key-that-is-not-defined-in-the-model"></a><span data-ttu-id="827fd-146">Переименование внешнего ключа, который не определен в модели</span><span class="sxs-lookup"><span data-stu-id="827fd-146">Renaming a Foreign Key That Is Not Defined in the Model</span></span>  

<span data-ttu-id="827fd-147">Если вы решили не определять внешний ключ для типа CLR, но хотите указать, какое имя оно должно иметь в базе данных, выполните следующие действия.</span><span class="sxs-lookup"><span data-stu-id="827fd-147">If you choose not to define a foreign key on the CLR type, but want to specify what name it should have in the database, do the following:</span></span>  

``` csharp
modelBuilder.Entity<Course>()
    .HasRequired(c => c.Department)
    .WithMany(t => t.Courses)
    .Map(m => m.MapKey("ChangedDepartmentID"));
```  

## <a name="configuring-a-foreign-key-name-that-does-not-follow-the-code-first-convention"></a><span data-ttu-id="827fd-148">Настройка имени внешнего ключа, который не соответствует соглашению о Code First</span><span class="sxs-lookup"><span data-stu-id="827fd-148">Configuring a Foreign Key Name That Does Not Follow the Code First Convention</span></span>  

<span data-ttu-id="827fd-149">Если свойство внешнего ключа класса Course называлось Сомедепартментид вместо DepartmentID, необходимо выполнить следующие действия, чтобы указать, что Сомедепартментид должен быть внешним ключом:</span><span class="sxs-lookup"><span data-stu-id="827fd-149">If the foreign key property on the Course class was called SomeDepartmentID instead of DepartmentID, you would need to do the following to specify that you want SomeDepartmentID to be the foreign key:</span></span>  

``` csharp
modelBuilder.Entity<Course>()
         .HasRequired(c => c.Department)
         .WithMany(d => d.Courses)
         .HasForeignKey(c => c.SomeDepartmentID);
```  

## <a name="model-used-in-samples"></a><span data-ttu-id="827fd-150">Модель, используемая в примерах</span><span class="sxs-lookup"><span data-stu-id="827fd-150">Model Used in Samples</span></span>  

<span data-ttu-id="827fd-151">Для примеров на этой странице используется следующая Code First модель.</span><span class="sxs-lookup"><span data-stu-id="827fd-151">The following Code First model is used for the samples on this page.</span></span>  

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
