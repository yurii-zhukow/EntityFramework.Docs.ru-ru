---
title: Типы сущностей без ключей — EF Core
description: Настройка типов сущностей без ключей с помощью Entity Framework Core
author: AndriySvyryd
ms.author: ansvyryd
ms.date: 9/13/2019
uid: core/modeling/keyless-entity-types
ms.openlocfilehash: 00e8f17c88fd51e39df3c1e45c648c203bbbe324
ms.sourcegitcommit: 387cbd8109c0fc5ce6bdc85d0dec1aed72ad4c33
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/23/2020
ms.locfileid: "82103130"
---
# <a name="keyless-entity-types"></a><span data-ttu-id="c58a2-103">Типы сущностей без ключей</span><span class="sxs-lookup"><span data-stu-id="c58a2-103">Keyless Entity Types</span></span>

> [!NOTE]
> <span data-ttu-id="c58a2-104">Эта функция была добавлена в EF Core 2,1 под именем типов запросов.</span><span class="sxs-lookup"><span data-stu-id="c58a2-104">This feature was added in EF Core 2.1 under the name of query types.</span></span> <span data-ttu-id="c58a2-105">В EF Core 3,0 концепция была переименована в типы сущностей без ключей.</span><span class="sxs-lookup"><span data-stu-id="c58a2-105">In EF Core 3.0 the concept was renamed to keyless entity types.</span></span> <span data-ttu-id="c58a2-106">Аннотация `[Keyless]` данных стала доступной в ефкоре 5,0.</span><span class="sxs-lookup"><span data-stu-id="c58a2-106">The `[Keyless]` Data Annotation became available in EFCore 5.0.</span></span>

<span data-ttu-id="c58a2-107">В дополнение к обычным типам сущностей модель EF Core может содержать незашифрованные _типы сущностей_, которые можно использовать для выполнения запросов к базе данных, не содержащей значений ключей.</span><span class="sxs-lookup"><span data-stu-id="c58a2-107">In addition to regular entity types, an EF Core model can contain _keyless entity types_, which can be used to carry out database queries against data that doesn't contain key values.</span></span>

## <a name="defining-keyless-entity-types"></a><span data-ttu-id="c58a2-108">Определение типов сущностей без ключей</span><span class="sxs-lookup"><span data-stu-id="c58a2-108">Defining Keyless entity types</span></span>

<span data-ttu-id="c58a2-109">Типы сущностей без ключей можно определить с помощью аннотации данных или API-интерфейса Fluent:</span><span class="sxs-lookup"><span data-stu-id="c58a2-109">Keyless entity types can be defined using either the Data Annotation or the Fluent API:</span></span>

### <a name="data-annotations"></a>[<span data-ttu-id="c58a2-110">Заметки к данным</span><span class="sxs-lookup"><span data-stu-id="c58a2-110">Data Annotations</span></span>](#tab/data-annotations)

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Keyless.cs?Name=Keyless&highlight=1)]

### <a name="fluent-api"></a>[<span data-ttu-id="c58a2-111">Текучий API</span><span class="sxs-lookup"><span data-stu-id="c58a2-111">Fluent API</span></span>](#tab/fluent-api)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Keyless.cs?Name=Keyless&highlight=4)]

***

## <a name="keyless-entity-types-characteristics"></a><span data-ttu-id="c58a2-112">Характеристики типов сущностей без ключей</span><span class="sxs-lookup"><span data-stu-id="c58a2-112">Keyless entity types characteristics</span></span>

<span data-ttu-id="c58a2-113">Типы сущностей без ключей поддерживают многие из тех же возможностей сопоставления, что и обычные типы сущностей, такие как сопоставление наследования и свойства навигации.</span><span class="sxs-lookup"><span data-stu-id="c58a2-113">Keyless entity types support many of the same mapping capabilities as regular entity types, like inheritance mapping and navigation properties.</span></span> <span data-ttu-id="c58a2-114">В реляционных хранилищах они могут настраивать объекты и столбцы целевой базы данных с помощью методов API Fluent или заметок к данным.</span><span class="sxs-lookup"><span data-stu-id="c58a2-114">On relational stores, they can configure the target database objects and columns via fluent API methods or data annotations.</span></span>

<span data-ttu-id="c58a2-115">Однако они отличаются от обычных типов сущностей тем, что они:</span><span class="sxs-lookup"><span data-stu-id="c58a2-115">However, they are different from regular entity types in that they:</span></span>

