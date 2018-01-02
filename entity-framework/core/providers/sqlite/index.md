---
title: "Поставщик базы данных SQLite — EF Core"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 3e2f7698-fec2-4cec-9e2d-2e3e0074120c
ms.technology: entity-framework-core
uid: core/providers/sqlite/index
ms.openlocfilehash: 0f3905a491fb5f7a657ce9037d166771e1f326d8
ms.sourcegitcommit: 5e2d97e731f975cf3405ff3deab2a3c75ad1b969
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/15/2017
---
# <a name="sqlite-ef-core-database-provider"></a><span data-ttu-id="0c084-102">Поставщик базы данных SQLite EF Core</span><span class="sxs-lookup"><span data-stu-id="0c084-102">SQLite EF Core Database Provider</span></span>

<span data-ttu-id="0c084-103">Этот поставщик базы данных позволяет использовать Entity Framework Core с SQLite.</span><span class="sxs-lookup"><span data-stu-id="0c084-103">This database provider allows Entity Framework Core to be used with SQLite.</span></span> <span data-ttu-id="0c084-104">Его разработка ведется в рамках [проекта GitHub EntityFramework](https://github.com/aspnet/EntityFramework).</span><span class="sxs-lookup"><span data-stu-id="0c084-104">The provider is maintained as part of the [EntityFramework GitHub project](https://github.com/aspnet/EntityFramework).</span></span>

## <a name="install"></a><span data-ttu-id="0c084-105">Установка</span><span class="sxs-lookup"><span data-stu-id="0c084-105">Install</span></span>

<span data-ttu-id="0c084-106">Установите [пакет NuGet Microsoft.EntityFrameworkCore.Sqlite](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Sqlite/).</span><span class="sxs-lookup"><span data-stu-id="0c084-106">Install the [Microsoft.EntityFrameworkCore.Sqlite NuGet package](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Sqlite/).</span></span>

``` powershell
Install-Package Microsoft.EntityFrameworkCore.Sqlite
```

## <a name="get-started"></a><span data-ttu-id="0c084-107">Приступая к работе</span><span class="sxs-lookup"><span data-stu-id="0c084-107">Get Started</span></span>

<span data-ttu-id="0c084-108">Для начала работы с поставщиком используйте указанные ниже ресурсы.</span><span class="sxs-lookup"><span data-stu-id="0c084-108">The following resources will help you get started with this provider.</span></span>
* [<span data-ttu-id="0c084-109">Локальный SQLite на UWP</span><span class="sxs-lookup"><span data-stu-id="0c084-109">Local SQLite on UWP</span></span>](../../get-started/uwp/getting-started.md)

* [<span data-ttu-id="0c084-110">Приложение .NET Core для новой базы данных SQLite</span><span class="sxs-lookup"><span data-stu-id="0c084-110">.NET Core Application to New SQLite Database</span></span>](../../get-started/netcore/new-db-sqlite.md)

* [<span data-ttu-id="0c084-111">Пример приложения Unicorn Clicker</span><span class="sxs-lookup"><span data-stu-id="0c084-111">Unicorn Clicker Sample Application</span></span>](https://github.com/rowanmiller/UnicornStore/tree/master/UnicornClicker/UWP)

* [<span data-ttu-id="0c084-112">Пример приложения Unicorn Packer</span><span class="sxs-lookup"><span data-stu-id="0c084-112">Unicorn Packer Sample Application</span></span>](https://github.com/rowanmiller/UnicornStore/tree/master/UnicornPacker)

## <a name="supported-database-engines"></a><span data-ttu-id="0c084-113">Поддерживаемые ядра СУБД</span><span class="sxs-lookup"><span data-stu-id="0c084-113">Supported Database Engines</span></span>

* <span data-ttu-id="0c084-114">SQLite (3.7 и выше)</span><span class="sxs-lookup"><span data-stu-id="0c084-114">SQLite (3.7 onwards)</span></span>

## <a name="supported-platforms"></a><span data-ttu-id="0c084-115">Поддерживаемые платформы</span><span class="sxs-lookup"><span data-stu-id="0c084-115">Supported Platforms</span></span>

* <span data-ttu-id="0c084-116">.NET Framework (4.5.1 и выше)</span><span class="sxs-lookup"><span data-stu-id="0c084-116">.NET Framework (4.5.1 onwards)</span></span>

* <span data-ttu-id="0c084-117">.NET Core</span><span class="sxs-lookup"><span data-stu-id="0c084-117">.NET Core</span></span>

* <span data-ttu-id="0c084-118">Mono (4.2.0 и выше)</span><span class="sxs-lookup"><span data-stu-id="0c084-118">Mono (4.2.0 onwards)</span></span>

* <span data-ttu-id="0c084-119">Универсальная платформа Windows </span><span class="sxs-lookup"><span data-stu-id="0c084-119">Universal Windows Platform</span></span>

## <a name="limitations"></a><span data-ttu-id="0c084-120">Ограничения</span><span class="sxs-lookup"><span data-stu-id="0c084-120">Limitations</span></span>

<span data-ttu-id="0c084-121">Сведения о некоторых важных ограничениях поставщика SQLite см. в статье [Ограничения SQLite](limitations.md).</span><span class="sxs-lookup"><span data-stu-id="0c084-121">See [SQLite Limitations](limitations.md) for some important limitations of the SQLite provider.</span></span>
