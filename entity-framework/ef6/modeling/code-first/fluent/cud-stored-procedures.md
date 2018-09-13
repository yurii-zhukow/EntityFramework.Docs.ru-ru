---
title: Код первой вставки, обновления и удаления хранимых процедур — EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 9a7ae7f9-4072-4843-877d-506dd7eef576
ms.openlocfilehash: bfc56671814aec1965ac054ff901297e5cdbbecb
ms.sourcegitcommit: 2b787009fd5be5627f1189ee396e708cd130e07b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/13/2018
ms.locfileid: "45489626"
---
# <a name="code-first-insert-update-and-delete-stored-procedures"></a><span data-ttu-id="0ad20-102">Код первой вставки, обновления и удаления хранимых процедур</span><span class="sxs-lookup"><span data-stu-id="0ad20-102">Code First Insert, Update, and Delete Stored Procedures</span></span>
> [!NOTE]
> <span data-ttu-id="0ad20-103">**Только в EF6 и более поздних версиях**. Функции, API и другие возможности, описанные на этой странице, появились в Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="0ad20-103">**EF6 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 6.</span></span> <span data-ttu-id="0ad20-104">При использовании более ранней версии могут быть неприменимы некоторые или все сведения.</span><span class="sxs-lookup"><span data-stu-id="0ad20-104">If you are using an earlier version, some or all of the information does not apply.</span></span>  

<span data-ttu-id="0ad20-105">По умолчанию Code First настроит все сущности, чтобы выполнить операцию вставки, обновления и удаления команд с помощью прямого доступа к таблицам.</span><span class="sxs-lookup"><span data-stu-id="0ad20-105">By default, Code First will configure all entities to perform insert, update and delete commands using direct table access.</span></span> <span data-ttu-id="0ad20-106">Начиная с EF6 можно настроить модель данных Code First с помощью хранимых процедур для некоторых или всех сущностей в модели.</span><span class="sxs-lookup"><span data-stu-id="0ad20-106">Starting in EF6 you can configure your Code First model to use stored procedures for some or all entities in your model.</span></span>  

## <a name="basic-entity-mapping"></a><span data-ttu-id="0ad20-107">Основные сущности сопоставления</span><span class="sxs-lookup"><span data-stu-id="0ad20-107">Basic Entity Mapping</span></span>  

<span data-ttu-id="0ad20-108">Можно явно задано использование хранимых процедур для вставки, обновления и удаления с помощью Fluent API.</span><span class="sxs-lookup"><span data-stu-id="0ad20-108">You can opt into using stored procedures for insert, update and delete using the Fluent API.</span></span>  

``` csharp
modelBuilder
  .Entity<Blog>()
  .MapToStoredProcedures();
```  

<span data-ttu-id="0ad20-109">Это приведет к неспособности Code First, использование некоторых соглашений для создания Прогнозируемая форма хранимые процедуры в базе данных.</span><span class="sxs-lookup"><span data-stu-id="0ad20-109">Doing this will cause Code First to use some conventions to build the expected shape of the stored procedures in the database.</span></span>  

- <span data-ttu-id="0ad20-110">Три хранимые процедуры с именами  **\<type_name\>_вставить**,  **\<type_name\>_обновить** и  **\<type_ имя\>_Удалить** (например, Blog_Insert, Blog_Update и Blog_Delete).</span><span class="sxs-lookup"><span data-stu-id="0ad20-110">Three stored procedures named **\<type_name\>_Insert**, **\<type_name\>_Update** and **\<type_name\>_Delete** (for example, Blog_Insert, Blog_Update and Blog_Delete).</span></span>  
- <span data-ttu-id="0ad20-111">Имена параметров соответствуют именам свойств.</span><span class="sxs-lookup"><span data-stu-id="0ad20-111">Parameter names correspond to the property names.</span></span>  
  > [!NOTE]
  > <span data-ttu-id="0ad20-112">Если вы используете HasColumnName() или атрибут столбца для переименования столбца для заданного свойства, это имя используется для параметров вместо имени свойства.</span><span class="sxs-lookup"><span data-stu-id="0ad20-112">If you use HasColumnName() or the Column attribute to rename the column for a given property then this name is used for parameters instead of the property name.</span></span>  
