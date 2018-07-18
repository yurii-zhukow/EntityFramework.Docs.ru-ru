---
title: Спецификация MSL - EF6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: 13ae7bc1-74b4-4ee4-8d73-c337be841467
caps.latest.revision: 4
ms.openlocfilehash: 7448efc99f9fd9c6cdf930256a26347376fb354c
ms.sourcegitcommit: f05e7b62584cf228f17390bb086a61d505712e1b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/08/2018
ms.locfileid: "39122337"
---
# <a name="msl-specification"></a><span data-ttu-id="a9957-102">Спецификация MSL</span><span class="sxs-lookup"><span data-stu-id="a9957-102">MSL Specification</span></span>
<span data-ttu-id="a9957-103">Язык определения сопоставлений (MSL) — это язык на основе XML, описывающий сопоставление между концептуальной моделью и моделью хранения приложения Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="a9957-103">Mapping specification language (MSL) is an XML-based language that describes the mapping between the conceptual model and storage model of an Entity Framework application.</span></span>

<span data-ttu-id="a9957-104">В приложении Entity Framework метаданные сопоставления загружается из MSL-файла (написанного на языке MSL) во время сборки.</span><span class="sxs-lookup"><span data-stu-id="a9957-104">In an Entity Framework application, mapping metadata is loaded from an .msl file (written in MSL) at build time.</span></span> <span data-ttu-id="a9957-105">Платформа Entity Framework метаданные сопоставления используются во время выполнения для преобразования запросов к концептуальной модели для конкретного хранилища команды.</span><span class="sxs-lookup"><span data-stu-id="a9957-105">Entity Framework uses mapping metadata at runtime to translate queries against the conceptual model to store-specific commands.</span></span>

<span data-ttu-id="a9957-106">Entity Framework Designer (конструктор EF) хранит сведения о сопоставлении в EDMX-файла во время разработки.</span><span class="sxs-lookup"><span data-stu-id="a9957-106">The Entity Framework Designer (EF Designer) stores mapping information in an .edmx file at design time.</span></span> <span data-ttu-id="a9957-107">Во время построения конструктор сущностей использует сведения в EDMX-файла, создаваемого MSL-файл, необходимый Entity Framework во время выполнения</span><span class="sxs-lookup"><span data-stu-id="a9957-107">At build time, the Entity Designer uses information in an .edmx file to create the .msl file that is needed by Entity Framework at runtime</span></span>

<span data-ttu-id="a9957-108">Имена всех типов концептуальной модели или модели хранения, которые упоминаются в языке MSL, должны указываться вместе с именами соответствующих пространств имен.</span><span class="sxs-lookup"><span data-stu-id="a9957-108">Names of all conceptual or storage model types that are referenced in MSL must be qualified by their respective namespace names.</span></span> <span data-ttu-id="a9957-109">Сведения об имени пространства имен концептуальной модели, см. в разделе [спецификация языка CSDL](~/ef6/modeling/designer/advanced/edmx/csdl-spec.md).</span><span class="sxs-lookup"><span data-stu-id="a9957-109">For information about the conceptual model namespace name, see [CSDL Specification](~/ef6/modeling/designer/advanced/edmx/csdl-spec.md).</span></span> <span data-ttu-id="a9957-110">Сведения об имени пространства имен модели хранения, см. в разделе [спецификация SSDL](~/ef6/modeling/designer/advanced/edmx/ssdl-spec.md).</span><span class="sxs-lookup"><span data-stu-id="a9957-110">For information about the storage model namespace name, see [SSDL Specification](~/ef6/modeling/designer/advanced/edmx/ssdl-spec.md).</span></span>

<span data-ttu-id="a9957-111">Версии MSL различаются по пространствам имен XML.</span><span class="sxs-lookup"><span data-stu-id="a9957-111">Versions of MSL are differentiated by XML namespaces.</span></span>

| <span data-ttu-id="a9957-112">Версия MSL</span><span class="sxs-lookup"><span data-stu-id="a9957-112">MSL Version</span></span> | <span data-ttu-id="a9957-113">Пространство имен XML</span><span class="sxs-lookup"><span data-stu-id="a9957-113">XML Namespace</span></span>                                        |
|:------------|:-----------------------------------------------------|
| <span data-ttu-id="a9957-114">Язык MSL v1</span><span class="sxs-lookup"><span data-stu-id="a9957-114">MSL v1</span></span>      | <span data-ttu-id="a9957-115">urn: schemas-microsoft-com:windows:storage:mapping:CS</span><span class="sxs-lookup"><span data-stu-id="a9957-115">urn:schemas-microsoft-com:windows:storage:mapping:CS</span></span> |
| <span data-ttu-id="a9957-116">Язык MSL v2</span><span class="sxs-lookup"><span data-stu-id="a9957-116">MSL v2</span></span>      | http://schemas.microsoft.com/ado/2008/09/mapping/cs  |
| <span data-ttu-id="a9957-117">Язык MSL v3</span><span class="sxs-lookup"><span data-stu-id="a9957-117">MSL v3</span></span>      | http://schemas.microsoft.com/ado/2009/11/mapping/cs  |

## <a name="alias-element-msl"></a><span data-ttu-id="a9957-118">Элемент Alias (язык MSL)</span><span class="sxs-lookup"><span data-stu-id="a9957-118">Alias Element (MSL)</span></span>

<span data-ttu-id="a9957-119">**Псевдоним** элемент в язык определения сопоставлений (MSL) — это дочерний элемент элемента сопоставления, который используется для определения псевдонимов для концептуальной модели и хранения пространства имен модели.</span><span class="sxs-lookup"><span data-stu-id="a9957-119">The **Alias** element in mapping specification language (MSL) is a child of the Mapping element that is used to define aliases for conceptual model and storage model namespaces.</span></span> <span data-ttu-id="a9957-120">Имена всех типов концептуальной модели или модели хранения, которые упоминаются в языке MSL, должны указываться вместе с именами соответствующих пространств имен.</span><span class="sxs-lookup"><span data-stu-id="a9957-120">Names of all conceptual or storage model types that are referenced in MSL must be qualified by their respective namespace names.</span></span> <span data-ttu-id="a9957-121">Сведения об имени пространства имен концептуальной модели см. в разделе элемент схемы (CSDL).</span><span class="sxs-lookup"><span data-stu-id="a9957-121">For information about the conceptual model namespace name, see Schema Element (CSDL).</span></span> <span data-ttu-id="a9957-122">Сведения об имени пространства имен модели хранения см. в разделе элемент схемы (SSDL).</span><span class="sxs-lookup"><span data-stu-id="a9957-122">For information about the storage model namespace name, see Schema Element (SSDL).</span></span>

<span data-ttu-id="a9957-123">**Псевдоним** элемент не может иметь дочерние элементы.</span><span class="sxs-lookup"><span data-stu-id="a9957-123">The **Alias** element cannot have child elements.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="a9957-124">Применимые атрибуты</span><span class="sxs-lookup"><span data-stu-id="a9957-124">Applicable Attributes</span></span>

<span data-ttu-id="a9957-125">В следующей таблице описаны атрибуты, которые могут применяться к **псевдоним** элемент.</span><span class="sxs-lookup"><span data-stu-id="a9957-125">The table below describes the attributes that can be applied to the **Alias** element.</span></span>

| <span data-ttu-id="a9957-126">Имя атрибута</span><span class="sxs-lookup"><span data-stu-id="a9957-126">Attribute Name</span></span> | <span data-ttu-id="a9957-127">Необходимо</span><span class="sxs-lookup"><span data-stu-id="a9957-127">Is Required</span></span> | <span data-ttu-id="a9957-128">Значение</span><span class="sxs-lookup"><span data-stu-id="a9957-128">Value</span></span>                                                                     |
|:---------------|:------------|:--------------------------------------------------------------------------|
| <span data-ttu-id="a9957-129">**Key**</span><span class="sxs-lookup"><span data-stu-id="a9957-129">**Key**</span></span>        | <span data-ttu-id="a9957-130">Да</span><span class="sxs-lookup"><span data-stu-id="a9957-130">Yes</span></span>         | <span data-ttu-id="a9957-131">Псевдоним для пространства имен, который задается параметром **значение** атрибута.</span><span class="sxs-lookup"><span data-stu-id="a9957-131">The alias for the namespace that is specified by the **Value** attribute.</span></span> |
| <span data-ttu-id="a9957-132">**Значение**</span><span class="sxs-lookup"><span data-stu-id="a9957-132">**Value**</span></span>      | <span data-ttu-id="a9957-133">Да</span><span class="sxs-lookup"><span data-stu-id="a9957-133">Yes</span></span>         | <span data-ttu-id="a9957-134">Пространство имен, для которого значение **ключ** элемент является псевдонимом.</span><span class="sxs-lookup"><span data-stu-id="a9957-134">The namespace for which the value of the **Key** element is an alias.</span></span>     |

### <a name="example"></a><span data-ttu-id="a9957-135">Пример</span><span class="sxs-lookup"><span data-stu-id="a9957-135">Example</span></span>

<span data-ttu-id="a9957-136">В следующем примере показан **псевдоним** элемент, который определяет псевдоним, `c`, для типов, определенных в концептуальной модели.</span><span class="sxs-lookup"><span data-stu-id="a9957-136">The following example shows an **Alias** element that defines an alias, `c`, for types that are defined in the conceptual model.</span></span>

