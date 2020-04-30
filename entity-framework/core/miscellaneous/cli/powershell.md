---
title: Справочник по средствам EF Core (консоль диспетчера пакетов) — EF Core
author: bricelam
ms.author: bricelam
ms.date: 09/18/2018
uid: core/miscellaneous/cli/powershell
ms.openlocfilehash: 0676475d46a8d21dee7bd10e25dd273a11e96ac3
ms.sourcegitcommit: 79e460f76b6664e1da5886d102bd97f651d2ffff
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/29/2020
ms.locfileid: "82538404"
---
# <a name="entity-framework-core-tools-reference---package-manager-console-in-visual-studio"></a><span data-ttu-id="f6134-102">Справочник по средствам Entity Framework Core. консоль диспетчера пакетов в Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f6134-102">Entity Framework Core tools reference - Package Manager Console in Visual Studio</span></span>

<span data-ttu-id="f6134-103">Средства консоли диспетчера пакетов (PMC) для Entity Framework Core выполнения задач разработки во время разработки.</span><span class="sxs-lookup"><span data-stu-id="f6134-103">The Package Manager Console (PMC) tools for Entity Framework Core perform design-time development tasks.</span></span> <span data-ttu-id="f6134-104">Например, они создают [миграции](/aspnet/core/data/ef-mvc/migrations?view=aspnetcore-2.0), применяют миграции и создают код для модели на основе существующей базы данных.</span><span class="sxs-lookup"><span data-stu-id="f6134-104">For example, they create [migrations](/aspnet/core/data/ef-mvc/migrations?view=aspnetcore-2.0), apply migrations, and generate code for a model based on an existing database.</span></span> <span data-ttu-id="f6134-105">Команды выполняются в Visual Studio с помощью [консоли диспетчера пакетов](/nuget/tools/package-manager-console).</span><span class="sxs-lookup"><span data-stu-id="f6134-105">The commands run inside of Visual Studio using the [Package Manager Console](/nuget/tools/package-manager-console).</span></span> <span data-ttu-id="f6134-106">Эти инструменты совместимы с проектами .NET Framework и .NET Core.</span><span class="sxs-lookup"><span data-stu-id="f6134-106">These tools work with both .NET Framework and .NET Core projects.</span></span>

<span data-ttu-id="f6134-107">Если вы не используете Visual Studio, вместо этого рекомендуется использовать [средства командной строки EF Core](dotnet.md) .</span><span class="sxs-lookup"><span data-stu-id="f6134-107">If you aren't using Visual Studio, we recommend the [EF Core Command-line Tools](dotnet.md) instead.</span></span> <span data-ttu-id="f6134-108">Средства CLI выполняются на разных платформах и выполняются в командной строке.</span><span class="sxs-lookup"><span data-stu-id="f6134-108">The CLI tools are cross-platform and run inside a command prompt.</span></span>

## <a name="installing-the-tools"></a><span data-ttu-id="f6134-109">Установка инструментов</span><span class="sxs-lookup"><span data-stu-id="f6134-109">Installing the tools</span></span>

<span data-ttu-id="f6134-110">Процедуры установки и обновления инструментов отличаются от ASP.NET Core 2.1 + и более ранних версий или других типов проектов.</span><span class="sxs-lookup"><span data-stu-id="f6134-110">The procedures for installing and updating the tools differ between ASP.NET Core 2.1+ and earlier versions or other project types.</span></span>

### <a name="aspnet-core-version-21-and-later"></a><span data-ttu-id="f6134-111">ASP.NET Core версии 2,1 и более поздних</span><span class="sxs-lookup"><span data-stu-id="f6134-111">ASP.NET Core version 2.1 and later</span></span>

