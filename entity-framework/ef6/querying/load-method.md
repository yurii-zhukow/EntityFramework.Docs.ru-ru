---
title: Метод Load - EF6
author: divega
ms.date: 2016-10-23
ms.assetid: 03c5a069-b7b4-455f-a16f-ee3b96cc4e28
ms.openlocfilehash: f7e8410b8fb8b5c3e86c51cd61868604a7566d0c
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/27/2018
ms.locfileid: "42996655"
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
