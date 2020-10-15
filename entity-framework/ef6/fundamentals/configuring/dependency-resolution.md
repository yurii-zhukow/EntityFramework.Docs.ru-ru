---
title: Разрешение зависимостей — EF6
description: Разрешение зависимостей в Entity Framework 6
author: ajcvickers
ms.date: 10/23/2016
uid: ef6/fundamentals/configuring/dependency-resolution
ms.openlocfilehash: 84f2391cdad974bb14aecd7e8650d46e78e47b75
ms.sourcegitcommit: 0a25c03fa65ae6e0e0e3f66bac48d59eceb96a5a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/14/2020
ms.locfileid: "92063313"
---
# <a name="dependency-resolution"></a><span data-ttu-id="8e6c2-103">Разрешение зависимостей</span><span class="sxs-lookup"><span data-stu-id="8e6c2-103">Dependency resolution</span></span>
> [!NOTE]
> <span data-ttu-id="8e6c2-104">**Только в EF6 и более поздних версиях**. Функции, API и другие возможности, описанные на этой странице, появились в Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="8e6c2-104">**EF6 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 6.</span></span> <span data-ttu-id="8e6c2-105">При использовании более ранней версии могут быть неприменимы некоторые или все сведения.</span><span class="sxs-lookup"><span data-stu-id="8e6c2-105">If you are using an earlier version, some or all of the information does not apply.</span></span>  

<span data-ttu-id="8e6c2-106">Начиная с EF6, Entity Framework содержит универсальный механизм для получения необходимых реализаций служб.</span><span class="sxs-lookup"><span data-stu-id="8e6c2-106">Starting with EF6, Entity Framework contains a general-purpose mechanism for obtaining implementations of services that it requires.</span></span> <span data-ttu-id="8e6c2-107">То есть, когда EF использует экземпляр некоторых интерфейсов или базовых классов, он запросит конкретную реализацию интерфейса или базового класса, который будет использоваться.</span><span class="sxs-lookup"><span data-stu-id="8e6c2-107">That is, when EF uses an instance of some interfaces or base classes it will ask for a concrete implementation of the interface or base class to use.</span></span> <span data-ttu-id="8e6c2-108">Это достигается с помощью интерфейса Идбдепенденциресолвер:</span><span class="sxs-lookup"><span data-stu-id="8e6c2-108">This is achieved through use of the IDbDependencyResolver interface:</span></span>  

``` csharp
public interface IDbDependencyResolver
{
    object GetService(Type type, object key);
}
```  

<span data-ttu-id="8e6c2-109">Метод WebMethod обычно вызывается EF и обрабатывается реализацией Идбдепенденциресолвер, предоставляемой EF или приложением.</span><span class="sxs-lookup"><span data-stu-id="8e6c2-109">The GetService method is typically called by EF and is handled by an implementation of IDbDependencyResolver provided either by EF or by the application.</span></span> <span data-ttu-id="8e6c2-110">При вызове аргумент типа является интерфейсом или типом базового класса запрашиваемой службы, а ключевой объект имеет значение null или объект, предоставляющий контекстные сведения о запрашиваемой службе.</span><span class="sxs-lookup"><span data-stu-id="8e6c2-110">When called, the type argument is the interface or base class type of the service being requested, and the key object is either null or an object providing contextual information about the requested service.</span></span>  

<span data-ttu-id="8e6c2-111">Если не указано иное, то возвращаемый объект должен быть потокобезопасным, так как он может использоваться как одноэлементный.</span><span class="sxs-lookup"><span data-stu-id="8e6c2-111">Unless otherwise stated any object returned must be thread-safe since it can be used as a singleton.</span></span> <span data-ttu-id="8e6c2-112">Во многих случаях возвращаемый объект является фабрикой. в этом случае сама фабрика должна быть потокобезопасной, но объект, возвращаемый фабрикой, не обязательно должен быть потокобезопасным, так как новый экземпляр запрашивается из фабрики для каждого использования.</span><span class="sxs-lookup"><span data-stu-id="8e6c2-112">In many cases the object returned is a factory in which case the factory itself must be thread-safe but the object returned from the factory does not need to be thread-safe since a new instance is requested from the factory for each use.</span></span>

<span data-ttu-id="8e6c2-113">В этой статье не содержатся полные сведения о том, как реализовать Идбдепенденциресолвер, но он действует как ссылка на типы служб (то есть интерфейс и типы базовых классов), для которых EF вызывает GetObject и семантику ключевого объекта для каждого из этих вызовов.</span><span class="sxs-lookup"><span data-stu-id="8e6c2-113">This article does not contain full details on how to implement IDbDependencyResolver, but instead acts as a reference for the service types (that is, the interface and base class types) for which EF calls GetService and the semantics of the key object for each of these calls.</span></span>

