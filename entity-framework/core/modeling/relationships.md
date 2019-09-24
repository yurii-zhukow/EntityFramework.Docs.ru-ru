---
title: Связи — EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 0ff736a3-f1b0-4b58-a49c-4a7094bd6935
uid: core/modeling/relationships
ms.openlocfilehash: 1e9c62bec47263ef452c7ac425a0bb446f9371d8
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/23/2019
ms.locfileid: "71197649"
---
# <a name="relationships"></a><span data-ttu-id="a05ff-102">Отношения</span><span class="sxs-lookup"><span data-stu-id="a05ff-102">Relationships</span></span>

<span data-ttu-id="a05ff-103">Связь определяет, как две сущности связаны друг с другом.</span><span class="sxs-lookup"><span data-stu-id="a05ff-103">A relationship defines how two entities relate to each other.</span></span> <span data-ttu-id="a05ff-104">В реляционной базе данных это представление представляется ограничением внешнего ключа.</span><span class="sxs-lookup"><span data-stu-id="a05ff-104">In a relational database, this is represented by a foreign key constraint.</span></span>

> [!NOTE]  
> <span data-ttu-id="a05ff-105">Большинство примеров в этой статье используют связь «один ко многим» для демонстрации концепций.</span><span class="sxs-lookup"><span data-stu-id="a05ff-105">Most of the samples in this article use a one-to-many relationship to demonstrate concepts.</span></span> <span data-ttu-id="a05ff-106">Примеры связей "один к одному" и "многие ко многим" см. в разделе [другие шаблоны связей](#other-relationship-patterns) в конце статьи.</span><span class="sxs-lookup"><span data-stu-id="a05ff-106">For examples of one-to-one and many-to-many relationships see the [Other Relationship Patterns](#other-relationship-patterns) section at the end of the article.</span></span>

## <a name="definition-of-terms"></a><span data-ttu-id="a05ff-107">Определение терминов</span><span class="sxs-lookup"><span data-stu-id="a05ff-107">Definition of Terms</span></span>

<span data-ttu-id="a05ff-108">Существует ряд терминов, используемых для описания связей.</span><span class="sxs-lookup"><span data-stu-id="a05ff-108">There are a number of terms used to describe relationships</span></span>

* <span data-ttu-id="a05ff-109">**Зависимая сущность:** Это сущность, содержащая свойства внешнего ключа.</span><span class="sxs-lookup"><span data-stu-id="a05ff-109">**Dependent entity:** This is the entity that contains the foreign key property(s).</span></span> <span data-ttu-id="a05ff-110">Иногда называется "дочерним" отношением.</span><span class="sxs-lookup"><span data-stu-id="a05ff-110">Sometimes referred to as the 'child' of the relationship.</span></span>

* <span data-ttu-id="a05ff-111">**Сущность субъекта:** Это сущность, содержащая свойства первичного или альтернативного ключа.</span><span class="sxs-lookup"><span data-stu-id="a05ff-111">**Principal entity:** This is the entity that contains the primary/alternate key property(s).</span></span> <span data-ttu-id="a05ff-112">Иногда называется "родителем" связи.</span><span class="sxs-lookup"><span data-stu-id="a05ff-112">Sometimes referred to as the 'parent' of the relationship.</span></span>

* <span data-ttu-id="a05ff-113">**Внешний ключ:** Свойства в зависимой сущности, используемые для хранения значений свойства ключа участника, с которым связана сущность.</span><span class="sxs-lookup"><span data-stu-id="a05ff-113">**Foreign key:** The property(s) in the dependent entity that is used to store the values of the principal key property that the entity is related to.</span></span>

* <span data-ttu-id="a05ff-114">**Основной ключ:** Свойства, которые однозначно идентифицируют сущность Principal.</span><span class="sxs-lookup"><span data-stu-id="a05ff-114">**Principal key:** The property(s) that uniquely identifies the principal entity.</span></span> <span data-ttu-id="a05ff-115">Это может быть первичный ключ или альтернативный ключ.</span><span class="sxs-lookup"><span data-stu-id="a05ff-115">This may be the primary key or an alternate key.</span></span>

* <span data-ttu-id="a05ff-116">**Свойство навигации:** Свойство, определенное для основной и (или) зависимой сущности, содержащей ссылки на связанные сущности.</span><span class="sxs-lookup"><span data-stu-id="a05ff-116">**Navigation property:** A property defined on the principal and/or dependent entity that contains a reference(s) to the related entity(s).</span></span>

  * <span data-ttu-id="a05ff-117">**Свойство навигации коллекции:** Свойство навигации, содержащее ссылки на множество связанных сущностей.</span><span class="sxs-lookup"><span data-stu-id="a05ff-117">**Collection navigation property:** A navigation property that contains references to many related entities.</span></span>

  * <span data-ttu-id="a05ff-118">**Свойство навигации по ссылке:** Свойство навигации, содержащее ссылку на одну связанную сущность.</span><span class="sxs-lookup"><span data-stu-id="a05ff-118">**Reference navigation property:** A navigation property that holds a reference to a single related entity.</span></span>

  * <span data-ttu-id="a05ff-119">**Обратное свойство навигации:** При обсуждении определенного свойства навигации этот термин относится к свойству навигации на другом конце связи.</span><span class="sxs-lookup"><span data-stu-id="a05ff-119">**Inverse navigation property:** When discussing a particular navigation property, this term refers to the navigation property on the other end of the relationship.</span></span>

<span data-ttu-id="a05ff-120">В следующем листинге кода показана связь «один ко многим» `Blog` между и`Post`</span><span class="sxs-lookup"><span data-stu-id="a05ff-120">The following code listing shows a one-to-many relationship between `Blog` and `Post`</span></span>

* <span data-ttu-id="a05ff-121">`Post`является зависимой сущностью</span><span class="sxs-lookup"><span data-stu-id="a05ff-121">`Post` is the dependent entity</span></span>

* <span data-ttu-id="a05ff-122">`Blog`является основной сущностью</span><span class="sxs-lookup"><span data-stu-id="a05ff-122">`Blog` is the principal entity</span></span>

* <span data-ttu-id="a05ff-123">`Post.BlogId`является внешним ключом</span><span class="sxs-lookup"><span data-stu-id="a05ff-123">`Post.BlogId` is the foreign key</span></span>

* <span data-ttu-id="a05ff-124">`Blog.BlogId`является основным ключом (в данном случае это первичный ключ, а не альтернативный ключ);</span><span class="sxs-lookup"><span data-stu-id="a05ff-124">`Blog.BlogId` is the principal key (in this case it is a primary key rather than an alternate key)</span></span>

* <span data-ttu-id="a05ff-125">`Post.Blog`является свойством навигации по ссылке</span><span class="sxs-lookup"><span data-stu-id="a05ff-125">`Post.Blog` is a reference navigation property</span></span>

* <span data-ttu-id="a05ff-126">`Blog.Posts`— Это свойство навигации коллекции</span><span class="sxs-lookup"><span data-stu-id="a05ff-126">`Blog.Posts` is a collection navigation property</span></span>

* <span data-ttu-id="a05ff-127">`Post.Blog`Свойство обратной навигации `Blog.Posts` (и наоборот)</span><span class="sxs-lookup"><span data-stu-id="a05ff-127">`Post.Blog` is the inverse navigation property of `Blog.Posts` (and vice versa)</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Relationships/Full.cs#Entities)]

## <a name="conventions"></a><span data-ttu-id="a05ff-128">Соглашения</span><span class="sxs-lookup"><span data-stu-id="a05ff-128">Conventions</span></span>

<span data-ttu-id="a05ff-129">По соглашению, связь будет создана при обнаружении свойства навигации для типа.</span><span class="sxs-lookup"><span data-stu-id="a05ff-129">By convention, a relationship will be created when there is a navigation property discovered on a type.</span></span> <span data-ttu-id="a05ff-130">Свойство считается свойством навигации, если тип, на который он указывает, не может быть сопоставлен с текущим поставщиком базы данных как скалярный тип.</span><span class="sxs-lookup"><span data-stu-id="a05ff-130">A property is considered a navigation property if the type it points to can not be mapped as a scalar type by the current database provider.</span></span>

> [!NOTE]  
> <span data-ttu-id="a05ff-131">Связи, обнаруженные соглашением, всегда будут указывать первичный ключ основной сущности.</span><span class="sxs-lookup"><span data-stu-id="a05ff-131">Relationships that are discovered by convention will always target the primary key of the principal entity.</span></span> <span data-ttu-id="a05ff-132">Чтобы выбрать альтернативный ключ, необходимо выполнить дополнительную настройку с помощью API Fluent.</span><span class="sxs-lookup"><span data-stu-id="a05ff-132">To target an alternate key, additional configuration must be performed using the Fluent API.</span></span>

### <a name="fully-defined-relationships"></a><span data-ttu-id="a05ff-133">Полностью определенные связи</span><span class="sxs-lookup"><span data-stu-id="a05ff-133">Fully Defined Relationships</span></span>

<span data-ttu-id="a05ff-134">Наиболее распространенным шаблоном для связей является наличие свойств навигации, определенных на обоих концах связи, и свойства внешнего ключа, определенного в зависимом классе сущности.</span><span class="sxs-lookup"><span data-stu-id="a05ff-134">The most common pattern for relationships is to have navigation properties defined on both ends of the relationship and a foreign key property defined in the dependent entity class.</span></span>

* <span data-ttu-id="a05ff-135">Если между двумя типами обнаружена пара свойств навигации, они будут настроены как обратные свойства навигации одной и той же связи.</span><span class="sxs-lookup"><span data-stu-id="a05ff-135">If a pair of navigation properties is found between two types, then they will be configured as inverse navigation properties of the same relationship.</span></span>

* <span data-ttu-id="a05ff-136">Если зависимая сущность содержит свойство с `<primary key property name>`именем `<navigation property name><primary key property name>`, или `<principal entity name><primary key property name>` , то оно будет настроено как внешний ключ.</span><span class="sxs-lookup"><span data-stu-id="a05ff-136">If the dependent entity contains a property named `<primary key property name>`, `<navigation property name><primary key property name>`, or `<principal entity name><primary key property name>` then it will be configured as the foreign key.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Relationships/Full.cs?name=Entities&highlight=6,15,16)]

