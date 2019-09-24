---
title: Сопоставление столбцов — EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 05a47de9-1078-488e-a823-b516a4208f33
uid: core/modeling/relational/columns
ms.openlocfilehash: eaffc0cc1642f64edabeeef974f5f6de7a23b656
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/23/2019
ms.locfileid: "71197212"
---
# <a name="column-mapping"></a>Сопоставление столбцов

> [!NOTE]  
> Описанная в этом разделе конфигурации применяется к реляционным базам данных в общем случае. Показанные здесь методы расширения будут доступны после установки поставщика реляционной базы данных (посредством общего *пакета Microsoft.EntityFrameworkCore.Relational*).

Сопоставление столбцов определяет, какие данные столбца должны быть запрошены и сохранены в базе данных.

## <a name="conventions"></a>Соглашения

По соглашению каждое свойство будет настроено для соотнесения со столбцом с тем же именем, что и у свойства.

## <a name="data-annotations"></a>Заметки к данным

Заметки к данным можно использовать для настройки столбца, с которым сопоставлено свойство.

[!code-csharp[Main](../../../../samples/core/Modeling/DataAnnotations/Relational/Column.cs?highlight=13)]

## <a name="fluent-api"></a>Текучий API

API-интерфейс Fluent можно использовать для настройки столбца, с которым сопоставлено свойство.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/Column.cs?highlight=11-13)]