## <a name="systemdataentityidatabaseinitializertcontext"></a><span data-ttu-id="8e6c2-114">System. Data. Entity. Идатабасеинитиализер<Тконтекст\></span><span class="sxs-lookup"><span data-stu-id="8e6c2-114">System.Data.Entity.IDatabaseInitializer<TContext\></span></span>  

<span data-ttu-id="8e6c2-115">**Представленная версия**: EF 6.0.0</span><span class="sxs-lookup"><span data-stu-id="8e6c2-115">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="8e6c2-116">**Возвращенный объект**: инициализатор базы данных для данного типа контекста</span><span class="sxs-lookup"><span data-stu-id="8e6c2-116">**Object returned**: A database initializer for the given context type</span></span>  

<span data-ttu-id="8e6c2-117">**Ключ**: не используется; будет иметь значение null</span><span class="sxs-lookup"><span data-stu-id="8e6c2-117">**Key**: Not used; will be null</span></span>  

## <a name="funcsystemdataentitymigrationssqlmigrationsqlgenerator"></a><span data-ttu-id="8e6c2-118">Func<System. Data. Entity. migrations. SQL. Мигратионсклженератор\></span><span class="sxs-lookup"><span data-stu-id="8e6c2-118">Func<System.Data.Entity.Migrations.Sql.MigrationSqlGenerator\></span></span>  

<span data-ttu-id="8e6c2-119">**Представленная версия**: EF 6.0.0</span><span class="sxs-lookup"><span data-stu-id="8e6c2-119">**Version introduced**: EF6.0.0</span></span>

<span data-ttu-id="8e6c2-120">**Возвращенный объект**: фабрика для создания генератора SQL, который можно использовать для миграций и других действий, которые приводят к созданию базы данных, например для создания базы данных с инициализаторами баз данных.</span><span class="sxs-lookup"><span data-stu-id="8e6c2-120">**Object returned**: A factory to create a SQL generator that can be used for Migrations and other actions that cause a database to be created, such as database creation with database initializers.</span></span>  

<span data-ttu-id="8e6c2-121">**Key**: строка, содержащая инвариантное имя поставщика ADO.NET, указывающее тип базы данных, для которой будет сформирован SQL.</span><span class="sxs-lookup"><span data-stu-id="8e6c2-121">**Key**: A string containing the ADO.NET provider invariant name specifying the type of database for which SQL will be generated.</span></span> <span data-ttu-id="8e6c2-122">Например, для ключа "System. Data. SqlClient" возвращается генератор SQL Server SQL.</span><span class="sxs-lookup"><span data-stu-id="8e6c2-122">For example, the SQL Server SQL generator is returned for the key "System.Data.SqlClient".</span></span>  

>[!NOTE]
> <span data-ttu-id="8e6c2-123">Дополнительные сведения о службах, связанных с поставщиками, в EF6 см. в разделе [модель поставщика EF6](xref:ef6/fundamentals/providers/provider-model) .</span><span class="sxs-lookup"><span data-stu-id="8e6c2-123">For more details on provider-related services in EF6 see the [EF6 provider model](xref:ef6/fundamentals/providers/provider-model) section.</span></span>  

## <a name="systemdataentitycorecommondbproviderservices"></a><span data-ttu-id="8e6c2-124">System. Data. Entity. Core. Common. DbProviderServices</span><span class="sxs-lookup"><span data-stu-id="8e6c2-124">System.Data.Entity.Core.Common.DbProviderServices</span></span>  

<span data-ttu-id="8e6c2-125">**Представленная версия**: EF 6.0.0</span><span class="sxs-lookup"><span data-stu-id="8e6c2-125">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="8e6c2-126">**Возвращенный объект**: поставщик EF, используемый для неизменяемого имени поставщика</span><span class="sxs-lookup"><span data-stu-id="8e6c2-126">**Object returned**: The EF provider to use for a given provider invariant name</span></span>  

<span data-ttu-id="8e6c2-127">**Key**: строка, содержащая инвариантное имя поставщика ADO.NET, указывающее тип базы данных, для которой требуется поставщик.</span><span class="sxs-lookup"><span data-stu-id="8e6c2-127">**Key**: A string containing the ADO.NET provider invariant name specifying the type of database for which a provider is needed.</span></span> <span data-ttu-id="8e6c2-128">Например, для ключа "System. Data. SqlClient" возвращается поставщик SQL Server.</span><span class="sxs-lookup"><span data-stu-id="8e6c2-128">For example, the SQL Server provider is returned for the key "System.Data.SqlClient".</span></span>  

>[!NOTE]
> <span data-ttu-id="8e6c2-129">Дополнительные сведения о службах, связанных с поставщиками, в EF6 см. в разделе [модель поставщика EF6](xref:ef6/fundamentals/providers/provider-model) .</span><span class="sxs-lookup"><span data-stu-id="8e6c2-129">For more details on provider-related services in EF6 see the [EF6 provider model](xref:ef6/fundamentals/providers/provider-model) section.</span></span>  

