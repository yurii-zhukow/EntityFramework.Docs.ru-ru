---
title: Обзор Entity Framework 6 — EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 8ae74d63-6bad-4686-b325-bbf9d68f3743
uid: ef6/index
ms.openlocfilehash: 28a13879416a52cbe8035c23013f16390c75c4c9
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/06/2019
ms.locfileid: "73656183"
---
# <a name="entity-framework-6"></a><span data-ttu-id="e3e3a-102">Entity Framework 6</span><span class="sxs-lookup"><span data-stu-id="e3e3a-102">Entity Framework 6</span></span>
<span data-ttu-id="e3e3a-103">Entity Framework 6 (EF6) — это проверенное средство объектно-реляционного сопоставления (O/RM) для .NET, которое разрабатывалось и совершенствовалось в течение нескольких лет.</span><span class="sxs-lookup"><span data-stu-id="e3e3a-103">Entity Framework 6 (EF6) is a tried and tested object-relational mapper (O/RM) for .NET with many years of feature development and stabilization.</span></span>

<span data-ttu-id="e3e3a-104">В качестве O/RM EF6 уменьшает несогласованность между реляционным и объектно ориентированным мирами, позволяя разработчикам создавать приложения, которые взаимодействуют с данными, хранящимися в реляционных базах данных, с помощью строго типизированных объектов .NET, представляющих прикладную область, и устраняя необходимость писать большой объем инфраструктурного кода для доступа к данным.</span><span class="sxs-lookup"><span data-stu-id="e3e3a-104">As an O/RM, EF6 reduces the impedance mismatch between the relational and object-oriented worlds, enabling developers to write applications that interact with data stored in relational databases using strongly-typed .NET objects that represent the application's domain, and eliminating the need for a large portion of the data access "plumbing" code that they usually need to write.</span></span>

