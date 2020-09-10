---
title: Пространственные данные — EF Core
description: Использование пространственных данных в модели Entity Framework Core
author: bricelam
ms.author: bricelam
ms.date: 11/01/2018
ms.assetid: 2BDE29FC-4161-41A0-841E-69F51CCD9341
uid: core/modeling/spatial
ms.openlocfilehash: 42386fb132f135d725a2068d91dc49c7f613e277
ms.sourcegitcommit: 7c3939504bb9da3f46bea3443638b808c04227c2
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/09/2020
ms.locfileid: "89616700"
---
# <a name="spatial-data"></a><span data-ttu-id="71a35-103">Пространственные данные</span><span class="sxs-lookup"><span data-stu-id="71a35-103">Spatial Data</span></span>

> [!NOTE]
> <span data-ttu-id="71a35-104">Эта функция была добавлена в EF Core 2,2.</span><span class="sxs-lookup"><span data-stu-id="71a35-104">This feature was added in EF Core 2.2.</span></span>

<span data-ttu-id="71a35-105">Пространственные данные представляют физическое расположение и форму объектов.</span><span class="sxs-lookup"><span data-stu-id="71a35-105">Spatial data represents the physical location and the shape of objects.</span></span> <span data-ttu-id="71a35-106">Многие базы данных обеспечивают поддержку этого типа данных, поэтому их можно индексировать и запрашивать вместе с другими данными.</span><span class="sxs-lookup"><span data-stu-id="71a35-106">Many databases provide support for this type of data so it can be indexed and queried alongside other data.</span></span> <span data-ttu-id="71a35-107">Типичные сценарии включают запросы объектов в заданном расстоянии от расположения или выбор объекта, граница которого содержит заданное расположение.</span><span class="sxs-lookup"><span data-stu-id="71a35-107">Common scenarios include querying for objects within a given distance from a location, or selecting the object whose border contains a given location.</span></span> <span data-ttu-id="71a35-108">EF Core поддерживает сопоставление с пространственными типами данных с помощью пространственной библиотеки [неттопологисуите](https://github.com/NetTopologySuite/NetTopologySuite) .</span><span class="sxs-lookup"><span data-stu-id="71a35-108">EF Core supports mapping to spatial data types using the [NetTopologySuite](https://github.com/NetTopologySuite/NetTopologySuite) spatial library.</span></span>

## <a name="installing"></a><span data-ttu-id="71a35-109">Установка</span><span class="sxs-lookup"><span data-stu-id="71a35-109">Installing</span></span>

<span data-ttu-id="71a35-110">Чтобы использовать пространственные данные с EF Core, необходимо установить соответствующий поддерживающий пакет NuGet.</span><span class="sxs-lookup"><span data-stu-id="71a35-110">In order to use spatial data with EF Core, you need to install the appropriate supporting NuGet package.</span></span> <span data-ttu-id="71a35-111">Пакет, который необходимо установить, зависит от поставщика, который вы используете.</span><span class="sxs-lookup"><span data-stu-id="71a35-111">Which package you need to install depends on the provider you're using.</span></span>

<span data-ttu-id="71a35-112">Поставщик EF Core</span><span class="sxs-lookup"><span data-stu-id="71a35-112">EF Core Provider</span></span>                        | <span data-ttu-id="71a35-113">Пространственный пакет NuGet</span><span class="sxs-lookup"><span data-stu-id="71a35-113">Spatial NuGet Package</span></span>
--------------------------------------- | ---------------------
<span data-ttu-id="71a35-114">Microsoft.EntityFrameworkCore.SqlServer</span><span class="sxs-lookup"><span data-stu-id="71a35-114">Microsoft.EntityFrameworkCore.SqlServer</span></span> | [<span data-ttu-id="71a35-115">Microsoft. EntityFrameworkCore. SqlServer. Неттопологисуите</span><span class="sxs-lookup"><span data-stu-id="71a35-115">Microsoft.EntityFrameworkCore.SqlServer.NetTopologySuite</span></span>](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer.NetTopologySuite)
<span data-ttu-id="71a35-116">Microsoft.EntityFrameworkCore.Sqlite</span><span class="sxs-lookup"><span data-stu-id="71a35-116">Microsoft.EntityFrameworkCore.Sqlite</span></span>    | [<span data-ttu-id="71a35-117">Microsoft. EntityFrameworkCore. SQLite. Неттопологисуите</span><span class="sxs-lookup"><span data-stu-id="71a35-117">Microsoft.EntityFrameworkCore.Sqlite.NetTopologySuite</span></span>](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Sqlite.NetTopologySuite)
<span data-ttu-id="71a35-118">Microsoft.EntityFrameworkCore.InMemory</span><span class="sxs-lookup"><span data-stu-id="71a35-118">Microsoft.EntityFrameworkCore.InMemory</span></span>  | [<span data-ttu-id="71a35-119">неттопологисуите</span><span class="sxs-lookup"><span data-stu-id="71a35-119">NetTopologySuite</span></span>](https://www.nuget.org/packages/NetTopologySuite)
<span data-ttu-id="71a35-120">Npgsql.EntityFrameworkCore.PostgreSQL</span><span class="sxs-lookup"><span data-stu-id="71a35-120">Npgsql.EntityFrameworkCore.PostgreSQL</span></span>   | [<span data-ttu-id="71a35-121">Npgsql. EntityFrameworkCore. PostgreSQL. Неттопологисуите</span><span class="sxs-lookup"><span data-stu-id="71a35-121">Npgsql.EntityFrameworkCore.PostgreSQL.NetTopologySuite</span></span>](https://www.nuget.org/packages/Npgsql.EntityFrameworkCore.PostgreSQL.NetTopologySuite)

## <a name="reverse-engineering"></a><span data-ttu-id="71a35-122">Реконструирование</span><span class="sxs-lookup"><span data-stu-id="71a35-122">Reverse engineering</span></span>

<span data-ttu-id="71a35-123">Пространственные пакеты NuGet также включают модели [реконструирования](xref:core/managing-schemas/scaffolding) с пространственными свойствами, но необходимо установить пакет ***перед*** запуском `Scaffold-DbContext` или `dotnet ef dbcontext scaffold` .</span><span class="sxs-lookup"><span data-stu-id="71a35-123">The spatial NuGet packages also enable [reverse engineering](xref:core/managing-schemas/scaffolding) models with spatial properties, but you need to install the package ***before*** running `Scaffold-DbContext` or `dotnet ef dbcontext scaffold`.</span></span> <span data-ttu-id="71a35-124">В противном случае вы получите предупреждения о том, что не удается найти сопоставления типов для столбцов, и столбцы будут пропущены.</span><span class="sxs-lookup"><span data-stu-id="71a35-124">If you don't, you'll receive warnings about not finding type mappings for the columns and the columns will be skipped.</span></span>

## <a name="nettopologysuite-nts"></a><span data-ttu-id="71a35-125">Неттопологисуите (NTS)</span><span class="sxs-lookup"><span data-stu-id="71a35-125">NetTopologySuite (NTS)</span></span>

<span data-ttu-id="71a35-126">Неттопологисуите является пространственной библиотекой для .NET.</span><span class="sxs-lookup"><span data-stu-id="71a35-126">NetTopologySuite is a spatial library for .NET.</span></span> <span data-ttu-id="71a35-127">EF Core поддерживает сопоставление с пространственными типами данных в базе данных с помощью типов NTS в модели.</span><span class="sxs-lookup"><span data-stu-id="71a35-127">EF Core enables mapping to spatial data types in the database by using NTS types in your model.</span></span>

<span data-ttu-id="71a35-128">Чтобы включить сопоставление с пространственными типами через NTS, вызовите метод Усенеттопологисуите в построителе параметров DbContext поставщика.</span><span class="sxs-lookup"><span data-stu-id="71a35-128">To enable mapping to spatial types via NTS, call the UseNetTopologySuite method on the provider's DbContext options builder.</span></span> <span data-ttu-id="71a35-129">Например, с SQL Server можно назвать его следующим образом.</span><span class="sxs-lookup"><span data-stu-id="71a35-129">For example, with SQL Server you'd call it like this.</span></span>

``` csharp
optionsBuilder.UseSqlServer(
    @"Data Source=(localdb)\MSSQLLocalDB;Initial Catalog=WideWorldImporters",
    x => x.UseNetTopologySuite());
```

<span data-ttu-id="71a35-130">Существует несколько типов пространственных данных.</span><span class="sxs-lookup"><span data-stu-id="71a35-130">There are several spatial data types.</span></span> <span data-ttu-id="71a35-131">Используемый тип зависит от типов фигур, которые вы хотите разрешить.</span><span class="sxs-lookup"><span data-stu-id="71a35-131">Which type you use depends on the types of shapes you want to allow.</span></span> <span data-ttu-id="71a35-132">Ниже приведена иерархия типов NTS, которые можно использовать для свойств в модели.</span><span class="sxs-lookup"><span data-stu-id="71a35-132">Here is the hierarchy of NTS types that you can use for properties in your model.</span></span> <span data-ttu-id="71a35-133">Они находятся в пределах `NetTopologySuite.Geometries` пространства имен.</span><span class="sxs-lookup"><span data-stu-id="71a35-133">They're located within the `NetTopologySuite.Geometries` namespace.</span></span>

* <span data-ttu-id="71a35-134">Геометрия</span><span class="sxs-lookup"><span data-stu-id="71a35-134">Geometry</span></span>
  * <span data-ttu-id="71a35-135">Точка</span><span class="sxs-lookup"><span data-stu-id="71a35-135">Point</span></span>
  * <span data-ttu-id="71a35-136">LineString</span><span class="sxs-lookup"><span data-stu-id="71a35-136">LineString</span></span>
  * <span data-ttu-id="71a35-137">Многоугольник</span><span class="sxs-lookup"><span data-stu-id="71a35-137">Polygon</span></span>
  * <span data-ttu-id="71a35-138">GeometryCollection</span><span class="sxs-lookup"><span data-stu-id="71a35-138">GeometryCollection</span></span>
    * <span data-ttu-id="71a35-139">MultiPoint</span><span class="sxs-lookup"><span data-stu-id="71a35-139">MultiPoint</span></span>
    * <span data-ttu-id="71a35-140">MultiLineString</span><span class="sxs-lookup"><span data-stu-id="71a35-140">MultiLineString</span></span>
    * <span data-ttu-id="71a35-141">MultiPolygon</span><span class="sxs-lookup"><span data-stu-id="71a35-141">MultiPolygon</span></span>

> [!WARNING]
> <span data-ttu-id="71a35-142">CircularString, CompoundCurve и Куреполигон не поддерживаются NTS.</span><span class="sxs-lookup"><span data-stu-id="71a35-142">CircularString, CompoundCurve, and CurePolygon aren't supported by NTS.</span></span>

<span data-ttu-id="71a35-143">Использование базового типа geometry позволяет задать любой тип фигуры в свойстве.</span><span class="sxs-lookup"><span data-stu-id="71a35-143">Using the base Geometry type allows any type of shape to be specified by the property.</span></span>

<span data-ttu-id="71a35-144">Следующие классы сущностей можно использовать для соотнесения с таблицами в [образце базы данных Wide World импортеров](https://go.microsoft.com/fwlink/?LinkID=800630).</span><span class="sxs-lookup"><span data-stu-id="71a35-144">The following entity classes could be used to map to tables in the [Wide World Importers sample database](https://go.microsoft.com/fwlink/?LinkID=800630).</span></span>

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

### <a name="creating-values"></a><span data-ttu-id="71a35-145">Создание значений</span><span class="sxs-lookup"><span data-stu-id="71a35-145">Creating values</span></span>

<span data-ttu-id="71a35-146">Для создания объектов Geometry можно использовать конструкторы. Однако NTS рекомендует использовать вместо нее геометрическую фабрику.</span><span class="sxs-lookup"><span data-stu-id="71a35-146">You can use constructors to create geometry objects; however, NTS recommends using a geometry factory instead.</span></span> <span data-ttu-id="71a35-147">Это позволяет указать SRID по умолчанию (систему пространственной ссылки, используемую координатами) и позволяет управлять более сложными возможностями, такими как модель точности (используемая во время вычислений), и последовательность координат (определяет, какие координаты и меры —).</span><span class="sxs-lookup"><span data-stu-id="71a35-147">This lets you specify a default SRID (the spatial reference system used by the coordinates) and gives you control over more advanced things like the precision model (used during calculations) and the coordinate sequence (determines which ordinates--dimensions and measures--are available).</span></span>

``` csharp
var geometryFactory = NtsGeometryServices.Instance.CreateGeometryFactory(srid: 4326);
var currentLocation = geometryFactory.CreatePoint(new Coordinate(-122.121512, 47.6739882));
```

> [!NOTE]
> <span data-ttu-id="71a35-148">4326 относится к WGS 84, стандарту, используемому в GPS и других географических системах.</span><span class="sxs-lookup"><span data-stu-id="71a35-148">4326 refers to WGS 84, a standard used in GPS and other geographic systems.</span></span>

### <a name="longitude-and-latitude"></a><span data-ttu-id="71a35-149">Долгота и Широта</span><span class="sxs-lookup"><span data-stu-id="71a35-149">Longitude and Latitude</span></span>

<span data-ttu-id="71a35-150">Координаты в NTS находятся в терминах значений X и Y.</span><span class="sxs-lookup"><span data-stu-id="71a35-150">Coordinates in NTS are in terms of X and Y values.</span></span> <span data-ttu-id="71a35-151">Чтобы представить долготу и широту, используйте X для долготы и Y для широты.</span><span class="sxs-lookup"><span data-stu-id="71a35-151">To represent longitude and latitude, use X for longitude and Y for latitude.</span></span> <span data-ttu-id="71a35-152">Обратите внимание, что это происходит **обратно** из `latitude, longitude` формата, в котором обычно отображаются эти значения.</span><span class="sxs-lookup"><span data-stu-id="71a35-152">Note that this is **backwards** from the `latitude, longitude` format in which you typically see these values.</span></span>

### <a name="srid-ignored-during-client-operations"></a><span data-ttu-id="71a35-153">SRID игнорируется во время операций клиента</span><span class="sxs-lookup"><span data-stu-id="71a35-153">SRID Ignored during client operations</span></span>

<span data-ttu-id="71a35-154">NTS игнорирует значения SRID во время операций.</span><span class="sxs-lookup"><span data-stu-id="71a35-154">NTS ignores SRID values during operations.</span></span> <span data-ttu-id="71a35-155">Предполагается, что это плоская система координат.</span><span class="sxs-lookup"><span data-stu-id="71a35-155">It assumes a planar coordinate system.</span></span> <span data-ttu-id="71a35-156">Это означает, что если указать координаты в виде долготы и широты, то некоторые значения, вычисляемые клиентом, такие как расстояние, Длина и площадь, будут в градусах, а не в метрах.</span><span class="sxs-lookup"><span data-stu-id="71a35-156">This means that if you specify coordinates in terms of longitude and latitude, some client-evaluated values like distance, length, and area will be in degrees, not meters.</span></span> <span data-ttu-id="71a35-157">Чтобы получить более значимые значения, сначала необходимо проецировать координаты в другую систему координат, используя библиотеку, например [ProjNet4GeoAPI](https://github.com/NetTopologySuite/ProjNet4GeoAPI) , перед вычислением этих значений.</span><span class="sxs-lookup"><span data-stu-id="71a35-157">For more meaningful values, you first need to project the coordinates to another coordinate system using a library like [ProjNet4GeoAPI](https://github.com/NetTopologySuite/ProjNet4GeoAPI) before calculating these values.</span></span>

<span data-ttu-id="71a35-158">Если операция оценивается сервером с помощью EF Core с помощью SQL, то единица результата будет определяться базой данных.</span><span class="sxs-lookup"><span data-stu-id="71a35-158">If an operation is server-evaluated by EF Core via SQL, the result's unit will be determined by the database.</span></span>

<span data-ttu-id="71a35-159">Ниже приведен пример использования ProjNet4GeoAPI для вычисления расстояния между двумя городами.</span><span class="sxs-lookup"><span data-stu-id="71a35-159">Here is an example of using ProjNet4GeoAPI to calculate the distance between two cities.</span></span>

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

## <a name="querying-data"></a><span data-ttu-id="71a35-160">Запросы к данным</span><span class="sxs-lookup"><span data-stu-id="71a35-160">Querying Data</span></span>

<span data-ttu-id="71a35-161">В LINQ методы и свойства NTS, доступные в качестве функций базы данных, будут преобразованы в SQL.</span><span class="sxs-lookup"><span data-stu-id="71a35-161">In LINQ, the NTS methods and properties available as database functions will be translated to SQL.</span></span> <span data-ttu-id="71a35-162">Например, методы Distance и Contains преобразуются в следующие запросы.</span><span class="sxs-lookup"><span data-stu-id="71a35-162">For example, the Distance and Contains methods are translated in the following queries.</span></span> <span data-ttu-id="71a35-163">В таблице в конце этой статьи показано, какие элементы поддерживаются различными поставщиками EF Core.</span><span class="sxs-lookup"><span data-stu-id="71a35-163">The table at the end of this article shows which members are supported by various EF Core providers.</span></span>

``` csharp
var nearestCity = db.Cities
    .OrderBy(c => c.Location.Distance(currentLocation))
    .FirstOrDefault();

var currentCountry = db.Countries
    .FirstOrDefault(c => c.Border.Contains(currentLocation));
```

## <a name="sql-server"></a><span data-ttu-id="71a35-164">SQL Server</span><span class="sxs-lookup"><span data-stu-id="71a35-164">SQL Server</span></span>

<span data-ttu-id="71a35-165">Если вы используете SQL Server, необходимо учитывать некоторые дополнительные моменты.</span><span class="sxs-lookup"><span data-stu-id="71a35-165">If you're using SQL Server, there are some additional things you should be aware of.</span></span>

### <a name="geography-or-geometry"></a><span data-ttu-id="71a35-166">География или геометрические объекты</span><span class="sxs-lookup"><span data-stu-id="71a35-166">Geography or geometry</span></span>

<span data-ttu-id="71a35-167">По умолчанию пространственные свойства сопоставляются со `geography` столбцами в SQL Server.</span><span class="sxs-lookup"><span data-stu-id="71a35-167">By default, spatial properties are mapped to `geography` columns in SQL Server.</span></span> <span data-ttu-id="71a35-168">Чтобы использовать `geometry` , [Настройте тип столбца](xref:core/modeling/entity-properties#column-data-types) в модели.</span><span class="sxs-lookup"><span data-stu-id="71a35-168">To use `geometry`, [configure the column type](xref:core/modeling/entity-properties#column-data-types) in your model.</span></span>

### <a name="geography-polygon-rings"></a><span data-ttu-id="71a35-169">Кольца географических многоугольников</span><span class="sxs-lookup"><span data-stu-id="71a35-169">Geography polygon rings</span></span>

<span data-ttu-id="71a35-170">При использовании `geography` типа столбца SQL Server накладывает дополнительные требования на внешнее кольцо (или оболочку) и внутренние кольца (или отверстия).</span><span class="sxs-lookup"><span data-stu-id="71a35-170">When using the `geography` column type, SQL Server imposes additional requirements on the exterior ring (or shell) and interior rings (or holes).</span></span> <span data-ttu-id="71a35-171">Внешнее кольцо должно быть ориентировано против часовой стрелки и внутренних колец по часовой стрелке.</span><span class="sxs-lookup"><span data-stu-id="71a35-171">The exterior ring must be oriented counterclockwise and the interior rings clockwise.</span></span> <span data-ttu-id="71a35-172">NTS проверяет это перед отправкой значений в базу данных.</span><span class="sxs-lookup"><span data-stu-id="71a35-172">NTS validates this before sending values to the database.</span></span>

### <a name="fullglobe"></a><span data-ttu-id="71a35-173">FullGlobe</span><span class="sxs-lookup"><span data-stu-id="71a35-173">FullGlobe</span></span>

<span data-ttu-id="71a35-174">SQL Server имеет нестандартный тип геометрии для представления полного земного шара при использовании `geography` типа столбца.</span><span class="sxs-lookup"><span data-stu-id="71a35-174">SQL Server has a non-standard geometry type to represent the full globe when using the `geography` column type.</span></span> <span data-ttu-id="71a35-175">У него также есть способ представления многоугольников на основе полного земного шара (без внешнего кольца).</span><span class="sxs-lookup"><span data-stu-id="71a35-175">It also has a way to represent polygons based on the full globe (without an exterior ring).</span></span> <span data-ttu-id="71a35-176">Ни один из этих элементов не поддерживается NTS.</span><span class="sxs-lookup"><span data-stu-id="71a35-176">Neither of these are supported by NTS.</span></span>

> [!WARNING]
> <span data-ttu-id="71a35-177">FullGlobe и многоугольники, основанные на нем, не поддерживаются NTS.</span><span class="sxs-lookup"><span data-stu-id="71a35-177">FullGlobe and polygons based on it aren't supported by NTS.</span></span>

## <a name="sqlite"></a><span data-ttu-id="71a35-178">SQLite</span><span class="sxs-lookup"><span data-stu-id="71a35-178">SQLite</span></span>

<span data-ttu-id="71a35-179">Ниже приведена дополнительная информация для тех, кто использует SQLite.</span><span class="sxs-lookup"><span data-stu-id="71a35-179">Here is some additional information for those using SQLite.</span></span>

### <a name="installing-spatialite"></a><span data-ttu-id="71a35-180">Установка Спатиалите</span><span class="sxs-lookup"><span data-stu-id="71a35-180">Installing SpatiaLite</span></span>

<span data-ttu-id="71a35-181">В Windows собственная библиотека mod_spatialite распространяется как зависимость пакета NuGet.</span><span class="sxs-lookup"><span data-stu-id="71a35-181">On Windows, the native mod_spatialite library is distributed as a NuGet package dependency.</span></span> <span data-ttu-id="71a35-182">Другие платформы должны установить его отдельно.</span><span class="sxs-lookup"><span data-stu-id="71a35-182">Other platforms need to install it separately.</span></span> <span data-ttu-id="71a35-183">Обычно это делается с помощью диспетчера пакетов программного обеспечения.</span><span class="sxs-lookup"><span data-stu-id="71a35-183">This is typically done using a software package manager.</span></span> <span data-ttu-id="71a35-184">Например, можно использовать APT для Ubuntu и Homebrew на MacOS.</span><span class="sxs-lookup"><span data-stu-id="71a35-184">For example, you can use APT on Ubuntu and Homebrew on MacOS.</span></span>

``` sh
# Ubuntu
apt-get install libsqlite3-mod-spatialite

# macOS
brew install libspatialite
```

<span data-ttu-id="71a35-185">К сожалению, новые версии PROJ (зависимость от Спатиалите) несовместимы с [набором склитепклрав](/dotnet/standard/data/sqlite/custom-versions#bundles)по умолчанию EF.</span><span class="sxs-lookup"><span data-stu-id="71a35-185">Unfortunately, newer versions of PROJ (a dependency of SpatiaLite) are incompatible with EF's default [SQLitePCLRaw bundle](/dotnet/standard/data/sqlite/custom-versions#bundles).</span></span> <span data-ttu-id="71a35-186">Это можно обойти, создав пользовательский [поставщик склитепклрав](/dotnet/standard/data/sqlite/custom-versions#sqlitepclraw-providers) , использующий библиотеку System SQLite, или можно установить пользовательскую сборку, отключив поддержку proj для спатиалите.</span><span class="sxs-lookup"><span data-stu-id="71a35-186">You can work around this by either creating a custom [SQLitePCLRaw provider](/dotnet/standard/data/sqlite/custom-versions#sqlitepclraw-providers) that uses the system SQLite library, or you can install a custom build of SpatiaLite disabling PROJ support.</span></span>

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

### <a name="configuring-srid"></a><span data-ttu-id="71a35-187">Настройка SRID</span><span class="sxs-lookup"><span data-stu-id="71a35-187">Configuring SRID</span></span>

<span data-ttu-id="71a35-188">В Спатиалите столбцам необходимо указать SRID для каждого столбца.</span><span class="sxs-lookup"><span data-stu-id="71a35-188">In SpatiaLite, columns need to specify an SRID per column.</span></span> <span data-ttu-id="71a35-189">По умолчанию SRID имеет значение `0` .</span><span class="sxs-lookup"><span data-stu-id="71a35-189">The default SRID is `0`.</span></span> <span data-ttu-id="71a35-190">Укажите другой SRID с помощью метода Форсклитехассрид.</span><span class="sxs-lookup"><span data-stu-id="71a35-190">Specify a different SRID using the ForSqliteHasSrid method.</span></span>

``` csharp
modelBuilder.Entity<City>().Property(c => c.Location)
    .ForSqliteHasSrid(4326);
```

### <a name="dimension"></a><span data-ttu-id="71a35-191">Измерение</span><span class="sxs-lookup"><span data-stu-id="71a35-191">Dimension</span></span>

<span data-ttu-id="71a35-192">Как и в случае с SRID, измерение столбца (или координата) также указывается как часть столбца.</span><span class="sxs-lookup"><span data-stu-id="71a35-192">Similar to SRID, a column's dimension (or ordinates) is also specified as part of the column.</span></span> <span data-ttu-id="71a35-193">По умолчанию используются координаты X и Y. Включите дополнительные координаты (Z и M) с помощью метода Форсклитехасдименсион.</span><span class="sxs-lookup"><span data-stu-id="71a35-193">The default ordinates are X and Y. Enable additional ordinates (Z and M) using the ForSqliteHasDimension method.</span></span>

``` csharp
modelBuilder.Entity<City>().Property(c => c.Location)
    .ForSqliteHasDimension(Ordinates.XYZ);
```

## <a name="translated-operations"></a><span data-ttu-id="71a35-194">Переведенные операции</span><span class="sxs-lookup"><span data-stu-id="71a35-194">Translated Operations</span></span>

<span data-ttu-id="71a35-195">В этой таблице показано, какие элементы NTS преобразуются в SQL каждым поставщиком EF Core.</span><span class="sxs-lookup"><span data-stu-id="71a35-195">This table shows which NTS members are translated into SQL by each EF Core provider.</span></span>

<span data-ttu-id="71a35-196">неттопологисуите</span><span class="sxs-lookup"><span data-stu-id="71a35-196">NetTopologySuite</span></span> | <span data-ttu-id="71a35-197">SQL Server (геометрия)</span><span class="sxs-lookup"><span data-stu-id="71a35-197">SQL Server (geometry)</span></span> | <span data-ttu-id="71a35-198">SQL Server (география)</span><span class="sxs-lookup"><span data-stu-id="71a35-198">SQL Server (geography)</span></span> | <span data-ttu-id="71a35-199">SQLite</span><span class="sxs-lookup"><span data-stu-id="71a35-199">SQLite</span></span> | <span data-ttu-id="71a35-200">Npgsql</span><span class="sxs-lookup"><span data-stu-id="71a35-200">Npgsql</span></span>
--- |:---:|:---:|:---:|:---:
<span data-ttu-id="71a35-201">Геометрия. область</span><span class="sxs-lookup"><span data-stu-id="71a35-201">Geometry.Area</span></span> | <span data-ttu-id="71a35-202">✔</span><span class="sxs-lookup"><span data-stu-id="71a35-202">✔</span></span> | <span data-ttu-id="71a35-203">✔</span><span class="sxs-lookup"><span data-stu-id="71a35-203">✔</span></span> | <span data-ttu-id="71a35-204">✔</span><span class="sxs-lookup"><span data-stu-id="71a35-204">✔</span></span> | <span data-ttu-id="71a35-205">✔</span><span class="sxs-lookup"><span data-stu-id="71a35-205">✔</span></span>
<span data-ttu-id="71a35-206">Geometry. Асбинари ()</span><span class="sxs-lookup"><span data-stu-id="71a35-206">Geometry.AsBinary()</span></span> | <span data-ttu-id="71a35-207">✔</span><span class="sxs-lookup"><span data-stu-id="71a35-207">✔</span></span> | <span data-ttu-id="71a35-208">✔</span><span class="sxs-lookup"><span data-stu-id="71a35-208">✔</span></span> | <span data-ttu-id="71a35-209">✔</span><span class="sxs-lookup"><span data-stu-id="71a35-209">✔</span></span> | <span data-ttu-id="71a35-210">✔</span><span class="sxs-lookup"><span data-stu-id="71a35-210">✔</span></span>
<span data-ttu-id="71a35-211">Geometry. Астекст ()</span><span class="sxs-lookup"><span data-stu-id="71a35-211">Geometry.AsText()</span></span> | <span data-ttu-id="71a35-212">✔</span><span class="sxs-lookup"><span data-stu-id="71a35-212">✔</span></span> | <span data-ttu-id="71a35-213">✔</span><span class="sxs-lookup"><span data-stu-id="71a35-213">✔</span></span> | <span data-ttu-id="71a35-214">✔</span><span class="sxs-lookup"><span data-stu-id="71a35-214">✔</span></span> | <span data-ttu-id="71a35-215">✔</span><span class="sxs-lookup"><span data-stu-id="71a35-215">✔</span></span>
<span data-ttu-id="71a35-216">Geometry. Граничный</span><span class="sxs-lookup"><span data-stu-id="71a35-216">Geometry.Boundary</span></span> | <span data-ttu-id="71a35-217">✔</span><span class="sxs-lookup"><span data-stu-id="71a35-217">✔</span></span> | | <span data-ttu-id="71a35-218">✔</span><span class="sxs-lookup"><span data-stu-id="71a35-218">✔</span></span> | <span data-ttu-id="71a35-219">✔</span><span class="sxs-lookup"><span data-stu-id="71a35-219">✔</span></span>
<span data-ttu-id="71a35-220">Геометрия. buffer (двойное)</span><span class="sxs-lookup"><span data-stu-id="71a35-220">Geometry.Buffer(double)</span></span> | <span data-ttu-id="71a35-221">✔</span><span class="sxs-lookup"><span data-stu-id="71a35-221">✔</span></span> | <span data-ttu-id="71a35-222">✔</span><span class="sxs-lookup"><span data-stu-id="71a35-222">✔</span></span> | <span data-ttu-id="71a35-223">✔</span><span class="sxs-lookup"><span data-stu-id="71a35-223">✔</span></span> | <span data-ttu-id="71a35-224">✔</span><span class="sxs-lookup"><span data-stu-id="71a35-224">✔</span></span>
<span data-ttu-id="71a35-225">Геометрия. buffer (Double, int)</span><span class="sxs-lookup"><span data-stu-id="71a35-225">Geometry.Buffer(double, int)</span></span> | | | <span data-ttu-id="71a35-226">✔</span><span class="sxs-lookup"><span data-stu-id="71a35-226">✔</span></span> | <span data-ttu-id="71a35-227">✔</span><span class="sxs-lookup"><span data-stu-id="71a35-227">✔</span></span>
<span data-ttu-id="71a35-228">Geometry. центроид</span><span class="sxs-lookup"><span data-stu-id="71a35-228">Geometry.Centroid</span></span> | <span data-ttu-id="71a35-229">✔</span><span class="sxs-lookup"><span data-stu-id="71a35-229">✔</span></span> | | <span data-ttu-id="71a35-230">✔</span><span class="sxs-lookup"><span data-stu-id="71a35-230">✔</span></span> | <span data-ttu-id="71a35-231">✔</span><span class="sxs-lookup"><span data-stu-id="71a35-231">✔</span></span>
<span data-ttu-id="71a35-232">Геометрия. Contains (геометрия)</span><span class="sxs-lookup"><span data-stu-id="71a35-232">Geometry.Contains(Geometry)</span></span> | <span data-ttu-id="71a35-233">✔</span><span class="sxs-lookup"><span data-stu-id="71a35-233">✔</span></span> | <span data-ttu-id="71a35-234">✔</span><span class="sxs-lookup"><span data-stu-id="71a35-234">✔</span></span> | <span data-ttu-id="71a35-235">✔</span><span class="sxs-lookup"><span data-stu-id="71a35-235">✔</span></span> | <span data-ttu-id="71a35-236">✔</span><span class="sxs-lookup"><span data-stu-id="71a35-236">✔</span></span>
<span data-ttu-id="71a35-237">Geometry. Конвексхулл ()</span><span class="sxs-lookup"><span data-stu-id="71a35-237">Geometry.ConvexHull()</span></span> | <span data-ttu-id="71a35-238">✔</span><span class="sxs-lookup"><span data-stu-id="71a35-238">✔</span></span> | <span data-ttu-id="71a35-239">✔</span><span class="sxs-lookup"><span data-stu-id="71a35-239">✔</span></span> | <span data-ttu-id="71a35-240">✔</span><span class="sxs-lookup"><span data-stu-id="71a35-240">✔</span></span> | <span data-ttu-id="71a35-241">✔</span><span class="sxs-lookup"><span data-stu-id="71a35-241">✔</span></span>
<span data-ttu-id="71a35-242">Geometry. Ковередби (геометрия)</span><span class="sxs-lookup"><span data-stu-id="71a35-242">Geometry.CoveredBy(Geometry)</span></span> | | | <span data-ttu-id="71a35-243">✔</span><span class="sxs-lookup"><span data-stu-id="71a35-243">✔</span></span> | <span data-ttu-id="71a35-244">✔</span><span class="sxs-lookup"><span data-stu-id="71a35-244">✔</span></span>
<span data-ttu-id="71a35-245">Geometry. крышки (Geometry)</span><span class="sxs-lookup"><span data-stu-id="71a35-245">Geometry.Covers(Geometry)</span></span> | | | <span data-ttu-id="71a35-246">✔</span><span class="sxs-lookup"><span data-stu-id="71a35-246">✔</span></span> | <span data-ttu-id="71a35-247">✔</span><span class="sxs-lookup"><span data-stu-id="71a35-247">✔</span></span>
<span data-ttu-id="71a35-248">Геометрия. перекрестий (Geometry)</span><span class="sxs-lookup"><span data-stu-id="71a35-248">Geometry.Crosses(Geometry)</span></span> | <span data-ttu-id="71a35-249">✔</span><span class="sxs-lookup"><span data-stu-id="71a35-249">✔</span></span> | | <span data-ttu-id="71a35-250">✔</span><span class="sxs-lookup"><span data-stu-id="71a35-250">✔</span></span> | <span data-ttu-id="71a35-251">✔</span><span class="sxs-lookup"><span data-stu-id="71a35-251">✔</span></span>
<span data-ttu-id="71a35-252">Геометрия. Difference (геометрия)</span><span class="sxs-lookup"><span data-stu-id="71a35-252">Geometry.Difference(Geometry)</span></span> | <span data-ttu-id="71a35-253">✔</span><span class="sxs-lookup"><span data-stu-id="71a35-253">✔</span></span> | <span data-ttu-id="71a35-254">✔</span><span class="sxs-lookup"><span data-stu-id="71a35-254">✔</span></span> | <span data-ttu-id="71a35-255">✔</span><span class="sxs-lookup"><span data-stu-id="71a35-255">✔</span></span> | <span data-ttu-id="71a35-256">✔</span><span class="sxs-lookup"><span data-stu-id="71a35-256">✔</span></span>
<span data-ttu-id="71a35-257">Geometry. Dimension</span><span class="sxs-lookup"><span data-stu-id="71a35-257">Geometry.Dimension</span></span> | <span data-ttu-id="71a35-258">✔</span><span class="sxs-lookup"><span data-stu-id="71a35-258">✔</span></span> | <span data-ttu-id="71a35-259">✔</span><span class="sxs-lookup"><span data-stu-id="71a35-259">✔</span></span> | <span data-ttu-id="71a35-260">✔</span><span class="sxs-lookup"><span data-stu-id="71a35-260">✔</span></span> | <span data-ttu-id="71a35-261">✔</span><span class="sxs-lookup"><span data-stu-id="71a35-261">✔</span></span>
<span data-ttu-id="71a35-262">Geometry. unjoin (геометрия)</span><span class="sxs-lookup"><span data-stu-id="71a35-262">Geometry.Disjoint(Geometry)</span></span> | <span data-ttu-id="71a35-263">✔</span><span class="sxs-lookup"><span data-stu-id="71a35-263">✔</span></span> | <span data-ttu-id="71a35-264">✔</span><span class="sxs-lookup"><span data-stu-id="71a35-264">✔</span></span> | <span data-ttu-id="71a35-265">✔</span><span class="sxs-lookup"><span data-stu-id="71a35-265">✔</span></span> | <span data-ttu-id="71a35-266">✔</span><span class="sxs-lookup"><span data-stu-id="71a35-266">✔</span></span>
<span data-ttu-id="71a35-267">Геометрия. Distance (геометрия)</span><span class="sxs-lookup"><span data-stu-id="71a35-267">Geometry.Distance(Geometry)</span></span> | <span data-ttu-id="71a35-268">✔</span><span class="sxs-lookup"><span data-stu-id="71a35-268">✔</span></span> | <span data-ttu-id="71a35-269">✔</span><span class="sxs-lookup"><span data-stu-id="71a35-269">✔</span></span> | <span data-ttu-id="71a35-270">✔</span><span class="sxs-lookup"><span data-stu-id="71a35-270">✔</span></span> | <span data-ttu-id="71a35-271">✔</span><span class="sxs-lookup"><span data-stu-id="71a35-271">✔</span></span>
<span data-ttu-id="71a35-272">Геометрия. конверт</span><span class="sxs-lookup"><span data-stu-id="71a35-272">Geometry.Envelope</span></span> | <span data-ttu-id="71a35-273">✔</span><span class="sxs-lookup"><span data-stu-id="71a35-273">✔</span></span> | | <span data-ttu-id="71a35-274">✔</span><span class="sxs-lookup"><span data-stu-id="71a35-274">✔</span></span> | <span data-ttu-id="71a35-275">✔</span><span class="sxs-lookup"><span data-stu-id="71a35-275">✔</span></span>
<span data-ttu-id="71a35-276">Geometry. Екуалсексакт (геометрия)</span><span class="sxs-lookup"><span data-stu-id="71a35-276">Geometry.EqualsExact(Geometry)</span></span> | | | | <span data-ttu-id="71a35-277">✔</span><span class="sxs-lookup"><span data-stu-id="71a35-277">✔</span></span>
<span data-ttu-id="71a35-278">Geometry. Екуалстопологикалли (геометрия)</span><span class="sxs-lookup"><span data-stu-id="71a35-278">Geometry.EqualsTopologically(Geometry)</span></span> | <span data-ttu-id="71a35-279">✔</span><span class="sxs-lookup"><span data-stu-id="71a35-279">✔</span></span> | <span data-ttu-id="71a35-280">✔</span><span class="sxs-lookup"><span data-stu-id="71a35-280">✔</span></span> | <span data-ttu-id="71a35-281">✔</span><span class="sxs-lookup"><span data-stu-id="71a35-281">✔</span></span> | <span data-ttu-id="71a35-282">✔</span><span class="sxs-lookup"><span data-stu-id="71a35-282">✔</span></span>
<span data-ttu-id="71a35-283">Geometry. Жеометритипе</span><span class="sxs-lookup"><span data-stu-id="71a35-283">Geometry.GeometryType</span></span> | <span data-ttu-id="71a35-284">✔</span><span class="sxs-lookup"><span data-stu-id="71a35-284">✔</span></span> | <span data-ttu-id="71a35-285">✔</span><span class="sxs-lookup"><span data-stu-id="71a35-285">✔</span></span> | <span data-ttu-id="71a35-286">✔</span><span class="sxs-lookup"><span data-stu-id="71a35-286">✔</span></span> | <span data-ttu-id="71a35-287">✔</span><span class="sxs-lookup"><span data-stu-id="71a35-287">✔</span></span>
<span data-ttu-id="71a35-288">Geometry. Жетжеометрин (int)</span><span class="sxs-lookup"><span data-stu-id="71a35-288">Geometry.GetGeometryN(int)</span></span> | <span data-ttu-id="71a35-289">✔</span><span class="sxs-lookup"><span data-stu-id="71a35-289">✔</span></span> | | <span data-ttu-id="71a35-290">✔</span><span class="sxs-lookup"><span data-stu-id="71a35-290">✔</span></span> | <span data-ttu-id="71a35-291">✔</span><span class="sxs-lookup"><span data-stu-id="71a35-291">✔</span></span>
<span data-ttu-id="71a35-292">Geometry. Интериорпоинт</span><span class="sxs-lookup"><span data-stu-id="71a35-292">Geometry.InteriorPoint</span></span> | <span data-ttu-id="71a35-293">✔</span><span class="sxs-lookup"><span data-stu-id="71a35-293">✔</span></span> | | <span data-ttu-id="71a35-294">✔</span><span class="sxs-lookup"><span data-stu-id="71a35-294">✔</span></span> | <span data-ttu-id="71a35-295">✔</span><span class="sxs-lookup"><span data-stu-id="71a35-295">✔</span></span>
<span data-ttu-id="71a35-296">Geometry. пересечение (Geometry)</span><span class="sxs-lookup"><span data-stu-id="71a35-296">Geometry.Intersection(Geometry)</span></span> | <span data-ttu-id="71a35-297">✔</span><span class="sxs-lookup"><span data-stu-id="71a35-297">✔</span></span> | <span data-ttu-id="71a35-298">✔</span><span class="sxs-lookup"><span data-stu-id="71a35-298">✔</span></span> | <span data-ttu-id="71a35-299">✔</span><span class="sxs-lookup"><span data-stu-id="71a35-299">✔</span></span> | <span data-ttu-id="71a35-300">✔</span><span class="sxs-lookup"><span data-stu-id="71a35-300">✔</span></span>
<span data-ttu-id="71a35-301">Геометрию. intersects (геометрия)</span><span class="sxs-lookup"><span data-stu-id="71a35-301">Geometry.Intersects(Geometry)</span></span> | <span data-ttu-id="71a35-302">✔</span><span class="sxs-lookup"><span data-stu-id="71a35-302">✔</span></span> | <span data-ttu-id="71a35-303">✔</span><span class="sxs-lookup"><span data-stu-id="71a35-303">✔</span></span> | <span data-ttu-id="71a35-304">✔</span><span class="sxs-lookup"><span data-stu-id="71a35-304">✔</span></span> | <span data-ttu-id="71a35-305">✔</span><span class="sxs-lookup"><span data-stu-id="71a35-305">✔</span></span>
<span data-ttu-id="71a35-306">Geometry. isEmpty</span><span class="sxs-lookup"><span data-stu-id="71a35-306">Geometry.IsEmpty</span></span> | <span data-ttu-id="71a35-307">✔</span><span class="sxs-lookup"><span data-stu-id="71a35-307">✔</span></span> | <span data-ttu-id="71a35-308">✔</span><span class="sxs-lookup"><span data-stu-id="71a35-308">✔</span></span> | <span data-ttu-id="71a35-309">✔</span><span class="sxs-lookup"><span data-stu-id="71a35-309">✔</span></span> | <span data-ttu-id="71a35-310">✔</span><span class="sxs-lookup"><span data-stu-id="71a35-310">✔</span></span>
<span data-ttu-id="71a35-311">Geometry. Простое_имя</span><span class="sxs-lookup"><span data-stu-id="71a35-311">Geometry.IsSimple</span></span> | <span data-ttu-id="71a35-312">✔</span><span class="sxs-lookup"><span data-stu-id="71a35-312">✔</span></span> | | <span data-ttu-id="71a35-313">✔</span><span class="sxs-lookup"><span data-stu-id="71a35-313">✔</span></span> | <span data-ttu-id="71a35-314">✔</span><span class="sxs-lookup"><span data-stu-id="71a35-314">✔</span></span>
<span data-ttu-id="71a35-315">Geometry. IsValid</span><span class="sxs-lookup"><span data-stu-id="71a35-315">Geometry.IsValid</span></span> | <span data-ttu-id="71a35-316">✔</span><span class="sxs-lookup"><span data-stu-id="71a35-316">✔</span></span> | <span data-ttu-id="71a35-317">✔</span><span class="sxs-lookup"><span data-stu-id="71a35-317">✔</span></span> | <span data-ttu-id="71a35-318">✔</span><span class="sxs-lookup"><span data-stu-id="71a35-318">✔</span></span> | <span data-ttu-id="71a35-319">✔</span><span class="sxs-lookup"><span data-stu-id="71a35-319">✔</span></span>
<span data-ttu-id="71a35-320">Geometry. Исвисиндистанце (геометрия, Double)</span><span class="sxs-lookup"><span data-stu-id="71a35-320">Geometry.IsWithinDistance(Geometry, double)</span></span> | <span data-ttu-id="71a35-321">✔</span><span class="sxs-lookup"><span data-stu-id="71a35-321">✔</span></span> | | <span data-ttu-id="71a35-322">✔</span><span class="sxs-lookup"><span data-stu-id="71a35-322">✔</span></span> | <span data-ttu-id="71a35-323">✔</span><span class="sxs-lookup"><span data-stu-id="71a35-323">✔</span></span>
<span data-ttu-id="71a35-324">Геометрия геометрии. length</span><span class="sxs-lookup"><span data-stu-id="71a35-324">Geometry.Length</span></span> | <span data-ttu-id="71a35-325">✔</span><span class="sxs-lookup"><span data-stu-id="71a35-325">✔</span></span> | <span data-ttu-id="71a35-326">✔</span><span class="sxs-lookup"><span data-stu-id="71a35-326">✔</span></span> | <span data-ttu-id="71a35-327">✔</span><span class="sxs-lookup"><span data-stu-id="71a35-327">✔</span></span> | <span data-ttu-id="71a35-328">✔</span><span class="sxs-lookup"><span data-stu-id="71a35-328">✔</span></span>
<span data-ttu-id="71a35-329">Geometry. Нумжеометриес</span><span class="sxs-lookup"><span data-stu-id="71a35-329">Geometry.NumGeometries</span></span> | <span data-ttu-id="71a35-330">✔</span><span class="sxs-lookup"><span data-stu-id="71a35-330">✔</span></span> | <span data-ttu-id="71a35-331">✔</span><span class="sxs-lookup"><span data-stu-id="71a35-331">✔</span></span> | <span data-ttu-id="71a35-332">✔</span><span class="sxs-lookup"><span data-stu-id="71a35-332">✔</span></span> | <span data-ttu-id="71a35-333">✔</span><span class="sxs-lookup"><span data-stu-id="71a35-333">✔</span></span>
<span data-ttu-id="71a35-334">Geometry. Нумпоинтс</span><span class="sxs-lookup"><span data-stu-id="71a35-334">Geometry.NumPoints</span></span> | <span data-ttu-id="71a35-335">✔</span><span class="sxs-lookup"><span data-stu-id="71a35-335">✔</span></span> | <span data-ttu-id="71a35-336">✔</span><span class="sxs-lookup"><span data-stu-id="71a35-336">✔</span></span> | <span data-ttu-id="71a35-337">✔</span><span class="sxs-lookup"><span data-stu-id="71a35-337">✔</span></span> | <span data-ttu-id="71a35-338">✔</span><span class="sxs-lookup"><span data-stu-id="71a35-338">✔</span></span>
<span data-ttu-id="71a35-339">Geometry. Огкжеометритипе</span><span class="sxs-lookup"><span data-stu-id="71a35-339">Geometry.OgcGeometryType</span></span> | <span data-ttu-id="71a35-340">✔</span><span class="sxs-lookup"><span data-stu-id="71a35-340">✔</span></span> | <span data-ttu-id="71a35-341">✔</span><span class="sxs-lookup"><span data-stu-id="71a35-341">✔</span></span> | <span data-ttu-id="71a35-342">✔</span><span class="sxs-lookup"><span data-stu-id="71a35-342">✔</span></span> | <span data-ttu-id="71a35-343">✔</span><span class="sxs-lookup"><span data-stu-id="71a35-343">✔</span></span>
<span data-ttu-id="71a35-344">Геометрия Geometry. пересекающих объектов (Geometry)</span><span class="sxs-lookup"><span data-stu-id="71a35-344">Geometry.Overlaps(Geometry)</span></span> | <span data-ttu-id="71a35-345">✔</span><span class="sxs-lookup"><span data-stu-id="71a35-345">✔</span></span> | <span data-ttu-id="71a35-346">✔</span><span class="sxs-lookup"><span data-stu-id="71a35-346">✔</span></span> | <span data-ttu-id="71a35-347">✔</span><span class="sxs-lookup"><span data-stu-id="71a35-347">✔</span></span> | <span data-ttu-id="71a35-348">✔</span><span class="sxs-lookup"><span data-stu-id="71a35-348">✔</span></span>
<span data-ttu-id="71a35-349">Geometry. Поинтонсурфаце</span><span class="sxs-lookup"><span data-stu-id="71a35-349">Geometry.PointOnSurface</span></span> | <span data-ttu-id="71a35-350">✔</span><span class="sxs-lookup"><span data-stu-id="71a35-350">✔</span></span> | | <span data-ttu-id="71a35-351">✔</span><span class="sxs-lookup"><span data-stu-id="71a35-351">✔</span></span> | <span data-ttu-id="71a35-352">✔</span><span class="sxs-lookup"><span data-stu-id="71a35-352">✔</span></span>
<span data-ttu-id="71a35-353">Geometry. Late (геометрия, строка)</span><span class="sxs-lookup"><span data-stu-id="71a35-353">Geometry.Relate(Geometry, string)</span></span> | <span data-ttu-id="71a35-354">✔</span><span class="sxs-lookup"><span data-stu-id="71a35-354">✔</span></span> | | <span data-ttu-id="71a35-355">✔</span><span class="sxs-lookup"><span data-stu-id="71a35-355">✔</span></span> | <span data-ttu-id="71a35-356">✔</span><span class="sxs-lookup"><span data-stu-id="71a35-356">✔</span></span>
<span data-ttu-id="71a35-357">Геометрия. Reverse ()</span><span class="sxs-lookup"><span data-stu-id="71a35-357">Geometry.Reverse()</span></span> | | | <span data-ttu-id="71a35-358">✔</span><span class="sxs-lookup"><span data-stu-id="71a35-358">✔</span></span> | <span data-ttu-id="71a35-359">✔</span><span class="sxs-lookup"><span data-stu-id="71a35-359">✔</span></span>
<span data-ttu-id="71a35-360">Geometry. SRID</span><span class="sxs-lookup"><span data-stu-id="71a35-360">Geometry.SRID</span></span> | <span data-ttu-id="71a35-361">✔</span><span class="sxs-lookup"><span data-stu-id="71a35-361">✔</span></span> | <span data-ttu-id="71a35-362">✔</span><span class="sxs-lookup"><span data-stu-id="71a35-362">✔</span></span> | <span data-ttu-id="71a35-363">✔</span><span class="sxs-lookup"><span data-stu-id="71a35-363">✔</span></span> | <span data-ttu-id="71a35-364">✔</span><span class="sxs-lookup"><span data-stu-id="71a35-364">✔</span></span>
<span data-ttu-id="71a35-365">Geometry. Симметрикдифференце (геометрия)</span><span class="sxs-lookup"><span data-stu-id="71a35-365">Geometry.SymmetricDifference(Geometry)</span></span> | <span data-ttu-id="71a35-366">✔</span><span class="sxs-lookup"><span data-stu-id="71a35-366">✔</span></span> | <span data-ttu-id="71a35-367">✔</span><span class="sxs-lookup"><span data-stu-id="71a35-367">✔</span></span> | <span data-ttu-id="71a35-368">✔</span><span class="sxs-lookup"><span data-stu-id="71a35-368">✔</span></span> | <span data-ttu-id="71a35-369">✔</span><span class="sxs-lookup"><span data-stu-id="71a35-369">✔</span></span>
<span data-ttu-id="71a35-370">Geometry. Тобинари ()</span><span class="sxs-lookup"><span data-stu-id="71a35-370">Geometry.ToBinary()</span></span> | <span data-ttu-id="71a35-371">✔</span><span class="sxs-lookup"><span data-stu-id="71a35-371">✔</span></span> | <span data-ttu-id="71a35-372">✔</span><span class="sxs-lookup"><span data-stu-id="71a35-372">✔</span></span> | <span data-ttu-id="71a35-373">✔</span><span class="sxs-lookup"><span data-stu-id="71a35-373">✔</span></span> | <span data-ttu-id="71a35-374">✔</span><span class="sxs-lookup"><span data-stu-id="71a35-374">✔</span></span>
<span data-ttu-id="71a35-375">Geometry. Тотекст ()</span><span class="sxs-lookup"><span data-stu-id="71a35-375">Geometry.ToText()</span></span> | <span data-ttu-id="71a35-376">✔</span><span class="sxs-lookup"><span data-stu-id="71a35-376">✔</span></span> | <span data-ttu-id="71a35-377">✔</span><span class="sxs-lookup"><span data-stu-id="71a35-377">✔</span></span> | <span data-ttu-id="71a35-378">✔</span><span class="sxs-lookup"><span data-stu-id="71a35-378">✔</span></span> | <span data-ttu-id="71a35-379">✔</span><span class="sxs-lookup"><span data-stu-id="71a35-379">✔</span></span>
<span data-ttu-id="71a35-380">Geometry. касания (геометрия)</span><span class="sxs-lookup"><span data-stu-id="71a35-380">Geometry.Touches(Geometry)</span></span> | <span data-ttu-id="71a35-381">✔</span><span class="sxs-lookup"><span data-stu-id="71a35-381">✔</span></span> | | <span data-ttu-id="71a35-382">✔</span><span class="sxs-lookup"><span data-stu-id="71a35-382">✔</span></span> | <span data-ttu-id="71a35-383">✔</span><span class="sxs-lookup"><span data-stu-id="71a35-383">✔</span></span>
<span data-ttu-id="71a35-384">Geometry. Union ()</span><span class="sxs-lookup"><span data-stu-id="71a35-384">Geometry.Union()</span></span> | | | <span data-ttu-id="71a35-385">✔</span><span class="sxs-lookup"><span data-stu-id="71a35-385">✔</span></span> | <span data-ttu-id="71a35-386">✔</span><span class="sxs-lookup"><span data-stu-id="71a35-386">✔</span></span>
<span data-ttu-id="71a35-387">Geometry. Union (геометрия)</span><span class="sxs-lookup"><span data-stu-id="71a35-387">Geometry.Union(Geometry)</span></span> | <span data-ttu-id="71a35-388">✔</span><span class="sxs-lookup"><span data-stu-id="71a35-388">✔</span></span> | <span data-ttu-id="71a35-389">✔</span><span class="sxs-lookup"><span data-stu-id="71a35-389">✔</span></span> | <span data-ttu-id="71a35-390">✔</span><span class="sxs-lookup"><span data-stu-id="71a35-390">✔</span></span> | <span data-ttu-id="71a35-391">✔</span><span class="sxs-lookup"><span data-stu-id="71a35-391">✔</span></span>
<span data-ttu-id="71a35-392">Геометрия. внутри (Geometry)</span><span class="sxs-lookup"><span data-stu-id="71a35-392">Geometry.Within(Geometry)</span></span> | <span data-ttu-id="71a35-393">✔</span><span class="sxs-lookup"><span data-stu-id="71a35-393">✔</span></span> | <span data-ttu-id="71a35-394">✔</span><span class="sxs-lookup"><span data-stu-id="71a35-394">✔</span></span> | <span data-ttu-id="71a35-395">✔</span><span class="sxs-lookup"><span data-stu-id="71a35-395">✔</span></span> | <span data-ttu-id="71a35-396">✔</span><span class="sxs-lookup"><span data-stu-id="71a35-396">✔</span></span>
<span data-ttu-id="71a35-397">GeometryCollection. Count</span><span class="sxs-lookup"><span data-stu-id="71a35-397">GeometryCollection.Count</span></span> | <span data-ttu-id="71a35-398">✔</span><span class="sxs-lookup"><span data-stu-id="71a35-398">✔</span></span> | <span data-ttu-id="71a35-399">✔</span><span class="sxs-lookup"><span data-stu-id="71a35-399">✔</span></span> | <span data-ttu-id="71a35-400">✔</span><span class="sxs-lookup"><span data-stu-id="71a35-400">✔</span></span> | <span data-ttu-id="71a35-401">✔</span><span class="sxs-lookup"><span data-stu-id="71a35-401">✔</span></span>
<span data-ttu-id="71a35-402">GeometryCollection [int]</span><span class="sxs-lookup"><span data-stu-id="71a35-402">GeometryCollection[int]</span></span> | <span data-ttu-id="71a35-403">✔</span><span class="sxs-lookup"><span data-stu-id="71a35-403">✔</span></span> | <span data-ttu-id="71a35-404">✔</span><span class="sxs-lookup"><span data-stu-id="71a35-404">✔</span></span> | <span data-ttu-id="71a35-405">✔</span><span class="sxs-lookup"><span data-stu-id="71a35-405">✔</span></span> | <span data-ttu-id="71a35-406">✔</span><span class="sxs-lookup"><span data-stu-id="71a35-406">✔</span></span>
<span data-ttu-id="71a35-407">LineString. Count</span><span class="sxs-lookup"><span data-stu-id="71a35-407">LineString.Count</span></span> | <span data-ttu-id="71a35-408">✔</span><span class="sxs-lookup"><span data-stu-id="71a35-408">✔</span></span> | <span data-ttu-id="71a35-409">✔</span><span class="sxs-lookup"><span data-stu-id="71a35-409">✔</span></span> | <span data-ttu-id="71a35-410">✔</span><span class="sxs-lookup"><span data-stu-id="71a35-410">✔</span></span> | <span data-ttu-id="71a35-411">✔</span><span class="sxs-lookup"><span data-stu-id="71a35-411">✔</span></span>
<span data-ttu-id="71a35-412">LineString. EndPoint</span><span class="sxs-lookup"><span data-stu-id="71a35-412">LineString.EndPoint</span></span> | <span data-ttu-id="71a35-413">✔</span><span class="sxs-lookup"><span data-stu-id="71a35-413">✔</span></span> | <span data-ttu-id="71a35-414">✔</span><span class="sxs-lookup"><span data-stu-id="71a35-414">✔</span></span> | <span data-ttu-id="71a35-415">✔</span><span class="sxs-lookup"><span data-stu-id="71a35-415">✔</span></span> | <span data-ttu-id="71a35-416">✔</span><span class="sxs-lookup"><span data-stu-id="71a35-416">✔</span></span>
<span data-ttu-id="71a35-417">LineString. Жетпоинтн (int)</span><span class="sxs-lookup"><span data-stu-id="71a35-417">LineString.GetPointN(int)</span></span> | <span data-ttu-id="71a35-418">✔</span><span class="sxs-lookup"><span data-stu-id="71a35-418">✔</span></span> | <span data-ttu-id="71a35-419">✔</span><span class="sxs-lookup"><span data-stu-id="71a35-419">✔</span></span> | <span data-ttu-id="71a35-420">✔</span><span class="sxs-lookup"><span data-stu-id="71a35-420">✔</span></span> | <span data-ttu-id="71a35-421">✔</span><span class="sxs-lookup"><span data-stu-id="71a35-421">✔</span></span>
<span data-ttu-id="71a35-422">LineString. крыл</span><span class="sxs-lookup"><span data-stu-id="71a35-422">LineString.IsClosed</span></span> | <span data-ttu-id="71a35-423">✔</span><span class="sxs-lookup"><span data-stu-id="71a35-423">✔</span></span> | <span data-ttu-id="71a35-424">✔</span><span class="sxs-lookup"><span data-stu-id="71a35-424">✔</span></span> | <span data-ttu-id="71a35-425">✔</span><span class="sxs-lookup"><span data-stu-id="71a35-425">✔</span></span> | <span data-ttu-id="71a35-426">✔</span><span class="sxs-lookup"><span data-stu-id="71a35-426">✔</span></span>
<span data-ttu-id="71a35-427">LineString. Звонок</span><span class="sxs-lookup"><span data-stu-id="71a35-427">LineString.IsRing</span></span> | <span data-ttu-id="71a35-428">✔</span><span class="sxs-lookup"><span data-stu-id="71a35-428">✔</span></span> | | <span data-ttu-id="71a35-429">✔</span><span class="sxs-lookup"><span data-stu-id="71a35-429">✔</span></span> | <span data-ttu-id="71a35-430">✔</span><span class="sxs-lookup"><span data-stu-id="71a35-430">✔</span></span>
<span data-ttu-id="71a35-431">LineString. StartPoint</span><span class="sxs-lookup"><span data-stu-id="71a35-431">LineString.StartPoint</span></span> | <span data-ttu-id="71a35-432">✔</span><span class="sxs-lookup"><span data-stu-id="71a35-432">✔</span></span> | <span data-ttu-id="71a35-433">✔</span><span class="sxs-lookup"><span data-stu-id="71a35-433">✔</span></span> | <span data-ttu-id="71a35-434">✔</span><span class="sxs-lookup"><span data-stu-id="71a35-434">✔</span></span> | <span data-ttu-id="71a35-435">✔</span><span class="sxs-lookup"><span data-stu-id="71a35-435">✔</span></span>
<span data-ttu-id="71a35-436">MultiLineString. крыл</span><span class="sxs-lookup"><span data-stu-id="71a35-436">MultiLineString.IsClosed</span></span> | <span data-ttu-id="71a35-437">✔</span><span class="sxs-lookup"><span data-stu-id="71a35-437">✔</span></span> | <span data-ttu-id="71a35-438">✔</span><span class="sxs-lookup"><span data-stu-id="71a35-438">✔</span></span> | <span data-ttu-id="71a35-439">✔</span><span class="sxs-lookup"><span data-stu-id="71a35-439">✔</span></span> | <span data-ttu-id="71a35-440">✔</span><span class="sxs-lookup"><span data-stu-id="71a35-440">✔</span></span>
<span data-ttu-id="71a35-441">Point. M</span><span class="sxs-lookup"><span data-stu-id="71a35-441">Point.M</span></span> | <span data-ttu-id="71a35-442">✔</span><span class="sxs-lookup"><span data-stu-id="71a35-442">✔</span></span> | <span data-ttu-id="71a35-443">✔</span><span class="sxs-lookup"><span data-stu-id="71a35-443">✔</span></span> | <span data-ttu-id="71a35-444">✔</span><span class="sxs-lookup"><span data-stu-id="71a35-444">✔</span></span> | <span data-ttu-id="71a35-445">✔</span><span class="sxs-lookup"><span data-stu-id="71a35-445">✔</span></span>
<span data-ttu-id="71a35-446">Point. X</span><span class="sxs-lookup"><span data-stu-id="71a35-446">Point.X</span></span> | <span data-ttu-id="71a35-447">✔</span><span class="sxs-lookup"><span data-stu-id="71a35-447">✔</span></span> | <span data-ttu-id="71a35-448">✔</span><span class="sxs-lookup"><span data-stu-id="71a35-448">✔</span></span> | <span data-ttu-id="71a35-449">✔</span><span class="sxs-lookup"><span data-stu-id="71a35-449">✔</span></span> | <span data-ttu-id="71a35-450">✔</span><span class="sxs-lookup"><span data-stu-id="71a35-450">✔</span></span>
<span data-ttu-id="71a35-451">Point. Y</span><span class="sxs-lookup"><span data-stu-id="71a35-451">Point.Y</span></span> | <span data-ttu-id="71a35-452">✔</span><span class="sxs-lookup"><span data-stu-id="71a35-452">✔</span></span> | <span data-ttu-id="71a35-453">✔</span><span class="sxs-lookup"><span data-stu-id="71a35-453">✔</span></span> | <span data-ttu-id="71a35-454">✔</span><span class="sxs-lookup"><span data-stu-id="71a35-454">✔</span></span> | <span data-ttu-id="71a35-455">✔</span><span class="sxs-lookup"><span data-stu-id="71a35-455">✔</span></span>
<span data-ttu-id="71a35-456">Point. Z</span><span class="sxs-lookup"><span data-stu-id="71a35-456">Point.Z</span></span> | <span data-ttu-id="71a35-457">✔</span><span class="sxs-lookup"><span data-stu-id="71a35-457">✔</span></span> | <span data-ttu-id="71a35-458">✔</span><span class="sxs-lookup"><span data-stu-id="71a35-458">✔</span></span> | <span data-ttu-id="71a35-459">✔</span><span class="sxs-lookup"><span data-stu-id="71a35-459">✔</span></span> | <span data-ttu-id="71a35-460">✔</span><span class="sxs-lookup"><span data-stu-id="71a35-460">✔</span></span>
<span data-ttu-id="71a35-461">Polygon. Екстериорринг</span><span class="sxs-lookup"><span data-stu-id="71a35-461">Polygon.ExteriorRing</span></span> | <span data-ttu-id="71a35-462">✔</span><span class="sxs-lookup"><span data-stu-id="71a35-462">✔</span></span> | <span data-ttu-id="71a35-463">✔</span><span class="sxs-lookup"><span data-stu-id="71a35-463">✔</span></span> | <span data-ttu-id="71a35-464">✔</span><span class="sxs-lookup"><span data-stu-id="71a35-464">✔</span></span> | <span data-ttu-id="71a35-465">✔</span><span class="sxs-lookup"><span data-stu-id="71a35-465">✔</span></span>
<span data-ttu-id="71a35-466">Polygon. Жетинтериоррингн (int)</span><span class="sxs-lookup"><span data-stu-id="71a35-466">Polygon.GetInteriorRingN(int)</span></span> | <span data-ttu-id="71a35-467">✔</span><span class="sxs-lookup"><span data-stu-id="71a35-467">✔</span></span> | <span data-ttu-id="71a35-468">✔</span><span class="sxs-lookup"><span data-stu-id="71a35-468">✔</span></span> | <span data-ttu-id="71a35-469">✔</span><span class="sxs-lookup"><span data-stu-id="71a35-469">✔</span></span> | <span data-ttu-id="71a35-470">✔</span><span class="sxs-lookup"><span data-stu-id="71a35-470">✔</span></span>
<span data-ttu-id="71a35-471">Polygon. Нуминтериоррингс</span><span class="sxs-lookup"><span data-stu-id="71a35-471">Polygon.NumInteriorRings</span></span> | <span data-ttu-id="71a35-472">✔</span><span class="sxs-lookup"><span data-stu-id="71a35-472">✔</span></span> | <span data-ttu-id="71a35-473">✔</span><span class="sxs-lookup"><span data-stu-id="71a35-473">✔</span></span> | <span data-ttu-id="71a35-474">✔</span><span class="sxs-lookup"><span data-stu-id="71a35-474">✔</span></span> | <span data-ttu-id="71a35-475">✔</span><span class="sxs-lookup"><span data-stu-id="71a35-475">✔</span></span>

## <a name="additional-resources"></a><span data-ttu-id="71a35-476">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="71a35-476">Additional resources</span></span>

* [<span data-ttu-id="71a35-477">Пространственные данные SQL Server</span><span class="sxs-lookup"><span data-stu-id="71a35-477">Spatial Data in SQL Server</span></span>](/sql/relational-databases/spatial/spatial-data-sql-server)
* [<span data-ttu-id="71a35-478">Домашняя страница Спатиалите</span><span class="sxs-lookup"><span data-stu-id="71a35-478">SpatiaLite Homepage</span></span>](https://www.gaia-gis.it/fossil/libspatialite)
* [<span data-ttu-id="71a35-479">Документация по Npgsqlной пространственной документации</span><span class="sxs-lookup"><span data-stu-id="71a35-479">Npgsql Spatial Documentation</span></span>](https://www.npgsql.org/efcore/mapping/nts.html)
* [<span data-ttu-id="71a35-480">Документация по PostGIS</span><span class="sxs-lookup"><span data-stu-id="71a35-480">PostGIS Documentation</span></span>](https://postgis.net/documentation/)
