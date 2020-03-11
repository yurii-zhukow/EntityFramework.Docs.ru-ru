---
title: Метод Load — EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 03c5a069-b7b4-455f-a16f-ee3b96cc4e28
ms.openlocfilehash: bcea8ab2477f44281cd5de824457a72a84ccc766
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/06/2020
ms.locfileid: "78414497"
---
# <a name="the-load-method"></a><span data-ttu-id="940f8-102">Метод Load</span><span class="sxs-lookup"><span data-stu-id="940f8-102">The Load Method</span></span>
<span data-ttu-id="940f8-103">Существует несколько сценариев, в которых может потребоваться загрузка сущностей из базы данных в контекст без немедленного выполнения каких-либо действий с этими сущностями.</span><span class="sxs-lookup"><span data-stu-id="940f8-103">There are several scenarios where you may want to load entities from the database into the context without immediately doing anything with those entities.</span></span> <span data-ttu-id="940f8-104">Хорошим примером этого является загрузка сущностей для привязки данных, как описано в разделе [локальные данные](~/ef6/querying/local-data.md).</span><span class="sxs-lookup"><span data-stu-id="940f8-104">A good example of this is loading entities for data binding as described in [Local Data](~/ef6/querying/local-data.md).</span></span> <span data-ttu-id="940f8-105">Один из распространенных способов сделать это — написать запрос LINQ, а затем вызвать метод ToList, только чтобы немедленно отменить созданный список.</span><span class="sxs-lookup"><span data-stu-id="940f8-105">One common way to do this is to write a LINQ query and then call ToList on it, only to immediately discard the created list.</span></span> <span data-ttu-id="940f8-106">Метод расширения нагрузки работает так же, как ToList, за исключением того, что он полностью предотвращает создание списка.</span><span class="sxs-lookup"><span data-stu-id="940f8-106">The Load extension method works just like ToList except that it avoids the creation of the list altogether.</span></span>  

<span data-ttu-id="940f8-107">Методы, представленные в этом разделе, также применимы к моделям, созданным с помощью Code First и конструктора EF.</span><span class="sxs-lookup"><span data-stu-id="940f8-107">The techniques shown in this topic apply equally to models created with Code First and the EF Designer.</span></span>  

<span data-ttu-id="940f8-108">Ниже приведены два примера использования Load.</span><span class="sxs-lookup"><span data-stu-id="940f8-108">Here are two examples of using Load.</span></span> <span data-ttu-id="940f8-109">Первый берется из Windows Forms приложения привязки данных, где Load используется для запроса сущностей перед привязкой к локальной коллекции, как описано в разделе [локальные данные](~/ef6/querying/local-data.md):</span><span class="sxs-lookup"><span data-stu-id="940f8-109">The first is taken from a Windows Forms data binding application where Load is used to query for entities before binding to the local collection, as described in [Local Data](~/ef6/querying/local-data.md):</span></span>  

``` csharp
protected override void OnLoad(EventArgs e)
{
    base.OnLoad(e);

    _context = new ProductContext();

    _context.Categories.Load();
    categoryBindingSource.DataSource = _context.Categories.Local.ToBindingList();
}
```  

<span data-ttu-id="940f8-110">Во втором примере показано использование Load для загрузки фильтрованной коллекции связанных сущностей, как описано в разделе [Загрузка связанных сущностей](~/ef6/querying/related-data.md):</span><span class="sxs-lookup"><span data-stu-id="940f8-110">The second example shows using Load to load a filtered collection of related entities, as described in [Loading Related Entities](~/ef6/querying/related-data.md):</span></span>  

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
