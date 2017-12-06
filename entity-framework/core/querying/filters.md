---
title: "Глобальный запрос фильтры - EF Core"
author: anpete
ms.author: anpete
ms.date: 11/03/2017
ms.technology: entity-framework-core
uid: core/querying/filters
ms.openlocfilehash: 4e3c3c99d155f69e00fed99c415f519808ea1a68
ms.sourcegitcommit: 6e379265e4f087fb7cf180c824722c81750554dc
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/15/2017
---
# <a name="global-query-filters"></a><span data-ttu-id="ca6af-102">Глобальный запрос фильтры</span><span class="sxs-lookup"><span data-stu-id="ca6af-102">Global Query Filters</span></span>

<span data-ttu-id="ca6af-103">Глобальный запрос фильтры являются предикатов запросов LINQ (логическое выражение, обычно передается LINQ *где* оператора запроса) применяется к типам сущностей в модели метаданных (обычно в *OnModelCreating*).</span><span class="sxs-lookup"><span data-stu-id="ca6af-103">Global query filters are LINQ query predicates (a boolean expression typically passed to the LINQ *Where* query operator) applied to Entity Types in the metadata model (usually in *OnModelCreating*).</span></span> <span data-ttu-id="ca6af-104">Такие фильтры автоматически применяются к любой запросы LINQ, связанных с этими типами сущностей, включая ссылки на свойства типов сущностей адресуемая косвенно, например, с помощью Include или прямой навигации.</span><span class="sxs-lookup"><span data-stu-id="ca6af-104">Such filters are automatically applied to any LINQ queries involving those Entity Types, including Entity Types referenced indirectly, such as through the use of Include or direct navigation property references.</span></span> <span data-ttu-id="ca6af-105">Приведены некоторые распространенные приложения этого компонента.</span><span class="sxs-lookup"><span data-stu-id="ca6af-105">Some common applications of this feature are:</span></span>

* <span data-ttu-id="ca6af-106">**Мягкая удалить** -определяет тип сущности *IsDeleted* свойство.</span><span class="sxs-lookup"><span data-stu-id="ca6af-106">**Soft delete** - An Entity Type defines an *IsDeleted* property.</span></span>
* <span data-ttu-id="ca6af-107">**Многопользовательские приложения** -определяет тип сущности *TenantId* свойство.</span><span class="sxs-lookup"><span data-stu-id="ca6af-107">**Multi-tenancy** - An Entity Type defines a *TenantId* property.</span></span>

## <a name="example"></a><span data-ttu-id="ca6af-108">Пример</span><span class="sxs-lookup"><span data-stu-id="ca6af-108">Example</span></span>

<span data-ttu-id="ca6af-109">В следующем примере показано, как использовать глобальные фильтры запроса для реализации поведения soft-delete и многопользовательский режим запросов в модели простого ведения блогов.</span><span class="sxs-lookup"><span data-stu-id="ca6af-109">The following example shows how to use Global Query Filters to implement soft-delete and multi-tenancy query behaviors in a simple blogging model.</span></span>

