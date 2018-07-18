---
title: Fluent API — Настройка и сопоставление типов и свойств — EF6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: 648ed274-c501-4630-88e0-d728ab5c4057
caps.latest.revision: 3
ms.openlocfilehash: ec8b484433d13899a88f44e37823dd1a4bed6530
ms.sourcegitcommit: 390f3a37bc55105ed7cc5b0e0925b7f9c9e80ba6
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/09/2018
ms.locfileid: "39122614"
---
# <a name="fluent-api---configuring-and-mapping-properties-and-types"></a><span data-ttu-id="1aae3-102">Fluent API — Настройка и сопоставление типов и свойств</span><span class="sxs-lookup"><span data-stu-id="1aae3-102">Fluent API - Configuring and Mapping Properties and Types</span></span>
<span data-ttu-id="1aae3-103">При работе с Entity Framework Code First поведение по умолчанию — классы POCO сопоставляются с таблицами, используя набор соглашений, помещенного в EF.</span><span class="sxs-lookup"><span data-stu-id="1aae3-103">When working with Entity Framework Code First the default behavior is to map your POCO classes to tables using a set of conventions baked into EF.</span></span> <span data-ttu-id="1aae3-104">Иногда тем не менее, нельзя или нежелательно выполните эти соглашения и нужно сопоставить сущности с отличные от контекстам.</span><span class="sxs-lookup"><span data-stu-id="1aae3-104">Sometimes, however, you cannot or do not want to follow those conventions and need to map entities to something other than what the conventions dictate.</span></span>  

<span data-ttu-id="1aae3-105">Существует два основных способа, можно настроить EF использовать отличные от соглашений, а именно [заметки](~/ef6/modeling/code-first/data-annotations.md) или текучего API файловой системы EFs.</span><span class="sxs-lookup"><span data-stu-id="1aae3-105">There are two main ways you can configure EF to use something other than conventions, namely [annotations](~/ef6/modeling/code-first/data-annotations.md) or EFs fluent API.</span></span> <span data-ttu-id="1aae3-106">Заметки только охватывает часть возможностей, текучего API, поэтому существуют сценарии сопоставления, которые невозможно сделать с помощью заметок.</span><span class="sxs-lookup"><span data-stu-id="1aae3-106">The annotations only cover a subset of the fluent API functionality, so there are mapping scenarios that cannot be achieved using annotations.</span></span> <span data-ttu-id="1aae3-107">Эта статья предназначена для демонстрации использовать текучий API для настройки свойств.</span><span class="sxs-lookup"><span data-stu-id="1aae3-107">This article is designed to demonstrate how to use the fluent API to configure properties.</span></span>  

