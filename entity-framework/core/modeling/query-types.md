---
title: Типы запросов — EF Core
author: anpete
ms.author: anpete
ms.date: 2/26/2018
ms.assetid: 9F4450C5-1A3F-4BB6-AC19-9FAC64292AAD
ms.technology: entity-framework-core
uid: core/modeling/query-types
ms.openlocfilehash: 89f5be356654dc02e353441a83e34c90fc727593
ms.sourcegitcommit: fd50ac53b93a03825dcbb42ed2e7ca95ca858d5f
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/07/2018
ms.locfileid: "37900308"
---
# <a name="query-types"></a><span data-ttu-id="b9192-102">Типы запросов</span><span class="sxs-lookup"><span data-stu-id="b9192-102">Query Types</span></span>
> [!NOTE]
> <span data-ttu-id="b9192-103">Эта функция находится в EF Core 2.1</span><span class="sxs-lookup"><span data-stu-id="b9192-103">This feature is new in EF Core 2.1</span></span>

<span data-ttu-id="b9192-104">В дополнение к типам сущностей, могут содержать модель EF Core _типы запросов_, который может использоваться для выполнения запросов к базе данных для данных, которые не сопоставлены с типами сущностей.</span><span class="sxs-lookup"><span data-stu-id="b9192-104">In addition to entity types, an EF Core model can contain _query types_, which can be used to carry out database queries against data that isn't mapped to entity types.</span></span>

<span data-ttu-id="b9192-105">Типы запросов имеют много общего с типами сущностей:</span><span class="sxs-lookup"><span data-stu-id="b9192-105">Query types have many similarities with entity types:</span></span>

- <span data-ttu-id="b9192-106">Они также могут добавляться к модели либо в `OnModelCreating`, или с помощью свойства «набор» на производный _DbContext_.</span><span class="sxs-lookup"><span data-stu-id="b9192-106">They can also be added to the model either in `OnModelCreating`, or via a "set" property on a derived _DbContext_.</span></span>
- <span data-ttu-id="b9192-107">Они поддерживают многие из те же возможности сопоставления, как наследование, сопоставление свойства навигации (см. в разделе ограничений ниже) и в реляционных хранилищах, возможность настройки целевых объектов базы данных и столбцы с помощью текучего API методы или заметки к данным.</span><span class="sxs-lookup"><span data-stu-id="b9192-107">They support many of the same mapping capabilities, like inheritance mapping, navigation properties (see limitations below) and, on relational stores, the ability to configure the target database objects and columns via fluent API methods or data annotations.</span></span>

<span data-ttu-id="b9192-108">Тем не менее они отличаются от сущностей тем, что в них типы:</span><span class="sxs-lookup"><span data-stu-id="b9192-108">However they are different from entity types in that they:</span></span>

- <span data-ttu-id="b9192-109">Ключ должен быть определен не требуется.</span><span class="sxs-lookup"><span data-stu-id="b9192-109">Do not require a key to be defined.</span></span>
- <span data-ttu-id="b9192-110">Никогда не отслеживаются для изменения на _DbContext_ и поэтому никогда не вставке, удалении или обновлении в базе данных.</span><span class="sxs-lookup"><span data-stu-id="b9192-110">Are never tracked for changes on the _DbContext_ and therefore are never inserted, updated or deleted on the database.</span></span>
- <span data-ttu-id="b9192-111">Никогда не обнаруживаются по соглашению.</span><span class="sxs-lookup"><span data-stu-id="b9192-111">Are never discovered by convention.</span></span>
- <span data-ttu-id="b9192-112">Поддерживают только подмножество возможностей навигации сопоставления — в частности:</span><span class="sxs-lookup"><span data-stu-id="b9192-112">Only support a subset of navigation mapping capabilities - Specifically:</span></span>
  - <span data-ttu-id="b9192-113">Они никогда не может выступать в качестве основной конец связи.</span><span class="sxs-lookup"><span data-stu-id="b9192-113">They may never act as the principal end of a relationship.</span></span>
  - <span data-ttu-id="b9192-114">Могут содержать только свойства навигации ссылки на сущности.</span><span class="sxs-lookup"><span data-stu-id="b9192-114">They can only contain reference navigation properties pointing to entities.</span></span>
  - <span data-ttu-id="b9192-115">Сущности не может содержать свойства навигации к типам запросов.</span><span class="sxs-lookup"><span data-stu-id="b9192-115">Entities cannot contain navigation properties to query types.</span></span>
