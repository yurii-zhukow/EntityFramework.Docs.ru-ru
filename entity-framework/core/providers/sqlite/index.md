---
title: Поставщик базы данных SQLite — EF Core
description: Сведения о поставщике базы данных SQLite Entity Framework Core
author: rowanmiller
ms.date: 10/27/2016
uid: core/providers/sqlite/index
ms.openlocfilehash: ba537acdf537fa475378c08b8c6290930b29239d
ms.sourcegitcommit: abda0872f86eefeca191a9a11bfca976bc14468b
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/14/2020
ms.locfileid: "90071242"
---
# <a name="sqlite-ef-core-database-provider"></a><span data-ttu-id="eb35b-103">Поставщик базы данных SQLite EF Core</span><span class="sxs-lookup"><span data-stu-id="eb35b-103">SQLite EF Core Database Provider</span></span>

<span data-ttu-id="eb35b-104">Этот поставщик базы данных позволяет использовать Entity Framework Core с SQLite.</span><span class="sxs-lookup"><span data-stu-id="eb35b-104">This database provider allows Entity Framework Core to be used with SQLite.</span></span> <span data-ttu-id="eb35b-105">Работы над этим поставщиком ведутся в рамках [проекта Entity Framework Core](https://github.com/aspnet/EntityFrameworkCore).</span><span class="sxs-lookup"><span data-stu-id="eb35b-105">The provider is maintained as part of the [Entity Framework Core project](https://github.com/aspnet/EntityFrameworkCore).</span></span>

## <a name="install"></a><span data-ttu-id="eb35b-106">Установка</span><span class="sxs-lookup"><span data-stu-id="eb35b-106">Install</span></span>

<span data-ttu-id="eb35b-107">Установите [пакет NuGet Microsoft.EntityFrameworkCore.Sqlite](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Sqlite/).</span><span class="sxs-lookup"><span data-stu-id="eb35b-107">Install the [Microsoft.EntityFrameworkCore.Sqlite NuGet package](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Sqlite/).</span></span>

### <a name="net-core-cli"></a>[<span data-ttu-id="eb35b-108">Интерфейс командной строки .NET Core</span><span class="sxs-lookup"><span data-stu-id="eb35b-108">.NET Core CLI</span></span>](#tab/dotnet-core-cli)

```dotnetcli
dotnet add package Microsoft.EntityFrameworkCore.Sqlite
```

### <a name="visual-studio"></a>[<span data-ttu-id="eb35b-109">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="eb35b-109">Visual Studio</span></span>](#tab/vs)

``` powershell
Install-Package Microsoft.EntityFrameworkCore.Sqlite
```

***

## <a name="supported-database-engines"></a><span data-ttu-id="eb35b-110">Поддерживаемые ядра СУБД</span><span class="sxs-lookup"><span data-stu-id="eb35b-110">Supported Database Engines</span></span>

* <span data-ttu-id="eb35b-111">SQLite (3.7 и выше)</span><span class="sxs-lookup"><span data-stu-id="eb35b-111">SQLite (3.7 onwards)</span></span>

## <a name="limitations"></a><span data-ttu-id="eb35b-112">Ограничения</span><span class="sxs-lookup"><span data-stu-id="eb35b-112">Limitations</span></span>

<span data-ttu-id="eb35b-113">Сведения о некоторых важных ограничениях поставщика SQLite см. в статье [Ограничения SQLite](xref:core/providers/sqlite/limitations).</span><span class="sxs-lookup"><span data-stu-id="eb35b-113">See [SQLite Limitations](xref:core/providers/sqlite/limitations) for some important limitations of the SQLite provider.</span></span>
