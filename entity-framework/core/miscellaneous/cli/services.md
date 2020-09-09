---
title: Службы времени разработки — EF Core
description: Сведения о Entity Framework Core служб времени разработки
author: bricelam
ms.author: bricelam
ms.date: 10/26/2017
uid: core/miscellaneous/cli/services
ms.openlocfilehash: aa761c4a20e0848a77aa7b4ad625124a1d372a70
ms.sourcegitcommit: 7c3939504bb9da3f46bea3443638b808c04227c2
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/09/2020
ms.locfileid: "89617848"
---
# <a name="design-time-services"></a><span data-ttu-id="8bda5-103">Службы времени разработки</span><span class="sxs-lookup"><span data-stu-id="8bda5-103">Design-time services</span></span>

<span data-ttu-id="8bda5-104">Некоторые службы, используемые этими инструментами, используются только во время разработки.</span><span class="sxs-lookup"><span data-stu-id="8bda5-104">Some services used by the tools are only used at design time.</span></span> <span data-ttu-id="8bda5-105">Эти службы управляются отдельно от служб среды выполнения EF Core, чтобы предотвратить их развертывание в приложении.</span><span class="sxs-lookup"><span data-stu-id="8bda5-105">These services are managed separately from EF Core's runtime services to prevent them from being deployed with your app.</span></span> <span data-ttu-id="8bda5-106">Чтобы переопределить одну из этих служб (например, службу для создания файлов миграции), добавьте реализацию `IDesignTimeServices` в запускаемый проект.</span><span class="sxs-lookup"><span data-stu-id="8bda5-106">To override one of these services (for example the service to generate migration files), add an implementation of `IDesignTimeServices` to your startup project.</span></span>

[!code-csharp[Main](../../../../samples/core/Miscellaneous/CommandLine/DesignTimeServices.cs)]
