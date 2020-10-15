---
title: Спецификация MSL — EF6
description: Спецификация MSL в Entity Framework 6
author: ajcvickers
ms.date: 10/23/2016
uid: ef6/modeling/designer/advanced/edmx/msl-spec
ms.openlocfilehash: 0780a549d4cbb60bf84bbed927d52588e99148e1
ms.sourcegitcommit: 0a25c03fa65ae6e0e0e3f66bac48d59eceb96a5a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/14/2020
ms.locfileid: "92066347"
---
# <a name="msl-specification"></a><span data-ttu-id="f41c0-103">Спецификация MSL</span><span class="sxs-lookup"><span data-stu-id="f41c0-103">MSL Specification</span></span>
<span data-ttu-id="f41c0-104">Язык MSL — это язык на основе XML, который описывает сопоставление между концептуальной моделью и моделью хранения Entity Framework приложения.</span><span class="sxs-lookup"><span data-stu-id="f41c0-104">Mapping specification language (MSL) is an XML-based language that describes the mapping between the conceptual model and storage model of an Entity Framework application.</span></span>

<span data-ttu-id="f41c0-105">В приложении Entity Framework метаданные сопоставления загружаются из MSL-файла (написанного на языке MSL) во время сборки.</span><span class="sxs-lookup"><span data-stu-id="f41c0-105">In an Entity Framework application, mapping metadata is loaded from an .msl file (written in MSL) at build time.</span></span> <span data-ttu-id="f41c0-106">Entity Framework использует метаданные сопоставления во время выполнения для преобразования запросов к концептуальной модели для хранения команд, относящихся к хранению.</span><span class="sxs-lookup"><span data-stu-id="f41c0-106">Entity Framework uses mapping metadata at runtime to translate queries against the conceptual model to store-specific commands.</span></span>

<span data-ttu-id="f41c0-107">Entity Framework Designer (конструктор EF) сохраняет сведения о сопоставлении в EDMX-файле во время разработки.</span><span class="sxs-lookup"><span data-stu-id="f41c0-107">The Entity Framework Designer (EF Designer) stores mapping information in an .edmx file at design time.</span></span> <span data-ttu-id="f41c0-108">Во время сборки Entity Designer использует сведения в EDMX-файле для создания MSL-файла, необходимого для Entity Framework во время выполнения.</span><span class="sxs-lookup"><span data-stu-id="f41c0-108">At build time, the Entity Designer uses information in an .edmx file to create the .msl file that is needed by Entity Framework at runtime</span></span>

<span data-ttu-id="f41c0-109">Имена всех типов концептуальной модели или модели хранения, которые упоминаются в языке MSL, должны указываться вместе с именами соответствующих пространств имен.</span><span class="sxs-lookup"><span data-stu-id="f41c0-109">Names of all conceptual or storage model types that are referenced in MSL must be qualified by their respective namespace names.</span></span> <span data-ttu-id="f41c0-110">Дополнительные сведения об имени пространства имен концептуальной модели см. в разделе [Спецификация языка CSDL](xref:ef6/modeling/designer/advanced/edmx/csdl-spec).</span><span class="sxs-lookup"><span data-stu-id="f41c0-110">For information about the conceptual model namespace name, see [CSDL Specification](xref:ef6/modeling/designer/advanced/edmx/csdl-spec).</span></span> <span data-ttu-id="f41c0-111">Дополнительные сведения об имени пространства имен модели хранения см. в разделе [Спецификация языка SSDL](xref:ef6/modeling/designer/advanced/edmx/ssdl-spec).</span><span class="sxs-lookup"><span data-stu-id="f41c0-111">For information about the storage model namespace name, see [SSDL Specification](xref:ef6/modeling/designer/advanced/edmx/ssdl-spec).</span></span>

<span data-ttu-id="f41c0-112">Версии MSL отличаются по пространствам имен XML.</span><span class="sxs-lookup"><span data-stu-id="f41c0-112">Versions of MSL are differentiated by XML namespaces.</span></span>

| <span data-ttu-id="f41c0-113">Версия MSL</span><span class="sxs-lookup"><span data-stu-id="f41c0-113">MSL Version</span></span> | <span data-ttu-id="f41c0-114">Пространство имен XML</span><span class="sxs-lookup"><span data-stu-id="f41c0-114">XML Namespace</span></span>                                        |
|:------------|:-----------------------------------------------------|
| <span data-ttu-id="f41c0-115">MSL v1</span><span class="sxs-lookup"><span data-stu-id="f41c0-115">MSL v1</span></span>      | <span data-ttu-id="f41c0-116">urn: schemas-microsoft-com: Windows: хранилище: сопоставление: CS</span><span class="sxs-lookup"><span data-stu-id="f41c0-116">urn:schemas-microsoft-com:windows:storage:mapping:CS</span></span> |
| <span data-ttu-id="f41c0-117">MSL v2</span><span class="sxs-lookup"><span data-stu-id="f41c0-117">MSL v2</span></span>      | https://schemas.microsoft.com/ado/2008/09/mapping/cs |
| <span data-ttu-id="f41c0-118">MSL v3</span><span class="sxs-lookup"><span data-stu-id="f41c0-118">MSL v3</span></span>      | https://schemas.microsoft.com/ado/2009/11/mapping/cs  |

## <a name="alias-element-msl"></a><span data-ttu-id="f41c0-119">Элемент Alias (язык MSL)</span><span class="sxs-lookup"><span data-stu-id="f41c0-119">Alias Element (MSL)</span></span>

<span data-ttu-id="f41c0-120">Элемент **Alias** в языке MSL является дочерним по отношению к элементу Mapping, который используется для определения псевдонимов для концептуальной модели и пространств имен модели хранения.</span><span class="sxs-lookup"><span data-stu-id="f41c0-120">The **Alias** element in mapping specification language (MSL) is a child of the Mapping element that is used to define aliases for conceptual model and storage model namespaces.</span></span> <span data-ttu-id="f41c0-121">Имена всех типов концептуальной модели или модели хранения, которые упоминаются в языке MSL, должны указываться вместе с именами соответствующих пространств имен.</span><span class="sxs-lookup"><span data-stu-id="f41c0-121">Names of all conceptual or storage model types that are referenced in MSL must be qualified by their respective namespace names.</span></span> <span data-ttu-id="f41c0-122">Дополнительные сведения об имени пространства имен концептуальной модели см. в разделе элемент Schema (CSDL).</span><span class="sxs-lookup"><span data-stu-id="f41c0-122">For information about the conceptual model namespace name, see Schema Element (CSDL).</span></span> <span data-ttu-id="f41c0-123">Дополнительные сведения об имени пространства имен модели хранения см. в разделе элемент Schema (SSDL).</span><span class="sxs-lookup"><span data-stu-id="f41c0-123">For information about the storage model namespace name, see Schema Element (SSDL).</span></span>

<span data-ttu-id="f41c0-124">Элемент **Alias** не может иметь дочерние элементы.</span><span class="sxs-lookup"><span data-stu-id="f41c0-124">The **Alias** element cannot have child elements.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="f41c0-125">Применимые атрибуты</span><span class="sxs-lookup"><span data-stu-id="f41c0-125">Applicable Attributes</span></span>

<span data-ttu-id="f41c0-126">В следующей таблице описаны атрибуты, которые можно применить к элементу **Alias** .</span><span class="sxs-lookup"><span data-stu-id="f41c0-126">The table below describes the attributes that can be applied to the **Alias** element.</span></span>

| <span data-ttu-id="f41c0-127">Имя атрибута</span><span class="sxs-lookup"><span data-stu-id="f41c0-127">Attribute Name</span></span> | <span data-ttu-id="f41c0-128">Обязательный</span><span class="sxs-lookup"><span data-stu-id="f41c0-128">Is Required</span></span> | <span data-ttu-id="f41c0-129">Значение</span><span class="sxs-lookup"><span data-stu-id="f41c0-129">Value</span></span>                                                                     |
|:---------------|:------------|:--------------------------------------------------------------------------|
| <span data-ttu-id="f41c0-130">**Ключ**</span><span class="sxs-lookup"><span data-stu-id="f41c0-130">**Key**</span></span>        | <span data-ttu-id="f41c0-131">Да</span><span class="sxs-lookup"><span data-stu-id="f41c0-131">Yes</span></span>         | <span data-ttu-id="f41c0-132">Псевдоним для пространства имен, определяемого атрибутом **value** .</span><span class="sxs-lookup"><span data-stu-id="f41c0-132">The alias for the namespace that is specified by the **Value** attribute.</span></span> |
| <span data-ttu-id="f41c0-133">**Значение**</span><span class="sxs-lookup"><span data-stu-id="f41c0-133">**Value**</span></span>      | <span data-ttu-id="f41c0-134">Да</span><span class="sxs-lookup"><span data-stu-id="f41c0-134">Yes</span></span>         | <span data-ttu-id="f41c0-135">Пространство имен, для которого значение элемента **Key** является псевдонимом.</span><span class="sxs-lookup"><span data-stu-id="f41c0-135">The namespace for which the value of the **Key** element is an alias.</span></span>     |

### <a name="example"></a><span data-ttu-id="f41c0-136">Пример</span><span class="sxs-lookup"><span data-stu-id="f41c0-136">Example</span></span>

<span data-ttu-id="f41c0-137">В следующем примере показан элемент **Alias** , определяющий псевдоним `c` для типов, определенных в концептуальной модели.</span><span class="sxs-lookup"><span data-stu-id="f41c0-137">The following example shows an **Alias** element that defines an alias, `c`, for types that are defined in the conceptual model.</span></span>

``` xml
 <Mapping Space="C-S"
          xmlns="https://schemas.microsoft.com/ado/2009/11/mapping/cs">
   <Alias Key="c" Value="SchoolModel"/>
   <EntityContainerMapping StorageEntityContainer="SchoolModelStoreContainer"
                           CdmEntityContainer="SchoolModelEntities">
     <EntitySetMapping Name="Courses">
       <EntityTypeMapping TypeName="c.Course">
         <MappingFragment StoreEntitySet="Course">
           <ScalarProperty Name="CourseID" ColumnName="CourseID" />
           <ScalarProperty Name="Title" ColumnName="Title" />
           <ScalarProperty Name="Credits" ColumnName="Credits" />
           <ScalarProperty Name="DepartmentID" ColumnName="DepartmentID" />
         </MappingFragment>
       </EntityTypeMapping>
     </EntitySetMapping>
     <EntitySetMapping Name="Departments">
       <EntityTypeMapping TypeName="c.Department">
         <MappingFragment StoreEntitySet="Department">
           <ScalarProperty Name="DepartmentID" ColumnName="DepartmentID" />
           <ScalarProperty Name="Name" ColumnName="Name" />
           <ScalarProperty Name="Budget" ColumnName="Budget" />
           <ScalarProperty Name="StartDate" ColumnName="StartDate" />
           <ScalarProperty Name="Administrator" ColumnName="Administrator" />
         </MappingFragment>
       </EntityTypeMapping>
     </EntitySetMapping>
   </EntityContainerMapping>
 </Mapping>
```

## <a name="associationend-element-msl"></a><span data-ttu-id="f41c0-138">Элемент AssociationEnd (MSL)</span><span class="sxs-lookup"><span data-stu-id="f41c0-138">AssociationEnd Element (MSL)</span></span>

<span data-ttu-id="f41c0-139">Элемент **AssociationEnd** на языке MSL используется, когда функции изменения типа сущности в концептуальной модели сопоставляются с хранимыми процедурами в базовой базе данных.</span><span class="sxs-lookup"><span data-stu-id="f41c0-139">The **AssociationEnd** element in mapping specification language (MSL) is used when the modification functions of an entity type in the conceptual model are mapped to stored procedures in the underlying database.</span></span> <span data-ttu-id="f41c0-140">Если хранимая процедура изменения принимает параметр, значение которого хранится в свойстве Association, элемент **AssociationEnd** сопоставляет значение свойства с параметром.</span><span class="sxs-lookup"><span data-stu-id="f41c0-140">If a modification stored procedure takes a parameter whose value is held in an association property, the **AssociationEnd** element maps the property value to the parameter.</span></span> <span data-ttu-id="f41c0-141">Дополнительные сведения см. в приведенных ниже примерах.</span><span class="sxs-lookup"><span data-stu-id="f41c0-141">For more information, see the example below.</span></span>

<span data-ttu-id="f41c0-142">Дополнительные сведения о сопоставлении функций изменения типов сущностей с хранимыми процедурами см. в разделе элемент ModificationFunctionMapping (MSL) и пошаговое руководство. Сопоставление сущности с хранимыми процедурами.</span><span class="sxs-lookup"><span data-stu-id="f41c0-142">For more information about mapping modification functions of entity types to stored procedures, see ModificationFunctionMapping Element (MSL) and Walkthrough: Mapping an Entity to Stored Procedures.</span></span>

<span data-ttu-id="f41c0-143">Элемент **AssociationEnd** может иметь следующие дочерние элементы:</span><span class="sxs-lookup"><span data-stu-id="f41c0-143">The **AssociationEnd** element can have the following child elements:</span></span>

-   <span data-ttu-id="f41c0-144">ScalarProperty</span><span class="sxs-lookup"><span data-stu-id="f41c0-144">ScalarProperty</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="f41c0-145">Применимые атрибуты</span><span class="sxs-lookup"><span data-stu-id="f41c0-145">Applicable Attributes</span></span>

<span data-ttu-id="f41c0-146">В следующей таблице описаны атрибуты, применимые к элементу **AssociationEnd** .</span><span class="sxs-lookup"><span data-stu-id="f41c0-146">The following table describes the attributes that are applicable to the **AssociationEnd** element.</span></span>

| <span data-ttu-id="f41c0-147">Имя атрибута</span><span class="sxs-lookup"><span data-stu-id="f41c0-147">Attribute Name</span></span>     | <span data-ttu-id="f41c0-148">Обязательный</span><span class="sxs-lookup"><span data-stu-id="f41c0-148">Is Required</span></span> | <span data-ttu-id="f41c0-149">Значение</span><span class="sxs-lookup"><span data-stu-id="f41c0-149">Value</span></span>                                                                                                                                                                             |
|:-------------------|:------------|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="f41c0-150">**AssociationSet**</span><span class="sxs-lookup"><span data-stu-id="f41c0-150">**AssociationSet**</span></span> | <span data-ttu-id="f41c0-151">Да</span><span class="sxs-lookup"><span data-stu-id="f41c0-151">Yes</span></span>         | <span data-ttu-id="f41c0-152">Имя сопоставляемой ассоциации.</span><span class="sxs-lookup"><span data-stu-id="f41c0-152">The name of the association that is being mapped.</span></span>                                                                                                                                 |
| <span data-ttu-id="f41c0-153">**От**</span><span class="sxs-lookup"><span data-stu-id="f41c0-153">**From**</span></span>           | <span data-ttu-id="f41c0-154">Да</span><span class="sxs-lookup"><span data-stu-id="f41c0-154">Yes</span></span>         | <span data-ttu-id="f41c0-155">Значение атрибута **FromRole** свойства навигации, соответствующего сопоставленной ассоциации.</span><span class="sxs-lookup"><span data-stu-id="f41c0-155">The value of the **FromRole** attribute of the navigation property that corresponds to the association being mapped.</span></span> <span data-ttu-id="f41c0-156">Дополнительные сведения см. в разделе Элемент NavigationProperty (CSDL).</span><span class="sxs-lookup"><span data-stu-id="f41c0-156">For more information, see NavigationProperty Element (CSDL).</span></span> |
| <span data-ttu-id="f41c0-157">**Чтобы**</span><span class="sxs-lookup"><span data-stu-id="f41c0-157">**To**</span></span>             | <span data-ttu-id="f41c0-158">Да</span><span class="sxs-lookup"><span data-stu-id="f41c0-158">Yes</span></span>         | <span data-ttu-id="f41c0-159">Значение атрибута **ToRole** свойства навигации, соответствующего сопоставленной ассоциации.</span><span class="sxs-lookup"><span data-stu-id="f41c0-159">The value of the **ToRole** attribute of the navigation property that corresponds to the association being mapped.</span></span> <span data-ttu-id="f41c0-160">Дополнительные сведения см. в разделе Элемент NavigationProperty (CSDL).</span><span class="sxs-lookup"><span data-stu-id="f41c0-160">For more information, see NavigationProperty Element (CSDL).</span></span>   |

### <a name="example"></a><span data-ttu-id="f41c0-161">Пример</span><span class="sxs-lookup"><span data-stu-id="f41c0-161">Example</span></span>

<span data-ttu-id="f41c0-162">Рассмотрим следующий тип сущности в концептуальной модели:</span><span class="sxs-lookup"><span data-stu-id="f41c0-162">Consider the following conceptual model entity type:</span></span>

``` xml
 <EntityType Name="Course">
   <Key>
     <PropertyRef Name="CourseID" />
   </Key>
   <Property Type="Int32" Name="CourseID" Nullable="false" />
   <Property Type="String" Name="Title" Nullable="false" MaxLength="100"
             FixedLength="false" Unicode="true" />
   <Property Type="Int32" Name="Credits" Nullable="false" />
   <NavigationProperty Name="Department"
                       Relationship="SchoolModel.FK_Course_Department"
                       FromRole="Course" ToRole="Department" />
 </EntityType>
```

<span data-ttu-id="f41c0-163">Предположим, имеется следующая хранимая процедура:</span><span class="sxs-lookup"><span data-stu-id="f41c0-163">Also consider the following stored procedure:</span></span>

``` SQL
 CREATE PROCEDURE [dbo].[UpdateCourse]
                                @CourseID int,
                                @Title nvarchar(50),
                                @Credits int,
                                @DepartmentID int
                                AS
                                UPDATE Course SET Title=@Title,
                                                              Credits=@Credits,
                                                              DepartmentID=@DepartmentID
                                WHERE CourseID=@CourseID;
```

<span data-ttu-id="f41c0-164">Чтобы связать функцию обновления `Course` сущности с этой хранимой процедурой, необходимо указать значение для параметра **DepartmentID** .</span><span class="sxs-lookup"><span data-stu-id="f41c0-164">In order to map the update function of the `Course` entity to this stored procedure, you must supply a value to the **DepartmentID** parameter.</span></span> <span data-ttu-id="f41c0-165">Значение для `DepartmentID` не соответствует свойству типа сущности. Оно содержится в независимом сопоставлении с сопоставлением, показанным ниже:</span><span class="sxs-lookup"><span data-stu-id="f41c0-165">The value for `DepartmentID` does not correspond to a property on the entity type; it is contained in an independent association whose mapping is shown here:</span></span>

``` xml
 <AssociationSetMapping Name="FK_Course_Department"
                        TypeName="SchoolModel.FK_Course_Department"
                        StoreEntitySet="Course">
   <EndProperty Name="Course">
     <ScalarProperty Name="CourseID" ColumnName="CourseID" />
   </EndProperty>
   <EndProperty Name="Department">
     <ScalarProperty Name="DepartmentID" ColumnName="DepartmentID" />
   </EndProperty>
 </AssociationSetMapping>
```

<span data-ttu-id="f41c0-166">В следующем коде показан элемент **AssociationEnd** , используемый для сопоставления свойства **DepartmentID** ассоциации \*\* \_ \_ Департамента курса FK\*\* с хранимой процедурой **упдатекаурсе** (для которой сопоставлена функция Update типа сущности **Course** ):</span><span class="sxs-lookup"><span data-stu-id="f41c0-166">The following code shows the **AssociationEnd** element used to map the **DepartmentID** property of the **FK\_Course\_Department** association to the **UpdateCourse** stored procedure (to which the update function of the **Course** entity type is mapped):</span></span>

``` xml
 <EntitySetMapping Name="Courses">
   <EntityTypeMapping TypeName="SchoolModel.Course">
     <MappingFragment StoreEntitySet="Course">
       <ScalarProperty Name="Credits" ColumnName="Credits" />
       <ScalarProperty Name="Title" ColumnName="Title" />
       <ScalarProperty Name="CourseID" ColumnName="CourseID" />
     </MappingFragment>
   </EntityTypeMapping>
   <EntityTypeMapping TypeName="SchoolModel.Course">
     <ModificationFunctionMapping>
       <UpdateFunction FunctionName="SchoolModel.Store.UpdateCourse">
         <AssociationEnd AssociationSet="FK_Course_Department"
                         From="Course" To="Department">
           <ScalarProperty Name="DepartmentID"
                           ParameterName="DepartmentID"
                           Version="Current" />
         </AssociationEnd>
         <ScalarProperty Name="Credits" ParameterName="Credits"
                         Version="Current" />
         <ScalarProperty Name="Title" ParameterName="Title"
                         Version="Current" />
         <ScalarProperty Name="CourseID" ParameterName="CourseID"
                         Version="Current" />
       </UpdateFunction>
     </ModificationFunctionMapping>
   </EntityTypeMapping>
 </EntitySetMapping>
```

## <a name="associationsetmapping-element-msl"></a><span data-ttu-id="f41c0-167">Элемент AssociationSetMapping (MSL)</span><span class="sxs-lookup"><span data-stu-id="f41c0-167">AssociationSetMapping Element (MSL)</span></span>

<span data-ttu-id="f41c0-168">Элемент **AssociationSetMapping** на языке MSL определяет сопоставление между Ассоциацией в концептуальной модели и столбцами таблицы в базовой базе данных.</span><span class="sxs-lookup"><span data-stu-id="f41c0-168">The **AssociationSetMapping** element in mapping specification language (MSL) defines the mapping between an association in the conceptual model and table columns in the underlying database.</span></span>

<span data-ttu-id="f41c0-169">Сопоставления в концептуальной модели — это типы, свойства которых представляют столбцы первичного и внешнего ключа в основной базе данных.</span><span class="sxs-lookup"><span data-stu-id="f41c0-169">Associations in the conceptual model are types whose properties represent primary and foreign key columns in the underlying database.</span></span> <span data-ttu-id="f41c0-170">Элемент **AssociationSetMapping** использует два элемента EndProperty для определения сопоставлений между свойствами и столбцами типа взаимосвязи в базе данных.</span><span class="sxs-lookup"><span data-stu-id="f41c0-170">The **AssociationSetMapping** element uses two EndProperty elements to define the mappings between association type properties and columns in the database.</span></span> <span data-ttu-id="f41c0-171">Можно поместить условия сопоставления в элемент Condition.</span><span class="sxs-lookup"><span data-stu-id="f41c0-171">You can place conditions on these mappings with the Condition element.</span></span> <span data-ttu-id="f41c0-172">Сопоставьте функции вставки, обновления и удаления сопоставлений с хранимыми процедурами в базе данных с помощью элемента ModificationFunctionMapping.</span><span class="sxs-lookup"><span data-stu-id="f41c0-172">Map the insert, update, and delete functions for associations to stored procedures in the database with the ModificationFunctionMapping element.</span></span> <span data-ttu-id="f41c0-173">Определение сопоставлений только для чтения между ассоциациями и столбцами таблицы с помощью Entity SQL строки в элементе QueryView.</span><span class="sxs-lookup"><span data-stu-id="f41c0-173">Define read-only mappings between associations and table columns by using an Entity SQL string in a QueryView element.</span></span>

> [!NOTE]
> <span data-ttu-id="f41c0-174">Если для ассоциации в концептуальной модели определено ссылочное ограничение, Ассоциация не должна сопоставляться с элементом **AssociationSetMapping** .</span><span class="sxs-lookup"><span data-stu-id="f41c0-174">If a referential constraint is defined for an association in the conceptual model, the association does not need to be mapped with an **AssociationSetMapping** element.</span></span> <span data-ttu-id="f41c0-175">Если для ассоциации, имеющей справочное ограничение, имеется элемент **AssociationSetMapping** , то сопоставления, определенные в элементе **AssociationSetMapping** , будут игнорироваться.</span><span class="sxs-lookup"><span data-stu-id="f41c0-175">If an **AssociationSetMapping** element is present for an association that has a referential constraint, the mappings defined in the **AssociationSetMapping** element will be ignored.</span></span> <span data-ttu-id="f41c0-176">Дополнительные сведения см. в разделе элемент ReferentialConstraint (CSDL).</span><span class="sxs-lookup"><span data-stu-id="f41c0-176">For more information, see ReferentialConstraint Element (CSDL).</span></span>

<span data-ttu-id="f41c0-177">Элемент **AssociationSetMapping** может иметь следующие дочерние элементы</span><span class="sxs-lookup"><span data-stu-id="f41c0-177">The **AssociationSetMapping** element can have the following child elements</span></span>

-   <span data-ttu-id="f41c0-178">QueryView (ноль или один)</span><span class="sxs-lookup"><span data-stu-id="f41c0-178">QueryView (zero or one)</span></span>
-   <span data-ttu-id="f41c0-179">EndProperty (ноль или два)</span><span class="sxs-lookup"><span data-stu-id="f41c0-179">EndProperty (zero or two)</span></span>
-   <span data-ttu-id="f41c0-180">Condition (ноль и более)</span><span class="sxs-lookup"><span data-stu-id="f41c0-180">Condition (zero or more)</span></span>
-   <span data-ttu-id="f41c0-181">ModificationFunctionMapping (ноль или один)</span><span class="sxs-lookup"><span data-stu-id="f41c0-181">ModificationFunctionMapping (zero or one)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="f41c0-182">Применимые атрибуты</span><span class="sxs-lookup"><span data-stu-id="f41c0-182">Applicable Attributes</span></span>

