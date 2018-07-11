---
title: Краткий обзор Entity Framework 6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: 8ae74d63-6bad-4686-b325-bbf9d68f3743
caps.latest.revision: 5
uid: ef6/index
ms.openlocfilehash: df661f19afdeef53257c86bdd32b1444737c9b0a
ms.sourcegitcommit: 45494121254ad4fdcec613d1dd22d850068d6f39
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/08/2018
ms.locfileid: "37913503"
---
# <a name="entity-framework-6-quick-overview"></a><span data-ttu-id="2658d-102">Краткий обзор Entity Framework 6</span><span class="sxs-lookup"><span data-stu-id="2658d-102">Entity Framework 6 Quick Overview</span></span>

<span data-ttu-id="2658d-103">Entity Framework 6 (EF6) — это проверенное средство объектно-реляционного сопоставления (O/RM) для .NET, которое разрабатывалось и совершенствовалось в течение нескольких лет.</span><span class="sxs-lookup"><span data-stu-id="2658d-103">Entity Framework 6 (EF6) is a tried and tested object-relational mapper (O/RM) for .NET with many years of feature development and stabilization.</span></span>

<span data-ttu-id="2658d-104">В EF6 реализованы многие популярные возможности O/RM:</span><span class="sxs-lookup"><span data-stu-id="2658d-104">EF6 implements many popular O/RM features:</span></span>
- <span data-ttu-id="2658d-105">Сопоставление игнорирующих сохраняемость классов сущностей (также известных как POCO), которые не зависят от каких-либо типов EF.</span><span class="sxs-lookup"><span data-stu-id="2658d-105">Mapping of "persistence ignorant" (also known as "POCO", for Plan-Old CLR Object) entity classes which do not depend on any EF types</span></span>
- <span data-ttu-id="2658d-106">Автоматическое отслеживание изменений.</span><span class="sxs-lookup"><span data-stu-id="2658d-106">Automatic change tracking</span></span>
- <span data-ttu-id="2658d-107">Разрешение идентификаторов и единицы работы.</span><span class="sxs-lookup"><span data-stu-id="2658d-107">Identity resolution and Unit of Work</span></span>
- <span data-ttu-id="2658d-108">Безотложная, отложенная и явная загрузка.</span><span class="sxs-lookup"><span data-stu-id="2658d-108">Eager, lazy and explicit loading</span></span>
- <span data-ttu-id="2658d-109">Преобразование строго типизированных запросов с помощью LINQ (Language INtegrated Query).</span><span class="sxs-lookup"><span data-stu-id="2658d-109">Translation of strongly-typed queries using LINQ (Language INtegrated Query)</span></span> 
- <span data-ttu-id="2658d-110">Широкие возможности сопоставления, включая поддержку следующих элементов:</span><span class="sxs-lookup"><span data-stu-id="2658d-110">Rich mapping capabilities, including support for:</span></span>
  - <span data-ttu-id="2658d-111">Наследование (одна таблица на иерархию, одна таблица на тип и одна таблица на конкретный класс).</span><span class="sxs-lookup"><span data-stu-id="2658d-111">Inheritance (table per hierarchy, table per type and table per concrete class)</span></span>
  - <span data-ttu-id="2658d-112">Сложные типы</span><span class="sxs-lookup"><span data-stu-id="2658d-112">Complex types</span></span>
  - <span data-ttu-id="2658d-113">Хранимые процедуры</span><span class="sxs-lookup"><span data-stu-id="2658d-113">Stored procedures</span></span>
- <span data-ttu-id="2658d-114">Визуальный конструктор для создания моделей сущностей.</span><span class="sxs-lookup"><span data-stu-id="2658d-114">A visual designer to create entity models.</span></span>
- <span data-ttu-id="2658d-115">Интерфейс Code First, который поддерживает создание моделей сущностей путем написания кода.</span><span class="sxs-lookup"><span data-stu-id="2658d-115">A "Code First" experience that supports creating entity models by writing code.</span></span>
- <span data-ttu-id="2658d-116">Модели можно создавать на основе существующих баз данных и затем редактировать вручную или же создавать с нуля и затем использовать для создания новых баз данных.</span><span class="sxs-lookup"><span data-stu-id="2658d-116">Models can either be generated form existing databases and then hand-edited, or they can be created from scratch and then used to generate new databases.</span></span>
- <span data-ttu-id="2658d-117">Интеграция с моделями приложений .NET Framework, включая ASP.NET, и через привязку данных с помощью WPF и WinForms.</span><span class="sxs-lookup"><span data-stu-id="2658d-117">Integration with .NET Framework application models, including ASP.NET, and through databinding, with WPF and WinForms.</span></span>
- <span data-ttu-id="2658d-118">Возможность подключения к базам данных SQL Server, Oracle, MySQL, SQLite, PostgreSQL, DB2 и т. д. на основе ADO.NET и различных поставщиков.</span><span class="sxs-lookup"><span data-stu-id="2658d-118">Database connectivity based on ADO.NET and numerous providers available to connect to SQL Server, Oracle, MySQL, SQLite, PostgreSQL, DB2, etc.</span></span>

