---
title: Установка Entity Framework Core
author: divega
ms.date: 08/06/2017
ms.assetid: 608cc774-c570-4809-8a3e-cd2c8446b8b2
uid: core/get-started/install/index
ms.openlocfilehash: 7831e6a54e885cf0b89ef3eef2cd81a9292df606
ms.sourcegitcommit: 0d36e8ff0892b7f034b765b15e041f375f88579a
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/09/2018
ms.locfileid: "44250326"
---
# <a name="installing-entity-framework-core"></a><span data-ttu-id="cc801-102">Установка Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="cc801-102">Installing Entity Framework Core</span></span>

## <a name="prerequisites"></a><span data-ttu-id="cc801-103">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="cc801-103">Prerequisites</span></span>

* <span data-ttu-id="cc801-104">Для разработки приложений, предназначенных для .NET Core 2.1, установите [пакет SDK для .NET Core 2.1](https://www.microsoft.com/net/download/core).</span><span class="sxs-lookup"><span data-stu-id="cc801-104">To develop apps that target .NET Core 2.1, install [the .NET Core 2.1 SDK](https://www.microsoft.com/net/download/core).</span></span> <span data-ttu-id="cc801-105">Это нужно сделать, даже если у вас установлена последняя версия Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="cc801-105">The SDK has to be installed even if you have the latest version of Visual Studio 2017.</span></span>

* <span data-ttu-id="cc801-106">Для разработки в Visual Studio приложений, предназначенных для .NET Core 2.1, установите Visual Studio 2017 версии 15.7 или новее.</span><span class="sxs-lookup"><span data-stu-id="cc801-106">To use Visual Studio for development of apps that target .NET Core 2.1, install Visual Studio 2017 version 15.7 or later.</span></span>

* <span data-ttu-id="cc801-107">Использовать Entity Framework 2.1 можно в приложениях ASP.NET Core версии 2.1.</span><span class="sxs-lookup"><span data-stu-id="cc801-107">To use Entity Framework 2.1 in ASP.NET Core applications, use ASP.NET Core 2.1.</span></span> <span data-ttu-id="cc801-108">Приложения с более ранними версиями ASP.NET Core нужно обновить до версии 2.1.</span><span class="sxs-lookup"><span data-stu-id="cc801-108">Applications that use earlier versions of ASP.NET Core must be updated to 2.1.</span></span>

* <span data-ttu-id="cc801-109">Для разработки приложений, предназначенных для .NET Framework 4.6.1 и более поздних версий, можно использовать Visual Studio 2015.</span><span class="sxs-lookup"><span data-stu-id="cc801-109">You can use Visual Studio 2015 for apps that target the .NET Framework 4.6.1 or later.</span></span> <span data-ttu-id="cc801-110">Однако вам нужна версия NuGet, поддерживающая .NET Standard 2.0 и совместимые среды.</span><span class="sxs-lookup"><span data-stu-id="cc801-110">But you need a version of NuGet that is aware of the .NET Standard 2.0 and its compatible frameworks.</span></span> <span data-ttu-id="cc801-111">Чтобы получить ее в Visual Studio 2015, [обновите клиент NuGet до версии 3.6.0](https://www.nuget.org/downloads).</span><span class="sxs-lookup"><span data-stu-id="cc801-111">To get that in Visual Studio 2015, [upgrade the NuGet client to version 3.6.0](https://www.nuget.org/downloads).</span></span>

## <a name="get-the-entity-framework-core-runtime"></a><span data-ttu-id="cc801-112">Получение среды выполнения Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="cc801-112">Get the Entity Framework Core runtime</span></span>

<span data-ttu-id="cc801-113">Чтобы добавить в приложение библиотеки среды выполнения EF Core, установите пакет NuGet для поставщика базы данных, который вы хотите использовать.</span><span class="sxs-lookup"><span data-stu-id="cc801-113">To add EF Core runtime libraries to an application, install the NuGet package for the database provider you want to use.</span></span> <span data-ttu-id="cc801-114">Список поддерживаемых поставщиков и имена их пакетов NuGet см. в разделе [Поставщики баз данных](../../providers/index.md).</span><span class="sxs-lookup"><span data-stu-id="cc801-114">For a list of supported providers and their NuGet package names, see [Database providers](../../providers/index.md).</span></span>

<span data-ttu-id="cc801-115">Установить или обновить пакеты NuGet можно в интерфейсе командной строки .NET Core, диалоговом окне диспетчера пакетов Visual Studio или консоли диспетчера пакетов Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="cc801-115">To install or update NuGet packages, use the .NET Core CLI, the Visual Studio Package Manager Dialog, or the Visual Studio Package Manager Console.</span></span>

<span data-ttu-id="cc801-116">Для приложений ASP.NET Core 2.1 поставщики в памяти и поставщики SQL Server включены автоматически и устанавливать их отдельно не требуется.</span><span class="sxs-lookup"><span data-stu-id="cc801-116">For ASP.NET Core 2.1 applications, the in-memory and SQL Server providers are automatically included, so there's no need to install them separately.</span></span>

> [!TIP]  
> <span data-ttu-id="cc801-117">Если требуется обновить приложение, использующее сторонний поставщик базы данных, всегда ищите обновление поставщика, совместимое с нужной вам версией EF Core.</span><span class="sxs-lookup"><span data-stu-id="cc801-117">If you need to update an application that is using a third-party database provider, always check for an update of the provider that is compatible with the version of EF Core you want to use.</span></span> <span data-ttu-id="cc801-118">Например, поставщики баз данных для предыдущих версий несовместимы с версией 2.1 среды выполнения EF Core.</span><span class="sxs-lookup"><span data-stu-id="cc801-118">For example, database providers for previous versions are not compatible with version 2.1 of the EF Core runtime.</span></span>  

### <a name="net-core-cli"></a><span data-ttu-id="cc801-119">Интерфейс командной строки .NET Core</span><span class="sxs-lookup"><span data-stu-id="cc801-119">.NET Core CLI</span></span>

<span data-ttu-id="cc801-120">Следующая команда интерфейса командной строки .NET Core устанавливает или обновляет поставщик SQL Server:</span><span class="sxs-lookup"><span data-stu-id="cc801-120">The following .NET Core CLI command installs or updates the SQL Server provider:</span></span>

``` Console
dotnet add package Microsoft.EntityFrameworkCore.SqlServer
```

<span data-ttu-id="cc801-121">Вы можете указать в команде `dotnet add package` конкретную версию, используя модификатор `-v`.</span><span class="sxs-lookup"><span data-stu-id="cc801-121">You can indicate a specific version in the `dotnet add package` command, using the `-v` modifier.</span></span> <span data-ttu-id="cc801-122">Например, чтобы установить пакеты EF Core 2.1.0, добавьте к команде `-v 2.1.0`.</span><span class="sxs-lookup"><span data-stu-id="cc801-122">For example, to install EF Core 2.1.0 packages, append `-v 2.1.0` to the command.</span></span>

### <a name="visual-studio-nuget-package-manager-dialog"></a><span data-ttu-id="cc801-123">Диалоговое окно диспетчера пакетов NuGet в Visual Studio</span><span class="sxs-lookup"><span data-stu-id="cc801-123">Visual Studio NuGet Package Manager Dialog</span></span>

* <span data-ttu-id="cc801-124">В меню выберите **Проект > Управление пакетами NuGet**</span><span class="sxs-lookup"><span data-stu-id="cc801-124">From the menu, select **Project > Manage NuGet Packages**</span></span>

* <span data-ttu-id="cc801-125">Нажмите кнопку **Обзор** или откройте вкладку **Обновления**.</span><span class="sxs-lookup"><span data-stu-id="cc801-125">Click on the **Browse** or the **Updates** tab</span></span>

* <span data-ttu-id="cc801-126">Чтобы установить или обновить поставщик SQL Server, выберите пакет `Microsoft.EntityFrameworkCore.SqlServer` и подтвердите свой выбор.</span><span class="sxs-lookup"><span data-stu-id="cc801-126">To install or update the SQL Server provider, select the `Microsoft.EntityFrameworkCore.SqlServer` package, and confirm.</span></span>

<span data-ttu-id="cc801-127">Дополнительные сведения: [Диалоговое окно диспетчера пакетов NuGet](https://docs.microsoft.com/nuget/tools/package-manager-ui).</span><span class="sxs-lookup"><span data-stu-id="cc801-127">For more information, see [NuGet Package Manager Dialog](https://docs.microsoft.com/nuget/tools/package-manager-ui).</span></span>

### <a name="visual-studio-nuget-package-manager-console"></a><span data-ttu-id="cc801-128">Консоль диспетчера пакетов NuGet в Visual Studio</span><span class="sxs-lookup"><span data-stu-id="cc801-128">Visual Studio NuGet Package Manager Console</span></span>

* <span data-ttu-id="cc801-129">В меню выберите **Сервис > Диспетчер пакетов NuGet > Консоль диспетчера пакетов**.</span><span class="sxs-lookup"><span data-stu-id="cc801-129">From the menu, select **Tools > NuGet Package Manager > Package Manager Console**</span></span>

* <span data-ttu-id="cc801-130">Чтобы установить поставщик SQL Server, в консоли диспетчера пакетов выполните следующую команду:</span><span class="sxs-lookup"><span data-stu-id="cc801-130">To install the SQL Server provider, run the following command in the Package Manager Console:</span></span>

  ``` PowerShell  
  Install-Package Microsoft.EntityFrameworkCore.SqlServer
  ```
* <span data-ttu-id="cc801-131">Чтобы обновить поставщик, используйте команду `Update-Package`.</span><span class="sxs-lookup"><span data-stu-id="cc801-131">To update the provider, use the `Update-Package` command.</span></span>

* <span data-ttu-id="cc801-132">Чтобы указать конкретную версию, используйте модификатор `-Version`.</span><span class="sxs-lookup"><span data-stu-id="cc801-132">To specify a specific version, use the `-Version` modifier.</span></span> <span data-ttu-id="cc801-133">Например, чтобы установить пакеты EF Core 2.1.0, добавьте в команды `-Version 2.1.0`</span><span class="sxs-lookup"><span data-stu-id="cc801-133">For example, to install EF Core 2.1.0 packages, append `-Version 2.1.0` to the commands</span></span>

<span data-ttu-id="cc801-134">Дополнительные сведения: [Консоль диспетчера пакетов](https://docs.microsoft.com/nuget/tools/package-manager-console).</span><span class="sxs-lookup"><span data-stu-id="cc801-134">For more information, see [Package Manager Console](https://docs.microsoft.com/nuget/tools/package-manager-console).</span></span>

## <a name="get-entity-framework-core-tools"></a><span data-ttu-id="cc801-135">Получение инструментов Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="cc801-135">Get Entity Framework Core tools</span></span>

<span data-ttu-id="cc801-136">Помимо библиотек среды выполнения, вы можете установить инструменты для выполнения в проекте определенных задач, связанных с EF Core, во время разработки.</span><span class="sxs-lookup"><span data-stu-id="cc801-136">Besides the runtime libraries, you can install tools that can perform some EF Core-related tasks in your project at design time.</span></span> <span data-ttu-id="cc801-137">Например, вы можете создавать миграции, выполнять их и создавать модель на основе существующей базы данных.</span><span class="sxs-lookup"><span data-stu-id="cc801-137">For example, you can create migrations, apply migrations, and create a model based on an existing database.</span></span>

<span data-ttu-id="cc801-138">Доступно два набора инструментов:</span><span class="sxs-lookup"><span data-stu-id="cc801-138">Two sets of tools are available:</span></span>
* <span data-ttu-id="cc801-139">[Инструменты интерфейса командной строки (CLI)](../../miscellaneous/cli/dotnet.md) .NET Core можно использовать в Windows, Linux и macOS.</span><span class="sxs-lookup"><span data-stu-id="cc801-139">The .NET Core [command-line interface (CLI) tools](../../miscellaneous/cli/dotnet.md) can be used on Windows, Linux, or macOS.</span></span> <span data-ttu-id="cc801-140">Эти команды начинаются с `dotnet ef`.</span><span class="sxs-lookup"><span data-stu-id="cc801-140">These commands begin with `dotnet ef`.</span></span> 
* <span data-ttu-id="cc801-141">[Инструменты консоли диспетчера пакетов](../../miscellaneous/cli/powershell.md) работают в Visual Studio 2017 на Windows.</span><span class="sxs-lookup"><span data-stu-id="cc801-141">The [Package Manager Console tools](../../miscellaneous/cli/powershell.md)  run in Visual Studio 2017 on Windows.</span></span> <span data-ttu-id="cc801-142">Эти команды начинаются с глагола, например `Add-Migration`, `Update-Database`.</span><span class="sxs-lookup"><span data-stu-id="cc801-142">These commands start with a verb, for example `Add-Migration`, `Update-Database`.</span></span>

<span data-ttu-id="cc801-143">Хотя вы можете использовать команды `dotnet ef` в консоли диспетчера пакетов, в Visual Studio удобнее использовать инструменты консоли диспетчера пакетов:</span><span class="sxs-lookup"><span data-stu-id="cc801-143">Although you can use the `dotnet ef` commands from the Package Manager Console, it's more convenient to use the Package Manager Console tools when you're using Visual Studio:</span></span>
* <span data-ttu-id="cc801-144">Они автоматически работают в текущем проекте, выбранном в консоли диспетчера пакетов, без необходимости ручного переключения каталогов.</span><span class="sxs-lookup"><span data-stu-id="cc801-144">They automatically work with the current project selected in the Package Manager Console without requiring manually switching directories.</span></span>  
* <span data-ttu-id="cc801-145">Они автоматически открывают файлы, созданные командами в Visual Studio, после завершения соответствующей команды.</span><span class="sxs-lookup"><span data-stu-id="cc801-145">They automatically open files generated by the commands in Visual Studio after the command is completed.</span></span>

<a name="cli"></a>

### <a name="get-the-cli-tools"></a><span data-ttu-id="cc801-146">Получение инструментов интерфейса командной строки</span><span class="sxs-lookup"><span data-stu-id="cc801-146">Get the CLI tools</span></span>

<span data-ttu-id="cc801-147">Команды `dotnet ef` входят в пакет SDK для .NET Core, но чтобы включить их, нужно установить пакет `Microsoft.EntityFrameworkCore.Design`:</span><span class="sxs-lookup"><span data-stu-id="cc801-147">The `dotnet ef` commands are included in the .NET Core SDK, but to enable the commands you have to install the `Microsoft.EntityFrameworkCore.Design` package:</span></span>

 ``` Console    
dotnet add package Microsoft.EntityFrameworkCore.Design 
``` 

<span data-ttu-id="cc801-148">Этот пакет автоматически включается в приложения ASP.NET Core 2.1.</span><span class="sxs-lookup"><span data-stu-id="cc801-148">For ASP.NET Core 2.1 apps, this package is included automatically.</span></span>

<span data-ttu-id="cc801-149">Как говорилось ранее в разделе [Предварительные требования](#prerequisites), вам также нужно установить пакет SDK для .NET Core 2.1.</span><span class="sxs-lookup"><span data-stu-id="cc801-149">As explained earlier in [Prerequisites](#prerequisites), you also need to install the .NET Core 2.1 SDK.</span></span>

> [!IMPORTANT]      
> <span data-ttu-id="cc801-150">Всегда используйте версии пакетов инструментов, которые соответствуют основному номеру версии для пакетов среды выполнения.</span><span class="sxs-lookup"><span data-stu-id="cc801-150">Always use the version of the tools package that matches the major version of the runtime packages.</span></span>

### <a name="get-the-package-manager-console-tools"></a><span data-ttu-id="cc801-151">Получение инструментов консоли диспетчера пакетов</span><span class="sxs-lookup"><span data-stu-id="cc801-151">Get the Package Manager Console tools</span></span>

<span data-ttu-id="cc801-152">Чтобы получить инструменты консоли диспетчера пакетов для EF Core, установите пакет `Microsoft.EntityFrameworkCore.Tools`:</span><span class="sxs-lookup"><span data-stu-id="cc801-152">To get the Package Manager Console tools for EF Core, install the `Microsoft.EntityFrameworkCore.Tools` package:</span></span>

 ``` Console    
dotnet add package Microsoft.EntityFrameworkCore.Tools
``` 

<span data-ttu-id="cc801-153">Этот пакет автоматически включается в приложения ASP.NET Core 2.1.</span><span class="sxs-lookup"><span data-stu-id="cc801-153">For ASP.NET Core 2.1 apps, this package is included automatically.</span></span>

## <a name="upgrading-to-ef-core-21"></a><span data-ttu-id="cc801-154">Обновление до EF Core 2.1</span><span class="sxs-lookup"><span data-stu-id="cc801-154">Upgrading to EF Core 2.1</span></span>

<span data-ttu-id="cc801-155">При обновлении существующего приложения до EF Core 2.1 некоторые ссылки на более старые пакеты EF Core, возможно, потребуется удалить вручную:</span><span class="sxs-lookup"><span data-stu-id="cc801-155">If you're upgrading an existing application to EF Core 2.1, some references to older EF Core packages may need to be removed manually:</span></span>

* <span data-ttu-id="cc801-156">Пакеты поставщиков баз данных, используемые во время разработки, например `Microsoft.EntityFrameworkCore.SqlServer.Design`, больше не требуются и не поддерживаются в EF Core 2.1, но они не удаляются автоматически при обновлении других пакетов.</span><span class="sxs-lookup"><span data-stu-id="cc801-156">Database provider design-time packages such as `Microsoft.EntityFrameworkCore.SqlServer.Design` are no longer required or supported in EF Core 2.1, but aren't automatically removed when upgrading the other packages.</span></span>

* <span data-ttu-id="cc801-157">Средства .NET CLI теперь включены в пакет SDK для .NET, следовательно ссылку на этот пакет можно удалить из файла *.csproj*:</span><span class="sxs-lookup"><span data-stu-id="cc801-157">The .NET CLI tools are now included in the .NET SDK, so the reference to that package can be removed from the *.csproj* file:</span></span>

  ```
  <DotNetCliToolReference Include="Microsoft.EntityFrameworkCore.Tools.DotNet" Version="2.0.0" />
  ```

<span data-ttu-id="cc801-158">Убедитесь, что приложения, предназначенные для .NET Framework и созданные в более ранних версиях Visual Studio, совместимы с библиотеками .NET Standard 2.0:</span><span class="sxs-lookup"><span data-stu-id="cc801-158">For applications that target the .NET Framework and were created by earlier versions of Visual Studio, make sure that they are compatible with .NET Standard 2.0 libraries:</span></span>

  * <span data-ttu-id="cc801-159">Измените файл проекта и включите в группу начальной свойств следующую запись.</span><span class="sxs-lookup"><span data-stu-id="cc801-159">Edit the project file and make sure the following entry appears in the initial property group:</span></span>

    ``` xml
    <AutoGenerateBindingRedirects>true</AutoGenerateBindingRedirects>
    ```

  * <span data-ttu-id="cc801-160">Для тестовых проектов также включите следующую запись.</span><span class="sxs-lookup"><span data-stu-id="cc801-160">For test projects, also make sure the following entry is present:</span></span>

    ``` xml
    <GenerateBindingRedirectsOutputType>true</GenerateBindingRedirectsOutputType>
    ```
