---
title: Новые возможности EF Core 5.0
description: Обзор новых возможностей в EF Core 5.0
author: ajcvickers
ms.date: 03/30/2020
uid: core/what-is-new/ef-core-5.0/whatsnew.md
ms.openlocfilehash: c902988920e3b1a6039808fe0658fc19dee2728a
ms.sourcegitcommit: 387cbd8109c0fc5ce6bdc85d0dec1aed72ad4c33
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/23/2020
ms.locfileid: "82103078"
---
# <a name="whats-new-in-ef-core-50"></a>Новые возможности EF Core 5.0

Сейчас EF Core 5.0 находится на стадии разработки.
На этой странице представлен обзор интересных изменений, появившихся в каждой предварительной версии.

Эта страница не является аналогом статьи о [планировании для EF Core 5.0](plan.md).
В плане описываются общие темы для EF Core 5.0, в том числе все, что планируется включить перед выпуском окончательной версии.

Мы будем добавлять ссылки на официальную документацию по мере ее публикации.

## <a name="preview-3"></a>Предварительная версия 3

### <a name="filtered-include"></a>Включение с фильтрацией

Метод Include теперь поддерживает фильтрацию включенных сущностей.
Пример:

```CSharp
var blogs = context.Blogs
    .Include(e => e.Posts.Where(p => p.Title.Contains("Cheese")))
    .ToList();
```

Этот запрос возвращает блоги со всеми связанными записями, но только в том случае, если заголовок записи содержит Cheese.

Для сокращения числа включаемых сущностей также можно использовать методы Skip и Take.
Пример:
 
```CSharp
var blogs = context.Blogs
    .Include(e => e.Posts.OrderByDescending(post => post.Title).Take(5)))
    .ToList();
```
Этот запрос возвратит блоги, включив не более пяти записей для каждого блога.

