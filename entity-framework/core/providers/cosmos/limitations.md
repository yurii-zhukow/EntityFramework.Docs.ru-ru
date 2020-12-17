---
title: Поставщик Azure Cosmos DB — ограничения — EF Core
description: Ограничения поставщика Entity Framework Core Azure Cosmos DB по сравнению с другими поставщиками
author: AndriySvyryd
ms.date: 11/05/2020
uid: core/providers/cosmos/limitations
ms.openlocfilehash: 088f593dddd9b5691d87566d7e31a699ba90d7c5
ms.sourcegitcommit: 4860d036ea0fb392c28799907bcc924c987d2d7b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/17/2020
ms.locfileid: "97635722"
---
# <a name="ef-core-azure-cosmos-db-provider-limitations"></a>Ограничения поставщика EF Core Azure Cosmos DB

У поставщика Cosmos есть ряд ограничений. Многие из этих ограничений являются следствием ограничений в базовом ядре СУБД Cosmos и не относятся к EF. Но большинство просто еще [не реализовано](https://github.com/dotnet/efcore/issues?page=1&q=is%3Aissue+is%3Aopen+Cosmos+in%3Atitle+label%3Atype-enhancement+sort%3Areactions-%2B1-desc).

Ниже перечислены некоторые из наиболее часто запрашиваемых компонентов.

- [`Include` технической](https://github.com/dotnet/efcore/issues/16920)
- [`Join` технической](https://github.com/dotnet/efcore/issues/17314)
- [Поддержка коллекций типов-примитивов](https://github.com/dotnet/efcore/issues/14762)

## <a name="azure-cosmos-db-sdk-limitations"></a>Ограничения пакета SDK для Azure Cosmos DB

- Предоставляются только асинхронные методы

> [!WARNING]
> Поскольку отсутствуют версии синхронизации методов низкого уровня, EF Core полагается на, в настоящее время соответствующая функциональность реализуется путем вызова метода `.Wait()` на возвращенном `Task` . Это означает, что использование таких методов `SaveChanges` , как или, `ToList` а не их асинхронных аналогов, может привести к взаимоблокировке приложения.

## <a name="azure-cosmos-db-limitations"></a>Ограничения Azure Cosmos DB

Вы можете увидеть полный обзор [Azure Cosmos DB поддерживаемых функций](/azure/cosmos-db/modeling-data). это наиболее важные отличия по сравнению с реляционной базой данных.

- Инициированные клиентом транзакции не поддерживаются.
- Некоторые запросы между секциями выполняются медленнее в зависимости от вовлеченных операторов (например `Skip/Take` , или `OFFSET LIMIT` ).
