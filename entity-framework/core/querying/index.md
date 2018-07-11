---
title: Запросы к данным — EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 7c65ec3e-46c8-48f8-8232-9e31f96c277b
ms.technology: entity-framework-core
uid: core/querying/index
ms.openlocfilehash: 447f48b780bc48b7a79153d17dcc1b8ef0cc508c
ms.sourcegitcommit: bdd06c9a591ba5e6d6a3ec046c80de98f598f3f3
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/10/2018
ms.locfileid: "37949279"
---
# <a name="querying-data"></a><span data-ttu-id="8e428-102">Запросы к данным</span><span class="sxs-lookup"><span data-stu-id="8e428-102">Querying Data</span></span>

<span data-ttu-id="8e428-103">Entity Framework Core использует LINQ для запроса данных из базы данных.</span><span class="sxs-lookup"><span data-stu-id="8e428-103">Entity Framework Core uses Language Integrated Query (LINQ) to query data from the database.</span></span> <span data-ttu-id="8e428-104">LINQ позволяет использовать C# (или предпочитаемый вами язык .NET) для написания строго типизированных запросов, основанных на производном контексте и классах сущностей.</span><span class="sxs-lookup"><span data-stu-id="8e428-104">LINQ allows you to use C# (or your .NET language of choice) to write strongly typed queries based on your derived context and entity classes.</span></span> <span data-ttu-id="8e428-105">Представление запроса LINQ передается поставщику базы данных, который преобразует его в язык запросов конкретной базы данных (например, SQL для реляционной базы данных).</span><span class="sxs-lookup"><span data-stu-id="8e428-105">A representation of the LINQ query is passed to the database provider, to be translated in database-specific query language (for example, SQL for a relational database).</span></span> <span data-ttu-id="8e428-106">Дополнительные сведения по обработке запроса см. в разделе [Принцип работы запроса](overview.md).</span><span class="sxs-lookup"><span data-stu-id="8e428-106">For more detailed information on how a query is processed, see [How Query Works](overview.md).</span></span>
