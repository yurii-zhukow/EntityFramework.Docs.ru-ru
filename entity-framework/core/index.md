---
title: Обзор Entity Framework Core — EF Core
description: Общие вводные сведения об Entity Framework Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: bc2a2676-bc46-493f-bf49-e3cc97994d57
uid: core/index
ms.openlocfilehash: 4816f2d57590ddd2ad4807daa2cc1c5000cd5713
ms.sourcegitcommit: 7c3939504bb9da3f46bea3443638b808c04227c2
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/09/2020
ms.locfileid: "89619485"
---
# <a name="entity-framework-core"></a><span data-ttu-id="e06a3-103">Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="e06a3-103">Entity Framework Core</span></span>

<span data-ttu-id="e06a3-104">Entity Framework (EF) Core — это простая, кроссплатформенная и расширяемая версия популярной технологии доступа к данным Entity Framework [с открытым исходным кодом](https://github.com/aspnet/EntityFrameworkCore).</span><span class="sxs-lookup"><span data-stu-id="e06a3-104">Entity Framework (EF) Core is a lightweight, extensible, [open source](https://github.com/aspnet/EntityFrameworkCore) and cross-platform version of the popular Entity Framework data access technology.</span></span>

<span data-ttu-id="e06a3-105">EF Core может использоваться как объектно реляционный модуль сопоставления (O/RM), позволяя разработчикам .NET работать с базой данных с помощью объектов .NET и устраняя необходимость в написании большей части кода, требуемого для доступа к данным.</span><span class="sxs-lookup"><span data-stu-id="e06a3-105">EF Core can serve as an object-relational mapper (O/RM), enabling .NET developers to work with a database using .NET objects, and eliminating the need for most of the data-access code they usually need to write.</span></span>

<span data-ttu-id="e06a3-106">EF Core поддерживает множество систем баз данных. Дополнительные сведения см. в разделе [Поставщики баз данных](xref:core/providers/index).</span><span class="sxs-lookup"><span data-stu-id="e06a3-106">EF Core supports many database engines, see [Database Providers](xref:core/providers/index) for details.</span></span>

## <a name="the-model"></a><span data-ttu-id="e06a3-107">Модель</span><span class="sxs-lookup"><span data-stu-id="e06a3-107">The Model</span></span>

<span data-ttu-id="e06a3-108">В EF Core доступ к данным осуществляется с помощью модели.</span><span class="sxs-lookup"><span data-stu-id="e06a3-108">With EF Core, data access is performed using a model.</span></span> <span data-ttu-id="e06a3-109">Модель состоит из классов сущностей и объекта контекста, который представляет сеанс взаимодействия с базой данных, позволяя запрашивать и сохранять данные.</span><span class="sxs-lookup"><span data-stu-id="e06a3-109">A model is made up of entity classes and a context object that represents a session with the database, allowing you to query and save data.</span></span> <span data-ttu-id="e06a3-110">Дополнительные сведения см. в разделе [Создание модели](xref:core/modeling/index).</span><span class="sxs-lookup"><span data-stu-id="e06a3-110">See [Creating a Model](xref:core/modeling/index) to learn more.</span></span>

<span data-ttu-id="e06a3-111">Вы можете создать модель из существующей базы данных, вручную составить код модели, соответствующий базе данных, или использовать [миграции EF](xref:core/managing-schemas/migrations/index) для создания базы данных из модели (и ее модификации по мере изменения модели).</span><span class="sxs-lookup"><span data-stu-id="e06a3-111">You can generate a model from an existing database, hand code a model to match your database, or use [EF Migrations](xref:core/managing-schemas/migrations/index) to create a database from your model, and then evolve it as your model changes over time.</span></span>

[!code-csharp[Main](../../samples/core/Intro/Model.cs)]

## <a name="querying"></a><span data-ttu-id="e06a3-112">Выполнение запроса</span><span class="sxs-lookup"><span data-stu-id="e06a3-112">Querying</span></span>

<span data-ttu-id="e06a3-113">Экземпляры классов сущностей извлекаются из базы данных с помощью LINQ.</span><span class="sxs-lookup"><span data-stu-id="e06a3-113">Instances of your entity classes are retrieved from the database using Language Integrated Query (LINQ).</span></span> <span data-ttu-id="e06a3-114">Дополнительные сведения см. в разделе [Запросы к данным](xref:core/querying/index).</span><span class="sxs-lookup"><span data-stu-id="e06a3-114">See [Querying Data](xref:core/querying/index) to learn more.</span></span>

[!code-csharp[Main](../../samples/core/Intro/Program.cs#Querying)]

## <a name="saving-data"></a><span data-ttu-id="e06a3-115">Сохранение данных</span><span class="sxs-lookup"><span data-stu-id="e06a3-115">Saving Data</span></span>

<span data-ttu-id="e06a3-116">Для создания, удаления и изменения данных в базе данных используются экземпляры классов сущностей.</span><span class="sxs-lookup"><span data-stu-id="e06a3-116">Data is created, deleted, and modified in the database using instances of your entity classes.</span></span> <span data-ttu-id="e06a3-117">Дополнительные сведения см. в разделе [Сохранение данных](xref:core/saving/index).</span><span class="sxs-lookup"><span data-stu-id="e06a3-117">See [Saving Data](xref:core/saving/index) to learn more.</span></span>

[!code-csharp[Main](../../samples/core/Intro/Program.cs#SavingData)]

## <a name="next-steps"></a><span data-ttu-id="e06a3-118">Дальнейшие действия</span><span class="sxs-lookup"><span data-stu-id="e06a3-118">Next steps</span></span>

<span data-ttu-id="e06a3-119">Вводные руководства см. в разделе [Начало работы с Entity Framework Core](xref:core/get-started/index).</span><span class="sxs-lookup"><span data-stu-id="e06a3-119">For introductory tutorials, see [Getting Started with Entity Framework Core](xref:core/get-started/index).</span></span>
