---
title: Типы сущностей без ключей - EF Core
description: Как настроить типы сущностей без бесключевого ключа с помощью Core Entity Framework
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 9/13/2019
uid: core/modeling/keyless-entity-types
ms.openlocfilehash: 496e1e8983ba2d5e15dbee02607ea3f2c861503e
ms.sourcegitcommit: 144edccf9b29a7ffad119c235ac9808ec1a46193
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/16/2020
ms.locfileid: "81434218"
---
# <a name="keyless-entity-types"></a><span data-ttu-id="bd9bb-103">Типы сущностей без ключей</span><span class="sxs-lookup"><span data-stu-id="bd9bb-103">Keyless Entity Types</span></span>

> [!NOTE]
> <span data-ttu-id="bd9bb-104">Эта функция была добавлена в EF Core 2.1 под названием типов запросов.</span><span class="sxs-lookup"><span data-stu-id="bd9bb-104">This feature was added in EF Core 2.1 under the name of query types.</span></span> <span data-ttu-id="bd9bb-105">В EF Core 3.0 концепция была переименована в типы бесключевого сущности.</span><span class="sxs-lookup"><span data-stu-id="bd9bb-105">In EF Core 3.0 the concept was renamed to keyless entity types.</span></span>

<span data-ttu-id="bd9bb-106">В дополнение к обычным типам сущностей модель EF Core может содержать _типы сущностей без бесключевого,_ которые могут использоваться для выполнения запросов баз данных в отношении данных, не содержащих ключевых значений.</span><span class="sxs-lookup"><span data-stu-id="bd9bb-106">In addition to regular entity types, an EF Core model can contain _keyless entity types_, which can be used to carry out database queries against data that doesn't contain key values.</span></span>

## <a name="defining-keyless-entity-types"></a><span data-ttu-id="bd9bb-107">Определение типов сущностей Keyless</span><span class="sxs-lookup"><span data-stu-id="bd9bb-107">Defining Keyless entity types</span></span>

<span data-ttu-id="bd9bb-108">Типы сущности Keyless могут быть определены с помощью аннотации данных или API Fluent:</span><span class="sxs-lookup"><span data-stu-id="bd9bb-108">Keyless entity types can be defined using either the Data Annotation or the Fluent API:</span></span>

### <a name="data-annotations"></a>[<span data-ttu-id="bd9bb-109">Аннотации данных</span><span class="sxs-lookup"><span data-stu-id="bd9bb-109">Data Annotations</span></span>](#tab/data-annotations)

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Keyless.cs?Name=Keyless&highlight=1)]

### <a name="fluent-api"></a>[<span data-ttu-id="bd9bb-110">Текучий API</span><span class="sxs-lookup"><span data-stu-id="bd9bb-110">Fluent API</span></span>](#tab/fluent-api)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Keyless.cs?Name=Keyless&highlight=4)]

***

## <a name="keyless-entity-types-characteristics"></a><span data-ttu-id="bd9bb-111">Характеристики типов бесключевых сущностей</span><span class="sxs-lookup"><span data-stu-id="bd9bb-111">Keyless entity types characteristics</span></span>

<span data-ttu-id="bd9bb-112">Типы сущности Keyless поддерживают многие из тех же возможностей отображения, что и обычные типы сущности, такие как отображение наследования и свойства навигации.</span><span class="sxs-lookup"><span data-stu-id="bd9bb-112">Keyless entity types support many of the same mapping capabilities as regular entity types, like inheritance mapping and navigation properties.</span></span> <span data-ttu-id="bd9bb-113">В реляционных хранилищах они могут настраивать объекты и столбцы целевой базы данных с помощью свободно используемых методов API или аннотаций данных.</span><span class="sxs-lookup"><span data-stu-id="bd9bb-113">On relational stores, they can configure the target database objects and columns via fluent API methods or data annotations.</span></span>

<span data-ttu-id="bd9bb-114">Тем не менее, они отличаются от обычных типов сущности в том, что они:</span><span class="sxs-lookup"><span data-stu-id="bd9bb-114">However, they are different from regular entity types in that they:</span></span>

