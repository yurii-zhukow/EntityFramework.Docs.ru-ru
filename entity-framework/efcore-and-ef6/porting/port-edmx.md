---
title: Перенос из EF6 в EF Core-перенос модели на основе EDMX — EF
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 63003709-f1ec-4bdc-8083-65a60c4826d2
uid: efcore-and-ef6/porting/port-edmx
ms.openlocfilehash: f0bb06dc687aaa774981d97daadc55f00fbd527e
ms.sourcegitcommit: 708b18520321c587b2046ad2ea9fa7c48aeebfe5
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/09/2019
ms.locfileid: "72182068"
---
# <a name="porting-an-ef6-edmx-based-model-to-ef-core"></a>Перенос модели на основе EF6 EDMX в EF Core

EF Core не поддерживает формат файлов EDMX для моделей. Лучшим вариантом для переноса этих моделей является создание новой модели на основе кода из базы данных для приложения.

## <a name="install-ef-core-nuget-packages"></a>Установка EF Core пакетов NuGet

Установите пакет NuGet `Microsoft.EntityFrameworkCore.Tools`.

## <a name="regenerate-the-model"></a>Повторное создание модели

Теперь можно использовать функцию реконструирования, чтобы создать модель на основе существующей базы данных.

В консоли диспетчера пакетов выполните следующую команду (Tools — > Диспетчер пакетов NuGet — > консоль диспетчера пакетов). Параметры команды для формирования шаблонов подмножества таблиц и т. д. см. в разделе [консоль диспетчера пакетов (Visual Studio)](../../core/miscellaneous/cli/powershell.md) .

``` powershell
Scaffold-DbContext "<connection string>" <database provider name>
```

Например, ниже приведена команда для формирования шаблона модели из базы данных блогов на SQL Server экземпляре LocalDB.

``` powershell
Scaffold-DbContext "Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer
```

## <a name="remove-ef6-model"></a>Удалить модель EF6

Теперь можно удалить модель EF6 из приложения.

Можно оставить установленный пакет NuGet EF6 (EntityFramework), так как EF Core и EF6 могут использоваться параллельно в одном приложении. Однако если вы не планируете использовать EF6 в каких бы то ни было областях приложения, то удаление пакета поможет предоставить ошибки компиляции для фрагментов кода, требующих внимания.

## <a name="update-your-code"></a>Обновление кода

На этом этапе необходимо устранить ошибки компиляции и просмотреть код, чтобы узнать, не повлияли ли изменения в поведении между EF6 и EF Core.

## <a name="test-the-port"></a>Проверка порта

Так как приложение компилируется, не означает, что оно успешно перенесено в EF Core. Необходимо протестировать все области приложения, чтобы предотвратить неблагоприятное воздействие каких-либо изменений в поведение приложения.
