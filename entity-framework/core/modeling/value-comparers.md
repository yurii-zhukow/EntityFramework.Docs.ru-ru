---
title: Компараторы значений — EF Core
description: Использование компараторов значений для управления EF Core сравнения значений свойств
author: ajcvickers
ms.date: 03/20/2020
uid: core/modeling/value-comparers
ms.openlocfilehash: fa5352129977d858d54d4aede746b320c91b0ad3
ms.sourcegitcommit: 949faaba02e07e44359e77d7935f540af5c32093
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/03/2020
ms.locfileid: "87526788"
---
# <a name="value-comparers"></a><span data-ttu-id="cfefd-103">Компараторы значений</span><span class="sxs-lookup"><span data-stu-id="cfefd-103">Value Comparers</span></span>

> [!NOTE]  
> <span data-ttu-id="cfefd-104">Эта функция впервые реализована в EF Core 3,0.</span><span class="sxs-lookup"><span data-stu-id="cfefd-104">This feature is new in EF Core 3.0.</span></span>

> [!TIP]  
> <span data-ttu-id="cfefd-105">Код в этом документе можно найти на сайте GitHub в качестве [готового к запуску примера](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Modeling/ValueConversions/).</span><span class="sxs-lookup"><span data-stu-id="cfefd-105">The code in this document can be found on GitHub as a [runnable sample](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Modeling/ValueConversions/).</span></span>

## <a name="background"></a><span data-ttu-id="cfefd-106">Историческая справка</span><span class="sxs-lookup"><span data-stu-id="cfefd-106">Background</span></span>

<span data-ttu-id="cfefd-107">EF Core необходимо сравнить значения свойств, если:</span><span class="sxs-lookup"><span data-stu-id="cfefd-107">EF Core needs to compare property values when:</span></span>

* <span data-ttu-id="cfefd-108">Определение, было ли изменено свойство в процессе [обнаружения изменений для обновлений](xref:core/saving/basic)</span><span class="sxs-lookup"><span data-stu-id="cfefd-108">Determining whether a property has been changed as part of [detecting changes for updates](xref:core/saving/basic)</span></span>
* <span data-ttu-id="cfefd-109">Определение того, совпадают ли два значения ключа при разрешении связей</span><span class="sxs-lookup"><span data-stu-id="cfefd-109">Determining whether two key values are the same when resolving relationships</span></span> 

<span data-ttu-id="cfefd-110">Он обрабатывается автоматически для распространенных типов примитивов, таких как int, bool, DateTime и т. д.</span><span class="sxs-lookup"><span data-stu-id="cfefd-110">This is handled automatically for common primitive types such as int, bool, DateTime, etc.</span></span>

<span data-ttu-id="cfefd-111">Для более сложных типов необходимо выбрать способ сравнения.</span><span class="sxs-lookup"><span data-stu-id="cfefd-111">For more complex types, choices need to be made as to how to do the comparison.</span></span>
<span data-ttu-id="cfefd-112">Например, можно сравнить массив байтов:</span><span class="sxs-lookup"><span data-stu-id="cfefd-112">For example, a byte array could be compared:</span></span>

* <span data-ttu-id="cfefd-113">По ссылке, что различие обнаруживается только при использовании нового массива байтов</span><span class="sxs-lookup"><span data-stu-id="cfefd-113">By reference, such that a difference is only detected if a new byte array is used</span></span>
* <span data-ttu-id="cfefd-114">По глубокому сравнению, что обнаружено изменение байтов в массиве</span><span class="sxs-lookup"><span data-stu-id="cfefd-114">By deep comparison, such that mutation of the bytes in the array is detected</span></span>

