---
title: Поставщик базы данных SQLite — EF Core
description: Сведения о поставщике базы данных SQLite Entity Framework Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 3e2f7698-fec2-4cec-9e2d-2e3e0074120c
uid: core/providers/sqlite/index
ms.openlocfilehash: 8620f0e37825368cb3d7965a05118ab1297fb9e8
ms.sourcegitcommit: 7c3939504bb9da3f46bea3443638b808c04227c2
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/09/2020
ms.locfileid: "89616559"
---
# <a name="sqlite-ef-core-database-provider"></a><span data-ttu-id="f8bde-103">Поставщик базы данных SQLite EF Core</span><span class="sxs-lookup"><span data-stu-id="f8bde-103">SQLite EF Core Database Provider</span></span>

<span data-ttu-id="f8bde-104">Этот поставщик базы данных позволяет использовать Entity Framework Core с SQLite.</span><span class="sxs-lookup"><span data-stu-id="f8bde-104">This database provider allows Entity Framework Core to be used with SQLite.</span></span> <span data-ttu-id="f8bde-105">Работы над этим поставщиком ведутся в рамках [проекта Entity Framework Core](https://github.com/aspnet/EntityFrameworkCore).</span><span class="sxs-lookup"><span data-stu-id="f8bde-105">The provider is maintained as part of the [Entity Framework Core project](https://github.com/aspnet/EntityFrameworkCore).</span></span>

## <a name="install"></a><span data-ttu-id="f8bde-106">Установка</span><span class="sxs-lookup"><span data-stu-id="f8bde-106">Install</span></span>

<span data-ttu-id="f8bde-107">Установите [пакет NuGet Microsoft.EntityFrameworkCore.Sqlite](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Sqlite/).</span><span class="sxs-lookup"><span data-stu-id="f8bde-107">Install the [Microsoft.EntityFrameworkCore.Sqlite NuGet package](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Sqlite/).</span></span>

### <a name="net-core-cli"></a>[<span data-ttu-id="f8bde-108">Интерфейс командной строки .NET Core</span><span class="sxs-lookup"><span data-stu-id="f8bde-108">.NET Core CLI</span></span>](#tab/dotnet-core-cli)

```dotnetcli
dotnet add package Microsoft.EntityFrameworkCore.Sqlite
```

### <a name="visual-studio"></a>[<span data-ttu-id="f8bde-109">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f8bde-109">Visual Studio</span></span>](#tab/vs)

``` powershell
Install-Package Microsoft.EntityFrameworkCore.Sqlite
```

***

## <a name="supported-database-engines"></a><span data-ttu-id="f8bde-110">Поддерживаемые ядра СУБД</span><span class="sxs-lookup"><span data-stu-id="f8bde-110">Supported Database Engines</span></span>

* <span data-ttu-id="f8bde-111">SQLite (3.7 и выше)</span><span class="sxs-lookup"><span data-stu-id="f8bde-111">SQLite (3.7 onwards)</span></span>

## <a name="limitations"></a><span data-ttu-id="f8bde-112">Ограничения</span><span class="sxs-lookup"><span data-stu-id="f8bde-112">Limitations</span></span>

<span data-ttu-id="f8bde-113">Сведения о некоторых важных ограничениях поставщика SQLite см. в статье [Ограничения SQLite](xref:core/providers/sqlite/limitations).</span><span class="sxs-lookup"><span data-stu-id="f8bde-113">See [SQLite Limitations](xref:core/providers/sqlite/limitations) for some important limitations of the SQLite provider.</span></span>
