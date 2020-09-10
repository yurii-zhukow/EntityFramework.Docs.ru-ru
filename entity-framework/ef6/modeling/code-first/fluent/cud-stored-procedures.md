---
title: Code First вставки, обновления и удаления хранимых процедур — EF6
description: Code First вставки, обновления и удаления хранимых процедур в Entity Framework 6
author: divega
ms.date: 10/23/2016
ms.assetid: 9a7ae7f9-4072-4843-877d-506dd7eef576
uid: ef6/modeling/code-first/fluent/cud-stored-procedures
ms.openlocfilehash: 4db54d7199baa408017159e25ce79a9d70707c59
ms.sourcegitcommit: 7c3939504bb9da3f46bea3443638b808c04227c2
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/09/2020
ms.locfileid: "89618120"
---
# <a name="code-first-insert-update-and-delete-stored-procedures"></a><span data-ttu-id="71f97-103">Code First вставки, обновления и удаления хранимых процедур</span><span class="sxs-lookup"><span data-stu-id="71f97-103">Code First Insert, Update, and Delete Stored Procedures</span></span>
> [!NOTE]
> <span data-ttu-id="71f97-104">**Только в EF6 и более поздних версиях**. Функции, API и другие возможности, описанные на этой странице, появились в Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="71f97-104">**EF6 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 6.</span></span> <span data-ttu-id="71f97-105">При использовании более ранней версии могут быть неприменимы некоторые или все сведения.</span><span class="sxs-lookup"><span data-stu-id="71f97-105">If you are using an earlier version, some or all of the information does not apply.</span></span>  

<span data-ttu-id="71f97-106">По умолчанию Code First настраивает все сущности для выполнения команд INSERT, Update и DELETE с прямым доступом к таблицам.</span><span class="sxs-lookup"><span data-stu-id="71f97-106">By default, Code First will configure all entities to perform insert, update and delete commands using direct table access.</span></span> <span data-ttu-id="71f97-107">Начиная с EF6 можно настроить модель Code First для использования хранимых процедур для некоторых или всех сущностей в модели.</span><span class="sxs-lookup"><span data-stu-id="71f97-107">Starting in EF6 you can configure your Code First model to use stored procedures for some or all entities in your model.</span></span>  

## <a name="basic-entity-mapping"></a><span data-ttu-id="71f97-108">Базовое сопоставление сущностей</span><span class="sxs-lookup"><span data-stu-id="71f97-108">Basic Entity Mapping</span></span>  

<span data-ttu-id="71f97-109">Вы можете использовать хранимые процедуры для вставки, обновления и удаления с помощью API Fluent.</span><span class="sxs-lookup"><span data-stu-id="71f97-109">You can opt into using stored procedures for insert, update and delete using the Fluent API.</span></span>  

``` csharp
modelBuilder
  .Entity<Blog>()
  .MapToStoredProcedures();
```  

<span data-ttu-id="71f97-110">Это приведет к тому, что Code First будет использовать некоторые соглашения для создания ожидаемой формы хранимых процедур в базе данных.</span><span class="sxs-lookup"><span data-stu-id="71f97-110">Doing this will cause Code First to use some conventions to build the expected shape of the stored procedures in the database.</span></span>  

- <span data-ttu-id="71f97-111">Три хранимые процедуры с именами \*\* \<type_name\> _Insert**, \*\* \<type_name\> _Update** и \*\* \<type_name\> _Delete\*\* (например, Blog_Insert, Blog_Update и Blog_Delete).</span><span class="sxs-lookup"><span data-stu-id="71f97-111">Three stored procedures named **\<type_name\>_Insert**, **\<type_name\>_Update** and **\<type_name\>_Delete** (for example, Blog_Insert, Blog_Update and Blog_Delete).</span></span>  
- <span data-ttu-id="71f97-112">Имена параметров соответствуют именам свойств.</span><span class="sxs-lookup"><span data-stu-id="71f97-112">Parameter names correspond to the property names.</span></span>  
  > [!NOTE]
  > <span data-ttu-id="71f97-113">Если вы используете Хасколумннаме () или атрибут Column для переименования столбца для данного свойства, то это имя используется для параметров, а не для имени свойства.</span><span class="sxs-lookup"><span data-stu-id="71f97-113">If you use HasColumnName() or the Column attribute to rename the column for a given property then this name is used for parameters instead of the property name.</span></span>  
