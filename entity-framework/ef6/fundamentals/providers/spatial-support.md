---
title: Поддержка пространственных типов - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 1097cb00-15f5-453d-90ed-bff9403d23e3
ms.openlocfilehash: 9c00e82c663daec219fe649a8d889afcc81564f7
ms.sourcegitcommit: 5e11125c9b838ce356d673ef5504aec477321724
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/25/2018
ms.locfileid: "50022279"
---
# <a name="provider-support-for-spatial-types"></a><span data-ttu-id="c88d9-102">Поддержка пространственных типов</span><span class="sxs-lookup"><span data-stu-id="c88d9-102">Provider Support for Spatial Types</span></span>
<span data-ttu-id="c88d9-103">Платформа Entity Framework поддерживает работу с пространственными данными через классы DbGeography или DbGeometry.</span><span class="sxs-lookup"><span data-stu-id="c88d9-103">Entity Framework supports working with spatial data through the DbGeography or DbGeometry classes.</span></span> <span data-ttu-id="c88d9-104">Эти классы используют функциональные возможности конкретной базы данных, предлагаемых поставщиком Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="c88d9-104">These classes rely on database-specific functionality offered by the Entity Framework provider.</span></span> <span data-ttu-id="c88d9-105">Не все поставщики поддерживают Пространственные данные и которые, возможно, дополнительные необходимые компоненты, например, установка сборок пространственный тип.</span><span class="sxs-lookup"><span data-stu-id="c88d9-105">Not all providers support spatial data and those that do may have additional prerequisites such as the installation of spatial type assemblies.</span></span> <span data-ttu-id="c88d9-106">Дополнительные сведения о поддержке поставщика для пространственных типов приведены ниже.</span><span class="sxs-lookup"><span data-stu-id="c88d9-106">More information about provider support for spatial types is provided below.</span></span>  

<span data-ttu-id="c88d9-107">Дополнительные сведения об использовании пространственных типов в приложении можно найти в двух пошаговых руководствах, один для Code First, другой — для Database First или Model First:</span><span class="sxs-lookup"><span data-stu-id="c88d9-107">Additional information on how to use spatial types in an application can be found in two walkthroughs, one for Code First, the other for Database First or Model First:</span></span>  

- [<span data-ttu-id="c88d9-108">Типы пространственных данных в коде сначала</span><span class="sxs-lookup"><span data-stu-id="c88d9-108">Spatial Data Types in Code First</span></span>](~/ef6/modeling/code-first/data-types/spatial.md)  
- [<span data-ttu-id="c88d9-109">Типы пространственных данных в конструкторе EF</span><span class="sxs-lookup"><span data-stu-id="c88d9-109">Spatial Data Types in EF Designer</span></span>](~/ef6/modeling/designer/data-types/spatial.md)  

## <a name="ef-releases-that-support-spatial-types"></a><span data-ttu-id="c88d9-110">Выпуски EF, которые поддерживают Пространственные типы</span><span class="sxs-lookup"><span data-stu-id="c88d9-110">EF releases that support spatial types</span></span>  

<span data-ttu-id="c88d9-111">Поддержка пространственных типов появилась в EF5.</span><span class="sxs-lookup"><span data-stu-id="c88d9-111">Support for spatial types was introduced in EF5.</span></span> <span data-ttu-id="c88d9-112">Тем не менее в EF5 Пространственные типы поддерживаются только если предназначен для приложения и запускается на .NET 4.5.</span><span class="sxs-lookup"><span data-stu-id="c88d9-112">However, in EF5 spatial types are only supported when the application targets and runs on .NET 4.5.</span></span>  

<span data-ttu-id="c88d9-113">Начиная с EF6, Пространственные типы поддерживаются для приложений, предназначенных для .NET 4 и .NET 4.5.</span><span class="sxs-lookup"><span data-stu-id="c88d9-113">Starting with EF6 spatial types are supported for applications targeting both .NET 4 and .NET 4.5.</span></span>  

## <a name="ef-providers-that-support-spatial-types"></a><span data-ttu-id="c88d9-114">Поставщики EF, которые поддерживают Пространственные типы</span><span class="sxs-lookup"><span data-stu-id="c88d9-114">EF providers that support spatial types</span></span>  

### <a name="ef5"></a><span data-ttu-id="c88d9-115">EF5</span><span class="sxs-lookup"><span data-stu-id="c88d9-115">EF5</span></span>  

<span data-ttu-id="c88d9-116">Поставщики Entity Framework для EF5, нам известно, что поддержка пространственных типов:</span><span class="sxs-lookup"><span data-stu-id="c88d9-116">The Entity Framework providers for EF5 that we are aware of that support spatial types are:</span></span>  

- <span data-ttu-id="c88d9-117">Поставщик Microsoft SQL Server</span><span class="sxs-lookup"><span data-stu-id="c88d9-117">Microsoft SQL Server provider</span></span>  
    - <span data-ttu-id="c88d9-118">Этот поставщик поставляется как часть EF5.</span><span class="sxs-lookup"><span data-stu-id="c88d9-118">This provider is shipped as part of EF5.</span></span>  
    - <span data-ttu-id="c88d9-119">Этот поставщик зависит от некоторые дополнительные библиотеки низкого уровня, которые может потребоваться установить — дополнительные сведения приведены ниже.</span><span class="sxs-lookup"><span data-stu-id="c88d9-119">This provider depends on some additional low-level libraries that may need to be installed—see below for details.</span></span>  
