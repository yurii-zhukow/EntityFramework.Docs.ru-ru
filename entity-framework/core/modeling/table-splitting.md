---
title: Разбиение таблицы — EF Core
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 04/10/2019
ms.assetid: 0EC2CCE1-BD55-45D8-9EA9-20634987F094
uid: core/modeling/table-splitting
ms.openlocfilehash: 684fcfbb66debfd1b89e23c8aaf0a32909378c6b
ms.sourcegitcommit: cbaa6cc89bd71d5e0bcc891e55743f0e8ea3393b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/20/2019
ms.locfileid: "71149187"
---
# <a name="table-splitting"></a>Разделение таблиц

>[!NOTE]
> Эта функция впервые реализована в EF Core 2,0.

EF Core позволяет сопоставлять две или более сущностей с одной строкой. Это называется _разбиением таблицы_ или _совместно используемой таблицей_.

## <a name="configuration"></a>Параметр Configuration

Чтобы использовать разделение таблиц, типы сущностей должны быть сопоставлены с одной и той же таблицей, иметь первичные ключи, сопоставленные с теми же столбцами и по крайней мере одну связь между первичным ключом одного типа сущности и другой в той же таблице.

Распространенным сценарием разбиения таблицы является использование только подмножества столбцов в таблице для повышения производительности или инкапсуляции.

В этом примере `Order` представляет `DetailedOrder`подмножество.

[!code-csharp[Order](../../../samples/core/Modeling/TableSplitting/Order.cs?name=Order)]

[!code-csharp[DetailedOrder](../../../samples/core/Modeling/TableSplitting/DetailedOrder.cs?name=DetailedOrder)]

В дополнение к требуемой конфигурации, которую `Property(o => o.Status).HasColumnName("Status")` мы вызываем `DetailedOrder.Status` , сопоставлены с `Order.Status`тем же столбцом, что и.

[!code-csharp[TableSplittingConfiguration](../../../samples/core/Modeling/TableSplitting/TableSplittingContext.cs?name=TableSplitting&highlight=3)]

> [!TIP]
> См. [полный пример проекта](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Modeling/TableSplitting) для получения дополнительных контекстов.

## <a name="usage"></a>Использование

Сохранение и запрос сущностей с помощью разбиения таблицы выполняется так же, как и другие сущности. И начиная с EF Core 3,0, ссылка на зависимую сущность может иметь `null`значение. Если все столбцы, используемые зависимой сущностью, являются `NULL` базой данных, то при запросе не будет создаваться экземпляр для нее. Это также приводит к `null`необязательным значениям свойств и имеет значение, которое может быть непредсказуемым.

[!code-csharp[Usage](../../../samples/core/Modeling/TableSplitting/Program.cs?name=Usage)]

## <a name="concurrency-tokens"></a>Маркеры параллелизма

Если любой из типов сущностей, совместно использующий таблицу, имеет маркер параллелизма, он должен быть включен во все остальные типы сущностей, чтобы избежать устаревшего значения маркера параллелизма при обновлении только одной из сущностей, сопоставленных с той же таблицей.

Чтобы не допустить использования этого кода в коде, его можно создать в теневом состоянии.

[!code-csharp[TableSplittingConfiguration](../../../samples/core/Modeling/TableSplitting/TableSplittingContext.cs?name=ConcurrencyToken&highlight=2)]