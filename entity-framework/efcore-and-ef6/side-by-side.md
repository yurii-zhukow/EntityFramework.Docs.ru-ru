---
title: EF6 и Core EF — параллельное использование в одном приложении
author: ajcvickers
ms.date: 01/23/2019
ms.assetid: a06e3c35-110c-4294-a1e2-32d2c31c90a7
uid: efcore-and-ef6/side-by-side
ms.openlocfilehash: bcf0a26535c4ec880a9ac25478c987fb683f6d26
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/07/2020
ms.locfileid: "78413867"
---
# <a name="using-ef-core-and-ef6-in-the-same-application"></a><span data-ttu-id="89f95-102">Использование EF Core и EF6 в одном приложении</span><span class="sxs-lookup"><span data-stu-id="89f95-102">Using EF Core and EF6 in the Same Application</span></span>

<span data-ttu-id="89f95-103">EF Core и EF6 можно использовать в одном приложении или библиотеке, установив оба пакета NuGet.</span><span class="sxs-lookup"><span data-stu-id="89f95-103">It is possible to use EF Core and EF6 in the same application or library by installing both NuGet packages.</span></span>

<span data-ttu-id="89f95-104">Некоторые типы имеют одинаковые имена в EF Core и EF6, различаясь только пространством имен, что может усложнить параллельное использование EF Core и EF6 в одном файле кода.</span><span class="sxs-lookup"><span data-stu-id="89f95-104">Some types have the same names in EF Core and EF6 and differ only by namespace, which may complicate using both EF Core and EF6 in the same code file.</span></span> <span data-ttu-id="89f95-105">Эту неоднозначность можно легко устранить с помощью директив псевдонима пространства имен.</span><span class="sxs-lookup"><span data-stu-id="89f95-105">The ambiguity can be easily removed using namespace alias directives.</span></span> <span data-ttu-id="89f95-106">Пример:</span><span class="sxs-lookup"><span data-stu-id="89f95-106">For example:</span></span>

``` csharp
using Microsoft.EntityFrameworkCore; // use DbContext for EF Core
using EF6 = System.Data.Entity; // use EF6.DbContext for the EF6 version
```

<span data-ttu-id="89f95-107">Если вы портируете существующее приложение, в котором есть несколько моделей EF, у вас есть возможность перенести часть этих моделей в EF Core и сохранить EF6 для остальных.</span><span class="sxs-lookup"><span data-stu-id="89f95-107">If you are porting an existing application that has multiple EF models, you can choose to selectively port some of them to EF Core, and continue using EF6 for the others.</span></span>
