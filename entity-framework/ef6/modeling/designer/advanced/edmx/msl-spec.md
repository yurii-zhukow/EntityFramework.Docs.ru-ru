---
title: Спецификация MSL - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 13ae7bc1-74b4-4ee4-8d73-c337be841467
ms.openlocfilehash: 9519155422d8542d4a14bc1c612e91ebc22bf15e
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/13/2018
ms.locfileid: "45490562"
---
# <a name="msl-specification"></a>Спецификация MSL
Язык определения сопоставлений (MSL) — это язык на основе XML, описывающий сопоставление между концептуальной моделью и моделью хранения приложения Entity Framework.

В приложении Entity Framework метаданные сопоставления загружается из MSL-файла (написанного на языке MSL) во время сборки. Платформа Entity Framework метаданные сопоставления используются во время выполнения для преобразования запросов к концептуальной модели для конкретного хранилища команды.

Entity Framework Designer (конструктор EF) хранит сведения о сопоставлении в EDMX-файла во время разработки. Во время построения конструктор сущностей использует сведения в EDMX-файла, создаваемого MSL-файл, необходимый Entity Framework во время выполнения

Имена всех типов концептуальной модели или модели хранения, которые упоминаются в языке MSL, должны указываться вместе с именами соответствующих пространств имен. Сведения об имени пространства имен концептуальной модели, см. в разделе [спецификация языка CSDL](~/ef6/modeling/designer/advanced/edmx/csdl-spec.md). Сведения об имени пространства имен модели хранения, см. в разделе [спецификация SSDL](~/ef6/modeling/designer/advanced/edmx/ssdl-spec.md).

Версии MSL различаются по пространствам имен XML.

| Версия MSL | Пространство имен XML                                        |
|:------------|:-----------------------------------------------------|
| Язык MSL v1      | urn: schemas-microsoft-com:windows:storage:mapping:CS |
| Язык MSL v2      | http://schemas.microsoft.com/ado/2008/09/mapping/cs  |
| Язык MSL v3      | http://schemas.microsoft.com/ado/2009/11/mapping/cs  |

## <a name="alias-element-msl"></a>Элемент Alias (язык MSL)

**Псевдоним** элемент в язык определения сопоставлений (MSL) — это дочерний элемент элемента сопоставления, который используется для определения псевдонимов для концептуальной модели и хранения пространства имен модели. Имена всех типов концептуальной модели или модели хранения, которые упоминаются в языке MSL, должны указываться вместе с именами соответствующих пространств имен. Сведения об имени пространства имен концептуальной модели см. в разделе элемент схемы (CSDL). Сведения об имени пространства имен модели хранения см. в разделе элемент схемы (SSDL).

**Псевдоним** элемент не может иметь дочерние элементы.

### <a name="applicable-attributes"></a>Применимые атрибуты

В следующей таблице описаны атрибуты, которые могут применяться к **псевдоним** элемент.

| Имя атрибута | Необходимо | Значение                                                                     |
|:---------------|:------------|:--------------------------------------------------------------------------|
| **Key**        | Да         | Псевдоним для пространства имен, который задается параметром **значение** атрибута. |
| **Значение**      | Да         | Пространство имен, для которого значение **ключ** элемент является псевдонимом.     |

### <a name="example"></a>Пример

В следующем примере показан **псевдоним** элемент, который определяет псевдоним, `c`, для типов, определенных в концептуальной модели.

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

## <a name="associationend-element-msl"></a>Элемент AssociationEnd (MSL)

**AssociationEnd** элемент в язык определения сопоставлений (MSL) используется при сопоставлении функций изменения типа сущности в концептуальной модели с хранимыми процедурами в основной базе данных. Если хранимая процедура изменения принимает параметр, значение которого содержится в свойстве ассоциации, **AssociationEnd** сопоставляет значение свойства параметра. Дополнительные сведения см. в приведенных ниже примерах.

Дополнительные сведения о сопоставлении функций изменения типов сущностей с хранимыми процедурами см. в разделе элемент ModificationFunctionMapping (MSL) и пошаговое руководство: сопоставление сущности с хранимых процедур.

**AssociationEnd** элемент может иметь следующие дочерние элементы:

-   ScalarProperty

### <a name="applicable-attributes"></a>Применимые атрибуты

В следующей таблице описаны атрибуты, применимые к **AssociationEnd** элемент.

| Имя атрибута     | Необходимо | Значение                                                                                                                                                                             |
|:-------------------|:------------|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **AssociationSet** | Да         | Имя сопоставляемой ассоциации.                                                                                                                                 |
| **From**           | Да         | Значение **FromRole** атрибут свойства навигации, которое соответствует сопоставляемой ассоциации. Дополнительные сведения см. в разделе элемент NavigationProperty (CSDL). |
| **Задача**             | Да         | Значение **ToRole** атрибут свойства навигации, которое соответствует сопоставляемой ассоциации. Дополнительные сведения см. в разделе элемент NavigationProperty (CSDL).   |

### <a name="example"></a>Пример

Рассмотрим следующий тип сущности в концептуальной модели:

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

Предположим, имеется следующая хранимая процедура:

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

Чтобы сопоставить функцию обновления `Course` сущности в хранимую процедуру, необходимо указать значение для **DepartmentID** параметра. Значение для `DepartmentID` не соответствует свойству типа сущности. Оно содержится в независимом сопоставлении с сопоставлением, показанным ниже:

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

В следующем коде показан **AssociationEnd** элемент, используемый для сопоставления **DepartmentID** свойство **FK\_курс\_отдел** связь с **UpdateCourse** хранимой процедуры (к которому функция обновления **курс** тип сущности сопоставлен):

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

## <a name="associationsetmapping-element-msl"></a>Элемент AssociationSetMapping (MSL)

**AssociationSetMapping** элемент в язык определения сопоставлений (MSL) определяет сопоставление между ассоциацией в концептуальной модели и столбцами таблицы в основной базе данных.

Сопоставления в концептуальной модели — это типы, свойства которых представляют столбцы первичного и внешнего ключа в основной базе данных. **AssociationSetMapping** элемент использует два элемента EndProperty, чтобы определить сопоставления между свойствами типов сопоставлений и столбцами в базе данных. Можно определить условия сопоставления в элемент Condition. Сопоставление insert, update и delete функции для ассоциаций с хранимыми процедурами в базе данных с элементом ModificationFunctionMapping. Определите только для чтения сопоставление между ассоциациями и столбцами таблицы с помощью строк Entity SQL в QueryView-элемент.

