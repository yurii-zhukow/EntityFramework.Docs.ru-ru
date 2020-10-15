---
title: Новые возможности в Core EF 1.1 (EF Core)
description: Изменения и улучшения в Entity Framework Core 1.1
author: ajcvickers
ms.date: 10/27/2016
uid: core/what-is-new/ef-core-1.1
ms.openlocfilehash: a865270d0b1b690ed2596e7ed550463ab0877bca
ms.sourcegitcommit: 0a25c03fa65ae6e0e0e3f66bac48d59eceb96a5a
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/14/2020
ms.locfileid: "92063469"
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
