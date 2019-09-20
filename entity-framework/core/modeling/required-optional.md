---
title: Обязательные/необязательные свойства — EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: ddaa0a54-9f43-4c34-aae3-f95c96c69842
uid: core/modeling/required-optional
ms.openlocfilehash: 7200cd2eeeba2f22365ef09b1f50edd077240130
ms.sourcegitcommit: cbaa6cc89bd71d5e0bcc891e55743f0e8ea3393b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/20/2019
ms.locfileid: "71149144"
---
# <a name="required-and-optional-properties"></a><span data-ttu-id="3f53b-102">Обязательные и необязательные свойства</span><span class="sxs-lookup"><span data-stu-id="3f53b-102">Required and Optional Properties</span></span>

<span data-ttu-id="3f53b-103">Свойство считается необязательным, если для него допустимо значение `null`.</span><span class="sxs-lookup"><span data-stu-id="3f53b-103">A property is considered optional if it is valid for it to contain `null`.</span></span> <span data-ttu-id="3f53b-104">Если `null` не является допустимым значением свойства, свойство считается обязательным.</span><span class="sxs-lookup"><span data-stu-id="3f53b-104">If `null` is not a valid value to be assigned to a property then it is considered to be a required property.</span></span>

## <a name="conventions"></a><span data-ttu-id="3f53b-105">Соглашения</span><span class="sxs-lookup"><span data-stu-id="3f53b-105">Conventions</span></span>

<span data-ttu-id="3f53b-106">По соглашению свойство, тип .NET которого может содержать значение null, будет настроен как необязательный `byte[]`(`string`, `int?`, и т. д.).</span><span class="sxs-lookup"><span data-stu-id="3f53b-106">By convention, a property whose .NET type can contain null will be configured as optional (`string`, `int?`, `byte[]`, etc.).</span></span> <span data-ttu-id="3f53b-107">Свойства, для которых CLR-тип не может иметь значение null, будут настроены как обязательные (`int`, `decimal`, `bool`и т. д.).</span><span class="sxs-lookup"><span data-stu-id="3f53b-107">Properties whose CLR type cannot contain null will be configured as required (`int`, `decimal`, `bool`, etc.).</span></span>

> [!NOTE]  
> <span data-ttu-id="3f53b-108">Свойство, тип .NET которого не может содержать значение null, не может быть настроено как необязательный.</span><span class="sxs-lookup"><span data-stu-id="3f53b-108">A property whose .NET type cannot contain null cannot be configured as optional.</span></span> <span data-ttu-id="3f53b-109">Такое свойство всегда будет расцениваться Entity Framework как обязательное.</span><span class="sxs-lookup"><span data-stu-id="3f53b-109">The property will always be considered required by Entity Framework.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="3f53b-110">Заметки к данным</span><span class="sxs-lookup"><span data-stu-id="3f53b-110">Data Annotations</span></span>

<span data-ttu-id="3f53b-111">Можно использовать заметки к данным, чтобы указать, что свойство является обязательным.</span><span class="sxs-lookup"><span data-stu-id="3f53b-111">You can use Data Annotations to indicate that a property is required.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Samples/Required.cs?highlight=14)]

## <a name="fluent-api"></a><span data-ttu-id="3f53b-112">Текучий API</span><span class="sxs-lookup"><span data-stu-id="3f53b-112">Fluent API</span></span>

<span data-ttu-id="3f53b-113">Fluent API можно использовать для указания того, что свойство является обязательным.</span><span class="sxs-lookup"><span data-stu-id="3f53b-113">You can use the Fluent API to indicate that a property is required.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/Required.cs?highlight=11-13)]

