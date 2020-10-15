---
title: Определение Дбсетс-EF6
description: Определение Дбсетс в Entity Framework 6
author: ajcvickers
ms.date: 10/23/2016
uid: ef6/modeling/code-first/dbsets
ms.openlocfilehash: dc5ad7f8b4ba32454c702f354b37223007e856e3
ms.sourcegitcommit: 0a25c03fa65ae6e0e0e3f66bac48d59eceb96a5a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/14/2020
ms.locfileid: "92065229"
---
# <a name="defining-dbsets"></a>Определение Дбсетс
При разработке с помощью Code First рабочего процесса определяется производный DbContext, который представляет сеанс с базой данных и предоставляет DbSet для каждого типа в модели. В этом разделе рассматриваются различные способы определения свойств DbSet.  

## <a name="dbcontext-with-dbset-properties"></a>DbContext со свойствами DbSet  

Типичный случай, показанный в Code First примерах, — наличие DbContext с общедоступными свойствами DbSet для типов сущностей вашей модели. Например.  

``` csharp
public class BloggingContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }
    public DbSet<Post> Posts { get; set; }
}
```  

При использовании в Code Firstном режиме это позволит настроить блоги и записи в качестве типов сущностей, а также настроить другие типы, доступные из них. Кроме того, DbContext автоматически вызывает метод задания для каждого из этих свойств, чтобы задать экземпляр соответствующего DbSet.  

## <a name="dbcontext-with-idbset-properties"></a>DbContext со свойствами Идбсет  

Существуют ситуации, например при создании макетов или поддельных ситуаций, где более удобно объявлять свойства набора с помощью интерфейса. В таких случаях вместо DbSet можно использовать интерфейс Идбсет. Например.  

``` csharp
public class BloggingContext : DbContext
{
    public IDbSet<Blog> Blogs { get; set; }
    public IDbSet<Post> Posts { get; set; }
}
```  

Этот контекст работает точно так же, как контекст, использующий класс DbSet для своих свойств Set.  

## <a name="dbcontext-with-read-only-set-properties"></a>DbContext с заданными свойствами только для чтения  

Если вы не хотите предоставлять общедоступные методы задания для свойств DbSet или Идбсет, вы можете создать свойства только для чтения и самостоятельно создать экземпляры набора. Например.  

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

Обратите внимание, что DbContext кэширует экземпляр DbSet, возвращенный методом set, чтобы каждое из этих свойств возвращало один и тот же экземпляр при каждом вызове.  

Обнаружение типов сущностей для Code First работает таким же образом, как и для свойств с открытыми методами получения и заданиями.  
