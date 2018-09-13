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
# <a name="code-first-insert-update-and-delete-stored-procedures"></a>Код первой вставки, обновления и удаления хранимых процедур
> [!NOTE]
> **Только в EF6 и более поздних версиях**. Функции, API и другие возможности, описанные на этой странице, появились в Entity Framework 6. При использовании более ранней версии могут быть неприменимы некоторые или все сведения.  

По умолчанию Code First настроит все сущности, чтобы выполнить операцию вставки, обновления и удаления команд с помощью прямого доступа к таблицам. Начиная с EF6 можно настроить модель данных Code First с помощью хранимых процедур для некоторых или всех сущностей в модели.  

## <a name="basic-entity-mapping"></a>Основные сущности сопоставления  

Можно явно задано использование хранимых процедур для вставки, обновления и удаления с помощью Fluent API.  

``` csharp
modelBuilder
  .Entity<Blog>()
  .MapToStoredProcedures();
```  

Это приведет к неспособности Code First, использование некоторых соглашений для создания Прогнозируемая форма хранимые процедуры в базе данных.  

- Три хранимые процедуры с именами  **\<type_name\>_вставить**,  **\<type_name\>_обновить** и  **\<type_ имя\>_Удалить** (например, Blog_Insert, Blog_Update и Blog_Delete).  
- Имена параметров соответствуют именам свойств.  
  > [!NOTE]
  > Если вы используете HasColumnName() или атрибут столбца для переименования столбца для заданного свойства, это имя используется для параметров вместо имени свойства.  
- **Хранимая процедура insert** будет иметь параметр для каждого свойства, за исключением отмечаются как хранения созданных (идентификаторов или вычисляемых). Хранимая процедура должна возвращать результирующий набор со столбцом для каждого свойства хранения созданных.  
- **Обновление хранимой процедуры** будет иметь параметр для каждого свойства, за исключением тех, отмеченные хранения созданных шаблон «Computed». Некоторые маркеры параллелизма требует параметра для исходного значения, см. в разделе *маркеры параллелизма* сведения в приведенном ниже разделе. Хранимая процедура должна возвращать результирующий набор со столбцом для каждого вычисляемого свойства.  
- **Удаление хранимой процедуры** должен иметь параметр для значения ключа сущности (или несколько параметров, если сущность имеет составной ключ). Кроме того процедура delete также иметь параметры для любых внешних ключей в независимом сопоставлении для целевой таблицы (связей, у которых нет соответствующих свойств внешнего ключа объявлено в сущности). Некоторые маркеры параллелизма требует параметра для исходного значения, см. в разделе *маркеры параллелизма* сведения в приведенном ниже разделе.  

Используя в качестве примера следующий класс:  

``` csharp
public class Blog  
{  
  public int BlogId { get; set; }  
  public string Name { get; set; }  
  public string Url { get; set; }  
}
```  

По умолчанию, будет хранимых процедур:  

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

### <a name="overriding-the-defaults"></a>Переопределение параметров по умолчанию  

Вы можете переопределить частично или полностью тем, что было настроено по умолчанию.  

Можно изменить имя одной или нескольких хранимых процедур. В этом примере переименовывает хранимую процедуру обновления только.  

``` csharp
modelBuilder  
  .Entity<Blog>()  
  .MapToStoredProcedures(s =>  
    s.Update(u => u.HasName("modify_blog")));
```  

В этом примере переименовывает всех трех хранимых процедур.  

``` csharp
modelBuilder  
  .Entity<Blog>()  
  .MapToStoredProcedures(s =>  
    s.Update(u => u.HasName("modify_blog"))  
     .Delete(d => d.HasName("delete_blog"))  
     .Insert(i => i.HasName("insert_blog")));
```  

В этих примерах вызовы соединяются друг с другом, но можно также использовать синтаксис блока лямбда-выражения.  

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

В этом примере переименовывает параметр для свойства BlogId хранимая процедура update.  

``` csharp
modelBuilder  
  .Entity<Blog>()  
  .MapToStoredProcedures(s =>  
    s.Update(u => u.Parameter(b => b.BlogId, "blog_id")));
```  

Эти вызовы являются все к цепочкам и композицию. Ниже приведен пример, который переименовывает все три хранимые процедуры и их параметрах.  

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

Можно также изменить имена столбцов в результирующем наборе, который содержит значения, сформированные базой данных.  

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

## <a name="relationships-without-a-foreign-key-in-the-class-independent-associations"></a>Связи без внешнего ключа в классе (независимых сопоставлений)  

Если свойство внешнего ключа включен в определение класса, соответствующего параметра могут быть переименованы в так же, как любое другое свойство. Если связь существует без свойство внешнего ключа в классе, имя параметра по умолчанию —  **\<navigation_property_name\>_\<primary_key_name\>**.  

Например следующие определения класса может привести параметр Blog_BlogId ожидаемая в хранимые процедуры для вставки и обновления записей.  

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