<span data-ttu-id="f41c0-183">В следующей таблице описаны атрибуты, которые можно применить к элементу **AssociationSetMapping** .</span><span class="sxs-lookup"><span data-stu-id="f41c0-183">The following table describes the attributes that can be applied to the **AssociationSetMapping** element.</span></span>

| <span data-ttu-id="f41c0-184">Имя атрибута</span><span class="sxs-lookup"><span data-stu-id="f41c0-184">Attribute Name</span></span>     | <span data-ttu-id="f41c0-185">Обязательный</span><span class="sxs-lookup"><span data-stu-id="f41c0-185">Is Required</span></span> | <span data-ttu-id="f41c0-186">Значение</span><span class="sxs-lookup"><span data-stu-id="f41c0-186">Value</span></span>                                                                                       |
|:-------------------|:------------|:--------------------------------------------------------------------------------------------|
| <span data-ttu-id="f41c0-187">**имя**;</span><span class="sxs-lookup"><span data-stu-id="f41c0-187">**Name**</span></span>           | <span data-ttu-id="f41c0-188">Да</span><span class="sxs-lookup"><span data-stu-id="f41c0-188">Yes</span></span>         | <span data-ttu-id="f41c0-189">Имя набора ассоциаций концептуальной модели, с которым выполняется сопоставление.</span><span class="sxs-lookup"><span data-stu-id="f41c0-189">The name of the conceptual model association set that is being mapped.</span></span>                      |
| <span data-ttu-id="f41c0-190">**Имени**</span><span class="sxs-lookup"><span data-stu-id="f41c0-190">**TypeName**</span></span>       | <span data-ttu-id="f41c0-191">Нет</span><span class="sxs-lookup"><span data-stu-id="f41c0-191">No</span></span>          | <span data-ttu-id="f41c0-192">Имя типа ассоциации концептуальной модели из полного пространства имен, с которым выполняется сопоставление.</span><span class="sxs-lookup"><span data-stu-id="f41c0-192">The namespace-qualified name of the conceptual model association type that is being mapped.</span></span> |
| <span data-ttu-id="f41c0-193">**сторинтитисет**</span><span class="sxs-lookup"><span data-stu-id="f41c0-193">**StoreEntitySet**</span></span> | <span data-ttu-id="f41c0-194">Нет</span><span class="sxs-lookup"><span data-stu-id="f41c0-194">No</span></span>          | <span data-ttu-id="f41c0-195">Имя таблицы, с которой производится сопоставление.</span><span class="sxs-lookup"><span data-stu-id="f41c0-195">The name of the table that is being mapped.</span></span>                                                 |

### <a name="example"></a><span data-ttu-id="f41c0-196">Пример</span><span class="sxs-lookup"><span data-stu-id="f41c0-196">Example</span></span>

<span data-ttu-id="f41c0-197">В следующем примере показан элемент **AssociationSetMapping** , в котором набор ассоциаций \*\* \_ \_ отдела FK Course\*\* в концептуальной модели сопоставлен с таблицей **Course** в базе данных.</span><span class="sxs-lookup"><span data-stu-id="f41c0-197">The following example shows an **AssociationSetMapping** element in which the **FK\_Course\_Department** association set in the conceptual model is mapped to the **Course** table in the database.</span></span> <span data-ttu-id="f41c0-198">Сопоставления между свойствами типа ассоциации и столбцами таблицы указываются в дочерних элементах **EndProperty** .</span><span class="sxs-lookup"><span data-stu-id="f41c0-198">Mappings between association type properties and table columns are specified in child **EndProperty** elements.</span></span>

``` xml
 <AssociationSetMapping Name="FK_Course_Department"
                        TypeName="SchoolModel.FK_Course_Department"
                        StoreEntitySet="Course">
   <EndProperty Name="Department">
     <ScalarProperty Name="DepartmentID" ColumnName="DepartmentID" />
   </EndProperty>
   <EndProperty Name="Course">
     <ScalarProperty Name="CourseID" ColumnName="CourseID" />
   </EndProperty>
 </AssociationSetMapping>
```

## <a name="complexproperty-element-msl"></a><span data-ttu-id="f41c0-199">Элемент ComplexProperty (MSL)</span><span class="sxs-lookup"><span data-stu-id="f41c0-199">ComplexProperty Element (MSL)</span></span>

<span data-ttu-id="f41c0-200">Элемент **комплекспроперти** на языке MSL определяет сопоставление между свойством сложного типа в типе сущности концептуальной модели и столбцами таблицы в базовой базе данных.</span><span class="sxs-lookup"><span data-stu-id="f41c0-200">A **ComplexProperty** element in mapping specification language (MSL) defines the mapping between a complex type property on a conceptual model entity type and table columns in the underlying database.</span></span> <span data-ttu-id="f41c0-201">Сопоставления столбца свойств указаны в дочерних элементах ScalarProperty.</span><span class="sxs-lookup"><span data-stu-id="f41c0-201">The property-column mappings are specified in child ScalarProperty elements.</span></span>

<span data-ttu-id="f41c0-202">Элемент свойства **complexType** может иметь следующие дочерние элементы:</span><span class="sxs-lookup"><span data-stu-id="f41c0-202">The **ComplexType** property element can have the following child elements:</span></span>

