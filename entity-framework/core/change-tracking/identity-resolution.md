---
title: Разрешение идентификации — EF Core
description: Разрешение нескольких экземпляров сущности в один экземпляр с использованием значений первичного ключа
author: ajcvickers
ms.date: 12/30/2020
uid: core/change-tracking/identity-resolution
ms.openlocfilehash: d4c8f935c8d0ab92eaecd8fc7a4156bd824713d4
ms.sourcegitcommit: 704240349e18b6404e5a809f5b7c9d365b152e2e
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/16/2021
ms.locfileid: "100543618"
---
# <a name="identity-resolution-in-ef-core"></a><span data-ttu-id="e86e5-103">Разрешение идентификаторов в EF Core</span><span class="sxs-lookup"><span data-stu-id="e86e5-103">Identity Resolution in EF Core</span></span>

<span data-ttu-id="e86e5-104"><xref:Microsoft.EntityFrameworkCore.DbContext>Может отслеживаниь только одного экземпляра сущности с любым значением первичного ключа.</span><span class="sxs-lookup"><span data-stu-id="e86e5-104">A <xref:Microsoft.EntityFrameworkCore.DbContext> can only track one entity instance with any given primary key value.</span></span> <span data-ttu-id="e86e5-105">Это означает, что несколько экземпляров сущности с одинаковым значением ключа должны быть разрешены в один экземпляр.</span><span class="sxs-lookup"><span data-stu-id="e86e5-105">This means multiple instances of an entity with the same key value must be resolved to a single instance.</span></span> <span data-ttu-id="e86e5-106">Это называется разрешением идентификации.</span><span class="sxs-lookup"><span data-stu-id="e86e5-106">This is called "identity resolution".</span></span> <span data-ttu-id="e86e5-107">Разрешение идентификации гарантирует, Entity Framework Core (EF Core) отслеживает единообразные графы без неоднозначности относительно связей или значений свойств сущностей.</span><span class="sxs-lookup"><span data-stu-id="e86e5-107">Identity resolution ensures Entity Framework Core (EF Core) is tracking a consistent graph with no ambiguities about the relationships or property values of the entities.</span></span>

> [!TIP]
> <span data-ttu-id="e86e5-108">В этом документе предполагается, что состояния сущностей и основы отслеживания изменений EF Core понятны.</span><span class="sxs-lookup"><span data-stu-id="e86e5-108">This document assumes that entity states and the basics of EF Core change tracking are understood.</span></span> <span data-ttu-id="e86e5-109">Дополнительные сведения по этим темам см. [в разделе Отслеживание изменений в EF Core](xref:core/change-tracking/index) .</span><span class="sxs-lookup"><span data-stu-id="e86e5-109">See [Change Tracking in EF Core](xref:core/change-tracking/index) for more information on these topics.</span></span>

> [!TIP]
> <span data-ttu-id="e86e5-110">Вы можете запустить и отладить весь код, используемый в этой документации, [скачав пример кода из GitHub](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/ChangeTracking/IdentityResolutionInEFCore).</span><span class="sxs-lookup"><span data-stu-id="e86e5-110">You can run and debug into all the code in this document by [downloading the sample code from GitHub](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/ChangeTracking/IdentityResolutionInEFCore).</span></span>

## <a name="introduction"></a><span data-ttu-id="e86e5-111">Введение</span><span class="sxs-lookup"><span data-stu-id="e86e5-111">Introduction</span></span>

<span data-ttu-id="e86e5-112">Следующий код запрашивает сущность, а затем пытается присоединить другой экземпляр с тем же значением первичного ключа:</span><span class="sxs-lookup"><span data-stu-id="e86e5-112">The following code queries for an entity and then attempts to attach a different instance with the same primary key value:</span></span>

<!--
            using var context = new BlogsContext();

            var blogA = context.Blogs.Single(e => e.Id == 1);
            var blogB = new Blog { Id = 1, Name = ".NET Blog (All new!)" };

            try
            {
                context.Update(blogB); // This will throw
            }
            catch (Exception e)
            {
                Console.WriteLine(e);
            }
-->
[!code-csharp[Identity_Resolution_in_EF_Core_1](../../../samples/core/ChangeTracking/IdentityResolutionInEFCore/IdentityResolutionSamples.cs?name=Identity_Resolution_in_EF_Core_1)]

<span data-ttu-id="e86e5-113">Выполнение этого кода приводит к возникновению следующего исключения:</span><span class="sxs-lookup"><span data-stu-id="e86e5-113">Running this code results in the following exception:</span></span>

> <span data-ttu-id="e86e5-114">System. InvalidOperationException: экземпляр типа сущности "Blog" не может быть отслеживаем, так как уже отслеживается другой экземпляр со значением ключа "{ID: 1}".</span><span class="sxs-lookup"><span data-stu-id="e86e5-114">System.InvalidOperationException: The instance of entity type 'Blog' cannot be tracked because another instance with the key value '{Id: 1}' is already being tracked.</span></span> <span data-ttu-id="e86e5-115">При присоединении существующих сущностей убедитесь, что присоединен только один экземпляр сущности с заданным значением ключа.</span><span class="sxs-lookup"><span data-stu-id="e86e5-115">When attaching existing entities, ensure that only one entity instance with a given key value is attached.</span></span>

<span data-ttu-id="e86e5-116">EF Core требуется один экземпляр, так как:</span><span class="sxs-lookup"><span data-stu-id="e86e5-116">EF Core requires a single instance because:</span></span>

- <span data-ttu-id="e86e5-117">Значения свойств могут различаться для нескольких экземпляров.</span><span class="sxs-lookup"><span data-stu-id="e86e5-117">Property values may be different between multiple instances.</span></span> <span data-ttu-id="e86e5-118">При обновлении базы данных EF Core необходимо выяснить, какие значения свойств следует использовать.</span><span class="sxs-lookup"><span data-stu-id="e86e5-118">When updating the database, EF Core needs to know which property values to use.</span></span>
- <span data-ttu-id="e86e5-119">Связи с другими сущностями могут различаться между несколькими экземплярами.</span><span class="sxs-lookup"><span data-stu-id="e86e5-119">Relationships with other entities may be different between multiple instances.</span></span> <span data-ttu-id="e86e5-120">Например, "блог" может быть связан с другой коллекцией записей, чем "Блогб".</span><span class="sxs-lookup"><span data-stu-id="e86e5-120">For example, "blogA" may be related to a different collection of posts than "blogB".</span></span>

<span data-ttu-id="e86e5-121">Приведенное выше исключение часто встречается в следующих ситуациях:</span><span class="sxs-lookup"><span data-stu-id="e86e5-121">The exception above is commonly encountered in these situations:</span></span>

- <span data-ttu-id="e86e5-122">При попытке обновить сущность</span><span class="sxs-lookup"><span data-stu-id="e86e5-122">When attempting to update an entity</span></span>
- <span data-ttu-id="e86e5-123">При попытке отслеживании сериализованного графа сущностей</span><span class="sxs-lookup"><span data-stu-id="e86e5-123">When attempting to track a serialized graph of entities</span></span>
- <span data-ttu-id="e86e5-124">Не удается задать значение ключа, которое не создается автоматически</span><span class="sxs-lookup"><span data-stu-id="e86e5-124">When failing to set a key value that is not automatically generated</span></span>
- <span data-ttu-id="e86e5-125">При повторном использовании экземпляра DbContext для нескольких единиц работы</span><span class="sxs-lookup"><span data-stu-id="e86e5-125">When reusing a DbContext instance for multiple units-of-work</span></span>

<span data-ttu-id="e86e5-126">Каждая из этих ситуаций обсуждается в следующих разделах.</span><span class="sxs-lookup"><span data-stu-id="e86e5-126">Each of these situations is discussed in the following sections.</span></span>

## <a name="updating-an-entity"></a><span data-ttu-id="e86e5-127">Обновление сущности</span><span class="sxs-lookup"><span data-stu-id="e86e5-127">Updating an entity</span></span>

