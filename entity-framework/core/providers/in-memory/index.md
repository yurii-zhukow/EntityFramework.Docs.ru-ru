---
title: Поставщик выполняющейся в памяти базы данных — EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 9af0cba7-7605-4f8f-9cfa-dd616fcb880c
uid: core/providers/in-memory/index
ms.openlocfilehash: fd31c8ef2dc2e35e69f9845933a5578a5ff84c9c
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/06/2020
ms.locfileid: "78413009"
---
# <a name="ef-core-in-memory-database-provider"></a>Поставщик выполняющейся в памяти базы данных EF Core

Этот поставщик базы данных позволяет использовать Entity Framework Core с выполняющейся в памяти базой данных. Это может быть полезно для тестирования несмотря на то, что поставщик SQLite в режиме выполнения в памяти может в большей степени подходить для тестирования реляционных баз данных. Работы над этим поставщиком ведутся в рамках [проекта Entity Framework Core](https://github.com/aspnet/EntityFrameworkCore).

## <a name="install"></a>Установка

Установите [пакет NuGet Microsoft.EntityFrameworkCore.InMemory](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.InMemory/).

### <a name="net-core-cli"></a>[Интерфейс командной строки .NET Core](#tab/dotnet-core-cli)

```dotnetcli
dotnet add package Microsoft.EntityFrameworkCore.InMemory
```

### <a name="visual-studio"></a>[Visual Studio](#tab/vs)

``` powershell
Install-Package Microsoft.EntityFrameworkCore.InMemory
```

***

## <a name="get-started"></a>Приступая к работе

Для начала работы с поставщиком используйте указанные ниже ресурсы.

* [Тестирование с использованием выполнения в памяти](../../miscellaneous/testing/in-memory.md)
* [Тесты примера приложения UnicornStore](https://github.com/rowanmiller/UnicornStore/blob/master/UnicornStore/src/UnicornStore.Tests/Controllers/ShippingControllerTests.cs)

## <a name="supported-database-engines"></a>Поддерживаемые ядра СУБД

База данных во внутрипроцессной памяти (предназначена исключительно для тестирования)
