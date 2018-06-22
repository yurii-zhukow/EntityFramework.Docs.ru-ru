---
title: Тестирование с помощью SQLite - EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 7a2b75e2-1875-4487-9877-feff0651b5a6
ms.technology: entity-framework-core
uid: core/miscellaneous/testing/sqlite
ms.openlocfilehash: 8fcb4b1a37da6f52219ebe3c672160627fe28fab
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/27/2017
ms.locfileid: "26052704"
---
# <a name="testing-with-sqlite"></a>Тестирование с помощью SQLite

SQLite имеет режим в памяти, который можно использовать для написания тестов с реляционной базой данных, одновременно снижая издержки фактических операций базы данных SQLite.

> [!TIP]  
> Можно просмотреть в этой статье [пример](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Testing) на GitHub

## <a name="example-testing-scenario"></a>Пример сценария тестирования

Рассмотрим следующую службу, благодаря которой код приложения для выполнения некоторых операций, связанных с блоги. Внутренне используется `DbContext` , подключается к базе данных SQL Server. Оно полезно для замены этот контекст для подключения к базе данных SQLite в памяти, чтобы мы писать эффективный тесты для этой службы без необходимости изменения кода, либо выполнить большой объем работы, чтобы создать тестовый двойные контекста.

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BlogService.cs)]

## <a name="get-your-context-ready"></a>Подготовьтесь к контексту

### <a name="avoid-configuring-two-database-providers"></a>Не настраивать двух поставщиков базы данных

В тестах вы собираетесь настроить контекст для использования поставщика InMemory извне. При настройке поставщика базы данных путем переопределения `OnConfiguring` в контексте текущего пользователя, затем необходимо добавить некоторые условного коде, чтобы гарантировать только Настройка базы данных поставщика, если уже не был настроен.

> [!TIP]  
> При использовании ASP.NET Core затем нет необходимости этот код так как настроен поставщик базы данных вне контекста (в файле Startup.cs).

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BloggingContext.cs#OnConfiguring)]

### <a name="add-a-constructor-for-testing"></a>Добавьте конструктор для тестирования

Самый простой способ тестирования в другой базе данных требуется изменить контекст для предоставления конструктор, принимающий `DbContextOptions<TContext>`.

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BloggingContext.cs#Constructors)]

> [!TIP]  
> `DbContextOptions<TContext>`Указывает контекст, все его параметры, такие как базы данных для подключения к. Это тот же объект, который создается при выполнении метода OnConfiguring в контексте текущего пользователя.

## <a name="writing-tests"></a>Написание тестов

Ключ для тестирования с этим поставщиком является возможность определить контекст SQLite и управлять областью базы данных в памяти. Открытие и закрытие соединения управляется области базы данных. Базы данных, ограничиваются продолжительность времени, которая при открытом соединении. Обычно требуется чистую базу данных для каждого метода теста.

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/TestProject/SQLite/BlogServiceTests.cs)]
