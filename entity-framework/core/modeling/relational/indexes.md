---
title: Индексы — EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 4581e7ba-5e7f-452c-9937-0aaf790ba10a
uid: core/modeling/relational/indexes
ms.openlocfilehash: 605b30ce710d9034deab97f695496ec66a576565
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/27/2018
ms.locfileid: "42993221"
---
# <a name="indexes"></a><span data-ttu-id="85619-102">Индексы</span><span class="sxs-lookup"><span data-stu-id="85619-102">Indexes</span></span>

> [!NOTE]  
> <span data-ttu-id="85619-103">Описанная в этом разделе конфигурации применяется к реляционным базам данных в общем случае.</span><span class="sxs-lookup"><span data-stu-id="85619-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="85619-104">Показанные здесь методы расширения будут доступны после установки поставщика реляционной базы данных (посредством общего *пакета Microsoft.EntityFrameworkCore.Relational*).</span><span class="sxs-lookup"><span data-stu-id="85619-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="85619-105">Совпадает с концепцией индекса в ядро Entity Framework сопоставляет индекса в реляционной базе данных.</span><span class="sxs-lookup"><span data-stu-id="85619-105">An index in a relational database maps to the same concept as an index in the core of Entity Framework.</span></span>

## <a name="conventions"></a><span data-ttu-id="85619-106">Соглашения</span><span class="sxs-lookup"><span data-stu-id="85619-106">Conventions</span></span>

<span data-ttu-id="85619-107">По соглашению, называются индексы `IX_<type name>_<property name>`.</span><span class="sxs-lookup"><span data-stu-id="85619-107">By convention, indexes are named `IX_<type name>_<property name>`.</span></span> <span data-ttu-id="85619-108">Для индексов являются составные индексы `<property name>` становится список имен свойств, разделенных символом подчеркивания.</span><span class="sxs-lookup"><span data-stu-id="85619-108">For composite indexes `<property name>` becomes an underscore separated list of property names.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="85619-109">Заметки к данным</span><span class="sxs-lookup"><span data-stu-id="85619-109">Data Annotations</span></span>

<span data-ttu-id="85619-110">Не индексов можно настроить с помощью заметок к данным.</span><span class="sxs-lookup"><span data-stu-id="85619-110">Indexes can not be configured using Data Annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="85619-111">Текучий API</span><span class="sxs-lookup"><span data-stu-id="85619-111">Fluent API</span></span>

<span data-ttu-id="85619-112">Fluent API можно использовать для настройки имени индекса.</span><span class="sxs-lookup"><span data-stu-id="85619-112">You can use the Fluent API to configure the name of an index.</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Samples/Relational/IndexName.cs?name=Model&highlight=9)]

<span data-ttu-id="85619-113">Можно также указать фильтр.</span><span class="sxs-lookup"><span data-stu-id="85619-113">You can also specify a filter.</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Samples/Relational/IndexFilter.cs?name=Model&highlight=9)]

<span data-ttu-id="85619-114">При использовании поставщика SQL Server EF добавляет фильтрации «IS NOT NULL» для всех столбцов допускает значения NULL, которые являются частью уникального индекса.</span><span class="sxs-lookup"><span data-stu-id="85619-114">When using the SQL Server provider EF adds a 'IS NOT NULL' filter for all nullable columns that are part of a unique index.</span></span> <span data-ttu-id="85619-115">Чтобы переопределить это соглашение, можно указать `null` значение.</span><span class="sxs-lookup"><span data-stu-id="85619-115">To override this convention you can supply a `null` value.</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Samples/Relational/IndexNoFilter.cs?name=Model&highlight=10)]
