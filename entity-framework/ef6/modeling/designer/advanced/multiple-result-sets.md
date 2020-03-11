---
title: Хранимые процедуры с несколькими результирующими наборами — EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 1b3797f9-cd3d-4752-a55e-47b84b399dc1
ms.openlocfilehash: 098ed88ba52e211965baf3660f0e51bd74c71efd
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/06/2020
ms.locfileid: "78415457"
---
# <a name="stored-procedures-with-multiple-result-sets"></a><span data-ttu-id="e148e-102">Хранимые процедуры с несколькими результирующими наборами</span><span class="sxs-lookup"><span data-stu-id="e148e-102">Stored Procedures with Multiple Result Sets</span></span>
<span data-ttu-id="e148e-103">Иногда при использовании хранимых процедур потребуется возвратить более одного результирующего набора.</span><span class="sxs-lookup"><span data-stu-id="e148e-103">Sometimes when using stored procedures you will need to return more than one result set.</span></span> <span data-ttu-id="e148e-104">Этот сценарий обычно используется для уменьшения количества обращений к базе данных, необходимых для создания одного экрана.</span><span class="sxs-lookup"><span data-stu-id="e148e-104">This scenario is commonly used to reduce the number of database round trips required to compose a single screen.</span></span><span data-ttu-id="e148e-105"> До EF5 Entity Framework позволяла вызывать хранимую процедуру, но возвращал бы только первый результирующий набор в вызывающий код.</span><span class="sxs-lookup"><span data-stu-id="e148e-105"> Prior to EF5, Entity Framework would allow the stored procedure to be called but would only return the first result set to the calling code.</span></span>

<span data-ttu-id="e148e-106">В этой статье показано два способа, которые можно использовать для доступа к нескольким результирующим наборам из хранимой процедуры в Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="e148e-106">This article will show you two ways that you can use to access more than one result set from a stored procedure in Entity Framework.</span></span> <span data-ttu-id="e148e-107">Одна из них использует только код и работает с кодом First и EF Designer, а другой — только с конструктором EF.</span><span class="sxs-lookup"><span data-stu-id="e148e-107">One that uses just code and works with both Code first and the EF Designer and one that only works with the EF Designer.</span></span> <span data-ttu-id="e148e-108">Средства и поддержка API для этого должны улучшаться в будущих версиях Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="e148e-108">The tooling and API support for this should improve in future versions of Entity Framework.</span></span>

## <a name="model"></a><span data-ttu-id="e148e-109">Модель</span><span class="sxs-lookup"><span data-stu-id="e148e-109">Model</span></span>

<span data-ttu-id="e148e-110">В примерах, приведенных в этой статье, используется базовая модель блога и публикации, в которой в блоге имеется много записей, а запись принадлежит одному блогу.</span><span class="sxs-lookup"><span data-stu-id="e148e-110">The examples in this article use a basic Blog and Posts model where a blog has many posts and a post belongs to a single blog.</span></span> <span data-ttu-id="e148e-111">Мы будем использовать хранимую процедуру в базе данных, которая возвращает все блоги и записи, примерно так:</span><span class="sxs-lookup"><span data-stu-id="e148e-111">We will use a stored procedure in the database that returns all blogs and posts, something like this:</span></span>

``` SQL
    CREATE PROCEDURE [dbo].[GetAllBlogsAndPosts]
    AS
        SELECT * FROM dbo.Blogs
        SELECT * FROM dbo.Posts
```

## <a name="accessing-multiple-result-sets-with-code"></a><span data-ttu-id="e148e-112">Доступ к нескольким результирующим наборам с помощью кода</span><span class="sxs-lookup"><span data-stu-id="e148e-112">Accessing Multiple Result Sets with Code</span></span>

<span data-ttu-id="e148e-113">Мы можем выполнить код с помощью кода, чтобы выполнить необработанную команду SQL для выполнения хранимой процедуры.</span><span class="sxs-lookup"><span data-stu-id="e148e-113">We can execute use code to issue a raw SQL command to execute our stored procedure.</span></span> <span data-ttu-id="e148e-114">Преимуществом этого подхода является то, что он работает с кодом First и конструктором EF.</span><span class="sxs-lookup"><span data-stu-id="e148e-114">The advantage of this approach is that it works with both Code first and the EF Designer.</span></span>

