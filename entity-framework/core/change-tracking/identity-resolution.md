---
title: Разрешение идентификации — EF Core
description: Разрешение нескольких экземпляров сущности в один экземпляр с использованием значений первичного ключа
author: ajcvickers
ms.date: 12/30/2020
uid: core/change-tracking/identity-resolution
ms.openlocfilehash: 24b2fbeea5f740dd2830676bfe8a49720c2b86a9
ms.sourcegitcommit: 4798ab8d04c1fdbe6dd204d94d770fcbf309d09b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/11/2021
ms.locfileid: "103024463"
---
# <a name="identity-resolution-in-ef-core"></a>Разрешение идентификаторов в EF Core

<xref:Microsoft.EntityFrameworkCore.DbContext>Может отслеживаниь только одного экземпляра сущности с любым значением первичного ключа. Это означает, что несколько экземпляров сущности с одинаковым значением ключа должны быть разрешены в один экземпляр. Это называется разрешением идентификации. Разрешение идентификации гарантирует, Entity Framework Core (EF Core) отслеживает единообразные графы без неоднозначности относительно связей или значений свойств сущностей.

> [!TIP]
> В этом документе предполагается, что состояния сущностей и основы отслеживания изменений EF Core понятны. Дополнительные сведения по этим темам см. [в разделе Отслеживание изменений в EF Core](xref:core/change-tracking/index) .

