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
# <a name="overview-of-logging-and-interception"></a>Общие сведения о ведении журналов и перехвате

Entity Framework Core (EF Core) содержит несколько механизмов создания журналов, реагирования на события и получения диагностики. Каждый из них предназначен для различных ситуаций, и важно выбрать оптимальный механизм для конкретной задачи, даже если для ее решения подходят несколько механизмов. Например, для ведения журнала SQL можно использовать перехватчик базы данных, однако лучше для этой цели подойдет один из специализированных механизмов ведения журнала. На этой странице представлен обзор каждого из этих механизмов и описано, когда они должны использоваться.

## <a name="simple-logging"></a>Простое ведение журнала

> [!NOTE]
> Эта функция была добавлена в EF Core 5.0.

Доступ к журналам EF Core можно получить из любого типа приложений с помощью метода [LogTo](https://github.com/dotnet/efcore/blob/ec3df8fd7e4ea4ebeebfa747619cef37b23ab2c6/src/EFCore/DbContextOptionsBuilder.cs#L135) <!-- Issue #2748 <xref:Microsoft.EntityFrameworkCore.DbContextOptionsBuilder.LogTo%2A> --> при [настройке экземпляра DbContext](xref:core/miscellaneous/configuring-dbcontext). Такая конфигурация обычно выполняется при переопределении <xref:Microsoft.EntityFrameworkCore.DbContext.OnConfiguring%2A?displayProperty=nameWithType>. Пример:

<!--
    protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
        => optionsBuilder.LogTo(Console.WriteLine);
-->
[!code-csharp[LogToConsole](../../../../samples/core/Miscellaneous/Logging/SimpleLogging/Program.cs?name=LogToConsole)]

Эта концепция аналогична <xref:System.Data.Entity.Database.Log?displayProperty=nameWithType> в EF6.

Дополнительные сведения см. в статье [Простое ведение журнала](xref:core/miscellaneous/events/simple-logging).
