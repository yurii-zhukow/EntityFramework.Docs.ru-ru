---
title: Начало работы в UWP — новая база данных — Core EF
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.topic: get-started-article
ms.assetid: a0ae2f21-1eef-43c6-83ad-92275f9c0727
ms.technology: entity-framework-core
uid: core/get-started/uwp/getting-started
ms.openlocfilehash: f743ff5392d1f30283a13d2e7fb8029be88387aa
ms.sourcegitcommit: 96324e58c02b97277395ed43173bf13ac80d2012
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/01/2017
ms.locfileid: "26049837"
---
# <a name="getting-started-with-ef-core-on-universal-windows-platform-uwp-with-a-new-database"></a>Начало работы с EF Core на универсальной платформе Windows (UWP) с новой базой данных

> [!NOTE]
> В этом руководстве используется EF Core 2.0.1 (выпущен вместе с ASP.NET Core и пакетом SDK для .NET Core 2.0.3). В EF Core 2.0.0 еще не исправлены некоторые ошибки, оказывающие важное влияние на работу с UWP.

В этом пошаговом руководстве вы создадите приложение UWP, которое осуществляет базовые операции доступа к локальной базе данных SQLite с помощью Entity Framework.

> [!IMPORTANT]
> **Старайтесь не использовать анонимные типы в запросах LINQ на UWP**. Чтобы развернуть приложение UWP в магазине приложений, оно должно быть скомпилировано в .NET Native. Запросы с анонимными типами плохо работают в среде .NET Native.

> [!TIP]
> Для этой статьи вы можете скачать [пример](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/UWP/UWP.SQLite) из репозитория GitHub.

## <a name="prerequisites"></a>Предварительные требования

Для выполнения этого пошагового руководства необходимы следующие элементы:

