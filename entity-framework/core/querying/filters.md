---
title: Глобальные фильтры запросов — EF Core
author: anpete
ms.date: 11/03/2017
uid: core/querying/filters
ms.openlocfilehash: 9262ff7970b0502945480c673315071cbc3f44b9
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/06/2020
ms.locfileid: "78413765"
---
# <a name="global-query-filters"></a><span data-ttu-id="0578f-102">Глобальные фильтры запросов</span><span class="sxs-lookup"><span data-stu-id="0578f-102">Global Query Filters</span></span>

> [!NOTE]
> <span data-ttu-id="0578f-103">Эта возможность появилась в EF Core 2.0.</span><span class="sxs-lookup"><span data-stu-id="0578f-103">This feature was introduced in EF Core 2.0.</span></span>

<span data-ttu-id="0578f-104">Глобальные фильтры запросов являются предикатами запросов LINQ (логическое выражение, которое обычно передается в оператор запроса LINQ *Where*), которые применяются непосредственно в типах сущностей в модели метаданных (обычно в *OnModelCreating*).</span><span class="sxs-lookup"><span data-stu-id="0578f-104">Global query filters are LINQ query predicates (a boolean expression typically passed to the LINQ *Where* query operator) applied to Entity Types in the metadata model (usually in *OnModelCreating*).</span></span> <span data-ttu-id="0578f-105">Такие фильтры автоматически применяются к любым запросам LINQ, связанным с этими типами сущностей, включая типы сущностей, указанные косвенно, например, с помощью оператора Include или ссылок на свойства прямой навигации.</span><span class="sxs-lookup"><span data-stu-id="0578f-105">Such filters are automatically applied to any LINQ queries involving those Entity Types, including Entity Types referenced indirectly, such as through the use of Include or direct navigation property references.</span></span> <span data-ttu-id="0578f-106">Ниже приведены некоторые типичные способы применения этой функции.</span><span class="sxs-lookup"><span data-stu-id="0578f-106">Some common applications of this feature are:</span></span>

* <span data-ttu-id="0578f-107">**Обратимое удаление**. Тип сущности определяет свойство *IsDeleted*.</span><span class="sxs-lookup"><span data-stu-id="0578f-107">**Soft delete** - An Entity Type defines an *IsDeleted* property.</span></span>
* <span data-ttu-id="0578f-108">**Мультитенантность**. Тип сущности определяет свойство *TenantId*.</span><span class="sxs-lookup"><span data-stu-id="0578f-108">**Multi-tenancy** - An Entity Type defines a *TenantId* property.</span></span>

## <a name="example"></a><span data-ttu-id="0578f-109">Пример</span><span class="sxs-lookup"><span data-stu-id="0578f-109">Example</span></span>

<span data-ttu-id="0578f-110">В следующем примере показано, как использовать глобальные фильтры запросов для реализации такого поведения запроса, как обратимое удаление и мультитенантность, в простой модели ведения блогов.</span><span class="sxs-lookup"><span data-stu-id="0578f-110">The following example shows how to use Global Query Filters to implement soft-delete and multi-tenancy query behaviors in a simple blogging model.</span></span>

> [!TIP]
> <span data-ttu-id="0578f-111">Для этой статьи вы можете скачать [пример](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/QueryFilters) из репозитория GitHub.</span><span class="sxs-lookup"><span data-stu-id="0578f-111">You can view this article's [sample](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/QueryFilters) on GitHub.</span></span>

<span data-ttu-id="0578f-112">Сначала определите сущности:</span><span class="sxs-lookup"><span data-stu-id="0578f-112">First, define the entities:</span></span>

