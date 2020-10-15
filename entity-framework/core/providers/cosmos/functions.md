---
title: Сопоставления функций — поставщик Azure Cosmos DB — EF Core
description: Сопоставления функций поставщика Azure Cosmos DB EF Core
author: bricelam
ms.date: 10/08/2020
uid: core/providers/cosmos/functions
ms.openlocfilehash: ffcf82fbe57ea16e6fc62f3c9c80c572b3e1ed91
ms.sourcegitcommit: 0a25c03fa65ae6e0e0e3f66bac48d59eceb96a5a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/14/2020
ms.locfileid: "92066536"
---
# <a name="function-mappings-of-the-azure-cosmos-db-ef-core-provider"></a>Сопоставления функций поставщика Azure Cosmos DB EF Core

На этой странице показано, какие члены .NET преобразуются в функции SQL при использовании поставщика Azure Cosmos DB.

.NET                          | SQL                              | Добавлено в
----------------------------- | -------------------------------- | --------
набор. Contains (элемент)     | @item ОКНЕ @collection
stringValue. Contains (значение)   | СОДЕРЖИТ ( @stringValue , @value )   | EF Core 5.0
stringValue. EndsWith (значение)   | ENDSWITH ( @stringValue , @value )   | EF Core 5.0
stringValue. FirstOrDefault ()  | LEFT ( @stringValue , 1)            | EF Core 5.0
stringValue. LastOrDefault ()   | RIGHT ( @stringValue , 1)           | EF Core 5.0
stringValue. StartsWith (значение) | STARTSWITH ( @stringValue , @value ) | EF Core 5.0
