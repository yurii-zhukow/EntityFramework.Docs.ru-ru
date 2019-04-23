---
title: Связи — EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 0ff736a3-f1b0-4b58-a49c-4a7094bd6935
uid: core/modeling/relationships
ms.openlocfilehash: 9ef1a9269fc99f5b27a81c11a161ed5f9d74180d
ms.sourcegitcommit: 87fcaba46535aa351db4bdb1231bd14b40e459b9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/22/2019
ms.locfileid: "59929941"
---
# <a name="relationships"></a><span data-ttu-id="00a12-102">Отношения</span><span class="sxs-lookup"><span data-stu-id="00a12-102">Relationships</span></span>

<span data-ttu-id="00a12-103">Отношение определяет, каким образом две сущности связаны друг с другом.</span><span class="sxs-lookup"><span data-stu-id="00a12-103">A relationship defines how two entities relate to each other.</span></span> <span data-ttu-id="00a12-104">В реляционной базе данных оно представлено ограничение внешнего ключа.</span><span class="sxs-lookup"><span data-stu-id="00a12-104">In a relational database, this is represented by a foreign key constraint.</span></span>

> [!NOTE]  
> <span data-ttu-id="00a12-105">Большинство примеров в этой статье для демонстрации понятия использовать отношение один ко многим.</span><span class="sxs-lookup"><span data-stu-id="00a12-105">Most of the samples in this article use a one-to-many relationship to demonstrate concepts.</span></span> <span data-ttu-id="00a12-106">Примеры использования одному "и" многие ко многим "см. [другие шаблоны связь](#other-relationship-patterns) в конце статьи.</span><span class="sxs-lookup"><span data-stu-id="00a12-106">For examples of one-to-one and many-to-many relationships see the [Other Relationship Patterns](#other-relationship-patterns) section at the end of the article.</span></span>

## <a name="definition-of-terms"></a><span data-ttu-id="00a12-107">Определение терминов</span><span class="sxs-lookup"><span data-stu-id="00a12-107">Definition of Terms</span></span>

<span data-ttu-id="00a12-108">Существует несколько терминов, используемых для описания связей</span><span class="sxs-lookup"><span data-stu-id="00a12-108">There are a number of terms used to describe relationships</span></span>

* <span data-ttu-id="00a12-109">**Зависимой сущности:** Это сущность, которая содержит свойства внешнего ключа.</span><span class="sxs-lookup"><span data-stu-id="00a12-109">**Dependent entity:** This is the entity that contains the foreign key property(s).</span></span> <span data-ttu-id="00a12-110">Иногда называют «дочерний» связи.</span><span class="sxs-lookup"><span data-stu-id="00a12-110">Sometimes referred to as the 'child' of the relationship.</span></span>

* <span data-ttu-id="00a12-111">**Основной сущности:** Это сущность, которая содержит ключевые свойства основного или альтернативное.</span><span class="sxs-lookup"><span data-stu-id="00a12-111">**Principal entity:** This is the entity that contains the primary/alternate key property(s).</span></span> <span data-ttu-id="00a12-112">Иногда называют «parent» связи.</span><span class="sxs-lookup"><span data-stu-id="00a12-112">Sometimes referred to as the 'parent' of the relationship.</span></span>

* <span data-ttu-id="00a12-113">**Внешний ключ:** Свойства в зависимой сущности, который используется для хранения значений субъекта ключевое свойство, сущность связана с.</span><span class="sxs-lookup"><span data-stu-id="00a12-113">**Foreign key:** The property(s) in the dependent entity that is used to store the values of the principal key property that the entity is related to.</span></span>

* <span data-ttu-id="00a12-114">**Основной ключ:** Свойства, которое однозначно определяет основной сущности.</span><span class="sxs-lookup"><span data-stu-id="00a12-114">**Principal key:** The property(s) that uniquely identifies the principal entity.</span></span> <span data-ttu-id="00a12-115">Это может быть первичный ключ или резервный ключ.</span><span class="sxs-lookup"><span data-stu-id="00a12-115">This may be the primary key or an alternate key.</span></span>

