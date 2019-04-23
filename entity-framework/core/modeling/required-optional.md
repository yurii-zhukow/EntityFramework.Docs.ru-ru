---
title: Обязательные и необязательные свойства — EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: ddaa0a54-9f43-4c34-aae3-f95c96c69842
uid: core/modeling/required-optional
ms.openlocfilehash: 564d9e62e2ed4f1a52b569630ed4994529e31dc1
ms.sourcegitcommit: 87fcaba46535aa351db4bdb1231bd14b40e459b9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/22/2019
ms.locfileid: "59929814"
---
# <a name="required-and-optional-properties"></a>Обязательные и необязательные свойства

Свойство считается необязательным, если для него допустимо значение `null`. Если `null` не является допустимым значением свойства, свойство считается обязательным.

## <a name="conventions"></a>Соглашения

По соглашению, свойство, для которого CLR-тип может иметь значение null, будет настроено как необязательное (`string`, `int?`, `byte[]`и т. д.). Свойства, для которых CLR-тип не может иметь значение null, будут настроены как обязательные (`int`, `decimal`, `bool`и т. д.).

> [!NOTE]  
> Свойство, для которого CLR-тип не может иметь значение null, не может быть настроено как необязательное. Такое свойство всегда будет расцениваться Entity Framework как обязательное.

## <a name="data-annotations"></a>Заметки к данным

Заметки к данным можно использовать для указания, что свойство является обязательным.

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Samples/Required.cs?highlight=14)]

## <a name="fluent-api"></a>Fluent API

Fluent API можно использовать для указания того, что свойство является обязательным.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Required.cs?highlight=11-13)]

