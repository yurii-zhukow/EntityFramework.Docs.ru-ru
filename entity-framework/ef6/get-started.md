---
title: Начало работы с платформой Entity Framework 6 — EF6
description: Начало работы с платформой Entity Framework 6
author: divega
ms.date: 10/23/2016
ms.assetid: 66ce9113-81d2-480f-8c16-d00ec405b2f7
uid: ef6/get-started
ms.openlocfilehash: a03fa403bedc260def3f8110a028e972824cb756
ms.sourcegitcommit: 7c3939504bb9da3f46bea3443638b808c04227c2
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/09/2020
ms.locfileid: "89618205"
---
# <a name="get-started-with-entity-framework-6"></a><span data-ttu-id="90bac-103">Начало работы с платформой Entity Framework 6</span><span class="sxs-lookup"><span data-stu-id="90bac-103">Get started with Entity Framework 6</span></span>

<span data-ttu-id="90bac-104">Это руководство содержит набор ссылок на отдельные статьи с документацией, пошаговые руководства и видео, которые помогут быстро начать работу.</span><span class="sxs-lookup"><span data-stu-id="90bac-104">This guide contains a collection of links to selected documentation articles, walkthroughs and videos that can help you get started quickly.</span></span>

## <a name="fundamentals"></a><span data-ttu-id="90bac-105">Основы</span><span class="sxs-lookup"><span data-stu-id="90bac-105">Fundamentals</span></span>

* [<span data-ttu-id="90bac-106">Установка Entity Framework</span><span class="sxs-lookup"><span data-stu-id="90bac-106">Get Entity Framework</span></span>](xref:ef6/fundamentals/install)

  <span data-ttu-id="90bac-107">Здесь вы узнаете, как добавить Entity Framework в свои приложения. Если вы хотите использовать конструктор EF, установите его в Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="90bac-107">Here you will learn how to add Entity Framework to your applications and, if you want to use the EF Designer, make sure you get it installed in Visual Studio.</span></span>

* [<span data-ttu-id="90bac-108">Создание модели: Code First, конструктор EF и рабочие процессы EF</span><span class="sxs-lookup"><span data-stu-id="90bac-108">Creating a Model: Code First, the EF Designer, and the EF Workflows</span></span>](xref:ef6/modeling/index)

  <span data-ttu-id="90bac-109">Вы предпочитаете задать модель EF, написав код или нарисовав поля и линии?</span><span class="sxs-lookup"><span data-stu-id="90bac-109">Do you prefer to specify your EF model writing code or drawing boxes and lines?</span></span>
<span data-ttu-id="90bac-110">Вы собираетесь использовать EF для сопоставления объектов с существующей базой данных или хотите создать в EF базу данных, настроенную для ваших объектов?</span><span class="sxs-lookup"><span data-stu-id="90bac-110">Are you going to use EF to map your objects to an existing database or would you like EF to create a database tailored for your objects?</span></span>
<span data-ttu-id="90bac-111">Здесь вы узнаете о двух разных подходах к использованию EF6: конструктор EF и Code First.</span><span class="sxs-lookup"><span data-stu-id="90bac-111">Here you learn about two different approaches to use EF6: EF Designer and Code First.</span></span>
<span data-ttu-id="90bac-112">Обязательно подпишитесь на обсуждение и посмотрите видео о различиях между ними.</span><span class="sxs-lookup"><span data-stu-id="90bac-112">Make sure you follow the discussion and watch the video about the difference.</span></span>

* [<span data-ttu-id="90bac-113">Работа с DbContext</span><span class="sxs-lookup"><span data-stu-id="90bac-113">Working with DbContext</span></span>](xref:ef6/fundamentals/working-with-dbcontext)

  <span data-ttu-id="90bac-114">DbContext — это первый и самый важный тип EF, который вам нужно научиться использовать.</span><span class="sxs-lookup"><span data-stu-id="90bac-114">DbContext is the first and most important EF type that you need to learn how to use.</span></span> <span data-ttu-id="90bac-115">Он служит источником запросов базы данных и отслеживает изменения, внесенные в объекты, чтобы их можно было сохранить в базе данных.</span><span class="sxs-lookup"><span data-stu-id="90bac-115">It serves as the launchpad for database queries and keeps track of changes you make to objects so that they can be persisted back to the database.</span></span>

