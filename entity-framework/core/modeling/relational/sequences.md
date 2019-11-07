---
title: Последовательности — EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 94f81a92-3c72-4e14-912a-f99310374e42
uid: core/modeling/relational/sequences
ms.openlocfilehash: b810caaffa329bb5ad6f3486145d0ade9287eada
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/06/2019
ms.locfileid: "73656113"
---
# <a name="sequences"></a>Последовательности

> [!NOTE]  
> Описанная в этом разделе конфигурации применяется к реляционным базам данных в общем случае. Показанные здесь методы расширения будут доступны после установки поставщика реляционной базы данных (посредством общего *пакета Microsoft.EntityFrameworkCore.Relational*).

Последовательность формирует последовательные числовые значения в базе данных. Последовательности не связаны с определенной таблицей.

## <a name="conventions"></a>Соглашения

В соответствии с соглашением последовательности не вводятся в модель.

## <a name="data-annotations"></a>Заметки к данным

Вы не можете настроить последовательность с помощью заметок к данным.

## <a name="fluent-api"></a>Текучий API

Для создания последовательности в модели можно использовать API-интерфейс Fluent.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/Sequence.cs?name=Model&highlight=7)]

Можно также настроить дополнительный аспект последовательности, например ее схему, начальное значение и шаг приращения.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/SequenceConfigured.cs?name=Sequence&highlight=7,8,9)]

После того как последовательность введена, ее можно использовать для создания значений свойств в модели. Например, можно использовать [значения по умолчанию](default-values.md) для вставки следующего значения из последовательности.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/SequenceUsed.cs?name=Default&highlight=13)]
