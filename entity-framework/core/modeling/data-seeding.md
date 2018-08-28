---
title: Заполнение данными — EF Core
author: AndriySvyryd
ms.date: 02/23/2018
ms.assetid: 3154BF3C-1749-4C60-8D51-AE86773AA116
uid: core/modeling/data-seeding
ms.openlocfilehash: 48ba2389de4b57dbe4c2b2124911c71440d45556
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/27/2018
ms.locfileid: "42994483"
---
# <a name="data-seeding"></a>Присвоение начальных значений данных

> [!NOTE]  
> Это новая возможность в EF Core 2.1.

Заполнение данными позволяет предоставить начальные данные для заполнения базы данных. В отличие от в EF6 в EF Core, присвоение начальных значений данных, связанный с типом сущности как часть конфигурации модели. Затем EF Core [миграций](xref:core/managing-schemas/migrations/index) может автоматически вычислить, что вставить, обновить или удалить операции необходимо применить при обновлении базы данных до новой версии модели.

Например, можно использовать это для настройки начальные данные для `Blog` в `OnModelCreating`:

[!code-csharp[Main](../../../samples/core/DataSeeding/DataSeedingContext.cs?name=BlogSeed)]

Добавление сущностей, которые имеют отношение значения внешнего ключа необходимо указать. Часто свойств внешнего ключа находятся в состоянии тени, чтобы иметь возможность установить значения анонимный класс следует использовать:

[!code-csharp[Main](../../../samples/core/DataSeeding/DataSeedingContext.cs?name=PostSeed)]

После добавления сущности, рекомендуется использовать [миграций](xref:core/managing-schemas/migrations/index) для применения изменений. 

Кроме того, можно использовать `context.Database.EnsureCreated()` для создания новой базы данных, содержащей начальные данные, например для тестовой базы данных или при использовании поставщика в памяти. Обратите внимание, что если база данных уже существует, `EnsureCreated()` не обновляет схему и начальные данные в базе данных.
