---
title: Связи — EF Core
description: Настройка связей между типами сущностей при использовании Entity Framework Core
author: AndriySvyryd
ms.date: 11/21/2019
uid: core/modeling/relationships
ms.openlocfilehash: 9946b2190cb3c3973f245d44da7e359b60845541
ms.sourcegitcommit: 7c3939504bb9da3f46bea3443638b808c04227c2
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/09/2020
ms.locfileid: "89619111"
---
# <a name="relationships"></a><span data-ttu-id="eb1ce-103">Отношения</span><span class="sxs-lookup"><span data-stu-id="eb1ce-103">Relationships</span></span>

<span data-ttu-id="eb1ce-104">Связь определяет, как две сущности связаны друг с другом.</span><span class="sxs-lookup"><span data-stu-id="eb1ce-104">A relationship defines how two entities relate to each other.</span></span> <span data-ttu-id="eb1ce-105">В реляционной базе данных это представление представляется ограничением внешнего ключа.</span><span class="sxs-lookup"><span data-stu-id="eb1ce-105">In a relational database, this is represented by a foreign key constraint.</span></span>

> [!NOTE]  
> <span data-ttu-id="eb1ce-106">Большинство примеров в этой статье используют связь «один ко многим» для демонстрации концепций.</span><span class="sxs-lookup"><span data-stu-id="eb1ce-106">Most of the samples in this article use a one-to-many relationship to demonstrate concepts.</span></span> <span data-ttu-id="eb1ce-107">Примеры связей "один к одному" и "многие ко многим" см. в разделе [другие шаблоны связей](#other-relationship-patterns) в конце статьи.</span><span class="sxs-lookup"><span data-stu-id="eb1ce-107">For examples of one-to-one and many-to-many relationships see the [Other Relationship Patterns](#other-relationship-patterns) section at the end of the article.</span></span>

## <a name="definition-of-terms"></a><span data-ttu-id="eb1ce-108">Определение терминов</span><span class="sxs-lookup"><span data-stu-id="eb1ce-108">Definition of terms</span></span>

<span data-ttu-id="eb1ce-109">Существует ряд терминов, используемых для описания связей.</span><span class="sxs-lookup"><span data-stu-id="eb1ce-109">There are a number of terms used to describe relationships</span></span>

* <span data-ttu-id="eb1ce-110">**Зависимая сущность:** Это сущность, содержащая свойства внешнего ключа.</span><span class="sxs-lookup"><span data-stu-id="eb1ce-110">**Dependent entity:** This is the entity that contains the foreign key properties.</span></span> <span data-ttu-id="eb1ce-111">Иногда называется "дочерним" отношением.</span><span class="sxs-lookup"><span data-stu-id="eb1ce-111">Sometimes referred to as the 'child' of the relationship.</span></span>

* <span data-ttu-id="eb1ce-112">**Сущность субъекта:** Это сущность, содержащая свойства первичного или альтернативного ключа.</span><span class="sxs-lookup"><span data-stu-id="eb1ce-112">**Principal entity:** This is the entity that contains the primary/alternate key properties.</span></span> <span data-ttu-id="eb1ce-113">Иногда называется "родителем" связи.</span><span class="sxs-lookup"><span data-stu-id="eb1ce-113">Sometimes referred to as the 'parent' of the relationship.</span></span>

* <span data-ttu-id="eb1ce-114">**Основной ключ:** Свойства, однозначно идентифицирующие сущность Principal.</span><span class="sxs-lookup"><span data-stu-id="eb1ce-114">**Principal key:** The properties that uniquely identify the principal entity.</span></span> <span data-ttu-id="eb1ce-115">Это может быть первичный ключ или альтернативный ключ.</span><span class="sxs-lookup"><span data-stu-id="eb1ce-115">This may be the primary key or an alternate key.</span></span>

* <span data-ttu-id="eb1ce-116">**Внешний ключ:** Свойства в зависимой сущности, используемые для хранения значений основных ключей для связанной сущности.</span><span class="sxs-lookup"><span data-stu-id="eb1ce-116">**Foreign key:** The properties in the dependent entity that are used to store the principal key values for the related entity.</span></span>

* <span data-ttu-id="eb1ce-117">**Свойство навигации:** Свойство, определенное для основной и (или) зависимой сущности, ссылающейся на связанную сущность.</span><span class="sxs-lookup"><span data-stu-id="eb1ce-117">**Navigation property:** A property defined on the principal and/or dependent entity that references the related entity.</span></span>

  * <span data-ttu-id="eb1ce-118">**Свойство навигации коллекции:** Свойство навигации, содержащее ссылки на множество связанных сущностей.</span><span class="sxs-lookup"><span data-stu-id="eb1ce-118">**Collection navigation property:** A navigation property that contains references to many related entities.</span></span>

  * <span data-ttu-id="eb1ce-119">**Свойство навигации по ссылке:** Свойство навигации, содержащее ссылку на одну связанную сущность.</span><span class="sxs-lookup"><span data-stu-id="eb1ce-119">**Reference navigation property:** A navigation property that holds a reference to a single related entity.</span></span>

  * <span data-ttu-id="eb1ce-120">**Обратное свойство навигации:** При обсуждении определенного свойства навигации этот термин относится к свойству навигации на другом конце связи.</span><span class="sxs-lookup"><span data-stu-id="eb1ce-120">**Inverse navigation property:** When discussing a particular navigation property, this term refers to the navigation property on the other end of the relationship.</span></span>
  
* <span data-ttu-id="eb1ce-121">**Связь, ссылающаяся на себя:** Связь, в которой зависимые и основные типы сущностей совпадают.</span><span class="sxs-lookup"><span data-stu-id="eb1ce-121">**Self-referencing relationship:** A relationship in which the dependent and the principal entity types are the same.</span></span>

<span data-ttu-id="eb1ce-122">В следующем коде показана связь «один ко многим» между `Blog` и `Post`</span><span class="sxs-lookup"><span data-stu-id="eb1ce-122">The following code shows a one-to-many relationship between `Blog` and `Post`</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Relationships/Full.cs#Full)]

* <span data-ttu-id="eb1ce-123">`Post` является зависимой сущностью</span><span class="sxs-lookup"><span data-stu-id="eb1ce-123">`Post` is the dependent entity</span></span>

* <span data-ttu-id="eb1ce-124">`Blog` является основной сущностью</span><span class="sxs-lookup"><span data-stu-id="eb1ce-124">`Blog` is the principal entity</span></span>

* <span data-ttu-id="eb1ce-125">`Blog.BlogId` является основным ключом (в данном случае это первичный ключ, а не альтернативный ключ);</span><span class="sxs-lookup"><span data-stu-id="eb1ce-125">`Blog.BlogId` is the principal key (in this case it is a primary key rather than an alternate key)</span></span>

* <span data-ttu-id="eb1ce-126">`Post.BlogId` является внешним ключом</span><span class="sxs-lookup"><span data-stu-id="eb1ce-126">`Post.BlogId` is the foreign key</span></span>

* <span data-ttu-id="eb1ce-127">`Post.Blog` является свойством навигации по ссылке</span><span class="sxs-lookup"><span data-stu-id="eb1ce-127">`Post.Blog` is a reference navigation property</span></span>

* <span data-ttu-id="eb1ce-128">`Blog.Posts` — Это свойство навигации коллекции</span><span class="sxs-lookup"><span data-stu-id="eb1ce-128">`Blog.Posts` is a collection navigation property</span></span>

* <span data-ttu-id="eb1ce-129">`Post.Blog` Свойство обратной навигации `Blog.Posts` (и наоборот)</span><span class="sxs-lookup"><span data-stu-id="eb1ce-129">`Post.Blog` is the inverse navigation property of `Blog.Posts` (and vice versa)</span></span>

## <a name="conventions"></a><span data-ttu-id="eb1ce-130">Соглашения</span><span class="sxs-lookup"><span data-stu-id="eb1ce-130">Conventions</span></span>

<span data-ttu-id="eb1ce-131">По умолчанию связь будет создана при обнаружении свойства навигации для типа.</span><span class="sxs-lookup"><span data-stu-id="eb1ce-131">By default, a relationship will be created when there is a navigation property discovered on a type.</span></span> <span data-ttu-id="eb1ce-132">Свойство считается свойством навигации, если тип, на который он указывает, не может быть сопоставлен с текущим поставщиком базы данных как скалярный тип.</span><span class="sxs-lookup"><span data-stu-id="eb1ce-132">A property is considered a navigation property if the type it points to can not be mapped as a scalar type by the current database provider.</span></span>

> [!NOTE]  
> <span data-ttu-id="eb1ce-133">Связи, обнаруженные соглашением, всегда будут указывать первичный ключ основной сущности.</span><span class="sxs-lookup"><span data-stu-id="eb1ce-133">Relationships that are discovered by convention will always target the primary key of the principal entity.</span></span> <span data-ttu-id="eb1ce-134">Чтобы выбрать альтернативный ключ, необходимо выполнить дополнительную настройку с помощью API Fluent.</span><span class="sxs-lookup"><span data-stu-id="eb1ce-134">To target an alternate key, additional configuration must be performed using the Fluent API.</span></span>

### <a name="fully-defined-relationships"></a><span data-ttu-id="eb1ce-135">Полностью определенные связи</span><span class="sxs-lookup"><span data-stu-id="eb1ce-135">Fully defined relationships</span></span>

<span data-ttu-id="eb1ce-136">Наиболее распространенным шаблоном для связей является наличие свойств навигации, определенных на обоих концах связи, и свойства внешнего ключа, определенного в зависимом классе сущности.</span><span class="sxs-lookup"><span data-stu-id="eb1ce-136">The most common pattern for relationships is to have navigation properties defined on both ends of the relationship and a foreign key property defined in the dependent entity class.</span></span>

* <span data-ttu-id="eb1ce-137">Если между двумя типами обнаружена пара свойств навигации, они будут настроены как обратные свойства навигации одной и той же связи.</span><span class="sxs-lookup"><span data-stu-id="eb1ce-137">If a pair of navigation properties is found between two types, then they will be configured as inverse navigation properties of the same relationship.</span></span>

* <span data-ttu-id="eb1ce-138">Если зависимая сущность содержит свойство с именем, соответствующим одному из этих шаблонов, оно будет настроено как внешний ключ:</span><span class="sxs-lookup"><span data-stu-id="eb1ce-138">If the dependent entity contains a property with a name matching one of these patterns then it will be configured as the foreign key:</span></span>
  * `<navigation property name><principal key property name>`
  * `<navigation property name>Id`
  * `<principal entity name><principal key property name>`
  * `<principal entity name>Id`

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Relationships/Full.cs?name=Full&highlight=6,15-16)]

