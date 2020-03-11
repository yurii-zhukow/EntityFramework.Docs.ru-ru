---
title: Разрешение зависимостей — EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 32d19ac6-9186-4ae1-8655-64ee49da55d0
ms.openlocfilehash: 6082124481f5795bbcb62fff2bb6a58ecdcb48e4
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/06/2020
ms.locfileid: "78414797"
---
# <a name="dependency-resolution"></a><span data-ttu-id="eee3c-102">Разрешение зависимостей</span><span class="sxs-lookup"><span data-stu-id="eee3c-102">Dependency resolution</span></span>
> [!NOTE]
> <span data-ttu-id="eee3c-103">**Только в EF6 и более поздних версиях**. Функции, API и другие возможности, описанные на этой странице, появились в Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="eee3c-103">**EF6 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 6.</span></span> <span data-ttu-id="eee3c-104">При использовании более ранней версии могут быть неприменимы некоторые или все сведения.</span><span class="sxs-lookup"><span data-stu-id="eee3c-104">If you are using an earlier version, some or all of the information does not apply.</span></span>  

<span data-ttu-id="eee3c-105">Начиная с EF6, Entity Framework содержит универсальный механизм для получения необходимых реализаций служб.</span><span class="sxs-lookup"><span data-stu-id="eee3c-105">Starting with EF6, Entity Framework contains a general-purpose mechanism for obtaining implementations of services that it requires.</span></span> <span data-ttu-id="eee3c-106">То есть, когда EF использует экземпляр некоторых интерфейсов или базовых классов, он запросит конкретную реализацию интерфейса или базового класса, который будет использоваться.</span><span class="sxs-lookup"><span data-stu-id="eee3c-106">That is, when EF uses an instance of some interfaces or base classes it will ask for a concrete implementation of the interface or base class to use.</span></span> <span data-ttu-id="eee3c-107">Это достигается с помощью интерфейса Идбдепенденциресолвер:</span><span class="sxs-lookup"><span data-stu-id="eee3c-107">This is achieved through use of the IDbDependencyResolver interface:</span></span>  

``` csharp
public interface IDbDependencyResolver
{
    object GetService(Type type, object key);
}
```  

<span data-ttu-id="eee3c-108">Метод WebMethod обычно вызывается EF и обрабатывается реализацией Идбдепенденциресолвер, предоставляемой EF или приложением.</span><span class="sxs-lookup"><span data-stu-id="eee3c-108">The GetService method is typically called by EF and is handled by an implementation of IDbDependencyResolver provided either by EF or by the application.</span></span> <span data-ttu-id="eee3c-109">При вызове аргумент типа является интерфейсом или типом базового класса запрашиваемой службы, а ключевой объект имеет значение null или объект, предоставляющий контекстные сведения о запрашиваемой службе.</span><span class="sxs-lookup"><span data-stu-id="eee3c-109">When called, the type argument is the interface or base class type of the service being requested, and the key object is either null or an object providing contextual information about the requested service.</span></span>  

<span data-ttu-id="eee3c-110">Если не указано иное, то возвращаемый объект должен быть потокобезопасным, так как он может использоваться как одноэлементный.</span><span class="sxs-lookup"><span data-stu-id="eee3c-110">Unless otherwise stated any object returned must be thread-safe since it can be used as a singleton.</span></span> <span data-ttu-id="eee3c-111">Во многих случаях возвращаемый объект является фабрикой. в этом случае сама фабрика должна быть потокобезопасной, но объект, возвращаемый фабрикой, не обязательно должен быть потокобезопасным, так как новый экземпляр запрашивается из фабрики для каждого использования.</span><span class="sxs-lookup"><span data-stu-id="eee3c-111">In many cases the object returned is a factory in which case the factory itself must be thread-safe but the object returned from the factory does not need to be thread-safe since a new instance is requested from the factory for each use.</span></span>

<span data-ttu-id="eee3c-112">В этой статье не содержатся полные сведения о том, как реализовать Идбдепенденциресолвер, но он действует как ссылка на типы служб (то есть интерфейс и типы базовых классов), для которых EF вызывает GetObject и семантику ключевого объекта для каждого из них. вызывает.</span><span class="sxs-lookup"><span data-stu-id="eee3c-112">This article does not contain full details on how to implement IDbDependencyResolver, but instead acts as a reference for the service types (that is, the interface and base class types) for which EF calls GetService and the semantics of the key object for each of these calls.</span></span>

## <a name="systemdataentityidatabaseinitializertcontext"></a><span data-ttu-id="eee3c-113">System. Data. Entity. Идатабасеинитиализер < Тконтекст\></span><span class="sxs-lookup"><span data-stu-id="eee3c-113">System.Data.Entity.IDatabaseInitializer<TContext\></span></span>  

