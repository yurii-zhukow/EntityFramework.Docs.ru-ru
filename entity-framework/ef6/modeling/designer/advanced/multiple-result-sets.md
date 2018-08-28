---
title: Хранимые процедуры с несколькими результирующими наборами - EF6
author: divega
ms.date: 2016-10-23
ms.assetid: 1b3797f9-cd3d-4752-a55e-47b84b399dc1
ms.openlocfilehash: bb104ac5f584d26d279259a173de9afe3f018968
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/27/2018
ms.locfileid: "42996179"
---
# <a name="stored-procedures-with-multiple-result-sets"></a>Хранимые процедуры с несколькими результирующими наборами
Иногда при использовании хранимых процедур, которые требуется вернуть несколько результатов значение. Этот сценарий обычно используется для сокращения объема базы данных циклами, необходимые для составления на одном экране. До EF5 Entity Framework позволяет вызывать хранимую процедуру, но возвратит только первый результирующий набор в вызывающий код.

В этой статье мы покажем два способа, которые можно использовать для доступа к более чем одного результирующего набора из хранимой процедуры в Entity Framework. Используется только код и работает с и код сначала и конструкторе EF и тот, который работает только с помощью конструктора EF. Средства и API поддерживает это должно улучшить в будущих версиях Entity Framework.

## <a name="model"></a>Модель

В примерах в этой статье используется базовый блог и сообщения модель, где блог имеет записях и блога принадлежит к одной записи. Мы будем использовать хранимую процедуру в базе данных, который возвращает все блогов и записей, примерно так:

``` SQL
    CREATE PROCEDURE [dbo].[GetAllBlogsAndPosts]
    AS
        SELECT * FROM dbo.Blogs
        SELECT * FROM dbo.Posts
```

## <a name="accessing-multiple-result-sets-with-code"></a>Доступ к несколько результирующих наборов с помощью кода

Позволяет выполнить использовать код для выдачи необработанные команду SQL для выполнения хранимой процедуры. Этот подход удобен для работы с и код сначала и конструкторе EF.

Чтобы получить несколько результирующих задает рабочего, нам нужно уменьшится до API ObjectContext с помощью интерфейса IObjectContextAdapter.

Когда у нас ObjectContext мы можно использовать метод Translate преобразовать результаты наших хранимой процедуры, в сущности, которые могут отслеживаться и использоваться в EF в обычном режиме. В следующем образце кода демонстрируется это в действии.

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

Метод Translate принимает средство чтения, мы получили, когда выполнена процедура, является именем набора сущностей и MergeOption. Имя EntitySet будет таким же, как свойства DbSet в вашем производном контексте. Перечисление MergeOption управляет, как обрабатываются результаты, если та же сущность уже существует в памяти.

Здесь мы итерации по коллекции блогов, прежде чем мы называем NextResult, это очень важно, учитывая приведенный выше код так, как первый результирующий набор, которые должны использоваться перед переходом к следующему результирующему набору.

После перевода двух методы вызываются, то сущности Blog и Post отслеживаются EF так же, как любой другой сущности и поэтому можно изменить или удалить и сохранен в обычном режиме.

>[!NOTE]
> При создании сущности с помощью метода Translate EF не принимает любое сопоставление в учетной записи. Он просто будет соответствовать имена столбцов в результирующем наборе с именами свойств в классах.

>[!NOTE]
> Что если имеется отложенная загрузка включена, при обращении к свойству сообщения на одной сущности блог затем EF будет соединиться с базой данных для пассивной загрузки все сообщения, несмотря на то, что мы уже загружен их все. Это обусловлено EF не знает ли вы загрузили все сообщения, или если есть и другие базы данных. Если вы хотите избежать этого, необходимо отключить отложенную загрузку.

## <a name="multiple-result-sets-with-configured-in-edmx"></a>Несколько результирующих наборов с помощью настроенного в EDMX

>[!NOTE]
> Необходимо ориентироваться .NET Framework 4.5, чтобы иметь возможность настроить несколько результирующих наборов в EDMX. Если вы ориентируетесь на .NET 4.0, можно использовать метод на основе кода, показанный в предыдущем разделе.

Если вы используете конструктор EF, знает о различных результирующих наборов, которые будут возвращены, также можно изменить модель. Необходимо знать, прежде чем вручную в том, что инструментарий не несколько результирующих значение помнить, поэтому необходимо будет вручную изменить EDMX-файла. Изменения в EDMX-файл, как это будет работать, но он также отключению проверки модели в Visual STUDIO. Поэтому если проверка модели вы всегда получите ошибки.

-   Для этого необходимо добавить хранимую процедуру в модель, как это делается для одного результирующего набора запроса.
-   После этого необходимо щелкнуть правой кнопкой мыши модель и выберите **открыть с помощью...** затем **Xml**

    ![OpenAs](~/ef6/media/openas.png)

Создав модель открывается как XML, то необходимо выполнить следующие действия.

-   Найти сложный тип и функцию импорта в модель:

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
-   Обновления, импорта функции, таким образом, чтобы он был сопоставлен сущностей, в нашем случае он будет выглядеть следующим образом:

``` xml
    <FunctionImport Name="GetAllBlogsAndPosts">
      <ReturnType EntitySet="Blogs" Type="Collection(BlogModel.Blog)" />
      <ReturnType EntitySet="Posts" Type="Collection(BlogModel.Post)" />
    </FunctionImport>
```

Это сообщает модели, что хранимая процедура вернет двух коллекций, одну из записей в блогах и из операции post.

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

-   Замените сопоставление результат одной для каждой сущности, возвращаемых, например следующие:

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

Это также можно сопоставить с результирующими наборами сложные типы, например, созданный по умолчанию. Для этого создайте новый сложный тип, а отключите их и использовать сложные типы везде, вы использовали имена сущностей в приведенных выше примерах.

После изменились эти сопоставления можно сохранить модель и выполните следующий код, чтобы хранимая процедура:

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
> Если вы вручную вносили в EDMX-файл для модели он будет перезаписан, если вы когда-либо повторно создать модель из базы данных.

## <a name="summary"></a>Сводка

Здесь мы показали, два метода доступа к несколько результирующих наборов с помощью Entity Framework. Оба значения являются одинаково допустимыми зависимости от конкретной ситуации и предпочтения и вам следует выбрать тот, который кажется наиболее обстоятельствах. Планируется, что поддержка нескольких результирующих наборов, будет улучшена в будущем версиях Entity Framework и, выполнив действия в этом документе будет больше не нужен.
