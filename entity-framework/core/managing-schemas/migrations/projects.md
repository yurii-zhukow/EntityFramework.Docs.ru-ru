---
title: Использование отдельного проекта миграции — EF Core
description: Использование отдельного проекта миграции для управления схемами базы данных с помощью Entity Framework Core
author: bricelam
ms.date: 11/06/2020
uid: core/managing-schemas/migrations/projects
ms.openlocfilehash: 2d1c093e0bb307584e2bf19cb93deec98aa10692
ms.sourcegitcommit: f3512e3a98e685a3ba409c1d0157ce85cc390cf4
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2020
ms.locfileid: "94429810"
---
# <a name="using-a-separate-migrations-project"></a><span data-ttu-id="06649-103">Использование отдельного проекта миграции</span><span class="sxs-lookup"><span data-stu-id="06649-103">Using a Separate Migrations Project</span></span>

<span data-ttu-id="06649-104">Возможно, вы захотите сохранить миграцию в проекте, отличном от того, который содержит ваш `DbContext` .</span><span class="sxs-lookup"><span data-stu-id="06649-104">You may want to store your migrations in a different project than the one containing your `DbContext`.</span></span> <span data-ttu-id="06649-105">Эту стратегию также можно использовать для поддержки нескольких наборов миграций, например одной для разработки и другой для обновлений выпуска в выпуске.</span><span class="sxs-lookup"><span data-stu-id="06649-105">You can also use this strategy to maintain multiple sets of migrations, for example, one for development and another for release-to-release upgrades.</span></span>

> [!TIP]
> <span data-ttu-id="06649-106">Вы можете скачать используемый в этой статье [пример](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Schemas/ThreeProjectMigrations) из репозитория GitHub.</span><span class="sxs-lookup"><span data-stu-id="06649-106">You can view this article's [sample on GitHub](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Schemas/ThreeProjectMigrations).</span></span>

## <a name="steps"></a><span data-ttu-id="06649-107">Шаги</span><span class="sxs-lookup"><span data-stu-id="06649-107">Steps</span></span>

1. <span data-ttu-id="06649-108">Создайте новую библиотеку классов.</span><span class="sxs-lookup"><span data-stu-id="06649-108">Create a new class library.</span></span>

2. <span data-ttu-id="06649-109">Добавьте ссылку на проект DbContext.</span><span class="sxs-lookup"><span data-stu-id="06649-109">Add a reference to your DbContext project.</span></span>

3. <span data-ttu-id="06649-110">Переместите миграцию и файлы моментальных снимков модели в библиотеку классов.</span><span class="sxs-lookup"><span data-stu-id="06649-110">Move the migrations and model snapshot files to the class library.</span></span>
   > [!TIP]
   > <span data-ttu-id="06649-111">Если нет существующих миграций, создайте ее в проекте, содержащем DbContext, а затем переместите его.</span><span class="sxs-lookup"><span data-stu-id="06649-111">If you have no existing migrations, generate one in the project containing the DbContext then move it.</span></span>
   > <span data-ttu-id="06649-112">Это важно, поскольку если проект миграции не содержит существующую миграцию, команда Add-Migration не сможет найти DbContext.</span><span class="sxs-lookup"><span data-stu-id="06649-112">This is important because if the migrations project does not contain an existing migration, the Add-Migration command will be unable to find the DbContext.</span></span>

4. <span data-ttu-id="06649-113">Настройте сборку миграций:</span><span class="sxs-lookup"><span data-stu-id="06649-113">Configure the migrations assembly:</span></span>

   [!code-csharp[](../../../../samples/core/Schemas/ThreeProjectMigrations/WebApplication1/Startup.cs#snippet_MigrationsAssembly)]

5. <span data-ttu-id="06649-114">Добавьте ссылку на проект миграций из **запускаемого** проекта.</span><span class="sxs-lookup"><span data-stu-id="06649-114">Add a reference to your migrations project from the **startup** project.</span></span>

   ```xml
   <ItemGroup>
     <ProjectReference Include="..\WebApplication1.Migrations\WebApplication1.Migrations.csproj">
   </ItemGroup>
   ```

   <span data-ttu-id="06649-115">Если это вызывает циклическую зависимость, то вместо этого можно обновить базовый выходной путь проекта **миграции** :</span><span class="sxs-lookup"><span data-stu-id="06649-115">If this causes a circular dependency, you can update the base output path of the **migrations** project instead:</span></span>

   ```xml
   <PropertyGroup>
     <BaseOutputPath>..\WebApplication1\bin\</BaseOutputPath>
   </PropertyGroup>
   ```

<span data-ttu-id="06649-116">Если вы все сделали правильно, вы сможете добавить новые миграции в проект.</span><span class="sxs-lookup"><span data-stu-id="06649-116">If you did everything correctly, you should be able to add new migrations to the project.</span></span>

## <a name="net-core-cli"></a>[<span data-ttu-id="06649-117">Интерфейс командной строки .NET Core</span><span class="sxs-lookup"><span data-stu-id="06649-117">.NET Core CLI</span></span>](#tab/dotnet-core-cli)

```dotnetcli
dotnet ef migrations add NewMigration --project WebApplication1.Migrations
```

## <a name="visual-studio"></a>[<span data-ttu-id="06649-118">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="06649-118">Visual Studio</span></span>](#tab/vs)

```powershell
Add-Migration NewMigration -Project WebApplication1.Migrations
```

***