<span data-ttu-id="eee3c-114">**Представленная версия**: EF 6.0.0</span><span class="sxs-lookup"><span data-stu-id="eee3c-114">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="eee3c-115">**Возвращенный объект**: инициализатор базы данных для данного типа контекста</span><span class="sxs-lookup"><span data-stu-id="eee3c-115">**Object returned**: A database initializer for the given context type</span></span>  

<span data-ttu-id="eee3c-116">**Ключ**: не используется; будет иметь значение null</span><span class="sxs-lookup"><span data-stu-id="eee3c-116">**Key**: Not used; will be null</span></span>  

## <a name="funcsystemdataentitymigrationssqlmigrationsqlgenerator"></a><span data-ttu-id="eee3c-117">Func < System. Data. Entity. migrations. SQL. Мигратионсклженератор\></span><span class="sxs-lookup"><span data-stu-id="eee3c-117">Func<System.Data.Entity.Migrations.Sql.MigrationSqlGenerator\></span></span>  

<span data-ttu-id="eee3c-118">**Представленная версия**: EF 6.0.0</span><span class="sxs-lookup"><span data-stu-id="eee3c-118">**Version introduced**: EF6.0.0</span></span>

<span data-ttu-id="eee3c-119">**Возвращенный объект**: фабрика для создания генератора SQL, который можно использовать для миграций и других действий, которые приводят к созданию базы данных, например для создания базы данных с инициализаторами баз данных.</span><span class="sxs-lookup"><span data-stu-id="eee3c-119">**Object returned**: A factory to create a SQL generator that can be used for Migrations and other actions that cause a database to be created, such as database creation with database initializers.</span></span>  

<span data-ttu-id="eee3c-120">**Key**: строка, содержащая инвариантное имя поставщика ADO.NET, указывающее тип базы данных, для которой будет сформирован SQL.</span><span class="sxs-lookup"><span data-stu-id="eee3c-120">**Key**: A string containing the ADO.NET provider invariant name specifying the type of database for which SQL will be generated.</span></span> <span data-ttu-id="eee3c-121">Например, для ключа "System. Data. SqlClient" возвращается генератор SQL Server SQL.</span><span class="sxs-lookup"><span data-stu-id="eee3c-121">For example, the SQL Server SQL generator is returned for the key "System.Data.SqlClient".</span></span>  

>[!NOTE]
> <span data-ttu-id="eee3c-122">Дополнительные сведения о службах, связанных с поставщиками, в EF6 см. в разделе [модель поставщика EF6](~/ef6/fundamentals/providers/provider-model.md) .</span><span class="sxs-lookup"><span data-stu-id="eee3c-122">For more details on provider-related services in EF6 see the [EF6 provider model](~/ef6/fundamentals/providers/provider-model.md) section.</span></span>  

## <a name="systemdataentitycorecommondbproviderservices"></a><span data-ttu-id="eee3c-123">System. Data. Entity. Core. Common. DbProviderServices</span><span class="sxs-lookup"><span data-stu-id="eee3c-123">System.Data.Entity.Core.Common.DbProviderServices</span></span>  

<span data-ttu-id="eee3c-124">**Представленная версия**: EF 6.0.0</span><span class="sxs-lookup"><span data-stu-id="eee3c-124">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="eee3c-125">**Возвращенный объект**: поставщик EF, используемый для неизменяемого имени поставщика</span><span class="sxs-lookup"><span data-stu-id="eee3c-125">**Object returned**: The EF provider to use for a given provider invariant name</span></span>  

<span data-ttu-id="eee3c-126">**Key**: строка, содержащая инвариантное имя поставщика ADO.NET, указывающее тип базы данных, для которой требуется поставщик.</span><span class="sxs-lookup"><span data-stu-id="eee3c-126">**Key**: A string containing the ADO.NET provider invariant name specifying the type of database for which a provider is needed.</span></span> <span data-ttu-id="eee3c-127">Например, для ключа "System. Data. SqlClient" возвращается поставщик SQL Server.</span><span class="sxs-lookup"><span data-stu-id="eee3c-127">For example, the SQL Server provider is returned for the key "System.Data.SqlClient".</span></span>  

>[!NOTE]
> <span data-ttu-id="eee3c-128">Дополнительные сведения о службах, связанных с поставщиками, в EF6 см. в разделе [модель поставщика EF6](~/ef6/fundamentals/providers/provider-model.md) .</span><span class="sxs-lookup"><span data-stu-id="eee3c-128">For more details on provider-related services in EF6 see the [EF6 provider model](~/ef6/fundamentals/providers/provider-model.md) section.</span></span>  

