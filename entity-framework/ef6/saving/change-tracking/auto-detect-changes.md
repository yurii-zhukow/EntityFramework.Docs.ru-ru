---
title: Автоматическое обнаружение изменений — EF6
description: Автоматическое обнаружение изменений в Entity Framework 6
author: divega
ms.date: 10/23/2016
ms.assetid: a8d1488d-9a54-4623-a76b-e81329ff2756
uid: ef6/saving/change-tracking/auto-detect-changes
ms.openlocfilehash: 938af809ac89d3490da9885497fc5601453e1c34
ms.sourcegitcommit: 7c3939504bb9da3f46bea3443638b808c04227c2
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/09/2020
ms.locfileid: "89619998"
---
# <a name="automatic-detect-changes"></a><span data-ttu-id="b6198-103">Автоматическое обнаружение изменений</span><span class="sxs-lookup"><span data-stu-id="b6198-103">Automatic detect changes</span></span>
<span data-ttu-id="b6198-104">При использовании большинства сущностей POCO определение того, каким образом изменилась сущность (и, следовательно, какие обновления необходимо отправить в базу данных), обрабатывается алгоритмом обнаружения изменений.</span><span class="sxs-lookup"><span data-stu-id="b6198-104">When using most POCO entities the determination of how an entity has changed (and therefore which updates need to be sent to the database) is handled by the Detect Changes algorithm.</span></span> <span data-ttu-id="b6198-105">Обнаружение изменений осуществляется путем обнаружения различий между текущими значениями свойств сущности и значениями исходных свойств, которые хранятся в моментальном снимке при запросе или присоединении сущности.</span><span class="sxs-lookup"><span data-stu-id="b6198-105">Detect Changes works by detecting the differences between the current property values of the entity and the original property values that are stored in a snapshot when the entity was queried or attached.</span></span> <span data-ttu-id="b6198-106">Методы, представленные в этом разделе, также применимы к моделям, созданным с помощью Code First и конструктора EF.</span><span class="sxs-lookup"><span data-stu-id="b6198-106">The techniques shown in this topic apply equally to models created with Code First and the EF Designer.</span></span>  

<span data-ttu-id="b6198-107">По умолчанию Entity Framework выполняет обнаружение изменений автоматически при вызове следующих методов:</span><span class="sxs-lookup"><span data-stu-id="b6198-107">By default, Entity Framework performs Detect Changes automatically when the following methods are called:</span></span>  

- <span data-ttu-id="b6198-108">DbSet.Find</span><span class="sxs-lookup"><span data-stu-id="b6198-108">DbSet.Find</span></span>  
- <span data-ttu-id="b6198-109">DbSet. local</span><span class="sxs-lookup"><span data-stu-id="b6198-109">DbSet.Local</span></span>  
- <span data-ttu-id="b6198-110">DbSet. Add</span><span class="sxs-lookup"><span data-stu-id="b6198-110">DbSet.Add</span></span>  
- <span data-ttu-id="b6198-111">DbSet. AddRange</span><span class="sxs-lookup"><span data-stu-id="b6198-111">DbSet.AddRange</span></span>
- <span data-ttu-id="b6198-112">DbSet. Remove</span><span class="sxs-lookup"><span data-stu-id="b6198-112">DbSet.Remove</span></span>  
- <span data-ttu-id="b6198-113">DbSet. Ремоверанже</span><span class="sxs-lookup"><span data-stu-id="b6198-113">DbSet.RemoveRange</span></span>
- <span data-ttu-id="b6198-114">DbSet. Attach</span><span class="sxs-lookup"><span data-stu-id="b6198-114">DbSet.Attach</span></span>  
- <span data-ttu-id="b6198-115">DbContext.SaveChanges</span><span class="sxs-lookup"><span data-stu-id="b6198-115">DbContext.SaveChanges</span></span>  
- <span data-ttu-id="b6198-116">DbContext. Жетвалидатионеррорс</span><span class="sxs-lookup"><span data-stu-id="b6198-116">DbContext.GetValidationErrors</span></span>  
- <span data-ttu-id="b6198-117">DbContext.Entry</span><span class="sxs-lookup"><span data-stu-id="b6198-117">DbContext.Entry</span></span>  
- <span data-ttu-id="b6198-118">Дбчанжетраккер. записи</span><span class="sxs-lookup"><span data-stu-id="b6198-118">DbChangeTracker.Entries</span></span>  

## <a name="disabling-automatic-detection-of-changes"></a><span data-ttu-id="b6198-119">Отключение автоматического обнаружения изменений</span><span class="sxs-lookup"><span data-stu-id="b6198-119">Disabling automatic detection of changes</span></span>  

<span data-ttu-id="b6198-120">При отслеживании большого количества сущностей в контексте и многократном вызове одного из этих методов в цикле вы можете значительно повысить производительность, отключив обнаружение изменений в течение цикла.</span><span class="sxs-lookup"><span data-stu-id="b6198-120">If you are tracking a lot of entities in your context and you call one of these methods many times in a loop, then you may get significant performance improvements by turning off detection of changes for the duration of the loop.</span></span> <span data-ttu-id="b6198-121">Пример:</span><span class="sxs-lookup"><span data-stu-id="b6198-121">For example:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    try
    {
        context.Configuration.AutoDetectChangesEnabled = false;

        // Make many calls in a loop
        foreach (var blog in aLotOfBlogs)
        {
            context.Blogs.Add(blog);
        }
    }
    finally
    {
        context.Configuration.AutoDetectChangesEnabled = true;
    }
}
```  

<span data-ttu-id="b6198-122">Не забудьте повторно включить обнаружение изменений после цикла — мы использовали конструкцию try/finally, чтобы убедиться, что она всегда включена повторно, даже если код в цикле вызывает исключение.</span><span class="sxs-lookup"><span data-stu-id="b6198-122">Don’t forget to re-enable detection of changes after the loop — We've used a try/finally to ensure it is always re-enabled even if code in the loop throws an exception.</span></span>  

<span data-ttu-id="b6198-123">Альтернативой отключению и повторному включению является автоматическое обнаружение отключенных изменений в любое время и либо контекст вызова. ChangeTracker. DetectChanges явным образом или используют прокси-серверы отслеживания изменений более тщательно.</span><span class="sxs-lookup"><span data-stu-id="b6198-123">An alternative to disabling and re-enabling is to leave automatic detection of changes turned off at all times and either call context.ChangeTracker.DetectChanges explicitly or use change tracking proxies diligently.</span></span> <span data-ttu-id="b6198-124">Оба эти варианта являются расширенными и могут легко добавить в приложение небольшие ошибки, чтобы использовать их с осторожностью.</span><span class="sxs-lookup"><span data-stu-id="b6198-124">Both of these options are advanced and can easily introduce subtle bugs into your application so use them with care.</span></span>  

<span data-ttu-id="b6198-125">Если необходимо добавить или удалить несколько объектов из контекста, рассмотрите возможность использования DbSet. AddRange и DbSet. Ремоверанже.</span><span class="sxs-lookup"><span data-stu-id="b6198-125">If you need to add or remove many objects from a context, consider using DbSet.AddRange and DbSet.RemoveRange.</span></span> <span data-ttu-id="b6198-126">Эти методы автоматически обнаруживают изменения только один раз после завершения операций добавления или удаления.</span><span class="sxs-lookup"><span data-stu-id="b6198-126">This methods automatically detect changes only once after the add or remove operations are completed.</span></span> 
