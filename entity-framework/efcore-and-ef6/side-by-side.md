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
# <a name="using-ef-core-and-ef6-in-the-same-application"></a><span data-ttu-id="836a8-103">Использование EF Core и EF6 в одном приложении</span><span class="sxs-lookup"><span data-stu-id="836a8-103">Using EF Core and EF6 in the Same Application</span></span>

<span data-ttu-id="836a8-104">EF Core и EF6 можно использовать в одном приложении или библиотеке, установив оба пакета NuGet.</span><span class="sxs-lookup"><span data-stu-id="836a8-104">It is possible to use EF Core and EF6 in the same application or library by installing both NuGet packages.</span></span>

<span data-ttu-id="836a8-105">Некоторые типы имеют одинаковые имена в EF Core и EF6, различаясь только пространством имен, что может усложнить параллельное использование EF Core и EF6 в одном файле кода.</span><span class="sxs-lookup"><span data-stu-id="836a8-105">Some types have the same names in EF Core and EF6 and differ only by namespace, which may complicate using both EF Core and EF6 in the same code file.</span></span> <span data-ttu-id="836a8-106">Эту неоднозначность можно легко устранить с помощью директив псевдонима пространства имен.</span><span class="sxs-lookup"><span data-stu-id="836a8-106">The ambiguity can be easily removed using namespace alias directives.</span></span> <span data-ttu-id="836a8-107">Пример:</span><span class="sxs-lookup"><span data-stu-id="836a8-107">For example:</span></span>

``` csharp
using Microsoft.EntityFrameworkCore; // use DbContext for EF Core
using EF6 = System.Data.Entity; // use EF6.DbContext for the EF6 version
```

<span data-ttu-id="836a8-108">Если вы портируете существующее приложение, в котором есть несколько моделей EF, у вас есть возможность перенести часть этих моделей в EF Core и сохранить EF6 для остальных.</span><span class="sxs-lookup"><span data-stu-id="836a8-108">If you are porting an existing application that has multiple EF models, you can choose to selectively port some of them to EF Core, and continue using EF6 for the others.</span></span>