<span data-ttu-id="1aae3-108">Fluent API для code first чаще всего осуществляется путем переопределения [OnModelCreating](https://msdn.microsoft.com/library/system.data.entity.dbcontext.onmodelcreating.aspx) метод в производном [DbContext](https://msdn.microsoft.com/library/system.data.entity.dbcontext.aspx).</span><span class="sxs-lookup"><span data-stu-id="1aae3-108">The code first fluent API is most commonly accessed by overriding the [OnModelCreating](https://msdn.microsoft.com/library/system.data.entity.dbcontext.onmodelcreating.aspx) method on your derived [DbContext](https://msdn.microsoft.com/library/system.data.entity.dbcontext.aspx).</span></span> <span data-ttu-id="1aae3-109">Приведенные ниже примеры предназначены для показано, как выполнять различные задачи с помощью текучего api и скопируйте код и настроить его в соответствии с вашей модели, если вы хотите увидеть модель, они могут использоваться в качестве-то он предоставляется в конце этой статьи.</span><span class="sxs-lookup"><span data-stu-id="1aae3-109">The following samples are designed to show how to do various tasks with the fluent api and allow you to copy the code out and customize it to suit your model, if you wish to see the model that they can be used with as-is then it is provided at the end of this article.</span></span>  

## <a name="model-wide-settings"></a><span data-ttu-id="1aae3-110">Параметры на уровне модели</span><span class="sxs-lookup"><span data-stu-id="1aae3-110">Model-Wide Settings</span></span>  

### <a name="default-schema-ef6-onwards"></a><span data-ttu-id="1aae3-111">Схема по умолчанию (для EF6 и выше)</span><span class="sxs-lookup"><span data-stu-id="1aae3-111">Default Schema (EF6 onwards)</span></span>  

<span data-ttu-id="1aae3-112">Начиная с EF6 можно использовать HasDefaultSchema метод на DbModelBuilder для указания схемы базы данных для таблиц, хранимых процедур, и т.д. Этот параметр по умолчанию будет переопределен для любых объектов, которые явным образом настроить другую схему для.</span><span class="sxs-lookup"><span data-stu-id="1aae3-112">Starting with EF6 you can use the HasDefaultSchema method on DbModelBuilder to specify the database schema to use for all tables, stored procedures, etc. This default setting will be overridden for any objects that you explicitly configure a different schema for.</span></span>  

``` csharp
modelBuilder.HasDefaultSchema(“sales”);
```  

### <a name="custom-conventions-ef6-onwards"></a><span data-ttu-id="1aae3-113">Соглашения об именовании (для EF6 и выше)</span><span class="sxs-lookup"><span data-stu-id="1aae3-113">Custom Conventions (EF6 onwards)</span></span>  

<span data-ttu-id="1aae3-114">Начиная с EF6, можно создать собственные правила, чтобы дополнить тех, которые включены в Code First.</span><span class="sxs-lookup"><span data-stu-id="1aae3-114">Starting with EF6 you can create your own conventions to supplement the ones included in Code First.</span></span> <span data-ttu-id="1aae3-115">Дополнительные сведения см. в разделе [первый соглашения о написании пользовательского кода](~/ef6/modeling/code-first/conventions/custom.md).</span><span class="sxs-lookup"><span data-stu-id="1aae3-115">For more details, see [Custom Code First Conventions](~/ef6/modeling/code-first/conventions/custom.md).</span></span>  

## <a name="property-mapping"></a><span data-ttu-id="1aae3-116">Сопоставление свойств</span><span class="sxs-lookup"><span data-stu-id="1aae3-116">Property Mapping</span></span>  

<span data-ttu-id="1aae3-117">[Свойство](https://msdn.microsoft.com/library/system.data.entity.infrastructure.dbentityentry.property.aspx) метод используется для настройки атрибутов для каждого свойства, относящиеся к сущности или сложного типа.</span><span class="sxs-lookup"><span data-stu-id="1aae3-117">The [Property](https://msdn.microsoft.com/library/system.data.entity.infrastructure.dbentityentry.property.aspx) method is used to configure attributes for each property belonging to an entity or complex type.</span></span> <span data-ttu-id="1aae3-118">Метод свойства используется для получения объекта конфигурации для заданного свойства.</span><span class="sxs-lookup"><span data-stu-id="1aae3-118">The Property method is used to obtain a configuration object for a given property.</span></span> <span data-ttu-id="1aae3-119">Параметры в объекте конфигурации используются только в настраиваемый; тип Например IsUnicode доступны только для свойства строки.</span><span class="sxs-lookup"><span data-stu-id="1aae3-119">The options on the configuration object are specific to the type being configured; IsUnicode is available only on string properties for example.</span></span>  

### <a name="configuring-a-primary-key"></a><span data-ttu-id="1aae3-120">Настройка первичного ключа</span><span class="sxs-lookup"><span data-stu-id="1aae3-120">Configuring a Primary Key</span></span>  

<span data-ttu-id="1aae3-121">Действует соглашение, Entity Framework для первичных ключей:</span><span class="sxs-lookup"><span data-stu-id="1aae3-121">The Entity Framework convention for primary keys is:</span></span>  

1. <span data-ttu-id="1aae3-122">Ваш класс определяет свойство, имя которого является «ID» или «Id»</span><span class="sxs-lookup"><span data-stu-id="1aae3-122">Your class defines a property whose name is “ID” or “Id”</span></span>  
2. <span data-ttu-id="1aae3-123">или имя класса, за которым следует «ID» или «Id»</span><span class="sxs-lookup"><span data-stu-id="1aae3-123">or a class name followed by “ID” or “Id”</span></span>  

<span data-ttu-id="1aae3-124">Чтобы явно задать свойство в качестве первичного ключа, можно использовать метод HasKey.</span><span class="sxs-lookup"><span data-stu-id="1aae3-124">To explicitly set a property to be a primary key, you can use the HasKey method.</span></span> <span data-ttu-id="1aae3-125">В следующем примере метод HasKey используется для настройки InstructorID первичный ключ типа OfficeAssignment.</span><span class="sxs-lookup"><span data-stu-id="1aae3-125">In the following example, the HasKey method is used to configure the InstructorID primary key on the OfficeAssignment type.</span></span>  

``` csharp
modelBuilder.Entity<OfficeAssignment>().HasKey(t => t.InstructorID);
```  

### <a name="configuring-a-composite-primary-key"></a><span data-ttu-id="1aae3-126">Настройка составного первичного ключа</span><span class="sxs-lookup"><span data-stu-id="1aae3-126">Configuring a Composite Primary Key</span></span>  

<span data-ttu-id="1aae3-127">В следующем примере настраивается свойств DepartmentID и имя составного первичного ключа типа отдела.</span><span class="sxs-lookup"><span data-stu-id="1aae3-127">The following example configures the DepartmentID and Name properties to be the composite primary key of the Department type.</span></span>  

``` csharp
modelBuilder.Entity<Department>().HasKey(t => new { t.DepartmentID, t.Name });
```  

### <a name="switching-off-identity-for-numeric-primary-keys"></a><span data-ttu-id="1aae3-128">Выключение идентификации для числовых первичных ключей</span><span class="sxs-lookup"><span data-stu-id="1aae3-128">Switching off Identity for Numeric Primary Keys</span></span>  

<span data-ttu-id="1aae3-129">В следующем примере свойство DepartmentID для System.ComponentModel.DataAnnotations.DatabaseGeneratedOption.None, чтобы указать, что значение не создается в базе данных.</span><span class="sxs-lookup"><span data-stu-id="1aae3-129">The following example sets the DepartmentID property to System.ComponentModel.DataAnnotations.DatabaseGeneratedOption.None to indicate that the value will not be generated by the database.</span></span>  

``` csharp
modelBuilder.Entity<Department>().Property(t => t.DepartmentID)
    .HasDatabaseGeneratedOption(DatabaseGeneratedOption.None);
```  

### <a name="specifying-the-maximum-length-on-a-property"></a><span data-ttu-id="1aae3-130">Указание Максимальная длина свойства</span><span class="sxs-lookup"><span data-stu-id="1aae3-130">Specifying the Maximum Length on a Property</span></span>  

<span data-ttu-id="1aae3-131">В следующем примере свойство Name должно быть не более 50 символов.</span><span class="sxs-lookup"><span data-stu-id="1aae3-131">In the following example, the Name property should be no longer than 50 characters.</span></span> <span data-ttu-id="1aae3-132">Если значение длиннее 50 символов, вы получите [DbEntityValidationException](https://msdn.microsoft.com/library/system.data.entity.validation.dbentityvalidationexception.aspx) исключение.</span><span class="sxs-lookup"><span data-stu-id="1aae3-132">If you make the value longer than 50 characters, you will get a [DbEntityValidationException](https://msdn.microsoft.com/library/system.data.entity.validation.dbentityvalidationexception.aspx) exception.</span></span> <span data-ttu-id="1aae3-133">Если Code First создает базу данных из этой модели она будет также задан Максимальная длина имени столбца до 50 символов.</span><span class="sxs-lookup"><span data-stu-id="1aae3-133">If Code First creates a database from this model it will also set the maximum length of the Name column to 50 characters.</span></span>  

``` csharp
modelBuilder.Entity<Department>().Property(t => t.Name).HasMaxLength(50);
```  

### <a name="configuring-the-property-to-be-required"></a><span data-ttu-id="1aae3-134">Настройка свойства обязательным</span><span class="sxs-lookup"><span data-stu-id="1aae3-134">Configuring the Property to be Required</span></span>  

<span data-ttu-id="1aae3-135">В следующем примере свойство Name является обязательным.</span><span class="sxs-lookup"><span data-stu-id="1aae3-135">In the following example, the Name property is required.</span></span> <span data-ttu-id="1aae3-136">Если вы не укажете имя, вы получите исключение DbEntityValidationException.</span><span class="sxs-lookup"><span data-stu-id="1aae3-136">If you do not specify the Name, you will get a DbEntityValidationException exception.</span></span> <span data-ttu-id="1aae3-137">Если Code First создает базу данных из этой модели выберите столбец, используемый для хранения этого свойства обычно будет не допускающие значения NULL.</span><span class="sxs-lookup"><span data-stu-id="1aae3-137">If Code First creates a database from this model then the column used to store this property will usually be non-nullable.</span></span>  

> [!NOTE]
> <span data-ttu-id="1aae3-138">В некоторых случаях может оказаться невозможным для столбца в базе данных и не допускало, несмотря на то, что свойство является обязательным.</span><span class="sxs-lookup"><span data-stu-id="1aae3-138">In some cases it may not be possible for the column in the database to be non-nullable even though the property is required.</span></span> <span data-ttu-id="1aae3-139">Например, когда с помощью данных стратегии наследование TPH для нескольких типов хранится в одной таблице.</span><span class="sxs-lookup"><span data-stu-id="1aae3-139">For example, when using a TPH inheritance strategy data for multiple types is stored in a single table.</span></span> <span data-ttu-id="1aae3-140">Если производный тип содержит свойство столбца невозможно не допускающие значения NULL, так как не все типы в иерархии, это свойство будет задано.</span><span class="sxs-lookup"><span data-stu-id="1aae3-140">If a derived type includes a required property the column cannot be made non-nullable since not all types in the hierarchy will have this property.</span></span>  

``` csharp
modelBuilder.Entity<Department>().Property(t => t.Name).IsRequired();
```  

### <a name="configuring-an-index-on-one-or-more-properties"></a><span data-ttu-id="1aae3-141">Настройка индекса для одного или нескольких свойств</span><span class="sxs-lookup"><span data-stu-id="1aae3-141">Configuring an Index on one or more properties</span></span>  

> [!NOTE]
> <span data-ttu-id="1aae3-142">**EF6.1 и более поздних версий только** -индекс, представляющий атрибут появился в Entity Framework 6.1.</span><span class="sxs-lookup"><span data-stu-id="1aae3-142">**EF6.1 Onwards Only** - The Index attribute was introduced in Entity Framework 6.1.</span></span> <span data-ttu-id="1aae3-143">При использовании более ранней версии сведения в этом разделе не применяется.</span><span class="sxs-lookup"><span data-stu-id="1aae3-143">If you are using an earlier version the information in this section does not apply.</span></span>  

<span data-ttu-id="1aae3-144">Создание индексов не поддерживается в собственном коде Fluent API-интерфейсом, но чтобы использовать поддержку **IndexAttribute** через Fluent API.</span><span class="sxs-lookup"><span data-stu-id="1aae3-144">Creating indexes isn't natively supported by the Fluent API, but you can make use of the support for **IndexAttribute** via the Fluent API.</span></span> <span data-ttu-id="1aae3-145">Атрибуты индекса обрабатываются путем включения заметку в виде модели на модель, которая затем преобразуется в индекс в базе данных далее в конвейере.</span><span class="sxs-lookup"><span data-stu-id="1aae3-145">Index attributes are processed by including a model annotation on the model that is then turned into an Index in the database later in the pipeline.</span></span> <span data-ttu-id="1aae3-146">Можно вручную добавить эти же заметок с помощью Fluent API.</span><span class="sxs-lookup"><span data-stu-id="1aae3-146">You can manually add these same annotations using the Fluent API.</span></span>  

<span data-ttu-id="1aae3-147">Для этого проще всего создать экземпляр **IndexAttribute** , содержащий все параметры для нового индекса.</span><span class="sxs-lookup"><span data-stu-id="1aae3-147">The easiest way to do this is to create an instance of **IndexAttribute** that contains all the settings for the new index.</span></span> <span data-ttu-id="1aae3-148">Затем можно создать экземпляр **IndexAnnotation** это определенного типа EF, которое преобразует **IndexAttribute** параметры в заметку в виде модели, которые могут быть сохранены на модели EF.</span><span class="sxs-lookup"><span data-stu-id="1aae3-148">You can then create an instance of **IndexAnnotation** which is an EF specific type that will convert the **IndexAttribute** settings into a model annotation that can be stored on the EF model.</span></span> <span data-ttu-id="1aae3-149">Это может быть передан **HasColumnAnnotation** метод Fluent API-интерфейсы, указав имя **индекс** для заметки.</span><span class="sxs-lookup"><span data-stu-id="1aae3-149">These can then be passed to the **HasColumnAnnotation** method on the Fluent API, specifying the name **Index** for the annotation.</span></span>  

``` csharp
modelBuilder
    .Entity<Department>()
    .Property(t => t.Name)
    .HasColumnAnnotation("Index", new IndexAnnotation(new IndexAttribute()));
```  

<span data-ttu-id="1aae3-150">Полный список параметров, доступных в **IndexAttribute**, см. в разделе *индекс* раздел [заметки данных Code First](~/ef6/modeling/code-first/data-annotations.md).</span><span class="sxs-lookup"><span data-stu-id="1aae3-150">For a complete list of the settings available in **IndexAttribute**, see the *Index* section of [Code First Data Annotations](~/ef6/modeling/code-first/data-annotations.md).</span></span> <span data-ttu-id="1aae3-151">Сюда входят Настройка имя индекса, создание уникальных индексов и создания индексы с несколькими столбцами.</span><span class="sxs-lookup"><span data-stu-id="1aae3-151">This includes customizing the index name, creating unique indexes, and creating multi-column indexes.</span></span>  

<span data-ttu-id="1aae3-152">Можно указать несколько заметок индекса по одному свойству, передавая ему массив **IndexAttribute** конструктору **IndexAnnotation**.</span><span class="sxs-lookup"><span data-stu-id="1aae3-152">You can specify multiple index annotations on a single property by passing an array of **IndexAttribute** to the constructor of **IndexAnnotation**.</span></span>  

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

### <a name="specifying-not-to-map-a-clr-property-to-a-column-in-the-database"></a><span data-ttu-id="1aae3-153">Указание не для сопоставления свойства CLR со столбцом в базе данных</span><span class="sxs-lookup"><span data-stu-id="1aae3-153">Specifying Not to Map a CLR Property to a Column in the Database</span></span>  

<span data-ttu-id="1aae3-154">Приведенный ниже показано, как указать, что свойство на основе типа CLR не сопоставлен со столбцом в базе данных.</span><span class="sxs-lookup"><span data-stu-id="1aae3-154">The following example shows how to specify that a property on a CLR type is not mapped to a column in the database.</span></span>  

``` csharp
modelBuilder.Entity<Department>().Ignore(t => t.Budget);
```  

### <a name="mapping-a-clr-property-to-a-specific-column-in-the-database"></a><span data-ttu-id="1aae3-155">Сопоставление свойства CLR для заданного столбца в базе данных</span><span class="sxs-lookup"><span data-stu-id="1aae3-155">Mapping a CLR Property to a Specific Column in the Database</span></span>  

<span data-ttu-id="1aae3-156">Следующий пример сопоставляет свойство CLR имя столбца базы данных «отдел».</span><span class="sxs-lookup"><span data-stu-id="1aae3-156">The following example maps the Name CLR property to the DepartmentName database column.</span></span>  

``` csharp
modelBuilder.Entity<Department>()
    .Property(t => t.Name)
    .HasColumnName("DepartmentName");
```  

### <a name="renaming-a-foreign-key-that-is-not-defined-in-the-model"></a><span data-ttu-id="1aae3-157">Переименование внешний ключ, который не определен в модели</span><span class="sxs-lookup"><span data-stu-id="1aae3-157">Renaming a Foreign Key That Is Not Defined in the Model</span></span>  

<span data-ttu-id="1aae3-158">Если вы не захотите определения внешнего ключа на основе типа CLR, но, чтобы задать имя, она должна иметь в базе данных, сделайте следующее:</span><span class="sxs-lookup"><span data-stu-id="1aae3-158">If you choose not to define a foreign key on a CLR type, but want to specify what name it should have in the database, do the following:</span></span>  

``` csharp
modelBuilder.Entity<Course>()
    .HasRequired(c => c.Department)
    .WithMany(t => t.Courses)
    .Map(m => m.MapKey("ChangedDepartmentID"));
```  

### <a name="configuring-whether-a-string-property-supports-unicode-content"></a><span data-ttu-id="1aae3-159">Настройка ли строковое свойство поддерживать содержимое в Юникоде</span><span class="sxs-lookup"><span data-stu-id="1aae3-159">Configuring whether a String Property Supports Unicode Content</span></span>  

<span data-ttu-id="1aae3-160">По умолчанию строки являются Юникода (nvarchar в SQL Server).</span><span class="sxs-lookup"><span data-stu-id="1aae3-160">By default strings are Unicode (nvarchar in SQL Server).</span></span> <span data-ttu-id="1aae3-161">Чтобы указать, что строка должна иметь тип varchar, можно использовать метод IsUnicode.</span><span class="sxs-lookup"><span data-stu-id="1aae3-161">You can use the IsUnicode method to specify that a string should be of varchar type.</span></span>  

``` csharp
modelBuilder.Entity<Department>()
    .Property(t => t.Name)
    .IsUnicode(false);
```  

### <a name="configuring-the-data-type-of-a-database-column"></a><span data-ttu-id="1aae3-162">Настройка типа данных столбца базы данных</span><span class="sxs-lookup"><span data-stu-id="1aae3-162">Configuring the Data Type of a Database Column</span></span>  

<span data-ttu-id="1aae3-163">[HasColumnType](https://msdn.microsoft.com/library/system.data.entity.modelconfiguration.configuration.stringcolumnconfiguration.hascolumntype.aspx) метод, который позволяет сопоставление с различными представлениями один и тот же базовый тип.</span><span class="sxs-lookup"><span data-stu-id="1aae3-163">The [HasColumnType](https://msdn.microsoft.com/library/system.data.entity.modelconfiguration.configuration.stringcolumnconfiguration.hascolumntype.aspx) method enables mapping to different representations of the same basic type.</span></span> <span data-ttu-id="1aae3-164">С помощью этого метода не позволяют выполнять любые преобразования данных во время выполнения.</span><span class="sxs-lookup"><span data-stu-id="1aae3-164">Using this method does not enable you to perform any conversion of the data at run time.</span></span> <span data-ttu-id="1aae3-165">Обратите внимание на то, что IsUnicode является предпочтительным способом настройки столбцов varchar, так как он зависит от базы данных.</span><span class="sxs-lookup"><span data-stu-id="1aae3-165">Note that IsUnicode is the preferred way of setting columns to varchar, as it is database agnostic.</span></span>  

``` csharp
modelBuilder.Entity<Department>()   
    .Property(p => p.Name)   
    .HasColumnType("varchar");
```  

### <a name="configuring-properties-on-a-complex-type"></a><span data-ttu-id="1aae3-166">Настройка свойств для сложного типа</span><span class="sxs-lookup"><span data-stu-id="1aae3-166">Configuring Properties on a Complex Type</span></span>  

<span data-ttu-id="1aae3-167">Существует два способа настройки скалярные свойства сложного типа.</span><span class="sxs-lookup"><span data-stu-id="1aae3-167">There are two ways to configure scalar properties on a complex type.</span></span>  

<span data-ttu-id="1aae3-168">ComplexTypeConfiguration можно вызвать свойство.</span><span class="sxs-lookup"><span data-stu-id="1aae3-168">You can call Property on ComplexTypeConfiguration.</span></span>  

``` csharp
modelBuilder.ComplexType<Details>()
    .Property(t => t.Location)
    .HasMaxLength(20);
```  

<span data-ttu-id="1aae3-169">Можно также использовать нотацию для доступа к свойству сложного типа.</span><span class="sxs-lookup"><span data-stu-id="1aae3-169">You can also use the dot notation to access a property of a complex type.</span></span>  

``` csharp
modelBuilder.Entity<OnsiteCourse>()
    .Property(t => t.Details.Location)
    .HasMaxLength(20);
```  

### <a name="configuring-a-property-to-be-used-as-an-optimistic-concurrency-token"></a><span data-ttu-id="1aae3-170">Настройка свойств для использования в качестве маркера оптимистичного параллелизма</span><span class="sxs-lookup"><span data-stu-id="1aae3-170">Configuring a Property to Be Used as an Optimistic Concurrency Token</span></span>  

<span data-ttu-id="1aae3-171">Чтобы указать, что свойство в сущности представляет маркер параллелизма, можно использовать атрибут ConcurrencyCheck или метод IsConcurrencyToken.</span><span class="sxs-lookup"><span data-stu-id="1aae3-171">To specify that a property in an entity represents a concurrency token, you can use either the ConcurrencyCheck attribute or the IsConcurrencyToken method.</span></span>  

``` csharp
modelBuilder.Entity<OfficeAssignment>()
    .Property(t => t.Timestamp)
    .IsConcurrencyToken();
```  

<span data-ttu-id="1aae3-172">Можно также использовать метод IsRowVersion свойство может быть версию строки в базе данных.</span><span class="sxs-lookup"><span data-stu-id="1aae3-172">You can also use the IsRowVersion method to configure the property to be a row version in the database.</span></span> <span data-ttu-id="1aae3-173">Задание свойства следует убедиться, что версия строки автоматически настраивает его, чтобы быть маркера оптимистичного параллелизма.</span><span class="sxs-lookup"><span data-stu-id="1aae3-173">Setting the property to be a row version automatically configures it to be an optimistic concurrency token.</span></span>  

``` csharp
modelBuilder.Entity<OfficeAssignment>()
    .Property(t => t.Timestamp)
    .IsRowVersion();
```  

## <a name="type-mapping"></a><span data-ttu-id="1aae3-174">Сопоставление типов</span><span class="sxs-lookup"><span data-stu-id="1aae3-174">Type Mapping</span></span>  

### <a name="specifying-that-a-class-is-a-complex-type"></a><span data-ttu-id="1aae3-175">Указывается, что класс является сложным типом</span><span class="sxs-lookup"><span data-stu-id="1aae3-175">Specifying That a Class Is a Complex Type</span></span>  

<span data-ttu-id="1aae3-176">По соглашению типом, который не имеет первичного ключа указан рассматривается как сложный тип.</span><span class="sxs-lookup"><span data-stu-id="1aae3-176">By convention, a type that has no primary key specified is treated as a complex type.</span></span> <span data-ttu-id="1aae3-177">Существуют некоторые сценарии, где Code First не обнаружит сложного типа (например, если у вас есть свойство с именем идентификатор, но не значит, для того, чтобы быть первичным ключом).</span><span class="sxs-lookup"><span data-stu-id="1aae3-177">There are some scenarios where Code First will not detect a complex type (for example, if you do have a property called ID, but you do not mean for it to be a primary key).</span></span> <span data-ttu-id="1aae3-178">В таких случаях используется fluent API для явного указания, что тип является сложным типом.</span><span class="sxs-lookup"><span data-stu-id="1aae3-178">In such cases, you would use the fluent API to explicitly specify that a type is a complex type.</span></span>  

``` csharp
modelBuilder.ComplexType<Details>();
```  

### <a name="specifying-not-to-map-a-clr-entity-type-to-a-table-in-the-database"></a><span data-ttu-id="1aae3-179">Указание не для того, чтобы сопоставить тип CLR сущности в таблицу в базе данных</span><span class="sxs-lookup"><span data-stu-id="1aae3-179">Specifying Not to Map a CLR Entity Type to a Table in the Database</span></span>  

<span data-ttu-id="1aae3-180">Приведенный ниже показано, как исключение из таблицы в базе данных, сопоставляемого типа CLR.</span><span class="sxs-lookup"><span data-stu-id="1aae3-180">The following example shows how to exclude a CLR type from being mapped to a table in the database.</span></span>  

``` csharp
modelBuilder.Ignore<OnlineCourse>();
```  

### <a name="mapping-an-entity-type-to-a-specific-table-in-the-database"></a><span data-ttu-id="1aae3-181">Сопоставление типа сущности в конкретную таблицу в базе данных</span><span class="sxs-lookup"><span data-stu-id="1aae3-181">Mapping an Entity Type to a Specific Table in the Database</span></span>  

<span data-ttu-id="1aae3-182">Все свойства отдела будет сопоставлено со столбцами в таблице, именуемой t_ отдела.</span><span class="sxs-lookup"><span data-stu-id="1aae3-182">All properties of Department will be mapped to columns in a table called t_ Department.</span></span>  

``` csharp
modelBuilder.Entity<Department>()  
    .ToTable("t_Department");
```  

<span data-ttu-id="1aae3-183">Можно также указать имя схемы следующим образом:</span><span class="sxs-lookup"><span data-stu-id="1aae3-183">You can also specify the schema name like this:</span></span>  

``` csharp
modelBuilder.Entity<Department>()  
    .ToTable("t_Department", "school");
```  

### <a name="mapping-the-table-per-hierarchy-tph-inheritance"></a><span data-ttu-id="1aae3-184">Сопоставление наследования одна таблица на иерархию (TPH)</span><span class="sxs-lookup"><span data-stu-id="1aae3-184">Mapping the Table-Per-Hierarchy (TPH) Inheritance</span></span>  

<span data-ttu-id="1aae3-185">В случае сопоставления TPH всех типов в иерархии наследования сопоставляются с одной таблицей.</span><span class="sxs-lookup"><span data-stu-id="1aae3-185">In the TPH mapping scenario, all types in an inheritance hierarchy are mapped to a single table.</span></span> <span data-ttu-id="1aae3-186">Столбец дискриминатора используется для определения типа каждой строки.</span><span class="sxs-lookup"><span data-stu-id="1aae3-186">A discriminator column is used to identify the type of each row.</span></span> <span data-ttu-id="1aae3-187">При создании модели с помощью Code First, TPH — это стратегия по умолчанию для типов, которые участвуют в иерархии наследования.</span><span class="sxs-lookup"><span data-stu-id="1aae3-187">When creating your model with Code First, TPH is the default strategy for the types that participate in the inheritance hierarchy.</span></span> <span data-ttu-id="1aae3-188">По умолчанию столбец дискриминатора добавляется в таблицу с именем «Дискриминатора» и имя типа CLR каждого типа в иерархии используется для значения дискриминатора.</span><span class="sxs-lookup"><span data-stu-id="1aae3-188">By default, the discriminator column is added to the table with the name “Discriminator” and the CLR type name of each type in the hierarchy is used for the discriminator values.</span></span> <span data-ttu-id="1aae3-189">Поведение по умолчанию можно изменить с помощью текучего API.</span><span class="sxs-lookup"><span data-stu-id="1aae3-189">You can modify the default behavior by using the fluent API.</span></span>  

``` csharp
modelBuilder.Entity<Course>()  
    .Map<Course>(m => m.Requires("Type").HasValue("Course"))  
    .Map<OnsiteCourse>(m => m.Requires("Type").HasValue("OnsiteCourse"));
```  

### <a name="mapping-the-table-per-type-tpt-inheritance"></a><span data-ttu-id="1aae3-190">Сопоставление наследования одна таблица на тип (TPT)</span><span class="sxs-lookup"><span data-stu-id="1aae3-190">Mapping the Table-Per-Type (TPT) Inheritance</span></span>  

<span data-ttu-id="1aae3-191">В сценарии сопоставления TPT все типы сопоставляются с отдельными таблицами.</span><span class="sxs-lookup"><span data-stu-id="1aae3-191">In the TPT mapping scenario, all types are mapped to individual tables.</span></span> <span data-ttu-id="1aae3-192">Как свойства базового типа, так и свойства производного типа хранятся в таблице, сопоставленной с этим типом.</span><span class="sxs-lookup"><span data-stu-id="1aae3-192">Properties that belong solely to a base type or derived type are stored in a table that maps to that type.</span></span> <span data-ttu-id="1aae3-193">Таблицы, которые сопоставляются производные типы также хранить внешний ключ, который соединяет производной таблицы с базовой таблицей.</span><span class="sxs-lookup"><span data-stu-id="1aae3-193">Tables that map to derived types also store a foreign key that joins the derived table with the base table.</span></span>  

``` csharp
modelBuilder.Entity<Course>().ToTable("Course");  
modelBuilder.Entity<OnsiteCourse>().ToTable("OnsiteCourse");
```  

### <a name="mapping-the-table-per-concrete-class-tpc-inheritance"></a><span data-ttu-id="1aae3-194">Сопоставление наследования таблица на конкретный класс (TPC)</span><span class="sxs-lookup"><span data-stu-id="1aae3-194">Mapping the Table-Per-Concrete Class (TPC) Inheritance</span></span>  

<span data-ttu-id="1aae3-195">В сценарии сопоставления TPC все не являющиеся абстрактными типы в иерархии сопоставляются с отдельными таблицами.</span><span class="sxs-lookup"><span data-stu-id="1aae3-195">In the TPC mapping scenario, all non-abstract types in the hierarchy are mapped to individual tables.</span></span> <span data-ttu-id="1aae3-196">Таблицы, которые сопоставляются производные классы имеют связана ни таблицей, которая сопоставляется с базовым классом в базе данных.</span><span class="sxs-lookup"><span data-stu-id="1aae3-196">The tables that map to the derived classes have no relationship to the table that maps to the base class in the database.</span></span> <span data-ttu-id="1aae3-197">Все свойства класса, включая унаследованные, сопоставляются столбцы соответствующей таблицы.</span><span class="sxs-lookup"><span data-stu-id="1aae3-197">All properties of a class, including inherited properties, are mapped to columns of the corresponding table.</span></span>  

<span data-ttu-id="1aae3-198">Вызовите метод MapInheritedProperties для настройки каждого производного типа.</span><span class="sxs-lookup"><span data-stu-id="1aae3-198">Call the MapInheritedProperties method to configure each derived type.</span></span> <span data-ttu-id="1aae3-199">MapInheritedProperties повторно отображает все свойства, унаследованные от базового класса в новые столбцы в таблице для производного класса.</span><span class="sxs-lookup"><span data-stu-id="1aae3-199">MapInheritedProperties remaps all properties that were inherited from the base class to new columns in the table for the derived class.</span></span>  

> [!NOTE]
> <span data-ttu-id="1aae3-200">Обратите внимание, что так как не используют совместно таблиц, участвующих в иерархии наследования TPC первичного ключа ключи повторяющиеся сущности при вставке в таблицах, сопоставленных с подклассами, если сформированный базой данных значения с тем же начальное значение идентификатора.</span><span class="sxs-lookup"><span data-stu-id="1aae3-200">Note that because the tables participating in TPC inheritance hierarchy do not share a primary key there will be duplicate entity keys when inserting in tables that are mapped to subclasses if you have database generated values with the same identity seed.</span></span> <span data-ttu-id="1aae3-201">Чтобы решить эту проблему, можно указать разные начальное значение для каждой таблицы или отключить удостоверения на свойство первичного ключа.</span><span class="sxs-lookup"><span data-stu-id="1aae3-201">To solve this problem you can either specify a different initial seed value for each table or switch off identity on the primary key property.</span></span> <span data-ttu-id="1aae3-202">Удостоверение значение по умолчанию для ключевых свойств целое число, при работе в режиме Code First.</span><span class="sxs-lookup"><span data-stu-id="1aae3-202">Identity is the default value for integer key properties when working with Code First.</span></span>  

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

### <a name="mapping-properties-of-an-entity-type-to-multiple-tables-in-the-database-entity-splitting"></a><span data-ttu-id="1aae3-203">Сопоставление свойств типа сущности с несколькими таблицами в базе данных (разбиение сущностей)</span><span class="sxs-lookup"><span data-stu-id="1aae3-203">Mapping Properties of an Entity Type to Multiple Tables in the Database (Entity Splitting)</span></span>  

<span data-ttu-id="1aae3-204">Разбиение сущностей позволяет свойства типа сущности и распределяются по нескольким таблицам.</span><span class="sxs-lookup"><span data-stu-id="1aae3-204">Entity splitting allows the properties of an entity type to be spread across multiple tables.</span></span> <span data-ttu-id="1aae3-205">В следующем примере сущность Department разбивается на две таблицы: отдела и DepartmentDetails.</span><span class="sxs-lookup"><span data-stu-id="1aae3-205">In the following example, the Department entity is split into two tables: Department and DepartmentDetails.</span></span> <span data-ttu-id="1aae3-206">Разделение сущностей использует несколько вызовов метода карты для сопоставления подмножества свойств в конкретную таблицу.</span><span class="sxs-lookup"><span data-stu-id="1aae3-206">Entity splitting uses multiple calls to the Map method to map a subset of properties to a specific table.</span></span>  

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

### <a name="mapping-multiple-entity-types-to-one-table-in-the-database-table-splitting"></a><span data-ttu-id="1aae3-207">Сопоставление нескольких типов сущностей к одной таблице в базе данных (разбиение таблиц)</span><span class="sxs-lookup"><span data-stu-id="1aae3-207">Mapping Multiple Entity Types to One Table in the Database (Table Splitting)</span></span>  

<span data-ttu-id="1aae3-208">Следующий пример сопоставляет двумя типами сущностей, которые совместно используют первичный ключ для одной таблицы.</span><span class="sxs-lookup"><span data-stu-id="1aae3-208">The following example maps two entity types that share a primary key to one table.</span></span>  

``` csharp
modelBuilder.Entity<OfficeAssignment>()
    .HasKey(t => t.InstructorID);

modelBuilder.Entity<Instructor>()
    .HasRequired(t => t.OfficeAssignment)
    .WithRequiredPrincipal(t => t.Instructor);

modelBuilder.Entity<Instructor>().ToTable("Instructor");

modelBuilder.Entity<OfficeAssignment>().ToTable("Instructor");
```  

### <a name="mapping-an-entity-type-to-insertupdatedelete-stored-procedures-ef6-onwards"></a><span data-ttu-id="1aae3-209">Сопоставление типа сущности с хранимые процедуры Insert/Update/Delete (для EF6 и выше)</span><span class="sxs-lookup"><span data-stu-id="1aae3-209">Mapping an Entity Type to Insert/Update/Delete Stored Procedures (EF6 onwards)</span></span>  

<span data-ttu-id="1aae3-210">Начиная с EF6 можно сопоставить сущность использовать хранимые процедуры для вставки обновления и удаления.</span><span class="sxs-lookup"><span data-stu-id="1aae3-210">Starting with EF6 you can map an entity to use stored procedures for insert update and delete.</span></span> <span data-ttu-id="1aae3-211">Дополнительные сведения см. в разделе, [код первого Insert/Update/Delete хранимые процедуры](~/ef6/modeling/code-first/fluent/cud-stored-procedures.md).</span><span class="sxs-lookup"><span data-stu-id="1aae3-211">For more details see, [Code First Insert/Update/Delete Stored Procedures](~/ef6/modeling/code-first/fluent/cud-stored-procedures.md).</span></span>  

## <a name="model-used-in-samples"></a><span data-ttu-id="1aae3-212">Модели, используемые в примерах</span><span class="sxs-lookup"><span data-stu-id="1aae3-212">Model Used in Samples</span></span>  

<span data-ttu-id="1aae3-213">Для примеров на этой странице используется следующая модель Code First.</span><span class="sxs-lookup"><span data-stu-id="1aae3-213">The following Code First model is used for the samples on this page.</span></span>  

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