<span data-ttu-id="e148e-115">Чтобы получить несколько результирующих наборов, необходимо перейти к API ObjectContext с помощью интерфейса Иобжектконтекстадаптер.</span><span class="sxs-lookup"><span data-stu-id="e148e-115">In order to get multiple result sets working we need to drop to the ObjectContext API by using the IObjectContextAdapter interface.</span></span>

<span data-ttu-id="e148e-116">После того, как у нас есть ObjectContext, мы можем использовать метод сдвига для преобразования результатов хранимой процедуры в сущности, которые можно относить и использовать в EF как обычные.</span><span class="sxs-lookup"><span data-stu-id="e148e-116">Once we have an ObjectContext then we can use the Translate method to translate the results of our stored procedure into entities that can be tracked and used in EF as normal.</span></span> <span data-ttu-id="e148e-117">В следующем примере кода демонстрируется это в действии.</span><span class="sxs-lookup"><span data-stu-id="e148e-117">The following code sample demonstrates this in action.</span></span>

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

<span data-ttu-id="e148e-118">Метод транслируют принимает считывание, полученное при выполнении процедуры, имени EntitySet и собой MergeOption.</span><span class="sxs-lookup"><span data-stu-id="e148e-118">The Translate method accepts the reader that we received when we executed the procedure, an EntitySet name, and a MergeOption.</span></span> <span data-ttu-id="e148e-119">Имя EntitySet будет таким же, как и свойство DbSet в производном контексте.</span><span class="sxs-lookup"><span data-stu-id="e148e-119">The EntitySet name will be the same as the DbSet property on your derived context.</span></span> <span data-ttu-id="e148e-120">Перечисление собой MergeOption управляет тем, как результаты обрабатываются, если такая же сущность уже существует в памяти.</span><span class="sxs-lookup"><span data-stu-id="e148e-120">The MergeOption enum controls how results are handled if the same entity already exists in memory.</span></span>

<span data-ttu-id="e148e-121">Здесь мы перебираем коллекцию блогов перед вызовом Некстресулт. это важно для кода, приведенного выше, так как перед переходом к следующему результирующему набору необходимо использовать первый результирующий набор.</span><span class="sxs-lookup"><span data-stu-id="e148e-121">Here we iterate through the collection of blogs before we call NextResult, this is important given the above code because the first result set must be consumed before moving to the next result set.</span></span>

<span data-ttu-id="e148e-122">После вызова двух методов перевода запись блога и сущности отправляются EF так же, как и любая другая сущность, и поэтому их можно изменить или удалить и сохранить как нормальную.</span><span class="sxs-lookup"><span data-stu-id="e148e-122">Once the two translate methods are called then the Blog and Post entities are tracked by EF the same way as any other entity and so can be modified or deleted and saved as normal.</span></span>

>[!NOTE]
> <span data-ttu-id="e148e-123">EF не принимает никаких сопоставлений в учетную запись при создании сущностей с помощью метода сдвига.</span><span class="sxs-lookup"><span data-stu-id="e148e-123">EF does not take any mapping into account when it creates entities using the Translate method.</span></span> <span data-ttu-id="e148e-124">Он будет просто сопоставлять имена столбцов в результирующем наборе с именами свойств в классах.</span><span class="sxs-lookup"><span data-stu-id="e148e-124">It will simply match column names in the result set with property names on your classes.</span></span>

