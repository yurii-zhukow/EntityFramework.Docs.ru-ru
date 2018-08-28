---
title: Альтернативные ключи (ограничения уникальности) — EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 3d419dcf-2b5d-467c-b408-ea03d830721a
uid: core/modeling/relational/unique-constraints
ms.openlocfilehash: 7ec58ee31aac79e15329dc8542f37fd117772fbe
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/27/2018
ms.locfileid: "42994195"
---
# <a name="alternate-keys-unique-constraints"></a>Альтернативные ключи (ограничения уникальности)

> [!NOTE]  
> Описанная в этом разделе конфигурации применяется к реляционным базам данных в общем случае. Показанные здесь методы расширения будут доступны после установки поставщика реляционной базы данных (посредством общего *пакета Microsoft.EntityFrameworkCore.Relational*).

Для каждого альтернативного ключа в модели введено ограничение уникальности.

## <a name="conventions"></a>Соглашения

По соглашению, индекс и ограничения, введенные для альтернативного ключа будет называться `AK_<type name>_<property name>`. Для составных ключей альтернативный `<property name>` становится список имен свойств, разделенных символом подчеркивания.

## <a name="data-annotations"></a>Заметки к данным

Не ограничения UNIQUE можно настроить с помощью заметок к данным.

## <a name="fluent-api"></a>Текучий API

Fluent API можно использовать для настройки имени индексов и ограничений для альтернативного ключа.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Samples/Relational/AlternateKeyName.cs?name=Model&highlight=9)]
