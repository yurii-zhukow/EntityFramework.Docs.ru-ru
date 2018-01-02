---
title: "Сравнение EF Core и EF6"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: a6b9cd22-6803-4c6c-a4d4-21147c0a81cb
uid: efcore-and-ef6/index
ms.openlocfilehash: 4442e6931327f6a07d98aee47211ddbeed51a467
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/27/2017
---
# <a name="compare-ef-core--ef6"></a><span data-ttu-id="3e88f-102">Сравнение EF Core и EF6</span><span class="sxs-lookup"><span data-stu-id="3e88f-102">Compare EF Core & EF6</span></span>

<span data-ttu-id="3e88f-103">Существуют две версии Entity Framework — Entity Framework Core и Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="3e88f-103">There are two versions of Entity Framework, Entity Framework Core and Entity Framework 6.</span></span>

## <a name="entity-framework-6"></a><span data-ttu-id="3e88f-104">Entity Framework 6</span><span class="sxs-lookup"><span data-stu-id="3e88f-104">Entity Framework 6</span></span>

<span data-ttu-id="3e88f-105">Entity Framework 6 (EF6) — это проверенная технология доступа к данных, для которой в течение многих лет велась доработка функций и повышалась стабильность работы.</span><span class="sxs-lookup"><span data-stu-id="3e88f-105">Entity Framework 6 (EF6) is a tried and tested data access technology with many years of features and stabilization.</span></span> <span data-ttu-id="3e88f-106">Впервые этот продукт был выпущен в 2008 году в составе пакета обновления 1 (SP1) для платформы .NET Framework 3.5 и пакета обновления 1 (SP1) для Visual Studio 2008.</span><span class="sxs-lookup"><span data-stu-id="3e88f-106">It first released in 2008, as part of .NET Framework 3.5 SP1 and Visual Studio 2008 SP1.</span></span> <span data-ttu-id="3e88f-107">Начиная с выпуска EF4.1, он поставлялся в виде [пакета NuGet EntityFramework](https://www.nuget.org/packages/EntityFramework/), который сейчас стал самым популярным пакетом на сайте NuGet.org.</span><span class="sxs-lookup"><span data-stu-id="3e88f-107">Starting with the EF4.1 release it has shipped as the [EntityFramework NuGet package](https://www.nuget.org/packages/EntityFramework/) - currently the most popular package on NuGet.org.</span></span>

<span data-ttu-id="3e88f-108">Продукт EF6 все еще поддерживается, и для него постепенно будут выходить исправления ошибок и небольшие улучшения.</span><span class="sxs-lookup"><span data-stu-id="3e88f-108">EF6 continues to be a supported product, and will continue to see bug fixes and minor improvements for some time to come.</span></span>

## <a name="entity-framework-core"></a><span data-ttu-id="3e88f-109">Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="3e88f-109">Entity Framework Core</span></span>

<span data-ttu-id="3e88f-110">Entity Framework Core (EF Core) — это упрощенная, расширяемая и кроссплатформенная версия Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="3e88f-110">Entity Framework Core (EF Core) is a lightweight, extensible, and cross-platform version of Entity Framework.</span></span> <span data-ttu-id="3e88f-111">EF Core отличается от EF6 многочисленными усовершенствованиями и новыми функциями.</span><span class="sxs-lookup"><span data-stu-id="3e88f-111">EF Core introduces many improvements and new features when compared with EF6.</span></span> <span data-ttu-id="3e88f-112">При этом EF Core является новой базой кода, не настолько зрелой как EF6.</span><span class="sxs-lookup"><span data-stu-id="3e88f-112">At the same time, EF Core is a new code base and not as mature as EF6.</span></span>

<span data-ttu-id="3e88f-113">В EF Core взаимодействие с разработчиком осуществляется так же, как и в EF6, кроме того, там сохранена основная часть API верхнего уровня, поэтому EF Core покажется очень знакомым тем, кто уже работал с EF6.</span><span class="sxs-lookup"><span data-stu-id="3e88f-113">EF Core keeps the developer experience from EF6, and most of the top-level APIs remain the same too, so EF Core will feel very familiar to folks who have used EF6.</span></span> <span data-ttu-id="3e88f-114">При этом EF Core основан на совершенно новом наборе основных компонентов.</span><span class="sxs-lookup"><span data-stu-id="3e88f-114">At the same time, EF Core is built over a completely new set of core components.</span></span> <span data-ttu-id="3e88f-115">Это означает, что EF Core не наследуют все функции EF6 автоматически.</span><span class="sxs-lookup"><span data-stu-id="3e88f-115">This means EF Core doesn't automatically inherit all the features from EF6.</span></span> <span data-ttu-id="3e88f-116">Некоторые из них появятся в будущих выпусках (например, отложенная загрузка и устойчивость подключений), другие, реже используемые функции, в EF Core реализованы не будут.</span><span class="sxs-lookup"><span data-stu-id="3e88f-116">Some of these features will show up in future releases (such as lazy loading and connection resiliency), other less commonly used features will not be implemented in EF Core.</span></span>

<span data-ttu-id="3e88f-117">Новое, расширяемое и упрощенное ядро позволило добавить в EF Core некоторые функции, которые не будут реализованы в EF6 (например, альтернативные ключи и смешанная оценка клиента и базы данных в запросах LINQ).</span><span class="sxs-lookup"><span data-stu-id="3e88f-117">The new, extensible, and lightweight core has also allowed us to add some features to EF Core that will not be implemented in EF6 (such as alternate keys and mixed client/database evaluation in LINQ queries).</span></span>