## <a name="systemdataentityinfrastructureidbconnectionfactory"></a><span data-ttu-id="eee3c-129">System. Data. Entity. Infrastructure. Идбконнектионфактори</span><span class="sxs-lookup"><span data-stu-id="eee3c-129">System.Data.Entity.Infrastructure.IDbConnectionFactory</span></span>  

<span data-ttu-id="eee3c-130">**Представленная версия**: EF 6.0.0</span><span class="sxs-lookup"><span data-stu-id="eee3c-130">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="eee3c-131">**Возвращенный объект**: фабрика соединения, которая будет использоваться, когда EF создает подключение к базе данных по соглашению.</span><span class="sxs-lookup"><span data-stu-id="eee3c-131">**Object returned**: The connection factory that will be used when EF creates a database connection by convention.</span></span> <span data-ttu-id="eee3c-132">Это значит, что если соединение или строка подключения не переданы EF, а строка подключения не найдена в файле App. config или Web. config, то эта служба используется для создания соединения по соглашению.</span><span class="sxs-lookup"><span data-stu-id="eee3c-132">That is, when no connection or connection string is given to EF, and no connection string can be found in the app.config or web.config, then this service is used to create a connection by convention.</span></span> <span data-ttu-id="eee3c-133">Изменение фабрики соединений может позволить EF использовать базу данных другого типа (например, SQL Server Compact Edition) по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="eee3c-133">Changing the connection factory can allow EF to use a different type of database (for example, SQL Server Compact Edition) by default.</span></span>  

<span data-ttu-id="eee3c-134">**Ключ**: не используется; будет иметь значение null</span><span class="sxs-lookup"><span data-stu-id="eee3c-134">**Key**: Not used; will be null</span></span>  

>[!NOTE]
> <span data-ttu-id="eee3c-135">Дополнительные сведения о службах, связанных с поставщиками, в EF6 см. в разделе [модель поставщика EF6](~/ef6/fundamentals/providers/provider-model.md) .</span><span class="sxs-lookup"><span data-stu-id="eee3c-135">For more details on provider-related services in EF6 see the [EF6 provider model](~/ef6/fundamentals/providers/provider-model.md) section.</span></span>  

## <a name="systemdataentityinfrastructureimanifesttokenservice"></a><span data-ttu-id="eee3c-136">System. Data. Entity. Infrastructure. Иманифесттокенсервице</span><span class="sxs-lookup"><span data-stu-id="eee3c-136">System.Data.Entity.Infrastructure.IManifestTokenService</span></span>  

<span data-ttu-id="eee3c-137">**Представленная версия**: EF 6.0.0</span><span class="sxs-lookup"><span data-stu-id="eee3c-137">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="eee3c-138">**Возвращенный объект**: служба, которая может создать токен манифеста поставщика из соединения.</span><span class="sxs-lookup"><span data-stu-id="eee3c-138">**Object returned**: A service that can generate a provider manifest token from a connection.</span></span> <span data-ttu-id="eee3c-139">Эта служба обычно используется двумя способами.</span><span class="sxs-lookup"><span data-stu-id="eee3c-139">This service is typically used in two ways.</span></span> <span data-ttu-id="eee3c-140">Во-первых, ее можно использовать, чтобы избежать Code First соединения с базой данных при построении модели.</span><span class="sxs-lookup"><span data-stu-id="eee3c-140">First, it can be used to avoid Code First connecting to the database when building a model.</span></span> <span data-ttu-id="eee3c-141">Во-вторых, его можно использовать для принудительного Code First создания модели для конкретной версии базы данных, например, для принудительного применения модели для SQL Server 2005, даже если иногда используется SQL Server 2008.</span><span class="sxs-lookup"><span data-stu-id="eee3c-141">Second, it can be used to force Code First to build a model for a specific database version -- for example, to force a model for SQL Server 2005 even if sometimes SQL Server 2008 is used.</span></span>  

<span data-ttu-id="eee3c-142">**Время существования объекта**: Singleton — один и тот же объект может использоваться в разных потоках несколько раз и одновременно.</span><span class="sxs-lookup"><span data-stu-id="eee3c-142">**Object lifetime**: Singleton -- the same object may be used multiple times and concurrently by different threads</span></span>  

<span data-ttu-id="eee3c-143">**Ключ**: не используется; будет иметь значение null</span><span class="sxs-lookup"><span data-stu-id="eee3c-143">**Key**: Not used; will be null</span></span>  

## <a name="systemdataentityinfrastructureidbproviderfactoryservice"></a><span data-ttu-id="eee3c-144">System. Data. Entity. Infrastructure. Идбпровидерфакторисервице</span><span class="sxs-lookup"><span data-stu-id="eee3c-144">System.Data.Entity.Infrastructure.IDbProviderFactoryService</span></span>  

