---
title: Запросы к данным — EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 7c65ec3e-46c8-48f8-8232-9e31f96c277b
uid: core/querying/index
ms.openlocfilehash: 51aaa5de11d3fe38b4fba82db8dcb5658088cc27
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/27/2018
ms.locfileid: "42993538"
---
# <a name="querying-data"></a><span data-ttu-id="d7720-102">Запросы к данным</span><span class="sxs-lookup"><span data-stu-id="d7720-102">Querying Data</span></span>

<span data-ttu-id="d7720-103">Entity Framework Core использует LINQ для запроса данных из базы данных.</span><span class="sxs-lookup"><span data-stu-id="d7720-103">Entity Framework Core uses Language Integrated Query (LINQ) to query data from the database.</span></span> <span data-ttu-id="d7720-104">LINQ позволяет использовать C# (или предпочитаемый вами язык .NET) для написания строго типизированных запросов, основанных на производном контексте и классах сущностей.</span><span class="sxs-lookup"><span data-stu-id="d7720-104">LINQ allows you to use C# (or your .NET language of choice) to write strongly typed queries based on your derived context and entity classes.</span></span> <span data-ttu-id="d7720-105">Представление запроса LINQ передается поставщику базы данных, который преобразует его в язык запросов конкретной базы данных (например, SQL для реляционной базы данных).</span><span class="sxs-lookup"><span data-stu-id="d7720-105">A representation of the LINQ query is passed to the database provider, to be translated in database-specific query language (for example, SQL for a relational database).</span></span> <span data-ttu-id="d7720-106">Дополнительные сведения по обработке запроса см. в разделе [Принцип работы запроса](overview.md).</span><span class="sxs-lookup"><span data-stu-id="d7720-106">For more detailed information on how a query is processed, see [How Query Works](overview.md).</span></span>
