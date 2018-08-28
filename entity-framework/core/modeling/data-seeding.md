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
# <a name="data-seeding"></a><span data-ttu-id="2ee0d-102">Присвоение начальных значений данных</span><span class="sxs-lookup"><span data-stu-id="2ee0d-102">Data Seeding</span></span>

> [!NOTE]  
> <span data-ttu-id="2ee0d-103">Это новая возможность в EF Core 2.1.</span><span class="sxs-lookup"><span data-stu-id="2ee0d-103">This feature is new in EF Core 2.1.</span></span>

<span data-ttu-id="2ee0d-104">Заполнение данными позволяет предоставить начальные данные для заполнения базы данных.</span><span class="sxs-lookup"><span data-stu-id="2ee0d-104">Data seeding allows to provide initial data to populate a database.</span></span> <span data-ttu-id="2ee0d-105">В отличие от в EF6 в EF Core, присвоение начальных значений данных, связанный с типом сущности как часть конфигурации модели.</span><span class="sxs-lookup"><span data-stu-id="2ee0d-105">Unlike in EF6, in EF Core, seeding data is associated with an entity type as part of the model configuration.</span></span> <span data-ttu-id="2ee0d-106">Затем EF Core [миграций](xref:core/managing-schemas/migrations/index) может автоматически вычислить, что вставить, обновить или удалить операции необходимо применить при обновлении базы данных до новой версии модели.</span><span class="sxs-lookup"><span data-stu-id="2ee0d-106">Then EF Core [migrations](xref:core/managing-schemas/migrations/index) can automatically compute what insert, update or delete operations need to be applied when upgrading the database to a new version of the model.</span></span>

<span data-ttu-id="2ee0d-107">Например, можно использовать это для настройки начальные данные для `Blog` в `OnModelCreating`:</span><span class="sxs-lookup"><span data-stu-id="2ee0d-107">As an example, you can use this to configure seed data for a `Blog` in `OnModelCreating`:</span></span>

[!code-csharp[Main](../../../samples/core/DataSeeding/DataSeedingContext.cs?name=BlogSeed)]

<span data-ttu-id="2ee0d-108">Добавление сущностей, которые имеют отношение значения внешнего ключа необходимо указать.</span><span class="sxs-lookup"><span data-stu-id="2ee0d-108">To add entities that have a relationship the foreign key values need to be specified.</span></span> <span data-ttu-id="2ee0d-109">Часто свойств внешнего ключа находятся в состоянии тени, чтобы иметь возможность установить значения анонимный класс следует использовать:</span><span class="sxs-lookup"><span data-stu-id="2ee0d-109">Frequently the foreign key properties are in shadow state, so to be able to set the values an anonymous class should be used:</span></span>

[!code-csharp[Main](../../../samples/core/DataSeeding/DataSeedingContext.cs?name=PostSeed)]

<span data-ttu-id="2ee0d-110">После добавления сущности, рекомендуется использовать [миграций](xref:core/managing-schemas/migrations/index) для применения изменений.</span><span class="sxs-lookup"><span data-stu-id="2ee0d-110">Once entities have been added, it is recommended to use [migrations](xref:core/managing-schemas/migrations/index) to apply changes.</span></span> 

<span data-ttu-id="2ee0d-111">Кроме того, можно использовать `context.Database.EnsureCreated()` для создания новой базы данных, содержащей начальные данные, например для тестовой базы данных или при использовании поставщика в памяти.</span><span class="sxs-lookup"><span data-stu-id="2ee0d-111">Alternatively, you can use `context.Database.EnsureCreated()` to create a new database containing the seed data, for example for a test database or when using the in-memory provider.</span></span> <span data-ttu-id="2ee0d-112">Обратите внимание, что если база данных уже существует, `EnsureCreated()` не обновляет схему и начальные данные в базе данных.</span><span class="sxs-lookup"><span data-stu-id="2ee0d-112">Note that if the database already exists, `EnsureCreated()` will neither update the schema nor the seed data in the database.</span></span>
