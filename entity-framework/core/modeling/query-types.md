---
title: Типы запросов - EF Core
author: anpete
ms.author: anpete
ms.date: 2/26/2018
ms.assetid: 9F4450C5-1A3F-4BB6-AC19-9FAC64292AAD
ms.technology: entity-framework-core
uid: core/modeling/query-types
ms.openlocfilehash: 4e02f106e086d243b23a60c02838f32555be210e
ms.sourcegitcommit: 26f33758c47399ae933f22fec8e1d19fa7d2c0b7
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/19/2018
---
# <a name="query-types"></a><span data-ttu-id="2cfd8-102">Типы запросов</span><span class="sxs-lookup"><span data-stu-id="2cfd8-102">Query Types</span></span>
> [!NOTE]
> <span data-ttu-id="2cfd8-103">Этот компонент является новые возможности EF Core 2.1</span><span class="sxs-lookup"><span data-stu-id="2cfd8-103">This feature is new in EF Core 2.1</span></span>

<span data-ttu-id="2cfd8-104">Помимо типов сущностей, могут содержать EF базовой модели _типами запросов_, который может использоваться для выполнения запросов к базе данных с данными, который не сопоставляется с типами сущностей.</span><span class="sxs-lookup"><span data-stu-id="2cfd8-104">In addition to entity types, an EF Core model can contain _query types_, which can be used to carry out database queries against data that isn't mapped to entity types.</span></span>

<span data-ttu-id="2cfd8-105">Типы запросов имеют много общего с типами сущностей:</span><span class="sxs-lookup"><span data-stu-id="2cfd8-105">Query types have many similarities with entity types:</span></span>

- <span data-ttu-id="2cfd8-106">Их можно также добавить в модель либо в `OnModelCreating`, или через свойство «набор» на производный _DbContext_.</span><span class="sxs-lookup"><span data-stu-id="2cfd8-106">They can also be added to the model either in `OnModelCreating`, or via a "set" property on a derived _DbContext_.</span></span>
- <span data-ttu-id="2cfd8-107">Они поддерживают многие те же возможности сопоставления, как наследование, сопоставление свойств навигации (см. ниже ограничения) и в реляционных хранилищах, возможность настройки целевых объектов базы данных и столбцы через методы fluent API или заметок к данным.</span><span class="sxs-lookup"><span data-stu-id="2cfd8-107">They support many of the same mapping capabilities, like inheritance mapping, navigation properties (see limitations below) and, on relational stores, the ability to configure the target database objects and columns via fluent API methods or data annotations.</span></span>

<span data-ttu-id="2cfd8-108">Однако они отличаются от сущности типов в том, что они:</span><span class="sxs-lookup"><span data-stu-id="2cfd8-108">However they are different from entity types in that they:</span></span>

- <span data-ttu-id="2cfd8-109">Ключ определен не требуется.</span><span class="sxs-lookup"><span data-stu-id="2cfd8-109">Do not require a key to be defined.</span></span>
- <span data-ttu-id="2cfd8-110">Никогда не отслеживаются для изменения на _DbContext_ и поэтому не вставлены, обновляются или удаляются в базе данных.</span><span class="sxs-lookup"><span data-stu-id="2cfd8-110">Are never tracked for changes on the _DbContext_ and therefore are never inserted, updated or deleted on the database.</span></span>
- <span data-ttu-id="2cfd8-111">Никогда не обнаруживаются по соглашению.</span><span class="sxs-lookup"><span data-stu-id="2cfd8-111">Are never discovered by convention.</span></span>
- <span data-ttu-id="2cfd8-112">Только поддерживают подмножество возможности навигации сопоставление — в частности, никогда не может выступать в роли основной элемент связи.</span><span class="sxs-lookup"><span data-stu-id="2cfd8-112">Only support a subset of navigation mapping capabilities - Specifically, they may never act as the principal end of a relationship.</span></span>
- <span data-ttu-id="2cfd8-113">Посвящена _построитель ModelBuilder_ с помощью `Query` метод вместо `Entity` метод.</span><span class="sxs-lookup"><span data-stu-id="2cfd8-113">Are addressed on the _ModelBuilder_ using the `Query` method rather than the `Entity` method.</span></span>
- <span data-ttu-id="2cfd8-114">Сопоставленные на _DbContext_ по свойствам типа `DbQuery<T>` вместо `DbSet<T>`</span><span class="sxs-lookup"><span data-stu-id="2cfd8-114">Are mapped on the _DbContext_ through properties of type `DbQuery<T>` rather than `DbSet<T>`</span></span>
- <span data-ttu-id="2cfd8-115">Сопоставляются с объектами базы данных с помощью `ToView` метода, а не `ToTable`.</span><span class="sxs-lookup"><span data-stu-id="2cfd8-115">Are mapped to database objects using the `ToView` method, rather than `ToTable`.</span></span>
- <span data-ttu-id="2cfd8-116">Может быть сопоставлен _определение запроса_ — определение запроса — это дополнительный запрос, объявленные в модели, который выступает в источник данных для типа запроса.</span><span class="sxs-lookup"><span data-stu-id="2cfd8-116">May be mapped to a _defining query_ - A defining query is a secondary query declared in the model that acts a data source for a query type.</span></span>

