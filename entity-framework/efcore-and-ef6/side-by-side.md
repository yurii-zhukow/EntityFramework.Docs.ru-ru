---
title: EF6 и Core EF — параллельное использование в одном приложении
author: ajcvickers
ms.date: 01/23/2019
ms.assetid: a06e3c35-110c-4294-a1e2-32d2c31c90a7
uid: efcore-and-ef6/side-by-side
ms.openlocfilehash: bcf0a26535c4ec880a9ac25478c987fb683f6d26
ms.sourcegitcommit: b3cf5d2e3cb170b9916795d1d8c88678269639b1
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/30/2020
ms.locfileid: "76888139"
---
# <a name="using-ef-core-and-ef6-in-the-same-application"></a><span data-ttu-id="b3f16-102">Использование EF Core и EF6 в одном приложении</span><span class="sxs-lookup"><span data-stu-id="b3f16-102">Using EF Core and EF6 in the Same Application</span></span>

<span data-ttu-id="b3f16-103">EF Core и EF6 можно использовать в одном приложении или библиотеке, установив оба пакета NuGet.</span><span class="sxs-lookup"><span data-stu-id="b3f16-103">It is possible to use EF Core and EF6 in the same application or library by installing both NuGet packages.</span></span>

<span data-ttu-id="b3f16-104">Некоторые типы имеют одинаковые имена в EF Core и EF6, различаясь только пространством имен, что может усложнить параллельное использование EF Core и EF6 в одном файле кода.</span><span class="sxs-lookup"><span data-stu-id="b3f16-104">Some types have the same names in EF Core and EF6 and differ only by namespace, which may complicate using both EF Core and EF6 in the same code file.</span></span> <span data-ttu-id="b3f16-105">Эту неоднозначность можно легко устранить с помощью директив псевдонима пространства имен.</span><span class="sxs-lookup"><span data-stu-id="b3f16-105">The ambiguity can be easily removed using namespace alias directives.</span></span> <span data-ttu-id="b3f16-106">Пример:</span><span class="sxs-lookup"><span data-stu-id="b3f16-106">For example:</span></span>

``` csharp
using Microsoft.EntityFrameworkCore; // use DbContext for EF Core
using EF6 = System.Data.Entity; // use EF6.DbContext for the EF6 version
```

<span data-ttu-id="b3f16-107">Если вы портируете существующее приложение, в котором есть несколько моделей EF, у вас есть возможность перенести часть этих моделей в EF Core и сохранить EF6 для остальных.</span><span class="sxs-lookup"><span data-stu-id="b3f16-107">If you are porting an existing application that has multiple EF models, you can choose to selectively port some of them to EF Core, and continue using EF6 for the others.</span></span>
