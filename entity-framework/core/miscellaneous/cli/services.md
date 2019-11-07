---
title: Службы времени разработки — EF Core
author: bricelam
ms.author: bricelam
ms.date: 10/26/2017
uid: core/miscellaneous/cli/services
ms.openlocfilehash: 57294ab41e7c251b1dafae9d573aa98676c5d939
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/06/2019
ms.locfileid: "73655825"
---
# <a name="design-time-services"></a><span data-ttu-id="a87a4-102">Службы времени разработки</span><span class="sxs-lookup"><span data-stu-id="a87a4-102">Design-time services</span></span>

<span data-ttu-id="a87a4-103">Некоторые службы, используемые этими инструментами, используются только во время разработки.</span><span class="sxs-lookup"><span data-stu-id="a87a4-103">Some services used by the tools are only used at design time.</span></span> <span data-ttu-id="a87a4-104">Эти службы управляются отдельно от служб среды выполнения EF Core, чтобы предотвратить их развертывание в приложении.</span><span class="sxs-lookup"><span data-stu-id="a87a4-104">These services are managed separately from EF Core's runtime services to prevent them from being deployed with your app.</span></span> <span data-ttu-id="a87a4-105">Чтобы переопределить одну из этих служб (например, службу для создания файлов миграции), добавьте в запускаемый проект реализацию `IDesignTimeServices`.</span><span class="sxs-lookup"><span data-stu-id="a87a4-105">To override one of these services (for example the service to generate migration files), add an implementation of `IDesignTimeServices` to your startup project.</span></span>

[!code-csharp[Main](../../../../samples/core/Miscellaneous/CommandLine/DesignTimeServices.cs)]
