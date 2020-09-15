---
title: EF6 и Core EF — параллельное использование в одном приложении
description: Рекомендации по одновременному использованию Entity Framework Core и Entity Framework 6 в одном приложении
author: ajcvickers
ms.date: 01/23/2019
ms.assetid: a06e3c35-110c-4294-a1e2-32d2c31c90a7
uid: efcore-and-ef6/side-by-side
ms.openlocfilehash: 9bd3d837a333eb23be4cb3095b7f387ad303376d
ms.sourcegitcommit: 7c3939504bb9da3f46bea3443638b808c04227c2
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/09/2020
ms.locfileid: "89619546"
---
# <a name="using-ef-core-and-ef6-in-the-same-application"></a>Использование EF Core и EF6 в одном приложении

EF Core и EF6 можно использовать в одном приложении или библиотеке, установив оба пакета NuGet.

Некоторые типы имеют одинаковые имена в EF Core и EF6, различаясь только пространством имен, что может усложнить параллельное использование EF Core и EF6 в одном файле кода. Эту неоднозначность можно легко устранить с помощью директив псевдонима пространства имен. Пример:

``` csharp
using Microsoft.EntityFrameworkCore; // use DbContext for EF Core
using EF6 = System.Data.Entity; // use EF6.DbContext for the EF6 version
```

Если вы портируете существующее приложение, в котором есть несколько моделей EF, у вас есть возможность перенести часть этих моделей в EF Core и сохранить EF6 для остальных.
