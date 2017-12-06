---
title: "Типы данных, EF Core"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 9d2e647f-29e4-483b-af00-74269eb06e8f
ms.technology: entity-framework-core
uid: core/modeling/relational/data-types
ms.openlocfilehash: fd4668a3f9554eb9d3b1161d5dddce2fcdcac712
ms.sourcegitcommit: 860ec5d047342fbc4063a0de881c9861cc1f8813
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/05/2017
---
# <a name="data-types"></a><span data-ttu-id="4b605-102">Типы данных</span><span class="sxs-lookup"><span data-stu-id="4b605-102">Data Types</span></span>

> [!NOTE]  
> <span data-ttu-id="4b605-103">В этом разделе конфигурации применяется для реляционных баз данных в целом.</span><span class="sxs-lookup"><span data-stu-id="4b605-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="4b605-104">Методы расширения, показанный здесь будут доступны после установки поставщика реляционной базы данных (из-за общей *Microsoft.EntityFrameworkCore.Relational* пакета).</span><span class="sxs-lookup"><span data-stu-id="4b605-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="4b605-105">Тип данных ссылается на конкретный тип столбца, с которым сопоставлено свойство базы данных.</span><span class="sxs-lookup"><span data-stu-id="4b605-105">Data type refers to the database specific type of the column to which a property is mapped.</span></span>

## <a name="conventions"></a><span data-ttu-id="4b605-106">Соглашения</span><span class="sxs-lookup"><span data-stu-id="4b605-106">Conventions</span></span>

<span data-ttu-id="4b605-107">По соглашению поставщика базы данных выбирает тип данных, на основе типа CLR свойства.</span><span class="sxs-lookup"><span data-stu-id="4b605-107">By convention, the database provider selects a data type based on the CLR type of the property.</span></span> <span data-ttu-id="4b605-108">Он принимает во внимание другие метаданные, такие как настроенного [максимальную длину](../max-length.md), является ли свойство частью первичного ключа, и т. д.</span><span class="sxs-lookup"><span data-stu-id="4b605-108">It also takes into account other metadata, such as the configured [Maximum Length](../max-length.md), whether the property is part of a primary key, etc.</span></span>

<span data-ttu-id="4b605-109">Например, SQL Server использует `datetime2(7)` для `DateTime` свойства, и `nvarchar(max)` для `string` свойства (или `nvarchar(450)` для `string` свойства, используемые в качестве ключа).</span><span class="sxs-lookup"><span data-stu-id="4b605-109">For example, SQL Server uses `datetime2(7)` for `DateTime` properties, and `nvarchar(max)` for `string` properties (or `nvarchar(450)` for `string` properties that are used as a key).</span></span>

## <a name="data-annotations"></a><span data-ttu-id="4b605-110">Заметки к данным</span><span class="sxs-lookup"><span data-stu-id="4b605-110">Data Annotations</span></span>

<span data-ttu-id="4b605-111">Заметок к данным можно использовать для указания типа данных для столбца.</span><span class="sxs-lookup"><span data-stu-id="4b605-111">You can use Data Annotations to specify an exact data type for a column.</span></span>

<span data-ttu-id="4b605-112">Например, следующий код настраивает `Url` как строка не в Юникоде с максимальной длиной `200` и `Rating` как десятичное число с точностью `5` и масштабирование объекта `2`.</span><span class="sxs-lookup"><span data-stu-id="4b605-112">For example the following code configures `Url` as a non-unicode string with maximum length of `200` and `Rating` as decimal with precision of `5` and scale of `2`.</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/DataAnnotations/Samples/Relational/DataType.cs?name=Entities&highlight=4,6)]

## <a name="fluent-api"></a><span data-ttu-id="4b605-113">Fluent API</span><span class="sxs-lookup"><span data-stu-id="4b605-113">Fluent API</span></span>

<span data-ttu-id="4b605-114">Fluent API также можно указать одинаковые типы данных для столбцов.</span><span class="sxs-lookup"><span data-stu-id="4b605-114">You can also use the Fluent API to specify the same data types for the columns.</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Samples/Relational/DataType.cs?name=Model&highlight=9-10)]
