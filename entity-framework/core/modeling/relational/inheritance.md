---
title: Наследование (реляционная база данных) — EF Core
description: Настройка наследования типа сущности в реляционной базе данных с помощью Entity Framework Core
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 11/06/2019
uid: core/modeling/relational/inheritance
ms.openlocfilehash: 30e25aa2968ceab03404baddb46e0ae59fc3ea6b
ms.sourcegitcommit: 7a709ce4f77134782393aa802df5ab2718714479
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/04/2019
ms.locfileid: "74824755"
---
# <a name="inheritance-relational-database"></a><span data-ttu-id="88e61-103">Наследование (реляционная база данных)</span><span class="sxs-lookup"><span data-stu-id="88e61-103">Inheritance (Relational Database)</span></span>

> [!NOTE]  
> <span data-ttu-id="88e61-104">Описанная в этом разделе конфигурации применяется к реляционным базам данных в общем случае.</span><span class="sxs-lookup"><span data-stu-id="88e61-104">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="88e61-105">Показанные здесь методы расширения будут доступны после установки поставщика реляционной базы данных (посредством общего *пакета Microsoft.EntityFrameworkCore.Relational*).</span><span class="sxs-lookup"><span data-stu-id="88e61-105">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="88e61-106">Наследование в модели EF используется для управления тем, как наследование в классах сущностей представлено в базе данных.</span><span class="sxs-lookup"><span data-stu-id="88e61-106">Inheritance in the EF model is used to control how inheritance in the entity classes is represented in the database.</span></span>

> [!NOTE]  
> <span data-ttu-id="88e61-107">В настоящее время в EF Core реализуется только шаблон «одна таблица на иерархию».</span><span class="sxs-lookup"><span data-stu-id="88e61-107">Currently, only the table-per-hierarchy (TPH) pattern is implemented in EF Core.</span></span> <span data-ttu-id="88e61-108">Другие распространенные шаблоны, такие как Table-for Type (TPT) и Table-on-Type (TPC), пока недоступны.</span><span class="sxs-lookup"><span data-stu-id="88e61-108">Other common patterns like table-per-type (TPT) and table-per-concrete-type (TPC) are not yet available.</span></span>

## <a name="conventions"></a><span data-ttu-id="88e61-109">Обозначения</span><span class="sxs-lookup"><span data-stu-id="88e61-109">Conventions</span></span>

<span data-ttu-id="88e61-110">По умолчанию наследование будет сопоставляться с использованием шаблона «одна таблица на иерархию».</span><span class="sxs-lookup"><span data-stu-id="88e61-110">By default, inheritance will be mapped using the table-per-hierarchy (TPH) pattern.</span></span> <span data-ttu-id="88e61-111">Функция «подтаблица» использует одну таблицу для хранения данных всех типов в иерархии.</span><span class="sxs-lookup"><span data-stu-id="88e61-111">TPH uses a single table to store the data for all types in the hierarchy.</span></span> <span data-ttu-id="88e61-112">Столбец дискриминатора используется для указания типа, который представляет каждая строка.</span><span class="sxs-lookup"><span data-stu-id="88e61-112">A discriminator column is used to identify which type each row represents.</span></span>

<span data-ttu-id="88e61-113">EF Core будет настраивать наследование только в том случае, если в модель явно включены два или более унаследованных типа (см. Дополнительные сведения в разделе [наследование](../inheritance.md) ).</span><span class="sxs-lookup"><span data-stu-id="88e61-113">EF Core will only setup inheritance if two or more inherited types are explicitly included in the model (see [Inheritance](../inheritance.md) for more details).</span></span>

<span data-ttu-id="88e61-114">Ниже приведен пример простого сценария наследования и данных, хранящихся в таблице реляционной базы данных с использованием шаблона «подтаблица».</span><span class="sxs-lookup"><span data-stu-id="88e61-114">Below is an example showing a simple inheritance scenario and the data stored in a relational database table using the TPH pattern.</span></span> <span data-ttu-id="88e61-115">Столбец *дискриминатора* определяет, какой тип *блога* хранится в каждой строке.</span><span class="sxs-lookup"><span data-stu-id="88e61-115">The *Discriminator* column identifies which type of *Blog* is stored in each row.</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/Conventions/InheritanceDbSets.cs#Model)]

