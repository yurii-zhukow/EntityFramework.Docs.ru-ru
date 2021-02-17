---
title: Пространственные данные — EF Core
description: Использование пространственных данных в модели Entity Framework Core
author: bricelam
ms.date: 10/02/2020
uid: core/modeling/spatial
ms.openlocfilehash: 721aa2628d17b89b79160f8f658f8ef0dd78d6a6
ms.sourcegitcommit: 704240349e18b6404e5a809f5b7c9d365b152e2e
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/16/2021
ms.locfileid: "100543293"
---
# <a name="spatial-data"></a><span data-ttu-id="30d15-103">Пространственные данные</span><span class="sxs-lookup"><span data-stu-id="30d15-103">Spatial Data</span></span>

> [!NOTE]
> <span data-ttu-id="30d15-104">Эта функция появилась в EF Core 2,2.</span><span class="sxs-lookup"><span data-stu-id="30d15-104">This feature was introduced in EF Core 2.2.</span></span>

<span data-ttu-id="30d15-105">Пространственные данные представляют физическое расположение и форму объектов.</span><span class="sxs-lookup"><span data-stu-id="30d15-105">Spatial data represents the physical location and the shape of objects.</span></span> <span data-ttu-id="30d15-106">Многие базы данных обеспечивают поддержку этого типа данных, поэтому их можно индексировать и запрашивать вместе с другими данными.</span><span class="sxs-lookup"><span data-stu-id="30d15-106">Many databases provide support for this type of data so it can be indexed and queried alongside other data.</span></span> <span data-ttu-id="30d15-107">Типичные сценарии включают запросы объектов в заданном расстоянии от расположения или выбор объекта, граница которого содержит заданное расположение.</span><span class="sxs-lookup"><span data-stu-id="30d15-107">Common scenarios include querying for objects within a given distance from a location, or selecting the object whose border contains a given location.</span></span> <span data-ttu-id="30d15-108">EF Core поддерживает сопоставление с пространственными типами данных с помощью пространственной библиотеки Неттопологисуите.</span><span class="sxs-lookup"><span data-stu-id="30d15-108">EF Core supports mapping to spatial data types using the NetTopologySuite spatial library.</span></span>

## <a name="installing"></a><span data-ttu-id="30d15-109">Установка</span><span class="sxs-lookup"><span data-stu-id="30d15-109">Installing</span></span>

<span data-ttu-id="30d15-110">Чтобы использовать пространственные данные с EF Core, необходимо установить соответствующий поддерживающий пакет NuGet.</span><span class="sxs-lookup"><span data-stu-id="30d15-110">In order to use spatial data with EF Core, you need to install the appropriate supporting NuGet package.</span></span> <span data-ttu-id="30d15-111">Пакет, который необходимо установить, зависит от поставщика, который вы используете.</span><span class="sxs-lookup"><span data-stu-id="30d15-111">Which package you need to install depends on the provider you're using.</span></span>

