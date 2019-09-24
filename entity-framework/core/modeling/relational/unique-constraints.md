---
title: Альтернативные ключи (ограничения UNIQUE) — EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 3d419dcf-2b5d-467c-b408-ea03d830721a
uid: core/modeling/relational/unique-constraints
ms.openlocfilehash: 7afcb804aeeccbd5e07c228a8fd9850ca00a2919
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/23/2019
ms.locfileid: "71197616"
---
# <a name="alternate-keys-unique-constraints"></a>Альтернативные ключи (ограничения UNIQUE)

> [!NOTE]  
> Описанная в этом разделе конфигурации применяется к реляционным базам данных в общем случае. Показанные здесь методы расширения будут доступны после установки поставщика реляционной базы данных (посредством общего *пакета Microsoft.EntityFrameworkCore.Relational*).

Для каждого альтернативного ключа в модели вводится ограничение UNIQUE.

## <a name="conventions"></a>Соглашения

По соглашению индекс и ограничение, введенные для альтернативного ключа, будут называться `AK_<type name>_<property name>`. Для составных альтернативных `<property name>` ключей преобразуется в список имен свойств с разделителями подчеркивания.

## <a name="data-annotations"></a>Заметки к данным

Ограничения UNIQUE не могут быть настроены с помощью заметок к данным.

## <a name="fluent-api"></a>Текучий API

API-интерфейс Fluent можно использовать для настройки индекса и имени ограничения для альтернативного ключа.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/AlternateKeyName.cs?name=Model&highlight=9)]