* <span data-ttu-id="00a12-116">**Свойство навигации:** Свойства, определенные в основной и/или зависимой сущности, содержащей ссылки на связанные объекты.</span><span class="sxs-lookup"><span data-stu-id="00a12-116">**Navigation property:** A property defined on the principal and/or dependent entity that contains a reference(s) to the related entity(s).</span></span>

  * <span data-ttu-id="00a12-117">**Свойство навигации по коллекции:** Свойство навигации, которое содержит ссылки на множество связанных сущностей.</span><span class="sxs-lookup"><span data-stu-id="00a12-117">**Collection navigation property:** A navigation property that contains references to many related entities.</span></span>

  * <span data-ttu-id="00a12-118">**Свойство навигации ссылки на:** Свойство навигации, которое содержит ссылку на одну связанную сущность.</span><span class="sxs-lookup"><span data-stu-id="00a12-118">**Reference navigation property:** A navigation property that holds a reference to a single related entity.</span></span>

  * <span data-ttu-id="00a12-119">**Свойство обратной навигации:** При обсуждении конкретным свойством навигации, этот термин относится к свойству навигации на другом конце связи.</span><span class="sxs-lookup"><span data-stu-id="00a12-119">**Inverse navigation property:** When discussing a particular navigation property, this term refers to the navigation property on the other end of the relationship.</span></span>

<span data-ttu-id="00a12-120">В следующем примере кода показаны отношение "один ко многим" между `Blog` и `Post`</span><span class="sxs-lookup"><span data-stu-id="00a12-120">The following code listing shows a one-to-many relationship between `Blog` and `Post`</span></span>

* <span data-ttu-id="00a12-121">`Post` является зависимой сущности</span><span class="sxs-lookup"><span data-stu-id="00a12-121">`Post` is the dependent entity</span></span>

* <span data-ttu-id="00a12-122">`Blog` — Это основной сущности</span><span class="sxs-lookup"><span data-stu-id="00a12-122">`Blog` is the principal entity</span></span>

* <span data-ttu-id="00a12-123">`Post.BlogId` внешний ключ</span><span class="sxs-lookup"><span data-stu-id="00a12-123">`Post.BlogId` is the foreign key</span></span>

* <span data-ttu-id="00a12-124">`Blog.BlogId` является главным ключом (в данном случае это первичный ключ, а не альтернативного ключа)</span><span class="sxs-lookup"><span data-stu-id="00a12-124">`Blog.BlogId` is the principal key (in this case it is a primary key rather than an alternate key)</span></span>

* <span data-ttu-id="00a12-125">`Post.Blog` — Это свойство навигации ссылки</span><span class="sxs-lookup"><span data-stu-id="00a12-125">`Post.Blog` is a reference navigation property</span></span>

* <span data-ttu-id="00a12-126">`Blog.Posts` является свойством навигации коллекции</span><span class="sxs-lookup"><span data-stu-id="00a12-126">`Blog.Posts` is a collection navigation property</span></span>

* <span data-ttu-id="00a12-127">`Post.Blog` свойство навигации обратный `Blog.Posts` (и наоборот)</span><span class="sxs-lookup"><span data-stu-id="00a12-127">`Post.Blog` is the inverse navigation property of `Blog.Posts` (and vice versa)</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Samples/Relationships/Full.cs#Entities)]

## <a name="conventions"></a><span data-ttu-id="00a12-128">Соглашения</span><span class="sxs-lookup"><span data-stu-id="00a12-128">Conventions</span></span>

<span data-ttu-id="00a12-129">По соглашению связь будет создан при наличии свойства навигации, обнаруженных на тип.</span><span class="sxs-lookup"><span data-stu-id="00a12-129">By convention, a relationship will be created when there is a navigation property discovered on a type.</span></span> <span data-ttu-id="00a12-130">Свойство считается свойство навигации, если тип, который указывает не могут сопоставляться как скалярный тип текущим поставщиком базы данных.</span><span class="sxs-lookup"><span data-stu-id="00a12-130">A property is considered a navigation property if the type it points to can not be mapped as a scalar type by the current database provider.</span></span>

> [!NOTE]  
> <span data-ttu-id="00a12-131">Связи, которые обнаруживаются в соответствии с соглашением, всегда будут обрабатываться первичный ключ основной сущности.</span><span class="sxs-lookup"><span data-stu-id="00a12-131">Relationships that are discovered by convention will always target the primary key of the principal entity.</span></span> <span data-ttu-id="00a12-132">Для альтернативного ключа, дополнительная настройка выполняется с помощью Fluent API.</span><span class="sxs-lookup"><span data-stu-id="00a12-132">To target an alternate key, additional configuration must be performed using the Fluent API.</span></span>

