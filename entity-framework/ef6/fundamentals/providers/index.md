---
title: Поставщики Entity Framework в EF6
description: Использование поставщиков Entity Framework в Entity Framework 6
author: divega
ms.date: 06/27/2018
uid: ef6/fundamentals/providers/index
ms.openlocfilehash: bb573cb0666556de8fb37802f465af97fc40673a
ms.sourcegitcommit: abda0872f86eefeca191a9a11bfca976bc14468b
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/14/2020
ms.locfileid: "90073153"
---
# <a name="entity-framework-6-providers"></a><span data-ttu-id="0e06b-103">Поставщики Entity Framework 6</span><span class="sxs-lookup"><span data-stu-id="0e06b-103">Entity Framework 6 Providers</span></span>
> [!NOTE]
> <span data-ttu-id="0e06b-104">**Только в EF6 и более поздних версиях**. Функции, API и другие возможности, описанные на этой странице, появились в Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="0e06b-104">**EF6 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 6.</span></span> <span data-ttu-id="0e06b-105">При использовании более ранней версии могут быть неприменимы некоторые или все сведения.</span><span class="sxs-lookup"><span data-stu-id="0e06b-105">If you are using an earlier version, some or all of the information does not apply.</span></span>

<span data-ttu-id="0e06b-106">Сейчас Entity Framework разрабатывается в рамках лицензии на ПО с открытым исходным кодом, поэтому EF6 и более поздние версии не будут поставляться в составе платформы .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="0e06b-106">The Entity Framework is now being developed under an open-source license and EF6 and above will not be shipped as part of the .NET Framework.</span></span> <span data-ttu-id="0e06b-107">Такая ситуация дает множество преимуществ, но также требует, чтобы поставщики EF были перестроены в соответствии со сборками EF6.</span><span class="sxs-lookup"><span data-stu-id="0e06b-107">This has many advantages but also requires that EF providers be rebuilt against the EF6 assemblies.</span></span> <span data-ttu-id="0e06b-108">Это означает, что поставщики EF для EF5 и предыдущих версий будут работать с EF6 только после их перестроения.</span><span class="sxs-lookup"><span data-stu-id="0e06b-108">This means that EF providers for EF5 and below will not work with EF6 until they have been rebuilt.</span></span>

## <a name="which-providers-are-available-for-ef6"></a><span data-ttu-id="0e06b-109">Какие поставщики доступны для EF6?</span><span class="sxs-lookup"><span data-stu-id="0e06b-109">Which providers are available for EF6?</span></span>

<span data-ttu-id="0e06b-110">В число известных перестроенных для EF6 поставщиков входят следующие.</span><span class="sxs-lookup"><span data-stu-id="0e06b-110">Providers we are aware of that have been rebuilt for EF6 include:</span></span>

