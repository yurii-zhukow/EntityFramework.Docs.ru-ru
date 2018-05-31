---
title: Глобальные фильтры запросов — EF Core
author: anpete
ms.author: anpete
ms.date: 11/03/2017
ms.technology: entity-framework-core
uid: core/querying/filters
ms.openlocfilehash: 4e3c3c99d155f69e00fed99c415f519808ea1a68
ms.sourcegitcommit: 6e379265e4f087fb7cf180c824722c81750554dc
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/15/2017
ms.locfileid: "26053904"
---
# <a name="global-query-filters"></a><span data-ttu-id="093cb-102">Глобальные фильтры запросов</span><span class="sxs-lookup"><span data-stu-id="093cb-102">Global Query Filters</span></span>

<span data-ttu-id="093cb-103">Глобальные фильтры запросов являются предикатами запросов LINQ (логическое выражение, которое обычно передается в оператор запроса LINQ *Where*), которые применяются непосредственно в типах сущностей в модели метаданных (обычно в *OnModelCreating*).</span><span class="sxs-lookup"><span data-stu-id="093cb-103">Global query filters are LINQ query predicates (a boolean expression typically passed to the LINQ *Where* query operator) applied to Entity Types in the metadata model (usually in *OnModelCreating*).</span></span> <span data-ttu-id="093cb-104">Такие фильтры автоматически применяются к любым запросам LINQ, связанным с этими типами сущностей, включая типы сущностей, указанные косвенно, например, с помощью оператора Include или ссылок на свойства прямой навигации.</span><span class="sxs-lookup"><span data-stu-id="093cb-104">Such filters are automatically applied to any LINQ queries involving those Entity Types, including Entity Types referenced indirectly, such as through the use of Include or direct navigation property references.</span></span> <span data-ttu-id="093cb-105">Ниже приведены некоторые типичные способы применения этой функции.</span><span class="sxs-lookup"><span data-stu-id="093cb-105">Some common applications of this feature are:</span></span>

* <span data-ttu-id="093cb-106">**Обратимое удаление**. Тип сущности определяет свойство *IsDeleted*.</span><span class="sxs-lookup"><span data-stu-id="093cb-106">**Soft delete** - An Entity Type defines an *IsDeleted* property.</span></span>
* <span data-ttu-id="093cb-107">**Мультитенантность**. Тип сущности определяет свойство *TenantId*.</span><span class="sxs-lookup"><span data-stu-id="093cb-107">**Multi-tenancy** - An Entity Type defines a *TenantId* property.</span></span>

## <a name="example"></a><span data-ttu-id="093cb-108">Пример</span><span class="sxs-lookup"><span data-stu-id="093cb-108">Example</span></span>

<span data-ttu-id="093cb-109">В следующем примере показано, как использовать глобальные фильтры запросов для реализации такого поведения запроса, как обратимое удаление и мультитенантность, в простой модели ведения блогов.</span><span class="sxs-lookup"><span data-stu-id="093cb-109">The following example shows how to use Global Query Filters to implement soft-delete and multi-tenancy query behaviors in a simple blogging model.</span></span>

