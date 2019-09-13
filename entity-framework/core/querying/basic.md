---
title: Базовые запросы — EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: ab6e35f1-397f-41c0-9ef4-85aec5466377
uid: core/querying/basic
ms.openlocfilehash: 49daa0d37175244617993cc6e911cbd59d27079f
ms.sourcegitcommit: b2b9468de2cf930687f8b85c3ce54ff8c449f644
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/12/2019
ms.locfileid: "70921741"
---
# <a name="basic-queries"></a>Базовые запросы

Узнайте, как загружать сущности из базы данных с помощью LINQ.

> [!TIP]  
> Для этой статьи вы можете скачать [пример](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) из репозитория GitHub.

## <a name="101-linq-samples"></a>101 пример LINQ

На этой странице показано несколько примеров выполнения типичных задач с использованием Entity Framework Core. Широкий набор примеров, демонстрирующих возможности LINQ, см. в разделе со [101 примером LINQ](https://code.msdn.microsoft.com/101-LINQ-Samples-3fb9811b).

## <a name="loading-all-data"></a>Загрузка всех данных

<!-- [!code-csharp[Main](samples/core/Querying/Basics/Sample.cs)] -->
``` csharp
using (var context = new BloggingContext())
{
    var blogs = context.Blogs.ToList();
}
```

## <a name="loading-a-single-entity"></a>Загрузка отдельной сущности

<!-- [!code-csharp[Main](samples/core/Querying/Basics/Sample.cs)] -->
``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs
        .Single(b => b.BlogId == 1);
}
```

## <a name="filtering"></a>Фильтрация

<!-- [!code-csharp[Main](samples/core/Querying/Basics/Sample.cs)] -->
``` csharp
using (var context = new BloggingContext())
{
    var blogs = context.Blogs
        .Where(b => b.Url.Contains("dotnet"))
        .ToList();
}
```
