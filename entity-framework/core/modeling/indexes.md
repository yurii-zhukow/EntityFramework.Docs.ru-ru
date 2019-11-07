---
title: Индексы — EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 85b92003-b692-417d-ac1d-76d40dce664b
uid: core/modeling/indexes
ms.openlocfilehash: d1b5cd6853cd24f7e1aa3aace2f0a3455c657cc1
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/06/2019
ms.locfileid: "73655701"
---
# <a name="indexes"></a><span data-ttu-id="283ec-102">Индексы</span><span class="sxs-lookup"><span data-stu-id="283ec-102">Indexes</span></span>

<span data-ttu-id="283ec-103">Индексы являются распространенной концепцией для многих хранилищ данных.</span><span class="sxs-lookup"><span data-stu-id="283ec-103">Indexes are a common concept across many data stores.</span></span> <span data-ttu-id="283ec-104">Хотя их реализация в хранилище данных может различаться, они используются для выполнения уточняющих запросов, основанных на столбце (или наборе столбцов), которые более эффективны.</span><span class="sxs-lookup"><span data-stu-id="283ec-104">While their implementation in the data store may vary, they are used to make lookups based on a column (or set of columns) more efficient.</span></span>

## <a name="conventions"></a><span data-ttu-id="283ec-105">Соглашения</span><span class="sxs-lookup"><span data-stu-id="283ec-105">Conventions</span></span>

<span data-ttu-id="283ec-106">По соглашению индекс создается в каждом свойстве (или наборе свойств), используемом в качестве внешнего ключа.</span><span class="sxs-lookup"><span data-stu-id="283ec-106">By convention, an index is created in each property (or set of properties) that are used as a foreign key.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="283ec-107">Заметки к данным</span><span class="sxs-lookup"><span data-stu-id="283ec-107">Data Annotations</span></span>

<span data-ttu-id="283ec-108">Индексы нельзя создавать с помощью заметок к данным.</span><span class="sxs-lookup"><span data-stu-id="283ec-108">Indexes can not be created using data annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="283ec-109">Текучий API</span><span class="sxs-lookup"><span data-stu-id="283ec-109">Fluent API</span></span>

<span data-ttu-id="283ec-110">Вы можете использовать API Fluent, чтобы указать индекс для одного свойства.</span><span class="sxs-lookup"><span data-stu-id="283ec-110">You can use the Fluent API to specify an index on a single property.</span></span> <span data-ttu-id="283ec-111">По умолчанию индексы не являются уникальными.</span><span class="sxs-lookup"><span data-stu-id="283ec-111">By default, indexes are non-unique.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Index.cs?name=Index&highlight=7,8)]

<span data-ttu-id="283ec-112">Можно также указать, что индекс должен быть уникальным, то есть ни одна из двух сущностей не может иметь одинаковые значения для заданных свойств.</span><span class="sxs-lookup"><span data-stu-id="283ec-112">You can also specify that an index should be unique, meaning that no two entities can have the same value(s) for the given property(s).</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/IndexUnique.cs?name=ModelBuilder&highlight=3)]

<span data-ttu-id="283ec-113">Можно также указать индекс более чем для одного столбца.</span><span class="sxs-lookup"><span data-stu-id="283ec-113">You can also specify an index over more than one column.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/IndexComposite.cs?name=Composite&highlight=7,8)]

> [!TIP]  
> <span data-ttu-id="283ec-114">Для каждого отдельного набора свойств существует только один индекс.</span><span class="sxs-lookup"><span data-stu-id="283ec-114">There is only one index per distinct set of properties.</span></span> <span data-ttu-id="283ec-115">Если вы используете API Fluent для настройки индекса для набора свойств, в котором уже определен индекс, либо в соответствии с соглашением, либо с помощью предыдущей конфигурации, вы измените определение этого индекса.</span><span class="sxs-lookup"><span data-stu-id="283ec-115">If you use the Fluent API to configure an index on a set of properties that already has an index defined, either by convention or previous configuration, then you will be changing the definition of that index.</span></span> <span data-ttu-id="283ec-116">Это полезно, если необходимо дополнительно настроить индекс, созданный по соглашению.</span><span class="sxs-lookup"><span data-stu-id="283ec-116">This is useful if you want to further configure an index that was created by convention.</span></span>
