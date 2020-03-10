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
# <a name="testing-components-using-ef-core"></a>Тестирование компонентов с использованием EF Core

Вам может потребоваться протестировать компоненты, используя средство, имитирующее подключение к реальной базе данных, без дополнительных затрат, создаваемых операциями ввода-вывода базы данных.

Для этого существует два основных способа.

* [Режим выполнения в памяти SQLite](sqlite.md) позволяет создавать эффективные тесты для поставщика, который ведет себя как реляционная база данных.
* [Поставщик InMemory](in-memory.md) является упрощенным поставщиком, который имеет минимальные зависимости, но не всегда ведет себя как реляционная база данных.