> [!WARNING]  
> <span data-ttu-id="a05ff-137">Если существует несколько свойств навигации, определенных между двумя типами (то есть более чем одной отдельной парой переходов, которая указывает друг на друга), связи не создаются по соглашению, и вам потребуется вручную настроить их, чтобы определить, как Связывание свойств навигации.</span><span class="sxs-lookup"><span data-stu-id="a05ff-137">If there are multiple navigation properties defined between two types (that is, more than one distinct pair of navigations that point to each other), then no relationships will be created by convention and you will need to manually configure them to identify how the navigation properties pair up.</span></span>

### <a name="no-foreign-key-property"></a><span data-ttu-id="a05ff-138">Нет свойства внешнего ключа</span><span class="sxs-lookup"><span data-stu-id="a05ff-138">No Foreign Key Property</span></span>

<span data-ttu-id="a05ff-139">Хотя рекомендуется использовать свойство внешнего ключа, определенное в зависимом классе сущности, оно не является обязательным.</span><span class="sxs-lookup"><span data-stu-id="a05ff-139">While it is recommended to have a foreign key property defined in the dependent entity class, it is not required.</span></span> <span data-ttu-id="a05ff-140">Если свойство внешнего ключа не найдено, свойство теневого внешнего ключа будет введено с именем `<navigation property name><principal key property name>` (Дополнительные сведения см. в разделе [Свойства тени](shadow-properties.md) ).</span><span class="sxs-lookup"><span data-stu-id="a05ff-140">If no foreign key property is found, a shadow foreign key property will be introduced with the name `<navigation property name><principal key property name>` (see [Shadow Properties](shadow-properties.md) for more information).</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Relationships/NoForeignKey.cs?name=Entities&highlight=6,15)]

