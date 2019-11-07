---
title: Значения по умолчанию — EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: e541366a-130f-47dd-9997-1b110a11febe
uid: core/modeling/relational/default-values
ms.openlocfilehash: b6ac283d551e2c6ee119f7de6933363b5d8793a1
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/06/2019
ms.locfileid: "73655902"
---
# <a name="default-values"></a>Значения по умолчанию

> [!NOTE]  
> Описанная в этом разделе конфигурации применяется к реляционным базам данных в общем случае. Показанные здесь методы расширения будут доступны после установки поставщика реляционной базы данных (посредством общего *пакета Microsoft.EntityFrameworkCore.Relational*).

По умолчанию столбец имеет значение, которое будет вставлено при вставке новой строки, но для столбца не указано значение.

## <a name="conventions"></a>Соглашения

По соглашению значение по умолчанию не настроено.

## <a name="data-annotations"></a>Заметки к данным

Нельзя задать значение по умолчанию с помощью заметок к данным.

## <a name="fluent-api"></a>Текучий API

API-интерфейс Fluent можно использовать для указания значения по умолчанию для свойства.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/DefaultValue.cs?name=DefaultValue&highlight=9)]

Можно также указать фрагмент SQL, используемый для вычисления значения по умолчанию.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/DefaultValueSql.cs?name=DefaultValueSql&highlight=9)]
