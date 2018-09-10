---
title: Разрешение зависимостей - EF6
author: divega
ms.date: 2016-10-23
ms.assetid: 32d19ac6-9186-4ae1-8655-64ee49da55d0
ms.openlocfilehash: c6c56c3048e17a5c888ffe564e7606abf8b0c4ed
ms.sourcegitcommit: 0d36e8ff0892b7f034b765b15e041f375f88579a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/09/2018
ms.locfileid: "44251249"
---
# <a name="dependency-resolution"></a><span data-ttu-id="9c5c5-102">Разрешение зависимостей</span><span class="sxs-lookup"><span data-stu-id="9c5c5-102">Dependency resolution</span></span>
> [!NOTE]
> <span data-ttu-id="9c5c5-103">**Только в EF6 и более поздних версиях**. Функции, API и другие возможности, описанные на этой странице, появились в Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="9c5c5-103">**EF6 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 6.</span></span> <span data-ttu-id="9c5c5-104">При использовании более ранней версии могут быть неприменимы некоторые или все сведения.</span><span class="sxs-lookup"><span data-stu-id="9c5c5-104">If you are using an earlier version, some or all of the information does not apply.</span></span>  

<span data-ttu-id="9c5c5-105">Начиная с EF6, Entity Framework содержит механизм общего назначения для получения реализаций служб, которые необходимы.</span><span class="sxs-lookup"><span data-stu-id="9c5c5-105">Starting with EF6, Entity Framework contains a general-purpose mechanism for obtaining implementations of services that it requires.</span></span> <span data-ttu-id="9c5c5-106">То есть при EF используется экземпляр некоторые интерфейсы или базовые классы вам будет предложено конкретную реализацию интерфейса или базового класса для использования.</span><span class="sxs-lookup"><span data-stu-id="9c5c5-106">That is, when EF uses an instance of some interfaces or base classes it will ask for a concrete implementation of the interface or base class to use.</span></span> <span data-ttu-id="9c5c5-107">Это достигается путем использования интерфейса IDbDependencyResolver:</span><span class="sxs-lookup"><span data-stu-id="9c5c5-107">This is achieved through use of the IDbDependencyResolver interface:</span></span>  

``` csharp
public interface IDbDependencyResolver
{
    object GetService(Type type, object key);
}
```  

<span data-ttu-id="9c5c5-108">Метод GetService обычно вызывается EF и обрабатывается реализацию IDbDependencyResolver, предоставленное платформой EF или приложением.</span><span class="sxs-lookup"><span data-stu-id="9c5c5-108">The GetService method is typically called by EF and is handled by an implementation of IDbDependencyResolver provided either by EF or by the application.</span></span> <span data-ttu-id="9c5c5-109">При вызове аргумент типа является типом интерфейса или базового класса запрашиваемой службы, а объект ключа равно null или объект, предоставляющий контекстную информацию о запрошенную службу.</span><span class="sxs-lookup"><span data-stu-id="9c5c5-109">When called, the type argument is the interface or base class type of the service being requested, and the key object is either null or an object providing contextual information about the requested service.</span></span>  

<span data-ttu-id="9c5c5-110">Если не указано иное, любой объект, возвращаемый должен быть поточно ориентированной, так как он может использоваться как единственный экземпляр.</span><span class="sxs-lookup"><span data-stu-id="9c5c5-110">Unless otherwise stated any object returned must be thread-safe since it can be used as a singleton.</span></span> <span data-ttu-id="9c5c5-111">Во многих случаях объекты, возвращаемые в этом случае является фабрикой самой фабрики должен быть поточно ориентированной, но не нужно быть потокобезопасными, поскольку у фабрики для каждого использования запрашивается новый экземпляр объекта, возвращаемого из фабрики.</span><span class="sxs-lookup"><span data-stu-id="9c5c5-111">In many cases the object returned is a factory in which case the factory itself must be thread-safe but the object returned from the factory does not need to be thread-safe since a new instance is requested from the factory for each use.</span></span>

<span data-ttu-id="9c5c5-112">Эта статья содержит подробные сведения о том, как реализовать IDbDependencyResolver, но вместо выступает в качестве ссылки для типов служб (то есть, интерфейсов и базовых типах классов), для которых EF вызывает GetService и семантика объекта ключа для каждого из них вызовы.</span><span class="sxs-lookup"><span data-stu-id="9c5c5-112">This article does not contain full details on how to implement IDbDependencyResolver, but instead acts as a reference for the service types (that is, the interface and base class types) for which EF calls GetService and the semantics of the key object for each of these calls.</span></span>

## <a name="systemdataentityidatabaseinitializertcontext"></a><span data-ttu-id="9c5c5-113">System.Data.Entity.IDatabaseInitializer < TContext\></span><span class="sxs-lookup"><span data-stu-id="9c5c5-113">System.Data.Entity.IDatabaseInitializer<TContext\></span></span>  

