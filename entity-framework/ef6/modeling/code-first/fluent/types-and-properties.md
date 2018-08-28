---
title: Fluent API — Настройка и сопоставление типов и свойств — EF6
author: divega
ms.date: 2016-10-23
ms.assetid: 648ed274-c501-4630-88e0-d728ab5c4057
ms.openlocfilehash: e65a3f4721e5c28de63d143e1143f3584e145477
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/27/2018
ms.locfileid: "42996991"
---
# <a name="fluent-api---configuring-and-mapping-properties-and-types"></a>Fluent API — Настройка и сопоставление типов и свойств
При работе с Entity Framework Code First поведение по умолчанию — классы POCO сопоставляются с таблицами, используя набор соглашений, помещенного в EF. Иногда тем не менее, нельзя или нежелательно выполните эти соглашения и нужно сопоставить сущности с отличные от контекстам.  

Существует два основных способа, можно настроить EF использовать отличные от соглашений, а именно [заметки](~/ef6/modeling/code-first/data-annotations.md) или текучего API файловой системы EFs. Заметки только охватывает часть возможностей, текучего API, поэтому существуют сценарии сопоставления, которые невозможно сделать с помощью заметок. Эта статья предназначена для демонстрации использовать текучий API для настройки свойств.  

Fluent API для code first чаще всего осуществляется путем переопределения [OnModelCreating](https://msdn.microsoft.com/library/system.data.entity.dbcontext.onmodelcreating.aspx) метод в производном [DbContext](https://msdn.microsoft.com/library/system.data.entity.dbcontext.aspx). Приведенные ниже примеры предназначены для показано, как выполнять различные задачи с помощью текучего api и скопируйте код и настроить его в соответствии с вашей модели, если вы хотите увидеть модель, они могут использоваться в качестве-то он предоставляется в конце этой статьи.  

## <a name="model-wide-settings"></a>Параметры на уровне модели  

### <a name="default-schema-ef6-onwards"></a>Схема по умолчанию (для EF6 и выше)  

Начиная с EF6 можно использовать HasDefaultSchema метод на DbModelBuilder для указания схемы базы данных для таблиц, хранимых процедур, и т.д. Этот параметр по умолчанию будет переопределен для любых объектов, которые явным образом настроить другую схему для.  

``` csharp
modelBuilder.HasDefaultSchema(“sales”);
```  

### <a name="custom-conventions-ef6-onwards"></a>Соглашения об именовании (для EF6 и выше)  

Начиная с EF6, можно создать собственные правила, чтобы дополнить тех, которые включены в Code First. Дополнительные сведения см. в разделе [первый соглашения о написании пользовательского кода](~/ef6/modeling/code-first/conventions/custom.md).  

## <a name="property-mapping"></a>Сопоставление свойств  

[Свойство](https://msdn.microsoft.com/library/system.data.entity.infrastructure.dbentityentry.property.aspx) метод используется для настройки атрибутов для каждого свойства, относящиеся к сущности или сложного типа. Метод свойства используется для получения объекта конфигурации для заданного свойства. Параметры в объекте конфигурации используются только в настраиваемый; тип Например IsUnicode доступны только для свойства строки.  

### <a name="configuring-a-primary-key"></a>Настройка первичного ключа  

Действует соглашение, Entity Framework для первичных ключей:  

1. Ваш класс определяет свойство, имя которого является «ID» или «Id»  
2. или имя класса, за которым следует «ID» или «Id»  

Чтобы явно задать свойство в качестве первичного ключа, можно использовать метод HasKey. В следующем примере метод HasKey используется для настройки InstructorID первичный ключ типа OfficeAssignment.  

``` csharp
modelBuilder.Entity<OfficeAssignment>().HasKey(t => t.InstructorID);
```  

### <a name="configuring-a-composite-primary-key"></a>Настройка составного первичного ключа  

В следующем примере настраивается свойств DepartmentID и имя составного первичного ключа типа отдела.  

``` csharp
modelBuilder.Entity<Department>().HasKey(t => new { t.DepartmentID, t.Name });
```  

### <a name="switching-off-identity-for-numeric-primary-keys"></a>Выключение идентификации для числовых первичных ключей  

В следующем примере свойство DepartmentID для System.ComponentModel.DataAnnotations.DatabaseGeneratedOption.None, чтобы указать, что значение не создается в базе данных.  

``` csharp
modelBuilder.Entity<Department>().Property(t => t.DepartmentID)
    .HasDatabaseGeneratedOption(DatabaseGeneratedOption.None);
```  

### <a name="specifying-the-maximum-length-on-a-property"></a>Указание Максимальная длина свойства  

В следующем примере свойство Name должно быть не более 50 символов. Если значение длиннее 50 символов, вы получите [DbEntityValidationException](https://msdn.microsoft.com/library/system.data.entity.validation.dbentityvalidationexception.aspx) исключение. Если Code First создает базу данных из этой модели она будет также задан Максимальная длина имени столбца до 50 символов.  

``` csharp
modelBuilder.Entity<Department>().Property(t => t.Name).HasMaxLength(50);
```  

### <a name="configuring-the-property-to-be-required"></a>Настройка свойства обязательным  

В следующем примере свойство Name является обязательным. Если вы не укажете имя, вы получите исключение DbEntityValidationException. Если Code First создает базу данных из этой модели выберите столбец, используемый для хранения этого свойства обычно будет не допускающие значения NULL.  

> [!NOTE]
> В некоторых случаях может оказаться невозможным для столбца в базе данных и не допускало, несмотря на то, что свойство является обязательным. Например, когда с помощью данных стратегии наследование TPH для нескольких типов хранится в одной таблице. Если производный тип содержит свойство столбца невозможно не допускающие значения NULL, так как не все типы в иерархии, это свойство будет задано.  

``` csharp
modelBuilder.Entity<Department>().Property(t => t.Name).IsRequired();
```  

### <a name="configuring-an-index-on-one-or-more-properties"></a>Настройка индекса для одного или нескольких свойств  

> [!NOTE]
> **EF6.1 и более поздних версий только** -индекс, представляющий атрибут появился в Entity Framework 6.1. При использовании более ранней версии сведения в этом разделе не применяется.  

Создание индексов не поддерживается в собственном коде Fluent API-интерфейсом, но чтобы использовать поддержку **IndexAttribute** через Fluent API. Атрибуты индекса обрабатываются путем включения заметку в виде модели на модель, которая затем преобразуется в индекс в базе данных далее в конвейере. Можно вручную добавить эти же заметок с помощью Fluent API.  

Для этого проще всего создать экземпляр **IndexAttribute** , содержащий все параметры для нового индекса. Затем можно создать экземпляр **IndexAnnotation** это определенного типа EF, которое преобразует **IndexAttribute** параметры в заметку в виде модели, которые могут быть сохранены на модели EF. Это может быть передан **HasColumnAnnotation** метод Fluent API-интерфейсы, указав имя **индекс** для заметки.  

``` csharp
modelBuilder
    .Entity<Department>()
    .Property(t => t.Name)
    .HasColumnAnnotation("Index", new IndexAnnotation(new IndexAttribute()));
```  

Полный список параметров, доступных в **IndexAttribute**, см. в разделе *индекс* раздел [заметки данных Code First](~/ef6/modeling/code-first/data-annotations.md). Сюда входят Настройка имя индекса, создание уникальных индексов и создания индексы с несколькими столбцами.  

Можно указать несколько заметок индекса по одному свойству, передавая ему массив **IndexAttribute** конструктору **IndexAnnotation**.  

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

### <a name="specifying-not-to-map-a-clr-property-to-a-column-in-the-database"></a>Указание не для сопоставления свойства CLR со столбцом в базе данных  

Приведенный ниже показано, как указать, что свойство на основе типа CLR не сопоставлен со столбцом в базе данных.  

``` csharp
modelBuilder.Entity<Department>().Ignore(t => t.Budget);
```  

### <a name="mapping-a-clr-property-to-a-specific-column-in-the-database"></a>Сопоставление свойства CLR для заданного столбца в базе данных  

Следующий пример сопоставляет свойство CLR имя столбца базы данных «отдел».  

``` csharp
modelBuilder.Entity<Department>()
    .Property(t => t.Name)
    .HasColumnName("DepartmentName");
```  

### <a name="renaming-a-foreign-key-that-is-not-defined-in-the-model"></a>Переименование внешний ключ, который не определен в модели  

Если вы не захотите определения внешнего ключа на основе типа CLR, но, чтобы задать имя, она должна иметь в базе данных, сделайте следующее:  

``` csharp
modelBuilder.Entity<Course>()
    .HasRequired(c => c.Department)
    .WithMany(t => t.Courses)
    .Map(m => m.MapKey("ChangedDepartmentID"));
```  

### <a name="configuring-whether-a-string-property-supports-unicode-content"></a>Настройка ли строковое свойство поддерживать содержимое в Юникоде  

По умолчанию строки являются Юникода (nvarchar в SQL Server). Чтобы указать, что строка должна иметь тип varchar, можно использовать метод IsUnicode.  

``` csharp
modelBuilder.Entity<Department>()
    .Property(t => t.Name)
    .IsUnicode(false);
```  

### <a name="configuring-the-data-type-of-a-database-column"></a>Настройка типа данных столбца базы данных  

[HasColumnType](https://msdn.microsoft.com/library/system.data.entity.modelconfiguration.configuration.stringcolumnconfiguration.hascolumntype.aspx) метод, который позволяет сопоставление с различными представлениями один и тот же базовый тип. С помощью этого метода не позволяют выполнять любые преобразования данных во время выполнения. Обратите внимание на то, что IsUnicode является предпочтительным способом настройки столбцов varchar, так как он зависит от базы данных.  

``` csharp
modelBuilder.Entity<Department>()   
    .Property(p => p.Name)   
    .HasColumnType("varchar");
```  

### <a name="configuring-properties-on-a-complex-type"></a>Настройка свойств для сложного типа  

Существует два способа настройки скалярные свойства сложного типа.  

ComplexTypeConfiguration можно вызвать свойство.  

``` csharp
modelBuilder.ComplexType<Details>()
    .Property(t => t.Location)
    .HasMaxLength(20);
```  

Можно также использовать нотацию для доступа к свойству сложного типа.  

``` csharp
modelBuilder.Entity<OnsiteCourse>()
    .Property(t => t.Details.Location)
    .HasMaxLength(20);
```  

### <a name="configuring-a-property-to-be-used-as-an-optimistic-concurrency-token"></a>Настройка свойств для использования в качестве маркера оптимистичного параллелизма  

Чтобы указать, что свойство в сущности представляет маркер параллелизма, можно использовать атрибут ConcurrencyCheck или метод IsConcurrencyToken.  

``` csharp
modelBuilder.Entity<OfficeAssignment>()
    .Property(t => t.Timestamp)
    .IsConcurrencyToken();
```  

Можно также использовать метод IsRowVersion свойство может быть версию строки в базе данных. Задание свойства следует убедиться, что версия строки автоматически настраивает его, чтобы быть маркера оптимистичного параллелизма.  

``` csharp
modelBuilder.Entity<OfficeAssignment>()
    .Property(t => t.Timestamp)
    .IsRowVersion();
```  

## <a name="type-mapping"></a>Сопоставление типов  

### <a name="specifying-that-a-class-is-a-complex-type"></a>Указывается, что класс является сложным типом  

По соглашению типом, который не имеет первичного ключа указан рассматривается как сложный тип. Существуют некоторые сценарии, где Code First не обнаружит сложного типа (например, если у вас есть свойство с именем идентификатор, но не значит, для того, чтобы быть первичным ключом). В таких случаях используется fluent API для явного указания, что тип является сложным типом.  

``` csharp
modelBuilder.ComplexType<Details>();
```  

### <a name="specifying-not-to-map-a-clr-entity-type-to-a-table-in-the-database"></a>Указание не для того, чтобы сопоставить тип CLR сущности в таблицу в базе данных  

Приведенный ниже показано, как исключение из таблицы в базе данных, сопоставляемого типа CLR.  

``` csharp
modelBuilder.Ignore<OnlineCourse>();
```  

### <a name="mapping-an-entity-type-to-a-specific-table-in-the-database"></a>Сопоставление типа сущности в конкретную таблицу в базе данных  

Все свойства отдела будет сопоставлено со столбцами в таблице, именуемой t_ отдела.  

``` csharp
modelBuilder.Entity<Department>()  
    .ToTable("t_Department");
```  

Можно также указать имя схемы следующим образом:  

``` csharp
modelBuilder.Entity<Department>()  
    .ToTable("t_Department", "school");
```  

### <a name="mapping-the-table-per-hierarchy-tph-inheritance"></a>Сопоставление наследования одна таблица на иерархию (TPH)  

В случае сопоставления TPH всех типов в иерархии наследования сопоставляются с одной таблицей. Столбец дискриминатора используется для определения типа каждой строки. При создании модели с помощью Code First, TPH — это стратегия по умолчанию для типов, которые участвуют в иерархии наследования. По умолчанию столбец дискриминатора добавляется в таблицу с именем «Дискриминатора» и имя типа CLR каждого типа в иерархии используется для значения дискриминатора. Поведение по умолчанию можно изменить с помощью текучего API.  

``` csharp
modelBuilder.Entity<Course>()  
    .Map<Course>(m => m.Requires("Type").HasValue("Course"))  
    .Map<OnsiteCourse>(m => m.Requires("Type").HasValue("OnsiteCourse"));
```  

### <a name="mapping-the-table-per-type-tpt-inheritance"></a>Сопоставление наследования одна таблица на тип (TPT)  

В сценарии сопоставления TPT все типы сопоставляются с отдельными таблицами. Как свойства базового типа, так и свойства производного типа хранятся в таблице, сопоставленной с этим типом. Таблицы, которые сопоставляются производные типы также хранить внешний ключ, который соединяет производной таблицы с базовой таблицей.  

``` csharp
modelBuilder.Entity<Course>().ToTable("Course");  
modelBuilder.Entity<OnsiteCourse>().ToTable("OnsiteCourse");
```  

### <a name="mapping-the-table-per-concrete-class-tpc-inheritance"></a>Сопоставление наследования таблица на конкретный класс (TPC)  

В сценарии сопоставления TPC все не являющиеся абстрактными типы в иерархии сопоставляются с отдельными таблицами. Таблицы, которые сопоставляются производные классы имеют связана ни таблицей, которая сопоставляется с базовым классом в базе данных. Все свойства класса, включая унаследованные, сопоставляются столбцы соответствующей таблицы.  

Вызовите метод MapInheritedProperties для настройки каждого производного типа. MapInheritedProperties повторно отображает все свойства, унаследованные от базового класса в новые столбцы в таблице для производного класса.  

> [!NOTE]
> Обратите внимание, что так как не используют совместно таблиц, участвующих в иерархии наследования TPC первичного ключа ключи повторяющиеся сущности при вставке в таблицах, сопоставленных с подклассами, если сформированный базой данных значения с тем же начальное значение идентификатора. Чтобы решить эту проблему, можно указать разные начальное значение для каждой таблицы или отключить удостоверения на свойство первичного ключа. Удостоверение значение по умолчанию для ключевых свойств целое число, при работе в режиме Code First.  

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

Разбиение сущностей позволяет свойства типа сущности и распределяются по нескольким таблицам. В следующем примере сущность Department разбивается на две таблицы: отдела и DepartmentDetails. Разделение сущностей использует несколько вызовов метода карты для сопоставления подмножества свойств в конкретную таблицу.  

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

### <a name="mapping-multiple-entity-types-to-one-table-in-the-database-table-splitting"></a>Сопоставление нескольких типов сущностей к одной таблице в базе данных (разбиение таблиц)  

Следующий пример сопоставляет двумя типами сущностей, которые совместно используют первичный ключ для одной таблицы.  

``` csharp
modelBuilder.Entity<OfficeAssignment>()
    .HasKey(t => t.InstructorID);

modelBuilder.Entity<Instructor>()
    .HasRequired(t => t.OfficeAssignment)
    .WithRequiredPrincipal(t => t.Instructor);

modelBuilder.Entity<Instructor>().ToTable("Instructor");

modelBuilder.Entity<OfficeAssignment>().ToTable("Instructor");
```  

### <a name="mapping-an-entity-type-to-insertupdatedelete-stored-procedures-ef6-onwards"></a>Сопоставление типа сущности с хранимые процедуры Insert/Update/Delete (для EF6 и выше)  

Начиная с EF6 можно сопоставить сущность использовать хранимые процедуры для вставки обновления и удаления. Дополнительные сведения см. в разделе, [код первого Insert/Update/Delete хранимые процедуры](~/ef6/modeling/code-first/fluent/cud-stored-procedures.md).  

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
