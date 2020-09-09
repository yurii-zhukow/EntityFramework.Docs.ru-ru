---
title: Справочник по средствам EF Core (консоль диспетчера пакетов) — EF Core
description: Справочное руководство по консоли диспетчера пакетов Visual Studio Entity Framework Core
author: bricelam
ms.author: bricelam
ms.date: 09/18/2018
uid: core/miscellaneous/cli/powershell
ms.openlocfilehash: 84ca41dc08f7bc813ee9491b66fc91b2854c7632
ms.sourcegitcommit: 7c3939504bb9da3f46bea3443638b808c04227c2
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/09/2020
ms.locfileid: "89617861"
---
# <a name="entity-framework-core-tools-reference---package-manager-console-in-visual-studio"></a><span data-ttu-id="9e0c6-103">Справочник по средствам Entity Framework Core. консоль диспетчера пакетов в Visual Studio</span><span class="sxs-lookup"><span data-stu-id="9e0c6-103">Entity Framework Core tools reference - Package Manager Console in Visual Studio</span></span>

<span data-ttu-id="9e0c6-104">Средства консоли диспетчера пакетов (PMC) для Entity Framework Core выполнения задач разработки во время разработки.</span><span class="sxs-lookup"><span data-stu-id="9e0c6-104">The Package Manager Console (PMC) tools for Entity Framework Core perform design-time development tasks.</span></span> <span data-ttu-id="9e0c6-105">Например, они создают [миграции](/aspnet/core/data/ef-mvc/migrations), применяют миграции и создают код для модели на основе существующей базы данных.</span><span class="sxs-lookup"><span data-stu-id="9e0c6-105">For example, they create [migrations](/aspnet/core/data/ef-mvc/migrations), apply migrations, and generate code for a model based on an existing database.</span></span> <span data-ttu-id="9e0c6-106">Команды выполняются в Visual Studio с помощью [консоли диспетчера пакетов](/nuget/tools/package-manager-console).</span><span class="sxs-lookup"><span data-stu-id="9e0c6-106">The commands run inside of Visual Studio using the [Package Manager Console](/nuget/tools/package-manager-console).</span></span> <span data-ttu-id="9e0c6-107">Эти инструменты совместимы с проектами .NET Framework и .NET Core.</span><span class="sxs-lookup"><span data-stu-id="9e0c6-107">These tools work with both .NET Framework and .NET Core projects.</span></span>

<span data-ttu-id="9e0c6-108">Если вы не используете Visual Studio, вместо этого рекомендуется использовать [средства командной строки EF Core](xref:core/miscellaneous/cli/dotnet) .</span><span class="sxs-lookup"><span data-stu-id="9e0c6-108">If you aren't using Visual Studio, we recommend the [EF Core Command-line Tools](xref:core/miscellaneous/cli/dotnet) instead.</span></span> <span data-ttu-id="9e0c6-109">Средства .NET Core CLI являются кросс-платформенными и выполняются в командной строке.</span><span class="sxs-lookup"><span data-stu-id="9e0c6-109">The .NET Core CLI tools are cross-platform and run inside a command prompt.</span></span>

## <a name="installing-the-tools"></a><span data-ttu-id="9e0c6-110">Установка инструментов</span><span class="sxs-lookup"><span data-stu-id="9e0c6-110">Installing the tools</span></span>

<span data-ttu-id="9e0c6-111">Процедуры установки и обновления инструментов отличаются от ASP.NET Core 2.1 + и более ранних версий или других типов проектов.</span><span class="sxs-lookup"><span data-stu-id="9e0c6-111">The procedures for installing and updating the tools differ between ASP.NET Core 2.1+ and earlier versions or other project types.</span></span>

### <a name="aspnet-core-version-21-and-later"></a><span data-ttu-id="9e0c6-112">ASP.NET Core версии 2,1 и более поздних</span><span class="sxs-lookup"><span data-stu-id="9e0c6-112">ASP.NET Core version 2.1 and later</span></span>