<span data-ttu-id="9c5c5-114">**Представленные версии**: EF6.0.0</span><span class="sxs-lookup"><span data-stu-id="9c5c5-114">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="9c5c5-115">**Объект, возвращаемый**: инициализатор базы данных для заданного контекста типа</span><span class="sxs-lookup"><span data-stu-id="9c5c5-115">**Object returned**: A database initializer for the given context type</span></span>  

<span data-ttu-id="9c5c5-116">**Ключ**: не использовать, будет иметь значение null</span><span class="sxs-lookup"><span data-stu-id="9c5c5-116">**Key**: Not used; will be null</span></span>  

## <a name="funcsystemdataentitymigrationssqlmigrationsqlgenerator"></a><span data-ttu-id="9c5c5-117">Func < System.Data.Entity.Migrations.Sql.MigrationSqlGenerator\></span><span class="sxs-lookup"><span data-stu-id="9c5c5-117">Func<System.Data.Entity.Migrations.Sql.MigrationSqlGenerator\></span></span>  

<span data-ttu-id="9c5c5-118">**Представленные версии**: EF6.0.0</span><span class="sxs-lookup"><span data-stu-id="9c5c5-118">**Version introduced**: EF6.0.0</span></span>

<span data-ttu-id="9c5c5-119">**Объект, возвращаемый**: фабрику для создания генератора SQL, который может использоваться для миграции и другие действия, вызывающие базы данных, создаваемых, таких как создание базы данных с инициализаторами базы данных.</span><span class="sxs-lookup"><span data-stu-id="9c5c5-119">**Object returned**: A factory to create a SQL generator that can be used for Migrations and other actions that cause a database to be created, such as database creation with database initializers.</span></span>  

<span data-ttu-id="9c5c5-120">**Ключ**: строка, содержащая неизменяемое имя поставщика ADO.NET, указав тип базы данных, для которого будет создаваться SQL.</span><span class="sxs-lookup"><span data-stu-id="9c5c5-120">**Key**: A string containing the ADO.NET provider invariant name specifying the type of database for which SQL will be generated.</span></span> <span data-ttu-id="9c5c5-121">Например генератор SQL Server SQL возвращается для ключа «System.Data.SqlClient»».</span><span class="sxs-lookup"><span data-stu-id="9c5c5-121">For example, the SQL Server SQL generator is returned for the key "System.Data.SqlClient".</span></span>  

>[!NOTE]
> <span data-ttu-id="9c5c5-122">См. Дополнительные сведения о службах, связанные с поставщиком, в EF6 [модель поставщика EF6](~/ef6/fundamentals/providers/provider-model.md) раздел.</span><span class="sxs-lookup"><span data-stu-id="9c5c5-122">For more details on provider-related services in EF6 see the [EF6 provider model](~/ef6/fundamentals/providers/provider-model.md) section.</span></span>  

## <a name="systemdataentitycorecommondbproviderservices"></a><span data-ttu-id="9c5c5-123">System.Data.Entity.Core.Common.DbProviderServices</span><span class="sxs-lookup"><span data-stu-id="9c5c5-123">System.Data.Entity.Core.Common.DbProviderServices</span></span>  

<span data-ttu-id="9c5c5-124">**Представленные версии**: EF6.0.0</span><span class="sxs-lookup"><span data-stu-id="9c5c5-124">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="9c5c5-125">**Объект, возвращаемый**: поставщик EF для использования данного неизменяемое имя поставщика</span><span class="sxs-lookup"><span data-stu-id="9c5c5-125">**Object returned**: The EF provider to use for a given provider invariant name</span></span>  

<span data-ttu-id="9c5c5-126">**Ключ**: строка, содержащая неизменяемое имя поставщика ADO.NET, указав тип базы данных, для которого требуется поставщик.</span><span class="sxs-lookup"><span data-stu-id="9c5c5-126">**Key**: A string containing the ADO.NET provider invariant name specifying the type of database for which a provider is needed.</span></span> <span data-ttu-id="9c5c5-127">Например поставщик SQL Server возвращается для ключа «System.Data.SqlClient»».</span><span class="sxs-lookup"><span data-stu-id="9c5c5-127">For example, the SQL Server provider is returned for the key "System.Data.SqlClient".</span></span>  

>[!NOTE]
> <span data-ttu-id="9c5c5-128">См. Дополнительные сведения о службах, связанные с поставщиком, в EF6 [модель поставщика EF6](~/ef6/fundamentals/providers/provider-model.md) раздел.</span><span class="sxs-lookup"><span data-stu-id="9c5c5-128">For more details on provider-related services in EF6 see the [EF6 provider model](~/ef6/fundamentals/providers/provider-model.md) section.</span></span>  

