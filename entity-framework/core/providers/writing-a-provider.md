---
title: Создание поставщика базы данных — EF Core
author: anmiller
ms.date: 10/27/2016
ms.assetid: 1165e2ec-e421-43fc-92ab-d92f9ab3c494
uid: core/providers/writing-a-provider
ms.openlocfilehash: 2d9e4a6cdfda80d7dfcfb6e7bf0480eb49f8e057
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/06/2020
ms.locfileid: "78414809"
---
# <a name="writing-a-database-provider"></a><span data-ttu-id="71362-102">Создание поставщика баз данных</span><span class="sxs-lookup"><span data-stu-id="71362-102">Writing a Database Provider</span></span>

<span data-ttu-id="71362-103">Дополнительные сведения о создании Entity Framework Core поставщика базы данных см. [в разделе Создание поставщика EF Core](https://blog.oneunicorn.com/2016/11/11/so-you-want-to-write-an-ef-core-provider/) с помощью [Артур Виккерс](https://github.com/ajcvickers).</span><span class="sxs-lookup"><span data-stu-id="71362-103">For information about writing an Entity Framework Core database provider, see [So you want to write an EF Core provider](https://blog.oneunicorn.com/2016/11/11/so-you-want-to-write-an-ef-core-provider/) by [Arthur Vickers](https://github.com/ajcvickers).</span></span>

> [!NOTE]
> <span data-ttu-id="71362-104">Эти записи не были обновлены с момента EF Core 1,1 и были внесены значительные изменения, так как в этом случае [681](https://github.com/dotnet/EntityFramework.Docs/issues/681) отслеживает обновления этой документации.</span><span class="sxs-lookup"><span data-stu-id="71362-104">These posts have not been updated since EF Core 1.1 and there have been significant changes since that time [Issue 681](https://github.com/dotnet/EntityFramework.Docs/issues/681) is tracking updates to this documentation.</span></span>

<span data-ttu-id="71362-105">EF Core CodeBase является открытым исходным кодом и содержит несколько поставщиков баз данных, которые можно использовать в качестве ссылки.</span><span class="sxs-lookup"><span data-stu-id="71362-105">The EF Core codebase is open source and contains several database providers that can be used as a reference.</span></span> <span data-ttu-id="71362-106">Исходный код можно найти по адресу <https://github.com/aspnet/EntityFrameworkCore>.</span><span class="sxs-lookup"><span data-stu-id="71362-106">You can find the source code at <https://github.com/aspnet/EntityFrameworkCore>.</span></span> <span data-ttu-id="71362-107">Также может быть полезно взглянуть на код для часто используемых сторонних поставщиков, таких как [Npgsql](https://github.com/npgsql/Npgsql.EntityFrameworkCore.PostgreSQL), [Pomelo MySQL](https://github.com/PomeloFoundation/Pomelo.EntityFrameworkCore.MySql)и [SQL Server Compact](https://github.com/ErikEJ/EntityFramework.SqlServerCompact).</span><span class="sxs-lookup"><span data-stu-id="71362-107">It may also be helpful to look at the code for commonly used third-party providers, such as [Npgsql](https://github.com/npgsql/Npgsql.EntityFrameworkCore.PostgreSQL), [Pomelo MySQL](https://github.com/PomeloFoundation/Pomelo.EntityFrameworkCore.MySql), and [SQL Server Compact](https://github.com/ErikEJ/EntityFramework.SqlServerCompact).</span></span> <span data-ttu-id="71362-108">В частности, эти проекты настроены для расширения и выполнения функциональных тестов, публикуемых в NuGet.</span><span class="sxs-lookup"><span data-stu-id="71362-108">In particular, these projects are setup to extend from and run functional tests that we publish on NuGet.</span></span> <span data-ttu-id="71362-109">Этот тип установки настоятельно рекомендуется.</span><span class="sxs-lookup"><span data-stu-id="71362-109">This kind of setup is strongly recommended.</span></span>

## <a name="keeping-up-to-date-with-provider-changes"></a><span data-ttu-id="71362-110">Актуальность изменений поставщика</span><span class="sxs-lookup"><span data-stu-id="71362-110">Keeping up-to-date with provider changes</span></span>

<span data-ttu-id="71362-111">Начиная с работы после выпуска 2,1 мы создали [Журнал изменений](provider-log.md) , которые могут потребовать внесения соответствующих изменений в код поставщика.</span><span class="sxs-lookup"><span data-stu-id="71362-111">Starting with work after the 2.1 release, we have created a [log of changes](provider-log.md) that may need corresponding changes in provider code.</span></span> <span data-ttu-id="71362-112">Это предназначено для помощи при обновлении существующего поставщика для работы с новой версией EF Core.</span><span class="sxs-lookup"><span data-stu-id="71362-112">This is intended to help when updating an existing provider to work with a new version of EF Core.</span></span>

<span data-ttu-id="71362-113">До 2,1 мы использовали метки [`providers-beware`](https://github.com/aspnet/EntityFrameworkCore/labels/providers-beware) и [`providers-fyi`](https://github.com/aspnet/EntityFrameworkCore/labels/providers-fyi) на наших проблемах GitHub и запросах на вытягивание для аналогичного назначения.</span><span class="sxs-lookup"><span data-stu-id="71362-113">Prior to 2.1, we used the [`providers-beware`](https://github.com/aspnet/EntityFrameworkCore/labels/providers-beware) and [`providers-fyi`](https://github.com/aspnet/EntityFrameworkCore/labels/providers-fyi) labels on our GitHub issues and pull requests for a similar purpose.</span></span> <span data-ttu-id="71362-114">Мы континиуе с этими метками о проблемах, чтобы указать, какие рабочие элементы в данном выпуске могут также требовать выполнения работы в поставщиках.</span><span class="sxs-lookup"><span data-stu-id="71362-114">We will continiue to use these lables on issues to give an indication which work items in a given release may also require work to be done in providers.</span></span> <span data-ttu-id="71362-115">Метка `providers-beware` обычно означает, что реализация рабочего элемента может привести к нарушению работы поставщиков, а `providers-fyi` метка обычно означает, что поставщики не будут разорваны, но может потребоваться изменить код, например, чтобы включить новые функциональные возможности.</span><span class="sxs-lookup"><span data-stu-id="71362-115">A `providers-beware` label typically means that the implementation of an work item may break providers, while a `providers-fyi` label typically means that providers will not be broken, but code may need to be changed anyway, for example, to enable new functionality.</span></span>

## <a name="suggested-naming-of-third-party-providers"></a><span data-ttu-id="71362-116">Предлагаемое именование сторонних поставщиков</span><span class="sxs-lookup"><span data-stu-id="71362-116">Suggested naming of third party providers</span></span>

<span data-ttu-id="71362-117">Мы рекомендуем использовать следующее именование для пакетов NuGet.</span><span class="sxs-lookup"><span data-stu-id="71362-117">We suggest using the following naming for NuGet packages.</span></span> <span data-ttu-id="71362-118">Это согласуется с именами пакетов, доставленных командой EF Core.</span><span class="sxs-lookup"><span data-stu-id="71362-118">This is consistent with the names of packages delivered by the EF Core team.</span></span>

`<Optional project/company name>.EntityFrameworkCore.<Database engine name>`

<span data-ttu-id="71362-119">Пример:</span><span class="sxs-lookup"><span data-stu-id="71362-119">For example:</span></span>

* `Microsoft.EntityFrameworkCore.SqlServer`
* `Npgsql.EntityFrameworkCore.PostgreSQL`
* `EntityFrameworkCore.SqlServerCompact40`
