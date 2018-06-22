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
# <a name="testing"></a>Тестирование

Вам может потребоваться протестировать компоненты, используя средство, имитирующее подключение к реальной базе данных, без дополнительных затрат, создаваемых операциями ввода-вывода базы данных.

Для этого существует два основных способа.
 * [Режим выполнения в памяти SQLite](sqlite.md) позволяет создавать эффективные тесты для поставщика, который ведет себя как реляционная база данных.
 * [Поставщик InMemory](in-memory.md) является упрощенным поставщиком, который имеет минимальные зависимости, но не всегда ведет себя как реляционная база данных.
