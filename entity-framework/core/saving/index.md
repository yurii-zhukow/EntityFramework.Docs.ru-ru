---
title: Сохранение данных — EF Core
description: Общие сведения о сохранении данных с помощью Entity Framework Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: ef044629-feca-4fd1-a48f-d208daedaf92
uid: core/saving/index
ms.openlocfilehash: 13e7c470e71f9cf9a1112aad18a8902e2235cef1
ms.sourcegitcommit: 7c3939504bb9da3f46bea3443638b808c04227c2
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/09/2020
ms.locfileid: "89616310"
---
# <a name="saving-data"></a><span data-ttu-id="e4e9f-103">Сохранение данных</span><span class="sxs-lookup"><span data-stu-id="e4e9f-103">Saving Data</span></span>

<span data-ttu-id="e4e9f-104">Каждый экземпляр контекста имеет `ChangeTracker`, отвечающий за отслеживание изменений, которые требуется записать в базу данных.</span><span class="sxs-lookup"><span data-stu-id="e4e9f-104">Each context instance has a `ChangeTracker` that is responsible for keeping track of changes that need to be written to the database.</span></span> <span data-ttu-id="e4e9f-105">При внесении изменений в экземпляры классов сущностей эти изменения регистрируются в `ChangeTracker` и затем записываются в базу данных при вызове `SaveChanges`.</span><span class="sxs-lookup"><span data-stu-id="e4e9f-105">As you make changes to instances of your entity classes, these changes are recorded in the `ChangeTracker` and then written to the database when you call `SaveChanges`.</span></span> <span data-ttu-id="e4e9f-106">Поставщик баз данных обеспечивает преобразование этих изменений в операции для конкретной базы данных (например, команды `INSERT`, `UPDATE` и `DELETE` для реляционной базы данных).</span><span class="sxs-lookup"><span data-stu-id="e4e9f-106">The database provider is responsible for translating the changes into database-specific operations (for example, `INSERT`, `UPDATE`, and `DELETE` commands for a relational database).</span></span>
