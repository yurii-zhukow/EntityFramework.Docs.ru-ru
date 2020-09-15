---
title: API Fluent с VB.NET-EF6
description: API Fluent с VB.NET в Entity Framework 6
author: divega
ms.date: 10/23/2016
uid: ef6/modeling/code-first/fluent/vb
ms.openlocfilehash: d59e685578fb151aea2ae0b41a933f3e13de6c50
ms.sourcegitcommit: abda0872f86eefeca191a9a11bfca976bc14468b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/14/2020
ms.locfileid: "90072581"
---
# <a name="fluent-api-with-vbnet"></a><span data-ttu-id="d417a-103">API Fluent с VB.NET</span><span class="sxs-lookup"><span data-stu-id="d417a-103">Fluent API with VB.NET</span></span>
<span data-ttu-id="d417a-104">Code First позволяет определить модель с помощью \# классов C или VB.NET.</span><span class="sxs-lookup"><span data-stu-id="d417a-104">Code First allows you to define your model using C\# or VB.NET classes.</span></span> <span data-ttu-id="d417a-105">Дополнительную конфигурацию можно также выполнить с помощью атрибутов в классах и свойствах или с помощью API Fluent.</span><span class="sxs-lookup"><span data-stu-id="d417a-105">Additional configuration can optionally be performed using attributes on your classes and properties or by using a fluent API.</span></span> <span data-ttu-id="d417a-106">В этом пошаговом руководстве показано, как выполнить настройку API Fluent с помощью VB.NET.</span><span class="sxs-lookup"><span data-stu-id="d417a-106">This walkthrough shows how to perform fluent API configuration using VB.NET.</span></span>

<span data-ttu-id="d417a-107">На этой странице предполагается, что у вас есть базовое представление о Code First.</span><span class="sxs-lookup"><span data-stu-id="d417a-107">This page assumes you have a basic understanding of Code First.</span></span> <span data-ttu-id="d417a-108">Дополнительные сведения о Code First см. в следующих пошаговых руководствах.</span><span class="sxs-lookup"><span data-stu-id="d417a-108">Check out the following walkthroughs for more information on Code First:</span></span>

-   [<span data-ttu-id="d417a-109">Создание базы данных с помощью Code First</span><span class="sxs-lookup"><span data-stu-id="d417a-109">Code First to a New Database</span></span>](xref:ef6/modeling/code-first/workflows/new-database)
-   [<span data-ttu-id="d417a-110">Code First существующей базы данных</span><span class="sxs-lookup"><span data-stu-id="d417a-110">Code First to an Existing Database</span></span>](xref:ef6/modeling/code-first/workflows/existing-database)

## <a name="pre-requisites"></a><span data-ttu-id="d417a-111">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="d417a-111">Pre-Requisites</span></span>

<span data-ttu-id="d417a-112">Для выполнения инструкций этого пошагового руководства необходимо установить Visual Studio 2010 или Visual Studio 2012.</span><span class="sxs-lookup"><span data-stu-id="d417a-112">You will need to have at least Visual Studio 2010 or Visual Studio 2012 installed to complete this walkthrough.</span></span>

