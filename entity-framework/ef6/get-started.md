---
title: Начало работы с платформой Entity Framework 6 — EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 66ce9113-81d2-480f-8c16-d00ec405b2f7
uid: ef6/get-started
ms.openlocfilehash: 729dea2c474c35f638ccaf6673550f76e88e2667
ms.sourcegitcommit: c3b8386071d64953ee68788ef9d951144881a6ab
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/24/2020
ms.locfileid: "80136269"
---
# <a name="get-started-with-entity-framework-6"></a><span data-ttu-id="e46b1-102">Начало работы с платформой Entity Framework 6</span><span class="sxs-lookup"><span data-stu-id="e46b1-102">Get started with Entity Framework 6</span></span>

<span data-ttu-id="e46b1-103">Это руководство содержит набор ссылок на отдельные статьи с документацией, пошаговые руководства и видео, которые помогут быстро начать работу.</span><span class="sxs-lookup"><span data-stu-id="e46b1-103">This guide contains a collection of links to selected documentation articles, walkthroughs and videos that can help you get started quickly.</span></span>

## <a name="fundamentals"></a><span data-ttu-id="e46b1-104">Основы</span><span class="sxs-lookup"><span data-stu-id="e46b1-104">Fundamentals</span></span>

* [<span data-ttu-id="e46b1-105">Установка Entity Framework</span><span class="sxs-lookup"><span data-stu-id="e46b1-105">Get Entity Framework</span></span>](~/ef6/fundamentals/install.md)

  <span data-ttu-id="e46b1-106">Здесь вы узнаете, как добавить Entity Framework в свои приложения. Если вы хотите использовать конструктор EF, установите его в Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="e46b1-106">Here you will learn how to add Entity Framework to your applications and, if you want to use the EF Designer, make sure you get it installed in Visual Studio.</span></span>

* [<span data-ttu-id="e46b1-107">Создание модели: Code First, конструктор EF и рабочие процессы EF</span><span class="sxs-lookup"><span data-stu-id="e46b1-107">Creating a Model: Code First, the EF Designer, and the EF Workflows</span></span>](~/ef6/modeling/index.md)

  <span data-ttu-id="e46b1-108">Вы предпочитаете задать модель EF, написав код или нарисовав поля и линии?</span><span class="sxs-lookup"><span data-stu-id="e46b1-108">Do you prefer to specify your EF model writing code or drawing boxes and lines?</span></span>
<span data-ttu-id="e46b1-109">Вы собираетесь использовать EF для сопоставления объектов с существующей базой данных или хотите создать в EF базу данных, настроенную для ваших объектов?</span><span class="sxs-lookup"><span data-stu-id="e46b1-109">Are you going to use EF to map your objects to an existing database or would you like EF to create a database tailored for your objects?</span></span>
<span data-ttu-id="e46b1-110">Здесь вы узнаете о двух разных подходах к использованию EF6: конструктор EF и Code First.</span><span class="sxs-lookup"><span data-stu-id="e46b1-110">Here you learn about two different approaches to use EF6: EF Designer and Code First.</span></span>
<span data-ttu-id="e46b1-111">Обязательно подпишитесь на обсуждение и посмотрите видео о различиях между ними.</span><span class="sxs-lookup"><span data-stu-id="e46b1-111">Make sure you follow the discussion and watch the video about the difference.</span></span>

* [<span data-ttu-id="e46b1-112">Работа с DbContext</span><span class="sxs-lookup"><span data-stu-id="e46b1-112">Working with DbContext</span></span>](~/ef6/fundamentals/working-with-dbcontext.md)

  <span data-ttu-id="e46b1-113">DbContext — это первый и самый важный тип EF, который вам нужно научиться использовать.</span><span class="sxs-lookup"><span data-stu-id="e46b1-113">DbContext is the first and most important EF type that you need to learn how to use.</span></span> <span data-ttu-id="e46b1-114">Он служит источником запросов базы данных и отслеживает изменения, внесенные в объекты, чтобы их можно было сохранить в базе данных.</span><span class="sxs-lookup"><span data-stu-id="e46b1-114">It serves as the launchpad for database queries and keeps track of changes you make to objects so that they can be persisted back to the database.</span></span>

