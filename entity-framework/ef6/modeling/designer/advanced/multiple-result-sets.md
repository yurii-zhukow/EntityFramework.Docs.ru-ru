---
title: Хранимые процедуры с несколькими результирующими наборами — EF6
description: Хранимые процедуры с несколькими результирующими наборами в Entity Framework 6
author: ajcvickers
ms.date: 10/23/2016
uid: ef6/modeling/designer/advanced/multiple-result-sets
ms.openlocfilehash: 8c80e2b8c861a763b7f63ea2523194028498dace
ms.sourcegitcommit: 0a25c03fa65ae6e0e0e3f66bac48d59eceb96a5a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/14/2020
ms.locfileid: "92066282"
---
# <a name="stored-procedures-with-multiple-result-sets"></a>Хранимые процедуры с несколькими результирующими наборами
Иногда при использовании хранимых процедур потребуется возвратить более одного результирующего набора. Этот сценарий обычно используется для уменьшения количества обращений к базе данных, необходимых для создания одного экрана.До EF5 Entity Framework позволяла вызывать хранимую процедуру, но возвращал бы только первый результирующий набор в вызывающий код.

В этой статье показано два способа, которые можно использовать для доступа к нескольким результирующим наборам из хранимой процедуры в Entity Framework. Одна из них использует только код и работает с кодом First и EF Designer, а другой — только с конструктором EF. Средства и поддержка API для этого должны улучшаться в будущих версиях Entity Framework.

## <a name="model"></a>Модель

В примерах, приведенных в этой статье, используется базовая модель блога и публикации, в которой в блоге имеется много записей, а запись принадлежит одному блогу. Мы будем использовать хранимую процедуру в базе данных, которая возвращает все блоги и записи, примерно так:

``` SQL
    CREATE PROCEDURE [dbo].[GetAllBlogsAndPosts]
    AS
        SELECT * FROM dbo.Blogs
        SELECT * FROM dbo.Posts
```

## <a name="accessing-multiple-result-sets-with-code"></a>Доступ к нескольким результирующим наборам с помощью кода

Мы можем выполнить код с помощью кода, чтобы выполнить необработанную команду SQL для выполнения хранимой процедуры. Преимуществом этого подхода является то, что он работает с кодом First и конструктором EF.

Чтобы получить несколько результирующих наборов, необходимо перейти к API ObjectContext с помощью интерфейса Иобжектконтекстадаптер.

После того, как у нас есть ObjectContext, мы можем использовать метод сдвига для преобразования результатов хранимой процедуры в сущности, которые можно относить и использовать в EF как обычные. В следующем примере кода демонстрируется это в действии.

``` csharp
    using (var db = new BloggingContext())
    {
        // If using Code First we need to make sure the model is built before we open the connection
        // This isn't required for models created with the EF Designer
        db.Database.Initialize(force: false);

        // Create a SQL command to execute the sproc
        var cmd = db.Database.Connection.CreateCommand();
        cmd.CommandText = "[dbo].[GetAllBlogsAndPosts]";

        try
        {

            db.Database.Connection.Open();
            // Run the sproc
            var reader = cmd.ExecuteReader();

            // Read Blogs from the first result set
            var blogs = ((IObjectContextAdapter)db)
                .ObjectContext
                .Translate<Blog>(reader, "Blogs", MergeOption.AppendOnly);   


            foreach (var item in blogs)
            {
                Console.WriteLine(item.Name);
            }        

            // Move to second result set and read Posts
            reader.NextResult();
            var posts = ((IObjectContextAdapter)db)
                .ObjectContext
                .Translate<Post>(reader, "Posts", MergeOption.AppendOnly);


            foreach (var item in posts)
            {
                Console.WriteLine(item.Title);
            }
        }
        finally
        {
            db.Database.Connection.Close();
        }
    }
```

Метод транслируют принимает считывание, полученное при выполнении процедуры, имени EntitySet и собой MergeOption. Имя EntitySet будет таким же, как и свойство DbSet в производном контексте. Перечисление собой MergeOption управляет тем, как результаты обрабатываются, если такая же сущность уже существует в памяти.

Здесь мы перебираем коллекцию блогов перед вызовом Некстресулт. это важно для кода, приведенного выше, так как перед переходом к следующему результирующему набору необходимо использовать первый результирующий набор.

После вызова двух методов перевода запись блога и сущности отправляются EF так же, как и любая другая сущность, и поэтому их можно изменить или удалить и сохранить как нормальную.

>[!NOTE]
> EF не принимает никаких сопоставлений в учетную запись при создании сущностей с помощью метода сдвига. Он будет просто сопоставлять имена столбцов в результирующем наборе с именами свойств в классах.

>[!NOTE]
> Если включена отложенная загрузка, то при доступе к свойству posts одной из сущностей блога EF будет подключаться к базе данных для неактивной загрузки всех записей, даже если все они уже загружены. Это обусловлено тем, что EF не может определить, загружены ли все записи или в базе данных больше. Если вы хотите избежать этого, необходимо будет отключить отложенную загрузку.

## <a name="multiple-result-sets-with-configured-in-edmx"></a>Несколько результирующих наборов с настроенным в EDMX

