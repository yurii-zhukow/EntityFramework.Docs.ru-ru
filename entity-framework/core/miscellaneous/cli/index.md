---
title: Справочник по инструментам Entity Framework Core — EF Core
author: bricelam
ms.author: bricelam
ms.date: 09/19/2018
uid: core/miscellaneous/cli/index
ms.openlocfilehash: 237192c55ea3542521a7a292ac8550d72e4ef82c
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/07/2020
ms.locfileid: "78412819"
---
# <a name="entity-framework-core-tools-reference"></a>Справочник по инструментам Entity Framework Core

Инструменты Entity Framework Core используются для задач разработки. Эти инструменты используются в основном для управления миграциями и формирования `DbContext` и типов сущностей путем реконструирования схемы базы данных.

* [Инструменты консоли диспетчера пакетов EF Core](powershell.md) запускаются в [консоли диспетчера пакетов](https://docs.microsoft.com/nuget/tools/package-manager-console) в Visual Studio.

* [Инструменты интерфейса командной строки EF Core .NET](dotnet.md) представляют собой расширение для кроссплатформенных [инструментов .NET Core CLI](https://docs.microsoft.com/dotnet/core/tools/). Этим инструментам нужен проект пакета SDK .NET Core (с `Sdk="Microsoft.NET.Sdk"` или аналогичным объектом в файле проекта).

Оба варианта инструментов предоставляют одинаковую функциональность. При разработке в Visual Studio мы рекомендуем использовать инструменты **консоли диспетчера пакетов**, так как они лучше интегрированы.

## <a name="next-steps"></a>Дальнейшие действия

* [Справочник по инструментам консоли диспетчера пакетов EF Core](powershell.md)
* [Справочник по инструментам EF Core — .NET CLI](dotnet.md)