> [!NOTE]
> Если для ассоциации в концептуальной модели определено ссылочное ограничение, ассоциации необходимо сопоставить с **AssociationSetMapping** элемент. Если **AssociationSetMapping** элемент присутствует ассоциацию со справочным ограничением, то сопоставление, определенное в **AssociationSetMapping** элемент будет игнорироваться. Дополнительные сведения см. в разделе элемент ReferentialConstraint (CSDL).

**AssociationSetMapping** элемент может иметь следующие дочерние элементы

-   QueryView (ноль или один)
-   EndProperty (ноль или два)
-   Условие (ноль и более)
-   ModificationFunctionMapping (ноль или один)

### <a name="applicable-attributes"></a>Применимые атрибуты

В следующей таблице описаны атрибуты, которые могут применяться к **AssociationSetMapping** элемент.

| Имя атрибута     | Необходимо | Значение                                                                                       |
|:-------------------|:------------|:--------------------------------------------------------------------------------------------|
| **Name**           | Да         | Имя набора ассоциаций концептуальной модели, с которым выполняется сопоставление.                      |
| **Имя типа**       | Нет          | Имя типа ассоциации концептуальной модели из полного пространства имен, с которым выполняется сопоставление. |
| **StoreEntitySet** | Нет          | Имя таблицы, с которой производится сопоставление.                                                 |

### <a name="example"></a>Пример

В следующем примере показан **AssociationSetMapping** элемента, в котором **FK\_курс\_отдел** набор ассоциаций в концептуальной модели сопоставляется с  **Курс** таблицы в базе данных. Сопоставления между свойствами типов сопоставлений и столбцами таблицы указываются в дочерних **EndProperty** элементов.

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

## <a name="complexproperty-element-msl"></a>Элемент ComplexProperty (MSL)

Объект **ComplexProperty** элемент в язык определения сопоставлений (MSL) определяет сопоставление между свойством сложного типа в концептуальной модели сущности типа и столбцами таблицы в основной базе данных. Сопоставления столбца свойств указываются в дочерних элементах ScalarProperty.

**ComplexType** свойство может иметь следующие дочерние элементы:

-   ScalarProperty (ноль и более)
-   **ComplexProperty** (ноль и более)
-   ComplextTypeMapping (ноль и более)
-   Условие (ноль и более)

### <a name="applicable-attributes"></a>Применимые атрибуты

В следующей таблице описаны атрибуты, применимые к **ComplexProperty** элемент:

| Имя атрибута | Необходимо | Значение                                                                                            |
|:---------------|:------------|:-------------------------------------------------------------------------------------------------|
| **Name**       | Да         | Имя сложного свойства типа сущности в концептуальной модели, с которым выполняется сопоставление. |
| **Имя типа**   | Нет          | Полное имя пространства имен типа свойства концептуальной модели.                              |

### <a name="example"></a>Пример

Следующий пример основан на модели School. К концептуальной модели добавлен следующий сложный тип.

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

**LastName** и **FirstName** свойства **Person** тип сущности были заменены одним сложным свойством **имя**:

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

Следующей спецификации MSL показан **ComplexProperty** элемент, используемый для сопоставления **имя** свойства со столбцами в основной базе данных:

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

## <a name="complextypemapping-element-msl"></a>Элемент ComplexTypeMapping (MSL)

**ComplexTypeMapping** элементом в язык определения сопоставлений (MSL) — это дочерний элемент ResultMapping и определяет сопоставление между импортом функции в концептуальной модели и хранимой процедуры в базовом База данных, при выполнении следующих условий:

-   Импорт функции возвращает концептуальный сложный тип.
-   Имена столбцов, возвращаемые хранимой процедурой, не соответствуют в точности именам свойств в сложном типе.

По умолчанию сопоставление между столбцами, возвращаемыми хранимой процедурой, и сложным типом основано на именах столбцов и свойств. Если имена столбцов не точно совпадают с именами свойств, необходимо использовать **ComplexTypeMapping** элемент для определения сопоставления. Пример сопоставления по умолчанию см. в разделе элемент FunctionImportMapping (MSL).

**ComplexTypeMapping** элемент может иметь следующие дочерние элементы:

-   ScalarProperty (ноль и более)

### <a name="applicable-attributes"></a>Применимые атрибуты

В следующей таблице описаны атрибуты, применимые к **ComplexTypeMapping** элемент.

| Имя атрибута | Необходимо | Значение                                                                  |
|:---------------|:------------|:-----------------------------------------------------------------------|
| **Имя типа**   | Да         | Имя сопоставляемого сложного типа с указанием пространства имен. |

### <a name="example"></a>Пример

Рассмотрим следующую хранимую процедуру:

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

Следует также обратить внимание на следующий сложный тип концептуальной модели:

``` xml
 <ComplexType Name="GradeInfo">
   <Property Type="Int32" Name="EnrollmentID" Nullable="false" />
   <Property Type="Decimal" Name="Grade" Nullable="true"
             Precision="3" Scale="2" />
   <Property Type="Int32" Name="CourseID" Nullable="false" />
   <Property Type="Int32" Name="StudentID" Nullable="false" />
 </ComplexType>
```

Для создания импорта функции, возвращающей экземпляры предыдущего сложного типа, сопоставление между столбцами, возвращаемыми хранимой процедуры и тип сущности должен быть определен в **ComplexTypeMapping** элемент:

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

## <a name="condition-element-msl"></a>Элемент Condition (MSL)

**Условие** элемент в язык определения сопоставлений (MSL) Задает условия для сопоставления между концептуальной моделью и основной базе данных. Сопоставление, определенные в XML-узла является допустимым, если все условия, заданные в дочерних **условие** элементов, будут выполнены. В противном случае сопоставление недействительно. Например, если элемент MappingFragment содержит один или несколько **условие** дочерние элементы, то сопоставление, определенное в **MappingFragment** узел будет допустимым, если все условия дочернегоэлемента **Условие** элементы будут выполнены.

Каждое условие может применяться либо **имя** (имя свойства сущности концептуальной модели, определяемое **имя** атрибут), или **ColumnName** (имя столбца в базы данных, определяемое **ColumnName** атрибут). Когда **имя** атрибут имеет значение, условие проверяется по значению свойства сущности. Когда **ColumnName** атрибут имеет значение, условие проверяется значение столбца. Только один из **имя** или **ColumnName** атрибут может быть указан в **условие** элемент.

> [!NOTE]
> Когда **условие** элемент используется внутри элемента FunctionImportMapping только **имя** атрибут неприменим.

**Условие** элемент может быть дочерним элементом следующих элементов:

