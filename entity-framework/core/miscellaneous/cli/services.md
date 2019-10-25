---
title: Службы времени разработки — EF Core
author: bricelam
ms.author: bricelam
ms.date: 10/26/2017
uid: core/miscellaneous/cli/services
ms.openlocfilehash: ff0243a588d5e957aed89fcf1ce7462b5b9a747a
ms.sourcegitcommit: 2355447d89496a8ca6bcbfc0a68a14a0bf7f0327
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/23/2019
ms.locfileid: "72811947"
---
# <a name="design-time-services"></a><span data-ttu-id="c0ef5-102">Службы времени разработки</span><span class="sxs-lookup"><span data-stu-id="c0ef5-102">Design-time services</span></span>

<span data-ttu-id="c0ef5-103">Некоторые службы, используемые этими инструментами, используются только во время разработки.</span><span class="sxs-lookup"><span data-stu-id="c0ef5-103">Some services used by the tools are only used at design time.</span></span> <span data-ttu-id="c0ef5-104">Эти службы управляются отдельно от служб среды выполнения EF Core, чтобы предотвратить их развертывание в приложении.</span><span class="sxs-lookup"><span data-stu-id="c0ef5-104">These services are managed separately from EF Core's runtime services to prevent them from being deployed with your app.</span></span> <span data-ttu-id="c0ef5-105">Чтобы переопределить одну из этих служб (например, службу для создания файлов миграции), добавьте в запускаемый проект реализацию `IDesignTimeServices`.</span><span class="sxs-lookup"><span data-stu-id="c0ef5-105">To override one of these services (for example the service to generate migration files), add an implementation of `IDesignTimeServices` to your startup project.</span></span>

``` csharp
class MyDesignTimeServices : IDesignTimeServices
{
    public void ConfigureDesignTimeServices(IServiceCollection services)
        => services.AddSingleton<IMigrationsCodeGenerator, MyMigrationsCodeGenerator>()
}
```