## <a name="systemdataentityinfrastructureidbconnectionfactory"></a><span data-ttu-id="8e6c2-130">System. Data. Entity. Infrastructure. Идбконнектионфактори</span><span class="sxs-lookup"><span data-stu-id="8e6c2-130">System.Data.Entity.Infrastructure.IDbConnectionFactory</span></span>  

<span data-ttu-id="8e6c2-131">**Представленная версия**: EF 6.0.0</span><span class="sxs-lookup"><span data-stu-id="8e6c2-131">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="8e6c2-132">**Возвращенный объект**: фабрика соединения, которая будет использоваться, когда EF создает подключение к базе данных по соглашению.</span><span class="sxs-lookup"><span data-stu-id="8e6c2-132">**Object returned**: The connection factory that will be used when EF creates a database connection by convention.</span></span> <span data-ttu-id="8e6c2-133">Это значит, что если соединение или строка подключения не переданы EF, а строка подключения не найдена в app.config или web.config, то эта служба используется для создания соединения по соглашению.</span><span class="sxs-lookup"><span data-stu-id="8e6c2-133">That is, when no connection or connection string is given to EF, and no connection string can be found in the app.config or web.config, then this service is used to create a connection by convention.</span></span> <span data-ttu-id="8e6c2-134">Изменение фабрики соединений может позволить EF использовать базу данных другого типа (например, SQL Server Compact Edition) по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="8e6c2-134">Changing the connection factory can allow EF to use a different type of database (for example, SQL Server Compact Edition) by default.</span></span>  

<span data-ttu-id="8e6c2-135">**Ключ**: не используется; будет иметь значение null</span><span class="sxs-lookup"><span data-stu-id="8e6c2-135">**Key**: Not used; will be null</span></span>  

>[!NOTE]
> <span data-ttu-id="8e6c2-136">Дополнительные сведения о службах, связанных с поставщиками, в EF6 см. в разделе [модель поставщика EF6](xref:ef6/fundamentals/providers/provider-model) .</span><span class="sxs-lookup"><span data-stu-id="8e6c2-136">For more details on provider-related services in EF6 see the [EF6 provider model](xref:ef6/fundamentals/providers/provider-model) section.</span></span>  

## <a name="systemdataentityinfrastructureimanifesttokenservice"></a><span data-ttu-id="8e6c2-137">System. Data. Entity. Infrastructure. Иманифесттокенсервице</span><span class="sxs-lookup"><span data-stu-id="8e6c2-137">System.Data.Entity.Infrastructure.IManifestTokenService</span></span>  

<span data-ttu-id="8e6c2-138">**Представленная версия**: EF 6.0.0</span><span class="sxs-lookup"><span data-stu-id="8e6c2-138">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="8e6c2-139">**Возвращенный объект**: служба, которая может создать токен манифеста поставщика из соединения.</span><span class="sxs-lookup"><span data-stu-id="8e6c2-139">**Object returned**: A service that can generate a provider manifest token from a connection.</span></span> <span data-ttu-id="8e6c2-140">Эта служба обычно используется двумя способами.</span><span class="sxs-lookup"><span data-stu-id="8e6c2-140">This service is typically used in two ways.</span></span> <span data-ttu-id="8e6c2-141">Во-первых, ее можно использовать, чтобы избежать Code First соединения с базой данных при построении модели.</span><span class="sxs-lookup"><span data-stu-id="8e6c2-141">First, it can be used to avoid Code First connecting to the database when building a model.</span></span> <span data-ttu-id="8e6c2-142">Во-вторых, его можно использовать для принудительного Code First создания модели для конкретной версии базы данных, например, для принудительного применения модели для SQL Server 2005, даже если иногда используется SQL Server 2008.</span><span class="sxs-lookup"><span data-stu-id="8e6c2-142">Second, it can be used to force Code First to build a model for a specific database version -- for example, to force a model for SQL Server 2005 even if sometimes SQL Server 2008 is used.</span></span>  

<span data-ttu-id="8e6c2-143">**Время существования объекта**: Singleton — один и тот же объект может использоваться в разных потоках несколько раз и одновременно.</span><span class="sxs-lookup"><span data-stu-id="8e6c2-143">**Object lifetime**: Singleton -- the same object may be used multiple times and concurrently by different threads</span></span>  

<span data-ttu-id="8e6c2-144">**Ключ**: не используется; будет иметь значение null</span><span class="sxs-lookup"><span data-stu-id="8e6c2-144">**Key**: Not used; will be null</span></span>  

## <a name="systemdataentityinfrastructureidbproviderfactoryservice"></a><span data-ttu-id="8e6c2-145">System. Data. Entity. Infrastructure. Идбпровидерфакторисервице</span><span class="sxs-lookup"><span data-stu-id="8e6c2-145">System.Data.Entity.Infrastructure.IDbProviderFactoryService</span></span>  

