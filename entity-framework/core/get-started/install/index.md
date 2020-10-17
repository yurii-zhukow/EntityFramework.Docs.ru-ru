---
title: Установка Entity Framework Core — EF Core
description: Инструкции по установке для Entity Framework Core
author: bricelam
ms.date: 08/06/2017
uid: core/get-started/install/index
ms.openlocfilehash: 3aae80998768d8d1bfbad7a872abc3648b792ad5
ms.sourcegitcommit: 0a25c03fa65ae6e0e0e3f66bac48d59eceb96a5a
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/14/2020
ms.locfileid: "92062104"
---
# <a name="installing-entity-framework-core"></a><span data-ttu-id="d40e4-103">Установка Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="d40e4-103">Installing Entity Framework Core</span></span>

## <a name="prerequisites"></a><span data-ttu-id="d40e4-104">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="d40e4-104">Prerequisites</span></span>

* <span data-ttu-id="d40e4-105">EF Core — это библиотека [.NET Standard 2.0](/dotnet/standard/net-standard).</span><span class="sxs-lookup"><span data-stu-id="d40e4-105">EF Core is a [.NET Standard 2.0](/dotnet/standard/net-standard) library.</span></span> <span data-ttu-id="d40e4-106">Для запуска EF Core требуется реализация .NET, которая поддерживает .NET Standard 2.0.</span><span class="sxs-lookup"><span data-stu-id="d40e4-106">So EF Core requires a .NET implementation that supports .NET Standard 2.0 to run.</span></span> <span data-ttu-id="d40e4-107">На EF Core также могут ссылаться другие библиотеки .NET Standard 2.0.</span><span class="sxs-lookup"><span data-stu-id="d40e4-107">EF Core can also be referenced by other .NET Standard 2.0 libraries.</span></span>

