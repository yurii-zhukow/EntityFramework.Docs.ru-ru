---
title: Выпуски Visual Studio — EF6
description: Выпуски Visual Studio и Entity Framework 6
author: divega
ms.date: 07/05/2018
uid: ef6/what-is-new/visual-studio
ms.openlocfilehash: 90982c49f47c8967cf3a3ed91fa51a392d94636f
ms.sourcegitcommit: abda0872f86eefeca191a9a11bfca976bc14468b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/14/2020
ms.locfileid: "90073634"
---
# <a name="visual-studio-releases"></a>Выпуски Visual Studio

Рекомендуется всегда использовать последнюю версию Visual Studio, поскольку она содержит новейшие средства для .NET, NuGet и Entity Framework.
На самом деле, в различных примерах и пошаговых руководствах по Entity Frameworkной документации предполагается, что вы используете последнюю версию Visual Studio.

Однако можно использовать более старые версии Visual Studio с разными версиями Entity Framework, если вы учитываете некоторые различия:

## <a name="visual-studio-2017-157-and-newer"></a>Visual Studio 2017 15,7 и более поздние версии

- Эта версия Visual Studio включает в себя последний выпуск Entity Framework инструментов и среду выполнения EF 6,2 и не требует дополнительных действий по установке.
Дополнительные сведения об этих выпусках см. в разделе [новые](xref:ef6/what-is-new/index) возможности.
- Добавление Entity Framework к новым проектам с помощью средств EF автоматически добавит пакет NuGet 6,2.
Вы можете вручную установить или обновить любой пакет EF NuGet, доступный в Интернете.
- По умолчанию экземпляр SQL Server, доступный в этой версии Visual Studio, является экземпляром LocalDB с именем MSSQLLocalDB.
В разделе "сервер" строки подключения следует использовать "(LocalDB) \\ MSSQLLocalDB".
`@` \\ \\ При указании строки подключения в коде C# следует использовать буквальную строку с префиксом или двойными символами обратной косой черты "".  


## <a name="visual-studio-2015-to-visual-studio-2017-156"></a>Visual Studio 2015 в Visual Studio 2017 15,6

- Эти версии Visual Studio включают Entity Framework средства и среду выполнения 6.1.3.
Дополнительные сведения об этих выпусках см. в разделе [предыдущие выпуски](xref:ef6/what-is-new/past-releases#ef-613) .
- Добавление Entity Framework в новые проекты с помощью средств EF автоматически добавит пакет NuGet 6.1.3.
Вы можете вручную установить или обновить любой пакет EF NuGet, доступный в Интернете.
- По умолчанию экземпляр SQL Server, доступный в этой версии Visual Studio, является экземпляром LocalDB с именем MSSQLLocalDB.
В разделе "сервер" строки подключения следует использовать "(LocalDB) \\ MSSQLLocalDB".
`@` \\ \\ При указании строки подключения в коде C# следует использовать буквальную строку с префиксом или двойными символами обратной косой черты "".  


## <a name="visual-studio-2013"></a>Visual Studio 2013
- Эта версия Visual Studio включает и более ранние версии Entity Framework средств и среды выполнения.
Рекомендуется выполнить обновление до Entity Framework Tools 6.1.3 с помощью [установщика](https://www.microsoft.com/download/details.aspx?id=40762) , доступного в центре загрузки Майкрософт.
Дополнительные сведения об этих выпусках см. в разделе [предыдущие выпуски](xref:ef6/what-is-new/past-releases#ef-613) .
- Добавление Entity Framework в новые проекты с помощью обновленных средств EF автоматически приведет к добавлению пакета NuGet 6.1.3.
Вы можете вручную установить или обновить любой пакет EF NuGet, доступный в Интернете.
- По умолчанию экземпляр SQL Server, доступный в этой версии Visual Studio, является экземпляром LocalDB с именем MSSQLLocalDB.
В разделе "сервер" строки подключения следует использовать "(LocalDB) \\ MSSQLLocalDB".
`@` \\ \\ При указании строки подключения в коде C# следует использовать буквальную строку с префиксом или двойными символами обратной косой черты "".  

## <a name="visual-studio-2012"></a>Visual Studio 2012

- Эта версия Visual Studio включает и более ранние версии Entity Framework средств и среды выполнения.
Рекомендуется выполнить обновление до Entity Framework Tools 6.1.3 с помощью [установщика](https://www.microsoft.com/download/details.aspx?id=40762) , доступного в центре загрузки Майкрософт.
Дополнительные сведения об этих выпусках см. в разделе [предыдущие выпуски](xref:ef6/what-is-new/past-releases#ef-613) .
- Добавление Entity Framework в новые проекты с помощью обновленных средств EF автоматически приведет к добавлению пакета NuGet 6.1.3.
Вы можете вручную установить или обновить любой пакет EF NuGet, доступный в Интернете.
- По умолчанию экземпляр SQL Server, доступный в этой версии Visual Studio, является экземпляром LocalDB с именем v 11.0.
В разделе "сервер" строки подключения следует использовать "(LocalDB) \\ v 11.0".
`@` \\ \\ При указании строки подключения в коде C# следует использовать буквальную строку с префиксом или двойными символами обратной косой черты "".  

## <a name="visual-studio-2010"></a>Visual Studio 2010

- Версия Entity Framework Tools, доступная в этой версии Visual Studio, несовместима с средой выполнения Entity Framework 6 и не может быть обновлена.
- По умолчанию средства Entity Framework добавляют в проекты Entity Framework 4,0.
Для создания приложений с использованием любых более новых версий EF необходимо сначала установить [расширение диспетчера пакетов NuGet](https://marketplace.visualstudio.com/items?itemName=NuGetTeam.NuGetPackageManager).
- По умолчанию все создание кода в версии средств EF основано на EntityObject и Entity Framework 4.
Рекомендуется переключить создание кода на основе DbContext и Entity Framework 5, установив шаблоны создания кода DbContext для [C#](https://marketplace.visualstudio.com/items?itemName=EntityFrameworkTeam.EF5xDbContextGeneratorforC) или [Visual Basic](https://marketplace.visualstudio.com/items?itemName=EntityFrameworkTeam.EF5xDbContextGeneratorforVBNET).
- После установки расширений диспетчера пакетов NuGet можно вручную установить или обновить любой пакет EF NuGet, доступный в сети, и использовать EF6 с Code First, для которого не требуется конструктор.
- По умолчанию экземпляр SQL Server, доступный в этой версии Visual Studio, SQL Server Express с именем SQLEXPRESS.
В разделе "сервер" строки подключения следует использовать ". \\ SQLEXPRESS ".
`@` \\ \\ При указании строки подключения в коде C# следует использовать буквальную строку с префиксом или двойными символами обратной косой черты "".
