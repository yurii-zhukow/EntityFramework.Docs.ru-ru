---
title: Поставщик базы данных Microsoft SQL Server — параметры базы данных SQL Azure — EF Core
description: Как указать уровень служб и производительности для базы данных SQL Azure с помощью поставщика базы данных SQL Server Entity Framework Core
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 11/05/2019
uid: core/providers/sql-server/azure-sql-database
ms.openlocfilehash: 3863a5a224ba26df8cb319844bc2af4158d2497a
ms.sourcegitcommit: 7c3939504bb9da3f46bea3443638b808c04227c2
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/09/2020
ms.locfileid: "89618950"
---
# <a name="specifying-azure-sql-database-options"></a><span data-ttu-id="fe649-103">Указание параметров базы данных SQL Azure</span><span class="sxs-lookup"><span data-stu-id="fe649-103">Specifying Azure SQL Database Options</span></span>

>[!NOTE]
> <span data-ttu-id="fe649-104">Этот API впервые поEF Core 3,1.</span><span class="sxs-lookup"><span data-stu-id="fe649-104">This API is new in EF Core 3.1.</span></span>

<span data-ttu-id="fe649-105">База данных SQL Azure предоставляет [различные варианты ценообразования](https://azure.microsoft.com/pricing/details/sql-database/single/) , которые обычно настраиваются на портале Azure.</span><span class="sxs-lookup"><span data-stu-id="fe649-105">Azure SQL Database provides [a variety of pricing options](https://azure.microsoft.com/pricing/details/sql-database/single/) that are usually configured through the Azure Portal.</span></span> <span data-ttu-id="fe649-106">Однако если вы управляете схемой с помощью [EF Coreных миграций](xref:core/managing-schemas/migrations/index) , вы можете указать нужные параметры в самой модели.</span><span class="sxs-lookup"><span data-stu-id="fe649-106">However if you are managing the schema using [EF Core migrations](xref:core/managing-schemas/migrations/index) you can specify the desired options in the model itself.</span></span>

<span data-ttu-id="fe649-107">Уровень служб базы данных (выпуск) можно указать с помощью [хассервицетиер](/dotnet/api/Microsoft.EntityFrameworkCore.SqlServerModelBuilderExtensions.HasServiceTier):</span><span class="sxs-lookup"><span data-stu-id="fe649-107">You can specify the service tier of the database (EDITION) using [HasServiceTier](/dotnet/api/Microsoft.EntityFrameworkCore.SqlServerModelBuilderExtensions.HasServiceTier):</span></span>

[!code-csharp[HasServiceTier](../../../../samples/core/SqlServer/AzureDatabase/AzureSqlContext.cs?name=HasServiceTier)]

<span data-ttu-id="fe649-108">Максимальный размер базы данных можно указать с помощью [хасдатабасемакссизе](/dotnet/api/Microsoft.EntityFrameworkCore.SqlServerModelBuilderExtensions.HasDatabaseMaxSize):</span><span class="sxs-lookup"><span data-stu-id="fe649-108">You can specify the maximum size of the database using [HasDatabaseMaxSize](/dotnet/api/Microsoft.EntityFrameworkCore.SqlServerModelBuilderExtensions.HasDatabaseMaxSize):</span></span>

[!code-csharp[HasDatabaseMaxSize](../../../../samples/core/SqlServer/AzureDatabase/AzureSqlContext.cs?name=HasDatabaseMaxSize)]

<span data-ttu-id="fe649-109">Можно указать уровень производительности базы данных (SERVICE_OBJECTIVE) с помощью [хасперформанцелевел](/dotnet/api/Microsoft.EntityFrameworkCore.SqlServerModelBuilderExtensions.HasPerformanceLevel):</span><span class="sxs-lookup"><span data-stu-id="fe649-109">You can specify the performance level of the database (SERVICE_OBJECTIVE) using [HasPerformanceLevel](/dotnet/api/Microsoft.EntityFrameworkCore.SqlServerModelBuilderExtensions.HasPerformanceLevel):</span></span>

[!code-csharp[HasPerformanceLevel](../../../../samples/core/SqlServer/AzureDatabase/AzureSqlContext.cs?name=HasPerformanceLevel)]

<span data-ttu-id="fe649-110">Используйте [хасперформанцелевелскл](/dotnet/api/Microsoft.EntityFrameworkCore.SqlServerModelBuilderExtensions.HasPerformanceLevelSql) для настройки эластичного пула, так как значение не является строковым литералом:</span><span class="sxs-lookup"><span data-stu-id="fe649-110">Use [HasPerformanceLevelSql](/dotnet/api/Microsoft.EntityFrameworkCore.SqlServerModelBuilderExtensions.HasPerformanceLevelSql) to configure the elastic pool, since the value is not a string literal:</span></span>

[!code-csharp[HasPerformanceLevel](../../../../samples/core/SqlServer/AzureDatabase/AzureSqlContext.cs?name=HasPerformanceLevelSql)]

>[!TIP]
> <span data-ttu-id="fe649-111">Все поддерживаемые значения можно найти в [документации по ALTER DATABASE](/sql/t-sql/statements/alter-database-transact-sql?view=azuresqldb-current&preserve-view=true).</span><span class="sxs-lookup"><span data-stu-id="fe649-111">You can find all the supported values in the [ALTER DATABASE documentation](/sql/t-sql/statements/alter-database-transact-sql?view=azuresqldb-current&preserve-view=true).</span></span>
