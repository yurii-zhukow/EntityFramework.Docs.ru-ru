---
title: Спецификация MSL — EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 13ae7bc1-74b4-4ee4-8d73-c337be841467
ms.openlocfilehash: 8990d1373ea2121ce11337a43dbcdf3b9e1532bd
ms.sourcegitcommit: 708b18520321c587b2046ad2ea9fa7c48aeebfe5
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/09/2019
ms.locfileid: "72182555"
---
# <a name="msl-specification"></a>Спецификация MSL
Язык MSL — это язык на основе XML, который описывает сопоставление между концептуальной моделью и моделью хранения Entity Framework приложения.

В приложении Entity Framework метаданные сопоставления загружаются из MSL-файла (написанного на языке MSL) во время сборки. Entity Framework использует метаданные сопоставления во время выполнения для преобразования запросов к концептуальной модели для хранения команд, относящихся к хранению.

Entity Framework Designer (конструктор EF) сохраняет сведения о сопоставлении в EDMX-файле во время разработки. Во время сборки Entity Designer использует сведения в EDMX-файле для создания MSL-файла, необходимого для Entity Framework во время выполнения.

Имена всех типов концептуальной модели или модели хранения, которые упоминаются в языке MSL, должны указываться вместе с именами соответствующих пространств имен. Дополнительные сведения об имени пространства имен концептуальной модели см. в разделе [Спецификация языка CSDL](~/ef6/modeling/designer/advanced/edmx/csdl-spec.md). Дополнительные сведения об имени пространства имен модели хранения см. в разделе [Спецификация языка SSDL](~/ef6/modeling/designer/advanced/edmx/ssdl-spec.md).

Версии MSL отличаются по пространствам имен XML.

| Версия MSL | Пространство имен XML                                        |
|:------------|:-----------------------------------------------------|
| MSL v1      | urn: schemas-microsoft-com: Windows: хранилище: сопоставление: CS |
| MSL v2      | https://schemas.microsoft.com/ado/2008/09/mapping/cs |
| MSL v3      | https://schemas.microsoft.com/ado/2009/11/mapping/cs  |

## <a name="alias-element-msl"></a>Элемент Alias (язык MSL)

Элемент **Alias** в языке MSL является дочерним по отношению к элементу Mapping, который используется для определения псевдонимов для концептуальной модели и пространств имен модели хранения. Имена всех типов концептуальной модели или модели хранения, которые упоминаются в языке MSL, должны указываться вместе с именами соответствующих пространств имен. Дополнительные сведения об имени пространства имен концептуальной модели см. в разделе элемент Schema (CSDL). Дополнительные сведения об имени пространства имен модели хранения см. в разделе элемент Schema (SSDL).

Элемент **Alias** не может иметь дочерние элементы.

### <a name="applicable-attributes"></a>Применимые атрибуты

В следующей таблице описаны атрибуты, которые можно применить к элементу **Alias** .

| Имя атрибута | Необходимо | Значение                                                                     |
|:---------------|:------------|:--------------------------------------------------------------------------|
| **Key**        | Да         | Псевдоним для пространства имен, определяемого атрибутом **value** . |
| **Значение**      | Да         | Пространство имен, для которого значение элемента **Key** является псевдонимом.     |

### <a name="example"></a>Пример

В следующем примере показан элемент **Alias** , определяющий псевдоним, `c`для типов, определенных в концептуальной модели.

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

## <a name="associationend-element-msl"></a>Элемент AssociationEnd (MSL)

Элемент **AssociationEnd** на языке MSL используется, когда функции изменения типа сущности в концептуальной модели сопоставляются с хранимыми процедурами в базовой базе данных. Если хранимая процедура изменения принимает параметр, значение которого хранится в свойстве Association, элемент **AssociationEnd** сопоставляет значение свойства с параметром. Дополнительные сведения см. в приведенных ниже примерах.

Дополнительные сведения о сопоставлении функций изменения типов сущностей с хранимыми процедурами см. в разделе элемент ModificationFunctionMapping (MSL) и пошаговое руководство. Сопоставление сущности с хранимыми процедурами.

Элемент **AssociationEnd** может иметь следующие дочерние элементы:

-   ScalarProperty

### <a name="applicable-attributes"></a>Применимые атрибуты

В следующей таблице описаны атрибуты, применимые к элементу **AssociationEnd** .

| Имя атрибута     | Необходимо | Значение                                                                                                                                                                             |
|:-------------------|:------------|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **AssociationSet** | Да         | Имя сопоставляемой ассоциации.                                                                                                                                 |
| **From**           | Да         | Значение атрибута **FromRole** свойства навигации, соответствующего сопоставленной ассоциации. Дополнительные сведения см. в разделе Элемент NavigationProperty (CSDL). |
| **Задача**             | Да         | Значение атрибута **ToRole** свойства навигации, соответствующего сопоставленной ассоциации. Дополнительные сведения см. в разделе Элемент NavigationProperty (CSDL).   |

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

Чтобы связать функцию Update сущности `Course` с этой хранимой процедурой, необходимо указать значение для параметра **DepartmentID** . Значение для `DepartmentID` не соответствует свойству типа сущности. Оно содержится в независимом сопоставлении с сопоставлением, показанным ниже:

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

В следующем коде показан элемент **AssociationEnd** , используемый для сопоставления свойства **DepartmentID** **\_ного курса FK\_Course** с хранимой процедурой **упдатекаурсе** (для которой сопоставлена функция Update типа сущности **Course** ):

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

Элемент **AssociationSetMapping** на языке MSL определяет сопоставление между Ассоциацией в концептуальной модели и столбцами таблицы в базовой базе данных.

Сопоставления в концептуальной модели — это типы, свойства которых представляют столбцы первичного и внешнего ключа в основной базе данных. Элемент **AssociationSetMapping** использует два элемента EndProperty для определения сопоставлений между свойствами и столбцами типа взаимосвязи в базе данных. Можно поместить условия сопоставления в элемент Condition. Сопоставьте функции вставки, обновления и удаления сопоставлений с хранимыми процедурами в базе данных с помощью элемента ModificationFunctionMapping. Определение сопоставлений только для чтения между ассоциациями и столбцами таблицы с помощью Entity SQL строки в элементе QueryView.

