---
title: Связи - EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 0ff736a3-f1b0-4b58-a49c-4a7094bd6935
ms.technology: entity-framework-core
uid: core/modeling/relationships
ms.openlocfilehash: 1732d32643effb0f12111191ed4ba3abb4182d93
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/27/2017
ms.locfileid: "26053034"
---
# <a name="relationships"></a><span data-ttu-id="403e8-102">Отношения</span><span class="sxs-lookup"><span data-stu-id="403e8-102">Relationships</span></span>

<span data-ttu-id="403e8-103">Связь определяет, как две сущности связаны друг с другом.</span><span class="sxs-lookup"><span data-stu-id="403e8-103">A relationship defines how two entities relate to each other.</span></span> <span data-ttu-id="403e8-104">В реляционной базе данных это будет представлен ограничение внешнего ключа.</span><span class="sxs-lookup"><span data-stu-id="403e8-104">In a relational database, this is represented by a foreign key constraint.</span></span>

> [!NOTE]  
> <span data-ttu-id="403e8-105">Большинство примеров в этой статье используется один ко многим для демонстрации концепции.</span><span class="sxs-lookup"><span data-stu-id="403e8-105">Most of the samples in this article use a one-to-many relationship to demonstrate concepts.</span></span> <span data-ttu-id="403e8-106">См. Примеры одному "и" многие ко многим " [другие шаблоны связь](#other-relationship-patterns) в конце статьи.</span><span class="sxs-lookup"><span data-stu-id="403e8-106">For examples of one-to-one and many-to-many relationships see the [Other Relationship Patterns](#other-relationship-patterns) section at the end of the article.</span></span>

## <a name="definition-of-terms"></a><span data-ttu-id="403e8-107">Определение терминов</span><span class="sxs-lookup"><span data-stu-id="403e8-107">Definition of Terms</span></span>

<span data-ttu-id="403e8-108">Существует несколько терминов, используемых для описания связей</span><span class="sxs-lookup"><span data-stu-id="403e8-108">There are a number of terms used to describe relationships</span></span>

* <span data-ttu-id="403e8-109">**Зависимой сущности:** это сущность, которая содержит внешние ключевые свойства.</span><span class="sxs-lookup"><span data-stu-id="403e8-109">**Dependent entity:** This is the entity that contains the foreign key property(s).</span></span> <span data-ttu-id="403e8-110">Иногда называют 'child' связи.</span><span class="sxs-lookup"><span data-stu-id="403e8-110">Sometimes referred to as the 'child' of the relationship.</span></span>

* <span data-ttu-id="403e8-111">**Основной сущности:** это сущность, которая содержит первичный/альтернативного ключевые свойства.</span><span class="sxs-lookup"><span data-stu-id="403e8-111">**Principal entity:** This is the entity that contains the primary/alternate key property(s).</span></span> <span data-ttu-id="403e8-112">Иногда называют «parent» связи.</span><span class="sxs-lookup"><span data-stu-id="403e8-112">Sometimes referred to as the 'parent' of the relationship.</span></span>

* <span data-ttu-id="403e8-113">**Внешний ключ:** свойств в зависимой сущности, которая используется для хранения значения основной ключевого свойства, относящиеся к сущности.</span><span class="sxs-lookup"><span data-stu-id="403e8-113">**Foreign key:** The property(s) in the dependent entity that is used to store the values of the principal key property that the entity is related to.</span></span>

* <span data-ttu-id="403e8-114">**Основной ключ:** свойства, которое однозначно определяет основной сущности.</span><span class="sxs-lookup"><span data-stu-id="403e8-114">**Principal key:** The property(s) that uniquely identifies the principal entity.</span></span> <span data-ttu-id="403e8-115">Это может быть первичным ключом или дополнительный ключ.</span><span class="sxs-lookup"><span data-stu-id="403e8-115">This may be the primary key or an alternate key.</span></span>

