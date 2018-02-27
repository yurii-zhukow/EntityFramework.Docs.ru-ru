---
title: "Поставщик базы данных IBM Data Server (DB2) — EF Core"
author: rowanmiller
ms.author: divega
ms.date: 02/15/2017
ms.assetid: 825e5332-5aa3-4600-9efb-ab71aaff59ec
ms.technology: entity-framework-core
uid: core/providers/ibm/index
ms.openlocfilehash: a9caa8df63850d4f6b5f2164dad7ac5af7504076
ms.sourcegitcommit: 5e2d97e731f975cf3405ff3deab2a3c75ad1b969
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/15/2017
---
# <a name="ibm-data-server-db2-ef-core-database-providers"></a>Поставщик базы данных IBM Data Server (DB2) EF Core

Этот поставщик базы данных позволяет использовать Entity Framework Core с IBM Data Server. Поставщик разрабатывается компанией IBM.

> [!NOTE]  
> В рамках проекта Entity Framework Core работы над ним не ведутся. Выбирая сторонний поставщик, обязательно оцените качество, лицензирование, поддержку и другие показатели на соответствие вашим требованиям.

## <a name="install"></a>Установка

Для работы с IBM Data Server в Windows установите [пакет NuGet IBM.EntityFrameworkCore](https://www.nuget.org/packages/IBM.EntityFrameworkCore).

``` powershell
Install-Package IBM.EntityFrameworkCore
```

Для работы с IBM Data Server в Linux установите [пакет NuGet IBM.EntityFrameworkCore-lnx](https://www.nuget.org/packages/IBM.EntityFrameworkCore-lnx).

``` powershell
Install-Package IBM.EntityFrameworkCore-lnx
```

## <a name="get-started"></a>Приступая к работе

[Начало работы с поставщиком IBM .NET для .NET Core](https://www.ibm.com/developerworks/community/blogs/96960515-2ea1-4391-8170-b0515d08e4da/entry/DB2DotnetCore?lang=en)

## <a name="supported-database-engines"></a>Поддерживаемые ядра СУБД

* zOS
* LUW, включая IBM dashDB
* IBM I
* Informix

## <a name="supported-platforms"></a>Поддерживаемые платформы

* .NET Framework (4.6 и выше)
* .NET Core
