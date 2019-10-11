---
title: Тестирование компонентов с помощью EF Core — EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 1603be0c-69bc-4dd9-9a08-3d0129cdc6c1
uid: core/miscellaneous/testing/index
ms.openlocfilehash: 8de7df80ce91c4d94133a96d759dd552d0ba1884
ms.sourcegitcommit: 708b18520321c587b2046ad2ea9fa7c48aeebfe5
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/09/2019
ms.locfileid: "72181312"
---
# <a name="testing-components-using-ef-core"></a>Тестирование компонентов с использованием EF Core

Вам может потребоваться протестировать компоненты, используя средство, имитирующее подключение к реальной базе данных, без дополнительных затрат, создаваемых операциями ввода-вывода базы данных.

Для этого существует два основных способа.
 * [Режим выполнения в памяти SQLite](sqlite.md) позволяет создавать эффективные тесты для поставщика, который ведет себя как реляционная база данных.
 * [Поставщик InMemory](in-memory.md) является упрощенным поставщиком, который имеет минимальные зависимости, но не всегда ведет себя как реляционная база данных.
