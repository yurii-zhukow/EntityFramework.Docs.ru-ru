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
# <a name="data-seeding"></a><span data-ttu-id="2a361-102">Присвоение начальных значений данных</span><span class="sxs-lookup"><span data-stu-id="2a361-102">Data Seeding</span></span>

> [!NOTE]  
> <span data-ttu-id="2a361-103">Этот компонент впервые появился в EF Core 2.1.</span><span class="sxs-lookup"><span data-stu-id="2a361-103">This feature is new in EF Core 2.1.</span></span>

<span data-ttu-id="2a361-104">Заполнение данными позволяет предоставить исходные данные для заполнения базы данных.</span><span class="sxs-lookup"><span data-stu-id="2a361-104">Data seeding allows to provide initial data to populate a database.</span></span> <span data-ttu-id="2a361-105">В отличие от EF6 EF ядер, инициализация данных не связан с тип сущности как часть конфигурации модели.</span><span class="sxs-lookup"><span data-stu-id="2a361-105">Unlike in EF6, in EF Core, seeding data is associated with an entity type as part of the model configuration.</span></span> <span data-ttu-id="2a361-106">Затем EF Core [миграций](xref:core/managing-schemas/migrations/index) автоматически может вычислять новые вставки, обновления или удаления операций необходимо применить при обновлении базы данных до новой версии модели.</span><span class="sxs-lookup"><span data-stu-id="2a361-106">Then EF Core [migrations](xref:core/managing-schemas/migrations/index) can automatically compute what insert, update or delete operations need to be applied when upgrading the database to a new version of the model.</span></span>

<span data-ttu-id="2a361-107">Например, можно использовать это для настройки данных начальное значение для `Blog` в `OnModelCreating`:</span><span class="sxs-lookup"><span data-stu-id="2a361-107">As an example, you can use this to configure seed data for a `Blog` in `OnModelCreating`:</span></span>

[!code-csharp[Main](../../../samples/core/DataSeeding/DataSeedingContext.cs?name=BlogSeed)]

<span data-ttu-id="2a361-108">Добавление сущностей, которые имеют отношение значения внешнего ключа должны быть указаны.</span><span class="sxs-lookup"><span data-stu-id="2a361-108">To add entities that have a relationship the foreign key values need to be specified.</span></span> <span data-ttu-id="2a361-109">Часто свойства внешнего ключа находятся в состоянии теневого, поэтому возможность задать значения анонимный класс следует использовать:</span><span class="sxs-lookup"><span data-stu-id="2a361-109">Frequently the foreign key properties are in shadow state, so to be able to set the values an anonymous class should be used:</span></span>

[!code-csharp[Main](../../../samples/core/DataSeeding/DataSeedingContext.cs?name=PostSeed)]

<span data-ttu-id="2a361-110">После добавления сущностей, рекомендуется использовать [миграций](xref:core/managing-schemas/migrations/index) для применения изменений.</span><span class="sxs-lookup"><span data-stu-id="2a361-110">Once entities have been added, it is recommended to use [migrations](xref:core/managing-schemas/migrations/index) to apply changes.</span></span> 

<span data-ttu-id="2a361-111">Кроме того, можно использовать `context.Database.EnsureCreated()` для создания новой базы данных, содержащий данные начальное значение, например для тестовой базы данных или при использовании поставщика в памяти.</span><span class="sxs-lookup"><span data-stu-id="2a361-111">Alternatively, you can use `context.Database.EnsureCreated()` to create a new database containing the seed data, for example for a test database or when using the in-memory provider.</span></span> <span data-ttu-id="2a361-112">Обратите внимание, что если база данных уже существует, `EnsureCreated()` будет ни обновлять схему ни начального значения данных в базе данных.</span><span class="sxs-lookup"><span data-stu-id="2a361-112">Note that if the database already exists, `EnsureCreated()` will neither update the schema nor the seed data in the database.</span></span>