> [!NOTE]
> Если для ассоциации в концептуальной модели определено ссылочное ограничение, Ассоциация не должна сопоставляться с элементом **AssociationSetMapping** . Если для ассоциации, имеющей справочное ограничение, имеется элемент **AssociationSetMapping** , то сопоставления, определенные в элементе **AssociationSetMapping** , будут игнорироваться. Дополнительные сведения см. в разделе элемент ReferentialConstraint (CSDL).

Элемент **AssociationSetMapping** может иметь следующие дочерние элементы

-   Представление QueryView (ноль или один)
-   EndProperty (ноль или два)
-   Условие (ноль или более)
-   ModificationFunctionMapping (ноль или один)

### <a name="applicable-attributes"></a>Применимые атрибуты

В следующей таблице описаны атрибуты, которые можно применить к элементу **AssociationSetMapping** .

| Имя атрибута     | Необходимо | Значение                                                                                       |
|:-------------------|:------------|:--------------------------------------------------------------------------------------------|
| **Name**           | Да         | Имя набора ассоциаций концептуальной модели, с которым выполняется сопоставление.                      |
| **Имени**       | Нет          | Имя типа ассоциации концептуальной модели из полного пространства имен, с которым выполняется сопоставление. |
| **сторинтитисет** | Нет          | Имя таблицы, с которой производится сопоставление.                                                 |

### <a name="example"></a>Пример

В следующем примере показан элемент **AssociationSetMapping** , в котором набор ассоциаций " **\_FK"\_** "в концептуальной модели сопоставлен с таблицей **Course** в базе данных. Сопоставления между свойствами типа ассоциации и столбцами таблицы указываются в дочерних элементах **EndProperty** .

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

Элемент **комплекспроперти** на языке MSL определяет сопоставление между свойством сложного типа в типе сущности концептуальной модели и столбцами таблицы в базовой базе данных. Сопоставления столбца свойств указаны в дочерних элементах ScalarProperty.

Элемент свойства **complexType** может иметь следующие дочерние элементы:

-   ScalarProperty (ноль или более)
-   **Комплекспроперти** (ноль или более)
-   Комплексттипемаппинг (ноль или более)
-   Условие (ноль или более)

### <a name="applicable-attributes"></a>Применимые атрибуты

В следующей таблице описаны атрибуты, применимые к элементу **комплекспроперти** .

| Имя атрибута | Необходимо | Значение                                                                                            |
|:---------------|:------------|:-------------------------------------------------------------------------------------------------|
| **Name**       | Да         | Имя сложного свойства типа сущности в концептуальной модели, с которым выполняется сопоставление. |
| **Имени**   | Нет          | Полное имя пространства имен типа свойства концептуальной модели.                              |

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

Свойства **LastName** и **FirstName** типа сущности **Person** заменены одним сложным свойством, **именем**:

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

В следующем MSL показан элемент **комплекспроперти** , используемый для отображения свойства **Name** в столбцах базовой базы данных:

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

Элемент **комплекстипемаппинг** в языке MSL является дочерним элементом элемента ресултмаппинг и определяет сопоставление между импортом функции в концептуальной модели и хранимой процедурой в базовой базе данных, если выполняются следующие условия.

-   Импорт функции возвращает концептуальный сложный тип.
-   Имена столбцов, возвращаемые хранимой процедурой, не соответствуют в точности именам свойств в сложном типе.

По умолчанию сопоставление между столбцами, возвращаемыми хранимой процедурой, и сложным типом основано на именах столбцов и свойств. Если имена столбцов не точно соответствуют именам свойств, для определения сопоставления необходимо использовать элемент **комплекстипемаппинг** . Пример сопоставления по умолчанию см. в разделе элемент FunctionImportMapping (MSL).

Элемент **комплекстипемаппинг** может иметь следующие дочерние элементы:

-   ScalarProperty (ноль или более)

### <a name="applicable-attributes"></a>Применимые атрибуты

В следующей таблице описаны атрибуты, применимые к элементу **комплекстипемаппинг** .

| Имя атрибута | Необходимо | Значение                                                                  |
|:---------------|:------------|:-----------------------------------------------------------------------|
| **Имени**   | Да         | Имя сопоставляемого сложного типа с указанием пространства имен. |

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

Чтобы создать импорт функции, возвращающий экземпляры предыдущего сложного типа, сопоставление столбцов, возвращаемых хранимой процедурой и типом сущности, должно быть определено в элементе **комплекстипемаппинг** :

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

Элемент **Condition** на языке MSL размещает условия для сопоставлений между концептуальной моделью и базовой базой данных. Сопоставление, определенное в узле XML, допустимо, если выполняются все условия, указанные в дочерних элементах **Condition** . В противном случае сопоставление недействительно. Например, если элемент MappingFragment содержит одно или несколько дочерних элементов **Condition** , сопоставление, определенное в узле **MappingFragment** , будет действительным только в том случае, если выполняются все условия дочерних элементов **Condition** .

Каждое условие может применяться либо к **имени** (имени свойства сущности концептуальной модели, заданному атрибутом **Name** ), либо к **ColumnName** (имени столбца в базе данных, указанному атрибутом **ColumnName** ). Если задан атрибут **Name** , условие проверяется по значению свойства сущности. Если атрибут **ColumnName** установлен, условие проверяется на соответствие значению столбца. В элементе **Condition** можно указать только одно из атрибутов **Name** или **ColumnName** .

> [!NOTE]
> Если элемент **Condition** используется в элементе FunctionImportMapping, то только атрибут **Name** неприменим.

Элемент **Condition** может быть дочерним для следующих элементов:

-   AssociationSetMapping
-   ComplexProperty
-   EntitySetMapping
-   MappingFragment
-   EntityTypeMapping

Элемент **Condition** не может иметь дочерних элементов.

### <a name="applicable-attributes"></a>Применимые атрибуты

В следующей таблице описаны атрибуты, применимые к элементу **Condition** .

