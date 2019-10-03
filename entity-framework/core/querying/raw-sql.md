---
title: Необработанные SQL-запросы — EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 70aae9b5-8743-4557-9c5d-239f688bf418
uid: core/querying/raw-sql
ms.openlocfilehash: d8f52edfdf4bd7776ab8d81185c867cbfd7bcf44
ms.sourcegitcommit: 6c28926a1e35e392b198a8729fc13c1c1968a27b
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/02/2019
ms.locfileid: "71813596"
---
# <a name="raw-sql-queries"></a>Необработанные SQL-запросы

Entity Framework Core позволяет вам переходить на уровень необработанных SQL-запросов при работе с реляционной базой данных. Это может быть полезно, если запрос, который вы хотите выполнить, не может быть выражен с помощью LINQ или если использование запроса LINQ приводит к отправке неэффективного SQL-запроса. Необработанные SQL-запросы могут возвращать обычные типы сущностей или [типы сущностей без ключей](xref:core/modeling/keyless-entity-types), которые являются частью модели.

> [!TIP]  
> Для этой статьи вы можете скачать [пример](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying/Querying/RawSQL/Sample.cs) из репозитория GitHub.

## <a name="basic-raw-sql-queries"></a>Основные необработанные SQL-запросы

Вы можете использовать метод расширения `FromSqlRaw`, чтобы начать запрос LINQ на основе необработанного SQL-запроса.

<!-- [!code-csharp[Main](samples/core/Querying/RawSQL/Sample.cs)] -->
``` csharp
var blogs = context.Blogs
    .FromSqlRaw("SELECT * FROM dbo.Blogs")
    .ToList();
```

Необработанные SQL-запросы могут использоваться для выполнения хранимой процедуры.

<!-- [!code-csharp[Main](samples/core/Querying/RawSQL/Sample.cs)] -->
``` csharp
var blogs = context.Blogs
    .FromSqlRaw("EXECUTE dbo.GetMostPopularBlogs")
    .ToList();
```

## <a name="passing-parameters"></a>Передача параметров

> [!WARNING]
> **Всегда используйте параметризацию для необработанных SQL-запросов.**
>
> При добавлении предоставленных пользователем значений в необработанный SQL-запрос необходимо соблюдать осторожность, чтобы не исключить атаки путем внедрения кода SQL. Помимо проверки отсутствия в этих значениях недопустимых символов, всегда следует использовать параметризацию, которая отправляет значения отдельно от текста SQL.
>
> В частности, никогда не передавайте объединенную или интерполированную строку (`$""`) с непроверенными предоставленными пользователем значениями в `FromSqlRaw` или `ExecuteSqlRaw`. Методы `FromSqlInterpolated` и `ExecuteSqlInterpolated` позволяют использовать синтаксис интерполяции строк таким способом, который обеспечивает защиту от атак путем внедрения кода SQL.

В следующем примере показана передача одного параметра в хранимую процедуру путем включения заполнителя параметра в строку SQL-запроса и предоставления дополнительного аргумента. Хотя это может выглядеть как синтаксис `String.Format`, предоставленное значение упаковано в `DbParameter` и созданное имя параметра вставляется вместо указанного заполнителя `{0}`.

<!-- [!code-csharp[Main](samples/core/Querying/RawSQL/Sample.cs)] -->
``` csharp
var user = "johndoe";

var blogs = context.Blogs
    .FromSqlRaw("EXECUTE dbo.GetMostPopularBlogsForUser {0}", user)
    .ToList();
```

В качестве альтернативы `FromSqlRaw` можно использовать `FromSqlInterpolated` для безопасного применения интерполяции строки. Как и в предыдущем примере, значение преобразуется в `DbParameter` и, следовательно, не является уязвимым к внедрению кода SQL.

