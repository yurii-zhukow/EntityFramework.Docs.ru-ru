---
title: Базовое сохранение — EF Core
description: Основные сведения о добавлении, обновлении и удалении данных с помощью Entity Framework Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 850d842e-3fad-4ef2-be17-053768e97b9e
uid: core/saving/basic
ms.openlocfilehash: 21a9be10d081591a7d3b33a8e9cc48552c95fb6c
ms.sourcegitcommit: 7c3939504bb9da3f46bea3443638b808c04227c2
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/09/2020
ms.locfileid: "89617429"
---
# <a name="basic-save"></a>Базовое сохранение

Узнайте, как добавлять, изменять и удалять данные, используя контекст и классы сущностей.

> [!TIP]  
> Для этой статьи вы можете скачать [пример](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Saving/Basics/) из репозитория GitHub.

## <a name="adding-data"></a>Добавление данных

Добавлять новые экземпляры классов сущностей можно с помощью метода *DbSet.Add*. Данные будут вставлены в базу данных при вызове метода *SaveChanges*.

[!code-csharp[Main](../../../samples/core/Saving/Basics/Sample.cs#Add)]

> [!TIP]  
> Методы Add, Attach и Update работают с полным графом переданных им сущностей, как описано в статье [Сохранение связанных данных](xref:core/saving/related-data). В качестве альтернативы свойство EntityEntry.State можно использовать для установки состояния только одной сущности. Например, `context.Entry(blog).State = EntityState.Modified`.

## <a name="updating-data"></a>Обновление данных

EF автоматически обнаружит изменения, внесенные в существующую сущность, которая отслеживается контекстом. Сюда входят сущности, которые вы загружаете или запрашиваете из базы данных, и сущности, которые ранее были добавлены и сохранены в базе данных.

Просто измените значения, присвоенные свойствам, а затем вызовите метод *SaveChanges*.

[!code-csharp[Main](../../../samples/core/Saving/Basics/Sample.cs#Update)]

## <a name="deleting-data"></a>Удаление данных

С помощью метода *DbSet.Remove* можно удалять экземпляры классов сущностей.

Если сущность уже существует в базе данных, она будет удалена во время выполнения метода *SaveChanges*. Если сущность еще не сохранена в базе данных (т. е. она отслеживается как добавленная), она будет удалена из контекста и больше не будет вставляться при вызове метода *SaveChanges*.

[!code-csharp[Main](../../../samples/core/Saving/Basics/Sample.cs#Remove)]

## <a name="multiple-operations-in-a-single-savechanges"></a>Несколько операций в одном методе SaveChanges

Вы можете объединить несколько операций Add, Update и Remove в один вызов метода *SaveChanges*.

> [!NOTE]  
> Для большинства поставщиков баз данных метод *SaveChanges* является транзакционным. Это означает, что все операции будут либо успешными, либо неудачными и никогда не будут частично применены.

[!code-csharp[Main](../../../samples/core/Saving/Basics/Sample.cs#MultipleOperations)]
