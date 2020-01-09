---
title: Пространственные данные — EF Core
author: bricelam
ms.author: bricelam
ms.date: 11/01/2018
ms.assetid: 2BDE29FC-4161-41A0-841E-69F51CCD9341
uid: core/modeling/spatial
ms.openlocfilehash: 8dae1ab949c77ffa08904b12a5716b729e6913a1
ms.sourcegitcommit: 32c51c22988c6f83ed4f8e50a1d01be3f4114e81
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/27/2019
ms.locfileid: "75502244"
---
# <a name="spatial-data"></a><span data-ttu-id="9e1f4-102">Пространственные данные</span><span class="sxs-lookup"><span data-stu-id="9e1f4-102">Spatial Data</span></span>

> [!NOTE]
> <span data-ttu-id="9e1f4-103">Эта функция была добавлена в EF Core 2,2.</span><span class="sxs-lookup"><span data-stu-id="9e1f4-103">This feature was added in EF Core 2.2.</span></span>

<span data-ttu-id="9e1f4-104">Пространственные данные представляют физическое расположение и форму объектов.</span><span class="sxs-lookup"><span data-stu-id="9e1f4-104">Spatial data represents the physical location and the shape of objects.</span></span> <span data-ttu-id="9e1f4-105">Многие базы данных обеспечивают поддержку этого типа данных, поэтому их можно индексировать и запрашивать вместе с другими данными.</span><span class="sxs-lookup"><span data-stu-id="9e1f4-105">Many databases provide support for this type of data so it can be indexed and queried alongside other data.</span></span> <span data-ttu-id="9e1f4-106">Типичные сценарии включают запросы объектов в заданном расстоянии от расположения или выбор объекта, граница которого содержит заданное расположение.</span><span class="sxs-lookup"><span data-stu-id="9e1f4-106">Common scenarios include querying for objects within a given distance from a location, or selecting the object whose border contains a given location.</span></span> <span data-ttu-id="9e1f4-107">EF Core поддерживает сопоставление с пространственными типами данных с помощью пространственной библиотеки [неттопологисуите](https://github.com/NetTopologySuite/NetTopologySuite) .</span><span class="sxs-lookup"><span data-stu-id="9e1f4-107">EF Core supports mapping to spatial data types using the [NetTopologySuite](https://github.com/NetTopologySuite/NetTopologySuite) spatial library.</span></span>

## <a name="installing"></a><span data-ttu-id="9e1f4-108">Установка</span><span class="sxs-lookup"><span data-stu-id="9e1f4-108">Installing</span></span>

<span data-ttu-id="9e1f4-109">Чтобы использовать пространственные данные с EF Core, необходимо установить соответствующий поддерживающий пакет NuGet.</span><span class="sxs-lookup"><span data-stu-id="9e1f4-109">In order to use spatial data with EF Core, you need to install the appropriate supporting NuGet package.</span></span> <span data-ttu-id="9e1f4-110">Пакет, который необходимо установить, зависит от поставщика, который вы используете.</span><span class="sxs-lookup"><span data-stu-id="9e1f4-110">Which package you need to install depends on the provider you're using.</span></span>

<span data-ttu-id="9e1f4-111">Поставщик EF Core</span><span class="sxs-lookup"><span data-stu-id="9e1f4-111">EF Core Provider</span></span>                        | <span data-ttu-id="9e1f4-112">Пространственный пакет NuGet</span><span class="sxs-lookup"><span data-stu-id="9e1f4-112">Spatial NuGet Package</span></span>
--------------------------------------- | ---------------------
<span data-ttu-id="9e1f4-113">Microsoft.EntityFrameworkCore.SqlServer</span><span class="sxs-lookup"><span data-stu-id="9e1f4-113">Microsoft.EntityFrameworkCore.SqlServer</span></span> | [<span data-ttu-id="9e1f4-114">Microsoft. EntityFrameworkCore. SqlServer. Неттопологисуите</span><span class="sxs-lookup"><span data-stu-id="9e1f4-114">Microsoft.EntityFrameworkCore.SqlServer.NetTopologySuite</span></span>](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer.NetTopologySuite)
<span data-ttu-id="9e1f4-115">Microsoft.EntityFrameworkCore.Sqlite</span><span class="sxs-lookup"><span data-stu-id="9e1f4-115">Microsoft.EntityFrameworkCore.Sqlite</span></span>    | [<span data-ttu-id="9e1f4-116">Microsoft. EntityFrameworkCore. SQLite. Неттопологисуите</span><span class="sxs-lookup"><span data-stu-id="9e1f4-116">Microsoft.EntityFrameworkCore.Sqlite.NetTopologySuite</span></span>](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Sqlite.NetTopologySuite)
<span data-ttu-id="9e1f4-117">Microsoft.EntityFrameworkCore.InMemory</span><span class="sxs-lookup"><span data-stu-id="9e1f4-117">Microsoft.EntityFrameworkCore.InMemory</span></span>  | [<span data-ttu-id="9e1f4-118">неттопологисуите</span><span class="sxs-lookup"><span data-stu-id="9e1f4-118">NetTopologySuite</span></span>](https://www.nuget.org/packages/NetTopologySuite)
<span data-ttu-id="9e1f4-119">Npgsql.EntityFrameworkCore.PostgreSQL</span><span class="sxs-lookup"><span data-stu-id="9e1f4-119">Npgsql.EntityFrameworkCore.PostgreSQL</span></span>   | [<span data-ttu-id="9e1f4-120">Npgsql. EntityFrameworkCore. PostgreSQL. Неттопологисуите</span><span class="sxs-lookup"><span data-stu-id="9e1f4-120">Npgsql.EntityFrameworkCore.PostgreSQL.NetTopologySuite</span></span>](https://www.nuget.org/packages/Npgsql.EntityFrameworkCore.PostgreSQL.NetTopologySuite)

## <a name="reverse-engineering"></a><span data-ttu-id="9e1f4-121">реконструирование;</span><span class="sxs-lookup"><span data-stu-id="9e1f4-121">Reverse engineering</span></span>

<span data-ttu-id="9e1f4-122">Пространственные пакеты NuGet также включают модели [реконструирования](../managing-schemas/scaffolding.md) с пространственными свойствами, но ***перед*** запуском `Scaffold-DbContext` или `dotnet ef dbcontext scaffold`необходимо установить пакет.</span><span class="sxs-lookup"><span data-stu-id="9e1f4-122">The spatial NuGet packages also enable [reverse engineering](../managing-schemas/scaffolding.md) models with spatial properties, but you need to install the package ***before*** running `Scaffold-DbContext` or `dotnet ef dbcontext scaffold`.</span></span> <span data-ttu-id="9e1f4-123">В противном случае вы получите предупреждения о том, что не удается найти сопоставления типов для столбцов, и столбцы будут пропущены.</span><span class="sxs-lookup"><span data-stu-id="9e1f4-123">If you don't, you'll receive warnings about not finding type mappings for the columns and the columns will be skipped.</span></span>

## <a name="nettopologysuite-nts"></a><span data-ttu-id="9e1f4-124">Неттопологисуите (NTS)</span><span class="sxs-lookup"><span data-stu-id="9e1f4-124">NetTopologySuite (NTS)</span></span>

<span data-ttu-id="9e1f4-125">Неттопологисуите является пространственной библиотекой для .NET.</span><span class="sxs-lookup"><span data-stu-id="9e1f4-125">NetTopologySuite is a spatial library for .NET.</span></span> <span data-ttu-id="9e1f4-126">EF Core поддерживает сопоставление с пространственными типами данных в базе данных с помощью типов NTS в модели.</span><span class="sxs-lookup"><span data-stu-id="9e1f4-126">EF Core enables mapping to spatial data types in the database by using NTS types in your model.</span></span>

<span data-ttu-id="9e1f4-127">Чтобы включить сопоставление с пространственными типами через NTS, вызовите метод Усенеттопологисуите в построителе параметров DbContext поставщика.</span><span class="sxs-lookup"><span data-stu-id="9e1f4-127">To enable mapping to spatial types via NTS, call the UseNetTopologySuite method on the provider's DbContext options builder.</span></span> <span data-ttu-id="9e1f4-128">Например, с SQL Server можно назвать его следующим образом.</span><span class="sxs-lookup"><span data-stu-id="9e1f4-128">For example, with SQL Server you'd call it like this.</span></span>

``` csharp
optionsBuilder.UseSqlServer(
    @"Data Source=(localdb)\MSSQLLocalDB;Initial Catalog=WideWorldImporters",
    x => x.UseNetTopologySuite());
```

<span data-ttu-id="9e1f4-129">Существует несколько типов пространственных данных.</span><span class="sxs-lookup"><span data-stu-id="9e1f4-129">There are several spatial data types.</span></span> <span data-ttu-id="9e1f4-130">Используемый тип зависит от типов фигур, которые вы хотите разрешить.</span><span class="sxs-lookup"><span data-stu-id="9e1f4-130">Which type you use depends on the types of shapes you want to allow.</span></span> <span data-ttu-id="9e1f4-131">Ниже приведена иерархия типов NTS, которые можно использовать для свойств в модели.</span><span class="sxs-lookup"><span data-stu-id="9e1f4-131">Here is the hierarchy of NTS types that you can use for properties in your model.</span></span> <span data-ttu-id="9e1f4-132">Они находятся в пространстве имен `NetTopologySuite.Geometries`.</span><span class="sxs-lookup"><span data-stu-id="9e1f4-132">They're located within the `NetTopologySuite.Geometries` namespace.</span></span>

* <span data-ttu-id="9e1f4-133">Geometry</span><span class="sxs-lookup"><span data-stu-id="9e1f4-133">Geometry</span></span>
  * <span data-ttu-id="9e1f4-134">отчетов</span><span class="sxs-lookup"><span data-stu-id="9e1f4-134">Point</span></span>
  * <span data-ttu-id="9e1f4-135">LineString</span><span class="sxs-lookup"><span data-stu-id="9e1f4-135">LineString</span></span>
  * <span data-ttu-id="9e1f4-136">Polygon</span><span class="sxs-lookup"><span data-stu-id="9e1f4-136">Polygon</span></span>
  * <span data-ttu-id="9e1f4-137">GeometryCollection</span><span class="sxs-lookup"><span data-stu-id="9e1f4-137">GeometryCollection</span></span>
    * <span data-ttu-id="9e1f4-138">MultiPoint</span><span class="sxs-lookup"><span data-stu-id="9e1f4-138">MultiPoint</span></span>
    * <span data-ttu-id="9e1f4-139">MultiLineString</span><span class="sxs-lookup"><span data-stu-id="9e1f4-139">MultiLineString</span></span>
    * <span data-ttu-id="9e1f4-140">MultiPolygon</span><span class="sxs-lookup"><span data-stu-id="9e1f4-140">MultiPolygon</span></span>

> [!WARNING]
> <span data-ttu-id="9e1f4-141">CircularString, CompoundCurve и Куреполигон не поддерживаются NTS.</span><span class="sxs-lookup"><span data-stu-id="9e1f4-141">CircularString, CompoundCurve, and CurePolygon aren't supported by NTS.</span></span>

<span data-ttu-id="9e1f4-142">Использование базового типа geometry позволяет задать любой тип фигуры в свойстве.</span><span class="sxs-lookup"><span data-stu-id="9e1f4-142">Using the base Geometry type allows any type of shape to be specified by the property.</span></span>

<span data-ttu-id="9e1f4-143">Следующие классы сущностей можно использовать для соотнесения с таблицами в [образце базы данных Wide World импортеров](https://go.microsoft.com/fwlink/?LinkID=800630).</span><span class="sxs-lookup"><span data-stu-id="9e1f4-143">The following entity classes could be used to map to tables in the [Wide World Importers sample database](https://go.microsoft.com/fwlink/?LinkID=800630).</span></span>

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

### <a name="creating-values"></a><span data-ttu-id="9e1f4-144">Создание значений</span><span class="sxs-lookup"><span data-stu-id="9e1f4-144">Creating values</span></span>

<span data-ttu-id="9e1f4-145">Для создания объектов Geometry можно использовать конструкторы. Однако NTS рекомендует использовать вместо нее геометрическую фабрику.</span><span class="sxs-lookup"><span data-stu-id="9e1f4-145">You can use constructors to create geometry objects; however, NTS recommends using a geometry factory instead.</span></span> <span data-ttu-id="9e1f4-146">Это позволяет указать SRID по умолчанию (систему пространственной ссылки, используемую координатами) и дает возможность управлять более сложными возможностями, такими как модель точности (используемая во время вычислений), и последовательность координат (определяет, какие координаты оси). и меры — доступны).</span><span class="sxs-lookup"><span data-stu-id="9e1f4-146">This lets you specify a default SRID (the spatial reference system used by the coordinates) and gives you control over more advanced things like the precision model (used during calculations) and the coordinate sequence (determines which ordinates--dimensions and measures--are available).</span></span>

``` csharp
var geometryFactory = NtsGeometryServices.Instance.CreateGeometryFactory(srid: 4326);
var currentLocation = geometryFactory.CreatePoint(-122.121512, 47.6739882);
```

> [!NOTE]
> <span data-ttu-id="9e1f4-147">4326 относится к WGS 84, стандарту, используемому в GPS и других географических системах.</span><span class="sxs-lookup"><span data-stu-id="9e1f4-147">4326 refers to WGS 84, a standard used in GPS and other geographic systems.</span></span>

### <a name="longitude-and-latitude"></a><span data-ttu-id="9e1f4-148">Долгота и Широта</span><span class="sxs-lookup"><span data-stu-id="9e1f4-148">Longitude and Latitude</span></span>

<span data-ttu-id="9e1f4-149">Координаты в NTS находятся в терминах значений X и Y.</span><span class="sxs-lookup"><span data-stu-id="9e1f4-149">Coordinates in NTS are in terms of X and Y values.</span></span> <span data-ttu-id="9e1f4-150">Чтобы представить долготу и широту, используйте X для долготы и Y для широты.</span><span class="sxs-lookup"><span data-stu-id="9e1f4-150">To represent longitude and latitude, use X for longitude and Y for latitude.</span></span> <span data-ttu-id="9e1f4-151">Обратите внимание, что это происходит **обратно** из формата `latitude, longitude`, в котором обычно отображаются эти значения.</span><span class="sxs-lookup"><span data-stu-id="9e1f4-151">Note that this is **backwards** from the `latitude, longitude` format in which you typically see these values.</span></span>

### <a name="srid-ignored-during-client-operations"></a><span data-ttu-id="9e1f4-152">SRID игнорируется во время операций клиента</span><span class="sxs-lookup"><span data-stu-id="9e1f4-152">SRID Ignored during client operations</span></span>

<span data-ttu-id="9e1f4-153">NTS игнорирует значения SRID во время операций.</span><span class="sxs-lookup"><span data-stu-id="9e1f4-153">NTS ignores SRID values during operations.</span></span> <span data-ttu-id="9e1f4-154">Предполагается, что это плоская система координат.</span><span class="sxs-lookup"><span data-stu-id="9e1f4-154">It assumes a planar coordinate system.</span></span> <span data-ttu-id="9e1f4-155">Это означает, что если указать координаты в виде долготы и широты, то некоторые значения, вычисляемые клиентом, такие как расстояние, Длина и площадь, будут в градусах, а не в метрах.</span><span class="sxs-lookup"><span data-stu-id="9e1f4-155">This means that if you specify coordinates in terms of longitude and latitude, some client-evaluated values like distance, length, and area will be in degrees, not meters.</span></span> <span data-ttu-id="9e1f4-156">Чтобы получить более значимые значения, сначала необходимо проецировать координаты в другую систему координат, используя библиотеку, например [ProjNet4GeoAPI](https://github.com/NetTopologySuite/ProjNet4GeoAPI) , перед вычислением этих значений.</span><span class="sxs-lookup"><span data-stu-id="9e1f4-156">For more meaningful values, you first need to project the coordinates to another coordinate system using a library like [ProjNet4GeoAPI](https://github.com/NetTopologySuite/ProjNet4GeoAPI) before calculating these values.</span></span>

<span data-ttu-id="9e1f4-157">Если операция оценивается сервером с помощью EF Core с помощью SQL, то единица результата будет определяться базой данных.</span><span class="sxs-lookup"><span data-stu-id="9e1f4-157">If an operation is server-evaluated by EF Core via SQL, the result's unit will be determined by the database.</span></span>

<span data-ttu-id="9e1f4-158">Ниже приведен пример использования ProjNet4GeoAPI для вычисления расстояния между двумя городами.</span><span class="sxs-lookup"><span data-stu-id="9e1f4-158">Here is an example of using ProjNet4GeoAPI to calculate the distance between two cities.</span></span>

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

## <a name="querying-data"></a><span data-ttu-id="9e1f4-159">Запросы к данным</span><span class="sxs-lookup"><span data-stu-id="9e1f4-159">Querying Data</span></span>

<span data-ttu-id="9e1f4-160">В LINQ методы и свойства NTS, доступные в качестве функций базы данных, будут преобразованы в SQL.</span><span class="sxs-lookup"><span data-stu-id="9e1f4-160">In LINQ, the NTS methods and properties available as database functions will be translated to SQL.</span></span> <span data-ttu-id="9e1f4-161">Например, методы Distance и Contains преобразуются в следующие запросы.</span><span class="sxs-lookup"><span data-stu-id="9e1f4-161">For example, the Distance and Contains methods are translated in the following queries.</span></span> <span data-ttu-id="9e1f4-162">В таблице в конце этой статьи показано, какие элементы поддерживаются различными поставщиками EF Core.</span><span class="sxs-lookup"><span data-stu-id="9e1f4-162">The table at the end of this article shows which members are supported by various EF Core providers.</span></span>

``` csharp
var nearestCity = db.Cities
    .OrderBy(c => c.Location.Distance(currentLocation))
    .FirstOrDefault();

var currentCountry = db.Countries
    .FirstOrDefault(c => c.Border.Contains(currentLocation));
```

## <a name="sql-server"></a><span data-ttu-id="9e1f4-163">SQL Server</span><span class="sxs-lookup"><span data-stu-id="9e1f4-163">SQL Server</span></span>

<span data-ttu-id="9e1f4-164">Если вы используете SQL Server, необходимо учитывать некоторые дополнительные моменты.</span><span class="sxs-lookup"><span data-stu-id="9e1f4-164">If you're using SQL Server, there are some additional things you should be aware of.</span></span>

### <a name="geography-or-geometry"></a><span data-ttu-id="9e1f4-165">География или геометрические объекты</span><span class="sxs-lookup"><span data-stu-id="9e1f4-165">Geography or geometry</span></span>

<span data-ttu-id="9e1f4-166">По умолчанию пространственные свойства сопоставляются со столбцами `geography` в SQL Server.</span><span class="sxs-lookup"><span data-stu-id="9e1f4-166">By default, spatial properties are mapped to `geography` columns in SQL Server.</span></span> <span data-ttu-id="9e1f4-167">Чтобы использовать `geometry`, [Настройте тип столбца](xref:core/modeling/entity-properties#column-data-types) в модели.</span><span class="sxs-lookup"><span data-stu-id="9e1f4-167">To use `geometry`, [configure the column type](xref:core/modeling/entity-properties#column-data-types) in your model.</span></span>

### <a name="geography-polygon-rings"></a><span data-ttu-id="9e1f4-168">Кольца географических многоугольников</span><span class="sxs-lookup"><span data-stu-id="9e1f4-168">Geography polygon rings</span></span>

<span data-ttu-id="9e1f4-169">При использовании типа столбца `geography` SQL Server накладывает дополнительные требования на внешнее кольцо (или оболочку) и внутренние кольца (или отверстия).</span><span class="sxs-lookup"><span data-stu-id="9e1f4-169">When using the `geography` column type, SQL Server imposes additional requirements on the exterior ring (or shell) and interior rings (or holes).</span></span> <span data-ttu-id="9e1f4-170">Внешнее кольцо должно быть ориентировано против часовой стрелки и внутренних колец по часовой стрелке.</span><span class="sxs-lookup"><span data-stu-id="9e1f4-170">The exterior ring must be oriented counterclockwise and the interior rings clockwise.</span></span> <span data-ttu-id="9e1f4-171">NTS проверяет это перед отправкой значений в базу данных.</span><span class="sxs-lookup"><span data-stu-id="9e1f4-171">NTS validates this before sending values to the database.</span></span>

### <a name="fullglobe"></a><span data-ttu-id="9e1f4-172">FullGlobe</span><span class="sxs-lookup"><span data-stu-id="9e1f4-172">FullGlobe</span></span>

<span data-ttu-id="9e1f4-173">SQL Server имеет нестандартный тип Geometry для представления полного земного шара при использовании типа столбца `geography`.</span><span class="sxs-lookup"><span data-stu-id="9e1f4-173">SQL Server has a non-standard geometry type to represent the full globe when using the `geography` column type.</span></span> <span data-ttu-id="9e1f4-174">У него также есть способ представления многоугольников на основе полного земного шара (без внешнего кольца).</span><span class="sxs-lookup"><span data-stu-id="9e1f4-174">It also has a way to represent polygons based on the full globe (without an exterior ring).</span></span> <span data-ttu-id="9e1f4-175">Ни один из этих элементов не поддерживается NTS.</span><span class="sxs-lookup"><span data-stu-id="9e1f4-175">Neither of these are supported by NTS.</span></span>

> [!WARNING]
> <span data-ttu-id="9e1f4-176">FullGlobe и многоугольники, основанные на нем, не поддерживаются NTS.</span><span class="sxs-lookup"><span data-stu-id="9e1f4-176">FullGlobe and polygons based on it aren't supported by NTS.</span></span>

## <a name="sqlite"></a><span data-ttu-id="9e1f4-177">SQLite</span><span class="sxs-lookup"><span data-stu-id="9e1f4-177">SQLite</span></span>

<span data-ttu-id="9e1f4-178">Ниже приведена дополнительная информация для тех, кто использует SQLite.</span><span class="sxs-lookup"><span data-stu-id="9e1f4-178">Here is some additional information for those using SQLite.</span></span>

### <a name="installing-spatialite"></a><span data-ttu-id="9e1f4-179">Установка Спатиалите</span><span class="sxs-lookup"><span data-stu-id="9e1f4-179">Installing SpatiaLite</span></span>

<span data-ttu-id="9e1f4-180">В Windows собственная библиотека mod_spatialite распространяется как зависимость пакета NuGet.</span><span class="sxs-lookup"><span data-stu-id="9e1f4-180">On Windows, the native mod_spatialite library is distributed as a NuGet package dependency.</span></span> <span data-ttu-id="9e1f4-181">Другие платформы должны установить его отдельно.</span><span class="sxs-lookup"><span data-stu-id="9e1f4-181">Other platforms need to install it separately.</span></span> <span data-ttu-id="9e1f4-182">Обычно это делается с помощью диспетчера пакетов программного обеспечения.</span><span class="sxs-lookup"><span data-stu-id="9e1f4-182">This is typically done using a software package manager.</span></span> <span data-ttu-id="9e1f4-183">Например, можно использовать APT для Ubuntu и Homebrew на MacOS.</span><span class="sxs-lookup"><span data-stu-id="9e1f4-183">For example, you can use APT on Ubuntu and Homebrew on MacOS.</span></span>

``` sh
# Ubuntu
apt-get install libsqlite3-mod-spatialite

# macOS
brew install libspatialite
```

### <a name="configuring-srid"></a><span data-ttu-id="9e1f4-184">Настройка SRID</span><span class="sxs-lookup"><span data-stu-id="9e1f4-184">Configuring SRID</span></span>

<span data-ttu-id="9e1f4-185">В Спатиалите столбцам необходимо указать SRID для каждого столбца.</span><span class="sxs-lookup"><span data-stu-id="9e1f4-185">In SpatiaLite, columns need to specify an SRID per column.</span></span> <span data-ttu-id="9e1f4-186">Значение по умолчанию для SRID — `0`.</span><span class="sxs-lookup"><span data-stu-id="9e1f4-186">The default SRID is `0`.</span></span> <span data-ttu-id="9e1f4-187">Укажите другой SRID с помощью метода Форсклитехассрид.</span><span class="sxs-lookup"><span data-stu-id="9e1f4-187">Specify a different SRID using the ForSqliteHasSrid method.</span></span>

``` csharp
modelBuilder.Entity<City>().Property(c => c.Location)
    .ForSqliteHasSrid(4326);
```

### <a name="dimension"></a><span data-ttu-id="9e1f4-188">Измерение</span><span class="sxs-lookup"><span data-stu-id="9e1f4-188">Dimension</span></span>

<span data-ttu-id="9e1f4-189">Как и в случае с SRID, измерение столбца (или координата) также указывается как часть столбца.</span><span class="sxs-lookup"><span data-stu-id="9e1f4-189">Similar to SRID, a column's dimension (or ordinates) is also specified as part of the column.</span></span> <span data-ttu-id="9e1f4-190">По умолчанию используются координаты X и Y. Включите дополнительные координаты (Z и M) с помощью метода Форсклитехасдименсион.</span><span class="sxs-lookup"><span data-stu-id="9e1f4-190">The default ordinates are X and Y. Enable additional ordinates (Z and M) using the ForSqliteHasDimension method.</span></span>

``` csharp
modelBuilder.Entity<City>().Property(c => c.Location)
    .ForSqliteHasDimension(Ordinates.XYZ);
```

## <a name="translated-operations"></a><span data-ttu-id="9e1f4-191">Переведенные операции</span><span class="sxs-lookup"><span data-stu-id="9e1f4-191">Translated Operations</span></span>

<span data-ttu-id="9e1f4-192">В этой таблице показано, какие элементы NTS преобразуются в SQL каждым поставщиком EF Core.</span><span class="sxs-lookup"><span data-stu-id="9e1f4-192">This table shows which NTS members are translated into SQL by each EF Core provider.</span></span>

<span data-ttu-id="9e1f4-193">неттопологисуите</span><span class="sxs-lookup"><span data-stu-id="9e1f4-193">NetTopologySuite</span></span> | <span data-ttu-id="9e1f4-194">SQL Server (геометрия)</span><span class="sxs-lookup"><span data-stu-id="9e1f4-194">SQL Server (geometry)</span></span> | <span data-ttu-id="9e1f4-195">SQL Server (география)</span><span class="sxs-lookup"><span data-stu-id="9e1f4-195">SQL Server (geography)</span></span> | <span data-ttu-id="9e1f4-196">SQLite</span><span class="sxs-lookup"><span data-stu-id="9e1f4-196">SQLite</span></span> | <span data-ttu-id="9e1f4-197">Npgsql</span><span class="sxs-lookup"><span data-stu-id="9e1f4-197">Npgsql</span></span>
--- |:---:|:---:|:---:|:---:
<span data-ttu-id="9e1f4-198">Геометрия. область</span><span class="sxs-lookup"><span data-stu-id="9e1f4-198">Geometry.Area</span></span> | <span data-ttu-id="9e1f4-199">✔</span><span class="sxs-lookup"><span data-stu-id="9e1f4-199">✔</span></span> | <span data-ttu-id="9e1f4-200">✔</span><span class="sxs-lookup"><span data-stu-id="9e1f4-200">✔</span></span> | <span data-ttu-id="9e1f4-201">✔</span><span class="sxs-lookup"><span data-stu-id="9e1f4-201">✔</span></span> | <span data-ttu-id="9e1f4-202">✔</span><span class="sxs-lookup"><span data-stu-id="9e1f4-202">✔</span></span>
<span data-ttu-id="9e1f4-203">Geometry. Асбинари ()</span><span class="sxs-lookup"><span data-stu-id="9e1f4-203">Geometry.AsBinary()</span></span> | <span data-ttu-id="9e1f4-204">✔</span><span class="sxs-lookup"><span data-stu-id="9e1f4-204">✔</span></span> | <span data-ttu-id="9e1f4-205">✔</span><span class="sxs-lookup"><span data-stu-id="9e1f4-205">✔</span></span> | <span data-ttu-id="9e1f4-206">✔</span><span class="sxs-lookup"><span data-stu-id="9e1f4-206">✔</span></span> | <span data-ttu-id="9e1f4-207">✔</span><span class="sxs-lookup"><span data-stu-id="9e1f4-207">✔</span></span>
<span data-ttu-id="9e1f4-208">Geometry. Астекст ()</span><span class="sxs-lookup"><span data-stu-id="9e1f4-208">Geometry.AsText()</span></span> | <span data-ttu-id="9e1f4-209">✔</span><span class="sxs-lookup"><span data-stu-id="9e1f4-209">✔</span></span> | <span data-ttu-id="9e1f4-210">✔</span><span class="sxs-lookup"><span data-stu-id="9e1f4-210">✔</span></span> | <span data-ttu-id="9e1f4-211">✔</span><span class="sxs-lookup"><span data-stu-id="9e1f4-211">✔</span></span> | <span data-ttu-id="9e1f4-212">✔</span><span class="sxs-lookup"><span data-stu-id="9e1f4-212">✔</span></span>
<span data-ttu-id="9e1f4-213">Geometry. Граничный</span><span class="sxs-lookup"><span data-stu-id="9e1f4-213">Geometry.Boundary</span></span> | <span data-ttu-id="9e1f4-214">✔</span><span class="sxs-lookup"><span data-stu-id="9e1f4-214">✔</span></span> | | <span data-ttu-id="9e1f4-215">✔</span><span class="sxs-lookup"><span data-stu-id="9e1f4-215">✔</span></span> | <span data-ttu-id="9e1f4-216">✔</span><span class="sxs-lookup"><span data-stu-id="9e1f4-216">✔</span></span>
<span data-ttu-id="9e1f4-217">Геометрия. buffer (двойное)</span><span class="sxs-lookup"><span data-stu-id="9e1f4-217">Geometry.Buffer(double)</span></span> | <span data-ttu-id="9e1f4-218">✔</span><span class="sxs-lookup"><span data-stu-id="9e1f4-218">✔</span></span> | <span data-ttu-id="9e1f4-219">✔</span><span class="sxs-lookup"><span data-stu-id="9e1f4-219">✔</span></span> | <span data-ttu-id="9e1f4-220">✔</span><span class="sxs-lookup"><span data-stu-id="9e1f4-220">✔</span></span> | <span data-ttu-id="9e1f4-221">✔</span><span class="sxs-lookup"><span data-stu-id="9e1f4-221">✔</span></span>
<span data-ttu-id="9e1f4-222">Геометрия. buffer (Double, int)</span><span class="sxs-lookup"><span data-stu-id="9e1f4-222">Geometry.Buffer(double, int)</span></span> | | | <span data-ttu-id="9e1f4-223">✔</span><span class="sxs-lookup"><span data-stu-id="9e1f4-223">✔</span></span> | <span data-ttu-id="9e1f4-224">✔</span><span class="sxs-lookup"><span data-stu-id="9e1f4-224">✔</span></span>
<span data-ttu-id="9e1f4-225">Geometry. центроид</span><span class="sxs-lookup"><span data-stu-id="9e1f4-225">Geometry.Centroid</span></span> | <span data-ttu-id="9e1f4-226">✔</span><span class="sxs-lookup"><span data-stu-id="9e1f4-226">✔</span></span> | | <span data-ttu-id="9e1f4-227">✔</span><span class="sxs-lookup"><span data-stu-id="9e1f4-227">✔</span></span> | <span data-ttu-id="9e1f4-228">✔</span><span class="sxs-lookup"><span data-stu-id="9e1f4-228">✔</span></span>
<span data-ttu-id="9e1f4-229">Геометрия. Contains (геометрия)</span><span class="sxs-lookup"><span data-stu-id="9e1f4-229">Geometry.Contains(Geometry)</span></span> | <span data-ttu-id="9e1f4-230">✔</span><span class="sxs-lookup"><span data-stu-id="9e1f4-230">✔</span></span> | <span data-ttu-id="9e1f4-231">✔</span><span class="sxs-lookup"><span data-stu-id="9e1f4-231">✔</span></span> | <span data-ttu-id="9e1f4-232">✔</span><span class="sxs-lookup"><span data-stu-id="9e1f4-232">✔</span></span> | <span data-ttu-id="9e1f4-233">✔</span><span class="sxs-lookup"><span data-stu-id="9e1f4-233">✔</span></span>
<span data-ttu-id="9e1f4-234">Geometry. Конвексхулл ()</span><span class="sxs-lookup"><span data-stu-id="9e1f4-234">Geometry.ConvexHull()</span></span> | <span data-ttu-id="9e1f4-235">✔</span><span class="sxs-lookup"><span data-stu-id="9e1f4-235">✔</span></span> | <span data-ttu-id="9e1f4-236">✔</span><span class="sxs-lookup"><span data-stu-id="9e1f4-236">✔</span></span> | <span data-ttu-id="9e1f4-237">✔</span><span class="sxs-lookup"><span data-stu-id="9e1f4-237">✔</span></span> | <span data-ttu-id="9e1f4-238">✔</span><span class="sxs-lookup"><span data-stu-id="9e1f4-238">✔</span></span>
<span data-ttu-id="9e1f4-239">Geometry. Ковередби (геометрия)</span><span class="sxs-lookup"><span data-stu-id="9e1f4-239">Geometry.CoveredBy(Geometry)</span></span> | | | <span data-ttu-id="9e1f4-240">✔</span><span class="sxs-lookup"><span data-stu-id="9e1f4-240">✔</span></span> | <span data-ttu-id="9e1f4-241">✔</span><span class="sxs-lookup"><span data-stu-id="9e1f4-241">✔</span></span>
<span data-ttu-id="9e1f4-242">Geometry. крышки (Geometry)</span><span class="sxs-lookup"><span data-stu-id="9e1f4-242">Geometry.Covers(Geometry)</span></span> | | | <span data-ttu-id="9e1f4-243">✔</span><span class="sxs-lookup"><span data-stu-id="9e1f4-243">✔</span></span> | <span data-ttu-id="9e1f4-244">✔</span><span class="sxs-lookup"><span data-stu-id="9e1f4-244">✔</span></span>
<span data-ttu-id="9e1f4-245">Геометрия. перекрестий (Geometry)</span><span class="sxs-lookup"><span data-stu-id="9e1f4-245">Geometry.Crosses(Geometry)</span></span> | <span data-ttu-id="9e1f4-246">✔</span><span class="sxs-lookup"><span data-stu-id="9e1f4-246">✔</span></span> | | <span data-ttu-id="9e1f4-247">✔</span><span class="sxs-lookup"><span data-stu-id="9e1f4-247">✔</span></span> | <span data-ttu-id="9e1f4-248">✔</span><span class="sxs-lookup"><span data-stu-id="9e1f4-248">✔</span></span>
<span data-ttu-id="9e1f4-249">Геометрия. Difference (геометрия)</span><span class="sxs-lookup"><span data-stu-id="9e1f4-249">Geometry.Difference(Geometry)</span></span> | <span data-ttu-id="9e1f4-250">✔</span><span class="sxs-lookup"><span data-stu-id="9e1f4-250">✔</span></span> | <span data-ttu-id="9e1f4-251">✔</span><span class="sxs-lookup"><span data-stu-id="9e1f4-251">✔</span></span> | <span data-ttu-id="9e1f4-252">✔</span><span class="sxs-lookup"><span data-stu-id="9e1f4-252">✔</span></span> | <span data-ttu-id="9e1f4-253">✔</span><span class="sxs-lookup"><span data-stu-id="9e1f4-253">✔</span></span>
<span data-ttu-id="9e1f4-254">Geometry. Dimension</span><span class="sxs-lookup"><span data-stu-id="9e1f4-254">Geometry.Dimension</span></span> | <span data-ttu-id="9e1f4-255">✔</span><span class="sxs-lookup"><span data-stu-id="9e1f4-255">✔</span></span> | <span data-ttu-id="9e1f4-256">✔</span><span class="sxs-lookup"><span data-stu-id="9e1f4-256">✔</span></span> | <span data-ttu-id="9e1f4-257">✔</span><span class="sxs-lookup"><span data-stu-id="9e1f4-257">✔</span></span> | <span data-ttu-id="9e1f4-258">✔</span><span class="sxs-lookup"><span data-stu-id="9e1f4-258">✔</span></span>
<span data-ttu-id="9e1f4-259">Geometry. unjoin (геометрия)</span><span class="sxs-lookup"><span data-stu-id="9e1f4-259">Geometry.Disjoint(Geometry)</span></span> | <span data-ttu-id="9e1f4-260">✔</span><span class="sxs-lookup"><span data-stu-id="9e1f4-260">✔</span></span> | <span data-ttu-id="9e1f4-261">✔</span><span class="sxs-lookup"><span data-stu-id="9e1f4-261">✔</span></span> | <span data-ttu-id="9e1f4-262">✔</span><span class="sxs-lookup"><span data-stu-id="9e1f4-262">✔</span></span> | <span data-ttu-id="9e1f4-263">✔</span><span class="sxs-lookup"><span data-stu-id="9e1f4-263">✔</span></span>
<span data-ttu-id="9e1f4-264">Геометрия. Distance (геометрия)</span><span class="sxs-lookup"><span data-stu-id="9e1f4-264">Geometry.Distance(Geometry)</span></span> | <span data-ttu-id="9e1f4-265">✔</span><span class="sxs-lookup"><span data-stu-id="9e1f4-265">✔</span></span> | <span data-ttu-id="9e1f4-266">✔</span><span class="sxs-lookup"><span data-stu-id="9e1f4-266">✔</span></span> | <span data-ttu-id="9e1f4-267">✔</span><span class="sxs-lookup"><span data-stu-id="9e1f4-267">✔</span></span> | <span data-ttu-id="9e1f4-268">✔</span><span class="sxs-lookup"><span data-stu-id="9e1f4-268">✔</span></span>
<span data-ttu-id="9e1f4-269">Геометрия. конверт</span><span class="sxs-lookup"><span data-stu-id="9e1f4-269">Geometry.Envelope</span></span> | <span data-ttu-id="9e1f4-270">✔</span><span class="sxs-lookup"><span data-stu-id="9e1f4-270">✔</span></span> | | <span data-ttu-id="9e1f4-271">✔</span><span class="sxs-lookup"><span data-stu-id="9e1f4-271">✔</span></span> | <span data-ttu-id="9e1f4-272">✔</span><span class="sxs-lookup"><span data-stu-id="9e1f4-272">✔</span></span>
<span data-ttu-id="9e1f4-273">Geometry. Екуалсексакт (геометрия)</span><span class="sxs-lookup"><span data-stu-id="9e1f4-273">Geometry.EqualsExact(Geometry)</span></span> | | | | <span data-ttu-id="9e1f4-274">✔</span><span class="sxs-lookup"><span data-stu-id="9e1f4-274">✔</span></span>
<span data-ttu-id="9e1f4-275">Geometry. Екуалстопологикалли (геометрия)</span><span class="sxs-lookup"><span data-stu-id="9e1f4-275">Geometry.EqualsTopologically(Geometry)</span></span> | <span data-ttu-id="9e1f4-276">✔</span><span class="sxs-lookup"><span data-stu-id="9e1f4-276">✔</span></span> | <span data-ttu-id="9e1f4-277">✔</span><span class="sxs-lookup"><span data-stu-id="9e1f4-277">✔</span></span> | <span data-ttu-id="9e1f4-278">✔</span><span class="sxs-lookup"><span data-stu-id="9e1f4-278">✔</span></span> | <span data-ttu-id="9e1f4-279">✔</span><span class="sxs-lookup"><span data-stu-id="9e1f4-279">✔</span></span>
<span data-ttu-id="9e1f4-280">Geometry. Жеометритипе</span><span class="sxs-lookup"><span data-stu-id="9e1f4-280">Geometry.GeometryType</span></span> | <span data-ttu-id="9e1f4-281">✔</span><span class="sxs-lookup"><span data-stu-id="9e1f4-281">✔</span></span> | <span data-ttu-id="9e1f4-282">✔</span><span class="sxs-lookup"><span data-stu-id="9e1f4-282">✔</span></span> | <span data-ttu-id="9e1f4-283">✔</span><span class="sxs-lookup"><span data-stu-id="9e1f4-283">✔</span></span> | <span data-ttu-id="9e1f4-284">✔</span><span class="sxs-lookup"><span data-stu-id="9e1f4-284">✔</span></span>
<span data-ttu-id="9e1f4-285">Geometry. Жетжеометрин (int)</span><span class="sxs-lookup"><span data-stu-id="9e1f4-285">Geometry.GetGeometryN(int)</span></span> | <span data-ttu-id="9e1f4-286">✔</span><span class="sxs-lookup"><span data-stu-id="9e1f4-286">✔</span></span> | | <span data-ttu-id="9e1f4-287">✔</span><span class="sxs-lookup"><span data-stu-id="9e1f4-287">✔</span></span> | <span data-ttu-id="9e1f4-288">✔</span><span class="sxs-lookup"><span data-stu-id="9e1f4-288">✔</span></span>
<span data-ttu-id="9e1f4-289">Geometry. Интериорпоинт</span><span class="sxs-lookup"><span data-stu-id="9e1f4-289">Geometry.InteriorPoint</span></span> | <span data-ttu-id="9e1f4-290">✔</span><span class="sxs-lookup"><span data-stu-id="9e1f4-290">✔</span></span> | | <span data-ttu-id="9e1f4-291">✔</span><span class="sxs-lookup"><span data-stu-id="9e1f4-291">✔</span></span> | <span data-ttu-id="9e1f4-292">✔</span><span class="sxs-lookup"><span data-stu-id="9e1f4-292">✔</span></span>
<span data-ttu-id="9e1f4-293">Geometry. пересечение (Geometry)</span><span class="sxs-lookup"><span data-stu-id="9e1f4-293">Geometry.Intersection(Geometry)</span></span> | <span data-ttu-id="9e1f4-294">✔</span><span class="sxs-lookup"><span data-stu-id="9e1f4-294">✔</span></span> | <span data-ttu-id="9e1f4-295">✔</span><span class="sxs-lookup"><span data-stu-id="9e1f4-295">✔</span></span> | <span data-ttu-id="9e1f4-296">✔</span><span class="sxs-lookup"><span data-stu-id="9e1f4-296">✔</span></span> | <span data-ttu-id="9e1f4-297">✔</span><span class="sxs-lookup"><span data-stu-id="9e1f4-297">✔</span></span>
<span data-ttu-id="9e1f4-298">Геометрию. intersects (геометрия)</span><span class="sxs-lookup"><span data-stu-id="9e1f4-298">Geometry.Intersects(Geometry)</span></span> | <span data-ttu-id="9e1f4-299">✔</span><span class="sxs-lookup"><span data-stu-id="9e1f4-299">✔</span></span> | <span data-ttu-id="9e1f4-300">✔</span><span class="sxs-lookup"><span data-stu-id="9e1f4-300">✔</span></span> | <span data-ttu-id="9e1f4-301">✔</span><span class="sxs-lookup"><span data-stu-id="9e1f4-301">✔</span></span> | <span data-ttu-id="9e1f4-302">✔</span><span class="sxs-lookup"><span data-stu-id="9e1f4-302">✔</span></span>
<span data-ttu-id="9e1f4-303">Geometry. isEmpty</span><span class="sxs-lookup"><span data-stu-id="9e1f4-303">Geometry.IsEmpty</span></span> | <span data-ttu-id="9e1f4-304">✔</span><span class="sxs-lookup"><span data-stu-id="9e1f4-304">✔</span></span> | <span data-ttu-id="9e1f4-305">✔</span><span class="sxs-lookup"><span data-stu-id="9e1f4-305">✔</span></span> | <span data-ttu-id="9e1f4-306">✔</span><span class="sxs-lookup"><span data-stu-id="9e1f4-306">✔</span></span> | <span data-ttu-id="9e1f4-307">✔</span><span class="sxs-lookup"><span data-stu-id="9e1f4-307">✔</span></span>
<span data-ttu-id="9e1f4-308">Geometry. Простое_имя</span><span class="sxs-lookup"><span data-stu-id="9e1f4-308">Geometry.IsSimple</span></span> | <span data-ttu-id="9e1f4-309">✔</span><span class="sxs-lookup"><span data-stu-id="9e1f4-309">✔</span></span> | | <span data-ttu-id="9e1f4-310">✔</span><span class="sxs-lookup"><span data-stu-id="9e1f4-310">✔</span></span> | <span data-ttu-id="9e1f4-311">✔</span><span class="sxs-lookup"><span data-stu-id="9e1f4-311">✔</span></span>
<span data-ttu-id="9e1f4-312">Geometry. IsValid</span><span class="sxs-lookup"><span data-stu-id="9e1f4-312">Geometry.IsValid</span></span> | <span data-ttu-id="9e1f4-313">✔</span><span class="sxs-lookup"><span data-stu-id="9e1f4-313">✔</span></span> | <span data-ttu-id="9e1f4-314">✔</span><span class="sxs-lookup"><span data-stu-id="9e1f4-314">✔</span></span> | <span data-ttu-id="9e1f4-315">✔</span><span class="sxs-lookup"><span data-stu-id="9e1f4-315">✔</span></span> | <span data-ttu-id="9e1f4-316">✔</span><span class="sxs-lookup"><span data-stu-id="9e1f4-316">✔</span></span>
<span data-ttu-id="9e1f4-317">Geometry. Исвисиндистанце (геометрия, Double)</span><span class="sxs-lookup"><span data-stu-id="9e1f4-317">Geometry.IsWithinDistance(Geometry, double)</span></span> | <span data-ttu-id="9e1f4-318">✔</span><span class="sxs-lookup"><span data-stu-id="9e1f4-318">✔</span></span> | | <span data-ttu-id="9e1f4-319">✔</span><span class="sxs-lookup"><span data-stu-id="9e1f4-319">✔</span></span> | <span data-ttu-id="9e1f4-320">✔</span><span class="sxs-lookup"><span data-stu-id="9e1f4-320">✔</span></span>
<span data-ttu-id="9e1f4-321">Геометрия геометрии. length</span><span class="sxs-lookup"><span data-stu-id="9e1f4-321">Geometry.Length</span></span> | <span data-ttu-id="9e1f4-322">✔</span><span class="sxs-lookup"><span data-stu-id="9e1f4-322">✔</span></span> | <span data-ttu-id="9e1f4-323">✔</span><span class="sxs-lookup"><span data-stu-id="9e1f4-323">✔</span></span> | <span data-ttu-id="9e1f4-324">✔</span><span class="sxs-lookup"><span data-stu-id="9e1f4-324">✔</span></span> | <span data-ttu-id="9e1f4-325">✔</span><span class="sxs-lookup"><span data-stu-id="9e1f4-325">✔</span></span>
<span data-ttu-id="9e1f4-326">Geometry. Нумжеометриес</span><span class="sxs-lookup"><span data-stu-id="9e1f4-326">Geometry.NumGeometries</span></span> | <span data-ttu-id="9e1f4-327">✔</span><span class="sxs-lookup"><span data-stu-id="9e1f4-327">✔</span></span> | <span data-ttu-id="9e1f4-328">✔</span><span class="sxs-lookup"><span data-stu-id="9e1f4-328">✔</span></span> | <span data-ttu-id="9e1f4-329">✔</span><span class="sxs-lookup"><span data-stu-id="9e1f4-329">✔</span></span> | <span data-ttu-id="9e1f4-330">✔</span><span class="sxs-lookup"><span data-stu-id="9e1f4-330">✔</span></span>
<span data-ttu-id="9e1f4-331">Geometry. Нумпоинтс</span><span class="sxs-lookup"><span data-stu-id="9e1f4-331">Geometry.NumPoints</span></span> | <span data-ttu-id="9e1f4-332">✔</span><span class="sxs-lookup"><span data-stu-id="9e1f4-332">✔</span></span> | <span data-ttu-id="9e1f4-333">✔</span><span class="sxs-lookup"><span data-stu-id="9e1f4-333">✔</span></span> | <span data-ttu-id="9e1f4-334">✔</span><span class="sxs-lookup"><span data-stu-id="9e1f4-334">✔</span></span> | <span data-ttu-id="9e1f4-335">✔</span><span class="sxs-lookup"><span data-stu-id="9e1f4-335">✔</span></span>
<span data-ttu-id="9e1f4-336">Geometry. Огкжеометритипе</span><span class="sxs-lookup"><span data-stu-id="9e1f4-336">Geometry.OgcGeometryType</span></span> | <span data-ttu-id="9e1f4-337">✔</span><span class="sxs-lookup"><span data-stu-id="9e1f4-337">✔</span></span> | <span data-ttu-id="9e1f4-338">✔</span><span class="sxs-lookup"><span data-stu-id="9e1f4-338">✔</span></span> | <span data-ttu-id="9e1f4-339">✔</span><span class="sxs-lookup"><span data-stu-id="9e1f4-339">✔</span></span> | <span data-ttu-id="9e1f4-340">✔</span><span class="sxs-lookup"><span data-stu-id="9e1f4-340">✔</span></span>
<span data-ttu-id="9e1f4-341">Геометрия Geometry. пересекающих объектов (Geometry)</span><span class="sxs-lookup"><span data-stu-id="9e1f4-341">Geometry.Overlaps(Geometry)</span></span> | <span data-ttu-id="9e1f4-342">✔</span><span class="sxs-lookup"><span data-stu-id="9e1f4-342">✔</span></span> | <span data-ttu-id="9e1f4-343">✔</span><span class="sxs-lookup"><span data-stu-id="9e1f4-343">✔</span></span> | <span data-ttu-id="9e1f4-344">✔</span><span class="sxs-lookup"><span data-stu-id="9e1f4-344">✔</span></span> | <span data-ttu-id="9e1f4-345">✔</span><span class="sxs-lookup"><span data-stu-id="9e1f4-345">✔</span></span>
<span data-ttu-id="9e1f4-346">Geometry. Поинтонсурфаце</span><span class="sxs-lookup"><span data-stu-id="9e1f4-346">Geometry.PointOnSurface</span></span> | <span data-ttu-id="9e1f4-347">✔</span><span class="sxs-lookup"><span data-stu-id="9e1f4-347">✔</span></span> | | <span data-ttu-id="9e1f4-348">✔</span><span class="sxs-lookup"><span data-stu-id="9e1f4-348">✔</span></span> | <span data-ttu-id="9e1f4-349">✔</span><span class="sxs-lookup"><span data-stu-id="9e1f4-349">✔</span></span>
<span data-ttu-id="9e1f4-350">Geometry. Late (геометрия, строка)</span><span class="sxs-lookup"><span data-stu-id="9e1f4-350">Geometry.Relate(Geometry, string)</span></span> | <span data-ttu-id="9e1f4-351">✔</span><span class="sxs-lookup"><span data-stu-id="9e1f4-351">✔</span></span> | | <span data-ttu-id="9e1f4-352">✔</span><span class="sxs-lookup"><span data-stu-id="9e1f4-352">✔</span></span> | <span data-ttu-id="9e1f4-353">✔</span><span class="sxs-lookup"><span data-stu-id="9e1f4-353">✔</span></span>
<span data-ttu-id="9e1f4-354">Геометрия. Reverse ()</span><span class="sxs-lookup"><span data-stu-id="9e1f4-354">Geometry.Reverse()</span></span> | | | <span data-ttu-id="9e1f4-355">✔</span><span class="sxs-lookup"><span data-stu-id="9e1f4-355">✔</span></span> | <span data-ttu-id="9e1f4-356">✔</span><span class="sxs-lookup"><span data-stu-id="9e1f4-356">✔</span></span>
<span data-ttu-id="9e1f4-357">Geometry. SRID</span><span class="sxs-lookup"><span data-stu-id="9e1f4-357">Geometry.SRID</span></span> | <span data-ttu-id="9e1f4-358">✔</span><span class="sxs-lookup"><span data-stu-id="9e1f4-358">✔</span></span> | <span data-ttu-id="9e1f4-359">✔</span><span class="sxs-lookup"><span data-stu-id="9e1f4-359">✔</span></span> | <span data-ttu-id="9e1f4-360">✔</span><span class="sxs-lookup"><span data-stu-id="9e1f4-360">✔</span></span> | <span data-ttu-id="9e1f4-361">✔</span><span class="sxs-lookup"><span data-stu-id="9e1f4-361">✔</span></span>
<span data-ttu-id="9e1f4-362">Geometry. Симметрикдифференце (геометрия)</span><span class="sxs-lookup"><span data-stu-id="9e1f4-362">Geometry.SymmetricDifference(Geometry)</span></span> | <span data-ttu-id="9e1f4-363">✔</span><span class="sxs-lookup"><span data-stu-id="9e1f4-363">✔</span></span> | <span data-ttu-id="9e1f4-364">✔</span><span class="sxs-lookup"><span data-stu-id="9e1f4-364">✔</span></span> | <span data-ttu-id="9e1f4-365">✔</span><span class="sxs-lookup"><span data-stu-id="9e1f4-365">✔</span></span> | <span data-ttu-id="9e1f4-366">✔</span><span class="sxs-lookup"><span data-stu-id="9e1f4-366">✔</span></span>
<span data-ttu-id="9e1f4-367">Geometry. Тобинари ()</span><span class="sxs-lookup"><span data-stu-id="9e1f4-367">Geometry.ToBinary()</span></span> | <span data-ttu-id="9e1f4-368">✔</span><span class="sxs-lookup"><span data-stu-id="9e1f4-368">✔</span></span> | <span data-ttu-id="9e1f4-369">✔</span><span class="sxs-lookup"><span data-stu-id="9e1f4-369">✔</span></span> | <span data-ttu-id="9e1f4-370">✔</span><span class="sxs-lookup"><span data-stu-id="9e1f4-370">✔</span></span> | <span data-ttu-id="9e1f4-371">✔</span><span class="sxs-lookup"><span data-stu-id="9e1f4-371">✔</span></span>
<span data-ttu-id="9e1f4-372">Geometry. Тотекст ()</span><span class="sxs-lookup"><span data-stu-id="9e1f4-372">Geometry.ToText()</span></span> | <span data-ttu-id="9e1f4-373">✔</span><span class="sxs-lookup"><span data-stu-id="9e1f4-373">✔</span></span> | <span data-ttu-id="9e1f4-374">✔</span><span class="sxs-lookup"><span data-stu-id="9e1f4-374">✔</span></span> | <span data-ttu-id="9e1f4-375">✔</span><span class="sxs-lookup"><span data-stu-id="9e1f4-375">✔</span></span> | <span data-ttu-id="9e1f4-376">✔</span><span class="sxs-lookup"><span data-stu-id="9e1f4-376">✔</span></span>
<span data-ttu-id="9e1f4-377">Geometry. касания (геометрия)</span><span class="sxs-lookup"><span data-stu-id="9e1f4-377">Geometry.Touches(Geometry)</span></span> | <span data-ttu-id="9e1f4-378">✔</span><span class="sxs-lookup"><span data-stu-id="9e1f4-378">✔</span></span> | | <span data-ttu-id="9e1f4-379">✔</span><span class="sxs-lookup"><span data-stu-id="9e1f4-379">✔</span></span> | <span data-ttu-id="9e1f4-380">✔</span><span class="sxs-lookup"><span data-stu-id="9e1f4-380">✔</span></span>
<span data-ttu-id="9e1f4-381">Geometry. Union ()</span><span class="sxs-lookup"><span data-stu-id="9e1f4-381">Geometry.Union()</span></span> | | | <span data-ttu-id="9e1f4-382">✔</span><span class="sxs-lookup"><span data-stu-id="9e1f4-382">✔</span></span> | <span data-ttu-id="9e1f4-383">✔</span><span class="sxs-lookup"><span data-stu-id="9e1f4-383">✔</span></span>
<span data-ttu-id="9e1f4-384">Geometry. Union (геометрия)</span><span class="sxs-lookup"><span data-stu-id="9e1f4-384">Geometry.Union(Geometry)</span></span> | <span data-ttu-id="9e1f4-385">✔</span><span class="sxs-lookup"><span data-stu-id="9e1f4-385">✔</span></span> | <span data-ttu-id="9e1f4-386">✔</span><span class="sxs-lookup"><span data-stu-id="9e1f4-386">✔</span></span> | <span data-ttu-id="9e1f4-387">✔</span><span class="sxs-lookup"><span data-stu-id="9e1f4-387">✔</span></span> | <span data-ttu-id="9e1f4-388">✔</span><span class="sxs-lookup"><span data-stu-id="9e1f4-388">✔</span></span>
<span data-ttu-id="9e1f4-389">Геометрия. внутри (Geometry)</span><span class="sxs-lookup"><span data-stu-id="9e1f4-389">Geometry.Within(Geometry)</span></span> | <span data-ttu-id="9e1f4-390">✔</span><span class="sxs-lookup"><span data-stu-id="9e1f4-390">✔</span></span> | <span data-ttu-id="9e1f4-391">✔</span><span class="sxs-lookup"><span data-stu-id="9e1f4-391">✔</span></span> | <span data-ttu-id="9e1f4-392">✔</span><span class="sxs-lookup"><span data-stu-id="9e1f4-392">✔</span></span> | <span data-ttu-id="9e1f4-393">✔</span><span class="sxs-lookup"><span data-stu-id="9e1f4-393">✔</span></span>
<span data-ttu-id="9e1f4-394">GeometryCollection. Count</span><span class="sxs-lookup"><span data-stu-id="9e1f4-394">GeometryCollection.Count</span></span> | <span data-ttu-id="9e1f4-395">✔</span><span class="sxs-lookup"><span data-stu-id="9e1f4-395">✔</span></span> | <span data-ttu-id="9e1f4-396">✔</span><span class="sxs-lookup"><span data-stu-id="9e1f4-396">✔</span></span> | <span data-ttu-id="9e1f4-397">✔</span><span class="sxs-lookup"><span data-stu-id="9e1f4-397">✔</span></span> | <span data-ttu-id="9e1f4-398">✔</span><span class="sxs-lookup"><span data-stu-id="9e1f4-398">✔</span></span>
<span data-ttu-id="9e1f4-399">GeometryCollection [int]</span><span class="sxs-lookup"><span data-stu-id="9e1f4-399">GeometryCollection[int]</span></span> | <span data-ttu-id="9e1f4-400">✔</span><span class="sxs-lookup"><span data-stu-id="9e1f4-400">✔</span></span> | <span data-ttu-id="9e1f4-401">✔</span><span class="sxs-lookup"><span data-stu-id="9e1f4-401">✔</span></span> | <span data-ttu-id="9e1f4-402">✔</span><span class="sxs-lookup"><span data-stu-id="9e1f4-402">✔</span></span> | <span data-ttu-id="9e1f4-403">✔</span><span class="sxs-lookup"><span data-stu-id="9e1f4-403">✔</span></span>
<span data-ttu-id="9e1f4-404">LineString. Count</span><span class="sxs-lookup"><span data-stu-id="9e1f4-404">LineString.Count</span></span> | <span data-ttu-id="9e1f4-405">✔</span><span class="sxs-lookup"><span data-stu-id="9e1f4-405">✔</span></span> | <span data-ttu-id="9e1f4-406">✔</span><span class="sxs-lookup"><span data-stu-id="9e1f4-406">✔</span></span> | <span data-ttu-id="9e1f4-407">✔</span><span class="sxs-lookup"><span data-stu-id="9e1f4-407">✔</span></span> | <span data-ttu-id="9e1f4-408">✔</span><span class="sxs-lookup"><span data-stu-id="9e1f4-408">✔</span></span>
<span data-ttu-id="9e1f4-409">LineString. EndPoint</span><span class="sxs-lookup"><span data-stu-id="9e1f4-409">LineString.EndPoint</span></span> | <span data-ttu-id="9e1f4-410">✔</span><span class="sxs-lookup"><span data-stu-id="9e1f4-410">✔</span></span> | <span data-ttu-id="9e1f4-411">✔</span><span class="sxs-lookup"><span data-stu-id="9e1f4-411">✔</span></span> | <span data-ttu-id="9e1f4-412">✔</span><span class="sxs-lookup"><span data-stu-id="9e1f4-412">✔</span></span> | <span data-ttu-id="9e1f4-413">✔</span><span class="sxs-lookup"><span data-stu-id="9e1f4-413">✔</span></span>
<span data-ttu-id="9e1f4-414">LineString. Жетпоинтн (int)</span><span class="sxs-lookup"><span data-stu-id="9e1f4-414">LineString.GetPointN(int)</span></span> | <span data-ttu-id="9e1f4-415">✔</span><span class="sxs-lookup"><span data-stu-id="9e1f4-415">✔</span></span> | <span data-ttu-id="9e1f4-416">✔</span><span class="sxs-lookup"><span data-stu-id="9e1f4-416">✔</span></span> | <span data-ttu-id="9e1f4-417">✔</span><span class="sxs-lookup"><span data-stu-id="9e1f4-417">✔</span></span> | <span data-ttu-id="9e1f4-418">✔</span><span class="sxs-lookup"><span data-stu-id="9e1f4-418">✔</span></span>
<span data-ttu-id="9e1f4-419">LineString. крыл</span><span class="sxs-lookup"><span data-stu-id="9e1f4-419">LineString.IsClosed</span></span> | <span data-ttu-id="9e1f4-420">✔</span><span class="sxs-lookup"><span data-stu-id="9e1f4-420">✔</span></span> | <span data-ttu-id="9e1f4-421">✔</span><span class="sxs-lookup"><span data-stu-id="9e1f4-421">✔</span></span> | <span data-ttu-id="9e1f4-422">✔</span><span class="sxs-lookup"><span data-stu-id="9e1f4-422">✔</span></span> | <span data-ttu-id="9e1f4-423">✔</span><span class="sxs-lookup"><span data-stu-id="9e1f4-423">✔</span></span>
<span data-ttu-id="9e1f4-424">LineString. Звонок</span><span class="sxs-lookup"><span data-stu-id="9e1f4-424">LineString.IsRing</span></span> | <span data-ttu-id="9e1f4-425">✔</span><span class="sxs-lookup"><span data-stu-id="9e1f4-425">✔</span></span> | | <span data-ttu-id="9e1f4-426">✔</span><span class="sxs-lookup"><span data-stu-id="9e1f4-426">✔</span></span> | <span data-ttu-id="9e1f4-427">✔</span><span class="sxs-lookup"><span data-stu-id="9e1f4-427">✔</span></span>
<span data-ttu-id="9e1f4-428">LineString. StartPoint</span><span class="sxs-lookup"><span data-stu-id="9e1f4-428">LineString.StartPoint</span></span> | <span data-ttu-id="9e1f4-429">✔</span><span class="sxs-lookup"><span data-stu-id="9e1f4-429">✔</span></span> | <span data-ttu-id="9e1f4-430">✔</span><span class="sxs-lookup"><span data-stu-id="9e1f4-430">✔</span></span> | <span data-ttu-id="9e1f4-431">✔</span><span class="sxs-lookup"><span data-stu-id="9e1f4-431">✔</span></span> | <span data-ttu-id="9e1f4-432">✔</span><span class="sxs-lookup"><span data-stu-id="9e1f4-432">✔</span></span>
<span data-ttu-id="9e1f4-433">MultiLineString. крыл</span><span class="sxs-lookup"><span data-stu-id="9e1f4-433">MultiLineString.IsClosed</span></span> | <span data-ttu-id="9e1f4-434">✔</span><span class="sxs-lookup"><span data-stu-id="9e1f4-434">✔</span></span> | <span data-ttu-id="9e1f4-435">✔</span><span class="sxs-lookup"><span data-stu-id="9e1f4-435">✔</span></span> | <span data-ttu-id="9e1f4-436">✔</span><span class="sxs-lookup"><span data-stu-id="9e1f4-436">✔</span></span> | <span data-ttu-id="9e1f4-437">✔</span><span class="sxs-lookup"><span data-stu-id="9e1f4-437">✔</span></span>
<span data-ttu-id="9e1f4-438">Point. M</span><span class="sxs-lookup"><span data-stu-id="9e1f4-438">Point.M</span></span> | <span data-ttu-id="9e1f4-439">✔</span><span class="sxs-lookup"><span data-stu-id="9e1f4-439">✔</span></span> | <span data-ttu-id="9e1f4-440">✔</span><span class="sxs-lookup"><span data-stu-id="9e1f4-440">✔</span></span> | <span data-ttu-id="9e1f4-441">✔</span><span class="sxs-lookup"><span data-stu-id="9e1f4-441">✔</span></span> | <span data-ttu-id="9e1f4-442">✔</span><span class="sxs-lookup"><span data-stu-id="9e1f4-442">✔</span></span>
<span data-ttu-id="9e1f4-443">Point. X</span><span class="sxs-lookup"><span data-stu-id="9e1f4-443">Point.X</span></span> | <span data-ttu-id="9e1f4-444">✔</span><span class="sxs-lookup"><span data-stu-id="9e1f4-444">✔</span></span> | <span data-ttu-id="9e1f4-445">✔</span><span class="sxs-lookup"><span data-stu-id="9e1f4-445">✔</span></span> | <span data-ttu-id="9e1f4-446">✔</span><span class="sxs-lookup"><span data-stu-id="9e1f4-446">✔</span></span> | <span data-ttu-id="9e1f4-447">✔</span><span class="sxs-lookup"><span data-stu-id="9e1f4-447">✔</span></span>
<span data-ttu-id="9e1f4-448">Point. Y</span><span class="sxs-lookup"><span data-stu-id="9e1f4-448">Point.Y</span></span> | <span data-ttu-id="9e1f4-449">✔</span><span class="sxs-lookup"><span data-stu-id="9e1f4-449">✔</span></span> | <span data-ttu-id="9e1f4-450">✔</span><span class="sxs-lookup"><span data-stu-id="9e1f4-450">✔</span></span> | <span data-ttu-id="9e1f4-451">✔</span><span class="sxs-lookup"><span data-stu-id="9e1f4-451">✔</span></span> | <span data-ttu-id="9e1f4-452">✔</span><span class="sxs-lookup"><span data-stu-id="9e1f4-452">✔</span></span>
<span data-ttu-id="9e1f4-453">Point. Z</span><span class="sxs-lookup"><span data-stu-id="9e1f4-453">Point.Z</span></span> | <span data-ttu-id="9e1f4-454">✔</span><span class="sxs-lookup"><span data-stu-id="9e1f4-454">✔</span></span> | <span data-ttu-id="9e1f4-455">✔</span><span class="sxs-lookup"><span data-stu-id="9e1f4-455">✔</span></span> | <span data-ttu-id="9e1f4-456">✔</span><span class="sxs-lookup"><span data-stu-id="9e1f4-456">✔</span></span> | <span data-ttu-id="9e1f4-457">✔</span><span class="sxs-lookup"><span data-stu-id="9e1f4-457">✔</span></span>
<span data-ttu-id="9e1f4-458">Polygon. Екстериорринг</span><span class="sxs-lookup"><span data-stu-id="9e1f4-458">Polygon.ExteriorRing</span></span> | <span data-ttu-id="9e1f4-459">✔</span><span class="sxs-lookup"><span data-stu-id="9e1f4-459">✔</span></span> | <span data-ttu-id="9e1f4-460">✔</span><span class="sxs-lookup"><span data-stu-id="9e1f4-460">✔</span></span> | <span data-ttu-id="9e1f4-461">✔</span><span class="sxs-lookup"><span data-stu-id="9e1f4-461">✔</span></span> | <span data-ttu-id="9e1f4-462">✔</span><span class="sxs-lookup"><span data-stu-id="9e1f4-462">✔</span></span>
<span data-ttu-id="9e1f4-463">Polygon. Жетинтериоррингн (int)</span><span class="sxs-lookup"><span data-stu-id="9e1f4-463">Polygon.GetInteriorRingN(int)</span></span> | <span data-ttu-id="9e1f4-464">✔</span><span class="sxs-lookup"><span data-stu-id="9e1f4-464">✔</span></span> | <span data-ttu-id="9e1f4-465">✔</span><span class="sxs-lookup"><span data-stu-id="9e1f4-465">✔</span></span> | <span data-ttu-id="9e1f4-466">✔</span><span class="sxs-lookup"><span data-stu-id="9e1f4-466">✔</span></span> | <span data-ttu-id="9e1f4-467">✔</span><span class="sxs-lookup"><span data-stu-id="9e1f4-467">✔</span></span>
<span data-ttu-id="9e1f4-468">Polygon. Нуминтериоррингс</span><span class="sxs-lookup"><span data-stu-id="9e1f4-468">Polygon.NumInteriorRings</span></span> | <span data-ttu-id="9e1f4-469">✔</span><span class="sxs-lookup"><span data-stu-id="9e1f4-469">✔</span></span> | <span data-ttu-id="9e1f4-470">✔</span><span class="sxs-lookup"><span data-stu-id="9e1f4-470">✔</span></span> | <span data-ttu-id="9e1f4-471">✔</span><span class="sxs-lookup"><span data-stu-id="9e1f4-471">✔</span></span> | <span data-ttu-id="9e1f4-472">✔</span><span class="sxs-lookup"><span data-stu-id="9e1f4-472">✔</span></span>

## <a name="additional-resources"></a><span data-ttu-id="9e1f4-473">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="9e1f4-473">Additional resources</span></span>

* [<span data-ttu-id="9e1f4-474">Пространственные данные в SQL Server</span><span class="sxs-lookup"><span data-stu-id="9e1f4-474">Spatial Data in SQL Server</span></span>](https://docs.microsoft.com/sql/relational-databases/spatial/spatial-data-sql-server)
* [<span data-ttu-id="9e1f4-475">Домашняя страница Спатиалите</span><span class="sxs-lookup"><span data-stu-id="9e1f4-475">SpatiaLite Homepage</span></span>](https://www.gaia-gis.it/fossil/libspatialite)
* [<span data-ttu-id="9e1f4-476">Документация по Npgsqlной пространственной документации</span><span class="sxs-lookup"><span data-stu-id="9e1f4-476">Npgsql Spatial Documentation</span></span>](https://www.npgsql.org/efcore/mapping/nts.html)
* [<span data-ttu-id="9e1f4-477">Документация по PostGIS</span><span class="sxs-lookup"><span data-stu-id="9e1f4-477">PostGIS Documentation</span></span>](https://postgis.net/documentation/)