* <span data-ttu-id="403e8-116">**Свойство навигации:** свойство, определенное в основной и зависимый сущность, которая содержит ссылки на связанные объекты.</span><span class="sxs-lookup"><span data-stu-id="403e8-116">**Navigation property:** A property defined on the principal and/or dependent entity that contains a reference(s) to the related entity(s).</span></span>

  * <span data-ttu-id="403e8-117">**Свойство навигации коллекции:** свойство навигации, которое содержит ссылки на множество связанных сущностей.</span><span class="sxs-lookup"><span data-stu-id="403e8-117">**Collection navigation property:** A navigation property that contains references to many related entities.</span></span>

  * <span data-ttu-id="403e8-118">**Ссылаться на свойство навигации:** свойство навигации, которое содержит ссылку на одну связанную сущность.</span><span class="sxs-lookup"><span data-stu-id="403e8-118">**Reference navigation property:** A navigation property that holds a reference to a single related entity.</span></span>

  * <span data-ttu-id="403e8-119">**Обратное свойство навигации:** при обсуждении свойству навигации определенного, этот термин относится к свойству навигации на другом конце связи.</span><span class="sxs-lookup"><span data-stu-id="403e8-119">**Inverse navigation property:** When discussing a particular navigation property, this term refers to the navigation property on the other end of the relationship.</span></span>

<span data-ttu-id="403e8-120">В следующем коде показано отношение "один ко многим" между `Blog` и`Post`</span><span class="sxs-lookup"><span data-stu-id="403e8-120">The following code listing shows a one-to-many relationship between `Blog` and `Post`</span></span>

* <span data-ttu-id="403e8-121">`Post`является зависимой сущности</span><span class="sxs-lookup"><span data-stu-id="403e8-121">`Post` is the dependent entity</span></span>

* <span data-ttu-id="403e8-122">`Blog`является основной сущности</span><span class="sxs-lookup"><span data-stu-id="403e8-122">`Blog` is the principal entity</span></span>

* <span data-ttu-id="403e8-123">`Post.BlogId`внешний ключ</span><span class="sxs-lookup"><span data-stu-id="403e8-123">`Post.BlogId` is the foreign key</span></span>

* <span data-ttu-id="403e8-124">`Blog.BlogId`основной ключ (в данном случае это первичный ключ, а не дополнительный ключ)</span><span class="sxs-lookup"><span data-stu-id="403e8-124">`Blog.BlogId` is the principal key (in this case it is a primary key rather than an alternate key)</span></span>

* <span data-ttu-id="403e8-125">`Post.Blog`является свойством навигации ссылки</span><span class="sxs-lookup"><span data-stu-id="403e8-125">`Post.Blog` is a reference navigation property</span></span>

* <span data-ttu-id="403e8-126">`Blog.Posts`является свойством навигации коллекции</span><span class="sxs-lookup"><span data-stu-id="403e8-126">`Blog.Posts` is a collection navigation property</span></span>

* <span data-ttu-id="403e8-127">`Post.Blog`свойство навигации обратный `Blog.Posts` (и наоборот)</span><span class="sxs-lookup"><span data-stu-id="403e8-127">`Post.Blog` is the inverse navigation property of `Blog.Posts` (and vice versa)</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Samples/Relationships/Full.cs#Entities)]

## <a name="conventions"></a><span data-ttu-id="403e8-128">Соглашения</span><span class="sxs-lookup"><span data-stu-id="403e8-128">Conventions</span></span>

<span data-ttu-id="403e8-129">По соглашению связь создается, если свойство навигации, обнаруженных на тип.</span><span class="sxs-lookup"><span data-stu-id="403e8-129">By convention, a relationship will be created when there is a navigation property discovered on a type.</span></span> <span data-ttu-id="403e8-130">Если тип, который он указывает не могут быть сопоставлены как скалярный тип поставщиком базы данных, свойство считается свойством навигации.</span><span class="sxs-lookup"><span data-stu-id="403e8-130">A property is considered a navigation property if the type it points to can not be mapped as a scalar type by the current database provider.</span></span>

> [!NOTE]  
> <span data-ttu-id="403e8-131">Связи, которые обнаруживаются в соответствии с соглашением всегда будет использовать первичный ключ основной сущности.</span><span class="sxs-lookup"><span data-stu-id="403e8-131">Relationships that are discovered by convention will always target the primary key of the principal entity.</span></span> <span data-ttu-id="403e8-132">Чтобы выбрать дополнительный ключ, дополнительная настройка выполняется с помощью плавного API.</span><span class="sxs-lookup"><span data-stu-id="403e8-132">To target an alternate key, additional configuration must be performed using the Fluent API.</span></span>