| Имя атрибута | Необходимо | Значение                                                                                                                                                                                                                                                                                         |
|:---------------|:------------|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **имя_столбца** | Нет          | Имя столбца таблицы, значение которого используется для оценки условия.                                                                                                                                                                                                                   |
| **IsNull**     | Нет          | **True** или **false**. Если значение равно **true** , а значение столбца равно **null**или если значение равно **false** , а значение столбца не равно **null**, условие имеет значение true. В противном случае условие имеет значение false. <br/> Атрибуты **IsNull** и **value** нельзя использовать одновременно. |
| **Значение**      | Нет          | Значение, с которым сравнивается значение столбца. Если значения совпадают, условие верно. В противном случае условие имеет значение false. <br/> Атрибуты **IsNull** и **value** нельзя использовать одновременно.                                                                       |
| **Name**       | Нет          | Имя свойства сущности концептуальной модели, значение которого используется для оценки условия. <br/> Этот атрибут неприменим, если элемент **Condition** используется в элементе FunctionImportMapping.                                                                           |

### <a name="example"></a>Пример

В следующем примере элементы **Condition** показаны как дочерние элементы элементов **MappingFragment** . Если параметр **HireDate** не равен null и **енроллментдате** имеет значение null, данные сопоставляются между типом **счулмодел. Instructor** и столбцами **PersonID** и **HireDate** таблицы **Person** . Если **енроллментдате** имеет значение NOT NULL, а значение **HireDate** равно null, то данные сопоставляются между типом **счулмодел. Student** и столбцами **PersonID** и **регистрации** таблицы **Person** .

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

Элемент **DeleteFunction** в языке MSL сопоставляет функцию Delete типа сущности или ассоциации в концептуальной модели с хранимой процедурой в базовой базе данных. Хранимые процедуры, с которыми сопоставляются функции изменения, должны объявляться в модели хранения. Дополнительные сведения см. в разделе элемент Function (SSDL).

> [!NOTE]
> Если не выполнить сопоставление всех трех операций вставки, обновления или удаления типа сущности с хранимыми процедурами, несопоставленные операции завершатся ошибкой, если они выполняются во время выполнения и создается исключение Упдатиксцептион.

### <a name="deletefunction-applied-to-entitytypemapping"></a>Элемент DeleteFunction, применяемый к EntityTypeMapping

При применении к элементу EntityTypeMapping элемент **DeleteFunction** сопоставляет функцию Delete типа сущности в концептуальной модели с хранимой процедурой.

Элемент **DeleteFunction** может иметь следующие дочерние элементы при применении к элементу **EntityTypeMapping** :

-   AssociationEnd (ноль или более)
-   ComplexProperty (ноль и более)
-   Скарларпроперти (ноль или более)

#### <a name="applicable-attributes"></a>Применимые атрибуты

В следующей таблице описаны атрибуты, которые можно применить к элементу **DeleteFunction** при его применении к элементу **EntityTypeMapping** .

| Имя атрибута            | Необходимо | Значение                                                                                                                                                    |
|:--------------------------|:------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------|
| **FunctionName**          | Да         | Имя хранимой процедуры (с указанием пространства имен), с которой сопоставляется функция удаления. Хранимая процедура должна объявляться в модели хранения. |
| **ровсаффектедпараметер** | Нет          | Имя выходного параметра, возвращающего количество обработанных строк.                                                                               |

#### <a name="example"></a>Пример

Следующий пример основан на модели School и показывает элемент **DeleteFunction** , сопоставленный функции Delete типа сущности **Person** с хранимой процедурой **делетеперсон** . Хранимая процедура **делетеперсон** объявляется в модели хранения.

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

При применении к элементу AssociationSetMapping элемент **DeleteFunction** сопоставляет функцию Delete ассоциации в концептуальной модели с хранимой процедурой.

Элемент **DeleteFunction** может иметь следующие дочерние элементы при применении к элементу **AssociationSetMapping** :

-   EndProperty

#### <a name="applicable-attributes"></a>Применимые атрибуты

В следующей таблице описаны атрибуты, которые можно применить к элементу **DeleteFunction** при его применении к элементу **AssociationSetMapping** .

| Имя атрибута            | Необходимо | Значение                                                                                                                                                    |
|:--------------------------|:------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------|
| **FunctionName**          | Да         | Имя хранимой процедуры (с указанием пространства имен), с которой сопоставляется функция удаления. Хранимая процедура должна объявляться в модели хранения. |
| **ровсаффектедпараметер** | Нет          | Имя выходного параметра, возвращающего количество обработанных строк.                                                                               |

#### <a name="example"></a>Пример

Следующий пример основан на модели School и показывает элемент **DeleteFunction** , используемый для сопоставления функции Delete ассоциации **каурсеинструктор** с хранимой процедурой **делетекаурсеинструктор** . Хранимая процедура **делетекаурсеинструктор** объявляется в модели хранения.

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

Элемент **EndProperty** на языке MSL определяет сопоставление между функцией End или модификации ассоциации концептуальной модели и базовой базы данных. Сопоставление столбца свойств указано в дочернем элементе ScalarProperty.

Если элемент **EndProperty** используется для определения сопоставления в конце ассоциации концептуальной модели, он является дочерним элементом элемента AssociationSetMapping. Если элемент **EndProperty** используется для определения сопоставления для функции изменения ассоциации концептуальной модели, это дочерний элемент элемента InsertFunction или DeleteFunction.

Элемент **EndProperty** может иметь следующие дочерние элементы:

-   ScalarProperty (ноль или более)

### <a name="applicable-attributes"></a>Применимые атрибуты

В следующей таблице описаны атрибуты, применимые к элементу **EndProperty** .

| Имя атрибута | Необходимо | Значение                                                 |
|:---------------|:------------|:------------------------------------------------------|
| Имя           | Да         | Имя сопоставляемого конца ассоциации. |

### <a name="example"></a>Пример

В следующем примере показан элемент **AssociationSetMapping** , в котором отношение **FK\_Course\_ассоциацией между подразделениями** в концептуальной модели сопоставляется с таблицей **Course** в базе данных. Сопоставления между свойствами типа ассоциации и столбцами таблицы указываются в дочерних элементах **EndProperty** .

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

