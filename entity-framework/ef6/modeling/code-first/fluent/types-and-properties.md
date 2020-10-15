---
title: API-интерфейс Fluent — Настройка и сопоставление свойств и типов — EF6
description: API-интерфейс Fluent — Настройка и сопоставление свойств и типов в Entity Framework 6
author: ajcvickers
ms.date: 10/23/2016
uid: ef6/modeling/code-first/fluent/types-and-properties
ms.openlocfilehash: 821672bcb797314c96189443ace7f875a79c8582
ms.sourcegitcommit: 0a25c03fa65ae6e0e0e3f66bac48d59eceb96a5a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/14/2020
ms.locfileid: "92065151"
---
# <a name="fluent-api---configuring-and-mapping-properties-and-types"></a>API-интерфейс Fluent — Настройка и сопоставление свойств и типов
При работе с Entity Framework Code First поведением по умолчанию является привязка классов POCO к таблицам с помощью набора соглашений, помогут в EF. Однако иногда вы не можете следовать этим соглашениям и не хотите сопоставлять сущности с другими условиями.  

Существует два основных способа настройки EF для использования чего-либо, кроме соглашений, а именно [заметок](xref:ef6/modeling/code-first/data-annotations) или API Fluent системы EFS. Заметки охватывают только подмножество функций API Fluent, поэтому существуют сценарии сопоставления, которые не могут быть достигнуты с помощью заметок. В этой статье показано, как использовать API Fluent для настройки свойств.  

