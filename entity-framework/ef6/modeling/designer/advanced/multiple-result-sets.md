---
title: Хранимые процедуры с несколькими результирующими наборами - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 1b3797f9-cd3d-4752-a55e-47b84b399dc1
ms.openlocfilehash: 098ed88ba52e211965baf3660f0e51bd74c71efd
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/13/2018
ms.locfileid: "45489314"
---
# <a name="stored-procedures-with-multiple-result-sets"></a><span data-ttu-id="462b0-102">Хранимые процедуры с несколькими результирующими наборами</span><span class="sxs-lookup"><span data-stu-id="462b0-102">Stored Procedures with Multiple Result Sets</span></span>
<span data-ttu-id="462b0-103">Иногда при использовании хранимых процедур, которые требуется вернуть несколько результатов значение.</span><span class="sxs-lookup"><span data-stu-id="462b0-103">Sometimes when using stored procedures you will need to return more than one result set.</span></span> <span data-ttu-id="462b0-104">Этот сценарий обычно используется для сокращения объема базы данных циклами, необходимые для составления на одном экране.</span><span class="sxs-lookup"><span data-stu-id="462b0-104">This scenario is commonly used to reduce the number of database round trips required to compose a single screen.</span></span> <span data-ttu-id="462b0-105">До EF5 Entity Framework позволяет вызывать хранимую процедуру, но возвратит только первый результирующий набор в вызывающий код.</span><span class="sxs-lookup"><span data-stu-id="462b0-105">Prior to EF5, Entity Framework would allow the stored procedure to be called but would only return the first result set to the calling code.</span></span>

<span data-ttu-id="462b0-106">В этой статье мы покажем два способа, которые можно использовать для доступа к более чем одного результирующего набора из хранимой процедуры в Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="462b0-106">This article will show you two ways that you can use to access more than one result set from a stored procedure in Entity Framework.</span></span> <span data-ttu-id="462b0-107">Используется только код и работает с и код сначала и конструкторе EF и тот, который работает только с помощью конструктора EF.</span><span class="sxs-lookup"><span data-stu-id="462b0-107">One that uses just code and works with both Code first and the EF Designer and one that only works with the EF Designer.</span></span> <span data-ttu-id="462b0-108">Средства и API поддерживает это должно улучшить в будущих версиях Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="462b0-108">The tooling and API support for this should improve in future versions of Entity Framework.</span></span>

## <a name="model"></a><span data-ttu-id="462b0-109">Модель</span><span class="sxs-lookup"><span data-stu-id="462b0-109">Model</span></span>

<span data-ttu-id="462b0-110">В примерах в этой статье используется базовый блог и сообщения модель, где блог имеет записях и блога принадлежит к одной записи.</span><span class="sxs-lookup"><span data-stu-id="462b0-110">The examples in this article use a basic Blog and Posts model where a blog has many posts and a post belongs to a single blog.</span></span> <span data-ttu-id="462b0-111">Мы будем использовать хранимую процедуру в базе данных, который возвращает все блогов и записей, примерно так:</span><span class="sxs-lookup"><span data-stu-id="462b0-111">We will use a stored procedure in the database that returns all blogs and posts, something like this:</span></span>

``` SQL
    CREATE PROCEDURE [dbo].[GetAllBlogsAndPosts]
    AS
        SELECT * FROM dbo.Blogs
        SELECT * FROM dbo.Posts
```

## <a name="accessing-multiple-result-sets-with-code"></a><span data-ttu-id="462b0-112">Доступ к несколько результирующих наборов с помощью кода</span><span class="sxs-lookup"><span data-stu-id="462b0-112">Accessing Multiple Result Sets with Code</span></span>

<span data-ttu-id="462b0-113">Позволяет выполнить использовать код для выдачи необработанные команду SQL для выполнения хранимой процедуры.</span><span class="sxs-lookup"><span data-stu-id="462b0-113">We can execute use code to issue a raw SQL command to execute our stored procedure.</span></span> <span data-ttu-id="462b0-114">Этот подход удобен для работы с и код сначала и конструкторе EF.</span><span class="sxs-lookup"><span data-stu-id="462b0-114">The advantage of this approach is that it works with both Code first and the EF Designer.</span></span>