<span data-ttu-id="eee3c-145">**Представленная версия**: EF 6.0.0</span><span class="sxs-lookup"><span data-stu-id="eee3c-145">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="eee3c-146">**Возвращенный объект**: служба, которая может получить фабрику поставщика из заданного соединения.</span><span class="sxs-lookup"><span data-stu-id="eee3c-146">**Object returned**: A service that can obtain a provider factory from a given connection.</span></span> <span data-ttu-id="eee3c-147">В .NET 4,5 поставщик общедоступен из подключения.</span><span class="sxs-lookup"><span data-stu-id="eee3c-147">On .NET 4.5 the provider is publicly accessible from the connection.</span></span> <span data-ttu-id="eee3c-148">В .NET 4 реализация по умолчанию для этой службы использует некоторые эвристические методы для поиска соответствующего поставщика.</span><span class="sxs-lookup"><span data-stu-id="eee3c-148">On .NET 4 the default implementation of this service uses some heuristics to find the matching provider.</span></span> <span data-ttu-id="eee3c-149">В случае сбоя можно зарегистрировать новую реализацию этой службы, чтобы обеспечить соответствующее разрешение.</span><span class="sxs-lookup"><span data-stu-id="eee3c-149">If these fail then a new implementation of this service can be registered to provide an appropriate resolution.</span></span>  

<span data-ttu-id="eee3c-150">**Ключ**: не используется; будет иметь значение null</span><span class="sxs-lookup"><span data-stu-id="eee3c-150">**Key**: Not used; will be null</span></span>  

## <a name="funcdbcontext-systemdataentityinfrastructureidbmodelcachekey"></a><span data-ttu-id="eee3c-151">Func < DbContext, System. Data. Entity. Infrastructure. Идбмоделкачекэй\></span><span class="sxs-lookup"><span data-stu-id="eee3c-151">Func<DbContext, System.Data.Entity.Infrastructure.IDbModelCacheKey\></span></span>  

<span data-ttu-id="eee3c-152">**Представленная версия**: EF 6.0.0</span><span class="sxs-lookup"><span data-stu-id="eee3c-152">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="eee3c-153">**Возвращенный объект**: фабрика, которая создаст ключ кэша модели для данного контекста.</span><span class="sxs-lookup"><span data-stu-id="eee3c-153">**Object returned**: A factory that will generate a model cache key for a given context.</span></span> <span data-ttu-id="eee3c-154">По умолчанию EF кэширует по одной модели для каждого типа DbContext на каждого поставщика.</span><span class="sxs-lookup"><span data-stu-id="eee3c-154">By default, EF caches one model per DbContext type per provider.</span></span> <span data-ttu-id="eee3c-155">Другую реализацию этой службы можно использовать для добавления в ключ кэша других сведений, например имени схемы.</span><span class="sxs-lookup"><span data-stu-id="eee3c-155">A different implementation of this service can be used to add other information, such as schema name, to the cache key.</span></span>  

<span data-ttu-id="eee3c-156">**Ключ**: не используется; будет иметь значение null</span><span class="sxs-lookup"><span data-stu-id="eee3c-156">**Key**: Not used; will be null</span></span>  

## <a name="systemdataentityspatialdbspatialservices"></a><span data-ttu-id="eee3c-157">System. Data. Entity. пространственный. Дбспатиалсервицес</span><span class="sxs-lookup"><span data-stu-id="eee3c-157">System.Data.Entity.Spatial.DbSpatialServices</span></span>  

<span data-ttu-id="eee3c-158">**Представленная версия**: EF 6.0.0</span><span class="sxs-lookup"><span data-stu-id="eee3c-158">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="eee3c-159">**Возвращенный объект**. ПРОСТРАНСТВЕННЫЙ поставщик EF, который добавляет поддержку базового поставщика EF для пространственных и геометрических типов.</span><span class="sxs-lookup"><span data-stu-id="eee3c-159">**Object returned**: An EF spatial provider that adds support to the basic EF provider for geography and geometry spatial types.</span></span>  

<span data-ttu-id="eee3c-160">**Ключ**: дбсптиалсервицес запрашивается двумя способами.</span><span class="sxs-lookup"><span data-stu-id="eee3c-160">**Key**: DbSptialServices is asked for in two ways.</span></span> <span data-ttu-id="eee3c-161">Во-первых, пространственные службы, зависящие от поставщика, запрашиваются с помощью объекта Дбпровидеринфо (который содержит инвариантное имя и токен манифеста) в качестве ключа.</span><span class="sxs-lookup"><span data-stu-id="eee3c-161">First, provider-specific spatial services are requested using a DbProviderInfo object (which contains invariant name and manifest token) as the key.</span></span> <span data-ttu-id="eee3c-162">Во вторых, Дбспатиалсервицес может запрашиваться без ключа.</span><span class="sxs-lookup"><span data-stu-id="eee3c-162">Second, DbSpatialServices can be asked for with no key.</span></span> <span data-ttu-id="eee3c-163">Используется для разрешения "глобального пространственного поставщика", который используется при создании изолированных типов заданное DbGeography или заданное DbGeometry.</span><span class="sxs-lookup"><span data-stu-id="eee3c-163">This is used to resolve the "global spatial provider" which is used when creating stand-alone DbGeography or DbGeometry types.</span></span>  

