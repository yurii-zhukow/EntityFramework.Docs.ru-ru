---
title: Разрешение зависимостей - EF6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: 32d19ac6-9186-4ae1-8655-64ee49da55d0
caps.latest.revision: 3
ms.openlocfilehash: 88fd859b4f9a8069eeb08f32bb1d35ddcd21aec5
ms.sourcegitcommit: bdd06c9a591ba5e6d6a3ec046c80de98f598f3f3
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/10/2018
ms.locfileid: "39122787"
---
# <a name="dependency-resolution"></a><span data-ttu-id="421cd-102">Разрешение зависимостей</span><span class="sxs-lookup"><span data-stu-id="421cd-102">Dependency resolution</span></span>
> [!NOTE]
> <span data-ttu-id="421cd-103">**Только в EF6 и более поздних версиях**. Функции, API и другие возможности, описанные на этой странице, появились в Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="421cd-103">**EF6 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 6.</span></span> <span data-ttu-id="421cd-104">При использовании более ранней версии могут быть неприменимы некоторые или все сведения.</span><span class="sxs-lookup"><span data-stu-id="421cd-104">If you are using an earlier version, some or all of the information does not apply.</span></span>  

<span data-ttu-id="421cd-105">Начиная с EF6, Entity Framework содержит механизм общего назначения для получения реализаций служб, которые необходимы.</span><span class="sxs-lookup"><span data-stu-id="421cd-105">Starting with EF6, Entity Framework contains a general-purpose mechanism for obtaining implementations of services that it requires.</span></span> <span data-ttu-id="421cd-106">То есть при EF используется экземпляр некоторые интерфейсы или базовые классы вам будет предложено конкретную реализацию интерфейса или базового класса для использования.</span><span class="sxs-lookup"><span data-stu-id="421cd-106">That is, when EF uses an instance of some interfaces or base classes it will ask for a concrete implementation of the interface or base class to use.</span></span> <span data-ttu-id="421cd-107">Это достигается путем использования интерфейса IDbDependencyResolver:</span><span class="sxs-lookup"><span data-stu-id="421cd-107">This is achieved through use of the IDbDependencyResolver interface:</span></span>  

``` csharp
public interface IDbDependencyResolver
{
    object GetService(Type type, object key);
}
```  

<span data-ttu-id="421cd-108">Метод GetService обычно вызывается EF и обрабатывается реализацию IDbDependencyResolver, предоставленное платформой EF или приложением.</span><span class="sxs-lookup"><span data-stu-id="421cd-108">The GetService method is typically called by EF and is handled by an implementation of IDbDependencyResolver provided either by EF or by the application.</span></span> <span data-ttu-id="421cd-109">При вызове аргумент типа является типом интерфейса или базового класса запрашиваемой службы, а объект ключа равно null или объект, предоставляющий контекстную информацию о запрошенную службу.</span><span class="sxs-lookup"><span data-stu-id="421cd-109">When called, the type argument is the interface or base class type of the service being requested, and the key object is either null or an object providing contextual information about the requested service.</span></span>  

<span data-ttu-id="421cd-110">Эта статья содержит подробные сведения о том, как реализовать IDbDependencyResolver, но вместо выступает в качестве ссылки для типов служб (то есть, интерфейсов и базовых типах классов), для которых EF вызывает GetService и семантика объекта ключа для каждого из них вызовы.</span><span class="sxs-lookup"><span data-stu-id="421cd-110">This article does not contain full details on how to implement IDbDependencyResolver, but instead acts as a reference for the service types (that is, the interface and base class types) for which EF calls GetService and the semantics of the key object for each of these calls.</span></span> <span data-ttu-id="421cd-111">В этом документе будет постоянно обновляются, по мере добавления дополнительных служб.</span><span class="sxs-lookup"><span data-stu-id="421cd-111">This document will be kept up-to-date as additional services are added.</span></span>  

## <a name="services-resolved"></a><span data-ttu-id="421cd-112">Разрешения службы</span><span class="sxs-lookup"><span data-stu-id="421cd-112">Services resolved</span></span>  

<span data-ttu-id="421cd-113">Если не указано иное, любой объект, возвращаемый должен быть поточно ориентированной, так как он может использоваться как единственный экземпляр.</span><span class="sxs-lookup"><span data-stu-id="421cd-113">Unless otherwise stated any object returned must be thread-safe since it can be used as a singleton.</span></span> <span data-ttu-id="421cd-114">Во многих случаях объекты, возвращаемые в этом случае является фабрикой самой фабрики должен быть поточно ориентированной, но не нужно быть потокобезопасными, поскольку у фабрики для каждого использования запрашивается новый экземпляр объекта, возвращаемого из фабрики.</span><span class="sxs-lookup"><span data-stu-id="421cd-114">In many cases the object returned is a factory in which case the factory itself must be thread-safe but the object returned from the factory does not need to be thread-safe since a new instance is requested from the factory for each use.</span></span>  

