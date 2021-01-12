---
title: Компараторы значений — EF Core
description: Использование компараторов значений для управления EF Core сравнения значений свойств
author: ajcvickers
ms.date: 03/20/2020
uid: core/modeling/value-comparers
ms.openlocfilehash: 618341315de05f6efae8f43384809ed72226e18b
ms.sourcegitcommit: 032a1767d7a6e42052a005f660b80372c6521e7e
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/12/2021
ms.locfileid: "98128515"
---
# <a name="value-comparers"></a><span data-ttu-id="36d46-103">Компараторы значений</span><span class="sxs-lookup"><span data-stu-id="36d46-103">Value Comparers</span></span>

> [!NOTE]
> <span data-ttu-id="36d46-104">Эта возможность появилась в EF Core 3.0.</span><span class="sxs-lookup"><span data-stu-id="36d46-104">This feature was introduced in EF Core 3.0.</span></span>

> [!TIP]
> <span data-ttu-id="36d46-105">Код в этом документе можно найти на сайте GitHub в качестве [готового к запуску примера](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Modeling/ValueConversions/).</span><span class="sxs-lookup"><span data-stu-id="36d46-105">The code in this document can be found on GitHub as a [runnable sample](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Modeling/ValueConversions/).</span></span>

## <a name="background"></a><span data-ttu-id="36d46-106">Историческая справка</span><span class="sxs-lookup"><span data-stu-id="36d46-106">Background</span></span>

<span data-ttu-id="36d46-107">Отслеживание изменений означает, что EF Core автоматически определяет, какие изменения были выполнены приложением на загруженном экземпляре сущности, чтобы эти изменения можно было сохранить обратно в базу данных при <xref:Microsoft.EntityFrameworkCore.DbContext.SaveChanges%2A> вызове метода.</span><span class="sxs-lookup"><span data-stu-id="36d46-107">Change tracking means that EF Core automatically determines what changes were performed by the application on a loaded entity instance, so that those changes can be saved back to the database when <xref:Microsoft.EntityFrameworkCore.DbContext.SaveChanges%2A> is called.</span></span> <span data-ttu-id="36d46-108">EF Core обычно выполняет это путем *создания моментального снимка* экземпляра при его загрузке из базы данных и *сравнения* этого моментального снимка с экземпляром приложения.</span><span class="sxs-lookup"><span data-stu-id="36d46-108">EF Core usually performs this by taking a *snapshot* of the instance when it's loaded from the database, and *comparing* that snapshot to the instance handed out to the application.</span></span>

<span data-ttu-id="36d46-109">EF Core поставляется со встроенной логикой для значит и сравнивает большинство стандартных типов, используемых в базах данных, поэтому пользователям обычно не нужно беспокоиться об этом разделе.</span><span class="sxs-lookup"><span data-stu-id="36d46-109">EF Core comes with built-in logic for snapshotting and comparing most standard types used in databases, so users don't usually need to worry about this topic.</span></span> <span data-ttu-id="36d46-110">Однако если свойство сопоставлено через [преобразователь значений](xref:core/modeling/value-conversions), EF Core необходимо выполнить сравнение с произвольными типами пользователей, что может быть сложным.</span><span class="sxs-lookup"><span data-stu-id="36d46-110">However, when a property is mapped through a [value converter](xref:core/modeling/value-conversions), EF Core needs to perform comparison on arbitrary user types, which may be complex.</span></span> <span data-ttu-id="36d46-111">По умолчанию EF Core использует сравнение на равенство по умолчанию, определенное типами (например, `Equals` метод); для значит типы значений копируются для создания моментального снимка, а для ссылочных типов не выполняется копирование, и в качестве моментального снимка используется тот же экземпляр.</span><span class="sxs-lookup"><span data-stu-id="36d46-111">By default, EF Core uses the default equality comparison defined by types (e.g. the `Equals` method); for snapshotting, value types are copied to produce the snapshot, while for reference types no copying occurs, and the same instance is used as the snapshot.</span></span>

