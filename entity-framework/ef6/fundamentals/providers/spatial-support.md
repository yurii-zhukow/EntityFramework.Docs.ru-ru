---
title: Поддержка пространственных типов в поставщике — EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 1097cb00-15f5-453d-90ed-bff9403d23e3
ms.openlocfilehash: 863f1b4551bd62160915eba90fee7ba6c49c169c
ms.sourcegitcommit: 708b18520321c587b2046ad2ea9fa7c48aeebfe5
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/09/2019
ms.locfileid: "72181594"
---
# <a name="provider-support-for-spatial-types"></a><span data-ttu-id="d5467-102">Поддержка пространственных типов в поставщике</span><span class="sxs-lookup"><span data-stu-id="d5467-102">Provider Support for Spatial Types</span></span>
<span data-ttu-id="d5467-103">Entity Framework поддерживает работу с пространственными данными с помощью классов заданное DbGeography или заданное DbGeometry.</span><span class="sxs-lookup"><span data-stu-id="d5467-103">Entity Framework supports working with spatial data through the DbGeography or DbGeometry classes.</span></span> <span data-ttu-id="d5467-104">Эти классы полагаются на функциональность базы данных, предлагаемую поставщиком Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="d5467-104">These classes rely on database-specific functionality offered by the Entity Framework provider.</span></span> <span data-ttu-id="d5467-105">Не все поставщики поддерживают пространственные данные и могут иметь дополнительные предварительные требования, например установку сборок пространственных типов.</span><span class="sxs-lookup"><span data-stu-id="d5467-105">Not all providers support spatial data and those that do may have additional prerequisites such as the installation of spatial type assemblies.</span></span> <span data-ttu-id="d5467-106">Ниже приведены дополнительные сведения о поддержке пространственных типов для поставщиков.</span><span class="sxs-lookup"><span data-stu-id="d5467-106">More information about provider support for spatial types is provided below.</span></span>  

<span data-ttu-id="d5467-107">Дополнительные сведения об использовании пространственных типов в приложении можно найти в двух пошаговых руководствах: один для Code First, другой — для Database First или Model First.</span><span class="sxs-lookup"><span data-stu-id="d5467-107">Additional information on how to use spatial types in an application can be found in two walkthroughs, one for Code First, the other for Database First or Model First:</span></span>  

- [<span data-ttu-id="d5467-108">Типы пространственных данных в Code First</span><span class="sxs-lookup"><span data-stu-id="d5467-108">Spatial Data Types in Code First</span></span>](~/ef6/modeling/code-first/data-types/spatial.md)  
- [<span data-ttu-id="d5467-109">Типы пространственных данных в конструкторе EF</span><span class="sxs-lookup"><span data-stu-id="d5467-109">Spatial Data Types in EF Designer</span></span>](~/ef6/modeling/designer/data-types/spatial.md)  

## <a name="ef-releases-that-support-spatial-types"></a><span data-ttu-id="d5467-110">Выпуски EF, поддерживающие пространственные типы</span><span class="sxs-lookup"><span data-stu-id="d5467-110">EF releases that support spatial types</span></span>  

<span data-ttu-id="d5467-111">Поддержка пространственных типов была представлена в EF5.</span><span class="sxs-lookup"><span data-stu-id="d5467-111">Support for spatial types was introduced in EF5.</span></span> <span data-ttu-id="d5467-112">Однако в пространственных типах EF5 поддерживается только в том случае, если приложение предназначено и работает на платформе .NET 4,5.</span><span class="sxs-lookup"><span data-stu-id="d5467-112">However, in EF5 spatial types are only supported when the application targets and runs on .NET 4.5.</span></span>  

<span data-ttu-id="d5467-113">Начиная с пространственных типов EF6, поддерживаются для приложений, предназначенных как для .NET 4, так и для .NET 4,5.</span><span class="sxs-lookup"><span data-stu-id="d5467-113">Starting with EF6 spatial types are supported for applications targeting both .NET 4 and .NET 4.5.</span></span>  

## <a name="ef-providers-that-support-spatial-types"></a><span data-ttu-id="d5467-114">Поставщики EF, поддерживающие пространственные типы</span><span class="sxs-lookup"><span data-stu-id="d5467-114">EF providers that support spatial types</span></span>  

### <a name="ef5"></a><span data-ttu-id="d5467-115">EF5</span><span class="sxs-lookup"><span data-stu-id="d5467-115">EF5</span></span>  

<span data-ttu-id="d5467-116">Поставщики Entity Framework для EF5, о которых мы осведомлены о том, что поддерживают пространственные типы:</span><span class="sxs-lookup"><span data-stu-id="d5467-116">The Entity Framework providers for EF5 that we are aware of that support spatial types are:</span></span>  

- <span data-ttu-id="d5467-117">Поставщик Microsoft SQL Server</span><span class="sxs-lookup"><span data-stu-id="d5467-117">Microsoft SQL Server provider</span></span>  
    - <span data-ttu-id="d5467-118">Этот поставщик поставляется в составе EF5.</span><span class="sxs-lookup"><span data-stu-id="d5467-118">This provider is shipped as part of EF5.</span></span>  
    - <span data-ttu-id="d5467-119">Этот поставщик зависит от некоторых дополнительных библиотек низкого уровня, которые, возможно, потребуется установить — дополнительные сведения см. ниже.</span><span class="sxs-lookup"><span data-stu-id="d5467-119">This provider depends on some additional low-level libraries that may need to be installed—see below for details.</span></span>  
