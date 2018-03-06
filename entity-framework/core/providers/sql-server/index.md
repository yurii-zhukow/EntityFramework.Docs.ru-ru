---
title: "Поставщик базы данных Microsoft SQL Server — EF Core"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 2e007c82-c6e4-45bb-8129-851b79ec1a0a
ms.technology: entity-framework-core
uid: core/providers/sql-server/index
ms.openlocfilehash: 2ed7c0dd127db03d5e7340fde1ef83cf01b30135
ms.sourcegitcommit: b2d94cebdc32edad4fecb07e53fece66437d1b04
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/28/2018
---
# <a name="microsoft-sql-server-ef-core-database-provider"></a><span data-ttu-id="17895-102">Поставщик базы данных Microsoft SQL Server EF Core</span><span class="sxs-lookup"><span data-stu-id="17895-102">Microsoft SQL Server EF Core Database Provider</span></span>

<span data-ttu-id="17895-103">Этот поставщик базы данных позволяет использовать Entity Framework Core с Microsoft SQL Server (включая SQL Azure).</span><span class="sxs-lookup"><span data-stu-id="17895-103">This database provider allows Entity Framework Core to be used with Microsoft SQL Server (including SQL Azure).</span></span> <span data-ttu-id="17895-104">Работы над этим поставщиком ведутся в рамках [проекта Entity Framework Core](https://github.com/aspnet/EntityFrameworkCore).</span><span class="sxs-lookup"><span data-stu-id="17895-104">The provider is maintained as part of the [Entity Framework Core Project](https://github.com/aspnet/EntityFrameworkCore).</span></span>

## <a name="install"></a><span data-ttu-id="17895-105">Установка</span><span class="sxs-lookup"><span data-stu-id="17895-105">Install</span></span>

<span data-ttu-id="17895-106">Установите [пакет NuGet Microsoft.EntityFrameworkCore.SqlServer](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer/).</span><span class="sxs-lookup"><span data-stu-id="17895-106">Install the [Microsoft.EntityFrameworkCore.SqlServer NuGet package](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer/).</span></span>

``` powershell
Install-Package Microsoft.EntityFrameworkCore.SqlServer
```

## <a name="get-started"></a><span data-ttu-id="17895-107">Приступая к работе</span><span class="sxs-lookup"><span data-stu-id="17895-107">Get Started</span></span>

<span data-ttu-id="17895-108">Для начала работы с поставщиком используйте указанные ниже ресурсы.</span><span class="sxs-lookup"><span data-stu-id="17895-108">The following resources will help you get started with this provider.</span></span>
* [<span data-ttu-id="17895-109">Начало работы в .NET Framework (консоль, WinForms, WPF и т. д.)</span><span class="sxs-lookup"><span data-stu-id="17895-109">Getting Started on .NET Framework (Console, WinForms, WPF, etc.)</span></span>](../../get-started/full-dotnet/index.md)

* [<span data-ttu-id="17895-110">Начало работы в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="17895-110">Getting Started on ASP.NET Core</span></span>](../../get-started/aspnetcore/index.md)

* [<span data-ttu-id="17895-111">Пример приложения UnicornStore</span><span class="sxs-lookup"><span data-stu-id="17895-111">UnicornStore Sample Application</span></span>](https://github.com/rowanmiller/UnicornStore/tree/master/UnicornStore)

## <a name="supported-database-engines"></a><span data-ttu-id="17895-112">Поддерживаемые ядра СУБД</span><span class="sxs-lookup"><span data-stu-id="17895-112">Supported Database Engines</span></span>

* <span data-ttu-id="17895-113">Microsoft SQL Server (2008 и выше)</span><span class="sxs-lookup"><span data-stu-id="17895-113">Microsoft SQL Server (2008 onwards)</span></span>

## <a name="supported-platforms"></a><span data-ttu-id="17895-114">Поддерживаемые платформы</span><span class="sxs-lookup"><span data-stu-id="17895-114">Supported Platforms</span></span>

* <span data-ttu-id="17895-115">.NET Framework (4.5.1 и выше)</span><span class="sxs-lookup"><span data-stu-id="17895-115">.NET Framework (4.5.1 onwards)</span></span>

* <span data-ttu-id="17895-116">.NET Core</span><span class="sxs-lookup"><span data-stu-id="17895-116">.NET Core</span></span>

* <span data-ttu-id="17895-117">Mono (4.2.0 и выше)</span><span class="sxs-lookup"><span data-stu-id="17895-117">Mono (4.2.0 onwards)</span></span>

      Caution: Using this provider on Mono will make use of the Mono SQL Client implementation, which has a number of known issues. For example, it does not support secure connections (SSL).