<span data-ttu-id="36d46-112">В случаях, когда встроенное поведение сравнения не подходит, пользователи могут предоставить *компаратор значений*, который содержит логику для значит, сравнив и вычисляя хэш-код.</span><span class="sxs-lookup"><span data-stu-id="36d46-112">In cases where the built-in comparison behavior isn't appropriate, users may provide a *value comparer*, which contains logic for snapshotting, comparing and calculating a hash code.</span></span> <span data-ttu-id="36d46-113">Например, следующая строка устанавливает преобразование значения для `List<int>` свойства в значение, преобразованное в строку JSON в базе данных, а также определяет соответствующий компаратор значений.</span><span class="sxs-lookup"><span data-stu-id="36d46-113">For example, the following sets up value conversion for `List<int>` property to be value converted to a JSON string in the database, and defines an appropriate value comparer as well:</span></span>

[!code-csharp[ListProperty](../../../samples/core/Modeling/ValueConversions/MappingListProperty.cs?name=ConfigureListProperty)]

<span data-ttu-id="36d46-114">Дополнительные сведения см. в разделе [Изменяемые классы](#mutable-classes) ниже.</span><span class="sxs-lookup"><span data-stu-id="36d46-114">See [mutable classes](#mutable-classes) below for further details.</span></span>

<span data-ttu-id="36d46-115">Обратите внимание, что компараторы значений также используются при определении того, совпадают ли два значения ключа при разрешении связей. Это объясняется ниже.</span><span class="sxs-lookup"><span data-stu-id="36d46-115">Note that value comparers are also used when determining whether two key values are the same when resolving relationships; this is explained below.</span></span>

## <a name="shallow-vs-deep-comparison"></a><span data-ttu-id="36d46-116">Неполное и глубокое сравнение</span><span class="sxs-lookup"><span data-stu-id="36d46-116">Shallow vs. deep comparison</span></span>

<span data-ttu-id="36d46-117">Для небольших неизменяемых типов значений, таких как `int` , логика по умолчанию EF Core работает правильно: значение копируется как есть при снапшоттед и сравнивается со встроенным сравнением равенства типа.</span><span class="sxs-lookup"><span data-stu-id="36d46-117">For small, immutable value types such as `int`, EF Core's default logic works well: the value is copied as-is when snapshotted, and compared with the type's built-in equality comparison.</span></span> <span data-ttu-id="36d46-118">При реализации собственного компаратора значений важно учитывать, подходит ли логика глубокого или неглубокого сравнения (и значит).</span><span class="sxs-lookup"><span data-stu-id="36d46-118">When implementing your own value comparer, it's important to consider whether deep or shallow comparison (and snapshotting) logic is appropriate.</span></span>

<span data-ttu-id="36d46-119">Рассмотрите возможность использовать массивы байтов, которые могут иметь произвольное большое число.</span><span class="sxs-lookup"><span data-stu-id="36d46-119">Consider byte arrays, which can be arbitrarily large.</span></span> <span data-ttu-id="36d46-120">Это можно сравнить:</span><span class="sxs-lookup"><span data-stu-id="36d46-120">These could be compared:</span></span>

* <span data-ttu-id="36d46-121">По ссылке, что различие обнаруживается только при использовании нового массива байтов</span><span class="sxs-lookup"><span data-stu-id="36d46-121">By reference, such that a difference is only detected if a new byte array is used</span></span>
* <span data-ttu-id="36d46-122">По глубокому сравнению, что обнаружено изменение байтов в массиве</span><span class="sxs-lookup"><span data-stu-id="36d46-122">By deep comparison, such that mutation of the bytes in the array is detected</span></span>

<span data-ttu-id="36d46-123">По умолчанию EF Core использует первый из этих подходов для неключевых массивов байтов.</span><span class="sxs-lookup"><span data-stu-id="36d46-123">By default, EF Core uses the first of these approaches for non-key byte arrays.</span></span> <span data-ttu-id="36d46-124">То есть сравниваются только ссылки, а изменение обнаруживается только в том случае, если существующий массив байтов заменяется новым.</span><span class="sxs-lookup"><span data-stu-id="36d46-124">That is, only references are compared and a change is detected only when an existing byte array is replaced with a new one.</span></span> <span data-ttu-id="36d46-125">Это практичное решение, которое позволяет избежать копирования целых массивов и сравнения их байтов и байтов при выполнении <xref:Microsoft.EntityFrameworkCore.DbContext.SaveChanges%2A> , и распространенный сценарий замены, скажем, одного изображения с другим, обрабатывается в удобном направлении.</span><span class="sxs-lookup"><span data-stu-id="36d46-125">This is a pragmatic decision that avoids copying entire arrays and comparing them byte-to-byte when executing <xref:Microsoft.EntityFrameworkCore.DbContext.SaveChanges%2A>, and the common scenario of replacing, say, one image with another is handled in a performant way.</span></span>

<span data-ttu-id="36d46-126">С другой стороны, равенство ссылок не будет работать, если массивы байтов используются для представления двоичных ключей, так как очень маловероятно, чтобы свойство FK было задано тем _же экземпляром_ , что и свойство PK, для которого требуется сравнение.</span><span class="sxs-lookup"><span data-stu-id="36d46-126">On the other hand, reference equality would not work when byte arrays are used to represent binary keys, since it's very unlikely that an FK property is set to the _same instance_ as a PK property to which it needs to be compared.</span></span> <span data-ttu-id="36d46-127">Таким образом, EF Core использует глубокие сравнения для массивов байтов, действующих в качестве ключей. Это вряд ли пострадает от снижения производительности, так как обычно двоичные ключи являются короткими.</span><span class="sxs-lookup"><span data-stu-id="36d46-127">Therefore, EF Core uses deep comparisons for byte arrays acting as keys; this is unlikely to have a big performance hit since binary keys are usually short.</span></span>

<span data-ttu-id="36d46-128">Обратите внимание, что выбранная логика сравнения и значита должна соответствовать друг другу: чтобы обеспечить правильное функционирование, для глубокого сравнения требуется глубокий значит.</span><span class="sxs-lookup"><span data-stu-id="36d46-128">Note that the chosen comparison and snapshotting logic must correspond to each other: deep comparison requires deep snapshotting to function correctly.</span></span>

## <a name="simple-immutable-classes"></a><span data-ttu-id="36d46-129">Простые неизменяемые классы</span><span class="sxs-lookup"><span data-stu-id="36d46-129">Simple immutable classes</span></span>

<span data-ttu-id="36d46-130">Рассмотрим свойство, которое использует преобразователь значений для отображения простого, неизменяемого класса.</span><span class="sxs-lookup"><span data-stu-id="36d46-130">Consider a property that uses a value converter to map a simple, immutable class.</span></span>

[!code-csharp[SimpleImmutableClass](../../../samples/core/Modeling/ValueConversions/MappingImmutableClassProperty.cs?name=SimpleImmutableClass)]

[!code-csharp[ConfigureImmutableClassProperty](../../../samples/core/Modeling/ValueConversions/MappingImmutableClassProperty.cs?name=ConfigureImmutableClassProperty)]

<span data-ttu-id="36d46-131">Свойства этого типа не требуют специальных сравнений или моментальных снимков, поскольку:</span><span class="sxs-lookup"><span data-stu-id="36d46-131">Properties of this type do not need special comparisons or snapshots because:</span></span>

* <span data-ttu-id="36d46-132">Равенство переопределяется, так что различные экземпляры будут сравниваться правильно</span><span class="sxs-lookup"><span data-stu-id="36d46-132">Equality is overridden so that different instances will compare correctly</span></span>
* <span data-ttu-id="36d46-133">Тип является неизменяемым, поэтому невозможно изменить значение моментального снимка</span><span class="sxs-lookup"><span data-stu-id="36d46-133">The type is immutable, so there is no chance of mutating a snapshot value</span></span>

<span data-ttu-id="36d46-134">Поэтому в этом случае поведение EF Core по умолчанию отлично от.</span><span class="sxs-lookup"><span data-stu-id="36d46-134">So in this case the default behavior of EF Core is fine as it is.</span></span>

## <a name="simple-immutable-structs"></a><span data-ttu-id="36d46-135">Простые неизменяемые структуры</span><span class="sxs-lookup"><span data-stu-id="36d46-135">Simple immutable structs</span></span>

<span data-ttu-id="36d46-136">Сопоставление простых структур также является простым и не требует специальных компараторов или значит.</span><span class="sxs-lookup"><span data-stu-id="36d46-136">The mapping for simple structs is also simple and requires no special comparers or snapshotting.</span></span>

[!code-csharp[SimpleImmutableStruct](../../../samples/core/Modeling/ValueConversions/MappingImmutableStructProperty.cs?name=SimpleImmutableStruct)]

[!code-csharp[ConfigureImmutableStructProperty](../../../samples/core/Modeling/ValueConversions/MappingImmutableStructProperty.cs?name=ConfigureImmutableStructProperty)]

<span data-ttu-id="36d46-137">EF Core имеет встроенную поддержку создания скомпилированных, почленном сравнений свойств структуры.</span><span class="sxs-lookup"><span data-stu-id="36d46-137">EF Core has built-in support for generating compiled, memberwise comparisons of struct properties.</span></span>
<span data-ttu-id="36d46-138">Это означает, что структуры не требуют переопределения равенства для EF Core, но вы по-прежнему можете выбрать это по [другим причинам](/dotnet/csharp/programming-guide/statements-expressions-operators/how-to-define-value-equality-for-a-type).</span><span class="sxs-lookup"><span data-stu-id="36d46-138">This means structs don't need to have equality overridden for EF Core, but you may still choose to do this for [other reasons](/dotnet/csharp/programming-guide/statements-expressions-operators/how-to-define-value-equality-for-a-type).</span></span>
<span data-ttu-id="36d46-139">Кроме того, Специальные значит не нужны, так как структуры являются неизменяемыми и всегда копируются почленном в любом случае.</span><span class="sxs-lookup"><span data-stu-id="36d46-139">Also, special snapshotting is not needed since structs are immutable and are always copied memberwise anyway.</span></span>
<span data-ttu-id="36d46-140">(Это также справедливо для изменяемых структур, но [в общем случае изменяющиеся структуры следует избегать](/dotnet/csharp/write-safe-efficient-code).)</span><span class="sxs-lookup"><span data-stu-id="36d46-140">(This is also true for mutable structs, but [mutable structs should in general be avoided](/dotnet/csharp/write-safe-efficient-code).)</span></span>

## <a name="mutable-classes"></a><span data-ttu-id="36d46-141">Изменяемые классы</span><span class="sxs-lookup"><span data-stu-id="36d46-141">Mutable classes</span></span>

<span data-ttu-id="36d46-142">Рекомендуется использовать неизменяемые типы (классы или структуры) с преобразователями значений, если это возможно.</span><span class="sxs-lookup"><span data-stu-id="36d46-142">It is recommended that you use immutable types (classes or structs) with value converters when possible.</span></span>
<span data-ttu-id="36d46-143">Обычно это более эффективно и имеет семантику очистки, чем использование изменяемого типа.</span><span class="sxs-lookup"><span data-stu-id="36d46-143">This is usually more efficient and has cleaner semantics than using a mutable type.</span></span>

<span data-ttu-id="36d46-144">Тем не менее, часто используются свойства типов, которые приложение не может изменять.</span><span class="sxs-lookup"><span data-stu-id="36d46-144">However, that being said, it is common to use properties of types that the application cannot change.</span></span>
<span data-ttu-id="36d46-145">Например, сопоставление свойства, содержащего список чисел:</span><span class="sxs-lookup"><span data-stu-id="36d46-145">For example, mapping a property containing a list of numbers:</span></span>

[!code-csharp[ListProperty](../../../samples/core/Modeling/ValueConversions/MappingListProperty.cs?name=ListProperty)]

<span data-ttu-id="36d46-146">Класс <xref:System.Collections.Generic.List%601>:</span><span class="sxs-lookup"><span data-stu-id="36d46-146">The <xref:System.Collections.Generic.List%601> class:</span></span>

* <span data-ttu-id="36d46-147">Имеет равенство ссылок; два списка, содержащие одни и те же значения, рассматриваются как разные.</span><span class="sxs-lookup"><span data-stu-id="36d46-147">Has reference equality; two lists containing the same values are treated as different.</span></span>
* <span data-ttu-id="36d46-148">Является изменяемым; значения в списке можно добавлять и удалять.</span><span class="sxs-lookup"><span data-stu-id="36d46-148">Is mutable; values in the list can be added and removed.</span></span>

<span data-ttu-id="36d46-149">Стандартное преобразование значения для свойства списка может привести к преобразованию списка в JSON и из него:</span><span class="sxs-lookup"><span data-stu-id="36d46-149">A typical value conversion on a list property might convert the list to and from JSON:</span></span>

### <a name="ef-core-50"></a>[<span data-ttu-id="36d46-150">EF Core 5.0</span><span class="sxs-lookup"><span data-stu-id="36d46-150">EF Core 5.0</span></span>](#tab/ef5)

[!code-csharp[ListProperty](../../../samples/core/Modeling/ValueConversions/MappingListProperty.cs?name=ConfigureListProperty&highlight=7-10)]

### <a name="older-versions"></a>[<span data-ttu-id="36d46-151">Более старые версии</span><span class="sxs-lookup"><span data-stu-id="36d46-151">Older versions</span></span>](#tab/older-versions)

[!code-csharp[ListProperty](../../../samples/core/Modeling/ValueConversions/MappingListPropertyOld.cs?name=ConfigureListProperty&highlight=8-11,17)]

***

<span data-ttu-id="36d46-152"><xref:Microsoft.EntityFrameworkCore.ChangeTracking.ValueComparer%601>Конструктор принимает три выражения:</span><span class="sxs-lookup"><span data-stu-id="36d46-152">The <xref:Microsoft.EntityFrameworkCore.ChangeTracking.ValueComparer%601> constructor accepts three expressions:</span></span>

* <span data-ttu-id="36d46-153">Выражение для проверки равенства</span><span class="sxs-lookup"><span data-stu-id="36d46-153">An expression for checking equality</span></span>
* <span data-ttu-id="36d46-154">Выражение для создания хэш-кода</span><span class="sxs-lookup"><span data-stu-id="36d46-154">An expression for generating a hash code</span></span>
* <span data-ttu-id="36d46-155">Выражение для создания снимка значения</span><span class="sxs-lookup"><span data-stu-id="36d46-155">An expression to snapshot a value</span></span>

<span data-ttu-id="36d46-156">В этом случае сравнение выполняется путем проверки того, совпадают ли последовательности чисел.</span><span class="sxs-lookup"><span data-stu-id="36d46-156">In this case the comparison is done by checking if the sequences of numbers are the same.</span></span>

<span data-ttu-id="36d46-157">Аналогичным образом хэш-код строится из этой же последовательности.</span><span class="sxs-lookup"><span data-stu-id="36d46-157">Likewise, the hash code is built from this same sequence.</span></span>
<span data-ttu-id="36d46-158">(Обратите внимание, что это хэш-код для изменяемых значений и, следовательно, может [вызвать проблемы](https://ericlippert.com/2011/02/28/guidelines-and-rules-for-gethashcode/).</span><span class="sxs-lookup"><span data-stu-id="36d46-158">(Note that this is a hash code over mutable values and hence can [cause problems](https://ericlippert.com/2011/02/28/guidelines-and-rules-for-gethashcode/).</span></span>
<span data-ttu-id="36d46-159">Быть неизменяемым вместо этого, если это возможно.)</span><span class="sxs-lookup"><span data-stu-id="36d46-159">Be immutable instead if you can.)</span></span>

<span data-ttu-id="36d46-160">Моментальный снимок создается путем клонирования списка с помощью `ToList` .</span><span class="sxs-lookup"><span data-stu-id="36d46-160">The snapshot is created by cloning the list with `ToList`.</span></span>
<span data-ttu-id="36d46-161">Опять же, это необходимо, только если списки будут изменяться.</span><span class="sxs-lookup"><span data-stu-id="36d46-161">Again, this is only needed if the lists are going to be mutated.</span></span>
<span data-ttu-id="36d46-162">Быть неизменяемым вместо этого, если это возможно.</span><span class="sxs-lookup"><span data-stu-id="36d46-162">Be immutable instead if you can.</span></span>

> [!NOTE]
> <span data-ttu-id="36d46-163">Преобразователи значений и компараторы создаются с помощью выражений, а не простых делегатов.</span><span class="sxs-lookup"><span data-stu-id="36d46-163">Value converters and comparers are constructed using expressions rather than simple delegates.</span></span>
> <span data-ttu-id="36d46-164">Это связано с тем, что EF Core вставляет эти выражения в гораздо более сложное дерево выражения, которое затем компилируется в делегат сущности-фигуры.</span><span class="sxs-lookup"><span data-stu-id="36d46-164">This is because EF Core inserts these expressions into a much more complex expression tree that is then compiled into an entity shaper delegate.</span></span>
> <span data-ttu-id="36d46-165">По сути, это похоже на встраивание компилятора.</span><span class="sxs-lookup"><span data-stu-id="36d46-165">Conceptually, this is similar to compiler inlining.</span></span>
> <span data-ttu-id="36d46-166">Например, простое преобразование может быть просто скомпилированным в приведении, а не вызовом другого метода для преобразования.</span><span class="sxs-lookup"><span data-stu-id="36d46-166">For example, a simple conversion may just be a compiled in cast, rather than a call to another method to do the conversion.</span></span>

## <a name="key-comparers"></a><span data-ttu-id="36d46-167">Ключевые компараторы</span><span class="sxs-lookup"><span data-stu-id="36d46-167">Key comparers</span></span>

<span data-ttu-id="36d46-168">В разделе Background описывается, почему для сравнения ключей может потребоваться специальная семантика.</span><span class="sxs-lookup"><span data-stu-id="36d46-168">The background section covers why key comparisons may require special semantics.</span></span>
<span data-ttu-id="36d46-169">Обязательно создайте средство сравнения, которое подходит для ключей при его задании в свойстве первичного, основного или внешнего ключа.</span><span class="sxs-lookup"><span data-stu-id="36d46-169">Make sure to create a comparer that is appropriate for keys when setting it on a primary, principal, or foreign key property.</span></span>

<span data-ttu-id="36d46-170">Используйте <xref:Microsoft.EntityFrameworkCore.MutablePropertyExtensions.SetKeyValueComparer%2A> в редких случаях, когда для одного свойства требуется другая семантика.</span><span class="sxs-lookup"><span data-stu-id="36d46-170">Use <xref:Microsoft.EntityFrameworkCore.MutablePropertyExtensions.SetKeyValueComparer%2A> in the rare cases where different semantics is required on the same property.</span></span>

> [!NOTE]
> <span data-ttu-id="36d46-171"><xref:Microsoft.EntityFrameworkCore.MutablePropertyExtensions.SetStructuralValueComparer%2A> является устаревшим в EF Core 5,0.</span><span class="sxs-lookup"><span data-stu-id="36d46-171"><xref:Microsoft.EntityFrameworkCore.MutablePropertyExtensions.SetStructuralValueComparer%2A> has been obsoleted in EF Core 5.0.</span></span> <span data-ttu-id="36d46-172">Взамен рекомендуется использовать <xref:Microsoft.EntityFrameworkCore.MutablePropertyExtensions.SetKeyValueComparer%2A>.</span><span class="sxs-lookup"><span data-stu-id="36d46-172">Use <xref:Microsoft.EntityFrameworkCore.MutablePropertyExtensions.SetKeyValueComparer%2A> instead.</span></span>

## <a name="overriding-the-default-comparer"></a><span data-ttu-id="36d46-173">Переопределение компаратора по умолчанию</span><span class="sxs-lookup"><span data-stu-id="36d46-173">Overriding the default comparer</span></span>

<span data-ttu-id="36d46-174">Иногда сравнение по умолчанию, используемое EF Core, может быть неприменимо.</span><span class="sxs-lookup"><span data-stu-id="36d46-174">Sometimes the default comparison used by EF Core may not be appropriate.</span></span>
<span data-ttu-id="36d46-175">Например, изменение массивов байтов по умолчанию не определяется в EF Core.</span><span class="sxs-lookup"><span data-stu-id="36d46-175">For example, mutation of byte arrays is not, by default, detected in EF Core.</span></span>
<span data-ttu-id="36d46-176">Это можно переопределить, задав другое средство сравнения для свойства:</span><span class="sxs-lookup"><span data-stu-id="36d46-176">This can be overridden by setting a different comparer on the property:</span></span>

[!code-csharp[OverrideComparer](../../../samples/core/Modeling/ValueConversions/OverridingByteArrayComparisons.cs?name=OverrideComparer)]

<span data-ttu-id="36d46-177">EF Core теперь будет сравнивать последовательности байтов и, следовательно, определит изменения массива байтов.</span><span class="sxs-lookup"><span data-stu-id="36d46-177">EF Core will now compare byte sequences and will therefore detect byte array mutations.</span></span>
