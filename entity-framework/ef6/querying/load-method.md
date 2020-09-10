---
title: Метод Load — EF6
description: Метод Load в Entity Framework 6
author: divega
ms.date: 10/23/2016
ms.assetid: 03c5a069-b7b4-455f-a16f-ee3b96cc4e28
uid: ef6/querying/load-method
ms.openlocfilehash: 5fbb55b899ae0ee026d42df90f80cc18fe95bfa2
ms.sourcegitcommit: 7c3939504bb9da3f46bea3443638b808c04227c2
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/09/2020
ms.locfileid: "89620322"
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
