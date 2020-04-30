---
title: Создание поставщика базы данных — EF Core
author: anmiller
ms.date: 10/27/2016
ms.assetid: 1165e2ec-e421-43fc-92ab-d92f9ab3c494
uid: core/providers/writing-a-provider
ms.openlocfilehash: 662c7e386bbdf0ff1e4e5051349d6a5c56a3df7b
ms.sourcegitcommit: 79e460f76b6664e1da5886d102bd97f651d2ffff
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/29/2020
ms.locfileid: "82538461"
---
# <a name="writing-a-database-provider"></a>Создание поставщика баз данных

Дополнительные сведения о создании Entity Framework Core поставщика базы данных см. [в разделе Создание поставщика EF Core](https://blog.oneunicorn.com/2016/11/11/so-you-want-to-write-an-ef-core-provider/) с помощью [Артур Виккерс](https://github.com/ajcvickers).

> [!NOTE]
> Эти записи не были обновлены с момента EF Core 1,1 и были значительными изменениями с момента этого времени.  
[Проблема 681](https://github.com/dotnet/EntityFramework.Docs/issues/681) отслеживает обновления для этой документации.

EF Core CodeBase является открытым исходным кодом и содержит несколько поставщиков баз данных, которые можно использовать в качестве ссылки. Исходный код можно найти по адресу <https://github.com/aspnet/EntityFrameworkCore>. Также может быть полезно взглянуть на код для часто используемых сторонних поставщиков, таких как [Npgsql](https://github.com/npgsql/Npgsql.EntityFrameworkCore.PostgreSQL), [Pomelo MySQL](https://github.com/PomeloFoundation/Pomelo.EntityFrameworkCore.MySql)и [SQL Server Compact](https://github.com/ErikEJ/EntityFramework.SqlServerCompact). В частности, эти проекты настроены для расширения и выполнения функциональных тестов, публикуемых в NuGet. Этот тип установки настоятельно рекомендуется.

## <a name="keeping-up-to-date-with-provider-changes"></a>Актуальность изменений поставщика

Начиная с работы после выпуска 2,1 мы создали [Журнал изменений](provider-log.md) , которые могут потребовать внесения соответствующих изменений в код поставщика. Это предназначено для помощи при обновлении существующего поставщика для работы с новой версией EF Core.

До 2,1 мы использовали метки [`providers-beware`](https://github.com/aspnet/EntityFrameworkCore/labels/providers-beware) и [`providers-fyi`](https://github.com/aspnet/EntityFrameworkCore/labels/providers-fyi) в наших проблемах GitHub и запросах на вытягивание для аналогичной цели. Мы континиуе с этими метками о проблемах, чтобы указать, какие рабочие элементы в данном выпуске могут также требовать выполнения работы в поставщиках. `providers-beware` Метка обычно означает, что реализация рабочего элемента может привести к нарушению работы поставщиков, а `providers-fyi` метка обычно означает, что поставщики не будут разорваны, но может потребоваться изменить код, например, чтобы включить новые функциональные возможности.

## <a name="suggested-naming-of-third-party-providers"></a>Предлагаемое именование сторонних поставщиков

Мы рекомендуем использовать следующее именование для пакетов NuGet. Это согласуется с именами пакетов, доставленных командой EF Core.

`<Optional project/company name>.EntityFrameworkCore.<Database engine name>`

Пример:

* `Microsoft.EntityFrameworkCore.SqlServer`
* `Npgsql.EntityFrameworkCore.PostgreSQL`
* `EntityFrameworkCore.SqlServerCompact40`