### <a name="single-navigation-property"></a><span data-ttu-id="a05ff-141">Одно свойство навигации</span><span class="sxs-lookup"><span data-stu-id="a05ff-141">Single Navigation Property</span></span>

<span data-ttu-id="a05ff-142">Включение только одного свойства навигации (без обратной навигации и свойства внешнего ключа) достаточно для того, чтобы иметь связь, определенную по соглашению.</span><span class="sxs-lookup"><span data-stu-id="a05ff-142">Including just one navigation property (no inverse navigation, and no foreign key property) is enough to have a relationship defined by convention.</span></span> <span data-ttu-id="a05ff-143">Можно также иметь одно свойство навигации и внешнее ключевое свойство.</span><span class="sxs-lookup"><span data-stu-id="a05ff-143">You can also have a single navigation property and a foreign key property.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Relationships/OneNavigation.cs?name=Entities&highlight=6)]

### <a name="cascade-delete"></a><span data-ttu-id="a05ff-144">Каскадное удаление</span><span class="sxs-lookup"><span data-stu-id="a05ff-144">Cascade Delete</span></span>

<span data-ttu-id="a05ff-145">По соглашению каскадное удаление будет установлено в *CASCADE* для требуемых связей и *клиентсетнулл* для необязательных связей.</span><span class="sxs-lookup"><span data-stu-id="a05ff-145">By convention, cascade delete will be set to *Cascade* for required relationships and *ClientSetNull* for optional relationships.</span></span> <span data-ttu-id="a05ff-146">*CASCADE* означает также удаление зависимых сущностей.</span><span class="sxs-lookup"><span data-stu-id="a05ff-146">*Cascade* means dependent entities are also deleted.</span></span> <span data-ttu-id="a05ff-147">*Клиентсетнулл* означает, что зависимые сущности, которые не загружены в память, останутся неизменными и должны быть удалены вручную или обновлены, чтобы они указывали на действительную сущность Principal.</span><span class="sxs-lookup"><span data-stu-id="a05ff-147">*ClientSetNull* means that dependent entities that are not loaded into memory will remain unchanged and must be manually deleted, or updated to point to a valid principal entity.</span></span> <span data-ttu-id="a05ff-148">Для сущностей, загруженных в память, EF Core попытается установить свойства внешнего ключа в значение null.</span><span class="sxs-lookup"><span data-stu-id="a05ff-148">For entities that are loaded into memory, EF Core will attempt to set the foreign key properties to null.</span></span>