*   <span data-ttu-id="0e06b-111">**Поставщик Microsoft SQL Server**</span><span class="sxs-lookup"><span data-stu-id="0e06b-111">**Microsoft SQL Server provider**</span></span>
    *   <span data-ttu-id="0e06b-112">Создан на [базе открытого исходного кода Entity Framework](https://github.com/aspnet/EntityFramework6).</span><span class="sxs-lookup"><span data-stu-id="0e06b-112">Built from the [Entity Framework open source code base](https://github.com/aspnet/EntityFramework6)</span></span>
    *   <span data-ttu-id="0e06b-113">Поставляется в составе [пакета NuGet для EntityFramework](https://nuget.org/packages/EntityFramework).</span><span class="sxs-lookup"><span data-stu-id="0e06b-113">Shipped as part of the [EntityFramework NuGet package](https://nuget.org/packages/EntityFramework)</span></span>
*   <span data-ttu-id="0e06b-114">**Поставщик Microsoft SQL Server Compact Edition**</span><span class="sxs-lookup"><span data-stu-id="0e06b-114">**Microsoft SQL Server Compact Edition provider**</span></span>
    *   <span data-ttu-id="0e06b-115">Создан на [базе открытого исходного кода Entity Framework](https://github.com/aspnet/EntityFramework6).</span><span class="sxs-lookup"><span data-stu-id="0e06b-115">Built from the [Entity Framework open source code base](https://github.com/aspnet/EntityFramework6)</span></span>
    *   <span data-ttu-id="0e06b-116">Поставляется в [пакете NuGet EntityFramework.SqlServerCompact](https://nuget.org/packages/EntityFramework.SqlServerCompact).</span><span class="sxs-lookup"><span data-stu-id="0e06b-116">Shipped in the [EntityFramework.SqlServerCompact NuGet package](https://nuget.org/packages/EntityFramework.SqlServerCompact)</span></span>
*   [<span data-ttu-id="0e06b-117">**Поставщики данных Devart dotConnect**</span><span class="sxs-lookup"><span data-stu-id="0e06b-117">**Devart dotConnect Data Providers**</span></span>](https://www.devart.com/dotconnect/)
    *   <span data-ttu-id="0e06b-118">Существуют сторонние поставщики от компании [Devart](https://www.devart.com/) для различных баз данных, включая Oracle, MySQL, PostgreSQL, SQLite, Salesforce, DB2 и SQL Server.</span><span class="sxs-lookup"><span data-stu-id="0e06b-118">There are third-party providers from [Devart](https://www.devart.com/) for a variety of databases including Oracle, MySQL, PostgreSQL, SQLite, Salesforce, DB2, and SQL Server</span></span>
*   [<span data-ttu-id="0e06b-119">**Поставщики CData Software**</span><span class="sxs-lookup"><span data-stu-id="0e06b-119">**CData Software providers**</span></span>](https://www.cdata.com/ado/)
    *   <span data-ttu-id="0e06b-120">Существуют сторонние поставщики от [CData Software](https://www.cdata.com/ado/) для различных хранилищ данных, включая Salesforce, хранилище таблиц Azure, MySql и многие другие.</span><span class="sxs-lookup"><span data-stu-id="0e06b-120">There are third-party providers from [CData Software](https://www.cdata.com/ado/) for a variety of data stores including Salesforce, Azure Table Storage, MySql, and many more</span></span>
*   <span data-ttu-id="0e06b-121">**Поставщик Firebird**</span><span class="sxs-lookup"><span data-stu-id="0e06b-121">**Firebird provider**</span></span>
    *   <span data-ttu-id="0e06b-122">Доступен как [пакет NuGet](https://www.nuget.org/packages/EntityFramework.Firebird/).</span><span class="sxs-lookup"><span data-stu-id="0e06b-122">Available as a [NuGet Package](https://www.nuget.org/packages/EntityFramework.Firebird/)</span></span>
*   <span data-ttu-id="0e06b-123">**Поставщик Visual Fox Pro**</span><span class="sxs-lookup"><span data-stu-id="0e06b-123">**Visual Fox Pro provider**</span></span>
    *   <span data-ttu-id="0e06b-124">Доступен как [пакет NuGet](https://www.nuget.org/packages/VFPEntityFrameworkProvider2/).</span><span class="sxs-lookup"><span data-stu-id="0e06b-124">Available as a [NuGet package](https://www.nuget.org/packages/VFPEntityFrameworkProvider2/)</span></span>
*   <span data-ttu-id="0e06b-125">**MySQL**</span><span class="sxs-lookup"><span data-stu-id="0e06b-125">**MySQL**</span></span>
    *   [<span data-ttu-id="0e06b-126">MySQL Connector/NET для Entity Framework</span><span class="sxs-lookup"><span data-stu-id="0e06b-126">MySQL Connector/NET for Entity Framework</span></span>](https://dev.mysql.com/doc/connector-net/en/connector-net-entityframework60.html)
*   <span data-ttu-id="0e06b-127">**PostgreSQL**</span><span class="sxs-lookup"><span data-stu-id="0e06b-127">**PostgreSQL**</span></span>
    *   <span data-ttu-id="0e06b-128">Npgsql доступен как [пакет NuGet](https://www.nuget.org/packages/EntityFramework6.Npgsql/).</span><span class="sxs-lookup"><span data-stu-id="0e06b-128">Npgsql is available as a [NuGet package](https://www.nuget.org/packages/EntityFramework6.Npgsql/)</span></span>
*   <span data-ttu-id="0e06b-129">**Oracle**</span><span class="sxs-lookup"><span data-stu-id="0e06b-129">**Oracle**</span></span>
    *   <span data-ttu-id="0e06b-130">ODP.NET доступен как [пакет NuGet](https://www.nuget.org/packages/Oracle.ManagedDataAccess.EntityFramework/).</span><span class="sxs-lookup"><span data-stu-id="0e06b-130">ODP.NET is available as a [NuGet package](https://www.nuget.org/packages/Oracle.ManagedDataAccess.EntityFramework/)</span></span>

<span data-ttu-id="0e06b-131">Обратите внимание, что при включении в этот список не указывается уровень функциональности или поддержки для данного поставщика. Это значит только то, что стала доступной сборка для EF6.</span><span class="sxs-lookup"><span data-stu-id="0e06b-131">Note that inclusion in this list does not indicate the level of functionality or support for a given provider, only that a build for EF6 has been made available.</span></span>

## <a name="registering-ef-providers"></a><span data-ttu-id="0e06b-132">Регистрация поставщиков EF</span><span class="sxs-lookup"><span data-stu-id="0e06b-132">Registering EF providers</span></span>

<span data-ttu-id="0e06b-133">Начиная с Entity Framework 6, поставщики EF можно регистрировать, используя либо конфигурацию на основе кода, либо файл конфигурации приложения.</span><span class="sxs-lookup"><span data-stu-id="0e06b-133">Starting with Entity Framework 6 EF providers can be registered using either code-based configuration or in the application’s config file.</span></span>

### <a name="config-file-registration"></a><span data-ttu-id="0e06b-134">Регистрация в файле конфигурации</span><span class="sxs-lookup"><span data-stu-id="0e06b-134">Config file registration</span></span>

<span data-ttu-id="0e06b-135">Регистрация поставщика EF в файлах app.config или web.config имеет следующий формат.</span><span class="sxs-lookup"><span data-stu-id="0e06b-135">Registration of the EF provider in app.config or web.config has the following format:</span></span>


``` xml
    <entityFramework>
       <providers>
         <provider invariantName="My.Invariant.Name" type="MyProvider.MyProviderServices, MyAssembly" />
       </providers>
    </entityFramework>
```

<span data-ttu-id="0e06b-136">Обратите внимание: часто, если поставщик EF устанавливается из пакета NuGet, пакет NuGet автоматически добавит эту регистрацию в файл конфигурации.</span><span class="sxs-lookup"><span data-stu-id="0e06b-136">Note that often if the EF provider is installed from NuGet, then the NuGet package will automatically add this registration to the config file.</span></span> <span data-ttu-id="0e06b-137">При установке пакета NuGet в проект, который не является запускаемым проектом для вашего приложения, может потребоваться скопировать регистрацию в файл конфигурации для запускаемого проекта.</span><span class="sxs-lookup"><span data-stu-id="0e06b-137">If you install the NuGet package into a project that is not the startup project for your app, then you may need to copy the registration into the config file for your startup project.</span></span>

<span data-ttu-id="0e06b-138">invariantName в этой регистрации является тем же инвариантным именем, которое используется для идентификации поставщика ADO.NET.</span><span class="sxs-lookup"><span data-stu-id="0e06b-138">The “invariantName” in this registration is the same invariant name used to identify an ADO.NET provider.</span></span> <span data-ttu-id="0e06b-139">Его можно найти в виде атрибута invariant в регистрации DbProviderFactory и в виде атрибута providerName в регистрации строки подключения.</span><span class="sxs-lookup"><span data-stu-id="0e06b-139">This can be found as the “invariant” attribute in a DbProviderFactories registration and as the “providerName” attribute in a connection string registration.</span></span> <span data-ttu-id="0e06b-140">Используемое инвариантное имя также должно быть включено в документацию для поставщика.</span><span class="sxs-lookup"><span data-stu-id="0e06b-140">The invariant name to use should also be included in documentation for the provider.</span></span> <span data-ttu-id="0e06b-141">Примеры инвариантных имен: System.Data.SqlClient для SQL Server и System.Data.SqlServerCe.4.0 для SQL Server Compact.</span><span class="sxs-lookup"><span data-stu-id="0e06b-141">Examples of invariant names are “System.Data.SqlClient” for SQL Server and “System.Data.SqlServerCe.4.0” for SQL Server Compact.</span></span>

<span data-ttu-id="0e06b-142">type в данной регистрации — это имя с указанием сборки для типа поставщика, который является производным от System.Data.Entity.Core.Common.DbProviderServices.</span><span class="sxs-lookup"><span data-stu-id="0e06b-142">The “type” in this registration is the assembly-qualified name of the provider type that derives from “System.Data.Entity.Core.Common.DbProviderServices”.</span></span> <span data-ttu-id="0e06b-143">Например, для SQL Compact будет использоваться строка System.Data.Entity.SqlServerCompact.SqlCeProviderServices, EntityFramework.SqlServerCompact.</span><span class="sxs-lookup"><span data-stu-id="0e06b-143">For example, the string to use for SQL Compact is “System.Data.Entity.SqlServerCompact.SqlCeProviderServices, EntityFramework.SqlServerCompact”.</span></span> <span data-ttu-id="0e06b-144">Используемый здесь тип должен быть включен в документацию по поставщику.</span><span class="sxs-lookup"><span data-stu-id="0e06b-144">The type to use here should be included in documentation for the provider.</span></span>

### <a name="code-based-registration"></a><span data-ttu-id="0e06b-145">Регистрация на основе кода</span><span class="sxs-lookup"><span data-stu-id="0e06b-145">Code-based registration</span></span>

<span data-ttu-id="0e06b-146">Начиная с Entity Framework 6, конфигурацию в масштабе приложения для EF можно указывать в коде.</span><span class="sxs-lookup"><span data-stu-id="0e06b-146">Starting with Entity Framework 6 application-wide configuration for EF can be specified in code.</span></span> <span data-ttu-id="0e06b-147">Все сведения см. в разделе _[Entity Framework Code-Based Configuration](https://msdn.microsoft.com/data/jj680699)_ (Конфигурация Entity Framework на основе кода).</span><span class="sxs-lookup"><span data-stu-id="0e06b-147">For full details see _[Entity Framework Code-Based Configuration](https://msdn.microsoft.com/data/jj680699)_.</span></span> <span data-ttu-id="0e06b-148">Стандартным способом регистрации поставщика EF с помощью конфигурации на основе кода является создание класса, производного от System.Data.Entity.DbConfiguration, и его размещение в одной сборке с классом DbContext.</span><span class="sxs-lookup"><span data-stu-id="0e06b-148">The normal way to register an EF provider using code-based configuration is to create a new class that derives from System.Data.Entity.DbConfiguration and place it in the same assembly as your DbContext class.</span></span> <span data-ttu-id="0e06b-149">Затем класс DbConfiguration должен зарегистрировать поставщик в своем конструкторе.</span><span class="sxs-lookup"><span data-stu-id="0e06b-149">Your DbConfiguration class should then register the provider in its constructor.</span></span> <span data-ttu-id="0e06b-150">Например, чтобы зарегистрировать поставщик SQL Compact, класс DbConfiguration выглядит следующим образом.</span><span class="sxs-lookup"><span data-stu-id="0e06b-150">For example, to register the SQL Compact provider the DbConfiguration class looks like this:</span></span>

``` csharp
    public class MyConfiguration : DbConfiguration
    {
        public MyConfiguration()
        {
            SetProviderServices(
                SqlCeProviderServices.ProviderInvariantName,
                SqlCeProviderServices.Instance);
        }
    }
```

<span data-ttu-id="0e06b-151">В этом коде SqlCeProviderServices.ProviderInvariantName является удобным способом выражения строки инвариантного имени поставщика SQL Server Compact (System.Data.SqlServerCe.4.0), а SqlCeProviderServices.Instance возвращает единственный экземпляр поставщика SQL Compact EF.</span><span class="sxs-lookup"><span data-stu-id="0e06b-151">In this code “SqlCeProviderServices.ProviderInvariantName” is a convenience for the SQL Server Compact provider invariant name string (“System.Data.SqlServerCe.4.0”) and SqlCeProviderServices.Instance returns the singleton instance of the SQL Compact EF provider.</span></span>

## <a name="what-if-the-provider-i-need-isnt-available"></a><span data-ttu-id="0e06b-152">Что делать, если нужный поставщик недоступен?</span><span class="sxs-lookup"><span data-stu-id="0e06b-152">What if the provider I need isn’t available?</span></span>

<span data-ttu-id="0e06b-153">Если поставщик доступен в предыдущих версиях EF, мы рекомендуем обратиться к владельцу поставщика и попросить его создать версию EF6.</span><span class="sxs-lookup"><span data-stu-id="0e06b-153">If the provider is available for previous versions of EF, then we encourage you to contact the owner of the provider and ask them to create an EF6 version.</span></span> <span data-ttu-id="0e06b-154">Необходимо включить ссылку на [документацию к модели поставщика EF6](xref:ef6/fundamentals/providers/provider-model).</span><span class="sxs-lookup"><span data-stu-id="0e06b-154">You should include a reference to the [documentation for the EF6 provider model](xref:ef6/fundamentals/providers/provider-model).</span></span>

## <a name="can-i-write-a-provider-myself"></a><span data-ttu-id="0e06b-155">Можно ли создать поставщик самостоятельно?</span><span class="sxs-lookup"><span data-stu-id="0e06b-155">Can I write a provider myself?</span></span>

<span data-ttu-id="0e06b-156">Конечно, вы можете создать поставщик EF самостоятельно несмотря на то, что эта процедура не должна считаться тривиальной задачей.</span><span class="sxs-lookup"><span data-stu-id="0e06b-156">It is certainly possible to create an EF provider yourself although it should not be considered a trivial undertaking.</span></span> <span data-ttu-id="0e06b-157">Лучше всего начать с приведенной выше ссылки о модели поставщика EF6.</span><span class="sxs-lookup"><span data-stu-id="0e06b-157">The the link above about the EF6 provider model is a good place to start.</span></span> <span data-ttu-id="0e06b-158">Кроме того, возможно, вы сочтете полезным использовать качестве отправной точки или в справочных целях код для поставщика SQL Server и SQL CE, включенный в [базу открытого исходного кода EF](https://github.com/aspnet/EntityFramework6).</span><span class="sxs-lookup"><span data-stu-id="0e06b-158">You may also find it useful to use the code for the SQL Server and SQL CE provider included in the [EF open source codebase](https://github.com/aspnet/EntityFramework6) as a starting point or for reference.</span></span>

<span data-ttu-id="0e06b-159">Обратите внимание, что, начиная с EF6, поставщик EF не так тесно связан с поставщиком ADO.NET.</span><span class="sxs-lookup"><span data-stu-id="0e06b-159">Note that starting with EF6 the EF provider is less tightly coupled to the underlying ADO.NET provider.</span></span> <span data-ttu-id="0e06b-160">Это упрощает создание поставщика EF без написания или упаковки классов ADO.NET.</span><span class="sxs-lookup"><span data-stu-id="0e06b-160">This makes it easier to write an EF provider without needing to write or wrap the ADO.NET classes.</span></span>
