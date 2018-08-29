---
title: Типы данных — EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 9d2e647f-29e4-483b-af00-74269eb06e8f
uid: core/modeling/relational/data-types
ms.openlocfilehash: 9060f66c752be01090ce40be6bf3a32f348ce571
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/27/2018
ms.locfileid: "42993525"
---
# <a name="data-types"></a><span data-ttu-id="2d90b-102">Типы данных</span><span class="sxs-lookup"><span data-stu-id="2d90b-102">Data Types</span></span>

> [!NOTE]  
> <span data-ttu-id="2d90b-103">Описанная в этом разделе конфигурации применяется к реляционным базам данных в общем случае.</span><span class="sxs-lookup"><span data-stu-id="2d90b-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="2d90b-104">Показанные здесь методы расширения будут доступны после установки поставщика реляционной базы данных (посредством общего *пакета Microsoft.EntityFrameworkCore.Relational*).</span><span class="sxs-lookup"><span data-stu-id="2d90b-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="2d90b-105">Тип данных относится к конкретному типу базы данных столбца, с которым сопоставлено свойство.</span><span class="sxs-lookup"><span data-stu-id="2d90b-105">Data type refers to the database specific type of the column to which a property is mapped.</span></span>

## <a name="conventions"></a><span data-ttu-id="2d90b-106">Соглашения</span><span class="sxs-lookup"><span data-stu-id="2d90b-106">Conventions</span></span>

<span data-ttu-id="2d90b-107">По соглашению поставщик базы данных выбирает тип данных, на основе типа CLR свойства.</span><span class="sxs-lookup"><span data-stu-id="2d90b-107">By convention, the database provider selects a data type based on the CLR type of the property.</span></span> <span data-ttu-id="2d90b-108">Он также принимает во внимание другие метаданные, например настроенного [Максимальная длина](../max-length.md), является ли свойство частью первичного ключа и т. д.</span><span class="sxs-lookup"><span data-stu-id="2d90b-108">It also takes into account other metadata, such as the configured [Maximum Length](../max-length.md), whether the property is part of a primary key, etc.</span></span>

<span data-ttu-id="2d90b-109">Например, SQL Server использует `datetime2(7)` для `DateTime` свойства, и `nvarchar(max)` для `string` свойства (или `nvarchar(450)` для `string` свойствах, которые используются в качестве ключа).</span><span class="sxs-lookup"><span data-stu-id="2d90b-109">For example, SQL Server uses `datetime2(7)` for `DateTime` properties, and `nvarchar(max)` for `string` properties (or `nvarchar(450)` for `string` properties that are used as a key).</span></span>

## <a name="data-annotations"></a><span data-ttu-id="2d90b-110">Заметки к данным</span><span class="sxs-lookup"><span data-stu-id="2d90b-110">Data Annotations</span></span>

<span data-ttu-id="2d90b-111">Заметки к данным можно использовать для указания свой тип данных для столбца.</span><span class="sxs-lookup"><span data-stu-id="2d90b-111">You can use Data Annotations to specify an exact data type for a column.</span></span>

<span data-ttu-id="2d90b-112">Например, приведенный ниже код настраивает `Url` как строка не в Юникоде с максимальной длиной `200` и `Rating` как тип decimal с точностью `5` и масштабирование объекта `2`.</span><span class="sxs-lookup"><span data-stu-id="2d90b-112">For example the following code configures `Url` as a non-unicode string with maximum length of `200` and `Rating` as decimal with precision of `5` and scale of `2`.</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/DataAnnotations/Samples/Relational/DataType.cs?name=Entities&highlight=4,6)]

## <a name="fluent-api"></a><span data-ttu-id="2d90b-113">Текучий API</span><span class="sxs-lookup"><span data-stu-id="2d90b-113">Fluent API</span></span>

<span data-ttu-id="2d90b-114">Fluent API также можно указать разные типы данных для столбцов.</span><span class="sxs-lookup"><span data-stu-id="2d90b-114">You can also use the Fluent API to specify the same data types for the columns.</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Samples/Relational/DataType.cs?name=Model&highlight=9-10)]