<span data-ttu-id="2cfd8-117">Ниже приведены некоторые из сценариев использования основных типов запросов.</span><span class="sxs-lookup"><span data-stu-id="2cfd8-117">Some of the main usage scenarios for query types are:</span></span>

- <span data-ttu-id="2cfd8-118">Для нерегламентированных выступают в качестве возвращаемого типа для `FromSql()` запросов.</span><span class="sxs-lookup"><span data-stu-id="2cfd8-118">Serving as the return type for ad hoc `FromSql()` queries.</span></span>
- <span data-ttu-id="2cfd8-119">Сопоставление для представления базы данных.</span><span class="sxs-lookup"><span data-stu-id="2cfd8-119">Mapping to database views.</span></span>
- <span data-ttu-id="2cfd8-120">Сопоставление таблиц, у которых нет первичного ключа.</span><span class="sxs-lookup"><span data-stu-id="2cfd8-120">Mapping to tables that do not have a primary key defined.</span></span>
- <span data-ttu-id="2cfd8-121">Сопоставление запросов, определенных в модели.</span><span class="sxs-lookup"><span data-stu-id="2cfd8-121">Mapping to queries defined in the model.</span></span>

> [!TIP]
> <span data-ttu-id="2cfd8-122">Сопоставление типа запроса к объекту базы данных достигается с помощью `ToView` fluent API.</span><span class="sxs-lookup"><span data-stu-id="2cfd8-122">Mapping a query type to a database object is achieved using the `ToView` fluent API.</span></span> <span data-ttu-id="2cfd8-123">С точки зрения EF Core, является объект базы данных, указанный в этом методе _представление_, то есть, он рассматривается как исходный запрос только для чтения и не может быть целевым объектом обновления, insert или delete операций.</span><span class="sxs-lookup"><span data-stu-id="2cfd8-123">From the perspective of EF Core, the database object specified in this method is a _view_, meaning that it is treated as a read-only query source and cannot be the target of update, insert or delete operations.</span></span> <span data-ttu-id="2cfd8-124">Однако это не означает, что объект базы данных фактически требуется представление базы данных - в качестве альтернативы можно таблицу базы данных, которое будет интерпретироваться как доступный только для чтения.</span><span class="sxs-lookup"><span data-stu-id="2cfd8-124">However, this does not mean that the database object is actually required to be a database view - It can alternatively be a database table that will be treated as read-only.</span></span> <span data-ttu-id="2cfd8-125">И наоборот, для типов сущностей, EF Core предполагается, что объект базы данных, указанный в `ToTable` метод могут рассматриваться как _таблицы_, это означает, что можно использовать в качестве источника запроса, но также распространяется обновление, удаление и вставка операции.</span><span class="sxs-lookup"><span data-stu-id="2cfd8-125">Conversely, for entity types, EF Core assumes that a database object specified in the `ToTable` method can be treated as a _table_, meaning that it can be used as a query source but also targeted by update, delete and insert operations.</span></span> <span data-ttu-id="2cfd8-126">На самом деле, можно указать имя представления базы данных в `ToTable` и все данные должны корректно работать, пока представление настроен в качестве обновляемой базы данных.</span><span class="sxs-lookup"><span data-stu-id="2cfd8-126">In fact, you can specify the name of a database view in `ToTable` and everything should work fine as long as the view is configured to be updatable on the database.</span></span>

