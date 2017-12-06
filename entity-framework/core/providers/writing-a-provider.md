---
title: "Разработка поставщика базы данных - EF Core"
author: anmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 1165e2ec-e421-43fc-92ab-d92f9ab3c494
ms.technology: entity-framework-core
uid: core/providers/writing-a-provider
ms.openlocfilehash: 9d6d3748a9097b3b8eeee2a8a516c53f3b2afa58
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/27/2017
---
# <a name="writing-a-database-provider"></a>Разработка поставщика базы данных

Сведения о создании поставщика Entity Framework Core базы данных см. в разделе [, требуется написать поставщик EF Core](https://blog.oneunicorn.com/2016/11/11/so-you-want-to-write-an-ef-core-provider/) по [Vickers Артуром](https://github.com/ajcvickers).

База кода EF Core является открытым исходным кодом и содержит несколько поставщиков базы данных, которые могут использоваться в качестве ссылки. Исходный код можно найти в https://github.com/aspnet/EntityFramework.

## <a name="the-providers-beware-label"></a>Поставщики Берегитесь метки

После начала работы над поставщика следить за [ `providers-beware` ](https://github.com/aspnet/EntityFramework/labels/providers-beware) метка на нашем GitHub проблемы и запросы на получение. Мы используем эту метку для выявления изменений, которые могут влиять на модули записи поставщика.

## <a name="suggested-naming-of-third-party-providers"></a>Предлагаемые именования сторонних поставщиков

Мы советуем использовать с помощью следующих именования для пакетов NuGet. Это согласуется с именами пакетов, предоставляемых EF основная группа.

`<Optional project/company name>.EntityFrameworkCore.<Database engine name>`

Например:
* `Microsoft.EntityFrameworkCore.SqlServer`
* `Npgsql.EntityFrameworkCore.PostgreSQL`
* `EntityFrameworkCore.SqlServerCompact40`