``` xml
 <Mapping Space="C-S"
          xmlns="http://schemas.microsoft.com/ado/2009/11/mapping/cs">
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

## <a name="associationend-element-msl"></a><span data-ttu-id="a9957-137">Элемент AssociationEnd (MSL)</span><span class="sxs-lookup"><span data-stu-id="a9957-137">AssociationEnd Element (MSL)</span></span>

<span data-ttu-id="a9957-138">**AssociationEnd** элемент в язык определения сопоставлений (MSL) используется при сопоставлении функций изменения типа сущности в концептуальной модели с хранимыми процедурами в основной базе данных.</span><span class="sxs-lookup"><span data-stu-id="a9957-138">The **AssociationEnd** element in mapping specification language (MSL) is used when the modification functions of an entity type in the conceptual model are mapped to stored procedures in the underlying database.</span></span> <span data-ttu-id="a9957-139">Если хранимая процедура изменения принимает параметр, значение которого содержится в свойстве ассоциации, **AssociationEnd** сопоставляет значение свойства параметра.</span><span class="sxs-lookup"><span data-stu-id="a9957-139">If a modification stored procedure takes a parameter whose value is held in an association property, the **AssociationEnd** element maps the property value to the parameter.</span></span> <span data-ttu-id="a9957-140">Дополнительные сведения см. в приведенных ниже примерах.</span><span class="sxs-lookup"><span data-stu-id="a9957-140">For more information, see the example below.</span></span>

<span data-ttu-id="a9957-141">Дополнительные сведения о сопоставлении функций изменения типов сущностей с хранимыми процедурами см. в разделе элемент ModificationFunctionMapping (MSL) и пошаговое руководство: сопоставление сущности с хранимых процедур.</span><span class="sxs-lookup"><span data-stu-id="a9957-141">For more information about mapping modification functions of entity types to stored procedures, see ModificationFunctionMapping Element (MSL) and Walkthrough: Mapping an Entity to Stored Procedures.</span></span>

<span data-ttu-id="a9957-142">**AssociationEnd** элемент может иметь следующие дочерние элементы:</span><span class="sxs-lookup"><span data-stu-id="a9957-142">The **AssociationEnd** element can have the following child elements:</span></span>

-   <span data-ttu-id="a9957-143">ScalarProperty</span><span class="sxs-lookup"><span data-stu-id="a9957-143">ScalarProperty</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="a9957-144">Применимые атрибуты</span><span class="sxs-lookup"><span data-stu-id="a9957-144">Applicable Attributes</span></span>

<span data-ttu-id="a9957-145">В следующей таблице описаны атрибуты, применимые к **AssociationEnd** элемент.</span><span class="sxs-lookup"><span data-stu-id="a9957-145">The following table describes the attributes that are applicable to the **AssociationEnd** element.</span></span>

| <span data-ttu-id="a9957-146">Имя атрибута</span><span class="sxs-lookup"><span data-stu-id="a9957-146">Attribute Name</span></span>     | <span data-ttu-id="a9957-147">Необходимо</span><span class="sxs-lookup"><span data-stu-id="a9957-147">Is Required</span></span> | <span data-ttu-id="a9957-148">Значение</span><span class="sxs-lookup"><span data-stu-id="a9957-148">Value</span></span>                                                                                                                                                                             |
|:-------------------|:------------|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="a9957-149">**AssociationSet**</span><span class="sxs-lookup"><span data-stu-id="a9957-149">**AssociationSet**</span></span> | <span data-ttu-id="a9957-150">Да</span><span class="sxs-lookup"><span data-stu-id="a9957-150">Yes</span></span>         | <span data-ttu-id="a9957-151">Имя сопоставляемой ассоциации.</span><span class="sxs-lookup"><span data-stu-id="a9957-151">The name of the association that is being mapped.</span></span>                                                                                                                                 |
| <span data-ttu-id="a9957-152">**From**</span><span class="sxs-lookup"><span data-stu-id="a9957-152">**From**</span></span>           | <span data-ttu-id="a9957-153">Да</span><span class="sxs-lookup"><span data-stu-id="a9957-153">Yes</span></span>         | <span data-ttu-id="a9957-154">Значение **FromRole** атрибут свойства навигации, которое соответствует сопоставляемой ассоциации.</span><span class="sxs-lookup"><span data-stu-id="a9957-154">The value of the **FromRole** attribute of the navigation property that corresponds to the association being mapped.</span></span> <span data-ttu-id="a9957-155">Дополнительные сведения см. в разделе элемент NavigationProperty (CSDL).</span><span class="sxs-lookup"><span data-stu-id="a9957-155">For more information, see NavigationProperty Element (CSDL).</span></span> |
| <span data-ttu-id="a9957-156">**Задача**</span><span class="sxs-lookup"><span data-stu-id="a9957-156">**To**</span></span>             | <span data-ttu-id="a9957-157">Да</span><span class="sxs-lookup"><span data-stu-id="a9957-157">Yes</span></span>         | <span data-ttu-id="a9957-158">Значение **ToRole** атрибут свойства навигации, которое соответствует сопоставляемой ассоциации.</span><span class="sxs-lookup"><span data-stu-id="a9957-158">The value of the **ToRole** attribute of the navigation property that corresponds to the association being mapped.</span></span> <span data-ttu-id="a9957-159">Дополнительные сведения см. в разделе элемент NavigationProperty (CSDL).</span><span class="sxs-lookup"><span data-stu-id="a9957-159">For more information, see NavigationProperty Element (CSDL).</span></span>   |

### <a name="example"></a><span data-ttu-id="a9957-160">Пример</span><span class="sxs-lookup"><span data-stu-id="a9957-160">Example</span></span>

<span data-ttu-id="a9957-161">Рассмотрим следующий тип сущности в концептуальной модели:</span><span class="sxs-lookup"><span data-stu-id="a9957-161">Consider the following conceptual model entity type:</span></span>

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

<span data-ttu-id="a9957-162">Предположим, имеется следующая хранимая процедура:</span><span class="sxs-lookup"><span data-stu-id="a9957-162">Also consider the following stored procedure:</span></span>

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

<span data-ttu-id="a9957-163">Чтобы сопоставить функцию обновления `Course` сущности в хранимую процедуру, необходимо указать значение для **DepartmentID** параметра.</span><span class="sxs-lookup"><span data-stu-id="a9957-163">In order to map the update function of the `Course` entity to this stored procedure, you must supply a value to the **DepartmentID** parameter.</span></span> <span data-ttu-id="a9957-164">Значение для `DepartmentID` не соответствует свойству типа сущности. Оно содержится в независимом сопоставлении с сопоставлением, показанным ниже:</span><span class="sxs-lookup"><span data-stu-id="a9957-164">The value for `DepartmentID` does not correspond to a property on the entity type; it is contained in an independent association whose mapping is shown here:</span></span>

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

<span data-ttu-id="a9957-165">В следующем коде показан **AssociationEnd** элемент, используемый для сопоставления **DepartmentID** свойство **FK\_курс\_отдел** связь с **UpdateCourse** хранимой процедуры (к которому функция обновления **курс** тип сущности сопоставлен):</span><span class="sxs-lookup"><span data-stu-id="a9957-165">The following code shows the **AssociationEnd** element used to map the **DepartmentID** property of the **FK\_Course\_Department** association to the **UpdateCourse** stored procedure (to which the update function of the **Course** entity type is mapped):</span></span>

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

## <a name="associationsetmapping-element-msl"></a><span data-ttu-id="a9957-166">Элемент AssociationSetMapping (MSL)</span><span class="sxs-lookup"><span data-stu-id="a9957-166">AssociationSetMapping Element (MSL)</span></span>

<span data-ttu-id="a9957-167">**AssociationSetMapping** элемент в язык определения сопоставлений (MSL) определяет сопоставление между ассоциацией в концептуальной модели и столбцами таблицы в основной базе данных.</span><span class="sxs-lookup"><span data-stu-id="a9957-167">The **AssociationSetMapping** element in mapping specification language (MSL) defines the mapping between an association in the conceptual model and table columns in the underlying database.</span></span>

<span data-ttu-id="a9957-168">Сопоставления в концептуальной модели — это типы, свойства которых представляют столбцы первичного и внешнего ключа в основной базе данных.</span><span class="sxs-lookup"><span data-stu-id="a9957-168">Associations in the conceptual model are types whose properties represent primary and foreign key columns in the underlying database.</span></span> <span data-ttu-id="a9957-169">**AssociationSetMapping** элемент использует два элемента EndProperty, чтобы определить сопоставления между свойствами типов сопоставлений и столбцами в базе данных.</span><span class="sxs-lookup"><span data-stu-id="a9957-169">The **AssociationSetMapping** element uses two EndProperty elements to define the mappings between association type properties and columns in the database.</span></span> <span data-ttu-id="a9957-170">Можно определить условия сопоставления в элемент Condition.</span><span class="sxs-lookup"><span data-stu-id="a9957-170">You can place conditions on these mappings with the Condition element.</span></span> <span data-ttu-id="a9957-171">Сопоставление insert, update и delete функции для ассоциаций с хранимыми процедурами в базе данных с элементом ModificationFunctionMapping.</span><span class="sxs-lookup"><span data-stu-id="a9957-171">Map the insert, update, and delete functions for associations to stored procedures in the database with the ModificationFunctionMapping element.</span></span> <span data-ttu-id="a9957-172">Определите только для чтения сопоставление между ассоциациями и столбцами таблицы с помощью строк Entity SQL в QueryView-элемент.</span><span class="sxs-lookup"><span data-stu-id="a9957-172">Define read-only mappings between associations and table columns by using an Entity SQL string in a QueryView element.</span></span>

> [!NOTE]
> <span data-ttu-id="a9957-173">Если для ассоциации в концептуальной модели определено ссылочное ограничение, ассоциации необходимо сопоставить с **AssociationSetMapping** элемент.</span><span class="sxs-lookup"><span data-stu-id="a9957-173">If a referential constraint is defined for an association in the conceptual model, the association does not need to be mapped with an **AssociationSetMapping** element.</span></span> <span data-ttu-id="a9957-174">Если **AssociationSetMapping** элемент присутствует ассоциацию со справочным ограничением, то сопоставление, определенное в **AssociationSetMapping** элемент будет игнорироваться.</span><span class="sxs-lookup"><span data-stu-id="a9957-174">If an **AssociationSetMapping** element is present for an association that has a referential constraint, the mappings defined in the **AssociationSetMapping** element will be ignored.</span></span> <span data-ttu-id="a9957-175">Дополнительные сведения см. в разделе элемент ReferentialConstraint (CSDL).</span><span class="sxs-lookup"><span data-stu-id="a9957-175">For more information, see ReferentialConstraint Element (CSDL).</span></span>

<span data-ttu-id="a9957-176">**AssociationSetMapping** элемент может иметь следующие дочерние элементы</span><span class="sxs-lookup"><span data-stu-id="a9957-176">The **AssociationSetMapping** element can have the following child elements</span></span>

-   <span data-ttu-id="a9957-177">QueryView (ноль или один)</span><span class="sxs-lookup"><span data-stu-id="a9957-177">QueryView (zero or one)</span></span>
-   <span data-ttu-id="a9957-178">EndProperty (ноль или два)</span><span class="sxs-lookup"><span data-stu-id="a9957-178">EndProperty (zero or two)</span></span>
-   <span data-ttu-id="a9957-179">Условие (ноль и более)</span><span class="sxs-lookup"><span data-stu-id="a9957-179">Condition (zero or more)</span></span>
-   <span data-ttu-id="a9957-180">ModificationFunctionMapping (ноль или один)</span><span class="sxs-lookup"><span data-stu-id="a9957-180">ModificationFunctionMapping (zero or one)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="a9957-181">Применимые атрибуты</span><span class="sxs-lookup"><span data-stu-id="a9957-181">Applicable Attributes</span></span>

<span data-ttu-id="a9957-182">В следующей таблице описаны атрибуты, которые могут применяться к **AssociationSetMapping** элемент.</span><span class="sxs-lookup"><span data-stu-id="a9957-182">The following table describes the attributes that can be applied to the **AssociationSetMapping** element.</span></span>

| <span data-ttu-id="a9957-183">Имя атрибута</span><span class="sxs-lookup"><span data-stu-id="a9957-183">Attribute Name</span></span>     | <span data-ttu-id="a9957-184">Необходимо</span><span class="sxs-lookup"><span data-stu-id="a9957-184">Is Required</span></span> | <span data-ttu-id="a9957-185">Значение</span><span class="sxs-lookup"><span data-stu-id="a9957-185">Value</span></span>                                                                                       |
|:-------------------|:------------|:--------------------------------------------------------------------------------------------|
| <span data-ttu-id="a9957-186">**Name**</span><span class="sxs-lookup"><span data-stu-id="a9957-186">**Name**</span></span>           | <span data-ttu-id="a9957-187">Да</span><span class="sxs-lookup"><span data-stu-id="a9957-187">Yes</span></span>         | <span data-ttu-id="a9957-188">Имя набора ассоциаций концептуальной модели, с которым выполняется сопоставление.</span><span class="sxs-lookup"><span data-stu-id="a9957-188">The name of the conceptual model association set that is being mapped.</span></span>                      |
| <span data-ttu-id="a9957-189">**Имя типа**</span><span class="sxs-lookup"><span data-stu-id="a9957-189">**TypeName**</span></span>       | <span data-ttu-id="a9957-190">Нет</span><span class="sxs-lookup"><span data-stu-id="a9957-190">No</span></span>          | <span data-ttu-id="a9957-191">Имя типа ассоциации концептуальной модели из полного пространства имен, с которым выполняется сопоставление.</span><span class="sxs-lookup"><span data-stu-id="a9957-191">The namespace-qualified name of the conceptual model association type that is being mapped.</span></span> |
| <span data-ttu-id="a9957-192">**StoreEntitySet**</span><span class="sxs-lookup"><span data-stu-id="a9957-192">**StoreEntitySet**</span></span> | <span data-ttu-id="a9957-193">Нет</span><span class="sxs-lookup"><span data-stu-id="a9957-193">No</span></span>          | <span data-ttu-id="a9957-194">Имя таблицы, с которой производится сопоставление.</span><span class="sxs-lookup"><span data-stu-id="a9957-194">The name of the table that is being mapped.</span></span>                                                 |

### <a name="example"></a><span data-ttu-id="a9957-195">Пример</span><span class="sxs-lookup"><span data-stu-id="a9957-195">Example</span></span>

<span data-ttu-id="a9957-196">В следующем примере показан **AssociationSetMapping** элемента, в котором **FK\_курс\_отдел** набор ассоциаций в концептуальной модели сопоставляется с  **Курс** таблицы в базе данных.</span><span class="sxs-lookup"><span data-stu-id="a9957-196">The following example shows an **AssociationSetMapping** element in which the **FK\_Course\_Department** association set in the conceptual model is mapped to the **Course** table in the database.</span></span> <span data-ttu-id="a9957-197">Сопоставления между свойствами типов сопоставлений и столбцами таблицы указываются в дочерних **EndProperty** элементов.</span><span class="sxs-lookup"><span data-stu-id="a9957-197">Mappings between association type properties and table columns are specified in child **EndProperty** elements.</span></span>

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

## <a name="complexproperty-element-msl"></a><span data-ttu-id="a9957-198">Элемент ComplexProperty (MSL)</span><span class="sxs-lookup"><span data-stu-id="a9957-198">ComplexProperty Element (MSL)</span></span>

<span data-ttu-id="a9957-199">Объект **ComplexProperty** элемент в язык определения сопоставлений (MSL) определяет сопоставление между свойством сложного типа в концептуальной модели сущности типа и столбцами таблицы в основной базе данных.</span><span class="sxs-lookup"><span data-stu-id="a9957-199">A **ComplexProperty** element in mapping specification language (MSL) defines the mapping between a complex type property on a conceptual model entity type and table columns in the underlying database.</span></span> <span data-ttu-id="a9957-200">Сопоставления столбца свойств указываются в дочерних элементах ScalarProperty.</span><span class="sxs-lookup"><span data-stu-id="a9957-200">The property-column mappings are specified in child ScalarProperty elements.</span></span>

<span data-ttu-id="a9957-201">**ComplexType** свойство может иметь следующие дочерние элементы:</span><span class="sxs-lookup"><span data-stu-id="a9957-201">The **ComplexType** property element can have the following child elements:</span></span>

-   <span data-ttu-id="a9957-202">ScalarProperty (ноль и более)</span><span class="sxs-lookup"><span data-stu-id="a9957-202">ScalarProperty (zero or more)</span></span>
-   <span data-ttu-id="a9957-203">**ComplexProperty** (ноль и более)</span><span class="sxs-lookup"><span data-stu-id="a9957-203">**ComplexProperty** (zero or more)</span></span>
-   <span data-ttu-id="a9957-204">ComplextTypeMapping (ноль и более)</span><span class="sxs-lookup"><span data-stu-id="a9957-204">ComplextTypeMapping (zero or more)</span></span>
-   <span data-ttu-id="a9957-205">Условие (ноль и более)</span><span class="sxs-lookup"><span data-stu-id="a9957-205">Condition (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="a9957-206">Применимые атрибуты</span><span class="sxs-lookup"><span data-stu-id="a9957-206">Applicable Attributes</span></span>

<span data-ttu-id="a9957-207">В следующей таблице описаны атрибуты, применимые к **ComplexProperty** элемент:</span><span class="sxs-lookup"><span data-stu-id="a9957-207">The following table describes the attributes that are applicable to the **ComplexProperty** element:</span></span>

| <span data-ttu-id="a9957-208">Имя атрибута</span><span class="sxs-lookup"><span data-stu-id="a9957-208">Attribute Name</span></span> | <span data-ttu-id="a9957-209">Необходимо</span><span class="sxs-lookup"><span data-stu-id="a9957-209">Is Required</span></span> | <span data-ttu-id="a9957-210">Значение</span><span class="sxs-lookup"><span data-stu-id="a9957-210">Value</span></span>                                                                                            |
|:---------------|:------------|:-------------------------------------------------------------------------------------------------|
| <span data-ttu-id="a9957-211">**Name**</span><span class="sxs-lookup"><span data-stu-id="a9957-211">**Name**</span></span>       | <span data-ttu-id="a9957-212">Да</span><span class="sxs-lookup"><span data-stu-id="a9957-212">Yes</span></span>         | <span data-ttu-id="a9957-213">Имя сложного свойства типа сущности в концептуальной модели, с которым выполняется сопоставление.</span><span class="sxs-lookup"><span data-stu-id="a9957-213">The name of the complex property of an entity type in the conceptual model that is being mapped.</span></span> |
| <span data-ttu-id="a9957-214">**Имя типа**</span><span class="sxs-lookup"><span data-stu-id="a9957-214">**TypeName**</span></span>   | <span data-ttu-id="a9957-215">Нет</span><span class="sxs-lookup"><span data-stu-id="a9957-215">No</span></span>          | <span data-ttu-id="a9957-216">Полное имя пространства имен типа свойства концептуальной модели.</span><span class="sxs-lookup"><span data-stu-id="a9957-216">The namespace-qualified name of the conceptual model property type.</span></span>                              |

### <a name="example"></a><span data-ttu-id="a9957-217">Пример</span><span class="sxs-lookup"><span data-stu-id="a9957-217">Example</span></span>

<span data-ttu-id="a9957-218">Следующий пример основан на модели School.</span><span class="sxs-lookup"><span data-stu-id="a9957-218">The following example is based on the School model.</span></span> <span data-ttu-id="a9957-219">К концептуальной модели добавлен следующий сложный тип.</span><span class="sxs-lookup"><span data-stu-id="a9957-219">The following complex type has been added to the conceptual model:</span></span>

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

<span data-ttu-id="a9957-220">**LastName** и **FirstName** свойства **Person** тип сущности были заменены одним сложным свойством **имя**:</span><span class="sxs-lookup"><span data-stu-id="a9957-220">The **LastName** and **FirstName** properties of the **Person** entity type have been replaced with one complex property, **Name**:</span></span>

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

<span data-ttu-id="a9957-221">Следующей спецификации MSL показан **ComplexProperty** элемент, используемый для сопоставления **имя** свойства со столбцами в основной базе данных:</span><span class="sxs-lookup"><span data-stu-id="a9957-221">The following MSL shows the **ComplexProperty** element used to map the **Name** property to columns in the underlying database:</span></span>

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

## <a name="complextypemapping-element-msl"></a><span data-ttu-id="a9957-222">Элемент ComplexTypeMapping (MSL)</span><span class="sxs-lookup"><span data-stu-id="a9957-222">ComplexTypeMapping Element (MSL)</span></span>

<span data-ttu-id="a9957-223">**ComplexTypeMapping** элементом в язык определения сопоставлений (MSL) — это дочерний элемент ResultMapping и определяет сопоставление между импортом функции в концептуальной модели и хранимой процедуры в базовом База данных, при выполнении следующих условий:</span><span class="sxs-lookup"><span data-stu-id="a9957-223">The **ComplexTypeMapping** element in mapping specification language (MSL) is a child of the ResultMapping element and defines the mapping between a function import in the conceptual model and a stored procedure in the underlying database when the following are true:</span></span>

-   <span data-ttu-id="a9957-224">Импорт функции возвращает концептуальный сложный тип.</span><span class="sxs-lookup"><span data-stu-id="a9957-224">The function import returns a conceptual complex type.</span></span>
-   <span data-ttu-id="a9957-225">Имена столбцов, возвращаемые хранимой процедурой, не соответствуют в точности именам свойств в сложном типе.</span><span class="sxs-lookup"><span data-stu-id="a9957-225">The names of the columns returned by the stored procedure do not exactly match the names of the properties on the complex type.</span></span>

<span data-ttu-id="a9957-226">По умолчанию сопоставление между столбцами, возвращаемыми хранимой процедурой, и сложным типом основано на именах столбцов и свойств.</span><span class="sxs-lookup"><span data-stu-id="a9957-226">By default, the mapping between the columns returned by a stored procedure and a complex type is based on column and property names.</span></span> <span data-ttu-id="a9957-227">Если имена столбцов не точно совпадают с именами свойств, необходимо использовать **ComplexTypeMapping** элемент для определения сопоставления.</span><span class="sxs-lookup"><span data-stu-id="a9957-227">If column names do not exactly match property names, you must use the **ComplexTypeMapping** element to define the mapping.</span></span> <span data-ttu-id="a9957-228">Пример сопоставления по умолчанию см. в разделе элемент FunctionImportMapping (MSL).</span><span class="sxs-lookup"><span data-stu-id="a9957-228">For an example of the default mapping, see FunctionImportMapping Element (MSL).</span></span>

<span data-ttu-id="a9957-229">**ComplexTypeMapping** элемент может иметь следующие дочерние элементы:</span><span class="sxs-lookup"><span data-stu-id="a9957-229">The **ComplexTypeMapping** element can have the following child elements:</span></span>

-   <span data-ttu-id="a9957-230">ScalarProperty (ноль и более)</span><span class="sxs-lookup"><span data-stu-id="a9957-230">ScalarProperty (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="a9957-231">Применимые атрибуты</span><span class="sxs-lookup"><span data-stu-id="a9957-231">Applicable Attributes</span></span>

<span data-ttu-id="a9957-232">В следующей таблице описаны атрибуты, применимые к **ComplexTypeMapping** элемент.</span><span class="sxs-lookup"><span data-stu-id="a9957-232">The following table describes the attributes that are applicable to the **ComplexTypeMapping** element.</span></span>

| <span data-ttu-id="a9957-233">Имя атрибута</span><span class="sxs-lookup"><span data-stu-id="a9957-233">Attribute Name</span></span> | <span data-ttu-id="a9957-234">Необходимо</span><span class="sxs-lookup"><span data-stu-id="a9957-234">Is Required</span></span> | <span data-ttu-id="a9957-235">Значение</span><span class="sxs-lookup"><span data-stu-id="a9957-235">Value</span></span>                                                                  |
|:---------------|:------------|:-----------------------------------------------------------------------|
| <span data-ttu-id="a9957-236">**Имя типа**</span><span class="sxs-lookup"><span data-stu-id="a9957-236">**TypeName**</span></span>   | <span data-ttu-id="a9957-237">Да</span><span class="sxs-lookup"><span data-stu-id="a9957-237">Yes</span></span>         | <span data-ttu-id="a9957-238">Имя сопоставляемого сложного типа с указанием пространства имен.</span><span class="sxs-lookup"><span data-stu-id="a9957-238">The namespace-qualified name of the complex type that is being mapped.</span></span> |

### <a name="example"></a><span data-ttu-id="a9957-239">Пример</span><span class="sxs-lookup"><span data-stu-id="a9957-239">Example</span></span>

<span data-ttu-id="a9957-240">Рассмотрим следующую хранимую процедуру:</span><span class="sxs-lookup"><span data-stu-id="a9957-240">Consider the following stored procedure:</span></span>

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

<span data-ttu-id="a9957-241">Следует также обратить внимание на следующий сложный тип концептуальной модели:</span><span class="sxs-lookup"><span data-stu-id="a9957-241">Also consider the following conceptual model complex type:</span></span>

``` xml
 <ComplexType Name="GradeInfo">
   <Property Type="Int32" Name="EnrollmentID" Nullable="false" />
   <Property Type="Decimal" Name="Grade" Nullable="true"
             Precision="3" Scale="2" />
   <Property Type="Int32" Name="CourseID" Nullable="false" />
   <Property Type="Int32" Name="StudentID" Nullable="false" />
 </ComplexType>
