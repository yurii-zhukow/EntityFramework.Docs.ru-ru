---
title: Службы времени разработки — EF Core
description: Сведения о Entity Framework Core служб времени разработки
author: bricelam
ms.date: 10/26/2017
uid: core/miscellaneous/cli/services
ms.openlocfilehash: 6778a1fba025e9fbce1bb3e43462b0ee7c5dcfc4
ms.sourcegitcommit: 0a25c03fa65ae6e0e0e3f66bac48d59eceb96a5a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/14/2020
ms.locfileid: "92061948"
---
# <a name="design-time-services"></a>Службы времени разработки

Некоторые службы, используемые этими инструментами, используются только во время разработки. Эти службы управляются отдельно от служб среды выполнения EF Core, чтобы предотвратить их развертывание в приложении. Чтобы переопределить одну из этих служб (например, службу для создания файлов миграции), добавьте реализацию `IDesignTimeServices` в запускаемый проект.

[!code-csharp[Main](../../../../samples/core/Miscellaneous/CommandLine/DesignTimeServices.cs)]
