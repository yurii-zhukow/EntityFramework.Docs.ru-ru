---
title: Интерфейсы API Fluent — взаимосвязи — EF6
description: Связи API Fluent в Entity Framework 6
author: divega
ms.date: 10/23/2016
ms.assetid: fd73b4f8-16d5-40f1-9640-885ceafe67a1
uid: ef6/modeling/code-first/fluent/relationships
ms.openlocfilehash: 8cc56f7341df6da7f60f649308ea7042ef23b537
ms.sourcegitcommit: 7c3939504bb9da3f46bea3443638b808c04227c2
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/09/2020
ms.locfileid: "89617001"
---
# <a name="fluent-api---relationships"></a>Взаимосвязи API Fluent
> [!NOTE]
> На этой странице содержатся сведения о настройке связей в модели Code First с помощью API Fluent. Общие сведения о связях в EF и доступе к данным и управлении ими с помощью связей см. в разделе [связи & свойства навигации](xref:ef6/fundamentals/relationships).  

При работе с Code First вы определяете модель, определяя классы CLR домена. По умолчанию Entity Framework использует соглашения Code First, чтобы сопоставлять классы с схемой базы данных. Если используются соглашения об именовании Code First, в большинстве случаев можно полагаться на Code First, чтобы настроить связи между таблицами на основе внешних ключей и свойств навигации, определенных в классах. Если вы не соответствуете соглашениям при определении классов или хотите изменить способ работы соглашений, можно использовать API-интерфейс Fluent или заметки к данным для настройки классов таким образом, чтобы Code First могли сопоставлять связи между таблицами.  

## <a name="introduction"></a>Введение  

При настройке связи с API Fluent необходимо начать с экземпляра Ентититипеконфигуратион, а затем использовать метод Хасрекуиред, Хасоптионал или Хасмани для указания типа связи, в которой участвует эта сущность. Методы Хасрекуиред и Хасоптионал принимают лямбда-выражение, представляющее свойство навигации ссылки. Метод Хасмани принимает лямбда-выражение, представляющее свойство навигации коллекции. Затем можно настроить обратное свойство навигации с помощью методов Висрекуиред, Висоптионал и Висмани. Эти методы имеют перегрузки, которые не принимают аргументы и могут использоваться для задания количества элементов с однонаправленными переходами.  

Затем можно настроить свойства внешнего ключа с помощью метода Хасфореигнкэй. Этот метод принимает лямбда-выражение, представляющее свойство, которое будет использоваться в качестве внешнего ключа.  

## <a name="configuring-a-required-to-optional-relationship-one-tozero-or-one"></a>Настройка связи «обязательно-ко-многим» (один к одному — ноль или один)  

В следующем примере настраивается связь «один к нулю» или «одна-одна». OfficeAssignment имеет свойство InstructorID, которое является первичным ключом и внешним ключом, поскольку имя свойства не соответствует соглашению, для настройки первичного ключа используется метод HasKey.  

``` csharp
// Configure the primary key for the OfficeAssignment
modelBuilder.Entity<OfficeAssignment>()
    .HasKey(t => t.InstructorID);

// Map one-to-zero or one relationship
modelBuilder.Entity<OfficeAssignment>()
    .HasRequired(t => t.Instructor)
    .WithOptional(t => t.OfficeAssignment);
```  

## <a name="configuring-a-relationship-where-both-ends-are-required-one-to-one"></a>Настройка связи, где требуются оба конца (один к одному)  

В большинстве случаев Entity Framework может определить, какой из типов является зависимым и является участником в связи. Однако, если требуются оба конца связи, или обе стороны являются необязательными Entity Framework не может опознать зависимый и основной субъект. Если требуются оба конца связи, используйте ВисрекуиредпринЦипал или Висрекуиреддепендент после метода Хасрекуиред. Если оба конца связи являются необязательными, используйте ВисоптионалпринЦипал или Висоптионалдепендент после метода Хасоптионал.  

``` csharp
// Configure the primary key for the OfficeAssignment
modelBuilder.Entity<OfficeAssignment>()
    .HasKey(t => t.InstructorID);

modelBuilder.Entity<Instructor>()
    .HasRequired(t => t.OfficeAssignment)
    .WithRequiredPrincipal(t => t.Instructor);
```  