>[!NOTE]
> <span data-ttu-id="eee3c-164">Дополнительные сведения о службах, связанных с поставщиками, в EF6 см. в разделе [модель поставщика EF6](~/ef6/fundamentals/providers/provider-model.md) .</span><span class="sxs-lookup"><span data-stu-id="eee3c-164">For more details on provider-related services in EF6 see the [EF6 provider model](~/ef6/fundamentals/providers/provider-model.md) section.</span></span>  

## <a name="funcsystemdataentityinfrastructureidbexecutionstrategy"></a><span data-ttu-id="eee3c-165">Func < System. Data. Entity. Infrastructure. IDbExecutionStrategy\></span><span class="sxs-lookup"><span data-stu-id="eee3c-165">Func<System.Data.Entity.Infrastructure.IDbExecutionStrategy\></span></span>  

<span data-ttu-id="eee3c-166">**Представленная версия**: EF 6.0.0</span><span class="sxs-lookup"><span data-stu-id="eee3c-166">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="eee3c-167">**Возвращенный объект**: фабрика для создания службы, которая позволяет поставщику реализовать повторные попытки или другое поведение при выполнении запросов и команд в базе данных.</span><span class="sxs-lookup"><span data-stu-id="eee3c-167">**Object returned**: A factory to create a service that allows a provider to implement retries or other behavior when queries and commands are executed against the database.</span></span> <span data-ttu-id="eee3c-168">Если реализация не предоставлена, EF будет просто выполнять команды и распространять все возникшие исключения.</span><span class="sxs-lookup"><span data-stu-id="eee3c-168">If no implementation is provided, then EF will simply execute the commands and propagate any exceptions thrown.</span></span> <span data-ttu-id="eee3c-169">Для SQL Server эта служба используется для предоставления политики повтора, которая особенно полезна при запуске на облачных серверах баз данных, таких как SQL Azure.</span><span class="sxs-lookup"><span data-stu-id="eee3c-169">For SQL Server this service is used to provide a retry policy which is especially useful when running against cloud-based database servers such as SQL Azure.</span></span>  

<span data-ttu-id="eee3c-170">**Key**: объект ексекутионстратегикэй, содержащий инвариантное имя поставщика, и, при необходимости, имя сервера, для которого будет использоваться стратегия выполнения.</span><span class="sxs-lookup"><span data-stu-id="eee3c-170">**Key**: An ExecutionStrategyKey object that contains the provider invariant name and optionally a server name for which the execution strategy will be used.</span></span>  

>[!NOTE]
> <span data-ttu-id="eee3c-171">Дополнительные сведения о службах, связанных с поставщиками, в EF6 см. в разделе [модель поставщика EF6](~/ef6/fundamentals/providers/provider-model.md) .</span><span class="sxs-lookup"><span data-stu-id="eee3c-171">For more details on provider-related services in EF6 see the [EF6 provider model](~/ef6/fundamentals/providers/provider-model.md) section.</span></span>  

## <a name="funcdbconnection-string-systemdataentitymigrationshistoryhistorycontext"></a><span data-ttu-id="eee3c-172">Func < DbConnection, String, System. Data. Entity. migrations. History. Хисториконтекст\></span><span class="sxs-lookup"><span data-stu-id="eee3c-172">Func<DbConnection, string, System.Data.Entity.Migrations.History.HistoryContext\></span></span>  

<span data-ttu-id="eee3c-173">**Представленная версия**: EF 6.0.0</span><span class="sxs-lookup"><span data-stu-id="eee3c-173">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="eee3c-174">**Возвращенный объект**: фабрика, позволяющая поставщику настроить сопоставление хисториконтекст с `__MigrationHistory` таблицей, используемой в миграции EF.</span><span class="sxs-lookup"><span data-stu-id="eee3c-174">**Object returned**: A factory that allows a provider to configure the mapping of the HistoryContext to the `__MigrationHistory` table used by EF Migrations.</span></span> <span data-ttu-id="eee3c-175">Хисториконтекст — это Code First DbContext. его можно настроить с помощью обычного API-интерфейса Fluent, чтобы изменить такие параметры, как имя таблицы и спецификации сопоставления столбцов.</span><span class="sxs-lookup"><span data-stu-id="eee3c-175">The HistoryContext is a Code First DbContext and can be configured using the normal fluent API to change things like the name of the table and the column mapping specifications.</span></span>  

