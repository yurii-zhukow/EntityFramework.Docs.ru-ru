---
title: Поставщик базы данных Microsoft SQL Server — EF Core
description: Документация для поставщика базы данных, которая позволяет использовать Entity Framework Core с Microsoft SQL Server
author: AndriySvyryd
ms.date: 11/05/2019
uid: core/providers/sql-server/index
ms.openlocfilehash: 344bf243577d39d2d5a03cf321f8d84c275d10d3
ms.sourcegitcommit: 0a25c03fa65ae6e0e0e3f66bac48d59eceb96a5a
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/14/2020
ms.locfileid: "92065333"
---
# <a name="microsoft-sql-server-ef-core-database-provider"></a><span data-ttu-id="ffe15-103">Поставщик базы данных Microsoft SQL Server EF Core</span><span class="sxs-lookup"><span data-stu-id="ffe15-103">Microsoft SQL Server EF Core Database Provider</span></span>

<span data-ttu-id="ffe15-104">Этот поставщик базы данных позволяет использовать Entity Framework Core с Microsoft SQL Server (включая базу данных SQL Azure).</span><span class="sxs-lookup"><span data-stu-id="ffe15-104">This database provider allows Entity Framework Core to be used with Microsoft SQL Server (including Azure SQL Database).</span></span> <span data-ttu-id="ffe15-105">Работы над этим поставщиком ведутся в рамках [проекта Entity Framework Core](https://github.com/aspnet/EntityFrameworkCore).</span><span class="sxs-lookup"><span data-stu-id="ffe15-105">The provider is maintained as part of the [Entity Framework Core Project](https://github.com/aspnet/EntityFrameworkCore).</span></span>

## <a name="install"></a><span data-ttu-id="ffe15-106">Установка</span><span class="sxs-lookup"><span data-stu-id="ffe15-106">Install</span></span>

<span data-ttu-id="ffe15-107">Установите [пакет NuGet Microsoft.EntityFrameworkCore.SqlServer](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer/).</span><span class="sxs-lookup"><span data-stu-id="ffe15-107">Install the [Microsoft.EntityFrameworkCore.SqlServer NuGet package](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer/).</span></span>

### <a name="net-core-cli"></a>[<span data-ttu-id="ffe15-108">Интерфейс командной строки .NET Core</span><span class="sxs-lookup"><span data-stu-id="ffe15-108">.NET Core CLI</span></span>](#tab/dotnet-core-cli)

```dotnetcli
dotnet add package Microsoft.EntityFrameworkCore.SqlServer
```

### <a name="visual-studio"></a>[<span data-ttu-id="ffe15-109">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="ffe15-109">Visual Studio</span></span>](#tab/vs)

```powershell
Install-Package Microsoft.EntityFrameworkCore.SqlServer
```

***

> [!NOTE]
> <span data-ttu-id="ffe15-110">Начиная с версии 3.0.0,поставщик ссылается на Microsoft.Data.SqlClient (предыдущие версии зависят от System.Data.SqlClient).</span><span class="sxs-lookup"><span data-stu-id="ffe15-110">Since version 3.0.0, the provider references Microsoft.Data.SqlClient (previous versions depended on System.Data.SqlClient).</span></span> <span data-ttu-id="ffe15-111">Если проект принимает прямую зависимость от SqlClient, убедитесь, что добавлена ссылка на пакет Microsoft.Data.SqlClient.</span><span class="sxs-lookup"><span data-stu-id="ffe15-111">If your project takes a direct dependency on SqlClient, make sure it references the Microsoft.Data.SqlClient package.</span></span>

## <a name="supported-database-engines"></a><span data-ttu-id="ffe15-112">Поддерживаемые ядра СУБД</span><span class="sxs-lookup"><span data-stu-id="ffe15-112">Supported Database Engines</span></span>

* <span data-ttu-id="ffe15-113">Microsoft SQL Server (2012 и последующие версии)</span><span class="sxs-lookup"><span data-stu-id="ffe15-113">Microsoft SQL Server (2012 onwards)</span></span>
