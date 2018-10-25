---
title: Поставщики Entity Framework в EF6
author: divega
ms.date: 06/27/2018
ms.assetid: 7BFB7763-CD6C-4520-93A2-7B265F5FA586
ms.openlocfilehash: 0cfe6c30d6533b0207f78a031d342847a7f68632
ms.sourcegitcommit: 5e11125c9b838ce356d673ef5504aec477321724
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/25/2018
ms.locfileid: "50022227"
---
# <a name="entity-framework-6-providers"></a><span data-ttu-id="bbc02-102">Поставщики Entity Framework 6</span><span class="sxs-lookup"><span data-stu-id="bbc02-102">Entity Framework 6 Providers</span></span>
> [!NOTE]
> <span data-ttu-id="bbc02-103">**Только в EF6 и более поздних версиях**. Функции, API и другие возможности, описанные на этой странице, появились в Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="bbc02-103">**EF6 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 6.</span></span> <span data-ttu-id="bbc02-104">При использовании более ранней версии могут быть неприменимы некоторые или все сведения.</span><span class="sxs-lookup"><span data-stu-id="bbc02-104">If you are using an earlier version, some or all of the information does not apply.</span></span>

<span data-ttu-id="bbc02-105">Сейчас Entity Framework разрабатывается в рамках лицензии на ПО с открытым исходным кодом, поэтому EF6 и более поздние версии не будут поставляться в составе платформы .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="bbc02-105">The Entity Framework is now being developed under an open-source license and EF6 and above will not be shipped as part of the .NET Framework.</span></span> <span data-ttu-id="bbc02-106">Такая ситуация дает множество преимуществ, но также требует, чтобы поставщики EF были перестроены в соответствии со сборками EF6.</span><span class="sxs-lookup"><span data-stu-id="bbc02-106">This has many advantages but also requires that EF providers be rebuilt against the EF6 assemblies.</span></span> <span data-ttu-id="bbc02-107">Это означает, что поставщики EF для EF5 и предыдущих версий будут работать с EF6 только после их перестроения.</span><span class="sxs-lookup"><span data-stu-id="bbc02-107">This means that EF providers for EF5 and below will not work with EF6 until they have been rebuilt.</span></span>

## <a name="which-providers-are-available-for-ef6"></a><span data-ttu-id="bbc02-108">Какие поставщики доступны для EF6?</span><span class="sxs-lookup"><span data-stu-id="bbc02-108">Which providers are available for EF6?</span></span>

<span data-ttu-id="bbc02-109">В число известных перестроенных для EF6 поставщиков входят следующие.</span><span class="sxs-lookup"><span data-stu-id="bbc02-109">Providers we are aware of that have been rebuilt for EF6 include:</span></span>

