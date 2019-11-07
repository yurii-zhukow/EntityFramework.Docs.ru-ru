---
title: Ключи (основной) — EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 912ffef7-86a0-4cdc-a776-55f907459d20
uid: core/modeling/keys
ms.openlocfilehash: 66c64c389294e8e109a614a2bea8311932660dea
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/06/2019
ms.locfileid: "73655949"
---
# <a name="keys-primary"></a>Ключи (первичные)

Ключ служит в качестве первичного уникального идентификатора для каждого экземпляра сущности. При использовании реляционной базы данных эта схема сопоставляется с понятием *первичного ключа*. Можно также настроить уникальный идентификатор, который не является первичным ключом (Дополнительные сведения см. в разделе [альтернативные ключи](alternate-keys.md) ).

Для настройки или создания первичного ключа можно использовать один из следующих методов.

## <a name="conventions"></a>Соглашения

По соглашению свойство с именем `Id` или `<type name>Id` будет настроено в качестве ключа сущности.

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/KeyId.cs?name=KeyId&highlight=3)]

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/KeyTypeNameId.cs?name=KeyIdhighlight=3)]

## <a name="data-annotations"></a>Заметки к данным

Заметки к данным можно использовать для настройки одного свойства в качестве ключа сущности.

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/KeySingle.cs?highlight=13)]

## <a name="fluent-api"></a>Текучий API

API-интерфейс Fluent можно использовать для настройки одного свойства в качестве ключа сущности.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/KeySingle.cs?highlight=11,12)]

Можно также использовать API Fluent, чтобы настроить несколько свойств в качестве ключа сущности (называемого составным ключом). Составные ключи могут быть настроены только с помощью правил API Fluent, но не будут настраивать составной ключ, и вы не сможете использовать заметки к данным для их настройки.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/KeyComposite.cs?highlight=11,12)]
