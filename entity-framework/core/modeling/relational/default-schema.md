---
title: Схема по умолчанию — EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: e6e58473-9f5e-4a1f-ac0f-b87d2cbb667e
uid: core/modeling/relational/default-schema
ms.openlocfilehash: 1579fed007997aa4cf49b4c1290aee86c81c0000
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/06/2019
ms.locfileid: "73655971"
---
# <a name="default-schema"></a><span data-ttu-id="39383-102">Схема по умолчанию</span><span class="sxs-lookup"><span data-stu-id="39383-102">Default Schema</span></span>

> [!NOTE]  
> <span data-ttu-id="39383-103">Описанная в этом разделе конфигурации применяется к реляционным базам данных в общем случае.</span><span class="sxs-lookup"><span data-stu-id="39383-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="39383-104">Показанные здесь методы расширения будут доступны после установки поставщика реляционной базы данных (посредством общего *пакета Microsoft.EntityFrameworkCore.Relational*).</span><span class="sxs-lookup"><span data-stu-id="39383-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="39383-105">Схема по умолчанию — это схема базы данных, в которой будут создаваться объекты, если схема не настроена явно для этого объекта.</span><span class="sxs-lookup"><span data-stu-id="39383-105">The default schema is the database schema that objects will be created in if a schema is not explicitly configured for that object.</span></span>

## <a name="conventions"></a><span data-ttu-id="39383-106">Соглашения</span><span class="sxs-lookup"><span data-stu-id="39383-106">Conventions</span></span>

<span data-ttu-id="39383-107">По соглашению поставщик базы данных будет выбирать наиболее подходящую схему по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="39383-107">By convention, the database provider will choose the most appropriate default schema.</span></span> <span data-ttu-id="39383-108">Например, Microsoft SQL Server будет использовать схему `dbo`, а SQLite не будет использовать схему (поскольку схемы не поддерживаются в SQLite).</span><span class="sxs-lookup"><span data-stu-id="39383-108">For example, Microsoft SQL Server will use the `dbo` schema and SQLite will not use a schema (since schemas are not supported in SQLite).</span></span>

## <a name="data-annotations"></a><span data-ttu-id="39383-109">Заметки к данным</span><span class="sxs-lookup"><span data-stu-id="39383-109">Data Annotations</span></span>

<span data-ttu-id="39383-110">Невозможно задать схему по умолчанию с помощью заметок к данным.</span><span class="sxs-lookup"><span data-stu-id="39383-110">You can not set the default schema using Data Annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="39383-111">Текучий API</span><span class="sxs-lookup"><span data-stu-id="39383-111">Fluent API</span></span>

<span data-ttu-id="39383-112">Для указания схемы по умолчанию можно использовать API Fluent.</span><span class="sxs-lookup"><span data-stu-id="39383-112">You can use the Fluent API to specify a default schema.</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/DefaultSchema.cs?name=DefaultSchema&highlight=7)]
