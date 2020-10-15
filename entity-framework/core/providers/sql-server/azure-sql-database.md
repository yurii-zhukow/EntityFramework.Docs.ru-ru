---
title: Поставщик базы данных Microsoft SQL Server — параметры базы данных SQL Azure — EF Core
description: Как указать уровень служб и производительности для базы данных SQL Azure с помощью поставщика базы данных SQL Server Entity Framework Core
author: AndriySvyryd
ms.date: 11/05/2019
uid: core/providers/sql-server/azure-sql-database
ms.openlocfilehash: 9b2cc5e0790beb41295dcea6ae30c4ad15b537fa
ms.sourcegitcommit: 0a25c03fa65ae6e0e0e3f66bac48d59eceb96a5a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/14/2020
ms.locfileid: "92065372"
---
# <a name="specifying-azure-sql-database-options"></a>Указание параметров базы данных SQL Azure

>[!NOTE]
> Этот API впервые поEF Core 3,1.

База данных SQL Azure предоставляет [различные варианты ценообразования](https://azure.microsoft.com/pricing/details/sql-database/single/) , которые обычно настраиваются на портале Azure. Однако если вы управляете схемой с помощью [EF Coreных миграций](xref:core/managing-schemas/migrations/index) , вы можете указать нужные параметры в самой модели.

Уровень служб базы данных (выпуск) можно указать с помощью [хассервицетиер](/dotnet/api/Microsoft.EntityFrameworkCore.SqlServerModelBuilderExtensions.HasServiceTier):

[!code-csharp[HasServiceTier](../../../../samples/core/SqlServer/AzureDatabase/AzureSqlContext.cs?name=HasServiceTier)]

Максимальный размер базы данных можно указать с помощью [хасдатабасемакссизе](/dotnet/api/Microsoft.EntityFrameworkCore.SqlServerModelBuilderExtensions.HasDatabaseMaxSize):

[!code-csharp[HasDatabaseMaxSize](../../../../samples/core/SqlServer/AzureDatabase/AzureSqlContext.cs?name=HasDatabaseMaxSize)]

Можно указать уровень производительности базы данных (SERVICE_OBJECTIVE) с помощью [хасперформанцелевел](/dotnet/api/Microsoft.EntityFrameworkCore.SqlServerModelBuilderExtensions.HasPerformanceLevel):

[!code-csharp[HasPerformanceLevel](../../../../samples/core/SqlServer/AzureDatabase/AzureSqlContext.cs?name=HasPerformanceLevel)]

Используйте [хасперформанцелевелскл](/dotnet/api/Microsoft.EntityFrameworkCore.SqlServerModelBuilderExtensions.HasPerformanceLevelSql) для настройки эластичного пула, так как значение не является строковым литералом:

[!code-csharp[HasPerformanceLevel](../../../../samples/core/SqlServer/AzureDatabase/AzureSqlContext.cs?name=HasPerformanceLevelSql)]

>[!TIP]
> Все поддерживаемые значения можно найти в [документации по ALTER DATABASE](/sql/t-sql/statements/alter-database-transact-sql?view=azuresqldb-current&preserve-view=true).
