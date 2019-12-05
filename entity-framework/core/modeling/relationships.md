---
title: Связи — EF Core
description: Настройка связей между типами сущностей при использовании Entity Framework Core
author: AndriySvyryd
ms.date: 11/21/2019
uid: core/modeling/relationships
ms.openlocfilehash: 452169c902d56fda0a65a5c2846a9b42c80640fd
ms.sourcegitcommit: 7a709ce4f77134782393aa802df5ab2718714479
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/04/2019
ms.locfileid: "74824760"
---
# <a name="relationships"></a><span data-ttu-id="7fa84-103">Отношения</span><span class="sxs-lookup"><span data-stu-id="7fa84-103">Relationships</span></span>

<span data-ttu-id="7fa84-104">Связь определяет, как две сущности связаны друг с другом.</span><span class="sxs-lookup"><span data-stu-id="7fa84-104">A relationship defines how two entities relate to each other.</span></span> <span data-ttu-id="7fa84-105">В реляционной базе данных это представление представляется ограничением внешнего ключа.</span><span class="sxs-lookup"><span data-stu-id="7fa84-105">In a relational database, this is represented by a foreign key constraint.</span></span>

> [!NOTE]  
> <span data-ttu-id="7fa84-106">Большинство примеров в этой статье используют связь «один ко многим» для демонстрации концепций.</span><span class="sxs-lookup"><span data-stu-id="7fa84-106">Most of the samples in this article use a one-to-many relationship to demonstrate concepts.</span></span> <span data-ttu-id="7fa84-107">Примеры связей "один к одному" и "многие ко многим" см. в разделе [другие шаблоны связей](#other-relationship-patterns) в конце статьи.</span><span class="sxs-lookup"><span data-stu-id="7fa84-107">For examples of one-to-one and many-to-many relationships see the [Other Relationship Patterns](#other-relationship-patterns) section at the end of the article.</span></span>

## <a name="definition-of-terms"></a><span data-ttu-id="7fa84-108">Определение терминов</span><span class="sxs-lookup"><span data-stu-id="7fa84-108">Definition of terms</span></span>

<span data-ttu-id="7fa84-109">Существует ряд терминов, используемых для описания связей.</span><span class="sxs-lookup"><span data-stu-id="7fa84-109">There are a number of terms used to describe relationships</span></span>

* <span data-ttu-id="7fa84-110">**Зависимая сущность:** Это сущность, содержащая свойства внешнего ключа.</span><span class="sxs-lookup"><span data-stu-id="7fa84-110">**Dependent entity:** This is the entity that contains the foreign key properties.</span></span> <span data-ttu-id="7fa84-111">Иногда называется "дочерним" отношением.</span><span class="sxs-lookup"><span data-stu-id="7fa84-111">Sometimes referred to as the 'child' of the relationship.</span></span>

* <span data-ttu-id="7fa84-112">**Сущность субъекта:** Это сущность, содержащая свойства первичного или альтернативного ключа.</span><span class="sxs-lookup"><span data-stu-id="7fa84-112">**Principal entity:** This is the entity that contains the primary/alternate key properties.</span></span> <span data-ttu-id="7fa84-113">Иногда называется "родителем" связи.</span><span class="sxs-lookup"><span data-stu-id="7fa84-113">Sometimes referred to as the 'parent' of the relationship.</span></span>

* <span data-ttu-id="7fa84-114">**Внешний ключ:** Свойства в зависимой сущности, используемые для хранения значений основных ключей для связанной сущности.</span><span class="sxs-lookup"><span data-stu-id="7fa84-114">**Foreign key:** The properties in the dependent entity that are used to store the principal key values for the related entity.</span></span>

* <span data-ttu-id="7fa84-115">**Основной ключ:** Свойства, однозначно идентифицирующие сущность Principal.</span><span class="sxs-lookup"><span data-stu-id="7fa84-115">**Principal key:** The properties that uniquely identify the principal entity.</span></span> <span data-ttu-id="7fa84-116">Это может быть первичный ключ или альтернативный ключ.</span><span class="sxs-lookup"><span data-stu-id="7fa84-116">This may be the primary key or an alternate key.</span></span>

* <span data-ttu-id="7fa84-117">**Свойство навигации:** Свойство, определенное для основной и (или) зависимой сущности, ссылающейся на связанную сущность.</span><span class="sxs-lookup"><span data-stu-id="7fa84-117">**Navigation property:** A property defined on the principal and/or dependent entity that references the related entity.</span></span>

  * <span data-ttu-id="7fa84-118">**Свойство навигации коллекции:** Свойство навигации, содержащее ссылки на множество связанных сущностей.</span><span class="sxs-lookup"><span data-stu-id="7fa84-118">**Collection navigation property:** A navigation property that contains references to many related entities.</span></span>

  * <span data-ttu-id="7fa84-119">**Свойство навигации по ссылке:** Свойство навигации, содержащее ссылку на одну связанную сущность.</span><span class="sxs-lookup"><span data-stu-id="7fa84-119">**Reference navigation property:** A navigation property that holds a reference to a single related entity.</span></span>

  * <span data-ttu-id="7fa84-120">**Обратное свойство навигации:** При обсуждении определенного свойства навигации этот термин относится к свойству навигации на другом конце связи.</span><span class="sxs-lookup"><span data-stu-id="7fa84-120">**Inverse navigation property:** When discussing a particular navigation property, this term refers to the navigation property on the other end of the relationship.</span></span>
  
* <span data-ttu-id="7fa84-121">**Связь, ссылающаяся на себя:** Связь, в которой зависимые и основные типы сущностей совпадают.</span><span class="sxs-lookup"><span data-stu-id="7fa84-121">**Self-referencing relationship:** A relationship in which the dependent and the principal entity types are the same.</span></span>

<span data-ttu-id="7fa84-122">В следующем коде показана связь «один ко многим» между `Blog` и `Post`</span><span class="sxs-lookup"><span data-stu-id="7fa84-122">The following code shows a one-to-many relationship between `Blog` and `Post`</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Relationships/Full.cs#Entities)]

* <span data-ttu-id="7fa84-123">`Post` является зависимой сущностью</span><span class="sxs-lookup"><span data-stu-id="7fa84-123">`Post` is the dependent entity</span></span>

* <span data-ttu-id="7fa84-124">`Blog` является субъектом-участником</span><span class="sxs-lookup"><span data-stu-id="7fa84-124">`Blog` is the principal entity</span></span>

* <span data-ttu-id="7fa84-125">`Post.BlogId` является внешним ключом</span><span class="sxs-lookup"><span data-stu-id="7fa84-125">`Post.BlogId` is the foreign key</span></span>

* <span data-ttu-id="7fa84-126">`Blog.BlogId` является основным ключом (в данном случае это первичный ключ, а не альтернативный ключ);</span><span class="sxs-lookup"><span data-stu-id="7fa84-126">`Blog.BlogId` is the principal key (in this case it is a primary key rather than an alternate key)</span></span>

* <span data-ttu-id="7fa84-127">`Post.Blog` является свойством навигации по ссылке</span><span class="sxs-lookup"><span data-stu-id="7fa84-127">`Post.Blog` is a reference navigation property</span></span>

* <span data-ttu-id="7fa84-128">`Blog.Posts` является свойством навигации коллекции</span><span class="sxs-lookup"><span data-stu-id="7fa84-128">`Blog.Posts` is a collection navigation property</span></span>

* <span data-ttu-id="7fa84-129">`Post.Blog` является свойством обратной навигации `Blog.Posts` (и наоборот)</span><span class="sxs-lookup"><span data-stu-id="7fa84-129">`Post.Blog` is the inverse navigation property of `Blog.Posts` (and vice versa)</span></span>

## <a name="conventions"></a><span data-ttu-id="7fa84-130">Обозначения</span><span class="sxs-lookup"><span data-stu-id="7fa84-130">Conventions</span></span>

<span data-ttu-id="7fa84-131">По умолчанию связь будет создана при обнаружении свойства навигации для типа.</span><span class="sxs-lookup"><span data-stu-id="7fa84-131">By default, a relationship will be created when there is a navigation property discovered on a type.</span></span> <span data-ttu-id="7fa84-132">Свойство считается свойством навигации, если тип, на который он указывает, не может быть сопоставлен с текущим поставщиком базы данных как скалярный тип.</span><span class="sxs-lookup"><span data-stu-id="7fa84-132">A property is considered a navigation property if the type it points to can not be mapped as a scalar type by the current database provider.</span></span>

> [!NOTE]  
> <span data-ttu-id="7fa84-133">Связи, обнаруженные соглашением, всегда будут указывать первичный ключ основной сущности.</span><span class="sxs-lookup"><span data-stu-id="7fa84-133">Relationships that are discovered by convention will always target the primary key of the principal entity.</span></span> <span data-ttu-id="7fa84-134">Чтобы выбрать альтернативный ключ, необходимо выполнить дополнительную настройку с помощью API Fluent.</span><span class="sxs-lookup"><span data-stu-id="7fa84-134">To target an alternate key, additional configuration must be performed using the Fluent API.</span></span>

### <a name="fully-defined-relationships"></a><span data-ttu-id="7fa84-135">Полностью определенные связи</span><span class="sxs-lookup"><span data-stu-id="7fa84-135">Fully defined relationships</span></span>

<span data-ttu-id="7fa84-136">Наиболее распространенным шаблоном для связей является наличие свойств навигации, определенных на обоих концах связи, и свойства внешнего ключа, определенного в зависимом классе сущности.</span><span class="sxs-lookup"><span data-stu-id="7fa84-136">The most common pattern for relationships is to have navigation properties defined on both ends of the relationship and a foreign key property defined in the dependent entity class.</span></span>

* <span data-ttu-id="7fa84-137">Если между двумя типами обнаружена пара свойств навигации, они будут настроены как обратные свойства навигации одной и той же связи.</span><span class="sxs-lookup"><span data-stu-id="7fa84-137">If a pair of navigation properties is found between two types, then they will be configured as inverse navigation properties of the same relationship.</span></span>

* <span data-ttu-id="7fa84-138">Если зависимая сущность содержит свойство с именем, математическим одним из этих шаблонов, оно будет настроено как внешний ключ:</span><span class="sxs-lookup"><span data-stu-id="7fa84-138">If the dependent entity contains a property with a name mathing one of these patterns then it will be configured as the foreign key:</span></span>
  * `<navigation property name><principal key property name>`
  * `<navigation property name>Id`
  * `<principal entity name><principal key property name>`
  * `<principal entity name>Id`

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Relationships/Full.cs?name=Entities&highlight=6,15,16)]