<span data-ttu-id="462b0-115">Чтобы получить несколько результирующих задает рабочего, нам нужно уменьшится до API ObjectContext с помощью интерфейса IObjectContextAdapter.</span><span class="sxs-lookup"><span data-stu-id="462b0-115">In order to get multiple result sets working we need to drop to the ObjectContext API by using the IObjectContextAdapter interface.</span></span>

<span data-ttu-id="462b0-116">Когда у нас ObjectContext мы можно использовать метод Translate преобразовать результаты наших хранимой процедуры, в сущности, которые могут отслеживаться и использоваться в EF в обычном режиме.</span><span class="sxs-lookup"><span data-stu-id="462b0-116">Once we have an ObjectContext then we can use the Translate method to translate the results of our stored procedure into entities that can be tracked and used in EF as normal.</span></span> <span data-ttu-id="462b0-117">В следующем образце кода демонстрируется это в действии.</span><span class="sxs-lookup"><span data-stu-id="462b0-117">The following code sample demonstrates this in action.</span></span>

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

<span data-ttu-id="462b0-118">Метод Translate принимает средство чтения, мы получили, когда выполнена процедура, является именем набора сущностей и MergeOption.</span><span class="sxs-lookup"><span data-stu-id="462b0-118">The Translate method accepts the reader that we received when we executed the procedure, an EntitySet name, and a MergeOption.</span></span> <span data-ttu-id="462b0-119">Имя EntitySet будет таким же, как свойства DbSet в вашем производном контексте.</span><span class="sxs-lookup"><span data-stu-id="462b0-119">The EntitySet name will be the same as the DbSet property on your derived context.</span></span> <span data-ttu-id="462b0-120">Перечисление MergeOption управляет, как обрабатываются результаты, если та же сущность уже существует в памяти.</span><span class="sxs-lookup"><span data-stu-id="462b0-120">The MergeOption enum controls how results are handled if the same entity already exists in memory.</span></span>

<span data-ttu-id="462b0-121">Здесь мы итерации по коллекции блогов, прежде чем мы называем NextResult, это очень важно, учитывая приведенный выше код так, как первый результирующий набор, которые должны использоваться перед переходом к следующему результирующему набору.</span><span class="sxs-lookup"><span data-stu-id="462b0-121">Here we iterate through the collection of blogs before we call NextResult, this is important given the above code because the first result set must be consumed before moving to the next result set.</span></span>

<span data-ttu-id="462b0-122">После перевода двух методы вызываются, то сущности Blog и Post отслеживаются EF так же, как любой другой сущности и поэтому можно изменить или удалить и сохранен в обычном режиме.</span><span class="sxs-lookup"><span data-stu-id="462b0-122">Once the two translate methods are called then the Blog and Post entities are tracked by EF the same way as any other entity and so can be modified or deleted and saved as normal.</span></span>

>[!NOTE]
> <span data-ttu-id="462b0-123">При создании сущности с помощью метода Translate EF не принимает любое сопоставление в учетной записи.</span><span class="sxs-lookup"><span data-stu-id="462b0-123">EF does not take any mapping into account when it creates entities using the Translate method.</span></span> <span data-ttu-id="462b0-124">Он просто будет соответствовать имена столбцов в результирующем наборе с именами свойств в классах.</span><span class="sxs-lookup"><span data-stu-id="462b0-124">It will simply match column names in the result set with property names on your classes.</span></span>

