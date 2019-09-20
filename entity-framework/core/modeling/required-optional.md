---
title: Обязательные/необязательные свойства — EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: ddaa0a54-9f43-4c34-aae3-f95c96c69842
uid: core/modeling/required-optional
ms.openlocfilehash: 7200cd2eeeba2f22365ef09b1f50edd077240130
ms.sourcegitcommit: cbaa6cc89bd71d5e0bcc891e55743f0e8ea3393b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/20/2019
ms.locfileid: "71149144"
---
# <a name="required-and-optional-properties"></a>Обязательные и необязательные свойства

Свойство считается необязательным, если для него допустимо значение `null`. Если `null` не является допустимым значением свойства, свойство считается обязательным.

## <a name="conventions"></a>Соглашения

По соглашению свойство, тип .NET которого может содержать значение null, будет настроен как необязательный `byte[]`(`string`, `int?`, и т. д.). Свойства, для которых CLR-тип не может иметь значение null, будут настроены как обязательные (`int`, `decimal`, `bool`и т. д.).

> [!NOTE]  
> Свойство, тип .NET которого не может содержать значение null, не может быть настроено как необязательный. Такое свойство всегда будет расцениваться Entity Framework как обязательное.

## <a name="data-annotations"></a>Заметки к данным

Можно использовать заметки к данным, чтобы указать, что свойство является обязательным.

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Samples/Required.cs?highlight=14)]

## <a name="fluent-api"></a>Текучий API

Fluent API можно использовать для указания того, что свойство является обязательным.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Required.cs?highlight=11-13)]