- <span data-ttu-id="0ad20-113">**Хранимая процедура insert** будет иметь параметр для каждого свойства, за исключением отмечаются как хранения созданных (идентификаторов или вычисляемых).</span><span class="sxs-lookup"><span data-stu-id="0ad20-113">**The insert stored procedure** will have a parameter for every property, except for those marked as store generated (identity or computed).</span></span> <span data-ttu-id="0ad20-114">Хранимая процедура должна возвращать результирующий набор со столбцом для каждого свойства хранения созданных.</span><span class="sxs-lookup"><span data-stu-id="0ad20-114">The stored procedure should return a result set with a column for each store generated property.</span></span>  
- <span data-ttu-id="0ad20-115">**Обновление хранимой процедуры** будет иметь параметр для каждого свойства, за исключением тех, отмеченные хранения созданных шаблон «Computed».</span><span class="sxs-lookup"><span data-stu-id="0ad20-115">**The update stored procedure** will have a parameter for every property, except for those marked with a store generated pattern of 'Computed'.</span></span> <span data-ttu-id="0ad20-116">Некоторые маркеры параллелизма требует параметра для исходного значения, см. в разделе *маркеры параллелизма* сведения в приведенном ниже разделе.</span><span class="sxs-lookup"><span data-stu-id="0ad20-116">Some concurrency tokens require a parameter for the original value, see the *Concurrency Tokens* section below for details.</span></span> <span data-ttu-id="0ad20-117">Хранимая процедура должна возвращать результирующий набор со столбцом для каждого вычисляемого свойства.</span><span class="sxs-lookup"><span data-stu-id="0ad20-117">The stored procedure should return a result set with a column for each computed property.</span></span>  
- <span data-ttu-id="0ad20-118">**Удаление хранимой процедуры** должен иметь параметр для значения ключа сущности (или несколько параметров, если сущность имеет составной ключ).</span><span class="sxs-lookup"><span data-stu-id="0ad20-118">**The delete stored procedure** should have a parameter for the key value of the entity (or multiple parameters if the entity has a composite key).</span></span> <span data-ttu-id="0ad20-119">Кроме того процедура delete также иметь параметры для любых внешних ключей в независимом сопоставлении для целевой таблицы (связей, у которых нет соответствующих свойств внешнего ключа объявлено в сущности).</span><span class="sxs-lookup"><span data-stu-id="0ad20-119">Additionally, the delete procedure should also have parameters for any independent association foreign keys on the target table (relationships that do not have corresponding foreign key properties declared in the entity).</span></span> <span data-ttu-id="0ad20-120">Некоторые маркеры параллелизма требует параметра для исходного значения, см. в разделе *маркеры параллелизма* сведения в приведенном ниже разделе.</span><span class="sxs-lookup"><span data-stu-id="0ad20-120">Some concurrency tokens require a parameter for the original value, see the *Concurrency Tokens* section below for details.</span></span>  

<span data-ttu-id="0ad20-121">Используя в качестве примера следующий класс:</span><span class="sxs-lookup"><span data-stu-id="0ad20-121">Using the following class as an example:</span></span>  

``` csharp
public class Blog  
{  
  public int BlogId { get; set; }  
  public string Name { get; set; }  
  public string Url { get; set; }  
}
```  

<span data-ttu-id="0ad20-122">По умолчанию, будет хранимых процедур:</span><span class="sxs-lookup"><span data-stu-id="0ad20-122">The default stored procedures would be:</span></span>  

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

### <a name="overriding-the-defaults"></a><span data-ttu-id="0ad20-123">Переопределение параметров по умолчанию</span><span class="sxs-lookup"><span data-stu-id="0ad20-123">Overriding the Defaults</span></span>  