<span data-ttu-id="cfefd-115">По умолчанию EF Core использует первый из этих подходов для неключевых массивов байтов.</span><span class="sxs-lookup"><span data-stu-id="cfefd-115">By default, EF Core uses the first of these approaches for non-key byte arrays.</span></span>
<span data-ttu-id="cfefd-116">То есть сравниваются только ссылки, а изменение обнаруживается только в том случае, если существующий массив байтов заменяется новым.</span><span class="sxs-lookup"><span data-stu-id="cfefd-116">That is, only references are compared and a change is detected only when an existing byte array is replaced with a new one.</span></span>
<span data-ttu-id="cfefd-117">Это практичное решение, которое позволяет избежать глубокого сравнения большого числа больших массивов байтов при выполнении команды SaveChanges.</span><span class="sxs-lookup"><span data-stu-id="cfefd-117">This is a pragmatic decision that avoids deep comparison of many large byte arrays when executing SaveChanges.</span></span>
<span data-ttu-id="cfefd-118">Но распространенный сценарий замены, скажем, изображения с другим изображением, обрабатывается по-своему.</span><span class="sxs-lookup"><span data-stu-id="cfefd-118">But the common scenario of replacing, say, an image with a different image is handled in a performant way.</span></span>

<span data-ttu-id="cfefd-119">С другой стороны, равенство ссылок не будет работать, если массивы байтов используются для представления двоичных ключей.</span><span class="sxs-lookup"><span data-stu-id="cfefd-119">On the other hand, reference equality would not work when byte arrays are used to represent binary keys.</span></span>
<span data-ttu-id="cfefd-120">Очень маловероятно, чтобы свойство FK было равно тому _же экземпляру_ , что и свойство PK, для которого требуется сравнение.</span><span class="sxs-lookup"><span data-stu-id="cfefd-120">It's very unlikely that an FK property is set to the _same instance_ as a PK property to which it needs to be compared.</span></span>
<span data-ttu-id="cfefd-121">Таким образом, EF Core использует глубокие сравнения для массивов байтов, действующих в качестве ключей.</span><span class="sxs-lookup"><span data-stu-id="cfefd-121">Therefore, EF Core uses deep comparisons for byte arrays acting as keys.</span></span>
<span data-ttu-id="cfefd-122">Это вряд ли пострадает от снижения производительности, так как обычно двоичные ключи являются короткими.</span><span class="sxs-lookup"><span data-stu-id="cfefd-122">This is unlikely to have a big performance hit since binary keys are usually short.</span></span>

### <a name="snapshots"></a><span data-ttu-id="cfefd-123">Моментальные снимки</span><span class="sxs-lookup"><span data-stu-id="cfefd-123">Snapshots</span></span>

<span data-ttu-id="cfefd-124">Углубленное сравнение изменяемых типов означает, что EF Core требуется возможность создания глубокого моментального снимка значения свойства.</span><span class="sxs-lookup"><span data-stu-id="cfefd-124">Deep comparisons on mutable types means that EF Core needs the ability to create a deep "snapshot" of the property value.</span></span>
<span data-ttu-id="cfefd-125">Просто копирование ссылки приведет к изменению текущего значения и моментального снимка, так как они являются одним и тем _же объектом_.</span><span class="sxs-lookup"><span data-stu-id="cfefd-125">Just copying the reference instead would result in mutating both the current value and the snapshot, since they are _the same object_.</span></span>
<span data-ttu-id="cfefd-126">Таким образом, если для изменяемых типов используются глубокие сравнения, также требуется глубокий значит.</span><span class="sxs-lookup"><span data-stu-id="cfefd-126">Therefore, when deep comparisons are used on mutable types, deep snapshotting is also required.</span></span>

## <a name="properties-with-value-converters"></a><span data-ttu-id="cfefd-127">Свойства с преобразователями значений</span><span class="sxs-lookup"><span data-stu-id="cfefd-127">Properties with value converters</span></span>

<span data-ttu-id="cfefd-128">В приведенном выше случае EF Core имеет встроенную поддержку сопоставления для массивов байтов, поэтому может автоматически выбирать соответствующие значения по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="cfefd-128">In the case above, EF Core has native mapping support for byte arrays and so can automatically choose appropriate defaults.</span></span>
<span data-ttu-id="cfefd-129">Однако если свойство сопоставлено через [преобразователь значений](xref:core/modeling/value-conversions), то EF Core не всегда может определить соответствующее сравнение для использования.</span><span class="sxs-lookup"><span data-stu-id="cfefd-129">However, if the property is mapped through a [value converter](xref:core/modeling/value-conversions), then EF Core can't always determine the appropriate comparison to use.</span></span>
<span data-ttu-id="cfefd-130">Вместо этого EF Core всегда использует сравнение на равенство по умолчанию, определенное типом свойства.</span><span class="sxs-lookup"><span data-stu-id="cfefd-130">Instead, EF Core always uses the default equality comparison defined by the type of the property.</span></span>
<span data-ttu-id="cfefd-131">Это часто верно, но при сопоставлении более сложных типов может потребоваться переопределение.</span><span class="sxs-lookup"><span data-stu-id="cfefd-131">This is often correct, but may need to be overridden when mapping more complex types.</span></span>

