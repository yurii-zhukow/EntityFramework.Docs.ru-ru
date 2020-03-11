---
title: Спецификация MSL — EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 13ae7bc1-74b4-4ee4-8d73-c337be841467
ms.openlocfilehash: 8990d1373ea2121ce11337a43dbcdf3b9e1532bd
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/06/2020
ms.locfileid: "78415481"
---
# <a name="msl-specification"></a><span data-ttu-id="febb6-102">Спецификация MSL</span><span class="sxs-lookup"><span data-stu-id="febb6-102">MSL Specification</span></span>
<span data-ttu-id="febb6-103">Язык MSL — это язык на основе XML, который описывает сопоставление между концептуальной моделью и моделью хранения Entity Framework приложения.</span><span class="sxs-lookup"><span data-stu-id="febb6-103">Mapping specification language (MSL) is an XML-based language that describes the mapping between the conceptual model and storage model of an Entity Framework application.</span></span>

<span data-ttu-id="febb6-104">В приложении Entity Framework метаданные сопоставления загружаются из MSL-файла (написанного на языке MSL) во время сборки.</span><span class="sxs-lookup"><span data-stu-id="febb6-104">In an Entity Framework application, mapping metadata is loaded from an .msl file (written in MSL) at build time.</span></span> <span data-ttu-id="febb6-105">Entity Framework использует метаданные сопоставления во время выполнения для преобразования запросов к концептуальной модели для хранения команд, относящихся к хранению.</span><span class="sxs-lookup"><span data-stu-id="febb6-105">Entity Framework uses mapping metadata at runtime to translate queries against the conceptual model to store-specific commands.</span></span>

<span data-ttu-id="febb6-106">Entity Framework Designer (конструктор EF) сохраняет сведения о сопоставлении в EDMX-файле во время разработки.</span><span class="sxs-lookup"><span data-stu-id="febb6-106">The Entity Framework Designer (EF Designer) stores mapping information in an .edmx file at design time.</span></span> <span data-ttu-id="febb6-107">Во время сборки Entity Designer использует сведения в EDMX-файле для создания MSL-файла, необходимого для Entity Framework во время выполнения.</span><span class="sxs-lookup"><span data-stu-id="febb6-107">At build time, the Entity Designer uses information in an .edmx file to create the .msl file that is needed by Entity Framework at runtime</span></span>

<span data-ttu-id="febb6-108">Имена всех типов концептуальной модели или модели хранения, которые упоминаются в языке MSL, должны указываться вместе с именами соответствующих пространств имен.</span><span class="sxs-lookup"><span data-stu-id="febb6-108">Names of all conceptual or storage model types that are referenced in MSL must be qualified by their respective namespace names.</span></span> <span data-ttu-id="febb6-109">Дополнительные сведения об имени пространства имен концептуальной модели см. в разделе [Спецификация языка CSDL](~/ef6/modeling/designer/advanced/edmx/csdl-spec.md).</span><span class="sxs-lookup"><span data-stu-id="febb6-109">For information about the conceptual model namespace name, see [CSDL Specification](~/ef6/modeling/designer/advanced/edmx/csdl-spec.md).</span></span> <span data-ttu-id="febb6-110">Дополнительные сведения об имени пространства имен модели хранения см. в разделе [Спецификация языка SSDL](~/ef6/modeling/designer/advanced/edmx/ssdl-spec.md).</span><span class="sxs-lookup"><span data-stu-id="febb6-110">For information about the storage model namespace name, see [SSDL Specification](~/ef6/modeling/designer/advanced/edmx/ssdl-spec.md).</span></span>

<span data-ttu-id="febb6-111">Версии MSL отличаются по пространствам имен XML.</span><span class="sxs-lookup"><span data-stu-id="febb6-111">Versions of MSL are differentiated by XML namespaces.</span></span>

| <span data-ttu-id="febb6-112">Версия MSL</span><span class="sxs-lookup"><span data-stu-id="febb6-112">MSL Version</span></span> | <span data-ttu-id="febb6-113">Пространство имен XML</span><span class="sxs-lookup"><span data-stu-id="febb6-113">XML Namespace</span></span>                                        |
|:------------|:-----------------------------------------------------|
| <span data-ttu-id="febb6-114">MSL v1</span><span class="sxs-lookup"><span data-stu-id="febb6-114">MSL v1</span></span>      | <span data-ttu-id="febb6-115">urn: schemas-microsoft-com: Windows: хранилище: сопоставление: CS</span><span class="sxs-lookup"><span data-stu-id="febb6-115">urn:schemas-microsoft-com:windows:storage:mapping:CS</span></span> |
| <span data-ttu-id="febb6-116">MSL v2</span><span class="sxs-lookup"><span data-stu-id="febb6-116">MSL v2</span></span>      | https://schemas.microsoft.com/ado/2008/09/mapping/cs |
| <span data-ttu-id="febb6-117">MSL v3</span><span class="sxs-lookup"><span data-stu-id="febb6-117">MSL v3</span></span>      | https://schemas.microsoft.com/ado/2009/11/mapping/cs  |

## <a name="alias-element-msl"></a><span data-ttu-id="febb6-118">Элемент Alias (язык MSL)</span><span class="sxs-lookup"><span data-stu-id="febb6-118">Alias Element (MSL)</span></span>

<span data-ttu-id="febb6-119">Элемент **Alias** в языке MSL является дочерним по отношению к элементу Mapping, который используется для определения псевдонимов для концептуальной модели и пространств имен модели хранения.</span><span class="sxs-lookup"><span data-stu-id="febb6-119">The **Alias** element in mapping specification language (MSL) is a child of the Mapping element that is used to define aliases for conceptual model and storage model namespaces.</span></span> <span data-ttu-id="febb6-120">Имена всех типов концептуальной модели или модели хранения, которые упоминаются в языке MSL, должны указываться вместе с именами соответствующих пространств имен.</span><span class="sxs-lookup"><span data-stu-id="febb6-120">Names of all conceptual or storage model types that are referenced in MSL must be qualified by their respective namespace names.</span></span> <span data-ttu-id="febb6-121">Дополнительные сведения об имени пространства имен концептуальной модели см. в разделе элемент Schema (CSDL).</span><span class="sxs-lookup"><span data-stu-id="febb6-121">For information about the conceptual model namespace name, see Schema Element (CSDL).</span></span> <span data-ttu-id="febb6-122">Дополнительные сведения об имени пространства имен модели хранения см. в разделе элемент Schema (SSDL).</span><span class="sxs-lookup"><span data-stu-id="febb6-122">For information about the storage model namespace name, see Schema Element (SSDL).</span></span>

<span data-ttu-id="febb6-123">Элемент **Alias** не может иметь дочерние элементы.</span><span class="sxs-lookup"><span data-stu-id="febb6-123">The **Alias** element cannot have child elements.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="febb6-124">Применимые атрибуты</span><span class="sxs-lookup"><span data-stu-id="febb6-124">Applicable Attributes</span></span>

<span data-ttu-id="febb6-125">В следующей таблице описаны атрибуты, которые можно применить к элементу **Alias** .</span><span class="sxs-lookup"><span data-stu-id="febb6-125">The table below describes the attributes that can be applied to the **Alias** element.</span></span>

| <span data-ttu-id="febb6-126">Имя атрибута</span><span class="sxs-lookup"><span data-stu-id="febb6-126">Attribute Name</span></span> | <span data-ttu-id="febb6-127">Обязательный</span><span class="sxs-lookup"><span data-stu-id="febb6-127">Is Required</span></span> | <span data-ttu-id="febb6-128">Значение</span><span class="sxs-lookup"><span data-stu-id="febb6-128">Value</span></span>                                                                     |
|:---------------|:------------|:--------------------------------------------------------------------------|
| <span data-ttu-id="febb6-129">**Key**</span><span class="sxs-lookup"><span data-stu-id="febb6-129">**Key**</span></span>        | <span data-ttu-id="febb6-130">Да</span><span class="sxs-lookup"><span data-stu-id="febb6-130">Yes</span></span>         | <span data-ttu-id="febb6-131">Псевдоним для пространства имен, определяемого атрибутом **value** .</span><span class="sxs-lookup"><span data-stu-id="febb6-131">The alias for the namespace that is specified by the **Value** attribute.</span></span> |
| <span data-ttu-id="febb6-132">**Value**</span><span class="sxs-lookup"><span data-stu-id="febb6-132">**Value**</span></span>      | <span data-ttu-id="febb6-133">Да</span><span class="sxs-lookup"><span data-stu-id="febb6-133">Yes</span></span>         | <span data-ttu-id="febb6-134">Пространство имен, для которого значение элемента **Key** является псевдонимом.</span><span class="sxs-lookup"><span data-stu-id="febb6-134">The namespace for which the value of the **Key** element is an alias.</span></span>     |

### <a name="example"></a><span data-ttu-id="febb6-135">Пример</span><span class="sxs-lookup"><span data-stu-id="febb6-135">Example</span></span>

<span data-ttu-id="febb6-136">В следующем примере показан элемент **Alias** , определяющий псевдоним, `c`для типов, определенных в концептуальной модели.</span><span class="sxs-lookup"><span data-stu-id="febb6-136">The following example shows an **Alias** element that defines an alias, `c`, for types that are defined in the conceptual model.</span></span>

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

## <a name="associationend-element-msl"></a><span data-ttu-id="febb6-137">Элемент AssociationEnd (MSL)</span><span class="sxs-lookup"><span data-stu-id="febb6-137">AssociationEnd Element (MSL)</span></span>

<span data-ttu-id="febb6-138">Элемент **AssociationEnd** на языке MSL используется, когда функции изменения типа сущности в концептуальной модели сопоставляются с хранимыми процедурами в базовой базе данных.</span><span class="sxs-lookup"><span data-stu-id="febb6-138">The **AssociationEnd** element in mapping specification language (MSL) is used when the modification functions of an entity type in the conceptual model are mapped to stored procedures in the underlying database.</span></span> <span data-ttu-id="febb6-139">Если хранимая процедура изменения принимает параметр, значение которого хранится в свойстве Association, элемент **AssociationEnd** сопоставляет значение свойства с параметром.</span><span class="sxs-lookup"><span data-stu-id="febb6-139">If a modification stored procedure takes a parameter whose value is held in an association property, the **AssociationEnd** element maps the property value to the parameter.</span></span> <span data-ttu-id="febb6-140">Дополнительные сведения см. в приведенных ниже примерах.</span><span class="sxs-lookup"><span data-stu-id="febb6-140">For more information, see the example below.</span></span>

<span data-ttu-id="febb6-141">Дополнительные сведения о сопоставлении функций изменения типов сущностей с хранимыми процедурами см. в разделе элемент ModificationFunctionMapping (MSL) и пошаговое руководство. Сопоставление сущности с хранимыми процедурами.</span><span class="sxs-lookup"><span data-stu-id="febb6-141">For more information about mapping modification functions of entity types to stored procedures, see ModificationFunctionMapping Element (MSL) and Walkthrough: Mapping an Entity to Stored Procedures.</span></span>

<span data-ttu-id="febb6-142">Элемент **AssociationEnd** может иметь следующие дочерние элементы:</span><span class="sxs-lookup"><span data-stu-id="febb6-142">The **AssociationEnd** element can have the following child elements:</span></span>

-   <span data-ttu-id="febb6-143">ScalarProperty</span><span class="sxs-lookup"><span data-stu-id="febb6-143">ScalarProperty</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="febb6-144">Применимые атрибуты</span><span class="sxs-lookup"><span data-stu-id="febb6-144">Applicable Attributes</span></span>

<span data-ttu-id="febb6-145">В следующей таблице описаны атрибуты, применимые к элементу **AssociationEnd** .</span><span class="sxs-lookup"><span data-stu-id="febb6-145">The following table describes the attributes that are applicable to the **AssociationEnd** element.</span></span>

| <span data-ttu-id="febb6-146">Имя атрибута</span><span class="sxs-lookup"><span data-stu-id="febb6-146">Attribute Name</span></span>     | <span data-ttu-id="febb6-147">Обязательный</span><span class="sxs-lookup"><span data-stu-id="febb6-147">Is Required</span></span> | <span data-ttu-id="febb6-148">Значение</span><span class="sxs-lookup"><span data-stu-id="febb6-148">Value</span></span>                                                                                                                                                                             |
|:-------------------|:------------|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="febb6-149">**AssociationSet**</span><span class="sxs-lookup"><span data-stu-id="febb6-149">**AssociationSet**</span></span> | <span data-ttu-id="febb6-150">Да</span><span class="sxs-lookup"><span data-stu-id="febb6-150">Yes</span></span>         | <span data-ttu-id="febb6-151">Имя сопоставляемой ассоциации.</span><span class="sxs-lookup"><span data-stu-id="febb6-151">The name of the association that is being mapped.</span></span>                                                                                                                                 |
| <span data-ttu-id="febb6-152">**От**</span><span class="sxs-lookup"><span data-stu-id="febb6-152">**From**</span></span>           | <span data-ttu-id="febb6-153">Да</span><span class="sxs-lookup"><span data-stu-id="febb6-153">Yes</span></span>         | <span data-ttu-id="febb6-154">Значение атрибута **FromRole** свойства навигации, соответствующего сопоставленной ассоциации.</span><span class="sxs-lookup"><span data-stu-id="febb6-154">The value of the **FromRole** attribute of the navigation property that corresponds to the association being mapped.</span></span> <span data-ttu-id="febb6-155">Дополнительные сведения см. в разделе Элемент NavigationProperty (CSDL).</span><span class="sxs-lookup"><span data-stu-id="febb6-155">For more information, see NavigationProperty Element (CSDL).</span></span> |
| <span data-ttu-id="febb6-156">**Чтобы**</span><span class="sxs-lookup"><span data-stu-id="febb6-156">**To**</span></span>             | <span data-ttu-id="febb6-157">Да</span><span class="sxs-lookup"><span data-stu-id="febb6-157">Yes</span></span>         | <span data-ttu-id="febb6-158">Значение атрибута **ToRole** свойства навигации, соответствующего сопоставленной ассоциации.</span><span class="sxs-lookup"><span data-stu-id="febb6-158">The value of the **ToRole** attribute of the navigation property that corresponds to the association being mapped.</span></span> <span data-ttu-id="febb6-159">Дополнительные сведения см. в разделе Элемент NavigationProperty (CSDL).</span><span class="sxs-lookup"><span data-stu-id="febb6-159">For more information, see NavigationProperty Element (CSDL).</span></span>   |

### <a name="example"></a><span data-ttu-id="febb6-160">Пример</span><span class="sxs-lookup"><span data-stu-id="febb6-160">Example</span></span>

<span data-ttu-id="febb6-161">Рассмотрим следующий тип сущности в концептуальной модели:</span><span class="sxs-lookup"><span data-stu-id="febb6-161">Consider the following conceptual model entity type:</span></span>

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

<span data-ttu-id="febb6-162">Предположим, имеется следующая хранимая процедура:</span><span class="sxs-lookup"><span data-stu-id="febb6-162">Also consider the following stored procedure:</span></span>

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

<span data-ttu-id="febb6-163">Чтобы связать функцию Update сущности `Course` с этой хранимой процедурой, необходимо указать значение для параметра **DepartmentID** .</span><span class="sxs-lookup"><span data-stu-id="febb6-163">In order to map the update function of the `Course` entity to this stored procedure, you must supply a value to the **DepartmentID** parameter.</span></span> <span data-ttu-id="febb6-164">Значение для `DepartmentID` не соответствует свойству типа сущности. Оно содержится в независимом сопоставлении с сопоставлением, показанным ниже:</span><span class="sxs-lookup"><span data-stu-id="febb6-164">The value for `DepartmentID` does not correspond to a property on the entity type; it is contained in an independent association whose mapping is shown here:</span></span>

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

<span data-ttu-id="febb6-165">В следующем коде показан элемент **AssociationEnd** , используемый для сопоставления свойства **DepartmentID** **\_ного курса FK\_Course** с хранимой процедурой **упдатекаурсе** (для которой сопоставлена функция Update типа сущности **Course** ):</span><span class="sxs-lookup"><span data-stu-id="febb6-165">The following code shows the **AssociationEnd** element used to map the **DepartmentID** property of the **FK\_Course\_Department** association to the **UpdateCourse** stored procedure (to which the update function of the **Course** entity type is mapped):</span></span>

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

## <a name="associationsetmapping-element-msl"></a><span data-ttu-id="febb6-166">Элемент AssociationSetMapping (MSL)</span><span class="sxs-lookup"><span data-stu-id="febb6-166">AssociationSetMapping Element (MSL)</span></span>

<span data-ttu-id="febb6-167">Элемент **AssociationSetMapping** на языке MSL определяет сопоставление между Ассоциацией в концептуальной модели и столбцами таблицы в базовой базе данных.</span><span class="sxs-lookup"><span data-stu-id="febb6-167">The **AssociationSetMapping** element in mapping specification language (MSL) defines the mapping between an association in the conceptual model and table columns in the underlying database.</span></span>

<span data-ttu-id="febb6-168">Сопоставления в концептуальной модели — это типы, свойства которых представляют столбцы первичного и внешнего ключа в основной базе данных.</span><span class="sxs-lookup"><span data-stu-id="febb6-168">Associations in the conceptual model are types whose properties represent primary and foreign key columns in the underlying database.</span></span> <span data-ttu-id="febb6-169">Элемент **AssociationSetMapping** использует два элемента EndProperty для определения сопоставлений между свойствами и столбцами типа взаимосвязи в базе данных.</span><span class="sxs-lookup"><span data-stu-id="febb6-169">The **AssociationSetMapping** element uses two EndProperty elements to define the mappings between association type properties and columns in the database.</span></span> <span data-ttu-id="febb6-170">Можно поместить условия сопоставления в элемент Condition.</span><span class="sxs-lookup"><span data-stu-id="febb6-170">You can place conditions on these mappings with the Condition element.</span></span> <span data-ttu-id="febb6-171">Сопоставьте функции вставки, обновления и удаления сопоставлений с хранимыми процедурами в базе данных с помощью элемента ModificationFunctionMapping.</span><span class="sxs-lookup"><span data-stu-id="febb6-171">Map the insert, update, and delete functions for associations to stored procedures in the database with the ModificationFunctionMapping element.</span></span> <span data-ttu-id="febb6-172">Определение сопоставлений только для чтения между ассоциациями и столбцами таблицы с помощью Entity SQL строки в элементе QueryView.</span><span class="sxs-lookup"><span data-stu-id="febb6-172">Define read-only mappings between associations and table columns by using an Entity SQL string in a QueryView element.</span></span>

> [!NOTE]
> <span data-ttu-id="febb6-173">Если для ассоциации в концептуальной модели определено ссылочное ограничение, Ассоциация не должна сопоставляться с элементом **AssociationSetMapping** .</span><span class="sxs-lookup"><span data-stu-id="febb6-173">If a referential constraint is defined for an association in the conceptual model, the association does not need to be mapped with an **AssociationSetMapping** element.</span></span> <span data-ttu-id="febb6-174">Если для ассоциации, имеющей справочное ограничение, имеется элемент **AssociationSetMapping** , то сопоставления, определенные в элементе **AssociationSetMapping** , будут игнорироваться.</span><span class="sxs-lookup"><span data-stu-id="febb6-174">If an **AssociationSetMapping** element is present for an association that has a referential constraint, the mappings defined in the **AssociationSetMapping** element will be ignored.</span></span> <span data-ttu-id="febb6-175">Дополнительные сведения см. в разделе элемент ReferentialConstraint (CSDL).</span><span class="sxs-lookup"><span data-stu-id="febb6-175">For more information, see ReferentialConstraint Element (CSDL).</span></span>

<span data-ttu-id="febb6-176">Элемент **AssociationSetMapping** может иметь следующие дочерние элементы</span><span class="sxs-lookup"><span data-stu-id="febb6-176">The **AssociationSetMapping** element can have the following child elements</span></span>