-   AssociationSetMapping
-   ComplexProperty
-   EntitySetMapping
-   MappingFragment
-   EntityTypeMapping

**Условие** элемент может иметь дочерних элементов.

### <a name="applicable-attributes"></a>Применимые атрибуты

В следующей таблице описаны атрибуты, применимые к **условие** элемент:

| Имя атрибута | Необходимо | Значение                                                                                                                                                                                                                                                                                         |
|:---------------|:------------|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **ColumnName** | Нет          | Имя столбца таблицы, значение которого используется для оценки условия.                                                                                                                                                                                                                   |
| **IsNull**     | Нет          | **Значение true,** или **False**. Если значение равно **True** и значение столбца равно **null**, или если значение равно **False** и значение столбца не **null**, условие имеет значение true . В противном случае условие имеет значение false. <br/> **IsNull** и **значение** атрибуты нельзя использовать одновременно. |
| **Значение**      | Нет          | Значение, с которым сравнивается значение столбца. Если значения совпадают, условие верно. В противном случае условие имеет значение false. <br/> **IsNull** и **значение** атрибуты нельзя использовать одновременно.                                                                       |
| **Name**       | Нет          | Имя свойства сущности концептуальной модели, значение которого используется для оценки условия. <br/> Этот атрибут неприменим Если **условие** элемент используется в элемент FunctionImportMapping.                                                                           |

### <a name="example"></a>Пример

В следующем примере показан **условие** элементы как дочерние элементы **MappingFragment** элементов. Когда **HireDate** не равно null и **EnrollmentDate** является null, то данные сопоставляются между **SchoolModel.Instructor** типа и **PersonID**и **HireDate** столбцы **Person** таблицы. Когда **EnrollmentDate** не равно null и **HireDate** является null, то данные сопоставляются между **SchoolModel.Student** типа и **PersonID** и **регистрации** столбцы **Person** таблицы.

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

## <a name="deletefunction-element-msl"></a>Элемент DeleteFunction (MSL)

**DeleteFunction** в язык определения сопоставлений (MSL) сопоставляет функцию удаления типа сущности или ассоциации в концептуальной модели с хранимой процедурой в основной базе данных. Хранимые процедуры, с которыми сопоставляются функции изменения, должны объявляться в модели хранения. Дополнительные сведения см. в разделе элемента функция (SSDL).

> [!NOTE]
> Если не выполнено сопоставление всех трех вставки, обновления или удаления типов сущностей с хранимыми процедурами, то несопоставленные операции завершится ошибкой, если во время выполнения и UpdateException возникает исключение.

### <a name="deletefunction-applied-to-entitytypemapping"></a>Элемент DeleteFunction, применяемый к EntityTypeMapping

При применении к элементу EntityTypeMapping **DeleteFunction** сопоставляет функцию удаления типа сущности в концептуальной модели с хранимой процедурой.

**DeleteFunction** элемент может иметь следующие дочерние элементы при применении к **EntityTypeMapping** элемент:

-   AssociationEnd (ноль и более)
-   ComplexProperty (ноль и более)
-   ScarlarProperty (ноль и более)

#### <a name="applicable-attributes"></a>Применимые атрибуты

В следующей таблице описаны атрибуты, которые могут применяться к **DeleteFunction** элемента при его применении к **EntityTypeMapping** элемент.

| Имя атрибута            | Необходимо | Значение                                                                                                                                                    |
|:--------------------------|:------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------|
| **FunctionName**          | Да         | Имя хранимой процедуры (с указанием пространства имен), с которой сопоставляется функция удаления. Хранимая процедура должна объявляться в модели хранения. |
| **RowsAffectedParameter** | Нет          | Имя выходного параметра, возвращающего количество обработанных строк.                                                                               |

#### <a name="example"></a>Пример

В следующем примере, основана на модели School и показывает **DeleteFunction** элемент сопоставления функции удаления **Person** тип сущности, **DeletePerson** Хранимая процедура. **DeletePerson** хранимая процедура объявлена в модели хранения.

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

### <a name="deletefunction-applied-to-associationsetmapping"></a>Элемент DeleteFunction, применяемый к AssociationSetMapping

При применении к элементу AssociationSetMapping **DeleteFunction** сопоставляет функцию удаления ассоциации в концептуальной модели с хранимой процедурой.

**DeleteFunction** элемент может иметь следующие дочерние элементы при применении к **AssociationSetMapping** элемент:

-   EndProperty

#### <a name="applicable-attributes"></a>Применимые атрибуты

В следующей таблице описаны атрибуты, которые могут применяться к **DeleteFunction** элемента при его применении к **AssociationSetMapping** элемент.

| Имя атрибута            | Необходимо | Значение                                                                                                                                                    |
|:--------------------------|:------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------|
| **FunctionName**          | Да         | Имя хранимой процедуры (с указанием пространства имен), с которой сопоставляется функция удаления. Хранимая процедура должна объявляться в модели хранения. |
| **RowsAffectedParameter** | Нет          | Имя выходного параметра, возвращающего количество обработанных строк.                                                                               |

#### <a name="example"></a>Пример

В следующем примере, основана на модели School и показывает **DeleteFunction** элемент, используемый для сопоставления функции удаления **CourseInstructor** взаимосвязи  **DeleteCourseInstructor** хранимой процедуры. **DeleteCourseInstructor** хранимая процедура объявлена в модели хранения.

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

## <a name="endproperty-element-msl"></a>Элемент EndProperty (MSL)

**EndProperty** элемент в язык определения сопоставлений (MSL) определяет сопоставление между функцией изменения ассоциации концептуальной модели и основной базе данных или концом. Сопоставление столбца свойств указано в дочерний элемент ScalarProperty.

Когда **EndProperty** элемент используется для определения сопоставления конца ассоциации концептуальной модели, он является дочерним элементом элемента AssociationSetMapping. Когда **EndProperty** элемент используется для определения сопоставления функции изменения ассоциации концептуальной модели, он является дочерним элемента InsertFunction или элемент DeleteFunction.

**EndProperty** элемент может иметь следующие дочерние элементы:

-   ScalarProperty (ноль и более)

### <a name="applicable-attributes"></a>Применимые атрибуты

В следующей таблице описаны атрибуты, применимые к **EndProperty** элемент:

| Имя атрибута | Необходимо | Значение                                                 |
|:---------------|:------------|:------------------------------------------------------|
| name           | Да         | Имя сопоставляемого конца ассоциации. |

### <a name="example"></a>Пример

В следующем примере показан **AssociationSetMapping** элемента, в котором **FK\_курс\_отдел** ассоциации в концептуальной модели сопоставляется с **Курс** таблицы в базе данных. Сопоставления между свойствами типов сопоставлений и столбцами таблицы указываются в дочерних **EndProperty** элементов.

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

