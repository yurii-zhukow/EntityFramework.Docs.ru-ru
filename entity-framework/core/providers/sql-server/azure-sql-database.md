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
# <a name="specifying-azure-sql-database-options"></a><span data-ttu-id="612f9-103">Указание параметров базы данных SQL Azure</span><span class="sxs-lookup"><span data-stu-id="612f9-103">Specifying Azure SQL Database Options</span></span>

>[!NOTE]
> <span data-ttu-id="612f9-104">Этот API впервые поEF Core 3,1.</span><span class="sxs-lookup"><span data-stu-id="612f9-104">This API is new in EF Core 3.1.</span></span>

<span data-ttu-id="612f9-105">База данных SQL Azure предоставляет [различные варианты ценообразования](https://azure.microsoft.com/pricing/details/sql-database/single/) , которые обычно настраиваются на портале Azure.</span><span class="sxs-lookup"><span data-stu-id="612f9-105">Azure SQL Database provides [a variety of pricing options](https://azure.microsoft.com/pricing/details/sql-database/single/) that are usually configured through the Azure Portal.</span></span> <span data-ttu-id="612f9-106">Однако если вы управляете схемой с помощью [EF Coreных миграций](xref:core/managing-schemas/migrations/index) , вы можете указать нужные параметры в самой модели.</span><span class="sxs-lookup"><span data-stu-id="612f9-106">However if you are managing the schema using [EF Core migrations](xref:core/managing-schemas/migrations/index) you can specify the desired options in the model itself.</span></span>

<span data-ttu-id="612f9-107">Уровень служб базы данных (выпуск) можно указать с помощью [хассервицетиер](/dotnet/api/Microsoft.EntityFrameworkCore.SqlServerModelBuilderExtensions.HasServiceTier):</span><span class="sxs-lookup"><span data-stu-id="612f9-107">You can specify the service tier of the database (EDITION) using [HasServiceTier](/dotnet/api/Microsoft.EntityFrameworkCore.SqlServerModelBuilderExtensions.HasServiceTier):</span></span>

[!code-csharp[HasServiceTier](../../../../samples/core/SqlServer/AzureDatabase/AzureSqlContext.cs?name=HasServiceTier)]

<span data-ttu-id="612f9-108">Максимальный размер базы данных можно указать с помощью [хасдатабасемакссизе](/dotnet/api/Microsoft.EntityFrameworkCore.SqlServerModelBuilderExtensions.HasDatabaseMaxSize):</span><span class="sxs-lookup"><span data-stu-id="612f9-108">You can specify the maximum size of the database using [HasDatabaseMaxSize](/dotnet/api/Microsoft.EntityFrameworkCore.SqlServerModelBuilderExtensions.HasDatabaseMaxSize):</span></span>

[!code-csharp[HasDatabaseMaxSize](../../../../samples/core/SqlServer/AzureDatabase/AzureSqlContext.cs?name=HasDatabaseMaxSize)]

<span data-ttu-id="612f9-109">Можно указать уровень производительности базы данных (SERVICE_OBJECTIVE) с помощью [хасперформанцелевел](/dotnet/api/Microsoft.EntityFrameworkCore.SqlServerModelBuilderExtensions.HasPerformanceLevel):</span><span class="sxs-lookup"><span data-stu-id="612f9-109">You can specify the performance level of the database (SERVICE_OBJECTIVE) using [HasPerformanceLevel](/dotnet/api/Microsoft.EntityFrameworkCore.SqlServerModelBuilderExtensions.HasPerformanceLevel):</span></span>

[!code-csharp[HasPerformanceLevel](../../../../samples/core/SqlServer/AzureDatabase/AzureSqlContext.cs?name=HasPerformanceLevel)]

<span data-ttu-id="612f9-110">Используйте [хасперформанцелевелскл](/dotnet/api/Microsoft.EntityFrameworkCore.SqlServerModelBuilderExtensions.HasPerformanceLevelSql) для настройки эластичного пула, так как значение не является строковым литералом:</span><span class="sxs-lookup"><span data-stu-id="612f9-110">Use [HasPerformanceLevelSql](/dotnet/api/Microsoft.EntityFrameworkCore.SqlServerModelBuilderExtensions.HasPerformanceLevelSql) to configure the elastic pool, since the value is not a string literal:</span></span>

[!code-csharp[HasPerformanceLevel](../../../../samples/core/SqlServer/AzureDatabase/AzureSqlContext.cs?name=HasPerformanceLevelSql)]


>[!TIP]
> <span data-ttu-id="612f9-111">Все поддерживаемые значения можно найти в [документации по ALTER DATABASE](/sql/t-sql/statements/alter-database-transact-sql?view=azuresqldb-current).</span><span class="sxs-lookup"><span data-stu-id="612f9-111">You can find all the supported values in the [ALTER DATABASE documentation](/sql/t-sql/statements/alter-database-transact-sql?view=azuresqldb-current).</span></span>