<span data-ttu-id="eee3c-176">**Ключ**: не используется; будет иметь значение null</span><span class="sxs-lookup"><span data-stu-id="eee3c-176">**Key**: Not used; will be null</span></span>  

>[!NOTE]
> <span data-ttu-id="eee3c-177">Дополнительные сведения о службах, связанных с поставщиками, в EF6 см. в разделе [модель поставщика EF6](~/ef6/fundamentals/providers/provider-model.md) .</span><span class="sxs-lookup"><span data-stu-id="eee3c-177">For more details on provider-related services in EF6 see the [EF6 provider model](~/ef6/fundamentals/providers/provider-model.md) section.</span></span>  

## <a name="systemdatacommondbproviderfactory"></a><span data-ttu-id="eee3c-178">System. Data. Common. DbProviderFactory</span><span class="sxs-lookup"><span data-stu-id="eee3c-178">System.Data.Common.DbProviderFactory</span></span>  

<span data-ttu-id="eee3c-179">**Представленная версия**: EF 6.0.0</span><span class="sxs-lookup"><span data-stu-id="eee3c-179">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="eee3c-180">**Возвращенный объект**: поставщик ADO.NET, используемый для данного инвариантного имени поставщика.</span><span class="sxs-lookup"><span data-stu-id="eee3c-180">**Object returned**: The ADO.NET provider to use for a given provider invariant name.</span></span>  

<span data-ttu-id="eee3c-181">**Key**: строка, содержащая неизменяемое имя поставщика ADO.NET</span><span class="sxs-lookup"><span data-stu-id="eee3c-181">**Key**: A string containing the ADO.NET provider invariant name</span></span>  

>[!NOTE]
> <span data-ttu-id="eee3c-182">Эта служба обычно не изменяется напрямую, так как реализация по умолчанию использует обычную регистрацию поставщика ADO.NET.</span><span class="sxs-lookup"><span data-stu-id="eee3c-182">This service is not usually changed directly since the default implementation uses the normal ADO.NET provider registration.</span></span> <span data-ttu-id="eee3c-183">Дополнительные сведения о службах, связанных с поставщиками, в EF6 см. в разделе [модель поставщика EF6](~/ef6/fundamentals/providers/provider-model.md) .</span><span class="sxs-lookup"><span data-stu-id="eee3c-183">For more details on provider-related services in EF6 see the [EF6 provider model](~/ef6/fundamentals/providers/provider-model.md) section.</span></span>  

## <a name="systemdataentityinfrastructureiproviderinvariantname"></a><span data-ttu-id="eee3c-184">System. Data. Entity. Infrastructure. Ипровидеринвариантнаме</span><span class="sxs-lookup"><span data-stu-id="eee3c-184">System.Data.Entity.Infrastructure.IProviderInvariantName</span></span>  

<span data-ttu-id="eee3c-185">**Представленная версия**: EF 6.0.0</span><span class="sxs-lookup"><span data-stu-id="eee3c-185">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="eee3c-186">**Возвращенный объект**: служба, используемая для определения инвариантного имени поставщика для данного типа DbProviderFactory.</span><span class="sxs-lookup"><span data-stu-id="eee3c-186">**Object returned**: a service that is used to determine a provider invariant name for a given type of DbProviderFactory.</span></span> <span data-ttu-id="eee3c-187">Реализация по умолчанию этой службы использует регистрацию поставщика ADO.NET.</span><span class="sxs-lookup"><span data-stu-id="eee3c-187">The default implementation of this service uses the ADO.NET provider registration.</span></span> <span data-ttu-id="eee3c-188">Это означает, что если поставщик ADO.NET не зарегистрирован обычным образом, поскольку DbProviderFactory разрешается EF, то он также потребуется для разрешения этой службы.</span><span class="sxs-lookup"><span data-stu-id="eee3c-188">This means that if the ADO.NET provider is not registered in the normal way because DbProviderFactory is being resolved by EF, then it will also be necessary to resolve this service.</span></span>  

<span data-ttu-id="eee3c-189">**Key**: экземпляр DbProviderFactory, для которого требуется инвариантное имя.</span><span class="sxs-lookup"><span data-stu-id="eee3c-189">**Key**: The DbProviderFactory instance for which an invariant name is required.</span></span>  

>[!NOTE]
> <span data-ttu-id="eee3c-190">Дополнительные сведения о службах, связанных с поставщиками, в EF6 см. в разделе [модель поставщика EF6](~/ef6/fundamentals/providers/provider-model.md) .</span><span class="sxs-lookup"><span data-stu-id="eee3c-190">For more details on provider-related services in EF6 see the [EF6 provider model](~/ef6/fundamentals/providers/provider-model.md) section.</span></span>  

