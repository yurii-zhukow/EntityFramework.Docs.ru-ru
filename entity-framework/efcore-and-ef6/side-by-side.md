---
title: EF6 и Core EF — параллельное использование в одном приложении
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: a06e3c35-110c-4294-a1e2-32d2c31c90a7
uid: efcore-and-ef6/side-by-side
ms.openlocfilehash: 8bf9f51c0e5c4b1b3adf4a6a9a894689dc13d2d9
ms.sourcegitcommit: cbaa6cc89bd71d5e0bcc891e55743f0e8ea3393b
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/20/2019
ms.locfileid: "71149294"
---
# <a name="using-ef-core-and-ef6-in-the-same-application"></a>Использование EF Core и EF6 в одном приложении

EF Core и EF6 можно использовать в одном приложении или библиотеке, установив оба пакета NuGet.

Некоторые типы имеют одинаковые имена в EF Core и EF6, различаясь только пространством имен, что может усложнить параллельное использование EF Core и EF6 в одном файле кода. Эту неоднозначность можно легко устранить с помощью директив псевдонима пространства имен. Например:

``` csharp
using Microsoft.EntityFrameworkCore; // use DbContext for EF Core
using EF6 = System.Data.Entity; // use EF6.DbContext for the EF6 version
```

Если вы портируете существующее приложение, в котором есть несколько моделей EF, у вас есть возможность перенести часть этих моделей в EF Core и сохранить EF6 для остальных.
