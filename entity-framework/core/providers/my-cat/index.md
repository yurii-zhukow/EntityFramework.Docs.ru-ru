---
title: "Поставщик базы данных Pomelo MyCat — EF Core"
author: rowanmiller
ms.author: divega
ms.date: 02/27/2017
ms.assetid: ad798f02-a7a4-45c1-b0a9-8e92f5dc6ff0
ms.technology: entity-framework-core
uid: core/providers/my-cat/index
ms.openlocfilehash: e13da3ab47e56d1b869e445f2398eda6ea84a79f
ms.sourcegitcommit: 6ed04bb05a3d05c367f0f55616807af2bf4037ae
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/27/2018
---
# <a name="pomelo-mycat-ef-core-database-provider"></a><span data-ttu-id="914c8-102">Поставщик базы данных Pomelo MyCat EF Core</span><span class="sxs-lookup"><span data-stu-id="914c8-102">Pomelo MyCat EF Core Database Provider</span></span>

<span data-ttu-id="914c8-103">Этот поставщик базы данных позволяет использовать Entity Framework Core с [MyCat](https://github.com/MyCATApache/Mycat-Server).</span><span class="sxs-lookup"><span data-stu-id="914c8-103">This database provider allows Entity Framework Core to be used with [MyCat](https://github.com/MyCATApache/Mycat-Server).</span></span> <span data-ttu-id="914c8-104">Его разработка ведется в рамках [проекта Pomelo Foundation](https://github.com/PomeloFoundation/Entity-Framework-Core-MyCat-Proxy).</span><span class="sxs-lookup"><span data-stu-id="914c8-104">The provider is maintained as part of the [Pomelo Foundation Project](https://github.com/PomeloFoundation/Entity-Framework-Core-MyCat-Proxy).</span></span>

> [!NOTE]  
> <span data-ttu-id="914c8-105">В рамках проекта Entity Framework Core работы над ним не ведутся.</span><span class="sxs-lookup"><span data-stu-id="914c8-105">This provider is not maintained as part of the Entity Framework Core project.</span></span> <span data-ttu-id="914c8-106">Выбирая сторонний поставщик, обязательно оцените качество, лицензирование, поддержку и другие показатели на соответствие вашим требованиям.</span><span class="sxs-lookup"><span data-stu-id="914c8-106">When considering a third party provider, be sure to evaluate quality, licensing, support, etc. to ensure they meet your requirements.</span></span>

## <a name="install"></a><span data-ttu-id="914c8-107">Установка</span><span class="sxs-lookup"><span data-stu-id="914c8-107">Install</span></span>

<span data-ttu-id="914c8-108">Скачайте и запустите [последний выпуск с сайта проекта](https://github.com/PomeloFoundation/Entity-Framework-Core-MyCat-Proxy/releases).</span><span class="sxs-lookup"><span data-stu-id="914c8-108">Download and run [the latest release from the project site](https://github.com/PomeloFoundation/Entity-Framework-Core-MyCat-Proxy/releases).</span></span>

## <a name="get-started"></a><span data-ttu-id="914c8-109">Приступая к работе</span><span class="sxs-lookup"><span data-stu-id="914c8-109">Get Started</span></span>

<span data-ttu-id="914c8-110">Для начала работы с поставщиком используйте указанные ниже ресурсы.</span><span class="sxs-lookup"><span data-stu-id="914c8-110">The following resources will help you get started with this provider.</span></span>
 * [<span data-ttu-id="914c8-111">Этапы настройки</span><span class="sxs-lookup"><span data-stu-id="914c8-111">Setup steps</span></span>](https://github.com/aspnet/EntityFramework.Docs/issues/252)
 * [<span data-ttu-id="914c8-112">Видео о начале работы</span><span class="sxs-lookup"><span data-stu-id="914c8-112">Getting started video</span></span>](https://www.youtube.com/watch?v=q0CXfFNtMZo)

## <a name="supported-database-engines"></a><span data-ttu-id="914c8-113">Поддерживаемые ядра СУБД</span><span class="sxs-lookup"><span data-stu-id="914c8-113">Supported Database Engines</span></span>

* <span data-ttu-id="914c8-114">MyCat</span><span class="sxs-lookup"><span data-stu-id="914c8-114">MyCat</span></span>

## <a name="supported-platforms"></a><span data-ttu-id="914c8-115">Поддерживаемые платформы</span><span class="sxs-lookup"><span data-stu-id="914c8-115">Supported Platforms</span></span>

* <span data-ttu-id="914c8-116">.NET Framework (4.5.1 и выше)</span><span class="sxs-lookup"><span data-stu-id="914c8-116">.NET Framework (4.5.1 onwards)</span></span>
* <span data-ttu-id="914c8-117">.NET Core</span><span class="sxs-lookup"><span data-stu-id="914c8-117">.NET Core</span></span>
* <span data-ttu-id="914c8-118">Mono (4.2.0 и выше)</span><span class="sxs-lookup"><span data-stu-id="914c8-118">Mono (4.2.0 onwards)</span></span>
