---
title: Начало работы в .NET Core — новая база данных — Core EF
author: rick-anderson
ms.author: riande
description: Начало работы с .NET Core и Entity Framework Core
ms.date: 08/03/2018
ms.assetid: 099d179e-dd7b-4755-8f3c-fcde914bf50b
uid: core/get-started/netcore/new-db-sqlite
ms.openlocfilehash: 6cebe14e179cb6998592f5d3823c114b3bda0138
ms.sourcegitcommit: 5e11125c9b838ce356d673ef5504aec477321724
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/25/2018
ms.locfileid: "50022315"
---
# <a name="getting-started-with-ef-core-on-net-core-console-app-with-a-new-database"></a>Начало работы с EF Core в консольном приложении .NET Core с новой базой данных

В этом руководстве вы создадите консольное приложение .NET Core, которое осуществляет доступ к базе данных SQLite с помощью Entity Framework Core. Вы примените процесс миграций, чтобы создать базу данных из модели. В разделе [ASP.NET Core - новая база данных](xref:core/get-started/aspnetcore/new-db) описана версия для Visual Studio с использованием ASP.NET Core MVC.

[Пример для этой статьи на GitHub](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/NetCore/ConsoleApp.SQLite).

## <a name="prerequisites"></a>Предварительные требования

* [Пакет SDK для NET Core 2.1](https://www.microsoft.com/net/core)

## <a name="create-a-new-project"></a>Создание нового проекта

* Создание консольного проекта:

  ``` Console
  dotnet new console -o ConsoleApp.SQLite
  ```
## <a name="change-the-current-directory"></a>Изменение текущего каталога

В последующих шагах нам нужно выдавать команды `dotnet` для приложения.

* Мы изменяем текущий каталог на каталог приложения следующим образом:

  ``` Console
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

Когда модель будет готова, создайте базу данных с помощью [миграций](xref:core/managing-schemas/migrations/index).

* Запустите `dotnet ef migrations add InitialCreate`, чтобы сформировать шаблон миграции и создать начальный набор таблиц для модели.
* Запустите `dotnet ef database update`, чтобы применить созданную миграцию к базе данных. Эта команда создает базу данных и применяет к ней миграции.

База данных SQLite *blogging.db* * находится в каталоге проекта.

## <a name="use-the-model"></a>Использование модели

* Откройте файл *Program.cs* и замените его содержимое следующим кодом:

  [!code-csharp[Main](../../../../samples/core/GetStarted/NetCore/ConsoleApp.SQLite/Program.cs)]

* Тестирование приложения из консоли. См. раздел [Примечание Visual Studio](#vs) для запуска приложения из Visual Studio.

  `dotnet run`

  В базе данных будет создан один блог, а затем в консоли появятся сведения обо всех блогах.

  ```Console
  ConsoleApp.SQLite>dotnet run
  1 records saved to database

  All blogs in database:
   - http://blogs.msdn.com/adonet
  ```

### <a name="changing-the-model"></a>Изменение модели

- Если вы внесете в модель изменения, с помощью команды `dotnet ef migrations add` вы можете сформировать новую [миграцию](xref:core/managing-schemas/migrations/index). Проверьте сформированный код (и внесите необходимые правки), а затем используйте команду `dotnet ef database update`, чтобы применить изменения схемы к базе данных.
- EF Core использует таблицу `__EFMigrationsHistory` в базе данных для отслеживания уже выполнявшихся в ней миграций.
- Ядро СУБД SQLite не поддерживает некоторые изменения схем, поддерживаемые большинством других реляционных баз данных. Например, не поддерживается операция `DropColumn`. При миграциях EF Core будет создан код для этих операций. Но если вы попробуете применить их к базе данных или сгенерировать скрипт, EF Core будет выдавать исключения. Подробнее о них можно узнать [в этой статье](../../providers/sqlite/limitations.md). Если вы разрабатываете новое приложение, при изменении модели лучше не использовать миграции, а просто отказаться от старой базы данных и создать новую.

<a name="vs"></a>
### <a name="run-from-visual-studio"></a>Запуск из Visual Studio

Чтобы запустить этот пример из Visual Studio, необходимо вручную задать рабочий каталог, который должен быть корнем проекта. Если рабочий каталог не задан, возникает следующее исключение `Microsoft.Data.Sqlite.SqliteException`: `SQLite Error 1: 'no such table: Blogs'`.

Чтобы задать рабочий каталог, выполните следующие действия.

* В **обозревателе решений** щелкните проект правой кнопкой мыши и выберите пункт **Свойства**.
* Выберите вкладку **Отладка** в левой панели.
* Задайте каталог проекта в качестве **рабочего каталога**.
* Сохраните изменения.

## <a name="additional-resources"></a>Дополнительные ресурсы

* [Руководство. Начало работы с EF Core в ASP.NET Core с новой базой данных с использованием SQLite](xref:core/get-started/aspnetcore/new-db)
* [Руководство. Начало работы с Razor Pages в ASP.NET Core](https://docs.microsoft.com/aspnet/core/tutorials/razor-pages/razor-pages-start)
* [Руководство. Razor Pages с Entity Framework Core в ASP.NET Core](https://docs.microsoft.com/aspnet/core/data/ef-rp/intro)
