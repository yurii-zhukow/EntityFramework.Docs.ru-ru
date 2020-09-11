---
title: Поставщик выполняющейся в памяти базы данных — EF Core
description: Сведения о поставщике базы данных InMemory Entity Framework Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 9af0cba7-7605-4f8f-9cfa-dd616fcb880c
uid: core/providers/in-memory/index
ms.openlocfilehash: 6af1d61a6ff76b82cc0096edbf095a6338d21109
ms.sourcegitcommit: 7c3939504bb9da3f46bea3443638b808c04227c2
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/09/2020
ms.locfileid: "89619002"
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

* [Тестирование с использованием выполнения в памяти](xref:core/miscellaneous/testing/in-memory)
* [Тесты примера приложения UnicornStore](https://github.com/rowanmiller/UnicornStore/blob/master/UnicornStore/src/UnicornStore.Tests/Controllers/ShippingControllerTests.cs)

## <a name="supported-database-engines"></a>Поддерживаемые ядра СУБД

База данных во внутрипроцессной памяти (предназначена исключительно для тестирования)