```

<span data-ttu-id="a9957-242">Для создания импорта функции, возвращающей экземпляры предыдущего сложного типа, сопоставление между столбцами, возвращаемыми хранимой процедуры и тип сущности должен быть определен в **ComplexTypeMapping** элемент:</span><span class="sxs-lookup"><span data-stu-id="a9957-242">In order to create a function import that returns instances of the previous complex type, the mapping between the columns returned by the stored procedure and the entity type must be defined in a **ComplexTypeMapping** element:</span></span>

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

## <a name="condition-element-msl"></a><span data-ttu-id="a9957-243">Элемент Condition (MSL)</span><span class="sxs-lookup"><span data-stu-id="a9957-243">Condition Element (MSL)</span></span>

<span data-ttu-id="a9957-244">**Условие** элемент в язык определения сопоставлений (MSL) Задает условия для сопоставления между концептуальной моделью и основной базе данных.</span><span class="sxs-lookup"><span data-stu-id="a9957-244">The **Condition** element in mapping specification language (MSL) places conditions on mappings between the conceptual model and the underlying database.</span></span> <span data-ttu-id="a9957-245">Сопоставление, определенные в XML-узла является допустимым, если все условия, заданные в дочерних **условие** элементов, будут выполнены.</span><span class="sxs-lookup"><span data-stu-id="a9957-245">The mapping that is defined within an XML node is valid if all conditions, as specified in child **Condition** elements, are met.</span></span> <span data-ttu-id="a9957-246">В противном случае сопоставление недействительно.</span><span class="sxs-lookup"><span data-stu-id="a9957-246">Otherwise, the mapping is not valid.</span></span> <span data-ttu-id="a9957-247">Например, если элемент MappingFragment содержит один или несколько **условие** дочерние элементы, то сопоставление, определенное в **MappingFragment** узел будет допустимым, если все условия дочернегоэлемента **Условие** элементы будут выполнены.</span><span class="sxs-lookup"><span data-stu-id="a9957-247">For example, if a MappingFragment element contains one or more **Condition** child elements, the mapping defined within the **MappingFragment** node will only be valid if all the conditions of the child **Condition** elements are met.</span></span>

<span data-ttu-id="a9957-248">Каждое условие может применяться либо **имя** (имя свойства сущности концептуальной модели, определяемое **имя** атрибут), или **ColumnName** (имя столбца в базы данных, определяемое **ColumnName** атрибут).</span><span class="sxs-lookup"><span data-stu-id="a9957-248">Each condition can apply to either a **Name** (the name of a conceptual model entity property, specified by the **Name** attribute), or a **ColumnName** (the name of a column in the database, specified by the **ColumnName** attribute).</span></span> <span data-ttu-id="a9957-249">Когда **имя** атрибут имеет значение, условие проверяется по значению свойства сущности.</span><span class="sxs-lookup"><span data-stu-id="a9957-249">When the **Name** attribute is set, the condition is checked against an entity property value.</span></span> <span data-ttu-id="a9957-250">Когда **ColumnName** атрибут имеет значение, условие проверяется значение столбца.</span><span class="sxs-lookup"><span data-stu-id="a9957-250">When the **ColumnName** attribute is set, the condition is checked against a column value.</span></span> <span data-ttu-id="a9957-251">Только один из **имя** или **ColumnName** атрибут может быть указан в **условие** элемент.</span><span class="sxs-lookup"><span data-stu-id="a9957-251">Only one of the **Name** or **ColumnName** attribute can be specified in a **Condition** element.</span></span>

> [!NOTE]
> <span data-ttu-id="a9957-252">Когда **условие** элемент используется внутри элемента FunctionImportMapping только **имя** атрибут неприменим.</span><span class="sxs-lookup"><span data-stu-id="a9957-252">When the **Condition** element is used within a FunctionImportMapping element, only the **Name** attribute is not applicable.</span></span>

<span data-ttu-id="a9957-253">**Условие** элемент может быть дочерним элементом следующих элементов:</span><span class="sxs-lookup"><span data-stu-id="a9957-253">The **Condition** element can be a child of the following elements:</span></span>

-   <span data-ttu-id="a9957-254">AssociationSetMapping</span><span class="sxs-lookup"><span data-stu-id="a9957-254">AssociationSetMapping</span></span>
-   <span data-ttu-id="a9957-255">ComplexProperty</span><span class="sxs-lookup"><span data-stu-id="a9957-255">ComplexProperty</span></span>
-   <span data-ttu-id="a9957-256">EntitySetMapping</span><span class="sxs-lookup"><span data-stu-id="a9957-256">EntitySetMapping</span></span>
-   <span data-ttu-id="a9957-257">MappingFragment</span><span class="sxs-lookup"><span data-stu-id="a9957-257">MappingFragment</span></span>
-   <span data-ttu-id="a9957-258">EntityTypeMapping</span><span class="sxs-lookup"><span data-stu-id="a9957-258">EntityTypeMapping</span></span>

<span data-ttu-id="a9957-259">**Условие** элемент может иметь дочерних элементов.</span><span class="sxs-lookup"><span data-stu-id="a9957-259">The **Condition** element can have no child elements.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="a9957-260">Применимые атрибуты</span><span class="sxs-lookup"><span data-stu-id="a9957-260">Applicable Attributes</span></span>

<span data-ttu-id="a9957-261">В следующей таблице описаны атрибуты, применимые к **условие** элемент:</span><span class="sxs-lookup"><span data-stu-id="a9957-261">The following table describes the attributes that are applicable to the **Condition** element:</span></span>

| <span data-ttu-id="a9957-262">Имя атрибута</span><span class="sxs-lookup"><span data-stu-id="a9957-262">Attribute Name</span></span> | <span data-ttu-id="a9957-263">Необходимо</span><span class="sxs-lookup"><span data-stu-id="a9957-263">Is Required</span></span> | <span data-ttu-id="a9957-264">Значение</span><span class="sxs-lookup"><span data-stu-id="a9957-264">Value</span></span>                                                                                                                                                                                                                                                                                         |
|:---------------|:------------|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="a9957-265">**ColumnName**</span><span class="sxs-lookup"><span data-stu-id="a9957-265">**ColumnName**</span></span> | <span data-ttu-id="a9957-266">Нет</span><span class="sxs-lookup"><span data-stu-id="a9957-266">No</span></span>          | <span data-ttu-id="a9957-267">Имя столбца таблицы, значение которого используется для оценки условия.</span><span class="sxs-lookup"><span data-stu-id="a9957-267">The name of the table column whose value is used to evaluate the condition.</span></span>                                                                                                                                                                                                                   |
| <span data-ttu-id="a9957-268">**IsNull**</span><span class="sxs-lookup"><span data-stu-id="a9957-268">**IsNull**</span></span>     | <span data-ttu-id="a9957-269">Нет</span><span class="sxs-lookup"><span data-stu-id="a9957-269">No</span></span>          | <span data-ttu-id="a9957-270">**Значение true,** или **False**.</span><span class="sxs-lookup"><span data-stu-id="a9957-270">**True** or **False**.</span></span> <span data-ttu-id="a9957-271">Если значение равно **True** и значение столбца равно **null**, или если значение равно **False** и значение столбца не **null**, условие имеет значение true .</span><span class="sxs-lookup"><span data-stu-id="a9957-271">If the value is **True** and the column value is **null**, or if the value is **False** and the column value is not **null**, the condition is true.</span></span> <span data-ttu-id="a9957-272">В противном случае условие имеет значение false.</span><span class="sxs-lookup"><span data-stu-id="a9957-272">Otherwise, the condition is false.</span></span> <br/> <span data-ttu-id="a9957-273">**IsNull** и **значение** атрибуты нельзя использовать одновременно.</span><span class="sxs-lookup"><span data-stu-id="a9957-273">The **IsNull** and **Value** attributes cannot be used at the same time.</span></span> |
| <span data-ttu-id="a9957-274">**Значение**</span><span class="sxs-lookup"><span data-stu-id="a9957-274">**Value**</span></span>      | <span data-ttu-id="a9957-275">Нет</span><span class="sxs-lookup"><span data-stu-id="a9957-275">No</span></span>          | <span data-ttu-id="a9957-276">Значение, с которым сравнивается значение столбца.</span><span class="sxs-lookup"><span data-stu-id="a9957-276">The value with which the column value is compared.</span></span> <span data-ttu-id="a9957-277">Если значения совпадают, условие верно.</span><span class="sxs-lookup"><span data-stu-id="a9957-277">If the values are the same, the condition is true.</span></span> <span data-ttu-id="a9957-278">В противном случае условие имеет значение false.</span><span class="sxs-lookup"><span data-stu-id="a9957-278">Otherwise, the condition is false.</span></span> <br/> <span data-ttu-id="a9957-279">**IsNull** и **значение** атрибуты нельзя использовать одновременно.</span><span class="sxs-lookup"><span data-stu-id="a9957-279">The **IsNull** and **Value** attributes cannot be used at the same time.</span></span>                                                                       |
| <span data-ttu-id="a9957-280">**Name**</span><span class="sxs-lookup"><span data-stu-id="a9957-280">**Name**</span></span>       | <span data-ttu-id="a9957-281">Нет</span><span class="sxs-lookup"><span data-stu-id="a9957-281">No</span></span>          | <span data-ttu-id="a9957-282">Имя свойства сущности концептуальной модели, значение которого используется для оценки условия.</span><span class="sxs-lookup"><span data-stu-id="a9957-282">The name of the conceptual model entity property whose value is used to evaluate the condition.</span></span> <br/> <span data-ttu-id="a9957-283">Этот атрибут неприменим Если **условие** элемент используется в элемент FunctionImportMapping.</span><span class="sxs-lookup"><span data-stu-id="a9957-283">This attribute is not applicable if the **Condition** element is used within a FunctionImportMapping element.</span></span>                                                                           |

### <a name="example"></a><span data-ttu-id="a9957-284">Пример</span><span class="sxs-lookup"><span data-stu-id="a9957-284">Example</span></span>

<span data-ttu-id="a9957-285">В следующем примере показан **условие** элементы как дочерние элементы **MappingFragment** элементов.</span><span class="sxs-lookup"><span data-stu-id="a9957-285">The following example shows **Condition** elements as children of **MappingFragment** elements.</span></span> <span data-ttu-id="a9957-286">Когда **HireDate** не равно null и **EnrollmentDate** является null, то данные сопоставляются между **SchoolModel.Instructor** типа и **PersonID**и **HireDate** столбцы **Person** таблицы.</span><span class="sxs-lookup"><span data-stu-id="a9957-286">When **HireDate** is not null and **EnrollmentDate** is null, data is mapped between the **SchoolModel.Instructor** type and the **PersonID** and **HireDate** columns of the **Person** table.</span></span> <span data-ttu-id="a9957-287">Когда **EnrollmentDate** не равно null и **HireDate** является null, то данные сопоставляются между **SchoolModel.Student** типа и **PersonID** и **регистрации** столбцы **Person** таблицы.</span><span class="sxs-lookup"><span data-stu-id="a9957-287">When **EnrollmentDate** is not null and **HireDate** is null, data is mapped between the **SchoolModel.Student** type and the **PersonID** and **Enrollment** columns of the **Person** table.</span></span>

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

## <a name="deletefunction-element-msl"></a><span data-ttu-id="a9957-288">Элемент DeleteFunction (MSL)</span><span class="sxs-lookup"><span data-stu-id="a9957-288">DeleteFunction Element (MSL)</span></span>

<span data-ttu-id="a9957-289">**DeleteFunction** в язык определения сопоставлений (MSL) сопоставляет функцию удаления типа сущности или ассоциации в концептуальной модели с хранимой процедурой в основной базе данных.</span><span class="sxs-lookup"><span data-stu-id="a9957-289">The **DeleteFunction** element in mapping specification language (MSL) maps the delete function of an entity type or association in the conceptual model to a stored procedure in the underlying database.</span></span> <span data-ttu-id="a9957-290">Хранимые процедуры, с которыми сопоставляются функции изменения, должны объявляться в модели хранения.</span><span class="sxs-lookup"><span data-stu-id="a9957-290">Stored procedures to which modification functions are mapped must be declared in the storage model.</span></span> <span data-ttu-id="a9957-291">Дополнительные сведения см. в разделе элемента функция (SSDL).</span><span class="sxs-lookup"><span data-stu-id="a9957-291">For more information, see Function Element (SSDL).</span></span>

> [!NOTE]
> <span data-ttu-id="a9957-292">Если не выполнено сопоставление всех трех вставки, обновления или удаления типов сущностей с хранимыми процедурами, то несопоставленные операции завершится ошибкой, если во время выполнения и UpdateException возникает исключение.</span><span class="sxs-lookup"><span data-stu-id="a9957-292">If you do not map all three of the insert, update, or delete operations of a entity type to stored procedures, the unmapped operations will fail if executed at runtime and an UpdateException is thrown.</span></span>

### <a name="deletefunction-applied-to-entitytypemapping"></a><span data-ttu-id="a9957-293">Элемент DeleteFunction, применяемый к EntityTypeMapping</span><span class="sxs-lookup"><span data-stu-id="a9957-293">DeleteFunction Applied to EntityTypeMapping</span></span>

<span data-ttu-id="a9957-294">При применении к элементу EntityTypeMapping **DeleteFunction** сопоставляет функцию удаления типа сущности в концептуальной модели с хранимой процедурой.</span><span class="sxs-lookup"><span data-stu-id="a9957-294">When applied to the EntityTypeMapping element, the **DeleteFunction** element maps the delete function of an entity type in the conceptual model to a stored procedure.</span></span>

<span data-ttu-id="a9957-295">**DeleteFunction** элемент может иметь следующие дочерние элементы при применении к **EntityTypeMapping** элемент:</span><span class="sxs-lookup"><span data-stu-id="a9957-295">The **DeleteFunction** element can have the following child elements when applied to an **EntityTypeMapping** element:</span></span>

-   <span data-ttu-id="a9957-296">AssociationEnd (ноль и более)</span><span class="sxs-lookup"><span data-stu-id="a9957-296">AssociationEnd (zero or more)</span></span>
-   <span data-ttu-id="a9957-297">ComplexProperty (ноль и более)</span><span class="sxs-lookup"><span data-stu-id="a9957-297">ComplexProperty (zero or more)</span></span>
-   <span data-ttu-id="a9957-298">ScarlarProperty (ноль и более)</span><span class="sxs-lookup"><span data-stu-id="a9957-298">ScarlarProperty (zero or more)</span></span>

#### <a name="applicable-attributes"></a><span data-ttu-id="a9957-299">Применимые атрибуты</span><span class="sxs-lookup"><span data-stu-id="a9957-299">Applicable Attributes</span></span>

<span data-ttu-id="a9957-300">В следующей таблице описаны атрибуты, которые могут применяться к **DeleteFunction** элемента при его применении к **EntityTypeMapping** элемент.</span><span class="sxs-lookup"><span data-stu-id="a9957-300">The following table describes the attributes that can be applied to the **DeleteFunction** element when it is applied to an **EntityTypeMapping** element.</span></span>

| <span data-ttu-id="a9957-301">Имя атрибута</span><span class="sxs-lookup"><span data-stu-id="a9957-301">Attribute Name</span></span>            | <span data-ttu-id="a9957-302">Необходимо</span><span class="sxs-lookup"><span data-stu-id="a9957-302">Is Required</span></span> | <span data-ttu-id="a9957-303">Значение</span><span class="sxs-lookup"><span data-stu-id="a9957-303">Value</span></span>                                                                                                                                                    |
|:--------------------------|:------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="a9957-304">**FunctionName**</span><span class="sxs-lookup"><span data-stu-id="a9957-304">**FunctionName**</span></span>          | <span data-ttu-id="a9957-305">Да</span><span class="sxs-lookup"><span data-stu-id="a9957-305">Yes</span></span>         | <span data-ttu-id="a9957-306">Имя хранимой процедуры (с указанием пространства имен), с которой сопоставляется функция удаления.</span><span class="sxs-lookup"><span data-stu-id="a9957-306">The namespace-qualified name of the stored procedure to which the delete function is mapped.</span></span> <span data-ttu-id="a9957-307">Хранимая процедура должна объявляться в модели хранения.</span><span class="sxs-lookup"><span data-stu-id="a9957-307">The stored procedure must be declared in the storage model.</span></span> |
| <span data-ttu-id="a9957-308">**RowsAffectedParameter**</span><span class="sxs-lookup"><span data-stu-id="a9957-308">**RowsAffectedParameter**</span></span> | <span data-ttu-id="a9957-309">Нет</span><span class="sxs-lookup"><span data-stu-id="a9957-309">No</span></span>          | <span data-ttu-id="a9957-310">Имя выходного параметра, возвращающего количество обработанных строк.</span><span class="sxs-lookup"><span data-stu-id="a9957-310">The name of the output parameter that returns the number of rows affected.</span></span>                                                                               |

#### <a name="example"></a><span data-ttu-id="a9957-311">Пример</span><span class="sxs-lookup"><span data-stu-id="a9957-311">Example</span></span>

<span data-ttu-id="a9957-312">В следующем примере, основана на модели School и показывает **DeleteFunction** элемент сопоставления функции удаления **Person** тип сущности, **DeletePerson** Хранимая процедура.</span><span class="sxs-lookup"><span data-stu-id="a9957-312">The following example is based on the School model and shows the **DeleteFunction** element mapping the delete function of the **Person** entity type to the **DeletePerson** stored procedure.</span></span> <span data-ttu-id="a9957-313">**DeletePerson** хранимая процедура объявлена в модели хранения.</span><span class="sxs-lookup"><span data-stu-id="a9957-313">The **DeletePerson** stored procedure is declared in the storage model.</span></span>

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

### <a name="deletefunction-applied-to-associationsetmapping"></a><span data-ttu-id="a9957-314">Элемент DeleteFunction, применяемый к AssociationSetMapping</span><span class="sxs-lookup"><span data-stu-id="a9957-314">DeleteFunction Applied to AssociationSetMapping</span></span>

<span data-ttu-id="a9957-315">При применении к элементу AssociationSetMapping **DeleteFunction** сопоставляет функцию удаления ассоциации в концептуальной модели с хранимой процедурой.</span><span class="sxs-lookup"><span data-stu-id="a9957-315">When applied to the AssociationSetMapping element, the **DeleteFunction** element maps the delete function of an association in the conceptual model to a stored procedure.</span></span>

<span data-ttu-id="a9957-316">**DeleteFunction** элемент может иметь следующие дочерние элементы при применении к **AssociationSetMapping** элемент:</span><span class="sxs-lookup"><span data-stu-id="a9957-316">The **DeleteFunction** element can have the following child elements when applied to the **AssociationSetMapping** element:</span></span>

-   <span data-ttu-id="a9957-317">EndProperty</span><span class="sxs-lookup"><span data-stu-id="a9957-317">EndProperty</span></span>

#### <a name="applicable-attributes"></a><span data-ttu-id="a9957-318">Применимые атрибуты</span><span class="sxs-lookup"><span data-stu-id="a9957-318">Applicable Attributes</span></span>

<span data-ttu-id="a9957-319">В следующей таблице описаны атрибуты, которые могут применяться к **DeleteFunction** элемента при его применении к **AssociationSetMapping** элемент.</span><span class="sxs-lookup"><span data-stu-id="a9957-319">The following table describes the attributes that can be applied to the **DeleteFunction** element when it is applied to the **AssociationSetMapping** element.</span></span>

| <span data-ttu-id="a9957-320">Имя атрибута</span><span class="sxs-lookup"><span data-stu-id="a9957-320">Attribute Name</span></span>            | <span data-ttu-id="a9957-321">Необходимо</span><span class="sxs-lookup"><span data-stu-id="a9957-321">Is Required</span></span> | <span data-ttu-id="a9957-322">Значение</span><span class="sxs-lookup"><span data-stu-id="a9957-322">Value</span></span>                                                                                                                                                    |
|:--------------------------|:------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="a9957-323">**FunctionName**</span><span class="sxs-lookup"><span data-stu-id="a9957-323">**FunctionName**</span></span>          | <span data-ttu-id="a9957-324">Да</span><span class="sxs-lookup"><span data-stu-id="a9957-324">Yes</span></span>         | <span data-ttu-id="a9957-325">Имя хранимой процедуры (с указанием пространства имен), с которой сопоставляется функция удаления.</span><span class="sxs-lookup"><span data-stu-id="a9957-325">The namespace-qualified name of the stored procedure to which the delete function is mapped.</span></span> <span data-ttu-id="a9957-326">Хранимая процедура должна объявляться в модели хранения.</span><span class="sxs-lookup"><span data-stu-id="a9957-326">The stored procedure must be declared in the storage model.</span></span> |
| <span data-ttu-id="a9957-327">**RowsAffectedParameter**</span><span class="sxs-lookup"><span data-stu-id="a9957-327">**RowsAffectedParameter**</span></span> | <span data-ttu-id="a9957-328">Нет</span><span class="sxs-lookup"><span data-stu-id="a9957-328">No</span></span>          | <span data-ttu-id="a9957-329">Имя выходного параметра, возвращающего количество обработанных строк.</span><span class="sxs-lookup"><span data-stu-id="a9957-329">The name of the output parameter that returns the number of rows affected.</span></span>                                                                               |

#### <a name="example"></a><span data-ttu-id="a9957-330">Пример</span><span class="sxs-lookup"><span data-stu-id="a9957-330">Example</span></span>

<span data-ttu-id="a9957-331">В следующем примере, основана на модели School и показывает **DeleteFunction** элемент, используемый для сопоставления функции удаления **CourseInstructor** взаимосвязи  **DeleteCourseInstructor** хранимой процедуры.</span><span class="sxs-lookup"><span data-stu-id="a9957-331">The following example is based on the School model and shows the **DeleteFunction** element used to map delete function of the **CourseInstructor** association to the **DeleteCourseInstructor** stored procedure.</span></span> <span data-ttu-id="a9957-332">**DeleteCourseInstructor** хранимая процедура объявлена в модели хранения.</span><span class="sxs-lookup"><span data-stu-id="a9957-332">The **DeleteCourseInstructor** stored procedure is declared in the storage model.</span></span>

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

## <a name="endproperty-element-msl"></a><span data-ttu-id="a9957-333">Элемент EndProperty (MSL)</span><span class="sxs-lookup"><span data-stu-id="a9957-333">EndProperty Element (MSL)</span></span>

<span data-ttu-id="a9957-334">**EndProperty** элемент в язык определения сопоставлений (MSL) определяет сопоставление между функцией изменения ассоциации концептуальной модели и основной базе данных или концом.</span><span class="sxs-lookup"><span data-stu-id="a9957-334">The **EndProperty** element in mapping specification language (MSL) defines the mapping between an end or a modification function of a conceptual model association and the underlying database.</span></span> <span data-ttu-id="a9957-335">Сопоставление столбца свойств указано в дочерний элемент ScalarProperty.</span><span class="sxs-lookup"><span data-stu-id="a9957-335">The property-column mapping is specified in a child ScalarProperty element.</span></span>

<span data-ttu-id="a9957-336">Когда **EndProperty** элемент используется для определения сопоставления конца ассоциации концептуальной модели, он является дочерним элементом элемента AssociationSetMapping.</span><span class="sxs-lookup"><span data-stu-id="a9957-336">When an **EndProperty** element is used to define the mapping for the end of a conceptual model association, it is a child of an AssociationSetMapping element.</span></span> <span data-ttu-id="a9957-337">Когда **EndProperty** элемент используется для определения сопоставления функции изменения ассоциации концептуальной модели, он является дочерним элемента InsertFunction или элемент DeleteFunction.</span><span class="sxs-lookup"><span data-stu-id="a9957-337">When the **EndProperty** element is used to define the mapping for a modification function of a conceptual model association, it is a child of an InsertFunction element or DeleteFunction element.</span></span>

<span data-ttu-id="a9957-338">**EndProperty** элемент может иметь следующие дочерние элементы:</span><span class="sxs-lookup"><span data-stu-id="a9957-338">The **EndProperty** element can have the following child elements:</span></span>

-   <span data-ttu-id="a9957-339">ScalarProperty (ноль и более)</span><span class="sxs-lookup"><span data-stu-id="a9957-339">ScalarProperty (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="a9957-340">Применимые атрибуты</span><span class="sxs-lookup"><span data-stu-id="a9957-340">Applicable Attributes</span></span>

<span data-ttu-id="a9957-341">В следующей таблице описаны атрибуты, применимые к **EndProperty** элемент:</span><span class="sxs-lookup"><span data-stu-id="a9957-341">The following table describes the attributes that are applicable to the **EndProperty** element:</span></span>

| <span data-ttu-id="a9957-342">Имя атрибута</span><span class="sxs-lookup"><span data-stu-id="a9957-342">Attribute Name</span></span> | <span data-ttu-id="a9957-343">Необходимо</span><span class="sxs-lookup"><span data-stu-id="a9957-343">Is Required</span></span> | <span data-ttu-id="a9957-344">Значение</span><span class="sxs-lookup"><span data-stu-id="a9957-344">Value</span></span>                                                 |
|:---------------|:------------|:------------------------------------------------------|
| <span data-ttu-id="a9957-345">name</span><span class="sxs-lookup"><span data-stu-id="a9957-345">Name</span></span>           | <span data-ttu-id="a9957-346">Да</span><span class="sxs-lookup"><span data-stu-id="a9957-346">Yes</span></span>         | <span data-ttu-id="a9957-347">Имя сопоставляемого конца ассоциации.</span><span class="sxs-lookup"><span data-stu-id="a9957-347">The name of the association end that is being mapped.</span></span> |

### <a name="example"></a><span data-ttu-id="a9957-348">Пример</span><span class="sxs-lookup"><span data-stu-id="a9957-348">Example</span></span>

<span data-ttu-id="a9957-349">В следующем примере показан **AssociationSetMapping** элемента, в котором **FK\_курс\_отдел** ассоциации в концептуальной модели сопоставляется с **Курс** таблицы в базе данных.</span><span class="sxs-lookup"><span data-stu-id="a9957-349">The following example shows an **AssociationSetMapping** element in which the **FK\_Course\_Department** association in the conceptual model is mapped to the **Course** table in the database.</span></span> <span data-ttu-id="a9957-350">Сопоставления между свойствами типов сопоставлений и столбцами таблицы указываются в дочерних **EndProperty** элементов.</span><span class="sxs-lookup"><span data-stu-id="a9957-350">Mappings between association type properties and table columns are specified in child **EndProperty** elements.</span></span>

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

### <a name="example"></a><span data-ttu-id="a9957-351">Пример</span><span class="sxs-lookup"><span data-stu-id="a9957-351">Example</span></span>

<span data-ttu-id="a9957-352">В следующем примере показан **EndProperty** элемент сопоставления функции вставки и удаления ассоциации (**CourseInstructor**) с хранимыми процедурами в основной базе данных.</span><span class="sxs-lookup"><span data-stu-id="a9957-352">The following example shows the **EndProperty** element mapping the insert and delete functions of an association (**CourseInstructor**) to stored procedures in the underlying database.</span></span> <span data-ttu-id="a9957-353">Функции, с которыми выполняется сопоставление, объявляются в модели хранения.</span><span class="sxs-lookup"><span data-stu-id="a9957-353">The functions that are mapped to are declared in the storage model.</span></span>

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

## <a name="entitycontainermapping-element-msl"></a><span data-ttu-id="a9957-354">Элемент EntityContainerMapping (MSL)</span><span class="sxs-lookup"><span data-stu-id="a9957-354">EntityContainerMapping Element (MSL)</span></span>

<span data-ttu-id="a9957-355">**EntityContainerMapping** в язык определения сопоставлений (MSL) сопоставляет контейнер сущностей в концептуальной модели контейнер сущностей в модели хранения.</span><span class="sxs-lookup"><span data-stu-id="a9957-355">The **EntityContainerMapping** element in mapping specification language (MSL) maps the entity container in the conceptual model to the entity container in the storage model.</span></span> <span data-ttu-id="a9957-356">**EntityContainerMapping** элемент является дочерним элемента сопоставления.</span><span class="sxs-lookup"><span data-stu-id="a9957-356">The **EntityContainerMapping** element is a child of the Mapping element.</span></span>

<span data-ttu-id="a9957-357">**EntityContainerMapping** элемент может иметь следующие дочерние элементы (в указанном порядке):</span><span class="sxs-lookup"><span data-stu-id="a9957-357">The **EntityContainerMapping** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="a9957-358">EntitySetMapping (ноль и более)</span><span class="sxs-lookup"><span data-stu-id="a9957-358">EntitySetMapping (zero or more)</span></span>
-   <span data-ttu-id="a9957-359">AssociationSetMapping (ноль и более)</span><span class="sxs-lookup"><span data-stu-id="a9957-359">AssociationSetMapping (zero or more)</span></span>
-   <span data-ttu-id="a9957-360">FunctionImportMapping (ноль и более)</span><span class="sxs-lookup"><span data-stu-id="a9957-360">FunctionImportMapping (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="a9957-361">Применимые атрибуты</span><span class="sxs-lookup"><span data-stu-id="a9957-361">Applicable Attributes</span></span>

<span data-ttu-id="a9957-362">В следующей таблице описаны атрибуты, которые могут применяться к **EntityContainerMapping** элемент.</span><span class="sxs-lookup"><span data-stu-id="a9957-362">The following table describes the attributes that can be applied to the **EntityContainerMapping** element.</span></span>

| <span data-ttu-id="a9957-363">Имя атрибута</span><span class="sxs-lookup"><span data-stu-id="a9957-363">Attribute Name</span></span>            | <span data-ttu-id="a9957-364">Необходимо</span><span class="sxs-lookup"><span data-stu-id="a9957-364">Is Required</span></span> | <span data-ttu-id="a9957-365">Значение</span><span class="sxs-lookup"><span data-stu-id="a9957-365">Value</span></span>                                                                                                                                                                                                                                                    |
|:--------------------------|:------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="a9957-366">**StorageModelContainer**</span><span class="sxs-lookup"><span data-stu-id="a9957-366">**StorageModelContainer**</span></span> | <span data-ttu-id="a9957-367">Да</span><span class="sxs-lookup"><span data-stu-id="a9957-367">Yes</span></span>         | <span data-ttu-id="a9957-368">Имя контейнера сущностей модели хранения, с которым выполняется сопоставление.</span><span class="sxs-lookup"><span data-stu-id="a9957-368">The name of the storage model entity container that is being mapped.</span></span>                                                                                                                                                                                     |
| <span data-ttu-id="a9957-369">**CdmEntityContainer**</span><span class="sxs-lookup"><span data-stu-id="a9957-369">**CdmEntityContainer**</span></span>    | <span data-ttu-id="a9957-370">Да</span><span class="sxs-lookup"><span data-stu-id="a9957-370">Yes</span></span>         | <span data-ttu-id="a9957-371">Имя контейнера сущностей концептуальной модели, с которым выполняется сопоставление.</span><span class="sxs-lookup"><span data-stu-id="a9957-371">The name of the conceptual model entity container that is being mapped.</span></span>                                                                                                                                                                                  |
| <span data-ttu-id="a9957-372">**Generateupdateviews как**</span><span class="sxs-lookup"><span data-stu-id="a9957-372">**GenerateUpdateViews**</span></span>   | <span data-ttu-id="a9957-373">Нет</span><span class="sxs-lookup"><span data-stu-id="a9957-373">No</span></span>          | <span data-ttu-id="a9957-374">**Значение true,** или **False**.</span><span class="sxs-lookup"><span data-stu-id="a9957-374">**True** or **False**.</span></span> <span data-ttu-id="a9957-375">Если **False**, представления обновлений не создаются.</span><span class="sxs-lookup"><span data-stu-id="a9957-375">If **False**, no update views are generated.</span></span> <span data-ttu-id="a9957-376">Этот атрибут должно быть присвоено **False** при наличии сопоставление только для чтения, которое может стать недействительным, так как данные могут быть не выполнить обратное преобразование успешно.</span><span class="sxs-lookup"><span data-stu-id="a9957-376">This attribute should be set to **False** when you have a read-only mapping that would be invalid because data may not round-trip successfully.</span></span> <br/> <span data-ttu-id="a9957-377">Значение по умолчанию — **True**.</span><span class="sxs-lookup"><span data-stu-id="a9957-377">The default value is **True**.</span></span> |

### <a name="example"></a><span data-ttu-id="a9957-378">Пример</span><span class="sxs-lookup"><span data-stu-id="a9957-378">Example</span></span>

<span data-ttu-id="a9957-379">В следующем примере показан **EntityContainerMapping** сопоставляет **SchoolModelEntities** контейнера (контейнер сущностей концептуальной модели) для  **SchoolModelStoreContainer** контейнера (контейнер сущностей модели хранения):</span><span class="sxs-lookup"><span data-stu-id="a9957-379">The following example shows an **EntityContainerMapping** element that maps the **SchoolModelEntities** container (the conceptual model entity container) to the **SchoolModelStoreContainer** container (the storage model entity container):</span></span>

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

## <a name="entitysetmapping-element-msl"></a><span data-ttu-id="a9957-380">Элемент EntitySetMapping (MSL)</span><span class="sxs-lookup"><span data-stu-id="a9957-380">EntitySetMapping Element (MSL)</span></span>

<span data-ttu-id="a9957-381">**EntitySetMapping** задает элемент в языке MSL сопоставляет все типы в сущности концептуальной модели с наборами сущностей в модели хранения.</span><span class="sxs-lookup"><span data-stu-id="a9957-381">The **EntitySetMapping** element in mapping specification language (MSL) maps all types in a conceptual model entity set to entity sets in the storage model.</span></span> <span data-ttu-id="a9957-382">Набор сущностей в концептуальной модели — это логический контейнер для экземпляров сущностей одного типа (и производных типов).</span><span class="sxs-lookup"><span data-stu-id="a9957-382">An entity set in the conceptual model is a logical container for instances of entities of the same type (and derived types).</span></span> <span data-ttu-id="a9957-383">Набор сущностей в модели хранения представляет таблицу или представление в основной базе данных.</span><span class="sxs-lookup"><span data-stu-id="a9957-383">An entity set in the storage model represents a table or view in the underlying database.</span></span> <span data-ttu-id="a9957-384">Набор сущностей концептуальной модели задается значение **имя** атрибут **EntitySetMapping** элемент.</span><span class="sxs-lookup"><span data-stu-id="a9957-384">The conceptual model entity set is specified by the value of the **Name** attribute of the **EntitySetMapping** element.</span></span> <span data-ttu-id="a9957-385">Таблица или представление определяется **StoreEntitySet** атрибут в каждый дочерний элемент MappingFragment или на **EntitySetMapping** сам элемент.</span><span class="sxs-lookup"><span data-stu-id="a9957-385">The mapped-to table or view is specified by the **StoreEntitySet** attribute in each child MappingFragment element or in the **EntitySetMapping** element itself.</span></span>

<span data-ttu-id="a9957-386">**EntitySetMapping** элемент может иметь следующие дочерние элементы:</span><span class="sxs-lookup"><span data-stu-id="a9957-386">The **EntitySetMapping** element can have the following child elements:</span></span>

-   <span data-ttu-id="a9957-387">EntityTypeMapping (ноль и более)</span><span class="sxs-lookup"><span data-stu-id="a9957-387">EntityTypeMapping (zero or more)</span></span>
-   <span data-ttu-id="a9957-388">QueryView (ноль или один)</span><span class="sxs-lookup"><span data-stu-id="a9957-388">QueryView (zero or one)</span></span>
-   <span data-ttu-id="a9957-389">MappingFragment (ноль и более)</span><span class="sxs-lookup"><span data-stu-id="a9957-389">MappingFragment (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="a9957-390">Применимые атрибуты</span><span class="sxs-lookup"><span data-stu-id="a9957-390">Applicable Attributes</span></span>

<span data-ttu-id="a9957-391">В следующей таблице описаны атрибуты, которые могут применяться к **EntitySetMapping** элемент.</span><span class="sxs-lookup"><span data-stu-id="a9957-391">The following table describes the attributes that can be applied to the **EntitySetMapping** element.</span></span>

| <span data-ttu-id="a9957-392">Имя атрибута</span><span class="sxs-lookup"><span data-stu-id="a9957-392">Attribute Name</span></span>           | <span data-ttu-id="a9957-393">Необходимо</span><span class="sxs-lookup"><span data-stu-id="a9957-393">Is Required</span></span> | <span data-ttu-id="a9957-394">Значение</span><span class="sxs-lookup"><span data-stu-id="a9957-394">Value</span></span>                                                                                                                                                                                                                         |
|:-------------------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="a9957-395">**Name**</span><span class="sxs-lookup"><span data-stu-id="a9957-395">**Name**</span></span>                 | <span data-ttu-id="a9957-396">Да</span><span class="sxs-lookup"><span data-stu-id="a9957-396">Yes</span></span>         | <span data-ttu-id="a9957-397">Имя сопоставляемого набора сущностей концептуальной модели.</span><span class="sxs-lookup"><span data-stu-id="a9957-397">The name of the conceptual model entity set that is being mapped.</span></span>                                                                                                                                                             |
| <span data-ttu-id="a9957-398">**TypeName** **1**</span><span class="sxs-lookup"><span data-stu-id="a9957-398">**TypeName** **1**</span></span>       | <span data-ttu-id="a9957-399">Нет</span><span class="sxs-lookup"><span data-stu-id="a9957-399">No</span></span>          | <span data-ttu-id="a9957-400">Имя сопоставляемого типа сущности концептуальной модели.</span><span class="sxs-lookup"><span data-stu-id="a9957-400">The name of the conceptual model entity type that is being mapped.</span></span>                                                                                                                                                            |
| <span data-ttu-id="a9957-401">**StoreEntitySet** **1**</span><span class="sxs-lookup"><span data-stu-id="a9957-401">**StoreEntitySet** **1**</span></span> | <span data-ttu-id="a9957-402">Нет</span><span class="sxs-lookup"><span data-stu-id="a9957-402">No</span></span>          | <span data-ttu-id="a9957-403">Имя сопоставляемого набора сущностей модели хранения.</span><span class="sxs-lookup"><span data-stu-id="a9957-403">The name of the storage model entity set that is being mapped to.</span></span>                                                                                                                                                             |
| <span data-ttu-id="a9957-404">**MakeColumnsDistinct**</span><span class="sxs-lookup"><span data-stu-id="a9957-404">**MakeColumnsDistinct**</span></span>  | <span data-ttu-id="a9957-405">Нет</span><span class="sxs-lookup"><span data-stu-id="a9957-405">No</span></span>          | <span data-ttu-id="a9957-406">**Значение true,** или **False** в зависимости от того, возвращаются только уникальные строки.</span><span class="sxs-lookup"><span data-stu-id="a9957-406">**True** or **False** depending on whether only distinct rows are returned.</span></span> <br/> <span data-ttu-id="a9957-407">Если этот атрибут имеет значение **True**, **generateupdateviews как** атрибут элемента EntityContainerMapping должно быть присвоено **False**.</span><span class="sxs-lookup"><span data-stu-id="a9957-407">If this attribute is set to **True**, the **GenerateUpdateViews** attribute of the EntityContainerMapping element must be set to **False**.</span></span> |

 

<span data-ttu-id="a9957-408">**1** **TypeName** и **StoreEntitySet** атрибуты можно использовать вместо дочерние элементы EntityTypeMapping и MappingFragment для сопоставления один тип сущности с одной таблицей.</span><span class="sxs-lookup"><span data-stu-id="a9957-408">**1** The **TypeName** and **StoreEntitySet** attributes can be used in place of the EntityTypeMapping and MappingFragment child elements to map a single entity type to a single table.</span></span>

### <a name="example"></a><span data-ttu-id="a9957-409">Пример</span><span class="sxs-lookup"><span data-stu-id="a9957-409">Example</span></span>

<span data-ttu-id="a9957-410">В следующем примере показан **EntitySetMapping** элемент, сопоставляющий три типа (базового типа и два производных типа) в **курсы** набор сущностей в концептуальной модели с тремя разными таблицами основной базе данных.</span><span class="sxs-lookup"><span data-stu-id="a9957-410">The following example shows an **EntitySetMapping** element that maps three types (a base type and two derived types) in the **Courses** entity set of the conceptual model to three different tables in the underlying database.</span></span> <span data-ttu-id="a9957-411">Таблицы заданы **StoreEntitySet** атрибута в каждом **MappingFragment** элемент.</span><span class="sxs-lookup"><span data-stu-id="a9957-411">The tables are specified by the **StoreEntitySet** attribute in each **MappingFragment** element.</span></span>

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

## <a name="entitytypemapping-element-msl"></a><span data-ttu-id="a9957-412">Элемент EntityTypeMapping (MSL)</span><span class="sxs-lookup"><span data-stu-id="a9957-412">EntityTypeMapping Element (MSL)</span></span>

<span data-ttu-id="a9957-413">**EntityTypeMapping** элемент в язык определения сопоставлений (MSL) определяет сопоставление между типом сущности в концептуальной модели и таблицы или представления в основной базе данных.</span><span class="sxs-lookup"><span data-stu-id="a9957-413">The **EntityTypeMapping** element in mapping specification language (MSL) defines the mapping between an entity type in the conceptual model and tables or views in the underlying database.</span></span> <span data-ttu-id="a9957-414">Сведения о типах сущностей концептуальной модели и базовых таблиц базы данных или представления см. в разделе элемент EntityType (CSDL) и элемент EntitySet (SSDL).</span><span class="sxs-lookup"><span data-stu-id="a9957-414">For information about conceptual model entity types and underlying database tables or views, see EntityType Element (CSDL) and EntitySet Element (SSDL).</span></span> <span data-ttu-id="a9957-415">Определяется тип сущности концептуальной модели, с которым выполняется сопоставление **TypeName** атрибут **EntityTypeMapping** элемент.</span><span class="sxs-lookup"><span data-stu-id="a9957-415">The conceptual model entity type that is being mapped is specified by the **TypeName** attribute of the **EntityTypeMapping** element.</span></span> <span data-ttu-id="a9957-416">Таблицы или представления, с которым выполняется сопоставление определяется **StoreEntitySet** атрибут MappingFragment дочернего элемента.</span><span class="sxs-lookup"><span data-stu-id="a9957-416">The table or view that is being mapped is specified by the **StoreEntitySet** attribute of the child MappingFragment element.</span></span>

<span data-ttu-id="a9957-417">ModificationFunctionMapping дочерний элемент может использоваться для сопоставления вставки, обновления или удаления функций типов сущностей с хранимыми процедурами в базе данных.</span><span class="sxs-lookup"><span data-stu-id="a9957-417">The ModificationFunctionMapping child element can be used to map the insert, update, or delete functions of entity types to stored procedures in the database.</span></span>

<span data-ttu-id="a9957-418">**EntityTypeMapping** элемент может иметь следующие дочерние элементы:</span><span class="sxs-lookup"><span data-stu-id="a9957-418">The **EntityTypeMapping** element can have the following child elements:</span></span>

-   <span data-ttu-id="a9957-419">MappingFragment (ноль и более)</span><span class="sxs-lookup"><span data-stu-id="a9957-419">MappingFragment (zero or more)</span></span>
-   <span data-ttu-id="a9957-420">ModificationFunctionMapping (ноль или один)</span><span class="sxs-lookup"><span data-stu-id="a9957-420">ModificationFunctionMapping (zero or one)</span></span>
-   <span data-ttu-id="a9957-421">ScalarProperty</span><span class="sxs-lookup"><span data-stu-id="a9957-421">ScalarProperty</span></span>
-   <span data-ttu-id="a9957-422">Условие</span><span class="sxs-lookup"><span data-stu-id="a9957-422">Condition</span></span>

> [!NOTE]
> <span data-ttu-id="a9957-423">**MappingFragment** и **ModificationFunctionMapping** элементы не могут быть дочерними элементами элемента **EntityTypeMapping** элемент, в то же время.</span><span class="sxs-lookup"><span data-stu-id="a9957-423">**MappingFragment** and **ModificationFunctionMapping** elements cannot be child elements of the **EntityTypeMapping** element at the same time.</span></span>


> [!NOTE]
> <span data-ttu-id="a9957-424">**ScalarProperty** и **условие** элементов может быть только дочерние элементы **EntityTypeMapping** элемент, когда он используется внутри элемента FunctionImportMapping.</span><span class="sxs-lookup"><span data-stu-id="a9957-424">The **ScalarProperty** and **Condition** elements can only be child elements of the **EntityTypeMapping** element when it is used within a FunctionImportMapping element.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="a9957-425">Применимые атрибуты</span><span class="sxs-lookup"><span data-stu-id="a9957-425">Applicable Attributes</span></span>

<span data-ttu-id="a9957-426">В следующей таблице описаны атрибуты, которые могут применяться к **EntityTypeMapping** элемент.</span><span class="sxs-lookup"><span data-stu-id="a9957-426">The following table describes the attributes that can be applied to the **EntityTypeMapping** element.</span></span>

| <span data-ttu-id="a9957-427">Имя атрибута</span><span class="sxs-lookup"><span data-stu-id="a9957-427">Attribute Name</span></span> | <span data-ttu-id="a9957-428">Необходимо</span><span class="sxs-lookup"><span data-stu-id="a9957-428">Is Required</span></span> | <span data-ttu-id="a9957-429">Значение</span><span class="sxs-lookup"><span data-stu-id="a9957-429">Value</span></span>                                                                                                                                                                                                |
|:---------------|:------------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="a9957-430">**Имя типа**</span><span class="sxs-lookup"><span data-stu-id="a9957-430">**TypeName**</span></span>   | <span data-ttu-id="a9957-431">Да</span><span class="sxs-lookup"><span data-stu-id="a9957-431">Yes</span></span>         | <span data-ttu-id="a9957-432">Имя с указанием пространства имен типа сущности концептуальной модели, с которым выполняется сопоставление.</span><span class="sxs-lookup"><span data-stu-id="a9957-432">The namespace-qualified name of the conceptual model entity type that is being mapped.</span></span> <br/> <span data-ttu-id="a9957-433">Если тип является абстрактным или производным типом, значение должно быть равно `IsOfType(Namespace-qualified_type_name)`.</span><span class="sxs-lookup"><span data-stu-id="a9957-433">If the type is abstract or a derived type, the value must be `IsOfType(Namespace-qualified_type_name)`.</span></span> |

### <a name="example"></a><span data-ttu-id="a9957-434">Пример</span><span class="sxs-lookup"><span data-stu-id="a9957-434">Example</span></span>

<span data-ttu-id="a9957-435">В следующем примере показано элемент EntitySetMapping с двумя дочерними **EntityTypeMapping** элементов.</span><span class="sxs-lookup"><span data-stu-id="a9957-435">The following example shows an EntitySetMapping element with two child **EntityTypeMapping** elements.</span></span> <span data-ttu-id="a9957-436">В первом **EntityTypeMapping** элемент, **SchoolModel.Person** сопоставляется с типом сущности **Person** таблицы.</span><span class="sxs-lookup"><span data-stu-id="a9957-436">In the first **EntityTypeMapping** element, the **SchoolModel.Person** entity type is mapped to the **Person** table.</span></span> <span data-ttu-id="a9957-437">Во втором **EntityTypeMapping** элемент, поэтому функциональные возможности обновления **SchoolModel.Person** тип сопоставлен с хранимой процедурой, **UpdatePerson**, в базе данных .</span><span class="sxs-lookup"><span data-stu-id="a9957-437">In the second **EntityTypeMapping** element, the update functionality of the **SchoolModel.Person** type is mapped to a stored procedure, **UpdatePerson**, in the database.</span></span>

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

### <a name="example"></a><span data-ttu-id="a9957-438">Пример</span><span class="sxs-lookup"><span data-stu-id="a9957-438">Example</span></span>

<span data-ttu-id="a9957-439">В следующем примере показано сопоставление иерархии типа, в которой корневой тип является абстрактным.</span><span class="sxs-lookup"><span data-stu-id="a9957-439">The next example shows the mapping of a type hierarchy in which the root type is abstract.</span></span> <span data-ttu-id="a9957-440">Обратите внимание на использование `IsOfType` синтаксис **TypeName** атрибуты.</span><span class="sxs-lookup"><span data-stu-id="a9957-440">Note the use of the `IsOfType` syntax for the **TypeName** attributes.</span></span>

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

## <a name="functionimportmapping-element-msl"></a><span data-ttu-id="a9957-441">Элемент FunctionImportMapping (MSL)</span><span class="sxs-lookup"><span data-stu-id="a9957-441">FunctionImportMapping Element (MSL)</span></span>

<span data-ttu-id="a9957-442">**FunctionImportMapping** элемент в язык определения сопоставлений (MSL) определяет сопоставление между импортом функции в концептуальной модели и хранимой процедурой или функцией в основной базе данных.</span><span class="sxs-lookup"><span data-stu-id="a9957-442">The **FunctionImportMapping** element in mapping specification language (MSL) defines the mapping between a function import in the conceptual model and a stored procedure or function in the underlying database.</span></span> <span data-ttu-id="a9957-443">Импорт функций должен быть объявлен в концептуальной модели, а хранимые процедуры должны быть объявлены в модели хранения.</span><span class="sxs-lookup"><span data-stu-id="a9957-443">Function imports must be declared in the conceptual model and stored procedures must be declared in the storage model.</span></span> <span data-ttu-id="a9957-444">Дополнительные сведения см. в разделе элемент FunctionImport (CSDL) и функция элемент (SSDL).</span><span class="sxs-lookup"><span data-stu-id="a9957-444">For more information, see FunctionImport Element (CSDL) and Function Element (SSDL).</span></span>

> [!NOTE]
> <span data-ttu-id="a9957-445">По умолчанию импорт функции возвращает тип сущности концептуальной модели или сложный тип. В этом случае имена столбцов, которые вернула основная хранимая процедура, должны точно соответствовать именам свойств в типе концептуальной модели.</span><span class="sxs-lookup"><span data-stu-id="a9957-445">By default, if a function import returns a conceptual model entity type or complex type, then the names of the columns returned by the underlying stored procedure must exactly match the names of the properties on the conceptual model type.</span></span> <span data-ttu-id="a9957-446">Если имена столбцов не точно совпадают с именами свойств, сопоставление должен быть определен в элементе ResultMapping.</span><span class="sxs-lookup"><span data-stu-id="a9957-446">If the column names do not exactly match the property names, the mapping must be defined in a ResultMapping element.</span></span>

<span data-ttu-id="a9957-447">**FunctionImportMapping** элемент может иметь следующие дочерние элементы:</span><span class="sxs-lookup"><span data-stu-id="a9957-447">The **FunctionImportMapping** element can have the following child elements:</span></span>

-   <span data-ttu-id="a9957-448">ResultMapping (ноль и более)</span><span class="sxs-lookup"><span data-stu-id="a9957-448">ResultMapping (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="a9957-449">Применимые атрибуты</span><span class="sxs-lookup"><span data-stu-id="a9957-449">Applicable Attributes</span></span>

<span data-ttu-id="a9957-450">В следующей таблице описаны атрибуты, применимые к **FunctionImportMapping** элемент:</span><span class="sxs-lookup"><span data-stu-id="a9957-450">The following table describes the attributes that are applicable to the **FunctionImportMapping** element:</span></span>

| <span data-ttu-id="a9957-451">Имя атрибута</span><span class="sxs-lookup"><span data-stu-id="a9957-451">Attribute Name</span></span>         | <span data-ttu-id="a9957-452">Необходимо</span><span class="sxs-lookup"><span data-stu-id="a9957-452">Is Required</span></span> | <span data-ttu-id="a9957-453">Значение</span><span class="sxs-lookup"><span data-stu-id="a9957-453">Value</span></span>                                                                                   |
|:-----------------------|:------------|:----------------------------------------------------------------------------------------|
| <span data-ttu-id="a9957-454">**FunctionImportName**</span><span class="sxs-lookup"><span data-stu-id="a9957-454">**FunctionImportName**</span></span> | <span data-ttu-id="a9957-455">Да</span><span class="sxs-lookup"><span data-stu-id="a9957-455">Yes</span></span>         | <span data-ttu-id="a9957-456">Имя импорта функции в концептуальной модели, с которым выполняется сопоставление.</span><span class="sxs-lookup"><span data-stu-id="a9957-456">The name of the function import in the conceptual model that is being mapped.</span></span>           |
| <span data-ttu-id="a9957-457">**FunctionName**</span><span class="sxs-lookup"><span data-stu-id="a9957-457">**FunctionName**</span></span>       | <span data-ttu-id="a9957-458">Да</span><span class="sxs-lookup"><span data-stu-id="a9957-458">Yes</span></span>         | <span data-ttu-id="a9957-459">Имя импорта функции в модели хранения из полного пространства имен, с которым выполняется сопоставление.</span><span class="sxs-lookup"><span data-stu-id="a9957-459">The namespace-qualified name of the function in the storage model that is being mapped.</span></span> |

### <a name="example"></a><span data-ttu-id="a9957-460">Пример</span><span class="sxs-lookup"><span data-stu-id="a9957-460">Example</span></span>

<span data-ttu-id="a9957-461">Следующий пример основан на модели School.</span><span class="sxs-lookup"><span data-stu-id="a9957-461">The following example is based on the School model.</span></span> <span data-ttu-id="a9957-462">Рассмотрим следующую функцию в модели хранения:</span><span class="sxs-lookup"><span data-stu-id="a9957-462">Consider the following function in the storage model:</span></span>

``` xml
 <Function Name="GetStudentGrades" Aggregate="false"
           BuiltIn="false" NiladicFunction="false"
           IsComposable="false" ParameterTypeSemantics="AllowImplicitConversion"
           Schema="dbo">
   <Parameter Name="StudentID" Type="int" Mode="In" />
 </Function>
