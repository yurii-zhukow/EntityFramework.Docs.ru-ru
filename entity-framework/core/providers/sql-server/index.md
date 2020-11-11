---
title: Поставщик базы данных Microsoft SQL Server — EF Core
description: Документация для поставщика базы данных, которая позволяет использовать Entity Framework Core с Microsoft SQL Server
author: AndriySvyryd
ms.date: 11/05/2019
uid: core/providers/sql-server/index
ms.openlocfilehash: 4118cd5737ece1ad084bb85d409523d217065353
ms.sourcegitcommit: f3512e3a98e685a3ba409c1d0157ce85cc390cf4
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2020
ms.locfileid: "94430328"
---
# <a name="microsoft-sql-server-ef-core-database-provider"></a>Поставщик базы данных Microsoft SQL Server EF Core

Этот поставщик базы данных позволяет использовать Entity Framework Core с Microsoft SQL Server (включая базу данных SQL Azure). Работы над этим поставщиком ведутся в рамках [проекта Entity Framework Core](https://github.com/dotnet/efcore).

## <a name="install"></a>Установка

Установите [пакет NuGet Microsoft.EntityFrameworkCore.SqlServer](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer/).

### <a name="net-core-cli"></a>[Интерфейс командной строки .NET Core](#tab/dotnet-core-cli)

```dotnetcli
dotnet add package Microsoft.EntityFrameworkCore.SqlServer
```

### <a name="visual-studio"></a>[Visual Studio](#tab/vs)

```powershell
Install-Package Microsoft.EntityFrameworkCore.SqlServer
```

***

> [!NOTE]
> Начиная с версии 3.0.0,поставщик ссылается на Microsoft.Data.SqlClient (предыдущие версии зависят от System.Data.SqlClient). Если проект принимает прямую зависимость от SqlClient, убедитесь, что добавлена ссылка на пакет Microsoft.Data.SqlClient.

## <a name="supported-database-engines"></a>Поддерживаемые ядра СУБД

* Microsoft SQL Server (2012 и последующие версии)