<span data-ttu-id="0ad20-124">Вы можете переопределить частично или полностью тем, что было настроено по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="0ad20-124">You can override part or all of what was configured by default.</span></span>  

<span data-ttu-id="0ad20-125">Можно изменить имя одной или нескольких хранимых процедур.</span><span class="sxs-lookup"><span data-stu-id="0ad20-125">You can change the name of one or more stored procedures.</span></span> <span data-ttu-id="0ad20-126">В этом примере переименовывает хранимую процедуру обновления только.</span><span class="sxs-lookup"><span data-stu-id="0ad20-126">This example renames the update stored procedure only.</span></span>  

``` csharp
modelBuilder  
  .Entity<Blog>()  
  .MapToStoredProcedures(s =>  
    s.Update(u => u.HasName("modify_blog")));
```  

<span data-ttu-id="0ad20-127">В этом примере переименовывает всех трех хранимых процедур.</span><span class="sxs-lookup"><span data-stu-id="0ad20-127">This example renames all three stored procedures.</span></span>  

``` csharp
modelBuilder  
  .Entity<Blog>()  
  .MapToStoredProcedures(s =>  
    s.Update(u => u.HasName("modify_blog"))  
     .Delete(d => d.HasName("delete_blog"))  
     .Insert(i => i.HasName("insert_blog")));
```  

<span data-ttu-id="0ad20-128">В этих примерах вызовы соединяются друг с другом, но можно также использовать синтаксис блока лямбда-выражения.</span><span class="sxs-lookup"><span data-stu-id="0ad20-128">In these examples the calls are chained together, but you can also use lambda block syntax.</span></span>  

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

<span data-ttu-id="0ad20-129">В этом примере переименовывает параметр для свойства BlogId хранимая процедура update.</span><span class="sxs-lookup"><span data-stu-id="0ad20-129">This example renames the parameter for the BlogId property on the update stored procedure.</span></span>  

``` csharp
modelBuilder  
  .Entity<Blog>()  
  .MapToStoredProcedures(s =>  
    s.Update(u => u.Parameter(b => b.BlogId, "blog_id")));
```  

<span data-ttu-id="0ad20-130">Эти вызовы являются все к цепочкам и композицию.</span><span class="sxs-lookup"><span data-stu-id="0ad20-130">These calls are all chainable and composable.</span></span> <span data-ttu-id="0ad20-131">Ниже приведен пример, который переименовывает все три хранимые процедуры и их параметрах.</span><span class="sxs-lookup"><span data-stu-id="0ad20-131">Here is an example that renames all three stored procedures and their parameters.</span></span>  

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

<span data-ttu-id="0ad20-132">Можно также изменить имена столбцов в результирующем наборе, который содержит значения, сформированные базой данных.</span><span class="sxs-lookup"><span data-stu-id="0ad20-132">You can also change the name of the columns in the result set that contains database generated values.</span></span>  

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

## <a name="relationships-without-a-foreign-key-in-the-class-independent-associations"></a><span data-ttu-id="0ad20-133">Связи без внешнего ключа в классе (независимых сопоставлений)</span><span class="sxs-lookup"><span data-stu-id="0ad20-133">Relationships Without a Foreign Key in the Class (Independent Associations)</span></span>  

<span data-ttu-id="0ad20-134">Если свойство внешнего ключа включен в определение класса, соответствующего параметра могут быть переименованы в так же, как любое другое свойство.</span><span class="sxs-lookup"><span data-stu-id="0ad20-134">When a foreign key property is included in the class definition, the corresponding parameter can be renamed in the same way as any other property.</span></span> <span data-ttu-id="0ad20-135">Если связь существует без свойство внешнего ключа в классе, имя параметра по умолчанию —  **\<navigation_property_name\>_\<primary_key_name\>**.</span><span class="sxs-lookup"><span data-stu-id="0ad20-135">When a relationship exists without a foreign key property in the class, the default parameter name is **\<navigation_property_name\>_\<primary_key_name\>**.</span></span>  

