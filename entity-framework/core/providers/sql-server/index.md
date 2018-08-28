---
title: Поставщик базы данных Microsoft SQL Server — EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 2e007c82-c6e4-45bb-8129-851b79ec1a0a
uid: core/providers/sql-server/index
ms.openlocfilehash: a524794a61a9f5078998aea04b45c31c19357f2b
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/27/2018
ms.locfileid: "42995673"
---
# <a name="microsoft-sql-server-ef-core-database-provider"></a><span data-ttu-id="31eba-102">Поставщик базы данных Microsoft SQL Server EF Core</span><span class="sxs-lookup"><span data-stu-id="31eba-102">Microsoft SQL Server EF Core Database Provider</span></span>

<span data-ttu-id="31eba-103">Этот поставщик базы данных позволяет использовать Entity Framework Core с Microsoft SQL Server (включая SQL Azure).</span><span class="sxs-lookup"><span data-stu-id="31eba-103">This database provider allows Entity Framework Core to be used with Microsoft SQL Server (including SQL Azure).</span></span> <span data-ttu-id="31eba-104">Работы над этим поставщиком ведутся в рамках [проекта Entity Framework Core](https://github.com/aspnet/EntityFrameworkCore).</span><span class="sxs-lookup"><span data-stu-id="31eba-104">The provider is maintained as part of the [Entity Framework Core Project](https://github.com/aspnet/EntityFrameworkCore).</span></span>

## <a name="install"></a><span data-ttu-id="31eba-105">Установка</span><span class="sxs-lookup"><span data-stu-id="31eba-105">Install</span></span>

<span data-ttu-id="31eba-106">Установите [пакет NuGet Microsoft.EntityFrameworkCore.SqlServer](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer/).</span><span class="sxs-lookup"><span data-stu-id="31eba-106">Install the [Microsoft.EntityFrameworkCore.SqlServer NuGet package](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer/).</span></span>

``` powershell
Install-Package Microsoft.EntityFrameworkCore.SqlServer
```

## <a name="get-started"></a><span data-ttu-id="31eba-107">Приступая к работе</span><span class="sxs-lookup"><span data-stu-id="31eba-107">Get Started</span></span>

<span data-ttu-id="31eba-108">Для начала работы с поставщиком используйте указанные ниже ресурсы.</span><span class="sxs-lookup"><span data-stu-id="31eba-108">The following resources will help you get started with this provider.</span></span>
* [<span data-ttu-id="31eba-109">Начало работы в .NET Framework (консоль, WinForms, WPF и т. д.)</span><span class="sxs-lookup"><span data-stu-id="31eba-109">Getting Started on .NET Framework (Console, WinForms, WPF, etc.)</span></span>](../../get-started/full-dotnet/index.md)

* [<span data-ttu-id="31eba-110">Начало работы в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="31eba-110">Getting Started on ASP.NET Core</span></span>](../../get-started/aspnetcore/index.md)

* [<span data-ttu-id="31eba-111">Пример приложения UnicornStore</span><span class="sxs-lookup"><span data-stu-id="31eba-111">UnicornStore Sample Application</span></span>](https://github.com/rowanmiller/UnicornStore/tree/master/UnicornStore)

## <a name="supported-database-engines"></a><span data-ttu-id="31eba-112">Поддерживаемые ядра СУБД</span><span class="sxs-lookup"><span data-stu-id="31eba-112">Supported Database Engines</span></span>

* <span data-ttu-id="31eba-113">Microsoft SQL Server (2008 и выше)</span><span class="sxs-lookup"><span data-stu-id="31eba-113">Microsoft SQL Server (2008 onwards)</span></span>

## <a name="supported-platforms"></a><span data-ttu-id="31eba-114">Поддерживаемые платформы</span><span class="sxs-lookup"><span data-stu-id="31eba-114">Supported Platforms</span></span>

* <span data-ttu-id="31eba-115">.NET Framework (4.5.1 и выше)</span><span class="sxs-lookup"><span data-stu-id="31eba-115">.NET Framework (4.5.1 onwards)</span></span>

* <span data-ttu-id="31eba-116">.NET Core</span><span class="sxs-lookup"><span data-stu-id="31eba-116">.NET Core</span></span>

* <span data-ttu-id="31eba-117">Mono (4.2.0 и выше)</span><span class="sxs-lookup"><span data-stu-id="31eba-117">Mono (4.2.0 onwards)</span></span>

      Caution: Using this provider on Mono will make use of the Mono SQL Client implementation, which has a number of known issues. For example, it does not support secure connections (SSL).
