---
title: Тестирование компонентов с использованием Entity Framework — EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 1603be0c-69bc-4dd9-9a08-3d0129cdc6c1
uid: core/miscellaneous/testing/index
ms.openlocfilehash: fc751b9053c337e4911f4016b65b370d1276046b
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/27/2018
ms.locfileid: "42997841"
---
# <a name="testing"></a><span data-ttu-id="3cf9f-102">Тестирование</span><span class="sxs-lookup"><span data-stu-id="3cf9f-102">Testing</span></span>

<span data-ttu-id="3cf9f-103">Вам может потребоваться протестировать компоненты, используя средство, имитирующее подключение к реальной базе данных, без дополнительных затрат, создаваемых операциями ввода-вывода базы данных.</span><span class="sxs-lookup"><span data-stu-id="3cf9f-103">You may want to test components using something that approximates connecting to the real database, without the overhead of actual database I/O operations.</span></span>

<span data-ttu-id="3cf9f-104">Для этого существует два основных способа.</span><span class="sxs-lookup"><span data-stu-id="3cf9f-104">There are two main options for doing this:</span></span>
 * <span data-ttu-id="3cf9f-105">[Режим выполнения в памяти SQLite](sqlite.md) позволяет создавать эффективные тесты для поставщика, который ведет себя как реляционная база данных.</span><span class="sxs-lookup"><span data-stu-id="3cf9f-105">[SQLite in-memory mode](sqlite.md) allows you to write efficient tests against a provider that behaves like a relational database.</span></span>
 * <span data-ttu-id="3cf9f-106">[Поставщик InMemory](in-memory.md) является упрощенным поставщиком, который имеет минимальные зависимости, но не всегда ведет себя как реляционная база данных.</span><span class="sxs-lookup"><span data-stu-id="3cf9f-106">[The InMemory provider](in-memory.md) is a lightweight provider that has minimal dependencies, but does not always behave like a relational database.</span></span>