>[!NOTE]
> <span data-ttu-id="e148e-125">Если включена отложенная загрузка, то при доступе к свойству posts одной из сущностей блога EF будет подключаться к базе данных для неактивной загрузки всех записей, даже если все они уже загружены.</span><span class="sxs-lookup"><span data-stu-id="e148e-125">That if you have lazy loading enabled, accessing the posts property on one of the blog entities then EF will connect to the database to lazily load all posts, even though we have already loaded them all.</span></span> <span data-ttu-id="e148e-126">Это обусловлено тем, что EF не может определить, загружены ли все записи или в базе данных больше.</span><span class="sxs-lookup"><span data-stu-id="e148e-126">This is because EF cannot know whether or not you have loaded all posts or if there are more in the database.</span></span> <span data-ttu-id="e148e-127">Если вы хотите избежать этого, необходимо будет отключить отложенную загрузку.</span><span class="sxs-lookup"><span data-stu-id="e148e-127">If you want to avoid this then you will need to disable lazy loading.</span></span>

## <a name="multiple-result-sets-with-configured-in-edmx"></a><span data-ttu-id="e148e-128">Несколько результирующих наборов с настроенным в EDMX</span><span class="sxs-lookup"><span data-stu-id="e148e-128">Multiple Result Sets with Configured in EDMX</span></span>

>[!NOTE]
> <span data-ttu-id="e148e-129">Для настройки нескольких результирующих наборов в EDMX необходимо выбрать .NET Framework 4,5.</span><span class="sxs-lookup"><span data-stu-id="e148e-129">You must target .NET Framework 4.5 to be able to configure multiple result sets in EDMX.</span></span> <span data-ttu-id="e148e-130">При разработке для .NET 4,0 можно использовать метод на основе кода, показанный в предыдущем разделе.</span><span class="sxs-lookup"><span data-stu-id="e148e-130">If you are targeting .NET 4.0 you can use the code-based method shown in the previous section.</span></span>

<span data-ttu-id="e148e-131">При использовании конструктора EF можно также изменить модель так, чтобы она знала о различных результирующих наборах, которые будут возвращены.</span><span class="sxs-lookup"><span data-stu-id="e148e-131">If you are using the EF Designer, you can also modify your model so that it knows about the different result sets that will be returned.</span></span> <span data-ttu-id="e148e-132">Прежде всего необходимо знать, что инструментарий не учитывает несколько результирующих наборов, поэтому необходимо вручную изменить EDMX-файл.</span><span class="sxs-lookup"><span data-stu-id="e148e-132">One thing to know before hand is that the tooling is not multiple result set aware, so you will need to manually edit the edmx file.</span></span> <span data-ttu-id="e148e-133">Редактирование файла EDMX будет работать, но также приведет к нарушению проверки модели в VS.</span><span class="sxs-lookup"><span data-stu-id="e148e-133">Editing the edmx file like this will work but it will also break the validation of the model in VS.</span></span> <span data-ttu-id="e148e-134">Поэтому при проверке модели всегда будут выводится ошибки.</span><span class="sxs-lookup"><span data-stu-id="e148e-134">So if you validate your model you will always get errors.</span></span>

-   <span data-ttu-id="e148e-135">Для этого необходимо добавить хранимую процедуру в модель, как в случае с одним запросом результирующего набора.</span><span class="sxs-lookup"><span data-stu-id="e148e-135">In order to do this you need to add the stored procedure to your model as you would for a single result set query.</span></span>
-   <span data-ttu-id="e148e-136">После этого необходимо щелкнуть модель правой кнопкой мыши и выбрать пункт **Открыть с помощью.**</span><span class="sxs-lookup"><span data-stu-id="e148e-136">Once you have this then you need to right click on your model and select **Open With..**</span></span> <span data-ttu-id="e148e-137">затем **XML**</span><span class="sxs-lookup"><span data-stu-id="e148e-137">then **Xml**</span></span>

    ![Открыть как](~/ef6/media/openas.png)

<span data-ttu-id="e148e-139">После открытия модели в формате XML необходимо выполнить следующие действия.</span><span class="sxs-lookup"><span data-stu-id="e148e-139">Once you have the model opened as XML then you need to do the following steps:</span></span>

-   <span data-ttu-id="e148e-140">Найдите сложный тип и импорт функции в модели:</span><span class="sxs-lookup"><span data-stu-id="e148e-140">Find the complex type and function import in your model:</span></span>

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

 

