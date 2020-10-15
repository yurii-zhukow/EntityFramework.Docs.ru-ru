---
title: Поставщик базы данных SQLite — пространственные данные — EF Core
description: Использование пространственных данных с поставщиком базы данных Entity Framework Core SQLite
author: bricelam
ms.date: 10/02/2020
uid: core/providers/sqlite/spatial
ms.openlocfilehash: 3296955dc046b91b53a1dcb09c51b340bc853b4a
ms.sourcegitcommit: 0a25c03fa65ae6e0e0e3f66bac48d59eceb96a5a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/14/2020
ms.locfileid: "92066546"
---
# <a name="spatial-data-in-the-sqlite-ef-core-provider"></a><span data-ttu-id="391bf-103">Пространственные данные в поставщике EF Core SQLite</span><span class="sxs-lookup"><span data-stu-id="391bf-103">Spatial Data in the SQLite EF Core Provider</span></span>

<span data-ttu-id="391bf-104">На этой странице содержатся дополнительные сведения об использовании пространственных данных с поставщиком базы данных SQLite.</span><span class="sxs-lookup"><span data-stu-id="391bf-104">This page includes additional information about using spatial data with the SQLite database provider.</span></span> <span data-ttu-id="391bf-105">Общие сведения об использовании пространственных данных в EF Core см. в основной документации по [пространственным данным](xref:core/modeling/spatial) .</span><span class="sxs-lookup"><span data-stu-id="391bf-105">For general information about using spatial data in EF Core, see the main [Spatial Data](xref:core/modeling/spatial) documentation.</span></span>

## <a name="installing-spatialite"></a><span data-ttu-id="391bf-106">Установка Спатиалите</span><span class="sxs-lookup"><span data-stu-id="391bf-106">Installing SpatiaLite</span></span>

<span data-ttu-id="391bf-107">В Windows собственная библиотека mod_spatialite распространяется как зависимость пакета NuGet.</span><span class="sxs-lookup"><span data-stu-id="391bf-107">On Windows, the native mod_spatialite library is distributed as a NuGet package dependency.</span></span> <span data-ttu-id="391bf-108">Другие платформы должны установить его отдельно.</span><span class="sxs-lookup"><span data-stu-id="391bf-108">Other platforms need to install it separately.</span></span> <span data-ttu-id="391bf-109">Обычно это делается с помощью диспетчера пакетов программного обеспечения.</span><span class="sxs-lookup"><span data-stu-id="391bf-109">This is typically done using a software package manager.</span></span> <span data-ttu-id="391bf-110">Например, можно использовать APT в Debian и Ubuntu; и Homebrew на MacOS.</span><span class="sxs-lookup"><span data-stu-id="391bf-110">For example, you can use APT on Debian and Ubuntu; and Homebrew on MacOS.</span></span>

```bash
# Debian/Ubuntu
apt-get install libsqlite3-mod-spatialite

# macOS
brew install libspatialite
```

