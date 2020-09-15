---
title: Автоматическое обнаружение изменений — EF6
description: Автоматическое обнаружение изменений в Entity Framework 6
author: divega
ms.date: 10/23/2016
uid: ef6/saving/change-tracking/auto-detect-changes
ms.openlocfilehash: aa8b94d843d99a8f040bdb065297d2e19b4770d7
ms.sourcegitcommit: abda0872f86eefeca191a9a11bfca976bc14468b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/14/2020
ms.locfileid: "90073786"
---
# <a name="automatic-detect-changes"></a><span data-ttu-id="8f89d-103">Автоматическое обнаружение изменений</span><span class="sxs-lookup"><span data-stu-id="8f89d-103">Automatic detect changes</span></span>
<span data-ttu-id="8f89d-104">При использовании большинства сущностей POCO определение того, каким образом изменилась сущность (и, следовательно, какие обновления необходимо отправить в базу данных), обрабатывается алгоритмом обнаружения изменений.</span><span class="sxs-lookup"><span data-stu-id="8f89d-104">When using most POCO entities the determination of how an entity has changed (and therefore which updates need to be sent to the database) is handled by the Detect Changes algorithm.</span></span> <span data-ttu-id="8f89d-105">Обнаружение изменений осуществляется путем обнаружения различий между текущими значениями свойств сущности и значениями исходных свойств, которые хранятся в моментальном снимке при запросе или присоединении сущности.</span><span class="sxs-lookup"><span data-stu-id="8f89d-105">Detect Changes works by detecting the differences between the current property values of the entity and the original property values that are stored in a snapshot when the entity was queried or attached.</span></span> <span data-ttu-id="8f89d-106">Методы, представленные в этом разделе, также применимы к моделям, созданным с помощью Code First и конструктора EF.</span><span class="sxs-lookup"><span data-stu-id="8f89d-106">The techniques shown in this topic apply equally to models created with Code First and the EF Designer.</span></span>  

<span data-ttu-id="8f89d-107">По умолчанию Entity Framework выполняет обнаружение изменений автоматически при вызове следующих методов:</span><span class="sxs-lookup"><span data-stu-id="8f89d-107">By default, Entity Framework performs Detect Changes automatically when the following methods are called:</span></span>  

- <span data-ttu-id="8f89d-108">DbSet.Find</span><span class="sxs-lookup"><span data-stu-id="8f89d-108">DbSet.Find</span></span>  
- <span data-ttu-id="8f89d-109">DbSet. local</span><span class="sxs-lookup"><span data-stu-id="8f89d-109">DbSet.Local</span></span>  
- <span data-ttu-id="8f89d-110">DbSet. Add</span><span class="sxs-lookup"><span data-stu-id="8f89d-110">DbSet.Add</span></span>  
- <span data-ttu-id="8f89d-111">DbSet. AddRange</span><span class="sxs-lookup"><span data-stu-id="8f89d-111">DbSet.AddRange</span></span>
- <span data-ttu-id="8f89d-112">DbSet. Remove</span><span class="sxs-lookup"><span data-stu-id="8f89d-112">DbSet.Remove</span></span>  
- <span data-ttu-id="8f89d-113">DbSet. Ремоверанже</span><span class="sxs-lookup"><span data-stu-id="8f89d-113">DbSet.RemoveRange</span></span>
- <span data-ttu-id="8f89d-114">DbSet. Attach</span><span class="sxs-lookup"><span data-stu-id="8f89d-114">DbSet.Attach</span></span>  
- <span data-ttu-id="8f89d-115">DbContext.SaveChanges</span><span class="sxs-lookup"><span data-stu-id="8f89d-115">DbContext.SaveChanges</span></span>  
- <span data-ttu-id="8f89d-116">DbContext. Жетвалидатионеррорс</span><span class="sxs-lookup"><span data-stu-id="8f89d-116">DbContext.GetValidationErrors</span></span>  
- <span data-ttu-id="8f89d-117">DbContext.Entry</span><span class="sxs-lookup"><span data-stu-id="8f89d-117">DbContext.Entry</span></span>  
- <span data-ttu-id="8f89d-118">Дбчанжетраккер. записи</span><span class="sxs-lookup"><span data-stu-id="8f89d-118">DbChangeTracker.Entries</span></span>  

## <a name="disabling-automatic-detection-of-changes"></a><span data-ttu-id="8f89d-119">Отключение автоматического обнаружения изменений</span><span class="sxs-lookup"><span data-stu-id="8f89d-119">Disabling automatic detection of changes</span></span>  

<span data-ttu-id="8f89d-120">При отслеживании большого количества сущностей в контексте и многократном вызове одного из этих методов в цикле вы можете значительно повысить производительность, отключив обнаружение изменений в течение цикла.</span><span class="sxs-lookup"><span data-stu-id="8f89d-120">If you are tracking a lot of entities in your context and you call one of these methods many times in a loop, then you may get significant performance improvements by turning off detection of changes for the duration of the loop.</span></span> <span data-ttu-id="8f89d-121">Пример:</span><span class="sxs-lookup"><span data-stu-id="8f89d-121">For example:</span></span>  

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

<span data-ttu-id="8f89d-122">Не забудьте повторно включить обнаружение изменений после цикла — мы использовали конструкцию try/finally, чтобы убедиться, что она всегда включена повторно, даже если код в цикле вызывает исключение.</span><span class="sxs-lookup"><span data-stu-id="8f89d-122">Don’t forget to re-enable detection of changes after the loop — We've used a try/finally to ensure it is always re-enabled even if code in the loop throws an exception.</span></span>  

<span data-ttu-id="8f89d-123">Альтернативой отключению и повторному включению является автоматическое обнаружение отключенных изменений в любое время и либо контекст вызова. ChangeTracker. DetectChanges явным образом или используют прокси-серверы отслеживания изменений более тщательно.</span><span class="sxs-lookup"><span data-stu-id="8f89d-123">An alternative to disabling and re-enabling is to leave automatic detection of changes turned off at all times and either call context.ChangeTracker.DetectChanges explicitly or use change tracking proxies diligently.</span></span> <span data-ttu-id="8f89d-124">Оба эти варианта являются расширенными и могут легко добавить в приложение небольшие ошибки, чтобы использовать их с осторожностью.</span><span class="sxs-lookup"><span data-stu-id="8f89d-124">Both of these options are advanced and can easily introduce subtle bugs into your application so use them with care.</span></span>  

<span data-ttu-id="8f89d-125">Если необходимо добавить или удалить несколько объектов из контекста, рассмотрите возможность использования DbSet. AddRange и DbSet. Ремоверанже.</span><span class="sxs-lookup"><span data-stu-id="8f89d-125">If you need to add or remove many objects from a context, consider using DbSet.AddRange and DbSet.RemoveRange.</span></span> <span data-ttu-id="8f89d-126">Эти методы автоматически обнаруживают изменения только один раз после завершения операций добавления или удаления.</span><span class="sxs-lookup"><span data-stu-id="8f89d-126">This methods automatically detect changes only once after the add or remove operations are completed.</span></span> 
