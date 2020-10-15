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
# <a name="spatial-data-in-the-sqlite-ef-core-provider"></a>Пространственные данные в поставщике EF Core SQLite

На этой странице содержатся дополнительные сведения об использовании пространственных данных с поставщиком базы данных SQLite. Общие сведения об использовании пространственных данных в EF Core см. в основной документации по [пространственным данным](xref:core/modeling/spatial) .

## <a name="installing-spatialite"></a>Установка Спатиалите

В Windows собственная библиотека mod_spatialite распространяется как зависимость пакета NuGet. Другие платформы должны установить его отдельно. Обычно это делается с помощью диспетчера пакетов программного обеспечения. Например, можно использовать APT в Debian и Ubuntu; и Homebrew на MacOS.

```bash
# Debian/Ubuntu
apt-get install libsqlite3-mod-spatialite

# macOS
brew install libspatialite
```

К сожалению, новые версии PROJ (зависимость от Спатиалите) несовместимы с [набором склитепклрав](/dotnet/standard/data/sqlite/custom-versions#bundles)по умолчанию EF. Это можно обойти, используя системную библиотеку SQLite.

```xml
<ItemGroup>
  <!-- Use bundle_sqlite3 instead with SpatiaLite on macOS and Linux -->
  <!--<PackageReference Include="Microsoft.EntityFrameworkCore.Sqlite" Version="3.1.0" />-->
  <PackageReference Include="Microsoft.EntityFrameworkCore.Sqlite.Core" Version="3.1.0" />
  <PackageReference Include="SQLitePCLRaw.bundle_sqlite3" Version="2.0.4" />

  <PackageReference Include="Microsoft.EntityFrameworkCore.Sqlite.NetTopologySuite" Version="3.1.0" />
</ItemGroup>
```

В **macOS**вам также потребуется задать переменную среды перед запуском приложения, чтобы использовать версию SQLite для Homebrew. В Visual Studio для Mac это можно сделать в разделе **Параметры проекта > проекта > запуск конфигураций > > умолчанию**

```bash
DYLD_LIBRARY_PATH=/usr/local/opt/sqlite/lib
```

## <a name="configuring-srid"></a>Настройка SRID

В Спатиалите столбцам необходимо указать SRID для каждого столбца. По умолчанию SRID имеет значение `0` . Укажите другой SRID с помощью метода Форсклитехассрид.

```csharp
modelBuilder.Entity<City>().Property(c => c.Location)
    .ForSqliteHasSrid(4326);
```

> [!NOTE]
> 4326 относится к WGS 84, стандарту, используемому в GPS и других географических системах.

## <a name="dimension"></a>Измерение

По умолчанию для столбца используется измерение X и Y. Чтобы включить дополнительные координаты, такие как Z или M, настройте тип столбца.

```csharp
modelBuilder.Entity<City>().Property(c => c.Location)
    .HasColumnType("POINTZ");
```

## <a name="spatial-function-mappings"></a>Сопоставления пространственных функций

В этой таблице показано, какие элементы [неттопологисуите](https://nettopologysuite.github.io/NetTopologySuite/) (NTS) преобразуются в функции SQL.

.NET                                        | SQL
------------------------------------------- | ---
объект. Гистограмм                               | Область ( @geometry )
объект. Асбинари ()                         | Асбинари ( @geometry )
объект. Астекст ()                           | Астекст ( @geometry )
объект. Границ                           | Граница ( @geometry )
объект. Буфер (расстояние)                   | Буфер ( @geometry , @distance )
объект. Буфер (расстояние, Куадрантсегментс) | Буфер ( @geometry , @distance , @quadrantSegments )
объект. Центроид                           | Центроид ( @geometry )
объект. Содержит (g)                        | Содержит ( @geometry , @g )
объект. Конвексхулл ()                       | Конвексхулл ( @geometry )
объект. Ковередби (g)                       | Ковередби ( @geometry , @g )
объект. Обложки (g)                          | Охватывает ( @geometry , @g )
объект. Перекрестные (g)                         | Перекрестные ( @geometry , @g )
объект. Разница (другая)                  | Разница ( @geometry , @other )
объект. Фокусирован                          | Dimension ( @geometry )
объект. Несвязанная (g)                        | Несвязанная ( @geometry , @g )
объект. Расстояние (g)                        | Расстояние ( @geometry , @g )
объект. Оболочки                           | Конверт ( @geometry )
объект. Екуалстопологикалли (g)             | Equals ( @geometry , @g )
объект. жеометритипе                       | Жеометритипе ( @geometry )
объект. Жетжеометрин (n)                    | Жеометрин ( @geometry , @n + 1)
объект. интериорпоинт                      | Поинтонсурфаце ( @geometry )
объект. Пересечение (другое)                | Пересечение ( @geometry , @other )
объект. Пересечения (g)                      | Пересечения ( @geometry , @g )
объект. IsEmpty                            | IsEmpty ( @geometry )
объект. Простой                           | Простое_имя ( @geometry )
объект. IsValid                            | IsValid ( @geometry )
объект. Исвисиндистанце (жеом, distance)   | Расстояние ( @geometry , @geom ) <= @distance
объект. Недопустим                             | Гленгс ( @geometry )
объект. нумжеометриес                      | Нумжеометриес ( @geometry )
объект. нумпоинтс                          | Нумпоинтс ( @geometry )
объект. огкжеометритипе                    | CASE Жеометритипе ( @geometry ), когда "Point" then 1... КОНЦЕ
объект. Перекрытия (g)                        | Перекрытия ( @geometry , @g )
объект. поинтонсурфаце                     | Поинтонсурфаце ( @geometry )
объект. Связь (g, Интерсектионпаттерн)     | Связь ( @geometry , @g , @intersectionPattern )
объект. Reverse ()                          | ST_Reverse ( @geometry )
объект. SRID                               | SRID ( @geometry )
объект. Симметрикдифференце (другой)         | Симдифференце ( @geometry , @other )
объект. Тобинари ()                         | Асбинари ( @geometry )
объект. Тотекст ()                           | Астекст ( @geometry )
объект. Касается (g)                         | Касается ( @geometry , @g )
объект. Union ()                            | Унарюнион ( @geometry )
объект. Объединение (другое)                       | Гунион ( @geometry , @other )
объект. В пределах (g)                          | Внутри ( @geometry , @g )
geometryCollection [i]                       | Жеометрин ( @geometryCollection , @i + 1)
geometryCollection. Count                    | Нумжеометриес ( @geometryCollection )
lineString. Count                            | Нумпоинтс ( @lineString )
lineString. EndPoint                         | Конечная точка ( @lineString )
lineString. Жетпоинтн (n)                     | Поинтн ( @lineString , @n + 1)
lineString. крыл                         | Закрыто ( @lineString )
lineString. Звонок                           | Звонок ( @lineString )
lineString. StartPoint                       | StartPoint ( @lineString )
multiLineString. крыл                    | Закрыто ( @multiLineString )
точки. Пн                                     | M ( @point )
точки. X                                     | X ( @point )
точки. &                                     | Y ( @point )
точки. Гармошкой                                     | Z ( @point )
фигуры. екстериорринг                        | Екстериорринг ( @polygon )
фигуры. Жетинтериоррингн (n)                 | Интериоррингн ( @polygon , @n + 1)
фигуры. нуминтериоррингс                    | Нуминтериорринг ( @polygon )

## <a name="additional-resources"></a>Дополнительные ресурсы

* [Домашняя страница Спатиалите](https://www.gaia-gis.it/fossil/libspatialite)
* [Документы Неттопологисуите](https://nettopologysuite.github.io/NetTopologySuite/)
