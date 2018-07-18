---
title: Работа с прокси - EF6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: 869ee4dc-06f1-471d-8e0e-0a1a2bc59c30
caps.latest.revision: 3
ms.openlocfilehash: 4632e246d28a3cd53dabe5ac76e44f4538739abc
ms.sourcegitcommit: f05e7b62584cf228f17390bb086a61d505712e1b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/08/2018
ms.locfileid: "39121878"
---
# <a name="working-with-proxies"></a><span data-ttu-id="9c52f-102">Работа с прокси</span><span class="sxs-lookup"><span data-stu-id="9c52f-102">Working with proxies</span></span>
<span data-ttu-id="9c52f-103">При создании экземпляров типов сущностей POCO, Entity Framework часто создает экземпляры динамически создаваемого производный тип, который выступает в качестве прокси для сущности.</span><span class="sxs-lookup"><span data-stu-id="9c52f-103">When creating instances of POCO entity types, Entity Framework often creates instances of a dynamically generated derived type that acts as a proxy for the entity.</span></span> <span data-ttu-id="9c52f-104">Этот прокси-сервер переопределяет некоторые виртуальные свойства сущности, которую нужно вставить обработчик для выполнения действий автоматически, при обращении к свойству.</span><span class="sxs-lookup"><span data-stu-id="9c52f-104">This proxy overrides some virtual properties of the entity to insert hooks for performing actions automatically when the property is accessed.</span></span> <span data-ttu-id="9c52f-105">Например этот механизм используется для поддержки отложенная загрузка связей.</span><span class="sxs-lookup"><span data-stu-id="9c52f-105">For example, this mechanism is used to support lazy loading of relationships.</span></span> <span data-ttu-id="9c52f-106">Методы, представленные в этом разделе, также применимы к моделям, созданным с помощью Code First и конструктора EF.</span><span class="sxs-lookup"><span data-stu-id="9c52f-106">The techniques shown in this topic apply equally to models created with Code First and the EF Designer.</span></span>  

## <a name="disabling-proxy-creation"></a><span data-ttu-id="9c52f-107">Отключение создания прокси-сервера</span><span class="sxs-lookup"><span data-stu-id="9c52f-107">Disabling proxy creation</span></span>  

<span data-ttu-id="9c52f-108">Иногда полезно запретить Entity Framework создавать экземпляры прокси-сервера.</span><span class="sxs-lookup"><span data-stu-id="9c52f-108">Sometimes it is useful to prevent Entity Framework from creating proxy instances.</span></span> <span data-ttu-id="9c52f-109">Например сериализация экземпляров без прокси-сервера — значительно легче, чем сериализации экземпляров прокси-сервера.</span><span class="sxs-lookup"><span data-stu-id="9c52f-109">For example, serializing non-proxy instances is considerably easier than serializing proxy instances.</span></span> <span data-ttu-id="9c52f-110">Создание прокси-сервер можно отключить, сняв флаг ProxyCreationEnabled.</span><span class="sxs-lookup"><span data-stu-id="9c52f-110">Proxy creation can be turned off by clearing the ProxyCreationEnabled flag.</span></span> <span data-ttu-id="9c52f-111">Централизованно, это можно сделать это в конструкторе контекста.</span><span class="sxs-lookup"><span data-stu-id="9c52f-111">One place you could do this is in the constructor of your context.</span></span> <span data-ttu-id="9c52f-112">Пример:</span><span class="sxs-lookup"><span data-stu-id="9c52f-112">For example:</span></span>  

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

<span data-ttu-id="9c52f-113">Обратите внимание, что EF не будет создавать учетные записи-посредники для типов там, где нет ничего для прокси-сервера для выполнения.</span><span class="sxs-lookup"><span data-stu-id="9c52f-113">Note that the EF will not create proxies for types where there is nothing for the proxy to do.</span></span> <span data-ttu-id="9c52f-114">Это означает, что прокси-серверы можно избежать благодаря использованию типов, которые являются запечатанными или нет виртуальных свойств.</span><span class="sxs-lookup"><span data-stu-id="9c52f-114">This means that you can also avoid proxies by having types that are sealed and/or have no virtual properties.</span></span>  

## <a name="explicitly-creating-an-instance-of-a-proxy"></a><span data-ttu-id="9c52f-115">Явного создания экземпляра прокси-сервера</span><span class="sxs-lookup"><span data-stu-id="9c52f-115">Explicitly creating an instance of a proxy</span></span>  

