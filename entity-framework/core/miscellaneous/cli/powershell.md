---
title: Справочник по средствам EF Core (консоль диспетчера пакетов) — EF Core
author: bricelam
ms.author: bricelam
ms.date: 09/18/2018
uid: core/miscellaneous/cli/powershell
ms.openlocfilehash: 45370a82131da9db8b724fe395d41b1e3641fcf8
ms.sourcegitcommit: 708b18520321c587b2046ad2ea9fa7c48aeebfe5
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/09/2019
ms.locfileid: "72181335"
---
# <a name="entity-framework-core-tools-reference---package-manager-console-in-visual-studio"></a><span data-ttu-id="b3d67-102">Справочник по средствам Entity Framework Core. консоль диспетчера пакетов в Visual Studio</span><span class="sxs-lookup"><span data-stu-id="b3d67-102">Entity Framework Core tools reference - Package Manager Console in Visual Studio</span></span>

<span data-ttu-id="b3d67-103">Средства консоли диспетчера пакетов (PMC) для Entity Framework Core выполнения задач разработки во время разработки.</span><span class="sxs-lookup"><span data-stu-id="b3d67-103">The Package Manager Console (PMC) tools for Entity Framework Core perform design-time development tasks.</span></span> <span data-ttu-id="b3d67-104">Например, они создают [миграции](/aspnet/core/data/ef-mvc/migrations?view=aspnetcore-2.0), применяют миграции и создают код для модели на основе существующей базы данных.</span><span class="sxs-lookup"><span data-stu-id="b3d67-104">For example, they create [migrations](/aspnet/core/data/ef-mvc/migrations?view=aspnetcore-2.0), apply migrations, and generate code for a model based on an existing database.</span></span> <span data-ttu-id="b3d67-105">Команды выполняются в Visual Studio с помощью [консоли диспетчера пакетов](/nuget/tools/package-manager-console).</span><span class="sxs-lookup"><span data-stu-id="b3d67-105">The commands run inside of Visual Studio using the [Package Manager Console](/nuget/tools/package-manager-console).</span></span> <span data-ttu-id="b3d67-106">Эти инструменты совместимы с проектами .NET Framework и .NET Core.</span><span class="sxs-lookup"><span data-stu-id="b3d67-106">These tools work with both .NET Framework and .NET Core projects.</span></span>

<span data-ttu-id="b3d67-107">Если вы не используете Visual Studio, вместо этого рекомендуется использовать [средства командной строки EF Core](dotnet.md) .</span><span class="sxs-lookup"><span data-stu-id="b3d67-107">If you aren't using Visual Studio, we recommend the [EF Core Command-line Tools](dotnet.md) instead.</span></span> <span data-ttu-id="b3d67-108">Средства CLI выполняются на разных платформах и выполняются в командной строке.</span><span class="sxs-lookup"><span data-stu-id="b3d67-108">The CLI tools are cross-platform and run inside a command prompt.</span></span>

## <a name="installing-the-tools"></a><span data-ttu-id="b3d67-109">Установка инструментов</span><span class="sxs-lookup"><span data-stu-id="b3d67-109">Installing the tools</span></span>

<span data-ttu-id="b3d67-110">Процедуры установки и обновления инструментов отличаются от ASP.NET Core 2.1 + и более ранних версий или других типов проектов.</span><span class="sxs-lookup"><span data-stu-id="b3d67-110">The procedures for installing and updating the tools differ between ASP.NET Core 2.1+ and earlier versions or other project types.</span></span>

### <a name="aspnet-core-version-21-and-later"></a><span data-ttu-id="b3d67-111">ASP.NET Core версии 2,1 и более поздних</span><span class="sxs-lookup"><span data-stu-id="b3d67-111">ASP.NET Core version 2.1 and later</span></span>

