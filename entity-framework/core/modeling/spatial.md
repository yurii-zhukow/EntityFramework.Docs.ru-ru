---
title: Пространственные данные — EF Core
description: Использование пространственных данных в модели Entity Framework Core
author: bricelam
ms.date: 10/02/2020
uid: core/modeling/spatial
ms.openlocfilehash: a38e75e150b3d3404f2ed7619d05ba478c411117
ms.sourcegitcommit: 788a56c2248523967b846bcca0e98c2ed7ef0d6b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/20/2020
ms.locfileid: "95003449"
---
# <a name="spatial-data"></a><span data-ttu-id="0eb23-103">Пространственные данные</span><span class="sxs-lookup"><span data-stu-id="0eb23-103">Spatial Data</span></span>

> [!NOTE]
> <span data-ttu-id="0eb23-104">Эта функция появилась в EF Core 2,2.</span><span class="sxs-lookup"><span data-stu-id="0eb23-104">This feature was introduced in EF Core 2.2.</span></span>

<span data-ttu-id="0eb23-105">Пространственные данные представляют физическое расположение и форму объектов.</span><span class="sxs-lookup"><span data-stu-id="0eb23-105">Spatial data represents the physical location and the shape of objects.</span></span> <span data-ttu-id="0eb23-106">Многие базы данных обеспечивают поддержку этого типа данных, поэтому их можно индексировать и запрашивать вместе с другими данными.</span><span class="sxs-lookup"><span data-stu-id="0eb23-106">Many databases provide support for this type of data so it can be indexed and queried alongside other data.</span></span> <span data-ttu-id="0eb23-107">Типичные сценарии включают запросы объектов в заданном расстоянии от расположения или выбор объекта, граница которого содержит заданное расположение.</span><span class="sxs-lookup"><span data-stu-id="0eb23-107">Common scenarios include querying for objects within a given distance from a location, or selecting the object whose border contains a given location.</span></span> <span data-ttu-id="0eb23-108">EF Core поддерживает сопоставление с пространственными типами данных с помощью пространственной библиотеки Неттопологисуите.</span><span class="sxs-lookup"><span data-stu-id="0eb23-108">EF Core supports mapping to spatial data types using the NetTopologySuite spatial library.</span></span>

## <a name="installing"></a><span data-ttu-id="0eb23-109">Установка</span><span class="sxs-lookup"><span data-stu-id="0eb23-109">Installing</span></span>

<span data-ttu-id="0eb23-110">Чтобы использовать пространственные данные с EF Core, необходимо установить соответствующий поддерживающий пакет NuGet.</span><span class="sxs-lookup"><span data-stu-id="0eb23-110">In order to use spatial data with EF Core, you need to install the appropriate supporting NuGet package.</span></span> <span data-ttu-id="0eb23-111">Пакет, который необходимо установить, зависит от поставщика, который вы используете.</span><span class="sxs-lookup"><span data-stu-id="0eb23-111">Which package you need to install depends on the provider you're using.</span></span>

