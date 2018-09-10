---
title: Установка Entity Framework Core
author: divega
ms.date: 08/06/2017
ms.assetid: 608cc774-c570-4809-8a3e-cd2c8446b8b2
uid: core/get-started/install/index
ms.openlocfilehash: 7831e6a54e885cf0b89ef3eef2cd81a9292df606
ms.sourcegitcommit: 0d36e8ff0892b7f034b765b15e041f375f88579a
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/09/2018
ms.locfileid: "44250326"
---
# <a name="installing-entity-framework-core"></a>Установка Entity Framework Core

## <a name="prerequisites"></a>Предварительные требования

* Для разработки приложений, предназначенных для .NET Core 2.1, установите [пакет SDK для .NET Core 2.1](https://www.microsoft.com/net/download/core). Это нужно сделать, даже если у вас установлена последняя версия Visual Studio 2017.

* Для разработки в Visual Studio приложений, предназначенных для .NET Core 2.1, установите Visual Studio 2017 версии 15.7 или новее.

* Использовать Entity Framework 2.1 можно в приложениях ASP.NET Core версии 2.1. Приложения с более ранними версиями ASP.NET Core нужно обновить до версии 2.1.

* Для разработки приложений, предназначенных для .NET Framework 4.6.1 и более поздних версий, можно использовать Visual Studio 2015. Однако вам нужна версия NuGet, поддерживающая .NET Standard 2.0 и совместимые среды. Чтобы получить ее в Visual Studio 2015, [обновите клиент NuGet до версии 3.6.0](https://www.nuget.org/downloads).

## <a name="get-the-entity-framework-core-runtime"></a>Получение среды выполнения Entity Framework Core

Чтобы добавить в приложение библиотеки среды выполнения EF Core, установите пакет NuGet для поставщика базы данных, который вы хотите использовать. Список поддерживаемых поставщиков и имена их пакетов NuGet см. в разделе [Поставщики баз данных](../../providers/index.md).

Установить или обновить пакеты NuGet можно в интерфейсе командной строки .NET Core, диалоговом окне диспетчера пакетов Visual Studio или консоли диспетчера пакетов Visual Studio.

Для приложений ASP.NET Core 2.1 поставщики в памяти и поставщики SQL Server включены автоматически и устанавливать их отдельно не требуется.

> [!TIP]  
> Если требуется обновить приложение, использующее сторонний поставщик базы данных, всегда ищите обновление поставщика, совместимое с нужной вам версией EF Core. Например, поставщики баз данных для предыдущих версий несовместимы с версией 2.1 среды выполнения EF Core.  

### <a name="net-core-cli"></a>Интерфейс командной строки .NET Core

Следующая команда интерфейса командной строки .NET Core устанавливает или обновляет поставщик SQL Server:

``` Console
dotnet add package Microsoft.EntityFrameworkCore.SqlServer
```

Вы можете указать в команде `dotnet add package` конкретную версию, используя модификатор `-v`. Например, чтобы установить пакеты EF Core 2.1.0, добавьте к команде `-v 2.1.0`.

### <a name="visual-studio-nuget-package-manager-dialog"></a>Диалоговое окно диспетчера пакетов NuGet в Visual Studio

* В меню выберите **Проект > Управление пакетами NuGet**

* Нажмите кнопку **Обзор** или откройте вкладку **Обновления**.

* Чтобы установить или обновить поставщик SQL Server, выберите пакет `Microsoft.EntityFrameworkCore.SqlServer` и подтвердите свой выбор.

Дополнительные сведения: [Диалоговое окно диспетчера пакетов NuGet](https://docs.microsoft.com/nuget/tools/package-manager-ui).

### <a name="visual-studio-nuget-package-manager-console"></a>Консоль диспетчера пакетов NuGet в Visual Studio

* В меню выберите **Сервис > Диспетчер пакетов NuGet > Консоль диспетчера пакетов**.

* Чтобы установить поставщик SQL Server, в консоли диспетчера пакетов выполните следующую команду:

  ``` PowerShell  
  Install-Package Microsoft.EntityFrameworkCore.SqlServer
  ```
* Чтобы обновить поставщик, используйте команду `Update-Package`.

* Чтобы указать конкретную версию, используйте модификатор `-Version`. Например, чтобы установить пакеты EF Core 2.1.0, добавьте в команды `-Version 2.1.0`

Дополнительные сведения: [Консоль диспетчера пакетов](https://docs.microsoft.com/nuget/tools/package-manager-console).

## <a name="get-entity-framework-core-tools"></a>Получение инструментов Entity Framework Core

Помимо библиотек среды выполнения, вы можете установить инструменты для выполнения в проекте определенных задач, связанных с EF Core, во время разработки. Например, вы можете создавать миграции, выполнять их и создавать модель на основе существующей базы данных.

Доступно два набора инструментов:
* [Инструменты интерфейса командной строки (CLI)](../../miscellaneous/cli/dotnet.md) .NET Core можно использовать в Windows, Linux и macOS. Эти команды начинаются с `dotnet ef`. 
* [Инструменты консоли диспетчера пакетов](../../miscellaneous/cli/powershell.md) работают в Visual Studio 2017 на Windows. Эти команды начинаются с глагола, например `Add-Migration`, `Update-Database`.

Хотя вы можете использовать команды `dotnet ef` в консоли диспетчера пакетов, в Visual Studio удобнее использовать инструменты консоли диспетчера пакетов:
* Они автоматически работают в текущем проекте, выбранном в консоли диспетчера пакетов, без необходимости ручного переключения каталогов.  
* Они автоматически открывают файлы, созданные командами в Visual Studio, после завершения соответствующей команды.

<a name="cli"></a>

### <a name="get-the-cli-tools"></a>Получение инструментов интерфейса командной строки

Команды `dotnet ef` входят в пакет SDK для .NET Core, но чтобы включить их, нужно установить пакет `Microsoft.EntityFrameworkCore.Design`:

 ``` Console    
dotnet add package Microsoft.EntityFrameworkCore.Design 
``` 

Этот пакет автоматически включается в приложения ASP.NET Core 2.1.

Как говорилось ранее в разделе [Предварительные требования](#prerequisites), вам также нужно установить пакет SDK для .NET Core 2.1.

> [!IMPORTANT]      
> Всегда используйте версии пакетов инструментов, которые соответствуют основному номеру версии для пакетов среды выполнения.

### <a name="get-the-package-manager-console-tools"></a>Получение инструментов консоли диспетчера пакетов

Чтобы получить инструменты консоли диспетчера пакетов для EF Core, установите пакет `Microsoft.EntityFrameworkCore.Tools`:

 ``` Console    
dotnet add package Microsoft.EntityFrameworkCore.Tools
``` 

Этот пакет автоматически включается в приложения ASP.NET Core 2.1.

## <a name="upgrading-to-ef-core-21"></a>Обновление до EF Core 2.1

При обновлении существующего приложения до EF Core 2.1 некоторые ссылки на более старые пакеты EF Core, возможно, потребуется удалить вручную:

* Пакеты поставщиков баз данных, используемые во время разработки, например `Microsoft.EntityFrameworkCore.SqlServer.Design`, больше не требуются и не поддерживаются в EF Core 2.1, но они не удаляются автоматически при обновлении других пакетов.

* Средства .NET CLI теперь включены в пакет SDK для .NET, следовательно ссылку на этот пакет можно удалить из файла *.csproj*:

  ```
  <DotNetCliToolReference Include="Microsoft.EntityFrameworkCore.Tools.DotNet" Version="2.0.0" />
  ```

Убедитесь, что приложения, предназначенные для .NET Framework и созданные в более ранних версиях Visual Studio, совместимы с библиотеками .NET Standard 2.0:

  * Измените файл проекта и включите в группу начальной свойств следующую запись.

    ``` xml
    <AutoGenerateBindingRedirects>true</AutoGenerateBindingRedirects>
    ```

  * Для тестовых проектов также включите следующую запись.

    ``` xml
    <GenerateBindingRedirectsOutputType>true</GenerateBindingRedirectsOutputType>
    ```
