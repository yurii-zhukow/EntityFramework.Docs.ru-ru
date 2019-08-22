---
title: Параметры файла конфигурации — EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 000044c6-1d32-4cf7-ae1f-ea21d86ebf8f
ms.openlocfilehash: 86389e4a3a3bac46e2a4cf2da648a4b19e29f3c3
ms.sourcegitcommit: 299011fc4bd576eed58a4274f967639fa13fec53
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/21/2019
ms.locfileid: "69886555"
---
# <a name="configuration-file-settings"></a><span data-ttu-id="76a95-102">Параметры файла конфигурации</span><span class="sxs-lookup"><span data-stu-id="76a95-102">Configuration File Settings</span></span>
<span data-ttu-id="76a95-103">Entity Framework позволяет указать несколько параметров из файла конфигурации.</span><span class="sxs-lookup"><span data-stu-id="76a95-103">Entity Framework allows a number of settings to be specified from the configuration file.</span></span> <span data-ttu-id="76a95-104">В целом EF соответствует принципу "соглашение над конфигурацией". все параметры, обсуждаемые в этой записи, имеют поведение по умолчанию. вам нужно только изменить параметр, когда значение по умолчанию больше не соответствует вашим требованиям.</span><span class="sxs-lookup"><span data-stu-id="76a95-104">In general EF follows a ‘convention over configuration’ principle: all the settings discussed in this post have a default behavior, you only need to worry about changing the setting when the default no longer satisfies your requirements.</span></span>  

## <a name="a-code-based-alternative"></a><span data-ttu-id="76a95-105">Альтернатива на основе кода</span><span class="sxs-lookup"><span data-stu-id="76a95-105">A Code-Based Alternative</span></span>  

<span data-ttu-id="76a95-106">Все эти параметры также можно применить с помощью кода.</span><span class="sxs-lookup"><span data-stu-id="76a95-106">All of these settings can also be applied using code.</span></span> <span data-ttu-id="76a95-107">Начиная с EF6 мы предоставили [конфигурацию на основе кода](code-based.md), которая предоставляет централизованный способ применения конфигурации из кода.</span><span class="sxs-lookup"><span data-stu-id="76a95-107">Starting in EF6 we introduced [code-based configuration](code-based.md), which provides a central way of applying configuration from code.</span></span> <span data-ttu-id="76a95-108">До EF6 конфигурация по-прежнему может быть применена из кода, но для настройки различных областей необходимо использовать различные интерфейсы API.</span><span class="sxs-lookup"><span data-stu-id="76a95-108">Prior to EF6, configuration can still be applied from code but you need to use various APIs to configure different areas.</span></span> <span data-ttu-id="76a95-109">Параметр файла конфигурации позволяет легко изменять эти параметры во время развертывания без обновления кода.</span><span class="sxs-lookup"><span data-stu-id="76a95-109">The configuration file option allows these settings to be easily changed during deployment without updating your code.</span></span>

## <a name="the-entity-framework-configuration-section"></a><span data-ttu-id="76a95-110">Раздел конфигурации Entity Framework</span><span class="sxs-lookup"><span data-stu-id="76a95-110">The Entity Framework Configuration Section</span></span>  

<span data-ttu-id="76a95-111">Начиная с EF 4.1, можно задать инициализатор базы данных для контекста с помощью раздела **appSettings** файла конфигурации.</span><span class="sxs-lookup"><span data-stu-id="76a95-111">Starting with EF4.1 you could set the database initializer for a context using the **appSettings** section of the configuration file.</span></span> <span data-ttu-id="76a95-112">В EF 4,3 мы предоставили раздел Custom **entityFramework** для управления новыми параметрами.</span><span class="sxs-lookup"><span data-stu-id="76a95-112">In EF 4.3 we introduced the custom **entityFramework** section to handle the new settings.</span></span> <span data-ttu-id="76a95-113">Entity Framework будет по-прежнему распознать инициализаторы баз данных, установленные с использованием старого формата, но мы рекомендуем по возможности перейти к новому формату.</span><span class="sxs-lookup"><span data-stu-id="76a95-113">Entity Framework will still recognize database initializers set using the old format, but we recommend moving to the new format where possible.</span></span>

