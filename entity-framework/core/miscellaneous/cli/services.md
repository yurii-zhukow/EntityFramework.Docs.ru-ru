---
title: Службы времени разработки — EF Core
description: Сведения о Entity Framework Core служб времени разработки
author: bricelam
ms.author: bricelam
ms.date: 10/26/2017
uid: core/miscellaneous/cli/services
ms.openlocfilehash: aa761c4a20e0848a77aa7b4ad625124a1d372a70
ms.sourcegitcommit: 7c3939504bb9da3f46bea3443638b808c04227c2
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/09/2020
ms.locfileid: "89617848"
---
# <a name="design-time-services"></a>Службы времени разработки

Некоторые службы, используемые этими инструментами, используются только во время разработки. Эти службы управляются отдельно от служб среды выполнения EF Core, чтобы предотвратить их развертывание в приложении. Чтобы переопределить одну из этих служб (например, службу для создания файлов миграции), добавьте реализацию `IDesignTimeServices` в запускаемый проект.

[!code-csharp[Main](../../../../samples/core/Miscellaneous/CommandLine/DesignTimeServices.cs)]