### <a name="simple-immutable-classes"></a><span data-ttu-id="cfefd-132">Простые неизменяемые классы</span><span class="sxs-lookup"><span data-stu-id="cfefd-132">Simple immutable classes</span></span>

<span data-ttu-id="cfefd-133">Рассмотрим свойство, которое использует преобразователь значений для отображения простого, неизменяемого класса.</span><span class="sxs-lookup"><span data-stu-id="cfefd-133">Consider a property that uses a value converter to map a simple, immutable class.</span></span>

[!code-csharp[SimpleImmutableClass](../../../samples/core/Modeling/ValueConversions/MappingImmutableClassProperty.cs?name=SimpleImmutableClass)]

[!code-csharp[ConfigureImmutableClassProperty](../../../samples/core/Modeling/ValueConversions/MappingImmutableClassProperty.cs?name=ConfigureImmutableClassProperty)]

<span data-ttu-id="cfefd-134">Свойства этого типа не требуют специальных сравнений или моментальных снимков, поскольку:</span><span class="sxs-lookup"><span data-stu-id="cfefd-134">Properties of this type do not need special comparisons or snapshots because:</span></span>
* <span data-ttu-id="cfefd-135">Равенство переопределяется, так что различные экземпляры будут сравниваться правильно</span><span class="sxs-lookup"><span data-stu-id="cfefd-135">Equality is overridden so that different instances will compare correctly</span></span>
* <span data-ttu-id="cfefd-136">Тип является неизменяемым, поэтому невозможно изменить значение моментального снимка</span><span class="sxs-lookup"><span data-stu-id="cfefd-136">The type is immutable, so there is no chance of mutating a snapshot value</span></span>

<span data-ttu-id="cfefd-137">Поэтому в этом случае поведение EF Core по умолчанию отлично от.</span><span class="sxs-lookup"><span data-stu-id="cfefd-137">So in this case the default behavior of EF Core is fine as it is.</span></span>

### <a name="simple-immutable-structs"></a><span data-ttu-id="cfefd-138">Простые неизменяемые структуры</span><span class="sxs-lookup"><span data-stu-id="cfefd-138">Simple immutable Structs</span></span>

<span data-ttu-id="cfefd-139">Сопоставление простых структур также является простым и не требует специальных компараторов или значит.</span><span class="sxs-lookup"><span data-stu-id="cfefd-139">The mapping for simple structs is also simple and requires no special comparers or snapshotting.</span></span>

[!code-csharp[SimpleImmutableStruct](../../../samples/core/Modeling/ValueConversions/MappingImmutableStructProperty.cs?name=SimpleImmutableStruct)]

[!code-csharp[ConfigureImmutableStructProperty](../../../samples/core/Modeling/ValueConversions/MappingImmutableStructProperty.cs?name=ConfigureImmutableStructProperty)]