### <a name="fully-defined-relationships"></a><span data-ttu-id="403e8-133">Полностью определенные связи</span><span class="sxs-lookup"><span data-stu-id="403e8-133">Fully Defined Relationships</span></span>

<span data-ttu-id="403e8-134">Чаще всего используется шаблон для связи — имеют свойства навигации, определенные на обоих концах связи и свойству внешнего ключа, определенное в классе зависимой сущности.</span><span class="sxs-lookup"><span data-stu-id="403e8-134">The most common pattern for relationships is to have navigation properties defined on both ends of the relationship and a foreign key property defined in the dependent entity class.</span></span>

* <span data-ttu-id="403e8-135">Найденные парой свойств навигации между двумя типами, они будут настроены как свойства навигации обратный той же связи.</span><span class="sxs-lookup"><span data-stu-id="403e8-135">If a pair of navigation properties is found between two types, then they will be configured as inverse navigation properties of the same relationship.</span></span>

* <span data-ttu-id="403e8-136">Если зависимая сущность содержит свойство с именем `<primary key property name>`, `<navigation property name><primary key property name>`, или `<principal entity name><primary key property name>` , то он будет настроен в качестве внешнего ключа.</span><span class="sxs-lookup"><span data-stu-id="403e8-136">If the dependent entity contains a property named `<primary key property name>`, `<navigation property name><primary key property name>`, or `<principal entity name><primary key property name>` then it will be configured as the foreign key.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Samples/Relationships/Full.cs?name=Entities&highlight=6,15,16)]

> [!WARNING]  
> <span data-ttu-id="403e8-137">Если существует несколько свойств навигации, определенных между двумя типами (т. е. существует несколько различных пары переходы, которые указывают друг с другом), затем связи не будут созданы по соглашению, и необходимо будет вручную настроить их для идентификации как Сопоставить свойства навигации.</span><span class="sxs-lookup"><span data-stu-id="403e8-137">If there are multiple navigation properties defined between two types (i.e. more than one distinct pair of navigations that point to each other), then no relationships will be created by convention and you will need to manually configure them to identify how the navigation properties pair up.</span></span>

### <a name="no-foreign-key-property"></a><span data-ttu-id="403e8-138">Нет свойство внешнего ключа</span><span class="sxs-lookup"><span data-stu-id="403e8-138">No Foreign Key Property</span></span>

<span data-ttu-id="403e8-139">Рекомендуется иметь свойство внешнего ключа, определенное в классе зависимой сущности, однако он не требуется.</span><span class="sxs-lookup"><span data-stu-id="403e8-139">While it is recommended to have a foreign key property defined in the dependent entity class, it is not required.</span></span> <span data-ttu-id="403e8-140">При обнаружении не свойство внешнего ключа с именем будут вводиться тени свойство внешнего ключа `<navigation property name><principal key property name>` (см. [замещения свойств](shadow-properties.md) для получения дополнительной информации).</span><span class="sxs-lookup"><span data-stu-id="403e8-140">If no foreign key property is found, a shadow foreign key property will be introduced with the name `<navigation property name><principal key property name>` (see [Shadow Properties](shadow-properties.md) for more information).</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Samples/Relationships/NoForeignKey.cs?name=Entities&highlight=6,15)]

### <a name="single-navigation-property"></a><span data-ttu-id="403e8-141">Одному свойству навигации</span><span class="sxs-lookup"><span data-stu-id="403e8-141">Single Navigation Property</span></span>

<span data-ttu-id="403e8-142">Включая только одно свойство навигации (без обратной навигации и не свойство внешнего ключа) достаточно определена по соглашению связь.</span><span class="sxs-lookup"><span data-stu-id="403e8-142">Including just one navigation property (no inverse navigation, and no foreign key property) is enough to have a relationship defined by convention.</span></span> <span data-ttu-id="403e8-143">Вы также можете к одному свойству навигации и свойству внешнего ключа.</span><span class="sxs-lookup"><span data-stu-id="403e8-143">You can also have a single navigation property and a foreign key property.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Samples/Relationships/OneNavigation.cs?name=Entities&highlight=6)]

### <a name="cascade-delete"></a><span data-ttu-id="403e8-144">Каскадное удаление</span><span class="sxs-lookup"><span data-stu-id="403e8-144">Cascade Delete</span></span>

