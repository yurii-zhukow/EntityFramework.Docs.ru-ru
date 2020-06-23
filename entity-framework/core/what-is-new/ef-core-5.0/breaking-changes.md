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
# <a name="breaking-changes-in-ef-core-50"></a><span data-ttu-id="343e3-102">Критические изменения в EF Core 5.0</span><span class="sxs-lookup"><span data-stu-id="343e3-102">Breaking changes in EF Core 5.0</span></span>

<span data-ttu-id="343e3-103">Указанные ниже изменения в API и поведении могут нарушить работу существующих приложений при их обновлении до EF Core 5.0.0.</span><span class="sxs-lookup"><span data-stu-id="343e3-103">The following API and behavior changes have the potential to break existing applications updating to EF Core 5.0.0.</span></span>

## <a name="summary"></a><span data-ttu-id="343e3-104">Сводка</span><span class="sxs-lookup"><span data-stu-id="343e3-104">Summary</span></span>

| <span data-ttu-id="343e3-105">**Критическое изменение**</span><span class="sxs-lookup"><span data-stu-id="343e3-105">**Breaking change**</span></span>                                                                                                               | <span data-ttu-id="343e3-106">**Влияние**</span><span class="sxs-lookup"><span data-stu-id="343e3-106">**Impact**</span></span> |
|:----------------------------------------------------------------------------------------------------------------------------------|------------|
| [<span data-ttu-id="343e3-107">Из расширения NTS для SQLite удален метод HasGeometricDimension.</span><span class="sxs-lookup"><span data-stu-id="343e3-107">Removed HasGeometricDimension method from SQLite NTS extension</span></span>](#removed-hasgeometricdimension-method-from-sqlite-nts-extension) | <span data-ttu-id="343e3-108">Низкий</span><span class="sxs-lookup"><span data-stu-id="343e3-108">Low</span></span>        |

### <a name="removed-hasgeometricdimension-method-from-sqlite-nts-extension"></a><span data-ttu-id="343e3-109">Из расширения NTS для SQLite удален метод HasGeometricDimension.</span><span class="sxs-lookup"><span data-stu-id="343e3-109">Removed HasGeometricDimension method from SQLite NTS extension</span></span>

[<span data-ttu-id="343e3-110">Отслеживание проблемы #14257</span><span class="sxs-lookup"><span data-stu-id="343e3-110">Tracking Issue #14257</span></span>](https://github.com/aspnet/EntityFrameworkCore/issues/14257)

<span data-ttu-id="343e3-111">**Старое поведение**</span><span class="sxs-lookup"><span data-stu-id="343e3-111">**Old behavior**</span></span>

<span data-ttu-id="343e3-112">Метод HasGeometricDimension использовался для включения дополнительных измерений (Z и M) для столбцов геометрии.</span><span class="sxs-lookup"><span data-stu-id="343e3-112">HasGeometricDimension was used to enable additional dimensions (Z and M) on geometry columns.</span></span> <span data-ttu-id="343e3-113">Однако он влиял только на создание базы данных.</span><span class="sxs-lookup"><span data-stu-id="343e3-113">However, it only ever affected database creation.</span></span> <span data-ttu-id="343e3-114">Его не требовалось указывать для запроса значений с дополнительными измерениями.</span><span class="sxs-lookup"><span data-stu-id="343e3-114">It was unnecessary to specify it to query values with additional dimensions.</span></span> <span data-ttu-id="343e3-115">Он также не работал корректно при вставке или обновлении значений с дополнительными измерениями ([см. проблему #14257](https://github.com/aspnet/EntityFrameworkCore/issues/14257)).</span><span class="sxs-lookup"><span data-stu-id="343e3-115">It also didn't work correctly when inserting or updating values with additional dimensions ([see #14257](https://github.com/aspnet/EntityFrameworkCore/issues/14257)).</span></span>

<span data-ttu-id="343e3-116">**Новое поведение**</span><span class="sxs-lookup"><span data-stu-id="343e3-116">**New behavior**</span></span>

<span data-ttu-id="343e3-117">Чтобы включить вставку и обновление геометрических значений с дополнительными измерениями (Z и M), необходимо указать измерение в составе имени типа столбца.</span><span class="sxs-lookup"><span data-stu-id="343e3-117">To enable inserting and updating geometry values with additional dimensions (Z and M), the dimension needs to be specified as part of the column type name.</span></span> <span data-ttu-id="343e3-118">Это более точно соответствует базовой работе функции AddGeometryColumn расширения SpatiaLite.</span><span class="sxs-lookup"><span data-stu-id="343e3-118">This more closely matches the underlying behavior of SpatiaLite's AddGeometryColumn function.</span></span>

<span data-ttu-id="343e3-119">**Причина**</span><span class="sxs-lookup"><span data-stu-id="343e3-119">**Why**</span></span>

<span data-ttu-id="343e3-120">Использование метода HasGeometricDimension после указания измерения в типе столбца является ненужным и избыточным, поэтому этот метод полностью удален.</span><span class="sxs-lookup"><span data-stu-id="343e3-120">Using HasGeometricDimension after specifying the dimension in the column type is unnecessary and redundant, so we removed HasGeometricDimension entirely.</span></span>

<span data-ttu-id="343e3-121">**Решение проблемы**</span><span class="sxs-lookup"><span data-stu-id="343e3-121">**Mitigations**</span></span>

<span data-ttu-id="343e3-122">Чтобы указать измерение, используйте `HasColumnType`:</span><span class="sxs-lookup"><span data-stu-id="343e3-122">Use `HasColumnType` to specify the dimension:</span></span>

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
