---
title: Начало работы в ASP.NET Core — новая база данных — Core EF
author: rick-anderson
ms.author: riande
ms.date: 08/03/2018
ms.assetid: e153627f-f132-4c11-b13c-6c9a607addce
uid: core/get-started/aspnetcore/new-db
ms.openlocfilehash: 2eb1668b8c077fabc9cb21088452fd1bead7ff22
ms.sourcegitcommit: ea1cdec0b982b922a59b9d9301d3ed2b94baca0f
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/31/2019
ms.locfileid: "66452248"
---
# <a name="getting-started-with-ef-core-on-aspnet-core-with-a-new-database"></a>Начало работы с EF Core в ASP.NET Core с новой базой данных

В этом руководстве вы создадите приложение ASP.NET Core MVC, которое выполняет базовые операции доступа к данным через платформу Entity Framework Core. В этом руководстве используются миграции для создания базы данных из модели данных.

Вы можете выполнять это руководство с помощью Visual Studio 2017 в Windows или с помощью интерфейса командной строки .NET Core CLI в Windows, macOS или Linux.

Пример для этой статьи на GitHub:
* [Visual Studio 2017 с SQL Server](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb)
* [.NET Core CLI с SQLite](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb.Sqlite).

## <a name="prerequisites"></a>Предварительные требования

Установите следующее программное обеспечение:

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* [Visual Studio 2017 версии 15.7 или более поздней](https://www.visualstudio.com/downloads/) со следующими рабочими нагрузками:
  * **ASP.NET и веб-разработка** в разделе **Интернет и облако**)
  * **Кроссплатформенная разработка .NET Core** (в разделе **Другие наборы инструментов**)