> [!TIP]
> <span data-ttu-id="093cb-110">Для этой статьи вы можете скачать [пример](https://github.com/aspnet/EntityFrameworkCore/tree/dev/samples/QueryFilters) из репозитория GitHub.</span><span class="sxs-lookup"><span data-stu-id="093cb-110">You can view this article's [sample](https://github.com/aspnet/EntityFrameworkCore/tree/dev/samples/QueryFilters) on GitHub.</span></span>

<span data-ttu-id="093cb-111">Сначала определите сущности:</span><span class="sxs-lookup"><span data-stu-id="093cb-111">First, define the entities:</span></span>

[!code-csharp[Main](../../../efcore-dev/samples/QueryFilters/Program.cs#Entities)]

<span data-ttu-id="093cb-112">Запишите объявление поля __tenantId_ в сущности _Blog_.</span><span class="sxs-lookup"><span data-stu-id="093cb-112">Note the declaration of a __tenantId_ field on the _Blog_ entity.</span></span> <span data-ttu-id="093cb-113">Оно будет использоваться для связывания каждого экземпляра блога с конкретным клиентом.</span><span class="sxs-lookup"><span data-stu-id="093cb-113">This will be used to associate each Blog instance with a specific tenant.</span></span> <span data-ttu-id="093cb-114">Также определено свойство _IsDeleted_ в типе сущности _Post_.</span><span class="sxs-lookup"><span data-stu-id="093cb-114">Also defined is an _IsDeleted_ property on the _Post_ entity type.</span></span> <span data-ttu-id="093cb-115">Оно используется, чтобы проверять, был ли экземпляр _Post_ удален "обратимо".</span><span class="sxs-lookup"><span data-stu-id="093cb-115">This is used this to keep track of whether a _Post_ instance has been "soft-deleted".</span></span> <span data-ttu-id="093cb-116">То есть, экземпляр помечен как удаленный без физического удаления базовых данных.</span><span class="sxs-lookup"><span data-stu-id="093cb-116">I.e. The instance is marked as deleted withouth physically removing the underlying data.</span></span>

<span data-ttu-id="093cb-117">Затем настройте фильтры запросов в _OnModelCreating_, используя API ```HasQueryFilter```.</span><span class="sxs-lookup"><span data-stu-id="093cb-117">Next, configure the query filters in _OnModelCreating_ using the ```HasQueryFilter``` API.</span></span>

[!code-csharp[Main](../../../efcore-dev/samples/QueryFilters/Program.cs#Configuration)]

<span data-ttu-id="093cb-118">Выражения предиката, поступающие в вызовы _HasQueryFilter_, будут автоматически применяться ко всем запросам LINQ для этих типов.</span><span class="sxs-lookup"><span data-stu-id="093cb-118">The predicate expressions passed to the _HasQueryFilter_ calls will now automatically be applied to any LINQ queries for those types.</span></span>

> [!TIP]
> <span data-ttu-id="093cb-119">Обратите внимание на использование поля уровня экземпляра DbContext. ```_tenantId``` используется для установки текущего клиента.</span><span class="sxs-lookup"><span data-stu-id="093cb-119">Note the use of a DbContext instance level field: ```_tenantId``` used to set the current tenant.</span></span> <span data-ttu-id="093cb-120">Фильтры на уровне модели будут использовать значение из правильного экземпляра контекста.</span><span class="sxs-lookup"><span data-stu-id="093cb-120">Model-level filters will use the value from the correct context instance.</span></span> <span data-ttu-id="093cb-121">То есть, экземпляра, который выполняет этот запрос.</span><span class="sxs-lookup"><span data-stu-id="093cb-121">I.e. The instance that is executing the query.</span></span>

## <a name="disabling-filters"></a><span data-ttu-id="093cb-122">Отключение фильтров</span><span class="sxs-lookup"><span data-stu-id="093cb-122">Disabling Filters</span></span>

<span data-ttu-id="093cb-123">Фильтры можно отключить для отдельных запросов LINQ, используя оператор ```IgnoreQueryFilters()```.</span><span class="sxs-lookup"><span data-stu-id="093cb-123">Filters may be disabled for individual LINQ queries by using the ```IgnoreQueryFilters()``` operator.</span></span>

[!code-csharp[Main](../../../efcore-dev/samples/QueryFilters/Program.cs#IgnoreFilters)]

## <a name="limitations"></a><span data-ttu-id="093cb-124">Ограничения</span><span class="sxs-lookup"><span data-stu-id="093cb-124">Limitations</span></span>

<span data-ttu-id="093cb-125">Глобальные фильтры запросов имеют следующие ограничения:</span><span class="sxs-lookup"><span data-stu-id="093cb-125">Global query filters have the following limitations:</span></span>

* <span data-ttu-id="093cb-126">Фильтры не должны содержать ссылки на свойства навигации.</span><span class="sxs-lookup"><span data-stu-id="093cb-126">Filters cannot contain references to navigation properties.</span></span>
* <span data-ttu-id="093cb-127">Фильтры можно определить только для корневого типа сущности в иерархии наследования.</span><span class="sxs-lookup"><span data-stu-id="093cb-127">Filters can only be defined for the root Entity Type of an inheritance hierarchy.</span></span>
