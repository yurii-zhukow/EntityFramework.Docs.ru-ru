---
title: Поставщик базы данных Microsoft SQL Server — EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 2e007c82-c6e4-45bb-8129-851b79ec1a0a
uid: core/providers/sql-server/index
ms.openlocfilehash: 70e7f3d4bc1f57f1b23d9b3e0bd6264236ddbd27
ms.sourcegitcommit: cbaa6cc89bd71d5e0bcc891e55743f0e8ea3393b
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/20/2019
ms.locfileid: "71149199"
---
# <a name="microsoft-sql-server-ef-core-database-provider"></a><span data-ttu-id="463c0-102">Поставщик базы данных Microsoft SQL Server EF Core</span><span class="sxs-lookup"><span data-stu-id="463c0-102">Microsoft SQL Server EF Core Database Provider</span></span>

<span data-ttu-id="463c0-103">Этот поставщик базы данных позволяет использовать Entity Framework Core с Microsoft SQL Server (включая SQL Azure).</span><span class="sxs-lookup"><span data-stu-id="463c0-103">This database provider allows Entity Framework Core to be used with Microsoft SQL Server (including SQL Azure).</span></span> <span data-ttu-id="463c0-104">Работы над этим поставщиком ведутся в рамках [проекта Entity Framework Core](https://github.com/aspnet/EntityFrameworkCore).</span><span class="sxs-lookup"><span data-stu-id="463c0-104">The provider is maintained as part of the [Entity Framework Core Project](https://github.com/aspnet/EntityFrameworkCore).</span></span>

## <a name="install"></a><span data-ttu-id="463c0-105">Установка</span><span class="sxs-lookup"><span data-stu-id="463c0-105">Install</span></span>

<span data-ttu-id="463c0-106">Установите [пакет NuGet Microsoft.EntityFrameworkCore.SqlServer](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer/).</span><span class="sxs-lookup"><span data-stu-id="463c0-106">Install the [Microsoft.EntityFrameworkCore.SqlServer NuGet package](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer/).</span></span>

``` powershell
Install-Package Microsoft.EntityFrameworkCore.SqlServer
```

## <a name="supported-database-engines"></a><span data-ttu-id="463c0-107">Поддерживаемые ядра СУБД</span><span class="sxs-lookup"><span data-stu-id="463c0-107">Supported Database Engines</span></span>

* <span data-ttu-id="463c0-108">Microsoft SQL Server (2012 и последующие версии)</span><span class="sxs-lookup"><span data-stu-id="463c0-108">Microsoft SQL Server (2012 onwards)</span></span>