### <a name="example"></a>Пример

В следующем примере показан **EndProperty** элемент сопоставления функции вставки и удаления ассоциации (**CourseInstructor**) с хранимыми процедурами в основной базе данных. Функции, с которыми выполняется сопоставление, объявляются в модели хранения.

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

## <a name="entitycontainermapping-element-msl"></a>Элемент EntityContainerMapping (MSL)

**EntityContainerMapping** в язык определения сопоставлений (MSL) сопоставляет контейнер сущностей в концептуальной модели контейнер сущностей в модели хранения. **EntityContainerMapping** элемент является дочерним элемента сопоставления.

**EntityContainerMapping** элемент может иметь следующие дочерние элементы (в указанном порядке):

-   EntitySetMapping (ноль и более)
-   AssociationSetMapping (ноль и более)
-   FunctionImportMapping (ноль и более)

### <a name="applicable-attributes"></a>Применимые атрибуты

В следующей таблице описаны атрибуты, которые могут применяться к **EntityContainerMapping** элемент.

| Имя атрибута            | Необходимо | Значение                                                                                                                                                                                                                                                    |
|:--------------------------|:------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **StorageModelContainer** | Да         | Имя контейнера сущностей модели хранения, с которым выполняется сопоставление.                                                                                                                                                                                     |
| **CdmEntityContainer**    | Да         | Имя контейнера сущностей концептуальной модели, с которым выполняется сопоставление.                                                                                                                                                                                  |
| **Generateupdateviews как**   | Нет          | **Значение true,** или **False**. Если **False**, представления обновлений не создаются. Этот атрибут должно быть присвоено **False** при наличии сопоставление только для чтения, которое может стать недействительным, так как данные могут быть не выполнить обратное преобразование успешно. <br/> Значение по умолчанию — **True**. |

### <a name="example"></a>Пример

В следующем примере показан **EntityContainerMapping** сопоставляет **SchoolModelEntities** контейнера (контейнер сущностей концептуальной модели) для  **SchoolModelStoreContainer** контейнера (контейнер сущностей модели хранения):

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

## <a name="entitysetmapping-element-msl"></a>Элемент EntitySetMapping (MSL)

**EntitySetMapping** задает элемент в языке MSL сопоставляет все типы в сущности концептуальной модели с наборами сущностей в модели хранения. Набор сущностей в концептуальной модели — это логический контейнер для экземпляров сущностей одного типа (и производных типов). Набор сущностей в модели хранения представляет таблицу или представление в основной базе данных. Набор сущностей концептуальной модели задается значение **имя** атрибут **EntitySetMapping** элемент. Таблица или представление определяется **StoreEntitySet** атрибут в каждый дочерний элемент MappingFragment или на **EntitySetMapping** сам элемент.

**EntitySetMapping** элемент может иметь следующие дочерние элементы:

-   EntityTypeMapping (ноль и более)
-   QueryView (ноль или один)
-   MappingFragment (ноль и более)

### <a name="applicable-attributes"></a>Применимые атрибуты

В следующей таблице описаны атрибуты, которые могут применяться к **EntitySetMapping** элемент.

| Имя атрибута           | Необходимо | Значение                                                                                                                                                                                                                         |
|:-------------------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Name**                 | Да         | Имя сопоставляемого набора сущностей концептуальной модели.                                                                                                                                                             |
| **TypeName** **1**       | Нет          | Имя сопоставляемого типа сущности концептуальной модели.                                                                                                                                                            |
| **StoreEntitySet** **1** | Нет          | Имя сопоставляемого набора сущностей модели хранения.                                                                                                                                                             |
| **MakeColumnsDistinct**  | Нет          | **Значение true,** или **False** в зависимости от того, возвращаются только уникальные строки. <br/> Если этот атрибут имеет значение **True**, **generateupdateviews как** атрибут элемента EntityContainerMapping должно быть присвоено **False**. |

 

**1** **TypeName** и **StoreEntitySet** атрибуты можно использовать вместо дочерние элементы EntityTypeMapping и MappingFragment для сопоставления один тип сущности с одной таблицей.

### <a name="example"></a>Пример

В следующем примере показан **EntitySetMapping** элемент, сопоставляющий три типа (базового типа и два производных типа) в **курсы** набор сущностей в концептуальной модели с тремя разными таблицами основной базе данных. Таблицы заданы **StoreEntitySet** атрибута в каждом **MappingFragment** элемент.

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

## <a name="entitytypemapping-element-msl"></a>Элемент EntityTypeMapping (MSL)

**EntityTypeMapping** элемент в язык определения сопоставлений (MSL) определяет сопоставление между типом сущности в концептуальной модели и таблицы или представления в основной базе данных. Сведения о типах сущностей концептуальной модели и базовых таблиц базы данных или представления см. в разделе элемент EntityType (CSDL) и элемент EntitySet (SSDL). Определяется тип сущности концептуальной модели, с которым выполняется сопоставление **TypeName** атрибут **EntityTypeMapping** элемент. Таблицы или представления, с которым выполняется сопоставление определяется **StoreEntitySet** атрибут MappingFragment дочернего элемента.

ModificationFunctionMapping дочерний элемент может использоваться для сопоставления вставки, обновления или удаления функций типов сущностей с хранимыми процедурами в базе данных.

**EntityTypeMapping** элемент может иметь следующие дочерние элементы:

-   MappingFragment (ноль и более)
-   ModificationFunctionMapping (ноль или один)
-   ScalarProperty
-   Условие

> [!NOTE]
> **MappingFragment** и **ModificationFunctionMapping** элементы не могут быть дочерними элементами элемента **EntityTypeMapping** элемент, в то же время.


> [!NOTE]
> **ScalarProperty** и **условие** элементов может быть только дочерние элементы **EntityTypeMapping** элемент, когда он используется внутри элемента FunctionImportMapping.

### <a name="applicable-attributes"></a>Применимые атрибуты

В следующей таблице описаны атрибуты, которые могут применяться к **EntityTypeMapping** элемент.

| Имя атрибута | Необходимо | Значение                                                                                                                                                                                                |
|:---------------|:------------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Имя типа**   | Да         | Имя с указанием пространства имен типа сущности концептуальной модели, с которым выполняется сопоставление. <br/> Если тип является абстрактным или производным типом, значение должно быть равно `IsOfType(Namespace-qualified_type_name)`. |

### <a name="example"></a>Пример