- [<span data-ttu-id="d5467-120">Devart dotConnect для Oracle</span><span class="sxs-lookup"><span data-stu-id="d5467-120">Devart dotConnect for Oracle</span></span>](https://www.devart.com/dotconnect/oracle/)  
    - <span data-ttu-id="d5467-121">Это сторонний поставщик от Devart.</span><span class="sxs-lookup"><span data-stu-id="d5467-121">This is a third-party provider from Devart.</span></span>  

<span data-ttu-id="d5467-122">Если вы знакомы с поставщиком EF5, который поддерживает пространственные типы, то получите контакт, и мы будем рады добавить его в этот список.</span><span class="sxs-lookup"><span data-stu-id="d5467-122">If you know of an EF5 provider that supports spatial types then please get in contact and we will be happy to add it to this list.</span></span>  

### <a name="ef6"></a><span data-ttu-id="d5467-123">EF6</span><span class="sxs-lookup"><span data-stu-id="d5467-123">EF6</span></span>  

<span data-ttu-id="d5467-124">Поставщики Entity Framework для EF6, о которых мы осведомлены о том, что поддерживают пространственные типы:</span><span class="sxs-lookup"><span data-stu-id="d5467-124">The Entity Framework providers for EF6 that we are aware of that support spatial types are:</span></span>  

- <span data-ttu-id="d5467-125">Поставщик Microsoft SQL Server</span><span class="sxs-lookup"><span data-stu-id="d5467-125">Microsoft SQL Server provider</span></span>  
    - <span data-ttu-id="d5467-126">Этот поставщик поставляется в составе EF6.</span><span class="sxs-lookup"><span data-stu-id="d5467-126">This provider is shipped as part of EF6.</span></span>  
    - <span data-ttu-id="d5467-127">Этот поставщик зависит от некоторых дополнительных библиотек низкого уровня, которые, возможно, потребуется установить — дополнительные сведения см. ниже.</span><span class="sxs-lookup"><span data-stu-id="d5467-127">This provider depends on some additional low-level libraries that may need to be installed—see below for details.</span></span>  
- [<span data-ttu-id="d5467-128">Devart dotConnect для Oracle</span><span class="sxs-lookup"><span data-stu-id="d5467-128">Devart dotConnect for Oracle</span></span>](https://www.devart.com/dotconnect/oracle/)  
    - <span data-ttu-id="d5467-129">Это сторонний поставщик от Devart.</span><span class="sxs-lookup"><span data-stu-id="d5467-129">This is a third-party provider from Devart.</span></span>  

<span data-ttu-id="d5467-130">Если вы знакомы с поставщиком EF6, который поддерживает пространственные типы, то получите контакт, и мы будем рады добавить его в этот список.</span><span class="sxs-lookup"><span data-stu-id="d5467-130">If you know of an EF6 provider that supports spatial types then please get in contact and we will be happy to add it to this list.</span></span>  

## <a name="prerequisites-for-spatial-types-with-microsoft-sql-server"></a><span data-ttu-id="d5467-131">Необходимые условия для пространственных типов с Microsoft SQL Server</span><span class="sxs-lookup"><span data-stu-id="d5467-131">Prerequisites for spatial types with Microsoft SQL Server</span></span>  

<span data-ttu-id="d5467-132">SQL Server пространственной поддержки зависит от SQL Server типов SqlGeography и SqlGeometry низкого уровня.</span><span class="sxs-lookup"><span data-stu-id="d5467-132">SQL Server spatial support depends on the low-level, SQL Server-specific types SqlGeography and SqlGeometry.</span></span> <span data-ttu-id="d5467-133">Эти типы находятся в сборке Microsoft. SqlServer. types. dll, и эта сборка не поставляется как часть EF или как часть .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="d5467-133">These types live in Microsoft.SqlServer.Types.dll assembly, and this assembly is not shipped as part of EF or as part of the .NET Framework.</span></span>  

<span data-ttu-id="d5467-134">При установке Visual Studio часто также устанавливается версия SQL Server, которая включает в себя установку Microsoft. SqlServer. types. dll.</span><span class="sxs-lookup"><span data-stu-id="d5467-134">When Visual Studio is installed it will often also install a version of SQL Server, and this will include installation of the Microsoft.SqlServer.Types.dll.</span></span>  

<span data-ttu-id="d5467-135">Если SQL Server не установлен на компьютере, где вы хотите использовать пространственные типы, или если пространственные типы были исключены из установки SQL Server, необходимо установить их вручную.</span><span class="sxs-lookup"><span data-stu-id="d5467-135">If SQL Server is not installed on the machine where you want to use spatial types, or if spatial types were excluded from the SQL Server installation, then you will need to install them manually.</span></span> <span data-ttu-id="d5467-136">Типы можно установить с помощью `SQLSysClrTypes.msi`, который входит в состав Microsoft SQL Server пакета дополнительных компонентов.</span><span class="sxs-lookup"><span data-stu-id="d5467-136">The types can be installed using `SQLSysClrTypes.msi`, which is part of Microsoft SQL Server Feature Pack.</span></span> <span data-ttu-id="d5467-137">Пространственные типы SQL Server относятся к конкретной версии, поэтому рекомендуем [выполнить поиск по запросу "пакет компонентов SQL Server"](https://www.microsoft.com/search/result.aspx?q=sql+server+feature+pack) в центре загрузки Майкрософт, а затем выбрать и скачать параметр, соответствующий используемой версии SQL Server.</span><span class="sxs-lookup"><span data-stu-id="d5467-137">Spatial types are SQL Server version-specific, so we recommend [search for "SQL Server Feature Pack"](https://www.microsoft.com/search/result.aspx?q=sql+server+feature+pack) in the Microsoft Download Center, then select and download the option that corresponds to the version of SQL Server you will use.</span></span>
