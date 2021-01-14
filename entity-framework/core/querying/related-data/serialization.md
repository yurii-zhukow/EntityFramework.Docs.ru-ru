---
title: Связанные данные и сериализация — EF Core
description: Сведения о том, как циклы в связанных данных Entity Framework Core могут влиять на платформы сериализации
author: roji
ms.date: 9/8/2020
uid: core/querying/related-data/serialization
ms.openlocfilehash: 3e1152f09493d039032d9d88493b52a5ef425eb9
ms.sourcegitcommit: 032a1767d7a6e42052a005f660b80372c6521e7e
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/12/2021
ms.locfileid: "98129139"
---
# <a name="related-data-and-serialization"></a><span data-ttu-id="990c3-103">Связанные данные и сериализация</span><span class="sxs-lookup"><span data-stu-id="990c3-103">Related data and serialization</span></span>

<span data-ttu-id="990c3-104">Так как EF Core автоматически исправляет свойства навигации, вы можете завершить циклы в графе объектов.</span><span class="sxs-lookup"><span data-stu-id="990c3-104">Because EF Core automatically does fix-up of navigation properties, you can end up with cycles in your object graph.</span></span> <span data-ttu-id="990c3-105">Например, загрузка блога и связанных записей приведет к созданию объекта блога, который ссылается на коллекцию записей.</span><span class="sxs-lookup"><span data-stu-id="990c3-105">For example, loading a blog and its related posts will result in a blog object that references a collection of posts.</span></span> <span data-ttu-id="990c3-106">Каждая из этих записей будет ссылкой на блог.</span><span class="sxs-lookup"><span data-stu-id="990c3-106">Each of those posts will have a reference back to the blog.</span></span>

<span data-ttu-id="990c3-107">Некоторые платформы сериализации не допускают такие циклы.</span><span class="sxs-lookup"><span data-stu-id="990c3-107">Some serialization frameworks don't allow such cycles.</span></span> <span data-ttu-id="990c3-108">Например Json.NET вызовет следующее исключение при обнаружении цикла.</span><span class="sxs-lookup"><span data-stu-id="990c3-108">For example, Json.NET will throw the following exception if a cycle is found.</span></span>

> <span data-ttu-id="990c3-109">Newtonsoft.Json.JsonSerializationException: обнаружен самоссылающийся цикл для свойства "Blog" с типом "MyApplication.Models.Blog".</span><span class="sxs-lookup"><span data-stu-id="990c3-109">Newtonsoft.Json.JsonSerializationException: Self referencing loop detected for property 'Blog' with type 'MyApplication.Models.Blog'.</span></span>

<span data-ttu-id="990c3-110">Если вы используете ASP.NET Core, вы можете настроить Json.NET для игнорирования циклов, которые он находит в графе объектов.</span><span class="sxs-lookup"><span data-stu-id="990c3-110">If you're using ASP.NET Core, you can configure Json.NET to ignore cycles that it finds in the object graph.</span></span> <span data-ttu-id="990c3-111">Настройка осуществляется с помощью метода `ConfigureServices(...)` в `Startup.cs`.</span><span class="sxs-lookup"><span data-stu-id="990c3-111">This configuration is done in the `ConfigureServices(...)` method in `Startup.cs`.</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    ...

    services.AddMvc()
        .AddJsonOptions(
            options => options.SerializerSettings.ReferenceLoopHandling = Newtonsoft.Json.ReferenceLoopHandling.Ignore
        );

    ...
}
```

<span data-ttu-id="990c3-112">Другой вариант — дополнение одного из свойств навигации атрибутом `[JsonIgnore]`, который указывает Json.NET игнорировать это свойство навигации во время сериализации.</span><span class="sxs-lookup"><span data-stu-id="990c3-112">Another alternative is to decorate one of the navigation properties with the `[JsonIgnore]` attribute, which instructs Json.NET to not traverse that navigation property while serializing.</span></span>
