---
title: Начало работы в ASP.NET Core — новая база данных — Core EF
author: rick-anderson
ms.author: riande
ms.date: 08/03/2018
ms.assetid: e153627f-f132-4c11-b13c-6c9a607addce
uid: core/get-started/aspnetcore/new-db
ms.openlocfilehash: c6a86dd943dc7fe6f600455fe6743ea01a062aab
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/27/2018
ms.locfileid: "42996068"
---
# <a name="getting-started-with-ef-core-on-aspnet-core-with-a-new-database"></a>Начало работы с EF Core в ASP.NET Core с новой базой данных

В этом руководстве вы создадите приложение ASP.NET Core MVC, которое выполняет базовые операции доступа к данным через платформу Entity Framework Core. Вы создадите из модели EF Core базу данных при помощи миграций.

[Пример для этой статьи на GitHub](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb).

## <a name="prerequisites"></a>Предварительные требования

Установите следующее программное обеспечение:

* [Visual Studio 2017 версии 15.7](https://www.visualstudio.com/downloads/) со следующими рабочими нагрузками:
  * **ASP.NET и веб-разработка** в разделе **Интернет и облако**)
  * **Кроссплатформенная разработка .NET Core** (в разделе **Другие наборы инструментов**)
* [Пакет SDK для NET Core 2.1](https://www.microsoft.com/net/download/core).

## <a name="create-a-new-project-in-visual-studio-2017"></a>Создание нового проекта в Visual Studio 2017

* Откройте Visual Studio 2017.
* Последовательно выберите **Файл > Создать > Проект**.
* В меню слева выберите **"Установленные" > Visual C# > .NET Core**.
* Выберите **Новое веб-приложение ASP.NET Core**.
* Введите имя проекта **EFGetStarted.AspNetCore.NewDb** и щелкните **ОК**.
* В диалоговом окне **Создание веб-приложения ASP.NET Core** сделайте следующее.
  * Выберите пункты **.NET Core** и **ASP.NET Core 2.1** в раскрывающихся списках
  * Выберите **Web Application (Model-View-Controller)** (Веб-приложение "модель — представление — контроллер").
  * Убедитесь, что для параметра **Проверка подлинности** задано значение **Без проверки подлинности**.
  * Нажмите кнопку **ОК**.

Предупреждение. Если для параметра **Проверка подлинности** вы установите значение **Учетные записи отдельных пользователей** вместо **Без проверки подлинности**, в ваш проект будет добавлен файл `Models\IdentityModel.cs` с моделью Entity Framework Core. Используя методы, представленные в этом руководстве, вы можете добавить еще одну модель или расширить уже созданную, включив в нее ваши классы сущностей.

## <a name="install-entity-framework-core"></a>Установка Entity Framework Core

Чтобы установить EF Core, установите пакеты целевых поставщиков базы данных EF Core, с которыми вы будете работать. Список доступных поставщиков вы найдете в разделе [Database Providers](../../providers/index.md) (Поставщики базы данных). 

В этом руководстве используется SQL Server, поэтому устанавливать пакет поставщиков не требуется. Пакет поставщиков SQL Server включен в [метапакет Microsoft.AspnetCore.App](https://docs.microsoft.com/en-us/aspnet/core/fundamentals/metapackage-app?view=aspnetcore-2.1).

## <a name="create-the-model"></a>Создание модели

Задайте класс контекста и классы сущностей, составляющие модель:

* Щелкните папку **Models** (Модели) правой кнопкой мыши и выберите **Добавить > Класс**.
* Введите имя класса **Model.cs** и щелкните **ОК**.
* Замените все содержимое этого файла следующим кодом:

  [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb/Models/Model.cs)]

В реальном приложении каждый класс из модели обычно размещается в отдельном файле. В этом руководстве для простоты все классы заносятся в один файл.

## <a name="register-your-context-with-dependency-injection"></a>Регистрация контекста с помощью внедрения зависимостей

С помощью [внедрения зависимостей](http://docs.asp.net/en/latest/fundamentals/dependency-injection.html) службы (например, `BloggingContext`) регистрируются во время запуска приложения. Затем компоненты, которые используют эти службы (например, контроллеры MVC), обращаются к ним через параметры или свойства конструктора.

Чтобы сделать контекст `BloggingContext` доступным MVC-контроллерам, зарегистрируйте его как службу.

* Откройте файл **Startup.cs**.
* Добавьте в него следующие инструкции `using`:

  [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb/Startup.cs#AddedUsings)]

Вызовите метод `AddDbContext`, чтобы зарегистрировать контекст как службу.

* Добавьте следующий выделенный код в метод `ConfigureServices`:

  [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb/Startup.cs?name=ConfigureServices&highlight=13-14)]

Примечание. В реальном приложении строка подключения обычно помещается в файл конфигурации или переменную среды. В этом руководстве для простоты она задана в коде. Дополнительные сведения вы найдете в статье [Connection Strings](../../miscellaneous/connection-strings.md) (Строки подключения).

## <a name="create-the-database"></a>Создание базы данных

Когда модель будет готова, из нее можно создать базу данных с помощью [миграций](https://docs.microsoft.com/aspnet/core/data/ef-mvc/migrations#introduction-to-migrations):

* Последовательно выберите пункты **Средства > Диспетчер пакетов NuGet > Консоль диспетчера пакетов**.
* Запустите `Add-Migration InitialCreate`, чтобы сформировать шаблон миграции для создания начального набора таблиц для модели. Если появляется сообщение об ошибке `The term 'add-migration' is not recognized as the name of a cmdlet`, закройте и снова откройте Visual Studio.
* Запустите `Update-Database`, чтобы применить созданную миграцию к базе данных. Эта команда создает базу данных и применяет к ней миграции.

## <a name="create-a-controller"></a>Создание контроллера

Добавьте шаблон контроллера и представлений для сущности `Blog`.

* В **обозревателе решений** щелкните папку **Контроллеры** правой кнопкой мыши и выберите пункт **Добавить -> Контроллер**.
* Выберите **Контроллер MVC с представлениями, использующий Entity Framework** и щелкните **Добавить**.
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
