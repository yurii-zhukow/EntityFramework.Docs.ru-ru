---
title: Сопоставление столбцов — EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 05a47de9-1078-488e-a823-b516a4208f33
uid: core/modeling/relational/columns
ms.openlocfilehash: 22fcafbfcf9daf765c94e6ca9c42d7770d3e7d07
ms.sourcegitcommit: 87fcaba46535aa351db4bdb1231bd14b40e459b9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/22/2019
ms.locfileid: "59929866"
---
# <a name="column-mapping"></a>Сопоставление столбцов

> [!NOTE]  
> Описанная в этом разделе конфигурации применяется к реляционным базам данных в общем случае. Показанные здесь методы расширения будут доступны после установки поставщика реляционной базы данных (посредством общего *пакета Microsoft.EntityFrameworkCore.Relational*).

Сопоставление столбцов определяет, какие данные столбца следует оттуда и сохранены в базе данных.

## <a name="conventions"></a>Соглашения

По соглашению каждое свойство будет настраиваться для сопоставления со столбцом с тем же именем, как свойство.

## <a name="data-annotations"></a>Заметки к данным

Заметки к данным можно использовать для настройки столбца, с которым сопоставлено свойство.

[!code-csharp[Main](../../../../samples/core/Modeling/DataAnnotations/Samples/Relational/Column.cs?highlight=13)]

## <a name="fluent-api"></a>Текучий API

Fluent API можно использовать для настройки столбца, с которым сопоставлено свойство.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Samples/Relational/Column.cs?highlight=11-13)]
