---
title: Каскадное удаление — EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: ee8e14ec-2158-4c9c-96b5-118715e2ed9e
ms.technology: entity-framework-core
uid: core/saving/cascade-delete
ms.openlocfilehash: 0fc8929c56d4c657b7fb1e3c8e4b1a71659220c9
ms.sourcegitcommit: 507a40ed050fee957bcf8cf05f6e0ec8a3b1a363
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/26/2018
ms.locfileid: "31812681"
---
# <a name="cascade-delete"></a>Каскадное удаление

Каскадное удаление обычно используется в терминологии базы данных, чтобы описать характеристики, которые позволяют удалить строку, чтобы автоматически инициировать удаление связанных строк. Тесно связанная концепция, которая также охватывается поведением удаления EF Core, — это автоматическое удаление дочерней сущности, когда ее связь с родительской сущностью разорвана. Часто это называется "удалением потерянных объектов".

EF Core реализует несколько поведений удаления и предоставляет конфигурацию поведений удаления отдельных связей. Эта платформа также реализует соглашения, с помощью которых можно автоматически настроить полезные стандартные поведения удаления для каждой связи на основе [необходимости связей](../modeling/relationships.md#required-and-optional-relationships).

## <a name="delete-behaviors"></a>Поведения удаления
Поведения удаления определены в типе перечислителя *DeleteBehavior*. Их можно передать в текучий API *OnDelete*, чтобы определить, будет ли удаление основной или родительской сущности или разрыв связей с зависимыми или дочерними сущностями иметь побочный эффект в зависимых или дочерних сущностях.

Существует три действия, которые EF может предпринять при удалении основной или родительской сущности, а также при разрыве связи с дочерней сущностью:
* дочерняя или зависимая сущность может быть удалена;
* значения дочернего внешнего ключа могут быть NULL;
* дочерняя сущность остается неизменной.

> [!NOTE]  
> Поведение удаления, настроенное в модели EF, применяется, только когда основная сущность удалена с помощью EF Core, а зависимые сущности загружены в память (например, для отслеживаемых зависимостей). В базе данных необходимо настроить соответствующее каскадное поведение, чтобы убедиться, что к данным, не отслеживаемым контекстом, применены необходимые действия. Если вы создаете базу данных с помощью EF Core, каскадное поведение будет установлено автоматически.

Для второго действия, указанного выше, устанавливать для внешнего ключа значение NULL недопустимо, если он не допускает этого значения. (Внешний ключ, не допускающий значения NULL, соответствует обязательной связи.) В этих случаях EF Core отслеживает, чтобы свойство внешнего ключа было отмечено как NULL, до вызова метода SaveChanges. Иначе возникнет исключение, так как изменение нельзя сохранить в базе данных. Это похоже на получение нарушения ограничения из базы данных.

Есть четыре поведения удаления, как показано в таблицах ниже. Для необязательных связей (с внешним ключом, допускающим значение NULL) _можно_ сохранить значение NULL внешнего ключа. Это имеет такие последствия:

| Имя поведения               | Влияние на зависимую или дочернюю сущность в памяти    | Влияние на зависимую или дочернюю сущность в базе данных  |
|:----------------------------|:---------------------------------------|:---------------------------------------|
| **Cascade**                 | Сущности удаляются                   | Сущности удаляются                   |
| **ClientSetNull** (по умолчанию) | Свойства внешнего ключа имеют значения NULL | Нет                                   |
| **SetNull**                 | Свойства внешнего ключа имеют значения NULL | Свойства внешнего ключа имеют значения NULL |
| **Restrict**                | Нет                                   | Нет                                   |

Для обязательных связей (внешний ключ, не допускающий значения NULL) _невозможно_ сохранить значение NULL внешнего ключа. Это имеет такие последствия:

| Имя поведения         | Влияние на зависимую или дочернюю сущность в памяти | Влияние на зависимую или дочернюю сущность в базе данных |
|:----------------------|:------------------------------------|:--------------------------------------|
| **Cascade** (по умолчанию) | Сущности удаляются                | Сущности удаляются                  |
| **ClientSetNull**     | Строки SaveChanges                  | Нет                                  |
| **SetNull**           | Строки SaveChanges                  | Строки SaveChanges                    |
| **Restrict**          | Нет                                | Нет                                  |

Указанное в таблице выше значение *Нет* может привести к нарушению ограничения. Например, если основная или дочерняя сущность удалена, но никаких действий, чтобы изменить внешний ключ зависимой или дочерней сущности не выполняется, то база данных, вероятно, вызовет метод SaveChanges из-за нарушения ограничения внешнего ключа.

На высоком уровне:
* Если у вас есть сущности, которые не могут существовать без родительской сущности, и вы хотите, чтобы EF автоматически удалил дочернюю сущность, воспользуйтесь поведением *Cascade*.
  * Сущности, которые не могут существовать без родительского элемента, обычно используют обязательные связи, в которых по умолчанию используется поведение *Cascade*.
* Если у вас есть сущности, у которых может быть или отсутствовать родительский элемент, и вы хотите, чтобы EF автоматически обнулил внешний ключ, используйте поведение *ClientSetNull*.
  * Сущности, которые могут существовать без родительского элемента, обычно используют необязательные связи, в которых по умолчанию используется поведение *ClientSetNull*.
  * Если требуется, чтобы база данных также пыталась распространять значения NULL для дочерних внешних ключей, даже когда дочерняя сущность не загружена, используйте поведение *SetNull*. Тем не менее, обратите внимание, что база данных должна поддерживать эту возможность. Настройка базы данных таким образом может привести к другим ограничениям, которые на практике часто делают этот вариант непрактичным. Поэтому *SetNull* не используется по умолчанию.
* Если вы хотите, чтобы платформа EF Core никогда автоматически не удаляла сущность или не обнуляла внешний ключ, используйте поведение *Restrict*. Обратите внимание, что для этого необходимо вручную синхронизировать в коде дочерние сущности и их значения внешнего ключа, в противном случае возникнут исключения ограничения.

> [!NOTE]
> В EF Core, в отличие от EF6, каскадные эффекты не происходят немедленно, а только при вызове метода SaveChanges.

> [!NOTE]  
> **Изменения в EF Core 2.0.** В предыдущих выпусках поведение *Restrict* присваивало для необязательных свойств внешнего ключа в отслеживаемых зависимых сущностях значение NULL. Это было стандартное поведение удаления для необязательных связей. В EF Core 2.0 предоставлено *ClientSetNull*, которое имеет аналогичное поведение и является стандартным для необязательных связей. Поведение *Restrict* было изменено, чтобы никогда не иметь побочных эффектов в зависимых сущностях.

## <a name="entity-deletion-examples"></a>Примеры удаления сущности

Приведенный ниже код является частью [примера](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/CascadeDelete/), который можно скачать и выполнить. В примере показано, что происходит с каждым поведением удаления для обязательных и необязательных связей при удалении родительской сущности.

[!code-csharp[Main](../../../samples/core/Saving/Saving/CascadeDelete/Sample.cs#DeleteBehaviorVariations)]

Рассмотрим каждый вариант, чтобы понять, что происходит.

### <a name="deletebehaviorcascade-with-required-or-optional-relationship"></a>DeleteBehavior.Cascade с обязательной или необязательной связью

```
  After loading entities:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.

  After deleting blog '1':
    Blog '1' is in state Deleted with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.

  Saving changes:
    DELETE FROM [Posts] WHERE [PostId] = 1
    DELETE FROM [Posts] WHERE [PostId] = 2
    DELETE FROM [Blogs] WHERE [BlogId] = 1

  After SaveChanges:
    Blog '1' is in state Detached with 2 posts referenced.
      Post '1' is in state Detached with FK '1' and no reference to a blog.
      Post '1' is in state Detached with FK '1' and no reference to a blog.
```

* Блог помечен как удаленный.
* Записи изначально остаются без изменений, так как каскадные удаления не выполняются до вызова метода SaveChanges.
* SaveChanges отправляет операции удаления в зависимые или дочерние сущности (записи), а затем в основные или родительские (блог).
* После сохранения все сущности отсоединяются, так как они были удалены из базы данных.

### <a name="deletebehaviorclientsetnull-or-deletebehaviorsetnull-with-required-relationship"></a>DeleteBehavior.ClientSetNull или DeleteBehavior.SetNull с обязательной связью

```
  After loading entities:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.

  After deleting blog '1':
    Blog '1' is in state Deleted with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.

  Saving changes:
    UPDATE [Posts] SET [BlogId] = NULL WHERE [PostId] = 1

  SaveChanges threw DbUpdateException: Cannot insert the value NULL into column 'BlogId', table 'EFSaving.CascadeDelete.dbo.Posts'; column does not allow nulls. UPDATE fails. The statement has been terminated.
```

* Блог помечен как удаленный.
* Записи изначально остаются без изменений, так как каскадные удаления не выполняются до вызова метода SaveChanges.
* SaveChanges пытается установить для внешнего ключа записи значение NULL. Однако эта попытка завершается неудачей, так как внешний ключ не допускает этого значения.

### <a name="deletebehaviorclientsetnull-or-deletebehaviorsetnull-with-optional-relationship"></a>DeleteBehavior.ClientSetNull или DeleteBehavior.SetNull с необязательной связью

```
  After loading entities:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.

  After deleting blog '1':
    Blog '1' is in state Deleted with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.

  Saving changes:
    UPDATE [Posts] SET [BlogId] = NULL WHERE [PostId] = 1
    UPDATE [Posts] SET [BlogId] = NULL WHERE [PostId] = 2
    DELETE FROM [Blogs] WHERE [BlogId] = 1

  After SaveChanges:
    Blog '1' is in state Detached with 2 posts referenced.
      Post '1' is in state Unchanged with FK 'null' and no reference to a blog.
      Post '1' is in state Unchanged with FK 'null' and no reference to a blog.
```

* Блог помечен как удаленный.
* Записи изначально остаются без изменений, так как каскадные удаления не выполняются до вызова метода SaveChanges.
* SaveChanges пытается установить для внешнего ключа зависимых или дочерних сущностей (записей) значение NULL, прежде чем удалить основные или родительские (блог).
* После сохранения основная или родительская сущность (блог) удаляется, но зависимые или дочерние сущности (записи) по-прежнему отслеживаются.
* Отслеживаемые зависимые и дочерние сущности (записи) теперь имеют NULL в качестве значений внешних ключей, а их связь с удаленными основными и родительскими сущностями (блог) удалена.

### <a name="deletebehaviorrestrict-with-required-or-optional-relationship"></a>DeleteBehavior.Restrict с обязательной или необязательной связью

```
  After loading entities:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.

  After deleting blog '1':
    Blog '1' is in state Deleted with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.

  Saving changes:
  SaveChanges threw InvalidOperationException: The association between entity types 'Blog' and 'Post' has been severed but the foreign key for this relationship cannot be set to null. If the dependent entity should be deleted, then setup the relationship to use cascade deletes.
```

* Блог помечен как удаленный.
* Записи изначально остаются без изменений, так как каскадные удаления не выполняются до вызова метода SaveChanges.
* Так как *Restrict* указывает EF не устанавливать автоматически для внешнего ключа значение NULL, он не имеет этого значения и метод SaveChanges вызывается без сохранения.

## <a name="delete-orphans-examples"></a>Примеры удаления потерянных объектов

Приведенный ниже код является частью [примера](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/CascadeDelete/), который можно скачать и выполнить. В примере показано, что происходит с каждым поведением удаления для обязательных и необязательных связей при разрыве связи между родительскими или основными сущностями и их дочерними или зависимыми сущностями. В этом примере связь разрывается путем удаления зависимых или дочерних сущностей (записей) из свойства навигации коллекции в основных или родительских сущностях (блог). Тем не менее поведение остается прежним, если ссылка из зависимой или дочерней сущности к основной или родительской сущности обнулена.

[!code-csharp[Main](../../../samples/core/Saving/Saving/CascadeDelete/Sample.cs#DeleteOrphansVariations)]

Рассмотрим каждый вариант, чтобы понять, что происходит.

### <a name="deletebehaviorcascade-with-required-or-optional-relationship"></a>DeleteBehavior.Cascade с обязательной или необязательной связью

```
  After loading entities:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.

  After making posts orphans:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Modified with FK '1' and no reference to a blog.
      Post '1' is in state Modified with FK '1' and no reference to a blog.

  Saving changes:
    DELETE FROM [Posts] WHERE [PostId] = 1
    DELETE FROM [Posts] WHERE [PostId] = 2

  After SaveChanges:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Detached with FK '1' and no reference to a blog.
      Post '1' is in state Detached with FK '1' and no reference to a blog.
```

* Записи помечены как измененные, так как из-за разрыва связи внешний ключ помечен как NULL.
  * Если внешний ключ не допускает значения NULL, то фактическое значение не будет изменено, даже если оно помечено как NULL.
* SaveChanges отправляет операции удаления в зависимые или дочерние сущности (записи).
* После сохранения зависимые или дочерние сущности (записи) отсоединяются, так как они были удалены из базы данных.

### <a name="deletebehaviorclientsetnull-or-deletebehaviorsetnull-with-required-relationship"></a>DeleteBehavior.ClientSetNull или DeleteBehavior.SetNull с обязательной связью

```
  After loading entities:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.

  After making posts orphans:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Modified with FK 'null' and no reference to a blog.
      Post '1' is in state Modified with FK 'null' and no reference to a blog.

  Saving changes:
    UPDATE [Posts] SET [BlogId] = NULL WHERE [PostId] = 1

  SaveChanges threw DbUpdateException: Cannot insert the value NULL into column 'BlogId', table 'EFSaving.CascadeDelete.dbo.Posts'; column does not allow nulls. UPDATE fails. The statement has been terminated.
```

* Записи помечены как измененные, так как из-за разрыва связи внешний ключ помечен как NULL.
  * Если внешний ключ не допускает значения NULL, то фактическое значение не будет изменено, даже если оно помечено как NULL.
* SaveChanges пытается установить для внешнего ключа записи значение NULL. Однако эта попытка завершается неудачей, так как внешний ключ не допускает этого значения.

### <a name="deletebehaviorclientsetnull-or-deletebehaviorsetnull-with-optional-relationship"></a>DeleteBehavior.ClientSetNull или DeleteBehavior.SetNull с необязательной связью

```
  After loading entities:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.

  After making posts orphans:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Modified with FK 'null' and no reference to a blog.
      Post '1' is in state Modified with FK 'null' and no reference to a blog.

  Saving changes:
    UPDATE [Posts] SET [BlogId] = NULL WHERE [PostId] = 1
    UPDATE [Posts] SET [BlogId] = NULL WHERE [PostId] = 2

  After SaveChanges:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Unchanged with FK 'null' and no reference to a blog.
      Post '1' is in state Unchanged with FK 'null' and no reference to a blog.
```

* Записи помечены как измененные, так как из-за разрыва связи внешний ключ помечен как NULL.
  * Если внешний ключ не допускает значения NULL, то фактическое значение не будет изменено, даже если оно помечено как NULL.
* SaveChanges устанавливает для внешнего ключа зависимых или дочерних сущностей (записей) значение NULL.
* После сохранения зависимые или дочерние сущности (записи) теперь имеют NULL в качестве значений внешних ключей, а их связь с удаленными основными или родительскими сущностями (блог) удалена.

### <a name="deletebehaviorrestrict-with-required-or-optional-relationship"></a>DeleteBehavior.Restrict с обязательной или необязательной связью

```
  After loading entities:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.
      Post '1' is in state Unchanged with FK '1' and reference to blog '1'.

  After making posts orphans:
    Blog '1' is in state Unchanged with 2 posts referenced.
      Post '1' is in state Modified with FK '1' and no reference to a blog.
      Post '1' is in state Modified with FK '1' and no reference to a blog.

  Saving changes:
  SaveChanges threw InvalidOperationException: The association between entity types 'Blog' and 'Post' has been severed but the foreign key for this relationship cannot be set to null. If the dependent entity should be deleted, then setup the relationship to use cascade deletes.
```

* Записи помечены как измененные, так как из-за разрыва связи внешний ключ помечен как NULL.
  * Если внешний ключ не допускает значения NULL, то фактическое значение не будет изменено, даже если оно помечено как NULL.
* Так как *Restrict* указывает EF не устанавливать автоматически для внешнего ключа значение NULL, он не имеет этого значения и метод SaveChanges вызывается без сохранения.

## <a name="cascading-to-untracked-entities"></a>Использование каскадных функций в неотслеживаемых сущностях

При вызове метода *SaveChanges* правила каскадного удаления будут применяться ко всем сущностям, которые отслеживаются контекстом. Такая ситуация приведена в примерах выше. Вот почему был создан код SQL, чтобы удалить основные или родительские сущности (блог) и все зависимые или дочерние сущности (записи).

```sql
    DELETE FROM [Posts] WHERE [PostId] = 1
    DELETE FROM [Posts] WHERE [PostId] = 2
    DELETE FROM [Blogs] WHERE [BlogId] = 1
```

Если загружается только субъект, например, когда запрос, выполненный для блога без `Include(b => b.Posts)`, также содержит записи, то SaveChanges создаст код SQL, чтобы удалить основную или родительскую сущность.

```sql
    DELETE FROM [Blogs] WHERE [BlogId] = 1
```

Зависимые или дочерние сущности (записи) будут удалены, только если в базе данных настроено соответствующее каскадное поведение. Если вы создаете базу данных с помощью EF, каскадное поведение будет установлено автоматически.
