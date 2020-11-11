---
title: Поставщик базы данных SQLite — EF Core
description: Сведения о поставщике базы данных SQLite Entity Framework Core
author: bricelam
ms.date: 10/27/2016
uid: core/providers/sqlite/index
ms.openlocfilehash: 6dac555456ed347b3eb07bbd0f2a89ab9179d20e
ms.sourcegitcommit: f3512e3a98e685a3ba409c1d0157ce85cc390cf4
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2020
ms.locfileid: "94430304"
---
# <a name="sqlite-ef-core-database-provider"></a>Поставщик базы данных SQLite EF Core

Этот поставщик базы данных позволяет использовать Entity Framework Core с SQLite. Работы над этим поставщиком ведутся в рамках [проекта Entity Framework Core](https://github.com/dotnet/efcore).

## <a name="install"></a>Установка

Установите [пакет NuGet Microsoft.EntityFrameworkCore.Sqlite](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Sqlite/).

### <a name="net-core-cli"></a>[Интерфейс командной строки .NET Core](#tab/dotnet-core-cli)

```dotnetcli
dotnet add package Microsoft.EntityFrameworkCore.Sqlite
```

### <a name="visual-studio"></a>[Visual Studio](#tab/vs)

```powershell
Install-Package Microsoft.EntityFrameworkCore.Sqlite
```

***

## <a name="supported-database-engines"></a>Поддерживаемые ядра СУБД

* SQLite (3.7 и выше)

## <a name="limitations"></a>Ограничения

Сведения о некоторых важных ограничениях поставщика SQLite см. в статье [Ограничения SQLite](xref:core/providers/sqlite/limitations).
