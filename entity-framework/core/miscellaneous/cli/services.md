---
title: Службы времени разработки — EF Core
author: bricelam
ms.author: bricelam
ms.date: 10/26/2017
uid: core/miscellaneous/cli/services
ms.openlocfilehash: 57294ab41e7c251b1dafae9d573aa98676c5d939
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/06/2019
ms.locfileid: "73655825"
---
# <a name="design-time-services"></a>Службы времени разработки

Некоторые службы, используемые этими инструментами, используются только во время разработки. Эти службы управляются отдельно от служб среды выполнения EF Core, чтобы предотвратить их развертывание в приложении. Чтобы переопределить одну из этих служб (например, службу для создания файлов миграции), добавьте в запускаемый проект реализацию `IDesignTimeServices`.

[!code-csharp[Main](../../../../samples/core/Miscellaneous/CommandLine/DesignTimeServices.cs)]
