---
title: Совместное использование баз данных между тестами — EF Core
description: Пример, демонстрирующий совместное использование базы данных несколькими тестами
author: ajcvickers
ms.date: 04/25/2020
uid: core/testing/sharing-databases
ms.openlocfilehash: 7a90a144271d5c34e9d5043aa439f84db805c6af
ms.sourcegitcommit: 032a1767d7a6e42052a005f660b80372c6521e7e
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/12/2021
ms.locfileid: "98128840"
---
# <a name="sharing-databases-between-tests"></a>Совместное использование баз данных в тестах

[Пример EF Core тестирования](xref:core/testing/testing-sample) показал, как тестировать приложения в разных системах баз данных.
Для этого примера каждый тест создал новую базу данных.
Это хороший шаблон при использовании SQLite или базы данных EF в памяти, но при использовании других систем баз данных может воздействовать значительная нагрузка.

Этот пример строится на предыдущем примере путем перемещения создания базы данных в средство тестирования.
Это позволяет создать одну SQL Server базу данных и заполнить ее только один раз для всех тестов.

> [!TIP]
> Перед продолжением работы ознакомьтесь с [примером EF Core тестирования](xref:core/testing/testing-sample) .

Писать несколько тестов для одной и той же базы данных несложно.
Хитрость заключается в том, что тесты не переносятся друг на друга по мере выполнения.
Для этого необходимо понимание:

* Безопасное совместное использование объектов между тестами
* Когда платформа тестирования выполняет тесты в параллельном режиме
* Сохранение базы данных в чистом состоянии для каждого теста

## <a name="the-fixture"></a>Основы

Мы будем использовать средство тестирования для совместного использования объектов между тестами.
В [документации xUnit](https://xunit.net/docs/shared-context.html) указано, что следует использовать средство, если необходимо создать один контекст теста и предоставить его для всех тестов в классе, а затем очистить его после завершения всех тестов в классе. "

> [!TIP]
> В этом примере используется [xUnit](https://xunit.net/), но аналогичные концепции существуют в других платформах тестирования, включая [NUnit](https://nunit.org/).

Это означает, что необходимо переместить создание и заполнение базы данных в класс основы.
Он выглядит следующим образом.

[!code-csharp[SharedDatabaseFixture](../../../samples/core/Miscellaneous/Testing/ItemsWebApi/SharedDatabaseTests/SharedDatabaseFixture.cs?name=SharedDatabaseFixture)]

Теперь обратите внимание на то, как конструктор:

* Создает одно подключение к базе данных в течение времени существования основы.
* Создает и заполняет эту базу данных путем вызова `Seed` метода.

Игнорировать блокировку сейчас; Мы вернемся к нему позже.

> [!TIP]
> Код создания и заполнения не обязательно должен быть асинхронным.
> Его асинхронное выполнение усложняет код и не повышает производительность и пропускную способность тестов.

База данных создается при первом удалении любой существующей базы данных, а затем при создании новой базы данных.
Это гарантирует, что база данных будет соответствовать текущей модели EF, даже если она была изменена с момента последнего тестового запуска.

> [!TIP]
> Можно быстрее «очищать» существующую базу данных, используя нечто вроде перезапуска [, а не](https://jimmybogard.com/tag/respawn/) повторно создавать ее каждый раз.
> Однако необходимо соблюдать осторожность, чтобы обеспечить актуальность схемы базы данных с помощью модели EF.

Подключение к базе данных удаляется при удалении основы.
На этом этапе также можно удалить тестовую базу данных.
Однако это потребует дополнительной блокировки и подсчета ссылок, если оборудование является общим для нескольких тестовых классов.
Кроме того, часто бывает полезно, чтобы тестовая база данных по-прежнему была доступна для отладки неудачных тестов.

## <a name="using-the-fixture"></a>Использование основы

XUnit имеет общий шаблон для связывания средства тестирования с классом тестов:

[!code-csharp[UsingTheFixture](../../../samples/core/Miscellaneous/Testing/ItemsWebApi/SharedDatabaseTests/SharedDatabaseTest.cs?name=UsingTheFixture)]

XUnit теперь создаст один экземпляр основы и передаст его каждому экземпляру тестового класса.
(Помните из первого [примера тестирования](xref:core/testing/testing-sample) , который xUnit создает новый экземпляр тестового класса при каждом выполнении теста.) Это означает, что база данных будет создана и заполнена один раз, а затем каждый тест будет использовать эту базу данных.

Обратите внимание, что тесты внутри одного класса не будут выполняться параллельно.
Это означает, что для каждого теста можно использовать одно и то же подключение к базе данных, даже если `DbConnection` объект не является потокобезопасным.

## <a name="maintaining-database-state"></a>Обслуживание состояния базы данных

Тесты часто требуют изменения тестовых данных с помощью вставок, обновлений и удалений.
Но эти изменения повлияют на другие тесты, в которых ожидается чистая заполненная база данных.

Это может быть вызвано выполнением изменения тестов внутри транзакции.
Пример:

[!code-csharp[CanAddItem](../../../samples/core/Miscellaneous/Testing/ItemsWebApi/SharedDatabaseTests/SharedDatabaseTest.cs?name=CanAddItem)]

Обратите внимание, что транзакция создается по мере начала и удаления теста по завершении.
Удаление транзакции приводит к ее откату, поэтому никакие изменения не будут видны другим тестам.

Вспомогательный метод для создания контекста (см. Приведенный выше код основы) принимает эту транзакцию и разрешает DbContext использовать ее.

## <a name="sharing-the-fixture"></a>Совместное использование основы

Возможно, вы обратили внимание на код блокировки создания и заполнения базы данных.
Это не требуется для этого примера, так как только один класс тестов использует средство, поэтому создается только один экземпляр основы.

Тем не менее может потребоваться использовать один и тот же набор с несколькими классами тестов.
XUnit создаст один экземпляр основы для каждого из этих классов.
Они могут использоваться различными потоками, выполняющими тесты параллельно.
Поэтому важно иметь подходящую блокировку, чтобы гарантировать создание и заполнение базы данных только одним потоком.

> [!TIP]
> `lock`Здесь очень просто.
> Нет необходимости предпринимать какие-либо более сложные действия, например шаблоны без блокировки.
