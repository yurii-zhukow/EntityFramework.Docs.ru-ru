---
title: Code First вставки, обновления и удаления хранимых процедур — EF6
description: Code First вставки, обновления и удаления хранимых процедур в Entity Framework 6
author: divega
ms.date: 10/23/2016
uid: ef6/modeling/code-first/fluent/cud-stored-procedures
ms.openlocfilehash: 2bf2b3d343b70d80c5c56922a31f9b1ece54f60a
ms.sourcegitcommit: abda0872f86eefeca191a9a11bfca976bc14468b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/14/2020
ms.locfileid: "90073942"
---
# <a name="code-first-insert-update-and-delete-stored-procedures"></a>Code First вставки, обновления и удаления хранимых процедур
> [!NOTE]
> **Только в EF6 и более поздних версиях**. Функции, API и другие возможности, описанные на этой странице, появились в Entity Framework 6. При использовании более ранней версии могут быть неприменимы некоторые или все сведения.  

По умолчанию Code First настраивает все сущности для выполнения команд INSERT, Update и DELETE с прямым доступом к таблицам. Начиная с EF6 можно настроить модель Code First для использования хранимых процедур для некоторых или всех сущностей в модели.  

## <a name="basic-entity-mapping"></a>Базовое сопоставление сущностей  

Вы можете использовать хранимые процедуры для вставки, обновления и удаления с помощью API Fluent.  

``` csharp
modelBuilder
  .Entity<Blog>()
  .MapToStoredProcedures();
```  

Это приведет к тому, что Code First будет использовать некоторые соглашения для создания ожидаемой формы хранимых процедур в базе данных.  

- Три хранимые процедуры с именами ** \<type_name\> _Insert**, ** \<type_name\> _Update** и ** \<type_name\> _Delete** (например, Blog_Insert, Blog_Update и Blog_Delete).  
- Имена параметров соответствуют именам свойств.  
  > [!NOTE]
  > Если вы используете Хасколумннаме () или атрибут Column для переименования столбца для данного свойства, то это имя используется для параметров, а не для имени свойства.  
- **Хранимая процедура INSERT** будет иметь параметр для каждого свойства, за исключением тех, которые помечены как созданные хранилищем (Identity или computeed). Хранимая процедура должна возвращать результирующий набор со столбцом для каждого свойства, созданного хранилищем.  
- **Хранимая процедура UPDATE** будет иметь параметр для каждого свойства, за исключением тех, которые помечены шаблоном, сформированным хранилищем "rebyd". Некоторым маркерам параллелизма требуется параметр для исходного значения. Дополнительные сведения см. в разделе " *маркеры параллелизма* " ниже. Хранимая процедура должна возвращать результирующий набор со столбцом для каждого вычисляемого свойства.  
- **Хранимая процедура DELETE** должна иметь параметр для значения ключа сущности (или несколько параметров, если у сущности есть составной ключ). Кроме того, процедура удаления также должна иметь параметры для любых независимых внешних ключей ассоциации в целевой таблице (связи, у которых нет соответствующих свойств внешнего ключа, объявленных в сущности). Некоторым маркерам параллелизма требуется параметр для исходного значения. Дополнительные сведения см. в разделе " *маркеры параллелизма* " ниже.  

В качестве примера используйте следующий класс:  

``` csharp
public class Blog  
{  
  public int BlogId { get; set; }  
  public string Name { get; set; }  
  public string Url { get; set; }  
}
```  

Хранимые процедуры по умолчанию будут выглядеть следующим образом:  

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

### <a name="overriding-the-defaults"></a>Переопределение значений по умолчанию  

Часть, настроенную по умолчанию, можно переопределить.  

Можно изменить имя одной или нескольких хранимых процедур. В этом примере переименовывается только хранимая процедура UPDATE.  

``` csharp
modelBuilder  
  .Entity<Blog>()  
  .MapToStoredProcedures(s =>  
    s.Update(u => u.HasName("modify_blog")));
```  

В этом примере переименование всех трех хранимых процедур.  

``` csharp
modelBuilder  
  .Entity<Blog>()  
  .MapToStoredProcedures(s =>  
    s.Update(u => u.HasName("modify_blog"))  
     .Delete(d => d.HasName("delete_blog"))  
     .Insert(i => i.HasName("insert_blog")));
```  

В этих примерах вызовы объединяются в цепочки, но можно также использовать синтаксис лямбда-блоков.  

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

В этом примере переименовывается параметр для свойства Блогид в хранимой процедуре Update.  

``` csharp
modelBuilder  
  .Entity<Blog>()  
  .MapToStoredProcedures(s =>  
    s.Update(u => u.Parameter(b => b.BlogId, "blog_id")));
```  

Все эти вызовы являются связанными и взаимосвязанными. Ниже приведен пример, в котором переименование всех трех хранимых процедур и их параметров.  

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

Также можно изменить имена столбцов в результирующем наборе, содержащем значения, сформированные базой данных.  

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

## <a name="relationships-without-a-foreign-key-in-the-class-independent-associations"></a>Связи без внешнего ключа в классе (независимые ассоциации)  

