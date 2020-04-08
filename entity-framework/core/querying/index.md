---
title: Запросы к данным — EF Core
author: smitpatel
ms.date: 10/03/2019
ms.assetid: 7c65ec3e-46c8-48f8-8232-9e31f96c277b
uid: core/querying/index
ms.openlocfilehash: 0e1e50d1a3f647d65301552d0a447f9fcae81438
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/07/2020
ms.locfileid: "78413119"
---
# <a name="querying-data"></a><span data-ttu-id="901b0-102">Запросы к данным</span><span class="sxs-lookup"><span data-stu-id="901b0-102">Querying Data</span></span>

<span data-ttu-id="901b0-103">Entity Framework Core использует LINQ для запроса данных из базы данных.</span><span class="sxs-lookup"><span data-stu-id="901b0-103">Entity Framework Core uses Language Integrated Query (LINQ) to query data from the database.</span></span> <span data-ttu-id="901b0-104">LINQ позволяет использовать C# (или предпочитаемый вами язык .NET) для написания строго типизированных запросов.</span><span class="sxs-lookup"><span data-stu-id="901b0-104">LINQ allows you to use C# (or your .NET language of choice) to write strongly typed queries.</span></span> <span data-ttu-id="901b0-105">Он использует производный контекст и классы сущностей для ссылки на объекты базы данных.</span><span class="sxs-lookup"><span data-stu-id="901b0-105">It uses your derived context and entity classes to reference database objects.</span></span> <span data-ttu-id="901b0-106">EF Core передает поставщику базы данных представление запроса LINQ.</span><span class="sxs-lookup"><span data-stu-id="901b0-106">EF Core passes a representation of the LINQ query to the database provider.</span></span> <span data-ttu-id="901b0-107">Поставщик базы данных преобразует его в язык запроса базы данных (например, SQL для реляционной базы данных).</span><span class="sxs-lookup"><span data-stu-id="901b0-107">Database providers in turn translate it to database-specific query language (for example, SQL for a relational database).</span></span>

> [!TIP]
> <span data-ttu-id="901b0-108">Для этой статьи вы можете скачать [пример](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Querying) из репозитория GitHub.</span><span class="sxs-lookup"><span data-stu-id="901b0-108">You can view this article's [sample](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Querying) on GitHub.</span></span>

<span data-ttu-id="901b0-109">В следующих фрагментах кода показано несколько примеров выполнения типичных задач с использованием Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="901b0-109">The following snippets show a few examples of how to achieve common tasks with Entity Framework Core.</span></span>

## <a name="loading-all-data"></a><span data-ttu-id="901b0-110">Загрузка всех данных</span><span class="sxs-lookup"><span data-stu-id="901b0-110">Loading all data</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Basics/Sample.cs#LoadingAllData)]

## <a name="loading-a-single-entity"></a><span data-ttu-id="901b0-111">Загрузка отдельной сущности</span><span class="sxs-lookup"><span data-stu-id="901b0-111">Loading a single entity</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Basics/Sample.cs#LoadingSingleEntity)]

## <a name="filtering"></a><span data-ttu-id="901b0-112">Фильтрация</span><span class="sxs-lookup"><span data-stu-id="901b0-112">Filtering</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Basics/Sample.cs#Filtering)]

## <a name="further-readings"></a><span data-ttu-id="901b0-113">Дополнительные материалы</span><span class="sxs-lookup"><span data-stu-id="901b0-113">Further readings</span></span>

- <span data-ttu-id="901b0-114">См. сведения о [выражениях запросов LINQ](/dotnet/csharp/programming-guide/concepts/linq/basic-linq-query-operations).</span><span class="sxs-lookup"><span data-stu-id="901b0-114">Learn more about [LINQ query expressions](/dotnet/csharp/programming-guide/concepts/linq/basic-linq-query-operations)</span></span>
- <span data-ttu-id="901b0-115">Дополнительные сведения по обработке запроса в EF Core см. в разделе [Принцип работы запроса](xref:core/querying/how-query-works).</span><span class="sxs-lookup"><span data-stu-id="901b0-115">For more detailed information on how a query is processed in EF Core, see [How Query Works](xref:core/querying/how-query-works).</span></span>
