---
title: Включая &, исключая типы EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: cbe6935e-2679-4b77-8914-a8d772240cf1
uid: core/modeling/included-types
ms.openlocfilehash: 1249e71c02e58afe7fe06b3fdcf523dfa0c9b17c
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/06/2019
ms.locfileid: "73655739"
---
# <a name="including--excluding-types"></a><span data-ttu-id="d02be-102">Включение и исключение типов</span><span class="sxs-lookup"><span data-stu-id="d02be-102">Including & Excluding Types</span></span>

<span data-ttu-id="d02be-103">Включение типа в модель означает, что EF имеет метаданные об этом типе и будет пытаться считывать и записывать экземпляры из базы данных или из нее.</span><span class="sxs-lookup"><span data-stu-id="d02be-103">Including a type in the model means that EF has metadata about that type and will attempt to read and write instances from/to the database.</span></span>

## <a name="conventions"></a><span data-ttu-id="d02be-104">Соглашения</span><span class="sxs-lookup"><span data-stu-id="d02be-104">Conventions</span></span>

<span data-ttu-id="d02be-105">По соглашению типы, предоставляемые в свойствах `DbSet` в контексте, включаются в модель.</span><span class="sxs-lookup"><span data-stu-id="d02be-105">By convention, types that are exposed in `DbSet` properties on your context are included in your model.</span></span> <span data-ttu-id="d02be-106">Кроме того, также включены типы, упоминаемые в методе `OnModelCreating`.</span><span class="sxs-lookup"><span data-stu-id="d02be-106">In addition, types that are mentioned in the `OnModelCreating` method are also included.</span></span> <span data-ttu-id="d02be-107">Наконец, все типы, которые обнаруживаются рекурсивным просмотром свойств навигации обнаруженных типов, также включаются в модель.</span><span class="sxs-lookup"><span data-stu-id="d02be-107">Finally, any types that are found by recursively exploring the navigation properties of discovered types are also included in the model.</span></span>

<span data-ttu-id="d02be-108">**Например, в следующем коде обнаруживаются все три типа:**</span><span class="sxs-lookup"><span data-stu-id="d02be-108">**For example, in the following code listing all three types are discovered:**</span></span>

* <span data-ttu-id="d02be-109">`Blog`, так как он предоставляется в свойстве `DbSet` в контексте</span><span class="sxs-lookup"><span data-stu-id="d02be-109">`Blog` because it is exposed in a `DbSet` property on the context</span></span>

* <span data-ttu-id="d02be-110">`Post`, так как он обнаруживается с помощью свойства навигации `Blog.Posts`</span><span class="sxs-lookup"><span data-stu-id="d02be-110">`Post` because it is discovered via the `Blog.Posts` navigation property</span></span>

* <span data-ttu-id="d02be-111">`AuditEntry`, так как он упоминается в `OnModelCreating`</span><span class="sxs-lookup"><span data-stu-id="d02be-111">`AuditEntry` because it is mentioned in `OnModelCreating`</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/IncludedTypes.cs?name=IncludedTypes&highlight=3,7,16)]

## <a name="data-annotations"></a><span data-ttu-id="d02be-112">Заметки к данным</span><span class="sxs-lookup"><span data-stu-id="d02be-112">Data Annotations</span></span>

<span data-ttu-id="d02be-113">Заметки к данным можно использовать для исключения типа из модели.</span><span class="sxs-lookup"><span data-stu-id="d02be-113">You can use Data Annotations to exclude a type from the model.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/IgnoreType.cs?highlight=20)]

## <a name="fluent-api"></a><span data-ttu-id="d02be-114">Текучий API</span><span class="sxs-lookup"><span data-stu-id="d02be-114">Fluent API</span></span>

<span data-ttu-id="d02be-115">API-интерфейс Fluent можно использовать для исключения типа из модели.</span><span class="sxs-lookup"><span data-stu-id="d02be-115">You can use the Fluent API to exclude a type from the model.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/IgnoreType.cs?highlight=12)]
