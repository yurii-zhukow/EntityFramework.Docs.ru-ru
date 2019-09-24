---
title: Типы данных — EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 9d2e647f-29e4-483b-af00-74269eb06e8f
uid: core/modeling/relational/data-types
ms.openlocfilehash: 26664ebe18abcdeaa2b9c8dc23a6410204f53c8e
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/23/2019
ms.locfileid: "71197182"
---
# <a name="data-types"></a><span data-ttu-id="89ecf-102">Типы данных</span><span class="sxs-lookup"><span data-stu-id="89ecf-102">Data Types</span></span>

> [!NOTE]  
> <span data-ttu-id="89ecf-103">Описанная в этом разделе конфигурации применяется к реляционным базам данных в общем случае.</span><span class="sxs-lookup"><span data-stu-id="89ecf-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="89ecf-104">Показанные здесь методы расширения будут доступны после установки поставщика реляционной базы данных (посредством общего *пакета Microsoft.EntityFrameworkCore.Relational*).</span><span class="sxs-lookup"><span data-stu-id="89ecf-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="89ecf-105">Тип данных относится к конкретному типу базы данных столбец, с которым сопоставлено свойство.</span><span class="sxs-lookup"><span data-stu-id="89ecf-105">Data type refers to the database specific type of the column to which a property is mapped.</span></span>

## <a name="conventions"></a><span data-ttu-id="89ecf-106">Соглашения</span><span class="sxs-lookup"><span data-stu-id="89ecf-106">Conventions</span></span>

<span data-ttu-id="89ecf-107">По соглашению поставщик базы данных выбирает тип данных на основе типа .NET свойства.</span><span class="sxs-lookup"><span data-stu-id="89ecf-107">By convention, the database provider selects a data type based on the .NET type of the property.</span></span> <span data-ttu-id="89ecf-108">Он также учитывает другие метаданные, такие как настроенная [Максимальная длина](../max-length.md), является ли свойство частью первичного ключа и т. д.</span><span class="sxs-lookup"><span data-stu-id="89ecf-108">It also takes into account other metadata, such as the configured [Maximum Length](../max-length.md), whether the property is part of a primary key, etc.</span></span>

<span data-ttu-id="89ecf-109">Например, SQL Server использует `datetime2(7)` для `DateTime` свойств, а `nvarchar(max)` также для `string` свойств (или `nvarchar(450)` для `string` свойств, которые используются в качестве ключа).</span><span class="sxs-lookup"><span data-stu-id="89ecf-109">For example, SQL Server uses `datetime2(7)` for `DateTime` properties, and `nvarchar(max)` for `string` properties (or `nvarchar(450)` for `string` properties that are used as a key).</span></span>

## <a name="data-annotations"></a><span data-ttu-id="89ecf-110">Заметки к данным</span><span class="sxs-lookup"><span data-stu-id="89ecf-110">Data Annotations</span></span>

<span data-ttu-id="89ecf-111">Можно использовать заметки к данным, чтобы указать точный тип данных для столбца.</span><span class="sxs-lookup"><span data-stu-id="89ecf-111">You can use Data Annotations to specify an exact data type for a column.</span></span>

<span data-ttu-id="89ecf-112">Например, `Url` следующий код настраивает в качестве строки не в Юникоде с максимальной `200` длиной и `Rating` `2`в виде десятичного значения с точностью `5` до и масштабом.</span><span class="sxs-lookup"><span data-stu-id="89ecf-112">For example the following code configures `Url` as a non-unicode string with maximum length of `200` and `Rating` as decimal with precision of `5` and scale of `2`.</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/DataAnnotations/Relational/DataType.cs?name=Entities&highlight=4,6)]

## <a name="fluent-api"></a><span data-ttu-id="89ecf-113">Текучий API</span><span class="sxs-lookup"><span data-stu-id="89ecf-113">Fluent API</span></span>

<span data-ttu-id="89ecf-114">Можно также использовать API Fluent, чтобы указать те же типы данных для столбцов.</span><span class="sxs-lookup"><span data-stu-id="89ecf-114">You can also use the Fluent API to specify the same data types for the columns.</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/DataType.cs?name=Model&highlight=9-10)]