В следующем примере показан элемент **EndProperty** , сопоставленный с функциями INSERT и DELETE ассоциации (**каурсеинструктор**) с хранимыми процедурами в базовой базе данных. Функции, с которыми выполняется сопоставление, объявляются в модели хранения.

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

Элемент **EntityContainerMapping** на языке MSL сопоставляет контейнер сущностей в концептуальной модели с контейнером сущностей в модели хранения. Элемент **EntityContainerMapping** является дочерним по отношению к элементу Mapping.

Элемент **EntityContainerMapping** может иметь следующие дочерние элементы (в указанном порядке):

-   EntitySetMapping (ноль и более)
-   AssociationSetMapping (ноль или более)
-   FunctionImportMapping (ноль или более)

### <a name="applicable-attributes"></a>Применимые атрибуты

В следующей таблице описаны атрибуты, которые можно применить к элементу **EntityContainerMapping** .

| Имя атрибута            | Необходимо | Значение                                                                                                                                                                                                                                                    |
|:--------------------------|:------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **сторажемоделконтаинер** | Да         | Имя контейнера сущностей модели хранения, с которым выполняется сопоставление.                                                                                                                                                                                     |
| **кдментитиконтаинер**    | Да         | Имя контейнера сущностей концептуальной модели, с которым выполняется сопоставление.                                                                                                                                                                                  |
| **женератеупдатевиевс**   | Нет          | **True** или **false**. Если задано **значение false**, представления обновлений не создаются. Этот атрибут должен иметь значение **false** , если имеется сопоставление только для чтения, которое будет недействительным, так как данные могут не пройти циклический обмен данными. <br/> По умолчанию используется значение **True**. |

### <a name="example"></a>Пример

В следующем примере показан элемент **EntityContainerMapping** , который сопоставляет контейнер **счулмоделентитиес** (контейнер сущностей концептуальной модели) с контейнером **счулмоделстореконтаинер** (контейнер сущностей модели хранения):

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

Элемент **EntitySetMapping** на языке MSL сопоставляет все типы сущности концептуальной модели наборам сущностей в модели хранения. Набор сущностей в концептуальной модели представляет собой логический контейнер для экземпляров сущностей одного типа (и производных типов). Набор сущностей в модели хранения представляет таблицу или представление в основной базе данных. Набор сущностей концептуальной модели задается значением атрибута **Name** элемента **EntitySetMapping** . Сопоставленная таблица или представление задаются атрибутом **сторинтитисет** в каждом дочернем элементе MappingFragment или в самом элементе **EntitySetMapping** .

Элемент **EntitySetMapping** может иметь следующие дочерние элементы:

-   EntityTypeMapping (ноль и более)
-   Представление QueryView (ноль или один)
-   MappingFragment (ноль или более)

### <a name="applicable-attributes"></a>Применимые атрибуты

В следующей таблице описаны атрибуты, которые можно применить к элементу **EntitySetMapping** .

| Имя атрибута           | Необходимо | Значение                                                                                                                                                                                                                         |
|:-------------------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Name**                 | Да         | Имя сопоставляемого набора сущностей концептуальной модели.                                                                                                                                                             |
| **Имя типа** **1**       | Нет          | Имя сопоставляемого типа сущности концептуальной модели.                                                                                                                                                            |
| **Сторинтитисет** **1** | Нет          | Имя сопоставляемого набора сущностей модели хранения.                                                                                                                                                             |
| **Атрибутом makecolumnsdistinct**  | Нет          | **Значение true** или **false** в зависимости от того, возвращаются ли только отдельные строки. <br/> Если для этого атрибута задано значение **true**, атрибуту **женератеупдатевиевс** элемента EntityContainerMapping должно быть присвоено значение **false**. |

 

**1** для отображения одного типа сущности в одну таблицу можно использовать атрибуты **TypeName** и **сторинтитисет** вместо дочерних элементов EntityTypeMapping и MappingFragment.

### <a name="example"></a>Пример

В следующем примере показан элемент **EntitySetMapping** , сопоставляющий три типа (базовый тип и два производных типа) в наборе сущностей **Courses** концептуальной модели с тремя различными таблицами в основной базе данных. Таблицы задаются атрибутом **сторинтитисет** в каждом элементе **MappingFragment** .

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

Элемент **EntityTypeMapping** на языке MSL определяет сопоставление между типом сущности в концептуальной модели и таблицах или представлениях в базовой базе данных. Сведения о типах сущностей концептуальной модели и базовых таблицах или представлениях базы данных см. в разделах Элемент EntityType (язык CSDL) и Элемент EntitySet (язык SSDL). Сопоставляемый тип сущности концептуальной модели задается атрибутом **TypeName** элемента **EntityTypeMapping** . Сопоставляемая таблица или представление задается атрибутом **сторинтитисет** дочернего элемента MappingFragment.

Дочерний элемент ModificationFunctionMapping может применяться для сопоставления функций вставки, обновления или удаления типов сущностей с хранимыми процедурами в базе данных.

Элемент **EntityTypeMapping** может иметь следующие дочерние элементы:

-   MappingFragment (ноль или более)
-   ModificationFunctionMapping (ноль или один)
-   ScalarProperty
-   Условие

> [!NOTE]
> Элементы **MappingFragment** и **ModificationFunctionMapping** не могут одновременно быть дочерними элементами элемента **EntityTypeMapping** .


> [!NOTE]
> Элементы **ScalarProperty** и **Condition** могут быть дочерними элементами элемента **EntityTypeMapping** , когда он используется в элементе FunctionImportMapping.

### <a name="applicable-attributes"></a>Применимые атрибуты

В следующей таблице описаны атрибуты, которые можно применить к элементу **EntityTypeMapping** .

| Имя атрибута | Необходимо | Значение                                                                                                                                                                                                |
|:---------------|:------------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Имени**   | Да         | Имя с указанием пространства имен типа сущности концептуальной модели, с которым выполняется сопоставление. <br/> Если тип является абстрактным или производным типом, значение должно быть равно `IsOfType(Namespace-qualified_type_name)`. |

