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
# <a name="automatic-detect-changes"></a>Автоматическое обнаружение изменений
При использовании большинство сущностей POCO определение как была изменена сущность (и поэтому какие обновления должны отправляться в базу данных) обрабатывается алгоритмом обнаружение изменений. Обнаружение изменений работает путем выявления различий между текущие значения свойств сущности и исходные значения свойств, которые хранятся в моментальном снимке, когда сущность была запросе или присоединении. Методы, представленные в этом разделе, также применимы к моделям, созданным с помощью Code First и конструктора EF.  

По умолчанию Entity Framework автоматически выполняет обнаружение изменений, при вызове следующих методов:  

- DbSet.Find  
- DbSet.Local  
- DbSet.Add  
- DbSet.AddRange
- DbSet.Remove  
- DbSet.RemoveRange
- DbSet.Attach  
- DbContext.SaveChanges  
- DbContext.GetValidationErrors  
- DbContext.Entry  
- DbChangeTracker.Entries  

## <a name="disabling-automatic-detection-of-changes"></a>Отключить автоматическое обнаружение изменений  

Если вы наблюдаете массу сущностей в контексте, вызовите один из этих методов много раз в цикле может добиться заметного повышения производительности, отключив обнаружение изменений в течение всего цикла. Пример:  

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

Не забудьте снова включить обнаружение изменений после цикла, мы использовали try/finally, чтобы он всегда повторно включен даже если код в цикле вызывает исключение.  

Альтернативы для отключения и повторного включения это поле остается автоматическое обнаружение изменений, отключить все значения времени и либо контекста вызова. ChangeTracker.DetectChanges явным образом или используйте посредников для отслеживания изменений, тщательно. Обе функции являются расширенными и может привести к тонким ошибкам в приложение таким образом их использовать с осторожностью.  

Если вам нужно добавить или удалить множество объектов из контекста, рассмотрите возможность использования DbSet.AddRange и DbSet.RemoveRange. Этот метод автоматически обнаруживает изменения только один раз после завершения операций добавления или удаления. 
