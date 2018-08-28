---
title: Альтернативные ключи (ограничения уникальности) — EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 3d419dcf-2b5d-467c-b408-ea03d830721a
uid: core/modeling/relational/unique-constraints
ms.openlocfilehash: 7ec58ee31aac79e15329dc8542f37fd117772fbe
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/27/2018
ms.locfileid: "42994195"
---
# <a name="alternate-keys-unique-constraints"></a><span data-ttu-id="2f760-102">Альтернативные ключи (ограничения уникальности)</span><span class="sxs-lookup"><span data-stu-id="2f760-102">Alternate Keys (Unique Constraints)</span></span>

> [!NOTE]  
> <span data-ttu-id="2f760-103">Описанная в этом разделе конфигурации применяется к реляционным базам данных в общем случае.</span><span class="sxs-lookup"><span data-stu-id="2f760-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="2f760-104">Показанные здесь методы расширения будут доступны после установки поставщика реляционной базы данных (посредством общего *пакета Microsoft.EntityFrameworkCore.Relational*).</span><span class="sxs-lookup"><span data-stu-id="2f760-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="2f760-105">Для каждого альтернативного ключа в модели введено ограничение уникальности.</span><span class="sxs-lookup"><span data-stu-id="2f760-105">A unique constraint is introduced for each alternate key in the model.</span></span>

## <a name="conventions"></a><span data-ttu-id="2f760-106">Соглашения</span><span class="sxs-lookup"><span data-stu-id="2f760-106">Conventions</span></span>

<span data-ttu-id="2f760-107">По соглашению, индекс и ограничения, введенные для альтернативного ключа будет называться `AK_<type name>_<property name>`.</span><span class="sxs-lookup"><span data-stu-id="2f760-107">By convention, the index and constraint that are introduced for an alternate key will be named `AK_<type name>_<property name>`.</span></span> <span data-ttu-id="2f760-108">Для составных ключей альтернативный `<property name>` становится список имен свойств, разделенных символом подчеркивания.</span><span class="sxs-lookup"><span data-stu-id="2f760-108">For composite alternate keys `<property name>` becomes an underscore separated list of property names.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="2f760-109">Заметки к данным</span><span class="sxs-lookup"><span data-stu-id="2f760-109">Data Annotations</span></span>

<span data-ttu-id="2f760-110">Не ограничения UNIQUE можно настроить с помощью заметок к данным.</span><span class="sxs-lookup"><span data-stu-id="2f760-110">Unique constraints can not be configured using Data Annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="2f760-111">Текучий API</span><span class="sxs-lookup"><span data-stu-id="2f760-111">Fluent API</span></span>

<span data-ttu-id="2f760-112">Fluent API можно использовать для настройки имени индексов и ограничений для альтернативного ключа.</span><span class="sxs-lookup"><span data-stu-id="2f760-112">You can use the Fluent API to configure the index and constraint name for an alternate key.</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Samples/Relational/AlternateKeyName.cs?name=Model&highlight=9)]