В следующем примере показано элемент EntitySetMapping с двумя дочерними **EntityTypeMapping** элементов. В первом **EntityTypeMapping** элемент, **SchoolModel.Person** сопоставляется с типом сущности **Person** таблицы. Во втором **EntityTypeMapping** элемент, поэтому функциональные возможности обновления **SchoolModel.Person** тип сопоставлен с хранимой процедурой, **UpdatePerson**, в базе данных .

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

### <a name="example"></a>Пример

В следующем примере показано сопоставление иерархии типа, в которой корневой тип является абстрактным. Обратите внимание на использование `IsOfType` синтаксис **TypeName** атрибуты.

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

## <a name="functionimportmapping-element-msl"></a>Элемент FunctionImportMapping (MSL)

**FunctionImportMapping** элемент в язык определения сопоставлений (MSL) определяет сопоставление между импортом функции в концептуальной модели и хранимой процедурой или функцией в основной базе данных. Импорт функций должен быть объявлен в концептуальной модели, а хранимые процедуры должны быть объявлены в модели хранения. Дополнительные сведения см. в разделе элемент FunctionImport (CSDL) и функция элемент (SSDL).

> [!NOTE]
> По умолчанию импорт функции возвращает тип сущности концептуальной модели или сложный тип. В этом случае имена столбцов, которые вернула основная хранимая процедура, должны точно соответствовать именам свойств в типе концептуальной модели. Если имена столбцов не точно совпадают с именами свойств, сопоставление должен быть определен в элементе ResultMapping.

**FunctionImportMapping** элемент может иметь следующие дочерние элементы:

-   ResultMapping (ноль и более)

### <a name="applicable-attributes"></a>Применимые атрибуты

В следующей таблице описаны атрибуты, применимые к **FunctionImportMapping** элемент:

| Имя атрибута         | Необходимо | Значение                                                                                   |
|:-----------------------|:------------|:----------------------------------------------------------------------------------------|
| **FunctionImportName** | Да         | Имя импорта функции в концептуальной модели, с которым выполняется сопоставление.           |
| **FunctionName**       | Да         | Имя импорта функции в модели хранения из полного пространства имен, с которым выполняется сопоставление. |

### <a name="example"></a>Пример

Следующий пример основан на модели School. Рассмотрим следующую функцию в модели хранения:

``` xml
 <Function Name="GetStudentGrades" Aggregate="false"
           BuiltIn="false" NiladicFunction="false"
           IsComposable="false" ParameterTypeSemantics="AllowImplicitConversion"
           Schema="dbo">
   <Parameter Name="StudentID" Type="int" Mode="In" />
 </Function>
```

Также рассмотрим данный импорт функции в концептуальной модели:

``` xml
 <FunctionImport Name="GetStudentGrades" EntitySet="StudentGrades"
                 ReturnType="Collection(SchoolModel.StudentGrade)">
   <Parameter Name="StudentID" Mode="In" Type="Int32" />
 </FunctionImport>
```

В следующем примере показан **FunctionImportMapping** элемент, используемый для сопоставления функции и импорта функции друг с другом:

``` xml
 <FunctionImportMapping FunctionImportName="GetStudentGrades"
                        FunctionName="SchoolModel.Store.GetStudentGrades" />
```
 
## <a name="insertfunction-element-msl"></a>Элемент InsertFunction (MSL)

**InsertFunction** в язык определения сопоставлений (MSL) сопоставляет функцию вставки типа сущности или ассоциации в концептуальной модели с хранимой процедурой в основной базе данных. Хранимые процедуры, с которыми сопоставляются функции изменения, должны объявляться в модели хранения. Дополнительные сведения см. в разделе элемента функция (SSDL).

> [!NOTE]
> Если не выполнено сопоставление всех трех вставки, обновления или удаления типов сущностей с хранимыми процедурами, то несопоставленные операции завершится ошибкой, если во время выполнения и UpdateException возникает исключение.

**InsertFunction** элемент может быть дочерним элементом ModificationFunctionMapping и применить к EntityTypeMapping-элемент или элемент AssociationSetMapping.

### <a name="insertfunction-applied-to-entitytypemapping"></a>Элемент InsertFunction, применяемый к EntityTypeMapping

При применении к элементу EntityTypeMapping **InsertFunction** сопоставляет функцию вставки типа сущности в концептуальной модели с хранимой процедурой.

**InsertFunction** элемент может иметь следующие дочерние элементы при применении к **EntityTypeMapping** элемент:

-   AssociationEnd (ноль и более)
-   ComplexProperty (ноль и более)
-   ResultBinding (ноль или один)
-   ScarlarProperty (ноль и более)

#### <a name="applicable-attributes"></a>Применимые атрибуты

В следующей таблице описаны атрибуты, которые могут применяться к **InsertFunction** элемент при применении к **EntityTypeMapping** элемент.

| Имя атрибута            | Необходимо | Значение                                                                                                                                                    |
|:--------------------------|:------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------|
| **FunctionName**          | Да         | Имя хранимой процедуры (с указанием пространства имен), с которой сопоставляется функция вставки. Хранимая процедура должна объявляться в модели хранения. |
| **RowsAffectedParameter** | Нет          | Имя выходного параметра, возвращающего количество обработанных строк.                                                                               |

#### <a name="example"></a>Пример

В следующем примере, основана на модели School и показывает **InsertFunction** элемент, используемый для сопоставления функции вставки типа сущности Person в **InsertPerson** хранимой процедуры. **InsertPerson** хранимая процедура объявлена в модели хранения.

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
### <a name="insertfunction-applied-to-associationsetmapping"></a>Элемент InsertFunction, применяемый к AssociationSetMapping

При применении к элементу AssociationSetMapping **InsertFunction** сопоставляет функцию вставки ассоциации в концептуальной модели с хранимой процедурой.

**InsertFunction** элемент может иметь следующие дочерние элементы при применении к **AssociationSetMapping** элемент:

-   EndProperty

#### <a name="applicable-attributes"></a>Применимые атрибуты

В следующей таблице описаны атрибуты, которые могут применяться к **InsertFunction** элемента при его применении к **AssociationSetMapping** элемент.

| Имя атрибута            | Необходимо | Значение                                                                                                                                                    |
|:--------------------------|:------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------|
| **FunctionName**          | Да         | Имя хранимой процедуры (с указанием пространства имен), с которой сопоставляется функция вставки. Хранимая процедура должна объявляться в модели хранения. |
| **RowsAffectedParameter** | Нет          | Имя выходного параметра, возвращающего количество обработанных строк.                                                                               |

#### <a name="example"></a>Пример

