---
title: Метод Load — EF6
description: Метод Load в Entity Framework 6
author: ajcvickers
ms.date: 10/23/2016
uid: ef6/querying/load-method
ms.openlocfilehash: 055d7dd6957a31cb876904af55a1126bb944d338
ms.sourcegitcommit: 0a25c03fa65ae6e0e0e3f66bac48d59eceb96a5a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/14/2020
ms.locfileid: "92065902"
---
# <a name="the-load-method"></a>Метод Load
Существует несколько сценариев, в которых может потребоваться загрузка сущностей из базы данных в контекст без немедленного выполнения каких-либо действий с этими сущностями. Хорошим примером этого является загрузка сущностей для привязки данных, как описано в разделе [локальные данные](xref:ef6/querying/local-data). Один из распространенных способов сделать это — написать запрос LINQ, а затем вызвать метод ToList, только чтобы немедленно отменить созданный список. Метод расширения нагрузки работает так же, как ToList, за исключением того, что он полностью предотвращает создание списка.  

Методы, представленные в этом разделе, также применимы к моделям, созданным с помощью Code First и конструктора EF.  

Ниже приведены два примера использования Load. Первый берется из Windows Forms приложения привязки данных, где Load используется для запроса сущностей перед привязкой к локальной коллекции, как описано в разделе [локальные данные](xref:ef6/querying/local-data):  

``` csharp
protected override void OnLoad(EventArgs e)
{
    base.OnLoad(e);

    _context = new ProductContext();

    _context.Categories.Load();
    categoryBindingSource.DataSource = _context.Categories.Local.ToBindingList();
}
```  

Во втором примере показано использование Load для загрузки фильтрованной коллекции связанных сущностей, как описано в разделе [Загрузка связанных сущностей](xref:ef6/querying/related-data):  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Find(1);

    // Load the posts with the 'entity-framework' tag related to a given blog
    context.Entry(blog)
        .Collection(b => b.Posts)
        .Query()
        .Where(p => p.Tags.Contains("entity-framework"))
        .Load();
}
```  
