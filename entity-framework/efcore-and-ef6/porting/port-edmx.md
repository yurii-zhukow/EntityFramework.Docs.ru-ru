---
title: Перенос из EF6 в EF Core — перенос модели на основе EDMX — EF
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 63003709-f1ec-4bdc-8083-65a60c4826d2
uid: efcore-and-ef6/porting/port-edmx
ms.openlocfilehash: f0bb06dc687aaa774981d97daadc55f00fbd527e
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/06/2020
ms.locfileid: "78413531"
---
# <a name="porting-an-ef6-edmx-based-model-to-ef-core"></a>Перенос модели на основе EDMX EF6 в EF Core

EF Core не поддерживает формат файлов EDMX для моделей. Оптимальным вариантом для переноса этих моделей является создание новой модели на основе кода из базы данных для приложения.

## <a name="install-ef-core-nuget-packages"></a>Установка пакетов NuGet EF Core

Установите пакет NuGet `Microsoft.EntityFrameworkCore.Tools`.

## <a name="regenerate-the-model"></a>Повторное создание модели

Теперь можно использовать функцию реконструирования, чтобы создать модель на основе существующей базы данных.

Выполните ниже команду в консоли диспетчера пакетов ("Средства" -> "Диспетчер пакетов NuGet" -> "Консоль диспетчера пакетов"). В разделе [Консоль диспетчера пакетов (Visual Studio)](../../core/miscellaneous/cli/powershell.md) приведены параметры команды для формирования шаблонов для подмножества таблиц и т. п.

``` powershell
Scaffold-DbContext "<connection string>" <database provider name>
```

Например, ниже приведена команда для формирования шаблона модели из базы данных Blogging в экземпляре LocalDB SQL Server.

``` powershell
Scaffold-DbContext "Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer
```

## <a name="remove-ef6-model"></a>Удаление модели EF6

Теперь можно удалить модель EF6 из приложения.

Можно оставить установленный пакет NuGet EF6 (EntityFramework), так как EF Core и EF6 могут использоваться параллельно в одном приложении. Однако если вы не планируете использовать EF6 в каких-либо областях приложения, то удаление пакета поможет вывести ошибки компиляции для фрагментов кода, требующих внимания.

## <a name="update-your-code"></a>Обновление кода

На этом этапе осуществляется устранение ошибок компиляции и проверка кода, чтобы узнать, затронут ли вас изменения поведения между EF6 и EF Core.

## <a name="test-the-port"></a>Тестирование переноса

Даже если приложение компилируется, это не означает, что оно успешно перенесено в EF Core. Необходимо протестировать все области приложения, чтобы предотвратить отсутствие неблагоприятного воздействия каких-либо изменений поведения.
