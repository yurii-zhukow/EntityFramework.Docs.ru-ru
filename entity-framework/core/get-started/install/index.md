---
title: Установка EF Core
author: divega
ms.author: divega
ms.date: 08/06/2017
ms.assetid: 608cc774-c570-4809-8a3e-cd2c8446b8b2
ms.technology: entity-framework-core
uid: core/get-started/install/index
ms.openlocfilehash: 7bb2ee11940a4fd5736c7a23c16533ef53018f7b
ms.sourcegitcommit: bdd06c9a591ba5e6d6a3ec046c80de98f598f3f3
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/10/2018
ms.locfileid: "37949196"
---
# <a name="installing-ef-core"></a>Установка EF Core

## <a name="prerequisites"></a>Предварительные требования

Для разработки приложений .NET Core 2.0 (включая приложения ASP.NET Core 2.0, предназначенные для .NET Core) нужно скачать и установить подходящую версию [пакета SDK .NET Core 2.0](https://www.microsoft.com/net/download/core) для используемой платформы. **Это справедливо, даже если вы установили Visual Studio 2017 версии 15.3.**

Чтобы использовать EF Core 2.0 или любую другую библиотеку .NET Standard 2.0 с платформами .NET, кроме .NET Core 2.0 (например, с .NET Framework 4.6.1 или более поздней версии), вам понадобится версия NuGet, которая поддерживает .NET Standard 2.0 и ее совместимые платформы. Ниже приведено несколько способов, позволяющих получить ее.

* Установите Visual Studio 2017 версии 15.3.
* Если вы используете Visual Studio 2015, [скачайте и обновите клиент NuGet до версии 3.6.0](https://www.nuget.org/downloads).

Чтобы обеспечить совместимость проектов, созданных в предыдущих версиях Visual Studio и предназначенных для .NET Framework, с библиотеками NET Standard 2.0, в эти проекты может потребоваться внести некоторые изменения.

* Измените файл проекта и включите в группу начальной свойств следующую запись.
  ``` xml
  <AutoGenerateBindingRedirects>true</AutoGenerateBindingRedirects>
  ```

* Для тестовых проектов также включите следующую запись.
  ``` xml
  <GenerateBindingRedirectsOutputType>true</GenerateBindingRedirectsOutputType>
  ```

## <a name="getting-the-bits"></a>Получение битов
Для добавления библиотек времени выполнения EF Core в приложение рекомендуется установить поставщик базы данных EF Core из NuGet.

Кроме библиотек времени выполнения, можно установить средства, облегчающие выполнение нескольких задач, связанных с EF Core, во время разработки, таких как создание и применение миграций, а также создание модели на основе существующей базы данных.

> [!TIP]  
> Если требуется обновить приложение, использующее сторонний поставщик базы данных, всегда ищите обновление поставщика, совместимое с нужной вам версией EF Core. Например, поставщики баз данных для предыдущих версий несовместимы с версией 2.0 среды выполнения EF Core.  

> [!TIP]  
> Приложения, предназначенные для ASP.NET Core 2.0, могут использовать EF Core 2.0 без дополнительных зависимостей, кроме сторонних поставщиков базы данных. Приложения, предназначенные для предыдущих версий ASP.NET Core, нужно обновить до ASP.NET Core 2.0, чтобы они могли использовать EF Core 2.0.

<a name="cli"></a>
### <a name="cross-platform-development-using-the-net-core-command-line-interface-cli"></a>Кроссплатформенная разработка с помощью интерфейса командной строки (CLI) .NET Core

Для разработки приложений, предназначенных для [.NET Core](https://www.microsoft.com/net/download/core), можно использовать [команды CLI `dotnet`](https://docs.microsoft.com/dotnet/core/tools/) в сочетании с вашим любимым текстовым редактором или интегрированную среду разработки (IDE), такую как Visual Studio, Visual Studio для Mac или Visual Studio Code.

> [!IMPORTANT]  
> Для использования приложений, предназначенных для .NET Core, требуются определенные версии Visual Studio. Например, для разработки .NET Core 1.x требуется Visual Studio 2017, а для разработки .NET Core 2.0 — Visual Studio 2017 версии 15.3.

Чтобы установить или обновить поставщик SQL Server в кроссплатформенном приложении .NET Core, перейдите в каталог приложения и выполните следующую команду в командной строке.

``` Console
dotnet add package Microsoft.EntityFrameworkCore.SqlServer
```

Вы можете указать в команде `dotnet add package` установку конкретной версии, используя модификатор `-v`. Например, чтобы установить пакеты EF Core 2.0, включите `-v 2.0.0` в команду.

EF Core содержит набор [дополнительных команд для CLI `dotnet`](../../miscellaneous/cli/dotnet.md), начиная с `dotnet ef`. Чтобы использовать команды CLI `dotnet ef`, файл `.csproj` вашего приложения должен содержать следующую запись.

``` xml
<ItemGroup>
  <DotNetCliToolReference Include="Microsoft.EntityFrameworkCore.Tools.DotNet" Version="2.0.0" />
</ItemGroup>
```

Инструментам CLI .NET Core CLI для EF Core также требуется отдельный пакет — Microsoft.EntityFrameworkCore.Design. Вы можете просто добавить его в проект с помощью команды.

``` Console
dotnet add package Microsoft.EntityFrameworkCore.Design
```

> [!IMPORTANT]  
> Всегда используйте версии пакетов инструментов, которые соответствуют основному номеру версии для пакетов среды выполнения.

<a name="visual-studio"></a>
### <a name="visual-studio-development"></a>Разработка в Visual Studio

С помощью Visual Studio можно разрабатывать множество разных типов приложений, предназначенных для .NET Core, .NET Framework или других платформ, поддерживаемых EF Core.

Установить поставщик базы данных EF Core в своем приложении из Visual Studio можно двумя способами.

#### <a name="using-nugets-package-manager-user-interfacehttpsdocsmicrosoftcomnugettoolspackage-manager-ui"></a>Использование [пользовательского интерфейса диспетчера пакетов](https://docs.microsoft.com/nuget/tools/package-manager-ui) NuGet

* Выберите в меню пункты **Проект > Управление пакетами NuGet**

* Нажмите кнопку **Обзор** или откройте вкладку **Обновления**.

* Выберите пакет `Microsoft.EntityFrameworkCore.SqlServer` и нужную версию, а затем подтвердите операцию.

#### <a name="using-nugets-package-manager-console-pmchttpsdocsmicrosoftcomnugettoolspackage-manager-console"></a>Использование [консоли диспетчера пакетов (PMC)](https://docs.microsoft.com/nuget/tools/package-manager-console) NuGet

* В меню выберите пункты **"Сервис" > "Диспетчер пакетов NuGet" > "Консоль диспетчера пакетов"**.

* Введите и запустите следующую команду в PMC.

  ``` PowerShell  
  Install-Package Microsoft.EntityFrameworkCore.SqlServer
  ```
* Вместо этого можно использовать команду `Update-Package`, чтобы обновить уже установленный пакет до более новой версии.

* Чтобы указать определенную версию, можно использовать модификатор `-Version`. Например, чтобы установить пакеты EF Core 2.0, включите `-Version 2.0.0` в команды.

#### <a name="tools"></a>Инструменты

Кроме того, в Visual Studio существуют версии PowerShell для [команд EF Core, выполняемых внутри PMC,](../../miscellaneous/cli/powershell.md) возможности которых аналогичны командам `dotnet ef`. Чтобы использовать их, установите пакет `Microsoft.EntityFrameworkCore.Tools` с помощью пользовательского интерфейса диспетчера пакетов или консоли PMC.

> [!IMPORTANT]  
> Всегда используйте версии пакетов инструментов, которые соответствуют основному номеру версии для пакетов среды выполнения.

> [!TIP]  
> Хотя команды `dotnet ef` можно использовать из PMC в Visual Studio, гораздо удобнее использовать их версии PowerShell.
> * Они автоматически работают для выбранного в PMC проекта без ручного переключения каталогов.  
> * Они автоматически открывают файлы, созданные командами в Visual Studio, после завершения соответствующей команды.

> [!IMPORTANT]  
> **Нерекомендуемые пакеты в EF Core 2.0:** при обновлении существующего приложения до EF Core 2.0 некоторые ссылки на более старые пакеты EF Core может потребоваться удалить вручную. В частности, пакеты поставщиков баз данных во время разработки, например `Microsoft.EntityFrameworkCore.SqlServer.Design`, в EF Core 2.0 не требуется и не поддерживается, но они не удаляется автоматически при обновлении других пакетов.
