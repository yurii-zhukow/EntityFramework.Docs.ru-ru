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
# <a name="sqlite-ef-core-database-provider-limitations"></a><span data-ttu-id="bdf00-102">Ограничения функций поставщика SQLite EF ядра базы данных</span><span class="sxs-lookup"><span data-stu-id="bdf00-102">SQLite EF Core Database Provider Limitations</span></span>

<span data-ttu-id="bdf00-103">Поставщик SQLite имеет ряд ограничений миграции.</span><span class="sxs-lookup"><span data-stu-id="bdf00-103">The SQLite provider has a number of migrations limitations.</span></span> <span data-ttu-id="bdf00-104">Большинство этих ограничений представляют собой результат применения ограничения в компонент database engine SQLite и не относятся к EF.</span><span class="sxs-lookup"><span data-stu-id="bdf00-104">Most of these limitations are a result of limitations in the underlying SQLite database engine and are not specific to EF.</span></span>

## <a name="modeling-limitations"></a><span data-ttu-id="bdf00-105">Ограничения моделирования</span><span class="sxs-lookup"><span data-stu-id="bdf00-105">Modeling limitations</span></span>

<span data-ttu-id="bdf00-106">Общей библиотекой реляционных (совместно используемые поставщиками реляционной базы данных Entity Framework) определяет интерфейсы API для моделирования, основные понятия, которые являются общими для большинства ядра реляционной базы данных.</span><span class="sxs-lookup"><span data-stu-id="bdf00-106">The common relational library (shared by Entity Framework relational database providers) defines APIs for modelling concepts that are common to most relational database engines.</span></span> <span data-ttu-id="bdf00-107">Несколько из этих концепций не поддерживаются поставщиком SQLite.</span><span class="sxs-lookup"><span data-stu-id="bdf00-107">A couple of these concepts are not supported by the SQLite provider.</span></span>

* <span data-ttu-id="bdf00-108">Схемы</span><span class="sxs-lookup"><span data-stu-id="bdf00-108">Schemas</span></span>
* <span data-ttu-id="bdf00-109">Последовательности</span><span class="sxs-lookup"><span data-stu-id="bdf00-109">Sequences</span></span>

## <a name="migrations-limitations"></a><span data-ttu-id="bdf00-110">Ограничения миграции</span><span class="sxs-lookup"><span data-stu-id="bdf00-110">Migrations limitations</span></span>

<span data-ttu-id="bdf00-111">Ядро базы данных SQLite поддерживает ряд операций схемы, которые поддерживаются большинством других реляционных баз данных.</span><span class="sxs-lookup"><span data-stu-id="bdf00-111">The SQLite database engine does not support a number of schema operations that are supported by the majority of other relational databases.</span></span> <span data-ttu-id="bdf00-112">При попытке применить одну из неподдерживаемых операций в базу данных SQLite то `NotSupportedException` будет создано.</span><span class="sxs-lookup"><span data-stu-id="bdf00-112">If you attempt to apply one of the unsupported operations to a SQLite database then a `NotSupportedException` will be thrown.</span></span>

