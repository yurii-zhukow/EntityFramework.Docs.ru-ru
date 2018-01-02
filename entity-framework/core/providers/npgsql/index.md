---
title: "Поставщик базы данных Npgsql для PostgreSQL — EF Core"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 5ecd1b22-c68e-4d87-ba39-b0761f4d5b90
ms.technology: entity-framework-core
uid: core/providers/npgsql/index
ms.openlocfilehash: acf2e18d7a608b0d75b571a5ac0199d84f86066b
ms.sourcegitcommit: 5e2d97e731f975cf3405ff3deab2a3c75ad1b969
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/15/2017
---
# <a name="npgsql-ef-core-database-provider-for-postgresql"></a><span data-ttu-id="13a82-102">Поставщик базы данных Npgsql EF Core для PostgreSQL</span><span class="sxs-lookup"><span data-stu-id="13a82-102">Npgsql EF Core Database Provider for PostgreSQL</span></span>

<span data-ttu-id="13a82-103">Этот поставщик базы данных позволяет использовать Entity Framework Core с PostgreSQL.</span><span class="sxs-lookup"><span data-stu-id="13a82-103">This database provider allows Entity Framework Core to be used with PostgreSQL.</span></span> <span data-ttu-id="13a82-104">Его разработка ведется в рамках [проекта Npgsql](http://www.npgsql.org).</span><span class="sxs-lookup"><span data-stu-id="13a82-104">The provider is maintained as part of the [Npgsql project](http://www.npgsql.org).</span></span>

> [!NOTE]  
> <span data-ttu-id="13a82-105">В рамках проекта Entity Framework Core работы над ним не ведутся.</span><span class="sxs-lookup"><span data-stu-id="13a82-105">This provider is not maintained as part of the Entity Framework Core project.</span></span> <span data-ttu-id="13a82-106">Выбирая сторонний поставщик, обязательно оцените качество, лицензирование, поддержку и другие показатели на соответствие вашим требованиям.</span><span class="sxs-lookup"><span data-stu-id="13a82-106">When considering a third party provider, be sure to evaluate quality, licensing, support, etc. to ensure they meet your requirements.</span></span>

## <a name="install"></a><span data-ttu-id="13a82-107">Установка</span><span class="sxs-lookup"><span data-stu-id="13a82-107">Install</span></span>

<span data-ttu-id="13a82-108">Установите [пакет NuGet Npgsql.EntityFrameworkCore.PostgreSQL](https://www.nuget.org/packages/Npgsql.EntityFrameworkCore.PostgreSQL).</span><span class="sxs-lookup"><span data-stu-id="13a82-108">Install the [Npgsql.EntityFrameworkCore.PostgreSQL NuGet package](https://www.nuget.org/packages/Npgsql.EntityFrameworkCore.PostgreSQL).</span></span>

``` powershell
Install-Package Npgsql.EntityFrameworkCore.PostgreSQL
```

## <a name="get-started"></a><span data-ttu-id="13a82-109">Приступая к работе</span><span class="sxs-lookup"><span data-stu-id="13a82-109">Get Started</span></span>

<span data-ttu-id="13a82-110">Чтобы приступить к работе, см. [документацию по Npgsql](http://www.npgsql.org/efcore/index.html).</span><span class="sxs-lookup"><span data-stu-id="13a82-110">See the [Npgsql documentation](http://www.npgsql.org/efcore/index.html) to get started.</span></span>

## <a name="supported-database-engines"></a><span data-ttu-id="13a82-111">Поддерживаемые ядра СУБД</span><span class="sxs-lookup"><span data-stu-id="13a82-111">Supported Database Engines</span></span>

* <span data-ttu-id="13a82-112">PostgreSQL</span><span class="sxs-lookup"><span data-stu-id="13a82-112">PostgreSQL</span></span>

## <a name="supported-platforms"></a><span data-ttu-id="13a82-113">Поддерживаемые платформы</span><span class="sxs-lookup"><span data-stu-id="13a82-113">Supported Platforms</span></span>

* <span data-ttu-id="13a82-114">.NET Framework (4.5.1 и выше)</span><span class="sxs-lookup"><span data-stu-id="13a82-114">.NET Framework (4.5.1 onwards)</span></span>

* <span data-ttu-id="13a82-115">.NET Core</span><span class="sxs-lookup"><span data-stu-id="13a82-115">.NET Core</span></span>

* <span data-ttu-id="13a82-116">Mono (4.2.0 и выше)</span><span class="sxs-lookup"><span data-stu-id="13a82-116">Mono (4.2.0 onwards)</span></span>
