---
title: Справочник по инструментам Entity Framework Core — EF Core
description: Справочное руководство по средству CLI Entity Framework Core и консоли диспетчера пакетов Visual Studio
author: bricelam
ms.author: bricelam
ms.date: 09/19/2018
uid: core/miscellaneous/cli/index
ms.openlocfilehash: 7d5fb984aafed2cf45efa9e5b83bf4bc6c18a44a
ms.sourcegitcommit: 7c3939504bb9da3f46bea3443638b808c04227c2
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/09/2020
ms.locfileid: "89619420"
---
# <a name="entity-framework-core-tools-reference"></a>Справочник по инструментам Entity Framework Core

Инструменты Entity Framework Core используются для задач разработки. Эти инструменты используются в основном для управления миграциями и формирования `DbContext` и типов сущностей путем реконструирования схемы базы данных.

* [Инструменты консоли диспетчера пакетов EF Core](xref:core/miscellaneous/cli/powershell) запускаются в [консоли диспетчера пакетов](/nuget/tools/package-manager-console) в Visual Studio.

* [Инструменты интерфейса командной строки EF Core .NET](xref:core/miscellaneous/cli/dotnet) представляют собой расширение для кроссплатформенных [инструментов .NET Core CLI](/dotnet/core/tools/). Этим инструментам нужен проект пакета SDK .NET Core (с `Sdk="Microsoft.NET.Sdk"` или аналогичным объектом в файле проекта).

Оба варианта инструментов предоставляют одинаковую функциональность. При разработке в Visual Studio мы рекомендуем использовать инструменты **консоли диспетчера пакетов**, так как они лучше интегрированы.

## <a name="next-steps"></a>Следующие шаги

* [Справочник по инструментам консоли диспетчера пакетов EF Core](xref:core/miscellaneous/cli/powershell)
* [Справочник по инструментам EF Core — .NET CLI](xref:core/miscellaneous/cli/dotnet)