<span data-ttu-id="e86e5-128">Существует несколько различных подходов к обновлению сущности новыми значениями, как описано в [Отслеживание изменений в EF Core](xref:core/change-tracking/index) и [явном отслеживании сущностей](xref:core/change-tracking/explicit-tracking).</span><span class="sxs-lookup"><span data-stu-id="e86e5-128">There are several different approaches to update an entity with new values, as covered in [Change Tracking in EF Core](xref:core/change-tracking/index) and [Explicitly Tracking Entities](xref:core/change-tracking/explicit-tracking).</span></span> <span data-ttu-id="e86e5-129">Эти подходы описаны ниже в контексте разрешения идентификации.</span><span class="sxs-lookup"><span data-stu-id="e86e5-129">These approaches are outlined below in the context of identity resolution.</span></span> <span data-ttu-id="e86e5-130">Важно отметить, что каждый из подходов использует либо запрос, либо вызов одного из `Update` или `Attach` , но **_никогда не оба_**.</span><span class="sxs-lookup"><span data-stu-id="e86e5-130">An important point to notice is that each of the approaches use either a query or a call to one of `Update` or `Attach`, but **_never both_**.</span></span>

### <a name="call-update"></a><span data-ttu-id="e86e5-131">Вызов обновления</span><span class="sxs-lookup"><span data-stu-id="e86e5-131">Call Update</span></span>

<span data-ttu-id="e86e5-132">Часто обновляемая сущность не поступает из запроса к DbContext, который мы будем использовать для SaveChanges.</span><span class="sxs-lookup"><span data-stu-id="e86e5-132">Often the entity to update does not come from a query on the DbContext that we are going to use for SaveChanges.</span></span> <span data-ttu-id="e86e5-133">Например, в веб-приложении экземпляр сущности может быть создан на основе информации в запросе POST.</span><span class="sxs-lookup"><span data-stu-id="e86e5-133">For example, in a web application, an entity instance may be created from the information in a POST request.</span></span> <span data-ttu-id="e86e5-134">Самый простой способ обработать это — использовать <xref:Microsoft.EntityFrameworkCore.DbContext.Update%2A?displayProperty=nameWithType> или <xref:Microsoft.EntityFrameworkCore.DbSet%601.Update%2A?displayProperty=nameWithType> .</span><span class="sxs-lookup"><span data-stu-id="e86e5-134">The simplest way to handle this is to use <xref:Microsoft.EntityFrameworkCore.DbContext.Update%2A?displayProperty=nameWithType> or <xref:Microsoft.EntityFrameworkCore.DbSet%601.Update%2A?displayProperty=nameWithType>.</span></span> <span data-ttu-id="e86e5-135">Например:</span><span class="sxs-lookup"><span data-stu-id="e86e5-135">For example:</span></span>

<!--
    public static void UpdateFromHttpPost1(Blog blog)
    {
        using var context = new BlogsContext();

        context.Update(blog);

        context.SaveChanges();
    }
-->
[!code-csharp[Updating_an_entity_1](../../../samples/core/ChangeTracking/IdentityResolutionInEFCore/IdentityResolutionSamples.cs?name=Updating_an_entity_1)]

<span data-ttu-id="e86e5-136">В данном случае:</span><span class="sxs-lookup"><span data-stu-id="e86e5-136">In this case:</span></span>

- <span data-ttu-id="e86e5-137">Создается только один экземпляр сущности.</span><span class="sxs-lookup"><span data-stu-id="e86e5-137">Only a single instance of the entity is created.</span></span>
- <span data-ttu-id="e86e5-138">Экземпляр сущности **не** запрашивается из базы данных в процессе выполнения обновления.</span><span class="sxs-lookup"><span data-stu-id="e86e5-138">The entity instance is **not** queried from the database as part of making the update.</span></span>
- <span data-ttu-id="e86e5-139">Все значения свойств будут обновлены в базе данных независимо от того, изменились ли они.</span><span class="sxs-lookup"><span data-stu-id="e86e5-139">All property values will be updated in the database, regardless of whether they have actually changed or not.</span></span>
- <span data-ttu-id="e86e5-140">Выполняется цикл обработки одной базы данных.</span><span class="sxs-lookup"><span data-stu-id="e86e5-140">One database round-trip is made.</span></span>

### <a name="query-then-apply-changes"></a><span data-ttu-id="e86e5-141">Запрос, затем применить изменения</span><span class="sxs-lookup"><span data-stu-id="e86e5-141">Query then apply changes</span></span>

<span data-ttu-id="e86e5-142">Обычно неизвестно, какие значения свойств фактически были изменены при создании сущности на основе информации в запросе POST или аналогичной.</span><span class="sxs-lookup"><span data-stu-id="e86e5-142">Usually it is not known which property values have actually been changed when an entity is created from information in a POST request or similar.</span></span> <span data-ttu-id="e86e5-143">Часто можно просто обновить все значения в базе данных, как в предыдущем примере.</span><span class="sxs-lookup"><span data-stu-id="e86e5-143">Often it is fine to just update all values in the database, as we did in the previous example.</span></span> <span data-ttu-id="e86e5-144">Однако если приложение обрабатывает много сущностей, а только небольшое число содержит фактические изменения, может быть полезно ограничить количество отправленных обновлений.</span><span class="sxs-lookup"><span data-stu-id="e86e5-144">However, if the application is handling many entities and only a small number of those have actual changes, then it may be useful to limit the updates sent.</span></span> <span data-ttu-id="e86e5-145">Это можно сделать, выполнив запрос для учета сущностей, которые в настоящее время существуют в базе данных, а затем применяя изменения к этим отслеживающим сущностям.</span><span class="sxs-lookup"><span data-stu-id="e86e5-145">This can be achieved by executing a query to track the entities as they currently exist in the database, and then applying changes to these tracked entities.</span></span> <span data-ttu-id="e86e5-146">Например:</span><span class="sxs-lookup"><span data-stu-id="e86e5-146">For example:</span></span>

<!--
    public static void UpdateFromHttpPost2(Blog blog)
    {
        using var context = new BlogsContext();

        var trackedBlog = context.Blogs.Find(blog.Id);

        trackedBlog.Name = blog.Name;
        trackedBlog.Summary = blog.Summary;

        context.SaveChanges();
    }
-->
[!code-csharp[Updating_an_entity_2](../../../samples/core/ChangeTracking/IdentityResolutionInEFCore/IdentityResolutionSamples.cs?name=Updating_an_entity_2)]

<span data-ttu-id="e86e5-147">В данном случае:</span><span class="sxs-lookup"><span data-stu-id="e86e5-147">In this case:</span></span>

- <span data-ttu-id="e86e5-148">Отслеживание будет только один экземпляр сущности. Объект, возвращаемый запросом из базы данных `Find` .</span><span class="sxs-lookup"><span data-stu-id="e86e5-148">Only a single instance of the entity is tracked; the one that is returned from the database by the `Find` query.</span></span>
- <span data-ttu-id="e86e5-149">`Update`, `Attach` и т. д. **не** используются.</span><span class="sxs-lookup"><span data-stu-id="e86e5-149">`Update`, `Attach`, etc. are **not** used.</span></span>
- <span data-ttu-id="e86e5-150">В базе данных обновляются только значения свойств, которые действительно изменились.</span><span class="sxs-lookup"><span data-stu-id="e86e5-150">Only property values that have actually changed are updated in the database.</span></span>
- <span data-ttu-id="e86e5-151">Выполняется два круговых обращения к базе данных.</span><span class="sxs-lookup"><span data-stu-id="e86e5-151">Two database round-trips are made.</span></span>

<span data-ttu-id="e86e5-152">EF Core имеет некоторые вспомогательные методы для передачи значений свойств следующим образом.</span><span class="sxs-lookup"><span data-stu-id="e86e5-152">EF Core has some helpers for transferring property values like this.</span></span> <span data-ttu-id="e86e5-153">Например, <xref:Microsoft.EntityFrameworkCore.ChangeTracking.PropertyValues.SetValues%2A?displayProperty=nameWithType> копирует все значения из заданного объекта и настраивает их для объекта, на который производится запись:</span><span class="sxs-lookup"><span data-stu-id="e86e5-153">For example, <xref:Microsoft.EntityFrameworkCore.ChangeTracking.PropertyValues.SetValues%2A?displayProperty=nameWithType> will copy all the values from the given object and set them on the tracked object:</span></span>

<!--
    public static void UpdateFromHttpPost3(Blog blog)
    {
        using var context = new BlogsContext();

        var trackedBlog = context.Blogs.Find(blog.Id);

        context.Entry(trackedBlog).CurrentValues.SetValues(blog);

        context.SaveChanges();
    }
-->
[!code-csharp[Updating_an_entity_3](../../../samples/core/ChangeTracking/IdentityResolutionInEFCore/IdentityResolutionSamples.cs?name=Updating_an_entity_3)]