### <a name="example"></a>Пример

В следующем примере показан элемент EntitySetMapping с двумя дочерними элементами **EntityTypeMapping** . В первом элементе **EntityTypeMapping** тип сущности **счулмодел. Person** сопоставляется с таблицей **Person** . Во втором элементе **EntityTypeMapping** функция обновления типа **счулмодел. Person** сопоставляется с хранимой процедурой **упдатеперсон**в базе данных.

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

В следующем примере показано сопоставление иерархии типа, в которой корневой тип является абстрактным. Обратите внимание на использование синтаксиса `IsOfType` для атрибутов **TypeName** .

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

Элемент **FunctionImportMapping** на языке MSL определяет сопоставление между импортом функции в концептуальной модели и хранимой процедурой или функцией в базовой базе данных. Импорт функций должен быть объявлен в концептуальной модели, а хранимые процедуры должны быть объявлены в модели хранения. Дополнительные сведения см. в разделе элемент FunctionImport (CSDL) и элемент Function (SSDL).

> [!NOTE]
> По умолчанию импорт функции возвращает тип сущности концептуальной модели или сложный тип. В этом случае имена столбцов, которые вернула основная хранимая процедура, должны точно соответствовать именам свойств в типе концептуальной модели. Если имена столбцов не точно совпадают с именами свойств, сопоставление должно быть определено в элементе Ресултмаппинг.

Элемент **FunctionImportMapping** может иметь следующие дочерние элементы:

-   Ресултмаппинг (ноль или более)

### <a name="applicable-attributes"></a>Применимые атрибуты

В следующей таблице описаны атрибуты, применимые к элементу **FunctionImportMapping** .

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

В следующем примере показан элемент **FunctionImportMapping** , используемый для отображения функции и импорта функции выше.

``` xml
 <FunctionImportMapping FunctionImportName="GetStudentGrades"
                        FunctionName="SchoolModel.Store.GetStudentGrades" />
```
 
## <a name="insertfunction-element-msl"></a>Элемент InsertFunction (MSL)

Элемент **InsertFunction** в языке MSL сопоставляет функцию INSERT типа сущности или ассоциацию в концептуальной модели с хранимой процедурой в базовой базе данных. Хранимые процедуры, с которыми сопоставляются функции изменения, должны объявляться в модели хранения. Дополнительные сведения см. в разделе элемент Function (SSDL).

> [!NOTE]
> Если не выполнить сопоставление всех трех операций вставки, обновления или удаления типа сущности с хранимыми процедурами, несопоставленные операции завершатся ошибкой, если они выполняются во время выполнения и создается исключение Упдатиксцептион.

Элемент **InsertFunction** может быть дочерним элементом элемента ModificationFunctionMapping и применен к элементу EntityTypeMapping или элементу AssociationSetMapping.

### <a name="insertfunction-applied-to-entitytypemapping"></a>Элемент InsertFunction, применяемый к EntityTypeMapping

При применении к элементу EntityTypeMapping элемент **InsertFunction** сопоставляет функцию вставки типа сущности в концептуальной модели с хранимой процедурой.

Элемент **InsertFunction** может иметь следующие дочерние элементы при применении к элементу **EntityTypeMapping** :

-   AssociationEnd (ноль или более)
-   ComplexProperty (ноль и более)
-   ResultBinding (ноль или один)
-   Скарларпроперти (ноль или более)

#### <a name="applicable-attributes"></a>Применимые атрибуты

В следующей таблице описаны атрибуты, которые можно применить к элементу **InsertFunction** при применении к элементу **EntityTypeMapping** .

| Имя атрибута            | Необходимо | Значение                                                                                                                                                    |
|:--------------------------|:------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------|
| **FunctionName**          | Да         | Имя хранимой процедуры (с указанием пространства имен), с которой сопоставляется функция вставки. Хранимая процедура должна объявляться в модели хранения. |
| **ровсаффектедпараметер** | Нет          | Имя выходного параметра, возвращающего количество обработанных строк.                                                                               |

#### <a name="example"></a>Пример

Следующий пример основан на модели School и показывает элемент **InsertFunction** , используемый для преобразования функции вставки типа сущности Person в хранимую процедуру **инсертперсон** . Хранимая процедура **инсертперсон** объявляется в модели хранения.

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

При применении к элементу AssociationSetMapping элемент **InsertFunction** сопоставляет функцию вставки ассоциации в концептуальной модели с хранимой процедурой.

Элемент **InsertFunction** может иметь следующие дочерние элементы при применении к элементу **AssociationSetMapping** :

-   EndProperty

#### <a name="applicable-attributes"></a>Применимые атрибуты

В следующей таблице описаны атрибуты, которые можно применить к элементу **InsertFunction** при его применении к элементу **AssociationSetMapping** .

| Имя атрибута            | Необходимо | Значение                                                                                                                                                    |
|:--------------------------|:------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------|
| **FunctionName**          | Да         | Имя хранимой процедуры (с указанием пространства имен), с которой сопоставляется функция вставки. Хранимая процедура должна объявляться в модели хранения. |
| **ровсаффектедпараметер** | Нет          | Имя выходного параметра, возвращающего количество обработанных строк.                                                                               |

#### <a name="example"></a>Пример

Следующий пример основан на модели School и показывает элемент **InsertFunction** , используемый для сопоставления функции вставки ассоциации **каурсеинструктор** с хранимой процедурой **инсерткаурсеинструктор** . Хранимая процедура **инсерткаурсеинструктор** объявляется в модели хранения.

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

Элемент **Mapping** на языке MSL содержит сведения для сопоставления объектов, определенных в концептуальной модели, с базой данных (как описано в модели хранения). Дополнительные сведения см. в статье спецификации языка CSDL и спецификации SSDL.

Элемент **Mapping** является корневым элементом для спецификации сопоставления. Пространство имен XML для спецификаций сопоставления — https://schemas.microsoft.com/ado/2009/11/mapping/cs.

У элемента Mapping могут быть следующие дочерние элементы (в порядке перечисления):

