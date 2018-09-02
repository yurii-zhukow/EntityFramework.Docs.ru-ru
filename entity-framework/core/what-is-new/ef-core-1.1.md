---
title: Новые возможности в Core EF 1.1 (EF Core)
author: divega
ms.date: 10/27/2016
ms.assetid: C7FE8C85-445A-4F0C-97EC-CC3F7F1D6F5E
uid: core/what-is-new/ef-core-1.1
ms.openlocfilehash: 9f8f2d46f967c7d8ec4f8ea410e51531dfe3ca7b
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/27/2018
ms.locfileid: "42995439"
---
# <a name="new-features-in-ef-core-11"></a><span data-ttu-id="c01e7-102">Новые возможности в EF Core 1.1</span><span class="sxs-lookup"><span data-stu-id="c01e7-102">New features in EF Core 1.1</span></span>

## <a name="modelling"></a><span data-ttu-id="c01e7-103">Моделирование</span><span class="sxs-lookup"><span data-stu-id="c01e7-103">Modelling</span></span>
### <a name="field-mapping"></a><span data-ttu-id="c01e7-104">Сопоставление полей</span><span class="sxs-lookup"><span data-stu-id="c01e7-104">Field mapping</span></span>
<span data-ttu-id="c01e7-105">Позволяет настроить резервное поле для свойства.</span><span class="sxs-lookup"><span data-stu-id="c01e7-105">Allows you to configure a backing field for a property.</span></span> <span data-ttu-id="c01e7-106">Это может быть полезно для свойств, доступных только для чтения, или данных, для которых вместо свойств используются методы Get и Set.</span><span class="sxs-lookup"><span data-stu-id="c01e7-106">This can be useful for read-only properties, or data that has Get/Set methods rather than a property.</span></span>
### <a name="mapping-to-memory-optimized-tables-in-sql-server"></a><span data-ttu-id="c01e7-107">Сопоставление с оптимизированными для памяти таблицами в SQL Server</span><span class="sxs-lookup"><span data-stu-id="c01e7-107">Mapping to Memory-Optimized Tables in SQL Server</span></span>
<span data-ttu-id="c01e7-108">Вы можете указать, что сопоставленная с таблицей сущность оптимизирована для памяти.</span><span class="sxs-lookup"><span data-stu-id="c01e7-108">You can specify that the table an entity is mapped to is memory-optimized.</span></span> <span data-ttu-id="c01e7-109">Если вы используете EF Core для создания и обслуживания базы данных на основе используемой модели (с помощью миграций или `Database.EnsureCreated()`), то для таких сущностей будет создана оптимизированная для памяти таблица.</span><span class="sxs-lookup"><span data-stu-id="c01e7-109">When using EF Core to create and maintain a database based on your model (either with migrations or `Database.EnsureCreated()`), a memory-optimized table will be created for these entities.</span></span>

## <a name="change-tracking"></a><span data-ttu-id="c01e7-110">Change tracking</span><span class="sxs-lookup"><span data-stu-id="c01e7-110">Change tracking</span></span>
### <a name="additional-change-tracking-apis-from-ef6"></a><span data-ttu-id="c01e7-111">Дополнительные API из EF6 для отслеживания изменений</span><span class="sxs-lookup"><span data-stu-id="c01e7-111">Additional change tracking APIs from EF6</span></span>
<span data-ttu-id="c01e7-112">Например, `Reload`, `GetModifiedProperties`, `GetDatabaseValues` и т. п.</span><span class="sxs-lookup"><span data-stu-id="c01e7-112">Such as `Reload`, `GetModifiedProperties`, `GetDatabaseValues` etc.</span></span>

## <a name="query"></a><span data-ttu-id="c01e7-113">Запрос</span><span class="sxs-lookup"><span data-stu-id="c01e7-113">Query</span></span>
### <a name="explicit-loading"></a><span data-ttu-id="c01e7-114">Явная загрузка</span><span class="sxs-lookup"><span data-stu-id="c01e7-114">Explicit Loading</span></span>
<span data-ttu-id="c01e7-115">Позволяет активировать заполнение свойства навигации для сущности, ранее загруженной из базы данных.</span><span class="sxs-lookup"><span data-stu-id="c01e7-115">Allows you to trigger population of a navigation property on an entity that was previously loaded from the database.</span></span>
### <a name="dbsetfind"></a><span data-ttu-id="c01e7-116">DbSet.Find</span><span class="sxs-lookup"><span data-stu-id="c01e7-116">DbSet.Find</span></span>
<span data-ttu-id="c01e7-117">Предоставляет простой способ получить сущность по значению первичного ключа.</span><span class="sxs-lookup"><span data-stu-id="c01e7-117">Provides an easy way to fetch an entity based on its primary key value.</span></span>

## <a name="other"></a><span data-ttu-id="c01e7-118">Другое</span><span class="sxs-lookup"><span data-stu-id="c01e7-118">Other</span></span>
### <a name="connection-resiliency"></a><span data-ttu-id="c01e7-119">Устойчивость подключений</span><span class="sxs-lookup"><span data-stu-id="c01e7-119">Connection resiliency</span></span>
<span data-ttu-id="c01e7-120">Автоматически выполняет повторные попытки для неудачных обращений к базе данных.</span><span class="sxs-lookup"><span data-stu-id="c01e7-120">Automatically retries failed database commands.</span></span> <span data-ttu-id="c01e7-121">Это особенно полезно при подключении к базе данных SQL Azure, для которой характерны временные сбои.</span><span class="sxs-lookup"><span data-stu-id="c01e7-121">This is especially useful when connection to SQL Azure, where transient failures are common.</span></span>
### <a name="simplified-service-replacement"></a><span data-ttu-id="c01e7-122">Упрощенная замена служб</span><span class="sxs-lookup"><span data-stu-id="c01e7-122">Simplified service replacement</span></span>
<span data-ttu-id="c01e7-123">Позволяет легко заменить внутренние службы, используемые EF.</span><span class="sxs-lookup"><span data-stu-id="c01e7-123">Makes it easier to replace internal services that EF uses.</span></span>
