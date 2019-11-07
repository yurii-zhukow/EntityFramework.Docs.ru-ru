---
title: Наследование — EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 754be334-dd21-450e-9d22-2591e80012a2
uid: core/modeling/inheritance
ms.openlocfilehash: abc1caa4d3839b7cdb52b316bcfc8f648b609b70
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/06/2019
ms.locfileid: "73655690"
---
# <a name="inheritance"></a><span data-ttu-id="3b010-102">Наследование</span><span class="sxs-lookup"><span data-stu-id="3b010-102">Inheritance</span></span>

<span data-ttu-id="3b010-103">Наследование в модели EF используется для управления тем, как наследование в классах сущностей представлено в базе данных.</span><span class="sxs-lookup"><span data-stu-id="3b010-103">Inheritance in the EF model is used to control how inheritance in the entity classes is represented in the database.</span></span>

## <a name="conventions"></a><span data-ttu-id="3b010-104">Соглашения</span><span class="sxs-lookup"><span data-stu-id="3b010-104">Conventions</span></span>

<span data-ttu-id="3b010-105">По соглашению поставщик базы данных должен определить, как будет представлено наследование в базе данных.</span><span class="sxs-lookup"><span data-stu-id="3b010-105">By convention, it is up to the database provider to determine how inheritance will be represented in the database.</span></span> <span data-ttu-id="3b010-106">Сведения об обработке этого с помощью поставщика реляционной базы данных см. в разделе [наследование (реляционная база данных)](relational/inheritance.md) .</span><span class="sxs-lookup"><span data-stu-id="3b010-106">See [Inheritance (Relational Database)](relational/inheritance.md) for how this is handled with a relational database provider.</span></span>

<span data-ttu-id="3b010-107">EF будет настраивать наследование только в том случае, если в модель явно включены два или более унаследованных типа.</span><span class="sxs-lookup"><span data-stu-id="3b010-107">EF will only setup inheritance if two or more inherited types are explicitly included in the model.</span></span> <span data-ttu-id="3b010-108">EF не будет проверять базовые или производные типы, которые не были добавлены в модель иным образом.</span><span class="sxs-lookup"><span data-stu-id="3b010-108">EF will not scan for base or derived types that were not otherwise included in the model.</span></span> <span data-ttu-id="3b010-109">В модель можно включить типы, предоставив *DbSet\<* для каждого типа в иерархии наследования.</span><span class="sxs-lookup"><span data-stu-id="3b010-109">You can include types in the model by exposing a *DbSet\<TEntity>* for each type in the inheritance hierarchy.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/InheritanceDbSets.cs?highlight=3-4&name=Model)]

<span data-ttu-id="3b010-110">Если вы не хотите предоставлять *DbSet\<* для одной или нескольких сущностей в иерархии, можно использовать API Fluent, чтобы убедиться, что они включены в модель.</span><span class="sxs-lookup"><span data-stu-id="3b010-110">If you don't want to expose a *DbSet\<TEntity>* for one or more entities in the hierarchy, you can use the Fluent API to ensure they are included in the model.</span></span>
<span data-ttu-id="3b010-111">Если вы не полагаетесь на соглашения, можно явно указать базовый тип с помощью `HasBaseType`.</span><span class="sxs-lookup"><span data-stu-id="3b010-111">And if you don't rely on conventions, you can specify the base type explicitly using `HasBaseType`.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/InheritanceModelBuilder.cs?highlight=7&name=Context)]

> [!NOTE]
> <span data-ttu-id="3b010-112">Для удаления типа сущности из иерархии можно использовать `.HasBaseType((Type)null)`.</span><span class="sxs-lookup"><span data-stu-id="3b010-112">You can use `.HasBaseType((Type)null)` to remove an entity type from the hierarchy.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="3b010-113">Заметки к данным</span><span class="sxs-lookup"><span data-stu-id="3b010-113">Data Annotations</span></span>

<span data-ttu-id="3b010-114">Для настройки наследования нельзя использовать заметки к данным.</span><span class="sxs-lookup"><span data-stu-id="3b010-114">You cannot use Data Annotations to configure inheritance.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="3b010-115">Текучий API</span><span class="sxs-lookup"><span data-stu-id="3b010-115">Fluent API</span></span>

<span data-ttu-id="3b010-116">API-интерфейс Fluent для наследования зависит от используемого поставщика базы данных.</span><span class="sxs-lookup"><span data-stu-id="3b010-116">The Fluent API for inheritance depends on the database provider you are using.</span></span> <span data-ttu-id="3b010-117">См. раздел [наследование (реляционная база данных)](relational/inheritance.md) для конфигурации, которую можно выполнить для поставщика реляционной базы данных.</span><span class="sxs-lookup"><span data-stu-id="3b010-117">See [Inheritance (Relational Database)](relational/inheritance.md) for the configuration you can perform for a relational database provider.</span></span>
