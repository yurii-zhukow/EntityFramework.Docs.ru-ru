---
title: Базовое сохранение — EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 850d842e-3fad-4ef2-be17-053768e97b9e
uid: core/saving/basic
ms.openlocfilehash: 066d67d6104316832a33f5a3648f1f2fa6cc9c50
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/06/2020
ms.locfileid: "78413687"
---
# <a name="basic-save"></a><span data-ttu-id="6f7f8-102">Базовое сохранение</span><span class="sxs-lookup"><span data-stu-id="6f7f8-102">Basic Save</span></span>

<span data-ttu-id="6f7f8-103">Узнайте, как добавлять, изменять и удалять данные, используя контекст и классы сущностей.</span><span class="sxs-lookup"><span data-stu-id="6f7f8-103">Learn how to add, modify, and remove data using your context and entity classes.</span></span>

> [!TIP]  
> <span data-ttu-id="6f7f8-104">Для этой статьи вы можете скачать [пример](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Saving/Basics/) из репозитория GitHub.</span><span class="sxs-lookup"><span data-stu-id="6f7f8-104">You can view this article's [sample](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Saving/Basics/) on GitHub.</span></span>

## <a name="adding-data"></a><span data-ttu-id="6f7f8-105">Добавление данных</span><span class="sxs-lookup"><span data-stu-id="6f7f8-105">Adding Data</span></span>

<span data-ttu-id="6f7f8-106">Добавлять новые экземпляры классов сущностей можно с помощью метода *DbSet.Add*.</span><span class="sxs-lookup"><span data-stu-id="6f7f8-106">Use the *DbSet.Add* method to add new instances of your entity classes.</span></span> <span data-ttu-id="6f7f8-107">Данные будут вставлены в базу данных при вызове метода *SaveChanges*.</span><span class="sxs-lookup"><span data-stu-id="6f7f8-107">The data will be inserted in the database when you call *SaveChanges*.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Basics/Sample.cs#Add)]

> [!TIP]  
> <span data-ttu-id="6f7f8-108">Методы Add, Attach и Update работают с полным графом переданных им сущностей, как описано в статье [Сохранение связанных данных](related-data.md).</span><span class="sxs-lookup"><span data-stu-id="6f7f8-108">The Add, Attach, and Update methods all work on the full graph of entities passed to them, as described in the [Related Data](related-data.md) section.</span></span> <span data-ttu-id="6f7f8-109">В качестве альтернативы свойство EntityEntry.State можно использовать для установки состояния только одной сущности.</span><span class="sxs-lookup"><span data-stu-id="6f7f8-109">Alternately, the EntityEntry.State property can be used to set the state of just a single entity.</span></span> <span data-ttu-id="6f7f8-110">Например, `context.Entry(blog).State = EntityState.Modified`.</span><span class="sxs-lookup"><span data-stu-id="6f7f8-110">For example, `context.Entry(blog).State = EntityState.Modified`.</span></span>

## <a name="updating-data"></a><span data-ttu-id="6f7f8-111">Обновление данных</span><span class="sxs-lookup"><span data-stu-id="6f7f8-111">Updating Data</span></span>

<span data-ttu-id="6f7f8-112">EF автоматически обнаружит изменения, внесенные в существующую сущность, которая отслеживается контекстом.</span><span class="sxs-lookup"><span data-stu-id="6f7f8-112">EF will automatically detect changes made to an existing entity that is tracked by the context.</span></span> <span data-ttu-id="6f7f8-113">Сюда входят сущности, которые вы загружаете или запрашиваете из базы данных, и сущности, которые ранее были добавлены и сохранены в базе данных.</span><span class="sxs-lookup"><span data-stu-id="6f7f8-113">This includes entities that you load/query from the database, and entities that were previously added and saved to the database.</span></span>

<span data-ttu-id="6f7f8-114">Просто измените значения, присвоенные свойствам, а затем вызовите метод *SaveChanges*.</span><span class="sxs-lookup"><span data-stu-id="6f7f8-114">Simply modify the values assigned to properties and then call *SaveChanges*.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Basics/Sample.cs#Update)]

## <a name="deleting-data"></a><span data-ttu-id="6f7f8-115">Удаление данных</span><span class="sxs-lookup"><span data-stu-id="6f7f8-115">Deleting Data</span></span>

<span data-ttu-id="6f7f8-116">С помощью метода *DbSet.Remove* можно удалять экземпляры классов сущностей.</span><span class="sxs-lookup"><span data-stu-id="6f7f8-116">Use the *DbSet.Remove* method to delete instances of your entity classes.</span></span>

<span data-ttu-id="6f7f8-117">Если сущность уже существует в базе данных, она будет удалена во время выполнения метода *SaveChanges*.</span><span class="sxs-lookup"><span data-stu-id="6f7f8-117">If the entity already exists in the database, it will be deleted during *SaveChanges*.</span></span> <span data-ttu-id="6f7f8-118">Если сущность еще не сохранена в базе данных (т. е. она отслеживается как добавленная), она будет удалена из контекста и больше не будет вставляться при вызове метода *SaveChanges*.</span><span class="sxs-lookup"><span data-stu-id="6f7f8-118">If the entity has not yet been saved to the database (that is, it is tracked as added) then it will be removed from the context and will no longer be inserted when *SaveChanges* is called.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Basics/Sample.cs#Remove)]

## <a name="multiple-operations-in-a-single-savechanges"></a><span data-ttu-id="6f7f8-119">Несколько операций в одном методе SaveChanges</span><span class="sxs-lookup"><span data-stu-id="6f7f8-119">Multiple Operations in a single SaveChanges</span></span>

<span data-ttu-id="6f7f8-120">Вы можете объединить несколько операций Add, Update и Remove в один вызов метода *SaveChanges*.</span><span class="sxs-lookup"><span data-stu-id="6f7f8-120">You can combine multiple Add/Update/Remove operations into a single call to *SaveChanges*.</span></span>

> [!NOTE]  
> <span data-ttu-id="6f7f8-121">Для большинства поставщиков баз данных метод *SaveChanges* является транзакционным.</span><span class="sxs-lookup"><span data-stu-id="6f7f8-121">For most database providers, *SaveChanges* is transactional.</span></span> <span data-ttu-id="6f7f8-122">Это означает, что все операции будут либо успешными, либо неудачными и никогда не будут частично применены.</span><span class="sxs-lookup"><span data-stu-id="6f7f8-122">This means  all the operations will either succeed or fail and the operations will never be left partially applied.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Basics/Sample.cs#MultipleOperations)]
