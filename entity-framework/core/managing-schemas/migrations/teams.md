---
title: Миграция в командных средах — EF Core
author: bricelam
ms.author: bricelam
ms.date: 10/30/2017
ms.technology: entity-framework-core
ms.openlocfilehash: cf76df32099c25f33d5d94edf6bccec099cf714a
ms.sourcegitcommit: de491b0988eab91b84dcbd941b7551e597e9c051
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/11/2018
ms.locfileid: "38228384"
---
<a name="migrations-in-team-environments"></a><span data-ttu-id="e438d-102">Миграция в командных средах</span><span class="sxs-lookup"><span data-stu-id="e438d-102">Migrations in Team Environments</span></span>
===============================
<span data-ttu-id="e438d-103">При работе с Миграциями в командных средах, особое внимание с файлом моментального снимка модели.</span><span class="sxs-lookup"><span data-stu-id="e438d-103">When working with Migrations in team environments, pay extra attention to the model snapshot file.</span></span> <span data-ttu-id="e438d-104">Этот файл может сообщить, если ваш коллега миграции четко объединяет с вашими или если вам требуется устранить конфликт, повторно создав миграции прежде чем использовать их.</span><span class="sxs-lookup"><span data-stu-id="e438d-104">This file can tell you if your teammate's migration merges cleanly with yours or if you need to resolve a conflict by re-creating your migration before sharing it.</span></span>

<a name="merging"></a><span data-ttu-id="e438d-105">слияние</span><span class="sxs-lookup"><span data-stu-id="e438d-105">Merging</span></span>
-------
<span data-ttu-id="e438d-106">При миграции слияние от коллег, в файле моментального снимка модели может появиться конфликтов.</span><span class="sxs-lookup"><span data-stu-id="e438d-106">When you merge migrations from your teammates, you may get conflicts in your model snapshot file.</span></span> <span data-ttu-id="e438d-107">Если оба изменения не связаны, слияние является тривиальным, и две операции миграции могут сосуществовать.</span><span class="sxs-lookup"><span data-stu-id="e438d-107">If both changes are unrelated, the merge is trivial and the two migrations can coexist.</span></span> <span data-ttu-id="e438d-108">Например можно получить конфликта слияния в конфигурации типа сущности customer, выглядит следующим образом:</span><span class="sxs-lookup"><span data-stu-id="e438d-108">For example, you may get a merge conflict in the customer entity type configuration that looks like this:</span></span>

    <<<<<<< Mine
    b.Property<bool>("Deactivated");
    =======
    b.Property<int>("LoyaltyPoints");
    >>>>>>> Theirs

<span data-ttu-id="e438d-109">Так как оба эти свойства должны существовать в конечной модели, Завершение слияния, добавив оба свойства.</span><span class="sxs-lookup"><span data-stu-id="e438d-109">Since both of these properties need to exist in the final model, complete the merge by adding both properties.</span></span> <span data-ttu-id="e438d-110">Во многих случаях системы управления версиями может автоматически объединить такие изменения автоматически.</span><span class="sxs-lookup"><span data-stu-id="e438d-110">In many cases, your version control system may automatically merge such changes for you.</span></span>

``` csharp
b.Property<bool>("Deactivated");
b.Property<int>("LoyaltyPoints");
```

<span data-ttu-id="e438d-111">В этих случаях миграции и миграции ваш коллега независимы друг от друга.</span><span class="sxs-lookup"><span data-stu-id="e438d-111">In these cases, your migration and your teammate's migration are independent of each other.</span></span> <span data-ttu-id="e438d-112">Так как сначала любого из них может применяться, не нужно вносить дополнительные изменения миграции перед предоставлением общего доступа со своей командой.</span><span class="sxs-lookup"><span data-stu-id="e438d-112">Since either of them could be applied first, you don't need to make any additional changes to your migration before sharing it with your team.</span></span>

<a name="resolving-conflicts"></a><span data-ttu-id="e438d-113">Разрешение конфликтов</span><span class="sxs-lookup"><span data-stu-id="e438d-113">Resolving conflicts</span></span>
-------------------
<span data-ttu-id="e438d-114">Иногда возникают true конфликтов при слиянии модель моментального снимка.</span><span class="sxs-lookup"><span data-stu-id="e438d-114">Sometimes you encounter a true conflict when merging the model snapshot model.</span></span> <span data-ttu-id="e438d-115">Например вы и ваш партнер по группе каждого было изменено и то же свойство.</span><span class="sxs-lookup"><span data-stu-id="e438d-115">For example, you and your teammate may each have renamed the same property.</span></span>

    <<<<<<< Mine
    b.Property<string>("Username");
    =======
    b.Property<string>("Alias");
    >>>>>>> Theirs

<span data-ttu-id="e438d-116">Если вы столкнулись с такого рода конфликт, устранить ее повторное создание миграции.</span><span class="sxs-lookup"><span data-stu-id="e438d-116">If you encounter this kind of conflict, resolve it by re-creating your migration.</span></span> <span data-ttu-id="e438d-117">Выполните следующие действия.</span><span class="sxs-lookup"><span data-stu-id="e438d-117">Follow these steps:</span></span>

1. <span data-ttu-id="e438d-118">Отменить слияние "и" rollback в рабочий каталог до слияния</span><span class="sxs-lookup"><span data-stu-id="e438d-118">Abort the merge and rollback to your working directory before the merge</span></span>
2. <span data-ttu-id="e438d-119">Удалить переход (но сохранить изменения модели)</span><span class="sxs-lookup"><span data-stu-id="e438d-119">Remove your migration (but keep your model changes)</span></span>
3. <span data-ttu-id="e438d-120">Слияние изменений ваш коллега в рабочий каталог</span><span class="sxs-lookup"><span data-stu-id="e438d-120">Merge your teammate's changes into your working directory</span></span>
4. <span data-ttu-id="e438d-121">Повторно добавить переход</span><span class="sxs-lookup"><span data-stu-id="e438d-121">Re-add your migration</span></span>

<span data-ttu-id="e438d-122">После этого, две операции миграции могут применяться в правильном порядке.</span><span class="sxs-lookup"><span data-stu-id="e438d-122">After doing this, the two migrations can be applied in the correct order.</span></span> <span data-ttu-id="e438d-123">Миграцию применяется в первую очередь, переименование столбца *псевдоним*, после миграции переименовывает его, чтобы *Username*.</span><span class="sxs-lookup"><span data-stu-id="e438d-123">Their migration is applied first, renaming the column to *Alias*, thereafter your migration renames it to *Username*.</span></span>

<span data-ttu-id="e438d-124">Миграцию можно безопасно использовать с остальными участниками команды.</span><span class="sxs-lookup"><span data-stu-id="e438d-124">Your migration can safely be shared with the rest of the team.</span></span>
