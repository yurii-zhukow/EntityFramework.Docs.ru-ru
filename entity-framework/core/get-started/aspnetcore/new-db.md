---
title: "Начало работы в ASP.NET Core — новая база данных — Core EF"
author: rick-anderson
ms.author: riande
ms.author2: tdykstra
ms.date: 04/07/2017
ms.topic: get-started-article
ms.assetid: e153627f-f132-4c11-b13c-6c9a607addce
ms.technology: entity-framework-core
uid: core/get-started/aspnetcore/new-db
ms.openlocfilehash: f6ed19d3c5d2ae8d1f5756558e50c1f0dddd2f07
ms.sourcegitcommit: d2434edbfa6fbcee7287e33b4915033b796e417e
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/12/2018
---
# <a name="getting-started-with-ef-core-on-aspnet-core-with-a-new-database"></a>Начало работы с EF Core в ASP.NET Core с новой базой данных

В этом пошаговом руководстве вы создадите приложение ASP.NET Core MVC, которое выполняет базовые операции доступа к данным через платформу Entity Framework Core. Вы примените процесс миграций, чтобы создать базу данных из модели EF Core. В разделе [Дополнительные ресурсы](#additional-resources) вы найдете еще несколько руководств по платформе Entity Framework Core.

Для работы с этим руководством вам потребуется следующее:
* [Visual Studio 2017 15.3](https://www.visualstudio.com/downloads/) с такими рабочими нагрузками:
  * **ASP.NET и веб-разработка** в разделе **Интернет и облако**)
  * **Кроссплатформенная разработка .NET Core** (в разделе **Другие наборы инструментов**)
* [пакет SDK для .NET Core 2.0](https://www.microsoft.com/net/download/core).

> [!TIP]  
> Для этой статьи вы можете скачать [пример](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb) из репозитория GitHub.

## <a name="create-a-new-project-in-visual-studio-2017"></a>Создание нового проекта в Visual Studio 2017

* Последовательно выберите **Файл > Создать > Проект**.
* В меню слева выберите **Установленные > Шаблоны > Visual C# -> .NET Core**.
* Выберите **Новое веб-приложение ASP.NET Core**.
* Введите имя проекта **EFGetStarted.AspNetCore.NewDb** и щелкните **ОК**.
* В диалоговом окне **Создание веб-приложения ASP.NET Core** сделайте следующее.
  * Убедитесь, что в раскрывающихся списках выбраны варианты **.NET Core** и **ASP.NET Core 2.0**.
  * Выберите **Web Application (Model-View-Controller)** (Веб-приложение "модель — представление — контроллер").
  * Убедитесь, что для параметра **Проверка подлинности** задано значение **Без проверки подлинности**.
  * Нажмите кнопку **ОК**.

Предупреждение. Если для параметра **Проверка подлинности** вы установите значение **Учетные записи отдельных пользователей** вместо **Без проверки подлинности**, в ваш проект будет добавлен файл `Models\IdentityModel.cs` с моделью Entity Framework Core. Используя методы, которые представлены в этом пошаговом руководстве, вы можете добавить еще одну модель или включить ваши классы сущностей в уже созданную модель.

## <a name="install-entity-framework-core"></a>Установка Entity Framework Core

Установите пакеты для поставщиков базы данных EF Core, с которыми вы будете работать. В этом пошаговом руководстве используется SQL Server. Список доступных поставщиков вы найдете в разделе [Database Providers](../../providers/index.md) (Поставщики базы данных).

* Последовательно выберите пункты **Средства > Диспетчер пакетов NuGet > Консоль диспетчера пакетов**.

* Запуск `Install-Package Microsoft.EntityFrameworkCore.SqlServer`

Нам понадобятся некоторые средства платформы Entity Framework, чтобы создать базу данных из модели EF Core. Поэтому мы установим пакет средств:

* Запуск `Install-Package Microsoft.EntityFrameworkCore.Tools`

Позднее мы применим средства формирования шаблонов для ASP.NET Core, чтобы создать контроллеры и представления. Поэтому мы установим этот пакет разработки:

* Запуск `Install-Package Microsoft.VisualStudio.Web.CodeGeneration.Design`

## <a name="create-the-model"></a>Создание модели

Определите контекст и классы сущности, которые входят в модель:

* Щелкните папку **Models** (Модели) правой кнопкой мыши и выберите **Добавить > Класс**.
* Введите имя класса **Model.cs** и щелкните **ОК**.
* Замените все содержимое этого файла следующим кодом:

 [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb/Models/Model.cs)]

Примечание. В реальном приложении каждый класс из модели обычно размещается в отдельном файле. В этом руководстве мы для простоты заносим все классы в один файл.

## <a name="register-your-context-with-dependency-injection"></a>Регистрация контекста с помощью внедрения зависимостей

С помощью [внедрения зависимостей](http://docs.asp.net/en/latest/fundamentals/dependency-injection.html) службы (например, `BloggingContext`) регистрируются во время запуска приложения. Затем компоненты, которые используют эти службы (например, контроллеры MVC), обращаются к ним через параметры или свойства конструктора.

Чтобы контроллеры MVC могли использовать службу `BloggingContext`, мы должны ее зарегистрировать:

* Откройте файл **Startup.cs**.
* Добавьте в него следующие инструкции `using`:

 [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb/Startup.cs#AddedUsings)]

Добавьте метод `AddDbContext`, чтобы зарегистрировать нашу службу:

* Добавьте следующий код в метод `ConfigureServices`:

 [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb/Startup.cs?name=ConfigureServices&highlight=7-8)]

Примечание. В реальном приложении строка подключения обычно помещается в файл конфигурации. Здесь мы для простоты определяем ее прямо в коде. Дополнительные сведения вы найдете в статье [Connection Strings](../../miscellaneous/connection-strings.md) (Строки подключения).

## <a name="create-your-database"></a>Создание базы данных

Когда модель будет готова, из нее можно создать базу данных с помощью [миграций](https://docs.microsoft.com/aspnet/core/data/ef-mvc/migrations#introduction-to-migrations):

* Откройте PMC:

  Последовательно выберите пункты **Средства -> Диспетчер пакетов NuGet -> Консоль диспетчера пакетов**.
* Запустите `Add-Migration InitialCreate`, чтобы сформировать шаблон миграции для создания начального набора таблиц для модели. Если появляется сообщение об ошибке `The term 'add-migration' is not recognized as the name of a cmdlet`, закройте и снова откройте Visual Studio.
* Запустите `Update-Database`, чтобы применить созданную миграцию к базе данных. Эта команда создает базу данных и применяет к ней миграции.

## <a name="create-a-controller"></a>Создание контроллера

Включите в проект формирование шаблонов:

* В **обозревателе решений** щелкните папку **Контроллеры** правой кнопкой мыши и выберите пункт **Добавить -> Контроллер**.
* Выберите **Минимальные зависимости** и щелкните **Добавить**.
* Файл *ScaffoldingReadMe.txt* можно игнорировать или удалить.

Теперь формирование шаблонов включено, и мы можем сформировать контроллер для сущности `Blog`:

* В **обозревателе решений** щелкните папку **Контроллеры** правой кнопкой мыши и выберите пункт **Добавить -> Контроллер**.
* Выберите **Контроллер MVC с представлениями, использующий Entity Framework** и щелкните **ОК**.
* Для параметра **Класс модели** установите значение **Блог**, а для параметра **Класс контекста данных** — **BloggingContext**.
* Нажмите кнопку **Добавить**.


## <a name="run-the-application"></a>Запуск приложения

Нажмите клавишу F5, чтобы запустить приложение для проверки.

* Перейдите к папке `/Blogs`.
* С помощью ссылки создания внесите в блог несколько записей. Проверьте также работу ссылок для просмотра и удаления.

![изображение](_static/create.png)

![изображение](_static/index-new-db.png)

## <a name="additional-resources"></a>Дополнительные ресурсы

* [Руководство по применению кроссплатформенной консоли EF с новой базой данных SQLite](xref:core/get-started/netcore/new-db-sqlite)
* [Введение в ASP.NET Core MVC для Mac и Linux](https://docs.microsoft.com/aspnet/core/tutorials/first-mvc-app-xplat/index)
* [Введение в ASP.NET Core MVC для Visual Studio](https://docs.microsoft.com/aspnet/core/tutorials/first-mvc-app/index)
* [Начало работы с ASP.NET Core и Entity Framework Core с использованием Visual Studio](https://docs.microsoft.com/aspnet/core/data/ef-mvc/index)