### <a name="systemdataentityidatabaseinitializertcontext"></a><span data-ttu-id="421cd-115">System.Data.Entity.IDatabaseInitializer < TContext\></span><span class="sxs-lookup"><span data-stu-id="421cd-115">System.Data.Entity.IDatabaseInitializer<TContext\></span></span>  

<span data-ttu-id="421cd-116">**Представленные версии**: EF6.0.0</span><span class="sxs-lookup"><span data-stu-id="421cd-116">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="421cd-117">**Объект, возвращаемый**: инициализатор базы данных для заданного контекста типа</span><span class="sxs-lookup"><span data-stu-id="421cd-117">**Object returned**: A database initializer for the given context type</span></span>  

<span data-ttu-id="421cd-118">**Ключ**: не использовать, будет иметь значение null</span><span class="sxs-lookup"><span data-stu-id="421cd-118">**Key**: Not used; will be null</span></span>  

### <a name="funcsystemdataentitymigrationssqlmigrationsqlgenerator"></a><span data-ttu-id="421cd-119">Func < System.Data.Entity.Migrations.Sql.MigrationSqlGenerator\></span><span class="sxs-lookup"><span data-stu-id="421cd-119">Func<System.Data.Entity.Migrations.Sql.MigrationSqlGenerator\></span></span>  

<span data-ttu-id="421cd-120">**Представленные версии**: EF6.0.0</span><span class="sxs-lookup"><span data-stu-id="421cd-120">**Version introduced**: EF6.0.0</span></span>

<span data-ttu-id="421cd-121">**Объект, возвращаемый**: фабрику для создания генератора SQL, который может использоваться для миграции и другие действия, вызывающие базы данных, создаваемых, таких как создание базы данных с инициализаторами базы данных.</span><span class="sxs-lookup"><span data-stu-id="421cd-121">**Object returned**: A factory to create a SQL generator that can be used for Migrations and other actions that cause a database to be created, such as database creation with database initializers.</span></span>  

<span data-ttu-id="421cd-122">**Ключ**: строка, содержащая неизменяемое имя поставщика ADO.NET, указав тип базы данных, для которого будет создаваться SQL.</span><span class="sxs-lookup"><span data-stu-id="421cd-122">**Key**: A string containing the ADO.NET provider invariant name specifying the type of database for which SQL will be generated.</span></span> <span data-ttu-id="421cd-123">Например генератор SQL Server SQL возвращается для ключа «System.Data.SqlClient»».</span><span class="sxs-lookup"><span data-stu-id="421cd-123">For example, the SQL Server SQL generator is returned for the key "System.Data.SqlClient".</span></span>  

>[!NOTE]
> <span data-ttu-id="421cd-124">См. Дополнительные сведения о службах, связанные с поставщиком, в EF6 [модель поставщика EF6](~/ef6/fundamentals/providers/provider-model.md) раздел.</span><span class="sxs-lookup"><span data-stu-id="421cd-124">For more details on provider-related services in EF6 see the [EF6 provider model](~/ef6/fundamentals/providers/provider-model.md) section.</span></span>  

### <a name="systemdataentitycorecommondbproviderservices"></a><span data-ttu-id="421cd-125">System.Data.Entity.Core.Common.DbProviderServices</span><span class="sxs-lookup"><span data-stu-id="421cd-125">System.Data.Entity.Core.Common.DbProviderServices</span></span>  

<span data-ttu-id="421cd-126">**Представленные версии**: EF6.0.0</span><span class="sxs-lookup"><span data-stu-id="421cd-126">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="421cd-127">**Объект, возвращаемый**: поставщик EF для использования данного неизменяемое имя поставщика</span><span class="sxs-lookup"><span data-stu-id="421cd-127">**Object returned**: The EF provider to use for a given provider invariant name</span></span>  

<span data-ttu-id="421cd-128">**Ключ**: строка, содержащая неизменяемое имя поставщика ADO.NET, указав тип базы данных, для которого требуется поставщик.</span><span class="sxs-lookup"><span data-stu-id="421cd-128">**Key**: A string containing the ADO.NET provider invariant name specifying the type of database for which a provider is needed.</span></span> <span data-ttu-id="421cd-129">Например поставщик SQL Server возвращается для ключа «System.Data.SqlClient»».</span><span class="sxs-lookup"><span data-stu-id="421cd-129">For example, the SQL Server provider is returned for the key "System.Data.SqlClient".</span></span>  

