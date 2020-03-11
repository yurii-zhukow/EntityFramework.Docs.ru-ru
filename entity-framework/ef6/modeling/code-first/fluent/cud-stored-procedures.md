---
title: Code First вставки, обновления и удаления хранимых процедур — EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 9a7ae7f9-4072-4843-877d-506dd7eef576
ms.openlocfilehash: bfc56671814aec1965ac054ff901297e5cdbbecb
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/06/2020
ms.locfileid: "78415775"
---
# <a name="code-first-insert-update-and-delete-stored-procedures"></a><span data-ttu-id="77f23-102">Code First вставки, обновления и удаления хранимых процедур</span><span class="sxs-lookup"><span data-stu-id="77f23-102">Code First Insert, Update, and Delete Stored Procedures</span></span>
> [!NOTE]
> <span data-ttu-id="77f23-103">**Только в EF6 и более поздних версиях**. Функции, API и другие возможности, описанные на этой странице, появились в Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="77f23-103">**EF6 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 6.</span></span> <span data-ttu-id="77f23-104">При использовании более ранней версии могут быть неприменимы некоторые или все сведения.</span><span class="sxs-lookup"><span data-stu-id="77f23-104">If you are using an earlier version, some or all of the information does not apply.</span></span>  

<span data-ttu-id="77f23-105">По умолчанию Code First настраивает все сущности для выполнения команд INSERT, Update и DELETE с прямым доступом к таблицам.</span><span class="sxs-lookup"><span data-stu-id="77f23-105">By default, Code First will configure all entities to perform insert, update and delete commands using direct table access.</span></span> <span data-ttu-id="77f23-106">Начиная с EF6 можно настроить модель Code First для использования хранимых процедур для некоторых или всех сущностей в модели.</span><span class="sxs-lookup"><span data-stu-id="77f23-106">Starting in EF6 you can configure your Code First model to use stored procedures for some or all entities in your model.</span></span>  

## <a name="basic-entity-mapping"></a><span data-ttu-id="77f23-107">Базовое сопоставление сущностей</span><span class="sxs-lookup"><span data-stu-id="77f23-107">Basic Entity Mapping</span></span>  

<span data-ttu-id="77f23-108">Вы можете использовать хранимые процедуры для вставки, обновления и удаления с помощью API Fluent.</span><span class="sxs-lookup"><span data-stu-id="77f23-108">You can opt into using stored procedures for insert, update and delete using the Fluent API.</span></span>  

``` csharp
modelBuilder
  .Entity<Blog>()
  .MapToStoredProcedures();
```  

<span data-ttu-id="77f23-109">Это приведет к тому, что Code First будет использовать некоторые соглашения для создания ожидаемой формы хранимых процедур в базе данных.</span><span class="sxs-lookup"><span data-stu-id="77f23-109">Doing this will cause Code First to use some conventions to build the expected shape of the stored procedures in the database.</span></span>  

- <span data-ttu-id="77f23-110">Три хранимые процедуры под названием **\<type_name\>_Insert**, **\<** type_name\>_Update и **\<** type_name\>_Delete (например, Blog_Insert, Blog_Update и Blog_Delete).</span><span class="sxs-lookup"><span data-stu-id="77f23-110">Three stored procedures named **\<type_name\>_Insert**, **\<type_name\>_Update** and **\<type_name\>_Delete** (for example, Blog_Insert, Blog_Update and Blog_Delete).</span></span>  
- <span data-ttu-id="77f23-111">Имена параметров соответствуют именам свойств.</span><span class="sxs-lookup"><span data-stu-id="77f23-111">Parameter names correspond to the property names.</span></span>  
  > [!NOTE]
  > <span data-ttu-id="77f23-112">Если вы используете Хасколумннаме () или атрибут Column для переименования столбца для данного свойства, то это имя используется для параметров, а не для имени свойства.</span><span class="sxs-lookup"><span data-stu-id="77f23-112">If you use HasColumnName() or the Column attribute to rename the column for a given property then this name is used for parameters instead of the property name.</span></span>  