## <a name="example"></a><span data-ttu-id="2cfd8-127">Пример</span><span class="sxs-lookup"><span data-stu-id="2cfd8-127">Example</span></span>

<span data-ttu-id="2cfd8-128">В следующем примере показано, как используется тип запроса для запроса представления базы данных.</span><span class="sxs-lookup"><span data-stu-id="2cfd8-128">The following example shows how to use Query Type to query a database view.</span></span>

> [!TIP]
> <span data-ttu-id="2cfd8-129">Для этой статьи вы можете скачать [пример](https://github.com/aspnet/EntityFrameworkCore/tree/dev/samples/QueryTypes) из репозитория GitHub.</span><span class="sxs-lookup"><span data-stu-id="2cfd8-129">You can view this article's [sample](https://github.com/aspnet/EntityFrameworkCore/tree/dev/samples/QueryTypes) on GitHub.</span></span>

<span data-ttu-id="2cfd8-130">Во-первых определим простой модели блога и Post.</span><span class="sxs-lookup"><span data-stu-id="2cfd8-130">First, we define a simple Blog and Post model:</span></span>

[!code-csharp[Main](../../../efcore-dev/samples/QueryTypes/Program.cs#Entities)]

<span data-ttu-id="2cfd8-131">Затем определяется представление простой базы данных, мы будем запрашивать количество записей, связанных с каждого блога:</span><span class="sxs-lookup"><span data-stu-id="2cfd8-131">Next, we define a simple database view that will allow us to query the number of posts associated with each blog:</span></span>

[!code-csharp[Main](../../../efcore-dev/samples/QueryTypes/Program.cs#View)]

<span data-ttu-id="2cfd8-132">Затем определяется класс, содержащий результат из представления базы данных:</span><span class="sxs-lookup"><span data-stu-id="2cfd8-132">Next, we define a class to hold the result from the database view:</span></span>

[!code-csharp[Main](../../../efcore-dev/samples/QueryTypes/Program.cs#QueryType)]

<span data-ttu-id="2cfd8-133">Далее мы настраиваем тип запроса в _OnModelCreating_ с помощью `modelBuilder.Query<T>` API.</span><span class="sxs-lookup"><span data-stu-id="2cfd8-133">Next, we configure the query type in _OnModelCreating_ using the `modelBuilder.Query<T>` API.</span></span>
<span data-ttu-id="2cfd8-134">Мы используем стандартной конфигурации fluent API-интерфейсы для настройки сопоставления для типа запроса:</span><span class="sxs-lookup"><span data-stu-id="2cfd8-134">We use standard fluent configuration APIs to configure the mapping for the Query Type:</span></span>

[!code-csharp[Main](../../../efcore-dev/samples/QueryTypes/Program.cs#Configuration)]

<span data-ttu-id="2cfd8-135">Наконец можно запросить представления базы данных обычным образом:</span><span class="sxs-lookup"><span data-stu-id="2cfd8-135">Finally, we can query the database view in the standard way:</span></span>

[!code-csharp[Main](../../../efcore-dev/samples/QueryTypes/Program.cs#Query)]

> [!TIP]
> <span data-ttu-id="2cfd8-136">Обратите внимание, что мы определили свойство контекста запроса уровня (DbQuery) в качестве корневой для запросов этого типа.</span><span class="sxs-lookup"><span data-stu-id="2cfd8-136">Note we have also defined a context level query property (DbQuery) to act as a root for queries against this type.</span></span>