<span data-ttu-id="403e8-145">По соглашению каскадное удаление будет присвоено *Cascade* для необходимые связи и *ClientSetNull* для связей, необязательно.</span><span class="sxs-lookup"><span data-stu-id="403e8-145">By convention, cascade delete will be set to *Cascade* for required relationships and *ClientSetNull* for optional relationships.</span></span> <span data-ttu-id="403e8-146">*CASCADE* означает зависимые сущности также удаляются.</span><span class="sxs-lookup"><span data-stu-id="403e8-146">*Cascade* means dependent entities are also deleted.</span></span> <span data-ttu-id="403e8-147">*ClientSetNull* означает, что зависимые сущности, которые не были загружены в память останется без изменений и должны быть удалены вручную или обновлен, чтобы она указывала на допустимый основной сущности.</span><span class="sxs-lookup"><span data-stu-id="403e8-147">*ClientSetNull* means that dependent entities that are not loaded into memory will remain unchanged and must be manually deleted, or updated to point to a valid principal entity.</span></span> <span data-ttu-id="403e8-148">Для сущностей, которые загружаются в память ядра EF попытается установить для свойства внешнего ключа значение null.</span><span class="sxs-lookup"><span data-stu-id="403e8-148">For entities that are loaded into memory, EF Core will attempt to set the foreign key properties to null.</span></span>

<span data-ttu-id="403e8-149">. В разделе [связи обязательные и необязательные](#required-and-optional-relationships) раздел разницу между обязательных и необязательных связи.</span><span class="sxs-lookup"><span data-stu-id="403e8-149">See the [Required and Optional Relationships](#required-and-optional-relationships) section for the difference between required and optional relationships.</span></span>

<span data-ttu-id="403e8-150">В разделе [каскадное удаление](../saving/cascade-delete.md) для поведения и значения по умолчанию используется соглашение об удалении Дополнительные сведения о различных.</span><span class="sxs-lookup"><span data-stu-id="403e8-150">See [Cascade Delete](../saving/cascade-delete.md) for more details about the different delete behaviors and the defaults used by convention.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="403e8-151">Заметки к данным</span><span class="sxs-lookup"><span data-stu-id="403e8-151">Data Annotations</span></span>

<span data-ttu-id="403e8-152">Существует два данных заметок, которые можно использовать для настройки связи, `[ForeignKey]` и `[InverseProperty]`.</span><span class="sxs-lookup"><span data-stu-id="403e8-152">There are two data annotations that can be used to configure relationships, `[ForeignKey]` and `[InverseProperty]`.</span></span>

### <a name="foreignkey"></a><span data-ttu-id="403e8-153">[ForeignKey]</span><span class="sxs-lookup"><span data-stu-id="403e8-153">[ForeignKey]</span></span>

<span data-ttu-id="403e8-154">Заметки данных можно использовать для настройки, какое свойство следует использовать как свойство внешнего ключа для данного отношения.</span><span class="sxs-lookup"><span data-stu-id="403e8-154">You can use the Data Annotations to configure which property should be used as the foreign key property for a given relationship.</span></span> <span data-ttu-id="403e8-155">Обычно это делается, если свойство внешнего ключа не обнаруживаются по соглашению.</span><span class="sxs-lookup"><span data-stu-id="403e8-155">This is typically done when the foreign key property is not discovered by convention.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Samples/Relationships/ForeignKey.cs?name=Entities&highlight=17)]

> [!TIP]  
> <span data-ttu-id="403e8-156">`[ForeignKey]` Заметки можно поместить в свойство навигации, либо в связи.</span><span class="sxs-lookup"><span data-stu-id="403e8-156">The `[ForeignKey]` annotation can be placed on either navigation property in the relationship.</span></span> <span data-ttu-id="403e8-157">Он не должен перейти на свойство навигации в классе зависимой сущности.</span><span class="sxs-lookup"><span data-stu-id="403e8-157">It does not need to go on the navigation property in the dependent entity class.</span></span>

### <a name="inverseproperty"></a><span data-ttu-id="403e8-158">[InverseProperty]</span><span class="sxs-lookup"><span data-stu-id="403e8-158">[InverseProperty]</span></span>