- <span data-ttu-id="71f97-114">**Хранимая процедура INSERT** будет иметь параметр для каждого свойства, за исключением тех, которые помечены как созданные хранилищем (Identity или computeed).</span><span class="sxs-lookup"><span data-stu-id="71f97-114">**The insert stored procedure** will have a parameter for every property, except for those marked as store generated (identity or computed).</span></span> <span data-ttu-id="71f97-115">Хранимая процедура должна возвращать результирующий набор со столбцом для каждого свойства, созданного хранилищем.</span><span class="sxs-lookup"><span data-stu-id="71f97-115">The stored procedure should return a result set with a column for each store generated property.</span></span>  
- <span data-ttu-id="71f97-116">**Хранимая процедура UPDATE** будет иметь параметр для каждого свойства, за исключением тех, которые помечены шаблоном, сформированным хранилищем "rebyd".</span><span class="sxs-lookup"><span data-stu-id="71f97-116">**The update stored procedure** will have a parameter for every property, except for those marked with a store generated pattern of 'Computed'.</span></span> <span data-ttu-id="71f97-117">Некоторым маркерам параллелизма требуется параметр для исходного значения. Дополнительные сведения см. в разделе " *маркеры параллелизма* " ниже.</span><span class="sxs-lookup"><span data-stu-id="71f97-117">Some concurrency tokens require a parameter for the original value, see the *Concurrency Tokens* section below for details.</span></span> <span data-ttu-id="71f97-118">Хранимая процедура должна возвращать результирующий набор со столбцом для каждого вычисляемого свойства.</span><span class="sxs-lookup"><span data-stu-id="71f97-118">The stored procedure should return a result set with a column for each computed property.</span></span>  
- <span data-ttu-id="71f97-119">**Хранимая процедура DELETE** должна иметь параметр для значения ключа сущности (или несколько параметров, если у сущности есть составной ключ).</span><span class="sxs-lookup"><span data-stu-id="71f97-119">**The delete stored procedure** should have a parameter for the key value of the entity (or multiple parameters if the entity has a composite key).</span></span> <span data-ttu-id="71f97-120">Кроме того, процедура удаления также должна иметь параметры для любых независимых внешних ключей ассоциации в целевой таблице (связи, у которых нет соответствующих свойств внешнего ключа, объявленных в сущности).</span><span class="sxs-lookup"><span data-stu-id="71f97-120">Additionally, the delete procedure should also have parameters for any independent association foreign keys on the target table (relationships that do not have corresponding foreign key properties declared in the entity).</span></span> <span data-ttu-id="71f97-121">Некоторым маркерам параллелизма требуется параметр для исходного значения. Дополнительные сведения см. в разделе " *маркеры параллелизма* " ниже.</span><span class="sxs-lookup"><span data-stu-id="71f97-121">Some concurrency tokens require a parameter for the original value, see the *Concurrency Tokens* section below for details.</span></span>  

<span data-ttu-id="71f97-122">В качестве примера используйте следующий класс:</span><span class="sxs-lookup"><span data-stu-id="71f97-122">Using the following class as an example:</span></span>  

``` csharp
public class Blog  
{  
  public int BlogId { get; set; }  
  public string Name { get; set; }  
  public string Url { get; set; }  
}
```  

<span data-ttu-id="71f97-123">Хранимые процедуры по умолчанию будут выглядеть следующим образом:</span><span class="sxs-lookup"><span data-stu-id="71f97-123">The default stored procedures would be:</span></span>  

``` SQL
CREATE PROCEDURE [dbo].[Blog_Insert]  
  @Name nvarchar(max),  
  @Url nvarchar(max)  
AS  
BEGIN
  INSERT INTO [dbo].[Blogs] ([Name], [Url])
  VALUES (@Name, @Url)

  SELECT SCOPE_IDENTITY() AS BlogId
END
CREATE PROCEDURE [dbo].[Blog_Update]  
  @BlogId int,  
  @Name nvarchar(max),  
  @Url nvarchar(max)  
AS  
  UPDATE [dbo].[Blogs]
  SET [Name] = @Name, [Url] = @Url     
  WHERE BlogId = @BlogId;
CREATE PROCEDURE [dbo].[Blog_Delete]  
  @BlogId int  
AS  
  DELETE FROM [dbo].[Blogs]
  WHERE BlogId = @BlogId
```  