- <span data-ttu-id="77f23-113">**Хранимая процедура INSERT** будет иметь параметр для каждого свойства, за исключением тех, которые помечены как созданные хранилищем (Identity или computeed).</span><span class="sxs-lookup"><span data-stu-id="77f23-113">**The insert stored procedure** will have a parameter for every property, except for those marked as store generated (identity or computed).</span></span> <span data-ttu-id="77f23-114">Хранимая процедура должна возвращать результирующий набор со столбцом для каждого свойства, созданного хранилищем.</span><span class="sxs-lookup"><span data-stu-id="77f23-114">The stored procedure should return a result set with a column for each store generated property.</span></span>  
- <span data-ttu-id="77f23-115">**Хранимая процедура UPDATE** будет иметь параметр для каждого свойства, за исключением тех, которые помечены шаблоном, сформированным хранилищем "rebyd".</span><span class="sxs-lookup"><span data-stu-id="77f23-115">**The update stored procedure** will have a parameter for every property, except for those marked with a store generated pattern of 'Computed'.</span></span> <span data-ttu-id="77f23-116">Некоторым маркерам параллелизма требуется параметр для исходного значения. Дополнительные сведения см. в разделе " *маркеры параллелизма* " ниже.</span><span class="sxs-lookup"><span data-stu-id="77f23-116">Some concurrency tokens require a parameter for the original value, see the *Concurrency Tokens* section below for details.</span></span> <span data-ttu-id="77f23-117">Хранимая процедура должна возвращать результирующий набор со столбцом для каждого вычисляемого свойства.</span><span class="sxs-lookup"><span data-stu-id="77f23-117">The stored procedure should return a result set with a column for each computed property.</span></span>  
- <span data-ttu-id="77f23-118">**Хранимая процедура DELETE** должна иметь параметр для значения ключа сущности (или несколько параметров, если у сущности есть составной ключ).</span><span class="sxs-lookup"><span data-stu-id="77f23-118">**The delete stored procedure** should have a parameter for the key value of the entity (or multiple parameters if the entity has a composite key).</span></span> <span data-ttu-id="77f23-119">Кроме того, процедура удаления также должна иметь параметры для любых независимых внешних ключей ассоциации в целевой таблице (связи, у которых нет соответствующих свойств внешнего ключа, объявленных в сущности).</span><span class="sxs-lookup"><span data-stu-id="77f23-119">Additionally, the delete procedure should also have parameters for any independent association foreign keys on the target table (relationships that do not have corresponding foreign key properties declared in the entity).</span></span> <span data-ttu-id="77f23-120">Некоторым маркерам параллелизма требуется параметр для исходного значения. Дополнительные сведения см. в разделе " *маркеры параллелизма* " ниже.</span><span class="sxs-lookup"><span data-stu-id="77f23-120">Some concurrency tokens require a parameter for the original value, see the *Concurrency Tokens* section below for details.</span></span>  

<span data-ttu-id="77f23-121">В качестве примера используйте следующий класс:</span><span class="sxs-lookup"><span data-stu-id="77f23-121">Using the following class as an example:</span></span>  

``` csharp
public class Blog  
{  
  public int BlogId { get; set; }  
  public string Name { get; set; }  
  public string Url { get; set; }  
}
```  

<span data-ttu-id="77f23-122">Хранимые процедуры по умолчанию будут выглядеть следующим образом:</span><span class="sxs-lookup"><span data-stu-id="77f23-122">The default stored procedures would be:</span></span>  

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

### <a name="overriding-the-defaults"></a><span data-ttu-id="77f23-123">Переопределение значений по умолчанию</span><span class="sxs-lookup"><span data-stu-id="77f23-123">Overriding the Defaults</span></span>  