В следующем примере, основана на модели School и показывает **InsertFunction** элемент, используемый для сопоставления функции вставки **CourseInstructor** взаимосвязи  **InsertCourseInstructor** хранимой процедуры. **InsertCourseInstructor** хранимая процедура объявлена в модели хранения.

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

## <a name="mapping-element-msl"></a>Элемент сопоставления (MSL)

**Сопоставление** элемент в язык определения сопоставлений (MSL) содержит сведения для сопоставления объектов, определенных в концептуальной модели в базу данных (как описано в модели хранения). Дополнительные сведения см. в разделе спецификации языка CSDL и SSDL спецификации.

**Сопоставление** элемент является корневым элементом спецификации сопоставления. Пространство имен XML для спецификаций сопоставлений http://schemas.microsoft.com/ado/2009/11/mapping/cs.

У элемента Mapping могут быть следующие дочерние элементы (в порядке перечисления):

-   Псевдоним (ноль и более)
-   EntityContainerMapping (ровно один элемент)

Имена типов концептуальной модели и модели хранения, которые упоминаются в языке MSL, должны указываться вместе с именами соответствующих пространств имен. Сведения об имени пространства имен концептуальной модели см. в разделе элемент схемы (CSDL). Сведения об имени пространства имен модели хранения см. в разделе элемент схемы (SSDL). Элемент Alias можно определить псевдонимы для пространств имен, используемых в языке MSL.

### <a name="applicable-attributes"></a>Применимые атрибуты

В следующей таблице описаны атрибуты, которые могут применяться к **сопоставление** элемент.

| Имя атрибута | Необходимо | Значение                                                 |
|:---------------|:------------|:------------------------------------------------------|
| **ПРОБЕЛ**      | Да         | **C-S**. Это фиксированное значение, и его невозможно изменить. |

### <a name="example"></a>Пример

В следующем примере показан **сопоставление** элемент, основанный на модели School. Дополнительные сведения о модели School см. Краткое руководство (Entity Framework).

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

## <a name="mappingfragment-element-msl"></a>Элемент MappingFragment (MSL)

**MappingFragment** элемент в язык определения сопоставлений (MSL) определяет сопоставление между свойствами типа сущности концептуальной модели и таблицы или представления в базе данных. Сведения о типах сущностей концептуальной модели и базовых таблиц базы данных или представления см. в разделе элемент EntityType (CSDL) и элемент EntitySet (SSDL). **MappingFragment** может быть дочерним элементом элемента EntityTypeMapping-элемент или элемент EntitySetMapping.

**MappingFragment** элемент может иметь следующие дочерние элементы:

-   ComplexType (ноль и более)
-   ScalarProperty (ноль и более)
-   Условие (ноль и более)

### <a name="applicable-attributes"></a>Применимые атрибуты

В следующей таблице описаны атрибуты, которые могут применяться к **MappingFragment** элемент.

| Имя атрибута          | Необходимо | Значение                                                                                                                                                                                                                         |
|:------------------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **StoreEntitySet**      | Да         | Имя столбца таблицы или представления, с которым производится сопоставление.                                                                                                                                                                           |
| **MakeColumnsDistinct** | Нет          | **Значение true,** или **False** в зависимости от того, возвращаются только уникальные строки. <br/> Если этот атрибут имеет значение **True**, **generateupdateviews как** атрибут элемента EntityContainerMapping должно быть присвоено **False**. |

### <a name="example"></a>Пример

В следующем примере показан **MappingFragment** элемент дочерним **EntityTypeMapping** элемент. В этом примере свойства **курс** типа в концептуальной модели сопоставляются со столбцами из **курс** таблицы в базе данных.

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

### <a name="example"></a>Пример

В следующем примере показан **MappingFragment** элемент дочерним **EntitySetMapping** элемент. Как показано в примере выше свойства **курс** типа в концептуальной модели сопоставляются со столбцами из **курс** таблицы в базе данных.

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

## <a name="modificationfunctionmapping-element-msl"></a>Элемент ModificationFunctionMapping (MSL)

**ModificationFunctionMapping** в язык определения сопоставлений (MSL) сопоставляет вставки, обновления и удаления функций типа сущности концептуальной модели с хранимыми процедурами в основной базе данных. **ModificationFunctionMapping** элемент также можно сопоставить Вставка и удаление функций для многие ко многим ассоциаций в концептуальной модели с хранимыми процедурами в основной базе данных. Хранимые процедуры, с которыми сопоставляются функции изменения, должны объявляться в модели хранения. Дополнительные сведения см. в разделе элемента функция (SSDL).

> [!NOTE]
> Если не выполнено сопоставление всех трех вставки, обновления или удаления типов сущностей с хранимыми процедурами, то несопоставленные операции завершится ошибкой, если во время выполнения и UpdateException возникает исключение.


> [!NOTE]
> Если функции преобразования для одной сущности в иерархии наследования сопоставляются с хранимыми процедурами, то функции преобразования всех типов в иерархии должны быть сопоставлены с хранимыми процедурами.

**ModificationFunctionMapping** элемент может быть дочерний элемент EntityTypeMapping или AssociationSetMapping-элемент.

**ModificationFunctionMapping** элемент может иметь следующие дочерние элементы:

-   Элемент DeleteFunction (ноль или один)
-   Элемент InsertFunction (ноль или один)
-   UpdateFunction (ноль или один)

Нет атрибутов, применимых к **ModificationFunctionMapping** элемент.

### <a name="example"></a>Пример

В следующем примере показано сопоставление набора сущностей для **людей** набору сущностей в модели School. В дополнение к сопоставлению столбцов **Person** тип сущности, сопоставление вставки, обновления и удаления функций **Person** отображаются тип. Функции, с которыми выполняется сопоставление, объявляются в модели хранения.

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

### <a name="example"></a>Пример

В следующем примере показано сопоставление набора ассоциаций для **CourseInstructor** набор ассоциаций в модели School. В дополнение к сопоставлению столбцов **CourseInstructor** ассоциации, сопоставление функции insert и delete **CourseInstructor** показаны связи. Функции, с которыми выполняется сопоставление, объявляются в модели хранения.

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
 

 

## <a name="queryview-element-msl"></a>Элемент QueryView (MSL)

**QueryView** элемент в язык определения сопоставлений (MSL) определяет сопоставление только для чтения между типом сущности или ассоциации в концептуальной модели и таблицей в основной базе данных. Сопоставление определяется с помощью запроса Entity SQL, которое вычисляется по модели хранения и express результирующий набор с точки зрения сущность или ассоциацию в концептуальной модели. Представления запросов доступны только для чтения, поэтому для обновления типов, которые определены представлениями запросов, не могут применяться стандартные команды обновления. Представления запросов доступны только для чтения, поэтому для обновления типов, которые определены представлениями запросов, не могут применяться стандартные команды обновления. Дополнительные сведения см. в разделе Практическое: функции изменения сопоставления хранимых процедур.

