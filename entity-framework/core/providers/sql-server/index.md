---
title: "Поставщик базы данных Microsoft SQL Server — EF Core"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 2e007c82-c6e4-45bb-8129-851b79ec1a0a
ms.technology: entity-framework-core
uid: core/providers/sql-server/index
ms.openlocfilehash: b2faf932e0484da4df0c1774afa7ba7ae2d077a5
ms.sourcegitcommit: 5e2d97e731f975cf3405ff3deab2a3c75ad1b969
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/15/2017
---
# <a name="microsoft-sql-server-ef-core-database-provider"></a><span data-ttu-id="4ce3f-102">Поставщик базы данных Microsoft SQL Server EF Core</span><span class="sxs-lookup"><span data-stu-id="4ce3f-102">Microsoft SQL Server EF Core Database Provider</span></span>

<span data-ttu-id="4ce3f-103">Этот поставщик базы данных позволяет использовать Entity Framework Core с Microsoft SQL Server (включая SQL Azure).</span><span class="sxs-lookup"><span data-stu-id="4ce3f-103">This database provider allows Entity Framework Core to be used with Microsoft SQL Server (including SQL Azure).</span></span> <span data-ttu-id="4ce3f-104">Его разработка ведется в рамках [проекта GitHub EntityFramework](https://github.com/aspnet/EntityFramework).</span><span class="sxs-lookup"><span data-stu-id="4ce3f-104">The provider is maintained as part of the [EntityFramework GitHub project](https://github.com/aspnet/EntityFramework).</span></span>

## <a name="install"></a><span data-ttu-id="4ce3f-105">Установка</span><span class="sxs-lookup"><span data-stu-id="4ce3f-105">Install</span></span>

<span data-ttu-id="4ce3f-106">Установите [пакет NuGet Microsoft.EntityFrameworkCore.SqlServer](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer/).</span><span class="sxs-lookup"><span data-stu-id="4ce3f-106">Install the [Microsoft.EntityFrameworkCore.SqlServer NuGet package](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer/).</span></span>

``` powershell
Install-Package Microsoft.EntityFrameworkCore.SqlServer
```

## <a name="get-started"></a><span data-ttu-id="4ce3f-107">Приступая к работе</span><span class="sxs-lookup"><span data-stu-id="4ce3f-107">Get Started</span></span>

<span data-ttu-id="4ce3f-108">Для начала работы с поставщиком используйте указанные ниже ресурсы.</span><span class="sxs-lookup"><span data-stu-id="4ce3f-108">The following resources will help you get started with this provider.</span></span>
* [<span data-ttu-id="4ce3f-109">Начало работы в .NET Framework (консоль, WinForms, WPF и т. д.)</span><span class="sxs-lookup"><span data-stu-id="4ce3f-109">Getting Started on .NET Framework (Console, WinForms, WPF, etc.)</span></span>](../../get-started/full-dotnet/index.md)

* [<span data-ttu-id="4ce3f-110">Начало работы в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="4ce3f-110">Getting Started on ASP.NET Core</span></span>](../../get-started/aspnetcore/index.md)

* [<span data-ttu-id="4ce3f-111">Пример приложения UnicornStore</span><span class="sxs-lookup"><span data-stu-id="4ce3f-111">UnicornStore Sample Application</span></span>](https://github.com/rowanmiller/UnicornStore/tree/master/UnicornStore)

## <a name="supported-database-engines"></a><span data-ttu-id="4ce3f-112">Поддерживаемые ядра СУБД</span><span class="sxs-lookup"><span data-stu-id="4ce3f-112">Supported Database Engines</span></span>

* <span data-ttu-id="4ce3f-113">Microsoft SQL Server (2008 и выше)</span><span class="sxs-lookup"><span data-stu-id="4ce3f-113">Microsoft SQL Server (2008 onwards)</span></span>

## <a name="supported-platforms"></a><span data-ttu-id="4ce3f-114">Поддерживаемые платформы</span><span class="sxs-lookup"><span data-stu-id="4ce3f-114">Supported Platforms</span></span>

* <span data-ttu-id="4ce3f-115">.NET Framework (4.5.1 и выше)</span><span class="sxs-lookup"><span data-stu-id="4ce3f-115">.NET Framework (4.5.1 onwards)</span></span>

* <span data-ttu-id="4ce3f-116">.NET Core</span><span class="sxs-lookup"><span data-stu-id="4ce3f-116">.NET Core</span></span>

* <span data-ttu-id="4ce3f-117">Mono (4.2.0 и выше)</span><span class="sxs-lookup"><span data-stu-id="4ce3f-117">Mono (4.2.0 onwards)</span></span>

      Caution: Using this provider on Mono will make use of the Mono SQL Client implementation, which has a number of known issues. For example, it does not support secure connections (SSL).
