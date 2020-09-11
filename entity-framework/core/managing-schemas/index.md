---
title: Управление схемами баз данных — EF Core
description: Общие сведения о стратегиях управления схемами базы данных с помощью Entity Framework Core
author: bricelam
ms.date: 10/30/2017
uid: core/managing-schemas/index
ms.openlocfilehash: e4f8c82125534e9e7e0c2de552bce336a544a2aa
ms.sourcegitcommit: 7c3939504bb9da3f46bea3443638b808c04227c2
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/09/2020
ms.locfileid: "89619496"
---
# <a name="managing-database-schemas"></a><span data-ttu-id="dd7d1-103">Управление схемами баз данных</span><span class="sxs-lookup"><span data-stu-id="dd7d1-103">Managing Database Schemas</span></span>

<span data-ttu-id="dd7d1-104">EF Core предоставляет два основных способа синхронизации схемы базы данных и модели EF Core. Чтобы выбрать один из них, определите, что является истинным — ваша схема базы данных или модель EF Core.</span><span class="sxs-lookup"><span data-stu-id="dd7d1-104">EF Core provides two primary ways of keeping your EF Core model and database schema in sync. To choose between the two, decide whether your EF Core model or the database schema is the source of truth.</span></span>

<span data-ttu-id="dd7d1-105">Если истинной должна быть модель EF Core, используйте [миграции][1].</span><span class="sxs-lookup"><span data-stu-id="dd7d1-105">If you want your EF Core model to be the source of truth, use [Migrations][1].</span></span> <span data-ttu-id="dd7d1-106">При внесении изменений в модель EF Core этот подход постепенно применяет соответствующие изменения схемы к базе данных, чтобы она оставалась совместимой с вашей моделью EF Core.</span><span class="sxs-lookup"><span data-stu-id="dd7d1-106">As you make changes to your EF Core model, this approach incrementally applies the corresponding schema changes to your database so that it remains compatible with your EF Core model.</span></span>

<span data-ttu-id="dd7d1-107">Используйте [реконструирование][2], если хотите сделать истинной схему базы данных.</span><span class="sxs-lookup"><span data-stu-id="dd7d1-107">Use [Reverse Engineering][2] if you want your database schema to be the source of truth.</span></span> <span data-ttu-id="dd7d1-108">Этот подход позволяет формировать DbContext и классы типов сущностей, реконструируя схему базы данных в модель EF Core.</span><span class="sxs-lookup"><span data-stu-id="dd7d1-108">This approach allows you to scaffold a DbContext and the entity type classes by reverse engineering your database schema into an EF Core model.</span></span>

> [!NOTE]
> <span data-ttu-id="dd7d1-109">[API создания и удаления][3] также позволяют создать схему базы данных из модели EF Core.</span><span class="sxs-lookup"><span data-stu-id="dd7d1-109">The [create and drop APIs][3] can also create the database schema from your EF Core model.</span></span> <span data-ttu-id="dd7d1-110">Но они предназначены главным образом для тестирования, создания прототипов и других сценариев, где допустимо удаление базы данных.</span><span class="sxs-lookup"><span data-stu-id="dd7d1-110">However, they are primarily for testing, prototyping, and other scenarios where dropping the database is acceptable.</span></span>

  [1]: xref:core/managing-schemas/migrations/index
  [2]: xref:core/managing-schemas/scaffolding
  [3]: xref:core/managing-schemas/ensure-created