<span data-ttu-id="9e0c6-113">Эти средства автоматически включаются в проект ASP.NET Core 2.1 +, так как `Microsoft.EntityFrameworkCore.Tools` пакет входит в состав [метапакет Microsoft. AspNetCore. app](/aspnet/core/fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="9e0c6-113">The tools are automatically included in an ASP.NET Core 2.1+ project because the `Microsoft.EntityFrameworkCore.Tools` package is included in the [Microsoft.AspNetCore.App metapackage](/aspnet/core/fundamentals/metapackage-app).</span></span>

<span data-ttu-id="9e0c6-114">Поэтому вам не нужно ничего делать для установки средств, но вам нужно:</span><span class="sxs-lookup"><span data-stu-id="9e0c6-114">Therefore, you don't have to do anything to install the tools, but you do have to:</span></span>

* <span data-ttu-id="9e0c6-115">Восстановите пакеты перед использованием средств в новом проекте.</span><span class="sxs-lookup"><span data-stu-id="9e0c6-115">Restore packages before using the tools in a new project.</span></span>
* <span data-ttu-id="9e0c6-116">Установите пакет, чтобы обновить средства до более новой версии.</span><span class="sxs-lookup"><span data-stu-id="9e0c6-116">Install a package to update the tools to a newer version.</span></span>

<span data-ttu-id="9e0c6-117">Чтобы убедиться, что вы получаете последнюю версию средств, мы рекомендуем также выполнить следующие действия.</span><span class="sxs-lookup"><span data-stu-id="9e0c6-117">To make sure that you're getting the latest version of the tools, we recommend that you also do the following step:</span></span>

* <span data-ttu-id="9e0c6-118">Измените *CSPROJ* -файл и добавьте строку, указывающую последнюю версию пакета [Microsoft. EntityFrameworkCore. Tools](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools/) .</span><span class="sxs-lookup"><span data-stu-id="9e0c6-118">Edit your *.csproj* file and add a line specifying the latest version of the [Microsoft.EntityFrameworkCore.Tools](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools/) package.</span></span> <span data-ttu-id="9e0c6-119">Например, файл *. csproj* может включать в себя следующий `ItemGroup` вид:</span><span class="sxs-lookup"><span data-stu-id="9e0c6-119">For example, the *.csproj* file might include an `ItemGroup` that looks like this:</span></span>

  ```xml
  <ItemGroup>
    <PackageReference Include="Microsoft.AspNetCore.App" />
    <PackageReference Include="Microsoft.EntityFrameworkCore.Tools" Version="3.1.3" />
    <PackageReference Include="Microsoft.VisualStudio.Web.CodeGeneration.Design" Version="3.1.2" />
  </ItemGroup>
  ```

<span data-ttu-id="9e0c6-120">Обновите средства при получении сообщения, как в следующем примере:</span><span class="sxs-lookup"><span data-stu-id="9e0c6-120">Update the tools when you get a message like the following example:</span></span>

> <span data-ttu-id="9e0c6-121">Версия инструментов EF Core "короче-RTM-30846" старше, чем версия среды выполнения "2.1.3-RTM-32065".</span><span class="sxs-lookup"><span data-stu-id="9e0c6-121">The EF Core tools version '2.1.1-rtm-30846' is older than that of the runtime '2.1.3-rtm-32065'.</span></span> <span data-ttu-id="9e0c6-122">Обновите средства для получения последних компонентов и исправлений ошибок.</span><span class="sxs-lookup"><span data-stu-id="9e0c6-122">Update the tools for the latest features and bug fixes.</span></span>

<span data-ttu-id="9e0c6-123">Чтобы обновить средства, выполните следующие действия.</span><span class="sxs-lookup"><span data-stu-id="9e0c6-123">To update the tools:</span></span>

* <span data-ttu-id="9e0c6-124">Установите пакет SDK для .NET Core последней версии.</span><span class="sxs-lookup"><span data-stu-id="9e0c6-124">Install the latest .NET Core SDK.</span></span>
* <span data-ttu-id="9e0c6-125">Обновите Visual Studio до последней версии.</span><span class="sxs-lookup"><span data-stu-id="9e0c6-125">Update Visual Studio to the latest version.</span></span>
* <span data-ttu-id="9e0c6-126">Измените *CSPROJ* -файл, чтобы он включал ссылку на пакет для последней версии пакета инструментов, как показано выше.</span><span class="sxs-lookup"><span data-stu-id="9e0c6-126">Edit the *.csproj* file so that it includes a package reference to the latest tools package, as shown earlier.</span></span>

### <a name="other-versions-and-project-types"></a><span data-ttu-id="9e0c6-127">Другие версии и типы проектов</span><span class="sxs-lookup"><span data-stu-id="9e0c6-127">Other versions and project types</span></span>

<span data-ttu-id="9e0c6-128">Установите средства консоли диспетчера пакетов, выполнив следующую команду в **консоли диспетчера пакетов**:</span><span class="sxs-lookup"><span data-stu-id="9e0c6-128">Install the Package Manager Console tools by running the following command in **Package Manager Console**:</span></span>

``` powershell
Install-Package Microsoft.EntityFrameworkCore.Tools
```

<span data-ttu-id="9e0c6-129">Обновите средства, выполнив следующую команду в **консоли диспетчера пакетов**.</span><span class="sxs-lookup"><span data-stu-id="9e0c6-129">Update the tools by running the following command in **Package Manager Console**.</span></span>

``` powershell
Update-Package Microsoft.EntityFrameworkCore.Tools
```

### <a name="verify-the-installation"></a><span data-ttu-id="9e0c6-130">Проверка установки</span><span class="sxs-lookup"><span data-stu-id="9e0c6-130">Verify the installation</span></span>

<span data-ttu-id="9e0c6-131">Убедитесь, что средства установлены, выполнив следующую команду:</span><span class="sxs-lookup"><span data-stu-id="9e0c6-131">Verify that the tools are installed by running this command:</span></span>

``` powershell
Get-Help about_EntityFrameworkCore
```

<span data-ttu-id="9e0c6-132">Выходные данные выглядят следующим образом (он не указывает, какую версию инструментов вы используете):</span><span class="sxs-lookup"><span data-stu-id="9e0c6-132">The output looks like this (it doesn't tell you which version of the tools you're using):</span></span>

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

## <a name="using-the-tools"></a><span data-ttu-id="9e0c6-133">Использование средств</span><span class="sxs-lookup"><span data-stu-id="9e0c6-133">Using the tools</span></span>

<span data-ttu-id="9e0c6-134">Перед использованием средств:</span><span class="sxs-lookup"><span data-stu-id="9e0c6-134">Before using the tools:</span></span>

* <span data-ttu-id="9e0c6-135">Изучите разницу между целевым и запускаемым проектами.</span><span class="sxs-lookup"><span data-stu-id="9e0c6-135">Understand the difference between target and startup project.</span></span>
* <span data-ttu-id="9e0c6-136">Узнайте, как использовать средства с библиотеками классов .NET Standard.</span><span class="sxs-lookup"><span data-stu-id="9e0c6-136">Learn how to use the tools with .NET Standard class libraries.</span></span>
* <span data-ttu-id="9e0c6-137">Для проектов ASP.NET Core задайте среду.</span><span class="sxs-lookup"><span data-stu-id="9e0c6-137">For ASP.NET Core projects, set the environment.</span></span>

### <a name="target-and-startup-project"></a><span data-ttu-id="9e0c6-138">Целевой и запускаемый проекты</span><span class="sxs-lookup"><span data-stu-id="9e0c6-138">Target and startup project</span></span>

<span data-ttu-id="9e0c6-139">Команды ссылаются на *проект* и *запускаемый проект*.</span><span class="sxs-lookup"><span data-stu-id="9e0c6-139">The commands refer to a *project* and a *startup project*.</span></span>

* <span data-ttu-id="9e0c6-140">*Проект* также называется *целевым проектом* , так как он служит для добавления или удаления файлов.</span><span class="sxs-lookup"><span data-stu-id="9e0c6-140">The *project* is also known as the *target project* because it's where the commands add or remove files.</span></span> <span data-ttu-id="9e0c6-141">По умолчанию **проект** , выбранный в **консоли диспетчера пакетов** , является целевым.</span><span class="sxs-lookup"><span data-stu-id="9e0c6-141">By default, the **Default project** selected in **Package Manager Console** is the target project.</span></span> <span data-ttu-id="9e0c6-142">Можно указать другой проект в качестве целевого проекта с помощью <nobr>`--project`</nobr> параметра.</span><span class="sxs-lookup"><span data-stu-id="9e0c6-142">You can specify a different project as target project by using the <nobr>`--project`</nobr> option.</span></span>

* <span data-ttu-id="9e0c6-143">*Запускаемый проект* — это тот, который выполняет сборка и запуск средств.</span><span class="sxs-lookup"><span data-stu-id="9e0c6-143">The *startup project* is the one that the tools build and run.</span></span> <span data-ttu-id="9e0c6-144">Средства должны выполнять код приложения во время разработки для получения сведений о проекте, таких как строка подключения к базе данных и Конфигурация модели.</span><span class="sxs-lookup"><span data-stu-id="9e0c6-144">The tools have to execute application code at design time to get information about the project, such as the database connection string and the configuration of the model.</span></span> <span data-ttu-id="9e0c6-145">По умолчанию **запускаемым проектом** в **Обозреватель решений** является запускаемый проект.</span><span class="sxs-lookup"><span data-stu-id="9e0c6-145">By default, the **Startup Project** in **Solution Explorer** is the startup project.</span></span> <span data-ttu-id="9e0c6-146">Можно указать другой проект в качестве запускаемого проекта с помощью <nobr>`--startup-project`</nobr> параметра.</span><span class="sxs-lookup"><span data-stu-id="9e0c6-146">You can specify a different project as startup project by using the <nobr>`--startup-project`</nobr> option.</span></span>

<span data-ttu-id="9e0c6-147">Запускаемый проект и целевой проект часто являются одним и тем же проектом.</span><span class="sxs-lookup"><span data-stu-id="9e0c6-147">The startup project and target project are often the same project.</span></span> <span data-ttu-id="9e0c6-148">Типичный сценарий, в котором они являются отдельными проектами, состоит в следующих случаях:</span><span class="sxs-lookup"><span data-stu-id="9e0c6-148">A typical scenario where they are separate projects is when:</span></span>

* <span data-ttu-id="9e0c6-149">Контекст EF Core и классы сущностей находятся в библиотеке классов .NET Core.</span><span class="sxs-lookup"><span data-stu-id="9e0c6-149">The EF Core context and entity classes are in a .NET Core class library.</span></span>
* <span data-ttu-id="9e0c6-150">Консольное приложение .NET Core или веб-приложение ссылается на библиотеку классов.</span><span class="sxs-lookup"><span data-stu-id="9e0c6-150">A .NET Core console app or web app references the class library.</span></span>

<span data-ttu-id="9e0c6-151">Также можно [разместить код миграции в библиотеке классов, отдельном от контекста EF Core](xref:core/managing-schemas/migrations/projects).</span><span class="sxs-lookup"><span data-stu-id="9e0c6-151">It's also possible to [put migrations code in a class library separate from the EF Core context](xref:core/managing-schemas/migrations/projects).</span></span>

### <a name="other-target-frameworks"></a><span data-ttu-id="9e0c6-152">Другие целевые платформы</span><span class="sxs-lookup"><span data-stu-id="9e0c6-152">Other target frameworks</span></span>

<span data-ttu-id="9e0c6-153">Средства консоли диспетчера пакетов работают с проектами .NET Core или .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="9e0c6-153">The Package Manager Console tools work with .NET Core or .NET Framework projects.</span></span> <span data-ttu-id="9e0c6-154">Приложения с моделью EF Core в библиотеке классов .NET Standard могут не иметь проекта .NET Core или .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="9e0c6-154">Apps that have the EF Core model in a .NET Standard class library might not have a .NET Core or .NET Framework project.</span></span> <span data-ttu-id="9e0c6-155">Например, это справедливо для приложений Xamarin и универсальная платформа Windows.</span><span class="sxs-lookup"><span data-stu-id="9e0c6-155">For example, this is true of Xamarin and Universal Windows Platform apps.</span></span> <span data-ttu-id="9e0c6-156">В таких случаях можно создать проект консольного приложения .NET Core или .NET Framework, предназначенное только для запуска в качестве запускаемого проекта для инструментов.</span><span class="sxs-lookup"><span data-stu-id="9e0c6-156">In such cases, you can create a .NET Core or .NET Framework console app project whose only purpose is to act as startup project for the tools.</span></span> <span data-ttu-id="9e0c6-157">Проект может быть фиктивным проектом без реального кода, &mdash; поэтому он необходим только для предоставления целей для инструментов.</span><span class="sxs-lookup"><span data-stu-id="9e0c6-157">The project can be a dummy project with no real code &mdash; it is only needed to provide a target for the tooling.</span></span>

<span data-ttu-id="9e0c6-158">Зачем нужен фиктивный проект?</span><span class="sxs-lookup"><span data-stu-id="9e0c6-158">Why is a dummy project required?</span></span> <span data-ttu-id="9e0c6-159">Как упоминалось ранее, средства должны выполнять код приложения во время разработки.</span><span class="sxs-lookup"><span data-stu-id="9e0c6-159">As mentioned earlier, the tools have to execute application code at design time.</span></span> <span data-ttu-id="9e0c6-160">Для этого необходимо использовать среду выполнения .NET Core или .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="9e0c6-160">To do that, they need to use the .NET Core or .NET Framework runtime.</span></span> <span data-ttu-id="9e0c6-161">Если EF Coreная модель находится в проекте, ориентированном на .NET Core или .NET Framework, EF Core средства позаимствованы среду выполнения из проекта.</span><span class="sxs-lookup"><span data-stu-id="9e0c6-161">When the EF Core model is in a project that targets .NET Core or .NET Framework, the EF Core tools borrow the runtime from the project.</span></span> <span data-ttu-id="9e0c6-162">Они не могут сделать это, если модель EF Core находится в библиотеке классов .NET Standard.</span><span class="sxs-lookup"><span data-stu-id="9e0c6-162">They can't do that if the EF Core model is in a .NET Standard class library.</span></span> <span data-ttu-id="9e0c6-163">.NET Standard не является реальной реализацией .NET; это спецификация набора интерфейсов API, которые должны поддерживаться реализациями .NET.</span><span class="sxs-lookup"><span data-stu-id="9e0c6-163">The .NET Standard is not an actual .NET implementation; it's a specification of a set of APIs that .NET implementations must support.</span></span> <span data-ttu-id="9e0c6-164">Поэтому .NET Standard недостаточно для того, чтобы средства EF Core выполняли код приложения.</span><span class="sxs-lookup"><span data-stu-id="9e0c6-164">Therefore .NET Standard is not sufficient for the EF Core tools to execute application code.</span></span> <span data-ttu-id="9e0c6-165">Фиктивный проект, который создается для использования в качестве запускаемого проекта, предоставляет конкретную целевую платформу, в которую средства могут загрузить библиотеку классов .NET Standard.</span><span class="sxs-lookup"><span data-stu-id="9e0c6-165">The dummy project you create to use as startup project provides a concrete target platform into which the tools can load the .NET Standard class library.</span></span>

### <a name="aspnet-core-environment"></a><span data-ttu-id="9e0c6-166">Среда ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="9e0c6-166">ASP.NET Core environment</span></span>

<span data-ttu-id="9e0c6-167">Чтобы указать среду для проектов ASP.NET Core, установите параметр **env: ASPNETCORE_ENVIRONMENT** перед выполнением команд.</span><span class="sxs-lookup"><span data-stu-id="9e0c6-167">To specify the environment for ASP.NET Core projects, set **env:ASPNETCORE_ENVIRONMENT** before running commands.</span></span>

## <a name="common-parameters"></a><span data-ttu-id="9e0c6-168">Общие параметры</span><span class="sxs-lookup"><span data-stu-id="9e0c6-168">Common parameters</span></span>

<span data-ttu-id="9e0c6-169">В следующей таблице приведены параметры, которые являются общими для всех EF Coreных команд.</span><span class="sxs-lookup"><span data-stu-id="9e0c6-169">The following table shows parameters that are common to all of the EF Core commands:</span></span>

| <span data-ttu-id="9e0c6-170">Параметр</span><span class="sxs-lookup"><span data-stu-id="9e0c6-170">Parameter</span></span>                 | <span data-ttu-id="9e0c6-171">Описание</span><span class="sxs-lookup"><span data-stu-id="9e0c6-171">Description</span></span>                                                                                                                                                                                                          |
|:--------------------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="9e0c6-172">Контекст \<String></span><span class="sxs-lookup"><span data-stu-id="9e0c6-172">-Context \<String></span></span>        | <span data-ttu-id="9e0c6-173">Класс `DbContext` для использования.</span><span class="sxs-lookup"><span data-stu-id="9e0c6-173">The `DbContext` class to use.</span></span> <span data-ttu-id="9e0c6-174">Только имя класса или полный квалификатор с пространствами имен.</span><span class="sxs-lookup"><span data-stu-id="9e0c6-174">Class name only or fully qualified with namespaces.</span></span>  <span data-ttu-id="9e0c6-175">Если этот параметр опущен, EF Core находит класс контекста.</span><span class="sxs-lookup"><span data-stu-id="9e0c6-175">If this parameter is omitted, EF Core finds the context class.</span></span> <span data-ttu-id="9e0c6-176">Если имеется несколько классов контекста, этот параметр является обязательным.</span><span class="sxs-lookup"><span data-stu-id="9e0c6-176">If there are multiple context classes, this parameter is required.</span></span> |
| <span data-ttu-id="9e0c6-177">-Проект \<String></span><span class="sxs-lookup"><span data-stu-id="9e0c6-177">-Project \<String></span></span>        | <span data-ttu-id="9e0c6-178">Целевой проект.</span><span class="sxs-lookup"><span data-stu-id="9e0c6-178">The target project.</span></span> <span data-ttu-id="9e0c6-179">Если этот параметр не указан, в качестве целевого проекта используется **проект по умолчанию** для **консоли диспетчера пакетов** .</span><span class="sxs-lookup"><span data-stu-id="9e0c6-179">If this parameter is omitted, the **Default project** for **Package Manager Console** is used as the target project.</span></span>                                                                             |
| <span data-ttu-id="9e0c6-180"><nobr>-Стартуппрожект</nobr>\<String></span><span class="sxs-lookup"><span data-stu-id="9e0c6-180"><nobr>-StartupProject</nobr> \<String></span></span> | <span data-ttu-id="9e0c6-181">Автоматически запускаемый проект.</span><span class="sxs-lookup"><span data-stu-id="9e0c6-181">The startup project.</span></span> <span data-ttu-id="9e0c6-182">Если этот параметр не указан, в качестве целевого проекта используется **запускаемый проект** в **свойствах решения** .</span><span class="sxs-lookup"><span data-stu-id="9e0c6-182">If this parameter is omitted, the **Startup project** in **Solution properties** is used as the target project.</span></span>                                                                                 |
| <span data-ttu-id="9e0c6-183">-Verbose</span><span class="sxs-lookup"><span data-stu-id="9e0c6-183">-Verbose</span></span>                  | <span data-ttu-id="9e0c6-184">Отображение подробных выходных данных.</span><span class="sxs-lookup"><span data-stu-id="9e0c6-184">Show verbose output.</span></span>                                                                                                                                                                                                 |

<span data-ttu-id="9e0c6-185">Чтобы отобразить справочные сведения о команде, используйте команду PowerShell `Get-Help` .</span><span class="sxs-lookup"><span data-stu-id="9e0c6-185">To show help information about a command, use PowerShell's `Get-Help` command.</span></span>

> [!TIP]
> <span data-ttu-id="9e0c6-186">Параметры Context, Project и Стартуппрожект поддерживают расширение Tab.</span><span class="sxs-lookup"><span data-stu-id="9e0c6-186">The Context, Project, and StartupProject parameters support tab-expansion.</span></span>

## <a name="add-migration"></a><span data-ttu-id="9e0c6-187">Добавление и миграция</span><span class="sxs-lookup"><span data-stu-id="9e0c6-187">Add-Migration</span></span>

<span data-ttu-id="9e0c6-188">Добавляет новый перенос.</span><span class="sxs-lookup"><span data-stu-id="9e0c6-188">Adds a new migration.</span></span>

<span data-ttu-id="9e0c6-189">Параметры</span><span class="sxs-lookup"><span data-stu-id="9e0c6-189">Parameters:</span></span>

| <span data-ttu-id="9e0c6-190">Параметр</span><span class="sxs-lookup"><span data-stu-id="9e0c6-190">Parameter</span></span>                         | <span data-ttu-id="9e0c6-191">Описание</span><span class="sxs-lookup"><span data-stu-id="9e0c6-191">Description</span></span>                                                                                                             |
|:----------------------------------|:------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="9e0c6-192"><nobr>-Имя \<String><nobr></span><span class="sxs-lookup"><span data-stu-id="9e0c6-192"><nobr>-Name \<String><nobr></span></span>       | <span data-ttu-id="9e0c6-193">Имя миграции.</span><span class="sxs-lookup"><span data-stu-id="9e0c6-193">The name of the migration.</span></span> <span data-ttu-id="9e0c6-194">Этот параметр является позиционированным и является обязательным.</span><span class="sxs-lookup"><span data-stu-id="9e0c6-194">This is a positional parameter and is required.</span></span>                                              |
| <span data-ttu-id="9e0c6-195"><nobr>-OutputDir \<String></nobr></span><span class="sxs-lookup"><span data-stu-id="9e0c6-195"><nobr>-OutputDir \<String></nobr></span></span> | <span data-ttu-id="9e0c6-196">Каталог используется для вывода файлов.</span><span class="sxs-lookup"><span data-stu-id="9e0c6-196">The directory use to output the files.</span></span> <span data-ttu-id="9e0c6-197">Пути задаются относительно целевого каталога проекта.</span><span class="sxs-lookup"><span data-stu-id="9e0c6-197">Paths are relative to the target project directory.</span></span> <span data-ttu-id="9e0c6-198">По умолчанию используется значение "миграции".</span><span class="sxs-lookup"><span data-stu-id="9e0c6-198">Defaults to "Migrations".</span></span> |
| <span data-ttu-id="9e0c6-199"><nobr>-Namespace \<String></nobr></span><span class="sxs-lookup"><span data-stu-id="9e0c6-199"><nobr>-Namespace \<String></nobr></span></span> | <span data-ttu-id="9e0c6-200">Пространство имен, используемое для создаваемых классов.</span><span class="sxs-lookup"><span data-stu-id="9e0c6-200">The namespace to use for the generated classes.</span></span> <span data-ttu-id="9e0c6-201">По умолчанию создается из выходного каталога.</span><span class="sxs-lookup"><span data-stu-id="9e0c6-201">Defaults to generated from the output directory.</span></span> <span data-ttu-id="9e0c6-202">(Доступно из Ефкоре 5.0.0 и выше.)</span><span class="sxs-lookup"><span data-stu-id="9e0c6-202">(Available from EFCore 5.0.0 onwards.)</span></span> |

## <a name="drop-database"></a><span data-ttu-id="9e0c6-203">Удалить базу данных</span><span class="sxs-lookup"><span data-stu-id="9e0c6-203">Drop-Database</span></span>

<span data-ttu-id="9e0c6-204">Удаляет базу данных.</span><span class="sxs-lookup"><span data-stu-id="9e0c6-204">Drops the database.</span></span>

<span data-ttu-id="9e0c6-205">Параметры</span><span class="sxs-lookup"><span data-stu-id="9e0c6-205">Parameters:</span></span>

| <span data-ttu-id="9e0c6-206">Параметр</span><span class="sxs-lookup"><span data-stu-id="9e0c6-206">Parameter</span></span> | <span data-ttu-id="9e0c6-207">Описание</span><span class="sxs-lookup"><span data-stu-id="9e0c6-207">Description</span></span>                                              |
|:----------|:---------------------------------------------------------|
| <span data-ttu-id="9e0c6-208">-WhatIf</span><span class="sxs-lookup"><span data-stu-id="9e0c6-208">-WhatIf</span></span>   | <span data-ttu-id="9e0c6-209">Показывает, какую базу данных следует удалить, но не удалять.</span><span class="sxs-lookup"><span data-stu-id="9e0c6-209">Show which database would be dropped, but don't drop it.</span></span> |

## <a name="get-dbcontext"></a><span data-ttu-id="9e0c6-210">Get-DbContext</span><span class="sxs-lookup"><span data-stu-id="9e0c6-210">Get-DbContext</span></span>

<span data-ttu-id="9e0c6-211">Возвращает сведения о `DbContext` типе.</span><span class="sxs-lookup"><span data-stu-id="9e0c6-211">Gets information about a `DbContext` type.</span></span>

## <a name="remove-migration"></a><span data-ttu-id="9e0c6-212">Remove-Migration</span><span class="sxs-lookup"><span data-stu-id="9e0c6-212">Remove-Migration</span></span>

<span data-ttu-id="9e0c6-213">Удаляет последнюю миграцию (выполняет откат изменений кода, выполненных для миграции).</span><span class="sxs-lookup"><span data-stu-id="9e0c6-213">Removes the last migration (rolls back the code changes that were done for the migration).</span></span>

<span data-ttu-id="9e0c6-214">Параметры</span><span class="sxs-lookup"><span data-stu-id="9e0c6-214">Parameters:</span></span>

| <span data-ttu-id="9e0c6-215">Параметр</span><span class="sxs-lookup"><span data-stu-id="9e0c6-215">Parameter</span></span> | <span data-ttu-id="9e0c6-216">Описание</span><span class="sxs-lookup"><span data-stu-id="9e0c6-216">Description</span></span>                                                                     |
|:----------|:--------------------------------------------------------------------------------|
| <span data-ttu-id="9e0c6-217">-Force</span><span class="sxs-lookup"><span data-stu-id="9e0c6-217">-Force</span></span>    | <span data-ttu-id="9e0c6-218">Отмените миграцию (выполните откат изменений, примененных к базе данных).</span><span class="sxs-lookup"><span data-stu-id="9e0c6-218">Revert the migration (roll back the changes that were applied to the database).</span></span> |

## <a name="scaffold-dbcontext"></a><span data-ttu-id="9e0c6-219">Формирование шаблонов — DbContext</span><span class="sxs-lookup"><span data-stu-id="9e0c6-219">Scaffold-DbContext</span></span>

<span data-ttu-id="9e0c6-220">Создает код для `DbContext` типов сущностей и для базы данных.</span><span class="sxs-lookup"><span data-stu-id="9e0c6-220">Generates code for a `DbContext` and entity types for a database.</span></span> <span data-ttu-id="9e0c6-221">`Scaffold-DbContext`Чтобы создать тип сущности, таблица базы данных должна иметь первичный ключ.</span><span class="sxs-lookup"><span data-stu-id="9e0c6-221">In order for `Scaffold-DbContext` to generate an entity type, the database table must have a primary key.</span></span>

<span data-ttu-id="9e0c6-222">Параметры</span><span class="sxs-lookup"><span data-stu-id="9e0c6-222">Parameters:</span></span>

| <span data-ttu-id="9e0c6-223">Параметр</span><span class="sxs-lookup"><span data-stu-id="9e0c6-223">Parameter</span></span>                          | <span data-ttu-id="9e0c6-224">Описание</span><span class="sxs-lookup"><span data-stu-id="9e0c6-224">Description</span></span>                                                                                                                                                                                                                                                             |
|:-----------------------------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="9e0c6-225"><nobr>— Подключение \<String></nobr></span><span class="sxs-lookup"><span data-stu-id="9e0c6-225"><nobr>-Connection \<String></nobr></span></span> | <span data-ttu-id="9e0c6-226">Строка подключения к базе данных.</span><span class="sxs-lookup"><span data-stu-id="9e0c6-226">The connection string to the database.</span></span> <span data-ttu-id="9e0c6-227">Для проектов ASP.NET Core 2. x значением может быть \*Name = \<name of connection string> \*.</span><span class="sxs-lookup"><span data-stu-id="9e0c6-227">For ASP.NET Core 2.x projects, the value can be *name=\<name of connection string>*.</span></span> <span data-ttu-id="9e0c6-228">В этом случае имя берется из источников конфигурации, настроенных для проекта.</span><span class="sxs-lookup"><span data-stu-id="9e0c6-228">In that case the name comes from the configuration sources that are set up for the project.</span></span> <span data-ttu-id="9e0c6-229">Этот параметр является позиционированным и является обязательным.</span><span class="sxs-lookup"><span data-stu-id="9e0c6-229">This is a positional parameter and is required.</span></span> |
| <span data-ttu-id="9e0c6-230"><nobr>— Поставщик \<String></nobr></span><span class="sxs-lookup"><span data-stu-id="9e0c6-230"><nobr>-Provider \<String></nobr></span></span>   | <span data-ttu-id="9e0c6-231">Используемый поставщик.</span><span class="sxs-lookup"><span data-stu-id="9e0c6-231">The provider to use.</span></span> <span data-ttu-id="9e0c6-232">Обычно это имя пакета NuGet, например: `Microsoft.EntityFrameworkCore.SqlServer` .</span><span class="sxs-lookup"><span data-stu-id="9e0c6-232">Typically this is the name of the NuGet package, for example: `Microsoft.EntityFrameworkCore.SqlServer`.</span></span> <span data-ttu-id="9e0c6-233">Этот параметр является позиционированным и является обязательным.</span><span class="sxs-lookup"><span data-stu-id="9e0c6-233">This is a positional parameter and is required.</span></span>                                                                                           |
| <span data-ttu-id="9e0c6-234">-OutputDir \<String></span><span class="sxs-lookup"><span data-stu-id="9e0c6-234">-OutputDir \<String></span></span>               | <span data-ttu-id="9e0c6-235">Каталог для размещения файлов.</span><span class="sxs-lookup"><span data-stu-id="9e0c6-235">The directory to put files in.</span></span> <span data-ttu-id="9e0c6-236">Пути задаются относительно каталога проекта.</span><span class="sxs-lookup"><span data-stu-id="9e0c6-236">Paths are relative to the project directory.</span></span>                                                                                                                                                                                             |
| <span data-ttu-id="9e0c6-237">-Контекстдир \<String></span><span class="sxs-lookup"><span data-stu-id="9e0c6-237">-ContextDir \<String></span></span>              | <span data-ttu-id="9e0c6-238">Каталог, в котором следует разместить `DbContext` файл.</span><span class="sxs-lookup"><span data-stu-id="9e0c6-238">The directory to put the `DbContext` file in.</span></span> <span data-ttu-id="9e0c6-239">Пути задаются относительно каталога проекта.</span><span class="sxs-lookup"><span data-stu-id="9e0c6-239">Paths are relative to the project directory.</span></span>                                                                                                                                                               |
| <span data-ttu-id="9e0c6-240">-Namespace \<String></span><span class="sxs-lookup"><span data-stu-id="9e0c6-240">-Namespace \<String></span></span>               | <span data-ttu-id="9e0c6-241">Пространство имен, используемое для всех создаваемых классов.</span><span class="sxs-lookup"><span data-stu-id="9e0c6-241">The namespace to use for all generated classes.</span></span> <span data-ttu-id="9e0c6-242">По умолчанию создается из корневого пространства имен и выходного каталога.</span><span class="sxs-lookup"><span data-stu-id="9e0c6-242">Defaults to generated from the root namespace and the output directory.</span></span> <span data-ttu-id="9e0c6-243">(Доступно из Ефкоре 5.0.0 и выше.)</span><span class="sxs-lookup"><span data-stu-id="9e0c6-243">(Available from EFCore 5.0.0 onwards.)</span></span> |
| <span data-ttu-id="9e0c6-244">-ContextNamespace \<String></span><span class="sxs-lookup"><span data-stu-id="9e0c6-244">-ContextNamespace \<String></span></span>        | <span data-ttu-id="9e0c6-245">Пространство имен, используемое для создаваемого `DbContext` класса.</span><span class="sxs-lookup"><span data-stu-id="9e0c6-245">The namespace to use for the generated `DbContext` class.</span></span> <span data-ttu-id="9e0c6-246">Примечание. Переопределение `-Namespace` .</span><span class="sxs-lookup"><span data-stu-id="9e0c6-246">Note: overrides `-Namespace`.</span></span> <span data-ttu-id="9e0c6-247">(Доступно из Ефкоре 5.0.0 и выше.)</span><span class="sxs-lookup"><span data-stu-id="9e0c6-247">(Available from EFCore 5.0.0 onwards.)</span></span> |
| <span data-ttu-id="9e0c6-248">Контекст \<String></span><span class="sxs-lookup"><span data-stu-id="9e0c6-248">-Context \<String></span></span>                 | <span data-ttu-id="9e0c6-249">Имя `DbContext` создаваемого класса.</span><span class="sxs-lookup"><span data-stu-id="9e0c6-249">The name of the `DbContext` class to generate.</span></span>                                                                                                                                                                                                                          |
| <span data-ttu-id="9e0c6-250">-Schemas \<String[]></span><span class="sxs-lookup"><span data-stu-id="9e0c6-250">-Schemas \<String[]></span></span>               | <span data-ttu-id="9e0c6-251">Схемы таблиц, для которых создаются типы сущностей.</span><span class="sxs-lookup"><span data-stu-id="9e0c6-251">The schemas of tables to generate entity types for.</span></span> <span data-ttu-id="9e0c6-252">Если этот параметр пропущен, включаются все схемы.</span><span class="sxs-lookup"><span data-stu-id="9e0c6-252">If this parameter is omitted, all schemas are included.</span></span>                                                                                                                                                             |
| <span data-ttu-id="9e0c6-253">-Таблицы \<String[]></span><span class="sxs-lookup"><span data-stu-id="9e0c6-253">-Tables \<String[]></span></span>                | <span data-ttu-id="9e0c6-254">Таблицы, для которых создаются типы сущностей.</span><span class="sxs-lookup"><span data-stu-id="9e0c6-254">The tables to generate entity types for.</span></span> <span data-ttu-id="9e0c6-255">Если этот параметр пропущен, включаются все таблицы.</span><span class="sxs-lookup"><span data-stu-id="9e0c6-255">If this parameter is omitted, all tables are included.</span></span>                                                                                                                                                                         |
| <span data-ttu-id="9e0c6-256">-Аннотации</span><span class="sxs-lookup"><span data-stu-id="9e0c6-256">-DataAnnotations</span></span>                   | <span data-ttu-id="9e0c6-257">Используйте атрибуты для настройки модели (по возможности).</span><span class="sxs-lookup"><span data-stu-id="9e0c6-257">Use attributes to configure the model (where possible).</span></span> <span data-ttu-id="9e0c6-258">Если этот параметр пропущен, используется только API-интерфейс Fluent.</span><span class="sxs-lookup"><span data-stu-id="9e0c6-258">If this parameter is omitted, only the fluent API is used.</span></span>                                                                                                                                                      |
| <span data-ttu-id="9e0c6-259">-Уседатабасенамес</span><span class="sxs-lookup"><span data-stu-id="9e0c6-259">-UseDatabaseNames</span></span>                  | <span data-ttu-id="9e0c6-260">Имена таблиц и столбцов следует использовать в точности так, как они отображаются в базе данных.</span><span class="sxs-lookup"><span data-stu-id="9e0c6-260">Use table and column names exactly as they appear in the database.</span></span> <span data-ttu-id="9e0c6-261">Если этот параметр опущен, имена баз данных изменяются в соответствии с соглашениями о стиле имени C#.</span><span class="sxs-lookup"><span data-stu-id="9e0c6-261">If this parameter is omitted, database names are changed to more closely conform to C# name style conventions.</span></span>                                                                                       |
| <span data-ttu-id="9e0c6-262">-Force</span><span class="sxs-lookup"><span data-stu-id="9e0c6-262">-Force</span></span>                             | <span data-ttu-id="9e0c6-263">Перезаписать существующие файлы.</span><span class="sxs-lookup"><span data-stu-id="9e0c6-263">Overwrite existing files.</span></span>                                                                                                                                                                                                                                               |
| <span data-ttu-id="9e0c6-264">-Нунконфигуринг</span><span class="sxs-lookup"><span data-stu-id="9e0c6-264">-NoOnConfiguring</span></span>                   | <span data-ttu-id="9e0c6-265">Подавляет создание `OnConfiguring` метода в созданном `DbContext` классе.</span><span class="sxs-lookup"><span data-stu-id="9e0c6-265">Suppresses generation of the `OnConfiguring` method in the generated `DbContext` class.</span></span> <span data-ttu-id="9e0c6-266">(Доступно из Ефкоре 5.0.0 и выше.)</span><span class="sxs-lookup"><span data-stu-id="9e0c6-266">(Available from EFCore 5.0.0 onwards.)</span></span> |

<span data-ttu-id="9e0c6-267">Пример.</span><span class="sxs-lookup"><span data-stu-id="9e0c6-267">Example:</span></span>

```powershell
Scaffold-DbContext "Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer -OutputDir Models
```

<span data-ttu-id="9e0c6-268">Пример: формирование шаблонов только для выбранных таблиц и создание контекста в отдельной папке с указанным именем и пространством имен.</span><span class="sxs-lookup"><span data-stu-id="9e0c6-268">Example that scaffolds only selected tables and creates the context in a separate folder with a specified name and namespace:</span></span>

```powershell
Scaffold-DbContext "Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer -OutputDir Models -Tables "Blog","Post" -ContextDir Context -Context BlogContext -ContextNamespace New.Namespace
```

## <a name="script-migration"></a><span data-ttu-id="9e0c6-269">Скрипт — миграция</span><span class="sxs-lookup"><span data-stu-id="9e0c6-269">Script-Migration</span></span>

<span data-ttu-id="9e0c6-270">Создает скрипт SQL, который применяет все изменения из одной выбранной миграции в другую выбранную миграцию.</span><span class="sxs-lookup"><span data-stu-id="9e0c6-270">Generates a SQL script that applies all of the changes from one selected migration to another selected migration.</span></span>

<span data-ttu-id="9e0c6-271">Параметры</span><span class="sxs-lookup"><span data-stu-id="9e0c6-271">Parameters:</span></span>

| <span data-ttu-id="9e0c6-272">Параметр</span><span class="sxs-lookup"><span data-stu-id="9e0c6-272">Parameter</span></span>                | <span data-ttu-id="9e0c6-273">Описание</span><span class="sxs-lookup"><span data-stu-id="9e0c6-273">Description</span></span>                                                                                                                                                                                                                |
|:-------------------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="9e0c6-274">*— От*\<String></span><span class="sxs-lookup"><span data-stu-id="9e0c6-274">*-From* \<String></span></span>        | <span data-ttu-id="9e0c6-275">Начало миграции.</span><span class="sxs-lookup"><span data-stu-id="9e0c6-275">The starting migration.</span></span> <span data-ttu-id="9e0c6-276">Миграция может быть идентифицирована по имени или по ИДЕНТИФИКАТОРу.</span><span class="sxs-lookup"><span data-stu-id="9e0c6-276">Migrations may be identified by name or by ID.</span></span> <span data-ttu-id="9e0c6-277">Число 0 — это особый случай, который означает *перед первой миграцией*.</span><span class="sxs-lookup"><span data-stu-id="9e0c6-277">The number 0 is a special case that means *before the first migration*.</span></span> <span data-ttu-id="9e0c6-278">Значение по умолчанию — 0.</span><span class="sxs-lookup"><span data-stu-id="9e0c6-278">Defaults to 0.</span></span>                                                              |
| <span data-ttu-id="9e0c6-279">*— Для*\<String></span><span class="sxs-lookup"><span data-stu-id="9e0c6-279">*-To* \<String></span></span>          | <span data-ttu-id="9e0c6-280">Завершение миграции.</span><span class="sxs-lookup"><span data-stu-id="9e0c6-280">The ending migration.</span></span> <span data-ttu-id="9e0c6-281">По умолчанию используется последняя миграция.</span><span class="sxs-lookup"><span data-stu-id="9e0c6-281">Defaults to the last migration.</span></span>                                                                                                                                                                      |
| <span data-ttu-id="9e0c6-282"><nobr>-Идемпотентными</nobr></span><span class="sxs-lookup"><span data-stu-id="9e0c6-282"><nobr>-Idempotent</nobr></span></span> | <span data-ttu-id="9e0c6-283">Создание скрипта, который может использоваться в базе данных при любой миграции.</span><span class="sxs-lookup"><span data-stu-id="9e0c6-283">Generate a script that can be used on a database at any migration.</span></span>                                                                                                                                                         |
| <span data-ttu-id="9e0c6-284">-Output \<String></span><span class="sxs-lookup"><span data-stu-id="9e0c6-284">-Output \<String></span></span>        | <span data-ttu-id="9e0c6-285">Файл, в который записывается результат.</span><span class="sxs-lookup"><span data-stu-id="9e0c6-285">The file to write the result to.</span></span> <span data-ttu-id="9e0c6-286">Если этот параметр не указан, файл создается с именем, созданным в той же папке, что и файлы среды выполнения приложения, например: */obj/Debug/netcoreapp2.1/ghbkztfz.SQL/*.</span><span class="sxs-lookup"><span data-stu-id="9e0c6-286">IF this parameter is omitted, the file is created with a generated name in the same folder as the app's runtime files are created, for example: */obj/Debug/netcoreapp2.1/ghbkztfz.sql/*.</span></span> |

> [!TIP]
> <span data-ttu-id="9e0c6-287">Параметры to, from и Output поддерживают расширение клавишей TAB.</span><span class="sxs-lookup"><span data-stu-id="9e0c6-287">The To, From, and Output parameters support tab-expansion.</span></span>

<span data-ttu-id="9e0c6-288">В следующем примере создается скрипт для миграции InitialCreate с использованием имени миграции.</span><span class="sxs-lookup"><span data-stu-id="9e0c6-288">The following example creates a script for the InitialCreate migration, using the migration name.</span></span>

```powershell
Script-Migration -To InitialCreate
```

<span data-ttu-id="9e0c6-289">В следующем примере создается скрипт для всех миграций после миграции InitialCreate с использованием идентификатора миграции.</span><span class="sxs-lookup"><span data-stu-id="9e0c6-289">The following example creates a script for all migrations after the InitialCreate migration, using the migration ID.</span></span>

```powershell
Script-Migration -From 20180904195021_InitialCreate
```

## <a name="update-database"></a><span data-ttu-id="9e0c6-290">Обновление базы данных</span><span class="sxs-lookup"><span data-stu-id="9e0c6-290">Update-Database</span></span>

<span data-ttu-id="9e0c6-291">Обновляет базу данных до последней миграции или до указанной миграции.</span><span class="sxs-lookup"><span data-stu-id="9e0c6-291">Updates the database to the last migration or to a specified migration.</span></span>

| <span data-ttu-id="9e0c6-292">Параметр</span><span class="sxs-lookup"><span data-stu-id="9e0c6-292">Parameter</span></span>                           | <span data-ttu-id="9e0c6-293">Описание</span><span class="sxs-lookup"><span data-stu-id="9e0c6-293">Description</span></span>                                                                                                                                                                                                                                                     |
|:------------------------------------|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="9e0c6-294"><nobr>*-Миграция*\<String></nobr></span><span class="sxs-lookup"><span data-stu-id="9e0c6-294"><nobr>*-Migration* \<String></nobr></span></span> | <span data-ttu-id="9e0c6-295">Целевая миграция.</span><span class="sxs-lookup"><span data-stu-id="9e0c6-295">The target migration.</span></span> <span data-ttu-id="9e0c6-296">Миграция может быть идентифицирована по имени или по ИДЕНТИФИКАТОРу.</span><span class="sxs-lookup"><span data-stu-id="9e0c6-296">Migrations may be identified by name or by ID.</span></span> <span data-ttu-id="9e0c6-297">Число 0 — это особый случай, который означает *перед первой миграцией* и приводит к отмене всех миграций.</span><span class="sxs-lookup"><span data-stu-id="9e0c6-297">The number 0 is a special case that means *before the first migration* and causes all migrations to be reverted.</span></span> <span data-ttu-id="9e0c6-298">Если миграция не указана, команда по умолчанию принимает значение последней миграции.</span><span class="sxs-lookup"><span data-stu-id="9e0c6-298">If no migration is specified, the command defaults to the last migration.</span></span> |
| <span data-ttu-id="9e0c6-299"><nobr>— Подключение \<String></nobr></span><span class="sxs-lookup"><span data-stu-id="9e0c6-299"><nobr>-Connection \<String></nobr></span></span>  | <span data-ttu-id="9e0c6-300">Строка подключения к базе данных.</span><span class="sxs-lookup"><span data-stu-id="9e0c6-300">The connection string to the database.</span></span> <span data-ttu-id="9e0c6-301">По умолчанию используется значение, заданное в `AddDbContext` или `OnConfiguring` .</span><span class="sxs-lookup"><span data-stu-id="9e0c6-301">Defaults to the one specified in `AddDbContext` or `OnConfiguring`.</span></span> |

> [!TIP]
> <span data-ttu-id="9e0c6-302">Параметр миграции поддерживает расширение клавиш TAB.</span><span class="sxs-lookup"><span data-stu-id="9e0c6-302">The Migration parameter supports tab-expansion.</span></span>

<span data-ttu-id="9e0c6-303">В следующем примере возвращаются все миграции.</span><span class="sxs-lookup"><span data-stu-id="9e0c6-303">The following example reverts all migrations.</span></span>

```powershell
Update-Database -Migration 0
```

<span data-ttu-id="9e0c6-304">В следующих примерах база данных обновляется до указанной миграции.</span><span class="sxs-lookup"><span data-stu-id="9e0c6-304">The following examples update the database to a specified migration.</span></span> <span data-ttu-id="9e0c6-305">В первом случае используется имя миграции, а во втором используется идентификатор миграции и указанное соединение:</span><span class="sxs-lookup"><span data-stu-id="9e0c6-305">The first uses the migration name and the second uses the migration ID and a specified connection:</span></span>

```powershell
Update-Database -Migration InitialCreate
Update-Database -Migration 20180904195021_InitialCreate -Connection your_connection_string
```

## <a name="additional-resources"></a><span data-ttu-id="9e0c6-306">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="9e0c6-306">Additional resources</span></span>

* [<span data-ttu-id="9e0c6-307">Миграции</span><span class="sxs-lookup"><span data-stu-id="9e0c6-307">Migrations</span></span>](xref:core/managing-schemas/migrations/index)
* [<span data-ttu-id="9e0c6-308">Реконструирование</span><span class="sxs-lookup"><span data-stu-id="9e0c6-308">Reverse Engineering</span></span>](xref:core/managing-schemas/scaffolding)
