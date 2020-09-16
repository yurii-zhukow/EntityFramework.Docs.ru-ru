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
# <a name="loading-related-data"></a><span data-ttu-id="59aaf-103">Загрузка связанных данных</span><span class="sxs-lookup"><span data-stu-id="59aaf-103">Loading Related Data</span></span>

<span data-ttu-id="59aaf-104">Entity Framework Core позволяет использовать свойства навигации в модели для загрузки связанных сущностей.</span><span class="sxs-lookup"><span data-stu-id="59aaf-104">Entity Framework Core allows you to use the navigation properties in your model to load related entities.</span></span> <span data-ttu-id="59aaf-105">Для загрузки связанных данных используются три общих шаблона объектно-реляционного сопоставления (O/RM).</span><span class="sxs-lookup"><span data-stu-id="59aaf-105">There are three common O/RM patterns used to load related data.</span></span>

* <span data-ttu-id="59aaf-106">**[Безотложная загрузка](xref:core/querying/related-data/eager)** означает, что связанные данные загружаются из базы данных как часть исходного запроса.</span><span class="sxs-lookup"><span data-stu-id="59aaf-106">**[Eager loading](xref:core/querying/related-data/eager)** means that the related data is loaded from the database as part of the initial query.</span></span>
* <span data-ttu-id="59aaf-107">**[Явная загрузка](xref:core/querying/related-data/explicit)** означает, что связанные данные явно загружаются из базы данных через некоторое время.</span><span class="sxs-lookup"><span data-stu-id="59aaf-107">**[Explicit loading](xref:core/querying/related-data/explicit)** means that the related data is explicitly loaded from the database at a later time.</span></span>
* <span data-ttu-id="59aaf-108">**[Отложенная загрузка](xref:core/querying/related-data/lazy)** означает, что связанные данные прозрачно загружаются из базы данных при доступе к свойству навигации.</span><span class="sxs-lookup"><span data-stu-id="59aaf-108">**[Lazy loading](xref:core/querying/related-data/lazy)** means that the related data is transparently loaded from the database when the navigation property is accessed.</span></span>

> [!TIP]
> <span data-ttu-id="59aaf-109">[Примеры](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Querying), описанные в этом разделе, можно просмотреть на сайте GitHub.</span><span class="sxs-lookup"><span data-stu-id="59aaf-109">You can view the [samples](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Querying) under this section on GitHub.</span></span>
