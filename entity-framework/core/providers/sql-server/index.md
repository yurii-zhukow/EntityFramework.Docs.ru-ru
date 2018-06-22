---
title: Поставщик базы данных Microsoft SQL Server — EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 2e007c82-c6e4-45bb-8129-851b79ec1a0a
ms.technology: entity-framework-core
uid: core/providers/sql-server/index
ms.openlocfilehash: 2ed7c0dd127db03d5e7340fde1ef83cf01b30135
ms.sourcegitcommit: b2d94cebdc32edad4fecb07e53fece66437d1b04
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/28/2018
ms.locfileid: "29678654"
---
# <a name="microsoft-sql-server-ef-core-database-provider"></a>Поставщик базы данных Microsoft SQL Server EF Core

Этот поставщик базы данных позволяет использовать Entity Framework Core с Microsoft SQL Server (включая SQL Azure). Работы над этим поставщиком ведутся в рамках [проекта Entity Framework Core](https://github.com/aspnet/EntityFrameworkCore).

## <a name="install"></a>Установка

Установите [пакет NuGet Microsoft.EntityFrameworkCore.SqlServer](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer/).

``` powershell
Install-Package Microsoft.EntityFrameworkCore.SqlServer
```

## <a name="get-started"></a>Приступая к работе

Для начала работы с поставщиком используйте указанные ниже ресурсы.
* [Начало работы в .NET Framework (консоль, WinForms, WPF и т. д.)](../../get-started/full-dotnet/index.md)

* [Начало работы в ASP.NET Core](../../get-started/aspnetcore/index.md)

* [Пример приложения UnicornStore](https://github.com/rowanmiller/UnicornStore/tree/master/UnicornStore)

## <a name="supported-database-engines"></a>Поддерживаемые ядра СУБД

* Microsoft SQL Server (2008 и выше)

## <a name="supported-platforms"></a>Поддерживаемые платформы

* .NET Framework (4.5.1 и выше)

* .NET Core

* Mono (4.2.0 и выше)

      Caution: Using this provider on Mono will make use of the Mono SQL Client implementation, which has a number of known issues. For example, it does not support secure connections (SSL).