### <a name="overriding-the-defaults"></a>Переопределение параметров по умолчанию  

Вы можете изменить параметры для внешних ключей, которые не находятся в классе, указав путь к свойство первичного ключа в метод параметр.  

``` csharp
modelBuilder
  .Entity<Post>()  
  .MapToStoredProcedures(s =>  
    s.Insert(i => i.Parameter(p => p.Blog.BlogId, "blog_id")));
```  

Если у вас нет свойства навигации на зависимой сущности (например) свойство не Post.Blog) методу связи можно использовать для идентификации на другом конце связи, а затем настройте параметры, соответствующие каждому из свойств ключа.  

``` csharp
modelBuilder
  .Entity<Post>()  
  .MapToStoredProcedures(s =>  
    s.Insert(i => i.Navigation<Blog>(  
      b => b.Posts,  
      c => c.Parameter(b => b.BlogId, "blog_id"))));
```  

## <a name="concurrency-tokens"></a>Маркеры параллелизма  

Обновления и удаления хранимых процедур может понадобиться иметь дело с параллелизмом:  

- Если сущность содержит маркеры параллелизма, хранимая процедура может быть Необязательный выходной параметр, который возвращает количество обновленные или удаленные строки (обработано строк). Такие параметры должны быть настроены с помощью метода RowsAffectedParameter.  
По умолчанию EF использует возвращаемое значение из ExecuteNonQuery, чтобы определить, сколько строк затронуто. Указание параметра выходные данные строк, затронутых полезно, если выполнить какую-либо логику в sproc, приведет к возвращаемому значению ExecuteNonQuery, неверный (с точки зрения платформы EF) в конце выполнения.  
- Для каждого параллелизма существует токен будет параметр с именем  **\<property_name\>_Original** (например, Timestamp_Original). Он будет передаваться исходное значение этого свойства — значение, при запросе из базы данных.  
    - Маркеры параллелизма, которые вычисляются по базе данных — например отметки времени — будет иметь только параметром исходное значение.  
    - Вычисленный отличные от свойств, заданных как маркеры параллелизма также получат новое значение параметра в процедуре обновления. При этом используется соглашения об именовании, описанные для новых значений. Примером такого маркера объект с помощью URL-адрес блога как маркер параллелизма, новое значение является обязательной, поскольку это могут быть обновлены на новое значение в коде (в отличие от маркер отметки времени, который обновляется только в базе данных).  

Ниже приведен пример класса и хранимая процедура update с маркером параллелизма отметки времени.  

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

Ниже приведен пример класса и хранимая процедура update с маркером невычисляемый параллелизма.  

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

### <a name="overriding-the-defaults"></a>Переопределение параметров по умолчанию  

При необходимости можно ввести параметр числа затронутых строк.  

``` csharp
modelBuilder  
  .Entity<Blog>()  
  .MapToStoredProcedures(s =>  
    s.Update(u => u.RowsAffectedParameter("rows_affected")));
```  

Для базы данных, рассчитанное параллелизма маркеры — где передается только исходное значение — можно просто использовать стандартный параметр переименование механизм для переименования параметра для исходного значения.  

``` csharp
modelBuilder  
  .Entity<Blog>()  
  .MapToStoredProcedures(s =>  
    s.Update(u => u.Parameter(b => b.Timestamp, "blog_timestamp")));
```  

Для маркеров параллелизма невычисляемый — где как исходное и новое значение передаются — можно использовать перегрузку параметр, который позволяет указать имя для каждого параметра.  

``` csharp
modelBuilder
 .Entity<Blog>()
 .MapToStoredProcedures(s => s.Update(u => u.Parameter(b => b.Url, "blog_url", "blog_original_url")));
```  

## <a name="many-to-many-relationships"></a>Отношения &quot;многие ко многим&quot;  

Мы будем использовать следующие классы в качестве примера в этом разделе.  

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

Многие к много связей могут сопоставляться с хранимыми процедурами, используя следующий синтаксис.  

``` csharp
modelBuilder  
  .Entity<Post>()  
  .HasMany(p => p.Tags)  
  .WithMany(t => t.Posts)  
  .MapToStoredProcedures();
```  

Если указано полностью выполнили настройку по умолчанию используется хранимая процедура следующего вида.  

- Две хранимые процедуры с именами  **\<type_one\>\<type_two\>_вставить** и  **\<type_one\>\<type_two \>_Удалить** (например, PostTag_Insert и PostTag_Delete).  
- Параметры будут иметь значения ключа для каждого типа. Имя каждого параметра **\<type_name\>_\<property_name\>** (например, Post_PostId и Tag_TagId).

Вот пример вставки и обновления хранимых процедур.  

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

### <a name="overriding-the-defaults"></a>Переопределение параметров по умолчанию  

Имена процедур и параметров можно настроить в так же, как сущности хранимой процедуры.  

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
