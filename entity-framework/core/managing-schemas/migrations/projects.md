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
# <a name="using-a-separate-migrations-project"></a>Использование отдельного проекта миграции

Вам может потребоваться сохранить миграцию в сборке, отличной от той, в которой находится `DbContext` . Эту стратегию также можно использовать для поддержки нескольких наборов миграций, например одной для разработки и другой для обновлений выпуска в выпуске.

Действие

1. Создайте новую библиотеку классов.

2. Добавьте ссылку на сборку DbContext.

3. Переместите миграцию и файлы моментальных снимков модели в библиотеку классов.
   > [!TIP]
   > Если нет существующих миграций, создайте ее в проекте, содержащем DbContext, а затем переместите его.
   > Это важно, так как если сборка миграции не содержит существующую миграцию, команда Add-Migration не сможет найти DbContext.

4. Настройте сборку миграций:

   ```csharp
   options.UseSqlServer(
       connectionString,
       x => x.MigrationsAssembly("MyApp.Migrations"));
   ```

5. Добавьте ссылку на сборку миграции из стартовой сборки.
   * Если это вызывает циклическую зависимость, обновите выходной путь библиотеки классов:

     ```xml
     <PropertyGroup>
       <OutputPath>..\MyStartupProject\bin\$(Configuration)\</OutputPath>
     </PropertyGroup>
     ```

Если вы все сделали правильно, вы сможете добавить новые миграции в проект.

## <a name="net-core-cli"></a>[Интерфейс командной строки .NET Core](#tab/dotnet-core-cli)

```dotnetcli
dotnet ef migrations add NewMigration --project MyApp.Migrations
```

## <a name="visual-studio"></a>[Visual Studio](#tab/vs)

```powershell
Add-Migration NewMigration -Project MyApp.Migrations
```

***