- <span data-ttu-id="b9192-116">Посвящена _ModelBuilder_ с помощью `Query` метод вместо `Entity` метод.</span><span class="sxs-lookup"><span data-stu-id="b9192-116">Are addressed on the _ModelBuilder_ using the `Query` method rather than the `Entity` method.</span></span>
- <span data-ttu-id="b9192-117">Сопоставляются на _DbContext_ через свойства типа `DbQuery<T>` вместо `DbSet<T>`</span><span class="sxs-lookup"><span data-stu-id="b9192-117">Are mapped on the _DbContext_ through properties of type `DbQuery<T>` rather than `DbSet<T>`</span></span>
- <span data-ttu-id="b9192-118">Сопоставляются с объектами базы данных с помощью `ToView` метод, а не `ToTable`.</span><span class="sxs-lookup"><span data-stu-id="b9192-118">Are mapped to database objects using the `ToView` method, rather than `ToTable`.</span></span>
- <span data-ttu-id="b9192-119">Могут быть сопоставлены с _запроса_ — определение запроса — это дополнительный запрос, объявленные в модели, используемый источник данных для типа запроса.</span><span class="sxs-lookup"><span data-stu-id="b9192-119">May be mapped to a _defining query_ - A defining query is a secondary query declared in the model that acts a data source for a query type.</span></span>

<span data-ttu-id="b9192-120">Ниже приведены некоторые из основных варианта применения типы запросов.</span><span class="sxs-lookup"><span data-stu-id="b9192-120">Some of the main usage scenarios for query types are:</span></span>

- <span data-ttu-id="b9192-121">Выступая в качестве возвращаемого типа для нерегламентированных `FromSql()` запросов.</span><span class="sxs-lookup"><span data-stu-id="b9192-121">Serving as the return type for ad hoc `FromSql()` queries.</span></span>
- <span data-ttu-id="b9192-122">Сопоставление с представлениями базы данных.</span><span class="sxs-lookup"><span data-stu-id="b9192-122">Mapping to database views.</span></span>
- <span data-ttu-id="b9192-123">Сопоставление таблиц, у которых не определен первичный ключ.</span><span class="sxs-lookup"><span data-stu-id="b9192-123">Mapping to tables that do not have a primary key defined.</span></span>
- <span data-ttu-id="b9192-124">Сопоставление с запросами, определенными в модели.</span><span class="sxs-lookup"><span data-stu-id="b9192-124">Mapping to queries defined in the model.</span></span>

> [!TIP]
> <span data-ttu-id="b9192-125">Сопоставление типа запроса к объекту базы данных достигается с помощью `ToView` текучего API.</span><span class="sxs-lookup"><span data-stu-id="b9192-125">Mapping a query type to a database object is achieved using the `ToView` fluent API.</span></span> <span data-ttu-id="b9192-126">С точки зрения EF Core, является объект базы данных, указанный в этом методе _представление_, это означает, что она обрабатывается как источник запроса только для чтения и не может быть целевым объектом update, insert или delete операций.</span><span class="sxs-lookup"><span data-stu-id="b9192-126">From the perspective of EF Core, the database object specified in this method is a _view_, meaning that it is treated as a read-only query source and cannot be the target of update, insert or delete operations.</span></span> <span data-ttu-id="b9192-127">Тем не менее это не означает, что объект базы данных требуется фактически быть представления базы данных - также может быть таблицей базы данных, которое будет интерпретироваться как доступный только для чтения.</span><span class="sxs-lookup"><span data-stu-id="b9192-127">However, this does not mean that the database object is actually required to be a database view - It can alternatively be a database table that will be treated as read-only.</span></span> <span data-ttu-id="b9192-128">И наоборот, для типов сущностей, EF Core предполагает, что объект базы данных, указанные в `ToTable` метод может рассматриваться как _таблицы_, это означает, что он может использоваться как источник запроса, но также предназначено обновление, удаление и вставка операции.</span><span class="sxs-lookup"><span data-stu-id="b9192-128">Conversely, for entity types, EF Core assumes that a database object specified in the `ToTable` method can be treated as a _table_, meaning that it can be used as a query source but also targeted by update, delete and insert operations.</span></span> <span data-ttu-id="b9192-129">На самом деле, можно указать имя представления базы данных в `ToTable` и все должно работать нормально, пока представление настроено как обновляемые в базе данных.</span><span class="sxs-lookup"><span data-stu-id="b9192-129">In fact, you can specify the name of a database view in `ToTable` and everything should work fine as long as the view is configured to be updatable on the database.</span></span>

