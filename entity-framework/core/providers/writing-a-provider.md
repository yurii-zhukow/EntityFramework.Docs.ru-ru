---
title: Создание поставщика базы данных — EF Core
description: Сведения о создании нового поставщика Entity Framework Core
author: anmiller
ms.date: 10/27/2016
ms.assetid: 1165e2ec-e421-43fc-92ab-d92f9ab3c494
uid: core/providers/writing-a-provider
ms.openlocfilehash: 6d7a8a03c612eeda4d65917a2713e4570c671002
ms.sourcegitcommit: 7c3939504bb9da3f46bea3443638b808c04227c2
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/09/2020
ms.locfileid: "89616506"
---
# <a name="writing-a-database-provider"></a><span data-ttu-id="125a1-103">Создание поставщика баз данных</span><span class="sxs-lookup"><span data-stu-id="125a1-103">Writing a Database Provider</span></span>

<span data-ttu-id="125a1-104">Дополнительные сведения о создании Entity Framework Core поставщика базы данных см. [в разделе Создание поставщика EF Core](https://blog.oneunicorn.com/2016/11/11/so-you-want-to-write-an-ef-core-provider/) с помощью [Артур Виккерс](https://github.com/ajcvickers).</span><span class="sxs-lookup"><span data-stu-id="125a1-104">For information about writing an Entity Framework Core database provider, see [So you want to write an EF Core provider](https://blog.oneunicorn.com/2016/11/11/so-you-want-to-write-an-ef-core-provider/) by [Arthur Vickers](https://github.com/ajcvickers).</span></span>

> [!NOTE]
> <span data-ttu-id="125a1-105">Эти записи не были обновлены с момента EF Core 1,1 и были значительными изменениями с момента этого времени.</span><span class="sxs-lookup"><span data-stu-id="125a1-105">These posts have not been updated since EF Core 1.1 and there have been significant changes since that time.</span></span>  
<span data-ttu-id="125a1-106">[Проблема 681](https://github.com/dotnet/EntityFramework.Docs/issues/681) отслеживает обновления для этой документации.</span><span class="sxs-lookup"><span data-stu-id="125a1-106">[Issue 681](https://github.com/dotnet/EntityFramework.Docs/issues/681) is tracking updates to this documentation.</span></span>

<span data-ttu-id="125a1-107">EF Core CodeBase является открытым исходным кодом и содержит несколько поставщиков баз данных, которые можно использовать в качестве ссылки.</span><span class="sxs-lookup"><span data-stu-id="125a1-107">The EF Core codebase is open source and contains several database providers that can be used as a reference.</span></span> <span data-ttu-id="125a1-108">Исходный код можно найти по адресу <https://github.com/aspnet/EntityFrameworkCore> .</span><span class="sxs-lookup"><span data-stu-id="125a1-108">You can find the source code at <https://github.com/aspnet/EntityFrameworkCore>.</span></span> <span data-ttu-id="125a1-109">Также может быть полезно взглянуть на код для часто используемых сторонних поставщиков, таких как [Npgsql](https://github.com/npgsql/Npgsql.EntityFrameworkCore.PostgreSQL), [Pomelo MySQL](https://github.com/PomeloFoundation/Pomelo.EntityFrameworkCore.MySql)и [SQL Server Compact](https://github.com/ErikEJ/EntityFramework.SqlServerCompact).</span><span class="sxs-lookup"><span data-stu-id="125a1-109">It may also be helpful to look at the code for commonly used third-party providers, such as [Npgsql](https://github.com/npgsql/Npgsql.EntityFrameworkCore.PostgreSQL), [Pomelo MySQL](https://github.com/PomeloFoundation/Pomelo.EntityFrameworkCore.MySql), and [SQL Server Compact](https://github.com/ErikEJ/EntityFramework.SqlServerCompact).</span></span> <span data-ttu-id="125a1-110">В частности, эти проекты настроены для расширения и выполнения функциональных тестов, публикуемых в NuGet.</span><span class="sxs-lookup"><span data-stu-id="125a1-110">In particular, these projects are setup to extend from and run functional tests that we publish on NuGet.</span></span> <span data-ttu-id="125a1-111">Этот тип установки настоятельно рекомендуется.</span><span class="sxs-lookup"><span data-stu-id="125a1-111">This kind of setup is strongly recommended.</span></span>

## <a name="keeping-up-to-date-with-provider-changes"></a><span data-ttu-id="125a1-112">Актуальность изменений поставщика</span><span class="sxs-lookup"><span data-stu-id="125a1-112">Keeping up-to-date with provider changes</span></span>

<span data-ttu-id="125a1-113">Начиная с работы после выпуска 2,1 мы создали [Журнал изменений](xref:core/providers/provider-log) , которые могут потребовать внесения соответствующих изменений в код поставщика.</span><span class="sxs-lookup"><span data-stu-id="125a1-113">Starting with work after the 2.1 release, we have created a [log of changes](xref:core/providers/provider-log) that may need corresponding changes in provider code.</span></span> <span data-ttu-id="125a1-114">Это предназначено для помощи при обновлении существующего поставщика для работы с новой версией EF Core.</span><span class="sxs-lookup"><span data-stu-id="125a1-114">This is intended to help when updating an existing provider to work with a new version of EF Core.</span></span>

<span data-ttu-id="125a1-115">До 2,1 мы использовали [`providers-beware`](https://github.com/aspnet/EntityFrameworkCore/labels/providers-beware) [`providers-fyi`](https://github.com/aspnet/EntityFrameworkCore/labels/providers-fyi) метки и в наших проблемах GitHub и запросах на вытягивание для аналогичной цели.</span><span class="sxs-lookup"><span data-stu-id="125a1-115">Prior to 2.1, we used the [`providers-beware`](https://github.com/aspnet/EntityFrameworkCore/labels/providers-beware) and [`providers-fyi`](https://github.com/aspnet/EntityFrameworkCore/labels/providers-fyi) labels on our GitHub issues and pull requests for a similar purpose.</span></span> <span data-ttu-id="125a1-116">Мы континиуе с этими метками о проблемах, чтобы указать, какие рабочие элементы в данном выпуске могут также требовать выполнения работы в поставщиках.</span><span class="sxs-lookup"><span data-stu-id="125a1-116">We will continiue to use these lables on issues to give an indication which work items in a given release may also require work to be done in providers.</span></span> <span data-ttu-id="125a1-117">`providers-beware`Метка обычно означает, что реализация рабочего элемента может привести к нарушению работы поставщиков, а `providers-fyi` Метка обычно означает, что поставщики не будут разорваны, но может потребоваться изменить код, например, чтобы включить новые функциональные возможности.</span><span class="sxs-lookup"><span data-stu-id="125a1-117">A `providers-beware` label typically means that the implementation of an work item may break providers, while a `providers-fyi` label typically means that providers will not be broken, but code may need to be changed anyway, for example, to enable new functionality.</span></span>

## <a name="suggested-naming-of-third-party-providers"></a><span data-ttu-id="125a1-118">Предлагаемое именование сторонних поставщиков</span><span class="sxs-lookup"><span data-stu-id="125a1-118">Suggested naming of third party providers</span></span>

<span data-ttu-id="125a1-119">Мы рекомендуем использовать следующее именование для пакетов NuGet.</span><span class="sxs-lookup"><span data-stu-id="125a1-119">We suggest using the following naming for NuGet packages.</span></span> <span data-ttu-id="125a1-120">Это согласуется с именами пакетов, доставленных командой EF Core.</span><span class="sxs-lookup"><span data-stu-id="125a1-120">This is consistent with the names of packages delivered by the EF Core team.</span></span>

`<Optional project/company name>.EntityFrameworkCore.<Database engine name>`

<span data-ttu-id="125a1-121">Пример:</span><span class="sxs-lookup"><span data-stu-id="125a1-121">For example:</span></span>

* `Microsoft.EntityFrameworkCore.SqlServer`
* `Npgsql.EntityFrameworkCore.PostgreSQL`
* `EntityFrameworkCore.SqlServerCompact40`