<span data-ttu-id="77f23-124">Часть, настроенную по умолчанию, можно переопределить.</span><span class="sxs-lookup"><span data-stu-id="77f23-124">You can override part or all of what was configured by default.</span></span>  

<span data-ttu-id="77f23-125">Можно изменить имя одной или нескольких хранимых процедур.</span><span class="sxs-lookup"><span data-stu-id="77f23-125">You can change the name of one or more stored procedures.</span></span> <span data-ttu-id="77f23-126">В этом примере переименовывается только хранимая процедура UPDATE.</span><span class="sxs-lookup"><span data-stu-id="77f23-126">This example renames the update stored procedure only.</span></span>  

``` csharp
modelBuilder  
  .Entity<Blog>()  
  .MapToStoredProcedures(s =>  
    s.Update(u => u.HasName("modify_blog")));
```  

<span data-ttu-id="77f23-127">В этом примере переименование всех трех хранимых процедур.</span><span class="sxs-lookup"><span data-stu-id="77f23-127">This example renames all three stored procedures.</span></span>  

``` csharp
modelBuilder  
  .Entity<Blog>()  
  .MapToStoredProcedures(s =>  
    s.Update(u => u.HasName("modify_blog"))  
     .Delete(d => d.HasName("delete_blog"))  
     .Insert(i => i.HasName("insert_blog")));
```  

<span data-ttu-id="77f23-128">В этих примерах вызовы объединяются в цепочки, но можно также использовать синтаксис лямбда-блоков.</span><span class="sxs-lookup"><span data-stu-id="77f23-128">In these examples the calls are chained together, but you can also use lambda block syntax.</span></span>  

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

<span data-ttu-id="77f23-129">В этом примере переименовывается параметр для свойства Блогид в хранимой процедуре Update.</span><span class="sxs-lookup"><span data-stu-id="77f23-129">This example renames the parameter for the BlogId property on the update stored procedure.</span></span>  

``` csharp
modelBuilder  
  .Entity<Blog>()  
  .MapToStoredProcedures(s =>  
    s.Update(u => u.Parameter(b => b.BlogId, "blog_id")));
```  

<span data-ttu-id="77f23-130">Все эти вызовы являются связанными и взаимосвязанными.</span><span class="sxs-lookup"><span data-stu-id="77f23-130">These calls are all chainable and composable.</span></span> <span data-ttu-id="77f23-131">Ниже приведен пример, в котором переименование всех трех хранимых процедур и их параметров.</span><span class="sxs-lookup"><span data-stu-id="77f23-131">Here is an example that renames all three stored procedures and their parameters.</span></span>  

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

<span data-ttu-id="77f23-132">Также можно изменить имена столбцов в результирующем наборе, содержащем значения, сформированные базой данных.</span><span class="sxs-lookup"><span data-stu-id="77f23-132">You can also change the name of the columns in the result set that contains database generated values.</span></span>  

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

## <a name="relationships-without-a-foreign-key-in-the-class-independent-associations"></a><span data-ttu-id="77f23-133">Связи без внешнего ключа в классе (независимые ассоциации)</span><span class="sxs-lookup"><span data-stu-id="77f23-133">Relationships Without a Foreign Key in the Class (Independent Associations)</span></span>  

<span data-ttu-id="77f23-134">Если свойство внешнего ключа включено в определение класса, соответствующий параметр можно переименовать так же, как и любое другое свойство.</span><span class="sxs-lookup"><span data-stu-id="77f23-134">When a foreign key property is included in the class definition, the corresponding parameter can be renamed in the same way as any other property.</span></span> <span data-ttu-id="77f23-135">Если отношение существует без свойства внешнего ключа в классе, имя параметра по умолчанию — **\<navigation_property_name\>_\<primary_key_name\>** .</span><span class="sxs-lookup"><span data-stu-id="77f23-135">When a relationship exists without a foreign key property in the class, the default parameter name is **\<navigation_property_name\>_\<primary_key_name\>**.</span></span>  

