---
title: Заполнение данными - EF Core
author: AndriySvyryd
ms.author: divega
ms.date: 02/23/2018
ms.assetid: 3154BF3C-1749-4C60-8D51-AE86773AA116
ms.technology: entity-framework-core
uid: core/modeling/data-seeding
ms.openlocfilehash: 7028e1923152b27f56721dab75aae8b9c2f5ad75
ms.sourcegitcommit: 038acd91ce2f5a28d76dcd2eab72eeba225e366d
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/14/2018
ms.locfileid: "34163204"
---
# <a name="data-seeding"></a>Присвоение начальных значений данных

> [!NOTE]  
> Этот компонент впервые появился в EF Core 2.1.

Заполнение данными позволяет предоставить исходные данные для заполнения базы данных. В отличие от EF6 EF ядер, инициализация данных не связан с тип сущности как часть конфигурации модели. Затем EF Core [миграций](xref:core/managing-schemas/migrations/index) автоматически может вычислять новые вставки, обновления или удаления операций необходимо применить при обновлении базы данных до новой версии модели.

Например, можно использовать это для настройки данных начальное значение для `Blog` в `OnModelCreating`:

[!code-csharp[Main](../../../samples/core/DataSeeding/DataSeedingContext.cs?name=BlogSeed)]

Добавление сущностей, которые имеют отношение значения внешнего ключа должны быть указаны. Часто свойства внешнего ключа находятся в состоянии теневого, поэтому возможность задать значения анонимный класс следует использовать:

[!code-csharp[Main](../../../samples/core/DataSeeding/DataSeedingContext.cs?name=PostSeed)]

После добавления сущностей, рекомендуется использовать [миграций](xref:core/managing-schemas/migrations/index) для применения изменений. 

Кроме того, можно использовать `context.Database.EnsureCreated()` для создания новой базы данных, содержащий данные начальное значение, например для тестовой базы данных или при использовании поставщика в памяти. Обратите внимание, что если база данных уже существует, `EnsureCreated()` будет ни обновлять схему ни начального значения данных в базе данных.
