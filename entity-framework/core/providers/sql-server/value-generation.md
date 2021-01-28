---
title: Поставщик базы данных Microsoft SQL Server — создание значения EF Core
description: Шаблоны создания значений, характерные для поставщика базы данных SQL Server Entity Framework Core
author: roji
ms.date: 1/10/2020
uid: core/providers/sql-server/value-generation
ms.openlocfilehash: 90608f254a3d20e3c36586ae8325e0a982a7fa27
ms.sourcegitcommit: 7700840119b1639275f3b64836e7abb59103f2e7
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/28/2021
ms.locfileid: "98987142"
---
# <a name="sql-server-value-generation"></a>Создание значения SQL Server

На этой странице подробно описана конфигурация и шаблоны создания значений, характерные для поставщика SQL Server. Рекомендуется сначала прочитать [страницу Общие сведения о создании значения](xref:core/modeling/generated-properties).

## <a name="identity-columns"></a>Столбцы IDENTITY

По соглашению числовые столбцы, для которых настроены значения для добавления, настраиваются как [SQL Server столбцы идентификаторов](https://docs.microsoft.com/sql/t-sql/statements/create-table-transact-sql-identity-property).

### <a name="seed-and-increment"></a>Начальное значение и приращение

По умолчанию столбцы ИДЕНТИФИКАТОРов начинаются с 1 (начальное значение) и увеличиваются на 1 каждый раз, когда добавляется строка (шаг приращения). Вы можете настроить другое начальное значение и шаг приращения следующим образом:

[!code-csharp[Main](../../../../samples/core/SqlServer/ValueGeneration/IdentityOptionsContext.cs?name=IdentityOptions&highlight=5)]

> [!NOTE]
> Возможность настройки начального значения и приращения идентификатора появилась в EF Core 3,0.

### <a name="inserting-explicit-values-into-identity-columns"></a>Вставка явных значений в столбцы ИДЕНТИФИКАТОРов

По умолчанию SQL Server не допускает вставку явных значений в столбцы ИДЕНТИФИКАТОРов. Для этого необходимо вручную включить `IDENTITY_INSERT` перед вызовом `SaveChanges()` , как показано ниже.

[!code-csharp[Main](../../../../samples/core/SqlServer/ValueGeneration/ExplicitIdentityValues.cs?name=ExplicitIdentityValues)]

> [!NOTE]
> В нашем журнале заказов есть [запрос на функцию](https://github.com/aspnet/EntityFramework/issues/703), которая позволяет сделать это автоматически в пределах поставщика SQL Server.

## <a name="sequences"></a>Последовательности

В качестве альтернативы столбцам ИДЕНТИФИКАТОРов можно использовать стандартные последовательности. Это может быть полезно в различных сценариях. Например, может потребоваться, чтобы несколько столбцов рисулись значения по умолчанию из одной последовательности.

SQL Server позволяет создавать последовательности и использовать их в качестве подробных сведений [на странице "Общие" в последовательностях](xref:core/modeling/sequences). Вы можете настроить свойства для использования последовательностей с помощью `HasDefaultValueSql()` .
