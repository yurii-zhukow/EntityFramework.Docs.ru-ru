---
title: Критические изменения в EF Core 5.0 — EF Core
author: bricelam
ms.date: 06/05/2020
uid: core/what-is-new/ef-core-5.0/breaking-changes
ms.openlocfilehash: 0ffe62d9b21dd901c2cdaf0ea5d416d14938497f
ms.sourcegitcommit: 92d54fe3702e0c92e198334da22bacb42e9842b1
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/10/2020
ms.locfileid: "84666171"
---
# <a name="breaking-changes-in-ef-core-50"></a>Критические изменения в EF Core 5.0

Указанные ниже изменения в API и поведении могут нарушить работу существующих приложений при их обновлении до EF Core 5.0.0.

## <a name="summary"></a>Сводка

| **Критическое изменение**                                                                                                               | **Влияние** |
|:----------------------------------------------------------------------------------------------------------------------------------|------------|
| [Из расширения NTS для SQLite удален метод HasGeometricDimension.](#removed-hasgeometricdimension-method-from-sqlite-nts-extension) | Низкий        |

### <a name="removed-hasgeometricdimension-method-from-sqlite-nts-extension"></a>Из расширения NTS для SQLite удален метод HasGeometricDimension.

[Отслеживание проблемы #14257](https://github.com/aspnet/EntityFrameworkCore/issues/14257)

**Старое поведение**

Метод HasGeometricDimension использовался для включения дополнительных измерений (Z и M) для столбцов геометрии. Однако он влиял только на создание базы данных. Его не требовалось указывать для запроса значений с дополнительными измерениями. Он также не работал корректно при вставке или обновлении значений с дополнительными измерениями ([см. проблему #14257](https://github.com/aspnet/EntityFrameworkCore/issues/14257)).

**Новое поведение**

Чтобы включить вставку и обновление геометрических значений с дополнительными измерениями (Z и M), необходимо указать измерение в составе имени типа столбца. Это более точно соответствует базовой работе функции AddGeometryColumn расширения SpatiaLite.

**Причина**

Использование метода HasGeometricDimension после указания измерения в типе столбца является ненужным и избыточным, поэтому этот метод полностью удален.

**Решение проблемы**

Чтобы указать измерение, используйте `HasColumnType`:

```cs
modelBuilder.Entity<GeoEntity>(
    x =>
    {
        // Allow any GEOMETRY value with optional Z and M values
        x.Property(e => e.Geometry).HasColumnType("GEOMETRYZM");

        // Allow only POINT values with an optional Z value
        x.Property(e => e.Point).HasColumnType("POINTZ");
    });
```