## <a name="systemdataentitycoremappingviewgenerationiviewassemblycache"></a><span data-ttu-id="eee3c-191">System. Data. Entity. Core. mapping. Виевженератион. Ивиевассембликаче</span><span class="sxs-lookup"><span data-stu-id="eee3c-191">System.Data.Entity.Core.Mapping.ViewGeneration.IViewAssemblyCache</span></span>  

<span data-ttu-id="eee3c-192">**Представленная версия**: EF 6.0.0</span><span class="sxs-lookup"><span data-stu-id="eee3c-192">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="eee3c-193">**Возвращенный объект**: кэш сборок, содержащих предварительно созданные представления.</span><span class="sxs-lookup"><span data-stu-id="eee3c-193">**Object returned**: a cache of the assemblies that contain pre-generated views.</span></span> <span data-ttu-id="eee3c-194">Замена обычно используется, чтобы сообщить EF, какие сборки содержат предварительно созданные представления без выполнения обнаружения.</span><span class="sxs-lookup"><span data-stu-id="eee3c-194">A replacement is typically used to let EF know which assemblies contain pre-generated views without doing any discovery.</span></span>  

<span data-ttu-id="eee3c-195">**Ключ**: не используется; будет иметь значение null</span><span class="sxs-lookup"><span data-stu-id="eee3c-195">**Key**: Not used; will be null</span></span>  

## <a name="systemdataentityinfrastructurepluralizationipluralizationservice"></a><span data-ttu-id="eee3c-196">System. Data. Entity. Infrastructure. plural. Иплурализатионсервице</span><span class="sxs-lookup"><span data-stu-id="eee3c-196">System.Data.Entity.Infrastructure.Pluralization.IPluralizationService</span></span>

<span data-ttu-id="eee3c-197">**Представленная версия**: EF 6.0.0</span><span class="sxs-lookup"><span data-stu-id="eee3c-197">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="eee3c-198">**Возвращенный объект**: служба, используемая EF к именам создания множественного и единственного числа.</span><span class="sxs-lookup"><span data-stu-id="eee3c-198">**Object returned**: a service used by EF to pluralize and singularize names.</span></span> <span data-ttu-id="eee3c-199">По умолчанию используется служба множественного преобразования английского языка.</span><span class="sxs-lookup"><span data-stu-id="eee3c-199">By default an English pluralization service is used.</span></span>  

<span data-ttu-id="eee3c-200">**Ключ**: не используется; будет иметь значение null</span><span class="sxs-lookup"><span data-stu-id="eee3c-200">**Key**: Not used; will be null</span></span>  

## <a name="systemdataentityinfrastructureinterceptionidbinterceptor"></a><span data-ttu-id="eee3c-201">System. Data. Entity. Infrastructure. перехват. Идбинтерцептор</span><span class="sxs-lookup"><span data-stu-id="eee3c-201">System.Data.Entity.Infrastructure.Interception.IDbInterceptor</span></span>  

<span data-ttu-id="eee3c-202">**Представленная версия**: EF 6.0.0</span><span class="sxs-lookup"><span data-stu-id="eee3c-202">**Version introduced**: EF6.0.0</span></span>

<span data-ttu-id="eee3c-203">**Возвращенные объекты**: любые перехватчики, которые должны быть зарегистрированы при запуске приложения.</span><span class="sxs-lookup"><span data-stu-id="eee3c-203">**Objects returned**: Any interceptors that should be registered when the application starts.</span></span> <span data-ttu-id="eee3c-204">Обратите внимание, что эти объекты запрашиваются с помощью вызова служб и все перехватчики, возвращаемые любым сопоставительом зависимостей, будут зарегистрированы.</span><span class="sxs-lookup"><span data-stu-id="eee3c-204">Note that these objects are requested using the GetServices call and all interceptors returned by any dependency resolver will registered.</span></span>

<span data-ttu-id="eee3c-205">**Ключ**: не используется; будет иметь значение null.</span><span class="sxs-lookup"><span data-stu-id="eee3c-205">**Key**: Not used; will be null.</span></span>  

## <a name="funcsystemdataentitydbcontext-actionstring-systemdataentityinfrastructureinterceptiondatabaselogformatter"></a><span data-ttu-id="eee3c-206">Func < System. Data. Entity. DbContext, действие < строку\>, System. Data. Entity. Infrastructure. перехват. Датабаселогформаттер\></span><span class="sxs-lookup"><span data-stu-id="eee3c-206">Func<System.Data.Entity.DbContext, Action<string\>, System.Data.Entity.Infrastructure.Interception.DatabaseLogFormatter\></span></span>  

