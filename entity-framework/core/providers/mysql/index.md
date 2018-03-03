---
title: "Поставщик базы данных MySQL — EF Core"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 4900b882-79c5-40d2-a44a-ccb0292f6ed9
ms.technology: entity-framework-core
uid: core/providers/mysql/index
ms.openlocfilehash: 1500d017cb463c3f394131a79b9063ff90cce5e2
ms.sourcegitcommit: 6ed04bb05a3d05c367f0f55616807af2bf4037ae
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/27/2018
---
# <a name="mysql-ef-core-database-provider"></a>Поставщик базы данных MySQL EF Core

Этот поставщик базы данных позволяет использовать Entity Framework Core с MySQL. Его разработка ведется в рамках [проекта MySQL](http://dev.mysql.com).

> [!WARNING]  
> Этот поставщик находится на этапе предварительной версии.

> [!NOTE]  
> В рамках проекта Entity Framework Core работы над ним не ведутся. Выбирая сторонний поставщик, обязательно оцените качество, лицензирование, поддержку и другие показатели на соответствие вашим требованиям.

## <a name="install"></a>Установка

Установите [пакет NuGet MySql.Data.EntityFrameworkCore](https://www.nuget.org/packages/MySql.Data.EntityFrameworkCore).

``` powershell
Install-Package MySql.Data.EntityFrameworkCore -Pre
```

## <a name="get-started"></a>Приступая к работе

См. статью [Начало работы с поставщиком MySQL EF Core и Connector/Net 7.0.4](http://insidemysql.com/howto-starting-with-mysql-ef-core-provider-and-connectornet-7-0-4/).

## <a name="supported-database-engines"></a>Поддерживаемые ядра СУБД

* MySQL

## <a name="supported-platforms"></a>Поддерживаемые платформы

* .NET Framework (4.5.1 и выше)

* .NET Core

Обязательно ознакомьтесь с документацией MySQL по совместимости версий [здесь](https://dev.mysql.com/doc/connector-net/en/connector-net-versions.html) и [здесь](https://dev.mysql.com/doc/connector-net/en/connector-net-entityframework-core.html)
