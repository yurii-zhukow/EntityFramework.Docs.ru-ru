---
title: Миграция в средах групп — EF Core
description: Рекомендации по управлению миграцией и устранению конфликтов в средах групп с помощью Entity Framework Core
author: bricelam
ms.author: bricelam
ms.date: 10/30/2017
uid: core/managing-schemas/migrations/teams
ms.openlocfilehash: 1fbb7173a52218a4d00780ebc76e33600f3558c1
ms.sourcegitcommit: 7c3939504bb9da3f46bea3443638b808c04227c2
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/09/2020
ms.locfileid: "89619206"
---
# <a name="migrations-in-team-environments"></a><span data-ttu-id="7b082-103">Миграция в командных средах</span><span class="sxs-lookup"><span data-stu-id="7b082-103">Migrations in Team Environments</span></span>

<span data-ttu-id="7b082-104">При работе с миграциями в средах групп Обратите особое внимание на файл моментальных снимков модели.</span><span class="sxs-lookup"><span data-stu-id="7b082-104">When working with Migrations in team environments, pay extra attention to the model snapshot file.</span></span> <span data-ttu-id="7b082-105">Этот файл может сообщить вам о том, что миграция вашей коллеги объединяется с вами, или если необходимо устранить конфликт путем повторного создания миграции перед предоставлением общего доступа к ней.</span><span class="sxs-lookup"><span data-stu-id="7b082-105">This file can tell you if your teammate's migration merges cleanly with yours or if you need to resolve a conflict by re-creating your migration before sharing it.</span></span>

## <a name="merging"></a><span data-ttu-id="7b082-106">слияние</span><span class="sxs-lookup"><span data-stu-id="7b082-106">Merging</span></span>

<span data-ttu-id="7b082-107">При слиянии миграций из ваших коллег может возникнуть конфликт в файле моментального снимка модели.</span><span class="sxs-lookup"><span data-stu-id="7b082-107">When you merge migrations from your teammates, you may get conflicts in your model snapshot file.</span></span> <span data-ttu-id="7b082-108">Если оба изменения не связаны, объединение является тривиальным и две миграции могут сосуществовать.</span><span class="sxs-lookup"><span data-stu-id="7b082-108">If both changes are unrelated, the merge is trivial and the two migrations can coexist.</span></span> <span data-ttu-id="7b082-109">Например, вы можете получить конфликт слияния в конфигурации типа сущности Customer, которая выглядит следующим образом:</span><span class="sxs-lookup"><span data-stu-id="7b082-109">For example, you may get a merge conflict in the customer entity type configuration that looks like this:</span></span>

``` output
<<<<<<< Mine
b.Property<bool>("Deactivated");
=======
b.Property<int>("LoyaltyPoints");
>>>>>>> Theirs
```

<span data-ttu-id="7b082-110">Так как оба свойства должны существовать в окончательной модели, завершите слияние, добавив оба свойства.</span><span class="sxs-lookup"><span data-stu-id="7b082-110">Since both of these properties need to exist in the final model, complete the merge by adding both properties.</span></span> <span data-ttu-id="7b082-111">Во многих случаях система управления версиями может автоматически объединять такие изменения.</span><span class="sxs-lookup"><span data-stu-id="7b082-111">In many cases, your version control system may automatically merge such changes for you.</span></span>

``` csharp
b.Property<bool>("Deactivated");
b.Property<int>("LoyaltyPoints");
```

<span data-ttu-id="7b082-112">В таких случаях миграция и миграция коллег не зависят друг от друга.</span><span class="sxs-lookup"><span data-stu-id="7b082-112">In these cases, your migration and your teammate's migration are independent of each other.</span></span> <span data-ttu-id="7b082-113">Так как одна из них может быть применена первыми, вам не нужно вносить дополнительные изменения в процесс миграции, прежде чем поделиться им с вашей командой.</span><span class="sxs-lookup"><span data-stu-id="7b082-113">Since either of them could be applied first, you don't need to make any additional changes to your migration before sharing it with your team.</span></span>

## <a name="resolving-conflicts"></a><span data-ttu-id="7b082-114">Разрешение конфликтов</span><span class="sxs-lookup"><span data-stu-id="7b082-114">Resolving conflicts</span></span>

<span data-ttu-id="7b082-115">Иногда возникает истинный конфликт при объединении модели моментальных снимков модели.</span><span class="sxs-lookup"><span data-stu-id="7b082-115">Sometimes you encounter a true conflict when merging the model snapshot model.</span></span> <span data-ttu-id="7b082-116">Например, вы и ваши коллеги могут переименовать одно и то же свойство.</span><span class="sxs-lookup"><span data-stu-id="7b082-116">For example, you and your teammate may each have renamed the same property.</span></span>

``` output
<<<<<<< Mine
b.Property<string>("Username");
=======
b.Property<string>("Alias");
>>>>>>> Theirs
```

<span data-ttu-id="7b082-117">Если вы столкнулись с этим видом конфликта, устраните его, повторно создав миграцию.</span><span class="sxs-lookup"><span data-stu-id="7b082-117">If you encounter this kind of conflict, resolve it by re-creating your migration.</span></span> <span data-ttu-id="7b082-118">Выполните следующие действия.</span><span class="sxs-lookup"><span data-stu-id="7b082-118">Follow these steps:</span></span>

1. <span data-ttu-id="7b082-119">Прервать слияние и откат к рабочему каталогу перед слиянием</span><span class="sxs-lookup"><span data-stu-id="7b082-119">Abort the merge and rollback to your working directory before the merge</span></span>
2. <span data-ttu-id="7b082-120">Удалите миграцию (но не задерживайте изменения модели).</span><span class="sxs-lookup"><span data-stu-id="7b082-120">Remove your migration (but keep your model changes)</span></span>
3. <span data-ttu-id="7b082-121">Объединение изменений, внесенных коллегами, в рабочий каталог</span><span class="sxs-lookup"><span data-stu-id="7b082-121">Merge your teammate's changes into your working directory</span></span>
4. <span data-ttu-id="7b082-122">Повторное добавление миграции</span><span class="sxs-lookup"><span data-stu-id="7b082-122">Re-add your migration</span></span>

<span data-ttu-id="7b082-123">После этого две миграции можно применить в правильном порядке.</span><span class="sxs-lookup"><span data-stu-id="7b082-123">After doing this, the two migrations can be applied in the correct order.</span></span> <span data-ttu-id="7b082-124">Сначала применяется перенос, переименование столбца в *псевдоним*, после чего миграция переименовывается в *username*.</span><span class="sxs-lookup"><span data-stu-id="7b082-124">Their migration is applied first, renaming the column to *Alias*, thereafter your migration renames it to *Username*.</span></span>

<span data-ttu-id="7b082-125">Миграцию можно безопасно использовать совместно с остальными участниками группы.</span><span class="sxs-lookup"><span data-stu-id="7b082-125">Your migration can safely be shared with the rest of the team.</span></span>
