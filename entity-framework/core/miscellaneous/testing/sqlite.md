---
title: Тестирование с помощью SQLite — EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 7a2b75e2-1875-4487-9877-feff0651b5a6
uid: core/miscellaneous/testing/sqlite
ms.openlocfilehash: bc9d6768a90ce17160c4126d2a68fddaa30d63de
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/27/2018
ms.locfileid: "42996872"
---
# <a name="testing-with-sqlite"></a>Тестирование с помощью SQLite

SQLite имеет режим в памяти, можно использовать для написания тестов с реляционной базой данных, без издержек на фактических операций базы данных SQLite.

> [!TIP]  
> Можно просмотреть в этой статье [пример](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Testing) на GitHub

## <a name="example-testing-scenario"></a>Пример сценария тестирования

Предположим, что благодаря которой код приложения для выполнения некоторых операций, связанных с блоги. Внутри она использует `DbContext` , соединяется с базой данных SQL Server. Оно полезно для обмена данном контексте для подключения к базе данных SQLite в памяти, таким образом, мы может создавать эффективные тесты для этой службы без необходимости изменять код или выполнить большой объем работы по созданию теста double контекста.

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BlogService.cs)]

## <a name="get-your-context-ready"></a>Подготовка к контексту

### <a name="avoid-configuring-two-database-providers"></a>Не настраивать два поставщика базы данных

В тестах вы собираетесь извне настроить контекст, используемый поставщик InMemory. При настройке поставщика базы данных путем переопределения `OnConfiguring` в контексте, затем необходимо добавить некоторые условный код, чтобы убедиться, только настроить поставщик базы данных, если уже не был настроен.

> [!TIP]  
> Если вы используете ASP.NET Core, то нет необходимости этот код с момента настройки поставщика базы данных вне контекста (в файле Startup.cs).

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BloggingContext.cs#OnConfiguring)]

### <a name="add-a-constructor-for-testing"></a>Добавьте конструктор для тестирования

Чтобы обеспечить возможность тестирования в другой базе данных проще всего изменить текущий контекст предоставляют конструктор, принимающий `DbContextOptions<TContext>`.

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BloggingContext.cs#Constructors)]

> [!TIP]  
> `DbContextOptions<TContext>` Указывает контексту на все его параметры, например базу данных для подключения к. Это тот же объект, в которой создано, выполнив метод OnConfiguring в вашем контексте.

## <a name="writing-tests"></a>Написание тестов

Ключ для тестирования с этим поставщиком является возможность определить контекст SQLite и управлять областью базы данных в памяти. Область базы данных управляется Открытие и закрытие соединения. Базы данных, ограничиваются длительность, при открытом соединении. Обычно требуется очистить базу данных для каждого метода теста.

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/TestProject/SQLite/BlogServiceTests.cs)]
