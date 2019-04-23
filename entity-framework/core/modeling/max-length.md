---
title: Максимальная длина — EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: c39c5d43-018d-48b8-94f2-b8bc7c686c69
uid: core/modeling/max-length
ms.openlocfilehash: 3220518cb0a409b6e802d2f3a98acdb949ffbf56
ms.sourcegitcommit: 87fcaba46535aa351db4bdb1231bd14b40e459b9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/22/2019
ms.locfileid: "59929853"
---
# <a name="maximum-length"></a><span data-ttu-id="5f44d-102">Максимальная длина</span><span class="sxs-lookup"><span data-stu-id="5f44d-102">Maximum Length</span></span>

<span data-ttu-id="5f44d-103">Настройка максимальной длиной предоставляет подсказку в хранилище данных о соответствующего типа данных для заданного свойства.</span><span class="sxs-lookup"><span data-stu-id="5f44d-103">Configuring a maximum length provides a hint to the data store about the appropriate data type to use for a given property.</span></span> <span data-ttu-id="5f44d-104">Максимальная длина применяется только в типы данных массивов, такие как `string` и `byte[]`.</span><span class="sxs-lookup"><span data-stu-id="5f44d-104">Maximum length only applies to array data types, such as `string` and `byte[]`.</span></span>

> [!NOTE]  
> <span data-ttu-id="5f44d-105">Платформа Entity Framework не выполняет проверку максимальной длиной перед передачей данных к поставщику.</span><span class="sxs-lookup"><span data-stu-id="5f44d-105">Entity Framework does not do any validation of maximum length before passing data to the provider.</span></span> <span data-ttu-id="5f44d-106">Возлагается поставщик или хранилище данных для проверки при необходимости.</span><span class="sxs-lookup"><span data-stu-id="5f44d-106">It is up to the provider or data store to validate if appropriate.</span></span> <span data-ttu-id="5f44d-107">Например когда исключение, так как тип данных базового столбца для различных версий SQL Server, превышающего максимальную длину приведет к не позволит избыточные данные для сохранения.</span><span class="sxs-lookup"><span data-stu-id="5f44d-107">For example, when targeting SQL Server, exceeding the maximum length will result in an exception as the data type of the underlying column will not allow excess data to be stored.</span></span>

## <a name="conventions"></a><span data-ttu-id="5f44d-108">Соглашения</span><span class="sxs-lookup"><span data-stu-id="5f44d-108">Conventions</span></span>

<span data-ttu-id="5f44d-109">По соглашению он остается до поставщик базы данных, чтобы выбрать соответствующий тип данных для свойства.</span><span class="sxs-lookup"><span data-stu-id="5f44d-109">By convention, it is left up to the database provider to choose an appropriate data type for properties.</span></span> <span data-ttu-id="5f44d-110">Для свойств, которые имеют длину поставщик базы данных обычно будет выбирать тип данных, который позволяет самая длинная длину данных.</span><span class="sxs-lookup"><span data-stu-id="5f44d-110">For properties that have a length, the database provider will generally choose a data type that allows for the longest length of data.</span></span> <span data-ttu-id="5f44d-111">Например, Microsoft SQL Server будет использовать `nvarchar(max)` для `string` свойства (или `nvarchar(450)` Если столбец используется в качестве ключа).</span><span class="sxs-lookup"><span data-stu-id="5f44d-111">For example, Microsoft SQL Server will use `nvarchar(max)` for `string` properties (or `nvarchar(450)` if the column is used as a key).</span></span>

## <a name="data-annotations"></a><span data-ttu-id="5f44d-112">Заметки к данным</span><span class="sxs-lookup"><span data-stu-id="5f44d-112">Data Annotations</span></span>

<span data-ttu-id="5f44d-113">Заметки данных можно использовать для настройки Максимальная длина для свойства.</span><span class="sxs-lookup"><span data-stu-id="5f44d-113">You can use the Data Annotations to configure a maximum length for a property.</span></span> <span data-ttu-id="5f44d-114">В этом примере для различных версий SQL Server, в результате `nvarchar(500)` используемого типа данных.</span><span class="sxs-lookup"><span data-stu-id="5f44d-114">In this example, targeting SQL Server this would result in the `nvarchar(500)` data type being used.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Samples/MaxLength.cs?highlight=14)]

## <a name="fluent-api"></a><span data-ttu-id="5f44d-115">Текучий API</span><span class="sxs-lookup"><span data-stu-id="5f44d-115">Fluent API</span></span>

<span data-ttu-id="5f44d-116">Fluent API можно использовать для настройки Максимальная длина для свойства.</span><span class="sxs-lookup"><span data-stu-id="5f44d-116">You can use the Fluent API to configure a maximum length for a property.</span></span> <span data-ttu-id="5f44d-117">В этом примере для различных версий SQL Server, в результате `nvarchar(500)` используемого типа данных.</span><span class="sxs-lookup"><span data-stu-id="5f44d-117">In this example, targeting SQL Server this would result in the `nvarchar(500)` data type being used.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Samples/MaxLength.cs?highlight=11-13)]
