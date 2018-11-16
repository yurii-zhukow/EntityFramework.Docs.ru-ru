---
title: Пространственных данных — EF Core
author: bricelam
ms.author: bricelam
ms.date: 11/01/2018
ms.assetid: 2BDE29FC-4161-41A0-841E-69F51CCD9341
uid: core/modeling/spatial
ms.openlocfilehash: 49c18758af2f2383ea082ead2f6df4c022152b36
ms.sourcegitcommit: b3c2b34d5f006ee3b41d6668f16fe7dcad1b4317
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/15/2018
ms.locfileid: "51688794"
---
# <a name="spatial-data"></a>Пространственные данные

> [!NOTE]
> Этот компонент впервые появился в EF Core 2.2.

Пространственные данные представляет физическое расположение и форму объектов. Многие базы данных поддерживают этот тип данных, его можно индексировать и запрашивать вместе с другими данными. Обычные сценарии включают запрос для объектов в пределах заданного расстояния из расположения, или выберите объект, границу которого содержит заданное расположение. EF Core поддерживает сопоставление типов пространственных данных с использованием [NetTopologySuite](https://github.com/NetTopologySuite/NetTopologySuite) пространственной библиотеки.

## <a name="installing"></a>Установка

Чтобы использовать Пространственные данные с EF Core, необходимо установить соответствующий пакет поддержки NuGet. Какой пакет, необходимо установить зависит от поставщика, которую вы используете.

Поставщик EF Core                        | Пространственные пакета NuGet
--------------------------------------- | ---------------------
Microsoft.EntityFrameworkCore.SqlServer | [Microsoft.EntityFrameworkCore.SqlServer.NetTopologySuite](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer.NetTopologySuite)
Microsoft.EntityFrameworkCore.Sqlite    | [Microsoft.EntityFrameworkCore.Sqlite.NetTopologySuite](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Sqlite.NetTopologySuite)
Microsoft.EntityFrameworkCore.InMemory  | [NetTopologySuite](https://www.nuget.org/packages/NetTopologySuite)
Npgsql.EntityFrameworkCore.PostgreSQL   | [Npgsql.EntityFrameworkCore.PostgreSQL.NetTopologySuite](https://www.nuget.org/packages/Npgsql.EntityFrameworkCore.PostgreSQL.NetTopologySuite)

## <a name="reverse-engineering"></a>Реконструирование

Пространственные NuGet также пакеты enable [реконструирование](../managing-schemas/scaffolding.md) моделей с пространственных свойств, но вам нужно установить пакет ***перед*** под управлением `Scaffold-DbContext` или `dotnet ef dbcontext scaffold`. Если этого не сделать, вы будете получать предупреждения о не находит сопоставления типов для столбцов, а столбцы будут пропущены.

## <a name="nettopologysuite-nts"></a>NetTopologySuite (NTS)

NetTopologySuite представляет собой пространственных библиотеку для .NET. EF Core обеспечивает сопоставление пространственных данных типов в базе данных с помощью NTS типов в модели.

Чтобы включить сопоставление для пространственных типов с помощью NTS, вызовите метод UseNetTopologySuite построитель параметры поставщика DbContext. К примеру используя SQL Server можно будет вызвать следующим образом.

``` csharp
optionsBuilder.UseSqlServer(
    @"Data Source=(localdb)\MSSQLLocalDB;Initial Catalog=WideWorldImporters",
    x => x.UseNetTopologySuite());
```

Существует несколько типов пространственных данных. Какой тип использовать зависит от типов фигур, которые вы хотите разрешить. Вот иерархии NTS типов, которые можно использовать для свойств в модели. Они находятся в пределах `NetTopologySuite.Geometries` пространства имен. Соответствующие интерфейсы в пакете GeoAPI (`GeoAPI.Geometries` пространства имен) также можно использовать.

* Geometry
  * Точка
  * LineString
  * Polygon
  * GeometryCollection
    * MultiPoint
    * MultiLineString
    * MultiPolygon

> [!WARNING]
> CircularString, CompoundCurve и CurePolygon не поддерживаются NTS.

С помощью базовый тип геометрического объекта любого типа фигуры, указанного в свойстве.

Следующие классы сущностей может использоваться для сопоставления с таблицами в [образца базы данных Wide World Importers](http://go.microsoft.com/fwlink/?LinkID=800630).

``` csharp
[Table("Cities", Schema = "Application"))]
class City
{
    public int CityID { get; set; }

    public string CityName { get; set; }

    public IPoint Location { get; set; }
}

[Table("Countries", Schema = "Application"))]
class Country
{
    public int CountryID { get; set; }

    public string CountryName { get; set; }

    // Database includes both Polygon and MultiPolygon values
    public IGeometry Border { get; set; }
}
```

### <a name="creating-values"></a>Создание значения

Конструкторы можно использовать для создания геометрических объектов; Тем не менее NTS рекомендует вместо этого использовать фабрику geometry. Это позволяет задать значение по умолчанию SRID (пространственную систему, используемых координаты) и предоставляет контроль над более сложных такие вещи, как точность модели (используется во время вычисления) и координат последовательности (определяет, какие ordinates--измерений и показателями — существуют доступны).

``` csharp
var geometryFactory = NtsGeometryServices.Instance.CreateGeometryFactory(srid: 4326);
var currentLocation = geometryFactory.CreatePoint(-122.121512, 47.6739882);
```

> [!NOTE]
> 4326 ссылается на WGS 84, стандартом GPS и других географических систем.

### <a name="longitude-and-latitude"></a>Широту и долготу

Координаты в NTS, указываются в виде значения X и Y. Для представления, широту и долготу, используйте X для широты и Y для широты. Обратите внимание, что это **обратной** из `latitude, longitude` формат, в котором обычно увидеть эти значения.

### <a name="srid-ignored-during-client-operations"></a>SRID пропущены во время операции клиента

NTS SRID значения игнорируются во время операций. Предполагается планарная система координат. Это означает, что если указать координаты с точки зрения широту и долготу, некоторые значения клиента вычисляется как расстояние, длина и области будут в градусах, не счетчики. Для более осмысленными значениями, необходимо сначала проект координат в другую систему координат, используя библиотеку как [ProjNet4GeoAPI](https://github.com/NetTopologySuite/ProjNet4GeoAPI) перед вычисление этих значений.

Если операция выполняется на сервере оцениваются в EF Core с помощью SQL, единицы результат будет определяться в базе данных.

Ниже приведен пример использования ProjNet4GeoAPI для вычисления расстояния между двумя городами.

``` csharp
static class GeometryExtensions
{
    static readonly IGeometryServices _geometryServices = NtsGeometryServices.Instance;
    static readonly ICoordinateSystemServices _coordinateSystemServices
        = new CoordinateSystemServices(
            new CoordinateSystemFactory(),
            new CoordinateTransformationFactory(),
            new Dictionary<int, string>
            {
                // Coordinate systems:

                // (3857 and 4326 included automatically)

                // This coordinate system covers the area of our data.
                // Different data requires a different coordinate system.
                [2855] =
                @"
                    PROJCS[""NAD83(HARN) / Washington North"",
                        GEOGCS[""NAD83(HARN)"",
                            DATUM[""NAD83_High_Accuracy_Regional_Network"",
                                SPHEROID[""GRS 1980"",6378137,298.257222101,
                                    AUTHORITY[""EPSG"",""7019""]],
                                AUTHORITY[""EPSG"",""6152""]],
                            PRIMEM[""Greenwich"",0,
                                AUTHORITY[""EPSG"",""8901""]],
                            UNIT[""degree"",0.01745329251994328,
                                AUTHORITY[""EPSG"",""9122""]],
                            AUTHORITY[""EPSG"",""4152""]],
                        PROJECTION[""Lambert_Conformal_Conic_2SP""],
                        PARAMETER[""standard_parallel_1"",48.73333333333333],
                        PARAMETER[""standard_parallel_2"",47.5],
                        PARAMETER[""latitude_of_origin"",47],
                        PARAMETER[""central_meridian"",-120.8333333333333],
                        PARAMETER[""false_easting"",500000],
                        PARAMETER[""false_northing"",0],
                        UNIT[""metre"",1,
                            AUTHORITY[""EPSG"",""9001""]],
                        AUTHORITY[""EPSG"",""2855""]]
                "
            });

    public static IGeometry ProjectTo(this IGeometry geometry, int srid)
    {
        var geometryFactory = _geometryServices.CreateGeometryFactory(srid);
        var transformation = _coordinateSystemServices.CreateTransformation(geometry.SRID, srid);

        return GeometryTransform.TransformGeometry(
            geometryFactory,
            geometry,
            transformation.MathTransform);
    }
}
```

``` csharp
var seattle = new Point(-122.333056, 47.609722) { SRID = 4326 };
var redmond = new Point(-122.123889, 47.669444) { SRID = 4326 };

var distance = seattle.ProjectTo(2855).Distance(redmond.ProjectTo(2855));
```

## <a name="querying-data"></a>Запросы к данным

NTS методы и свойства, доступные как функции базы данных будут переведены в LINQ to SQL. Например расстояние и содержит методы, преобразуются в следующих запросах. В таблице в конце этой статьи показано, какие члены поддерживаются различные поставщики EF Core.

``` csharp
var nearestCity = db.Cities
    .OrderBy(c => c.Location.Distance(currentLocation))
    .FirstOrDefault();

var currentCountry = db.Countries
    .FirstOrDefault(c => c.Border.Contains(currentLocation));
```

## <a name="sql-server"></a>SQL Server

Если вы используете SQL Server, существуют некоторые дополнительные действия, которые следует учитывать.

### <a name="geography-or-geometry"></a>Географическому или геометрическому объекту

По умолчанию Пространственные свойства сопоставляются `geography` столбцы в SQL Server. Чтобы использовать `geometry`, [настроить тип столбца](xref:core/modeling/relational/data-types) в модели.

### <a name="geography-polygon-rings"></a>Кольца многоугольника Geography

При использовании `geography` тип столбца, SQL Server налагает дополнительные требования на внешнее кольцо (или оболочки) и внутренних колец (или бреши в системе). Внешнее кольцо против часовой стрелки должен быть ориентирован и внутренняя область кольцами по часовой стрелке. NTS проверяет это перед отправкой значения в базе данных.

### <a name="fullglobe"></a>FullGlobe

SQL Server имеет нестандартные геометрический тип для представления полного земного шара при использовании `geography` тип столбца. Он также содержит способ представления многоугольников основании полного земного шара (без внешним кольцом). Ни один из этих поддерживаемых NTS.

> [!WARNING]
> NTS не поддерживает FullGlobe и многоугольников на его основе.

## <a name="sqlite"></a>SQLite

Вот некоторые дополнительные сведения для тех, с помощью SQLite.

### <a name="installing-spatialite"></a>Установка SpatiaLite

В Windows библиотеки собственного mod_spatialite распространяется в виде зависимость пакета NuGet. Другие платформы должны устанавливать его отдельно. Обычно это делается с помощью диспетчера пакетов программного обеспечения. Например можно использовать APT в Ubuntu и Homebrew на MacOS.

``` sh
# Ubuntu
apt-get install libsqlite3-mod-spatialite

# macOS
brew install libspatialite
```

### <a name="configuring-srid"></a>Настройка SRID

В SpatiaLite столбцы должны быть указан SRID каждого столбца. По умолчанию используется SRID `0`. Укажите другим значением SRID, с помощью метода ForSqliteHasSrid.

``` csharp
modelBuilder.Entity<City>().Property(c => c.Location)
    .ForSqliteHasSrid(4326);
```

### <a name="dimension"></a>Измерение

Как и SRID, измерение столбца (или ordinates) также указывается как часть столбца. По умолчанию ordinates являются X и Y. Включение дополнительных ordinates (Z и M) с помощью метода ForSqliteHasDimension.

``` csharp
modelBuilder.Entity<City>().Property(c => c.Location)
    .ForSqliteHasDimension(Ordinates.XYZ);
```

## <a name="translated-operations"></a>Переведенные операций

В этой таблице показаны члены, которые NTS преобразуются в SQL каждым поставщиком EF Core.

NetTopologySuite | SQL Server (geometry) | SQL Server (geography) | SQLite | Npgsql
--- |:---:|:---:|:---:|:---:
Geometry.Area | ✔ | ✔ | ✔ | ✔
Geometry.AsBinary() | ✔ | ✔ | ✔ | ✔
Geometry.AsText() | ✔ | ✔ | ✔ | ✔
Geometry.Boundary | ✔ | | ✔ | ✔
Geometry.Buffer(double) | ✔ | ✔ | ✔ | ✔
Geometry.Buffer (double, int) | | | ✔
Geometry.Centroid | ✔ | | ✔ | ✔
Geometry.Contains(Geometry) | ✔ | ✔ | ✔ | ✔
Geometry.ConvexHull() | ✔ | ✔ | ✔ | ✔
Geometry.CoveredBy(Geometry) | | | ✔ | ✔
Geometry.Covers(Geometry) | | | ✔ | ✔
Geometry.Crosses(Geometry) | ✔ | | ✔ | ✔
Geometry.Difference(Geometry) | ✔ | ✔ | ✔ | ✔
Geometry.Dimension | ✔ | ✔ | ✔ | ✔
Geometry.Disjoint(Geometry) | ✔ | ✔ | ✔ | ✔
Geometry.Distance(Geometry) | ✔ | ✔ | ✔ | ✔
Geometry.Envelope | ✔ | | ✔ | ✔
Geometry.EqualsExact(Geometry) | | | | ✔
Geometry.EqualsTopologically(Geometry) | ✔ | ✔ | ✔ | ✔
Geometry.GeometryType | ✔ | ✔ | ✔ | ✔
Geometry.GetGeometryN(int) | ✔ | | ✔ | ✔
Geometry.InteriorPoint | ✔ | | ✔
Geometry.Intersection(Geometry) | ✔ | ✔ | ✔ | ✔
Geometry.Intersects(Geometry) | ✔ | ✔ | ✔ | ✔
Geometry.IsEmpty | ✔ | ✔ | ✔ | ✔
Geometry.IsSimple | ✔ | | ✔ | ✔
Geometry.IsValid | ✔ | ✔ | ✔ | ✔
Geometry.IsWithinDistance (Geometry, double) | ✔ | | ✔
Geometry.Length | ✔ | ✔ | ✔ | ✔
Geometry.NumGeometries | ✔ | ✔ | ✔ | ✔
Geometry.NumPoints | ✔ | ✔ | ✔ | ✔
Geometry.OgcGeometryType | ✔ | ✔ | ✔
Geometry.Overlaps(Geometry) | ✔ | ✔ | ✔ | ✔
Geometry.PointOnSurface | ✔ | | ✔ | ✔
Geometry.Relate (Geometry, строка) | ✔ | | ✔ | ✔
Geometry.Reverse() | | | ✔ | ✔
Geometry.SRID | ✔ | ✔ | ✔ | ✔
Geometry.SymmetricDifference(Geometry) | ✔ | ✔ | ✔ | ✔
Geometry.ToBinary() | ✔ | ✔ | ✔ | ✔
Geometry.ToText() | ✔ | ✔ | ✔ | ✔
Geometry.Touches(Geometry) | ✔ | | ✔ | ✔
Geometry.Union() | | | ✔
Geometry.Union(Geometry) | ✔ | ✔ | ✔ | ✔
Geometry.Within(Geometry) | ✔ | ✔ | ✔ | ✔
GeometryCollection.Count | ✔ | ✔ | ✔ | ✔
GeometryCollection [int] | ✔ | ✔ | ✔ | ✔
LineString.Count | ✔ | ✔ | ✔ | ✔
LineString.EndPoint | ✔ | ✔ | ✔ | ✔
LineString.GetPointN(int) | ✔ | ✔ | ✔ | ✔
LineString.IsClosed | ✔ | ✔ | ✔ | ✔
LineString.IsRing | ✔ | | ✔ | ✔
LineString.StartPoint | ✔ | ✔ | ✔ | ✔
MultiLineString.IsClosed | ✔ | ✔ | ✔ | ✔
Point.M | ✔ | ✔ | ✔ | ✔
Point.X | ✔ | ✔ | ✔ | ✔
Point.Y | ✔ | ✔ | ✔ | ✔
Point.Z | ✔ | ✔ | ✔ | ✔
Polygon.ExteriorRing | ✔ | ✔ | ✔ | ✔
Polygon.GetInteriorRingN(int) | ✔ | ✔ | ✔ | ✔
Polygon.NumInteriorRings | ✔ | ✔ | ✔ | ✔

## <a name="additional-resources"></a>Дополнительные ресурсы

* [Пространственные данные в SQL Server](https://docs.microsoft.com/sql/relational-databases/spatial/spatial-data-sql-server)
* [Домашняя страница SpatiaLite](https://www.gaia-gis.it/fossil/libspatialite)
* [Документация по PostGIS](http://postgis.net/documentation/)
