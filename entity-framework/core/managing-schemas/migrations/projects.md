---
title: Использование отдельного проекта миграции — EF Core
description: Использование отдельного проекта миграции для управления схемами базы данных с помощью Entity Framework Core
author: bricelam
ms.date: 11/06/2020
uid: core/managing-schemas/migrations/projects
ms.openlocfilehash: 9a6b8977f9d7bcdae0fb9aea6966a7eb43e9e7db
ms.sourcegitcommit: 4798ab8d04c1fdbe6dd204d94d770fcbf309d09b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/11/2021
ms.locfileid: "103024151"
---
# <a name="using-a-separate-migrations-project"></a><span data-ttu-id="0aa65-103">Использование отдельного проекта миграции</span><span class="sxs-lookup"><span data-stu-id="0aa65-103">Using a Separate Migrations Project</span></span>

<span data-ttu-id="0aa65-104">Возможно, вы захотите сохранить миграцию в проекте, отличном от того, который содержит ваш `DbContext` .</span><span class="sxs-lookup"><span data-stu-id="0aa65-104">You may want to store your migrations in a different project than the one containing your `DbContext`.</span></span> <span data-ttu-id="0aa65-105">Эту стратегию также можно использовать для поддержки нескольких наборов миграций, например одной для разработки и другой для обновлений выпуска в выпуске.</span><span class="sxs-lookup"><span data-stu-id="0aa65-105">You can also use this strategy to maintain multiple sets of migrations, for example, one for development and another for release-to-release upgrades.</span></span>

> [!TIP]
> <span data-ttu-id="0aa65-106">Вы можете скачать используемый в этой статье [пример](https://github.com/dotnet/EntityFramework.Docs/tree/main/samples/core/Schemas/ThreeProjectMigrations) из репозитория GitHub.</span><span class="sxs-lookup"><span data-stu-id="0aa65-106">You can view this article's [sample on GitHub](https://github.com/dotnet/EntityFramework.Docs/tree/main/samples/core/Schemas/ThreeProjectMigrations).</span></span>

## <a name="steps"></a><span data-ttu-id="0aa65-107">Шаги</span><span class="sxs-lookup"><span data-stu-id="0aa65-107">Steps</span></span>

1. <span data-ttu-id="0aa65-108">Создайте новую библиотеку классов.</span><span class="sxs-lookup"><span data-stu-id="0aa65-108">Create a new class library.</span></span>

2. <span data-ttu-id="0aa65-109">Добавьте ссылку на проект DbContext.</span><span class="sxs-lookup"><span data-stu-id="0aa65-109">Add a reference to your DbContext project.</span></span>

3. <span data-ttu-id="0aa65-110">Переместите миграцию и файлы моментальных снимков модели в библиотеку классов.</span><span class="sxs-lookup"><span data-stu-id="0aa65-110">Move the migrations and model snapshot files to the class library.</span></span>
   > [!TIP]
   > <span data-ttu-id="0aa65-111">Если нет существующих миграций, создайте ее в проекте, содержащем DbContext, а затем переместите его.</span><span class="sxs-lookup"><span data-stu-id="0aa65-111">If you have no existing migrations, generate one in the project containing the DbContext then move it.</span></span>
   > <span data-ttu-id="0aa65-112">Это важно, поскольку если проект миграции не содержит существующую миграцию, команда Add-Migration не сможет найти DbContext.</span><span class="sxs-lookup"><span data-stu-id="0aa65-112">This is important because if the migrations project does not contain an existing migration, the Add-Migration command will be unable to find the DbContext.</span></span>

4. <span data-ttu-id="0aa65-113">Настройте сборку миграций:</span><span class="sxs-lookup"><span data-stu-id="0aa65-113">Configure the migrations assembly:</span></span>

   [!code-csharp[](../../../../samples/core/Schemas/ThreeProjectMigrations/WebApplication1/Startup.cs#snippet_MigrationsAssembly)]

5. <span data-ttu-id="0aa65-114">Добавьте ссылку на проект миграций из **запускаемого** проекта.</span><span class="sxs-lookup"><span data-stu-id="0aa65-114">Add a reference to your migrations project from the **startup** project.</span></span>

   ```xml
   <ItemGroup>
     <ProjectReference Include="..\WebApplication1.Migrations\WebApplication1.Migrations.csproj">
   </ItemGroup>
   ```

   <span data-ttu-id="0aa65-115">Если это вызывает циклическую зависимость, то вместо этого можно обновить базовый выходной путь проекта **миграции** :</span><span class="sxs-lookup"><span data-stu-id="0aa65-115">If this causes a circular dependency, you can update the base output path of the **migrations** project instead:</span></span>

   ```xml
   <PropertyGroup>
     <BaseOutputPath>..\WebApplication1\bin\</BaseOutputPath>
   </PropertyGroup>
   ```

<span data-ttu-id="0aa65-116">Если вы все сделали правильно, вы сможете добавить новые миграции в проект.</span><span class="sxs-lookup"><span data-stu-id="0aa65-116">If you did everything correctly, you should be able to add new migrations to the project.</span></span>

## <a name="net-core-cli"></a>[<span data-ttu-id="0aa65-117">Интерфейс командной строки .NET Core</span><span class="sxs-lookup"><span data-stu-id="0aa65-117">.NET Core CLI</span></span>](#tab/dotnet-core-cli)

```dotnetcli
dotnet ef migrations add NewMigration --project WebApplication1.Migrations
```

## <a name="visual-studio"></a>[<span data-ttu-id="0aa65-118">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="0aa65-118">Visual Studio</span></span>](#tab/vs)

```powershell
Add-Migration NewMigration -Project WebApplication1.Migrations
```

***
