---
title: "Запросы к данным — EF Core"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 7c65ec3e-46c8-48f8-8232-9e31f96c277b
ms.technology: entity-framework-core
uid: core/querying/index
ms.openlocfilehash: a2dd830b25c64b007a881c105a87b5c631b00266
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/27/2017
---
# <a name="querying-data"></a><span data-ttu-id="1a503-102">Запросы к данным</span><span class="sxs-lookup"><span data-stu-id="1a503-102">Querying Data</span></span>

<span data-ttu-id="1a503-103">Entity Framework Core использует LINQ для запроса данных из базы данных.</span><span class="sxs-lookup"><span data-stu-id="1a503-103">Entity Framework Core uses Language Integrate Query (LINQ) to query data from the database.</span></span> <span data-ttu-id="1a503-104">LINQ позволяет использовать C# (или предпочитаемый вами язык .NET) для написания строго типизированных запросов, основанных на производном контексте и классах сущностей.</span><span class="sxs-lookup"><span data-stu-id="1a503-104">LINQ allows you to use C# (or your .NET language of choice) to write strongly typed queries based on your derived context and entity classes.</span></span> <span data-ttu-id="1a503-105">Представление запроса LINQ передается поставщику базы данных, который преобразует его на язык запросов конкретной базы данных (например, SQL для реляционной базы данных).</span><span class="sxs-lookup"><span data-stu-id="1a503-105">A representation of the LINQ query is passed to the database provider, to be translated in database-specific query language (e.g. SQL for a relational database).</span></span> <span data-ttu-id="1a503-106">Дополнительные сведения по обработке запроса см. в разделе [Принцип работы запроса](overview.md).</span><span class="sxs-lookup"><span data-stu-id="1a503-106">For more detailed information on how a query is processed, see [How Query Works](overview.md).</span></span>
