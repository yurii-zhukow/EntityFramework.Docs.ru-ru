---
title: "Тестирование с помощью InMemory - EF Core"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 0d0590f1-1ea3-4d5c-8f44-db17395cd3f3
ms.technology: entity-framework-core
uid: core/miscellaneous/testing/in-memory
ms.openlocfilehash: c5c48c575e9fd693d1f28d1a6d10eb83ebbc9d70
ms.sourcegitcommit: 860ec5d047342fbc4063a0de881c9861cc1f8813
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/05/2017
---
# <a name="testing-with-inmemory"></a>Тестирование с использованием памяти

Поставщик InMemory полезен в тех случаях, когда для проверки компонентов с помощью то, что приблизительно подключение к базе данных real, одновременно снижая издержки фактических операций базы данных.

> [!TIP]  
> Можно просмотреть в этой статье [пример](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Testing) на GitHub.

## <a name="inmemory-is-not-a-relational-database"></a>InMemory не является реляционной базы данных

Поставщики базы данных основной EF не обязательно реляционных баз данных. InMemory предназначен для базы данных общего назначения для тестирования и не предназначен для имитации реляционной базы данных.

Некоторые примеры включают:
* InMemory дает возможность сохранения данных, нарушающих ограничения ссылочной целостности в реляционной базе данных.

* Если вы используете DefaultValueSql(string) для свойства в модели, это — это реляционная база данных API и не будет действовать при выполнении для InMemory.

> [!TIP]  
> Многие теста для целей эти различия не будет иметь значения. Тем не менее, если вы хотите протестировать то, что во многом напоминает true реляционной базы данных, рассмотрите возможность использования [режиме in-memory SQLite](sqlite.md).

## <a name="example-testing-scenario"></a>Пример сценария тестирования

Рассмотрим следующую службу, благодаря которой код приложения для выполнения некоторых операций, связанных с блоги. Внутренне используется `DbContext` , подключается к базе данных SQL Server. Оно полезно для замены этот контекст для подключения к базе данных InMemory таким образом, мы писать эффективный тесты для этой службы без необходимости изменения кода или выполнить большой объем работы, чтобы создать тестовый двойные контекста.

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BlogService.cs)]

## <a name="get-your-context-ready"></a>Подготовьтесь к контексту

### <a name="avoid-configuring-two-database-providers"></a>Не настраивать двух поставщиков базы данных

В тестах вы собираетесь настроить контекст для использования поставщика InMemory извне. При настройке поставщика базы данных путем переопределения `OnConfiguring` в контексте текущего пользователя, затем необходимо добавить некоторые условного коде, чтобы гарантировать только Настройка базы данных поставщика, если уже не был настроен.

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BloggingContext.cs#OnConfiguring)]

> [!TIP]  
> При использовании ASP.NET Core затем нет необходимости этот код, так как поставщик базы данных уже настроены вне контекста (в файле Startup.cs).

### <a name="add-a-constructor-for-testing"></a>Добавьте конструктор для тестирования

Самый простой способ тестирования в другой базе данных требуется изменить контекст для предоставления конструктор, принимающий `DbContextOptions<TContext>`.

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BloggingContext.cs#Constructors)]

> [!TIP]  
> `DbContextOptions<TContext>`Указывает контекст, все его параметры, такие как базы данных для подключения к. Это тот же объект, который создается при выполнении метода OnConfiguring в контексте текущего пользователя.

## <a name="writing-tests"></a>Написание тестов

Ключ для тестирования с этим поставщиком является возможность определить контекст для использования поставщика InMemory и управлять областью базы данных в памяти. Обычно требуется чистую базу данных для каждого метода теста.

Ниже приведен пример тестового класса, использующего InMemory базы данных. Каждый метод теста указывает уникальное имя базы данных, это означает, что каждый метод имеет собственную базу данных в памяти.

>[!TIP]
> Для использования `.UseInMemoryDatabase()` метод расширения, ссылка пакета Nuget `Microsoft.EntityFrameworkCore.InMemory`.

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/TestProject/InMemory/BlogServiceTests.cs)]
