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
<a name="design-time-services"></a>Службы времени разработки
====================
Некоторые службы, используемые в средствах используются только во время разработки. Эти службы управляются отдельно от основной EF службы времени выполнения для предотвращения развертываемого вместе с приложением. Чтобы переопределить один из этих служб (например службы, чтобы создать файлы миграции), добавьте реализацию `IDesignTimeServices` для запускаемого проекта.

``` csharp
class MyDesignTimeServices : IDesignTimeServices
{
    public void ConfigureDesignTimeServices(IServiceCollection services)
        => services.AddSingleton<IMigrationsCodeGenerator, MyMigrationsCodeGenerator>()
}
```