> [!TIP]
> <span data-ttu-id="ca6af-110">Можно просмотреть в этой статье [пример](https://github.com/aspnet/EntityFrameworkCore/tree/dev/samples/QueryFilters) на GitHub.</span><span class="sxs-lookup"><span data-stu-id="ca6af-110">You can view this article's [sample](https://github.com/aspnet/EntityFrameworkCore/tree/dev/samples/QueryFilters) on GitHub.</span></span>

<span data-ttu-id="ca6af-111">Во-первых определите сущности:</span><span class="sxs-lookup"><span data-stu-id="ca6af-111">First, define the entities:</span></span>

[!code-csharp[Main](../../../efcore-dev/samples/QueryFilters/Program.cs#Entities)]

<span data-ttu-id="ca6af-112">Обратите внимание, объявление __tenantId_ на _блог_ сущности.</span><span class="sxs-lookup"><span data-stu-id="ca6af-112">Note the declaration of a __tenantId_ field on the _Blog_ entity.</span></span> <span data-ttu-id="ca6af-113">Это будет использоваться для присоединения к ним каждый экземпляр блог конкретных клиентов.</span><span class="sxs-lookup"><span data-stu-id="ca6af-113">This will be used to associate each Blog instance with a specific tenant.</span></span> <span data-ttu-id="ca6af-114">Также определяется _IsDeleted_ свойство _Post_ типа сущности.</span><span class="sxs-lookup"><span data-stu-id="ca6af-114">Also defined is an _IsDeleted_ property on the _Post_ entity type.</span></span> <span data-ttu-id="ca6af-115">Этот параметр используется этот параметр, чтобы следить, следует ли _Post_ экземпляр был «удаляется».</span><span class="sxs-lookup"><span data-stu-id="ca6af-115">This is used this to keep track of whether a _Post_ instance has been "soft-deleted".</span></span> <span data-ttu-id="ca6af-116">Т. е. Экземпляр помечен как удаленный withouth физическое удаление базовых данных.</span><span class="sxs-lookup"><span data-stu-id="ca6af-116">I.e. The instance is marked as deleted withouth physically removing the underlying data.</span></span>

<span data-ttu-id="ca6af-117">Настройте фильтров запроса в _OnModelCreating_ с помощью ```HasQueryFilter``` API.</span><span class="sxs-lookup"><span data-stu-id="ca6af-117">Next, configure the query filters in _OnModelCreating_ using the ```HasQueryFilter``` API.</span></span>

[!code-csharp[Main](../../../efcore-dev/samples/QueryFilters/Program.cs#Configuration)]

<span data-ttu-id="ca6af-118">Предиката выражения, передаваемые _HasQueryFilter_ вызовы теперь автоматически применяются к любой запросы LINQ для этих типов.</span><span class="sxs-lookup"><span data-stu-id="ca6af-118">The predicate expressions passed to the _HasQueryFilter_ calls will now automatically be applied to any LINQ queries for those types.</span></span>

> [!TIP]
> <span data-ttu-id="ca6af-119">Обратите внимание на использование поле уровня экземпляра DbContext: ```_tenantId``` используется для задания текущего клиента.</span><span class="sxs-lookup"><span data-stu-id="ca6af-119">Note the use of a DbContext instance level field: ```_tenantId``` used to set the current tenant.</span></span> <span data-ttu-id="ca6af-120">Фильтры на уровне модели будут использовать значение из экземпляра правильный контекст.</span><span class="sxs-lookup"><span data-stu-id="ca6af-120">Model-level filters will use the value from the correct context instance.</span></span> <span data-ttu-id="ca6af-121">Т. е. Экземпляр, выполняющего запрос.</span><span class="sxs-lookup"><span data-stu-id="ca6af-121">I.e. The instance that is executing the query.</span></span>

## <a name="disabling-filters"></a><span data-ttu-id="ca6af-122">Отключение фильтров</span><span class="sxs-lookup"><span data-stu-id="ca6af-122">Disabling Filters</span></span>

<span data-ttu-id="ca6af-123">Фильтры могут быть отключены для отдельных запросов LINQ с помощью ```IgnoreQueryFilters()``` оператор.</span><span class="sxs-lookup"><span data-stu-id="ca6af-123">Filters may be disabled for individual LINQ queries by using the ```IgnoreQueryFilters()``` operator.</span></span>

[!code-csharp[Main](../../../efcore-dev/samples/QueryFilters/Program.cs#IgnoreFilters)]

## <a name="limitations"></a><span data-ttu-id="ca6af-124">Ограничения</span><span class="sxs-lookup"><span data-stu-id="ca6af-124">Limitations</span></span>

<span data-ttu-id="ca6af-125">Глобальный запрос фильтров имеют следующие ограничения:</span><span class="sxs-lookup"><span data-stu-id="ca6af-125">Global query filters have the following limitations:</span></span>

* <span data-ttu-id="ca6af-126">Фильтры не может содержать ссылки на свойства навигации.</span><span class="sxs-lookup"><span data-stu-id="ca6af-126">Filters cannot contain references to navigation properties.</span></span>
* <span data-ttu-id="ca6af-127">Фильтры могут быть определены только для корневого типа сущности в иерархии наследования.</span><span class="sxs-lookup"><span data-stu-id="ca6af-127">Filters can only be defined for the root Entity Type of an inheritance hierarchy.</span></span>
