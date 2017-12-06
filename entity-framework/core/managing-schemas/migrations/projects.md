---
title: "Миграция с несколькими проектами - EF Core"
author: bricelam
ms.author: bricelam
ms.date: 10/30/2017
ms.technology: entity-framework-core
ms.openlocfilehash: e059deb6c7555a4f6732fe7942855e95b3f9a34c
ms.sourcegitcommit: b467368cc350e6059fdc0949e042a41cb11e61d9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/15/2017
---
<a name="using-a-separate-project"></a><span data-ttu-id="ff609-102">С помощью отдельного проекта</span><span class="sxs-lookup"><span data-stu-id="ff609-102">Using a Separate Project</span></span>
========================
<span data-ttu-id="ff609-103">Вы можете хранить миграции в сборку, содержащую один вашей `DbContext`.</span><span class="sxs-lookup"><span data-stu-id="ff609-103">You may want to store your migrations in a different assembly than the one containing your `DbContext`.</span></span> <span data-ttu-id="ff609-104">Также можно использовать эту стратегию, чтобы поддерживать несколько наборов миграций и, например, один для разработки, а другой для обновления выпуска к выпуску.</span><span class="sxs-lookup"><span data-stu-id="ff609-104">You can also use this strategy to maintain multiple sets of migrations, for example, one for development and another for release-to-release upgrades.</span></span>

<span data-ttu-id="ff609-105">Действие</span><span class="sxs-lookup"><span data-stu-id="ff609-105">To do this...</span></span>

1. <span data-ttu-id="ff609-106">Создайте новую библиотеку классов.</span><span class="sxs-lookup"><span data-stu-id="ff609-106">Create a new class library.</span></span>

2. <span data-ttu-id="ff609-107">Добавьте ссылку на сборку DbContext.</span><span class="sxs-lookup"><span data-stu-id="ff609-107">Add a reference to your DbContext assembly.</span></span>

3. <span data-ttu-id="ff609-108">Переместите миграции и файлы моментальных снимков модели библиотеки классов.</span><span class="sxs-lookup"><span data-stu-id="ff609-108">Move the migrations and model snapshot files to the class library.</span></span>
   * <span data-ttu-id="ff609-109">Если вы еще не добавлен, добавьте в проект DbContext, а затем переместить его.</span><span class="sxs-lookup"><span data-stu-id="ff609-109">If you haven't added any, add one to the DbContext project then move it.</span></span>

4. <span data-ttu-id="ff609-110">Настройка сборки миграции:</span><span class="sxs-lookup"><span data-stu-id="ff609-110">Configure the migrations assembly:</span></span>

   ``` csharp
   options.UseSqlServer(
       connectionString,
       x => x.MigrationsAssembly("MyApp.Migrations"));
   ```

5. <span data-ttu-id="ff609-111">Добавьте ссылку на сборку миграции из сборки при запуске.</span><span class="sxs-lookup"><span data-stu-id="ff609-111">Add a reference to your migrations assembly from the startup assembly.</span></span>
   * <span data-ttu-id="ff609-112">Если это приводит циклическую зависимость, обновите выходной путь библиотеки классов:</span><span class="sxs-lookup"><span data-stu-id="ff609-112">If this causes a circular dependency, update the output path of the class library:</span></span>

     ``` xml
     <PropertyGroup>
       <OutputPath>..\MyStarupProject\bin\$(Configuration)\</OutputPath>
     </PropertyGroup>
     ```

<span data-ttu-id="ff609-113">Если вы выполнили все правильно, необходимо добавить в проект новый миграции.</span><span class="sxs-lookup"><span data-stu-id="ff609-113">If you did everything correctly, you should be able to add new migrations to the project.</span></span>

``` powershell
Add-Migration NewMigration -Project MyApp.Migrations
```
``` Console
dotnet ef migraitons add NewMigration --project MyApp.Migrations
```
