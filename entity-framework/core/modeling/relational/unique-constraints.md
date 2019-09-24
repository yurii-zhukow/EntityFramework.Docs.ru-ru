---
title: Альтернативные ключи (ограничения UNIQUE) — EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 3d419dcf-2b5d-467c-b408-ea03d830721a
uid: core/modeling/relational/unique-constraints
ms.openlocfilehash: 7afcb804aeeccbd5e07c228a8fd9850ca00a2919
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/23/2019
ms.locfileid: "71197616"
---
# <a name="alternate-keys-unique-constraints"></a><span data-ttu-id="60726-102">Альтернативные ключи (ограничения UNIQUE)</span><span class="sxs-lookup"><span data-stu-id="60726-102">Alternate Keys (Unique Constraints)</span></span>

> [!NOTE]  
> <span data-ttu-id="60726-103">Описанная в этом разделе конфигурации применяется к реляционным базам данных в общем случае.</span><span class="sxs-lookup"><span data-stu-id="60726-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="60726-104">Показанные здесь методы расширения будут доступны после установки поставщика реляционной базы данных (посредством общего *пакета Microsoft.EntityFrameworkCore.Relational*).</span><span class="sxs-lookup"><span data-stu-id="60726-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="60726-105">Для каждого альтернативного ключа в модели вводится ограничение UNIQUE.</span><span class="sxs-lookup"><span data-stu-id="60726-105">A unique constraint is introduced for each alternate key in the model.</span></span>

## <a name="conventions"></a><span data-ttu-id="60726-106">Соглашения</span><span class="sxs-lookup"><span data-stu-id="60726-106">Conventions</span></span>

<span data-ttu-id="60726-107">По соглашению индекс и ограничение, введенные для альтернативного ключа, будут называться `AK_<type name>_<property name>`.</span><span class="sxs-lookup"><span data-stu-id="60726-107">By convention, the index and constraint that are introduced for an alternate key will be named `AK_<type name>_<property name>`.</span></span> <span data-ttu-id="60726-108">Для составных альтернативных `<property name>` ключей преобразуется в список имен свойств с разделителями подчеркивания.</span><span class="sxs-lookup"><span data-stu-id="60726-108">For composite alternate keys `<property name>` becomes an underscore separated list of property names.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="60726-109">Заметки к данным</span><span class="sxs-lookup"><span data-stu-id="60726-109">Data Annotations</span></span>

<span data-ttu-id="60726-110">Ограничения UNIQUE не могут быть настроены с помощью заметок к данным.</span><span class="sxs-lookup"><span data-stu-id="60726-110">Unique constraints can not be configured using Data Annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="60726-111">Текучий API</span><span class="sxs-lookup"><span data-stu-id="60726-111">Fluent API</span></span>

<span data-ttu-id="60726-112">API-интерфейс Fluent можно использовать для настройки индекса и имени ограничения для альтернативного ключа.</span><span class="sxs-lookup"><span data-stu-id="60726-112">You can use the Fluent API to configure the index and constraint name for an alternate key.</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/AlternateKeyName.cs?name=Model&highlight=9)]
