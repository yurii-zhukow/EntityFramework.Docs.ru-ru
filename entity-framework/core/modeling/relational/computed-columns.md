---
title: Вычисленные столбцы — EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: e9d81f06-805d-45c9-97c2-3546df654829
uid: core/modeling/relational/computed-columns
ms.openlocfilehash: 4e92ed6d785f3b7bdf54015101bdcddb9e4e0e1c
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/06/2019
ms.locfileid: "73655918"
---
# <a name="computed-columns"></a>Вычисляемые столбцы

> [!NOTE]  
> Описанная в этом разделе конфигурации применяется к реляционным базам данных в общем случае. Показанные здесь методы расширения будут доступны после установки поставщика реляционной базы данных (посредством общего *пакета Microsoft.EntityFrameworkCore.Relational*).

Вычисляемый столбец — это столбец, значение которого вычисляется в базе данных. Вычисляемый столбец может использовать другие столбцы таблицы для вычисления ее значения.

## <a name="conventions"></a>Соглашения

По соглашению, вычисленные столбцы не создаются в модели.

## <a name="data-annotations"></a>Заметки к данным

Вычисленные столбцы не могут быть настроены с помощью заметок к данным.

## <a name="fluent-api"></a>Текучий API

API-интерфейс Fluent можно использовать для указания того, что свойство должно сопоставляться с вычисляемым столбцом.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/ComputedColumn.cs?name=ComputedColumn&highlight=9)]
