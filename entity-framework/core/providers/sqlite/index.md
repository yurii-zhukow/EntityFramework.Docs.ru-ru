---
title: Поставщик базы данных SQLite — EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 3e2f7698-fec2-4cec-9e2d-2e3e0074120c
uid: core/providers/sqlite/index
ms.openlocfilehash: e4cbdba46f901831892192a343db2920a5760042
ms.sourcegitcommit: cbaa6cc89bd71d5e0bcc891e55743f0e8ea3393b
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/20/2019
ms.locfileid: "71149268"
---
# <a name="sqlite-ef-core-database-provider"></a><span data-ttu-id="2765e-102">Поставщик базы данных SQLite EF Core</span><span class="sxs-lookup"><span data-stu-id="2765e-102">SQLite EF Core Database Provider</span></span>

<span data-ttu-id="2765e-103">Этот поставщик базы данных позволяет использовать Entity Framework Core с SQLite.</span><span class="sxs-lookup"><span data-stu-id="2765e-103">This database provider allows Entity Framework Core to be used with SQLite.</span></span> <span data-ttu-id="2765e-104">Работы над этим поставщиком ведутся в рамках [проекта Entity Framework Core](https://github.com/aspnet/EntityFrameworkCore).</span><span class="sxs-lookup"><span data-stu-id="2765e-104">The provider is maintained as part of the [Entity Framework Core project](https://github.com/aspnet/EntityFrameworkCore).</span></span>

## <a name="install"></a><span data-ttu-id="2765e-105">Установка</span><span class="sxs-lookup"><span data-stu-id="2765e-105">Install</span></span>

<span data-ttu-id="2765e-106">Установите [пакет NuGet Microsoft.EntityFrameworkCore.Sqlite](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Sqlite/).</span><span class="sxs-lookup"><span data-stu-id="2765e-106">Install the [Microsoft.EntityFrameworkCore.Sqlite NuGet package](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Sqlite/).</span></span>

``` powershell
Install-Package Microsoft.EntityFrameworkCore.Sqlite
```

## <a name="supported-database-engines"></a><span data-ttu-id="2765e-107">Поддерживаемые ядра СУБД</span><span class="sxs-lookup"><span data-stu-id="2765e-107">Supported Database Engines</span></span>

* <span data-ttu-id="2765e-108">SQLite (3.7 и выше)</span><span class="sxs-lookup"><span data-stu-id="2765e-108">SQLite (3.7 onwards)</span></span>

## <a name="limitations"></a><span data-ttu-id="2765e-109">Ограничения</span><span class="sxs-lookup"><span data-stu-id="2765e-109">Limitations</span></span>

<span data-ttu-id="2765e-110">Сведения о некоторых важных ограничениях поставщика SQLite см. в статье [Ограничения SQLite](limitations.md).</span><span class="sxs-lookup"><span data-stu-id="2765e-110">See [SQLite Limitations](limitations.md) for some important limitations of the SQLite provider.</span></span>