> [!NOTE]
> До версии 3.0 `FromSqlRaw` и `FromSqlInterpolated` представляли собой две перегрузки `FromSql`. Дополнительные сведения см. в [разделе о предыдущих версиях](#previous-versions).

<!-- [!code-csharp[Main](samples/core/Querying/RawSQL/Sample.cs)] -->
``` csharp
var user = "johndoe";

var blogs = context.Blogs
    .FromSqlInterpolated($"EXECUTE dbo.GetMostPopularBlogsForUser {user}")
    .ToList();
```

Вы также можете создать DbParameter и указать его как значение параметра. Поскольку используется обычный заполнитель параметра SQL, а не заполнитель строки, можно применить `FromSqlRaw`.

<!-- [!code-csharp[Main](samples/core/Querying/RawSQL/Sample.cs)] -->
``` csharp
var user = new SqlParameter("user", "johndoe");

var blogs = context.Blogs
    .FromSqlRaw("EXECUTE dbo.GetMostPopularBlogsForUser @user", user)
    .ToList();
```

Благодаря этому можно использовать именованные параметры в строке SQL-запроса, что полезно, если у хранимой процедуры есть необязательные параметры.

<!-- [!code-csharp[Main](samples/core/Querying/RawSQL/Sample.cs)] -->
``` csharp
var user = new SqlParameter("user", "johndoe");

var blogs = context.Blogs
    .FromSqlRaw("EXECUTE dbo.GetMostPopularBlogs @filterByUser=@user", user)
    .ToList();
```

## <a name="composing-with-linq"></a>Создание с помощью LINQ

Если SQL-запрос может быть составлен в базе данных, вы можете использовать метод compose поверх исходного необработанного SQL-запроса с помощью операторов LINQ. SQL-запросы, поверх которых можно использовать метод compose, содержат ключевое слово `SELECT`.

В следующем примере используется необработанный SQL-запрос, который выбирает из функции с табличным значением (TVF), а затем выполняет компоновку на ее основе с использованием LINQ для выполнения фильтрации и сортировки.

<!-- [!code-csharp[Main](samples/core/Querying/RawSQL/Sample.cs)] -->
``` csharp
var searchTerm = ".NET";

var blogs = context.Blogs
    .FromSqlInterpolated($"SELECT * FROM dbo.SearchBlogs({searchTerm})")
    .Where(b => b.Rating > 3)
    .OrderByDescending(b => b.Rating)
    .ToList();
```

Будет создан следующий SQL-запрос:

``` sql
SELECT [b].[Id], [b].[Name], [b].[Rating]
        FROM (
            SELECT * FROM dbo.SearchBlogs(@p0)
        ) AS b
        WHERE b."Rating" > 3
        ORDER BY b."Rating" DESC
```

## <a name="change-tracking"></a>Отслеживание изменений

Запросы, использующие методы `FromSql`, следуют тем же правилам отслеживания изменений, что и любой другой запрос LINQ в EF Core. Например, если запрос проецирует типы сущностей, результаты будут отслеживаться по умолчанию.

В следующем примере используется необработанный SQL-запрос, который выбирает одну из функций с табличным значением, а затем отключает отслеживание изменений с помощью вызова `AsNoTracking`:

<!-- [!code-csharp[Main](samples/core/Querying/RawSQL/Sample.cs)] -->
``` csharp
var searchTerm = ".NET";

var blogs = context.Query<SearchBlogsDto>()
    .FromSqlInterpolated($"SELECT * FROM dbo.SearchBlogs({searchTerm})")
    .AsNoTracking()
    .ToList();
```

## <a name="including-related-data"></a>Включение связанных данных

Метод `Include` можно использовать для включения связанных данных, как с любым другим запросом LINQ:

<!-- [!code-csharp[Main](samples/core/Querying/RawSQL/Sample.cs)] -->
``` csharp
var searchTerm = ".NET";

var blogs = context.Blogs
    .FromSqlInterpolated($"SELECT * FROM dbo.SearchBlogs({searchTerm})")
    .Include(b => b.Posts)
    .ToList();
```

Обратите внимание, что для этого требуется необработанный SQL-запрос с поддержкой составления. В противном случае он не будет работать с вызовами хранимых процедур. Примечания о составлении см. в разделе [Ограничения](#limitations).

## <a name="limitations"></a>Ограничения

Есть несколько ограничений, которые следует учитывать при использовании необработанных SQL-запросов:

* SQL-запрос должен возвращать данные для всех свойств типа сущности.

* Имена столбцов в результирующем наборе должны совпадать с именами столбцов, с которыми сопоставляются свойства. Обратите внимание, что это отличается от EF6, где сопоставление свойств или столбцов игнорировалось для необработанных SQL-запросов, а имена столбцов в результирующем наборе должны были соответствовать именам свойств.

* SQL-запрос не может содержать связанные данные. Однако во многих случаях вы можете использовать метод compose поверх запроса с помощью оператора `Include` для возврата связанных данных (см. раздел [Включение связанных данных](#including-related-data)).

* Инструкции `SELECT`, переданные этому методу, как правило, должны быть составными: если EF Core вычисляет дополнительные операторы запросов на сервере (например, для преобразования операторов LINQ, применяемых после методов `FromSql`), передаваемый SQL-запрос будет рассматриваться как вложенный запрос. Это означает, что переданный SQL-запрос не должен содержать символы и параметры, которые не допускаются во вложенных запросах, такие как:
  * конечная точка с запятой;
  * на сервере SQL Server конечное указание на уровне запроса (например, `OPTION (HASH JOIN)`);
  * на сервере SQL Server предложение `ORDER BY`, которое не сопровождается `OFFSET 0` или `TOP 100 PERCENT` в предложении `SELECT`.

* Обратите внимание, что SQL Server не допускает составления вызовов хранимых процедур, поэтому любая попытка применить дополнительные операторы запроса к такому вызову приведет к формированию недопустимого SQL. Операторы запроса могут быть добавлены после `AsEnumerable()` для выполнения в клиенте.

## <a name="previous-versions"></a>Предыдущие версии

В EF Core версии 2.2 и более ранних версиях существовали две перегрузки `FromSql`, действие которых было аналогично поведению новых `FromSqlRaw` и `FromSqlInterpolated`. Это приводило к случайному вызову метода необработанной строки в тех случаях, когда требовалось вызвать метод интерполированной строки, и наоборот. Это может привести к тому, что запросы не параметризуются, хотя они должны параметризоваться.
