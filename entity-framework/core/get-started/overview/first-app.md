---
title: Начало работы — EF Core
description: Руководство. Начало работы с Entity Framework Core
author: rick-anderson
ms.date: 09/17/2019
uid: core/get-started/overview/first-app
ms.openlocfilehash: 560a6f293e32f4e1f75c13367291c4e31e83b157
ms.sourcegitcommit: 4798ab8d04c1fdbe6dd204d94d770fcbf309d09b
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/11/2021
ms.locfileid: "103024346"
---
# <a name="getting-started-with-ef-core"></a>Начало работы с EF Core

В этом руководстве вы создадите консольное приложение .NET Core, которое осуществляет доступ к базе данных SQLite с помощью Entity Framework Core.

Инструкции из этого руководства можно выполнять с помощью Visual Studio 2017 в Windows, а также .NET Core CLI в Windows, macOS или Linux.

[Пример для этой статьи на GitHub](https://github.com/dotnet/EntityFramework.Docs/tree/main/samples/core/GetStarted).

## <a name="prerequisites"></a>Предварительные требования

Установите следующее программное обеспечение:

### <a name="net-core-cli"></a>[Интерфейс командной строки .NET Core](#tab/netcore-cli)

* [пакет SDK для .NET Core](https://www.microsoft.com/net/download/core);

### <a name="visual-studio"></a>[Visual Studio](#tab/visual-studio)

* [Visual Studio 2019 16.3 или последующей версии](https://www.visualstudio.com/downloads/) с этой рабочей нагрузкой:
  * **Кроссплатформенная разработка .NET Core** (в разделе **Другие наборы инструментов**)

---

## <a name="create-a-new-project"></a>Создание нового проекта

### <a name="net-core-cli"></a>[Интерфейс командной строки .NET Core](#tab/netcore-cli)

```dotnetcli
dotnet new console -o EFGetStarted
cd EFGetStarted
```

### <a name="visual-studio"></a>[Visual Studio](#tab/visual-studio)

* Открытие Visual Studio
* Щелкните **Создать проект**.
* Выберите **Консольное приложение (.NET Core )** с тегом **C#** и щелкните **Далее**.
* Задайте проекту имя **EFGetStarted** и щелкните **Создать**.

---

## <a name="install-entity-framework-core"></a>Установка Entity Framework Core

Чтобы установить EF Core, установите пакеты целевых поставщиков базы данных EF Core, с которыми вы будете работать. В этом руководстве используется SQLite, так как он работает на всех платформах, поддерживаемых .NET Core. Список доступных поставщиков см. в разделе [Database Providers](xref:core/providers/index) (Поставщики базы данных).

### <a name="net-core-cli"></a>[Интерфейс командной строки .NET Core](#tab/netcore-cli)

```dotnetcli
dotnet add package Microsoft.EntityFrameworkCore.Sqlite
```

### <a name="visual-studio"></a>[Visual Studio](#tab/visual-studio)

* Последовательно выберите пункты **Средства > Диспетчер пакетов NuGet > Консоль диспетчера пакетов**.
* Выполните следующие команды:

  ```powershell
  Install-Package Microsoft.EntityFrameworkCore.Sqlite
  ```

Совет. Можно также установить пакеты, щелкнув проект правой кнопкой мыши и выбрав **Управление пакетами NuGet**.

---

## <a name="create-the-model"></a>Создание модели

Задайте класс контекста и классы сущностей, составляющие модель.

### <a name="net-core-cli"></a>[Интерфейс командной строки .NET Core](#tab/netcore-cli)

* В каталоге проекта создайте файл **Model.cs** с таким кодом:

### <a name="visual-studio"></a>[Visual Studio](#tab/visual-studio)

* Щелкните проект правой кнопкой мыши и выберите **Добавить > Класс**.
* Задайте имя **Model.cs** и щелкните **Добавить**.
* Замените все содержимое этого файла следующим кодом:

---

[!code-csharp[Main](../../../../samples/core/GetStarted/Model.cs)]

В EF Core также можно [реконструировать](xref:core/managing-schemas/scaffolding) модель из существующей базы данных.

Совет. Эта программа намеренно упрощена для наглядности. В приложениях для рабочей среды не следует хранить [строки подключений](xref:core/miscellaneous/connection-strings) в коде. Возможно, также будет лучше поместить каждый класс C# в отдельный файл.

## <a name="create-the-database"></a>Создание базы данных

В следующих действиях используются [миграции](xref:core/managing-schemas/migrations/index) для создания базы данных.

### <a name="net-core-cli"></a>[Интерфейс командной строки .NET Core](#tab/netcore-cli)

* Выполните следующие команды:

  ```dotnetcli
  dotnet tool install --global dotnet-ef
  dotnet add package Microsoft.EntityFrameworkCore.Design
  dotnet ef migrations add InitialCreate
  dotnet ef database update
  ```

  При этом устанавливается [dotnet ef](xref:core/cli/dotnet) и пакет конструктора, требуемый для выполнения команды в проекте. Команда `migrations` формирует шаблон миграции для создания начального набора таблиц в модели. Команда `database update` создает базу данных и применяет к ней созданную миграцию.

### <a name="visual-studio"></a>[Visual Studio](#tab/visual-studio)

* В **консоли диспетчера пакетов (PMC)** выполните следующие команды:

  ```powershell
  Install-Package Microsoft.EntityFrameworkCore.Tools
  Add-Migration InitialCreate
  Update-Database
  ```

  При этом устанавливаются [средства консоли диспетчера пакетов для EF Core](xref:core/cli/powershell). Команда `Add-Migration` формирует шаблон миграции для создания начального набора таблиц в модели. Команда `Update-Database` создает базу данных и применяет к ней созданную миграцию.

---

## <a name="create-read-update--delete"></a>Создание, чтение, обновление и удаление

* Откройте файл *Program.cs* и замените его содержимое следующим кодом:

  [!code-csharp[Main](../../../../samples/core/GetStarted/Program.cs)]

## <a name="run-the-app"></a>Запуск приложения

### <a name="net-core-cli"></a>[Интерфейс командной строки .NET Core](#tab/netcore-cli)

```dotnetcli
dotnet run
```

### <a name="visual-studio"></a>[Visual Studio](#tab/visual-studio)

**"Отладка" > "Запустить без отладки"**

---

## <a name="next-steps"></a>Следующие шаги

* Следуйте инструкциям из [руководства по ASP.NET Core](/aspnet/core/data/ef-rp/intro), чтобы использовать EF Core в веб-приложении.
* См. сведения о [выражениях запросов LINQ](/dotnet/csharp/programming-guide/concepts/linq/basic-linq-query-operations).
* [Настройте модель](xref:core/modeling/index), указав [требуемую](xref:core/modeling/entity-properties#required-and-optional-properties) и [максимальную длину](xref:core/modeling/entity-properties#maximum-length).
* Используйте [миграции](xref:core/managing-schemas/migrations/index) для обновления схемы базы данных после изменения модели.
