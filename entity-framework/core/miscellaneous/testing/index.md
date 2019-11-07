---
title: Тестирование компонентов с помощью EF Core — EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 1603be0c-69bc-4dd9-9a08-3d0129cdc6c1
uid: core/miscellaneous/testing/index
ms.openlocfilehash: 1ca900528ed42ad4b41016f22964c3494b0286eb
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/06/2019
ms.locfileid: "73655789"
---
# <a name="testing-components-using-ef-core"></a><span data-ttu-id="28802-102">Тестирование компонентов с использованием EF Core</span><span class="sxs-lookup"><span data-stu-id="28802-102">Testing components using EF Core</span></span>

<span data-ttu-id="28802-103">Вам может потребоваться протестировать компоненты, используя средство, имитирующее подключение к реальной базе данных, без дополнительных затрат, создаваемых операциями ввода-вывода базы данных.</span><span class="sxs-lookup"><span data-stu-id="28802-103">You may want to test components using something that approximates connecting to the real database, without the overhead of actual database I/O operations.</span></span>

<span data-ttu-id="28802-104">Для этого существует два основных способа.</span><span class="sxs-lookup"><span data-stu-id="28802-104">There are two main options for doing this:</span></span>

* <span data-ttu-id="28802-105">[Режим выполнения в памяти SQLite](sqlite.md) позволяет создавать эффективные тесты для поставщика, который ведет себя как реляционная база данных.</span><span class="sxs-lookup"><span data-stu-id="28802-105">[SQLite in-memory mode](sqlite.md) allows you to write efficient tests against a provider that behaves like a relational database.</span></span>
* <span data-ttu-id="28802-106">[Поставщик InMemory](in-memory.md) является упрощенным поставщиком, который имеет минимальные зависимости, но не всегда ведет себя как реляционная база данных.</span><span class="sxs-lookup"><span data-stu-id="28802-106">[The InMemory provider](in-memory.md) is a lightweight provider that has minimal dependencies, but does not always behave like a relational database.</span></span>
