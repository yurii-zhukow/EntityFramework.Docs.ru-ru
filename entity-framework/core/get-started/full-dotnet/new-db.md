---
title: Начало работы в .NET Framework — новая база данных — Core EF
author: rowanmiller
ms.date: 08/06/2018
ms.assetid: 52b69727-ded9-4a7b-b8d5-73f3acfbbad3
uid: core/get-started/full-dotnet/new-db
ms.openlocfilehash: 66d9011a5978fc3d8253a963bdb9df27848e9ff9
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/27/2018
ms.locfileid: "42997591"
---
# <a name="getting-started-with-ef-core-on-net-framework-with-a-new-database"></a>Начало работы с EF Core в .NET Framework с новой базой данных

В этом руководстве вы создадите консольное приложение, которое реализует простейший доступ к базе данных Microsoft SQL Server с помощью Entity Framework. Вы создадите из модели базу данных при помощи миграций.

[Пример для этой статьи на GitHub](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/FullNet/ConsoleApp.NewDb).

## <a name="prerequisites"></a>Предварительные требования

* [Visual Studio 2017 версии 15.7 или выше](https://www.visualstudio.com/downloads/)

## <a name="create-a-new-project"></a>Создание нового проекта

* Откройте Visual Studio 2017.

* **"Файл" > "Создать" > "Проект"…**

* В меню слева выберите **"Установленные" > Visual C# > Классическое приложение Windows**

* Выберите шаблон проекта **Консольное приложение (.NET Framework)**.

* Задайте **.NET Framework 4.6.1** или более позднюю версию в качестве целевой платформы проекта

* Назовите проект *ConsoleApp.NewDb* и нажмите кнопку **ОК**

## <a name="install-entity-framework"></a>Установка Entity Framework

Чтобы использовать EF Core, установите пакеты для поставщиков базы данных, с которыми вы будете работать. В этом руководстве используется SQL Server. Список доступных поставщиков вы найдете в разделе [Database Providers](../../providers/index.md) (Поставщики базы данных).

* Последовательно выберите пункты "Средства" -> "Диспетчер пакетов NuGet" -> "Консоль диспетчера пакетов".

* Запуск `Install-Package Microsoft.EntityFrameworkCore.SqlServer`

Далее в этом руководстве вы воспользуетесь рядом инструментов Entity Framework Tools для обслуживания базы данных. Поэтому также установите пакет инструментов.

* Запуск `Install-Package Microsoft.EntityFrameworkCore.Tools`

## <a name="create-the-model"></a>Создание модели

Теперь нужно задать контекст и классы сущностей, которые составляют модель.

* **"Проект" > "Добавить класс"…**

* Введите имя класса *Model.cs* и щелкните **ОК**.

* Замените все содержимое этого файла следующим кодом:

  [!code-csharp[Main](../../../../samples/core/GetStarted/FullNet/ConsoleApp.NewDb/Model.cs)] 

> [!TIP]  
> В реальном приложении каждый класс помещается в отдельный файл, а строка подключения — в файл конфигурации или переменную среды. В этом руководстве для простоты все занесено в один файл кода.

## <a name="create-the-database"></a>Создание базы данных

Итак, модель готова, и вы можете создать из нее базу данных с помощью миграций.

* Последовательно выберите пункты **Средства > Диспетчер пакетов NuGet > Консоль диспетчера пакетов**.

* Выполните `Add-Migration InitialCreate`, чтобы сформировать шаблон миграции и создать начальный набор таблиц для модели.

* Запустите `Update-Database`, чтобы применить созданную миграцию к базе данных. Так как база данных еще не существует, она будет создана перед выполнением миграции.

> [!TIP]  
> Если вы внесете в модель изменения, с помощью команды `Add-Migration` вы сможете сформировать новый шаблон миграции, который вносит необходимые изменения в схему базы данных. Проверьте сформированный код шаблона (и внесите правки, если потребуется), а затем выполните команду `Update-Database`, чтобы применить изменения к базе данных.
>
> EF использует таблицу `__EFMigrationsHistory` в базе данных, чтобы отслеживать уже примененные к базе данных миграции.

## <a name="use-the-model"></a>Использование модели

Теперь вы можете использовать созданную модель для доступа к данным.

* Откройте файл *Program.cs*.

* Замените все содержимое этого файла следующим кодом:

  [!code-csharp[Main](../../../../samples/core/GetStarted/FullNet/ConsoleApp.NewDb/Program.cs)]

* **"Отладка" > "Запустить без отладки"**

  Вы увидите, как один блог сохраняется в базе данных, а затем сведения обо всех блогах выводятся в консоль.

  ![изображение](_static/output-new-db.png)

## <a name="additional-resources"></a>Дополнительные ресурсы

* [EF Core в .NET Framework с существующей базой данных](xref:core/get-started/full-dotnet/existing-db)
* [EF Core в .NET Core с новой базой данных — SQLite](xref:core/get-started/netcore/new-db-sqlite). Руководство по кроссплатформенной консоли EF.