## <a name="systemdataentityinfrastructureidbconnectionfactory"></a><span data-ttu-id="9c5c5-129">System.Data.Entity.Infrastructure.IDbConnectionFactory</span><span class="sxs-lookup"><span data-stu-id="9c5c5-129">System.Data.Entity.Infrastructure.IDbConnectionFactory</span></span>  

<span data-ttu-id="9c5c5-130">**Представленные версии**: EF6.0.0</span><span class="sxs-lookup"><span data-stu-id="9c5c5-130">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="9c5c5-131">**Объект, возвращаемый**: фабрику соединений, который будет использоваться при EF создает подключение к базе данных по соглашению.</span><span class="sxs-lookup"><span data-stu-id="9c5c5-131">**Object returned**: The connection factory that will be used when EF creates a database connection by convention.</span></span> <span data-ttu-id="9c5c5-132">То есть при без подключения или строка подключения передается EF, а не строку подключения можно найти в файле app.config или web.config, эта служба затем используется для создания подключения, по соглашению.</span><span class="sxs-lookup"><span data-stu-id="9c5c5-132">That is, when no connection or connection string is given to EF, and no connection string can be found in the app.config or web.config, then this service is used to create a connection by convention.</span></span> <span data-ttu-id="9c5c5-133">Изменение Фабрика соединений можно разрешить EF использовать другой тип базы данных (например, SQL Server Compact Edition) по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="9c5c5-133">Changing the connection factory can allow EF to use a different type of database (for example, SQL Server Compact Edition) by default.</span></span>  

<span data-ttu-id="9c5c5-134">**Ключ**: не использовать, будет иметь значение null</span><span class="sxs-lookup"><span data-stu-id="9c5c5-134">**Key**: Not used; will be null</span></span>  

>[!NOTE]
> <span data-ttu-id="9c5c5-135">См. Дополнительные сведения о службах, связанные с поставщиком, в EF6 [модель поставщика EF6](~/ef6/fundamentals/providers/provider-model.md) раздел.</span><span class="sxs-lookup"><span data-stu-id="9c5c5-135">For more details on provider-related services in EF6 see the [EF6 provider model](~/ef6/fundamentals/providers/provider-model.md) section.</span></span>  

## <a name="systemdataentityinfrastructureimanifesttokenservice"></a><span data-ttu-id="9c5c5-136">System.Data.Entity.Infrastructure.IManifestTokenService</span><span class="sxs-lookup"><span data-stu-id="9c5c5-136">System.Data.Entity.Infrastructure.IManifestTokenService</span></span>  

<span data-ttu-id="9c5c5-137">**Представленные версии**: EF6.0.0</span><span class="sxs-lookup"><span data-stu-id="9c5c5-137">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="9c5c5-138">**Объект, возвращаемый**: служба, можно создать маркер манифеста поставщика из соединения.</span><span class="sxs-lookup"><span data-stu-id="9c5c5-138">**Object returned**: A service that can generate a provider manifest token from a connection.</span></span> <span data-ttu-id="9c5c5-139">Эта служба обычно используется двумя способами.</span><span class="sxs-lookup"><span data-stu-id="9c5c5-139">This service is typically used in two ways.</span></span> <span data-ttu-id="9c5c5-140">Во-первых он может использоваться для предотвращения Code First, соединения с базой данных при построении модели.</span><span class="sxs-lookup"><span data-stu-id="9c5c5-140">First, it can be used to avoid Code First connecting to the database when building a model.</span></span> <span data-ttu-id="9c5c5-141">Во-вторых его можно использовать для принудительного Code First для создания модели для версии конкретной базы данных — например, чтобы принудительно модели для SQL Server 2005, даже если иногда используется SQL Server 2008.</span><span class="sxs-lookup"><span data-stu-id="9c5c5-141">Second, it can be used to force Code First to build a model for a specific database version -- for example, to force a model for SQL Server 2005 even if sometimes SQL Server 2008 is used.</span></span>  

<span data-ttu-id="9c5c5-142">**Время жизни объекта**: одноэлементный--тот же объект может быть используется несколько раз и одновременно в разных потоков</span><span class="sxs-lookup"><span data-stu-id="9c5c5-142">**Object lifetime**: Singleton -- the same object may be used multiple times and concurrently by different threads</span></span>  

<span data-ttu-id="9c5c5-143">**Ключ**: не использовать, будет иметь значение null</span><span class="sxs-lookup"><span data-stu-id="9c5c5-143">**Key**: Not used; will be null</span></span>  

## <a name="systemdataentityinfrastructureidbproviderfactoryservice"></a><span data-ttu-id="9c5c5-144">System.Data.Entity.Infrastructure.IDbProviderFactoryService</span><span class="sxs-lookup"><span data-stu-id="9c5c5-144">System.Data.Entity.Infrastructure.IDbProviderFactoryService</span></span>  

