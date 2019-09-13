---
title: Установка Entity Framework Core
author: divega
ms.date: 08/06/2017
ms.assetid: 608cc774-c570-4809-8a3e-cd2c8446b8b2
uid: core/get-started/install/index
ms.openlocfilehash: 62194d1db4efcdaed53ca0e14f160315f8e3cf03
ms.sourcegitcommit: b2b9468de2cf930687f8b85c3ce54ff8c449f644
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/12/2019
ms.locfileid: "70921759"
---
# <a name="installing-entity-framework-core"></a>Установка Entity Framework Core

## <a name="prerequisites"></a>Предварительные требования

* EF Core — это библиотека [.NET Standard 2.0](/dotnet/standard/net-standard). Для запуска EF Core требуется реализация .NET, которая поддерживает .NET Standard 2.0. На EF Core также могут ссылаться другие библиотеки .NET Standard 2.0. 

* Например, EF Core можно использовать для разработки приложений, предназначенных для .NET Core. Для создания приложений .NET Core требуется [пакет SDK для .NET Core](https://dotnet.microsoft.com/download). При необходимости также можно использовать среду разработки, например Visual Studio, Visual Studio для Mac или Visual Studio Code. Дополнительные сведения см. в разделе [Приступая к работе с .NET Core](/dotnet/core/get-started).

* EF Core можно использовать для разработки приложений для .NET Framework 4.6.1 или более поздней версии в Windows с использованием Visual Studio. Рекомендуется использовать последнюю версию [Visual Studio](https://visualstudio.microsoft.com/vs). Если вы хотите использовать более старую версию, например Visual Studio 2015, [обновите клиент NuGet до версии 3.6.0](https://www.nuget.org/downloads) для работы с библиотеками .NET Standard 2.0.

* EF Core можно запустить в других реализациях .NET, таких как [Xamarin](https://dotnet.microsoft.com/apps/xamarin) и .NET Native. Но на практике у этих реализаций есть ограничения среды выполнения, которые могут повлиять на то, насколько хорошо EF Core работает в приложении. Дополнительные сведения см. в разделе [Реализации .NET, поддерживаемые EF Core](xref:core/platforms/index).

* Наконец, для различных поставщиков баз данных требуются различные версии ядер СУБД, реализаций .NET и операционных систем. Убедитесь, что доступен [поставщик базы данных EF Core](xref:core/providers/index), поддерживающий подходящую среду для вашего приложения.

## <a name="get-the-entity-framework-core-runtime"></a>Получение среды выполнения Entity Framework Core

Чтобы добавить EF Core в приложение, установите пакет NuGet для поставщика базы данных, который вы хотите использовать.

Если вы создаете приложение ASP.NET Core, устанавливать поставщики в памяти и поставщики SQL Server не нужно. Эти поставщики включены в текущие версии ASP.NET Core, а также в среду выполнения EF Core.  

Установить или обновить пакеты NuGet можно в интерфейсе командной строки (CLI) .NET Core, а также в диалоговом окне или консоли диспетчера пакетов Visual Studio.

### <a name="net-core-cli"></a>Интерфейс командной строки .NET Core

* Чтобы установить или обновить поставщик EF Core SQL Server, выполните следующую команду интерфейса командной строки .NET Core в командной строке операционной системы:

  ``` Console
  dotnet add package Microsoft.EntityFrameworkCore.SqlServer
  ```

* Вы можете указать в команде `dotnet add package` конкретную версию, используя модификатор `-v`. Например, чтобы установить пакеты EF Core 2.2.0, добавьте к команде `-v 2.2.0`.

Дополнительные сведения см. в разделе [Средства интерфейса командной строки (CLI) .NET](/dotnet/core/tools/).

### <a name="visual-studio-nuget-package-manager-dialog"></a>Диалоговое окно диспетчера пакетов NuGet в Visual Studio

* В меню Visual Studio выберите **"Проект" > "Управление пакетами NuGet"** .

* Нажмите кнопку **Обзор** или откройте вкладку **Обновления**.

* Чтобы установить или обновить поставщик SQL Server, выберите пакет `Microsoft.EntityFrameworkCore.SqlServer` и подтвердите свой выбор.

Дополнительные сведения: [Диалоговое окно диспетчера пакетов NuGet](/nuget/tools/package-manager-ui).

### <a name="visual-studio-nuget-package-manager-console"></a>Консоль диспетчера пакетов NuGet в Visual Studio

* В меню Visual Studio выберите **"Сервис" > "Диспетчер пакетов NuGet" > "Консоль диспетчера пакетов"** .

* Чтобы установить поставщик SQL Server, в консоли диспетчера пакетов выполните следующую команду:

  ``` PowerShell  
  Install-Package Microsoft.EntityFrameworkCore.SqlServer
  ```
* Чтобы обновить поставщик, используйте команду `Update-Package`.

* Чтобы указать конкретную версию, используйте модификатор `-Version`. Например, чтобы установить пакеты EF Core 2.2.0, добавьте в команды `-Version 2.2.0`.

Дополнительные сведения: [Консоль диспетчера пакетов](/nuget/tools/package-manager-console).

## <a name="get-the-entity-framework-core-tools"></a>Получение инструментов Entity Framework Core

Вы можете установить инструменты для выполнения задач, связанных с EF Core, в своем проекте, таких как создание и применение миграций базы данных, а также создание модели EF Core на основе существующей базы данных.

Доступно два набора инструментов:

* [Инструменты интерфейса командной строки (CLI)](xref:core/miscellaneous/cli/dotnet) .NET Core можно использовать в Windows, Linux и macOS. Эти команды начинаются с `dotnet ef`. 

* [Инструменты консоли диспетчера пакетов](xref:core/miscellaneous/cli/powershell) работают в Visual Studio в Windows. Эти команды начинаются с глагола, например `Add-Migration`, `Update-Database`.

Хотя вы можете использовать команды `dotnet ef` в консоли диспетчера пакетов, в Visual Studio рекомендуется использовать инструменты консоли диспетчера пакетов:

* Они автоматически работают для выбранного в PMC проекта в Visual Studio без переключения каталогов вручную.  

* Они автоматически открывают файлы, созданные командами в Visual Studio, после завершения соответствующей команды.

<a name="cli"></a>

### <a name="get-the-net-core-cli-tools"></a>Получение инструментов CLI для .NET Core

Для инструментов CLI .NET Core требуется пакет SDK для .NET Core, указанный ранее в разделе [Необходимые компоненты](#prerequisites).

Команды `dotnet ef` входят в текущие версии пакета SDK для .NET Core, но, чтобы включить их для конкретного проекта, нужно установить пакет `Microsoft.EntityFrameworkCore.Design`:

``` Console 
dotnet add package Microsoft.EntityFrameworkCore.Design 
``` 

Этот пакет автоматически включается в приложения ASP.NET Core.

> [!IMPORTANT]      
> Всегда используйте версии пакетов инструментов, которые соответствуют основному номеру версии для пакетов среды выполнения.

### <a name="get-the-package-manager-console-tools"></a>Получение инструментов консоли диспетчера пакетов

Чтобы получить инструменты консоли диспетчера пакетов для EF Core, установите пакет `Microsoft.EntityFrameworkCore.Tools`. Например, в Visual Studio:

``` PowerShell  
Install-Package Microsoft.EntityFrameworkCore.Tools
``` 

Этот пакет автоматически включается в приложения ASP.NET Core.

## <a name="upgrading-to-the-latest-ef-core"></a>Обновление до последней версии EF Core

* Каждый раз при выпуске новой версии EF Core мы также выпускаем новую версию поставщиков, которые входят в проект EF Core, например Microsoft.EntityFrameworkCore.SqlServer, Microsoft.EntityFrameworkCore.Sqlite и Microsoft.EntityFrameworkCore.InMemory. Кроме того, можно просто обновить поставщик до новой версии, чтобы получить все улучшения. 

* EF Core вместе с поставщиками SQL Server и поставщиками в памяти включены в текущие версии ASP.NET Core. Чтобы обновить существующее приложение до более новой версии EF Core, всегда обновляйте версию ASP.NET Core.

* Если требуется обновить приложение, использующее сторонний поставщик базы данных, всегда ищите обновление поставщика, совместимое с нужной вам версией EF Core. Например, поставщики баз данных для предыдущих версий несовместимы с версией 2.0 среды выполнения EF Core.

* Сторонние поставщики EF Core обычно не выпускают исправления вместе со средой выполнения EF Core. Чтобы обновить приложение, использующее сторонний поставщик, до версии исправления EF Core, может потребоваться добавить прямую ссылку на отдельные компоненты EF Core, такие как Microsoft.EntityFrameworkCore и Microsoft.EntityFrameworkCore.Relational.

* При обновлении существующего приложения до последней версии EF Core некоторые ссылки на более старые пакеты EF Core, возможно, потребуется удалить вручную:

  * Пакеты поставщиков баз данных, используемые во время разработки, например `Microsoft.EntityFrameworkCore.SqlServer.Design`, больше не требуются и не поддерживаются в EF Core 2.0 и более поздних версиях, но они не удаляются автоматически при обновлении других пакетов.

  * Инструменты .NET CLI включены в пакет SDK для .NET версии 2.1, поэтому ссылку на этот пакет можно удалить из файла проекта:

    ```
    <DotNetCliToolReference Include="Microsoft.EntityFrameworkCore.Tools.DotNet" Version="2.0.0" />
    ```

* Приложения, предназначенные для .NET Framework, может потребоваться изменить для работы с библиотеками .NET Standard 2.0:

  * Измените файл проекта и включите в группу начальной свойств следующую запись.

    ``` xml
    <AutoGenerateBindingRedirects>true</AutoGenerateBindingRedirects>
    ```

  * Для тестовых проектов также включите следующую запись.

    ``` xml
    <GenerateBindingRedirectsOutputType>true</GenerateBindingRedirectsOutputType>
    ```
