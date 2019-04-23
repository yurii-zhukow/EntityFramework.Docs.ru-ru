---
title: Включение и исключение свойств — EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: e9dff604-3469-4a05-8f9e-18ac281d82a9
uid: core/modeling/included-properties
ms.openlocfilehash: 022534091bb48e491c8808791a401216a339d7b0
ms.sourcegitcommit: 87fcaba46535aa351db4bdb1231bd14b40e459b9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/22/2019
ms.locfileid: "59929829"
---
# <a name="including--excluding-properties"></a>Включение и исключение свойств

Включая свойства в модели означает, что EF содержит метаданные об этом свойстве и будет предпринята попытка чтения и записи значений в базе данных.

## <a name="conventions"></a>Соглашения

По соглашению будут включены свойства getter и setter в модели.

## <a name="data-annotations"></a>Заметки к данным

Чтобы исключить свойство из модели можно использовать заметки к данным.

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Samples/IgnoreProperty.cs?highlight=17)]

## <a name="fluent-api"></a>Текучий API

Fluent API можно использовать, чтобы исключить свойство из модели.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/IgnoreProperty.cs?highlight=12,13)]
