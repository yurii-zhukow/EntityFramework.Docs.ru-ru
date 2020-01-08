---
title: Fluent API - связей — EF6
author: divega
ms.date: 10/23/2016
ms.assetid: fd73b4f8-16d5-40f1-9640-885ceafe67a1
ms.openlocfilehash: 05f282c02699f8bf3c71197ac5e01000f1855917
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/13/2018
ms.locfileid: "45490471"
---
# <a name="fluent-api---relationships"></a>Fluent API - связи
> [!NOTE]
> Эта страница содержит сведения о настройке связи в модели Code First с помощью текучего API. Общие сведения о связях в EF и способах доступа к данных и управления ими с помощью связей см. в разделе [связей и свойств навигации](~/ef6/fundamentals/relationships.md).  

При работе в режиме Code First, можно определить модель, определение классов CLR вашего домена. По умолчанию Entity Framework использует соглашения Code First для сопоставления классов со схемой базы данных. При использовании Code First соглашения об именовании, в большинстве случаев можно положиться на Code First, устанавливать отношения между таблицами на основе внешних ключей и свойства навигации, которые определяются в классах. Если не использовать соглашения при определении классов, если вы хотите изменить способ правила работают, можно использовать fluent API или заметки к данным для настройки классов, поэтому Code First можно сопоставить связи между таблицами.  

## <a name="introduction"></a>Вступление  

При настройке связи с помощью текучего API, сначала с экземпляром EntityTypeConfiguration и затем используется метод HasRequired, HasOptional или HasMany для указания типа отношения, в которых участвует эта сущность. Методы HasRequired и HasOptional принимают лямбда-выражение, представляющее свойство навигации ссылки. Метод HasMany принимает лямбда-выражение, представляющее свойство навигации по коллекции. Свойство обратной навигации, которое затем можно настроить с помощью методов WithRequired WithOptional и WithMany. Эти методы имеют перегрузки, которые не принимают аргументы и может использоваться для указания количества элементов с однонаправленным переходов.  

Затем можно настроить свойства внешнего ключа с помощью метода HasForeignKey. Этот метод принимает лямбда-выражение, представляющее свойство для использования в качестве внешнего ключа.  

## <a name="configuring-a-required-to-optional-relationship-one-tozero-or-one"></a>Настройка необходимых к дополнительную связь (к нулю или одному)  

В следующем примере настраивается отношение один к нулю или одному. OfficeAssignment имеет InstructorID свойства, которое является первичным ключом и внешнего ключа, так как имя свойства не соответствует соглашение, которое метод HasKey используется для настройки первичного ключа.  

``` csharp
// Configure the primary key for the OfficeAssignment
modelBuilder.Entity<OfficeAssignment>()
    .HasKey(t => t.InstructorID);

// Map one-to-zero or one relationship
modelBuilder.Entity<OfficeAssignment>()
    .HasRequired(t => t.Instructor)
    .WithOptional(t => t.OfficeAssignment);
```  

## <a name="configuring-a-relationship-where-both-ends-are-required-one-to-one"></a>Настройка связи, где требуются (один к одному) обоих концах  

В большинстве случаев Entity Framework может определить, какой тип является зависимым и который является основным для связи. Тем не менее при оба конца связи являются обязательными или обеих сторон необязательно Entity Framework не может идентифицировать зависимым и участника. Если требуются оба конца связи, используйте WithRequiredPrincipal или WithRequiredDependent после метода HasRequired. Когда оба конца связи являются необязательными, используйте WithOptionalPrincipal или WithOptionalDependent после метода HasOptional.  

``` csharp
// Configure the primary key for the OfficeAssignment
modelBuilder.Entity<OfficeAssignment>()
    .HasKey(t => t.InstructorID);

modelBuilder.Entity<Instructor>()
    .HasRequired(t => t.OfficeAssignment)
    .WithRequiredPrincipal(t => t.Instructor);
```  

## <a name="configuring-a-many-to-many-relationship"></a>Настройка связи многие ко многим  

