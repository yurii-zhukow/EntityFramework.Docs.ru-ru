---
title: Альтернативные ключи (ограничения Unique) - EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 3d419dcf-2b5d-467c-b408-ea03d830721a
ms.technology: entity-framework-core
uid: core/modeling/relational/unique-constraints
ms.openlocfilehash: 1b7e2bef6ede95f8c27211ba00dcc6b97cccde9b
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/27/2017
ms.locfileid: "26052794"
---
# <a name="alternate-keys-unique-constraints"></a>Альтернативные ключи (ограничения Unique)

> [!NOTE]  
> В этом разделе конфигурации применяется для реляционных баз данных в целом. Методы расширения, показанный здесь будут доступны после установки поставщика реляционной базы данных (из-за общей *Microsoft.EntityFrameworkCore.Relational* пакета).

Ограничение уникальности введен для каждого альтернативный ключ в модели.

## <a name="conventions"></a>Соглашения

По соглашению, индекс и ограничения, введенные для дополнительный ключ будет называться `AK_<type name>_<property name>`. Для составных ключей альтернативный `<property name>` становится подчеркивания запятыми список имен свойств.

## <a name="data-annotations"></a>Заметки к данным

Ограничения UNIQUE не следует задавать с помощью заметок к данным.

## <a name="fluent-api"></a>Fluent API

Чтобы задать имя индекса и ограничения для дополнительный ключ можно использовать Fluent API.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Samples/Relational/AlternateKeyName.cs?name=Model&highlight=9)]
