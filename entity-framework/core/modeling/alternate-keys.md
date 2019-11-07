---
title: Альтернативные ключи — EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 8a5931d4-b480-4298-af36-0e29d74a37c0
uid: core/modeling/alternate-keys
ms.openlocfilehash: e5bb0602adb465435c8e88d045395a9424b2d9a3
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/06/2019
ms.locfileid: "73655777"
---
# <a name="alternate-keys"></a><span data-ttu-id="8eab8-102">Альтернативные ключи</span><span class="sxs-lookup"><span data-stu-id="8eab8-102">Alternate Keys</span></span>

<span data-ttu-id="8eab8-103">Альтернативный ключ служит в качестве альтернативного уникального идентификатора для каждого экземпляра сущности в дополнение к первичному ключу.</span><span class="sxs-lookup"><span data-stu-id="8eab8-103">An alternate key serves as an alternate unique identifier for each entity instance in addition to the primary key.</span></span> <span data-ttu-id="8eab8-104">Альтернативные ключи можно использовать в качестве целевого объекта связи.</span><span class="sxs-lookup"><span data-stu-id="8eab8-104">Alternate keys can be used as the target of a relationship.</span></span> <span data-ttu-id="8eab8-105">При использовании реляционной базы данных эта схема сопоставляется с концепцией уникального индекса или ограничения для альтернативных ключевых столбцов и одного или нескольких ограничений внешнего ключа, которые ссылаются на эти столбцы.</span><span class="sxs-lookup"><span data-stu-id="8eab8-105">When using a relational database this maps to the concept of a unique index/constraint on the alternate key column(s) and one or more foreign key constraints that reference the column(s).</span></span>

> [!TIP]  
> <span data-ttu-id="8eab8-106">Если нужно просто обеспечить уникальность столбца, тогда вам нужен уникальный индекс, а не альтернативный ключ, см. раздел [индексы](indexes.md).</span><span class="sxs-lookup"><span data-stu-id="8eab8-106">If you just want to enforce uniqueness of a column then you want a unique index rather than an alternate key, see [Indexes](indexes.md).</span></span> <span data-ttu-id="8eab8-107">В EF альтернативные ключи предоставляют больше функций, чем уникальные индексы, так как их можно использовать в качестве целевого объекта внешнего ключа.</span><span class="sxs-lookup"><span data-stu-id="8eab8-107">In EF, alternate keys provide greater functionality than unique indexes because they can be used as the target of a foreign key.</span></span>

<span data-ttu-id="8eab8-108">Альтернативные ключи обычно вводятся при необходимости, и их не нужно настраивать вручную.</span><span class="sxs-lookup"><span data-stu-id="8eab8-108">Alternate keys are typically introduced for you when needed and you do not need to manually configure them.</span></span> <span data-ttu-id="8eab8-109">Дополнительные сведения см. в разделе [соглашения](#conventions) .</span><span class="sxs-lookup"><span data-stu-id="8eab8-109">See [Conventions](#conventions) for more details.</span></span>

## <a name="conventions"></a><span data-ttu-id="8eab8-110">Соглашения</span><span class="sxs-lookup"><span data-stu-id="8eab8-110">Conventions</span></span>

<span data-ttu-id="8eab8-111">По соглашению альтернативный ключ вводится при определении свойства, которое не является первичным ключом, в качестве целевого объекта связи.</span><span class="sxs-lookup"><span data-stu-id="8eab8-111">By convention, an alternate key is introduced for you when you identify a property, that is not the primary key, as the target of a relationship.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/AlternateKey.cs?name=AlternateKey&highlight=12)]

## <a name="data-annotations"></a><span data-ttu-id="8eab8-112">Заметки к данным</span><span class="sxs-lookup"><span data-stu-id="8eab8-112">Data Annotations</span></span>

<span data-ttu-id="8eab8-113">Альтернативные ключи не могут быть настроены с помощью заметок к данным.</span><span class="sxs-lookup"><span data-stu-id="8eab8-113">Alternate keys can not be configured using Data Annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="8eab8-114">Текучий API</span><span class="sxs-lookup"><span data-stu-id="8eab8-114">Fluent API</span></span>

<span data-ttu-id="8eab8-115">API-интерфейс Fluent можно использовать для настройки одного свойства в качестве альтернативного ключа.</span><span class="sxs-lookup"><span data-stu-id="8eab8-115">You can use the Fluent API to configure a single property to be an alternate key.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/AlternateKeySingle.cs?name=AlternateKeySingle&highlight=7,8)]

<span data-ttu-id="8eab8-116">Кроме того, API-интерфейс Fluent можно использовать для настройки нескольких свойств в качестве альтернативного ключа (называемого составным альтернативным ключом).</span><span class="sxs-lookup"><span data-stu-id="8eab8-116">You can also use the Fluent API to configure multiple properties to be an alternate key (known as a composite alternate key).</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/AlternateKeyComposite.cs?name=AlternateKeyComposite&highlight=7,8)]