>[!NOTE]
> <span data-ttu-id="421cd-130">См. Дополнительные сведения о службах, связанные с поставщиком, в EF6 [модель поставщика EF6](~/ef6/fundamentals/providers/provider-model.md) раздел.</span><span class="sxs-lookup"><span data-stu-id="421cd-130">For more details on provider-related services in EF6 see the [EF6 provider model](~/ef6/fundamentals/providers/provider-model.md) section.</span></span>  

### <a name="systemdataentityinfrastructureidbconnectionfactory"></a><span data-ttu-id="421cd-131">System.Data.Entity.Infrastructure.IDbConnectionFactory</span><span class="sxs-lookup"><span data-stu-id="421cd-131">System.Data.Entity.Infrastructure.IDbConnectionFactory</span></span>  

<span data-ttu-id="421cd-132">**Представленные версии**: EF6.0.0</span><span class="sxs-lookup"><span data-stu-id="421cd-132">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="421cd-133">**Объект, возвращаемый**: фабрику соединений, который будет использоваться при EF создает подключение к базе данных по соглашению.</span><span class="sxs-lookup"><span data-stu-id="421cd-133">**Object returned**: The connection factory that will be used when EF creates a database connection by convention.</span></span> <span data-ttu-id="421cd-134">То есть при без подключения или строка подключения передается EF, а не строку подключения можно найти в файле app.config или web.config, эта служба затем используется для создания подключения, по соглашению.</span><span class="sxs-lookup"><span data-stu-id="421cd-134">That is, when no connection or connection string is given to EF, and no connection string can be found in the app.config or web.config, then this service is used to create a connection by convention.</span></span> <span data-ttu-id="421cd-135">Изменение Фабрика соединений можно разрешить EF использовать другой тип базы данных (например, SQL Server Compact Edition) по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="421cd-135">Changing the connection factory can allow EF to use a different type of database (for example, SQL Server Compact Edition) by default.</span></span>  

<span data-ttu-id="421cd-136">**Ключ**: не использовать, будет иметь значение null</span><span class="sxs-lookup"><span data-stu-id="421cd-136">**Key**: Not used; will be null</span></span>  

>[!NOTE]
> <span data-ttu-id="421cd-137">См. Дополнительные сведения о службах, связанные с поставщиком, в EF6 [модель поставщика EF6](~/ef6/fundamentals/providers/provider-model.md) раздел.</span><span class="sxs-lookup"><span data-stu-id="421cd-137">For more details on provider-related services in EF6 see the [EF6 provider model](~/ef6/fundamentals/providers/provider-model.md) section.</span></span>  

### <a name="systemdataentityinfrastructureimanifesttokenservice"></a><span data-ttu-id="421cd-138">System.Data.Entity.Infrastructure.IManifestTokenService</span><span class="sxs-lookup"><span data-stu-id="421cd-138">System.Data.Entity.Infrastructure.IManifestTokenService</span></span>  

<span data-ttu-id="421cd-139">**Представленные версии**: EF6.0.0</span><span class="sxs-lookup"><span data-stu-id="421cd-139">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="421cd-140">**Объект, возвращаемый**: служба, можно создать маркер манифеста поставщика из соединения.</span><span class="sxs-lookup"><span data-stu-id="421cd-140">**Object returned**: A service that can generate a provider manifest token from a connection.</span></span> <span data-ttu-id="421cd-141">Эта служба обычно используется двумя способами.</span><span class="sxs-lookup"><span data-stu-id="421cd-141">This service is typically used in two ways.</span></span> <span data-ttu-id="421cd-142">Во-первых он может использоваться для предотвращения Code First, соединения с базой данных при построении модели.</span><span class="sxs-lookup"><span data-stu-id="421cd-142">First, it can be used to avoid Code First connecting to the database when building a model.</span></span> <span data-ttu-id="421cd-143">Во-вторых его можно использовать для принудительного Code First для создания модели для версии конкретной базы данных — например, чтобы принудительно модели для SQL Server 2005, даже если иногда используется SQL Server 2008.</span><span class="sxs-lookup"><span data-stu-id="421cd-143">Second, it can be used to force Code First to build a model for a specific database version -- for example, to force a model for SQL Server 2005 even if sometimes SQL Server 2008 is used.</span></span>  

<span data-ttu-id="421cd-144">**Время жизни объекта**: одноэлементный--тот же объект может быть используется несколько раз и одновременно в разных потоков</span><span class="sxs-lookup"><span data-stu-id="421cd-144">**Object lifetime**: Singleton -- the same object may be used multiple times and concurrently by different threads</span></span>  

<span data-ttu-id="421cd-145">**Ключ**: не использовать, будет иметь значение null</span><span class="sxs-lookup"><span data-stu-id="421cd-145">**Key**: Not used; will be null</span></span>  

