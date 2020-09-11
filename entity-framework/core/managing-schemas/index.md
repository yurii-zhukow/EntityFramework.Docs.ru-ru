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
# <a name="managing-database-schemas"></a>Управление схемами баз данных

EF Core предоставляет два основных способа синхронизации схемы базы данных и модели EF Core. Чтобы выбрать один из них, определите, что является истинным — ваша схема базы данных или модель EF Core.

Если истинной должна быть модель EF Core, используйте [миграции][1]. При внесении изменений в модель EF Core этот подход постепенно применяет соответствующие изменения схемы к базе данных, чтобы она оставалась совместимой с вашей моделью EF Core.

Используйте [реконструирование][2], если хотите сделать истинной схему базы данных. Этот подход позволяет формировать DbContext и классы типов сущностей, реконструируя схему базы данных в модель EF Core.

> [!NOTE]
> [API создания и удаления][3] также позволяют создать схему базы данных из модели EF Core. Но они предназначены главным образом для тестирования, создания прототипов и других сценариев, где допустимо удаление базы данных.

  [1]: xref:core/managing-schemas/migrations/index
  [2]: xref:core/managing-schemas/scaffolding
  [3]: xref:core/managing-schemas/ensure-created
