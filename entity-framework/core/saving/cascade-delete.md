---
title: "Каскадное удаление - EF Core"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: ee8e14ec-2158-4c9c-96b5-118715e2ed9e
ms.technology: entity-framework-core
uid: core/saving/cascade-delete
ms.openlocfilehash: a9481fe851cc264ab3eaecad052c2e683ae57a44
ms.sourcegitcommit: 5367516f063cb42804ec92c31cdf76322554f2b5
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/08/2017
---
# <a name="cascade-delete"></a>Каскадное удаление

Каскадное удаление обычно используется для описания характеристик, которое допускает удаление строки автоматически активировать к удалению связанных строк в терминологии связанных баз данных. Тесно связанных понятие, также охвачен поведения удаления EF Core заключается в автоматическое удаление дочерней сущности, если это отношение к родительскому элементу было разорвано — это i часто называется «удаление потерянных».

EF Core реализует несколько разных delete поведения и позволяет настраивать поведение удаления отдельных связей. EF Core также реализует соглашения, которые автоматически настраивают поведения удаления полезные значения по умолчанию для каждой связи в зависимости от [requiredness связи] (.. /Modeling/Relationships.md#Required-and-Optional-Relationships).

## <a name="delete-behaviors"></a>Удаление поведения
Удалить поведения определяются в *DeleteBehavior* перечислителя типа и может быть передан *OnDelete* fluent API для управления ли Удаление участника или родительской сущности или прерывание из связь с зависимостью/дочерних сущностей должна иметь побочный эффект для зависимой/дочерних сущностей.

Существует три действия может занять EF при удалении участника или родительской сущности или разрыва связи в дочернюю.
* Можно удалить дочерний/зависимых
* Значение дочернего элемента значения внешнего ключа можно задать значение null
* Дочерние остается без изменений

> [!NOTE]  
> Поведение удаления, настроенный в базовой EF модели является применяются только в том случае, если основной сущности удаляется с помощью основных EF и зависимые сущности загружаются в память (т. е. для записанные зависимые элементы). У программы установки в базе данных, чтобы данные, не отслеживается контекстом необходимые действие, применяемое необходимо соответствующим поведением каскадное. EF Core используется для создания базы данных, это поведение cascade будет ли программа установки автоматически.

Для второго действия выше установив значение внешнего ключа значение null является недопустимым, если внешний ключ не допускает значения NULL. (Не допускающим внешний ключ соответствует необходимая связь.) В этих случаях EF Core отслеживает, что свойство внешнего ключа был помечен как null до вызова метода SaveChanges, после чего создается исключение из-за изменения не могут быть сохранены в базе данных. Это похоже на получение нарушение ограничения из базы данных.

Существует четыре удалить поведения, перечисленные в приведенной ниже таблице. Для связей, необязательные (допускающие значение NULL внешний ключ) его _—_ можно сохранить значение null, значение внешнего ключа, что приводит к следующему:

| Имя поведения | Влияет на зависимые потомки в памяти | Влияет на зависимые потомки в базе данных
|-|-|-
| **CASCADE** | Сущности будут удалены. | Сущности будут удалены.
| **ClientSetNull** (по умолчанию) | Свойства внешнего ключа устанавливаются в значение null | Нет
| **SetNull** | Свойства внешнего ключа устанавливаются в значение null | Свойства внешнего ключа устанавливаются в значение null
| **Ограничения** | Нет | Нет

Необходимые связи (допускающий внешний ключ) — _не_ можно сохранить значение null, значение внешнего ключа, что приводит к следующему:

| Имя поведения | Влияет на зависимые потомки в памяти | Влияет на зависимые потомки в базе данных
|-|-|-
| **CASCADE** (по умолчанию) | Сущности будут удалены. | Сущности будут удалены.
| **ClientSetNull** | Вызывает метод SaveChanges | Нет
| **SetNull** | Вызывает метод SaveChanges | Вызывает метод SaveChanges
| **Ограничения** | Нет | Нет

В таблицах выше *нет* может привести к нарушению ограничения. Например если основной и дочерней сущностью удаляется, но никакие действия не выполняются, чтобы изменить внешний ключ зависимые потомки, затем базу данных скорее всего вызовет на SaveChanges из-за нарушения ограничения внешнего.

На высоком уровне:
* Если у вас есть сущностей, которые не могут существовать без родительских и вы EF для обеспечения автоматического удаления дочерних элементов, а затем использовать *Cascade*.
  * Сущности, которые не могут существовать без родительской обычно сделать использование необходимые связи, для которого *Cascade* значение по умолчанию.
* Если сущности, которые не могут иметь родительский элемент, и вы EF, отвечающий за обнуление внешнего ключа для вас, а затем использовать *ClientSetNull*
  * Сущности, которые могут существовать без родительской обычно сделать использовать необязательный связей, для которого *ClientSetNull* значение по умолчанию.
  * Если требуется, чтобы базы данных также попытаться распространение значения null во внешних ключах дочерних даже при дочерней сущности не загружается, затем с помощью *SetNull*. Тем не менее Обратите внимание, что базы данных должен поддерживать это настройки базы данных, как это может привести к другие ограничения, что на практике часто делает этот параметр нецелесообразным. Именно поэтому *SetNull* не значение по умолчанию.
* Если вы не хотите Core EF, когда-либо автоматически удалить сущность или значение null, внешний ключ, автоматически, а затем использовать *ограничение*. Обратите внимание, что для этого необходимо, код синхронизации дочерних сущностей и их значения внешнего ключа вручную в противном случае ограничение исключения будут создаваться.

> [!NOTE]
> В ядре EF, в отличие от EF6 каскадный эффект не осуществляется немедленно, но вместо этого только, когда вызывается метод SaveChanges.

> [!NOTE]  
> **Изменения в версии 2.0 основные EF:** в предыдущих выпусках *ограничение* вызовет необязательные свойства внешнего ключа в записанные зависимые сущности будет присвоено значение null и был поведение необязательно связей удаления по умолчанию. В версии 2.0 основные EF *ClientSetNull* был введен для представления этого поведения и стала значение по умолчанию для связей, необязательно. Поведение *ограничение* была скорректирована для никогда не оказывает никакого влияния на зависимые сущности.

## <a name="entity-deletion-examples"></a>Примеры удаления сущности

В следующем примере кода является частью [пример](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/CascadeDelete/) , можно загрузить выполнения. В образце показано, что происходит для каждого вида поведения удаления для связи с обязательными и необязательными, при удалении родительской сущности.

[!code-csharp[Main](../../../samples/core/Saving/Saving/CascadeDelete/Sample.cs#DeleteBehaviorVariations)]

Давайте рассмотрим каждый вариант, чтобы понять, что произошло.

### <a name="deletebehaviorcascade-with-required-or-optional-relationship"></a>DeleteBehavior.Cascade с обязательными или необязательными связи

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

* Блог помечается как Deleted
* В блогах изначально оставить без изменений, поскольку каскадные проводится до SaveChanges
* SaveChanges производится пересылка операций удаления для зависимых элементов и дочерних элементов (сообщений) и затем основной или родительский (блог)
* После сохранения все сущности отсоединяются, так как они теперь были удалены из базы данных

### <a name="deletebehaviorclientsetnull-or-deletebehaviorsetnull-with-required-relationship"></a>DeleteBehavior.ClientSetNull или DeleteBehavior.SetNull с обязательную связь

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

* Блог помечается как Deleted
* В блогах изначально оставить без изменений, поскольку каскадные проводится до SaveChanges
* SaveChanges пытается установить post внешнего ключа значение null, но это не удается, так как внешнего ключа не допускает значения NULL

### <a name="deletebehaviorclientsetnull-or-deletebehaviorsetnull-with-optional-relationship"></a>DeleteBehavior.ClientSetNull или DeleteBehavior.SetNull с дополнительную связь

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

* Блог помечается как Deleted
* В блогах изначально оставить без изменений, поскольку каскадные проводится до SaveChanges
* Попыток SaveChanges внешнего ключа оба зависимых элементов и дочерних элементов (сообщений) задает значение null перед удалением основной или родительский (блог)
* После сохранения, основной или родительский (блог) удаляется, но по-прежнему отслеживаются зависимые объекты или дочерние элементы (сообщений)
* Записанные зависимые элементы и дочерние элементы (сообщений) теперь имеют значение null, значения внешнего ключа и их ссылки на удаленные основной или родительский (блог) был удален.

### <a name="deletebehaviorrestrict-with-required-or-optional-relationship"></a>DeleteBehavior.Restrict с обязательными или необязательными связи

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

* Блог помечается как Deleted
* В блогах изначально оставить без изменений, поскольку каскадные проводится до SaveChanges
* Поскольку *ограничение* сообщает EF, чтобы задать автоматически внешнего ключа null, он остается отличное от null и SaveChanges выдает без сохранения изменений

## <a name="delete-orphans-examples"></a>Обновлены примеры удаления

В следующем примере кода является частью [пример](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/CascadeDelete/) , можно загрузить выполнения. В этом примере, сопровождающие для каждого вида поведения удаления для связи с обязательными и необязательными разрыва связи между родительский или основной и его дочерние элементы и зависимости. В этом примере разрыва связи, удалив зависимые объекты или дочерние элементы (сообщений) из свойства навигации коллекции основной или родительский (блог). Тем не менее действует, если ссылка на основной или родительский зависимые потомки вместо очищается.

[!code-csharp[Main](../../../samples/core/Saving/Saving/CascadeDelete/Sample.cs#DeleteOrphansVariations)]

Давайте рассмотрим каждый вариант, чтобы понять, что произошло.

### <a name="deletebehaviorcascade-with-required-or-optional-relationship"></a>DeleteBehavior.Cascade с обязательными или необязательными связи

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

* В блогах помечаются как измененные, поскольку прерывание связи вызвал FK помечается как значение null
  * Если внешнего ключа не допускает значения NULL, затем фактическое значение не изменится даже если она помечена как допускающий значения null
* SaveChanges производится пересылка операций удаления для зависимых элементов и дочерних элементов (сообщений)
* После сохранения отсоединены зависимые объекты или дочерние элементы (сообщений), так как они теперь были удалены из базы данных

### <a name="deletebehaviorclientsetnull-or-deletebehaviorsetnull-with-required-relationship"></a>DeleteBehavior.ClientSetNull или DeleteBehavior.SetNull с обязательную связь

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

* В блогах помечаются как измененные, поскольку прерывание связи вызвал FK помечается как значение null
  * Если внешнего ключа не допускает значения NULL, затем фактическое значение не изменится даже если она помечена как допускающий значения null
* SaveChanges пытается установить post внешнего ключа значение null, но это не удается, так как внешнего ключа не допускает значения NULL

### <a name="deletebehaviorclientsetnull-or-deletebehaviorsetnull-with-optional-relationship"></a>DeleteBehavior.ClientSetNull или DeleteBehavior.SetNull с дополнительную связь

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

* В блогах помечаются как измененные, поскольку прерывание связи вызвал FK помечается как значение null
  * Если внешнего ключа не допускает значения NULL, затем фактическое значение не изменится даже если она помечена как допускающий значения null
* SaveChanges внешнего ключа оба зависимых элементов и дочерних элементов (сообщений) задает значение null
* После сохранения зависимые объекты или дочерние элементы (сообщений) теперь имеют значение null, значения внешнего ключа и их ссылки на удаленные основной или родительский (блог) был удален.

### <a name="deletebehaviorrestrict-with-required-or-optional-relationship"></a>DeleteBehavior.Restrict с обязательными или необязательными связи

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

* В блогах помечаются как измененные, поскольку прерывание связи вызвал FK помечается как значение null
  * Если внешнего ключа не допускает значения NULL, затем фактическое значение не изменится даже если она помечена как допускающий значения null
* Поскольку *ограничение* сообщает EF, чтобы задать автоматически внешнего ключа null, он остается отличное от null и SaveChanges выдает без сохранения изменений

## <a name="cascading-to-untracked-entities"></a>Каскадные отслеживаются сущностям

При вызове *SaveChanges*, каскадное удаление правил, которые будут применяться к любой сущности, которые отслеживаются контекстом. Ситуация во всех приведенных выше примеров, вот почему SQL был создан для удаления основной или родительский (блог) и все зависимые объекты или дочерние элементы (записей):

```sql
    DELETE FROM [Posts] WHERE [PostId] = 1
    DELETE FROM [Posts] WHERE [PostId] = 2
    DELETE FROM [Blogs] WHERE [BlogId] = 1
```

Если только участнику загружается — например, когда отправляется запрос для блога без `Include(b => b.Posts)` для включения в блогах — то SaveChanges только создаст SQL для удаления основной или родительский:

```sql
    DELETE FROM [Blogs] WHERE [BlogId] = 1
```

Зависимые объекты или дочерние элементы (сообщений) могут быть удалены только в том случае, если соответствующий поведение cascade настройки базы данных. EF используется для создания базы данных, это поведение cascade будет ли программа установки автоматически.
