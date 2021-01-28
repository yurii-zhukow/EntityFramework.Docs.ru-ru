---
title: Поставщик базы данных Microsoft SQL Server — создание значения EF Core
description: Шаблоны создания значений, характерные для поставщика базы данных SQL Server Entity Framework Core
author: roji
ms.date: 1/10/2020
uid: core/providers/sql-server/value-generation
ms.openlocfilehash: 90608f254a3d20e3c36586ae8325e0a982a7fa27
ms.sourcegitcommit: 7700840119b1639275f3b64836e7abb59103f2e7
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/28/2021
ms.locfileid: "98987142"
---
# <a name="sql-server-value-generation"></a><span data-ttu-id="bf805-103">Создание значения SQL Server</span><span class="sxs-lookup"><span data-stu-id="bf805-103">SQL Server Value Generation</span></span>

<span data-ttu-id="bf805-104">На этой странице подробно описана конфигурация и шаблоны создания значений, характерные для поставщика SQL Server.</span><span class="sxs-lookup"><span data-stu-id="bf805-104">This page details value generation configuration  and patterns that are specific to the SQL Server provider.</span></span> <span data-ttu-id="bf805-105">Рекомендуется сначала прочитать [страницу Общие сведения о создании значения](xref:core/modeling/generated-properties).</span><span class="sxs-lookup"><span data-stu-id="bf805-105">It's recommended to first read [the general page on value generation](xref:core/modeling/generated-properties).</span></span>

## <a name="identity-columns"></a><span data-ttu-id="bf805-106">Столбцы IDENTITY</span><span class="sxs-lookup"><span data-stu-id="bf805-106">IDENTITY columns</span></span>

<span data-ttu-id="bf805-107">По соглашению числовые столбцы, для которых настроены значения для добавления, настраиваются как [SQL Server столбцы идентификаторов](https://docs.microsoft.com/sql/t-sql/statements/create-table-transact-sql-identity-property).</span><span class="sxs-lookup"><span data-stu-id="bf805-107">By convention, numeric columns that are configured to have their values generated on add are set up as [SQL Server IDENTITY columns](https://docs.microsoft.com/sql/t-sql/statements/create-table-transact-sql-identity-property).</span></span>

### <a name="seed-and-increment"></a><span data-ttu-id="bf805-108">Начальное значение и приращение</span><span class="sxs-lookup"><span data-stu-id="bf805-108">Seed and increment</span></span>

<span data-ttu-id="bf805-109">По умолчанию столбцы ИДЕНТИФИКАТОРов начинаются с 1 (начальное значение) и увеличиваются на 1 каждый раз, когда добавляется строка (шаг приращения).</span><span class="sxs-lookup"><span data-stu-id="bf805-109">By default, IDENTITY columns start off at 1 (the seed), and increment by 1 each time a row is added (the increment).</span></span> <span data-ttu-id="bf805-110">Вы можете настроить другое начальное значение и шаг приращения следующим образом:</span><span class="sxs-lookup"><span data-stu-id="bf805-110">You can configure a different seed and increment as follows:</span></span>

[!code-csharp[Main](../../../../samples/core/SqlServer/ValueGeneration/IdentityOptionsContext.cs?name=IdentityOptions&highlight=5)]

> [!NOTE]
> <span data-ttu-id="bf805-111">Возможность настройки начального значения и приращения идентификатора появилась в EF Core 3,0.</span><span class="sxs-lookup"><span data-stu-id="bf805-111">The ability to configure IDENTITY seed and increment was introduced in EF Core 3.0.</span></span>

### <a name="inserting-explicit-values-into-identity-columns"></a><span data-ttu-id="bf805-112">Вставка явных значений в столбцы ИДЕНТИФИКАТОРов</span><span class="sxs-lookup"><span data-stu-id="bf805-112">Inserting explicit values into IDENTITY columns</span></span>

<span data-ttu-id="bf805-113">По умолчанию SQL Server не допускает вставку явных значений в столбцы ИДЕНТИФИКАТОРов.</span><span class="sxs-lookup"><span data-stu-id="bf805-113">By default, SQL Server doesn't allow inserting explicit values into IDENTITY columns.</span></span> <span data-ttu-id="bf805-114">Для этого необходимо вручную включить `IDENTITY_INSERT` перед вызовом `SaveChanges()` , как показано ниже.</span><span class="sxs-lookup"><span data-stu-id="bf805-114">To do so, you must manually enable `IDENTITY_INSERT` before calling `SaveChanges()`, as follows:</span></span>

[!code-csharp[Main](../../../../samples/core/SqlServer/ValueGeneration/ExplicitIdentityValues.cs?name=ExplicitIdentityValues)]

> [!NOTE]
> <span data-ttu-id="bf805-115">В нашем журнале заказов есть [запрос на функцию](https://github.com/aspnet/EntityFramework/issues/703), которая позволяет сделать это автоматически в пределах поставщика SQL Server.</span><span class="sxs-lookup"><span data-stu-id="bf805-115">We have a [feature request](https://github.com/aspnet/EntityFramework/issues/703) on our backlog to do this automatically within the SQL Server provider.</span></span>

## <a name="sequences"></a><span data-ttu-id="bf805-116">Последовательности</span><span class="sxs-lookup"><span data-stu-id="bf805-116">Sequences</span></span>

<span data-ttu-id="bf805-117">В качестве альтернативы столбцам ИДЕНТИФИКАТОРов можно использовать стандартные последовательности.</span><span class="sxs-lookup"><span data-stu-id="bf805-117">As an alternative to IDENTITY columns, you can use standard sequences.</span></span> <span data-ttu-id="bf805-118">Это может быть полезно в различных сценариях. Например, может потребоваться, чтобы несколько столбцов рисулись значения по умолчанию из одной последовательности.</span><span class="sxs-lookup"><span data-stu-id="bf805-118">This can be useful in various scenarios; for example, you may want to have multiple columns drawing their default values from a single sequence.</span></span>

<span data-ttu-id="bf805-119">SQL Server позволяет создавать последовательности и использовать их в качестве подробных сведений [на странице "Общие" в последовательностях](xref:core/modeling/sequences).</span><span class="sxs-lookup"><span data-stu-id="bf805-119">SQL Server allows you to create sequences and use them as detailed in [the general page on sequences](xref:core/modeling/sequences).</span></span> <span data-ttu-id="bf805-120">Вы можете настроить свойства для использования последовательностей с помощью `HasDefaultValueSql()` .</span><span class="sxs-lookup"><span data-stu-id="bf805-120">It's up to you to configure your properties to use sequences via `HasDefaultValueSql()`.</span></span>
