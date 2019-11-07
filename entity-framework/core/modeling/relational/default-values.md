---
title: Значения по умолчанию — EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: e541366a-130f-47dd-9997-1b110a11febe
uid: core/modeling/relational/default-values
ms.openlocfilehash: b6ac283d551e2c6ee119f7de6933363b5d8793a1
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/06/2019
ms.locfileid: "73655902"
---
# <a name="default-values"></a><span data-ttu-id="88994-102">Значения по умолчанию</span><span class="sxs-lookup"><span data-stu-id="88994-102">Default Values</span></span>

> [!NOTE]  
> <span data-ttu-id="88994-103">Описанная в этом разделе конфигурации применяется к реляционным базам данных в общем случае.</span><span class="sxs-lookup"><span data-stu-id="88994-103">The configuration in this section is applicable to relational databases in general.</span></span> <span data-ttu-id="88994-104">Показанные здесь методы расширения будут доступны после установки поставщика реляционной базы данных (посредством общего *пакета Microsoft.EntityFrameworkCore.Relational*).</span><span class="sxs-lookup"><span data-stu-id="88994-104">The extension methods shown here will become available when you install a relational database provider (due to the shared *Microsoft.EntityFrameworkCore.Relational* package).</span></span>

<span data-ttu-id="88994-105">По умолчанию столбец имеет значение, которое будет вставлено при вставке новой строки, но для столбца не указано значение.</span><span class="sxs-lookup"><span data-stu-id="88994-105">The default value of a column is the value that will be inserted if a new row is inserted but no value is specified for the column.</span></span>

## <a name="conventions"></a><span data-ttu-id="88994-106">Соглашения</span><span class="sxs-lookup"><span data-stu-id="88994-106">Conventions</span></span>

<span data-ttu-id="88994-107">По соглашению значение по умолчанию не настроено.</span><span class="sxs-lookup"><span data-stu-id="88994-107">By convention, a default value is not configured.</span></span>

## <a name="data-annotations"></a><span data-ttu-id="88994-108">Заметки к данным</span><span class="sxs-lookup"><span data-stu-id="88994-108">Data Annotations</span></span>

<span data-ttu-id="88994-109">Нельзя задать значение по умолчанию с помощью заметок к данным.</span><span class="sxs-lookup"><span data-stu-id="88994-109">You can not set a default value using Data Annotations.</span></span>

## <a name="fluent-api"></a><span data-ttu-id="88994-110">Текучий API</span><span class="sxs-lookup"><span data-stu-id="88994-110">Fluent API</span></span>

<span data-ttu-id="88994-111">API-интерфейс Fluent можно использовать для указания значения по умолчанию для свойства.</span><span class="sxs-lookup"><span data-stu-id="88994-111">You can use the Fluent API to specify the default value for a property.</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/DefaultValue.cs?name=DefaultValue&highlight=9)]

<span data-ttu-id="88994-112">Можно также указать фрагмент SQL, используемый для вычисления значения по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="88994-112">You can also specify a SQL fragment that is used to calculate the default value.</span></span>

[!code-csharp[Main](../../../../samples/core/Modeling/FluentAPI/Relational/DefaultValueSql.cs?name=DefaultValueSql&highlight=9)]
