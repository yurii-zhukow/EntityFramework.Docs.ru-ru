---
title: Общие сведения о ведении журналов и перехвате — EF Core
description: Общие сведения о ведении журналов, событиях, перехватчиках и диагностике для EF Core
author: ajcvickers
ms.date: 10/01/2020
uid: core/miscellaneous/events/index
ms.openlocfilehash: 8c46b4efb205c60be51529a89cd9dd8fb5268156
ms.sourcegitcommit: 0a25c03fa65ae6e0e0e3f66bac48d59eceb96a5a
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/14/2020
ms.locfileid: "92066500"
---
# <a name="overview-of-logging-and-interception"></a><span data-ttu-id="8c319-103">Общие сведения о ведении журналов и перехвате</span><span class="sxs-lookup"><span data-stu-id="8c319-103">Overview of logging and interception</span></span>

<span data-ttu-id="8c319-104">Entity Framework Core (EF Core) содержит несколько механизмов создания журналов, реагирования на события и получения диагностики.</span><span class="sxs-lookup"><span data-stu-id="8c319-104">Entity Framework Core (EF Core) contains several mechanisms for generating logs, responding to events, and obtaining diagnostics.</span></span> <span data-ttu-id="8c319-105">Каждый из них предназначен для различных ситуаций, и важно выбрать оптимальный механизм для конкретной задачи, даже если для ее решения подходят несколько механизмов.</span><span class="sxs-lookup"><span data-stu-id="8c319-105">Each of these are tailored to different situations, and it is important to select the best mechanism for the task in hand, even when multiple mechanisms could work.</span></span> <span data-ttu-id="8c319-106">Например, для ведения журнала SQL можно использовать перехватчик базы данных, однако лучше для этой цели подойдет один из специализированных механизмов ведения журнала.</span><span class="sxs-lookup"><span data-stu-id="8c319-106">For example, a database interceptor could be used to log SQL, but this is better handled by one of the logging-specific mechanisms.</span></span> <span data-ttu-id="8c319-107">На этой странице представлен обзор каждого из этих механизмов и описано, когда они должны использоваться.</span><span class="sxs-lookup"><span data-stu-id="8c319-107">This page presents an overview of each of these mechanisms and describes when each should be used.</span></span>

## <a name="simple-logging"></a><span data-ttu-id="8c319-108">Простое ведение журнала</span><span class="sxs-lookup"><span data-stu-id="8c319-108">Simple logging</span></span>

> [!NOTE]
> <span data-ttu-id="8c319-109">Эта функция была добавлена в EF Core 5.0.</span><span class="sxs-lookup"><span data-stu-id="8c319-109">This feature was added in EF Core 5.0.</span></span>

<span data-ttu-id="8c319-110">Доступ к журналам EF Core можно получить из любого типа приложений с помощью метода [LogTo](https://github.com/dotnet/efcore/blob/ec3df8fd7e4ea4ebeebfa747619cef37b23ab2c6/src/EFCore/DbContextOptionsBuilder.cs#L135)</span><span class="sxs-lookup"><span data-stu-id="8c319-110">EF Core logs can be accessed from any type of application through use of [LogTo](https://github.com/dotnet/efcore/blob/ec3df8fd7e4ea4ebeebfa747619cef37b23ab2c6/src/EFCore/DbContextOptionsBuilder.cs#L135)</span></span> <!-- Issue #2748 <xref:Microsoft.EntityFrameworkCore.DbContextOptionsBuilder.LogTo%2A> --> <span data-ttu-id="8c319-111">при [настройке экземпляра DbContext](xref:core/miscellaneous/configuring-dbcontext).</span><span class="sxs-lookup"><span data-stu-id="8c319-111">when [configuring a DbContext instance](xref:core/miscellaneous/configuring-dbcontext).</span></span> <span data-ttu-id="8c319-112">Такая конфигурация обычно выполняется при переопределении <xref:Microsoft.EntityFrameworkCore.DbContext.OnConfiguring%2A?displayProperty=nameWithType>.</span><span class="sxs-lookup"><span data-stu-id="8c319-112">This configuration is commonly done in an override of <xref:Microsoft.EntityFrameworkCore.DbContext.OnConfiguring%2A?displayProperty=nameWithType>.</span></span> <span data-ttu-id="8c319-113">Пример:</span><span class="sxs-lookup"><span data-stu-id="8c319-113">For example:</span></span>

<!--
    protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
        => optionsBuilder.LogTo(Console.WriteLine);
-->
[!code-csharp[LogToConsole](../../../../samples/core/Miscellaneous/Logging/SimpleLogging/Program.cs?name=LogToConsole)]

<span data-ttu-id="8c319-114">Эта концепция аналогична <xref:System.Data.Entity.Database.Log?displayProperty=nameWithType> в EF6.</span><span class="sxs-lookup"><span data-stu-id="8c319-114">This concept is similar to <xref:System.Data.Entity.Database.Log?displayProperty=nameWithType> in EF6.</span></span>

<span data-ttu-id="8c319-115">Дополнительные сведения см. в статье [Простое ведение журнала](xref:core/miscellaneous/events/simple-logging).</span><span class="sxs-lookup"><span data-stu-id="8c319-115">See [Simple Logging](xref:core/miscellaneous/events/simple-logging) for more information.</span></span>
