---
title: Разработка поставщика базы данных — EF Core
author: anmiller
ms.date: 10/27/2016
ms.assetid: 1165e2ec-e421-43fc-92ab-d92f9ab3c494
uid: core/providers/writing-a-provider
ms.openlocfilehash: c7130b0d104cd26584d298da98eb3e7080ee7f3c
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/27/2018
ms.locfileid: "42993670"
---
# <a name="writing-a-database-provider"></a>Разработка поставщика базы данных

Сведения о написании поставщика базы данных Entity Framework Core, см. в разделе [, требуется написать поставщика EF Core](https://blog.oneunicorn.com/2016/11/11/so-you-want-to-write-an-ef-core-provider/) по [Vickers Артур](https://github.com/ajcvickers).

> [!NOTE]
> Эти сообщения не были обновлены с момента EF Core 1.1 и с этого момента были сделаны существенные изменения [681 проблема](https://github.com/aspnet/EntityFramework.Docs/issues/681) отслеживает изменения в эту документацию.

База кода EF Core имеет открытый исходный код и содержит несколько поставщиков базы данных, которые могут использоваться в качестве ссылки. Можно найти исходный код в https://github.com/aspnet/EntityFrameworkCore. Также это может быть полезно посмотреть на код для часто используемых сторонних поставщиков, таких как [Npgsql](https://github.com/npgsql/Npgsql.EntityFrameworkCore.PostgreSQL), [Pomelo MySQL](https://github.com/PomeloFoundation/Pomelo.EntityFrameworkCore.MySql), и [SQL Server Compact](https://github.com/ErikEJ/EntityFramework.SqlServerCompact). В частности эти проекты, программу установки, чтобы расширить и запуск функциональных тестов, которые мы публикуем в NuGet. Настоятельно рекомендуется такого типа установки.

## <a name="keeping-up-to-date-with-provider-changes"></a>Обновление с изменениями поставщика

Начиная с рабочих после 2.1 выпуска, мы создали [журнала изменений](provider-log.md) , может потребоваться соответствующие изменения в код поставщика. Это призвано помочь при обновлении существующего поставщика для работы с новой версии EF Core.

Прежде чем 2.1, мы использовали [ `providers-beware` ](https://github.com/aspnet/EntityFrameworkCore/labels/providers-beware) и [ `providers-fyi` ](https://github.com/aspnet/EntityFrameworkCore/labels/providers-fyi) меток на наши вопросы GitHub и запросов на Вытягивание с одинаковой цели. Мы выполним continiue использование этих меток по проблемам, для предоставления значение, указывающее какие рабочие элементы в определенном выпуске также может потребоваться должны быть выполнены в поставщиках. Объект `providers-beware` метки обычно означает, что реализации рабочего элемента может нарушить работу поставщиков, хотя `providers-fyi` метки обычно означает, что поставщики не будет нарушена, но код может понадобиться изменить в любом случае, например, чтобы задействовать новые функциональные возможности.

## <a name="suggested-naming-of-third-party-providers"></a>Предлагаемые именования сторонних поставщиков

Мы советуем использовать следующие имена пакетов NuGet. Это согласуется с именами пакетов, предоставленные группе EF Core.

`<Optional project/company name>.EntityFrameworkCore.<Database engine name>`

Пример:
* `Microsoft.EntityFrameworkCore.SqlServer`
* `Npgsql.EntityFrameworkCore.PostgreSQL`
* `EntityFrameworkCore.SqlServerCompact40`