-   Псевдоним (ноль или более)
-   EntityContainerMapping (ровно один)

Имена типов концептуальной модели и модели хранения, которые упоминаются в языке MSL, должны указываться вместе с именами соответствующих пространств имен. Дополнительные сведения об имени пространства имен концептуальной модели см. в разделе элемент Schema (CSDL). Дополнительные сведения об имени пространства имен модели хранения см. в разделе элемент Schema (SSDL). Псевдонимы для пространств имен, используемых в языке MSL, могут быть заданы с использованием элемента Alias.

### <a name="applicable-attributes"></a>Применимые атрибуты

В следующей таблице описаны атрибуты, которые можно применить к элементу **Mapping** .

| Имя атрибута | Необходимо | Значение                                                 |
|:---------------|:------------|:------------------------------------------------------|
| **ПРОБЕЛ**      | Да         | **C-S**. Это фиксированное значение, и его невозможно изменить. |

### <a name="example"></a>Пример

В следующем примере показан элемент **Mapping** , основанный на части модели School. Дополнительные сведения о модели School см. в разделе Краткое руководство (Entity Framework):

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

## <a name="mappingfragment-element-msl"></a>Элемент MappingFragment (MSL)

Элемент **MappingFragment** на языке MSL определяет сопоставление свойств типа сущности концептуальной модели и таблицы или представления в базе данных. Сведения о типах сущностей концептуальной модели и базовых таблицах или представлениях базы данных см. в разделах Элемент EntityType (язык CSDL) и Элемент EntitySet (язык SSDL). **MappingFragment** может быть дочерним элементом элемента EntityTypeMapping или элемента EntitySetMapping.

Элемент **MappingFragment** может иметь следующие дочерние элементы:

-   ComplexType (ноль или более)
-   ScalarProperty (ноль или более)
-   Условие (ноль или более)

### <a name="applicable-attributes"></a>Применимые атрибуты

В следующей таблице описаны атрибуты, которые можно применить к элементу **MappingFragment** .

| Имя атрибута          | Необходимо | Значение                                                                                                                                                                                                                         |
|:------------------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **сторинтитисет**      | Да         | Имя столбца таблицы или представления, с которым производится сопоставление.                                                                                                                                                                           |
| **Атрибутом makecolumnsdistinct** | Нет          | **Значение true** или **false** в зависимости от того, возвращаются ли только отдельные строки. <br/> Если для этого атрибута задано значение **true**, атрибуту **женератеупдатевиевс** элемента EntityContainerMapping должно быть присвоено значение **false**. |

### <a name="example"></a>Пример

В следующем примере показан элемент **MappingFragment** в качестве дочернего элемента элемента **EntityTypeMapping** . В этом примере свойства типа **курса** в концептуальной модели сопоставляются со столбцами таблицы **Course** в базе данных.

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

В следующем примере показан элемент **MappingFragment** в качестве дочернего элемента элемента **EntitySetMapping** . Как и в приведенном выше примере, свойства типа **курса** в концептуальной модели сопоставляются со столбцами таблицы **Course** в базе данных.

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

Элемент **ModificationFunctionMapping** на языке MSL сопоставляет функции вставки, обновления и удаления типа сущности концептуальной модели с хранимыми процедурами в базовой базе данных. Элемент **ModificationFunctionMapping** также может сопоставлять функции INSERT и DELETE для ассоциаций "многие ко многим" в концептуальной модели с хранимыми процедурами в базовой базе данных. Хранимые процедуры, с которыми сопоставляются функции изменения, должны объявляться в модели хранения. Дополнительные сведения см. в разделе элемент Function (SSDL).

> [!NOTE]
> Если не выполнить сопоставление всех трех операций вставки, обновления или удаления типа сущности с хранимыми процедурами, несопоставленные операции завершатся ошибкой, если они выполняются во время выполнения и создается исключение Упдатиксцептион.


> [!NOTE]
> Если функции преобразования для одной сущности в иерархии наследования сопоставляются с хранимыми процедурами, то функции преобразования всех типов в иерархии должны быть сопоставлены с хранимыми процедурами.

Элемент **ModificationFunctionMapping** может быть дочерним по отношению к элементу EntityTypeMapping или элементу AssociationSetMapping.

Элемент **ModificationFunctionMapping** может иметь следующие дочерние элементы:

-   DeleteFunction (ноль или один)
-   InsertFunction (ноль или один)
-   Упдатефунктион (ноль или один)

К элементу **ModificationFunctionMapping** не применяются атрибуты.

### <a name="example"></a>Пример

В следующем примере показано сопоставление набора сущностей для набора сущностей « **люди** » в модели School. В дополнение к сопоставлению столбцов для типа сущности **Person** отображаются сопоставления функций вставки, обновления и удаления типа **Person** . Функции, с которыми выполняется сопоставление, объявляются в модели хранения.

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

В следующем примере показано сопоставление набора ассоциаций для набора ассоциаций **каурсеинструктор** в модели School. Помимо сопоставления столбцов для ассоциации **каурсеинструктор** , отображаются сопоставления функций вставки и удаления ассоциации **каурсеинструктор** . Функции, с которыми выполняется сопоставление, объявляются в модели хранения.

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

Элемент **QueryView** на языке MSL определяет сопоставление только для чтения между типом сущности или ассоциацией в концептуальной модели и таблицей в базовой базе данных. Сопоставление определяется с помощью Entity SQL запроса, который вычисляется для модели хранения, а результирующий набор выражается в терминах сущности или ассоциации в концептуальной модели. Представления запросов доступны только для чтения, поэтому для обновления типов, которые определены представлениями запросов, не могут применяться стандартные команды обновления. Представления запросов доступны только для чтения, поэтому для обновления типов, которые определены представлениями запросов, не могут применяться стандартные команды обновления. Дополнительные сведения см. в разделе инструкции. Сопоставьте функции изменения с хранимыми процедурами.

> [!NOTE]
> В элементе **QueryView** Entity SQL выражения, содержащие **GroupBy**, групповые агрегаты или свойства навигации, не поддерживаются.

 

