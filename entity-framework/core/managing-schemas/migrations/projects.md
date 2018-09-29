---
title: Миграция с несколькими проектами — EF Core
author: bricelam
ms.author: bricelam
ms.date: 10/30/2017
uid: core/managing-schemas/migrations/projects
ms.openlocfilehash: 30a6afad1488e74ce2585be3d780186311379a97
ms.sourcegitcommit: ad1bdea58ed35d0f19791044efe9f72f94189c18
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/28/2018
ms.locfileid: "47447148"
---
<a name="using-a-separate-project"></a><span data-ttu-id="5db3a-102">Использование отдельного проекта</span><span class="sxs-lookup"><span data-stu-id="5db3a-102">Using a Separate Project</span></span>
========================
<span data-ttu-id="5db3a-103">Вам может потребоваться сохранить миграции в сборку, содержащую один вашей `DbContext`.</span><span class="sxs-lookup"><span data-stu-id="5db3a-103">You may want to store your migrations in a different assembly than the one containing your `DbContext`.</span></span> <span data-ttu-id="5db3a-104">Также можно использовать эту стратегию, чтобы поддерживать несколько миграций, например, один для разработки, а другой для обновления до выпуска к выпуску.</span><span class="sxs-lookup"><span data-stu-id="5db3a-104">You can also use this strategy to maintain multiple sets of migrations, for example, one for development and another for release-to-release upgrades.</span></span>

<span data-ttu-id="5db3a-105">Действие</span><span class="sxs-lookup"><span data-stu-id="5db3a-105">To do this...</span></span>

1. <span data-ttu-id="5db3a-106">Создайте новую библиотеку классов.</span><span class="sxs-lookup"><span data-stu-id="5db3a-106">Create a new class library.</span></span>

2. <span data-ttu-id="5db3a-107">Добавьте ссылку на сборку DbContext.</span><span class="sxs-lookup"><span data-stu-id="5db3a-107">Add a reference to your DbContext assembly.</span></span>

3. <span data-ttu-id="5db3a-108">Переместите миграции и файлы моментальных снимков модели к библиотеке классов.</span><span class="sxs-lookup"><span data-stu-id="5db3a-108">Move the migrations and model snapshot files to the class library.</span></span>
   > [!TIP]
   > <span data-ttu-id="5db3a-109">Если у вас нет имеющихся миграций, создать его в проект, содержащий DbContext, а затем переместите ее.</span><span class="sxs-lookup"><span data-stu-id="5db3a-109">If you have no existing migrations, generate one in the project containing the DbContext then move it.</span></span> <span data-ttu-id="5db3a-110">Это важно, так как миграция не содержит существующие миграции, команда Add-Migration будет не удается найти DbContext.</span><span class="sxs-lookup"><span data-stu-id="5db3a-110">This is important because if the migrations assembly does not contain an existing migration, the Add-Migration command will be unable to find the DbContext.</span></span>

4. <span data-ttu-id="5db3a-111">Настройка сборки миграции:</span><span class="sxs-lookup"><span data-stu-id="5db3a-111">Configure the migrations assembly:</span></span>

   ``` csharp
   options.UseSqlServer(
       connectionString,
       x => x.MigrationsAssembly("MyApp.Migrations"));
   ```

5. <span data-ttu-id="5db3a-112">Добавьте ссылку на сборку миграции из запуска сборки.</span><span class="sxs-lookup"><span data-stu-id="5db3a-112">Add a reference to your migrations assembly from the startup assembly.</span></span>
   * <span data-ttu-id="5db3a-113">Если это приводит к циклической зависимости, обновите путь к выходной библиотеке классов:</span><span class="sxs-lookup"><span data-stu-id="5db3a-113">If this causes a circular dependency, update the output path of the class library:</span></span>

     ``` xml
     <PropertyGroup>
       <OutputPath>..\MyStartupProject\bin\$(Configuration)\</OutputPath>
     </PropertyGroup>
     ```

<span data-ttu-id="5db3a-114">Если все делается правильно, можно добавить в проект новые миграции.</span><span class="sxs-lookup"><span data-stu-id="5db3a-114">If you did everything correctly, you should be able to add new migrations to the project.</span></span>

``` powershell
Add-Migration NewMigration -Project MyApp.Migrations
```
``` Console
dotnet ef migrations add NewMigration --project MyApp.Migrations
```