<span data-ttu-id="7fa84-139">В этом примере выделенные свойства будут использоваться для настройки связи.</span><span class="sxs-lookup"><span data-stu-id="7fa84-139">In this example the highlighted properties will be used to configure the relationship.</span></span>

> [!NOTE]
> <span data-ttu-id="7fa84-140">Если свойство является первичным ключом или имеет тип, несовместимый с ключом субъекта, он не будет настроен в качестве внешнего ключа.</span><span class="sxs-lookup"><span data-stu-id="7fa84-140">If the property is the primary key or is of a type not compatible with the principal key then it won't be configured as the foreign key.</span></span>

> [!NOTE]
> <span data-ttu-id="7fa84-141">До EF Core 3,0 свойство с именем, точно совпадающее со свойством ключа субъекта, [также было сопоставлено с внешним ключом](https://github.com/aspnet/EntityFrameworkCore/issues/13274) .</span><span class="sxs-lookup"><span data-stu-id="7fa84-141">Before EF Core 3.0 the property named exactly the same as the principal key property [was also matched as the foreign key](https://github.com/aspnet/EntityFrameworkCore/issues/13274)</span></span>

### <a name="no-foreign-key-property"></a><span data-ttu-id="7fa84-142">Нет свойства внешнего ключа</span><span class="sxs-lookup"><span data-stu-id="7fa84-142">No foreign key property</span></span>

<span data-ttu-id="7fa84-143">Хотя рекомендуется использовать свойство внешнего ключа, определенное в зависимом классе сущности, оно не является обязательным.</span><span class="sxs-lookup"><span data-stu-id="7fa84-143">While it is recommended to have a foreign key property defined in the dependent entity class, it is not required.</span></span> <span data-ttu-id="7fa84-144">Если свойство внешнего ключа не найдено, [свойство теневого внешнего ключа](shadow-properties.md) будет введено с именем `<navigation property name><principal key property name>` или `<principal entity name><principal key property name>`, если в зависимом типе отсутствует Навигация.</span><span class="sxs-lookup"><span data-stu-id="7fa84-144">If no foreign key property is found, a [shadow foreign key property](shadow-properties.md) will be introduced with the name `<navigation property name><principal key property name>` or `<principal entity name><principal key property name>` if no navigation is present on the dependent type.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Relationships/NoForeignKey.cs?name=Entities&highlight=6,15)]

