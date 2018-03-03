---
title: "Поставщик базы данных Npgsql для PostgreSQL — EF Core"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 5ecd1b22-c68e-4d87-ba39-b0761f4d5b90
ms.technology: entity-framework-core
uid: core/providers/npgsql/index
ms.openlocfilehash: acf2e18d7a608b0d75b571a5ac0199d84f86066b
ms.sourcegitcommit: 6ed04bb05a3d05c367f0f55616807af2bf4037ae
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/27/2018
---
# <a name="npgsql-ef-core-database-provider-for-postgresql"></a>Поставщик базы данных Npgsql EF Core для PostgreSQL

Этот поставщик базы данных позволяет использовать Entity Framework Core с PostgreSQL. Его разработка ведется в рамках [проекта Npgsql](http://www.npgsql.org).

> [!NOTE]  
> В рамках проекта Entity Framework Core работы над ним не ведутся. Выбирая сторонний поставщик, обязательно оцените качество, лицензирование, поддержку и другие показатели на соответствие вашим требованиям.

## <a name="install"></a>Установка

Установите [пакет NuGet Npgsql.EntityFrameworkCore.PostgreSQL](https://www.nuget.org/packages/Npgsql.EntityFrameworkCore.PostgreSQL).

``` powershell
Install-Package Npgsql.EntityFrameworkCore.PostgreSQL
```

## <a name="get-started"></a>Приступая к работе

Чтобы приступить к работе, см. [документацию по Npgsql](http://www.npgsql.org/efcore/index.html).

## <a name="supported-database-engines"></a>Поддерживаемые ядра СУБД

* PostgreSQL

## <a name="supported-platforms"></a>Поддерживаемые платформы

* .NET Framework (4.5.1 и выше)

* .NET Core

* Mono (4.2.0 и выше)
