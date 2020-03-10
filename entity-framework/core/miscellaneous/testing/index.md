---
title: Тестирование компонентов с помощью EF Core — EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 1603be0c-69bc-4dd9-9a08-3d0129cdc6c1
uid: core/miscellaneous/testing/index
ms.openlocfilehash: 1ca900528ed42ad4b41016f22964c3494b0286eb
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/06/2020
ms.locfileid: "78412799"
---
# <a name="testing-components-using-ef-core"></a><span data-ttu-id="4ddfe-102">Тестирование компонентов с использованием EF Core</span><span class="sxs-lookup"><span data-stu-id="4ddfe-102">Testing components using EF Core</span></span>

<span data-ttu-id="4ddfe-103">Вам может потребоваться протестировать компоненты, используя средство, имитирующее подключение к реальной базе данных, без дополнительных затрат, создаваемых операциями ввода-вывода базы данных.</span><span class="sxs-lookup"><span data-stu-id="4ddfe-103">You may want to test components using something that approximates connecting to the real database, without the overhead of actual database I/O operations.</span></span>

<span data-ttu-id="4ddfe-104">Для этого существует два основных способа.</span><span class="sxs-lookup"><span data-stu-id="4ddfe-104">There are two main options for doing this:</span></span>

* <span data-ttu-id="4ddfe-105">[Режим выполнения в памяти SQLite](sqlite.md) позволяет создавать эффективные тесты для поставщика, который ведет себя как реляционная база данных.</span><span class="sxs-lookup"><span data-stu-id="4ddfe-105">[SQLite in-memory mode](sqlite.md) allows you to write efficient tests against a provider that behaves like a relational database.</span></span>
* <span data-ttu-id="4ddfe-106">[Поставщик InMemory](in-memory.md) является упрощенным поставщиком, который имеет минимальные зависимости, но не всегда ведет себя как реляционная база данных.</span><span class="sxs-lookup"><span data-stu-id="4ddfe-106">[The InMemory provider](in-memory.md) is a lightweight provider that has minimal dependencies, but does not always behave like a relational database.</span></span>