- [<span data-ttu-id="c88d9-120">Devart dotConnect для Oracle</span><span class="sxs-lookup"><span data-stu-id="c88d9-120">Devart dotConnect for Oracle</span></span>](http://www.devart.com/dotconnect/oracle/)  
    - <span data-ttu-id="c88d9-121">Это стороннего поставщика из Devart.</span><span class="sxs-lookup"><span data-stu-id="c88d9-121">This is a third-party provider from Devart.</span></span>  

<span data-ttu-id="c88d9-122">Если вы знаете об EF5 поставщика, поддерживающего Пространственные типы затем запустить контакта, и мы будем рады добавить его в этот список.</span><span class="sxs-lookup"><span data-stu-id="c88d9-122">If you know of an EF5 provider that supports spatial types then please get in contact and we will be happy to add it to this list.</span></span>  

### <a name="ef6"></a><span data-ttu-id="c88d9-123">EF6</span><span class="sxs-lookup"><span data-stu-id="c88d9-123">EF6</span></span>  

<span data-ttu-id="c88d9-124">Поставщики Entity Framework для EF6, нам известно, что поддержка пространственных типов:</span><span class="sxs-lookup"><span data-stu-id="c88d9-124">The Entity Framework providers for EF6 that we are aware of that support spatial types are:</span></span>  

- <span data-ttu-id="c88d9-125">Поставщик Microsoft SQL Server</span><span class="sxs-lookup"><span data-stu-id="c88d9-125">Microsoft SQL Server provider</span></span>  
    - <span data-ttu-id="c88d9-126">Этот поставщик поставляется как часть EF6.</span><span class="sxs-lookup"><span data-stu-id="c88d9-126">This provider is shipped as part of EF6.</span></span>  
    - <span data-ttu-id="c88d9-127">Этот поставщик зависит от некоторые дополнительные библиотеки низкого уровня, которые может потребоваться установить — дополнительные сведения приведены ниже.</span><span class="sxs-lookup"><span data-stu-id="c88d9-127">This provider depends on some additional low-level libraries that may need to be installed—see below for details.</span></span>  
- [<span data-ttu-id="c88d9-128">Devart dotConnect для Oracle</span><span class="sxs-lookup"><span data-stu-id="c88d9-128">Devart dotConnect for Oracle</span></span>](http://www.devart.com/dotconnect/oracle/)  
    - <span data-ttu-id="c88d9-129">Это стороннего поставщика из Devart.</span><span class="sxs-lookup"><span data-stu-id="c88d9-129">This is a third-party provider from Devart.</span></span>  

<span data-ttu-id="c88d9-130">Если вы знаете из EF6 поставщика, поддерживающего пространственных типов можно затем запустить обратитесь в службу, и мы будем рады добавить его в этот список.</span><span class="sxs-lookup"><span data-stu-id="c88d9-130">If you know of an EF6 provider that supports spatial types then please get in contact and we will be happy to add it to this list.</span></span>  

## <a name="prerequisites-for-spatial-types-with-microsoft-sql-server"></a><span data-ttu-id="c88d9-131">Необходимые условия для пространственных типов с помощью Microsoft SQL Server</span><span class="sxs-lookup"><span data-stu-id="c88d9-131">Prerequisites for spatial types with Microsoft SQL Server</span></span>  

<span data-ttu-id="c88d9-132">Пространственная поддержка SQL Server зависит от низкого уровня, связанные с SQL Server типов SqlGeography и SqlGeometry.</span><span class="sxs-lookup"><span data-stu-id="c88d9-132">SQL Server spatial support depends on the low-level, SQL Server-specific types SqlGeography and SqlGeometry.</span></span> <span data-ttu-id="c88d9-133">Эти типы расположены в сборку Microsoft.SqlServer.Types.dll, и эта сборка не поставляется как часть EF или как часть платформы .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="c88d9-133">These types live in Microsoft.SqlServer.Types.dll assembly, and this assembly is not shipped as part of EF or as part of the .NET Framework.</span></span>  

<span data-ttu-id="c88d9-134">При установке Visual Studio также часто установит версию SQL Server, и это будет включать копию файла Microsoft.SqlServer.Types.dll.</span><span class="sxs-lookup"><span data-stu-id="c88d9-134">When Visual Studio is installed it will often also install a version of SQL Server, and this will include installation of the Microsoft.SqlServer.Types.dll.</span></span>  

<span data-ttu-id="c88d9-135">Если SQL Server не установлен на компьютере, где вы хотите использовать Пространственные типы или Пространственные типы были исключены из установки SQL Server, вам потребуется установить их вручную.</span><span class="sxs-lookup"><span data-stu-id="c88d9-135">If SQL Server is not installed on the machine where you want to use spatial types, or if spatial types were excluded from the SQL Server installation, then you will need to install them manually.</span></span> <span data-ttu-id="c88d9-136">Типы могут быть установлены с помощью `SQLSysClrTypes.msi`, который является частью пакета дополнительных компонентов Microsoft SQL Server.</span><span class="sxs-lookup"><span data-stu-id="c88d9-136">The types can be installed using `SQLSysClrTypes.msi`, which is part of Microsoft SQL Server Feature Pack.</span></span> <span data-ttu-id="c88d9-137">Пространственные типы — SQL Server от версии, поэтому рекомендуется [поиск «SQL Server Feature Pack»](https://www.microsoft.com/search/result.aspx?q=sql+server+feature+pack) в центре загрузки Майкрософт, затем выберите и загрузите соответствующий до версии SQL Server, будет использоваться.</span><span class="sxs-lookup"><span data-stu-id="c88d9-137">Spatial types are SQL Server version-specific, so we recommend [search for "SQL Server Feature Pack"](https://www.microsoft.com/search/result.aspx?q=sql+server+feature+pack) in the Microsoft Download Center, then select and download the option that corresponds to the version of SQL Server you will use.</span></span>
