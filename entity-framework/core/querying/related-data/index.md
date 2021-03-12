---
title: Загрузка связанных данных — EF Core
description: Различные стратегии загрузки связанных данных при использовании Entity Framework Core
author: roji
ms.date: 9/11/2020
uid: core/querying/related-data
ms.openlocfilehash: 8d7fa1ac5673fe4289b18c5b8e12563683463fe8
ms.sourcegitcommit: 4798ab8d04c1fdbe6dd204d94d770fcbf309d09b
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/11/2021
ms.locfileid: "103023722"
---
# <a name="loading-related-data"></a>Загрузка связанных данных

Entity Framework Core позволяет использовать свойства навигации в модели для загрузки связанных сущностей. Для загрузки связанных данных используются три общих шаблона объектно-реляционного сопоставления (O/RM).

* **[Безотложная загрузка](xref:core/querying/related-data/eager)** означает, что связанные данные загружаются из базы данных как часть исходного запроса.
* **[Явная загрузка](xref:core/querying/related-data/explicit)** означает, что связанные данные явно загружаются из базы данных через некоторое время.
* **[Отложенная загрузка](xref:core/querying/related-data/lazy)** означает, что связанные данные прозрачно загружаются из базы данных при доступе к свойству навигации.

> [!TIP]
> [Примеры](https://github.com/dotnet/EntityFramework.Docs/tree/main/samples/core/Querying/RelatedData), описанные в этом разделе, можно просмотреть на сайте GitHub.
