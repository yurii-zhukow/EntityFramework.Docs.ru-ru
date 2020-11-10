---
title: Службы времени разработки — EF Core
description: Сведения о Entity Framework Core служб времени разработки
author: bricelam
ms.date: 10/22/2020
uid: core/cli/services
ms.openlocfilehash: 07fc8a39fb106f357e135a17f6867ab338621910
ms.sourcegitcommit: f3512e3a98e685a3ba409c1d0157ce85cc390cf4
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2020
ms.locfileid: "94431355"
---
# <a name="design-time-services"></a><span data-ttu-id="c00d9-103">Службы времени разработки</span><span class="sxs-lookup"><span data-stu-id="c00d9-103">Design-time services</span></span>

<span data-ttu-id="c00d9-104">Некоторые службы, используемые этими инструментами, используются только во время разработки.</span><span class="sxs-lookup"><span data-stu-id="c00d9-104">Some services used by the tools are only used at design time.</span></span> <span data-ttu-id="c00d9-105">Эти службы управляются отдельно от служб среды выполнения EF Core, чтобы предотвратить их развертывание в приложении.</span><span class="sxs-lookup"><span data-stu-id="c00d9-105">These services are managed separately from EF Core's runtime services to prevent them from being deployed with your app.</span></span> <span data-ttu-id="c00d9-106">Чтобы переопределить одну из этих служб (например, службу для создания файлов миграции), добавьте реализацию `IDesignTimeServices` в запускаемый проект.</span><span class="sxs-lookup"><span data-stu-id="c00d9-106">To override one of these services (for example the service to generate migration files), add an implementation of `IDesignTimeServices` to your startup project.</span></span>