<span data-ttu-id="a05ff-149">Различия между обязательными и дополнительными связями см. в разделе [обязательные и дополнительные связи](#required-and-optional-relationships) .</span><span class="sxs-lookup"><span data-stu-id="a05ff-149">See the [Required and Optional Relationships](#required-and-optional-relationships) section for the difference between required and optional relationships.</span></span>

<span data-ttu-id="a05ff-150">Дополнительные сведения о различных поведениях при удалении и значения по умолчанию, используемые по соглашению, см. в разделе [каскадное удаление](../saving/cascade-delete.md) .</span><span class="sxs-lookup"><span data-stu-id="a05ff-150">See [Cascade Delete](../saving/cascade-delete.md) for more details about the different delete behaviors and the defaults used by convention.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="a05ff-151">Заметки к данным</span><span class="sxs-lookup"><span data-stu-id="a05ff-151">Data Annotations</span></span>

<span data-ttu-id="a05ff-152">Существуют две заметки к данным, которые можно использовать для настройки связей, `[ForeignKey]` и `[InverseProperty]`.</span><span class="sxs-lookup"><span data-stu-id="a05ff-152">There are two data annotations that can be used to configure relationships, `[ForeignKey]` and `[InverseProperty]`.</span></span> <span data-ttu-id="a05ff-153">Они доступны в `System.ComponentModel.DataAnnotations.Schema` пространстве имен.</span><span class="sxs-lookup"><span data-stu-id="a05ff-153">These are available in the `System.ComponentModel.DataAnnotations.Schema` namespace.</span></span>

### <a name="foreignkey"></a><span data-ttu-id="a05ff-154">[Фореигнкэй]</span><span class="sxs-lookup"><span data-stu-id="a05ff-154">[ForeignKey]</span></span>

<span data-ttu-id="a05ff-155">Заметки к данным можно использовать для настройки того, какое свойство должно использоваться в качестве свойства внешнего ключа для данной связи.</span><span class="sxs-lookup"><span data-stu-id="a05ff-155">You can use the Data Annotations to configure which property should be used as the foreign key property for a given relationship.</span></span> <span data-ttu-id="a05ff-156">Обычно это делается, когда свойство внешнего ключа не обнаруживается по соглашению.</span><span class="sxs-lookup"><span data-stu-id="a05ff-156">This is typically done when the foreign key property is not discovered by convention.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Relationships/ForeignKey.cs?highlight=30)]