<span data-ttu-id="7fa84-145">В этом примере теневым внешним ключом является `BlogId`, так как имя навигации должно быть избыточным.</span><span class="sxs-lookup"><span data-stu-id="7fa84-145">In this example the shadow foreign key is `BlogId` because prepending the navigation name would be redundant.</span></span>

> [!NOTE]
> <span data-ttu-id="7fa84-146">Если свойство с таким именем уже существует, имя теневого свойства будет суффиксом с номером.</span><span class="sxs-lookup"><span data-stu-id="7fa84-146">If a property with the same name already exists then the shadow property name will be suffixed with a number.</span></span>

### <a name="single-navigation-property"></a><span data-ttu-id="7fa84-147">Одно свойство навигации</span><span class="sxs-lookup"><span data-stu-id="7fa84-147">Single navigation property</span></span>

<span data-ttu-id="7fa84-148">Включение только одного свойства навигации (без обратной навигации и свойства внешнего ключа) достаточно для того, чтобы иметь связь, определенную по соглашению.</span><span class="sxs-lookup"><span data-stu-id="7fa84-148">Including just one navigation property (no inverse navigation, and no foreign key property) is enough to have a relationship defined by convention.</span></span> <span data-ttu-id="7fa84-149">Можно также иметь одно свойство навигации и внешнее ключевое свойство.</span><span class="sxs-lookup"><span data-stu-id="7fa84-149">You can also have a single navigation property and a foreign key property.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Relationships/OneNavigation.cs?name=Entities&highlight=6)]