### <a name="overriding-the-defaults"></a><span data-ttu-id="71f97-124">Переопределение значений по умолчанию</span><span class="sxs-lookup"><span data-stu-id="71f97-124">Overriding the Defaults</span></span>  

<span data-ttu-id="71f97-125">Часть, настроенную по умолчанию, можно переопределить.</span><span class="sxs-lookup"><span data-stu-id="71f97-125">You can override part or all of what was configured by default.</span></span>  

<span data-ttu-id="71f97-126">Можно изменить имя одной или нескольких хранимых процедур.</span><span class="sxs-lookup"><span data-stu-id="71f97-126">You can change the name of one or more stored procedures.</span></span> <span data-ttu-id="71f97-127">В этом примере переименовывается только хранимая процедура UPDATE.</span><span class="sxs-lookup"><span data-stu-id="71f97-127">This example renames the update stored procedure only.</span></span>  

``` csharp
modelBuilder  
  .Entity<Blog>()  
  .MapToStoredProcedures(s =>  
    s.Update(u => u.HasName("modify_blog")));
```  

<span data-ttu-id="71f97-128">В этом примере переименование всех трех хранимых процедур.</span><span class="sxs-lookup"><span data-stu-id="71f97-128">This example renames all three stored procedures.</span></span>  

``` csharp
modelBuilder  
  .Entity<Blog>()  
  .MapToStoredProcedures(s =>  
    s.Update(u => u.HasName("modify_blog"))  
     .Delete(d => d.HasName("delete_blog"))  
     .Insert(i => i.HasName("insert_blog")));
```  

<span data-ttu-id="71f97-129">В этих примерах вызовы объединяются в цепочки, но можно также использовать синтаксис лямбда-блоков.</span><span class="sxs-lookup"><span data-stu-id="71f97-129">In these examples the calls are chained together, but you can also use lambda block syntax.</span></span>  

``` csharp
modelBuilder  
  .Entity<Blog>()  
  .MapToStoredProcedures(s =>  
    {  
      s.Update(u => u.HasName("modify_blog"));  
      s.Delete(d => d.HasName("delete_blog"));  
      s.Insert(i => i.HasName("insert_blog"));  
    });
```  

<span data-ttu-id="71f97-130">В этом примере переименовывается параметр для свойства Блогид в хранимой процедуре Update.</span><span class="sxs-lookup"><span data-stu-id="71f97-130">This example renames the parameter for the BlogId property on the update stored procedure.</span></span>  

``` csharp
modelBuilder  
  .Entity<Blog>()  
  .MapToStoredProcedures(s =>  
    s.Update(u => u.Parameter(b => b.BlogId, "blog_id")));
```  

<span data-ttu-id="71f97-131">Все эти вызовы являются связанными и взаимосвязанными.</span><span class="sxs-lookup"><span data-stu-id="71f97-131">These calls are all chainable and composable.</span></span> <span data-ttu-id="71f97-132">Ниже приведен пример, в котором переименование всех трех хранимых процедур и их параметров.</span><span class="sxs-lookup"><span data-stu-id="71f97-132">Here is an example that renames all three stored procedures and their parameters.</span></span>  

``` csharp
modelBuilder  
  .Entity<Blog>()  
  .MapToStoredProcedures(s =>  
    s.Update(u => u.HasName("modify_blog")  
                   .Parameter(b => b.BlogId, "blog_id")  
                   .Parameter(b => b.Name, "blog_name")  
                   .Parameter(b => b.Url, "blog_url"))  
     .Delete(d => d.HasName("delete_blog")  
                   .Parameter(b => b.BlogId, "blog_id"))  
     .Insert(i => i.HasName("insert_blog")  
                   .Parameter(b => b.Name, "blog_name")  
                   .Parameter(b => b.Url, "blog_url")));
```  

