---
title: Пример EF Core тестирования — EF Core
description: Пример, демонстрирующий тестирование приложений, использующих Entity Framework Core
author: ajcvickers
ms.date: 04/22/2020
uid: core/testing/testing-sample
no-loc:
- Item
- Tag
- Items
- Tags
- items
- tags
ms.openlocfilehash: a1c858333ee8ebe5a0ad82358c518c86576cdd32
ms.sourcegitcommit: 4798ab8d04c1fdbe6dd204d94d770fcbf309d09b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/11/2021
ms.locfileid: "103023462"
---
# <a name="ef-core-testing-sample"></a>Пример тестирования EF Core

> [!TIP]
> Код в этом документе можно найти на сайте GitHub в качестве [готового к запуску примера](https://github.com/dotnet/EntityFramework.Docs/tree/main/samples/core/Miscellaneous/Testing/ItemsWebApi/).
> Обратите внимание, что некоторые из этих тестов должны **завершаться ошибкой**. Причины этого описаны ниже.

В этом документе рассматривается пример для тестирования кода, который использует EF Core.

## <a name="the-application"></a>Приложение

[Пример](https://github.com/dotnet/EntityFramework.Docs/tree/main/samples/core/Miscellaneous/Testing/ItemsWebApi/) содержит два проекта:

- ItemsWebApi: очень простой [веб-API, поддерживаемый ASP.NET Core](/aspnet/core/tutorials/first-web-api) с одним контроллером
- Тесты: тестовый проект [xUnit](https://xunit.net/) для тестирования контроллера

### <a name="the-model-and-business-rules"></a>Модель и бизнес-правила

Модель, которая включает этот API, имеет два типа сущностей: Items и Tags .

- Items зависят от регистра имя и коллекцию Tags .
- Каждый Tag имеет метку и число, представляющее количество применений к Item .
- Каждый Item должен иметь только один Tag с заданной меткой.
  - Если элемент помечен одной и той же меткой более одного раза, то число в существующем теге с этой меткой увеличивается вместо создания нового тега.
- Удаление Item должно удалить все связанные Tags .

#### <a name="the-item-entity-type"></a>ItemТип сущности

`Item`Тип сущности:

[!code-csharp[ItemEntityType](../../../samples/core/Miscellaneous/Testing/ItemsWebApi/ItemsWebApi/Item.cs?name=ItemEntityType)]

И его конфигурация в `DbContext.OnModelCreating` :

[!code-csharp[ConfigureItem](../../../samples/core/Miscellaneous/Testing/ItemsWebApi/ItemsWebApi/ItemsContext.cs?name=ConfigureItem)]

Обратите внимание, что тип сущности ограничивается способом, который можно использовать для отражения модели предметной области и бизнес-правил. В частности:

- Первичный ключ сопоставляется непосредственно с `_id` полем и не предоставляется публично
  - EF обнаруживает и использует закрытый конструктор, принимающий значение и имя первичного ключа.
- `Name`Свойство доступно только для чтения и задается только в конструкторе.
- Tags предоставляются в виде, `IReadOnlyList<Tag>` чтобы предотвратить произвольное изменение.
  - EF связывает `Tags` свойство с `_tags` резервным полем, сопоставляя их имена.
  - `AddTag`Метод принимает метку тега и реализует бизнес-правило, описанное выше.
    То есть, тег добавляется только для новых меток.
    В противном случае значение счетчика в существующей метке увеличивается.
- `Tags`Свойство навигации настроено для связи "многие к одному"
  - Нет необходимости в свойстве навигации из Tag Item , поэтому оно не включено.
  - Кроме того, не Tag определяет свойство внешнего ключа.
    Вместо этого EF будет создавать свойство в теневом состоянии и управлять им.

#### <a name="the-tag-entity-type"></a>TagТип сущности

`Tag`Тип сущности:

[!code-csharp[TagEntityType](../../../samples/core/Miscellaneous/Testing/ItemsWebApi/ItemsWebApi/Tag.cs?name=TagEntityType)]

И его конфигурация в `DbContext.OnModelCreating` :

[!code-csharp[ConfigureTag](../../../samples/core/Miscellaneous/Testing/ItemsWebApi/ItemsWebApi/ItemsContext.cs?name=ConfigureTag)]

Аналогично Item Tag скрывает его первичный ключ и делает `Label` свойство доступным только для чтения.

### <a name="the-itemscontroller"></a>ItemsКонтроллер

Контроллер веб-API довольно прост.
Он получает `DbContext` из контейнера внедрения зависимостей посредством внедрения конструктора:

[!code-csharp[Constructor](../../../samples/core/Miscellaneous/Testing/ItemsWebApi/ItemsWebApi/Controllers/ItemsController.cs?name=Constructor)]

Он имеет методы для получения всех Items или Item с заданным именем:

[!code-csharp[Get](../../../samples/core/Miscellaneous/Testing/ItemsWebApi/ItemsWebApi/Controllers/ItemsController.cs?name=Get)]

В нем имеется метод для добавления нового Item :

[!code-csharp[PostItem](../../../samples/core/Miscellaneous/Testing/ItemsWebApi/ItemsWebApi/Controllers/ItemsController.cs?name=PostItem)]

Метод для добавления метки к тегу Item с меткой:

[!code-csharp[PostTag](../../../samples/core/Miscellaneous/Testing/ItemsWebApi/ItemsWebApi/Controllers/ItemsController.cs?name=PostTag)]

И метод для удаления Item и все связанные Tags :

[!code-csharp[DeleteItem](../../../samples/core/Miscellaneous/Testing/ItemsWebApi/ItemsWebApi/Controllers/ItemsController.cs?name=DeleteItem)]

Большинство проверок и обработка ошибок были удалены для уменьшения перегруженности.

## <a name="the-tests"></a>Тесты

Тесты организованы для выполнения с несколькими конфигурациями поставщика базы данных:

- Поставщик SQL Server, который является поставщиком, используемым приложением.
- Поставщик SQLite
- Поставщик SQLite, использующий базы данных SQLite в памяти
- Поставщик базы данных EF в памяти

Это достигается путем размещения всех тестов в базовом классе и последующего наследования от него для тестирования с каждым поставщиком.

> [!TIP]
> Если вы не используете LocalDB, необходимо изменить строку подключения SQL Server.
> Рекомендации по использованию SQLite для тестирования в памяти см. в разделе [тестирование с помощью Sqlite](xref:core/testing/sqlite) .

Ожидается, что следующие два теста завершаются ошибкой:

- `Can_remove_item_and_all_associated_tags` При работе с поставщиком базы данных EF в памяти
- `Can_add_item_differing_only_by_case` При работе с поставщиком SQL Server

Более подробно эти сведения рассматриваются ниже.

### <a name="setting-up-and-seeding-the-database"></a>Настройка и заполнение базы данных

XUnit, как и большинство платформ тестирования, создаст новый экземпляр тестового класса для каждого тестового запуска.
Кроме того, XUnit не будет выполнять тесты внутри данного тестового класса параллельно.
Это означает, что мы можем установить и настроить базу данных в конструкторе тестов, и она будет находиться в известном состоянии для каждого теста.

> [!TIP]
> Этот пример повторно создает базу данных для каждого теста.
> Это хорошо подходит для тестирования SQLite и EF в памяти, но может включать значительные издержки, связанные с другими системами баз данных, включая SQL Server.
> Подходы к сокращению этих затрат рассматриваются в разделе [совместное использование баз данных в разных тестах](xref:core/testing/sharing-databases).

При выполнении каждого теста:

- DbContextOptions настроены для используемого поставщика и передаются конструктору базового класса
  - Эти параметры хранятся в свойстве и используются во всех тестах для создания экземпляров DbContext.
- Для создания и заполнения базы данных вызывается метод SEED
  - Метод SEED гарантирует, что база данных будет очищена, удалив ее, а затем повторно создав ее.
  - Некоторые хорошо известные тестовые сущности создаются и сохраняются в базе данных.

[!code-csharp[Seeding](../../../samples/core/Miscellaneous/Testing/ItemsWebApi/Tests/ItemsControllerTest.cs?name=Seeding)]

Затем каждый конкретный тестовый класс наследуется от этого.
Например:

[!code-csharp[SqliteItemsControllerTest](../../../samples/core/Miscellaneous/Testing/ItemsWebApi/Tests/SqliteItemsControllerTest.cs?name=SqliteItemsControllerTest)]

### <a name="test-structure"></a>Структура теста

Несмотря на то, что приложение использует внедрение зависимостей, тесты не имеют.
Здесь было бы неплохо использовать внедрение зависимостей, но дополнительный код, который он требует, имеет небольшую ценность.
Вместо этого DbContext создается с помощью `new` , а затем напрямую передается в качестве зависимости контроллеру.

Затем каждый тест выполняет тестируемый метод на контроллере и утверждает результаты ожидаемым образом.
Например:

[!code-csharp[CanGetItems](../../../samples/core/Miscellaneous/Testing/ItemsWebApi/Tests/ItemsControllerTest.cs?name=CanGetItems)]

Обратите внимание, что для заполнения базы данных и выполнения тестов используются различные экземпляры DbContext.
Это гарантирует, что тест не использует (или не переходит) сущности, которые отслеживает контекст при заполнении.
Он также лучше соответствует тому, что происходит в веб-приложениях и службах.

Тесты, изменяющие базу данных, создают второй экземпляр DbContext в тесте по тем же причинам.
То есть создание нового, чистого и последующего контекста, а затем чтение из базы данных, чтобы убедиться, что изменения были сохранены в базе данных.
Например:

[!code-csharp[CanAddItem](../../../samples/core/Miscellaneous/Testing/ItemsWebApi/Tests/ItemsControllerTest.cs?name=CanAddItem)]

Два еще более важных тестирования охватывают бизнес-логику, связанную с добавлением tags .

[!code-csharp[CanAddTag](../../../samples/core/Miscellaneous/Testing/ItemsWebApi/Tests/ItemsControllerTest.cs?name=CanAddTag)]

[!code-csharp[CanUpTagCount](../../../samples/core/Miscellaneous/Testing/ItemsWebApi/Tests/ItemsControllerTest.cs?name=CanUpTagCount)]

## <a name="issues-using-different-database-providers"></a>Проблемы с использованием разных поставщиков баз данных

Тестирование с использованием другой системы баз данных, чем используется в рабочем приложении, может привести к проблемам.
Они описаны на концептуальном уровне в [тестовом коде, который использует EF Core](xref:core/testing/index).
В следующих разделах рассматриваются два примера таких проблем, продемонстрированных в тестах в этом примере.

### <a name="test-passes-when-the-application-is-broken"></a>Тест проходит успешно при нарушении приложения

Одно из требований для нашего приложения заключается в том, что для этого используется Items имя с учетом регистра и коллекция Tags .
Это довольно просто для тестирования:

[!code-csharp[CanAddItemCaseInsensitive](../../../samples/core/Miscellaneous/Testing/ItemsWebApi/Tests/ItemsControllerTest.cs?name=CanAddItemCaseInsensitive)]

Выполнение этого теста для базы данных EF в памяти означает, что все правильно.
При использовании SQLite все еще выглядит нормально.
Но тест завершается ошибкой при запуске SQL Server!

```output
System.InvalidOperationException : Sequence contains more than one element
   at System.Linq.ThrowHelper.ThrowMoreThanOneElementException()
   at System.Linq.Enumerable.Single[TSource](IEnumerable`1 source)
   at Microsoft.EntityFrameworkCore.Query.Internal.QueryCompiler.Execute[TResult](Expression query)
   at Microsoft.EntityFrameworkCore.Query.Internal.EntityQueryProvider.Execute[TResult](Expression expression)
   at System.Linq.Queryable.Single[TSource](IQueryable`1 source, Expression`1 predicate)
   at Tests.ItemsControllerTest.Can_add_item_differing_only_by_case()
```

Это происходит потому, что по умолчанию учитывается регистр в базе данных EF в памяти и в базе данных SQLite.
SQL Server, с другой стороны, не учитывает регистр.

EF Core, по дизайну, не изменяет эти поведения, поскольку принудительное изменение чувствительности к регистру может существенно повлиять на производительность.

Когда мы понимаем, что это проблема, можно исправить приложение и компенсировать тесты.
Однако в этом случае эта ошибка может быть пропущена, если только тестирование выполняется с использованием в базе данных EF в памяти или поставщиков SQLite.

### <a name="test-fails-when-the-application-is-correct"></a>Проверка завершается ошибкой, если приложение является правильным

Другим требованием для нашего приложения является то, что "Удаление Item должно удалить все связанные Tags ".
Опять же, легко тестировать:

[!code-csharp[DeleteItem](../../../samples/core/Miscellaneous/Testing/ItemsWebApi/Tests/ItemsControllerTest.cs?name=DeleteItem)]

Этот тест передается в SQL Server и SQLite, но завершается сбоем с базой данных EF в памяти!

```output
Assert.False() Failure
Expected: False
Actual:   True
   at Tests.ItemsControllerTest.Can_remove_item_and_all_associated_tags()
```

В этом случае приложение работает правильно, так как SQL Server поддерживает [каскадное удаление](xref:core/saving/cascade-delete).
SQLite также поддерживает каскадное удаление, как и большинство реляционных баз данных, поэтому тестирование этого файла в SQLite работает.
С другой стороны, база данных EF в памяти [не поддерживает каскадное удаление](https://github.com/dotnet/efcore/issues/3924).
Это означает, что эта часть приложения не может быть протестирована с помощью поставщика базы данных EF в памяти.