### <a name="limitations"></a><span data-ttu-id="7fa84-150">Ограничения</span><span class="sxs-lookup"><span data-stu-id="7fa84-150">Limitations</span></span>

<span data-ttu-id="7fa84-151">Если между двумя типами определено несколько свойств навигации (то есть более одной пары переходов, которые указывают друг на друга), связи, представленные свойствами навигации, являются неоднозначными.</span><span class="sxs-lookup"><span data-stu-id="7fa84-151">When there are multiple navigation properties defined between two types (that is, more than just one pair of navigations that point to each other) the relationships represented by the navigation properties are ambiguous.</span></span> <span data-ttu-id="7fa84-152">Их необходимо настроить вручную для устранения неоднозначности.</span><span class="sxs-lookup"><span data-stu-id="7fa84-152">You will need to manually configure them to resolve the ambiguity.</span></span>

### <a name="cascade-delete"></a><span data-ttu-id="7fa84-153">Каскадное удаление</span><span class="sxs-lookup"><span data-stu-id="7fa84-153">Cascade delete</span></span>

<span data-ttu-id="7fa84-154">По соглашению каскадное удаление будет установлено в *CASCADE* для требуемых связей и *клиентсетнулл* для необязательных связей.</span><span class="sxs-lookup"><span data-stu-id="7fa84-154">By convention, cascade delete will be set to *Cascade* for required relationships and *ClientSetNull* for optional relationships.</span></span> <span data-ttu-id="7fa84-155">*CASCADE* означает также удаление зависимых сущностей.</span><span class="sxs-lookup"><span data-stu-id="7fa84-155">*Cascade* means dependent entities are also deleted.</span></span> <span data-ttu-id="7fa84-156">*Клиентсетнулл* означает, что зависимые сущности, которые не загружены в память, останутся неизменными и должны быть удалены вручную или обновлены, чтобы они указывали на действительную сущность Principal.</span><span class="sxs-lookup"><span data-stu-id="7fa84-156">*ClientSetNull* means that dependent entities that are not loaded into memory will remain unchanged and must be manually deleted, or updated to point to a valid principal entity.</span></span> <span data-ttu-id="7fa84-157">Для сущностей, загруженных в память, EF Core попытается установить свойства внешнего ключа в значение null.</span><span class="sxs-lookup"><span data-stu-id="7fa84-157">For entities that are loaded into memory, EF Core will attempt to set the foreign key properties to null.</span></span>