<span data-ttu-id="9c5c5-145">**Представленные версии**: EF6.0.0</span><span class="sxs-lookup"><span data-stu-id="9c5c5-145">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="9c5c5-146">**Объект, возвращаемый**: служба, можно получить фабрику поставщика из данного соединения.</span><span class="sxs-lookup"><span data-stu-id="9c5c5-146">**Object returned**: A service that can obtain a provider factory from a given connection.</span></span> <span data-ttu-id="9c5c5-147">В .NET 4.5 поставщик является общедоступным из соединения.</span><span class="sxs-lookup"><span data-stu-id="9c5c5-147">On .NET 4.5 the provider is publicly accessible from the connection.</span></span> <span data-ttu-id="9c5c5-148">В .NET 4 Реализация по умолчанию эта служба использует часть эвристических правил, чтобы найти соответствующий поставщик.</span><span class="sxs-lookup"><span data-stu-id="9c5c5-148">On .NET 4 the default implementation of this service uses some heuristics to find the matching provider.</span></span> <span data-ttu-id="9c5c5-149">Если они не затем новую реализацию этой службы можно зарегистрировать для обеспечения соответствующего разрешения.</span><span class="sxs-lookup"><span data-stu-id="9c5c5-149">If these fail then a new implementation of this service can be registered to provide an appropriate resolution.</span></span>  

<span data-ttu-id="9c5c5-150">**Ключ**: не использовать, будет иметь значение null</span><span class="sxs-lookup"><span data-stu-id="9c5c5-150">**Key**: Not used; will be null</span></span>  

## <a name="funcdbcontext-systemdataentityinfrastructureidbmodelcachekey"></a><span data-ttu-id="9c5c5-151">Func < DbContext, System.Data.Entity.Infrastructure.IDbModelCacheKey\></span><span class="sxs-lookup"><span data-stu-id="9c5c5-151">Func<DbContext, System.Data.Entity.Infrastructure.IDbModelCacheKey\></span></span>  

<span data-ttu-id="9c5c5-152">**Представленные версии**: EF6.0.0</span><span class="sxs-lookup"><span data-stu-id="9c5c5-152">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="9c5c5-153">**Объект, возвращаемый**: фабрику, которая создаст ключ кэша модели в данном контексте.</span><span class="sxs-lookup"><span data-stu-id="9c5c5-153">**Object returned**: A factory that will generate a model cache key for a given context.</span></span> <span data-ttu-id="9c5c5-154">По умолчанию EF кэширует одной модели каждого типа DbContext поставщика.</span><span class="sxs-lookup"><span data-stu-id="9c5c5-154">By default, EF caches one model per DbContext type per provider.</span></span> <span data-ttu-id="9c5c5-155">Чтобы добавить другие сведения, такие как имя схемы, ключ кэша можно использовать другую реализацию этой службы.</span><span class="sxs-lookup"><span data-stu-id="9c5c5-155">A different implementation of this service can be used to add other information, such as schema name, to the cache key.</span></span>  

<span data-ttu-id="9c5c5-156">**Ключ**: не использовать, будет иметь значение null</span><span class="sxs-lookup"><span data-stu-id="9c5c5-156">**Key**: Not used; will be null</span></span>  

## <a name="systemdataentityspatialdbspatialservices"></a><span data-ttu-id="9c5c5-157">System.Data.Entity.Spatial.DbSpatialServices</span><span class="sxs-lookup"><span data-stu-id="9c5c5-157">System.Data.Entity.Spatial.DbSpatialServices</span></span>  

<span data-ttu-id="9c5c5-158">**Представленные версии**: EF6.0.0</span><span class="sxs-lookup"><span data-stu-id="9c5c5-158">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="9c5c5-159">**Объект, возвращаемый**: EF пространственных поставщика, который добавляет в базовый поставщик EF для geography и geometry пространственных типов.</span><span class="sxs-lookup"><span data-stu-id="9c5c5-159">**Object returned**: An EF spatial provider that adds support to the basic EF provider for geography and geometry spatial types.</span></span>  

<span data-ttu-id="9c5c5-160">**Ключ**: DbSptialServices запрашивается двумя способами.</span><span class="sxs-lookup"><span data-stu-id="9c5c5-160">**Key**: DbSptialServices is asked for in two ways.</span></span> <span data-ttu-id="9c5c5-161">Во-первых, поставщика пространственной службы запрашиваются с помощью объекта DbProviderInfo (который содержит инвариант имя и маркер манифеста) как ключ.</span><span class="sxs-lookup"><span data-stu-id="9c5c5-161">First, provider-specific spatial services are requested using a DbProviderInfo object (which contains invariant name and manifest token) as the key.</span></span> <span data-ttu-id="9c5c5-162">Во-вторых DbSpatialServices могут запрашиваться без ключа.</span><span class="sxs-lookup"><span data-stu-id="9c5c5-162">Second, DbSpatialServices can be asked for with no key.</span></span> <span data-ttu-id="9c5c5-163">Это используется для разрешения «глобальные пространственных поставщик» используемый при создании автономного DbGeography или DbGeometry типов.</span><span class="sxs-lookup"><span data-stu-id="9c5c5-163">This is used to resolve the "global spatial provider" which is used when creating stand-alone DbGeography or DbGeometry types.</span></span>  