| <span data-ttu-id="bdf00-113">Операция</span><span class="sxs-lookup"><span data-stu-id="bdf00-113">Operation</span></span>            | <span data-ttu-id="bdf00-114">Поддерживается?</span><span class="sxs-lookup"><span data-stu-id="bdf00-114">Supported?</span></span> |
| -------------------- | ---------- |
| <span data-ttu-id="bdf00-115">AddColumn</span><span class="sxs-lookup"><span data-stu-id="bdf00-115">AddColumn</span></span>            | <span data-ttu-id="bdf00-116">✔</span><span class="sxs-lookup"><span data-stu-id="bdf00-116">✔</span></span>          |
| <span data-ttu-id="bdf00-117">AddForeignKey</span><span class="sxs-lookup"><span data-stu-id="bdf00-117">AddForeignKey</span></span>        | <span data-ttu-id="bdf00-118">✗</span><span class="sxs-lookup"><span data-stu-id="bdf00-118">✗</span></span>          |
| <span data-ttu-id="bdf00-119">AddPrimaryKey</span><span class="sxs-lookup"><span data-stu-id="bdf00-119">AddPrimaryKey</span></span>        | <span data-ttu-id="bdf00-120">✗</span><span class="sxs-lookup"><span data-stu-id="bdf00-120">✗</span></span>          |
| <span data-ttu-id="bdf00-121">AddUniqueConstraint</span><span class="sxs-lookup"><span data-stu-id="bdf00-121">AddUniqueConstraint</span></span>  | <span data-ttu-id="bdf00-122">✗</span><span class="sxs-lookup"><span data-stu-id="bdf00-122">✗</span></span>          |
| <span data-ttu-id="bdf00-123">AlterColumn</span><span class="sxs-lookup"><span data-stu-id="bdf00-123">AlterColumn</span></span>          | <span data-ttu-id="bdf00-124">✗</span><span class="sxs-lookup"><span data-stu-id="bdf00-124">✗</span></span>          |
| <span data-ttu-id="bdf00-125">CreateIndex</span><span class="sxs-lookup"><span data-stu-id="bdf00-125">CreateIndex</span></span>          | <span data-ttu-id="bdf00-126">✔</span><span class="sxs-lookup"><span data-stu-id="bdf00-126">✔</span></span>          |
| <span data-ttu-id="bdf00-127">CreateTable</span><span class="sxs-lookup"><span data-stu-id="bdf00-127">CreateTable</span></span>          | <span data-ttu-id="bdf00-128">✔</span><span class="sxs-lookup"><span data-stu-id="bdf00-128">✔</span></span>          |
| <span data-ttu-id="bdf00-129">DropColumn</span><span class="sxs-lookup"><span data-stu-id="bdf00-129">DropColumn</span></span>           | <span data-ttu-id="bdf00-130">✗</span><span class="sxs-lookup"><span data-stu-id="bdf00-130">✗</span></span>          |
| <span data-ttu-id="bdf00-131">DropForeignKey</span><span class="sxs-lookup"><span data-stu-id="bdf00-131">DropForeignKey</span></span>       | <span data-ttu-id="bdf00-132">✗</span><span class="sxs-lookup"><span data-stu-id="bdf00-132">✗</span></span>          |
| <span data-ttu-id="bdf00-133">DropIndex</span><span class="sxs-lookup"><span data-stu-id="bdf00-133">DropIndex</span></span>            | <span data-ttu-id="bdf00-134">✔</span><span class="sxs-lookup"><span data-stu-id="bdf00-134">✔</span></span>          |
| <span data-ttu-id="bdf00-135">DropPrimaryKey</span><span class="sxs-lookup"><span data-stu-id="bdf00-135">DropPrimaryKey</span></span>       | <span data-ttu-id="bdf00-136">✗</span><span class="sxs-lookup"><span data-stu-id="bdf00-136">✗</span></span>          |
| <span data-ttu-id="bdf00-137">DropTable</span><span class="sxs-lookup"><span data-stu-id="bdf00-137">DropTable</span></span>            | <span data-ttu-id="bdf00-138">✔</span><span class="sxs-lookup"><span data-stu-id="bdf00-138">✔</span></span>          |
| <span data-ttu-id="bdf00-139">DropUniqueConstraint</span><span class="sxs-lookup"><span data-stu-id="bdf00-139">DropUniqueConstraint</span></span> | <span data-ttu-id="bdf00-140">✗</span><span class="sxs-lookup"><span data-stu-id="bdf00-140">✗</span></span>          |
| <span data-ttu-id="bdf00-141">RenameColumn</span><span class="sxs-lookup"><span data-stu-id="bdf00-141">RenameColumn</span></span>         | <span data-ttu-id="bdf00-142">✗</span><span class="sxs-lookup"><span data-stu-id="bdf00-142">✗</span></span>          |
| <span data-ttu-id="bdf00-143">RenameIndex</span><span class="sxs-lookup"><span data-stu-id="bdf00-143">RenameIndex</span></span>          | <span data-ttu-id="bdf00-144">✗</span><span class="sxs-lookup"><span data-stu-id="bdf00-144">✗</span></span>          |
| <span data-ttu-id="bdf00-145">RenameTable</span><span class="sxs-lookup"><span data-stu-id="bdf00-145">RenameTable</span></span>          | <span data-ttu-id="bdf00-146">✔</span><span class="sxs-lookup"><span data-stu-id="bdf00-146">✔</span></span>          |

## <a name="migrations-limitations-workaround"></a><span data-ttu-id="bdf00-147">Обходной путь для ограничения миграции</span><span class="sxs-lookup"><span data-stu-id="bdf00-147">Migrations limitations workaround</span></span>

<span data-ttu-id="bdf00-148">Инструкции по решению можно выполнить некоторые из этих ограничений, написав код в миграции для выполнения таблицы вручную перестроить.</span><span class="sxs-lookup"><span data-stu-id="bdf00-148">You can workaround some of these limitations by manually writing code in your migrations to perform a table rebuild.</span></span> <span data-ttu-id="bdf00-149">Перестройка таблицы включает в себя переименование существующей таблицы, создав новую таблицу, копирование данных в новую таблицу и удалить старую таблицу.</span><span class="sxs-lookup"><span data-stu-id="bdf00-149">A table rebuild involves renaming the existing table, creating a new table, copying data to the new table, and dropping the old table.</span></span> <span data-ttu-id="bdf00-150">Необходимо будет использовать `Sql(string)` метод для выполнения некоторых из следующих действий.</span><span class="sxs-lookup"><span data-stu-id="bdf00-150">You will need to use the `Sql(string)` method to perform some of these steps.</span></span>

<span data-ttu-id="bdf00-151">В разделе [внесения других типов из таблицы изменений схемы](http://sqlite.org/lang_altertable.html#otheralter) в SQLite документации для получения дополнительных сведений.</span><span class="sxs-lookup"><span data-stu-id="bdf00-151">See [Making Other Kinds Of Table Schema Changes](http://sqlite.org/lang_altertable.html#otheralter) in the SQLite documentation for more details.</span></span>

<span data-ttu-id="bdf00-152">В будущем EF поддерживает некоторые из этих операций, используя подход перестроения таблица на самом деле.</span><span class="sxs-lookup"><span data-stu-id="bdf00-152">In the future, EF may support some of these operations by using the table rebuild approach under the covers.</span></span> <span data-ttu-id="bdf00-153">Вы можете [отслеживать этот компонент на нашем сайте GitHub проекта](https://github.com/aspnet/EntityFramework/issues/329).</span><span class="sxs-lookup"><span data-stu-id="bdf00-153">You can [track this feature on our GitHub project](https://github.com/aspnet/EntityFramework/issues/329).</span></span>
