---
title: Индексы (реляционная база данных) — EF Core
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 11/05/2019
uid: core/modeling/relational/indexes
ms.openlocfilehash: e14615275f85ee9b6b32d080905465d33963feca
ms.sourcegitcommit: 7a709ce4f77134782393aa802df5ab2718714479
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/04/2019
ms.locfileid: "74824573"
---
# <a name="indexes-relational-database"></a><span data-ttu-id="acd3d-102">Индексы (реляционная база данных)</span><span class="sxs-lookup"><span data-stu-id="acd3d-102">Indexes (Relational Database)</span></span>

> [!NOTE]  
> <span data-ttu-id="acd3d-103">Описанная в этом разделе конфигурации применяется к реляционным базам данных в общем случае.</span><span class="sxs-lookup"><span data-stu-id="acd3d-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="acd3d-104">Показанные здесь методы расширения будут доступны после установки поставщика реляционной базы данных (посредством общего *пакета Microsoft.EntityFrameworkCore.Relational*).</span><span class="sxs-lookup"><span data-stu-id="acd3d-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="acd3d-105">Индекс в реляционной базе данных сопоставляется с тем же понятием, что и индекс в ядре Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="acd3d-105">An index in a relational database maps to the same concept as an index in the core of Entity Framework.</span></span>

## <a name="conventions"></a><span data-ttu-id="acd3d-106">Обозначения</span><span class="sxs-lookup"><span data-stu-id="acd3d-106">Conventions</span></span>

<span data-ttu-id="acd3d-107">По соглашению индексы называются `IX_<type name>_<property name>`.</span><span class="sxs-lookup"><span data-stu-id="acd3d-107">By convention, indexes are named `IX_<type name>_<property name>`.</span></span> <span data-ttu-id="acd3d-108">Для составных индексов `<property name>` преобразуется в список имен свойств с разделителями подчеркивания.</span><span class="sxs-lookup"><span data-stu-id="acd3d-108">For composite indexes `<property name>` becomes an underscore separated list of property names.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="acd3d-109">Заметки к данным</span><span class="sxs-lookup"><span data-stu-id="acd3d-109">Data Annotations</span></span>

<span data-ttu-id="acd3d-110">Индексы не могут быть настроены с помощью заметок к данным.</span><span class="sxs-lookup"><span data-stu-id="acd3d-110">Indexes can not be configured using Data Annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="acd3d-111">Текучий API</span><span class="sxs-lookup"><span data-stu-id="acd3d-111">Fluent API</span></span>

<span data-ttu-id="acd3d-112">Для настройки имени индекса можно использовать API-интерфейс Fluent.</span><span class="sxs-lookup"><span data-stu-id="acd3d-112">You can use the Fluent API to configure the name of an index.</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/IndexName.cs?name=Model&highlight=9)]

<span data-ttu-id="acd3d-113">Можно также указать фильтр.</span><span class="sxs-lookup"><span data-stu-id="acd3d-113">You can also specify a filter.</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/IndexFilter.cs?name=Model&highlight=9)]

<span data-ttu-id="acd3d-114">При использовании поставщика SQL Server EF добавляет фильтр `'IS NOT NULL'` для всех столбцов, допускающих значение null, которые являются частью уникального индекса.</span><span class="sxs-lookup"><span data-stu-id="acd3d-114">When using the SQL Server provider EF adds an `'IS NOT NULL'` filter for all nullable columns that are part of a unique index.</span></span> <span data-ttu-id="acd3d-115">Чтобы переопределить это соглашение, можно указать значение `null`.</span><span class="sxs-lookup"><span data-stu-id="acd3d-115">To override this convention you can supply a `null` value.</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/IndexNoFilter.cs?name=Model&highlight=10)]

### <a name="include-columns-in-sql-server-indexes"></a><span data-ttu-id="acd3d-116">Включить столбцы в индексы SQL Server</span><span class="sxs-lookup"><span data-stu-id="acd3d-116">Include Columns in SQL Server Indexes</span></span>

<span data-ttu-id="acd3d-117">Можно настроить [индексы с помощью включаемых столбцов](https://docs.microsoft.com/sql/relational-databases/indexes/create-indexes-with-included-columns) , чтобы значительно повысить производительность запросов, если все столбцы в запросе включены в индекс в качестве ключевых или неключевых столбцов.</span><span class="sxs-lookup"><span data-stu-id="acd3d-117">You can configure [indexes with included columns](https://docs.microsoft.com/sql/relational-databases/indexes/create-indexes-with-included-columns) to significantly improve query performance when all columns in the query are included in the index as key or non-key columns.</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/IndexInclude.cs?name=Model)]