>[!NOTE]
> <span data-ttu-id="9c5c5-164">См. Дополнительные сведения о службах, связанные с поставщиком, в EF6 [модель поставщика EF6](~/ef6/fundamentals/providers/provider-model.md) раздел.</span><span class="sxs-lookup"><span data-stu-id="9c5c5-164">For more details on provider-related services in EF6 see the [EF6 provider model](~/ef6/fundamentals/providers/provider-model.md) section.</span></span>  

## <a name="funcsystemdataentityinfrastructureidbexecutionstrategy"></a><span data-ttu-id="9c5c5-165">Func < System.Data.Entity.Infrastructure.IDbExecutionStrategy\></span><span class="sxs-lookup"><span data-stu-id="9c5c5-165">Func<System.Data.Entity.Infrastructure.IDbExecutionStrategy\></span></span>  

<span data-ttu-id="9c5c5-166">**Представленные версии**: EF6.0.0</span><span class="sxs-lookup"><span data-stu-id="9c5c5-166">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="9c5c5-167">**Объект, возвращаемый**: фабрику для создания службы, которая позволяет поставщику для реализации повторных попыток или другого поведения, при выполнении запросов и команд в базе данных.</span><span class="sxs-lookup"><span data-stu-id="9c5c5-167">**Object returned**: A factory to create a service that allows a provider to implement retries or other behavior when queries and commands are executed against the database.</span></span> <span data-ttu-id="9c5c5-168">Если реализации не указаны, затем EF будет просто выполните команды и распространить все исключения.</span><span class="sxs-lookup"><span data-stu-id="9c5c5-168">If no implementation is provided, then EF will simply execute the commands and propagate any exceptions thrown.</span></span> <span data-ttu-id="9c5c5-169">Для SQL Server эта служба используется для предоставления политику повтора, что особенно полезно при запуске для серверов базы данных на основе облака, таких как SQL Azure.</span><span class="sxs-lookup"><span data-stu-id="9c5c5-169">For SQL Server this service is used to provide a retry policy which is especially useful when running against cloud-based database servers such as SQL Azure.</span></span>  

<span data-ttu-id="9c5c5-170">**Ключ**: ExecutionStrategyKey объект, содержащий неизменяемое имя поставщика и, при необходимости, имя сервера, для которого будет использоваться стратегии выполнения.</span><span class="sxs-lookup"><span data-stu-id="9c5c5-170">**Key**: An ExecutionStrategyKey object that contains the provider invariant name and optionally a server name for which the execution strategy will be used.</span></span>  

>[!NOTE]
> <span data-ttu-id="9c5c5-171">См. Дополнительные сведения о службах, связанные с поставщиком, в EF6 [модель поставщика EF6](~/ef6/fundamentals/providers/provider-model.md) раздел.</span><span class="sxs-lookup"><span data-stu-id="9c5c5-171">For more details on provider-related services in EF6 see the [EF6 provider model](~/ef6/fundamentals/providers/provider-model.md) section.</span></span>  

## <a name="funcdbconnection-string-systemdataentitymigrationshistoryhistorycontext"></a><span data-ttu-id="9c5c5-172">Func < DbConnection, строка, System.Data.Entity.Migrations.History.HistoryContext\></span><span class="sxs-lookup"><span data-stu-id="9c5c5-172">Func<DbConnection, string, System.Data.Entity.Migrations.History.HistoryContext\></span></span>  

<span data-ttu-id="9c5c5-173">**Представленные версии**: EF6.0.0</span><span class="sxs-lookup"><span data-stu-id="9c5c5-173">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="9c5c5-174">**Объект, возвращаемый**: фабрику, которая позволяет поставщику настроить сопоставление HistoryContext для `__MigrationHistory` таблицу, используемую миграции EF.</span><span class="sxs-lookup"><span data-stu-id="9c5c5-174">**Object returned**: A factory that allows a provider to configure the mapping of the HistoryContext to the `__MigrationHistory` table used by EF Migrations.</span></span> <span data-ttu-id="9c5c5-175">HistoryContext является первый DbContext кода и могут быть настроены в обычном fluent API для изменения действия, такие как имя таблицы и спецификации сопоставления столбцов.</span><span class="sxs-lookup"><span data-stu-id="9c5c5-175">The HistoryContext is a Code First DbContext and can be configured using the normal fluent API to change things like the name of the table and the column mapping specifications.</span></span>  