```

<span data-ttu-id="a9957-463">Также рассмотрим данный импорт функции в концептуальной модели:</span><span class="sxs-lookup"><span data-stu-id="a9957-463">Also consider this function import in the conceptual model:</span></span>

``` xml
 <FunctionImport Name="GetStudentGrades" EntitySet="StudentGrades"
                 ReturnType="Collection(SchoolModel.StudentGrade)">
   <Parameter Name="StudentID" Mode="In" Type="Int32" />
 </FunctionImport>
```

<span data-ttu-id="a9957-464">В следующем примере показан **FunctionImportMapping** элемент, используемый для сопоставления функции и импорта функции друг с другом:</span><span class="sxs-lookup"><span data-stu-id="a9957-464">The following example show a **FunctionImportMapping** element used to map the function and function import above to each other:</span></span>

``` xml
 <FunctionImportMapping FunctionImportName="GetStudentGrades"
                        FunctionName="SchoolModel.Store.GetStudentGrades" />
```
 
## <a name="insertfunction-element-msl"></a><span data-ttu-id="a9957-465">Элемент InsertFunction (MSL)</span><span class="sxs-lookup"><span data-stu-id="a9957-465">InsertFunction Element (MSL)</span></span>

<span data-ttu-id="a9957-466">**InsertFunction** в язык определения сопоставлений (MSL) сопоставляет функцию вставки типа сущности или ассоциации в концептуальной модели с хранимой процедурой в основной базе данных.</span><span class="sxs-lookup"><span data-stu-id="a9957-466">The **InsertFunction** element in mapping specification language (MSL) maps the insert function of an entity type or association in the conceptual model to a stored procedure in the underlying database.</span></span> <span data-ttu-id="a9957-467">Хранимые процедуры, с которыми сопоставляются функции изменения, должны объявляться в модели хранения.</span><span class="sxs-lookup"><span data-stu-id="a9957-467">Stored procedures to which modification functions are mapped must be declared in the storage model.</span></span> <span data-ttu-id="a9957-468">Дополнительные сведения см. в разделе элемента функция (SSDL).</span><span class="sxs-lookup"><span data-stu-id="a9957-468">For more information, see Function Element (SSDL).</span></span>

> [!NOTE]
> <span data-ttu-id="a9957-469">Если не выполнено сопоставление всех трех вставки, обновления или удаления типов сущностей с хранимыми процедурами, то несопоставленные операции завершится ошибкой, если во время выполнения и UpdateException возникает исключение.</span><span class="sxs-lookup"><span data-stu-id="a9957-469">If you do not map all three of the insert, update, or delete operations of a entity type to stored procedures, the unmapped operations will fail if executed at runtime and an UpdateException is thrown.</span></span>

<span data-ttu-id="a9957-470">**InsertFunction** элемент может быть дочерним элементом ModificationFunctionMapping и применить к EntityTypeMapping-элемент или элемент AssociationSetMapping.</span><span class="sxs-lookup"><span data-stu-id="a9957-470">The **InsertFunction** element can be a child of the ModificationFunctionMapping element and applied to the EntityTypeMapping element or the AssociationSetMapping element.</span></span>

### <a name="insertfunction-applied-to-entitytypemapping"></a><span data-ttu-id="a9957-471">Элемент InsertFunction, применяемый к EntityTypeMapping</span><span class="sxs-lookup"><span data-stu-id="a9957-471">InsertFunction Applied to EntityTypeMapping</span></span>

<span data-ttu-id="a9957-472">При применении к элементу EntityTypeMapping **InsertFunction** сопоставляет функцию вставки типа сущности в концептуальной модели с хранимой процедурой.</span><span class="sxs-lookup"><span data-stu-id="a9957-472">When applied to the EntityTypeMapping element, the **InsertFunction** element maps the insert function of an entity type in the conceptual model to a stored procedure.</span></span>

<span data-ttu-id="a9957-473">**InsertFunction** элемент может иметь следующие дочерние элементы при применении к **EntityTypeMapping** элемент:</span><span class="sxs-lookup"><span data-stu-id="a9957-473">The **InsertFunction** element can have the following child elements when applied to an **EntityTypeMapping** element:</span></span>

-   <span data-ttu-id="a9957-474">AssociationEnd (ноль и более)</span><span class="sxs-lookup"><span data-stu-id="a9957-474">AssociationEnd (zero or more)</span></span>
-   <span data-ttu-id="a9957-475">ComplexProperty (ноль и более)</span><span class="sxs-lookup"><span data-stu-id="a9957-475">ComplexProperty (zero or more)</span></span>
-   <span data-ttu-id="a9957-476">ResultBinding (ноль или один)</span><span class="sxs-lookup"><span data-stu-id="a9957-476">ResultBinding (zero or one)</span></span>
-   <span data-ttu-id="a9957-477">ScarlarProperty (ноль и более)</span><span class="sxs-lookup"><span data-stu-id="a9957-477">ScarlarProperty (zero or more)</span></span>

#### <a name="applicable-attributes"></a><span data-ttu-id="a9957-478">Применимые атрибуты</span><span class="sxs-lookup"><span data-stu-id="a9957-478">Applicable Attributes</span></span>

<span data-ttu-id="a9957-479">В следующей таблице описаны атрибуты, которые могут применяться к **InsertFunction** элемент при применении к **EntityTypeMapping** элемент.</span><span class="sxs-lookup"><span data-stu-id="a9957-479">The following table describes the attributes that can be applied to the **InsertFunction** element when applied to an **EntityTypeMapping** element.</span></span>

| <span data-ttu-id="a9957-480">Имя атрибута</span><span class="sxs-lookup"><span data-stu-id="a9957-480">Attribute Name</span></span>            | <span data-ttu-id="a9957-481">Необходимо</span><span class="sxs-lookup"><span data-stu-id="a9957-481">Is Required</span></span> | <span data-ttu-id="a9957-482">Значение</span><span class="sxs-lookup"><span data-stu-id="a9957-482">Value</span></span>                                                                                                                                                    |
|:--------------------------|:------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="a9957-483">**FunctionName**</span><span class="sxs-lookup"><span data-stu-id="a9957-483">**FunctionName**</span></span>          | <span data-ttu-id="a9957-484">Да</span><span class="sxs-lookup"><span data-stu-id="a9957-484">Yes</span></span>         | <span data-ttu-id="a9957-485">Имя хранимой процедуры (с указанием пространства имен), с которой сопоставляется функция вставки.</span><span class="sxs-lookup"><span data-stu-id="a9957-485">The namespace-qualified name of the stored procedure to which the insert function is mapped.</span></span> <span data-ttu-id="a9957-486">Хранимая процедура должна объявляться в модели хранения.</span><span class="sxs-lookup"><span data-stu-id="a9957-486">The stored procedure must be declared in the storage model.</span></span> |
| <span data-ttu-id="a9957-487">**RowsAffectedParameter**</span><span class="sxs-lookup"><span data-stu-id="a9957-487">**RowsAffectedParameter**</span></span> | <span data-ttu-id="a9957-488">Нет</span><span class="sxs-lookup"><span data-stu-id="a9957-488">No</span></span>          | <span data-ttu-id="a9957-489">Имя выходного параметра, возвращающего количество обработанных строк.</span><span class="sxs-lookup"><span data-stu-id="a9957-489">The name of the output parameter that returns the number of affected rows.</span></span>                                                                               |

#### <a name="example"></a><span data-ttu-id="a9957-490">Пример</span><span class="sxs-lookup"><span data-stu-id="a9957-490">Example</span></span>

<span data-ttu-id="a9957-491">В следующем примере, основана на модели School и показывает **InsertFunction** элемент, используемый для сопоставления функции вставки типа сущности Person в **InsertPerson** хранимой процедуры.</span><span class="sxs-lookup"><span data-stu-id="a9957-491">The following example is based on the School model and shows the **InsertFunction** element used to map insert function of the Person entity type to the **InsertPerson** stored procedure.</span></span> <span data-ttu-id="a9957-492">**InsertPerson** хранимая процедура объявлена в модели хранения.</span><span class="sxs-lookup"><span data-stu-id="a9957-492">The **InsertPerson** stored procedure is declared in the storage model.</span></span>

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
### <a name="insertfunction-applied-to-associationsetmapping"></a><span data-ttu-id="a9957-493">Элемент InsertFunction, применяемый к AssociationSetMapping</span><span class="sxs-lookup"><span data-stu-id="a9957-493">InsertFunction Applied to AssociationSetMapping</span></span>

<span data-ttu-id="a9957-494">При применении к элементу AssociationSetMapping **InsertFunction** сопоставляет функцию вставки ассоциации в концептуальной модели с хранимой процедурой.</span><span class="sxs-lookup"><span data-stu-id="a9957-494">When applied to the AssociationSetMapping element, the **InsertFunction** element maps the insert function of an association in the conceptual model to a stored procedure.</span></span>

<span data-ttu-id="a9957-495">**InsertFunction** элемент может иметь следующие дочерние элементы при применении к **AssociationSetMapping** элемент:</span><span class="sxs-lookup"><span data-stu-id="a9957-495">The **InsertFunction** element can have the following child elements when applied to the **AssociationSetMapping** element:</span></span>

-   <span data-ttu-id="a9957-496">EndProperty</span><span class="sxs-lookup"><span data-stu-id="a9957-496">EndProperty</span></span>

#### <a name="applicable-attributes"></a><span data-ttu-id="a9957-497">Применимые атрибуты</span><span class="sxs-lookup"><span data-stu-id="a9957-497">Applicable Attributes</span></span>

<span data-ttu-id="a9957-498">В следующей таблице описаны атрибуты, которые могут применяться к **InsertFunction** элемента при его применении к **AssociationSetMapping** элемент.</span><span class="sxs-lookup"><span data-stu-id="a9957-498">The following table describes the attributes that can be applied to the **InsertFunction** element when it is applied to the **AssociationSetMapping** element.</span></span>

| <span data-ttu-id="a9957-499">Имя атрибута</span><span class="sxs-lookup"><span data-stu-id="a9957-499">Attribute Name</span></span>            | <span data-ttu-id="a9957-500">Необходимо</span><span class="sxs-lookup"><span data-stu-id="a9957-500">Is Required</span></span> | <span data-ttu-id="a9957-501">Значение</span><span class="sxs-lookup"><span data-stu-id="a9957-501">Value</span></span>                                                                                                                                                    |
|:--------------------------|:------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="a9957-502">**FunctionName**</span><span class="sxs-lookup"><span data-stu-id="a9957-502">**FunctionName**</span></span>          | <span data-ttu-id="a9957-503">Да</span><span class="sxs-lookup"><span data-stu-id="a9957-503">Yes</span></span>         | <span data-ttu-id="a9957-504">Имя хранимой процедуры (с указанием пространства имен), с которой сопоставляется функция вставки.</span><span class="sxs-lookup"><span data-stu-id="a9957-504">The namespace-qualified name of the stored procedure to which the insert function is mapped.</span></span> <span data-ttu-id="a9957-505">Хранимая процедура должна объявляться в модели хранения.</span><span class="sxs-lookup"><span data-stu-id="a9957-505">The stored procedure must be declared in the storage model.</span></span> |
| <span data-ttu-id="a9957-506">**RowsAffectedParameter**</span><span class="sxs-lookup"><span data-stu-id="a9957-506">**RowsAffectedParameter**</span></span> | <span data-ttu-id="a9957-507">Нет</span><span class="sxs-lookup"><span data-stu-id="a9957-507">No</span></span>          | <span data-ttu-id="a9957-508">Имя выходного параметра, возвращающего количество обработанных строк.</span><span class="sxs-lookup"><span data-stu-id="a9957-508">The name of the output parameter that returns the number of rows affected.</span></span>                                                                               |

#### <a name="example"></a><span data-ttu-id="a9957-509">Пример</span><span class="sxs-lookup"><span data-stu-id="a9957-509">Example</span></span>

<span data-ttu-id="a9957-510">В следующем примере, основана на модели School и показывает **InsertFunction** элемент, используемый для сопоставления функции вставки **CourseInstructor** взаимосвязи  **InsertCourseInstructor** хранимой процедуры.</span><span class="sxs-lookup"><span data-stu-id="a9957-510">The following example is based on the School model and shows the **InsertFunction** element used to map insert function of the **CourseInstructor** association to the **InsertCourseInstructor** stored procedure.</span></span> <span data-ttu-id="a9957-511">**InsertCourseInstructor** хранимая процедура объявлена в модели хранения.</span><span class="sxs-lookup"><span data-stu-id="a9957-511">The **InsertCourseInstructor** stored procedure is declared in the storage model.</span></span>

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

## <a name="mapping-element-msl"></a><span data-ttu-id="a9957-512">Элемент сопоставления (MSL)</span><span class="sxs-lookup"><span data-stu-id="a9957-512">Mapping Element (MSL)</span></span>

<span data-ttu-id="a9957-513">**Сопоставление** элемент в язык определения сопоставлений (MSL) содержит сведения для сопоставления объектов, определенных в концептуальной модели в базу данных (как описано в модели хранения).</span><span class="sxs-lookup"><span data-stu-id="a9957-513">The **Mapping** element in mapping specification language (MSL) contains information for mapping objects that are defined in a conceptual model to a database (as described in a storage model).</span></span> <span data-ttu-id="a9957-514">Дополнительные сведения см. в разделе спецификации языка CSDL и SSDL спецификации.</span><span class="sxs-lookup"><span data-stu-id="a9957-514">For more information, see CSDL Specification and SSDL Specification.</span></span>

<span data-ttu-id="a9957-515">**Сопоставление** элемент является корневым элементом спецификации сопоставления.</span><span class="sxs-lookup"><span data-stu-id="a9957-515">The **Mapping** element is the root element for a mapping specification.</span></span> <span data-ttu-id="a9957-516">Пространство имен XML для спецификаций сопоставлений http://schemas.microsoft.com/ado/2009/11/mapping/cs.</span><span class="sxs-lookup"><span data-stu-id="a9957-516">The XML namespace for mapping specifications is http://schemas.microsoft.com/ado/2009/11/mapping/cs.</span></span>

<span data-ttu-id="a9957-517">У элемента Mapping могут быть следующие дочерние элементы (в порядке перечисления):</span><span class="sxs-lookup"><span data-stu-id="a9957-517">The mapping element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="a9957-518">Псевдоним (ноль и более)</span><span class="sxs-lookup"><span data-stu-id="a9957-518">Alias (zero or more)</span></span>
-   <span data-ttu-id="a9957-519">EntityContainerMapping (ровно один элемент)</span><span class="sxs-lookup"><span data-stu-id="a9957-519">EntityContainerMapping (exactly one)</span></span>

<span data-ttu-id="a9957-520">Имена типов концептуальной модели и модели хранения, которые упоминаются в языке MSL, должны указываться вместе с именами соответствующих пространств имен.</span><span class="sxs-lookup"><span data-stu-id="a9957-520">Names of conceptual and storage model types that are referenced in MSL must be qualified by their respective namespace names.</span></span> <span data-ttu-id="a9957-521">Сведения об имени пространства имен концептуальной модели см. в разделе элемент схемы (CSDL).</span><span class="sxs-lookup"><span data-stu-id="a9957-521">For information about the conceptual model namespace name, see Schema Element (CSDL).</span></span> <span data-ttu-id="a9957-522">Сведения об имени пространства имен модели хранения см. в разделе элемент схемы (SSDL).</span><span class="sxs-lookup"><span data-stu-id="a9957-522">For information about the storage model namespace name, see Schema Element (SSDL).</span></span> <span data-ttu-id="a9957-523">Элемент Alias можно определить псевдонимы для пространств имен, используемых в языке MSL.</span><span class="sxs-lookup"><span data-stu-id="a9957-523">Aliases for namespaces that are used in MSL can be defined with the Alias element.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="a9957-524">Применимые атрибуты</span><span class="sxs-lookup"><span data-stu-id="a9957-524">Applicable Attributes</span></span>

<span data-ttu-id="a9957-525">В следующей таблице описаны атрибуты, которые могут применяться к **сопоставление** элемент.</span><span class="sxs-lookup"><span data-stu-id="a9957-525">The table below describes the attributes that can be applied to the **Mapping** element.</span></span>

| <span data-ttu-id="a9957-526">Имя атрибута</span><span class="sxs-lookup"><span data-stu-id="a9957-526">Attribute Name</span></span> | <span data-ttu-id="a9957-527">Необходимо</span><span class="sxs-lookup"><span data-stu-id="a9957-527">Is Required</span></span> | <span data-ttu-id="a9957-528">Значение</span><span class="sxs-lookup"><span data-stu-id="a9957-528">Value</span></span>                                                 |
|:---------------|:------------|:------------------------------------------------------|
| <span data-ttu-id="a9957-529">**ПРОБЕЛ**</span><span class="sxs-lookup"><span data-stu-id="a9957-529">**Space**</span></span>      | <span data-ttu-id="a9957-530">Да</span><span class="sxs-lookup"><span data-stu-id="a9957-530">Yes</span></span>         | <span data-ttu-id="a9957-531">**C-S**.</span><span class="sxs-lookup"><span data-stu-id="a9957-531">**C-S**.</span></span> <span data-ttu-id="a9957-532">Это фиксированное значение, и его невозможно изменить.</span><span class="sxs-lookup"><span data-stu-id="a9957-532">This is a fixed value and cannot be changed.</span></span> |

### <a name="example"></a><span data-ttu-id="a9957-533">Пример</span><span class="sxs-lookup"><span data-stu-id="a9957-533">Example</span></span>

<span data-ttu-id="a9957-534">В следующем примере показан **сопоставление** элемент, основанный на модели School.</span><span class="sxs-lookup"><span data-stu-id="a9957-534">The following example shows a **Mapping** element that is based on part of the School model.</span></span> <span data-ttu-id="a9957-535">Дополнительные сведения о модели School см. Краткое руководство (Entity Framework).</span><span class="sxs-lookup"><span data-stu-id="a9957-535">For more information about the School model, see Quickstart (Entity Framework):</span></span>

``` xml
 <Mapping Space="C-S"
          xmlns="http://schemas.microsoft.com/ado/2009/11/mapping/cs">
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