<span data-ttu-id="cfefd-140">EF Core имеет встроенную поддержку создания скомпилированных, почленном сравнений свойств структуры.</span><span class="sxs-lookup"><span data-stu-id="cfefd-140">EF Core has built-in support for generating compiled, memberwise comparisons of struct properties.</span></span>
<span data-ttu-id="cfefd-141">Это означает, что структуры не требуют переопределения равенства для EF, но вы по-прежнему можете выбрать это по [другим причинам](/dotnet/csharp/programming-guide/statements-expressions-operators/how-to-define-value-equality-for-a-type).</span><span class="sxs-lookup"><span data-stu-id="cfefd-141">This means structs don't need to have equality overridden for EF, but you may still choose to do this for [other reasons](/dotnet/csharp/programming-guide/statements-expressions-operators/how-to-define-value-equality-for-a-type).</span></span>
<span data-ttu-id="cfefd-142">Кроме того, Специальные значит не требуются, так как структуры неизменяемы и всегда почленном копируются.</span><span class="sxs-lookup"><span data-stu-id="cfefd-142">Also, special snapshotting is not needed since structs immutable and are always memberwise copied anyway.</span></span>
<span data-ttu-id="cfefd-143">(Это также справедливо для изменяемых структур, но [в общем случае изменяющиеся структуры следует избегать](/dotnet/csharp/write-safe-efficient-code).)</span><span class="sxs-lookup"><span data-stu-id="cfefd-143">(This is also true for mutable structs, but [mutable structs should in general be avoided](/dotnet/csharp/write-safe-efficient-code).)</span></span>

### <a name="mutable-classes"></a><span data-ttu-id="cfefd-144">Изменяемые классы</span><span class="sxs-lookup"><span data-stu-id="cfefd-144">Mutable classes</span></span>

<span data-ttu-id="cfefd-145">Рекомендуется использовать неизменяемые типы (классы или структуры) с преобразователями значений, если это возможно.</span><span class="sxs-lookup"><span data-stu-id="cfefd-145">It is recommended that you use immutable types (classes or structs) with value converters when possible.</span></span>
<span data-ttu-id="cfefd-146">Обычно это более эффективно и имеет семантику очистки, чем использование изменяемого типа.</span><span class="sxs-lookup"><span data-stu-id="cfefd-146">This is usually more efficient and has cleaner semantics than using a mutable type.</span></span>

<span data-ttu-id="cfefd-147">Тем не менее, часто используются свойства типов, которые приложение не может изменять.</span><span class="sxs-lookup"><span data-stu-id="cfefd-147">However, that being said, it is common to use properties of types that the application cannot change.</span></span>
<span data-ttu-id="cfefd-148">Например, сопоставление свойства, содержащего список чисел:</span><span class="sxs-lookup"><span data-stu-id="cfefd-148">For example, mapping a property containing a list of numbers:</span></span> 

[!code-csharp[ListProperty](../../../samples/core/Modeling/ValueConversions/MappingListProperty.cs?name=ListProperty)]

<span data-ttu-id="cfefd-149">[ `List<T>` Класс](/dotnet/api/system.collections.generic.list-1?view=netstandard-2.1):</span><span class="sxs-lookup"><span data-stu-id="cfefd-149">The [`List<T>` class](/dotnet/api/system.collections.generic.list-1?view=netstandard-2.1):</span></span>
* <span data-ttu-id="cfefd-150">Имеет равенство ссылок; два списка, содержащие одни и те же значения, рассматриваются как разные.</span><span class="sxs-lookup"><span data-stu-id="cfefd-150">Has reference equality; two lists containing the same values are treated as different.</span></span>
* <span data-ttu-id="cfefd-151">Является изменяемым; значения в списке можно добавлять и удалять.</span><span class="sxs-lookup"><span data-stu-id="cfefd-151">Is mutable; values in the list can be added and removed.</span></span>

<span data-ttu-id="cfefd-152">Стандартное преобразование значения для свойства списка может привести к преобразованию списка в JSON и из него:</span><span class="sxs-lookup"><span data-stu-id="cfefd-152">A typical value conversion on a list property might convert the list to and from JSON:</span></span>

[!code-csharp[ConfigureListProperty](../../../samples/core/Modeling/ValueConversions/MappingListProperty.cs?name=ConfigureListProperty)]

<span data-ttu-id="cfefd-153">После этого необходимо задать для `ValueComparer<T>` свойства значение, чтобы принудительно EF Core использовать правильные сравнения с этим преобразованием:</span><span class="sxs-lookup"><span data-stu-id="cfefd-153">This then requires setting a `ValueComparer<T>` on the property to force EF Core use correct comparisons with this conversion:</span></span>

