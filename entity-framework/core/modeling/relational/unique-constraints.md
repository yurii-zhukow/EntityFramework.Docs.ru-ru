---
title: "Альтернативные ключи (ограничения Unique) - EF Core"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 3d419dcf-2b5d-467c-b408-ea03d830721a
ms.technology: entity-framework-core
uid: core/modeling/relational/unique-constraints
ms.openlocfilehash: 1b7e2bef6ede95f8c27211ba00dcc6b97cccde9b
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/27/2017
---
# <a name="alternate-keys-unique-constraints"></a><span data-ttu-id="aecfd-102">Альтернативные ключи (ограничения Unique)</span><span class="sxs-lookup"><span data-stu-id="aecfd-102">Alternate Keys (Unique Constraints)</span></span>

> [!NOTE]  
> <span data-ttu-id="aecfd-103">В этом разделе конфигурации применяется для реляционных баз данных в целом.</span><span class="sxs-lookup"><span data-stu-id="aecfd-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="aecfd-104">Методы расширения, показанный здесь будут доступны после установки поставщика реляционной базы данных (из-за общей *Microsoft.EntityFrameworkCore.Relational* пакета).</span><span class="sxs-lookup"><span data-stu-id="aecfd-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="aecfd-105">Ограничение уникальности введен для каждого альтернативный ключ в модели.</span><span class="sxs-lookup"><span data-stu-id="aecfd-105">A unique constraint is introduced for each alternate key in the model.</span></span>

## <a name="conventions"></a><span data-ttu-id="aecfd-106">Соглашения</span><span class="sxs-lookup"><span data-stu-id="aecfd-106">Conventions</span></span>

<span data-ttu-id="aecfd-107">По соглашению, индекс и ограничения, введенные для дополнительный ключ будет называться `AK_<type name>_<property name>`.</span><span class="sxs-lookup"><span data-stu-id="aecfd-107">By convention, the index and constraint that are introduced for an alternate key will be named `AK_<type name>_<property name>`.</span></span> <span data-ttu-id="aecfd-108">Для составных ключей альтернативный `<property name>` становится подчеркивания запятыми список имен свойств.</span><span class="sxs-lookup"><span data-stu-id="aecfd-108">For composite alternate keys `<property name>` becomes an underscore separated list of property names.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="aecfd-109">Заметки к данным</span><span class="sxs-lookup"><span data-stu-id="aecfd-109">Data Annotations</span></span>

<span data-ttu-id="aecfd-110">Ограничения UNIQUE не следует задавать с помощью заметок к данным.</span><span class="sxs-lookup"><span data-stu-id="aecfd-110">Unique constraints can not be configured using Data Annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="aecfd-111">Fluent API</span><span class="sxs-lookup"><span data-stu-id="aecfd-111">Fluent API</span></span>

<span data-ttu-id="aecfd-112">Чтобы задать имя индекса и ограничения для дополнительный ключ можно использовать Fluent API.</span><span class="sxs-lookup"><span data-stu-id="aecfd-112">You can use the Fluent API to configure the index and constraint name for an alternate key.</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Samples/Relational/AlternateKeyName.cs?name=Model&highlight=9)]