### <a name="systemdataentityinfrastructureidbproviderfactoryservice"></a><span data-ttu-id="421cd-146">System.Data.Entity.Infrastructure.IDbProviderFactoryService</span><span class="sxs-lookup"><span data-stu-id="421cd-146">System.Data.Entity.Infrastructure.IDbProviderFactoryService</span></span>  

<span data-ttu-id="421cd-147">**Представленные версии**: EF6.0.0</span><span class="sxs-lookup"><span data-stu-id="421cd-147">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="421cd-148">**Объект, возвращаемый**: служба, можно получить фабрику поставщика из данного соединения.</span><span class="sxs-lookup"><span data-stu-id="421cd-148">**Object returned**: A service that can obtain a provider factory from a given connection.</span></span> <span data-ttu-id="421cd-149">В .NET 4.5 поставщик является общедоступным из соединения.</span><span class="sxs-lookup"><span data-stu-id="421cd-149">On .NET 4.5 the provider is publicly accessible from the connection.</span></span> <span data-ttu-id="421cd-150">В .NET 4 Реализация по умолчанию эта служба использует часть эвристических правил, чтобы найти соответствующий поставщик.</span><span class="sxs-lookup"><span data-stu-id="421cd-150">On .NET 4 the default implementation of this service uses some heuristics to find the matching provider.</span></span> <span data-ttu-id="421cd-151">Если они не затем новую реализацию этой службы можно зарегистрировать для обеспечения соответствующего разрешения.</span><span class="sxs-lookup"><span data-stu-id="421cd-151">If these fail then a new implementation of this service can be registered to provide an appropriate resolution.</span></span>  

<span data-ttu-id="421cd-152">**Ключ**: не использовать, будет иметь значение null</span><span class="sxs-lookup"><span data-stu-id="421cd-152">**Key**: Not used; will be null</span></span>  

### <a name="funcdbcontext-systemdataentityinfrastructureidbmodelcachekey"></a><span data-ttu-id="421cd-153">Func < DbContext, System.Data.Entity.Infrastructure.IDbModelCacheKey\></span><span class="sxs-lookup"><span data-stu-id="421cd-153">Func<DbContext, System.Data.Entity.Infrastructure.IDbModelCacheKey\></span></span>  

<span data-ttu-id="421cd-154">**Представленные версии**: EF6.0.0</span><span class="sxs-lookup"><span data-stu-id="421cd-154">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="421cd-155">**Объект, возвращаемый**: фабрику, которая создаст ключ кэша модели в данном контексте.</span><span class="sxs-lookup"><span data-stu-id="421cd-155">**Object returned**: A factory that will generate a model cache key for a given context.</span></span> <span data-ttu-id="421cd-156">По умолчанию EF кэширует одной модели каждого типа DbContext поставщика.</span><span class="sxs-lookup"><span data-stu-id="421cd-156">By default, EF caches one model per DbContext type per provider.</span></span> <span data-ttu-id="421cd-157">Чтобы добавить другие сведения, такие как имя схемы, ключ кэша можно использовать другую реализацию этой службы.</span><span class="sxs-lookup"><span data-stu-id="421cd-157">A different implementation of this service can be used to add other information, such as schema name, to the cache key.</span></span>  

<span data-ttu-id="421cd-158">**Ключ**: не использовать, будет иметь значение null</span><span class="sxs-lookup"><span data-stu-id="421cd-158">**Key**: Not used; will be null</span></span>  

### <a name="systemdataentityspatialdbspatialservices"></a><span data-ttu-id="421cd-159">System.Data.Entity.Spatial.DbSpatialServices</span><span class="sxs-lookup"><span data-stu-id="421cd-159">System.Data.Entity.Spatial.DbSpatialServices</span></span>  

<span data-ttu-id="421cd-160">**Представленные версии**: EF6.0.0</span><span class="sxs-lookup"><span data-stu-id="421cd-160">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="421cd-161">**Объект, возвращаемый**: EF пространственных поставщика, который добавляет в базовый поставщик EF для geography и geometry пространственных типов.</span><span class="sxs-lookup"><span data-stu-id="421cd-161">**Object returned**: An EF spatial provider that adds support to the basic EF provider for geography and geometry spatial types.</span></span>  

