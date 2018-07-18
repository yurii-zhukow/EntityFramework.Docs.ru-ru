---
title: Метод Load - EF6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: 03c5a069-b7b4-455f-a16f-ee3b96cc4e28
caps.latest.revision: 3
ms.openlocfilehash: 83af79220b52de6e3063868fd9bdac56867d49cb
ms.sourcegitcommit: f05e7b62584cf228f17390bb086a61d505712e1b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/08/2018
ms.locfileid: "39122313"
---
# <a name="the-load-method"></a>Метод Load
Существует несколько сценариев, где может потребоваться загрузка сущностей из базы данных в контекст, не выполняя никаких действий с этими сущностями немедленно. Хорошим примером этого загрузка сущностей для привязки данных, как описано в разделе [локальных данных](~/ef6/querying/local-data.md). Один из способов сделать это — написать запрос LINQ, а затем вызывать ToList в его, только для немедленного удаления созданный список. Метода расширения Load работает так же, как ToList, за исключением того, что позволяет избежать создания списка полностью.  

Методы, представленные в этом разделе, также применимы к моделям, созданным с помощью Code First и конструктора EF.  

Ниже приведены два примера использования нагрузки. Первый берется из приложения привязки данных Windows Forms, где нагрузки используется для запроса сущностей перед привязкой к локальной коллекции, как описано в разделе [локальных данных](~/ef6/querying/local-data.md):  

``` csharp
protected override void OnLoad(EventArgs e)
{
    base.OnLoad(e);

    _context = new ProductContext();

    _context.Categories.Load();
    categoryBindingSource.DataSource = _context.Categories.Local.ToBindingList();
}
```  

Второй пример показывает использование загрузки для загрузки фильтрованную коллекцию связанных сущностей, как описано в [загрузка связанных сущностей](~/ef6/querying/related-data.md):  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Find(1);

    // Load the posts with the 'entity-framework' tag related to a given blog
    context.Entry(blog)
        .Collection(b => b.Posts)
        .Query()
        .Where(p => p.Tags.Contains("entity-framework")
        .Load();
}
```  