<span data-ttu-id="eb1ce-139">В этом примере выделенные свойства будут использоваться для настройки связи.</span><span class="sxs-lookup"><span data-stu-id="eb1ce-139">In this example the highlighted properties will be used to configure the relationship.</span></span>

> [!NOTE]
> <span data-ttu-id="eb1ce-140">Если свойство является первичным ключом или имеет тип, несовместимый с ключом субъекта, он не будет настроен в качестве внешнего ключа.</span><span class="sxs-lookup"><span data-stu-id="eb1ce-140">If the property is the primary key or is of a type not compatible with the principal key then it won't be configured as the foreign key.</span></span>

> [!NOTE]
> <span data-ttu-id="eb1ce-141">До EF Core 3,0 свойство с именем, точно совпадающее со свойством ключа субъекта, [также было сопоставлено с внешним ключом](https://github.com/aspnet/EntityFrameworkCore/issues/13274) .</span><span class="sxs-lookup"><span data-stu-id="eb1ce-141">Before EF Core 3.0 the property named exactly the same as the principal key property [was also matched as the foreign key](https://github.com/aspnet/EntityFrameworkCore/issues/13274)</span></span>

### <a name="no-foreign-key-property"></a><span data-ttu-id="eb1ce-142">Нет свойства внешнего ключа</span><span class="sxs-lookup"><span data-stu-id="eb1ce-142">No foreign key property</span></span>

<span data-ttu-id="eb1ce-143">Хотя рекомендуется использовать свойство внешнего ключа, определенное в зависимом классе сущности, оно не является обязательным.</span><span class="sxs-lookup"><span data-stu-id="eb1ce-143">While it is recommended to have a foreign key property defined in the dependent entity class, it is not required.</span></span> <span data-ttu-id="eb1ce-144">Если свойство внешнего ключа не найдено, [свойство теневого внешнего ключа](xref:core/modeling/shadow-properties) будет вводиться с именем или, `<navigation property name><principal key property name>` `<principal entity name><principal key property name>` Если в зависимом типе отсутствует Навигация.</span><span class="sxs-lookup"><span data-stu-id="eb1ce-144">If no foreign key property is found, a [shadow foreign key property](xref:core/modeling/shadow-properties) will be introduced with the name `<navigation property name><principal key property name>` or `<principal entity name><principal key property name>` if no navigation is present on the dependent type.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Relationships/NoForeignKey.cs?name=NoForeignKey&highlight=6,15)]

