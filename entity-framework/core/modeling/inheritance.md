---
title: Наследование — EF Core
description: Настройка наследования типов сущностей с помощью Entity Framework Core
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 10/27/2016
uid: core/modeling/inheritance
ms.openlocfilehash: 4d43a432174c92ab7f3f9d78a234aefb0a4a17e8
ms.sourcegitcommit: 7a709ce4f77134782393aa802df5ab2718714479
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/04/2019
ms.locfileid: "74824676"
---
# <a name="inheritance"></a><span data-ttu-id="c5dac-103">Наследование</span><span class="sxs-lookup"><span data-stu-id="c5dac-103">Inheritance</span></span>

<span data-ttu-id="c5dac-104">Наследование в модели EF используется для управления тем, как наследование в классах сущностей представлено в базе данных.</span><span class="sxs-lookup"><span data-stu-id="c5dac-104">Inheritance in the EF model is used to control how inheritance in the entity classes is represented in the database.</span></span>

## <a name="conventions"></a><span data-ttu-id="c5dac-105">Обозначения</span><span class="sxs-lookup"><span data-stu-id="c5dac-105">Conventions</span></span>

<span data-ttu-id="c5dac-106">По умолчанию поставщик базы данных определяет, как будет представлено наследование в базе данных.</span><span class="sxs-lookup"><span data-stu-id="c5dac-106">By default, it is up to the database provider to determine how inheritance will be represented in the database.</span></span> <span data-ttu-id="c5dac-107">Сведения об обработке этого с помощью поставщика реляционной базы данных см. в разделе [наследование (реляционная база данных)](relational/inheritance.md) .</span><span class="sxs-lookup"><span data-stu-id="c5dac-107">See [Inheritance (Relational Database)](relational/inheritance.md) for how this is handled with a relational database provider.</span></span>

<span data-ttu-id="c5dac-108">EF будет настраивать наследование только в том случае, если в модель явно включены два или более унаследованных типа.</span><span class="sxs-lookup"><span data-stu-id="c5dac-108">EF will only setup inheritance if two or more inherited types are explicitly included in the model.</span></span> <span data-ttu-id="c5dac-109">EF не будет проверять базовые или производные типы, которые не были добавлены в модель иным образом.</span><span class="sxs-lookup"><span data-stu-id="c5dac-109">EF will not scan for base or derived types that were not otherwise included in the model.</span></span> <span data-ttu-id="c5dac-110">В модель можно включить типы, предоставив *DbSet\<* для каждого типа в иерархии наследования.</span><span class="sxs-lookup"><span data-stu-id="c5dac-110">You can include types in the model by exposing a *DbSet\<TEntity>* for each type in the inheritance hierarchy.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/InheritanceDbSets.cs?highlight=3-4&name=Model)]

<span data-ttu-id="c5dac-111">Если вы не хотите предоставлять *DbSet\<* для одной или нескольких сущностей в иерархии, можно использовать API Fluent, чтобы убедиться, что они включены в модель.</span><span class="sxs-lookup"><span data-stu-id="c5dac-111">If you don't want to expose a *DbSet\<TEntity>* for one or more entities in the hierarchy, you can use the Fluent API to ensure they are included in the model.</span></span>
<span data-ttu-id="c5dac-112">Если вы не полагаетесь на соглашения, можно явно указать базовый тип с помощью `HasBaseType`.</span><span class="sxs-lookup"><span data-stu-id="c5dac-112">And if you don't rely on conventions, you can specify the base type explicitly using `HasBaseType`.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/InheritanceModelBuilder.cs?highlight=7&name=Context)]

> [!NOTE]
> <span data-ttu-id="c5dac-113">Для удаления типа сущности из иерархии можно использовать `.HasBaseType((Type)null)`.</span><span class="sxs-lookup"><span data-stu-id="c5dac-113">You can use `.HasBaseType((Type)null)` to remove an entity type from the hierarchy.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="c5dac-114">Заметки к данным</span><span class="sxs-lookup"><span data-stu-id="c5dac-114">Data Annotations</span></span>

<span data-ttu-id="c5dac-115">Для настройки наследования нельзя использовать заметки к данным.</span><span class="sxs-lookup"><span data-stu-id="c5dac-115">You cannot use Data Annotations to configure inheritance.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="c5dac-116">Текучий API</span><span class="sxs-lookup"><span data-stu-id="c5dac-116">Fluent API</span></span>

<span data-ttu-id="c5dac-117">API-интерфейс Fluent для наследования зависит от используемого поставщика базы данных.</span><span class="sxs-lookup"><span data-stu-id="c5dac-117">The Fluent API for inheritance depends on the database provider you are using.</span></span> <span data-ttu-id="c5dac-118">См. раздел [наследование (реляционная база данных)](relational/inheritance.md) для конфигурации, которую можно выполнить для поставщика реляционной базы данных.</span><span class="sxs-lookup"><span data-stu-id="c5dac-118">See [Inheritance (Relational Database)](relational/inheritance.md) for the configuration you can perform for a relational database provider.</span></span>
