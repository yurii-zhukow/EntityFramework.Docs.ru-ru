---
title: Поставщик Azure Cosmos DB — ограничения — EF Core
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 09/12/2019
ms.assetid: 9d02a2cd-484e-4687-b8a8-3748ba46dbc9
uid: core/providers/cosmos/limitations
ms.openlocfilehash: 8dcc82a68c89e21ad1902a0bbbce8ebbc3535801
ms.sourcegitcommit: cbaa6cc89bd71d5e0bcc891e55743f0e8ea3393b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/20/2019
ms.locfileid: "71150767"
---
# <a name="ef-core-azure-cosmos-db-provider-limitations"></a>Ограничения поставщика EF Core Azure Cosmos DB

У поставщика Cosmos есть ряд ограничений. Многие из этих ограничений являются следствием ограничений в базовом ядре СУБД Cosmos и не относятся к EF. Но большинство просто еще [не реализовано](https://github.com/aspnet/EntityFrameworkCore/issues?page=1&q=is%3Aissue+is%3Aopen+Cosmos+in%3Atitle+label%3Atype-enhancement+sort%3Areactions-%2B1-desc).

## <a name="temporary-limitations"></a>Временные ограничения

- Даже если существует только один тип сущности без наследования, сопоставленный с контейнером, он по-прежнему имеет свойство дискриминатора.
- Типы сущностей с ключами секций неправильно работают в некоторых сценариях
- `Include`вызовы не поддерживаются
- `Join`вызовы не поддерживаются

## <a name="azure-cosmos-db-sdk-limitations"></a>Ограничения пакета SDK для Azure Cosmos DB

- Предоставляются только асинхронные методы

> [!WARNING]
> Поскольку отсутствуют версии синхронизации методов низкого уровня, EF Core полагается на, в настоящее время соответствующая функциональность реализуется путем вызова `.Wait()` метода на возвращенном. `Task` Это означает, что использование таких `SaveChanges`методов, `ToList` как или, а не их асинхронных аналогов, может привести к взаимоблокировке приложения.

## <a name="azure-cosmos-db-limitations"></a>Ограничения Azure Cosmos DB

Вы можете увидеть полный обзор [Azure Cosmos DB поддерживаемых функций](https://docs.microsoft.com/en-us/azure/cosmos-db/modeling-data). это наиболее важные отличия по сравнению с реляционной базой данных.

- Инициированные клиентом транзакции не поддерживаются.
- Некоторые запросы между секциями либо не поддерживаются, либо выполняются слишком медленно в зависимости от используемых операторов