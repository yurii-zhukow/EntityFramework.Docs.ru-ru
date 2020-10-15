---
title: Поставщик базы данных Microsoft SQL Server — пространственные данные EF Core
description: Использование пространственных данных с поставщиком базы данных Entity Framework Core Microsoft SQL Server
author: bricelam
ms.date: 10/02/2020
uid: core/providers/sql-server/spatial
ms.openlocfilehash: 1356d2d86a497f01c4eacca777a8a260f33c0e9b
ms.sourcegitcommit: 0a25c03fa65ae6e0e0e3f66bac48d59eceb96a5a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/14/2020
ms.locfileid: "92066553"
---
# <a name="spatial-data-in-the-sql-server-ef-core-provider"></a>Пространственные данные в поставщике EF Core SQL Server

На этой странице содержатся дополнительные сведения об использовании пространственных данных с поставщиком базы данных Microsoft SQL Server. Общие сведения об использовании пространственных данных в EF Core см. в основной документации по [пространственным данным](xref:core/modeling/spatial) .

## <a name="geography-or-geometry"></a>География или геометрические объекты

По умолчанию пространственные свойства сопоставляются со `geography` столбцами в SQL Server. Чтобы использовать `geometry` , [Настройте тип столбца](xref:core/modeling/entity-properties#column-data-types) в модели.

## <a name="geography-polygon-rings"></a>Кольца географических многоугольников

При использовании `geography` типа столбца SQL Server накладывает дополнительные требования на внешнее кольцо (или оболочку) и внутренние кольца (или отверстия). Внешнее кольцо должно быть ориентировано против часовой стрелки и внутренних колец по часовой стрелке. [Неттопологисуите](https://nettopologysuite.github.io/NetTopologySuite/) (NTS) проверяет это перед отправкой значений в базу данных.

## <a name="fullglobe"></a>FullGlobe

SQL Server имеет нестандартный тип геометрии для представления полного земного шара при использовании `geography` типа столбца. У него также есть способ представления многоугольников на основе полного земного шара (без внешнего кольца). Ни один из этих элементов не поддерживается NTS.

> [!WARNING]
> FullGlobe и многоугольники, основанные на нем, не поддерживаются NTS.

## <a name="curves"></a>Кривые

Как упоминалось в основной документации по [пространственным данным](xref:core/modeling/spatial) , NTS в настоящее время не может представлять кривые. Это означает, что необходимо преобразовать значения CircularString, CompoundCurve и Куреполигон с помощью метода [STCurveToLine](/sql/t-sql/spatial-geography/stcurvetoline-geography-data-type) , прежде чем использовать их в EF Core.

> [!WARNING]
> CircularString, CompoundCurve и Куреполигон не поддерживаются NTS.

## <a name="spatial-function-mappings"></a>Сопоставления пространственных функций

В этой таблице показано, какие элементы NTS преобразуются в функции SQL. Обратите внимание, что переводы зависят от того, относится ли столбец к типу geography или geometry.

.NET                                      | SQL (география)                                              | SQL (геометрия)
----------------------------------------- | ------------------------------------------------------------ | --------------
объект. Гистограмм                             | @geometry.STArea()                                           | @geometry.STArea()
объект. Асбинари ()                       | @geometry.STAsBinary()                                       | @geometry.STAsBinary()
объект. Астекст ()                         | @geometry.AsTextZM()                                         | @geometry.AsTextZM()
объект. Границ                         |                                                              | @geometry.STBoundary()
объект. Буфер (расстояние)                 | @geometry.STBuffer(@distance)                                | @geometry.STBuffer(@distance)
объект. Центроид                         |                                                              | @geometry.STCentroid()
объект. Содержит (g)                      | @geometry.STContains(@g)                                     | @geometry.STContains(@g)
объект. Конвексхулл ()                     | @geometry.STConvexHull()                                     | @geometry.STConvexHull()
объект. Перекрестные (g)                       |                                                              | @geometry.STCrosses(@g)
объект. Разница (другая)                | @geometry.STDifference(@other)                               | @geometry.STDifference(@other)
объект. Фокусирован                        | @geometry.STDimension()                                      | @geometry.STDimension()
объект. Несвязанная (g)                      | @geometry.STDisjoint(@g)                                     | @geometry.STDisjoint(@g)
объект. Расстояние (g)                      | @geometry.STDistance(@g)                                     | @geometry.STDistance(@g)
объект. Оболочки                         |                                                              | @geometry.STEnvelope()
объект. Екуалстопологикалли (g)           | @geometry.STEquals(@g)                                       | @geometry.STEquals(@g)
объект. жеометритипе                     | @geometry.STGeometryType()                                   | @geometry.STGeometryType()
объект. Жетжеометрин (n)                  | @geometry.STGeometryN( @n + 1)                                | @geometry.STGeometryN( @n + 1)
объект. интериорпоинт                    |                                                              | @geometry.STPointOnSurface()
объект. Пересечение (другое)              | @geometry.STIntersection(@other)                             | @geometry.STIntersection(@other)
объект. Пересечения (g)                    | @geometry.STIntersects(@g)                                   | @geometry.STIntersects(@g)
объект. IsEmpty                          | @geometry.STIsEmpty()                                        | @geometry.STIsEmpty()
объект. Простой                         |                                                              | @geometry.STIsSimple()
объект. IsValid                          | @geometry.STIsValid()                                        | @geometry.STIsValid()
объект. Исвисиндистанце (жеом, distance) | @geometry.STDistance( @geom ) <= @distance                     | @geometry.STDistance( @geom ) <= @distance
объект. Недопустим                           | @geometry.STLength()                                         | @geometry.STLength()
объект. нумжеометриес                    | @geometry.STNumGeometries()                                  | @geometry.STNumGeometries()
объект. нумпоинтс                        | @geometry.STNumPoints()                                      | @geometry.STNumPoints()
объект. огкжеометритипе                  | CASE @geometry.STGeometryType (), когда н'поинт ' then 1... КОНЦЕ | CASE @geometry.STGeometryType (), когда н'поинт ' then 1... КОНЦЕ
объект. Перекрытия (g)                      | @geometry.STOverlaps(@g)                                     | @geometry.STOverlaps(@g)
объект. поинтонсурфаце                   |                                                              | @geometry.STPointOnSurface()
объект. Связь (g, Интерсектионпаттерн)   |                                                              | @geometry.STRelate(@g, @intersectionPattern)
объект. SRID                             | @geometry.STSrid                                             | @geometry.STSrid
объект. Симметрикдифференце (другой)       | @geometry.STSymDifference(@other)                            | @geometry.STSymDifference(@other)
объект. Тобинари ()                       | @geometry.STAsBinary()                                       | @geometry.STAsBinary()
объект. Тотекст ()                         | @geometry.AsTextZM()                                         | @geometry.AsTextZM()
объект. Касается (g)                       |                                                              | @geometry.STTouches(@g)
объект. Объединение (другое)                     | @geometry.STUnion(@other)                                    | @geometry.STUnion(@other)
объект. В пределах (g)                        | @geometry.STWithin(@g)                                       | @geometry.STWithin(@g)
geometryCollection [i]                     | @geometryCollection.STGeometryN( @i + 1)                      | @geometryCollection.STGeometryN( @i + 1)
geometryCollection. Count                  | @geometryCollection.STNumGeometries()                        | @geometryCollection.STNumGeometries()
lineString. Count                          | @lineString.STNumPoints()                                    | @lineString.STNumPoints()
lineString. EndPoint                       | @lineString.STEndPoint()                                     | @lineString.STEndPoint()
lineString. Жетпоинтн (n)                   | @lineString.STPointN( @n + 1)                                 | @lineString.STPointN( @n + 1)
lineString. крыл                       | @lineString.STIsClosed()                                     | @lineString.STIsClosed()
lineString. Звонок                         |                                                              | @lineString.IsRing()
lineString. StartPoint                     | @lineString.STStartPoint()                                   | @lineString.STStartPoint()
multiLineString. крыл                  | @multiLineString.STIsClosed()                                | @multiLineString.STIsClosed()
точки. Пн                                   | @point.M                                                     | @point.M
точки. X                                   | @point.Long                                                  | @point.STX
точки. &                                   | @point.Lat                                                   | @point.STY
точки. Гармошкой                                   | @point.Z                                                     | @point.Z
фигуры. екстериорринг                      | @polygon.RingNодного                                            | @polygon.STExteriorRing()
фигуры. Жетинтериоррингн (n)               | @polygon.RingN( @n + 2)                                       | @polygon.STInteriorRingN( @n + 1)
фигуры. нуминтериоррингс                  | @polygon.NumRings() — 1                                      | @polygon.STNumInteriorRing()

## <a name="additional-resources"></a>Дополнительные ресурсы

* [Пространственные данные SQL Server](/sql/relational-databases/spatial/spatial-data-sql-server)
* [Документы Неттопологисуите](https://nettopologysuite.github.io/NetTopologySuite/)