>[!NOTE]
> <span data-ttu-id="462b0-125">Что если имеется отложенная загрузка включена, при обращении к свойству сообщения на одной сущности блог затем EF будет соединиться с базой данных для пассивной загрузки все сообщения, несмотря на то, что мы уже загружен их все.</span><span class="sxs-lookup"><span data-stu-id="462b0-125">That if you have lazy loading enabled, accessing the posts property on one of the blog entities then EF will connect to the database to lazily load all posts, even though we have already loaded them all.</span></span> <span data-ttu-id="462b0-126">Это обусловлено EF не знает ли вы загрузили все сообщения, или если есть и другие базы данных.</span><span class="sxs-lookup"><span data-stu-id="462b0-126">This is because EF cannot know whether or not you have loaded all posts or if there are more in the database.</span></span> <span data-ttu-id="462b0-127">Если вы хотите избежать этого, необходимо отключить отложенную загрузку.</span><span class="sxs-lookup"><span data-stu-id="462b0-127">If you want to avoid this then you will need to disable lazy loading.</span></span>

## <a name="multiple-result-sets-with-configured-in-edmx"></a><span data-ttu-id="462b0-128">Несколько результирующих наборов с помощью настроенного в EDMX</span><span class="sxs-lookup"><span data-stu-id="462b0-128">Multiple Result Sets with Configured in EDMX</span></span>

>[!NOTE]
> <span data-ttu-id="462b0-129">Необходимо ориентироваться .NET Framework 4.5, чтобы иметь возможность настроить несколько результирующих наборов в EDMX.</span><span class="sxs-lookup"><span data-stu-id="462b0-129">You must target .NET Framework 4.5 to be able to configure multiple result sets in EDMX.</span></span> <span data-ttu-id="462b0-130">Если вы ориентируетесь на .NET 4.0, можно использовать метод на основе кода, показанный в предыдущем разделе.</span><span class="sxs-lookup"><span data-stu-id="462b0-130">If you are targeting .NET 4.0 you can use the code-based method shown in the previous section.</span></span>

<span data-ttu-id="462b0-131">Если вы используете конструктор EF, знает о различных результирующих наборов, которые будут возвращены, также можно изменить модель.</span><span class="sxs-lookup"><span data-stu-id="462b0-131">If you are using the EF Designer, you can also modify your model so that it knows about the different result sets that will be returned.</span></span> <span data-ttu-id="462b0-132">Необходимо знать, прежде чем вручную в том, что инструментарий не несколько результирующих значение помнить, поэтому необходимо будет вручную изменить EDMX-файла.</span><span class="sxs-lookup"><span data-stu-id="462b0-132">One thing to know before hand is that the tooling is not multiple result set aware, so you will need to manually edit the edmx file.</span></span> <span data-ttu-id="462b0-133">Изменения в EDMX-файл, как это будет работать, но он также отключению проверки модели в Visual STUDIO.</span><span class="sxs-lookup"><span data-stu-id="462b0-133">Editing the edmx file like this will work but it will also break the validation of the model in VS.</span></span> <span data-ttu-id="462b0-134">Поэтому если проверка модели вы всегда получите ошибки.</span><span class="sxs-lookup"><span data-stu-id="462b0-134">So if you validate your model you will always get errors.</span></span>

-   <span data-ttu-id="462b0-135">Для этого необходимо добавить хранимую процедуру в модель, как это делается для одного результирующего набора запроса.</span><span class="sxs-lookup"><span data-stu-id="462b0-135">In order to do this you need to add the stored procedure to your model as you would for a single result set query.</span></span>
-   <span data-ttu-id="462b0-136">После этого необходимо щелкнуть правой кнопкой мыши модель и выберите **открыть с помощью...**</span><span class="sxs-lookup"><span data-stu-id="462b0-136">Once you have this then you need to right click on your model and select **Open With..**</span></span> <span data-ttu-id="462b0-137">затем **Xml**</span><span class="sxs-lookup"><span data-stu-id="462b0-137">then **Xml**</span></span>

    ![Открыть как](~/ef6/media/openas.png)

<span data-ttu-id="462b0-139">Создав модель открывается как XML, то необходимо выполнить следующие действия.</span><span class="sxs-lookup"><span data-stu-id="462b0-139">Once you have the model opened as XML then you need to do the following steps:</span></span>

-   <span data-ttu-id="462b0-140">Найти сложный тип и функцию импорта в модель:</span><span class="sxs-lookup"><span data-stu-id="462b0-140">Find the complex type and function import in your model:</span></span>

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

 