<span data-ttu-id="eb1ce-145">В этом примере теневым внешним ключом является то, `BlogId` что предожидание имени навигации будет избыточным.</span><span class="sxs-lookup"><span data-stu-id="eb1ce-145">In this example the shadow foreign key is `BlogId` because prepending the navigation name would be redundant.</span></span>

> [!NOTE]
> <span data-ttu-id="eb1ce-146">Если свойство с таким именем уже существует, имя теневого свойства будет суффиксом с номером.</span><span class="sxs-lookup"><span data-stu-id="eb1ce-146">If a property with the same name already exists then the shadow property name will be suffixed with a number.</span></span>

### <a name="single-navigation-property"></a><span data-ttu-id="eb1ce-147">Одно свойство навигации</span><span class="sxs-lookup"><span data-stu-id="eb1ce-147">Single navigation property</span></span>

<span data-ttu-id="eb1ce-148">Включение только одного свойства навигации (без обратной навигации и свойства внешнего ключа) достаточно для того, чтобы иметь связь, определенную по соглашению.</span><span class="sxs-lookup"><span data-stu-id="eb1ce-148">Including just one navigation property (no inverse navigation, and no foreign key property) is enough to have a relationship defined by convention.</span></span> <span data-ttu-id="eb1ce-149">Можно также иметь одно свойство навигации и внешнее ключевое свойство.</span><span class="sxs-lookup"><span data-stu-id="eb1ce-149">You can also have a single navigation property and a foreign key property.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Relationships/OneNavigation.cs?name=OneNavigation&highlight=6)]

### <a name="limitations"></a><span data-ttu-id="eb1ce-150">Ограничения</span><span class="sxs-lookup"><span data-stu-id="eb1ce-150">Limitations</span></span>

<span data-ttu-id="eb1ce-151">Если между двумя типами определено несколько свойств навигации (то есть более одной пары переходов, которые указывают друг на друга), связи, представленные свойствами навигации, являются неоднозначными.</span><span class="sxs-lookup"><span data-stu-id="eb1ce-151">When there are multiple navigation properties defined between two types (that is, more than just one pair of navigations that point to each other) the relationships represented by the navigation properties are ambiguous.</span></span> <span data-ttu-id="eb1ce-152">Их необходимо настроить вручную для устранения неоднозначности.</span><span class="sxs-lookup"><span data-stu-id="eb1ce-152">You will need to manually configure them to resolve the ambiguity.</span></span>

