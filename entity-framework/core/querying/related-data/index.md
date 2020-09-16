---
title: Загрузка связанных данных — EF Core
description: Различные стратегии загрузки связанных данных при использовании Entity Framework Core
author: roji
ms.date: 9/11/2020
ms.assetid: f9fb64e2-6699-4d70-a773-592918c04c19
uid: core/querying/related-data
ms.openlocfilehash: 5c1addac6735de39d5c72f447d5d06280411a21e
ms.sourcegitcommit: abda0872f86eefeca191a9a11bfca976bc14468b
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/14/2020
ms.locfileid: "90078855"
---
# <a name="loading-related-data"></a>Загрузка связанных данных

Entity Framework Core позволяет использовать свойства навигации в модели для загрузки связанных сущностей. Для загрузки связанных данных используются три общих шаблона объектно-реляционного сопоставления (O/RM).

* **[Безотложная загрузка](xref:core/querying/related-data/eager)** означает, что связанные данные загружаются из базы данных как часть исходного запроса.
* **[Явная загрузка](xref:core/querying/related-data/explicit)** означает, что связанные данные явно загружаются из базы данных через некоторое время.
* **[Отложенная загрузка](xref:core/querying/related-data/lazy)** означает, что связанные данные прозрачно загружаются из базы данных при доступе к свойству навигации.

> [!TIP]
> [Примеры](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Querying), описанные в этом разделе, можно просмотреть на сайте GitHub.
