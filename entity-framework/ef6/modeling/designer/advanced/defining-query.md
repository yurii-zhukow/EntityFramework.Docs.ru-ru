---
title: Определение запроса - конструктор EF - EF6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: e52a297e-85aa-42f6-a922-ba960f8a4b22
caps.latest.revision: 3
ms.openlocfilehash: 593fb9925a7a0b59a69b8c8dc4846640627756aa
ms.sourcegitcommit: f05e7b62584cf228f17390bb086a61d505712e1b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/08/2018
ms.locfileid: "39122281"
---
# <a name="defining-query---ef-designer"></a>Определяющий запрос - конструктор EF
В этом пошаговом руководстве показан способ добавления определяющего запроса и соответствующей сущности, тип модели, в конструкторе EF. Определяющий запрос обычно используется для предоставления функций, которые аналогичны предоставляемым представления базы данных, но оно определено в модели, а не в базе данных. Определяющий запрос позволяет выполнить инструкцию SQL, который указан в **DefiningQuery** элемент EDMX-файла. Дополнительные сведения см. в разделе **DefiningQuery** в [спецификация SSDL](~/ef6/modeling/designer/advanced/edmx/ssdl-spec.md).

При использовании определение запросов, необходимо также определить тип сущности в модели. Тип сущности используется для отображения данных с помощью определения запроса. Обратите внимание, что данные, отображаемые через этот тип сущности только для чтения.

Параметризованные запросы не могут выполняться как определяющие запросы. Однако их обновление возможно через сопоставление функций вставки, обновления и удаления типа сущности, который отображает данные в хранимых процедурах. Дополнительные сведения см. в разделе [вставки, обновления и удаления с помощью хранимых процедур](~/ef6/modeling/designer/stored-procedures/cud.md).

В этом разделе показано, как выполнять следующие задачи.

-   Добавление определяющего запроса
-   Добавление типа сущности в модель
-   Тип сущности сопоставляются с определяющего запроса

## <a name="prerequisites"></a>Предварительные требования

Для выполнения данного пошагового руководства требуется:

- Последнюю версию Visual Studio.
- [Образца базы данных School](~/ef6/resources/school-database.md).

## <a name="set-up-the-project"></a>Настройка проекта

В этом пошаговом руководстве используется Visual Studio 2012 или более поздней версии.

-   Запустите Visual Studio.
-   В меню **Файл** выберите пункт **Создать**, а затем команду **Проект**.
-   В левой области щелкните **Visual C\#**, а затем выберите **консольное приложение** шаблона.
-   Введите **DefiningQuerySample** как имя проекта и нажмите кнопку **ОК**.

 

## <a name="create-a-model-based-on-the-school-database"></a>Создать модель, основанную на базе данных School

-   Щелкните правой кнопкой мыши имя проекта в обозревателе решений, выберите пункт **добавить**, а затем нажмите кнопку **новый элемент**.
-   Выберите **данных** меню слева, а затем выберите **ADO.NET Entity Data Model** в области шаблонов.
-   Введите **DefiningQueryModel.edmx** имя файла, а затем нажмите кнопку **добавить**.
-   В диалоговом окне Выбор содержимого модели выберите **создать из базы данных**, а затем нажмите кнопку **Далее**.
-   Щелкните новое подключение. В диалоговом окне Свойства соединения, введите имя сервера (например, **(localdb)\\mssqllocaldb**) выберите метод проверки подлинности, тип **School** для имени базы данных, а затем Нажмите кнопку **ОК**.
    В диалоговом окне Выбор подключения к базе данных обновляется параметр подключения базы данных.
-   В диалоговом окне Выбор объектов базы данных проверьте **таблиц** узла. Это добавит все таблицы, которые **School** модели.
-   Нажмите кнопку **Готово**.
-   В обозревателе решений щелкните правой кнопкой мыши **DefiningQueryModel.edmx** файл и выберите **открыть с помощью...** .
-   Выберите **редактор (текстовый) XML**.

    ![XMLEditor](~/ef6/media/xmleditor.png)

-   Нажмите кнопку **Да** при появлении соответствующего запроса со следующим сообщением:

    ![Warning2](~/ef6/media/warning2.png)

 

## <a name="add-a-defining-query"></a>Добавление определяющего запроса

На этом шаге мы воспользуемся редактора XML для добавления определяющего запроса и тип сущности в разделе SSDL EDMX-файла. 

-   Добавить **EntitySet** элемент в раздел SSDL EDMX-файла (строка 5 по 13). Укажите следующие сведения:
    -   Только **имя** и **EntityType** атрибуты **EntitySet** указанных элемента.
    -   Полное имя типа сущности используется в **EntityType** атрибута.
    -   Инструкция SQL для выполнения указана в **DefiningQuery** элемент.

``` xml
    <!-- SSDL content -->
    <edmx:StorageModels>
      <Schema Namespace="SchoolModel.Store" Alias="Self" Provider="System.Data.SqlClient" ProviderManifestToken="2008" xmlns:store="http://schemas.microsoft.com/ado/2007/12/edm/EntityStoreSchemaGenerator" xmlns="http://schemas.microsoft.com/ado/2009/11/edm/ssdl">
        <EntityContainer Name="SchoolModelStoreContainer">
           <EntitySet Name="GradeReport" EntityType="SchoolModel.Store.GradeReport">
              <DefiningQuery>
                SELECT CourseID, Grade, FirstName, LastName
                FROM StudentGrade
                JOIN
                (SELECT * FROM Person WHERE EnrollmentDate IS NOT NULL) AS p
                ON StudentID = p.PersonID
              </DefiningQuery>
          </EntitySet>
          <EntitySet Name="Course" EntityType="SchoolModel.Store.Course" store:Type="Tables" Schema="dbo" />
```