* [<span data-ttu-id="90bac-116">Вопросы</span><span class="sxs-lookup"><span data-stu-id="90bac-116">Ask a Question</span></span>](xref:ef6/resources/get-help)

  <span data-ttu-id="90bac-117">Вы узнаете, как получить справку от экспертов и сможете сами отвечать на вопросы сообщества.</span><span class="sxs-lookup"><span data-stu-id="90bac-117">Find out how to get help from the experts and contribute your own answers to the community.</span></span>

* [<span data-ttu-id="90bac-118">Участие</span><span class="sxs-lookup"><span data-stu-id="90bac-118">Contribute</span></span>](https://github.com/aspnet/EntityFramework6/)

  <span data-ttu-id="90bac-119">В Entity Framework 6 используется модель разработки с открытым исходным кодом.</span><span class="sxs-lookup"><span data-stu-id="90bac-119">Entity Framework 6 uses an open development model.</span></span> <span data-ttu-id="90bac-120">Посетите репозиторий GitHub, чтобы узнать, что поможет улучшить EF.</span><span class="sxs-lookup"><span data-stu-id="90bac-120">Find out how you can help make EF even better by visiting our GitHub repository.</span></span>

## <a name="code-first-resources"></a><span data-ttu-id="90bac-121">Ресурсы по Code First</span><span class="sxs-lookup"><span data-stu-id="90bac-121">Code First resources</span></span>

  - [<span data-ttu-id="90bac-122">Code First для рабочего процесса существующей базы данных</span><span class="sxs-lookup"><span data-stu-id="90bac-122">Code First to an Existing Database Workflow</span></span>](xref:ef6/modeling/code-first/workflows/existing-database)
  - [<span data-ttu-id="90bac-123">Code First для рабочего процесса новой базы данных</span><span class="sxs-lookup"><span data-stu-id="90bac-123">Code First to a New Database Workflow</span></span>](xref:ef6/modeling/code-first/workflows/new-database)
  - [<span data-ttu-id="90bac-124">Сопоставление перечислений с помощью Code First</span><span class="sxs-lookup"><span data-stu-id="90bac-124">Mapping Enums Using Code First</span></span>](xref:ef6/modeling/code-first/data-types/enums)
  - [<span data-ttu-id="90bac-125">Сопоставление пространственных типов с помощью Code First</span><span class="sxs-lookup"><span data-stu-id="90bac-125">Mapping Spatial Types Using Code First</span></span>](xref:ef6/modeling/code-first/data-types/spatial)
  - [<span data-ttu-id="90bac-126">Запись пользовательских соглашений Code First</span><span class="sxs-lookup"><span data-stu-id="90bac-126">Writing Custom Code First Conventions</span></span>](xref:ef6/modeling/code-first/conventions/custom)
  - [<span data-ttu-id="90bac-127">Настройка Code First для использования текучего API в Visual Basic</span><span class="sxs-lookup"><span data-stu-id="90bac-127">Using Code First Fluent Configuration with Visual Basic</span></span>](xref:ef6/modeling/code-first/fluent/vb)
  - [<span data-ttu-id="90bac-128">Code First Migrations</span><span class="sxs-lookup"><span data-stu-id="90bac-128">Code First Migrations</span></span>](xref:ef6/modeling/code-first/migrations/index)
  - [<span data-ttu-id="90bac-129">Code First Migrations в командных средах</span><span class="sxs-lookup"><span data-stu-id="90bac-129">Code First Migrations in Team Environments</span></span>](xref:ef6/modeling/code-first/migrations/teams)
  - <span data-ttu-id="90bac-130">[Автоматические миграции Code First Migrations](xref:ef6/modeling/code-first/migrations/automatic) (больше не рекомендуется)</span><span class="sxs-lookup"><span data-stu-id="90bac-130">[Automatic Code First Migrations](xref:ef6/modeling/code-first/migrations/automatic) (This is no longer recommended)</span></span>

## <a name="ef-designer-resources"></a><span data-ttu-id="90bac-131">Ресурсы по конструктору EF</span><span class="sxs-lookup"><span data-stu-id="90bac-131">EF Designer resources</span></span>
  - [<span data-ttu-id="90bac-132">Рабочий процесс Database First</span><span class="sxs-lookup"><span data-stu-id="90bac-132">Database First Workflow</span></span>](xref:ef6/modeling/designer/workflows/database-first)
  - [<span data-ttu-id="90bac-133">Рабочий процесс Model First</span><span class="sxs-lookup"><span data-stu-id="90bac-133">Model First Workflow</span></span>](xref:ef6/modeling/designer/workflows/model-first)
  - [<span data-ttu-id="90bac-134">Сопоставление перечислений</span><span class="sxs-lookup"><span data-stu-id="90bac-134">Mapping Enums</span></span>](xref:ef6/modeling/designer/data-types/enums)
  - [<span data-ttu-id="90bac-135">Сопоставление пространственных типов</span><span class="sxs-lookup"><span data-stu-id="90bac-135">Mapping Spatial Types</span></span>](xref:ef6/modeling/designer/data-types/spatial)
  - [<span data-ttu-id="90bac-136">Сопоставление наследования "одна таблица на иерархию"</span><span class="sxs-lookup"><span data-stu-id="90bac-136">Table-Per Hierarchy Inheritance Mapping</span></span>](xref:ef6/modeling/designer/inheritance/tph)
  - [<span data-ttu-id="90bac-137">Сопоставление наследования "одна таблица на тип"</span><span class="sxs-lookup"><span data-stu-id="90bac-137">Table-Per Type Inheritance Mapping</span></span>](xref:ef6/modeling/designer/inheritance/tpt)
  - [<span data-ttu-id="90bac-138">Сопоставление хранимых процедур для обновлений</span><span class="sxs-lookup"><span data-stu-id="90bac-138">Stored Procedure Mapping for Updates</span></span>](xref:ef6/modeling/designer/stored-procedures/cud)
  - [<span data-ttu-id="90bac-139">Сопоставление хранимых процедур для запроса</span><span class="sxs-lookup"><span data-stu-id="90bac-139">Stored Procedure Mapping for Query</span></span>](xref:ef6/modeling/designer/stored-procedures/query)
  - [<span data-ttu-id="90bac-140">Разделение сущности</span><span class="sxs-lookup"><span data-stu-id="90bac-140">Entity Splitting</span></span>](xref:ef6/modeling/designer/entity-splitting)
  - [<span data-ttu-id="90bac-141">Разделение таблицы</span><span class="sxs-lookup"><span data-stu-id="90bac-141">Table Splitting</span></span>](xref:ef6/modeling/designer/table-splitting)
  - <span data-ttu-id="90bac-142">[Определение запроса](xref:ef6/modeling/designer/advanced/defining-query) (дополнительно)</span><span class="sxs-lookup"><span data-stu-id="90bac-142">[Defining Query](xref:ef6/modeling/designer/advanced/defining-query) (Advanced)</span></span>
  - <span data-ttu-id="90bac-143">[Функции с табличными значениями](xref:ef6/modeling/designer/advanced/tvfs) (дополнительно)</span><span class="sxs-lookup"><span data-stu-id="90bac-143">[Table-Valued Functions](xref:ef6/modeling/designer/advanced/tvfs) (Advanced)</span></span>

## <a name="other-resources"></a><span data-ttu-id="90bac-144">Другие ресурсы</span><span class="sxs-lookup"><span data-stu-id="90bac-144">Other resources</span></span>
  - [<span data-ttu-id="90bac-145">Асинхронный запрос и сохранение</span><span class="sxs-lookup"><span data-stu-id="90bac-145">Async Query and Save</span></span>](xref:ef6/fundamentals/async)
  - [<span data-ttu-id="90bac-146">Привязка данных с помощью WinForms</span><span class="sxs-lookup"><span data-stu-id="90bac-146">Databinding with WinForms</span></span>](xref:ef6/fundamentals/databinding/winforms)
  - [<span data-ttu-id="90bac-147">Привязка данных с помощью WPF</span><span class="sxs-lookup"><span data-stu-id="90bac-147">Databinding with WPF</span></span>](xref:ef6/fundamentals/databinding/wpf)
  - <span data-ttu-id="90bac-148">[Отключенные сценарии с самоотслеживающимися сущностями](xref:ef6/fundamentals/disconnected-entities/self-tracking-entities/walkthrough) (больше не рекомендуется)</span><span class="sxs-lookup"><span data-stu-id="90bac-148">[Disconnected scenarios with Self-Tracking Entities](xref:ef6/fundamentals/disconnected-entities/self-tracking-entities/walkthrough) (This is no longer recommended)</span></span>
