---
title: Новые возможности EF Core 5.0
author: ajcvickers
ms.date: 01/29/2020
uid: core/what-is-new/ef-core-5.0/whatsnew.md
ms.openlocfilehash: 65d7bd43e8a00c77fd6091a74c677635710d03e3
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/06/2020
ms.locfileid: "78413849"
---
# <a name="whats-new-in-ef-core-50"></a>Новые возможности EF Core 5.0

Сейчас EF Core 5.0 находится на стадии разработки.
На этой странице представлен обзор интересных изменений, появившихся в каждой предварительной версии.
Ожидается, что первая предварительная версия EF Core 5.0 выйдет в первом квартале 2020 г.

Эта страница не является аналогом статьи о [планировании для EF Core 5.0](plan.md).
В плане описываются общие темы для EF Core 5.0, в том числе все, что планируется включить перед выпуском окончательной версии.

Мы будем добавлять ссылки на официальную документацию по мере ее публикации.

## <a name="preview-1-not-yet-shipped"></a>Предварительная версия 1 (еще не выпущена)

### <a name="simple-logging"></a>Простое ведение журнала

Эта функция добавляет возможность, аналогичную `Database.Log` в EF6.
Это значит, что она позволяет легко получать журналы из EF Core без настройки какой-либо внешней платформы ведения журналов.

Предварительная документация включена в раздел с описанием [еженедельного состояния EF на 5 декабря 2019 г](https://github.com/dotnet/efcore/issues/15403#issuecomment-562332863).

Дополнительная документация отслеживается по проблеме [#2085](https://github.com/dotnet/EntityFramework.Docs/issues/2085).

### <a name="simple-way-to-get-generated-sql"></a>Простой способ получения созданного кода SQL

В EF Core 5.0 представлен метод расширения `ToQueryString`, который будет возвращать код SQL, создаваемый в EF Core при выполнении запроса LINQ.

Предварительная документация включена в раздел с описанием [еженедельного состояния EF на 9 января 2020 г](https://github.com/dotnet/efcore/issues/19549#issuecomment-572823246).

Дополнительная документация отслеживается по проблеме [#1331](https://github.com/dotnet/EntityFramework.Docs/issues/1331).

### <a name="enhanced-debug-views"></a>Улучшенные представления отладки

В представлениях отладки удобно просматривать внутренние компоненты EF Core при отладке.
Представление отладки для модели было реализовано некоторое время назад.
В EF Core 5.0 представление модели стало более удобочитаемым. Кроме того, добавлено новое представление отладки для отслеживаемых сущностей в диспетчере состояний.

Предварительная документация включена в раздел с описанием [еженедельного состояния EF на 12 декабря 2019 г](https://github.com/dotnet/efcore/issues/15403#issuecomment-565196206).

Дополнительная документация отслеживается по проблеме [#2086](https://github.com/dotnet/EntityFramework.Docs/issues/2086).

### <a name="connection-or-connection-string-can-be-changed-on-initialized-dbcontext"></a>Возможность изменения подключения или строки подключения в инициализированном DbContext

Теперь создание экземпляра DbContext без подключения или строки подключения стало еще проще.
Кроме того, подключение или строку подключения можно изменить в экземпляре контекста.
Это позволяет одному и тому же экземпляру контекста динамически подключаться к разным базам данных.

Документация отслеживается по проблеме [#2075](https://github.com/dotnet/EntityFramework.Docs/issues/2075).

### <a name="change-tracking-proxies"></a>Прокси-серверы отслеживания изменений

Теперь EF Core может создавать прокси-серверы среды выполнения, которые автоматически реализуют интерфейсы [INotifyPropertyChanging](https://docs.microsoft.com/dotnet/api/system.componentmodel.inotifypropertychanging?view=netcore-3.1) и [INotifyPropertyChanged](https://docs.microsoft.com/dotnet/api/system.componentmodel.inotifypropertychanged?view=netcore-3.1).
Затем они сообщают об изменениях значений свойств сущности напрямую в EF Core, что позволяет не выполнять проверку на наличие изменений.
Однако прокси-серверы имеют собственный набор ограничений, поэтому они предназначены не для всех.

Документация отслеживается по проблеме [#2076](https://github.com/dotnet/EntityFramework.Docs/issues/2076).

### <a name="improved-handling-of-database-null-semantics"></a>Улучшенная обработка семантики значений NULL в базе данных

Реляционные базы данных обычно обрабатывают NULL как неизвестное значение и, следовательно, как не равное любому другому значению NULL.
С другой стороны, C# рассматривает NULL как определенное значение, которое аналогично любому другому значению NULL.
EF Core по умолчанию преобразует запросы так, чтобы они использовали семантику значений NULL C#.
В EF Core 5.0 эффективность таких преобразований значительно повышена.

Документация отслеживается по проблеме [#1612](https://github.com/dotnet/EntityFramework.Docs/issues/1612).

### <a name="indexer-properties"></a>Свойства индексатора

EF Core 5.0 поддерживает сопоставление свойств индексатора C#.
Это позволяет сущностям выступать в качестве контейнеров свойств, в которых столбцы сопоставляются с именованными свойствами в контейнере.

Документация отслеживается по проблеме [#2018](https://github.com/dotnet/EntityFramework.Docs/issues/2018).

### <a name="generation-of-check-constraints-for-enum-mappings"></a>Создание ограничений CHECK для сопоставлений перечислений

Функция миграции в EF Core 5.0 теперь может создавать ограничения CHECK для сопоставлений свойств перечисления.
Пример:

```SQL
MyEnumColumn VARCHAR(10) NOT NULL CHECK (MyEnumColumn IN('Useful', 'Useless', 'Unknown'))
```

Документация отслеживается по проблеме [#2082](https://github.com/dotnet/EntityFramework.Docs/issues/2082).

### <a name="query-translations-for-more-datetime-constructs"></a>Преобразования запросов для дополнительных конструкций DateTime

Теперь запросы, содержащие новую конструкцию DateTime, поддерживают преобразование.
Кроме того, функция DateDiffWeek SQL Server является сопоставляемой.

Документация отслеживается по проблеме [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079).

### <a name="query-translations-for-more-byte-array-constructs"></a>Преобразования запросов для дополнительных конструкций массивов байтов

Запросы, использующие свойства Contains, Length, SequenceEqual и т. д. в массиве byte[], преобразуются в SQL.

Предварительная документация включена в раздел с описанием [еженедельного состояния EF на 5 декабря 2019 г](https://github.com/dotnet/efcore/issues/15403#issuecomment-562332863).

Дополнительная документация отслеживается по проблеме [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079).

### <a name="query-translation-for-reverse"></a>Преобразование запросов, использующих Reverse

Теперь можно преобразовывать запросы, использующие `Reverse`.
Пример:

```CSharp
context.Employees.OrderBy(e => e.EmployeeID).Reverse()
```

Документация отслеживается по проблеме [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079).

### <a name="query-translation-for-bitwise-operators"></a>Преобразование запросов для побитовых операторов

Запросы, использующие побитовые операторы, теперь преобразуются в ряде дополнительных случаев, например:

```CSharp
context.Orders.Where(o => ~o.OrderID == negatedId)
```

Документация отслеживается по проблеме [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079).

### <a name="query-translation-for-strings-on-cosmos"></a>Преобразование запросов для строк в Cosmos

Запросы, использующие строковые методы Contains, StartsWith и EndsWith, теперь преобразуются при использовании поставщика Azure Cosmos DB.

Документация отслеживается по проблеме [#2079](https://github.com/dotnet/EntityFramework.Docs/issues/2079).
