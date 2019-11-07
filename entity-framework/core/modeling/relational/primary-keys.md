---
title: Первичные ключи — EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: c78f8f42-564a-45a4-aca7-3ede9f7ed2bc
uid: core/modeling/relational/primary-keys
ms.openlocfilehash: c195cc37859ea0689b5c6fbb84051564cf96c83a
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/06/2019
ms.locfileid: "73656052"
---
# <a name="primary-keys"></a>Первичные ключи

> [!NOTE]  
> Описанная в этом разделе конфигурации применяется к реляционным базам данных в общем случае. Показанные здесь методы расширения будут доступны после установки поставщика реляционной базы данных (посредством общего *пакета Microsoft.EntityFrameworkCore.Relational*).

Ограничение первичного ключа вводится для ключа каждого типа сущности.

## <a name="conventions"></a>Соглашения

По соглашению первичный ключ в базе данных будет называться `PK_<type name>`.

## <a name="data-annotations"></a>Заметки к данным

Никакие аспекты реляционной базы данных, относящиеся к первичному ключу, не могут быть настроены с помощью заметок к данным.

## <a name="fluent-api"></a>Текучий API

API-интерфейс Fluent можно использовать для настройки имени ограничения первичного ключа в базе данных.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/KeyName.cs?name=KeyName&highlight=9)]
