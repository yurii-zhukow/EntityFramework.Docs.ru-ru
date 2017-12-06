---
title: "Основные Save - EF Core"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 850d842e-3fad-4ef2-be17-053768e97b9e
ms.technology: entity-framework-core
uid: core/saving/basic
ms.openlocfilehash: e99d755b8f7c42b15a73cfdb6a2f8999a405a739
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/27/2017
---
# <a name="basic-save"></a><span data-ttu-id="8d40f-102">Основные сохранения</span><span class="sxs-lookup"><span data-stu-id="8d40f-102">Basic Save</span></span>

<span data-ttu-id="8d40f-103">Описание способов добавления, изменения и удаления данных с помощью классов контекста и сущности.</span><span class="sxs-lookup"><span data-stu-id="8d40f-103">Learn how to add, modify, and remove data using your context and entity classes.</span></span>

> [!TIP]  
> <span data-ttu-id="8d40f-104">Можно просмотреть в этой статье [пример](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/Basics/) на GitHub.</span><span class="sxs-lookup"><span data-stu-id="8d40f-104">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/Basics/) on GitHub.</span></span>

## <a name="adding-data"></a><span data-ttu-id="8d40f-105">Добавление данных</span><span class="sxs-lookup"><span data-stu-id="8d40f-105">Adding Data</span></span>

<span data-ttu-id="8d40f-106">Используйте *DbSet.Add* метод, чтобы добавить новые экземпляры классов сущностей.</span><span class="sxs-lookup"><span data-stu-id="8d40f-106">Use the *DbSet.Add* method to add new instances of your entity classes.</span></span> <span data-ttu-id="8d40f-107">В базе данных будут вставляться данные, при вызове *SaveChanges*.</span><span class="sxs-lookup"><span data-stu-id="8d40f-107">The data will be inserted in the database when you call *SaveChanges*.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Basics/Sample.cs#Add)]

## <a name="updating-data"></a><span data-ttu-id="8d40f-108">Обновление данных</span><span class="sxs-lookup"><span data-stu-id="8d40f-108">Updating Data</span></span>

<span data-ttu-id="8d40f-109">EF автоматически обнаруживает изменения, внесенные в существующую сущность, которая отслеживается контекстом.</span><span class="sxs-lookup"><span data-stu-id="8d40f-109">EF will automatically detect changes made to an existing entity that is tracked by the context.</span></span> <span data-ttu-id="8d40f-110">Сюда входят сущности, которые можно загрузить или запрос из базы данных и сущностями, которые были ранее добавлены и сохранены в базе данных.</span><span class="sxs-lookup"><span data-stu-id="8d40f-110">This includes entities that you load/query from the database, and entities that were previously added and saved to the database.</span></span>

<span data-ttu-id="8d40f-111">Просто измените значения свойств, а затем вызвать *SaveChanges*.</span><span class="sxs-lookup"><span data-stu-id="8d40f-111">Simply modify the values assigned to properties and then call *SaveChanges*.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Basics/Sample.cs#Update)]

## <a name="deleting-data"></a><span data-ttu-id="8d40f-112">Удаление данных</span><span class="sxs-lookup"><span data-stu-id="8d40f-112">Deleting Data</span></span>

<span data-ttu-id="8d40f-113">Используйте *DbSet.Remove* метод для удаления экземпляров вы классы сущностей.</span><span class="sxs-lookup"><span data-stu-id="8d40f-113">Use the *DbSet.Remove* method to delete instances of you entity classes.</span></span>

<span data-ttu-id="8d40f-114">Если сущность уже существует в базе данных, они будут удалены при *SaveChanges*.</span><span class="sxs-lookup"><span data-stu-id="8d40f-114">If the entity already exists in the database, it will be deleted during *SaveChanges*.</span></span> <span data-ttu-id="8d40f-115">Если сущность не были сохранены в базе данных (т. е. он отслеживаются по мере добавления), то он будет удален из контекста и больше не будет вставлен при *SaveChanges* вызывается.</span><span class="sxs-lookup"><span data-stu-id="8d40f-115">If the entity has not yet been saved to the database (i.e. it is tracked as added) then it will be removed from the context and will no longer be inserted when *SaveChanges* is called.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Basics/Sample.cs#Remove)]

## <a name="multiple-operations-in-a-single-savechanges"></a><span data-ttu-id="8d40f-116">Несколько операций в одном SaveChanges</span><span class="sxs-lookup"><span data-stu-id="8d40f-116">Multiple Operations in a single SaveChanges</span></span>

<span data-ttu-id="8d40f-117">Можно объединить несколько операций добавить, обновить или удалить, в рамках одного вызова *SaveChanges*.</span><span class="sxs-lookup"><span data-stu-id="8d40f-117">You can combine multiple Add/Update/Remove operations into a single call to *SaveChanges*.</span></span>

> [!NOTE]  
> <span data-ttu-id="8d40f-118">Для большинства поставщиков базы данных *SaveChanges* является транзакционной.</span><span class="sxs-lookup"><span data-stu-id="8d40f-118">For most database providers, *SaveChanges* is transactional.</span></span> <span data-ttu-id="8d40f-119">Это означает, что все операции либо успех или неудача и операции никогда не осталось применяются частично.</span><span class="sxs-lookup"><span data-stu-id="8d40f-119">This means  all the operations will either succeed or fail and the operations will never be left partially applied.</span></span>

[!code-csharp[Main](../../../samples/core/Saving/Saving/Basics/Sample.cs#MultipleOperations)]