<span data-ttu-id="391bf-111">К сожалению, новые версии PROJ (зависимость от Спатиалите) несовместимы с [набором склитепклрав](/dotnet/standard/data/sqlite/custom-versions#bundles)по умолчанию EF.</span><span class="sxs-lookup"><span data-stu-id="391bf-111">Unfortunately, newer versions of PROJ (a dependency of SpatiaLite) are incompatible with EF's default [SQLitePCLRaw bundle](/dotnet/standard/data/sqlite/custom-versions#bundles).</span></span> <span data-ttu-id="391bf-112">Это можно обойти, используя системную библиотеку SQLite.</span><span class="sxs-lookup"><span data-stu-id="391bf-112">You can work around this by using the system SQLite library instead.</span></span>

```xml
<ItemGroup>
  <!-- Use bundle_sqlite3 instead with SpatiaLite on macOS and Linux -->
  <!--<PackageReference Include="Microsoft.EntityFrameworkCore.Sqlite" Version="3.1.0" />-->
  <PackageReference Include="Microsoft.EntityFrameworkCore.Sqlite.Core" Version="3.1.0" />
  <PackageReference Include="SQLitePCLRaw.bundle_sqlite3" Version="2.0.4" />

  <PackageReference Include="Microsoft.EntityFrameworkCore.Sqlite.NetTopologySuite" Version="3.1.0" />
</ItemGroup>
```

<span data-ttu-id="391bf-113">В **macOS**вам также потребуется задать переменную среды перед запуском приложения, чтобы использовать версию SQLite для Homebrew.</span><span class="sxs-lookup"><span data-stu-id="391bf-113">On **macOS**, you'll also need set an environment variable before running your app so it uses Homebrew's version of SQLite.</span></span> <span data-ttu-id="391bf-114">В Visual Studio для Mac это можно сделать в разделе **Параметры проекта > проекта > запуск конфигураций > > умолчанию**</span><span class="sxs-lookup"><span data-stu-id="391bf-114">In Visual Studio for Mac, you can set this under **Project > Project Options > Run > Configurations > Default**</span></span>

```bash
DYLD_LIBRARY_PATH=/usr/local/opt/sqlite/lib
```

## <a name="configuring-srid"></a><span data-ttu-id="391bf-115">Настройка SRID</span><span class="sxs-lookup"><span data-stu-id="391bf-115">Configuring SRID</span></span>

<span data-ttu-id="391bf-116">В Спатиалите столбцам необходимо указать SRID для каждого столбца.</span><span class="sxs-lookup"><span data-stu-id="391bf-116">In SpatiaLite, columns need to specify an SRID per column.</span></span> <span data-ttu-id="391bf-117">По умолчанию SRID имеет значение `0` .</span><span class="sxs-lookup"><span data-stu-id="391bf-117">The default SRID is `0`.</span></span> <span data-ttu-id="391bf-118">Укажите другой SRID с помощью метода Форсклитехассрид.</span><span class="sxs-lookup"><span data-stu-id="391bf-118">Specify a different SRID using the ForSqliteHasSrid method.</span></span>

```csharp
modelBuilder.Entity<City>().Property(c => c.Location)
    .ForSqliteHasSrid(4326);
```

> [!NOTE]
> <span data-ttu-id="391bf-119">4326 относится к WGS 84, стандарту, используемому в GPS и других географических системах.</span><span class="sxs-lookup"><span data-stu-id="391bf-119">4326 refers to WGS 84, a standard used in GPS and other geographic systems.</span></span>

## <a name="dimension"></a><span data-ttu-id="391bf-120">Измерение</span><span class="sxs-lookup"><span data-stu-id="391bf-120">Dimension</span></span>

<span data-ttu-id="391bf-121">По умолчанию для столбца используется измерение X и Y. Чтобы включить дополнительные координаты, такие как Z или M, настройте тип столбца.</span><span class="sxs-lookup"><span data-stu-id="391bf-121">The default dimension (or ordinates) of a column is X and Y. To enable additional ordinates like Z or M, configure the column type.</span></span>

```csharp
modelBuilder.Entity<City>().Property(c => c.Location)
    .HasColumnType("POINTZ");
```

## <a name="spatial-function-mappings"></a><span data-ttu-id="391bf-122">Сопоставления пространственных функций</span><span class="sxs-lookup"><span data-stu-id="391bf-122">Spatial function mappings</span></span>

<span data-ttu-id="391bf-123">В этой таблице показано, какие элементы [неттопологисуите](https://nettopologysuite.github.io/NetTopologySuite/) (NTS) преобразуются в функции SQL.</span><span class="sxs-lookup"><span data-stu-id="391bf-123">This table shows which [NetTopologySuite](https://nettopologysuite.github.io/NetTopologySuite/) (NTS) members are translated into which SQL functions.</span></span>

<span data-ttu-id="391bf-124">.NET</span><span class="sxs-lookup"><span data-stu-id="391bf-124">.NET</span></span>                                        | <span data-ttu-id="391bf-125">SQL</span><span class="sxs-lookup"><span data-stu-id="391bf-125">SQL</span></span>
------------------------------------------- | ---
<span data-ttu-id="391bf-126">объект. Гистограмм</span><span class="sxs-lookup"><span data-stu-id="391bf-126">geometry.Area</span></span>                               | <span data-ttu-id="391bf-127">Область ( @geometry )</span><span class="sxs-lookup"><span data-stu-id="391bf-127">Area(@geometry)</span></span>
<span data-ttu-id="391bf-128">объект. Асбинари ()</span><span class="sxs-lookup"><span data-stu-id="391bf-128">geometry.AsBinary()</span></span>                         | <span data-ttu-id="391bf-129">Асбинари ( @geometry )</span><span class="sxs-lookup"><span data-stu-id="391bf-129">AsBinary(@geometry)</span></span>
<span data-ttu-id="391bf-130">объект. Астекст ()</span><span class="sxs-lookup"><span data-stu-id="391bf-130">geometry.AsText()</span></span>                           | <span data-ttu-id="391bf-131">Астекст ( @geometry )</span><span class="sxs-lookup"><span data-stu-id="391bf-131">AsText(@geometry)</span></span>
<span data-ttu-id="391bf-132">объект. Границ</span><span class="sxs-lookup"><span data-stu-id="391bf-132">geometry.Boundary</span></span>                           | <span data-ttu-id="391bf-133">Граница ( @geometry )</span><span class="sxs-lookup"><span data-stu-id="391bf-133">Boundary(@geometry)</span></span>
<span data-ttu-id="391bf-134">объект. Буфер (расстояние)</span><span class="sxs-lookup"><span data-stu-id="391bf-134">geometry.Buffer(distance)</span></span>                   | <span data-ttu-id="391bf-135">Буфер ( @geometry , @distance )</span><span class="sxs-lookup"><span data-stu-id="391bf-135">Buffer(@geometry, @distance)</span></span>
<span data-ttu-id="391bf-136">объект. Буфер (расстояние, Куадрантсегментс)</span><span class="sxs-lookup"><span data-stu-id="391bf-136">geometry.Buffer(distance, quadrantSegments)</span></span> | <span data-ttu-id="391bf-137">Буфер ( @geometry , @distance , @quadrantSegments )</span><span class="sxs-lookup"><span data-stu-id="391bf-137">Buffer(@geometry, @distance, @quadrantSegments)</span></span>
<span data-ttu-id="391bf-138">объект. Центроид</span><span class="sxs-lookup"><span data-stu-id="391bf-138">geometry.Centroid</span></span>                           | <span data-ttu-id="391bf-139">Центроид ( @geometry )</span><span class="sxs-lookup"><span data-stu-id="391bf-139">Centroid(@geometry)</span></span>
<span data-ttu-id="391bf-140">объект. Содержит (g)</span><span class="sxs-lookup"><span data-stu-id="391bf-140">geometry.Contains(g)</span></span>                        | <span data-ttu-id="391bf-141">Содержит ( @geometry , @g )</span><span class="sxs-lookup"><span data-stu-id="391bf-141">Contains(@geometry, @g)</span></span>
<span data-ttu-id="391bf-142">объект. Конвексхулл ()</span><span class="sxs-lookup"><span data-stu-id="391bf-142">geometry.ConvexHull()</span></span>                       | <span data-ttu-id="391bf-143">Конвексхулл ( @geometry )</span><span class="sxs-lookup"><span data-stu-id="391bf-143">ConvexHull(@geometry)</span></span>
<span data-ttu-id="391bf-144">объект. Ковередби (g)</span><span class="sxs-lookup"><span data-stu-id="391bf-144">geometry.CoveredBy(g)</span></span>                       | <span data-ttu-id="391bf-145">Ковередби ( @geometry , @g )</span><span class="sxs-lookup"><span data-stu-id="391bf-145">CoveredBy(@geometry, @g)</span></span>
<span data-ttu-id="391bf-146">объект. Обложки (g)</span><span class="sxs-lookup"><span data-stu-id="391bf-146">geometry.Covers(g)</span></span>                          | <span data-ttu-id="391bf-147">Охватывает ( @geometry , @g )</span><span class="sxs-lookup"><span data-stu-id="391bf-147">Covers(@geometry, @g)</span></span>
<span data-ttu-id="391bf-148">объект. Перекрестные (g)</span><span class="sxs-lookup"><span data-stu-id="391bf-148">geometry.Crosses(g)</span></span>                         | <span data-ttu-id="391bf-149">Перекрестные ( @geometry , @g )</span><span class="sxs-lookup"><span data-stu-id="391bf-149">Crosses(@geometry, @g)</span></span>
<span data-ttu-id="391bf-150">объект. Разница (другая)</span><span class="sxs-lookup"><span data-stu-id="391bf-150">geometry.Difference(other)</span></span>                  | <span data-ttu-id="391bf-151">Разница ( @geometry , @other )</span><span class="sxs-lookup"><span data-stu-id="391bf-151">Difference(@geometry, @other)</span></span>
<span data-ttu-id="391bf-152">объект. Фокусирован</span><span class="sxs-lookup"><span data-stu-id="391bf-152">geometry.Dimension</span></span>                          | <span data-ttu-id="391bf-153">Dimension ( @geometry )</span><span class="sxs-lookup"><span data-stu-id="391bf-153">Dimension(@geometry)</span></span>
<span data-ttu-id="391bf-154">объект. Несвязанная (g)</span><span class="sxs-lookup"><span data-stu-id="391bf-154">geometry.Disjoint(g)</span></span>                        | <span data-ttu-id="391bf-155">Несвязанная ( @geometry , @g )</span><span class="sxs-lookup"><span data-stu-id="391bf-155">Disjoint(@geometry, @g)</span></span>
<span data-ttu-id="391bf-156">объект. Расстояние (g)</span><span class="sxs-lookup"><span data-stu-id="391bf-156">geometry.Distance(g)</span></span>                        | <span data-ttu-id="391bf-157">Расстояние ( @geometry , @g )</span><span class="sxs-lookup"><span data-stu-id="391bf-157">Distance(@geometry, @g)</span></span>
<span data-ttu-id="391bf-158">объект. Оболочки</span><span class="sxs-lookup"><span data-stu-id="391bf-158">geometry.Envelope</span></span>                           | <span data-ttu-id="391bf-159">Конверт ( @geometry )</span><span class="sxs-lookup"><span data-stu-id="391bf-159">Envelope(@geometry)</span></span>
<span data-ttu-id="391bf-160">объект. Екуалстопологикалли (g)</span><span class="sxs-lookup"><span data-stu-id="391bf-160">geometry.EqualsTopologically(g)</span></span>             | <span data-ttu-id="391bf-161">Equals ( @geometry , @g )</span><span class="sxs-lookup"><span data-stu-id="391bf-161">Equals(@geometry, @g)</span></span>
<span data-ttu-id="391bf-162">объект. жеометритипе</span><span class="sxs-lookup"><span data-stu-id="391bf-162">geometry.GeometryType</span></span>                       | <span data-ttu-id="391bf-163">Жеометритипе ( @geometry )</span><span class="sxs-lookup"><span data-stu-id="391bf-163">GeometryType(@geometry)</span></span>
<span data-ttu-id="391bf-164">объект. Жетжеометрин (n)</span><span class="sxs-lookup"><span data-stu-id="391bf-164">geometry.GetGeometryN(n)</span></span>                    | <span data-ttu-id="391bf-165">Жеометрин ( @geometry , @n + 1)</span><span class="sxs-lookup"><span data-stu-id="391bf-165">GeometryN(@geometry, @n + 1)</span></span>
<span data-ttu-id="391bf-166">объект. интериорпоинт</span><span class="sxs-lookup"><span data-stu-id="391bf-166">geometry.InteriorPoint</span></span>                      | <span data-ttu-id="391bf-167">Поинтонсурфаце ( @geometry )</span><span class="sxs-lookup"><span data-stu-id="391bf-167">PointOnSurface(@geometry)</span></span>
<span data-ttu-id="391bf-168">объект. Пересечение (другое)</span><span class="sxs-lookup"><span data-stu-id="391bf-168">geometry.Intersection(other)</span></span>                | <span data-ttu-id="391bf-169">Пересечение ( @geometry , @other )</span><span class="sxs-lookup"><span data-stu-id="391bf-169">Intersection(@geometry, @other)</span></span>
<span data-ttu-id="391bf-170">объект. Пересечения (g)</span><span class="sxs-lookup"><span data-stu-id="391bf-170">geometry.Intersects(g)</span></span>                      | <span data-ttu-id="391bf-171">Пересечения ( @geometry , @g )</span><span class="sxs-lookup"><span data-stu-id="391bf-171">Intersects(@geometry, @g)</span></span>
<span data-ttu-id="391bf-172">объект. IsEmpty</span><span class="sxs-lookup"><span data-stu-id="391bf-172">geometry.IsEmpty</span></span>                            | <span data-ttu-id="391bf-173">IsEmpty ( @geometry )</span><span class="sxs-lookup"><span data-stu-id="391bf-173">IsEmpty(@geometry)</span></span>
<span data-ttu-id="391bf-174">объект. Простой</span><span class="sxs-lookup"><span data-stu-id="391bf-174">geometry.IsSimple</span></span>                           | <span data-ttu-id="391bf-175">Простое_имя ( @geometry )</span><span class="sxs-lookup"><span data-stu-id="391bf-175">IsSimple(@geometry)</span></span>
<span data-ttu-id="391bf-176">объект. IsValid</span><span class="sxs-lookup"><span data-stu-id="391bf-176">geometry.IsValid</span></span>                            | <span data-ttu-id="391bf-177">IsValid ( @geometry )</span><span class="sxs-lookup"><span data-stu-id="391bf-177">IsValid(@geometry)</span></span>
<span data-ttu-id="391bf-178">объект. Исвисиндистанце (жеом, distance)</span><span class="sxs-lookup"><span data-stu-id="391bf-178">geometry.IsWithinDistance(geom, distance)</span></span>   | <span data-ttu-id="391bf-179">Расстояние ( @geometry , @geom ) <= @distance</span><span class="sxs-lookup"><span data-stu-id="391bf-179">Distance(@geometry, @geom) <= @distance</span></span>
<span data-ttu-id="391bf-180">объект. Недопустим</span><span class="sxs-lookup"><span data-stu-id="391bf-180">geometry.Length</span></span>                             | <span data-ttu-id="391bf-181">Гленгс ( @geometry )</span><span class="sxs-lookup"><span data-stu-id="391bf-181">GLength(@geometry)</span></span>
<span data-ttu-id="391bf-182">объект. нумжеометриес</span><span class="sxs-lookup"><span data-stu-id="391bf-182">geometry.NumGeometries</span></span>                      | <span data-ttu-id="391bf-183">Нумжеометриес ( @geometry )</span><span class="sxs-lookup"><span data-stu-id="391bf-183">NumGeometries(@geometry)</span></span>
<span data-ttu-id="391bf-184">объект. нумпоинтс</span><span class="sxs-lookup"><span data-stu-id="391bf-184">geometry.NumPoints</span></span>                          | <span data-ttu-id="391bf-185">Нумпоинтс ( @geometry )</span><span class="sxs-lookup"><span data-stu-id="391bf-185">NumPoints(@geometry)</span></span>
<span data-ttu-id="391bf-186">объект. огкжеометритипе</span><span class="sxs-lookup"><span data-stu-id="391bf-186">geometry.OgcGeometryType</span></span>                    | <span data-ttu-id="391bf-187">CASE Жеометритипе ( @geometry ), когда "Point" then 1... КОНЦЕ</span><span class="sxs-lookup"><span data-stu-id="391bf-187">CASE GeometryType(@geometry) WHEN 'POINT' THEN 1 ... END</span></span>
<span data-ttu-id="391bf-188">объект. Перекрытия (g)</span><span class="sxs-lookup"><span data-stu-id="391bf-188">geometry.Overlaps(g)</span></span>                        | <span data-ttu-id="391bf-189">Перекрытия ( @geometry , @g )</span><span class="sxs-lookup"><span data-stu-id="391bf-189">Overlaps(@geometry, @g)</span></span>
<span data-ttu-id="391bf-190">объект. поинтонсурфаце</span><span class="sxs-lookup"><span data-stu-id="391bf-190">geometry.PointOnSurface</span></span>                     | <span data-ttu-id="391bf-191">Поинтонсурфаце ( @geometry )</span><span class="sxs-lookup"><span data-stu-id="391bf-191">PointOnSurface(@geometry)</span></span>
<span data-ttu-id="391bf-192">объект. Связь (g, Интерсектионпаттерн)</span><span class="sxs-lookup"><span data-stu-id="391bf-192">geometry.Relate(g, intersectionPattern)</span></span>     | <span data-ttu-id="391bf-193">Связь ( @geometry , @g , @intersectionPattern )</span><span class="sxs-lookup"><span data-stu-id="391bf-193">Relate(@geometry, @g, @intersectionPattern)</span></span>
<span data-ttu-id="391bf-194">объект. Reverse ()</span><span class="sxs-lookup"><span data-stu-id="391bf-194">geometry.Reverse()</span></span>                          | <span data-ttu-id="391bf-195">ST_Reverse ( @geometry )</span><span class="sxs-lookup"><span data-stu-id="391bf-195">ST_Reverse(@geometry)</span></span>
<span data-ttu-id="391bf-196">объект. SRID</span><span class="sxs-lookup"><span data-stu-id="391bf-196">geometry.SRID</span></span>                               | <span data-ttu-id="391bf-197">SRID ( @geometry )</span><span class="sxs-lookup"><span data-stu-id="391bf-197">SRID(@geometry)</span></span>
<span data-ttu-id="391bf-198">объект. Симметрикдифференце (другой)</span><span class="sxs-lookup"><span data-stu-id="391bf-198">geometry.SymmetricDifference(other)</span></span>         | <span data-ttu-id="391bf-199">Симдифференце ( @geometry , @other )</span><span class="sxs-lookup"><span data-stu-id="391bf-199">SymDifference(@geometry, @other)</span></span>
<span data-ttu-id="391bf-200">объект. Тобинари ()</span><span class="sxs-lookup"><span data-stu-id="391bf-200">geometry.ToBinary()</span></span>                         | <span data-ttu-id="391bf-201">Асбинари ( @geometry )</span><span class="sxs-lookup"><span data-stu-id="391bf-201">AsBinary(@geometry)</span></span>
<span data-ttu-id="391bf-202">объект. Тотекст ()</span><span class="sxs-lookup"><span data-stu-id="391bf-202">geometry.ToText()</span></span>                           | <span data-ttu-id="391bf-203">Астекст ( @geometry )</span><span class="sxs-lookup"><span data-stu-id="391bf-203">AsText(@geometry)</span></span>
<span data-ttu-id="391bf-204">объект. Касается (g)</span><span class="sxs-lookup"><span data-stu-id="391bf-204">geometry.Touches(g)</span></span>                         | <span data-ttu-id="391bf-205">Касается ( @geometry , @g )</span><span class="sxs-lookup"><span data-stu-id="391bf-205">Touches(@geometry, @g)</span></span>
<span data-ttu-id="391bf-206">объект. Union ()</span><span class="sxs-lookup"><span data-stu-id="391bf-206">geometry.Union()</span></span>                            | <span data-ttu-id="391bf-207">Унарюнион ( @geometry )</span><span class="sxs-lookup"><span data-stu-id="391bf-207">UnaryUnion(@geometry)</span></span>
<span data-ttu-id="391bf-208">объект. Объединение (другое)</span><span class="sxs-lookup"><span data-stu-id="391bf-208">geometry.Union(other)</span></span>                       | <span data-ttu-id="391bf-209">Гунион ( @geometry , @other )</span><span class="sxs-lookup"><span data-stu-id="391bf-209">GUnion(@geometry, @other)</span></span>
<span data-ttu-id="391bf-210">объект. В пределах (g)</span><span class="sxs-lookup"><span data-stu-id="391bf-210">geometry.Within(g)</span></span>                          | <span data-ttu-id="391bf-211">Внутри ( @geometry , @g )</span><span class="sxs-lookup"><span data-stu-id="391bf-211">Within(@geometry, @g)</span></span>
<span data-ttu-id="391bf-212">geometryCollection [i]</span><span class="sxs-lookup"><span data-stu-id="391bf-212">geometryCollection[i]</span></span>                       | <span data-ttu-id="391bf-213">Жеометрин ( @geometryCollection , @i + 1)</span><span class="sxs-lookup"><span data-stu-id="391bf-213">GeometryN(@geometryCollection, @i + 1)</span></span>
<span data-ttu-id="391bf-214">geometryCollection. Count</span><span class="sxs-lookup"><span data-stu-id="391bf-214">geometryCollection.Count</span></span>                    | <span data-ttu-id="391bf-215">Нумжеометриес ( @geometryCollection )</span><span class="sxs-lookup"><span data-stu-id="391bf-215">NumGeometries(@geometryCollection)</span></span>
<span data-ttu-id="391bf-216">lineString. Count</span><span class="sxs-lookup"><span data-stu-id="391bf-216">lineString.Count</span></span>                            | <span data-ttu-id="391bf-217">Нумпоинтс ( @lineString )</span><span class="sxs-lookup"><span data-stu-id="391bf-217">NumPoints(@lineString)</span></span>
<span data-ttu-id="391bf-218">lineString. EndPoint</span><span class="sxs-lookup"><span data-stu-id="391bf-218">lineString.EndPoint</span></span>                         | <span data-ttu-id="391bf-219">Конечная точка ( @lineString )</span><span class="sxs-lookup"><span data-stu-id="391bf-219">EndPoint(@lineString)</span></span>
<span data-ttu-id="391bf-220">lineString. Жетпоинтн (n)</span><span class="sxs-lookup"><span data-stu-id="391bf-220">lineString.GetPointN(n)</span></span>                     | <span data-ttu-id="391bf-221">Поинтн ( @lineString , @n + 1)</span><span class="sxs-lookup"><span data-stu-id="391bf-221">PointN(@lineString, @n + 1)</span></span>
<span data-ttu-id="391bf-222">lineString. крыл</span><span class="sxs-lookup"><span data-stu-id="391bf-222">lineString.IsClosed</span></span>                         | <span data-ttu-id="391bf-223">Закрыто ( @lineString )</span><span class="sxs-lookup"><span data-stu-id="391bf-223">IsClosed(@lineString)</span></span>
<span data-ttu-id="391bf-224">lineString. Звонок</span><span class="sxs-lookup"><span data-stu-id="391bf-224">lineString.IsRing</span></span>                           | <span data-ttu-id="391bf-225">Звонок ( @lineString )</span><span class="sxs-lookup"><span data-stu-id="391bf-225">IsRing(@lineString)</span></span>
<span data-ttu-id="391bf-226">lineString. StartPoint</span><span class="sxs-lookup"><span data-stu-id="391bf-226">lineString.StartPoint</span></span>                       | <span data-ttu-id="391bf-227">StartPoint ( @lineString )</span><span class="sxs-lookup"><span data-stu-id="391bf-227">StartPoint(@lineString)</span></span>
<span data-ttu-id="391bf-228">multiLineString. крыл</span><span class="sxs-lookup"><span data-stu-id="391bf-228">multiLineString.IsClosed</span></span>                    | <span data-ttu-id="391bf-229">Закрыто ( @multiLineString )</span><span class="sxs-lookup"><span data-stu-id="391bf-229">IsClosed(@multiLineString)</span></span>
<span data-ttu-id="391bf-230">точки. Пн</span><span class="sxs-lookup"><span data-stu-id="391bf-230">point.M</span></span>                                     | <span data-ttu-id="391bf-231">M ( @point )</span><span class="sxs-lookup"><span data-stu-id="391bf-231">M(@point)</span></span>
<span data-ttu-id="391bf-232">точки. X</span><span class="sxs-lookup"><span data-stu-id="391bf-232">point.X</span></span>                                     | <span data-ttu-id="391bf-233">X ( @point )</span><span class="sxs-lookup"><span data-stu-id="391bf-233">X(@point)</span></span>
<span data-ttu-id="391bf-234">точки. &</span><span class="sxs-lookup"><span data-stu-id="391bf-234">point.Y</span></span>                                     | <span data-ttu-id="391bf-235">Y ( @point )</span><span class="sxs-lookup"><span data-stu-id="391bf-235">Y(@point)</span></span>
<span data-ttu-id="391bf-236">точки. Гармошкой</span><span class="sxs-lookup"><span data-stu-id="391bf-236">point.Z</span></span>                                     | <span data-ttu-id="391bf-237">Z ( @point )</span><span class="sxs-lookup"><span data-stu-id="391bf-237">Z(@point)</span></span>
<span data-ttu-id="391bf-238">фигуры. екстериорринг</span><span class="sxs-lookup"><span data-stu-id="391bf-238">polygon.ExteriorRing</span></span>                        | <span data-ttu-id="391bf-239">Екстериорринг ( @polygon )</span><span class="sxs-lookup"><span data-stu-id="391bf-239">ExteriorRing(@polygon)</span></span>
<span data-ttu-id="391bf-240">фигуры. Жетинтериоррингн (n)</span><span class="sxs-lookup"><span data-stu-id="391bf-240">polygon.GetInteriorRingN(n)</span></span>                 | <span data-ttu-id="391bf-241">Интериоррингн ( @polygon , @n + 1)</span><span class="sxs-lookup"><span data-stu-id="391bf-241">InteriorRingN(@polygon, @n + 1)</span></span>
<span data-ttu-id="391bf-242">фигуры. нуминтериоррингс</span><span class="sxs-lookup"><span data-stu-id="391bf-242">polygon.NumInteriorRings</span></span>                    | <span data-ttu-id="391bf-243">Нуминтериорринг ( @polygon )</span><span class="sxs-lookup"><span data-stu-id="391bf-243">NumInteriorRing(@polygon)</span></span>

## <a name="additional-resources"></a><span data-ttu-id="391bf-244">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="391bf-244">Additional resources</span></span>

* [<span data-ttu-id="391bf-245">Домашняя страница Спатиалите</span><span class="sxs-lookup"><span data-stu-id="391bf-245">SpatiaLite Homepage</span></span>](https://www.gaia-gis.it/fossil/libspatialite)
* [<span data-ttu-id="391bf-246">Документы Неттопологисуите</span><span class="sxs-lookup"><span data-stu-id="391bf-246">NetTopologySuite Docs</span></span>](https://nettopologysuite.github.io/NetTopologySuite/)
