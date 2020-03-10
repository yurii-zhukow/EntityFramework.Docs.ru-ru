---
title: Сохранение данных — EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: ef044629-feca-4fd1-a48f-d208daedaf92
uid: core/saving/index
ms.openlocfilehash: c610ea2a9138482f93d2d54c9085ef827af276c8
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/06/2020
ms.locfileid: "78413099"
---
# <a name="saving-data"></a><span data-ttu-id="b4b32-102">Сохранение данных</span><span class="sxs-lookup"><span data-stu-id="b4b32-102">Saving Data</span></span>

<span data-ttu-id="b4b32-103">Каждый экземпляр контекста имеет `ChangeTracker`, отвечающий за отслеживание изменений, которые требуется записать в базу данных.</span><span class="sxs-lookup"><span data-stu-id="b4b32-103">Each context instance has a `ChangeTracker` that is responsible for keeping track of changes that need to be written to the database.</span></span> <span data-ttu-id="b4b32-104">При внесении изменений в экземпляры классов сущностей эти изменения регистрируются в `ChangeTracker` и затем записываются в базу данных при вызове `SaveChanges`.</span><span class="sxs-lookup"><span data-stu-id="b4b32-104">As you make changes to instances of your entity classes, these changes are recorded in the `ChangeTracker` and then written to the database when you call `SaveChanges`.</span></span> <span data-ttu-id="b4b32-105">Поставщик баз данных обеспечивает преобразование этих изменений в операции для конкретной базы данных (например, команды `INSERT`, `UPDATE` и `DELETE` для реляционной базы данных).</span><span class="sxs-lookup"><span data-stu-id="b4b32-105">The database provider is responsible for translating the changes into database-specific operations (for example, `INSERT`, `UPDATE`, and `DELETE` commands for a relational database).</span></span>