> [!TIP]  
> <span data-ttu-id="a05ff-157">`[ForeignKey]` Заметку можно поместить в любое свойство навигации в связи.</span><span class="sxs-lookup"><span data-stu-id="a05ff-157">The `[ForeignKey]` annotation can be placed on either navigation property in the relationship.</span></span> <span data-ttu-id="a05ff-158">Не требуется переходить к свойству навигации в классе зависимой сущности.</span><span class="sxs-lookup"><span data-stu-id="a05ff-158">It does not need to go on the navigation property in the dependent entity class.</span></span>

### <a name="inverseproperty"></a><span data-ttu-id="a05ff-159">[Инверсепроперти]</span><span class="sxs-lookup"><span data-stu-id="a05ff-159">[InverseProperty]</span></span>

<span data-ttu-id="a05ff-160">Заметки к данным можно использовать для настройки способа связывания свойств навигации в зависимых и субъектах сущностей.</span><span class="sxs-lookup"><span data-stu-id="a05ff-160">You can use the Data Annotations to configure how navigation properties on the dependent and principal entities pair up.</span></span> <span data-ttu-id="a05ff-161">Обычно это делается при наличии более одной пары свойств навигации между двумя типами сущностей.</span><span class="sxs-lookup"><span data-stu-id="a05ff-161">This is typically done when there is more than one pair of navigation properties between two entity types.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Relationships/InverseProperty.cs?highlight=33,36)]

## <a name="fluent-api"></a><span data-ttu-id="a05ff-162">Текучий API</span><span class="sxs-lookup"><span data-stu-id="a05ff-162">Fluent API</span></span>

<span data-ttu-id="a05ff-163">Чтобы настроить связь в API Fluent, начните с определения свойств навигации, составляющих связь.</span><span class="sxs-lookup"><span data-stu-id="a05ff-163">To configure a relationship in the Fluent API, you start by identifying the navigation properties that make up the relationship.</span></span> <span data-ttu-id="a05ff-164">`HasOne`или `HasMany` определяет свойство навигации для типа сущности, на котором начинается настройка.</span><span class="sxs-lookup"><span data-stu-id="a05ff-164">`HasOne` or `HasMany` identifies the navigation property on the entity type you are beginning the configuration on.</span></span> <span data-ttu-id="a05ff-165">Затем вы позвоните в `WithOne` цепочку или `WithMany` для поиска обратной навигации.</span><span class="sxs-lookup"><span data-stu-id="a05ff-165">You then chain a call to `WithOne` or `WithMany` to identify the inverse navigation.</span></span> <span data-ttu-id="a05ff-166">`HasOne`/`WithOne`используются для свойств навигации по ссылке и `HasMany` / `WithMany` используются для свойств навигации по коллекциям.</span><span class="sxs-lookup"><span data-stu-id="a05ff-166">`HasOne`/`WithOne` are used for reference navigation properties and `HasMany`/`WithMany` are used for collection navigation properties.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/NoForeignKey.cs?highlight=14-16)]