<span data-ttu-id="71f97-133">Также можно изменить имена столбцов в результирующем наборе, содержащем значения, сформированные базой данных.</span><span class="sxs-lookup"><span data-stu-id="71f97-133">You can also change the name of the columns in the result set that contains database generated values.</span></span>  

``` csharp
modelBuilder
  .Entity<Blog>()
  .MapToStoredProcedures(s =>
    s.Insert(i => i.Result(b => b.BlogId, "generated_blog_identity")));
```

``` SQL
CREATE PROCEDURE [dbo].[Blog_Insert]  
  @Name nvarchar(max),  
  @Url nvarchar(max)  
AS  
BEGIN
  INSERT INTO [dbo].[Blogs] ([Name], [Url])
  VALUES (@Name, @Url)

  SELECT SCOPE_IDENTITY() AS generated_blog_id
END
```  

## <a name="relationships-without-a-foreign-key-in-the-class-independent-associations"></a><span data-ttu-id="71f97-134">Связи без внешнего ключа в классе (независимые ассоциации)</span><span class="sxs-lookup"><span data-stu-id="71f97-134">Relationships Without a Foreign Key in the Class (Independent Associations)</span></span>  

<span data-ttu-id="71f97-135">Если свойство внешнего ключа включено в определение класса, соответствующий параметр можно переименовать так же, как и любое другое свойство.</span><span class="sxs-lookup"><span data-stu-id="71f97-135">When a foreign key property is included in the class definition, the corresponding parameter can be renamed in the same way as any other property.</span></span> <span data-ttu-id="71f97-136">Если отношение существует без свойства внешнего ключа в классе, имя параметра по умолчанию — \*\* \<navigation_property_name\> _ \<primary_key_name\> \*\*.</span><span class="sxs-lookup"><span data-stu-id="71f97-136">When a relationship exists without a foreign key property in the class, the default parameter name is **\<navigation_property_name\>_\<primary_key_name\>**.</span></span>  

<span data-ttu-id="71f97-137">Например, следующие определения классов приведут к тому, что в хранимых процедурах для вставки и обновления записей будет Blog_BlogId ожидаемый параметр.</span><span class="sxs-lookup"><span data-stu-id="71f97-137">For example, the following class definitions would result in a Blog_BlogId parameter being expected in the stored procedures to insert and update Posts.</span></span>  

``` csharp
public class Blog  
{  
  public int BlogId { get; set; }  
  public string Name { get; set; }  
  public string Url { get; set; }

  public List<Post> Posts { get; set; }  
}  

public class Post  
{  
  public int PostId { get; set; }  
  public string Title { get; set; }  
  public string Content { get; set; }  

  public Blog Blog { get; set; }  
}
```  

### <a name="overriding-the-defaults"></a><span data-ttu-id="71f97-138">Переопределение значений по умолчанию</span><span class="sxs-lookup"><span data-stu-id="71f97-138">Overriding the Defaults</span></span>  

<span data-ttu-id="71f97-139">Параметры внешних ключей, не включаемых в класс, можно изменить, указав путь к свойству первичного ключа в методе Parameter.</span><span class="sxs-lookup"><span data-stu-id="71f97-139">You can change parameters for foreign keys that are not included in the class by supplying the path to the primary key property to the Parameter method.</span></span>  

``` csharp
modelBuilder
  .Entity<Post>()  
  .MapToStoredProcedures(s =>  
    s.Insert(i => i.Parameter(p => p.Blog.BlogId, "blog_id")));
```  

