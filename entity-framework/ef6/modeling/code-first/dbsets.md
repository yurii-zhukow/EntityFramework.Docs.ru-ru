---
title: Определение DbSets - EF6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: 4528a509-ace7-4dfb-8065-1b833f5e03a0
caps.latest.revision: 3
ms.openlocfilehash: 8a495c6ce74d9a346a84b0e10fb28395f4dce07b
ms.sourcegitcommit: 00cb52625b57c1ea339ded1454179fe89b6bcfea
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/16/2018
ms.locfileid: "39123267"
---
# <a name="defining-dbsets"></a>Определение DbSets
При разработке с помощью Code First рабочего процесса вы определяете производном DbContext, который представляет сеанс с базой данных и предоставляет DbSet для каждого типа в модели. В этом разделе рассматриваются различные способы, можно определить свойства DbSet.  

## <a name="dbcontext-with-dbset-properties"></a>DbContext с помощью свойства DbSet  

Распространенный случай, показано в примерах Code First является DbContext с помощью общедоступного автоматические свойства DbSet для типов сущностей модели. Пример:  

``` csharp
public class BloggingContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }
    public DbSet<Post> Posts { get; set; }
}
```  

При использовании в режиме Code First, это настроит блогов и записей как типы сущностей, а также настройка других доступен на основе этих типов. Кроме DbContext автоматически вызывает метод задания для каждого из этих свойств, чтобы задать экземпляр соответствующего DbSet.  

## <a name="dbcontext-with-idbset-properties"></a>DbContext со свойствами IDbSet  

Существуют ситуации, например при создании макетов или fakes, где это удобнее для объявления свойств набора с помощью интерфейса. В таких случаях IDbSet интерфейс можно использовать вместо DbSet. Пример:  

``` csharp
public class BloggingContext : DbContext
{
    public IDbSet<Blog> Blogs { get; set; }
    public IDbSet<Post> Posts { get; set; }
}
```  

Этот контекст работает точно так же, как контекст, который использует класс DbSet для его задания свойств.  

## <a name="dbcontext-with-read-only-set-properties"></a>DbContext со свойствами только для чтения  

Если вы не хотите предоставлять открытые методы задания свойств DbSet или IDbSet вместо этого можно создать свойства только для чтения и создания экземпляров набора самостоятельно. Пример:  

``` csharp
public class BloggingContext : DbContext
{
    public DbSet<Blog> Blogs
    {
        get { return Set<Blog>(); }
    }

    public DbSet<Post> Posts
    {
        get { return Set<Post>(); }
    }
}
```  

Обратите внимание на то, что DbContext кэширует экземпляр DbSet, возвращается из метода Set, чтобы каждое из этих свойств возвращает тот же экземпляр при каждом вызове.  

Обнаружение типов сущностей для Code First работает так же, так и для свойства с помощью общедоступного методы get и set.  
