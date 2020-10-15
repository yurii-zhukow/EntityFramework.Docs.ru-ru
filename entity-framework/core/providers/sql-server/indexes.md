---
title: Поставщик базы данных Microsoft SQL Server — индексы — EF Core
description: Функции индекса, относящиеся к поставщику Entity Framework Core SQL Server
author: roji
ms.date: 9/1/2020
uid: core/providers/sql-server/indexes
ms.openlocfilehash: a01ab3d12f5bf5f05f0925c93d90c0ee40fe977c
ms.sourcegitcommit: 0a25c03fa65ae6e0e0e3f66bac48d59eceb96a5a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/14/2020
ms.locfileid: "92061870"
---
# <a name="index-features-specific-to-the-entity-framework-core-sql-server-provider"></a>Функции индекса, относящиеся к поставщику Entity Framework Core SQL Server

На этой странице подробно описаны параметры конфигурации индекса, характерные для поставщика SQL Server.

## <a name="clustering"></a>Кластеризация

Кластеризованные индексы сортируют и хранят строки данных в таблицах или представлениях на основе их ключевых значений. Создание правильного кластеризованного индекса для таблицы может значительно повысить скорость выполнения запросов, так как данные уже размещены в оптимальном порядке. Существует только один кластеризованный индекс для каждой таблицы, так как строки данных могут храниться в единственном порядке. Дополнительные сведения см. [в SQL Server документации по кластеризованным и некластеризованным индексам](/sql/relational-databases/indexes/clustered-and-nonclustered-indexes-described).

По умолчанию первичный ключевой столбец таблицы неявно поддерживается кластеризованным индексом, а все остальные индексы не являются кластеризованными.

Вы можете настроить индекс или ключ для кластеризации следующим образом:

[!code-csharp[ClusteredIndex](../../../../samples/core/SqlServer/Indexes/ClusteredIndexContext.cs?name=ClusteredIndex)]

## <a name="fill-factor"></a>Коэффициент заполнения.

> [!NOTE]
> Эта возможность появилась в EF Core 5.0.

Для точной настройки хранения и производительности индексированных данных используется параметр индексный коэффициент заполнения. Дополнительные сведения см. [в SQL Server документации по коэффициенту заполнения](/sql/relational-databases/indexes/specify-fill-factor-for-an-index).

Коэффициент заполнения индекса можно настроить следующим образом:

[!code-csharp[IndexFillFactor](../../../../samples/core/SqlServer/Indexes/IndexFillFactorContext.cs?name=IndexFillFactor)]

## <a name="online-creation"></a>Создание в сети

Параметр ONLINE позволяет одновременным пользователям обращаться к базовой таблице или данным кластеризованного индекса, а также ко всем связанным некластеризованным индексам во время создания индекса, чтобы пользователи могли продолжать обновлять и запрашивать базовые данные. Если операции языка описания данных DDL (DDL), такие как построение или перестроение кластеризованного индекса, выполняются в режиме «вне сети», то они удерживают монопольные блокировки на базовые данные и связанные индексы. Дополнительные сведения см. [в SQL Server документации по параметру индекса Online](/sql/relational-databases/indexes/perform-index-operations-online).

Можно настроить индекс с помощью параметра ONLINE, как показано ниже.

[!code-csharp[IndexOnline](../../../../samples/core/SqlServer/Indexes/IndexOnlineContext.cs?name=IndexOnline)]