### <a name="cascade-delete"></a><span data-ttu-id="eb1ce-153">Каскадное удаление</span><span class="sxs-lookup"><span data-stu-id="eb1ce-153">Cascade delete</span></span>

<span data-ttu-id="eb1ce-154">По соглашению каскадное удаление будет установлено в *CASCADE* для требуемых связей и *клиентсетнулл* для необязательных связей.</span><span class="sxs-lookup"><span data-stu-id="eb1ce-154">By convention, cascade delete will be set to *Cascade* for required relationships and *ClientSetNull* for optional relationships.</span></span> <span data-ttu-id="eb1ce-155">*CASCADE* означает также удаление зависимых сущностей.</span><span class="sxs-lookup"><span data-stu-id="eb1ce-155">*Cascade* means dependent entities are also deleted.</span></span> <span data-ttu-id="eb1ce-156">*Клиентсетнулл* означает, что зависимые сущности, которые не загружены в память, останутся неизменными и должны быть удалены вручную или обновлены, чтобы они указывали на действительную сущность Principal.</span><span class="sxs-lookup"><span data-stu-id="eb1ce-156">*ClientSetNull* means that dependent entities that are not loaded into memory will remain unchanged and must be manually deleted, or updated to point to a valid principal entity.</span></span> <span data-ttu-id="eb1ce-157">Для сущностей, загруженных в память, EF Core попытается установить свойства внешнего ключа в значение null.</span><span class="sxs-lookup"><span data-stu-id="eb1ce-157">For entities that are loaded into memory, EF Core will attempt to set the foreign key properties to null.</span></span>

<span data-ttu-id="eb1ce-158">Различия между обязательными и дополнительными связями см. в разделе [обязательные и дополнительные связи](#required-and-optional-relationships) .</span><span class="sxs-lookup"><span data-stu-id="eb1ce-158">See the [Required and Optional Relationships](#required-and-optional-relationships) section for the difference between required and optional relationships.</span></span>

<span data-ttu-id="eb1ce-159">Дополнительные сведения о различных поведениях при удалении и значения по умолчанию, используемые по соглашению, см. в разделе [каскадное удаление](xref:core/saving/cascade-delete) .</span><span class="sxs-lookup"><span data-stu-id="eb1ce-159">See [Cascade Delete](xref:core/saving/cascade-delete) for more details about the different delete behaviors and the defaults used by convention.</span></span>

## <a name="manual-configuration"></a><span data-ttu-id="eb1ce-160">Настройка вручную</span><span class="sxs-lookup"><span data-stu-id="eb1ce-160">Manual configuration</span></span>

### <a name="fluent-api"></a>[<span data-ttu-id="eb1ce-161">Текучий API</span><span class="sxs-lookup"><span data-stu-id="eb1ce-161">Fluent API</span></span>](#tab/fluent-api)

<span data-ttu-id="eb1ce-162">Чтобы настроить связь в API Fluent, начните с определения свойств навигации, составляющих связь.</span><span class="sxs-lookup"><span data-stu-id="eb1ce-162">To configure a relationship in the Fluent API, you start by identifying the navigation properties that make up the relationship.</span></span> <span data-ttu-id="eb1ce-163">`HasOne` или `HasMany` определяет свойство навигации для типа сущности, на котором начинается настройка.</span><span class="sxs-lookup"><span data-stu-id="eb1ce-163">`HasOne` or `HasMany` identifies the navigation property on the entity type you are beginning the configuration on.</span></span> <span data-ttu-id="eb1ce-164">Затем вы позвоните в цепочку `WithOne` или `WithMany` для поиска обратной навигации.</span><span class="sxs-lookup"><span data-stu-id="eb1ce-164">You then chain a call to `WithOne` or `WithMany` to identify the inverse navigation.</span></span> <span data-ttu-id="eb1ce-165">`HasOne`/`WithOne`используются для свойств навигации по ссылке и `HasMany` / `WithMany` используются для свойств навигации по коллекциям.</span><span class="sxs-lookup"><span data-stu-id="eb1ce-165">`HasOne`/`WithOne` are used for reference navigation properties and `HasMany`/`WithMany` are used for collection navigation properties.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/NoForeignKey.cs?name=NoForeignKey&highlight=8-10)]

### <a name="data-annotations"></a>[<span data-ttu-id="eb1ce-166">Заметки к данным</span><span class="sxs-lookup"><span data-stu-id="eb1ce-166">Data annotations</span></span>](#tab/data-annotations)

<span data-ttu-id="eb1ce-167">Заметки к данным можно использовать для настройки способа связывания свойств навигации в зависимых и субъектах сущностей.</span><span class="sxs-lookup"><span data-stu-id="eb1ce-167">You can use the Data Annotations to configure how navigation properties on the dependent and principal entities pair up.</span></span> <span data-ttu-id="eb1ce-168">Обычно это делается при наличии более одной пары свойств навигации между двумя типами сущностей.</span><span class="sxs-lookup"><span data-stu-id="eb1ce-168">This is typically done when there is more than one pair of navigation properties between two entity types.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Relationships/InverseProperty.cs?name=InverseProperty&highlight=20,23)]

