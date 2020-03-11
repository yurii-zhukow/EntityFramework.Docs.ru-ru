---
title: Службы времени разработки — EF Core
author: bricelam
ms.author: bricelam
ms.date: 10/26/2017
uid: core/miscellaneous/cli/services
ms.openlocfilehash: 57294ab41e7c251b1dafae9d573aa98676c5d939
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/06/2020
ms.locfileid: "78414209"
---
# <a name="design-time-services"></a><span data-ttu-id="54694-102">Службы времени разработки</span><span class="sxs-lookup"><span data-stu-id="54694-102">Design-time services</span></span>

<span data-ttu-id="54694-103">Некоторые службы, используемые этими инструментами, используются только во время разработки.</span><span class="sxs-lookup"><span data-stu-id="54694-103">Some services used by the tools are only used at design time.</span></span> <span data-ttu-id="54694-104">Эти службы управляются отдельно от служб среды выполнения EF Core, чтобы предотвратить их развертывание в приложении.</span><span class="sxs-lookup"><span data-stu-id="54694-104">These services are managed separately from EF Core's runtime services to prevent them from being deployed with your app.</span></span> <span data-ttu-id="54694-105">Чтобы переопределить одну из этих служб (например, службу для создания файлов миграции), добавьте в запускаемый проект реализацию `IDesignTimeServices`.</span><span class="sxs-lookup"><span data-stu-id="54694-105">To override one of these services (for example the service to generate migration files), add an implementation of `IDesignTimeServices` to your startup project.</span></span>

[!code-csharp[Main](../../../../samples/core/Miscellaneous/CommandLine/DesignTimeServices.cs)]
