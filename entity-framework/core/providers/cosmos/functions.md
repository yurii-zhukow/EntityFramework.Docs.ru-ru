---
title: Сопоставления функций — поставщик Azure Cosmos DB — EF Core
description: Сопоставления функций поставщика Azure Cosmos DB EF Core
author: bricelam
ms.date: 1/26/2021
uid: core/providers/cosmos/functions
ms.openlocfilehash: d4d45ba7db50befb5eb775feed0af44468ee3f74
ms.sourcegitcommit: 704240349e18b6404e5a809f5b7c9d365b152e2e
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/16/2021
ms.locfileid: "100543592"
---
# <a name="function-mappings-of-the-azure-cosmos-db-ef-core-provider"></a>Сопоставления функций поставщика Azure Cosmos DB EF Core

На этой странице показано, какие члены .NET преобразуются в функции SQL при использовании поставщика Azure Cosmos DB.

.NET                          | SQL                              | Добавлено в
----------------------------- | -------------------------------- | --------
набор. Contains (элемент)     | @item ОКНЕ @collection
#. Functions. Random ()         | RAND ()                           | EF Core 6.0
stringValue. Contains (значение)   | СОДЕРЖИТ ( @stringValue , @value )   | EF Core 5.0
stringValue. EndsWith (значение)   | ENDSWITH ( @stringValue , @value )   | EF Core 5.0
stringValue. FirstOrDefault ()  | LEFT ( @stringValue , 1)            | EF Core 5.0
stringValue. LastOrDefault ()   | RIGHT ( @stringValue , 1)           | EF Core 5.0
stringValue. StartsWith (значение) | STARTSWITH ( @stringValue , @value ) | EF Core 5.0