<span data-ttu-id="7fa84-158">Различия между обязательными и дополнительными связями см. в разделе [обязательные и дополнительные связи](#required-and-optional-relationships) .</span><span class="sxs-lookup"><span data-stu-id="7fa84-158">See the [Required and Optional Relationships](#required-and-optional-relationships) section for the difference between required and optional relationships.</span></span>

<span data-ttu-id="7fa84-159">Дополнительные сведения о различных поведениях при удалении и значения по умолчанию, используемые по соглашению, см. в разделе [каскадное удаление](../saving/cascade-delete.md) .</span><span class="sxs-lookup"><span data-stu-id="7fa84-159">See [Cascade Delete](../saving/cascade-delete.md) for more details about the different delete behaviors and the defaults used by convention.</span></span>

## <a name="manual-configuration"></a><span data-ttu-id="7fa84-160">Ручная настройка</span><span class="sxs-lookup"><span data-stu-id="7fa84-160">Manual configuration</span></span>

#### <a name="fluent-apitabfluent-api"></a>[<span data-ttu-id="7fa84-161">API Fluent</span><span class="sxs-lookup"><span data-stu-id="7fa84-161">Fluent API</span></span>](#tab/fluent-api)

<span data-ttu-id="7fa84-162">Чтобы настроить связь в API Fluent, начните с определения свойств навигации, составляющих связь.</span><span class="sxs-lookup"><span data-stu-id="7fa84-162">To configure a relationship in the Fluent API, you start by identifying the navigation properties that make up the relationship.</span></span> <span data-ttu-id="7fa84-163">`HasOne` или `HasMany` определяет свойство навигации для типа сущности, на котором начинается настройка.</span><span class="sxs-lookup"><span data-stu-id="7fa84-163">`HasOne` or `HasMany` identifies the navigation property on the entity type you are beginning the configuration on.</span></span> <span data-ttu-id="7fa84-164">Затем можно связать вызов `WithOne` или `WithMany`, чтобы обозначить обратную навигацию.</span><span class="sxs-lookup"><span data-stu-id="7fa84-164">You then chain a call to `WithOne` or `WithMany` to identify the inverse navigation.</span></span> <span data-ttu-id="7fa84-165">`HasOne`/`WithOne` используются для свойств навигации по ссылке и `HasMany`/`WithMany` используются для свойств навигации по коллекциям.</span><span class="sxs-lookup"><span data-stu-id="7fa84-165">`HasOne`/`WithOne` are used for reference navigation properties and `HasMany`/`WithMany` are used for collection navigation properties.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/NoForeignKey.cs?highlight=14-16)]

#### <a name="data-annotationstabdata-annotations"></a>[<span data-ttu-id="7fa84-166">Заметки к данным</span><span class="sxs-lookup"><span data-stu-id="7fa84-166">Data annotations</span></span>](#tab/data-annotations)

<span data-ttu-id="7fa84-167">Заметки к данным можно использовать для настройки способа связывания свойств навигации в зависимых и субъектах сущностей.</span><span class="sxs-lookup"><span data-stu-id="7fa84-167">You can use the Data Annotations to configure how navigation properties on the dependent and principal entities pair up.</span></span> <span data-ttu-id="7fa84-168">Обычно это делается при наличии более одной пары свойств навигации между двумя типами сущностей.</span><span class="sxs-lookup"><span data-stu-id="7fa84-168">This is typically done when there is more than one pair of navigation properties between two entity types.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Relationships/InverseProperty.cs?highlight=33,36)]

> [!NOTE]
> <span data-ttu-id="7fa84-169">[Обязательное значение] можно использовать только для свойств зависимой сущности, чтобы повлиять на требуемую связь.</span><span class="sxs-lookup"><span data-stu-id="7fa84-169">You can only use [Required] on properties on the dependent entity to impact the requiredness of the relationship.</span></span> <span data-ttu-id="7fa84-170">[Обязательное значение] в переходе от основной сущности обычно игнорируется, но может привести к тому, что сущность будет зависимой от нее.</span><span class="sxs-lookup"><span data-stu-id="7fa84-170">[Required] on the navigation from the principal entity is usually ignored, but it may cause the entity to become the dependent one.</span></span>

> [!NOTE]
> <span data-ttu-id="7fa84-171">Заметки к данным `[ForeignKey]` и `[InverseProperty]` доступны в пространстве имен `System.ComponentModel.DataAnnotations.Schema`.</span><span class="sxs-lookup"><span data-stu-id="7fa84-171">The data annotations `[ForeignKey]` and `[InverseProperty]` are available in the `System.ComponentModel.DataAnnotations.Schema` namespace.</span></span> <span data-ttu-id="7fa84-172">`[Required]` доступен в пространстве имен `System.ComponentModel.DataAnnotations`.</span><span class="sxs-lookup"><span data-stu-id="7fa84-172">`[Required]` is available in the `System.ComponentModel.DataAnnotations` namespace.</span></span>

---

### <a name="single-navigation-property"></a><span data-ttu-id="7fa84-173">Одно свойство навигации</span><span class="sxs-lookup"><span data-stu-id="7fa84-173">Single navigation property</span></span>

<span data-ttu-id="7fa84-174">Если у вас есть только одно свойство навигации, то существуют перегрузки без параметров `WithOne` и `WithMany`.</span><span class="sxs-lookup"><span data-stu-id="7fa84-174">If you only have one navigation property then there are parameterless overloads of `WithOne` and `WithMany`.</span></span> <span data-ttu-id="7fa84-175">Это указывает на то, что на другом конце связи имеется концептуальная ссылка или коллекция, но в классе сущностей не содержится свойство навигации.</span><span class="sxs-lookup"><span data-stu-id="7fa84-175">This indicates that there is conceptually a reference or collection on the other end of the relationship, but there is no navigation property included in the entity class.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/OneNavigation.cs?highlight=14-16)]