<span data-ttu-id="9c52f-116">Экземпляр прокси-сервера не создаются при создании экземпляра сущности с помощью оператора new.</span><span class="sxs-lookup"><span data-stu-id="9c52f-116">A proxy instance will not be created if you create an instance of an entity using the new operator.</span></span> <span data-ttu-id="9c52f-117">Это может не быть проблемой, но если вам нужно создать экземпляр прокси-сервера (например, благодаря чему отложенной загрузки или прокси-сервера отслеживания изменений будет работать) затем это можно сделать с помощью метода Create объекта DbSet.</span><span class="sxs-lookup"><span data-stu-id="9c52f-117">This may not be a problem, but if you need to create a proxy instance (for example, so that lazy loading or proxy change tracking will work) then you can do so using the Create method of DbSet.</span></span> <span data-ttu-id="9c52f-118">Пример:</span><span class="sxs-lookup"><span data-stu-id="9c52f-118">For example:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Create();
}
```  

<span data-ttu-id="9c52f-119">Если вы хотите создать экземпляр производного типа сущности используется универсальная версия инструкции Create.</span><span class="sxs-lookup"><span data-stu-id="9c52f-119">The generic version of Create can be used if you want to create an instance of a derived entity type.</span></span> <span data-ttu-id="9c52f-120">Пример:</span><span class="sxs-lookup"><span data-stu-id="9c52f-120">For example:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    var admin = context.Users.Create<Administrator>();
}
```  

<span data-ttu-id="9c52f-121">Обратите внимание, что метод Create не добавить или присоединить к контексту созданной сущности.</span><span class="sxs-lookup"><span data-stu-id="9c52f-121">Note that the Create method does not add or attach the created entity to the context.</span></span>  

<span data-ttu-id="9c52f-122">Обратите внимание, что метод Create просто создаст экземпляр самого типа сущности при создании прокси-тип сущности будет иметь значение не, поскольку он бы не выполняет никаких действий.</span><span class="sxs-lookup"><span data-stu-id="9c52f-122">Note that the Create method will just create an instance of the entity type itself if creating a proxy type for the entity would have no value because it would not do anything.</span></span> <span data-ttu-id="9c52f-123">Например если тип сущности является запечатанным и/или не имеет виртуальных свойств, а затем создать будет просто создайте экземпляр типа сущности.</span><span class="sxs-lookup"><span data-stu-id="9c52f-123">For example, if the entity type is sealed and/or has no virtual properties then Create will just create an instance of the entity type.</span></span>  

## <a name="getting-the-actual-entity-type-from-a-proxy-type"></a><span data-ttu-id="9c52f-124">Получение типа фактические сущности из типа прокси-сервера</span><span class="sxs-lookup"><span data-stu-id="9c52f-124">Getting the actual entity type from a proxy type</span></span>  

<span data-ttu-id="9c52f-125">Прокси-типы имеют имена, которые выглядят примерно следующим образом:</span><span class="sxs-lookup"><span data-stu-id="9c52f-125">Proxy types have names that look something like this:</span></span>  

<span data-ttu-id="9c52f-126">System.Data.Entity.DynamicProxies.Blog_5E43C6C196972BF0754973E48C9C941092D86818CD94005E9A759B70BF6E48E6</span><span class="sxs-lookup"><span data-stu-id="9c52f-126">System.Data.Entity.DynamicProxies.Blog_5E43C6C196972BF0754973E48C9C941092D86818CD94005E9A759B70BF6E48E6</span></span>  

<span data-ttu-id="9c52f-127">Тип сущности можно найти для этого типа прокси-сервера, с помощью метода GetObjectType от ObjectContext.</span><span class="sxs-lookup"><span data-stu-id="9c52f-127">You can find the entity type for this proxy type using the GetObjectType method from ObjectContext.</span></span> <span data-ttu-id="9c52f-128">Пример:</span><span class="sxs-lookup"><span data-stu-id="9c52f-128">For example:</span></span>  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Find(1);
    var entityType = ObjectContext.GetObjectType(blog.GetType());
}
```  

<span data-ttu-id="9c52f-129">Обратите внимание, что если тип, передаваемый GetObjectType является экземпляром типа сущности, не является типом прокси-сервера, затем тип сущности, все равно будет возвращена.</span><span class="sxs-lookup"><span data-stu-id="9c52f-129">Note that if the type passed to GetObjectType is an instance of an entity type that is not a proxy type then the type of entity is still returned.</span></span> <span data-ttu-id="9c52f-130">Это означает, что этот метод всегда можно использовать для получения фактического объекта типа без каких-либо других проверок для просмотра, если тип является типом прокси-сервера, или нет.</span><span class="sxs-lookup"><span data-stu-id="9c52f-130">This means you can always use this method to get the actual entity type without any other checking to see if the type is a proxy type or not.</span></span>  