### <a name="fully-defined-relationships"></a><span data-ttu-id="00a12-133">Полностью определенные связи</span><span class="sxs-lookup"><span data-stu-id="00a12-133">Fully Defined Relationships</span></span>

<span data-ttu-id="00a12-134">Самый распространенный шаблон для связи — имеют свойства навигации, определенные на обоих концах связи и свойство внешнего ключа, определенные в классе зависимой сущности.</span><span class="sxs-lookup"><span data-stu-id="00a12-134">The most common pattern for relationships is to have navigation properties defined on both ends of the relationship and a foreign key property defined in the dependent entity class.</span></span>

* <span data-ttu-id="00a12-135">При обнаружении пара свойств навигации между двумя типами они будут настроены как свойства навигации обратный той же связи.</span><span class="sxs-lookup"><span data-stu-id="00a12-135">If a pair of navigation properties is found between two types, then they will be configured as inverse navigation properties of the same relationship.</span></span>

* <span data-ttu-id="00a12-136">Если зависимая сущность содержит свойство с именем `<primary key property name>`, `<navigation property name><primary key property name>`, или `<principal entity name><primary key property name>` то он будет настроен в качестве внешнего ключа.</span><span class="sxs-lookup"><span data-stu-id="00a12-136">If the dependent entity contains a property named `<primary key property name>`, `<navigation property name><primary key property name>`, or `<principal entity name><primary key property name>` then it will be configured as the foreign key.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Samples/Relationships/Full.cs?name=Entities&highlight=6,15,16)]

> [!WARNING]  
> <span data-ttu-id="00a12-137">Если есть несколько свойств навигации, определенных между двумя типами (то есть несколько уникальных пара переходов, которые указывают друг с другом), затем связи не будут созданы по соглашению, и необходимо будет вручную настроить их для идентификации как Пара свойств навигации вверх.</span><span class="sxs-lookup"><span data-stu-id="00a12-137">If there are multiple navigation properties defined between two types (that is, more than one distinct pair of navigations that point to each other), then no relationships will be created by convention and you will need to manually configure them to identify how the navigation properties pair up.</span></span>

### <a name="no-foreign-key-property"></a><span data-ttu-id="00a12-138">Нет свойство внешнего ключа</span><span class="sxs-lookup"><span data-stu-id="00a12-138">No Foreign Key Property</span></span>

<span data-ttu-id="00a12-139">Рекомендуется иметь свойство внешнего ключа, определенные в классе зависимой сущности, однако он не требуется.</span><span class="sxs-lookup"><span data-stu-id="00a12-139">While it is recommended to have a foreign key property defined in the dependent entity class, it is not required.</span></span> <span data-ttu-id="00a12-140">При обнаружении не свойство внешнего ключа, свойство внешнего ключа тени познакомитесь с именем `<navigation property name><principal key property name>` (см. в разделе [замещения свойств](shadow-properties.md) Дополнительные сведения).</span><span class="sxs-lookup"><span data-stu-id="00a12-140">If no foreign key property is found, a shadow foreign key property will be introduced with the name `<navigation property name><principal key property name>` (see [Shadow Properties](shadow-properties.md) for more information).</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Samples/Relationships/NoForeignKey.cs?name=Entities&highlight=6,15)]

### <a name="single-navigation-property"></a><span data-ttu-id="00a12-141">Одного свойства навигации</span><span class="sxs-lookup"><span data-stu-id="00a12-141">Single Navigation Property</span></span>

<span data-ttu-id="00a12-142">Включая только одно свойство навигации (не обратное навигации и не свойство внешнего ключа) достаточно, чтобы иметь связи, определенной по соглашению.</span><span class="sxs-lookup"><span data-stu-id="00a12-142">Including just one navigation property (no inverse navigation, and no foreign key property) is enough to have a relationship defined by convention.</span></span> <span data-ttu-id="00a12-143">Также возможно одного свойства навигации и свойство внешнего ключа.</span><span class="sxs-lookup"><span data-stu-id="00a12-143">You can also have a single navigation property and a foreign key property.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Samples/Relationships/OneNavigation.cs?name=Entities&highlight=6)]

