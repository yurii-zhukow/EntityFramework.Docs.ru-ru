---
title: "Базовый EF - ограничения - поставщик базы данных SQLite"
author: rowanmiller
ms.author: divega
ms.date: 04/09/2017
ms.assetid: 94ab4800-c460-4caa-a5e8-acdfee6e6ce2
ms.technology: entity-framework-core
uid: core/providers/sqlite/limitations
ms.openlocfilehash: 08a4b8c26a3678491d412b333a7415cb45d4231f
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/27/2017
---
# <a name="sqlite-ef-core-database-provider-limitations"></a>Ограничения функций поставщика SQLite EF ядра базы данных

Поставщик SQLite имеет ряд ограничений миграции. Большинство этих ограничений представляют собой результат применения ограничения в компонент database engine SQLite и не относятся к EF.

## <a name="modeling-limitations"></a>Ограничения моделирования

Общей библиотекой реляционных (совместно используемые поставщиками реляционной базы данных Entity Framework) определяет интерфейсы API для моделирования, основные понятия, которые являются общими для большинства ядра реляционной базы данных. Несколько из этих концепций не поддерживаются поставщиком SQLite.

* Схемы
* Последовательности

## <a name="migrations-limitations"></a>Ограничения миграции

Ядро базы данных SQLite поддерживает ряд операций схемы, которые поддерживаются большинством других реляционных баз данных. При попытке применить одну из неподдерживаемых операций в базу данных SQLite то `NotSupportedException` будет создано.

| Операция            | Поддерживается? |
| -------------------- | ---------- |
| AddColumn            | ✔          |
| AddForeignKey        | ✗          |
| AddPrimaryKey        | ✗          |
| AddUniqueConstraint  | ✗          |
| AlterColumn          | ✗          |
| CreateIndex          | ✔          |
| CreateTable          | ✔          |
| DropColumn           | ✗          |
| DropForeignKey       | ✗          |
| DropIndex            | ✔          |
| DropPrimaryKey       | ✗          |
| DropTable            | ✔          |
| DropUniqueConstraint | ✗          |
| RenameColumn         | ✗          |
| RenameIndex          | ✗          |
| RenameTable          | ✔          |

## <a name="migrations-limitations-workaround"></a>Обходной путь для ограничения миграции

Инструкции по решению можно выполнить некоторые из этих ограничений, написав код в миграции для выполнения таблицы вручную перестроить. Перестройка таблицы включает в себя переименование существующей таблицы, создав новую таблицу, копирование данных в новую таблицу и удалить старую таблицу. Необходимо будет использовать `Sql(string)` метод для выполнения некоторых из следующих действий.

В разделе [внесения других типов из таблицы изменений схемы](http://sqlite.org/lang_altertable.html#otheralter) в SQLite документации для получения дополнительных сведений.

В будущем EF поддерживает некоторые из этих операций, используя подход перестроения таблица на самом деле. Вы можете [отслеживать этот компонент на нашем сайте GitHub проекта](https://github.com/aspnet/EntityFramework/issues/329).
