---
title: Определение DbSets - EF6
author: divega
ms.date: 2016-10-23
ms.assetid: 4528a509-ace7-4dfb-8065-1b833f5e03a0
ms.openlocfilehash: cc45ed1ceb20bc90090adb3e93c10651c69c9a6a
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/27/2018
ms.locfileid: "42993861"
---
# <a name="defining-dbsets"></a><span data-ttu-id="dd728-102">Определение DbSets</span><span class="sxs-lookup"><span data-stu-id="dd728-102">Defining DbSets</span></span>
<span data-ttu-id="dd728-103">При разработке с помощью Code First рабочего процесса вы определяете производном DbContext, который представляет сеанс с базой данных и предоставляет DbSet для каждого типа в модели.</span><span class="sxs-lookup"><span data-stu-id="dd728-103">When developing with the Code First workflow you define a derived DbContext that represents your session with the database and exposes a DbSet for each type in your model.</span></span> <span data-ttu-id="dd728-104">В этом разделе рассматриваются различные способы, можно определить свойства DbSet.</span><span class="sxs-lookup"><span data-stu-id="dd728-104">This topic covers the various ways you can define the DbSet properties.</span></span>  

## <a name="dbcontext-with-dbset-properties"></a><span data-ttu-id="dd728-105">DbContext с помощью свойства DbSet</span><span class="sxs-lookup"><span data-stu-id="dd728-105">DbContext with DbSet properties</span></span>  

<span data-ttu-id="dd728-106">Распространенный случай, показано в примерах Code First является DbContext с помощью общедоступного автоматические свойства DbSet для типов сущностей модели.</span><span class="sxs-lookup"><span data-stu-id="dd728-106">The common case shown in Code First examples is to have a DbContext with public automatic DbSet properties for the entity types of your model.</span></span> <span data-ttu-id="dd728-107">Пример:</span><span class="sxs-lookup"><span data-stu-id="dd728-107">For example:</span></span>  

``` csharp
public class BloggingContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }
    public DbSet<Post> Posts { get; set; }
}
```  

<span data-ttu-id="dd728-108">При использовании в режиме Code First, это настроит блогов и записей как типы сущностей, а также настройка других доступен на основе этих типов.</span><span class="sxs-lookup"><span data-stu-id="dd728-108">When used in Code First mode, this will configure Blogs and Posts as entity types, as well as configuring other types reachable from these.</span></span> <span data-ttu-id="dd728-109">Кроме DbContext автоматически вызывает метод задания для каждого из этих свойств, чтобы задать экземпляр соответствующего DbSet.</span><span class="sxs-lookup"><span data-stu-id="dd728-109">In addition DbContext will automatically call the setter for each of these properties to set an instance of the appropriate DbSet.</span></span>  

## <a name="dbcontext-with-idbset-properties"></a><span data-ttu-id="dd728-110">DbContext со свойствами IDbSet</span><span class="sxs-lookup"><span data-stu-id="dd728-110">DbContext with IDbSet properties</span></span>  

<span data-ttu-id="dd728-111">Существуют ситуации, например при создании макетов или fakes, где это удобнее для объявления свойств набора с помощью интерфейса.</span><span class="sxs-lookup"><span data-stu-id="dd728-111">There are situations, such as when creating mocks or fakes, where it is more useful to declare your set properties using an interface.</span></span> <span data-ttu-id="dd728-112">В таких случаях IDbSet интерфейс можно использовать вместо DbSet.</span><span class="sxs-lookup"><span data-stu-id="dd728-112">In such cases the IDbSet interface can be used in place of DbSet.</span></span> <span data-ttu-id="dd728-113">Пример:</span><span class="sxs-lookup"><span data-stu-id="dd728-113">For example:</span></span>  

``` csharp
public class BloggingContext : DbContext
{
    public IDbSet<Blog> Blogs { get; set; }
    public IDbSet<Post> Posts { get; set; }
}
```  

<span data-ttu-id="dd728-114">Этот контекст работает точно так же, как контекст, который использует класс DbSet для его задания свойств.</span><span class="sxs-lookup"><span data-stu-id="dd728-114">This context works in exactly the same way as the context that uses the DbSet class for its set properties.</span></span>  

## <a name="dbcontext-with-read-only-set-properties"></a><span data-ttu-id="dd728-115">DbContext со свойствами только для чтения</span><span class="sxs-lookup"><span data-stu-id="dd728-115">DbContext with read-only set properties</span></span>  

<span data-ttu-id="dd728-116">Если вы не хотите предоставлять открытые методы задания свойств DbSet или IDbSet вместо этого можно создать свойства только для чтения и создания экземпляров набора самостоятельно.</span><span class="sxs-lookup"><span data-stu-id="dd728-116">If you do not wish to expose public setters for your DbSet or IDbSet properties you can instead create read-only properties and create the set instances yourself.</span></span> <span data-ttu-id="dd728-117">Пример:</span><span class="sxs-lookup"><span data-stu-id="dd728-117">For example:</span></span>  

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

<span data-ttu-id="dd728-118">Обратите внимание на то, что DbContext кэширует экземпляр DbSet, возвращается из метода Set, чтобы каждое из этих свойств возвращает тот же экземпляр при каждом вызове.</span><span class="sxs-lookup"><span data-stu-id="dd728-118">Note that DbContext caches the instance of DbSet returned from the Set method so that each of these properties will return the same instance every time it is called.</span></span>  

<span data-ttu-id="dd728-119">Обнаружение типов сущностей для Code First работает так же, так и для свойства с помощью общедоступного методы get и set.</span><span class="sxs-lookup"><span data-stu-id="dd728-119">Discovery of entity types for Code First works in the same way here as it does for properties with public getters and setters.</span></span>  
