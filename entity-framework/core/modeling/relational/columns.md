---
title: Сопоставление столбцов — EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 05a47de9-1078-488e-a823-b516a4208f33
uid: core/modeling/relational/columns
ms.openlocfilehash: eaffc0cc1642f64edabeeef974f5f6de7a23b656
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/23/2019
ms.locfileid: "71197212"
---
# <a name="column-mapping"></a><span data-ttu-id="28d8e-102">Сопоставление столбцов</span><span class="sxs-lookup"><span data-stu-id="28d8e-102">Column Mapping</span></span>

> [!NOTE]  
> <span data-ttu-id="28d8e-103">Описанная в этом разделе конфигурации применяется к реляционным базам данных в общем случае.</span><span class="sxs-lookup"><span data-stu-id="28d8e-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="28d8e-104">Показанные здесь методы расширения будут доступны после установки поставщика реляционной базы данных (посредством общего *пакета Microsoft.EntityFrameworkCore.Relational*).</span><span class="sxs-lookup"><span data-stu-id="28d8e-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="28d8e-105">Сопоставление столбцов определяет, какие данные столбца должны быть запрошены и сохранены в базе данных.</span><span class="sxs-lookup"><span data-stu-id="28d8e-105">Column mapping identifies which column data should be queried from and saved to in the database.</span></span>

## <a name="conventions"></a><span data-ttu-id="28d8e-106">Соглашения</span><span class="sxs-lookup"><span data-stu-id="28d8e-106">Conventions</span></span>

<span data-ttu-id="28d8e-107">По соглашению каждое свойство будет настроено для соотнесения со столбцом с тем же именем, что и у свойства.</span><span class="sxs-lookup"><span data-stu-id="28d8e-107">By convention, each property will be set up to map to a column with the same name as the property.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="28d8e-108">Заметки к данным</span><span class="sxs-lookup"><span data-stu-id="28d8e-108">Data Annotations</span></span>

<span data-ttu-id="28d8e-109">Заметки к данным можно использовать для настройки столбца, с которым сопоставлено свойство.</span><span class="sxs-lookup"><span data-stu-id="28d8e-109">You can use Data Annotations to configure the column to which a property is mapped.</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/DataAnnotations/Relational/Column.cs?highlight=13)]

## <a name="fluent-api"></a><span data-ttu-id="28d8e-110">Текучий API</span><span class="sxs-lookup"><span data-stu-id="28d8e-110">Fluent API</span></span>

<span data-ttu-id="28d8e-111">API-интерфейс Fluent можно использовать для настройки столбца, с которым сопоставлено свойство.</span><span class="sxs-lookup"><span data-stu-id="28d8e-111">You can use the Fluent API to configure the column to which a property is mapped.</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/Column.cs?highlight=11-13)]