<span data-ttu-id="421cd-162">**Ключ**: DbSptialServices запрашивается двумя способами.</span><span class="sxs-lookup"><span data-stu-id="421cd-162">**Key**: DbSptialServices is asked for in two ways.</span></span> <span data-ttu-id="421cd-163">Во-первых, поставщика пространственной службы запрашиваются с помощью объекта DbProviderInfo (который содержит инвариант имя и маркер манифеста) как ключ.</span><span class="sxs-lookup"><span data-stu-id="421cd-163">First, provider-specific spatial services are requested using a DbProviderInfo object (which contains invariant name and manifest token) as the key.</span></span> <span data-ttu-id="421cd-164">Во-вторых DbSpatialServices могут запрашиваться без ключа.</span><span class="sxs-lookup"><span data-stu-id="421cd-164">Second, DbSpatialServices can be asked for with no key.</span></span> <span data-ttu-id="421cd-165">Это используется для разрешения «глобальные пространственных поставщик» используемый при создании автономного DbGeography или DbGeometry типов.</span><span class="sxs-lookup"><span data-stu-id="421cd-165">This is used to resolve the "global spatial provider" which is used when creating stand-alone DbGeography or DbGeometry types.</span></span>  

>[!NOTE]
> <span data-ttu-id="421cd-166">См. Дополнительные сведения о службах, связанные с поставщиком, в EF6 [модель поставщика EF6](~/ef6/fundamentals/providers/provider-model.md) раздел.</span><span class="sxs-lookup"><span data-stu-id="421cd-166">For more details on provider-related services in EF6 see the [EF6 provider model](~/ef6/fundamentals/providers/provider-model.md) section.</span></span>  

### <a name="funcsystemdataentityinfrastructureidbexecutionstrategy"></a><span data-ttu-id="421cd-167">Func < System.Data.Entity.Infrastructure.IDbExecutionStrategy\></span><span class="sxs-lookup"><span data-stu-id="421cd-167">Func<System.Data.Entity.Infrastructure.IDbExecutionStrategy\></span></span>  

<span data-ttu-id="421cd-168">**Представленные версии**: EF6.0.0</span><span class="sxs-lookup"><span data-stu-id="421cd-168">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="421cd-169">**Объект, возвращаемый**: фабрику для создания службы, которая позволяет поставщику для реализации повторных попыток или другого поведения, при выполнении запросов и команд в базе данных.</span><span class="sxs-lookup"><span data-stu-id="421cd-169">**Object returned**: A factory to create a service that allows a provider to implement retries or other behavior when queries and commands are executed against the database.</span></span> <span data-ttu-id="421cd-170">Если реализации не указаны, затем EF будет просто выполните команды и распространить все исключения.</span><span class="sxs-lookup"><span data-stu-id="421cd-170">If no implementation is provided, then EF will simply execute the commands and propagate any exceptions thrown.</span></span> <span data-ttu-id="421cd-171">Для SQL Server эта служба используется для предоставления политику повтора, что особенно полезно при запуске для серверов базы данных на основе облака, таких как SQL Azure.</span><span class="sxs-lookup"><span data-stu-id="421cd-171">For SQL Server this service is used to provide a retry policy which is especially useful when running against cloud-based database servers such as SQL Azure.</span></span>  

<span data-ttu-id="421cd-172">**Ключ**: ExecutionStrategyKey объект, содержащий неизменяемое имя поставщика и, при необходимости, имя сервера, для которого будет использоваться стратегии выполнения.</span><span class="sxs-lookup"><span data-stu-id="421cd-172">**Key**: An ExecutionStrategyKey object that contains the provider invariant name and optionally a server name for which the execution strategy will be used.</span></span>  

>[!NOTE]
> <span data-ttu-id="421cd-173">См. Дополнительные сведения о службах, связанные с поставщиком, в EF6 [модель поставщика EF6](~/ef6/fundamentals/providers/provider-model.md) раздел.</span><span class="sxs-lookup"><span data-stu-id="421cd-173">For more details on provider-related services in EF6 see the [EF6 provider model](~/ef6/fundamentals/providers/provider-model.md) section.</span></span>  

### <a name="funcdbconnection-string-systemdataentitymigrationshistoryhistorycontext"></a><span data-ttu-id="421cd-174">Func < DbConnection, строка, System.Data.Entity.Migrations.History.HistoryContext\></span><span class="sxs-lookup"><span data-stu-id="421cd-174">Func<DbConnection, string, System.Data.Entity.Migrations.History.HistoryContext\></span></span>  

<span data-ttu-id="421cd-175">**Представленные версии**: EF6.0.0</span><span class="sxs-lookup"><span data-stu-id="421cd-175">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="421cd-176">**Объект, возвращаемый**: фабрику, которая позволяет поставщику настроить сопоставление HistoryContext для `__MigrationHistory` таблицу, используемую миграции EF.</span><span class="sxs-lookup"><span data-stu-id="421cd-176">**Object returned**: A factory that allows a provider to configure the mapping of the HistoryContext to the `__MigrationHistory` table used by EF Migrations.</span></span> <span data-ttu-id="421cd-177">HistoryContext является первый DbContext кода и могут быть настроены в обычном fluent API для изменения действия, такие как имя таблицы и спецификации сопоставления столбцов.</span><span class="sxs-lookup"><span data-stu-id="421cd-177">The HistoryContext is a Code First DbContext and can be configured using the normal fluent API to change things like the name of the table and the column mapping specifications.</span></span>  

