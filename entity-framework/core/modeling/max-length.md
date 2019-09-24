---
title: Максимальная длина — EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: c39c5d43-018d-48b8-94f2-b8bc7c686c69
uid: core/modeling/max-length
ms.openlocfilehash: b6f0594fed0c491b4f79dcda5273cdebe9ecf35f
ms.sourcegitcommit: ec196918691f50cd0b21693515b0549f06d9f39c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/23/2019
ms.locfileid: "71197223"
---
# <a name="maximum-length"></a><span data-ttu-id="af3c5-102">Максимальная длина</span><span class="sxs-lookup"><span data-stu-id="af3c5-102">Maximum Length</span></span>

<span data-ttu-id="af3c5-103">Настройка максимальной длины предоставляет подсказку для хранилища данных о соответствующем типе данных, который будет использоваться для данного свойства.</span><span class="sxs-lookup"><span data-stu-id="af3c5-103">Configuring a maximum length provides a hint to the data store about the appropriate data type to use for a given property.</span></span> <span data-ttu-id="af3c5-104">Максимальная длина применяется только к типам данных массива, таким как `string` и `byte[]`.</span><span class="sxs-lookup"><span data-stu-id="af3c5-104">Maximum length only applies to array data types, such as `string` and `byte[]`.</span></span>

> [!NOTE]  
> <span data-ttu-id="af3c5-105">Entity Framework не выполняет никаких проверок максимальной длины перед передачей данных поставщику.</span><span class="sxs-lookup"><span data-stu-id="af3c5-105">Entity Framework does not do any validation of maximum length before passing data to the provider.</span></span> <span data-ttu-id="af3c5-106">Поставщик или хранилище данных можно проверить, если это уместно.</span><span class="sxs-lookup"><span data-stu-id="af3c5-106">It is up to the provider or data store to validate if appropriate.</span></span> <span data-ttu-id="af3c5-107">Например, если целевой объект SQL Server, то превышение максимальной длины приведет к возникновению исключения, так как тип данных базового столбца не позволит сохранить излишние данные.</span><span class="sxs-lookup"><span data-stu-id="af3c5-107">For example, when targeting SQL Server, exceeding the maximum length will result in an exception as the data type of the underlying column will not allow excess data to be stored.</span></span>

## <a name="conventions"></a><span data-ttu-id="af3c5-108">Соглашения</span><span class="sxs-lookup"><span data-stu-id="af3c5-108">Conventions</span></span>

<span data-ttu-id="af3c5-109">По соглашению, оно остается поставщиком базы данных, чтобы выбрать подходящий тип данных для свойств.</span><span class="sxs-lookup"><span data-stu-id="af3c5-109">By convention, it is left up to the database provider to choose an appropriate data type for properties.</span></span> <span data-ttu-id="af3c5-110">Для свойств, имеющих длину, поставщик базы данных обычно выбирает тип данных, позволяющий получить самую длинную длину данных.</span><span class="sxs-lookup"><span data-stu-id="af3c5-110">For properties that have a length, the database provider will generally choose a data type that allows for the longest length of data.</span></span> <span data-ttu-id="af3c5-111">Например, Microsoft SQL Server будет использовать `nvarchar(max)` для `string` свойств (или `nvarchar(450)` если столбец используется в качестве ключа).</span><span class="sxs-lookup"><span data-stu-id="af3c5-111">For example, Microsoft SQL Server will use `nvarchar(max)` for `string` properties (or `nvarchar(450)` if the column is used as a key).</span></span>

## <a name="data-annotations"></a><span data-ttu-id="af3c5-112">Заметки к данным</span><span class="sxs-lookup"><span data-stu-id="af3c5-112">Data Annotations</span></span>

<span data-ttu-id="af3c5-113">Заметки к данным можно использовать для настройки максимальной длины свойства.</span><span class="sxs-lookup"><span data-stu-id="af3c5-113">You can use the Data Annotations to configure a maximum length for a property.</span></span> <span data-ttu-id="af3c5-114">В этом примере нацеливание SQL Server это приведет к тому, `nvarchar(500)` что тип данных будет использоваться.</span><span class="sxs-lookup"><span data-stu-id="af3c5-114">In this example, targeting SQL Server this would result in the `nvarchar(500)` data type being used.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/MaxLength.cs?highlight=14)]

## <a name="fluent-api"></a><span data-ttu-id="af3c5-115">Текучий API</span><span class="sxs-lookup"><span data-stu-id="af3c5-115">Fluent API</span></span>

<span data-ttu-id="af3c5-116">Для настройки максимальной длины свойства можно использовать API Fluent.</span><span class="sxs-lookup"><span data-stu-id="af3c5-116">You can use the Fluent API to configure a maximum length for a property.</span></span> <span data-ttu-id="af3c5-117">В этом примере нацеливание SQL Server это приведет к тому, `nvarchar(500)` что тип данных будет использоваться.</span><span class="sxs-lookup"><span data-stu-id="af3c5-117">In this example, targeting SQL Server this would result in the `nvarchar(500)` data type being used.</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/MaxLength.cs?highlight=11-13)]