<span data-ttu-id="0eb23-112">Поставщик EF Core</span><span class="sxs-lookup"><span data-stu-id="0eb23-112">EF Core Provider</span></span>                        | <span data-ttu-id="0eb23-113">Пространственный пакет NuGet</span><span class="sxs-lookup"><span data-stu-id="0eb23-113">Spatial NuGet Package</span></span>
--------------------------------------- | ---------------------
<span data-ttu-id="0eb23-114">Microsoft.EntityFrameworkCore.SqlServer</span><span class="sxs-lookup"><span data-stu-id="0eb23-114">Microsoft.EntityFrameworkCore.SqlServer</span></span> | [<span data-ttu-id="0eb23-115">Microsoft. EntityFrameworkCore. SqlServer. Неттопологисуите</span><span class="sxs-lookup"><span data-stu-id="0eb23-115">Microsoft.EntityFrameworkCore.SqlServer.NetTopologySuite</span></span>](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer.NetTopologySuite)
<span data-ttu-id="0eb23-116">Microsoft.EntityFrameworkCore.Sqlite</span><span class="sxs-lookup"><span data-stu-id="0eb23-116">Microsoft.EntityFrameworkCore.Sqlite</span></span>    | [<span data-ttu-id="0eb23-117">Microsoft. EntityFrameworkCore. SQLite. Неттопологисуите</span><span class="sxs-lookup"><span data-stu-id="0eb23-117">Microsoft.EntityFrameworkCore.Sqlite.NetTopologySuite</span></span>](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Sqlite.NetTopologySuite)
<span data-ttu-id="0eb23-118">Microsoft.EntityFrameworkCore.InMemory</span><span class="sxs-lookup"><span data-stu-id="0eb23-118">Microsoft.EntityFrameworkCore.InMemory</span></span>  | [<span data-ttu-id="0eb23-119">неттопологисуите</span><span class="sxs-lookup"><span data-stu-id="0eb23-119">NetTopologySuite</span></span>](https://www.nuget.org/packages/NetTopologySuite)
<span data-ttu-id="0eb23-120">Npgsql.EntityFrameworkCore.PostgreSQL</span><span class="sxs-lookup"><span data-stu-id="0eb23-120">Npgsql.EntityFrameworkCore.PostgreSQL</span></span>   | [<span data-ttu-id="0eb23-121">Npgsql. EntityFrameworkCore. PostgreSQL. Неттопологисуите</span><span class="sxs-lookup"><span data-stu-id="0eb23-121">Npgsql.EntityFrameworkCore.PostgreSQL.NetTopologySuite</span></span>](https://www.nuget.org/packages/Npgsql.EntityFrameworkCore.PostgreSQL.NetTopologySuite)
<span data-ttu-id="0eb23-122">Pomelo.EntityFrameworkCore.MySql</span><span class="sxs-lookup"><span data-stu-id="0eb23-122">Pomelo.EntityFrameworkCore.MySql</span></span>        | [<span data-ttu-id="0eb23-123">Pomelo. EntityFrameworkCore. MySql. Неттопологисуите</span><span class="sxs-lookup"><span data-stu-id="0eb23-123">Pomelo.EntityFrameworkCore.MySql.NetTopologySuite</span></span>](https://www.nuget.org/packages/Pomelo.EntityFrameworkCore.MySql.NetTopologySuite)
<span data-ttu-id="0eb23-124">Devart.Data.MySql.EFCore</span><span class="sxs-lookup"><span data-stu-id="0eb23-124">Devart.Data.MySql.EFCore</span></span>                | [<span data-ttu-id="0eb23-125">Devart. Data. MySql. Ефкоре. Неттопологисуите</span><span class="sxs-lookup"><span data-stu-id="0eb23-125">Devart.Data.MySql.EFCore.NetTopologySuite</span></span>](https://www.nuget.org/packages/Devart.Data.MySql.EFCore.NetTopologySuite)
<span data-ttu-id="0eb23-126">Devart.Data.PostgreSql.EFCore</span><span class="sxs-lookup"><span data-stu-id="0eb23-126">Devart.Data.PostgreSql.EFCore</span></span>           | [<span data-ttu-id="0eb23-127">Devart. Data. PostgreSql. Ефкоре. Неттопологисуите</span><span class="sxs-lookup"><span data-stu-id="0eb23-127">Devart.Data.PostgreSql.EFCore.NetTopologySuite</span></span>](https://www.nuget.org/packages/Devart.Data.PostgreSql.EFCore.NetTopologySuite)
<span data-ttu-id="0eb23-128">Devart.Data.SQLite.EFCore</span><span class="sxs-lookup"><span data-stu-id="0eb23-128">Devart.Data.SQLite.EFCore</span></span>               | [<span data-ttu-id="0eb23-129">Devart. Data. SQLite. Ефкоре. Неттопологисуите</span><span class="sxs-lookup"><span data-stu-id="0eb23-129">Devart.Data.SQLite.EFCore.NetTopologySuite</span></span>](https://www.nuget.org/packages/Devart.Data.SQLite.EFCore.NetTopologySuite)
<span data-ttu-id="0eb23-130">Teradata.EntityFrameworkCore</span><span class="sxs-lookup"><span data-stu-id="0eb23-130">Teradata.EntityFrameworkCore</span></span>            | [<span data-ttu-id="0eb23-131">Teradata. EntityFrameworkCore. Неттопологисуите</span><span class="sxs-lookup"><span data-stu-id="0eb23-131">Teradata.EntityFrameworkCore.NetTopologySuite</span></span>](https://www.nuget.org/packages/Teradata.EntityFrameworkCore.NetTopologySuite)

## <a name="nettopologysuite"></a><span data-ttu-id="0eb23-132">неттопологисуите</span><span class="sxs-lookup"><span data-stu-id="0eb23-132">NetTopologySuite</span></span>

<span data-ttu-id="0eb23-133">[Неттопологисуите](https://nettopologysuite.github.io/NetTopologySuite/) (NTS) является пространственной библиотекой для .NET.</span><span class="sxs-lookup"><span data-stu-id="0eb23-133">[NetTopologySuite](https://nettopologysuite.github.io/NetTopologySuite/) (NTS) is a spatial library for .NET.</span></span> <span data-ttu-id="0eb23-134">EF Core поддерживает сопоставление с пространственными типами данных в базе данных с помощью типов NTS в модели.</span><span class="sxs-lookup"><span data-stu-id="0eb23-134">EF Core enables mapping to spatial data types in the database by using NTS types in your model.</span></span>

<span data-ttu-id="0eb23-135">Чтобы включить сопоставление с пространственными типами через NTS, вызовите метод Усенеттопологисуите в построителе параметров DbContext поставщика.</span><span class="sxs-lookup"><span data-stu-id="0eb23-135">To enable mapping to spatial types via NTS, call the UseNetTopologySuite method on the provider's DbContext options builder.</span></span> <span data-ttu-id="0eb23-136">Например, с SQL Server можно назвать его следующим образом.</span><span class="sxs-lookup"><span data-stu-id="0eb23-136">For example, with SQL Server you'd call it like this.</span></span>

[!code-csharp[](../../../samples/core/Spatial/SqlServer/Models/WideWorldImportersContext.cs?name=snippet_UseNetTopologySuite)]

<span data-ttu-id="0eb23-137">Существует несколько типов пространственных данных.</span><span class="sxs-lookup"><span data-stu-id="0eb23-137">There are several spatial data types.</span></span> <span data-ttu-id="0eb23-138">Используемый тип зависит от типов фигур, которые вы хотите разрешить.</span><span class="sxs-lookup"><span data-stu-id="0eb23-138">Which type you use depends on the types of shapes you want to allow.</span></span> <span data-ttu-id="0eb23-139">Ниже приведена иерархия типов NTS, которые можно использовать для свойств в модели.</span><span class="sxs-lookup"><span data-stu-id="0eb23-139">Here is the hierarchy of NTS types that you can use for properties in your model.</span></span> <span data-ttu-id="0eb23-140">Они находятся в пределах `NetTopologySuite.Geometries` пространства имен.</span><span class="sxs-lookup"><span data-stu-id="0eb23-140">They're located within the `NetTopologySuite.Geometries` namespace.</span></span>

* <span data-ttu-id="0eb23-141">Геометрия</span><span class="sxs-lookup"><span data-stu-id="0eb23-141">Geometry</span></span>
  * <span data-ttu-id="0eb23-142">Точка</span><span class="sxs-lookup"><span data-stu-id="0eb23-142">Point</span></span>
  * <span data-ttu-id="0eb23-143">LineString</span><span class="sxs-lookup"><span data-stu-id="0eb23-143">LineString</span></span>
  * <span data-ttu-id="0eb23-144">Многоугольник</span><span class="sxs-lookup"><span data-stu-id="0eb23-144">Polygon</span></span>
  * <span data-ttu-id="0eb23-145">GeometryCollection</span><span class="sxs-lookup"><span data-stu-id="0eb23-145">GeometryCollection</span></span>
    * <span data-ttu-id="0eb23-146">MultiPoint</span><span class="sxs-lookup"><span data-stu-id="0eb23-146">MultiPoint</span></span>
    * <span data-ttu-id="0eb23-147">MultiLineString</span><span class="sxs-lookup"><span data-stu-id="0eb23-147">MultiLineString</span></span>
    * <span data-ttu-id="0eb23-148">MultiPolygon</span><span class="sxs-lookup"><span data-stu-id="0eb23-148">MultiPolygon</span></span>

> [!WARNING]
> <span data-ttu-id="0eb23-149">CircularString, CompoundCurve и Куреполигон не поддерживаются NTS.</span><span class="sxs-lookup"><span data-stu-id="0eb23-149">CircularString, CompoundCurve, and CurePolygon aren't supported by NTS.</span></span>

<span data-ttu-id="0eb23-150">Использование базового типа geometry позволяет задать любой тип фигуры в свойстве.</span><span class="sxs-lookup"><span data-stu-id="0eb23-150">Using the base Geometry type allows any type of shape to be specified by the property.</span></span>

## <a name="longitude-and-latitude"></a><span data-ttu-id="0eb23-151">Долгота и Широта</span><span class="sxs-lookup"><span data-stu-id="0eb23-151">Longitude and Latitude</span></span>

<span data-ttu-id="0eb23-152">Координаты в NTS находятся в терминах значений X и Y.</span><span class="sxs-lookup"><span data-stu-id="0eb23-152">Coordinates in NTS are in terms of X and Y values.</span></span> <span data-ttu-id="0eb23-153">Чтобы представить долготу и широту, используйте X для долготы и Y для широты.</span><span class="sxs-lookup"><span data-stu-id="0eb23-153">To represent longitude and latitude, use X for longitude and Y for latitude.</span></span> <span data-ttu-id="0eb23-154">Обратите внимание, что это происходит **обратно** из `latitude, longitude` формата, в котором обычно отображаются эти значения.</span><span class="sxs-lookup"><span data-stu-id="0eb23-154">Note that this is **backwards** from the `latitude, longitude` format in which you typically see these values.</span></span>

## <a name="querying-data"></a><span data-ttu-id="0eb23-155">Запросы к данным</span><span class="sxs-lookup"><span data-stu-id="0eb23-155">Querying Data</span></span>

<span data-ttu-id="0eb23-156">Следующие классы сущностей можно использовать для соотнесения с таблицами в [образце базы данных Wide World импортеров](https://go.microsoft.com/fwlink/?LinkID=800630).</span><span class="sxs-lookup"><span data-stu-id="0eb23-156">The following entity classes could be used to map to tables in the [Wide World Importers sample database](https://go.microsoft.com/fwlink/?LinkID=800630).</span></span>

[!code-csharp[](../../../samples/core/Spatial/SqlServer/Models/City.cs?name=snippet_City)]

[!code-csharp[](../../../samples/core/Spatial/SqlServer/Models/Country.cs?name=snippet_Country)]

<span data-ttu-id="0eb23-157">В LINQ методы и свойства NTS, доступные в качестве функций базы данных, будут преобразованы в SQL.</span><span class="sxs-lookup"><span data-stu-id="0eb23-157">In LINQ, the NTS methods and properties available as database functions will be translated to SQL.</span></span> <span data-ttu-id="0eb23-158">Например, методы Distance и Contains преобразуются в следующие запросы.</span><span class="sxs-lookup"><span data-stu-id="0eb23-158">For example, the Distance and Contains methods are translated in the following queries.</span></span> <span data-ttu-id="0eb23-159">Сведения о поддерживаемых методах см. в документации поставщика.</span><span class="sxs-lookup"><span data-stu-id="0eb23-159">See your provider's documentation for which methods are supported.</span></span>

[!code-csharp[](../../../samples/core/Spatial/SqlServer/Program.cs?name=snippet_Distance)]

[!code-csharp[](../../../samples/core/Spatial/SqlServer/Program.cs?name=snippet_Contains)]

## <a name="reverse-engineering"></a><span data-ttu-id="0eb23-160">Реконструирование</span><span class="sxs-lookup"><span data-stu-id="0eb23-160">Reverse engineering</span></span>

<span data-ttu-id="0eb23-161">Пространственные пакеты NuGet также включают модели [реконструирования](xref:core/managing-schemas/scaffolding) с пространственными свойствами, но необходимо установить пакет \**_перед_* _ запуском `Scaffold-DbContext` или `dotnet ef dbcontext scaffold` .</span><span class="sxs-lookup"><span data-stu-id="0eb23-161">The spatial NuGet packages also enable [reverse engineering](xref:core/managing-schemas/scaffolding) models with spatial properties, but you need to install the package \**_before_* _ running `Scaffold-DbContext` or `dotnet ef dbcontext scaffold`.</span></span> <span data-ttu-id="0eb23-162">В противном случае вы получите предупреждения о том, что не удается найти сопоставления типов для столбцов, и столбцы будут пропущены.</span><span class="sxs-lookup"><span data-stu-id="0eb23-162">If you don't, you'll receive warnings about not finding type mappings for the columns and the columns will be skipped.</span></span>

## <a name="srid-ignored-during-client-operations"></a><span data-ttu-id="0eb23-163">SRID игнорируется во время операций клиента</span><span class="sxs-lookup"><span data-stu-id="0eb23-163">SRID Ignored during client operations</span></span>

<span data-ttu-id="0eb23-164">NTS игнорирует значения SRID во время операций.</span><span class="sxs-lookup"><span data-stu-id="0eb23-164">NTS ignores SRID values during operations.</span></span> <span data-ttu-id="0eb23-165">Предполагается, что это плоская система координат.</span><span class="sxs-lookup"><span data-stu-id="0eb23-165">It assumes a planar coordinate system.</span></span> <span data-ttu-id="0eb23-166">Это означает, что если указать координаты в виде долготы и широты, то некоторые значения, вычисляемые клиентом, такие как расстояние, Длина и площадь, будут в градусах, а не в метрах.</span><span class="sxs-lookup"><span data-stu-id="0eb23-166">This means that if you specify coordinates in terms of longitude and latitude, some client-evaluated values like distance, length, and area will be in degrees, not meters.</span></span> <span data-ttu-id="0eb23-167">Чтобы получить более значимые значения, сначала необходимо проецировать координаты в другую систему координат, используя библиотеку, например [ProjNet4GeoAPI](https://github.com/NetTopologySuite/ProjNet4GeoAPI) , перед вычислением этих значений.</span><span class="sxs-lookup"><span data-stu-id="0eb23-167">For more meaningful values, you first need to project the coordinates to another coordinate system using a library like [ProjNet4GeoAPI](https://github.com/NetTopologySuite/ProjNet4GeoAPI) before calculating these values.</span></span>

<span data-ttu-id="0eb23-168">Если операция оценивается сервером с помощью EF Core с помощью SQL, то единица результата будет определяться базой данных.</span><span class="sxs-lookup"><span data-stu-id="0eb23-168">If an operation is server-evaluated by EF Core via SQL, the result's unit will be determined by the database.</span></span>

<span data-ttu-id="0eb23-169">Ниже приведен пример использования ProjNet4GeoAPI для вычисления расстояния между двумя городами.</span><span class="sxs-lookup"><span data-stu-id="0eb23-169">Here is an example of using ProjNet4GeoAPI to calculate the distance between two cities.</span></span>

[!code-csharp[](../../../samples/core/Spatial/Projections/GeometryExtensions.cs?name=snippet_GeometryExtensions)]

[!code-csharp[](../../../samples/core/Spatial/Projections/Program.cs?name=snippet_ProjectTo)]

> [!NOTE]
> <span data-ttu-id="0eb23-170">4326 относится к WGS 84, стандарту, используемому в GPS и других географических системах.</span><span class="sxs-lookup"><span data-stu-id="0eb23-170">4326 refers to WGS 84, a standard used in GPS and other geographic systems.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="0eb23-171">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="0eb23-171">Additional resources</span></span>

### <a name="database-specific-information"></a><span data-ttu-id="0eb23-172">Сведения, относящиеся к базе данных</span><span class="sxs-lookup"><span data-stu-id="0eb23-172">Database-specific information</span></span>

<span data-ttu-id="0eb23-173">Обязательно ознакомьтесь с документацией поставщика, чтобы получить дополнительные сведения о работе с пространственными данными.</span><span class="sxs-lookup"><span data-stu-id="0eb23-173">Be sure to read your provider's documentation for additional information on working with spatial data.</span></span>

<span data-ttu-id="0eb23-174">_ [Пространственные данные в поставщике SQL Server](xref:core/providers/sql-server/spatial)</span><span class="sxs-lookup"><span data-stu-id="0eb23-174">_ [Spatial Data in the SQL Server Provider](xref:core/providers/sql-server/spatial)</span></span>
* [<span data-ttu-id="0eb23-175">Пространственные данные в поставщике SQLite</span><span class="sxs-lookup"><span data-stu-id="0eb23-175">Spatial Data in the SQLite Provider</span></span>](xref:core/providers/sqlite/spatial)
* [<span data-ttu-id="0eb23-176">Пространственные данные в поставщике Npgsql</span><span class="sxs-lookup"><span data-stu-id="0eb23-176">Spatial Data in the Npgsql Provider</span></span>](https://www.npgsql.org/efcore/mapping/nts.html)

### <a name="other-resources"></a><span data-ttu-id="0eb23-177">Другие источники</span><span class="sxs-lookup"><span data-stu-id="0eb23-177">Other resources</span></span>

* [<span data-ttu-id="0eb23-178">Документы Неттопологисуите</span><span class="sxs-lookup"><span data-stu-id="0eb23-178">NetTopologySuite Docs</span></span>](https://nettopologysuite.github.io/NetTopologySuite/)
* <span data-ttu-id="0eb23-179">[EF Core сеанс standup Community](https://www.youtube.com/watch?v=IHslY5rrxD0&list=PLdo4fOcmZ0oX-DBuRG4u58ZTAJgBAeQ-t&index=15), сосредоточенный на пространственных данных и неттопологисуите.</span><span class="sxs-lookup"><span data-stu-id="0eb23-179">[EF Core Community Standup session](https://www.youtube.com/watch?v=IHslY5rrxD0&list=PLdo4fOcmZ0oX-DBuRG4u58ZTAJgBAeQ-t&index=15), focusing on spatial data and NetTopologySuite.</span></span>
