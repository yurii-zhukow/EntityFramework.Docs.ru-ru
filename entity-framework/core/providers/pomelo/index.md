---
title: "Поставщик базы данных Pomelo MySQL — EF Core"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: d0198c04-d30d-4419-98f8-a54690cea3c8
ms.technology: entity-framework-core
uid: core/providers/pomelo/index
ms.openlocfilehash: 9ddcda1ae7b058c01937867817e2666b97e7f37a
ms.sourcegitcommit: 5e2d97e731f975cf3405ff3deab2a3c75ad1b969
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/15/2017
---
# <a name="pomelo-ef-core-database-provider-for-mysql"></a><span data-ttu-id="61f20-102">Поставщик базы данных Pomelo EF Core для MySQL</span><span class="sxs-lookup"><span data-stu-id="61f20-102">Pomelo EF Core Database Provider for MySQL</span></span>

<span data-ttu-id="61f20-103">Этот поставщик базы данных позволяет использовать Entity Framework Core с MySQL.</span><span class="sxs-lookup"><span data-stu-id="61f20-103">This database provider allows Entity Framework Core to be used with MySQL.</span></span> <span data-ttu-id="61f20-104">Его разработка ведется в рамках [проекта Pomelo Foundation](https://github.com/PomeloFoundation/Pomelo.EntityFrameworkCore.MySql).</span><span class="sxs-lookup"><span data-stu-id="61f20-104">The provider is maintained as part of the [Pomelo Foundation Project](https://github.com/PomeloFoundation/Pomelo.EntityFrameworkCore.MySql).</span></span>

> [!NOTE]  
>
> <span data-ttu-id="61f20-105">В рамках проекта Entity Framework Core работы над ним не ведутся.</span><span class="sxs-lookup"><span data-stu-id="61f20-105">This provider is not maintained as part of the Entity Framework Core project.</span></span> <span data-ttu-id="61f20-106">Выбирая сторонний поставщик, обязательно оцените качество, лицензирование, поддержку и другие показатели на соответствие вашим требованиям.</span><span class="sxs-lookup"><span data-stu-id="61f20-106">When considering a third party provider, be sure to evaluate quality, licensing, support, etc. to ensure they meet your requirements.</span></span>

## <a name="install"></a><span data-ttu-id="61f20-107">Установка</span><span class="sxs-lookup"><span data-stu-id="61f20-107">Install</span></span>

<span data-ttu-id="61f20-108">Установите [пакет NuGet Pomelo.EntityFrameworkCore.MySql](https://www.nuget.org/packages/Pomelo.EntityFrameworkCore.MySql).</span><span class="sxs-lookup"><span data-stu-id="61f20-108">Install the [Pomelo.EntityFrameworkCore.MySql NuGet package](https://www.nuget.org/packages/Pomelo.EntityFrameworkCore.MySql).</span></span>

``` powershell
Install-Package Pomelo.EntityFrameworkCore.MySql
```

## <a name="get-started"></a><span data-ttu-id="61f20-109">Приступая к работе</span><span class="sxs-lookup"><span data-stu-id="61f20-109">Get Started</span></span>

<span data-ttu-id="61f20-110">Для начала работы с поставщиком используйте указанные ниже ресурсы.</span><span class="sxs-lookup"><span data-stu-id="61f20-110">The following resources will help you get started with this provider.</span></span>
* [<span data-ttu-id="61f20-111">Документация по началу работы</span><span class="sxs-lookup"><span data-stu-id="61f20-111">Getting started documentation</span></span>](https://github.com/PomeloFoundation/Pomelo.EntityFrameworkCore.MySql/blob/master/README.md#getting-started)

* [<span data-ttu-id="61f20-112">Пример приложения в блоге Юко (Yuuko)</span><span class="sxs-lookup"><span data-stu-id="61f20-112">Yuuko Blog sample application</span></span>](https://github.com/PomeloFoundation/YuukoBlog)

## <a name="supported-database-engines"></a><span data-ttu-id="61f20-113">Поддерживаемые ядра СУБД</span><span class="sxs-lookup"><span data-stu-id="61f20-113">Supported Database Engines</span></span>

* <span data-ttu-id="61f20-114">MySQL</span><span class="sxs-lookup"><span data-stu-id="61f20-114">MySQL</span></span>
* <span data-ttu-id="61f20-115">MariaDB</span><span class="sxs-lookup"><span data-stu-id="61f20-115">MariaDB</span></span>

## <a name="supported-platforms"></a><span data-ttu-id="61f20-116">Поддерживаемые платформы</span><span class="sxs-lookup"><span data-stu-id="61f20-116">Supported Platforms</span></span>

* <span data-ttu-id="61f20-117">.NET Framework (4.5.1 и выше)</span><span class="sxs-lookup"><span data-stu-id="61f20-117">.NET Framework (4.5.1 onwards)</span></span>

* <span data-ttu-id="61f20-118">.NET Core</span><span class="sxs-lookup"><span data-stu-id="61f20-118">.NET Core</span></span>

* <span data-ttu-id="61f20-119">Mono (4.2.0 и выше)</span><span class="sxs-lookup"><span data-stu-id="61f20-119">Mono (4.2.0 onwards)</span></span>
