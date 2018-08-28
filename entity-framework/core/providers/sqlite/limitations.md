---
title: База данных SQLite Provider - ограничения — EF Core
author: rowanmiller
ms.date: 04/09/2017
ms.assetid: 94ab4800-c460-4caa-a5e8-acdfee6e6ce2
uid: core/providers/sqlite/limitations
ms.openlocfilehash: 69c40fcd8b7ddb925728b1bad9992ad2a81e7540
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/27/2018
ms.locfileid: "42994668"
---
# <a name="sqlite-ef-core-database-provider-limitations"></a>Ограничения функций поставщика базы данных SQLite EF Core

Поставщик SQLite имеет ряд ограничений миграции. Большинство из этих ограничений представляют собой результат применения ограничения в ядро СУБД SQLite, а не только к EF.

## <a name="modeling-limitations"></a>Ограничения моделирования

Общей библиотекой реляционных (совместно используемые поставщиками реляционной базы данных Entity Framework) определяет API-интерфейсы для моделирования основные понятия, которые являются общими для большинства реляционных СУБД. Несколько из этих понятий, не поддерживаются поставщиком SQLite.

* Схемы
* Последовательности

## <a name="migrations-limitations"></a>Ограничения миграции

Ядро СУБД SQLite поддерживает ряд операций схемы, которые поддерживаются в большинстве других реляционных баз данных. При попытке применить одну из неподдерживаемых операций с базой данных SQLite то `NotSupportedException` будет создано.

| Операция            | Поддерживается? | Требуется версия |
|:---------------------|:-----------|:-----------------|
| AddColumn            | ✔          | 1.0              |
| AddForeignKey        | ✗          |                  |
| AddPrimaryKey        | ✗          |                  |
| AddUniqueConstraint  | ✗          |                  |
| AlterColumn          | ✗          |                  |
| CreateIndex          | ✔          | 1.0              |
| CreateTable          | ✔          | 1.0              |
| DropColumn           | ✗          |                  |
| DropForeignKey       | ✗          |                  |
| DropIndex            | ✔          | 1.0              |
| DropPrimaryKey       | ✗          |                  |
| DropTable            | ✔          | 1.0              |
| DropUniqueConstraint | ✗          |                  |
| RenameColumn         | ✗          |                  |
| RenameIndex          | ✔          | 2.1              |
| RenameTable          | ✔          | 1.0              |
| EnsureSchema         | ✔ (нет-op)  | 2.0              |
| DropSchema           | ✔ (нет-op)  | 2.0              |
| Insert               | ✔          | 2.0              |
| Обновление               | ✔          | 2.0              |
| Удаление               | ✔          | 2.0              |

## <a name="migrations-limitations-workaround"></a>Инструкции по решению ограничения миграции

Вы можете устранить некоторые из этих ограничений, написав код в миграции для выполнения таблицы вручную перестроить. Перестройка таблицы включает в себя переименование существующей таблицы, создание новой таблицы, копирование данных в новую таблицу и удалить старую таблицу. Необходимо будет использовать `Sql(string)` метод для выполнения некоторых из этих действий.

См. в разделе [внесения других типов из таблицы изменений схемы](http://sqlite.org/lang_altertable.html#otheralter) в SQLite документации для получения дополнительных сведений.

В будущем EF может поддерживать некоторые из этих операций, используя подход перестроение таблицы, на самом деле. Вы можете [отслеживать эту функцию на проект на портале GitHub](https://github.com/aspnet/EntityFrameworkCore/issues/329).
