---
title: Индексы — EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 4581e7ba-5e7f-452c-9937-0aaf790ba10a
uid: core/modeling/relational/indexes
ms.openlocfilehash: 605b30ce710d9034deab97f695496ec66a576565
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/27/2018
ms.locfileid: "42993221"
---
# <a name="indexes"></a>Индексы

> [!NOTE]  
> Описанная в этом разделе конфигурации применяется к реляционным базам данных в общем случае. Показанные здесь методы расширения будут доступны после установки поставщика реляционной базы данных (посредством общего *пакета Microsoft.EntityFrameworkCore.Relational*).

Совпадает с концепцией индекса в ядро Entity Framework сопоставляет индекса в реляционной базе данных.

## <a name="conventions"></a>Соглашения

По соглашению, называются индексы `IX_<type name>_<property name>`. Для индексов являются составные индексы `<property name>` становится список имен свойств, разделенных символом подчеркивания.

## <a name="data-annotations"></a>Заметки к данным

Не индексов можно настроить с помощью заметок к данным.

## <a name="fluent-api"></a>Текучий API

Fluent API можно использовать для настройки имени индекса.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Samples/Relational/IndexName.cs?name=Model&highlight=9)]

Можно также указать фильтр.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Samples/Relational/IndexFilter.cs?name=Model&highlight=9)]

При использовании поставщика SQL Server EF добавляет фильтрации «IS NOT NULL» для всех столбцов допускает значения NULL, которые являются частью уникального индекса. Чтобы переопределить это соглашение, можно указать `null` значение.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Samples/Relational/IndexNoFilter.cs?name=Model&highlight=10)]