![image](_static/inheritance-tph-data.png)

>[!NOTE]
> <span data-ttu-id="88e61-117">Столбцы базы данных автоматически допускают значение null при необходимости при использовании сопоставления «одна таблица».</span><span class="sxs-lookup"><span data-stu-id="88e61-117">Database columns are automatically made nullable as necessary when using TPH mapping.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="88e61-118">Заметки к данным</span><span class="sxs-lookup"><span data-stu-id="88e61-118">Data Annotations</span></span>

<span data-ttu-id="88e61-119">Для настройки наследования нельзя использовать заметки к данным.</span><span class="sxs-lookup"><span data-stu-id="88e61-119">You cannot use Data Annotations to configure inheritance.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="88e61-120">Текучий API</span><span class="sxs-lookup"><span data-stu-id="88e61-120">Fluent API</span></span>

<span data-ttu-id="88e61-121">API-интерфейс Fluent можно использовать для настройки имени и типа столбца дискриминатора и значений, которые используются для обнаружения каждого типа в иерархии.</span><span class="sxs-lookup"><span data-stu-id="88e61-121">You can use the Fluent API to configure the name and type of the discriminator column and the values that are used to identify each type in the hierarchy.</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/InheritanceTPHDiscriminator.cs#Inheritance)]

## <a name="configuring-the-discriminator-property"></a><span data-ttu-id="88e61-122">Настройка свойства дискриминатора</span><span class="sxs-lookup"><span data-stu-id="88e61-122">Configuring the discriminator property</span></span>

<span data-ttu-id="88e61-123">В приведенных выше примерах дискриминатор создается как [свойство теневой копии](xref:core/modeling/shadow-properties) базовой сущности иерархии.</span><span class="sxs-lookup"><span data-stu-id="88e61-123">In the examples above, the discriminator is created as a [shadow property](xref:core/modeling/shadow-properties) on the base entity of the hierarchy.</span></span> <span data-ttu-id="88e61-124">Так как это свойство в модели, его можно настроить так же, как и другие свойства.</span><span class="sxs-lookup"><span data-stu-id="88e61-124">Since it is a property in the model, it can be configured just like other properties.</span></span> <span data-ttu-id="88e61-125">Например, чтобы задать максимальную длину по умолчанию, используется дискриминатор по соглашению:</span><span class="sxs-lookup"><span data-stu-id="88e61-125">For example, to set the max length when the default, by-convention discriminator is being used:</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/DefaultDiscriminator.cs#DiscriminatorConfiguration)]

<span data-ttu-id="88e61-126">Дискриминатор также можно сопоставить со свойством .NET в сущности и настроить его.</span><span class="sxs-lookup"><span data-stu-id="88e61-126">The discriminator can also be mapped to a .NET property in your entity and configure it.</span></span> <span data-ttu-id="88e61-127">Например:</span><span class="sxs-lookup"><span data-stu-id="88e61-127">For example:</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/NonShadowDiscriminator.cs#NonShadowDiscriminator)]

## <a name="shared-columns"></a><span data-ttu-id="88e61-128">Общие столбцы</span><span class="sxs-lookup"><span data-stu-id="88e61-128">Shared columns</span></span>

<span data-ttu-id="88e61-129">Если два одноэлементных типа сущности имеют свойство с тем же именем, по умолчанию они будут сопоставляться с двумя отдельными столбцами.</span><span class="sxs-lookup"><span data-stu-id="88e61-129">When two sibling entity types have a property with the same name they will be mapped to two separate columns by default.</span></span> <span data-ttu-id="88e61-130">Но если они совместимы, они могут быть сопоставлены с одним столбцом:</span><span class="sxs-lookup"><span data-stu-id="88e61-130">But if they are compatible they can be mapped to the same column:</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/SharedTPHColumns.cs#SharedTPHColumns)]