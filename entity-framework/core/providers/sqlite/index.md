---
title: Поставщик базы данных SQLite — EF Core
description: Сведения о поставщике базы данных SQLite Entity Framework Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 3e2f7698-fec2-4cec-9e2d-2e3e0074120c
uid: core/providers/sqlite/index
ms.openlocfilehash: 8620f0e37825368cb3d7965a05118ab1297fb9e8
ms.sourcegitcommit: 7c3939504bb9da3f46bea3443638b808c04227c2
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/09/2020
ms.locfileid: "89616559"
---
# <a name="sqlite-ef-core-database-provider"></a>Поставщик базы данных SQLite EF Core

Этот поставщик базы данных позволяет использовать Entity Framework Core с SQLite. Работы над этим поставщиком ведутся в рамках [проекта Entity Framework Core](https://github.com/aspnet/EntityFrameworkCore).

## <a name="install"></a>Установка

Установите [пакет NuGet Microsoft.EntityFrameworkCore.Sqlite](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Sqlite/).

### <a name="net-core-cli"></a>[Интерфейс командной строки .NET Core](#tab/dotnet-core-cli)

```dotnetcli
dotnet add package Microsoft.EntityFrameworkCore.Sqlite
```

### <a name="visual-studio"></a>[Visual Studio](#tab/vs)

``` powershell
Install-Package Microsoft.EntityFrameworkCore.Sqlite
```

***

## <a name="supported-database-engines"></a>Поддерживаемые ядра СУБД

* SQLite (3.7 и выше)

## <a name="limitations"></a>Ограничения

Сведения о некоторых важных ограничениях поставщика SQLite см. в статье [Ограничения SQLite](xref:core/providers/sqlite/limitations).