* [Windows 10 Fall Creators Update](https://support.microsoft.com/en-us/help/4027667/windows-update-windows-10) (10.0.16299.0);

* [пакет SDK .NET Core 2.0.0](https://www.microsoft.com/net/core) или более поздней версии;

* [Visual Studio 2017](https://www.visualstudio.com/downloads/) версии 15.4 или более поздней версии с рабочей нагрузкой **Разработка с помощью универсальной платформы Windows**.

## <a name="create-a-new-model-project"></a>Создание нового проекта модели

> [!WARNING]
> Из-за ограничений в методах взаимодействия между средствами .NET Core и проектами UWP, модель нельзя помещать в проект UWP, если для нее нужно выполнять команды миграции в консоли диспетчера пакетов.

* Откройте Visual Studio.

* Последовательно выберите "Файл" > "Создать" > "Проект".

* В меню слева выберите "Шаблоны" -> Visual C#.

* Выберите шаблон проекта **Библиотека классов (.NET Standard)**.

* Присвойте проекту имя и щелкните **ОК**.

## <a name="install-entity-framework"></a>Установка Entity Framework

Чтобы использовать EF Core, установите пакеты для поставщиков базы данных, с которыми вы будете работать. В этом пошаговом руководстве используется SQLite. Список доступных поставщиков вы найдете в разделе [Database Providers](../../providers/index.md) (Поставщики базы данных).

* Последовательно выберите пункты "Средства" -> "Диспетчер пакетов NuGet" -> "Консоль диспетчера пакетов".

* Выполните `Install-Package Microsoft.EntityFrameworkCore.Sqlite`.

Далее в этом пошаговом руководстве мы будем использовать некоторые средства платформы Entity Framework для обслуживания базы данных. Поэтому мы установим пакет средств:

* Выполните `Install-Package Microsoft.EntityFrameworkCore.Tools`.

* Измените CSPROJ-файл, заменив в нем `<TargetFramework>netstandard2.0</TargetFramework>` на `<TargetFrameworks>netcoreapp2.0;netstandard2.0</TargetFrameworks>`.

## <a name="create-your-model"></a>Создание модели

Теперь нам нужно определить контекст и классы сущности, которые входят в модель:

* Выберите элементы "Проект" > "Добавить класс".

* Введите имя класса *Model.cs* и щелкните **ОК**.

* Замените все содержимое этого файла следующим кодом:

[!code-csharp[Main](../../../../samples/core/GetStarted/UWP/UWP.Model/Model.cs)]

## <a name="create-a-new-uwp-project"></a>Создание нового проекта UWP

* Откройте Visual Studio.

* Последовательно выберите "Файл" > "Создать" > "Проект".

* В меню слева выберите элементы "Шаблоны" > "Visual C#" > "Универсальные приложения Windows".

* Выберите шаблон проекта **Пустое приложение (универсальное приложение Windows)**.

* Присвойте проекту имя и щелкните **ОК**.

* В качестве целевой и минимальной версий укажите по меньшей мере `Windows 10 Fall Creators Update (10.0; build 16299.0)`.

## <a name="create-your-database"></a>Создание базы данных

Итак, модель будет готова, и вы можете создать из нее базу данных с помощью миграций:

* Последовательно выберите пункты "Средства" -> "Диспетчер пакетов NuGet" -> "Консоль диспетчера пакетов".

* Выберите проект модели в качестве проекта по умолчанию и укажите его в качестве запускаемого проекта.

* Запустите `Add-Migration MyFirstMigration`, чтобы сформировать шаблон миграции для создания начального набора таблиц для модели.

Поскольку мы хотим создать базу данных на устройстве, где работает приложение, нам нужно добавить код, который при запуске приложения применит все незавершенные миграции к локальной базе данных. Таким образом, при первом запуске приложения локальная база данных будет создана автоматически.

* В **обозревателе решений** щелкните правой кнопкой мыши файл **App.xaml** и выберите команду **Просмотреть код**.

* Добавьте в начало файла выделенные инструкции using.

* Добавьте выделенный код, который применяет незавершенные миграции:

[!code-csharp[Main](../../../../samples/core/GetStarted/UWP/UWP.SQLite/App.xaml.cs?highlight=1,25-28)]

> [!TIP]  
> Если позднее вы внесете изменения в эту модель, с помощью команды `Add-Migration` вы сможете создать новую миграцию, которая применяет необходимые изменения к базе данных. Теперь при каждом запуске приложения к локальной базе данных на каждом устройстве будут применяться все незавершенные миграции.
>
>EF использует таблицу `__EFMigrationsHistory` в базе данных, чтобы отслеживать уже примененные к базе данных миграции.

## <a name="use-your-model"></a>Использование модели

Теперь вы можете использовать созданную модель для доступа к данным.

* Откройте файл *MainPage.xaml*.

* Добавьте обработчик загрузки страницы и содержимое пользовательского интерфейса, которые выделены ниже.

[!code-xml[Main](../../../../samples/core/GetStarted/UWP/UWP.SQLite/MainPage.xaml?highlight=9,11-23)]

Теперь мы добавим код, который подключает пользовательский интерфейс к базе данных:

* В **обозревателе решений** щелкните правой кнопкой мыши файл **MainPage.xaml** и выберите команду **Просмотреть код**.

* Добавьте в него код, выделенный в следующем примере:

[!code-csharp[Main](../../../../samples/core/GetStarted/UWP/UWP.SQLite/MainPage.xaml.cs?highlight=30-48)]

Теперь вы можете запустить приложение и увидеть, как оно работает:

* Выберите "Отладка" -> "Запустить без отладки".

* Это действие компилирует и запускает приложение.

* Введите URL-адрес и нажмите кнопку **Добавить**.

![image](_static/create.png)

![image](_static/list.png)

## <a name="next-steps"></a>Следующие шаги

> [!TIP]
> Производительность метода `SaveChanges()` можно повысить, реализовав в типах сущностей [`INotifyPropertyChanged`](https://msdn.microsoft.com/en-us/library/system.componentmodel.inotifypropertychanged.aspx), [`INotifyPropertyChanging`](https://msdn.microsoft.com/en-us/library/system.componentmodel.inotifypropertychanging.aspx), [`INotifyCollectionChanged`](https://msdn.microsoft.com/en-us/library/system.collections.specialized.inotifycollectionchanged.aspx) и применив к ним `ChangeTrackingStrategy.ChangingAndChangedNotifications`.

Готово! Теперь у вас есть простое приложение UWP, на котором выполняется Entity Framework.

Изучите другие статьи в этом пакете документации, чтобы расширить свои знания о возможностях платформы Entity Framework.