-   <span data-ttu-id="462b0-141">Удаление сложного типа</span><span class="sxs-lookup"><span data-stu-id="462b0-141">Remove the complex type</span></span>
-   <span data-ttu-id="462b0-142">Обновления, импорта функции, таким образом, чтобы он был сопоставлен сущностей, в нашем случае он будет выглядеть следующим образом:</span><span class="sxs-lookup"><span data-stu-id="462b0-142">Update the function import so that it maps to your entities, in our case it will look like the following:</span></span>

``` xml
    <FunctionImport Name="GetAllBlogsAndPosts">
      <ReturnType EntitySet="Blogs" Type="Collection(BlogModel.Blog)" />
      <ReturnType EntitySet="Posts" Type="Collection(BlogModel.Post)" />
    </FunctionImport>
```

<span data-ttu-id="462b0-143">Это сообщает модели, что хранимая процедура вернет двух коллекций, одну из записей в блогах и из операции post.</span><span class="sxs-lookup"><span data-stu-id="462b0-143">This tells the model that the stored procedure will return two collections, one of blog entries and one of post entries.</span></span>

-   <span data-ttu-id="462b0-144">Найдите элемент сопоставления функции:</span><span class="sxs-lookup"><span data-stu-id="462b0-144">Find the function mapping element:</span></span>

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

-   <span data-ttu-id="462b0-145">Замените сопоставление результат одной для каждой сущности, возвращаемых, например следующие:</span><span class="sxs-lookup"><span data-stu-id="462b0-145">Replace the result mapping with one for each entity being returned, such as the following:</span></span>

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

<span data-ttu-id="462b0-146">Это также можно сопоставить с результирующими наборами сложные типы, например, созданный по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="462b0-146">It is also possible to map the result sets to complex types, such as the one created by default.</span></span> <span data-ttu-id="462b0-147">Для этого создайте новый сложный тип, а отключите их и использовать сложные типы везде, вы использовали имена сущностей в приведенных выше примерах.</span><span class="sxs-lookup"><span data-stu-id="462b0-147">To do this you create a new complex type, instead of removing them, and use the complex types everywhere that you had used the entity names in the examples above.</span></span>

<span data-ttu-id="462b0-148">После изменились эти сопоставления можно сохранить модель и выполните следующий код, чтобы хранимая процедура:</span><span class="sxs-lookup"><span data-stu-id="462b0-148">Once these mappings have been changed then you can save the model and execute the following code to use the stored procedure:</span></span>

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
> <span data-ttu-id="462b0-149">Если вы вручную вносили в EDMX-файл для модели он будет перезаписан, если вы когда-либо повторно создать модель из базы данных.</span><span class="sxs-lookup"><span data-stu-id="462b0-149">If you manually edit the edmx file for your model it will be overwritten if you ever regenerate the model from the database.</span></span>

## <a name="summary"></a><span data-ttu-id="462b0-150">Сводка</span><span class="sxs-lookup"><span data-stu-id="462b0-150">Summary</span></span>

<span data-ttu-id="462b0-151">Здесь мы показали, два метода доступа к несколько результирующих наборов с помощью Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="462b0-151">Here we have shown two different methods of accessing multiple result sets using Entity Framework.</span></span> <span data-ttu-id="462b0-152">Оба значения являются одинаково допустимыми зависимости от конкретной ситуации и предпочтения и вам следует выбрать тот, который кажется наиболее обстоятельствах.</span><span class="sxs-lookup"><span data-stu-id="462b0-152">Both of them are equally valid depending on your situation and preferences and you should choose the one that seems best for your circumstances.</span></span> <span data-ttu-id="462b0-153">Планируется, что поддержка нескольких результирующих наборов, будет улучшена в будущем версиях Entity Framework и, выполнив действия в этом документе будет больше не нужен.</span><span class="sxs-lookup"><span data-stu-id="462b0-153">It is planned that the support for multiple result sets will be improved in future versions of Entity Framework and that performing the steps in this document will no longer be necessary.</span></span>
