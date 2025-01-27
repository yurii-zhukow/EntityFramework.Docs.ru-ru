---
title: Выпуски и планирование EF Core
description: Сведения о текущих выпусках EF Core, а также о графике и планах на будущие выпуски.
author: ajcvickers
ms.date: 01/12/2021
uid: core/what-is-new/index
ms.openlocfilehash: 15e0f50bfe27e52c8c545c56c3ee5b4b141152c1
ms.sourcegitcommit: 4798ab8d04c1fdbe6dd204d94d770fcbf309d09b
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/11/2021
ms.locfileid: "103023566"
---
# <a name="ef-core-releases-and-planning"></a>Выпуски и планирование EF Core

## <a name="stable-releases"></a>Стабильные выпуски

| Выпуск | Целевая платформа | Поддержка до | Ссылки
|:--------|------------------|-----------------|------
| [EF Core 5.0](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore) | .NET Standard 2.1 | Середина февраля 2022 г. | [Объявление](https://devblogs.microsoft.com/dotnet/announcing-the-release-of-ef-core-5-0/) / [Критические изменения](xref:core/what-is-new/ef-core-5.0/breaking-changes)
| [EF Core 3.1](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore/3.1.10) | .NET Standard 2.0 | 3 декабря 2022 г. (LTS) | [Объявление](https://devblogs.microsoft.com/dotnet/announcing-entity-framework-core-3-1-and-entity-framework-6-4/)
| ~~[EF Core 3.0](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore/3.0.3)~~ | .NET Standard 2.1 | Срок действия истек 3 марта 2020 г. | [Объявление](https://devblogs.microsoft.com/dotnet/announcing-ef-core-3-0-and-ef-6-3-general-availability/) / [Критические изменения](xref:core/what-is-new/ef-core-3.x/breaking-changes)
| ~~[EF Core 2.2](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore/2.2.6)~~ | .NET Standard 2.0 | Срок действия истек 23 декабря 2019 г. | [Объявление](https://devblogs.microsoft.com/dotnet/announcing-entity-framework-core-2-2/)
| [EF Core 2.1](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore/2.1.14) | .NET Standard 2.0 | 21 августа 2021 г. (LTS) | [Объявление](https://devblogs.microsoft.com/dotnet/announcing-entity-framework-core-2-1/)
| ~~[EF Core 2.0](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore/2.0.3)~~ | .NET Standard 2.0 | Срок действия истек 1 октября 2018 г. | [Объявление](https://devblogs.microsoft.com/dotnet/announcing-entity-framework-core-2-0/)
| ~~[EF Core 1.1](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore/1.1.6)~~ | .NET Standard 1.3 | Срок действия истек 27 июня 2019 г. | [Объявление](https://devblogs.microsoft.com/dotnet/announcing-entity-framework-core-1-1/)
| ~~[EF Core 1.0](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore/1.0.6)~~ | .NET Standard 1.3 | Срок действия истек 27 июня 2019 г. | [Объявление](https://devblogs.microsoft.com/dotnet/entity-framework-core-1-0-0-available/)

Сведения о конкретных платформах, поддерживаемых каждым выпуском EF Core, см. в разделе [Поддерживаемые платформы](xref:core/miscellaneous/platforms).

Сведения о сроках действия поддержки и выпусках с долгосрочной поддержкой (LTS) см. в [Политике поддержки .NET](https://dotnet.microsoft.com/platform/support/policy/dotnet-core).

## <a name="guidance-on-updating-to-new-releases"></a>Руководство по обновлению до новых выпусков

* В поддерживаемых выпусках исправлены проблемы безопасности и другие критические ошибки. Всегда используйте последнее исправление данного выпуска. Например, для EF Core 2.1 используйте версию 2.1.x для максимального доступного значения "x".
* Обновления основного номера версии (например, с EF Core 2 до EF Core 3) часто содержат критические изменения. Рекомендуется тщательное тестирование при обновлении основных версий. Используйте ссылки на критические изменения выше, чтобы получить рекомендации по работе с критическими изменениями.
* Обновления дополнительного номера версии обычно не содержат критических изменений. Однако тщательное тестирование по-прежнему рекомендуется, так как новые функции могут привести к регрессиям.

## <a name="release-planning-and-schedules"></a>Процесс планирования выпусков

Выпуски EF Core согласованы с [расписанием поставки .NET Core](https://github.com/dotnet/core/blob/main/roadmap.md).

Выпуски исправлений обычно предоставляются ежемесячно, но имеют долгое время выполнения.
Мы работаем над улучшением этой ситуации.

См. сведения о том, как мы определяем, что именно предоставлять в каждом выпуске, в описании [процесса планирования выпуска](xref:core/what-is-new/release-planning).
Как правило, планирование предусматривает только определение основных или дополнительных выпусков.

## <a name="ef-core-60"></a>EF Core 6.0

Следующий запланированный стабильный выпуск — **EF Core 6.0**, выход которого запланирован на **ноябрь 2021 г**.

[Общий план для EF Core 6.0](xref:core/what-is-new/ef-core-6.0/plan) был создан в соответствии с задокументированным [процессом планирования выпусков](xref:core/what-is-new/release-planning).

Нам важно ваше мнение о планировании.
Лучший способ указать важность проблемы — проголосовать за нее на GitHub (👍).
Эти данные будут учитываться в процессе планирования для следующего выпуска.

### <a name="get-it-now"></a>Получить

Пакеты EF Core 6.0 **теперь доступны** как

* [ежедневные сборки](https://github.com/dotnet/aspnetcore/blob/master/docs/DailyBuilds.md).
  * Содержат все последние функции и исправления ошибок. В целом очень стабильны. Для каждой сборки выполнено более 75 000 тестов.

Кроме того, по мере продвижения часто выпускаемые предварительные версии будут отправляться в NuGet. Обратите внимание, что предварительные версии отстают от ежедневных сборок, но тестируются для работы с соответствующими предварительными версиями ASP.NET Core и .NET Core.

Использование предварительных версий и ежедневных сборок — это отличный способ найти проблемы и отправить отзыв как можно скорее.
Чем раньше мы получим обратную связь, тем больше вероятность, что мы примем меры до следующего официального выпуска.