<span data-ttu-id="e3e3a-105">В EF6 реализованы многие популярные возможности O/RM:</span><span class="sxs-lookup"><span data-stu-id="e3e3a-105">EF6 implements many popular O/RM features:</span></span>
- <span data-ttu-id="e3e3a-106">Сопоставление классов сущностей [POCO](xref:ef6/resources/glossary#poco) не зависит ни от каких типов EF</span><span class="sxs-lookup"><span data-stu-id="e3e3a-106">Mapping of [POCO](xref:ef6/resources/glossary#poco) entity classes which do not depend on any EF types</span></span>
- <span data-ttu-id="e3e3a-107">Автоматическое отслеживание изменений.</span><span class="sxs-lookup"><span data-stu-id="e3e3a-107">Automatic change tracking</span></span>
- <span data-ttu-id="e3e3a-108">Разрешение идентификаторов и единицы работы.</span><span class="sxs-lookup"><span data-stu-id="e3e3a-108">Identity resolution and Unit of Work</span></span>
- <span data-ttu-id="e3e3a-109">Безотложная, отложенная и явная загрузка.</span><span class="sxs-lookup"><span data-stu-id="e3e3a-109">Eager, lazy and explicit loading</span></span>
- <span data-ttu-id="e3e3a-110">Преобразование строго типизированных запросов с помощью [LINQ (Language INtegrated Query)](https://aka.ms/AA6hsvu)</span><span class="sxs-lookup"><span data-stu-id="e3e3a-110">Translation of strongly-typed queries using [LINQ (Language INtegrated Query)](https://aka.ms/AA6hsvu)</span></span>
- <span data-ttu-id="e3e3a-111">Широкие возможности сопоставления, включая поддержку следующих элементов:</span><span class="sxs-lookup"><span data-stu-id="e3e3a-111">Rich mapping capabilities, including support for:</span></span>
  - <span data-ttu-id="e3e3a-112">Отношения "один к одному", "один ко многим" и "многие ко многим"</span><span class="sxs-lookup"><span data-stu-id="e3e3a-112">One-to-one, one-to-many and many-to-many relationships</span></span>
  - <span data-ttu-id="e3e3a-113">Наследование (одна таблица на иерархию, одна таблица на тип и одна таблица на конкретный класс).</span><span class="sxs-lookup"><span data-stu-id="e3e3a-113">Inheritance (table per hierarchy, table per type and table per concrete class)</span></span>
  - <span data-ttu-id="e3e3a-114">Сложные типы</span><span class="sxs-lookup"><span data-stu-id="e3e3a-114">Complex types</span></span>
  - <span data-ttu-id="e3e3a-115">Хранимые процедуры</span><span class="sxs-lookup"><span data-stu-id="e3e3a-115">Stored procedures</span></span>
- <span data-ttu-id="e3e3a-116">Визуальный конструктор для создания моделей сущностей.</span><span class="sxs-lookup"><span data-stu-id="e3e3a-116">A visual designer to create entity models.</span></span>
- <span data-ttu-id="e3e3a-117">Интерфейс Code First для создания моделей сущностей путем написания кода.</span><span class="sxs-lookup"><span data-stu-id="e3e3a-117">A "Code First" experience to create entity models by writing code.</span></span>
- <span data-ttu-id="e3e3a-118">Модели можно создавать на основе существующих баз данных и затем редактировать вручную или же создавать с нуля и затем использовать для создания новых баз данных.</span><span class="sxs-lookup"><span data-stu-id="e3e3a-118">Models can either be generated from existing databases and then hand-edited, or they can be created from scratch and then used to generate new databases.</span></span>
- <span data-ttu-id="e3e3a-119">Интеграция с моделями приложений .NET Framework, включая ASP.NET, и через привязку данных с помощью WPF и WinForms.</span><span class="sxs-lookup"><span data-stu-id="e3e3a-119">Integration with .NET Framework application models, including ASP.NET, and through databinding, with WPF and WinForms.</span></span>
- <span data-ttu-id="e3e3a-120">Возможность подключения к базам данных SQL Server, Oracle, MySQL, SQLite, PostgreSQL, DB2 и т. д. на основе ADO.NET и различных [поставщиков](xref:ef6/fundamentals/providers/index).</span><span class="sxs-lookup"><span data-stu-id="e3e3a-120">Database connectivity based on ADO.NET and numerous [providers](xref:ef6/fundamentals/providers/index) available to connect to SQL Server, Oracle, MySQL, SQLite, PostgreSQL, DB2, etc.</span></span>

## <a name="should-i-use-ef6-or-ef-core"></a><span data-ttu-id="e3e3a-121">Что выбрать — EF6 или EF Core?</span><span class="sxs-lookup"><span data-stu-id="e3e3a-121">Should I use EF6 or EF Core?</span></span>

<span data-ttu-id="e3e3a-122">EF Core — это более современная, упрощенная и расширяемая версия Entity Framework, почти аналогичная EF6 по возможностям и преимуществам.</span><span class="sxs-lookup"><span data-stu-id="e3e3a-122">EF Core is a more modern, lightweight and extensible version of Entity Framework that has very similar capabilities and benefits to EF6.</span></span>
<span data-ttu-id="e3e3a-123">EF Core была создана с нуля и содержит новые возможности, недоступные в EF6, хотя в ней пока отсутствуют некоторые расширенные возможности сопоставления из EF6.</span><span class="sxs-lookup"><span data-stu-id="e3e3a-123">EF Core is a complete rewrite and contains many new features not available in EF6, although it also still lacks some of the most advanced mapping capabilities of EF6.</span></span>
<span data-ttu-id="e3e3a-124">Используйте EF Core в новых приложениях, если набор функций соответствует вашим требованиям.</span><span class="sxs-lookup"><span data-stu-id="e3e3a-124">Consider using EF Core in new applications if the feature set matches your requirements.</span></span>
<span data-ttu-id="e3e3a-125">В разделе [Сравнение EF Core и EF6](xref:efcore-and-ef6/index) подробно описывается, как сделать выбор.</span><span class="sxs-lookup"><span data-stu-id="e3e3a-125">[Compare EF Core & EF6](xref:efcore-and-ef6/index) examines this choice in greater detail.</span></span>

## <a name="get-startedxrefef6get-started"></a>[<span data-ttu-id="e3e3a-126">Начало работы</span><span class="sxs-lookup"><span data-stu-id="e3e3a-126">Get Started</span></span>](xref:ef6/get-started)

<span data-ttu-id="e3e3a-127">Добавьте в проект пакет NuGet EntityFramework или установите [Entity Framework Tools для Visual Studio](https://aka.ms/AA6i8c5).</span><span class="sxs-lookup"><span data-stu-id="e3e3a-127">Add the EntityFramework NuGet package to your project or install the [Entity Framework Tools for Visual Studio](https://aka.ms/AA6i8c5).</span></span> <span data-ttu-id="e3e3a-128">А затем смотрите видео, читайте руководства и знакомьтесь с расширенной документацией, чтобы научиться использовать все возможности Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="e3e3a-128">Then watch videos, read tutorials, and advanced documentation to help you make the most of EF6.</span></span>

## <a name="past-entity-framework-versions"></a><span data-ttu-id="e3e3a-129">Предыдущие версии Entity Framework</span><span class="sxs-lookup"><span data-stu-id="e3e3a-129">Past Entity Framework Versions</span></span>

<span data-ttu-id="e3e3a-130">Это документация по последней версии Entity Framework 6, но по большей части она применима и к предыдущим выпускам.</span><span class="sxs-lookup"><span data-stu-id="e3e3a-130">This is the documentation for the latest version of Entity Framework 6, although much of it also applies to past releases.</span></span>
<span data-ttu-id="e3e3a-131">Прочитайте разделы [Новые возможности](xref:ef6/what-is-new/index) и [Прошлые выпуски](xref:ef6/what-is-new/past-releases), чтобы узнать обо всех выпусках EF и их возможностях.</span><span class="sxs-lookup"><span data-stu-id="e3e3a-131">Check out [What's New](xref:ef6/what-is-new/index) and [Past Releases](xref:ef6/what-is-new/past-releases) for a complete list of EF releases and the features they introduced.</span></span>
