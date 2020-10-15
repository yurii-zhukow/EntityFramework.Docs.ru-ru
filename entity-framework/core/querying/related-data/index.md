---
title: Загрузка связанных данных — EF Core
description: Различные стратегии загрузки связанных данных при использовании Entity Framework Core
author: roji
ms.date: 9/11/2020
uid: core/querying/related-data
ms.openlocfilehash: a86cae91dbfbf5ebcf820c6afb72d812e278dc66
ms.sourcegitcommit: 0a25c03fa65ae6e0e0e3f66bac48d59eceb96a5a
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/14/2020
ms.locfileid: "92063677"
---
# <a name="loading-related-data"></a>Загрузка связанных данных

Entity Framework Core позволяет использовать свойства навигации в модели для загрузки связанных сущностей. Для загрузки связанных данных используются три общих шаблона объектно-реляционного сопоставления (O/RM).

* **[Безотложная загрузка](xref:core/querying/related-data/eager)** означает, что связанные данные загружаются из базы данных как часть исходного запроса.
* **[Явная загрузка](xref:core/querying/related-data/explicit)** означает, что связанные данные явно загружаются из базы данных через некоторое время.
* **[Отложенная загрузка](xref:core/querying/related-data/lazy)** означает, что связанные данные прозрачно загружаются из базы данных при доступе к свойству навигации.

> [!TIP]
> [Примеры](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Querying/RelatedData), описанные в этом разделе, можно просмотреть на сайте GitHub.