<span data-ttu-id="8e6c2-146">**Представленная версия**: EF 6.0.0</span><span class="sxs-lookup"><span data-stu-id="8e6c2-146">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="8e6c2-147">**Возвращенный объект**: служба, которая может получить фабрику поставщика из заданного соединения.</span><span class="sxs-lookup"><span data-stu-id="8e6c2-147">**Object returned**: A service that can obtain a provider factory from a given connection.</span></span> <span data-ttu-id="8e6c2-148">В .NET 4,5 поставщик общедоступен из подключения.</span><span class="sxs-lookup"><span data-stu-id="8e6c2-148">On .NET 4.5 the provider is publicly accessible from the connection.</span></span> <span data-ttu-id="8e6c2-149">В .NET 4 реализация по умолчанию для этой службы использует некоторые эвристические методы для поиска соответствующего поставщика.</span><span class="sxs-lookup"><span data-stu-id="8e6c2-149">On .NET 4 the default implementation of this service uses some heuristics to find the matching provider.</span></span> <span data-ttu-id="8e6c2-150">В случае сбоя можно зарегистрировать новую реализацию этой службы, чтобы обеспечить соответствующее разрешение.</span><span class="sxs-lookup"><span data-stu-id="8e6c2-150">If these fail then a new implementation of this service can be registered to provide an appropriate resolution.</span></span>  

<span data-ttu-id="8e6c2-151">**Ключ**: не используется; будет иметь значение null</span><span class="sxs-lookup"><span data-stu-id="8e6c2-151">**Key**: Not used; will be null</span></span>  

## <a name="funcdbcontext-systemdataentityinfrastructureidbmodelcachekey"></a><span data-ttu-id="8e6c2-152">Func<DbContext, System. Data. Entity. Infrastructure. Идбмоделкачекэй\></span><span class="sxs-lookup"><span data-stu-id="8e6c2-152">Func<DbContext, System.Data.Entity.Infrastructure.IDbModelCacheKey\></span></span>  

<span data-ttu-id="8e6c2-153">**Представленная версия**: EF 6.0.0</span><span class="sxs-lookup"><span data-stu-id="8e6c2-153">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="8e6c2-154">**Возвращенный объект**: фабрика, которая создаст ключ кэша модели для данного контекста.</span><span class="sxs-lookup"><span data-stu-id="8e6c2-154">**Object returned**: A factory that will generate a model cache key for a given context.</span></span> <span data-ttu-id="8e6c2-155">По умолчанию EF кэширует по одной модели для каждого типа DbContext на каждого поставщика.</span><span class="sxs-lookup"><span data-stu-id="8e6c2-155">By default, EF caches one model per DbContext type per provider.</span></span> <span data-ttu-id="8e6c2-156">Другую реализацию этой службы можно использовать для добавления в ключ кэша других сведений, например имени схемы.</span><span class="sxs-lookup"><span data-stu-id="8e6c2-156">A different implementation of this service can be used to add other information, such as schema name, to the cache key.</span></span>  

<span data-ttu-id="8e6c2-157">**Ключ**: не используется; будет иметь значение null</span><span class="sxs-lookup"><span data-stu-id="8e6c2-157">**Key**: Not used; will be null</span></span>  

## <a name="systemdataentityspatialdbspatialservices"></a><span data-ttu-id="8e6c2-158">System. Data. Entity. пространственный. Дбспатиалсервицес</span><span class="sxs-lookup"><span data-stu-id="8e6c2-158">System.Data.Entity.Spatial.DbSpatialServices</span></span>  

<span data-ttu-id="8e6c2-159">**Представленная версия**: EF 6.0.0</span><span class="sxs-lookup"><span data-stu-id="8e6c2-159">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="8e6c2-160">**Возвращенный объект**. ПРОСТРАНСТВЕННЫЙ поставщик EF, который добавляет поддержку базового поставщика EF для пространственных и геометрических типов.</span><span class="sxs-lookup"><span data-stu-id="8e6c2-160">**Object returned**: An EF spatial provider that adds support to the basic EF provider for geography and geometry spatial types.</span></span>  

<span data-ttu-id="8e6c2-161">**Ключ**: дбсптиалсервицес запрашивается двумя способами.</span><span class="sxs-lookup"><span data-stu-id="8e6c2-161">**Key**: DbSptialServices is asked for in two ways.</span></span> <span data-ttu-id="8e6c2-162">Во-первых, пространственные службы, зависящие от поставщика, запрашиваются с помощью объекта Дбпровидеринфо (который содержит инвариантное имя и токен манифеста) в качестве ключа.</span><span class="sxs-lookup"><span data-stu-id="8e6c2-162">First, provider-specific spatial services are requested using a DbProviderInfo object (which contains invariant name and manifest token) as the key.</span></span> <span data-ttu-id="8e6c2-163">Во вторых, Дбспатиалсервицес может запрашиваться без ключа.</span><span class="sxs-lookup"><span data-stu-id="8e6c2-163">Second, DbSpatialServices can be asked for with no key.</span></span> <span data-ttu-id="8e6c2-164">Используется для разрешения "глобального пространственного поставщика", который используется при создании изолированных типов заданное DbGeography или заданное DbGeometry.</span><span class="sxs-lookup"><span data-stu-id="8e6c2-164">This is used to resolve the "global spatial provider" which is used when creating stand-alone DbGeography or DbGeometry types.</span></span>  

