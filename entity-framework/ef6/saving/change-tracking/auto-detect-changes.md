---
title: Автоматическое обнаружение изменений — EF6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: a8d1488d-9a54-4623-a76b-e81329ff2756
caps.latest.revision: 3
ms.openlocfilehash: 62f2f026426346fc1230a2f5743c8cb7d232ec7f
ms.sourcegitcommit: f05e7b62584cf228f17390bb086a61d505712e1b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/08/2018
ms.locfileid: "39121825"
---
# <a name="automatic-detect-changes"></a><span data-ttu-id="bbcbd-102">Автоматическое обнаружение изменений</span><span class="sxs-lookup"><span data-stu-id="bbcbd-102">Automatic detect changes</span></span>
<span data-ttu-id="bbcbd-103">При использовании большинство сущностей POCO определение как была изменена сущность (и поэтому какие обновления должны отправляться в базу данных) обрабатывается алгоритмом обнаружение изменений.</span><span class="sxs-lookup"><span data-stu-id="bbcbd-103">When using most POCO entities the determination of how an entity has changed (and therefore which updates need to be sent to the database) is handled by the Detect Changes algorithm.</span></span> <span data-ttu-id="bbcbd-104">Обнаружение изменений работает путем выявления различий между текущие значения свойств сущности и исходные значения свойств, которые хранятся в моментальном снимке, когда сущность была запросе или присоединении.</span><span class="sxs-lookup"><span data-stu-id="bbcbd-104">Detect Changes works by detecting the differences between the current property values of the entity and the original property values that are stored in a snapshot when the entity was queried or attached.</span></span> <span data-ttu-id="bbcbd-105">Методы, представленные в этом разделе, также применимы к моделям, созданным с помощью Code First и конструктора EF.</span><span class="sxs-lookup"><span data-stu-id="bbcbd-105">The techniques shown in this topic apply equally to models created with Code First and the EF Designer.</span></span>  

<span data-ttu-id="bbcbd-106">По умолчанию Entity Framework автоматически выполняет обнаружение изменений, при вызове следующих методов:</span><span class="sxs-lookup"><span data-stu-id="bbcbd-106">By default, Entity Framework performs Detect Changes automatically when the following methods are called:</span></span>  

- <span data-ttu-id="bbcbd-107">DbSet.Find</span><span class="sxs-lookup"><span data-stu-id="bbcbd-107">DbSet.Find</span></span>  
- <span data-ttu-id="bbcbd-108">DbSet.Local</span><span class="sxs-lookup"><span data-stu-id="bbcbd-108">DbSet.Local</span></span>  
- <span data-ttu-id="bbcbd-109">DbSet.Add</span><span class="sxs-lookup"><span data-stu-id="bbcbd-109">DbSet.Add</span></span>  
- <span data-ttu-id="bbcbd-110">DbSet.AddRange</span><span class="sxs-lookup"><span data-stu-id="bbcbd-110">DbSet.AddRange</span></span>
- <span data-ttu-id="bbcbd-111">DbSet.Remove</span><span class="sxs-lookup"><span data-stu-id="bbcbd-111">DbSet.Remove</span></span>  
- <span data-ttu-id="bbcbd-112">DbSet.RemoveRange</span><span class="sxs-lookup"><span data-stu-id="bbcbd-112">DbSet.RemoveRange</span></span>
- <span data-ttu-id="bbcbd-113">DbSet.Attach</span><span class="sxs-lookup"><span data-stu-id="bbcbd-113">DbSet.Attach</span></span>  
- <span data-ttu-id="bbcbd-114">DbContext.SaveChanges</span><span class="sxs-lookup"><span data-stu-id="bbcbd-114">DbContext.SaveChanges</span></span>  
- <span data-ttu-id="bbcbd-115">DbContext.GetValidationErrors</span><span class="sxs-lookup"><span data-stu-id="bbcbd-115">DbContext.GetValidationErrors</span></span>  
- <span data-ttu-id="bbcbd-116">DbContext.Entry</span><span class="sxs-lookup"><span data-stu-id="bbcbd-116">DbContext.Entry</span></span>  
- <span data-ttu-id="bbcbd-117">DbChangeTracker.Entries</span><span class="sxs-lookup"><span data-stu-id="bbcbd-117">DbChangeTracker.Entries</span></span>  

## <a name="disabling-automatic-detection-of-changes"></a><span data-ttu-id="bbcbd-118">Отключить автоматическое обнаружение изменений</span><span class="sxs-lookup"><span data-stu-id="bbcbd-118">Disabling automatic detection of changes</span></span>  

<span data-ttu-id="bbcbd-119">Если вы наблюдаете массу сущностей в контексте, вызовите один из этих методов много раз в цикле может добиться заметного повышения производительности, отключив обнаружение изменений в течение всего цикла.</span><span class="sxs-lookup"><span data-stu-id="bbcbd-119">If you are tracking a lot of entities in your context and you call one of these methods many times in a loop, then you may get significant performance improvements by turning off detection of changes for the duration of the loop.</span></span> <span data-ttu-id="bbcbd-120">Пример:</span><span class="sxs-lookup"><span data-stu-id="bbcbd-120">For example:</span></span>  

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

<span data-ttu-id="bbcbd-121">Не забудьте снова включить обнаружение изменений после цикла, мы использовали try/finally, чтобы он всегда повторно включен даже если код в цикле вызывает исключение.</span><span class="sxs-lookup"><span data-stu-id="bbcbd-121">Don’t forget to re-enable detection of changes after the loop — We've used a try/finally to ensure it is always re-enabled even if code in the loop throws an exception.</span></span>  

<span data-ttu-id="bbcbd-122">Альтернативы для отключения и повторного включения это поле остается автоматическое обнаружение изменений, отключить все значения времени и либо контекста вызова. ChangeTracker.DetectChanges явным образом или используйте посредников для отслеживания изменений, тщательно.</span><span class="sxs-lookup"><span data-stu-id="bbcbd-122">An alternative to disabling and re-enabling is to leave automatic detection of changes turned off at all times and either call context.ChangeTracker.DetectChanges explicitly or use change tracking proxies diligently.</span></span> <span data-ttu-id="bbcbd-123">Обе функции являются расширенными и может привести к тонким ошибкам в приложение таким образом их использовать с осторожностью.</span><span class="sxs-lookup"><span data-stu-id="bbcbd-123">Both of these options are advanced and can easily introduce subtle bugs into your application so use them with care.</span></span>  

<span data-ttu-id="bbcbd-124">Если вам нужно добавить или удалить множество объектов из контекста, рассмотрите возможность использования DbSet.AddRange и DbSet.RemoveRange.</span><span class="sxs-lookup"><span data-stu-id="bbcbd-124">If you need to add or remove many objects from a context, consider using DbSet.AddRange and DbSet.RemoveRange.</span></span> <span data-ttu-id="bbcbd-125">Этот метод автоматически обнаруживает изменения только один раз после завершения операций добавления или удаления.</span><span class="sxs-lookup"><span data-stu-id="bbcbd-125">This methods automatically detect changes only once after the add or remove operations are completed.</span></span> 