<span data-ttu-id="76a95-114">Раздел **entityFramework** был автоматически добавлен в файл конфигурации проекта при установке пакета NuGet entityFramework.</span><span class="sxs-lookup"><span data-stu-id="76a95-114">The **entityFramework** section was automatically added to the configuration file of your project when you installed the EntityFramework NuGet package.</span></span>  

``` xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <configSections>
    <!-- For more information on Entity Framework configuration, visit http://go.microsoft.com/fwlink/?LinkID=237468 -->
    <section name="entityFramework"
       type="System.Data.Entity.Internal.ConfigFile.EntityFrameworkSection, EntityFramework, Version=4.3.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089" />
  </configSections>
</configuration>
```  

## <a name="connection-strings"></a><span data-ttu-id="76a95-115">Строки подключения</span><span class="sxs-lookup"><span data-stu-id="76a95-115">Connection Strings</span></span>  

<span data-ttu-id="76a95-116">На [этой странице](~/ef6/fundamentals/configuring/connection-strings.md) содержатся дополнительные сведения о том, как Entity Framework определяет используемую базу данных, включая строки подключения в файле конфигурации.</span><span class="sxs-lookup"><span data-stu-id="76a95-116">[This page](~/ef6/fundamentals/configuring/connection-strings.md) provides more details on how Entity Framework determines the database to be used, including connection strings in the configuration file.</span></span>  

<span data-ttu-id="76a95-117">Строки подключения попадают в стандартный элемент **ConnectionString** и не нуждаются в разделе **entityFramework** .</span><span class="sxs-lookup"><span data-stu-id="76a95-117">Connection strings go in the standard **connectionStrings** element and do not require the **entityFramework** section.</span></span>  

<span data-ttu-id="76a95-118">Модели на основе Code First используют обычные строки подключения ADO.NET.</span><span class="sxs-lookup"><span data-stu-id="76a95-118">Code First based models use normal ADO.NET connection strings.</span></span> <span data-ttu-id="76a95-119">Например:</span><span class="sxs-lookup"><span data-stu-id="76a95-119">For example:</span></span>  

``` xml
<connectionStrings>
  <add name="BlogContext"  
        providerName="System.Data.SqlClient"  
        connectionString="Server=.\SQLEXPRESS;Database=Blogging;Integrated Security=True;"/>
</connectionStrings>
```  

<span data-ttu-id="76a95-120">Модели на основе конструктора EF используют специальные строки подключения EF.</span><span class="sxs-lookup"><span data-stu-id="76a95-120">EF Designer based models use special EF connection strings.</span></span> <span data-ttu-id="76a95-121">Например:</span><span class="sxs-lookup"><span data-stu-id="76a95-121">For example:</span></span>  

``` xml  
<connectionStrings>
  <add name="BlogContext"  
    connectionString=
      "metadata=
        res://*/BloggingModel.csdl|
        res://*/BloggingModel.ssdl|
        res://*/BloggingModel.msl;
      provider=System.Data.SqlClient;
      provider connection string=
        &quot;data source=(localdb)\mssqllocaldb;
        initial catalog=Blogging;
        integrated security=True;
        multipleactiveresultsets=True;&quot;"
     providerName="System.Data.EntityClient" />
</connectionStrings>
```

## <a name="code-based-configuration-type-ef6-onwards"></a><span data-ttu-id="76a95-122">Тип конфигурации на основе кода (EF6)</span><span class="sxs-lookup"><span data-stu-id="76a95-122">Code-Based Configuration Type (EF6 Onwards)</span></span>  

<span data-ttu-id="76a95-123">Начиная с EF6, можно указать DbConfiguration для EF для использования в [конфигурации на основе кода](code-based.md) в приложении.</span><span class="sxs-lookup"><span data-stu-id="76a95-123">Starting with EF6, you can specify the DbConfiguration for EF to use for [code-based configuration](code-based.md) in your application.</span></span> <span data-ttu-id="76a95-124">В большинстве случаев не нужно указывать этот параметр, так как EF автоматически обнаружит ваш DbConfiguration.</span><span class="sxs-lookup"><span data-stu-id="76a95-124">In most cases you don't need to specify this setting as EF will automatically discover your DbConfiguration.</span></span> <span data-ttu-id="76a95-125">Дополнительные сведения о том, когда может потребоваться указать DbConfiguration в файле конфигурации, см. в разделе **Перемещение DbConfiguration** в [конфигурации на основе кода](code-based.md).</span><span class="sxs-lookup"><span data-stu-id="76a95-125">For details of when you may need to specify DbConfiguration in your config file see the **Moving DbConfiguration** section of [Code-Based Configuration](code-based.md).</span></span>  

