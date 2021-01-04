---
title: Справочник по инструментам Entity Framework Core — EF Core
description: Справочное руководство по средству CLI Entity Framework Core и консоли диспетчера пакетов Visual Studio
author: bricelam
ms.date: 09/19/2018
uid: core/cli/index
ms.openlocfilehash: 1ffc773cb8ed30516d682b90bbd9accef634ae6a
ms.sourcegitcommit: 4860d036ea0fb392c28799907bcc924c987d2d7b
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/17/2020
ms.locfileid: "97635385"
---
# <a name="entity-framework-core-tools-reference"></a>Справочник по инструментам Entity Framework Core

Инструменты Entity Framework Core используются для задач разработки. Эти инструменты используются в основном для управления миграциями и формирования `DbContext` и типов сущностей путем реконструирования схемы базы данных.

Можно установить любое из следующих средств, так как оба средства предоставляют одинаковую функциональность:

* [Инструменты консоли диспетчера пакетов EF Core](xref:core/cli/powershell) запускаются в [консоли диспетчера пакетов](/nuget/tools/package-manager-console) в Visual Studio. Мы рекомендуем использовать эти средства при разработке в Visual Studio, так как они лучше интегрированы.

* [Инструменты интерфейса командной строки EF Core .NET](xref:core/cli/dotnet) представляют собой расширение для кроссплатформенных [инструментов .NET Core CLI](/dotnet/core/tools/). Этим инструментам нужен проект пакета SDK .NET Core (с `Sdk="Microsoft.NET.Sdk"` или аналогичным объектом в файле проекта).

## <a name="next-steps"></a>Следующие шаги

* [Справочник по инструментам консоли диспетчера пакетов EF Core](xref:core/cli/powershell)
* [Справочник по инструментам EF Core — .NET CLI](xref:core/cli/dotnet)
