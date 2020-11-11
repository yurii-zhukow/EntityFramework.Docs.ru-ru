---
title: Поставщик выполняющейся в памяти базы данных — EF Core
description: Сведения о поставщике базы данных InMemory Entity Framework Core
author: ajcvickers
ms.date: 10/27/2016
uid: core/providers/in-memory/index
ms.openlocfilehash: 1af75088ae892e3b428caf6bdb31dd2b750a05fe
ms.sourcegitcommit: f3512e3a98e685a3ba409c1d0157ce85cc390cf4
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2020
ms.locfileid: "94430174"
---
# <a name="ef-core-in-memory-database-provider"></a>Поставщик выполняющейся в памяти базы данных EF Core

Этот поставщик базы данных позволяет использовать Entity Framework Core с выполняющейся в памяти базой данных. База данных в памяти может быть удобна для тестирования, хотя поставщик SQLite в памяти может лучше подходить для тестирования реляционных баз данных. База данных в памяти предназначена только для тестирования. Работы над этим поставщиком ведутся в рамках [проекта Entity Framework Core](https://github.com/dotnet/efcore).

## <a name="install"></a>Установка

Установите [пакет NuGet Microsoft.EntityFrameworkCore.InMemory](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.InMemory/).

### <a name="net-core-cli"></a>[Интерфейс командной строки .NET Core](#tab/dotnet-core-cli)

```dotnetcli
dotnet add package Microsoft.EntityFrameworkCore.InMemory
```

### <a name="visual-studio"></a>[Visual Studio](#tab/vs)

```powershell
Install-Package Microsoft.EntityFrameworkCore.InMemory
```

***

## <a name="get-started"></a>Приступая к работе

Для начала работы с поставщиком используйте указанные ниже ресурсы.

* [Тестирование с использованием выполнения в памяти](xref:core/testing/in-memory)
* [Тесты примера приложения UnicornStore](https://github.com/rowanmiller/UnicornStore/blob/master/UnicornStore/src/UnicornStore.Tests/Controllers/ShippingControllerTests.cs)

## <a name="supported-database-engines"></a>Поддерживаемые ядра СУБД

База данных во внутрипроцессной памяти, предназначенная исключительно для тестирования.
