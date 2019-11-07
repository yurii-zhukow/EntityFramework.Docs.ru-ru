---
title: Поставщик базы данных SQLite — EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 3e2f7698-fec2-4cec-9e2d-2e3e0074120c
uid: core/providers/sqlite/index
ms.openlocfilehash: 4637044b1712280d3cdca6bcca1ca61564ff78ea
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/06/2019
ms.locfileid: "73656074"
---
# <a name="sqlite-ef-core-database-provider"></a><span data-ttu-id="abf7e-102">Поставщик базы данных SQLite EF Core</span><span class="sxs-lookup"><span data-stu-id="abf7e-102">SQLite EF Core Database Provider</span></span>

<span data-ttu-id="abf7e-103">Этот поставщик базы данных позволяет использовать Entity Framework Core с SQLite.</span><span class="sxs-lookup"><span data-stu-id="abf7e-103">This database provider allows Entity Framework Core to be used with SQLite.</span></span> <span data-ttu-id="abf7e-104">Работы над этим поставщиком ведутся в рамках [проекта Entity Framework Core](https://github.com/aspnet/EntityFrameworkCore).</span><span class="sxs-lookup"><span data-stu-id="abf7e-104">The provider is maintained as part of the [Entity Framework Core project](https://github.com/aspnet/EntityFrameworkCore).</span></span>

## <a name="install"></a><span data-ttu-id="abf7e-105">Установка</span><span class="sxs-lookup"><span data-stu-id="abf7e-105">Install</span></span>

<span data-ttu-id="abf7e-106">Установите [пакет NuGet Microsoft.EntityFrameworkCore.Sqlite](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Sqlite/).</span><span class="sxs-lookup"><span data-stu-id="abf7e-106">Install the [Microsoft.EntityFrameworkCore.Sqlite NuGet package](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Sqlite/).</span></span>

## <a name="net-core-clitabdotnet-core-cli"></a>[<span data-ttu-id="abf7e-107">Интерфейс командной строки .NET Core</span><span class="sxs-lookup"><span data-stu-id="abf7e-107">.NET Core CLI</span></span>](#tab/dotnet-core-cli)

``` console
dotnet add package Microsoft.EntityFrameworkCore.Sqlite
```

## <a name="visual-studiotabvs"></a>[<span data-ttu-id="abf7e-108">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="abf7e-108">Visual Studio</span></span>](#tab/vs)

``` powershell
Install-Package Microsoft.EntityFrameworkCore.Sqlite
```

***

## <a name="supported-database-engines"></a><span data-ttu-id="abf7e-109">Поддерживаемые ядра СУБД</span><span class="sxs-lookup"><span data-stu-id="abf7e-109">Supported Database Engines</span></span>

* <span data-ttu-id="abf7e-110">SQLite (3.7 и выше)</span><span class="sxs-lookup"><span data-stu-id="abf7e-110">SQLite (3.7 onwards)</span></span>

## <a name="limitations"></a><span data-ttu-id="abf7e-111">Ограничения</span><span class="sxs-lookup"><span data-stu-id="abf7e-111">Limitations</span></span>

<span data-ttu-id="abf7e-112">Сведения о некоторых важных ограничениях поставщика SQLite см. в статье [Ограничения SQLite](limitations.md).</span><span class="sxs-lookup"><span data-stu-id="abf7e-112">See [SQLite Limitations](limitations.md) for some important limitations of the SQLite provider.</span></span>