<span data-ttu-id="76a95-126">Чтобы задать тип DbConfiguration, укажите полное имя типа сборки в элементе **кодеконфигуратионтипе** .</span><span class="sxs-lookup"><span data-stu-id="76a95-126">To set a DbConfiguration type, you specify the assembly qualified type name in the **codeConfigurationType** element.</span></span>  

> [!NOTE]
> <span data-ttu-id="76a95-127">Полное имя сборки — это полное имя пространства имен, за которым следует запятая, а затем сборка, в которой находится тип.</span><span class="sxs-lookup"><span data-stu-id="76a95-127">An assembly qualified name is the namespace qualified name, followed by a comma, then the assembly that the type resides in.</span></span> <span data-ttu-id="76a95-128">При необходимости можно также указать версию сборки, язык и региональные параметры и токен открытого ключа.</span><span class="sxs-lookup"><span data-stu-id="76a95-128">You can optionally also specify the assembly version, culture and public key token.</span></span>  

``` xml
<entityFramework codeConfigurationType="MyNamespace.MyConfiguration, MyAssembly">
</entityFramework>
```  

## <a name="ef-database-providers-ef6-onwards"></a><span data-ttu-id="76a95-129">Поставщики баз данных EF (EF6)</span><span class="sxs-lookup"><span data-stu-id="76a95-129">EF Database Providers (EF6 Onwards)</span></span>  

<span data-ttu-id="76a95-130">До EF6 часть поставщика базы данных, относящаяся к Entity Framework, должна быть включена в состав основного поставщика ADO.NET.</span><span class="sxs-lookup"><span data-stu-id="76a95-130">Prior to EF6, Entity Framework-specific parts of a database provider had to be included as part of the core ADO.NET provider.</span></span> <span data-ttu-id="76a95-131">Начиная с EF6, относящиеся к EF части теперь управляются и регистрируются отдельно.</span><span class="sxs-lookup"><span data-stu-id="76a95-131">Starting with EF6, the EF specific parts are now managed and registered separately.</span></span>  

<span data-ttu-id="76a95-132">Обычно вам не нужно регистрировать поставщиков самостоятельно.</span><span class="sxs-lookup"><span data-stu-id="76a95-132">Normally you won't need to register providers yourself.</span></span> <span data-ttu-id="76a95-133">Обычно это происходит поставщиком при установке.</span><span class="sxs-lookup"><span data-stu-id="76a95-133">This will typically be done by the provider when you install it.</span></span>  

<span data-ttu-id="76a95-134">Поставщики регистрируются путем включения элемента **поставщика** в дочерний раздел **providers** раздела **entityFramework** .</span><span class="sxs-lookup"><span data-stu-id="76a95-134">Providers are registered by including a **provider** element under the **providers** child section of the **entityFramework** section.</span></span> <span data-ttu-id="76a95-135">Существует два обязательных атрибута для записи поставщика:</span><span class="sxs-lookup"><span data-stu-id="76a95-135">There are two required attributes for a provider entry:</span></span>  

- <span data-ttu-id="76a95-136">**invariantName** идентифицирует основного поставщика ADO.NET, к которому обращается этот поставщик EF</span><span class="sxs-lookup"><span data-stu-id="76a95-136">**invariantName** identifies the core ADO.NET provider that this EF provider targets</span></span>  
- <span data-ttu-id="76a95-137">**Type** — это полное имя типа сборки для реализации поставщика EF</span><span class="sxs-lookup"><span data-stu-id="76a95-137">**type** is the assembly qualified type name of the EF provider implementation</span></span>  

