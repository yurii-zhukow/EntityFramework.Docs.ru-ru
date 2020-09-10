---
title: Работа с прокси-серверами — EF6
description: Работа с учетными записями-посредниками в Entity Framework 6
author: divega
ms.date: 10/23/2016
ms.assetid: 869ee4dc-06f1-471d-8e0e-0a1a2bc59c30
uid: ef6/fundamentals/proxies
ms.openlocfilehash: 26493ecf1a894a1cd421f574de38678661f324a0
ms.sourcegitcommit: 7c3939504bb9da3f46bea3443638b808c04227c2
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/09/2020
ms.locfileid: "89618365"
---
# <a name="working-with-proxies"></a><span data-ttu-id="1fda2-103">Работа с учетными записями-посредниками</span><span class="sxs-lookup"><span data-stu-id="1fda2-103">Working with proxies</span></span>
<span data-ttu-id="1fda2-104">При создании экземпляров типов сущностей POCO Entity Framework часто создает экземпляры динамически создаваемого производного типа, который выступает в качестве прокси-сервера для сущности.</span><span class="sxs-lookup"><span data-stu-id="1fda2-104">When creating instances of POCO entity types, Entity Framework often creates instances of a dynamically generated derived type that acts as a proxy for the entity.</span></span> <span data-ttu-id="1fda2-105">Этот прокси-сервер переопределяет некоторые виртуальные свойства сущности, чтобы вставлять обработчики для выполнения действий автоматически при обращении к свойству.</span><span class="sxs-lookup"><span data-stu-id="1fda2-105">This proxy overrides some virtual properties of the entity to insert hooks for performing actions automatically when the property is accessed.</span></span> <span data-ttu-id="1fda2-106">Например, этот механизм используется для поддержки отложенной загрузки связей.</span><span class="sxs-lookup"><span data-stu-id="1fda2-106">For example, this mechanism is used to support lazy loading of relationships.</span></span> <span data-ttu-id="1fda2-107">Методы, представленные в этом разделе, также применимы к моделям, созданным с помощью Code First и конструктора EF.</span><span class="sxs-lookup"><span data-stu-id="1fda2-107">The techniques shown in this topic apply equally to models created with Code First and the EF Designer.</span></span>  

## <a name="disabling-proxy-creation"></a><span data-ttu-id="1fda2-108">Отключение создания прокси-сервера</span><span class="sxs-lookup"><span data-stu-id="1fda2-108">Disabling proxy creation</span></span>  

<span data-ttu-id="1fda2-109">Иногда бывает полезно предотвратить создание экземпляров прокси-сервера Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="1fda2-109">Sometimes it is useful to prevent Entity Framework from creating proxy instances.</span></span> <span data-ttu-id="1fda2-110">Например, сериализация экземпляров, не являющихся прокси, значительно упрощается, чем сериализация экземпляров прокси-сервера.</span><span class="sxs-lookup"><span data-stu-id="1fda2-110">For example, serializing non-proxy instances is considerably easier than serializing proxy instances.</span></span> <span data-ttu-id="1fda2-111">Создание прокси-сервера может быть отключено путем снятия флага Проксикреатионенаблед.</span><span class="sxs-lookup"><span data-stu-id="1fda2-111">Proxy creation can be turned off by clearing the ProxyCreationEnabled flag.</span></span> <span data-ttu-id="1fda2-112">В одном месте это можно сделать в конструкторе контекста.</span><span class="sxs-lookup"><span data-stu-id="1fda2-112">One place you could do this is in the constructor of your context.</span></span> <span data-ttu-id="1fda2-113">Пример:</span><span class="sxs-lookup"><span data-stu-id="1fda2-113">For example:</span></span>  

``` csharp
public class BloggingContext : DbContext
{
    public BloggingContext()
    {
        this.Configuration.ProxyCreationEnabled = false;
    }  

    public DbSet<Blog> Blogs { get; set; }
    public DbSet<Post> Posts { get; set; }
}
```  

<span data-ttu-id="1fda2-114">Обратите внимание, что EF не будет создавать прокси для типов, где нет никаких действий для прокси-сервера.</span><span class="sxs-lookup"><span data-stu-id="1fda2-114">Note that the EF will not create proxies for types where there is nothing for the proxy to do.</span></span> <span data-ttu-id="1fda2-115">Это означает, что вы также можете избежать прокси-серверов, применяя типы, которые являются запечатанными и/или не имеют виртуальных свойств.</span><span class="sxs-lookup"><span data-stu-id="1fda2-115">This means that you can also avoid proxies by having types that are sealed and/or have no virtual properties.</span></span>  

## <a name="explicitly-creating-an-instance-of-a-proxy"></a><span data-ttu-id="1fda2-116">Явное создание экземпляра прокси-сервера</span><span class="sxs-lookup"><span data-stu-id="1fda2-116">Explicitly creating an instance of a proxy</span></span>  

