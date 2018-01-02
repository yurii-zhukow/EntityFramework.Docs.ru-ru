---
title: "Управление схемами баз данных — EF Core"
author: bricelam
ms.author: divega
ms.date: 10/30/2017
ms.technology: entity-framework-core
ms.openlocfilehash: 765c80f43832e51471928d5f653aa12c6bd7c7ac
ms.sourcegitcommit: b467368cc350e6059fdc0949e042a41cb11e61d9
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/15/2017
---
# <a name="managing-database-schemas"></a><span data-ttu-id="cd84c-102">Управление схемами баз данных</span><span class="sxs-lookup"><span data-stu-id="cd84c-102">Managing Database Schemas</span></span>
<span data-ttu-id="cd84c-103">EF Core предоставляет два основных способа синхронизации схемы базы данных и модели EF Core. Чтобы выбрать один из них, определите, что является истинным — ваша схема базы данных или модель EF Core.</span><span class="sxs-lookup"><span data-stu-id="cd84c-103">EF Core provides two primary ways of keeping your EF Core model and database schema in sync. To choose between the two, decide whether your EF Core model or the database schema is the source of truth.</span></span>

<span data-ttu-id="cd84c-104">Если истинной должна быть модель EF Core, используйте [миграции][1].</span><span class="sxs-lookup"><span data-stu-id="cd84c-104">If you want your EF Core model to be the source of truth, use [Migrations][1].</span></span> <span data-ttu-id="cd84c-105">При внесении изменений в модель EF Core этот подход постепенно применяет соответствующие изменения схемы к базе данных, чтобы она оставалась совместимой с вашей моделью EF Core.</span><span class="sxs-lookup"><span data-stu-id="cd84c-105">As you make changes to your EF Core model, this approach incrementally applies the corresponding schema changes to your database so that it remains compatible with your EF Core model.</span></span>

<span data-ttu-id="cd84c-106">Используйте [реконструирование][2], если хотите сделать истинной схему базы данных.</span><span class="sxs-lookup"><span data-stu-id="cd84c-106">Use [Reverse Engineering][2] if you want your database schema to be the source of truth.</span></span> <span data-ttu-id="cd84c-107">Этот подход позволяет формировать DbContext и классы типов сущностей, реконструируя схему базы данных в модель EF Core.</span><span class="sxs-lookup"><span data-stu-id="cd84c-107">This approach allows you to scaffold a DbContext and the entity type classes by reverse engineering your database schema into an EF Core model.</span></span>

> [!NOTE]
> <span data-ttu-id="cd84c-108">[API создания и удаления][3] также позволяют создать схему базы данных из модели EF Core.</span><span class="sxs-lookup"><span data-stu-id="cd84c-108">The [create and drop APIs][3] can also create the database schema from your EF Core model.</span></span> <span data-ttu-id="cd84c-109">Но они предназначены главным образом для тестирования, создания прототипов и других сценариев, где допустимо удаление базы данных.</span><span class="sxs-lookup"><span data-stu-id="cd84c-109">However, they are primarily for testing, prototyping, and other scenarios where dropping the database is acceptable.</span></span>


  [1]: migrations/index.md
  [2]: scaffolding.md
  [3]: ensure-created.md