### <a name="single-navigation-property"></a><span data-ttu-id="a05ff-167">Одно свойство навигации</span><span class="sxs-lookup"><span data-stu-id="a05ff-167">Single Navigation Property</span></span>

<span data-ttu-id="a05ff-168">Если у вас есть только одно свойство навигации, то существуют перегрузки `WithOne` без параметров и. `WithMany`</span><span class="sxs-lookup"><span data-stu-id="a05ff-168">If you only have one navigation property then there are parameterless overloads of `WithOne` and `WithMany`.</span></span> <span data-ttu-id="a05ff-169">Это указывает на то, что на другом конце связи имеется концептуальная ссылка или коллекция, но в классе сущностей не содержится свойство навигации.</span><span class="sxs-lookup"><span data-stu-id="a05ff-169">This indicates that there is conceptually a reference or collection on the other end of the relationship, but there is no navigation property included in the entity class.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/OneNavigation.cs?highlight=14-16)]

### <a name="foreign-key"></a><span data-ttu-id="a05ff-170">Внешний ключ</span><span class="sxs-lookup"><span data-stu-id="a05ff-170">Foreign Key</span></span>

<span data-ttu-id="a05ff-171">API-интерфейс Fluent можно использовать для настройки того, какое свойство должно использоваться в качестве свойства внешнего ключа для данной связи.</span><span class="sxs-lookup"><span data-stu-id="a05ff-171">You can use the Fluent API to configure which property should be used as the foreign key property for a given relationship.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/ForeignKey.cs?highlight=17)]

<span data-ttu-id="a05ff-172">В следующем примере кода показано, как настроить составной внешний ключ.</span><span class="sxs-lookup"><span data-stu-id="a05ff-172">The following code listing shows how to configure a composite foreign key.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/CompositeForeignKey.cs?highlight=20)]

<span data-ttu-id="a05ff-173">`HasForeignKey(...)` Чтобы настроить свойство теневого копирования в качестве внешнего ключа, можно использовать строку перегрузки в. Дополнительные сведения см. в разделе [Свойства тени](shadow-properties.md) .</span><span class="sxs-lookup"><span data-stu-id="a05ff-173">You can use the string overload of `HasForeignKey(...)` to configure a shadow property as a foreign key (see [Shadow Properties](shadow-properties.md) for more information).</span></span> <span data-ttu-id="a05ff-174">Рекомендуется явно добавить свойство Shadow в модель, прежде чем использовать ее как внешний ключ (как показано ниже).</span><span class="sxs-lookup"><span data-stu-id="a05ff-174">We recommend explicitly adding the shadow property to the model before using it as a foreign key (as shown below).</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/ShadowForeignKey.cs#Sample)]

### <a name="without-navigation-property"></a><span data-ttu-id="a05ff-175">Без свойства навигации</span><span class="sxs-lookup"><span data-stu-id="a05ff-175">Without Navigation Property</span></span>

<span data-ttu-id="a05ff-176">Указывать свойство навигации не обязательно.</span><span class="sxs-lookup"><span data-stu-id="a05ff-176">You don't necessarily need to provide a navigation property.</span></span> <span data-ttu-id="a05ff-177">Можно просто предоставить внешний ключ на одной стороне связи.</span><span class="sxs-lookup"><span data-stu-id="a05ff-177">You can simply provide a Foreign Key on one side of the relationship.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/NoNavigation.cs?highlight=14-17)]

### <a name="principal-key"></a><span data-ttu-id="a05ff-178">Основной ключ</span><span class="sxs-lookup"><span data-stu-id="a05ff-178">Principal Key</span></span>

<span data-ttu-id="a05ff-179">Если требуется, чтобы внешний ключ ссылался на свойство, отличное от первичного ключа, можно использовать API Fluent, чтобы настроить свойство ключа субъекта для связи.</span><span class="sxs-lookup"><span data-stu-id="a05ff-179">If you want the foreign key to reference a property other than the primary key, you can use the Fluent API to configure the principal key property for the relationship.</span></span> <span data-ttu-id="a05ff-180">Свойство, настраиваемое в качестве ключа участника, будет автоматически настроено в качестве альтернативного ключа (Дополнительные сведения см. в разделе [альтернативные ключи](alternate-keys.md) ).</span><span class="sxs-lookup"><span data-stu-id="a05ff-180">The property that you configure as the principal key will automatically be setup as an alternate key (see [Alternate Keys](alternate-keys.md) for more information).</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Relationships/PrincipalKey.cs?highlight=11)] -->
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

