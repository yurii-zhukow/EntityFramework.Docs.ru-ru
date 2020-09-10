---
title: Поставщик базы данных Microsoft SQL Server — индексы — EF Core
description: Функции индекса, относящиеся к поставщику Entity Framework Core SQL Server
author: roji
ms.author: shrojans
ms.date: 9/1/2020
uid: core/providers/sql-server/indexes
ms.openlocfilehash: 3830377562fcc6a59239cd2c09d1419bbd00922e
ms.sourcegitcommit: 7c3939504bb9da3f46bea3443638b808c04227c2
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/09/2020
ms.locfileid: "89620715"
---
# <a name="index-features-specific-to-the-entity-framework-core-sql-server-provider"></a><span data-ttu-id="ceb0d-103">Функции индекса, относящиеся к поставщику Entity Framework Core SQL Server</span><span class="sxs-lookup"><span data-stu-id="ceb0d-103">Index features specific to the Entity Framework Core SQL Server provider</span></span>

<span data-ttu-id="ceb0d-104">На этой странице подробно описаны параметры конфигурации индекса, характерные для поставщика SQL Server.</span><span class="sxs-lookup"><span data-stu-id="ceb0d-104">This page details index configuration options that are specific to the SQL Server provider.</span></span>

## <a name="clustering"></a><span data-ttu-id="ceb0d-105">Кластеризация</span><span class="sxs-lookup"><span data-stu-id="ceb0d-105">Clustering</span></span>

<span data-ttu-id="ceb0d-106">Кластеризованные индексы сортируют и хранят строки данных в таблицах или представлениях на основе их ключевых значений.</span><span class="sxs-lookup"><span data-stu-id="ceb0d-106">Clustered indexes sort and store the data rows in the table or view based on their key values.</span></span> <span data-ttu-id="ceb0d-107">Создание правильного кластеризованного индекса для таблицы может значительно повысить скорость выполнения запросов, так как данные уже размещены в оптимальном порядке.</span><span class="sxs-lookup"><span data-stu-id="ceb0d-107">Creating the right clustered index for your table can significantly improve the speed of your queries, as data is already laid out in the optimal order.</span></span> <span data-ttu-id="ceb0d-108">Существует только один кластеризованный индекс для каждой таблицы, так как строки данных могут храниться в единственном порядке.</span><span class="sxs-lookup"><span data-stu-id="ceb0d-108">There can be only one clustered index per table, because the data rows themselves can be stored in only one order.</span></span> <span data-ttu-id="ceb0d-109">Дополнительные сведения см. [в SQL Server документации по кластеризованным и некластеризованным индексам](/sql/relational-databases/indexes/clustered-and-nonclustered-indexes-described).</span><span class="sxs-lookup"><span data-stu-id="ceb0d-109">For more information, see [the SQL Server documentation on clustered and non-clustered indexes](/sql/relational-databases/indexes/clustered-and-nonclustered-indexes-described).</span></span>

<span data-ttu-id="ceb0d-110">По умолчанию первичный ключевой столбец таблицы неявно поддерживается кластеризованным индексом, а все остальные индексы не являются кластеризованными.</span><span class="sxs-lookup"><span data-stu-id="ceb0d-110">By default, the primary key column of a table is implicitly backed by a clustered index, and all other indexes are non-clustered.</span></span>

<span data-ttu-id="ceb0d-111">Вы можете настроить индекс или ключ для кластеризации следующим образом:</span><span class="sxs-lookup"><span data-stu-id="ceb0d-111">You can configure an index or key to be clustered as follows:</span></span>

[!code-csharp[ClusteredIndex](../../../../samples/core/SqlServer/Indexes/ClusteredIndexContext.cs?name=ClusteredIndex)]

## <a name="fill-factor"></a><span data-ttu-id="ceb0d-112">Коэффициент заполнения.</span><span class="sxs-lookup"><span data-stu-id="ceb0d-112">Fill factor</span></span>

> [!NOTE]
> <span data-ttu-id="ceb0d-113">Эта возможность появилась в EF Core 5.0.</span><span class="sxs-lookup"><span data-stu-id="ceb0d-113">This feature is introduced in EF Core 5.0.</span></span>

<span data-ttu-id="ceb0d-114">Для точной настройки хранения и производительности индексированных данных используется параметр индексный коэффициент заполнения.</span><span class="sxs-lookup"><span data-stu-id="ceb0d-114">The index fill-factor option is provided for fine-tuning index data storage and performance.</span></span> <span data-ttu-id="ceb0d-115">Дополнительные сведения см. [в SQL Server документации по коэффициенту заполнения](/sql/relational-databases/indexes/specify-fill-factor-for-an-index).</span><span class="sxs-lookup"><span data-stu-id="ceb0d-115">For more information, see [the SQL Server documentation on fill factor](/sql/relational-databases/indexes/specify-fill-factor-for-an-index).</span></span>

<span data-ttu-id="ceb0d-116">Коэффициент заполнения индекса можно настроить следующим образом:</span><span class="sxs-lookup"><span data-stu-id="ceb0d-116">You can configure an index's fill factor as follows:</span></span>

[!code-csharp[IndexFillFactor](../../../../samples/core/SqlServer/Indexes/IndexFillFactorContext.cs?name=IndexFillFactor)]

## <a name="online-creation"></a><span data-ttu-id="ceb0d-117">Создание в сети</span><span class="sxs-lookup"><span data-stu-id="ceb0d-117">Online creation</span></span>

<span data-ttu-id="ceb0d-118">Параметр ONLINE позволяет одновременным пользователям обращаться к базовой таблице или данным кластеризованного индекса, а также ко всем связанным некластеризованным индексам во время создания индекса, чтобы пользователи могли продолжать обновлять и запрашивать базовые данные.</span><span class="sxs-lookup"><span data-stu-id="ceb0d-118">The ONLINE option allows concurrent user access to the underlying table or clustered index data and any associated nonclustered indexes during index creation, so that users can continue to update and query the underlying data.</span></span> <span data-ttu-id="ceb0d-119">Если операции языка описания данных DDL (DDL), такие как построение или перестроение кластеризованного индекса, выполняются в режиме «вне сети», то они удерживают монопольные блокировки на базовые данные и связанные индексы.</span><span class="sxs-lookup"><span data-stu-id="ceb0d-119">When you perform data definition language (DDL) operations offline, such as building or rebuilding a clustered index; these operations hold exclusive locks on the underlying data and associated indexes.</span></span> <span data-ttu-id="ceb0d-120">Дополнительные сведения см. [в SQL Server документации по параметру индекса Online](/sql/relational-databases/indexes/perform-index-operations-online).</span><span class="sxs-lookup"><span data-stu-id="ceb0d-120">For more information, see [the SQL Server documentation on the ONLINE index option](/sql/relational-databases/indexes/perform-index-operations-online).</span></span>

<span data-ttu-id="ceb0d-121">Можно настроить индекс с помощью параметра ONLINE, как показано ниже.</span><span class="sxs-lookup"><span data-stu-id="ceb0d-121">You can configure an index with the ONLINE option as follows:</span></span>

[!code-csharp[IndexOnline](../../../../samples/core/SqlServer/Indexes/IndexOnlineContext.cs?name=IndexOnline)]