<span data-ttu-id="9c5c5-176">**Ключ**: не использовать, будет иметь значение null</span><span class="sxs-lookup"><span data-stu-id="9c5c5-176">**Key**: Not used; will be null</span></span>  

>[!NOTE]
> <span data-ttu-id="9c5c5-177">См. Дополнительные сведения о службах, связанные с поставщиком, в EF6 [модель поставщика EF6](~/ef6/fundamentals/providers/provider-model.md) раздел.</span><span class="sxs-lookup"><span data-stu-id="9c5c5-177">For more details on provider-related services in EF6 see the [EF6 provider model](~/ef6/fundamentals/providers/provider-model.md) section.</span></span>  

## <a name="systemdatacommondbproviderfactory"></a><span data-ttu-id="9c5c5-178">System.Data.Common.DbProviderFactory</span><span class="sxs-lookup"><span data-stu-id="9c5c5-178">System.Data.Common.DbProviderFactory</span></span>  

<span data-ttu-id="9c5c5-179">**Представленные версии**: EF6.0.0</span><span class="sxs-lookup"><span data-stu-id="9c5c5-179">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="9c5c5-180">**Объект, возвращаемый**: поставщик ADO.NET для использования данного неизменяемое имя поставщика.</span><span class="sxs-lookup"><span data-stu-id="9c5c5-180">**Object returned**: The ADO.NET provider to use for a given provider invariant name.</span></span>  

<span data-ttu-id="9c5c5-181">**Ключ**: строка, содержащая неизменяемое имя поставщика ADO.NET</span><span class="sxs-lookup"><span data-stu-id="9c5c5-181">**Key**: A string containing the ADO.NET provider invariant name</span></span>  

>[!NOTE]
> <span data-ttu-id="9c5c5-182">Эта служба не изменяется обычно непосредственно, так как в реализации по умолчанию используется обычный Регистрация поставщика ADO.NET.</span><span class="sxs-lookup"><span data-stu-id="9c5c5-182">This service is not usually changed directly since the default implementation uses the normal ADO.NET provider registration.</span></span> <span data-ttu-id="9c5c5-183">См. Дополнительные сведения о службах, связанные с поставщиком, в EF6 [модель поставщика EF6](~/ef6/fundamentals/providers/provider-model.md) раздел.</span><span class="sxs-lookup"><span data-stu-id="9c5c5-183">For more details on provider-related services in EF6 see the [EF6 provider model](~/ef6/fundamentals/providers/provider-model.md) section.</span></span>  

## <a name="systemdataentityinfrastructureiproviderinvariantname"></a><span data-ttu-id="9c5c5-184">System.Data.Entity.Infrastructure.IProviderInvariantName</span><span class="sxs-lookup"><span data-stu-id="9c5c5-184">System.Data.Entity.Infrastructure.IProviderInvariantName</span></span>  

<span data-ttu-id="9c5c5-185">**Представленные версии**: EF6.0.0</span><span class="sxs-lookup"><span data-stu-id="9c5c5-185">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="9c5c5-186">**Объект, возвращаемый**: это служба, которая используется для определения неизменяемое имя поставщика для данного типа DbProviderFactory.</span><span class="sxs-lookup"><span data-stu-id="9c5c5-186">**Object returned**: a service that is used to determine a provider invariant name for a given type of DbProviderFactory.</span></span> <span data-ttu-id="9c5c5-187">Реализация по умолчанию эта служба использует Регистрация поставщика ADO.NET.</span><span class="sxs-lookup"><span data-stu-id="9c5c5-187">The default implementation of this service uses the ADO.NET provider registration.</span></span> <span data-ttu-id="9c5c5-188">Это означает, что если поставщик ADO.NET не зарегистрирован обычным способом, так как объект DbProviderFactory разрешается путем EF, затем он также будет необходимо для устранения этой службы.</span><span class="sxs-lookup"><span data-stu-id="9c5c5-188">This means that if the ADO.NET provider is not registered in the normal way because DbProviderFactory is being resolved by EF, then it will also be necessary to resolve this service.</span></span>  

<span data-ttu-id="9c5c5-189">**Ключ**: The DbProviderFactory экземпляра, для которого необходим является инвариантным именем.</span><span class="sxs-lookup"><span data-stu-id="9c5c5-189">**Key**: The DbProviderFactory instance for which an invariant name is required.</span></span>  

>[!NOTE]
> <span data-ttu-id="9c5c5-190">См. Дополнительные сведения о службах, связанные с поставщиком, в EF6 [модель поставщика EF6](~/ef6/fundamentals/providers/provider-model.md) раздел.</span><span class="sxs-lookup"><span data-stu-id="9c5c5-190">For more details on provider-related services in EF6 see the [EF6 provider model](~/ef6/fundamentals/providers/provider-model.md) section.</span></span>  