*   <span data-ttu-id="bbc02-110">**Поставщик Microsoft SQL Server**</span><span class="sxs-lookup"><span data-stu-id="bbc02-110">**Microsoft SQL Server provider**</span></span>
    *   <span data-ttu-id="bbc02-111">Создан на [базе открытого исходного кода Entity Framework](http://github.com/aspnet/EntityFramework6).</span><span class="sxs-lookup"><span data-stu-id="bbc02-111">Built from the [Entity Framework open source code base](http://github.com/aspnet/EntityFramework6)</span></span>
    *   <span data-ttu-id="bbc02-112">Поставляется в составе [пакета NuGet для EntityFramework](http://nuget.org/packages/EntityFramework).</span><span class="sxs-lookup"><span data-stu-id="bbc02-112">Shipped as part of the [EntityFramework NuGet package](http://nuget.org/packages/EntityFramework)</span></span>
*   <span data-ttu-id="bbc02-113">**Поставщик Microsoft SQL Server Compact Edition**</span><span class="sxs-lookup"><span data-stu-id="bbc02-113">**Microsoft SQL Server Compact Edition provider**</span></span>
    *   <span data-ttu-id="bbc02-114">Создан на [базе открытого исходного кода Entity Framework](http://github.com/aspnet/EntityFramework6).</span><span class="sxs-lookup"><span data-stu-id="bbc02-114">Built from the [Entity Framework open source code base](http://github.com/aspnet/EntityFramework6)</span></span>
    *   <span data-ttu-id="bbc02-115">Поставляется в [пакете NuGet EntityFramework.SqlServerCompact](http://nuget.org/packages/EntityFramework.SqlServerCompact).</span><span class="sxs-lookup"><span data-stu-id="bbc02-115">Shipped in the [EntityFramework.SqlServerCompact NuGet package](http://nuget.org/packages/EntityFramework.SqlServerCompact)</span></span>
*   [<span data-ttu-id="bbc02-116">**Поставщики данных Devart dotConnect**</span><span class="sxs-lookup"><span data-stu-id="bbc02-116">**Devart dotConnect Data Providers**</span></span>](http://www.devart.com/dotconnect/)
    *   <span data-ttu-id="bbc02-117">Существуют сторонние поставщики от компании [Devart](http://www.devart.com/) для различных баз данных, включая Oracle, MySQL, PostgreSQL, SQLite, Salesforce, DB2 и SQL Server.</span><span class="sxs-lookup"><span data-stu-id="bbc02-117">There are third-party providers from [Devart](http://www.devart.com/) for a variety of databases including Oracle, MySQL, PostgreSQL, SQLite, Salesforce, DB2, and SQL Server</span></span>
*   [<span data-ttu-id="bbc02-118">**Поставщики CData Software**</span><span class="sxs-lookup"><span data-stu-id="bbc02-118">**CData Software providers**</span></span>](http://www.cdata.com/ado/)
    *   <span data-ttu-id="bbc02-119">Существуют сторонние поставщики от [CData Software](http://www.cdata.com/ado/) для различных хранилищ данных, включая Salesforce, хранилище таблиц Azure, MySql и многие другие.</span><span class="sxs-lookup"><span data-stu-id="bbc02-119">There are third-party providers from [CData Software](http://www.cdata.com/ado/) for a variety of data stores including Salesforce, Azure Table Storage, MySql, and many more</span></span>
*   <span data-ttu-id="bbc02-120">**Поставщик Firebird**</span><span class="sxs-lookup"><span data-stu-id="bbc02-120">**Firebird provider**</span></span>
    *   <span data-ttu-id="bbc02-121">Доступен как [пакет NuGet](http://www.nuget.org/packages/FirebirdSql.Data.FirebirdClient/).</span><span class="sxs-lookup"><span data-stu-id="bbc02-121">Available as a [NuGet Package](http://www.nuget.org/packages/FirebirdSql.Data.FirebirdClient/)</span></span>
*   <span data-ttu-id="bbc02-122">**Поставщик Visual Fox Pro**</span><span class="sxs-lookup"><span data-stu-id="bbc02-122">**Visual Fox Pro provider**</span></span>
    *   <span data-ttu-id="bbc02-123">Доступен как [пакет NuGet](https://www.nuget.org/packages/VFPEntityFrameworkProvider2/).</span><span class="sxs-lookup"><span data-stu-id="bbc02-123">Available as a [NuGet package](https://www.nuget.org/packages/VFPEntityFrameworkProvider2/)</span></span>
*   <span data-ttu-id="bbc02-124">**MySQL**</span><span class="sxs-lookup"><span data-stu-id="bbc02-124">**MySQL**</span></span>
    *   [<span data-ttu-id="bbc02-125">MySQL Connector для Net</span><span class="sxs-lookup"><span data-stu-id="bbc02-125">MySQL Connector/Net</span></span>](http://dev.mysql.com/downloads/connector/net/)
*   <span data-ttu-id="bbc02-126">**PostgreSQL**</span><span class="sxs-lookup"><span data-stu-id="bbc02-126">**PostgreSQL**</span></span>
    *   <span data-ttu-id="bbc02-127">Npgsql доступен как [пакет NuGet](http://www.nuget.org/packages/Npgsql.EF6/).</span><span class="sxs-lookup"><span data-stu-id="bbc02-127">Npgsql is available as a [NuGet package](http://www.nuget.org/packages/Npgsql.EF6/)</span></span>
*   <span data-ttu-id="bbc02-128">**Oracle**</span><span class="sxs-lookup"><span data-stu-id="bbc02-128">**Oracle**</span></span>
    *   <span data-ttu-id="bbc02-129">ODP.NET доступен как [пакет NuGet](https://www.nuget.org/packages/Oracle.ManagedDataAccess.EntityFramework/).</span><span class="sxs-lookup"><span data-stu-id="bbc02-129">ODP.NET is available as a [NuGet package](https://www.nuget.org/packages/Oracle.ManagedDataAccess.EntityFramework/)</span></span>

<span data-ttu-id="bbc02-130">Обратите внимание, что при включении в этот список не указывается уровень функциональности или поддержки для данного поставщика. Это значит только то, что стала доступной сборка для EF6.</span><span class="sxs-lookup"><span data-stu-id="bbc02-130">Note that inclusion in this list does not indicate the level of functionality or support for a given provider, only that a build for EF6 has been made available.</span></span>

## <a name="registering-ef-providers"></a><span data-ttu-id="bbc02-131">Регистрация поставщиков EF</span><span class="sxs-lookup"><span data-stu-id="bbc02-131">Registering EF providers</span></span>

<span data-ttu-id="bbc02-132">Начиная с Entity Framework 6, поставщики EF можно регистрировать, используя либо конфигурацию на основе кода, либо файл конфигурации приложения.</span><span class="sxs-lookup"><span data-stu-id="bbc02-132">Starting with Entity Framework 6 EF providers can be registered using either code-based configuration or in the application’s config file.</span></span>

### <a name="config-file-registration"></a><span data-ttu-id="bbc02-133">Регистрация в файле конфигурации</span><span class="sxs-lookup"><span data-stu-id="bbc02-133">Config file registration</span></span>

<span data-ttu-id="bbc02-134">Регистрация поставщика EF в файлах app.config или web.config имеет следующий формат.</span><span class="sxs-lookup"><span data-stu-id="bbc02-134">Registration of the EF provider in app.config or web.config has the following format:</span></span>


``` xml
    <entityFramework>
       <providers>
         <provider invariantName="My.Invariant.Name" type="MyProvider.MyProviderServices, MyAssembly" />
       </providers>
    </entityFramework>
```

<span data-ttu-id="bbc02-135">Обратите внимание: часто, если поставщик EF устанавливается из пакета NuGet, пакет NuGet автоматически добавит эту регистрацию в файл конфигурации.</span><span class="sxs-lookup"><span data-stu-id="bbc02-135">Note that often if the EF provider is installed from NuGet, then the NuGet package will automatically add this registration to the config file.</span></span> <span data-ttu-id="bbc02-136">При установке пакета NuGet в проект, который не является запускаемым проектом для вашего приложения, может потребоваться скопировать регистрацию в файл конфигурации для запускаемого проекта.</span><span class="sxs-lookup"><span data-stu-id="bbc02-136">If you install the NuGet package into a project that is not the startup project for your app, then you may need to copy the registration into the config file for your startup project.</span></span>

<span data-ttu-id="bbc02-137">invariantName в этой регистрации является тем же инвариантным именем, которое используется для идентификации поставщика ADO.NET.</span><span class="sxs-lookup"><span data-stu-id="bbc02-137">The “invariantName” in this registration is the same invariant name used to identify an ADO.NET provider.</span></span> <span data-ttu-id="bbc02-138">Его можно найти в виде атрибута invariant в регистрации DbProviderFactory и в виде атрибута providerName в регистрации строки подключения.</span><span class="sxs-lookup"><span data-stu-id="bbc02-138">This can be found as the “invariant” attribute in a DbProviderFactories registration and as the “providerName” attribute in a connection string registration.</span></span> <span data-ttu-id="bbc02-139">Используемое инвариантное имя также должно быть включено в документацию для поставщика.</span><span class="sxs-lookup"><span data-stu-id="bbc02-139">The invariant name to use should also be included in documentation for the provider.</span></span> <span data-ttu-id="bbc02-140">Примеры инвариантных имен: System.Data.SqlClient для SQL Server и System.Data.SqlServerCe.4.0 для SQL Server Compact.</span><span class="sxs-lookup"><span data-stu-id="bbc02-140">Examples of invariant names are “System.Data.SqlClient” for SQL Server and “System.Data.SqlServerCe.4.0” for SQL Server Compact.</span></span>

<span data-ttu-id="bbc02-141">type в данной регистрации — это имя с указанием сборки для типа поставщика, который является производным от System.Data.Entity.Core.Common.DbProviderServices.</span><span class="sxs-lookup"><span data-stu-id="bbc02-141">The “type” in this registration is the assembly-qualified name of the provider type that derives from “System.Data.Entity.Core.Common.DbProviderServices”.</span></span> <span data-ttu-id="bbc02-142">Например, для SQL Compact будет использоваться строка System.Data.Entity.SqlServerCompact.SqlCeProviderServices, EntityFramework.SqlServerCompact.</span><span class="sxs-lookup"><span data-stu-id="bbc02-142">For example, the string to use for SQL Compact is “System.Data.Entity.SqlServerCompact.SqlCeProviderServices, EntityFramework.SqlServerCompact”.</span></span> <span data-ttu-id="bbc02-143">Используемый здесь тип должен быть включен в документацию по поставщику.</span><span class="sxs-lookup"><span data-stu-id="bbc02-143">The type to use here should be included in documentation for the provider.</span></span>

### <a name="code-based-registration"></a><span data-ttu-id="bbc02-144">Регистрация на основе кода</span><span class="sxs-lookup"><span data-stu-id="bbc02-144">Code-based registration</span></span>

<span data-ttu-id="bbc02-145">Начиная с Entity Framework 6, конфигурацию в масштабе приложения для EF можно указывать в коде.</span><span class="sxs-lookup"><span data-stu-id="bbc02-145">Starting with Entity Framework 6 application-wide configuration for EF can be specified in code.</span></span> <span data-ttu-id="bbc02-146">Все сведения см. в разделе _[Entity Framework Code-Based Configuration](https://msdn.microsoft.com/data/jj680699)_ (Конфигурация Entity Framework на основе кода).</span><span class="sxs-lookup"><span data-stu-id="bbc02-146">For full details see _[Entity Framework Code-Based Configuration](https://msdn.microsoft.com/data/jj680699)_.</span></span> <span data-ttu-id="bbc02-147">Стандартным способом регистрации поставщика EF с помощью конфигурации на основе кода является создание класса, производного от System.Data.Entity.DbConfiguration, и его размещение в одной сборке с классом DbContext.</span><span class="sxs-lookup"><span data-stu-id="bbc02-147">The normal way to register an EF provider using code-based configuration is to create a new class that derives from System.Data.Entity.DbConfiguration and place it in the same assembly as your DbContext class.</span></span> <span data-ttu-id="bbc02-148">Затем класс DbConfiguration должен зарегистрировать поставщик в своем конструкторе.</span><span class="sxs-lookup"><span data-stu-id="bbc02-148">Your DbConfiguration class should then register the provider in its constructor.</span></span> <span data-ttu-id="bbc02-149">Например, чтобы зарегистрировать поставщик SQL Compact, класс DbConfiguration выглядит следующим образом.</span><span class="sxs-lookup"><span data-stu-id="bbc02-149">For example, to register the SQL Compact provider the DbConfiguration class looks like this:</span></span>

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

<span data-ttu-id="bbc02-150">В этом коде SqlCeProviderServices.ProviderInvariantName является удобным способом выражения строки инвариантного имени поставщика SQL Server Compact (System.Data.SqlServerCe.4.0), а SqlCeProviderServices.Instance возвращает единственный экземпляр поставщика SQL Compact EF.</span><span class="sxs-lookup"><span data-stu-id="bbc02-150">In this code “SqlCeProviderServices.ProviderInvariantName” is a convenience for the SQL Server Compact provider invariant name string (“System.Data.SqlServerCe.4.0”) and SqlCeProviderServices.Instance returns the singleton instance of the SQL Compact EF provider.</span></span>

## <a name="what-if-the-provider-i-need-isnt-available"></a><span data-ttu-id="bbc02-151">Что делать, если нужный поставщик недоступен?</span><span class="sxs-lookup"><span data-stu-id="bbc02-151">What if the provider I need isn’t available?</span></span>

<span data-ttu-id="bbc02-152">Если поставщик доступен в предыдущих версиях EF, мы рекомендуем обратиться к владельцу поставщика и попросить его создать версию EF6.</span><span class="sxs-lookup"><span data-stu-id="bbc02-152">If the provider is available for previous versions of EF, then we encourage you to contact the owner of the provider and ask them to create an EF6 version.</span></span> <span data-ttu-id="bbc02-153">Необходимо включить ссылку на [документацию к модели поставщика EF6](~/ef6/fundamentals/providers/provider-model.md).</span><span class="sxs-lookup"><span data-stu-id="bbc02-153">You should include a reference to the [documentation for the EF6 provider model](~/ef6/fundamentals/providers/provider-model.md).</span></span>

## <a name="can-i-write-a-provider-myself"></a><span data-ttu-id="bbc02-154">Можно ли создать поставщик самостоятельно?</span><span class="sxs-lookup"><span data-stu-id="bbc02-154">Can I write a provider myself?</span></span>

<span data-ttu-id="bbc02-155">Конечно, вы можете создать поставщик EF самостоятельно несмотря на то, что эта процедура не должна считаться тривиальной задачей.</span><span class="sxs-lookup"><span data-stu-id="bbc02-155">It is certainly possible to create an EF provider yourself although it should not be considered a trivial undertaking.</span></span> <span data-ttu-id="bbc02-156">Лучше всего начать с приведенной выше ссылки о модели поставщика EF6.</span><span class="sxs-lookup"><span data-stu-id="bbc02-156">The the link above about the EF6 provider model is a good place to start.</span></span> <span data-ttu-id="bbc02-157">Кроме того, возможно, вы сочтете полезным использовать качестве отправной точки или в справочных целях код для поставщика SQL Server и SQL CE, включенный в [базу открытого исходного кода EF](https://github.com/aspnet/EntityFramework6).</span><span class="sxs-lookup"><span data-stu-id="bbc02-157">You may also find it useful to use the code for the SQL Server and SQL CE provider included in the [EF open source codebase](https://github.com/aspnet/EntityFramework6) as a starting point or for reference.</span></span>

<span data-ttu-id="bbc02-158">Обратите внимание, что, начиная с EF6, поставщик EF не так тесно связан с поставщиком ADO.NET.</span><span class="sxs-lookup"><span data-stu-id="bbc02-158">Note that starting with EF6 the EF provider is less tightly coupled to the underlying ADO.NET provider.</span></span> <span data-ttu-id="bbc02-159">Это упрощает создание поставщика EF без написания или упаковки классов ADO.NET.</span><span class="sxs-lookup"><span data-stu-id="bbc02-159">This makes it easier to write an EF provider without needing to write or wrap the ADO.NET classes.</span></span>