[!code-csharp[Main](../../../samples/core/Miscellaneous/CommandLine/DesignTimeServices.cs#DesignTimeServices)]

## <a name="referencing-microsoftentityframeworkcoredesign"></a><span data-ttu-id="c00d9-107">Ссылка на Microsoft. EntityFrameworkCore. Design</span><span class="sxs-lookup"><span data-stu-id="c00d9-107">Referencing Microsoft.EntityFrameworkCore.Design</span></span>

<span data-ttu-id="c00d9-108">Microsoft. EntityFrameworkCore. Design — это пакет DevelopmentDependency.</span><span class="sxs-lookup"><span data-stu-id="c00d9-108">Microsoft.EntityFrameworkCore.Design is a DevelopmentDependency package.</span></span> <span data-ttu-id="c00d9-109">Это означает, что зависимость не будет передаваться в другие проекты и не может быть по умолчанию ссылаться на ее типы.</span><span class="sxs-lookup"><span data-stu-id="c00d9-109">This means that the dependency won't flow transitively into other projects, and that you cannot, by default, reference its types.</span></span>

<span data-ttu-id="c00d9-110">Чтобы сослаться на типы и переопределить службы времени разработки, обновите метаданные элемента PackageReference в файле проекта.</span><span class="sxs-lookup"><span data-stu-id="c00d9-110">In order to reference its types and override design-time services, update the PackageReference item's metadata in your project file.</span></span>

```xml
<PackageReference Include="Microsoft.EntityFrameworkCore.Design" Version="3.1.9">
  <PrivateAssets>all</PrivateAssets>
  <!-- Remove IncludeAssets to allow compiling against the assembly -->
  <!--<IncludeAssets>runtime; build; native; contentfiles; analyzers; buildtransitive</IncludeAssets>-->
</PackageReference>
```

<span data-ttu-id="c00d9-111">Если пакет упоминается транзитным образом через Microsoft. EntityFrameworkCore. Tools, необходимо добавить явный PackageReference в пакет и изменить его метаданные.</span><span class="sxs-lookup"><span data-stu-id="c00d9-111">If the package is being referenced transitively via Microsoft.EntityFrameworkCore.Tools, you will need to add an explicit PackageReference to the package and change its metadata.</span></span>

## <a name="list-of-services"></a><span data-ttu-id="c00d9-112">Список служб</span><span class="sxs-lookup"><span data-stu-id="c00d9-112">List of services</span></span>

<span data-ttu-id="c00d9-113">Ниже приведен список служб времени разработки.</span><span class="sxs-lookup"><span data-stu-id="c00d9-113">The following is a list of the design-time services.</span></span>

<span data-ttu-id="c00d9-114">Служба</span><span class="sxs-lookup"><span data-stu-id="c00d9-114">Service</span></span>                                                                              | <span data-ttu-id="c00d9-115">Описание</span><span class="sxs-lookup"><span data-stu-id="c00d9-115">Description</span></span>
------------------------------------------------------------------------------------ | -----------
<xref:Microsoft.EntityFrameworkCore.Design.IAnnotationCodeGenerator>                 | <span data-ttu-id="c00d9-116">Создает код для соответствующих заметок модели.</span><span class="sxs-lookup"><span data-stu-id="c00d9-116">Generates the code for corresponding model annotations.</span></span>
<xref:Microsoft.EntityFrameworkCore.Design.ICSharpHelper>                            | <span data-ttu-id="c00d9-117">Помогает создавать код C#.</span><span class="sxs-lookup"><span data-stu-id="c00d9-117">Helps with generating C# code.</span></span>
<xref:Microsoft.EntityFrameworkCore.Design.IPluralizer>                              | <span data-ttu-id="c00d9-118">Перевод и сингуларизес слова.</span><span class="sxs-lookup"><span data-stu-id="c00d9-118">Pluralizes and singularizes words.</span></span>
<xref:Microsoft.EntityFrameworkCore.Migrations.Design.IMigrationsCodeGenerator>      | <span data-ttu-id="c00d9-119">Создает код для миграции.</span><span class="sxs-lookup"><span data-stu-id="c00d9-119">Generates code for a migration.</span></span>
<xref:Microsoft.EntityFrameworkCore.Migrations.Design.IMigrationsScaffolder>         | <span data-ttu-id="c00d9-120">Основной класс для управления файлами миграции.</span><span class="sxs-lookup"><span data-stu-id="c00d9-120">The main class for managing migration files.</span></span>
<xref:Microsoft.EntityFrameworkCore.Scaffolding.IDatabaseModelFactory>               | <span data-ttu-id="c00d9-121">Создает модель базы данных на основе базы данных.</span><span class="sxs-lookup"><span data-stu-id="c00d9-121">Creates a database model from a database.</span></span>
<xref:Microsoft.EntityFrameworkCore.Scaffolding.IModelCodeGenerator>                 | <span data-ttu-id="c00d9-122">Создает код для модели.</span><span class="sxs-lookup"><span data-stu-id="c00d9-122">Generates code for a model.</span></span>
<xref:Microsoft.EntityFrameworkCore.Scaffolding.IProviderConfigurationCodeGenerator> | <span data-ttu-id="c00d9-123">Создает код для onconfiguring.</span><span class="sxs-lookup"><span data-stu-id="c00d9-123">Generates OnConfiguring code.</span></span>
<xref:Microsoft.EntityFrameworkCore.Scaffolding.IReverseEngineerScaffolder>          | <span data-ttu-id="c00d9-124">Основной класс для формирования шаблонов реконструированных моделей.</span><span class="sxs-lookup"><span data-stu-id="c00d9-124">The main class for scaffolding reverse engineered models.</span></span>

## <a name="using-services"></a><span data-ttu-id="c00d9-125">Использование служб</span><span class="sxs-lookup"><span data-stu-id="c00d9-125">Using services</span></span>

<span data-ttu-id="c00d9-126">Эти службы также могут быть полезны при создании собственных средств.</span><span class="sxs-lookup"><span data-stu-id="c00d9-126">These services can also be useful for creating your own tools.</span></span> <span data-ttu-id="c00d9-127">Например, если необходимо автоматизировать часть рабочего процесса времени разработки.</span><span class="sxs-lookup"><span data-stu-id="c00d9-127">For example, when you want to automate part of you design-time workflow.</span></span>

<span data-ttu-id="c00d9-128">Поставщик услуг, содержащий эти службы, можно создать с помощью методов расширения Аддентитифрамеворкдесигнтимесервицес и Адддбконтекстдесигнтимесервицес.</span><span class="sxs-lookup"><span data-stu-id="c00d9-128">You can build a service provider containing these services using the AddEntityFrameworkDesignTimeServices and AddDbContextDesignTimeServices extension methods.</span></span>

[!code-csharp[](../../../samples/core/Miscellaneous/CommandLine/CustomTools.cs#CustomTools)]
