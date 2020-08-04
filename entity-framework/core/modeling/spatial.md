---
title: Пространственные данные — EF Core
author: bricelam
ms.author: bricelam
ms.date: 11/01/2018
ms.assetid: 2BDE29FC-4161-41A0-841E-69F51CCD9341
uid: core/modeling/spatial
ms.openlocfilehash: 85124b7e252797ccd952d0d332e7309eff97ba56
ms.sourcegitcommit: 949faaba02e07e44359e77d7935f540af5c32093
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/03/2020
ms.locfileid: "87526671"
---
# <a name="spatial-data"></a>Пространственные данные

> [!NOTE]
> Эта функция была добавлена в EF Core 2,2.

Пространственные данные представляют физическое расположение и форму объектов. Многие базы данных обеспечивают поддержку этого типа данных, поэтому их можно индексировать и запрашивать вместе с другими данными. Типичные сценарии включают запросы объектов в заданном расстоянии от расположения или выбор объекта, граница которого содержит заданное расположение. EF Core поддерживает сопоставление с пространственными типами данных с помощью пространственной библиотеки [неттопологисуите](https://github.com/NetTopologySuite/NetTopologySuite) .

## <a name="installing"></a>Установка

Чтобы использовать пространственные данные с EF Core, необходимо установить соответствующий поддерживающий пакет NuGet. Пакет, который необходимо установить, зависит от поставщика, который вы используете.

Поставщик EF Core                        | Пространственный пакет NuGet
--------------------------------------- | ---------------------
Microsoft.EntityFrameworkCore.SqlServer | [Microsoft. EntityFrameworkCore. SqlServer. Неттопологисуите](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer.NetTopologySuite)
Microsoft.EntityFrameworkCore.Sqlite    | [Microsoft. EntityFrameworkCore. SQLite. Неттопологисуите](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Sqlite.NetTopologySuite)
Microsoft.EntityFrameworkCore.InMemory  | [неттопологисуите](https://www.nuget.org/packages/NetTopologySuite)
Npgsql.EntityFrameworkCore.PostgreSQL   | [Npgsql. EntityFrameworkCore. PostgreSQL. Неттопологисуите](https://www.nuget.org/packages/Npgsql.EntityFrameworkCore.PostgreSQL.NetTopologySuite)

## <a name="reverse-engineering"></a>Реконструирование

Пространственные пакеты NuGet также включают модели [реконструирования](../managing-schemas/scaffolding.md) с пространственными свойствами, но необходимо установить пакет ***перед*** запуском `Scaffold-DbContext` или `dotnet ef dbcontext scaffold` . В противном случае вы получите предупреждения о том, что не удается найти сопоставления типов для столбцов, и столбцы будут пропущены.

## <a name="nettopologysuite-nts"></a>Неттопологисуите (NTS)

Неттопологисуите является пространственной библиотекой для .NET. EF Core поддерживает сопоставление с пространственными типами данных в базе данных с помощью типов NTS в модели.

Чтобы включить сопоставление с пространственными типами через NTS, вызовите метод Усенеттопологисуите в построителе параметров DbContext поставщика. Например, с SQL Server можно назвать его следующим образом.

``` csharp
optionsBuilder.UseSqlServer(
    @"Data Source=(localdb)\MSSQLLocalDB;Initial Catalog=WideWorldImporters",
    x => x.UseNetTopologySuite());
```

Существует несколько типов пространственных данных. Используемый тип зависит от типов фигур, которые вы хотите разрешить. Ниже приведена иерархия типов NTS, которые можно использовать для свойств в модели. Они находятся в пределах `NetTopologySuite.Geometries` пространства имен.

* Геометрия
  * Точка
  * LineString
  * Polygon
  * GeometryCollection
    * MultiPoint
    * MultiLineString
    * MultiPolygon

> [!WARNING]
> CircularString, CompoundCurve и Куреполигон не поддерживаются NTS.

Использование базового типа geometry позволяет задать любой тип фигуры в свойстве.

Следующие классы сущностей можно использовать для соотнесения с таблицами в [образце базы данных Wide World импортеров](https://go.microsoft.com/fwlink/?LinkID=800630).

``` csharp
[Table("Cities", Schema = "Application"))]
class City
{
    public int CityID { get; set; }

    public string CityName { get; set; }

    public Point Location { get; set; }
}

[Table("Countries", Schema = "Application"))]
class Country
{
    public int CountryID { get; set; }

    public string CountryName { get; set; }

    // Database includes both Polygon and MultiPolygon values
    public Geometry Border { get; set; }
}
```

### <a name="creating-values"></a>Создание значений

Для создания объектов Geometry можно использовать конструкторы. Однако NTS рекомендует использовать вместо нее геометрическую фабрику. Это позволяет указать SRID по умолчанию (систему пространственной ссылки, используемую координатами) и позволяет управлять более сложными возможностями, такими как модель точности (используемая во время вычислений), и последовательность координат (определяет, какие координаты и меры —).

``` csharp
var geometryFactory = NtsGeometryServices.Instance.CreateGeometryFactory(srid: 4326);
var currentLocation = geometryFactory.CreatePoint(new Coordinate(-122.121512, 47.6739882));
```

> [!NOTE]
> 4326 относится к WGS 84, стандарту, используемому в GPS и других географических системах.

### <a name="longitude-and-latitude"></a>Долгота и Широта

Координаты в NTS находятся в терминах значений X и Y. Чтобы представить долготу и широту, используйте X для долготы и Y для широты. Обратите внимание, что это происходит **обратно** из `latitude, longitude` формата, в котором обычно отображаются эти значения.

### <a name="srid-ignored-during-client-operations"></a>SRID игнорируется во время операций клиента

NTS игнорирует значения SRID во время операций. Предполагается, что это плоская система координат. Это означает, что если указать координаты в виде долготы и широты, то некоторые значения, вычисляемые клиентом, такие как расстояние, Длина и площадь, будут в градусах, а не в метрах. Чтобы получить более значимые значения, сначала необходимо проецировать координаты в другую систему координат, используя библиотеку, например [ProjNet4GeoAPI](https://github.com/NetTopologySuite/ProjNet4GeoAPI) , перед вычислением этих значений.

Если операция оценивается сервером с помощью EF Core с помощью SQL, то единица результата будет определяться базой данных.

Ниже приведен пример использования ProjNet4GeoAPI для вычисления расстояния между двумя городами.

``` csharp
static class GeometryExtensions
{
    static readonly CoordinateSystemServices _coordinateSystemServices
        = new CoordinateSystemServices(
            new CoordinateSystemFactory(),
            new CoordinateTransformationFactory(),
            new Dictionary<int, string>
            {
                // Coordinate systems:

                [4326] = GeographicCoordinateSystem.WGS84.WKT,

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

    public static Geometry ProjectTo(this Geometry geometry, int srid)
    {
        var transformation = _coordinateSystemServices.CreateTransformation(geometry.SRID, srid);

        var result = geometry.Copy();
        result.Apply(new MathTransformFilter(transformation.MathTransform));

        return result;
    }

    class MathTransformFilter : ICoordinateSequenceFilter
    {
        readonly MathTransform _transform;

        public MathTransformFilter(MathTransform transform)
            => _transform = transform;

        public bool Done => false;
        public bool GeometryChanged => true;

        public void Filter(CoordinateSequence seq, int i)
        {
            var result = _transform.Transform(
                new[]
                {
                    seq.GetOrdinate(i, Ordinate.X),
                    seq.GetOrdinate(i, Ordinate.Y)
                });
            seq.SetOrdinate(i, Ordinate.X, result[0]);
            seq.SetOrdinate(i, Ordinate.Y, result[1]);
        }
    }
}
```

``` csharp
var seattle = new Point(-122.333056, 47.609722) { SRID = 4326 };
var redmond = new Point(-122.123889, 47.669444) { SRID = 4326 };

var distance = seattle.ProjectTo(2855).Distance(redmond.ProjectTo(2855));
```

## <a name="querying-data"></a>Запросы к данным

В LINQ методы и свойства NTS, доступные в качестве функций базы данных, будут преобразованы в SQL. Например, методы Distance и Contains преобразуются в следующие запросы. В таблице в конце этой статьи показано, какие элементы поддерживаются различными поставщиками EF Core.

``` csharp
var nearestCity = db.Cities
    .OrderBy(c => c.Location.Distance(currentLocation))
    .FirstOrDefault();

var currentCountry = db.Countries
    .FirstOrDefault(c => c.Border.Contains(currentLocation));
```

## <a name="sql-server"></a>SQL Server

Если вы используете SQL Server, необходимо учитывать некоторые дополнительные моменты.

### <a name="geography-or-geometry"></a>География или геометрические объекты

По умолчанию пространственные свойства сопоставляются со `geography` столбцами в SQL Server. Чтобы использовать `geometry` , [Настройте тип столбца](xref:core/modeling/entity-properties#column-data-types) в модели.

### <a name="geography-polygon-rings"></a>Кольца географических многоугольников

При использовании `geography` типа столбца SQL Server накладывает дополнительные требования на внешнее кольцо (или оболочку) и внутренние кольца (или отверстия). Внешнее кольцо должно быть ориентировано против часовой стрелки и внутренних колец по часовой стрелке. NTS проверяет это перед отправкой значений в базу данных.

### <a name="fullglobe"></a>FullGlobe

SQL Server имеет нестандартный тип геометрии для представления полного земного шара при использовании `geography` типа столбца. У него также есть способ представления многоугольников на основе полного земного шара (без внешнего кольца). Ни один из этих элементов не поддерживается NTS.

> [!WARNING]
> FullGlobe и многоугольники, основанные на нем, не поддерживаются NTS.

## <a name="sqlite"></a>SQLite

Ниже приведена дополнительная информация для тех, кто использует SQLite.

### <a name="installing-spatialite"></a>Установка Спатиалите

В Windows собственная библиотека mod_spatialite распространяется как зависимость пакета NuGet. Другие платформы должны установить его отдельно. Обычно это делается с помощью диспетчера пакетов программного обеспечения. Например, можно использовать APT для Ubuntu и Homebrew на MacOS.

``` sh
# Ubuntu
apt-get install libsqlite3-mod-spatialite

# macOS
brew install libspatialite
```

К сожалению, новые версии PROJ (зависимость от Спатиалите) несовместимы с [набором склитепклрав](/dotnet/standard/data/sqlite/custom-versions#bundles)по умолчанию EF. Это можно обойти, создав пользовательский [поставщик склитепклрав](/dotnet/standard/data/sqlite/custom-versions#sqlitepclraw-providers) , использующий библиотеку System SQLite, или можно установить пользовательскую сборку, отключив поддержку proj для спатиалите.

``` sh
curl https://www.gaia-gis.it/gaia-sins/libspatialite-4.3.0a.tar.gz | tar -xz
cd libspatialite-4.3.0a

if [[ `uname -s` == Darwin* ]]; then
    # Mac OS requires some minor patching
    sed -i "" "s/shrext_cmds='\`test \\.\$module = .yes && echo .so \\|\\| echo \\.dylib\`'/shrext_cmds='.dylib'/g" configure
fi

./configure --disable-proj
make
make install
```

### <a name="configuring-srid"></a>Настройка SRID

В Спатиалите столбцам необходимо указать SRID для каждого столбца. По умолчанию SRID имеет значение `0` . Укажите другой SRID с помощью метода Форсклитехассрид.

``` csharp
modelBuilder.Entity<City>().Property(c => c.Location)
    .ForSqliteHasSrid(4326);
```

### <a name="dimension"></a>Измерение

Как и в случае с SRID, измерение столбца (или координата) также указывается как часть столбца. По умолчанию используются координаты X и Y. Включите дополнительные координаты (Z и M) с помощью метода Форсклитехасдименсион.

``` csharp
modelBuilder.Entity<City>().Property(c => c.Location)
    .ForSqliteHasDimension(Ordinates.XYZ);
```

## <a name="translated-operations"></a>Переведенные операции

В этой таблице показано, какие элементы NTS преобразуются в SQL каждым поставщиком EF Core.

неттопологисуите | SQL Server (геометрия) | SQL Server (география) | SQLite | Npgsql
--- |:---:|:---:|:---:|:---:
Геометрия. область | ✔ | ✔ | ✔ | ✔
Geometry. Асбинари () | ✔ | ✔ | ✔ | ✔
Geometry. Астекст () | ✔ | ✔ | ✔ | ✔
Geometry. Граничный | ✔ | | ✔ | ✔
Геометрия. buffer (двойное) | ✔ | ✔ | ✔ | ✔
Геометрия. buffer (Double, int) | | | ✔ | ✔
Geometry. центроид | ✔ | | ✔ | ✔
Геометрия. Contains (геометрия) | ✔ | ✔ | ✔ | ✔
Geometry. Конвексхулл () | ✔ | ✔ | ✔ | ✔
Geometry. Ковередби (геометрия) | | | ✔ | ✔
Geometry. крышки (Geometry) | | | ✔ | ✔
Геометрия. перекрестий (Geometry) | ✔ | | ✔ | ✔
Геометрия. Difference (геометрия) | ✔ | ✔ | ✔ | ✔
Geometry. Dimension | ✔ | ✔ | ✔ | ✔
Geometry. unjoin (геометрия) | ✔ | ✔ | ✔ | ✔
Геометрия. Distance (геометрия) | ✔ | ✔ | ✔ | ✔
Геометрия. конверт | ✔ | | ✔ | ✔
Geometry. Екуалсексакт (геометрия) | | | | ✔
Geometry. Екуалстопологикалли (геометрия) | ✔ | ✔ | ✔ | ✔
Geometry. Жеометритипе | ✔ | ✔ | ✔ | ✔
Geometry. Жетжеометрин (int) | ✔ | | ✔ | ✔
Geometry. Интериорпоинт | ✔ | | ✔ | ✔
Geometry. пересечение (Geometry) | ✔ | ✔ | ✔ | ✔
Геометрию. intersects (геометрия) | ✔ | ✔ | ✔ | ✔
Geometry. isEmpty | ✔ | ✔ | ✔ | ✔
Geometry. Простое_имя | ✔ | | ✔ | ✔
Geometry. IsValid | ✔ | ✔ | ✔ | ✔
Geometry. Исвисиндистанце (геометрия, Double) | ✔ | | ✔ | ✔
Геометрия геометрии. length | ✔ | ✔ | ✔ | ✔
Geometry. Нумжеометриес | ✔ | ✔ | ✔ | ✔
Geometry. Нумпоинтс | ✔ | ✔ | ✔ | ✔
Geometry. Огкжеометритипе | ✔ | ✔ | ✔ | ✔
Геометрия Geometry. пересекающих объектов (Geometry) | ✔ | ✔ | ✔ | ✔
Geometry. Поинтонсурфаце | ✔ | | ✔ | ✔
Geometry. Late (геометрия, строка) | ✔ | | ✔ | ✔
Геометрия. Reverse () | | | ✔ | ✔
Geometry. SRID | ✔ | ✔ | ✔ | ✔
Geometry. Симметрикдифференце (геометрия) | ✔ | ✔ | ✔ | ✔
Geometry. Тобинари () | ✔ | ✔ | ✔ | ✔
Geometry. Тотекст () | ✔ | ✔ | ✔ | ✔
Geometry. касания (геометрия) | ✔ | | ✔ | ✔
Geometry. Union () | | | ✔ | ✔
Geometry. Union (геометрия) | ✔ | ✔ | ✔ | ✔
Геометрия. внутри (Geometry) | ✔ | ✔ | ✔ | ✔
GeometryCollection. Count | ✔ | ✔ | ✔ | ✔
GeometryCollection [int] | ✔ | ✔ | ✔ | ✔
LineString. Count | ✔ | ✔ | ✔ | ✔
LineString. EndPoint | ✔ | ✔ | ✔ | ✔
LineString. Жетпоинтн (int) | ✔ | ✔ | ✔ | ✔
LineString. крыл | ✔ | ✔ | ✔ | ✔
LineString. Звонок | ✔ | | ✔ | ✔
LineString. StartPoint | ✔ | ✔ | ✔ | ✔
MultiLineString. крыл | ✔ | ✔ | ✔ | ✔
Point. M | ✔ | ✔ | ✔ | ✔
Point. X | ✔ | ✔ | ✔ | ✔
Point. Y | ✔ | ✔ | ✔ | ✔
Point. Z | ✔ | ✔ | ✔ | ✔
Polygon. Екстериорринг | ✔ | ✔ | ✔ | ✔
Polygon. Жетинтериоррингн (int) | ✔ | ✔ | ✔ | ✔
Polygon. Нуминтериоррингс | ✔ | ✔ | ✔ | ✔

## <a name="additional-resources"></a>Дополнительные ресурсы

* [Пространственные данные SQL Server](/sql/relational-databases/spatial/spatial-data-sql-server)
* [Домашняя страница Спатиалите](https://www.gaia-gis.it/fossil/libspatialite)
* [Документация по Npgsqlной пространственной документации](https://www.npgsql.org/efcore/mapping/nts.html)
* [Документация по PostGIS](https://postgis.net/documentation/)