>[!NOTE]
> <span data-ttu-id="8e6c2-165">Дополнительные сведения о службах, связанных с поставщиками, в EF6 см. в разделе [модель поставщика EF6](xref:ef6/fundamentals/providers/provider-model) .</span><span class="sxs-lookup"><span data-stu-id="8e6c2-165">For more details on provider-related services in EF6 see the [EF6 provider model](xref:ef6/fundamentals/providers/provider-model) section.</span></span>  

## <a name="funcsystemdataentityinfrastructureidbexecutionstrategy"></a><span data-ttu-id="8e6c2-166">Func<System. Data. Entity. Infrastructure. IDbExecutionStrategy\></span><span class="sxs-lookup"><span data-stu-id="8e6c2-166">Func<System.Data.Entity.Infrastructure.IDbExecutionStrategy\></span></span>  

<span data-ttu-id="8e6c2-167">**Представленная версия**: EF 6.0.0</span><span class="sxs-lookup"><span data-stu-id="8e6c2-167">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="8e6c2-168">**Возвращенный объект**: фабрика для создания службы, которая позволяет поставщику реализовать повторные попытки или другое поведение при выполнении запросов и команд в базе данных.</span><span class="sxs-lookup"><span data-stu-id="8e6c2-168">**Object returned**: A factory to create a service that allows a provider to implement retries or other behavior when queries and commands are executed against the database.</span></span> <span data-ttu-id="8e6c2-169">Если реализация не предоставлена, EF будет просто выполнять команды и распространять все возникшие исключения.</span><span class="sxs-lookup"><span data-stu-id="8e6c2-169">If no implementation is provided, then EF will simply execute the commands and propagate any exceptions thrown.</span></span> <span data-ttu-id="8e6c2-170">Для SQL Server эта служба используется для предоставления политики повтора, которая особенно полезна при запуске на облачных серверах баз данных, таких как SQL Azure.</span><span class="sxs-lookup"><span data-stu-id="8e6c2-170">For SQL Server this service is used to provide a retry policy which is especially useful when running against cloud-based database servers such as SQL Azure.</span></span>  

<span data-ttu-id="8e6c2-171">**Key**: объект ексекутионстратегикэй, содержащий инвариантное имя поставщика, и, при необходимости, имя сервера, для которого будет использоваться стратегия выполнения.</span><span class="sxs-lookup"><span data-stu-id="8e6c2-171">**Key**: An ExecutionStrategyKey object that contains the provider invariant name and optionally a server name for which the execution strategy will be used.</span></span>  

>[!NOTE]
> <span data-ttu-id="8e6c2-172">Дополнительные сведения о службах, связанных с поставщиками, в EF6 см. в разделе [модель поставщика EF6](xref:ef6/fundamentals/providers/provider-model) .</span><span class="sxs-lookup"><span data-stu-id="8e6c2-172">For more details on provider-related services in EF6 see the [EF6 provider model](xref:ef6/fundamentals/providers/provider-model) section.</span></span>  

## <a name="funcdbconnection-string-systemdataentitymigrationshistoryhistorycontext"></a><span data-ttu-id="8e6c2-173">Func<DbConnection, String, System. Data. Entity. migrations. History. Хисториконтекст\></span><span class="sxs-lookup"><span data-stu-id="8e6c2-173">Func<DbConnection, string, System.Data.Entity.Migrations.History.HistoryContext\></span></span>  

<span data-ttu-id="8e6c2-174">**Представленная версия**: EF 6.0.0</span><span class="sxs-lookup"><span data-stu-id="8e6c2-174">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="8e6c2-175">**Возвращенный объект**: фабрика, позволяющая поставщику настроить сопоставление хисториконтекст с `__MigrationHistory` таблицей, используемой в миграции EF.</span><span class="sxs-lookup"><span data-stu-id="8e6c2-175">**Object returned**: A factory that allows a provider to configure the mapping of the HistoryContext to the `__MigrationHistory` table used by EF Migrations.</span></span> <span data-ttu-id="8e6c2-176">Хисториконтекст — это Code First DbContext. его можно настроить с помощью обычного API-интерфейса Fluent, чтобы изменить такие параметры, как имя таблицы и спецификации сопоставления столбцов.</span><span class="sxs-lookup"><span data-stu-id="8e6c2-176">The HistoryContext is a Code First DbContext and can be configured using the normal fluent API to change things like the name of the table and the column mapping specifications.</span></span>  

