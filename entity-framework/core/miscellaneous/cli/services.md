---
title: Службы времени разработки — EF Core
description: Сведения о Entity Framework Core служб времени разработки
author: bricelam
ms.date: 10/26/2017
uid: core/miscellaneous/cli/services
ms.openlocfilehash: 6778a1fba025e9fbce1bb3e43462b0ee7c5dcfc4
ms.sourcegitcommit: 0a25c03fa65ae6e0e0e3f66bac48d59eceb96a5a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/14/2020
ms.locfileid: "92061948"
---
# <a name="design-time-services"></a><span data-ttu-id="41997-103">Службы времени разработки</span><span class="sxs-lookup"><span data-stu-id="41997-103">Design-time services</span></span>

<span data-ttu-id="41997-104">Некоторые службы, используемые этими инструментами, используются только во время разработки.</span><span class="sxs-lookup"><span data-stu-id="41997-104">Some services used by the tools are only used at design time.</span></span> <span data-ttu-id="41997-105">Эти службы управляются отдельно от служб среды выполнения EF Core, чтобы предотвратить их развертывание в приложении.</span><span class="sxs-lookup"><span data-stu-id="41997-105">These services are managed separately from EF Core's runtime services to prevent them from being deployed with your app.</span></span> <span data-ttu-id="41997-106">Чтобы переопределить одну из этих служб (например, службу для создания файлов миграции), добавьте реализацию `IDesignTimeServices` в запускаемый проект.</span><span class="sxs-lookup"><span data-stu-id="41997-106">To override one of these services (for example the service to generate migration files), add an implementation of `IDesignTimeServices` to your startup project.</span></span>

[!code-csharp[Main](../../../../samples/core/Miscellaneous/CommandLine/DesignTimeServices.cs)]