- <span data-ttu-id="c58a2-116">Ключ не может быть определен.</span><span class="sxs-lookup"><span data-stu-id="c58a2-116">Cannot have a key defined.</span></span>
- <span data-ttu-id="c58a2-117">Никогда не отправляются для изменений в _DbContext_ , поэтому они никогда не вставляются, не обновляются или не удаляются в базе данных.</span><span class="sxs-lookup"><span data-stu-id="c58a2-117">Are never tracked for changes in the _DbContext_ and therefore are never inserted, updated or deleted on the database.</span></span>
- <span data-ttu-id="c58a2-118">Никогда не обнаруживаются по соглашению.</span><span class="sxs-lookup"><span data-stu-id="c58a2-118">Are never discovered by convention.</span></span>
- <span data-ttu-id="c58a2-119">Поддерживаются только подмножество возможностей сопоставления навигации, в частности:</span><span class="sxs-lookup"><span data-stu-id="c58a2-119">Only support a subset of navigation mapping capabilities, specifically:</span></span>
  - <span data-ttu-id="c58a2-120">Они не могут действовать в качестве основного элемента связи.</span><span class="sxs-lookup"><span data-stu-id="c58a2-120">They may never act as the principal end of a relationship.</span></span>
  - <span data-ttu-id="c58a2-121">Они не могут иметь переходы к собственным сущностям</span><span class="sxs-lookup"><span data-stu-id="c58a2-121">They may not have navigations to owned entities</span></span>
  - <span data-ttu-id="c58a2-122">Они могут содержать только свойства навигации по ссылке, указывающие на обычные сущности.</span><span class="sxs-lookup"><span data-stu-id="c58a2-122">They can only contain reference navigation properties pointing to regular entities.</span></span>
  - <span data-ttu-id="c58a2-123">Сущности не могут содержать свойства навигации для неключейых типов сущностей.</span><span class="sxs-lookup"><span data-stu-id="c58a2-123">Entities cannot contain navigation properties to keyless entity types.</span></span>
- <span data-ttu-id="c58a2-124">Необходимо настроить с заметкой `[Keyless]` данных или вызовом `.HasNoKey()` метода.</span><span class="sxs-lookup"><span data-stu-id="c58a2-124">Need to be configured with a `[Keyless]` data annotation or a `.HasNoKey()` method call.</span></span>
- <span data-ttu-id="c58a2-125">Может быть сопоставлен с _определяющим запросом_.</span><span class="sxs-lookup"><span data-stu-id="c58a2-125">May be mapped to a _defining query_.</span></span> <span data-ttu-id="c58a2-126">Определяющий запрос — это запрос, объявленный в модели, который выступает в качестве источника данных для типа сущности без ключей.</span><span class="sxs-lookup"><span data-stu-id="c58a2-126">A defining query is a query declared in the model that acts as a data source for a keyless entity type.</span></span>

## <a name="usage-scenarios"></a><span data-ttu-id="c58a2-127">Сценарии использования</span><span class="sxs-lookup"><span data-stu-id="c58a2-127">Usage scenarios</span></span>

<span data-ttu-id="c58a2-128">Ниже приведены некоторые основные сценарии использования для типов сущностей без ключей:</span><span class="sxs-lookup"><span data-stu-id="c58a2-128">Some of the main usage scenarios for keyless entity types are:</span></span>

- <span data-ttu-id="c58a2-129">Служит типом возвращаемого значения для [необработанных запросов SQL](xref:core/querying/raw-sql).</span><span class="sxs-lookup"><span data-stu-id="c58a2-129">Serving as the return type for [raw SQL queries](xref:core/querying/raw-sql).</span></span>
- <span data-ttu-id="c58a2-130">Сопоставление с представлениями базы данных, которые не содержат первичный ключ.</span><span class="sxs-lookup"><span data-stu-id="c58a2-130">Mapping to database views that do not contain a primary key.</span></span>
- <span data-ttu-id="c58a2-131">Сопоставление с таблицами, для которых не определен первичный ключ.</span><span class="sxs-lookup"><span data-stu-id="c58a2-131">Mapping to tables that do not have a primary key defined.</span></span>
- <span data-ttu-id="c58a2-132">Сопоставление с запросами, определенными в модели.</span><span class="sxs-lookup"><span data-stu-id="c58a2-132">Mapping to queries defined in the model.</span></span>

## <a name="mapping-to-database-objects"></a><span data-ttu-id="c58a2-133">Сопоставление с объектами базы данных</span><span class="sxs-lookup"><span data-stu-id="c58a2-133">Mapping to database objects</span></span>

<span data-ttu-id="c58a2-134">Сопоставление типа сущности без ключей с объектом базы данных достигается с помощью `ToTable` API `ToView` -интерфейса или Fluent.</span><span class="sxs-lookup"><span data-stu-id="c58a2-134">Mapping a keyless entity type to a database object is achieved using the `ToTable` or `ToView` fluent API.</span></span> <span data-ttu-id="c58a2-135">С точки зрения EF Core, объект базы данных, указанный в этом методе, является _представлением_, то есть он обрабатывается как источник запроса только для чтения и не может быть целевым объектом операций обновления, вставки или удаления.</span><span class="sxs-lookup"><span data-stu-id="c58a2-135">From the perspective of EF Core, the database object specified in this method is a _view_, meaning that it is treated as a read-only query source and cannot be the target of update, insert or delete operations.</span></span> <span data-ttu-id="c58a2-136">Однако это не означает, что объект базы данных действительно должен быть представлением базы данных.</span><span class="sxs-lookup"><span data-stu-id="c58a2-136">However, this does not mean that the database object is actually required to be a database view.</span></span> <span data-ttu-id="c58a2-137">В качестве альтернативы можно использовать таблицу базы данных, которая будет рассматриваться как доступная только для чтения.</span><span class="sxs-lookup"><span data-stu-id="c58a2-137">It can alternatively be a database table that will be treated as read-only.</span></span> <span data-ttu-id="c58a2-138">И наоборот, для обычных типов сущностей EF Core предполагается, что объект базы данных, указанный `ToTable` в методе, может рассматриваться как _Таблица_, что означает, что он может использоваться в качестве источника запроса, но также предназначен для операций обновления, удаления и вставки.</span><span class="sxs-lookup"><span data-stu-id="c58a2-138">Conversely, for regular entity types, EF Core assumes that a database object specified in the `ToTable` method can be treated as a _table_, meaning that it can be used as a query source but also targeted by update, delete and insert operations.</span></span> <span data-ttu-id="c58a2-139">На самом деле можно указать имя представления базы данных в `ToTable` , и все должно работать правильно, пока представление настроено для обновления в базе данных.</span><span class="sxs-lookup"><span data-stu-id="c58a2-139">In fact, you can specify the name of a database view in `ToTable` and everything should work fine as long as the view is configured to be updatable on the database.</span></span>

