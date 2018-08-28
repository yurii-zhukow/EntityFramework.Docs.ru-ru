---
title: Параметры файла конфигурации - EF6
author: divega
ms.date: 2016-10-23
ms.assetid: 000044c6-1d32-4cf7-ae1f-ea21d86ebf8f
ms.openlocfilehash: 88c2439f3a89c9fb9ee22f828789df4decf39cc5
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/27/2018
ms.locfileid: "42996507"
---
# <a name="configuration-file-settings"></a><span data-ttu-id="f7c48-102">Параметры файла конфигурации</span><span class="sxs-lookup"><span data-stu-id="f7c48-102">Configuration File Settings</span></span>
<span data-ttu-id="f7c48-103">Entity Framework дает ряд параметров, чтобы указать в файле конфигурации.</span><span class="sxs-lookup"><span data-stu-id="f7c48-103">Entity Framework allows a number of settings to be specified from the configuration file.</span></span> <span data-ttu-id="f7c48-104">В целом EF следует принципу «соглашение относительно настройки»: все параметры, описываемые в этой записи имеют поведение по умолчанию, необходимо только волноваться об изменении параметра, когда значение по умолчанию больше не удовлетворяет вашим требованиям.</span><span class="sxs-lookup"><span data-stu-id="f7c48-104">In general EF follows a ‘convention over configuration’ principle: all the settings discussed in this post have a default behavior, you only need to worry about changing the setting when the default no longer satisfies your requirements.</span></span>  

## <a name="a-code-based-alternative"></a><span data-ttu-id="f7c48-105">Это альтернативное решение на уровне кода</span><span class="sxs-lookup"><span data-stu-id="f7c48-105">A Code-Based Alternative</span></span>  

<span data-ttu-id="f7c48-106">Все эти параметры можно также применять, используя код.</span><span class="sxs-lookup"><span data-stu-id="f7c48-106">All of these settings can also be applied using code.</span></span> <span data-ttu-id="f7c48-107">Начиная с версии EF6, мы представили [конфигурация на основе кода](code-based.md), который предоставляет основные способы применения конфигурации из кода.</span><span class="sxs-lookup"><span data-stu-id="f7c48-107">Starting in EF6 we introduced [code-based configuration](code-based.md), which provides a central way of applying configuration from code.</span></span> <span data-ttu-id="f7c48-108">До EF6 конфигурация может применяться по-прежнему из кода, но вам нужно использовать различные API-интерфейсы для настройки различных областей.</span><span class="sxs-lookup"><span data-stu-id="f7c48-108">Prior to EF6, configuration can still be applied from code but you need to use various APIs to configure different areas.</span></span> <span data-ttu-id="f7c48-109">Параметр файла конфигурации позволяет легко изменять во время развертывания без обновления кода эти параметры.</span><span class="sxs-lookup"><span data-stu-id="f7c48-109">The configuration file option allows these settings to be easily changed during deployment without updating your code.</span></span>

## <a name="the-entity-framework-configuration-section"></a><span data-ttu-id="f7c48-110">Раздел конфигурации Entity Framework</span><span class="sxs-lookup"><span data-stu-id="f7c48-110">The Entity Framework Configuration Section</span></span>  

<span data-ttu-id="f7c48-111">Начиная с EF4.1 удалось задать инициализации базы данных для контекста с помощью **appSettings** раздел файла конфигурации.</span><span class="sxs-lookup"><span data-stu-id="f7c48-111">Starting with EF4.1 you could set the database initializer for a context using the **appSettings** section of the configuration file.</span></span> <span data-ttu-id="f7c48-112">В EF 4.3 мы представили пользовательский **entityFramework** разделе для обработки новых параметров.</span><span class="sxs-lookup"><span data-stu-id="f7c48-112">In EF 4.3 we introduced the custom **entityFramework** section to handle the new settings.</span></span> <span data-ttu-id="f7c48-113">Платформа Entity Framework по-прежнему будет распознавать инициализаторы базы данных, заданные с помощью старого формата, но мы рекомендуем перейти в новый формат, где это возможно.</span><span class="sxs-lookup"><span data-stu-id="f7c48-113">Entity Framework will still recognize database initializers set using the old format, but we recommend moving to the new format where possible.</span></span>

