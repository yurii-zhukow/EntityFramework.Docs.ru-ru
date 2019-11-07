---
title: Вычисленные столбцы — EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: e9d81f06-805d-45c9-97c2-3546df654829
uid: core/modeling/relational/computed-columns
ms.openlocfilehash: 4e92ed6d785f3b7bdf54015101bdcddb9e4e0e1c
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/06/2019
ms.locfileid: "73655918"
---
# <a name="computed-columns"></a><span data-ttu-id="1bc13-102">Вычисляемые столбцы</span><span class="sxs-lookup"><span data-stu-id="1bc13-102">Computed Columns</span></span>

> [!NOTE]  
> <span data-ttu-id="1bc13-103">Описанная в этом разделе конфигурации применяется к реляционным базам данных в общем случае.</span><span class="sxs-lookup"><span data-stu-id="1bc13-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="1bc13-104">Показанные здесь методы расширения будут доступны после установки поставщика реляционной базы данных (посредством общего *пакета Microsoft.EntityFrameworkCore.Relational*).</span><span class="sxs-lookup"><span data-stu-id="1bc13-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="1bc13-105">Вычисляемый столбец — это столбец, значение которого вычисляется в базе данных.</span><span class="sxs-lookup"><span data-stu-id="1bc13-105">A computed column is a column whose value is calculated in the database.</span></span> <span data-ttu-id="1bc13-106">Вычисляемый столбец может использовать другие столбцы таблицы для вычисления ее значения.</span><span class="sxs-lookup"><span data-stu-id="1bc13-106">A computed column can use other columns in the table to calculate its value.</span></span>

## <a name="conventions"></a><span data-ttu-id="1bc13-107">Соглашения</span><span class="sxs-lookup"><span data-stu-id="1bc13-107">Conventions</span></span>

<span data-ttu-id="1bc13-108">По соглашению, вычисленные столбцы не создаются в модели.</span><span class="sxs-lookup"><span data-stu-id="1bc13-108">By convention, computed columns are not created in the model.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="1bc13-109">Заметки к данным</span><span class="sxs-lookup"><span data-stu-id="1bc13-109">Data Annotations</span></span>

<span data-ttu-id="1bc13-110">Вычисленные столбцы не могут быть настроены с помощью заметок к данным.</span><span class="sxs-lookup"><span data-stu-id="1bc13-110">Computed columns can not be configured with Data Annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="1bc13-111">Текучий API</span><span class="sxs-lookup"><span data-stu-id="1bc13-111">Fluent API</span></span>

<span data-ttu-id="1bc13-112">API-интерфейс Fluent можно использовать для указания того, что свойство должно сопоставляться с вычисляемым столбцом.</span><span class="sxs-lookup"><span data-stu-id="1bc13-112">You can use the Fluent API to specify that a property should map to a computed column.</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/ComputedColumn.cs?name=ComputedColumn&highlight=9)]