<span data-ttu-id="403e8-159">Заметки данных можно использовать для настройки, как сопоставить свойства навигации для сущностей зависимой и участника.</span><span class="sxs-lookup"><span data-stu-id="403e8-159">You can use the Data Annotations to configure how navigation properties on the dependent and principal entities pair up.</span></span> <span data-ttu-id="403e8-160">Обычно это делается при наличии более двух свойств навигации между двумя типами сущностей.</span><span class="sxs-lookup"><span data-stu-id="403e8-160">This is typically done when there is more than one pair of navigation properties between two entity types.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Samples/Relationships/InverseProperty.cs?name=Entities&highlight=20,23)]

## <a name="fluent-api"></a><span data-ttu-id="403e8-161">Fluent API</span><span class="sxs-lookup"><span data-stu-id="403e8-161">Fluent API</span></span>

<span data-ttu-id="403e8-162">Чтобы настроить плавного API связи, начинается с определения свойства навигации, которые образуют связь.</span><span class="sxs-lookup"><span data-stu-id="403e8-162">To configure a relationship in the Fluent API, you start by identifying the navigation properties that make up the relationship.</span></span> <span data-ttu-id="403e8-163">`HasOne`или `HasMany` определяет свойство навигации типа сущности, в начале конфигурации.</span><span class="sxs-lookup"><span data-stu-id="403e8-163">`HasOne` or `HasMany` identifies the navigation property on the entity type you are beginning the configuration on.</span></span> <span data-ttu-id="403e8-164">Вы затем цепочку вызов `WithOne` или `WithMany` для идентификации обратной навигации.</span><span class="sxs-lookup"><span data-stu-id="403e8-164">You then chain a call to `WithOne` or `WithMany` to identify the inverse navigation.</span></span> <span data-ttu-id="403e8-165">`HasOne`/`WithOne`используется для свойств навигации ссылки и `HasMany` / `WithMany` используются для свойств навигации коллекции.</span><span class="sxs-lookup"><span data-stu-id="403e8-165">`HasOne`/`WithOne` are used for reference navigation properties and `HasMany`/`WithMany` are used for collection navigation properties.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Relationships/NoForeignKey.cs?name=Model&highlight=8,9,10)]

### <a name="single-navigation-property"></a><span data-ttu-id="403e8-166">Одному свойству навигации</span><span class="sxs-lookup"><span data-stu-id="403e8-166">Single Navigation Property</span></span>

<span data-ttu-id="403e8-167">Если имеется только одно свойство навигации, то существуют перегрузки без параметров `WithOne` и `WithMany`.</span><span class="sxs-lookup"><span data-stu-id="403e8-167">If you only have one navigation property then there are parameterless overloads of `WithOne` and `WithMany`.</span></span> <span data-ttu-id="403e8-168">Это означает, что существует по существу является ссылкой или коллекцией на другом конце канала связи, но отсутствует свойство навигации, включенных в класс сущностей.</span><span class="sxs-lookup"><span data-stu-id="403e8-168">This indicates that there is conceptually a reference or collection on the other end of the relationship, but there is no navigation property included in the entity class.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Relationships/OneNavigation.cs?name=Model&highlight=10)]

### <a name="foreign-key"></a><span data-ttu-id="403e8-169">Внешний ключ</span><span class="sxs-lookup"><span data-stu-id="403e8-169">Foreign Key</span></span>

<span data-ttu-id="403e8-170">Fluent API можно использовать для настройки того, какое свойство следует использовать как свойство внешнего ключа для данного отношения.</span><span class="sxs-lookup"><span data-stu-id="403e8-170">You can use the Fluent API to configure which property should be used as the foreign key property for a given relationship.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Relationships/ForeignKey.cs?name=Model&highlight=11)]

<span data-ttu-id="403e8-171">В следующем коде показано, как настроить составного внешнего ключа.</span><span class="sxs-lookup"><span data-stu-id="403e8-171">The following code listing shows how to configure a composite foreign key.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Relationships/CompositeForeignKey.cs?name=Model&highlight=13)]

<span data-ttu-id="403e8-172">Можно использовать перегруженный строка `HasForeignKey(...)` для настройки свойств тени в качестве внешнего ключа (см. [замещения свойств](shadow-properties.md) для получения дополнительной информации).</span><span class="sxs-lookup"><span data-stu-id="403e8-172">You can use the string overload of `HasForeignKey(...)` to configure a shadow property as a foreign key (see [Shadow Properties](shadow-properties.md) for more information).</span></span> <span data-ttu-id="403e8-173">Рекомендуется явно добавить в модель свойство теневого перед его использованием в качестве внешнего ключа (как показано ниже).</span><span class="sxs-lookup"><span data-stu-id="403e8-173">We recommend explicitly adding the shadow property to the model before using it as a foreign key (as shown below).</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Relationships/ShadowForeignKey.cs#Sample)]