[!code-csharp[ConfigureListPropertyComparer](../../../samples/core/Modeling/ValueConversions/MappingListProperty.cs?name=ConfigureListPropertyComparer)]

> [!NOTE]  
> <span data-ttu-id="cfefd-154">API-интерфейс конструктора моделей ("Fluent") для установки компаратора значений еще не реализован.</span><span class="sxs-lookup"><span data-stu-id="cfefd-154">The model builder ("fluent") API to set a value comparer has not yet been implemented.</span></span>
> <span data-ttu-id="cfefd-155">Вместо этого код вызывает Сетвалуекомпарер на Имутаблепроперти низкого уровня, предоставляемый построителем как "Metadata".</span><span class="sxs-lookup"><span data-stu-id="cfefd-155">Instead, the code above calls SetValueComparer on the lower-level IMutableProperty exposed by the builder as 'Metadata'.</span></span>

<span data-ttu-id="cfefd-156">`ValueComparer<T>`Конструктор принимает три выражения:</span><span class="sxs-lookup"><span data-stu-id="cfefd-156">The `ValueComparer<T>` constructor accepts three expressions:</span></span>
* <span data-ttu-id="cfefd-157">Выражение для проверки равенства</span><span class="sxs-lookup"><span data-stu-id="cfefd-157">An expression for checking equality</span></span>
* <span data-ttu-id="cfefd-158">Выражение для создания хэш-кода</span><span class="sxs-lookup"><span data-stu-id="cfefd-158">An expression for generating a hash code</span></span>
* <span data-ttu-id="cfefd-159">Выражение для создания снимка значения</span><span class="sxs-lookup"><span data-stu-id="cfefd-159">An expression to snapshot a value</span></span>  

<span data-ttu-id="cfefd-160">В этом случае сравнение выполняется путем проверки того, совпадают ли последовательности чисел.</span><span class="sxs-lookup"><span data-stu-id="cfefd-160">In this case the comparison is done by checking if the sequences of numbers are the same.</span></span>

<span data-ttu-id="cfefd-161">Аналогичным образом хэш-код строится из этой же последовательности.</span><span class="sxs-lookup"><span data-stu-id="cfefd-161">Likewise, the hash code is built from this same sequence.</span></span>
<span data-ttu-id="cfefd-162">(Обратите внимание, что это хэш-код для изменяемых значений и, следовательно, может [вызвать проблемы](https://ericlippert.com/2011/02/28/guidelines-and-rules-for-gethashcode/).</span><span class="sxs-lookup"><span data-stu-id="cfefd-162">(Note that this is a hash code over mutable values and hence can [cause problems](https://ericlippert.com/2011/02/28/guidelines-and-rules-for-gethashcode/).</span></span>
<span data-ttu-id="cfefd-163">Быть неизменяемым вместо этого, если это возможно.)</span><span class="sxs-lookup"><span data-stu-id="cfefd-163">Be immutable instead if you can.)</span></span>

<span data-ttu-id="cfefd-164">Моментальный снимок создается путем клонирования списка с помощью ToList.</span><span class="sxs-lookup"><span data-stu-id="cfefd-164">The snapshot is created by cloning the list with ToList.</span></span>
<span data-ttu-id="cfefd-165">Опять же, это необходимо, только если списки будут изменяться.</span><span class="sxs-lookup"><span data-stu-id="cfefd-165">Again, this is only needed if the lists are going to be mutated.</span></span>
<span data-ttu-id="cfefd-166">Быть неизменяемым вместо этого, если это возможно.</span><span class="sxs-lookup"><span data-stu-id="cfefd-166">Be immutable instead if you can.</span></span> 

