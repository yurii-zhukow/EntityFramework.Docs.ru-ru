---
title: Новые возможности в Core EF 1.1 (EF Core)
description: Изменения и улучшения в Entity Framework Core 1.1
author: divega
ms.date: 10/27/2016
uid: core/what-is-new/ef-core-1.1
ms.openlocfilehash: 5f81e8b25feba5cdf5ae5e84b1d3362912ab5b26
ms.sourcegitcommit: abda0872f86eefeca191a9a11bfca976bc14468b
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/14/2020
ms.locfileid: "90072412"
---
# <a name="new-features-in-ef-core-11"></a><span data-ttu-id="4d492-103">Новые возможности в EF Core 1.1</span><span class="sxs-lookup"><span data-stu-id="4d492-103">New features in EF Core 1.1</span></span>

## <a name="modeling"></a><span data-ttu-id="4d492-104">Моделирование</span><span class="sxs-lookup"><span data-stu-id="4d492-104">Modeling</span></span>

### <a name="field-mapping"></a><span data-ttu-id="4d492-105">Сопоставление полей</span><span class="sxs-lookup"><span data-stu-id="4d492-105">Field mapping</span></span>

<span data-ttu-id="4d492-106">Позволяет настроить резервное поле для свойства.</span><span class="sxs-lookup"><span data-stu-id="4d492-106">Allows you to configure a backing field for a property.</span></span> <span data-ttu-id="4d492-107">Это может быть полезно для свойств, доступных только для чтения, или данных, для которых вместо свойств используются методы Get и Set.</span><span class="sxs-lookup"><span data-stu-id="4d492-107">This can be useful for read-only properties, or data that has Get/Set methods rather than a property.</span></span>

### <a name="mapping-to-memory-optimized-tables-in-sql-server"></a><span data-ttu-id="4d492-108">Сопоставление с оптимизированными для памяти таблицами в SQL Server</span><span class="sxs-lookup"><span data-stu-id="4d492-108">Mapping to Memory-Optimized Tables in SQL Server</span></span>

<span data-ttu-id="4d492-109">Вы можете указать, что сопоставленная с таблицей сущность оптимизирована для памяти.</span><span class="sxs-lookup"><span data-stu-id="4d492-109">You can specify that the table an entity is mapped to is memory-optimized.</span></span> <span data-ttu-id="4d492-110">Если вы используете EF Core для создания и обслуживания базы данных на основе используемой модели (с помощью миграций или `Database.EnsureCreated()`), то для таких сущностей будет создана оптимизированная для памяти таблица.</span><span class="sxs-lookup"><span data-stu-id="4d492-110">When using EF Core to create and maintain a database based on your model (either with migrations or `Database.EnsureCreated()`), a memory-optimized table will be created for these entities.</span></span>

## <a name="change-tracking"></a><span data-ttu-id="4d492-111">отслеживание изменений</span><span class="sxs-lookup"><span data-stu-id="4d492-111">Change tracking</span></span>

### <a name="additional-change-tracking-apis-from-ef6"></a><span data-ttu-id="4d492-112">Дополнительные API из EF6 для отслеживания изменений</span><span class="sxs-lookup"><span data-stu-id="4d492-112">Additional change tracking APIs from EF6</span></span>

<span data-ttu-id="4d492-113">Например, `Reload`, `GetModifiedProperties`, `GetDatabaseValues` и т. п.</span><span class="sxs-lookup"><span data-stu-id="4d492-113">Such as `Reload`, `GetModifiedProperties`, `GetDatabaseValues` etc.</span></span>

## <a name="query"></a><span data-ttu-id="4d492-114">Запрос</span><span class="sxs-lookup"><span data-stu-id="4d492-114">Query</span></span>

### <a name="explicit-loading"></a><span data-ttu-id="4d492-115">Явная загрузка</span><span class="sxs-lookup"><span data-stu-id="4d492-115">Explicit Loading</span></span>

<span data-ttu-id="4d492-116">Позволяет активировать заполнение свойства навигации для сущности, ранее загруженной из базы данных.</span><span class="sxs-lookup"><span data-stu-id="4d492-116">Allows you to trigger population of a navigation property on an entity that was previously loaded from the database.</span></span>

### <a name="dbsetfind"></a><span data-ttu-id="4d492-117">DbSet.Find</span><span class="sxs-lookup"><span data-stu-id="4d492-117">DbSet.Find</span></span>

<span data-ttu-id="4d492-118">Предоставляет простой способ получить сущность по значению первичного ключа.</span><span class="sxs-lookup"><span data-stu-id="4d492-118">Provides an easy way to fetch an entity based on its primary key value.</span></span>

## <a name="other"></a><span data-ttu-id="4d492-119">Другие</span><span class="sxs-lookup"><span data-stu-id="4d492-119">Other</span></span>

### <a name="connection-resiliency"></a><span data-ttu-id="4d492-120">Устойчивость подключения</span><span class="sxs-lookup"><span data-stu-id="4d492-120">Connection resiliency</span></span>

<span data-ttu-id="4d492-121">Автоматически выполняет повторные попытки для неудачных обращений к базе данных.</span><span class="sxs-lookup"><span data-stu-id="4d492-121">Automatically retries failed database commands.</span></span> <span data-ttu-id="4d492-122">Это особенно полезно при подключении к базе данных SQL Azure, для которой характерны временные сбои.</span><span class="sxs-lookup"><span data-stu-id="4d492-122">This is especially useful when connection to SQL Azure, where transient failures are common.</span></span>

### <a name="simplified-service-replacement"></a><span data-ttu-id="4d492-123">Упрощенная замена служб</span><span class="sxs-lookup"><span data-stu-id="4d492-123">Simplified service replacement</span></span>

<span data-ttu-id="4d492-124">Позволяет легко заменить внутренние службы, используемые EF.</span><span class="sxs-lookup"><span data-stu-id="4d492-124">Makes it easier to replace internal services that EF uses.</span></span>
