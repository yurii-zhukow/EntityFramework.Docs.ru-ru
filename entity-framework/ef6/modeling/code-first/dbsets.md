---
title: Определение Дбсетс-EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 4528a509-ace7-4dfb-8065-1b833f5e03a0
ms.openlocfilehash: 045b22d2b9d26804948689dd7c9dd694baadda7e
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/06/2020
ms.locfileid: "78415781"
---
# <a name="defining-dbsets"></a><span data-ttu-id="d58f0-102">Определение Дбсетс</span><span class="sxs-lookup"><span data-stu-id="d58f0-102">Defining DbSets</span></span>
<span data-ttu-id="d58f0-103">При разработке с помощью Code First рабочего процесса определяется производный DbContext, который представляет сеанс с базой данных и предоставляет DbSet для каждого типа в модели.</span><span class="sxs-lookup"><span data-stu-id="d58f0-103">When developing with the Code First workflow you define a derived DbContext that represents your session with the database and exposes a DbSet for each type in your model.</span></span> <span data-ttu-id="d58f0-104">В этом разделе рассматриваются различные способы определения свойств DbSet.</span><span class="sxs-lookup"><span data-stu-id="d58f0-104">This topic covers the various ways you can define the DbSet properties.</span></span>  

## <a name="dbcontext-with-dbset-properties"></a><span data-ttu-id="d58f0-105">DbContext со свойствами DbSet</span><span class="sxs-lookup"><span data-stu-id="d58f0-105">DbContext with DbSet properties</span></span>  

<span data-ttu-id="d58f0-106">Типичный случай, показанный в Code First примерах, — наличие DbContext с общедоступными свойствами DbSet для типов сущностей вашей модели.</span><span class="sxs-lookup"><span data-stu-id="d58f0-106">The common case shown in Code First examples is to have a DbContext with public automatic DbSet properties for the entity types of your model.</span></span> <span data-ttu-id="d58f0-107">Пример:</span><span class="sxs-lookup"><span data-stu-id="d58f0-107">For example:</span></span>  

``` csharp
public class BloggingContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }
    public DbSet<Post> Posts { get; set; }
}
```  

<span data-ttu-id="d58f0-108">При использовании в Code Firstном режиме это позволит настроить блоги и записи в качестве типов сущностей, а также настроить другие типы, доступные из них.</span><span class="sxs-lookup"><span data-stu-id="d58f0-108">When used in Code First mode, this will configure Blogs and Posts as entity types, as well as configuring other types reachable from these.</span></span> <span data-ttu-id="d58f0-109">Кроме того, DbContext автоматически вызывает метод задания для каждого из этих свойств, чтобы задать экземпляр соответствующего DbSet.</span><span class="sxs-lookup"><span data-stu-id="d58f0-109">In addition DbContext will automatically call the setter for each of these properties to set an instance of the appropriate DbSet.</span></span>  

## <a name="dbcontext-with-idbset-properties"></a><span data-ttu-id="d58f0-110">DbContext со свойствами Идбсет</span><span class="sxs-lookup"><span data-stu-id="d58f0-110">DbContext with IDbSet properties</span></span>  

<span data-ttu-id="d58f0-111">Существуют ситуации, например при создании макетов или поддельных ситуаций, где более удобно объявлять свойства набора с помощью интерфейса.</span><span class="sxs-lookup"><span data-stu-id="d58f0-111">There are situations, such as when creating mocks or fakes, where it is more useful to declare your set properties using an interface.</span></span> <span data-ttu-id="d58f0-112">В таких случаях вместо DbSet можно использовать интерфейс Идбсет.</span><span class="sxs-lookup"><span data-stu-id="d58f0-112">In such cases the IDbSet interface can be used in place of DbSet.</span></span> <span data-ttu-id="d58f0-113">Пример:</span><span class="sxs-lookup"><span data-stu-id="d58f0-113">For example:</span></span>  

``` csharp
public class BloggingContext : DbContext
{
    public IDbSet<Blog> Blogs { get; set; }
    public IDbSet<Post> Posts { get; set; }
}
```  

<span data-ttu-id="d58f0-114">Этот контекст работает точно так же, как контекст, использующий класс DbSet для своих свойств Set.</span><span class="sxs-lookup"><span data-stu-id="d58f0-114">This context works in exactly the same way as the context that uses the DbSet class for its set properties.</span></span>  

## <a name="dbcontext-with-read-only-set-properties"></a><span data-ttu-id="d58f0-115">DbContext с заданными свойствами только для чтения</span><span class="sxs-lookup"><span data-stu-id="d58f0-115">DbContext with read-only set properties</span></span>  

<span data-ttu-id="d58f0-116">Если вы не хотите предоставлять общедоступные методы задания для свойств DbSet или Идбсет, вы можете создать свойства только для чтения и самостоятельно создать экземпляры набора.</span><span class="sxs-lookup"><span data-stu-id="d58f0-116">If you do not wish to expose public setters for your DbSet or IDbSet properties you can instead create read-only properties and create the set instances yourself.</span></span> <span data-ttu-id="d58f0-117">Пример:</span><span class="sxs-lookup"><span data-stu-id="d58f0-117">For example:</span></span>  

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

<span data-ttu-id="d58f0-118">Обратите внимание, что DbContext кэширует экземпляр DbSet, возвращенный методом set, чтобы каждое из этих свойств возвращало один и тот же экземпляр при каждом вызове.</span><span class="sxs-lookup"><span data-stu-id="d58f0-118">Note that DbContext caches the instance of DbSet returned from the Set method so that each of these properties will return the same instance every time it is called.</span></span>  

<span data-ttu-id="d58f0-119">Обнаружение типов сущностей для Code First работает таким же образом, как и для свойств с открытыми методами получения и заданиями.</span><span class="sxs-lookup"><span data-stu-id="d58f0-119">Discovery of entity types for Code First works in the same way here as it does for properties with public getters and setters.</span></span>  
