---
title: Обзор Entity Framework Core — EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: bc2a2676-bc46-493f-bf49-e3cc97994d57
uid: core/index
ms.openlocfilehash: e6127f775d6bbbdf81debf5519388fe252fe079d
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/06/2020
ms.locfileid: "78412839"
---
# <a name="entity-framework-core"></a><span data-ttu-id="08d4f-102">Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="08d4f-102">Entity Framework Core</span></span>

<span data-ttu-id="08d4f-103">Entity Framework (EF) Core — это простая, кроссплатформенная и расширяемая версия популярной технологии доступа к данным Entity Framework [с открытым исходным кодом](https://github.com/aspnet/EntityFrameworkCore).</span><span class="sxs-lookup"><span data-stu-id="08d4f-103">Entity Framework (EF) Core is a lightweight, extensible, [open source](https://github.com/aspnet/EntityFrameworkCore) and cross-platform version of the popular Entity Framework data access technology.</span></span>

<span data-ttu-id="08d4f-104">EF Core может использоваться как объектно реляционный модуль сопоставления (O/RM), позволяя разработчикам .NET работать с базой данных с помощью объектов .NET и устраняя необходимость в написании большей части кода, требуемого для доступа к данным.</span><span class="sxs-lookup"><span data-stu-id="08d4f-104">EF Core can serve as an object-relational mapper (O/RM), enabling .NET developers to work with a database using .NET objects, and eliminating the need for most of the data-access code they usually need to write.</span></span>

<span data-ttu-id="08d4f-105">EF Core поддерживает множество систем баз данных. Дополнительные сведения см. в разделе [Поставщики баз данных](providers/index.md).</span><span class="sxs-lookup"><span data-stu-id="08d4f-105">EF Core supports many database engines, see [Database Providers](providers/index.md) for details.</span></span>

## <a name="the-model"></a><span data-ttu-id="08d4f-106">Модель</span><span class="sxs-lookup"><span data-stu-id="08d4f-106">The Model</span></span>

<span data-ttu-id="08d4f-107">В EF Core доступ к данным осуществляется с помощью модели.</span><span class="sxs-lookup"><span data-stu-id="08d4f-107">With EF Core, data access is performed using a model.</span></span> <span data-ttu-id="08d4f-108">Модель состоит из классов сущностей и объекта контекста, который представляет сеанс взаимодействия с базой данных, позволяя запрашивать и сохранять данные.</span><span class="sxs-lookup"><span data-stu-id="08d4f-108">A model is made up of entity classes and a context object that represents a session with the database, allowing you to query and save data.</span></span> <span data-ttu-id="08d4f-109">Дополнительные сведения см. в разделе [Создание модели](modeling/index.md).</span><span class="sxs-lookup"><span data-stu-id="08d4f-109">See [Creating a Model](modeling/index.md) to learn more.</span></span>

<span data-ttu-id="08d4f-110">Вы можете создать модель из существующей базы данных, вручную составить код модели, соответствующий базе данных, или использовать [миграции EF](managing-schemas/migrations/index.md) для создания базы данных из модели (и ее модификации по мере изменения модели).</span><span class="sxs-lookup"><span data-stu-id="08d4f-110">You can generate a model from an existing database, hand code a model to match your database, or use [EF Migrations](managing-schemas/migrations/index.md) to create a database from your model, and then evolve it as your model changes over time.</span></span>

[!code-csharp[Main](../../samples/core/Intro/Model.cs)]

## <a name="querying"></a><span data-ttu-id="08d4f-111">Выполнение запросов</span><span class="sxs-lookup"><span data-stu-id="08d4f-111">Querying</span></span>

<span data-ttu-id="08d4f-112">Экземпляры классов сущностей извлекаются из базы данных с помощью LINQ.</span><span class="sxs-lookup"><span data-stu-id="08d4f-112">Instances of your entity classes are retrieved from the database using Language Integrated Query (LINQ).</span></span> <span data-ttu-id="08d4f-113">Дополнительные сведения см. в разделе [Запросы к данным](querying/index.md).</span><span class="sxs-lookup"><span data-stu-id="08d4f-113">See [Querying Data](querying/index.md) to learn more.</span></span>

[!code-csharp[Main](../../samples/core/Intro/Program.cs#Querying)]

## <a name="saving-data"></a><span data-ttu-id="08d4f-114">Сохранение данных</span><span class="sxs-lookup"><span data-stu-id="08d4f-114">Saving Data</span></span>

<span data-ttu-id="08d4f-115">Для создания, удаления и изменения данных в базе данных используются экземпляры классов сущностей.</span><span class="sxs-lookup"><span data-stu-id="08d4f-115">Data is created, deleted, and modified in the database using instances of your entity classes.</span></span> <span data-ttu-id="08d4f-116">Дополнительные сведения см. в разделе [Сохранение данных](saving/index.md).</span><span class="sxs-lookup"><span data-stu-id="08d4f-116">See [Saving Data](saving/index.md) to learn more.</span></span>

[!code-csharp[Main](../../samples/core/Intro/Program.cs#SavingData)]

## <a name="next-steps"></a><span data-ttu-id="08d4f-117">Следующие шаги</span><span class="sxs-lookup"><span data-stu-id="08d4f-117">Next steps</span></span>

<span data-ttu-id="08d4f-118">Вводные руководства см. в разделе [Начало работы с Entity Framework Core](get-started/index.md).</span><span class="sxs-lookup"><span data-stu-id="08d4f-118">For introductory tutorials, see [Getting Started with Entity Framework Core](get-started/index.md).</span></span>