### <a name="cascade-delete"></a><span data-ttu-id="00a12-144">Каскадное удаление</span><span class="sxs-lookup"><span data-stu-id="00a12-144">Cascade Delete</span></span>

<span data-ttu-id="00a12-145">По соглашению каскадное удаление будет присвоено *Cascade* для необходимые связи и *ClientSetNull* необязательно связей.</span><span class="sxs-lookup"><span data-stu-id="00a12-145">By convention, cascade delete will be set to *Cascade* for required relationships and *ClientSetNull* for optional relationships.</span></span> <span data-ttu-id="00a12-146">*CASCADE* означает, что зависимые сущности также удаляются.</span><span class="sxs-lookup"><span data-stu-id="00a12-146">*Cascade* means dependent entities are also deleted.</span></span> <span data-ttu-id="00a12-147">*ClientSetNull* означает, что зависимые сущности, которые не были загружены в память останется без изменений и должны быть вручную удалены или обновлены с допустимым основной сущности.</span><span class="sxs-lookup"><span data-stu-id="00a12-147">*ClientSetNull* means that dependent entities that are not loaded into memory will remain unchanged and must be manually deleted, or updated to point to a valid principal entity.</span></span> <span data-ttu-id="00a12-148">Для сущностей, которые загружаются в память EF Core попытается установить для свойств внешнего ключа значение null.</span><span class="sxs-lookup"><span data-stu-id="00a12-148">For entities that are loaded into memory, EF Core will attempt to set the foreign key properties to null.</span></span>

<span data-ttu-id="00a12-149">См. в разделе [обязательных и необязательных связи](#required-and-optional-relationships) раздел для разницы между обязательных и необязательных связи.</span><span class="sxs-lookup"><span data-stu-id="00a12-149">See the [Required and Optional Relationships](#required-and-optional-relationships) section for the difference between required and optional relationships.</span></span>

<span data-ttu-id="00a12-150">См. в разделе [каскадное удаление](../saving/cascade-delete.md) для поведения и значения по умолчанию, используемый в соответствии с соглашением об удалении Дополнительные сведения о различных.</span><span class="sxs-lookup"><span data-stu-id="00a12-150">See [Cascade Delete](../saving/cascade-delete.md) for more details about the different delete behaviors and the defaults used by convention.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="00a12-151">Заметки к данным</span><span class="sxs-lookup"><span data-stu-id="00a12-151">Data Annotations</span></span>

<span data-ttu-id="00a12-152">Существуют два заметок к данным, которые могут использоваться для настройки связей, `[ForeignKey]` и `[InverseProperty]`.</span><span class="sxs-lookup"><span data-stu-id="00a12-152">There are two data annotations that can be used to configure relationships, `[ForeignKey]` and `[InverseProperty]`.</span></span> <span data-ttu-id="00a12-153">Они доступны в `System.ComponentModel.DataAnnotations.Schema` пространства имен.</span><span class="sxs-lookup"><span data-stu-id="00a12-153">These are available in the `System.ComponentModel.DataAnnotations.Schema` namespace.</span></span>

### <a name="foreignkey"></a><span data-ttu-id="00a12-154">[ForeignKey]</span><span class="sxs-lookup"><span data-stu-id="00a12-154">[ForeignKey]</span></span>

<span data-ttu-id="00a12-155">Заметки данных можно использовать для настройки, какое свойство следует использовать как свойство внешнего ключа для конкретной связи.</span><span class="sxs-lookup"><span data-stu-id="00a12-155">You can use the Data Annotations to configure which property should be used as the foreign key property for a given relationship.</span></span> <span data-ttu-id="00a12-156">Обычно это делается свойства внешнего ключа не обнаружен по соглашению.</span><span class="sxs-lookup"><span data-stu-id="00a12-156">This is typically done when the foreign key property is not discovered by convention.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Samples/Relationships/ForeignKey.cs?highlight=30)]

> [!TIP]  
> <span data-ttu-id="00a12-157">`[ForeignKey]` Заметки можно поместить в свойство навигации, либо в связи.</span><span class="sxs-lookup"><span data-stu-id="00a12-157">The `[ForeignKey]` annotation can be placed on either navigation property in the relationship.</span></span> <span data-ttu-id="00a12-158">Он не должен перейти в свойстве навигации в классе зависимой сущности.</span><span class="sxs-lookup"><span data-stu-id="00a12-158">It does not need to go on the navigation property in the dependent entity class.</span></span>

