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
<a name="using-a-separate-project"></a>С помощью отдельного проекта
========================
Вы можете хранить миграции в сборку, содержащую один вашей `DbContext`. Также можно использовать эту стратегию, чтобы поддерживать несколько наборов миграций и, например, один для разработки, а другой для обновления выпуска к выпуску.

Действие

1. Создайте новую библиотеку классов.

2. Добавьте ссылку на сборку DbContext.

3. Переместите миграции и файлы моментальных снимков модели библиотеки классов.
   * Если вы еще не добавлен, добавьте в проект DbContext, а затем переместить его.

4. Настройка сборки миграции:

   ``` csharp
   options.UseSqlServer(
       connectionString,
       x => x.MigrationsAssembly("MyApp.Migrations"));
   ```

5. Добавьте ссылку на сборку миграции из сборки при запуске.
   * Если это приводит циклическую зависимость, обновите выходной путь библиотеки классов:

     ``` xml
     <PropertyGroup>
       <OutputPath>..\MyStarupProject\bin\$(Configuration)\</OutputPath>
     </PropertyGroup>
     ```

Если вы выполнили все правильно, необходимо добавить в проект новый миграции.

``` powershell
Add-Migration NewMigration -Project MyApp.Migrations
```
``` Console
dotnet ef migraitons add NewMigration --project MyApp.Migrations
```
