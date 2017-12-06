---
title: "Основные Save - EF Core"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 850d842e-3fad-4ef2-be17-053768e97b9e
ms.technology: entity-framework-core
uid: core/saving/basic
ms.openlocfilehash: e99d755b8f7c42b15a73cfdb6a2f8999a405a739
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/27/2017
---
# <a name="basic-save"></a>Основные сохранения

Описание способов добавления, изменения и удаления данных с помощью классов контекста и сущности.

> [!TIP]  
> Можно просмотреть в этой статье [пример](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/Basics/) на GitHub.

## <a name="adding-data"></a>Добавление данных

Используйте *DbSet.Add* метод, чтобы добавить новые экземпляры классов сущностей. В базе данных будут вставляться данные, при вызове *SaveChanges*.

[!code-csharp[Main](../../../samples/core/Saving/Saving/Basics/Sample.cs#Add)]

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
