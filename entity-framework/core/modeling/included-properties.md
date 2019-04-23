---
title: Включение и исключение свойств — EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: e9dff604-3469-4a05-8f9e-18ac281d82a9
uid: core/modeling/included-properties
ms.openlocfilehash: 022534091bb48e491c8808791a401216a339d7b0
ms.sourcegitcommit: 87fcaba46535aa351db4bdb1231bd14b40e459b9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/22/2019
ms.locfileid: "59929829"
---
# <a name="including--excluding-properties"></a><span data-ttu-id="bd874-102">Включение и исключение свойств</span><span class="sxs-lookup"><span data-stu-id="bd874-102">Including & Excluding Properties</span></span>

<span data-ttu-id="bd874-103">Включая свойства в модели означает, что EF содержит метаданные об этом свойстве и будет предпринята попытка чтения и записи значений в базе данных.</span><span class="sxs-lookup"><span data-stu-id="bd874-103">Including a property in the model means that EF has metadata about that property and will attempt to read and write values from/to the database.</span></span>

## <a name="conventions"></a><span data-ttu-id="bd874-104">Соглашения</span><span class="sxs-lookup"><span data-stu-id="bd874-104">Conventions</span></span>

<span data-ttu-id="bd874-105">По соглашению будут включены свойства getter и setter в модели.</span><span class="sxs-lookup"><span data-stu-id="bd874-105">By convention, public properties with a getter and a setter will be included in the model.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="bd874-106">Заметки к данным</span><span class="sxs-lookup"><span data-stu-id="bd874-106">Data Annotations</span></span>

<span data-ttu-id="bd874-107">Чтобы исключить свойство из модели можно использовать заметки к данным.</span><span class="sxs-lookup"><span data-stu-id="bd874-107">You can use Data Annotations to exclude a property from the model.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Samples/IgnoreProperty.cs?highlight=17)]

## <a name="fluent-api"></a><span data-ttu-id="bd874-108">Текучий API</span><span class="sxs-lookup"><span data-stu-id="bd874-108">Fluent API</span></span>

<span data-ttu-id="bd874-109">Fluent API можно использовать, чтобы исключить свойство из модели.</span><span class="sxs-lookup"><span data-stu-id="bd874-109">You can use the Fluent API to exclude a property from the model.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/IgnoreProperty.cs?highlight=12,13)]
