---
title: Справочник по инструментам Entity Framework Core — EF Core
author: bricelam
ms.author: bricelam
ms.date: 09/19/2018
uid: core/miscellaneous/cli/index
ms.openlocfilehash: 2821263f1e9cd962569e8050b01d704cf858e6c8
ms.sourcegitcommit: 949faaba02e07e44359e77d7935f540af5c32093
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/03/2020
ms.locfileid: "87526490"
---
# <a name="entity-framework-core-tools-reference"></a>Справочник по инструментам Entity Framework Core

Инструменты Entity Framework Core используются для задач разработки. Эти инструменты используются в основном для управления миграциями и формирования `DbContext` и типов сущностей путем реконструирования схемы базы данных.

* [Инструменты консоли диспетчера пакетов EF Core](powershell.md) запускаются в [консоли диспетчера пакетов](/nuget/tools/package-manager-console) в Visual Studio.

* [Инструменты интерфейса командной строки EF Core .NET](dotnet.md) представляют собой расширение для кроссплатформенных [инструментов .NET Core CLI](/dotnet/core/tools/). Этим инструментам нужен проект пакета SDK .NET Core (с `Sdk="Microsoft.NET.Sdk"` или аналогичным объектом в файле проекта).

Оба варианта инструментов предоставляют одинаковую функциональность. При разработке в Visual Studio мы рекомендуем использовать инструменты **консоли диспетчера пакетов**, так как они лучше интегрированы.

## <a name="next-steps"></a>Следующие шаги

* [Справочник по инструментам консоли диспетчера пакетов EF Core](powershell.md)
* [Справочник по инструментам EF Core — .NET CLI](dotnet.md)