<span data-ttu-id="8e6c2-177">**Ключ**: не используется; будет иметь значение null</span><span class="sxs-lookup"><span data-stu-id="8e6c2-177">**Key**: Not used; will be null</span></span>  

>[!NOTE]
> <span data-ttu-id="8e6c2-178">Дополнительные сведения о службах, связанных с поставщиками, в EF6 см. в разделе [модель поставщика EF6](xref:ef6/fundamentals/providers/provider-model) .</span><span class="sxs-lookup"><span data-stu-id="8e6c2-178">For more details on provider-related services in EF6 see the [EF6 provider model](xref:ef6/fundamentals/providers/provider-model) section.</span></span>  

## <a name="systemdatacommondbproviderfactory"></a><span data-ttu-id="8e6c2-179">System. Data. Common. DbProviderFactory</span><span class="sxs-lookup"><span data-stu-id="8e6c2-179">System.Data.Common.DbProviderFactory</span></span>  

<span data-ttu-id="8e6c2-180">**Представленная версия**: EF 6.0.0</span><span class="sxs-lookup"><span data-stu-id="8e6c2-180">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="8e6c2-181">**Возвращенный объект**: поставщик ADO.NET, используемый для данного инвариантного имени поставщика.</span><span class="sxs-lookup"><span data-stu-id="8e6c2-181">**Object returned**: The ADO.NET provider to use for a given provider invariant name.</span></span>  

<span data-ttu-id="8e6c2-182">**Key**: строка, содержащая неизменяемое имя поставщика ADO.NET</span><span class="sxs-lookup"><span data-stu-id="8e6c2-182">**Key**: A string containing the ADO.NET provider invariant name</span></span>  

>[!NOTE]
> <span data-ttu-id="8e6c2-183">Эта служба обычно не изменяется напрямую, так как реализация по умолчанию использует обычную регистрацию поставщика ADO.NET.</span><span class="sxs-lookup"><span data-stu-id="8e6c2-183">This service is not usually changed directly since the default implementation uses the normal ADO.NET provider registration.</span></span> <span data-ttu-id="8e6c2-184">Дополнительные сведения о службах, связанных с поставщиками, в EF6 см. в разделе [модель поставщика EF6](xref:ef6/fundamentals/providers/provider-model) .</span><span class="sxs-lookup"><span data-stu-id="8e6c2-184">For more details on provider-related services in EF6 see the [EF6 provider model](xref:ef6/fundamentals/providers/provider-model) section.</span></span>  

## <a name="systemdataentityinfrastructureiproviderinvariantname"></a><span data-ttu-id="8e6c2-185">System. Data. Entity. Infrastructure. Ипровидеринвариантнаме</span><span class="sxs-lookup"><span data-stu-id="8e6c2-185">System.Data.Entity.Infrastructure.IProviderInvariantName</span></span>  

<span data-ttu-id="8e6c2-186">**Представленная версия**: EF 6.0.0</span><span class="sxs-lookup"><span data-stu-id="8e6c2-186">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="8e6c2-187">**Возвращенный объект**: служба, используемая для определения инвариантного имени поставщика для данного типа DbProviderFactory.</span><span class="sxs-lookup"><span data-stu-id="8e6c2-187">**Object returned**: a service that is used to determine a provider invariant name for a given type of DbProviderFactory.</span></span> <span data-ttu-id="8e6c2-188">Реализация по умолчанию этой службы использует регистрацию поставщика ADO.NET.</span><span class="sxs-lookup"><span data-stu-id="8e6c2-188">The default implementation of this service uses the ADO.NET provider registration.</span></span> <span data-ttu-id="8e6c2-189">Это означает, что если поставщик ADO.NET не зарегистрирован обычным образом, поскольку DbProviderFactory разрешается EF, то он также потребуется для разрешения этой службы.</span><span class="sxs-lookup"><span data-stu-id="8e6c2-189">This means that if the ADO.NET provider is not registered in the normal way because DbProviderFactory is being resolved by EF, then it will also be necessary to resolve this service.</span></span>  

<span data-ttu-id="8e6c2-190">**Key**: экземпляр DbProviderFactory, для которого требуется инвариантное имя.</span><span class="sxs-lookup"><span data-stu-id="8e6c2-190">**Key**: The DbProviderFactory instance for which an invariant name is required.</span></span>  

>[!NOTE]
> <span data-ttu-id="8e6c2-191">Дополнительные сведения о службах, связанных с поставщиками, в EF6 см. в разделе [модель поставщика EF6](xref:ef6/fundamentals/providers/provider-model) .</span><span class="sxs-lookup"><span data-stu-id="8e6c2-191">For more details on provider-related services in EF6 see the [EF6 provider model](xref:ef6/fundamentals/providers/provider-model) section.</span></span>  

