---
title: "Поставщик базы данных IBM Data Server (DB2) — EF Core"
author: rowanmiller
ms.author: divega
ms.date: 02/15/2017
ms.assetid: 825e5332-5aa3-4600-9efb-ab71aaff59ec
ms.technology: entity-framework-core
uid: core/providers/ibm/index
ms.openlocfilehash: a9caa8df63850d4f6b5f2164dad7ac5af7504076
ms.sourcegitcommit: 5e2d97e731f975cf3405ff3deab2a3c75ad1b969
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/15/2017
---
# <a name="ibm-data-server-db2-ef-core-database-providers"></a><span data-ttu-id="b5abc-102">Поставщик базы данных IBM Data Server (DB2) EF Core</span><span class="sxs-lookup"><span data-stu-id="b5abc-102">IBM Data Server (DB2) EF Core Database Providers</span></span>

<span data-ttu-id="b5abc-103">Этот поставщик базы данных позволяет использовать Entity Framework Core с IBM Data Server.</span><span class="sxs-lookup"><span data-stu-id="b5abc-103">This database provider allows Entity Framework Core to be used with IBM Data Server.</span></span> <span data-ttu-id="b5abc-104">Поставщик разрабатывается компанией IBM.</span><span class="sxs-lookup"><span data-stu-id="b5abc-104">The provider is maintained by IBM.</span></span>

> [!NOTE]  
> <span data-ttu-id="b5abc-105">В рамках проекта Entity Framework Core работы над ним не ведутся.</span><span class="sxs-lookup"><span data-stu-id="b5abc-105">This provider is not maintained as part of the Entity Framework Core project.</span></span> <span data-ttu-id="b5abc-106">Выбирая сторонний поставщик, обязательно оцените качество, лицензирование, поддержку и другие показатели на соответствие вашим требованиям.</span><span class="sxs-lookup"><span data-stu-id="b5abc-106">When considering a third party provider, be sure to evaluate quality, licensing, support, etc. to ensure they meet your requirements.</span></span>

## <a name="install"></a><span data-ttu-id="b5abc-107">Установка</span><span class="sxs-lookup"><span data-stu-id="b5abc-107">Install</span></span>

<span data-ttu-id="b5abc-108">Для работы с IBM Data Server в Windows установите [пакет NuGet IBM.EntityFrameworkCore](https://www.nuget.org/packages/IBM.EntityFrameworkCore).</span><span class="sxs-lookup"><span data-stu-id="b5abc-108">To work with IBM Data Server on Windows, install the [IBM.EntityFrameworkCore NuGet package](https://www.nuget.org/packages/IBM.EntityFrameworkCore).</span></span>

``` powershell
Install-Package IBM.EntityFrameworkCore
```

<span data-ttu-id="b5abc-109">Для работы с IBM Data Server в Linux установите [пакет NuGet IBM.EntityFrameworkCore-lnx](https://www.nuget.org/packages/IBM.EntityFrameworkCore-lnx).</span><span class="sxs-lookup"><span data-stu-id="b5abc-109">To work with IBM Data Server on Linux, install the [IBM.EntityFrameworkCore-lnx NuGet package](https://www.nuget.org/packages/IBM.EntityFrameworkCore-lnx).</span></span>

``` powershell
Install-Package IBM.EntityFrameworkCore-lnx
```

## <a name="get-started"></a><span data-ttu-id="b5abc-110">Приступая к работе</span><span class="sxs-lookup"><span data-stu-id="b5abc-110">Get Started</span></span>

[<span data-ttu-id="b5abc-111">Начало работы с поставщиком IBM .NET для .NET Core</span><span class="sxs-lookup"><span data-stu-id="b5abc-111">Getting started with IBM .NET Provider for .NET Core</span></span>](https://www.ibm.com/developerworks/community/blogs/96960515-2ea1-4391-8170-b0515d08e4da/entry/DB2DotnetCore?lang=en)

## <a name="supported-database-engines"></a><span data-ttu-id="b5abc-112">Поддерживаемые ядра СУБД</span><span class="sxs-lookup"><span data-stu-id="b5abc-112">Supported Database Engines</span></span>

* <span data-ttu-id="b5abc-113">zOS</span><span class="sxs-lookup"><span data-stu-id="b5abc-113">zOS</span></span>
* <span data-ttu-id="b5abc-114">LUW, включая IBM dashDB</span><span class="sxs-lookup"><span data-stu-id="b5abc-114">LUW including IBM dashDB</span></span>
* <span data-ttu-id="b5abc-115">IBM I</span><span class="sxs-lookup"><span data-stu-id="b5abc-115">IBM I</span></span>
* <span data-ttu-id="b5abc-116">Informix</span><span class="sxs-lookup"><span data-stu-id="b5abc-116">Informix</span></span>

## <a name="supported-platforms"></a><span data-ttu-id="b5abc-117">Поддерживаемые платформы</span><span class="sxs-lookup"><span data-stu-id="b5abc-117">Supported Platforms</span></span>

* <span data-ttu-id="b5abc-118">.NET Framework (4.6 и выше)</span><span class="sxs-lookup"><span data-stu-id="b5abc-118">.NET Framework (4.6 onwards)</span></span>
* <span data-ttu-id="b5abc-119">.NET Core</span><span class="sxs-lookup"><span data-stu-id="b5abc-119">.NET Core</span></span>
