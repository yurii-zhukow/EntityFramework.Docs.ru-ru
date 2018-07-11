---
title: Сохранение данных — EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: ef044629-feca-4fd1-a48f-d208daedaf92
ms.technology: entity-framework-core
uid: core/saving/index
ms.openlocfilehash: 97d7f1248a8d0adeed9714619c1364fa8f9822db
ms.sourcegitcommit: bdd06c9a591ba5e6d6a3ec046c80de98f598f3f3
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/10/2018
ms.locfileid: "37949399"
---
# <a name="saving-data"></a><span data-ttu-id="7b1c6-102">Сохранение данных</span><span class="sxs-lookup"><span data-stu-id="7b1c6-102">Saving Data</span></span>

<span data-ttu-id="7b1c6-103">Каждый экземпляр контекста имеет `ChangeTracker`, отвечающий за отслеживание изменений, которые требуется записать в базу данных.</span><span class="sxs-lookup"><span data-stu-id="7b1c6-103">Each context instance has a `ChangeTracker` that is responsible for keeping track of changes that need to be written to the database.</span></span> <span data-ttu-id="7b1c6-104">При внесении изменений в экземпляры классов сущностей эти изменения регистрируются в `ChangeTracker` и затем записываются в базу данных при вызове `SaveChanges`.</span><span class="sxs-lookup"><span data-stu-id="7b1c6-104">As you make changes to instances of your entity classes, these changes are recorded in the `ChangeTracker` and then written to the database when you call `SaveChanges`.</span></span> <span data-ttu-id="7b1c6-105">Поставщик баз данных обеспечивает преобразование этих изменений в операции для конкретной базы данных (например, команды `INSERT`, `UPDATE` и `DELETE` для реляционной базы данных).</span><span class="sxs-lookup"><span data-stu-id="7b1c6-105">The database provider is responsible for translating the changes into database-specific operations (for example, `INSERT`, `UPDATE`, and `DELETE` commands for a relational database).</span></span>