### <a name="principal-key"></a><span data-ttu-id="403e8-174">Основной ключ</span><span class="sxs-lookup"><span data-stu-id="403e8-174">Principal Key</span></span>

<span data-ttu-id="403e8-175">Если требуется внешний ключ к ссылке на свойство не является первичным ключом, можно использовать Fluent API для настройки свойство основного ключа для связи.</span><span class="sxs-lookup"><span data-stu-id="403e8-175">If you want the foreign key to reference a property other than the primary key, you can use the Fluent API to configure the principal key property for the relationship.</span></span> <span data-ttu-id="403e8-176">Свойство, которое настроенному в качестве основного ключа будет автоматически будет настроен как дополнительный ключ (см. [альтернативные ключи](alternate-keys.md) для получения дополнительной информации).</span><span class="sxs-lookup"><span data-stu-id="403e8-176">The property that you configure as the principal key will automatically be setup as an alternate key (see [Alternate Keys](alternate-keys.md) for more information).</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Samples/Relationships/PrincipalKey.cs?highlight=11)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Car> Cars { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<RecordOfSale>()
            .HasOne(s => s.Car)
            .WithMany(c => c.SaleHistory)
            .HasForeignKey(s => s.CarLicensePlate)
            .HasPrincipalKey(c => c.LicensePlate);
    }
}

public class Car
{
    public int CarId { get; set; }
    public string LicensePlate { get; set; }
    public string Make { get; set; }
    public string Model { get; set; }

    public List<RecordOfSale> SaleHistory { get; set; }
}

public class RecordOfSale
{
    public int RecordOfSaleId { get; set; }
    public DateTime DateSold { get; set; }
    public decimal Price { get; set; }

    public string CarLicensePlate { get; set; }
    public Car Car { get; set; }
}
```

<span data-ttu-id="403e8-177">В следующем коде показано, как настройка составного ключа субъекта.</span><span class="sxs-lookup"><span data-stu-id="403e8-177">The following code listing shows how to configure a composite principal key.</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Samples/Relationships/CompositePrincipalKey.cs?highlight=11)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Car> Cars { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<RecordOfSale>()
            .HasOne(s => s.Car)
            .WithMany(c => c.SaleHistory)
            .HasForeignKey(s => new { s.CarState, s.CarLicensePlate })
            .HasPrincipalKey(c => new { c.State, c.LicensePlate });
    }
}

public class Car
{
    public int CarId { get; set; }
    public string State { get; set; }
    public string LicensePlate { get; set; }
    public string Make { get; set; }
    public string Model { get; set; }

    public List<RecordOfSale> SaleHistory { get; set; }
}

public class RecordOfSale
{
    public int RecordOfSaleId { get; set; }
    public DateTime DateSold { get; set; }
    public decimal Price { get; set; }

    public string CarState { get; set; }
    public string CarLicensePlate { get; set; }
    public Car Car { get; set; }
}
```

> [!WARNING]  
> <span data-ttu-id="403e8-178">Порядок, в котором указываются свойства основного ключа должен совпадать с порядком, в котором они указаны для внешнего ключа.</span><span class="sxs-lookup"><span data-stu-id="403e8-178">The order in which you specify principal key properties must match the order in which they are specified for the foreign key.</span></span>

### <a name="required-and-optional-relationships"></a><span data-ttu-id="403e8-179">Обязательные и необязательные связей</span><span class="sxs-lookup"><span data-stu-id="403e8-179">Required and Optional Relationships</span></span>

