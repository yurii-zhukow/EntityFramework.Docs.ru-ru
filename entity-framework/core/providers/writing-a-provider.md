---
title: Создание поставщика базы данных — EF Core
description: Сведения о создании нового поставщика Entity Framework Core
author: ajcvickers
ms.date: 10/27/2016
uid: core/providers/writing-a-provider
ms.openlocfilehash: e66c5b94d826e35bb5148d57897a1081de4e9736
ms.sourcegitcommit: 032a1767d7a6e42052a005f660b80372c6521e7e
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/12/2021
ms.locfileid: "98128424"
---
# <a name="writing-a-database-provider"></a>Создание поставщика баз данных

Дополнительные сведения о создании Entity Framework Core поставщика базы данных см. [в разделе Создание поставщика EF Core](https://blog.oneunicorn.com/2016/11/11/so-you-want-to-write-an-ef-core-provider/) с помощью [Артур Виккерс](https://github.com/ajcvickers).

> [!NOTE]
> Эти записи не были обновлены с момента EF Core 1,1 и были значительными изменениями с момента этого времени.
[Проблема 681](https://github.com/dotnet/EntityFramework.Docs/issues/681) отслеживает обновления для этой документации.

EF Core CodeBase является открытым исходным кодом и содержит несколько поставщиков баз данных, которые можно использовать в качестве ссылки. Исходный код можно найти по адресу <https://github.com/dotnet/efcore> . Также может быть полезно взглянуть на код для часто используемых сторонних поставщиков, таких как [Npgsql](https://github.com/npgsql/Npgsql.EntityFrameworkCore.PostgreSQL), [Pomelo MySQL](https://github.com/PomeloFoundation/Pomelo.EntityFrameworkCore.MySql)и [SQL Server Compact](https://github.com/ErikEJ/EntityFramework.SqlServerCompact). В частности, эти проекты настроены на расширение и выполнение функциональных тестов, публикуемых в NuGet. Этот тип установки настоятельно рекомендуется.

## <a name="keeping-up-to-date-with-provider-changes"></a>Актуальность изменений поставщика

Начиная с работы после выпуска 2,1 мы создали [Журнал изменений](xref:core/providers/provider-log) , которые могут потребовать внесения соответствующих изменений в код поставщика. Это предназначено для помощи при обновлении существующего поставщика для работы с новой версией EF Core.

До 2,1 мы использовали [`providers-beware`](https://github.com/dotnet/efcore/labels/providers-beware) [`providers-fyi`](https://github.com/dotnet/efcore/labels/providers-fyi) метки и в наших проблемах GitHub и запросах на вытягивание для аналогичной цели. Мы континиуе с этими метками о проблемах, чтобы указать, какие рабочие элементы в данном выпуске могут также требовать выполнения работы в поставщиках. `providers-beware`Метка обычно означает, что реализация рабочего элемента может привести к нарушению работы поставщиков, а `providers-fyi` Метка обычно означает, что поставщики не будут разорваны, но может потребоваться изменить код, например, чтобы включить новые функциональные возможности.

## <a name="suggested-naming-of-third-party-providers"></a>Предлагаемое именование сторонних поставщиков

Мы рекомендуем использовать следующее именование для пакетов NuGet. Это согласуется с именами пакетов, доставленных командой EF Core.

`<Optional project/company name>.EntityFrameworkCore.<Database engine name>`

Пример:

* `Microsoft.EntityFrameworkCore.SqlServer`
* `Npgsql.EntityFrameworkCore.PostgreSQL`
* `EntityFrameworkCore.SqlServerCompact40`
