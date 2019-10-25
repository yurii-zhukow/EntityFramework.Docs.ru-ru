---
title: Использование отдельного проекта миграции — EF Core
author: bricelam
ms.author: bricelam
ms.date: 10/30/2017
uid: core/managing-schemas/migrations/projects
ms.openlocfilehash: 0082b0af2905fe9e5c3c6509516f622c9d4f8370
ms.sourcegitcommit: 2355447d89496a8ca6bcbfc0a68a14a0bf7f0327
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/23/2019
ms.locfileid: "72812032"
---
# <a name="using-a-separate-migrations-project"></a><span data-ttu-id="14883-102">Использование отдельного проекта миграции</span><span class="sxs-lookup"><span data-stu-id="14883-102">Using a Separate Migrations Project</span></span>

<span data-ttu-id="14883-103">Возможно, вы захотите сохранить миграцию в сборке, отличной от той, которая содержит ваш `DbContext`.</span><span class="sxs-lookup"><span data-stu-id="14883-103">You may want to store your migrations in a different assembly than the one containing your `DbContext`.</span></span> <span data-ttu-id="14883-104">Эту стратегию также можно использовать для поддержки нескольких наборов миграций, например одной для разработки и другой для обновлений выпуска в выпуске.</span><span class="sxs-lookup"><span data-stu-id="14883-104">You can also use this strategy to maintain multiple sets of migrations, for example, one for development and another for release-to-release upgrades.</span></span>

<span data-ttu-id="14883-105">Действие</span><span class="sxs-lookup"><span data-stu-id="14883-105">To do this...</span></span>

1. <span data-ttu-id="14883-106">Создайте новую библиотеку классов.</span><span class="sxs-lookup"><span data-stu-id="14883-106">Create a new class library.</span></span>

2. <span data-ttu-id="14883-107">Добавьте ссылку на сборку DbContext.</span><span class="sxs-lookup"><span data-stu-id="14883-107">Add a reference to your DbContext assembly.</span></span>

3. <span data-ttu-id="14883-108">Переместите миграцию и файлы моментальных снимков модели в библиотеку классов.</span><span class="sxs-lookup"><span data-stu-id="14883-108">Move the migrations and model snapshot files to the class library.</span></span>
   > [!TIP]
   > <span data-ttu-id="14883-109">Если нет существующих миграций, создайте ее в проекте, содержащем DbContext, а затем переместите его.</span><span class="sxs-lookup"><span data-stu-id="14883-109">If you have no existing migrations, generate one in the project containing the DbContext then move it.</span></span>
   > <span data-ttu-id="14883-110">Это важно, так как если сборка миграции не содержит существующую миграцию, команда Add-Migration не сможет найти DbContext.</span><span class="sxs-lookup"><span data-stu-id="14883-110">This is important because if the migrations assembly does not contain an existing migration, the Add-Migration command will be unable to find the DbContext.</span></span>

4. <span data-ttu-id="14883-111">Настройте сборку миграций:</span><span class="sxs-lookup"><span data-stu-id="14883-111">Configure the migrations assembly:</span></span>

   ``` csharp
   options.UseSqlServer(
       connectionString,
       x => x.MigrationsAssembly("MyApp.Migrations"));
   ```

5. <span data-ttu-id="14883-112">Добавьте ссылку на сборку миграции из стартовой сборки.</span><span class="sxs-lookup"><span data-stu-id="14883-112">Add a reference to your migrations assembly from the startup assembly.</span></span>
   * <span data-ttu-id="14883-113">Если это вызывает циклическую зависимость, обновите выходной путь библиотеки классов:</span><span class="sxs-lookup"><span data-stu-id="14883-113">If this causes a circular dependency, update the output path of the class library:</span></span>

     ``` xml
     <PropertyGroup>
       <OutputPath>..\MyStartupProject\bin\$(Configuration)\</OutputPath>
     </PropertyGroup>
     ```

<span data-ttu-id="14883-114">Если вы все сделали правильно, вы сможете добавить новые миграции в проект.</span><span class="sxs-lookup"><span data-stu-id="14883-114">If you did everything correctly, you should be able to add new migrations to the project.</span></span>

``` powershell
Add-Migration NewMigration -Project MyApp.Migrations
```

``` Console
dotnet ef migrations add NewMigration --project MyApp.Migrations
```
