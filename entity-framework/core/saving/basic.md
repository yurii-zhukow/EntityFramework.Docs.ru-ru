---
title: Базовое сохранение — EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 850d842e-3fad-4ef2-be17-053768e97b9e
ms.technology: entity-framework-core
uid: core/saving/basic
ms.openlocfilehash: deead323301dc4a0ee0748b4536ddff4596b99e6
ms.sourcegitcommit: 4997314356118d0d97b04ad82e433e49bb9420a2
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/16/2018
ms.locfileid: "31006667"
---
# <a name="basic-save"></a>Базовое сохранение

Узнайте, как добавлять, изменять и удалять данные, используя контекст и классы сущностей.

> [!TIP]  
> Для этой статьи вы можете скачать [пример](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/Basics/) из репозитория GitHub.

## <a name="adding-data"></a>Добавление данных

Добавлять новые экземпляры классов сущностей можно с помощью метода *DbSet.Add*. Данные будут вставлены в базу данных при вызове метода *SaveChanges*.

[!code-csharp[Main](../../../samples/core/Saving/Saving/Basics/Sample.cs#Add)]

> [!TIP]  
> Методы Add, Attach и Update работают с полным графом переданных им сущностей, как описано в статье [Сохранение связанных данных](related-data.md). В качестве альтернативы свойство EntityEntry.State можно использовать для установки состояния только одной сущности. Например, `context.Entry(blog).State = EntityState.Modified`.

## <a name="updating-data"></a>Обновление данных

EF автоматически обнаружит изменения, внесенные в существующую сущность, которая отслеживается контекстом. Сюда входят сущности, которые вы загружаете или запрашиваете из базы данных, и сущности, которые ранее были добавлены и сохранены в базе данных.

Просто измените значения, присвоенные свойствам, а затем вызовите метод *SaveChanges*.

[!code-csharp[Main](../../../samples/core/Saving/Saving/Basics/Sample.cs#Update)]

## <a name="deleting-data"></a>Удаление данных

С помощью метода *DbSet.Remove* можно удалять экземпляры классов сущностей.

Если сущность уже существует в базе данных, она будет удалена во время выполнения метода *SaveChanges*. Если сущность еще не сохранена в базе данных (т. е. она отслеживается как добавленная), она будет удалена из контекста и больше не будет вставляться при вызове метода *SaveChanges*.

[!code-csharp[Main](../../../samples/core/Saving/Saving/Basics/Sample.cs#Remove)]

## <a name="multiple-operations-in-a-single-savechanges"></a>Несколько операций в одном методе SaveChanges

Вы можете объединить несколько операций Add, Update и Remove в один вызов метода *SaveChanges*.

> [!NOTE]  
> Для большинства поставщиков баз данных метод *SaveChanges* является транзакционным. Это означает, что все операции будут либо успешными, либо неудачными и никогда не будут частично применены.

[!code-csharp[Main](../../../samples/core/Saving/Saving/Basics/Sample.cs#MultipleOperations)]