<span data-ttu-id="a05ff-181">В следующем примере кода показано, как настроить составной ключ субъекта.</span><span class="sxs-lookup"><span data-stu-id="a05ff-181">The following code listing shows how to configure a composite principal key.</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Relationships/CompositePrincipalKey.cs?highlight=11)] -->
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
> <span data-ttu-id="a05ff-182">Порядок указания свойств ключа субъекта должен совпадать с порядком, в котором они указаны для внешнего ключа.</span><span class="sxs-lookup"><span data-stu-id="a05ff-182">The order in which you specify principal key properties must match the order in which they are specified for the foreign key.</span></span>

### <a name="required-and-optional-relationships"></a><span data-ttu-id="a05ff-183">Обязательные и необязательные связи</span><span class="sxs-lookup"><span data-stu-id="a05ff-183">Required and Optional Relationships</span></span>

<span data-ttu-id="a05ff-184">С помощью API-интерфейса Fluent можно настроить, является ли связь обязательной или необязательной.</span><span class="sxs-lookup"><span data-stu-id="a05ff-184">You can use the Fluent API to configure whether the relationship is required or optional.</span></span> <span data-ttu-id="a05ff-185">В конечном итоге это определяет, является ли свойство внешнего ключа обязательным или необязательным.</span><span class="sxs-lookup"><span data-stu-id="a05ff-185">Ultimately this controls whether the foreign key property is required or optional.</span></span> <span data-ttu-id="a05ff-186">Это наиболее полезно при использовании внешнего ключа теневого состояния.</span><span class="sxs-lookup"><span data-stu-id="a05ff-186">This is most useful when you are using a shadow state foreign key.</span></span> <span data-ttu-id="a05ff-187">Если у вас есть свойство внешнего ключа в классе сущностей, обязательность связи определяется на основе того, является ли свойство внешнего ключа обязательным или необязательным (Дополнительные сведения см. в разделе [обязательные и необязательные свойства](required-optional.md) ).</span><span class="sxs-lookup"><span data-stu-id="a05ff-187">If you have a foreign key property in your entity class then the requiredness of the relationship is determined based on whether the foreign key property is required or optional (see [Required and Optional properties](required-optional.md) for more information).</span></span>

<!-- [!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/Required.cs?highlight=11)] -->
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

### <a name="cascade-delete"></a><span data-ttu-id="a05ff-188">Каскадное удаление</span><span class="sxs-lookup"><span data-stu-id="a05ff-188">Cascade Delete</span></span>

<span data-ttu-id="a05ff-189">Для явной настройки поведения каскадного удаления для данной связи можно использовать API Fluent.</span><span class="sxs-lookup"><span data-stu-id="a05ff-189">You can use the Fluent API to configure the cascade delete behavior for a given relationship explicitly.</span></span>

<span data-ttu-id="a05ff-190">Подробное описание каждого варианта см. в разделе [каскадное удаление](../saving/cascade-delete.md) раздела сохранение данных.</span><span class="sxs-lookup"><span data-stu-id="a05ff-190">See [Cascade Delete](../saving/cascade-delete.md) on the Saving Data section for a detailed discussion of each option.</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Relationships/CascadeDelete.cs?highlight=11)] -->
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

## <a name="other-relationship-patterns"></a><span data-ttu-id="a05ff-191">Другие шаблоны отношений</span><span class="sxs-lookup"><span data-stu-id="a05ff-191">Other Relationship Patterns</span></span>

### <a name="one-to-one"></a><span data-ttu-id="a05ff-192">Один к одному</span><span class="sxs-lookup"><span data-stu-id="a05ff-192">One-to-one</span></span>

