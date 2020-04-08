---
title: Новые возможности в Core EF 1.1 (EF Core)
author: divega
ms.date: 10/27/2016
ms.assetid: C7FE8C85-445A-4F0C-97EC-CC3F7F1D6F5E
uid: core/what-is-new/ef-core-1.1
ms.openlocfilehash: d582712ed62443318f4b9e209511fb2a557d667e
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/07/2020
ms.locfileid: "78413591"
---
# <a name="new-features-in-ef-core-11"></a>Новые возможности в EF Core 1.1

## <a name="modeling"></a>Моделирование

### <a name="field-mapping"></a>Сопоставление полей

Позволяет настроить резервное поле для свойства. Это может быть полезно для свойств, доступных только для чтения, или данных, для которых вместо свойств используются методы Get и Set.

### <a name="mapping-to-memory-optimized-tables-in-sql-server"></a>Сопоставление с оптимизированными для памяти таблицами в SQL Server

Вы можете указать, что сопоставленная с таблицей сущность оптимизирована для памяти. Если вы используете EF Core для создания и обслуживания базы данных на основе используемой модели (с помощью миграций или `Database.EnsureCreated()`), то для таких сущностей будет создана оптимизированная для памяти таблица.

## <a name="change-tracking"></a>отслеживание изменений

### <a name="additional-change-tracking-apis-from-ef6"></a>Дополнительные API из EF6 для отслеживания изменений

Например, `Reload`, `GetModifiedProperties`, `GetDatabaseValues` и т. п.

## <a name="query"></a>Запрос

### <a name="explicit-loading"></a>Явная загрузка

Позволяет активировать заполнение свойства навигации для сущности, ранее загруженной из базы данных.

### <a name="dbsetfind"></a>DbSet.Find

Предоставляет простой способ получить сущность по значению первичного ключа.

## <a name="other"></a>Другие

### <a name="connection-resiliency"></a>Устойчивость подключения

Автоматически выполняет повторные попытки для неудачных обращений к базе данных. Это особенно полезно при подключении к базе данных SQL Azure, для которой характерны временные сбои.

### <a name="simplified-service-replacement"></a>Упрощенная замена служб

Позволяет легко заменить внутренние службы, используемые EF.