-   Добавить **EntityType** элемент в раздел SSDL EDMX-файла. файл, как показано ниже. Обратите внимание на следующее условия:
    -   Значение **имя** атрибут соответствует значению **EntityType** атрибут в **EntitySet** элемент выше, несмотря на то что полное доменное имя Тип сущности используется в **EntityType** атрибута.
    -   Имена свойств соответствуют именам столбцов, возвращаемых инструкцией SQL в **DefiningQuery** элемент (см. выше).
    -   В данном примере ключ сущности состоит из трех свойств, что обеспечивает уникальность значения ключа.

``` xml
    <EntityType Name="GradeReport">
      <Key>
        <PropertyRef Name="CourseID" />
        <PropertyRef Name="FirstName" />
        <PropertyRef Name="LastName" />
      </Key>
      <Property Name="CourseID"
                Type="int"
                Nullable="false" />
      <Property Name="Grade"
                Type="decimal"
                Precision="3"
                Scale="2" />
      <Property Name="FirstName"
                Type="nvarchar"
                Nullable="false"
                MaxLength="50" />
      <Property Name="LastName"
                Type="nvarchar"
                Nullable="false"
                MaxLength="50" />
    </EntityType>
```

>[!NOTE]
> Если вы запустите **мастер обновления моделей** диалоговое окно, любые изменения, внесенные в модель хранения, включая определение запросов, будут перезаписаны.

 

## <a name="add-an-entity-type-to-the-model"></a>Добавление типа сущности в модель

На этом этапе мы добавим тип сущности в концептуальную модель, в конструкторе EF.  Обратите внимание на следующее условия:

-   **Имя** сущности соответствует значению **EntityType** атрибут в **EntitySet** элемент выше.
-   Имена свойств соответствуют именам столбцов, возвращаемых инструкцией SQL в **DefiningQuery** элемент выше.
-   В данном примере ключ сущности состоит из трех свойств, что обеспечивает уникальность значения ключа.

Откройте модель в конструкторе EF.

-   Дважды щелкните DefiningQueryModel.edmx.
-   Скажем **Да** следующее сообщение:

    ![Warning2](~/ef6/media/warning2.png)

 

Конструктор сущностей, который предоставляет область конструктора для изменения модели, отображается.

-   Щелкните правой кнопкой мыши в конструкторе и выберите пункт **Add New**-&gt;**сущности...** .
-   Укажите **GradeReport** имя сущности и **CourseID** для **свойство Key**.
-   Щелкните правой кнопкой мыши **GradeReport** сущности и выберите **Add New** - &gt; **скалярное свойство**.
-   Изменить имя по умолчанию свойства **FirstName**.
-   Добавить еще одно скалярное свойство и указать **LastName** для имени.
-   Добавить еще одно скалярное свойство и указать **корпоративного класса** для имени.
-   В **свойства** измените **корпоративного класса** **тип** свойства **десятичное**.
-   Выберите **FirstName** и **LastName** свойства.
-   В **свойства** измените **EntityKey** значение свойства **True**.

Таким образом, были добавлены следующие элементы **CSDL** раздел EDMX-файла.

``` xml
    <EntitySet Name="GradeReport" EntityType="SchoolModel.GradeReport" />

    <EntityType Name="GradeReport">
    . . .
    </EntityType>
```

 

## <a name="map-the-defining-query-to-the-entity-type"></a>Тип сущности сопоставляются с определяющего запроса

На этом шаге мы будем использовать окно Mapping Details для сопоставления концептуальной и типа сущности хранилища.

-   Щелкните правой кнопкой мыши **GradeReport** сущности в область конструктора и выберите **сопоставление таблицы**.  
    **Сведения о сопоставлении** откроется диалоговое окно.
-   Выберите **GradeReport** из **&lt;добавить таблицу или представление&gt;** раскрывающегося списка (расположенный в **таблицы**s).  
    По умолчанию сопоставления между концептуальной и хранения **GradeReport** отображаются тип сущности.  
    ![MappingDetails3](~/ef6/media/mappingdetails.png)

В результате **EntitySetMapping** элемент добавляется в раздел сопоставления EDMX-файла. 

``` xml
    <EntitySetMapping Name="GradeReports">
      <EntityTypeMapping TypeName="IsTypeOf(SchoolModel.GradeReport)">
        <MappingFragment StoreEntitySet="GradeReport">
          <ScalarProperty Name="LastName" ColumnName="LastName" />
          <ScalarProperty Name="FirstName" ColumnName="FirstName" />
          <ScalarProperty Name="Grade" ColumnName="Grade" />
          <ScalarProperty Name="CourseID" ColumnName="CourseID" />
        </MappingFragment>
      </EntityTypeMapping>
    </EntitySetMapping>
```

-   Скомпилируйте приложение.

 

## <a name="call-the-defining-query-in-your-code"></a>Вызывать определяющего запроса в коде

Теперь, определяющий запрос можно выполнить с помощью **GradeReport** типа сущности. 

``` csharp
    using (var context = new SchoolEntities())
    {
        var report = context.GradeReports.FirstOrDefault();
        Console.WriteLine("{0} {1} got {2}",
            report.FirstName, report.LastName, report.Grade);
    }
```
