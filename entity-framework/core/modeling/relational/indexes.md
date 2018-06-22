---
title: Индексы - EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 4581e7ba-5e7f-452c-9937-0aaf790ba10a
ms.technology: entity-framework-core
uid: core/modeling/relational/indexes
ms.openlocfilehash: f577fccfefc6908edf2ac47ae630323d7a9f5f2b
ms.sourcegitcommit: b2d94cebdc32edad4fecb07e53fece66437d1b04
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/28/2018
ms.locfileid: "29679003"
---
# <a name="indexes"></a>Индексы

> [!NOTE]  
> Описанная в этом разделе конфигурации применяется к реляционным базам данных в общем случае. Показанные здесь методы расширения будут доступны после установки поставщика реляционной базы данных (посредством общего *пакета Microsoft.EntityFrameworkCore.Relational*).

Индекс в реляционной базе данных сопоставляет совпадает с концепцией индекса в ядро платформы Entity Framework.

## <a name="conventions"></a>Соглашения

По соглашению с именем индексы `IX_<type name>_<property name>`. Составные индексы `<property name>` становится подчеркивания запятыми список имен свойств.

## <a name="data-annotations"></a>Заметки к данным

Индексы не следует задавать с помощью заметок к данным.

## <a name="fluent-api"></a>Текучий API

Чтобы настроить имя индекса, можно использовать Fluent API.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Samples/Relational/IndexName.cs?name=Model&highlight=9)]

Можно также указать фильтр.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Samples/Relational/IndexFilter.cs?name=Model&highlight=9)]

Когда с помощью поставщика SQL Server EF добавляет фильтрации «IS NOT NULL» для всех столбцов допускает значения NULL, которые являются частью уникального индекса. Чтобы переопределить это соглашение можно указать `null` значение.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Samples/Relational/IndexNoFilter.cs?name=Model&highlight=10)]
