---
title: Схема по умолчанию — EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: e6e58473-9f5e-4a1f-ac0f-b87d2cbb667e
uid: core/modeling/relational/default-schema
ms.openlocfilehash: 1579fed007997aa4cf49b4c1290aee86c81c0000
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/06/2019
ms.locfileid: "73655971"
---
# <a name="default-schema"></a>Схема по умолчанию

> [!NOTE]  
> Описанная в этом разделе конфигурации применяется к реляционным базам данных в общем случае. Показанные здесь методы расширения будут доступны после установки поставщика реляционной базы данных (посредством общего *пакета Microsoft.EntityFrameworkCore.Relational*).

Схема по умолчанию — это схема базы данных, в которой будут создаваться объекты, если схема не настроена явно для этого объекта.

## <a name="conventions"></a>Соглашения

По соглашению поставщик базы данных будет выбирать наиболее подходящую схему по умолчанию. Например, Microsoft SQL Server будет использовать схему `dbo`, а SQLite не будет использовать схему (поскольку схемы не поддерживаются в SQLite).

## <a name="data-annotations"></a>Заметки к данным

Невозможно задать схему по умолчанию с помощью заметок к данным.

## <a name="fluent-api"></a>Текучий API

Для указания схемы по умолчанию можно использовать API Fluent.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/DefaultSchema.cs?name=DefaultSchema&highlight=7)]
