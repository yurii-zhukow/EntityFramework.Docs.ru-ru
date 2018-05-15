---
title: Начало работы в .NET Core — новая база данных — Core EF
author: rick-anderson
ms.author: riande
ms.author2: tdykstra
description: Начало работы с .NET Core и Entity Framework Core
keywords: .NET Core, Entity Framework Core, Visual Studio Code, VS Code, Mac, Linux
ms.date: 04/05/2017
ms.assetid: 099d179e-dd7b-4755-8f3c-fcde914bf50b
ms.technology: entity-framework-core
uid: core/get-started/netcore/new-db-sqlite
ms.openlocfilehash: fcace3c0f259b1a456d9ca1086e6a1549c070d57
ms.sourcegitcommit: 507a40ed050fee957bcf8cf05f6e0ec8a3b1a363
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/26/2018
---
# <a name="getting-started-with-ef-core-on-net-core-console-app-with-a-new-database"></a>Начало работы с EF Core в консольном приложении .NET Core с новой базой данных

В этом пошаговом руководстве вы создадите консольное приложение .NET Core, которое осуществляет базовые операции доступа к базе данных SQLite с помощью Entity Framework Core. Вы примените процесс миграций, чтобы создать базу данных из модели. В разделе [ASP.NET Core - новая база данных](xref:core/get-started/aspnetcore/new-db) описана версия для Visual Studio с использованием ASP.NET Core MVC.

> [!TIP]  
> Для этой статьи вы можете скачать [пример](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/NetCore/ConsoleApp.SQLite) из репозитория GitHub.

## <a name="prerequisites"></a>Предварительные требования

Для прохождения этого пошагового руководства нужны следующие элементы:
* операционная система, которая поддерживает .NET Core;
* [пакет SDK для .NET Core](https://www.microsoft.com/net/core) 2.0 (но эти же инструкции с очень небольшими изменениями позволят создать приложение и в предыдущей версии).

## <a name="create-a-new-project"></a>Создание нового проекта

* Создайте для проекта новую папку `ConsoleApp.SQLite` и с помощью команды `dotnet` заполните ее приложением .NET Core.

``` Console
mkdir ConsoleApp.SQLite
cd ConsoleApp.SQLite/
dotnet new console
```

## <a name="install-entity-framework-core"></a>Установка Entity Framework Core

Чтобы использовать EF Core, установите пакеты для поставщиков базы данных, с которыми вы будете работать. В этом пошаговом руководстве используется SQLite. Список доступных поставщиков вы найдете в разделе [Database Providers](../../providers/index.md) (Поставщики базы данных).

* Установите Microsoft.EntityFrameworkCore.Sqlite и Microsoft.EntityFrameworkCore.Design:

``` Console
dotnet add package Microsoft.EntityFrameworkCore.Sqlite
dotnet add package Microsoft.EntityFrameworkCore.Design
```

* Вручную измените `ConsoleApp.SQLite.csproj`, добавив значение DotNetCliToolReference в Microsoft.EntityFrameworkCore.Tools.DotNet:

  ``` xml
  <ItemGroup>
    <DotNetCliToolReference Include="Microsoft.EntityFrameworkCore.Tools.DotNet" Version="2.0.0" />
  </ItemGroup>
  ```

Теперь `ConsoleApp.SQLite.csproj` должен содержать следующее:

[!code[Main](../../../../samples/core/GetStarted/NetCore/ConsoleApp.SQLite/ConsoleApp.SQLite.csproj)]

 Примечание. Приведенные выше номера версий были правильными на момент публикации.

*  Выполните `dotnet restore`, чтобы установить новые пакеты.

## <a name="create-the-model"></a>Создание модели

Определите контекст и классы сущности, которые должны входить в модель:

* Создайте новый файл *Model.cs* со следующим содержимым:

[!code-csharp[Main](../../../../samples/core/GetStarted/NetCore/ConsoleApp.SQLite/Model.cs)]

Подсказка. В реальном приложении каждый класс помещается в отдельный файл, а строка подключения — в файл конфигурации. Для упрощения в этом руководстве мы заносим все данные в один файл.

## <a name="create-the-database"></a>Создание базы данных

Когда модель будет готова, из нее можно создать базу данных с помощью [миграций](https://docs.microsoft.com/aspnet/core/data/ef-mvc/migrations#introduction-to-migrations):

* Запустите `dotnet ef migrations add InitialCreate`, чтобы сформировать шаблон миграции и создать начальный набор таблиц для модели.
* Запустите `dotnet ef database update`, чтобы применить созданную миграцию к базе данных. Эта команда создает базу данных и применяет к ней миграции.

> [!NOTE]  
> Если для SQLite применяются относительные пути, они учитываются относительно главной сборки приложения. В этом примере основным двоичным файлом является `bin/Debug/netcoreapp2.0/ConsoleApp.SQLite.dll`, а значит база данных SQLite расположена в папке `bin/Debug/netcoreapp2.0/blogging.db`.

## <a name="use-your-model"></a>Использование модели

* Откройте файл *Program.cs* и замените его содержимое следующим кодом:

  [!code-csharp[Main](../../../../samples/core/GetStarted/NetCore/ConsoleApp.SQLite/Program.cs)]

* Проверьте работу приложения:

  `dotnet run`

  В базе данных будет создан один блог, а затем в консоли появятся сведения обо всех блогах.

  ``` Console
  ConsoleApp.SQLite>dotnet run
  1 records saved to database

  All blogs in database:
   - http://blogs.msdn.com/adonet
  ```

### <a name="changing-the-model"></a>Изменение модели

- Если вы внесете изменения в эту модель, с помощью команды `dotnet ef migrations add` вы сможете сформировать новую [миграцию](https://docs.microsoft.com/aspnet/core/data/ef-mvc/migrations#introduction-to-migrations), которая вносит необходимые изменения в схему базы данных. Проверьте сформированный код шаблона (и внесите правки, если потребуется), а затем выполните команду `dotnet ef database update`, чтобы применить изменения к базе данных.
- EF использует таблицу `__EFMigrationsHistory` в базе данных, чтобы отслеживать уже примененные к базе данных миграции.
- SQLite поддерживает не все миграции (изменения схемы) из-за собственных ограничений SQLite. Подробнее о них можно узнать [в этой статье](../../providers/sqlite/limitations.md). Если вы разрабатываете новое приложение, лучше не использовать миграции для изменения модели, а просто удалить старую базу данных и создать новую.

## <a name="additional-resources"></a>Дополнительные ресурсы

* [Руководство по применению кроссплатформенной консоли EF с .NET Core и новой базой данных SQLite](xref:core/get-started/netcore/new-db-sqlite)
* [Введение в ASP.NET Core MVC для Mac и Linux](https://docs.microsoft.com/aspnet/core/tutorials/first-mvc-app-xplat/index)
* [Введение в ASP.NET Core MVC для Visual Studio](https://docs.microsoft.com/aspnet/core/tutorials/first-mvc-app/index)
* [Начало работы с ASP.NET Core и Entity Framework Core с использованием Visual Studio](https://docs.microsoft.com/aspnet/core/data/ef-mvc/index)
