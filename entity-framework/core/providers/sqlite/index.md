---
title: Поставщик базы данных SQLite — EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 3e2f7698-fec2-4cec-9e2d-2e3e0074120c
uid: core/providers/sqlite/index
ms.openlocfilehash: 4637044b1712280d3cdca6bcca1ca61564ff78ea
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/06/2019
ms.locfileid: "73656074"
---
# <a name="sqlite-ef-core-database-provider"></a>Поставщик базы данных SQLite EF Core

Этот поставщик базы данных позволяет использовать Entity Framework Core с SQLite. Работы над этим поставщиком ведутся в рамках [проекта Entity Framework Core](https://github.com/aspnet/EntityFrameworkCore).

## <a name="install"></a>Установка

Установите [пакет NuGet Microsoft.EntityFrameworkCore.Sqlite](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Sqlite/).

## <a name="net-core-clitabdotnet-core-cli"></a>[Интерфейс командной строки .NET Core](#tab/dotnet-core-cli)

``` console
dotnet add package Microsoft.EntityFrameworkCore.Sqlite
```

## <a name="visual-studiotabvs"></a>[Visual Studio](#tab/vs)

``` powershell
Install-Package Microsoft.EntityFrameworkCore.Sqlite
```

***

## <a name="supported-database-engines"></a>Поддерживаемые ядра СУБД

* SQLite (3.7 и выше)

## <a name="limitations"></a>Ограничения

Сведения о некоторых важных ограничениях поставщика SQLite см. в статье [Ограничения SQLite](limitations.md).