>[!NOTE]
> Для настройки нескольких результирующих наборов в EDMX необходимо выбрать .NET Framework 4,5. При разработке для .NET 4,0 можно использовать метод на основе кода, показанный в предыдущем разделе.

При использовании конструктора EF можно также изменить модель так, чтобы она знала о различных результирующих наборах, которые будут возвращены. Прежде всего необходимо знать, что инструментарий не учитывает несколько результирующих наборов, поэтому необходимо вручную изменить EDMX-файл. Редактирование файла EDMX будет работать, но также приведет к нарушению проверки модели в VS. Поэтому при проверке модели всегда будут выводится ошибки.

-   Для этого необходимо добавить хранимую процедуру в модель, как в случае с одним запросом результирующего набора.
-   После этого необходимо щелкнуть модель правой кнопкой мыши и выбрать пункт **Открыть с помощью.** затем **XML**

    ![Открыть как](~/ef6/media/openas.png)

После открытия модели в формате XML необходимо выполнить следующие действия.

-   Найдите сложный тип и импорт функции в модели:

``` xml
    <!-- CSDL content -->
    <edmx:ConceptualModels>

    ...

      <FunctionImport Name="GetAllBlogsAndPosts" ReturnType="Collection(BlogModel.GetAllBlogsAndPosts_Result)" />

    ...

      <ComplexType Name="GetAllBlogsAndPosts_Result">
        <Property Type="Int32" Name="BlogId" Nullable="false" />
        <Property Type="String" Name="Name" Nullable="false" MaxLength="255" />
        <Property Type="String" Name="Description" Nullable="true" />
      </ComplexType>

    ...

    </edmx:ConceptualModels>
```

 

-   Удаление сложного типа
-   Обновите функцию импорта, чтобы она сопоставлялась с сущностями. в нашем случае она будет выглядеть следующим образом:

``` xml
    <FunctionImport Name="GetAllBlogsAndPosts">
      <ReturnType EntitySet="Blogs" Type="Collection(BlogModel.Blog)" />
      <ReturnType EntitySet="Posts" Type="Collection(BlogModel.Post)" />
    </FunctionImport>
```

Это указывает модели, что хранимая процедура будет возвращать две коллекции: одну из записей блога и одну из записей POST.

-   Найдите элемент сопоставления функции:

``` xml
    <!-- C-S mapping content -->
    <edmx:Mappings>

    ...

      <FunctionImportMapping FunctionImportName="GetAllBlogsAndPosts" FunctionName="BlogModel.Store.GetAllBlogsAndPosts">
        <ResultMapping>
          <ComplexTypeMapping TypeName="BlogModel.GetAllBlogsAndPosts_Result">
            <ScalarProperty Name="BlogId" ColumnName="BlogId" />
            <ScalarProperty Name="Name" ColumnName="Name" />
            <ScalarProperty Name="Description" ColumnName="Description" />
          </ComplexTypeMapping>
        </ResultMapping>
      </FunctionImportMapping>

    ...

    </edmx:Mappings>
```

-   Замените результат сопоставления на один для каждой возвращаемой сущности, как показано ниже:

``` xml
    <ResultMapping>
      <EntityTypeMapping TypeName ="BlogModel.Blog">
        <ScalarProperty Name="BlogId" ColumnName="BlogId" />
        <ScalarProperty Name="Name" ColumnName="Name" />
        <ScalarProperty Name="Description" ColumnName="Description" />
      </EntityTypeMapping>
    </ResultMapping>
    <ResultMapping>
      <EntityTypeMapping TypeName="BlogModel.Post">
        <ScalarProperty Name="BlogId" ColumnName="BlogId" />
        <ScalarProperty Name="PostId" ColumnName="PostId"/>
        <ScalarProperty Name="Title" ColumnName="Title" />
        <ScalarProperty Name="Text" ColumnName="Text" />
      </EntityTypeMapping>
    </ResultMapping>
```

Кроме того, можно сопоставлять результирующие наборы со сложными типами, например созданными по умолчанию. Для этого нужно создать новый сложный тип, а не удалить его и использовать сложные типы везде, где использовались имена сущностей в приведенных выше примерах.

После изменения этих сопоставлений можно сохранить модель и выполнить следующий код для использования хранимой процедуры:

``` csharp
    using (var db = new BlogEntities())
    {
        var results = db.GetAllBlogsAndPosts();

        foreach (var result in results)
        {
            Console.WriteLine("Blog: " + result.Name);
        }

        var posts = results.GetNextResult<Post>();

        foreach (var result in posts)
        {
            Console.WriteLine("Post: " + result.Title);
        }

        Console.ReadLine();
    }
```

>[!NOTE]
> Если вы вручную измените EDMX-файл для своей модели, он будет перезаписан при повторном создании модели из базы данных.

## <a name="summary"></a>Итоги

Здесь мы показали два разных метода доступа к нескольким результирующим наборам с помощью Entity Framework. Оба они являются одинаково допустимыми в зависимости от вашей ситуации и предпочтений, поэтому следует выбрать тот, который лучше всего подходит для ваших обстоятельств. Планируется, что поддержка нескольких результирующих наборов будет улучшена в будущих версиях Entity Framework и выполнение действий, описанных в этом документе, больше не потребуется.
