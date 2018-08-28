---
title: Управление схемами баз данных — EF Core
author: bricelam
ms.date: 10/30/2017
ms.openlocfilehash: c1ebe33b5575cab76a54721ef86ecbcb7ff8b98b
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/27/2018
ms.locfileid: "42994389"
---
# <a name="managing-database-schemas"></a><span data-ttu-id="5a307-102">Управление схемами баз данных</span><span class="sxs-lookup"><span data-stu-id="5a307-102">Managing Database Schemas</span></span>
<span data-ttu-id="5a307-103">EF Core предоставляет два основных способа синхронизации схемы базы данных и модели EF Core. Чтобы выбрать один из них, определите, что является истинным — ваша схема базы данных или модель EF Core.</span><span class="sxs-lookup"><span data-stu-id="5a307-103">EF Core provides two primary ways of keeping your EF Core model and database schema in sync. To choose between the two, decide whether your EF Core model or the database schema is the source of truth.</span></span>

<span data-ttu-id="5a307-104">Если истинной должна быть модель EF Core, используйте [миграции][1].</span><span class="sxs-lookup"><span data-stu-id="5a307-104">If you want your EF Core model to be the source of truth, use [Migrations][1].</span></span> <span data-ttu-id="5a307-105">При внесении изменений в модель EF Core этот подход постепенно применяет соответствующие изменения схемы к базе данных, чтобы она оставалась совместимой с вашей моделью EF Core.</span><span class="sxs-lookup"><span data-stu-id="5a307-105">As you make changes to your EF Core model, this approach incrementally applies the corresponding schema changes to your database so that it remains compatible with your EF Core model.</span></span>

<span data-ttu-id="5a307-106">Используйте [реконструирование][2], если хотите сделать истинной схему базы данных.</span><span class="sxs-lookup"><span data-stu-id="5a307-106">Use [Reverse Engineering][2] if you want your database schema to be the source of truth.</span></span> <span data-ttu-id="5a307-107">Этот подход позволяет формировать DbContext и классы типов сущностей, реконструируя схему базы данных в модель EF Core.</span><span class="sxs-lookup"><span data-stu-id="5a307-107">This approach allows you to scaffold a DbContext and the entity type classes by reverse engineering your database schema into an EF Core model.</span></span>

> [!NOTE]
> <span data-ttu-id="5a307-108">[API создания и удаления][3] также позволяют создать схему базы данных из модели EF Core.</span><span class="sxs-lookup"><span data-stu-id="5a307-108">The [create and drop APIs][3] can also create the database schema from your EF Core model.</span></span> <span data-ttu-id="5a307-109">Но они предназначены главным образом для тестирования, создания прототипов и других сценариев, где допустимо удаление базы данных.</span><span class="sxs-lookup"><span data-stu-id="5a307-109">However, they are primarily for testing, prototyping, and other scenarios where dropping the database is acceptable.</span></span>


  [1]: migrations/index.md
  [2]: scaffolding.md
  [3]: ensure-created.md
