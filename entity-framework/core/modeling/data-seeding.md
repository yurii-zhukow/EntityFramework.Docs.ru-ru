---
title: "Заполнение данными - EF Core"
author: AndriySvyryd
ms.author: divega
ms.date: 02/23/2018
ms.assetid: 3154BF3C-1749-4C60-8D51-AE86773AA116
ms.technology: entity-framework-core
uid: core/modeling/data-seeding
ms.openlocfilehash: 693ffe44e247a79e01ac7c98a36472bf2c68d37f
ms.sourcegitcommit: b2d94cebdc32edad4fecb07e53fece66437d1b04
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/28/2018
---
# <a name="data-seeding"></a><span data-ttu-id="e80cf-102">Начальное заполнение данных</span><span class="sxs-lookup"><span data-stu-id="e80cf-102">Data Seeding</span></span>

> [!NOTE]  
> <span data-ttu-id="e80cf-103">Этот компонент впервые появился в EF Core 2.1.</span><span class="sxs-lookup"><span data-stu-id="e80cf-103">This feature is new in EF Core 2.1.</span></span>

<span data-ttu-id="e80cf-104">Заполнение данными позволяет предоставить исходные данные для заполнения базы данных.</span><span class="sxs-lookup"><span data-stu-id="e80cf-104">Data seeding allows to provide initial data to populate a database.</span></span> <span data-ttu-id="e80cf-105">В отличие от EF6 EF ядер, инициализация данных не связан с тип сущности как часть конфигурации модели.</span><span class="sxs-lookup"><span data-stu-id="e80cf-105">Unlike in EF6, in EF Core, seeding data is associated with an entity type as part of the model configuration.</span></span> <span data-ttu-id="e80cf-106">Миграция EF Core можно автоматически вычисления как вставки, обновления или удаления операций необходимо применить при обновлении базы данных до новой версии модели.</span><span class="sxs-lookup"><span data-stu-id="e80cf-106">Then EF Core migrations can automatically compute what insert, update or delete operations need to be applied when upgrading the database to a new version of the model.</span></span>

<span data-ttu-id="e80cf-107">Например, можно использовать это для настройки данных начальное значение для `Blog` в `OnModelCreating`:</span><span class="sxs-lookup"><span data-stu-id="e80cf-107">As an example, you can use this to configure seed data for a `Blog` in `OnModelCreating`:</span></span>

[!code-csharp[Main](../../../samples/core/DataSeeding/DataSeedingContext.cs?name=BlogSeed)]

<span data-ttu-id="e80cf-108">Добавление сущностей, которые имеют отношение значения внешнего ключа должны быть указаны.</span><span class="sxs-lookup"><span data-stu-id="e80cf-108">To add entities that have a relationship the foreign key values need to be specified.</span></span> <span data-ttu-id="e80cf-109">Часто свойства внешнего ключа находятся в состоянии теневого, поэтому возможность задать значения анонимный класс следует использовать:</span><span class="sxs-lookup"><span data-stu-id="e80cf-109">Frequently the foreign key properties are in shadow state, so to be able to set the values an anonymous class should be used:</span></span>

[!code-csharp[Main](../../../samples/core/DataSeeding/DataSeedingContext.cs?name=PostSeed)]