* [Пакет SDK для NET Core 2.1](https://www.microsoft.com/net/download/core).

# <a name="net-core-clitabnetcore-cli"></a>[Интерфейс командной строки .NET Core](#tab/netcore-cli)

* [Пакет SDK для NET Core 2.1](https://www.microsoft.com/net/download/core).

---

## <a name="create-a-new-project"></a>Создание нового проекта

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* Откройте Visual Studio 2017.
* Последовательно выберите **Файл > Создать > Проект**.
* В меню слева выберите **Установленные > Visual C# > .NET Core**.
* Выберите **Новое веб-приложение ASP.NET Core**.
* Введите имя проекта **EFGetStarted.AspNetCore.NewDb** и щелкните **ОК**.
* В диалоговом окне **Создание веб-приложения ASP.NET Core** сделайте следующее.
  * Выберите **.NET Core** и **ASP.NET Core 2.1** в раскрывающихся списках.
  * Выберите **Web Application (Model-View-Controller)** (Веб-приложение "модель — представление — контроллер").
  * Убедитесь, что для параметра **Проверка подлинности** задано значение **Без проверки подлинности**.
  * Нажмите кнопку **ОК**.

Предупреждение: Если для параметра **Проверка подлинности** установить значение **Учетные записи отдельных пользователей** вместо **Без проверки подлинности**, в проект будет добавлен файл `Models\IdentityModel.cs` с моделью Entity Framework Core. Используя методы, представленные в этом руководстве, вы можете добавить еще одну модель или расширить уже созданную, включив в нее ваши классы сущностей.

# <a name="net-core-clitabnetcore-cli"></a>[Интерфейс командной строки .NET Core](#tab/netcore-cli)

* Выполните следующую команду для создания проекта MVC.

   ```cli
   dotnet new mvc -n EFGetStarted.AspNetCore.NewDb
   ```
* Перейдите в каталог проекта. Следующие команды нужно выполнять для нового проекта.

   ```cli
   cd EFGetStarted.AspNetCore.NewDb
   ```
---

## <a name="install-entity-framework-core"></a>Установка Entity Framework Core

Чтобы установить EF Core, установите пакеты целевых поставщиков базы данных EF Core, с которыми вы будете работать. Список доступных поставщиков см. в разделе [Database Providers](../../providers/index.md) (Поставщики базы данных).

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

В этом руководстве используется SQL Server, поэтому устанавливать пакет поставщиков не требуется. Пакет поставщиков SQL Server включен в [метапакет Microsoft.AspnetCore.App](https://docs.microsoft.com/aspnet/core/fundamentals/metapackage-app?view=aspnetcore-2.1).

# <a name="net-core-clitabnetcore-cli"></a>[Интерфейс командной строки .NET Core](#tab/netcore-cli)

В этом руководстве используется SQLite, так как он работает на всех платформах, поддерживаемых .NET Core.

* Чтобы установить поставщика SQLite, выполните следующую команду:

   ```cli
   dotnet add package Microsoft.EntityFrameworkCore.Sqlite
   ```

---

## <a name="create-the-model"></a>Создание модели

Задайте класс контекста и классы сущностей, составляющие модель.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* Щелкните папку **Models** (Модели) правой кнопкой мыши и выберите **Добавить > Класс**.
* Введите имя класса **Model.cs** и щелкните **ОК**.
* Замените все содержимое этого файла следующим кодом:

  [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb/Models/Model.cs)]

# <a name="net-core-clitabnetcore-cli"></a>[Интерфейс командной строки .NET Core](#tab/netcore-cli)

* В папке **Models** создайте файл **Model.cs**, содержащий следующий код:

  [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb.Sqlite/Models/Model.cs)]

---

Рабочее приложение обычно помещает каждый класс в отдельный файл. В этом руководстве для простоты все классы помещаются в один файл.

## <a name="register-the-context-with-dependency-injection"></a>Регистрация контекста с помощью внедрения зависимостей

Чтобы сделать контекст `BloggingContext` доступным контроллерам MVC, зарегистрируйте его как службу в `Startup.cs`.

Службы (такие как `BloggingContext`) регистрируются с помощью [внедрения зависимостей](http://docs.asp.net/en/latest/fundamentals/dependency-injection.html) во время запуска приложения, чтобы компоненты, которым они требуются (например, контроллеры MVC), могли обращаться к ним автоматически через свойства и параметры конструктора.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* Добавьте в файл **Startup.cs** следующие инструкции `using`:

  [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb/Startup.cs#AddedUsings)]

* Добавьте следующий выделенный код в метод `ConfigureServices`:

  [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb/Startup.cs?name=ConfigureServices&highlight=12-14)]

# <a name="net-core-clitabnetcore-cli"></a>[Интерфейс командной строки .NET Core](#tab/netcore-cli)

* Добавьте в файл **Startup.cs** следующие инструкции `using`:

  [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb.Sqlite/Startup.cs#AddedUsings)]

* Добавьте следующий выделенный код в метод `ConfigureServices`:

  [!code-csharp[Main](../../../../samples/core/GetStarted/AspNetCore/EFGetStarted.AspNetCore.NewDb.Sqlite/Startup.cs?name=ConfigureServices&highlight=12-14)]

---

В реальном рабочем приложении строка подключения обычно помещается в файл конфигурации или в переменную среды. В этом руководстве для простоты она задана в коде. Дополнительные сведения вы найдете в статье [Connection Strings](../../miscellaneous/connection-strings.md) (Строки подключения).

## <a name="create-the-database"></a>Создание базы данных

В следующих действиях используются [миграции](xref:core/managing-schemas/migrations/index) для создания базы данных.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* Последовательно выберите пункты **Средства > Диспетчер пакетов NuGet > Консоль диспетчера пакетов**.
* Выполните следующие команды:

  ```powershell
  Add-Migration InitialCreate
  Update-Database
  ```

  Если появляется сообщение об ошибке `The term 'add-migration' is not recognized as the name of a cmdlet`, закройте и снова откройте Visual Studio.

  Команда `Add-Migration` формирует шаблон миграции для создания начального набора таблиц в модели. Команда `Update-Database` создает базу данных и применяет к ней созданную миграцию.

# <a name="net-core-clitabnetcore-cli"></a>[Интерфейс командной строки .NET Core](#tab/netcore-cli)

* Выполните следующие команды:

  ```cli
  dotnet ef migrations add InitialCreate
  dotnet ef database update
  ```

  Команда `migrations` формирует шаблон миграции для создания начального набора таблиц в модели. Команда `database update` создает базу данных и применяет к ней созданную миграцию.

---

## <a name="create-a-controller"></a>Создание контроллера

Добавьте шаблон контроллера и представлений для сущности `Blog`.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* В **обозревателе решений** щелкните папку **Контроллеры** правой кнопкой мыши и выберите пункт **Добавить -> Контроллер**.
* Выберите **Контроллер MVC с представлениями, использующий Entity Framework** и щелкните **Добавить**.
* Для параметра **Класс модели** установите значение **Блог**, а для параметра **Класс контекста данных** — **BloggingContext**.
* Нажмите кнопку **Добавить**.

# <a name="net-core-clitabnetcore-cli"></a>[Интерфейс командной строки .NET Core](#tab/netcore-cli)

* Выполните следующие команды:

  ```cli
  dotnet tool install -g dotnet-aspnet-codegenerator
  dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
  dotnet restore
  dotnet aspnet-codegenerator controller -name BlogsController -m Blog -dc BloggingContext --relativeFolderPath Controllers --useDefaultLayout --referenceScriptLibraries
  ```

  Команды `tool install` и `add package` устанавливают средства, которые могут формировать контроллеры и представления. Команда `restore` удостоверяется, что все пакеты проекта загружены, а команда `aspnet-codegenerator` выполняет формирование.
---

Подсистема формирования шаблонов создает следующие файлы:

* файл контроллера (*Controllers/MoviesController.cs*);
* файлы представления Razor для страниц Create, Delete, Details, Edit и Index (_Views/Blogs/*.cshtml_).

## <a name="run-the-application"></a>Запуск приложения

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* **Отладка** > **Запуск без отладки**

# <a name="net-core-clitabnetcore-cli"></a>[Интерфейс командной строки .NET Core](#tab/netcore-cli)

```cli
dotnet run
```
---

* Перейдите к папке `/Blogs`.

* С помощью ссылки **Create New** (Создать новую запись) создайте несколько записей в блоге.

  ![Страница "Создать"](_static/create.png)

* Проверьте работу ссылок **Details**, **Edit** и **Delete**.

  ![Страница индексов](_static/index-new-db.png)

## <a name="additional-tutorials"></a>Дополнительные руководства

* [Начало работы с EF Core в консольном приложении .NET Core с новой базой данных](xref:core/get-started/netcore/new-db-sqlite)
* MVC ASP.NET Core:
  * [Начало работы с MVC ASP.NET Core](/aspnet/core/tutorials/first-mvc-app/start-mvc)
  * [Начало работы с EF Core в веб-приложении MVC ASP.NET](/aspnet/core/data/ef-mvc/intro)
* [Razor Pages](/aspnet/core/razor-pages/index):
  * [Начало работы с Razor Pages в ASP.NET Core](/aspnet/core/tutorials/razor-pages/razor-pages-start)
  * [Razor Pages с Entity Framework Core в ASP.NET Core](https://docs.microsoft.com/aspnet/core/data/ef-rp/intro)