- <span data-ttu-id="bd9bb-115">Не удается определить ключ.</span><span class="sxs-lookup"><span data-stu-id="bd9bb-115">Cannot have a key defined.</span></span>
- <span data-ttu-id="bd9bb-116">Никогда не отслеживаются для изменений в _DbContext_ и, следовательно, никогда не вставляются, обновляются или удаляются в базу данных.</span><span class="sxs-lookup"><span data-stu-id="bd9bb-116">Are never tracked for changes in the _DbContext_ and therefore are never inserted, updated or deleted on the database.</span></span>
- <span data-ttu-id="bd9bb-117">Никогда не обнаруживаются конвенцией.</span><span class="sxs-lookup"><span data-stu-id="bd9bb-117">Are never discovered by convention.</span></span>
- <span data-ttu-id="bd9bb-118">Только поддержка подмножество навигационных возможностей отображения, в частности:</span><span class="sxs-lookup"><span data-stu-id="bd9bb-118">Only support a subset of navigation mapping capabilities, specifically:</span></span>
  - <span data-ttu-id="bd9bb-119">Они никогда не могут выступать в качестве основного конца отношений.</span><span class="sxs-lookup"><span data-stu-id="bd9bb-119">They may never act as the principal end of a relationship.</span></span>
  - <span data-ttu-id="bd9bb-120">Они могут не иметь навигации для принадлежащих организаций</span><span class="sxs-lookup"><span data-stu-id="bd9bb-120">They may not have navigations to owned entities</span></span>
  - <span data-ttu-id="bd9bb-121">Они могут содержать только ориентировостые навигационные свойства, указывающие на обычные объекты.</span><span class="sxs-lookup"><span data-stu-id="bd9bb-121">They can only contain reference navigation properties pointing to regular entities.</span></span>
  - <span data-ttu-id="bd9bb-122">Сущности не могут содержать навигационные свойства к типам бесключевого объекта.</span><span class="sxs-lookup"><span data-stu-id="bd9bb-122">Entities cannot contain navigation properties to keyless entity types.</span></span>
- <span data-ttu-id="bd9bb-123">Необходимо настроить с аннотацией `[Keyless]` данных или вызовом метода. `.HasNoKey()`</span><span class="sxs-lookup"><span data-stu-id="bd9bb-123">Need to be configured with a `[Keyless]` data annotation or a `.HasNoKey()` method call.</span></span>
- <span data-ttu-id="bd9bb-124">Может быть отображены к _определяющему запросу._</span><span class="sxs-lookup"><span data-stu-id="bd9bb-124">May be mapped to a _defining query_.</span></span> <span data-ttu-id="bd9bb-125">Определяющим запросом является запрос, объявленный в модели, который действует как источник данных для типа бесключевого объекта.</span><span class="sxs-lookup"><span data-stu-id="bd9bb-125">A defining query is a query declared in the model that acts as a data source for a keyless entity type.</span></span>

## <a name="usage-scenarios"></a><span data-ttu-id="bd9bb-126">Сценарии использования</span><span class="sxs-lookup"><span data-stu-id="bd9bb-126">Usage scenarios</span></span>

<span data-ttu-id="bd9bb-127">Некоторые из основных сценариев использования для типов бесключевого сущности:</span><span class="sxs-lookup"><span data-stu-id="bd9bb-127">Some of the main usage scenarios for keyless entity types are:</span></span>