<span data-ttu-id="0ad20-136">Например следующие определения класса может привести параметр Blog_BlogId ожидаемая в хранимые процедуры для вставки и обновления записей.</span><span class="sxs-lookup"><span data-stu-id="0ad20-136">For example, the following class definitions would result in a Blog_BlogId parameter being expected in the stored procedures to insert and update Posts.</span></span>  

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

### <a name="overriding-the-defaults"></a><span data-ttu-id="0ad20-137">Переопределение параметров по умолчанию</span><span class="sxs-lookup"><span data-stu-id="0ad20-137">Overriding the Defaults</span></span>  

<span data-ttu-id="0ad20-138">Вы можете изменить параметры для внешних ключей, которые не находятся в классе, указав путь к свойство первичного ключа в метод параметр.</span><span class="sxs-lookup"><span data-stu-id="0ad20-138">You can change parameters for foreign keys that are not included in the class by supplying the path to the primary key property to the Parameter method.</span></span>  

``` csharp
modelBuilder
  .Entity<Post>()  
  .MapToStoredProcedures(s =>  
    s.Insert(i => i.Parameter(p => p.Blog.BlogId, "blog_id")));
```  

<span data-ttu-id="0ad20-139">Если у вас нет свойства навигации на зависимой сущности (например)</span><span class="sxs-lookup"><span data-stu-id="0ad20-139">If you don’t have a navigation property on the dependent entity (i.e</span></span> <span data-ttu-id="0ad20-140">свойство не Post.Blog) методу связи можно использовать для идентификации на другом конце связи, а затем настройте параметры, соответствующие каждому из свойств ключа.</span><span class="sxs-lookup"><span data-stu-id="0ad20-140">no Post.Blog property) then you can use the Association method to identify the other end of the relationship and then configure the parameters that correspond to each of the key property(s).</span></span>  

``` csharp
modelBuilder
  .Entity<Post>()  
  .MapToStoredProcedures(s =>  
    s.Insert(i => i.Navigation<Blog>(  
      b => b.Posts,  
      c => c.Parameter(b => b.BlogId, "blog_id"))));
```  

## <a name="concurrency-tokens"></a><span data-ttu-id="0ad20-141">Маркеры параллелизма</span><span class="sxs-lookup"><span data-stu-id="0ad20-141">Concurrency Tokens</span></span>  

<span data-ttu-id="0ad20-142">Обновления и удаления хранимых процедур может понадобиться иметь дело с параллелизмом:</span><span class="sxs-lookup"><span data-stu-id="0ad20-142">Update and delete stored procedures may also need to deal with concurrency:</span></span>  

