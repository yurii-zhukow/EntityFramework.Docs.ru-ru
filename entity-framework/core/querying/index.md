---
title: Запросы к данным — EF Core
description: Общие сведения о запросах в Entity Framework Core
author: smitpatel
ms.date: 10/03/2019
uid: core/querying/index
ms.openlocfilehash: 7cf53d59a577c5bc45f3ea4ea0cfbe257437b44e
ms.sourcegitcommit: 0a25c03fa65ae6e0e0e3f66bac48d59eceb96a5a
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/14/2020
ms.locfileid: "92062567"
---
# <a name="querying-data"></a><span data-ttu-id="2ec19-103">Запросы к данным</span><span class="sxs-lookup"><span data-stu-id="2ec19-103">Querying Data</span></span>

<span data-ttu-id="2ec19-104">Entity Framework Core использует LINQ для запроса данных из базы данных.</span><span class="sxs-lookup"><span data-stu-id="2ec19-104">Entity Framework Core uses Language Integrated Query (LINQ) to query data from the database.</span></span> <span data-ttu-id="2ec19-105">LINQ позволяет использовать C# (или предпочитаемый вами язык .NET) для написания строго типизированных запросов.</span><span class="sxs-lookup"><span data-stu-id="2ec19-105">LINQ allows you to use C# (or your .NET language of choice) to write strongly typed queries.</span></span> <span data-ttu-id="2ec19-106">Он использует производный контекст и классы сущностей для ссылки на объекты базы данных.</span><span class="sxs-lookup"><span data-stu-id="2ec19-106">It uses your derived context and entity classes to reference database objects.</span></span> <span data-ttu-id="2ec19-107">EF Core передает поставщику базы данных представление запроса LINQ.</span><span class="sxs-lookup"><span data-stu-id="2ec19-107">EF Core passes a representation of the LINQ query to the database provider.</span></span> <span data-ttu-id="2ec19-108">Поставщик базы данных преобразует его в язык запроса базы данных (например, SQL для реляционной базы данных).</span><span class="sxs-lookup"><span data-stu-id="2ec19-108">Database providers in turn translate it to database-specific query language (for example, SQL for a relational database).</span></span>

> [!TIP]
> <span data-ttu-id="2ec19-109">Для этой статьи вы можете скачать [пример](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Querying/Overview) из репозитория GitHub.</span><span class="sxs-lookup"><span data-stu-id="2ec19-109">You can view this article's [sample](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Querying/Overview) on GitHub.</span></span>

<span data-ttu-id="2ec19-110">В следующих фрагментах кода показано несколько примеров выполнения типичных задач с использованием Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="2ec19-110">The following snippets show a few examples of how to achieve common tasks with Entity Framework Core.</span></span>

## <a name="loading-all-data"></a><span data-ttu-id="2ec19-111">Загрузка всех данных</span><span class="sxs-lookup"><span data-stu-id="2ec19-111">Loading all data</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Overview/Program.cs#LoadingAllData)]

## <a name="loading-a-single-entity"></a><span data-ttu-id="2ec19-112">Загрузка отдельной сущности</span><span class="sxs-lookup"><span data-stu-id="2ec19-112">Loading a single entity</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Overview/Program.cs#LoadingSingleEntity)]

## <a name="filtering"></a><span data-ttu-id="2ec19-113">Фильтрация</span><span class="sxs-lookup"><span data-stu-id="2ec19-113">Filtering</span></span>

[!code-csharp[Main](../../../samples/core/Querying/Overview/Program.cs#Filtering)]

## <a name="further-readings"></a><span data-ttu-id="2ec19-114">Дополнительные материалы</span><span class="sxs-lookup"><span data-stu-id="2ec19-114">Further readings</span></span>

- <span data-ttu-id="2ec19-115">См. сведения о [выражениях запросов LINQ](/dotnet/csharp/programming-guide/concepts/linq/basic-linq-query-operations).</span><span class="sxs-lookup"><span data-stu-id="2ec19-115">Learn more about [LINQ query expressions](/dotnet/csharp/programming-guide/concepts/linq/basic-linq-query-operations)</span></span>
- <span data-ttu-id="2ec19-116">Дополнительные сведения по обработке запроса в EF Core см. в статье [Как работают запросы](xref:core/querying/how-query-works).</span><span class="sxs-lookup"><span data-stu-id="2ec19-116">For more detailed information on how a query is processed in EF Core, see [How queries Work](xref:core/querying/how-query-works).</span></span>
