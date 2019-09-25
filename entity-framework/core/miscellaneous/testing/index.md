---
title: Тестирование компонентов с помощью EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 1603be0c-69bc-4dd9-9a08-3d0129cdc6c1
uid: core/miscellaneous/testing/index
ms.openlocfilehash: a946387718546f14e1485b4093e6c8046188f62d
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/23/2019
ms.locfileid: "71197492"
---
# <a name="testing-components-using-ef-core"></a><span data-ttu-id="24dd6-102">Тестирование компонентов с использованием EF Core</span><span class="sxs-lookup"><span data-stu-id="24dd6-102">Testing components using EF Core</span></span>

<span data-ttu-id="24dd6-103">Вам может потребоваться протестировать компоненты, используя средство, имитирующее подключение к реальной базе данных, без дополнительных затрат, создаваемых операциями ввода-вывода базы данных.</span><span class="sxs-lookup"><span data-stu-id="24dd6-103">You may want to test components using something that approximates connecting to the real database, without the overhead of actual database I/O operations.</span></span>

<span data-ttu-id="24dd6-104">Для этого существует два основных способа.</span><span class="sxs-lookup"><span data-stu-id="24dd6-104">There are two main options for doing this:</span></span>
 * <span data-ttu-id="24dd6-105">[Режим выполнения в памяти SQLite](sqlite.md) позволяет создавать эффективные тесты для поставщика, который ведет себя как реляционная база данных.</span><span class="sxs-lookup"><span data-stu-id="24dd6-105">[SQLite in-memory mode](sqlite.md) allows you to write efficient tests against a provider that behaves like a relational database.</span></span>
 * <span data-ttu-id="24dd6-106">[Поставщик InMemory](in-memory.md) является упрощенным поставщиком, который имеет минимальные зависимости, но не всегда ведет себя как реляционная база данных.</span><span class="sxs-lookup"><span data-stu-id="24dd6-106">[The InMemory provider](in-memory.md) is a lightweight provider that has minimal dependencies, but does not always behave like a relational database.</span></span>