## <a name="systemdataentitycoremappingviewgenerationiviewassemblycache"></a><span data-ttu-id="8e6c2-192">System. Data. Entity. Core. mapping. Виевженератион. Ивиевассембликаче</span><span class="sxs-lookup"><span data-stu-id="8e6c2-192">System.Data.Entity.Core.Mapping.ViewGeneration.IViewAssemblyCache</span></span>  

<span data-ttu-id="8e6c2-193">**Представленная версия**: EF 6.0.0</span><span class="sxs-lookup"><span data-stu-id="8e6c2-193">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="8e6c2-194">**Возвращенный объект**: кэш сборок, содержащих предварительно созданные представления.</span><span class="sxs-lookup"><span data-stu-id="8e6c2-194">**Object returned**: a cache of the assemblies that contain pre-generated views.</span></span> <span data-ttu-id="8e6c2-195">Замена обычно используется, чтобы сообщить EF, какие сборки содержат предварительно созданные представления без выполнения обнаружения.</span><span class="sxs-lookup"><span data-stu-id="8e6c2-195">A replacement is typically used to let EF know which assemblies contain pre-generated views without doing any discovery.</span></span>  

<span data-ttu-id="8e6c2-196">**Ключ**: не используется; будет иметь значение null</span><span class="sxs-lookup"><span data-stu-id="8e6c2-196">**Key**: Not used; will be null</span></span>  

## <a name="systemdataentityinfrastructurepluralizationipluralizationservice"></a><span data-ttu-id="8e6c2-197">System. Data. Entity. Infrastructure. plural. Иплурализатионсервице</span><span class="sxs-lookup"><span data-stu-id="8e6c2-197">System.Data.Entity.Infrastructure.Pluralization.IPluralizationService</span></span>

<span data-ttu-id="8e6c2-198">**Представленная версия**: EF 6.0.0</span><span class="sxs-lookup"><span data-stu-id="8e6c2-198">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="8e6c2-199">**Возвращенный объект**: служба, используемая EF к именам создания множественного и единственного числа.</span><span class="sxs-lookup"><span data-stu-id="8e6c2-199">**Object returned**: a service used by EF to pluralize and singularize names.</span></span> <span data-ttu-id="8e6c2-200">По умолчанию используется служба множественного преобразования английского языка.</span><span class="sxs-lookup"><span data-stu-id="8e6c2-200">By default an English pluralization service is used.</span></span>  

<span data-ttu-id="8e6c2-201">**Ключ**: не используется; будет иметь значение null</span><span class="sxs-lookup"><span data-stu-id="8e6c2-201">**Key**: Not used; will be null</span></span>  

## <a name="systemdataentityinfrastructureinterceptionidbinterceptor"></a><span data-ttu-id="8e6c2-202">System. Data. Entity. Infrastructure. перехват. Идбинтерцептор</span><span class="sxs-lookup"><span data-stu-id="8e6c2-202">System.Data.Entity.Infrastructure.Interception.IDbInterceptor</span></span>  

<span data-ttu-id="8e6c2-203">**Представленная версия**: EF 6.0.0</span><span class="sxs-lookup"><span data-stu-id="8e6c2-203">**Version introduced**: EF6.0.0</span></span>

<span data-ttu-id="8e6c2-204">**Возвращенные объекты**: любые перехватчики, которые должны быть зарегистрированы при запуске приложения.</span><span class="sxs-lookup"><span data-stu-id="8e6c2-204">**Objects returned**: Any interceptors that should be registered when the application starts.</span></span> <span data-ttu-id="8e6c2-205">Обратите внимание, что эти объекты запрашиваются с помощью вызова служб и все перехватчики, возвращаемые любым сопоставительом зависимостей, будут зарегистрированы.</span><span class="sxs-lookup"><span data-stu-id="8e6c2-205">Note that these objects are requested using the GetServices call and all interceptors returned by any dependency resolver will registered.</span></span>

<span data-ttu-id="8e6c2-206">**Ключ**: не используется; будет иметь значение null.</span><span class="sxs-lookup"><span data-stu-id="8e6c2-206">**Key**: Not used; will be null.</span></span>  

## <a name="funcsystemdataentitydbcontext-actionstring-systemdataentityinfrastructureinterceptiondatabaselogformatter"></a><span data-ttu-id="8e6c2-207">Func<System. Data. Entity. DbContext, действие<строка \> , System. Data. Entity. Infrastructure. перехват. датабаселогформаттер\></span><span class="sxs-lookup"><span data-stu-id="8e6c2-207">Func<System.Data.Entity.DbContext, Action<string\>, System.Data.Entity.Infrastructure.Interception.DatabaseLogFormatter\></span></span>  

