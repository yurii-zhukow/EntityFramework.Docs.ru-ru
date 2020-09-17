---
title: EF6 и Core EF — параллельное использование в одном приложении
description: Рекомендации по одновременному использованию Entity Framework Core и Entity Framework 6 в одном приложении
author: ajcvickers
ms.date: 01/23/2019
uid: efcore-and-ef6/side-by-side
ms.openlocfilehash: a9a8ab0ec77acf0fb1d1b1408d5711a6c8fa6664
ms.sourcegitcommit: abda0872f86eefeca191a9a11bfca976bc14468b
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/14/2020
ms.locfileid: "90073530"
---
# <a name="using-ef-core-and-ef6-in-the-same-application"></a>Использование EF Core и EF6 в одном приложении

EF Core и EF6 можно использовать в одном приложении или библиотеке, установив оба пакета NuGet.

Некоторые типы имеют одинаковые имена в EF Core и EF6, различаясь только пространством имен, что может усложнить параллельное использование EF Core и EF6 в одном файле кода. Эту неоднозначность можно легко устранить с помощью директив псевдонима пространства имен. Пример:

``` csharp
using Microsoft.EntityFrameworkCore; // use DbContext for EF Core
using EF6 = System.Data.Entity; // use EF6.DbContext for the EF6 version
```

Если вы портируете существующее приложение, в котором есть несколько моделей EF, у вас есть возможность перенести часть этих моделей в EF Core и сохранить EF6 для остальных.
