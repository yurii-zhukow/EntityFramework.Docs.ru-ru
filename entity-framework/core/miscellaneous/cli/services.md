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
# <a name="design-time-services"></a>Службы времени разработки

Некоторые службы, используемые этими инструментами, используются только во время разработки. Эти службы управляются отдельно от служб среды выполнения EF Core, чтобы предотвратить их развертывание в приложении. Чтобы переопределить одну из этих служб (например, службу для создания файлов миграции), добавьте в запускаемый проект реализацию `IDesignTimeServices`.

``` csharp
class MyDesignTimeServices : IDesignTimeServices
{
    public void ConfigureDesignTimeServices(IServiceCollection services)
        => services.AddSingleton<IMigrationsCodeGenerator, MyMigrationsCodeGenerator>()
}
```
