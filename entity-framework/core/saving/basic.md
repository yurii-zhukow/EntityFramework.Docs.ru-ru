---
title: Основные Save - EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 850d842e-3fad-4ef2-be17-053768e97b9e
ms.technology: entity-framework-core
uid: core/saving/basic
ms.openlocfilehash: deead323301dc4a0ee0748b4536ddff4596b99e6
ms.sourcegitcommit: 4997314356118d0d97b04ad82e433e49bb9420a2
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/16/2018
---
# <a name="basic-save"></a>Основные сохранения

Описание способов добавления, изменения и удаления данных с помощью классов контекста и сущности.

> [!TIP]  
> Для этой статьи вы можете скачать [пример](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/Basics/) из репозитория GitHub.

## <a name="adding-data"></a>Добавление данных

Используйте *DbSet.Add* метод, чтобы добавить новые экземпляры классов сущностей. В базе данных будут вставляться данные, при вызове *SaveChanges*.

[!code-csharp[Main](../../../samples/core/Saving/Saving/Basics/Sample.cs#Add)]

> [!TIP]  
> Добавить, присоединить и обновление методы, работающие со полного графа сущностей, передаваемых в них, как описано в [связанные данные](related-data.md) раздела. Кроме того свойство EntityEntry.State, может использоваться для задания состояния одной сущности. Например, `context.Entry(blog).State = EntityState.Modified`.

## <a name="updating-data"></a>Обновление данных

EF автоматически обнаруживает изменения, внесенные в существующую сущность, которая отслеживается контекстом. Сюда входят сущности, которые можно загрузить или запрос из базы данных и сущностями, которые были ранее добавлены и сохранены в базе данных.

Просто измените значения свойств, а затем вызвать *SaveChanges*.

[!code-csharp[Main](../../../samples/core/Saving/Saving/Basics/Sample.cs#Update)]

## <a name="deleting-data"></a>Удаление данных

Используйте *DbSet.Remove* метод для удаления экземпляров вы классы сущностей.

Если сущность уже существует в базе данных, они будут удалены при *SaveChanges*. Если сущность не были сохранены в базе данных (т. е. он отслеживаются по мере добавления), то он будет удален из контекста и больше не будет вставлен при *SaveChanges* вызывается.

[!code-csharp[Main](../../../samples/core/Saving/Saving/Basics/Sample.cs#Remove)]

## <a name="multiple-operations-in-a-single-savechanges"></a>Несколько операций в одном SaveChanges

Можно объединить несколько операций добавить, обновить или удалить, в рамках одного вызова *SaveChanges*.

> [!NOTE]  
> Для большинства поставщиков базы данных *SaveChanges* является транзакционной. Это означает, что все операции либо успех или неудача и операции никогда не осталось применяются частично.

[!code-csharp[Main](../../../samples/core/Saving/Saving/Basics/Sample.cs#MultipleOperations)]