### <a name="inverseproperty"></a><span data-ttu-id="00a12-159">[InverseProperty]</span><span class="sxs-lookup"><span data-stu-id="00a12-159">[InverseProperty]</span></span>

<span data-ttu-id="00a12-160">Заметки данных можно использовать для настройки, как сопоставить свойства навигации в зависимой и основной сущности.</span><span class="sxs-lookup"><span data-stu-id="00a12-160">You can use the Data Annotations to configure how navigation properties on the dependent and principal entities pair up.</span></span> <span data-ttu-id="00a12-161">Обычно это делается при наличии более чем одна пара свойств навигации между двумя типами сущностей.</span><span class="sxs-lookup"><span data-stu-id="00a12-161">This is typically done when there is more than one pair of navigation properties between two entity types.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Samples/Relationships/InverseProperty.cs?highlight=33,36)]

## <a name="fluent-api"></a><span data-ttu-id="00a12-162">Текучий API</span><span class="sxs-lookup"><span data-stu-id="00a12-162">Fluent API</span></span>

<span data-ttu-id="00a12-163">Чтобы настроить отношение в Fluent API, начните с определение свойства навигации, составляющих связи.</span><span class="sxs-lookup"><span data-stu-id="00a12-163">To configure a relationship in the Fluent API, you start by identifying the navigation properties that make up the relationship.</span></span> <span data-ttu-id="00a12-164">`HasOne` или `HasMany` определяет свойство навигации типа сущности, в начале конфигурации.</span><span class="sxs-lookup"><span data-stu-id="00a12-164">`HasOne` or `HasMany` identifies the navigation property on the entity type you are beginning the configuration on.</span></span> <span data-ttu-id="00a12-165">Затем вы цепочку вызов `WithOne` или `WithMany` для идентификации обратной навигации.</span><span class="sxs-lookup"><span data-stu-id="00a12-165">You then chain a call to `WithOne` or `WithMany` to identify the inverse navigation.</span></span> <span data-ttu-id="00a12-166">`HasOne`/`WithOne` используются для свойства навигации по ссылке и `HasMany` / `WithMany` используются для свойства навигации по коллекции.</span><span class="sxs-lookup"><span data-stu-id="00a12-166">`HasOne`/`WithOne` are used for reference navigation properties and `HasMany`/`WithMany` are used for collection navigation properties.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Relationships/NoForeignKey.cs?highlight=14-16)]

### <a name="single-navigation-property"></a><span data-ttu-id="00a12-167">Одного свойства навигации</span><span class="sxs-lookup"><span data-stu-id="00a12-167">Single Navigation Property</span></span>

<span data-ttu-id="00a12-168">Если имеется только одно свойство навигации, то существуют перегрузки без параметров `WithOne` и `WithMany`.</span><span class="sxs-lookup"><span data-stu-id="00a12-168">If you only have one navigation property then there are parameterless overloads of `WithOne` and `WithMany`.</span></span> <span data-ttu-id="00a12-169">Это означает, что имеется по сути, является ссылкой или коллекцией на другом конце связи, но нет свойства навигации, включенные в классе сущности.</span><span class="sxs-lookup"><span data-stu-id="00a12-169">This indicates that there is conceptually a reference or collection on the other end of the relationship, but there is no navigation property included in the entity class.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Relationships/OneNavigation.cs?highlight=14-16)]

### <a name="foreign-key"></a><span data-ttu-id="00a12-170">Внешний ключ</span><span class="sxs-lookup"><span data-stu-id="00a12-170">Foreign Key</span></span>

<span data-ttu-id="00a12-171">Fluent API можно использовать для настройки того, какое свойство следует использовать как свойство внешнего ключа для конкретной связи.</span><span class="sxs-lookup"><span data-stu-id="00a12-171">You can use the Fluent API to configure which property should be used as the foreign key property for a given relationship.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Relationships/ForeignKey.cs?highlight=17)]

<span data-ttu-id="00a12-172">В следующем примере кода показаны способы настройки составного внешнего ключа.</span><span class="sxs-lookup"><span data-stu-id="00a12-172">The following code listing shows how to configure a composite foreign key.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Relationships/CompositeForeignKey.cs?highlight=20)]

