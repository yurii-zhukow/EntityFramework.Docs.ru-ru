---
title: Поставщик базы данных Microsoft SQL Server — EF Core
description: Документация для поставщика базы данных, которая позволяет использовать Entity Framework Core с Microsoft SQL Server
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 11/05/2019
uid: core/providers/sql-server/index
ms.openlocfilehash: baae668a7ec255e35ab0e23e5c5ddfa47bda917e
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/07/2020
ms.locfileid: "78413149"
---
# <a name="microsoft-sql-server-ef-core-database-provider"></a><span data-ttu-id="5fb6e-103">Поставщик базы данных Microsoft SQL Server EF Core</span><span class="sxs-lookup"><span data-stu-id="5fb6e-103">Microsoft SQL Server EF Core Database Provider</span></span>

<span data-ttu-id="5fb6e-104">Этот поставщик базы данных позволяет использовать Entity Framework Core с Microsoft SQL Server (включая базу данных SQL Azure).</span><span class="sxs-lookup"><span data-stu-id="5fb6e-104">This database provider allows Entity Framework Core to be used with Microsoft SQL Server (including Azure SQL Database).</span></span> <span data-ttu-id="5fb6e-105">Работы над этим поставщиком ведутся в рамках [проекта Entity Framework Core](https://github.com/aspnet/EntityFrameworkCore).</span><span class="sxs-lookup"><span data-stu-id="5fb6e-105">The provider is maintained as part of the [Entity Framework Core Project](https://github.com/aspnet/EntityFrameworkCore).</span></span>

## <a name="install"></a><span data-ttu-id="5fb6e-106">Установка</span><span class="sxs-lookup"><span data-stu-id="5fb6e-106">Install</span></span>

<span data-ttu-id="5fb6e-107">Установите [пакет NuGet Microsoft.EntityFrameworkCore.SqlServer](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer/).</span><span class="sxs-lookup"><span data-stu-id="5fb6e-107">Install the [Microsoft.EntityFrameworkCore.SqlServer NuGet package](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer/).</span></span>

### <a name="net-core-cli"></a>[<span data-ttu-id="5fb6e-108">Интерфейс командной строки .NET Core</span><span class="sxs-lookup"><span data-stu-id="5fb6e-108">.NET Core CLI</span></span>](#tab/dotnet-core-cli)

```dotnetcli
dotnet add package Microsoft.EntityFrameworkCore.SqlServer
```

### <a name="visual-studio"></a>[<span data-ttu-id="5fb6e-109">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="5fb6e-109">Visual Studio</span></span>](#tab/vs)

``` powershell
Install-Package Microsoft.EntityFrameworkCore.SqlServer
```

***

> [!NOTE]
> <span data-ttu-id="5fb6e-110">Начиная с версии 3.0.0,поставщик ссылается на Microsoft.Data.SqlClient (предыдущие версии зависят от System.Data.SqlClient).</span><span class="sxs-lookup"><span data-stu-id="5fb6e-110">Since version 3.0.0, the provider references Microsoft.Data.SqlClient (previous versions depended on System.Data.SqlClient).</span></span> <span data-ttu-id="5fb6e-111">Если проект принимает прямую зависимость от SqlClient, убедитесь, что добавлена ссылка на пакет Microsoft.Data.SqlClient.</span><span class="sxs-lookup"><span data-stu-id="5fb6e-111">If your project takes a direct dependency on SqlClient, make sure it references the Microsoft.Data.SqlClient package.</span></span>

## <a name="supported-database-engines"></a><span data-ttu-id="5fb6e-112">Поддерживаемые ядра СУБД</span><span class="sxs-lookup"><span data-stu-id="5fb6e-112">Supported Database Engines</span></span>

* <span data-ttu-id="5fb6e-113">Microsoft SQL Server (2012 и последующие версии)</span><span class="sxs-lookup"><span data-stu-id="5fb6e-113">Microsoft SQL Server (2012 onwards)</span></span>