<span data-ttu-id="b3d67-112">Эти средства автоматически включаются в проект ASP.NET Core 2.1 +, так как пакет `Microsoft.EntityFrameworkCore.Tools` входит в состав [метапакет Microsoft. AspNetCore. app](/aspnet/core/fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="b3d67-112">The tools are automatically included in an ASP.NET Core 2.1+ project because the `Microsoft.EntityFrameworkCore.Tools` package is included in the [Microsoft.AspNetCore.App metapackage](/aspnet/core/fundamentals/metapackage-app).</span></span>

<span data-ttu-id="b3d67-113">Поэтому вам не нужно ничего делать для установки средств, но вам нужно:</span><span class="sxs-lookup"><span data-stu-id="b3d67-113">Therefore, you don't have to do anything to install the tools, but you do have to:</span></span>
* <span data-ttu-id="b3d67-114">Восстановите пакеты перед использованием средств в новом проекте.</span><span class="sxs-lookup"><span data-stu-id="b3d67-114">Restore packages before using the tools in a new project.</span></span>
* <span data-ttu-id="b3d67-115">Установите пакет, чтобы обновить средства до более новой версии.</span><span class="sxs-lookup"><span data-stu-id="b3d67-115">Install a package to update the tools to a newer version.</span></span>

<span data-ttu-id="b3d67-116">Чтобы убедиться, что вы получаете последнюю версию средств, мы рекомендуем также выполнить следующие действия.</span><span class="sxs-lookup"><span data-stu-id="b3d67-116">To make sure that you're getting the latest version of the tools, we recommend that you also do the following step:</span></span>

* <span data-ttu-id="b3d67-117">Измените *CSPROJ* -файл и добавьте строку, указывающую последнюю версию пакета [Microsoft. EntityFrameworkCore. Tools](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools/) .</span><span class="sxs-lookup"><span data-stu-id="b3d67-117">Edit your *.csproj* file and add a line specifying the latest version of the [Microsoft.EntityFrameworkCore.Tools](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools/) package.</span></span> <span data-ttu-id="b3d67-118">Например, файл *CSPROJ* может включать `ItemGroup`, который выглядит следующим образом:</span><span class="sxs-lookup"><span data-stu-id="b3d67-118">For example, the *.csproj* file might include an `ItemGroup` that looks like this:</span></span>

  ```xml
  <ItemGroup>
    <PackageReference Include="Microsoft.AspNetCore.App" />
    <PackageReference Include="Microsoft.EntityFrameworkCore.Tools" Version="2.1.3" />
    <PackageReference Include="Microsoft.VisualStudio.Web.CodeGeneration.Design" Version="2.1.1" />
  </ItemGroup>
  ```

<span data-ttu-id="b3d67-119">Обновите средства при получении сообщения, как в следующем примере:</span><span class="sxs-lookup"><span data-stu-id="b3d67-119">Update the tools when you get a message like the following example:</span></span>

> <span data-ttu-id="b3d67-120">Версия инструментов EF Core "короче-RTM-30846" старше, чем версия среды выполнения "2.1.3-RTM-32065".</span><span class="sxs-lookup"><span data-stu-id="b3d67-120">The EF Core tools version '2.1.1-rtm-30846' is older than that of the runtime '2.1.3-rtm-32065'.</span></span> <span data-ttu-id="b3d67-121">Обновите средства для получения последних компонентов и исправлений ошибок.</span><span class="sxs-lookup"><span data-stu-id="b3d67-121">Update the tools for the latest features and bug fixes.</span></span>

<span data-ttu-id="b3d67-122">Чтобы обновить средства, выполните следующие действия.</span><span class="sxs-lookup"><span data-stu-id="b3d67-122">To update the tools:</span></span>
* <span data-ttu-id="b3d67-123">Установите последнюю пакет SDK для .NET Core.</span><span class="sxs-lookup"><span data-stu-id="b3d67-123">Install the latest .NET Core SDK.</span></span>
* <span data-ttu-id="b3d67-124">Обновите Visual Studio до последней версии.</span><span class="sxs-lookup"><span data-stu-id="b3d67-124">Update Visual Studio to the latest version.</span></span>
* <span data-ttu-id="b3d67-125">Измените *CSPROJ* -файл, чтобы он включал ссылку на пакет для последней версии пакета инструментов, как показано выше.</span><span class="sxs-lookup"><span data-stu-id="b3d67-125">Edit the *.csproj* file so that it includes a package reference to the latest tools package, as shown earlier.</span></span>

### <a name="other-versions-and-project-types"></a><span data-ttu-id="b3d67-126">Другие версии и типы проектов</span><span class="sxs-lookup"><span data-stu-id="b3d67-126">Other versions and project types</span></span>

<span data-ttu-id="b3d67-127">Установите средства консоли диспетчера пакетов, выполнив следующую команду в **консоли диспетчера пакетов**:</span><span class="sxs-lookup"><span data-stu-id="b3d67-127">Install the Package Manager Console tools by running the following command in **Package Manager Console**:</span></span>

``` powershell
Install-Package Microsoft.EntityFrameworkCore.Tools
```

<span data-ttu-id="b3d67-128">Обновите средства, выполнив следующую команду в **консоли диспетчера пакетов**.</span><span class="sxs-lookup"><span data-stu-id="b3d67-128">Update the tools by running the following command in **Package Manager Console**.</span></span>

``` powershell
Update-Package Microsoft.EntityFrameworkCore.Tools
```

### <a name="verify-the-installation"></a><span data-ttu-id="b3d67-129">Проверка установки</span><span class="sxs-lookup"><span data-stu-id="b3d67-129">Verify the installation</span></span>

<span data-ttu-id="b3d67-130">Убедитесь, что средства установлены, выполнив следующую команду:</span><span class="sxs-lookup"><span data-stu-id="b3d67-130">Verify that the tools are installed by running this command:</span></span>

``` powershell
Get-Help about_EntityFrameworkCore
```

<span data-ttu-id="b3d67-131">Выходные данные выглядят следующим образом (он не указывает, какую версию инструментов вы используете):</span><span class="sxs-lookup"><span data-stu-id="b3d67-131">The output looks like this (it doesn't tell you which version of the tools you're using):</span></span>

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

## <a name="using-the-tools"></a><span data-ttu-id="b3d67-132">Использование средств</span><span class="sxs-lookup"><span data-stu-id="b3d67-132">Using the tools</span></span>

<span data-ttu-id="b3d67-133">Перед использованием средств:</span><span class="sxs-lookup"><span data-stu-id="b3d67-133">Before using the tools:</span></span>
* <span data-ttu-id="b3d67-134">Изучите разницу между целевым и запускаемым проектами.</span><span class="sxs-lookup"><span data-stu-id="b3d67-134">Understand the difference between target and startup project.</span></span>
* <span data-ttu-id="b3d67-135">Узнайте, как использовать средства с библиотеками классов .NET Standard.</span><span class="sxs-lookup"><span data-stu-id="b3d67-135">Learn how to use the tools with .NET Standard class libraries.</span></span>
* <span data-ttu-id="b3d67-136">Для проектов ASP.NET Core задайте среду.</span><span class="sxs-lookup"><span data-stu-id="b3d67-136">For ASP.NET Core projects, set the environment.</span></span>

### <a name="target-and-startup-project"></a><span data-ttu-id="b3d67-137">Целевой и запускаемый проекты</span><span class="sxs-lookup"><span data-stu-id="b3d67-137">Target and startup project</span></span>

<span data-ttu-id="b3d67-138">Команды ссылаются на *проект* и *запускаемый проект*.</span><span class="sxs-lookup"><span data-stu-id="b3d67-138">The commands refer to a *project* and a *startup project*.</span></span>

* <span data-ttu-id="b3d67-139">*Проект* также называется *целевым проектом* , так как он служит для добавления или удаления файлов.</span><span class="sxs-lookup"><span data-stu-id="b3d67-139">The *project* is also known as the *target project* because it's where the commands add or remove files.</span></span> <span data-ttu-id="b3d67-140">По умолчанию **проект** , выбранный в **консоли диспетчера пакетов** , является целевым.</span><span class="sxs-lookup"><span data-stu-id="b3d67-140">By default, the **Default project** selected in **Package Manager Console** is the target project.</span></span> <span data-ttu-id="b3d67-141">Можно указать другой проект в качестве целевого проекта с помощью параметра <nobr>`--project`</nobr> .</span><span class="sxs-lookup"><span data-stu-id="b3d67-141">You can specify a different project as target project by using the <nobr>`--project`</nobr> option.</span></span>

* <span data-ttu-id="b3d67-142">*Запускаемый проект* — это тот, который выполняет сборка и запуск средств.</span><span class="sxs-lookup"><span data-stu-id="b3d67-142">The *startup project* is the one that the tools build and run.</span></span> <span data-ttu-id="b3d67-143">Средства должны выполнять код приложения во время разработки для получения сведений о проекте, таких как строка подключения к базе данных и Конфигурация модели.</span><span class="sxs-lookup"><span data-stu-id="b3d67-143">The tools have to execute application code at design time to get information about the project, such as the database connection string and the configuration of the model.</span></span> <span data-ttu-id="b3d67-144">По умолчанию **запускаемым проектом** в **Обозреватель решений** является запускаемый проект.</span><span class="sxs-lookup"><span data-stu-id="b3d67-144">By default, the **Startup Project** in **Solution Explorer** is the startup project.</span></span> <span data-ttu-id="b3d67-145">Можно указать другой проект в качестве запускаемого проекта с помощью параметра <nobr>`--startup-project`</nobr> .</span><span class="sxs-lookup"><span data-stu-id="b3d67-145">You can specify a different project as startup project by using the <nobr>`--startup-project`</nobr> option.</span></span>

<span data-ttu-id="b3d67-146">Запускаемый проект и целевой проект часто являются одним и тем же проектом.</span><span class="sxs-lookup"><span data-stu-id="b3d67-146">The startup project and target project are often the same project.</span></span> <span data-ttu-id="b3d67-147">Типичный сценарий, в котором они являются отдельными проектами, состоит в следующих случаях:</span><span class="sxs-lookup"><span data-stu-id="b3d67-147">A typical scenario where they are separate projects is when:</span></span>

* <span data-ttu-id="b3d67-148">Контекст EF Core и классы сущностей находятся в библиотеке классов .NET Core.</span><span class="sxs-lookup"><span data-stu-id="b3d67-148">The EF Core context and entity classes are in a .NET Core class library.</span></span>
* <span data-ttu-id="b3d67-149">Консольное приложение .NET Core или веб-приложение ссылается на библиотеку классов.</span><span class="sxs-lookup"><span data-stu-id="b3d67-149">A .NET Core console app or web app references the class library.</span></span>

<span data-ttu-id="b3d67-150">Также можно [разместить код миграции в библиотеке классов, отдельном от контекста EF Core](xref:core/managing-schemas/migrations/projects).</span><span class="sxs-lookup"><span data-stu-id="b3d67-150">It's also possible to [put migrations code in a class library separate from the EF Core context](xref:core/managing-schemas/migrations/projects).</span></span>

### <a name="other-target-frameworks"></a><span data-ttu-id="b3d67-151">Другие целевые платформы</span><span class="sxs-lookup"><span data-stu-id="b3d67-151">Other target frameworks</span></span>

<span data-ttu-id="b3d67-152">Средства консоли диспетчера пакетов работают с проектами .NET Core или .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="b3d67-152">The Package Manager Console tools work with .NET Core or .NET Framework projects.</span></span> <span data-ttu-id="b3d67-153">Приложения с моделью EF Core в библиотеке классов .NET Standard могут не иметь проекта .NET Core или .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="b3d67-153">Apps that have the EF Core model in a .NET Standard class library might not have a .NET Core or .NET Framework project.</span></span> <span data-ttu-id="b3d67-154">Например, это справедливо для приложений Xamarin и универсальная платформа Windows.</span><span class="sxs-lookup"><span data-stu-id="b3d67-154">For example, this is true of Xamarin and Universal Windows Platform apps.</span></span> <span data-ttu-id="b3d67-155">В таких случаях можно создать проект консольного приложения .NET Core или .NET Framework, предназначенное только для запуска в качестве запускаемого проекта для инструментов.</span><span class="sxs-lookup"><span data-stu-id="b3d67-155">In such cases, you can create a .NET Core or .NET Framework console app project whose only purpose is to act as startup project for the tools.</span></span> <span data-ttu-id="b3d67-156">Проект может быть фиктивным проектом без действительного кода &mdash;. он необходим только для предоставления целей для инструментов.</span><span class="sxs-lookup"><span data-stu-id="b3d67-156">The project can be a dummy project with no real code &mdash; it is only needed to provide a target for the tooling.</span></span>

<span data-ttu-id="b3d67-157">Зачем нужен фиктивный проект?</span><span class="sxs-lookup"><span data-stu-id="b3d67-157">Why is a dummy project required?</span></span> <span data-ttu-id="b3d67-158">Как упоминалось ранее, средства должны выполнять код приложения во время разработки.</span><span class="sxs-lookup"><span data-stu-id="b3d67-158">As mentioned earlier, the tools have to execute application code at design time.</span></span> <span data-ttu-id="b3d67-159">Для этого необходимо использовать среду выполнения .NET Core или .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="b3d67-159">To do that, they need to use the .NET Core or .NET Framework runtime.</span></span> <span data-ttu-id="b3d67-160">Если EF Coreная модель находится в проекте, ориентированном на .NET Core или .NET Framework, EF Core средства позаимствованы среду выполнения из проекта.</span><span class="sxs-lookup"><span data-stu-id="b3d67-160">When the EF Core model is in a project that targets .NET Core or .NET Framework, the EF Core tools borrow the runtime from the project.</span></span> <span data-ttu-id="b3d67-161">Они не могут сделать это, если модель EF Core находится в библиотеке классов .NET Standard.</span><span class="sxs-lookup"><span data-stu-id="b3d67-161">They can't do that if the EF Core model is in a .NET Standard class library.</span></span> <span data-ttu-id="b3d67-162">.NET Standard не является реальной реализацией .NET; это спецификация набора интерфейсов API, которые должны поддерживаться реализациями .NET.</span><span class="sxs-lookup"><span data-stu-id="b3d67-162">The .NET Standard is not an actual .NET implementation; it's a specification of a set of APIs that .NET implementations must support.</span></span> <span data-ttu-id="b3d67-163">Поэтому .NET Standard недостаточно для того, чтобы средства EF Core выполняли код приложения.</span><span class="sxs-lookup"><span data-stu-id="b3d67-163">Therefore .NET Standard is not sufficient for the EF Core tools to execute application code.</span></span> <span data-ttu-id="b3d67-164">Фиктивный проект, который создается для использования в качестве запускаемого проекта, предоставляет конкретную целевую платформу, в которую средства могут загрузить библиотеку классов .NET Standard.</span><span class="sxs-lookup"><span data-stu-id="b3d67-164">The dummy project you create to use as startup project provides a concrete target platform into which the tools can load the .NET Standard class library.</span></span>

### <a name="aspnet-core-environment"></a><span data-ttu-id="b3d67-165">Среда ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="b3d67-165">ASP.NET Core environment</span></span>

<span data-ttu-id="b3d67-166">Чтобы указать среду для проектов ASP.NET Core, установите **env: ASPNETCORE_ENVIRONMENT** перед выполнением команд.</span><span class="sxs-lookup"><span data-stu-id="b3d67-166">To specify the environment for ASP.NET Core projects, set **env:ASPNETCORE_ENVIRONMENT** before running commands.</span></span>

## <a name="common-parameters"></a><span data-ttu-id="b3d67-167">Общие параметры</span><span class="sxs-lookup"><span data-stu-id="b3d67-167">Common parameters</span></span>

<span data-ttu-id="b3d67-168">В следующей таблице приведены параметры, которые являются общими для всех EF Coreных команд.</span><span class="sxs-lookup"><span data-stu-id="b3d67-168">The following table shows parameters that are common to all of the EF Core commands:</span></span>

| <span data-ttu-id="b3d67-169">Параметр</span><span class="sxs-lookup"><span data-stu-id="b3d67-169">Parameter</span></span>                 | <span data-ttu-id="b3d67-170">Описание</span><span class="sxs-lookup"><span data-stu-id="b3d67-170">Description</span></span>                                                                                                                                                                                                          |
|:--------------------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="b3d67-171">-Context \<String ></span><span class="sxs-lookup"><span data-stu-id="b3d67-171">-Context \<String></span></span>        | <span data-ttu-id="b3d67-172">Класс `DbContext` для использования.</span><span class="sxs-lookup"><span data-stu-id="b3d67-172">The `DbContext` class to use.</span></span> <span data-ttu-id="b3d67-173">Только имя класса или полный квалификатор с пространствами имен.</span><span class="sxs-lookup"><span data-stu-id="b3d67-173">Class name only or fully qualified with namespaces.</span></span>  <span data-ttu-id="b3d67-174">Если этот параметр опущен, EF Core находит класс контекста.</span><span class="sxs-lookup"><span data-stu-id="b3d67-174">If this parameter is omitted, EF Core finds the context class.</span></span> <span data-ttu-id="b3d67-175">Если имеется несколько классов контекста, этот параметр является обязательным.</span><span class="sxs-lookup"><span data-stu-id="b3d67-175">If there are multiple context classes, this parameter is required.</span></span> |
| <span data-ttu-id="b3d67-176">-Project \<String ></span><span class="sxs-lookup"><span data-stu-id="b3d67-176">-Project \<String></span></span>        | <span data-ttu-id="b3d67-177">Целевой проект.</span><span class="sxs-lookup"><span data-stu-id="b3d67-177">The target project.</span></span> <span data-ttu-id="b3d67-178">Если этот параметр не указан, в качестве целевого проекта используется **проект по умолчанию** для **консоли диспетчера пакетов** .</span><span class="sxs-lookup"><span data-stu-id="b3d67-178">If this parameter is omitted, the **Default project** for **Package Manager Console** is used as the target project.</span></span>                                                                             |
| <span data-ttu-id="b3d67-179">-Стартуппрожект @no__t — 0String ></span><span class="sxs-lookup"><span data-stu-id="b3d67-179">-StartupProject \<String></span></span> | <span data-ttu-id="b3d67-180">Запускаемый проект.</span><span class="sxs-lookup"><span data-stu-id="b3d67-180">The startup project.</span></span> <span data-ttu-id="b3d67-181">Если этот параметр не указан, в качестве целевого проекта используется **запускаемый проект** в **свойствах решения** .</span><span class="sxs-lookup"><span data-stu-id="b3d67-181">If this parameter is omitted, the **Startup project** in **Solution properties** is used as the target project.</span></span>                                                                                 |
| <span data-ttu-id="b3d67-182">-Verbose</span><span class="sxs-lookup"><span data-stu-id="b3d67-182">-Verbose</span></span>                  | <span data-ttu-id="b3d67-183">Отображение подробных выходных данных.</span><span class="sxs-lookup"><span data-stu-id="b3d67-183">Show verbose output.</span></span>                                                                                                                                                                                                 |

<span data-ttu-id="b3d67-184">Чтобы отобразить справочные сведения о команде, используйте команду PowerShell `Get-Help`.</span><span class="sxs-lookup"><span data-stu-id="b3d67-184">To show help information about a command, use PowerShell's `Get-Help` command.</span></span>

> [!TIP]
> <span data-ttu-id="b3d67-185">Параметры Context, Project и Стартуппрожект поддерживают расширение Tab.</span><span class="sxs-lookup"><span data-stu-id="b3d67-185">The Context, Project, and StartupProject parameters support tab-expansion.</span></span>

## <a name="add-migration"></a><span data-ttu-id="b3d67-186">Добавление и миграция</span><span class="sxs-lookup"><span data-stu-id="b3d67-186">Add-Migration</span></span>

<span data-ttu-id="b3d67-187">Добавляет новый перенос.</span><span class="sxs-lookup"><span data-stu-id="b3d67-187">Adds a new migration.</span></span>

<span data-ttu-id="b3d67-188">Параметры:</span><span class="sxs-lookup"><span data-stu-id="b3d67-188">Parameters:</span></span>

| <span data-ttu-id="b3d67-189">Параметр</span><span class="sxs-lookup"><span data-stu-id="b3d67-189">Parameter</span></span>                         | <span data-ttu-id="b3d67-190">Описание</span><span class="sxs-lookup"><span data-stu-id="b3d67-190">Description</span></span>                                                                                                             |
|:----------------------------------|:------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="b3d67-191">@no__t -0-Name \<String > <nobr></span><span class="sxs-lookup"><span data-stu-id="b3d67-191"><nobr>-Name \<String><nobr></span></span>       | <span data-ttu-id="b3d67-192">Имя миграции.</span><span class="sxs-lookup"><span data-stu-id="b3d67-192">The name of the migration.</span></span> <span data-ttu-id="b3d67-193">Этот параметр является позиционированным и является обязательным.</span><span class="sxs-lookup"><span data-stu-id="b3d67-193">This is a positional parameter and is required.</span></span>                                              |
| <span data-ttu-id="b3d67-194"><nobr>-OutputDir @no__t — 1String ></nobr></span><span class="sxs-lookup"><span data-stu-id="b3d67-194"><nobr>-OutputDir \<String></nobr></span></span> | <span data-ttu-id="b3d67-195">Используемый каталог (и подпространство имен).</span><span class="sxs-lookup"><span data-stu-id="b3d67-195">The directory (and sub-namespace) to use.</span></span> <span data-ttu-id="b3d67-196">Пути задаются относительно целевого каталога проекта.</span><span class="sxs-lookup"><span data-stu-id="b3d67-196">Paths are relative to the target project directory.</span></span> <span data-ttu-id="b3d67-197">По умолчанию используется значение "миграции".</span><span class="sxs-lookup"><span data-stu-id="b3d67-197">Defaults to "Migrations".</span></span> |

## <a name="drop-database"></a><span data-ttu-id="b3d67-198">Удалить базу данных</span><span class="sxs-lookup"><span data-stu-id="b3d67-198">Drop-Database</span></span>

<span data-ttu-id="b3d67-199">Удаляет базу данных.</span><span class="sxs-lookup"><span data-stu-id="b3d67-199">Drops the database.</span></span>

<span data-ttu-id="b3d67-200">Параметры:</span><span class="sxs-lookup"><span data-stu-id="b3d67-200">Parameters:</span></span>

| <span data-ttu-id="b3d67-201">Параметр</span><span class="sxs-lookup"><span data-stu-id="b3d67-201">Parameter</span></span> | <span data-ttu-id="b3d67-202">Описание</span><span class="sxs-lookup"><span data-stu-id="b3d67-202">Description</span></span>                                              |
|:----------|:---------------------------------------------------------|
| <span data-ttu-id="b3d67-203">-WhatIf</span><span class="sxs-lookup"><span data-stu-id="b3d67-203">-WhatIf</span></span>   | <span data-ttu-id="b3d67-204">Показывает, какую базу данных следует удалить, но не удалять.</span><span class="sxs-lookup"><span data-stu-id="b3d67-204">Show which database would be dropped, but don't drop it.</span></span> |

## <a name="get-dbcontext"></a><span data-ttu-id="b3d67-205">Get-DbContext</span><span class="sxs-lookup"><span data-stu-id="b3d67-205">Get-DbContext</span></span>

<span data-ttu-id="b3d67-206">Возвращает сведения о типе `DbContext`.</span><span class="sxs-lookup"><span data-stu-id="b3d67-206">Gets information about a `DbContext` type.</span></span>

## <a name="remove-migration"></a><span data-ttu-id="b3d67-207">Remove-Migration</span><span class="sxs-lookup"><span data-stu-id="b3d67-207">Remove-Migration</span></span>

<span data-ttu-id="b3d67-208">Удаляет последнюю миграцию (выполняет откат изменений кода, выполненных для миграции).</span><span class="sxs-lookup"><span data-stu-id="b3d67-208">Removes the last migration (rolls back the code changes that were done for the migration).</span></span>

<span data-ttu-id="b3d67-209">Параметры:</span><span class="sxs-lookup"><span data-stu-id="b3d67-209">Parameters:</span></span>

| <span data-ttu-id="b3d67-210">Параметр</span><span class="sxs-lookup"><span data-stu-id="b3d67-210">Parameter</span></span> | <span data-ttu-id="b3d67-211">Описание</span><span class="sxs-lookup"><span data-stu-id="b3d67-211">Description</span></span>                                                                     |
|:----------|:--------------------------------------------------------------------------------|
| <span data-ttu-id="b3d67-212">-Force</span><span class="sxs-lookup"><span data-stu-id="b3d67-212">-Force</span></span>    | <span data-ttu-id="b3d67-213">Отмените миграцию (выполните откат изменений, примененных к базе данных).</span><span class="sxs-lookup"><span data-stu-id="b3d67-213">Revert the migration (roll back the changes that were applied to the database).</span></span> |

## <a name="scaffold-dbcontext"></a><span data-ttu-id="b3d67-214">Формирование шаблонов — DbContext</span><span class="sxs-lookup"><span data-stu-id="b3d67-214">Scaffold-DbContext</span></span>

<span data-ttu-id="b3d67-215">Создает код для `DbContext` и типов сущностей для базы данных.</span><span class="sxs-lookup"><span data-stu-id="b3d67-215">Generates code for a `DbContext` and entity types for a database.</span></span> <span data-ttu-id="b3d67-216">Чтобы `Scaffold-DbContext` создавала тип сущности, таблица базы данных должна иметь первичный ключ.</span><span class="sxs-lookup"><span data-stu-id="b3d67-216">In order for `Scaffold-DbContext` to generate an entity type, the database table must have a primary key.</span></span>

<span data-ttu-id="b3d67-217">Параметры:</span><span class="sxs-lookup"><span data-stu-id="b3d67-217">Parameters:</span></span>

| <span data-ttu-id="b3d67-218">Параметр</span><span class="sxs-lookup"><span data-stu-id="b3d67-218">Parameter</span></span>                          | <span data-ttu-id="b3d67-219">Описание</span><span class="sxs-lookup"><span data-stu-id="b3d67-219">Description</span></span>                                                                                                                                                                                                                                                             |
|:-----------------------------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="b3d67-220"><nobr>— @No__t подключения — 1String ></nobr></span><span class="sxs-lookup"><span data-stu-id="b3d67-220"><nobr>-Connection \<String></nobr></span></span> | <span data-ttu-id="b3d67-221">Строка подключения к базе данных.</span><span class="sxs-lookup"><span data-stu-id="b3d67-221">The connection string to the database.</span></span> <span data-ttu-id="b3d67-222">Для проектов ASP.NET Core 2. x значением может быть *Name = \<name строки подключения >* .</span><span class="sxs-lookup"><span data-stu-id="b3d67-222">For ASP.NET Core 2.x projects, the value can be *name=\<name of connection string>*.</span></span> <span data-ttu-id="b3d67-223">В этом случае имя берется из источников конфигурации, настроенных для проекта.</span><span class="sxs-lookup"><span data-stu-id="b3d67-223">In that case the name comes from the configuration sources that are set up for the project.</span></span> <span data-ttu-id="b3d67-224">Этот параметр является позиционированным и является обязательным.</span><span class="sxs-lookup"><span data-stu-id="b3d67-224">This is a positional parameter and is required.</span></span> |
| <span data-ttu-id="b3d67-225"><nobr>— Поставщик @no__t — 1String ></nobr></span><span class="sxs-lookup"><span data-stu-id="b3d67-225"><nobr>-Provider \<String></nobr></span></span>   | <span data-ttu-id="b3d67-226">Используемый поставщик.</span><span class="sxs-lookup"><span data-stu-id="b3d67-226">The provider to use.</span></span> <span data-ttu-id="b3d67-227">Обычно это имя пакета NuGet, например: `Microsoft.EntityFrameworkCore.SqlServer`.</span><span class="sxs-lookup"><span data-stu-id="b3d67-227">Typically this is the name of the NuGet package, for example: `Microsoft.EntityFrameworkCore.SqlServer`.</span></span> <span data-ttu-id="b3d67-228">Этот параметр является позиционированным и является обязательным.</span><span class="sxs-lookup"><span data-stu-id="b3d67-228">This is a positional parameter and is required.</span></span>                                                                                           |
| <span data-ttu-id="b3d67-229">-OutputDir @no__t — 0String ></span><span class="sxs-lookup"><span data-stu-id="b3d67-229">-OutputDir \<String></span></span>               | <span data-ttu-id="b3d67-230">Каталог для размещения файлов.</span><span class="sxs-lookup"><span data-stu-id="b3d67-230">The directory to put files in.</span></span> <span data-ttu-id="b3d67-231">Пути задаются относительно каталога проекта.</span><span class="sxs-lookup"><span data-stu-id="b3d67-231">Paths are relative to the project directory.</span></span>                                                                                                                                                                                             |
| <span data-ttu-id="b3d67-232">-Контекстдир @no__t — 0String ></span><span class="sxs-lookup"><span data-stu-id="b3d67-232">-ContextDir \<String></span></span>              | <span data-ttu-id="b3d67-233">Каталог, в который помещается файл `DbContext`.</span><span class="sxs-lookup"><span data-stu-id="b3d67-233">The directory to put the `DbContext` file in.</span></span> <span data-ttu-id="b3d67-234">Пути задаются относительно каталога проекта.</span><span class="sxs-lookup"><span data-stu-id="b3d67-234">Paths are relative to the project directory.</span></span>                                                                                                                                                                              |
| <span data-ttu-id="b3d67-235">-Context \<String ></span><span class="sxs-lookup"><span data-stu-id="b3d67-235">-Context \<String></span></span>                 | <span data-ttu-id="b3d67-236">Имя создаваемого класса `DbContext`.</span><span class="sxs-lookup"><span data-stu-id="b3d67-236">The name of the `DbContext` class to generate.</span></span>                                                                                                                                                                                                                          |
| <span data-ttu-id="b3d67-237">-Schemas \<String [] ></span><span class="sxs-lookup"><span data-stu-id="b3d67-237">-Schemas \<String[]></span></span>               | <span data-ttu-id="b3d67-238">Схемы таблиц, для которых создаются типы сущностей.</span><span class="sxs-lookup"><span data-stu-id="b3d67-238">The schemas of tables to generate entity types for.</span></span> <span data-ttu-id="b3d67-239">Если этот параметр пропущен, включаются все схемы.</span><span class="sxs-lookup"><span data-stu-id="b3d67-239">If this parameter is omitted, all schemas are included.</span></span>                                                                                                                                                             |
| <span data-ttu-id="b3d67-240">-Tables \<String [] ></span><span class="sxs-lookup"><span data-stu-id="b3d67-240">-Tables \<String[]></span></span>                | <span data-ttu-id="b3d67-241">Таблицы, для которых создаются типы сущностей.</span><span class="sxs-lookup"><span data-stu-id="b3d67-241">The tables to generate entity types for.</span></span> <span data-ttu-id="b3d67-242">Если этот параметр пропущен, включаются все таблицы.</span><span class="sxs-lookup"><span data-stu-id="b3d67-242">If this parameter is omitted, all tables are included.</span></span>                                                                                                                                                                         |
| <span data-ttu-id="b3d67-243">-Аннотации</span><span class="sxs-lookup"><span data-stu-id="b3d67-243">-DataAnnotations</span></span>                   | <span data-ttu-id="b3d67-244">Используйте атрибуты для настройки модели (по возможности).</span><span class="sxs-lookup"><span data-stu-id="b3d67-244">Use attributes to configure the model (where possible).</span></span> <span data-ttu-id="b3d67-245">Если этот параметр пропущен, используется только API-интерфейс Fluent.</span><span class="sxs-lookup"><span data-stu-id="b3d67-245">If this parameter is omitted, only the fluent API is used.</span></span>                                                                                                                                                      |
| <span data-ttu-id="b3d67-246">-Уседатабасенамес</span><span class="sxs-lookup"><span data-stu-id="b3d67-246">-UseDatabaseNames</span></span>                  | <span data-ttu-id="b3d67-247">Имена таблиц и столбцов следует использовать в точности так, как они отображаются в базе данных.</span><span class="sxs-lookup"><span data-stu-id="b3d67-247">Use table and column names exactly as they appear in the database.</span></span> <span data-ttu-id="b3d67-248">Если этот параметр опущен, имена баз данных изменяются в соответствии с C# соглашениями о стиле имен.</span><span class="sxs-lookup"><span data-stu-id="b3d67-248">If this parameter is omitted, database names are changed to more closely conform to C# name style conventions.</span></span>                                                                                       |
| <span data-ttu-id="b3d67-249">-Force</span><span class="sxs-lookup"><span data-stu-id="b3d67-249">-Force</span></span>                             | <span data-ttu-id="b3d67-250">Перезаписать существующие файлы.</span><span class="sxs-lookup"><span data-stu-id="b3d67-250">Overwrite existing files.</span></span>                                                                                                                                                                                                                                               |

<span data-ttu-id="b3d67-251">Пример</span><span class="sxs-lookup"><span data-stu-id="b3d67-251">Example:</span></span>

```powershell
Scaffold-DbContext "Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer -OutputDir Models
```

<span data-ttu-id="b3d67-252">Например, формирование шаблонов только для выбранных таблиц и создает контекст в отдельной папке с указанным именем:</span><span class="sxs-lookup"><span data-stu-id="b3d67-252">Example that scaffolds only selected tables and creates the context in a separate folder with a specified name:</span></span>

```powershell
Scaffold-DbContext "Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer -OutputDir Models -Tables "Blog","Post" -ContextDir Context -Context BlogContext
```

## <a name="script-migration"></a><span data-ttu-id="b3d67-253">Скрипт — миграция</span><span class="sxs-lookup"><span data-stu-id="b3d67-253">Script-Migration</span></span>

<span data-ttu-id="b3d67-254">Создает скрипт SQL, который применяет все изменения из одной выбранной миграции в другую выбранную миграцию.</span><span class="sxs-lookup"><span data-stu-id="b3d67-254">Generates a SQL script that applies all of the changes from one selected migration to another selected migration.</span></span>

<span data-ttu-id="b3d67-255">Параметры:</span><span class="sxs-lookup"><span data-stu-id="b3d67-255">Parameters:</span></span>

| <span data-ttu-id="b3d67-256">Параметр</span><span class="sxs-lookup"><span data-stu-id="b3d67-256">Parameter</span></span>                | <span data-ttu-id="b3d67-257">Описание</span><span class="sxs-lookup"><span data-stu-id="b3d67-257">Description</span></span>                                                                                                                                                                                                                |
|:-------------------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="b3d67-258">*— Из* \<String ></span><span class="sxs-lookup"><span data-stu-id="b3d67-258">*-From* \<String></span></span>        | <span data-ttu-id="b3d67-259">Начало миграции.</span><span class="sxs-lookup"><span data-stu-id="b3d67-259">The starting migration.</span></span> <span data-ttu-id="b3d67-260">Миграция может быть идентифицирована по имени или по ИДЕНТИФИКАТОРу.</span><span class="sxs-lookup"><span data-stu-id="b3d67-260">Migrations may be identified by name or by ID.</span></span> <span data-ttu-id="b3d67-261">Число 0 — это особый случай, который означает *перед первой миграцией*.</span><span class="sxs-lookup"><span data-stu-id="b3d67-261">The number 0 is a special case that means *before the first migration*.</span></span> <span data-ttu-id="b3d67-262">Значение по умолчанию — 0.</span><span class="sxs-lookup"><span data-stu-id="b3d67-262">Defaults to 0.</span></span>                                                              |
| <span data-ttu-id="b3d67-263">*-To* \<String ></span><span class="sxs-lookup"><span data-stu-id="b3d67-263">*-To* \<String></span></span>          | <span data-ttu-id="b3d67-264">Завершение миграции.</span><span class="sxs-lookup"><span data-stu-id="b3d67-264">The ending migration.</span></span> <span data-ttu-id="b3d67-265">По умолчанию используется последняя миграция.</span><span class="sxs-lookup"><span data-stu-id="b3d67-265">Defaults to the last migration.</span></span>                                                                                                                                                                      |
| <span data-ttu-id="b3d67-266"><nobr>-Идемпотентными</nobr></span><span class="sxs-lookup"><span data-stu-id="b3d67-266"><nobr>-Idempotent</nobr></span></span> | <span data-ttu-id="b3d67-267">Создание скрипта, который может использоваться в базе данных при любой миграции.</span><span class="sxs-lookup"><span data-stu-id="b3d67-267">Generate a script that can be used on a database at any migration.</span></span>                                                                                                                                                         |
| <span data-ttu-id="b3d67-268">-Output \<String ></span><span class="sxs-lookup"><span data-stu-id="b3d67-268">-Output \<String></span></span>        | <span data-ttu-id="b3d67-269">Файл, в который записывается результат.</span><span class="sxs-lookup"><span data-stu-id="b3d67-269">The file to write the result to.</span></span> <span data-ttu-id="b3d67-270">Если этот параметр не указан, файл создается с именем, созданным в той же папке, что и файлы среды выполнения приложения, например: */obj/Debug/netcoreapp2.1/ghbkztfz.SQL/* .</span><span class="sxs-lookup"><span data-stu-id="b3d67-270">IF this parameter is omitted, the file is created with a generated name in the same folder as the app's runtime files are created, for example: */obj/Debug/netcoreapp2.1/ghbkztfz.sql/*.</span></span> |

> [!TIP]
> <span data-ttu-id="b3d67-271">Параметры to, from и Output поддерживают расширение клавишей TAB.</span><span class="sxs-lookup"><span data-stu-id="b3d67-271">The To, From, and Output parameters support tab-expansion.</span></span>

<span data-ttu-id="b3d67-272">В следующем примере создается скрипт для миграции InitialCreate с использованием имени миграции.</span><span class="sxs-lookup"><span data-stu-id="b3d67-272">The following example creates a script for the InitialCreate migration, using the migration name.</span></span>

```powershell
Script-Migration -To InitialCreate
```

<span data-ttu-id="b3d67-273">В следующем примере создается скрипт для всех миграций после миграции InitialCreate с использованием идентификатора миграции.</span><span class="sxs-lookup"><span data-stu-id="b3d67-273">The following example creates a script for all migrations after the InitialCreate migration, using the migration ID.</span></span>

```powershell
Script-Migration -From 20180904195021_InitialCreate
```

## <a name="update-database"></a><span data-ttu-id="b3d67-274">Обновление базы данных</span><span class="sxs-lookup"><span data-stu-id="b3d67-274">Update-Database</span></span>

<span data-ttu-id="b3d67-275">Обновляет базу данных до последней миграции или до указанной миграции.</span><span class="sxs-lookup"><span data-stu-id="b3d67-275">Updates the database to the last migration or to a specified migration.</span></span>

| <span data-ttu-id="b3d67-276">Параметр</span><span class="sxs-lookup"><span data-stu-id="b3d67-276">Parameter</span></span>                           | <span data-ttu-id="b3d67-277">Описание</span><span class="sxs-lookup"><span data-stu-id="b3d67-277">Description</span></span>                                                                                                                                                                                                                                                     |
|:------------------------------------|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="b3d67-278"><nobr> *-@No__t миграции* — 2String ></nobr></span><span class="sxs-lookup"><span data-stu-id="b3d67-278"><nobr>*-Migration* \<String></nobr></span></span> | <span data-ttu-id="b3d67-279">Целевая миграция.</span><span class="sxs-lookup"><span data-stu-id="b3d67-279">The target migration.</span></span> <span data-ttu-id="b3d67-280">Миграция может быть идентифицирована по имени или по ИДЕНТИФИКАТОРу.</span><span class="sxs-lookup"><span data-stu-id="b3d67-280">Migrations may be identified by name or by ID.</span></span> <span data-ttu-id="b3d67-281">Число 0 — это особый случай, который означает *перед первой миграцией* и приводит к отмене всех миграций.</span><span class="sxs-lookup"><span data-stu-id="b3d67-281">The number 0 is a special case that means *before the first migration* and causes all migrations to be reverted.</span></span> <span data-ttu-id="b3d67-282">Если миграция не указана, команда по умолчанию принимает значение последней миграции.</span><span class="sxs-lookup"><span data-stu-id="b3d67-282">If no migration is specified, the command defaults to the last migration.</span></span> |

> [!TIP]
> <span data-ttu-id="b3d67-283">Параметр миграции поддерживает расширение клавиш TAB.</span><span class="sxs-lookup"><span data-stu-id="b3d67-283">The Migration parameter supports tab-expansion.</span></span>

<span data-ttu-id="b3d67-284">В следующем примере возвращаются все миграции.</span><span class="sxs-lookup"><span data-stu-id="b3d67-284">The following example reverts all migrations.</span></span>

```powershell
Update-Database -Migration 0
```

<span data-ttu-id="b3d67-285">В следующих примерах база данных обновляется до указанной миграции.</span><span class="sxs-lookup"><span data-stu-id="b3d67-285">The following examples update the database to a specified migration.</span></span> <span data-ttu-id="b3d67-286">В первом случае используется имя миграции, а во втором используется идентификатор миграции:</span><span class="sxs-lookup"><span data-stu-id="b3d67-286">The first uses the migration name and the second uses the migration ID:</span></span>

```powershell
Update-Database -Migration InitialCreate
Update-Database -Migration 20180904195021_InitialCreate
```

## <a name="additional-resources"></a><span data-ttu-id="b3d67-287">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="b3d67-287">Additional resources</span></span>

* [<span data-ttu-id="b3d67-288">Миграции</span><span class="sxs-lookup"><span data-stu-id="b3d67-288">Migrations</span></span>](xref:core/managing-schemas/migrations/index)
* [<span data-ttu-id="b3d67-289">Реконструирование</span><span class="sxs-lookup"><span data-stu-id="b3d67-289">Reverse Engineering</span></span>](xref:core/managing-schemas/scaffolding)
