---
title: Начало работы в UWP — новая база данных — Core EF
author: rowanmiller
ms.date: 10/13/2018
ms.assetid: a0ae2f21-1eef-43c6-83ad-92275f9c0727
uid: core/get-started/uwp/getting-started
ms.openlocfilehash: 48d26adbe17e4734753a7ada547b9c13317bef0d
ms.sourcegitcommit: 8b42045cd21f80f425a92f5e4e9dd4972a31720b
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/14/2018
ms.locfileid: "49315624"
---
# <a name="getting-started-with-ef-core-on-universal-windows-platform-uwp-with-a-new-database"></a>Начало работы с EF Core на универсальной платформе Windows (UWP) с новой базой данных

В этом руководстве вы создадите приложение универсальной платформы Windows (UWP), которое осуществляет простейший доступ к локальной базе данных SQLite с помощью Entity Framework Core.

[Пример для этой статьи на GitHub](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/UWP).

## <a name="prerequisites"></a>Предварительные требования

* [Windows 10 Fall Creators Update (10.0, сборка 16299) или более поздняя версия](https://support.microsoft.com/en-us/help/4027667/windows-update-windows-10).

* [Visual Studio 2017 версии 15.7 или более поздняя версия](https://www.visualstudio.com/downloads/) с рабочей нагрузкой **Разработка приложений для универсальной платформы Windows**.

* [Пакет SDK для .NET Core 2.1](https://www.microsoft.com/net/core) или более поздняя версия.

> [!IMPORTANT]
> В этом руководстве используются команды [миграций](xref:core/managing-schemas/migrations/index) Entity Framework Core для создания и обновления схемы базы данных.
> Эти команды не работают напрямую в проектах UWP.
> По этой причине модель данных приложения помещается в общий проект библиотеки, и для выполнения команд используется отдельное консольное приложение .NET Core.

## <a name="create-a-library-project-to-hold-the-data-model"></a>Создание проекта библиотеки для хранения модели данных

* Открытие Visual Studio

* Последовательно выберите **Файл > Создать > Проект**.

* В меню слева выберите **"Установленные" > Visual C# > .NET Standard**.

* Выберите шаблон **Библиотека классов (.NET Standard)**.

* Назовите проект *Blogging.Model*.

* Назовите решение *Blogging*.

* Нажмите кнопку **ОК**.

## <a name="install-entity-framework-core-runtime-in-the-data-model-project"></a>Установка среды выполнения Entity Framework Core в проекте модели данных

Чтобы использовать EF Core, установите пакеты для поставщиков базы данных, с которыми вы будете работать. В этом руководстве используется SQLite. Список доступных поставщиков вы найдете в разделе [Database Providers](../../providers/index.md) (Поставщики базы данных).

* Выберите **"Сервис" > "Диспетчер пакетов NuGet" > "Консоль диспетчера пакетов"**.

* Убедитесь, что проект библиотеки *Blogging.Model* выбран в качестве **проекта по умолчанию** в консоли диспетчера пакетов.

* Запуск `Install-Package Microsoft.EntityFrameworkCore.Sqlite`

## <a name="create-the-data-model"></a>Создание модели данных

Теперь нужно задать контекст *DbContext* и классы сущностей, которые составляют модель.

* Удалите *Class1.cs*.

* Создайте файл *Model.cs* со следующим кодом:

  [!code-csharp[Main](../../../../samples/core/GetStarted/UWP/Blogging.Model/Model.cs)]

## <a name="create-a-new-console-project-to-run-migrations-commands"></a>Создание проекта консольного приложения для выполнения команд миграции

* В **обозревателе решений** щелкните решение правой кнопкой мыши и выберите **"Добавить" > "Новый проект"**.

* В меню слева выберите **"Установленные" > Visual C# > .NET Core**.

* Выберите шаблон проекта **Консольное приложение (.NET Core)**.

* Назовите проект *Blogging.Migrations.Startup* и нажмите кнопку **ОК**.

* Добавьте ссылку на проект из *Blogging.Migrations.Startup* в *Blogging.Model*.

## <a name="install-entity-framework-core-tools-in-the-migrations-startup-project"></a>Установка инструментов Entity Framework Core в проекте запуска миграций

Чтобы включить команды миграции EF Core в консоли диспетчера пакетов, установите пакет средств EF Core в консольном приложении.

* Последовательно выберите пункты **Средства > Диспетчер пакетов NuGet > Консоль диспетчера пакетов**.

* Запуск `Install-Package Microsoft.EntityFrameworkCore.Tools -ProjectName Blogging.Migrations.Startup`

## <a name="create-the-initial-migration"></a>Создание первоначальной миграции

 Создайте первоначальную миграцию, указав в качестве проекта запуска консольное приложение.

* Запуск `Add-Migration InitialCreate -StartupProject Blogging.Migrations.Startup`

Эта команда формирует миграцию, которая создает начальный набор таблиц базы данных для вашей модели.

## <a name="create-the-uwp-project"></a>Создание проекта UWP

* В **обозревателе решений** щелкните решение правой кнопкой мыши и выберите **"Добавить" > "Новый проект"**.

* В меню слева выберите **"Установленные" > Visual C# > "Универсальные приложения для Windows"**.

* Выберите шаблон проекта **Пустое приложение (универсальное приложение для Windows)**.

* Назовите проект *Blogging.UWP* и нажмите кнопку **ОК**

> [!IMPORTANT]
> Задайте в качестве целевой и минимальной версий как минимум **Windows 10 Fall Creators Update (10.0, сборка 16299.0)**.
> Предыдущие версии Windows 10 не поддерживают .NET Standard 2.0, который необходим для Entity Framework Core.

## <a name="add-code-to-create-the-database-on-application-startup"></a>Добавление кода для создания базы данных при запуске приложения

Так как вы хотите создать базу данных на устройстве, где работает приложение, добавьте код, который при запуске приложения выполнит все незавершенные миграции для локальной базы данных. Таким образом локальная база данных будет создана автоматически при первом запуске приложения.

* Добавьте ссылку на проект из *Blogging.UWP* в *Blogging.Model*.

* Откройте файл *App.xaml.cs*.

* Добавьте выделенный код, который выполняет незавершенные миграции.

  [!code-csharp[Main](../../../../samples/core/GetStarted/UWP/Blogging.UWP/App.xaml.cs?highlight=1-2,26-29)]

> [!TIP]  
> Если вы внесете в модель изменения, с помощью команды `Add-Migration` вы сможете сформировать новую миграцию, которая внесет соответствующие изменения в базу данных. Теперь при каждом запуске приложения к локальной базе данных на каждом устройстве будут применяться все незавершенные миграции.
>
>EF Core использует таблицу `__EFMigrationsHistory` в базе данных для отслеживания уже выполнявшихся в ней миграций.

## <a name="use-the-data-model"></a>Использование модели данных

Теперь вы можете использовать EF Core для доступа к данным.

* Откройте файл *MainPage.xaml*.

* Добавьте обработчик загрузки страницы и содержимое пользовательского интерфейса, которые выделены ниже.

[!code-xml[Main](../../../../samples/core/GetStarted/UWP/Blogging.UWP/MainPage.xaml?highlight=9,11-23)]

Теперь добавьте код, который свяжет пользовательский интерфейс с базой данных

* Откройте файл *MainPage.xaml.cs*.

* Добавьте в него код, выделенный в следующем листинге:

[!code-csharp[Main](../../../../samples/core/GetStarted/UWP/Blogging.UWP/MainPage.xaml.cs?highlight=1,31-49)]

Теперь вы можете запустить приложение и увидеть, как оно работает:

* В **обозревателе решений** щелкните правой кнопкой мыши проект *Blogging.UWP* и выберите **Развернуть**.

* Задайте *Blogging.UWP* в качестве автозагружаемого проекта.

* **"Отладка" > "Запустить без отладки"**

  Произойдет сборка и запуск приложения.

* Введите URL-адрес и нажмите кнопку **Добавить**.

  ![изображение](_static/create.png)

  ![изображение](_static/list.png)

  Готово! Теперь у вас есть простое приложение UWP, в котором работает Entity Framework Core.

## <a name="next-steps"></a>Следующие шаги

Сведения о совместимости и производительности, которые нужно знать при использовании EF Core с UWP, см. в разделе [Реализации .NET, поддерживаемые EF Core](../../platforms/index.md#universal-windows-platform).

Другие статьи этой документации помогут вам расширить свои знания о возможностях Entity Framework Core.
