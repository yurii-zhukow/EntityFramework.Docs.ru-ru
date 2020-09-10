---
title: API-интерфейс Fluent — Настройка и сопоставление свойств и типов — EF6
description: API-интерфейс Fluent — Настройка и сопоставление свойств и типов в Entity Framework 6
author: divega
ms.date: 10/23/2016
ms.assetid: 648ed274-c501-4630-88e0-d728ab5c4057
uid: ef6/modeling/code-first/fluent/types-and-properties
ms.openlocfilehash: d49da416b2df5828fce5618cc196e22dfbc2bd34
ms.sourcegitcommit: 7c3939504bb9da3f46bea3443638b808c04227c2
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/09/2020
ms.locfileid: "89618061"
---
# <a name="fluent-api---configuring-and-mapping-properties-and-types"></a><span data-ttu-id="a8950-103">API-интерфейс Fluent — Настройка и сопоставление свойств и типов</span><span class="sxs-lookup"><span data-stu-id="a8950-103">Fluent API - Configuring and Mapping Properties and Types</span></span>
<span data-ttu-id="a8950-104">При работе с Entity Framework Code First поведением по умолчанию является привязка классов POCO к таблицам с помощью набора соглашений, помогут в EF.</span><span class="sxs-lookup"><span data-stu-id="a8950-104">When working with Entity Framework Code First the default behavior is to map your POCO classes to tables using a set of conventions baked into EF.</span></span> <span data-ttu-id="a8950-105">Однако иногда вы не можете следовать этим соглашениям и не хотите сопоставлять сущности с другими условиями.</span><span class="sxs-lookup"><span data-stu-id="a8950-105">Sometimes, however, you cannot or do not want to follow those conventions and need to map entities to something other than what the conventions dictate.</span></span>  

<span data-ttu-id="a8950-106">Существует два основных способа настройки EF для использования чего-либо, кроме соглашений, а именно [заметок](xref:ef6/modeling/code-first/data-annotations) или API Fluent системы EFS.</span><span class="sxs-lookup"><span data-stu-id="a8950-106">There are two main ways you can configure EF to use something other than conventions, namely [annotations](xref:ef6/modeling/code-first/data-annotations) or EFs fluent API.</span></span> <span data-ttu-id="a8950-107">Заметки охватывают только подмножество функций API Fluent, поэтому существуют сценарии сопоставления, которые не могут быть достигнуты с помощью заметок.</span><span class="sxs-lookup"><span data-stu-id="a8950-107">The annotations only cover a subset of the fluent API functionality, so there are mapping scenarios that cannot be achieved using annotations.</span></span> <span data-ttu-id="a8950-108">В этой статье показано, как использовать API Fluent для настройки свойств.</span><span class="sxs-lookup"><span data-stu-id="a8950-108">This article is designed to demonstrate how to use the fluent API to configure properties.</span></span>  