## <a name="example"></a><span data-ttu-id="b9192-130">Пример</span><span class="sxs-lookup"><span data-stu-id="b9192-130">Example</span></span>

<span data-ttu-id="b9192-131">В следующем примере показано, как использовать тип запроса для запроса представления базы данных.</span><span class="sxs-lookup"><span data-stu-id="b9192-131">The following example shows how to use Query Type to query a database view.</span></span>

> [!TIP]
> <span data-ttu-id="b9192-132">Для этой статьи вы можете скачать [пример](https://github.com/aspnet/EntityFrameworkCore/tree/dev/samples/QueryTypes) из репозитория GitHub.</span><span class="sxs-lookup"><span data-stu-id="b9192-132">You can view this article's [sample](https://github.com/aspnet/EntityFrameworkCore/tree/dev/samples/QueryTypes) on GitHub.</span></span>

<span data-ttu-id="b9192-133">Во-первых определим простую модель блога и Post.</span><span class="sxs-lookup"><span data-stu-id="b9192-133">First, we define a simple Blog and Post model:</span></span>

[!code-csharp[Main](../../../efcore-repo/samples/QueryTypes/Program.cs#Entities)]

<span data-ttu-id="b9192-134">Далее мы определим представление простой базы данных, которые позволят нам запросить число сообщений, связанных с каждого блога:</span><span class="sxs-lookup"><span data-stu-id="b9192-134">Next, we define a simple database view that will allow us to query the number of posts associated with each blog:</span></span>

[!code-csharp[Main](../../../efcore-repo/samples/QueryTypes/Program.cs#View)]

<span data-ttu-id="b9192-135">Далее мы определим класс для хранения результата из представления базы данных:</span><span class="sxs-lookup"><span data-stu-id="b9192-135">Next, we define a class to hold the result from the database view:</span></span>

[!code-csharp[Main](../../../efcore-repo/samples/QueryTypes/Program.cs#QueryType)]

<span data-ttu-id="b9192-136">Далее мы настраиваем тип запроса в _OnModelCreating_ с помощью `modelBuilder.Query<T>` API.</span><span class="sxs-lookup"><span data-stu-id="b9192-136">Next, we configure the query type in _OnModelCreating_ using the `modelBuilder.Query<T>` API.</span></span>
<span data-ttu-id="b9192-137">Мы используем стандартную конфигурацию fluent API-интерфейсы для настройки сопоставления для типа запроса:</span><span class="sxs-lookup"><span data-stu-id="b9192-137">We use standard fluent configuration APIs to configure the mapping for the Query Type:</span></span>

[!code-csharp[Main](../../../efcore-repo/samples/QueryTypes/Program.cs#Configuration)]

<span data-ttu-id="b9192-138">Наконец можно запросить представления базы данных обычным образом:</span><span class="sxs-lookup"><span data-stu-id="b9192-138">Finally, we can query the database view in the standard way:</span></span>

[!code-csharp[Main](../../../efcore-repo/samples/QueryTypes/Program.cs#Query)]

> [!TIP]
> <span data-ttu-id="b9192-139">Обратите внимание, что мы определили свойство контекста запроса уровня (DbQuery) в качестве корневого для запросов этого типа.</span><span class="sxs-lookup"><span data-stu-id="b9192-139">Note we have also defined a context level query property (DbQuery) to act as a root for queries against this type.</span></span>
