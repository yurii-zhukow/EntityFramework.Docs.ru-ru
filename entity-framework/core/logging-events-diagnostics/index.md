---
title: Общие сведения о ведении журналов и перехвате — EF Core
description: Общие сведения о ведении журналов, событиях, перехватчиках и диагностике для EF Core
author: ajcvickers
ms.date: 10/01/2020
uid: core/logging-events-diagnostics/index
ms.openlocfilehash: 2c44772b22112645f85cf0bffa680bc510ea5afb
ms.sourcegitcommit: 788a56c2248523967b846bcca0e98c2ed7ef0d6b
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/20/2020
ms.locfileid: "95003527"
---
# <a name="overview-of-logging-and-interception"></a>Общие сведения о ведении журналов и перехвате

Entity Framework Core (EF Core) содержит несколько механизмов создания журналов, реагирования на события и получения диагностики. Каждый из них предназначен для различных ситуаций, и важно выбрать оптимальный механизм для конкретной задачи, даже если для ее решения подходят несколько механизмов. Например, для ведения журнала SQL можно использовать перехватчик базы данных, однако лучше для этой цели подойдет один из механизмов, адаптированный для ведения журнала. На этой странице представлен обзор каждого из этих механизмов и описано, когда они должны использоваться.

## <a name="quick-reference"></a>Краткий справочник

В указанной ниже таблице приведены краткие сведения о различиях между описанными здесь механизмами.

| Механизм |  Async | Область | Зарегистрировано | предполагаемое использование;
|:----------|--------|-------|------------|-------------
| Простое ведение журнала | нет | Для каждого контекста | Конфигурация контекста | Ведение журнала во время разработки
| Microsoft.Extensions.Logging | нет | Для каждого контекста* | Внедрение зависимостей или конфигурация контекста | Ведение журнала в рабочей среде
| События | нет | Для каждого контекста | Любое время | Реагирование на события EF
| Перехватчики | Да | Для каждого контекста | Конфигурация контекста | Обработка операций EF
| Прослушиватели данных диагностики | нет | Процесс | Глобально | Диагностика приложения

*Обычно `Microsoft.Extensions.Logging` настраивается для каждого приложения посредством внедрения зависимостей. Однако на уровне EF каждый контекст при необходимости _можно_ настроить с помощью разных средств ведения журнала.

## <a name="simple-logging"></a>Простое ведение журнала

> [!NOTE]
> Эта возможность появилась в EF Core 5.0.

Доступ к журналам EF Core можно получить из любого типа приложений с помощью метода [LogTo](https://github.com/dotnet/efcore/blob/ec3df8fd7e4ea4ebeebfa747619cef37b23ab2c6/src/EFCore/DbContextOptionsBuilder.cs#L135) <!-- Issue #2748 <xref:Microsoft.EntityFrameworkCore.DbContextOptionsBuilder.LogTo%2A> --> при [настройке экземпляра DbContext](xref:core/dbcontext-configuration/index). Такая конфигурация обычно выполняется при переопределении <xref:Microsoft.EntityFrameworkCore.DbContext.OnConfiguring%2A?displayProperty=nameWithType>. Пример:

<!--
    protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
        => optionsBuilder.LogTo(Console.WriteLine);
-->
[!code-csharp[LogToConsole](../../../samples/core/Miscellaneous/Logging/SimpleLogging/Program.cs?name=LogToConsole)]

Эта концепция аналогична <xref:System.Data.Entity.Database.Log?displayProperty=nameWithType> в EF6.

Дополнительные сведения см. в статье [Простое ведение журнала](xref:core/logging-events-diagnostics/simple-logging).

## <a name="microsoftextensionslogging"></a>Microsoft.Extensions.Logging

[Microsoft.Extensions.Logging](/dotnet/core/extensions/logging) — это расширяемый механизм ведения журнала с поставщиками подключаемых модулей для многих распространенных систем ведения журналов. EF Core полностью интегрируется с `Microsoft.Extensions.Logging`. Эта форма ведения журнала используется по умолчанию для приложений ASP.NET Core.

Дополнительные сведения см. в статье об [использовании Microsoft.Extensions.Logging в EF Core](xref:core/logging-events-diagnostics/extensions-logging).

## <a name="events"></a>События

> [!NOTE]
> В EF Core 5.0 были добавлены дополнительные события.

EF Core предоставляет [события .NET](/dotnet/standard/events/) в качестве обратных вызовов, когда в коде EF Core происходят определенные действия. Использовать события проще по сравнению с перехватчиками. Они также обеспечивают более гибкую регистрацию. Однако они только синхронизируются и поэтому не могут выполнять неблокирующие асинхронные операции ввода-вывода.

События регистрируются для каждого экземпляра DbContext. Эту регистрацию можно выполнить в любое время. С помощью [прослушивателя диагностики](xref:core/logging-events-diagnostics/diagnostic-listeners) можно получить ту же информацию, но для всех экземпляров DbContext в процессе.

Дополнительные сведения см. в статье о [событиях .NET в EF Core](xref:core/logging-events-diagnostics/events).

## <a name="interception"></a>Interception

> [!NOTE]
> Эта возможность появилась в EF Core 3.0. В EF Core 5.0 были добавлены дополнительные перехватчики.

Перехватчики EF Core позволяют выполнять перехват, изменение и подавление операций EF Core. Сюда входят низкоуровневые операции с базами данных, такие как выполнение команды, а также операции более высокого уровня, такие как вызовы SaveChanges.

Перехватчики отличаются от ведения журнала и диагностики, так как они позволяют изменять или подавлять перехватываемую операцию. Для ведения журнала лучше всего использовать [средство простого ведения журнала](xref:core/logging-events-diagnostics/simple-logging) или [Microsoft.Extensions.Logging](xref:core/logging-events-diagnostics/extensions-logging).

Перехватчики регистрируются для каждого экземпляра DbContext во время настройки контекста. С помощью [прослушивателя диагностики](xref:core/logging-events-diagnostics/diagnostic-listeners) можно получить ту же информацию, но для всех экземпляров DbContext в процессе.

Подробные сведения см. в статье [Перехватчики](xref:core/logging-events-diagnostics/interceptors).

## <a name="diagnostic-listeners"></a>Прослушиватели диагностики

Прослушиватели диагностики позволяют прослушивать любое событие EF Core, возникающее в текущем процессе .NET.

Прослушиватели диагностики не подходят для получения событий из одного экземпляра DbContext. Перехватчики EF Core предоставляют доступ к одним и тем же событиям с регистрацией для каждого контекста.

Прослушиватели диагностики не предназначены для ведения журнала. Для ведения журнала лучше всего использовать [средство простого ведения журнала](xref:core/logging-events-diagnostics/simple-logging) или [Microsoft.Extensions.Logging](xref:core/logging-events-diagnostics/extensions-logging).

Дополнительные сведения см. в статье [об использовании прослушивателей диагностики в EF Core](xref:core/logging-events-diagnostics/diagnostic-listeners).