Если свойство внешнего ключа включено в определение класса, соответствующий параметр можно переименовать так же, как и любое другое свойство. Если отношение существует без свойства внешнего ключа в классе, имя параметра по умолчанию — ** \<navigation_property_name\> _ \<primary_key_name\> **.  

Например, следующие определения классов приведут к тому, что в хранимых процедурах для вставки и обновления записей будет Blog_BlogId ожидаемый параметр.  

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

### <a name="overriding-the-defaults"></a>Переопределение значений по умолчанию  

Параметры внешних ключей, не включаемых в класс, можно изменить, указав путь к свойству первичного ключа в методе Parameter.  

``` csharp
modelBuilder
  .Entity<Post>()  
  .MapToStoredProcedures(s =>  
    s.Insert(i => i.Parameter(p => p.Blog.BlogId, "blog_id")));
```  

Если у вас нет свойства навигации в зависимой сущности (т. е. нет свойства POST. blog. Затем можно использовать метод Association, чтобы определить другой конец связи, а затем настроить параметры, соответствующие каждому из ключевых свойств.  

``` csharp
modelBuilder
  .Entity<Post>()  
  .MapToStoredProcedures(s =>  
    s.Insert(i => i.Navigation<Blog>(  
      b => b.Posts,  
      c => c.Parameter(b => b.BlogId, "blog_id"))));
```  

## <a name="concurrency-tokens"></a>Маркеры параллелизма  

Для выполнения хранимых процедур Update и Delete также может потребоваться обработка параллелизма.  

- Если сущность содержит маркеры параллелизма, то хранимая процедура может дополнительно иметь выходной параметр, возвращающий количество обновленных или удаленных строк (затронутых строк). Такой параметр должен быть настроен с помощью метода Ровсаффектедпараметер.  
По умолчанию EF использует значение, возвращаемое методом ExecuteNonQuery, для определения количества затронутых строк. Указание выходного параметра ROWS полезно, если вы выполняете любую логику в sproc, которая приведет к неправильному возвращаемому значению ExecuteNonQuery (от перспективы EF) в конце выполнения.  
- Для каждого маркера параллелизма будет указан параметр с именем ** \<property_name\> _Original** (например, Timestamp_Original). Ему будет передано исходное значение этого свойства — значение при запросе из базы данных.  
    - Маркеры параллелизма, которые вычисляются базой данных (например, метки времени), будут иметь только исходный параметр значения.  
    - Невычисленные свойства, заданные как маркеры параллелизма, также будут иметь параметр для нового значения в процедуре обновления. При этом используются соглашения об именовании, уже описанные для новых значений. Примером такого маркера будет использование URL-адреса блога в качестве маркера параллелизма. новое значение является обязательным, поскольку его можно обновить до нового значения в коде (в отличие от маркера метки времени, который обновляется только базой данных).  

Это пример класса и хранимая процедура UPDATE с маркером параллелизма timestamp.  

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

Ниже приведен пример класса и хранимая процедура UPDATE с невычисленным маркером параллелизма.  

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

### <a name="overriding-the-defaults"></a>Переопределение значений по умолчанию  

При необходимости можно ввести параметр, затронутый строкой.  

``` csharp
modelBuilder  
  .Entity<Blog>()  
  .MapToStoredProcedures(s =>  
    s.Update(u => u.RowsAffectedParameter("rows_affected")));
```  

Для рассчитанных маркеров параллелизма базы данных — где передано только исходное значение — можно просто использовать стандартный механизм переименования параметров, чтобы переименовать параметр для исходного значения.  

``` csharp
modelBuilder  
  .Entity<Blog>()  
  .MapToStoredProcedures(s =>  
    s.Update(u => u.Parameter(b => b.Timestamp, "blog_timestamp")));
```  

Для невычисленных маркеров параллелизма, где передаются исходное и новое значения — можно использовать перегрузку параметра, которая позволяет указать имя для каждого параметра.  

``` csharp
modelBuilder
 .Entity<Blog>()
 .MapToStoredProcedures(s => s.Update(u => u.Parameter(b => b.Url, "blog_url", "blog_original_url")));
```  

## <a name="many-to-many-relationships"></a>Отношения &quot;многие ко многим&quot;  

В этом разделе мы будем использовать следующие классы.  

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

Многие ко многим связям можно сопоставить с хранимыми процедурами с помощью следующего синтаксиса.  

``` csharp
modelBuilder  
  .Entity<Post>()  
  .HasMany(p => p.Tags)  
  .WithMany(t => t.Posts)  
  .MapToStoredProcedures();
```  

Если никакая другая конфигурация не указана, по умолчанию используется следующая фигура хранимой процедуры.  

- Две хранимые процедуры с именами ** \<type_one\> \<type_two\> _Insert** и ** \<type_one\> \<type_two\> _Delete** (например, PostTag_Insert и PostTag_Delete).  
- Параметры будут ключевыми значениями для каждого типа. Имя каждого параметра ** \<type_name\> _ \<property_name\> ** (например, Post_PostId и Tag_TagId).

Ниже приведены примеры хранимых процедур INSERT и Update.  

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

### <a name="overriding-the-defaults"></a>Переопределение значений по умолчанию  

Имена процедур и параметров можно настроить аналогичным образом для хранимых процедур сущностей.  

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
