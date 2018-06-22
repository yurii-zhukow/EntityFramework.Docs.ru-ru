---
title: Перенос приложений из EF6 в основные EF – перенос модель на основе EDMX
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 63003709-f1ec-4bdc-8083-65a60c4826d2
uid: efcore-and-ef6/porting/port-edmx
ms.openlocfilehash: c999d2114c76ee3a7615ae897b42ee3376cff14e
ms.sourcegitcommit: 507a40ed050fee957bcf8cf05f6e0ec8a3b1a363
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/26/2018
ms.locfileid: "31812694"
---
# <a name="porting-an-ef6-edmx-based-model-to-ef-core"></a>Перенос EF6 модели на основе EDMX EF ядра

EF Core не поддерживает формат файла EDMX для модели. Перенос этих моделей, лучше создать новую модель на основе кода из базы данных для приложения.

## <a name="install-ef-core-nuget-packages"></a>Установить пакеты EF Core NuGet

Установить `Microsoft.EntityFrameworkCore.Tools` пакет NuGet.

## <a name="regenerate-the-model"></a>Повторное создание модели

Чтобы создать модель, основанную на существующей базы данных теперь можно использовать функцию реконструирования.

Выполните следующую команду в консоли диспетчера пакетов (средства –> диспетчера пакетов NuGet –> консоль диспетчера пакетов). В разделе [Package Manager Console (Visual Studio)](../../core/miscellaneous/cli/powershell.md) для параметров команды для формировать подмножество таблиц и т. д.

``` powershell
Scaffold-DbContext "<connection string>" <database provider name>
```

Например вот команду, чтобы сформировать модель на основе базы данных ведения блогов на экземпляре SQL Server LocalDB.

``` powershell
Scaffold-DbContext "Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer
```

## <a name="remove-ef6-model"></a>Удаление модели EF6

Теперь EF6 модели будут удалены из приложения.

Это нормально оставить пакет EF6 NuGet (EntityFramework) установлен, как ядро EF и EF6 может быть используется side-by-side в одном приложении. Тем не менее если вы не собираетесь использовать EF6 в любой области приложения, затем при удалении пакета поможет выдают ошибки компиляции на элементы кода, требующие внимания.

## <a name="update-your-code"></a>Обновите ваш код

На этом этапе это вопрос адресации ошибки компиляции и проверка кода на предмет изменения в работе между EF6 и EF Core повлияет.

## <a name="test-the-port"></a>Проверить порт

Только потому, что компилирует приложения, не означает, что он успешно перенесена на EF Core. Необходимо для проверки всех областях приложения, чтобы убедиться, что никакие изменения в поведении отрицательно повлиять приложения.

> [!TIP]
> В разделе [Приступая к работе с EF Core в ASP.NET Core с существующей базы данных](xref:core/get-started/aspnetcore/existing-db) для ссылки на дополнительные по работе с существующей базой данных 