<span data-ttu-id="8e6c2-208">**Представленная версия**: EF 6.0.0</span><span class="sxs-lookup"><span data-stu-id="8e6c2-208">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="8e6c2-209">**Возвращенный объект**: фабрика, которая будет использоваться для создания модуля форматирования журнала базы данных, который будет использоваться при создании контекста. Свойство базы данных. log задано для данного контекста.</span><span class="sxs-lookup"><span data-stu-id="8e6c2-209">**Object returned**: A factory that will be used to create the database log formatter that will be used when the context.Database.Log property is set on the given context.</span></span>  

<span data-ttu-id="8e6c2-210">**Ключ**: не используется; будет иметь значение null.</span><span class="sxs-lookup"><span data-stu-id="8e6c2-210">**Key**: Not used; will be null.</span></span>  

## <a name="funcsystemdataentitydbcontext"></a><span data-ttu-id="8e6c2-211">Func<System. Data. Entity. DbContext\></span><span class="sxs-lookup"><span data-stu-id="8e6c2-211">Func<System.Data.Entity.DbContext\></span></span>  

<span data-ttu-id="8e6c2-212">**Представленная версия**: EF 6.1.0</span><span class="sxs-lookup"><span data-stu-id="8e6c2-212">**Version introduced**: EF6.1.0</span></span>  

<span data-ttu-id="8e6c2-213">**Возвращенный объект**: фабрика, которая будет использоваться для создания экземпляров контекста для миграции, если контекст не имеет доступного конструктора без параметров.</span><span class="sxs-lookup"><span data-stu-id="8e6c2-213">**Object returned**: A factory that will be used to create context instances for Migrations when the context does not have an accessible parameterless constructor.</span></span>  

<span data-ttu-id="8e6c2-214">**Key**: тип объекта для типа производного DbContext, для которого требуется фабрика.</span><span class="sxs-lookup"><span data-stu-id="8e6c2-214">**Key**: The Type object for the type of the derived DbContext for which a factory is needed.</span></span>  

## <a name="funcsystemdataentitycoremetadataedmimetadataannotationserializer"></a><span data-ttu-id="8e6c2-215">Func<System. Data. Entity. Core. Metadata. EDM. Иметадатааннотатионсериализер\></span><span class="sxs-lookup"><span data-stu-id="8e6c2-215">Func<System.Data.Entity.Core.Metadata.Edm.IMetadataAnnotationSerializer\></span></span>  

<span data-ttu-id="8e6c2-216">**Представленная версия**: EF 6.1.0</span><span class="sxs-lookup"><span data-stu-id="8e6c2-216">**Version introduced**: EF6.1.0</span></span>  

<span data-ttu-id="8e6c2-217">**Возвращенный объект**: фабрика, которая будет использоваться для создания сериализаторов для сериализации строго типизированных пользовательских заметок, чтобы их можно было сериализовать и ДЕСТЕРИЛИЗЕД в XML для использования в Code First migrations.</span><span class="sxs-lookup"><span data-stu-id="8e6c2-217">**Object returned**: A factory that will be used to create serializers for serialization of strongly-typed custom annotations such that they can be serialized and desterilized into XML for use in Code First Migrations.</span></span>  

<span data-ttu-id="8e6c2-218">**Key**: имя заметки, которая сериализуется или десериализуется.</span><span class="sxs-lookup"><span data-stu-id="8e6c2-218">**Key**: The name of the annotation that is being serialized or deserialized.</span></span>  

## <a name="funcsystemdataentityinfrastructuretransactionhandler"></a><span data-ttu-id="8e6c2-219">Func<System. Data. Entity. Infrastructure. Трансактионхандлер\></span><span class="sxs-lookup"><span data-stu-id="8e6c2-219">Func<System.Data.Entity.Infrastructure.TransactionHandler\></span></span>  

<span data-ttu-id="8e6c2-220">**Представленная версия**: EF 6.1.0</span><span class="sxs-lookup"><span data-stu-id="8e6c2-220">**Version introduced**: EF6.1.0</span></span>  

<span data-ttu-id="8e6c2-221">**Возвращенный объект**: фабрика, которая будет использоваться для создания обработчиков транзакций, чтобы особая обработка могла применяться в таких ситуациях, как обработка ошибок фиксации.</span><span class="sxs-lookup"><span data-stu-id="8e6c2-221">**Object returned**: A factory that will be used to create handlers for transactions so that special handling can be applied for situations such as handling commit failures.</span></span>  

<span data-ttu-id="8e6c2-222">**Key**: объект ексекутионстратегикэй, содержащий инвариантное имя поставщика, и, при необходимости, имя сервера, для которого будет использоваться обработчик транзакций.</span><span class="sxs-lookup"><span data-stu-id="8e6c2-222">**Key**: An ExecutionStrategyKey object that contains the provider invariant name and optionally a server name for which the transaction handler will be used.</span></span>  