<span data-ttu-id="403e8-180">Fluent API позволяет настроить, является ли связь обязательными или необязательными.</span><span class="sxs-lookup"><span data-stu-id="403e8-180">You can use the Fluent API to configure whether the relationship is required or optional.</span></span> <span data-ttu-id="403e8-181">В конечном счете это контролирует, представляет ли свойство внешнего ключа обязательными или необязательными.</span><span class="sxs-lookup"><span data-stu-id="403e8-181">Ultimately this controls whether the foreign key property is required or optional.</span></span> <span data-ttu-id="403e8-182">Это наиболее полезно при использовании внешнего ключа состояния тени.</span><span class="sxs-lookup"><span data-stu-id="403e8-182">This is most useful when you are using a shadow state foreign key.</span></span> <span data-ttu-id="403e8-183">Если у вас есть свойство внешнего ключа в классе сущностей, то requiredness связи определяется на основе того, является ли свойство внешнего ключа обязательными или необязательными (см. [обязательные и необязательные свойства](required-optional.md) для получения дополнительных сведений сведения о).</span><span class="sxs-lookup"><span data-stu-id="403e8-183">If you have a foreign key property in your entity class then the requiredness of the relationship is determined based on whether the foreign key property is required or optional (see [Required and Optional properties](required-optional.md) for more information).</span></span>

<!-- [!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Relationships/Required.cs?highlight=11)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }
    public DbSet<Post> Posts { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Post>()
            .HasOne(p => p.Blog)
            .WithMany(b => b.Posts)
            .IsRequired();
    }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }

    public List<Post> Posts { get; set; }
}

public class Post
{
    public int PostId { get; set; }
    public string Title { get; set; }
    public string Content { get; set; }

    public Blog Blog { get; set; }
}
```

### <a name="cascade-delete"></a><span data-ttu-id="403e8-184">Каскадное удаление</span><span class="sxs-lookup"><span data-stu-id="403e8-184">Cascade Delete</span></span>

<span data-ttu-id="403e8-185">Fluent API можно использовать для настройки поведения delete cascade для конкретной связи явным образом.</span><span class="sxs-lookup"><span data-stu-id="403e8-185">You can use the Fluent API to configure the cascade delete behavior for a given relationship explicitly.</span></span>

<span data-ttu-id="403e8-186">В разделе [каскадного удаления](../saving/cascade-delete.md) на раздел Сохранение данных подробное описание каждого параметра.</span><span class="sxs-lookup"><span data-stu-id="403e8-186">See [Cascade Delete](../saving/cascade-delete.md) on the Saving Data section for a detailed discussion of each option.</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Samples/Relationships/CascadeDelete.cs?highlight=11)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }
    public DbSet<Post> Posts { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Post>()
            .HasOne(p => p.Blog)
            .WithMany(b => b.Posts)
            .OnDelete(DeleteBehavior.Cascade);
    }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }

    public List<Post> Posts { get; set; }
}

public class Post
{
    public int PostId { get; set; }
    public string Title { get; set; }
    public string Content { get; set; }

    public int? BlogId { get; set; }
    public Blog Blog { get; set; }
}
```

## <a name="other-relationship-patterns"></a><span data-ttu-id="403e8-187">Другие модели, связи</span><span class="sxs-lookup"><span data-stu-id="403e8-187">Other Relationship Patterns</span></span>

### <a name="one-to-one"></a><span data-ttu-id="403e8-188">Один к одному</span><span class="sxs-lookup"><span data-stu-id="403e8-188">One-to-one</span></span>

<span data-ttu-id="403e8-189">Один к одному отношениями свойством навигации ссылки на обеих сторонах.</span><span class="sxs-lookup"><span data-stu-id="403e8-189">One to one relationships have a reference navigation property on both sides.</span></span> <span data-ttu-id="403e8-190">Они следуют тем же правилам, как один ко многим отношения, но появилась в свойстве внешнего ключа, чтобы убедиться, что каждому участнику связан только один зависит от уникального индекса.</span><span class="sxs-lookup"><span data-stu-id="403e8-190">They follow the same conventions as one-to-many relationships, but a unique index is introduced on the foreign key property to ensure only one dependent is related to each principal.</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/Conventions/Samples/Relationships/OneToOne.cs?highlight=6,15,16)] -->
``` csharp
public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }

    public BlogImage BlogImage { get; set; }
}

public class BlogImage
{
    public int BlogImageId { get; set; }
    public byte[] Image { get; set; }
    public string Caption { get; set; }

    public int BlogId { get; set; }
    public Blog Blog { get; set; }
}
```