<span data-ttu-id="e86e5-154">`SetValues` принимает различные типы объектов, в том числе объекты перемещения данных (DTO), с именами свойств, соответствующими свойствам типа сущности.</span><span class="sxs-lookup"><span data-stu-id="e86e5-154">`SetValues` accepts various object types, including data transfer objects (DTOs) with property names that match the properties of the entity type.</span></span> <span data-ttu-id="e86e5-155">Например:</span><span class="sxs-lookup"><span data-stu-id="e86e5-155">For example:</span></span>

<!--
    public static void UpdateFromHttpPost4(BlogDto dto)
    {
        using var context = new BlogsContext();

        var trackedBlog = context.Blogs.Find(dto.Id);

        context.Entry(trackedBlog).CurrentValues.SetValues(dto);

        context.SaveChanges();
    }
-->
[!code-csharp[Updating_an_entity_4](../../../samples/core/ChangeTracking/IdentityResolutionInEFCore/IdentityResolutionSamples.cs?name=Updating_an_entity_4)]

<span data-ttu-id="e86e5-156">Или словарь с записями "имя-значение" для значений свойств:</span><span class="sxs-lookup"><span data-stu-id="e86e5-156">Or a dictionary with name/value entries for the property values:</span></span>

<!--
    public static void UpdateFromHttpPost5(Dictionary<string, object> propertyValues)
    {
        using var context = new BlogsContext();

        var trackedBlog = context.Blogs.Find(propertyValues["Id"]);

        context.Entry(trackedBlog).CurrentValues.SetValues(propertyValues);

        context.SaveChanges();
    }
-->
[!code-csharp[Updating_an_entity_5](../../../samples/core/ChangeTracking/IdentityResolutionInEFCore/IdentityResolutionSamples.cs?name=Updating_an_entity_5)]

<span data-ttu-id="e86e5-157">Дополнительные сведения о работе со значениями свойств см. в разделе [доступ к отслеживающим сущностям](xref:core/change-tracking/entity-entries) .</span><span class="sxs-lookup"><span data-stu-id="e86e5-157">See [Accessing tracked entities](xref:core/change-tracking/entity-entries) for more information on working with property values like this.</span></span>

### <a name="use-original-values"></a><span data-ttu-id="e86e5-158">Использовать исходные значения</span><span class="sxs-lookup"><span data-stu-id="e86e5-158">Use original values</span></span>

<span data-ttu-id="e86e5-159">До сих пор каждый подход либо выполнил запрос перед выполнением обновления, либо обновил все значения свойств независимо от того, изменились ли они.</span><span class="sxs-lookup"><span data-stu-id="e86e5-159">So far each approach has either executed a query before making the update, or updated all property values regardless of whether or not they have changed.</span></span> <span data-ttu-id="e86e5-160">Для обновления только тех значений, которые были изменены без запроса в рамках обновления, требуются конкретные сведения об изменении значений свойств.</span><span class="sxs-lookup"><span data-stu-id="e86e5-160">To update only values that have changed without querying as part of the update requires specific information about which property values have changed.</span></span> <span data-ttu-id="e86e5-161">Распространенным способом получения этой информации является отправка текущих и исходных значений в POST HTTP или аналогичной операции.</span><span class="sxs-lookup"><span data-stu-id="e86e5-161">A common way to get this information is to send back both the current and original values in the HTTP Post or similar.</span></span> <span data-ttu-id="e86e5-162">Например:</span><span class="sxs-lookup"><span data-stu-id="e86e5-162">For example:</span></span>

<!--
    public static void UpdateFromHttpPost6(Blog blog, Dictionary<string, object> originalValues)
    {
        using var context = new BlogsContext();

        context.Attach(blog);
        context.Entry(blog).OriginalValues.SetValues(originalValues);

        context.SaveChanges();
    }
-->
[!code-csharp[Updating_an_entity_6](../../../samples/core/ChangeTracking/IdentityResolutionInEFCore/IdentityResolutionSamples.cs?name=Updating_an_entity_6)]

<span data-ttu-id="e86e5-163">В этом коде сначала присоединяется сущность с измененными значениями.</span><span class="sxs-lookup"><span data-stu-id="e86e5-163">In this code the entity with modified values is first attached.</span></span> <span data-ttu-id="e86e5-164">Это приводит к тому, что EF Core отслеживанию сущности в `Unchanged` состоянии, то есть без значений свойств, помеченных как измененные.</span><span class="sxs-lookup"><span data-stu-id="e86e5-164">This causes EF Core to track the entity in the `Unchanged` state; that is, with no property values marked as modified.</span></span> <span data-ttu-id="e86e5-165">Затем словарь исходных значений применяется к этой отслеживающей сущности.</span><span class="sxs-lookup"><span data-stu-id="e86e5-165">The dictionary of original values is then applied to this tracked entity.</span></span> <span data-ttu-id="e86e5-166">При этом будут помечаться как измененные свойства с разными текущими и исходными значениями.</span><span class="sxs-lookup"><span data-stu-id="e86e5-166">This will mark as modified properties with different current and original values.</span></span> <span data-ttu-id="e86e5-167">Свойства, имеющие одинаковые текущие и исходные значения, не будут помечены как измененные.</span><span class="sxs-lookup"><span data-stu-id="e86e5-167">Properties that have the same current and original values will not be marked as modified.</span></span>

<span data-ttu-id="e86e5-168">В данном случае:</span><span class="sxs-lookup"><span data-stu-id="e86e5-168">In this case:</span></span>

- <span data-ttu-id="e86e5-169">С помощью присоединения будет отслеживаниь только одного экземпляра сущности.</span><span class="sxs-lookup"><span data-stu-id="e86e5-169">Only a single instance of the entity is tracked, using Attach.</span></span>
- <span data-ttu-id="e86e5-170">Экземпляр сущности **не** запрашивается из базы данных в процессе выполнения обновления.</span><span class="sxs-lookup"><span data-stu-id="e86e5-170">The entity instance is **not** queried from the database as part of making the update.</span></span>
- <span data-ttu-id="e86e5-171">Применение исходных значений гарантирует, что в базе данных будут обновлены только значения свойств, которые действительно изменились.</span><span class="sxs-lookup"><span data-stu-id="e86e5-171">Applying the original values ensures that only property values that have actually changed are updated in the database.</span></span>
- <span data-ttu-id="e86e5-172">Выполняется цикл обработки одной базы данных.</span><span class="sxs-lookup"><span data-stu-id="e86e5-172">One database round-trip is made.</span></span>

<span data-ttu-id="e86e5-173">Как и в примерах из предыдущего раздела, исходные значения не должны передаваться в качестве словаря. будет также работать экземпляр сущности или DTO.</span><span class="sxs-lookup"><span data-stu-id="e86e5-173">As with the examples in the previous section, the original values do not have to passed as a dictionary; an entity instance or DTO will also work.</span></span>

> [!TIP]
> <span data-ttu-id="e86e5-174">Хотя этот подход имеет привлекательные характеристики, он требует отправки исходных значений сущности в веб-клиент и обратно.</span><span class="sxs-lookup"><span data-stu-id="e86e5-174">While this approach has appealing characteristics, it does require sending the entity's original values to and from the web client.</span></span> <span data-ttu-id="e86e5-175">Тщательно продумайте, стоит ли использовать эту дополнительную сложность. для многих приложений один из более простых подходов является более практичным.</span><span class="sxs-lookup"><span data-stu-id="e86e5-175">Carefully consider whether this extra complexity is worth the benefits; for many applications one of the simpler approaches is more pragmatic.</span></span>

## <a name="attaching-a-serialized-graph"></a><span data-ttu-id="e86e5-176">Присоединение сериализованного графа</span><span class="sxs-lookup"><span data-stu-id="e86e5-176">Attaching a serialized graph</span></span>

<span data-ttu-id="e86e5-177">EF Core работает с графами сущностей, подключенных через внешние ключи и свойства навигации, как описано в статье [изменение внешних ключей и переходов](xref:core/change-tracking/relationship-changes).</span><span class="sxs-lookup"><span data-stu-id="e86e5-177">EF Core works with graphs of entities connected via foreign keys and navigation properties, as described in [Changing Foreign Keys and Navigations](xref:core/change-tracking/relationship-changes).</span></span> <span data-ttu-id="e86e5-178">Если эти графы создаются вне EF Core с помощью, например, из JSON-файла, они могут иметь несколько экземпляров одной и той же сущности.</span><span class="sxs-lookup"><span data-stu-id="e86e5-178">If these graphs are created outside of EF Core using, for example, from a JSON file, then they can have multiple instances of the same entity.</span></span> <span data-ttu-id="e86e5-179">Эти дубликаты необходимо разрешить в отдельные экземпляры, прежде чем можно будет относиться к диаграмме.</span><span class="sxs-lookup"><span data-stu-id="e86e5-179">These duplicates need to be resolved into single instances before the graph can be tracked.</span></span>