<span data-ttu-id="1fda2-117">Экземпляр прокси-сервера не будет создан при создании экземпляра сущности с помощью оператора New.</span><span class="sxs-lookup"><span data-stu-id="1fda2-117">A proxy instance will not be created if you create an instance of an entity using the new operator.</span></span> <span data-ttu-id="1fda2-118">Это может не быть проблемой, но если вам нужно создать экземпляр прокси-сервера (например, чтобы можно было работать с отложенной загрузкой или отслеживанием изменений прокси-сервера), вы можете сделать это с помощью метода Create объекта DbSet.</span><span class="sxs-lookup"><span data-stu-id="1fda2-118">This may not be a problem, but if you need to create a proxy instance (for example, so that lazy loading or proxy change tracking will work) then you can do so using the Create method of DbSet.</span></span> <span data-ttu-id="1fda2-119">Пример:</span><span class="sxs-lookup"><span data-stu-id="1fda2-119">For example:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Create();
}
```  

<span data-ttu-id="1fda2-120">Универсальная версия Create может использоваться, если требуется создать экземпляр производного типа сущности.</span><span class="sxs-lookup"><span data-stu-id="1fda2-120">The generic version of Create can be used if you want to create an instance of a derived entity type.</span></span> <span data-ttu-id="1fda2-121">Пример:</span><span class="sxs-lookup"><span data-stu-id="1fda2-121">For example:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    var admin = context.Users.Create<Administrator>();
}
```  

<span data-ttu-id="1fda2-122">Обратите внимание, что метод Create не добавляет или не прикрепляет созданную сущность к контексту.</span><span class="sxs-lookup"><span data-stu-id="1fda2-122">Note that the Create method does not add or attach the created entity to the context.</span></span>  

<span data-ttu-id="1fda2-123">Обратите внимание, что метод Create просто создает экземпляр самого типа сущности, если создание прокси-типа для сущности не будет иметь значения, так как это не будет делать ничего.</span><span class="sxs-lookup"><span data-stu-id="1fda2-123">Note that the Create method will just create an instance of the entity type itself if creating a proxy type for the entity would have no value because it would not do anything.</span></span> <span data-ttu-id="1fda2-124">Например, если тип сущности — sealed и (или) не имеет виртуальных свойств, то при создании будет просто создан экземпляр типа сущности.</span><span class="sxs-lookup"><span data-stu-id="1fda2-124">For example, if the entity type is sealed and/or has no virtual properties then Create will just create an instance of the entity type.</span></span>  

## <a name="getting-the-actual-entity-type-from-a-proxy-type"></a><span data-ttu-id="1fda2-125">Получение фактического типа сущности из прокси-типа</span><span class="sxs-lookup"><span data-stu-id="1fda2-125">Getting the actual entity type from a proxy type</span></span>  

<span data-ttu-id="1fda2-126">Имена типов прокси-сервера выглядят примерно так:</span><span class="sxs-lookup"><span data-stu-id="1fda2-126">Proxy types have names that look something like this:</span></span>  

<span data-ttu-id="1fda2-127">System. Data. Entity. Динамикпроксиес. Blog_5E43C6C196972BF0754973E48C9C941092D86818CD94005E9A759B70BF6E48E6</span><span class="sxs-lookup"><span data-stu-id="1fda2-127">System.Data.Entity.DynamicProxies.Blog_5E43C6C196972BF0754973E48C9C941092D86818CD94005E9A759B70BF6E48E6</span></span>  

<span data-ttu-id="1fda2-128">Тип сущности для этого типа прокси можно найти с помощью метода Жетобжекттипе из ObjectContext.</span><span class="sxs-lookup"><span data-stu-id="1fda2-128">You can find the entity type for this proxy type using the GetObjectType method from ObjectContext.</span></span> <span data-ttu-id="1fda2-129">Пример:</span><span class="sxs-lookup"><span data-stu-id="1fda2-129">For example:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Find(1);
    var entityType = ObjectContext.GetObjectType(blog.GetType());
}
```  

<span data-ttu-id="1fda2-130">Обратите внимание, что, если тип, переданный в Жетобжекттипе, является экземпляром типа сущности, который не является прокси-типом, то возвращается тип сущности.</span><span class="sxs-lookup"><span data-stu-id="1fda2-130">Note that if the type passed to GetObjectType is an instance of an entity type that is not a proxy type then the type of entity is still returned.</span></span> <span data-ttu-id="1fda2-131">Это означает, что этот метод всегда можно использовать для получения фактического типа сущности без какой-либо другой проверки на наличие типа прокси.</span><span class="sxs-lookup"><span data-stu-id="1fda2-131">This means you can always use this method to get the actual entity type without any other checking to see if the type is a proxy type or not.</span></span>  