-   <span data-ttu-id="febb6-177">Представление QueryView (ноль или один)</span><span class="sxs-lookup"><span data-stu-id="febb6-177">QueryView (zero or one)</span></span>
-   <span data-ttu-id="febb6-178">EndProperty (ноль или два)</span><span class="sxs-lookup"><span data-stu-id="febb6-178">EndProperty (zero or two)</span></span>
-   <span data-ttu-id="febb6-179">Условие (ноль или более)</span><span class="sxs-lookup"><span data-stu-id="febb6-179">Condition (zero or more)</span></span>
-   <span data-ttu-id="febb6-180">ModificationFunctionMapping (ноль или один)</span><span class="sxs-lookup"><span data-stu-id="febb6-180">ModificationFunctionMapping (zero or one)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="febb6-181">Применимые атрибуты</span><span class="sxs-lookup"><span data-stu-id="febb6-181">Applicable Attributes</span></span>

<span data-ttu-id="febb6-182">В следующей таблице описаны атрибуты, которые можно применить к элементу **AssociationSetMapping** .</span><span class="sxs-lookup"><span data-stu-id="febb6-182">The following table describes the attributes that can be applied to the **AssociationSetMapping** element.</span></span>

| <span data-ttu-id="febb6-183">Имя атрибута</span><span class="sxs-lookup"><span data-stu-id="febb6-183">Attribute Name</span></span>     | <span data-ttu-id="febb6-184">Обязательный</span><span class="sxs-lookup"><span data-stu-id="febb6-184">Is Required</span></span> | <span data-ttu-id="febb6-185">Значение</span><span class="sxs-lookup"><span data-stu-id="febb6-185">Value</span></span>                                                                                       |
|:-------------------|:------------|:--------------------------------------------------------------------------------------------|
| <span data-ttu-id="febb6-186">**Название**</span><span class="sxs-lookup"><span data-stu-id="febb6-186">**Name**</span></span>           | <span data-ttu-id="febb6-187">Да</span><span class="sxs-lookup"><span data-stu-id="febb6-187">Yes</span></span>         | <span data-ttu-id="febb6-188">Имя набора ассоциаций концептуальной модели, с которым выполняется сопоставление.</span><span class="sxs-lookup"><span data-stu-id="febb6-188">The name of the conceptual model association set that is being mapped.</span></span>                      |
| <span data-ttu-id="febb6-189">**Имени**</span><span class="sxs-lookup"><span data-stu-id="febb6-189">**TypeName**</span></span>       | <span data-ttu-id="febb6-190">нет</span><span class="sxs-lookup"><span data-stu-id="febb6-190">No</span></span>          | <span data-ttu-id="febb6-191">Имя типа ассоциации концептуальной модели из полного пространства имен, с которым выполняется сопоставление.</span><span class="sxs-lookup"><span data-stu-id="febb6-191">The namespace-qualified name of the conceptual model association type that is being mapped.</span></span> |
| <span data-ttu-id="febb6-192">**сторинтитисет**</span><span class="sxs-lookup"><span data-stu-id="febb6-192">**StoreEntitySet**</span></span> | <span data-ttu-id="febb6-193">нет</span><span class="sxs-lookup"><span data-stu-id="febb6-193">No</span></span>          | <span data-ttu-id="febb6-194">Имя таблицы, с которой производится сопоставление.</span><span class="sxs-lookup"><span data-stu-id="febb6-194">The name of the table that is being mapped.</span></span>                                                 |

### <a name="example"></a><span data-ttu-id="febb6-195">Пример</span><span class="sxs-lookup"><span data-stu-id="febb6-195">Example</span></span>

<span data-ttu-id="febb6-196">В следующем примере показан элемент **AssociationSetMapping** , в котором набор ассоциаций " **\_FK"\_** "в концептуальной модели сопоставлен с таблицей **Course** в базе данных.</span><span class="sxs-lookup"><span data-stu-id="febb6-196">The following example shows an **AssociationSetMapping** element in which the **FK\_Course\_Department** association set in the conceptual model is mapped to the **Course** table in the database.</span></span> <span data-ttu-id="febb6-197">Сопоставления между свойствами типа ассоциации и столбцами таблицы указываются в дочерних элементах **EndProperty** .</span><span class="sxs-lookup"><span data-stu-id="febb6-197">Mappings between association type properties and table columns are specified in child **EndProperty** elements.</span></span>

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

## <a name="complexproperty-element-msl"></a><span data-ttu-id="febb6-198">Элемент ComplexProperty (MSL)</span><span class="sxs-lookup"><span data-stu-id="febb6-198">ComplexProperty Element (MSL)</span></span>

<span data-ttu-id="febb6-199">Элемент **комплекспроперти** на языке MSL определяет сопоставление между свойством сложного типа в типе сущности концептуальной модели и столбцами таблицы в базовой базе данных.</span><span class="sxs-lookup"><span data-stu-id="febb6-199">A **ComplexProperty** element in mapping specification language (MSL) defines the mapping between a complex type property on a conceptual model entity type and table columns in the underlying database.</span></span> <span data-ttu-id="febb6-200">Сопоставления столбца свойств указаны в дочерних элементах ScalarProperty.</span><span class="sxs-lookup"><span data-stu-id="febb6-200">The property-column mappings are specified in child ScalarProperty elements.</span></span>

<span data-ttu-id="febb6-201">Элемент свойства **complexType** может иметь следующие дочерние элементы:</span><span class="sxs-lookup"><span data-stu-id="febb6-201">The **ComplexType** property element can have the following child elements:</span></span>