> [!NOTE]
> <span data-ttu-id="76a95-138">Полное имя сборки — это полное имя пространства имен, за которым следует запятая, а затем сборка, в которой находится тип.</span><span class="sxs-lookup"><span data-stu-id="76a95-138">An assembly qualified name is the namespace qualified name, followed by a comma, then the assembly that the type resides in.</span></span> <span data-ttu-id="76a95-139">При необходимости можно также указать версию сборки, язык и региональные параметры и токен открытого ключа.</span><span class="sxs-lookup"><span data-stu-id="76a95-139">You can optionally also specify the assembly version, culture and public key token.</span></span>  

<span data-ttu-id="76a95-140">В качестве примера здесь приведена запись, созданная для регистрации поставщика SQL Server по умолчанию при установке Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="76a95-140">As an example here is the entry created to register the default SQL Server provider when you install Entity Framework.</span></span>  

``` xml  
<providers>
  <provider invariantName="System.Data.SqlClient" type="System.Data.Entity.SqlServer.SqlProviderServices, EntityFramework.SqlServer" />
</providers>
```  

## <a name="interceptors-ef61-onwards"></a><span data-ttu-id="76a95-141">Перехватчики (EF 6.1 и выше)</span><span class="sxs-lookup"><span data-stu-id="76a95-141">Interceptors (EF6.1 Onwards)</span></span>  

<span data-ttu-id="76a95-142">Начиная с EF 6.1, можно регистрировать перехватчики в файле конфигурации.</span><span class="sxs-lookup"><span data-stu-id="76a95-142">Starting with EF6.1 you can register interceptors in the configuration file.</span></span> <span data-ttu-id="76a95-143">Перехватчики позволяют выполнять дополнительную логику, когда EF выполняет определенные операции, такие как выполнение запросов к базе данных, открытие подключений и т. д.</span><span class="sxs-lookup"><span data-stu-id="76a95-143">Interceptors allow you to run additional logic when EF performs certain operations, such as executing database queries, opening connections, etc.</span></span>  

<span data-ttu-id="76a95-144">Перехватчики регистрируются путем включения элемента **перехватчика** в дочерний раздел **перехватчиков** раздела **entityFramework** .</span><span class="sxs-lookup"><span data-stu-id="76a95-144">Interceptors are registered by including an **interceptor** element under the **interceptors** child section of the **entityFramework** section.</span></span> <span data-ttu-id="76a95-145">Например, следующая конфигурация регистрирует встроенный перехватчик **датабаселогжер** , который будет записывать все операции базы данных в консоль.</span><span class="sxs-lookup"><span data-stu-id="76a95-145">For example, the following configuration registers the built-in **DatabaseLogger** interceptor that will log all database operations to the Console.</span></span>  

``` xml  
<interceptors>
  <interceptor type="System.Data.Entity.Infrastructure.Interception.DatabaseLogger, EntityFramework"/>
</interceptors>
```  

### <a name="logging-database-operations-to-a-file-ef61-onwards"></a><span data-ttu-id="76a95-146">Ведение журнала операций базы данных в файл (EF 6.1/назад)</span><span class="sxs-lookup"><span data-stu-id="76a95-146">Logging Database Operations to a File (EF6.1 Onwards)</span></span>  

<span data-ttu-id="76a95-147">Регистрация перехватчиков с помощью файла конфигурации особенно полезна, если требуется добавить ведение журнала в существующее приложение, чтобы упростить отладку проблемы.</span><span class="sxs-lookup"><span data-stu-id="76a95-147">Registering interceptors via the config file is especially useful when you want to add logging to an existing application to help debug an issue.</span></span> <span data-ttu-id="76a95-148">**Датабаселогжер** поддерживает ведение журнала в файл, предоставляя имя файла в качестве параметра конструктора.</span><span class="sxs-lookup"><span data-stu-id="76a95-148">**DatabaseLogger** supports logging to a file by supplying the file name as a constructor parameter.</span></span>  

``` xml  
<interceptors>
  <interceptor type="System.Data.Entity.Infrastructure.Interception.DatabaseLogger, EntityFramework">
    <parameters>
      <parameter value="C:\Temp\LogOutput.txt"/>
    </parameters>
  </interceptor>
</interceptors>
```  

