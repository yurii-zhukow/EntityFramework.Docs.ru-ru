---
title: Разбиение таблицы — EF Core
description: Настройка разделения таблиц с помощью Entity Framework Core
author: AndriySvyryd
ms.date: 01/03/2020
uid: core/modeling/table-splitting
ms.openlocfilehash: 43261ea0dab57528ab14faf7f455c0ed72d1bfc3
ms.sourcegitcommit: 4798ab8d04c1fdbe6dd204d94d770fcbf309d09b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/11/2021
ms.locfileid: "103023930"
---
# <a name="table-splitting"></a>Разделение таблиц

EF Core позволяет сопоставлять две или более сущностей с одной строкой. Это называется _разбиением таблицы_ или _совместно используемой таблицей_.

## <a name="configuration"></a>Конфигурация

Чтобы использовать разделение таблиц, типы сущностей должны быть сопоставлены с одной и той же таблицей, иметь первичные ключи, сопоставленные с теми же столбцами и по крайней мере одну связь между первичным ключом одного типа сущности и другой в той же таблице.

Распространенным сценарием разбиения таблицы является использование только подмножества столбцов в таблице для повышения производительности или инкапсуляции.

В этом примере `Order` представляет подмножество `DetailedOrder` .

[!code-csharp[Order](../../../samples/core/Modeling/TableSplitting/Order.cs?name=Order)]

[!code-csharp[DetailedOrder](../../../samples/core/Modeling/TableSplitting/DetailedOrder.cs?name=DetailedOrder)]

В дополнение к требуемой конфигурации, которую мы вызываем `Property(o => o.Status).HasColumnName("Status")` , сопоставлены с `DetailedOrder.Status` тем же столбцом, что и `Order.Status` .

[!code-csharp[TableSplittingConfiguration](../../../samples/core/Modeling/TableSplitting/TableSplittingContext.cs?name=TableSplitting)]

> [!TIP]
> См. [полный пример проекта](https://github.com/dotnet/EntityFramework.Docs/tree/main/samples/core/Modeling/TableSplitting) для получения дополнительных контекстов.

## <a name="usage"></a>Использование

Сохранение и запрос сущностей с помощью разбиения таблицы выполняется так же, как и для других сущностей:

[!code-csharp[Usage](../../../samples/core/Modeling/TableSplitting/Program.cs?name=Usage)]

## <a name="optional-dependent-entity"></a>Необязательная зависимая сущность

> [!NOTE]
> Эта возможность появилась в EF Core 3.0.

Если все столбцы, используемые зависимой сущностью, находятся `NULL` в базе данных, то при запросе не будет создано ни одного экземпляра. Это позволяет моделировать необязательную зависимую сущность, где свойство связи в участнике будет иметь значение null. Обратите внимание, что это также происходит, если все свойства зависимых объектов являются необязательными и имеют значение `null` , которое может быть не ожидаемым.

## <a name="concurrency-tokens"></a>Маркеры параллелизма

Если любой из типов сущностей, совместно использующий таблицу, имеет маркер параллелизма, он также должен включаться во все другие типы сущностей. Это необходимо для того, чтобы избежать устаревшего значения маркера параллелизма при обновлении только одной из сущностей, сопоставленных с одной и той же таблицей.

Чтобы предотвратить предоставление маркера параллелизма для кода, можно создать его как [свойство теневого копирования](xref:core/modeling/shadow-properties):

[!code-csharp[TableSplittingConfiguration](../../../samples/core/Modeling/TableSplitting/TableSplittingContext.cs?name=ConcurrencyToken&highlight=2)]