-   <span data-ttu-id="e148e-141">Удаление сложного типа</span><span class="sxs-lookup"><span data-stu-id="e148e-141">Remove the complex type</span></span>
-   <span data-ttu-id="e148e-142">Обновите функцию импорта, чтобы она сопоставлялась с сущностями. в нашем случае она будет выглядеть следующим образом:</span><span class="sxs-lookup"><span data-stu-id="e148e-142">Update the function import so that it maps to your entities, in our case it will look like the following:</span></span>

``` xml
    <FunctionImport Name="GetAllBlogsAndPosts">
      <ReturnType EntitySet="Blogs" Type="Collection(BlogModel.Blog)" />
      <ReturnType EntitySet="Posts" Type="Collection(BlogModel.Post)" />
    </FunctionImport>
```

<span data-ttu-id="e148e-143">Это указывает модели, что хранимая процедура будет возвращать две коллекции: одну из записей блога и одну из записей POST.</span><span class="sxs-lookup"><span data-stu-id="e148e-143">This tells the model that the stored procedure will return two collections, one of blog entries and one of post entries.</span></span>

-   <span data-ttu-id="e148e-144">Найдите элемент сопоставления функции:</span><span class="sxs-lookup"><span data-stu-id="e148e-144">Find the function mapping element:</span></span>

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

-   <span data-ttu-id="e148e-145">Замените результат сопоставления на один для каждой возвращаемой сущности, как показано ниже:</span><span class="sxs-lookup"><span data-stu-id="e148e-145">Replace the result mapping with one for each entity being returned, such as the following:</span></span>

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

<span data-ttu-id="e148e-146">Кроме того, можно сопоставлять результирующие наборы со сложными типами, например созданными по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="e148e-146">It is also possible to map the result sets to complex types, such as the one created by default.</span></span> <span data-ttu-id="e148e-147">Для этого нужно создать новый сложный тип, а не удалить его и использовать сложные типы везде, где использовались имена сущностей в приведенных выше примерах.</span><span class="sxs-lookup"><span data-stu-id="e148e-147">To do this you create a new complex type, instead of removing them, and use the complex types everywhere that you had used the entity names in the examples above.</span></span>

<span data-ttu-id="e148e-148">После изменения этих сопоставлений можно сохранить модель и выполнить следующий код для использования хранимой процедуры:</span><span class="sxs-lookup"><span data-stu-id="e148e-148">Once these mappings have been changed then you can save the model and execute the following code to use the stored procedure:</span></span>

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
> <span data-ttu-id="e148e-149">Если вы вручную измените EDMX-файл для своей модели, он будет перезаписан при повторном создании модели из базы данных.</span><span class="sxs-lookup"><span data-stu-id="e148e-149">If you manually edit the edmx file for your model it will be overwritten if you ever regenerate the model from the database.</span></span>

## <a name="summary"></a><span data-ttu-id="e148e-150">Сводка</span><span class="sxs-lookup"><span data-stu-id="e148e-150">Summary</span></span>

<span data-ttu-id="e148e-151">Здесь мы показали два разных метода доступа к нескольким результирующим наборам с помощью Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="e148e-151">Here we have shown two different methods of accessing multiple result sets using Entity Framework.</span></span> <span data-ttu-id="e148e-152">Оба они являются одинаково допустимыми в зависимости от вашей ситуации и предпочтений, поэтому следует выбрать тот, который лучше всего подходит для ваших обстоятельств.</span><span class="sxs-lookup"><span data-stu-id="e148e-152">Both of them are equally valid depending on your situation and preferences and you should choose the one that seems best for your circumstances.</span></span> <span data-ttu-id="e148e-153">Планируется, что поддержка нескольких результирующих наборов будет улучшена в будущих версиях Entity Framework и выполнение действий, описанных в этом документе, больше не потребуется.</span><span class="sxs-lookup"><span data-stu-id="e148e-153">It is planned that the support for multiple result sets will be improved in future versions of Entity Framework and that performing the steps in this document will no longer be necessary.</span></span>