### <a name="foreign-key"></a><span data-ttu-id="7fa84-176">Внешний ключ</span><span class="sxs-lookup"><span data-stu-id="7fa84-176">Foreign key</span></span>

#### <a name="fluent-apitabfluent-api"></a>[<span data-ttu-id="7fa84-177">API Fluent</span><span class="sxs-lookup"><span data-stu-id="7fa84-177">Fluent API</span></span>](#tab/fluent-api)

<span data-ttu-id="7fa84-178">API-интерфейс Fluent можно использовать для настройки того, какое свойство должно использоваться в качестве свойства внешнего ключа для данной связи.</span><span class="sxs-lookup"><span data-stu-id="7fa84-178">You can use the Fluent API to configure which property should be used as the foreign key property for a given relationship.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/ForeignKey.cs?highlight=17)]

#### <a name="data-annotationstabdata-annotations"></a>[<span data-ttu-id="7fa84-179">Заметки к данным</span><span class="sxs-lookup"><span data-stu-id="7fa84-179">Data annotations</span></span>](#tab/data-annotations)

<span data-ttu-id="7fa84-180">Заметки к данным можно использовать для настройки того, какое свойство должно использоваться в качестве свойства внешнего ключа для данной связи.</span><span class="sxs-lookup"><span data-stu-id="7fa84-180">You can use the Data Annotations to configure which property should be used as the foreign key property for a given relationship.</span></span> <span data-ttu-id="7fa84-181">Обычно это делается, когда свойство внешнего ключа не обнаруживается по соглашению.</span><span class="sxs-lookup"><span data-stu-id="7fa84-181">This is typically done when the foreign key property is not discovered by convention.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Relationships/ForeignKey.cs?highlight=30)]

> [!TIP]  
> <span data-ttu-id="7fa84-182">`[ForeignKey]` заметку можно поместить в любое свойство навигации в связи.</span><span class="sxs-lookup"><span data-stu-id="7fa84-182">The `[ForeignKey]` annotation can be placed on either navigation property in the relationship.</span></span> <span data-ttu-id="7fa84-183">Не требуется переходить к свойству навигации в классе зависимой сущности.</span><span class="sxs-lookup"><span data-stu-id="7fa84-183">It does not need to go on the navigation property in the dependent entity class.</span></span>

> [!NOTE]
> <span data-ttu-id="7fa84-184">Свойство, указанное с помощью `[ForeignKey]` свойства навигации, не обязательно должно существовать в зависимом типе.</span><span class="sxs-lookup"><span data-stu-id="7fa84-184">The property specified using `[ForeignKey]` on a navigation property doesn't need to exist the the dependent type.</span></span> <span data-ttu-id="7fa84-185">В этом случае для создания теневого внешнего ключа будет использоваться указанное имя.</span><span class="sxs-lookup"><span data-stu-id="7fa84-185">In this case the specified name will be used to create a shadow foreign key.</span></span>

---

<span data-ttu-id="7fa84-186">В следующем примере кода показано, как настроить составной внешний ключ.</span><span class="sxs-lookup"><span data-stu-id="7fa84-186">The following code shows how to configure a composite foreign key.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/CompositeForeignKey.cs?highlight=20)]

<span data-ttu-id="7fa84-187">Можно использовать перегрузку строк `HasForeignKey(...)`, чтобы настроить свойство теневого копирования в качестве внешнего ключа (Дополнительные сведения см. в разделе [Свойства тени](shadow-properties.md) ).</span><span class="sxs-lookup"><span data-stu-id="7fa84-187">You can use the string overload of `HasForeignKey(...)` to configure a shadow property as a foreign key (see [Shadow Properties](shadow-properties.md) for more information).</span></span> <span data-ttu-id="7fa84-188">Рекомендуется явно добавить свойство Shadow в модель, прежде чем использовать ее как внешний ключ (как показано ниже).</span><span class="sxs-lookup"><span data-stu-id="7fa84-188">We recommend explicitly adding the shadow property to the model before using it as a foreign key (as shown below).</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/ShadowForeignKey.cs#Sample)]

### <a name="without-navigation-property"></a><span data-ttu-id="7fa84-189">Без свойства навигации</span><span class="sxs-lookup"><span data-stu-id="7fa84-189">Without navigation property</span></span>