## <a name="configuring-a-many-to-many-relationship"></a>Настройка связи «многие ко многим»  

Следующий код настраивает связь «многие ко многим» между типами курсов и преподавателей. В следующем примере для создания таблицы соединений используются соглашения Code First по умолчанию. В результате таблица Каурсеинструктор создается с Course_CourseID и Instructor_InstructorID столбцами.  

``` csharp
modelBuilder.Entity<Course>()
    .HasMany(t => t.Instructors)
    .WithMany(t => t.Courses)
```  

Если необходимо указать имя таблицы соединений и имена столбцов в таблице, необходимо выполнить дополнительную настройку с помощью метода Map. Следующий код создает таблицу Каурсеинструктор со столбцами CourseID и InstructorID.  

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

Однонаправленная (также называемая однонаправленной) связь — это то, что свойство навигации определено только для одного из сторон связи, а не для обоих. По соглашению Code First всегда интерпретирует однонаправленную связь как "один ко многим". Например, если требуется связь «один к одному» между преподавателем и OfficeAssignment, в которой имеется свойство навигации только для типа инструктора, то для настройки этой связи необходимо использовать API-интерфейс Fluent.  

``` csharp
// Configure the primary Key for the OfficeAssignment
modelBuilder.Entity<OfficeAssignment>()
    .HasKey(t => t.InstructorID);

modelBuilder.Entity<Instructor>()
    .HasRequired(t => t.OfficeAssignment)
    .WithRequiredPrincipal();
```  

## <a name="enabling-cascade-delete"></a>Включение каскадного удаления  

Каскадное удаление для связи можно настроить с помощью метода Виллкаскадеонделете. Если внешний ключ в зависимой сущности не допускает значения NULL, то Code First задает каскадное удаление для связи. Если внешний ключ в зависимой сущности допускает значение null, Code First не устанавливает каскадное удаление для связи, и при удалении участника внешний ключ будет установлен в значение null.  

Эти правила каскадного удаления можно удалить с помощью:  

modelBuilder. Conventions. Remove \<OneToManyCascadeDeleteConvention\> ()  
modelBuilder. Conventions. Remove \<ManyToManyCascadeDeleteConvention\> ()  

Следующий код настраивает требуемую связь, а затем отключает каскадное удаление.  

``` csharp
modelBuilder.Entity<Course>()
    .HasRequired(t => t.Department)
    .WithMany(t => t.Courses)
    .HasForeignKey(d => d.DepartmentID)
    .WillCascadeOnDelete(false);
```  

## <a name="configuring-a-composite-foreign-key"></a>Настройка составного внешнего ключа  

Если первичный ключ в типе Отдела состоит из свойств DepartmentID и Name, то необходимо настроить первичный ключ для отдела и внешний ключ в типах курсов следующим образом:  

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

## <a name="renaming-a-foreign-key-that-is-not-defined-in-the-model"></a>Переименование внешнего ключа, который не определен в модели  

Если вы решили не определять внешний ключ для типа CLR, но хотите указать, какое имя оно должно иметь в базе данных, выполните следующие действия.  

``` csharp
modelBuilder.Entity<Course>()
    .HasRequired(c => c.Department)
    .WithMany(t => t.Courses)
    .Map(m => m.MapKey("ChangedDepartmentID"));
```  

## <a name="configuring-a-foreign-key-name-that-does-not-follow-the-code-first-convention"></a>Настройка имени внешнего ключа, который не соответствует соглашению о Code First  

Если свойство внешнего ключа класса Course называлось Сомедепартментид вместо DepartmentID, необходимо выполнить следующие действия, чтобы указать, что Сомедепартментид должен быть внешним ключом:  

``` csharp
modelBuilder.Entity<Course>()
         .HasRequired(c => c.Department)
         .WithMany(d => d.Courses)
         .HasForeignKey(c => c.SomeDepartmentID);
```  

## <a name="model-used-in-samples"></a>Модель, используемая в примерах  

Для примеров на этой странице используется следующая Code First модель.  

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
