---
title: Последовательности — EF Core
description: Настройка последовательностей в модели Entity Framework Core
author: roji
ms.date: 12/18/2019
uid: core/modeling/sequences
ms.openlocfilehash: e388bde11b1251564b37227e8884d2b8c34dc02e
ms.sourcegitcommit: 032a1767d7a6e42052a005f660b80372c6521e7e
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/12/2021
ms.locfileid: "98128528"
---
# <a name="sequences"></a><span data-ttu-id="871bd-103">Последовательности</span><span class="sxs-lookup"><span data-stu-id="871bd-103">Sequences</span></span>

> [!NOTE]
> <span data-ttu-id="871bd-104">Последовательности — это функция, которая обычно поддерживается только реляционными базами данных.</span><span class="sxs-lookup"><span data-stu-id="871bd-104">Sequences are a feature typically supported only by relational databases.</span></span> <span data-ttu-id="871bd-105">Если вы используете нереляционную базу данных, например Cosmos, обратитесь к документации по базе данных, чтобы создать уникальные значения.</span><span class="sxs-lookup"><span data-stu-id="871bd-105">If you're using a non-relational database such as Cosmos, check your database documentation on generating unique values.</span></span>

<span data-ttu-id="871bd-106">Последовательность формирует уникальные последовательные числовые значения в базе данных.</span><span class="sxs-lookup"><span data-stu-id="871bd-106">A sequence generates unique, sequential numeric values in the database.</span></span> <span data-ttu-id="871bd-107">Последовательности не связаны с определенной таблицей, и можно настроить несколько таблиц для вывода значений из одной и той же последовательности.</span><span class="sxs-lookup"><span data-stu-id="871bd-107">Sequences are not associated with a specific table, and multiple tables can be set up to draw values from the same sequence.</span></span>

## <a name="basic-usage"></a><span data-ttu-id="871bd-108">Базовое использование</span><span class="sxs-lookup"><span data-stu-id="871bd-108">Basic usage</span></span>

<span data-ttu-id="871bd-109">В модели можно настроить последовательность, а затем использовать ее для создания значений свойств.</span><span class="sxs-lookup"><span data-stu-id="871bd-109">You can set up a sequence in the model, and then use it to generate values for properties:</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Sequence.cs?name=Sequence&highlight=3,7)]

<span data-ttu-id="871bd-110">Обратите внимание, что конкретный SQL, используемый для формирования значения из последовательности, зависит от конкретной базы данных. Приведенный выше пример работает на SQL Server но в других базах данных завершается ошибкой.</span><span class="sxs-lookup"><span data-stu-id="871bd-110">Note that the specific SQL used to generate a value from a sequence is database-specific; the above example works on SQL Server but will fail on other databases.</span></span> <span data-ttu-id="871bd-111">Дополнительные сведения см. в документации по конкретной базе данных.</span><span class="sxs-lookup"><span data-stu-id="871bd-111">Consult your specific database's documentation for more information.</span></span>

## <a name="configuring-sequence-settings"></a><span data-ttu-id="871bd-112">Настройка параметров последовательности</span><span class="sxs-lookup"><span data-stu-id="871bd-112">Configuring sequence settings</span></span>

<span data-ttu-id="871bd-113">Можно также настроить дополнительные аспекты последовательности, например ее схему, начальное значение, приращение и т. д.:</span><span class="sxs-lookup"><span data-stu-id="871bd-113">You can also configure additional aspects of the sequence, such as its schema, start value, increment, etc.:</span></span>

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/SequenceConfiguration.cs?name=SequenceConfiguration&highlight=3-5)]
