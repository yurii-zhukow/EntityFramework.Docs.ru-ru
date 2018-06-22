---
title: Тестирование компонентов с использованием Entity Framework — EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 1603be0c-69bc-4dd9-9a08-3d0129cdc6c1
ms.technology: entity-framework-core
uid: core/miscellaneous/testing/index
ms.openlocfilehash: c82c25da393c39cf5e2deb46c7322e7395051937
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/27/2017
ms.locfileid: "26048866"
---
# <a name="testing"></a><span data-ttu-id="85872-102">Тестирование</span><span class="sxs-lookup"><span data-stu-id="85872-102">Testing</span></span>

<span data-ttu-id="85872-103">Вам может потребоваться протестировать компоненты, используя средство, имитирующее подключение к реальной базе данных, без дополнительных затрат, создаваемых операциями ввода-вывода базы данных.</span><span class="sxs-lookup"><span data-stu-id="85872-103">You may want to test components using something that approximates connecting to the real database, without the overhead of actual database I/O operations.</span></span>

<span data-ttu-id="85872-104">Для этого существует два основных способа.</span><span class="sxs-lookup"><span data-stu-id="85872-104">There are two main options for doing this:</span></span>
 * <span data-ttu-id="85872-105">[Режим выполнения в памяти SQLite](sqlite.md) позволяет создавать эффективные тесты для поставщика, который ведет себя как реляционная база данных.</span><span class="sxs-lookup"><span data-stu-id="85872-105">[SQLite in-memory mode](sqlite.md) allows you to write efficient tests against a provider that behaves like a relational database.</span></span>
 * <span data-ttu-id="85872-106">[Поставщик InMemory](in-memory.md) является упрощенным поставщиком, который имеет минимальные зависимости, но не всегда ведет себя как реляционная база данных.</span><span class="sxs-lookup"><span data-stu-id="85872-106">[The InMemory provider](in-memory.md) is a lightweight provider that has minimal dependencies, but does not always behave like a relational database.</span></span>