<span data-ttu-id="f7c48-114">**EntityFramework** раздел был автоматически добавлен в файл конфигурации проекта при установке пакета EntityFramework NuGet.</span><span class="sxs-lookup"><span data-stu-id="f7c48-114">The **entityFramework** section was automatically added to the configuration file of your project when you installed the EntityFramework NuGet package.</span></span>  

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

## <a name="connection-strings"></a><span data-ttu-id="f7c48-115">Строки подключения</span><span class="sxs-lookup"><span data-stu-id="f7c48-115">Connection Strings</span></span>  

<span data-ttu-id="f7c48-116">[Эта страница](~/ef6/fundamentals/configuring/connection-strings.md) Дополнительные сведения о как Entity Framework определяет базу данных для использования, включая строки подключения в файле конфигурации.</span><span class="sxs-lookup"><span data-stu-id="f7c48-116">[This page](~/ef6/fundamentals/configuring/connection-strings.md) provides more details on how Entity Framework determines the database to be used, including connection strings in the configuration file.</span></span>  

<span data-ttu-id="f7c48-117">Строки подключения перейдите в стандарте **connectionStrings** элемента и не требуют **entityFramework** раздел.</span><span class="sxs-lookup"><span data-stu-id="f7c48-117">Connection strings go in the standard **connectionStrings** element and do not require the **entityFramework** section.</span></span>  

<span data-ttu-id="f7c48-118">Модели кода, в первую очередь основано используйте обычные строки подключения ADO.NET.</span><span class="sxs-lookup"><span data-stu-id="f7c48-118">Code First based models use normal ADO.NET connection strings.</span></span> <span data-ttu-id="f7c48-119">Пример:</span><span class="sxs-lookup"><span data-stu-id="f7c48-119">For example:</span></span>  

``` xml
<connectionStrings>
  <add name="BlogContext"  
        providerName="System.Data.SqlClient"  
        connectionString="Server=.\SQLEXPRESS;Database=Blogging;Integrated Security=True;"/>
</connectionStrings>
```  

<span data-ttu-id="f7c48-120">Конструктор EF на основе строки подключения специальные EF моделей использования.</span><span class="sxs-lookup"><span data-stu-id="f7c48-120">EF Designer based models use special EF connection strings.</span></span> <span data-ttu-id="f7c48-121">Пример:</span><span class="sxs-lookup"><span data-stu-id="f7c48-121">For example:</span></span>  

``` xml  
<connectionStrings>
  <add name="BlogContext"  
    connectionString=
      "metadata=
        res://*/BloggingModel.csdl|
        res://*/BloggingModel.ssdl|
        res://*/BloggingModel.msl;
      provider=System.Data.SqlClient
      provider connection string=
        &quot;data source=(localdb)\mssqllocaldb;
        initial catalog=Blogging;
        integrated security=True;
        multipleactiveresultsets=True;&quot;"
     providerName="System.Data.EntityClient" />
</connectionStrings>
```

## <a name="code-based-configuration-type-ef6-onwards"></a><span data-ttu-id="f7c48-122">Тип конфигурации на основе кода (для EF6 и выше)</span><span class="sxs-lookup"><span data-stu-id="f7c48-122">Code-Based Configuration Type (EF6 Onwards)</span></span>  

<span data-ttu-id="f7c48-123">Начиная с EF6, можно указать DbConfiguration для Entity FRAMEWORK для [конфигурация на основе кода](code-based.md) в приложении.</span><span class="sxs-lookup"><span data-stu-id="f7c48-123">Starting with EF6, you can specify the DbConfiguration for EF to use for [code-based configuration](code-based.md) in your application.</span></span> <span data-ttu-id="f7c48-124">В большинстве случаев не нужно указать этот параметр, как EF автоматически обнаружит ваш DbConfiguration.</span><span class="sxs-lookup"><span data-stu-id="f7c48-124">In most cases you don't need to specify this setting as EF will automatically discover your DbConfiguration.</span></span> <span data-ttu-id="f7c48-125">Подробные сведения о при необходимо указать в файле config DbConfiguration **перемещение DbConfiguration** раздел [конфигурация на основе кода](code-based.md).</span><span class="sxs-lookup"><span data-stu-id="f7c48-125">For details of when you may need to specify DbConfiguration in your config file see the **Moving DbConfiguration** section of [Code-Based Configuration](code-based.md).</span></span>  

