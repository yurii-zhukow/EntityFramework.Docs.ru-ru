---
title: Миграция с несколькими проектами - EF Core
author: bricelam
ms.author: bricelam
ms.date: 10/30/2017
ms.technology: entity-framework-core
ms.openlocfilehash: 3684e86cce0005056380d89604d038c734054d14
ms.sourcegitcommit: ced2637bf8cc5964c6daa6c7fcfce501bf9ef6e8
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/22/2017
ms.locfileid: "27161231"
---
<a name="using-a-separate-project"></a><span data-ttu-id="fa6fb-102">С помощью отдельного проекта</span><span class="sxs-lookup"><span data-stu-id="fa6fb-102">Using a Separate Project</span></span>
========================
<span data-ttu-id="fa6fb-103">Вы можете хранить миграции в сборку, содержащую один вашей `DbContext`.</span><span class="sxs-lookup"><span data-stu-id="fa6fb-103">You may want to store your migrations in a different assembly than the one containing your `DbContext`.</span></span> <span data-ttu-id="fa6fb-104">Также можно использовать эту стратегию, чтобы поддерживать несколько наборов миграций и, например, один для разработки, а другой для обновления выпуска к выпуску.</span><span class="sxs-lookup"><span data-stu-id="fa6fb-104">You can also use this strategy to maintain multiple sets of migrations, for example, one for development and another for release-to-release upgrades.</span></span>

<span data-ttu-id="fa6fb-105">Действие</span><span class="sxs-lookup"><span data-stu-id="fa6fb-105">To do this...</span></span>

1. <span data-ttu-id="fa6fb-106">Создайте новую библиотеку классов.</span><span class="sxs-lookup"><span data-stu-id="fa6fb-106">Create a new class library.</span></span>

2. <span data-ttu-id="fa6fb-107">Добавьте ссылку на сборку DbContext.</span><span class="sxs-lookup"><span data-stu-id="fa6fb-107">Add a reference to your DbContext assembly.</span></span>

3. <span data-ttu-id="fa6fb-108">Переместите миграции и файлы моментальных снимков модели библиотеки классов.</span><span class="sxs-lookup"><span data-stu-id="fa6fb-108">Move the migrations and model snapshot files to the class library.</span></span>
   * <span data-ttu-id="fa6fb-109">Если вы еще не добавлен, добавьте в проект DbContext, а затем переместить его.</span><span class="sxs-lookup"><span data-stu-id="fa6fb-109">If you haven't added any, add one to the DbContext project then move it.</span></span>

4. <span data-ttu-id="fa6fb-110">Настройка сборки миграции:</span><span class="sxs-lookup"><span data-stu-id="fa6fb-110">Configure the migrations assembly:</span></span>

   ``` csharp
   options.UseSqlServer(
       connectionString,
       x => x.MigrationsAssembly("MyApp.Migrations"));
   ```

5. <span data-ttu-id="fa6fb-111">Добавьте ссылку на сборку миграции из сборки при запуске.</span><span class="sxs-lookup"><span data-stu-id="fa6fb-111">Add a reference to your migrations assembly from the startup assembly.</span></span>
   * <span data-ttu-id="fa6fb-112">Если это приводит циклическую зависимость, обновите выходной путь библиотеки классов:</span><span class="sxs-lookup"><span data-stu-id="fa6fb-112">If this causes a circular dependency, update the output path of the class library:</span></span>

     ``` xml
     <PropertyGroup>
       <OutputPath>..\MyStarupProject\bin\$(Configuration)\</OutputPath>
     </PropertyGroup>
     ```

<span data-ttu-id="fa6fb-113">Если вы выполнили все правильно, необходимо добавить в проект новый миграции.</span><span class="sxs-lookup"><span data-stu-id="fa6fb-113">If you did everything correctly, you should be able to add new migrations to the project.</span></span>

``` powershell
Add-Migration NewMigration -Project MyApp.Migrations
```
``` Console
dotnet ef migrations add NewMigration --project MyApp.Migrations
```