## <a name="systemdataentitycoremappingviewgenerationiviewassemblycache"></a><span data-ttu-id="9c5c5-191">System.Data.Entity.Core.Mapping.ViewGeneration.IViewAssemblyCache</span><span class="sxs-lookup"><span data-stu-id="9c5c5-191">System.Data.Entity.Core.Mapping.ViewGeneration.IViewAssemblyCache</span></span>  

<span data-ttu-id="9c5c5-192">**Представленные версии**: EF6.0.0</span><span class="sxs-lookup"><span data-stu-id="9c5c5-192">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="9c5c5-193">**Объект, возвращаемый**: кэш сборок, содержащих заранее сформированные представления.</span><span class="sxs-lookup"><span data-stu-id="9c5c5-193">**Object returned**: a cache of the assemblies that contain pre-generated views.</span></span> <span data-ttu-id="9c5c5-194">Замена обычно используется позволяет узнать, какие сборки содержит заранее сформированные представления без выполнения любой обнаружения EF.</span><span class="sxs-lookup"><span data-stu-id="9c5c5-194">A replacement is typically used to let EF know which assemblies contain pre-generated views without doing any discovery.</span></span>  

<span data-ttu-id="9c5c5-195">**Ключ**: не использовать, будет иметь значение null</span><span class="sxs-lookup"><span data-stu-id="9c5c5-195">**Key**: Not used; will be null</span></span>  

## <a name="systemdataentityinfrastructurepluralizationipluralizationservice"></a><span data-ttu-id="9c5c5-196">System.Data.Entity.Infrastructure.Pluralization.IPluralizationService</span><span class="sxs-lookup"><span data-stu-id="9c5c5-196">System.Data.Entity.Infrastructure.Pluralization.IPluralizationService</span></span>

<span data-ttu-id="9c5c5-197">**Представленные версии**: EF6.0.0</span><span class="sxs-lookup"><span data-stu-id="9c5c5-197">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="9c5c5-198">**Объект, возвращаемый**: служба, используемая EF pluralize и множественном числе имена.</span><span class="sxs-lookup"><span data-stu-id="9c5c5-198">**Object returned**: a service used by EF to pluralize and singularize names.</span></span> <span data-ttu-id="9c5c5-199">По умолчанию используется в службе преобразования во множественную форму на английском языке.</span><span class="sxs-lookup"><span data-stu-id="9c5c5-199">By default an English pluralization service is used.</span></span>  

<span data-ttu-id="9c5c5-200">**Ключ**: не использовать, будет иметь значение null</span><span class="sxs-lookup"><span data-stu-id="9c5c5-200">**Key**: Not used; will be null</span></span>  

## <a name="systemdataentityinfrastructureinterceptionidbinterceptor"></a><span data-ttu-id="9c5c5-201">System.Data.Entity.Infrastructure.Interception.IDbInterceptor</span><span class="sxs-lookup"><span data-stu-id="9c5c5-201">System.Data.Entity.Infrastructure.Interception.IDbInterceptor</span></span>  

<span data-ttu-id="9c5c5-202">**Представленные версии**: EF6.0.0</span><span class="sxs-lookup"><span data-stu-id="9c5c5-202">**Version introduced**: EF6.0.0</span></span>

<span data-ttu-id="9c5c5-203">**Объекты, возвращаемые**: любой перехватчики, которые должны быть зарегистрированы при запуске приложения.</span><span class="sxs-lookup"><span data-stu-id="9c5c5-203">**Objects returned**: Any interceptors that should be registered when the application starts.</span></span> <span data-ttu-id="9c5c5-204">Обратите внимание, что эти объекты запрошены с помощью вызова GetServices и все перехватчики, возвращаемый любой Сопоставитель зависимостей будет зарегистрирована.</span><span class="sxs-lookup"><span data-stu-id="9c5c5-204">Note that these objects are requested using the GetServices call and all interceptors returned by any dependency resolver will registered.</span></span>

<span data-ttu-id="9c5c5-205">**Ключ**: не использовать, будет иметь значение null.</span><span class="sxs-lookup"><span data-stu-id="9c5c5-205">**Key**: Not used; will be null.</span></span>  

## <a name="funcsystemdataentitydbcontext-actionstring-systemdataentityinfrastructureinterceptiondatabaselogformatter"></a><span data-ttu-id="9c5c5-206">Func < System.Data.Entity.DbContext, действие < строка\>, System.Data.Entity.Infrastructure.Interception.DatabaseLogFormatter\></span><span class="sxs-lookup"><span data-stu-id="9c5c5-206">Func<System.Data.Entity.DbContext, Action<string\>, System.Data.Entity.Infrastructure.Interception.DatabaseLogFormatter\></span></span>  

