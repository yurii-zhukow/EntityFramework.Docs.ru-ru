---
title: Управление схемами баз данных — EF Core
author: bricelam
ms.date: 10/30/2017
ms.openlocfilehash: 2da17865cb0192fb3e6e3396e4ca5f31fde9c52a
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/06/2020
ms.locfileid: "78412739"
---
# <a name="managing-database-schemas"></a><span data-ttu-id="73006-102">Управление схемами баз данных</span><span class="sxs-lookup"><span data-stu-id="73006-102">Managing Database Schemas</span></span>

<span data-ttu-id="73006-103">EF Core предоставляет два основных способа синхронизации схемы базы данных и модели EF Core. Чтобы выбрать один из них, определите, что является истинным — ваша схема базы данных или модель EF Core.</span><span class="sxs-lookup"><span data-stu-id="73006-103">EF Core provides two primary ways of keeping your EF Core model and database schema in sync. To choose between the two, decide whether your EF Core model or the database schema is the source of truth.</span></span>

<span data-ttu-id="73006-104">Если истинной должна быть модель EF Core, используйте [миграции][1].</span><span class="sxs-lookup"><span data-stu-id="73006-104">If you want your EF Core model to be the source of truth, use [Migrations][1].</span></span> <span data-ttu-id="73006-105">При внесении изменений в модель EF Core этот подход постепенно применяет соответствующие изменения схемы к базе данных, чтобы она оставалась совместимой с вашей моделью EF Core.</span><span class="sxs-lookup"><span data-stu-id="73006-105">As you make changes to your EF Core model, this approach incrementally applies the corresponding schema changes to your database so that it remains compatible with your EF Core model.</span></span>

<span data-ttu-id="73006-106">Используйте [реконструирование][2], если хотите сделать истинной схему базы данных.</span><span class="sxs-lookup"><span data-stu-id="73006-106">Use [Reverse Engineering][2] if you want your database schema to be the source of truth.</span></span> <span data-ttu-id="73006-107">Этот подход позволяет формировать DbContext и классы типов сущностей, реконструируя схему базы данных в модель EF Core.</span><span class="sxs-lookup"><span data-stu-id="73006-107">This approach allows you to scaffold a DbContext and the entity type classes by reverse engineering your database schema into an EF Core model.</span></span>

> [!NOTE]
> <span data-ttu-id="73006-108">[API создания и удаления][3] также позволяют создать схему базы данных из модели EF Core.</span><span class="sxs-lookup"><span data-stu-id="73006-108">The [create and drop APIs][3] can also create the database schema from your EF Core model.</span></span> <span data-ttu-id="73006-109">Но они предназначены главным образом для тестирования, создания прототипов и других сценариев, где допустимо удаление базы данных.</span><span class="sxs-lookup"><span data-stu-id="73006-109">However, they are primarily for testing, prototyping, and other scenarios where dropping the database is acceptable.</span></span>


  [1]: migrations/index.md
  [2]: scaffolding.md
  [3]: ensure-created.md
