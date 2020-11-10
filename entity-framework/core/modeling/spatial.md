---
title: Пространственные данные — EF Core
description: Использование пространственных данных в модели Entity Framework Core
author: bricelam
ms.date: 10/02/2020
uid: core/modeling/spatial
ms.openlocfilehash: fa9cf30ddb4291d96486934544b568b67b126846
ms.sourcegitcommit: f3512e3a98e685a3ba409c1d0157ce85cc390cf4
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2020
ms.locfileid: "94430525"
---
# <a name="spatial-data"></a>Пространственные данные

> [!NOTE]
> Эта функция была добавлена в EF Core 2,2.

Пространственные данные представляют физическое расположение и форму объектов. Многие базы данных обеспечивают поддержку этого типа данных, поэтому их можно индексировать и запрашивать вместе с другими данными. Типичные сценарии включают запросы объектов в заданном расстоянии от расположения или выбор объекта, граница которого содержит заданное расположение. EF Core поддерживает сопоставление с пространственными типами данных с помощью пространственной библиотеки Неттопологисуите.

## <a name="installing"></a>Установка

Чтобы использовать пространственные данные с EF Core, необходимо установить соответствующий поддерживающий пакет NuGet. Пакет, который необходимо установить, зависит от поставщика, который вы используете.

Поставщик EF Core                        | Пространственный пакет NuGet
--------------------------------------- | ---------------------
Microsoft.EntityFrameworkCore.SqlServer | [Microsoft. EntityFrameworkCore. SqlServer. Неттопологисуите](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer.NetTopologySuite)
Microsoft.EntityFrameworkCore.Sqlite    | [Microsoft. EntityFrameworkCore. SQLite. Неттопологисуите](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Sqlite.NetTopologySuite)
Microsoft.EntityFrameworkCore.InMemory  | [неттопологисуите](https://www.nuget.org/packages/NetTopologySuite)
Npgsql.EntityFrameworkCore.PostgreSQL   | [Npgsql. EntityFrameworkCore. PostgreSQL. Неттопологисуите](https://www.nuget.org/packages/Npgsql.EntityFrameworkCore.PostgreSQL.NetTopologySuite)
Pomelo.EntityFrameworkCore.MySql        | [Pomelo. EntityFrameworkCore. MySql. Неттопологисуите](https://www.nuget.org/packages/Pomelo.EntityFrameworkCore.MySql.NetTopologySuite)
Devart.Data.MySql.EFCore                | [Devart. Data. MySql. Ефкоре. Неттопологисуите](https://www.nuget.org/packages/Devart.Data.MySql.EFCore.NetTopologySuite)
Devart.Data.PostgreSql.EFCore           | [Devart. Data. PostgreSql. Ефкоре. Неттопологисуите](https://www.nuget.org/packages/Devart.Data.PostgreSql.EFCore.NetTopologySuite)
Devart.Data.SQLite.EFCore               | [Devart. Data. SQLite. Ефкоре. Неттопологисуите](https://www.nuget.org/packages/Devart.Data.SQLite.EFCore.NetTopologySuite)
Teradata.EntityFrameworkCore            | [Teradata. EntityFrameworkCore. Неттопологисуите](https://www.nuget.org/packages/Teradata.EntityFrameworkCore.NetTopologySuite)

## <a name="nettopologysuite"></a>неттопологисуите

[Неттопологисуите](https://nettopologysuite.github.io/NetTopologySuite/) (NTS) является пространственной библиотекой для .NET. EF Core поддерживает сопоставление с пространственными типами данных в базе данных с помощью типов NTS в модели.

Чтобы включить сопоставление с пространственными типами через NTS, вызовите метод Усенеттопологисуите в построителе параметров DbContext поставщика. Например, с SQL Server можно назвать его следующим образом.

[!code-csharp[](../../../samples/core/Spatial/SqlServer/Models/WideWorldImportersContext.cs?name=snippet_UseNetTopologySuite)]

Существует несколько типов пространственных данных. Используемый тип зависит от типов фигур, которые вы хотите разрешить. Ниже приведена иерархия типов NTS, которые можно использовать для свойств в модели. Они находятся в пределах `NetTopologySuite.Geometries` пространства имен.

* Геометрия
  * Точка
  * LineString
  * Многоугольник
  * GeometryCollection
    * MultiPoint
    * MultiLineString
    * MultiPolygon

> [!WARNING]
> CircularString, CompoundCurve и Куреполигон не поддерживаются NTS.

Использование базового типа geometry позволяет задать любой тип фигуры в свойстве.

## <a name="longitude-and-latitude"></a>Долгота и Широта

Координаты в NTS находятся в терминах значений X и Y. Чтобы представить долготу и широту, используйте X для долготы и Y для широты. Обратите внимание, что это происходит **обратно** из `latitude, longitude` формата, в котором обычно отображаются эти значения.

## <a name="querying-data"></a>Запросы к данным

Следующие классы сущностей можно использовать для соотнесения с таблицами в [образце базы данных Wide World импортеров](https://go.microsoft.com/fwlink/?LinkID=800630).

[!code-csharp[](../../../samples/core/Spatial/SqlServer/Models/City.cs?name=snippet_City)]

[!code-csharp[](../../../samples/core/Spatial/SqlServer/Models/Country.cs?name=snippet_Country)]

В LINQ методы и свойства NTS, доступные в качестве функций базы данных, будут преобразованы в SQL. Например, методы Distance и Contains преобразуются в следующие запросы. Сведения о поддерживаемых методах см. в документации поставщика.

[!code-csharp[](../../../samples/core/Spatial/SqlServer/Program.cs?name=snippet_Distance)]

[!code-csharp[](../../../samples/core/Spatial/SqlServer/Program.cs?name=snippet_Contains)]

## <a name="reverse-engineering"></a>Реконструирование

Пространственные пакеты NuGet также включают модели [реконструирования](xref:core/managing-schemas/scaffolding) с пространственными свойствами, но необходимо установить пакет * *_перед_* _ запуском `Scaffold-DbContext` или `dotnet ef dbcontext scaffold` . В противном случае вы получите предупреждения о том, что не удается найти сопоставления типов для столбцов, и столбцы будут пропущены.

## <a name="srid-ignored-during-client-operations"></a>SRID игнорируется во время операций клиента

NTS игнорирует значения SRID во время операций. Предполагается, что это плоская система координат. Это означает, что если указать координаты в виде долготы и широты, то некоторые значения, вычисляемые клиентом, такие как расстояние, Длина и площадь, будут в градусах, а не в метрах. Чтобы получить более значимые значения, сначала необходимо проецировать координаты в другую систему координат, используя библиотеку, например [ProjNet4GeoAPI](https://github.com/NetTopologySuite/ProjNet4GeoAPI) , перед вычислением этих значений.

Если операция оценивается сервером с помощью EF Core с помощью SQL, то единица результата будет определяться базой данных.

Ниже приведен пример использования ProjNet4GeoAPI для вычисления расстояния между двумя городами.

[!code-csharp[](../../../samples/core/Spatial/Projections/GeometryExtensions.cs?name=snippet_GeometryExtensions)]

[!code-csharp[](../../../samples/core/Spatial/Projections/Program.cs?name=snippet_ProjectTo)]

> [!NOTE]
> 4326 относится к WGS 84, стандарту, используемому в GPS и других географических системах.

## <a name="additional-resources"></a>Дополнительные ресурсы

### <a name="database-specific-information"></a>Сведения, относящиеся к базе данных

Обязательно ознакомьтесь с документацией поставщика, чтобы получить дополнительные сведения о работе с пространственными данными.

_ [Пространственные данные в поставщике SQL Server](xref:core/providers/sql-server/spatial)
* [Пространственные данные в поставщике SQLite](xref:core/providers/sqlite/spatial)
* [Пространственные данные в поставщике Npgsql](https://www.npgsql.org/efcore/mapping/nts.html)

### <a name="other-resources"></a>Другие источники

* [Документы Неттопологисуите](https://nettopologysuite.github.io/NetTopologySuite/)
* [EF Core сеанс standup Community](https://www.youtube.com/watch?v=IHslY5rrxD0&list=PLdo4fOcmZ0oX-DBuRG4u58ZTAJgBAeQ-t&index=15), сосредоточенный на пространственных данных и неттопологисуите.
