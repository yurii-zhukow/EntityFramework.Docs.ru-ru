---
title: Миграция с несколькими проектами — EF Core
author: bricelam
ms.author: bricelam
ms.date: 10/30/2017
ms.openlocfilehash: 76e88dd486b1c53dc69a24e35710511bf9cb673b
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/27/2018
ms.locfileid: "42997991"
---
<a name="using-a-separate-project"></a>Использование отдельного проекта
========================
Вам может потребоваться сохранить миграции в сборку, содержащую один вашей `DbContext`. Также можно использовать эту стратегию, чтобы поддерживать несколько миграций, например, один для разработки, а другой для обновления до выпуска к выпуску.

Действие

1. Создайте новую библиотеку классов.

2. Добавьте ссылку на сборку DbContext.

3. Переместите миграции и файлы моментальных снимков модели к библиотеке классов.
   > [!TIP]
   > Если у вас нет имеющихся миграций, создать его в проект, содержащий DbContext, а затем переместите ее. Это важно, так как миграция не содержит существующие миграции, команда Add-Migration будет не удается найти DbContext.

4. Настройка сборки миграции:

   ``` csharp
   options.UseSqlServer(
       connectionString,
       x => x.MigrationsAssembly("MyApp.Migrations"));
   ```

5. Добавьте ссылку на сборку миграции из запуска сборки.
   * Если это приводит к циклической зависимости, обновите путь к выходной библиотеке классов:

     ``` xml
     <PropertyGroup>
       <OutputPath>..\MyStartupProject\bin\$(Configuration)\</OutputPath>
     </PropertyGroup>
     ```

Если все делается правильно, можно добавить в проект новые миграции.

``` powershell
Add-Migration NewMigration -Project MyApp.Migrations
```
``` Console
dotnet ef migrations add NewMigration --project MyApp.Migrations
```
