---
title: Перенос из EF6 в EF Core — перенос модели на основе EDMX — EF
description: Конкретные сведения о переносе приложения модели на основе Entity Framework 6 EDMX в Entity Framework Core
author: ajcvickers
ms.date: 10/27/2016
uid: efcore-and-ef6/porting/port-edmx
ms.openlocfilehash: 27b37ad1c2e3436ae96a71bc97e953763c48ee50
ms.sourcegitcommit: 0a25c03fa65ae6e0e0e3f66bac48d59eceb96a5a
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/14/2020
ms.locfileid: "92064267"
---
# <a name="porting-an-ef6-edmx-based-model-to-ef-core"></a>Перенос модели на основе EDMX EF6 в EF Core

EF Core не поддерживает формат файлов EDMX для моделей. Оптимальным вариантом для переноса этих моделей является создание новой модели на основе кода из базы данных для приложения.

## <a name="install-ef-core-nuget-packages"></a>Установка пакетов NuGet EF Core

Установите пакет NuGet `Microsoft.EntityFrameworkCore.Tools`.

## <a name="regenerate-the-model"></a>Повторное создание модели

Теперь можно использовать функцию реконструирования, чтобы создать модель на основе существующей базы данных.

Выполните ниже команду в консоли диспетчера пакетов ("Средства" -> "Диспетчер пакетов NuGet" -> "Консоль диспетчера пакетов"). В разделе [Консоль диспетчера пакетов (Visual Studio)](xref:core/miscellaneous/cli/powershell) приведены параметры команды для формирования шаблонов для подмножества таблиц и т. п.

```powershell
Scaffold-DbContext "<connection string>" <database provider name>
```

Например, ниже приведена команда для формирования шаблона модели из базы данных Blogging в экземпляре LocalDB SQL Server.

```powershell
Scaffold-DbContext "Server=(localdb)\mssqllocaldb;Database=Blogging;Trusted_Connection=True;" Microsoft.EntityFrameworkCore.SqlServer
```

## <a name="remove-ef6-model"></a>Удаление модели EF6

Теперь можно удалить модель EF6 из приложения.

Можно оставить установленный пакет NuGet EF6 (EntityFramework), так как EF Core и EF6 могут использоваться параллельно в одном приложении. Однако если вы не планируете использовать EF6 в каких-либо областях приложения, то удаление пакета поможет вывести ошибки компиляции для фрагментов кода, требующих внимания.

## <a name="update-your-code"></a>Обновление кода

На этом этапе осуществляется устранение ошибок компиляции и проверка кода, чтобы узнать, затронут ли вас изменения поведения между EF6 и EF Core.

## <a name="test-the-port"></a>Тестирование переноса

Даже если приложение компилируется, это не означает, что оно успешно перенесено в EF Core. Необходимо протестировать все области приложения, чтобы предотвратить отсутствие неблагоприятного воздействия каких-либо изменений поведения.