<span data-ttu-id="9c5c5-207">**Представленные версии**: EF6.0.0</span><span class="sxs-lookup"><span data-stu-id="9c5c5-207">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="9c5c5-208">**Объект, возвращаемый**: фабрику, которая будет использоваться для создания модуля форматирования журнала базы данных, который будет использовать, когда контекст. Свойство Database.Log устанавливается в заданном контексте.</span><span class="sxs-lookup"><span data-stu-id="9c5c5-208">**Object returned**: A factory that will be used to create the database log formatter that will be used when the context.Database.Log property is set on the given context.</span></span>  

<span data-ttu-id="9c5c5-209">**Ключ**: не использовать, будет иметь значение null.</span><span class="sxs-lookup"><span data-stu-id="9c5c5-209">**Key**: Not used; will be null.</span></span>  

## <a name="funcsystemdataentitydbcontext"></a><span data-ttu-id="9c5c5-210">Func < System.Data.Entity.DbContext\></span><span class="sxs-lookup"><span data-stu-id="9c5c5-210">Func<System.Data.Entity.DbContext\></span></span>  

<span data-ttu-id="9c5c5-211">**Представленные версии**: EF6.1.0</span><span class="sxs-lookup"><span data-stu-id="9c5c5-211">**Version introduced**: EF6.1.0</span></span>  

<span data-ttu-id="9c5c5-212">**Объект, возвращаемый**: фабрику, которая будет использоваться для создания экземпляров контекста для миграций, если контекст не имеет доступный конструктор без параметров.</span><span class="sxs-lookup"><span data-stu-id="9c5c5-212">**Object returned**: A factory that will be used to create context instances for Migrations when the context does not have an accessible parameterless constructor.</span></span>  

<span data-ttu-id="9c5c5-213">**Ключ**: объект типа для типа производном DbContext, для которого требуется фабрику.</span><span class="sxs-lookup"><span data-stu-id="9c5c5-213">**Key**: The Type object for the type of the derived DbContext for which a factory is needed.</span></span>  

## <a name="funcsystemdataentitycoremetadataedmimetadataannotationserializer"></a><span data-ttu-id="9c5c5-214">Func < System.Data.Entity.Core.Metadata.Edm.IMetadataAnnotationSerializer\></span><span class="sxs-lookup"><span data-stu-id="9c5c5-214">Func<System.Data.Entity.Core.Metadata.Edm.IMetadataAnnotationSerializer\></span></span>  

<span data-ttu-id="9c5c5-215">**Представленные версии**: EF6.1.0</span><span class="sxs-lookup"><span data-stu-id="9c5c5-215">**Version introduced**: EF6.1.0</span></span>  

<span data-ttu-id="9c5c5-216">**Объект, возвращаемый**: фабрику, которая будет использоваться для создания сериализаторы для сериализации со строгой типизацией пользовательских заметок, таким образом, сериализации и desterilized в XML для использования в Code First Migrations.</span><span class="sxs-lookup"><span data-stu-id="9c5c5-216">**Object returned**: A factory that will be used to create serializers for serialization of strongly-typed custom annotations such that they can be serialized and desterilized into XML for use in Code First Migrations.</span></span>  

<span data-ttu-id="9c5c5-217">**Ключ**: имя, для которого создается заметка сериализации или десериализации.</span><span class="sxs-lookup"><span data-stu-id="9c5c5-217">**Key**: The name of the annotation that is being serialized or deserialized.</span></span>  

## <a name="funcsystemdataentityinfrastructuretransactionhandler"></a><span data-ttu-id="9c5c5-218">Func < System.Data.Entity.Infrastructure.TransactionHandler\></span><span class="sxs-lookup"><span data-stu-id="9c5c5-218">Func<System.Data.Entity.Infrastructure.TransactionHandler\></span></span>  

<span data-ttu-id="9c5c5-219">**Представленные версии**: EF6.1.0</span><span class="sxs-lookup"><span data-stu-id="9c5c5-219">**Version introduced**: EF6.1.0</span></span>  

<span data-ttu-id="9c5c5-220">**Объект, возвращаемый**: фабрику, которая будет использоваться для создания обработчиков для транзакций, таким образом, чтобы специальная обработка может применяться для ситуаций, таких как обработка сбоев.</span><span class="sxs-lookup"><span data-stu-id="9c5c5-220">**Object returned**: A factory that will be used to create handlers for transactions so that special handling can be applied for situations such as handling commit failures.</span></span>  

<span data-ttu-id="9c5c5-221">**Ключ**: ExecutionStrategyKey объект, содержащий неизменяемое имя поставщика и, при необходимости, имя сервера, для которого будет использоваться обработчик транзакции.</span><span class="sxs-lookup"><span data-stu-id="9c5c5-221">**Key**: An ExecutionStrategyKey object that contains the provider invariant name and optionally a server name for which the transaction handler will be used.</span></span>  
