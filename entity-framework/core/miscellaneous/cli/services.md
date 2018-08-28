---
title: Службы времени разработки — EF Core
author: bricelam
ms.author: bricelam
ms.date: 10/26/2017
ms.openlocfilehash: e1cacdd4f40f9c395d8c88a91df4a92ef27001a8
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/27/2018
ms.locfileid: "42997535"
---
<a name="design-time-services"></a><span data-ttu-id="6538c-102">Службы времени разработки</span><span class="sxs-lookup"><span data-stu-id="6538c-102">Design-time services</span></span>
====================
<span data-ttu-id="6538c-103">Некоторые службы, используемые средствами используются только во время разработки.</span><span class="sxs-lookup"><span data-stu-id="6538c-103">Some services used by the tools are only used at design time.</span></span> <span data-ttu-id="6538c-104">Эти службы управляются отдельно от служб среды выполнения EF Core, чтобы предотвратить их развертывание вместе с вашим приложением.</span><span class="sxs-lookup"><span data-stu-id="6538c-104">These services are managed separately from EF Core's runtime services to prevent them from being deployed with your app.</span></span> <span data-ttu-id="6538c-105">Чтобы переопределить одну из этих служб (например службы, чтобы создать файлы миграции), добавьте реализацию `IDesignTimeServices` для запускаемого проекта.</span><span class="sxs-lookup"><span data-stu-id="6538c-105">To override one of these services (for example the service to generate migration files), add an implementation of `IDesignTimeServices` to your startup project.</span></span>

``` csharp
class MyDesignTimeServices : IDesignTimeServices
{
    public void ConfigureDesignTimeServices(IServiceCollection services)
        => services.AddSingleton<IMigrationsCodeGenerator, MyMigrationsCodeGenerator>()
}
```