<span data-ttu-id="00a12-173">Можно использовать перегрузку строка `HasForeignKey(...)` для настройки теневого свойство как внешний ключ (см. в разделе [замещения свойств](shadow-properties.md) Дополнительные сведения).</span><span class="sxs-lookup"><span data-stu-id="00a12-173">You can use the string overload of `HasForeignKey(...)` to configure a shadow property as a foreign key (see [Shadow Properties](shadow-properties.md) for more information).</span></span> <span data-ttu-id="00a12-174">Мы рекомендуем явным образом добавить в модель теневые свойства перед его использованием в качестве внешнего ключа (как показано ниже).</span><span class="sxs-lookup"><span data-stu-id="00a12-174">We recommend explicitly adding the shadow property to the model before using it as a foreign key (as shown below).</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Relationships/ShadowForeignKey.cs#Sample)]

### <a name="principal-key"></a><span data-ttu-id="00a12-175">Ключ субъекта-службы</span><span class="sxs-lookup"><span data-stu-id="00a12-175">Principal Key</span></span>

<span data-ttu-id="00a12-176">Если требуется, чтобы внешний ключ для ссылки на свойство, отличный от первичного ключа, можно использовать Fluent API для настройки свойство основного ключа для связи.</span><span class="sxs-lookup"><span data-stu-id="00a12-176">If you want the foreign key to reference a property other than the primary key, you can use the Fluent API to configure the principal key property for the relationship.</span></span> <span data-ttu-id="00a12-177">Свойство, настроенному в качестве основного ключа будет автоматически иметь установки в качестве альтернативного ключа (см. в разделе [альтернативные ключи](alternate-keys.md) Дополнительные сведения).</span><span class="sxs-lookup"><span data-stu-id="00a12-177">The property that you configure as the principal key will automatically be setup as an alternate key (see [Alternate Keys](alternate-keys.md) for more information).</span></span>

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

<span data-ttu-id="00a12-178">В следующем коде демонстрируется настройка составного ключа субъекта.</span><span class="sxs-lookup"><span data-stu-id="00a12-178">The following code listing shows how to configure a composite principal key.</span></span>

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
> <span data-ttu-id="00a12-179">Порядок, в котором указываются свойства основного ключа должен соответствовать порядку, в котором они указаны для внешнего ключа.</span><span class="sxs-lookup"><span data-stu-id="00a12-179">The order in which you specify principal key properties must match the order in which they are specified for the foreign key.</span></span>

### <a name="required-and-optional-relationships"></a><span data-ttu-id="00a12-180">Обязательные и необязательные связей</span><span class="sxs-lookup"><span data-stu-id="00a12-180">Required and Optional Relationships</span></span>

<span data-ttu-id="00a12-181">Fluent API позволяет настроить, является ли связь обязательными или необязательными.</span><span class="sxs-lookup"><span data-stu-id="00a12-181">You can use the Fluent API to configure whether the relationship is required or optional.</span></span> <span data-ttu-id="00a12-182">В конечном счете определяет, является ли свойство внешнего ключа обязательными или необязательными.</span><span class="sxs-lookup"><span data-stu-id="00a12-182">Ultimately this controls whether the foreign key property is required or optional.</span></span> <span data-ttu-id="00a12-183">Это может пригодиться при использовании внешнего ключа состояние тени.</span><span class="sxs-lookup"><span data-stu-id="00a12-183">This is most useful when you are using a shadow state foreign key.</span></span> <span data-ttu-id="00a12-184">Если у вас есть свойство внешнего ключа в классе сущностей, а затем requiredness связи определяется в зависимости от свойства внешнего ключа обязательными или нет (см. в разделе [обязательные и необязательные свойства](required-optional.md) для получения дополнительных сведений сведения о).</span><span class="sxs-lookup"><span data-stu-id="00a12-184">If you have a foreign key property in your entity class then the requiredness of the relationship is determined based on whether the foreign key property is required or optional (see [Required and Optional properties](required-optional.md) for more information).</span></span>

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

### <a name="cascade-delete"></a><span data-ttu-id="00a12-185">Каскадное удаление</span><span class="sxs-lookup"><span data-stu-id="00a12-185">Cascade Delete</span></span>

<span data-ttu-id="00a12-186">Fluent API можно использовать для настройки поведения cascade delete для конкретной связи явно.</span><span class="sxs-lookup"><span data-stu-id="00a12-186">You can use the Fluent API to configure the cascade delete behavior for a given relationship explicitly.</span></span>

