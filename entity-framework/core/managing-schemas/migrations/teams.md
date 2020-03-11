---
title: Миграция в средах групп — EF Core
author: bricelam
ms.author: bricelam
ms.date: 10/30/2017
uid: core/managing-schemas/migrations/teams
ms.openlocfilehash: 6c17c56277821159962884aef72d46c624442e20
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/06/2020
ms.locfileid: "78414245"
---
# <a name="migrations-in-team-environments"></a><span data-ttu-id="97409-102">Миграция в командных средах</span><span class="sxs-lookup"><span data-stu-id="97409-102">Migrations in Team Environments</span></span>

<span data-ttu-id="97409-103">При работе с миграциями в средах групп Обратите особое внимание на файл моментальных снимков модели.</span><span class="sxs-lookup"><span data-stu-id="97409-103">When working with Migrations in team environments, pay extra attention to the model snapshot file.</span></span> <span data-ttu-id="97409-104">Этот файл может сообщить вам о том, что миграция вашей коллеги объединяется с вами, или если необходимо устранить конфликт путем повторного создания миграции перед предоставлением общего доступа к ней.</span><span class="sxs-lookup"><span data-stu-id="97409-104">This file can tell you if your teammate's migration merges cleanly with yours or if you need to resolve a conflict by re-creating your migration before sharing it.</span></span>

## <a name="merging"></a><span data-ttu-id="97409-105">слияние</span><span class="sxs-lookup"><span data-stu-id="97409-105">Merging</span></span>

<span data-ttu-id="97409-106">При слиянии миграций из ваших коллег может возникнуть конфликт в файле моментального снимка модели.</span><span class="sxs-lookup"><span data-stu-id="97409-106">When you merge migrations from your teammates, you may get conflicts in your model snapshot file.</span></span> <span data-ttu-id="97409-107">Если оба изменения не связаны, объединение является тривиальным и две миграции могут сосуществовать.</span><span class="sxs-lookup"><span data-stu-id="97409-107">If both changes are unrelated, the merge is trivial and the two migrations can coexist.</span></span> <span data-ttu-id="97409-108">Например, вы можете получить конфликт слияния в конфигурации типа сущности Customer, которая выглядит следующим образом:</span><span class="sxs-lookup"><span data-stu-id="97409-108">For example, you may get a merge conflict in the customer entity type configuration that looks like this:</span></span>

``` output
<<<<<<< Mine
b.Property<bool>("Deactivated");
=======
b.Property<int>("LoyaltyPoints");
>>>>>>> Theirs
```

<span data-ttu-id="97409-109">Так как оба свойства должны существовать в окончательной модели, завершите слияние, добавив оба свойства.</span><span class="sxs-lookup"><span data-stu-id="97409-109">Since both of these properties need to exist in the final model, complete the merge by adding both properties.</span></span> <span data-ttu-id="97409-110">Во многих случаях система управления версиями может автоматически объединять такие изменения.</span><span class="sxs-lookup"><span data-stu-id="97409-110">In many cases, your version control system may automatically merge such changes for you.</span></span>

``` csharp
b.Property<bool>("Deactivated");
b.Property<int>("LoyaltyPoints");
```

<span data-ttu-id="97409-111">В таких случаях миграция и миграция коллег не зависят друг от друга.</span><span class="sxs-lookup"><span data-stu-id="97409-111">In these cases, your migration and your teammate's migration are independent of each other.</span></span> <span data-ttu-id="97409-112">Так как одна из них может быть применена первыми, вам не нужно вносить дополнительные изменения в процесс миграции, прежде чем поделиться им с вашей командой.</span><span class="sxs-lookup"><span data-stu-id="97409-112">Since either of them could be applied first, you don't need to make any additional changes to your migration before sharing it with your team.</span></span>

## <a name="resolving-conflicts"></a><span data-ttu-id="97409-113">Разрешение конфликтов</span><span class="sxs-lookup"><span data-stu-id="97409-113">Resolving conflicts</span></span>

<span data-ttu-id="97409-114">Иногда возникает истинный конфликт при объединении модели моментальных снимков модели.</span><span class="sxs-lookup"><span data-stu-id="97409-114">Sometimes you encounter a true conflict when merging the model snapshot model.</span></span> <span data-ttu-id="97409-115">Например, вы и ваши коллеги могут переименовать одно и то же свойство.</span><span class="sxs-lookup"><span data-stu-id="97409-115">For example, you and your teammate may each have renamed the same property.</span></span>

``` output
<<<<<<< Mine
b.Property<string>("Username");
=======
b.Property<string>("Alias");
>>>>>>> Theirs
```

<span data-ttu-id="97409-116">Если вы столкнулись с этим видом конфликта, устраните его, повторно создав миграцию.</span><span class="sxs-lookup"><span data-stu-id="97409-116">If you encounter this kind of conflict, resolve it by re-creating your migration.</span></span> <span data-ttu-id="97409-117">Выполните следующие действия.</span><span class="sxs-lookup"><span data-stu-id="97409-117">Follow these steps:</span></span>

1. <span data-ttu-id="97409-118">Прервать слияние и откат к рабочему каталогу перед слиянием</span><span class="sxs-lookup"><span data-stu-id="97409-118">Abort the merge and rollback to your working directory before the merge</span></span>
2. <span data-ttu-id="97409-119">Удалите миграцию (но не задерживайте изменения модели).</span><span class="sxs-lookup"><span data-stu-id="97409-119">Remove your migration (but keep your model changes)</span></span>
3. <span data-ttu-id="97409-120">Объединение изменений, внесенных коллегами, в рабочий каталог</span><span class="sxs-lookup"><span data-stu-id="97409-120">Merge your teammate's changes into your working directory</span></span>
4. <span data-ttu-id="97409-121">Повторное добавление миграции</span><span class="sxs-lookup"><span data-stu-id="97409-121">Re-add your migration</span></span>

<span data-ttu-id="97409-122">После этого две миграции можно применить в правильном порядке.</span><span class="sxs-lookup"><span data-stu-id="97409-122">After doing this, the two migrations can be applied in the correct order.</span></span> <span data-ttu-id="97409-123">Сначала применяется перенос, переименование столбца в *псевдоним*, после чего миграция переименовывается в *username*.</span><span class="sxs-lookup"><span data-stu-id="97409-123">Their migration is applied first, renaming the column to *Alias*, thereafter your migration renames it to *Username*.</span></span>

<span data-ttu-id="97409-124">Миграцию можно безопасно использовать совместно с остальными участниками группы.</span><span class="sxs-lookup"><span data-stu-id="97409-124">Your migration can safely be shared with the rest of the team.</span></span>