Приведенный ниже код настраивает отношение "многие ко многим" между типами Course и Instructor. В следующем примере Чтобы создать таблицу соединения используются соглашения Code First по умолчанию. В результате создается таблица CourseInstructor со столбцами Course_CourseID и Instructor_InstructorID.  

``` csharp
modelBuilder.Entity<Course>()
    .HasMany(t => t.Instructors)
    .WithMany(t => t.Courses)
```  

Если вы хотите указать имя таблицы соединения и имена столбцов в таблице, необходимо выполнить дополнительную настройку с помощью метода карты. Следующий код приводит к возникновению ошибки CourseInstructor таблицу со столбцами, CourseID и InstructorID.  

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

## <a name="configuring-a-relationship-with-one-navigation-property"></a>Настройка связи с одним свойством навигации  

Однонаправленное (также называемый однонаправленные) связи выполняется во время определения только на одном из конечных точках, а не на оба свойства навигации. По соглашению Code First всегда интерпретирует Однонаправленная связь как один ко многим. Например если вы хотите, чтобы однозначное соответствие между Instructor и OfficeAssignment, при наличии свойства навигации на только тип данных Instructor, необходимо использовать текучий API для настройки этой связи.  

``` csharp
// Configure the primary Key for the OfficeAssignment
modelBuilder.Entity<OfficeAssignment>()
    .HasKey(t => t.InstructorID);

modelBuilder.Entity<Instructor>()
    .HasRequired(t => t.OfficeAssignment)
    .WithRequiredPrincipal();
```  

## <a name="enabling-cascade-delete"></a>Включение каскадное удаление  

Можно настроить каскадное удаление для связи с помощью метода WillCascadeOnDelete. Если внешний ключ для зависимой сущности не допускает значения NULL, то Code First устанавливает каскадное удаление в отношении. Если внешний ключ для зависимой сущности допускает значения NULL, Code First не устанавливает каскадное удаление для связи, и при удалении участника внешний ключ устанавливается в значение null.  

Эти правила cascade delete можно удалить с помощью:  

modelBuilder.Conventions.Remove\<OneToManyCascadeDeleteConvention\>)  
modelBuilder.Conventions.Remove\<ManyToManyCascadeDeleteConvention\>)  

Следующий код настраивает требуемую связь, а затем отключает каскадное удаление.  

``` csharp
modelBuilder.Entity<Course>()
    .HasRequired(t => t.Department)
    .WithMany(t => t.Courses)
    .HasForeignKey(d => d.DepartmentID)
    .WillCascadeOnDelete(false);
```  

## <a name="configuring-a-composite-foreign-key"></a>Настройка составного внешнего ключа  

Если первичный ключ типа отдела состоял из свойств DepartmentID и Name, вы бы настроить первичный ключ для подразделения и внешнего ключа типов курсов следующим образом:  

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

## <a name="renaming-a-foreign-key-that-is-not-defined-in-the-model"></a>Переименование внешний ключ, который не определен в модели  

Если вы не захотите определять внешний ключ для типа CLR, но решите задать его имя в базе данных, сделайте следующее:  

``` csharp
modelBuilder.Entity<Course>()
    .HasRequired(c => c.Department)
    .WithMany(t => t.Courses)
    .Map(m => m.MapKey("ChangedDepartmentID"));
```  

## <a name="configuring-a-foreign-key-name-that-does-not-follow-the-code-first-convention"></a>Настройка имя внешнего ключа, который является исключением из соглашения первого кода  

Если свойства внешнего ключа в классе курс называется SomeDepartmentID вместо DepartmentID, необходимо выполнить следующую команду, чтобы указать, что SomeDepartmentID в качестве внешнего ключа:  

``` csharp
modelBuilder.Entity<Course>()
         .HasRequired(c => c.Department)
         .WithMany(d => d.Courses)
         .HasForeignKey(c => c.SomeDepartmentID);
```  

## <a name="model-used-in-samples"></a>Модели, используемые в примерах  

Для примеров на этой странице используется следующая модель Code First.  

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
