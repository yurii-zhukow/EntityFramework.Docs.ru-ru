---
title: Использование отдельного проекта миграции — EF Core
description: Использование отдельного проекта миграции для управления схемами базы данных с помощью Entity Framework Core
author: bricelam
ms.date: 10/30/2017
uid: core/managing-schemas/migrations/projects
ms.openlocfilehash: 6f28027c320f0d1c13d13bef7d8227b2bb68df91
ms.sourcegitcommit: 0a25c03fa65ae6e0e0e3f66bac48d59eceb96a5a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/14/2020
ms.locfileid: "92062494"
---
# <a name="using-a-separate-migrations-project"></a><span data-ttu-id="95184-103">Использование отдельного проекта миграции</span><span class="sxs-lookup"><span data-stu-id="95184-103">Using a Separate Migrations Project</span></span>

<span data-ttu-id="95184-104">Вам может потребоваться сохранить миграцию в сборке, отличной от той, в которой находится `DbContext` .</span><span class="sxs-lookup"><span data-stu-id="95184-104">You may want to store your migrations in a different assembly than the one containing your `DbContext`.</span></span> <span data-ttu-id="95184-105">Эту стратегию также можно использовать для поддержки нескольких наборов миграций, например одной для разработки и другой для обновлений выпуска в выпуске.</span><span class="sxs-lookup"><span data-stu-id="95184-105">You can also use this strategy to maintain multiple sets of migrations, for example, one for development and another for release-to-release upgrades.</span></span>

<span data-ttu-id="95184-106">Действие</span><span class="sxs-lookup"><span data-stu-id="95184-106">To do this...</span></span>

1. <span data-ttu-id="95184-107">Создайте новую библиотеку классов.</span><span class="sxs-lookup"><span data-stu-id="95184-107">Create a new class library.</span></span>

2. <span data-ttu-id="95184-108">Добавьте ссылку на сборку DbContext.</span><span class="sxs-lookup"><span data-stu-id="95184-108">Add a reference to your DbContext assembly.</span></span>

3. <span data-ttu-id="95184-109">Переместите миграцию и файлы моментальных снимков модели в библиотеку классов.</span><span class="sxs-lookup"><span data-stu-id="95184-109">Move the migrations and model snapshot files to the class library.</span></span>
   > [!TIP]
   > <span data-ttu-id="95184-110">Если нет существующих миграций, создайте ее в проекте, содержащем DbContext, а затем переместите его.</span><span class="sxs-lookup"><span data-stu-id="95184-110">If you have no existing migrations, generate one in the project containing the DbContext then move it.</span></span>
   > <span data-ttu-id="95184-111">Это важно, так как если сборка миграции не содержит существующую миграцию, команда Add-Migration не сможет найти DbContext.</span><span class="sxs-lookup"><span data-stu-id="95184-111">This is important because if the migrations assembly does not contain an existing migration, the Add-Migration command will be unable to find the DbContext.</span></span>

4. <span data-ttu-id="95184-112">Настройте сборку миграций:</span><span class="sxs-lookup"><span data-stu-id="95184-112">Configure the migrations assembly:</span></span>

   ```csharp
   options.UseSqlServer(
       connectionString,
       x => x.MigrationsAssembly("MyApp.Migrations"));
   ```

5. <span data-ttu-id="95184-113">Добавьте ссылку на сборку миграции из стартовой сборки.</span><span class="sxs-lookup"><span data-stu-id="95184-113">Add a reference to your migrations assembly from the startup assembly.</span></span>
   * <span data-ttu-id="95184-114">Если это вызывает циклическую зависимость, обновите выходной путь библиотеки классов:</span><span class="sxs-lookup"><span data-stu-id="95184-114">If this causes a circular dependency, update the output path of the class library:</span></span>

     ```xml
     <PropertyGroup>
       <OutputPath>..\MyStartupProject\bin\$(Configuration)\</OutputPath>
     </PropertyGroup>
     ```

<span data-ttu-id="95184-115">Если вы все сделали правильно, вы сможете добавить новые миграции в проект.</span><span class="sxs-lookup"><span data-stu-id="95184-115">If you did everything correctly, you should be able to add new migrations to the project.</span></span>

## <a name="net-core-cli"></a>[<span data-ttu-id="95184-116">Интерфейс командной строки .NET Core</span><span class="sxs-lookup"><span data-stu-id="95184-116">.NET Core CLI</span></span>](#tab/dotnet-core-cli)

```dotnetcli
dotnet ef migrations add NewMigration --project MyApp.Migrations
```

## <a name="visual-studio"></a>[<span data-ttu-id="95184-117">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="95184-117">Visual Studio</span></span>](#tab/vs)

```powershell
Add-Migration NewMigration -Project MyApp.Migrations
```

***
