---
title: "Поставщик базы данных MySQL — EF Core"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 4900b882-79c5-40d2-a44a-ccb0292f6ed9
ms.technology: entity-framework-core
uid: core/providers/mysql/index
ms.openlocfilehash: c151845c8b08ef6a668b352f15545752156b0a9d
ms.sourcegitcommit: 5e2d97e731f975cf3405ff3deab2a3c75ad1b969
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/15/2017
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
