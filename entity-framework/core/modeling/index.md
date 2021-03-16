---
title: Создание и настройка модели — EF Core
description: Общие сведения о создании и настройке модели с помощью Entity Framework Core
author: AndriySvyryd
ms.date: 10/13/2020
uid: core/modeling/index
ms.openlocfilehash: f8ab985b8f17617ca30e7a528ebaac773e789f27
ms.sourcegitcommit: 4798ab8d04c1fdbe6dd204d94d770fcbf309d09b
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/11/2021
ms.locfileid: "103023982"
---
# <a name="creating-and-configuring-a-model"></a><span data-ttu-id="a8ae0-103">Создание и настройка модели</span><span class="sxs-lookup"><span data-stu-id="a8ae0-103">Creating and configuring a model</span></span>

<span data-ttu-id="a8ae0-104">Entity Framework Core использует набор соглашений для создания модели на основе формы классов сущностей.</span><span class="sxs-lookup"><span data-stu-id="a8ae0-104">Entity Framework Core uses a set of conventions to build a model based on the shape of your entity classes.</span></span> <span data-ttu-id="a8ae0-105">Вы можете указать дополнительную конфигурацию, чтобы дополнить и (или) переопределить данные, обнаруженные соглашением.</span><span class="sxs-lookup"><span data-stu-id="a8ae0-105">You can specify additional configuration to supplement and/or override what was discovered by convention.</span></span>

<span data-ttu-id="a8ae0-106">Эта статья описывает конфигурацию, которую можно применить к модели, ориентированной на любое хранилище данных, а также при ориентации на любую реляционную базу данных.</span><span class="sxs-lookup"><span data-stu-id="a8ae0-106">This article covers configuration that can be applied to a model targeting any data store and that which can be applied when targeting any relational database.</span></span> <span data-ttu-id="a8ae0-107">Поставщики также могут включить конфигурацию, предназначенную для конкретного хранилища данных.</span><span class="sxs-lookup"><span data-stu-id="a8ae0-107">Providers may also enable configuration that is specific to a particular data store.</span></span> <span data-ttu-id="a8ae0-108">Документацию по конфигурации для конкретного поставщика см. в разделе [Поставщики баз данных](xref:core/providers/index).</span><span class="sxs-lookup"><span data-stu-id="a8ae0-108">For documentation on provider specific configuration see the [Database Providers](xref:core/providers/index) section.</span></span>

> [!TIP]
> <span data-ttu-id="a8ae0-109">Для этой статьи вы можете просмотреть [пример](https://github.com/dotnet/EntityFramework.Docs/tree/main/samples) в репозитории GitHub.</span><span class="sxs-lookup"><span data-stu-id="a8ae0-109">You can view this article’s [sample](https://github.com/dotnet/EntityFramework.Docs/tree/main/samples) on GitHub.</span></span>

## <a name="use-fluent-api-to-configure-a-model"></a><span data-ttu-id="a8ae0-110">Использование текучего API для настройки модели</span><span class="sxs-lookup"><span data-stu-id="a8ae0-110">Use fluent API to configure a model</span></span>

<span data-ttu-id="a8ae0-111">Вы можете переопределить метод `OnModelCreating` в производном контексте и использовать `ModelBuilder API` для настройки модели.</span><span class="sxs-lookup"><span data-stu-id="a8ae0-111">You can override the `OnModelCreating` method in your derived context and use the `ModelBuilder API` to configure your model.</span></span> <span data-ttu-id="a8ae0-112">Это наиболее эффективный метод настройки, который позволяет задать конфигурацию без изменения ваших классов сущностей.</span><span class="sxs-lookup"><span data-stu-id="a8ae0-112">This is the most powerful method of configuration and allows configuration to be specified without modifying your entity classes.</span></span> <span data-ttu-id="a8ae0-113">Конфигурация текучего API имеет самый высокий приоритет и переопределяет соглашения и заметки к данным.</span><span class="sxs-lookup"><span data-stu-id="a8ae0-113">Fluent API configuration has the highest precedence and will override conventions and data annotations.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Required.cs?highlight=12-14)]

### <a name="grouping-configuration"></a><span data-ttu-id="a8ae0-114">Группирование конфигурации</span><span class="sxs-lookup"><span data-stu-id="a8ae0-114">Grouping configuration</span></span>

<span data-ttu-id="a8ae0-115">Чтобы уменьшить размер метода <xref:Microsoft.EntityFrameworkCore.DbContext.OnModelCreating%2A>, вы можете извлечь все конфигурации для типа сущности в отдельный класс, реализующий <xref:Microsoft.EntityFrameworkCore.IEntityTypeConfiguration%601>.</span><span class="sxs-lookup"><span data-stu-id="a8ae0-115">To reduce the size of the <xref:Microsoft.EntityFrameworkCore.DbContext.OnModelCreating%2A> method all configuration for an entity type can be extracted to a separate class implementing <xref:Microsoft.EntityFrameworkCore.IEntityTypeConfiguration%601>.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/EntityTypeConfiguration.cs?Name=IEntityTypeConfiguration)]

<span data-ttu-id="a8ae0-116">Затем просто вызовите метод `Configure` из `OnModelCreating`.</span><span class="sxs-lookup"><span data-stu-id="a8ae0-116">Then just invoke the `Configure` method from `OnModelCreating`.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/EntityTypeConfiguration.cs?Name=ApplyIEntityTypeConfiguration)]

<span data-ttu-id="a8ae0-117">Вы можете применить все конфигурации, заданные в типах, реализующих `IEntityTypeConfiguration` в определенной сборке.</span><span class="sxs-lookup"><span data-stu-id="a8ae0-117">It is possible to apply all configuration specified in types implementing `IEntityTypeConfiguration` in a given assembly.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/EntityTypeConfiguration.cs?Name=ApplyConfigurationsFromAssembly)]

> [!NOTE]
> <span data-ttu-id="a8ae0-118">Порядок, в котором будут применяться конфигурации, не определен. Поэтому этот метод следует использовать только в том случае, если порядок не имеет значения.</span><span class="sxs-lookup"><span data-stu-id="a8ae0-118">The order in which the configurations will be applied is undefined, therefore this method should only be used when the order doesn't matter.</span></span>

## <a name="use-data-annotations-to-configure-a-model"></a><span data-ttu-id="a8ae0-119">Использование заметок к данным для настройки модели</span><span class="sxs-lookup"><span data-stu-id="a8ae0-119">Use data annotations to configure a model</span></span>

<span data-ttu-id="a8ae0-120">Вы также можете применять для классов и свойств атрибуты (называемые заметками к данным).</span><span class="sxs-lookup"><span data-stu-id="a8ae0-120">You can also apply attributes (known as Data Annotations) to your classes and properties.</span></span> <span data-ttu-id="a8ae0-121">Заметки к данным переопределяют соглашения, но они будут перезаписаны конфигурацией текучего API.</span><span class="sxs-lookup"><span data-stu-id="a8ae0-121">Data annotations will override conventions, but will be overridden by Fluent API configuration.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Required.cs?highlight=15)]
