---
title: Свойства тени — EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 75369266-d2b9-4416-b118-ed238f81f599
ms.technology: entity-framework-core
uid: core/modeling/shadow-properties
ms.openlocfilehash: 8c7f70789ddc6ebd24f9bfae931069834db593c2
ms.sourcegitcommit: 860ec5d047342fbc4063a0de881c9861cc1f8813
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/05/2017
ms.locfileid: "26053554"
---
# <a name="shadow-properties"></a><span data-ttu-id="62577-102">Свойства тени</span><span class="sxs-lookup"><span data-stu-id="62577-102">Shadow Properties</span></span>

<span data-ttu-id="62577-103">Замещения свойств являются свойствами, которые не определены в классе сущностей .NET, но определены для данного типа сущности в EF базовой модели.</span><span class="sxs-lookup"><span data-stu-id="62577-103">Shadow properties are properties that are not defined in your .NET entity class but are defined for that entity type in the EF Core model.</span></span> <span data-ttu-id="62577-104">Значение и состояние этих свойств поддерживается исключительно в объекте отслеживания изменений.</span><span class="sxs-lookup"><span data-stu-id="62577-104">The value and state of these properties is maintained purely in the Change Tracker.</span></span>

<span data-ttu-id="62577-105">Замещения свойств полезны в тех случаях, когда в базе данных, которые не должны быть предоставлены для типов сущностей, сопоставленных данных.</span><span class="sxs-lookup"><span data-stu-id="62577-105">Shadow properties are useful when there is data in the database that should not be exposed on the mapped entity types.</span></span> <span data-ttu-id="62577-106">Чаще всего используются для свойства внешнего ключа, связь между двумя сущностями, представленного значение внешнего ключа в базе данных, когда связь осуществляется для типов сущностей, с помощью свойств навигации между типами сущностей.</span><span class="sxs-lookup"><span data-stu-id="62577-106">They are most often used for foreign key properties, where the relationship between two entities is represented by a foreign key value in the database, but the relationship is managed on the entity types using navigation properties between the entity types.</span></span>

<span data-ttu-id="62577-107">Значения свойств можно получить и изменить с помощью `ChangeTracker` API.</span><span class="sxs-lookup"><span data-stu-id="62577-107">Shadow property values can be obtained and changed through the `ChangeTracker` API.</span></span>

``` csharp
   context.Entry(myBlog).Property("LastUpdated").CurrentValue = DateTime.Now;
```

<span data-ttu-id="62577-108">Замещения свойств можно ссылаться в запросах LINQ через `EF.Property` статический метод.</span><span class="sxs-lookup"><span data-stu-id="62577-108">Shadow properties can be referenced in LINQ queries via the `EF.Property` static method.</span></span>

``` csharp
var blogs = context.Blogs
    .OrderBy(b => EF.Property<DateTime>(b, "LastUpdated"));
```

## <a name="conventions"></a><span data-ttu-id="62577-109">Соглашения</span><span class="sxs-lookup"><span data-stu-id="62577-109">Conventions</span></span>

<span data-ttu-id="62577-110">Замещения свойств могут создаваться по соглашению, если обнаруженные связи, но не свойство внешнего ключа не найден в классе зависимой сущности.</span><span class="sxs-lookup"><span data-stu-id="62577-110">Shadow properties can be created by convention when a relationship is discovered but no foreign key property is found in the dependent entity class.</span></span> <span data-ttu-id="62577-111">В этом случае будут вводиться тени свойство внешнего ключа.</span><span class="sxs-lookup"><span data-stu-id="62577-111">In this case, a shadow foreign key property will be introduced.</span></span> <span data-ttu-id="62577-112">Свойство внешнего ключа тень будет называться `<navigation property name><principal key property name>` (Навигация по зависимой сущности, которое указывает основной сущности, используется для именования).</span><span class="sxs-lookup"><span data-stu-id="62577-112">The shadow foreign key property will be named `<navigation property name><principal key property name>` (the navigation on the dependent entity, which points to the principal entity, is used for the naming).</span></span> <span data-ttu-id="62577-113">Если имя свойства основного ключа содержит имя свойства навигации, а затем имя будет просто `<principal key property name>`.</span><span class="sxs-lookup"><span data-stu-id="62577-113">If the principal key property name includes the name of the navigation property, then the name will just be `<principal key property name>`.</span></span> <span data-ttu-id="62577-114">Если зависимая сущность не свойство навигации, вместо него используется имя типа субъекта.</span><span class="sxs-lookup"><span data-stu-id="62577-114">If there is no navigation property on the dependent entity, then the principal type name is used in its place.</span></span>

<span data-ttu-id="62577-115">Например, следующий листинг кода приведет к `BlogId` проникновения в свойство теневого `Post` сущности.</span><span class="sxs-lookup"><span data-stu-id="62577-115">For example, the following code listing will result in a `BlogId` shadow property being introduced to the `Post` entity.</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/Conventions/Samples/ShadowForeignKey.cs)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }
    public DbSet<Post> Posts { get; set; }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }

    public List<Post> Posts { get; set; }
}

public class Post
{
    public int PostId { get; set; }
    public string Title { get; set; }
    public string Content { get; set; }

    public Blog Blog { get; set; }
}
```

## <a name="data-annotations"></a><span data-ttu-id="62577-116">Заметки к данным</span><span class="sxs-lookup"><span data-stu-id="62577-116">Data Annotations</span></span>

<span data-ttu-id="62577-117">Замещения свойств не могут создаваться с помощью заметок к данным.</span><span class="sxs-lookup"><span data-stu-id="62577-117">Shadow properties can not be created with data annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="62577-118">Fluent API</span><span class="sxs-lookup"><span data-stu-id="62577-118">Fluent API</span></span>

<span data-ttu-id="62577-119">Fluent API можно использовать для настройки свойств тени.</span><span class="sxs-lookup"><span data-stu-id="62577-119">You can use the Fluent API to configure shadow properties.</span></span> <span data-ttu-id="62577-120">После вызова перегруженный строка `Property` можно соединить в цепочку любой конфигурации вызовов, что и для других свойств.</span><span class="sxs-lookup"><span data-stu-id="62577-120">Once you have called the string overload of `Property` you can chain any of the configuration calls you would for other properties.</span></span>

<span data-ttu-id="62577-121">Если имя, указанное в `Property` метода совпадает с именем существующего свойства (свойство теневого или один определенный для класса сущностей), а затем код будет настроить существующие свойства, а не обучаться новое свойство тени.</span><span class="sxs-lookup"><span data-stu-id="62577-121">If the name supplied to the `Property` method matches the name of an existing property (a shadow property or one defined on the entity class), then the code will configure that existing property rather than introducing a new shadow property.</span></span>

<!-- [!code-csharp[Main](samples/core/Modeling/FluentAPI/Samples/ShadowProperty.cs?highlight=7,8)] -->
``` csharp
class MyContext : DbContext
{
    public DbSet<Blog> Blogs { get; set; }

    protected override void OnModelCreating(ModelBuilder modelBuilder)
    {
        modelBuilder.Entity<Blog>()
            .Property<DateTime>("LastUpdated");
    }
}

public class Blog
{
    public int BlogId { get; set; }
    public string Url { get; set; }
}
```
