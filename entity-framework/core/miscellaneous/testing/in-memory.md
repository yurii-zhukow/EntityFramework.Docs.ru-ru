---
title: Тестирование с помощью InMemory — EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 0d0590f1-1ea3-4d5c-8f44-db17395cd3f3
uid: core/miscellaneous/testing/in-memory
ms.openlocfilehash: 8aaea52f22954ef6a2b7d9b9c5627597c61ac644
ms.sourcegitcommit: 8f801993c9b8cd8a8fbfa7134818a8edca79e31a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/14/2019
ms.locfileid: "59562550"
---
# <a name="testing-with-inmemory"></a>Тестирование с помощью InMemory

Поставщик InMemory полезно в тех случаях, когда вы хотите протестировать компоненты, используя средство, имитирующее подключение к реальной базе данных, без издержек на фактических операций базы данных.

> [!TIP]  
> Для этой статьи вы можете скачать [пример](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Miscellaneous/Testing) из репозитория GitHub.

## <a name="inmemory-is-not-a-relational-database"></a>InMemory не является реляционной базы данных

Поставщики базы данных EF Core не быть реляционных баз данных. InMemory предназначен для базы данных общего назначения для тестирования и не предназначен для имитации реляционной базы данных.

Некоторые примеры включают:

* InMemory позволит вам сохранить данные, которые нарушают ограничения ссылочной целостности в реляционной базе данных.
* Если вы используете DefaultValueSql(string) для свойства в модели, это — это реляционная база данных API и не имеет смысла при работе с InMemory.
* [Параллелизм с помощью метки времени и строк версии](xref:core/modeling/concurrency#timestamprow-version) (`[Timestamp]` или `IsRowVersion`) не поддерживается. Не [DbUpdateConcurrencyException](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.dbupdateconcurrencyexception) возникает, если обновление выполняется с помощью старого маркера параллелизма.

> [!TIP]  
> Для многих тестовых целей эти различия будут имеет значения. Тем не менее, если вы хотите проверить то, что поведение во многом напоминает true реляционной базы данных, рассмотрите возможность использования [режиме in-memory SQLite](sqlite.md).

## <a name="example-testing-scenario"></a>Пример сценария тестирования

Предположим, что благодаря которой код приложения для выполнения некоторых операций, связанных с блоги. Внутри она использует `DbContext` , соединяется с базой данных SQL Server. Оно полезно для обмена данном контексте для подключения к базе данных InMemory, таким образом, мы может создавать эффективные тесты для этой службы без необходимости изменять код или выполнить большой объем работы по созданию теста double контекста.

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BlogService.cs)]

## <a name="get-your-context-ready"></a>Подготовка к контексту

### <a name="avoid-configuring-two-database-providers"></a>Не настраивать два поставщика базы данных

В тестах вы собираетесь извне настроить контекст, используемый поставщик InMemory. При настройке поставщика базы данных путем переопределения `OnConfiguring` в контексте, затем необходимо добавить некоторые условный код, чтобы убедиться, только настроить поставщик базы данных, если уже не был настроен.

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BloggingContext.cs#OnConfiguring)]

> [!TIP]  
> Если вы используете ASP.NET Core, то нет необходимости этот код так как поставщик базы данных уже настроена вне контекста (в файле Startup.cs).

### <a name="add-a-constructor-for-testing"></a>Добавьте конструктор для тестирования

Чтобы обеспечить возможность тестирования в другой базе данных проще всего изменить текущий контекст предоставляют конструктор, принимающий `DbContextOptions<TContext>`.

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/BusinessLogic/BloggingContext.cs#Constructors)]

> [!TIP]  
> `DbContextOptions<TContext>` Указывает контексту на все его параметры, например базу данных для подключения к. Это тот же объект, в которой создано, выполнив метод OnConfiguring в вашем контексте.

## <a name="writing-tests"></a>Написание тестов

Ключ для тестирования с этим поставщиком является возможность определить контекст для использования поставщика InMemory и управлять областью базы данных в памяти. Обычно требуется очистить базу данных для каждого метода теста.

Ниже приведен пример тестового класса, использующего базу данных InMemory. Каждый метод теста указывает уникальное имя базы данных, это означает, что каждый метод имеет свою собственную базу данных InMemory.

>[!TIP]
> Чтобы использовать `.UseInMemoryDatabase()` метод расширения, ссылка на пакет NuGet [Microsoft.EntityFrameworkCore.InMemory](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.InMemory/).

[!code-csharp[Main](../../../../samples/core/Miscellaneous/Testing/TestProject/InMemory/BlogServiceTests.cs)]
