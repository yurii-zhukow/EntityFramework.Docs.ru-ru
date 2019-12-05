---
title: Создание и настройка модели — EF Core
author: rowanmiller
ms.date: 11/05/2019
ms.assetid: 88253ff3-174e-485c-b3f8-768243d01ee1
uid: core/modeling/index
ms.openlocfilehash: 58be4a45473c6292790da341e360b3340de27be7
ms.sourcegitcommit: 7a709ce4f77134782393aa802df5ab2718714479
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/04/2019
ms.locfileid: "74824695"
---
# <a name="creating-and-configuring-a-model"></a><span data-ttu-id="86e45-102">Создание и настройка модели</span><span class="sxs-lookup"><span data-stu-id="86e45-102">Creating and configuring a model</span></span>

<span data-ttu-id="86e45-103">Entity Framework использует набор соглашений для создания модели на основе формы классов сущностей.</span><span class="sxs-lookup"><span data-stu-id="86e45-103">Entity Framework uses a set of conventions to build a model based on the shape of your entity classes.</span></span> <span data-ttu-id="86e45-104">Вы можете указать дополнительную конфигурацию, чтобы дополнить и (или) переопределить данные, обнаруженные соглашением.</span><span class="sxs-lookup"><span data-stu-id="86e45-104">You can specify additional configuration to supplement and/or override what was discovered by convention.</span></span>

<span data-ttu-id="86e45-105">Эта статья описывает конфигурацию, которую можно применить к модели, ориентированной на любое хранилище данных, а также при ориентации на любую реляционную базу данных.</span><span class="sxs-lookup"><span data-stu-id="86e45-105">This article covers configuration that can be applied to a model targeting any data store and that which can be applied when targeting any relational database.</span></span> <span data-ttu-id="86e45-106">Поставщики также могут включить конфигурацию, предназначенную для конкретного хранилища данных.</span><span class="sxs-lookup"><span data-stu-id="86e45-106">Providers may also enable configuration that is specific to a particular data store.</span></span> <span data-ttu-id="86e45-107">Документацию по конфигурации для конкретных поставщиков см. в разделе  [Поставщики баз данных](../providers/index.md) .</span><span class="sxs-lookup"><span data-stu-id="86e45-107">For documentation on provider specific configuration see the [Database Providers](../providers/index.md) section.</span></span>

> [!TIP]  
> <span data-ttu-id="86e45-108"> [Пример](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples) для этой статьи можно скачать в GitHub.</span><span class="sxs-lookup"><span data-stu-id="86e45-108">You can view this article’s [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples) on GitHub.</span></span>

## <a name="use-fluent-api-to-configure-a-model"></a><span data-ttu-id="86e45-109">Использование текучего API для настройки модели</span><span class="sxs-lookup"><span data-stu-id="86e45-109">Use fluent API to configure a model</span></span>

<span data-ttu-id="86e45-110">Вы можете переопределить `OnModelCreating` метод в производном контексте и использовать `ModelBuilder API` для настройки модели.</span><span class="sxs-lookup"><span data-stu-id="86e45-110">You can override the `OnModelCreating` method in your derived context and use the `ModelBuilder API` to configure your model.</span></span> <span data-ttu-id="86e45-111">Это наиболее эффективный метод настройки, который позволяет задать конфигурацию без изменения ваших классов сущностей.</span><span class="sxs-lookup"><span data-stu-id="86e45-111">This is the most powerful method of configuration and allows configuration to be specified without modifying your entity classes.</span></span> <span data-ttu-id="86e45-112">Конфигурация текучего API имеет самый высокий приоритет и переопределяет соглашения и заметки к данным.</span><span class="sxs-lookup"><span data-stu-id="86e45-112">Fluent API configuration has the highest precedence and will override conventions and data annotations.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Required.cs?highlight=11-13)]

## <a name="use-data-annotations-to-configure-a-model"></a><span data-ttu-id="86e45-113">Использование заметок к данным для настройки модели</span><span class="sxs-lookup"><span data-stu-id="86e45-113">Use data annotations to configure a model</span></span>

<span data-ttu-id="86e45-114">Вы также можете применять для классов и свойств атрибуты (называемые заметками к данным).</span><span class="sxs-lookup"><span data-stu-id="86e45-114">You can also apply attributes (known as Data Annotations) to your classes and properties.</span></span> <span data-ttu-id="86e45-115">Заметки к данным переопределяют соглашения, но они будут перезаписаны конфигурацией текучего API.</span><span class="sxs-lookup"><span data-stu-id="86e45-115">Data annotations will override conventions, but will be overridden by Fluent API configuration.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Required.cs?highlight=14)]
