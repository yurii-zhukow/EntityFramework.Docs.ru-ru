---
title: Последовательности — EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 94f81a92-3c72-4e14-912a-f99310374e42
uid: core/modeling/relational/sequences
ms.openlocfilehash: b810caaffa329bb5ad6f3486145d0ade9287eada
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/06/2019
ms.locfileid: "73656113"
---
# <a name="sequences"></a><span data-ttu-id="d831e-102">Последовательности</span><span class="sxs-lookup"><span data-stu-id="d831e-102">Sequences</span></span>

> [!NOTE]  
> <span data-ttu-id="d831e-103">Описанная в этом разделе конфигурации применяется к реляционным базам данных в общем случае.</span><span class="sxs-lookup"><span data-stu-id="d831e-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="d831e-104">Показанные здесь методы расширения будут доступны после установки поставщика реляционной базы данных (посредством общего *пакета Microsoft.EntityFrameworkCore.Relational*).</span><span class="sxs-lookup"><span data-stu-id="d831e-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="d831e-105">Последовательность формирует последовательные числовые значения в базе данных.</span><span class="sxs-lookup"><span data-stu-id="d831e-105">A sequence generates a sequential numeric values in the database.</span></span> <span data-ttu-id="d831e-106">Последовательности не связаны с определенной таблицей.</span><span class="sxs-lookup"><span data-stu-id="d831e-106">Sequences are not associated with a specific table.</span></span>

## <a name="conventions"></a><span data-ttu-id="d831e-107">Соглашения</span><span class="sxs-lookup"><span data-stu-id="d831e-107">Conventions</span></span>

<span data-ttu-id="d831e-108">В соответствии с соглашением последовательности не вводятся в модель.</span><span class="sxs-lookup"><span data-stu-id="d831e-108">By convention, sequences are not introduced in to the model.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="d831e-109">Заметки к данным</span><span class="sxs-lookup"><span data-stu-id="d831e-109">Data Annotations</span></span>

<span data-ttu-id="d831e-110">Вы не можете настроить последовательность с помощью заметок к данным.</span><span class="sxs-lookup"><span data-stu-id="d831e-110">You can not configure a sequence using Data Annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="d831e-111">Текучий API</span><span class="sxs-lookup"><span data-stu-id="d831e-111">Fluent API</span></span>

<span data-ttu-id="d831e-112">Для создания последовательности в модели можно использовать API-интерфейс Fluent.</span><span class="sxs-lookup"><span data-stu-id="d831e-112">You can use the Fluent API to create a sequence in the model.</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/Sequence.cs?name=Model&highlight=7)]

<span data-ttu-id="d831e-113">Можно также настроить дополнительный аспект последовательности, например ее схему, начальное значение и шаг приращения.</span><span class="sxs-lookup"><span data-stu-id="d831e-113">You can also configure additional aspect of the sequence, such as its schema, start value, and increment.</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/SequenceConfigured.cs?name=Sequence&highlight=7,8,9)]

<span data-ttu-id="d831e-114">После того как последовательность введена, ее можно использовать для создания значений свойств в модели.</span><span class="sxs-lookup"><span data-stu-id="d831e-114">Once a sequence is introduced, you can use it to generate values for properties in your model.</span></span> <span data-ttu-id="d831e-115">Например, можно использовать [значения по умолчанию](default-values.md) для вставки следующего значения из последовательности.</span><span class="sxs-lookup"><span data-stu-id="d831e-115">For example, you can use [Default Values](default-values.md) to insert the next value from the sequence.</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/SequenceUsed.cs?name=Default&highlight=13)]
