---
title: Разработка поставщика базы данных — EF Core
author: anmiller
ms.date: 10/27/2016
ms.assetid: 1165e2ec-e421-43fc-92ab-d92f9ab3c494
uid: core/providers/writing-a-provider
ms.openlocfilehash: c7130b0d104cd26584d298da98eb3e7080ee7f3c
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/27/2018
ms.locfileid: "42993670"
---
# <a name="writing-a-database-provider"></a><span data-ttu-id="3976f-102">Разработка поставщика базы данных</span><span class="sxs-lookup"><span data-stu-id="3976f-102">Writing a Database Provider</span></span>

<span data-ttu-id="3976f-103">Сведения о написании поставщика базы данных Entity Framework Core, см. в разделе [, требуется написать поставщика EF Core](https://blog.oneunicorn.com/2016/11/11/so-you-want-to-write-an-ef-core-provider/) по [Vickers Артур](https://github.com/ajcvickers).</span><span class="sxs-lookup"><span data-stu-id="3976f-103">For information about writing an Entity Framework Core database provider, see [So you want to write an EF Core provider](https://blog.oneunicorn.com/2016/11/11/so-you-want-to-write-an-ef-core-provider/) by [Arthur Vickers](https://github.com/ajcvickers).</span></span>

> [!NOTE]
> <span data-ttu-id="3976f-104">Эти сообщения не были обновлены с момента EF Core 1.1 и с этого момента были сделаны существенные изменения [681 проблема](https://github.com/aspnet/EntityFramework.Docs/issues/681) отслеживает изменения в эту документацию.</span><span class="sxs-lookup"><span data-stu-id="3976f-104">These posts have not been updated since EF Core 1.1 and there have been significant changes since that time [Issue 681](https://github.com/aspnet/EntityFramework.Docs/issues/681) is tracking updates to this documentation.</span></span>

<span data-ttu-id="3976f-105">База кода EF Core имеет открытый исходный код и содержит несколько поставщиков базы данных, которые могут использоваться в качестве ссылки.</span><span class="sxs-lookup"><span data-stu-id="3976f-105">The EF Core codebase is open source and contains several database providers that can be used as a reference.</span></span> <span data-ttu-id="3976f-106">Можно найти исходный код в https://github.com/aspnet/EntityFrameworkCore.</span><span class="sxs-lookup"><span data-stu-id="3976f-106">You can find the source code at https://github.com/aspnet/EntityFrameworkCore.</span></span> <span data-ttu-id="3976f-107">Также это может быть полезно посмотреть на код для часто используемых сторонних поставщиков, таких как [Npgsql](https://github.com/npgsql/Npgsql.EntityFrameworkCore.PostgreSQL), [Pomelo MySQL](https://github.com/PomeloFoundation/Pomelo.EntityFrameworkCore.MySql), и [SQL Server Compact](https://github.com/ErikEJ/EntityFramework.SqlServerCompact).</span><span class="sxs-lookup"><span data-stu-id="3976f-107">It may also be helpful to look at the code for commonly used third-party providers, such as [Npgsql](https://github.com/npgsql/Npgsql.EntityFrameworkCore.PostgreSQL), [Pomelo MySQL](https://github.com/PomeloFoundation/Pomelo.EntityFrameworkCore.MySql), and [SQL Server Compact](https://github.com/ErikEJ/EntityFramework.SqlServerCompact).</span></span> <span data-ttu-id="3976f-108">В частности эти проекты, программу установки, чтобы расширить и запуск функциональных тестов, которые мы публикуем в NuGet.</span><span class="sxs-lookup"><span data-stu-id="3976f-108">In particular, these projects are setup to extend from and run functional tests that we publish on NuGet.</span></span> <span data-ttu-id="3976f-109">Настоятельно рекомендуется такого типа установки.</span><span class="sxs-lookup"><span data-stu-id="3976f-109">This kind of setup is strongly recommended.</span></span>

## <a name="keeping-up-to-date-with-provider-changes"></a><span data-ttu-id="3976f-110">Обновление с изменениями поставщика</span><span class="sxs-lookup"><span data-stu-id="3976f-110">Keeping up-to-date with provider changes</span></span>

<span data-ttu-id="3976f-111">Начиная с рабочих после 2.1 выпуска, мы создали [журнала изменений](provider-log.md) , может потребоваться соответствующие изменения в код поставщика.</span><span class="sxs-lookup"><span data-stu-id="3976f-111">Starting with work after the 2.1 release, we have created a [log of changes](provider-log.md) that may need corresponding changes in provider code.</span></span> <span data-ttu-id="3976f-112">Это призвано помочь при обновлении существующего поставщика для работы с новой версии EF Core.</span><span class="sxs-lookup"><span data-stu-id="3976f-112">This is intended to help when updating an existing provider to work with a new version of EF Core.</span></span>

<span data-ttu-id="3976f-113">Прежде чем 2.1, мы использовали [ `providers-beware` ](https://github.com/aspnet/EntityFrameworkCore/labels/providers-beware) и [ `providers-fyi` ](https://github.com/aspnet/EntityFrameworkCore/labels/providers-fyi) меток на наши вопросы GitHub и запросов на Вытягивание с одинаковой цели.</span><span class="sxs-lookup"><span data-stu-id="3976f-113">Prior to 2.1, we used the [`providers-beware`](https://github.com/aspnet/EntityFrameworkCore/labels/providers-beware) and [`providers-fyi`](https://github.com/aspnet/EntityFrameworkCore/labels/providers-fyi) labels on our GitHub issues and pull requests for a similar purpose.</span></span> <span data-ttu-id="3976f-114">Мы выполним continiue использование этих меток по проблемам, для предоставления значение, указывающее какие рабочие элементы в определенном выпуске также может потребоваться должны быть выполнены в поставщиках.</span><span class="sxs-lookup"><span data-stu-id="3976f-114">We will continiue to use these lables on issues to give an indication which work items in a given release may also require work to be done in providers.</span></span> <span data-ttu-id="3976f-115">Объект `providers-beware` метки обычно означает, что реализации рабочего элемента может нарушить работу поставщиков, хотя `providers-fyi` метки обычно означает, что поставщики не будет нарушена, но код может понадобиться изменить в любом случае, например, чтобы задействовать новые функциональные возможности.</span><span class="sxs-lookup"><span data-stu-id="3976f-115">A `providers-beware` label typically means that the implementation of an work item may break providers, while a `providers-fyi` label typically means that providers will not be broken, but code may need to be changed anyway, for example, to enable new functionality.</span></span>

## <a name="suggested-naming-of-third-party-providers"></a><span data-ttu-id="3976f-116">Предлагаемые именования сторонних поставщиков</span><span class="sxs-lookup"><span data-stu-id="3976f-116">Suggested naming of third party providers</span></span>

<span data-ttu-id="3976f-117">Мы советуем использовать следующие имена пакетов NuGet.</span><span class="sxs-lookup"><span data-stu-id="3976f-117">We suggest using the following naming for NuGet packages.</span></span> <span data-ttu-id="3976f-118">Это согласуется с именами пакетов, предоставленные группе EF Core.</span><span class="sxs-lookup"><span data-stu-id="3976f-118">This is consistent with the names of packages delivered by the EF Core team.</span></span>

`<Optional project/company name>.EntityFrameworkCore.<Database engine name>`

<span data-ttu-id="3976f-119">Пример:</span><span class="sxs-lookup"><span data-stu-id="3976f-119">For example:</span></span>
* `Microsoft.EntityFrameworkCore.SqlServer`
* `Npgsql.EntityFrameworkCore.PostgreSQL`
* `EntityFrameworkCore.SqlServerCompact40`