<span data-ttu-id="f7c48-126">Чтобы задать тип DbConfiguration, укажите имя типа с указанием сборки в **codeConfigurationType** элемент.</span><span class="sxs-lookup"><span data-stu-id="f7c48-126">To set a DbConfiguration type, you specify the assembly qualified type name in the **codeConfigurationType** element.</span></span>  

> [!NOTE]
> <span data-ttu-id="f7c48-127">Полное имя сборки — это полное имя пространства имен, используя запятую, затем сборку, тип находится в.</span><span class="sxs-lookup"><span data-stu-id="f7c48-127">An assembly qualified name is the namespace qualified name, followed by a comma, then the assembly that the type resides in.</span></span> <span data-ttu-id="f7c48-128">При необходимости вы можете также указать версию сборки, язык и региональные параметры и маркер открытого ключа.</span><span class="sxs-lookup"><span data-stu-id="f7c48-128">You can optionally also specify the assembly version, culture and public key token.</span></span>  

``` xml
<entityFramework codeConfigurationType="MyNamespace.MyConfiguration, MyAssembly">
</entityFramework>
```  

## <a name="ef-database-providers-ef6-onwards"></a><span data-ttu-id="f7c48-129">Поставщики EF баз данных (для EF6 и выше)</span><span class="sxs-lookup"><span data-stu-id="f7c48-129">EF Database Providers (EF6 Onwards)</span></span>  

<span data-ttu-id="f7c48-130">До EF6 приходилось включаемое как часть основной поставщик ADO.NET Entity Framework части поставщика базы данных.</span><span class="sxs-lookup"><span data-stu-id="f7c48-130">Prior to EF6, Entity Framework-specific parts of a database provider had to be included as part of the core ADO.NET provider.</span></span> <span data-ttu-id="f7c48-131">Начиная с EF6, конкретных частей EF теперь управляемых и зарегистрировать отдельно.</span><span class="sxs-lookup"><span data-stu-id="f7c48-131">Starting with EF6, the EF specific parts are now managed and registered separately.</span></span>  

<span data-ttu-id="f7c48-132">Обычно не нужно регистрировать поставщики самостоятельно.</span><span class="sxs-lookup"><span data-stu-id="f7c48-132">Normally you won't need to register providers yourself.</span></span> <span data-ttu-id="f7c48-133">Это обычно выполняется поставщиком при его установке.</span><span class="sxs-lookup"><span data-stu-id="f7c48-133">This will typically be done by the provider when you install it.</span></span>  

<span data-ttu-id="f7c48-134">Поставщики регистрируются, включив **поставщика** элемента под **поставщики** дочернего **entityFramework** раздел.</span><span class="sxs-lookup"><span data-stu-id="f7c48-134">Providers are registered by including a **provider** element under the **providers** child section of the **entityFramework** section.</span></span> <span data-ttu-id="f7c48-135">Существует два обязательных атрибута для запись поставщика:</span><span class="sxs-lookup"><span data-stu-id="f7c48-135">There are two required attributes for a provider entry:</span></span>  

- <span data-ttu-id="f7c48-136">**invariantName** определяет основной поставщик ADO.NET, целевые объекты поставщика EF</span><span class="sxs-lookup"><span data-stu-id="f7c48-136">**invariantName** identifies the core ADO.NET provider that this EF provider targets</span></span>  
- <span data-ttu-id="f7c48-137">**Тип** — это имя типа с указанием сборки для реализации поставщика EF</span><span class="sxs-lookup"><span data-stu-id="f7c48-137">**type** is the assembly qualified type name of the EF provider implementation</span></span>  

