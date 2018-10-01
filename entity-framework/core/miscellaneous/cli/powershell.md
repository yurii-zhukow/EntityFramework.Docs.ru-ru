---
title: EF Core Справочник по средствам (консоль диспетчера пакетов) — EF Core
author: bricelam
ms.author: bricelam
ms.date: 09/18/2018
uid: core/miscellaneous/cli/powershell
ms.openlocfilehash: db4d89b6a0babe01bccbeadc51381a309ad8ca0f
ms.sourcegitcommit: c568d33214fc25c76e02c8529a29da7a356b37b4
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/30/2018
ms.locfileid: "47459568"
---
# <a name="entity-framework-core-tools-reference---package-manager-console-in-visual-studio"></a><span data-ttu-id="6596a-102">Справочник по - консоль диспетчера пакетов в Visual Studio средствам Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="6596a-102">Entity Framework Core tools reference - Package Manager Console in Visual Studio</span></span>

<span data-ttu-id="6596a-103">Инструменты консоли диспетчера пакетов (PMC) для Entity Framework Core выполнения задач разработки во время разработки.</span><span class="sxs-lookup"><span data-stu-id="6596a-103">The Package Manager Console (PMC) tools for Entity Framework Core perform design-time development tasks.</span></span> <span data-ttu-id="6596a-104">Например, они создают [миграций](/aspnet/core/data/ef-mvc/migrations?view=aspnetcore-2.0#introduction-to-migrations), применить миграции и создать код для модели, основанные на существующей базы данных.</span><span class="sxs-lookup"><span data-stu-id="6596a-104">For example, they create [migrations](/aspnet/core/data/ef-mvc/migrations?view=aspnetcore-2.0#introduction-to-migrations), apply migrations, and generate code for a model based on an existing database.</span></span> <span data-ttu-id="6596a-105">Команды выполняются внутри Visual Studio с помощью [консоль диспетчера пакетов](/nuget/tools/package-manager-console).</span><span class="sxs-lookup"><span data-stu-id="6596a-105">The commands run inside of Visual Studio using the [Package Manager Console](/nuget/tools/package-manager-console).</span></span> <span data-ttu-id="6596a-106">Эти инструменты совместимы с проектами .NET Framework и .NET Core.</span><span class="sxs-lookup"><span data-stu-id="6596a-106">These tools work with both .NET Framework and .NET Core projects.</span></span>

<span data-ttu-id="6596a-107">Если вы не используете Visual Studio, мы рекомендуем [инструменты командной строки EF Core](dotnet.md) вместо этого.</span><span class="sxs-lookup"><span data-stu-id="6596a-107">If you aren't using Visual Studio, we recommend the [EF Core Command-line Tools](dotnet.md) instead.</span></span> <span data-ttu-id="6596a-108">Средства CLI кросс платформенные и выполнения в командной строке.</span><span class="sxs-lookup"><span data-stu-id="6596a-108">The CLI tools are cross-platform and run inside a command prompt.</span></span>

## <a name="installing-the-tools"></a><span data-ttu-id="6596a-109">Установка инструментов</span><span class="sxs-lookup"><span data-stu-id="6596a-109">Installing the tools</span></span>

<span data-ttu-id="6596a-110">Процедуры по установке и средства обновления отличаются между ASP.NET Core 2.1 или более поздней и более ранних версий или других типов проектов.</span><span class="sxs-lookup"><span data-stu-id="6596a-110">The procedures for installing and updating the tools differ between ASP.NET Core 2.1+ and earlier versions or other project types.</span></span>

### <a name="aspnet-core-version-21-and-later"></a><span data-ttu-id="6596a-111">ASP.NET Core версии 2.1 и более поздние версии</span><span class="sxs-lookup"><span data-stu-id="6596a-111">ASP.NET Core version 2.1 and later</span></span>

<span data-ttu-id="6596a-112">Средства автоматически включаются в проекте ASP.NET Core 2.1 или более поздней, так как `Microsoft.EntityFrameworkCore.Tools` пакет включен в [метапакет Microsoft.AspNetCore.App](/aspnet/core/fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="6596a-112">The tools are automatically included in an ASP.NET Core 2.1+ project because the `Microsoft.EntityFrameworkCore.Tools` package is included in the [Microsoft.AspNetCore.App metapackage](/aspnet/core/fundamentals/metapackage-app).</span></span>

<span data-ttu-id="6596a-113">Поэтому не нужно ничего делать, чтобы установить средства, но вам нужно:</span><span class="sxs-lookup"><span data-stu-id="6596a-113">Therefore, you don't have to do anything to install the tools, but you do have to:</span></span>
* <span data-ttu-id="6596a-114">Восстановите пакеты перед использованием средства в новый проект.</span><span class="sxs-lookup"><span data-stu-id="6596a-114">Restore packages before using the tools in a new project.</span></span>
* <span data-ttu-id="6596a-115">Установите пакет для обновления до более новой версии инструментов.</span><span class="sxs-lookup"><span data-stu-id="6596a-115">Install a package to update the tools to a newer version.</span></span>

<span data-ttu-id="6596a-116">Чтобы убедиться в том, что вы получаете последнюю версию средства, рекомендуется также выполнить следующие действия:</span><span class="sxs-lookup"><span data-stu-id="6596a-116">To make sure that you're getting the latest version of the tools, we recommend that you also do the following step:</span></span>

* <span data-ttu-id="6596a-117">Изменить ваш *.csproj* файл и добавьте строку, указав последнюю версию [Microsoft.EntityFrameworkCore.Tools](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools/) пакета.</span><span class="sxs-lookup"><span data-stu-id="6596a-117">Edit your *.csproj* file and add a line specifying the latest version of the [Microsoft.EntityFrameworkCore.Tools](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools/) package.</span></span> <span data-ttu-id="6596a-118">Например *.csproj* файл может содержать `ItemGroup` , выглядящий следующим образом:</span><span class="sxs-lookup"><span data-stu-id="6596a-118">For example, the *.csproj* file might include an `ItemGroup` that looks like this:</span></span>

  ```xml
  <ItemGroup>
    <PackageReference Include="Microsoft.AspNetCore.App" />
    <PackageReference Include="Microsoft.EntityFrameworkCore.Tools" Version="2.1.3" />
    <PackageReference Include="Microsoft.VisualStudio.Web.CodeGeneration.Design" Version="2.1.1" />
  </ItemGroup>
  ```

<span data-ttu-id="6596a-119">Обновите инструменты при получении сообщения, как в следующем примере:</span><span class="sxs-lookup"><span data-stu-id="6596a-119">Update the tools when you get a message like the following example:</span></span>

> <span data-ttu-id="6596a-120">Версия средства EF Core «2.1.1-rtm-30846» старше, среды выполнения «2.1.3-rtm-32065».</span><span class="sxs-lookup"><span data-stu-id="6596a-120">The EF Core tools version '2.1.1-rtm-30846' is older than that of the runtime '2.1.3-rtm-32065'.</span></span> <span data-ttu-id="6596a-121">Обновите инструменты для новейших функций и исправления ошибок.</span><span class="sxs-lookup"><span data-stu-id="6596a-121">Update the tools for the latest features and bug fixes.</span></span>

<span data-ttu-id="6596a-122">Чтобы обновить средства:</span><span class="sxs-lookup"><span data-stu-id="6596a-122">To update the tools:</span></span>
* <span data-ttu-id="6596a-123">Установите последний пакет SDK .NET Core.</span><span class="sxs-lookup"><span data-stu-id="6596a-123">Install the latest .NET Core SDK.</span></span>
* <span data-ttu-id="6596a-124">Обновление Visual Studio до последней версии.</span><span class="sxs-lookup"><span data-stu-id="6596a-124">Update Visual Studio to the latest version.</span></span>
* <span data-ttu-id="6596a-125">Изменить *.csproj* файл, теперь она содержит ссылки на последнюю версию пакета средств, пакет, как показано выше.</span><span class="sxs-lookup"><span data-stu-id="6596a-125">Edit the *.csproj* file so that it includes a package reference to the latest tools package, as shown earlier.</span></span>

### <a name="other-versions-and-project-types"></a><span data-ttu-id="6596a-126">Другие версии и типы проектов</span><span class="sxs-lookup"><span data-stu-id="6596a-126">Other versions and project types</span></span>

<span data-ttu-id="6596a-127">Установить средства консоль диспетчера пакетов, выполнив следующую команду в **консоль диспетчера пакетов**:</span><span class="sxs-lookup"><span data-stu-id="6596a-127">Install the Package Manager Console tools by running the following command in **Package Manager Console**:</span></span>

``` powershell
Install-Package Microsoft.EntityFrameworkCore.Tools
```

<span data-ttu-id="6596a-128">Обновить инструменты, выполнив следующую команду в **консоль диспетчера пакетов**.</span><span class="sxs-lookup"><span data-stu-id="6596a-128">Update the tools by running the following command in **Package Manager Console**.</span></span>

``` powershell
Update-Package Microsoft.EntityFrameworkCore.Tools
```

### <a name="verify-the-installation"></a><span data-ttu-id="6596a-129">Проверка установки</span><span class="sxs-lookup"><span data-stu-id="6596a-129">Verify the installation</span></span>

<span data-ttu-id="6596a-130">Убедитесь, что установлены средства, выполнив следующую команду:</span><span class="sxs-lookup"><span data-stu-id="6596a-130">Verify that the tools are installed by running this command:</span></span>

``` powershell
Get-Help about_EntityFrameworkCore
```

<span data-ttu-id="6596a-131">Результат выглядит следующим образом (она не сообщает, какие версии инструментов, вы используете):</span><span class="sxs-lookup"><span data-stu-id="6596a-131">The output looks like this (it doesn't tell you which version of the tools you're using):</span></span>

```console

                     _/\__
               ---==/    \\
         ___  ___   |.    \|\
        | __|| __|  |  )   \\\
        | _| | _|   \_/ |  //|\\
        |___||_|       /   \\\/\\

TOPIC
    about_EntityFrameworkCore

SHORT DESCRIPTION
    Provides information about the Entity Framework Core Package Manager Console Tools.

<A list of available commands follows, omitted here.>
```

## <a name="using-the-tools"></a><span data-ttu-id="6596a-132">С помощью средств</span><span class="sxs-lookup"><span data-stu-id="6596a-132">Using the tools</span></span>

<span data-ttu-id="6596a-133">Перед использованием средства:</span><span class="sxs-lookup"><span data-stu-id="6596a-133">Before using the tools:</span></span>
* <span data-ttu-id="6596a-134">Понимать разницу между целевой и запуска проекта.</span><span class="sxs-lookup"><span data-stu-id="6596a-134">Understand the difference between target and startup project.</span></span>
* <span data-ttu-id="6596a-135">Узнайте, как использовать средства с помощью библиотеки классов .NET Standard.</span><span class="sxs-lookup"><span data-stu-id="6596a-135">Learn how to use the tools with .NET Standard class libraries.</span></span>
* <span data-ttu-id="6596a-136">Для проектов ASP.NET Core настройте среду.</span><span class="sxs-lookup"><span data-stu-id="6596a-136">For ASP.NET Core projects, set the environment.</span></span>

### <a name="target-and-startup-project"></a><span data-ttu-id="6596a-137">Целевой объект и запуска проекта</span><span class="sxs-lookup"><span data-stu-id="6596a-137">Target and startup project</span></span>

<span data-ttu-id="6596a-138">См. команды *проекта* и *запускаемым проектом*.</span><span class="sxs-lookup"><span data-stu-id="6596a-138">The commands refer to a *project* and a *startup project*.</span></span>

* <span data-ttu-id="6596a-139">*Проекта* также известен как *целевой проект* так как это где команды добавления или удаления файлов.</span><span class="sxs-lookup"><span data-stu-id="6596a-139">The *project* is also known as the *target project* because it's where the commands add or remove files.</span></span> <span data-ttu-id="6596a-140">По умолчанию **проект по умолчанию** на **консоль диспетчера пакетов** является целевого проекта.</span><span class="sxs-lookup"><span data-stu-id="6596a-140">By default, the **Default project** selected in **Package Manager Console** is the target project.</span></span> <span data-ttu-id="6596a-141">Можно указать другой проект в качестве целевого проекта с помощью <nobr> `--project` </nobr> параметр.</span><span class="sxs-lookup"><span data-stu-id="6596a-141">You can specify a different project as target project by using the <nobr>`--project`</nobr> option.</span></span>

* <span data-ttu-id="6596a-142">*Запускаемым проектом* является тот, который построение и запуск средств.</span><span class="sxs-lookup"><span data-stu-id="6596a-142">The *startup project* is the one that the tools build and run.</span></span> <span data-ttu-id="6596a-143">Средства должны выполнить код приложения во время разработки для получения сведений о проекте, например строку подключения базы данных и конфигурации модели.</span><span class="sxs-lookup"><span data-stu-id="6596a-143">The tools have to execute application code at design time to get information about the project, such as the database connection string and the configuration of the model.</span></span> <span data-ttu-id="6596a-144">По умолчанию **запускаемым проектом** в **обозревателе решений** является запускаемым проектом.</span><span class="sxs-lookup"><span data-stu-id="6596a-144">By default, the **Startup Project** in **Solution Explorer** is the startup project.</span></span> <span data-ttu-id="6596a-145">Можно указать другой проект запускаемым проектом с помощью <nobr> `--startup-project` </nobr> параметр.</span><span class="sxs-lookup"><span data-stu-id="6596a-145">You can specify a different project as startup project by using the <nobr>`--startup-project`</nobr> option.</span></span>

<span data-ttu-id="6596a-146">Зачастую того же проекта запускаемого проекта и целевой проект.</span><span class="sxs-lookup"><span data-stu-id="6596a-146">The startup project and target project are often the same project.</span></span> <span data-ttu-id="6596a-147">Типичный сценарий, где они находятся отдельные проекты — когда:</span><span class="sxs-lookup"><span data-stu-id="6596a-147">A typical scenario where they are separate projects is when:</span></span>

* <span data-ttu-id="6596a-148">В библиотеке классов .NET Core, EF Core контекст и классы сущностей.</span><span class="sxs-lookup"><span data-stu-id="6596a-148">The EF Core context and entity classes are in a .NET Core class library.</span></span>
* <span data-ttu-id="6596a-149">Консольное приложение .NET Core или веб-приложение ссылается на библиотеку классов.</span><span class="sxs-lookup"><span data-stu-id="6596a-149">A .NET Core console app or web app references the class library.</span></span>

<span data-ttu-id="6596a-150">Существует также возможность [поместите код миграции в библиотеку классов, отдельно от контекста EF Core](xref:core/managing-schemas/migrations/projects).</span><span class="sxs-lookup"><span data-stu-id="6596a-150">It's also possible to [put migrations code in a class library separate from the EF Core context](xref:core/managing-schemas/migrations/projects).</span></span>

### <a name="other-target-frameworks"></a><span data-ttu-id="6596a-151">Другие целевые платформы</span><span class="sxs-lookup"><span data-stu-id="6596a-151">Other target frameworks</span></span>

<span data-ttu-id="6596a-152">Консоль диспетчера пакетов средств работы с проектами .NET Core или .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="6596a-152">The Package Manager Console tools work with .NET Core or .NET Framework projects.</span></span> <span data-ttu-id="6596a-153">Приложения, которые имеют модель EF Core в библиотеку классов .NET Standard может отсутствовать, .NET Core или .NET Framework проекта.</span><span class="sxs-lookup"><span data-stu-id="6596a-153">Apps that have the EF Core model in a .NET Standard class library might not have a .NET Core or .NET Framework project.</span></span> <span data-ttu-id="6596a-154">Например это верно для приложений Xamarin и универсальной платформы Windows.</span><span class="sxs-lookup"><span data-stu-id="6596a-154">For example, this is true of Xamarin and Universal Windows Platform apps.</span></span> <span data-ttu-id="6596a-155">В таком случае можно создать .NET Core или .NET Framework проекта консольного приложения, единственным назначением которого является в качестве запускаемого проекта для средств.</span><span class="sxs-lookup"><span data-stu-id="6596a-155">In such cases, you can create a .NET Core or .NET Framework console app project whose only purpose is to act as startup project for the tools.</span></span> <span data-ttu-id="6596a-156">Проект может быть пустым проектом с не содержащим реального кода &mdash; этот компонент необходим только для предоставления целевой объект для выполнения средства.</span><span class="sxs-lookup"><span data-stu-id="6596a-156">The project can be a dummy project with no real code &mdash; it is only needed to provide a target for the tooling.</span></span>

<span data-ttu-id="6596a-157">Почему является пустым проектом требуется?</span><span class="sxs-lookup"><span data-stu-id="6596a-157">Why is a dummy project required?</span></span> <span data-ttu-id="6596a-158">Как упоминалось ранее, средства нужно выполнять код приложения во время разработки.</span><span class="sxs-lookup"><span data-stu-id="6596a-158">As mentioned earlier, the tools have to execute application code at design time.</span></span> <span data-ttu-id="6596a-159">Чтобы сделать это, они должны использовать среду выполнения .NET Core или .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="6596a-159">To do that, they need to use the .NET Core or .NET Framework runtime.</span></span> <span data-ttu-id="6596a-160">Когда модель EF Core находится в проекте, ориентированном на .NET Core или .NET Framework, средства EF Core позаимствовать среды выполнения из проекта.</span><span class="sxs-lookup"><span data-stu-id="6596a-160">When the EF Core model is in a project that targets .NET Core or .NET Framework, the EF Core tools borrow the runtime from the project.</span></span> <span data-ttu-id="6596a-161">Они не могут делать, если модель EF Core в библиотеку классов .NET Standard.</span><span class="sxs-lookup"><span data-stu-id="6596a-161">They can't do that if the EF Core model is in a .NET Standard class library.</span></span> <span data-ttu-id="6596a-162">.NET Standard не фактическую реализацию .NET; это спецификация набора API-интерфейсов, реализации .NET должны поддерживать.</span><span class="sxs-lookup"><span data-stu-id="6596a-162">The .NET Standard is not an actual .NET implementation; it's a specification of a set of APIs that .NET implementations must support.</span></span> <span data-ttu-id="6596a-163">Поэтому .NET Standard недостаточно для инструментов EF Core для выполнения кода приложения.</span><span class="sxs-lookup"><span data-stu-id="6596a-163">Therefore .NET Standard is not sufficient for the EF Core tools to execute application code.</span></span> <span data-ttu-id="6596a-164">Фиктивный проект, созданный для использования в качестве запускаемого проекта предоставляет конкретную целевую платформу, в которой средства можно загружать библиотеки классов .NET Standard.</span><span class="sxs-lookup"><span data-stu-id="6596a-164">The dummy project you create to use as startup project provides a concrete target platform into which the tools can load the .NET Standard class library.</span></span>

### <a name="aspnet-core-environment"></a><span data-ttu-id="6596a-165">Среда ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="6596a-165">ASP.NET Core environment</span></span>

<span data-ttu-id="6596a-166">Чтобы указать среду для проектов ASP.NET Core, задайте **env:ASPNETCORE_ENVIRONMENT** перед выполнением команд.</span><span class="sxs-lookup"><span data-stu-id="6596a-166">To specify the environment for ASP.NET Core projects, set **env:ASPNETCORE_ENVIRONMENT** before running commands.</span></span>

## <a name="common-parameters"></a><span data-ttu-id="6596a-167">Общие параметры</span><span class="sxs-lookup"><span data-stu-id="6596a-167">Common parameters</span></span>

<span data-ttu-id="6596a-168">В следующей таблице показаны параметры, которые являются общими для всех команд EF Core:</span><span class="sxs-lookup"><span data-stu-id="6596a-168">The following table shows parameters that are common to all of the EF Core commands:</span></span>

| <span data-ttu-id="6596a-169">Параметр</span><span class="sxs-lookup"><span data-stu-id="6596a-169">Parameter</span></span>                 | <span data-ttu-id="6596a-170">Описание</span><span class="sxs-lookup"><span data-stu-id="6596a-170">Description</span></span>                                                                                                                                                                                                          |
|:--------------------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="6596a-171">-Контексте \<строка ></span><span class="sxs-lookup"><span data-stu-id="6596a-171">-Context \<String></span></span>        | <span data-ttu-id="6596a-172">`DbContext` Класс для использования.</span><span class="sxs-lookup"><span data-stu-id="6596a-172">The `DbContext` class to use.</span></span> <span data-ttu-id="6596a-173">Имя класса, только из цифр или полное с пространствами имен.</span><span class="sxs-lookup"><span data-stu-id="6596a-173">Class name only or fully qualified with namespaces.</span></span>  <span data-ttu-id="6596a-174">Если этот параметр опущен, EF Core находит класс контекста.</span><span class="sxs-lookup"><span data-stu-id="6596a-174">If this parameter is omitted, EF Core finds the context class.</span></span> <span data-ttu-id="6596a-175">Если имеется несколько классов контекста, этот параметр является обязательным.</span><span class="sxs-lookup"><span data-stu-id="6596a-175">If there are multiple context classes, this parameter is required.</span></span> |
| <span data-ttu-id="6596a-176">— Проект \<строка ></span><span class="sxs-lookup"><span data-stu-id="6596a-176">-Project \<String></span></span>        | <span data-ttu-id="6596a-177">Целевой проект.</span><span class="sxs-lookup"><span data-stu-id="6596a-177">The target project.</span></span> <span data-ttu-id="6596a-178">Если этот параметр опущен, **проект по умолчанию** для **консоль диспетчера пакетов** используется в качестве целевого проекта.</span><span class="sxs-lookup"><span data-stu-id="6596a-178">If this parameter is omitted, the **Default project** for **Package Manager Console** is used as the target project.</span></span>                                                                             |
| <span data-ttu-id="6596a-179">-StartupProject \<строка ></span><span class="sxs-lookup"><span data-stu-id="6596a-179">-StartupProject \<String></span></span> | <span data-ttu-id="6596a-180">Запускаемый проект.</span><span class="sxs-lookup"><span data-stu-id="6596a-180">The startup project.</span></span> <span data-ttu-id="6596a-181">Если этот параметр опущен, **запускаемым проектом** в **свойства решения** используется в качестве целевого проекта.</span><span class="sxs-lookup"><span data-stu-id="6596a-181">If this parameter is omitted, the **Startup project** in **Solution properties** is used as the target project.</span></span>                                                                                 |
| <span data-ttu-id="6596a-182">-Verbose</span><span class="sxs-lookup"><span data-stu-id="6596a-182">-Verbose</span></span>                  | <span data-ttu-id="6596a-183">Показать подробные выходные данные.</span><span class="sxs-lookup"><span data-stu-id="6596a-183">Show verbose output.</span></span>                                                                                                                                                                                                 |

<span data-ttu-id="6596a-184">Чтобы отобразить справочные сведения о команде, используйте PowerShell `Get-Help` команды.</span><span class="sxs-lookup"><span data-stu-id="6596a-184">To show help information about a command, use PowerShell's `Get-Help` command.</span></span>

> [!TIP]
> <span data-ttu-id="6596a-185">Параметры контекста, проекта и StartupProject поддерживает расширение функций клавиши tab.</span><span class="sxs-lookup"><span data-stu-id="6596a-185">The Context, Project, and StartupProject parameters support tab-expansion.</span></span>

## <a name="add-migration"></a><span data-ttu-id="6596a-186">Add-Migration</span><span class="sxs-lookup"><span data-stu-id="6596a-186">Add-Migration</span></span>

<span data-ttu-id="6596a-187">Добавление новой миграции.</span><span class="sxs-lookup"><span data-stu-id="6596a-187">Adds a new migration.</span></span>

<span data-ttu-id="6596a-188">Параметры:</span><span class="sxs-lookup"><span data-stu-id="6596a-188">Parameters:</span></span>

| <span data-ttu-id="6596a-189">Параметр</span><span class="sxs-lookup"><span data-stu-id="6596a-189">Parameter</span></span>                         | <span data-ttu-id="6596a-190">Описание</span><span class="sxs-lookup"><span data-stu-id="6596a-190">Description</span></span>                                                                                                             |
|:----------------------------------|:------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="6596a-191"><nobr>-Name \<строка ><nobr></span><span class="sxs-lookup"><span data-stu-id="6596a-191"><nobr>-Name \<String><nobr></span></span>       | <span data-ttu-id="6596a-192">Имя миграции.</span><span class="sxs-lookup"><span data-stu-id="6596a-192">The name of the migration.</span></span> <span data-ttu-id="6596a-193">Это является позиционным параметром и является обязательным.</span><span class="sxs-lookup"><span data-stu-id="6596a-193">This is a positional parameter and is required.</span></span>                                              |
| <span data-ttu-id="6596a-194"><nobr>-OutputDir \<строка ></nobr></span><span class="sxs-lookup"><span data-stu-id="6596a-194"><nobr>-OutputDir \<String></nobr></span></span> | <span data-ttu-id="6596a-195">Каталог (и sub-namespace) для использования.</span><span class="sxs-lookup"><span data-stu-id="6596a-195">The directory (and sub-namespace) to use.</span></span> <span data-ttu-id="6596a-196">Пути задаются относительно каталога проекта целевой объект.</span><span class="sxs-lookup"><span data-stu-id="6596a-196">Paths are relative to the target project directory.</span></span> <span data-ttu-id="6596a-197">По умолчанию используется «Миграция».</span><span class="sxs-lookup"><span data-stu-id="6596a-197">Defaults to "Migrations".</span></span> |

## <a name="drop-database"></a><span data-ttu-id="6596a-198">DROP Database</span><span class="sxs-lookup"><span data-stu-id="6596a-198">Drop-Database</span></span>

<span data-ttu-id="6596a-199">Удаляет базу данных.</span><span class="sxs-lookup"><span data-stu-id="6596a-199">Drops the database.</span></span>

<span data-ttu-id="6596a-200">Параметры:</span><span class="sxs-lookup"><span data-stu-id="6596a-200">Parameters:</span></span>

| <span data-ttu-id="6596a-201">Параметр</span><span class="sxs-lookup"><span data-stu-id="6596a-201">Parameter</span></span> | <span data-ttu-id="6596a-202">Описание</span><span class="sxs-lookup"><span data-stu-id="6596a-202">Description</span></span>                                              |
|:----------|:---------------------------------------------------------|
| <span data-ttu-id="6596a-203">-WhatIf</span><span class="sxs-lookup"><span data-stu-id="6596a-203">-WhatIf</span></span>   | <span data-ttu-id="6596a-204">Показать базу данных, которая будет удалена, но не удаляйте ее.</span><span class="sxs-lookup"><span data-stu-id="6596a-204">Show which database would be dropped, but don't drop it.</span></span> |

## <a name="get-dbcontext"></a><span data-ttu-id="6596a-205">Get-DbContext</span><span class="sxs-lookup"><span data-stu-id="6596a-205">Get-DbContext</span></span>

<span data-ttu-id="6596a-206">Выводит список доступных `DbContext` типов.</span><span class="sxs-lookup"><span data-stu-id="6596a-206">Lists available `DbContext` types.</span></span>

## <a name="remove-migration"></a><span data-ttu-id="6596a-207">Remove-Migration</span><span class="sxs-lookup"><span data-stu-id="6596a-207">Remove-Migration</span></span>

<span data-ttu-id="6596a-208">Удаляет последнюю миграцию (выполняется откат изменений кода, которые были выполнены для миграции).</span><span class="sxs-lookup"><span data-stu-id="6596a-208">Removes the last migration (rolls back the code changes that were done for the migration).</span></span>

<span data-ttu-id="6596a-209">Параметры:</span><span class="sxs-lookup"><span data-stu-id="6596a-209">Parameters:</span></span>

| <span data-ttu-id="6596a-210">Параметр</span><span class="sxs-lookup"><span data-stu-id="6596a-210">Parameter</span></span> | <span data-ttu-id="6596a-211">Описание</span><span class="sxs-lookup"><span data-stu-id="6596a-211">Description</span></span>                                                                     |
|:----------|:--------------------------------------------------------------------------------|
| <span data-ttu-id="6596a-212">-Force</span><span class="sxs-lookup"><span data-stu-id="6596a-212">-Force</span></span>    | <span data-ttu-id="6596a-213">Миграции отменить (откатить изменения, которые были применены к базе данных).</span><span class="sxs-lookup"><span data-stu-id="6596a-213">Revert the migration (roll back the changes that were applied to the database).</span></span> |

## <a name="scaffold-dbcontext"></a><span data-ttu-id="6596a-214">Scaffold-DbContext</span><span class="sxs-lookup"><span data-stu-id="6596a-214">Scaffold-DbContext</span></span>

<span data-ttu-id="6596a-215">Создает код для `DbContext` и типы сущностей для базы данных.</span><span class="sxs-lookup"><span data-stu-id="6596a-215">Generates code for a `DbContext` and entity types for a database.</span></span>

<span data-ttu-id="6596a-216">Параметры:</span><span class="sxs-lookup"><span data-stu-id="6596a-216">Parameters:</span></span>

| <span data-ttu-id="6596a-217">Параметр</span><span class="sxs-lookup"><span data-stu-id="6596a-217">Parameter</span></span>                          | <span data-ttu-id="6596a-218">Описание</span><span class="sxs-lookup"><span data-stu-id="6596a-218">Description</span></span>                                                                                                                                                                                                                                                             |
|:-----------------------------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="6596a-219"><nobr>-Connection \<строка ></nobr></span><span class="sxs-lookup"><span data-stu-id="6596a-219"><nobr>-Connection \<String></nobr></span></span> | <span data-ttu-id="6596a-220">Строка подключения к базе данных.</span><span class="sxs-lookup"><span data-stu-id="6596a-220">The connection string to the database.</span></span> <span data-ttu-id="6596a-221">Для проектов ASP.NET Core 2.x, значение может быть *имя =\<имя строки подключения >*.</span><span class="sxs-lookup"><span data-stu-id="6596a-221">For ASP.NET Core 2.x projects, the value can be *name=\<name of connection string>*.</span></span> <span data-ttu-id="6596a-222">В этом случае имя берется из источников конфигурации, которые были настроены для проекта.</span><span class="sxs-lookup"><span data-stu-id="6596a-222">In that case the name comes from the configuration sources that are set up for the project.</span></span> <span data-ttu-id="6596a-223">Это является позиционным параметром и является обязательным.</span><span class="sxs-lookup"><span data-stu-id="6596a-223">This is a positional parameter and is required.</span></span> |
| <span data-ttu-id="6596a-224"><nobr>-Поставщик \<строка ></nobr></span><span class="sxs-lookup"><span data-stu-id="6596a-224"><nobr>-Provider \<String></nobr></span></span>   | <span data-ttu-id="6596a-225">Используемый поставщик.</span><span class="sxs-lookup"><span data-stu-id="6596a-225">The provider to use.</span></span> <span data-ttu-id="6596a-226">Обычно это имя пакета NuGet, например: `Microsoft.EntityFrameworkCore.SqlServer`.</span><span class="sxs-lookup"><span data-stu-id="6596a-226">Typically this is the name of the NuGet package, for example: `Microsoft.EntityFrameworkCore.SqlServer`.</span></span> <span data-ttu-id="6596a-227">Это является позиционным параметром и является обязательным.</span><span class="sxs-lookup"><span data-stu-id="6596a-227">This is a positional parameter and is required.</span></span>                                                                                           |
| <span data-ttu-id="6596a-228">-OutputDir \<строка ></span><span class="sxs-lookup"><span data-stu-id="6596a-228">-OutputDir \<String></span></span>               | <span data-ttu-id="6596a-229">Чтобы поместить файлы каталог.</span><span class="sxs-lookup"><span data-stu-id="6596a-229">The directory to put files in.</span></span> <span data-ttu-id="6596a-230">Пути задаются относительно каталога проекта.</span><span class="sxs-lookup"><span data-stu-id="6596a-230">Paths are relative to the project directory.</span></span>                                                                                                                                                                                             |
| <span data-ttu-id="6596a-231">-ContextDir \<строка ></span><span class="sxs-lookup"><span data-stu-id="6596a-231">-ContextDir \<String></span></span>              | <span data-ttu-id="6596a-232">Каталог для размещения `DbContext` в файл.</span><span class="sxs-lookup"><span data-stu-id="6596a-232">The directory to put the `DbContext` file in.</span></span> <span data-ttu-id="6596a-233">Пути задаются относительно каталога проекта.</span><span class="sxs-lookup"><span data-stu-id="6596a-233">Paths are relative to the project directory.</span></span>                                                                                                                                                                              |
| <span data-ttu-id="6596a-234">-Контексте \<строка ></span><span class="sxs-lookup"><span data-stu-id="6596a-234">-Context \<String></span></span>                 | <span data-ttu-id="6596a-235">Имя `DbContext` класса, необходимо создать.</span><span class="sxs-lookup"><span data-stu-id="6596a-235">The name of the `DbContext` class to generate.</span></span>                                                                                                                                                                                                                          |
| <span data-ttu-id="6596a-236">-Схемы \<String [] ></span><span class="sxs-lookup"><span data-stu-id="6596a-236">-Schemas \<String[]></span></span>               | <span data-ttu-id="6596a-237">Схемы таблиц для создания типов сущности для.</span><span class="sxs-lookup"><span data-stu-id="6596a-237">The schemas of tables to generate entity types for.</span></span> <span data-ttu-id="6596a-238">Если этот параметр опущен, включаются все схемы.</span><span class="sxs-lookup"><span data-stu-id="6596a-238">If this parameter is omitted, all schemas are included.</span></span>                                                                                                                                                             |
| <span data-ttu-id="6596a-239">-Таблицы \<String [] ></span><span class="sxs-lookup"><span data-stu-id="6596a-239">-Tables \<String[]></span></span>                | <span data-ttu-id="6596a-240">Для создания типов сущности для таблиц.</span><span class="sxs-lookup"><span data-stu-id="6596a-240">The tables to generate entity types for.</span></span> <span data-ttu-id="6596a-241">Если этот параметр опущен, включаются все таблицы.</span><span class="sxs-lookup"><span data-stu-id="6596a-241">If this parameter is omitted, all tables are included.</span></span>                                                                                                                                                                         |
| <span data-ttu-id="6596a-242">-DataAnnotations</span><span class="sxs-lookup"><span data-stu-id="6596a-242">-DataAnnotations</span></span>                   | <span data-ttu-id="6596a-243">Атрибуты можно используйте для настройки модели (где это возможно).</span><span class="sxs-lookup"><span data-stu-id="6596a-243">Use attributes to configure the model (where possible).</span></span> <span data-ttu-id="6596a-244">Если этот параметр опущен, используется только текучего API.</span><span class="sxs-lookup"><span data-stu-id="6596a-244">If this parameter is omitted, only the fluent API is used.</span></span>                                                                                                                                                      |
| <span data-ttu-id="6596a-245">-UseDatabaseNames</span><span class="sxs-lookup"><span data-stu-id="6596a-245">-UseDatabaseNames</span></span>                  | <span data-ttu-id="6596a-246">Используйте имена таблиц и столбцов, так же, как в базе данных.</span><span class="sxs-lookup"><span data-stu-id="6596a-246">Use table and column names exactly as they appear in the database.</span></span> <span data-ttu-id="6596a-247">Если этот параметр опущен, имена баз данных меняется на более точно соответствовать соглашениям C# имя стиля.</span><span class="sxs-lookup"><span data-stu-id="6596a-247">If this parameter is omitted, database names are changed to more closely conform to C# name style conventions.</span></span>                                                                                       |
| <span data-ttu-id="6596a-248">-Force</span><span class="sxs-lookup"><span data-stu-id="6596a-248">-Force</span></span>                             | <span data-ttu-id="6596a-249">Перезаписывайте существующие файлы.</span><span class="sxs-lookup"><span data-stu-id="6596a-249">Overwrite existing files.</span></span>                                                                                                                                                                                                                                               |

<span data-ttu-id="6596a-250">Пример</span><span class="sxs-lookup"><span data-stu-id="6596a-250">Example:</span></span>

```powershell
Scaffold-DbContext "Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer -OutputDir Models
```

<span data-ttu-id="6596a-251">Пример, который формирует шаблоны только указанных таблиц и создает контекст в отдельную папку с указанным именем.</span><span class="sxs-lookup"><span data-stu-id="6596a-251">Example that scaffolds only selected tables and creates the context in a separate folder with a specified name:</span></span>

```powershell
Scaffold-DbContext "Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer -OutputDir Models -Tables "Blog","Post" -ContextDir Context -Context BlogContext
```

## <a name="script-migration"></a><span data-ttu-id="6596a-252">Миграция скриптов</span><span class="sxs-lookup"><span data-stu-id="6596a-252">Script-Migration</span></span>

<span data-ttu-id="6596a-253">Формирует скрипт SQL, который применяет все изменения из одной выбранной миграции к другой выбранный миграции.</span><span class="sxs-lookup"><span data-stu-id="6596a-253">Generates a SQL script that applies all of the changes from one selected migration to another selected migration.</span></span>

<span data-ttu-id="6596a-254">Параметры:</span><span class="sxs-lookup"><span data-stu-id="6596a-254">Parameters:</span></span>

| <span data-ttu-id="6596a-255">Параметр</span><span class="sxs-lookup"><span data-stu-id="6596a-255">Parameter</span></span>                | <span data-ttu-id="6596a-256">Описание</span><span class="sxs-lookup"><span data-stu-id="6596a-256">Description</span></span>                                                                                                                                                                                                                |
|:-------------------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="6596a-257">*-Из* \<строка ></span><span class="sxs-lookup"><span data-stu-id="6596a-257">*-From* \<String></span></span>        | <span data-ttu-id="6596a-258">Начала миграции.</span><span class="sxs-lookup"><span data-stu-id="6596a-258">The starting migration.</span></span> <span data-ttu-id="6596a-259">Миграция может быть идентифицирован по имени или по идентификатору.</span><span class="sxs-lookup"><span data-stu-id="6596a-259">Migrations may be identified by name or by ID.</span></span> <span data-ttu-id="6596a-260">Число 0 является особым случаем, это значит, что *перед первой миграции*.</span><span class="sxs-lookup"><span data-stu-id="6596a-260">The number 0 is a special case that means *before the first migration*.</span></span> <span data-ttu-id="6596a-261">По умолчанию равно 0.</span><span class="sxs-lookup"><span data-stu-id="6596a-261">Defaults to 0.</span></span>                                                              |
| <span data-ttu-id="6596a-262">*-* \<Строка ></span><span class="sxs-lookup"><span data-stu-id="6596a-262">*-To* \<String></span></span>          | <span data-ttu-id="6596a-263">Окончания миграции.</span><span class="sxs-lookup"><span data-stu-id="6596a-263">The ending migration.</span></span> <span data-ttu-id="6596a-264">По умолчанию к последней миграции.</span><span class="sxs-lookup"><span data-stu-id="6596a-264">Defaults to the last migration.</span></span>                                                                                                                                                                      |
| <span data-ttu-id="6596a-265"><nobr>-Идемпотентными</nobr></span><span class="sxs-lookup"><span data-stu-id="6596a-265"><nobr>-Idempotent</nobr></span></span> | <span data-ttu-id="6596a-266">Создайте скрипт, который может использоваться в любой базе данных с любой миграции.</span><span class="sxs-lookup"><span data-stu-id="6596a-266">Generate a script that can be used on a database at any migration.</span></span>                                                                                                                                                         |
| <span data-ttu-id="6596a-267">-Выходной \<строка ></span><span class="sxs-lookup"><span data-stu-id="6596a-267">-Output \<String></span></span>        | <span data-ttu-id="6596a-268">Файл для записи результата.</span><span class="sxs-lookup"><span data-stu-id="6596a-268">The file to write the result to.</span></span> <span data-ttu-id="6596a-269">Если этот параметр указан, файл создается с созданным именем в той же папке при создании файлов среды выполнения приложения, например: */obj/Debug/netcoreapp2.1/ghbkztfz.sql/*.</span><span class="sxs-lookup"><span data-stu-id="6596a-269">IF this parameter is omitted, the file is created with a generated name in the same folder as the app's runtime files are created, for example: */obj/Debug/netcoreapp2.1/ghbkztfz.sql/*.</span></span> |

> [!TIP]
> <span data-ttu-id="6596a-270">To, From, и выходные параметры поддерживают расширение функций клавиши tab.</span><span class="sxs-lookup"><span data-stu-id="6596a-270">The To, From, and Output parameters support tab-expansion.</span></span>

<span data-ttu-id="6596a-271">В следующем примере создается скрипт для миграции это InitialCreate, используя имя миграции.</span><span class="sxs-lookup"><span data-stu-id="6596a-271">The following example creates a script for the InitialCreate migration, using the migration name.</span></span>

```powershell
Script-Migration -To InitialCreate
```

<span data-ttu-id="6596a-272">В следующем примере создается скрипт для всех операций миграции после миграции это InitialCreate, используя идентификатор миграции.</span><span class="sxs-lookup"><span data-stu-id="6596a-272">The following example creates a script for all migrations after the InitialCreate migration, using the migration ID.</span></span>

```powershell
Script-Migration -From 20180904195021_InitialCreate
```

## <a name="update-database"></a><span data-ttu-id="6596a-273">Обновления базы данных</span><span class="sxs-lookup"><span data-stu-id="6596a-273">Update-Database</span></span>

<span data-ttu-id="6596a-274">Обновляет базу данных, последнюю миграцию или указанного миграции.</span><span class="sxs-lookup"><span data-stu-id="6596a-274">Updates the database to the last migration or to a specified migration.</span></span>

| <span data-ttu-id="6596a-275">Параметр</span><span class="sxs-lookup"><span data-stu-id="6596a-275">Parameter</span></span>                           | <span data-ttu-id="6596a-276">Описание</span><span class="sxs-lookup"><span data-stu-id="6596a-276">Description</span></span>                                                                                                                                                                                                                                                     |
|:------------------------------------|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="6596a-277"><nobr>*-Миграции* \<строка ></nobr></span><span class="sxs-lookup"><span data-stu-id="6596a-277"><nobr>*-Migration* \<String></nobr></span></span> | <span data-ttu-id="6596a-278">Целевой объект миграции.</span><span class="sxs-lookup"><span data-stu-id="6596a-278">The target migration.</span></span> <span data-ttu-id="6596a-279">Миграция может быть идентифицирован по имени или по идентификатору.</span><span class="sxs-lookup"><span data-stu-id="6596a-279">Migrations may be identified by name or by ID.</span></span> <span data-ttu-id="6596a-280">Число 0 является особым случаем, это значит, что *перед первой миграции* и вызывает всех операций переноса для отмены изменений.</span><span class="sxs-lookup"><span data-stu-id="6596a-280">The number 0 is a special case that means *before the first migration* and causes all migrations to be reverted.</span></span> <span data-ttu-id="6596a-281">Если перенос не указан, команда по умолчанию к последней миграции.</span><span class="sxs-lookup"><span data-stu-id="6596a-281">If no migration is specified, the command defaults to the last migration.</span></span> |

> [!TIP]
> <span data-ttu-id="6596a-282">Параметр миграции поддерживает расширение функций клавиши tab.</span><span class="sxs-lookup"><span data-stu-id="6596a-282">The Migration parameter supports tab-expansion.</span></span>

<span data-ttu-id="6596a-283">Следующий пример возвращает все миграции.</span><span class="sxs-lookup"><span data-stu-id="6596a-283">The following example reverts all migrations.</span></span>

```powershell
Update-Database -Migration 0
```

<span data-ttu-id="6596a-284">Следующие примеры обновления базы данных для указанного миграции.</span><span class="sxs-lookup"><span data-stu-id="6596a-284">The following examples update the database to a specified migration.</span></span> <span data-ttu-id="6596a-285">Первый использует имя миграции, а второй использует идентификатор миграции:</span><span class="sxs-lookup"><span data-stu-id="6596a-285">The first uses the migration name and the second uses the migration ID:</span></span>

```powershell
Update-Database -Migration InitialCreate
Update-Database -Migration 20180904195021_InitialCreate
```