* [<span data-ttu-id="e46b1-115">Вопросы</span><span class="sxs-lookup"><span data-stu-id="e46b1-115">Ask a Question</span></span>](~/ef6/resources/get-help.md)

  <span data-ttu-id="e46b1-116">Вы узнаете, как получить справку от экспертов и сможете сами отвечать на вопросы сообщества.</span><span class="sxs-lookup"><span data-stu-id="e46b1-116">Find out how to get help from the experts and contribute your own answers to the community.</span></span>

* [<span data-ttu-id="e46b1-117">Участие</span><span class="sxs-lookup"><span data-stu-id="e46b1-117">Contribute</span></span>](https://github.com/aspnet/EntityFramework6/)

  <span data-ttu-id="e46b1-118">В Entity Framework 6 используется модель разработки с открытым исходным кодом.</span><span class="sxs-lookup"><span data-stu-id="e46b1-118">Entity Framework 6 uses an open development model.</span></span> <span data-ttu-id="e46b1-119">Посетите репозиторий GitHub, чтобы узнать, что поможет улучшить EF.</span><span class="sxs-lookup"><span data-stu-id="e46b1-119">Find out how you can help make EF even better by visiting our GitHub repository.</span></span>

## <a name="code-first-resources"></a><span data-ttu-id="e46b1-120">Ресурсы по Code First</span><span class="sxs-lookup"><span data-stu-id="e46b1-120">Code First resources</span></span>

  - [<span data-ttu-id="e46b1-121">Code First для рабочего процесса существующей базы данных</span><span class="sxs-lookup"><span data-stu-id="e46b1-121">Code First to an Existing Database Workflow</span></span>](~/ef6/modeling/code-first/workflows/existing-database.md)
  - [<span data-ttu-id="e46b1-122">Code First для рабочего процесса новой базы данных</span><span class="sxs-lookup"><span data-stu-id="e46b1-122">Code First to a New Database Workflow</span></span>](~/ef6/modeling/code-first/workflows/new-database.md)
  - [<span data-ttu-id="e46b1-123">Сопоставление перечислений с помощью Code First</span><span class="sxs-lookup"><span data-stu-id="e46b1-123">Mapping Enums Using Code First</span></span>](~/ef6/modeling/code-first/data-types/enums.md)
  - [<span data-ttu-id="e46b1-124">Сопоставление пространственных типов с помощью Code First</span><span class="sxs-lookup"><span data-stu-id="e46b1-124">Mapping Spatial Types Using Code First</span></span>](~/ef6/modeling/code-first/data-types/spatial.md)
  - [<span data-ttu-id="e46b1-125">Запись пользовательских соглашений Code First</span><span class="sxs-lookup"><span data-stu-id="e46b1-125">Writing Custom Code First Conventions</span></span>](~/ef6/modeling/code-first/conventions/custom.md)
  - [<span data-ttu-id="e46b1-126">Настройка Code First для использования текучего API в Visual Basic</span><span class="sxs-lookup"><span data-stu-id="e46b1-126">Using Code First Fluent Configuration with Visual Basic</span></span>](~/ef6/modeling/code-first/fluent/vb.md)
  - [<span data-ttu-id="e46b1-127">Code First Migrations</span><span class="sxs-lookup"><span data-stu-id="e46b1-127">Code First Migrations</span></span>](~/ef6/modeling/code-first/migrations/index.md)
  - [<span data-ttu-id="e46b1-128">Code First Migrations в командных средах</span><span class="sxs-lookup"><span data-stu-id="e46b1-128">Code First Migrations in Team Environments</span></span>](~/ef6/modeling/code-first/migrations/teams.md)
  - <span data-ttu-id="e46b1-129">[Автоматические миграции Code First Migrations](~/ef6/modeling/code-first/migrations/automatic.md) (больше не рекомендуется)</span><span class="sxs-lookup"><span data-stu-id="e46b1-129">[Automatic Code First Migrations](~/ef6/modeling/code-first/migrations/automatic.md) (This is no longer recommended)</span></span>

## <a name="ef-designer-resources"></a><span data-ttu-id="e46b1-130">Ресурсы по конструктору EF</span><span class="sxs-lookup"><span data-stu-id="e46b1-130">EF Designer resources</span></span>
  - [<span data-ttu-id="e46b1-131">Рабочий процесс Database First</span><span class="sxs-lookup"><span data-stu-id="e46b1-131">Database First Workflow</span></span>](~/ef6/modeling/designer/workflows/database-first.md)
  - [<span data-ttu-id="e46b1-132">Рабочий процесс Model First</span><span class="sxs-lookup"><span data-stu-id="e46b1-132">Model First Workflow</span></span>](~/ef6/modeling/designer/workflows/model-first.md)
  - [<span data-ttu-id="e46b1-133">Сопоставление перечислений</span><span class="sxs-lookup"><span data-stu-id="e46b1-133">Mapping Enums</span></span>](~/ef6/modeling/designer/data-types/enums.md)
  - [<span data-ttu-id="e46b1-134">Сопоставление пространственных типов</span><span class="sxs-lookup"><span data-stu-id="e46b1-134">Mapping Spatial Types</span></span>](~/ef6/modeling/designer/data-types/spatial.md)
  - [<span data-ttu-id="e46b1-135">Сопоставление наследования "одна таблица на иерархию"</span><span class="sxs-lookup"><span data-stu-id="e46b1-135">Table-Per Hierarchy Inheritance Mapping</span></span>](~/ef6/modeling/designer/inheritance/tph.md)
  - [<span data-ttu-id="e46b1-136">Сопоставление наследования "одна таблица на тип"</span><span class="sxs-lookup"><span data-stu-id="e46b1-136">Table-Per Type Inheritance Mapping</span></span>](~/ef6/modeling/designer/inheritance/tpt.md)
  - [<span data-ttu-id="e46b1-137">Сопоставление хранимых процедур для обновлений</span><span class="sxs-lookup"><span data-stu-id="e46b1-137">Stored Procedure Mapping for Updates</span></span>](~/ef6/modeling/designer/stored-procedures/cud.md)
  - [<span data-ttu-id="e46b1-138">Сопоставление хранимых процедур для запроса</span><span class="sxs-lookup"><span data-stu-id="e46b1-138">Stored Procedure Mapping for Query</span></span>](~/ef6/modeling/designer/stored-procedures/query.md)
  - [<span data-ttu-id="e46b1-139">Разделение сущности</span><span class="sxs-lookup"><span data-stu-id="e46b1-139">Entity Splitting</span></span>](~/ef6/modeling/designer/entity-splitting.md)
  - [<span data-ttu-id="e46b1-140">Разделение таблицы</span><span class="sxs-lookup"><span data-stu-id="e46b1-140">Table Splitting</span></span>](~/ef6/modeling/designer/table-splitting.md)
  - <span data-ttu-id="e46b1-141">[Определение запроса](~/ef6/modeling/designer/advanced/defining-query.md) (дополнительно)</span><span class="sxs-lookup"><span data-stu-id="e46b1-141">[Defining Query](~/ef6/modeling/designer/advanced/defining-query.md) (Advanced)</span></span>
  - <span data-ttu-id="e46b1-142">[Функции с табличными значениями](~/ef6/modeling/designer/advanced/tvfs.md) (дополнительно)</span><span class="sxs-lookup"><span data-stu-id="e46b1-142">[Table-Valued Functions](~/ef6/modeling/designer/advanced/tvfs.md) (Advanced)</span></span>

## <a name="other-resources"></a><span data-ttu-id="e46b1-143">Другие источники</span><span class="sxs-lookup"><span data-stu-id="e46b1-143">Other resources</span></span>
  - [<span data-ttu-id="e46b1-144">Асинхронный запрос и сохранение</span><span class="sxs-lookup"><span data-stu-id="e46b1-144">Async Query and Save</span></span>](~/ef6/fundamentals/async.md)
  - [<span data-ttu-id="e46b1-145">Привязка данных с помощью WinForms</span><span class="sxs-lookup"><span data-stu-id="e46b1-145">Databinding with WinForms</span></span>](~/ef6/fundamentals/databinding/winforms.md)
  - [<span data-ttu-id="e46b1-146">Привязка данных с помощью WPF</span><span class="sxs-lookup"><span data-stu-id="e46b1-146">Databinding with WPF</span></span>](~/ef6/fundamentals/databinding/wpf.md)
  - <span data-ttu-id="e46b1-147">[Отключенные сценарии с самоотслеживающимися сущностями](~/ef6/fundamentals/disconnected-entities/self-tracking-entities/walkthrough.md) (больше не рекомендуется)</span><span class="sxs-lookup"><span data-stu-id="e46b1-147">[Disconnected scenarios with Self-Tracking Entities](~/ef6/fundamentals/disconnected-entities/self-tracking-entities/walkthrough.md) (This is no longer recommended)</span></span>
