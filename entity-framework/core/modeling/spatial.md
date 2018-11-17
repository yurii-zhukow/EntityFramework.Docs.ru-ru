---
title: Пространственных данных — EF Core
author: bricelam
ms.author: bricelam
ms.date: 11/01/2018
ms.assetid: 2BDE29FC-4161-41A0-841E-69F51CCD9341
uid: core/modeling/spatial
ms.openlocfilehash: cf488c6b7d94ca19018efe1c23ff410fe7eb594b
ms.sourcegitcommit: 81c53ac43d8f15b900f117294ec71dc49fe028fa
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/16/2018
ms.locfileid: "51817914"
---
# <a name="spatial-data"></a><span data-ttu-id="c23c2-102">Пространственные данные</span><span class="sxs-lookup"><span data-stu-id="c23c2-102">Spatial Data</span></span>

> [!NOTE]
> <span data-ttu-id="c23c2-103">Этот компонент впервые появился в EF Core 2.2.</span><span class="sxs-lookup"><span data-stu-id="c23c2-103">This feature is new in EF Core 2.2.</span></span>

<span data-ttu-id="c23c2-104">Пространственные данные представляет физическое расположение и форму объектов.</span><span class="sxs-lookup"><span data-stu-id="c23c2-104">Spatial data represents the physical location and the shape of objects.</span></span> <span data-ttu-id="c23c2-105">Многие базы данных поддерживают этот тип данных, его можно индексировать и запрашивать вместе с другими данными.</span><span class="sxs-lookup"><span data-stu-id="c23c2-105">Many databases provide support for this type of data so it can be indexed and queried alongside other data.</span></span> <span data-ttu-id="c23c2-106">Обычные сценарии включают запрос для объектов в пределах заданного расстояния из расположения, или выберите объект, границу которого содержит заданное расположение.</span><span class="sxs-lookup"><span data-stu-id="c23c2-106">Common scenarios include querying for objects within a given distance from a location, or selecting the object whose border contains a given location.</span></span> <span data-ttu-id="c23c2-107">EF Core поддерживает сопоставление типов пространственных данных с использованием [NetTopologySuite](https://github.com/NetTopologySuite/NetTopologySuite) пространственной библиотеки.</span><span class="sxs-lookup"><span data-stu-id="c23c2-107">EF Core supports mapping to spatial data types using the [NetTopologySuite](https://github.com/NetTopologySuite/NetTopologySuite) spatial library.</span></span>

## <a name="installing"></a><span data-ttu-id="c23c2-108">Установка</span><span class="sxs-lookup"><span data-stu-id="c23c2-108">Installing</span></span>

<span data-ttu-id="c23c2-109">Чтобы использовать Пространственные данные с EF Core, необходимо установить соответствующий пакет поддержки NuGet.</span><span class="sxs-lookup"><span data-stu-id="c23c2-109">In order to use spatial data with EF Core, you need to install the appropriate supporting NuGet package.</span></span> <span data-ttu-id="c23c2-110">Какой пакет, необходимо установить зависит от поставщика, которую вы используете.</span><span class="sxs-lookup"><span data-stu-id="c23c2-110">Which package you need to install depends on the provider you're using.</span></span>

<span data-ttu-id="c23c2-111">Поставщик EF Core</span><span class="sxs-lookup"><span data-stu-id="c23c2-111">EF Core Provider</span></span>                        | <span data-ttu-id="c23c2-112">Пространственные пакета NuGet</span><span class="sxs-lookup"><span data-stu-id="c23c2-112">Spatial NuGet Package</span></span>
--------------------------------------- | ---------------------
<span data-ttu-id="c23c2-113">Microsoft.EntityFrameworkCore.SqlServer</span><span class="sxs-lookup"><span data-stu-id="c23c2-113">Microsoft.EntityFrameworkCore.SqlServer</span></span> | [<span data-ttu-id="c23c2-114">Microsoft.EntityFrameworkCore.SqlServer.NetTopologySuite</span><span class="sxs-lookup"><span data-stu-id="c23c2-114">Microsoft.EntityFrameworkCore.SqlServer.NetTopologySuite</span></span>](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer.NetTopologySuite)
<span data-ttu-id="c23c2-115">Microsoft.EntityFrameworkCore.Sqlite</span><span class="sxs-lookup"><span data-stu-id="c23c2-115">Microsoft.EntityFrameworkCore.Sqlite</span></span>    | [<span data-ttu-id="c23c2-116">Microsoft.EntityFrameworkCore.Sqlite.NetTopologySuite</span><span class="sxs-lookup"><span data-stu-id="c23c2-116">Microsoft.EntityFrameworkCore.Sqlite.NetTopologySuite</span></span>](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Sqlite.NetTopologySuite)
<span data-ttu-id="c23c2-117">Microsoft.EntityFrameworkCore.InMemory</span><span class="sxs-lookup"><span data-stu-id="c23c2-117">Microsoft.EntityFrameworkCore.InMemory</span></span>  | [<span data-ttu-id="c23c2-118">NetTopologySuite</span><span class="sxs-lookup"><span data-stu-id="c23c2-118">NetTopologySuite</span></span>](https://www.nuget.org/packages/NetTopologySuite)
<span data-ttu-id="c23c2-119">Npgsql.EntityFrameworkCore.PostgreSQL</span><span class="sxs-lookup"><span data-stu-id="c23c2-119">Npgsql.EntityFrameworkCore.PostgreSQL</span></span>   | [<span data-ttu-id="c23c2-120">Npgsql.EntityFrameworkCore.PostgreSQL.NetTopologySuite</span><span class="sxs-lookup"><span data-stu-id="c23c2-120">Npgsql.EntityFrameworkCore.PostgreSQL.NetTopologySuite</span></span>](https://www.nuget.org/packages/Npgsql.EntityFrameworkCore.PostgreSQL.NetTopologySuite)

## <a name="reverse-engineering"></a><span data-ttu-id="c23c2-121">Реконструирование</span><span class="sxs-lookup"><span data-stu-id="c23c2-121">Reverse engineering</span></span>

<span data-ttu-id="c23c2-122">Пространственные NuGet также пакеты enable [реконструирование](../managing-schemas/scaffolding.md) моделей с пространственных свойств, но вам нужно установить пакет ***перед*** под управлением `Scaffold-DbContext` или `dotnet ef dbcontext scaffold`.</span><span class="sxs-lookup"><span data-stu-id="c23c2-122">The spatial NuGet packages also enable [reverse engineering](../managing-schemas/scaffolding.md) models with spatial properties, but you need to install the package ***before*** running `Scaffold-DbContext` or `dotnet ef dbcontext scaffold`.</span></span> <span data-ttu-id="c23c2-123">Если этого не сделать, вы будете получать предупреждения о не находит сопоставления типов для столбцов, а столбцы будут пропущены.</span><span class="sxs-lookup"><span data-stu-id="c23c2-123">If you don't, you'll receive warnings about not finding type mappings for the columns and the columns will be skipped.</span></span>

## <a name="nettopologysuite-nts"></a><span data-ttu-id="c23c2-124">NetTopologySuite (NTS)</span><span class="sxs-lookup"><span data-stu-id="c23c2-124">NetTopologySuite (NTS)</span></span>

<span data-ttu-id="c23c2-125">NetTopologySuite представляет собой пространственных библиотеку для .NET.</span><span class="sxs-lookup"><span data-stu-id="c23c2-125">NetTopologySuite is a spatial library for .NET.</span></span> <span data-ttu-id="c23c2-126">EF Core обеспечивает сопоставление пространственных данных типов в базе данных с помощью NTS типов в модели.</span><span class="sxs-lookup"><span data-stu-id="c23c2-126">EF Core enables mapping to spatial data types in the database by using NTS types in your model.</span></span>

<span data-ttu-id="c23c2-127">Чтобы включить сопоставление для пространственных типов с помощью NTS, вызовите метод UseNetTopologySuite построитель параметры поставщика DbContext.</span><span class="sxs-lookup"><span data-stu-id="c23c2-127">To enable mapping to spatial types via NTS, call the UseNetTopologySuite method on the provider's DbContext options builder.</span></span> <span data-ttu-id="c23c2-128">К примеру используя SQL Server можно будет вызвать следующим образом.</span><span class="sxs-lookup"><span data-stu-id="c23c2-128">For example, with SQL Server you'd call it like this.</span></span>

``` csharp
optionsBuilder.UseSqlServer(
    @"Data Source=(localdb)\MSSQLLocalDB;Initial Catalog=WideWorldImporters",
    x => x.UseNetTopologySuite());
```

<span data-ttu-id="c23c2-129">Существует несколько типов пространственных данных.</span><span class="sxs-lookup"><span data-stu-id="c23c2-129">There are several spatial data types.</span></span> <span data-ttu-id="c23c2-130">Какой тип использовать зависит от типов фигур, которые вы хотите разрешить.</span><span class="sxs-lookup"><span data-stu-id="c23c2-130">Which type you use depends on the types of shapes you want to allow.</span></span> <span data-ttu-id="c23c2-131">Вот иерархии NTS типов, которые можно использовать для свойств в модели.</span><span class="sxs-lookup"><span data-stu-id="c23c2-131">Here is the hierarchy of NTS types that you can use for properties in your model.</span></span> <span data-ttu-id="c23c2-132">Они находятся в пределах `NetTopologySuite.Geometries` пространства имен.</span><span class="sxs-lookup"><span data-stu-id="c23c2-132">They're located within the `NetTopologySuite.Geometries` namespace.</span></span> <span data-ttu-id="c23c2-133">Соответствующие интерфейсы в пакете GeoAPI (`GeoAPI.Geometries` пространства имен) также можно использовать.</span><span class="sxs-lookup"><span data-stu-id="c23c2-133">Corresponding interfaces in the GeoAPI package (`GeoAPI.Geometries` namespace) can also be used.</span></span>

* <span data-ttu-id="c23c2-134">Geometry</span><span class="sxs-lookup"><span data-stu-id="c23c2-134">Geometry</span></span>
  * <span data-ttu-id="c23c2-135">Точка</span><span class="sxs-lookup"><span data-stu-id="c23c2-135">Point</span></span>
  * <span data-ttu-id="c23c2-136">LineString</span><span class="sxs-lookup"><span data-stu-id="c23c2-136">LineString</span></span>
  * <span data-ttu-id="c23c2-137">Polygon</span><span class="sxs-lookup"><span data-stu-id="c23c2-137">Polygon</span></span>
  * <span data-ttu-id="c23c2-138">GeometryCollection</span><span class="sxs-lookup"><span data-stu-id="c23c2-138">GeometryCollection</span></span>
    * <span data-ttu-id="c23c2-139">MultiPoint</span><span class="sxs-lookup"><span data-stu-id="c23c2-139">MultiPoint</span></span>
    * <span data-ttu-id="c23c2-140">MultiLineString</span><span class="sxs-lookup"><span data-stu-id="c23c2-140">MultiLineString</span></span>
    * <span data-ttu-id="c23c2-141">MultiPolygon</span><span class="sxs-lookup"><span data-stu-id="c23c2-141">MultiPolygon</span></span>

> [!WARNING]
> <span data-ttu-id="c23c2-142">CircularString, CompoundCurve и CurePolygon не поддерживаются NTS.</span><span class="sxs-lookup"><span data-stu-id="c23c2-142">CircularString, CompoundCurve, and CurePolygon aren't supported by NTS.</span></span>

<span data-ttu-id="c23c2-143">С помощью базовый тип геометрического объекта любого типа фигуры, указанного в свойстве.</span><span class="sxs-lookup"><span data-stu-id="c23c2-143">Using the base Geometry type allows any type of shape to be specified by the property.</span></span>

<span data-ttu-id="c23c2-144">Следующие классы сущностей может использоваться для сопоставления с таблицами в [образца базы данных Wide World Importers](http://go.microsoft.com/fwlink/?LinkID=800630).</span><span class="sxs-lookup"><span data-stu-id="c23c2-144">The following entity classes could be used to map to tables in the [Wide World Importers sample database](http://go.microsoft.com/fwlink/?LinkID=800630).</span></span>

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

### <a name="creating-values"></a><span data-ttu-id="c23c2-145">Создание значения</span><span class="sxs-lookup"><span data-stu-id="c23c2-145">Creating values</span></span>

<span data-ttu-id="c23c2-146">Конструкторы можно использовать для создания геометрических объектов; Тем не менее NTS рекомендует вместо этого использовать фабрику geometry.</span><span class="sxs-lookup"><span data-stu-id="c23c2-146">You can use constructors to create geometry objects; however, NTS recommends using a geometry factory instead.</span></span> <span data-ttu-id="c23c2-147">Это позволяет задать значение по умолчанию SRID (пространственную систему, используемых координаты) и предоставляет контроль над более сложных такие вещи, как точность модели (используется во время вычисления) и координат последовательности (определяет, какие ordinates--измерений и показателями — существуют доступны).</span><span class="sxs-lookup"><span data-stu-id="c23c2-147">This lets you specify a default SRID (the spatial reference system used by the coordinates) and gives you control over more advanced things like the precision model (used during calculations) and the coordinate sequence (determines which ordinates--dimensions and measures--are available).</span></span>

``` csharp
var geometryFactory = NtsGeometryServices.Instance.CreateGeometryFactory(srid: 4326);
var currentLocation = geometryFactory.CreatePoint(-122.121512, 47.6739882);
```

> [!NOTE]
> <span data-ttu-id="c23c2-148">4326 ссылается на WGS 84, стандартом GPS и других географических систем.</span><span class="sxs-lookup"><span data-stu-id="c23c2-148">4326 refers to WGS 84, a standard used in GPS and other geographic systems.</span></span>

### <a name="longitude-and-latitude"></a><span data-ttu-id="c23c2-149">Широту и долготу</span><span class="sxs-lookup"><span data-stu-id="c23c2-149">Longitude and Latitude</span></span>

<span data-ttu-id="c23c2-150">Координаты в NTS, указываются в виде значения X и Y.</span><span class="sxs-lookup"><span data-stu-id="c23c2-150">Coordinates in NTS are in terms of X and Y values.</span></span> <span data-ttu-id="c23c2-151">Для представления, широту и долготу, используйте X для широты и Y для широты.</span><span class="sxs-lookup"><span data-stu-id="c23c2-151">To represent longitude and latitude, use X for longitude and Y for latitude.</span></span> <span data-ttu-id="c23c2-152">Обратите внимание, что это **обратной** из `latitude, longitude` формат, в котором обычно увидеть эти значения.</span><span class="sxs-lookup"><span data-stu-id="c23c2-152">Note that this is **backwards** from the `latitude, longitude` format in which you typically see these values.</span></span>

### <a name="srid-ignored-during-client-operations"></a><span data-ttu-id="c23c2-153">SRID пропущены во время операции клиента</span><span class="sxs-lookup"><span data-stu-id="c23c2-153">SRID Ignored during client operations</span></span>

<span data-ttu-id="c23c2-154">NTS SRID значения игнорируются во время операций.</span><span class="sxs-lookup"><span data-stu-id="c23c2-154">NTS ignores SRID values during operations.</span></span> <span data-ttu-id="c23c2-155">Предполагается планарная система координат.</span><span class="sxs-lookup"><span data-stu-id="c23c2-155">It assumes a planar coordinate system.</span></span> <span data-ttu-id="c23c2-156">Это означает, что если указать координаты с точки зрения широту и долготу, некоторые значения клиента вычисляется как расстояние, длина и области будут в градусах, не счетчики.</span><span class="sxs-lookup"><span data-stu-id="c23c2-156">This means that if you specify coordinates in terms of longitude and latitude, some client-evaluated values like distance, length, and area will be in degrees, not meters.</span></span> <span data-ttu-id="c23c2-157">Для более осмысленными значениями, необходимо сначала проект координат в другую систему координат, используя библиотеку как [ProjNet4GeoAPI](https://github.com/NetTopologySuite/ProjNet4GeoAPI) перед вычисление этих значений.</span><span class="sxs-lookup"><span data-stu-id="c23c2-157">For more meaningful values, you first need to project the coordinates to another coordinate system using a library like [ProjNet4GeoAPI](https://github.com/NetTopologySuite/ProjNet4GeoAPI) before calculating these values.</span></span>

<span data-ttu-id="c23c2-158">Если операция выполняется на сервере оцениваются в EF Core с помощью SQL, единицы результат будет определяться в базе данных.</span><span class="sxs-lookup"><span data-stu-id="c23c2-158">If an operation is server-evaluated by EF Core via SQL, the result's unit will be determined by the database.</span></span>

<span data-ttu-id="c23c2-159">Ниже приведен пример использования ProjNet4GeoAPI для вычисления расстояния между двумя городами.</span><span class="sxs-lookup"><span data-stu-id="c23c2-159">Here is an example of using ProjNet4GeoAPI to calculate the distance between two cities.</span></span>

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

## <a name="querying-data"></a><span data-ttu-id="c23c2-160">Запросы к данным</span><span class="sxs-lookup"><span data-stu-id="c23c2-160">Querying Data</span></span>

<span data-ttu-id="c23c2-161">NTS методы и свойства, доступные как функции базы данных будут переведены в LINQ to SQL.</span><span class="sxs-lookup"><span data-stu-id="c23c2-161">In LINQ, the NTS methods and properties available as database functions will be translated to SQL.</span></span> <span data-ttu-id="c23c2-162">Например расстояние и содержит методы, преобразуются в следующих запросах.</span><span class="sxs-lookup"><span data-stu-id="c23c2-162">For example, the Distance and Contains methods are translated in the following queries.</span></span> <span data-ttu-id="c23c2-163">В таблице в конце этой статьи показано, какие члены поддерживаются различные поставщики EF Core.</span><span class="sxs-lookup"><span data-stu-id="c23c2-163">The table at the end of this article shows which members are supported by various EF Core providers.</span></span>

``` csharp
var nearestCity = db.Cities
    .OrderBy(c => c.Location.Distance(currentLocation))
    .FirstOrDefault();

var currentCountry = db.Countries
    .FirstOrDefault(c => c.Border.Contains(currentLocation));
```

## <a name="sql-server"></a><span data-ttu-id="c23c2-164">SQL Server</span><span class="sxs-lookup"><span data-stu-id="c23c2-164">SQL Server</span></span>

<span data-ttu-id="c23c2-165">Если вы используете SQL Server, существуют некоторые дополнительные действия, которые следует учитывать.</span><span class="sxs-lookup"><span data-stu-id="c23c2-165">If you're using SQL Server, there are some additional things you should be aware of.</span></span>

### <a name="geography-or-geometry"></a><span data-ttu-id="c23c2-166">Географическому или геометрическому объекту</span><span class="sxs-lookup"><span data-stu-id="c23c2-166">Geography or geometry</span></span>

<span data-ttu-id="c23c2-167">По умолчанию Пространственные свойства сопоставляются `geography` столбцы в SQL Server.</span><span class="sxs-lookup"><span data-stu-id="c23c2-167">By default, spatial properties are mapped to `geography` columns in SQL Server.</span></span> <span data-ttu-id="c23c2-168">Чтобы использовать `geometry`, [настроить тип столбца](xref:core/modeling/relational/data-types) в модели.</span><span class="sxs-lookup"><span data-stu-id="c23c2-168">To use `geometry`, [configure the column type](xref:core/modeling/relational/data-types) in your model.</span></span>

### <a name="geography-polygon-rings"></a><span data-ttu-id="c23c2-169">Кольца многоугольника Geography</span><span class="sxs-lookup"><span data-stu-id="c23c2-169">Geography polygon rings</span></span>

<span data-ttu-id="c23c2-170">При использовании `geography` тип столбца, SQL Server налагает дополнительные требования на внешнее кольцо (или оболочки) и внутренних колец (или бреши в системе).</span><span class="sxs-lookup"><span data-stu-id="c23c2-170">When using the `geography` column type, SQL Server imposes additional requirements on the exterior ring (or shell) and interior rings (or holes).</span></span> <span data-ttu-id="c23c2-171">Внешнее кольцо против часовой стрелки должен быть ориентирован и внутренняя область кольцами по часовой стрелке.</span><span class="sxs-lookup"><span data-stu-id="c23c2-171">The exterior ring must be oriented counterclockwise and the interior rings clockwise.</span></span> <span data-ttu-id="c23c2-172">NTS проверяет это перед отправкой значения в базе данных.</span><span class="sxs-lookup"><span data-stu-id="c23c2-172">NTS validates this before sending values to the database.</span></span>

### <a name="fullglobe"></a><span data-ttu-id="c23c2-173">FullGlobe</span><span class="sxs-lookup"><span data-stu-id="c23c2-173">FullGlobe</span></span>

<span data-ttu-id="c23c2-174">SQL Server имеет нестандартные геометрический тип для представления полного земного шара при использовании `geography` тип столбца.</span><span class="sxs-lookup"><span data-stu-id="c23c2-174">SQL Server has a non-standard geometry type to represent the full globe when using the `geography` column type.</span></span> <span data-ttu-id="c23c2-175">Он также содержит способ представления многоугольников основании полного земного шара (без внешним кольцом).</span><span class="sxs-lookup"><span data-stu-id="c23c2-175">It also has a way to represent polygons based on the full globe (without an exterior ring).</span></span> <span data-ttu-id="c23c2-176">Ни один из этих поддерживаемых NTS.</span><span class="sxs-lookup"><span data-stu-id="c23c2-176">Neither of these are supported by NTS.</span></span>

> [!WARNING]
> <span data-ttu-id="c23c2-177">NTS не поддерживает FullGlobe и многоугольников на его основе.</span><span class="sxs-lookup"><span data-stu-id="c23c2-177">FullGlobe and polygons based on it aren't supported by NTS.</span></span>

## <a name="sqlite"></a><span data-ttu-id="c23c2-178">SQLite</span><span class="sxs-lookup"><span data-stu-id="c23c2-178">SQLite</span></span>

<span data-ttu-id="c23c2-179">Вот некоторые дополнительные сведения для тех, с помощью SQLite.</span><span class="sxs-lookup"><span data-stu-id="c23c2-179">Here is some additional information for those using SQLite.</span></span>

### <a name="installing-spatialite"></a><span data-ttu-id="c23c2-180">Установка SpatiaLite</span><span class="sxs-lookup"><span data-stu-id="c23c2-180">Installing SpatiaLite</span></span>

<span data-ttu-id="c23c2-181">В Windows библиотеки собственного mod_spatialite распространяется в виде зависимость пакета NuGet.</span><span class="sxs-lookup"><span data-stu-id="c23c2-181">On Windows, the native mod_spatialite library is distributed as a NuGet package dependency.</span></span> <span data-ttu-id="c23c2-182">Другие платформы должны устанавливать его отдельно.</span><span class="sxs-lookup"><span data-stu-id="c23c2-182">Other platforms need to install it separately.</span></span> <span data-ttu-id="c23c2-183">Обычно это делается с помощью диспетчера пакетов программного обеспечения.</span><span class="sxs-lookup"><span data-stu-id="c23c2-183">This is typically done using a software package manager.</span></span> <span data-ttu-id="c23c2-184">Например можно использовать APT в Ubuntu и Homebrew на MacOS.</span><span class="sxs-lookup"><span data-stu-id="c23c2-184">For example, you can use APT on Ubuntu and Homebrew on MacOS.</span></span>

``` sh
# Ubuntu
apt-get install libsqlite3-mod-spatialite

# macOS
brew install libspatialite
```

### <a name="configuring-srid"></a><span data-ttu-id="c23c2-185">Настройка SRID</span><span class="sxs-lookup"><span data-stu-id="c23c2-185">Configuring SRID</span></span>

<span data-ttu-id="c23c2-186">В SpatiaLite столбцы должны быть указан SRID каждого столбца.</span><span class="sxs-lookup"><span data-stu-id="c23c2-186">In SpatiaLite, columns need to specify an SRID per column.</span></span> <span data-ttu-id="c23c2-187">По умолчанию используется SRID `0`.</span><span class="sxs-lookup"><span data-stu-id="c23c2-187">The default SRID is `0`.</span></span> <span data-ttu-id="c23c2-188">Укажите другим значением SRID, с помощью метода ForSqliteHasSrid.</span><span class="sxs-lookup"><span data-stu-id="c23c2-188">Specify a different SRID using the ForSqliteHasSrid method.</span></span>

``` csharp
modelBuilder.Entity<City>().Property(c => c.Location)
    .ForSqliteHasSrid(4326);
```

### <a name="dimension"></a><span data-ttu-id="c23c2-189">Измерение</span><span class="sxs-lookup"><span data-stu-id="c23c2-189">Dimension</span></span>

<span data-ttu-id="c23c2-190">Как и SRID, измерение столбца (или ordinates) также указывается как часть столбца.</span><span class="sxs-lookup"><span data-stu-id="c23c2-190">Similar to SRID, a column's dimension (or ordinates) is also specified as part of the column.</span></span> <span data-ttu-id="c23c2-191">По умолчанию ordinates являются X и Y. Включение дополнительных ordinates (Z и M) с помощью метода ForSqliteHasDimension.</span><span class="sxs-lookup"><span data-stu-id="c23c2-191">The default ordinates are X and Y. Enable additional ordinates (Z and M) using the ForSqliteHasDimension method.</span></span>

``` csharp
modelBuilder.Entity<City>().Property(c => c.Location)
    .ForSqliteHasDimension(Ordinates.XYZ);
```

## <a name="translated-operations"></a><span data-ttu-id="c23c2-192">Переведенные операций</span><span class="sxs-lookup"><span data-stu-id="c23c2-192">Translated Operations</span></span>

<span data-ttu-id="c23c2-193">В этой таблице показаны члены, которые NTS преобразуются в SQL каждым поставщиком EF Core.</span><span class="sxs-lookup"><span data-stu-id="c23c2-193">This table shows which NTS members are translated into SQL by each EF Core provider.</span></span>

<span data-ttu-id="c23c2-194">NetTopologySuite</span><span class="sxs-lookup"><span data-stu-id="c23c2-194">NetTopologySuite</span></span> | <span data-ttu-id="c23c2-195">SQL Server (geometry)</span><span class="sxs-lookup"><span data-stu-id="c23c2-195">SQL Server (geometry)</span></span> | <span data-ttu-id="c23c2-196">SQL Server (geography)</span><span class="sxs-lookup"><span data-stu-id="c23c2-196">SQL Server (geography)</span></span> | <span data-ttu-id="c23c2-197">SQLite</span><span class="sxs-lookup"><span data-stu-id="c23c2-197">SQLite</span></span> | <span data-ttu-id="c23c2-198">Npgsql</span><span class="sxs-lookup"><span data-stu-id="c23c2-198">Npgsql</span></span>
--- |:---:|:---:|:---:|:---:
<span data-ttu-id="c23c2-199">Geometry.Area</span><span class="sxs-lookup"><span data-stu-id="c23c2-199">Geometry.Area</span></span> | <span data-ttu-id="c23c2-200">✔</span><span class="sxs-lookup"><span data-stu-id="c23c2-200">✔</span></span> | <span data-ttu-id="c23c2-201">✔</span><span class="sxs-lookup"><span data-stu-id="c23c2-201">✔</span></span> | <span data-ttu-id="c23c2-202">✔</span><span class="sxs-lookup"><span data-stu-id="c23c2-202">✔</span></span> | <span data-ttu-id="c23c2-203">✔</span><span class="sxs-lookup"><span data-stu-id="c23c2-203">✔</span></span>
<span data-ttu-id="c23c2-204">Geometry.AsBinary()</span><span class="sxs-lookup"><span data-stu-id="c23c2-204">Geometry.AsBinary()</span></span> | <span data-ttu-id="c23c2-205">✔</span><span class="sxs-lookup"><span data-stu-id="c23c2-205">✔</span></span> | <span data-ttu-id="c23c2-206">✔</span><span class="sxs-lookup"><span data-stu-id="c23c2-206">✔</span></span> | <span data-ttu-id="c23c2-207">✔</span><span class="sxs-lookup"><span data-stu-id="c23c2-207">✔</span></span> | <span data-ttu-id="c23c2-208">✔</span><span class="sxs-lookup"><span data-stu-id="c23c2-208">✔</span></span>
<span data-ttu-id="c23c2-209">Geometry.AsText()</span><span class="sxs-lookup"><span data-stu-id="c23c2-209">Geometry.AsText()</span></span> | <span data-ttu-id="c23c2-210">✔</span><span class="sxs-lookup"><span data-stu-id="c23c2-210">✔</span></span> | <span data-ttu-id="c23c2-211">✔</span><span class="sxs-lookup"><span data-stu-id="c23c2-211">✔</span></span> | <span data-ttu-id="c23c2-212">✔</span><span class="sxs-lookup"><span data-stu-id="c23c2-212">✔</span></span> | <span data-ttu-id="c23c2-213">✔</span><span class="sxs-lookup"><span data-stu-id="c23c2-213">✔</span></span>
<span data-ttu-id="c23c2-214">Geometry.Boundary</span><span class="sxs-lookup"><span data-stu-id="c23c2-214">Geometry.Boundary</span></span> | <span data-ttu-id="c23c2-215">✔</span><span class="sxs-lookup"><span data-stu-id="c23c2-215">✔</span></span> | | <span data-ttu-id="c23c2-216">✔</span><span class="sxs-lookup"><span data-stu-id="c23c2-216">✔</span></span> | <span data-ttu-id="c23c2-217">✔</span><span class="sxs-lookup"><span data-stu-id="c23c2-217">✔</span></span>
<span data-ttu-id="c23c2-218">Geometry.Buffer(double)</span><span class="sxs-lookup"><span data-stu-id="c23c2-218">Geometry.Buffer(double)</span></span> | <span data-ttu-id="c23c2-219">✔</span><span class="sxs-lookup"><span data-stu-id="c23c2-219">✔</span></span> | <span data-ttu-id="c23c2-220">✔</span><span class="sxs-lookup"><span data-stu-id="c23c2-220">✔</span></span> | <span data-ttu-id="c23c2-221">✔</span><span class="sxs-lookup"><span data-stu-id="c23c2-221">✔</span></span> | <span data-ttu-id="c23c2-222">✔</span><span class="sxs-lookup"><span data-stu-id="c23c2-222">✔</span></span>
<span data-ttu-id="c23c2-223">Geometry.Buffer (double, int)</span><span class="sxs-lookup"><span data-stu-id="c23c2-223">Geometry.Buffer(double, int)</span></span> | | | <span data-ttu-id="c23c2-224">✔</span><span class="sxs-lookup"><span data-stu-id="c23c2-224">✔</span></span>
<span data-ttu-id="c23c2-225">Geometry.Centroid</span><span class="sxs-lookup"><span data-stu-id="c23c2-225">Geometry.Centroid</span></span> | <span data-ttu-id="c23c2-226">✔</span><span class="sxs-lookup"><span data-stu-id="c23c2-226">✔</span></span> | | <span data-ttu-id="c23c2-227">✔</span><span class="sxs-lookup"><span data-stu-id="c23c2-227">✔</span></span> | <span data-ttu-id="c23c2-228">✔</span><span class="sxs-lookup"><span data-stu-id="c23c2-228">✔</span></span>
<span data-ttu-id="c23c2-229">Geometry.Contains(Geometry)</span><span class="sxs-lookup"><span data-stu-id="c23c2-229">Geometry.Contains(Geometry)</span></span> | <span data-ttu-id="c23c2-230">✔</span><span class="sxs-lookup"><span data-stu-id="c23c2-230">✔</span></span> | <span data-ttu-id="c23c2-231">✔</span><span class="sxs-lookup"><span data-stu-id="c23c2-231">✔</span></span> | <span data-ttu-id="c23c2-232">✔</span><span class="sxs-lookup"><span data-stu-id="c23c2-232">✔</span></span> | <span data-ttu-id="c23c2-233">✔</span><span class="sxs-lookup"><span data-stu-id="c23c2-233">✔</span></span>
<span data-ttu-id="c23c2-234">Geometry.ConvexHull()</span><span class="sxs-lookup"><span data-stu-id="c23c2-234">Geometry.ConvexHull()</span></span> | <span data-ttu-id="c23c2-235">✔</span><span class="sxs-lookup"><span data-stu-id="c23c2-235">✔</span></span> | <span data-ttu-id="c23c2-236">✔</span><span class="sxs-lookup"><span data-stu-id="c23c2-236">✔</span></span> | <span data-ttu-id="c23c2-237">✔</span><span class="sxs-lookup"><span data-stu-id="c23c2-237">✔</span></span> | <span data-ttu-id="c23c2-238">✔</span><span class="sxs-lookup"><span data-stu-id="c23c2-238">✔</span></span>
<span data-ttu-id="c23c2-239">Geometry.CoveredBy(Geometry)</span><span class="sxs-lookup"><span data-stu-id="c23c2-239">Geometry.CoveredBy(Geometry)</span></span> | | | <span data-ttu-id="c23c2-240">✔</span><span class="sxs-lookup"><span data-stu-id="c23c2-240">✔</span></span> | <span data-ttu-id="c23c2-241">✔</span><span class="sxs-lookup"><span data-stu-id="c23c2-241">✔</span></span>
<span data-ttu-id="c23c2-242">Geometry.Covers(Geometry)</span><span class="sxs-lookup"><span data-stu-id="c23c2-242">Geometry.Covers(Geometry)</span></span> | | | <span data-ttu-id="c23c2-243">✔</span><span class="sxs-lookup"><span data-stu-id="c23c2-243">✔</span></span> | <span data-ttu-id="c23c2-244">✔</span><span class="sxs-lookup"><span data-stu-id="c23c2-244">✔</span></span>
<span data-ttu-id="c23c2-245">Geometry.Crosses(Geometry)</span><span class="sxs-lookup"><span data-stu-id="c23c2-245">Geometry.Crosses(Geometry)</span></span> | <span data-ttu-id="c23c2-246">✔</span><span class="sxs-lookup"><span data-stu-id="c23c2-246">✔</span></span> | | <span data-ttu-id="c23c2-247">✔</span><span class="sxs-lookup"><span data-stu-id="c23c2-247">✔</span></span> | <span data-ttu-id="c23c2-248">✔</span><span class="sxs-lookup"><span data-stu-id="c23c2-248">✔</span></span>
<span data-ttu-id="c23c2-249">Geometry.Difference(Geometry)</span><span class="sxs-lookup"><span data-stu-id="c23c2-249">Geometry.Difference(Geometry)</span></span> | <span data-ttu-id="c23c2-250">✔</span><span class="sxs-lookup"><span data-stu-id="c23c2-250">✔</span></span> | <span data-ttu-id="c23c2-251">✔</span><span class="sxs-lookup"><span data-stu-id="c23c2-251">✔</span></span> | <span data-ttu-id="c23c2-252">✔</span><span class="sxs-lookup"><span data-stu-id="c23c2-252">✔</span></span> | <span data-ttu-id="c23c2-253">✔</span><span class="sxs-lookup"><span data-stu-id="c23c2-253">✔</span></span>
<span data-ttu-id="c23c2-254">Geometry.Dimension</span><span class="sxs-lookup"><span data-stu-id="c23c2-254">Geometry.Dimension</span></span> | <span data-ttu-id="c23c2-255">✔</span><span class="sxs-lookup"><span data-stu-id="c23c2-255">✔</span></span> | <span data-ttu-id="c23c2-256">✔</span><span class="sxs-lookup"><span data-stu-id="c23c2-256">✔</span></span> | <span data-ttu-id="c23c2-257">✔</span><span class="sxs-lookup"><span data-stu-id="c23c2-257">✔</span></span> | <span data-ttu-id="c23c2-258">✔</span><span class="sxs-lookup"><span data-stu-id="c23c2-258">✔</span></span>
<span data-ttu-id="c23c2-259">Geometry.Disjoint(Geometry)</span><span class="sxs-lookup"><span data-stu-id="c23c2-259">Geometry.Disjoint(Geometry)</span></span> | <span data-ttu-id="c23c2-260">✔</span><span class="sxs-lookup"><span data-stu-id="c23c2-260">✔</span></span> | <span data-ttu-id="c23c2-261">✔</span><span class="sxs-lookup"><span data-stu-id="c23c2-261">✔</span></span> | <span data-ttu-id="c23c2-262">✔</span><span class="sxs-lookup"><span data-stu-id="c23c2-262">✔</span></span> | <span data-ttu-id="c23c2-263">✔</span><span class="sxs-lookup"><span data-stu-id="c23c2-263">✔</span></span>
<span data-ttu-id="c23c2-264">Geometry.Distance(Geometry)</span><span class="sxs-lookup"><span data-stu-id="c23c2-264">Geometry.Distance(Geometry)</span></span> | <span data-ttu-id="c23c2-265">✔</span><span class="sxs-lookup"><span data-stu-id="c23c2-265">✔</span></span> | <span data-ttu-id="c23c2-266">✔</span><span class="sxs-lookup"><span data-stu-id="c23c2-266">✔</span></span> | <span data-ttu-id="c23c2-267">✔</span><span class="sxs-lookup"><span data-stu-id="c23c2-267">✔</span></span> | <span data-ttu-id="c23c2-268">✔</span><span class="sxs-lookup"><span data-stu-id="c23c2-268">✔</span></span>
<span data-ttu-id="c23c2-269">Geometry.Envelope</span><span class="sxs-lookup"><span data-stu-id="c23c2-269">Geometry.Envelope</span></span> | <span data-ttu-id="c23c2-270">✔</span><span class="sxs-lookup"><span data-stu-id="c23c2-270">✔</span></span> | | <span data-ttu-id="c23c2-271">✔</span><span class="sxs-lookup"><span data-stu-id="c23c2-271">✔</span></span> | <span data-ttu-id="c23c2-272">✔</span><span class="sxs-lookup"><span data-stu-id="c23c2-272">✔</span></span>
<span data-ttu-id="c23c2-273">Geometry.EqualsExact(Geometry)</span><span class="sxs-lookup"><span data-stu-id="c23c2-273">Geometry.EqualsExact(Geometry)</span></span> | | | | <span data-ttu-id="c23c2-274">✔</span><span class="sxs-lookup"><span data-stu-id="c23c2-274">✔</span></span>
<span data-ttu-id="c23c2-275">Geometry.EqualsTopologically(Geometry)</span><span class="sxs-lookup"><span data-stu-id="c23c2-275">Geometry.EqualsTopologically(Geometry)</span></span> | <span data-ttu-id="c23c2-276">✔</span><span class="sxs-lookup"><span data-stu-id="c23c2-276">✔</span></span> | <span data-ttu-id="c23c2-277">✔</span><span class="sxs-lookup"><span data-stu-id="c23c2-277">✔</span></span> | <span data-ttu-id="c23c2-278">✔</span><span class="sxs-lookup"><span data-stu-id="c23c2-278">✔</span></span> | <span data-ttu-id="c23c2-279">✔</span><span class="sxs-lookup"><span data-stu-id="c23c2-279">✔</span></span>
<span data-ttu-id="c23c2-280">Geometry.GeometryType</span><span class="sxs-lookup"><span data-stu-id="c23c2-280">Geometry.GeometryType</span></span> | <span data-ttu-id="c23c2-281">✔</span><span class="sxs-lookup"><span data-stu-id="c23c2-281">✔</span></span> | <span data-ttu-id="c23c2-282">✔</span><span class="sxs-lookup"><span data-stu-id="c23c2-282">✔</span></span> | <span data-ttu-id="c23c2-283">✔</span><span class="sxs-lookup"><span data-stu-id="c23c2-283">✔</span></span> | <span data-ttu-id="c23c2-284">✔</span><span class="sxs-lookup"><span data-stu-id="c23c2-284">✔</span></span>
<span data-ttu-id="c23c2-285">Geometry.GetGeometryN(int)</span><span class="sxs-lookup"><span data-stu-id="c23c2-285">Geometry.GetGeometryN(int)</span></span> | <span data-ttu-id="c23c2-286">✔</span><span class="sxs-lookup"><span data-stu-id="c23c2-286">✔</span></span> | | <span data-ttu-id="c23c2-287">✔</span><span class="sxs-lookup"><span data-stu-id="c23c2-287">✔</span></span> | <span data-ttu-id="c23c2-288">✔</span><span class="sxs-lookup"><span data-stu-id="c23c2-288">✔</span></span>
<span data-ttu-id="c23c2-289">Geometry.InteriorPoint</span><span class="sxs-lookup"><span data-stu-id="c23c2-289">Geometry.InteriorPoint</span></span> | <span data-ttu-id="c23c2-290">✔</span><span class="sxs-lookup"><span data-stu-id="c23c2-290">✔</span></span> | | <span data-ttu-id="c23c2-291">✔</span><span class="sxs-lookup"><span data-stu-id="c23c2-291">✔</span></span>
<span data-ttu-id="c23c2-292">Geometry.Intersection(Geometry)</span><span class="sxs-lookup"><span data-stu-id="c23c2-292">Geometry.Intersection(Geometry)</span></span> | <span data-ttu-id="c23c2-293">✔</span><span class="sxs-lookup"><span data-stu-id="c23c2-293">✔</span></span> | <span data-ttu-id="c23c2-294">✔</span><span class="sxs-lookup"><span data-stu-id="c23c2-294">✔</span></span> | <span data-ttu-id="c23c2-295">✔</span><span class="sxs-lookup"><span data-stu-id="c23c2-295">✔</span></span> | <span data-ttu-id="c23c2-296">✔</span><span class="sxs-lookup"><span data-stu-id="c23c2-296">✔</span></span>
<span data-ttu-id="c23c2-297">Geometry.Intersects(Geometry)</span><span class="sxs-lookup"><span data-stu-id="c23c2-297">Geometry.Intersects(Geometry)</span></span> | <span data-ttu-id="c23c2-298">✔</span><span class="sxs-lookup"><span data-stu-id="c23c2-298">✔</span></span> | <span data-ttu-id="c23c2-299">✔</span><span class="sxs-lookup"><span data-stu-id="c23c2-299">✔</span></span> | <span data-ttu-id="c23c2-300">✔</span><span class="sxs-lookup"><span data-stu-id="c23c2-300">✔</span></span> | <span data-ttu-id="c23c2-301">✔</span><span class="sxs-lookup"><span data-stu-id="c23c2-301">✔</span></span>
<span data-ttu-id="c23c2-302">Geometry.IsEmpty</span><span class="sxs-lookup"><span data-stu-id="c23c2-302">Geometry.IsEmpty</span></span> | <span data-ttu-id="c23c2-303">✔</span><span class="sxs-lookup"><span data-stu-id="c23c2-303">✔</span></span> | <span data-ttu-id="c23c2-304">✔</span><span class="sxs-lookup"><span data-stu-id="c23c2-304">✔</span></span> | <span data-ttu-id="c23c2-305">✔</span><span class="sxs-lookup"><span data-stu-id="c23c2-305">✔</span></span> | <span data-ttu-id="c23c2-306">✔</span><span class="sxs-lookup"><span data-stu-id="c23c2-306">✔</span></span>
<span data-ttu-id="c23c2-307">Geometry.IsSimple</span><span class="sxs-lookup"><span data-stu-id="c23c2-307">Geometry.IsSimple</span></span> | <span data-ttu-id="c23c2-308">✔</span><span class="sxs-lookup"><span data-stu-id="c23c2-308">✔</span></span> | | <span data-ttu-id="c23c2-309">✔</span><span class="sxs-lookup"><span data-stu-id="c23c2-309">✔</span></span> | <span data-ttu-id="c23c2-310">✔</span><span class="sxs-lookup"><span data-stu-id="c23c2-310">✔</span></span>
<span data-ttu-id="c23c2-311">Geometry.IsValid</span><span class="sxs-lookup"><span data-stu-id="c23c2-311">Geometry.IsValid</span></span> | <span data-ttu-id="c23c2-312">✔</span><span class="sxs-lookup"><span data-stu-id="c23c2-312">✔</span></span> | <span data-ttu-id="c23c2-313">✔</span><span class="sxs-lookup"><span data-stu-id="c23c2-313">✔</span></span> | <span data-ttu-id="c23c2-314">✔</span><span class="sxs-lookup"><span data-stu-id="c23c2-314">✔</span></span> | <span data-ttu-id="c23c2-315">✔</span><span class="sxs-lookup"><span data-stu-id="c23c2-315">✔</span></span>
<span data-ttu-id="c23c2-316">Geometry.IsWithinDistance (Geometry, double)</span><span class="sxs-lookup"><span data-stu-id="c23c2-316">Geometry.IsWithinDistance(Geometry, double)</span></span> | <span data-ttu-id="c23c2-317">✔</span><span class="sxs-lookup"><span data-stu-id="c23c2-317">✔</span></span> | | <span data-ttu-id="c23c2-318">✔</span><span class="sxs-lookup"><span data-stu-id="c23c2-318">✔</span></span>
<span data-ttu-id="c23c2-319">Geometry.Length</span><span class="sxs-lookup"><span data-stu-id="c23c2-319">Geometry.Length</span></span> | <span data-ttu-id="c23c2-320">✔</span><span class="sxs-lookup"><span data-stu-id="c23c2-320">✔</span></span> | <span data-ttu-id="c23c2-321">✔</span><span class="sxs-lookup"><span data-stu-id="c23c2-321">✔</span></span> | <span data-ttu-id="c23c2-322">✔</span><span class="sxs-lookup"><span data-stu-id="c23c2-322">✔</span></span> | <span data-ttu-id="c23c2-323">✔</span><span class="sxs-lookup"><span data-stu-id="c23c2-323">✔</span></span>
<span data-ttu-id="c23c2-324">Geometry.NumGeometries</span><span class="sxs-lookup"><span data-stu-id="c23c2-324">Geometry.NumGeometries</span></span> | <span data-ttu-id="c23c2-325">✔</span><span class="sxs-lookup"><span data-stu-id="c23c2-325">✔</span></span> | <span data-ttu-id="c23c2-326">✔</span><span class="sxs-lookup"><span data-stu-id="c23c2-326">✔</span></span> | <span data-ttu-id="c23c2-327">✔</span><span class="sxs-lookup"><span data-stu-id="c23c2-327">✔</span></span> | <span data-ttu-id="c23c2-328">✔</span><span class="sxs-lookup"><span data-stu-id="c23c2-328">✔</span></span>
<span data-ttu-id="c23c2-329">Geometry.NumPoints</span><span class="sxs-lookup"><span data-stu-id="c23c2-329">Geometry.NumPoints</span></span> | <span data-ttu-id="c23c2-330">✔</span><span class="sxs-lookup"><span data-stu-id="c23c2-330">✔</span></span> | <span data-ttu-id="c23c2-331">✔</span><span class="sxs-lookup"><span data-stu-id="c23c2-331">✔</span></span> | <span data-ttu-id="c23c2-332">✔</span><span class="sxs-lookup"><span data-stu-id="c23c2-332">✔</span></span> | <span data-ttu-id="c23c2-333">✔</span><span class="sxs-lookup"><span data-stu-id="c23c2-333">✔</span></span>
<span data-ttu-id="c23c2-334">Geometry.OgcGeometryType</span><span class="sxs-lookup"><span data-stu-id="c23c2-334">Geometry.OgcGeometryType</span></span> | <span data-ttu-id="c23c2-335">✔</span><span class="sxs-lookup"><span data-stu-id="c23c2-335">✔</span></span> | <span data-ttu-id="c23c2-336">✔</span><span class="sxs-lookup"><span data-stu-id="c23c2-336">✔</span></span> | <span data-ttu-id="c23c2-337">✔</span><span class="sxs-lookup"><span data-stu-id="c23c2-337">✔</span></span>
<span data-ttu-id="c23c2-338">Geometry.Overlaps(Geometry)</span><span class="sxs-lookup"><span data-stu-id="c23c2-338">Geometry.Overlaps(Geometry)</span></span> | <span data-ttu-id="c23c2-339">✔</span><span class="sxs-lookup"><span data-stu-id="c23c2-339">✔</span></span> | <span data-ttu-id="c23c2-340">✔</span><span class="sxs-lookup"><span data-stu-id="c23c2-340">✔</span></span> | <span data-ttu-id="c23c2-341">✔</span><span class="sxs-lookup"><span data-stu-id="c23c2-341">✔</span></span> | <span data-ttu-id="c23c2-342">✔</span><span class="sxs-lookup"><span data-stu-id="c23c2-342">✔</span></span>
<span data-ttu-id="c23c2-343">Geometry.PointOnSurface</span><span class="sxs-lookup"><span data-stu-id="c23c2-343">Geometry.PointOnSurface</span></span> | <span data-ttu-id="c23c2-344">✔</span><span class="sxs-lookup"><span data-stu-id="c23c2-344">✔</span></span> | | <span data-ttu-id="c23c2-345">✔</span><span class="sxs-lookup"><span data-stu-id="c23c2-345">✔</span></span> | <span data-ttu-id="c23c2-346">✔</span><span class="sxs-lookup"><span data-stu-id="c23c2-346">✔</span></span>
<span data-ttu-id="c23c2-347">Geometry.Relate (Geometry, строка)</span><span class="sxs-lookup"><span data-stu-id="c23c2-347">Geometry.Relate(Geometry, string)</span></span> | <span data-ttu-id="c23c2-348">✔</span><span class="sxs-lookup"><span data-stu-id="c23c2-348">✔</span></span> | | <span data-ttu-id="c23c2-349">✔</span><span class="sxs-lookup"><span data-stu-id="c23c2-349">✔</span></span> | <span data-ttu-id="c23c2-350">✔</span><span class="sxs-lookup"><span data-stu-id="c23c2-350">✔</span></span>
<span data-ttu-id="c23c2-351">Geometry.Reverse()</span><span class="sxs-lookup"><span data-stu-id="c23c2-351">Geometry.Reverse()</span></span> | | | <span data-ttu-id="c23c2-352">✔</span><span class="sxs-lookup"><span data-stu-id="c23c2-352">✔</span></span> | <span data-ttu-id="c23c2-353">✔</span><span class="sxs-lookup"><span data-stu-id="c23c2-353">✔</span></span>
<span data-ttu-id="c23c2-354">Geometry.SRID</span><span class="sxs-lookup"><span data-stu-id="c23c2-354">Geometry.SRID</span></span> | <span data-ttu-id="c23c2-355">✔</span><span class="sxs-lookup"><span data-stu-id="c23c2-355">✔</span></span> | <span data-ttu-id="c23c2-356">✔</span><span class="sxs-lookup"><span data-stu-id="c23c2-356">✔</span></span> | <span data-ttu-id="c23c2-357">✔</span><span class="sxs-lookup"><span data-stu-id="c23c2-357">✔</span></span> | <span data-ttu-id="c23c2-358">✔</span><span class="sxs-lookup"><span data-stu-id="c23c2-358">✔</span></span>
<span data-ttu-id="c23c2-359">Geometry.SymmetricDifference(Geometry)</span><span class="sxs-lookup"><span data-stu-id="c23c2-359">Geometry.SymmetricDifference(Geometry)</span></span> | <span data-ttu-id="c23c2-360">✔</span><span class="sxs-lookup"><span data-stu-id="c23c2-360">✔</span></span> | <span data-ttu-id="c23c2-361">✔</span><span class="sxs-lookup"><span data-stu-id="c23c2-361">✔</span></span> | <span data-ttu-id="c23c2-362">✔</span><span class="sxs-lookup"><span data-stu-id="c23c2-362">✔</span></span> | <span data-ttu-id="c23c2-363">✔</span><span class="sxs-lookup"><span data-stu-id="c23c2-363">✔</span></span>
<span data-ttu-id="c23c2-364">Geometry.ToBinary()</span><span class="sxs-lookup"><span data-stu-id="c23c2-364">Geometry.ToBinary()</span></span> | <span data-ttu-id="c23c2-365">✔</span><span class="sxs-lookup"><span data-stu-id="c23c2-365">✔</span></span> | <span data-ttu-id="c23c2-366">✔</span><span class="sxs-lookup"><span data-stu-id="c23c2-366">✔</span></span> | <span data-ttu-id="c23c2-367">✔</span><span class="sxs-lookup"><span data-stu-id="c23c2-367">✔</span></span> | <span data-ttu-id="c23c2-368">✔</span><span class="sxs-lookup"><span data-stu-id="c23c2-368">✔</span></span>
<span data-ttu-id="c23c2-369">Geometry.ToText()</span><span class="sxs-lookup"><span data-stu-id="c23c2-369">Geometry.ToText()</span></span> | <span data-ttu-id="c23c2-370">✔</span><span class="sxs-lookup"><span data-stu-id="c23c2-370">✔</span></span> | <span data-ttu-id="c23c2-371">✔</span><span class="sxs-lookup"><span data-stu-id="c23c2-371">✔</span></span> | <span data-ttu-id="c23c2-372">✔</span><span class="sxs-lookup"><span data-stu-id="c23c2-372">✔</span></span> | <span data-ttu-id="c23c2-373">✔</span><span class="sxs-lookup"><span data-stu-id="c23c2-373">✔</span></span>
<span data-ttu-id="c23c2-374">Geometry.Touches(Geometry)</span><span class="sxs-lookup"><span data-stu-id="c23c2-374">Geometry.Touches(Geometry)</span></span> | <span data-ttu-id="c23c2-375">✔</span><span class="sxs-lookup"><span data-stu-id="c23c2-375">✔</span></span> | | <span data-ttu-id="c23c2-376">✔</span><span class="sxs-lookup"><span data-stu-id="c23c2-376">✔</span></span> | <span data-ttu-id="c23c2-377">✔</span><span class="sxs-lookup"><span data-stu-id="c23c2-377">✔</span></span>
<span data-ttu-id="c23c2-378">Geometry.Union()</span><span class="sxs-lookup"><span data-stu-id="c23c2-378">Geometry.Union()</span></span> | | | <span data-ttu-id="c23c2-379">✔</span><span class="sxs-lookup"><span data-stu-id="c23c2-379">✔</span></span>
<span data-ttu-id="c23c2-380">Geometry.Union(Geometry)</span><span class="sxs-lookup"><span data-stu-id="c23c2-380">Geometry.Union(Geometry)</span></span> | <span data-ttu-id="c23c2-381">✔</span><span class="sxs-lookup"><span data-stu-id="c23c2-381">✔</span></span> | <span data-ttu-id="c23c2-382">✔</span><span class="sxs-lookup"><span data-stu-id="c23c2-382">✔</span></span> | <span data-ttu-id="c23c2-383">✔</span><span class="sxs-lookup"><span data-stu-id="c23c2-383">✔</span></span> | <span data-ttu-id="c23c2-384">✔</span><span class="sxs-lookup"><span data-stu-id="c23c2-384">✔</span></span>
<span data-ttu-id="c23c2-385">Geometry.Within(Geometry)</span><span class="sxs-lookup"><span data-stu-id="c23c2-385">Geometry.Within(Geometry)</span></span> | <span data-ttu-id="c23c2-386">✔</span><span class="sxs-lookup"><span data-stu-id="c23c2-386">✔</span></span> | <span data-ttu-id="c23c2-387">✔</span><span class="sxs-lookup"><span data-stu-id="c23c2-387">✔</span></span> | <span data-ttu-id="c23c2-388">✔</span><span class="sxs-lookup"><span data-stu-id="c23c2-388">✔</span></span> | <span data-ttu-id="c23c2-389">✔</span><span class="sxs-lookup"><span data-stu-id="c23c2-389">✔</span></span>
<span data-ttu-id="c23c2-390">GeometryCollection.Count</span><span class="sxs-lookup"><span data-stu-id="c23c2-390">GeometryCollection.Count</span></span> | <span data-ttu-id="c23c2-391">✔</span><span class="sxs-lookup"><span data-stu-id="c23c2-391">✔</span></span> | <span data-ttu-id="c23c2-392">✔</span><span class="sxs-lookup"><span data-stu-id="c23c2-392">✔</span></span> | <span data-ttu-id="c23c2-393">✔</span><span class="sxs-lookup"><span data-stu-id="c23c2-393">✔</span></span> | <span data-ttu-id="c23c2-394">✔</span><span class="sxs-lookup"><span data-stu-id="c23c2-394">✔</span></span>
<span data-ttu-id="c23c2-395">GeometryCollection [int]</span><span class="sxs-lookup"><span data-stu-id="c23c2-395">GeometryCollection[int]</span></span> | <span data-ttu-id="c23c2-396">✔</span><span class="sxs-lookup"><span data-stu-id="c23c2-396">✔</span></span> | <span data-ttu-id="c23c2-397">✔</span><span class="sxs-lookup"><span data-stu-id="c23c2-397">✔</span></span> | <span data-ttu-id="c23c2-398">✔</span><span class="sxs-lookup"><span data-stu-id="c23c2-398">✔</span></span> | <span data-ttu-id="c23c2-399">✔</span><span class="sxs-lookup"><span data-stu-id="c23c2-399">✔</span></span>
<span data-ttu-id="c23c2-400">LineString.Count</span><span class="sxs-lookup"><span data-stu-id="c23c2-400">LineString.Count</span></span> | <span data-ttu-id="c23c2-401">✔</span><span class="sxs-lookup"><span data-stu-id="c23c2-401">✔</span></span> | <span data-ttu-id="c23c2-402">✔</span><span class="sxs-lookup"><span data-stu-id="c23c2-402">✔</span></span> | <span data-ttu-id="c23c2-403">✔</span><span class="sxs-lookup"><span data-stu-id="c23c2-403">✔</span></span> | <span data-ttu-id="c23c2-404">✔</span><span class="sxs-lookup"><span data-stu-id="c23c2-404">✔</span></span>
<span data-ttu-id="c23c2-405">LineString.EndPoint</span><span class="sxs-lookup"><span data-stu-id="c23c2-405">LineString.EndPoint</span></span> | <span data-ttu-id="c23c2-406">✔</span><span class="sxs-lookup"><span data-stu-id="c23c2-406">✔</span></span> | <span data-ttu-id="c23c2-407">✔</span><span class="sxs-lookup"><span data-stu-id="c23c2-407">✔</span></span> | <span data-ttu-id="c23c2-408">✔</span><span class="sxs-lookup"><span data-stu-id="c23c2-408">✔</span></span> | <span data-ttu-id="c23c2-409">✔</span><span class="sxs-lookup"><span data-stu-id="c23c2-409">✔</span></span>
<span data-ttu-id="c23c2-410">LineString.GetPointN(int)</span><span class="sxs-lookup"><span data-stu-id="c23c2-410">LineString.GetPointN(int)</span></span> | <span data-ttu-id="c23c2-411">✔</span><span class="sxs-lookup"><span data-stu-id="c23c2-411">✔</span></span> | <span data-ttu-id="c23c2-412">✔</span><span class="sxs-lookup"><span data-stu-id="c23c2-412">✔</span></span> | <span data-ttu-id="c23c2-413">✔</span><span class="sxs-lookup"><span data-stu-id="c23c2-413">✔</span></span> | <span data-ttu-id="c23c2-414">✔</span><span class="sxs-lookup"><span data-stu-id="c23c2-414">✔</span></span>
<span data-ttu-id="c23c2-415">LineString.IsClosed</span><span class="sxs-lookup"><span data-stu-id="c23c2-415">LineString.IsClosed</span></span> | <span data-ttu-id="c23c2-416">✔</span><span class="sxs-lookup"><span data-stu-id="c23c2-416">✔</span></span> | <span data-ttu-id="c23c2-417">✔</span><span class="sxs-lookup"><span data-stu-id="c23c2-417">✔</span></span> | <span data-ttu-id="c23c2-418">✔</span><span class="sxs-lookup"><span data-stu-id="c23c2-418">✔</span></span> | <span data-ttu-id="c23c2-419">✔</span><span class="sxs-lookup"><span data-stu-id="c23c2-419">✔</span></span>
<span data-ttu-id="c23c2-420">LineString.IsRing</span><span class="sxs-lookup"><span data-stu-id="c23c2-420">LineString.IsRing</span></span> | <span data-ttu-id="c23c2-421">✔</span><span class="sxs-lookup"><span data-stu-id="c23c2-421">✔</span></span> | | <span data-ttu-id="c23c2-422">✔</span><span class="sxs-lookup"><span data-stu-id="c23c2-422">✔</span></span> | <span data-ttu-id="c23c2-423">✔</span><span class="sxs-lookup"><span data-stu-id="c23c2-423">✔</span></span>
<span data-ttu-id="c23c2-424">LineString.StartPoint</span><span class="sxs-lookup"><span data-stu-id="c23c2-424">LineString.StartPoint</span></span> | <span data-ttu-id="c23c2-425">✔</span><span class="sxs-lookup"><span data-stu-id="c23c2-425">✔</span></span> | <span data-ttu-id="c23c2-426">✔</span><span class="sxs-lookup"><span data-stu-id="c23c2-426">✔</span></span> | <span data-ttu-id="c23c2-427">✔</span><span class="sxs-lookup"><span data-stu-id="c23c2-427">✔</span></span> | <span data-ttu-id="c23c2-428">✔</span><span class="sxs-lookup"><span data-stu-id="c23c2-428">✔</span></span>
<span data-ttu-id="c23c2-429">MultiLineString.IsClosed</span><span class="sxs-lookup"><span data-stu-id="c23c2-429">MultiLineString.IsClosed</span></span> | <span data-ttu-id="c23c2-430">✔</span><span class="sxs-lookup"><span data-stu-id="c23c2-430">✔</span></span> | <span data-ttu-id="c23c2-431">✔</span><span class="sxs-lookup"><span data-stu-id="c23c2-431">✔</span></span> | <span data-ttu-id="c23c2-432">✔</span><span class="sxs-lookup"><span data-stu-id="c23c2-432">✔</span></span> | <span data-ttu-id="c23c2-433">✔</span><span class="sxs-lookup"><span data-stu-id="c23c2-433">✔</span></span>
<span data-ttu-id="c23c2-434">Point.M</span><span class="sxs-lookup"><span data-stu-id="c23c2-434">Point.M</span></span> | <span data-ttu-id="c23c2-435">✔</span><span class="sxs-lookup"><span data-stu-id="c23c2-435">✔</span></span> | <span data-ttu-id="c23c2-436">✔</span><span class="sxs-lookup"><span data-stu-id="c23c2-436">✔</span></span> | <span data-ttu-id="c23c2-437">✔</span><span class="sxs-lookup"><span data-stu-id="c23c2-437">✔</span></span> | <span data-ttu-id="c23c2-438">✔</span><span class="sxs-lookup"><span data-stu-id="c23c2-438">✔</span></span>
<span data-ttu-id="c23c2-439">Point.X</span><span class="sxs-lookup"><span data-stu-id="c23c2-439">Point.X</span></span> | <span data-ttu-id="c23c2-440">✔</span><span class="sxs-lookup"><span data-stu-id="c23c2-440">✔</span></span> | <span data-ttu-id="c23c2-441">✔</span><span class="sxs-lookup"><span data-stu-id="c23c2-441">✔</span></span> | <span data-ttu-id="c23c2-442">✔</span><span class="sxs-lookup"><span data-stu-id="c23c2-442">✔</span></span> | <span data-ttu-id="c23c2-443">✔</span><span class="sxs-lookup"><span data-stu-id="c23c2-443">✔</span></span>
<span data-ttu-id="c23c2-444">Point.Y</span><span class="sxs-lookup"><span data-stu-id="c23c2-444">Point.Y</span></span> | <span data-ttu-id="c23c2-445">✔</span><span class="sxs-lookup"><span data-stu-id="c23c2-445">✔</span></span> | <span data-ttu-id="c23c2-446">✔</span><span class="sxs-lookup"><span data-stu-id="c23c2-446">✔</span></span> | <span data-ttu-id="c23c2-447">✔</span><span class="sxs-lookup"><span data-stu-id="c23c2-447">✔</span></span> | <span data-ttu-id="c23c2-448">✔</span><span class="sxs-lookup"><span data-stu-id="c23c2-448">✔</span></span>
<span data-ttu-id="c23c2-449">Point.Z</span><span class="sxs-lookup"><span data-stu-id="c23c2-449">Point.Z</span></span> | <span data-ttu-id="c23c2-450">✔</span><span class="sxs-lookup"><span data-stu-id="c23c2-450">✔</span></span> | <span data-ttu-id="c23c2-451">✔</span><span class="sxs-lookup"><span data-stu-id="c23c2-451">✔</span></span> | <span data-ttu-id="c23c2-452">✔</span><span class="sxs-lookup"><span data-stu-id="c23c2-452">✔</span></span> | <span data-ttu-id="c23c2-453">✔</span><span class="sxs-lookup"><span data-stu-id="c23c2-453">✔</span></span>
<span data-ttu-id="c23c2-454">Polygon.ExteriorRing</span><span class="sxs-lookup"><span data-stu-id="c23c2-454">Polygon.ExteriorRing</span></span> | <span data-ttu-id="c23c2-455">✔</span><span class="sxs-lookup"><span data-stu-id="c23c2-455">✔</span></span> | <span data-ttu-id="c23c2-456">✔</span><span class="sxs-lookup"><span data-stu-id="c23c2-456">✔</span></span> | <span data-ttu-id="c23c2-457">✔</span><span class="sxs-lookup"><span data-stu-id="c23c2-457">✔</span></span> | <span data-ttu-id="c23c2-458">✔</span><span class="sxs-lookup"><span data-stu-id="c23c2-458">✔</span></span>
<span data-ttu-id="c23c2-459">Polygon.GetInteriorRingN(int)</span><span class="sxs-lookup"><span data-stu-id="c23c2-459">Polygon.GetInteriorRingN(int)</span></span> | <span data-ttu-id="c23c2-460">✔</span><span class="sxs-lookup"><span data-stu-id="c23c2-460">✔</span></span> | <span data-ttu-id="c23c2-461">✔</span><span class="sxs-lookup"><span data-stu-id="c23c2-461">✔</span></span> | <span data-ttu-id="c23c2-462">✔</span><span class="sxs-lookup"><span data-stu-id="c23c2-462">✔</span></span> | <span data-ttu-id="c23c2-463">✔</span><span class="sxs-lookup"><span data-stu-id="c23c2-463">✔</span></span>
<span data-ttu-id="c23c2-464">Polygon.NumInteriorRings</span><span class="sxs-lookup"><span data-stu-id="c23c2-464">Polygon.NumInteriorRings</span></span> | <span data-ttu-id="c23c2-465">✔</span><span class="sxs-lookup"><span data-stu-id="c23c2-465">✔</span></span> | <span data-ttu-id="c23c2-466">✔</span><span class="sxs-lookup"><span data-stu-id="c23c2-466">✔</span></span> | <span data-ttu-id="c23c2-467">✔</span><span class="sxs-lookup"><span data-stu-id="c23c2-467">✔</span></span> | <span data-ttu-id="c23c2-468">✔</span><span class="sxs-lookup"><span data-stu-id="c23c2-468">✔</span></span>

## <a name="additional-resources"></a><span data-ttu-id="c23c2-469">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="c23c2-469">Additional resources</span></span>

* [<span data-ttu-id="c23c2-470">Пространственные данные в SQL Server</span><span class="sxs-lookup"><span data-stu-id="c23c2-470">Spatial Data in SQL Server</span></span>](https://docs.microsoft.com/sql/relational-databases/spatial/spatial-data-sql-server)
* [<span data-ttu-id="c23c2-471">Домашняя страница SpatiaLite</span><span class="sxs-lookup"><span data-stu-id="c23c2-471">SpatiaLite Homepage</span></span>](https://www.gaia-gis.it/fossil/libspatialite)
* [<span data-ttu-id="c23c2-472">Пространственные документацию по Npgsql</span><span class="sxs-lookup"><span data-stu-id="c23c2-472">Npgsql Spatial Documentation</span></span>](http://www.npgsql.org/efcore/mapping/nts.html)
* [<span data-ttu-id="c23c2-473">Документация по PostGIS</span><span class="sxs-lookup"><span data-stu-id="c23c2-473">PostGIS Documentation</span></span>](http://postgis.net/documentation/)