<span data-ttu-id="421cd-178">**Ключ**: не использовать, будет иметь значение null</span><span class="sxs-lookup"><span data-stu-id="421cd-178">**Key**: Not used; will be null</span></span>  

>[!NOTE]
> <span data-ttu-id="421cd-179">См. Дополнительные сведения о службах, связанные с поставщиком, в EF6 [модель поставщика EF6](~/ef6/fundamentals/providers/provider-model.md) раздел.</span><span class="sxs-lookup"><span data-stu-id="421cd-179">For more details on provider-related services in EF6 see the [EF6 provider model](~/ef6/fundamentals/providers/provider-model.md) section.</span></span>  

### <a name="systemdatacommondbproviderfactory"></a><span data-ttu-id="421cd-180">System.Data.Common.DbProviderFactory</span><span class="sxs-lookup"><span data-stu-id="421cd-180">System.Data.Common.DbProviderFactory</span></span>  

<span data-ttu-id="421cd-181">**Представленные версии**: EF6.0.0</span><span class="sxs-lookup"><span data-stu-id="421cd-181">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="421cd-182">**Объект, возвращаемый**: поставщик ADO.NET для использования данного неизменяемое имя поставщика.</span><span class="sxs-lookup"><span data-stu-id="421cd-182">**Object returned**: The ADO.NET provider to use for a given provider invariant name.</span></span>  

<span data-ttu-id="421cd-183">**Ключ**: строка, содержащая неизменяемое имя поставщика ADO.NET</span><span class="sxs-lookup"><span data-stu-id="421cd-183">**Key**: A string containing the ADO.NET provider invariant name</span></span>  

>[!NOTE]
> <span data-ttu-id="421cd-184">Эта служба не изменяется обычно непосредственно, так как в реализации по умолчанию используется обычный Регистрация поставщика ADO.NET.</span><span class="sxs-lookup"><span data-stu-id="421cd-184">This service is not usually changed directly since the default implementation uses the normal ADO.NET provider registration.</span></span> <span data-ttu-id="421cd-185">См. Дополнительные сведения о службах, связанные с поставщиком, в EF6 [модель поставщика EF6](~/ef6/fundamentals/providers/provider-model.md) раздел.</span><span class="sxs-lookup"><span data-stu-id="421cd-185">For more details on provider-related services in EF6 see the [EF6 provider model](~/ef6/fundamentals/providers/provider-model.md) section.</span></span>  

### <a name="systemdataentityinfrastructureiproviderinvariantname"></a><span data-ttu-id="421cd-186">System.Data.Entity.Infrastructure.IProviderInvariantName</span><span class="sxs-lookup"><span data-stu-id="421cd-186">System.Data.Entity.Infrastructure.IProviderInvariantName</span></span>  

<span data-ttu-id="421cd-187">**Представленные версии**: EF6.0.0</span><span class="sxs-lookup"><span data-stu-id="421cd-187">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="421cd-188">**Объект, возвращаемый**: это служба, которая используется для определения неизменяемое имя поставщика для данного типа DbProviderFactory.</span><span class="sxs-lookup"><span data-stu-id="421cd-188">**Object returned**: a service that is used to determine a provider invariant name for a given type of DbProviderFactory.</span></span> <span data-ttu-id="421cd-189">Реализация по умолчанию эта служба использует Регистрация поставщика ADO.NET.</span><span class="sxs-lookup"><span data-stu-id="421cd-189">The default implementation of this service uses the ADO.NET provider registration.</span></span> <span data-ttu-id="421cd-190">Это означает, что если поставщик ADO.NET не зарегистрирован обычным способом, так как объект DbProviderFactory разрешается путем EF, затем он также будет необходимо для устранения этой службы.</span><span class="sxs-lookup"><span data-stu-id="421cd-190">This means that if the ADO.NET provider is not registered in the normal way because DbProviderFactory is being resolved by EF, then it will also be necessary to resolve this service.</span></span>  

<span data-ttu-id="421cd-191">**Ключ**: The DbProviderFactory экземпляра, для которого необходим является инвариантным именем.</span><span class="sxs-lookup"><span data-stu-id="421cd-191">**Key**: The DbProviderFactory instance for which an invariant name is required.</span></span>  

>[!NOTE]
> <span data-ttu-id="421cd-192">См. Дополнительные сведения о службах, связанные с поставщиком, в EF6 [модель поставщика EF6](~/ef6/fundamentals/providers/provider-model.md) раздел.</span><span class="sxs-lookup"><span data-stu-id="421cd-192">For more details on provider-related services in EF6 see the [EF6 provider model](~/ef6/fundamentals/providers/provider-model.md) section.</span></span>  

