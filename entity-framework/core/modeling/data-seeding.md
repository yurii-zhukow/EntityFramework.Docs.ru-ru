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
# <a name="data-seeding"></a>Начальное заполнение данных

> [!NOTE]  
> Этот компонент впервые появился в EF Core 2.1.

Заполнение данными позволяет предоставить исходные данные для заполнения базы данных. В отличие от EF6 EF ядер, инициализация данных не связан с тип сущности как часть конфигурации модели. Миграция EF Core можно автоматически вычисления как вставки, обновления или удаления операций необходимо применить при обновлении базы данных до новой версии модели.

Например, можно использовать это для настройки данных начальное значение для `Blog` в `OnModelCreating`:

[!code-csharp[Main](../../../samples/core/DataSeeding/DataSeedingContext.cs?name=BlogSeed)]

Добавление сущностей, которые имеют отношение значения внешнего ключа должны быть указаны. Часто свойства внешнего ключа находятся в состоянии теневого, поэтому возможность задать значения анонимный класс следует использовать:

[!code-csharp[Main](../../../samples/core/DataSeeding/DataSeedingContext.cs?name=PostSeed)]