<span data-ttu-id="a8950-109">Наиболее часто доступ к интерфейсу API для первого интерфейса Fluent, переопределяя метод [OnModelCreating](https://msdn.microsoft.com/library/system.data.entity.dbcontext.onmodelcreating.aspx) на производном [DbContext](https://msdn.microsoft.com/library/system.data.entity.dbcontext.aspx).</span><span class="sxs-lookup"><span data-stu-id="a8950-109">The code first fluent API is most commonly accessed by overriding the [OnModelCreating](https://msdn.microsoft.com/library/system.data.entity.dbcontext.onmodelcreating.aspx) method on your derived [DbContext](https://msdn.microsoft.com/library/system.data.entity.dbcontext.aspx).</span></span> <span data-ttu-id="a8950-110">Следующие примеры предназначены для демонстрации выполнения различных задач с помощью API-интерфейса Fluent и позволяют скопировать код и настроить его в соответствии с моделью. Если вы хотите увидеть модель, которую можно использовать с параметром "как есть", она указана в конце этой статьи.</span><span class="sxs-lookup"><span data-stu-id="a8950-110">The following samples are designed to show how to do various tasks with the fluent api and allow you to copy the code out and customize it to suit your model, if you wish to see the model that they can be used with as-is then it is provided at the end of this article.</span></span>  

## <a name="model-wide-settings"></a><span data-ttu-id="a8950-111">Параметры уровня модели</span><span class="sxs-lookup"><span data-stu-id="a8950-111">Model-Wide Settings</span></span>  

### <a name="default-schema-ef6-onwards"></a><span data-ttu-id="a8950-112">Схема по умолчанию (EF6)</span><span class="sxs-lookup"><span data-stu-id="a8950-112">Default Schema (EF6 onwards)</span></span>  

<span data-ttu-id="a8950-113">Начиная с EF6 можно использовать метод Хасдефаултсчема на Дбмоделбуилдер, чтобы указать схему базы данных, которая будет использоваться для всех таблиц, хранимых процедур и т. д. Этот параметр по умолчанию будет переопределен для всех объектов, для которых явно настроена другая схема.</span><span class="sxs-lookup"><span data-stu-id="a8950-113">Starting with EF6 you can use the HasDefaultSchema method on DbModelBuilder to specify the database schema to use for all tables, stored procedures, etc. This default setting will be overridden for any objects that you explicitly configure a different schema for.</span></span>  

``` csharp
modelBuilder.HasDefaultSchema("sales");
```  

### <a name="custom-conventions-ef6-onwards"></a><span data-ttu-id="a8950-114">Пользовательские соглашения (EF6)</span><span class="sxs-lookup"><span data-stu-id="a8950-114">Custom Conventions (EF6 onwards)</span></span>  

<span data-ttu-id="a8950-115">Начиная с EF6 можно создавать собственные соглашения для дополнения к Code First.</span><span class="sxs-lookup"><span data-stu-id="a8950-115">Starting with EF6 you can create your own conventions to supplement the ones included in Code First.</span></span> <span data-ttu-id="a8950-116">Дополнительные сведения см. в разделе [Custom Code First Conventions](xref:ef6/modeling/code-first/conventions/custom).</span><span class="sxs-lookup"><span data-stu-id="a8950-116">For more details, see [Custom Code First Conventions](xref:ef6/modeling/code-first/conventions/custom).</span></span>  

## <a name="property-mapping"></a><span data-ttu-id="a8950-117">Сопоставление свойств</span><span class="sxs-lookup"><span data-stu-id="a8950-117">Property Mapping</span></span>  

<span data-ttu-id="a8950-118">Метод [Property](https://msdn.microsoft.com/library/system.data.entity.infrastructure.dbentityentry.property.aspx) используется для настройки атрибутов для каждого свойства, принадлежащего сущности или сложному типу.</span><span class="sxs-lookup"><span data-stu-id="a8950-118">The [Property](https://msdn.microsoft.com/library/system.data.entity.infrastructure.dbentityentry.property.aspx) method is used to configure attributes for each property belonging to an entity or complex type.</span></span> <span data-ttu-id="a8950-119">Метод property используется для получения объекта конфигурации для заданного свойства.</span><span class="sxs-lookup"><span data-stu-id="a8950-119">The Property method is used to obtain a configuration object for a given property.</span></span> <span data-ttu-id="a8950-120">Параметры объекта конфигурации относятся к настраиваемому типу. Параметр Unicode доступен только для строковых свойств, например.</span><span class="sxs-lookup"><span data-stu-id="a8950-120">The options on the configuration object are specific to the type being configured; IsUnicode is available only on string properties for example.</span></span>  

### <a name="configuring-a-primary-key"></a><span data-ttu-id="a8950-121">Настройка первичного ключа</span><span class="sxs-lookup"><span data-stu-id="a8950-121">Configuring a Primary Key</span></span>  

<span data-ttu-id="a8950-122">Соглашение Entity Framework для первичных ключей:</span><span class="sxs-lookup"><span data-stu-id="a8950-122">The Entity Framework convention for primary keys is:</span></span>  

1. <span data-ttu-id="a8950-123">Класс определяет свойство, имя которого равно ID или ID.</span><span class="sxs-lookup"><span data-stu-id="a8950-123">Your class defines a property whose name is “ID” or “Id”</span></span>  
2. <span data-ttu-id="a8950-124">или имя класса, за которым следует "ID" или "ID"</span><span class="sxs-lookup"><span data-stu-id="a8950-124">or a class name followed by “ID” or “Id”</span></span>  

<span data-ttu-id="a8950-125">Чтобы явно задать свойство как первичный ключ, можно использовать метод HasKey.</span><span class="sxs-lookup"><span data-stu-id="a8950-125">To explicitly set a property to be a primary key, you can use the HasKey method.</span></span> <span data-ttu-id="a8950-126">В следующем примере метод HasKey используется для настройки первичного ключа InstructorID в типе OfficeAssignment.</span><span class="sxs-lookup"><span data-stu-id="a8950-126">In the following example, the HasKey method is used to configure the InstructorID primary key on the OfficeAssignment type.</span></span>  

``` csharp
modelBuilder.Entity<OfficeAssignment>().HasKey(t => t.InstructorID);
```  

### <a name="configuring-a-composite-primary-key"></a><span data-ttu-id="a8950-127">Настройка составного первичного ключа</span><span class="sxs-lookup"><span data-stu-id="a8950-127">Configuring a Composite Primary Key</span></span>  

<span data-ttu-id="a8950-128">В следующем примере свойства DepartmentID и Name настраиваются как составной первичный ключ типа отдела.</span><span class="sxs-lookup"><span data-stu-id="a8950-128">The following example configures the DepartmentID and Name properties to be the composite primary key of the Department type.</span></span>  

``` csharp
modelBuilder.Entity<Department>().HasKey(t => new { t.DepartmentID, t.Name });
```  

### <a name="switching-off-identity-for-numeric-primary-keys"></a><span data-ttu-id="a8950-129">Отключение удостоверения для числовых первичных ключей</span><span class="sxs-lookup"><span data-stu-id="a8950-129">Switching off Identity for Numeric Primary Keys</span></span>  

<span data-ttu-id="a8950-130">В следующем примере свойству DepartmentID присваивается значение System. ComponentModel. аннотаций. Датабасеженератедоптион. None, чтобы указать, что оно не будет создано базой данных.</span><span class="sxs-lookup"><span data-stu-id="a8950-130">The following example sets the DepartmentID property to System.ComponentModel.DataAnnotations.DatabaseGeneratedOption.None to indicate that the value will not be generated by the database.</span></span>  

``` csharp
modelBuilder.Entity<Department>().Property(t => t.DepartmentID)
    .HasDatabaseGeneratedOption(DatabaseGeneratedOption.None);
```  

### <a name="specifying-the-maximum-length-on-a-property"></a><span data-ttu-id="a8950-131">Указание максимальной длины свойства</span><span class="sxs-lookup"><span data-stu-id="a8950-131">Specifying the Maximum Length on a Property</span></span>  

<span data-ttu-id="a8950-132">В следующем примере свойство Name должно содержать не более 50 символов.</span><span class="sxs-lookup"><span data-stu-id="a8950-132">In the following example, the Name property should be no longer than 50 characters.</span></span> <span data-ttu-id="a8950-133">Если сделать значение длиннее 50 символов, вы получите исключение [дбентитивалидатионексцептион](https://msdn.microsoft.com/library/system.data.entity.validation.dbentityvalidationexception.aspx) .</span><span class="sxs-lookup"><span data-stu-id="a8950-133">If you make the value longer than 50 characters, you will get a [DbEntityValidationException](https://msdn.microsoft.com/library/system.data.entity.validation.dbentityvalidationexception.aspx) exception.</span></span> <span data-ttu-id="a8950-134">Если Code First создает базу данных из этой модели, она также установит максимальную длину столбца имени в 50 символов.</span><span class="sxs-lookup"><span data-stu-id="a8950-134">If Code First creates a database from this model it will also set the maximum length of the Name column to 50 characters.</span></span>  

``` csharp
modelBuilder.Entity<Department>().Property(t => t.Name).HasMaxLength(50);
```  

### <a name="configuring-the-property-to-be-required"></a><span data-ttu-id="a8950-135">Настройка требуемого свойства</span><span class="sxs-lookup"><span data-stu-id="a8950-135">Configuring the Property to be Required</span></span>  

<span data-ttu-id="a8950-136">В следующем примере свойство Name является обязательным.</span><span class="sxs-lookup"><span data-stu-id="a8950-136">In the following example, the Name property is required.</span></span> <span data-ttu-id="a8950-137">Если не указать имя, будет получено исключение Дбентитивалидатионексцептион.</span><span class="sxs-lookup"><span data-stu-id="a8950-137">If you do not specify the Name, you will get a DbEntityValidationException exception.</span></span> <span data-ttu-id="a8950-138">Если Code First создает базу данных из этой модели, то столбец, используемый для хранения этого свойства, обычно не допускает значения NULL.</span><span class="sxs-lookup"><span data-stu-id="a8950-138">If Code First creates a database from this model then the column used to store this property will usually be non-nullable.</span></span>  

> [!NOTE]
> <span data-ttu-id="a8950-139">В некоторых случаях для столбца в базе данных может оказаться невозможным значение null, даже если свойство является обязательным.</span><span class="sxs-lookup"><span data-stu-id="a8950-139">In some cases it may not be possible for the column in the database to be non-nullable even though the property is required.</span></span> <span data-ttu-id="a8950-140">Например, при использовании ИЕРАРХИческих данных стратегии наследования для нескольких типов хранится в одной таблице.</span><span class="sxs-lookup"><span data-stu-id="a8950-140">For example, when using a TPH inheritance strategy data for multiple types is stored in a single table.</span></span> <span data-ttu-id="a8950-141">Если производный тип содержит обязательное свойство, столбец не может быть сделан пустым, так как не все типы в иерархии будут иметь это свойство.</span><span class="sxs-lookup"><span data-stu-id="a8950-141">If a derived type includes a required property the column cannot be made non-nullable since not all types in the hierarchy will have this property.</span></span>  

``` csharp
modelBuilder.Entity<Department>().Property(t => t.Name).IsRequired();
```  

### <a name="configuring-an-index-on-one-or-more-properties"></a><span data-ttu-id="a8950-142">Настройка индекса для одного или нескольких свойств</span><span class="sxs-lookup"><span data-stu-id="a8950-142">Configuring an Index on one or more properties</span></span>  

> [!NOTE]
> <span data-ttu-id="a8950-143">**EF 6.1 только в сторону** — атрибут index появился в Entity Framework 6,1.</span><span class="sxs-lookup"><span data-stu-id="a8950-143">**EF6.1 Onwards Only** - The Index attribute was introduced in Entity Framework 6.1.</span></span> <span data-ttu-id="a8950-144">Если вы используете более раннюю версию, сведения в этом разделе не применяются.</span><span class="sxs-lookup"><span data-stu-id="a8950-144">If you are using an earlier version the information in this section does not apply.</span></span>  

<span data-ttu-id="a8950-145">Создание индексов изначально не поддерживается API-интерфейсом Fluent, но вы можете использовать поддержку **индексаттрибуте** через API Fluent.</span><span class="sxs-lookup"><span data-stu-id="a8950-145">Creating indexes isn't natively supported by the Fluent API, but you can make use of the support for **IndexAttribute** via the Fluent API.</span></span> <span data-ttu-id="a8950-146">Атрибуты индекса обрабатываются путем включения заметки модели для модели, которая затем преобразуется в индекс в базе данных позже в конвейере.</span><span class="sxs-lookup"><span data-stu-id="a8950-146">Index attributes are processed by including a model annotation on the model that is then turned into an Index in the database later in the pipeline.</span></span> <span data-ttu-id="a8950-147">Эти заметки можно добавить вручную с помощью API-интерфейса Fluent.</span><span class="sxs-lookup"><span data-stu-id="a8950-147">You can manually add these same annotations using the Fluent API.</span></span>  

<span data-ttu-id="a8950-148">Самый простой способ сделать это — создать экземпляр **индексаттрибуте** , содержащий все параметры для нового индекса.</span><span class="sxs-lookup"><span data-stu-id="a8950-148">The easiest way to do this is to create an instance of **IndexAttribute** that contains all the settings for the new index.</span></span> <span data-ttu-id="a8950-149">Затем можно создать экземпляр **индексаннотатион** , который является особым типом EF, который преобразует параметры **индексаттрибуте** в заметку модели, которая может храниться в модели EF.</span><span class="sxs-lookup"><span data-stu-id="a8950-149">You can then create an instance of **IndexAnnotation** which is an EF specific type that will convert the **IndexAttribute** settings into a model annotation that can be stored on the EF model.</span></span> <span data-ttu-id="a8950-150">Затем их можно передать в метод **хасколумнаннотатион** в API-интерфейсе Fluent, указав **индекс** имени для заметки.</span><span class="sxs-lookup"><span data-stu-id="a8950-150">These can then be passed to the **HasColumnAnnotation** method on the Fluent API, specifying the name **Index** for the annotation.</span></span>  

``` csharp
modelBuilder
    .Entity<Department>()
    .Property(t => t.Name)
    .HasColumnAnnotation("Index", new IndexAnnotation(new IndexAttribute()));
```  

<span data-ttu-id="a8950-151">Полный список параметров, доступных в **индексаттрибуте**, см. в разделе *index* статьи [Code First Data Annotations](xref:ef6/modeling/code-first/data-annotations).</span><span class="sxs-lookup"><span data-stu-id="a8950-151">For a complete list of the settings available in **IndexAttribute**, see the *Index* section of [Code First Data Annotations](xref:ef6/modeling/code-first/data-annotations).</span></span> <span data-ttu-id="a8950-152">Это включает в себя настройку имени индекса, создание уникальных индексов и создание индексов с несколькими столбцами.</span><span class="sxs-lookup"><span data-stu-id="a8950-152">This includes customizing the index name, creating unique indexes, and creating multi-column indexes.</span></span>  

<span data-ttu-id="a8950-153">Можно указать несколько аннотаций индекса для одного свойства, передав массив **индексаттрибуте** в конструктор **индексаннотатион**.</span><span class="sxs-lookup"><span data-stu-id="a8950-153">You can specify multiple index annotations on a single property by passing an array of **IndexAttribute** to the constructor of **IndexAnnotation**.</span></span>  

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

### <a name="specifying-not-to-map-a-clr-property-to-a-column-in-the-database"></a><span data-ttu-id="a8950-154">Указание не сопоставлять свойство CLR со столбцом в базе данных</span><span class="sxs-lookup"><span data-stu-id="a8950-154">Specifying Not to Map a CLR Property to a Column in the Database</span></span>  

<span data-ttu-id="a8950-155">В следующем примере показано, как указать, что свойство типа CLR не сопоставлено со столбцом в базе данных.</span><span class="sxs-lookup"><span data-stu-id="a8950-155">The following example shows how to specify that a property on a CLR type is not mapped to a column in the database.</span></span>  

``` csharp
modelBuilder.Entity<Department>().Ignore(t => t.Budget);
```  

### <a name="mapping-a-clr-property-to-a-specific-column-in-the-database"></a><span data-ttu-id="a8950-156">Сопоставление свойства CLR с конкретным столбцом в базе данных</span><span class="sxs-lookup"><span data-stu-id="a8950-156">Mapping a CLR Property to a Specific Column in the Database</span></span>  

<span data-ttu-id="a8950-157">В следующем примере показано сопоставление свойства CLR Name со столбцом базы данных Департментнаме.</span><span class="sxs-lookup"><span data-stu-id="a8950-157">The following example maps the Name CLR property to the DepartmentName database column.</span></span>  

``` csharp
modelBuilder.Entity<Department>()
    .Property(t => t.Name)
    .HasColumnName("DepartmentName");
```  

### <a name="renaming-a-foreign-key-that-is-not-defined-in-the-model"></a><span data-ttu-id="a8950-158">Переименование внешнего ключа, который не определен в модели</span><span class="sxs-lookup"><span data-stu-id="a8950-158">Renaming a Foreign Key That Is Not Defined in the Model</span></span>  

<span data-ttu-id="a8950-159">Если вы решили не определять внешний ключ для типа CLR, но хотите указать, какое имя оно должно иметь в базе данных, выполните следующие действия.</span><span class="sxs-lookup"><span data-stu-id="a8950-159">If you choose not to define a foreign key on a CLR type, but want to specify what name it should have in the database, do the following:</span></span>  

``` csharp
modelBuilder.Entity<Course>()
    .HasRequired(c => c.Department)
    .WithMany(t => t.Courses)
    .Map(m => m.MapKey("ChangedDepartmentID"));
```  

### <a name="configuring-whether-a-string-property-supports-unicode-content"></a><span data-ttu-id="a8950-160">Настройка того, поддерживает ли строковое свойство содержимое в Юникоде</span><span class="sxs-lookup"><span data-stu-id="a8950-160">Configuring whether a String Property Supports Unicode Content</span></span>  

<span data-ttu-id="a8950-161">По умолчанию строки задаются в Юникоде (nvarchar в SQL Server).</span><span class="sxs-lookup"><span data-stu-id="a8950-161">By default strings are Unicode (nvarchar in SQL Server).</span></span> <span data-ttu-id="a8950-162">Чтобы указать, что строка должна иметь тип varchar, можно использовать метод Unicode.</span><span class="sxs-lookup"><span data-stu-id="a8950-162">You can use the IsUnicode method to specify that a string should be of varchar type.</span></span>  

``` csharp
modelBuilder.Entity<Department>()
    .Property(t => t.Name)
    .IsUnicode(false);
```  

### <a name="configuring-the-data-type-of-a-database-column"></a><span data-ttu-id="a8950-163">Настройка типа данных столбца базы данных</span><span class="sxs-lookup"><span data-stu-id="a8950-163">Configuring the Data Type of a Database Column</span></span>  

<span data-ttu-id="a8950-164">Метод [хасколумнтипе](https://msdn.microsoft.com/library/system.data.entity.modelconfiguration.configuration.stringcolumnconfiguration.hascolumntype.aspx) позволяет сопоставлять различные представления одного и того же базового типа.</span><span class="sxs-lookup"><span data-stu-id="a8950-164">The [HasColumnType](https://msdn.microsoft.com/library/system.data.entity.modelconfiguration.configuration.stringcolumnconfiguration.hascolumntype.aspx) method enables mapping to different representations of the same basic type.</span></span> <span data-ttu-id="a8950-165">Использование этого метода не позволяет выполнять преобразование данных во время выполнения.</span><span class="sxs-lookup"><span data-stu-id="a8950-165">Using this method does not enable you to perform any conversion of the data at run time.</span></span> <span data-ttu-id="a8950-166">Обратите внимание, что в качестве параметра Columns можно задать значение типа varchar, так как он является независимым от базы данных.</span><span class="sxs-lookup"><span data-stu-id="a8950-166">Note that IsUnicode is the preferred way of setting columns to varchar, as it is database agnostic.</span></span>  

``` csharp
modelBuilder.Entity<Department>()   
    .Property(p => p.Name)   
    .HasColumnType("varchar");
```  

### <a name="configuring-properties-on-a-complex-type"></a><span data-ttu-id="a8950-167">Настройка свойств сложного типа</span><span class="sxs-lookup"><span data-stu-id="a8950-167">Configuring Properties on a Complex Type</span></span>  

<span data-ttu-id="a8950-168">Существует два способа настройки скалярных свойств для сложного типа.</span><span class="sxs-lookup"><span data-stu-id="a8950-168">There are two ways to configure scalar properties on a complex type.</span></span>  

<span data-ttu-id="a8950-169">Свойство можно вызвать в Комплекстипеконфигуратион.</span><span class="sxs-lookup"><span data-stu-id="a8950-169">You can call Property on ComplexTypeConfiguration.</span></span>  

``` csharp
modelBuilder.ComplexType<Details>()
    .Property(t => t.Location)
    .HasMaxLength(20);
```  

<span data-ttu-id="a8950-170">Можно также использовать точечную нотацию для доступа к свойству сложного типа.</span><span class="sxs-lookup"><span data-stu-id="a8950-170">You can also use the dot notation to access a property of a complex type.</span></span>  

``` csharp
modelBuilder.Entity<OnsiteCourse>()
    .Property(t => t.Details.Location)
    .HasMaxLength(20);
```  

### <a name="configuring-a-property-to-be-used-as-an-optimistic-concurrency-token"></a><span data-ttu-id="a8950-171">Настройка свойства для использования в качестве маркера оптимистичного параллелизма</span><span class="sxs-lookup"><span data-stu-id="a8950-171">Configuring a Property to Be Used as an Optimistic Concurrency Token</span></span>  

<span data-ttu-id="a8950-172">Чтобы указать, что свойство в сущности представляет маркер параллелизма, можно использовать либо атрибут ConcurrencyCheck, либо метод Исконкурренцитокен.</span><span class="sxs-lookup"><span data-stu-id="a8950-172">To specify that a property in an entity represents a concurrency token, you can use either the ConcurrencyCheck attribute or the IsConcurrencyToken method.</span></span>  

``` csharp
modelBuilder.Entity<OfficeAssignment>()
    .Property(t => t.Timestamp)
    .IsConcurrencyToken();
```  

<span data-ttu-id="a8950-173">Можно также использовать метод Исровверсион, чтобы настроить свойство в качестве версии строки в базе данных.</span><span class="sxs-lookup"><span data-stu-id="a8950-173">You can also use the IsRowVersion method to configure the property to be a row version in the database.</span></span> <span data-ttu-id="a8950-174">Если задать для свойства значение "версия строки", она автоматически настроит его как маркер оптимистичного параллелизма.</span><span class="sxs-lookup"><span data-stu-id="a8950-174">Setting the property to be a row version automatically configures it to be an optimistic concurrency token.</span></span>  

``` csharp
modelBuilder.Entity<OfficeAssignment>()
    .Property(t => t.Timestamp)
    .IsRowVersion();
```  

## <a name="type-mapping"></a><span data-ttu-id="a8950-175">Сопоставление типов</span><span class="sxs-lookup"><span data-stu-id="a8950-175">Type Mapping</span></span>  

### <a name="specifying-that-a-class-is-a-complex-type"></a><span data-ttu-id="a8950-176">Указание того, что класс является сложным типом</span><span class="sxs-lookup"><span data-stu-id="a8950-176">Specifying That a Class Is a Complex Type</span></span>  

<span data-ttu-id="a8950-177">По соглашению тип, для которого не задан первичный ключ, рассматривается как сложный тип.</span><span class="sxs-lookup"><span data-stu-id="a8950-177">By convention, a type that has no primary key specified is treated as a complex type.</span></span> <span data-ttu-id="a8950-178">Существует несколько сценариев, в которых Code First не может обнаружить сложный тип (например, если у вас есть свойство с именем ID, но это не означает, что он является первичным ключом).</span><span class="sxs-lookup"><span data-stu-id="a8950-178">There are some scenarios where Code First will not detect a complex type (for example, if you do have a property called ID, but you do not mean for it to be a primary key).</span></span> <span data-ttu-id="a8950-179">В таких случаях следует использовать API Fluent, чтобы явно указать, что тип является сложным типом.</span><span class="sxs-lookup"><span data-stu-id="a8950-179">In such cases, you would use the fluent API to explicitly specify that a type is a complex type.</span></span>  

``` csharp
modelBuilder.ComplexType<Details>();
```  

### <a name="specifying-not-to-map-a-clr-entity-type-to-a-table-in-the-database"></a><span data-ttu-id="a8950-180">Указание не сопоставлять тип сущности CLR с таблицей в базе данных</span><span class="sxs-lookup"><span data-stu-id="a8950-180">Specifying Not to Map a CLR Entity Type to a Table in the Database</span></span>  

<span data-ttu-id="a8950-181">В следующем примере показано, как исключить тип CLR из сопоставления с таблицей в базе данных.</span><span class="sxs-lookup"><span data-stu-id="a8950-181">The following example shows how to exclude a CLR type from being mapped to a table in the database.</span></span>  

``` csharp
modelBuilder.Ignore<OnlineCourse>();
```  

### <a name="mapping-an-entity-type-to-a-specific-table-in-the-database"></a><span data-ttu-id="a8950-182">Сопоставление типа сущности с определенной таблицей в базе данных</span><span class="sxs-lookup"><span data-stu-id="a8950-182">Mapping an Entity Type to a Specific Table in the Database</span></span>  

<span data-ttu-id="a8950-183">Все свойства отдела будут сопоставлены со столбцами в таблице с именем t_ Department.</span><span class="sxs-lookup"><span data-stu-id="a8950-183">All properties of Department will be mapped to columns in a table called t_ Department.</span></span>  

``` csharp
modelBuilder.Entity<Department>()  
    .ToTable("t_Department");
```  

<span data-ttu-id="a8950-184">Также можно указать имя схемы следующим образом:</span><span class="sxs-lookup"><span data-stu-id="a8950-184">You can also specify the schema name like this:</span></span>  

``` csharp
modelBuilder.Entity<Department>()  
    .ToTable("t_Department", "school");
```  

### <a name="mapping-the-table-per-hierarchy-tph-inheritance"></a><span data-ttu-id="a8950-185">Сопоставление наследования «одна таблица на иерархию»</span><span class="sxs-lookup"><span data-stu-id="a8950-185">Mapping the Table-Per-Hierarchy (TPH) Inheritance</span></span>  

<span data-ttu-id="a8950-186">В сценарии сопоставления «одна таблица» все типы в иерархии наследования сопоставлены с одной таблицей.</span><span class="sxs-lookup"><span data-stu-id="a8950-186">In the TPH mapping scenario, all types in an inheritance hierarchy are mapped to a single table.</span></span> <span data-ttu-id="a8950-187">Столбец дискриминатора используется для задания типа каждой строки.</span><span class="sxs-lookup"><span data-stu-id="a8950-187">A discriminator column is used to identify the type of each row.</span></span> <span data-ttu-id="a8950-188">При создании модели с помощью Code First ИЕРАРХИя является стратегией по умолчанию для типов, участвующих в иерархии наследования.</span><span class="sxs-lookup"><span data-stu-id="a8950-188">When creating your model with Code First, TPH is the default strategy for the types that participate in the inheritance hierarchy.</span></span> <span data-ttu-id="a8950-189">По умолчанию столбец дискриминатора добавляется в таблицу с именем "дискриминатор", а имя типа CLR каждого типа в иерархии используется для значений дискриминатора.</span><span class="sxs-lookup"><span data-stu-id="a8950-189">By default, the discriminator column is added to the table with the name “Discriminator” and the CLR type name of each type in the hierarchy is used for the discriminator values.</span></span> <span data-ttu-id="a8950-190">Поведение по умолчанию можно изменить с помощью API-интерфейса Fluent.</span><span class="sxs-lookup"><span data-stu-id="a8950-190">You can modify the default behavior by using the fluent API.</span></span>  

``` csharp
modelBuilder.Entity<Course>()  
    .Map<Course>(m => m.Requires("Type").HasValue("Course"))  
    .Map<OnsiteCourse>(m => m.Requires("Type").HasValue("OnsiteCourse"));
```  

### <a name="mapping-the-table-per-type-tpt-inheritance"></a><span data-ttu-id="a8950-191">Сопоставление наследования типа «таблица на тип» (TPT)</span><span class="sxs-lookup"><span data-stu-id="a8950-191">Mapping the Table-Per-Type (TPT) Inheritance</span></span>  

<span data-ttu-id="a8950-192">В сценарии сопоставления TPT все типы сопоставлены с отдельными таблицами.</span><span class="sxs-lookup"><span data-stu-id="a8950-192">In the TPT mapping scenario, all types are mapped to individual tables.</span></span> <span data-ttu-id="a8950-193">Как свойства базового типа, так и свойства производного типа хранятся в таблице, сопоставленной с этим типом.</span><span class="sxs-lookup"><span data-stu-id="a8950-193">Properties that belong solely to a base type or derived type are stored in a table that maps to that type.</span></span> <span data-ttu-id="a8950-194">Таблицы, которые сопоставляются с производными типами, также хранят внешний ключ, который соединяет производную таблицу с базовой таблицей.</span><span class="sxs-lookup"><span data-stu-id="a8950-194">Tables that map to derived types also store a foreign key that joins the derived table with the base table.</span></span>  

``` csharp
modelBuilder.Entity<Course>().ToTable("Course");  
modelBuilder.Entity<OnsiteCourse>().ToTable("OnsiteCourse");
```  

### <a name="mapping-the-table-per-concrete-class-tpc-inheritance"></a><span data-ttu-id="a8950-195">Сопоставление наследования класса "Таблица — конкретный класс" (TPC)</span><span class="sxs-lookup"><span data-stu-id="a8950-195">Mapping the Table-Per-Concrete Class (TPC) Inheritance</span></span>  

<span data-ttu-id="a8950-196">В сценарии сопоставления TPC все неабстрактные типы в иерархии сопоставляются с отдельными таблицами.</span><span class="sxs-lookup"><span data-stu-id="a8950-196">In the TPC mapping scenario, all non-abstract types in the hierarchy are mapped to individual tables.</span></span> <span data-ttu-id="a8950-197">Таблицы, сопоставленные с производными классами, не имеют связи с таблицей, сопоставленной с базовым классом в базе данных.</span><span class="sxs-lookup"><span data-stu-id="a8950-197">The tables that map to the derived classes have no relationship to the table that maps to the base class in the database.</span></span> <span data-ttu-id="a8950-198">Все свойства класса, включая унаследованные свойства, сопоставляются со столбцами соответствующей таблицы.</span><span class="sxs-lookup"><span data-stu-id="a8950-198">All properties of a class, including inherited properties, are mapped to columns of the corresponding table.</span></span>  

<span data-ttu-id="a8950-199">Вызовите метод Мапинхеритедпропертиес, чтобы настроить каждый производный тип.</span><span class="sxs-lookup"><span data-stu-id="a8950-199">Call the MapInheritedProperties method to configure each derived type.</span></span> <span data-ttu-id="a8950-200">Мапинхеритедпропертиес повторно сопоставляет все свойства, унаследованные от базового класса, с новыми столбцами в таблице для производного класса.</span><span class="sxs-lookup"><span data-stu-id="a8950-200">MapInheritedProperties remaps all properties that were inherited from the base class to new columns in the table for the derived class.</span></span>  

> [!NOTE]
> <span data-ttu-id="a8950-201">Обратите внимание, что поскольку таблицы, участвующие в иерархии наследования TPC, не используют первичный ключ, при вставке в таблицы, сопоставленные с подклассами, будут использоваться дубликаты ключей сущностей, если созданные базой данных значения имеют одинаковое начальное значение идентификатора.</span><span class="sxs-lookup"><span data-stu-id="a8950-201">Note that because the tables participating in TPC inheritance hierarchy do not share a primary key there will be duplicate entity keys when inserting in tables that are mapped to subclasses if you have database generated values with the same identity seed.</span></span> <span data-ttu-id="a8950-202">Чтобы решить эту проблему, можно указать другое начальное начальное значение для каждой таблицы или отключить удостоверение в свойстве первичного ключа.</span><span class="sxs-lookup"><span data-stu-id="a8950-202">To solve this problem you can either specify a different initial seed value for each table or switch off identity on the primary key property.</span></span> <span data-ttu-id="a8950-203">Identity — это значение по умолчанию для целочисленных свойств ключа при работе с Code First.</span><span class="sxs-lookup"><span data-stu-id="a8950-203">Identity is the default value for integer key properties when working with Code First.</span></span>  

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

### <a name="mapping-properties-of-an-entity-type-to-multiple-tables-in-the-database-entity-splitting"></a><span data-ttu-id="a8950-204">Сопоставление свойств типа сущности с несколькими таблицами в базе данных (разбиение сущностей)</span><span class="sxs-lookup"><span data-stu-id="a8950-204">Mapping Properties of an Entity Type to Multiple Tables in the Database (Entity Splitting)</span></span>  

<span data-ttu-id="a8950-205">Разбиение сущностей позволяет распределять свойства типа сущности по нескольким таблицам.</span><span class="sxs-lookup"><span data-stu-id="a8950-205">Entity splitting allows the properties of an entity type to be spread across multiple tables.</span></span> <span data-ttu-id="a8950-206">В следующем примере сущность отделов разбивается на две таблицы: Department и Департментдетаилс.</span><span class="sxs-lookup"><span data-stu-id="a8950-206">In the following example, the Department entity is split into two tables: Department and DepartmentDetails.</span></span> <span data-ttu-id="a8950-207">При разбиении сущностей используется несколько вызовов метода Map для соответствия подмножества свойств определенной таблице.</span><span class="sxs-lookup"><span data-stu-id="a8950-207">Entity splitting uses multiple calls to the Map method to map a subset of properties to a specific table.</span></span>  

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

### <a name="mapping-multiple-entity-types-to-one-table-in-the-database-table-splitting"></a><span data-ttu-id="a8950-208">Сопоставление нескольких типов сущностей с одной таблицей в базе данных (разделение таблицы)</span><span class="sxs-lookup"><span data-stu-id="a8950-208">Mapping Multiple Entity Types to One Table in the Database (Table Splitting)</span></span>  

<span data-ttu-id="a8950-209">В следующем примере сопоставляются два типа сущностей, которые совместно используют первичный ключ с одной таблицей.</span><span class="sxs-lookup"><span data-stu-id="a8950-209">The following example maps two entity types that share a primary key to one table.</span></span>  

``` csharp
modelBuilder.Entity<OfficeAssignment>()
    .HasKey(t => t.InstructorID);

modelBuilder.Entity<Instructor>()
    .HasRequired(t => t.OfficeAssignment)
    .WithRequiredPrincipal(t => t.Instructor);

modelBuilder.Entity<Instructor>().ToTable("Instructor");

modelBuilder.Entity<OfficeAssignment>().ToTable("Instructor");
```  

### <a name="mapping-an-entity-type-to-insertupdatedelete-stored-procedures-ef6-onwards"></a><span data-ttu-id="a8950-210">Сопоставление типа сущности для вставки, обновления и удаления хранимых процедур (EF6)</span><span class="sxs-lookup"><span data-stu-id="a8950-210">Mapping an Entity Type to Insert/Update/Delete Stored Procedures (EF6 onwards)</span></span>  

<span data-ttu-id="a8950-211">Начиная с EF6 можно сопоставлять сущности для использования хранимых процедур для вставки обновлений и удаления.</span><span class="sxs-lookup"><span data-stu-id="a8950-211">Starting with EF6 you can map an entity to use stored procedures for insert update and delete.</span></span> <span data-ttu-id="a8950-212">Дополнительные сведения см. в разделе [Code First вставки, обновления и удаления хранимых процедур](xref:ef6/modeling/code-first/fluent/cud-stored-procedures).</span><span class="sxs-lookup"><span data-stu-id="a8950-212">For more details see, [Code First Insert/Update/Delete Stored Procedures](xref:ef6/modeling/code-first/fluent/cud-stored-procedures).</span></span>  

## <a name="model-used-in-samples"></a><span data-ttu-id="a8950-213">Модель, используемая в примерах</span><span class="sxs-lookup"><span data-stu-id="a8950-213">Model Used in Samples</span></span>  

<span data-ttu-id="a8950-214">Для примеров на этой странице используется следующая Code First модель.</span><span class="sxs-lookup"><span data-stu-id="a8950-214">The following Code First model is used for the samples on this page.</span></span>  

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