Элемент **QueryView** может быть дочерним по отношению к элементу EntitySetMapping или элементу AssociationSetMapping. В первом случае представление запроса определяет сопоставление только для чтения для сущности в концептуальной модели. Во втором случае представление запроса определяет сопоставление только для чтения для связи в концептуальной модели.

> [!NOTE]
> Если элемент **AssociationSetMapping** предназначен для связи с ссылочным ограничением, элемент **AssociationSetMapping** игнорируется. Дополнительные сведения см. в разделе элемент ReferentialConstraint (CSDL).

Элемент **QueryView** не может иметь дочерних элементов.

### <a name="applicable-attributes"></a>Применимые атрибуты

В следующей таблице описаны атрибуты, которые можно применить к элементу **QueryView** .

| Имя атрибута | Необходимо | Значение                                                                         |
|:---------------|:------------|:------------------------------------------------------------------------------|
| **Имени**   | Нет          | Имя типа концептуальной модели, сопоставляемой представлением запроса. |

### <a name="example"></a>Пример

В следующем примере показано, как элемент **QueryView** является дочерним элементом элемента **EntitySetMapping** и определяет сопоставление представления запроса для типа сущности « **Отдел** » в модели School.

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

Поскольку запрос возвращает только подмножество элементов типа **отдела** в модели хранения, тип **отдела** в модели School был изменен на основе этого сопоставления следующим образом:

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

В следующем примере показан элемент **QueryView** как дочерний элемент элемента **AssociationSetMapping** и определяется сопоставление только для чтения для `FK_Course_Department` ассоциации в модели School.

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
 
### <a name="comments"></a>Comments

Представления запросов можно определять для реализации следующих сценариев.

-   Определение сущности в концептуальной модели, не включающей все свойства сущности в модели хранения. Сюда входят свойства, не имеющие значений по умолчанию и не поддерживающие значения **null** .
-   Сопоставление вычисляемых столбцов в модели хранения со свойствами типов сущностей в концептуальной модели.
-   Определение сопоставления, в котором условия, используемые для секционирования сущностей в концептуальной модели, не базируются на равенстве. При указании условного сопоставления с помощью элемента **Condition** указанное условие должно быть равно указанному значению. Дополнительные сведения см. в разделе элемент Condition (MSL).
-   Сопоставление одного столбца в модели хранения с несколькими типами в концептуальной модели.
-   Сопоставление нескольких типов с одной и той же таблицей.
-   Определение ассоциаций в концептуальной модели, базирующихся на внешних ключах реляционной схемы.
-   Применение пользовательской бизнес-логики для задания значений свойств в концептуальной модели. Например, строковое значение «T» в источнике данных можно преобразовать в значение « **true**» (логическое) в концептуальной модели.
-   Определение условных фильтров для результатов запроса.
-   Наложение меньшего числа ограничений на данные в концептуальной модели, нежели в модели хранения. Например, можно сделать свойство в концептуальной модели обнуляемым, даже если столбец, с которым он сопоставлен, не поддерживает значения **null**.

При определении представлений запросов для сущностей необходимо принимать во внимание следующие соображения.

-   Представления запросов доступны только для чтения. Обновление сущностей можно осуществлять только с помощью функций изменения.
-   В случае определения типа сущности через представление запроса необходимо также определять все связанные сущности через представления запросов.
-   При сопоставлении связи "многие ко многим" с сущностью в модели хранения, которая представляет связанную таблицу в реляционной схеме, необходимо определить элемент **QueryView** в элементе **AssociationSetMapping** для этой таблицы ссылок.
-   Представления запросов должны быть определены для всех типов в иерархии типов. Это можно сделать следующими способами:
-   -   С одним элементом **QueryView** , указывающим один Entity SQL запрос, возвращающий объединение всех типов сущностей в иерархии.
    -   С одним элементом **QueryView** , указывающим один Entity SQL запрос, использующий оператор Case для возврата определенного типа сущности в иерархии на основе определенного условия.
    -   С дополнительным элементом **QueryView** для определенного типа в иерархии. В этом случае используйте атрибут **TypeName** элемента **QueryView** , чтобы указать тип сущности для каждого представления.
-   Если задано представление запроса, нельзя указать атрибут **сторажесетнаме** в элементе **EntitySetMapping** .
-   Если определено представление запроса, элемент **EntitySetMapping**также не может содержать сопоставления **свойств** .

## <a name="resultbinding-element-msl"></a>Элемент ResultBinding (язык MSL)

Элемент **ResultBinding** в языке MSL сопоставляет значения столбцов, возвращаемые хранимыми процедурами, со свойствами сущностей в концептуальной модели, если функции изменения типов сущностей сопоставлены с хранимыми процедурами в базовой базе данных. Например, если значение столбца идентификаторов возвращается хранимой процедурой INSERT, элемент **ResultBinding** сопоставляет возвращенное значение со свойством типа сущности в концептуальной модели.

Элемент **ResultBinding** может быть дочерним по отношению к элементу InsertFunction или элементу упдатефунктион.

Элемент **ResultBinding** не может иметь дочерних элементов.

### <a name="applicable-attributes"></a>Применимые атрибуты

В следующей таблице описаны атрибуты, применимые к элементу **ResultBinding** .

| Имя атрибута | Необходимо | Значение                                                                         |
|:---------------|:------------|:------------------------------------------------------------------------------|
| **Name**       | Да         | Имя свойства сущности в концептуальной модели, с которым выполняется сопоставление. |
| **имя_столбца** | Да         | Имя столбца, с которым выполнятся сопоставление.                                          |

### <a name="example"></a>Пример

Следующий пример основан на модели School и показывает элемент **InsertFunction** , используемый для преобразования функции вставки типа сущности **Person** в хранимую процедуру **инсертперсон** . (Хранимая процедура **инсертперсон** показана ниже и объявлена в модели хранения.) Элемент **ResultBinding** используется для отображения значения столбца, возвращаемого хранимой процедурой (**невперсонид**), в свойство типа сущности (**PersonID**).

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

Следующая инструкция Transact-SQL описывает хранимую процедуру **инсертперсон** :

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

