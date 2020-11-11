---
title: Поставщик базы данных SQLite — EF Core
description: Сведения о поставщике базы данных SQLite Entity Framework Core
author: bricelam
ms.date: 10/27/2016
uid: core/providers/sqlite/index
ms.openlocfilehash: 6dac555456ed347b3eb07bbd0f2a89ab9179d20e
ms.sourcegitcommit: f3512e3a98e685a3ba409c1d0157ce85cc390cf4
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2020
ms.locfileid: "94430304"
---
# <a name="sqlite-ef-core-database-provider"></a><span data-ttu-id="33dd3-103">Поставщик базы данных SQLite EF Core</span><span class="sxs-lookup"><span data-stu-id="33dd3-103">SQLite EF Core Database Provider</span></span>

<span data-ttu-id="33dd3-104">Этот поставщик базы данных позволяет использовать Entity Framework Core с SQLite.</span><span class="sxs-lookup"><span data-stu-id="33dd3-104">This database provider allows Entity Framework Core to be used with SQLite.</span></span> <span data-ttu-id="33dd3-105">Работы над этим поставщиком ведутся в рамках [проекта Entity Framework Core](https://github.com/dotnet/efcore).</span><span class="sxs-lookup"><span data-stu-id="33dd3-105">The provider is maintained as part of the [Entity Framework Core project](https://github.com/dotnet/efcore).</span></span>

## <a name="install"></a><span data-ttu-id="33dd3-106">Установка</span><span class="sxs-lookup"><span data-stu-id="33dd3-106">Install</span></span>

<span data-ttu-id="33dd3-107">Установите [пакет NuGet Microsoft.EntityFrameworkCore.Sqlite](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Sqlite/).</span><span class="sxs-lookup"><span data-stu-id="33dd3-107">Install the [Microsoft.EntityFrameworkCore.Sqlite NuGet package](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Sqlite/).</span></span>

### <a name="net-core-cli"></a>[<span data-ttu-id="33dd3-108">Интерфейс командной строки .NET Core</span><span class="sxs-lookup"><span data-stu-id="33dd3-108">.NET Core CLI</span></span>](#tab/dotnet-core-cli)

```dotnetcli
dotnet add package Microsoft.EntityFrameworkCore.Sqlite
```

### <a name="visual-studio"></a>[<span data-ttu-id="33dd3-109">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="33dd3-109">Visual Studio</span></span>](#tab/vs)

```powershell
Install-Package Microsoft.EntityFrameworkCore.Sqlite
```

***

## <a name="supported-database-engines"></a><span data-ttu-id="33dd3-110">Поддерживаемые ядра СУБД</span><span class="sxs-lookup"><span data-stu-id="33dd3-110">Supported Database Engines</span></span>

* <span data-ttu-id="33dd3-111">SQLite (3.7 и выше)</span><span class="sxs-lookup"><span data-stu-id="33dd3-111">SQLite (3.7 onwards)</span></span>

## <a name="limitations"></a><span data-ttu-id="33dd3-112">Ограничения</span><span class="sxs-lookup"><span data-stu-id="33dd3-112">Limitations</span></span>

<span data-ttu-id="33dd3-113">Сведения о некоторых важных ограничениях поставщика SQLite см. в статье [Ограничения SQLite](xref:core/providers/sqlite/limitations).</span><span class="sxs-lookup"><span data-stu-id="33dd3-113">See [SQLite Limitations](xref:core/providers/sqlite/limitations) for some important limitations of the SQLite provider.</span></span>