> [!TIP]
> Вы можете запустить и отладить весь код, используемый в этой документации, [скачав пример кода из GitHub](https://github.com/dotnet/EntityFramework.Docs/tree/main/samples/core/ChangeTracking/IdentityResolutionInEFCore).

## <a name="introduction"></a>Введение

Следующий код запрашивает сущность, а затем пытается присоединить другой экземпляр с тем же значением первичного ключа:

<!--
            using var context = new BlogsContext();

            var blogA = context.Blogs.Single(e => e.Id == 1);
            var blogB = new Blog { Id = 1, Name = ".NET Blog (All new!)" };

            try
            {
                context.Update(blogB); // This will throw
            }
            catch (Exception e)
            {
                Console.WriteLine(e);
            }
-->
[!code-csharp[Identity_Resolution_in_EF_Core_1](../../../samples/core/ChangeTracking/IdentityResolutionInEFCore/IdentityResolutionSamples.cs?name=Identity_Resolution_in_EF_Core_1)]

Выполнение этого кода приводит к возникновению следующего исключения:

> System. InvalidOperationException: экземпляр типа сущности "Blog" не может быть отслеживаем, так как уже отслеживается другой экземпляр со значением ключа "{ID: 1}". При присоединении существующих сущностей убедитесь, что присоединен только один экземпляр сущности с заданным значением ключа.

EF Core требуется один экземпляр, так как:

- Значения свойств могут различаться для нескольких экземпляров. При обновлении базы данных EF Core необходимо выяснить, какие значения свойств следует использовать.
- Связи с другими сущностями могут различаться между несколькими экземплярами. Например, "блог" может быть связан с другой коллекцией записей, чем "Блогб".

Приведенное выше исключение часто встречается в следующих ситуациях:

- При попытке обновить сущность
- При попытке отслеживании сериализованного графа сущностей
- Не удается задать значение ключа, которое не создается автоматически
- При повторном использовании экземпляра DbContext для нескольких единиц работы

Каждая из этих ситуаций обсуждается в следующих разделах.

## <a name="updating-an-entity"></a>Обновление сущности

Существует несколько различных подходов к обновлению сущности новыми значениями, как описано в [Отслеживание изменений в EF Core](xref:core/change-tracking/index) и [явном отслеживании сущностей](xref:core/change-tracking/explicit-tracking). Эти подходы описаны ниже в контексте разрешения идентификации. Важно отметить, что каждый из подходов использует либо запрос, либо вызов одного из `Update` или `Attach` , но **_никогда не оба_**.

### <a name="call-update"></a>Вызов обновления

Часто обновляемая сущность не поступает из запроса к DbContext, который мы будем использовать для SaveChanges. Например, в веб-приложении экземпляр сущности может быть создан на основе информации в запросе POST. Самый простой способ обработать это — использовать <xref:Microsoft.EntityFrameworkCore.DbContext.Update%2A?displayProperty=nameWithType> или <xref:Microsoft.EntityFrameworkCore.DbSet%601.Update%2A?displayProperty=nameWithType> . Например:

<!--
    public static void UpdateFromHttpPost1(Blog blog)
    {
        using var context = new BlogsContext();

        context.Update(blog);

        context.SaveChanges();
    }
-->
[!code-csharp[Updating_an_entity_1](../../../samples/core/ChangeTracking/IdentityResolutionInEFCore/IdentityResolutionSamples.cs?name=Updating_an_entity_1)]

В данном случае:

- Создается только один экземпляр сущности.
- Экземпляр сущности **не** запрашивается из базы данных в процессе выполнения обновления.
- Все значения свойств будут обновлены в базе данных независимо от того, изменились ли они.
- Выполняется цикл обработки одной базы данных.

### <a name="query-then-apply-changes"></a>Запрос, затем применить изменения

Обычно неизвестно, какие значения свойств фактически были изменены при создании сущности на основе информации в запросе POST или аналогичной. Часто можно просто обновить все значения в базе данных, как в предыдущем примере. Однако если приложение обрабатывает много сущностей, а только небольшое число содержит фактические изменения, может быть полезно ограничить количество отправленных обновлений. Это можно сделать, выполнив запрос для учета сущностей, которые в настоящее время существуют в базе данных, а затем применяя изменения к этим отслеживающим сущностям. Например:

<!--
    public static void UpdateFromHttpPost2(Blog blog)
    {
        using var context = new BlogsContext();

        var trackedBlog = context.Blogs.Find(blog.Id);

        trackedBlog.Name = blog.Name;
        trackedBlog.Summary = blog.Summary;

        context.SaveChanges();
    }
-->
[!code-csharp[Updating_an_entity_2](../../../samples/core/ChangeTracking/IdentityResolutionInEFCore/IdentityResolutionSamples.cs?name=Updating_an_entity_2)]

В данном случае:

- Отслеживание будет только один экземпляр сущности. Объект, возвращаемый запросом из базы данных `Find` .
- `Update`, `Attach` и т. д. **не** используются.
- В базе данных обновляются только значения свойств, которые действительно изменились.
- Выполняется два круговых обращения к базе данных.

EF Core имеет некоторые вспомогательные методы для передачи значений свойств следующим образом. Например, <xref:Microsoft.EntityFrameworkCore.ChangeTracking.PropertyValues.SetValues%2A?displayProperty=nameWithType> копирует все значения из заданного объекта и настраивает их для объекта, на который производится запись:

<!--
    public static void UpdateFromHttpPost3(Blog blog)
    {
        using var context = new BlogsContext();

        var trackedBlog = context.Blogs.Find(blog.Id);

        context.Entry(trackedBlog).CurrentValues.SetValues(blog);

        context.SaveChanges();
    }
-->
[!code-csharp[Updating_an_entity_3](../../../samples/core/ChangeTracking/IdentityResolutionInEFCore/IdentityResolutionSamples.cs?name=Updating_an_entity_3)]

`SetValues` принимает различные типы объектов, в том числе объекты перемещения данных (DTO), с именами свойств, соответствующими свойствам типа сущности. Например:

<!--
    public static void UpdateFromHttpPost4(BlogDto dto)
    {
        using var context = new BlogsContext();

        var trackedBlog = context.Blogs.Find(dto.Id);

        context.Entry(trackedBlog).CurrentValues.SetValues(dto);

        context.SaveChanges();
    }
-->
[!code-csharp[Updating_an_entity_4](../../../samples/core/ChangeTracking/IdentityResolutionInEFCore/IdentityResolutionSamples.cs?name=Updating_an_entity_4)]

Или словарь с записями "имя-значение" для значений свойств:

<!--
    public static void UpdateFromHttpPost5(Dictionary<string, object> propertyValues)
    {
        using var context = new BlogsContext();

        var trackedBlog = context.Blogs.Find(propertyValues["Id"]);

        context.Entry(trackedBlog).CurrentValues.SetValues(propertyValues);

        context.SaveChanges();
    }
-->
[!code-csharp[Updating_an_entity_5](../../../samples/core/ChangeTracking/IdentityResolutionInEFCore/IdentityResolutionSamples.cs?name=Updating_an_entity_5)]

Дополнительные сведения о работе со значениями свойств см. в разделе [доступ к отслеживающим сущностям](xref:core/change-tracking/entity-entries) .

### <a name="use-original-values"></a>Использовать исходные значения

До сих пор каждый подход либо выполнил запрос перед выполнением обновления, либо обновил все значения свойств независимо от того, изменились ли они. Для обновления только тех значений, которые были изменены без запроса в рамках обновления, требуются конкретные сведения об изменении значений свойств. Распространенным способом получения этой информации является отправка текущих и исходных значений в POST HTTP или аналогичной операции. Например:

<!--
    public static void UpdateFromHttpPost6(Blog blog, Dictionary<string, object> originalValues)
    {
        using var context = new BlogsContext();

        context.Attach(blog);
        context.Entry(blog).OriginalValues.SetValues(originalValues);

        context.SaveChanges();
    }
-->
[!code-csharp[Updating_an_entity_6](../../../samples/core/ChangeTracking/IdentityResolutionInEFCore/IdentityResolutionSamples.cs?name=Updating_an_entity_6)]

В этом коде сначала присоединяется сущность с измененными значениями. Это приводит к тому, что EF Core отслеживанию сущности в `Unchanged` состоянии, то есть без значений свойств, помеченных как измененные. Затем словарь исходных значений применяется к этой отслеживающей сущности. При этом будут помечаться как измененные свойства с разными текущими и исходными значениями. Свойства, имеющие одинаковые текущие и исходные значения, не будут помечены как измененные.

В данном случае:

- С помощью присоединения будет отслеживаниь только одного экземпляра сущности.
- Экземпляр сущности **не** запрашивается из базы данных в процессе выполнения обновления.
- Применение исходных значений гарантирует, что в базе данных будут обновлены только значения свойств, которые действительно изменились.
- Выполняется цикл обработки одной базы данных.

Как и в примерах из предыдущего раздела, исходные значения не должны передаваться в качестве словаря. будет также работать экземпляр сущности или DTO.

> [!TIP]
> Хотя этот подход имеет привлекательные характеристики, он требует отправки исходных значений сущности в веб-клиент и обратно. Тщательно продумайте, стоит ли использовать эту дополнительную сложность. для многих приложений один из более простых подходов является более практичным.

## <a name="attaching-a-serialized-graph"></a>Присоединение сериализованного графа

EF Core работает с графами сущностей, подключенных через внешние ключи и свойства навигации, как описано в статье [изменение внешних ключей и переходов](xref:core/change-tracking/relationship-changes). Если эти графы создаются вне EF Core с помощью, например, из JSON-файла, они могут иметь несколько экземпляров одной и той же сущности. Эти дубликаты необходимо разрешить в отдельные экземпляры, прежде чем можно будет относиться к диаграмме.

### <a name="graphs-with-no-duplicates"></a>Графы без дубликатов

Прежде чем продолжить, важно понять, что:

- Сериализаторы часто имеют параметры для обработки циклов и повторяющихся экземпляров в графе.
- Выбранный объект, используемый в качестве корня графа, часто может помочь уменьшить или удалить дубликаты.

По возможности используйте параметры сериализации и выберите корни, которые не приводят к дублированию. Например, следующий код использует [JSON.NET](https://www.nuget.org/packages/Newtonsoft.Json/) для сериализации списка блогов со связанными записями:

<!--
            using var context = new BlogsContext();

            var blogs = context.Blogs.Include(e => e.Posts).ToList();

            var serialized = JsonConvert.SerializeObject(
                blogs,
                new JsonSerializerSettings
                {
                    ReferenceLoopHandling = ReferenceLoopHandling.Ignore,
                    Formatting = Formatting.Indented
                });

            Console.WriteLine(serialized);
-->
[!code-csharp[Attaching_a_serialized_graph_1a](../../../samples/core/ChangeTracking/IdentityResolutionInEFCore/SerializedGraphExamples.cs?name=Attaching_a_serialized_graph_1a)]

Код JSON, созданный из этого кода:

```json
[
  {
    "Id": 1,
    "Name": ".NET Blog",
    "Summary": "Posts about .NET",
    "Posts": [
      {
        "Id": 1,
        "Title": "Announcing the Release of EF Core 5.0",
        "Content": "Announcing the release of EF Core 5.0, a full featured cross-platform...",
        "BlogId": 1
      },
      {
        "Id": 2,
        "Title": "Announcing F# 5",
        "Content": "F# 5 is the latest version of F#, the functional programming language...",
        "BlogId": 1
      }
    ]
  },
  {
    "Id": 2,
    "Name": "Visual Studio Blog",
    "Summary": "Posts about Visual Studio",
    "Posts": [
      {
        "Id": 3,
        "Title": "Disassembly improvements for optimized managed debugging",
        "Content": "If you are focused on squeezing out the last bits of performance for your .NET service or...",
        "BlogId": 2
      },
      {
        "Id": 4,
        "Title": "Database Profiling with Visual Studio",
        "Content": "Examine when database queries were executed and measure how long the take using...",
        "BlogId": 2
      }
    ]
  }
]
```

Обратите внимание, что в JSON нет повторяющихся блогов или записей. Это означает, что простые вызовы `Update` будут работать для обновления этих сущностей в базе данных:

<!--
        public static void UpdateBlogsFromJson(string json)
        {
            using var context = new BlogsContext();

            var blogs = JsonConvert.DeserializeObject<List<Blog>>(json);

            foreach (var blog in blogs)
            {
                context.Update(blog);
            }

            context.SaveChanges();
        }
-->
[!code-csharp[Attaching_a_serialized_graph_1b](../../../samples/core/ChangeTracking/IdentityResolutionInEFCore/SerializedGraphExamples.cs?name=Attaching_a_serialized_graph_1b)]

### <a name="handling-duplicates"></a>Обработка дубликатов

Код в предыдущем примере сериализован каждый блог со связанными записями. Если он был изменен для сериализации каждой записи со связанным с ним блогом, то в сериализованный JSON вводятся дубликаты. Например:

<!--
            using var context = new BlogsContext();

            var posts = context.Posts.Include(e => e.Blog).ToList();

            var serialized = JsonConvert.SerializeObject(
                posts,
                new JsonSerializerSettings
                {
                    ReferenceLoopHandling = ReferenceLoopHandling.Ignore,
                    Formatting = Formatting.Indented
                });

            Console.WriteLine(serialized);
-->
[!code-csharp[Attaching_a_serialized_graph_2](../../../samples/core/ChangeTracking/IdentityResolutionInEFCore/SerializedGraphExamples.cs?name=Attaching_a_serialized_graph_2)]

Сериализованный JSON теперь выглядит следующим образом:

```json
[
  {
    "Id": 1,
    "Title": "Announcing the Release of EF Core 5.0",
    "Content": "Announcing the release of EF Core 5.0, a full featured cross-platform...",
    "BlogId": 1,
    "Blog": {
      "Id": 1,
      "Name": ".NET Blog",
      "Summary": "Posts about .NET",
      "Posts": [
        {
          "Id": 2,
          "Title": "Announcing F# 5",
          "Content": "F# 5 is the latest version of F#, the functional programming language...",
          "BlogId": 1
        }
      ]
    }
  },
  {
    "Id": 2,
    "Title": "Announcing F# 5",
    "Content": "F# 5 is the latest version of F#, the functional programming language...",
    "BlogId": 1,
    "Blog": {
      "Id": 1,
      "Name": ".NET Blog",
      "Summary": "Posts about .NET",
      "Posts": [
        {
          "Id": 1,
          "Title": "Announcing the Release of EF Core 5.0",
          "Content": "Announcing the release of EF Core 5.0, a full featured cross-platform...",
          "BlogId": 1
        }
      ]
    }
  },
  {
    "Id": 3,
    "Title": "Disassembly improvements for optimized managed debugging",
    "Content": "If you are focused on squeezing out the last bits of performance for your .NET service or...",
    "BlogId": 2,
    "Blog": {
      "Id": 2,
      "Name": "Visual Studio Blog",
      "Summary": "Posts about Visual Studio",
      "Posts": [
        {
          "Id": 4,
          "Title": "Database Profiling with Visual Studio",
          "Content": "Examine when database queries were executed and measure how long the take using...",
          "BlogId": 2
        }
      ]
    }
  },
  {
    "Id": 4,
    "Title": "Database Profiling with Visual Studio",
    "Content": "Examine when database queries were executed and measure how long the take using...",
    "BlogId": 2,
    "Blog": {
      "Id": 2,
      "Name": "Visual Studio Blog",
      "Summary": "Posts about Visual Studio",
      "Posts": [
        {
          "Id": 3,
          "Title": "Disassembly improvements for optimized managed debugging",
          "Content": "If you are focused on squeezing out the last bits of performance for your .NET service or...",
          "BlogId": 2
        }
      ]
    }
  }
]
```

Обратите внимание, что теперь граф содержит несколько экземпляров блогов с одинаковым значением ключа, а также несколько экземпляров Post с одинаковым значением ключа. Попытка отслеживанию этого графа, как в предыдущем примере, приведет к созданию исключения:

> System. InvalidOperationException: экземпляр типа сущности "Post" не может быть отслеживаем, так как уже отслеживается другой экземпляр со значением ключа "{ID: 2}". При присоединении существующих сущностей убедитесь, что присоединен только один экземпляр сущности с заданным значением ключа.

Это можно исправить двумя способами:

- Использование параметров сериализации JSON, сохраняющих ссылки
- Выполнение разрешения идентификации во время трассировки графа

#### <a name="preserve-references"></a>Сохранение ссылок

Json.NET предоставляет `PreserveReferencesHandling` возможность справиться с этим. Например:

<!--
            var serialized = JsonConvert.SerializeObject(
                posts,
                new JsonSerializerSettings
                {
                    PreserveReferencesHandling = PreserveReferencesHandling.All,
                    Formatting = Formatting.Indented
                });
-->
[!code-csharp[Attaching_a_serialized_graph_3](../../../samples/core/ChangeTracking/IdentityResolutionInEFCore/SerializedGraphExamples.cs?name=Attaching_a_serialized_graph_3)]

Полученный JSON теперь выглядит следующим образом:

```json
{
  "$id": "1",
  "$values": [
    {
      "$id": "2",
      "Id": 1,
      "Title": "Announcing the Release of EF Core 5.0",
      "Content": "Announcing the release of EF Core 5.0, a full featured cross-platform...",
      "BlogId": 1,
      "Blog": {
        "$id": "3",
        "Id": 1,
        "Name": ".NET Blog",
        "Summary": "Posts about .NET",
        "Posts": [
          {
            "$ref": "2"
          },
          {
            "$id": "4",
            "Id": 2,
            "Title": "Announcing F# 5",
            "Content": "F# 5 is the latest version of F#, the functional programming language...",
            "BlogId": 1,
            "Blog": {
              "$ref": "3"
            }
          }
        ]
      }
    },
    {
      "$ref": "4"
    },
    {
      "$id": "5",
      "Id": 3,
      "Title": "Disassembly improvements for optimized managed debugging",
      "Content": "If you are focused on squeezing out the last bits of performance for your .NET service or...",
      "BlogId": 2,
      "Blog": {
        "$id": "6",
        "Id": 2,
        "Name": "Visual Studio Blog",
        "Summary": "Posts about Visual Studio",
        "Posts": [
          {
            "$ref": "5"
          },
          {
            "$id": "7",
            "Id": 4,
            "Title": "Database Profiling with Visual Studio",
            "Content": "Examine when database queries were executed and measure how long the take using...",
            "BlogId": 2,
            "Blog": {
              "$ref": "6"
            }
          }
        ]
      }
    },
    {
      "$ref": "7"
    }
  ]
}
```

Обратите внимание, что этот JSON заменяет дубликаты ссылками `"$ref": "5"` , которые ссылаются на уже существующий экземпляр в графе. Эту диаграмму можно еще раз отслеживанию с помощью простых вызовов `Update` , как показано выше.

<xref:System.Text.Json>Поддержка в библиотеках базовых классов (BCL) .NET аналогична параметру, который дает тот же результат. Например:

<!--
            var serialized = System.Text.Json.JsonSerializer.Serialize(posts, new System.Text.Json.JsonSerializerOptions
            {
                ReferenceHandler = ReferenceHandler.Preserve,
                WriteIndented = true
            });
-->
[!code-csharp[Attaching_a_serialized_graph_4](../../../samples/core/ChangeTracking/IdentityResolutionInEFCore/SerializedGraphExamples.cs?name=Attaching_a_serialized_graph_4)]

#### <a name="resolve-duplicates"></a>Разрешить дубликаты

Если не удается исключить дубликаты в процессе сериализации, то <xref:Microsoft.EntityFrameworkCore.ChangeTracking.ChangeTracker.TrackGraph%2A?displayProperty=nameWithType> предоставляет способ обработки этого. Траккграф работает так же `Add` , как `Attach` и, `Update` за исключением того, что он создает обратный вызов для каждого экземпляра сущности перед его отслеживанием. Этот обратный вызов можно использовать для записи или пропуска сущности. Например:

<!--
        public static void UpdatePostsFromJsonWithIdentityResolution(string json)
        {
            using var context = new BlogsContext();

            var posts = JsonConvert.DeserializeObject<List<Post>>(json);

            foreach (var post in posts)
            {
                context.ChangeTracker.TrackGraph(
                    post, node =>
                        {
                            var keyValue = node.Entry.Property("Id").CurrentValue;
                            var entityType = node.Entry.Metadata;

                            var existingEntity = node.Entry.Context.ChangeTracker.Entries()
                                .FirstOrDefault(
                                    e => Equals(e.Metadata, entityType)
                                         && Equals(e.Property("Id").CurrentValue, keyValue));

                            if (existingEntity == null)
                            {
                                Console.WriteLine($"Tracking {entityType} entity with key value {keyValue}");

                                node.Entry.State = EntityState.Modified;
                            }
                            else
                            {
                                Console.WriteLine($"Discarding duplicate {entityType} entity with key value {keyValue}");
                            }
                        });
            }

            context.SaveChanges();
        }
-->
[!code-csharp[Attaching_a_serialized_graph_5](../../../samples/core/ChangeTracking/IdentityResolutionInEFCore/SerializedGraphExamples.cs?name=Attaching_a_serialized_graph_5)]

Для каждой сущности в графе этот код будет следующим:

- Поиск типа сущности и значения ключа сущности
- Поиск сущности с этим ключом в средстве записи изменений
  - Если сущность найдена, дальнейшие действия не выполняются, так как сущность дублируется
  - Если сущность не найдена, она будет отслеживаниь, задав для состояния значение `Modified`

Результат выполнения этого кода:

```output
Tracking EntityType: Post entity with key value 1
Tracking EntityType: Blog entity with key value 1
Tracking EntityType: Post entity with key value 2
Discarding duplicate EntityType: Post entity with key value 2
Tracking EntityType: Post entity with key value 3
Tracking EntityType: Blog entity with key value 2
Tracking EntityType: Post entity with key value 4
Discarding duplicate EntityType: Post entity with key value 4
```

> [!IMPORTANT]
> В этом коде **предполагается, что все дубликаты идентичны**. Это позволяет получить возможность произвольно выбрать одну из дубликатов, чтобы отказаться от других. Если дубликаты могут различаться, то код должен решить, как определить, какой из них следует использовать, а также как объединить свойства и значения навигации.

> [!NOTE]
> Для простоты в этом коде предполагается, что каждая сущность имеет свойство первичного ключа с именем `Id` . Это можно кодифицированные в абстрактный базовый класс или интерфейс. Кроме того, свойство или свойства первичного ключа можно получить из <xref:Microsoft.EntityFrameworkCore.Metadata.IEntityType> метаданных таким образом, чтобы этот код работал с любым типом сущности.

## <a name="failing-to-set-key-values"></a>Не удалось задать значения ключей

Типы сущностей часто настраиваются для использования [автоматически создаваемых значений ключа](xref:core/modeling/generated-properties). Это значение по умолчанию для свойств Integer и GUID несоставных ключей. Однако если тип сущности не настроен для использования автоматически создаваемых значений ключа, перед отслеживанием сущности необходимо задать явное значение ключа. Например, используя следующий тип сущности:

<!--
    public class Pet
    {
        [DatabaseGenerated(DatabaseGeneratedOption.None)]
        public int Id { get; set; }

        public string Name { get; set; }
    }
-->
[!code-csharp[Pet](../../../samples/core/ChangeTracking/IdentityResolutionInEFCore/IdentityResolutionSamples.cs?name=Pet)]

Рассмотрим код, который пытается прослеживать два новых экземпляра сущности, не задавая значения ключей:

<!--
            using var context = new BlogsContext();

            context.Add(new Pet { Name = "Smokey" });

            try
            {
                context.Add(new Pet { Name = "Clippy" }); // This will throw
            }
            catch (Exception e)
            {
                Console.WriteLine(e);
            }
-->
[!code-csharp[Failing_to_set_key_values_1](../../../samples/core/ChangeTracking/IdentityResolutionInEFCore/IdentityResolutionSamples.cs?name=Failing_to_set_key_values_1)]

Этот код выдаст следующее:

> System. InvalidOperationException: экземпляр типа сущности "Pet" не может быть отслеживаем, так как уже отслеживается другой экземпляр со значением ключа "{ID: 0}". При присоединении существующих сущностей убедитесь, что присоединен только один экземпляр сущности с заданным значением ключа.

Исправление для этого заключается в явном задании значений ключа или настройке свойства ключа для использования созданных значений ключа. Дополнительные сведения см. в разделе [созданные значения](xref:core/modeling/generated-properties) .

## <a name="overusing-a-single-dbcontext-instance"></a>Использование одного экземпляра DbContext

<xref:Microsoft.EntityFrameworkCore.DbContext> разработана для представления кратковременной единицы работы, как описано в разделе [DbContext Initialization and Configuration (инициализация и конфигурация](xref:core/dbcontext-configuration/index)) и усовершенствована в [Отслеживание изменений в EF Core](xref:core/change-tracking/index). Это не следует делать в ситуациях, когда предпринимается попытка наблюдения за несколькими экземплярами одной и той же сущности. Ниже приведены распространенные примеры.

- Использование одного и того же экземпляра DbContext для настройки состояния теста и последующего выполнения теста. Это часто приводит к DbContext отслеживания одного экземпляра сущности из настройки тестирования, а затем пытается присоединить новый экземпляр в нужном тесте. Вместо этого используйте другой экземпляр DbContext для настройки состояния теста и надлежащего кода теста.
- Использование общего экземпляра DbContext в репозитории или аналогичного кода. Вместо этого убедитесь, что репозиторий использует один экземпляр DbContext для каждой единицы работы.

## <a name="identity-resolution-and-queries"></a>Разрешение и запросы идентификации

Разрешение идентификации происходит автоматически при отслеживании сущностей из запроса. Это означает, что если экземпляр сущности с заданным значением ключа уже был записан, то вместо создания нового экземпляра будет использоваться существующий отслеживаниющий экземпляр. Это имеет большое значение: Если данные были изменены в базе данных, это не будет отражено в результатах запроса. Это хорошая причина для использования нового экземпляра DbContext для каждой единицы работы, как описано в разделе [Инициализация и конфигурация DbContext](xref:core/dbcontext-configuration/index), а также в [Отслеживание изменений в EF Core](xref:core/change-tracking/index).

> [!IMPORTANT]
> Важно понимать, что EF Core всегда выполняет запрос LINQ для базы данных DbSet и возвращает результаты только в зависимости от того, что находится в базе данных. Однако для запроса отслеживания, если возвращаемые сущности уже отслеживаются, то вместо создания экземпляров на основе данных в базе данных используются Отслеживаемые экземпляры.

<xref:Microsoft.EntityFrameworkCore.ChangeTracking.EntityEntry.Reload> или <xref:Microsoft.EntityFrameworkCore.ChangeTracking.EntityEntry.GetDatabaseValues> может использоваться, если отслеживание сущностей необходимо обновить последними данными из базы данных. Дополнительные сведения см. в разделе [доступ к отслеживающим сущностям](xref:core/change-tracking/entity-entries) .

В отличие от отслеживания запросов, запросы без отслеживания не выполняют разрешение идентификаторов. Это означает, что запросы без отслеживания могут возвращать дубликаты точно так же, как в случае сериализации JSON, описанном выше. Обычно это не является проблемой, если результаты запроса будут сериализованы и отправлены клиенту.

> [!TIP]
> Не осуществляйте регулярный запрос без отслеживания, а затем присоедините возвращенные сущности к тому же контексту. Это будет как медленнее, так и труднее, чем с помощью запроса отслеживания.

Запросы без отслеживания не выполняют разрешение идентификации, так как это влияет на производительность потоковой передачи большого количества сущностей из запроса. Это обусловлено тем, что разрешение идентификации требует отслеживания каждого возвращаемого экземпляра, чтобы его можно было использовать вместо создания дубликата.

Начиная с EF Core 5,0, запросы без отслеживания могут принудительно выполнять разрешение идентификаторов с помощью <xref:Microsoft.EntityFrameworkCore.EntityFrameworkQueryableExtensions.AsNoTrackingWithIdentityResolution%60%601(System.Linq.IQueryable{%60%600})> . После этого запрос будет отслеживать возвращаемые экземпляры (не отслеживая их обычным способом) и не будет создавать дубликаты в результатах запроса.

## <a name="overriding-object-equality"></a>Переопределение равенства объектов

EF Core использует [равенство ссылок](/dotnet/csharp/programming-guide/statements-expressions-operators/equality-comparisons) при сравнении экземпляров сущностей. Это происходит даже в том случае, если типы сущностей переопределяют <xref:System.Object.Equals(System.Object)?displayProperty=nameWithType> или иным образом изменяют равенство объектов. Однако существует одно место, где переопределение равенства может повлиять на EF Core поведение: когда навигация коллекции использует переопределенную равенство вместо равенства ссылок, и, следовательно, сообщает о нескольких экземплярах.

Поэтому рекомендуется избегать переопределения равенства сущностей. Если он используется, обязательно создайте навигацию по коллекциям, которые принудительно выполняют равенство ссылок. Например, создайте компаратор проверки на равенство, использующий равенство ссылок:

<!--
    public sealed class ReferenceEqualityComparer : IEqualityComparer<object>
    {
        private ReferenceEqualityComparer()
        {
        }

        public static ReferenceEqualityComparer Instance { get; } = new ReferenceEqualityComparer();

        bool IEqualityComparer<object>.Equals(object x, object y) => x == y;

        int IEqualityComparer<object>.GetHashCode(object obj) => RuntimeHelpers.GetHashCode(obj);
    }
-->
[!code-csharp[ReferenceEqualityComparer](../../../samples/core/ChangeTracking/IdentityResolutionInEFCore/ReferenceEqualityComparer.cs?name=ReferenceEqualityComparer)]

(Начиная с .NET 5, он включается в BCL как <xref:System.Collections.Generic.ReferenceEqualityComparer> .)

Затем этот компаратор можно использовать при создании переходов по коллекции. Например:

<!--
        public ICollection<Order> Orders { get; set; }
            = new HashSet<Order>(ReferenceEqualityComparer.Instance);
-->
[!code-csharp[OrdersCollection](../../../samples/core/ChangeTracking/IdentityResolutionInEFCore/IdentityResolutionSamples.cs?name=OrdersCollection)]

### <a name="comparing-key-properties"></a>Сравнение свойств ключа

Помимо сравнения на равенство, значения ключей также должны быть упорядочены. Это важно для предотвращения взаимоблокировок при обновлении нескольких сущностей в одном вызове метода SaveChanges. Все типы, используемые для свойств первичного, альтернативного или внешнего ключа, а также те, которые используются для уникальных индексов, должны реализовывать <xref:System.IComparable%601> и <xref:System.IEquatable%601> . Типы, обычно используемые в качестве ключей (int, GUID, String и т. д.), уже поддерживают эти интерфейсы. Типы пользовательских ключей могут добавлять эти интерфейсы.