<span data-ttu-id="2658d-119">В качестве O/RM EF6 уменьшает несогласованность между реляционным и объектно ориентированным мирами, позволяя разработчикам создавать приложения, которые взаимодействуют с данными, хранящимися в реляционных базах данных, с помощью строго типизированных объектов .NET, представляющих прикладную область, и устраняя необходимость писать большой объем инфраструктурного кода для доступа к данным.</span><span class="sxs-lookup"><span data-stu-id="2658d-119">As an O/RM, EF6 reduces the impedance mismatch between the relational and object-oriented worlds, enabling developers to write applications that interact with data stored in relational databases using strongly-typed .NET objects that represent the application's domain, and eliminating the need for a large portion of the data access "plumbing" code that they usually need to write.</span></span>

## <a name="should-i-use-ef6-or-ef-core"></a><span data-ttu-id="2658d-120">Что выбрать — EF6 или EF Core?</span><span class="sxs-lookup"><span data-stu-id="2658d-120">Should I use EF6 or EF Core?</span></span>

<span data-ttu-id="2658d-121">EF Core — это более современная, упрощенная и расширяемая версия Entity Framework, почти аналогичная EF6 по возможностям и преимуществам.</span><span class="sxs-lookup"><span data-stu-id="2658d-121">EF Core is a more modern, lightweight and extensible version of Entity Framework that has very similar capabilities and benefits to EF6.</span></span>
<span data-ttu-id="2658d-122">EF Core была создана с нуля и содержит новые возможности, недоступные в EF6, хотя в ней пока отсутствуют некоторые расширенные возможности сопоставления из EF6.</span><span class="sxs-lookup"><span data-stu-id="2658d-122">EF Core is a complete rewrite and contains many new features not available in EF6, although it also still lacks some of the most advanced mapping capabilities of EF6.</span></span>
<span data-ttu-id="2658d-123">Мы рекомендуем использовать EF Core в новых приложениях, если набор функций соответствует вашим требованиям.</span><span class="sxs-lookup"><span data-stu-id="2658d-123">We recommend using EF Core in new applications as long as the feature set matches your requirements.</span></span>
<span data-ttu-id="2658d-124">В разделе [Сравнение EF Core и EF6](xref:efcore-and-ef6/index) подробно описывается, как сделать выбор.</span><span class="sxs-lookup"><span data-stu-id="2658d-124">[Compare EF Core & EF6](xref:efcore-and-ef6/index) examines this choice in greater detail.</span></span>

## <a name="get-startedef6get-startedmd"></a>[<span data-ttu-id="2658d-125">Начало работы</span><span class="sxs-lookup"><span data-stu-id="2658d-125">Get Started</span></span>](~/ef6/get-started.md)

<span data-ttu-id="2658d-126">Добавьте в проект пакет NuGet EntityFramework или установите Entity Framework Tools для Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="2658d-126">Add the EntityFramework NuGet package to your project or install the Entity Framework Tools for Visual Studio.</span></span> <span data-ttu-id="2658d-127">Затем посмотрите видеоролики, прочитайте руководства и расширенную документацию для эффективного использования Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="2658d-127">Then watch videos, read tutorials, and advanced documentation to help you make the most of Entity Framework 6.</span></span>

## <a name="past-entity-framework-versions"></a><span data-ttu-id="2658d-128">Предыдущие версии Entity Framework</span><span class="sxs-lookup"><span data-stu-id="2658d-128">Past Entity Framework Versions</span></span>

<span data-ttu-id="2658d-129">Это документация по последней версии Entity Framework 6, но по большей части она применима и к предыдущим выпускам.</span><span class="sxs-lookup"><span data-stu-id="2658d-129">This is the documentation for the latest version of Entity Framework 6, although much of it also applies to past releases.</span></span>
<span data-ttu-id="2658d-130">Прочитайте разделы [Новые возможности](~/ef6/what-is-new/index.md) и [Прошлые выпуски](~/ef6/what-is-new/past-releases.md), чтобы узнать обо всех выпусках EF и их возможностях.</span><span class="sxs-lookup"><span data-stu-id="2658d-130">Check out [What's New](~/ef6/what-is-new/index.md) and [Past Releases](~/ef6/what-is-new/past-releases.md) for a complete list of EF releases and the features they introduced.</span></span>
