---
title: Миграция в средах групп - EF Core
author: bricelam
ms.author: bricelam
ms.date: 10/30/2017
ms.technology: entity-framework-core
ms.openlocfilehash: 40cbc1c1bb0273bf733fadb884bffadcceeb162b
ms.sourcegitcommit: b467368cc350e6059fdc0949e042a41cb11e61d9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/15/2017
ms.locfileid: "26053804"
---
<a name="migrations-in-team-environments"></a><span data-ttu-id="c135a-102">Миграция в средах групп</span><span class="sxs-lookup"><span data-stu-id="c135a-102">Migrations in Team Environments</span></span>
===============================
<span data-ttu-id="c135a-103">При работе с миграции в средах рабочих групп, необходимо проявлять особое внимание в файле моментального снимка модели.</span><span class="sxs-lookup"><span data-stu-id="c135a-103">When working with Migrations in team environments, pay extra attention to the model snapshot file.</span></span> <span data-ttu-id="c135a-104">Этот файл может сообщить, при миграции вашей помощником объединяет полностью с вашими из Если необходимо разрешить конфликт, повторное создание миграции до предоставления доступа к ней.</span><span class="sxs-lookup"><span data-stu-id="c135a-104">This file can tell you if your teammate's migration merges cleanly with yours of if you need to resolve a conflict by re-creating your migration before sharing it.</span></span>

<a name="merging"></a><span data-ttu-id="c135a-105">слияние</span><span class="sxs-lookup"><span data-stu-id="c135a-105">Merging</span></span>
-------
<span data-ttu-id="c135a-106">При миграции объединить с другими членами команды, можно получить в файле моментального снимка модели конфликтов.</span><span class="sxs-lookup"><span data-stu-id="c135a-106">When you merge migrations from your teammates, you may get conflicts in your model snapshot file.</span></span> <span data-ttu-id="c135a-107">Если оба изменения не связаны, слияния является тривиальным, и два миграции могут сосуществовать.</span><span class="sxs-lookup"><span data-stu-id="c135a-107">If both changes are unrelated, the merge is trivial and the two migrations can coexist.</span></span> <span data-ttu-id="c135a-108">Например могут получить конфликта слияния в конфигурации типа сущности customer, выглядит следующим образом:</span><span class="sxs-lookup"><span data-stu-id="c135a-108">For example, you may get a merge conflict in the customer entity type configuration that looks like this:</span></span>

    <<<<<<< Mine
    b.Property<bool>("Deactivated");
    =======
    b.Property<int>("LoyaltyPoints");
    >>>>>>> Theirs

<span data-ttu-id="c135a-109">Так как оба эти свойства должны существовать в конечной модели, Завершение слияния, добавив оба свойства.</span><span class="sxs-lookup"><span data-stu-id="c135a-109">Since both of these properties need to exist in the final model, complete the merge by adding both properties.</span></span> <span data-ttu-id="c135a-110">Во многих случаях системы управления версиями может автоматически объединить такие изменения автоматически.</span><span class="sxs-lookup"><span data-stu-id="c135a-110">In many cases, your version control system may automatically merge such changes for you.</span></span>

``` csharp
b.Property<bool>("Deactivated");
b.Property<int>("LoyaltyPoints");
```

<span data-ttu-id="c135a-111">В этих случаях миграции и миграции вашей помощником независимо друг от друга.</span><span class="sxs-lookup"><span data-stu-id="c135a-111">In these cases, your migration and your teammate's migration are independent of each other.</span></span> <span data-ttu-id="c135a-112">Поскольку любое из них может быть применена во-первых, не нужно вносить дополнительные изменения в миграции перед предоставлением совместного доступа с другими участниками команды.</span><span class="sxs-lookup"><span data-stu-id="c135a-112">Since either of them could be applied first, you don't need to make any additional changes to your migration before sharing it with your team.</span></span>

<a name="resolving-conflicts"></a><span data-ttu-id="c135a-113">Разрешение конфликтов</span><span class="sxs-lookup"><span data-stu-id="c135a-113">Resolving conflicts</span></span>
-------------------
<span data-ttu-id="c135a-114">Иногда обнаруживается конфликт true при слиянии снимка модели.</span><span class="sxs-lookup"><span data-stu-id="c135a-114">Sometimes you encounter a true conflict when merging the model snapshot model.</span></span> <span data-ttu-id="c135a-115">Например вы и ваш партнер по группе каждого было изменено и то же свойство.</span><span class="sxs-lookup"><span data-stu-id="c135a-115">For example, you and your teammate may each have renamed the same property.</span></span>

    <<<<<<< Mine
    b.Property<string>("Username");
    =======
    b.Property<string>("Alias");
    >>>>>>> Theirs

<span data-ttu-id="c135a-116">При возникновении такого рода конфликт необходимо разрешить ее, повторно создав переноса.</span><span class="sxs-lookup"><span data-stu-id="c135a-116">If you encounter this kind of conflict, resolve it by re-creating your migration.</span></span> <span data-ttu-id="c135a-117">Выполните следующие действия.</span><span class="sxs-lookup"><span data-stu-id="c135a-117">Follow these steps:</span></span>

1. <span data-ttu-id="c135a-118">Прервать слияния и отката в рабочий каталог перед слиянием</span><span class="sxs-lookup"><span data-stu-id="c135a-118">Abort the merge and rollback to your working directory before the merge</span></span>
2. <span data-ttu-id="c135a-119">Удаление перехода (сохранением изменений модели)</span><span class="sxs-lookup"><span data-stu-id="c135a-119">Remove your migration (but keep your model changes)</span></span>
3. <span data-ttu-id="c135a-120">Слияние изменений ваш находится в рабочий каталог</span><span class="sxs-lookup"><span data-stu-id="c135a-120">Merge your teammate's changes into your working directory</span></span>
4. <span data-ttu-id="c135a-121">Снова добавьте миграции</span><span class="sxs-lookup"><span data-stu-id="c135a-121">Re-add your migration</span></span>

<span data-ttu-id="c135a-122">После этого можно применять два миграции в правильном порядке.</span><span class="sxs-lookup"><span data-stu-id="c135a-122">After doing this, the two migrations can be applied in the correct order.</span></span> <span data-ttu-id="c135a-123">Их миграция применяется в первую очередь, переименование столбца *псевдоним*, после переноса переименовывает его *Username*.</span><span class="sxs-lookup"><span data-stu-id="c135a-123">Their migration is applied first, renaming the column to *Alias*, thereafter your migration renames it to *Username*.</span></span>

<span data-ttu-id="c135a-124">Миграцию можно безопасно использовать с остальными участниками команды.</span><span class="sxs-lookup"><span data-stu-id="c135a-124">Your migration can safely be shared with the rest of the team.</span></span>