Элемент **ресултмаппинг** на языке MSL определяет сопоставление между импортом функции в концептуальной модели и хранимой процедурой в базовой базе данных, если выполняются следующие условия.

-   Импорт функции возвращает тип сущности концептуальной модели или сложный тип.
-   Имена столбцов, возвращаемые хранимой процедурой, не совпадают в точности с именами свойств в сложном типе или типе сущности.

По умолчанию сопоставление между столбцами, возвращаемыми хранимой процедурой, и сложным типом или типом сущности выполняется по именам столбцов и свойств. Если имена столбцов не точно соответствуют именам свойств, для определения сопоставления необходимо использовать элемент **ресултмаппинг** . Пример сопоставления по умолчанию см. в разделе элемент FunctionImportMapping (MSL).

Элемент **ресултмаппинг** является дочерним элементом элемента FunctionImportMapping.

Элемент **ресултмаппинг** может иметь следующие дочерние элементы:

-   EntityTypeMapping (ноль и более)
-   ComplexTypeMapping

К элементу **ресултмаппинг** не применяются атрибуты.

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

Чтобы создать импорт функции, возвращающий экземпляры предыдущего типа сущности, сопоставление столбцов, возвращаемых хранимой процедурой и типом сущности, должно быть определено в элементе **ресултмаппинг** :

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

Элемент **ScalarProperty** на языке MSL сопоставляет свойство типа сущности концептуальной модели, сложного типа или ассоциации со столбцом таблицы или параметром хранимой процедуры в базовой базе данных.

> [!NOTE]
> Хранимые процедуры, с которыми сопоставляются функции изменения, должны объявляться в модели хранения. Дополнительные сведения см. в разделе элемент Function (SSDL).

Элемент **ScalarProperty** может быть дочерним для следующих элементов:

-   MappingFragment
-   InsertFunction
-   UpdateFunction
-   DeleteFunction
-   EndProperty
-   ComplexProperty
-   ResultMapping

Элемент **ScalarProperty** , являющийся дочерним элементом элемента **MappingFragment**, **комплекспроперти**или **EndProperty** , сопоставляет свойство концептуальной модели со столбцом в базе данных. Как дочерний элемент элемента **InsertFunction**, **упдатефунктион**или **DeleteFunction** , элемент **ScalarProperty** сопоставляет свойство концептуальной модели с параметром хранимой процедуры.

Элемент **ScalarProperty** не может иметь дочерних элементов.

### <a name="applicable-attributes"></a>Применимые атрибуты

Атрибуты, применяемые к элементу **ScalarProperty** , различаются в зависимости от роли элемента.

В следующей таблице описаны атрибуты, применимые, если элемент **ScalarProperty** используется для отображения свойства концептуальной модели в столбце базы данных:

| Имя атрибута | Необходимо | Значение                                                           |
|:---------------|:------------|:----------------------------------------------------------------|
| **Name**       | Да         | Имя свойства концептуальной модели, с которым выполняется сопоставление. |
| **имя_столбца** | Да         | Имя столбца таблицы, с которым производится сопоставление.              |

В следующей таблице описаны атрибуты, применимые к элементу **ScalarProperty** , когда он используется для отображения свойства концептуальной модели в параметре хранимой процедуры.

| Имя атрибута    | Необходимо | Значение                                                                                                                                           |
|:------------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------|
| **Name**          | Да         | Имя свойства концептуальной модели, с которым выполняется сопоставление.                                                                                 |
| **ParameterName** | Да         | Имя сопоставляемого параметра.                                                                                                 |
| **Версия**       | Нет          | **Current** или **Original** в зависимости от того, следует ли использовать текущее значение или исходное значение свойства для проверок параллелизма. |

### <a name="example"></a>Пример

В следующем примере показан элемент **ScalarProperty** , используемый двумя способами:

-   Для отображения свойств типа сущности **Person** в столбцах таблицы **Person**.
-   Чтобы связать свойства типа сущности **Person** с параметрами хранимой процедуры **упдатеперсон** . Хранимые процедуры объявляются в модели хранения.

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

В следующем примере показан элемент **ScalarProperty** , используемый для сопоставления функций вставки и удаления ассоциации концептуальной модели с хранимыми процедурами в базе данных. Хранимые процедуры объявляются в модели хранения.

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

Элемент **упдатефунктион** на языке MSL сопоставляет функцию Update типа сущности в концептуальной модели с хранимой процедурой в базовой базе данных. Хранимые процедуры, с которыми сопоставляются функции изменения, должны объявляться в модели хранения. Дополнительные сведения см. в разделе элемент Function (SSDL).

> [!NOTE]
>  Если не выполнить сопоставление всех трех операций вставки, обновления или удаления типа сущности с хранимыми процедурами, несопоставленные операции завершатся ошибкой, если они выполняются во время выполнения и создается исключение Упдатиксцептион.

Элемент **упдатефунктион** может быть дочерним элементом элемента ModificationFunctionMapping и применен к элементу EntityTypeMapping.

Элемент **упдатефунктион** может иметь следующие дочерние элементы:

-   AssociationEnd (ноль или более)
-   ComplexProperty (ноль и более)
-   ResultBinding (ноль или один)
-   Скарларпроперти (ноль или более)

### <a name="applicable-attributes"></a>Применимые атрибуты

В следующей таблице описаны атрибуты, которые можно применить к элементу **упдатефунктион** .

| Имя атрибута            | Необходимо | Значение                                                                                                                                                    |
|:--------------------------|:------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------|
| **FunctionName**          | Да         | Имя хранимой процедуры (с указанием пространства имен), с которой сопоставляется функция обновления. Хранимая процедура должна объявляться в модели хранения. |
| **ровсаффектедпараметер** | Нет          | Имя выходного параметра, возвращающего количество обработанных строк.                                                                               |

### <a name="example"></a>Пример

Следующий пример основан на модели School и показывает элемент **упдатефунктион** , используемый для преобразования функции обновления типа сущности **Person** в хранимую процедуру **упдатеперсон** . Хранимая процедура **упдатеперсон** объявляется в модели хранения.

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