> [!NOTE]
> <span data-ttu-id="eb1ce-169">[Обязательное значение] можно использовать только для свойств зависимой сущности, чтобы повлиять на требуемую связь.</span><span class="sxs-lookup"><span data-stu-id="eb1ce-169">You can only use [Required] on properties on the dependent entity to impact the requiredness of the relationship.</span></span> <span data-ttu-id="eb1ce-170">[Обязательное значение] в переходе от основной сущности обычно игнорируется, но может привести к тому, что сущность будет зависимой от нее.</span><span class="sxs-lookup"><span data-stu-id="eb1ce-170">[Required] on the navigation from the principal entity is usually ignored, but it may cause the entity to become the dependent one.</span></span>

> [!NOTE]
> <span data-ttu-id="eb1ce-171">Заметки к данным `[ForeignKey]` и `[InverseProperty]` доступны в `System.ComponentModel.DataAnnotations.Schema` пространстве имен.</span><span class="sxs-lookup"><span data-stu-id="eb1ce-171">The data annotations `[ForeignKey]` and `[InverseProperty]` are available in the `System.ComponentModel.DataAnnotations.Schema` namespace.</span></span> <span data-ttu-id="eb1ce-172">`[Required]` доступен в `System.ComponentModel.DataAnnotations` пространстве имен.</span><span class="sxs-lookup"><span data-stu-id="eb1ce-172">`[Required]` is available in the `System.ComponentModel.DataAnnotations` namespace.</span></span>

---

### <a name="single-navigation-property"></a><span data-ttu-id="eb1ce-173">Одно свойство навигации</span><span class="sxs-lookup"><span data-stu-id="eb1ce-173">Single navigation property</span></span>

<span data-ttu-id="eb1ce-174">Если у вас есть только одно свойство навигации, то существуют перегрузки без параметров `WithOne` и `WithMany` .</span><span class="sxs-lookup"><span data-stu-id="eb1ce-174">If you only have one navigation property then there are parameterless overloads of `WithOne` and `WithMany`.</span></span> <span data-ttu-id="eb1ce-175">Это указывает на то, что на другом конце связи имеется концептуальная ссылка или коллекция, но в классе сущностей не содержится свойство навигации.</span><span class="sxs-lookup"><span data-stu-id="eb1ce-175">This indicates that there is conceptually a reference or collection on the other end of the relationship, but there is no navigation property included in the entity class.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/OneNavigation.cs?name=OneNavigation&highlight=8-10)]

---

### <a name="configuring-navigation-properties"></a><span data-ttu-id="eb1ce-176">Настройка свойств навигации</span><span class="sxs-lookup"><span data-stu-id="eb1ce-176">Configuring navigation properties</span></span>

<span data-ttu-id="eb1ce-177">После создания свойства навигации может потребоваться его дальнейшая настройка.</span><span class="sxs-lookup"><span data-stu-id="eb1ce-177">After the navigation property has been created, you may need to further configure it.</span></span> <span data-ttu-id="eb1ce-178">В Ефкоре 5,0 добавлен новый API-интерфейс Fluent, позволяющий выполнить эту настройку.</span><span class="sxs-lookup"><span data-stu-id="eb1ce-178">In EFCore 5.0, new Fluent API is added to allow you to perform that configuration.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/NavigationConfiguration.cs?name=NavigationConfiguration&highlight=7-9)]

> [!NOTE]
> <span data-ttu-id="eb1ce-179">Этот вызов нельзя использовать для создания свойства навигации.</span><span class="sxs-lookup"><span data-stu-id="eb1ce-179">This call cannot be used to create a navigation property.</span></span> <span data-ttu-id="eb1ce-180">Он используется только для настройки свойства навигации, которое было ранее создано путем определения связи или соглашения.</span><span class="sxs-lookup"><span data-stu-id="eb1ce-180">It is only used to configure a navigation property which has been previously created by defining a relationship or from a convention.</span></span>

### <a name="foreign-key"></a><span data-ttu-id="eb1ce-181">Внешний ключ</span><span class="sxs-lookup"><span data-stu-id="eb1ce-181">Foreign key</span></span>

#### <a name="fluent-api-simple-key"></a>[<span data-ttu-id="eb1ce-182">API Fluent (простой ключ)</span><span class="sxs-lookup"><span data-stu-id="eb1ce-182">Fluent API (simple key)</span></span>](#tab/fluent-api-simple-key)

<span data-ttu-id="eb1ce-183">API-интерфейс Fluent можно использовать для настройки того, какое свойство должно использоваться в качестве свойства внешнего ключа для данной связи:</span><span class="sxs-lookup"><span data-stu-id="eb1ce-183">You can use the Fluent API to configure which property should be used as the foreign key property for a given relationship:</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/ForeignKey.cs?name=ForeignKey&highlight=11)]