* <span data-ttu-id="d40e4-108">Например, EF Core можно использовать для разработки приложений, предназначенных для .NET Core.</span><span class="sxs-lookup"><span data-stu-id="d40e4-108">For example, you can use EF Core to develop apps that target .NET Core.</span></span> <span data-ttu-id="d40e4-109">Для создания приложений .NET Core требуется [пакет SDK для .NET Core](https://dotnet.microsoft.com/download).</span><span class="sxs-lookup"><span data-stu-id="d40e4-109">Building .NET Core apps requires the [.NET Core SDK](https://dotnet.microsoft.com/download).</span></span> <span data-ttu-id="d40e4-110">При необходимости также можно использовать среду разработки, например [Visual Studio](https://visualstudio.microsoft.com/vs), [Visual Studio для Mac](https://visualstudio.microsoft.com/vs/mac) или [Visual Studio Code](https://code.visualstudio.com).</span><span class="sxs-lookup"><span data-stu-id="d40e4-110">Optionally, you can also use a development environment like [Visual Studio](https://visualstudio.microsoft.com/vs), [Visual Studio for Mac](https://visualstudio.microsoft.com/vs/mac), or [Visual Studio Code](https://code.visualstudio.com).</span></span> <span data-ttu-id="d40e4-111">Дополнительные сведения см. в разделе [Приступая к работе с .NET Core](/dotnet/core/get-started).</span><span class="sxs-lookup"><span data-stu-id="d40e4-111">For more information, check [Getting Started with .NET Core](/dotnet/core/get-started).</span></span>

* <span data-ttu-id="d40e4-112">EF Core можно использовать для разработки приложений в Windows с использованием Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="d40e4-112">You can use EF Core to develop applications on Windows using Visual Studio.</span></span> <span data-ttu-id="d40e4-113">Рекомендуется использовать последнюю версию [Visual Studio](https://visualstudio.microsoft.com/vs).</span><span class="sxs-lookup"><span data-stu-id="d40e4-113">The latest version of [Visual Studio](https://visualstudio.microsoft.com/vs) is recommended.</span></span>

* <span data-ttu-id="d40e4-114">EF Core можно запустить в других реализациях .NET, таких как [Xamarin](https://dotnet.microsoft.com/apps/xamarin) и .NET Native.</span><span class="sxs-lookup"><span data-stu-id="d40e4-114">EF Core can run on other .NET implementations like [Xamarin](https://dotnet.microsoft.com/apps/xamarin) and .NET Native.</span></span> <span data-ttu-id="d40e4-115">Но на практике у этих реализаций есть ограничения среды выполнения, которые могут повлиять на то, насколько хорошо EF Core работает в приложении.</span><span class="sxs-lookup"><span data-stu-id="d40e4-115">But in practice those implementations have runtime limitations that may affect how well EF Core works on your app.</span></span> <span data-ttu-id="d40e4-116">Дополнительные сведения см. в разделе [Реализации .NET, поддерживаемые EF Core](xref:core/platforms/index).</span><span class="sxs-lookup"><span data-stu-id="d40e4-116">For more information, see [.NET implementations supported by EF Core](xref:core/platforms/index).</span></span>

* <span data-ttu-id="d40e4-117">Наконец, для различных поставщиков баз данных требуются различные версии ядер СУБД, реализаций .NET и операционных систем.</span><span class="sxs-lookup"><span data-stu-id="d40e4-117">Finally, different database providers may require specific database engine versions, .NET implementations, or operating systems.</span></span> <span data-ttu-id="d40e4-118">Убедитесь, что доступен [поставщик базы данных EF Core](xref:core/providers/index), поддерживающий подходящую среду для вашего приложения.</span><span class="sxs-lookup"><span data-stu-id="d40e4-118">Make sure an [EF Core database provider](xref:core/providers/index) is available that supports the right environment for your application.</span></span>

## <a name="get-the-entity-framework-core-runtime"></a><span data-ttu-id="d40e4-119">Получение среды выполнения Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="d40e4-119">Get the Entity Framework Core runtime</span></span>

<span data-ttu-id="d40e4-120">Чтобы добавить EF Core в приложение, установите пакет NuGet для поставщика базы данных, который вы хотите использовать.</span><span class="sxs-lookup"><span data-stu-id="d40e4-120">To add EF Core to an application, install the NuGet package for the database provider you want to use.</span></span>

<span data-ttu-id="d40e4-121">Если вы создаете приложение ASP.NET Core, устанавливать поставщики в памяти и поставщики SQL Server не нужно.</span><span class="sxs-lookup"><span data-stu-id="d40e4-121">If you're building an ASP.NET Core application, you don't need to install the in-memory and SQL Server providers.</span></span> <span data-ttu-id="d40e4-122">Эти поставщики включены в текущие версии ASP.NET Core, а также в среду выполнения EF Core.</span><span class="sxs-lookup"><span data-stu-id="d40e4-122">Those providers are included in current versions of ASP.NET Core, alongside the EF Core runtime.</span></span>

<span data-ttu-id="d40e4-123">Установить или обновить пакеты NuGet можно в интерфейсе командной строки (CLI) .NET Core, а также в диалоговом окне или консоли диспетчера пакетов Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="d40e4-123">To install or update NuGet packages, you can use the .NET Core command-line interface (CLI), the Visual Studio Package Manager Dialog, or the Visual Studio Package Manager Console.</span></span>

### <a name="net-core-cli"></a><span data-ttu-id="d40e4-124">Интерфейс командной строки .NET Core</span><span class="sxs-lookup"><span data-stu-id="d40e4-124">.NET Core CLI</span></span>

* <span data-ttu-id="d40e4-125">Чтобы установить или обновить поставщик EF Core SQL Server, выполните следующую команду интерфейса командной строки .NET Core в командной строке операционной системы:</span><span class="sxs-lookup"><span data-stu-id="d40e4-125">Use the following .NET Core CLI command from the operating system's command line to install or update the EF Core SQL Server provider:</span></span>

  ```dotnetcli
  dotnet add package Microsoft.EntityFrameworkCore.SqlServer
  ```

* <span data-ttu-id="d40e4-126">Вы можете указать в команде `dotnet add package` конкретную версию, используя модификатор `-v`.</span><span class="sxs-lookup"><span data-stu-id="d40e4-126">You can indicate a specific version in the `dotnet add package` command, using the `-v` modifier.</span></span> <span data-ttu-id="d40e4-127">Например, чтобы установить пакеты EF Core 2.2.0, добавьте к команде `-v 2.2.0`.</span><span class="sxs-lookup"><span data-stu-id="d40e4-127">For example, to install EF Core 2.2.0 packages, append `-v 2.2.0` to the command.</span></span>

<span data-ttu-id="d40e4-128">Дополнительные сведения см. в разделе [Средства интерфейса командной строки (CLI) .NET](/dotnet/core/tools/).</span><span class="sxs-lookup"><span data-stu-id="d40e4-128">For more information, see [.NET command-line interface (CLI) tools](/dotnet/core/tools/).</span></span>

### <a name="visual-studio-nuget-package-manager-dialog"></a><span data-ttu-id="d40e4-129">Диалоговое окно диспетчера пакетов NuGet в Visual Studio</span><span class="sxs-lookup"><span data-stu-id="d40e4-129">Visual Studio NuGet Package Manager Dialog</span></span>

* <span data-ttu-id="d40e4-130">В меню Visual Studio выберите **"Проект" > "Управление пакетами NuGet"**.</span><span class="sxs-lookup"><span data-stu-id="d40e4-130">From the Visual Studio menu, select **Project > Manage NuGet Packages**</span></span>

* <span data-ttu-id="d40e4-131">Нажмите кнопку **Обзор** или откройте вкладку **Обновления**.</span><span class="sxs-lookup"><span data-stu-id="d40e4-131">Click on the **Browse** or the **Updates** tab</span></span>

* <span data-ttu-id="d40e4-132">Чтобы установить или обновить поставщик SQL Server, выберите пакет `Microsoft.EntityFrameworkCore.SqlServer` и подтвердите свой выбор.</span><span class="sxs-lookup"><span data-stu-id="d40e4-132">To install or update the SQL Server provider, select the `Microsoft.EntityFrameworkCore.SqlServer` package, and confirm.</span></span>

<span data-ttu-id="d40e4-133">Дополнительные сведения: [Диалоговое окно диспетчера пакетов NuGet](/nuget/tools/package-manager-ui).</span><span class="sxs-lookup"><span data-stu-id="d40e4-133">For more information, see [NuGet Package Manager Dialog](/nuget/tools/package-manager-ui).</span></span>

### <a name="visual-studio-nuget-package-manager-console"></a><span data-ttu-id="d40e4-134">Консоль диспетчера пакетов NuGet в Visual Studio</span><span class="sxs-lookup"><span data-stu-id="d40e4-134">Visual Studio NuGet Package Manager Console</span></span>

* <span data-ttu-id="d40e4-135">В меню Visual Studio выберите **"Сервис" > "Диспетчер пакетов NuGet" > "Консоль диспетчера пакетов"**.</span><span class="sxs-lookup"><span data-stu-id="d40e4-135">From the Visual Studio menu, select **Tools > NuGet Package Manager > Package Manager Console**</span></span>

* <span data-ttu-id="d40e4-136">Чтобы установить поставщик SQL Server, в консоли диспетчера пакетов выполните следующую команду:</span><span class="sxs-lookup"><span data-stu-id="d40e4-136">To install the SQL Server provider, run the following command in the Package Manager Console:</span></span>

  ```powershell
  Install-Package Microsoft.EntityFrameworkCore.SqlServer
  ```

* <span data-ttu-id="d40e4-137">Чтобы обновить поставщик, используйте команду `Update-Package`.</span><span class="sxs-lookup"><span data-stu-id="d40e4-137">To update the provider, use the `Update-Package` command.</span></span>

* <span data-ttu-id="d40e4-138">Чтобы указать конкретную версию, используйте модификатор `-Version`.</span><span class="sxs-lookup"><span data-stu-id="d40e4-138">To specify a specific version, use the `-Version` modifier.</span></span> <span data-ttu-id="d40e4-139">Например, чтобы установить пакеты EF Core 2.2.0, добавьте в команды `-Version 2.2.0`.</span><span class="sxs-lookup"><span data-stu-id="d40e4-139">For example, to install EF Core 2.2.0 packages, append `-Version 2.2.0` to the commands</span></span>

<span data-ttu-id="d40e4-140">Дополнительные сведения: [Консоль диспетчера пакетов](/nuget/tools/package-manager-console).</span><span class="sxs-lookup"><span data-stu-id="d40e4-140">For more information, see [Package Manager Console](/nuget/tools/package-manager-console).</span></span>

## <a name="get-the-entity-framework-core-tools"></a><span data-ttu-id="d40e4-141">Получение инструментов Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="d40e4-141">Get the Entity Framework Core tools</span></span>

<span data-ttu-id="d40e4-142">Вы можете установить инструменты для выполнения задач, связанных с EF Core, в своем проекте, таких как создание и применение миграций базы данных, а также создание модели EF Core на основе существующей базы данных.</span><span class="sxs-lookup"><span data-stu-id="d40e4-142">You can install tools to carry out EF Core-related tasks in your project, like creating and applying database migrations, or creating an EF Core model based on an existing database.</span></span>

<span data-ttu-id="d40e4-143">Доступно два набора инструментов:</span><span class="sxs-lookup"><span data-stu-id="d40e4-143">Two sets of tools are available:</span></span>

* <span data-ttu-id="d40e4-144">[Инструменты интерфейса командной строки (CLI)](xref:core/miscellaneous/cli/dotnet) .NET Core можно использовать в Windows, Linux и macOS.</span><span class="sxs-lookup"><span data-stu-id="d40e4-144">The [.NET Core command-line interface (CLI) tools](xref:core/miscellaneous/cli/dotnet) can be used on Windows, Linux, or macOS.</span></span> <span data-ttu-id="d40e4-145">Эти команды начинаются с `dotnet ef`.</span><span class="sxs-lookup"><span data-stu-id="d40e4-145">These commands begin with `dotnet ef`.</span></span>

* <span data-ttu-id="d40e4-146">[Инструменты консоли диспетчера пакетов](xref:core/miscellaneous/cli/powershell) работают в Visual Studio в Windows.</span><span class="sxs-lookup"><span data-stu-id="d40e4-146">The [Package Manager Console (PMC) tools](xref:core/miscellaneous/cli/powershell) run in Visual Studio on Windows.</span></span> <span data-ttu-id="d40e4-147">Эти команды начинаются с глагола, например `Add-Migration`, `Update-Database`.</span><span class="sxs-lookup"><span data-stu-id="d40e4-147">These commands start with a verb, for example `Add-Migration`, `Update-Database`.</span></span>

<span data-ttu-id="d40e4-148">Хотя вы можете использовать команды `dotnet ef` в консоли диспетчера пакетов, в Visual Studio рекомендуется использовать инструменты консоли диспетчера пакетов:</span><span class="sxs-lookup"><span data-stu-id="d40e4-148">Although you can also use the `dotnet ef` commands from the Package Manager Console, it's recommended to use the Package Manager Console tools when you're using Visual Studio:</span></span>

* <span data-ttu-id="d40e4-149">Они автоматически работают для выбранного в PMC проекта в Visual Studio без переключения каталогов вручную.</span><span class="sxs-lookup"><span data-stu-id="d40e4-149">They automatically work with the current project selected in the PMC in Visual Studio, without requiring manually switching directories.</span></span>

* <span data-ttu-id="d40e4-150">Они автоматически открывают файлы, созданные командами в Visual Studio, после завершения соответствующей команды.</span><span class="sxs-lookup"><span data-stu-id="d40e4-150">They automatically open files generated by the commands in Visual Studio after the command is completed.</span></span>

<a name="cli"></a>

### <a name="get-the-net-core-cli-tools"></a><span data-ttu-id="d40e4-151">Получение инструментов CLI для .NET Core</span><span class="sxs-lookup"><span data-stu-id="d40e4-151">Get the .NET Core CLI tools</span></span>

<span data-ttu-id="d40e4-152">Для инструментов CLI .NET Core требуется пакет SDK для .NET Core, указанный ранее в разделе [Необходимые компоненты](#prerequisites).</span><span class="sxs-lookup"><span data-stu-id="d40e4-152">.NET Core CLI tools require the .NET Core SDK, mentioned earlier in [Prerequisites](#prerequisites).</span></span>

* <span data-ttu-id="d40e4-153">Средство `dotnet ef` должно быть установлено в качестве глобального или локального средства.</span><span class="sxs-lookup"><span data-stu-id="d40e4-153">`dotnet ef` must be installed as a global or local tool.</span></span> <span data-ttu-id="d40e4-154">Большинство разработчиков предпочитают устанавливать средство `dotnet ef` в качестве глобального средства, используя следующую команду:</span><span class="sxs-lookup"><span data-stu-id="d40e4-154">Most developers prefer installing `dotnet ef` as a global tool using the following command:</span></span>

  ```dotnetcli
  dotnet tool install --global dotnet-ef
  ```

  <span data-ttu-id="d40e4-155">Средство `dotnet ef` также может использоваться в качестве локального средства.</span><span class="sxs-lookup"><span data-stu-id="d40e4-155">`dotnet ef` can also be used as a local tool.</span></span> <span data-ttu-id="d40e4-156">Чтобы использовать его в качестве локального средства, восстановите зависимости проекта, в котором оно объявляется в качестве соответствующей зависимости, с помощью [файла манифеста средства](/dotnet/core/tools/global-tools#install-a-local-tool).</span><span class="sxs-lookup"><span data-stu-id="d40e4-156">To use it as a local tool, restore the dependencies of a project that declares it as a tooling dependency using a [tool manifest file](/dotnet/core/tools/global-tools#install-a-local-tool).</span></span>

* <span data-ttu-id="d40e4-157">Чтобы обновить средства, используйте команду `dotnet tool update`.</span><span class="sxs-lookup"><span data-stu-id="d40e4-157">To update the tools, use the `dotnet tool update` command.</span></span>

* <span data-ttu-id="d40e4-158">Установите последнюю версию пакета `Microsoft.EntityFrameworkCore.Design`.</span><span class="sxs-lookup"><span data-stu-id="d40e4-158">Install the latest `Microsoft.EntityFrameworkCore.Design` package.</span></span>

  ```dotnetcli
  dotnet add package Microsoft.EntityFrameworkCore.Design
  ```

> [!IMPORTANT]
> <span data-ttu-id="d40e4-159">Всегда используйте версии пакетов инструментов, которые соответствуют основному номеру версии для пакетов среды выполнения.</span><span class="sxs-lookup"><span data-stu-id="d40e4-159">Always use the version of the tools package that matches the major version of the runtime packages.</span></span>

### <a name="get-the-package-manager-console-tools"></a><span data-ttu-id="d40e4-160">Получение инструментов консоли диспетчера пакетов</span><span class="sxs-lookup"><span data-stu-id="d40e4-160">Get the Package Manager Console tools</span></span>

<span data-ttu-id="d40e4-161">Чтобы получить инструменты консоли диспетчера пакетов для EF Core, установите пакет `Microsoft.EntityFrameworkCore.Tools`.</span><span class="sxs-lookup"><span data-stu-id="d40e4-161">To get the Package Manager Console tools for EF Core, install the `Microsoft.EntityFrameworkCore.Tools` package.</span></span> <span data-ttu-id="d40e4-162">Например, в Visual Studio:</span><span class="sxs-lookup"><span data-stu-id="d40e4-162">For example, from Visual Studio:</span></span>

```powershell
Install-Package Microsoft.EntityFrameworkCore.Tools
```

<span data-ttu-id="d40e4-163">Этот пакет автоматически включается в приложения ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="d40e4-163">For ASP.NET Core apps, this package is included automatically.</span></span>

## <a name="upgrading-to-the-latest-ef-core"></a><span data-ttu-id="d40e4-164">Обновление до последней версии EF Core</span><span class="sxs-lookup"><span data-stu-id="d40e4-164">Upgrading to the latest EF Core</span></span>

* <span data-ttu-id="d40e4-165">Каждый раз при выпуске новой версии EF Core мы также выпускаем новую версию поставщиков, которые входят в проект EF Core, например Microsoft.EntityFrameworkCore.SqlServer, Microsoft.EntityFrameworkCore.Sqlite и Microsoft.EntityFrameworkCore.InMemory.</span><span class="sxs-lookup"><span data-stu-id="d40e4-165">Any time we release a new version of EF Core, we also release a new version of the providers that are part of the EF Core project, like Microsoft.EntityFrameworkCore.SqlServer, Microsoft.EntityFrameworkCore.Sqlite, and Microsoft.EntityFrameworkCore.InMemory.</span></span> <span data-ttu-id="d40e4-166">Кроме того, можно просто обновить поставщик до новой версии, чтобы получить все улучшения.</span><span class="sxs-lookup"><span data-stu-id="d40e4-166">You can just upgrade to the new version of the provider to get all the improvements.</span></span>

* <span data-ttu-id="d40e4-167">EF Core вместе с поставщиками SQL Server и поставщиками в памяти включены в текущие версии ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="d40e4-167">EF Core, together with the SQL Server and the in-memory providers are included in current versions of ASP.NET Core.</span></span> <span data-ttu-id="d40e4-168">Чтобы обновить существующее приложение до более новой версии EF Core, всегда обновляйте версию ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="d40e4-168">To upgrade an existing ASP.NET Core application to a newer version of EF Core, always upgrade the version of ASP.NET Core.</span></span>

* <span data-ttu-id="d40e4-169">Если требуется обновить приложение, использующее сторонний поставщик базы данных, всегда ищите обновление поставщика, совместимое с нужной вам версией EF Core.</span><span class="sxs-lookup"><span data-stu-id="d40e4-169">If you need to update an application that is using a third-party database provider, always check for an update of the provider that is compatible with the version of EF Core you want to use.</span></span> <span data-ttu-id="d40e4-170">Например, поставщики баз данных для версии 1.0 не совместимы с версией 2.0 среды выполнения EF Core.</span><span class="sxs-lookup"><span data-stu-id="d40e4-170">For example, database providers for version 1.0 are not compatible with version 2.0 of the EF Core runtime.</span></span>

* <span data-ttu-id="d40e4-171">Сторонние поставщики EF Core обычно не выпускают исправления вместе со средой выполнения EF Core.</span><span class="sxs-lookup"><span data-stu-id="d40e4-171">Third-party providers for EF Core usually don't release patch versions alongside the EF Core runtime.</span></span> <span data-ttu-id="d40e4-172">Чтобы обновить приложение, использующее сторонний поставщик, до версии исправления EF Core, может потребоваться добавить прямую ссылку на отдельные компоненты EF Core, такие как Microsoft.EntityFrameworkCore и Microsoft.EntityFrameworkCore.Relational.</span><span class="sxs-lookup"><span data-stu-id="d40e4-172">To upgrade an application that uses a third-party provider to a patch version of EF Core, you may need to add a direct reference to individual EF Core runtime components, such as Microsoft.EntityFrameworkCore, and Microsoft.EntityFrameworkCore.Relational.</span></span>