### <a name="graphs-with-no-duplicates"></a><span data-ttu-id="e86e5-180">Графы без дубликатов</span><span class="sxs-lookup"><span data-stu-id="e86e5-180">Graphs with no duplicates</span></span>

<span data-ttu-id="e86e5-181">Прежде чем продолжить, важно понять, что:</span><span class="sxs-lookup"><span data-stu-id="e86e5-181">Before going any further it is important to recognize that:</span></span>

- <span data-ttu-id="e86e5-182">Сериализаторы часто имеют параметры для обработки циклов и повторяющихся экземпляров в графе.</span><span class="sxs-lookup"><span data-stu-id="e86e5-182">Serializers often have options for handling loops and duplicate instances in the graph.</span></span>
- <span data-ttu-id="e86e5-183">Выбранный объект, используемый в качестве корня графа, часто может помочь уменьшить или удалить дубликаты.</span><span class="sxs-lookup"><span data-stu-id="e86e5-183">The choice of object used as the graph root can often help reduce or remove duplicates.</span></span>

<span data-ttu-id="e86e5-184">По возможности используйте параметры сериализации и выберите корни, которые не приводят к дублированию.</span><span class="sxs-lookup"><span data-stu-id="e86e5-184">If possible, use serialization options and choose roots that do not result in duplicates.</span></span> <span data-ttu-id="e86e5-185">Например, следующий код использует [JSON.NET](https://www.nuget.org/packages/Newtonsoft.Json/) для сериализации списка блогов со связанными записями:</span><span class="sxs-lookup"><span data-stu-id="e86e5-185">For example, the following code uses [Json.NET](https://www.nuget.org/packages/Newtonsoft.Json/) to serialize a list of blogs each with its associated posts:</span></span>

<!--
            using var context = new BlogsContext();

            var blogs = context.Blogs.Include(e => e.Posts).ToList();

            var serialized = JsonConvert.SerializeObject(
                blogs,
                new JsonSerializerSettings
                {
                    ReferenceLoopHandling = ReferenceLoopHandling.Ignore,
                    Formatting = Formatting.Indented
                });

            Console.WriteLine(serialized);
-->
[!code-csharp[Attaching_a_serialized_graph_1a](../../../samples/core/ChangeTracking/IdentityResolutionInEFCore/SerializedGraphExamples.cs?name=Attaching_a_serialized_graph_1a)]

<span data-ttu-id="e86e5-186">Код JSON, созданный из этого кода:</span><span class="sxs-lookup"><span data-stu-id="e86e5-186">The JSON generated from this code is:</span></span>

```json
[
  {
    "Id": 1,
    "Name": ".NET Blog",
    "Summary": "Posts about .NET",
    "Posts": [
      {
        "Id": 1,
        "Title": "Announcing the Release of EF Core 5.0",
        "Content": "Announcing the release of EF Core 5.0, a full featured cross-platform...",
        "BlogId": 1
      },
      {
        "Id": 2,
        "Title": "Announcing F# 5",
        "Content": "F# 5 is the latest version of F#, the functional programming language...",
        "BlogId": 1
      }
    ]
  },
  {
    "Id": 2,
    "Name": "Visual Studio Blog",
    "Summary": "Posts about Visual Studio",
    "Posts": [
      {
        "Id": 3,
        "Title": "Disassembly improvements for optimized managed debugging",
        "Content": "If you are focused on squeezing out the last bits of performance for your .NET service or...",
        "BlogId": 2
      },
      {
        "Id": 4,
        "Title": "Database Profiling with Visual Studio",
        "Content": "Examine when database queries were executed and measure how long the take using...",
        "BlogId": 2
      }
    ]
  }
]
```

<span data-ttu-id="e86e5-187">Обратите внимание, что в JSON нет повторяющихся блогов или записей.</span><span class="sxs-lookup"><span data-stu-id="e86e5-187">Notice that there are no duplicate blogs or posts in the JSON.</span></span> <span data-ttu-id="e86e5-188">Это означает, что простые вызовы `Update` будут работать для обновления этих сущностей в базе данных:</span><span class="sxs-lookup"><span data-stu-id="e86e5-188">This means that simple calls to `Update` will work to update these entities in the database:</span></span>

<!--
        public static void UpdateBlogsFromJson(string json)
        {
            using var context = new BlogsContext();

            var blogs = JsonConvert.DeserializeObject<List<Blog>>(json);

            foreach (var blog in blogs)
            {
                context.Update(blog);
            }

            context.SaveChanges();
        }
-->
[!code-csharp[Attaching_a_serialized_graph_1b](../../../samples/core/ChangeTracking/IdentityResolutionInEFCore/SerializedGraphExamples.cs?name=Attaching_a_serialized_graph_1b)]

### <a name="handling-duplicates"></a><span data-ttu-id="e86e5-189">Обработка дубликатов</span><span class="sxs-lookup"><span data-stu-id="e86e5-189">Handling duplicates</span></span>

<span data-ttu-id="e86e5-190">Код в предыдущем примере сериализован каждый блог со связанными записями.</span><span class="sxs-lookup"><span data-stu-id="e86e5-190">The code in the previous example serialized each blog with its associated posts.</span></span> <span data-ttu-id="e86e5-191">Если он был изменен для сериализации каждой записи со связанным с ним блогом, то в сериализованный JSON вводятся дубликаты.</span><span class="sxs-lookup"><span data-stu-id="e86e5-191">If this is changed to serialize each post with its associated blog, then duplicates are introduced into the serialized JSON.</span></span> <span data-ttu-id="e86e5-192">Например:</span><span class="sxs-lookup"><span data-stu-id="e86e5-192">For example:</span></span>

<!--
            using var context = new BlogsContext();

            var posts = context.Posts.Include(e => e.Blog).ToList();

            var serialized = JsonConvert.SerializeObject(
                posts,
                new JsonSerializerSettings
                {
                    ReferenceLoopHandling = ReferenceLoopHandling.Ignore,
                    Formatting = Formatting.Indented
                });

            Console.WriteLine(serialized);
-->
[!code-csharp[Attaching_a_serialized_graph_2](../../../samples/core/ChangeTracking/IdentityResolutionInEFCore/SerializedGraphExamples.cs?name=Attaching_a_serialized_graph_2)]

<span data-ttu-id="e86e5-193">Сериализованный JSON теперь выглядит следующим образом:</span><span class="sxs-lookup"><span data-stu-id="e86e5-193">The serialized JSON now looks like this:</span></span>

```json
[
  {
    "Id": 1,
    "Title": "Announcing the Release of EF Core 5.0",
    "Content": "Announcing the release of EF Core 5.0, a full featured cross-platform...",
    "BlogId": 1,
    "Blog": {
      "Id": 1,
      "Name": ".NET Blog",
      "Summary": "Posts about .NET",
      "Posts": [
        {
          "Id": 2,
          "Title": "Announcing F# 5",
          "Content": "F# 5 is the latest version of F#, the functional programming language...",
          "BlogId": 1
        }
      ]
    }
  },
  {
    "Id": 2,
    "Title": "Announcing F# 5",
    "Content": "F# 5 is the latest version of F#, the functional programming language...",
    "BlogId": 1,
    "Blog": {
      "Id": 1,
      "Name": ".NET Blog",
      "Summary": "Posts about .NET",
      "Posts": [
        {
          "Id": 1,
          "Title": "Announcing the Release of EF Core 5.0",
          "Content": "Announcing the release of EF Core 5.0, a full featured cross-platform...",
          "BlogId": 1
        }
      ]
    }
  },
  {
    "Id": 3,
    "Title": "Disassembly improvements for optimized managed debugging",
    "Content": "If you are focused on squeezing out the last bits of performance for your .NET service or...",
    "BlogId": 2,
    "Blog": {
      "Id": 2,
      "Name": "Visual Studio Blog",
      "Summary": "Posts about Visual Studio",
      "Posts": [
        {
          "Id": 4,
          "Title": "Database Profiling with Visual Studio",
          "Content": "Examine when database queries were executed and measure how long the take using...",
          "BlogId": 2
        }
      ]
    }
  },
  {
    "Id": 4,
    "Title": "Database Profiling with Visual Studio",
    "Content": "Examine when database queries were executed and measure how long the take using...",
    "BlogId": 2,
    "Blog": {
      "Id": 2,
      "Name": "Visual Studio Blog",
      "Summary": "Posts about Visual Studio",
      "Posts": [
        {
          "Id": 3,
          "Title": "Disassembly improvements for optimized managed debugging",
          "Content": "If you are focused on squeezing out the last bits of performance for your .NET service or...",
          "BlogId": 2
        }
      ]
    }
  }
]
```

<span data-ttu-id="e86e5-194">Обратите внимание, что теперь граф содержит несколько экземпляров блогов с одинаковым значением ключа, а также несколько экземпляров Post с одинаковым значением ключа.</span><span class="sxs-lookup"><span data-stu-id="e86e5-194">Notice that the graph now includes multiple Blog instances with the same key value, as well as multiple Post instance with the same key value.</span></span> <span data-ttu-id="e86e5-195">Попытка отслеживанию этого графа, как в предыдущем примере, приведет к созданию исключения:</span><span class="sxs-lookup"><span data-stu-id="e86e5-195">Attempting to track this graph like we did in the previous example will throw:</span></span>

> <span data-ttu-id="e86e5-196">System. InvalidOperationException: экземпляр типа сущности "Post" не может быть отслеживаем, так как уже отслеживается другой экземпляр со значением ключа "{ID: 2}".</span><span class="sxs-lookup"><span data-stu-id="e86e5-196">System.InvalidOperationException: The instance of entity type 'Post' cannot be tracked because another instance with the key value '{Id: 2}' is already being tracked.</span></span> <span data-ttu-id="e86e5-197">При присоединении существующих сущностей убедитесь, что присоединен только один экземпляр сущности с заданным значением ключа.</span><span class="sxs-lookup"><span data-stu-id="e86e5-197">When attaching existing entities, ensure that only one entity instance with a given key value is attached.</span></span>

<span data-ttu-id="e86e5-198">Это можно исправить двумя способами:</span><span class="sxs-lookup"><span data-stu-id="e86e5-198">We can fix this in two ways:</span></span>

- <span data-ttu-id="e86e5-199">Использование параметров сериализации JSON, сохраняющих ссылки</span><span class="sxs-lookup"><span data-stu-id="e86e5-199">Use JSON serialization options that preserve references</span></span>
- <span data-ttu-id="e86e5-200">Выполнение разрешения идентификации во время трассировки графа</span><span class="sxs-lookup"><span data-stu-id="e86e5-200">Perform identity resolution while the graph is being tracked</span></span>

#### <a name="preserve-references"></a><span data-ttu-id="e86e5-201">Сохранение ссылок</span><span class="sxs-lookup"><span data-stu-id="e86e5-201">Preserve references</span></span>

<span data-ttu-id="e86e5-202">Json.NET предоставляет `PreserveReferencesHandling` возможность справиться с этим.</span><span class="sxs-lookup"><span data-stu-id="e86e5-202">Json.NET provides the `PreserveReferencesHandling` option to handle this.</span></span> <span data-ttu-id="e86e5-203">Например:</span><span class="sxs-lookup"><span data-stu-id="e86e5-203">For example:</span></span>

<!--
            var serialized = JsonConvert.SerializeObject(
                posts,
                new JsonSerializerSettings
                {
                    PreserveReferencesHandling = PreserveReferencesHandling.All,
                    Formatting = Formatting.Indented
                });
-->
[!code-csharp[Attaching_a_serialized_graph_3](../../../samples/core/ChangeTracking/IdentityResolutionInEFCore/SerializedGraphExamples.cs?name=Attaching_a_serialized_graph_3)]

<span data-ttu-id="e86e5-204">Полученный JSON теперь выглядит следующим образом:</span><span class="sxs-lookup"><span data-stu-id="e86e5-204">The resulting JSON now looks like this:</span></span>

```json
{
  "$id": "1",
  "$values": [
    {
      "$id": "2",
      "Id": 1,
      "Title": "Announcing the Release of EF Core 5.0",
      "Content": "Announcing the release of EF Core 5.0, a full featured cross-platform...",
      "BlogId": 1,
      "Blog": {
        "$id": "3",
        "Id": 1,
        "Name": ".NET Blog",
        "Summary": "Posts about .NET",
        "Posts": [
          {
            "$ref": "2"
          },
          {
            "$id": "4",
            "Id": 2,
            "Title": "Announcing F# 5",
            "Content": "F# 5 is the latest version of F#, the functional programming language...",
            "BlogId": 1,
            "Blog": {
              "$ref": "3"
            }
          }
        ]
      }
    },
    {
      "$ref": "4"
    },
    {
      "$id": "5",
      "Id": 3,
      "Title": "Disassembly improvements for optimized managed debugging",
      "Content": "If you are focused on squeezing out the last bits of performance for your .NET service or...",
      "BlogId": 2,
      "Blog": {
        "$id": "6",
        "Id": 2,
        "Name": "Visual Studio Blog",
        "Summary": "Posts about Visual Studio",
        "Posts": [
          {
            "$ref": "5"
          },
          {
            "$id": "7",
            "Id": 4,
            "Title": "Database Profiling with Visual Studio",
            "Content": "Examine when database queries were executed and measure how long the take using...",
            "BlogId": 2,
            "Blog": {
              "$ref": "6"
            }
          }
        ]
      }
    },
    {
      "$ref": "7"
    }
  ]
}
```

<span data-ttu-id="e86e5-205">Обратите внимание, что этот JSON заменяет дубликаты ссылками `"$ref": "5"` , которые ссылаются на уже существующий экземпляр в графе.</span><span class="sxs-lookup"><span data-stu-id="e86e5-205">Notice that this JSON has replaced duplicates with references like `"$ref": "5"` that refer to the already existing instance in the graph.</span></span> <span data-ttu-id="e86e5-206">Эту диаграмму можно еще раз отслеживанию с помощью простых вызовов `Update` , как показано выше.</span><span class="sxs-lookup"><span data-stu-id="e86e5-206">This graph can again be tracked using the simple calls to `Update`, as shown above.</span></span>

<span data-ttu-id="e86e5-207"><xref:System.Text.Json>Поддержка в библиотеках базовых классов (BCL) .NET аналогична параметру, который дает тот же результат.</span><span class="sxs-lookup"><span data-stu-id="e86e5-207">The <xref:System.Text.Json> support in the .NET base class libraries (BCL) has a similar option which produces the same result.</span></span> <span data-ttu-id="e86e5-208">Например:</span><span class="sxs-lookup"><span data-stu-id="e86e5-208">For example:</span></span>

<!--
            var serialized = System.Text.Json.JsonSerializer.Serialize(posts, new System.Text.Json.JsonSerializerOptions
            {
                ReferenceHandler = ReferenceHandler.Preserve,
                WriteIndented = true
            });
-->
[!code-csharp[Attaching_a_serialized_graph_4](../../../samples/core/ChangeTracking/IdentityResolutionInEFCore/SerializedGraphExamples.cs?name=Attaching_a_serialized_graph_4)]

#### <a name="resolve-duplicates"></a><span data-ttu-id="e86e5-209">Разрешить дубликаты</span><span class="sxs-lookup"><span data-stu-id="e86e5-209">Resolve duplicates</span></span>

<span data-ttu-id="e86e5-210">Если не удается исключить дубликаты в процессе сериализации, то <xref:Microsoft.EntityFrameworkCore.ChangeTracking.ChangeTracker.TrackGraph%2A?displayProperty=nameWithType> предоставляет способ обработки этого.</span><span class="sxs-lookup"><span data-stu-id="e86e5-210">If it is not possible to eliminate duplicates in the serialization process, then <xref:Microsoft.EntityFrameworkCore.ChangeTracking.ChangeTracker.TrackGraph%2A?displayProperty=nameWithType> provides a way to handle this.</span></span> <span data-ttu-id="e86e5-211">Траккграф работает так же `Add` , как `Attach` и, `Update` за исключением того, что он создает обратный вызов для каждого экземпляра сущности перед его отслеживанием.</span><span class="sxs-lookup"><span data-stu-id="e86e5-211">TrackGraph works like `Add`, `Attach` and `Update` except that it generates a callback for every entity instance before tracking it.</span></span> <span data-ttu-id="e86e5-212">Этот обратный вызов можно использовать для записи или пропуска сущности.</span><span class="sxs-lookup"><span data-stu-id="e86e5-212">This callback can be used to either track the entity or ignore it.</span></span> <span data-ttu-id="e86e5-213">Например:</span><span class="sxs-lookup"><span data-stu-id="e86e5-213">For example:</span></span>

<!--
        public static void UpdatePostsFromJsonWithIdentityResolution(string json)
        {
            using var context = new BlogsContext();

            var posts = JsonConvert.DeserializeObject<List<Post>>(json);

            foreach (var post in posts)
            {
                context.ChangeTracker.TrackGraph(
                    post, node =>
                        {
                            var keyValue = node.Entry.Property("Id").CurrentValue;
                            var entityType = node.Entry.Metadata;

                            var existingEntity = node.Entry.Context.ChangeTracker.Entries()
                                .FirstOrDefault(
                                    e => Equals(e.Metadata, entityType)
                                         && Equals(e.Property("Id").CurrentValue, keyValue));

                            if (existingEntity == null)
                            {
                                Console.WriteLine($"Tracking {entityType} entity with key value {keyValue}");

                                node.Entry.State = EntityState.Modified;
                            }
                            else
                            {
                                Console.WriteLine($"Discarding duplicate {entityType} entity with key value {keyValue}");
                            }
                        });
            }

            context.SaveChanges();
        }
-->
[!code-csharp[Attaching_a_serialized_graph_5](../../../samples/core/ChangeTracking/IdentityResolutionInEFCore/SerializedGraphExamples.cs?name=Attaching_a_serialized_graph_5)]

<span data-ttu-id="e86e5-214">Для каждой сущности в графе этот код будет следующим:</span><span class="sxs-lookup"><span data-stu-id="e86e5-214">For each entity in the graph, this code will:</span></span>

- <span data-ttu-id="e86e5-215">Поиск типа сущности и значения ключа сущности</span><span class="sxs-lookup"><span data-stu-id="e86e5-215">Find the entity type and key value of the entity</span></span>
- <span data-ttu-id="e86e5-216">Поиск сущности с этим ключом в средстве записи изменений</span><span class="sxs-lookup"><span data-stu-id="e86e5-216">Lookup the entity with this key in the change tracker</span></span>
  - <span data-ttu-id="e86e5-217">Если сущность найдена, дальнейшие действия не выполняются, так как сущность дублируется</span><span class="sxs-lookup"><span data-stu-id="e86e5-217">If the entity is found, then no further action is taken as the entity is a duplicate</span></span>
  - <span data-ttu-id="e86e5-218">Если сущность не найдена, она будет отслеживаниь, задав для состояния значение `Modified`</span><span class="sxs-lookup"><span data-stu-id="e86e5-218">If the entity is not found, then it is tracked by setting the state to `Modified`</span></span>

<span data-ttu-id="e86e5-219">Результат выполнения этого кода:</span><span class="sxs-lookup"><span data-stu-id="e86e5-219">The output from running this code is:</span></span>

```output
Tracking EntityType: Post entity with key value 1
Tracking EntityType: Blog entity with key value 1
Tracking EntityType: Post entity with key value 2
Discarding duplicate EntityType: Post entity with key value 2
Tracking EntityType: Post entity with key value 3
Tracking EntityType: Blog entity with key value 2
Tracking EntityType: Post entity with key value 4
Discarding duplicate EntityType: Post entity with key value 4
```

> [!IMPORTANT]
> <span data-ttu-id="e86e5-220">В этом коде **предполагается, что все дубликаты идентичны**.</span><span class="sxs-lookup"><span data-stu-id="e86e5-220">This code **assumes that all duplicates are identical**.</span></span> <span data-ttu-id="e86e5-221">Это позволяет получить возможность произвольно выбрать одну из дубликатов, чтобы отказаться от других.</span><span class="sxs-lookup"><span data-stu-id="e86e5-221">This makes it safe to arbitrarily choose one of the duplicates to track while discarding the others.</span></span> <span data-ttu-id="e86e5-222">Если дубликаты могут различаться, то код должен решить, как определить, какой из них следует использовать, а также как объединить свойства и значения навигации.</span><span class="sxs-lookup"><span data-stu-id="e86e5-222">If the duplicates can differ, then the code will need to decide how to determine which one to use, and how to combine property and navigation values together.</span></span>

> [!NOTE]
> <span data-ttu-id="e86e5-223">Для простоты в этом коде предполагается, что каждая сущность имеет свойство первичного ключа с именем `Id` .</span><span class="sxs-lookup"><span data-stu-id="e86e5-223">For simplicity, this code assumes each entity has a primary key property called `Id`.</span></span> <span data-ttu-id="e86e5-224">Это можно кодифицированные в абстрактный базовый класс или интерфейс.</span><span class="sxs-lookup"><span data-stu-id="e86e5-224">This could be codified into an abstract base class or interface.</span></span> <span data-ttu-id="e86e5-225">Кроме того, свойство или свойства первичного ключа можно получить из <xref:Microsoft.EntityFrameworkCore.Metadata.IEntityType> метаданных таким образом, чтобы этот код работал с любым типом сущности.</span><span class="sxs-lookup"><span data-stu-id="e86e5-225">Alternately, the primary key property or properties could be obtained from the <xref:Microsoft.EntityFrameworkCore.Metadata.IEntityType> metadata such that this code would work with any type of entity.</span></span>

## <a name="failing-to-set-key-values"></a><span data-ttu-id="e86e5-226">Не удалось задать значения ключей</span><span class="sxs-lookup"><span data-stu-id="e86e5-226">Failing to set key values</span></span>

<span data-ttu-id="e86e5-227">Типы сущностей часто настраиваются для использования [автоматически создаваемых значений ключа](xref:core/modeling/generated-properties).</span><span class="sxs-lookup"><span data-stu-id="e86e5-227">Entity types are often configured to use [automatically generated key values](xref:core/modeling/generated-properties).</span></span> <span data-ttu-id="e86e5-228">Это значение по умолчанию для свойств Integer и GUID несоставных ключей.</span><span class="sxs-lookup"><span data-stu-id="e86e5-228">This is the default for integer and GUID properties of non-composite keys.</span></span> <span data-ttu-id="e86e5-229">Однако если тип сущности не настроен для использования автоматически создаваемых значений ключа, перед отслеживанием сущности необходимо задать явное значение ключа.</span><span class="sxs-lookup"><span data-stu-id="e86e5-229">However, if the entity type is not configured to use automatically generated key values, then an explicit key value must be set before tracking the entity.</span></span> <span data-ttu-id="e86e5-230">Например, используя следующий тип сущности:</span><span class="sxs-lookup"><span data-stu-id="e86e5-230">For example, using the following entity type:</span></span>

<!--
    public class Pet
    {
        [DatabaseGenerated(DatabaseGeneratedOption.None)]
        public int Id { get; set; }

        public string Name { get; set; }
    }
-->
[!code-csharp[Pet](../../../samples/core/ChangeTracking/IdentityResolutionInEFCore/IdentityResolutionSamples.cs?name=Pet)]

<span data-ttu-id="e86e5-231">Рассмотрим код, который пытается прослеживать два новых экземпляра сущности, не задавая значения ключей:</span><span class="sxs-lookup"><span data-stu-id="e86e5-231">Consider code that attempts to tracker two new entity instances without setting key values:</span></span>

<!--
            using var context = new BlogsContext();

            context.Add(new Pet { Name = "Smokey" });

            try
            {
                context.Add(new Pet { Name = "Clippy" }); // This will throw
            }
            catch (Exception e)
            {
                Console.WriteLine(e);
            }
-->
[!code-csharp[Failing_to_set_key_values_1](../../../samples/core/ChangeTracking/IdentityResolutionInEFCore/IdentityResolutionSamples.cs?name=Failing_to_set_key_values_1)]

<span data-ttu-id="e86e5-232">Этот код выдаст следующее:</span><span class="sxs-lookup"><span data-stu-id="e86e5-232">This code will throw:</span></span>

> <span data-ttu-id="e86e5-233">System. InvalidOperationException: экземпляр типа сущности "Pet" не может быть отслеживаем, так как уже отслеживается другой экземпляр со значением ключа "{ID: 0}".</span><span class="sxs-lookup"><span data-stu-id="e86e5-233">System.InvalidOperationException: The instance of entity type 'Pet' cannot be tracked because another instance with the key value '{Id: 0}' is already being tracked.</span></span> <span data-ttu-id="e86e5-234">При присоединении существующих сущностей убедитесь, что присоединен только один экземпляр сущности с заданным значением ключа.</span><span class="sxs-lookup"><span data-stu-id="e86e5-234">When attaching existing entities, ensure that only one entity instance with a given key value is attached.</span></span>

<span data-ttu-id="e86e5-235">Исправление для этого заключается в явном задании значений ключа или настройке свойства ключа для использования созданных значений ключа.</span><span class="sxs-lookup"><span data-stu-id="e86e5-235">The fix for this is to either to set key values explicitly or configure the key property to use generated key values.</span></span> <span data-ttu-id="e86e5-236">Дополнительные сведения см. в разделе [созданные значения](xref:core/modeling/generated-properties) .</span><span class="sxs-lookup"><span data-stu-id="e86e5-236">See [Generated Values](xref:core/modeling/generated-properties) for more information.</span></span>

## <a name="overusing-a-single-dbcontext-instance"></a><span data-ttu-id="e86e5-237">Использование одного экземпляра DbContext</span><span class="sxs-lookup"><span data-stu-id="e86e5-237">Overusing a single DbContext instance</span></span>

<span data-ttu-id="e86e5-238"><xref:Microsoft.EntityFrameworkCore.DbContext> разработана для представления кратковременной единицы работы, как описано в разделе [DbContext Initialization and Configuration (инициализация и конфигурация](xref:core/dbcontext-configuration/index)) и усовершенствована в [Отслеживание изменений в EF Core](xref:core/change-tracking/index).</span><span class="sxs-lookup"><span data-stu-id="e86e5-238"><xref:Microsoft.EntityFrameworkCore.DbContext> is designed to represent a short-lived unit-of-work, as described in [DbContext Initialization and Configuration](xref:core/dbcontext-configuration/index), and elaborated on in [Change Tracking in EF Core](xref:core/change-tracking/index).</span></span> <span data-ttu-id="e86e5-239">Это не следует делать в ситуациях, когда предпринимается попытка наблюдения за несколькими экземплярами одной и той же сущности.</span><span class="sxs-lookup"><span data-stu-id="e86e5-239">Not following this guidance makes it is easy to run into situations where an attempt is made to track multiple instances of the same entity.</span></span> <span data-ttu-id="e86e5-240">Ниже приведены распространенные примеры.</span><span class="sxs-lookup"><span data-stu-id="e86e5-240">Common examples are:</span></span>

- <span data-ttu-id="e86e5-241">Использование одного и того же экземпляра DbContext для настройки состояния теста и последующего выполнения теста.</span><span class="sxs-lookup"><span data-stu-id="e86e5-241">Using the same DbContext instance to both set up test state and then execute the test.</span></span> <span data-ttu-id="e86e5-242">Это часто приводит к DbContext отслеживания одного экземпляра сущности из настройки тестирования, а затем пытается присоединить новый экземпляр в нужном тесте.</span><span class="sxs-lookup"><span data-stu-id="e86e5-242">This often results in the DbContext still tracking one entity instance from test setup, while then attempting to attach a new instance in the test proper.</span></span> <span data-ttu-id="e86e5-243">Вместо этого используйте другой экземпляр DbContext для настройки состояния теста и надлежащего кода теста.</span><span class="sxs-lookup"><span data-stu-id="e86e5-243">Instead, use a different DbContext instance for setting up test state and the test code proper.</span></span>
- <span data-ttu-id="e86e5-244">Использование общего экземпляра DbContext в репозитории или аналогичного кода.</span><span class="sxs-lookup"><span data-stu-id="e86e5-244">Using a shared DbContext instance in a repository or similar code.</span></span> <span data-ttu-id="e86e5-245">Вместо этого убедитесь, что репозиторий использует один экземпляр DbContext для каждой единицы работы.</span><span class="sxs-lookup"><span data-stu-id="e86e5-245">Instead, make sure your repository uses a single DbContext instance for each unit-of-work.</span></span>

## <a name="identity-resolution-and-queries"></a><span data-ttu-id="e86e5-246">Разрешение и запросы идентификации</span><span class="sxs-lookup"><span data-stu-id="e86e5-246">Identity resolution and queries</span></span>

<span data-ttu-id="e86e5-247">Разрешение идентификации происходит автоматически при отслеживании сущностей из запроса.</span><span class="sxs-lookup"><span data-stu-id="e86e5-247">Identity resolution happens automatically when entities are tracked from a query.</span></span> <span data-ttu-id="e86e5-248">Это означает, что если экземпляр сущности с заданным значением ключа уже был записан, то вместо создания нового экземпляра будет использоваться существующий отслеживаниющий экземпляр.</span><span class="sxs-lookup"><span data-stu-id="e86e5-248">This means that if an entity instance with a given key value is already tracked, then this existing tracked instance is used instead of creating a new instance.</span></span> <span data-ttu-id="e86e5-249">Это имеет большое значение: Если данные были изменены в базе данных, это не будет отражено в результатах запроса.</span><span class="sxs-lookup"><span data-stu-id="e86e5-249">This has an important consequence: if the data has changed in the database, then this will not be reflected in the results of the query.</span></span> <span data-ttu-id="e86e5-250">Это хорошая причина для использования нового экземпляра DbContext для каждой единицы работы, как описано в разделе [Инициализация и конфигурация DbContext](xref:core/dbcontext-configuration/index), а также в [Отслеживание изменений в EF Core](xref:core/change-tracking/index).</span><span class="sxs-lookup"><span data-stu-id="e86e5-250">This is a good reason to use a new DbContext instance for each unit-of-work, as described in [DbContext Initialization and Configuration](xref:core/dbcontext-configuration/index), and elaborated on in [Change Tracking in EF Core](xref:core/change-tracking/index).</span></span>

> [!IMPORTANT]
> <span data-ttu-id="e86e5-251">Важно понимать, что EF Core всегда выполняет запрос LINQ для базы данных DbSet и возвращает результаты только в зависимости от того, что находится в базе данных.</span><span class="sxs-lookup"><span data-stu-id="e86e5-251">It is important to understand that EF Core always executes a LINQ query on a DbSet against the database and only returns results based on what is in the database.</span></span> <span data-ttu-id="e86e5-252">Однако для запроса отслеживания, если возвращаемые сущности уже отслеживаются, то вместо создания экземпляров на основе данных в базе данных используются Отслеживаемые экземпляры.</span><span class="sxs-lookup"><span data-stu-id="e86e5-252">However, for a tracking query, if the entities returned are already tracked, then the tracked instances are used instead of creating a instances from the data in the database.</span></span>

<span data-ttu-id="e86e5-253"><xref:Microsoft.EntityFrameworkCore.ChangeTracking.EntityEntry.Reload> или <xref:Microsoft.EntityFrameworkCore.ChangeTracking.EntityEntry.GetDatabaseValues> может использоваться, если отслеживание сущностей необходимо обновить последними данными из базы данных.</span><span class="sxs-lookup"><span data-stu-id="e86e5-253"><xref:Microsoft.EntityFrameworkCore.ChangeTracking.EntityEntry.Reload> or <xref:Microsoft.EntityFrameworkCore.ChangeTracking.EntityEntry.GetDatabaseValues> can be used when tracked entities need to be refreshed with the latest data from the database.</span></span> <span data-ttu-id="e86e5-254">Дополнительные сведения см. в разделе [доступ к отслеживающим сущностям](xref:core/change-tracking/entity-entries) .</span><span class="sxs-lookup"><span data-stu-id="e86e5-254">See [Accessing Tracked Entities](xref:core/change-tracking/entity-entries) for more information.</span></span>

<span data-ttu-id="e86e5-255">В отличие от отслеживания запросов, запросы без отслеживания не выполняют разрешение идентификаторов.</span><span class="sxs-lookup"><span data-stu-id="e86e5-255">In contrast to tracking queries, no-tracking queries do not perform identity resolution.</span></span> <span data-ttu-id="e86e5-256">Это означает, что запросы без отслеживания могут возвращать дубликаты точно так же, как в случае сериализации JSON, описанном выше.</span><span class="sxs-lookup"><span data-stu-id="e86e5-256">This means that no-tracking queries can return duplicates just like in the JSON serialization case described earlier.</span></span> <span data-ttu-id="e86e5-257">Обычно это не является проблемой, если результаты запроса будут сериализованы и отправлены клиенту.</span><span class="sxs-lookup"><span data-stu-id="e86e5-257">This is usually not an issue if the query results are going to be serialized and sent to the client.</span></span>

> [!TIP]
> <span data-ttu-id="e86e5-258">Не осуществляйте регулярный запрос без отслеживания, а затем присоедините возвращенные сущности к тому же контексту.</span><span class="sxs-lookup"><span data-stu-id="e86e5-258">Do not routinely perform a no-tracking query and then attach the returned entities to the same context.</span></span> <span data-ttu-id="e86e5-259">Это будет как медленнее, так и труднее, чем с помощью запроса отслеживания.</span><span class="sxs-lookup"><span data-stu-id="e86e5-259">This will be both slower and harder to get right than using a tracking query.</span></span>

<span data-ttu-id="e86e5-260">Запросы без отслеживания не выполняют разрешение идентификации, так как это влияет на производительность потоковой передачи большого количества сущностей из запроса.</span><span class="sxs-lookup"><span data-stu-id="e86e5-260">No-tracking queries do not perform identity resolution because doing so impacts the performance of streaming a large number of entities from a query.</span></span> <span data-ttu-id="e86e5-261">Это обусловлено тем, что разрешение идентификации требует отслеживания каждого возвращаемого экземпляра, чтобы его можно было использовать вместо создания дубликата.</span><span class="sxs-lookup"><span data-stu-id="e86e5-261">This is because identity resolution requires keeping track of each instance returned so that it can be used instead of later creating a duplicate.</span></span>

<span data-ttu-id="e86e5-262">Начиная с EF Core 5,0, запросы без отслеживания могут принудительно выполнять разрешение идентификаторов с помощью <xref:Microsoft.EntityFrameworkCore.EntityFrameworkQueryableExtensions.AsNoTrackingWithIdentityResolution%60%601(System.Linq.IQueryable{%60%600})> .</span><span class="sxs-lookup"><span data-stu-id="e86e5-262">Starting with EF Core 5.0, no-tracking queries can be forced to perform identity resolution by using <xref:Microsoft.EntityFrameworkCore.EntityFrameworkQueryableExtensions.AsNoTrackingWithIdentityResolution%60%601(System.Linq.IQueryable{%60%600})>.</span></span> <span data-ttu-id="e86e5-263">После этого запрос будет отслеживать возвращаемые экземпляры (не отслеживая их обычным способом) и не будет создавать дубликаты в результатах запроса.</span><span class="sxs-lookup"><span data-stu-id="e86e5-263">The query will then keep track of returned instances (without tracking them in the normal way) and ensure no duplicates are created in the query results.</span></span>

## <a name="overriding-object-equality"></a><span data-ttu-id="e86e5-264">Переопределение равенства объектов</span><span class="sxs-lookup"><span data-stu-id="e86e5-264">Overriding object equality</span></span>

<span data-ttu-id="e86e5-265">EF Core использует [равенство ссылок](/dotnet/csharp/programming-guide/statements-expressions-operators/equality-comparisons) при сравнении экземпляров сущностей.</span><span class="sxs-lookup"><span data-stu-id="e86e5-265">EF Core uses [reference equality](/dotnet/csharp/programming-guide/statements-expressions-operators/equality-comparisons) when comparing entity instances.</span></span> <span data-ttu-id="e86e5-266">Это происходит даже в том случае, если типы сущностей переопределяют <xref:System.Object.Equals(System.Object)?displayProperty=nameWithType> или иным образом изменяют равенство объектов.</span><span class="sxs-lookup"><span data-stu-id="e86e5-266">This is the case even if the entity types override <xref:System.Object.Equals(System.Object)?displayProperty=nameWithType> or otherwise change object equality.</span></span> <span data-ttu-id="e86e5-267">Однако существует одно место, где переопределение равенства может повлиять на EF Core поведение: когда навигация коллекции использует переопределенную равенство вместо равенства ссылок, и, следовательно, сообщает о нескольких экземплярах.</span><span class="sxs-lookup"><span data-stu-id="e86e5-267">However, there is one place where overriding equality can impact EF Core behavior: when collection navigations use the overridden equality instead of reference equality, and hence report multiple instances as the same.</span></span>

<span data-ttu-id="e86e5-268">Поэтому рекомендуется избегать переопределения равенства сущностей.</span><span class="sxs-lookup"><span data-stu-id="e86e5-268">Because of this it is recommended that overriding entity equality should be avoided.</span></span> <span data-ttu-id="e86e5-269">Если он используется, обязательно создайте навигацию по коллекциям, которые принудительно выполняют равенство ссылок.</span><span class="sxs-lookup"><span data-stu-id="e86e5-269">If it is used, then make sure to create collection navigations that force reference equality.</span></span> <span data-ttu-id="e86e5-270">Например, создайте компаратор проверки на равенство, использующий равенство ссылок:</span><span class="sxs-lookup"><span data-stu-id="e86e5-270">For example, create an equality comparer that uses reference equality:</span></span>

<!--
    public sealed class ReferenceEqualityComparer : IEqualityComparer<object>
    {
        private ReferenceEqualityComparer()
        {
        }

        public static ReferenceEqualityComparer Instance { get; } = new ReferenceEqualityComparer();

        bool IEqualityComparer<object>.Equals(object x, object y) => x == y;

        int IEqualityComparer<object>.GetHashCode(object obj) => RuntimeHelpers.GetHashCode(obj);
    }
-->
[!code-csharp[ReferenceEqualityComparer](../../../samples/core/ChangeTracking/IdentityResolutionInEFCore/ReferenceEqualityComparer.cs?name=ReferenceEqualityComparer)]

<span data-ttu-id="e86e5-271">(Начиная с .NET 5, он включается в BCL как <xref:System.Collections.Generic.ReferenceEqualityComparer> .)</span><span class="sxs-lookup"><span data-stu-id="e86e5-271">(Starting with .NET 5, this is included in the BCL as <xref:System.Collections.Generic.ReferenceEqualityComparer>.)</span></span>

<span data-ttu-id="e86e5-272">Затем этот компаратор можно использовать при создании переходов по коллекции.</span><span class="sxs-lookup"><span data-stu-id="e86e5-272">This comparer can then be used when creating collection navigations.</span></span> <span data-ttu-id="e86e5-273">Например:</span><span class="sxs-lookup"><span data-stu-id="e86e5-273">For example:</span></span>

<!--
        public ICollection<Order> Orders { get; set; }
            = new HashSet<Order>(ReferenceEqualityComparer.Instance);
-->
[!code-csharp[OrdersCollection](../../../samples/core/ChangeTracking/IdentityResolutionInEFCore/IdentityResolutionSamples.cs?name=OrdersCollection)]

### <a name="comparing-key-properties"></a><span data-ttu-id="e86e5-274">Сравнение свойств ключа</span><span class="sxs-lookup"><span data-stu-id="e86e5-274">Comparing key properties</span></span>

<span data-ttu-id="e86e5-275">Помимо сравнения на равенство, значения ключей также должны быть упорядочены.</span><span class="sxs-lookup"><span data-stu-id="e86e5-275">In addition to equality comparisons, key values also need to be ordered.</span></span> <span data-ttu-id="e86e5-276">Это важно для предотвращения взаимоблокировок при обновлении нескольких сущностей в одном вызове метода SaveChanges.</span><span class="sxs-lookup"><span data-stu-id="e86e5-276">This is important for avoiding deadlocks when updating multiple entities in a single call to SaveChanges.</span></span> <span data-ttu-id="e86e5-277">Все типы, используемые для свойств первичного, альтернативного или внешнего ключа, а также те, которые используются для уникальных индексов, должны реализовывать <xref:System.IComparable%601> и <xref:System.IEquatable%601> .</span><span class="sxs-lookup"><span data-stu-id="e86e5-277">All types used for primary, alternate, or foreign key properties, as well as those used for unique indexes, must implement <xref:System.IComparable%601> and <xref:System.IEquatable%601>.</span></span> <span data-ttu-id="e86e5-278">Типы, обычно используемые в качестве ключей (int, GUID, String и т. д.), уже поддерживают эти интерфейсы.</span><span class="sxs-lookup"><span data-stu-id="e86e5-278">Types normally used as keys (int, Guid, string, etc.) already support these interfaces.</span></span> <span data-ttu-id="e86e5-279">Типы пользовательских ключей могут добавлять эти интерфейсы.</span><span class="sxs-lookup"><span data-stu-id="e86e5-279">Custom key types may to add these interfaces.</span></span>