#### <a name="fluent-api-composite-key"></a>[<span data-ttu-id="eb1ce-184">API Fluent (составной ключ)</span><span class="sxs-lookup"><span data-stu-id="eb1ce-184">Fluent API (composite key)</span></span>](#tab/fluent-api-composite-key)

<span data-ttu-id="eb1ce-185">API-интерфейс Fluent можно использовать для настройки свойств, которые должны использоваться в качестве свойств составного внешнего ключа для данной связи.</span><span class="sxs-lookup"><span data-stu-id="eb1ce-185">You can use the Fluent API to configure which properties should be used as the composite foreign key properties for a given relationship:</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/CompositeForeignKey.cs?name=CompositeForeignKey&highlight=13)]

#### <a name="data-annotations-simple-key"></a>[<span data-ttu-id="eb1ce-186">Заметки к данным (простой ключ)</span><span class="sxs-lookup"><span data-stu-id="eb1ce-186">Data annotations (simple key)</span></span>](#tab/data-annotations-simple-key)

<span data-ttu-id="eb1ce-187">Заметки к данным можно использовать для настройки того, какое свойство должно использоваться в качестве свойства внешнего ключа для данной связи.</span><span class="sxs-lookup"><span data-stu-id="eb1ce-187">You can use the Data Annotations to configure which property should be used as the foreign key property for a given relationship.</span></span> <span data-ttu-id="eb1ce-188">Обычно это делается, когда свойство внешнего ключа не обнаруживается по соглашению:</span><span class="sxs-lookup"><span data-stu-id="eb1ce-188">This is typically done when the foreign key property is not discovered by convention:</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Relationships/ForeignKey.cs?name=ForeignKey&highlight=17)]

> [!TIP]  
> <span data-ttu-id="eb1ce-189">`[ForeignKey]`Заметку можно поместить в любое свойство навигации в связи.</span><span class="sxs-lookup"><span data-stu-id="eb1ce-189">The `[ForeignKey]` annotation can be placed on either navigation property in the relationship.</span></span> <span data-ttu-id="eb1ce-190">Не требуется переходить к свойству навигации в классе зависимой сущности.</span><span class="sxs-lookup"><span data-stu-id="eb1ce-190">It does not need to go on the navigation property in the dependent entity class.</span></span>

> [!NOTE]
> <span data-ttu-id="eb1ce-191">Свойство, указанное с помощью `[ForeignKey]` в свойстве навигации, не обязательно должно существовать в зависимом типе.</span><span class="sxs-lookup"><span data-stu-id="eb1ce-191">The property specified using `[ForeignKey]` on a navigation property doesn't need to exist on the dependent type.</span></span> <span data-ttu-id="eb1ce-192">В этом случае для создания теневого внешнего ключа будет использоваться указанное имя.</span><span class="sxs-lookup"><span data-stu-id="eb1ce-192">In this case the specified name will be used to create a shadow foreign key.</span></span>

---

#### <a name="shadow-foreign-key"></a><span data-ttu-id="eb1ce-193">Теневой внешний ключ</span><span class="sxs-lookup"><span data-stu-id="eb1ce-193">Shadow foreign key</span></span>

<span data-ttu-id="eb1ce-194">`HasForeignKey(...)`Чтобы настроить свойство теневого копирования в качестве внешнего ключа, можно использовать строку перегрузки в. Дополнительные сведения см. в разделе [Свойства тени](xref:core/modeling/shadow-properties) .</span><span class="sxs-lookup"><span data-stu-id="eb1ce-194">You can use the string overload of `HasForeignKey(...)` to configure a shadow property as a foreign key (see [Shadow Properties](xref:core/modeling/shadow-properties) for more information).</span></span> <span data-ttu-id="eb1ce-195">Рекомендуется явно добавить свойство Shadow в модель, прежде чем использовать ее как внешний ключ (как показано ниже).</span><span class="sxs-lookup"><span data-stu-id="eb1ce-195">We recommend explicitly adding the shadow property to the model before using it as a foreign key (as shown below).</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/ShadowForeignKey.cs?name=ShadowForeignKey&highlight=10,16)]

#### <a name="foreign-key-constraint-name"></a><span data-ttu-id="eb1ce-196">Имя ограничения внешнего ключа</span><span class="sxs-lookup"><span data-stu-id="eb1ce-196">Foreign key constraint name</span></span>

<span data-ttu-id="eb1ce-197">По соглашению при нацеливании на реляционную базу данных ограничения внешнего ключа именуются FK_ <dependent type name> _<principal type name>_ <foreign key property name> .</span><span class="sxs-lookup"><span data-stu-id="eb1ce-197">By convention, when targeting a relational database, foreign key constraints are named FK_<dependent type name>_<principal type name>_<foreign key property name>.</span></span> <span data-ttu-id="eb1ce-198">Для составных внешних ключей <foreign key property name> преобразуется в список имен свойств внешнего ключа с разделителями подчеркивания.</span><span class="sxs-lookup"><span data-stu-id="eb1ce-198">For composite foreign keys <foreign key property name> becomes an underscore separated list of foreign key property names.</span></span>