Подробные сведения см. в [документации по методу Include](xref:core/querying/related-data#filtered-include).

### <a name="new-modelbuilder-api-for-navigation-properties"></a>Новый API ModelBuilder для свойств навигации

Свойства навигации главным образом настраиваются при [определении связей](xref:core/modeling/relationships).
Однако новый метод `Navigation` можно использовать в случаях, когда свойства навигации требуют дополнительной настройки.
Например, чтобы задать резервное поле для навигации, если не удалось найти поле по соглашению:

```CSharp
modelBuilder.Entity<Blog>().Navigation(e => e.Posts).HasField("_myposts");
```

Обратите внимание, что API `Navigation` не является заменой для конфигурации связи.
Вместо этого он допускает дополнительную настройку свойств навигации в уже обнаруженных или определенных связях.

Документация отслеживается по проблеме [#2302](https://github.com/dotnet/EntityFramework.Docs/issues/2302).

### <a name="new-command-line-parameters-for-namespaces-and-connection-strings"></a>Новые параметры командной строки для пространств имен и строк подключения 

Теперь можно указывать пространства имен в командной строке во время миграции и формирования шаблонов.
Это можно сделать, например, затем, чтобы реконструировать базу данных, поместив в нее классы контекста и модели в различных пространствах имен: 

```
dotnet ef dbcontext scaffold "connection string" Microsoft.EntityFrameworkCore.SqlServer --context-namespace "My.Context" --namespace "My.Model"
```

Кроме того, теперь можно передать строку подключения команде `database-update`.

```
dotnet ef database update --connection "connection string"
```

Аналогичные параметры также были добавлены в команды PowerShell, используемые в консоли диспетчера пакетов VS.

Документация отслеживается по проблеме [#2303](https://github.com/dotnet/EntityFramework.Docs/issues/2303).

### <a name="enabledetailederrors-has-returned"></a>Возвращение EnableDetailedErrors

По соображениям производительности EF не выполняет дополнительные проверки на NULL при считывании значений из базы данных.
Это может привести к возникновению исключений при обнаружении непредвиденных значений NULL, для которых будет сложно определить причину.

При использовании `EnableDetailedErrors` будет добавлена дополнительная проверка на NULL для запросов, чтобы облегчить определение причин этих ошибок за счет небольшого снижения производительности.  

Пример:
```CSharp
protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    => optionsBuilder
        .EnableDetailedErrors()
        .EnableSensitiveDataLogging() // Often also useful with EnableDetailedErrors 
        .UseSqlServer(Your.SqlServerConnectionString);
```

Документация отслеживается по проблеме [#955](https://github.com/dotnet/EntityFramework.Docs/issues/955).

### <a name="cosmos-partition-keys"></a>Ключи секций Cosmos

Теперь можно указать в запросе ключ секции, используемый для данного запроса.
Пример:

```CSharp
await context.Set<Customer>()
             .WithPartitionKey(myPartitionKey)
             .FirstAsync();
```

Документация отслеживается по проблеме [#2199](https://github.com/dotnet/EntityFramework.Docs/issues/2199).

### <a name="support-for-the-sql-server-datalength-function"></a>Поддержка функции DATALENGTH SQL Server

К ней можно обратиться с помощью нового метода `EF.Functions.DataLength`.
Пример:
```CSharp
var count = context.Orders.Count(c => 100 < EF.Functions.DataLength(c.OrderDate));
``` 

## <a name="preview-2"></a>Предварительная версия 2

### <a name="use-a-c-attribute-to-specify-a-property-backing-field"></a>Использование атрибута C# для указания резервного поля свойств

Теперь атрибут C# можно использовать для указания резервного поля для свойства.
Этот атрибут позволяет EF Core в обычном порядке выполнять запись и чтение из резервного поля, даже если резервное поле не может быть найдено автоматически.
Пример:

```CSharp
public class Blog
{
    private string _mainTitle;

    public int Id { get; set; }

    [BackingField(nameof(_mainTitle))]
    public string Title
    {
        get => _mainTitle;
        set => _mainTitle = value;
    }
}
```

Документация отслеживается по проблеме [#2230](https://github.com/dotnet/EntityFramework.Docs/issues/2230).

### <a name="complete-discriminator-mapping"></a>Завершение сопоставления дискриминатора

EF Core использует столбец дискриминатора для [сопоставления TPH в иерархии наследования](/ef/core/modeling/inheritance).
Некоторые улучшения производительности будут поддерживаться до тех пор, пока EF Core будут известны все возможные значения для дискриминатора.
Теперь эти улучшения реализованы в EF Core 5.0.

Например, в предыдущих версиях EF Core всегда создавался следующий код SQL для запроса, возвращающего все типы в иерархии:

```sql
SELECT [a].[Id], [a].[Discriminator], [a].[Name]
FROM [Animal] AS [a]
WHERE [a].[Discriminator] IN (N'Animal', N'Cat', N'Dog', N'Human')
```

Теперь при настройке полного сопоставления дискриминатора EF Core 5.0 будет формировать следующее:

```sql
SELECT [a].[Id], [a].[Discriminator], [a].[Name]
FROM [Animal] AS [a]
```

Начиная с предварительной версии 3 это поведение будет доступно по умолчанию.

### <a name="performance-improvements-in-microsoftdatasqlite"></a>Улучшения производительности в Microsoft.Data.Sqlite

В производительность SQLIte были внесены два улучшения:

* Теперь получение двоичных и строковых данных с помощью GetBytes, GetChars и GetTextReader осуществляется более эффективно за счет использования SqliteBlob и потоков.
* Инициализация SqliteConnection выполняется в отложенном режиме.

Эти улучшения добавлены в поставщик Microsoft.Data.Sqlite ADO.NET и, следовательно, позволяют повысить производительность за пределами EF Core.

## <a name="preview-1"></a>Предварительная версия 1

### <a name="simple-logging"></a>Простое ведение журнала

Эта функция добавляет возможность, аналогичную `Database.Log` в EF6.
Это значит, что она позволяет легко получать журналы из EF Core без настройки какой-либо внешней платформы ведения журналов.

Предварительная документация включена в раздел с описанием [еженедельного состояния EF на 5 декабря 2019 г](https://github.com/dotnet/efcore/issues/15403#issuecomment-562332863).

Дополнительная документация отслеживается по проблеме [#2085](https://github.com/dotnet/EntityFramework.Docs/issues/2085).

### <a name="simple-way-to-get-generated-sql"></a>Простой способ получения созданного кода SQL

В EF Core 5.0 представлен метод расширения `ToQueryString`, который будет возвращать код SQL, создаваемый в EF Core при выполнении запроса LINQ.

Предварительная документация включена в раздел с описанием [еженедельного состояния EF на 9 января 2020 г](https://github.com/dotnet/efcore/issues/19549#issuecomment-572823246).

Дополнительная документация отслеживается по проблеме [#1331](https://github.com/dotnet/EntityFramework.Docs/issues/1331).

### <a name="use-a-c-attribute-to-indicate-that-an-entity-has-no-key"></a>Использование атрибута C# для указания того, что сущность не имеет ключа

Теперь можно настроить тип сущности, не имеющей ключа, используя новый атрибут `KeylessAttribute`.
Пример:

```CSharp
[Keyless]
public class Address
{
    public string Street { get; set; }
    public string City { get; set; }
    public int Zip { get; set; }
}
```

Документация отслеживается по проблеме [#2186](https://github.com/dotnet/EntityFramework.Docs/issues/2186).

### <a name="connection-or-connection-string-can-be-changed-on-initialized-dbcontext"></a>Возможность изменения подключения или строки подключения в инициализированном DbContext

Теперь создание экземпляра DbContext без подключения или строки подключения стало еще проще.
Кроме того, подключение или строку подключения можно изменить в экземпляре контекста.
Эта возможность позволяет одному и тому же экземпляру контекста динамически подключаться к разным базам данных.

Документация отслеживается по проблеме [#2075](https://github.com/dotnet/EntityFramework.Docs/issues/2075).

### <a name="change-tracking-proxies"></a>Прокси-серверы отслеживания изменений

Теперь EF Core может создавать прокси-серверы среды выполнения, которые автоматически реализуют интерфейсы [INotifyPropertyChanging](https://docs.microsoft.com/dotnet/api/system.componentmodel.inotifypropertychanging?view=netcore-3.1) и [INotifyPropertyChanged](https://docs.microsoft.com/dotnet/api/system.componentmodel.inotifypropertychanged?view=netcore-3.1).
Затем они сообщают об изменениях значений свойств сущности напрямую в EF Core, что позволяет не выполнять проверку на наличие изменений.
Однако прокси-серверы имеют собственный набор ограничений, поэтому они предназначены не для всех.

Документация отслеживается по проблеме [#2076](https://github.com/dotnet/EntityFramework.Docs/issues/2076).

### <a name="enhanced-debug-views"></a>Улучшенные представления отладки

В представлениях отладки удобно просматривать внутренние компоненты EF Core при отладке.
Представление отладки для модели было реализовано некоторое время назад.
В EF Core 5.0 представление модели стало более удобочитаемым. Кроме того, добавлено новое представление отладки для отслеживаемых сущностей в диспетчере состояний.

Предварительная документация включена в раздел с описанием [еженедельного состояния EF на 12 декабря 2019 г](https://github.com/dotnet/efcore/issues/15403#issuecomment-565196206).

Дополнительная документация отслеживается по проблеме [#2086](https://github.com/dotnet/EntityFramework.Docs/issues/2086).

### <a name="improved-handling-of-database-null-semantics"></a>Улучшенная обработка семантики значений NULL в базе данных

Реляционные базы данных обычно обрабатывают NULL как неизвестное значение и, следовательно, как не равное любому другому значению NULL.
C# рассматривает NULL как определенное значение, которое аналогично любому другому значению NULL.
EF Core по умолчанию преобразует запросы так, чтобы они использовали семантику значений NULL C#.
В EF Core 5.0 эффективность таких преобразований значительно повышена.

Документация отслеживается по проблеме [#1612](https://github.com/dotnet/EntityFramework.Docs/issues/1612).

### <a name="indexer-properties"></a>Свойства индексатора

EF Core 5.0 поддерживает сопоставление свойств индексатора C#.
Эти свойства позволяют сущностям выступать в качестве контейнеров свойств, в которых столбцы сопоставляются с именованными свойствами в контейнере.

Документация отслеживается по проблеме [#2018](https://github.com/dotnet/EntityFramework.Docs/issues/2018).

### <a name="generation-of-check-constraints-for-enum-mappings"></a>Создание ограничений CHECK для сопоставлений перечислений

Функция миграции в EF Core 5.0 теперь может создавать ограничения CHECK для сопоставлений свойств перечисления.
Пример:

```SQL
MyEnumColumn VARCHAR(10) NOT NULL CHECK (MyEnumColumn IN ('Useful', 'Useless', 'Unknown'))
```

Документация отслеживается по проблеме [#2082](https://github.com/dotnet/EntityFramework.Docs/issues/2082).

### <a name="isrelational"></a>Является реляционным

В дополнение к существующим методам `IsSqlServer`, `IsSqlite` и `IsInMemory` был добавлен новый метод `IsRelational`.
Этот метод можно использовать для проверки того, использует ли DbContext какой-либо поставщик реляционной базы данных.
Пример:

```CSharp
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    if (Database.IsRelational())
    {
        // Do relational-specific model configuration.
    }
}
```

Документация отслеживается по проблеме [#2185](https://github.com/dotnet/EntityFramework.Docs/issues/2185).

### <a name="cosmos-optimistic-concurrency-with-etags"></a>Оптимистический параллелизм Cosmos с тегами ETag

Поставщик базы данных Azure Cosmos DB теперь поддерживает оптимистичный параллелизм с использованием тегов ETag.
Используйте построитель моделей в OnModelCreating для настройки ETag:

```CSharp
builder.Entity<Customer>().Property(c => c.ETag).IsEtagConcurrency();
```

Затем SaveChanges генерирует исключение `DbUpdateConcurrencyException` при конфликте параллелизма, которое [можно обработать](https://docs.microsoft.com/ef/core/saving/concurrency) для реализации повторных попыток и т. д.

Документация отслеживается по проблеме [#2099](https://github.com/dotnet/EntityFramework.Docs/issues/2099).

### <a name="query-translations-for-more-datetime-constructs"></a>Преобразования запросов для дополнительных конструкций DateTime

Теперь запросы, содержащие новую конструкцию DateTime, поддерживают преобразование.

Кроме того, теперь сопоставлены следующие функции SQL Server:

* DateDiffWeek
* DateFromParts

Пример:

```CSharp
var count = context.Orders.Count(c => date > EF.Functions.DateFromParts(DateTime.Now.Year, 12, 25));

```

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