> [!NOTE]  
> <span data-ttu-id="403e8-191">EF выбирает одну из сущности должны находиться зависимого его возможностями для обнаружения свойству внешнего ключа.</span><span class="sxs-lookup"><span data-stu-id="403e8-191">EF will choose one of the entities to be the dependent based on its ability to detect a foreign key property.</span></span> <span data-ttu-id="403e8-192">Если неверный сущности, выбирается в качестве зависимого, чтобы исправить эту ошибку можно использовать Fluent API.</span><span class="sxs-lookup"><span data-stu-id="403e8-192">If the wrong entity is chosen as the dependent, you can use the Fluent API to correct this.</span></span>

<span data-ttu-id="403e8-193">При настройке связи с помощью плавного API, используйте `HasOne` и `WithOne` методы.</span><span class="sxs-lookup"><span data-stu-id="403e8-193">When configuring the relationship with the Fluent API, you use the `HasOne` and `WithOne` methods.</span></span>

<span data-ttu-id="403e8-194">При настройке внешнего ключа, необходимо указать тип зависимого объекта - Обратите внимание, предоставленный для универсального параметра `HasForeignKey` в списке ниже.</span><span class="sxs-lookup"><span data-stu-id="403e8-194">When configuring the foreign key you need to specify the dependent entity type - notice the generic parameter provided to `HasForeignKey` in the listing below.</span></span> <span data-ttu-id="403e8-195">В отношении один ко многим было ясно, что зависит от сущности с ссылки навигации и с коллекции является участником.</span><span class="sxs-lookup"><span data-stu-id="403e8-195">In a one-to-many relationship it is clear that the entity with the reference navigation is the dependent and the one with the collection is the principal.</span></span> <span data-ttu-id="403e8-196">Но это не так в однозначное - таким образом необходимость явного определения.</span><span class="sxs-lookup"><span data-stu-id="403e8-196">But this is not so in a one-to-one relationship - hence the need to explicitly define it.</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Samples/Relationships/OneToOne.cs?highlight=11)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }
    public DbSet<BlogImage> BlogImages { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Blog>()
            .HasOne(p => p.BlogImage)
            .WithOne(i => i.Blog)
            .HasForeignKey<BlogImage>(b => b.BlogForeignKey);
    }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }

    public BlogImage BlogImage { get; set; }
}

public class BlogImage
{
    public int BlogImageId { get; set; }
    public byte[] Image { get; set; }
    public string Caption { get; set; }

    public int BlogForeignKey { get; set; }
    public Blog Blog { get; set; }
}
```

### <a name="many-to-many"></a><span data-ttu-id="403e8-197">«Многие ко многим»</span><span class="sxs-lookup"><span data-stu-id="403e8-197">Many-to-many</span></span>

<span data-ttu-id="403e8-198">Многие ко многим связи без класса сущности, который представляет таблицу соединения еще не поддерживается.</span><span class="sxs-lookup"><span data-stu-id="403e8-198">Many-to-many relationships without an entity class to represent the join table are not yet supported.</span></span> <span data-ttu-id="403e8-199">Тем не менее может представлять многие ко многим, включая класс сущностей для таблицы, соединения и две отдельные связи один ко многим сопоставления.</span><span class="sxs-lookup"><span data-stu-id="403e8-199">However, you can represent a many-to-many relationship by including an entity class for the join table and mapping two separate one-to-many relationships.</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Samples/Relationships/ManyToMany.cs?highlight=11,12,13,14,16,17,18,19,39,40,41,42,43,44,45,46)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Post> Posts { get; set; }
    public DbSet<Tag> Tags { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<PostTag>()
            .HasKey(t => new { t.PostId, t.TagId });

        modelBuilder.Entity<PostTag>()
            .HasOne(pt => pt.Post)
            .WithMany(p => p.PostTags)
            .HasForeignKey(pt => pt.PostId);

        modelBuilder.Entity<PostTag>()
            .HasOne(pt => pt.Tag)
            .WithMany(t => t.PostTags)
            .HasForeignKey(pt => pt.TagId);
    }
}

public class Post
{
    public int PostId { get; set; }
    public string Title { get; set; }
    public string Content { get; set; }

    public List<PostTag> PostTags { get; set; }
}

public class Tag
{
    public string TagId { get; set; }

    public List<PostTag> PostTags { get; set; }
}

public class PostTag
{
    public int PostId { get; set; }
    public Post Post { get; set; }

    public string TagId { get; set; }
    public Tag Tag { get; set; }
}
```
