---
title: Поставщик базы данных Microsoft SQL Server — EF Core
description: Документация для поставщика базы данных, которая позволяет использовать Entity Framework Core с Microsoft SQL Server
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 11/05/2019
uid: core/providers/sql-server/index
ms.openlocfilehash: baae668a7ec255e35ab0e23e5c5ddfa47bda917e
ms.sourcegitcommit: 32c51c22988c6f83ed4f8e50a1d01be3f4114e81
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/27/2019
ms.locfileid: "75502270"
---
# <a name="microsoft-sql-server-ef-core-database-provider"></a>Поставщик базы данных Microsoft SQL Server EF Core

Этот поставщик базы данных позволяет использовать Entity Framework Core с Microsoft SQL Server (включая базу данных SQL Azure). Работы над этим поставщиком ведутся в рамках [проекта Entity Framework Core](https://github.com/aspnet/EntityFrameworkCore).

## <a name="install"></a>Установка

Установите [пакет NuGet Microsoft.EntityFrameworkCore.SqlServer](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer/).

### <a name="net-core-clitabdotnet-core-cli"></a>[Интерфейс командной строки .NET Core](#tab/dotnet-core-cli)

```dotnetcli
dotnet add package Microsoft.EntityFrameworkCore.SqlServer
```

### <a name="visual-studiotabvs"></a>[Visual Studio](#tab/vs)

``` powershell
Install-Package Microsoft.EntityFrameworkCore.SqlServer
```

***

> [!NOTE]
> Начиная с версии 3.0.0,поставщик ссылается на Microsoft.Data.SqlClient (предыдущие версии зависят от System.Data.SqlClient). Если проект принимает прямую зависимость от SqlClient, убедитесь, что добавлена ссылка на пакет Microsoft.Data.SqlClient.

## <a name="supported-database-engines"></a>Поддерживаемые ядра СУБД

* Microsoft SQL Server (2012 и последующие версии)
