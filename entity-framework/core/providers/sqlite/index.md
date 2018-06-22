---
title: Поставщик базы данных SQLite — EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 3e2f7698-fec2-4cec-9e2d-2e3e0074120c
ms.technology: entity-framework-core
uid: core/providers/sqlite/index
ms.openlocfilehash: 2e392f382f0e6f4d092a362c44f2149eb336db17
ms.sourcegitcommit: b2d94cebdc32edad4fecb07e53fece66437d1b04
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/28/2018
ms.locfileid: "29678752"
---
# <a name="sqlite-ef-core-database-provider"></a><span data-ttu-id="41bb0-102">Поставщик базы данных SQLite EF Core</span><span class="sxs-lookup"><span data-stu-id="41bb0-102">SQLite EF Core Database Provider</span></span>

<span data-ttu-id="41bb0-103">Этот поставщик базы данных позволяет использовать Entity Framework Core с SQLite.</span><span class="sxs-lookup"><span data-stu-id="41bb0-103">This database provider allows Entity Framework Core to be used with SQLite.</span></span> <span data-ttu-id="41bb0-104">Работы над этим поставщиком ведутся в рамках [проекта Entity Framework Core](https://github.com/aspnet/EntityFrameworkCore).</span><span class="sxs-lookup"><span data-stu-id="41bb0-104">The provider is maintained as part of the [Entity Framework Core project](https://github.com/aspnet/EntityFrameworkCore).</span></span>

## <a name="install"></a><span data-ttu-id="41bb0-105">Установка</span><span class="sxs-lookup"><span data-stu-id="41bb0-105">Install</span></span>

<span data-ttu-id="41bb0-106">Установите [пакет NuGet Microsoft.EntityFrameworkCore.Sqlite](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Sqlite/).</span><span class="sxs-lookup"><span data-stu-id="41bb0-106">Install the [Microsoft.EntityFrameworkCore.Sqlite NuGet package](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Sqlite/).</span></span>

``` powershell
Install-Package Microsoft.EntityFrameworkCore.Sqlite
```

## <a name="get-started"></a><span data-ttu-id="41bb0-107">Приступая к работе</span><span class="sxs-lookup"><span data-stu-id="41bb0-107">Get Started</span></span>

<span data-ttu-id="41bb0-108">Для начала работы с поставщиком используйте указанные ниже ресурсы.</span><span class="sxs-lookup"><span data-stu-id="41bb0-108">The following resources will help you get started with this provider.</span></span>
* [<span data-ttu-id="41bb0-109">Локальный SQLite на UWP</span><span class="sxs-lookup"><span data-stu-id="41bb0-109">Local SQLite on UWP</span></span>](../../get-started/uwp/getting-started.md)

* [<span data-ttu-id="41bb0-110">Приложение .NET Core для новой базы данных SQLite</span><span class="sxs-lookup"><span data-stu-id="41bb0-110">.NET Core Application to New SQLite Database</span></span>](../../get-started/netcore/new-db-sqlite.md)

* [<span data-ttu-id="41bb0-111">Пример приложения Unicorn Clicker</span><span class="sxs-lookup"><span data-stu-id="41bb0-111">Unicorn Clicker Sample Application</span></span>](https://github.com/rowanmiller/UnicornStore/tree/master/UnicornClicker/UWP)

* [<span data-ttu-id="41bb0-112">Пример приложения Unicorn Packer</span><span class="sxs-lookup"><span data-stu-id="41bb0-112">Unicorn Packer Sample Application</span></span>](https://github.com/rowanmiller/UnicornStore/tree/master/UnicornPacker)

## <a name="supported-database-engines"></a><span data-ttu-id="41bb0-113">Поддерживаемые ядра СУБД</span><span class="sxs-lookup"><span data-stu-id="41bb0-113">Supported Database Engines</span></span>

* <span data-ttu-id="41bb0-114">SQLite (3.7 и выше)</span><span class="sxs-lookup"><span data-stu-id="41bb0-114">SQLite (3.7 onwards)</span></span>

## <a name="supported-platforms"></a><span data-ttu-id="41bb0-115">Поддерживаемые платформы</span><span class="sxs-lookup"><span data-stu-id="41bb0-115">Supported Platforms</span></span>

* <span data-ttu-id="41bb0-116">.NET Framework (4.5.1 и выше)</span><span class="sxs-lookup"><span data-stu-id="41bb0-116">.NET Framework (4.5.1 onwards)</span></span>

* <span data-ttu-id="41bb0-117">.NET Core</span><span class="sxs-lookup"><span data-stu-id="41bb0-117">.NET Core</span></span>

* <span data-ttu-id="41bb0-118">Mono (4.2.0 и выше)</span><span class="sxs-lookup"><span data-stu-id="41bb0-118">Mono (4.2.0 onwards)</span></span>

* <span data-ttu-id="41bb0-119">Универсальная платформа Windows </span><span class="sxs-lookup"><span data-stu-id="41bb0-119">Universal Windows Platform</span></span>

## <a name="limitations"></a><span data-ttu-id="41bb0-120">Ограничения</span><span class="sxs-lookup"><span data-stu-id="41bb0-120">Limitations</span></span>

<span data-ttu-id="41bb0-121">Сведения о некоторых важных ограничениях поставщика SQLite см. в статье [Ограничения SQLite](limitations.md).</span><span class="sxs-lookup"><span data-stu-id="41bb0-121">See [SQLite Limitations](limitations.md) for some important limitations of the SQLite provider.</span></span>