<span data-ttu-id="71f97-140">Если у вас нет свойства навигации в зависимой сущности (т. е.</span><span class="sxs-lookup"><span data-stu-id="71f97-140">If you don’t have a navigation property on the dependent entity (i.e</span></span> <span data-ttu-id="71f97-141">нет свойства POST. blog. Затем можно использовать метод Association, чтобы определить другой конец связи, а затем настроить параметры, соответствующие каждому из ключевых свойств.</span><span class="sxs-lookup"><span data-stu-id="71f97-141">no Post.Blog property) then you can use the Association method to identify the other end of the relationship and then configure the parameters that correspond to each of the key property(s).</span></span>  

``` csharp
modelBuilder
  .Entity<Post>()  
  .MapToStoredProcedures(s =>  
    s.Insert(i => i.Navigation<Blog>(  
      b => b.Posts,  
      c => c.Parameter(b => b.BlogId, "blog_id"))));
```  

## <a name="concurrency-tokens"></a><span data-ttu-id="71f97-142">Маркеры параллелизма</span><span class="sxs-lookup"><span data-stu-id="71f97-142">Concurrency Tokens</span></span>  

<span data-ttu-id="71f97-143">Для выполнения хранимых процедур Update и Delete также может потребоваться обработка параллелизма.</span><span class="sxs-lookup"><span data-stu-id="71f97-143">Update and delete stored procedures may also need to deal with concurrency:</span></span>  

- <span data-ttu-id="71f97-144">Если сущность содержит маркеры параллелизма, то хранимая процедура может дополнительно иметь выходной параметр, возвращающий количество обновленных или удаленных строк (затронутых строк).</span><span class="sxs-lookup"><span data-stu-id="71f97-144">If the entity contains concurrency tokens, the stored procedure can optionally have an output parameter that returns the number of rows updated/deleted (rows affected).</span></span> <span data-ttu-id="71f97-145">Такой параметр должен быть настроен с помощью метода Ровсаффектедпараметер.</span><span class="sxs-lookup"><span data-stu-id="71f97-145">Such a parameter must be configured using the RowsAffectedParameter method.</span></span>  
<span data-ttu-id="71f97-146">По умолчанию EF использует значение, возвращаемое методом ExecuteNonQuery, для определения количества затронутых строк.</span><span class="sxs-lookup"><span data-stu-id="71f97-146">By default EF uses the return value from ExecuteNonQuery to determine how many rows were affected.</span></span> <span data-ttu-id="71f97-147">Указание выходного параметра ROWS полезно, если вы выполняете любую логику в sproc, которая приведет к неправильному возвращаемому значению ExecuteNonQuery (от перспективы EF) в конце выполнения.</span><span class="sxs-lookup"><span data-stu-id="71f97-147">Specifying a rows affected output parameter is useful if you perform any logic in your sproc that would result in the return value of ExecuteNonQuery being incorrect (from EF's perspective) at the end of execution.</span></span>  
- <span data-ttu-id="71f97-148">Для каждого маркера параллелизма будет указан параметр с именем \*\* \<property_name\> _Original\*\* (например, Timestamp_Original).</span><span class="sxs-lookup"><span data-stu-id="71f97-148">For each concurrency token there will be a parameter named **\<property_name\>_Original** (for example, Timestamp_Original ).</span></span> <span data-ttu-id="71f97-149">Ему будет передано исходное значение этого свойства — значение при запросе из базы данных.</span><span class="sxs-lookup"><span data-stu-id="71f97-149">This will be passed the original value of this property – the value when queried from the database.</span></span>  
    - <span data-ttu-id="71f97-150">Маркеры параллелизма, которые вычисляются базой данных (например, метки времени), будут иметь только исходный параметр значения.</span><span class="sxs-lookup"><span data-stu-id="71f97-150">Concurrency tokens that are computed by the database – such as timestamps – will only have an original value parameter.</span></span>  
    - <span data-ttu-id="71f97-151">Невычисленные свойства, заданные как маркеры параллелизма, также будут иметь параметр для нового значения в процедуре обновления.</span><span class="sxs-lookup"><span data-stu-id="71f97-151">Non-computed properties that are set as concurrency tokens will also have a parameter for the new value in the update procedure.</span></span> <span data-ttu-id="71f97-152">При этом используются соглашения об именовании, уже описанные для новых значений.</span><span class="sxs-lookup"><span data-stu-id="71f97-152">This uses the naming conventions already discussed for new values.</span></span> <span data-ttu-id="71f97-153">Примером такого маркера будет использование URL-адреса блога в качестве маркера параллелизма. новое значение является обязательным, поскольку его можно обновить до нового значения в коде (в отличие от маркера метки времени, который обновляется только базой данных).</span><span class="sxs-lookup"><span data-stu-id="71f97-153">An example of such a token would be using a Blog's URL as a concurrency token, the new value is required because this can be updated to a new value by your code (unlike a Timestamp token which is only updated by the database).</span></span>  

<span data-ttu-id="71f97-154">Это пример класса и хранимая процедура UPDATE с маркером параллелизма timestamp.</span><span class="sxs-lookup"><span data-stu-id="71f97-154">This is an example class and update stored procedure with a timestamp concurrency token.</span></span>  

``` csharp
public class Blog  
{  
  public int BlogId { get; set; }  
  public string Name { get; set; }  
  public string Url { get; set; }  
  [Timestamp]
  public byte[] Timestamp { get; set; }
}
```

``` SQL
CREATE PROCEDURE [dbo].[Blog_Update]  
  @BlogId int,  
  @Name nvarchar(max),  
  @Url nvarchar(max),
  @Timestamp_Original rowversion  
AS  
  UPDATE [dbo].[Blogs]
  SET [Name] = @Name, [Url] = @Url     
  WHERE BlogId = @BlogId AND [Timestamp] = @Timestamp_Original
```  

<span data-ttu-id="71f97-155">Ниже приведен пример класса и хранимая процедура UPDATE с невычисленным маркером параллелизма.</span><span class="sxs-lookup"><span data-stu-id="71f97-155">Here is an example class and update stored procedure with non-computed concurrency token.</span></span>  

``` csharp
public class Blog  
{  
  public int BlogId { get; set; }  
  public string Name { get; set; }  
  [ConcurrencyCheck]
  public string Url { get; set; }  
}
```

``` SQL
CREATE PROCEDURE [dbo].[Blog_Update]  
  @BlogId int,  
  @Name nvarchar(max),  
  @Url nvarchar(max),
  @Url_Original nvarchar(max),
AS  
  UPDATE [dbo].[Blogs]
  SET [Name] = @Name, [Url] = @Url     
  WHERE BlogId = @BlogId AND [Url] = @Url_Original
```  

### <a name="overriding-the-defaults"></a><span data-ttu-id="71f97-156">Переопределение значений по умолчанию</span><span class="sxs-lookup"><span data-stu-id="71f97-156">Overriding the Defaults</span></span>  

<span data-ttu-id="71f97-157">При необходимости можно ввести параметр, затронутый строкой.</span><span class="sxs-lookup"><span data-stu-id="71f97-157">You can optionally introduce a rows affected parameter.</span></span>  

``` csharp
modelBuilder  
  .Entity<Blog>()  
  .MapToStoredProcedures(s =>  
    s.Update(u => u.RowsAffectedParameter("rows_affected")));
```  

<span data-ttu-id="71f97-158">Для рассчитанных маркеров параллелизма базы данных — где передано только исходное значение — можно просто использовать стандартный механизм переименования параметров, чтобы переименовать параметр для исходного значения.</span><span class="sxs-lookup"><span data-stu-id="71f97-158">For database computed concurrency tokens – where only the original value is passed – you can just use the standard parameter renaming mechanism to rename the parameter for the original value.</span></span>  

``` csharp
modelBuilder  
  .Entity<Blog>()  
  .MapToStoredProcedures(s =>  
    s.Update(u => u.Parameter(b => b.Timestamp, "blog_timestamp")));
```  

<span data-ttu-id="71f97-159">Для невычисленных маркеров параллелизма, где передаются исходное и новое значения — можно использовать перегрузку параметра, которая позволяет указать имя для каждого параметра.</span><span class="sxs-lookup"><span data-stu-id="71f97-159">For non-computed concurrency tokens – where both the original and new value are passed – you can use an overload of Parameter that allows you to supply a name for each parameter.</span></span>  

``` csharp
modelBuilder
 .Entity<Blog>()
 .MapToStoredProcedures(s => s.Update(u => u.Parameter(b => b.Url, "blog_url", "blog_original_url")));
```  

## <a name="many-to-many-relationships"></a><span data-ttu-id="71f97-160">Отношения &quot;многие ко многим&quot;</span><span class="sxs-lookup"><span data-stu-id="71f97-160">Many to Many Relationships</span></span>  

<span data-ttu-id="71f97-161">В этом разделе мы будем использовать следующие классы.</span><span class="sxs-lookup"><span data-stu-id="71f97-161">We’ll use the following classes as an example in this section.</span></span>  

``` csharp
public class Post  
{  
  public int PostId { get; set; }  
  public string Title { get; set; }  
  public string Content { get; set; }  

  public List<Tag> Tags { get; set; }  
}  

public class Tag  
{  
  public int TagId { get; set; }  
  public string TagName { get; set; }  

  public List<Post> Posts { get; set; }  
}
```  

<span data-ttu-id="71f97-162">Многие ко многим связям можно сопоставить с хранимыми процедурами с помощью следующего синтаксиса.</span><span class="sxs-lookup"><span data-stu-id="71f97-162">Many to many relationships can be mapped to stored procedures with the following syntax.</span></span>  

``` csharp
modelBuilder  
  .Entity<Post>()  
  .HasMany(p => p.Tags)  
  .WithMany(t => t.Posts)  
  .MapToStoredProcedures();
```  

<span data-ttu-id="71f97-163">Если никакая другая конфигурация не указана, по умолчанию используется следующая фигура хранимой процедуры.</span><span class="sxs-lookup"><span data-stu-id="71f97-163">If no other configuration is supplied then the following stored procedure shape is used by default.</span></span>  

- <span data-ttu-id="71f97-164">Две хранимые процедуры с именами \*\* \<type_one\> \<type_two\> _Insert\*\* и \*\* \<type_one\> \<type_two\> _Delete\*\* (например, PostTag_Insert и PostTag_Delete).</span><span class="sxs-lookup"><span data-stu-id="71f97-164">Two stored procedures named **\<type_one\>\<type_two\>_Insert** and **\<type_one\>\<type_two\>_Delete** (for example, PostTag_Insert and PostTag_Delete).</span></span>  
- <span data-ttu-id="71f97-165">Параметры будут ключевыми значениями для каждого типа.</span><span class="sxs-lookup"><span data-stu-id="71f97-165">The parameters will be the key value(s) for each type.</span></span> <span data-ttu-id="71f97-166">Имя каждого параметра \*\* \<type_name\> _ \<property_name\> \*\* (например, Post_PostId и Tag_TagId).</span><span class="sxs-lookup"><span data-stu-id="71f97-166">The name of each parameter being **\<type_name\>_\<property_name\>** (for example, Post_PostId and Tag_TagId).</span></span>

<span data-ttu-id="71f97-167">Ниже приведены примеры хранимых процедур INSERT и Update.</span><span class="sxs-lookup"><span data-stu-id="71f97-167">Here are example insert and update stored procedures.</span></span>  

``` SQL
CREATE PROCEDURE [dbo].[PostTag_Insert]  
  @Post_PostId int,  
  @Tag_TagId int  
AS  
  INSERT INTO [dbo].[Post_Tags] (Post_PostId, Tag_TagId)   
  VALUES (@Post_PostId, @Tag_TagId)
CREATE PROCEDURE [dbo].[PostTag_Delete]  
  @Post_PostId int,  
  @Tag_TagId int  
AS  
  DELETE FROM [dbo].[Post_Tags]    
  WHERE Post_PostId = @Post_PostId AND Tag_TagId = @Tag_TagId
```  

### <a name="overriding-the-defaults"></a><span data-ttu-id="71f97-168">Переопределение значений по умолчанию</span><span class="sxs-lookup"><span data-stu-id="71f97-168">Overriding the Defaults</span></span>  

<span data-ttu-id="71f97-169">Имена процедур и параметров можно настроить аналогичным образом для хранимых процедур сущностей.</span><span class="sxs-lookup"><span data-stu-id="71f97-169">The procedure and parameter names can be configured in a similar way to entity stored procedures.</span></span>  

``` csharp
modelBuilder  
  .Entity<Post>()  
  .HasMany(p => p.Tags)  
  .WithMany(t => t.Posts)  
  .MapToStoredProcedures(s =>  
    s.Insert(i => i.HasName("add_post_tag")  
                   .LeftKeyParameter(p => p.PostId, "post_id")  
                   .RightKeyParameter(t => t.TagId, "tag_id"))  
     .Delete(d => d.HasName("remove_post_tag")  
                   .LeftKeyParameter(p => p.PostId, "post_id")  
                   .RightKeyParameter(t => t.TagId, "tag_id")));
```  
