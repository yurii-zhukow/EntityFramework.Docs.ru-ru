---
title: Наследование — EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 754be334-dd21-450e-9d22-2591e80012a2
uid: core/modeling/inheritance
ms.openlocfilehash: c5fa9d13dec8cfc3e1cac69e471f509cbbb9e4c5
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/27/2018
ms.locfileid: "42995900"
---
# <a name="inheritance"></a><span data-ttu-id="31204-102">Наследование</span><span class="sxs-lookup"><span data-stu-id="31204-102">Inheritance</span></span>

<span data-ttu-id="31204-103">Наследование в модели EF используется для управления представление наследования в классы сущностей в базе данных.</span><span class="sxs-lookup"><span data-stu-id="31204-103">Inheritance in the EF model is used to control how inheritance in the entity classes is represented in the database.</span></span>

## <a name="conventions"></a><span data-ttu-id="31204-104">Соглашения</span><span class="sxs-lookup"><span data-stu-id="31204-104">Conventions</span></span>

<span data-ttu-id="31204-105">По соглашению возлагается поставщик базы данных, чтобы определить, как наследование, будет представлено в базе данных.</span><span class="sxs-lookup"><span data-stu-id="31204-105">By convention, it is up to the database provider to determine how inheritance will be represented in the database.</span></span> <span data-ttu-id="31204-106">См. в разделе [наследование (реляционная база данных)](relational/inheritance.md) это обработки с помощью поставщика реляционной базы данных.</span><span class="sxs-lookup"><span data-stu-id="31204-106">See [Inheritance (Relational Database)](relational/inheritance.md) for how this is handled with a relational database provider.</span></span>

<span data-ttu-id="31204-107">EF будет Настройка наследования только в том случае, если два или более наследуемых типов явно включены в модель.</span><span class="sxs-lookup"><span data-stu-id="31204-107">EF will only setup inheritance if two or more inherited types are explicitly included in the model.</span></span> <span data-ttu-id="31204-108">EF не будет выполнять сканирование для базовых или производных типов, в противном случае не включенные в модели.</span><span class="sxs-lookup"><span data-stu-id="31204-108">EF will not scan for base or derived types that were not otherwise included in the model.</span></span> <span data-ttu-id="31204-109">Можно включить типы в модели, предоставляя *DbSet<TEntity>*  для каждого типа в иерархии наследования.</span><span class="sxs-lookup"><span data-stu-id="31204-109">You can include types in the model by exposing a *DbSet<TEntity>* for each type in the inheritance hierarchy.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Samples/InheritanceDbSets.cs?highlight=3-4&name=Model)]

<span data-ttu-id="31204-110">Если вы не хотите предоставлять *DbSet<TEntity>*  для одной или нескольких сущностей в иерархии, можно использовать Fluent API для обеспечения того, они включаются в модель.</span><span class="sxs-lookup"><span data-stu-id="31204-110">If you don't want to expose a *DbSet<TEntity>* for one or more entities in the hierarchy, you can use the Fluent API to ensure they are included in the model.</span></span>
<span data-ttu-id="31204-111">Если вы не полагаться на соглашения можно указать базовый тип явно с помощью `HasBaseType`.</span><span class="sxs-lookup"><span data-stu-id="31204-111">And if you don't rely on conventions you can specify the base type explicitly using `HasBaseType`.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Samples/InheritanceModelBuilder.cs?highlight=7&name=Context)]

> [!NOTE]
> <span data-ttu-id="31204-112">Можно использовать `.HasBaseType((Type)null)` для удаления типа сущности в иерархии.</span><span class="sxs-lookup"><span data-stu-id="31204-112">You can use `.HasBaseType((Type)null)` to remove an entity type from the hierarchy.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="31204-113">Заметки к данным</span><span class="sxs-lookup"><span data-stu-id="31204-113">Data Annotations</span></span>

<span data-ttu-id="31204-114">Заметки к данным нельзя использовать для настройки наследования.</span><span class="sxs-lookup"><span data-stu-id="31204-114">You cannot use Data Annotations to configure inheritance.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="31204-115">Текучий API</span><span class="sxs-lookup"><span data-stu-id="31204-115">Fluent API</span></span>

<span data-ttu-id="31204-116">Fluent API для наследования зависит от поставщика базы данных, которую вы используете.</span><span class="sxs-lookup"><span data-stu-id="31204-116">The Fluent API for inheritance depends on the database provider you are using.</span></span> <span data-ttu-id="31204-117">См. в разделе [наследование (реляционная база данных)](relational/inheritance.md) для конфигурации, можно выполнить для поставщика реляционной базы данных.</span><span class="sxs-lookup"><span data-stu-id="31204-117">See [Inheritance (Relational Database)](relational/inheritance.md) for the configuration you can perform for a relational database provider.</span></span>