<span data-ttu-id="d417a-113">Если вы используете Visual Studio 2010, вам также потребуется установить [NuGet](https://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c) .</span><span class="sxs-lookup"><span data-stu-id="d417a-113">If you are using Visual Studio 2010, you will also need to have [NuGet](https://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c) installed</span></span>

## <a name="create-the-application"></a><span data-ttu-id="d417a-114">Создание приложения</span><span class="sxs-lookup"><span data-stu-id="d417a-114">Create the Application</span></span>

<span data-ttu-id="d417a-115">Чтобы не усложнять, мы создадим простое консольное приложение, которое использует Code First для доступа к данным.</span><span class="sxs-lookup"><span data-stu-id="d417a-115">To keep things simple we’re going to build a basic console application that uses Code First to perform data access.</span></span>

-   <span data-ttu-id="d417a-116">Открытие Visual Studio</span><span class="sxs-lookup"><span data-stu-id="d417a-116">Open Visual Studio</span></span>
-   <span data-ttu-id="d417a-117">**Файл- &gt; создать &gt; проект...**</span><span class="sxs-lookup"><span data-stu-id="d417a-117">**File -&gt; New -&gt; Project…**</span></span>
-   <span data-ttu-id="d417a-118">Выбор **окон** в меню слева и **консольное приложение**</span><span class="sxs-lookup"><span data-stu-id="d417a-118">Select **Windows** from the left menu and **Console Application**</span></span>
-   <span data-ttu-id="d417a-119">Введите **кодефирствбсампле** в качестве имени</span><span class="sxs-lookup"><span data-stu-id="d417a-119">Enter **CodeFirstVBSample** as the name</span></span>
-   <span data-ttu-id="d417a-120">Нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="d417a-120">Select **OK**</span></span>

## <a name="define-the-model"></a><span data-ttu-id="d417a-121">Определение модели</span><span class="sxs-lookup"><span data-stu-id="d417a-121">Define the Model</span></span>

<span data-ttu-id="d417a-122">На этом шаге предстоит определить типы сущностей POCO VB.NET, представляющие концептуальную модель.</span><span class="sxs-lookup"><span data-stu-id="d417a-122">In this step you will define VB.NET POCO entity types that represent the conceptual model.</span></span> <span data-ttu-id="d417a-123">Классы не обязательно должны быть производными от каких либо базовых классов, ни реализовывать какие бы то ни было интерфейсы.</span><span class="sxs-lookup"><span data-stu-id="d417a-123">The classes do not need to derive from any base classes or implement any interfaces.</span></span>

-   <span data-ttu-id="d417a-124">Добавьте в проект новый класс, введите **счулмодел** в качестве имени класса.</span><span class="sxs-lookup"><span data-stu-id="d417a-124">Add a new class to the project, enter **SchoolModel** for the class name</span></span>
-   <span data-ttu-id="d417a-125">Замените содержимое нового класса следующим кодом.</span><span class="sxs-lookup"><span data-stu-id="d417a-125">Replace the contents of the new class with the following code</span></span>

``` vb
Public Class Department
    Public Sub New()
        Me.Courses = New List(Of Course)()
    End Sub

    ' Primary key
    Public Property DepartmentID() As Integer
    Public Property Name() As String
    Public Property Budget() As Decimal
    Public Property StartDate() As Date
    Public Property Administrator() As Integer?
    Public Overridable Property Courses() As ICollection(Of Course)
End Class

Public Class Course
    Public Sub New()
        Me.Instructors = New HashSet(Of Instructor)()
    End Sub

    ' Primary key
    Public Property CourseID() As Integer
    Public Property Title() As String
    Public Property Credits() As Integer

    ' Foreign  key that does not follow the Code First convention.
    ' The fluent API will be used to configure DepartmentID_FK  to be the foreign key for this entity.
    Public Property DepartmentID_FK() As Integer

    ' Navigation properties
    Public Overridable Property Department() As Department
    Public Overridable Property Instructors() As ICollection(Of Instructor)
End Class

Public Class OnlineCourse
    Inherits Course

    Public Property URL() As String
End Class

Partial Public Class OnsiteCourse
    Inherits Course

    Public Sub New()
        Details = New OnsiteCourseDetails()
    End Sub

    Public Property Details() As OnsiteCourseDetails
 End Class

' Complex type
Public Class OnsiteCourseDetails
    Public Property Time() As Date
    Public Property Location() As String
    Public Property Days() As String
End Class

Public Class Person
    ' Primary key
    Public Property PersonID() As Integer
    Public Property LastName() As String
    Public Property FirstName() As String
End Class

Public Class Instructor
    Inherits Person

    Public Sub New()
        Me.Courses = New List(Of Course)()
    End Sub

    Public Property HireDate() As Date

    ' Navigation properties
    Private privateCourses As ICollection(Of Course)
    Public Overridable Property Courses() As ICollection(Of Course)
    Public Overridable Property OfficeAssignment() As OfficeAssignment
End Class

Public Class OfficeAssignment
    ' Primary key that does not follow the Code First convention.
    ' The HasKey method is used later to configure the primary key for the entity.
    Public Property InstructorID() As Integer

    Public Property Location() As String
    Public Property Timestamp() As Byte()

    ' Navigation property
    Public Overridable Property Instructor() As Instructor
End Class
```

## <a name="define-a-derived-context"></a><span data-ttu-id="d417a-126">Определение производного контекста</span><span class="sxs-lookup"><span data-stu-id="d417a-126">Define a Derived Context</span></span>

<span data-ttu-id="d417a-127">Мы собираемся начать использовать типы из Entity Framework, поэтому нам нужно добавить пакет NuGet EntityFramework.</span><span class="sxs-lookup"><span data-stu-id="d417a-127">We’re about to start to using types from the Entity Framework so we need to add the EntityFramework NuGet package.</span></span>

-   <span data-ttu-id="d417a-128">\* \* Проект — &gt; **Управление пакетами NuGet...**</span><span class="sxs-lookup"><span data-stu-id="d417a-128">\*\*Project –&gt; **Manage NuGet Packages…**</span></span>
> [!NOTE]
> <span data-ttu-id="d417a-129">Если у вас нет **пакетов NuGet для управления...**</span><span class="sxs-lookup"><span data-stu-id="d417a-129">If you don’t have the **Manage NuGet Packages…**</span></span> <span data-ttu-id="d417a-130">параметр следует установить [последнюю версию NuGet](https://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c)</span><span class="sxs-lookup"><span data-stu-id="d417a-130">option you should install the [latest version of NuGet](https://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c)</span></span>
-   <span data-ttu-id="d417a-131">Выбор вкладки "в **сети** "</span><span class="sxs-lookup"><span data-stu-id="d417a-131">Select the **Online** tab</span></span>
-   <span data-ttu-id="d417a-132">Выберите пакет **EntityFramework**</span><span class="sxs-lookup"><span data-stu-id="d417a-132">Select the **EntityFramework** package</span></span>
-   <span data-ttu-id="d417a-133">Щелкните **Установить**.</span><span class="sxs-lookup"><span data-stu-id="d417a-133">Click **Install**</span></span>

<span data-ttu-id="d417a-134">Теперь пора определить производный контекст, который представляет сеанс с базой данных, что позволит нам запрашивать и сохранять данные.</span><span class="sxs-lookup"><span data-stu-id="d417a-134">Now it’s time to define a derived context, which represents a session with the database, allowing us to query and save data.</span></span> <span data-ttu-id="d417a-135">Мы определяем контекст, производный от System. Data. Entity. DbContext, и предоставляет типизированный DbSetный &lt; домик &gt; для каждого класса в нашей модели.</span><span class="sxs-lookup"><span data-stu-id="d417a-135">We define a context that derives from System.Data.Entity.DbContext and exposes a typed DbSet&lt;TEntity&gt; for each class in our model.</span></span>

-   <span data-ttu-id="d417a-136">Добавьте в проект новый класс, введите **SchoolContext** в качестве имени класса.</span><span class="sxs-lookup"><span data-stu-id="d417a-136">Add a new class to the project, enter **SchoolContext** for the class name</span></span>
-   <span data-ttu-id="d417a-137">Замените содержимое нового класса следующим кодом.</span><span class="sxs-lookup"><span data-stu-id="d417a-137">Replace the contents of the new class with the following code</span></span>

```vb
Imports System.ComponentModel.DataAnnotations
Imports System.ComponentModel.DataAnnotations.Schema
Imports System.Data.Entity
Imports System.Data.Entity.Infrastructure
Imports System.Data.Entity.ModelConfiguration.Conventions

Public Class SchoolContext
    Inherits DbContext

    Public Property OfficeAssignments() As DbSet(Of OfficeAssignment)
    Public Property Instructors() As DbSet(Of Instructor)
    Public Property Courses() As DbSet(Of Course)
    Public Property Departments() As DbSet(Of Department)

    Protected Overrides Sub OnModelCreating(ByVal modelBuilder As DbModelBuilder)
    End Sub
End Class
```

## <a name="configuring-with-the-fluent-api"></a><span data-ttu-id="d417a-138">Настройка с помощью API Fluent</span><span class="sxs-lookup"><span data-stu-id="d417a-138">Configuring with the Fluent API</span></span>

<span data-ttu-id="d417a-139">В этом разделе показано, как использовать API-интерфейсы Fluent для настройки типов для сопоставления таблиц, свойств и сопоставлений столбцов, а также связей между \\ типами таблиц в модели.</span><span class="sxs-lookup"><span data-stu-id="d417a-139">This section demonstrates how to use the fluent APIs to configure types to tables mapping, properties to columns mapping, and relationships between tables\\type in your model.</span></span> <span data-ttu-id="d417a-140">API-интерфейс Fluent предоставляется через тип **дбмоделбуилдер** и чаще всего используется путем переопределения метода **OnModelCreating** в **DbContext**.</span><span class="sxs-lookup"><span data-stu-id="d417a-140">The fluent API is exposed through the **DbModelBuilder** type and is most commonly accessed by overriding the **OnModelCreating** method on **DbContext**.</span></span>

-   <span data-ttu-id="d417a-141">Скопируйте приведенный ниже код и добавьте его в метод **OnModelCreating** , определенный в классе **SchoolContext** . в комментариях объясняется, что делает каждое сопоставление.</span><span class="sxs-lookup"><span data-stu-id="d417a-141">Copy the following code and add it to the **OnModelCreating** method defined on the **SchoolContext** class The comments explain what each mapping does</span></span>

``` vb
' Configure Code First to ignore PluralizingTableName convention
' If you keep this convention then the generated tables
' will have pluralized names.
modelBuilder.Conventions.Remove(Of PluralizingTableNameConvention)()


' Specifying that a Class is a Complex Type

' The model defined in this topic defines a type OnsiteCourseDetails.
' By convention, a type that has no primary key specified
' is treated as a complex type.
' There are some scenarios where Code First will not
' detect a complex type (for example, if you do have a property
' called ID, but you do not mean for it to be a primary key).
' In such cases, you would use the fluent API to
' explicitly specify that a type is a complex type.
modelBuilder.ComplexType(Of OnsiteCourseDetails)()


' Mapping a CLR Entity Type to a Specific Table in the Database.

' All properties of OfficeAssignment will be mapped
' to columns  in a table called t_OfficeAssignment.
modelBuilder.Entity(Of OfficeAssignment)().ToTable("t_OfficeAssignment")


' Mapping the Table-Per-Hierarchy (TPH) Inheritance

' In the TPH mapping scenario, all types in an inheritance hierarchy
' are mapped to a single table.
' A discriminator column is used to identify the type of each row.
' When creating your model with Code First,      
' TPH is the default strategy for the types that
' participate in the inheritance hierarchy.
' By default, the discriminator column is added
' to the table with the name “Discriminator”
' and the CLR type name of each type in the hierarchy
' is used for the discriminator values.
' You can modify the default behavior by using the fluent API.
modelBuilder.Entity(Of Person)().
    Map(Of Person)(Function(t) t.Requires("Type").
        HasValue("Person")).
        Map(Of Instructor)(Function(t) t.Requires("Type").
        HasValue("Instructor"))


' Mapping the Table-Per-Type (TPT) Inheritance

' In the TPT mapping scenario, all types are mapped to individual tables.
' Properties that belong solely to a base type or derived type are stored
' in a table that maps to that type. Tables that map to derived types
' also store a foreign key that joins the derived table with the base table.
modelBuilder.Entity(Of Course)().ToTable("Course")
modelBuilder.Entity(Of OnsiteCourse)().ToTable("OnsiteCourse")
modelBuilder.Entity(Of OnlineCourse)().ToTable("OnlineCourse")


' Configuring a Primary Key

' If your class defines a property whose name is “ID” or “Id”,
' or a class name followed by “ID” or “Id”,
' the Entity Framework treats this property as a primary key by convention.
' If your property name does not follow this pattern, use the HasKey method
' to configure the primary key for the entity.
modelBuilder.Entity(Of OfficeAssignment)().
    HasKey(Function(t) t.InstructorID)


' Specifying the Maximum Length on a Property

' In the following example, the Name property
' should be no longer than 50 characters.
' If you make the value longer than 50 characters,
' you will get a DbEntityValidationException exception.
modelBuilder.Entity(Of Department)().Property(Function(t) t.Name).
    HasMaxLength(60)


' Configuring the Property to be Required

' In the following example, the Name property is required.
' If you do not specify the Name,
' you will get a DbEntityValidationException exception.
' The database column used to store this property will be non-nullable.
modelBuilder.Entity(Of Department)().Property(Function(t) t.Name).
    IsRequired()


' Switching off Identity for Numeric Primary Keys

' The following example sets the DepartmentID property to
' System.ComponentModel.DataAnnotations.DatabaseGeneratedOption.None to indicate that
' the value will not be generated by the database.
modelBuilder.Entity(Of Course)().Property(Function(t) t.CourseID).
    HasDatabaseGeneratedOption(DatabaseGeneratedOption.None)

'Specifying NOT to Map a CLR Property to a Column in the Database
 modelBuilder.Entity(Of Department)().
     Ignore(Function(t) t.Administrator)

'Mapping a CLR Property to a Specific Column in the Database
 modelBuilder.Entity(Of Department)().Property(Function(t) t.Budget).
     HasColumnName("DepartmentBudget")

'Configuring the Data Type of a Database Column
 modelBuilder.Entity(Of Department)().Property(Function(t) t.Name).
     HasColumnType("varchar")

'Configuring Properties on a Complex Type
modelBuilder.Entity(Of OnsiteCourse)().Property(Function(t) t.Details.Days).
    HasColumnName("Days")
modelBuilder.Entity(Of OnsiteCourse)().Property(Function(t) t.Details.Location).
    HasColumnName("Location")
modelBuilder.Entity(Of OnsiteCourse)().Property(Function(t) t.Details.Time).
    HasColumnName("Time")


' Map one-to-zero or one relationship

' The OfficeAssignment has the InstructorID
' property that is a primary key and a foreign key.
modelBuilder.Entity(Of OfficeAssignment)().
    HasRequired(Function(t) t.Instructor).
    WithOptional(Function(t) t.OfficeAssignment)


' Configuring a Many-to-Many Relationship

' The following code configures a many-to-many relationship
' between the Course  and Instructor types.
' In the following example, the default Code First conventions
' are used  to create a join table.
' As a result the CourseInstructor table is created with
' Course_CourseID  and Instructor_InstructorID columns.
modelBuilder.Entity(Of Course)().
    HasMany(Function(t) t.Instructors).
    WithMany(Function(t) t.Courses)


' Configuring a Many-to-Many Relationship and specifying the names
' of the columns in the join table

' If you want to specify the join table name
' and the names of the columns in the table
' you need to do additional configuration by using the Map method.
' The following code generates the CourseInstructor
' table with CourseID and InstructorID columns.
modelBuilder.Entity(Of Course)().
    HasMany(Function(t) t.Instructors).
    WithMany(Function(t) t.Courses).
    Map(Sub(m)
            m.ToTable("CourseInstructor")
            m.MapLeftKey("CourseID")
            m.MapRightKey("InstructorID")
        End Sub)


' Configuring a foreign key name that does not follow the Code First convention

' The foreign key property on the Course class is called DepartmentID_FK
' since that does not follow Code First conventions you need to explicitly specify
' that you want DepartmentID_FK to be the foreign key.
modelBuilder.Entity(Of Course)().
    HasRequired(Function(t) t.Department).
    WithMany(Function(t) t.Courses).
    HasForeignKey(Function(t) t.DepartmentID_FK)


' Enabling Cascade Delete

' By default, if a foreign key on the dependent entity is not nullable,
' then Code First sets cascade delete on the relationship.
' If a foreign key on the dependent entity is nullable,
' Code First does not set cascade delete on the relationship,
' and when the principal is deleted the foreign key will be set to null.
' The following code configures cascade delete on the relationship.

' You can also remove the cascade delete conventions by using:
' modelBuilder.Conventions.Remove<OneToManyCascadeDeleteConvention>()
' and modelBuilder.Conventions.Remove<ManyToManyCascadeDeleteConvention>().
modelBuilder.Entity(Of Course)().
    HasRequired(Function(t) t.Department).
    WithMany(Function(t) t.Courses).
    HasForeignKey(Function(d) d.DepartmentID_FK).
    WillCascadeOnDelete(False)
```

## <a name="using-the-model"></a><span data-ttu-id="d417a-142">Использование модели</span><span class="sxs-lookup"><span data-stu-id="d417a-142">Using the Model</span></span>

<span data-ttu-id="d417a-143">Давайте выполним доступ к данным с помощью **SchoolContext** , чтобы увидеть модель в действии.</span><span class="sxs-lookup"><span data-stu-id="d417a-143">Let's perform some data access using the **SchoolContext** to see out model in action.</span></span>

-   <span data-ttu-id="d417a-144">Откройте файл Module1. vb, в котором определена функция Main.</span><span class="sxs-lookup"><span data-stu-id="d417a-144">Open the Module1.vb file where the Main function is defined</span></span>
-   <span data-ttu-id="d417a-145">Скопируйте и вставьте следующее определение Module1.</span><span class="sxs-lookup"><span data-stu-id="d417a-145">Copy and paste the following Module1 definition</span></span>

``` vb
Imports System.Data.Entity

Module Module1

    Sub Main()

        Using context As New SchoolContext()

            ' Create and save a new Department.
            Console.Write("Enter a name for a new Department: ")
            Dim name = Console.ReadLine()

            Dim department = New Department With { .Name = name, .StartDate = DateTime.Now }
            context.Departments.Add(department)
            context.SaveChanges()

            ' Display all Departments from the database ordered by name
            Dim departments =
                From d In context.Departments
                Order By d.Name
                Select d

            Console.WriteLine("All Departments in the database:")
            For Each department In departments
                Console.WriteLine(department.Name)
            Next

        End Using

        Console.WriteLine("Press any key to exit...")
        Console.ReadKey()

    End Sub

End Module
```

<span data-ttu-id="d417a-146">Теперь можно запустить приложение и протестировать его.</span><span class="sxs-lookup"><span data-stu-id="d417a-146">You can now run the application and test it out.</span></span>

```console
Enter a name for a new Department: Computing
All Departments in the database:
Computing
Press any key to exit...
```