> [!NOTE]
> В **QueryView** элемент, выражения Entity SQL, содержащие **GroupBy**, групповые статистические выражения и свойства навигации не поддерживаются.

 

**QueryView** элемент может быть дочерним элементом элемента EntitySetMapping или AssociationSetMapping-элемент. В первом случае представление запроса определяет сопоставление только для чтения для сущности в концептуальной модели. Во втором случае представление запроса определяет сопоставление только для чтения для связи в концептуальной модели.

> [!NOTE]
> Если **AssociationSetMapping** элемент предназначен для связи со ссылочным ограничением, **AssociationSetMapping** элемент игнорируется. Дополнительные сведения см. в разделе элемент ReferentialConstraint (CSDL).

**QueryView** элемент не может иметь дочерние элементы.

### <a name="applicable-attributes"></a>Применимые атрибуты

В следующей таблице описаны атрибуты, которые могут применяться к **QueryView** элемент.

| Имя атрибута | Необходимо | Значение                                                                         |
|:---------------|:------------|:------------------------------------------------------------------------------|
| **Имя типа**   | Нет          | Имя типа концептуальной модели, сопоставляемой представлением запроса. |

### <a name="example"></a>Пример

В следующем примере показан **QueryView** как дочерний элемент элемента **EntitySetMapping** элемент и определяет сопоставление представления запроса для **отдел** типа сущности в Модель School.

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

Поскольку запрос возвращает только подмножество членов **отдел** тип в модели хранения, **отдел** тип в модели School был изменен на базе этих сопоставлений следующим образом:

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

### <a name="example"></a>Пример

В следующем примере показан **QueryView** элемент дочерним **AssociationSetMapping** элемент и определяет сопоставление только для чтения для `FK_Course_Department` ассоциации в модели School.

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
 
### <a name="comments"></a>Комментарии

Представления запросов можно определять для реализации следующих сценариев.

-   Определение сущности в концептуальной модели, не включающей все свойства сущности в модели хранения. Сюда входят свойства, которые не имеют значения по умолчанию и не поддерживают **null** значения.
-   Сопоставление вычисляемых столбцов в модели хранения со свойствами типов сущностей в концептуальной модели.
-   Определение сопоставления, в котором условия, используемые для секционирования сущностей в концептуальной модели, не базируются на равенстве. При указании условного сопоставления с помощью **условие** элемент, указанное условие должно быть равно указанному значению. Дополнительные сведения см. в разделе элемент Condition (MSL).
-   Сопоставление одного столбца в модели хранения с несколькими типами в концептуальной модели.
-   Сопоставление нескольких типов с одной и той же таблицей.
-   Определение ассоциаций в концептуальной модели, базирующихся на внешних ключах реляционной схемы.
-   Применение пользовательской бизнес-логики для задания значений свойств в концептуальной модели. Например, можно сопоставить строковое значение «T» в источнике данных в значение **true**, логическое значение, в концептуальной модели.
-   Определение условных фильтров для результатов запроса.
-   Наложение меньшего числа ограничений на данные в концептуальной модели, нежели в модели хранения. Например, можно определить свойство в концептуальной модели допускает значения null даже если столбец, с которым сопоставляется не поддерживает **null**значения.

При определении представлений запросов для сущностей необходимо принимать во внимание следующие соображения.

-   Представления запросов доступны только для чтения. Обновление сущностей можно осуществлять только с помощью функций изменения.
-   В случае определения типа сущности через представление запроса необходимо также определять все связанные сущности через представления запросов.
-   При сопоставлении ассоциации многие ко многим сущности в модели хранения, которая представляет связанную таблицу в реляционной схеме, необходимо определить **QueryView** элемент в **AssociationSetMapping** элемент для данной таблицы связей.
-   Представления запросов должны быть определены для всех типов в иерархии типов. Это можно сделать следующими способами:
-   -   С помощью одного **QueryView** элемент, который указывает один запрос Entity SQL, которая возвращает объединение всех типов сущностей в иерархии.
    -   С помощью одного **QueryView** элемент, который указывает один запрос Entity SQL, использующий оператор CASE для возврата определенного типа сущности в иерархии на основе определенного условия.
    -   С помощью дополнительного **QueryView** элемента для конкретного типа в иерархии. В этом случае следует использовать **TypeName** атрибут **QueryView** элемента, чтобы указать тип сущности для каждого представления.
-   Когда представление запроса определено, нельзя указать **StorageSetName** атрибут **EntitySetMapping** элемент.
-   Когда представление запроса определено, **EntitySetMapping**элемент не может также содержать **свойство** сопоставления.

## <a name="resultbinding-element-msl"></a>Элемент ResultBinding (язык MSL)

**ResultBinding** в язык определения сопоставлений (MSL) сопоставляет значения столбцов, возвращаемых хранимыми процедурами свойствам сущности в концептуальной модели при сопоставлении функций изменения типа сущности хранимой процедуры в основной базе данных. Например, если возвращается значение столбца идентификаторов в инструкции insert хранимой процедуры, **ResultBinding** элемент возвращаемое значение сопоставляется со свойством типа сущности в концептуальной модели.

**ResultBinding** элемент может быть дочерним элементом элемента InsertFunction или элемент UpdateFunction.

**ResultBinding** элемент не может иметь дочерние элементы.

### <a name="applicable-attributes"></a>Применимые атрибуты

В следующей таблице описаны атрибуты, применимые к **ResultBinding** элемент:

| Имя атрибута | Необходимо | Значение                                                                         |
|:---------------|:------------|:------------------------------------------------------------------------------|
| **Name**       | Да         | Имя свойства сущности в концептуальной модели, с которым выполняется сопоставление. |
| **ColumnName** | Да         | Имя столбца, с которым выполнятся сопоставление.                                          |

### <a name="example"></a>Пример

В следующем примере, основана на модели School и показывает **InsertFunction** элемент, используемый для сопоставления функции вставки **Person** тип сущности, **InsertPerson** Хранимая процедура. ( **InsertPerson** хранимой процедуры ниже и объявлена в модели хранения.) Объект **ResultBinding** элемент используется для сопоставления значения столбца, который возвращается хранимой процедурой (**NewPersonID**) со свойством типа сущности (**PersonID**).

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