### <a name="systemdataentitycoremappingviewgenerationiviewassemblycache"></a><span data-ttu-id="421cd-193">System.Data.Entity.Core.Mapping.ViewGeneration.IViewAssemblyCache</span><span class="sxs-lookup"><span data-stu-id="421cd-193">System.Data.Entity.Core.Mapping.ViewGeneration.IViewAssemblyCache</span></span>  

<span data-ttu-id="421cd-194">**Представленные версии**: EF6.0.0</span><span class="sxs-lookup"><span data-stu-id="421cd-194">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="421cd-195">**Объект, возвращаемый**: кэш сборок, содержащих заранее сформированные представления.</span><span class="sxs-lookup"><span data-stu-id="421cd-195">**Object returned**: a cache of the assemblies that contain pre-generated views.</span></span> <span data-ttu-id="421cd-196">Замена обычно используется позволяет узнать, какие сборки содержит заранее сформированные представления без выполнения любой обнаружения EF.</span><span class="sxs-lookup"><span data-stu-id="421cd-196">A replacement is typically used to let EF know which assemblies contain pre-generated views without doing any discovery.</span></span>  

<span data-ttu-id="421cd-197">**Ключ**: не использовать, будет иметь значение null</span><span class="sxs-lookup"><span data-stu-id="421cd-197">**Key**: Not used; will be null</span></span>  

### <a name="systemdataentityinfrastructurepluralizationipluralizationservice"></a><span data-ttu-id="421cd-198">System.Data.Entity.Infrastructure.Pluralization.IPluralizationService</span><span class="sxs-lookup"><span data-stu-id="421cd-198">System.Data.Entity.Infrastructure.Pluralization.IPluralizationService</span></span>

<span data-ttu-id="421cd-199">**Представленные версии**: EF6.0.0</span><span class="sxs-lookup"><span data-stu-id="421cd-199">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="421cd-200">**Объект, возвращаемый**: служба, используемая EF pluralize и множественном числе имена.</span><span class="sxs-lookup"><span data-stu-id="421cd-200">**Object returned**: a service used by EF to pluralize and singularize names.</span></span> <span data-ttu-id="421cd-201">По умолчанию используется в службе преобразования во множественную форму на английском языке.</span><span class="sxs-lookup"><span data-stu-id="421cd-201">By default an English pluralization service is used.</span></span>  

<span data-ttu-id="421cd-202">**Ключ**: не использовать, будет иметь значение null</span><span class="sxs-lookup"><span data-stu-id="421cd-202">**Key**: Not used; will be null</span></span>  

### <a name="systemdataentityinfrastructureinterceptionidbinterceptor"></a><span data-ttu-id="421cd-203">System.Data.Entity.Infrastructure.Interception.IDbInterceptor</span><span class="sxs-lookup"><span data-stu-id="421cd-203">System.Data.Entity.Infrastructure.Interception.IDbInterceptor</span></span>  

<span data-ttu-id="421cd-204">**Представленные версии**: EF6.0.0</span><span class="sxs-lookup"><span data-stu-id="421cd-204">**Version introduced**: EF6.0.0</span></span>

<span data-ttu-id="421cd-205">**Объекты, возвращаемые**: любой перехватчики, которые должны быть зарегистрированы при запуске приложения.</span><span class="sxs-lookup"><span data-stu-id="421cd-205">**Objects returned**: Any interceptors that should be registered when the application starts.</span></span> <span data-ttu-id="421cd-206">Обратите внимание, что эти объекты запрошены с помощью вызова GetServices и все перехватчики, возвращаемый любой Сопоставитель зависимостей будет зарегистрирована.</span><span class="sxs-lookup"><span data-stu-id="421cd-206">Note that these objects are requested using the GetServices call and all interceptors returned by any dependency resolver will registered.</span></span>

<span data-ttu-id="421cd-207">**Ключ**: не использовать, будет иметь значение null.</span><span class="sxs-lookup"><span data-stu-id="421cd-207">**Key**: Not used; will be null.</span></span>  

### <a name="funcsystemdataentitydbcontext-actionstring-systemdataentityinfrastructureinterceptiondatabaselogformatter"></a><span data-ttu-id="421cd-208">Func < System.Data.Entity.DbContext, действие < строка\>, System.Data.Entity.Infrastructure.Interception.DatabaseLogFormatter\></span><span class="sxs-lookup"><span data-stu-id="421cd-208">Func<System.Data.Entity.DbContext, Action<string\>, System.Data.Entity.Infrastructure.Interception.DatabaseLogFormatter\></span></span>  