<span data-ttu-id="7fa84-190">Указывать свойство навигации не обязательно.</span><span class="sxs-lookup"><span data-stu-id="7fa84-190">You don't necessarily need to provide a navigation property.</span></span> <span data-ttu-id="7fa84-191">Можно просто предоставить внешний ключ на одной стороне связи.</span><span class="sxs-lookup"><span data-stu-id="7fa84-191">You can simply provide a foreign key on one side of the relationship.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/NoNavigation.cs?highlight=14-17)]

### <a name="principal-key"></a><span data-ttu-id="7fa84-192">Основной ключ</span><span class="sxs-lookup"><span data-stu-id="7fa84-192">Principal key</span></span>

<span data-ttu-id="7fa84-193">Если требуется, чтобы внешний ключ ссылался на свойство, отличное от первичного ключа, можно использовать API Fluent, чтобы настроить свойство ключа субъекта для связи.</span><span class="sxs-lookup"><span data-stu-id="7fa84-193">If you want the foreign key to reference a property other than the primary key, you can use the Fluent API to configure the principal key property for the relationship.</span></span> <span data-ttu-id="7fa84-194">Свойство, настраиваемое в качестве ключа участника, будет автоматически настроено в качестве [альтернативного ключа](alternate-keys.md).</span><span class="sxs-lookup"><span data-stu-id="7fa84-194">The property that you configure as the principal key will automatically be setup as an [alternate key](alternate-keys.md).</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/PrincipalKey.cs?name=PrincipalKey&highlight=11)]

<span data-ttu-id="7fa84-195">В следующем примере кода показано, как настроить составной ключ участника.</span><span class="sxs-lookup"><span data-stu-id="7fa84-195">The following code shows how to configure a composite principal key.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/CompositePrincipalKey.cs?name=Composite&highlight=11)]

> [!WARNING]  
> <span data-ttu-id="7fa84-196">Порядок указания свойств ключа субъекта должен совпадать с порядком, в котором они указаны для внешнего ключа.</span><span class="sxs-lookup"><span data-stu-id="7fa84-196">The order in which you specify principal key properties must match the order in which they are specified for the foreign key.</span></span>

### <a name="required-and-optional-relationships"></a><span data-ttu-id="7fa84-197">Обязательные и необязательные связи</span><span class="sxs-lookup"><span data-stu-id="7fa84-197">Required and optional relationships</span></span>

<span data-ttu-id="7fa84-198">С помощью API-интерфейса Fluent можно настроить, является ли связь обязательной или необязательной.</span><span class="sxs-lookup"><span data-stu-id="7fa84-198">You can use the Fluent API to configure whether the relationship is required or optional.</span></span> <span data-ttu-id="7fa84-199">В конечном итоге это определяет, является ли свойство внешнего ключа обязательным или необязательным.</span><span class="sxs-lookup"><span data-stu-id="7fa84-199">Ultimately this controls whether the foreign key property is required or optional.</span></span> <span data-ttu-id="7fa84-200">Это наиболее полезно при использовании внешнего ключа теневого состояния.</span><span class="sxs-lookup"><span data-stu-id="7fa84-200">This is most useful when you are using a shadow state foreign key.</span></span> <span data-ttu-id="7fa84-201">Если у вас есть свойство внешнего ключа в классе сущностей, обязательность связи определяется на основе того, является ли свойство внешнего ключа обязательным или необязательным (Дополнительные сведения см. в разделе [обязательные и необязательные свойства](required-optional.md) ).</span><span class="sxs-lookup"><span data-stu-id="7fa84-201">If you have a foreign key property in your entity class then the requiredness of the relationship is determined based on whether the foreign key property is required or optional (see [Required and Optional properties](required-optional.md) for more information).</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/Required.cs?name=Required&highlight=11)]

> [!NOTE]
> <span data-ttu-id="7fa84-202">Вызов `IsRequired(false)` также делает свойство внешнего ключа необязательным, если оно не настроено в противном случае.</span><span class="sxs-lookup"><span data-stu-id="7fa84-202">Calling `IsRequired(false)` also makes the foreign key property optional unless it's configured otherwise.</span></span>

### <a name="cascade-delete"></a><span data-ttu-id="7fa84-203">Каскадное удаление</span><span class="sxs-lookup"><span data-stu-id="7fa84-203">Cascade delete</span></span>

<span data-ttu-id="7fa84-204">Для явной настройки поведения каскадного удаления для данной связи можно использовать API Fluent.</span><span class="sxs-lookup"><span data-stu-id="7fa84-204">You can use the Fluent API to configure the cascade delete behavior for a given relationship explicitly.</span></span>