> [!NOTE]
> <span data-ttu-id="c58a2-140">`ToView`Предполагается, что объект уже существует в базе данных и не создается при миграции.</span><span class="sxs-lookup"><span data-stu-id="c58a2-140">`ToView` assumes that the object already exists in the database and it won't be created by migrations.</span></span>

## <a name="example"></a><span data-ttu-id="c58a2-141">Пример</span><span class="sxs-lookup"><span data-stu-id="c58a2-141">Example</span></span>

<span data-ttu-id="c58a2-142">В следующем примере показано, как использовать типы сущностей без ключей для запроса представления базы данных.</span><span class="sxs-lookup"><span data-stu-id="c58a2-142">The following example shows how to use keyless entity types to query a database view.</span></span>

> [!TIP]
> <span data-ttu-id="c58a2-143">Вы можете просмотреть [Пример](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/KeylessEntityTypes) этой статьи на сайте GitHub.</span><span class="sxs-lookup"><span data-stu-id="c58a2-143">You can view this article's [sample](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/KeylessEntityTypes) on GitHub.</span></span>

<span data-ttu-id="c58a2-144">Во-первых, мы определим простой блог и модель Post:</span><span class="sxs-lookup"><span data-stu-id="c58a2-144">First, we define a simple Blog and Post model:</span></span>

[!code-csharp[Main](../../../samples/core/KeylessEntityTypes/Program.cs#Entities)]

<span data-ttu-id="c58a2-145">Далее мы определим простое представление базы данных, которое позволит запросить число записей, связанных с каждым блогом:</span><span class="sxs-lookup"><span data-stu-id="c58a2-145">Next, we define a simple database view that will allow us to query the number of posts associated with each blog:</span></span>

[!code-csharp[Main](../../../samples/core/KeylessEntityTypes/Program.cs#View)]

<span data-ttu-id="c58a2-146">Далее мы определим класс для хранения результатов из представления базы данных:</span><span class="sxs-lookup"><span data-stu-id="c58a2-146">Next, we define a class to hold the result from the database view:</span></span>

[!code-csharp[Main](../../../samples/core/KeylessEntityTypes/Program.cs#KeylessEntityType)]

<span data-ttu-id="c58a2-147">Далее мы настроим тип сущности без ключей в _OnModelCreating_ с помощью `HasNoKey` API.</span><span class="sxs-lookup"><span data-stu-id="c58a2-147">Next, we configure the keyless entity type in _OnModelCreating_ using the `HasNoKey` API.</span></span>
<span data-ttu-id="c58a2-148">Мы используем API-интерфейс конфигурации Fluent для настройки сопоставления для типа сущности без ключей:</span><span class="sxs-lookup"><span data-stu-id="c58a2-148">We use fluent configuration API to configure the mapping for the keyless entity type:</span></span>

[!code-csharp[Main](../../../samples/core/KeylessEntityTypes/Program.cs#Configuration)]

<span data-ttu-id="c58a2-149">Далее мы настроим `DbContext` для включения `DbSet<T>`:</span><span class="sxs-lookup"><span data-stu-id="c58a2-149">Next, we configure the `DbContext` to include the `DbSet<T>`:</span></span>

[!code-csharp[Main](../../../samples/core/KeylessEntityTypes/Program.cs#DbSet)]

<span data-ttu-id="c58a2-150">Наконец, можно запрашивать представление базы данных стандартным образом:</span><span class="sxs-lookup"><span data-stu-id="c58a2-150">Finally, we can query the database view in the standard way:</span></span>

[!code-csharp[Main](../../../samples/core/KeylessEntityTypes/Program.cs#Query)]

> [!TIP]
> <span data-ttu-id="c58a2-151">Обратите внимание, что мы также определили свойство запроса уровня контекста (DbSet), которое будет использоваться в качестве корневого для запросов к этому типу.</span><span class="sxs-lookup"><span data-stu-id="c58a2-151">Note we have also defined a context level query property (DbSet) to act as a root for queries against this type.</span></span>
