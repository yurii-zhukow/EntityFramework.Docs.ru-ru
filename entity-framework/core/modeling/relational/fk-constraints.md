---
title: Ограничения внешнего ключа-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: dbaf4bac-1fd5-46c0-ac57-64d7153bc574
uid: core/modeling/relational/fk-constraints
ms.openlocfilehash: df739f01a799ec8edad4cf44d8cf50edf292992f
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/06/2019
ms.locfileid: "73655999"
---
# <a name="foreign-key-constraints"></a>Ограничения внешнего ключа

> [!NOTE]  
> Описанная в этом разделе конфигурации применяется к реляционным базам данных в общем случае. Показанные здесь методы расширения будут доступны после установки поставщика реляционной базы данных (посредством общего *пакета Microsoft.EntityFrameworkCore.Relational*).

Ограничение внешнего ключа вводится для каждой связи в модели.

## <a name="conventions"></a>Соглашения

По соглашению ограничения внешнего ключа именуются `FK_<dependent type name>_<principal type name>_<foreign key property name>`. Для составных внешних ключей `<foreign key property name>` преобразуется в список имен свойств внешнего ключа с разделителями подчеркивания.

## <a name="data-annotations"></a>Заметки к данным

Имена ограничений внешнего ключа нельзя настроить с помощью заметок к данным.

## <a name="fluent-api"></a>Текучий API

С помощью API-интерфейса Fluent можно настроить имя ограничения внешнего ключа для связи.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/RelationshipConstraintName.cs?name=Constraint&highlight=12)]
