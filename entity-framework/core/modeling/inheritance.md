---
title: Наследование - EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 754be334-dd21-450e-9d22-2591e80012a2
ms.technology: entity-framework-core
uid: core/modeling/inheritance
ms.openlocfilehash: f0394bc55dfbfea8277b1ddf898361165dd1fe51
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/27/2017
ms.locfileid: "26052784"
---
# <a name="inheritance"></a><span data-ttu-id="0fc13-102">Наследование</span><span class="sxs-lookup"><span data-stu-id="0fc13-102">Inheritance</span></span>

<span data-ttu-id="0fc13-103">Наследование в модели EF используется для управления, как наследование в классах сущностей представляется в базе данных.</span><span class="sxs-lookup"><span data-stu-id="0fc13-103">Inheritance in the EF model is used to control how inheritance in the entity classes is represented in the database.</span></span>

## <a name="conventions"></a><span data-ttu-id="0fc13-104">Соглашения</span><span class="sxs-lookup"><span data-stu-id="0fc13-104">Conventions</span></span>

<span data-ttu-id="0fc13-105">По соглашению именно поставщик базы данных, чтобы определить представление наследования в базе данных.</span><span class="sxs-lookup"><span data-stu-id="0fc13-105">By convention, it is up to the database provider to determine how inheritance will be represented in the database.</span></span> <span data-ttu-id="0fc13-106">В разделе [наследования (реляционной базы данных)](relational/inheritance.md) это обработки с помощью реляционной базы данных поставщика.</span><span class="sxs-lookup"><span data-stu-id="0fc13-106">See [Inheritance (Relational Database)](relational/inheritance.md) for how this is handled with a relational database provider.</span></span>

<span data-ttu-id="0fc13-107">EF будет настройте наследования только в том случае, если два или более наследуемых типов явно включены в модель.</span><span class="sxs-lookup"><span data-stu-id="0fc13-107">EF will only setup inheritance if two or more inherited types are explicitly included in the model.</span></span> <span data-ttu-id="0fc13-108">EF не будут проверять базового или производного типа, в противном случае не включенные в модели.</span><span class="sxs-lookup"><span data-stu-id="0fc13-108">EF will not scan for base or derived types that were not otherwise included in the model.</span></span> <span data-ttu-id="0fc13-109">Типы можно включить в модель, предоставляя *DbSet<TEntity>*  для каждого типа в иерархии наследования.</span><span class="sxs-lookup"><span data-stu-id="0fc13-109">You can include types in the model by exposing a *DbSet<TEntity>* for each type in the inheritance hierarchy.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Samples/InheritanceDbSets.cs?highlight=3-4&name=Model)]

<span data-ttu-id="0fc13-110">Если вы не хотите предоставлять *DbSet<TEntity>*  для одной или нескольких сущностей в иерархии, чтобы они включаются в модель можно использовать Fluent API.</span><span class="sxs-lookup"><span data-stu-id="0fc13-110">If you don't want to expose a *DbSet<TEntity>* for one or more entities in the hierarchy, you can use the Fluent API to ensure they are included in the model.</span></span>
<span data-ttu-id="0fc13-111">Если не полагаться на соглашения можно указать базовый тип явно с помощью `HasBaseType`.</span><span class="sxs-lookup"><span data-stu-id="0fc13-111">And if you don't rely on conventions you can specify the base type explicitly using `HasBaseType`.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Samples/InheritanceModelBuilder.cs?highlight=7&name=Context)]

> [!NOTE]
> <span data-ttu-id="0fc13-112">Можно использовать `.HasBaseType((Type)null)` для удаления типа сущности в иерархии.</span><span class="sxs-lookup"><span data-stu-id="0fc13-112">You can use `.HasBaseType((Type)null)` to remove an entity type from the hierarchy.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="0fc13-113">Заметки к данным</span><span class="sxs-lookup"><span data-stu-id="0fc13-113">Data Annotations</span></span>

<span data-ttu-id="0fc13-114">Заметки данных нельзя использовать для настройки наследования.</span><span class="sxs-lookup"><span data-stu-id="0fc13-114">You cannot use Data Annotations to configure inheritance.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="0fc13-115">Fluent API</span><span class="sxs-lookup"><span data-stu-id="0fc13-115">Fluent API</span></span>

<span data-ttu-id="0fc13-116">Гибкий интерфейс API для наследования зависит от поставщика базы данных, которую вы используете.</span><span class="sxs-lookup"><span data-stu-id="0fc13-116">The Fluent API for inheritance depends on the database provider you are using.</span></span> <span data-ttu-id="0fc13-117">В разделе [наследования (реляционной базы данных)](relational/inheritance.md) для конфигурации, можно выполнять для поставщика реляционной базы данных.</span><span class="sxs-lookup"><span data-stu-id="0fc13-117">See [Inheritance (Relational Database)](relational/inheritance.md) for the configuration you can perform for a relational database provider.</span></span>
