---
title: Таблицы разделения — EF Core
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 04/10/2019
ms.assetid: 0EC2CCE1-BD55-45D8-9EA9-20634987F094
uid: core/modeling/table-splitting
ms.openlocfilehash: 4a0bfaf017106a0bfdff084b1c472bdc17459a89
ms.sourcegitcommit: 8f801993c9b8cd8a8fbfa7134818a8edca79e31a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/14/2019
ms.locfileid: "59562594"
---
# <a name="table-splitting"></a>Разбиение таблиц

>[!NOTE]
> Этот компонент впервые появился в EF Core 2.0.

EF Core позволяет сопоставить два или несколько сущностей с отдельной строкой. Это называется _таблицы разделение_ или _общий доступ к таблице_.

## <a name="configuration"></a>Параметр Configuration

Чтобы использовать разделение таблицы, типы сущностей должны быть добавлены к одной таблице, имеют первичные ключи, сопоставляются с теми же столбцами и по крайней мере одну связь, настроен между первичного ключа из одного типа сущности в той же таблице.

Распространенный сценарий для разбиения таблицы используется только подмножество столбцов в таблице больше производительности или инкапсуляции.

В этом примере `Order` представляет собой подмножество `DetailedOrder`.

[!code-csharp[Order](../../../samples/core/Modeling/TableSplitting/Order.cs?name=Order)]

[!code-csharp[DetailedOrder](../../../samples/core/Modeling/TableSplitting/DetailedOrder.cs?name=DetailedOrder)]

В дополнение к требуемой конфигурации мы вызываем `HasBaseType((string)null)` во избежание сопоставление `DetailedOrder` в той же иерархии, что `Order`.

[!code-csharp[TableSplittingConfiguration](../../../samples/core/Modeling/TableSplitting/TableSplittingContext.cs?name=TableSplitting&highlight=3)]

См. в разделе [полный пример проекта](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Modeling/TableSplitting) дополнительный контекст.

## <a name="usage"></a>Использование

Сохранение и запрос сущностей с помощью разбиения таблицы осуществляется так же как и для других сущностей, отличается только что требуется отслеживать все сущности, совместное использование строки вставки.

[!code-csharp[Usage](../../../samples/core/Modeling/TableSplitting/Program.cs?name=Usage)]

## <a name="concurrency-tokens"></a>Маркеры параллелизма

Если любой из типов сущностей, общий доступ к таблице имеется маркер параллелизма затем он должен быть включен в всех других типов сущностей, чтобы избежать значение токена устаревших параллелизма при обновлении только одной сущности сопоставляется с той же таблице.

Чтобы избежать предоставление ее в коде-потребителе возможна создайте одну в состоянии тени.

[!code-csharp[TableSplittingConfiguration](../../../samples/core/Modeling/TableSplitting/TableSplittingContext.cs?name=ConcurrencyToken&highlight=2)]