## <a name="mappingfragment-element-msl"></a><span data-ttu-id="a9957-536">Элемент MappingFragment (MSL)</span><span class="sxs-lookup"><span data-stu-id="a9957-536">MappingFragment Element (MSL)</span></span>

<span data-ttu-id="a9957-537">**MappingFragment** элемент в язык определения сопоставлений (MSL) определяет сопоставление между свойствами типа сущности концептуальной модели и таблицы или представления в базе данных.</span><span class="sxs-lookup"><span data-stu-id="a9957-537">The **MappingFragment** element in mapping specification language (MSL) defines the mapping between the properties of a conceptual model entity type and a table or view in the database.</span></span> <span data-ttu-id="a9957-538">Сведения о типах сущностей концептуальной модели и базовых таблиц базы данных или представления см. в разделе элемент EntityType (CSDL) и элемент EntitySet (SSDL).</span><span class="sxs-lookup"><span data-stu-id="a9957-538">For information about conceptual model entity types and underlying database tables or views, see EntityType Element (CSDL) and EntitySet Element (SSDL).</span></span> <span data-ttu-id="a9957-539">**MappingFragment** может быть дочерним элементом элемента EntityTypeMapping-элемент или элемент EntitySetMapping.</span><span class="sxs-lookup"><span data-stu-id="a9957-539">The **MappingFragment** can be a child element of the EntityTypeMapping element or the EntitySetMapping element.</span></span>

