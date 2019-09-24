---
title: Включение & за исключением свойств EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: e9dff604-3469-4a05-8f9e-18ac281d82a9
uid: core/modeling/included-properties
ms.openlocfilehash: cd111af891ef0bbaccf515eed0c1991f105bd362
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/23/2019
ms.locfileid: "71197422"
---
# <a name="including--excluding-properties"></a><span data-ttu-id="f74e2-102">Включение & за исключением свойств</span><span class="sxs-lookup"><span data-stu-id="f74e2-102">Including & Excluding Properties</span></span>

<span data-ttu-id="f74e2-103">Включение свойства в модель означает, что EF имеет метаданные об этом свойстве и будет пытаться считывать и записывать значения в базу данных.</span><span class="sxs-lookup"><span data-stu-id="f74e2-103">Including a property in the model means that EF has metadata about that property and will attempt to read and write values from/to the database.</span></span>

## <a name="conventions"></a><span data-ttu-id="f74e2-104">Соглашения</span><span class="sxs-lookup"><span data-stu-id="f74e2-104">Conventions</span></span>

<span data-ttu-id="f74e2-105">По соглашению в модель будут включаться открытые свойства с методом считывания и методом задания.</span><span class="sxs-lookup"><span data-stu-id="f74e2-105">By convention, public properties with a getter and a setter will be included in the model.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="f74e2-106">Заметки к данным</span><span class="sxs-lookup"><span data-stu-id="f74e2-106">Data Annotations</span></span>

<span data-ttu-id="f74e2-107">Заметки к данным можно использовать для исключения свойства из модели.</span><span class="sxs-lookup"><span data-stu-id="f74e2-107">You can use Data Annotations to exclude a property from the model.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/IgnoreProperty.cs?highlight=17)]

## <a name="fluent-api"></a><span data-ttu-id="f74e2-108">Текучий API</span><span class="sxs-lookup"><span data-stu-id="f74e2-108">Fluent API</span></span>

<span data-ttu-id="f74e2-109">API-интерфейс Fluent можно использовать для исключения свойства из модели.</span><span class="sxs-lookup"><span data-stu-id="f74e2-109">You can use the Fluent API to exclude a property from the model.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/IgnoreProperty.cs?highlight=12,13)]