<span data-ttu-id="421cd-209">**Представленные версии**: EF6.0.0</span><span class="sxs-lookup"><span data-stu-id="421cd-209">**Version introduced**: EF6.0.0</span></span>  

<span data-ttu-id="421cd-210">**Объект, возвращаемый**: фабрику, которая будет использоваться для создания модуля форматирования журнала базы данных, который будет использовать, когда контекст. Свойство Database.Log устанавливается в заданном контексте.</span><span class="sxs-lookup"><span data-stu-id="421cd-210">**Object returned**: A factory that will be used to create the database log formatter that will be used when the context.Database.Log property is set on the given context.</span></span>  

<span data-ttu-id="421cd-211">**Ключ**: не использовать, будет иметь значение null.</span><span class="sxs-lookup"><span data-stu-id="421cd-211">**Key**: Not used; will be null.</span></span>  

### <a name="funcsystemdataentitydbcontext"></a><span data-ttu-id="421cd-212">Func < System.Data.Entity.DbContext\></span><span class="sxs-lookup"><span data-stu-id="421cd-212">Func<System.Data.Entity.DbContext\></span></span>  

<span data-ttu-id="421cd-213">**Представленные версии**: EF6.1.0</span><span class="sxs-lookup"><span data-stu-id="421cd-213">**Version introduced**: EF6.1.0</span></span>  

<span data-ttu-id="421cd-214">**Объект, возвращаемый**: фабрику, которая будет использоваться для создания экземпляров контекста для миграций, если контекст не имеет доступный конструктор без параметров.</span><span class="sxs-lookup"><span data-stu-id="421cd-214">**Object returned**: A factory that will be used to create context instances for Migrations when the context does not have an accessible parameterless constructor.</span></span>  

<span data-ttu-id="421cd-215">**Ключ**: объект типа для типа производном DbContext, для которого требуется фабрику.</span><span class="sxs-lookup"><span data-stu-id="421cd-215">**Key**: The Type object for the type of the derived DbContext for which a factory is needed.</span></span>  

### <a name="funcsystemdataentitycoremetadataedmimetadataannotationserializer"></a><span data-ttu-id="421cd-216">Func < System.Data.Entity.Core.Metadata.Edm.IMetadataAnnotationSerializer\></span><span class="sxs-lookup"><span data-stu-id="421cd-216">Func<System.Data.Entity.Core.Metadata.Edm.IMetadataAnnotationSerializer\></span></span>  

<span data-ttu-id="421cd-217">**Представленные версии**: EF6.1.0</span><span class="sxs-lookup"><span data-stu-id="421cd-217">**Version introduced**: EF6.1.0</span></span>  

<span data-ttu-id="421cd-218">**Объект, возвращаемый**: фабрику, которая будет использоваться для создания сериализаторы для сериализации со строгой типизацией пользовательских заметок, таким образом, сериализации и desterilized в XML для использования в Code First Migrations.</span><span class="sxs-lookup"><span data-stu-id="421cd-218">**Object returned**: A factory that will be used to create serializers for serialization of strongly-typed custom annotations such that they can be serialized and desterilized into XML for use in Code First Migrations.</span></span>  

<span data-ttu-id="421cd-219">**Ключ**: имя, для которого создается заметка сериализации или десериализации.</span><span class="sxs-lookup"><span data-stu-id="421cd-219">**Key**: The name of the annotation that is being serialized or deserialized.</span></span>  

### <a name="funcsystemdataentityinfrastructuretransactionhandler"></a><span data-ttu-id="421cd-220">Func < System.Data.Entity.Infrastructure.TransactionHandler\></span><span class="sxs-lookup"><span data-stu-id="421cd-220">Func<System.Data.Entity.Infrastructure.TransactionHandler\></span></span>  

<span data-ttu-id="421cd-221">**Представленные версии**: EF6.1.0</span><span class="sxs-lookup"><span data-stu-id="421cd-221">**Version introduced**: EF6.1.0</span></span>  

<span data-ttu-id="421cd-222">**Объект, возвращаемый**: фабрику, которая будет использоваться для создания обработчиков для транзакций, таким образом, чтобы специальная обработка может применяться для ситуаций, таких как обработка сбоев.</span><span class="sxs-lookup"><span data-stu-id="421cd-222">**Object returned**: A factory that will be used to create handlers for transactions so that special handling can be applied for situations such as handling commit failures.</span></span>  

<span data-ttu-id="421cd-223">**Ключ**: ExecutionStrategyKey объект, содержащий неизменяемое имя поставщика и, при необходимости, имя сервера, для которого будет использоваться обработчик транзакции.</span><span class="sxs-lookup"><span data-stu-id="421cd-223">**Key**: An ExecutionStrategyKey object that contains the provider invariant name and optionally a server name for which the transaction handler will be used.</span></span>  
