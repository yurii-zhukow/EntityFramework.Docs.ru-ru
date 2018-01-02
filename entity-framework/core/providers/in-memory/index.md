---
title: "Поставщик выполняющейся в памяти базы данных — EF Core"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 9af0cba7-7605-4f8f-9cfa-dd616fcb880c
ms.technology: entity-framework-core
uid: core/providers/in-memory/index
ms.openlocfilehash: a8e05f50837f3da554b338475d24215706dfa2ec
ms.sourcegitcommit: 5e2d97e731f975cf3405ff3deab2a3c75ad1b969
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/15/2017
---
# <a name="ef-core-in-memory-database-provider"></a>Поставщик выполняющейся в памяти базы данных EF Core

Этот поставщик базы данных позволяет использовать Entity Framework Core с выполняющейся в памяти базой данных. Это удобно при тестировании кода, использующего Entity Framework Core. Его разработка ведется в рамках [проекта GitHub EntityFramework](https://github.com/aspnet/EntityFramework).

## <a name="install"></a>Установка

Установите [пакет NuGet Microsoft.EntityFrameworkCore.InMemory](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.InMemory/).

``` powershell
Install-Package Microsoft.EntityFrameworkCore.InMemory
```

## <a name="get-started"></a>Приступая к работе

Для начала работы с поставщиком используйте указанные ниже ресурсы.
* [Тестирование с использованием выполнения в памяти](../../miscellaneous/testing/in-memory.md)

* [Тесты примера приложения UnicornStore](https://github.com/rowanmiller/UnicornStore/blob/master/UnicornStore/src/UnicornStore.Tests/Controllers/ShippingControllerTests.cs)

## <a name="supported-database-engines"></a>Поддерживаемые ядра СУБД

* Встроенная выполняющаяся в памяти база (предназначена исключительно для тестирования)

## <a name="supported-platforms"></a>Поддерживаемые платформы

* .NET Framework (4.5.1 и выше)

* .NET Core

* Mono (4.2.0 и выше)

* Универсальная платформа Windows 
