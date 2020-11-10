---
title: Тестирование с помощью SQLite-EF Core
description: Использование SQLite для тестирования приложения Entity Framework Core
author: ajcvickers
ms.date: 04/24/2020
uid: core/testing/sqlite
ms.openlocfilehash: ebfcd36bf236cb83cab8683a8c31d4752d437998
ms.sourcegitcommit: f3512e3a98e685a3ba409c1d0157ce85cc390cf4
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2020
ms.locfileid: "94431375"
---
# <a name="using-sqlite-to-test-an-ef-core-application"></a>Использование SQLite для тестирования приложения EF Core

> [!WARNING]
> Использование SQLite может быть эффективным способом тестирования EF Core приложения.
> Однако могут возникнуть проблемы, когда SQLite ведет себя иначе из других систем баз данных.
> Описание проблем и компромиссов см. в разделе [тестирование кода, в котором используется EF Core](xref:core/testing/index) .  

В этом документе используются основные понятия, представленные в [примере, демонстрирующие тестирование приложений, использующих EF Core](xref:core/testing/testing-sample).
Приведенные здесь примеры кода взяты из этого примера.

## <a name="using-sqlite-in-memory-databases"></a>Использование SQLite в памяти баз данных

Как правило, SQLite создает базы данных в виде простых файлов и обращается к файлу внутри процесса с приложением.
Это очень быстро, особенно при использовании быстрого [SSD](https://en.wikipedia.org/wiki/Solid-state_drive).

SQLite также может использовать базы данных, созданные исключительно в памяти.
Это легко можно использовать с EF Core, если вы понимаете время существования базы данных в памяти:

* База данных создается при открытии подключения к ней
* База данных удаляется при закрытии соединения с ней

EF Core будет использовать уже открытое соединение при его наличии и никогда не попытается закрыть его.
Поэтому для использования EF Core с базой данных SQLite в памяти необходимо открыть подключение перед передачей его в EF.  

В [примере](xref:core/testing/testing-sample) это достигается с помощью следующего кода:

[!code-csharp[SqliteInMemory](../../../samples/core/Miscellaneous/Testing/ItemsWebApi/Tests/SqliteInMemoryItemsControllerTest.cs?name=SqliteInMemory)]

Примечание:

* `CreateInMemoryDatabase`Метод создает базу данных SQLite в памяти и открывает к ней подключение.
* Созданный объект `DbConnection` извлекается из `ContextOptions` и сохраняется.
* Соединение удаляется, когда тест удаляется, чтобы ресурсы не были утечки.

> [!NOTE]
> [Проблема #16103](https://github.com/dotnet/efcore/issues/16103) заключается в том, чтобы упростить управление подключениями.