Следующий запрос Transact-SQL описывает **InsertPerson** хранимой процедуры:

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

## <a name="resultmapping-element-msl"></a>Элемент ResultMapping (язык MSL)

**ResultMapping** элемент в язык определения сопоставлений (MSL) определяет сопоставление между импортом функции в концептуальной модели и хранимой процедуры в основной базе данных, при выполнении следующих условий:

-   Импорт функции возвращает тип сущности концептуальной модели или сложный тип.
-   Имена столбцов, возвращаемые хранимой процедурой, не совпадают в точности с именами свойств в сложном типе или типе сущности.

По умолчанию сопоставление между столбцами, возвращаемыми хранимой процедурой, и сложным типом или типом сущности выполняется по именам столбцов и свойств. Если имена столбцов не точно совпадают с именами свойств, необходимо использовать **ResultMapping** элемент для определения сопоставления. Пример сопоставления по умолчанию см. в разделе элемент FunctionImportMapping (MSL).

**ResultMapping** элемент является дочерним элементом элемента FunctionImportMapping.

**ResultMapping** элемент может иметь следующие дочерние элементы:

-   EntityTypeMapping (ноль и более)
-   ComplexTypeMapping

Нет атрибутов, применимых к **ResultMapping** элемент.

### <a name="example"></a>Пример

Рассмотрим следующую хранимую процедуру:

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

Рассмотрим следующий тип сущности концептуальной модели:

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

Для создания импорта функции, возвращающей экземпляры предыдущего типа сущности, сопоставление между столбцами, возвращаемыми хранимой процедуры и тип сущности должен быть определен в **ResultMapping** элемент:

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

## <a name="scalarproperty-element-msl"></a>Элемент ScalarProperty (MSL)

**ScalarProperty** в язык определения сопоставлений (MSL) сопоставляет свойство типа сущности концептуальной модели, сложного типа или сопоставления столбца или параметра хранимой процедуры в базе данных.

> [!NOTE]
> Хранимые процедуры, с которыми сопоставляются функции изменения, должны объявляться в модели хранения. Дополнительные сведения см. в разделе элемента функция (SSDL).

**ScalarProperty** элемент может быть дочерним элементом следующих элементов:

-   MappingFragment
-   InsertFunction
-   UpdateFunction
-   DeleteFunction
-   EndProperty
-   ComplexProperty
-   ResultMapping

Как дочерний **MappingFragment**, **ComplexProperty**, или **EndProperty** элемент, **ScalarProperty** сопоставляет свойство в концептуальной модели со столбцом в базе данных. Как дочерний **InsertFunction**, **UpdateFunction**, или **DeleteFunction** элемент, **ScalarProperty** сопоставляет свойство в концептуальной модели с параметром хранимой процедуры.

**ScalarProperty** элемент не может иметь дочерние элементы.

### <a name="applicable-attributes"></a>Применимые атрибуты

Атрибуты, которые применяются к **ScalarProperty** элемент различаться в зависимости от роли элемента.

В следующей таблице описаны атрибуты, которые применяются, когда **ScalarProperty** элемент используется для сопоставления свойства концептуальной модели со столбцом в базе данных:

| Имя атрибута | Необходимо | Значение                                                           |
|:---------------|:------------|:----------------------------------------------------------------|
| **Name**       | Да         | Имя свойства концептуальной модели, с которым выполняется сопоставление. |
| **ColumnName** | Да         | Имя столбца таблицы, с которым производится сопоставление.              |

В следующей таблице описаны атрибуты, применимые к **ScalarProperty** элементу, если он используется для сопоставления свойства концептуальной модели с параметром хранимой процедуры:

| Имя атрибута    | Необходимо | Значение                                                                                                                                           |
|:------------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------|
| **Name**          | Да         | Имя свойства концептуальной модели, с которым выполняется сопоставление.                                                                                 |
| **Имя_параметра** | Да         | Имя сопоставляемого параметра.                                                                                                 |
| **Version**       | Нет          | **Текущий** или **исходного** в зависимости от того, является ли текущее значение или исходное значение свойства должен использоваться для проверки на параллелизм. |

### <a name="example"></a>Пример

В следующем примере показан **ScalarProperty** элемента, используемого одним из двух способов:

-   Для сопоставления свойства **Person** тип сущности со столбцами **Person**таблицы.
-   Для сопоставления свойства **Person** типа сущности с параметрами **UpdatePerson** хранимой процедуры. Хранимые процедуры объявляются в модели хранения.

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

### <a name="example"></a>Пример

В следующем примере показан **ScalarProperty** элемент, используемый для сопоставления вставки и удаления ассоциации концептуальной модели с хранимыми процедурами в базе данных функции. Хранимые процедуры объявляются в модели хранения.

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

## <a name="updatefunction-element-msl"></a>Элемент UpdateFunction (MSL)

**UpdateFunction** в язык определения сопоставлений (MSL) сопоставляет функцию обновления типа сущности в концептуальной модели с хранимой процедурой в основной базе данных. Хранимые процедуры, с которыми сопоставляются функции изменения, должны объявляться в модели хранения. Дополнительные сведения см. в разделе элемента функция (SSDL).

> [!NOTE]
>  Если не выполнено сопоставление всех трех вставки, обновления или удаления типов сущностей с хранимыми процедурами, то несопоставленные операции завершится ошибкой, если во время выполнения и UpdateException возникает исключение.

**UpdateFunction** элемент может быть дочерним элементом ModificationFunctionMapping и применяется к элементу EntityTypeMapping.

**UpdateFunction** элемент может иметь следующие дочерние элементы:

-   AssociationEnd (ноль и более)
-   ComplexProperty (ноль и более)
-   ResultBinding (ноль или один)
-   ScarlarProperty (ноль и более)

### <a name="applicable-attributes"></a>Применимые атрибуты

В следующей таблице описаны атрибуты, которые могут применяться к **UpdateFunction** элемент.

| Имя атрибута            | Необходимо | Значение                                                                                                                                                    |
|:--------------------------|:------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------|
| **FunctionName**          | Да         | Имя хранимой процедуры (с указанием пространства имен), с которой сопоставляется функция обновления. Хранимая процедура должна объявляться в модели хранения. |
| **RowsAffectedParameter** | Нет          | Имя выходного параметра, возвращающего количество обработанных строк.                                                                               |

### <a name="example"></a>Пример

В следующем примере, основана на модели School и показывает **UpdateFunction** элемент, используемый для сопоставления функции обновления **Person** тип сущности, **UpdatePerson** Хранимая процедура. **UpdatePerson** хранимая процедура объявлена в модели хранения.

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