-   <span data-ttu-id="febb6-202">ScalarProperty (ноль или более)</span><span class="sxs-lookup"><span data-stu-id="febb6-202">ScalarProperty (zero or more)</span></span>
-   <span data-ttu-id="febb6-203">**Комплекспроперти** (ноль или более)</span><span class="sxs-lookup"><span data-stu-id="febb6-203">**ComplexProperty** (zero or more)</span></span>
-   <span data-ttu-id="febb6-204">Комплексттипемаппинг (ноль или более)</span><span class="sxs-lookup"><span data-stu-id="febb6-204">ComplextTypeMapping (zero or more)</span></span>
-   <span data-ttu-id="febb6-205">Условие (ноль или более)</span><span class="sxs-lookup"><span data-stu-id="febb6-205">Condition (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="febb6-206">Применимые атрибуты</span><span class="sxs-lookup"><span data-stu-id="febb6-206">Applicable Attributes</span></span>

<span data-ttu-id="febb6-207">В следующей таблице описаны атрибуты, применимые к элементу **комплекспроперти** .</span><span class="sxs-lookup"><span data-stu-id="febb6-207">The following table describes the attributes that are applicable to the **ComplexProperty** element:</span></span>

| <span data-ttu-id="febb6-208">Имя атрибута</span><span class="sxs-lookup"><span data-stu-id="febb6-208">Attribute Name</span></span> | <span data-ttu-id="febb6-209">Обязательный</span><span class="sxs-lookup"><span data-stu-id="febb6-209">Is Required</span></span> | <span data-ttu-id="febb6-210">Значение</span><span class="sxs-lookup"><span data-stu-id="febb6-210">Value</span></span>                                                                                            |
|:---------------|:------------|:-------------------------------------------------------------------------------------------------|
| <span data-ttu-id="febb6-211">**Название**</span><span class="sxs-lookup"><span data-stu-id="febb6-211">**Name**</span></span>       | <span data-ttu-id="febb6-212">Да</span><span class="sxs-lookup"><span data-stu-id="febb6-212">Yes</span></span>         | <span data-ttu-id="febb6-213">Имя сложного свойства типа сущности в концептуальной модели, с которым выполняется сопоставление.</span><span class="sxs-lookup"><span data-stu-id="febb6-213">The name of the complex property of an entity type in the conceptual model that is being mapped.</span></span> |
| <span data-ttu-id="febb6-214">**Имени**</span><span class="sxs-lookup"><span data-stu-id="febb6-214">**TypeName**</span></span>   | <span data-ttu-id="febb6-215">нет</span><span class="sxs-lookup"><span data-stu-id="febb6-215">No</span></span>          | <span data-ttu-id="febb6-216">Полное имя пространства имен типа свойства концептуальной модели.</span><span class="sxs-lookup"><span data-stu-id="febb6-216">The namespace-qualified name of the conceptual model property type.</span></span>                              |

### <a name="example"></a><span data-ttu-id="febb6-217">Пример</span><span class="sxs-lookup"><span data-stu-id="febb6-217">Example</span></span>

<span data-ttu-id="febb6-218">Следующий пример основан на модели School.</span><span class="sxs-lookup"><span data-stu-id="febb6-218">The following example is based on the School model.</span></span> <span data-ttu-id="febb6-219">К концептуальной модели добавлен следующий сложный тип.</span><span class="sxs-lookup"><span data-stu-id="febb6-219">The following complex type has been added to the conceptual model:</span></span>

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

<span data-ttu-id="febb6-220">Свойства **LastName** и **FirstName** типа сущности **Person** заменены одним сложным свойством, **именем**:</span><span class="sxs-lookup"><span data-stu-id="febb6-220">The **LastName** and **FirstName** properties of the **Person** entity type have been replaced with one complex property, **Name**:</span></span>

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

<span data-ttu-id="febb6-221">В следующем MSL показан элемент **комплекспроперти** , используемый для отображения свойства **Name** в столбцах базовой базы данных:</span><span class="sxs-lookup"><span data-stu-id="febb6-221">The following MSL shows the **ComplexProperty** element used to map the **Name** property to columns in the underlying database:</span></span>

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

## <a name="complextypemapping-element-msl"></a><span data-ttu-id="febb6-222">Элемент ComplexTypeMapping (MSL)</span><span class="sxs-lookup"><span data-stu-id="febb6-222">ComplexTypeMapping Element (MSL)</span></span>

<span data-ttu-id="febb6-223">Элемент **комплекстипемаппинг** в языке MSL является дочерним элементом элемента ресултмаппинг и определяет сопоставление между импортом функции в концептуальной модели и хранимой процедурой в базовой базе данных, если выполняются следующие условия.</span><span class="sxs-lookup"><span data-stu-id="febb6-223">The **ComplexTypeMapping** element in mapping specification language (MSL) is a child of the ResultMapping element and defines the mapping between a function import in the conceptual model and a stored procedure in the underlying database when the following are true:</span></span>

-   <span data-ttu-id="febb6-224">Импорт функции возвращает концептуальный сложный тип.</span><span class="sxs-lookup"><span data-stu-id="febb6-224">The function import returns a conceptual complex type.</span></span>
-   <span data-ttu-id="febb6-225">Имена столбцов, возвращаемые хранимой процедурой, не соответствуют в точности именам свойств в сложном типе.</span><span class="sxs-lookup"><span data-stu-id="febb6-225">The names of the columns returned by the stored procedure do not exactly match the names of the properties on the complex type.</span></span>

<span data-ttu-id="febb6-226">По умолчанию сопоставление между столбцами, возвращаемыми хранимой процедурой, и сложным типом основано на именах столбцов и свойств.</span><span class="sxs-lookup"><span data-stu-id="febb6-226">By default, the mapping between the columns returned by a stored procedure and a complex type is based on column and property names.</span></span> <span data-ttu-id="febb6-227">Если имена столбцов не точно соответствуют именам свойств, для определения сопоставления необходимо использовать элемент **комплекстипемаппинг** .</span><span class="sxs-lookup"><span data-stu-id="febb6-227">If column names do not exactly match property names, you must use the **ComplexTypeMapping** element to define the mapping.</span></span> <span data-ttu-id="febb6-228">Пример сопоставления по умолчанию см. в разделе элемент FunctionImportMapping (MSL).</span><span class="sxs-lookup"><span data-stu-id="febb6-228">For an example of the default mapping, see FunctionImportMapping Element (MSL).</span></span>

<span data-ttu-id="febb6-229">Элемент **комплекстипемаппинг** может иметь следующие дочерние элементы:</span><span class="sxs-lookup"><span data-stu-id="febb6-229">The **ComplexTypeMapping** element can have the following child elements:</span></span>

-   <span data-ttu-id="febb6-230">ScalarProperty (ноль или более)</span><span class="sxs-lookup"><span data-stu-id="febb6-230">ScalarProperty (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="febb6-231">Применимые атрибуты</span><span class="sxs-lookup"><span data-stu-id="febb6-231">Applicable Attributes</span></span>

<span data-ttu-id="febb6-232">В следующей таблице описаны атрибуты, применимые к элементу **комплекстипемаппинг** .</span><span class="sxs-lookup"><span data-stu-id="febb6-232">The following table describes the attributes that are applicable to the **ComplexTypeMapping** element.</span></span>

| <span data-ttu-id="febb6-233">Имя атрибута</span><span class="sxs-lookup"><span data-stu-id="febb6-233">Attribute Name</span></span> | <span data-ttu-id="febb6-234">Обязательный</span><span class="sxs-lookup"><span data-stu-id="febb6-234">Is Required</span></span> | <span data-ttu-id="febb6-235">Значение</span><span class="sxs-lookup"><span data-stu-id="febb6-235">Value</span></span>                                                                  |
|:---------------|:------------|:-----------------------------------------------------------------------|
| <span data-ttu-id="febb6-236">**Имени**</span><span class="sxs-lookup"><span data-stu-id="febb6-236">**TypeName**</span></span>   | <span data-ttu-id="febb6-237">Да</span><span class="sxs-lookup"><span data-stu-id="febb6-237">Yes</span></span>         | <span data-ttu-id="febb6-238">Имя сопоставляемого сложного типа с указанием пространства имен.</span><span class="sxs-lookup"><span data-stu-id="febb6-238">The namespace-qualified name of the complex type that is being mapped.</span></span> |

### <a name="example"></a><span data-ttu-id="febb6-239">Пример</span><span class="sxs-lookup"><span data-stu-id="febb6-239">Example</span></span>

<span data-ttu-id="febb6-240">Рассмотрим следующую хранимую процедуру:</span><span class="sxs-lookup"><span data-stu-id="febb6-240">Consider the following stored procedure:</span></span>

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

<span data-ttu-id="febb6-241">Следует также обратить внимание на следующий сложный тип концептуальной модели:</span><span class="sxs-lookup"><span data-stu-id="febb6-241">Also consider the following conceptual model complex type:</span></span>

``` xml
 <ComplexType Name="GradeInfo">
   <Property Type="Int32" Name="EnrollmentID" Nullable="false" />
   <Property Type="Decimal" Name="Grade" Nullable="true"
             Precision="3" Scale="2" />
   <Property Type="Int32" Name="CourseID" Nullable="false" />
   <Property Type="Int32" Name="StudentID" Nullable="false" />
 </ComplexType>
```

<span data-ttu-id="febb6-242">Чтобы создать импорт функции, возвращающий экземпляры предыдущего сложного типа, сопоставление столбцов, возвращаемых хранимой процедурой и типом сущности, должно быть определено в элементе **комплекстипемаппинг** :</span><span class="sxs-lookup"><span data-stu-id="febb6-242">In order to create a function import that returns instances of the previous complex type, the mapping between the columns returned by the stored procedure and the entity type must be defined in a **ComplexTypeMapping** element:</span></span>

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

## <a name="condition-element-msl"></a><span data-ttu-id="febb6-243">Элемент Condition (MSL)</span><span class="sxs-lookup"><span data-stu-id="febb6-243">Condition Element (MSL)</span></span>

<span data-ttu-id="febb6-244">Элемент **Condition** на языке MSL размещает условия для сопоставлений между концептуальной моделью и базовой базой данных.</span><span class="sxs-lookup"><span data-stu-id="febb6-244">The **Condition** element in mapping specification language (MSL) places conditions on mappings between the conceptual model and the underlying database.</span></span> <span data-ttu-id="febb6-245">Сопоставление, определенное в узле XML, допустимо, если выполняются все условия, указанные в дочерних элементах **Condition** .</span><span class="sxs-lookup"><span data-stu-id="febb6-245">The mapping that is defined within an XML node is valid if all conditions, as specified in child **Condition** elements, are met.</span></span> <span data-ttu-id="febb6-246">В противном случае сопоставление недействительно.</span><span class="sxs-lookup"><span data-stu-id="febb6-246">Otherwise, the mapping is not valid.</span></span> <span data-ttu-id="febb6-247">Например, если элемент MappingFragment содержит одно или несколько дочерних элементов **Condition** , сопоставление, определенное в узле **MappingFragment** , будет действительным только в том случае, если выполняются все условия дочерних элементов **Condition** .</span><span class="sxs-lookup"><span data-stu-id="febb6-247">For example, if a MappingFragment element contains one or more **Condition** child elements, the mapping defined within the **MappingFragment** node will only be valid if all the conditions of the child **Condition** elements are met.</span></span>

<span data-ttu-id="febb6-248">Каждое условие может применяться либо к **имени** (имени свойства сущности концептуальной модели, заданному атрибутом **Name** ), либо к **ColumnName** (имени столбца в базе данных, указанному атрибутом **ColumnName** ).</span><span class="sxs-lookup"><span data-stu-id="febb6-248">Each condition can apply to either a **Name** (the name of a conceptual model entity property, specified by the **Name** attribute), or a **ColumnName** (the name of a column in the database, specified by the **ColumnName** attribute).</span></span> <span data-ttu-id="febb6-249">Если задан атрибут **Name** , условие проверяется по значению свойства сущности.</span><span class="sxs-lookup"><span data-stu-id="febb6-249">When the **Name** attribute is set, the condition is checked against an entity property value.</span></span> <span data-ttu-id="febb6-250">Если атрибут **ColumnName** установлен, условие проверяется на соответствие значению столбца.</span><span class="sxs-lookup"><span data-stu-id="febb6-250">When the **ColumnName** attribute is set, the condition is checked against a column value.</span></span> <span data-ttu-id="febb6-251">В элементе **Condition** можно указать только одно из атрибутов **Name** или **ColumnName** .</span><span class="sxs-lookup"><span data-stu-id="febb6-251">Only one of the **Name** or **ColumnName** attribute can be specified in a **Condition** element.</span></span>

> [!NOTE]
> <span data-ttu-id="febb6-252">Если элемент **Condition** используется в элементе FunctionImportMapping, то только атрибут **Name** неприменим.</span><span class="sxs-lookup"><span data-stu-id="febb6-252">When the **Condition** element is used within a FunctionImportMapping element, only the **Name** attribute is not applicable.</span></span>

<span data-ttu-id="febb6-253">Элемент **Condition** может быть дочерним для следующих элементов:</span><span class="sxs-lookup"><span data-stu-id="febb6-253">The **Condition** element can be a child of the following elements:</span></span>

-   <span data-ttu-id="febb6-254">AssociationSetMapping</span><span class="sxs-lookup"><span data-stu-id="febb6-254">AssociationSetMapping</span></span>
-   <span data-ttu-id="febb6-255">ComplexProperty</span><span class="sxs-lookup"><span data-stu-id="febb6-255">ComplexProperty</span></span>
-   <span data-ttu-id="febb6-256">EntitySetMapping</span><span class="sxs-lookup"><span data-stu-id="febb6-256">EntitySetMapping</span></span>
-   <span data-ttu-id="febb6-257">MappingFragment</span><span class="sxs-lookup"><span data-stu-id="febb6-257">MappingFragment</span></span>
-   <span data-ttu-id="febb6-258">EntityTypeMapping</span><span class="sxs-lookup"><span data-stu-id="febb6-258">EntityTypeMapping</span></span>

<span data-ttu-id="febb6-259">Элемент **Condition** не может иметь дочерних элементов.</span><span class="sxs-lookup"><span data-stu-id="febb6-259">The **Condition** element can have no child elements.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="febb6-260">Применимые атрибуты</span><span class="sxs-lookup"><span data-stu-id="febb6-260">Applicable Attributes</span></span>

<span data-ttu-id="febb6-261">В следующей таблице описаны атрибуты, применимые к элементу **Condition** .</span><span class="sxs-lookup"><span data-stu-id="febb6-261">The following table describes the attributes that are applicable to the **Condition** element:</span></span>

| <span data-ttu-id="febb6-262">Имя атрибута</span><span class="sxs-lookup"><span data-stu-id="febb6-262">Attribute Name</span></span> | <span data-ttu-id="febb6-263">Обязательный</span><span class="sxs-lookup"><span data-stu-id="febb6-263">Is Required</span></span> | <span data-ttu-id="febb6-264">Значение</span><span class="sxs-lookup"><span data-stu-id="febb6-264">Value</span></span>                                                                                                                                                                                                                                                                                         |
|:---------------|:------------|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="febb6-265">**ColumnName**</span><span class="sxs-lookup"><span data-stu-id="febb6-265">**ColumnName**</span></span> | <span data-ttu-id="febb6-266">нет</span><span class="sxs-lookup"><span data-stu-id="febb6-266">No</span></span>          | <span data-ttu-id="febb6-267">Имя столбца таблицы, значение которого используется для оценки условия.</span><span class="sxs-lookup"><span data-stu-id="febb6-267">The name of the table column whose value is used to evaluate the condition.</span></span>                                                                                                                                                                                                                   |
| <span data-ttu-id="febb6-268">**IsNull**</span><span class="sxs-lookup"><span data-stu-id="febb6-268">**IsNull**</span></span>     | <span data-ttu-id="febb6-269">нет</span><span class="sxs-lookup"><span data-stu-id="febb6-269">No</span></span>          | <span data-ttu-id="febb6-270">**true** или **false**.</span><span class="sxs-lookup"><span data-stu-id="febb6-270">**True** or **False**.</span></span> <span data-ttu-id="febb6-271">Если значение равно **true** , а значение столбца равно **null**или если значение равно **false** , а значение столбца не равно **null**, условие имеет значение true.</span><span class="sxs-lookup"><span data-stu-id="febb6-271">If the value is **True** and the column value is **null**, or if the value is **False** and the column value is not **null**, the condition is true.</span></span> <span data-ttu-id="febb6-272">В противном случае условие имеет значение false.</span><span class="sxs-lookup"><span data-stu-id="febb6-272">Otherwise, the condition is false.</span></span> <br/> <span data-ttu-id="febb6-273">Атрибуты **IsNull** и **value** нельзя использовать одновременно.</span><span class="sxs-lookup"><span data-stu-id="febb6-273">The **IsNull** and **Value** attributes cannot be used at the same time.</span></span> |
| <span data-ttu-id="febb6-274">**Value**</span><span class="sxs-lookup"><span data-stu-id="febb6-274">**Value**</span></span>      | <span data-ttu-id="febb6-275">нет</span><span class="sxs-lookup"><span data-stu-id="febb6-275">No</span></span>          | <span data-ttu-id="febb6-276">Значение, с которым сравнивается значение столбца.</span><span class="sxs-lookup"><span data-stu-id="febb6-276">The value with which the column value is compared.</span></span> <span data-ttu-id="febb6-277">Если значения совпадают, условие верно.</span><span class="sxs-lookup"><span data-stu-id="febb6-277">If the values are the same, the condition is true.</span></span> <span data-ttu-id="febb6-278">В противном случае условие имеет значение false.</span><span class="sxs-lookup"><span data-stu-id="febb6-278">Otherwise, the condition is false.</span></span> <br/> <span data-ttu-id="febb6-279">Атрибуты **IsNull** и **value** нельзя использовать одновременно.</span><span class="sxs-lookup"><span data-stu-id="febb6-279">The **IsNull** and **Value** attributes cannot be used at the same time.</span></span>                                                                       |
| <span data-ttu-id="febb6-280">**Название**</span><span class="sxs-lookup"><span data-stu-id="febb6-280">**Name**</span></span>       | <span data-ttu-id="febb6-281">нет</span><span class="sxs-lookup"><span data-stu-id="febb6-281">No</span></span>          | <span data-ttu-id="febb6-282">Имя свойства сущности концептуальной модели, значение которого используется для оценки условия.</span><span class="sxs-lookup"><span data-stu-id="febb6-282">The name of the conceptual model entity property whose value is used to evaluate the condition.</span></span> <br/> <span data-ttu-id="febb6-283">Этот атрибут неприменим, если элемент **Condition** используется в элементе FunctionImportMapping.</span><span class="sxs-lookup"><span data-stu-id="febb6-283">This attribute is not applicable if the **Condition** element is used within a FunctionImportMapping element.</span></span>                                                                           |

### <a name="example"></a><span data-ttu-id="febb6-284">Пример</span><span class="sxs-lookup"><span data-stu-id="febb6-284">Example</span></span>

<span data-ttu-id="febb6-285">В следующем примере элементы **Condition** показаны как дочерние элементы элементов **MappingFragment** .</span><span class="sxs-lookup"><span data-stu-id="febb6-285">The following example shows **Condition** elements as children of **MappingFragment** elements.</span></span> <span data-ttu-id="febb6-286">Если параметр **HireDate** не равен null и **енроллментдате** имеет значение null, данные сопоставляются между типом **счулмодел. Instructor** и столбцами **PersonID** и **HireDate** таблицы **Person** .</span><span class="sxs-lookup"><span data-stu-id="febb6-286">When **HireDate** is not null and **EnrollmentDate** is null, data is mapped between the **SchoolModel.Instructor** type and the **PersonID** and **HireDate** columns of the **Person** table.</span></span> <span data-ttu-id="febb6-287">Если **енроллментдате** имеет значение NOT NULL, а значение **HireDate** равно null, то данные сопоставляются между типом **счулмодел. Student** и столбцами **PersonID** и **регистрации** таблицы **Person** .</span><span class="sxs-lookup"><span data-stu-id="febb6-287">When **EnrollmentDate** is not null and **HireDate** is null, data is mapped between the **SchoolModel.Student** type and the **PersonID** and **Enrollment** columns of the **Person** table.</span></span>

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

## <a name="deletefunction-element-msl"></a><span data-ttu-id="febb6-288">Элемент DeleteFunction (MSL)</span><span class="sxs-lookup"><span data-stu-id="febb6-288">DeleteFunction Element (MSL)</span></span>

<span data-ttu-id="febb6-289">Элемент **DeleteFunction** в языке MSL сопоставляет функцию Delete типа сущности или ассоциации в концептуальной модели с хранимой процедурой в базовой базе данных.</span><span class="sxs-lookup"><span data-stu-id="febb6-289">The **DeleteFunction** element in mapping specification language (MSL) maps the delete function of an entity type or association in the conceptual model to a stored procedure in the underlying database.</span></span> <span data-ttu-id="febb6-290">Хранимые процедуры, с которыми сопоставляются функции изменения, должны объявляться в модели хранения.</span><span class="sxs-lookup"><span data-stu-id="febb6-290">Stored procedures to which modification functions are mapped must be declared in the storage model.</span></span> <span data-ttu-id="febb6-291">Дополнительные сведения см. в разделе элемент Function (SSDL).</span><span class="sxs-lookup"><span data-stu-id="febb6-291">For more information, see Function Element (SSDL).</span></span>

> [!NOTE]
> <span data-ttu-id="febb6-292">Если не выполнить сопоставление всех трех операций вставки, обновления или удаления типа сущности с хранимыми процедурами, несопоставленные операции завершатся ошибкой, если они выполняются во время выполнения и создается исключение Упдатиксцептион.</span><span class="sxs-lookup"><span data-stu-id="febb6-292">If you do not map all three of the insert, update, or delete operations of a entity type to stored procedures, the unmapped operations will fail if executed at runtime and an UpdateException is thrown.</span></span>

### <a name="deletefunction-applied-to-entitytypemapping"></a><span data-ttu-id="febb6-293">Элемент DeleteFunction, применяемый к EntityTypeMapping</span><span class="sxs-lookup"><span data-stu-id="febb6-293">DeleteFunction Applied to EntityTypeMapping</span></span>

<span data-ttu-id="febb6-294">При применении к элементу EntityTypeMapping элемент **DeleteFunction** сопоставляет функцию Delete типа сущности в концептуальной модели с хранимой процедурой.</span><span class="sxs-lookup"><span data-stu-id="febb6-294">When applied to the EntityTypeMapping element, the **DeleteFunction** element maps the delete function of an entity type in the conceptual model to a stored procedure.</span></span>

<span data-ttu-id="febb6-295">Элемент **DeleteFunction** может иметь следующие дочерние элементы при применении к элементу **EntityTypeMapping** :</span><span class="sxs-lookup"><span data-stu-id="febb6-295">The **DeleteFunction** element can have the following child elements when applied to an **EntityTypeMapping** element:</span></span>

-   <span data-ttu-id="febb6-296">AssociationEnd (ноль или более)</span><span class="sxs-lookup"><span data-stu-id="febb6-296">AssociationEnd (zero or more)</span></span>
-   <span data-ttu-id="febb6-297">ComplexProperty (ноль и более)</span><span class="sxs-lookup"><span data-stu-id="febb6-297">ComplexProperty (zero or more)</span></span>
-   <span data-ttu-id="febb6-298">Скарларпроперти (ноль или более)</span><span class="sxs-lookup"><span data-stu-id="febb6-298">ScarlarProperty (zero or more)</span></span>

#### <a name="applicable-attributes"></a><span data-ttu-id="febb6-299">Применимые атрибуты</span><span class="sxs-lookup"><span data-stu-id="febb6-299">Applicable Attributes</span></span>

<span data-ttu-id="febb6-300">В следующей таблице описаны атрибуты, которые можно применить к элементу **DeleteFunction** при его применении к элементу **EntityTypeMapping** .</span><span class="sxs-lookup"><span data-stu-id="febb6-300">The following table describes the attributes that can be applied to the **DeleteFunction** element when it is applied to an **EntityTypeMapping** element.</span></span>

| <span data-ttu-id="febb6-301">Имя атрибута</span><span class="sxs-lookup"><span data-stu-id="febb6-301">Attribute Name</span></span>            | <span data-ttu-id="febb6-302">Обязательный</span><span class="sxs-lookup"><span data-stu-id="febb6-302">Is Required</span></span> | <span data-ttu-id="febb6-303">Значение</span><span class="sxs-lookup"><span data-stu-id="febb6-303">Value</span></span>                                                                                                                                                    |
|:--------------------------|:------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="febb6-304">**FunctionName**</span><span class="sxs-lookup"><span data-stu-id="febb6-304">**FunctionName**</span></span>          | <span data-ttu-id="febb6-305">Да</span><span class="sxs-lookup"><span data-stu-id="febb6-305">Yes</span></span>         | <span data-ttu-id="febb6-306">Имя хранимой процедуры (с указанием пространства имен), с которой сопоставляется функция удаления.</span><span class="sxs-lookup"><span data-stu-id="febb6-306">The namespace-qualified name of the stored procedure to which the delete function is mapped.</span></span> <span data-ttu-id="febb6-307">Хранимая процедура должна объявляться в модели хранения.</span><span class="sxs-lookup"><span data-stu-id="febb6-307">The stored procedure must be declared in the storage model.</span></span> |
| <span data-ttu-id="febb6-308">**ровсаффектедпараметер**</span><span class="sxs-lookup"><span data-stu-id="febb6-308">**RowsAffectedParameter**</span></span> | <span data-ttu-id="febb6-309">нет</span><span class="sxs-lookup"><span data-stu-id="febb6-309">No</span></span>          | <span data-ttu-id="febb6-310">Имя выходного параметра, возвращающего количество обработанных строк.</span><span class="sxs-lookup"><span data-stu-id="febb6-310">The name of the output parameter that returns the number of rows affected.</span></span>                                                                               |

#### <a name="example"></a><span data-ttu-id="febb6-311">Пример</span><span class="sxs-lookup"><span data-stu-id="febb6-311">Example</span></span>

<span data-ttu-id="febb6-312">Следующий пример основан на модели School и показывает элемент **DeleteFunction** , сопоставленный функции Delete типа сущности **Person** с хранимой процедурой **делетеперсон** .</span><span class="sxs-lookup"><span data-stu-id="febb6-312">The following example is based on the School model and shows the **DeleteFunction** element mapping the delete function of the **Person** entity type to the **DeletePerson** stored procedure.</span></span> <span data-ttu-id="febb6-313">Хранимая процедура **делетеперсон** объявляется в модели хранения.</span><span class="sxs-lookup"><span data-stu-id="febb6-313">The **DeletePerson** stored procedure is declared in the storage model.</span></span>

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

### <a name="deletefunction-applied-to-associationsetmapping"></a><span data-ttu-id="febb6-314">Элемент DeleteFunction, применяемый к AssociationSetMapping</span><span class="sxs-lookup"><span data-stu-id="febb6-314">DeleteFunction Applied to AssociationSetMapping</span></span>

<span data-ttu-id="febb6-315">При применении к элементу AssociationSetMapping элемент **DeleteFunction** сопоставляет функцию Delete ассоциации в концептуальной модели с хранимой процедурой.</span><span class="sxs-lookup"><span data-stu-id="febb6-315">When applied to the AssociationSetMapping element, the **DeleteFunction** element maps the delete function of an association in the conceptual model to a stored procedure.</span></span>

<span data-ttu-id="febb6-316">Элемент **DeleteFunction** может иметь следующие дочерние элементы при применении к элементу **AssociationSetMapping** :</span><span class="sxs-lookup"><span data-stu-id="febb6-316">The **DeleteFunction** element can have the following child elements when applied to the **AssociationSetMapping** element:</span></span>

-   <span data-ttu-id="febb6-317">EndProperty</span><span class="sxs-lookup"><span data-stu-id="febb6-317">EndProperty</span></span>

#### <a name="applicable-attributes"></a><span data-ttu-id="febb6-318">Применимые атрибуты</span><span class="sxs-lookup"><span data-stu-id="febb6-318">Applicable Attributes</span></span>

<span data-ttu-id="febb6-319">В следующей таблице описаны атрибуты, которые можно применить к элементу **DeleteFunction** при его применении к элементу **AssociationSetMapping** .</span><span class="sxs-lookup"><span data-stu-id="febb6-319">The following table describes the attributes that can be applied to the **DeleteFunction** element when it is applied to the **AssociationSetMapping** element.</span></span>

| <span data-ttu-id="febb6-320">Имя атрибута</span><span class="sxs-lookup"><span data-stu-id="febb6-320">Attribute Name</span></span>            | <span data-ttu-id="febb6-321">Обязательный</span><span class="sxs-lookup"><span data-stu-id="febb6-321">Is Required</span></span> | <span data-ttu-id="febb6-322">Значение</span><span class="sxs-lookup"><span data-stu-id="febb6-322">Value</span></span>                                                                                                                                                    |
|:--------------------------|:------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="febb6-323">**FunctionName**</span><span class="sxs-lookup"><span data-stu-id="febb6-323">**FunctionName**</span></span>          | <span data-ttu-id="febb6-324">Да</span><span class="sxs-lookup"><span data-stu-id="febb6-324">Yes</span></span>         | <span data-ttu-id="febb6-325">Имя хранимой процедуры (с указанием пространства имен), с которой сопоставляется функция удаления.</span><span class="sxs-lookup"><span data-stu-id="febb6-325">The namespace-qualified name of the stored procedure to which the delete function is mapped.</span></span> <span data-ttu-id="febb6-326">Хранимая процедура должна объявляться в модели хранения.</span><span class="sxs-lookup"><span data-stu-id="febb6-326">The stored procedure must be declared in the storage model.</span></span> |
| <span data-ttu-id="febb6-327">**ровсаффектедпараметер**</span><span class="sxs-lookup"><span data-stu-id="febb6-327">**RowsAffectedParameter**</span></span> | <span data-ttu-id="febb6-328">нет</span><span class="sxs-lookup"><span data-stu-id="febb6-328">No</span></span>          | <span data-ttu-id="febb6-329">Имя выходного параметра, возвращающего количество обработанных строк.</span><span class="sxs-lookup"><span data-stu-id="febb6-329">The name of the output parameter that returns the number of rows affected.</span></span>                                                                               |

#### <a name="example"></a><span data-ttu-id="febb6-330">Пример</span><span class="sxs-lookup"><span data-stu-id="febb6-330">Example</span></span>

<span data-ttu-id="febb6-331">Следующий пример основан на модели School и показывает элемент **DeleteFunction** , используемый для сопоставления функции Delete ассоциации **каурсеинструктор** с хранимой процедурой **делетекаурсеинструктор** .</span><span class="sxs-lookup"><span data-stu-id="febb6-331">The following example is based on the School model and shows the **DeleteFunction** element used to map delete function of the **CourseInstructor** association to the **DeleteCourseInstructor** stored procedure.</span></span> <span data-ttu-id="febb6-332">Хранимая процедура **делетекаурсеинструктор** объявляется в модели хранения.</span><span class="sxs-lookup"><span data-stu-id="febb6-332">The **DeleteCourseInstructor** stored procedure is declared in the storage model.</span></span>

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

## <a name="endproperty-element-msl"></a><span data-ttu-id="febb6-333">Элемент EndProperty (MSL)</span><span class="sxs-lookup"><span data-stu-id="febb6-333">EndProperty Element (MSL)</span></span>

<span data-ttu-id="febb6-334">Элемент **EndProperty** на языке MSL определяет сопоставление между функцией End или модификации ассоциации концептуальной модели и базовой базы данных.</span><span class="sxs-lookup"><span data-stu-id="febb6-334">The **EndProperty** element in mapping specification language (MSL) defines the mapping between an end or a modification function of a conceptual model association and the underlying database.</span></span> <span data-ttu-id="febb6-335">Сопоставление столбца свойств указано в дочернем элементе ScalarProperty.</span><span class="sxs-lookup"><span data-stu-id="febb6-335">The property-column mapping is specified in a child ScalarProperty element.</span></span>

<span data-ttu-id="febb6-336">Если элемент **EndProperty** используется для определения сопоставления в конце ассоциации концептуальной модели, он является дочерним элементом элемента AssociationSetMapping.</span><span class="sxs-lookup"><span data-stu-id="febb6-336">When an **EndProperty** element is used to define the mapping for the end of a conceptual model association, it is a child of an AssociationSetMapping element.</span></span> <span data-ttu-id="febb6-337">Если элемент **EndProperty** используется для определения сопоставления для функции изменения ассоциации концептуальной модели, это дочерний элемент элемента InsertFunction или DeleteFunction.</span><span class="sxs-lookup"><span data-stu-id="febb6-337">When the **EndProperty** element is used to define the mapping for a modification function of a conceptual model association, it is a child of an InsertFunction element or DeleteFunction element.</span></span>

<span data-ttu-id="febb6-338">Элемент **EndProperty** может иметь следующие дочерние элементы:</span><span class="sxs-lookup"><span data-stu-id="febb6-338">The **EndProperty** element can have the following child elements:</span></span>

-   <span data-ttu-id="febb6-339">ScalarProperty (ноль или более)</span><span class="sxs-lookup"><span data-stu-id="febb6-339">ScalarProperty (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="febb6-340">Применимые атрибуты</span><span class="sxs-lookup"><span data-stu-id="febb6-340">Applicable Attributes</span></span>

<span data-ttu-id="febb6-341">В следующей таблице описаны атрибуты, применимые к элементу **EndProperty** .</span><span class="sxs-lookup"><span data-stu-id="febb6-341">The following table describes the attributes that are applicable to the **EndProperty** element:</span></span>

| <span data-ttu-id="febb6-342">Имя атрибута</span><span class="sxs-lookup"><span data-stu-id="febb6-342">Attribute Name</span></span> | <span data-ttu-id="febb6-343">Обязательный</span><span class="sxs-lookup"><span data-stu-id="febb6-343">Is Required</span></span> | <span data-ttu-id="febb6-344">Значение</span><span class="sxs-lookup"><span data-stu-id="febb6-344">Value</span></span>                                                 |
|:---------------|:------------|:------------------------------------------------------|
| <span data-ttu-id="febb6-345">Имя</span><span class="sxs-lookup"><span data-stu-id="febb6-345">Name</span></span>           | <span data-ttu-id="febb6-346">Да</span><span class="sxs-lookup"><span data-stu-id="febb6-346">Yes</span></span>         | <span data-ttu-id="febb6-347">Имя сопоставляемого конца ассоциации.</span><span class="sxs-lookup"><span data-stu-id="febb6-347">The name of the association end that is being mapped.</span></span> |

### <a name="example"></a><span data-ttu-id="febb6-348">Пример</span><span class="sxs-lookup"><span data-stu-id="febb6-348">Example</span></span>

<span data-ttu-id="febb6-349">В следующем примере показан элемент **AssociationSetMapping** , в котором отношение **FK\_Course\_ассоциацией между подразделениями** в концептуальной модели сопоставляется с таблицей **Course** в базе данных.</span><span class="sxs-lookup"><span data-stu-id="febb6-349">The following example shows an **AssociationSetMapping** element in which the **FK\_Course\_Department** association in the conceptual model is mapped to the **Course** table in the database.</span></span> <span data-ttu-id="febb6-350">Сопоставления между свойствами типа ассоциации и столбцами таблицы указываются в дочерних элементах **EndProperty** .</span><span class="sxs-lookup"><span data-stu-id="febb6-350">Mappings between association type properties and table columns are specified in child **EndProperty** elements.</span></span>

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

### <a name="example"></a><span data-ttu-id="febb6-351">Пример</span><span class="sxs-lookup"><span data-stu-id="febb6-351">Example</span></span>

<span data-ttu-id="febb6-352">В следующем примере показан элемент **EndProperty** , сопоставленный с функциями INSERT и DELETE ассоциации (**каурсеинструктор**) с хранимыми процедурами в базовой базе данных.</span><span class="sxs-lookup"><span data-stu-id="febb6-352">The following example shows the **EndProperty** element mapping the insert and delete functions of an association (**CourseInstructor**) to stored procedures in the underlying database.</span></span> <span data-ttu-id="febb6-353">Функции, с которыми выполняется сопоставление, объявляются в модели хранения.</span><span class="sxs-lookup"><span data-stu-id="febb6-353">The functions that are mapped to are declared in the storage model.</span></span>

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

## <a name="entitycontainermapping-element-msl"></a><span data-ttu-id="febb6-354">Элемент EntityContainerMapping (MSL)</span><span class="sxs-lookup"><span data-stu-id="febb6-354">EntityContainerMapping Element (MSL)</span></span>

<span data-ttu-id="febb6-355">Элемент **EntityContainerMapping** на языке MSL сопоставляет контейнер сущностей в концептуальной модели с контейнером сущностей в модели хранения.</span><span class="sxs-lookup"><span data-stu-id="febb6-355">The **EntityContainerMapping** element in mapping specification language (MSL) maps the entity container in the conceptual model to the entity container in the storage model.</span></span> <span data-ttu-id="febb6-356">Элемент **EntityContainerMapping** является дочерним по отношению к элементу Mapping.</span><span class="sxs-lookup"><span data-stu-id="febb6-356">The **EntityContainerMapping** element is a child of the Mapping element.</span></span>

<span data-ttu-id="febb6-357">Элемент **EntityContainerMapping** может иметь следующие дочерние элементы (в указанном порядке):</span><span class="sxs-lookup"><span data-stu-id="febb6-357">The **EntityContainerMapping** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="febb6-358">EntitySetMapping (ноль и более)</span><span class="sxs-lookup"><span data-stu-id="febb6-358">EntitySetMapping (zero or more)</span></span>
-   <span data-ttu-id="febb6-359">AssociationSetMapping (ноль или более)</span><span class="sxs-lookup"><span data-stu-id="febb6-359">AssociationSetMapping (zero or more)</span></span>
-   <span data-ttu-id="febb6-360">FunctionImportMapping (ноль или более)</span><span class="sxs-lookup"><span data-stu-id="febb6-360">FunctionImportMapping (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="febb6-361">Применимые атрибуты</span><span class="sxs-lookup"><span data-stu-id="febb6-361">Applicable Attributes</span></span>

<span data-ttu-id="febb6-362">В следующей таблице описаны атрибуты, которые можно применить к элементу **EntityContainerMapping** .</span><span class="sxs-lookup"><span data-stu-id="febb6-362">The following table describes the attributes that can be applied to the **EntityContainerMapping** element.</span></span>

| <span data-ttu-id="febb6-363">Имя атрибута</span><span class="sxs-lookup"><span data-stu-id="febb6-363">Attribute Name</span></span>            | <span data-ttu-id="febb6-364">Обязательный</span><span class="sxs-lookup"><span data-stu-id="febb6-364">Is Required</span></span> | <span data-ttu-id="febb6-365">Значение</span><span class="sxs-lookup"><span data-stu-id="febb6-365">Value</span></span>                                                                                                                                                                                                                                                    |
|:--------------------------|:------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="febb6-366">**сторажемоделконтаинер**</span><span class="sxs-lookup"><span data-stu-id="febb6-366">**StorageModelContainer**</span></span> | <span data-ttu-id="febb6-367">Да</span><span class="sxs-lookup"><span data-stu-id="febb6-367">Yes</span></span>         | <span data-ttu-id="febb6-368">Имя контейнера сущностей модели хранения, с которым выполняется сопоставление.</span><span class="sxs-lookup"><span data-stu-id="febb6-368">The name of the storage model entity container that is being mapped.</span></span>                                                                                                                                                                                     |
| <span data-ttu-id="febb6-369">**кдментитиконтаинер**</span><span class="sxs-lookup"><span data-stu-id="febb6-369">**CdmEntityContainer**</span></span>    | <span data-ttu-id="febb6-370">Да</span><span class="sxs-lookup"><span data-stu-id="febb6-370">Yes</span></span>         | <span data-ttu-id="febb6-371">Имя контейнера сущностей концептуальной модели, с которым выполняется сопоставление.</span><span class="sxs-lookup"><span data-stu-id="febb6-371">The name of the conceptual model entity container that is being mapped.</span></span>                                                                                                                                                                                  |
| <span data-ttu-id="febb6-372">**женератеупдатевиевс**</span><span class="sxs-lookup"><span data-stu-id="febb6-372">**GenerateUpdateViews**</span></span>   | <span data-ttu-id="febb6-373">нет</span><span class="sxs-lookup"><span data-stu-id="febb6-373">No</span></span>          | <span data-ttu-id="febb6-374">**true** или **false**.</span><span class="sxs-lookup"><span data-stu-id="febb6-374">**True** or **False**.</span></span> <span data-ttu-id="febb6-375">Если задано **значение false**, представления обновлений не создаются.</span><span class="sxs-lookup"><span data-stu-id="febb6-375">If **False**, no update views are generated.</span></span> <span data-ttu-id="febb6-376">Этот атрибут должен иметь значение **false** , если имеется сопоставление только для чтения, которое будет недействительным, так как данные могут не пройти циклический обмен данными.</span><span class="sxs-lookup"><span data-stu-id="febb6-376">This attribute should be set to **False** when you have a read-only mapping that would be invalid because data may not round-trip successfully.</span></span> <br/> <span data-ttu-id="febb6-377">Значение по умолчанию ― **True**.</span><span class="sxs-lookup"><span data-stu-id="febb6-377">The default value is **True**.</span></span> |

### <a name="example"></a><span data-ttu-id="febb6-378">Пример</span><span class="sxs-lookup"><span data-stu-id="febb6-378">Example</span></span>

<span data-ttu-id="febb6-379">В следующем примере показан элемент **EntityContainerMapping** , который сопоставляет контейнер **счулмоделентитиес** (контейнер сущностей концептуальной модели) с контейнером **счулмоделстореконтаинер** (контейнер сущностей модели хранения):</span><span class="sxs-lookup"><span data-stu-id="febb6-379">The following example shows an **EntityContainerMapping** element that maps the **SchoolModelEntities** container (the conceptual model entity container) to the **SchoolModelStoreContainer** container (the storage model entity container):</span></span>

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

## <a name="entitysetmapping-element-msl"></a><span data-ttu-id="febb6-380">Элемент EntitySetMapping (MSL)</span><span class="sxs-lookup"><span data-stu-id="febb6-380">EntitySetMapping Element (MSL)</span></span>

<span data-ttu-id="febb6-381">Элемент **EntitySetMapping** на языке MSL сопоставляет все типы сущности концептуальной модели наборам сущностей в модели хранения.</span><span class="sxs-lookup"><span data-stu-id="febb6-381">The **EntitySetMapping** element in mapping specification language (MSL) maps all types in a conceptual model entity set to entity sets in the storage model.</span></span> <span data-ttu-id="febb6-382">Набор сущностей в концептуальной модели представляет собой логический контейнер для экземпляров сущностей одного типа (и производных типов).</span><span class="sxs-lookup"><span data-stu-id="febb6-382">An entity set in the conceptual model is a logical container for instances of entities of the same type (and derived types).</span></span> <span data-ttu-id="febb6-383">Набор сущностей в модели хранения представляет таблицу или представление в основной базе данных.</span><span class="sxs-lookup"><span data-stu-id="febb6-383">An entity set in the storage model represents a table or view in the underlying database.</span></span> <span data-ttu-id="febb6-384">Набор сущностей концептуальной модели задается значением атрибута **Name** элемента **EntitySetMapping** .</span><span class="sxs-lookup"><span data-stu-id="febb6-384">The conceptual model entity set is specified by the value of the **Name** attribute of the **EntitySetMapping** element.</span></span> <span data-ttu-id="febb6-385">Сопоставленная таблица или представление задаются атрибутом **сторинтитисет** в каждом дочернем элементе MappingFragment или в самом элементе **EntitySetMapping** .</span><span class="sxs-lookup"><span data-stu-id="febb6-385">The mapped-to table or view is specified by the **StoreEntitySet** attribute in each child MappingFragment element or in the **EntitySetMapping** element itself.</span></span>

<span data-ttu-id="febb6-386">Элемент **EntitySetMapping** может иметь следующие дочерние элементы:</span><span class="sxs-lookup"><span data-stu-id="febb6-386">The **EntitySetMapping** element can have the following child elements:</span></span>

-   <span data-ttu-id="febb6-387">EntityTypeMapping (ноль и более)</span><span class="sxs-lookup"><span data-stu-id="febb6-387">EntityTypeMapping (zero or more)</span></span>
-   <span data-ttu-id="febb6-388">Представление QueryView (ноль или один)</span><span class="sxs-lookup"><span data-stu-id="febb6-388">QueryView (zero or one)</span></span>
-   <span data-ttu-id="febb6-389">MappingFragment (ноль или более)</span><span class="sxs-lookup"><span data-stu-id="febb6-389">MappingFragment (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="febb6-390">Применимые атрибуты</span><span class="sxs-lookup"><span data-stu-id="febb6-390">Applicable Attributes</span></span>

<span data-ttu-id="febb6-391">В следующей таблице описаны атрибуты, которые можно применить к элементу **EntitySetMapping** .</span><span class="sxs-lookup"><span data-stu-id="febb6-391">The following table describes the attributes that can be applied to the **EntitySetMapping** element.</span></span>

| <span data-ttu-id="febb6-392">Имя атрибута</span><span class="sxs-lookup"><span data-stu-id="febb6-392">Attribute Name</span></span>           | <span data-ttu-id="febb6-393">Обязательный</span><span class="sxs-lookup"><span data-stu-id="febb6-393">Is Required</span></span> | <span data-ttu-id="febb6-394">Значение</span><span class="sxs-lookup"><span data-stu-id="febb6-394">Value</span></span>                                                                                                                                                                                                                         |
|:-------------------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="febb6-395">**Название**</span><span class="sxs-lookup"><span data-stu-id="febb6-395">**Name**</span></span>                 | <span data-ttu-id="febb6-396">Да</span><span class="sxs-lookup"><span data-stu-id="febb6-396">Yes</span></span>         | <span data-ttu-id="febb6-397">Имя сопоставляемого набора сущностей концептуальной модели.</span><span class="sxs-lookup"><span data-stu-id="febb6-397">The name of the conceptual model entity set that is being mapped.</span></span>                                                                                                                                                             |
| <span data-ttu-id="febb6-398">**Имя типа** **1**</span><span class="sxs-lookup"><span data-stu-id="febb6-398">**TypeName** **1**</span></span>       | <span data-ttu-id="febb6-399">нет</span><span class="sxs-lookup"><span data-stu-id="febb6-399">No</span></span>          | <span data-ttu-id="febb6-400">Имя сопоставляемого типа сущности концептуальной модели.</span><span class="sxs-lookup"><span data-stu-id="febb6-400">The name of the conceptual model entity type that is being mapped.</span></span>                                                                                                                                                            |
| <span data-ttu-id="febb6-401">**Сторинтитисет** **1**</span><span class="sxs-lookup"><span data-stu-id="febb6-401">**StoreEntitySet** **1**</span></span> | <span data-ttu-id="febb6-402">нет</span><span class="sxs-lookup"><span data-stu-id="febb6-402">No</span></span>          | <span data-ttu-id="febb6-403">Имя сопоставляемого набора сущностей модели хранения.</span><span class="sxs-lookup"><span data-stu-id="febb6-403">The name of the storage model entity set that is being mapped to.</span></span>                                                                                                                                                             |
| <span data-ttu-id="febb6-404">**Атрибутом makecolumnsdistinct**</span><span class="sxs-lookup"><span data-stu-id="febb6-404">**MakeColumnsDistinct**</span></span>  | <span data-ttu-id="febb6-405">нет</span><span class="sxs-lookup"><span data-stu-id="febb6-405">No</span></span>          | <span data-ttu-id="febb6-406">**Значение true** или **false** в зависимости от того, возвращаются ли только отдельные строки.</span><span class="sxs-lookup"><span data-stu-id="febb6-406">**True** or **False** depending on whether only distinct rows are returned.</span></span> <br/> <span data-ttu-id="febb6-407">Если для этого атрибута задано значение **true**, атрибуту **женератеупдатевиевс** элемента EntityContainerMapping должно быть присвоено значение **false**.</span><span class="sxs-lookup"><span data-stu-id="febb6-407">If this attribute is set to **True**, the **GenerateUpdateViews** attribute of the EntityContainerMapping element must be set to **False**.</span></span> |

 

<span data-ttu-id="febb6-408">**1** для отображения одного типа сущности в одну таблицу можно использовать атрибуты **TypeName** и **сторинтитисет** вместо дочерних элементов EntityTypeMapping и MappingFragment.</span><span class="sxs-lookup"><span data-stu-id="febb6-408">**1** The **TypeName** and **StoreEntitySet** attributes can be used in place of the EntityTypeMapping and MappingFragment child elements to map a single entity type to a single table.</span></span>

### <a name="example"></a><span data-ttu-id="febb6-409">Пример</span><span class="sxs-lookup"><span data-stu-id="febb6-409">Example</span></span>

<span data-ttu-id="febb6-410">В следующем примере показан элемент **EntitySetMapping** , сопоставляющий три типа (базовый тип и два производных типа) в наборе сущностей **Courses** концептуальной модели с тремя различными таблицами в основной базе данных.</span><span class="sxs-lookup"><span data-stu-id="febb6-410">The following example shows an **EntitySetMapping** element that maps three types (a base type and two derived types) in the **Courses** entity set of the conceptual model to three different tables in the underlying database.</span></span> <span data-ttu-id="febb6-411">Таблицы задаются атрибутом **сторинтитисет** в каждом элементе **MappingFragment** .</span><span class="sxs-lookup"><span data-stu-id="febb6-411">The tables are specified by the **StoreEntitySet** attribute in each **MappingFragment** element.</span></span>

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

## <a name="entitytypemapping-element-msl"></a><span data-ttu-id="febb6-412">Элемент EntityTypeMapping (MSL)</span><span class="sxs-lookup"><span data-stu-id="febb6-412">EntityTypeMapping Element (MSL)</span></span>

<span data-ttu-id="febb6-413">Элемент **EntityTypeMapping** на языке MSL определяет сопоставление между типом сущности в концептуальной модели и таблицах или представлениях в базовой базе данных.</span><span class="sxs-lookup"><span data-stu-id="febb6-413">The **EntityTypeMapping** element in mapping specification language (MSL) defines the mapping between an entity type in the conceptual model and tables or views in the underlying database.</span></span> <span data-ttu-id="febb6-414">Сведения о типах сущностей концептуальной модели и базовых таблицах или представлениях базы данных см. в разделах Элемент EntityType (язык CSDL) и Элемент EntitySet (язык SSDL).</span><span class="sxs-lookup"><span data-stu-id="febb6-414">For information about conceptual model entity types and underlying database tables or views, see EntityType Element (CSDL) and EntitySet Element (SSDL).</span></span> <span data-ttu-id="febb6-415">Сопоставляемый тип сущности концептуальной модели задается атрибутом **TypeName** элемента **EntityTypeMapping** .</span><span class="sxs-lookup"><span data-stu-id="febb6-415">The conceptual model entity type that is being mapped is specified by the **TypeName** attribute of the **EntityTypeMapping** element.</span></span> <span data-ttu-id="febb6-416">Сопоставляемая таблица или представление задается атрибутом **сторинтитисет** дочернего элемента MappingFragment.</span><span class="sxs-lookup"><span data-stu-id="febb6-416">The table or view that is being mapped is specified by the **StoreEntitySet** attribute of the child MappingFragment element.</span></span>

<span data-ttu-id="febb6-417">Дочерний элемент ModificationFunctionMapping может применяться для сопоставления функций вставки, обновления или удаления типов сущностей с хранимыми процедурами в базе данных.</span><span class="sxs-lookup"><span data-stu-id="febb6-417">The ModificationFunctionMapping child element can be used to map the insert, update, or delete functions of entity types to stored procedures in the database.</span></span>

<span data-ttu-id="febb6-418">Элемент **EntityTypeMapping** может иметь следующие дочерние элементы:</span><span class="sxs-lookup"><span data-stu-id="febb6-418">The **EntityTypeMapping** element can have the following child elements:</span></span>

-   <span data-ttu-id="febb6-419">MappingFragment (ноль или более)</span><span class="sxs-lookup"><span data-stu-id="febb6-419">MappingFragment (zero or more)</span></span>
-   <span data-ttu-id="febb6-420">ModificationFunctionMapping (ноль или один)</span><span class="sxs-lookup"><span data-stu-id="febb6-420">ModificationFunctionMapping (zero or one)</span></span>
-   <span data-ttu-id="febb6-421">ScalarProperty</span><span class="sxs-lookup"><span data-stu-id="febb6-421">ScalarProperty</span></span>
-   <span data-ttu-id="febb6-422">Условие</span><span class="sxs-lookup"><span data-stu-id="febb6-422">Condition</span></span>

> [!NOTE]
> <span data-ttu-id="febb6-423">Элементы **MappingFragment** и **ModificationFunctionMapping** не могут одновременно быть дочерними элементами элемента **EntityTypeMapping** .</span><span class="sxs-lookup"><span data-stu-id="febb6-423">**MappingFragment** and **ModificationFunctionMapping** elements cannot be child elements of the **EntityTypeMapping** element at the same time.</span></span>


> [!NOTE]
> <span data-ttu-id="febb6-424">Элементы **ScalarProperty** и **Condition** могут быть дочерними элементами элемента **EntityTypeMapping** , когда он используется в элементе FunctionImportMapping.</span><span class="sxs-lookup"><span data-stu-id="febb6-424">The **ScalarProperty** and **Condition** elements can only be child elements of the **EntityTypeMapping** element when it is used within a FunctionImportMapping element.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="febb6-425">Применимые атрибуты</span><span class="sxs-lookup"><span data-stu-id="febb6-425">Applicable Attributes</span></span>

<span data-ttu-id="febb6-426">В следующей таблице описаны атрибуты, которые можно применить к элементу **EntityTypeMapping** .</span><span class="sxs-lookup"><span data-stu-id="febb6-426">The following table describes the attributes that can be applied to the **EntityTypeMapping** element.</span></span>

| <span data-ttu-id="febb6-427">Имя атрибута</span><span class="sxs-lookup"><span data-stu-id="febb6-427">Attribute Name</span></span> | <span data-ttu-id="febb6-428">Обязательный</span><span class="sxs-lookup"><span data-stu-id="febb6-428">Is Required</span></span> | <span data-ttu-id="febb6-429">Значение</span><span class="sxs-lookup"><span data-stu-id="febb6-429">Value</span></span>                                                                                                                                                                                                |
|:---------------|:------------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="febb6-430">**Имени**</span><span class="sxs-lookup"><span data-stu-id="febb6-430">**TypeName**</span></span>   | <span data-ttu-id="febb6-431">Да</span><span class="sxs-lookup"><span data-stu-id="febb6-431">Yes</span></span>         | <span data-ttu-id="febb6-432">Имя с указанием пространства имен типа сущности концептуальной модели, с которым выполняется сопоставление.</span><span class="sxs-lookup"><span data-stu-id="febb6-432">The namespace-qualified name of the conceptual model entity type that is being mapped.</span></span> <br/> <span data-ttu-id="febb6-433">Если тип является абстрактным или производным типом, значение должно быть равно `IsOfType(Namespace-qualified_type_name)`.</span><span class="sxs-lookup"><span data-stu-id="febb6-433">If the type is abstract or a derived type, the value must be `IsOfType(Namespace-qualified_type_name)`.</span></span> |

### <a name="example"></a><span data-ttu-id="febb6-434">Пример</span><span class="sxs-lookup"><span data-stu-id="febb6-434">Example</span></span>

<span data-ttu-id="febb6-435">В следующем примере показан элемент EntitySetMapping с двумя дочерними элементами **EntityTypeMapping** .</span><span class="sxs-lookup"><span data-stu-id="febb6-435">The following example shows an EntitySetMapping element with two child **EntityTypeMapping** elements.</span></span> <span data-ttu-id="febb6-436">В первом элементе **EntityTypeMapping** тип сущности **счулмодел. Person** сопоставляется с таблицей **Person** .</span><span class="sxs-lookup"><span data-stu-id="febb6-436">In the first **EntityTypeMapping** element, the **SchoolModel.Person** entity type is mapped to the **Person** table.</span></span> <span data-ttu-id="febb6-437">Во втором элементе **EntityTypeMapping** функция обновления типа **счулмодел. Person** сопоставляется с хранимой процедурой **упдатеперсон**в базе данных.</span><span class="sxs-lookup"><span data-stu-id="febb6-437">In the second **EntityTypeMapping** element, the update functionality of the **SchoolModel.Person** type is mapped to a stored procedure, **UpdatePerson**, in the database.</span></span>

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

### <a name="example"></a><span data-ttu-id="febb6-438">Пример</span><span class="sxs-lookup"><span data-stu-id="febb6-438">Example</span></span>

<span data-ttu-id="febb6-439">В следующем примере показано сопоставление иерархии типа, в которой корневой тип является абстрактным.</span><span class="sxs-lookup"><span data-stu-id="febb6-439">The next example shows the mapping of a type hierarchy in which the root type is abstract.</span></span> <span data-ttu-id="febb6-440">Обратите внимание на использование синтаксиса `IsOfType` для атрибутов **TypeName** .</span><span class="sxs-lookup"><span data-stu-id="febb6-440">Note the use of the `IsOfType` syntax for the **TypeName** attributes.</span></span>

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

## <a name="functionimportmapping-element-msl"></a><span data-ttu-id="febb6-441">Элемент FunctionImportMapping (MSL)</span><span class="sxs-lookup"><span data-stu-id="febb6-441">FunctionImportMapping Element (MSL)</span></span>

<span data-ttu-id="febb6-442">Элемент **FunctionImportMapping** на языке MSL определяет сопоставление между импортом функции в концептуальной модели и хранимой процедурой или функцией в базовой базе данных.</span><span class="sxs-lookup"><span data-stu-id="febb6-442">The **FunctionImportMapping** element in mapping specification language (MSL) defines the mapping between a function import in the conceptual model and a stored procedure or function in the underlying database.</span></span> <span data-ttu-id="febb6-443">Импорт функций должен быть объявлен в концептуальной модели, а хранимые процедуры должны быть объявлены в модели хранения.</span><span class="sxs-lookup"><span data-stu-id="febb6-443">Function imports must be declared in the conceptual model and stored procedures must be declared in the storage model.</span></span> <span data-ttu-id="febb6-444">Дополнительные сведения см. в разделе элемент FunctionImport (CSDL) и элемент Function (SSDL).</span><span class="sxs-lookup"><span data-stu-id="febb6-444">For more information, see FunctionImport Element (CSDL) and Function Element (SSDL).</span></span>

> [!NOTE]
> <span data-ttu-id="febb6-445">По умолчанию импорт функции возвращает тип сущности концептуальной модели или сложный тип. В этом случае имена столбцов, которые вернула основная хранимая процедура, должны точно соответствовать именам свойств в типе концептуальной модели.</span><span class="sxs-lookup"><span data-stu-id="febb6-445">By default, if a function import returns a conceptual model entity type or complex type, then the names of the columns returned by the underlying stored procedure must exactly match the names of the properties on the conceptual model type.</span></span> <span data-ttu-id="febb6-446">Если имена столбцов не точно совпадают с именами свойств, сопоставление должно быть определено в элементе Ресултмаппинг.</span><span class="sxs-lookup"><span data-stu-id="febb6-446">If the column names do not exactly match the property names, the mapping must be defined in a ResultMapping element.</span></span>

<span data-ttu-id="febb6-447">Элемент **FunctionImportMapping** может иметь следующие дочерние элементы:</span><span class="sxs-lookup"><span data-stu-id="febb6-447">The **FunctionImportMapping** element can have the following child elements:</span></span>

-   <span data-ttu-id="febb6-448">Ресултмаппинг (ноль или более)</span><span class="sxs-lookup"><span data-stu-id="febb6-448">ResultMapping (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="febb6-449">Применимые атрибуты</span><span class="sxs-lookup"><span data-stu-id="febb6-449">Applicable Attributes</span></span>

<span data-ttu-id="febb6-450">В следующей таблице описаны атрибуты, применимые к элементу **FunctionImportMapping** .</span><span class="sxs-lookup"><span data-stu-id="febb6-450">The following table describes the attributes that are applicable to the **FunctionImportMapping** element:</span></span>

| <span data-ttu-id="febb6-451">Имя атрибута</span><span class="sxs-lookup"><span data-stu-id="febb6-451">Attribute Name</span></span>         | <span data-ttu-id="febb6-452">Обязательный</span><span class="sxs-lookup"><span data-stu-id="febb6-452">Is Required</span></span> | <span data-ttu-id="febb6-453">Значение</span><span class="sxs-lookup"><span data-stu-id="febb6-453">Value</span></span>                                                                                   |
|:-----------------------|:------------|:----------------------------------------------------------------------------------------|
| <span data-ttu-id="febb6-454">**FunctionImportName**</span><span class="sxs-lookup"><span data-stu-id="febb6-454">**FunctionImportName**</span></span> | <span data-ttu-id="febb6-455">Да</span><span class="sxs-lookup"><span data-stu-id="febb6-455">Yes</span></span>         | <span data-ttu-id="febb6-456">Имя импорта функции в концептуальной модели, с которым выполняется сопоставление.</span><span class="sxs-lookup"><span data-stu-id="febb6-456">The name of the function import in the conceptual model that is being mapped.</span></span>           |
| <span data-ttu-id="febb6-457">**FunctionName**</span><span class="sxs-lookup"><span data-stu-id="febb6-457">**FunctionName**</span></span>       | <span data-ttu-id="febb6-458">Да</span><span class="sxs-lookup"><span data-stu-id="febb6-458">Yes</span></span>         | <span data-ttu-id="febb6-459">Имя импорта функции в модели хранения из полного пространства имен, с которым выполняется сопоставление.</span><span class="sxs-lookup"><span data-stu-id="febb6-459">The namespace-qualified name of the function in the storage model that is being mapped.</span></span> |

### <a name="example"></a><span data-ttu-id="febb6-460">Пример</span><span class="sxs-lookup"><span data-stu-id="febb6-460">Example</span></span>

<span data-ttu-id="febb6-461">Следующий пример основан на модели School.</span><span class="sxs-lookup"><span data-stu-id="febb6-461">The following example is based on the School model.</span></span> <span data-ttu-id="febb6-462">Рассмотрим следующую функцию в модели хранения:</span><span class="sxs-lookup"><span data-stu-id="febb6-462">Consider the following function in the storage model:</span></span>

``` xml
 <Function Name="GetStudentGrades" Aggregate="false"
           BuiltIn="false" NiladicFunction="false"
           IsComposable="false" ParameterTypeSemantics="AllowImplicitConversion"
           Schema="dbo">
   <Parameter Name="StudentID" Type="int" Mode="In" />
 </Function>
```

<span data-ttu-id="febb6-463">Также рассмотрим данный импорт функции в концептуальной модели:</span><span class="sxs-lookup"><span data-stu-id="febb6-463">Also consider this function import in the conceptual model:</span></span>

``` xml
 <FunctionImport Name="GetStudentGrades" EntitySet="StudentGrades"
                 ReturnType="Collection(SchoolModel.StudentGrade)">
   <Parameter Name="StudentID" Mode="In" Type="Int32" />
 </FunctionImport>
```

<span data-ttu-id="febb6-464">В следующем примере показан элемент **FunctionImportMapping** , используемый для отображения функции и импорта функции выше.</span><span class="sxs-lookup"><span data-stu-id="febb6-464">The following example show a **FunctionImportMapping** element used to map the function and function import above to each other:</span></span>

``` xml
 <FunctionImportMapping FunctionImportName="GetStudentGrades"
                        FunctionName="SchoolModel.Store.GetStudentGrades" />
```
 
## <a name="insertfunction-element-msl"></a><span data-ttu-id="febb6-465">Элемент InsertFunction (MSL)</span><span class="sxs-lookup"><span data-stu-id="febb6-465">InsertFunction Element (MSL)</span></span>

<span data-ttu-id="febb6-466">Элемент **InsertFunction** в языке MSL сопоставляет функцию INSERT типа сущности или ассоциацию в концептуальной модели с хранимой процедурой в базовой базе данных.</span><span class="sxs-lookup"><span data-stu-id="febb6-466">The **InsertFunction** element in mapping specification language (MSL) maps the insert function of an entity type or association in the conceptual model to a stored procedure in the underlying database.</span></span> <span data-ttu-id="febb6-467">Хранимые процедуры, с которыми сопоставляются функции изменения, должны объявляться в модели хранения.</span><span class="sxs-lookup"><span data-stu-id="febb6-467">Stored procedures to which modification functions are mapped must be declared in the storage model.</span></span> <span data-ttu-id="febb6-468">Дополнительные сведения см. в разделе элемент Function (SSDL).</span><span class="sxs-lookup"><span data-stu-id="febb6-468">For more information, see Function Element (SSDL).</span></span>

> [!NOTE]
> <span data-ttu-id="febb6-469">Если не выполнить сопоставление всех трех операций вставки, обновления или удаления типа сущности с хранимыми процедурами, несопоставленные операции завершатся ошибкой, если они выполняются во время выполнения и создается исключение Упдатиксцептион.</span><span class="sxs-lookup"><span data-stu-id="febb6-469">If you do not map all three of the insert, update, or delete operations of a entity type to stored procedures, the unmapped operations will fail if executed at runtime and an UpdateException is thrown.</span></span>

<span data-ttu-id="febb6-470">Элемент **InsertFunction** может быть дочерним элементом элемента ModificationFunctionMapping и применен к элементу EntityTypeMapping или элементу AssociationSetMapping.</span><span class="sxs-lookup"><span data-stu-id="febb6-470">The **InsertFunction** element can be a child of the ModificationFunctionMapping element and applied to the EntityTypeMapping element or the AssociationSetMapping element.</span></span>

### <a name="insertfunction-applied-to-entitytypemapping"></a><span data-ttu-id="febb6-471">Элемент InsertFunction, применяемый к EntityTypeMapping</span><span class="sxs-lookup"><span data-stu-id="febb6-471">InsertFunction Applied to EntityTypeMapping</span></span>

<span data-ttu-id="febb6-472">При применении к элементу EntityTypeMapping элемент **InsertFunction** сопоставляет функцию вставки типа сущности в концептуальной модели с хранимой процедурой.</span><span class="sxs-lookup"><span data-stu-id="febb6-472">When applied to the EntityTypeMapping element, the **InsertFunction** element maps the insert function of an entity type in the conceptual model to a stored procedure.</span></span>

<span data-ttu-id="febb6-473">Элемент **InsertFunction** может иметь следующие дочерние элементы при применении к элементу **EntityTypeMapping** :</span><span class="sxs-lookup"><span data-stu-id="febb6-473">The **InsertFunction** element can have the following child elements when applied to an **EntityTypeMapping** element:</span></span>

-   <span data-ttu-id="febb6-474">AssociationEnd (ноль или более)</span><span class="sxs-lookup"><span data-stu-id="febb6-474">AssociationEnd (zero or more)</span></span>
-   <span data-ttu-id="febb6-475">ComplexProperty (ноль и более)</span><span class="sxs-lookup"><span data-stu-id="febb6-475">ComplexProperty (zero or more)</span></span>
-   <span data-ttu-id="febb6-476">ResultBinding (ноль или один)</span><span class="sxs-lookup"><span data-stu-id="febb6-476">ResultBinding (zero or one)</span></span>
-   <span data-ttu-id="febb6-477">Скарларпроперти (ноль или более)</span><span class="sxs-lookup"><span data-stu-id="febb6-477">ScarlarProperty (zero or more)</span></span>

#### <a name="applicable-attributes"></a><span data-ttu-id="febb6-478">Применимые атрибуты</span><span class="sxs-lookup"><span data-stu-id="febb6-478">Applicable Attributes</span></span>

<span data-ttu-id="febb6-479">В следующей таблице описаны атрибуты, которые можно применить к элементу **InsertFunction** при применении к элементу **EntityTypeMapping** .</span><span class="sxs-lookup"><span data-stu-id="febb6-479">The following table describes the attributes that can be applied to the **InsertFunction** element when applied to an **EntityTypeMapping** element.</span></span>

| <span data-ttu-id="febb6-480">Имя атрибута</span><span class="sxs-lookup"><span data-stu-id="febb6-480">Attribute Name</span></span>            | <span data-ttu-id="febb6-481">Обязательный</span><span class="sxs-lookup"><span data-stu-id="febb6-481">Is Required</span></span> | <span data-ttu-id="febb6-482">Значение</span><span class="sxs-lookup"><span data-stu-id="febb6-482">Value</span></span>                                                                                                                                                    |
|:--------------------------|:------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="febb6-483">**FunctionName**</span><span class="sxs-lookup"><span data-stu-id="febb6-483">**FunctionName**</span></span>          | <span data-ttu-id="febb6-484">Да</span><span class="sxs-lookup"><span data-stu-id="febb6-484">Yes</span></span>         | <span data-ttu-id="febb6-485">Имя хранимой процедуры (с указанием пространства имен), с которой сопоставляется функция вставки.</span><span class="sxs-lookup"><span data-stu-id="febb6-485">The namespace-qualified name of the stored procedure to which the insert function is mapped.</span></span> <span data-ttu-id="febb6-486">Хранимая процедура должна объявляться в модели хранения.</span><span class="sxs-lookup"><span data-stu-id="febb6-486">The stored procedure must be declared in the storage model.</span></span> |
| <span data-ttu-id="febb6-487">**ровсаффектедпараметер**</span><span class="sxs-lookup"><span data-stu-id="febb6-487">**RowsAffectedParameter**</span></span> | <span data-ttu-id="febb6-488">нет</span><span class="sxs-lookup"><span data-stu-id="febb6-488">No</span></span>          | <span data-ttu-id="febb6-489">Имя выходного параметра, возвращающего количество обработанных строк.</span><span class="sxs-lookup"><span data-stu-id="febb6-489">The name of the output parameter that returns the number of affected rows.</span></span>                                                                               |

#### <a name="example"></a><span data-ttu-id="febb6-490">Пример</span><span class="sxs-lookup"><span data-stu-id="febb6-490">Example</span></span>

<span data-ttu-id="febb6-491">Следующий пример основан на модели School и показывает элемент **InsertFunction** , используемый для преобразования функции вставки типа сущности Person в хранимую процедуру **инсертперсон** .</span><span class="sxs-lookup"><span data-stu-id="febb6-491">The following example is based on the School model and shows the **InsertFunction** element used to map insert function of the Person entity type to the **InsertPerson** stored procedure.</span></span> <span data-ttu-id="febb6-492">Хранимая процедура **инсертперсон** объявляется в модели хранения.</span><span class="sxs-lookup"><span data-stu-id="febb6-492">The **InsertPerson** stored procedure is declared in the storage model.</span></span>

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
### <a name="insertfunction-applied-to-associationsetmapping"></a><span data-ttu-id="febb6-493">Элемент InsertFunction, применяемый к AssociationSetMapping</span><span class="sxs-lookup"><span data-stu-id="febb6-493">InsertFunction Applied to AssociationSetMapping</span></span>

<span data-ttu-id="febb6-494">При применении к элементу AssociationSetMapping элемент **InsertFunction** сопоставляет функцию вставки ассоциации в концептуальной модели с хранимой процедурой.</span><span class="sxs-lookup"><span data-stu-id="febb6-494">When applied to the AssociationSetMapping element, the **InsertFunction** element maps the insert function of an association in the conceptual model to a stored procedure.</span></span>

<span data-ttu-id="febb6-495">Элемент **InsertFunction** может иметь следующие дочерние элементы при применении к элементу **AssociationSetMapping** :</span><span class="sxs-lookup"><span data-stu-id="febb6-495">The **InsertFunction** element can have the following child elements when applied to the **AssociationSetMapping** element:</span></span>

-   <span data-ttu-id="febb6-496">EndProperty</span><span class="sxs-lookup"><span data-stu-id="febb6-496">EndProperty</span></span>

#### <a name="applicable-attributes"></a><span data-ttu-id="febb6-497">Применимые атрибуты</span><span class="sxs-lookup"><span data-stu-id="febb6-497">Applicable Attributes</span></span>

<span data-ttu-id="febb6-498">В следующей таблице описаны атрибуты, которые можно применить к элементу **InsertFunction** при его применении к элементу **AssociationSetMapping** .</span><span class="sxs-lookup"><span data-stu-id="febb6-498">The following table describes the attributes that can be applied to the **InsertFunction** element when it is applied to the **AssociationSetMapping** element.</span></span>

| <span data-ttu-id="febb6-499">Имя атрибута</span><span class="sxs-lookup"><span data-stu-id="febb6-499">Attribute Name</span></span>            | <span data-ttu-id="febb6-500">Обязательный</span><span class="sxs-lookup"><span data-stu-id="febb6-500">Is Required</span></span> | <span data-ttu-id="febb6-501">Значение</span><span class="sxs-lookup"><span data-stu-id="febb6-501">Value</span></span>                                                                                                                                                    |
|:--------------------------|:------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="febb6-502">**FunctionName**</span><span class="sxs-lookup"><span data-stu-id="febb6-502">**FunctionName**</span></span>          | <span data-ttu-id="febb6-503">Да</span><span class="sxs-lookup"><span data-stu-id="febb6-503">Yes</span></span>         | <span data-ttu-id="febb6-504">Имя хранимой процедуры (с указанием пространства имен), с которой сопоставляется функция вставки.</span><span class="sxs-lookup"><span data-stu-id="febb6-504">The namespace-qualified name of the stored procedure to which the insert function is mapped.</span></span> <span data-ttu-id="febb6-505">Хранимая процедура должна объявляться в модели хранения.</span><span class="sxs-lookup"><span data-stu-id="febb6-505">The stored procedure must be declared in the storage model.</span></span> |
| <span data-ttu-id="febb6-506">**ровсаффектедпараметер**</span><span class="sxs-lookup"><span data-stu-id="febb6-506">**RowsAffectedParameter**</span></span> | <span data-ttu-id="febb6-507">нет</span><span class="sxs-lookup"><span data-stu-id="febb6-507">No</span></span>          | <span data-ttu-id="febb6-508">Имя выходного параметра, возвращающего количество обработанных строк.</span><span class="sxs-lookup"><span data-stu-id="febb6-508">The name of the output parameter that returns the number of rows affected.</span></span>                                                                               |

#### <a name="example"></a><span data-ttu-id="febb6-509">Пример</span><span class="sxs-lookup"><span data-stu-id="febb6-509">Example</span></span>

<span data-ttu-id="febb6-510">Следующий пример основан на модели School и показывает элемент **InsertFunction** , используемый для сопоставления функции вставки ассоциации **каурсеинструктор** с хранимой процедурой **инсерткаурсеинструктор** .</span><span class="sxs-lookup"><span data-stu-id="febb6-510">The following example is based on the School model and shows the **InsertFunction** element used to map insert function of the **CourseInstructor** association to the **InsertCourseInstructor** stored procedure.</span></span> <span data-ttu-id="febb6-511">Хранимая процедура **инсерткаурсеинструктор** объявляется в модели хранения.</span><span class="sxs-lookup"><span data-stu-id="febb6-511">The **InsertCourseInstructor** stored procedure is declared in the storage model.</span></span>

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

## <a name="mapping-element-msl"></a><span data-ttu-id="febb6-512">Элемент сопоставления (MSL)</span><span class="sxs-lookup"><span data-stu-id="febb6-512">Mapping Element (MSL)</span></span>

<span data-ttu-id="febb6-513">Элемент **Mapping** на языке MSL содержит сведения для сопоставления объектов, определенных в концептуальной модели, с базой данных (как описано в модели хранения).</span><span class="sxs-lookup"><span data-stu-id="febb6-513">The **Mapping** element in mapping specification language (MSL) contains information for mapping objects that are defined in a conceptual model to a database (as described in a storage model).</span></span> <span data-ttu-id="febb6-514">Дополнительные сведения см. в статье спецификации языка CSDL и спецификации SSDL.</span><span class="sxs-lookup"><span data-stu-id="febb6-514">For more information, see CSDL Specification and SSDL Specification.</span></span>

<span data-ttu-id="febb6-515">Элемент **Mapping** является корневым элементом для спецификации сопоставления.</span><span class="sxs-lookup"><span data-stu-id="febb6-515">The **Mapping** element is the root element for a mapping specification.</span></span> <span data-ttu-id="febb6-516">Пространство имен XML для спецификаций сопоставления — https://schemas.microsoft.com/ado/2009/11/mapping/cs.</span><span class="sxs-lookup"><span data-stu-id="febb6-516">The XML namespace for mapping specifications is https://schemas.microsoft.com/ado/2009/11/mapping/cs.</span></span>

<span data-ttu-id="febb6-517">У элемента Mapping могут быть следующие дочерние элементы (в порядке перечисления):</span><span class="sxs-lookup"><span data-stu-id="febb6-517">The mapping element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="febb6-518">Псевдоним (ноль или более)</span><span class="sxs-lookup"><span data-stu-id="febb6-518">Alias (zero or more)</span></span>
-   <span data-ttu-id="febb6-519">EntityContainerMapping (ровно один)</span><span class="sxs-lookup"><span data-stu-id="febb6-519">EntityContainerMapping (exactly one)</span></span>

<span data-ttu-id="febb6-520">Имена типов концептуальной модели и модели хранения, которые упоминаются в языке MSL, должны указываться вместе с именами соответствующих пространств имен.</span><span class="sxs-lookup"><span data-stu-id="febb6-520">Names of conceptual and storage model types that are referenced in MSL must be qualified by their respective namespace names.</span></span> <span data-ttu-id="febb6-521">Дополнительные сведения об имени пространства имен концептуальной модели см. в разделе элемент Schema (CSDL).</span><span class="sxs-lookup"><span data-stu-id="febb6-521">For information about the conceptual model namespace name, see Schema Element (CSDL).</span></span> <span data-ttu-id="febb6-522">Дополнительные сведения об имени пространства имен модели хранения см. в разделе элемент Schema (SSDL).</span><span class="sxs-lookup"><span data-stu-id="febb6-522">For information about the storage model namespace name, see Schema Element (SSDL).</span></span> <span data-ttu-id="febb6-523">Псевдонимы для пространств имен, используемых в языке MSL, могут быть заданы с использованием элемента Alias.</span><span class="sxs-lookup"><span data-stu-id="febb6-523">Aliases for namespaces that are used in MSL can be defined with the Alias element.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="febb6-524">Применимые атрибуты</span><span class="sxs-lookup"><span data-stu-id="febb6-524">Applicable Attributes</span></span>

<span data-ttu-id="febb6-525">В следующей таблице описаны атрибуты, которые можно применить к элементу **Mapping** .</span><span class="sxs-lookup"><span data-stu-id="febb6-525">The table below describes the attributes that can be applied to the **Mapping** element.</span></span>

| <span data-ttu-id="febb6-526">Имя атрибута</span><span class="sxs-lookup"><span data-stu-id="febb6-526">Attribute Name</span></span> | <span data-ttu-id="febb6-527">Обязательный</span><span class="sxs-lookup"><span data-stu-id="febb6-527">Is Required</span></span> | <span data-ttu-id="febb6-528">Значение</span><span class="sxs-lookup"><span data-stu-id="febb6-528">Value</span></span>                                                 |
|:---------------|:------------|:------------------------------------------------------|
| <span data-ttu-id="febb6-529">**ПРОБЕЛ**</span><span class="sxs-lookup"><span data-stu-id="febb6-529">**Space**</span></span>      | <span data-ttu-id="febb6-530">Да</span><span class="sxs-lookup"><span data-stu-id="febb6-530">Yes</span></span>         | <span data-ttu-id="febb6-531">**C-S**.</span><span class="sxs-lookup"><span data-stu-id="febb6-531">**C-S**.</span></span> <span data-ttu-id="febb6-532">Это фиксированное значение, и его невозможно изменить.</span><span class="sxs-lookup"><span data-stu-id="febb6-532">This is a fixed value and cannot be changed.</span></span> |

### <a name="example"></a><span data-ttu-id="febb6-533">Пример</span><span class="sxs-lookup"><span data-stu-id="febb6-533">Example</span></span>

<span data-ttu-id="febb6-534">В следующем примере показан элемент **Mapping** , основанный на части модели School.</span><span class="sxs-lookup"><span data-stu-id="febb6-534">The following example shows a **Mapping** element that is based on part of the School model.</span></span> <span data-ttu-id="febb6-535">Дополнительные сведения о модели School см. в разделе Краткое руководство (Entity Framework):</span><span class="sxs-lookup"><span data-stu-id="febb6-535">For more information about the School model, see Quickstart (Entity Framework):</span></span>

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

## <a name="mappingfragment-element-msl"></a><span data-ttu-id="febb6-536">Элемент MappingFragment (MSL)</span><span class="sxs-lookup"><span data-stu-id="febb6-536">MappingFragment Element (MSL)</span></span>

<span data-ttu-id="febb6-537">Элемент **MappingFragment** на языке MSL определяет сопоставление свойств типа сущности концептуальной модели и таблицы или представления в базе данных.</span><span class="sxs-lookup"><span data-stu-id="febb6-537">The **MappingFragment** element in mapping specification language (MSL) defines the mapping between the properties of a conceptual model entity type and a table or view in the database.</span></span> <span data-ttu-id="febb6-538">Сведения о типах сущностей концептуальной модели и базовых таблицах или представлениях базы данных см. в разделах Элемент EntityType (язык CSDL) и Элемент EntitySet (язык SSDL).</span><span class="sxs-lookup"><span data-stu-id="febb6-538">For information about conceptual model entity types and underlying database tables or views, see EntityType Element (CSDL) and EntitySet Element (SSDL).</span></span> <span data-ttu-id="febb6-539">**MappingFragment** может быть дочерним элементом элемента EntityTypeMapping или элемента EntitySetMapping.</span><span class="sxs-lookup"><span data-stu-id="febb6-539">The **MappingFragment** can be a child element of the EntityTypeMapping element or the EntitySetMapping element.</span></span>

<span data-ttu-id="febb6-540">Элемент **MappingFragment** может иметь следующие дочерние элементы:</span><span class="sxs-lookup"><span data-stu-id="febb6-540">The **MappingFragment** element can have the following child elements:</span></span>

-   <span data-ttu-id="febb6-541">ComplexType (ноль или более)</span><span class="sxs-lookup"><span data-stu-id="febb6-541">ComplexType (zero or more)</span></span>
-   <span data-ttu-id="febb6-542">ScalarProperty (ноль или более)</span><span class="sxs-lookup"><span data-stu-id="febb6-542">ScalarProperty (zero or more)</span></span>
-   <span data-ttu-id="febb6-543">Условие (ноль или более)</span><span class="sxs-lookup"><span data-stu-id="febb6-543">Condition (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="febb6-544">Применимые атрибуты</span><span class="sxs-lookup"><span data-stu-id="febb6-544">Applicable Attributes</span></span>

<span data-ttu-id="febb6-545">В следующей таблице описаны атрибуты, которые можно применить к элементу **MappingFragment** .</span><span class="sxs-lookup"><span data-stu-id="febb6-545">The following table describes the attributes that can be applied to the **MappingFragment** element.</span></span>

| <span data-ttu-id="febb6-546">Имя атрибута</span><span class="sxs-lookup"><span data-stu-id="febb6-546">Attribute Name</span></span>          | <span data-ttu-id="febb6-547">Обязательный</span><span class="sxs-lookup"><span data-stu-id="febb6-547">Is Required</span></span> | <span data-ttu-id="febb6-548">Значение</span><span class="sxs-lookup"><span data-stu-id="febb6-548">Value</span></span>                                                                                                                                                                                                                         |
|:------------------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="febb6-549">**сторинтитисет**</span><span class="sxs-lookup"><span data-stu-id="febb6-549">**StoreEntitySet**</span></span>      | <span data-ttu-id="febb6-550">Да</span><span class="sxs-lookup"><span data-stu-id="febb6-550">Yes</span></span>         | <span data-ttu-id="febb6-551">Имя столбца таблицы или представления, с которым производится сопоставление.</span><span class="sxs-lookup"><span data-stu-id="febb6-551">The name of the table or view that is being mapped.</span></span>                                                                                                                                                                           |
| <span data-ttu-id="febb6-552">**Атрибутом makecolumnsdistinct**</span><span class="sxs-lookup"><span data-stu-id="febb6-552">**MakeColumnsDistinct**</span></span> | <span data-ttu-id="febb6-553">нет</span><span class="sxs-lookup"><span data-stu-id="febb6-553">No</span></span>          | <span data-ttu-id="febb6-554">**Значение true** или **false** в зависимости от того, возвращаются ли только отдельные строки.</span><span class="sxs-lookup"><span data-stu-id="febb6-554">**True** or **False** depending on whether only distinct rows are returned.</span></span> <br/> <span data-ttu-id="febb6-555">Если для этого атрибута задано значение **true**, атрибуту **женератеупдатевиевс** элемента EntityContainerMapping должно быть присвоено значение **false**.</span><span class="sxs-lookup"><span data-stu-id="febb6-555">If this attribute is set to **True**, the **GenerateUpdateViews** attribute of the EntityContainerMapping element must be set to **False**.</span></span> |

### <a name="example"></a><span data-ttu-id="febb6-556">Пример</span><span class="sxs-lookup"><span data-stu-id="febb6-556">Example</span></span>

<span data-ttu-id="febb6-557">В следующем примере показан элемент **MappingFragment** в качестве дочернего элемента элемента **EntityTypeMapping** .</span><span class="sxs-lookup"><span data-stu-id="febb6-557">The following example shows a **MappingFragment** element as the child of an **EntityTypeMapping** element.</span></span> <span data-ttu-id="febb6-558">В этом примере свойства типа **курса** в концептуальной модели сопоставляются со столбцами таблицы **Course** в базе данных.</span><span class="sxs-lookup"><span data-stu-id="febb6-558">In this example, properties of the **Course** type in the conceptual model are mapped to columns of the **Course** table in the database.</span></span>

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

### <a name="example"></a><span data-ttu-id="febb6-559">Пример</span><span class="sxs-lookup"><span data-stu-id="febb6-559">Example</span></span>

<span data-ttu-id="febb6-560">В следующем примере показан элемент **MappingFragment** в качестве дочернего элемента элемента **EntitySetMapping** .</span><span class="sxs-lookup"><span data-stu-id="febb6-560">The following example shows a **MappingFragment** element as the child of an **EntitySetMapping** element.</span></span> <span data-ttu-id="febb6-561">Как и в приведенном выше примере, свойства типа **курса** в концептуальной модели сопоставляются со столбцами таблицы **Course** в базе данных.</span><span class="sxs-lookup"><span data-stu-id="febb6-561">As in the example above, properties of the **Course** type in the conceptual model are mapped to columns of the **Course** table in the database.</span></span>

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

## <a name="modificationfunctionmapping-element-msl"></a><span data-ttu-id="febb6-562">Элемент ModificationFunctionMapping (MSL)</span><span class="sxs-lookup"><span data-stu-id="febb6-562">ModificationFunctionMapping Element (MSL)</span></span>

<span data-ttu-id="febb6-563">Элемент **ModificationFunctionMapping** на языке MSL сопоставляет функции вставки, обновления и удаления типа сущности концептуальной модели с хранимыми процедурами в базовой базе данных.</span><span class="sxs-lookup"><span data-stu-id="febb6-563">The **ModificationFunctionMapping** element in mapping specification language (MSL) maps the insert, update, and delete functions of a conceptual model entity type to stored procedures in the underlying database.</span></span> <span data-ttu-id="febb6-564">Элемент **ModificationFunctionMapping** также может сопоставлять функции INSERT и DELETE для ассоциаций "многие ко многим" в концептуальной модели с хранимыми процедурами в базовой базе данных.</span><span class="sxs-lookup"><span data-stu-id="febb6-564">The **ModificationFunctionMapping** element can also map the insert and delete functions for many-to-many associations in the conceptual model to stored procedures in the underlying database.</span></span> <span data-ttu-id="febb6-565">Хранимые процедуры, с которыми сопоставляются функции изменения, должны объявляться в модели хранения.</span><span class="sxs-lookup"><span data-stu-id="febb6-565">Stored procedures to which modification functions are mapped must be declared in the storage model.</span></span> <span data-ttu-id="febb6-566">Дополнительные сведения см. в разделе элемент Function (SSDL).</span><span class="sxs-lookup"><span data-stu-id="febb6-566">For more information, see Function Element (SSDL).</span></span>

> [!NOTE]
> <span data-ttu-id="febb6-567">Если не выполнить сопоставление всех трех операций вставки, обновления или удаления типа сущности с хранимыми процедурами, несопоставленные операции завершатся ошибкой, если они выполняются во время выполнения и создается исключение Упдатиксцептион.</span><span class="sxs-lookup"><span data-stu-id="febb6-567">If you do not map all three of the insert, update, or delete operations of a entity type to stored procedures, the unmapped operations will fail if executed at runtime and an UpdateException is thrown.</span></span>


> [!NOTE]
> <span data-ttu-id="febb6-568">Если функции преобразования для одной сущности в иерархии наследования сопоставляются с хранимыми процедурами, то функции преобразования всех типов в иерархии должны быть сопоставлены с хранимыми процедурами.</span><span class="sxs-lookup"><span data-stu-id="febb6-568">If the modification functions for one entity in an inheritance hierarchy are mapped to stored procedures, then modification functions for all types in the hierarchy must be mapped to stored procedures.</span></span>

<span data-ttu-id="febb6-569">Элемент **ModificationFunctionMapping** может быть дочерним по отношению к элементу EntityTypeMapping или элементу AssociationSetMapping.</span><span class="sxs-lookup"><span data-stu-id="febb6-569">The **ModificationFunctionMapping** element can be a child of the EntityTypeMapping element or the AssociationSetMapping element.</span></span>

<span data-ttu-id="febb6-570">Элемент **ModificationFunctionMapping** может иметь следующие дочерние элементы:</span><span class="sxs-lookup"><span data-stu-id="febb6-570">The **ModificationFunctionMapping** element can have the following child elements:</span></span>

-   <span data-ttu-id="febb6-571">DeleteFunction (ноль или один)</span><span class="sxs-lookup"><span data-stu-id="febb6-571">DeleteFunction (zero or one)</span></span>
-   <span data-ttu-id="febb6-572">InsertFunction (ноль или один)</span><span class="sxs-lookup"><span data-stu-id="febb6-572">InsertFunction (zero or one)</span></span>
-   <span data-ttu-id="febb6-573">Упдатефунктион (ноль или один)</span><span class="sxs-lookup"><span data-stu-id="febb6-573">UpdateFunction (zero or one)</span></span>

<span data-ttu-id="febb6-574">К элементу **ModificationFunctionMapping** не применяются атрибуты.</span><span class="sxs-lookup"><span data-stu-id="febb6-574">No attributes are applicable to the **ModificationFunctionMapping** element.</span></span>

### <a name="example"></a><span data-ttu-id="febb6-575">Пример</span><span class="sxs-lookup"><span data-stu-id="febb6-575">Example</span></span>

<span data-ttu-id="febb6-576">В следующем примере показано сопоставление набора сущностей для набора сущностей « **люди** » в модели School.</span><span class="sxs-lookup"><span data-stu-id="febb6-576">The following example shows the entity set mapping for the **People** entity set in the School model.</span></span> <span data-ttu-id="febb6-577">В дополнение к сопоставлению столбцов для типа сущности **Person** отображаются сопоставления функций вставки, обновления и удаления типа **Person** .</span><span class="sxs-lookup"><span data-stu-id="febb6-577">In addition to the column mapping for the **Person** entity type, the mapping of the insert, update, and delete functions of the **Person** type are shown.</span></span> <span data-ttu-id="febb6-578">Функции, с которыми выполняется сопоставление, объявляются в модели хранения.</span><span class="sxs-lookup"><span data-stu-id="febb6-578">The functions that are mapped to are declared in the storage model.</span></span>

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

### <a name="example"></a><span data-ttu-id="febb6-579">Пример</span><span class="sxs-lookup"><span data-stu-id="febb6-579">Example</span></span>

<span data-ttu-id="febb6-580">В следующем примере показано сопоставление набора ассоциаций для набора ассоциаций **каурсеинструктор** в модели School.</span><span class="sxs-lookup"><span data-stu-id="febb6-580">The following example shows the association set mapping for the **CourseInstructor** association set in the School model.</span></span> <span data-ttu-id="febb6-581">Помимо сопоставления столбцов для ассоциации **каурсеинструктор** , отображаются сопоставления функций вставки и удаления ассоциации **каурсеинструктор** .</span><span class="sxs-lookup"><span data-stu-id="febb6-581">In addition to the column mapping for the **CourseInstructor** association, the mapping of the insert and delete functions of the **CourseInstructor** association are shown.</span></span> <span data-ttu-id="febb6-582">Функции, с которыми выполняется сопоставление, объявляются в модели хранения.</span><span class="sxs-lookup"><span data-stu-id="febb6-582">The functions that are mapped to are declared in the storage model.</span></span>

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
 

 

## <a name="queryview-element-msl"></a><span data-ttu-id="febb6-583">Элемент QueryView (MSL)</span><span class="sxs-lookup"><span data-stu-id="febb6-583">QueryView Element (MSL)</span></span>

<span data-ttu-id="febb6-584">Элемент **QueryView** на языке MSL определяет сопоставление только для чтения между типом сущности или ассоциацией в концептуальной модели и таблицей в базовой базе данных.</span><span class="sxs-lookup"><span data-stu-id="febb6-584">The **QueryView** element in mapping specification language (MSL) defines a read-only mapping between an entity type or association in the conceptual model and a table in the underlying database.</span></span> <span data-ttu-id="febb6-585">Сопоставление определяется с помощью Entity SQL запроса, который вычисляется для модели хранения, а результирующий набор выражается в терминах сущности или ассоциации в концептуальной модели.</span><span class="sxs-lookup"><span data-stu-id="febb6-585">The mapping is defined with an Entity SQL query that is evaluated against the storage model, and you express the result set in terms of an entity or association in the conceptual model.</span></span> <span data-ttu-id="febb6-586">Представления запросов доступны только для чтения, поэтому для обновления типов, которые определены представлениями запросов, не могут применяться стандартные команды обновления.</span><span class="sxs-lookup"><span data-stu-id="febb6-586">Because query views are read-only, you cannot use standard update commands to update types that are defined by query views.</span></span> <span data-ttu-id="febb6-587">Представления запросов доступны только для чтения, поэтому для обновления типов, которые определены представлениями запросов, не могут применяться стандартные команды обновления.</span><span class="sxs-lookup"><span data-stu-id="febb6-587">You can make updates to these types by using modification functions.</span></span> <span data-ttu-id="febb6-588">Дополнительные сведения см. в разделе инструкции. Сопоставьте функции изменения с хранимыми процедурами.</span><span class="sxs-lookup"><span data-stu-id="febb6-588">For more information, see How to: Map Modification Functions to Stored Procedures.</span></span>

> [!NOTE]
> <span data-ttu-id="febb6-589">В элементе **QueryView** Entity SQL выражения, содержащие **GroupBy**, групповые агрегаты или свойства навигации, не поддерживаются.</span><span class="sxs-lookup"><span data-stu-id="febb6-589">In the **QueryView** element, Entity SQL expressions that contain **GroupBy**, group aggregates, or navigation properties are not supported.</span></span>

 

<span data-ttu-id="febb6-590">Элемент **QueryView** может быть дочерним по отношению к элементу EntitySetMapping или элементу AssociationSetMapping.</span><span class="sxs-lookup"><span data-stu-id="febb6-590">The **QueryView** element can be a child of the EntitySetMapping element or the AssociationSetMapping element.</span></span> <span data-ttu-id="febb6-591">В первом случае представление запроса определяет сопоставление только для чтения для сущности в концептуальной модели.</span><span class="sxs-lookup"><span data-stu-id="febb6-591">In the former case, the query view defines a read-only mapping for an entity in the conceptual model.</span></span> <span data-ttu-id="febb6-592">Во втором случае представление запроса определяет сопоставление только для чтения для связи в концептуальной модели.</span><span class="sxs-lookup"><span data-stu-id="febb6-592">In the latter case, the query view defines a read-only mapping for an association in the conceptual model.</span></span>

> [!NOTE]
> <span data-ttu-id="febb6-593">Если элемент **AssociationSetMapping** предназначен для связи с ссылочным ограничением, элемент **AssociationSetMapping** игнорируется.</span><span class="sxs-lookup"><span data-stu-id="febb6-593">If the **AssociationSetMapping** element is for an association with a referential constraint, the **AssociationSetMapping** element is ignored.</span></span> <span data-ttu-id="febb6-594">Дополнительные сведения см. в разделе элемент ReferentialConstraint (CSDL).</span><span class="sxs-lookup"><span data-stu-id="febb6-594">For more information, see ReferentialConstraint Element (CSDL).</span></span>

<span data-ttu-id="febb6-595">Элемент **QueryView** не может иметь дочерних элементов.</span><span class="sxs-lookup"><span data-stu-id="febb6-595">The **QueryView** element cannot have any child elements.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="febb6-596">Применимые атрибуты</span><span class="sxs-lookup"><span data-stu-id="febb6-596">Applicable Attributes</span></span>

<span data-ttu-id="febb6-597">В следующей таблице описаны атрибуты, которые можно применить к элементу **QueryView** .</span><span class="sxs-lookup"><span data-stu-id="febb6-597">The following table describes the attributes that can be applied to the **QueryView** element.</span></span>

| <span data-ttu-id="febb6-598">Имя атрибута</span><span class="sxs-lookup"><span data-stu-id="febb6-598">Attribute Name</span></span> | <span data-ttu-id="febb6-599">Обязательный</span><span class="sxs-lookup"><span data-stu-id="febb6-599">Is Required</span></span> | <span data-ttu-id="febb6-600">Значение</span><span class="sxs-lookup"><span data-stu-id="febb6-600">Value</span></span>                                                                         |
|:---------------|:------------|:------------------------------------------------------------------------------|
| <span data-ttu-id="febb6-601">**Имени**</span><span class="sxs-lookup"><span data-stu-id="febb6-601">**TypeName**</span></span>   | <span data-ttu-id="febb6-602">нет</span><span class="sxs-lookup"><span data-stu-id="febb6-602">No</span></span>          | <span data-ttu-id="febb6-603">Имя типа концептуальной модели, сопоставляемой представлением запроса.</span><span class="sxs-lookup"><span data-stu-id="febb6-603">The name of the conceptual model type that is being mapped by the query view.</span></span> |

### <a name="example"></a><span data-ttu-id="febb6-604">Пример</span><span class="sxs-lookup"><span data-stu-id="febb6-604">Example</span></span>

<span data-ttu-id="febb6-605">В следующем примере показано, как элемент **QueryView** является дочерним элементом элемента **EntitySetMapping** и определяет сопоставление представления запроса для типа сущности « **Отдел** » в модели School.</span><span class="sxs-lookup"><span data-stu-id="febb6-605">The following example shows the **QueryView** element as a child of the **EntitySetMapping** element and defines a query view mapping for the **Department** entity type in the School Model.</span></span>

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

<span data-ttu-id="febb6-606">Поскольку запрос возвращает только подмножество элементов типа **отдела** в модели хранения, тип **отдела** в модели School был изменен на основе этого сопоставления следующим образом:</span><span class="sxs-lookup"><span data-stu-id="febb6-606">Because the query only returns a subset of the members of the **Department** type in the storage model, the **Department** type in the School model has been modified based on this mapping as follows:</span></span>

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

### <a name="example"></a><span data-ttu-id="febb6-607">Пример</span><span class="sxs-lookup"><span data-stu-id="febb6-607">Example</span></span>

<span data-ttu-id="febb6-608">В следующем примере показан элемент **QueryView** как дочерний элемент элемента **AssociationSetMapping** и определяется сопоставление только для чтения для `FK_Course_Department` ассоциации в модели School.</span><span class="sxs-lookup"><span data-stu-id="febb6-608">The next example shows the **QueryView** element as the child of an **AssociationSetMapping** element and defines a read-only mapping for the `FK_Course_Department` association in the School model.</span></span>

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
 
### <a name="comments"></a><span data-ttu-id="febb6-609">Комментарии</span><span class="sxs-lookup"><span data-stu-id="febb6-609">Comments</span></span>

<span data-ttu-id="febb6-610">Представления запросов можно определять для реализации следующих сценариев.</span><span class="sxs-lookup"><span data-stu-id="febb6-610">You can define query views to enable the following scenarios:</span></span>

-   <span data-ttu-id="febb6-611">Определение сущности в концептуальной модели, не включающей все свойства сущности в модели хранения.</span><span class="sxs-lookup"><span data-stu-id="febb6-611">Define an entity in the conceptual model that doesn't include all the properties of the entity in the storage model.</span></span> <span data-ttu-id="febb6-612">Сюда входят свойства, не имеющие значений по умолчанию и не поддерживающие значения **null** .</span><span class="sxs-lookup"><span data-stu-id="febb6-612">This includes properties that do not have default values and do not support **null** values.</span></span>
-   <span data-ttu-id="febb6-613">Сопоставление вычисляемых столбцов в модели хранения со свойствами типов сущностей в концептуальной модели.</span><span class="sxs-lookup"><span data-stu-id="febb6-613">Map computed columns in the storage model to properties of entity types in the conceptual model.</span></span>
-   <span data-ttu-id="febb6-614">Определение сопоставления, в котором условия, используемые для секционирования сущностей в концептуальной модели, не базируются на равенстве.</span><span class="sxs-lookup"><span data-stu-id="febb6-614">Define a mapping where conditions used to partition entities in the conceptual model are not based on equality.</span></span> <span data-ttu-id="febb6-615">При указании условного сопоставления с помощью элемента **Condition** указанное условие должно быть равно указанному значению.</span><span class="sxs-lookup"><span data-stu-id="febb6-615">When you specify a conditional mapping using the **Condition** element, the supplied condition must equal the specified value.</span></span> <span data-ttu-id="febb6-616">Дополнительные сведения см. в разделе элемент Condition (MSL).</span><span class="sxs-lookup"><span data-stu-id="febb6-616">For more information, see Condition Element (MSL).</span></span>
-   <span data-ttu-id="febb6-617">Сопоставление одного столбца в модели хранения с несколькими типами в концептуальной модели.</span><span class="sxs-lookup"><span data-stu-id="febb6-617">Map the same column in the storage model to multiple types in the conceptual model.</span></span>
-   <span data-ttu-id="febb6-618">Сопоставление нескольких типов с одной и той же таблицей.</span><span class="sxs-lookup"><span data-stu-id="febb6-618">Map multiple types to the same table.</span></span>
-   <span data-ttu-id="febb6-619">Определение ассоциаций в концептуальной модели, базирующихся на внешних ключах реляционной схемы.</span><span class="sxs-lookup"><span data-stu-id="febb6-619">Define associations in the conceptual model that are not based on foreign keys in the relational schema.</span></span>
-   <span data-ttu-id="febb6-620">Применение пользовательской бизнес-логики для задания значений свойств в концептуальной модели.</span><span class="sxs-lookup"><span data-stu-id="febb6-620">Use custom business logic to set the value of properties in the conceptual model.</span></span> <span data-ttu-id="febb6-621">Например, строковое значение «T» в источнике данных можно преобразовать в значение « **true**» (логическое) в концептуальной модели.</span><span class="sxs-lookup"><span data-stu-id="febb6-621">For example, you could map the string value "T" in the data source to a value of **true**, a Boolean, in the conceptual model.</span></span>
-   <span data-ttu-id="febb6-622">Определение условных фильтров для результатов запроса.</span><span class="sxs-lookup"><span data-stu-id="febb6-622">Define conditional filters for query results.</span></span>
-   <span data-ttu-id="febb6-623">Наложение меньшего числа ограничений на данные в концептуальной модели, нежели в модели хранения.</span><span class="sxs-lookup"><span data-stu-id="febb6-623">Enforce fewer restrictions on data in the conceptual model than in the storage model.</span></span> <span data-ttu-id="febb6-624">Например, можно сделать свойство в концептуальной модели обнуляемым, даже если столбец, с которым он сопоставлен, не поддерживает значения **null**.</span><span class="sxs-lookup"><span data-stu-id="febb6-624">For example, you could make a property in the conceptual model nullable even if the column to which it is mapped does not support **null**values.</span></span>

<span data-ttu-id="febb6-625">При определении представлений запросов для сущностей необходимо принимать во внимание следующие соображения.</span><span class="sxs-lookup"><span data-stu-id="febb6-625">The following considerations apply when you define query views for entities:</span></span>

-   <span data-ttu-id="febb6-626">Представления запросов доступны только для чтения.</span><span class="sxs-lookup"><span data-stu-id="febb6-626">Query views are read-only.</span></span> <span data-ttu-id="febb6-627">Обновление сущностей можно осуществлять только с помощью функций изменения.</span><span class="sxs-lookup"><span data-stu-id="febb6-627">You can only make updates to entities by using modification functions.</span></span>
-   <span data-ttu-id="febb6-628">В случае определения типа сущности через представление запроса необходимо также определять все связанные сущности через представления запросов.</span><span class="sxs-lookup"><span data-stu-id="febb6-628">When you define an entity type by a query view, you must also define all related entities by query views.</span></span>
-   <span data-ttu-id="febb6-629">При сопоставлении связи "многие ко многим" с сущностью в модели хранения, которая представляет связанную таблицу в реляционной схеме, необходимо определить элемент **QueryView** в элементе **AssociationSetMapping** для этой таблицы ссылок.</span><span class="sxs-lookup"><span data-stu-id="febb6-629">When you map a many-to-many association to an entity in the storage model that represents a link table in the relational schema, you must define a **QueryView** element in the **AssociationSetMapping** element for this link table.</span></span>
-   <span data-ttu-id="febb6-630">Представления запросов должны быть определены для всех типов в иерархии типов.</span><span class="sxs-lookup"><span data-stu-id="febb6-630">Query views must be defined for all types in a type hierarchy.</span></span> <span data-ttu-id="febb6-631">Это можно сделать следующими способами:</span><span class="sxs-lookup"><span data-stu-id="febb6-631">You can do this in the following ways:</span></span>
-   -   <span data-ttu-id="febb6-632">С одним элементом **QueryView** , указывающим один Entity SQL запрос, возвращающий объединение всех типов сущностей в иерархии.</span><span class="sxs-lookup"><span data-stu-id="febb6-632">With a single **QueryView** element that specifies a single Entity SQL query that returns a union of all of the entity types in the hierarchy.</span></span>
    -   <span data-ttu-id="febb6-633">С одним элементом **QueryView** , указывающим один Entity SQL запрос, использующий оператор Case для возврата определенного типа сущности в иерархии на основе определенного условия.</span><span class="sxs-lookup"><span data-stu-id="febb6-633">With a single **QueryView** element that specifies a single Entity SQL query that uses the CASE operator to return a specific entity type in the hierarchy based on a specific condition.</span></span>
    -   <span data-ttu-id="febb6-634">С дополнительным элементом **QueryView** для определенного типа в иерархии.</span><span class="sxs-lookup"><span data-stu-id="febb6-634">With an additional **QueryView** element for a specific type in the hierarchy.</span></span> <span data-ttu-id="febb6-635">В этом случае используйте атрибут **TypeName** элемента **QueryView** , чтобы указать тип сущности для каждого представления.</span><span class="sxs-lookup"><span data-stu-id="febb6-635">In this case, use the **TypeName** attribute of the **QueryView** element to specify the entity type for each view.</span></span>
-   <span data-ttu-id="febb6-636">Если задано представление запроса, нельзя указать атрибут **сторажесетнаме** в элементе **EntitySetMapping** .</span><span class="sxs-lookup"><span data-stu-id="febb6-636">When a query view is defined, you cannot specify the **StorageSetName** attribute on the **EntitySetMapping** element.</span></span>
-   <span data-ttu-id="febb6-637">Если определено представление запроса, элемент **EntitySetMapping**также не может содержать сопоставления **свойств** .</span><span class="sxs-lookup"><span data-stu-id="febb6-637">When a query view is defined, the **EntitySetMapping**element cannot also contain **Property** mappings.</span></span>

## <a name="resultbinding-element-msl"></a><span data-ttu-id="febb6-638">Элемент ResultBinding (язык MSL)</span><span class="sxs-lookup"><span data-stu-id="febb6-638">ResultBinding Element (MSL)</span></span>

<span data-ttu-id="febb6-639">Элемент **ResultBinding** в языке MSL сопоставляет значения столбцов, возвращаемые хранимыми процедурами, со свойствами сущностей в концептуальной модели, если функции изменения типов сущностей сопоставлены с хранимыми процедурами в базовой базе данных.</span><span class="sxs-lookup"><span data-stu-id="febb6-639">The **ResultBinding** element in mapping specification language (MSL) maps column values that are returned by stored procedures to entity properties in the conceptual model when entity type modification functions are mapped to stored procedures in the underlying database.</span></span> <span data-ttu-id="febb6-640">Например, если значение столбца идентификаторов возвращается хранимой процедурой INSERT, элемент **ResultBinding** сопоставляет возвращенное значение со свойством типа сущности в концептуальной модели.</span><span class="sxs-lookup"><span data-stu-id="febb6-640">For example, when the value of an identity column is returned by an insert stored procedure, the **ResultBinding** element maps the returned value to an entity type property in the conceptual model.</span></span>

<span data-ttu-id="febb6-641">Элемент **ResultBinding** может быть дочерним по отношению к элементу InsertFunction или элементу упдатефунктион.</span><span class="sxs-lookup"><span data-stu-id="febb6-641">The **ResultBinding** element can be child of the InsertFunction element or the UpdateFunction element.</span></span>

<span data-ttu-id="febb6-642">Элемент **ResultBinding** не может иметь дочерних элементов.</span><span class="sxs-lookup"><span data-stu-id="febb6-642">The **ResultBinding** element cannot have any child elements.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="febb6-643">Применимые атрибуты</span><span class="sxs-lookup"><span data-stu-id="febb6-643">Applicable Attributes</span></span>

<span data-ttu-id="febb6-644">В следующей таблице описаны атрибуты, применимые к элементу **ResultBinding** .</span><span class="sxs-lookup"><span data-stu-id="febb6-644">The following table describes the attributes that are applicable to the **ResultBinding** element:</span></span>

| <span data-ttu-id="febb6-645">Имя атрибута</span><span class="sxs-lookup"><span data-stu-id="febb6-645">Attribute Name</span></span> | <span data-ttu-id="febb6-646">Обязательный</span><span class="sxs-lookup"><span data-stu-id="febb6-646">Is Required</span></span> | <span data-ttu-id="febb6-647">Значение</span><span class="sxs-lookup"><span data-stu-id="febb6-647">Value</span></span>                                                                         |
|:---------------|:------------|:------------------------------------------------------------------------------|
| <span data-ttu-id="febb6-648">**Название**</span><span class="sxs-lookup"><span data-stu-id="febb6-648">**Name**</span></span>       | <span data-ttu-id="febb6-649">Да</span><span class="sxs-lookup"><span data-stu-id="febb6-649">Yes</span></span>         | <span data-ttu-id="febb6-650">Имя свойства сущности в концептуальной модели, с которым выполняется сопоставление.</span><span class="sxs-lookup"><span data-stu-id="febb6-650">The name of the entity property in the conceptual model that is being mapped.</span></span> |
| <span data-ttu-id="febb6-651">**ColumnName**</span><span class="sxs-lookup"><span data-stu-id="febb6-651">**ColumnName**</span></span> | <span data-ttu-id="febb6-652">Да</span><span class="sxs-lookup"><span data-stu-id="febb6-652">Yes</span></span>         | <span data-ttu-id="febb6-653">Имя столбца, с которым выполнятся сопоставление.</span><span class="sxs-lookup"><span data-stu-id="febb6-653">The name of the column being mapped.</span></span>                                          |

### <a name="example"></a><span data-ttu-id="febb6-654">Пример</span><span class="sxs-lookup"><span data-stu-id="febb6-654">Example</span></span>

<span data-ttu-id="febb6-655">Следующий пример основан на модели School и показывает элемент **InsertFunction** , используемый для преобразования функции вставки типа сущности **Person** в хранимую процедуру **инсертперсон** .</span><span class="sxs-lookup"><span data-stu-id="febb6-655">The following example is based on the School model and shows an **InsertFunction** element used to map the insert function of the **Person** entity type to the **InsertPerson** stored procedure.</span></span> <span data-ttu-id="febb6-656">(Хранимая процедура **инсертперсон** показана ниже и объявлена в модели хранения.) Элемент **ResultBinding** используется для отображения значения столбца, возвращаемого хранимой процедурой (**невперсонид**), в свойство типа сущности (**PersonID**).</span><span class="sxs-lookup"><span data-stu-id="febb6-656">(The **InsertPerson** stored procedure is shown below and is declared in the storage model.) A **ResultBinding** element is used to map a column value that is returned by the stored procedure (**NewPersonID**) to an entity type property (**PersonID**).</span></span>

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

<span data-ttu-id="febb6-657">Следующая инструкция Transact-SQL описывает хранимую процедуру **инсертперсон** :</span><span class="sxs-lookup"><span data-stu-id="febb6-657">The following Transact-SQL describes the **InsertPerson** stored procedure:</span></span>

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

## <a name="resultmapping-element-msl"></a><span data-ttu-id="febb6-658">Элемент ResultMapping (язык MSL)</span><span class="sxs-lookup"><span data-stu-id="febb6-658">ResultMapping Element (MSL)</span></span>

<span data-ttu-id="febb6-659">Элемент **ресултмаппинг** на языке MSL определяет сопоставление между импортом функции в концептуальной модели и хранимой процедурой в базовой базе данных, если выполняются следующие условия.</span><span class="sxs-lookup"><span data-stu-id="febb6-659">The **ResultMapping** element in mapping specification language (MSL) defines the mapping between a function import in the conceptual model and a stored procedure in the underlying database when the following are true:</span></span>

-   <span data-ttu-id="febb6-660">Импорт функции возвращает тип сущности концептуальной модели или сложный тип.</span><span class="sxs-lookup"><span data-stu-id="febb6-660">The function import returns a conceptual model entity type or complex type.</span></span>
-   <span data-ttu-id="febb6-661">Имена столбцов, возвращаемые хранимой процедурой, не совпадают в точности с именами свойств в сложном типе или типе сущности.</span><span class="sxs-lookup"><span data-stu-id="febb6-661">The names of the columns returned by the stored procedure do not exactly match the names of the properties on the entity type or complex type.</span></span>

<span data-ttu-id="febb6-662">По умолчанию сопоставление между столбцами, возвращаемыми хранимой процедурой, и сложным типом или типом сущности выполняется по именам столбцов и свойств.</span><span class="sxs-lookup"><span data-stu-id="febb6-662">By default, the mapping between the columns returned by a stored procedure and an entity type or complex type is based on column and property names.</span></span> <span data-ttu-id="febb6-663">Если имена столбцов не точно соответствуют именам свойств, для определения сопоставления необходимо использовать элемент **ресултмаппинг** .</span><span class="sxs-lookup"><span data-stu-id="febb6-663">If column names do not exactly match property names, you must use the **ResultMapping** element to define the mapping.</span></span> <span data-ttu-id="febb6-664">Пример сопоставления по умолчанию см. в разделе элемент FunctionImportMapping (MSL).</span><span class="sxs-lookup"><span data-stu-id="febb6-664">For an example of the default mapping, see FunctionImportMapping Element (MSL).</span></span>

<span data-ttu-id="febb6-665">Элемент **ресултмаппинг** является дочерним элементом элемента FunctionImportMapping.</span><span class="sxs-lookup"><span data-stu-id="febb6-665">The **ResultMapping** element is a child element of the FunctionImportMapping element.</span></span>

<span data-ttu-id="febb6-666">Элемент **ресултмаппинг** может иметь следующие дочерние элементы:</span><span class="sxs-lookup"><span data-stu-id="febb6-666">The **ResultMapping** element can have the following child elements:</span></span>

-   <span data-ttu-id="febb6-667">EntityTypeMapping (ноль и более)</span><span class="sxs-lookup"><span data-stu-id="febb6-667">EntityTypeMapping (zero or more)</span></span>
-   <span data-ttu-id="febb6-668">ComplexTypeMapping</span><span class="sxs-lookup"><span data-stu-id="febb6-668">ComplexTypeMapping</span></span>

<span data-ttu-id="febb6-669">К элементу **ресултмаппинг** не применяются атрибуты.</span><span class="sxs-lookup"><span data-stu-id="febb6-669">No attributes are applicable to the **ResultMapping** Element.</span></span>

### <a name="example"></a><span data-ttu-id="febb6-670">Пример</span><span class="sxs-lookup"><span data-stu-id="febb6-670">Example</span></span>

<span data-ttu-id="febb6-671">Рассмотрим следующую хранимую процедуру:</span><span class="sxs-lookup"><span data-stu-id="febb6-671">Consider the following stored procedure:</span></span>

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

<span data-ttu-id="febb6-672">Рассмотрим следующий тип сущности концептуальной модели:</span><span class="sxs-lookup"><span data-stu-id="febb6-672">Also consider the following conceptual model entity type:</span></span>

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

<span data-ttu-id="febb6-673">Чтобы создать импорт функции, возвращающий экземпляры предыдущего типа сущности, сопоставление столбцов, возвращаемых хранимой процедурой и типом сущности, должно быть определено в элементе **ресултмаппинг** :</span><span class="sxs-lookup"><span data-stu-id="febb6-673">In order to create a function import that returns instances of the previous entity type, the mapping between the columns returned by the stored procedure and the entity type must be defined in a **ResultMapping** element:</span></span>

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

## <a name="scalarproperty-element-msl"></a><span data-ttu-id="febb6-674">Элемент ScalarProperty (MSL)</span><span class="sxs-lookup"><span data-stu-id="febb6-674">ScalarProperty Element (MSL)</span></span>

<span data-ttu-id="febb6-675">Элемент **ScalarProperty** на языке MSL сопоставляет свойство типа сущности концептуальной модели, сложного типа или ассоциации со столбцом таблицы или параметром хранимой процедуры в базовой базе данных.</span><span class="sxs-lookup"><span data-stu-id="febb6-675">The **ScalarProperty** element in mapping specification language (MSL) maps a property on a conceptual model entity type, complex type, or association to a table column or stored procedure parameter in the underlying database.</span></span>

> [!NOTE]
> <span data-ttu-id="febb6-676">Хранимые процедуры, с которыми сопоставляются функции изменения, должны объявляться в модели хранения.</span><span class="sxs-lookup"><span data-stu-id="febb6-676">Stored procedures to which modification functions are mapped must be declared in the storage model.</span></span> <span data-ttu-id="febb6-677">Дополнительные сведения см. в разделе элемент Function (SSDL).</span><span class="sxs-lookup"><span data-stu-id="febb6-677">For more information, see Function Element (SSDL).</span></span>

<span data-ttu-id="febb6-678">Элемент **ScalarProperty** может быть дочерним для следующих элементов:</span><span class="sxs-lookup"><span data-stu-id="febb6-678">The **ScalarProperty** element can be a child of the following elements:</span></span>

-   <span data-ttu-id="febb6-679">MappingFragment</span><span class="sxs-lookup"><span data-stu-id="febb6-679">MappingFragment</span></span>
-   <span data-ttu-id="febb6-680">InsertFunction</span><span class="sxs-lookup"><span data-stu-id="febb6-680">InsertFunction</span></span>
-   <span data-ttu-id="febb6-681">UpdateFunction</span><span class="sxs-lookup"><span data-stu-id="febb6-681">UpdateFunction</span></span>
-   <span data-ttu-id="febb6-682">DeleteFunction</span><span class="sxs-lookup"><span data-stu-id="febb6-682">DeleteFunction</span></span>
-   <span data-ttu-id="febb6-683">EndProperty</span><span class="sxs-lookup"><span data-stu-id="febb6-683">EndProperty</span></span>
-   <span data-ttu-id="febb6-684">ComplexProperty</span><span class="sxs-lookup"><span data-stu-id="febb6-684">ComplexProperty</span></span>
-   <span data-ttu-id="febb6-685">ResultMapping</span><span class="sxs-lookup"><span data-stu-id="febb6-685">ResultMapping</span></span>

<span data-ttu-id="febb6-686">Элемент **ScalarProperty** , являющийся дочерним элементом элемента **MappingFragment**, **комплекспроперти**или **EndProperty** , сопоставляет свойство концептуальной модели со столбцом в базе данных.</span><span class="sxs-lookup"><span data-stu-id="febb6-686">As a child of the **MappingFragment**, **ComplexProperty**, or **EndProperty** element, the **ScalarProperty** element maps a property in the conceptual model to a column in the database.</span></span> <span data-ttu-id="febb6-687">Как дочерний элемент элемента **InsertFunction**, **упдатефунктион**или **DeleteFunction** , элемент **ScalarProperty** сопоставляет свойство концептуальной модели с параметром хранимой процедуры.</span><span class="sxs-lookup"><span data-stu-id="febb6-687">As a child of the **InsertFunction**, **UpdateFunction**, or **DeleteFunction** element, the **ScalarProperty** element maps a property in the conceptual model to a stored procedure parameter.</span></span>

<span data-ttu-id="febb6-688">Элемент **ScalarProperty** не может иметь дочерних элементов.</span><span class="sxs-lookup"><span data-stu-id="febb6-688">The **ScalarProperty** element cannot have any child elements.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="febb6-689">Применимые атрибуты</span><span class="sxs-lookup"><span data-stu-id="febb6-689">Applicable Attributes</span></span>

<span data-ttu-id="febb6-690">Атрибуты, применяемые к элементу **ScalarProperty** , различаются в зависимости от роли элемента.</span><span class="sxs-lookup"><span data-stu-id="febb6-690">The attributes that apply to the **ScalarProperty** element differ depending on the role of the element.</span></span>

<span data-ttu-id="febb6-691">В следующей таблице описаны атрибуты, применимые, если элемент **ScalarProperty** используется для отображения свойства концептуальной модели в столбце базы данных:</span><span class="sxs-lookup"><span data-stu-id="febb6-691">The following table describes the attributes that are applicable when the **ScalarProperty** element is used to map a conceptual model property to a column in the database:</span></span>

| <span data-ttu-id="febb6-692">Имя атрибута</span><span class="sxs-lookup"><span data-stu-id="febb6-692">Attribute Name</span></span> | <span data-ttu-id="febb6-693">Обязательный</span><span class="sxs-lookup"><span data-stu-id="febb6-693">Is Required</span></span> | <span data-ttu-id="febb6-694">Значение</span><span class="sxs-lookup"><span data-stu-id="febb6-694">Value</span></span>                                                           |
|:---------------|:------------|:----------------------------------------------------------------|
| <span data-ttu-id="febb6-695">**Название**</span><span class="sxs-lookup"><span data-stu-id="febb6-695">**Name**</span></span>       | <span data-ttu-id="febb6-696">Да</span><span class="sxs-lookup"><span data-stu-id="febb6-696">Yes</span></span>         | <span data-ttu-id="febb6-697">Имя свойства концептуальной модели, с которым выполняется сопоставление.</span><span class="sxs-lookup"><span data-stu-id="febb6-697">The name of the conceptual model property that is being mapped.</span></span> |
| <span data-ttu-id="febb6-698">**ColumnName**</span><span class="sxs-lookup"><span data-stu-id="febb6-698">**ColumnName**</span></span> | <span data-ttu-id="febb6-699">Да</span><span class="sxs-lookup"><span data-stu-id="febb6-699">Yes</span></span>         | <span data-ttu-id="febb6-700">Имя столбца таблицы, с которым производится сопоставление.</span><span class="sxs-lookup"><span data-stu-id="febb6-700">The name of the table column that is being mapped.</span></span>              |

<span data-ttu-id="febb6-701">В следующей таблице описаны атрибуты, применимые к элементу **ScalarProperty** , когда он используется для отображения свойства концептуальной модели в параметре хранимой процедуры.</span><span class="sxs-lookup"><span data-stu-id="febb6-701">The following table describes the attributes that are applicable to the **ScalarProperty** element when it is used to map a conceptual model property to a stored procedure parameter:</span></span>

| <span data-ttu-id="febb6-702">Имя атрибута</span><span class="sxs-lookup"><span data-stu-id="febb6-702">Attribute Name</span></span>    | <span data-ttu-id="febb6-703">Обязательный</span><span class="sxs-lookup"><span data-stu-id="febb6-703">Is Required</span></span> | <span data-ttu-id="febb6-704">Значение</span><span class="sxs-lookup"><span data-stu-id="febb6-704">Value</span></span>                                                                                                                                           |
|:------------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="febb6-705">**Название**</span><span class="sxs-lookup"><span data-stu-id="febb6-705">**Name**</span></span>          | <span data-ttu-id="febb6-706">Да</span><span class="sxs-lookup"><span data-stu-id="febb6-706">Yes</span></span>         | <span data-ttu-id="febb6-707">Имя свойства концептуальной модели, с которым выполняется сопоставление.</span><span class="sxs-lookup"><span data-stu-id="febb6-707">The name of the conceptual model property that is being mapped.</span></span>                                                                                 |
| <span data-ttu-id="febb6-708">**ParameterName**</span><span class="sxs-lookup"><span data-stu-id="febb6-708">**ParameterName**</span></span> | <span data-ttu-id="febb6-709">Да</span><span class="sxs-lookup"><span data-stu-id="febb6-709">Yes</span></span>         | <span data-ttu-id="febb6-710">Имя сопоставляемого параметра.</span><span class="sxs-lookup"><span data-stu-id="febb6-710">The name of the parameter that is being mapped.</span></span>                                                                                                 |
| <span data-ttu-id="febb6-711">**Версия**</span><span class="sxs-lookup"><span data-stu-id="febb6-711">**Version**</span></span>       | <span data-ttu-id="febb6-712">нет</span><span class="sxs-lookup"><span data-stu-id="febb6-712">No</span></span>          | <span data-ttu-id="febb6-713">**Current** или **Original** в зависимости от того, следует ли использовать текущее значение или исходное значение свойства для проверок параллелизма.</span><span class="sxs-lookup"><span data-stu-id="febb6-713">**Current** or **Original** depending on whether the current value or the original value of the property should be used for concurrency checks.</span></span> |

### <a name="example"></a><span data-ttu-id="febb6-714">Пример</span><span class="sxs-lookup"><span data-stu-id="febb6-714">Example</span></span>

<span data-ttu-id="febb6-715">В следующем примере показан элемент **ScalarProperty** , используемый двумя способами:</span><span class="sxs-lookup"><span data-stu-id="febb6-715">The following example shows the **ScalarProperty** element used in two ways:</span></span>

-   <span data-ttu-id="febb6-716">Для отображения свойств типа сущности **Person** в столбцах таблицы **Person**.</span><span class="sxs-lookup"><span data-stu-id="febb6-716">To map the properties of the **Person** entity type to the columns of the **Person**table.</span></span>
-   <span data-ttu-id="febb6-717">Чтобы связать свойства типа сущности **Person** с параметрами хранимой процедуры **упдатеперсон** .</span><span class="sxs-lookup"><span data-stu-id="febb6-717">To map the properties of the **Person** entity type to the parameters of the **UpdatePerson** stored procedure.</span></span> <span data-ttu-id="febb6-718">Хранимые процедуры объявляются в модели хранения.</span><span class="sxs-lookup"><span data-stu-id="febb6-718">The stored procedures are declared in the storage model.</span></span>

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

### <a name="example"></a><span data-ttu-id="febb6-719">Пример</span><span class="sxs-lookup"><span data-stu-id="febb6-719">Example</span></span>

<span data-ttu-id="febb6-720">В следующем примере показан элемент **ScalarProperty** , используемый для сопоставления функций вставки и удаления ассоциации концептуальной модели с хранимыми процедурами в базе данных.</span><span class="sxs-lookup"><span data-stu-id="febb6-720">The next example shows the **ScalarProperty** element used to map the insert and delete functions of a conceptual model association to stored procedures in the database.</span></span> <span data-ttu-id="febb6-721">Хранимые процедуры объявляются в модели хранения.</span><span class="sxs-lookup"><span data-stu-id="febb6-721">The stored procedures are declared in the storage model.</span></span>

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

## <a name="updatefunction-element-msl"></a><span data-ttu-id="febb6-722">Элемент UpdateFunction (MSL)</span><span class="sxs-lookup"><span data-stu-id="febb6-722">UpdateFunction Element (MSL)</span></span>

<span data-ttu-id="febb6-723">Элемент **упдатефунктион** на языке MSL сопоставляет функцию Update типа сущности в концептуальной модели с хранимой процедурой в базовой базе данных.</span><span class="sxs-lookup"><span data-stu-id="febb6-723">The **UpdateFunction** element in mapping specification language (MSL) maps the update function of an entity type in the conceptual model to a stored procedure in the underlying database.</span></span> <span data-ttu-id="febb6-724">Хранимые процедуры, с которыми сопоставляются функции изменения, должны объявляться в модели хранения.</span><span class="sxs-lookup"><span data-stu-id="febb6-724">Stored procedures to which modification functions are mapped must be declared in the storage model.</span></span> <span data-ttu-id="febb6-725">Дополнительные сведения см. в разделе элемент Function (SSDL).</span><span class="sxs-lookup"><span data-stu-id="febb6-725">For more information, see Function Element (SSDL).</span></span>

> [!NOTE]
>  <span data-ttu-id="febb6-726">Если не выполнить сопоставление всех трех операций вставки, обновления или удаления типа сущности с хранимыми процедурами, несопоставленные операции завершатся ошибкой, если они выполняются во время выполнения и создается исключение Упдатиксцептион.</span><span class="sxs-lookup"><span data-stu-id="febb6-726">If you do not map all three of the insert, update, or delete operations of a entity type to stored procedures, the unmapped operations will fail if executed at runtime and an UpdateException is thrown.</span></span>

<span data-ttu-id="febb6-727">Элемент **упдатефунктион** может быть дочерним элементом элемента ModificationFunctionMapping и применен к элементу EntityTypeMapping.</span><span class="sxs-lookup"><span data-stu-id="febb6-727">The **UpdateFunction** element can be a child of the ModificationFunctionMapping element and applied to the EntityTypeMapping element.</span></span>

<span data-ttu-id="febb6-728">Элемент **упдатефунктион** может иметь следующие дочерние элементы:</span><span class="sxs-lookup"><span data-stu-id="febb6-728">The **UpdateFunction** element can have the following child elements:</span></span>

-   <span data-ttu-id="febb6-729">AssociationEnd (ноль или более)</span><span class="sxs-lookup"><span data-stu-id="febb6-729">AssociationEnd (zero or more)</span></span>
-   <span data-ttu-id="febb6-730">ComplexProperty (ноль и более)</span><span class="sxs-lookup"><span data-stu-id="febb6-730">ComplexProperty (zero or more)</span></span>
-   <span data-ttu-id="febb6-731">ResultBinding (ноль или один)</span><span class="sxs-lookup"><span data-stu-id="febb6-731">ResultBinding (zero or one)</span></span>
-   <span data-ttu-id="febb6-732">Скарларпроперти (ноль или более)</span><span class="sxs-lookup"><span data-stu-id="febb6-732">ScarlarProperty (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="febb6-733">Применимые атрибуты</span><span class="sxs-lookup"><span data-stu-id="febb6-733">Applicable Attributes</span></span>

<span data-ttu-id="febb6-734">В следующей таблице описаны атрибуты, которые можно применить к элементу **упдатефунктион** .</span><span class="sxs-lookup"><span data-stu-id="febb6-734">The following table describes the attributes that can be applied to the **UpdateFunction** element.</span></span>

| <span data-ttu-id="febb6-735">Имя атрибута</span><span class="sxs-lookup"><span data-stu-id="febb6-735">Attribute Name</span></span>            | <span data-ttu-id="febb6-736">Обязательный</span><span class="sxs-lookup"><span data-stu-id="febb6-736">Is Required</span></span> | <span data-ttu-id="febb6-737">Значение</span><span class="sxs-lookup"><span data-stu-id="febb6-737">Value</span></span>                                                                                                                                                    |
|:--------------------------|:------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="febb6-738">**FunctionName**</span><span class="sxs-lookup"><span data-stu-id="febb6-738">**FunctionName**</span></span>          | <span data-ttu-id="febb6-739">Да</span><span class="sxs-lookup"><span data-stu-id="febb6-739">Yes</span></span>         | <span data-ttu-id="febb6-740">Имя хранимой процедуры (с указанием пространства имен), с которой сопоставляется функция обновления.</span><span class="sxs-lookup"><span data-stu-id="febb6-740">The namespace-qualified name of the stored procedure to which the update function is mapped.</span></span> <span data-ttu-id="febb6-741">Хранимая процедура должна объявляться в модели хранения.</span><span class="sxs-lookup"><span data-stu-id="febb6-741">The stored procedure must be declared in the storage model.</span></span> |
| <span data-ttu-id="febb6-742">**ровсаффектедпараметер**</span><span class="sxs-lookup"><span data-stu-id="febb6-742">**RowsAffectedParameter**</span></span> | <span data-ttu-id="febb6-743">нет</span><span class="sxs-lookup"><span data-stu-id="febb6-743">No</span></span>          | <span data-ttu-id="febb6-744">Имя выходного параметра, возвращающего количество обработанных строк.</span><span class="sxs-lookup"><span data-stu-id="febb6-744">The name of the output parameter that returns the number of rows affected.</span></span>                                                                               |

### <a name="example"></a><span data-ttu-id="febb6-745">Пример</span><span class="sxs-lookup"><span data-stu-id="febb6-745">Example</span></span>

<span data-ttu-id="febb6-746">Следующий пример основан на модели School и показывает элемент **упдатефунктион** , используемый для преобразования функции обновления типа сущности **Person** в хранимую процедуру **упдатеперсон** .</span><span class="sxs-lookup"><span data-stu-id="febb6-746">The following example is based on the School model and shows the **UpdateFunction** element used to map update function of the **Person** entity type to the **UpdatePerson** stored procedure.</span></span> <span data-ttu-id="febb6-747">Хранимая процедура **упдатеперсон** объявляется в модели хранения.</span><span class="sxs-lookup"><span data-stu-id="febb6-747">The **UpdatePerson** stored procedure is declared in the storage model.</span></span>

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