<span data-ttu-id="eb1ce-199">Имя ограничения также можно настроить следующим образом:</span><span class="sxs-lookup"><span data-stu-id="eb1ce-199">You can also configure the constraint name as follows:</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/ConstraintName.cs?name=ConstraintName&highlight=6-7)]

### <a name="without-navigation-property"></a><span data-ttu-id="eb1ce-200">Без свойства навигации</span><span class="sxs-lookup"><span data-stu-id="eb1ce-200">Without navigation property</span></span>

<span data-ttu-id="eb1ce-201">Указывать свойство навигации не обязательно.</span><span class="sxs-lookup"><span data-stu-id="eb1ce-201">You don't necessarily need to provide a navigation property.</span></span> <span data-ttu-id="eb1ce-202">Можно просто предоставить внешний ключ на одной стороне связи.</span><span class="sxs-lookup"><span data-stu-id="eb1ce-202">You can simply provide a foreign key on one side of the relationship.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/NoNavigation.cs?name=NoNavigation&highlight=8-11)]

### <a name="principal-key"></a><span data-ttu-id="eb1ce-203">Основной ключ</span><span class="sxs-lookup"><span data-stu-id="eb1ce-203">Principal key</span></span>

<span data-ttu-id="eb1ce-204">Если требуется, чтобы внешний ключ ссылался на свойство, отличное от первичного ключа, можно использовать API Fluent, чтобы настроить свойство ключа субъекта для связи.</span><span class="sxs-lookup"><span data-stu-id="eb1ce-204">If you want the foreign key to reference a property other than the primary key, you can use the Fluent API to configure the principal key property for the relationship.</span></span> <span data-ttu-id="eb1ce-205">Свойство, настраиваемое в качестве ключа участника, будет автоматически настроено в качестве [альтернативного ключа](xref:core/modeling/keys#alternate-keys).</span><span class="sxs-lookup"><span data-stu-id="eb1ce-205">The property that you configure as the principal key will automatically be setup as an [alternate key](xref:core/modeling/keys#alternate-keys).</span></span>

#### <a name="simple-key"></a>[<span data-ttu-id="eb1ce-206">Простой ключ</span><span class="sxs-lookup"><span data-stu-id="eb1ce-206">Simple key</span></span>](#tab/simple-key)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/PrincipalKey.cs?name=PrincipalKey&highlight=11)]

#### <a name="composite-key"></a>[<span data-ttu-id="eb1ce-207">Составной ключ</span><span class="sxs-lookup"><span data-stu-id="eb1ce-207">Composite key</span></span>](#tab/composite-key)

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/CompositePrincipalKey.cs?name=CompositePrincipalKey&highlight=11)]

> [!WARNING]  
> <span data-ttu-id="eb1ce-208">Порядок указания свойств ключа субъекта должен совпадать с порядком, в котором они указаны для внешнего ключа.</span><span class="sxs-lookup"><span data-stu-id="eb1ce-208">The order in which you specify principal key properties must match the order in which they are specified for the foreign key.</span></span>

---

### <a name="required-and-optional-relationships"></a><span data-ttu-id="eb1ce-209">Обязательные и необязательные связи</span><span class="sxs-lookup"><span data-stu-id="eb1ce-209">Required and optional relationships</span></span>

<span data-ttu-id="eb1ce-210">С помощью API-интерфейса Fluent можно настроить, является ли связь обязательной или необязательной.</span><span class="sxs-lookup"><span data-stu-id="eb1ce-210">You can use the Fluent API to configure whether the relationship is required or optional.</span></span> <span data-ttu-id="eb1ce-211">В конечном итоге это определяет, является ли свойство внешнего ключа обязательным или необязательным.</span><span class="sxs-lookup"><span data-stu-id="eb1ce-211">Ultimately this controls whether the foreign key property is required or optional.</span></span> <span data-ttu-id="eb1ce-212">Это наиболее полезно при использовании внешнего ключа теневого состояния.</span><span class="sxs-lookup"><span data-stu-id="eb1ce-212">This is most useful when you are using a shadow state foreign key.</span></span> <span data-ttu-id="eb1ce-213">Если у вас есть свойство внешнего ключа в классе сущностей, обязательность связи определяется на основе того, является ли свойство внешнего ключа обязательным или необязательным (Дополнительные сведения см. в разделе [обязательные и необязательные свойства](xref:core/modeling/entity-properties#required-and-optional-properties) ).</span><span class="sxs-lookup"><span data-stu-id="eb1ce-213">If you have a foreign key property in your entity class then the requiredness of the relationship is determined based on whether the foreign key property is required or optional (see [Required and Optional properties](xref:core/modeling/entity-properties#required-and-optional-properties) for more information).</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/Required.cs?name=Required&highlight=6)]

> [!NOTE]
> <span data-ttu-id="eb1ce-214">Вызов `IsRequired(false)` также делает свойство внешнего ключа необязательным, если оно не настроено в противном случае.</span><span class="sxs-lookup"><span data-stu-id="eb1ce-214">Calling `IsRequired(false)` also makes the foreign key property optional unless it's configured otherwise.</span></span>

### <a name="cascade-delete"></a><span data-ttu-id="eb1ce-215">Каскадное удаление</span><span class="sxs-lookup"><span data-stu-id="eb1ce-215">Cascade delete</span></span>

<span data-ttu-id="eb1ce-216">Для явной настройки поведения каскадного удаления для данной связи можно использовать API Fluent.</span><span class="sxs-lookup"><span data-stu-id="eb1ce-216">You can use the Fluent API to configure the cascade delete behavior for a given relationship explicitly.</span></span>

<span data-ttu-id="eb1ce-217">Подробное описание каждого варианта см. в разделе [каскадное удаление](xref:core/saving/cascade-delete) .</span><span class="sxs-lookup"><span data-stu-id="eb1ce-217">See [Cascade Delete](xref:core/saving/cascade-delete) for a detailed discussion of each option.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/CascadeDelete.cs?name=CascadeDelete&highlight=6)]