- <span data-ttu-id="bd9bb-128">Обслуживание в качестве типа возврата для [необработанных запросов S'L.](xref:core/querying/raw-sql)</span><span class="sxs-lookup"><span data-stu-id="bd9bb-128">Serving as the return type for [raw SQL queries](xref:core/querying/raw-sql).</span></span>
- <span data-ttu-id="bd9bb-129">Отображение представлений баз данных, не содержащих основной ключ.</span><span class="sxs-lookup"><span data-stu-id="bd9bb-129">Mapping to database views that do not contain a primary key.</span></span>
- <span data-ttu-id="bd9bb-130">Отображение к таблицам, которые не имеют основного ключа определены.</span><span class="sxs-lookup"><span data-stu-id="bd9bb-130">Mapping to tables that do not have a primary key defined.</span></span>
- <span data-ttu-id="bd9bb-131">Отображение запросов, определенных в модели.</span><span class="sxs-lookup"><span data-stu-id="bd9bb-131">Mapping to queries defined in the model.</span></span>

## <a name="mapping-to-database-objects"></a><span data-ttu-id="bd9bb-132">Отображение к объектам базы данных</span><span class="sxs-lookup"><span data-stu-id="bd9bb-132">Mapping to database objects</span></span>

<span data-ttu-id="bd9bb-133">Отображение типа бесключевой сущности `ToTable` для `ToView` объекта базы данных достигается с помощью или свободного API.</span><span class="sxs-lookup"><span data-stu-id="bd9bb-133">Mapping a keyless entity type to a database object is achieved using the `ToTable` or `ToView` fluent API.</span></span> <span data-ttu-id="bd9bb-134">С точки зрения EF Core объект базы данных, указанный в этом методе, представляет собой _представление,_ означающее, что он рассматривается как источник запроса только для чтения и не может быть целью обновления, вставки или удаления операций.</span><span class="sxs-lookup"><span data-stu-id="bd9bb-134">From the perspective of EF Core, the database object specified in this method is a _view_, meaning that it is treated as a read-only query source and cannot be the target of update, insert or delete operations.</span></span> <span data-ttu-id="bd9bb-135">Однако это не означает, что объект базы данных на самом деле должен быть представлением базы данных.</span><span class="sxs-lookup"><span data-stu-id="bd9bb-135">However, this does not mean that the database object is actually required to be a database view.</span></span> <span data-ttu-id="bd9bb-136">В качестве альтернативы это может быть таблица баз данных, которая будет рассматриваться как только для чтения.</span><span class="sxs-lookup"><span data-stu-id="bd9bb-136">It can alternatively be a database table that will be treated as read-only.</span></span> <span data-ttu-id="bd9bb-137">И наоборот, для обычных типов сущностей EF Core `ToTable` предполагает, что объект базы данных, указанный в методе, может рассматриваться как _таблица,_ что означает, что он может быть использован в качестве источника запроса, но также ориентирован на обновление, удаление и вставки операций.</span><span class="sxs-lookup"><span data-stu-id="bd9bb-137">Conversely, for regular entity types, EF Core assumes that a database object specified in the `ToTable` method can be treated as a _table_, meaning that it can be used as a query source but also targeted by update, delete and insert operations.</span></span> <span data-ttu-id="bd9bb-138">На самом деле, можно указать имя `ToTable` представления базы данных, и все должно работать нормально, пока представление настроено для updatable в базе данных.</span><span class="sxs-lookup"><span data-stu-id="bd9bb-138">In fact, you can specify the name of a database view in `ToTable` and everything should work fine as long as the view is configured to be updatable on the database.</span></span>

> [!NOTE]
> <span data-ttu-id="bd9bb-139">`ToView`предполагает, что объект уже существует в базе данных и не будет создан миграциями.</span><span class="sxs-lookup"><span data-stu-id="bd9bb-139">`ToView` assumes that the object already exists in the database and it won't be created by migrations.</span></span>

## <a name="example"></a><span data-ttu-id="bd9bb-140">Пример</span><span class="sxs-lookup"><span data-stu-id="bd9bb-140">Example</span></span>

<span data-ttu-id="bd9bb-141">В следующем примере показано, как использовать типы сущности без ключа для запроса представления базы данных.</span><span class="sxs-lookup"><span data-stu-id="bd9bb-141">The following example shows how to use keyless entity types to query a database view.</span></span>

> [!TIP]
> <span data-ttu-id="bd9bb-142">Вы можете просмотреть [пример](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/KeylessEntityTypes) этой статьи на GitHub.</span><span class="sxs-lookup"><span data-stu-id="bd9bb-142">You can view this article's [sample](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/KeylessEntityTypes) on GitHub.</span></span>

<span data-ttu-id="bd9bb-143">Во-первых, мы определяем простую модель блога и поста:</span><span class="sxs-lookup"><span data-stu-id="bd9bb-143">First, we define a simple Blog and Post model:</span></span>

[!code-csharp[Main](../../../samples/core/KeylessEntityTypes/Program.cs#Entities)]

<span data-ttu-id="bd9bb-144">Далее мы определяем простое представление базы данных, которое позволит нам задать вопрос о количестве сообщений, связанных с каждым блогом:</span><span class="sxs-lookup"><span data-stu-id="bd9bb-144">Next, we define a simple database view that will allow us to query the number of posts associated with each blog:</span></span>

[!code-csharp[Main](../../../samples/core/KeylessEntityTypes/Program.cs#View)]

<span data-ttu-id="bd9bb-145">Далее мы определяем класс для удержания результата из представления базы данных:</span><span class="sxs-lookup"><span data-stu-id="bd9bb-145">Next, we define a class to hold the result from the database view:</span></span>

[!code-csharp[Main](../../../samples/core/KeylessEntityTypes/Program.cs#KeylessEntityType)]

<span data-ttu-id="bd9bb-146">Далее мы настраиваем тип бесключевой сущности `HasNoKey` в _OnModelCreating_ с помощью API.</span><span class="sxs-lookup"><span data-stu-id="bd9bb-146">Next, we configure the keyless entity type in _OnModelCreating_ using the `HasNoKey` API.</span></span>
<span data-ttu-id="bd9bb-147">Мы используем свободно настроенный API конфигурации для настройки отображения для типа бесключевого объекта:</span><span class="sxs-lookup"><span data-stu-id="bd9bb-147">We use fluent configuration API to configure the mapping for the keyless entity type:</span></span>

[!code-csharp[Main](../../../samples/core/KeylessEntityTypes/Program.cs#Configuration)]

<span data-ttu-id="bd9bb-148">Далее мы настраиваем, `DbContext` `DbSet<T>`чтобы включить:</span><span class="sxs-lookup"><span data-stu-id="bd9bb-148">Next, we configure the `DbContext` to include the `DbSet<T>`:</span></span>

[!code-csharp[Main](../../../samples/core/KeylessEntityTypes/Program.cs#DbSet)]

<span data-ttu-id="bd9bb-149">Наконец, мы можем задавить представление базы данных стандартным способом:</span><span class="sxs-lookup"><span data-stu-id="bd9bb-149">Finally, we can query the database view in the standard way:</span></span>

[!code-csharp[Main](../../../samples/core/KeylessEntityTypes/Program.cs#Query)]

> [!TIP]
> <span data-ttu-id="bd9bb-150">Обратите внимание, что мы также определили свойство запроса контекстного уровня (DbSet) для того, чтобы служить корнем для запросов против этого типа.</span><span class="sxs-lookup"><span data-stu-id="bd9bb-150">Note we have also defined a context level query property (DbSet) to act as a root for queries against this type.</span></span>