- <span data-ttu-id="0ad20-143">Если сущность содержит маркеры параллелизма, хранимая процедура может быть Необязательный выходной параметр, который возвращает количество обновленные или удаленные строки (обработано строк).</span><span class="sxs-lookup"><span data-stu-id="0ad20-143">If the entity contains concurrency tokens, the stored procedure can optionally have an output parameter that returns the number of rows updated/deleted (rows affected).</span></span> <span data-ttu-id="0ad20-144">Такие параметры должны быть настроены с помощью метода RowsAffectedParameter.</span><span class="sxs-lookup"><span data-stu-id="0ad20-144">Such a parameter must be configured using the RowsAffectedParameter method.</span></span>  
<span data-ttu-id="0ad20-145">По умолчанию EF использует возвращаемое значение из ExecuteNonQuery, чтобы определить, сколько строк затронуто.</span><span class="sxs-lookup"><span data-stu-id="0ad20-145">By default EF uses the return value from ExecuteNonQuery to determine how many rows were affected.</span></span> <span data-ttu-id="0ad20-146">Указание параметра выходные данные строк, затронутых полезно, если выполнить какую-либо логику в sproc, приведет к возвращаемому значению ExecuteNonQuery, неверный (с точки зрения платформы EF) в конце выполнения.</span><span class="sxs-lookup"><span data-stu-id="0ad20-146">Specifying a rows affected output parameter is useful if you perform any logic in your sproc that would result in the return value of ExecuteNonQuery being incorrect (from EF's perspective) at the end of execution.</span></span>  
- <span data-ttu-id="0ad20-147">Для каждого параллелизма существует токен будет параметр с именем  **\<property_name\>_Original** (например, Timestamp_Original).</span><span class="sxs-lookup"><span data-stu-id="0ad20-147">For each concurrency token there will be a parameter named **\<property_name\>_Original** (for example, Timestamp_Original ).</span></span> <span data-ttu-id="0ad20-148">Он будет передаваться исходное значение этого свойства — значение, при запросе из базы данных.</span><span class="sxs-lookup"><span data-stu-id="0ad20-148">This will be passed the original value of this property – the value when queried from the database.</span></span>  
    - <span data-ttu-id="0ad20-149">Маркеры параллелизма, которые вычисляются по базе данных — например отметки времени — будет иметь только параметром исходное значение.</span><span class="sxs-lookup"><span data-stu-id="0ad20-149">Concurrency tokens that are computed by the database – such as timestamps – will only have an original value parameter.</span></span>  
    - <span data-ttu-id="0ad20-150">Вычисленный отличные от свойств, заданных как маркеры параллелизма также получат новое значение параметра в процедуре обновления.</span><span class="sxs-lookup"><span data-stu-id="0ad20-150">Non-computed properties that are set as concurrency tokens will also have a parameter for the new value in the update procedure.</span></span> <span data-ttu-id="0ad20-151">При этом используется соглашения об именовании, описанные для новых значений.</span><span class="sxs-lookup"><span data-stu-id="0ad20-151">This uses the naming conventions already discussed for new values.</span></span> <span data-ttu-id="0ad20-152">Примером такого маркера объект с помощью URL-адрес блога как маркер параллелизма, новое значение является обязательной, поскольку это могут быть обновлены на новое значение в коде (в отличие от маркер отметки времени, который обновляется только в базе данных).</span><span class="sxs-lookup"><span data-stu-id="0ad20-152">An example of such a token would be using a Blog's URL as a concurrency token, the new value is required because this can be updated to a new value by your code (unlike a Timestamp token which is only updated by the database).</span></span>  

<span data-ttu-id="0ad20-153">Ниже приведен пример класса и хранимая процедура update с маркером параллелизма отметки времени.</span><span class="sxs-lookup"><span data-stu-id="0ad20-153">This is an example class and update stored procedure with a timestamp concurrency token.</span></span>  

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

<span data-ttu-id="0ad20-154">Ниже приведен пример класса и хранимая процедура update с маркером невычисляемый параллелизма.</span><span class="sxs-lookup"><span data-stu-id="0ad20-154">Here is an example class and update stored procedure with non-computed concurrency token.</span></span>  

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

### <a name="overriding-the-defaults"></a><span data-ttu-id="0ad20-155">Переопределение параметров по умолчанию</span><span class="sxs-lookup"><span data-stu-id="0ad20-155">Overriding the Defaults</span></span>  

<span data-ttu-id="0ad20-156">При необходимости можно ввести параметр числа затронутых строк.</span><span class="sxs-lookup"><span data-stu-id="0ad20-156">You can optionally introduce a rows affected parameter.</span></span>  

``` csharp
modelBuilder  
  .Entity<Blog>()  
  .MapToStoredProcedures(s =>  
    s.Update(u => u.RowsAffectedParameter("rows_affected")));
```  

<span data-ttu-id="0ad20-157">Для базы данных, рассчитанное параллелизма маркеры — где передается только исходное значение — можно просто использовать стандартный параметр переименование механизм для переименования параметра для исходного значения.</span><span class="sxs-lookup"><span data-stu-id="0ad20-157">For database computed concurrency tokens – where only the original value is passed – you can just use the standard parameter renaming mechanism to rename the parameter for the original value.</span></span>  

``` csharp
modelBuilder  
  .Entity<Blog>()  
  .MapToStoredProcedures(s =>  
    s.Update(u => u.Parameter(b => b.Timestamp, "blog_timestamp")));
```  

<span data-ttu-id="0ad20-158">Для маркеров параллелизма невычисляемый — где как исходное и новое значение передаются — можно использовать перегрузку параметр, который позволяет указать имя для каждого параметра.</span><span class="sxs-lookup"><span data-stu-id="0ad20-158">For non-computed concurrency tokens – where both the original and new value are passed – you can use an overload of Parameter that allows you to supply a name for each parameter.</span></span>  

``` csharp
modelBuilder
 .Entity<Blog>()
 .MapToStoredProcedures(s => s.Update(u => u.Parameter(b => b.Url, "blog_url", "blog_original_url")));
```  

## <a name="many-to-many-relationships"></a><span data-ttu-id="0ad20-159">Отношения &quot;многие ко многим&quot;</span><span class="sxs-lookup"><span data-stu-id="0ad20-159">Many to Many Relationships</span></span>  

<span data-ttu-id="0ad20-160">Мы будем использовать следующие классы в качестве примера в этом разделе.</span><span class="sxs-lookup"><span data-stu-id="0ad20-160">We’ll use the following classes as an example in this section.</span></span>  

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

<span data-ttu-id="0ad20-161">Многие к много связей могут сопоставляться с хранимыми процедурами, используя следующий синтаксис.</span><span class="sxs-lookup"><span data-stu-id="0ad20-161">Many to many relationships can be mapped to stored procedures with the following syntax.</span></span>  

``` csharp
modelBuilder  
  .Entity<Post>()  
  .HasMany(p => p.Tags)  
  .WithMany(t => t.Posts)  
  .MapToStoredProcedures();
```  

<span data-ttu-id="0ad20-162">Если указано полностью выполнили настройку по умолчанию используется хранимая процедура следующего вида.</span><span class="sxs-lookup"><span data-stu-id="0ad20-162">If no other configuration is supplied then the following stored procedure shape is used by default.</span></span>  

- <span data-ttu-id="0ad20-163">Две хранимые процедуры с именами  **\<type_one\>\<type_two\>_вставить** и  **\<type_one\>\<type_two \>_Удалить** (например, PostTag_Insert и PostTag_Delete).</span><span class="sxs-lookup"><span data-stu-id="0ad20-163">Two stored procedures named **\<type_one\>\<type_two\>_Insert** and **\<type_one\>\<type_two\>_Delete** (for example, PostTag_Insert and PostTag_Delete).</span></span>  
- <span data-ttu-id="0ad20-164">Параметры будут иметь значения ключа для каждого типа.</span><span class="sxs-lookup"><span data-stu-id="0ad20-164">The parameters will be the key value(s) for each type.</span></span> <span data-ttu-id="0ad20-165">Имя каждого параметра **\<type_name\>_\<property_name\>** (например, Post_PostId и Tag_TagId).</span><span class="sxs-lookup"><span data-stu-id="0ad20-165">The name of each parameter being **\<type_name\>_\<property_name\>** (for example, Post_PostId and Tag_TagId).</span></span>

<span data-ttu-id="0ad20-166">Вот пример вставки и обновления хранимых процедур.</span><span class="sxs-lookup"><span data-stu-id="0ad20-166">Here are example insert and update stored procedures.</span></span>  

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

### <a name="overriding-the-defaults"></a><span data-ttu-id="0ad20-167">Переопределение параметров по умолчанию</span><span class="sxs-lookup"><span data-stu-id="0ad20-167">Overriding the Defaults</span></span>  

<span data-ttu-id="0ad20-168">Имена процедур и параметров можно настроить в так же, как сущности хранимой процедуры.</span><span class="sxs-lookup"><span data-stu-id="0ad20-168">The procedure and parameter names can be configured in a similar way to entity stored procedures.</span></span>  

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
