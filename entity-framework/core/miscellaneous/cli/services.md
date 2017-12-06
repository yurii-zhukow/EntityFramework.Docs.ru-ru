---
title: "Службы времени проектирования - EF Core"
author: bricelam
ms.author: bricelam
ms.date: 10/26/2017
ms.technology: entity-framework-core
ms.openlocfilehash: f9c8208a59bfcefeaab01ea69e65fe809a0b3d89
ms.sourcegitcommit: 5e2d97e731f975cf3405ff3deab2a3c75ad1b969
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/15/2017
---
<a name="design-time-services"></a><span data-ttu-id="6e219-102">Службы времени разработки</span><span class="sxs-lookup"><span data-stu-id="6e219-102">Design-time services</span></span>
====================
<span data-ttu-id="6e219-103">Некоторые службы, используемые в средствах используются только во время разработки.</span><span class="sxs-lookup"><span data-stu-id="6e219-103">Some services used by the tools are only used at design time.</span></span> <span data-ttu-id="6e219-104">Эти службы управляются отдельно от основной EF службы времени выполнения для предотвращения развертываемого вместе с приложением.</span><span class="sxs-lookup"><span data-stu-id="6e219-104">These services are managed separately from EF Core's runtime services to prevent them from being deployed with your app.</span></span> <span data-ttu-id="6e219-105">Чтобы переопределить один из этих служб (например службы, чтобы создать файлы миграции), добавьте реализацию `IDesignTimeServices` для запускаемого проекта.</span><span class="sxs-lookup"><span data-stu-id="6e219-105">To override one of these services (for example the service to generate migration files), add an implementation of `IDesignTimeServices` to your startup project.</span></span>

``` csharp
class MyDesignTimeServices : IDesignTimeServices
{
    public void ConfigureDesignTimeServices(IServiceCollection services)
        => services.AddSingleton<IMigrationsCodeGenerator, MyMigrationsCodeGenerator>()
}
```