<span data-ttu-id="00a12-187">См. в разделе [каскадное удаление](../saving/cascade-delete.md) на разделе Сохранение данных подробное описание каждого параметра.</span><span class="sxs-lookup"><span data-stu-id="00a12-187">See [Cascade Delete](../saving/cascade-delete.md) on the Saving Data section for a detailed discussion of each option.</span></span>

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

## <a name="other-relationship-patterns"></a><span data-ttu-id="00a12-188">Другие шаблоны связи</span><span class="sxs-lookup"><span data-stu-id="00a12-188">Other Relationship Patterns</span></span>

### <a name="one-to-one"></a><span data-ttu-id="00a12-189">Один к одному</span><span class="sxs-lookup"><span data-stu-id="00a12-189">One-to-one</span></span>

<span data-ttu-id="00a12-190">Связи один к одному имеют свойства навигации ссылки на обеих сторонах.</span><span class="sxs-lookup"><span data-stu-id="00a12-190">One to one relationships have a reference navigation property on both sides.</span></span> <span data-ttu-id="00a12-191">Они следуют тем же правилам как отношения один ко многим, но появилось в свойстве внешнего ключа, чтобы убедиться, что каждому участнику связан только один зависит от уникального индекса.</span><span class="sxs-lookup"><span data-stu-id="00a12-191">They follow the same conventions as one-to-many relationships, but a unique index is introduced on the foreign key property to ensure only one dependent is related to each principal.</span></span>

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
> <span data-ttu-id="00a12-192">EF выберет одной сущности к зависимым его возможностями для обнаружения свойство внешнего ключа.</span><span class="sxs-lookup"><span data-stu-id="00a12-192">EF will choose one of the entities to be the dependent based on its ability to detect a foreign key property.</span></span> <span data-ttu-id="00a12-193">Если выбрать неправильный сущности в качестве зависимого, Fluent API можно использовать для устранения этой проблемы.</span><span class="sxs-lookup"><span data-stu-id="00a12-193">If the wrong entity is chosen as the dependent, you can use the Fluent API to correct this.</span></span>

<span data-ttu-id="00a12-194">Настраивая связь с Fluent API, используйте `HasOne` и `WithOne` методы.</span><span class="sxs-lookup"><span data-stu-id="00a12-194">When configuring the relationship with the Fluent API, you use the `HasOne` and `WithOne` methods.</span></span>

<span data-ttu-id="00a12-195">При настройке внешнего ключа, необходимо указать тип зависимой сущности — Обратите внимание, что универсальный параметр, переданный `HasForeignKey` в списке ниже.</span><span class="sxs-lookup"><span data-stu-id="00a12-195">When configuring the foreign key you need to specify the dependent entity type - notice the generic parameter provided to `HasForeignKey` in the listing below.</span></span> <span data-ttu-id="00a12-196">В отношении один ко многим становится ясно, что зависит от сущности с навигации ссылки на и с коллекции является участником.</span><span class="sxs-lookup"><span data-stu-id="00a12-196">In a one-to-many relationship it is clear that the entity with the reference navigation is the dependent and the one with the collection is the principal.</span></span> <span data-ttu-id="00a12-197">Однако это не является таким образом, в взаимно-однозначной связи — таким образом необходимость явно определить его.</span><span class="sxs-lookup"><span data-stu-id="00a12-197">But this is not so in a one-to-one relationship - hence the need to explicitly define it.</span></span>

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

### <a name="many-to-many"></a><span data-ttu-id="00a12-198">Многие ко многим</span><span class="sxs-lookup"><span data-stu-id="00a12-198">Many-to-many</span></span>

<span data-ttu-id="00a12-199">Связи многие ко многим без класса сущности для представления таблицы соединения еще не поддерживаются.</span><span class="sxs-lookup"><span data-stu-id="00a12-199">Many-to-many relationships without an entity class to represent the join table are not yet supported.</span></span> <span data-ttu-id="00a12-200">Тем не менее вы можете представить связь многие ко многим, включив класс сущности таблицы соединения и две отдельные связи один ко многим сопоставления.</span><span class="sxs-lookup"><span data-stu-id="00a12-200">However, you can represent a many-to-many relationship by including an entity class for the join table and mapping two separate one-to-many relationships.</span></span>

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
