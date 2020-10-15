---
title: Поставщик Azure Cosmos DB — ограничения — EF Core
description: Ограничения поставщика Entity Framework Core Azure Cosmos DB по сравнению с другими поставщиками
author: AndriySvyryd
ms.date: 11/05/2019
uid: core/providers/cosmos/limitations
ms.openlocfilehash: e4d1c38da14d1e722797f8543313a69c7fb088cc
ms.sourcegitcommit: 0a25c03fa65ae6e0e0e3f66bac48d59eceb96a5a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/14/2020
ms.locfileid: "92064041"
---
# <a name="ef-core-azure-cosmos-db-provider-limitations"></a>Ограничения поставщика EF Core Azure Cosmos DB

У поставщика Cosmos есть ряд ограничений. Многие из этих ограничений являются следствием ограничений в базовом ядре СУБД Cosmos и не относятся к EF. Но большинство просто еще [не реализовано](https://github.com/aspnet/EntityFrameworkCore/issues?page=1&q=is%3Aissue+is%3Aopen+Cosmos+in%3Atitle+label%3Atype-enhancement+sort%3Areactions-%2B1-desc).

## <a name="temporary-limitations"></a>Временные ограничения

- Даже если существует только один тип сущности без наследования, сопоставленный с контейнером, он по-прежнему имеет свойство дискриминатора.
- Типы сущностей с ключами секций неправильно работают в некоторых сценариях
- `Include` вызовы не поддерживаются
- `Join` вызовы не поддерживаются

## <a name="azure-cosmos-db-sdk-limitations"></a>Ограничения пакета SDK для Azure Cosmos DB

- Предоставляются только асинхронные методы

> [!WARNING]
> Поскольку отсутствуют версии синхронизации методов низкого уровня, EF Core полагается на, в настоящее время соответствующая функциональность реализуется путем вызова метода `.Wait()` на возвращенном `Task` . Это означает, что использование таких методов `SaveChanges` , как или, `ToList` а не их асинхронных аналогов, может привести к взаимоблокировке приложения.

## <a name="azure-cosmos-db-limitations"></a>Ограничения Azure Cosmos DB

Вы можете увидеть полный обзор [Azure Cosmos DB поддерживаемых функций](/azure/cosmos-db/modeling-data). это наиболее важные отличия по сравнению с реляционной базой данных.

- Инициированные клиентом транзакции не поддерживаются.
- Некоторые запросы между секциями либо не поддерживаются, либо выполняются слишком медленно в зависимости от используемых операторов
