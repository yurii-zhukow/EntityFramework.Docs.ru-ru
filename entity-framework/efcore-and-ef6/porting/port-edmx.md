---
title: Перенос приложений из EF6 в EF Core — перенос модели на основе EDMX
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 63003709-f1ec-4bdc-8083-65a60c4826d2
uid: efcore-and-ef6/porting/port-edmx
ms.openlocfilehash: 2c3336ac675a830566001a0ddb3777839f52db18
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/27/2018
ms.locfileid: "42997415"
---
# <a name="porting-an-ef6-edmx-based-model-to-ef-core"></a>Перенос модель на основе EDMX EF6 в EF Core

EF Core не поддерживает формат файла EDMX для моделей. Перенос этих моделей, лучше создать новую модель на основе кода из базы данных для вашего приложения.

## <a name="install-ef-core-nuget-packages"></a>Установить пакеты EF Core NuGet

Установить `Microsoft.EntityFrameworkCore.Tools` пакет NuGet.

## <a name="regenerate-the-model"></a>Повторное создание модели

Теперь можно использовать функциональные возможности реконструирования, чтобы создать модель, основанную на существующей базы данных.

Выполните следующую команду в консоли диспетчера пакетов (средства –> Диспетчер пакетов NuGet –> консоль диспетчера пакетов). См. в разделе [консоль диспетчера пакетов (Visual Studio)](../../core/miscellaneous/cli/powershell.md) для параметры команд для формирования шаблонов подмножество таблиц и т.д.

``` powershell
Scaffold-DbContext "<connection string>" <database provider name>
```

Например вот команду, чтобы сформировать модель из базы данных блогов на локальном экземпляре SQL Server LocalDB.

``` powershell
Scaffold-DbContext "Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer
```

## <a name="remove-ef6-model"></a>Удаление модели EF6

Теперь вам нужно удалить модель EF6 из приложения.

Это можно оставить EF6 установленный пакет NuGet (EntityFramework), как EF Core и EF6 можно использовать side-by-side в одном приложении. Тем не менее если вы не собираетесь использовать EF6 в любой области приложения, то при удалении пакета поможет выдают ошибки компиляции на фрагменты кода, требуют внимания.

## <a name="update-your-code"></a>Обновление кода

На этом этапе это вопрос адресации ошибки компиляции и проверка кода ли изменения в работе между EF6 и EF Core повлияет на вас.

## <a name="test-the-port"></a>Протестировать порт

Компилирует приложение, не означает, что он успешно перенесена в EF Core. Необходимо будет проверить все области приложения, чтобы убедиться, что ни одно из изменений поведение негативно повлиять на приложения.

> [!TIP]
> См. в разделе [Приступая к работе с EF Core в ASP.NET Core с существующей базой данных](xref:core/get-started/aspnetcore/existing-db) для ссылки на дополнительные о том, как работать с существующей базой данных 