> [!NOTE]
> <span data-ttu-id="f7c48-138">Полное имя сборки — это полное имя пространства имен, используя запятую, затем сборку, тип находится в.</span><span class="sxs-lookup"><span data-stu-id="f7c48-138">An assembly qualified name is the namespace qualified name, followed by a comma, then the assembly that the type resides in.</span></span> <span data-ttu-id="f7c48-139">При необходимости вы можете также указать версию сборки, язык и региональные параметры и маркер открытого ключа.</span><span class="sxs-lookup"><span data-stu-id="f7c48-139">You can optionally also specify the assembly version, culture and public key token.</span></span>  

<span data-ttu-id="f7c48-140">В качестве примера ниже приведен записи, созданной для регистрации поставщика SQL Server по умолчанию при установке платформы Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="f7c48-140">As an example here is the entry created to register the default SQL Server provider when you install Entity Framework.</span></span>  

``` xml  
<providers>
  <provider invariantName="System.Data.SqlClient" type="System.Data.Entity.SqlServer.SqlProviderServices, EntityFramework.SqlServer" />
</providers>
```  

## <a name="interceptors-ef61-onwards"></a><span data-ttu-id="f7c48-141">Перехватчики (EF6.1 и более поздние версии)</span><span class="sxs-lookup"><span data-stu-id="f7c48-141">Interceptors (EF6.1 Onwards)</span></span>  

<span data-ttu-id="f7c48-142">Начиная с EF6.1 перехватчики можно зарегистрировать в файле конфигурации.</span><span class="sxs-lookup"><span data-stu-id="f7c48-142">Starting with EF6.1 you can register interceptors in the configuration file.</span></span> <span data-ttu-id="f7c48-143">Перехватчики позволяют запускать дополнительной логики, когда EF выполняет некоторые операции, такие как выполнение запросов к базе данных, открытия подключений и т. д.</span><span class="sxs-lookup"><span data-stu-id="f7c48-143">Interceptors allow you to run additional logic when EF performs certain operations, such as executing database queries, opening connections, etc.</span></span>  

<span data-ttu-id="f7c48-144">Перехватчики зарегистрированы, включая **перехватчик** элемента под **перехватчики** дочернего **entityFramework** раздел.</span><span class="sxs-lookup"><span data-stu-id="f7c48-144">Interceptors are registered by including an **interceptor** element under the **interceptors** child section of the **entityFramework** section.</span></span> <span data-ttu-id="f7c48-145">Например, следующая конфигурация регистрирует встроенной **DatabaseLogger** перехватчика, которые регистрируются все операции с базой данных в консоль.</span><span class="sxs-lookup"><span data-stu-id="f7c48-145">For example, the following configuration registers the built-in **DatabaseLogger** interceptor that will log all database operations to the Console.</span></span>  

``` xml  
<interceptors>
  <interceptor type="System.Data.Entity.Infrastructure.Interception.DatabaseLogger, EntityFramework"/>
</interceptors>
```  

### <a name="logging-database-operations-to-a-file-ef61-onwards"></a><span data-ttu-id="f7c48-146">Ведение журнала операций базы данных в файл (EF6.1 и более поздние версии)</span><span class="sxs-lookup"><span data-stu-id="f7c48-146">Logging Database Operations to a File (EF6.1 Onwards)</span></span>  

<span data-ttu-id="f7c48-147">Регистрация перехватчики через файл конфигурации особенно полезна при вы хотите добавить в существующее приложение для отладки проблемы ведения журнала.</span><span class="sxs-lookup"><span data-stu-id="f7c48-147">Registering interceptors via the config file is especially useful when you want to add logging to an existing application to help debug an issue.</span></span> <span data-ttu-id="f7c48-148">**DatabaseLogger** поддерживает ведение журнала в файл, указав имя файла в качестве параметра конструктора.</span><span class="sxs-lookup"><span data-stu-id="f7c48-148">**DatabaseLogger** supports logging to a file by supplying the file name as a constructor parameter.</span></span>  

``` xml  
<interceptors>
  <interceptor type="System.Data.Entity.Infrastructure.Interception.DatabaseLogger, EntityFramework">
    <parameters>
      <parameter value="C:\Temp\LogOutput.txt"/>
    </parameters>
  </interceptor>
</interceptors>
```  

