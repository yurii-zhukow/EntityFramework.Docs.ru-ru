---
title: Начало работы в .NET Core — новая база данных — Core EF
author: rick-anderson
ms.author: riande
description: Начало работы с .NET Core и Entity Framework Core
ms.date: 08/03/2018
ms.assetid: 099d179e-dd7b-4755-8f3c-fcde914bf50b
uid: core/get-started/netcore/new-db-sqlite
ms.openlocfilehash: 51f5752eebce5603c663072f7b36dfecd4ddf227
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/27/2018
ms.locfileid: "42993696"
---
# <a name="getting-started-with-ef-core-on-net-core-console-app-with-a-new-database"></a>Начало работы с EF Core в консольном приложении .NET Core с новой базой данных

В этом руководстве вы создадите консольное приложение .NET Core, которое осуществляет доступ к базе данных SQLite с помощью Entity Framework Core. Вы примените процесс миграций, чтобы создать базу данных из модели. В разделе [ASP.NET Core - новая база данных](xref:core/get-started/aspnetcore/new-db) описана версия для Visual Studio с использованием ASP.NET Core MVC.

Пример для этой статьи на GitHub](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/NetCore/ConsoleApp.SQLite).

## <a name="prerequisites"></a>Предварительные требования

* [Пакет SDK для NET Core 2.1](https://www.microsoft.com/net/core)

## <a name="create-a-new-project"></a>Создание нового проекта

* Создание консольного проекта:

  ``` Console
  dotnet new console -o ConsoleApp.SQLite
  cd ConsoleApp.SQLite/
  ```

## <a name="install-entity-framework-core"></a>Установка Entity Framework Core

Чтобы использовать EF Core, установите пакеты для поставщиков базы данных, с которыми вы будете работать. В этом пошаговом руководстве используется SQLite. Список доступных поставщиков вы найдете в разделе [Database Providers](../../providers/index.md) (Поставщики базы данных).

* Установите Microsoft.EntityFrameworkCore.Sqlite и Microsoft.EntityFrameworkCore.Design:

  ```Console
  dotnet add package Microsoft.EntityFrameworkCore.Sqlite
  dotnet add package Microsoft.EntityFrameworkCore.Design
  ```

* Выполните `dotnet restore`, чтобы установить новые пакеты.

## <a name="create-the-model"></a>Создание модели

Определите контекст и классы сущности, которые должны входить в модель:

* Создайте новый файл *Model.cs* со следующим содержимым:

  [!code-csharp[Main](../../../../samples/core/GetStarted/NetCore/ConsoleApp.SQLite/Model.cs)]

Совет. В реальном приложении каждый класс помещается в отдельный файл, а строка подключения — в файл конфигурации или переменную среды. Для упрощения в этом руководстве все данные содержатся в одном файле.

## <a name="create-the-database"></a>Создание базы данных

Когда модель будет готова, создайте базу данных с помощью [миграций](https://docs.microsoft.com/aspnet/core/data/ef-mvc/migrations#introduction-to-migrations).

* Запустите `dotnet ef migrations add InitialCreate`, чтобы сформировать шаблон миграции и создать начальный набор таблиц для модели.
* Запустите `dotnet ef database update`, чтобы применить созданную миграцию к базе данных. Эта команда создает базу данных и применяет к ней миграции.

База данных SQLite *blogging.db* * находится в каталоге проекта.

## <a name="use-the-model"></a>Использование модели

* Откройте файл *Program.cs* и замените его содержимое следующим кодом:

  [!code-csharp[Main](../../../../samples/core/GetStarted/NetCore/ConsoleApp.SQLite/Program.cs)]

* Проверьте работу приложения:

  `dotnet run`

  В базе данных будет создан один блог, а затем в консоли появятся сведения обо всех блогах.

  ```Console
  ConsoleApp.SQLite>dotnet run
  1 records saved to database

  All blogs in database:
   - http://blogs.msdn.com/adonet
  ```

### <a name="changing-the-model"></a>Изменение модели

- Если вы внесете в модель изменения, с помощью команды `dotnet ef migrations add` вы можете сформировать новую [миграцию](https://docs.microsoft.com/aspnet/core/data/ef-mvc/migrations#introduction-to-migrations). Проверьте сформированный код (и внесите необходимые правки), а затем используйте команду `dotnet ef database update`, чтобы применить изменения схемы к базе данных.
- EF Core использует таблицу `__EFMigrationsHistory` в базе данных для отслеживания уже выполнявшихся в ней миграций.
- Ядро СУБД SQLite не поддерживает некоторые изменения схем, поддерживаемые большинством других реляционных баз данных. Например, не поддерживается операция `DropColumn`. При миграциях EF Core будет создан код для этих операций. Но если вы попробуете применить их к базе данных или сгенерировать скрипт, EF Core будет выдавать исключения. Подробнее о них можно узнать [в этой статье](../../providers/sqlite/limitations.md). Если вы разрабатываете новое приложение, при изменении модели лучше не использовать миграции, а просто отказаться от старой базы данных и создать новую.

## <a name="additional-resources"></a>Дополнительные ресурсы

* [Введение в ASP.NET Core MVC для Mac и Linux](https://docs.microsoft.com/aspnet/core/tutorials/first-mvc-app-xplat/index)
* [Введение в ASP.NET Core MVC для Visual Studio](https://docs.microsoft.com/aspnet/core/tutorials/first-mvc-app/index)
* [Начало работы с ASP.NET Core и Entity Framework Core с использованием Visual Studio](https://docs.microsoft.com/aspnet/core/data/ef-mvc/index)