<span data-ttu-id="eee3c-207">**Представленная версия**: EF 6.0.0</span><span class="sxs-lookup"><span data-stu-id="eee3c-207">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="eee3c-208">**Возвращенный объект**: фабрика, которая будет использоваться для создания модуля форматирования журнала базы данных, который будет использоваться при создании контекста. Свойство базы данных. log задано для данного контекста.</span><span class="sxs-lookup"><span data-stu-id="eee3c-208">**Object returned**: A factory that will be used to create the database log formatter that will be used when the context.Database.Log property is set on the given context.</span></span>  

<span data-ttu-id="eee3c-209">**Ключ**: не используется; будет иметь значение null.</span><span class="sxs-lookup"><span data-stu-id="eee3c-209">**Key**: Not used; will be null.</span></span>  

## <a name="funcsystemdataentitydbcontext"></a><span data-ttu-id="eee3c-210">Func < System. Data. Entity. DbContext\></span><span class="sxs-lookup"><span data-stu-id="eee3c-210">Func<System.Data.Entity.DbContext\></span></span>  

<span data-ttu-id="eee3c-211">**Представленная версия**: EF 6.1.0</span><span class="sxs-lookup"><span data-stu-id="eee3c-211">**Version introduced**: EF6.1.0</span></span>  

<span data-ttu-id="eee3c-212">**Возвращенный объект**: фабрика, которая будет использоваться для создания экземпляров контекста для миграции, если контекст не имеет доступного конструктора без параметров.</span><span class="sxs-lookup"><span data-stu-id="eee3c-212">**Object returned**: A factory that will be used to create context instances for Migrations when the context does not have an accessible parameterless constructor.</span></span>  

<span data-ttu-id="eee3c-213">**Key**: тип объекта для типа производного DbContext, для которого требуется фабрика.</span><span class="sxs-lookup"><span data-stu-id="eee3c-213">**Key**: The Type object for the type of the derived DbContext for which a factory is needed.</span></span>  

## <a name="funcsystemdataentitycoremetadataedmimetadataannotationserializer"></a><span data-ttu-id="eee3c-214">Func < System. Data. Entity. Core. Metadata. EDM. Иметадатааннотатионсериализер\></span><span class="sxs-lookup"><span data-stu-id="eee3c-214">Func<System.Data.Entity.Core.Metadata.Edm.IMetadataAnnotationSerializer\></span></span>  

<span data-ttu-id="eee3c-215">**Представленная версия**: EF 6.1.0</span><span class="sxs-lookup"><span data-stu-id="eee3c-215">**Version introduced**: EF6.1.0</span></span>  

<span data-ttu-id="eee3c-216">**Возвращенный объект**: фабрика, которая будет использоваться для создания сериализаторов для сериализации строго типизированных пользовательских заметок, чтобы их можно было сериализовать и ДЕСТЕРИЛИЗЕД в XML для использования в Code First migrations.</span><span class="sxs-lookup"><span data-stu-id="eee3c-216">**Object returned**: A factory that will be used to create serializers for serialization of strongly-typed custom annotations such that they can be serialized and desterilized into XML for use in Code First Migrations.</span></span>  

<span data-ttu-id="eee3c-217">**Key**: имя заметки, которая сериализуется или десериализуется.</span><span class="sxs-lookup"><span data-stu-id="eee3c-217">**Key**: The name of the annotation that is being serialized or deserialized.</span></span>  

## <a name="funcsystemdataentityinfrastructuretransactionhandler"></a><span data-ttu-id="eee3c-218">Func < System. Data. Entity. Infrastructure. Трансактионхандлер\></span><span class="sxs-lookup"><span data-stu-id="eee3c-218">Func<System.Data.Entity.Infrastructure.TransactionHandler\></span></span>  

<span data-ttu-id="eee3c-219">**Представленная версия**: EF 6.1.0</span><span class="sxs-lookup"><span data-stu-id="eee3c-219">**Version introduced**: EF6.1.0</span></span>  

<span data-ttu-id="eee3c-220">**Возвращенный объект**: фабрика, которая будет использоваться для создания обработчиков транзакций, чтобы особая обработка могла применяться в таких ситуациях, как обработка ошибок фиксации.</span><span class="sxs-lookup"><span data-stu-id="eee3c-220">**Object returned**: A factory that will be used to create handlers for transactions so that special handling can be applied for situations such as handling commit failures.</span></span>  

<span data-ttu-id="eee3c-221">**Key**: объект ексекутионстратегикэй, содержащий инвариантное имя поставщика, и, при необходимости, имя сервера, для которого будет использоваться обработчик транзакций.</span><span class="sxs-lookup"><span data-stu-id="eee3c-221">**Key**: An ExecutionStrategyKey object that contains the provider invariant name and optionally a server name for which the transaction handler will be used.</span></span>  