> [!NOTE]  
> <span data-ttu-id="cfefd-167">Преобразователи значений и компараторы создаются с помощью выражений, а не простых делегатов.</span><span class="sxs-lookup"><span data-stu-id="cfefd-167">Value converters and comparers are constructed using expressions rather than simple delegates.</span></span>
> <span data-ttu-id="cfefd-168">Это обусловлено тем, что EF вставляет эти выражения в гораздо более сложное дерево выражения, которое затем компилируется в делегат сущности-фигуры.</span><span class="sxs-lookup"><span data-stu-id="cfefd-168">This is because EF inserts these expressions into a much more complex expression tree that is then compiled into an entity shaper delegate.</span></span>
> <span data-ttu-id="cfefd-169">По сути, это похоже на встраивание компилятора.</span><span class="sxs-lookup"><span data-stu-id="cfefd-169">Conceptually, this is similar to compiler inlining.</span></span>
> <span data-ttu-id="cfefd-170">Например, простое преобразование может быть просто скомпилированным в приведении, а не вызовом другого метода для преобразования.</span><span class="sxs-lookup"><span data-stu-id="cfefd-170">For example, a simple conversion may just be a compiled in cast, rather than a call to another method to do the conversion.</span></span>    

### <a name="key-comparers"></a><span data-ttu-id="cfefd-171">Ключевые компараторы</span><span class="sxs-lookup"><span data-stu-id="cfefd-171">Key comparers</span></span>

<span data-ttu-id="cfefd-172">В разделе Background описывается, почему для сравнения ключей может потребоваться специальная семантика.</span><span class="sxs-lookup"><span data-stu-id="cfefd-172">The background section covers why key comparisons may require special semantics.</span></span>
<span data-ttu-id="cfefd-173">Обязательно создайте средство сравнения, которое подходит для ключей при его задании в свойстве первичного, основного или внешнего ключа.</span><span class="sxs-lookup"><span data-stu-id="cfefd-173">Make sure to create a comparer that is appropriate for keys when setting it on a primary, principal, or foreign key property.</span></span>

<span data-ttu-id="cfefd-174">Используйте [сеткэйвалуекомпарер](/dotnet/api/microsoft.entityframeworkcore.mutablepropertyextensions.setkeyvaluecomparer?view=efcore-3.1) в редких случаях, когда для одного свойства требуется другая семантика.</span><span class="sxs-lookup"><span data-stu-id="cfefd-174">Use [SetKeyValueComparer](/dotnet/api/microsoft.entityframeworkcore.mutablepropertyextensions.setkeyvaluecomparer?view=efcore-3.1) in the rare cases where different semantics is required on the same property.</span></span>

> [!NOTE]  
> <span data-ttu-id="cfefd-175">Сетструктуралкомпарер в EF Core 5,0 устарел.</span><span class="sxs-lookup"><span data-stu-id="cfefd-175">SetStructuralComparer has been obsoleted in EF Core 5.0.</span></span>
> <span data-ttu-id="cfefd-176">Вместо этого используйте Сеткэйвалуекомпарер.</span><span class="sxs-lookup"><span data-stu-id="cfefd-176">Use SetKeyValueComparer instead.</span></span>

### <a name="overriding-defaults"></a><span data-ttu-id="cfefd-177">Переопределение параметров по умолчанию</span><span class="sxs-lookup"><span data-stu-id="cfefd-177">Overriding defaults</span></span>

<span data-ttu-id="cfefd-178">Иногда сравнение по умолчанию, используемое EF Core, может быть неприменимо.</span><span class="sxs-lookup"><span data-stu-id="cfefd-178">Sometimes the default comparison used by EF Core may not be appropriate.</span></span>
<span data-ttu-id="cfefd-179">Например, изменение массивов байтов по умолчанию не определяется в EF Core.</span><span class="sxs-lookup"><span data-stu-id="cfefd-179">For example, mutation of byte arrays is not, by default, detected in EF Core.</span></span>
<span data-ttu-id="cfefd-180">Это можно переопределить, задав другое средство сравнения для свойства:</span><span class="sxs-lookup"><span data-stu-id="cfefd-180">This can be overridden by setting a different comparer on the property:</span></span> 

[!code-csharp[OverrideComparer](../../../samples/core/Modeling/ValueConversions/OverridingByteArrayComparisons.cs?name=OverrideComparer)]

<span data-ttu-id="cfefd-181">EF Core теперь будет сравнивать последовательности байтов и, следовательно, определит изменения массива байтов.</span><span class="sxs-lookup"><span data-stu-id="cfefd-181">EF Core will now compare byte sequences and will therefore detect byte array mutations.</span></span>
