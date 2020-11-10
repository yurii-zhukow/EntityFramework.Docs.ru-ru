---
title: Поставщик Azure Cosmos DB — ограничения — EF Core
description: Ограничения поставщика Entity Framework Core Azure Cosmos DB по сравнению с другими поставщиками
author: AndriySvyryd
ms.date: 11/05/2020
uid: core/providers/cosmos/limitations
ms.openlocfilehash: f7517954b854c8ea1020c039e490b4fb0dea7181
ms.sourcegitcommit: f3512e3a98e685a3ba409c1d0157ce85cc390cf4
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2020
ms.locfileid: "94430200"
---
# <a name="ef-core-azure-cosmos-db-provider-limitations"></a>Ограничения поставщика EF Core Azure Cosmos DB

У поставщика Cosmos есть ряд ограничений. Многие из этих ограничений являются следствием ограничений в базовом ядре СУБД Cosmos и не относятся к EF. Но большинство просто еще [не реализовано](https://github.com/dotnet/efcore/issues?page=1&q=is%3Aissue+is%3Aopen+Cosmos+in%3Atitle+label%3Atype-enhancement+sort%3Areactions-%2B1-desc).

## <a name="temporary-limitations"></a>Временные ограничения

- `Include` вызовы не поддерживаются
- `Join` вызовы не поддерживаются

## <a name="azure-cosmos-db-sdk-limitations"></a>Ограничения пакета SDK для Azure Cosmos DB

- Предоставляются только асинхронные методы

> [!WARNING]
> Поскольку отсутствуют версии синхронизации методов низкого уровня, EF Core полагается на, в настоящее время соответствующая функциональность реализуется путем вызова метода `.Wait()` на возвращенном `Task` . Это означает, что использование таких методов `SaveChanges` , как или, `ToList` а не их асинхронных аналогов, может привести к взаимоблокировке приложения.

## <a name="azure-cosmos-db-limitations"></a>Ограничения Azure Cosmos DB

Вы можете увидеть полный обзор [Azure Cosmos DB поддерживаемых функций](/azure/cosmos-db/modeling-data). это наиболее важные отличия по сравнению с реляционной базой данных.

- Инициированные клиентом транзакции не поддерживаются.
- Некоторые запросы между секциями выполняются медленнее в зависимости от вовлеченных операторов (например `Skip/Take` , или `OFFSET LIMIT` ).
