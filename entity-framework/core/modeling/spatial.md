---
title: Пространственные данные — EF Core
author: bricelam
ms.author: bricelam
ms.date: 11/01/2018
ms.assetid: 2BDE29FC-4161-41A0-841E-69F51CCD9341
uid: core/modeling/spatial
ms.openlocfilehash: cced53edadb890e4e86753ec2628218ffc4d1d5b
ms.sourcegitcommit: 708b18520321c587b2046ad2ea9fa7c48aeebfe5
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/09/2019
ms.locfileid: "72181383"
---
# <a name="spatial-data"></a><span data-ttu-id="6da40-102">Пространственные данные</span><span class="sxs-lookup"><span data-stu-id="6da40-102">Spatial Data</span></span>

> [!NOTE]
> <span data-ttu-id="6da40-103">Эта функция была добавлена в EF Core 2,2.</span><span class="sxs-lookup"><span data-stu-id="6da40-103">This feature was added in EF Core 2.2.</span></span>

<span data-ttu-id="6da40-104">Пространственные данные представляют физическое расположение и форму объектов.</span><span class="sxs-lookup"><span data-stu-id="6da40-104">Spatial data represents the physical location and the shape of objects.</span></span> <span data-ttu-id="6da40-105">Многие базы данных обеспечивают поддержку этого типа данных, поэтому их можно индексировать и запрашивать вместе с другими данными.</span><span class="sxs-lookup"><span data-stu-id="6da40-105">Many databases provide support for this type of data so it can be indexed and queried alongside other data.</span></span> <span data-ttu-id="6da40-106">Типичные сценарии включают запросы объектов в заданном расстоянии от расположения или выбор объекта, граница которого содержит заданное расположение.</span><span class="sxs-lookup"><span data-stu-id="6da40-106">Common scenarios include querying for objects within a given distance from a location, or selecting the object whose border contains a given location.</span></span> <span data-ttu-id="6da40-107">EF Core поддерживает сопоставление с пространственными типами данных с помощью пространственной библиотеки [неттопологисуите](https://github.com/NetTopologySuite/NetTopologySuite) .</span><span class="sxs-lookup"><span data-stu-id="6da40-107">EF Core supports mapping to spatial data types using the [NetTopologySuite](https://github.com/NetTopologySuite/NetTopologySuite) spatial library.</span></span>

## <a name="installing"></a><span data-ttu-id="6da40-108">Установка</span><span class="sxs-lookup"><span data-stu-id="6da40-108">Installing</span></span>

<span data-ttu-id="6da40-109">Чтобы использовать пространственные данные с EF Core, необходимо установить соответствующий поддерживающий пакет NuGet.</span><span class="sxs-lookup"><span data-stu-id="6da40-109">In order to use spatial data with EF Core, you need to install the appropriate supporting NuGet package.</span></span> <span data-ttu-id="6da40-110">Пакет, который необходимо установить, зависит от поставщика, который вы используете.</span><span class="sxs-lookup"><span data-stu-id="6da40-110">Which package you need to install depends on the provider you're using.</span></span>

<span data-ttu-id="6da40-111">Поставщик EF Core</span><span class="sxs-lookup"><span data-stu-id="6da40-111">EF Core Provider</span></span>                        | <span data-ttu-id="6da40-112">Пространственный пакет NuGet</span><span class="sxs-lookup"><span data-stu-id="6da40-112">Spatial NuGet Package</span></span>
--------------------------------------- | ---------------------
<span data-ttu-id="6da40-113">Microsoft.EntityFrameworkCore.SqlServer</span><span class="sxs-lookup"><span data-stu-id="6da40-113">Microsoft.EntityFrameworkCore.SqlServer</span></span> | [<span data-ttu-id="6da40-114">Microsoft. EntityFrameworkCore. SqlServer. Неттопологисуите</span><span class="sxs-lookup"><span data-stu-id="6da40-114">Microsoft.EntityFrameworkCore.SqlServer.NetTopologySuite</span></span>](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer.NetTopologySuite)
<span data-ttu-id="6da40-115">Microsoft.EntityFrameworkCore.Sqlite</span><span class="sxs-lookup"><span data-stu-id="6da40-115">Microsoft.EntityFrameworkCore.Sqlite</span></span>    | [<span data-ttu-id="6da40-116">Microsoft. EntityFrameworkCore. SQLite. Неттопологисуите</span><span class="sxs-lookup"><span data-stu-id="6da40-116">Microsoft.EntityFrameworkCore.Sqlite.NetTopologySuite</span></span>](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Sqlite.NetTopologySuite)
<span data-ttu-id="6da40-117">Microsoft.EntityFrameworkCore.InMemory</span><span class="sxs-lookup"><span data-stu-id="6da40-117">Microsoft.EntityFrameworkCore.InMemory</span></span>  | [<span data-ttu-id="6da40-118">неттопологисуите</span><span class="sxs-lookup"><span data-stu-id="6da40-118">NetTopologySuite</span></span>](https://www.nuget.org/packages/NetTopologySuite)
<span data-ttu-id="6da40-119">Npgsql.EntityFrameworkCore.PostgreSQL</span><span class="sxs-lookup"><span data-stu-id="6da40-119">Npgsql.EntityFrameworkCore.PostgreSQL</span></span>   | [<span data-ttu-id="6da40-120">Npgsql. EntityFrameworkCore. PostgreSQL. Неттопологисуите</span><span class="sxs-lookup"><span data-stu-id="6da40-120">Npgsql.EntityFrameworkCore.PostgreSQL.NetTopologySuite</span></span>](https://www.nuget.org/packages/Npgsql.EntityFrameworkCore.PostgreSQL.NetTopologySuite)

## <a name="reverse-engineering"></a><span data-ttu-id="6da40-121">Реконструирование</span><span class="sxs-lookup"><span data-stu-id="6da40-121">Reverse engineering</span></span>

<span data-ttu-id="6da40-122">Пространственные пакеты NuGet также включают модели [реконструирования](../managing-schemas/scaffolding.md) с пространственными свойствами, но ***перед*** выполнением `Scaffold-DbContext` или `dotnet ef dbcontext scaffold` необходимо установить пакет.</span><span class="sxs-lookup"><span data-stu-id="6da40-122">The spatial NuGet packages also enable [reverse engineering](../managing-schemas/scaffolding.md) models with spatial properties, but you need to install the package ***before*** running `Scaffold-DbContext` or `dotnet ef dbcontext scaffold`.</span></span> <span data-ttu-id="6da40-123">В противном случае вы получите предупреждения о том, что не удается найти сопоставления типов для столбцов, и столбцы будут пропущены.</span><span class="sxs-lookup"><span data-stu-id="6da40-123">If you don't, you'll receive warnings about not finding type mappings for the columns and the columns will be skipped.</span></span>

## <a name="nettopologysuite-nts"></a><span data-ttu-id="6da40-124">Неттопологисуите (NTS)</span><span class="sxs-lookup"><span data-stu-id="6da40-124">NetTopologySuite (NTS)</span></span>

<span data-ttu-id="6da40-125">Неттопологисуите является пространственной библиотекой для .NET.</span><span class="sxs-lookup"><span data-stu-id="6da40-125">NetTopologySuite is a spatial library for .NET.</span></span> <span data-ttu-id="6da40-126">EF Core поддерживает сопоставление с пространственными типами данных в базе данных с помощью типов NTS в модели.</span><span class="sxs-lookup"><span data-stu-id="6da40-126">EF Core enables mapping to spatial data types in the database by using NTS types in your model.</span></span>

<span data-ttu-id="6da40-127">Чтобы включить сопоставление с пространственными типами через NTS, вызовите метод Усенеттопологисуите в построителе параметров DbContext поставщика.</span><span class="sxs-lookup"><span data-stu-id="6da40-127">To enable mapping to spatial types via NTS, call the UseNetTopologySuite method on the provider's DbContext options builder.</span></span> <span data-ttu-id="6da40-128">Например, с SQL Server можно назвать его следующим образом.</span><span class="sxs-lookup"><span data-stu-id="6da40-128">For example, with SQL Server you'd call it like this.</span></span>

``` csharp
optionsBuilder.UseSqlServer(
    @"Data Source=(localdb)\MSSQLLocalDB;Initial Catalog=WideWorldImporters",
    x => x.UseNetTopologySuite());
```

<span data-ttu-id="6da40-129">Существует несколько типов пространственных данных.</span><span class="sxs-lookup"><span data-stu-id="6da40-129">There are several spatial data types.</span></span> <span data-ttu-id="6da40-130">Используемый тип зависит от типов фигур, которые вы хотите разрешить.</span><span class="sxs-lookup"><span data-stu-id="6da40-130">Which type you use depends on the types of shapes you want to allow.</span></span> <span data-ttu-id="6da40-131">Ниже приведена иерархия типов NTS, которые можно использовать для свойств в модели.</span><span class="sxs-lookup"><span data-stu-id="6da40-131">Here is the hierarchy of NTS types that you can use for properties in your model.</span></span> <span data-ttu-id="6da40-132">Они находятся в пространстве имен `NetTopologySuite.Geometries`.</span><span class="sxs-lookup"><span data-stu-id="6da40-132">They're located within the `NetTopologySuite.Geometries` namespace.</span></span>

* <span data-ttu-id="6da40-133">Объект</span><span class="sxs-lookup"><span data-stu-id="6da40-133">Geometry</span></span>
  * <span data-ttu-id="6da40-134">Точка</span><span class="sxs-lookup"><span data-stu-id="6da40-134">Point</span></span>
  * <span data-ttu-id="6da40-135">LineString</span><span class="sxs-lookup"><span data-stu-id="6da40-135">LineString</span></span>
  * <span data-ttu-id="6da40-136">Polygon</span><span class="sxs-lookup"><span data-stu-id="6da40-136">Polygon</span></span>
  * <span data-ttu-id="6da40-137">GeometryCollection</span><span class="sxs-lookup"><span data-stu-id="6da40-137">GeometryCollection</span></span>
    * <span data-ttu-id="6da40-138">MultiPoint</span><span class="sxs-lookup"><span data-stu-id="6da40-138">MultiPoint</span></span>
    * <span data-ttu-id="6da40-139">MultiLineString</span><span class="sxs-lookup"><span data-stu-id="6da40-139">MultiLineString</span></span>
    * <span data-ttu-id="6da40-140">MultiPolygon</span><span class="sxs-lookup"><span data-stu-id="6da40-140">MultiPolygon</span></span>

> [!WARNING]
> <span data-ttu-id="6da40-141">CircularString, CompoundCurve и Куреполигон не поддерживаются NTS.</span><span class="sxs-lookup"><span data-stu-id="6da40-141">CircularString, CompoundCurve, and CurePolygon aren't supported by NTS.</span></span>

<span data-ttu-id="6da40-142">Использование базового типа geometry позволяет задать любой тип фигуры в свойстве.</span><span class="sxs-lookup"><span data-stu-id="6da40-142">Using the base Geometry type allows any type of shape to be specified by the property.</span></span>

<span data-ttu-id="6da40-143">Следующие классы сущностей можно использовать для соотнесения с таблицами в [образце базы данных Wide World импортеров](https://go.microsoft.com/fwlink/?LinkID=800630).</span><span class="sxs-lookup"><span data-stu-id="6da40-143">The following entity classes could be used to map to tables in the [Wide World Importers sample database](https://go.microsoft.com/fwlink/?LinkID=800630).</span></span>

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

### <a name="creating-values"></a><span data-ttu-id="6da40-144">Создание значений</span><span class="sxs-lookup"><span data-stu-id="6da40-144">Creating values</span></span>

<span data-ttu-id="6da40-145">Для создания объектов Geometry можно использовать конструкторы. Однако NTS рекомендует использовать вместо нее геометрическую фабрику.</span><span class="sxs-lookup"><span data-stu-id="6da40-145">You can use constructors to create geometry objects; however, NTS recommends using a geometry factory instead.</span></span> <span data-ttu-id="6da40-146">Это позволяет указать SRID по умолчанию (систему пространственной ссылки, используемую координатами) и дает возможность управлять более сложными возможностями, такими как модель точности (используемая во время вычислений), и последовательность координат (определяет, какие координаты оси). и меры — доступны).</span><span class="sxs-lookup"><span data-stu-id="6da40-146">This lets you specify a default SRID (the spatial reference system used by the coordinates) and gives you control over more advanced things like the precision model (used during calculations) and the coordinate sequence (determines which ordinates--dimensions and measures--are available).</span></span>

``` csharp
var geometryFactory = NtsGeometryServices.Instance.CreateGeometryFactory(srid: 4326);
var currentLocation = geometryFactory.CreatePoint(-122.121512, 47.6739882);
```

> [!NOTE]
> <span data-ttu-id="6da40-147">4326 относится к WGS 84, стандарту, используемому в GPS и других географических системах.</span><span class="sxs-lookup"><span data-stu-id="6da40-147">4326 refers to WGS 84, a standard used in GPS and other geographic systems.</span></span>

### <a name="longitude-and-latitude"></a><span data-ttu-id="6da40-148">Долгота и Широта</span><span class="sxs-lookup"><span data-stu-id="6da40-148">Longitude and Latitude</span></span>

<span data-ttu-id="6da40-149">Координаты в NTS находятся в терминах значений X и Y.</span><span class="sxs-lookup"><span data-stu-id="6da40-149">Coordinates in NTS are in terms of X and Y values.</span></span> <span data-ttu-id="6da40-150">Чтобы представить долготу и широту, используйте X для долготы и Y для широты.</span><span class="sxs-lookup"><span data-stu-id="6da40-150">To represent longitude and latitude, use X for longitude and Y for latitude.</span></span> <span data-ttu-id="6da40-151">Обратите внимание, что это происходит **обратно** из формата `latitude, longitude`, в котором обычно отображаются эти значения.</span><span class="sxs-lookup"><span data-stu-id="6da40-151">Note that this is **backwards** from the `latitude, longitude` format in which you typically see these values.</span></span>

### <a name="srid-ignored-during-client-operations"></a><span data-ttu-id="6da40-152">SRID игнорируется во время операций клиента</span><span class="sxs-lookup"><span data-stu-id="6da40-152">SRID Ignored during client operations</span></span>

<span data-ttu-id="6da40-153">NTS игнорирует значения SRID во время операций.</span><span class="sxs-lookup"><span data-stu-id="6da40-153">NTS ignores SRID values during operations.</span></span> <span data-ttu-id="6da40-154">Предполагается, что это плоская система координат.</span><span class="sxs-lookup"><span data-stu-id="6da40-154">It assumes a planar coordinate system.</span></span> <span data-ttu-id="6da40-155">Это означает, что если указать координаты в виде долготы и широты, то некоторые значения, вычисляемые клиентом, такие как расстояние, Длина и площадь, будут в градусах, а не в метрах.</span><span class="sxs-lookup"><span data-stu-id="6da40-155">This means that if you specify coordinates in terms of longitude and latitude, some client-evaluated values like distance, length, and area will be in degrees, not meters.</span></span> <span data-ttu-id="6da40-156">Чтобы получить более значимые значения, сначала необходимо проецировать координаты в другую систему координат, используя библиотеку, например [ProjNet4GeoAPI](https://github.com/NetTopologySuite/ProjNet4GeoAPI) , перед вычислением этих значений.</span><span class="sxs-lookup"><span data-stu-id="6da40-156">For more meaningful values, you first need to project the coordinates to another coordinate system using a library like [ProjNet4GeoAPI](https://github.com/NetTopologySuite/ProjNet4GeoAPI) before calculating these values.</span></span>

<span data-ttu-id="6da40-157">Если операция оценивается сервером с помощью EF Core с помощью SQL, то единица результата будет определяться базой данных.</span><span class="sxs-lookup"><span data-stu-id="6da40-157">If an operation is server-evaluated by EF Core via SQL, the result's unit will be determined by the database.</span></span>

<span data-ttu-id="6da40-158">Ниже приведен пример использования ProjNet4GeoAPI для вычисления расстояния между двумя городами.</span><span class="sxs-lookup"><span data-stu-id="6da40-158">Here is an example of using ProjNet4GeoAPI to calculate the distance between two cities.</span></span>

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

## <a name="querying-data"></a><span data-ttu-id="6da40-159">Запросы к данным</span><span class="sxs-lookup"><span data-stu-id="6da40-159">Querying Data</span></span>

<span data-ttu-id="6da40-160">В LINQ методы и свойства NTS, доступные в качестве функций базы данных, будут преобразованы в SQL.</span><span class="sxs-lookup"><span data-stu-id="6da40-160">In LINQ, the NTS methods and properties available as database functions will be translated to SQL.</span></span> <span data-ttu-id="6da40-161">Например, методы Distance и Contains преобразуются в следующие запросы.</span><span class="sxs-lookup"><span data-stu-id="6da40-161">For example, the Distance and Contains methods are translated in the following queries.</span></span> <span data-ttu-id="6da40-162">В таблице в конце этой статьи показано, какие элементы поддерживаются различными поставщиками EF Core.</span><span class="sxs-lookup"><span data-stu-id="6da40-162">The table at the end of this article shows which members are supported by various EF Core providers.</span></span>

``` csharp
var nearestCity = db.Cities
    .OrderBy(c => c.Location.Distance(currentLocation))
    .FirstOrDefault();

var currentCountry = db.Countries
    .FirstOrDefault(c => c.Border.Contains(currentLocation));
```

## <a name="sql-server"></a><span data-ttu-id="6da40-163">SQL Server</span><span class="sxs-lookup"><span data-stu-id="6da40-163">SQL Server</span></span>

<span data-ttu-id="6da40-164">Если вы используете SQL Server, необходимо учитывать некоторые дополнительные моменты.</span><span class="sxs-lookup"><span data-stu-id="6da40-164">If you're using SQL Server, there are some additional things you should be aware of.</span></span>

### <a name="geography-or-geometry"></a><span data-ttu-id="6da40-165">География или геометрические объекты</span><span class="sxs-lookup"><span data-stu-id="6da40-165">Geography or geometry</span></span>

<span data-ttu-id="6da40-166">По умолчанию пространственные свойства сопоставляются со столбцами `geography` в SQL Server.</span><span class="sxs-lookup"><span data-stu-id="6da40-166">By default, spatial properties are mapped to `geography` columns in SQL Server.</span></span> <span data-ttu-id="6da40-167">Чтобы использовать `geometry`, [Настройте тип столбца](xref:core/modeling/relational/data-types) в модели.</span><span class="sxs-lookup"><span data-stu-id="6da40-167">To use `geometry`, [configure the column type](xref:core/modeling/relational/data-types) in your model.</span></span>

### <a name="geography-polygon-rings"></a><span data-ttu-id="6da40-168">Кольца географических многоугольников</span><span class="sxs-lookup"><span data-stu-id="6da40-168">Geography polygon rings</span></span>

<span data-ttu-id="6da40-169">При использовании типа столбца `geography` SQL Server накладывает дополнительные требования на внешнее кольцо (или оболочку) и внутренние кольца (или отверстия).</span><span class="sxs-lookup"><span data-stu-id="6da40-169">When using the `geography` column type, SQL Server imposes additional requirements on the exterior ring (or shell) and interior rings (or holes).</span></span> <span data-ttu-id="6da40-170">Внешнее кольцо должно быть ориентировано против часовой стрелки и внутренних колец по часовой стрелке.</span><span class="sxs-lookup"><span data-stu-id="6da40-170">The exterior ring must be oriented counterclockwise and the interior rings clockwise.</span></span> <span data-ttu-id="6da40-171">NTS проверяет это перед отправкой значений в базу данных.</span><span class="sxs-lookup"><span data-stu-id="6da40-171">NTS validates this before sending values to the database.</span></span>

### <a name="fullglobe"></a><span data-ttu-id="6da40-172">FullGlobe</span><span class="sxs-lookup"><span data-stu-id="6da40-172">FullGlobe</span></span>

<span data-ttu-id="6da40-173">SQL Server имеет нестандартный тип Geometry для представления полного земного шара при использовании типа столбца `geography`.</span><span class="sxs-lookup"><span data-stu-id="6da40-173">SQL Server has a non-standard geometry type to represent the full globe when using the `geography` column type.</span></span> <span data-ttu-id="6da40-174">У него также есть способ представления многоугольников на основе полного земного шара (без внешнего кольца).</span><span class="sxs-lookup"><span data-stu-id="6da40-174">It also has a way to represent polygons based on the full globe (without an exterior ring).</span></span> <span data-ttu-id="6da40-175">Ни один из этих элементов не поддерживается NTS.</span><span class="sxs-lookup"><span data-stu-id="6da40-175">Neither of these are supported by NTS.</span></span>

> [!WARNING]
> <span data-ttu-id="6da40-176">FullGlobe и многоугольники, основанные на нем, не поддерживаются NTS.</span><span class="sxs-lookup"><span data-stu-id="6da40-176">FullGlobe and polygons based on it aren't supported by NTS.</span></span>

## <a name="sqlite"></a><span data-ttu-id="6da40-177">SQLite</span><span class="sxs-lookup"><span data-stu-id="6da40-177">SQLite</span></span>

<span data-ttu-id="6da40-178">Ниже приведена дополнительная информация для тех, кто использует SQLite.</span><span class="sxs-lookup"><span data-stu-id="6da40-178">Here is some additional information for those using SQLite.</span></span>

### <a name="installing-spatialite"></a><span data-ttu-id="6da40-179">Установка Спатиалите</span><span class="sxs-lookup"><span data-stu-id="6da40-179">Installing SpatiaLite</span></span>

<span data-ttu-id="6da40-180">В Windows собственная библиотека mod_spatialite распространяется как зависимость пакета NuGet.</span><span class="sxs-lookup"><span data-stu-id="6da40-180">On Windows, the native mod_spatialite library is distributed as a NuGet package dependency.</span></span> <span data-ttu-id="6da40-181">Другие платформы должны установить его отдельно.</span><span class="sxs-lookup"><span data-stu-id="6da40-181">Other platforms need to install it separately.</span></span> <span data-ttu-id="6da40-182">Обычно это делается с помощью диспетчера пакетов программного обеспечения.</span><span class="sxs-lookup"><span data-stu-id="6da40-182">This is typically done using a software package manager.</span></span> <span data-ttu-id="6da40-183">Например, можно использовать APT для Ubuntu и Homebrew на MacOS.</span><span class="sxs-lookup"><span data-stu-id="6da40-183">For example, you can use APT on Ubuntu and Homebrew on MacOS.</span></span>

``` sh
# Ubuntu
apt-get install libsqlite3-mod-spatialite

# macOS
brew install libspatialite
```

### <a name="configuring-srid"></a><span data-ttu-id="6da40-184">Настройка SRID</span><span class="sxs-lookup"><span data-stu-id="6da40-184">Configuring SRID</span></span>

<span data-ttu-id="6da40-185">В Спатиалите столбцам необходимо указать SRID для каждого столбца.</span><span class="sxs-lookup"><span data-stu-id="6da40-185">In SpatiaLite, columns need to specify an SRID per column.</span></span> <span data-ttu-id="6da40-186">По умолчанию SRID имеет значение `0`.</span><span class="sxs-lookup"><span data-stu-id="6da40-186">The default SRID is `0`.</span></span> <span data-ttu-id="6da40-187">Укажите другой SRID с помощью метода Форсклитехассрид.</span><span class="sxs-lookup"><span data-stu-id="6da40-187">Specify a different SRID using the ForSqliteHasSrid method.</span></span>

``` csharp
modelBuilder.Entity<City>().Property(c => c.Location)
    .ForSqliteHasSrid(4326);
```

### <a name="dimension"></a><span data-ttu-id="6da40-188">Измерение</span><span class="sxs-lookup"><span data-stu-id="6da40-188">Dimension</span></span>

<span data-ttu-id="6da40-189">Как и в случае с SRID, измерение столбца (или координата) также указывается как часть столбца.</span><span class="sxs-lookup"><span data-stu-id="6da40-189">Similar to SRID, a column's dimension (or ordinates) is also specified as part of the column.</span></span> <span data-ttu-id="6da40-190">По умолчанию используются координаты X и Y. Включите дополнительные координаты (Z и M) с помощью метода Форсклитехасдименсион.</span><span class="sxs-lookup"><span data-stu-id="6da40-190">The default ordinates are X and Y. Enable additional ordinates (Z and M) using the ForSqliteHasDimension method.</span></span>

``` csharp
modelBuilder.Entity<City>().Property(c => c.Location)
    .ForSqliteHasDimension(Ordinates.XYZ);
```

## <a name="translated-operations"></a><span data-ttu-id="6da40-191">Переведенные операции</span><span class="sxs-lookup"><span data-stu-id="6da40-191">Translated Operations</span></span>

<span data-ttu-id="6da40-192">В этой таблице показано, какие элементы NTS преобразуются в SQL каждым поставщиком EF Core.</span><span class="sxs-lookup"><span data-stu-id="6da40-192">This table shows which NTS members are translated into SQL by each EF Core provider.</span></span>

<span data-ttu-id="6da40-193">неттопологисуите</span><span class="sxs-lookup"><span data-stu-id="6da40-193">NetTopologySuite</span></span> | <span data-ttu-id="6da40-194">SQL Server (геометрия)</span><span class="sxs-lookup"><span data-stu-id="6da40-194">SQL Server (geometry)</span></span> | <span data-ttu-id="6da40-195">SQL Server (география)</span><span class="sxs-lookup"><span data-stu-id="6da40-195">SQL Server (geography)</span></span> | <span data-ttu-id="6da40-196">SQLite</span><span class="sxs-lookup"><span data-stu-id="6da40-196">SQLite</span></span> | <span data-ttu-id="6da40-197">Npgsql</span><span class="sxs-lookup"><span data-stu-id="6da40-197">Npgsql</span></span>
--- |:---:|:---:|:---:|:---:
<span data-ttu-id="6da40-198">Геометрия. область</span><span class="sxs-lookup"><span data-stu-id="6da40-198">Geometry.Area</span></span> | <span data-ttu-id="6da40-199">✔</span><span class="sxs-lookup"><span data-stu-id="6da40-199">✔</span></span> | <span data-ttu-id="6da40-200">✔</span><span class="sxs-lookup"><span data-stu-id="6da40-200">✔</span></span> | <span data-ttu-id="6da40-201">✔</span><span class="sxs-lookup"><span data-stu-id="6da40-201">✔</span></span> | <span data-ttu-id="6da40-202">✔</span><span class="sxs-lookup"><span data-stu-id="6da40-202">✔</span></span>
<span data-ttu-id="6da40-203">Geometry. Асбинари ()</span><span class="sxs-lookup"><span data-stu-id="6da40-203">Geometry.AsBinary()</span></span> | <span data-ttu-id="6da40-204">✔</span><span class="sxs-lookup"><span data-stu-id="6da40-204">✔</span></span> | <span data-ttu-id="6da40-205">✔</span><span class="sxs-lookup"><span data-stu-id="6da40-205">✔</span></span> | <span data-ttu-id="6da40-206">✔</span><span class="sxs-lookup"><span data-stu-id="6da40-206">✔</span></span> | <span data-ttu-id="6da40-207">✔</span><span class="sxs-lookup"><span data-stu-id="6da40-207">✔</span></span>
<span data-ttu-id="6da40-208">Geometry. Астекст ()</span><span class="sxs-lookup"><span data-stu-id="6da40-208">Geometry.AsText()</span></span> | <span data-ttu-id="6da40-209">✔</span><span class="sxs-lookup"><span data-stu-id="6da40-209">✔</span></span> | <span data-ttu-id="6da40-210">✔</span><span class="sxs-lookup"><span data-stu-id="6da40-210">✔</span></span> | <span data-ttu-id="6da40-211">✔</span><span class="sxs-lookup"><span data-stu-id="6da40-211">✔</span></span> | <span data-ttu-id="6da40-212">✔</span><span class="sxs-lookup"><span data-stu-id="6da40-212">✔</span></span>
<span data-ttu-id="6da40-213">Geometry. Граничный</span><span class="sxs-lookup"><span data-stu-id="6da40-213">Geometry.Boundary</span></span> | <span data-ttu-id="6da40-214">✔</span><span class="sxs-lookup"><span data-stu-id="6da40-214">✔</span></span> | | <span data-ttu-id="6da40-215">✔</span><span class="sxs-lookup"><span data-stu-id="6da40-215">✔</span></span> | <span data-ttu-id="6da40-216">✔</span><span class="sxs-lookup"><span data-stu-id="6da40-216">✔</span></span>
<span data-ttu-id="6da40-217">Геометрия. buffer (двойное)</span><span class="sxs-lookup"><span data-stu-id="6da40-217">Geometry.Buffer(double)</span></span> | <span data-ttu-id="6da40-218">✔</span><span class="sxs-lookup"><span data-stu-id="6da40-218">✔</span></span> | <span data-ttu-id="6da40-219">✔</span><span class="sxs-lookup"><span data-stu-id="6da40-219">✔</span></span> | <span data-ttu-id="6da40-220">✔</span><span class="sxs-lookup"><span data-stu-id="6da40-220">✔</span></span> | <span data-ttu-id="6da40-221">✔</span><span class="sxs-lookup"><span data-stu-id="6da40-221">✔</span></span>
<span data-ttu-id="6da40-222">Геометрия. buffer (Double, int)</span><span class="sxs-lookup"><span data-stu-id="6da40-222">Geometry.Buffer(double, int)</span></span> | | | <span data-ttu-id="6da40-223">✔</span><span class="sxs-lookup"><span data-stu-id="6da40-223">✔</span></span>
<span data-ttu-id="6da40-224">Geometry. центроид</span><span class="sxs-lookup"><span data-stu-id="6da40-224">Geometry.Centroid</span></span> | <span data-ttu-id="6da40-225">✔</span><span class="sxs-lookup"><span data-stu-id="6da40-225">✔</span></span> | | <span data-ttu-id="6da40-226">✔</span><span class="sxs-lookup"><span data-stu-id="6da40-226">✔</span></span> | <span data-ttu-id="6da40-227">✔</span><span class="sxs-lookup"><span data-stu-id="6da40-227">✔</span></span>
<span data-ttu-id="6da40-228">Геометрия. Contains (геометрия)</span><span class="sxs-lookup"><span data-stu-id="6da40-228">Geometry.Contains(Geometry)</span></span> | <span data-ttu-id="6da40-229">✔</span><span class="sxs-lookup"><span data-stu-id="6da40-229">✔</span></span> | <span data-ttu-id="6da40-230">✔</span><span class="sxs-lookup"><span data-stu-id="6da40-230">✔</span></span> | <span data-ttu-id="6da40-231">✔</span><span class="sxs-lookup"><span data-stu-id="6da40-231">✔</span></span> | <span data-ttu-id="6da40-232">✔</span><span class="sxs-lookup"><span data-stu-id="6da40-232">✔</span></span>
<span data-ttu-id="6da40-233">Geometry. Конвексхулл ()</span><span class="sxs-lookup"><span data-stu-id="6da40-233">Geometry.ConvexHull()</span></span> | <span data-ttu-id="6da40-234">✔</span><span class="sxs-lookup"><span data-stu-id="6da40-234">✔</span></span> | <span data-ttu-id="6da40-235">✔</span><span class="sxs-lookup"><span data-stu-id="6da40-235">✔</span></span> | <span data-ttu-id="6da40-236">✔</span><span class="sxs-lookup"><span data-stu-id="6da40-236">✔</span></span> | <span data-ttu-id="6da40-237">✔</span><span class="sxs-lookup"><span data-stu-id="6da40-237">✔</span></span>
<span data-ttu-id="6da40-238">Geometry. Ковередби (геометрия)</span><span class="sxs-lookup"><span data-stu-id="6da40-238">Geometry.CoveredBy(Geometry)</span></span> | | | <span data-ttu-id="6da40-239">✔</span><span class="sxs-lookup"><span data-stu-id="6da40-239">✔</span></span> | <span data-ttu-id="6da40-240">✔</span><span class="sxs-lookup"><span data-stu-id="6da40-240">✔</span></span>
<span data-ttu-id="6da40-241">Geometry. крышки (Geometry)</span><span class="sxs-lookup"><span data-stu-id="6da40-241">Geometry.Covers(Geometry)</span></span> | | | <span data-ttu-id="6da40-242">✔</span><span class="sxs-lookup"><span data-stu-id="6da40-242">✔</span></span> | <span data-ttu-id="6da40-243">✔</span><span class="sxs-lookup"><span data-stu-id="6da40-243">✔</span></span>
<span data-ttu-id="6da40-244">Геометрия. перекрестий (Geometry)</span><span class="sxs-lookup"><span data-stu-id="6da40-244">Geometry.Crosses(Geometry)</span></span> | <span data-ttu-id="6da40-245">✔</span><span class="sxs-lookup"><span data-stu-id="6da40-245">✔</span></span> | | <span data-ttu-id="6da40-246">✔</span><span class="sxs-lookup"><span data-stu-id="6da40-246">✔</span></span> | <span data-ttu-id="6da40-247">✔</span><span class="sxs-lookup"><span data-stu-id="6da40-247">✔</span></span>
<span data-ttu-id="6da40-248">Геометрия. Difference (геометрия)</span><span class="sxs-lookup"><span data-stu-id="6da40-248">Geometry.Difference(Geometry)</span></span> | <span data-ttu-id="6da40-249">✔</span><span class="sxs-lookup"><span data-stu-id="6da40-249">✔</span></span> | <span data-ttu-id="6da40-250">✔</span><span class="sxs-lookup"><span data-stu-id="6da40-250">✔</span></span> | <span data-ttu-id="6da40-251">✔</span><span class="sxs-lookup"><span data-stu-id="6da40-251">✔</span></span> | <span data-ttu-id="6da40-252">✔</span><span class="sxs-lookup"><span data-stu-id="6da40-252">✔</span></span>
<span data-ttu-id="6da40-253">Geometry. Dimension</span><span class="sxs-lookup"><span data-stu-id="6da40-253">Geometry.Dimension</span></span> | <span data-ttu-id="6da40-254">✔</span><span class="sxs-lookup"><span data-stu-id="6da40-254">✔</span></span> | <span data-ttu-id="6da40-255">✔</span><span class="sxs-lookup"><span data-stu-id="6da40-255">✔</span></span> | <span data-ttu-id="6da40-256">✔</span><span class="sxs-lookup"><span data-stu-id="6da40-256">✔</span></span> | <span data-ttu-id="6da40-257">✔</span><span class="sxs-lookup"><span data-stu-id="6da40-257">✔</span></span>
<span data-ttu-id="6da40-258">Geometry. unjoin (геометрия)</span><span class="sxs-lookup"><span data-stu-id="6da40-258">Geometry.Disjoint(Geometry)</span></span> | <span data-ttu-id="6da40-259">✔</span><span class="sxs-lookup"><span data-stu-id="6da40-259">✔</span></span> | <span data-ttu-id="6da40-260">✔</span><span class="sxs-lookup"><span data-stu-id="6da40-260">✔</span></span> | <span data-ttu-id="6da40-261">✔</span><span class="sxs-lookup"><span data-stu-id="6da40-261">✔</span></span> | <span data-ttu-id="6da40-262">✔</span><span class="sxs-lookup"><span data-stu-id="6da40-262">✔</span></span>
<span data-ttu-id="6da40-263">Геометрия. Distance (геометрия)</span><span class="sxs-lookup"><span data-stu-id="6da40-263">Geometry.Distance(Geometry)</span></span> | <span data-ttu-id="6da40-264">✔</span><span class="sxs-lookup"><span data-stu-id="6da40-264">✔</span></span> | <span data-ttu-id="6da40-265">✔</span><span class="sxs-lookup"><span data-stu-id="6da40-265">✔</span></span> | <span data-ttu-id="6da40-266">✔</span><span class="sxs-lookup"><span data-stu-id="6da40-266">✔</span></span> | <span data-ttu-id="6da40-267">✔</span><span class="sxs-lookup"><span data-stu-id="6da40-267">✔</span></span>
<span data-ttu-id="6da40-268">Геометрия. конверт</span><span class="sxs-lookup"><span data-stu-id="6da40-268">Geometry.Envelope</span></span> | <span data-ttu-id="6da40-269">✔</span><span class="sxs-lookup"><span data-stu-id="6da40-269">✔</span></span> | | <span data-ttu-id="6da40-270">✔</span><span class="sxs-lookup"><span data-stu-id="6da40-270">✔</span></span> | <span data-ttu-id="6da40-271">✔</span><span class="sxs-lookup"><span data-stu-id="6da40-271">✔</span></span>
<span data-ttu-id="6da40-272">Geometry. Екуалсексакт (геометрия)</span><span class="sxs-lookup"><span data-stu-id="6da40-272">Geometry.EqualsExact(Geometry)</span></span> | | | | <span data-ttu-id="6da40-273">✔</span><span class="sxs-lookup"><span data-stu-id="6da40-273">✔</span></span>
<span data-ttu-id="6da40-274">Geometry. Екуалстопологикалли (геометрия)</span><span class="sxs-lookup"><span data-stu-id="6da40-274">Geometry.EqualsTopologically(Geometry)</span></span> | <span data-ttu-id="6da40-275">✔</span><span class="sxs-lookup"><span data-stu-id="6da40-275">✔</span></span> | <span data-ttu-id="6da40-276">✔</span><span class="sxs-lookup"><span data-stu-id="6da40-276">✔</span></span> | <span data-ttu-id="6da40-277">✔</span><span class="sxs-lookup"><span data-stu-id="6da40-277">✔</span></span> | <span data-ttu-id="6da40-278">✔</span><span class="sxs-lookup"><span data-stu-id="6da40-278">✔</span></span>
<span data-ttu-id="6da40-279">Geometry. Жеометритипе</span><span class="sxs-lookup"><span data-stu-id="6da40-279">Geometry.GeometryType</span></span> | <span data-ttu-id="6da40-280">✔</span><span class="sxs-lookup"><span data-stu-id="6da40-280">✔</span></span> | <span data-ttu-id="6da40-281">✔</span><span class="sxs-lookup"><span data-stu-id="6da40-281">✔</span></span> | <span data-ttu-id="6da40-282">✔</span><span class="sxs-lookup"><span data-stu-id="6da40-282">✔</span></span> | <span data-ttu-id="6da40-283">✔</span><span class="sxs-lookup"><span data-stu-id="6da40-283">✔</span></span>
<span data-ttu-id="6da40-284">Geometry. Жетжеометрин (int)</span><span class="sxs-lookup"><span data-stu-id="6da40-284">Geometry.GetGeometryN(int)</span></span> | <span data-ttu-id="6da40-285">✔</span><span class="sxs-lookup"><span data-stu-id="6da40-285">✔</span></span> | | <span data-ttu-id="6da40-286">✔</span><span class="sxs-lookup"><span data-stu-id="6da40-286">✔</span></span> | <span data-ttu-id="6da40-287">✔</span><span class="sxs-lookup"><span data-stu-id="6da40-287">✔</span></span>
<span data-ttu-id="6da40-288">Geometry. Интериорпоинт</span><span class="sxs-lookup"><span data-stu-id="6da40-288">Geometry.InteriorPoint</span></span> | <span data-ttu-id="6da40-289">✔</span><span class="sxs-lookup"><span data-stu-id="6da40-289">✔</span></span> | | <span data-ttu-id="6da40-290">✔</span><span class="sxs-lookup"><span data-stu-id="6da40-290">✔</span></span>
<span data-ttu-id="6da40-291">Geometry. пересечение (Geometry)</span><span class="sxs-lookup"><span data-stu-id="6da40-291">Geometry.Intersection(Geometry)</span></span> | <span data-ttu-id="6da40-292">✔</span><span class="sxs-lookup"><span data-stu-id="6da40-292">✔</span></span> | <span data-ttu-id="6da40-293">✔</span><span class="sxs-lookup"><span data-stu-id="6da40-293">✔</span></span> | <span data-ttu-id="6da40-294">✔</span><span class="sxs-lookup"><span data-stu-id="6da40-294">✔</span></span> | <span data-ttu-id="6da40-295">✔</span><span class="sxs-lookup"><span data-stu-id="6da40-295">✔</span></span>
<span data-ttu-id="6da40-296">Геометрию. intersects (геометрия)</span><span class="sxs-lookup"><span data-stu-id="6da40-296">Geometry.Intersects(Geometry)</span></span> | <span data-ttu-id="6da40-297">✔</span><span class="sxs-lookup"><span data-stu-id="6da40-297">✔</span></span> | <span data-ttu-id="6da40-298">✔</span><span class="sxs-lookup"><span data-stu-id="6da40-298">✔</span></span> | <span data-ttu-id="6da40-299">✔</span><span class="sxs-lookup"><span data-stu-id="6da40-299">✔</span></span> | <span data-ttu-id="6da40-300">✔</span><span class="sxs-lookup"><span data-stu-id="6da40-300">✔</span></span>
<span data-ttu-id="6da40-301">Geometry. isEmpty</span><span class="sxs-lookup"><span data-stu-id="6da40-301">Geometry.IsEmpty</span></span> | <span data-ttu-id="6da40-302">✔</span><span class="sxs-lookup"><span data-stu-id="6da40-302">✔</span></span> | <span data-ttu-id="6da40-303">✔</span><span class="sxs-lookup"><span data-stu-id="6da40-303">✔</span></span> | <span data-ttu-id="6da40-304">✔</span><span class="sxs-lookup"><span data-stu-id="6da40-304">✔</span></span> | <span data-ttu-id="6da40-305">✔</span><span class="sxs-lookup"><span data-stu-id="6da40-305">✔</span></span>
<span data-ttu-id="6da40-306">Geometry. Простое_имя</span><span class="sxs-lookup"><span data-stu-id="6da40-306">Geometry.IsSimple</span></span> | <span data-ttu-id="6da40-307">✔</span><span class="sxs-lookup"><span data-stu-id="6da40-307">✔</span></span> | | <span data-ttu-id="6da40-308">✔</span><span class="sxs-lookup"><span data-stu-id="6da40-308">✔</span></span> | <span data-ttu-id="6da40-309">✔</span><span class="sxs-lookup"><span data-stu-id="6da40-309">✔</span></span>
<span data-ttu-id="6da40-310">Geometry. IsValid</span><span class="sxs-lookup"><span data-stu-id="6da40-310">Geometry.IsValid</span></span> | <span data-ttu-id="6da40-311">✔</span><span class="sxs-lookup"><span data-stu-id="6da40-311">✔</span></span> | <span data-ttu-id="6da40-312">✔</span><span class="sxs-lookup"><span data-stu-id="6da40-312">✔</span></span> | <span data-ttu-id="6da40-313">✔</span><span class="sxs-lookup"><span data-stu-id="6da40-313">✔</span></span> | <span data-ttu-id="6da40-314">✔</span><span class="sxs-lookup"><span data-stu-id="6da40-314">✔</span></span>
<span data-ttu-id="6da40-315">Geometry. Исвисиндистанце (геометрия, Double)</span><span class="sxs-lookup"><span data-stu-id="6da40-315">Geometry.IsWithinDistance(Geometry, double)</span></span> | <span data-ttu-id="6da40-316">✔</span><span class="sxs-lookup"><span data-stu-id="6da40-316">✔</span></span> | | <span data-ttu-id="6da40-317">✔</span><span class="sxs-lookup"><span data-stu-id="6da40-317">✔</span></span>
<span data-ttu-id="6da40-318">Геометрия геометрии. length</span><span class="sxs-lookup"><span data-stu-id="6da40-318">Geometry.Length</span></span> | <span data-ttu-id="6da40-319">✔</span><span class="sxs-lookup"><span data-stu-id="6da40-319">✔</span></span> | <span data-ttu-id="6da40-320">✔</span><span class="sxs-lookup"><span data-stu-id="6da40-320">✔</span></span> | <span data-ttu-id="6da40-321">✔</span><span class="sxs-lookup"><span data-stu-id="6da40-321">✔</span></span> | <span data-ttu-id="6da40-322">✔</span><span class="sxs-lookup"><span data-stu-id="6da40-322">✔</span></span>
<span data-ttu-id="6da40-323">Geometry. Нумжеометриес</span><span class="sxs-lookup"><span data-stu-id="6da40-323">Geometry.NumGeometries</span></span> | <span data-ttu-id="6da40-324">✔</span><span class="sxs-lookup"><span data-stu-id="6da40-324">✔</span></span> | <span data-ttu-id="6da40-325">✔</span><span class="sxs-lookup"><span data-stu-id="6da40-325">✔</span></span> | <span data-ttu-id="6da40-326">✔</span><span class="sxs-lookup"><span data-stu-id="6da40-326">✔</span></span> | <span data-ttu-id="6da40-327">✔</span><span class="sxs-lookup"><span data-stu-id="6da40-327">✔</span></span>
<span data-ttu-id="6da40-328">Geometry. Нумпоинтс</span><span class="sxs-lookup"><span data-stu-id="6da40-328">Geometry.NumPoints</span></span> | <span data-ttu-id="6da40-329">✔</span><span class="sxs-lookup"><span data-stu-id="6da40-329">✔</span></span> | <span data-ttu-id="6da40-330">✔</span><span class="sxs-lookup"><span data-stu-id="6da40-330">✔</span></span> | <span data-ttu-id="6da40-331">✔</span><span class="sxs-lookup"><span data-stu-id="6da40-331">✔</span></span> | <span data-ttu-id="6da40-332">✔</span><span class="sxs-lookup"><span data-stu-id="6da40-332">✔</span></span>
<span data-ttu-id="6da40-333">Geometry. Огкжеометритипе</span><span class="sxs-lookup"><span data-stu-id="6da40-333">Geometry.OgcGeometryType</span></span> | <span data-ttu-id="6da40-334">✔</span><span class="sxs-lookup"><span data-stu-id="6da40-334">✔</span></span> | <span data-ttu-id="6da40-335">✔</span><span class="sxs-lookup"><span data-stu-id="6da40-335">✔</span></span> | <span data-ttu-id="6da40-336">✔</span><span class="sxs-lookup"><span data-stu-id="6da40-336">✔</span></span>
<span data-ttu-id="6da40-337">Геометрия Geometry. пересекающих объектов (Geometry)</span><span class="sxs-lookup"><span data-stu-id="6da40-337">Geometry.Overlaps(Geometry)</span></span> | <span data-ttu-id="6da40-338">✔</span><span class="sxs-lookup"><span data-stu-id="6da40-338">✔</span></span> | <span data-ttu-id="6da40-339">✔</span><span class="sxs-lookup"><span data-stu-id="6da40-339">✔</span></span> | <span data-ttu-id="6da40-340">✔</span><span class="sxs-lookup"><span data-stu-id="6da40-340">✔</span></span> | <span data-ttu-id="6da40-341">✔</span><span class="sxs-lookup"><span data-stu-id="6da40-341">✔</span></span>
<span data-ttu-id="6da40-342">Geometry. Поинтонсурфаце</span><span class="sxs-lookup"><span data-stu-id="6da40-342">Geometry.PointOnSurface</span></span> | <span data-ttu-id="6da40-343">✔</span><span class="sxs-lookup"><span data-stu-id="6da40-343">✔</span></span> | | <span data-ttu-id="6da40-344">✔</span><span class="sxs-lookup"><span data-stu-id="6da40-344">✔</span></span> | <span data-ttu-id="6da40-345">✔</span><span class="sxs-lookup"><span data-stu-id="6da40-345">✔</span></span>
<span data-ttu-id="6da40-346">Geometry. Late (геометрия, строка)</span><span class="sxs-lookup"><span data-stu-id="6da40-346">Geometry.Relate(Geometry, string)</span></span> | <span data-ttu-id="6da40-347">✔</span><span class="sxs-lookup"><span data-stu-id="6da40-347">✔</span></span> | | <span data-ttu-id="6da40-348">✔</span><span class="sxs-lookup"><span data-stu-id="6da40-348">✔</span></span> | <span data-ttu-id="6da40-349">✔</span><span class="sxs-lookup"><span data-stu-id="6da40-349">✔</span></span>
<span data-ttu-id="6da40-350">Геометрия. Reverse ()</span><span class="sxs-lookup"><span data-stu-id="6da40-350">Geometry.Reverse()</span></span> | | | <span data-ttu-id="6da40-351">✔</span><span class="sxs-lookup"><span data-stu-id="6da40-351">✔</span></span> | <span data-ttu-id="6da40-352">✔</span><span class="sxs-lookup"><span data-stu-id="6da40-352">✔</span></span>
<span data-ttu-id="6da40-353">Geometry. SRID</span><span class="sxs-lookup"><span data-stu-id="6da40-353">Geometry.SRID</span></span> | <span data-ttu-id="6da40-354">✔</span><span class="sxs-lookup"><span data-stu-id="6da40-354">✔</span></span> | <span data-ttu-id="6da40-355">✔</span><span class="sxs-lookup"><span data-stu-id="6da40-355">✔</span></span> | <span data-ttu-id="6da40-356">✔</span><span class="sxs-lookup"><span data-stu-id="6da40-356">✔</span></span> | <span data-ttu-id="6da40-357">✔</span><span class="sxs-lookup"><span data-stu-id="6da40-357">✔</span></span>
<span data-ttu-id="6da40-358">Geometry. Симметрикдифференце (геометрия)</span><span class="sxs-lookup"><span data-stu-id="6da40-358">Geometry.SymmetricDifference(Geometry)</span></span> | <span data-ttu-id="6da40-359">✔</span><span class="sxs-lookup"><span data-stu-id="6da40-359">✔</span></span> | <span data-ttu-id="6da40-360">✔</span><span class="sxs-lookup"><span data-stu-id="6da40-360">✔</span></span> | <span data-ttu-id="6da40-361">✔</span><span class="sxs-lookup"><span data-stu-id="6da40-361">✔</span></span> | <span data-ttu-id="6da40-362">✔</span><span class="sxs-lookup"><span data-stu-id="6da40-362">✔</span></span>
<span data-ttu-id="6da40-363">Geometry. Тобинари ()</span><span class="sxs-lookup"><span data-stu-id="6da40-363">Geometry.ToBinary()</span></span> | <span data-ttu-id="6da40-364">✔</span><span class="sxs-lookup"><span data-stu-id="6da40-364">✔</span></span> | <span data-ttu-id="6da40-365">✔</span><span class="sxs-lookup"><span data-stu-id="6da40-365">✔</span></span> | <span data-ttu-id="6da40-366">✔</span><span class="sxs-lookup"><span data-stu-id="6da40-366">✔</span></span> | <span data-ttu-id="6da40-367">✔</span><span class="sxs-lookup"><span data-stu-id="6da40-367">✔</span></span>
<span data-ttu-id="6da40-368">Geometry. Тотекст ()</span><span class="sxs-lookup"><span data-stu-id="6da40-368">Geometry.ToText()</span></span> | <span data-ttu-id="6da40-369">✔</span><span class="sxs-lookup"><span data-stu-id="6da40-369">✔</span></span> | <span data-ttu-id="6da40-370">✔</span><span class="sxs-lookup"><span data-stu-id="6da40-370">✔</span></span> | <span data-ttu-id="6da40-371">✔</span><span class="sxs-lookup"><span data-stu-id="6da40-371">✔</span></span> | <span data-ttu-id="6da40-372">✔</span><span class="sxs-lookup"><span data-stu-id="6da40-372">✔</span></span>
<span data-ttu-id="6da40-373">Geometry. касания (геометрия)</span><span class="sxs-lookup"><span data-stu-id="6da40-373">Geometry.Touches(Geometry)</span></span> | <span data-ttu-id="6da40-374">✔</span><span class="sxs-lookup"><span data-stu-id="6da40-374">✔</span></span> | | <span data-ttu-id="6da40-375">✔</span><span class="sxs-lookup"><span data-stu-id="6da40-375">✔</span></span> | <span data-ttu-id="6da40-376">✔</span><span class="sxs-lookup"><span data-stu-id="6da40-376">✔</span></span>
<span data-ttu-id="6da40-377">Geometry. Union ()</span><span class="sxs-lookup"><span data-stu-id="6da40-377">Geometry.Union()</span></span> | | | <span data-ttu-id="6da40-378">✔</span><span class="sxs-lookup"><span data-stu-id="6da40-378">✔</span></span>
<span data-ttu-id="6da40-379">Geometry. Union (геометрия)</span><span class="sxs-lookup"><span data-stu-id="6da40-379">Geometry.Union(Geometry)</span></span> | <span data-ttu-id="6da40-380">✔</span><span class="sxs-lookup"><span data-stu-id="6da40-380">✔</span></span> | <span data-ttu-id="6da40-381">✔</span><span class="sxs-lookup"><span data-stu-id="6da40-381">✔</span></span> | <span data-ttu-id="6da40-382">✔</span><span class="sxs-lookup"><span data-stu-id="6da40-382">✔</span></span> | <span data-ttu-id="6da40-383">✔</span><span class="sxs-lookup"><span data-stu-id="6da40-383">✔</span></span>
<span data-ttu-id="6da40-384">Геометрия. внутри (Geometry)</span><span class="sxs-lookup"><span data-stu-id="6da40-384">Geometry.Within(Geometry)</span></span> | <span data-ttu-id="6da40-385">✔</span><span class="sxs-lookup"><span data-stu-id="6da40-385">✔</span></span> | <span data-ttu-id="6da40-386">✔</span><span class="sxs-lookup"><span data-stu-id="6da40-386">✔</span></span> | <span data-ttu-id="6da40-387">✔</span><span class="sxs-lookup"><span data-stu-id="6da40-387">✔</span></span> | <span data-ttu-id="6da40-388">✔</span><span class="sxs-lookup"><span data-stu-id="6da40-388">✔</span></span>
<span data-ttu-id="6da40-389">GeometryCollection. Count</span><span class="sxs-lookup"><span data-stu-id="6da40-389">GeometryCollection.Count</span></span> | <span data-ttu-id="6da40-390">✔</span><span class="sxs-lookup"><span data-stu-id="6da40-390">✔</span></span> | <span data-ttu-id="6da40-391">✔</span><span class="sxs-lookup"><span data-stu-id="6da40-391">✔</span></span> | <span data-ttu-id="6da40-392">✔</span><span class="sxs-lookup"><span data-stu-id="6da40-392">✔</span></span> | <span data-ttu-id="6da40-393">✔</span><span class="sxs-lookup"><span data-stu-id="6da40-393">✔</span></span>
<span data-ttu-id="6da40-394">GeometryCollection [int]</span><span class="sxs-lookup"><span data-stu-id="6da40-394">GeometryCollection[int]</span></span> | <span data-ttu-id="6da40-395">✔</span><span class="sxs-lookup"><span data-stu-id="6da40-395">✔</span></span> | <span data-ttu-id="6da40-396">✔</span><span class="sxs-lookup"><span data-stu-id="6da40-396">✔</span></span> | <span data-ttu-id="6da40-397">✔</span><span class="sxs-lookup"><span data-stu-id="6da40-397">✔</span></span> | <span data-ttu-id="6da40-398">✔</span><span class="sxs-lookup"><span data-stu-id="6da40-398">✔</span></span>
<span data-ttu-id="6da40-399">LineString. Count</span><span class="sxs-lookup"><span data-stu-id="6da40-399">LineString.Count</span></span> | <span data-ttu-id="6da40-400">✔</span><span class="sxs-lookup"><span data-stu-id="6da40-400">✔</span></span> | <span data-ttu-id="6da40-401">✔</span><span class="sxs-lookup"><span data-stu-id="6da40-401">✔</span></span> | <span data-ttu-id="6da40-402">✔</span><span class="sxs-lookup"><span data-stu-id="6da40-402">✔</span></span> | <span data-ttu-id="6da40-403">✔</span><span class="sxs-lookup"><span data-stu-id="6da40-403">✔</span></span>
<span data-ttu-id="6da40-404">LineString. EndPoint</span><span class="sxs-lookup"><span data-stu-id="6da40-404">LineString.EndPoint</span></span> | <span data-ttu-id="6da40-405">✔</span><span class="sxs-lookup"><span data-stu-id="6da40-405">✔</span></span> | <span data-ttu-id="6da40-406">✔</span><span class="sxs-lookup"><span data-stu-id="6da40-406">✔</span></span> | <span data-ttu-id="6da40-407">✔</span><span class="sxs-lookup"><span data-stu-id="6da40-407">✔</span></span> | <span data-ttu-id="6da40-408">✔</span><span class="sxs-lookup"><span data-stu-id="6da40-408">✔</span></span>
<span data-ttu-id="6da40-409">LineString. Жетпоинтн (int)</span><span class="sxs-lookup"><span data-stu-id="6da40-409">LineString.GetPointN(int)</span></span> | <span data-ttu-id="6da40-410">✔</span><span class="sxs-lookup"><span data-stu-id="6da40-410">✔</span></span> | <span data-ttu-id="6da40-411">✔</span><span class="sxs-lookup"><span data-stu-id="6da40-411">✔</span></span> | <span data-ttu-id="6da40-412">✔</span><span class="sxs-lookup"><span data-stu-id="6da40-412">✔</span></span> | <span data-ttu-id="6da40-413">✔</span><span class="sxs-lookup"><span data-stu-id="6da40-413">✔</span></span>
<span data-ttu-id="6da40-414">LineString. крыл</span><span class="sxs-lookup"><span data-stu-id="6da40-414">LineString.IsClosed</span></span> | <span data-ttu-id="6da40-415">✔</span><span class="sxs-lookup"><span data-stu-id="6da40-415">✔</span></span> | <span data-ttu-id="6da40-416">✔</span><span class="sxs-lookup"><span data-stu-id="6da40-416">✔</span></span> | <span data-ttu-id="6da40-417">✔</span><span class="sxs-lookup"><span data-stu-id="6da40-417">✔</span></span> | <span data-ttu-id="6da40-418">✔</span><span class="sxs-lookup"><span data-stu-id="6da40-418">✔</span></span>
<span data-ttu-id="6da40-419">LineString. Звонок</span><span class="sxs-lookup"><span data-stu-id="6da40-419">LineString.IsRing</span></span> | <span data-ttu-id="6da40-420">✔</span><span class="sxs-lookup"><span data-stu-id="6da40-420">✔</span></span> | | <span data-ttu-id="6da40-421">✔</span><span class="sxs-lookup"><span data-stu-id="6da40-421">✔</span></span> | <span data-ttu-id="6da40-422">✔</span><span class="sxs-lookup"><span data-stu-id="6da40-422">✔</span></span>
<span data-ttu-id="6da40-423">LineString. StartPoint</span><span class="sxs-lookup"><span data-stu-id="6da40-423">LineString.StartPoint</span></span> | <span data-ttu-id="6da40-424">✔</span><span class="sxs-lookup"><span data-stu-id="6da40-424">✔</span></span> | <span data-ttu-id="6da40-425">✔</span><span class="sxs-lookup"><span data-stu-id="6da40-425">✔</span></span> | <span data-ttu-id="6da40-426">✔</span><span class="sxs-lookup"><span data-stu-id="6da40-426">✔</span></span> | <span data-ttu-id="6da40-427">✔</span><span class="sxs-lookup"><span data-stu-id="6da40-427">✔</span></span>
<span data-ttu-id="6da40-428">MultiLineString. крыл</span><span class="sxs-lookup"><span data-stu-id="6da40-428">MultiLineString.IsClosed</span></span> | <span data-ttu-id="6da40-429">✔</span><span class="sxs-lookup"><span data-stu-id="6da40-429">✔</span></span> | <span data-ttu-id="6da40-430">✔</span><span class="sxs-lookup"><span data-stu-id="6da40-430">✔</span></span> | <span data-ttu-id="6da40-431">✔</span><span class="sxs-lookup"><span data-stu-id="6da40-431">✔</span></span> | <span data-ttu-id="6da40-432">✔</span><span class="sxs-lookup"><span data-stu-id="6da40-432">✔</span></span>
<span data-ttu-id="6da40-433">Point. M</span><span class="sxs-lookup"><span data-stu-id="6da40-433">Point.M</span></span> | <span data-ttu-id="6da40-434">✔</span><span class="sxs-lookup"><span data-stu-id="6da40-434">✔</span></span> | <span data-ttu-id="6da40-435">✔</span><span class="sxs-lookup"><span data-stu-id="6da40-435">✔</span></span> | <span data-ttu-id="6da40-436">✔</span><span class="sxs-lookup"><span data-stu-id="6da40-436">✔</span></span> | <span data-ttu-id="6da40-437">✔</span><span class="sxs-lookup"><span data-stu-id="6da40-437">✔</span></span>
<span data-ttu-id="6da40-438">Point. X</span><span class="sxs-lookup"><span data-stu-id="6da40-438">Point.X</span></span> | <span data-ttu-id="6da40-439">✔</span><span class="sxs-lookup"><span data-stu-id="6da40-439">✔</span></span> | <span data-ttu-id="6da40-440">✔</span><span class="sxs-lookup"><span data-stu-id="6da40-440">✔</span></span> | <span data-ttu-id="6da40-441">✔</span><span class="sxs-lookup"><span data-stu-id="6da40-441">✔</span></span> | <span data-ttu-id="6da40-442">✔</span><span class="sxs-lookup"><span data-stu-id="6da40-442">✔</span></span>
<span data-ttu-id="6da40-443">Point. Y</span><span class="sxs-lookup"><span data-stu-id="6da40-443">Point.Y</span></span> | <span data-ttu-id="6da40-444">✔</span><span class="sxs-lookup"><span data-stu-id="6da40-444">✔</span></span> | <span data-ttu-id="6da40-445">✔</span><span class="sxs-lookup"><span data-stu-id="6da40-445">✔</span></span> | <span data-ttu-id="6da40-446">✔</span><span class="sxs-lookup"><span data-stu-id="6da40-446">✔</span></span> | <span data-ttu-id="6da40-447">✔</span><span class="sxs-lookup"><span data-stu-id="6da40-447">✔</span></span>
<span data-ttu-id="6da40-448">Point. Z</span><span class="sxs-lookup"><span data-stu-id="6da40-448">Point.Z</span></span> | <span data-ttu-id="6da40-449">✔</span><span class="sxs-lookup"><span data-stu-id="6da40-449">✔</span></span> | <span data-ttu-id="6da40-450">✔</span><span class="sxs-lookup"><span data-stu-id="6da40-450">✔</span></span> | <span data-ttu-id="6da40-451">✔</span><span class="sxs-lookup"><span data-stu-id="6da40-451">✔</span></span> | <span data-ttu-id="6da40-452">✔</span><span class="sxs-lookup"><span data-stu-id="6da40-452">✔</span></span>
<span data-ttu-id="6da40-453">Polygon. Екстериорринг</span><span class="sxs-lookup"><span data-stu-id="6da40-453">Polygon.ExteriorRing</span></span> | <span data-ttu-id="6da40-454">✔</span><span class="sxs-lookup"><span data-stu-id="6da40-454">✔</span></span> | <span data-ttu-id="6da40-455">✔</span><span class="sxs-lookup"><span data-stu-id="6da40-455">✔</span></span> | <span data-ttu-id="6da40-456">✔</span><span class="sxs-lookup"><span data-stu-id="6da40-456">✔</span></span> | <span data-ttu-id="6da40-457">✔</span><span class="sxs-lookup"><span data-stu-id="6da40-457">✔</span></span>
<span data-ttu-id="6da40-458">Polygon. Жетинтериоррингн (int)</span><span class="sxs-lookup"><span data-stu-id="6da40-458">Polygon.GetInteriorRingN(int)</span></span> | <span data-ttu-id="6da40-459">✔</span><span class="sxs-lookup"><span data-stu-id="6da40-459">✔</span></span> | <span data-ttu-id="6da40-460">✔</span><span class="sxs-lookup"><span data-stu-id="6da40-460">✔</span></span> | <span data-ttu-id="6da40-461">✔</span><span class="sxs-lookup"><span data-stu-id="6da40-461">✔</span></span> | <span data-ttu-id="6da40-462">✔</span><span class="sxs-lookup"><span data-stu-id="6da40-462">✔</span></span>
<span data-ttu-id="6da40-463">Polygon. Нуминтериоррингс</span><span class="sxs-lookup"><span data-stu-id="6da40-463">Polygon.NumInteriorRings</span></span> | <span data-ttu-id="6da40-464">✔</span><span class="sxs-lookup"><span data-stu-id="6da40-464">✔</span></span> | <span data-ttu-id="6da40-465">✔</span><span class="sxs-lookup"><span data-stu-id="6da40-465">✔</span></span> | <span data-ttu-id="6da40-466">✔</span><span class="sxs-lookup"><span data-stu-id="6da40-466">✔</span></span> | <span data-ttu-id="6da40-467">✔</span><span class="sxs-lookup"><span data-stu-id="6da40-467">✔</span></span>

## <a name="additional-resources"></a><span data-ttu-id="6da40-468">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="6da40-468">Additional resources</span></span>

* [<span data-ttu-id="6da40-469">Пространственные данные в SQL Server</span><span class="sxs-lookup"><span data-stu-id="6da40-469">Spatial Data in SQL Server</span></span>](https://docs.microsoft.com/sql/relational-databases/spatial/spatial-data-sql-server)
* [<span data-ttu-id="6da40-470">Домашняя страница Спатиалите</span><span class="sxs-lookup"><span data-stu-id="6da40-470">SpatiaLite Homepage</span></span>](https://www.gaia-gis.it/fossil/libspatialite)
* [<span data-ttu-id="6da40-471">Документация по Npgsqlной пространственной документации</span><span class="sxs-lookup"><span data-stu-id="6da40-471">Npgsql Spatial Documentation</span></span>](https://www.npgsql.org/efcore/mapping/nts.html)
* [<span data-ttu-id="6da40-472">Документация по PostGIS</span><span class="sxs-lookup"><span data-stu-id="6da40-472">PostGIS Documentation</span></span>](https://postgis.net/documentation/)
