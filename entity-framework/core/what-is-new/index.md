---
title: Выпуски и планирование EF Core
author: ajcvickers
ms.date: 01/14/2020
ms.assetid: C21F89EE-FB08-4ED9-A2A0-76CB7656E6E4
uid: core/what-is-new/index
ms.openlocfilehash: 8d74c24021fd62c5c5d944eaf3973b344fdb1e9c
ms.sourcegitcommit: f2a38c086291699422d8b28a72d9611d1b24ad0d
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/16/2020
ms.locfileid: "76124409"
---
# <a name="ef-core-releases-and-planning"></a>Выпуски и планирование EF Core

## <a name="stable-releases"></a>Стабильные выпуски

| Выпуск | Целевая платформа | Поддержка до | Ссылки
|:--------|------------------|-----------------|------
| [EF Core 3.1](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore/3.1.1) | .NET Standard 2.0 | 3 декабря 2022 г. (LTS) | [Объявление](https://devblogs.microsoft.com/dotnet/announcing-entity-framework-core-3-1-and-entity-framework-6-4/)
| [EF Core 3.0](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore/3.0.1) | .NET Standard 2.1 | 3 марта 2020 г. | [Объявление](https://devblogs.microsoft.com/dotnet/announcing-ef-core-3-0-and-ef-6-3-general-availability/) / [Критические изменения](ef-core-3.0/breaking-changes.md)
| ~~[EF Core 2.2](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore/2.2.6)~~ | .NET Standard 2.0 | Срок действия истек 23 декабря 2019 г. | [Объявление](https://devblogs.microsoft.com/dotnet/announcing-entity-framework-core-2-2/)
| [EF Core 2.1](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore/2.1.14) | .NET Standard 2.0 | 21 августа 2021 г. (LTS) | [Объявление](https://devblogs.microsoft.com/dotnet/announcing-entity-framework-core-2-1/)
| ~~[EF Core 2.0](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore/2.0.3)~~ | .NET Standard 2.0 | Срок действия истек 1 октября 2018 г. | [Объявление](https://devblogs.microsoft.com/dotnet/announcing-entity-framework-core-2-0/)
| ~~[EF Core 1.1](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore/1.1.6)~~ | .NET Standard 1.3 | Срок действия истек 27 июня 2019 г. | [Объявление](https://devblogs.microsoft.com/dotnet/announcing-entity-framework-core-1-1/)
| ~~[EF Core 1.0](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore/1.0.6)~~ | .NET Standard 1.3 | Срок действия истек 27 июня 2019 г. | [Объявление](https://devblogs.microsoft.com/dotnet/entity-framework-core-1-0-0-available/)

Сведения о конкретных платформах, поддерживаемых каждым выпуском EF Core, см. в разделе [Поддерживаемые платформы](../platforms/index.md).

Сведения о сроках действия поддержки и выпусках с долгосрочной поддержкой (LTS) см. в [Политике поддержки .NET](https://dotnet.microsoft.com/platform/support/policy/dotnet-core).

## <a name="guidance-on-updating-to-new-releases"></a>Руководство по обновлению до новых выпусков

* В поддерживаемых выпусках исправлены проблемы безопасности и другие критические ошибки. Всегда используйте последнее исправление данного выпуска. Например, для EF Core 2.1 используйте 2.1.14.
* Обновления основного номера версии (например, с EF Core 2 до EF Core 3) часто содержат критические изменения. Рекомендуется тщательное тестирование при обновлении основных версий. Используйте ссылки на критические изменения выше, чтобы получить рекомендации по работе с критическими изменениями.
* Обновления дополнительного номера версии обычно не содержат критических изменений. Однако тщательное тестирование по-прежнему рекомендуется, так как новые функции могут привести к регрессиям.

## <a name="ef-core-50"></a>EF Core 5.0

Выпуски EF Core согласованы с [расписанием поставки .NET Core](https://github.com/dotnet/core/blob/master/roadmap.md). Следующий запланированный стабильный выпуск — **EF Core 5.0**, ожидаемый в ноябре 2020 года.

[Общий план для EF Core 5.0](ef-core-5.0/plan.md) был создан в соответствии с задокументированным [процессом планирования выпусков](release-planning.md).

Нам важно ваше мнение о планировании. Лучший способ указать важность проблемы — проголосовать за нее на GitHub. Эти данные будут учитываться в процессе планирования для следующего выпуска.

### <a name="get-it-now"></a>Получить сейчас

Пакеты EF Core 5.0 **теперь доступны** как [ежедневные сборки](https://github.com/aspnet/AspNetCore/blob/master/docs/DailyBuilds.md). 

Использование ежедневных сборок — это отличный способ найти проблемы и отправить отзыв как можно скорее. Чем раньше мы получим обратную связь, тем больше вероятность, что мы примем меры до следующего официального выпуска. Мы усердно работаем над тем, чтобы поддерживать ежедневные сборки в отличном состоянии, выполняя свыше 55 000 тестов на платформу для каждой сборки.

Пакеты предварительной версии будут поставляться в NuGet позднее в этом году.