<span data-ttu-id="a05ff-193">Связь «один к одному» имеет свойство навигации «ссылка» на обеих сторонах.</span><span class="sxs-lookup"><span data-stu-id="a05ff-193">One to one relationships have a reference navigation property on both sides.</span></span> <span data-ttu-id="a05ff-194">Они следуют тем же соглашениям, что и отношения "один ко многим", но уникальный индекс вводится в свойство внешнего ключа, чтобы гарантировать, что только один зависимый объект связан с каждым участником.</span><span class="sxs-lookup"><span data-stu-id="a05ff-194">They follow the same conventions as one-to-many relationships, but a unique index is introduced on the foreign key property to ensure only one dependent is related to each principal.</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/Conventions/Relationships/OneToOne.cs?highlight=6,15,16)] -->
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
> <span data-ttu-id="a05ff-195">EF выберет одну из сущностей, которая будет зависимой, исходя из возможности обнаружения свойства внешнего ключа.</span><span class="sxs-lookup"><span data-stu-id="a05ff-195">EF will choose one of the entities to be the dependent based on its ability to detect a foreign key property.</span></span> <span data-ttu-id="a05ff-196">Если в качестве зависимого объекта выбрана неверная сущность, можно исправить это с помощью API-интерфейса Fluent.</span><span class="sxs-lookup"><span data-stu-id="a05ff-196">If the wrong entity is chosen as the dependent, you can use the Fluent API to correct this.</span></span>

<span data-ttu-id="a05ff-197">При настройке связи с помощью API-интерфейса Fluent вы используете `HasOne` методы и. `WithOne`</span><span class="sxs-lookup"><span data-stu-id="a05ff-197">When configuring the relationship with the Fluent API, you use the `HasOne` and `WithOne` methods.</span></span>

<span data-ttu-id="a05ff-198">При настройке внешнего ключа необходимо указать тип зависимой сущности — Обратите внимание на универсальный параметр, предоставленный `HasForeignKey` в приведенном ниже списке.</span><span class="sxs-lookup"><span data-stu-id="a05ff-198">When configuring the foreign key you need to specify the dependent entity type - notice the generic parameter provided to `HasForeignKey` in the listing below.</span></span> <span data-ttu-id="a05ff-199">В связи «один ко многим» ясно, что сущность с навигацией по ссылке является зависимой, а она является участником коллекции.</span><span class="sxs-lookup"><span data-stu-id="a05ff-199">In a one-to-many relationship it is clear that the entity with the reference navigation is the dependent and the one with the collection is the principal.</span></span> <span data-ttu-id="a05ff-200">Но это не так в связи «один к одному», поэтому необходимо явно определить его.</span><span class="sxs-lookup"><span data-stu-id="a05ff-200">But this is not so in a one-to-one relationship - hence the need to explicitly define it.</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Relationships/OneToOne.cs?highlight=11)] -->
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

### <a name="many-to-many"></a><span data-ttu-id="a05ff-201">Многие ко многим</span><span class="sxs-lookup"><span data-stu-id="a05ff-201">Many-to-many</span></span>

<span data-ttu-id="a05ff-202">Связи «многие ко многим» без класса сущности для представления таблицы соединений пока не поддерживаются.</span><span class="sxs-lookup"><span data-stu-id="a05ff-202">Many-to-many relationships without an entity class to represent the join table are not yet supported.</span></span> <span data-ttu-id="a05ff-203">Однако можно представить связь «многие ко многим», включив класс сущности для таблицы Join и составляя две отдельные связи «один ко многим».</span><span class="sxs-lookup"><span data-stu-id="a05ff-203">However, you can represent a many-to-many relationship by including an entity class for the join table and mapping two separate one-to-many relationships.</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Relationships/ManyToMany.cs?highlight=11,12,13,14,16,17,18,19,39,40,41,42,43,44,45,46)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Post> Posts { get; set; }
    public DbSet<Tag> Tags { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<PostTag>()
            .HasKey(pt => new { pt.PostId, pt.TagId });

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