## <a name="other-relationship-patterns"></a><span data-ttu-id="eb1ce-218">Другие шаблоны отношений</span><span class="sxs-lookup"><span data-stu-id="eb1ce-218">Other relationship patterns</span></span>

### <a name="one-to-one"></a><span data-ttu-id="eb1ce-219">"Одна к одной"</span><span class="sxs-lookup"><span data-stu-id="eb1ce-219">One-to-one</span></span>

<span data-ttu-id="eb1ce-220">Связь «один к одному» имеет свойство навигации «ссылка» на обеих сторонах.</span><span class="sxs-lookup"><span data-stu-id="eb1ce-220">One to one relationships have a reference navigation property on both sides.</span></span> <span data-ttu-id="eb1ce-221">Они следуют тем же соглашениям, что и отношения "один ко многим", но уникальный индекс вводится в свойство внешнего ключа, чтобы гарантировать, что только один зависимый объект связан с каждым участником.</span><span class="sxs-lookup"><span data-stu-id="eb1ce-221">They follow the same conventions as one-to-many relationships, but a unique index is introduced on the foreign key property to ensure only one dependent is related to each principal.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Relationships/OneToOne.cs?name=OneToOne&highlight=6,15-16)]

> [!NOTE]  
> <span data-ttu-id="eb1ce-222">EF выберет одну из сущностей, которая будет зависимой, исходя из возможности обнаружения свойства внешнего ключа.</span><span class="sxs-lookup"><span data-stu-id="eb1ce-222">EF will choose one of the entities to be the dependent based on its ability to detect a foreign key property.</span></span> <span data-ttu-id="eb1ce-223">Если в качестве зависимого объекта выбрана неверная сущность, можно исправить это с помощью API-интерфейса Fluent.</span><span class="sxs-lookup"><span data-stu-id="eb1ce-223">If the wrong entity is chosen as the dependent, you can use the Fluent API to correct this.</span></span>

<span data-ttu-id="eb1ce-224">При настройке связи с помощью API-интерфейса Fluent вы используете `HasOne` методы и `WithOne` .</span><span class="sxs-lookup"><span data-stu-id="eb1ce-224">When configuring the relationship with the Fluent API, you use the `HasOne` and `WithOne` methods.</span></span>

<span data-ttu-id="eb1ce-225">При настройке внешнего ключа необходимо указать тип зависимой сущности — Обратите внимание на универсальный параметр, предоставленный `HasForeignKey` в приведенном ниже списке.</span><span class="sxs-lookup"><span data-stu-id="eb1ce-225">When configuring the foreign key you need to specify the dependent entity type - notice the generic parameter provided to `HasForeignKey` in the listing below.</span></span> <span data-ttu-id="eb1ce-226">В связи «один ко многим» ясно, что сущность с навигацией по ссылке является зависимой, а она является участником коллекции.</span><span class="sxs-lookup"><span data-stu-id="eb1ce-226">In a one-to-many relationship it is clear that the entity with the reference navigation is the dependent and the one with the collection is the principal.</span></span> <span data-ttu-id="eb1ce-227">Но это не так в связи «один к одному», поэтому необходимо явно определить его.</span><span class="sxs-lookup"><span data-stu-id="eb1ce-227">But this is not so in a one-to-one relationship - hence the need to explicitly define it.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/OneToOne.cs?name=OneToOne&highlight=11)]

### <a name="many-to-many"></a><span data-ttu-id="eb1ce-228">"Много ко многим"</span><span class="sxs-lookup"><span data-stu-id="eb1ce-228">Many-to-many</span></span>

<span data-ttu-id="eb1ce-229">Связи «многие ко многим» без класса сущности для представления таблицы соединений пока не поддерживаются.</span><span class="sxs-lookup"><span data-stu-id="eb1ce-229">Many-to-many relationships without an entity class to represent the join table are not yet supported.</span></span> <span data-ttu-id="eb1ce-230">Однако можно представить связь «многие ко многим», включив класс сущности для таблицы Join и составляя две отдельные связи «один ко многим».</span><span class="sxs-lookup"><span data-stu-id="eb1ce-230">However, you can represent a many-to-many relationship by including an entity class for the join table and mapping two separate one-to-many relationships.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/ManyToMany.cs?name=ManyToMany&highlight=11-14,16-19,39-46)]