<span data-ttu-id="7fa84-205">Подробное описание каждого варианта см. в разделе [каскадное удаление](../saving/cascade-delete.md) .</span><span class="sxs-lookup"><span data-stu-id="7fa84-205">See [Cascade Delete](../saving/cascade-delete.md) for a detailed discussion of each option.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/CascadeDelete.cs?name=CascadeDelete&highlight=11)]

## <a name="other-relationship-patterns"></a><span data-ttu-id="7fa84-206">Другие шаблоны отношений</span><span class="sxs-lookup"><span data-stu-id="7fa84-206">Other relationship patterns</span></span>

### <a name="one-to-one"></a><span data-ttu-id="7fa84-207">"Одна к одной"</span><span class="sxs-lookup"><span data-stu-id="7fa84-207">One-to-one</span></span>

<span data-ttu-id="7fa84-208">Связь «один к одному» имеет свойство навигации «ссылка» на обеих сторонах.</span><span class="sxs-lookup"><span data-stu-id="7fa84-208">One to one relationships have a reference navigation property on both sides.</span></span> <span data-ttu-id="7fa84-209">Они следуют тем же соглашениям, что и отношения "один ко многим", но уникальный индекс вводится в свойство внешнего ключа, чтобы гарантировать, что только один зависимый объект связан с каждым участником.</span><span class="sxs-lookup"><span data-stu-id="7fa84-209">They follow the same conventions as one-to-many relationships, but a unique index is introduced on the foreign key property to ensure only one dependent is related to each principal.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Relationships/OneToOne.cs?name=Property&highlight=6,15,16)]

> [!NOTE]  
> <span data-ttu-id="7fa84-210">EF выберет одну из сущностей, которая будет зависимой, исходя из возможности обнаружения свойства внешнего ключа.</span><span class="sxs-lookup"><span data-stu-id="7fa84-210">EF will choose one of the entities to be the dependent based on its ability to detect a foreign key property.</span></span> <span data-ttu-id="7fa84-211">Если в качестве зависимого объекта выбрана неверная сущность, можно исправить это с помощью API-интерфейса Fluent.</span><span class="sxs-lookup"><span data-stu-id="7fa84-211">If the wrong entity is chosen as the dependent, you can use the Fluent API to correct this.</span></span>

<span data-ttu-id="7fa84-212">При настройке связи с помощью API-интерфейса Fluent вы используете методы `HasOne` и `WithOne`.</span><span class="sxs-lookup"><span data-stu-id="7fa84-212">When configuring the relationship with the Fluent API, you use the `HasOne` and `WithOne` methods.</span></span>

<span data-ttu-id="7fa84-213">При настройке внешнего ключа необходимо указать тип зависимой сущности — Обратите внимание на универсальный параметр, предоставленный для `HasForeignKey` в списке ниже.</span><span class="sxs-lookup"><span data-stu-id="7fa84-213">When configuring the foreign key you need to specify the dependent entity type - notice the generic parameter provided to `HasForeignKey` in the listing below.</span></span> <span data-ttu-id="7fa84-214">В связи «один ко многим» ясно, что сущность с навигацией по ссылке является зависимой, а она является участником коллекции.</span><span class="sxs-lookup"><span data-stu-id="7fa84-214">In a one-to-many relationship it is clear that the entity with the reference navigation is the dependent and the one with the collection is the principal.</span></span> <span data-ttu-id="7fa84-215">Но это не так в связи «один к одному», поэтому необходимо явно определить его.</span><span class="sxs-lookup"><span data-stu-id="7fa84-215">But this is not so in a one-to-one relationship - hence the need to explicitly define it.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/OneToOne.cs?name=OneToOne&highlight=11)]

### <a name="many-to-many"></a><span data-ttu-id="7fa84-216">«Многие ко многим»</span><span class="sxs-lookup"><span data-stu-id="7fa84-216">Many-to-many</span></span>

<span data-ttu-id="7fa84-217">Связи «многие ко многим» без класса сущности для представления таблицы соединений пока не поддерживаются.</span><span class="sxs-lookup"><span data-stu-id="7fa84-217">Many-to-many relationships without an entity class to represent the join table are not yet supported.</span></span> <span data-ttu-id="7fa84-218">Однако можно представить связь «многие ко многим», включив класс сущности для таблицы Join и составляя две отдельные связи «один ко многим».</span><span class="sxs-lookup"><span data-stu-id="7fa84-218">However, you can represent a many-to-many relationship by including an entity class for the join table and mapping two separate one-to-many relationships.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Relationships/ManyToMany.cs?name=ManyToMany&highlight=11,12,13,14,16,17,18,19,39,40,41,42,43,44,45,46)]
