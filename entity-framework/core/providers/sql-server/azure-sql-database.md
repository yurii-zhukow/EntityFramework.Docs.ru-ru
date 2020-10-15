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
# <a name="specifying-azure-sql-database-options"></a><span data-ttu-id="d3448-103">Указание параметров базы данных SQL Azure</span><span class="sxs-lookup"><span data-stu-id="d3448-103">Specifying Azure SQL Database Options</span></span>

>[!NOTE]
> <span data-ttu-id="d3448-104">Этот API впервые поEF Core 3,1.</span><span class="sxs-lookup"><span data-stu-id="d3448-104">This API is new in EF Core 3.1.</span></span>

<span data-ttu-id="d3448-105">База данных SQL Azure предоставляет [различные варианты ценообразования](https://azure.microsoft.com/pricing/details/sql-database/single/) , которые обычно настраиваются на портале Azure.</span><span class="sxs-lookup"><span data-stu-id="d3448-105">Azure SQL Database provides [a variety of pricing options](https://azure.microsoft.com/pricing/details/sql-database/single/) that are usually configured through the Azure Portal.</span></span> <span data-ttu-id="d3448-106">Однако если вы управляете схемой с помощью [EF Coreных миграций](xref:core/managing-schemas/migrations/index) , вы можете указать нужные параметры в самой модели.</span><span class="sxs-lookup"><span data-stu-id="d3448-106">However if you are managing the schema using [EF Core migrations](xref:core/managing-schemas/migrations/index) you can specify the desired options in the model itself.</span></span>

<span data-ttu-id="d3448-107">Уровень служб базы данных (выпуск) можно указать с помощью [хассервицетиер](/dotnet/api/Microsoft.EntityFrameworkCore.SqlServerModelBuilderExtensions.HasServiceTier):</span><span class="sxs-lookup"><span data-stu-id="d3448-107">You can specify the service tier of the database (EDITION) using [HasServiceTier](/dotnet/api/Microsoft.EntityFrameworkCore.SqlServerModelBuilderExtensions.HasServiceTier):</span></span>

[!code-csharp[HasServiceTier](../../../../samples/core/SqlServer/AzureDatabase/AzureSqlContext.cs?name=HasServiceTier)]

<span data-ttu-id="d3448-108">Максимальный размер базы данных можно указать с помощью [хасдатабасемакссизе](/dotnet/api/Microsoft.EntityFrameworkCore.SqlServerModelBuilderExtensions.HasDatabaseMaxSize):</span><span class="sxs-lookup"><span data-stu-id="d3448-108">You can specify the maximum size of the database using [HasDatabaseMaxSize](/dotnet/api/Microsoft.EntityFrameworkCore.SqlServerModelBuilderExtensions.HasDatabaseMaxSize):</span></span>

[!code-csharp[HasDatabaseMaxSize](../../../../samples/core/SqlServer/AzureDatabase/AzureSqlContext.cs?name=HasDatabaseMaxSize)]

<span data-ttu-id="d3448-109">Можно указать уровень производительности базы данных (SERVICE_OBJECTIVE) с помощью [хасперформанцелевел](/dotnet/api/Microsoft.EntityFrameworkCore.SqlServerModelBuilderExtensions.HasPerformanceLevel):</span><span class="sxs-lookup"><span data-stu-id="d3448-109">You can specify the performance level of the database (SERVICE_OBJECTIVE) using [HasPerformanceLevel](/dotnet/api/Microsoft.EntityFrameworkCore.SqlServerModelBuilderExtensions.HasPerformanceLevel):</span></span>

[!code-csharp[HasPerformanceLevel](../../../../samples/core/SqlServer/AzureDatabase/AzureSqlContext.cs?name=HasPerformanceLevel)]

<span data-ttu-id="d3448-110">Используйте [хасперформанцелевелскл](/dotnet/api/Microsoft.EntityFrameworkCore.SqlServerModelBuilderExtensions.HasPerformanceLevelSql) для настройки эластичного пула, так как значение не является строковым литералом:</span><span class="sxs-lookup"><span data-stu-id="d3448-110">Use [HasPerformanceLevelSql](/dotnet/api/Microsoft.EntityFrameworkCore.SqlServerModelBuilderExtensions.HasPerformanceLevelSql) to configure the elastic pool, since the value is not a string literal:</span></span>

[!code-csharp[HasPerformanceLevel](../../../../samples/core/SqlServer/AzureDatabase/AzureSqlContext.cs?name=HasPerformanceLevelSql)]

>[!TIP]
> <span data-ttu-id="d3448-111">Все поддерживаемые значения можно найти в [документации по ALTER DATABASE](/sql/t-sql/statements/alter-database-transact-sql?view=azuresqldb-current&preserve-view=true).</span><span class="sxs-lookup"><span data-stu-id="d3448-111">You can find all the supported values in the [ALTER DATABASE documentation](/sql/t-sql/statements/alter-database-transact-sql?view=azuresqldb-current&preserve-view=true).</span></span>