Наиболее часто доступ к интерфейсу API для первого интерфейса Fluent, переопределяя метод [OnModelCreating](https://msdn.microsoft.com/library/system.data.entity.dbcontext.onmodelcreating.aspx) на производном [DbContext](https://msdn.microsoft.com/library/system.data.entity.dbcontext.aspx). Следующие примеры предназначены для демонстрации выполнения различных задач с помощью API-интерфейса Fluent и позволяют скопировать код и настроить его в соответствии с моделью. Если вы хотите увидеть модель, которую можно использовать с параметром "как есть", она указана в конце этой статьи.  

## <a name="model-wide-settings"></a>Параметры Model-Wide  

### <a name="default-schema-ef6-onwards"></a>Схема по умолчанию (EF6)  

Начиная с EF6 можно использовать метод Хасдефаултсчема на Дбмоделбуилдер, чтобы указать схему базы данных, которая будет использоваться для всех таблиц, хранимых процедур и т. д. Этот параметр по умолчанию будет переопределен для всех объектов, для которых явно настроена другая схема.  

``` csharp
modelBuilder.HasDefaultSchema("sales");
```  

### <a name="custom-conventions-ef6-onwards"></a>Пользовательские соглашения (EF6)  

Начиная с EF6 можно создавать собственные соглашения для дополнения к Code First. Дополнительные сведения см. в разделе [Custom Code First Conventions](xref:ef6/modeling/code-first/conventions/custom).  

## <a name="property-mapping"></a>Сопоставление свойств  

Метод [Property](https://msdn.microsoft.com/library/system.data.entity.infrastructure.dbentityentry.property.aspx) используется для настройки атрибутов для каждого свойства, принадлежащего сущности или сложному типу. Метод property используется для получения объекта конфигурации для заданного свойства. Параметры объекта конфигурации относятся к настраиваемому типу. Параметр Unicode доступен только для строковых свойств, например.  

### <a name="configuring-a-primary-key"></a>Настройка первичного ключа  

Соглашение Entity Framework для первичных ключей:  

1. Класс определяет свойство, имя которого равно ID или ID.  
2. или имя класса, за которым следует "ID" или "ID"  

Чтобы явно задать свойство как первичный ключ, можно использовать метод HasKey. В следующем примере метод HasKey используется для настройки первичного ключа InstructorID в типе OfficeAssignment.  

``` csharp
modelBuilder.Entity<OfficeAssignment>().HasKey(t => t.InstructorID);
```  

### <a name="configuring-a-composite-primary-key"></a>Настройка составного первичного ключа  

В следующем примере свойства DepartmentID и Name настраиваются как составной первичный ключ типа отдела.  

``` csharp
modelBuilder.Entity<Department>().HasKey(t => new { t.DepartmentID, t.Name });
```  

### <a name="switching-off-identity-for-numeric-primary-keys"></a>Отключение удостоверения для числовых первичных ключей  

В следующем примере свойству DepartmentID присваивается значение System. ComponentModel. аннотаций. Датабасеженератедоптион. None, чтобы указать, что оно не будет создано базой данных.  

``` csharp
modelBuilder.Entity<Department>().Property(t => t.DepartmentID)
    .HasDatabaseGeneratedOption(DatabaseGeneratedOption.None);
```  

### <a name="specifying-the-maximum-length-on-a-property"></a>Указание максимальной длины свойства  

В следующем примере свойство Name должно содержать не более 50 символов. Если сделать значение длиннее 50 символов, вы получите исключение [дбентитивалидатионексцептион](https://msdn.microsoft.com/library/system.data.entity.validation.dbentityvalidationexception.aspx) . Если Code First создает базу данных из этой модели, она также установит максимальную длину столбца имени в 50 символов.  

``` csharp
modelBuilder.Entity<Department>().Property(t => t.Name).HasMaxLength(50);
```  

### <a name="configuring-the-property-to-be-required"></a>Настройка требуемого свойства  

В следующем примере свойство Name является обязательным. Если не указать имя, будет получено исключение Дбентитивалидатионексцептион. Если Code First создает базу данных из этой модели, то столбец, используемый для хранения этого свойства, обычно не допускает значения NULL.  

> [!NOTE]
> В некоторых случаях для столбца в базе данных может оказаться невозможным значение null, даже если свойство является обязательным. Например, при использовании ИЕРАРХИческих данных стратегии наследования для нескольких типов хранится в одной таблице. Если производный тип содержит обязательное свойство, столбец не может быть сделан пустым, так как не все типы в иерархии будут иметь это свойство.  

``` csharp
modelBuilder.Entity<Department>().Property(t => t.Name).IsRequired();
```  

### <a name="configuring-an-index-on-one-or-more-properties"></a>Настройка индекса для одного или нескольких свойств  

> [!NOTE]
> **EF 6.1 только в сторону** — атрибут index появился в Entity Framework 6,1. Если вы используете более раннюю версию, сведения в этом разделе не применяются.  

Создание индексов изначально не поддерживается API-интерфейсом Fluent, но вы можете использовать поддержку **индексаттрибуте** через API Fluent. Атрибуты индекса обрабатываются путем включения заметки модели для модели, которая затем преобразуется в индекс в базе данных позже в конвейере. Эти заметки можно добавить вручную с помощью API-интерфейса Fluent.  

Самый простой способ сделать это — создать экземпляр **индексаттрибуте** , содержащий все параметры для нового индекса. Затем можно создать экземпляр **индексаннотатион** , который является особым типом EF, который преобразует параметры **индексаттрибуте** в заметку модели, которая может храниться в модели EF. Затем их можно передать в метод **хасколумнаннотатион** в API-интерфейсе Fluent, указав **индекс** имени для заметки.  

``` csharp
modelBuilder
    .Entity<Department>()
    .Property(t => t.Name)
    .HasColumnAnnotation("Index", new IndexAnnotation(new IndexAttribute()));
```  

Полный список параметров, доступных в **индексаттрибуте**, см. в разделе *index* статьи [Code First Data Annotations](xref:ef6/modeling/code-first/data-annotations). Это включает в себя настройку имени индекса, создание уникальных индексов и создание индексов с несколькими столбцами.  

Можно указать несколько аннотаций индекса для одного свойства, передав массив **индексаттрибуте** в конструктор **индексаннотатион**.  

``` csharp
modelBuilder
    .Entity<Department>()
    .Property(t => t.Name)
    .HasColumnAnnotation(
        "Index",  
        new IndexAnnotation(new[]
            {
                new IndexAttribute("Index1"),
                new IndexAttribute("Index2") { IsUnique = true }
            })));
```  

### <a name="specifying-not-to-map-a-clr-property-to-a-column-in-the-database"></a>Указание не сопоставлять свойство CLR со столбцом в базе данных  

В следующем примере показано, как указать, что свойство типа CLR не сопоставлено со столбцом в базе данных.  

``` csharp
modelBuilder.Entity<Department>().Ignore(t => t.Budget);
```  

### <a name="mapping-a-clr-property-to-a-specific-column-in-the-database"></a>Сопоставление свойства CLR с конкретным столбцом в базе данных  

В следующем примере показано сопоставление свойства CLR Name со столбцом базы данных Департментнаме.  

``` csharp
modelBuilder.Entity<Department>()
    .Property(t => t.Name)
    .HasColumnName("DepartmentName");
```  

### <a name="renaming-a-foreign-key-that-is-not-defined-in-the-model"></a>Переименование внешнего ключа, который не определен в модели  

Если вы решили не определять внешний ключ для типа CLR, но хотите указать, какое имя оно должно иметь в базе данных, выполните следующие действия.  

``` csharp
modelBuilder.Entity<Course>()
    .HasRequired(c => c.Department)
    .WithMany(t => t.Courses)
    .Map(m => m.MapKey("ChangedDepartmentID"));
```  

### <a name="configuring-whether-a-string-property-supports-unicode-content"></a>Настройка того, поддерживает ли строковое свойство содержимое в Юникоде  

По умолчанию строки задаются в Юникоде (nvarchar в SQL Server). Чтобы указать, что строка должна иметь тип varchar, можно использовать метод Unicode.  

``` csharp
modelBuilder.Entity<Department>()
    .Property(t => t.Name)
    .IsUnicode(false);
```  

### <a name="configuring-the-data-type-of-a-database-column"></a>Настройка типа данных столбца базы данных  

Метод [хасколумнтипе](https://msdn.microsoft.com/library/system.data.entity.modelconfiguration.configuration.stringcolumnconfiguration.hascolumntype.aspx) позволяет сопоставлять различные представления одного и того же базового типа. Использование этого метода не позволяет выполнять преобразование данных во время выполнения. Обратите внимание, что в качестве параметра Columns можно задать значение типа varchar, так как он является независимым от базы данных.  

``` csharp
modelBuilder.Entity<Department>()   
    .Property(p => p.Name)   
    .HasColumnType("varchar");
```  

### <a name="configuring-properties-on-a-complex-type"></a>Настройка свойств сложного типа  

Существует два способа настройки скалярных свойств для сложного типа.  

Свойство можно вызвать в Комплекстипеконфигуратион.  

``` csharp
modelBuilder.ComplexType<Details>()
    .Property(t => t.Location)
    .HasMaxLength(20);
```  

Можно также использовать точечную нотацию для доступа к свойству сложного типа.  

``` csharp
modelBuilder.Entity<OnsiteCourse>()
    .Property(t => t.Details.Location)
    .HasMaxLength(20);
```  

### <a name="configuring-a-property-to-be-used-as-an-optimistic-concurrency-token"></a>Настройка свойства для использования в качестве маркера оптимистичного параллелизма  

Чтобы указать, что свойство в сущности представляет маркер параллелизма, можно использовать либо атрибут ConcurrencyCheck, либо метод Исконкурренцитокен.  

``` csharp
modelBuilder.Entity<OfficeAssignment>()
    .Property(t => t.Timestamp)
    .IsConcurrencyToken();
```  

Можно также использовать метод Исровверсион, чтобы настроить свойство в качестве версии строки в базе данных. Если задать для свойства значение "версия строки", она автоматически настроит его как маркер оптимистичного параллелизма.  

``` csharp
modelBuilder.Entity<OfficeAssignment>()
    .Property(t => t.Timestamp)
    .IsRowVersion();
```  

## <a name="type-mapping"></a>Сопоставление типов  

### <a name="specifying-that-a-class-is-a-complex-type"></a>Указание того, что класс является сложным типом  

По соглашению тип, для которого не задан первичный ключ, рассматривается как сложный тип. Существует несколько сценариев, в которых Code First не может обнаружить сложный тип (например, если у вас есть свойство с именем ID, но это не означает, что он является первичным ключом). В таких случаях следует использовать API Fluent, чтобы явно указать, что тип является сложным типом.  

``` csharp
modelBuilder.ComplexType<Details>();
```  

### <a name="specifying-not-to-map-a-clr-entity-type-to-a-table-in-the-database"></a>Указание не сопоставлять тип сущности CLR с таблицей в базе данных  

В следующем примере показано, как исключить тип CLR из сопоставления с таблицей в базе данных.  

``` csharp
modelBuilder.Ignore<OnlineCourse>();
```  

### <a name="mapping-an-entity-type-to-a-specific-table-in-the-database"></a>Сопоставление типа сущности с определенной таблицей в базе данных  

Все свойства отдела будут сопоставлены со столбцами в таблице с именем t_ Department.  

``` csharp
modelBuilder.Entity<Department>()  
    .ToTable("t_Department");
```  

Также можно указать имя схемы следующим образом:  

``` csharp
modelBuilder.Entity<Department>()  
    .ToTable("t_Department", "school");
```  

### <a name="mapping-the-table-per-hierarchy-tph-inheritance"></a>Сопоставление наследования «одна таблица на иерархию»  

В сценарии сопоставления «одна таблица» все типы в иерархии наследования сопоставлены с одной таблицей. Столбец дискриминатора используется для задания типа каждой строки. При создании модели с помощью Code First ИЕРАРХИя является стратегией по умолчанию для типов, участвующих в иерархии наследования. По умолчанию столбец дискриминатора добавляется в таблицу с именем "дискриминатор", а имя типа CLR каждого типа в иерархии используется для значений дискриминатора. Поведение по умолчанию можно изменить с помощью API-интерфейса Fluent.  

``` csharp
modelBuilder.Entity<Course>()  
    .Map<Course>(m => m.Requires("Type").HasValue("Course"))  
    .Map<OnsiteCourse>(m => m.Requires("Type").HasValue("OnsiteCourse"));
```  

### <a name="mapping-the-table-per-type-tpt-inheritance"></a>Сопоставление наследования типа «таблица на тип» (TPT)  

В сценарии сопоставления TPT все типы сопоставлены с отдельными таблицами. Как свойства базового типа, так и свойства производного типа хранятся в таблице, сопоставленной с этим типом. Таблицы, которые сопоставляются с производными типами, также хранят внешний ключ, который соединяет производную таблицу с базовой таблицей.  

``` csharp
modelBuilder.Entity<Course>().ToTable("Course");  
modelBuilder.Entity<OnsiteCourse>().ToTable("OnsiteCourse");
```  

### <a name="mapping-the-table-per-concrete-class-tpc-inheritance"></a>Сопоставление наследования класса "Таблица — конкретный класс" (TPC)  

В сценарии сопоставления TPC все неабстрактные типы в иерархии сопоставляются с отдельными таблицами. Таблицы, сопоставленные с производными классами, не имеют связи с таблицей, сопоставленной с базовым классом в базе данных. Все свойства класса, включая унаследованные свойства, сопоставляются со столбцами соответствующей таблицы.  

Вызовите метод Мапинхеритедпропертиес, чтобы настроить каждый производный тип. Мапинхеритедпропертиес повторно сопоставляет все свойства, унаследованные от базового класса, с новыми столбцами в таблице для производного класса.  

> [!NOTE]
> Обратите внимание, что поскольку таблицы, участвующие в иерархии наследования TPC, не используют первичный ключ, при вставке в таблицы, сопоставленные с подклассами, будут использоваться дубликаты ключей сущностей, если созданные базой данных значения имеют одинаковое начальное значение идентификатора. Чтобы решить эту проблему, можно указать другое начальное начальное значение для каждой таблицы или отключить удостоверение в свойстве первичного ключа. Identity — это значение по умолчанию для целочисленных свойств ключа при работе с Code First.  

``` csharp
modelBuilder.Entity<Course>()
    .Property(c => c.CourseID)
    .HasDatabaseGeneratedOption(DatabaseGeneratedOption.None);

modelBuilder.Entity<OnsiteCourse>().Map(m =>
{
    m.MapInheritedProperties();
    m.ToTable("OnsiteCourse");
});

modelBuilder.Entity<OnlineCourse>().Map(m =>
{
    m.MapInheritedProperties();
    m.ToTable("OnlineCourse");
});
```  

### <a name="mapping-properties-of-an-entity-type-to-multiple-tables-in-the-database-entity-splitting"></a>Сопоставление свойств типа сущности с несколькими таблицами в базе данных (разбиение сущностей)  

Разбиение сущностей позволяет распределять свойства типа сущности по нескольким таблицам. В следующем примере сущность отделов разбивается на две таблицы: Department и Департментдетаилс. При разбиении сущностей используется несколько вызовов метода Map для соответствия подмножества свойств определенной таблице.  

``` csharp
modelBuilder.Entity<Department>()
    .Map(m =>
    {
        m.Properties(t => new { t.DepartmentID, t.Name });
        m.ToTable("Department");
    })
    .Map(m =>
    {
        m.Properties(t => new { t.DepartmentID, t.Administrator, t.StartDate, t.Budget });
        m.ToTable("DepartmentDetails");
    });
```  

### <a name="mapping-multiple-entity-types-to-one-table-in-the-database-table-splitting"></a>Сопоставление нескольких типов сущностей с одной таблицей в базе данных (разделение таблицы)  

В следующем примере сопоставляются два типа сущностей, которые совместно используют первичный ключ с одной таблицей.  

``` csharp
modelBuilder.Entity<OfficeAssignment>()
    .HasKey(t => t.InstructorID);

modelBuilder.Entity<Instructor>()
    .HasRequired(t => t.OfficeAssignment)
    .WithRequiredPrincipal(t => t.Instructor);

modelBuilder.Entity<Instructor>().ToTable("Instructor");

modelBuilder.Entity<OfficeAssignment>().ToTable("Instructor");
```  

### <a name="mapping-an-entity-type-to-insertupdatedelete-stored-procedures-ef6-onwards"></a>Сопоставление типа сущности для вставки, обновления и удаления хранимых процедур (EF6)  

Начиная с EF6 можно сопоставлять сущности для использования хранимых процедур для вставки обновлений и удаления. Дополнительные сведения см. в разделе [Code First вставки, обновления и удаления хранимых процедур](xref:ef6/modeling/code-first/fluent/cud-stored-procedures).  

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