<span data-ttu-id="f7c48-149">По умолчанию в результате файл журнала перезапись файла каждый раз, когда приложение запускается.</span><span class="sxs-lookup"><span data-stu-id="f7c48-149">By default this will cause the log file to be overwritten with a new file each time the app starts.</span></span> <span data-ttu-id="f7c48-150">Для вместо добавления в журнал файла, если он уже существует рекомендуется использовать примерно следующим образом:</span><span class="sxs-lookup"><span data-stu-id="f7c48-150">To instead append to the log file if it already exists use something like:</span></span>  

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

<span data-ttu-id="f7c48-151">Дополнительные сведения о **DatabaseLogger** и регистрации перехватчики, см. в записи блога [EF 6.1: Включение ведения журнала без повторной компиляции](https://blog.oneunicorn.com/2014/02/09/ef-6-1-turning-on-logging-without-recompiling/).</span><span class="sxs-lookup"><span data-stu-id="f7c48-151">For additional information on **DatabaseLogger** and registering interceptors, see the blog post [EF 6.1: Turning on logging without recompiling](https://blog.oneunicorn.com/2014/02/09/ef-6-1-turning-on-logging-without-recompiling/).</span></span>  

## <a name="code-first-default-connection-factory"></a><span data-ttu-id="f7c48-152">Фабрика соединений по умолчанию первый код</span><span class="sxs-lookup"><span data-stu-id="f7c48-152">Code First Default Connection Factory</span></span>  

<span data-ttu-id="f7c48-153">Раздел конфигурации позволяет указать фабрика соединений по умолчанию, который Code First следует использовать для поиска в базе данных для контекста.</span><span class="sxs-lookup"><span data-stu-id="f7c48-153">The configuration section allows you to specify a default connection factory that Code First should use to locate a database to use for a context.</span></span> <span data-ttu-id="f7c48-154">Фабрика соединений по умолчанию используется только в том случае, если строка подключения не был добавлен в файл конфигурации для контекста.</span><span class="sxs-lookup"><span data-stu-id="f7c48-154">The default connection factory is only used when no connection string has been added to the configuration file for a context.</span></span>  

<span data-ttu-id="f7c48-155">При установке пакета EF NuGet фабрика соединений по умолчанию был зарегистрирован, указывающий на SQL Express или LocalDB, в зависимости от того, какой из них установки.</span><span class="sxs-lookup"><span data-stu-id="f7c48-155">When you installed the EF NuGet package a default connection factory was registered that points to either SQL Express or LocalDB, depending on which one you have installed.</span></span>  

<span data-ttu-id="f7c48-156">Чтобы задать фабрику соединений, укажите имя типа с указанием сборки в **deafultConnectionFactory** элемент.</span><span class="sxs-lookup"><span data-stu-id="f7c48-156">To set a connection factory, you specify the assembly qualified type name in the **deafultConnectionFactory** element.</span></span>  

> [!NOTE]
> <span data-ttu-id="f7c48-157">Полное имя сборки — это полное имя пространства имен, используя запятую, затем сборку, тип находится в.</span><span class="sxs-lookup"><span data-stu-id="f7c48-157">An assembly qualified name is the namespace qualified name, followed by a comma, then the assembly that the type resides in.</span></span> <span data-ttu-id="f7c48-158">При необходимости вы можете также указать версию сборки, язык и региональные параметры и маркер открытого ключа.</span><span class="sxs-lookup"><span data-stu-id="f7c48-158">You can optionally also specify the assembly version, culture and public key token.</span></span>  

<span data-ttu-id="f7c48-159">Ниже приведен пример настройки собственной фабрики подключения по умолчанию:</span><span class="sxs-lookup"><span data-stu-id="f7c48-159">Here is an example of setting your own default connection factory:</span></span>  

``` xml  
<entityFramework>
  <defaultConnectionFactory type="MyNamespace.MyCustomFactory, MyAssembly"/>
</entityFramework>
```  

<span data-ttu-id="f7c48-160">Приведенный выше пример требует собственного производства, чтобы иметь конструктор без параметров.</span><span class="sxs-lookup"><span data-stu-id="f7c48-160">The above example requires the custom factory to have a parameterless constructor.</span></span> <span data-ttu-id="f7c48-161">При необходимости можно указать с помощью параметров конструктора **параметры** элемент.</span><span class="sxs-lookup"><span data-stu-id="f7c48-161">If needed, you can specify constructor parameters using the **parameters** element.</span></span>  

<span data-ttu-id="f7c48-162">Например SqlCeConnectionFactory, который включен в Entity Framework, необходимо предоставить неизменяемое имя поставщика в конструктор.</span><span class="sxs-lookup"><span data-stu-id="f7c48-162">For example, the SqlCeConnectionFactory, that is included in Entity Framework, requires you to supply a provider invariant name to the constructor.</span></span> <span data-ttu-id="f7c48-163">Неизменяемое имя поставщика определяет версию SQL Compact, вы хотите использовать.</span><span class="sxs-lookup"><span data-stu-id="f7c48-163">The provider invariant name identifies the version of SQL Compact you want to use.</span></span> <span data-ttu-id="f7c48-164">Следующая конфигурация вызовет контекстов для использования SQL Compact версии 4.0 по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="f7c48-164">The following configuration will cause contexts to use SQL Compact version 4.0 by default.</span></span>  

``` xml  
<entityFramework>
  <defaultConnectionFactory type="System.Data.Entity.Infrastructure.SqlCeConnectionFactory, EntityFramework">
    <parameters>
      <parameter value="System.Data.SqlServerCe.4.0" />
    </parameters>
  </defaultConnectionFactory>
</entityFramework>
```  

<span data-ttu-id="f7c48-165">Если фабрика соединений по умолчанию не задан, Code First использует SqlConnectionFactory, указывающий на `.\SQLEXPRESS`.</span><span class="sxs-lookup"><span data-stu-id="f7c48-165">If you don’t set a default connection factory, Code First uses the SqlConnectionFactory, pointing to `.\SQLEXPRESS`.</span></span> <span data-ttu-id="f7c48-166">SqlConnectionFactory также имеет конструктор, который позволяет переопределить части строки подключения.</span><span class="sxs-lookup"><span data-stu-id="f7c48-166">SqlConnectionFactory also has a constructor that allows you to override parts of the connection string.</span></span> <span data-ttu-id="f7c48-167">Если вы хотите использовать экземпляр SQL Server, отличное от `.\SQLEXPRESS` этот конструктор можно использовать для настройки сервера.</span><span class="sxs-lookup"><span data-stu-id="f7c48-167">If you want to use a SQL Server instance other than `.\SQLEXPRESS` you can use this constructor to set the server.</span></span>  

<span data-ttu-id="f7c48-168">Следующая конфигурация приведет к Code First для использования **Сервер_базы_данных** для контекстов, не имеющие явную строку подключения значение.</span><span class="sxs-lookup"><span data-stu-id="f7c48-168">The following configuration will cause Code First to use **MyDatabaseServer** for contexts that don’t have an explicit connection string set.</span></span>  

``` xml  
<entityFramework>
  <defaultConnectionFactory type="System.Data.Entity.Infrastructure.SqlConnectionFactory, EntityFramework">
    <parameters>
      <parameter value="Data Source=MyDatabaseServer; Integrated Security=True; MultipleActiveResultSets=True" />
    </parameters>
  </defaultConnectionFactory>
</entityFramework>
```  

<span data-ttu-id="f7c48-169">По умолчанию предполагается, что аргументы конструктора являются типом string.</span><span class="sxs-lookup"><span data-stu-id="f7c48-169">By default, it’s assumed that constructor arguments are of type string.</span></span> <span data-ttu-id="f7c48-170">Чтобы изменить этот параметр можно использовать атрибут type.</span><span class="sxs-lookup"><span data-stu-id="f7c48-170">You can use the type attribute to change this.</span></span>  

``` xml
<parameter value="2" type="System.Int32" />
```  

## <a name="database-initializers"></a><span data-ttu-id="f7c48-171">Инициализаторы базы данных</span><span class="sxs-lookup"><span data-stu-id="f7c48-171">Database Initializers</span></span>  

<span data-ttu-id="f7c48-172">Инициализаторы базы данных настраиваются на основе каждого контекста.</span><span class="sxs-lookup"><span data-stu-id="f7c48-172">Database initializers are configured on a per-context basis.</span></span> <span data-ttu-id="f7c48-173">Их можно задать в файле конфигурации с помощью **контекст** элемент.</span><span class="sxs-lookup"><span data-stu-id="f7c48-173">They can be set in the configuration file using the **context** element.</span></span> <span data-ttu-id="f7c48-174">Этот элемент используется полное имя сборки для определения контекста выполняется настройка.</span><span class="sxs-lookup"><span data-stu-id="f7c48-174">This element uses the assembly qualified name to identify the context being configured.</span></span>  

<span data-ttu-id="f7c48-175">По умолчанию Code First контекстов настроены на использование инициализатора CreateDatabaseIfNotExists.</span><span class="sxs-lookup"><span data-stu-id="f7c48-175">By default, Code First contexts are configured to use the CreateDatabaseIfNotExists initializer.</span></span> <span data-ttu-id="f7c48-176">Существует **disableDatabaseInitialization** атрибут **контекст** элемент, который может использоваться для отключения инициализация базы данных.</span><span class="sxs-lookup"><span data-stu-id="f7c48-176">There is a **disableDatabaseInitialization** attribute on the **context** element that can be used to disable database initialization.</span></span>  

<span data-ttu-id="f7c48-177">Например следующая конфигурация отключает инициализацию базы данных для Blogging.BlogContext контексте, определенном в файле MyAssembly.dll.</span><span class="sxs-lookup"><span data-stu-id="f7c48-177">For example, the following configuration disables database initialization for the Blogging.BlogContext context defined in MyAssembly.dll.</span></span>  

``` xml  
<contexts>
  <context type=" Blogging.BlogContext, MyAssembly" disableDatabaseInitialization="true" />
</contexts>
```  

<span data-ttu-id="f7c48-178">Можно использовать **databaseInitializer** элемент для задания настраиваемого инициализатора.</span><span class="sxs-lookup"><span data-stu-id="f7c48-178">You can use the **databaseInitializer** element to set a custom initializer.</span></span>  

``` xml
<contexts>
  <context type=" Blogging.BlogContext, MyAssembly">
    <databaseInitializer type="Blogging.MyCustomBlogInitializer, MyAssembly" />
  </context>
</contexts>
```  

<span data-ttu-id="f7c48-179">Параметры конструктора используется тот же синтаксис, как фабрики подключения по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="f7c48-179">Constructor parameters use the same syntax as default connection factories.</span></span>  

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

<span data-ttu-id="f7c48-180">Можно настроить один инициализаторов универсальную базу данных, включенных в Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="f7c48-180">You can configure one of the generic database initializers that are included in Entity Framework.</span></span> <span data-ttu-id="f7c48-181">**Тип** формата .NET Framework использует атрибут для универсальных типов.</span><span class="sxs-lookup"><span data-stu-id="f7c48-181">The **type** attribute uses the .NET Framework format for generic types.</span></span>  

<span data-ttu-id="f7c48-182">Например, при использовании Code First Migrations, можно настроить базу данных для миграции автоматически с помощью `MigrateDatabaseToLatestVersion<TContext, TMigrationsConfiguration>` инициализатора.</span><span class="sxs-lookup"><span data-stu-id="f7c48-182">For example, if you are using Code First Migrations, you can configure the database to be migrated automatically using the `MigrateDatabaseToLatestVersion<TContext, TMigrationsConfiguration>` initializer.</span></span>  

``` xml
<contexts>
  <context type="Blogging.BlogContext, MyAssembly">
    <databaseInitializer type="System.Data.Entity.MigrateDatabaseToLatestVersion`2[[Blogging.BlogContext, MyAssembly], [Blogging.Migrations.Configuration, MyAssembly]], EntityFramework" />
  </context>
</contexts>
```