-   <span data-ttu-id="f41c0-203">ScalarProperty (ноль и более)</span><span class="sxs-lookup"><span data-stu-id="f41c0-203">ScalarProperty (zero or more)</span></span>
-   <span data-ttu-id="f41c0-204">**Комплекспроперти** (ноль или более)</span><span class="sxs-lookup"><span data-stu-id="f41c0-204">**ComplexProperty** (zero or more)</span></span>
-   <span data-ttu-id="f41c0-205">ComplextTypeMapping (ноль и более)</span><span class="sxs-lookup"><span data-stu-id="f41c0-205">ComplextTypeMapping (zero or more)</span></span>
-   <span data-ttu-id="f41c0-206">Condition (ноль и более)</span><span class="sxs-lookup"><span data-stu-id="f41c0-206">Condition (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="f41c0-207">Применимые атрибуты</span><span class="sxs-lookup"><span data-stu-id="f41c0-207">Applicable Attributes</span></span>

<span data-ttu-id="f41c0-208">В следующей таблице описаны атрибуты, применимые к элементу **комплекспроперти** .</span><span class="sxs-lookup"><span data-stu-id="f41c0-208">The following table describes the attributes that are applicable to the **ComplexProperty** element:</span></span>

| <span data-ttu-id="f41c0-209">Имя атрибута</span><span class="sxs-lookup"><span data-stu-id="f41c0-209">Attribute Name</span></span> | <span data-ttu-id="f41c0-210">Обязательный</span><span class="sxs-lookup"><span data-stu-id="f41c0-210">Is Required</span></span> | <span data-ttu-id="f41c0-211">Значение</span><span class="sxs-lookup"><span data-stu-id="f41c0-211">Value</span></span>                                                                                            |
|:---------------|:------------|:-------------------------------------------------------------------------------------------------|
| <span data-ttu-id="f41c0-212">**имя**;</span><span class="sxs-lookup"><span data-stu-id="f41c0-212">**Name**</span></span>       | <span data-ttu-id="f41c0-213">Да</span><span class="sxs-lookup"><span data-stu-id="f41c0-213">Yes</span></span>         | <span data-ttu-id="f41c0-214">Имя сложного свойства типа сущности в концептуальной модели, с которым выполняется сопоставление.</span><span class="sxs-lookup"><span data-stu-id="f41c0-214">The name of the complex property of an entity type in the conceptual model that is being mapped.</span></span> |
| <span data-ttu-id="f41c0-215">**Имени**</span><span class="sxs-lookup"><span data-stu-id="f41c0-215">**TypeName**</span></span>   | <span data-ttu-id="f41c0-216">Нет</span><span class="sxs-lookup"><span data-stu-id="f41c0-216">No</span></span>          | <span data-ttu-id="f41c0-217">Полное имя пространства имен типа свойства концептуальной модели.</span><span class="sxs-lookup"><span data-stu-id="f41c0-217">The namespace-qualified name of the conceptual model property type.</span></span>                              |

### <a name="example"></a><span data-ttu-id="f41c0-218">Пример</span><span class="sxs-lookup"><span data-stu-id="f41c0-218">Example</span></span>

<span data-ttu-id="f41c0-219">Следующий пример основан на модели School.</span><span class="sxs-lookup"><span data-stu-id="f41c0-219">The following example is based on the School model.</span></span> <span data-ttu-id="f41c0-220">К концептуальной модели добавлен следующий сложный тип.</span><span class="sxs-lookup"><span data-stu-id="f41c0-220">The following complex type has been added to the conceptual model:</span></span>

``` xml
 <ComplexType Name="FullName">
   <Property Type="String" Name="LastName"
             Nullable="false" MaxLength="50"
             FixedLength="false" Unicode="true" />
   <Property Type="String" Name="FirstName"
             Nullable="false" MaxLength="50"
             FixedLength="false" Unicode="true" />
 </ComplexType>
```

<span data-ttu-id="f41c0-221">Свойства **LastName** и **FirstName** типа сущности **Person** заменены одним сложным свойством, **именем**:</span><span class="sxs-lookup"><span data-stu-id="f41c0-221">The **LastName** and **FirstName** properties of the **Person** entity type have been replaced with one complex property, **Name**:</span></span>

``` xml
 <EntityType Name="Person">
   <Key>
     <PropertyRef Name="PersonID" />
   </Key>
   <Property Name="PersonID" Type="Int32" Nullable="false"
             annotation:StoreGeneratedPattern="Identity" />
   <Property Name="HireDate" Type="DateTime" />
   <Property Name="EnrollmentDate" Type="DateTime" />
   <Property Name="Name" Type="SchoolModel.FullName" Nullable="false" />
 </EntityType>
```

<span data-ttu-id="f41c0-222">В следующем MSL показан элемент **комплекспроперти** , используемый для отображения свойства **Name** в столбцах базовой базы данных:</span><span class="sxs-lookup"><span data-stu-id="f41c0-222">The following MSL shows the **ComplexProperty** element used to map the **Name** property to columns in the underlying database:</span></span>

``` xml
 <EntitySetMapping Name="People">
   <EntityTypeMapping TypeName="SchoolModel.Person">
     <MappingFragment StoreEntitySet="Person">
       <ScalarProperty Name="PersonID" ColumnName="PersonID" />
       <ScalarProperty Name="HireDate" ColumnName="HireDate" />
       <ScalarProperty Name="EnrollmentDate" ColumnName="EnrollmentDate" />
       <ComplexProperty Name="Name" TypeName="SchoolModel.FullName">
         <ScalarProperty Name="FirstName" ColumnName="FirstName" />
         <ScalarProperty Name="LastName" ColumnName="LastName" />  
       </ComplexProperty>
     </MappingFragment>
   </EntityTypeMapping>
 </EntitySetMapping>
```

## <a name="complextypemapping-element-msl"></a><span data-ttu-id="f41c0-223">Элемент ComplexTypeMapping (MSL)</span><span class="sxs-lookup"><span data-stu-id="f41c0-223">ComplexTypeMapping Element (MSL)</span></span>

<span data-ttu-id="f41c0-224">Элемент **комплекстипемаппинг** в языке MSL является дочерним элементом элемента ресултмаппинг и определяет сопоставление между импортом функции в концептуальной модели и хранимой процедурой в базовой базе данных, если выполняются следующие условия.</span><span class="sxs-lookup"><span data-stu-id="f41c0-224">The **ComplexTypeMapping** element in mapping specification language (MSL) is a child of the ResultMapping element and defines the mapping between a function import in the conceptual model and a stored procedure in the underlying database when the following are true:</span></span>

-   <span data-ttu-id="f41c0-225">Импорт функции возвращает концептуальный сложный тип.</span><span class="sxs-lookup"><span data-stu-id="f41c0-225">The function import returns a conceptual complex type.</span></span>
-   <span data-ttu-id="f41c0-226">Имена столбцов, возвращаемые хранимой процедурой, не соответствуют в точности именам свойств в сложном типе.</span><span class="sxs-lookup"><span data-stu-id="f41c0-226">The names of the columns returned by the stored procedure do not exactly match the names of the properties on the complex type.</span></span>

<span data-ttu-id="f41c0-227">По умолчанию сопоставление между столбцами, возвращаемыми хранимой процедурой, и сложным типом основано на именах столбцов и свойств.</span><span class="sxs-lookup"><span data-stu-id="f41c0-227">By default, the mapping between the columns returned by a stored procedure and a complex type is based on column and property names.</span></span> <span data-ttu-id="f41c0-228">Если имена столбцов не точно соответствуют именам свойств, для определения сопоставления необходимо использовать элемент **комплекстипемаппинг** .</span><span class="sxs-lookup"><span data-stu-id="f41c0-228">If column names do not exactly match property names, you must use the **ComplexTypeMapping** element to define the mapping.</span></span> <span data-ttu-id="f41c0-229">Пример сопоставления по умолчанию см. в разделе элемент FunctionImportMapping (MSL).</span><span class="sxs-lookup"><span data-stu-id="f41c0-229">For an example of the default mapping, see FunctionImportMapping Element (MSL).</span></span>

<span data-ttu-id="f41c0-230">Элемент **комплекстипемаппинг** может иметь следующие дочерние элементы:</span><span class="sxs-lookup"><span data-stu-id="f41c0-230">The **ComplexTypeMapping** element can have the following child elements:</span></span>

-   <span data-ttu-id="f41c0-231">ScalarProperty (ноль и более)</span><span class="sxs-lookup"><span data-stu-id="f41c0-231">ScalarProperty (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="f41c0-232">Применимые атрибуты</span><span class="sxs-lookup"><span data-stu-id="f41c0-232">Applicable Attributes</span></span>

<span data-ttu-id="f41c0-233">В следующей таблице описаны атрибуты, применимые к элементу **комплекстипемаппинг** .</span><span class="sxs-lookup"><span data-stu-id="f41c0-233">The following table describes the attributes that are applicable to the **ComplexTypeMapping** element.</span></span>

| <span data-ttu-id="f41c0-234">Имя атрибута</span><span class="sxs-lookup"><span data-stu-id="f41c0-234">Attribute Name</span></span> | <span data-ttu-id="f41c0-235">Обязательный</span><span class="sxs-lookup"><span data-stu-id="f41c0-235">Is Required</span></span> | <span data-ttu-id="f41c0-236">Значение</span><span class="sxs-lookup"><span data-stu-id="f41c0-236">Value</span></span>                                                                  |
|:---------------|:------------|:-----------------------------------------------------------------------|
| <span data-ttu-id="f41c0-237">**Имени**</span><span class="sxs-lookup"><span data-stu-id="f41c0-237">**TypeName**</span></span>   | <span data-ttu-id="f41c0-238">Да</span><span class="sxs-lookup"><span data-stu-id="f41c0-238">Yes</span></span>         | <span data-ttu-id="f41c0-239">Имя сопоставляемого сложного типа с указанием пространства имен.</span><span class="sxs-lookup"><span data-stu-id="f41c0-239">The namespace-qualified name of the complex type that is being mapped.</span></span> |

### <a name="example"></a><span data-ttu-id="f41c0-240">Пример</span><span class="sxs-lookup"><span data-stu-id="f41c0-240">Example</span></span>

<span data-ttu-id="f41c0-241">Рассмотрим следующую хранимую процедуру:</span><span class="sxs-lookup"><span data-stu-id="f41c0-241">Consider the following stored procedure:</span></span>

``` SQL
 CREATE PROCEDURE [dbo].[GetGrades]
             @student_Id int
             AS
             SELECT     EnrollmentID as enroll_id,
                                                                             Grade as grade,
                                                                             CourseID as course_id,
                                                                             StudentID as student_id
                                               FROM dbo.StudentGrade
             WHERE StudentID = @student_Id
```

<span data-ttu-id="f41c0-242">Следует также обратить внимание на следующий сложный тип концептуальной модели:</span><span class="sxs-lookup"><span data-stu-id="f41c0-242">Also consider the following conceptual model complex type:</span></span>

``` xml
 <ComplexType Name="GradeInfo">
   <Property Type="Int32" Name="EnrollmentID" Nullable="false" />
   <Property Type="Decimal" Name="Grade" Nullable="true"
             Precision="3" Scale="2" />
   <Property Type="Int32" Name="CourseID" Nullable="false" />
   <Property Type="Int32" Name="StudentID" Nullable="false" />
 </ComplexType>
```

<span data-ttu-id="f41c0-243">Чтобы создать импорт функции, возвращающий экземпляры предыдущего сложного типа, сопоставление столбцов, возвращаемых хранимой процедурой и типом сущности, должно быть определено в элементе **комплекстипемаппинг** :</span><span class="sxs-lookup"><span data-stu-id="f41c0-243">In order to create a function import that returns instances of the previous complex type, the mapping between the columns returned by the stored procedure and the entity type must be defined in a **ComplexTypeMapping** element:</span></span>

``` xml
 <FunctionImportMapping FunctionImportName="GetGrades"
                        FunctionName="SchoolModel.Store.GetGrades" >
   <ResultMapping>
     <ComplexTypeMapping TypeName="SchoolModel.GradeInfo">
       <ScalarProperty Name="EnrollmentID" ColumnName="enroll_id"/>
       <ScalarProperty Name="CourseID" ColumnName="course_id"/>
       <ScalarProperty Name="StudentID" ColumnName="student_id"/>
       <ScalarProperty Name="Grade" ColumnName="grade"/>
     </ComplexTypeMapping>
   </ResultMapping>
 </FunctionImportMapping>
```

## <a name="condition-element-msl"></a><span data-ttu-id="f41c0-244">Элемент Condition (MSL)</span><span class="sxs-lookup"><span data-stu-id="f41c0-244">Condition Element (MSL)</span></span>

<span data-ttu-id="f41c0-245">Элемент **Condition** на языке MSL размещает условия для сопоставлений между концептуальной моделью и базовой базой данных.</span><span class="sxs-lookup"><span data-stu-id="f41c0-245">The **Condition** element in mapping specification language (MSL) places conditions on mappings between the conceptual model and the underlying database.</span></span> <span data-ttu-id="f41c0-246">Сопоставление, определенное в узле XML, допустимо, если выполняются все условия, указанные в дочерних элементах **Condition** .</span><span class="sxs-lookup"><span data-stu-id="f41c0-246">The mapping that is defined within an XML node is valid if all conditions, as specified in child **Condition** elements, are met.</span></span> <span data-ttu-id="f41c0-247">В противном случае сопоставление недействительно.</span><span class="sxs-lookup"><span data-stu-id="f41c0-247">Otherwise, the mapping is not valid.</span></span> <span data-ttu-id="f41c0-248">Например, если элемент MappingFragment содержит одно или несколько дочерних элементов **Condition** , сопоставление, определенное в узле **MappingFragment** , будет действительным только в том случае, если выполняются все условия дочерних элементов **Condition** .</span><span class="sxs-lookup"><span data-stu-id="f41c0-248">For example, if a MappingFragment element contains one or more **Condition** child elements, the mapping defined within the **MappingFragment** node will only be valid if all the conditions of the child **Condition** elements are met.</span></span>

<span data-ttu-id="f41c0-249">Каждое условие может применяться либо к **имени** (имени свойства сущности концептуальной модели, заданному атрибутом **Name** ), либо к **ColumnName** (имени столбца в базе данных, указанному атрибутом **ColumnName** ).</span><span class="sxs-lookup"><span data-stu-id="f41c0-249">Each condition can apply to either a **Name** (the name of a conceptual model entity property, specified by the **Name** attribute), or a **ColumnName** (the name of a column in the database, specified by the **ColumnName** attribute).</span></span> <span data-ttu-id="f41c0-250">Если задан атрибут **Name** , условие проверяется по значению свойства сущности.</span><span class="sxs-lookup"><span data-stu-id="f41c0-250">When the **Name** attribute is set, the condition is checked against an entity property value.</span></span> <span data-ttu-id="f41c0-251">Если атрибут **ColumnName** установлен, условие проверяется на соответствие значению столбца.</span><span class="sxs-lookup"><span data-stu-id="f41c0-251">When the **ColumnName** attribute is set, the condition is checked against a column value.</span></span> <span data-ttu-id="f41c0-252">В элементе **Condition** можно указать только одно из атрибутов **Name** или **ColumnName** .</span><span class="sxs-lookup"><span data-stu-id="f41c0-252">Only one of the **Name** or **ColumnName** attribute can be specified in a **Condition** element.</span></span>

> [!NOTE]
> <span data-ttu-id="f41c0-253">Если элемент **Condition** используется в элементе FunctionImportMapping, то только атрибут **Name** неприменим.</span><span class="sxs-lookup"><span data-stu-id="f41c0-253">When the **Condition** element is used within a FunctionImportMapping element, only the **Name** attribute is not applicable.</span></span>

<span data-ttu-id="f41c0-254">Элемент **Condition** может быть дочерним для следующих элементов:</span><span class="sxs-lookup"><span data-stu-id="f41c0-254">The **Condition** element can be a child of the following elements:</span></span>

-   <span data-ttu-id="f41c0-255">AssociationSetMapping</span><span class="sxs-lookup"><span data-stu-id="f41c0-255">AssociationSetMapping</span></span>
-   <span data-ttu-id="f41c0-256">ComplexProperty</span><span class="sxs-lookup"><span data-stu-id="f41c0-256">ComplexProperty</span></span>
-   <span data-ttu-id="f41c0-257">EntitySetMapping</span><span class="sxs-lookup"><span data-stu-id="f41c0-257">EntitySetMapping</span></span>
-   <span data-ttu-id="f41c0-258">MappingFragment</span><span class="sxs-lookup"><span data-stu-id="f41c0-258">MappingFragment</span></span>
-   <span data-ttu-id="f41c0-259">EntityTypeMapping</span><span class="sxs-lookup"><span data-stu-id="f41c0-259">EntityTypeMapping</span></span>

<span data-ttu-id="f41c0-260">Элемент **Condition** не может иметь дочерних элементов.</span><span class="sxs-lookup"><span data-stu-id="f41c0-260">The **Condition** element can have no child elements.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="f41c0-261">Применимые атрибуты</span><span class="sxs-lookup"><span data-stu-id="f41c0-261">Applicable Attributes</span></span>

<span data-ttu-id="f41c0-262">В следующей таблице описаны атрибуты, применимые к элементу **Condition** .</span><span class="sxs-lookup"><span data-stu-id="f41c0-262">The following table describes the attributes that are applicable to the **Condition** element:</span></span>

| <span data-ttu-id="f41c0-263">Имя атрибута</span><span class="sxs-lookup"><span data-stu-id="f41c0-263">Attribute Name</span></span> | <span data-ttu-id="f41c0-264">Обязательный</span><span class="sxs-lookup"><span data-stu-id="f41c0-264">Is Required</span></span> | <span data-ttu-id="f41c0-265">Значение</span><span class="sxs-lookup"><span data-stu-id="f41c0-265">Value</span></span>                                                                                                                                                                                                                                                                                         |
|:---------------|:------------|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="f41c0-266">**ColumnName**</span><span class="sxs-lookup"><span data-stu-id="f41c0-266">**ColumnName**</span></span> | <span data-ttu-id="f41c0-267">Нет</span><span class="sxs-lookup"><span data-stu-id="f41c0-267">No</span></span>          | <span data-ttu-id="f41c0-268">Имя столбца таблицы, значение которого используется для оценки условия.</span><span class="sxs-lookup"><span data-stu-id="f41c0-268">The name of the table column whose value is used to evaluate the condition.</span></span>                                                                                                                                                                                                                   |
| <span data-ttu-id="f41c0-269">**IsNull**</span><span class="sxs-lookup"><span data-stu-id="f41c0-269">**IsNull**</span></span>     | <span data-ttu-id="f41c0-270">Нет</span><span class="sxs-lookup"><span data-stu-id="f41c0-270">No</span></span>          | <span data-ttu-id="f41c0-271">**True** или **false**.</span><span class="sxs-lookup"><span data-stu-id="f41c0-271">**True** or **False**.</span></span> <span data-ttu-id="f41c0-272">Если значение равно **true** , а значение столбца равно **null**или если значение равно **false** , а значение столбца не равно **null**, условие имеет значение true.</span><span class="sxs-lookup"><span data-stu-id="f41c0-272">If the value is **True** and the column value is **null**, or if the value is **False** and the column value is not **null**, the condition is true.</span></span> <span data-ttu-id="f41c0-273">В противном случае условие имеет значение false.</span><span class="sxs-lookup"><span data-stu-id="f41c0-273">Otherwise, the condition is false.</span></span> <br/> <span data-ttu-id="f41c0-274">Атрибуты **IsNull** и **value** нельзя использовать одновременно.</span><span class="sxs-lookup"><span data-stu-id="f41c0-274">The **IsNull** and **Value** attributes cannot be used at the same time.</span></span> |
| <span data-ttu-id="f41c0-275">**Значение**</span><span class="sxs-lookup"><span data-stu-id="f41c0-275">**Value**</span></span>      | <span data-ttu-id="f41c0-276">Нет</span><span class="sxs-lookup"><span data-stu-id="f41c0-276">No</span></span>          | <span data-ttu-id="f41c0-277">Значение, с которым сравнивается значение столбца.</span><span class="sxs-lookup"><span data-stu-id="f41c0-277">The value with which the column value is compared.</span></span> <span data-ttu-id="f41c0-278">Если значения совпадают, условие верно.</span><span class="sxs-lookup"><span data-stu-id="f41c0-278">If the values are the same, the condition is true.</span></span> <span data-ttu-id="f41c0-279">В противном случае условие имеет значение false.</span><span class="sxs-lookup"><span data-stu-id="f41c0-279">Otherwise, the condition is false.</span></span> <br/> <span data-ttu-id="f41c0-280">Атрибуты **IsNull** и **value** нельзя использовать одновременно.</span><span class="sxs-lookup"><span data-stu-id="f41c0-280">The **IsNull** and **Value** attributes cannot be used at the same time.</span></span>                                                                       |
| <span data-ttu-id="f41c0-281">**имя**;</span><span class="sxs-lookup"><span data-stu-id="f41c0-281">**Name**</span></span>       | <span data-ttu-id="f41c0-282">Нет</span><span class="sxs-lookup"><span data-stu-id="f41c0-282">No</span></span>          | <span data-ttu-id="f41c0-283">Имя свойства сущности концептуальной модели, значение которого используется для оценки условия.</span><span class="sxs-lookup"><span data-stu-id="f41c0-283">The name of the conceptual model entity property whose value is used to evaluate the condition.</span></span> <br/> <span data-ttu-id="f41c0-284">Этот атрибут неприменим, если элемент **Condition** используется в элементе FunctionImportMapping.</span><span class="sxs-lookup"><span data-stu-id="f41c0-284">This attribute is not applicable if the **Condition** element is used within a FunctionImportMapping element.</span></span>                                                                           |

### <a name="example"></a><span data-ttu-id="f41c0-285">Пример</span><span class="sxs-lookup"><span data-stu-id="f41c0-285">Example</span></span>

<span data-ttu-id="f41c0-286">В следующем примере элементы **Condition** показаны как дочерние элементы элементов **MappingFragment** .</span><span class="sxs-lookup"><span data-stu-id="f41c0-286">The following example shows **Condition** elements as children of **MappingFragment** elements.</span></span> <span data-ttu-id="f41c0-287">Если параметр **HireDate** не равен null и **енроллментдате** имеет значение null, данные сопоставляются между типом **счулмодел. Instructor** и столбцами **PersonID** и **HireDate** таблицы **Person** .</span><span class="sxs-lookup"><span data-stu-id="f41c0-287">When **HireDate** is not null and **EnrollmentDate** is null, data is mapped between the **SchoolModel.Instructor** type and the **PersonID** and **HireDate** columns of the **Person** table.</span></span> <span data-ttu-id="f41c0-288">Если **енроллментдате** имеет значение NOT NULL, а значение **HireDate** равно null, то данные сопоставляются между типом **счулмодел. Student** и столбцами **PersonID** и **регистрации** таблицы **Person** .</span><span class="sxs-lookup"><span data-stu-id="f41c0-288">When **EnrollmentDate** is not null and **HireDate** is null, data is mapped between the **SchoolModel.Student** type and the **PersonID** and **Enrollment** columns of the **Person** table.</span></span>

``` xml
 <EntitySetMapping Name="People">
   <EntityTypeMapping TypeName="IsTypeOf(SchoolModel.Person)">
     <MappingFragment StoreEntitySet="Person">
       <ScalarProperty Name="PersonID" ColumnName="PersonID" />
       <ScalarProperty Name="FirstName" ColumnName="FirstName" />
       <ScalarProperty Name="LastName" ColumnName="LastName" />
     </MappingFragment>
   </EntityTypeMapping>
   <EntityTypeMapping TypeName="IsTypeOf(SchoolModel.Instructor)">
     <MappingFragment StoreEntitySet="Person">
       <ScalarProperty Name="PersonID" ColumnName="PersonID" />
       <ScalarProperty Name="HireDate" ColumnName="HireDate" />
       <Condition ColumnName="HireDate" IsNull="false" />
       <Condition ColumnName="EnrollmentDate" IsNull="true" />
     </MappingFragment>
   </EntityTypeMapping>
   <EntityTypeMapping TypeName="IsTypeOf(SchoolModel.Student)">
     <MappingFragment StoreEntitySet="Person">
       <ScalarProperty Name="PersonID" ColumnName="PersonID" />
       <ScalarProperty Name="EnrollmentDate"
                       ColumnName="EnrollmentDate" />
       <Condition ColumnName="EnrollmentDate" IsNull="false" />
       <Condition ColumnName="HireDate" IsNull="true" />
     </MappingFragment>
   </EntityTypeMapping>
 </EntitySetMapping>
```

## <a name="deletefunction-element-msl"></a><span data-ttu-id="f41c0-289">Элемент DeleteFunction (MSL)</span><span class="sxs-lookup"><span data-stu-id="f41c0-289">DeleteFunction Element (MSL)</span></span>

<span data-ttu-id="f41c0-290">Элемент **DeleteFunction** в языке MSL сопоставляет функцию Delete типа сущности или ассоциации в концептуальной модели с хранимой процедурой в базовой базе данных.</span><span class="sxs-lookup"><span data-stu-id="f41c0-290">The **DeleteFunction** element in mapping specification language (MSL) maps the delete function of an entity type or association in the conceptual model to a stored procedure in the underlying database.</span></span> <span data-ttu-id="f41c0-291">Хранимые процедуры, с которыми сопоставляются функции изменения, должны объявляться в модели хранения.</span><span class="sxs-lookup"><span data-stu-id="f41c0-291">Stored procedures to which modification functions are mapped must be declared in the storage model.</span></span> <span data-ttu-id="f41c0-292">Дополнительные сведения см. в разделе элемент Function (SSDL).</span><span class="sxs-lookup"><span data-stu-id="f41c0-292">For more information, see Function Element (SSDL).</span></span>

> [!NOTE]
> <span data-ttu-id="f41c0-293">Если не выполнить сопоставление всех трех операций вставки, обновления или удаления типа сущности с хранимыми процедурами, несопоставленные операции завершатся ошибкой, если они выполняются во время выполнения и создается исключение Упдатиксцептион.</span><span class="sxs-lookup"><span data-stu-id="f41c0-293">If you do not map all three of the insert, update, or delete operations of a entity type to stored procedures, the unmapped operations will fail if executed at runtime and an UpdateException is thrown.</span></span>

### <a name="deletefunction-applied-to-entitytypemapping"></a><span data-ttu-id="f41c0-294">Элемент DeleteFunction, применяемый к EntityTypeMapping</span><span class="sxs-lookup"><span data-stu-id="f41c0-294">DeleteFunction Applied to EntityTypeMapping</span></span>

<span data-ttu-id="f41c0-295">При применении к элементу EntityTypeMapping элемент **DeleteFunction** сопоставляет функцию Delete типа сущности в концептуальной модели с хранимой процедурой.</span><span class="sxs-lookup"><span data-stu-id="f41c0-295">When applied to the EntityTypeMapping element, the **DeleteFunction** element maps the delete function of an entity type in the conceptual model to a stored procedure.</span></span>

<span data-ttu-id="f41c0-296">Элемент **DeleteFunction** может иметь следующие дочерние элементы при применении к элементу **EntityTypeMapping** :</span><span class="sxs-lookup"><span data-stu-id="f41c0-296">The **DeleteFunction** element can have the following child elements when applied to an **EntityTypeMapping** element:</span></span>

-   <span data-ttu-id="f41c0-297">AssociationEnd (ноль и более)</span><span class="sxs-lookup"><span data-stu-id="f41c0-297">AssociationEnd (zero or more)</span></span>
-   <span data-ttu-id="f41c0-298">ComplexProperty (ноль и более)</span><span class="sxs-lookup"><span data-stu-id="f41c0-298">ComplexProperty (zero or more)</span></span>
-   <span data-ttu-id="f41c0-299">ScarlarProperty (ноль и более)</span><span class="sxs-lookup"><span data-stu-id="f41c0-299">ScarlarProperty (zero or more)</span></span>

#### <a name="applicable-attributes"></a><span data-ttu-id="f41c0-300">Применимые атрибуты</span><span class="sxs-lookup"><span data-stu-id="f41c0-300">Applicable Attributes</span></span>

<span data-ttu-id="f41c0-301">В следующей таблице описаны атрибуты, которые можно применить к элементу **DeleteFunction** при его применении к элементу **EntityTypeMapping** .</span><span class="sxs-lookup"><span data-stu-id="f41c0-301">The following table describes the attributes that can be applied to the **DeleteFunction** element when it is applied to an **EntityTypeMapping** element.</span></span>

| <span data-ttu-id="f41c0-302">Имя атрибута</span><span class="sxs-lookup"><span data-stu-id="f41c0-302">Attribute Name</span></span>            | <span data-ttu-id="f41c0-303">Обязательный</span><span class="sxs-lookup"><span data-stu-id="f41c0-303">Is Required</span></span> | <span data-ttu-id="f41c0-304">Значение</span><span class="sxs-lookup"><span data-stu-id="f41c0-304">Value</span></span>                                                                                                                                                    |
|:--------------------------|:------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="f41c0-305">**FunctionName**</span><span class="sxs-lookup"><span data-stu-id="f41c0-305">**FunctionName**</span></span>          | <span data-ttu-id="f41c0-306">Да</span><span class="sxs-lookup"><span data-stu-id="f41c0-306">Yes</span></span>         | <span data-ttu-id="f41c0-307">Имя хранимой процедуры (с указанием пространства имен), с которой сопоставляется функция удаления.</span><span class="sxs-lookup"><span data-stu-id="f41c0-307">The namespace-qualified name of the stored procedure to which the delete function is mapped.</span></span> <span data-ttu-id="f41c0-308">Хранимая процедура должна объявляться в модели хранения.</span><span class="sxs-lookup"><span data-stu-id="f41c0-308">The stored procedure must be declared in the storage model.</span></span> |
| <span data-ttu-id="f41c0-309">**ровсаффектедпараметер**</span><span class="sxs-lookup"><span data-stu-id="f41c0-309">**RowsAffectedParameter**</span></span> | <span data-ttu-id="f41c0-310">Нет</span><span class="sxs-lookup"><span data-stu-id="f41c0-310">No</span></span>          | <span data-ttu-id="f41c0-311">Имя выходного параметра, возвращающего количество обработанных строк.</span><span class="sxs-lookup"><span data-stu-id="f41c0-311">The name of the output parameter that returns the number of rows affected.</span></span>                                                                               |

#### <a name="example"></a><span data-ttu-id="f41c0-312">Пример</span><span class="sxs-lookup"><span data-stu-id="f41c0-312">Example</span></span>

<span data-ttu-id="f41c0-313">Следующий пример основан на модели School и показывает элемент **DeleteFunction** , сопоставленный функции Delete типа сущности **Person** с хранимой процедурой **делетеперсон** .</span><span class="sxs-lookup"><span data-stu-id="f41c0-313">The following example is based on the School model and shows the **DeleteFunction** element mapping the delete function of the **Person** entity type to the **DeletePerson** stored procedure.</span></span> <span data-ttu-id="f41c0-314">Хранимая процедура **делетеперсон** объявляется в модели хранения.</span><span class="sxs-lookup"><span data-stu-id="f41c0-314">The **DeletePerson** stored procedure is declared in the storage model.</span></span>

``` xml
 <EntitySetMapping Name="People">
   <EntityTypeMapping TypeName="SchoolModel.Person">
     <MappingFragment StoreEntitySet="Person">
       <ScalarProperty Name="PersonID" ColumnName="PersonID" />
       <ScalarProperty Name="LastName" ColumnName="LastName" />
       <ScalarProperty Name="FirstName" ColumnName="FirstName" />
       <ScalarProperty Name="HireDate" ColumnName="HireDate" />
       <ScalarProperty Name="EnrollmentDate"
                       ColumnName="EnrollmentDate" />
     </MappingFragment>
 </EntityTypeMapping>
   <EntityTypeMapping TypeName="SchoolModel.Person">
     <ModificationFunctionMapping>
       <InsertFunction FunctionName="SchoolModel.Store.InsertPerson">
         <ScalarProperty Name="EnrollmentDate"
                         ParameterName="EnrollmentDate" />
         <ScalarProperty Name="HireDate" ParameterName="HireDate" />
         <ScalarProperty Name="FirstName" ParameterName="FirstName" />
         <ScalarProperty Name="LastName" ParameterName="LastName" />
         <ResultBinding Name="PersonID" ColumnName="NewPersonID" />
       </InsertFunction>
       <UpdateFunction FunctionName="SchoolModel.Store.UpdatePerson">
         <ScalarProperty Name="EnrollmentDate"
                         ParameterName="EnrollmentDate"
                         Version="Current" />
         <ScalarProperty Name="HireDate" ParameterName="HireDate"
                         Version="Current" />
         <ScalarProperty Name="FirstName" ParameterName="FirstName"
                         Version="Current" />
         <ScalarProperty Name="LastName" ParameterName="LastName"
                         Version="Current" />
         <ScalarProperty Name="PersonID" ParameterName="PersonID"
                         Version="Current" />
       </UpdateFunction>
       <DeleteFunction FunctionName="SchoolModel.Store.DeletePerson">
         <ScalarProperty Name="PersonID" ParameterName="PersonID" />
       </DeleteFunction>
     </ModificationFunctionMapping>
   </EntityTypeMapping>
 </EntitySetMapping>
```

### <a name="deletefunction-applied-to-associationsetmapping"></a><span data-ttu-id="f41c0-315">Элемент DeleteFunction, применяемый к AssociationSetMapping</span><span class="sxs-lookup"><span data-stu-id="f41c0-315">DeleteFunction Applied to AssociationSetMapping</span></span>

<span data-ttu-id="f41c0-316">При применении к элементу AssociationSetMapping элемент **DeleteFunction** сопоставляет функцию Delete ассоциации в концептуальной модели с хранимой процедурой.</span><span class="sxs-lookup"><span data-stu-id="f41c0-316">When applied to the AssociationSetMapping element, the **DeleteFunction** element maps the delete function of an association in the conceptual model to a stored procedure.</span></span>

<span data-ttu-id="f41c0-317">Элемент **DeleteFunction** может иметь следующие дочерние элементы при применении к элементу **AssociationSetMapping** :</span><span class="sxs-lookup"><span data-stu-id="f41c0-317">The **DeleteFunction** element can have the following child elements when applied to the **AssociationSetMapping** element:</span></span>

-   <span data-ttu-id="f41c0-318">EndProperty</span><span class="sxs-lookup"><span data-stu-id="f41c0-318">EndProperty</span></span>

#### <a name="applicable-attributes"></a><span data-ttu-id="f41c0-319">Применимые атрибуты</span><span class="sxs-lookup"><span data-stu-id="f41c0-319">Applicable Attributes</span></span>

<span data-ttu-id="f41c0-320">В следующей таблице описаны атрибуты, которые можно применить к элементу **DeleteFunction** при его применении к элементу **AssociationSetMapping** .</span><span class="sxs-lookup"><span data-stu-id="f41c0-320">The following table describes the attributes that can be applied to the **DeleteFunction** element when it is applied to the **AssociationSetMapping** element.</span></span>

| <span data-ttu-id="f41c0-321">Имя атрибута</span><span class="sxs-lookup"><span data-stu-id="f41c0-321">Attribute Name</span></span>            | <span data-ttu-id="f41c0-322">Обязательный</span><span class="sxs-lookup"><span data-stu-id="f41c0-322">Is Required</span></span> | <span data-ttu-id="f41c0-323">Значение</span><span class="sxs-lookup"><span data-stu-id="f41c0-323">Value</span></span>                                                                                                                                                    |
|:--------------------------|:------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="f41c0-324">**FunctionName**</span><span class="sxs-lookup"><span data-stu-id="f41c0-324">**FunctionName**</span></span>          | <span data-ttu-id="f41c0-325">Да</span><span class="sxs-lookup"><span data-stu-id="f41c0-325">Yes</span></span>         | <span data-ttu-id="f41c0-326">Имя хранимой процедуры (с указанием пространства имен), с которой сопоставляется функция удаления.</span><span class="sxs-lookup"><span data-stu-id="f41c0-326">The namespace-qualified name of the stored procedure to which the delete function is mapped.</span></span> <span data-ttu-id="f41c0-327">Хранимая процедура должна объявляться в модели хранения.</span><span class="sxs-lookup"><span data-stu-id="f41c0-327">The stored procedure must be declared in the storage model.</span></span> |
| <span data-ttu-id="f41c0-328">**ровсаффектедпараметер**</span><span class="sxs-lookup"><span data-stu-id="f41c0-328">**RowsAffectedParameter**</span></span> | <span data-ttu-id="f41c0-329">Нет</span><span class="sxs-lookup"><span data-stu-id="f41c0-329">No</span></span>          | <span data-ttu-id="f41c0-330">Имя выходного параметра, возвращающего количество обработанных строк.</span><span class="sxs-lookup"><span data-stu-id="f41c0-330">The name of the output parameter that returns the number of rows affected.</span></span>                                                                               |

#### <a name="example"></a><span data-ttu-id="f41c0-331">Пример</span><span class="sxs-lookup"><span data-stu-id="f41c0-331">Example</span></span>

<span data-ttu-id="f41c0-332">Следующий пример основан на модели School и показывает элемент **DeleteFunction** , используемый для сопоставления функции Delete ассоциации **каурсеинструктор** с хранимой процедурой **делетекаурсеинструктор** .</span><span class="sxs-lookup"><span data-stu-id="f41c0-332">The following example is based on the School model and shows the **DeleteFunction** element used to map delete function of the **CourseInstructor** association to the **DeleteCourseInstructor** stored procedure.</span></span> <span data-ttu-id="f41c0-333">Хранимая процедура **делетекаурсеинструктор** объявляется в модели хранения.</span><span class="sxs-lookup"><span data-stu-id="f41c0-333">The **DeleteCourseInstructor** stored procedure is declared in the storage model.</span></span>

``` xml
 <AssociationSetMapping Name="CourseInstructor"
                        TypeName="SchoolModel.CourseInstructor"
                        StoreEntitySet="CourseInstructor">
   <EndProperty Name="Person">
     <ScalarProperty Name="PersonID" ColumnName="PersonID" />
   </EndProperty>
   <EndProperty Name="Course">
     <ScalarProperty Name="CourseID" ColumnName="CourseID" />
   </EndProperty>
   <ModificationFunctionMapping>
     <InsertFunction FunctionName="SchoolModel.Store.InsertCourseInstructor" >   
       <EndProperty Name="Course">
         <ScalarProperty Name="CourseID" ParameterName="courseId"/>
       </EndProperty>
       <EndProperty Name="Person">
         <ScalarProperty Name="PersonID" ParameterName="instructorId"/>
       </EndProperty>
     </InsertFunction>
     <DeleteFunction FunctionName="SchoolModel.Store.DeleteCourseInstructor">
       <EndProperty Name="Course">
         <ScalarProperty Name="CourseID" ParameterName="courseId"/>
       </EndProperty>
       <EndProperty Name="Person">
         <ScalarProperty Name="PersonID" ParameterName="instructorId"/>
       </EndProperty>
     </DeleteFunction>
   </ModificationFunctionMapping>
 </AssociationSetMapping>
```

## <a name="endproperty-element-msl"></a><span data-ttu-id="f41c0-334">Элемент EndProperty (MSL)</span><span class="sxs-lookup"><span data-stu-id="f41c0-334">EndProperty Element (MSL)</span></span>

<span data-ttu-id="f41c0-335">Элемент **EndProperty** на языке MSL определяет сопоставление между функцией End или модификации ассоциации концептуальной модели и базовой базы данных.</span><span class="sxs-lookup"><span data-stu-id="f41c0-335">The **EndProperty** element in mapping specification language (MSL) defines the mapping between an end or a modification function of a conceptual model association and the underlying database.</span></span> <span data-ttu-id="f41c0-336">Сопоставление столбца свойств указано в дочернем элементе ScalarProperty.</span><span class="sxs-lookup"><span data-stu-id="f41c0-336">The property-column mapping is specified in a child ScalarProperty element.</span></span>

<span data-ttu-id="f41c0-337">Если элемент **EndProperty** используется для определения сопоставления в конце ассоциации концептуальной модели, он является дочерним элементом элемента AssociationSetMapping.</span><span class="sxs-lookup"><span data-stu-id="f41c0-337">When an **EndProperty** element is used to define the mapping for the end of a conceptual model association, it is a child of an AssociationSetMapping element.</span></span> <span data-ttu-id="f41c0-338">Если элемент **EndProperty** используется для определения сопоставления для функции изменения ассоциации концептуальной модели, это дочерний элемент элемента InsertFunction или DeleteFunction.</span><span class="sxs-lookup"><span data-stu-id="f41c0-338">When the **EndProperty** element is used to define the mapping for a modification function of a conceptual model association, it is a child of an InsertFunction element or DeleteFunction element.</span></span>

<span data-ttu-id="f41c0-339">Элемент **EndProperty** может иметь следующие дочерние элементы:</span><span class="sxs-lookup"><span data-stu-id="f41c0-339">The **EndProperty** element can have the following child elements:</span></span>

-   <span data-ttu-id="f41c0-340">ScalarProperty (ноль и более)</span><span class="sxs-lookup"><span data-stu-id="f41c0-340">ScalarProperty (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="f41c0-341">Применимые атрибуты</span><span class="sxs-lookup"><span data-stu-id="f41c0-341">Applicable Attributes</span></span>

<span data-ttu-id="f41c0-342">В следующей таблице описаны атрибуты, применимые к элементу **EndProperty** .</span><span class="sxs-lookup"><span data-stu-id="f41c0-342">The following table describes the attributes that are applicable to the **EndProperty** element:</span></span>

| <span data-ttu-id="f41c0-343">Имя атрибута</span><span class="sxs-lookup"><span data-stu-id="f41c0-343">Attribute Name</span></span> | <span data-ttu-id="f41c0-344">Обязательный</span><span class="sxs-lookup"><span data-stu-id="f41c0-344">Is Required</span></span> | <span data-ttu-id="f41c0-345">Значение</span><span class="sxs-lookup"><span data-stu-id="f41c0-345">Value</span></span>                                                 |
|:---------------|:------------|:------------------------------------------------------|
| <span data-ttu-id="f41c0-346">Имя</span><span class="sxs-lookup"><span data-stu-id="f41c0-346">Name</span></span>           | <span data-ttu-id="f41c0-347">Да</span><span class="sxs-lookup"><span data-stu-id="f41c0-347">Yes</span></span>         | <span data-ttu-id="f41c0-348">Имя сопоставляемого конца ассоциации.</span><span class="sxs-lookup"><span data-stu-id="f41c0-348">The name of the association end that is being mapped.</span></span> |

### <a name="example"></a><span data-ttu-id="f41c0-349">Пример</span><span class="sxs-lookup"><span data-stu-id="f41c0-349">Example</span></span>

<span data-ttu-id="f41c0-350">В следующем примере показан элемент **AssociationSetMapping** , в котором связь \*\* \_ \_ отдела FK Course\*\* в концептуальной модели сопоставляется с таблицей **Course** в базе данных.</span><span class="sxs-lookup"><span data-stu-id="f41c0-350">The following example shows an **AssociationSetMapping** element in which the **FK\_Course\_Department** association in the conceptual model is mapped to the **Course** table in the database.</span></span> <span data-ttu-id="f41c0-351">Сопоставления между свойствами типа ассоциации и столбцами таблицы указываются в дочерних элементах **EndProperty** .</span><span class="sxs-lookup"><span data-stu-id="f41c0-351">Mappings between association type properties and table columns are specified in child **EndProperty** elements.</span></span>

``` xml
 <AssociationSetMapping Name="FK_Course_Department"
                        TypeName="SchoolModel.FK_Course_Department"
                        StoreEntitySet="Course">
   <EndProperty Name="Department">
     <ScalarProperty Name="DepartmentID" ColumnName="DepartmentID" />
   </EndProperty>
   <EndProperty Name="Course">
     <ScalarProperty Name="CourseID" ColumnName="CourseID" />
   </EndProperty>
 </AssociationSetMapping>
```

### <a name="example"></a><span data-ttu-id="f41c0-352">Пример</span><span class="sxs-lookup"><span data-stu-id="f41c0-352">Example</span></span>

<span data-ttu-id="f41c0-353">В следующем примере показан элемент **EndProperty** , сопоставленный с функциями INSERT и DELETE ассоциации (**каурсеинструктор**) с хранимыми процедурами в базовой базе данных.</span><span class="sxs-lookup"><span data-stu-id="f41c0-353">The following example shows the **EndProperty** element mapping the insert and delete functions of an association (**CourseInstructor**) to stored procedures in the underlying database.</span></span> <span data-ttu-id="f41c0-354">Функции, с которыми выполняется сопоставление, объявляются в модели хранения.</span><span class="sxs-lookup"><span data-stu-id="f41c0-354">The functions that are mapped to are declared in the storage model.</span></span>

``` xml
 <AssociationSetMapping Name="CourseInstructor"
                        TypeName="SchoolModel.CourseInstructor"
                        StoreEntitySet="CourseInstructor">
   <EndProperty Name="Person">
     <ScalarProperty Name="PersonID" ColumnName="PersonID" />
   </EndProperty>
   <EndProperty Name="Course">
     <ScalarProperty Name="CourseID" ColumnName="CourseID" />
   </EndProperty>
   <ModificationFunctionMapping>
     <InsertFunction FunctionName="SchoolModel.Store.InsertCourseInstructor" >   
       <EndProperty Name="Course">
         <ScalarProperty Name="CourseID" ParameterName="courseId"/>
       </EndProperty>
       <EndProperty Name="Person">
         <ScalarProperty Name="PersonID" ParameterName="instructorId"/>
       </EndProperty>
     </InsertFunction>
     <DeleteFunction FunctionName="SchoolModel.Store.DeleteCourseInstructor">
       <EndProperty Name="Course">
         <ScalarProperty Name="CourseID" ParameterName="courseId"/>
       </EndProperty>
       <EndProperty Name="Person">
         <ScalarProperty Name="PersonID" ParameterName="instructorId"/>
       </EndProperty>
     </DeleteFunction>
   </ModificationFunctionMapping>
 </AssociationSetMapping>
```

## <a name="entitycontainermapping-element-msl"></a><span data-ttu-id="f41c0-355">Элемент EntityContainerMapping (MSL)</span><span class="sxs-lookup"><span data-stu-id="f41c0-355">EntityContainerMapping Element (MSL)</span></span>

<span data-ttu-id="f41c0-356">Элемент **EntityContainerMapping** на языке MSL сопоставляет контейнер сущностей в концептуальной модели с контейнером сущностей в модели хранения.</span><span class="sxs-lookup"><span data-stu-id="f41c0-356">The **EntityContainerMapping** element in mapping specification language (MSL) maps the entity container in the conceptual model to the entity container in the storage model.</span></span> <span data-ttu-id="f41c0-357">Элемент **EntityContainerMapping** является дочерним по отношению к элементу Mapping.</span><span class="sxs-lookup"><span data-stu-id="f41c0-357">The **EntityContainerMapping** element is a child of the Mapping element.</span></span>

<span data-ttu-id="f41c0-358">Элемент **EntityContainerMapping** может иметь следующие дочерние элементы (в указанном порядке):</span><span class="sxs-lookup"><span data-stu-id="f41c0-358">The **EntityContainerMapping** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="f41c0-359">EntitySetMapping (ноль и более)</span><span class="sxs-lookup"><span data-stu-id="f41c0-359">EntitySetMapping (zero or more)</span></span>
-   <span data-ttu-id="f41c0-360">AssociationSetMapping (ноль и более)</span><span class="sxs-lookup"><span data-stu-id="f41c0-360">AssociationSetMapping (zero or more)</span></span>
-   <span data-ttu-id="f41c0-361">FunctionImportMapping (ноль и более)</span><span class="sxs-lookup"><span data-stu-id="f41c0-361">FunctionImportMapping (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="f41c0-362">Применимые атрибуты</span><span class="sxs-lookup"><span data-stu-id="f41c0-362">Applicable Attributes</span></span>

<span data-ttu-id="f41c0-363">В следующей таблице описаны атрибуты, которые можно применить к элементу **EntityContainerMapping** .</span><span class="sxs-lookup"><span data-stu-id="f41c0-363">The following table describes the attributes that can be applied to the **EntityContainerMapping** element.</span></span>

| <span data-ttu-id="f41c0-364">Имя атрибута</span><span class="sxs-lookup"><span data-stu-id="f41c0-364">Attribute Name</span></span>            | <span data-ttu-id="f41c0-365">Обязательный</span><span class="sxs-lookup"><span data-stu-id="f41c0-365">Is Required</span></span> | <span data-ttu-id="f41c0-366">Значение</span><span class="sxs-lookup"><span data-stu-id="f41c0-366">Value</span></span>                                                                                                                                                                                                                                                    |
|:--------------------------|:------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="f41c0-367">**сторажемоделконтаинер**</span><span class="sxs-lookup"><span data-stu-id="f41c0-367">**StorageModelContainer**</span></span> | <span data-ttu-id="f41c0-368">Да</span><span class="sxs-lookup"><span data-stu-id="f41c0-368">Yes</span></span>         | <span data-ttu-id="f41c0-369">Имя контейнера сущностей модели хранения, с которым выполняется сопоставление.</span><span class="sxs-lookup"><span data-stu-id="f41c0-369">The name of the storage model entity container that is being mapped.</span></span>                                                                                                                                                                                     |
| <span data-ttu-id="f41c0-370">**кдментитиконтаинер**</span><span class="sxs-lookup"><span data-stu-id="f41c0-370">**CdmEntityContainer**</span></span>    | <span data-ttu-id="f41c0-371">Да</span><span class="sxs-lookup"><span data-stu-id="f41c0-371">Yes</span></span>         | <span data-ttu-id="f41c0-372">Имя контейнера сущностей концептуальной модели, с которым выполняется сопоставление.</span><span class="sxs-lookup"><span data-stu-id="f41c0-372">The name of the conceptual model entity container that is being mapped.</span></span>                                                                                                                                                                                  |
| <span data-ttu-id="f41c0-373">**женератеупдатевиевс**</span><span class="sxs-lookup"><span data-stu-id="f41c0-373">**GenerateUpdateViews**</span></span>   | <span data-ttu-id="f41c0-374">Нет</span><span class="sxs-lookup"><span data-stu-id="f41c0-374">No</span></span>          | <span data-ttu-id="f41c0-375">**True** или **false**.</span><span class="sxs-lookup"><span data-stu-id="f41c0-375">**True** or **False**.</span></span> <span data-ttu-id="f41c0-376">Если задано **значение false**, представления обновлений не создаются.</span><span class="sxs-lookup"><span data-stu-id="f41c0-376">If **False**, no update views are generated.</span></span> <span data-ttu-id="f41c0-377">Этот атрибут должен иметь значение **false** , если имеется сопоставление только для чтения, которое будет недействительным, так как данные могут не пройти циклический обмен данными.</span><span class="sxs-lookup"><span data-stu-id="f41c0-377">This attribute should be set to **False** when you have a read-only mapping that would be invalid because data may not round-trip successfully.</span></span> <br/> <span data-ttu-id="f41c0-378">По умолчанию используется значение **True**.</span><span class="sxs-lookup"><span data-stu-id="f41c0-378">The default value is **True**.</span></span> |

### <a name="example"></a><span data-ttu-id="f41c0-379">Пример</span><span class="sxs-lookup"><span data-stu-id="f41c0-379">Example</span></span>

<span data-ttu-id="f41c0-380">В следующем примере показан элемент **EntityContainerMapping** , который сопоставляет контейнер **счулмоделентитиес** (контейнер сущностей концептуальной модели) с контейнером **счулмоделстореконтаинер** (контейнер сущностей модели хранения):</span><span class="sxs-lookup"><span data-stu-id="f41c0-380">The following example shows an **EntityContainerMapping** element that maps the **SchoolModelEntities** container (the conceptual model entity container) to the **SchoolModelStoreContainer** container (the storage model entity container):</span></span>

``` xml
 <EntityContainerMapping StorageEntityContainer="SchoolModelStoreContainer"
                         CdmEntityContainer="SchoolModelEntities">
   <EntitySetMapping Name="Courses">
     <EntityTypeMapping TypeName="c.Course">
       <MappingFragment StoreEntitySet="Course">
         <ScalarProperty Name="CourseID" ColumnName="CourseID" />
         <ScalarProperty Name="Title" ColumnName="Title" />
         <ScalarProperty Name="Credits" ColumnName="Credits" />
         <ScalarProperty Name="DepartmentID" ColumnName="DepartmentID" />
       </MappingFragment>
     </EntityTypeMapping>
   </EntitySetMapping>
   <EntitySetMapping Name="Departments">
     <EntityTypeMapping TypeName="c.Department">
       <MappingFragment StoreEntitySet="Department">
         <ScalarProperty Name="DepartmentID" ColumnName="DepartmentID" />
         <ScalarProperty Name="Name" ColumnName="Name" />
         <ScalarProperty Name="Budget" ColumnName="Budget" />
         <ScalarProperty Name="StartDate" ColumnName="StartDate" />
         <ScalarProperty Name="Administrator" ColumnName="Administrator" />
       </MappingFragment>
     </EntityTypeMapping>
   </EntitySetMapping>
 </EntityContainerMapping>
```

## <a name="entitysetmapping-element-msl"></a><span data-ttu-id="f41c0-381">Элемент EntitySetMapping (MSL)</span><span class="sxs-lookup"><span data-stu-id="f41c0-381">EntitySetMapping Element (MSL)</span></span>

<span data-ttu-id="f41c0-382">Элемент **EntitySetMapping** на языке MSL сопоставляет все типы сущности концептуальной модели наборам сущностей в модели хранения.</span><span class="sxs-lookup"><span data-stu-id="f41c0-382">The **EntitySetMapping** element in mapping specification language (MSL) maps all types in a conceptual model entity set to entity sets in the storage model.</span></span> <span data-ttu-id="f41c0-383">Набор сущностей в концептуальной модели представляет собой логический контейнер для экземпляров сущностей одного типа (и производных типов).</span><span class="sxs-lookup"><span data-stu-id="f41c0-383">An entity set in the conceptual model is a logical container for instances of entities of the same type (and derived types).</span></span> <span data-ttu-id="f41c0-384">Набор сущностей в модели хранения представляет таблицу или представление в основной базе данных.</span><span class="sxs-lookup"><span data-stu-id="f41c0-384">An entity set in the storage model represents a table or view in the underlying database.</span></span> <span data-ttu-id="f41c0-385">Набор сущностей концептуальной модели задается значением атрибута **Name** элемента **EntitySetMapping** .</span><span class="sxs-lookup"><span data-stu-id="f41c0-385">The conceptual model entity set is specified by the value of the **Name** attribute of the **EntitySetMapping** element.</span></span> <span data-ttu-id="f41c0-386">Сопоставленная таблица или представление задаются атрибутом **сторинтитисет** в каждом дочернем элементе MappingFragment или в самом элементе **EntitySetMapping** .</span><span class="sxs-lookup"><span data-stu-id="f41c0-386">The mapped-to table or view is specified by the **StoreEntitySet** attribute in each child MappingFragment element or in the **EntitySetMapping** element itself.</span></span>

<span data-ttu-id="f41c0-387">Элемент **EntitySetMapping** может иметь следующие дочерние элементы:</span><span class="sxs-lookup"><span data-stu-id="f41c0-387">The **EntitySetMapping** element can have the following child elements:</span></span>

-   <span data-ttu-id="f41c0-388">EntityTypeMapping (ноль и более)</span><span class="sxs-lookup"><span data-stu-id="f41c0-388">EntityTypeMapping (zero or more)</span></span>
-   <span data-ttu-id="f41c0-389">QueryView (ноль или один)</span><span class="sxs-lookup"><span data-stu-id="f41c0-389">QueryView (zero or one)</span></span>
-   <span data-ttu-id="f41c0-390">MappingFragment (ноль и более)</span><span class="sxs-lookup"><span data-stu-id="f41c0-390">MappingFragment (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="f41c0-391">Применимые атрибуты</span><span class="sxs-lookup"><span data-stu-id="f41c0-391">Applicable Attributes</span></span>

<span data-ttu-id="f41c0-392">В следующей таблице описаны атрибуты, которые можно применить к элементу **EntitySetMapping** .</span><span class="sxs-lookup"><span data-stu-id="f41c0-392">The following table describes the attributes that can be applied to the **EntitySetMapping** element.</span></span>

| <span data-ttu-id="f41c0-393">Имя атрибута</span><span class="sxs-lookup"><span data-stu-id="f41c0-393">Attribute Name</span></span>           | <span data-ttu-id="f41c0-394">Обязательный</span><span class="sxs-lookup"><span data-stu-id="f41c0-394">Is Required</span></span> | <span data-ttu-id="f41c0-395">Значение</span><span class="sxs-lookup"><span data-stu-id="f41c0-395">Value</span></span>                                                                                                                                                                                                                         |
|:-------------------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="f41c0-396">**имя**;</span><span class="sxs-lookup"><span data-stu-id="f41c0-396">**Name**</span></span>                 | <span data-ttu-id="f41c0-397">Да</span><span class="sxs-lookup"><span data-stu-id="f41c0-397">Yes</span></span>         | <span data-ttu-id="f41c0-398">Имя сопоставляемого набора сущностей концептуальной модели.</span><span class="sxs-lookup"><span data-stu-id="f41c0-398">The name of the conceptual model entity set that is being mapped.</span></span>                                                                                                                                                             |
| <span data-ttu-id="f41c0-399">**Имя типа** **1**</span><span class="sxs-lookup"><span data-stu-id="f41c0-399">**TypeName** **1**</span></span>       | <span data-ttu-id="f41c0-400">Нет</span><span class="sxs-lookup"><span data-stu-id="f41c0-400">No</span></span>          | <span data-ttu-id="f41c0-401">Имя сопоставляемого типа сущности концептуальной модели.</span><span class="sxs-lookup"><span data-stu-id="f41c0-401">The name of the conceptual model entity type that is being mapped.</span></span>                                                                                                                                                            |
| <span data-ttu-id="f41c0-402">**Сторинтитисет** **1**</span><span class="sxs-lookup"><span data-stu-id="f41c0-402">**StoreEntitySet** **1**</span></span> | <span data-ttu-id="f41c0-403">Нет</span><span class="sxs-lookup"><span data-stu-id="f41c0-403">No</span></span>          | <span data-ttu-id="f41c0-404">Имя сопоставляемого набора сущностей модели хранения.</span><span class="sxs-lookup"><span data-stu-id="f41c0-404">The name of the storage model entity set that is being mapped to.</span></span>                                                                                                                                                             |
| <span data-ttu-id="f41c0-405">**Атрибутом makecolumnsdistinct**</span><span class="sxs-lookup"><span data-stu-id="f41c0-405">**MakeColumnsDistinct**</span></span>  | <span data-ttu-id="f41c0-406">Нет</span><span class="sxs-lookup"><span data-stu-id="f41c0-406">No</span></span>          | <span data-ttu-id="f41c0-407">**Значение true** или **false** в зависимости от того, возвращаются ли только отдельные строки.</span><span class="sxs-lookup"><span data-stu-id="f41c0-407">**True** or **False** depending on whether only distinct rows are returned.</span></span> <br/> <span data-ttu-id="f41c0-408">Если для этого атрибута задано значение **true**, атрибуту **женератеупдатевиевс** элемента EntityContainerMapping должно быть присвоено значение **false**.</span><span class="sxs-lookup"><span data-stu-id="f41c0-408">If this attribute is set to **True**, the **GenerateUpdateViews** attribute of the EntityContainerMapping element must be set to **False**.</span></span> |

 

<span data-ttu-id="f41c0-409">**1** для отображения одного типа сущности в одну таблицу можно использовать атрибуты **TypeName** и **сторинтитисет** вместо дочерних элементов EntityTypeMapping и MappingFragment.</span><span class="sxs-lookup"><span data-stu-id="f41c0-409">**1** The **TypeName** and **StoreEntitySet** attributes can be used in place of the EntityTypeMapping and MappingFragment child elements to map a single entity type to a single table.</span></span>

### <a name="example"></a><span data-ttu-id="f41c0-410">Пример</span><span class="sxs-lookup"><span data-stu-id="f41c0-410">Example</span></span>

<span data-ttu-id="f41c0-411">В следующем примере показан элемент **EntitySetMapping** , сопоставляющий три типа (базовый тип и два производных типа) в наборе сущностей **Courses** концептуальной модели с тремя различными таблицами в основной базе данных.</span><span class="sxs-lookup"><span data-stu-id="f41c0-411">The following example shows an **EntitySetMapping** element that maps three types (a base type and two derived types) in the **Courses** entity set of the conceptual model to three different tables in the underlying database.</span></span> <span data-ttu-id="f41c0-412">Таблицы задаются атрибутом **сторинтитисет** в каждом элементе **MappingFragment** .</span><span class="sxs-lookup"><span data-stu-id="f41c0-412">The tables are specified by the **StoreEntitySet** attribute in each **MappingFragment** element.</span></span>

``` xml
 <EntitySetMapping Name="Courses">
   <EntityTypeMapping TypeName="IsTypeOf(SchoolModel1.Course)">
     <MappingFragment StoreEntitySet="Course">
       <ScalarProperty Name="CourseID" ColumnName="CourseID" />
       <ScalarProperty Name="DepartmentID" ColumnName="DepartmentID" />
       <ScalarProperty Name="Credits" ColumnName="Credits" />
       <ScalarProperty Name="Title" ColumnName="Title" />
     </MappingFragment>
   </EntityTypeMapping>
   <EntityTypeMapping TypeName="IsTypeOf(SchoolModel1.OnlineCourse)">
     <MappingFragment StoreEntitySet="OnlineCourse">
       <ScalarProperty Name="CourseID" ColumnName="CourseID" />
       <ScalarProperty Name="URL" ColumnName="URL" />
     </MappingFragment>
   </EntityTypeMapping>
   <EntityTypeMapping TypeName="IsTypeOf(SchoolModel1.OnsiteCourse)">
     <MappingFragment StoreEntitySet="OnsiteCourse">
       <ScalarProperty Name="CourseID" ColumnName="CourseID" />
       <ScalarProperty Name="Time" ColumnName="Time" />
       <ScalarProperty Name="Days" ColumnName="Days" />
       <ScalarProperty Name="Location" ColumnName="Location" />
     </MappingFragment>
   </EntityTypeMapping>
 </EntitySetMapping>
```

## <a name="entitytypemapping-element-msl"></a><span data-ttu-id="f41c0-413">Элемент EntityTypeMapping (MSL)</span><span class="sxs-lookup"><span data-stu-id="f41c0-413">EntityTypeMapping Element (MSL)</span></span>

<span data-ttu-id="f41c0-414">Элемент **EntityTypeMapping** на языке MSL определяет сопоставление между типом сущности в концептуальной модели и таблицах или представлениях в базовой базе данных.</span><span class="sxs-lookup"><span data-stu-id="f41c0-414">The **EntityTypeMapping** element in mapping specification language (MSL) defines the mapping between an entity type in the conceptual model and tables or views in the underlying database.</span></span> <span data-ttu-id="f41c0-415">Сведения о типах сущностей концептуальной модели и базовых таблицах или представлениях базы данных см. в разделах Элемент EntityType (язык CSDL) и Элемент EntitySet (язык SSDL).</span><span class="sxs-lookup"><span data-stu-id="f41c0-415">For information about conceptual model entity types and underlying database tables or views, see EntityType Element (CSDL) and EntitySet Element (SSDL).</span></span> <span data-ttu-id="f41c0-416">Сопоставляемый тип сущности концептуальной модели задается атрибутом **TypeName** элемента **EntityTypeMapping** .</span><span class="sxs-lookup"><span data-stu-id="f41c0-416">The conceptual model entity type that is being mapped is specified by the **TypeName** attribute of the **EntityTypeMapping** element.</span></span> <span data-ttu-id="f41c0-417">Сопоставляемая таблица или представление задается атрибутом **сторинтитисет** дочернего элемента MappingFragment.</span><span class="sxs-lookup"><span data-stu-id="f41c0-417">The table or view that is being mapped is specified by the **StoreEntitySet** attribute of the child MappingFragment element.</span></span>

<span data-ttu-id="f41c0-418">Дочерний элемент ModificationFunctionMapping может применяться для сопоставления функций вставки, обновления или удаления типов сущностей с хранимыми процедурами в базе данных.</span><span class="sxs-lookup"><span data-stu-id="f41c0-418">The ModificationFunctionMapping child element can be used to map the insert, update, or delete functions of entity types to stored procedures in the database.</span></span>

<span data-ttu-id="f41c0-419">Элемент **EntityTypeMapping** может иметь следующие дочерние элементы:</span><span class="sxs-lookup"><span data-stu-id="f41c0-419">The **EntityTypeMapping** element can have the following child elements:</span></span>

-   <span data-ttu-id="f41c0-420">MappingFragment (ноль и более)</span><span class="sxs-lookup"><span data-stu-id="f41c0-420">MappingFragment (zero or more)</span></span>
-   <span data-ttu-id="f41c0-421">ModificationFunctionMapping (ноль или один)</span><span class="sxs-lookup"><span data-stu-id="f41c0-421">ModificationFunctionMapping (zero or one)</span></span>
-   <span data-ttu-id="f41c0-422">ScalarProperty</span><span class="sxs-lookup"><span data-stu-id="f41c0-422">ScalarProperty</span></span>
-   <span data-ttu-id="f41c0-423">Условие</span><span class="sxs-lookup"><span data-stu-id="f41c0-423">Condition</span></span>

> [!NOTE]
> <span data-ttu-id="f41c0-424">Элементы **MappingFragment** и **ModificationFunctionMapping** не могут одновременно быть дочерними элементами элемента **EntityTypeMapping** .</span><span class="sxs-lookup"><span data-stu-id="f41c0-424">**MappingFragment** and **ModificationFunctionMapping** elements cannot be child elements of the **EntityTypeMapping** element at the same time.</span></span>


> [!NOTE]
> <span data-ttu-id="f41c0-425">Элементы **ScalarProperty** и **Condition** могут быть дочерними элементами элемента **EntityTypeMapping** , когда он используется в элементе FunctionImportMapping.</span><span class="sxs-lookup"><span data-stu-id="f41c0-425">The **ScalarProperty** and **Condition** elements can only be child elements of the **EntityTypeMapping** element when it is used within a FunctionImportMapping element.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="f41c0-426">Применимые атрибуты</span><span class="sxs-lookup"><span data-stu-id="f41c0-426">Applicable Attributes</span></span>

<span data-ttu-id="f41c0-427">В следующей таблице описаны атрибуты, которые можно применить к элементу **EntityTypeMapping** .</span><span class="sxs-lookup"><span data-stu-id="f41c0-427">The following table describes the attributes that can be applied to the **EntityTypeMapping** element.</span></span>

| <span data-ttu-id="f41c0-428">Имя атрибута</span><span class="sxs-lookup"><span data-stu-id="f41c0-428">Attribute Name</span></span> | <span data-ttu-id="f41c0-429">Обязательный</span><span class="sxs-lookup"><span data-stu-id="f41c0-429">Is Required</span></span> | <span data-ttu-id="f41c0-430">Значение</span><span class="sxs-lookup"><span data-stu-id="f41c0-430">Value</span></span>                                                                                                                                                                                                |
|:---------------|:------------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="f41c0-431">**Имени**</span><span class="sxs-lookup"><span data-stu-id="f41c0-431">**TypeName**</span></span>   | <span data-ttu-id="f41c0-432">Да</span><span class="sxs-lookup"><span data-stu-id="f41c0-432">Yes</span></span>         | <span data-ttu-id="f41c0-433">Имя с указанием пространства имен типа сущности концептуальной модели, с которым выполняется сопоставление.</span><span class="sxs-lookup"><span data-stu-id="f41c0-433">The namespace-qualified name of the conceptual model entity type that is being mapped.</span></span> <br/> <span data-ttu-id="f41c0-434">Если тип является абстрактным или производным типом, значение должно быть равно `IsOfType(Namespace-qualified_type_name)`.</span><span class="sxs-lookup"><span data-stu-id="f41c0-434">If the type is abstract or a derived type, the value must be `IsOfType(Namespace-qualified_type_name)`.</span></span> |

### <a name="example"></a><span data-ttu-id="f41c0-435">Пример</span><span class="sxs-lookup"><span data-stu-id="f41c0-435">Example</span></span>

<span data-ttu-id="f41c0-436">В следующем примере показан элемент EntitySetMapping с двумя дочерними элементами **EntityTypeMapping** .</span><span class="sxs-lookup"><span data-stu-id="f41c0-436">The following example shows an EntitySetMapping element with two child **EntityTypeMapping** elements.</span></span> <span data-ttu-id="f41c0-437">В первом элементе **EntityTypeMapping** тип сущности **счулмодел. Person** сопоставляется с таблицей **Person** .</span><span class="sxs-lookup"><span data-stu-id="f41c0-437">In the first **EntityTypeMapping** element, the **SchoolModel.Person** entity type is mapped to the **Person** table.</span></span> <span data-ttu-id="f41c0-438">Во втором элементе **EntityTypeMapping** функция обновления типа **счулмодел. Person** сопоставляется с хранимой процедурой **упдатеперсон**в базе данных.</span><span class="sxs-lookup"><span data-stu-id="f41c0-438">In the second **EntityTypeMapping** element, the update functionality of the **SchoolModel.Person** type is mapped to a stored procedure, **UpdatePerson**, in the database.</span></span>

``` xml
 <EntitySetMapping Name="People">
   <EntityTypeMapping TypeName="SchoolModel.Person">
     <MappingFragment StoreEntitySet="Person">
       <ScalarProperty Name="PersonID" ColumnName="PersonID" />
       <ScalarProperty Name="LastName" ColumnName="LastName" />
       <ScalarProperty Name="FirstName" ColumnName="FirstName" />
       <ScalarProperty Name="HireDate" ColumnName="HireDate" />
       <ScalarProperty Name="EnrollmentDate" ColumnName="EnrollmentDate" />
     </MappingFragment>
   </EntityTypeMapping>
   <EntityTypeMapping TypeName="SchoolModel.Person">
     <ModificationFunctionMapping>
       <UpdateFunction FunctionName="SchoolModel.Store.UpdatePerson">
         <ScalarProperty Name="EnrollmentDate" ParameterName="EnrollmentDate"
                         Version="Current" />
         <ScalarProperty Name="HireDate" ParameterName="HireDate"
                         Version="Current" />
         <ScalarProperty Name="FirstName" ParameterName="FirstName"
                         Version="Current" />
         <ScalarProperty Name="LastName" ParameterName="LastName"
                         Version="Current" />
         <ScalarProperty Name="PersonID" ParameterName="PersonID"
                         Version="Current" />
       </UpdateFunction>
     </ModificationFunctionMapping>
   </EntityTypeMapping>
 </EntitySetMapping>
```

### <a name="example"></a><span data-ttu-id="f41c0-439">Пример</span><span class="sxs-lookup"><span data-stu-id="f41c0-439">Example</span></span>

<span data-ttu-id="f41c0-440">В следующем примере показано сопоставление иерархии типа, в которой корневой тип является абстрактным.</span><span class="sxs-lookup"><span data-stu-id="f41c0-440">The next example shows the mapping of a type hierarchy in which the root type is abstract.</span></span> <span data-ttu-id="f41c0-441">Обратите внимание на использование `IsOfType` синтаксиса для атрибутов **TypeName** .</span><span class="sxs-lookup"><span data-stu-id="f41c0-441">Note the use of the `IsOfType` syntax for the **TypeName** attributes.</span></span>

``` xml
 <EntitySetMapping Name="People">
   <EntityTypeMapping TypeName="IsTypeOf(SchoolModel.Person)">
     <MappingFragment StoreEntitySet="Person">
       <ScalarProperty Name="PersonID" ColumnName="PersonID" />
       <ScalarProperty Name="FirstName" ColumnName="FirstName" />
       <ScalarProperty Name="LastName" ColumnName="LastName" />
     </MappingFragment>
   </EntityTypeMapping>
   <EntityTypeMapping TypeName="IsTypeOf(SchoolModel.Instructor)">
     <MappingFragment StoreEntitySet="Person">
       <ScalarProperty Name="PersonID" ColumnName="PersonID" />
       <ScalarProperty Name="HireDate" ColumnName="HireDate" />
       <Condition ColumnName="HireDate" IsNull="false" />
       <Condition ColumnName="EnrollmentDate" IsNull="true" />
     </MappingFragment>
   </EntityTypeMapping>
   <EntityTypeMapping TypeName="IsTypeOf(SchoolModel.Student)">
     <MappingFragment StoreEntitySet="Person">
       <ScalarProperty Name="PersonID" ColumnName="PersonID" />
       <ScalarProperty Name="EnrollmentDate"
                       ColumnName="EnrollmentDate" />
       <Condition ColumnName="EnrollmentDate" IsNull="false" />
       <Condition ColumnName="HireDate" IsNull="true" />
     </MappingFragment>
   </EntityTypeMapping>
 </EntitySetMapping>
```

## <a name="functionimportmapping-element-msl"></a><span data-ttu-id="f41c0-442">Элемент FunctionImportMapping (MSL)</span><span class="sxs-lookup"><span data-stu-id="f41c0-442">FunctionImportMapping Element (MSL)</span></span>

<span data-ttu-id="f41c0-443">Элемент **FunctionImportMapping** на языке MSL определяет сопоставление между импортом функции в концептуальной модели и хранимой процедурой или функцией в базовой базе данных.</span><span class="sxs-lookup"><span data-stu-id="f41c0-443">The **FunctionImportMapping** element in mapping specification language (MSL) defines the mapping between a function import in the conceptual model and a stored procedure or function in the underlying database.</span></span> <span data-ttu-id="f41c0-444">Импорт функций должен быть объявлен в концептуальной модели, а хранимые процедуры должны быть объявлены в модели хранения.</span><span class="sxs-lookup"><span data-stu-id="f41c0-444">Function imports must be declared in the conceptual model and stored procedures must be declared in the storage model.</span></span> <span data-ttu-id="f41c0-445">Дополнительные сведения см. в разделе элемент FunctionImport (CSDL) и элемент Function (SSDL).</span><span class="sxs-lookup"><span data-stu-id="f41c0-445">For more information, see FunctionImport Element (CSDL) and Function Element (SSDL).</span></span>

> [!NOTE]
> <span data-ttu-id="f41c0-446">По умолчанию импорт функции возвращает тип сущности концептуальной модели или сложный тип. В этом случае имена столбцов, которые вернула основная хранимая процедура, должны точно соответствовать именам свойств в типе концептуальной модели.</span><span class="sxs-lookup"><span data-stu-id="f41c0-446">By default, if a function import returns a conceptual model entity type or complex type, then the names of the columns returned by the underlying stored procedure must exactly match the names of the properties on the conceptual model type.</span></span> <span data-ttu-id="f41c0-447">Если имена столбцов не точно совпадают с именами свойств, сопоставление должно быть определено в элементе Ресултмаппинг.</span><span class="sxs-lookup"><span data-stu-id="f41c0-447">If the column names do not exactly match the property names, the mapping must be defined in a ResultMapping element.</span></span>

<span data-ttu-id="f41c0-448">Элемент **FunctionImportMapping** может иметь следующие дочерние элементы:</span><span class="sxs-lookup"><span data-stu-id="f41c0-448">The **FunctionImportMapping** element can have the following child elements:</span></span>

-   <span data-ttu-id="f41c0-449">Ресултмаппинг (ноль или более)</span><span class="sxs-lookup"><span data-stu-id="f41c0-449">ResultMapping (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="f41c0-450">Применимые атрибуты</span><span class="sxs-lookup"><span data-stu-id="f41c0-450">Applicable Attributes</span></span>

<span data-ttu-id="f41c0-451">В следующей таблице описаны атрибуты, применимые к элементу **FunctionImportMapping** .</span><span class="sxs-lookup"><span data-stu-id="f41c0-451">The following table describes the attributes that are applicable to the **FunctionImportMapping** element:</span></span>

| <span data-ttu-id="f41c0-452">Имя атрибута</span><span class="sxs-lookup"><span data-stu-id="f41c0-452">Attribute Name</span></span>         | <span data-ttu-id="f41c0-453">Обязательный</span><span class="sxs-lookup"><span data-stu-id="f41c0-453">Is Required</span></span> | <span data-ttu-id="f41c0-454">Значение</span><span class="sxs-lookup"><span data-stu-id="f41c0-454">Value</span></span>                                                                                   |
|:-----------------------|:------------|:----------------------------------------------------------------------------------------|
| <span data-ttu-id="f41c0-455">**FunctionImportName**</span><span class="sxs-lookup"><span data-stu-id="f41c0-455">**FunctionImportName**</span></span> | <span data-ttu-id="f41c0-456">Да</span><span class="sxs-lookup"><span data-stu-id="f41c0-456">Yes</span></span>         | <span data-ttu-id="f41c0-457">Имя импорта функции в концептуальной модели, с которым выполняется сопоставление.</span><span class="sxs-lookup"><span data-stu-id="f41c0-457">The name of the function import in the conceptual model that is being mapped.</span></span>           |
| <span data-ttu-id="f41c0-458">**FunctionName**</span><span class="sxs-lookup"><span data-stu-id="f41c0-458">**FunctionName**</span></span>       | <span data-ttu-id="f41c0-459">Да</span><span class="sxs-lookup"><span data-stu-id="f41c0-459">Yes</span></span>         | <span data-ttu-id="f41c0-460">Имя импорта функции в модели хранения из полного пространства имен, с которым выполняется сопоставление.</span><span class="sxs-lookup"><span data-stu-id="f41c0-460">The namespace-qualified name of the function in the storage model that is being mapped.</span></span> |

### <a name="example"></a><span data-ttu-id="f41c0-461">Пример</span><span class="sxs-lookup"><span data-stu-id="f41c0-461">Example</span></span>

<span data-ttu-id="f41c0-462">Следующий пример основан на модели School.</span><span class="sxs-lookup"><span data-stu-id="f41c0-462">The following example is based on the School model.</span></span> <span data-ttu-id="f41c0-463">Рассмотрим следующую функцию в модели хранения:</span><span class="sxs-lookup"><span data-stu-id="f41c0-463">Consider the following function in the storage model:</span></span>

``` xml
 <Function Name="GetStudentGrades" Aggregate="false"
           BuiltIn="false" NiladicFunction="false"
           IsComposable="false" ParameterTypeSemantics="AllowImplicitConversion"
           Schema="dbo">
   <Parameter Name="StudentID" Type="int" Mode="In" />
 </Function>
```

<span data-ttu-id="f41c0-464">Также рассмотрим данный импорт функции в концептуальной модели:</span><span class="sxs-lookup"><span data-stu-id="f41c0-464">Also consider this function import in the conceptual model:</span></span>

``` xml
 <FunctionImport Name="GetStudentGrades" EntitySet="StudentGrades"
                 ReturnType="Collection(SchoolModel.StudentGrade)">
   <Parameter Name="StudentID" Mode="In" Type="Int32" />
 </FunctionImport>
```

<span data-ttu-id="f41c0-465">В следующем примере показан элемент **FunctionImportMapping** , используемый для отображения функции и импорта функции выше.</span><span class="sxs-lookup"><span data-stu-id="f41c0-465">The following example show a **FunctionImportMapping** element used to map the function and function import above to each other:</span></span>

``` xml
 <FunctionImportMapping FunctionImportName="GetStudentGrades"
                        FunctionName="SchoolModel.Store.GetStudentGrades" />
```
 
## <a name="insertfunction-element-msl"></a><span data-ttu-id="f41c0-466">Элемент InsertFunction (MSL)</span><span class="sxs-lookup"><span data-stu-id="f41c0-466">InsertFunction Element (MSL)</span></span>

<span data-ttu-id="f41c0-467">Элемент **InsertFunction** в языке MSL сопоставляет функцию INSERT типа сущности или ассоциацию в концептуальной модели с хранимой процедурой в базовой базе данных.</span><span class="sxs-lookup"><span data-stu-id="f41c0-467">The **InsertFunction** element in mapping specification language (MSL) maps the insert function of an entity type or association in the conceptual model to a stored procedure in the underlying database.</span></span> <span data-ttu-id="f41c0-468">Хранимые процедуры, с которыми сопоставляются функции изменения, должны объявляться в модели хранения.</span><span class="sxs-lookup"><span data-stu-id="f41c0-468">Stored procedures to which modification functions are mapped must be declared in the storage model.</span></span> <span data-ttu-id="f41c0-469">Дополнительные сведения см. в разделе элемент Function (SSDL).</span><span class="sxs-lookup"><span data-stu-id="f41c0-469">For more information, see Function Element (SSDL).</span></span>

> [!NOTE]
> <span data-ttu-id="f41c0-470">Если не выполнить сопоставление всех трех операций вставки, обновления или удаления типа сущности с хранимыми процедурами, несопоставленные операции завершатся ошибкой, если они выполняются во время выполнения и создается исключение Упдатиксцептион.</span><span class="sxs-lookup"><span data-stu-id="f41c0-470">If you do not map all three of the insert, update, or delete operations of a entity type to stored procedures, the unmapped operations will fail if executed at runtime and an UpdateException is thrown.</span></span>

<span data-ttu-id="f41c0-471">Элемент **InsertFunction** может быть дочерним элементом элемента ModificationFunctionMapping и применен к элементу EntityTypeMapping или элементу AssociationSetMapping.</span><span class="sxs-lookup"><span data-stu-id="f41c0-471">The **InsertFunction** element can be a child of the ModificationFunctionMapping element and applied to the EntityTypeMapping element or the AssociationSetMapping element.</span></span>

### <a name="insertfunction-applied-to-entitytypemapping"></a><span data-ttu-id="f41c0-472">Элемент InsertFunction, применяемый к EntityTypeMapping</span><span class="sxs-lookup"><span data-stu-id="f41c0-472">InsertFunction Applied to EntityTypeMapping</span></span>

<span data-ttu-id="f41c0-473">При применении к элементу EntityTypeMapping элемент **InsertFunction** сопоставляет функцию вставки типа сущности в концептуальной модели с хранимой процедурой.</span><span class="sxs-lookup"><span data-stu-id="f41c0-473">When applied to the EntityTypeMapping element, the **InsertFunction** element maps the insert function of an entity type in the conceptual model to a stored procedure.</span></span>

<span data-ttu-id="f41c0-474">Элемент **InsertFunction** может иметь следующие дочерние элементы при применении к элементу **EntityTypeMapping** :</span><span class="sxs-lookup"><span data-stu-id="f41c0-474">The **InsertFunction** element can have the following child elements when applied to an **EntityTypeMapping** element:</span></span>

-   <span data-ttu-id="f41c0-475">AssociationEnd (ноль и более)</span><span class="sxs-lookup"><span data-stu-id="f41c0-475">AssociationEnd (zero or more)</span></span>
-   <span data-ttu-id="f41c0-476">ComplexProperty (ноль и более)</span><span class="sxs-lookup"><span data-stu-id="f41c0-476">ComplexProperty (zero or more)</span></span>
-   <span data-ttu-id="f41c0-477">ResultBinding (ноль или один)</span><span class="sxs-lookup"><span data-stu-id="f41c0-477">ResultBinding (zero or one)</span></span>
-   <span data-ttu-id="f41c0-478">ScarlarProperty (ноль и более)</span><span class="sxs-lookup"><span data-stu-id="f41c0-478">ScarlarProperty (zero or more)</span></span>

#### <a name="applicable-attributes"></a><span data-ttu-id="f41c0-479">Применимые атрибуты</span><span class="sxs-lookup"><span data-stu-id="f41c0-479">Applicable Attributes</span></span>

<span data-ttu-id="f41c0-480">В следующей таблице описаны атрибуты, которые можно применить к элементу **InsertFunction** при применении к элементу **EntityTypeMapping** .</span><span class="sxs-lookup"><span data-stu-id="f41c0-480">The following table describes the attributes that can be applied to the **InsertFunction** element when applied to an **EntityTypeMapping** element.</span></span>

| <span data-ttu-id="f41c0-481">Имя атрибута</span><span class="sxs-lookup"><span data-stu-id="f41c0-481">Attribute Name</span></span>            | <span data-ttu-id="f41c0-482">Обязательный</span><span class="sxs-lookup"><span data-stu-id="f41c0-482">Is Required</span></span> | <span data-ttu-id="f41c0-483">Значение</span><span class="sxs-lookup"><span data-stu-id="f41c0-483">Value</span></span>                                                                                                                                                    |
|:--------------------------|:------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="f41c0-484">**FunctionName**</span><span class="sxs-lookup"><span data-stu-id="f41c0-484">**FunctionName**</span></span>          | <span data-ttu-id="f41c0-485">Да</span><span class="sxs-lookup"><span data-stu-id="f41c0-485">Yes</span></span>         | <span data-ttu-id="f41c0-486">Имя хранимой процедуры (с указанием пространства имен), с которой сопоставляется функция вставки.</span><span class="sxs-lookup"><span data-stu-id="f41c0-486">The namespace-qualified name of the stored procedure to which the insert function is mapped.</span></span> <span data-ttu-id="f41c0-487">Хранимая процедура должна объявляться в модели хранения.</span><span class="sxs-lookup"><span data-stu-id="f41c0-487">The stored procedure must be declared in the storage model.</span></span> |
| <span data-ttu-id="f41c0-488">**ровсаффектедпараметер**</span><span class="sxs-lookup"><span data-stu-id="f41c0-488">**RowsAffectedParameter**</span></span> | <span data-ttu-id="f41c0-489">Нет</span><span class="sxs-lookup"><span data-stu-id="f41c0-489">No</span></span>          | <span data-ttu-id="f41c0-490">Имя выходного параметра, возвращающего количество обработанных строк.</span><span class="sxs-lookup"><span data-stu-id="f41c0-490">The name of the output parameter that returns the number of affected rows.</span></span>                                                                               |

#### <a name="example"></a><span data-ttu-id="f41c0-491">Пример</span><span class="sxs-lookup"><span data-stu-id="f41c0-491">Example</span></span>

<span data-ttu-id="f41c0-492">Следующий пример основан на модели School и показывает элемент **InsertFunction** , используемый для преобразования функции вставки типа сущности Person в хранимую процедуру **инсертперсон** .</span><span class="sxs-lookup"><span data-stu-id="f41c0-492">The following example is based on the School model and shows the **InsertFunction** element used to map insert function of the Person entity type to the **InsertPerson** stored procedure.</span></span> <span data-ttu-id="f41c0-493">Хранимая процедура **инсертперсон** объявляется в модели хранения.</span><span class="sxs-lookup"><span data-stu-id="f41c0-493">The **InsertPerson** stored procedure is declared in the storage model.</span></span>

``` xml
 <EntityTypeMapping TypeName="SchoolModel.Person">
   <ModificationFunctionMapping>
     <InsertFunction FunctionName="SchoolModel.Store.InsertPerson">
       <ScalarProperty Name="EnrollmentDate"
                       ParameterName="EnrollmentDate" />
       <ScalarProperty Name="HireDate" ParameterName="HireDate" />
       <ScalarProperty Name="FirstName" ParameterName="FirstName" />
       <ScalarProperty Name="LastName" ParameterName="LastName" />
       <ResultBinding Name="PersonID" ColumnName="NewPersonID" />
     </InsertFunction>
     <UpdateFunction FunctionName="SchoolModel.Store.UpdatePerson">
       <ScalarProperty Name="EnrollmentDate"
                       ParameterName="EnrollmentDate"
                       Version="Current" />
       <ScalarProperty Name="HireDate" ParameterName="HireDate"
                       Version="Current" />
       <ScalarProperty Name="FirstName" ParameterName="FirstName"
                       Version="Current" />
       <ScalarProperty Name="LastName" ParameterName="LastName"
                       Version="Current" />
       <ScalarProperty Name="PersonID" ParameterName="PersonID"
                       Version="Current" />
     </UpdateFunction>
     <DeleteFunction FunctionName="SchoolModel.Store.DeletePerson">
       <ScalarProperty Name="PersonID" ParameterName="PersonID" />
     </DeleteFunction>
   </ModificationFunctionMapping>
 </EntityTypeMapping>
```
### <a name="insertfunction-applied-to-associationsetmapping"></a><span data-ttu-id="f41c0-494">Элемент InsertFunction, применяемый к AssociationSetMapping</span><span class="sxs-lookup"><span data-stu-id="f41c0-494">InsertFunction Applied to AssociationSetMapping</span></span>

<span data-ttu-id="f41c0-495">При применении к элементу AssociationSetMapping элемент **InsertFunction** сопоставляет функцию вставки ассоциации в концептуальной модели с хранимой процедурой.</span><span class="sxs-lookup"><span data-stu-id="f41c0-495">When applied to the AssociationSetMapping element, the **InsertFunction** element maps the insert function of an association in the conceptual model to a stored procedure.</span></span>

<span data-ttu-id="f41c0-496">Элемент **InsertFunction** может иметь следующие дочерние элементы при применении к элементу **AssociationSetMapping** :</span><span class="sxs-lookup"><span data-stu-id="f41c0-496">The **InsertFunction** element can have the following child elements when applied to the **AssociationSetMapping** element:</span></span>

-   <span data-ttu-id="f41c0-497">EndProperty</span><span class="sxs-lookup"><span data-stu-id="f41c0-497">EndProperty</span></span>

#### <a name="applicable-attributes"></a><span data-ttu-id="f41c0-498">Применимые атрибуты</span><span class="sxs-lookup"><span data-stu-id="f41c0-498">Applicable Attributes</span></span>

<span data-ttu-id="f41c0-499">В следующей таблице описаны атрибуты, которые можно применить к элементу **InsertFunction** при его применении к элементу **AssociationSetMapping** .</span><span class="sxs-lookup"><span data-stu-id="f41c0-499">The following table describes the attributes that can be applied to the **InsertFunction** element when it is applied to the **AssociationSetMapping** element.</span></span>

| <span data-ttu-id="f41c0-500">Имя атрибута</span><span class="sxs-lookup"><span data-stu-id="f41c0-500">Attribute Name</span></span>            | <span data-ttu-id="f41c0-501">Обязательный</span><span class="sxs-lookup"><span data-stu-id="f41c0-501">Is Required</span></span> | <span data-ttu-id="f41c0-502">Значение</span><span class="sxs-lookup"><span data-stu-id="f41c0-502">Value</span></span>                                                                                                                                                    |
|:--------------------------|:------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="f41c0-503">**FunctionName**</span><span class="sxs-lookup"><span data-stu-id="f41c0-503">**FunctionName**</span></span>          | <span data-ttu-id="f41c0-504">Да</span><span class="sxs-lookup"><span data-stu-id="f41c0-504">Yes</span></span>         | <span data-ttu-id="f41c0-505">Имя хранимой процедуры (с указанием пространства имен), с которой сопоставляется функция вставки.</span><span class="sxs-lookup"><span data-stu-id="f41c0-505">The namespace-qualified name of the stored procedure to which the insert function is mapped.</span></span> <span data-ttu-id="f41c0-506">Хранимая процедура должна объявляться в модели хранения.</span><span class="sxs-lookup"><span data-stu-id="f41c0-506">The stored procedure must be declared in the storage model.</span></span> |
| <span data-ttu-id="f41c0-507">**ровсаффектедпараметер**</span><span class="sxs-lookup"><span data-stu-id="f41c0-507">**RowsAffectedParameter**</span></span> | <span data-ttu-id="f41c0-508">Нет</span><span class="sxs-lookup"><span data-stu-id="f41c0-508">No</span></span>          | <span data-ttu-id="f41c0-509">Имя выходного параметра, возвращающего количество обработанных строк.</span><span class="sxs-lookup"><span data-stu-id="f41c0-509">The name of the output parameter that returns the number of rows affected.</span></span>                                                                               |

#### <a name="example"></a><span data-ttu-id="f41c0-510">Пример</span><span class="sxs-lookup"><span data-stu-id="f41c0-510">Example</span></span>

<span data-ttu-id="f41c0-511">Следующий пример основан на модели School и показывает элемент **InsertFunction** , используемый для сопоставления функции вставки ассоциации **каурсеинструктор** с хранимой процедурой **инсерткаурсеинструктор** .</span><span class="sxs-lookup"><span data-stu-id="f41c0-511">The following example is based on the School model and shows the **InsertFunction** element used to map insert function of the **CourseInstructor** association to the **InsertCourseInstructor** stored procedure.</span></span> <span data-ttu-id="f41c0-512">Хранимая процедура **инсерткаурсеинструктор** объявляется в модели хранения.</span><span class="sxs-lookup"><span data-stu-id="f41c0-512">The **InsertCourseInstructor** stored procedure is declared in the storage model.</span></span>

``` xml
 <AssociationSetMapping Name="CourseInstructor"
                        TypeName="SchoolModel.CourseInstructor"
                        StoreEntitySet="CourseInstructor">
   <EndProperty Name="Person">
     <ScalarProperty Name="PersonID" ColumnName="PersonID" />
   </EndProperty>
   <EndProperty Name="Course">
     <ScalarProperty Name="CourseID" ColumnName="CourseID" />
   </EndProperty>
   <ModificationFunctionMapping>
     <InsertFunction FunctionName="SchoolModel.Store.InsertCourseInstructor" >   
       <EndProperty Name="Course">
         <ScalarProperty Name="CourseID" ParameterName="courseId"/>
       </EndProperty>
       <EndProperty Name="Person">
         <ScalarProperty Name="PersonID" ParameterName="instructorId"/>
       </EndProperty>
     </InsertFunction>
     <DeleteFunction FunctionName="SchoolModel.Store.DeleteCourseInstructor">
       <EndProperty Name="Course">
         <ScalarProperty Name="CourseID" ParameterName="courseId"/>
       </EndProperty>
       <EndProperty Name="Person">
         <ScalarProperty Name="PersonID" ParameterName="instructorId"/>
       </EndProperty>
     </DeleteFunction>
   </ModificationFunctionMapping>
 </AssociationSetMapping>
```

## <a name="mapping-element-msl"></a><span data-ttu-id="f41c0-513">Элемент сопоставления (MSL)</span><span class="sxs-lookup"><span data-stu-id="f41c0-513">Mapping Element (MSL)</span></span>

<span data-ttu-id="f41c0-514">Элемент **Mapping** на языке MSL содержит сведения для сопоставления объектов, определенных в концептуальной модели, с базой данных (как описано в модели хранения).</span><span class="sxs-lookup"><span data-stu-id="f41c0-514">The **Mapping** element in mapping specification language (MSL) contains information for mapping objects that are defined in a conceptual model to a database (as described in a storage model).</span></span> <span data-ttu-id="f41c0-515">Дополнительные сведения см. в статье спецификации языка CSDL и спецификации SSDL.</span><span class="sxs-lookup"><span data-stu-id="f41c0-515">For more information, see CSDL Specification and SSDL Specification.</span></span>

<span data-ttu-id="f41c0-516">Элемент **Mapping** является корневым элементом для спецификации сопоставления.</span><span class="sxs-lookup"><span data-stu-id="f41c0-516">The **Mapping** element is the root element for a mapping specification.</span></span> <span data-ttu-id="f41c0-517">Пространство имен XML для спецификаций сопоставления — https://schemas.microsoft.com/ado/2009/11/mapping/cs .</span><span class="sxs-lookup"><span data-stu-id="f41c0-517">The XML namespace for mapping specifications is https://schemas.microsoft.com/ado/2009/11/mapping/cs.</span></span>

<span data-ttu-id="f41c0-518">У элемента Mapping могут быть следующие дочерние элементы (в порядке перечисления):</span><span class="sxs-lookup"><span data-stu-id="f41c0-518">The mapping element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="f41c0-519">Alias (ноль и более элементов)</span><span class="sxs-lookup"><span data-stu-id="f41c0-519">Alias (zero or more)</span></span>
-   <span data-ttu-id="f41c0-520">EntityContainerMapping (ровно один)</span><span class="sxs-lookup"><span data-stu-id="f41c0-520">EntityContainerMapping (exactly one)</span></span>

<span data-ttu-id="f41c0-521">Имена типов концептуальной модели и модели хранения, которые упоминаются в языке MSL, должны указываться вместе с именами соответствующих пространств имен.</span><span class="sxs-lookup"><span data-stu-id="f41c0-521">Names of conceptual and storage model types that are referenced in MSL must be qualified by their respective namespace names.</span></span> <span data-ttu-id="f41c0-522">Дополнительные сведения об имени пространства имен концептуальной модели см. в разделе элемент Schema (CSDL).</span><span class="sxs-lookup"><span data-stu-id="f41c0-522">For information about the conceptual model namespace name, see Schema Element (CSDL).</span></span> <span data-ttu-id="f41c0-523">Дополнительные сведения об имени пространства имен модели хранения см. в разделе элемент Schema (SSDL).</span><span class="sxs-lookup"><span data-stu-id="f41c0-523">For information about the storage model namespace name, see Schema Element (SSDL).</span></span> <span data-ttu-id="f41c0-524">Псевдонимы для пространств имен, используемых в языке MSL, могут быть заданы с использованием элемента Alias.</span><span class="sxs-lookup"><span data-stu-id="f41c0-524">Aliases for namespaces that are used in MSL can be defined with the Alias element.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="f41c0-525">Применимые атрибуты</span><span class="sxs-lookup"><span data-stu-id="f41c0-525">Applicable Attributes</span></span>

<span data-ttu-id="f41c0-526">В следующей таблице описаны атрибуты, которые можно применить к элементу **Mapping** .</span><span class="sxs-lookup"><span data-stu-id="f41c0-526">The table below describes the attributes that can be applied to the **Mapping** element.</span></span>

| <span data-ttu-id="f41c0-527">Имя атрибута</span><span class="sxs-lookup"><span data-stu-id="f41c0-527">Attribute Name</span></span> | <span data-ttu-id="f41c0-528">Обязательный</span><span class="sxs-lookup"><span data-stu-id="f41c0-528">Is Required</span></span> | <span data-ttu-id="f41c0-529">Значение</span><span class="sxs-lookup"><span data-stu-id="f41c0-529">Value</span></span>                                                 |
|:---------------|:------------|:------------------------------------------------------|
| <span data-ttu-id="f41c0-530">**Модуль**</span><span class="sxs-lookup"><span data-stu-id="f41c0-530">**Space**</span></span>      | <span data-ttu-id="f41c0-531">Да</span><span class="sxs-lookup"><span data-stu-id="f41c0-531">Yes</span></span>         | <span data-ttu-id="f41c0-532">**C-S**.</span><span class="sxs-lookup"><span data-stu-id="f41c0-532">**C-S**.</span></span> <span data-ttu-id="f41c0-533">Это фиксированное значение, и его невозможно изменить.</span><span class="sxs-lookup"><span data-stu-id="f41c0-533">This is a fixed value and cannot be changed.</span></span> |

### <a name="example"></a><span data-ttu-id="f41c0-534">Пример</span><span class="sxs-lookup"><span data-stu-id="f41c0-534">Example</span></span>

<span data-ttu-id="f41c0-535">В следующем примере показан элемент **Mapping** , основанный на части модели School.</span><span class="sxs-lookup"><span data-stu-id="f41c0-535">The following example shows a **Mapping** element that is based on part of the School model.</span></span> <span data-ttu-id="f41c0-536">Дополнительные сведения о модели School см. в разделе Краткое руководство (Entity Framework):</span><span class="sxs-lookup"><span data-stu-id="f41c0-536">For more information about the School model, see Quickstart (Entity Framework):</span></span>

``` xml
 <Mapping Space="C-S"
          xmlns="https://schemas.microsoft.com/ado/2009/11/mapping/cs">
   <Alias Key="c" Value="SchoolModel"/>
   <EntityContainerMapping StorageEntityContainer="SchoolModelStoreContainer"
                           CdmEntityContainer="SchoolModelEntities">
     <EntitySetMapping Name="Courses">
       <EntityTypeMapping TypeName="c.Course">
         <MappingFragment StoreEntitySet="Course">
           <ScalarProperty Name="CourseID" ColumnName="CourseID" />
           <ScalarProperty Name="Title" ColumnName="Title" />
           <ScalarProperty Name="Credits" ColumnName="Credits" />
           <ScalarProperty Name="DepartmentID" ColumnName="DepartmentID" />
         </MappingFragment>
       </EntityTypeMapping>
     </EntitySetMapping>
     <EntitySetMapping Name="Departments">
       <EntityTypeMapping TypeName="c.Department">
         <MappingFragment StoreEntitySet="Department">
           <ScalarProperty Name="DepartmentID" ColumnName="DepartmentID" />
           <ScalarProperty Name="Name" ColumnName="Name" />
           <ScalarProperty Name="Budget" ColumnName="Budget" />
           <ScalarProperty Name="StartDate" ColumnName="StartDate" />
           <ScalarProperty Name="Administrator" ColumnName="Administrator" />
         </MappingFragment>
       </EntityTypeMapping>
     </EntitySetMapping>
   </EntityContainerMapping>
 </Mapping>
```

## <a name="mappingfragment-element-msl"></a><span data-ttu-id="f41c0-537">Элемент MappingFragment (MSL)</span><span class="sxs-lookup"><span data-stu-id="f41c0-537">MappingFragment Element (MSL)</span></span>

<span data-ttu-id="f41c0-538">Элемент **MappingFragment** на языке MSL определяет сопоставление свойств типа сущности концептуальной модели и таблицы или представления в базе данных.</span><span class="sxs-lookup"><span data-stu-id="f41c0-538">The **MappingFragment** element in mapping specification language (MSL) defines the mapping between the properties of a conceptual model entity type and a table or view in the database.</span></span> <span data-ttu-id="f41c0-539">Сведения о типах сущностей концептуальной модели и базовых таблицах или представлениях базы данных см. в разделах Элемент EntityType (язык CSDL) и Элемент EntitySet (язык SSDL).</span><span class="sxs-lookup"><span data-stu-id="f41c0-539">For information about conceptual model entity types and underlying database tables or views, see EntityType Element (CSDL) and EntitySet Element (SSDL).</span></span> <span data-ttu-id="f41c0-540">**MappingFragment** может быть дочерним элементом элемента EntityTypeMapping или элемента EntitySetMapping.</span><span class="sxs-lookup"><span data-stu-id="f41c0-540">The **MappingFragment** can be a child element of the EntityTypeMapping element or the EntitySetMapping element.</span></span>

<span data-ttu-id="f41c0-541">Элемент **MappingFragment** может иметь следующие дочерние элементы:</span><span class="sxs-lookup"><span data-stu-id="f41c0-541">The **MappingFragment** element can have the following child elements:</span></span>

-   <span data-ttu-id="f41c0-542">ComplexType (ноль или более)</span><span class="sxs-lookup"><span data-stu-id="f41c0-542">ComplexType (zero or more)</span></span>
-   <span data-ttu-id="f41c0-543">ScalarProperty (ноль и более)</span><span class="sxs-lookup"><span data-stu-id="f41c0-543">ScalarProperty (zero or more)</span></span>
-   <span data-ttu-id="f41c0-544">Condition (ноль и более)</span><span class="sxs-lookup"><span data-stu-id="f41c0-544">Condition (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="f41c0-545">Применимые атрибуты</span><span class="sxs-lookup"><span data-stu-id="f41c0-545">Applicable Attributes</span></span>

<span data-ttu-id="f41c0-546">В следующей таблице описаны атрибуты, которые можно применить к элементу **MappingFragment** .</span><span class="sxs-lookup"><span data-stu-id="f41c0-546">The following table describes the attributes that can be applied to the **MappingFragment** element.</span></span>

| <span data-ttu-id="f41c0-547">Имя атрибута</span><span class="sxs-lookup"><span data-stu-id="f41c0-547">Attribute Name</span></span>          | <span data-ttu-id="f41c0-548">Обязательный</span><span class="sxs-lookup"><span data-stu-id="f41c0-548">Is Required</span></span> | <span data-ttu-id="f41c0-549">Значение</span><span class="sxs-lookup"><span data-stu-id="f41c0-549">Value</span></span>                                                                                                                                                                                                                         |
|:------------------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="f41c0-550">**сторинтитисет**</span><span class="sxs-lookup"><span data-stu-id="f41c0-550">**StoreEntitySet**</span></span>      | <span data-ttu-id="f41c0-551">Да</span><span class="sxs-lookup"><span data-stu-id="f41c0-551">Yes</span></span>         | <span data-ttu-id="f41c0-552">Имя столбца таблицы или представления, с которым производится сопоставление.</span><span class="sxs-lookup"><span data-stu-id="f41c0-552">The name of the table or view that is being mapped.</span></span>                                                                                                                                                                           |
| <span data-ttu-id="f41c0-553">**Атрибутом makecolumnsdistinct**</span><span class="sxs-lookup"><span data-stu-id="f41c0-553">**MakeColumnsDistinct**</span></span> | <span data-ttu-id="f41c0-554">Нет</span><span class="sxs-lookup"><span data-stu-id="f41c0-554">No</span></span>          | <span data-ttu-id="f41c0-555">**Значение true** или **false** в зависимости от того, возвращаются ли только отдельные строки.</span><span class="sxs-lookup"><span data-stu-id="f41c0-555">**True** or **False** depending on whether only distinct rows are returned.</span></span> <br/> <span data-ttu-id="f41c0-556">Если для этого атрибута задано значение **true**, атрибуту **женератеупдатевиевс** элемента EntityContainerMapping должно быть присвоено значение **false**.</span><span class="sxs-lookup"><span data-stu-id="f41c0-556">If this attribute is set to **True**, the **GenerateUpdateViews** attribute of the EntityContainerMapping element must be set to **False**.</span></span> |

### <a name="example"></a><span data-ttu-id="f41c0-557">Пример</span><span class="sxs-lookup"><span data-stu-id="f41c0-557">Example</span></span>

<span data-ttu-id="f41c0-558">В следующем примере показан элемент **MappingFragment** в качестве дочернего элемента элемента **EntityTypeMapping** .</span><span class="sxs-lookup"><span data-stu-id="f41c0-558">The following example shows a **MappingFragment** element as the child of an **EntityTypeMapping** element.</span></span> <span data-ttu-id="f41c0-559">В этом примере свойства типа **курса** в концептуальной модели сопоставляются со столбцами таблицы **Course** в базе данных.</span><span class="sxs-lookup"><span data-stu-id="f41c0-559">In this example, properties of the **Course** type in the conceptual model are mapped to columns of the **Course** table in the database.</span></span>

``` xml
 <EntitySetMapping Name="Courses">
   <EntityTypeMapping TypeName="SchoolModel.Course">
     <MappingFragment StoreEntitySet="Course">
       <ScalarProperty Name="CourseID" ColumnName="CourseID" />
       <ScalarProperty Name="Title" ColumnName="Title" />
       <ScalarProperty Name="Credits" ColumnName="Credits" />
       <ScalarProperty Name="DepartmentID" ColumnName="DepartmentID" />
     </MappingFragment>
   </EntityTypeMapping>
 </EntitySetMapping>
```

### <a name="example"></a><span data-ttu-id="f41c0-560">Пример</span><span class="sxs-lookup"><span data-stu-id="f41c0-560">Example</span></span>

<span data-ttu-id="f41c0-561">В следующем примере показан элемент **MappingFragment** в качестве дочернего элемента элемента **EntitySetMapping** .</span><span class="sxs-lookup"><span data-stu-id="f41c0-561">The following example shows a **MappingFragment** element as the child of an **EntitySetMapping** element.</span></span> <span data-ttu-id="f41c0-562">Как и в приведенном выше примере, свойства типа **курса** в концептуальной модели сопоставляются со столбцами таблицы **Course** в базе данных.</span><span class="sxs-lookup"><span data-stu-id="f41c0-562">As in the example above, properties of the **Course** type in the conceptual model are mapped to columns of the **Course** table in the database.</span></span>

``` xml
 <EntitySetMapping Name="Courses" TypeName="SchoolModel.Course">
     <MappingFragment StoreEntitySet="Course">
       <ScalarProperty Name="CourseID" ColumnName="CourseID" />
       <ScalarProperty Name="Title" ColumnName="Title" />
       <ScalarProperty Name="Credits" ColumnName="Credits" />
       <ScalarProperty Name="DepartmentID" ColumnName="DepartmentID" />
     </MappingFragment>
 </EntitySetMapping>
```

## <a name="modificationfunctionmapping-element-msl"></a><span data-ttu-id="f41c0-563">Элемент ModificationFunctionMapping (MSL)</span><span class="sxs-lookup"><span data-stu-id="f41c0-563">ModificationFunctionMapping Element (MSL)</span></span>

<span data-ttu-id="f41c0-564">Элемент **ModificationFunctionMapping** на языке MSL сопоставляет функции вставки, обновления и удаления типа сущности концептуальной модели с хранимыми процедурами в базовой базе данных.</span><span class="sxs-lookup"><span data-stu-id="f41c0-564">The **ModificationFunctionMapping** element in mapping specification language (MSL) maps the insert, update, and delete functions of a conceptual model entity type to stored procedures in the underlying database.</span></span> <span data-ttu-id="f41c0-565">Элемент **ModificationFunctionMapping** также может сопоставлять функции INSERT и DELETE для ассоциаций "многие ко многим" в концептуальной модели с хранимыми процедурами в базовой базе данных.</span><span class="sxs-lookup"><span data-stu-id="f41c0-565">The **ModificationFunctionMapping** element can also map the insert and delete functions for many-to-many associations in the conceptual model to stored procedures in the underlying database.</span></span> <span data-ttu-id="f41c0-566">Хранимые процедуры, с которыми сопоставляются функции изменения, должны объявляться в модели хранения.</span><span class="sxs-lookup"><span data-stu-id="f41c0-566">Stored procedures to which modification functions are mapped must be declared in the storage model.</span></span> <span data-ttu-id="f41c0-567">Дополнительные сведения см. в разделе элемент Function (SSDL).</span><span class="sxs-lookup"><span data-stu-id="f41c0-567">For more information, see Function Element (SSDL).</span></span>

> [!NOTE]
> <span data-ttu-id="f41c0-568">Если не выполнить сопоставление всех трех операций вставки, обновления или удаления типа сущности с хранимыми процедурами, несопоставленные операции завершатся ошибкой, если они выполняются во время выполнения и создается исключение Упдатиксцептион.</span><span class="sxs-lookup"><span data-stu-id="f41c0-568">If you do not map all three of the insert, update, or delete operations of a entity type to stored procedures, the unmapped operations will fail if executed at runtime and an UpdateException is thrown.</span></span>


> [!NOTE]
> <span data-ttu-id="f41c0-569">Если функции преобразования для одной сущности в иерархии наследования сопоставляются с хранимыми процедурами, то функции преобразования всех типов в иерархии должны быть сопоставлены с хранимыми процедурами.</span><span class="sxs-lookup"><span data-stu-id="f41c0-569">If the modification functions for one entity in an inheritance hierarchy are mapped to stored procedures, then modification functions for all types in the hierarchy must be mapped to stored procedures.</span></span>

<span data-ttu-id="f41c0-570">Элемент **ModificationFunctionMapping** может быть дочерним по отношению к элементу EntityTypeMapping или элементу AssociationSetMapping.</span><span class="sxs-lookup"><span data-stu-id="f41c0-570">The **ModificationFunctionMapping** element can be a child of the EntityTypeMapping element or the AssociationSetMapping element.</span></span>

<span data-ttu-id="f41c0-571">Элемент **ModificationFunctionMapping** может иметь следующие дочерние элементы:</span><span class="sxs-lookup"><span data-stu-id="f41c0-571">The **ModificationFunctionMapping** element can have the following child elements:</span></span>

-   <span data-ttu-id="f41c0-572">DeleteFunction (ноль или один)</span><span class="sxs-lookup"><span data-stu-id="f41c0-572">DeleteFunction (zero or one)</span></span>
-   <span data-ttu-id="f41c0-573">InsertFunction (ноль или один)</span><span class="sxs-lookup"><span data-stu-id="f41c0-573">InsertFunction (zero or one)</span></span>
-   <span data-ttu-id="f41c0-574">UpdateFunction (ноль или один)</span><span class="sxs-lookup"><span data-stu-id="f41c0-574">UpdateFunction (zero or one)</span></span>

<span data-ttu-id="f41c0-575">К элементу **ModificationFunctionMapping** не применяются атрибуты.</span><span class="sxs-lookup"><span data-stu-id="f41c0-575">No attributes are applicable to the **ModificationFunctionMapping** element.</span></span>

### <a name="example"></a><span data-ttu-id="f41c0-576">Пример</span><span class="sxs-lookup"><span data-stu-id="f41c0-576">Example</span></span>

<span data-ttu-id="f41c0-577">В следующем примере показано сопоставление набора сущностей для набора сущностей « **люди** » в модели School.</span><span class="sxs-lookup"><span data-stu-id="f41c0-577">The following example shows the entity set mapping for the **People** entity set in the School model.</span></span> <span data-ttu-id="f41c0-578">В дополнение к сопоставлению столбцов для типа сущности **Person** отображаются сопоставления функций вставки, обновления и удаления типа **Person** .</span><span class="sxs-lookup"><span data-stu-id="f41c0-578">In addition to the column mapping for the **Person** entity type, the mapping of the insert, update, and delete functions of the **Person** type are shown.</span></span> <span data-ttu-id="f41c0-579">Функции, с которыми выполняется сопоставление, объявляются в модели хранения.</span><span class="sxs-lookup"><span data-stu-id="f41c0-579">The functions that are mapped to are declared in the storage model.</span></span>

``` xml
 <EntitySetMapping Name="People">
   <EntityTypeMapping TypeName="SchoolModel.Person">
     <MappingFragment StoreEntitySet="Person">
       <ScalarProperty Name="PersonID" ColumnName="PersonID" />
       <ScalarProperty Name="LastName" ColumnName="LastName" />
       <ScalarProperty Name="FirstName" ColumnName="FirstName" />
       <ScalarProperty Name="HireDate" ColumnName="HireDate" />
       <ScalarProperty Name="EnrollmentDate"
                       ColumnName="EnrollmentDate" />
     </MappingFragment>
 </EntityTypeMapping>
   <EntityTypeMapping TypeName="SchoolModel.Person">
     <ModificationFunctionMapping>
       <InsertFunction FunctionName="SchoolModel.Store.InsertPerson">
         <ScalarProperty Name="EnrollmentDate"
                         ParameterName="EnrollmentDate" />
         <ScalarProperty Name="HireDate" ParameterName="HireDate" />
         <ScalarProperty Name="FirstName" ParameterName="FirstName" />
         <ScalarProperty Name="LastName" ParameterName="LastName" />
         <ResultBinding Name="PersonID" ColumnName="NewPersonID" />
       </InsertFunction>
       <UpdateFunction FunctionName="SchoolModel.Store.UpdatePerson">
         <ScalarProperty Name="EnrollmentDate"
                         ParameterName="EnrollmentDate"
                         Version="Current" />
         <ScalarProperty Name="HireDate" ParameterName="HireDate"
                         Version="Current" />
         <ScalarProperty Name="FirstName" ParameterName="FirstName"
                         Version="Current" />
         <ScalarProperty Name="LastName" ParameterName="LastName"
                         Version="Current" />
         <ScalarProperty Name="PersonID" ParameterName="PersonID"
                         Version="Current" />
       </UpdateFunction>
       <DeleteFunction FunctionName="SchoolModel.Store.DeletePerson">
         <ScalarProperty Name="PersonID" ParameterName="PersonID" />
       </DeleteFunction>
     </ModificationFunctionMapping>
   </EntityTypeMapping>
 </EntitySetMapping>
```

### <a name="example"></a><span data-ttu-id="f41c0-580">Пример</span><span class="sxs-lookup"><span data-stu-id="f41c0-580">Example</span></span>

<span data-ttu-id="f41c0-581">В следующем примере показано сопоставление набора ассоциаций для набора ассоциаций **каурсеинструктор** в модели School.</span><span class="sxs-lookup"><span data-stu-id="f41c0-581">The following example shows the association set mapping for the **CourseInstructor** association set in the School model.</span></span> <span data-ttu-id="f41c0-582">Помимо сопоставления столбцов для ассоциации **каурсеинструктор** , отображаются сопоставления функций вставки и удаления ассоциации **каурсеинструктор** .</span><span class="sxs-lookup"><span data-stu-id="f41c0-582">In addition to the column mapping for the **CourseInstructor** association, the mapping of the insert and delete functions of the **CourseInstructor** association are shown.</span></span> <span data-ttu-id="f41c0-583">Функции, с которыми выполняется сопоставление, объявляются в модели хранения.</span><span class="sxs-lookup"><span data-stu-id="f41c0-583">The functions that are mapped to are declared in the storage model.</span></span>

``` xml
 <AssociationSetMapping Name="CourseInstructor"
                        TypeName="SchoolModel.CourseInstructor"
                        StoreEntitySet="CourseInstructor">
   <EndProperty Name="Person">
     <ScalarProperty Name="PersonID" ColumnName="PersonID" />
   </EndProperty>
   <EndProperty Name="Course">
     <ScalarProperty Name="CourseID" ColumnName="CourseID" />
   </EndProperty>
   <ModificationFunctionMapping>
     <InsertFunction FunctionName="SchoolModel.Store.InsertCourseInstructor" >   
       <EndProperty Name="Course">
         <ScalarProperty Name="CourseID" ParameterName="courseId"/>
       </EndProperty>
       <EndProperty Name="Person">
         <ScalarProperty Name="PersonID" ParameterName="instructorId"/>
       </EndProperty>
     </InsertFunction>
     <DeleteFunction FunctionName="SchoolModel.Store.DeleteCourseInstructor">
       <EndProperty Name="Course">
         <ScalarProperty Name="CourseID" ParameterName="courseId"/>
       </EndProperty>
       <EndProperty Name="Person">
         <ScalarProperty Name="PersonID" ParameterName="instructorId"/>
       </EndProperty>
     </DeleteFunction>
   </ModificationFunctionMapping>
 </AssociationSetMapping>
```
 

 

## <a name="queryview-element-msl"></a><span data-ttu-id="f41c0-584">Элемент QueryView (MSL)</span><span class="sxs-lookup"><span data-stu-id="f41c0-584">QueryView Element (MSL)</span></span>

<span data-ttu-id="f41c0-585">Элемент **QueryView** на языке MSL определяет сопоставление только для чтения между типом сущности или ассоциацией в концептуальной модели и таблицей в базовой базе данных.</span><span class="sxs-lookup"><span data-stu-id="f41c0-585">The **QueryView** element in mapping specification language (MSL) defines a read-only mapping between an entity type or association in the conceptual model and a table in the underlying database.</span></span> <span data-ttu-id="f41c0-586">Сопоставление определяется с помощью Entity SQL запроса, который вычисляется для модели хранения, а результирующий набор выражается в терминах сущности или ассоциации в концептуальной модели.</span><span class="sxs-lookup"><span data-stu-id="f41c0-586">The mapping is defined with an Entity SQL query that is evaluated against the storage model, and you express the result set in terms of an entity or association in the conceptual model.</span></span> <span data-ttu-id="f41c0-587">Представления запросов доступны только для чтения, поэтому для обновления типов, которые определены представлениями запросов, не могут применяться стандартные команды обновления.</span><span class="sxs-lookup"><span data-stu-id="f41c0-587">Because query views are read-only, you cannot use standard update commands to update types that are defined by query views.</span></span> <span data-ttu-id="f41c0-588">Представления запросов доступны только для чтения, поэтому для обновления типов, которые определены представлениями запросов, не могут применяться стандартные команды обновления.</span><span class="sxs-lookup"><span data-stu-id="f41c0-588">You can make updates to these types by using modification functions.</span></span> <span data-ttu-id="f41c0-589">Дополнительные сведения см. в разделе инструкции. Сопоставьте функции изменения с хранимыми процедурами.</span><span class="sxs-lookup"><span data-stu-id="f41c0-589">For more information, see How to: Map Modification Functions to Stored Procedures.</span></span>

> [!NOTE]
> <span data-ttu-id="f41c0-590">В элементе **QueryView** Entity SQL выражения, содержащие **GroupBy**, групповые агрегаты или свойства навигации, не поддерживаются.</span><span class="sxs-lookup"><span data-stu-id="f41c0-590">In the **QueryView** element, Entity SQL expressions that contain **GroupBy**, group aggregates, or navigation properties are not supported.</span></span>

 

<span data-ttu-id="f41c0-591">Элемент **QueryView** может быть дочерним по отношению к элементу EntitySetMapping или элементу AssociationSetMapping.</span><span class="sxs-lookup"><span data-stu-id="f41c0-591">The **QueryView** element can be a child of the EntitySetMapping element or the AssociationSetMapping element.</span></span> <span data-ttu-id="f41c0-592">В первом случае представление запроса определяет сопоставление только для чтения для сущности в концептуальной модели.</span><span class="sxs-lookup"><span data-stu-id="f41c0-592">In the former case, the query view defines a read-only mapping for an entity in the conceptual model.</span></span> <span data-ttu-id="f41c0-593">Во втором случае представление запроса определяет сопоставление только для чтения для связи в концептуальной модели.</span><span class="sxs-lookup"><span data-stu-id="f41c0-593">In the latter case, the query view defines a read-only mapping for an association in the conceptual model.</span></span>

> [!NOTE]
> <span data-ttu-id="f41c0-594">Если элемент **AssociationSetMapping** предназначен для связи с ссылочным ограничением, элемент **AssociationSetMapping** игнорируется.</span><span class="sxs-lookup"><span data-stu-id="f41c0-594">If the **AssociationSetMapping** element is for an association with a referential constraint, the **AssociationSetMapping** element is ignored.</span></span> <span data-ttu-id="f41c0-595">Дополнительные сведения см. в разделе элемент ReferentialConstraint (CSDL).</span><span class="sxs-lookup"><span data-stu-id="f41c0-595">For more information, see ReferentialConstraint Element (CSDL).</span></span>

<span data-ttu-id="f41c0-596">Элемент **QueryView** не может иметь дочерних элементов.</span><span class="sxs-lookup"><span data-stu-id="f41c0-596">The **QueryView** element cannot have any child elements.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="f41c0-597">Применимые атрибуты</span><span class="sxs-lookup"><span data-stu-id="f41c0-597">Applicable Attributes</span></span>

<span data-ttu-id="f41c0-598">В следующей таблице описаны атрибуты, которые можно применить к элементу **QueryView** .</span><span class="sxs-lookup"><span data-stu-id="f41c0-598">The following table describes the attributes that can be applied to the **QueryView** element.</span></span>

| <span data-ttu-id="f41c0-599">Имя атрибута</span><span class="sxs-lookup"><span data-stu-id="f41c0-599">Attribute Name</span></span> | <span data-ttu-id="f41c0-600">Обязательный</span><span class="sxs-lookup"><span data-stu-id="f41c0-600">Is Required</span></span> | <span data-ttu-id="f41c0-601">Значение</span><span class="sxs-lookup"><span data-stu-id="f41c0-601">Value</span></span>                                                                         |
|:---------------|:------------|:------------------------------------------------------------------------------|
| <span data-ttu-id="f41c0-602">**Имени**</span><span class="sxs-lookup"><span data-stu-id="f41c0-602">**TypeName**</span></span>   | <span data-ttu-id="f41c0-603">Нет</span><span class="sxs-lookup"><span data-stu-id="f41c0-603">No</span></span>          | <span data-ttu-id="f41c0-604">Имя типа концептуальной модели, сопоставляемой представлением запроса.</span><span class="sxs-lookup"><span data-stu-id="f41c0-604">The name of the conceptual model type that is being mapped by the query view.</span></span> |

### <a name="example"></a><span data-ttu-id="f41c0-605">Пример</span><span class="sxs-lookup"><span data-stu-id="f41c0-605">Example</span></span>

<span data-ttu-id="f41c0-606">В следующем примере показано, как элемент **QueryView** является дочерним элементом элемента **EntitySetMapping** и определяет сопоставление представления запроса для типа сущности « **Отдел** » в модели School.</span><span class="sxs-lookup"><span data-stu-id="f41c0-606">The following example shows the **QueryView** element as a child of the **EntitySetMapping** element and defines a query view mapping for the **Department** entity type in the School Model.</span></span>

``` xml
 <EntitySetMapping Name="Departments" >
   <QueryView>
     SELECT VALUE SchoolModel.Department(d.DepartmentID,
                                         d.Name,
                                         d.Budget,
                                         d.StartDate)
     FROM SchoolModelStoreContainer.Department AS d
     WHERE d.Budget > 150000
   </QueryView>
 </EntitySetMapping>
```

<span data-ttu-id="f41c0-607">Поскольку запрос возвращает только подмножество элементов типа **отдела** в модели хранения, тип **отдела** в модели School был изменен на основе этого сопоставления следующим образом:</span><span class="sxs-lookup"><span data-stu-id="f41c0-607">Because the query only returns a subset of the members of the **Department** type in the storage model, the **Department** type in the School model has been modified based on this mapping as follows:</span></span>

``` xml
 <EntityType Name="Department">
   <Key>
     <PropertyRef Name="DepartmentID" />
   </Key>
   <Property Type="Int32" Name="DepartmentID" Nullable="false" />
   <Property Type="String" Name="Name" Nullable="false"
             MaxLength="50" FixedLength="false" Unicode="true" />
   <Property Type="Decimal" Name="Budget" Nullable="false"
             Precision="19" Scale="4" />
   <Property Type="DateTime" Name="StartDate" Nullable="false" />
   <NavigationProperty Name="Courses"
                       Relationship="SchoolModel.FK_Course_Department"
                       FromRole="Department" ToRole="Course" />
 </EntityType>
```

### <a name="example"></a><span data-ttu-id="f41c0-608">Пример</span><span class="sxs-lookup"><span data-stu-id="f41c0-608">Example</span></span>

<span data-ttu-id="f41c0-609">В следующем примере показан элемент **QueryView** как дочерний элемент элемента **AssociationSetMapping** и определяется сопоставление только для чтения для `FK_Course_Department` ассоциации в модели School.</span><span class="sxs-lookup"><span data-stu-id="f41c0-609">The next example shows the **QueryView** element as the child of an **AssociationSetMapping** element and defines a read-only mapping for the `FK_Course_Department` association in the School model.</span></span>

``` xml
 <EntityContainerMapping StorageEntityContainer="SchoolModelStoreContainer"
                         CdmEntityContainer="SchoolEntities">
   <EntitySetMapping Name="Courses" >
     <QueryView>
       SELECT VALUE SchoolModel.Course(c.CourseID,
                                       c.Title,
                                       c.Credits)
       FROM SchoolModelStoreContainer.Course AS c
     </QueryView>
   </EntitySetMapping>
   <EntitySetMapping Name="Departments" >
     <QueryView>
       SELECT VALUE SchoolModel.Department(d.DepartmentID,
                                           d.Name,
                                           d.Budget,
                                           d.StartDate)
       FROM SchoolModelStoreContainer.Department AS d
       WHERE d.Budget > 150000
     </QueryView>
   </EntitySetMapping>
   <AssociationSetMapping Name="FK_Course_Department" >
     <QueryView>
       SELECT VALUE SchoolModel.FK_Course_Department(
         CREATEREF(SchoolEntities.Departments, row(c.DepartmentID), SchoolModel.Department),
         CREATEREF(SchoolEntities.Courses, row(c.CourseID)) )
       FROM SchoolModelStoreContainer.Course AS c
     </QueryView>
   </AssociationSetMapping>
 </EntityContainerMapping>
```
 
### <a name="comments"></a><span data-ttu-id="f41c0-610">Комментарии</span><span class="sxs-lookup"><span data-stu-id="f41c0-610">Comments</span></span>

<span data-ttu-id="f41c0-611">Представления запросов можно определять для реализации следующих сценариев.</span><span class="sxs-lookup"><span data-stu-id="f41c0-611">You can define query views to enable the following scenarios:</span></span>

-   <span data-ttu-id="f41c0-612">Определение сущности в концептуальной модели, не включающей все свойства сущности в модели хранения.</span><span class="sxs-lookup"><span data-stu-id="f41c0-612">Define an entity in the conceptual model that doesn't include all the properties of the entity in the storage model.</span></span> <span data-ttu-id="f41c0-613">Сюда входят свойства, не имеющие значений по умолчанию и не поддерживающие значения **null** .</span><span class="sxs-lookup"><span data-stu-id="f41c0-613">This includes properties that do not have default values and do not support **null** values.</span></span>
-   <span data-ttu-id="f41c0-614">Сопоставление вычисляемых столбцов в модели хранения со свойствами типов сущностей в концептуальной модели.</span><span class="sxs-lookup"><span data-stu-id="f41c0-614">Map computed columns in the storage model to properties of entity types in the conceptual model.</span></span>
-   <span data-ttu-id="f41c0-615">Определение сопоставления, в котором условия, используемые для секционирования сущностей в концептуальной модели, не базируются на равенстве.</span><span class="sxs-lookup"><span data-stu-id="f41c0-615">Define a mapping where conditions used to partition entities in the conceptual model are not based on equality.</span></span> <span data-ttu-id="f41c0-616">При указании условного сопоставления с помощью элемента **Condition** указанное условие должно быть равно указанному значению.</span><span class="sxs-lookup"><span data-stu-id="f41c0-616">When you specify a conditional mapping using the **Condition** element, the supplied condition must equal the specified value.</span></span> <span data-ttu-id="f41c0-617">Дополнительные сведения см. в разделе элемент Condition (MSL).</span><span class="sxs-lookup"><span data-stu-id="f41c0-617">For more information, see Condition Element (MSL).</span></span>
-   <span data-ttu-id="f41c0-618">Сопоставление одного столбца в модели хранения с несколькими типами в концептуальной модели.</span><span class="sxs-lookup"><span data-stu-id="f41c0-618">Map the same column in the storage model to multiple types in the conceptual model.</span></span>
-   <span data-ttu-id="f41c0-619">Сопоставление нескольких типов с одной и той же таблицей.</span><span class="sxs-lookup"><span data-stu-id="f41c0-619">Map multiple types to the same table.</span></span>
-   <span data-ttu-id="f41c0-620">Определение ассоциаций в концептуальной модели, базирующихся на внешних ключах реляционной схемы.</span><span class="sxs-lookup"><span data-stu-id="f41c0-620">Define associations in the conceptual model that are not based on foreign keys in the relational schema.</span></span>
-   <span data-ttu-id="f41c0-621">Применение пользовательской бизнес-логики для задания значений свойств в концептуальной модели.</span><span class="sxs-lookup"><span data-stu-id="f41c0-621">Use custom business logic to set the value of properties in the conceptual model.</span></span> <span data-ttu-id="f41c0-622">Например, строковое значение «T» в источнике данных можно преобразовать в значение « **true**» (логическое) в концептуальной модели.</span><span class="sxs-lookup"><span data-stu-id="f41c0-622">For example, you could map the string value "T" in the data source to a value of **true**, a Boolean, in the conceptual model.</span></span>
-   <span data-ttu-id="f41c0-623">Определение условных фильтров для результатов запроса.</span><span class="sxs-lookup"><span data-stu-id="f41c0-623">Define conditional filters for query results.</span></span>
-   <span data-ttu-id="f41c0-624">Наложение меньшего числа ограничений на данные в концептуальной модели, нежели в модели хранения.</span><span class="sxs-lookup"><span data-stu-id="f41c0-624">Enforce fewer restrictions on data in the conceptual model than in the storage model.</span></span> <span data-ttu-id="f41c0-625">Например, можно сделать свойство в концептуальной модели обнуляемым, даже если столбец, с которым он сопоставлен, не поддерживает значения **null**.</span><span class="sxs-lookup"><span data-stu-id="f41c0-625">For example, you could make a property in the conceptual model nullable even if the column to which it is mapped does not support **null**values.</span></span>

<span data-ttu-id="f41c0-626">При определении представлений запросов для сущностей необходимо принимать во внимание следующие соображения.</span><span class="sxs-lookup"><span data-stu-id="f41c0-626">The following considerations apply when you define query views for entities:</span></span>

-   <span data-ttu-id="f41c0-627">Представления запросов доступны только для чтения.</span><span class="sxs-lookup"><span data-stu-id="f41c0-627">Query views are read-only.</span></span> <span data-ttu-id="f41c0-628">Обновление сущностей можно осуществлять только с помощью функций изменения.</span><span class="sxs-lookup"><span data-stu-id="f41c0-628">You can only make updates to entities by using modification functions.</span></span>
-   <span data-ttu-id="f41c0-629">В случае определения типа сущности через представление запроса необходимо также определять все связанные сущности через представления запросов.</span><span class="sxs-lookup"><span data-stu-id="f41c0-629">When you define an entity type by a query view, you must also define all related entities by query views.</span></span>
-   <span data-ttu-id="f41c0-630">При сопоставлении связи "многие ко многим" с сущностью в модели хранения, которая представляет связанную таблицу в реляционной схеме, необходимо определить элемент **QueryView** в элементе **AssociationSetMapping** для этой таблицы ссылок.</span><span class="sxs-lookup"><span data-stu-id="f41c0-630">When you map a many-to-many association to an entity in the storage model that represents a link table in the relational schema, you must define a **QueryView** element in the **AssociationSetMapping** element for this link table.</span></span>
-   <span data-ttu-id="f41c0-631">Представления запросов должны быть определены для всех типов в иерархии типов.</span><span class="sxs-lookup"><span data-stu-id="f41c0-631">Query views must be defined for all types in a type hierarchy.</span></span> <span data-ttu-id="f41c0-632">Это можно сделать следующими способами:</span><span class="sxs-lookup"><span data-stu-id="f41c0-632">You can do this in the following ways:</span></span>
-   -   <span data-ttu-id="f41c0-633">С одним элементом **QueryView** , указывающим один Entity SQL запрос, возвращающий объединение всех типов сущностей в иерархии.</span><span class="sxs-lookup"><span data-stu-id="f41c0-633">With a single **QueryView** element that specifies a single Entity SQL query that returns a union of all of the entity types in the hierarchy.</span></span>
    -   <span data-ttu-id="f41c0-634">С одним элементом **QueryView** , указывающим один Entity SQL запрос, использующий оператор Case для возврата определенного типа сущности в иерархии на основе определенного условия.</span><span class="sxs-lookup"><span data-stu-id="f41c0-634">With a single **QueryView** element that specifies a single Entity SQL query that uses the CASE operator to return a specific entity type in the hierarchy based on a specific condition.</span></span>
    -   <span data-ttu-id="f41c0-635">С дополнительным элементом **QueryView** для определенного типа в иерархии.</span><span class="sxs-lookup"><span data-stu-id="f41c0-635">With an additional **QueryView** element for a specific type in the hierarchy.</span></span> <span data-ttu-id="f41c0-636">В этом случае используйте атрибут **TypeName** элемента **QueryView** , чтобы указать тип сущности для каждого представления.</span><span class="sxs-lookup"><span data-stu-id="f41c0-636">In this case, use the **TypeName** attribute of the **QueryView** element to specify the entity type for each view.</span></span>
-   <span data-ttu-id="f41c0-637">Если задано представление запроса, нельзя указать атрибут **сторажесетнаме** в элементе **EntitySetMapping** .</span><span class="sxs-lookup"><span data-stu-id="f41c0-637">When a query view is defined, you cannot specify the **StorageSetName** attribute on the **EntitySetMapping** element.</span></span>
-   <span data-ttu-id="f41c0-638">Если определено представление запроса, элемент **EntitySetMapping**также не может содержать сопоставления **свойств** .</span><span class="sxs-lookup"><span data-stu-id="f41c0-638">When a query view is defined, the **EntitySetMapping**element cannot also contain **Property** mappings.</span></span>

## <a name="resultbinding-element-msl"></a><span data-ttu-id="f41c0-639">Элемент ResultBinding (язык MSL)</span><span class="sxs-lookup"><span data-stu-id="f41c0-639">ResultBinding Element (MSL)</span></span>

<span data-ttu-id="f41c0-640">Элемент **ResultBinding** в языке MSL сопоставляет значения столбцов, возвращаемые хранимыми процедурами, со свойствами сущностей в концептуальной модели, если функции изменения типов сущностей сопоставлены с хранимыми процедурами в базовой базе данных.</span><span class="sxs-lookup"><span data-stu-id="f41c0-640">The **ResultBinding** element in mapping specification language (MSL) maps column values that are returned by stored procedures to entity properties in the conceptual model when entity type modification functions are mapped to stored procedures in the underlying database.</span></span> <span data-ttu-id="f41c0-641">Например, если значение столбца идентификаторов возвращается хранимой процедурой INSERT, элемент **ResultBinding** сопоставляет возвращенное значение со свойством типа сущности в концептуальной модели.</span><span class="sxs-lookup"><span data-stu-id="f41c0-641">For example, when the value of an identity column is returned by an insert stored procedure, the **ResultBinding** element maps the returned value to an entity type property in the conceptual model.</span></span>

<span data-ttu-id="f41c0-642">Элемент **ResultBinding** может быть дочерним по отношению к элементу InsertFunction или элементу упдатефунктион.</span><span class="sxs-lookup"><span data-stu-id="f41c0-642">The **ResultBinding** element can be child of the InsertFunction element or the UpdateFunction element.</span></span>

<span data-ttu-id="f41c0-643">Элемент **ResultBinding** не может иметь дочерних элементов.</span><span class="sxs-lookup"><span data-stu-id="f41c0-643">The **ResultBinding** element cannot have any child elements.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="f41c0-644">Применимые атрибуты</span><span class="sxs-lookup"><span data-stu-id="f41c0-644">Applicable Attributes</span></span>

<span data-ttu-id="f41c0-645">В следующей таблице описаны атрибуты, применимые к элементу **ResultBinding** .</span><span class="sxs-lookup"><span data-stu-id="f41c0-645">The following table describes the attributes that are applicable to the **ResultBinding** element:</span></span>

| <span data-ttu-id="f41c0-646">Имя атрибута</span><span class="sxs-lookup"><span data-stu-id="f41c0-646">Attribute Name</span></span> | <span data-ttu-id="f41c0-647">Обязательный</span><span class="sxs-lookup"><span data-stu-id="f41c0-647">Is Required</span></span> | <span data-ttu-id="f41c0-648">Значение</span><span class="sxs-lookup"><span data-stu-id="f41c0-648">Value</span></span>                                                                         |
|:---------------|:------------|:------------------------------------------------------------------------------|
| <span data-ttu-id="f41c0-649">**имя**;</span><span class="sxs-lookup"><span data-stu-id="f41c0-649">**Name**</span></span>       | <span data-ttu-id="f41c0-650">Да</span><span class="sxs-lookup"><span data-stu-id="f41c0-650">Yes</span></span>         | <span data-ttu-id="f41c0-651">Имя свойства сущности в концептуальной модели, с которым выполняется сопоставление.</span><span class="sxs-lookup"><span data-stu-id="f41c0-651">The name of the entity property in the conceptual model that is being mapped.</span></span> |
| <span data-ttu-id="f41c0-652">**ColumnName**</span><span class="sxs-lookup"><span data-stu-id="f41c0-652">**ColumnName**</span></span> | <span data-ttu-id="f41c0-653">Да</span><span class="sxs-lookup"><span data-stu-id="f41c0-653">Yes</span></span>         | <span data-ttu-id="f41c0-654">Имя столбца, с которым выполнятся сопоставление.</span><span class="sxs-lookup"><span data-stu-id="f41c0-654">The name of the column being mapped.</span></span>                                          |

### <a name="example"></a><span data-ttu-id="f41c0-655">Пример</span><span class="sxs-lookup"><span data-stu-id="f41c0-655">Example</span></span>

<span data-ttu-id="f41c0-656">Следующий пример основан на модели School и показывает элемент **InsertFunction** , используемый для преобразования функции вставки типа сущности **Person** в хранимую процедуру **инсертперсон** .</span><span class="sxs-lookup"><span data-stu-id="f41c0-656">The following example is based on the School model and shows an **InsertFunction** element used to map the insert function of the **Person** entity type to the **InsertPerson** stored procedure.</span></span> <span data-ttu-id="f41c0-657">(Хранимая процедура **инсертперсон** показана ниже и объявлена в модели хранения.) Элемент **ResultBinding** используется для отображения значения столбца, возвращаемого хранимой процедурой (**невперсонид**), в свойство типа сущности (**PersonID**).</span><span class="sxs-lookup"><span data-stu-id="f41c0-657">(The **InsertPerson** stored procedure is shown below and is declared in the storage model.) A **ResultBinding** element is used to map a column value that is returned by the stored procedure (**NewPersonID**) to an entity type property (**PersonID**).</span></span>

``` xml
 <EntityTypeMapping TypeName="SchoolModel.Person">
   <ModificationFunctionMapping>
     <InsertFunction FunctionName="SchoolModel.Store.InsertPerson">
       <ScalarProperty Name="EnrollmentDate"
                       ParameterName="EnrollmentDate" />
       <ScalarProperty Name="HireDate" ParameterName="HireDate" />
       <ScalarProperty Name="FirstName" ParameterName="FirstName" />
       <ScalarProperty Name="LastName" ParameterName="LastName" />
       <ResultBinding Name="PersonID" ColumnName="NewPersonID" />
     </InsertFunction>
     <UpdateFunction FunctionName="SchoolModel.Store.UpdatePerson">
       <ScalarProperty Name="EnrollmentDate"
                       ParameterName="EnrollmentDate"
                       Version="Current" />
       <ScalarProperty Name="HireDate" ParameterName="HireDate"
                       Version="Current" />
       <ScalarProperty Name="FirstName" ParameterName="FirstName"
                       Version="Current" />
       <ScalarProperty Name="LastName" ParameterName="LastName"
                       Version="Current" />
       <ScalarProperty Name="PersonID" ParameterName="PersonID"
                       Version="Current" />
     </UpdateFunction>
     <DeleteFunction FunctionName="SchoolModel.Store.DeletePerson">
       <ScalarProperty Name="PersonID" ParameterName="PersonID" />
     </DeleteFunction>
   </ModificationFunctionMapping>
 </EntityTypeMapping>
```

<span data-ttu-id="f41c0-658">Следующая инструкция Transact-SQL описывает хранимую процедуру **инсертперсон** :</span><span class="sxs-lookup"><span data-stu-id="f41c0-658">The following Transact-SQL describes the **InsertPerson** stored procedure:</span></span>

``` SQL
 CREATE PROCEDURE [dbo].[InsertPerson]
                                @LastName nvarchar(50),
                                @FirstName nvarchar(50),
                                @HireDate datetime,
                                @EnrollmentDate datetime
                                AS
                                INSERT INTO dbo.Person (LastName,
                                                                             FirstName,
                                                                             HireDate,
                                                                             EnrollmentDate)
                                VALUES (@LastName,
                                               @FirstName,
                                               @HireDate,
                                               @EnrollmentDate);
                                SELECT SCOPE_IDENTITY() as NewPersonID;
```

## <a name="resultmapping-element-msl"></a><span data-ttu-id="f41c0-659">Элемент ResultMapping (язык MSL)</span><span class="sxs-lookup"><span data-stu-id="f41c0-659">ResultMapping Element (MSL)</span></span>

<span data-ttu-id="f41c0-660">Элемент **ресултмаппинг** на языке MSL определяет сопоставление между импортом функции в концептуальной модели и хранимой процедурой в базовой базе данных, если выполняются следующие условия.</span><span class="sxs-lookup"><span data-stu-id="f41c0-660">The **ResultMapping** element in mapping specification language (MSL) defines the mapping between a function import in the conceptual model and a stored procedure in the underlying database when the following are true:</span></span>

-   <span data-ttu-id="f41c0-661">Импорт функции возвращает тип сущности концептуальной модели или сложный тип.</span><span class="sxs-lookup"><span data-stu-id="f41c0-661">The function import returns a conceptual model entity type or complex type.</span></span>
-   <span data-ttu-id="f41c0-662">Имена столбцов, возвращаемые хранимой процедурой, не совпадают в точности с именами свойств в сложном типе или типе сущности.</span><span class="sxs-lookup"><span data-stu-id="f41c0-662">The names of the columns returned by the stored procedure do not exactly match the names of the properties on the entity type or complex type.</span></span>

<span data-ttu-id="f41c0-663">По умолчанию сопоставление между столбцами, возвращаемыми хранимой процедурой, и сложным типом или типом сущности выполняется по именам столбцов и свойств.</span><span class="sxs-lookup"><span data-stu-id="f41c0-663">By default, the mapping between the columns returned by a stored procedure and an entity type or complex type is based on column and property names.</span></span> <span data-ttu-id="f41c0-664">Если имена столбцов не точно соответствуют именам свойств, для определения сопоставления необходимо использовать элемент **ресултмаппинг** .</span><span class="sxs-lookup"><span data-stu-id="f41c0-664">If column names do not exactly match property names, you must use the **ResultMapping** element to define the mapping.</span></span> <span data-ttu-id="f41c0-665">Пример сопоставления по умолчанию см. в разделе элемент FunctionImportMapping (MSL).</span><span class="sxs-lookup"><span data-stu-id="f41c0-665">For an example of the default mapping, see FunctionImportMapping Element (MSL).</span></span>

<span data-ttu-id="f41c0-666">Элемент **ресултмаппинг** является дочерним элементом элемента FunctionImportMapping.</span><span class="sxs-lookup"><span data-stu-id="f41c0-666">The **ResultMapping** element is a child element of the FunctionImportMapping element.</span></span>

<span data-ttu-id="f41c0-667">Элемент **ресултмаппинг** может иметь следующие дочерние элементы:</span><span class="sxs-lookup"><span data-stu-id="f41c0-667">The **ResultMapping** element can have the following child elements:</span></span>

-   <span data-ttu-id="f41c0-668">EntityTypeMapping (ноль и более)</span><span class="sxs-lookup"><span data-stu-id="f41c0-668">EntityTypeMapping (zero or more)</span></span>
-   <span data-ttu-id="f41c0-669">ComplexTypeMapping</span><span class="sxs-lookup"><span data-stu-id="f41c0-669">ComplexTypeMapping</span></span>

<span data-ttu-id="f41c0-670">К элементу **ресултмаппинг** не применяются атрибуты.</span><span class="sxs-lookup"><span data-stu-id="f41c0-670">No attributes are applicable to the **ResultMapping** Element.</span></span>

### <a name="example"></a><span data-ttu-id="f41c0-671">Пример</span><span class="sxs-lookup"><span data-stu-id="f41c0-671">Example</span></span>

<span data-ttu-id="f41c0-672">Рассмотрим следующую хранимую процедуру:</span><span class="sxs-lookup"><span data-stu-id="f41c0-672">Consider the following stored procedure:</span></span>

``` SQL
 CREATE PROCEDURE [dbo].[GetGrades]
             @student_Id int
             AS
             SELECT     EnrollmentID as enroll_id,
                                                                             Grade as grade,
                                                                             CourseID as course_id,
                                                                             StudentID as student_id
                                               FROM dbo.StudentGrade
             WHERE StudentID = @student_Id
```

<span data-ttu-id="f41c0-673">Рассмотрим следующий тип сущности концептуальной модели:</span><span class="sxs-lookup"><span data-stu-id="f41c0-673">Also consider the following conceptual model entity type:</span></span>

``` xml
 <EntityType Name="StudentGrade">
   <Key>
     <PropertyRef Name="EnrollmentID" />
   </Key>
   <Property Name="EnrollmentID" Type="Int32" Nullable="false"
             annotation:StoreGeneratedPattern="Identity" />
   <Property Name="CourseID" Type="Int32" Nullable="false" />
   <Property Name="StudentID" Type="Int32" Nullable="false" />
   <Property Name="Grade" Type="Decimal" Precision="3" Scale="2" />
 </EntityType>
```

<span data-ttu-id="f41c0-674">Чтобы создать импорт функции, возвращающий экземпляры предыдущего типа сущности, сопоставление столбцов, возвращаемых хранимой процедурой и типом сущности, должно быть определено в элементе **ресултмаппинг** :</span><span class="sxs-lookup"><span data-stu-id="f41c0-674">In order to create a function import that returns instances of the previous entity type, the mapping between the columns returned by the stored procedure and the entity type must be defined in a **ResultMapping** element:</span></span>

``` xml
 <FunctionImportMapping FunctionImportName="GetGrades"
                        FunctionName="SchoolModel.Store.GetGrades" >
   <ResultMapping>
     <EntityTypeMapping TypeName="SchoolModel.StudentGrade">
       <ScalarProperty Name="EnrollmentID" ColumnName="enroll_id"/>
       <ScalarProperty Name="CourseID" ColumnName="course_id"/>
       <ScalarProperty Name="StudentID" ColumnName="student_id"/>
       <ScalarProperty Name="Grade" ColumnName="grade"/>
     </EntityTypeMapping>
   </ResultMapping>
 </FunctionImportMapping>
```

## <a name="scalarproperty-element-msl"></a><span data-ttu-id="f41c0-675">Элемент ScalarProperty (MSL)</span><span class="sxs-lookup"><span data-stu-id="f41c0-675">ScalarProperty Element (MSL)</span></span>

<span data-ttu-id="f41c0-676">Элемент **ScalarProperty** на языке MSL сопоставляет свойство типа сущности концептуальной модели, сложного типа или ассоциации со столбцом таблицы или параметром хранимой процедуры в базовой базе данных.</span><span class="sxs-lookup"><span data-stu-id="f41c0-676">The **ScalarProperty** element in mapping specification language (MSL) maps a property on a conceptual model entity type, complex type, or association to a table column or stored procedure parameter in the underlying database.</span></span>

> [!NOTE]
> <span data-ttu-id="f41c0-677">Хранимые процедуры, с которыми сопоставляются функции изменения, должны объявляться в модели хранения.</span><span class="sxs-lookup"><span data-stu-id="f41c0-677">Stored procedures to which modification functions are mapped must be declared in the storage model.</span></span> <span data-ttu-id="f41c0-678">Дополнительные сведения см. в разделе элемент Function (SSDL).</span><span class="sxs-lookup"><span data-stu-id="f41c0-678">For more information, see Function Element (SSDL).</span></span>

<span data-ttu-id="f41c0-679">Элемент **ScalarProperty** может быть дочерним для следующих элементов:</span><span class="sxs-lookup"><span data-stu-id="f41c0-679">The **ScalarProperty** element can be a child of the following elements:</span></span>

-   <span data-ttu-id="f41c0-680">MappingFragment</span><span class="sxs-lookup"><span data-stu-id="f41c0-680">MappingFragment</span></span>
-   <span data-ttu-id="f41c0-681">InsertFunction</span><span class="sxs-lookup"><span data-stu-id="f41c0-681">InsertFunction</span></span>
-   <span data-ttu-id="f41c0-682">UpdateFunction</span><span class="sxs-lookup"><span data-stu-id="f41c0-682">UpdateFunction</span></span>
-   <span data-ttu-id="f41c0-683">DeleteFunction</span><span class="sxs-lookup"><span data-stu-id="f41c0-683">DeleteFunction</span></span>
-   <span data-ttu-id="f41c0-684">EndProperty</span><span class="sxs-lookup"><span data-stu-id="f41c0-684">EndProperty</span></span>
-   <span data-ttu-id="f41c0-685">ComplexProperty</span><span class="sxs-lookup"><span data-stu-id="f41c0-685">ComplexProperty</span></span>
-   <span data-ttu-id="f41c0-686">ResultMapping</span><span class="sxs-lookup"><span data-stu-id="f41c0-686">ResultMapping</span></span>

<span data-ttu-id="f41c0-687">Элемент **ScalarProperty** , являющийся дочерним элементом элемента **MappingFragment**, **комплекспроперти**или **EndProperty** , сопоставляет свойство концептуальной модели со столбцом в базе данных.</span><span class="sxs-lookup"><span data-stu-id="f41c0-687">As a child of the **MappingFragment**, **ComplexProperty**, or **EndProperty** element, the **ScalarProperty** element maps a property in the conceptual model to a column in the database.</span></span> <span data-ttu-id="f41c0-688">Как дочерний элемент элемента **InsertFunction**, **упдатефунктион**или **DeleteFunction** , элемент **ScalarProperty** сопоставляет свойство концептуальной модели с параметром хранимой процедуры.</span><span class="sxs-lookup"><span data-stu-id="f41c0-688">As a child of the **InsertFunction**, **UpdateFunction**, or **DeleteFunction** element, the **ScalarProperty** element maps a property in the conceptual model to a stored procedure parameter.</span></span>

<span data-ttu-id="f41c0-689">Элемент **ScalarProperty** не может иметь дочерних элементов.</span><span class="sxs-lookup"><span data-stu-id="f41c0-689">The **ScalarProperty** element cannot have any child elements.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="f41c0-690">Применимые атрибуты</span><span class="sxs-lookup"><span data-stu-id="f41c0-690">Applicable Attributes</span></span>

<span data-ttu-id="f41c0-691">Атрибуты, применяемые к элементу **ScalarProperty** , различаются в зависимости от роли элемента.</span><span class="sxs-lookup"><span data-stu-id="f41c0-691">The attributes that apply to the **ScalarProperty** element differ depending on the role of the element.</span></span>

<span data-ttu-id="f41c0-692">В следующей таблице описаны атрибуты, применимые, если элемент **ScalarProperty** используется для отображения свойства концептуальной модели в столбце базы данных:</span><span class="sxs-lookup"><span data-stu-id="f41c0-692">The following table describes the attributes that are applicable when the **ScalarProperty** element is used to map a conceptual model property to a column in the database:</span></span>

| <span data-ttu-id="f41c0-693">Имя атрибута</span><span class="sxs-lookup"><span data-stu-id="f41c0-693">Attribute Name</span></span> | <span data-ttu-id="f41c0-694">Обязательный</span><span class="sxs-lookup"><span data-stu-id="f41c0-694">Is Required</span></span> | <span data-ttu-id="f41c0-695">Значение</span><span class="sxs-lookup"><span data-stu-id="f41c0-695">Value</span></span>                                                           |
|:---------------|:------------|:----------------------------------------------------------------|
| <span data-ttu-id="f41c0-696">**имя**;</span><span class="sxs-lookup"><span data-stu-id="f41c0-696">**Name**</span></span>       | <span data-ttu-id="f41c0-697">Да</span><span class="sxs-lookup"><span data-stu-id="f41c0-697">Yes</span></span>         | <span data-ttu-id="f41c0-698">Имя свойства концептуальной модели, с которым выполняется сопоставление.</span><span class="sxs-lookup"><span data-stu-id="f41c0-698">The name of the conceptual model property that is being mapped.</span></span> |
| <span data-ttu-id="f41c0-699">**ColumnName**</span><span class="sxs-lookup"><span data-stu-id="f41c0-699">**ColumnName**</span></span> | <span data-ttu-id="f41c0-700">Да</span><span class="sxs-lookup"><span data-stu-id="f41c0-700">Yes</span></span>         | <span data-ttu-id="f41c0-701">Имя столбца таблицы, с которым производится сопоставление.</span><span class="sxs-lookup"><span data-stu-id="f41c0-701">The name of the table column that is being mapped.</span></span>              |

<span data-ttu-id="f41c0-702">В следующей таблице описаны атрибуты, применимые к элементу **ScalarProperty** , когда он используется для отображения свойства концептуальной модели в параметре хранимой процедуры.</span><span class="sxs-lookup"><span data-stu-id="f41c0-702">The following table describes the attributes that are applicable to the **ScalarProperty** element when it is used to map a conceptual model property to a stored procedure parameter:</span></span>

| <span data-ttu-id="f41c0-703">Имя атрибута</span><span class="sxs-lookup"><span data-stu-id="f41c0-703">Attribute Name</span></span>    | <span data-ttu-id="f41c0-704">Обязательный</span><span class="sxs-lookup"><span data-stu-id="f41c0-704">Is Required</span></span> | <span data-ttu-id="f41c0-705">Значение</span><span class="sxs-lookup"><span data-stu-id="f41c0-705">Value</span></span>                                                                                                                                           |
|:------------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="f41c0-706">**имя**;</span><span class="sxs-lookup"><span data-stu-id="f41c0-706">**Name**</span></span>          | <span data-ttu-id="f41c0-707">Да</span><span class="sxs-lookup"><span data-stu-id="f41c0-707">Yes</span></span>         | <span data-ttu-id="f41c0-708">Имя свойства концептуальной модели, с которым выполняется сопоставление.</span><span class="sxs-lookup"><span data-stu-id="f41c0-708">The name of the conceptual model property that is being mapped.</span></span>                                                                                 |
| <span data-ttu-id="f41c0-709">**ParameterName**</span><span class="sxs-lookup"><span data-stu-id="f41c0-709">**ParameterName**</span></span> | <span data-ttu-id="f41c0-710">Да</span><span class="sxs-lookup"><span data-stu-id="f41c0-710">Yes</span></span>         | <span data-ttu-id="f41c0-711">Имя сопоставляемого параметра.</span><span class="sxs-lookup"><span data-stu-id="f41c0-711">The name of the parameter that is being mapped.</span></span>                                                                                                 |
| <span data-ttu-id="f41c0-712">**Версия**</span><span class="sxs-lookup"><span data-stu-id="f41c0-712">**Version**</span></span>       | <span data-ttu-id="f41c0-713">Нет</span><span class="sxs-lookup"><span data-stu-id="f41c0-713">No</span></span>          | <span data-ttu-id="f41c0-714">**Current** или **Original** в зависимости от того, следует ли использовать текущее значение или исходное значение свойства для проверок параллелизма.</span><span class="sxs-lookup"><span data-stu-id="f41c0-714">**Current** or **Original** depending on whether the current value or the original value of the property should be used for concurrency checks.</span></span> |

### <a name="example"></a><span data-ttu-id="f41c0-715">Пример</span><span class="sxs-lookup"><span data-stu-id="f41c0-715">Example</span></span>

<span data-ttu-id="f41c0-716">В следующем примере показан элемент **ScalarProperty** , используемый двумя способами:</span><span class="sxs-lookup"><span data-stu-id="f41c0-716">The following example shows the **ScalarProperty** element used in two ways:</span></span>

-   <span data-ttu-id="f41c0-717">Для отображения свойств типа сущности **Person** в столбцах таблицы **Person**.</span><span class="sxs-lookup"><span data-stu-id="f41c0-717">To map the properties of the **Person** entity type to the columns of the **Person**table.</span></span>
-   <span data-ttu-id="f41c0-718">Чтобы связать свойства типа сущности **Person** с параметрами хранимой процедуры **упдатеперсон** .</span><span class="sxs-lookup"><span data-stu-id="f41c0-718">To map the properties of the **Person** entity type to the parameters of the **UpdatePerson** stored procedure.</span></span> <span data-ttu-id="f41c0-719">Хранимые процедуры объявляются в модели хранения.</span><span class="sxs-lookup"><span data-stu-id="f41c0-719">The stored procedures are declared in the storage model.</span></span>

``` xml
 <EntitySetMapping Name="People">
   <EntityTypeMapping TypeName="SchoolModel.Person">
     <MappingFragment StoreEntitySet="Person">
       <ScalarProperty Name="PersonID" ColumnName="PersonID" />
       <ScalarProperty Name="LastName" ColumnName="LastName" />
       <ScalarProperty Name="FirstName" ColumnName="FirstName" />
       <ScalarProperty Name="HireDate" ColumnName="HireDate" />
       <ScalarProperty Name="EnrollmentDate"
                       ColumnName="EnrollmentDate" />
     </MappingFragment>
 </EntityTypeMapping>
   <EntityTypeMapping TypeName="SchoolModel.Person">
     <ModificationFunctionMapping>
       <InsertFunction FunctionName="SchoolModel.Store.InsertPerson">
         <ScalarProperty Name="EnrollmentDate"
                         ParameterName="EnrollmentDate" />
         <ScalarProperty Name="HireDate" ParameterName="HireDate" />
         <ScalarProperty Name="FirstName" ParameterName="FirstName" />
         <ScalarProperty Name="LastName" ParameterName="LastName" />
         <ResultBinding Name="PersonID" ColumnName="NewPersonID" />
       </InsertFunction>
       <UpdateFunction FunctionName="SchoolModel.Store.UpdatePerson">
         <ScalarProperty Name="EnrollmentDate"
                         ParameterName="EnrollmentDate"
                         Version="Current" />
         <ScalarProperty Name="HireDate" ParameterName="HireDate"
                         Version="Current" />
         <ScalarProperty Name="FirstName" ParameterName="FirstName"
                         Version="Current" />
         <ScalarProperty Name="LastName" ParameterName="LastName"
                         Version="Current" />
         <ScalarProperty Name="PersonID" ParameterName="PersonID"
                         Version="Current" />
       </UpdateFunction>
       <DeleteFunction FunctionName="SchoolModel.Store.DeletePerson">
         <ScalarProperty Name="PersonID" ParameterName="PersonID" />
       </DeleteFunction>
     </ModificationFunctionMapping>
   </EntityTypeMapping>
 </EntitySetMapping>
```

### <a name="example"></a><span data-ttu-id="f41c0-720">Пример</span><span class="sxs-lookup"><span data-stu-id="f41c0-720">Example</span></span>

<span data-ttu-id="f41c0-721">В следующем примере показан элемент **ScalarProperty** , используемый для сопоставления функций вставки и удаления ассоциации концептуальной модели с хранимыми процедурами в базе данных.</span><span class="sxs-lookup"><span data-stu-id="f41c0-721">The next example shows the **ScalarProperty** element used to map the insert and delete functions of a conceptual model association to stored procedures in the database.</span></span> <span data-ttu-id="f41c0-722">Хранимые процедуры объявляются в модели хранения.</span><span class="sxs-lookup"><span data-stu-id="f41c0-722">The stored procedures are declared in the storage model.</span></span>

``` xml
 <AssociationSetMapping Name="CourseInstructor"
                        TypeName="SchoolModel.CourseInstructor"
                        StoreEntitySet="CourseInstructor">
   <EndProperty Name="Person">
     <ScalarProperty Name="PersonID" ColumnName="PersonID" />
   </EndProperty>
   <EndProperty Name="Course">
     <ScalarProperty Name="CourseID" ColumnName="CourseID" />
   </EndProperty>
   <ModificationFunctionMapping>
     <InsertFunction FunctionName="SchoolModel.Store.InsertCourseInstructor" >   
       <EndProperty Name="Course">
         <ScalarProperty Name="CourseID" ParameterName="courseId"/>
       </EndProperty>
       <EndProperty Name="Person">
         <ScalarProperty Name="PersonID" ParameterName="instructorId"/>
       </EndProperty>
     </InsertFunction>
     <DeleteFunction FunctionName="SchoolModel.Store.DeleteCourseInstructor">
       <EndProperty Name="Course">
         <ScalarProperty Name="CourseID" ParameterName="courseId"/>
       </EndProperty>
       <EndProperty Name="Person">
         <ScalarProperty Name="PersonID" ParameterName="instructorId"/>
       </EndProperty>
     </DeleteFunction>
   </ModificationFunctionMapping>
 </AssociationSetMapping>
```

## <a name="updatefunction-element-msl"></a><span data-ttu-id="f41c0-723">Элемент UpdateFunction (MSL)</span><span class="sxs-lookup"><span data-stu-id="f41c0-723">UpdateFunction Element (MSL)</span></span>

<span data-ttu-id="f41c0-724">Элемент **упдатефунктион** на языке MSL сопоставляет функцию Update типа сущности в концептуальной модели с хранимой процедурой в базовой базе данных.</span><span class="sxs-lookup"><span data-stu-id="f41c0-724">The **UpdateFunction** element in mapping specification language (MSL) maps the update function of an entity type in the conceptual model to a stored procedure in the underlying database.</span></span> <span data-ttu-id="f41c0-725">Хранимые процедуры, с которыми сопоставляются функции изменения, должны объявляться в модели хранения.</span><span class="sxs-lookup"><span data-stu-id="f41c0-725">Stored procedures to which modification functions are mapped must be declared in the storage model.</span></span> <span data-ttu-id="f41c0-726">Дополнительные сведения см. в разделе элемент Function (SSDL).</span><span class="sxs-lookup"><span data-stu-id="f41c0-726">For more information, see Function Element (SSDL).</span></span>

> [!NOTE]
>  <span data-ttu-id="f41c0-727">Если не выполнить сопоставление всех трех операций вставки, обновления или удаления типа сущности с хранимыми процедурами, несопоставленные операции завершатся ошибкой, если они выполняются во время выполнения и создается исключение Упдатиксцептион.</span><span class="sxs-lookup"><span data-stu-id="f41c0-727">If you do not map all three of the insert, update, or delete operations of a entity type to stored procedures, the unmapped operations will fail if executed at runtime and an UpdateException is thrown.</span></span>

<span data-ttu-id="f41c0-728">Элемент **упдатефунктион** может быть дочерним элементом элемента ModificationFunctionMapping и применен к элементу EntityTypeMapping.</span><span class="sxs-lookup"><span data-stu-id="f41c0-728">The **UpdateFunction** element can be a child of the ModificationFunctionMapping element and applied to the EntityTypeMapping element.</span></span>

<span data-ttu-id="f41c0-729">Элемент **упдатефунктион** может иметь следующие дочерние элементы:</span><span class="sxs-lookup"><span data-stu-id="f41c0-729">The **UpdateFunction** element can have the following child elements:</span></span>

-   <span data-ttu-id="f41c0-730">AssociationEnd (ноль и более)</span><span class="sxs-lookup"><span data-stu-id="f41c0-730">AssociationEnd (zero or more)</span></span>
-   <span data-ttu-id="f41c0-731">ComplexProperty (ноль и более)</span><span class="sxs-lookup"><span data-stu-id="f41c0-731">ComplexProperty (zero or more)</span></span>
-   <span data-ttu-id="f41c0-732">ResultBinding (ноль или один)</span><span class="sxs-lookup"><span data-stu-id="f41c0-732">ResultBinding (zero or one)</span></span>
-   <span data-ttu-id="f41c0-733">ScarlarProperty (ноль и более)</span><span class="sxs-lookup"><span data-stu-id="f41c0-733">ScarlarProperty (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="f41c0-734">Применимые атрибуты</span><span class="sxs-lookup"><span data-stu-id="f41c0-734">Applicable Attributes</span></span>

<span data-ttu-id="f41c0-735">В следующей таблице описаны атрибуты, которые можно применить к элементу **упдатефунктион** .</span><span class="sxs-lookup"><span data-stu-id="f41c0-735">The following table describes the attributes that can be applied to the **UpdateFunction** element.</span></span>

| <span data-ttu-id="f41c0-736">Имя атрибута</span><span class="sxs-lookup"><span data-stu-id="f41c0-736">Attribute Name</span></span>            | <span data-ttu-id="f41c0-737">Обязательный</span><span class="sxs-lookup"><span data-stu-id="f41c0-737">Is Required</span></span> | <span data-ttu-id="f41c0-738">Значение</span><span class="sxs-lookup"><span data-stu-id="f41c0-738">Value</span></span>                                                                                                                                                    |
|:--------------------------|:------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="f41c0-739">**FunctionName**</span><span class="sxs-lookup"><span data-stu-id="f41c0-739">**FunctionName**</span></span>          | <span data-ttu-id="f41c0-740">Да</span><span class="sxs-lookup"><span data-stu-id="f41c0-740">Yes</span></span>         | <span data-ttu-id="f41c0-741">Имя хранимой процедуры (с указанием пространства имен), с которой сопоставляется функция обновления.</span><span class="sxs-lookup"><span data-stu-id="f41c0-741">The namespace-qualified name of the stored procedure to which the update function is mapped.</span></span> <span data-ttu-id="f41c0-742">Хранимая процедура должна объявляться в модели хранения.</span><span class="sxs-lookup"><span data-stu-id="f41c0-742">The stored procedure must be declared in the storage model.</span></span> |
| <span data-ttu-id="f41c0-743">**ровсаффектедпараметер**</span><span class="sxs-lookup"><span data-stu-id="f41c0-743">**RowsAffectedParameter**</span></span> | <span data-ttu-id="f41c0-744">Нет</span><span class="sxs-lookup"><span data-stu-id="f41c0-744">No</span></span>          | <span data-ttu-id="f41c0-745">Имя выходного параметра, возвращающего количество обработанных строк.</span><span class="sxs-lookup"><span data-stu-id="f41c0-745">The name of the output parameter that returns the number of rows affected.</span></span>                                                                               |

### <a name="example"></a><span data-ttu-id="f41c0-746">Пример</span><span class="sxs-lookup"><span data-stu-id="f41c0-746">Example</span></span>

<span data-ttu-id="f41c0-747">Следующий пример основан на модели School и показывает элемент **упдатефунктион** , используемый для преобразования функции обновления типа сущности **Person** в хранимую процедуру **упдатеперсон** .</span><span class="sxs-lookup"><span data-stu-id="f41c0-747">The following example is based on the School model and shows the **UpdateFunction** element used to map update function of the **Person** entity type to the **UpdatePerson** stored procedure.</span></span> <span data-ttu-id="f41c0-748">Хранимая процедура **упдатеперсон** объявляется в модели хранения.</span><span class="sxs-lookup"><span data-stu-id="f41c0-748">The **UpdatePerson** stored procedure is declared in the storage model.</span></span>

``` xml
 <EntityTypeMapping TypeName="SchoolModel.Person">
   <ModificationFunctionMapping>
     <InsertFunction FunctionName="SchoolModel.Store.InsertPerson">
       <ScalarProperty Name="EnrollmentDate"
                       ParameterName="EnrollmentDate" />
       <ScalarProperty Name="HireDate" ParameterName="HireDate" />
       <ScalarProperty Name="FirstName" ParameterName="FirstName" />
       <ScalarProperty Name="LastName" ParameterName="LastName" />
       <ResultBinding Name="PersonID" ColumnName="NewPersonID" />
     </InsertFunction>
     <UpdateFunction FunctionName="SchoolModel.Store.UpdatePerson">
       <ScalarProperty Name="EnrollmentDate"
                       ParameterName="EnrollmentDate"
                       Version="Current" />
       <ScalarProperty Name="HireDate" ParameterName="HireDate"
                       Version="Current" />
       <ScalarProperty Name="FirstName" ParameterName="FirstName"
                       Version="Current" />
       <ScalarProperty Name="LastName" ParameterName="LastName"
                       Version="Current" />
       <ScalarProperty Name="PersonID" ParameterName="PersonID"
                       Version="Current" />
     </UpdateFunction>
     <DeleteFunction FunctionName="SchoolModel.Store.DeletePerson">
       <ScalarProperty Name="PersonID" ParameterName="PersonID" />
     </DeleteFunction>
   </ModificationFunctionMapping>
 </EntityTypeMapping>
```
