---
title: Поставщик базы данных Microsoft SQL Server — параметры базы данных SQL Azure — EF Core
description: Как указать уровень служб и производительности для базы данных SQL Azure с помощью поставщика базы данных SQL Server Entity Framework Core
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 11/05/2019
uid: core/providers/sql-server/azure-sql-database
ms.openlocfilehash: c4f7b91110a0e700ed06130661e611cf45bee05f
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/06/2020
ms.locfileid: "78414815"
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
> Все поддерживаемые значения можно найти в [документации по ALTER DATABASE](/sql/t-sql/statements/alter-database-transact-sql?view=azuresqldb-current).