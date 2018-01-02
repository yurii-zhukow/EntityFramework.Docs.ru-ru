---
title: "Поставщик базы данных FirebirdSQL — EF Core"
author: ralmsdeveloper
ms.author: ralmsdeveloper
ms.date: 11/22/2017
ms.assetid: d0168c04-d30d-4219-98f8-a54690cea3c6
ms.technology: entity-framework-core
uid: core/providers/firebird-community/index
ms.openlocfilehash: 682988a91ef04dbd552588a537f53124b931f17d
ms.sourcegitcommit: 1cbd3d3cd92bdaf8223b8821c58200bcfed10ede
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/23/2017
---
# <a name="firebird-ef-core-database-provider"></a><span data-ttu-id="ba627-102">Поставщик базы данных Firebird EF Core</span><span class="sxs-lookup"><span data-stu-id="ba627-102">Firebird EF Core Database Provider</span></span>

<span data-ttu-id="ba627-103">Этот поставщик базы данных позволяет использовать Entity Framework Core с FirebirdSQL.</span><span class="sxs-lookup"><span data-stu-id="ba627-103">This database provider allows Entity Framework Core to be used with FirebirdSQL.</span></span> <span data-ttu-id="ba627-104">Поставщик разрабатывается в рамках [проекта GitHub ralmsdeveloper/EntityFrameworkCore.FirebirdSQL](https://github.com/ralmsdeveloper/EntityFrameworkCore.FirebirdSQL).</span><span class="sxs-lookup"><span data-stu-id="ba627-104">The provider is maintained as part of the [ralmsdeveloper/EntityFrameworkCore.FirebirdSQL GitHub Project](https://github.com/ralmsdeveloper/EntityFrameworkCore.FirebirdSQL).</span></span>

> [!NOTE]  
>
> <span data-ttu-id="ba627-105">В рамках проекта Entity Framework Core работы над ним не ведутся.</span><span class="sxs-lookup"><span data-stu-id="ba627-105">This provider is not maintained as part of the Entity Framework Core project.</span></span> <span data-ttu-id="ba627-106">Выбирая сторонний поставщик, обязательно оцените качество, лицензирование, поддержку и другие показатели на соответствие вашим требованиям.</span><span class="sxs-lookup"><span data-stu-id="ba627-106">When considering a third party provider, be sure to evaluate quality, licensing, support, etc. to ensure they meet your requirements.</span></span>

## <a name="install"></a><span data-ttu-id="ba627-107">Установка</span><span class="sxs-lookup"><span data-stu-id="ba627-107">Install</span></span>

<span data-ttu-id="ba627-108">Установите [пакет NuGet EntityFrameworkCore.FirebirdSQL](https://www.nuget.org/packages/EntityFrameworkCore.FirebirdSQL).</span><span class="sxs-lookup"><span data-stu-id="ba627-108">Install the [EntityFrameworkCore.FirebirdSQL NuGet package](https://www.nuget.org/packages/EntityFrameworkCore.FirebirdSQL).</span></span>

``` powershell
Install-Package EntityFrameworkCore.FirebirdSQL
```

## <a name="get-started"></a><span data-ttu-id="ba627-109">Приступая к работе</span><span class="sxs-lookup"><span data-stu-id="ba627-109">Get Started</span></span>

<span data-ttu-id="ba627-110">См. [документацию по началу работы на сайте проекта](https://github.com/ralmsdeveloper/EntityFrameworkCore.FirebirdSQL/wiki).</span><span class="sxs-lookup"><span data-stu-id="ba627-110">See the [getting started documentation on the project site](https://github.com/ralmsdeveloper/EntityFrameworkCore.FirebirdSQL/wiki)</span></span>

## <a name="supported-database-engines"></a><span data-ttu-id="ba627-111">Поддерживаемые ядра СУБД</span><span class="sxs-lookup"><span data-stu-id="ba627-111">Supported Database Engines</span></span>

* <span data-ttu-id="ba627-112">FirebirdSql 2.5</span><span class="sxs-lookup"><span data-stu-id="ba627-112">FirebirdSql 2.5</span></span>
* <span data-ttu-id="ba627-113">FirebirdSql 3.X</span><span class="sxs-lookup"><span data-stu-id="ba627-113">FirebirdSql 3.X</span></span>

## <a name="supported-platforms"></a><span data-ttu-id="ba627-114">Поддерживаемые платформы</span><span class="sxs-lookup"><span data-stu-id="ba627-114">Supported Platforms</span></span>

* <span data-ttu-id="ba627-115">.NET Framework (4.5.1 и выше)</span><span class="sxs-lookup"><span data-stu-id="ba627-115">.NET Framework (4.5.1 onwards)</span></span>

* <span data-ttu-id="ba627-116">.NET Core</span><span class="sxs-lookup"><span data-stu-id="ba627-116">.NET Core</span></span>

* <span data-ttu-id="ba627-117">Mono (4.2.0 и выше)</span><span class="sxs-lookup"><span data-stu-id="ba627-117">Mono (4.2.0 onwards)</span></span>