<span data-ttu-id="30d15-112">Поставщик EF Core</span><span class="sxs-lookup"><span data-stu-id="30d15-112">EF Core Provider</span></span>                        | <span data-ttu-id="30d15-113">Пространственный пакет NuGet</span><span class="sxs-lookup"><span data-stu-id="30d15-113">Spatial NuGet Package</span></span>
--------------------------------------- | ---------------------
<span data-ttu-id="30d15-114">Microsoft.EntityFrameworkCore.SqlServer</span><span class="sxs-lookup"><span data-stu-id="30d15-114">Microsoft.EntityFrameworkCore.SqlServer</span></span> | [<span data-ttu-id="30d15-115">Microsoft. EntityFrameworkCore. SqlServer. Неттопологисуите</span><span class="sxs-lookup"><span data-stu-id="30d15-115">Microsoft.EntityFrameworkCore.SqlServer.NetTopologySuite</span></span>](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer.NetTopologySuite)
<span data-ttu-id="30d15-116">Microsoft.EntityFrameworkCore.Sqlite</span><span class="sxs-lookup"><span data-stu-id="30d15-116">Microsoft.EntityFrameworkCore.Sqlite</span></span>    | [<span data-ttu-id="30d15-117">Microsoft. EntityFrameworkCore. SQLite. Неттопологисуите</span><span class="sxs-lookup"><span data-stu-id="30d15-117">Microsoft.EntityFrameworkCore.Sqlite.NetTopologySuite</span></span>](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Sqlite.NetTopologySuite)
<span data-ttu-id="30d15-118">Microsoft.EntityFrameworkCore.InMemory</span><span class="sxs-lookup"><span data-stu-id="30d15-118">Microsoft.EntityFrameworkCore.InMemory</span></span>  | [<span data-ttu-id="30d15-119">неттопологисуите</span><span class="sxs-lookup"><span data-stu-id="30d15-119">NetTopologySuite</span></span>](https://www.nuget.org/packages/NetTopologySuite)
<span data-ttu-id="30d15-120">Npgsql.EntityFrameworkCore.PostgreSQL</span><span class="sxs-lookup"><span data-stu-id="30d15-120">Npgsql.EntityFrameworkCore.PostgreSQL</span></span>   | [<span data-ttu-id="30d15-121">Npgsql. EntityFrameworkCore. PostgreSQL. Неттопологисуите</span><span class="sxs-lookup"><span data-stu-id="30d15-121">Npgsql.EntityFrameworkCore.PostgreSQL.NetTopologySuite</span></span>](https://www.nuget.org/packages/Npgsql.EntityFrameworkCore.PostgreSQL.NetTopologySuite)
<span data-ttu-id="30d15-122">Pomelo.EntityFrameworkCore.MySql</span><span class="sxs-lookup"><span data-stu-id="30d15-122">Pomelo.EntityFrameworkCore.MySql</span></span>        | [<span data-ttu-id="30d15-123">Pomelo. EntityFrameworkCore. MySql. Неттопологисуите</span><span class="sxs-lookup"><span data-stu-id="30d15-123">Pomelo.EntityFrameworkCore.MySql.NetTopologySuite</span></span>](https://www.nuget.org/packages/Pomelo.EntityFrameworkCore.MySql.NetTopologySuite)
<span data-ttu-id="30d15-124">Devart.Data.MySql.EFCore</span><span class="sxs-lookup"><span data-stu-id="30d15-124">Devart.Data.MySql.EFCore</span></span>                | [<span data-ttu-id="30d15-125">Devart. Data. MySql. Ефкоре. Неттопологисуите</span><span class="sxs-lookup"><span data-stu-id="30d15-125">Devart.Data.MySql.EFCore.NetTopologySuite</span></span>](https://www.nuget.org/packages/Devart.Data.MySql.EFCore.NetTopologySuite)
<span data-ttu-id="30d15-126">Devart.Data.PostgreSql.EFCore</span><span class="sxs-lookup"><span data-stu-id="30d15-126">Devart.Data.PostgreSql.EFCore</span></span>           | [<span data-ttu-id="30d15-127">Devart. Data. PostgreSql. Ефкоре. Неттопологисуите</span><span class="sxs-lookup"><span data-stu-id="30d15-127">Devart.Data.PostgreSql.EFCore.NetTopologySuite</span></span>](https://www.nuget.org/packages/Devart.Data.PostgreSql.EFCore.NetTopologySuite)
<span data-ttu-id="30d15-128">Devart.Data.SQLite.EFCore</span><span class="sxs-lookup"><span data-stu-id="30d15-128">Devart.Data.SQLite.EFCore</span></span>               | [<span data-ttu-id="30d15-129">Devart. Data. SQLite. Ефкоре. Неттопологисуите</span><span class="sxs-lookup"><span data-stu-id="30d15-129">Devart.Data.SQLite.EFCore.NetTopologySuite</span></span>](https://www.nuget.org/packages/Devart.Data.SQLite.EFCore.NetTopologySuite)
<span data-ttu-id="30d15-130">Teradata.EntityFrameworkCore</span><span class="sxs-lookup"><span data-stu-id="30d15-130">Teradata.EntityFrameworkCore</span></span>            | [<span data-ttu-id="30d15-131">Teradata. EntityFrameworkCore. Неттопологисуите</span><span class="sxs-lookup"><span data-stu-id="30d15-131">Teradata.EntityFrameworkCore.NetTopologySuite</span></span>](https://www.nuget.org/packages/Teradata.EntityFrameworkCore.NetTopologySuite)

## <a name="nettopologysuite"></a><span data-ttu-id="30d15-132">неттопологисуите</span><span class="sxs-lookup"><span data-stu-id="30d15-132">NetTopologySuite</span></span>

<span data-ttu-id="30d15-133">[Неттопологисуите](https://nettopologysuite.github.io/NetTopologySuite/) (NTS) является пространственной библиотекой для .NET.</span><span class="sxs-lookup"><span data-stu-id="30d15-133">[NetTopologySuite](https://nettopologysuite.github.io/NetTopologySuite/) (NTS) is a spatial library for .NET.</span></span> <span data-ttu-id="30d15-134">EF Core поддерживает сопоставление с пространственными типами данных в базе данных с помощью типов NTS в модели.</span><span class="sxs-lookup"><span data-stu-id="30d15-134">EF Core enables mapping to spatial data types in the database by using NTS types in your model.</span></span>

<span data-ttu-id="30d15-135">Чтобы включить сопоставление с пространственными типами через NTS, вызовите метод Усенеттопологисуите в построителе параметров DbContext поставщика.</span><span class="sxs-lookup"><span data-stu-id="30d15-135">To enable mapping to spatial types via NTS, call the UseNetTopologySuite method on the provider's DbContext options builder.</span></span> <span data-ttu-id="30d15-136">Например, с SQL Server можно назвать его следующим образом.</span><span class="sxs-lookup"><span data-stu-id="30d15-136">For example, with SQL Server you'd call it like this.</span></span>

[!code-csharp[](../../../samples/core/Spatial/SqlServer/Models/WideWorldImportersContext.cs?name=snippet_UseNetTopologySuite)]

<span data-ttu-id="30d15-137">Существует несколько типов пространственных данных.</span><span class="sxs-lookup"><span data-stu-id="30d15-137">There are several spatial data types.</span></span> <span data-ttu-id="30d15-138">Используемый тип зависит от типов фигур, которые вы хотите разрешить.</span><span class="sxs-lookup"><span data-stu-id="30d15-138">Which type you use depends on the types of shapes you want to allow.</span></span> <span data-ttu-id="30d15-139">Ниже приведена иерархия типов NTS, которые можно использовать для свойств в модели.</span><span class="sxs-lookup"><span data-stu-id="30d15-139">Here is the hierarchy of NTS types that you can use for properties in your model.</span></span> <span data-ttu-id="30d15-140">Они находятся в пределах `NetTopologySuite.Geometries` пространства имен.</span><span class="sxs-lookup"><span data-stu-id="30d15-140">They're located within the `NetTopologySuite.Geometries` namespace.</span></span>

* <span data-ttu-id="30d15-141">Геометрия</span><span class="sxs-lookup"><span data-stu-id="30d15-141">Geometry</span></span>
  * <span data-ttu-id="30d15-142">Точка</span><span class="sxs-lookup"><span data-stu-id="30d15-142">Point</span></span>
  * <span data-ttu-id="30d15-143">LineString</span><span class="sxs-lookup"><span data-stu-id="30d15-143">LineString</span></span>
  * <span data-ttu-id="30d15-144">Многоугольник</span><span class="sxs-lookup"><span data-stu-id="30d15-144">Polygon</span></span>
  * <span data-ttu-id="30d15-145">GeometryCollection</span><span class="sxs-lookup"><span data-stu-id="30d15-145">GeometryCollection</span></span>
    * <span data-ttu-id="30d15-146">MultiPoint</span><span class="sxs-lookup"><span data-stu-id="30d15-146">MultiPoint</span></span>
    * <span data-ttu-id="30d15-147">MultiLineString</span><span class="sxs-lookup"><span data-stu-id="30d15-147">MultiLineString</span></span>
    * <span data-ttu-id="30d15-148">MultiPolygon</span><span class="sxs-lookup"><span data-stu-id="30d15-148">MultiPolygon</span></span>

> [!WARNING]
> <span data-ttu-id="30d15-149">CircularString, CompoundCurve и Куреполигон не поддерживаются NTS.</span><span class="sxs-lookup"><span data-stu-id="30d15-149">CircularString, CompoundCurve, and CurePolygon aren't supported by NTS.</span></span>

<span data-ttu-id="30d15-150">Использование базового типа geometry позволяет задать любой тип фигуры в свойстве.</span><span class="sxs-lookup"><span data-stu-id="30d15-150">Using the base Geometry type allows any type of shape to be specified by the property.</span></span>

## <a name="longitude-and-latitude"></a><span data-ttu-id="30d15-151">Долгота и Широта</span><span class="sxs-lookup"><span data-stu-id="30d15-151">Longitude and Latitude</span></span>

<span data-ttu-id="30d15-152">Координаты в NTS находятся в терминах значений X и Y.</span><span class="sxs-lookup"><span data-stu-id="30d15-152">Coordinates in NTS are in terms of X and Y values.</span></span> <span data-ttu-id="30d15-153">Чтобы представить долготу и широту, используйте X для долготы и Y для широты.</span><span class="sxs-lookup"><span data-stu-id="30d15-153">To represent longitude and latitude, use X for longitude and Y for latitude.</span></span> <span data-ttu-id="30d15-154">Обратите внимание, что это происходит **обратно** из `latitude, longitude` формата, в котором обычно отображаются эти значения.</span><span class="sxs-lookup"><span data-stu-id="30d15-154">Note that this is **backwards** from the `latitude, longitude` format in which you typically see these values.</span></span>

## <a name="querying-data"></a><span data-ttu-id="30d15-155">Запросы к данным</span><span class="sxs-lookup"><span data-stu-id="30d15-155">Querying Data</span></span>

<span data-ttu-id="30d15-156">Следующие классы сущностей можно использовать для соотнесения с таблицами в [образце базы данных Wide World импортеров](https://go.microsoft.com/fwlink/?LinkID=800630).</span><span class="sxs-lookup"><span data-stu-id="30d15-156">The following entity classes could be used to map to tables in the [Wide World Importers sample database](https://go.microsoft.com/fwlink/?LinkID=800630).</span></span>

[!code-csharp[](../../../samples/core/Spatial/SqlServer/Models/City.cs?name=snippet_City)]

[!code-csharp[](../../../samples/core/Spatial/SqlServer/Models/Country.cs?name=snippet_Country)]

<span data-ttu-id="30d15-157">В LINQ методы и свойства NTS, доступные в качестве функций базы данных, будут преобразованы в SQL.</span><span class="sxs-lookup"><span data-stu-id="30d15-157">In LINQ, the NTS methods and properties available as database functions will be translated to SQL.</span></span> <span data-ttu-id="30d15-158">Например, методы Distance и Contains преобразуются в следующие запросы.</span><span class="sxs-lookup"><span data-stu-id="30d15-158">For example, the Distance and Contains methods are translated in the following queries.</span></span> <span data-ttu-id="30d15-159">Сведения о поддерживаемых методах см. в документации поставщика.</span><span class="sxs-lookup"><span data-stu-id="30d15-159">See your provider's documentation for which methods are supported.</span></span>

[!code-csharp[](../../../samples/core/Spatial/SqlServer/Program.cs?name=snippet_Distance)]

[!code-csharp[](../../../samples/core/Spatial/SqlServer/Program.cs?name=snippet_Contains)]

## <a name="reverse-engineering"></a><span data-ttu-id="30d15-160">Реконструирование</span><span class="sxs-lookup"><span data-stu-id="30d15-160">Reverse engineering</span></span>

<span data-ttu-id="30d15-161">Пространственные пакеты NuGet также включают модели [реконструирования](xref:core/managing-schemas/scaffolding) с пространственными свойствами, но необходимо установить пакет ***перед*** запуском `Scaffold-DbContext` или `dotnet ef dbcontext scaffold` .</span><span class="sxs-lookup"><span data-stu-id="30d15-161">The spatial NuGet packages also enable [reverse engineering](xref:core/managing-schemas/scaffolding) models with spatial properties, but you need to install the package ***before*** running `Scaffold-DbContext` or `dotnet ef dbcontext scaffold`.</span></span> <span data-ttu-id="30d15-162">В противном случае вы получите предупреждения о том, что не удается найти сопоставления типов для столбцов, и столбцы будут пропущены.</span><span class="sxs-lookup"><span data-stu-id="30d15-162">If you don't, you'll receive warnings about not finding type mappings for the columns and the columns will be skipped.</span></span>

## <a name="srid-ignored-during-client-operations"></a><span data-ttu-id="30d15-163">SRID игнорируется во время операций клиента</span><span class="sxs-lookup"><span data-stu-id="30d15-163">SRID Ignored during client operations</span></span>

<span data-ttu-id="30d15-164">NTS игнорирует значения SRID во время операций.</span><span class="sxs-lookup"><span data-stu-id="30d15-164">NTS ignores SRID values during operations.</span></span> <span data-ttu-id="30d15-165">Предполагается, что это плоская система координат.</span><span class="sxs-lookup"><span data-stu-id="30d15-165">It assumes a planar coordinate system.</span></span> <span data-ttu-id="30d15-166">Это означает, что если указать координаты в виде долготы и широты, то некоторые значения, вычисляемые клиентом, такие как расстояние, Длина и площадь, будут в градусах, а не в метрах.</span><span class="sxs-lookup"><span data-stu-id="30d15-166">This means that if you specify coordinates in terms of longitude and latitude, some client-evaluated values like distance, length, and area will be in degrees, not meters.</span></span> <span data-ttu-id="30d15-167">Для более значимых значений необходимо сначала проецировать координаты в другую систему координат с помощью библиотеки, такой как [прожнет (для жеоапи)](https://github.com/NetTopologySuite/ProjNet4GeoAPI).</span><span class="sxs-lookup"><span data-stu-id="30d15-167">For more meaningful values, you first need to project the coordinates to another coordinate system using a library like [ProjNet (for GeoAPI)](https://github.com/NetTopologySuite/ProjNet4GeoAPI).</span></span>

> [!NOTE]
> <span data-ttu-id="30d15-168">Используйте более новый [пакет NuGet прожнет](https://www.nuget.org/packages/ProjNet/), а **не** более старый пакет с именем ProjNet4GeoAPI.</span><span class="sxs-lookup"><span data-stu-id="30d15-168">Use the newer [ProjNet NuGet package](https://www.nuget.org/packages/ProjNet/), **not** the older package called ProjNet4GeoAPI.</span></span>

<span data-ttu-id="30d15-169">Если операция оценивается сервером с помощью EF Core с помощью SQL, то единица результата будет определяться базой данных.</span><span class="sxs-lookup"><span data-stu-id="30d15-169">If an operation is server-evaluated by EF Core via SQL, the result's unit will be determined by the database.</span></span>

<span data-ttu-id="30d15-170">Ниже приведен пример использования Прожнет для вычисления расстояния между двумя городами.</span><span class="sxs-lookup"><span data-stu-id="30d15-170">Here is an example of using ProjNet to calculate the distance between two cities.</span></span>

[!code-csharp[](../../../samples/core/Spatial/Projections/GeometryExtensions.cs?name=snippet_GeometryExtensions)]

[!code-csharp[](../../../samples/core/Spatial/Projections/Program.cs?name=snippet_ProjectTo)]

> [!NOTE]
> <span data-ttu-id="30d15-171">4326 относится к WGS 84, стандарту, используемому в GPS и других географических системах.</span><span class="sxs-lookup"><span data-stu-id="30d15-171">4326 refers to WGS 84, a standard used in GPS and other geographic systems.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="30d15-172">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="30d15-172">Additional resources</span></span>

### <a name="database-specific-information"></a><span data-ttu-id="30d15-173">Сведения, относящиеся к базе данных</span><span class="sxs-lookup"><span data-stu-id="30d15-173">Database-specific information</span></span>

<span data-ttu-id="30d15-174">Обязательно ознакомьтесь с документацией поставщика, чтобы получить дополнительные сведения о работе с пространственными данными.</span><span class="sxs-lookup"><span data-stu-id="30d15-174">Be sure to read your provider's documentation for additional information on working with spatial data.</span></span>

* [<span data-ttu-id="30d15-175">Пространственные данные в поставщике SQL Server</span><span class="sxs-lookup"><span data-stu-id="30d15-175">Spatial Data in the SQL Server Provider</span></span>](xref:core/providers/sql-server/spatial)
* [<span data-ttu-id="30d15-176">Пространственные данные в поставщике SQLite</span><span class="sxs-lookup"><span data-stu-id="30d15-176">Spatial Data in the SQLite Provider</span></span>](xref:core/providers/sqlite/spatial)
* [<span data-ttu-id="30d15-177">Пространственные данные в поставщике Npgsql</span><span class="sxs-lookup"><span data-stu-id="30d15-177">Spatial Data in the Npgsql Provider</span></span>](https://www.npgsql.org/efcore/mapping/nts.html)

### <a name="other-resources"></a><span data-ttu-id="30d15-178">Другие источники</span><span class="sxs-lookup"><span data-stu-id="30d15-178">Other resources</span></span>

* [<span data-ttu-id="30d15-179">Документы Неттопологисуите</span><span class="sxs-lookup"><span data-stu-id="30d15-179">NetTopologySuite Docs</span></span>](https://nettopologysuite.github.io/NetTopologySuite/)
* <span data-ttu-id="30d15-180">[EF Core сеанс standup Community](https://www.youtube.com/watch?v=IHslY5rrxD0&list=PLdo4fOcmZ0oX-DBuRG4u58ZTAJgBAeQ-t&index=15), сосредоточенный на пространственных данных и неттопологисуите.</span><span class="sxs-lookup"><span data-stu-id="30d15-180">[EF Core Community Standup session](https://www.youtube.com/watch?v=IHslY5rrxD0&list=PLdo4fOcmZ0oX-DBuRG4u58ZTAJgBAeQ-t&index=15), focusing on spatial data and NetTopologySuite.</span></span>