<span data-ttu-id="f6134-112">Эти средства автоматически включаются в проект ASP.NET Core 2.1 +, так как `Microsoft.EntityFrameworkCore.Tools` пакет входит в состав [метапакет Microsoft. AspNetCore. app](/aspnet/core/fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="f6134-112">The tools are automatically included in an ASP.NET Core 2.1+ project because the `Microsoft.EntityFrameworkCore.Tools` package is included in the [Microsoft.AspNetCore.App metapackage](/aspnet/core/fundamentals/metapackage-app).</span></span>

<span data-ttu-id="f6134-113">Поэтому вам не нужно ничего делать для установки средств, но вам нужно:</span><span class="sxs-lookup"><span data-stu-id="f6134-113">Therefore, you don't have to do anything to install the tools, but you do have to:</span></span>

* <span data-ttu-id="f6134-114">Восстановите пакеты перед использованием средств в новом проекте.</span><span class="sxs-lookup"><span data-stu-id="f6134-114">Restore packages before using the tools in a new project.</span></span>
* <span data-ttu-id="f6134-115">Установите пакет, чтобы обновить средства до более новой версии.</span><span class="sxs-lookup"><span data-stu-id="f6134-115">Install a package to update the tools to a newer version.</span></span>

<span data-ttu-id="f6134-116">Чтобы убедиться, что вы получаете последнюю версию средств, мы рекомендуем также выполнить следующие действия.</span><span class="sxs-lookup"><span data-stu-id="f6134-116">To make sure that you're getting the latest version of the tools, we recommend that you also do the following step:</span></span>

* <span data-ttu-id="f6134-117">Измените *CSPROJ* -файл и добавьте строку, указывающую последнюю версию пакета [Microsoft. EntityFrameworkCore. Tools](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools/) .</span><span class="sxs-lookup"><span data-stu-id="f6134-117">Edit your *.csproj* file and add a line specifying the latest version of the [Microsoft.EntityFrameworkCore.Tools](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools/) package.</span></span> <span data-ttu-id="f6134-118">Например, файл *. csproj* может включать в `ItemGroup` себя следующий вид:</span><span class="sxs-lookup"><span data-stu-id="f6134-118">For example, the *.csproj* file might include an `ItemGroup` that looks like this:</span></span>

  ```xml
  <ItemGroup>
    <PackageReference Include="Microsoft.AspNetCore.App" />
    <PackageReference Include="Microsoft.EntityFrameworkCore.Tools" Version="2.1.3" />
    <PackageReference Include="Microsoft.VisualStudio.Web.CodeGeneration.Design" Version="2.1.1" />
  </ItemGroup>
  ```

<span data-ttu-id="f6134-119">Обновите средства при получении сообщения, как в следующем примере:</span><span class="sxs-lookup"><span data-stu-id="f6134-119">Update the tools when you get a message like the following example:</span></span>

> <span data-ttu-id="f6134-120">Версия инструментов EF Core "короче-RTM-30846" старше, чем версия среды выполнения "2.1.3-RTM-32065".</span><span class="sxs-lookup"><span data-stu-id="f6134-120">The EF Core tools version '2.1.1-rtm-30846' is older than that of the runtime '2.1.3-rtm-32065'.</span></span> <span data-ttu-id="f6134-121">Обновите средства для получения последних компонентов и исправлений ошибок.</span><span class="sxs-lookup"><span data-stu-id="f6134-121">Update the tools for the latest features and bug fixes.</span></span>

<span data-ttu-id="f6134-122">Чтобы обновить средства, выполните следующие действия.</span><span class="sxs-lookup"><span data-stu-id="f6134-122">To update the tools:</span></span>

* <span data-ttu-id="f6134-123">Установите пакет SDK для .NET Core последней версии.</span><span class="sxs-lookup"><span data-stu-id="f6134-123">Install the latest .NET Core SDK.</span></span>
* <span data-ttu-id="f6134-124">Обновите Visual Studio до последней версии.</span><span class="sxs-lookup"><span data-stu-id="f6134-124">Update Visual Studio to the latest version.</span></span>
* <span data-ttu-id="f6134-125">Измените *CSPROJ* -файл, чтобы он включал ссылку на пакет для последней версии пакета инструментов, как показано выше.</span><span class="sxs-lookup"><span data-stu-id="f6134-125">Edit the *.csproj* file so that it includes a package reference to the latest tools package, as shown earlier.</span></span>

### <a name="other-versions-and-project-types"></a><span data-ttu-id="f6134-126">Другие версии и типы проектов</span><span class="sxs-lookup"><span data-stu-id="f6134-126">Other versions and project types</span></span>

<span data-ttu-id="f6134-127">Установите средства консоли диспетчера пакетов, выполнив следующую команду в **консоли диспетчера пакетов**:</span><span class="sxs-lookup"><span data-stu-id="f6134-127">Install the Package Manager Console tools by running the following command in **Package Manager Console**:</span></span>

``` powershell
Install-Package Microsoft.EntityFrameworkCore.Tools
```

<span data-ttu-id="f6134-128">Обновите средства, выполнив следующую команду в **консоли диспетчера пакетов**.</span><span class="sxs-lookup"><span data-stu-id="f6134-128">Update the tools by running the following command in **Package Manager Console**.</span></span>

``` powershell
Update-Package Microsoft.EntityFrameworkCore.Tools
```

### <a name="verify-the-installation"></a><span data-ttu-id="f6134-129">Проверка установки</span><span class="sxs-lookup"><span data-stu-id="f6134-129">Verify the installation</span></span>

<span data-ttu-id="f6134-130">Убедитесь, что средства установлены, выполнив следующую команду:</span><span class="sxs-lookup"><span data-stu-id="f6134-130">Verify that the tools are installed by running this command:</span></span>

``` powershell
Get-Help about_EntityFrameworkCore
```

<span data-ttu-id="f6134-131">Выходные данные выглядят следующим образом (он не указывает, какую версию инструментов вы используете):</span><span class="sxs-lookup"><span data-stu-id="f6134-131">The output looks like this (it doesn't tell you which version of the tools you're using):</span></span>

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

## <a name="using-the-tools"></a><span data-ttu-id="f6134-132">Использование средств</span><span class="sxs-lookup"><span data-stu-id="f6134-132">Using the tools</span></span>

<span data-ttu-id="f6134-133">Перед использованием средств:</span><span class="sxs-lookup"><span data-stu-id="f6134-133">Before using the tools:</span></span>

* <span data-ttu-id="f6134-134">Изучите разницу между целевым и запускаемым проектами.</span><span class="sxs-lookup"><span data-stu-id="f6134-134">Understand the difference between target and startup project.</span></span>
* <span data-ttu-id="f6134-135">Узнайте, как использовать средства с библиотеками классов .NET Standard.</span><span class="sxs-lookup"><span data-stu-id="f6134-135">Learn how to use the tools with .NET Standard class libraries.</span></span>
* <span data-ttu-id="f6134-136">Для проектов ASP.NET Core задайте среду.</span><span class="sxs-lookup"><span data-stu-id="f6134-136">For ASP.NET Core projects, set the environment.</span></span>

### <a name="target-and-startup-project"></a><span data-ttu-id="f6134-137">Целевой и запускаемый проекты</span><span class="sxs-lookup"><span data-stu-id="f6134-137">Target and startup project</span></span>

<span data-ttu-id="f6134-138">Команды ссылаются на *проект* и *запускаемый проект*.</span><span class="sxs-lookup"><span data-stu-id="f6134-138">The commands refer to a *project* and a *startup project*.</span></span>

* <span data-ttu-id="f6134-139">*Проект* также называется *целевым проектом* , так как он служит для добавления или удаления файлов.</span><span class="sxs-lookup"><span data-stu-id="f6134-139">The *project* is also known as the *target project* because it's where the commands add or remove files.</span></span> <span data-ttu-id="f6134-140">По умолчанию **проект** , выбранный в **консоли диспетчера пакетов** , является целевым.</span><span class="sxs-lookup"><span data-stu-id="f6134-140">By default, the **Default project** selected in **Package Manager Console** is the target project.</span></span> <span data-ttu-id="f6134-141">Можно указать другой проект в качестве целевого проекта с помощью <nobr>`--project`</nobr> параметра.</span><span class="sxs-lookup"><span data-stu-id="f6134-141">You can specify a different project as target project by using the <nobr>`--project`</nobr> option.</span></span>

* <span data-ttu-id="f6134-142">*Запускаемый проект* — это тот, который выполняет сборка и запуск средств.</span><span class="sxs-lookup"><span data-stu-id="f6134-142">The *startup project* is the one that the tools build and run.</span></span> <span data-ttu-id="f6134-143">Средства должны выполнять код приложения во время разработки для получения сведений о проекте, таких как строка подключения к базе данных и Конфигурация модели.</span><span class="sxs-lookup"><span data-stu-id="f6134-143">The tools have to execute application code at design time to get information about the project, such as the database connection string and the configuration of the model.</span></span> <span data-ttu-id="f6134-144">По умолчанию **запускаемым проектом** в **Обозреватель решений** является запускаемый проект.</span><span class="sxs-lookup"><span data-stu-id="f6134-144">By default, the **Startup Project** in **Solution Explorer** is the startup project.</span></span> <span data-ttu-id="f6134-145">Можно указать другой проект в качестве запускаемого проекта с помощью <nobr>`--startup-project`</nobr> параметра.</span><span class="sxs-lookup"><span data-stu-id="f6134-145">You can specify a different project as startup project by using the <nobr>`--startup-project`</nobr> option.</span></span>

<span data-ttu-id="f6134-146">Запускаемый проект и целевой проект часто являются одним и тем же проектом.</span><span class="sxs-lookup"><span data-stu-id="f6134-146">The startup project and target project are often the same project.</span></span> <span data-ttu-id="f6134-147">Типичный сценарий, в котором они являются отдельными проектами, состоит в следующих случаях:</span><span class="sxs-lookup"><span data-stu-id="f6134-147">A typical scenario where they are separate projects is when:</span></span>

* <span data-ttu-id="f6134-148">Контекст EF Core и классы сущностей находятся в библиотеке классов .NET Core.</span><span class="sxs-lookup"><span data-stu-id="f6134-148">The EF Core context and entity classes are in a .NET Core class library.</span></span>
* <span data-ttu-id="f6134-149">Консольное приложение .NET Core или веб-приложение ссылается на библиотеку классов.</span><span class="sxs-lookup"><span data-stu-id="f6134-149">A .NET Core console app or web app references the class library.</span></span>

<span data-ttu-id="f6134-150">Также можно [разместить код миграции в библиотеке классов, отдельном от контекста EF Core](xref:core/managing-schemas/migrations/projects).</span><span class="sxs-lookup"><span data-stu-id="f6134-150">It's also possible to [put migrations code in a class library separate from the EF Core context](xref:core/managing-schemas/migrations/projects).</span></span>

### <a name="other-target-frameworks"></a><span data-ttu-id="f6134-151">Другие целевые платформы</span><span class="sxs-lookup"><span data-stu-id="f6134-151">Other target frameworks</span></span>

<span data-ttu-id="f6134-152">Средства консоли диспетчера пакетов работают с проектами .NET Core или .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="f6134-152">The Package Manager Console tools work with .NET Core or .NET Framework projects.</span></span> <span data-ttu-id="f6134-153">Приложения с моделью EF Core в библиотеке классов .NET Standard могут не иметь проекта .NET Core или .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="f6134-153">Apps that have the EF Core model in a .NET Standard class library might not have a .NET Core or .NET Framework project.</span></span> <span data-ttu-id="f6134-154">Например, это справедливо для приложений Xamarin и универсальная платформа Windows.</span><span class="sxs-lookup"><span data-stu-id="f6134-154">For example, this is true of Xamarin and Universal Windows Platform apps.</span></span> <span data-ttu-id="f6134-155">В таких случаях можно создать проект консольного приложения .NET Core или .NET Framework, предназначенное только для запуска в качестве запускаемого проекта для инструментов.</span><span class="sxs-lookup"><span data-stu-id="f6134-155">In such cases, you can create a .NET Core or .NET Framework console app project whose only purpose is to act as startup project for the tools.</span></span> <span data-ttu-id="f6134-156">Проект может быть фиктивным проектом без реального кода &mdash; , поэтому он необходим только для предоставления целей для инструментов.</span><span class="sxs-lookup"><span data-stu-id="f6134-156">The project can be a dummy project with no real code &mdash; it is only needed to provide a target for the tooling.</span></span>

<span data-ttu-id="f6134-157">Зачем нужен фиктивный проект?</span><span class="sxs-lookup"><span data-stu-id="f6134-157">Why is a dummy project required?</span></span> <span data-ttu-id="f6134-158">Как упоминалось ранее, средства должны выполнять код приложения во время разработки.</span><span class="sxs-lookup"><span data-stu-id="f6134-158">As mentioned earlier, the tools have to execute application code at design time.</span></span> <span data-ttu-id="f6134-159">Для этого необходимо использовать среду выполнения .NET Core или .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="f6134-159">To do that, they need to use the .NET Core or .NET Framework runtime.</span></span> <span data-ttu-id="f6134-160">Если EF Coreная модель находится в проекте, ориентированном на .NET Core или .NET Framework, EF Core средства позаимствованы среду выполнения из проекта.</span><span class="sxs-lookup"><span data-stu-id="f6134-160">When the EF Core model is in a project that targets .NET Core or .NET Framework, the EF Core tools borrow the runtime from the project.</span></span> <span data-ttu-id="f6134-161">Они не могут сделать это, если модель EF Core находится в библиотеке классов .NET Standard.</span><span class="sxs-lookup"><span data-stu-id="f6134-161">They can't do that if the EF Core model is in a .NET Standard class library.</span></span> <span data-ttu-id="f6134-162">.NET Standard не является реальной реализацией .NET; это спецификация набора интерфейсов API, которые должны поддерживаться реализациями .NET.</span><span class="sxs-lookup"><span data-stu-id="f6134-162">The .NET Standard is not an actual .NET implementation; it's a specification of a set of APIs that .NET implementations must support.</span></span> <span data-ttu-id="f6134-163">Поэтому .NET Standard недостаточно для того, чтобы средства EF Core выполняли код приложения.</span><span class="sxs-lookup"><span data-stu-id="f6134-163">Therefore .NET Standard is not sufficient for the EF Core tools to execute application code.</span></span> <span data-ttu-id="f6134-164">Фиктивный проект, который создается для использования в качестве запускаемого проекта, предоставляет конкретную целевую платформу, в которую средства могут загрузить библиотеку классов .NET Standard.</span><span class="sxs-lookup"><span data-stu-id="f6134-164">The dummy project you create to use as startup project provides a concrete target platform into which the tools can load the .NET Standard class library.</span></span>

### <a name="aspnet-core-environment"></a><span data-ttu-id="f6134-165">Среда ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="f6134-165">ASP.NET Core environment</span></span>

<span data-ttu-id="f6134-166">Чтобы указать среду для проектов ASP.NET Core, установите параметр **env: ASPNETCORE_ENVIRONMENT** перед выполнением команд.</span><span class="sxs-lookup"><span data-stu-id="f6134-166">To specify the environment for ASP.NET Core projects, set **env:ASPNETCORE_ENVIRONMENT** before running commands.</span></span>

## <a name="common-parameters"></a><span data-ttu-id="f6134-167">Общие параметры</span><span class="sxs-lookup"><span data-stu-id="f6134-167">Common parameters</span></span>

<span data-ttu-id="f6134-168">В следующей таблице приведены параметры, которые являются общими для всех EF Coreных команд.</span><span class="sxs-lookup"><span data-stu-id="f6134-168">The following table shows parameters that are common to all of the EF Core commands:</span></span>

| <span data-ttu-id="f6134-169">Параметр</span><span class="sxs-lookup"><span data-stu-id="f6134-169">Parameter</span></span>                 | <span data-ttu-id="f6134-170">Описание</span><span class="sxs-lookup"><span data-stu-id="f6134-170">Description</span></span>                                                                                                                                                                                                          |
|:--------------------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="f6134-171">— Строка \<контекста></span><span class="sxs-lookup"><span data-stu-id="f6134-171">-Context \<String></span></span>        | <span data-ttu-id="f6134-172">Класс `DbContext` для использования.</span><span class="sxs-lookup"><span data-stu-id="f6134-172">The `DbContext` class to use.</span></span> <span data-ttu-id="f6134-173">Только имя класса или полный квалификатор с пространствами имен.</span><span class="sxs-lookup"><span data-stu-id="f6134-173">Class name only or fully qualified with namespaces.</span></span>  <span data-ttu-id="f6134-174">Если этот параметр опущен, EF Core находит класс контекста.</span><span class="sxs-lookup"><span data-stu-id="f6134-174">If this parameter is omitted, EF Core finds the context class.</span></span> <span data-ttu-id="f6134-175">Если имеется несколько классов контекста, этот параметр является обязательным.</span><span class="sxs-lookup"><span data-stu-id="f6134-175">If there are multiple context classes, this parameter is required.</span></span> |
| <span data-ttu-id="f6134-176">-Строковый> проекта \<</span><span class="sxs-lookup"><span data-stu-id="f6134-176">-Project \<String></span></span>        | <span data-ttu-id="f6134-177">Целевой проект.</span><span class="sxs-lookup"><span data-stu-id="f6134-177">The target project.</span></span> <span data-ttu-id="f6134-178">Если этот параметр не указан, в качестве целевого проекта используется **проект по умолчанию** для **консоли диспетчера пакетов** .</span><span class="sxs-lookup"><span data-stu-id="f6134-178">If this parameter is omitted, the **Default project** for **Package Manager Console** is used as the target project.</span></span>                                                                             |
| <span data-ttu-id="f6134-179"><nobr>-Стартуппрожект</nobr> \<строка></span><span class="sxs-lookup"><span data-stu-id="f6134-179"><nobr>-StartupProject</nobr> \<String></span></span> | <span data-ttu-id="f6134-180">Автоматически запускаемый проект.</span><span class="sxs-lookup"><span data-stu-id="f6134-180">The startup project.</span></span> <span data-ttu-id="f6134-181">Если этот параметр не указан, в качестве целевого проекта используется **запускаемый проект** в **свойствах решения** .</span><span class="sxs-lookup"><span data-stu-id="f6134-181">If this parameter is omitted, the **Startup project** in **Solution properties** is used as the target project.</span></span>                                                                                 |
| <span data-ttu-id="f6134-182">-Verbose</span><span class="sxs-lookup"><span data-stu-id="f6134-182">-Verbose</span></span>                  | <span data-ttu-id="f6134-183">Отображение подробных выходных данных.</span><span class="sxs-lookup"><span data-stu-id="f6134-183">Show verbose output.</span></span>                                                                                                                                                                                                 |

<span data-ttu-id="f6134-184">Чтобы отобразить справочные сведения о команде, используйте `Get-Help` команду PowerShell.</span><span class="sxs-lookup"><span data-stu-id="f6134-184">To show help information about a command, use PowerShell's `Get-Help` command.</span></span>

> [!TIP]
> <span data-ttu-id="f6134-185">Параметры Context, Project и Стартуппрожект поддерживают расширение Tab.</span><span class="sxs-lookup"><span data-stu-id="f6134-185">The Context, Project, and StartupProject parameters support tab-expansion.</span></span>

## <a name="add-migration"></a><span data-ttu-id="f6134-186">Добавление и миграция</span><span class="sxs-lookup"><span data-stu-id="f6134-186">Add-Migration</span></span>

<span data-ttu-id="f6134-187">Добавляет новый перенос.</span><span class="sxs-lookup"><span data-stu-id="f6134-187">Adds a new migration.</span></span>

<span data-ttu-id="f6134-188">Параметры</span><span class="sxs-lookup"><span data-stu-id="f6134-188">Parameters:</span></span>

| <span data-ttu-id="f6134-189">Параметр</span><span class="sxs-lookup"><span data-stu-id="f6134-189">Parameter</span></span>                         | <span data-ttu-id="f6134-190">Описание</span><span class="sxs-lookup"><span data-stu-id="f6134-190">Description</span></span>                                                                                                             |
|:----------------------------------|:------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="f6134-191"><nobr>— Строка \<имени><nobr></span><span class="sxs-lookup"><span data-stu-id="f6134-191"><nobr>-Name \<String><nobr></span></span>       | <span data-ttu-id="f6134-192">Имя миграции.</span><span class="sxs-lookup"><span data-stu-id="f6134-192">The name of the migration.</span></span> <span data-ttu-id="f6134-193">Этот параметр является позиционированным и является обязательным.</span><span class="sxs-lookup"><span data-stu-id="f6134-193">This is a positional parameter and is required.</span></span>                                              |
| <span data-ttu-id="f6134-194"><nobr>-OutputDir \<строка></nobr></span><span class="sxs-lookup"><span data-stu-id="f6134-194"><nobr>-OutputDir \<String></nobr></span></span> | <span data-ttu-id="f6134-195">Каталог используется для вывода файлов.</span><span class="sxs-lookup"><span data-stu-id="f6134-195">The directory use to output the files.</span></span> <span data-ttu-id="f6134-196">Пути задаются относительно целевого каталога проекта.</span><span class="sxs-lookup"><span data-stu-id="f6134-196">Paths are relative to the target project directory.</span></span> <span data-ttu-id="f6134-197">По умолчанию используется значение "миграции".</span><span class="sxs-lookup"><span data-stu-id="f6134-197">Defaults to "Migrations".</span></span> |
| <span data-ttu-id="f6134-198"><nobr>— Строка \<пространства имен></nobr></span><span class="sxs-lookup"><span data-stu-id="f6134-198"><nobr>-Namespace \<String></nobr></span></span> | <span data-ttu-id="f6134-199">Пространство имен, используемое для создаваемых классов.</span><span class="sxs-lookup"><span data-stu-id="f6134-199">The namespace to use for the generated classes.</span></span> <span data-ttu-id="f6134-200">По умолчанию создается из выходного каталога.</span><span class="sxs-lookup"><span data-stu-id="f6134-200">Defaults to generated from the output directory.</span></span> |

## <a name="drop-database"></a><span data-ttu-id="f6134-201">Удалить базу данных</span><span class="sxs-lookup"><span data-stu-id="f6134-201">Drop-Database</span></span>

<span data-ttu-id="f6134-202">Удаляет базу данных.</span><span class="sxs-lookup"><span data-stu-id="f6134-202">Drops the database.</span></span>

<span data-ttu-id="f6134-203">Параметры</span><span class="sxs-lookup"><span data-stu-id="f6134-203">Parameters:</span></span>

| <span data-ttu-id="f6134-204">Параметр</span><span class="sxs-lookup"><span data-stu-id="f6134-204">Parameter</span></span> | <span data-ttu-id="f6134-205">Описание</span><span class="sxs-lookup"><span data-stu-id="f6134-205">Description</span></span>                                              |
|:----------|:---------------------------------------------------------|
| <span data-ttu-id="f6134-206">-WhatIf</span><span class="sxs-lookup"><span data-stu-id="f6134-206">-WhatIf</span></span>   | <span data-ttu-id="f6134-207">Показывает, какую базу данных следует удалить, но не удалять.</span><span class="sxs-lookup"><span data-stu-id="f6134-207">Show which database would be dropped, but don't drop it.</span></span> |

## <a name="get-dbcontext"></a><span data-ttu-id="f6134-208">Get-DbContext</span><span class="sxs-lookup"><span data-stu-id="f6134-208">Get-DbContext</span></span>

<span data-ttu-id="f6134-209">Возвращает сведения о `DbContext` типе.</span><span class="sxs-lookup"><span data-stu-id="f6134-209">Gets information about a `DbContext` type.</span></span>

## <a name="remove-migration"></a><span data-ttu-id="f6134-210">Remove-Migration</span><span class="sxs-lookup"><span data-stu-id="f6134-210">Remove-Migration</span></span>

<span data-ttu-id="f6134-211">Удаляет последнюю миграцию (выполняет откат изменений кода, выполненных для миграции).</span><span class="sxs-lookup"><span data-stu-id="f6134-211">Removes the last migration (rolls back the code changes that were done for the migration).</span></span>

<span data-ttu-id="f6134-212">Параметры</span><span class="sxs-lookup"><span data-stu-id="f6134-212">Parameters:</span></span>

| <span data-ttu-id="f6134-213">Параметр</span><span class="sxs-lookup"><span data-stu-id="f6134-213">Parameter</span></span> | <span data-ttu-id="f6134-214">Описание</span><span class="sxs-lookup"><span data-stu-id="f6134-214">Description</span></span>                                                                     |
|:----------|:--------------------------------------------------------------------------------|
| <span data-ttu-id="f6134-215">-Force</span><span class="sxs-lookup"><span data-stu-id="f6134-215">-Force</span></span>    | <span data-ttu-id="f6134-216">Отмените миграцию (выполните откат изменений, примененных к базе данных).</span><span class="sxs-lookup"><span data-stu-id="f6134-216">Revert the migration (roll back the changes that were applied to the database).</span></span> |

## <a name="scaffold-dbcontext"></a><span data-ttu-id="f6134-217">Формирование шаблонов — DbContext</span><span class="sxs-lookup"><span data-stu-id="f6134-217">Scaffold-DbContext</span></span>

<span data-ttu-id="f6134-218">Создает код для типов `DbContext` сущностей и для базы данных.</span><span class="sxs-lookup"><span data-stu-id="f6134-218">Generates code for a `DbContext` and entity types for a database.</span></span> <span data-ttu-id="f6134-219">`Scaffold-DbContext` Чтобы создать тип сущности, таблица базы данных должна иметь первичный ключ.</span><span class="sxs-lookup"><span data-stu-id="f6134-219">In order for `Scaffold-DbContext` to generate an entity type, the database table must have a primary key.</span></span>

<span data-ttu-id="f6134-220">Параметры</span><span class="sxs-lookup"><span data-stu-id="f6134-220">Parameters:</span></span>

| <span data-ttu-id="f6134-221">Параметр</span><span class="sxs-lookup"><span data-stu-id="f6134-221">Parameter</span></span>                          | <span data-ttu-id="f6134-222">Описание</span><span class="sxs-lookup"><span data-stu-id="f6134-222">Description</span></span>                                                                                                                                                                                                                                                             |
|:-----------------------------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="f6134-223"><nobr>— Строка \<подключения></nobr></span><span class="sxs-lookup"><span data-stu-id="f6134-223"><nobr>-Connection \<String></nobr></span></span> | <span data-ttu-id="f6134-224">Строка подключения к базе данных.</span><span class="sxs-lookup"><span data-stu-id="f6134-224">The connection string to the database.</span></span> <span data-ttu-id="f6134-225">Для проектов ASP.NET Core 2. x значением может быть *Name =\<name строки подключения>*.</span><span class="sxs-lookup"><span data-stu-id="f6134-225">For ASP.NET Core 2.x projects, the value can be *name=\<name of connection string>*.</span></span> <span data-ttu-id="f6134-226">В этом случае имя берется из источников конфигурации, настроенных для проекта.</span><span class="sxs-lookup"><span data-stu-id="f6134-226">In that case the name comes from the configuration sources that are set up for the project.</span></span> <span data-ttu-id="f6134-227">Этот параметр является позиционированным и является обязательным.</span><span class="sxs-lookup"><span data-stu-id="f6134-227">This is a positional parameter and is required.</span></span> |
| <span data-ttu-id="f6134-228"><nobr>— Строка \<поставщика></nobr></span><span class="sxs-lookup"><span data-stu-id="f6134-228"><nobr>-Provider \<String></nobr></span></span>   | <span data-ttu-id="f6134-229">Используемый поставщик.</span><span class="sxs-lookup"><span data-stu-id="f6134-229">The provider to use.</span></span> <span data-ttu-id="f6134-230">Обычно это имя пакета NuGet, например: `Microsoft.EntityFrameworkCore.SqlServer`.</span><span class="sxs-lookup"><span data-stu-id="f6134-230">Typically this is the name of the NuGet package, for example: `Microsoft.EntityFrameworkCore.SqlServer`.</span></span> <span data-ttu-id="f6134-231">Этот параметр является позиционированным и является обязательным.</span><span class="sxs-lookup"><span data-stu-id="f6134-231">This is a positional parameter and is required.</span></span>                                                                                           |
| <span data-ttu-id="f6134-232">-OutputDir \<строка></span><span class="sxs-lookup"><span data-stu-id="f6134-232">-OutputDir \<String></span></span>               | <span data-ttu-id="f6134-233">Каталог для размещения файлов.</span><span class="sxs-lookup"><span data-stu-id="f6134-233">The directory to put files in.</span></span> <span data-ttu-id="f6134-234">Пути задаются относительно каталога проекта.</span><span class="sxs-lookup"><span data-stu-id="f6134-234">Paths are relative to the project directory.</span></span>                                                                                                                                                                                             |
| <span data-ttu-id="f6134-235">-Контекстдир \<строка></span><span class="sxs-lookup"><span data-stu-id="f6134-235">-ContextDir \<String></span></span>              | <span data-ttu-id="f6134-236">Каталог, в котором следует `DbContext` разместить файл.</span><span class="sxs-lookup"><span data-stu-id="f6134-236">The directory to put the `DbContext` file in.</span></span> <span data-ttu-id="f6134-237">Пути задаются относительно каталога проекта.</span><span class="sxs-lookup"><span data-stu-id="f6134-237">Paths are relative to the project directory.</span></span>                                                                                                                                                               |
| <span data-ttu-id="f6134-238">— Строка \<пространства имен></span><span class="sxs-lookup"><span data-stu-id="f6134-238">-Namespace \<String></span></span>               | <span data-ttu-id="f6134-239">Пространство имен, используемое для всех создаваемых классов.</span><span class="sxs-lookup"><span data-stu-id="f6134-239">The namespace to use for all generated classes.</span></span> <span data-ttu-id="f6134-240">По умолчанию создается из корневого пространства имен и выходного каталога.</span><span class="sxs-lookup"><span data-stu-id="f6134-240">Defaults to generated from the root namespace and the output directory.</span></span>                                                                                                                                                                                             |
| <span data-ttu-id="f6134-241">-ContextNamespace \<строка></span><span class="sxs-lookup"><span data-stu-id="f6134-241">-ContextNamespace \<String></span></span>        | <span data-ttu-id="f6134-242">Пространство имен, используемое для создаваемого `DbContext` класса.</span><span class="sxs-lookup"><span data-stu-id="f6134-242">The namespace to use for the generated `DbContext` class.</span></span> <span data-ttu-id="f6134-243">Примечание. Переопределение `-Namespace`.</span><span class="sxs-lookup"><span data-stu-id="f6134-243">Note: overrides `-Namespace`.</span></span>                                                                                                                                                                              |
| <span data-ttu-id="f6134-244">— Строка \<контекста></span><span class="sxs-lookup"><span data-stu-id="f6134-244">-Context \<String></span></span>                 | <span data-ttu-id="f6134-245">Имя создаваемого `DbContext` класса.</span><span class="sxs-lookup"><span data-stu-id="f6134-245">The name of the `DbContext` class to generate.</span></span>                                                                                                                                                                                                                          |
| <span data-ttu-id="f6134-246">-Schemas \<строка [] ></span><span class="sxs-lookup"><span data-stu-id="f6134-246">-Schemas \<String[]></span></span>               | <span data-ttu-id="f6134-247">Схемы таблиц, для которых создаются типы сущностей.</span><span class="sxs-lookup"><span data-stu-id="f6134-247">The schemas of tables to generate entity types for.</span></span> <span data-ttu-id="f6134-248">Если этот параметр пропущен, включаются все схемы.</span><span class="sxs-lookup"><span data-stu-id="f6134-248">If this parameter is omitted, all schemas are included.</span></span>                                                                                                                                                             |
| <span data-ttu-id="f6134-249">-Tables \<String [] ></span><span class="sxs-lookup"><span data-stu-id="f6134-249">-Tables \<String[]></span></span>                | <span data-ttu-id="f6134-250">Таблицы, для которых создаются типы сущностей.</span><span class="sxs-lookup"><span data-stu-id="f6134-250">The tables to generate entity types for.</span></span> <span data-ttu-id="f6134-251">Если этот параметр пропущен, включаются все таблицы.</span><span class="sxs-lookup"><span data-stu-id="f6134-251">If this parameter is omitted, all tables are included.</span></span>                                                                                                                                                                         |
| <span data-ttu-id="f6134-252">-Аннотации</span><span class="sxs-lookup"><span data-stu-id="f6134-252">-DataAnnotations</span></span>                   | <span data-ttu-id="f6134-253">Используйте атрибуты для настройки модели (по возможности).</span><span class="sxs-lookup"><span data-stu-id="f6134-253">Use attributes to configure the model (where possible).</span></span> <span data-ttu-id="f6134-254">Если этот параметр пропущен, используется только API-интерфейс Fluent.</span><span class="sxs-lookup"><span data-stu-id="f6134-254">If this parameter is omitted, only the fluent API is used.</span></span>                                                                                                                                                      |
| <span data-ttu-id="f6134-255">-Уседатабасенамес</span><span class="sxs-lookup"><span data-stu-id="f6134-255">-UseDatabaseNames</span></span>                  | <span data-ttu-id="f6134-256">Имена таблиц и столбцов следует использовать в точности так, как они отображаются в базе данных.</span><span class="sxs-lookup"><span data-stu-id="f6134-256">Use table and column names exactly as they appear in the database.</span></span> <span data-ttu-id="f6134-257">Если этот параметр опущен, имена баз данных изменяются в соответствии с соглашениями о стиле имени C#.</span><span class="sxs-lookup"><span data-stu-id="f6134-257">If this parameter is omitted, database names are changed to more closely conform to C# name style conventions.</span></span>                                                                                       |
| <span data-ttu-id="f6134-258">-Force</span><span class="sxs-lookup"><span data-stu-id="f6134-258">-Force</span></span>                             | <span data-ttu-id="f6134-259">Перезаписать существующие файлы.</span><span class="sxs-lookup"><span data-stu-id="f6134-259">Overwrite existing files.</span></span>                                                                                                                                                                                                                                               |

<span data-ttu-id="f6134-260">Пример:</span><span class="sxs-lookup"><span data-stu-id="f6134-260">Example:</span></span>

```powershell
Scaffold-DbContext "Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer -OutputDir Models
```

<span data-ttu-id="f6134-261">Пример: формирование шаблонов только для выбранных таблиц и создание контекста в отдельной папке с указанным именем и пространством имен.</span><span class="sxs-lookup"><span data-stu-id="f6134-261">Example that scaffolds only selected tables and creates the context in a separate folder with a specified name and namespace:</span></span>

```powershell
Scaffold-DbContext "Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer -OutputDir Models -Tables "Blog","Post" -ContextDir Context -Context BlogContext -ContextNamespace New.Namespace
```

## <a name="script-migration"></a><span data-ttu-id="f6134-262">Скрипт — миграция</span><span class="sxs-lookup"><span data-stu-id="f6134-262">Script-Migration</span></span>

<span data-ttu-id="f6134-263">Создает скрипт SQL, который применяет все изменения из одной выбранной миграции в другую выбранную миграцию.</span><span class="sxs-lookup"><span data-stu-id="f6134-263">Generates a SQL script that applies all of the changes from one selected migration to another selected migration.</span></span>

<span data-ttu-id="f6134-264">Параметры</span><span class="sxs-lookup"><span data-stu-id="f6134-264">Parameters:</span></span>

| <span data-ttu-id="f6134-265">Параметр</span><span class="sxs-lookup"><span data-stu-id="f6134-265">Parameter</span></span>                | <span data-ttu-id="f6134-266">Описание</span><span class="sxs-lookup"><span data-stu-id="f6134-266">Description</span></span>                                                                                                                                                                                                                |
|:-------------------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="f6134-267">*-From* \<строка></span><span class="sxs-lookup"><span data-stu-id="f6134-267">*-From* \<String></span></span>        | <span data-ttu-id="f6134-268">Начало миграции.</span><span class="sxs-lookup"><span data-stu-id="f6134-268">The starting migration.</span></span> <span data-ttu-id="f6134-269">Миграция может быть идентифицирована по имени или по ИДЕНТИФИКАТОРу.</span><span class="sxs-lookup"><span data-stu-id="f6134-269">Migrations may be identified by name or by ID.</span></span> <span data-ttu-id="f6134-270">Число 0 — это особый случай, который означает *перед первой миграцией*.</span><span class="sxs-lookup"><span data-stu-id="f6134-270">The number 0 is a special case that means *before the first migration*.</span></span> <span data-ttu-id="f6134-271">Значение по умолчанию — 0.</span><span class="sxs-lookup"><span data-stu-id="f6134-271">Defaults to 0.</span></span>                                                              |
| <span data-ttu-id="f6134-272">*-To* \<String></span><span class="sxs-lookup"><span data-stu-id="f6134-272">*-To* \<String></span></span>          | <span data-ttu-id="f6134-273">Завершение миграции.</span><span class="sxs-lookup"><span data-stu-id="f6134-273">The ending migration.</span></span> <span data-ttu-id="f6134-274">По умолчанию используется последняя миграция.</span><span class="sxs-lookup"><span data-stu-id="f6134-274">Defaults to the last migration.</span></span>                                                                                                                                                                      |
| <span data-ttu-id="f6134-275"><nobr>-Идемпотентными</nobr></span><span class="sxs-lookup"><span data-stu-id="f6134-275"><nobr>-Idempotent</nobr></span></span> | <span data-ttu-id="f6134-276">Создание скрипта, который может использоваться в базе данных при любой миграции.</span><span class="sxs-lookup"><span data-stu-id="f6134-276">Generate a script that can be used on a database at any migration.</span></span>                                                                                                                                                         |
| <span data-ttu-id="f6134-277">— Выходная \<строка></span><span class="sxs-lookup"><span data-stu-id="f6134-277">-Output \<String></span></span>        | <span data-ttu-id="f6134-278">Файл, в который записывается результат.</span><span class="sxs-lookup"><span data-stu-id="f6134-278">The file to write the result to.</span></span> <span data-ttu-id="f6134-279">Если этот параметр не указан, файл создается с именем, созданным в той же папке, что и файлы среды выполнения приложения, например: */obj/Debug/netcoreapp2.1/ghbkztfz.SQL/*.</span><span class="sxs-lookup"><span data-stu-id="f6134-279">IF this parameter is omitted, the file is created with a generated name in the same folder as the app's runtime files are created, for example: */obj/Debug/netcoreapp2.1/ghbkztfz.sql/*.</span></span> |

> [!TIP]
> <span data-ttu-id="f6134-280">Параметры to, from и Output поддерживают расширение клавишей TAB.</span><span class="sxs-lookup"><span data-stu-id="f6134-280">The To, From, and Output parameters support tab-expansion.</span></span>

<span data-ttu-id="f6134-281">В следующем примере создается скрипт для миграции InitialCreate с использованием имени миграции.</span><span class="sxs-lookup"><span data-stu-id="f6134-281">The following example creates a script for the InitialCreate migration, using the migration name.</span></span>

```powershell
Script-Migration -To InitialCreate
```

<span data-ttu-id="f6134-282">В следующем примере создается скрипт для всех миграций после миграции InitialCreate с использованием идентификатора миграции.</span><span class="sxs-lookup"><span data-stu-id="f6134-282">The following example creates a script for all migrations after the InitialCreate migration, using the migration ID.</span></span>

```powershell
Script-Migration -From 20180904195021_InitialCreate
```

## <a name="update-database"></a><span data-ttu-id="f6134-283">Обновление базы данных</span><span class="sxs-lookup"><span data-stu-id="f6134-283">Update-Database</span></span>

<span data-ttu-id="f6134-284">Обновляет базу данных до последней миграции или до указанной миграции.</span><span class="sxs-lookup"><span data-stu-id="f6134-284">Updates the database to the last migration or to a specified migration.</span></span>

| <span data-ttu-id="f6134-285">Параметр</span><span class="sxs-lookup"><span data-stu-id="f6134-285">Parameter</span></span>                           | <span data-ttu-id="f6134-286">Описание</span><span class="sxs-lookup"><span data-stu-id="f6134-286">Description</span></span>                                                                                                                                                                                                                                                     |
|:------------------------------------|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="f6134-287"><nobr>*-Строка миграции* \<></nobr></span><span class="sxs-lookup"><span data-stu-id="f6134-287"><nobr>*-Migration* \<String></nobr></span></span> | <span data-ttu-id="f6134-288">Целевая миграция.</span><span class="sxs-lookup"><span data-stu-id="f6134-288">The target migration.</span></span> <span data-ttu-id="f6134-289">Миграция может быть идентифицирована по имени или по ИДЕНТИФИКАТОРу.</span><span class="sxs-lookup"><span data-stu-id="f6134-289">Migrations may be identified by name or by ID.</span></span> <span data-ttu-id="f6134-290">Число 0 — это особый случай, который означает *перед первой миграцией* и приводит к отмене всех миграций.</span><span class="sxs-lookup"><span data-stu-id="f6134-290">The number 0 is a special case that means *before the first migration* and causes all migrations to be reverted.</span></span> <span data-ttu-id="f6134-291">Если миграция не указана, команда по умолчанию принимает значение последней миграции.</span><span class="sxs-lookup"><span data-stu-id="f6134-291">If no migration is specified, the command defaults to the last migration.</span></span> |
| <span data-ttu-id="f6134-292"><nobr>— Строка \<подключения></nobr></span><span class="sxs-lookup"><span data-stu-id="f6134-292"><nobr>-Connection \<String></nobr></span></span>  | <span data-ttu-id="f6134-293">Строка подключения к базе данных.</span><span class="sxs-lookup"><span data-stu-id="f6134-293">The connection string to the database.</span></span> <span data-ttu-id="f6134-294">По умолчанию используется значение, `AddDbContext` заданное `OnConfiguring`в или.</span><span class="sxs-lookup"><span data-stu-id="f6134-294">Defaults to the one specified in `AddDbContext` or `OnConfiguring`.</span></span> |

> [!TIP]
> <span data-ttu-id="f6134-295">Параметр миграции поддерживает расширение клавиш TAB.</span><span class="sxs-lookup"><span data-stu-id="f6134-295">The Migration parameter supports tab-expansion.</span></span>

<span data-ttu-id="f6134-296">В следующем примере возвращаются все миграции.</span><span class="sxs-lookup"><span data-stu-id="f6134-296">The following example reverts all migrations.</span></span>

```powershell
Update-Database -Migration 0
```

<span data-ttu-id="f6134-297">В следующих примерах база данных обновляется до указанной миграции.</span><span class="sxs-lookup"><span data-stu-id="f6134-297">The following examples update the database to a specified migration.</span></span> <span data-ttu-id="f6134-298">В первом случае используется имя миграции, а во втором используется идентификатор миграции и указанное соединение:</span><span class="sxs-lookup"><span data-stu-id="f6134-298">The first uses the migration name and the second uses the migration ID and a specified connection:</span></span>

```powershell
Update-Database -Migration InitialCreate
Update-Database -Migration 20180904195021_InitialCreate -Connection your_connection_string
```

## <a name="additional-resources"></a><span data-ttu-id="f6134-299">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="f6134-299">Additional resources</span></span>

* [<span data-ttu-id="f6134-300">Миграции</span><span class="sxs-lookup"><span data-stu-id="f6134-300">Migrations</span></span>](xref:core/managing-schemas/migrations/index)
* [<span data-ttu-id="f6134-301">Реконструирование</span><span class="sxs-lookup"><span data-stu-id="f6134-301">Reverse Engineering</span></span>](xref:core/managing-schemas/scaffolding)
