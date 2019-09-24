---
title: Включение & за исключением свойств EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: e9dff604-3469-4a05-8f9e-18ac281d82a9
uid: core/modeling/included-properties
ms.openlocfilehash: cd111af891ef0bbaccf515eed0c1991f105bd362
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/23/2019
ms.locfileid: "71197422"
---
# <a name="including--excluding-properties"></a>Включение & за исключением свойств

Включение свойства в модель означает, что EF имеет метаданные об этом свойстве и будет пытаться считывать и записывать значения в базу данных.

## <a name="conventions"></a>Соглашения

По соглашению в модель будут включаться открытые свойства с методом считывания и методом задания.

## <a name="data-annotations"></a>Заметки к данным

Заметки к данным можно использовать для исключения свойства из модели.

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/IgnoreProperty.cs?highlight=17)]

## <a name="fluent-api"></a>Текучий API

API-интерфейс Fluent можно использовать для исключения свойства из модели.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/IgnoreProperty.cs?highlight=12,13)]
