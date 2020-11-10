---
title: Использование отдельного проекта миграции — EF Core
description: Использование отдельного проекта миграции для управления схемами базы данных с помощью Entity Framework Core
author: bricelam
ms.date: 11/06/2020
uid: core/managing-schemas/migrations/projects
ms.openlocfilehash: 2d1c093e0bb307584e2bf19cb93deec98aa10692
ms.sourcegitcommit: f3512e3a98e685a3ba409c1d0157ce85cc390cf4
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2020
ms.locfileid: "94429810"
---
# <a name="using-a-separate-migrations-project"></a>Использование отдельного проекта миграции

Возможно, вы захотите сохранить миграцию в проекте, отличном от того, который содержит ваш `DbContext` . Эту стратегию также можно использовать для поддержки нескольких наборов миграций, например одной для разработки и другой для обновлений выпуска в выпуске.

> [!TIP]
> Вы можете скачать используемый в этой статье [пример](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Schemas/ThreeProjectMigrations) из репозитория GitHub.

## <a name="steps"></a>Шаги

1. Создайте новую библиотеку классов.

2. Добавьте ссылку на проект DbContext.

3. Переместите миграцию и файлы моментальных снимков модели в библиотеку классов.
   > [!TIP]
   > Если нет существующих миграций, создайте ее в проекте, содержащем DbContext, а затем переместите его.
   > Это важно, поскольку если проект миграции не содержит существующую миграцию, команда Add-Migration не сможет найти DbContext.

4. Настройте сборку миграций:

   [!code-csharp[](../../../../samples/core/Schemas/ThreeProjectMigrations/WebApplication1/Startup.cs#snippet_MigrationsAssembly)]

5. Добавьте ссылку на проект миграций из **запускаемого** проекта.

   ```xml
   <ItemGroup>
     <ProjectReference Include="..\WebApplication1.Migrations\WebApplication1.Migrations.csproj">
   </ItemGroup>
   ```

   Если это вызывает циклическую зависимость, то вместо этого можно обновить базовый выходной путь проекта **миграции** :

   ```xml
   <PropertyGroup>
     <BaseOutputPath>..\WebApplication1\bin\</BaseOutputPath>
   </PropertyGroup>
   ```

Если вы все сделали правильно, вы сможете добавить новые миграции в проект.

## <a name="net-core-cli"></a>[Интерфейс командной строки .NET Core](#tab/dotnet-core-cli)

```dotnetcli
dotnet ef migrations add NewMigration --project WebApplication1.Migrations
```

## <a name="visual-studio"></a>[Visual Studio](#tab/vs)

```powershell
Add-Migration NewMigration -Project WebApplication1.Migrations
```

***