<span data-ttu-id="a9957-540">**MappingFragment** элемент может иметь следующие дочерние элементы:</span><span class="sxs-lookup"><span data-stu-id="a9957-540">The **MappingFragment** element can have the following child elements:</span></span>

-   <span data-ttu-id="a9957-541">ComplexType (ноль и более)</span><span class="sxs-lookup"><span data-stu-id="a9957-541">ComplexType (zero or more)</span></span>
-   <span data-ttu-id="a9957-542">ScalarProperty (ноль и более)</span><span class="sxs-lookup"><span data-stu-id="a9957-542">ScalarProperty (zero or more)</span></span>
-   <span data-ttu-id="a9957-543">Условие (ноль и более)</span><span class="sxs-lookup"><span data-stu-id="a9957-543">Condition (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="a9957-544">Применимые атрибуты</span><span class="sxs-lookup"><span data-stu-id="a9957-544">Applicable Attributes</span></span>

<span data-ttu-id="a9957-545">В следующей таблице описаны атрибуты, которые могут применяться к **MappingFragment** элемент.</span><span class="sxs-lookup"><span data-stu-id="a9957-545">The following table describes the attributes that can be applied to the **MappingFragment** element.</span></span>

| <span data-ttu-id="a9957-546">Имя атрибута</span><span class="sxs-lookup"><span data-stu-id="a9957-546">Attribute Name</span></span>          | <span data-ttu-id="a9957-547">Необходимо</span><span class="sxs-lookup"><span data-stu-id="a9957-547">Is Required</span></span> | <span data-ttu-id="a9957-548">Значение</span><span class="sxs-lookup"><span data-stu-id="a9957-548">Value</span></span>                                                                                                                                                                                                                         |
|:------------------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="a9957-549">**StoreEntitySet**</span><span class="sxs-lookup"><span data-stu-id="a9957-549">**StoreEntitySet**</span></span>      | <span data-ttu-id="a9957-550">Да</span><span class="sxs-lookup"><span data-stu-id="a9957-550">Yes</span></span>         | <span data-ttu-id="a9957-551">Имя столбца таблицы или представления, с которым производится сопоставление.</span><span class="sxs-lookup"><span data-stu-id="a9957-551">The name of the table or view that is being mapped.</span></span>                                                                                                                                                                           |
| <span data-ttu-id="a9957-552">**MakeColumnsDistinct**</span><span class="sxs-lookup"><span data-stu-id="a9957-552">**MakeColumnsDistinct**</span></span> | <span data-ttu-id="a9957-553">Нет</span><span class="sxs-lookup"><span data-stu-id="a9957-553">No</span></span>          | <span data-ttu-id="a9957-554">**Значение true,** или **False** в зависимости от того, возвращаются только уникальные строки.</span><span class="sxs-lookup"><span data-stu-id="a9957-554">**True** or **False** depending on whether only distinct rows are returned.</span></span> <br/> <span data-ttu-id="a9957-555">Если этот атрибут имеет значение **True**, **generateupdateviews как** атрибут элемента EntityContainerMapping должно быть присвоено **False**.</span><span class="sxs-lookup"><span data-stu-id="a9957-555">If this attribute is set to **True**, the **GenerateUpdateViews** attribute of the EntityContainerMapping element must be set to **False**.</span></span> |

### <a name="example"></a><span data-ttu-id="a9957-556">Пример</span><span class="sxs-lookup"><span data-stu-id="a9957-556">Example</span></span>

<span data-ttu-id="a9957-557">В следующем примере показан **MappingFragment** элемент дочерним **EntityTypeMapping** элемент.</span><span class="sxs-lookup"><span data-stu-id="a9957-557">The following example shows a **MappingFragment** element as the child of an **EntityTypeMapping** element.</span></span> <span data-ttu-id="a9957-558">В этом примере свойства **курс** типа в концептуальной модели сопоставляются со столбцами из **курс** таблицы в базе данных.</span><span class="sxs-lookup"><span data-stu-id="a9957-558">In this example, properties of the **Course** type in the conceptual model are mapped to columns of the **Course** table in the database.</span></span>

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

### <a name="example"></a><span data-ttu-id="a9957-559">Пример</span><span class="sxs-lookup"><span data-stu-id="a9957-559">Example</span></span>

<span data-ttu-id="a9957-560">В следующем примере показан **MappingFragment** элемент дочерним **EntitySetMapping** элемент.</span><span class="sxs-lookup"><span data-stu-id="a9957-560">The following example shows a **MappingFragment** element as the child of an **EntitySetMapping** element.</span></span> <span data-ttu-id="a9957-561">Как показано в примере выше свойства **курс** типа в концептуальной модели сопоставляются со столбцами из **курс** таблицы в базе данных.</span><span class="sxs-lookup"><span data-stu-id="a9957-561">As in the example above, properties of the **Course** type in the conceptual model are mapped to columns of the **Course** table in the database.</span></span>

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

## <a name="modificationfunctionmapping-element-msl"></a><span data-ttu-id="a9957-562">Элемент ModificationFunctionMapping (MSL)</span><span class="sxs-lookup"><span data-stu-id="a9957-562">ModificationFunctionMapping Element (MSL)</span></span>

<span data-ttu-id="a9957-563">**ModificationFunctionMapping** в язык определения сопоставлений (MSL) сопоставляет вставки, обновления и удаления функций типа сущности концептуальной модели с хранимыми процедурами в основной базе данных.</span><span class="sxs-lookup"><span data-stu-id="a9957-563">The **ModificationFunctionMapping** element in mapping specification language (MSL) maps the insert, update, and delete functions of a conceptual model entity type to stored procedures in the underlying database.</span></span> <span data-ttu-id="a9957-564">**ModificationFunctionMapping** элемент также можно сопоставить Вставка и удаление функций для многие ко многим ассоциаций в концептуальной модели с хранимыми процедурами в основной базе данных.</span><span class="sxs-lookup"><span data-stu-id="a9957-564">The **ModificationFunctionMapping** element can also map the insert and delete functions for many-to-many associations in the conceptual model to stored procedures in the underlying database.</span></span> <span data-ttu-id="a9957-565">Хранимые процедуры, с которыми сопоставляются функции изменения, должны объявляться в модели хранения.</span><span class="sxs-lookup"><span data-stu-id="a9957-565">Stored procedures to which modification functions are mapped must be declared in the storage model.</span></span> <span data-ttu-id="a9957-566">Дополнительные сведения см. в разделе элемента функция (SSDL).</span><span class="sxs-lookup"><span data-stu-id="a9957-566">For more information, see Function Element (SSDL).</span></span>

> [!NOTE]
> <span data-ttu-id="a9957-567">Если не выполнено сопоставление всех трех вставки, обновления или удаления типов сущностей с хранимыми процедурами, то несопоставленные операции завершится ошибкой, если во время выполнения и UpdateException возникает исключение.</span><span class="sxs-lookup"><span data-stu-id="a9957-567">If you do not map all three of the insert, update, or delete operations of a entity type to stored procedures, the unmapped operations will fail if executed at runtime and an UpdateException is thrown.</span></span>


> [!NOTE]
> <span data-ttu-id="a9957-568">Если функции преобразования для одной сущности в иерархии наследования сопоставляются с хранимыми процедурами, то функции преобразования всех типов в иерархии должны быть сопоставлены с хранимыми процедурами.</span><span class="sxs-lookup"><span data-stu-id="a9957-568">If the modification functions for one entity in an inheritance hierarchy are mapped to stored procedures, then modification functions for all types in the hierarchy must be mapped to stored procedures.</span></span>

<span data-ttu-id="a9957-569">**ModificationFunctionMapping** элемент может быть дочерний элемент EntityTypeMapping или AssociationSetMapping-элемент.</span><span class="sxs-lookup"><span data-stu-id="a9957-569">The **ModificationFunctionMapping** element can be a child of the EntityTypeMapping element or the AssociationSetMapping element.</span></span>

<span data-ttu-id="a9957-570">**ModificationFunctionMapping** элемент может иметь следующие дочерние элементы:</span><span class="sxs-lookup"><span data-stu-id="a9957-570">The **ModificationFunctionMapping** element can have the following child elements:</span></span>

-   <span data-ttu-id="a9957-571">Элемент DeleteFunction (ноль или один)</span><span class="sxs-lookup"><span data-stu-id="a9957-571">DeleteFunction (zero or one)</span></span>
-   <span data-ttu-id="a9957-572">Элемент InsertFunction (ноль или один)</span><span class="sxs-lookup"><span data-stu-id="a9957-572">InsertFunction (zero or one)</span></span>
-   <span data-ttu-id="a9957-573">UpdateFunction (ноль или один)</span><span class="sxs-lookup"><span data-stu-id="a9957-573">UpdateFunction (zero or one)</span></span>

<span data-ttu-id="a9957-574">Нет атрибутов, применимых к **ModificationFunctionMapping** элемент.</span><span class="sxs-lookup"><span data-stu-id="a9957-574">No attributes are applicable to the **ModificationFunctionMapping** element.</span></span>

### <a name="example"></a><span data-ttu-id="a9957-575">Пример</span><span class="sxs-lookup"><span data-stu-id="a9957-575">Example</span></span>

<span data-ttu-id="a9957-576">В следующем примере показано сопоставление набора сущностей для **людей** набору сущностей в модели School.</span><span class="sxs-lookup"><span data-stu-id="a9957-576">The following example shows the entity set mapping for the **People** entity set in the School model.</span></span> <span data-ttu-id="a9957-577">В дополнение к сопоставлению столбцов **Person** тип сущности, сопоставление вставки, обновления и удаления функций **Person** отображаются тип.</span><span class="sxs-lookup"><span data-stu-id="a9957-577">In addition to the column mapping for the **Person** entity type, the mapping of the insert, update, and delete functions of the **Person** type are shown.</span></span> <span data-ttu-id="a9957-578">Функции, с которыми выполняется сопоставление, объявляются в модели хранения.</span><span class="sxs-lookup"><span data-stu-id="a9957-578">The functions that are mapped to are declared in the storage model.</span></span>

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

### <a name="example"></a><span data-ttu-id="a9957-579">Пример</span><span class="sxs-lookup"><span data-stu-id="a9957-579">Example</span></span>

<span data-ttu-id="a9957-580">В следующем примере показано сопоставление набора ассоциаций для **CourseInstructor** набор ассоциаций в модели School.</span><span class="sxs-lookup"><span data-stu-id="a9957-580">The following example shows the association set mapping for the **CourseInstructor** association set in the School model.</span></span> <span data-ttu-id="a9957-581">В дополнение к сопоставлению столбцов **CourseInstructor** ассоциации, сопоставление функции insert и delete **CourseInstructor** показаны связи.</span><span class="sxs-lookup"><span data-stu-id="a9957-581">In addition to the column mapping for the **CourseInstructor** association, the mapping of the insert and delete functions of the **CourseInstructor** association are shown.</span></span> <span data-ttu-id="a9957-582">Функции, с которыми выполняется сопоставление, объявляются в модели хранения.</span><span class="sxs-lookup"><span data-stu-id="a9957-582">The functions that are mapped to are declared in the storage model.</span></span>

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
 

 

## <a name="queryview-element-msl"></a><span data-ttu-id="a9957-583">Элемент QueryView (MSL)</span><span class="sxs-lookup"><span data-stu-id="a9957-583">QueryView Element (MSL)</span></span>

<span data-ttu-id="a9957-584">**QueryView** элемент в язык определения сопоставлений (MSL) определяет сопоставление только для чтения между типом сущности или ассоциации в концептуальной модели и таблицей в основной базе данных.</span><span class="sxs-lookup"><span data-stu-id="a9957-584">The **QueryView** element in mapping specification language (MSL) defines a read-only mapping between an entity type or association in the conceptual model and a table in the underlying database.</span></span> <span data-ttu-id="a9957-585">Сопоставление определяется с помощью запроса Entity SQL, которое вычисляется по модели хранения и express результирующий набор с точки зрения сущность или ассоциацию в концептуальной модели.</span><span class="sxs-lookup"><span data-stu-id="a9957-585">The mapping is defined with an Entity SQL query that is evaluated against the storage model, and you express the result set in terms of an entity or association in the conceptual model.</span></span> <span data-ttu-id="a9957-586">Представления запросов доступны только для чтения, поэтому для обновления типов, которые определены представлениями запросов, не могут применяться стандартные команды обновления.</span><span class="sxs-lookup"><span data-stu-id="a9957-586">Because query views are read-only, you cannot use standard update commands to update types that are defined by query views.</span></span> <span data-ttu-id="a9957-587">Представления запросов доступны только для чтения, поэтому для обновления типов, которые определены представлениями запросов, не могут применяться стандартные команды обновления.</span><span class="sxs-lookup"><span data-stu-id="a9957-587">You can make updates to these types by using modification functions.</span></span> <span data-ttu-id="a9957-588">Дополнительные сведения см. в разделе Практическое: функции изменения сопоставления хранимых процедур.</span><span class="sxs-lookup"><span data-stu-id="a9957-588">For more information, see How to: Map Modification Functions to Stored Procedures.</span></span>

> [!NOTE]
> <span data-ttu-id="a9957-589">В **QueryView** элемент, выражения Entity SQL, содержащие **GroupBy**, групповые статистические выражения и свойства навигации не поддерживаются.</span><span class="sxs-lookup"><span data-stu-id="a9957-589">In the **QueryView** element, Entity SQL expressions that contain **GroupBy**, group aggregates, or navigation properties are not supported.</span></span>

 

<span data-ttu-id="a9957-590">**QueryView** элемент может быть дочерним элементом элемента EntitySetMapping или AssociationSetMapping-элемент.</span><span class="sxs-lookup"><span data-stu-id="a9957-590">The **QueryView** element can be a child of the EntitySetMapping element or the AssociationSetMapping element.</span></span> <span data-ttu-id="a9957-591">В первом случае представление запроса определяет сопоставление только для чтения для сущности в концептуальной модели.</span><span class="sxs-lookup"><span data-stu-id="a9957-591">In the former case, the query view defines a read-only mapping for an entity in the conceptual model.</span></span> <span data-ttu-id="a9957-592">Во втором случае представление запроса определяет сопоставление только для чтения для связи в концептуальной модели.</span><span class="sxs-lookup"><span data-stu-id="a9957-592">In the latter case, the query view defines a read-only mapping for an association in the conceptual model.</span></span>

> [!NOTE]
> <span data-ttu-id="a9957-593">Если **AssociationSetMapping** элемент предназначен для связи со ссылочным ограничением, **AssociationSetMapping** элемент игнорируется.</span><span class="sxs-lookup"><span data-stu-id="a9957-593">If the **AssociationSetMapping** element is for an association with a referential constraint, the **AssociationSetMapping** element is ignored.</span></span> <span data-ttu-id="a9957-594">Дополнительные сведения см. в разделе элемент ReferentialConstraint (CSDL).</span><span class="sxs-lookup"><span data-stu-id="a9957-594">For more information, see ReferentialConstraint Element (CSDL).</span></span>

<span data-ttu-id="a9957-595">**QueryView** элемент не может иметь дочерние элементы.</span><span class="sxs-lookup"><span data-stu-id="a9957-595">The **QueryView** element cannot have any child elements.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="a9957-596">Применимые атрибуты</span><span class="sxs-lookup"><span data-stu-id="a9957-596">Applicable Attributes</span></span>

<span data-ttu-id="a9957-597">В следующей таблице описаны атрибуты, которые могут применяться к **QueryView** элемент.</span><span class="sxs-lookup"><span data-stu-id="a9957-597">The following table describes the attributes that can be applied to the **QueryView** element.</span></span>

| <span data-ttu-id="a9957-598">Имя атрибута</span><span class="sxs-lookup"><span data-stu-id="a9957-598">Attribute Name</span></span> | <span data-ttu-id="a9957-599">Необходимо</span><span class="sxs-lookup"><span data-stu-id="a9957-599">Is Required</span></span> | <span data-ttu-id="a9957-600">Значение</span><span class="sxs-lookup"><span data-stu-id="a9957-600">Value</span></span>                                                                         |
|:---------------|:------------|:------------------------------------------------------------------------------|
| <span data-ttu-id="a9957-601">**Имя типа**</span><span class="sxs-lookup"><span data-stu-id="a9957-601">**TypeName**</span></span>   | <span data-ttu-id="a9957-602">Нет</span><span class="sxs-lookup"><span data-stu-id="a9957-602">No</span></span>          | <span data-ttu-id="a9957-603">Имя типа концептуальной модели, сопоставляемой представлением запроса.</span><span class="sxs-lookup"><span data-stu-id="a9957-603">The name of the conceptual model type that is being mapped by the query view.</span></span> |

### <a name="example"></a><span data-ttu-id="a9957-604">Пример</span><span class="sxs-lookup"><span data-stu-id="a9957-604">Example</span></span>

<span data-ttu-id="a9957-605">В следующем примере показан **QueryView** как дочерний элемент элемента **EntitySetMapping** элемент и определяет сопоставление представления запроса для **отдел** типа сущности в Модель School.</span><span class="sxs-lookup"><span data-stu-id="a9957-605">The following example shows the **QueryView** element as a child of the **EntitySetMapping** element and defines a query view mapping for the **Department** entity type in the School Model.</span></span>

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

<span data-ttu-id="a9957-606">Поскольку запрос возвращает только подмножество членов **отдел** тип в модели хранения, **отдел** тип в модели School был изменен на базе этих сопоставлений следующим образом:</span><span class="sxs-lookup"><span data-stu-id="a9957-606">Because the query only returns a subset of the members of the **Department** type in the storage model, the **Department** type in the School model has been modified based on this mapping as follows:</span></span>

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

### <a name="example"></a><span data-ttu-id="a9957-607">Пример</span><span class="sxs-lookup"><span data-stu-id="a9957-607">Example</span></span>

<span data-ttu-id="a9957-608">В следующем примере показан **QueryView** элемент дочерним **AssociationSetMapping** элемент и определяет сопоставление только для чтения для `FK_Course_Department` ассоциации в модели School.</span><span class="sxs-lookup"><span data-stu-id="a9957-608">The next example shows the **QueryView** element as the child of an **AssociationSetMapping** element and defines a read-only mapping for the `FK_Course_Department` association in the School model.</span></span>

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
 
### <a name="comments"></a><span data-ttu-id="a9957-609">Комментарии</span><span class="sxs-lookup"><span data-stu-id="a9957-609">Comments</span></span>

<span data-ttu-id="a9957-610">Представления запросов можно определять для реализации следующих сценариев.</span><span class="sxs-lookup"><span data-stu-id="a9957-610">You can define query views to enable the following scenarios:</span></span>

-   <span data-ttu-id="a9957-611">Определение сущности в концептуальной модели, не включающей все свойства сущности в модели хранения.</span><span class="sxs-lookup"><span data-stu-id="a9957-611">Define an entity in the conceptual model that doesn't include all the properties of the entity in the storage model.</span></span> <span data-ttu-id="a9957-612">Сюда входят свойства, которые не имеют значения по умолчанию и не поддерживают **null** значения.</span><span class="sxs-lookup"><span data-stu-id="a9957-612">This includes properties that do not have default values and do not support **null** values.</span></span>
-   <span data-ttu-id="a9957-613">Сопоставление вычисляемых столбцов в модели хранения со свойствами типов сущностей в концептуальной модели.</span><span class="sxs-lookup"><span data-stu-id="a9957-613">Map computed columns in the storage model to properties of entity types in the conceptual model.</span></span>
-   <span data-ttu-id="a9957-614">Определение сопоставления, в котором условия, используемые для секционирования сущностей в концептуальной модели, не базируются на равенстве.</span><span class="sxs-lookup"><span data-stu-id="a9957-614">Define a mapping where conditions used to partition entities in the conceptual model are not based on equality.</span></span> <span data-ttu-id="a9957-615">При указании условного сопоставления с помощью **условие** элемент, указанное условие должно быть равно указанному значению.</span><span class="sxs-lookup"><span data-stu-id="a9957-615">When you specify a conditional mapping using the **Condition** element, the supplied condition must equal the specified value.</span></span> <span data-ttu-id="a9957-616">Дополнительные сведения см. в разделе элемент Condition (MSL).</span><span class="sxs-lookup"><span data-stu-id="a9957-616">For more information, see Condition Element (MSL).</span></span>
-   <span data-ttu-id="a9957-617">Сопоставление одного столбца в модели хранения с несколькими типами в концептуальной модели.</span><span class="sxs-lookup"><span data-stu-id="a9957-617">Map the same column in the storage model to multiple types in the conceptual model.</span></span>
-   <span data-ttu-id="a9957-618">Сопоставление нескольких типов с одной и той же таблицей.</span><span class="sxs-lookup"><span data-stu-id="a9957-618">Map multiple types to the same table.</span></span>
-   <span data-ttu-id="a9957-619">Определение ассоциаций в концептуальной модели, базирующихся на внешних ключах реляционной схемы.</span><span class="sxs-lookup"><span data-stu-id="a9957-619">Define associations in the conceptual model that are not based on foreign keys in the relational schema.</span></span>
-   <span data-ttu-id="a9957-620">Применение пользовательской бизнес-логики для задания значений свойств в концептуальной модели.</span><span class="sxs-lookup"><span data-stu-id="a9957-620">Use custom business logic to set the value of properties in the conceptual model.</span></span> <span data-ttu-id="a9957-621">Например, можно сопоставить строковое значение «T» в источнике данных в значение **true**, логическое значение, в концептуальной модели.</span><span class="sxs-lookup"><span data-stu-id="a9957-621">For example, you could map the string value "T" in the data source to a value of **true**, a Boolean, in the conceptual model.</span></span>
-   <span data-ttu-id="a9957-622">Определение условных фильтров для результатов запроса.</span><span class="sxs-lookup"><span data-stu-id="a9957-622">Define conditional filters for query results.</span></span>
-   <span data-ttu-id="a9957-623">Наложение меньшего числа ограничений на данные в концептуальной модели, нежели в модели хранения.</span><span class="sxs-lookup"><span data-stu-id="a9957-623">Enforce fewer restrictions on data in the conceptual model than in the storage model.</span></span> <span data-ttu-id="a9957-624">Например, можно определить свойство в концептуальной модели допускает значения null даже если столбец, с которым сопоставляется не поддерживает **null**значения.</span><span class="sxs-lookup"><span data-stu-id="a9957-624">For example, you could make a property in the conceptual model nullable even if the column to which it is mapped does not support **null**values.</span></span>

<span data-ttu-id="a9957-625">При определении представлений запросов для сущностей необходимо принимать во внимание следующие соображения.</span><span class="sxs-lookup"><span data-stu-id="a9957-625">The following considerations apply when you define query views for entities:</span></span>

-   <span data-ttu-id="a9957-626">Представления запросов доступны только для чтения.</span><span class="sxs-lookup"><span data-stu-id="a9957-626">Query views are read-only.</span></span> <span data-ttu-id="a9957-627">Обновление сущностей можно осуществлять только с помощью функций изменения.</span><span class="sxs-lookup"><span data-stu-id="a9957-627">You can only make updates to entities by using modification functions.</span></span>
-   <span data-ttu-id="a9957-628">В случае определения типа сущности через представление запроса необходимо также определять все связанные сущности через представления запросов.</span><span class="sxs-lookup"><span data-stu-id="a9957-628">When you define an entity type by a query view, you must also define all related entities by query views.</span></span>
-   <span data-ttu-id="a9957-629">При сопоставлении ассоциации многие ко многим сущности в модели хранения, которая представляет связанную таблицу в реляционной схеме, необходимо определить **QueryView** элемент в **AssociationSetMapping** элемент для данной таблицы связей.</span><span class="sxs-lookup"><span data-stu-id="a9957-629">When you map a many-to-many association to an entity in the storage model that represents a link table in the relational schema, you must define a **QueryView** element in the **AssociationSetMapping** element for this link table.</span></span>
-   <span data-ttu-id="a9957-630">Представления запросов должны быть определены для всех типов в иерархии типов.</span><span class="sxs-lookup"><span data-stu-id="a9957-630">Query views must be defined for all types in a type hierarchy.</span></span> <span data-ttu-id="a9957-631">Это можно сделать следующими способами:</span><span class="sxs-lookup"><span data-stu-id="a9957-631">You can do this in the following ways:</span></span>
-   -   <span data-ttu-id="a9957-632">С помощью одного **QueryView** элемент, который указывает один запрос Entity SQL, которая возвращает объединение всех типов сущностей в иерархии.</span><span class="sxs-lookup"><span data-stu-id="a9957-632">With a single **QueryView** element that specifies a single Entity SQL query that returns a union of all of the entity types in the hierarchy.</span></span>
    -   <span data-ttu-id="a9957-633">С помощью одного **QueryView** элемент, который указывает один запрос Entity SQL, использующий оператор CASE для возврата определенного типа сущности в иерархии на основе определенного условия.</span><span class="sxs-lookup"><span data-stu-id="a9957-633">With a single **QueryView** element that specifies a single Entity SQL query that uses the CASE operator to return a specific entity type in the hierarchy based on a specific condition.</span></span>
    -   <span data-ttu-id="a9957-634">С помощью дополнительного **QueryView** элемента для конкретного типа в иерархии.</span><span class="sxs-lookup"><span data-stu-id="a9957-634">With an additional **QueryView** element for a specific type in the hierarchy.</span></span> <span data-ttu-id="a9957-635">В этом случае следует использовать **TypeName** атрибут **QueryView** элемента, чтобы указать тип сущности для каждого представления.</span><span class="sxs-lookup"><span data-stu-id="a9957-635">In this case, use the **TypeName** attribute of the **QueryView** element to specify the entity type for each view.</span></span>
-   <span data-ttu-id="a9957-636">Когда представление запроса определено, нельзя указать **StorageSetName** атрибут **EntitySetMapping** элемент.</span><span class="sxs-lookup"><span data-stu-id="a9957-636">When a query view is defined, you cannot specify the **StorageSetName** attribute on the **EntitySetMapping** element.</span></span>
-   <span data-ttu-id="a9957-637">Когда представление запроса определено, **EntitySetMapping**элемент не может также содержать **свойство** сопоставления.</span><span class="sxs-lookup"><span data-stu-id="a9957-637">When a query view is defined, the **EntitySetMapping**element cannot also contain **Property** mappings.</span></span>

## <a name="resultbinding-element-msl"></a><span data-ttu-id="a9957-638">Элемент ResultBinding (язык MSL)</span><span class="sxs-lookup"><span data-stu-id="a9957-638">ResultBinding Element (MSL)</span></span>

<span data-ttu-id="a9957-639">**ResultBinding** в язык определения сопоставлений (MSL) сопоставляет значения столбцов, возвращаемых хранимыми процедурами свойствам сущности в концептуальной модели при сопоставлении функций изменения типа сущности хранимой процедуры в основной базе данных.</span><span class="sxs-lookup"><span data-stu-id="a9957-639">The **ResultBinding** element in mapping specification language (MSL) maps column values that are returned by stored procedures to entity properties in the conceptual model when entity type modification functions are mapped to stored procedures in the underlying database.</span></span> <span data-ttu-id="a9957-640">Например, если возвращается значение столбца идентификаторов в инструкции insert хранимой процедуры, **ResultBinding** элемент возвращаемое значение сопоставляется со свойством типа сущности в концептуальной модели.</span><span class="sxs-lookup"><span data-stu-id="a9957-640">For example, when the value of an identity column is returned by an insert stored procedure, the **ResultBinding** element maps the returned value to an entity type property in the conceptual model.</span></span>

<span data-ttu-id="a9957-641">**ResultBinding** элемент может быть дочерним элементом элемента InsertFunction или элемент UpdateFunction.</span><span class="sxs-lookup"><span data-stu-id="a9957-641">The **ResultBinding** element can be child of the InsertFunction element or the UpdateFunction element.</span></span>

<span data-ttu-id="a9957-642">**ResultBinding** элемент не может иметь дочерние элементы.</span><span class="sxs-lookup"><span data-stu-id="a9957-642">The **ResultBinding** element cannot have any child elements.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="a9957-643">Применимые атрибуты</span><span class="sxs-lookup"><span data-stu-id="a9957-643">Applicable Attributes</span></span>

<span data-ttu-id="a9957-644">В следующей таблице описаны атрибуты, применимые к **ResultBinding** элемент:</span><span class="sxs-lookup"><span data-stu-id="a9957-644">The following table describes the attributes that are applicable to the **ResultBinding** element:</span></span>

| <span data-ttu-id="a9957-645">Имя атрибута</span><span class="sxs-lookup"><span data-stu-id="a9957-645">Attribute Name</span></span> | <span data-ttu-id="a9957-646">Необходимо</span><span class="sxs-lookup"><span data-stu-id="a9957-646">Is Required</span></span> | <span data-ttu-id="a9957-647">Значение</span><span class="sxs-lookup"><span data-stu-id="a9957-647">Value</span></span>                                                                         |
|:---------------|:------------|:------------------------------------------------------------------------------|
| <span data-ttu-id="a9957-648">**Name**</span><span class="sxs-lookup"><span data-stu-id="a9957-648">**Name**</span></span>       | <span data-ttu-id="a9957-649">Да</span><span class="sxs-lookup"><span data-stu-id="a9957-649">Yes</span></span>         | <span data-ttu-id="a9957-650">Имя свойства сущности в концептуальной модели, с которым выполняется сопоставление.</span><span class="sxs-lookup"><span data-stu-id="a9957-650">The name of the entity property in the conceptual model that is being mapped.</span></span> |
| <span data-ttu-id="a9957-651">**ColumnName**</span><span class="sxs-lookup"><span data-stu-id="a9957-651">**ColumnName**</span></span> | <span data-ttu-id="a9957-652">Да</span><span class="sxs-lookup"><span data-stu-id="a9957-652">Yes</span></span>         | <span data-ttu-id="a9957-653">Имя столбца, с которым выполнятся сопоставление.</span><span class="sxs-lookup"><span data-stu-id="a9957-653">The name of the column being mapped.</span></span>                                          |

### <a name="example"></a><span data-ttu-id="a9957-654">Пример</span><span class="sxs-lookup"><span data-stu-id="a9957-654">Example</span></span>

<span data-ttu-id="a9957-655">В следующем примере, основана на модели School и показывает **InsertFunction** элемент, используемый для сопоставления функции вставки **Person** тип сущности, **InsertPerson** Хранимая процедура.</span><span class="sxs-lookup"><span data-stu-id="a9957-655">The following example is based on the School model and shows an **InsertFunction** element used to map the insert function of the **Person** entity type to the **InsertPerson** stored procedure.</span></span> <span data-ttu-id="a9957-656">( **InsertPerson** хранимой процедуры ниже и объявлена в модели хранения.) Объект **ResultBinding** элемент используется для сопоставления значения столбца, который возвращается хранимой процедурой (**NewPersonID**) со свойством типа сущности (**PersonID**).</span><span class="sxs-lookup"><span data-stu-id="a9957-656">(The **InsertPerson** stored procedure is shown below and is declared in the storage model.) A **ResultBinding** element is used to map a column value that is returned by the stored procedure (**NewPersonID**) to an entity type property (**PersonID**).</span></span>

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

<span data-ttu-id="a9957-657">Следующий запрос Transact-SQL описывает **InsertPerson** хранимой процедуры:</span><span class="sxs-lookup"><span data-stu-id="a9957-657">The following Transact-SQL describes the **InsertPerson** stored procedure:</span></span>

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

## <a name="resultmapping-element-msl"></a><span data-ttu-id="a9957-658">Элемент ResultMapping (язык MSL)</span><span class="sxs-lookup"><span data-stu-id="a9957-658">ResultMapping Element (MSL)</span></span>

<span data-ttu-id="a9957-659">**ResultMapping** элемент в язык определения сопоставлений (MSL) определяет сопоставление между импортом функции в концептуальной модели и хранимой процедуры в основной базе данных, при выполнении следующих условий:</span><span class="sxs-lookup"><span data-stu-id="a9957-659">The **ResultMapping** element in mapping specification language (MSL) defines the mapping between a function import in the conceptual model and a stored procedure in the underlying database when the following are true:</span></span>

-   <span data-ttu-id="a9957-660">Импорт функции возвращает тип сущности концептуальной модели или сложный тип.</span><span class="sxs-lookup"><span data-stu-id="a9957-660">The function import returns a conceptual model entity type or complex type.</span></span>
-   <span data-ttu-id="a9957-661">Имена столбцов, возвращаемые хранимой процедурой, не совпадают в точности с именами свойств в сложном типе или типе сущности.</span><span class="sxs-lookup"><span data-stu-id="a9957-661">The names of the columns returned by the stored procedure do not exactly match the names of the properties on the entity type or complex type.</span></span>

<span data-ttu-id="a9957-662">По умолчанию сопоставление между столбцами, возвращаемыми хранимой процедурой, и сложным типом или типом сущности выполняется по именам столбцов и свойств.</span><span class="sxs-lookup"><span data-stu-id="a9957-662">By default, the mapping between the columns returned by a stored procedure and an entity type or complex type is based on column and property names.</span></span> <span data-ttu-id="a9957-663">Если имена столбцов не точно совпадают с именами свойств, необходимо использовать **ResultMapping** элемент для определения сопоставления.</span><span class="sxs-lookup"><span data-stu-id="a9957-663">If column names do not exactly match property names, you must use the **ResultMapping** element to define the mapping.</span></span> <span data-ttu-id="a9957-664">Пример сопоставления по умолчанию см. в разделе элемент FunctionImportMapping (MSL).</span><span class="sxs-lookup"><span data-stu-id="a9957-664">For an example of the default mapping, see FunctionImportMapping Element (MSL).</span></span>

<span data-ttu-id="a9957-665">**ResultMapping** элемент является дочерним элементом элемента FunctionImportMapping.</span><span class="sxs-lookup"><span data-stu-id="a9957-665">The **ResultMapping** element is a child element of the FunctionImportMapping element.</span></span>

<span data-ttu-id="a9957-666">**ResultMapping** элемент может иметь следующие дочерние элементы:</span><span class="sxs-lookup"><span data-stu-id="a9957-666">The **ResultMapping** element can have the following child elements:</span></span>

-   <span data-ttu-id="a9957-667">EntityTypeMapping (ноль и более)</span><span class="sxs-lookup"><span data-stu-id="a9957-667">EntityTypeMapping (zero or more)</span></span>
-   <span data-ttu-id="a9957-668">ComplexTypeMapping</span><span class="sxs-lookup"><span data-stu-id="a9957-668">ComplexTypeMapping</span></span>

<span data-ttu-id="a9957-669">Нет атрибутов, применимых к **ResultMapping** элемент.</span><span class="sxs-lookup"><span data-stu-id="a9957-669">No attributes are applicable to the **ResultMapping** Element.</span></span>

### <a name="example"></a><span data-ttu-id="a9957-670">Пример</span><span class="sxs-lookup"><span data-stu-id="a9957-670">Example</span></span>

<span data-ttu-id="a9957-671">Рассмотрим следующую хранимую процедуру:</span><span class="sxs-lookup"><span data-stu-id="a9957-671">Consider the following stored procedure:</span></span>

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

<span data-ttu-id="a9957-672">Рассмотрим следующий тип сущности концептуальной модели:</span><span class="sxs-lookup"><span data-stu-id="a9957-672">Also consider the following conceptual model entity type:</span></span>

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

<span data-ttu-id="a9957-673">Для создания импорта функции, возвращающей экземпляры предыдущего типа сущности, сопоставление между столбцами, возвращаемыми хранимой процедуры и тип сущности должен быть определен в **ResultMapping** элемент:</span><span class="sxs-lookup"><span data-stu-id="a9957-673">In order to create a function import that returns instances of the previous entity type, the mapping between the columns returned by the stored procedure and the entity type must be defined in a **ResultMapping** element:</span></span>

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

## <a name="scalarproperty-element-msl"></a><span data-ttu-id="a9957-674">Элемент ScalarProperty (MSL)</span><span class="sxs-lookup"><span data-stu-id="a9957-674">ScalarProperty Element (MSL)</span></span>

<span data-ttu-id="a9957-675">**ScalarProperty** в язык определения сопоставлений (MSL) сопоставляет свойство типа сущности концептуальной модели, сложного типа или сопоставления столбца или параметра хранимой процедуры в базе данных.</span><span class="sxs-lookup"><span data-stu-id="a9957-675">The **ScalarProperty** element in mapping specification language (MSL) maps a property on a conceptual model entity type, complex type, or association to a table column or stored procedure parameter in the underlying database.</span></span>

> [!NOTE]
> <span data-ttu-id="a9957-676">Хранимые процедуры, с которыми сопоставляются функции изменения, должны объявляться в модели хранения.</span><span class="sxs-lookup"><span data-stu-id="a9957-676">Stored procedures to which modification functions are mapped must be declared in the storage model.</span></span> <span data-ttu-id="a9957-677">Дополнительные сведения см. в разделе элемента функция (SSDL).</span><span class="sxs-lookup"><span data-stu-id="a9957-677">For more information, see Function Element (SSDL).</span></span>

<span data-ttu-id="a9957-678">**ScalarProperty** элемент может быть дочерним элементом следующих элементов:</span><span class="sxs-lookup"><span data-stu-id="a9957-678">The **ScalarProperty** element can be a child of the following elements:</span></span>

-   <span data-ttu-id="a9957-679">MappingFragment</span><span class="sxs-lookup"><span data-stu-id="a9957-679">MappingFragment</span></span>
-   <span data-ttu-id="a9957-680">InsertFunction</span><span class="sxs-lookup"><span data-stu-id="a9957-680">InsertFunction</span></span>
-   <span data-ttu-id="a9957-681">UpdateFunction</span><span class="sxs-lookup"><span data-stu-id="a9957-681">UpdateFunction</span></span>
-   <span data-ttu-id="a9957-682">DeleteFunction</span><span class="sxs-lookup"><span data-stu-id="a9957-682">DeleteFunction</span></span>
-   <span data-ttu-id="a9957-683">EndProperty</span><span class="sxs-lookup"><span data-stu-id="a9957-683">EndProperty</span></span>
-   <span data-ttu-id="a9957-684">ComplexProperty</span><span class="sxs-lookup"><span data-stu-id="a9957-684">ComplexProperty</span></span>
-   <span data-ttu-id="a9957-685">ResultMapping</span><span class="sxs-lookup"><span data-stu-id="a9957-685">ResultMapping</span></span>

<span data-ttu-id="a9957-686">Как дочерний **MappingFragment**, **ComplexProperty**, или **EndProperty** элемент, **ScalarProperty** сопоставляет свойство в концептуальной модели со столбцом в базе данных.</span><span class="sxs-lookup"><span data-stu-id="a9957-686">As a child of the **MappingFragment**, **ComplexProperty**, or **EndProperty** element, the **ScalarProperty** element maps a property in the conceptual model to a column in the database.</span></span> <span data-ttu-id="a9957-687">Как дочерний **InsertFunction**, **UpdateFunction**, или **DeleteFunction** элемент, **ScalarProperty** сопоставляет свойство в концептуальной модели с параметром хранимой процедуры.</span><span class="sxs-lookup"><span data-stu-id="a9957-687">As a child of the **InsertFunction**, **UpdateFunction**, or **DeleteFunction** element, the **ScalarProperty** element maps a property in the conceptual model to a stored procedure parameter.</span></span>

<span data-ttu-id="a9957-688">**ScalarProperty** элемент не может иметь дочерние элементы.</span><span class="sxs-lookup"><span data-stu-id="a9957-688">The **ScalarProperty** element cannot have any child elements.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="a9957-689">Применимые атрибуты</span><span class="sxs-lookup"><span data-stu-id="a9957-689">Applicable Attributes</span></span>

<span data-ttu-id="a9957-690">Атрибуты, которые применяются к **ScalarProperty** элемент различаться в зависимости от роли элемента.</span><span class="sxs-lookup"><span data-stu-id="a9957-690">The attributes that apply to the **ScalarProperty** element differ depending on the role of the element.</span></span>

<span data-ttu-id="a9957-691">В следующей таблице описаны атрибуты, которые применяются, когда **ScalarProperty** элемент используется для сопоставления свойства концептуальной модели со столбцом в базе данных:</span><span class="sxs-lookup"><span data-stu-id="a9957-691">The following table describes the attributes that are applicable when the **ScalarProperty** element is used to map a conceptual model property to a column in the database:</span></span>

| <span data-ttu-id="a9957-692">Имя атрибута</span><span class="sxs-lookup"><span data-stu-id="a9957-692">Attribute Name</span></span> | <span data-ttu-id="a9957-693">Необходимо</span><span class="sxs-lookup"><span data-stu-id="a9957-693">Is Required</span></span> | <span data-ttu-id="a9957-694">Значение</span><span class="sxs-lookup"><span data-stu-id="a9957-694">Value</span></span>                                                           |
|:---------------|:------------|:----------------------------------------------------------------|
| <span data-ttu-id="a9957-695">**Name**</span><span class="sxs-lookup"><span data-stu-id="a9957-695">**Name**</span></span>       | <span data-ttu-id="a9957-696">Да</span><span class="sxs-lookup"><span data-stu-id="a9957-696">Yes</span></span>         | <span data-ttu-id="a9957-697">Имя свойства концептуальной модели, с которым выполняется сопоставление.</span><span class="sxs-lookup"><span data-stu-id="a9957-697">The name of the conceptual model property that is being mapped.</span></span> |
| <span data-ttu-id="a9957-698">**ColumnName**</span><span class="sxs-lookup"><span data-stu-id="a9957-698">**ColumnName**</span></span> | <span data-ttu-id="a9957-699">Да</span><span class="sxs-lookup"><span data-stu-id="a9957-699">Yes</span></span>         | <span data-ttu-id="a9957-700">Имя столбца таблицы, с которым производится сопоставление.</span><span class="sxs-lookup"><span data-stu-id="a9957-700">The name of the table column that is being mapped.</span></span>              |

<span data-ttu-id="a9957-701">В следующей таблице описаны атрибуты, применимые к **ScalarProperty** элементу, если он используется для сопоставления свойства концептуальной модели с параметром хранимой процедуры:</span><span class="sxs-lookup"><span data-stu-id="a9957-701">The following table describes the attributes that are applicable to the **ScalarProperty** element when it is used to map a conceptual model property to a stored procedure parameter:</span></span>

| <span data-ttu-id="a9957-702">Имя атрибута</span><span class="sxs-lookup"><span data-stu-id="a9957-702">Attribute Name</span></span>    | <span data-ttu-id="a9957-703">Необходимо</span><span class="sxs-lookup"><span data-stu-id="a9957-703">Is Required</span></span> | <span data-ttu-id="a9957-704">Значение</span><span class="sxs-lookup"><span data-stu-id="a9957-704">Value</span></span>                                                                                                                                           |
|:------------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="a9957-705">**Name**</span><span class="sxs-lookup"><span data-stu-id="a9957-705">**Name**</span></span>          | <span data-ttu-id="a9957-706">Да</span><span class="sxs-lookup"><span data-stu-id="a9957-706">Yes</span></span>         | <span data-ttu-id="a9957-707">Имя свойства концептуальной модели, с которым выполняется сопоставление.</span><span class="sxs-lookup"><span data-stu-id="a9957-707">The name of the conceptual model property that is being mapped.</span></span>                                                                                 |
| <span data-ttu-id="a9957-708">**Имя_параметра**</span><span class="sxs-lookup"><span data-stu-id="a9957-708">**ParameterName**</span></span> | <span data-ttu-id="a9957-709">Да</span><span class="sxs-lookup"><span data-stu-id="a9957-709">Yes</span></span>         | <span data-ttu-id="a9957-710">Имя сопоставляемого параметра.</span><span class="sxs-lookup"><span data-stu-id="a9957-710">The name of the parameter that is being mapped.</span></span>                                                                                                 |
| <span data-ttu-id="a9957-711">**Version**</span><span class="sxs-lookup"><span data-stu-id="a9957-711">**Version**</span></span>       | <span data-ttu-id="a9957-712">Нет</span><span class="sxs-lookup"><span data-stu-id="a9957-712">No</span></span>          | <span data-ttu-id="a9957-713">**Текущий** или **исходного** в зависимости от того, является ли текущее значение или исходное значение свойства должен использоваться для проверки на параллелизм.</span><span class="sxs-lookup"><span data-stu-id="a9957-713">**Current** or **Original** depending on whether the current value or the original value of the property should be used for concurrency checks.</span></span> |

### <a name="example"></a><span data-ttu-id="a9957-714">Пример</span><span class="sxs-lookup"><span data-stu-id="a9957-714">Example</span></span>

<span data-ttu-id="a9957-715">В следующем примере показан **ScalarProperty** элемента, используемого одним из двух способов:</span><span class="sxs-lookup"><span data-stu-id="a9957-715">The following example shows the **ScalarProperty** element used in two ways:</span></span>

-   <span data-ttu-id="a9957-716">Для сопоставления свойства **Person** тип сущности со столбцами **Person**таблицы.</span><span class="sxs-lookup"><span data-stu-id="a9957-716">To map the properties of the **Person** entity type to the columns of the **Person**table.</span></span>
-   <span data-ttu-id="a9957-717">Для сопоставления свойства **Person** типа сущности с параметрами **UpdatePerson** хранимой процедуры.</span><span class="sxs-lookup"><span data-stu-id="a9957-717">To map the properties of the **Person** entity type to the parameters of the **UpdatePerson** stored procedure.</span></span> <span data-ttu-id="a9957-718">Хранимые процедуры объявляются в модели хранения.</span><span class="sxs-lookup"><span data-stu-id="a9957-718">The stored procedures are declared in the storage model.</span></span>

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

### <a name="example"></a><span data-ttu-id="a9957-719">Пример</span><span class="sxs-lookup"><span data-stu-id="a9957-719">Example</span></span>

<span data-ttu-id="a9957-720">В следующем примере показан **ScalarProperty** элемент, используемый для сопоставления вставки и удаления ассоциации концептуальной модели с хранимыми процедурами в базе данных функции.</span><span class="sxs-lookup"><span data-stu-id="a9957-720">The next example shows the **ScalarProperty** element used to map the insert and delete functions of a conceptual model association to stored procedures in the database.</span></span> <span data-ttu-id="a9957-721">Хранимые процедуры объявляются в модели хранения.</span><span class="sxs-lookup"><span data-stu-id="a9957-721">The stored procedures are declared in the storage model.</span></span>

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

## <a name="updatefunction-element-msl"></a><span data-ttu-id="a9957-722">Элемент UpdateFunction (MSL)</span><span class="sxs-lookup"><span data-stu-id="a9957-722">UpdateFunction Element (MSL)</span></span>

<span data-ttu-id="a9957-723">**UpdateFunction** в язык определения сопоставлений (MSL) сопоставляет функцию обновления типа сущности в концептуальной модели с хранимой процедурой в основной базе данных.</span><span class="sxs-lookup"><span data-stu-id="a9957-723">The **UpdateFunction** element in mapping specification language (MSL) maps the update function of an entity type in the conceptual model to a stored procedure in the underlying database.</span></span> <span data-ttu-id="a9957-724">Хранимые процедуры, с которыми сопоставляются функции изменения, должны объявляться в модели хранения.</span><span class="sxs-lookup"><span data-stu-id="a9957-724">Stored procedures to which modification functions are mapped must be declared in the storage model.</span></span> <span data-ttu-id="a9957-725">Дополнительные сведения см. в разделе элемента функция (SSDL).</span><span class="sxs-lookup"><span data-stu-id="a9957-725">For more information, see Function Element (SSDL).</span></span>

> [!NOTE]
>  <span data-ttu-id="a9957-726">Если не выполнено сопоставление всех трех вставки, обновления или удаления типов сущностей с хранимыми процедурами, то несопоставленные операции завершится ошибкой, если во время выполнения и UpdateException возникает исключение.</span><span class="sxs-lookup"><span data-stu-id="a9957-726">If you do not map all three of the insert, update, or delete operations of a entity type to stored procedures, the unmapped operations will fail if executed at runtime and an UpdateException is thrown.</span></span>

<span data-ttu-id="a9957-727">**UpdateFunction** элемент может быть дочерним элементом ModificationFunctionMapping и применяется к элементу EntityTypeMapping.</span><span class="sxs-lookup"><span data-stu-id="a9957-727">The **UpdateFunction** element can be a child of the ModificationFunctionMapping element and applied to the EntityTypeMapping element.</span></span>

<span data-ttu-id="a9957-728">**UpdateFunction** элемент может иметь следующие дочерние элементы:</span><span class="sxs-lookup"><span data-stu-id="a9957-728">The **UpdateFunction** element can have the following child elements:</span></span>

-   <span data-ttu-id="a9957-729">AssociationEnd (ноль и более)</span><span class="sxs-lookup"><span data-stu-id="a9957-729">AssociationEnd (zero or more)</span></span>
-   <span data-ttu-id="a9957-730">ComplexProperty (ноль и более)</span><span class="sxs-lookup"><span data-stu-id="a9957-730">ComplexProperty (zero or more)</span></span>
-   <span data-ttu-id="a9957-731">ResultBinding (ноль или один)</span><span class="sxs-lookup"><span data-stu-id="a9957-731">ResultBinding (zero or one)</span></span>
-   <span data-ttu-id="a9957-732">ScarlarProperty (ноль и более)</span><span class="sxs-lookup"><span data-stu-id="a9957-732">ScarlarProperty (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="a9957-733">Применимые атрибуты</span><span class="sxs-lookup"><span data-stu-id="a9957-733">Applicable Attributes</span></span>

<span data-ttu-id="a9957-734">В следующей таблице описаны атрибуты, которые могут применяться к **UpdateFunction** элемент.</span><span class="sxs-lookup"><span data-stu-id="a9957-734">The following table describes the attributes that can be applied to the **UpdateFunction** element.</span></span>

| <span data-ttu-id="a9957-735">Имя атрибута</span><span class="sxs-lookup"><span data-stu-id="a9957-735">Attribute Name</span></span>            | <span data-ttu-id="a9957-736">Необходимо</span><span class="sxs-lookup"><span data-stu-id="a9957-736">Is Required</span></span> | <span data-ttu-id="a9957-737">Значение</span><span class="sxs-lookup"><span data-stu-id="a9957-737">Value</span></span>                                                                                                                                                    |
|:--------------------------|:------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="a9957-738">**FunctionName**</span><span class="sxs-lookup"><span data-stu-id="a9957-738">**FunctionName**</span></span>          | <span data-ttu-id="a9957-739">Да</span><span class="sxs-lookup"><span data-stu-id="a9957-739">Yes</span></span>         | <span data-ttu-id="a9957-740">Имя хранимой процедуры (с указанием пространства имен), с которой сопоставляется функция обновления.</span><span class="sxs-lookup"><span data-stu-id="a9957-740">The namespace-qualified name of the stored procedure to which the update function is mapped.</span></span> <span data-ttu-id="a9957-741">Хранимая процедура должна объявляться в модели хранения.</span><span class="sxs-lookup"><span data-stu-id="a9957-741">The stored procedure must be declared in the storage model.</span></span> |
| <span data-ttu-id="a9957-742">**RowsAffectedParameter**</span><span class="sxs-lookup"><span data-stu-id="a9957-742">**RowsAffectedParameter**</span></span> | <span data-ttu-id="a9957-743">Нет</span><span class="sxs-lookup"><span data-stu-id="a9957-743">No</span></span>          | <span data-ttu-id="a9957-744">Имя выходного параметра, возвращающего количество обработанных строк.</span><span class="sxs-lookup"><span data-stu-id="a9957-744">The name of the output parameter that returns the number of rows affected.</span></span>                                                                               |

### <a name="example"></a><span data-ttu-id="a9957-745">Пример</span><span class="sxs-lookup"><span data-stu-id="a9957-745">Example</span></span>

<span data-ttu-id="a9957-746">В следующем примере, основана на модели School и показывает **UpdateFunction** элемент, используемый для сопоставления функции обновления **Person** тип сущности, **UpdatePerson** Хранимая процедура.</span><span class="sxs-lookup"><span data-stu-id="a9957-746">The following example is based on the School model and shows the **UpdateFunction** element used to map update function of the **Person** entity type to the **UpdatePerson** stored procedure.</span></span> <span data-ttu-id="a9957-747">**UpdatePerson** хранимая процедура объявлена в модели хранения.</span><span class="sxs-lookup"><span data-stu-id="a9957-747">The **UpdatePerson** stored procedure is declared in the storage model.</span></span>

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