[!code-csharp[Main](../../../samples/core/QueryFilters/Program.cs#Entities)]

<span data-ttu-id="0578f-113">Запишите объявление поля _tenantId_ в сущности _Blog_.</span><span class="sxs-lookup"><span data-stu-id="0578f-113">Note the declaration of a _tenantId_ field on the _Blog_ entity.</span></span> <span data-ttu-id="0578f-114">Оно будет использоваться для связывания каждого экземпляра блога с конкретным клиентом.</span><span class="sxs-lookup"><span data-stu-id="0578f-114">This will be used to associate each Blog instance with a specific tenant.</span></span> <span data-ttu-id="0578f-115">Также определено свойство _IsDeleted_ в типе сущности _Post_.</span><span class="sxs-lookup"><span data-stu-id="0578f-115">Also defined is an _IsDeleted_ property on the _Post_ entity type.</span></span> <span data-ttu-id="0578f-116">Оно используется, чтобы проверять, был ли экземпляр _Post_ удален "обратимо".</span><span class="sxs-lookup"><span data-stu-id="0578f-116">This is used to keep track of whether a _Post_ instance has been "soft-deleted".</span></span> <span data-ttu-id="0578f-117">То есть экземпляр помечен как удаленный без физического удаления базовых данных.</span><span class="sxs-lookup"><span data-stu-id="0578f-117">That is, the instance is marked as deleted without physically removing the underlying data.</span></span>

<span data-ttu-id="0578f-118">Затем настройте фильтры запросов в _OnModelCreating_, используя API `HasQueryFilter`.</span><span class="sxs-lookup"><span data-stu-id="0578f-118">Next, configure the query filters in _OnModelCreating_ using the `HasQueryFilter` API.</span></span>

[!code-csharp[Main](../../../samples/core/QueryFilters/Program.cs#Configuration)]

<span data-ttu-id="0578f-119">Выражения предиката, поступающие в вызовы _HasQueryFilter_, будут автоматически применяться ко всем запросам LINQ для этих типов.</span><span class="sxs-lookup"><span data-stu-id="0578f-119">The predicate expressions passed to the _HasQueryFilter_ calls will now automatically be applied to any LINQ queries for those types.</span></span>

> [!TIP]
> <span data-ttu-id="0578f-120">Обратите внимание на использование поля уровня экземпляра DbContext. `_tenantId` используется для установки текущего клиента.</span><span class="sxs-lookup"><span data-stu-id="0578f-120">Note the use of a DbContext instance level field: `_tenantId` used to set the current tenant.</span></span> <span data-ttu-id="0578f-121">Фильтры на уровне модели будут использовать значение из правильного экземпляра контекста (то есть экземпляра, выполняющего запрос).</span><span class="sxs-lookup"><span data-stu-id="0578f-121">Model-level filters will use the value from the correct context instance (that is, the instance that is executing the query).</span></span>

> [!NOTE]
> <span data-ttu-id="0578f-122">Сейчас невозможно определить несколько фильтров запросов для одной и той же сущности. Будет применен только последний из них.</span><span class="sxs-lookup"><span data-stu-id="0578f-122">It is currently not possible to define multiple query filters on the same entity - only the last one will be applied.</span></span> <span data-ttu-id="0578f-123">Однако с помощью логического оператора _AND_ ([`&&` в C#](https://docs.microsoft.com/dotnet/csharp/language-reference/operators/boolean-logical-operators#conditional-logical-and-operator-)) можно определить один фильтр с несколькими условиями.</span><span class="sxs-lookup"><span data-stu-id="0578f-123">However, you can define a single filter with multiple conditions using the logical _AND_ operator ([`&&` in C#](https://docs.microsoft.com/dotnet/csharp/language-reference/operators/boolean-logical-operators#conditional-logical-and-operator-)).</span></span>

## <a name="disabling-filters"></a><span data-ttu-id="0578f-124">Отключение фильтров</span><span class="sxs-lookup"><span data-stu-id="0578f-124">Disabling Filters</span></span>

<span data-ttu-id="0578f-125">Фильтры можно отключить для отдельных запросов LINQ, используя оператор `IgnoreQueryFilters()`.</span><span class="sxs-lookup"><span data-stu-id="0578f-125">Filters may be disabled for individual LINQ queries by using the `IgnoreQueryFilters()` operator.</span></span>

[!code-csharp[Main](../../../samples/core/QueryFilters/Program.cs#IgnoreFilters)]

## <a name="limitations"></a><span data-ttu-id="0578f-126">Ограничения</span><span class="sxs-lookup"><span data-stu-id="0578f-126">Limitations</span></span>

<span data-ttu-id="0578f-127">Глобальные фильтры запросов имеют следующие ограничения:</span><span class="sxs-lookup"><span data-stu-id="0578f-127">Global query filters have the following limitations:</span></span>

* <span data-ttu-id="0578f-128">Фильтры можно определить только для корневого типа сущности в иерархии наследования.</span><span class="sxs-lookup"><span data-stu-id="0578f-128">Filters can only be defined for the root Entity Type of an inheritance hierarchy.</span></span>