<span data-ttu-id="77f23-136">Например, следующие определения классов приведут к тому, что в хранимых процедурах для вставки и обновления записей будет Blog_BlogId ожидаемый параметр.</span><span class="sxs-lookup"><span data-stu-id="77f23-136">For example, the following class definitions would result in a Blog_BlogId parameter being expected in the stored procedures to insert and update Posts.</span></span>  

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

### <a name="overriding-the-defaults"></a><span data-ttu-id="77f23-137">Переопределение значений по умолчанию</span><span class="sxs-lookup"><span data-stu-id="77f23-137">Overriding the Defaults</span></span>  

<span data-ttu-id="77f23-138">Параметры внешних ключей, не включаемых в класс, можно изменить, указав путь к свойству первичного ключа в методе Parameter.</span><span class="sxs-lookup"><span data-stu-id="77f23-138">You can change parameters for foreign keys that are not included in the class by supplying the path to the primary key property to the Parameter method.</span></span>  

``` csharp
modelBuilder
  .Entity<Post>()  
  .MapToStoredProcedures(s =>  
    s.Insert(i => i.Parameter(p => p.Blog.BlogId, "blog_id")));
```  

<span data-ttu-id="77f23-139">Если у вас нет свойства навигации в зависимой сущности (т. е.</span><span class="sxs-lookup"><span data-stu-id="77f23-139">If you don’t have a navigation property on the dependent entity (i.e</span></span> <span data-ttu-id="77f23-140">нет свойства POST. blog. Затем можно использовать метод Association, чтобы определить другой конец связи, а затем настроить параметры, соответствующие каждому из ключевых свойств.</span><span class="sxs-lookup"><span data-stu-id="77f23-140">no Post.Blog property) then you can use the Association method to identify the other end of the relationship and then configure the parameters that correspond to each of the key property(s).</span></span>  

``` csharp
modelBuilder
  .Entity<Post>()  
  .MapToStoredProcedures(s =>  
    s.Insert(i => i.Navigation<Blog>(  
      b => b.Posts,  
      c => c.Parameter(b => b.BlogId, "blog_id"))));
```  

## <a name="concurrency-tokens"></a><span data-ttu-id="77f23-141">Маркеры параллелизма</span><span class="sxs-lookup"><span data-stu-id="77f23-141">Concurrency Tokens</span></span>  

<span data-ttu-id="77f23-142">Для выполнения хранимых процедур Update и Delete также может потребоваться обработка параллелизма.</span><span class="sxs-lookup"><span data-stu-id="77f23-142">Update and delete stored procedures may also need to deal with concurrency:</span></span>  

- <span data-ttu-id="77f23-143">Если сущность содержит маркеры параллелизма, то хранимая процедура может дополнительно иметь выходной параметр, возвращающий количество обновленных или удаленных строк (затронутых строк).</span><span class="sxs-lookup"><span data-stu-id="77f23-143">If the entity contains concurrency tokens, the stored procedure can optionally have an output parameter that returns the number of rows updated/deleted (rows affected).</span></span> <span data-ttu-id="77f23-144">Такой параметр должен быть настроен с помощью метода Ровсаффектедпараметер.</span><span class="sxs-lookup"><span data-stu-id="77f23-144">Such a parameter must be configured using the RowsAffectedParameter method.</span></span>  
<span data-ttu-id="77f23-145">По умолчанию EF использует значение, возвращаемое методом ExecuteNonQuery, для определения количества затронутых строк.</span><span class="sxs-lookup"><span data-stu-id="77f23-145">By default EF uses the return value from ExecuteNonQuery to determine how many rows were affected.</span></span> <span data-ttu-id="77f23-146">Указание выходного параметра ROWS полезно, если вы выполняете любую логику в sproc, которая приведет к неправильному возвращаемому значению ExecuteNonQuery (от перспективы EF) в конце выполнения.</span><span class="sxs-lookup"><span data-stu-id="77f23-146">Specifying a rows affected output parameter is useful if you perform any logic in your sproc that would result in the return value of ExecuteNonQuery being incorrect (from EF's perspective) at the end of execution.</span></span>  
- <span data-ttu-id="77f23-147">Для каждого маркера параллелизма будет указан параметр с именем **\<property_name\>_Original** (например, Timestamp_Original).</span><span class="sxs-lookup"><span data-stu-id="77f23-147">For each concurrency token there will be a parameter named **\<property_name\>_Original** (for example, Timestamp_Original ).</span></span> <span data-ttu-id="77f23-148">Ему будет передано исходное значение этого свойства — значение при запросе из базы данных.</span><span class="sxs-lookup"><span data-stu-id="77f23-148">This will be passed the original value of this property – the value when queried from the database.</span></span>  
    - <span data-ttu-id="77f23-149">Маркеры параллелизма, которые вычисляются базой данных (например, метки времени), будут иметь только исходный параметр значения.</span><span class="sxs-lookup"><span data-stu-id="77f23-149">Concurrency tokens that are computed by the database – such as timestamps – will only have an original value parameter.</span></span>  
    - <span data-ttu-id="77f23-150">Невычисленные свойства, заданные как маркеры параллелизма, также будут иметь параметр для нового значения в процедуре обновления.</span><span class="sxs-lookup"><span data-stu-id="77f23-150">Non-computed properties that are set as concurrency tokens will also have a parameter for the new value in the update procedure.</span></span> <span data-ttu-id="77f23-151">При этом используются соглашения об именовании, уже описанные для новых значений.</span><span class="sxs-lookup"><span data-stu-id="77f23-151">This uses the naming conventions already discussed for new values.</span></span> <span data-ttu-id="77f23-152">Примером такого маркера будет использование URL-адреса блога в качестве маркера параллелизма. новое значение является обязательным, поскольку его можно обновить до нового значения в коде (в отличие от маркера метки времени, который обновляется только базой данных).</span><span class="sxs-lookup"><span data-stu-id="77f23-152">An example of such a token would be using a Blog's URL as a concurrency token, the new value is required because this can be updated to a new value by your code (unlike a Timestamp token which is only updated by the database).</span></span>  

<span data-ttu-id="77f23-153">Это пример класса и хранимая процедура UPDATE с маркером параллелизма timestamp.</span><span class="sxs-lookup"><span data-stu-id="77f23-153">This is an example class and update stored procedure with a timestamp concurrency token.</span></span>  

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

<span data-ttu-id="77f23-154">Ниже приведен пример класса и хранимая процедура UPDATE с невычисленным маркером параллелизма.</span><span class="sxs-lookup"><span data-stu-id="77f23-154">Here is an example class and update stored procedure with non-computed concurrency token.</span></span>  

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

### <a name="overriding-the-defaults"></a><span data-ttu-id="77f23-155">Переопределение значений по умолчанию</span><span class="sxs-lookup"><span data-stu-id="77f23-155">Overriding the Defaults</span></span>  

<span data-ttu-id="77f23-156">При необходимости можно ввести параметр, затронутый строкой.</span><span class="sxs-lookup"><span data-stu-id="77f23-156">You can optionally introduce a rows affected parameter.</span></span>  

``` csharp
modelBuilder  
  .Entity<Blog>()  
  .MapToStoredProcedures(s =>  
    s.Update(u => u.RowsAffectedParameter("rows_affected")));
```  

<span data-ttu-id="77f23-157">Для рассчитанных маркеров параллелизма базы данных — где передано только исходное значение — можно просто использовать стандартный механизм переименования параметров, чтобы переименовать параметр для исходного значения.</span><span class="sxs-lookup"><span data-stu-id="77f23-157">For database computed concurrency tokens – where only the original value is passed – you can just use the standard parameter renaming mechanism to rename the parameter for the original value.</span></span>  

``` csharp
modelBuilder  
  .Entity<Blog>()  
  .MapToStoredProcedures(s =>  
    s.Update(u => u.Parameter(b => b.Timestamp, "blog_timestamp")));
```  

<span data-ttu-id="77f23-158">Для невычисленных маркеров параллелизма, где передаются исходное и новое значения — можно использовать перегрузку параметра, которая позволяет указать имя для каждого параметра.</span><span class="sxs-lookup"><span data-stu-id="77f23-158">For non-computed concurrency tokens – where both the original and new value are passed – you can use an overload of Parameter that allows you to supply a name for each parameter.</span></span>  

``` csharp
modelBuilder
 .Entity<Blog>()
 .MapToStoredProcedures(s => s.Update(u => u.Parameter(b => b.Url, "blog_url", "blog_original_url")));
```  

## <a name="many-to-many-relationships"></a><span data-ttu-id="77f23-159">Отношения &quot;многие ко многим&quot;</span><span class="sxs-lookup"><span data-stu-id="77f23-159">Many to Many Relationships</span></span>  

<span data-ttu-id="77f23-160">В этом разделе мы будем использовать следующие классы.</span><span class="sxs-lookup"><span data-stu-id="77f23-160">We’ll use the following classes as an example in this section.</span></span>  

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

<span data-ttu-id="77f23-161">Многие ко многим связям можно сопоставить с хранимыми процедурами с помощью следующего синтаксиса.</span><span class="sxs-lookup"><span data-stu-id="77f23-161">Many to many relationships can be mapped to stored procedures with the following syntax.</span></span>  

``` csharp
modelBuilder  
  .Entity<Post>()  
  .HasMany(p => p.Tags)  
  .WithMany(t => t.Posts)  
  .MapToStoredProcedures();
```  

<span data-ttu-id="77f23-162">Если никакая другая конфигурация не указана, по умолчанию используется следующая фигура хранимой процедуры.</span><span class="sxs-lookup"><span data-stu-id="77f23-162">If no other configuration is supplied then the following stored procedure shape is used by default.</span></span>  

- <span data-ttu-id="77f23-163">Две хранимые процедуры с именами **\<type_one\>\<type_two\>_Insert** и **\<type_one\>\<type_two\>_Delete** (например, PostTag_Insert и PostTag_Delete).</span><span class="sxs-lookup"><span data-stu-id="77f23-163">Two stored procedures named **\<type_one\>\<type_two\>_Insert** and **\<type_one\>\<type_two\>_Delete** (for example, PostTag_Insert and PostTag_Delete).</span></span>  
- <span data-ttu-id="77f23-164">Параметры будут ключевыми значениями для каждого типа.</span><span class="sxs-lookup"><span data-stu-id="77f23-164">The parameters will be the key value(s) for each type.</span></span> <span data-ttu-id="77f23-165">Имя каждого параметра, который **\<type_name\>_\<property_name**\>(например, Post_PostId и Tag_TagId).</span><span class="sxs-lookup"><span data-stu-id="77f23-165">The name of each parameter being **\<type_name\>_\<property_name\>** (for example, Post_PostId and Tag_TagId).</span></span>

<span data-ttu-id="77f23-166">Ниже приведены примеры хранимых процедур INSERT и Update.</span><span class="sxs-lookup"><span data-stu-id="77f23-166">Here are example insert and update stored procedures.</span></span>  

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

### <a name="overriding-the-defaults"></a><span data-ttu-id="77f23-167">Переопределение значений по умолчанию</span><span class="sxs-lookup"><span data-stu-id="77f23-167">Overriding the Defaults</span></span>  

<span data-ttu-id="77f23-168">Имена процедур и параметров можно настроить аналогичным образом для хранимых процедур сущностей.</span><span class="sxs-lookup"><span data-stu-id="77f23-168">The procedure and parameter names can be configured in a similar way to entity stored procedures.</span></span>  

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