<span data-ttu-id="76a95-149">По умолчанию файл журнала будет перезаписан новым файлом при каждом запуске приложения.</span><span class="sxs-lookup"><span data-stu-id="76a95-149">By default this will cause the log file to be overwritten with a new file each time the app starts.</span></span> <span data-ttu-id="76a95-150">Чтобы присоединиться к файлу журнала, если он уже существует, используйте нечто вроде:</span><span class="sxs-lookup"><span data-stu-id="76a95-150">To instead append to the log file if it already exists use something like:</span></span>  

``` xml  
<interceptors>
  <interceptor type="System.Data.Entity.Infrastructure.Interception.DatabaseLogger, EntityFramework">
    <parameters>
      <parameter value="C:\Temp\LogOutput.txt"/>
      <parameter value="true" type="System.Boolean"/>
    </parameters>
  </interceptor>
</interceptors>
```  

<span data-ttu-id="76a95-151">Дополнительные сведения о **датабаселогжер** и регистрации перехватчиков см. в записи блога [EF 6,1: Включение ведения журнала без повторной компиляции](https://blog.oneunicorn.com/2014/02/09/ef-6-1-turning-on-logging-without-recompiling/).</span><span class="sxs-lookup"><span data-stu-id="76a95-151">For additional information on **DatabaseLogger** and registering interceptors, see the blog post [EF 6.1: Turning on logging without recompiling](https://blog.oneunicorn.com/2014/02/09/ef-6-1-turning-on-logging-without-recompiling/).</span></span>  

## <a name="code-first-default-connection-factory"></a><span data-ttu-id="76a95-152">Фабрика подключений по умолчанию Code First</span><span class="sxs-lookup"><span data-stu-id="76a95-152">Code First Default Connection Factory</span></span>  

<span data-ttu-id="76a95-153">Раздел конфигурации позволяет указать фабрику соединений по умолчанию, которую Code First следует использовать для определения местоположения базы данных, используемой для контекста.</span><span class="sxs-lookup"><span data-stu-id="76a95-153">The configuration section allows you to specify a default connection factory that Code First should use to locate a database to use for a context.</span></span> <span data-ttu-id="76a95-154">Фабрика соединений по умолчанию используется только в том случае, если строка подключения не была добавлена в файл конфигурации для контекста.</span><span class="sxs-lookup"><span data-stu-id="76a95-154">The default connection factory is only used when no connection string has been added to the configuration file for a context.</span></span>  

<span data-ttu-id="76a95-155">При установке пакета для EF NuGet была зарегистрирована фабрика соединений по умолчанию, указывающая на SQL Express или LocalDB в зависимости от того, какая из них установлена.</span><span class="sxs-lookup"><span data-stu-id="76a95-155">When you installed the EF NuGet package a default connection factory was registered that points to either SQL Express or LocalDB, depending on which one you have installed.</span></span>  

<span data-ttu-id="76a95-156">Чтобы задать фабрику подключений, укажите полное имя типа сборки в элементе **дефаултконнектионфактори** .</span><span class="sxs-lookup"><span data-stu-id="76a95-156">To set a connection factory, you specify the assembly qualified type name in the **defaultConnectionFactory** element.</span></span>  

> [!NOTE]
> <span data-ttu-id="76a95-157">Полное имя сборки — это полное имя пространства имен, за которым следует запятая, а затем сборка, в которой находится тип.</span><span class="sxs-lookup"><span data-stu-id="76a95-157">An assembly qualified name is the namespace qualified name, followed by a comma, then the assembly that the type resides in.</span></span> <span data-ttu-id="76a95-158">При необходимости можно также указать версию сборки, язык и региональные параметры и токен открытого ключа.</span><span class="sxs-lookup"><span data-stu-id="76a95-158">You can optionally also specify the assembly version, culture and public key token.</span></span>  

<span data-ttu-id="76a95-159">Ниже приведен пример настройки фабрики подключений по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="76a95-159">Here is an example of setting your own default connection factory:</span></span>  

``` xml  
<entityFramework>
  <defaultConnectionFactory type="MyNamespace.MyCustomFactory, MyAssembly"/>
</entityFramework>
```  

<span data-ttu-id="76a95-160">В приведенном выше примере для пользовательской фабрики требуется конструктор без параметров.</span><span class="sxs-lookup"><span data-stu-id="76a95-160">The above example requires the custom factory to have a parameterless constructor.</span></span> <span data-ttu-id="76a95-161">При необходимости можно указать параметры конструктора с помощью элемента **Parameters** .</span><span class="sxs-lookup"><span data-stu-id="76a95-161">If needed, you can specify constructor parameters using the **parameters** element.</span></span>  

<span data-ttu-id="76a95-162">Например, Склцеконнектионфактори, который входит в Entity Framework, требует указать неизменяемое имя поставщика для конструктора.</span><span class="sxs-lookup"><span data-stu-id="76a95-162">For example, the SqlCeConnectionFactory, that is included in Entity Framework, requires you to supply a provider invariant name to the constructor.</span></span> <span data-ttu-id="76a95-163">Неизменяемое имя поставщика определяет версию SQL Compact, которую вы хотите использовать.</span><span class="sxs-lookup"><span data-stu-id="76a95-163">The provider invariant name identifies the version of SQL Compact you want to use.</span></span> <span data-ttu-id="76a95-164">Следующая конфигурация приведет к тому, что контексты будут использовать SQL Compact версии 4,0 по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="76a95-164">The following configuration will cause contexts to use SQL Compact version 4.0 by default.</span></span>  

``` xml  
<entityFramework>
  <defaultConnectionFactory type="System.Data.Entity.Infrastructure.SqlCeConnectionFactory, EntityFramework">
    <parameters>
      <parameter value="System.Data.SqlServerCe.4.0" />
    </parameters>
  </defaultConnectionFactory>
</entityFramework>
```  

<span data-ttu-id="76a95-165">Если не задать фабрику соединений по умолчанию, Code First использует Склконнектионфактори, указав `.\SQLEXPRESS`.</span><span class="sxs-lookup"><span data-stu-id="76a95-165">If you don’t set a default connection factory, Code First uses the SqlConnectionFactory, pointing to `.\SQLEXPRESS`.</span></span> <span data-ttu-id="76a95-166">Склконнектионфактори также имеет конструктор, позволяющий переопределять части строки подключения.</span><span class="sxs-lookup"><span data-stu-id="76a95-166">SqlConnectionFactory also has a constructor that allows you to override parts of the connection string.</span></span> <span data-ttu-id="76a95-167">Если вы хотите использовать экземпляр SQL Server, отличный от `.\SQLEXPRESS` , можно использовать этот конструктор для задания сервера.</span><span class="sxs-lookup"><span data-stu-id="76a95-167">If you want to use a SQL Server instance other than `.\SQLEXPRESS` you can use this constructor to set the server.</span></span>  

<span data-ttu-id="76a95-168">Следующая конфигурация приведет к тому, что Code First будет использовать **мидатабасесервер** для контекстов, для которых не задана явная строка подключения.</span><span class="sxs-lookup"><span data-stu-id="76a95-168">The following configuration will cause Code First to use **MyDatabaseServer** for contexts that don’t have an explicit connection string set.</span></span>  

``` xml  
<entityFramework>
  <defaultConnectionFactory type="System.Data.Entity.Infrastructure.SqlConnectionFactory, EntityFramework">
    <parameters>
      <parameter value="Data Source=MyDatabaseServer; Integrated Security=True; MultipleActiveResultSets=True" />
    </parameters>
  </defaultConnectionFactory>
</entityFramework>
```  

<span data-ttu-id="76a95-169">По умолчанию предполагается, что аргументы конструктора имеют тип String.</span><span class="sxs-lookup"><span data-stu-id="76a95-169">By default, it’s assumed that constructor arguments are of type string.</span></span> <span data-ttu-id="76a95-170">Для изменения этого атрибута можно использовать атрибут Type.</span><span class="sxs-lookup"><span data-stu-id="76a95-170">You can use the type attribute to change this.</span></span>  

``` xml
<parameter value="2" type="System.Int32" />
```  

## <a name="database-initializers"></a><span data-ttu-id="76a95-171">Инициализаторы баз данных</span><span class="sxs-lookup"><span data-stu-id="76a95-171">Database Initializers</span></span>  

<span data-ttu-id="76a95-172">Инициализаторы баз данных настраиваются отдельно для каждого контекста.</span><span class="sxs-lookup"><span data-stu-id="76a95-172">Database initializers are configured on a per-context basis.</span></span> <span data-ttu-id="76a95-173">Их можно задать в файле конфигурации с помощью элемента **context** .</span><span class="sxs-lookup"><span data-stu-id="76a95-173">They can be set in the configuration file using the **context** element.</span></span> <span data-ttu-id="76a95-174">Этот элемент использует полное имя сборки для задания настраиваемого контекста.</span><span class="sxs-lookup"><span data-stu-id="76a95-174">This element uses the assembly qualified name to identify the context being configured.</span></span>  

<span data-ttu-id="76a95-175">По умолчанию Code First контексты настроены для использования инициализатора CreateDatabaseIfNotExists.</span><span class="sxs-lookup"><span data-stu-id="76a95-175">By default, Code First contexts are configured to use the CreateDatabaseIfNotExists initializer.</span></span> <span data-ttu-id="76a95-176">В элементе **context** имеется атрибут **дисабледатабасеинитиализатион** , который можно использовать для отключения инициализации базы данных.</span><span class="sxs-lookup"><span data-stu-id="76a95-176">There is a **disableDatabaseInitialization** attribute on the **context** element that can be used to disable database initialization.</span></span>  

<span data-ttu-id="76a95-177">Например, следующая конфигурация отключает инициализацию базы данных для контекста блога. BlogContext, определенного в файле MyAssembly. dll.</span><span class="sxs-lookup"><span data-stu-id="76a95-177">For example, the following configuration disables database initialization for the Blogging.BlogContext context defined in MyAssembly.dll.</span></span>  

``` xml  
<contexts>
  <context type=" Blogging.BlogContext, MyAssembly" disableDatabaseInitialization="true" />
</contexts>
```  

<span data-ttu-id="76a95-178">Для задания пользовательского инициализатора можно использовать элемент **датабасеинитиализер** .</span><span class="sxs-lookup"><span data-stu-id="76a95-178">You can use the **databaseInitializer** element to set a custom initializer.</span></span>  

``` xml
<contexts>
  <context type=" Blogging.BlogContext, MyAssembly">
    <databaseInitializer type="Blogging.MyCustomBlogInitializer, MyAssembly" />
  </context>
</contexts>
```  

<span data-ttu-id="76a95-179">Параметры конструктора используют тот же синтаксис, что и фабрики соединений по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="76a95-179">Constructor parameters use the same syntax as default connection factories.</span></span>  

``` xml  
<contexts>
  <context type=" Blogging.BlogContext, MyAssembly">
    <databaseInitializer type="Blogging.MyCustomBlogInitializer, MyAssembly">
      <parameters>
        <parameter value="MyConstructorParameter" />
      </parameters>
    </databaseInitializer>
  </context>
</contexts>
```  

<span data-ttu-id="76a95-180">Можно настроить один из универсальных инициализаторов базы данных, которые включены в Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="76a95-180">You can configure one of the generic database initializers that are included in Entity Framework.</span></span> <span data-ttu-id="76a95-181">Атрибут **Type** использует формат .NET Framework для универсальных типов.</span><span class="sxs-lookup"><span data-stu-id="76a95-181">The **type** attribute uses the .NET Framework format for generic types.</span></span>  

<span data-ttu-id="76a95-182">Например, при использовании Code First migrations можно настроить автоматическую миграцию базы данных с помощью `MigrateDatabaseToLatestVersion<TContext, TMigrationsConfiguration>` инициализатора.</span><span class="sxs-lookup"><span data-stu-id="76a95-182">For example, if you are using Code First Migrations, you can configure the database to be migrated automatically using the `MigrateDatabaseToLatestVersion<TContext, TMigrationsConfiguration>` initializer.</span></span>  

``` xml
<contexts>
  <context type="Blogging.BlogContext, MyAssembly">
    <databaseInitializer type="System.Data.Entity.MigrateDatabaseToLatestVersion`2[[Blogging.BlogContext, MyAssembly], [Blogging.Migrations.Configuration, MyAssembly]], EntityFramework" />
  </context>
</contexts>
```
