---
title: Ограничения внешнего ключа-EF Core
author: AndriySvyryd
ms.date: 11/21/2019
uid: core/modeling/relational/fk-constraints
ms.openlocfilehash: 2855137adf2ba3c9edaabd15a05f7a209f00f685
ms.sourcegitcommit: 7a709ce4f77134782393aa802df5ab2718714479
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/04/2019
ms.locfileid: "74824589"
---
# <a name="foreign-key-constraints"></a>Ограничения внешнего ключа

> [!NOTE]  
> Описанная в этом разделе конфигурации применяется к реляционным базам данных в общем случае. Показанные здесь методы расширения будут доступны после установки поставщика реляционной базы данных (посредством общего *пакета Microsoft.EntityFrameworkCore.Relational*).

Ограничение внешнего ключа вводится для каждой связи в модели.

## <a name="conventions"></a>Обозначения

По соглашению ограничения внешнего ключа именуются `FK_<dependent type name>_<principal type name>_<foreign key property name>`. Для составных внешних ключей `<foreign key property name>` преобразуется в список имен свойств внешнего ключа с разделителями подчеркивания.

## <a name="data-annotations"></a>Заметки к данным

Имена ограничений внешнего ключа нельзя настроить с помощью заметок к данным.

## <a name="fluent-api"></a>Текучий API

С помощью API-интерфейса Fluent можно настроить имя ограничения внешнего ключа для связи.

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/RelationshipConstraintName.cs?name=Constraint&highlight=12)]
