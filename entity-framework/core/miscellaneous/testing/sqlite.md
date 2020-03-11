---
title: Тестирование с помощью SQLite-EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 7a2b75e2-1875-4487-9877-feff0651b5a6
uid: core/miscellaneous/testing/sqlite
ms.openlocfilehash: f7f847d8c766c0d4d7577ea6760ee72a17f84933
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/06/2020
ms.locfileid: "78414635"
---
# <a name="testing-with-sqlite"></a>Тестирование с помощью SQLite

SQLite имеет режим в памяти, который позволяет использовать SQLite для записи тестов в реляционную базу данных без затрат на реальные операции с базой данных.

> [!TIP]  
> Вы можете просмотреть [Пример](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Testing) этой статьи на сайте GitHub.

## <a name="example-testing-scenario"></a>Пример сценария тестирования

Рассмотрим следующую службу, которая позволяет коду приложения выполнять некоторые операции, связанные с блогами. На внутреннем уровне используется `DbContext`, который подключается к базе данных SQL Server. Было бы полезно переключить этот контекст для подключения к базе данных SQLite в памяти, чтобы мы могли писать эффективные тесты для этой службы, не изменяя код, или выполнять массовую работу по созданию тестовой двойной части контекста.

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BlogService.cs)]

## <a name="get-your-context-ready"></a>Подготовка контекста

### <a name="avoid-configuring-two-database-providers"></a>Избегайте настройки двух поставщиков баз данных

В тестах вы собираетесь извне настроить контекст для использования поставщика памяти. При настройке поставщика базы данных путем переопределения `OnConfiguring` в контексте необходимо добавить некоторый условный код, чтобы убедиться, что поставщик базы данных настроен только в том случае, если он еще не настроен.

> [!TIP]  
> Если вы используете ASP.NET Core, этот код не нужен, так как поставщик базы данных настроен вне контекста (в Startup.cs).

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BloggingContext.cs#OnConfiguring)]

### <a name="add-a-constructor-for-testing"></a>Добавление конструктора для тестирования

Самый простой способ включить тестирование в другой базе данных — изменить контекст, чтобы предоставить конструктор, принимающий `DbContextOptions<TContext>`.

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BloggingContext.cs#Constructors)]

> [!TIP]  
> `DbContextOptions<TContext>` сообщает контексту все параметры, такие как база данных для подключения. Это тот же объект, который создается путем запуска метода onconfiguring в контексте.

## <a name="writing-tests"></a>Написание тестов

Ключом к тестированию с помощью этого поставщика является возможность указать контексту использовать SQLite и контролировать область базы данных в памяти. Областью действия базы данных управляет, открывая и закрывая соединение. Областью действия базы данных является длительность открытия соединения. Обычно требуется чистая база данных для каждого метода теста.

>[!TIP]
> Чтобы использовать `SqliteConnection()` и метод расширения `.UseSqlite()`, укажите ссылку на пакет NuGet [Microsoft. EntityFrameworkCore. SQLite](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Sqlite/).

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/TestProject/SQLite/BlogServiceTests.cs)]
