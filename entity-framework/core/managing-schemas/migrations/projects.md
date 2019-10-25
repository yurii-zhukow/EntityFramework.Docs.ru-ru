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
# <a name="using-a-separate-migrations-project"></a>Использование отдельного проекта миграции

Возможно, вы захотите сохранить миграцию в сборке, отличной от той, которая содержит ваш `DbContext`. Эту стратегию также можно использовать для поддержки нескольких наборов миграций, например одной для разработки и другой для обновлений выпуска в выпуске.

Действие

1. Создайте новую библиотеку классов.

2. Добавьте ссылку на сборку DbContext.

3. Переместите миграцию и файлы моментальных снимков модели в библиотеку классов.
   > [!TIP]
   > Если нет существующих миграций, создайте ее в проекте, содержащем DbContext, а затем переместите его.
   > Это важно, так как если сборка миграции не содержит существующую миграцию, команда Add-Migration не сможет найти DbContext.

4. Настройте сборку миграций:

   ``` csharp
   options.UseSqlServer(
       connectionString,
       x => x.MigrationsAssembly("MyApp.Migrations"));
   ```

5. Добавьте ссылку на сборку миграции из стартовой сборки.
   * Если это вызывает циклическую зависимость, обновите выходной путь библиотеки классов:

     ``` xml
     <PropertyGroup>
       <OutputPath>..\MyStartupProject\bin\$(Configuration)\</OutputPath>
     </PropertyGroup>
     ```

Если вы все сделали правильно, вы сможете добавить новые миграции в проект.

``` powershell
Add-Migration NewMigration -Project MyApp.Migrations
```

``` Console
dotnet ef migrations add NewMigration --project MyApp.Migrations
```
