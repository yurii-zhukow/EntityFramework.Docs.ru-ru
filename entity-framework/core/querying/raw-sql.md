---
title: "Необработанный SQL-запросы - EF Core"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 70aae9b5-8743-4557-9c5d-239f688bf418
ms.technology: entity-framework-core
uid: core/querying/raw-sql
ms.openlocfilehash: 79894c7b9fd9e40cdf14460abf5d872ee2f4b9f0
ms.sourcegitcommit: ced2637bf8cc5964c6daa6c7fcfce501bf9ef6e8
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/22/2017
---
# <a name="raw-sql-queries"></a>Необработанный SQL-запросов

Entity Framework Core можно перейти вниз необработанных запросов SQL, при работе с реляционной базой данных. Это полезно в том случае, если запрос, который требуется выполнить, не могут быть выражены с помощью LINQ, или с помощью запроса LINQ в результате чего неэффективным SQL, отправляемых в базе данных.

> [!TIP]  
> Для этой статьи вы можете скачать [пример](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) из репозитория GitHub.

## <a name="limitations"></a>Ограничения

Существует ряд ограничений, которые следует учитывать при использовании необработанных запросов SQL.
* Запросы SQL могут использоваться только для возврата типов сущностей, которые являются частью модели. Отсутствует расширение на нашем невыполненной работы к [enable возвращение специальных типов из необработанных запросов SQL](https://github.com/aspnet/EntityFramework/issues/1862).

* SQL-запрос должен возвращать данные для всех свойств типа сущности.

* Имена столбцов в результирующем наборе должны соответствовать имена столбцов, которые сопоставляются свойства. Обратите внимание, что это отличается от EF6, где сопоставления свойство столбец был пропущен в необработанные запросы SQL и пришлось имена совпадают с именами свойств столбца результирующего набора.

* SQL-запроса не может содержать связанные данные. Однако во многих случаях можно создать на основе запроса с помощью `Include` оператор для возврата связанных данных (в разделе [включая связанные данные](#including-related-data)).

* `SELECT`операторы, переданного этому методу обычно должны быть композицию: Если Core EF необходимо оценить дополнительных операторов запроса на сервере (например для преобразования применяются после операторов LINQ `FromSql`), предоставленный SQL будет рассматриваться как вложенный запрос. Это означает, что переданный SQL не должна содержать символов и параметры, которые не допускаются на вложенных запросах, такие как:
  * Конечная точка с запятой
  * На сервере SQL Server в конце уровне запроса Указание, например`OPTION (HASH JOIN)`
  * На сервере SQL Server `ORDER BY` предложение, которое не сопровождается из `TOP 100 PERCENT` в `SELECT` предложения

* Инструкции SQL, отличных от `SELECT` автоматически распознает как не допускающие композицию. Как следствие, полные результаты из хранимых процедур всегда возвращаются клиенту, и любые операторы LINQ применяется после `FromSql` вычисленное в памяти. 

## <a name="basic-raw-sql-queries"></a>Базовые необработанные запросы SQL

Можно использовать *FromSql* метод расширения, чтобы начать на языке LINQ на основе необработанных запросов SQL.

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var blogs = context.Blogs
    .FromSql("SELECT * FROM dbo.Blogs")
    .ToList();
```

Необработанные запросы SQL могут использоваться для выполнения хранимой процедуры.

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var blogs = context.Blogs
    .FromSql("EXECUTE dbo.GetMostPopularBlogs")
    .ToList();
```

## <a name="passing-parameters"></a>Передача параметров

Как и для любого API, который принимает SQL, важно параметризовать любой пользователь, входных данных для защиты от атак путем внедрения кода SQL. Можно местозаполнителями параметров в строке запроса SQL, а затем укажите значения параметров, как дополнительные аргументы. Значения всех параметров, указываемое будут автоматически преобразованы в `DbParameter`.

В следующем примере передается один параметр хранимой процедуры. Хотя это может выглядеть как `String.Format` синтаксис, предоставленного значения упаковывается в параметр "и" имя создаваемого параметра добавлены where `{0}` заполнитель был указан.

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var user = "johndoe";

var blogs = context.Blogs
    .FromSql("EXECUTE dbo.GetMostPopularBlogsForUser {0}", user)
    .ToList();
```

То же запрос, но с использованием интерполяции синтаксис строки, который поддерживается в EF Core 2.0 и более поздних версий:

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var user = "johndoe";

var blogs = context.Blogs
    .FromSql($"EXECUTE dbo.GetMostPopularBlogsForUser {user}")
    .ToList();
```

Можно также создать DbParameter и предоставление его в качестве значения параметра. Это позволяет использовать именованные параметры в строке запроса SQL

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var user = new SqlParameter("user", "johndoe");

var blogs = context.Blogs
    .FromSql("EXECUTE dbo.GetMostPopularBlogsForUser @user", user)
    .ToList();
```

## <a name="composing-with-linq"></a>Составление с помощью LINQ

Если в запросе SQL можно создавать на базы данных, можно составить поверх исходный необработанный SQL-запрос с использованием операторов LINQ. Запросы SQL, которые могут быть составлены на время с `SELECT` ключевое слово.

В следующем примере необработанные SQL-запроса, который выбирает из возвращающего табличное значение функции (TVF) и составляется с помощью LINQ для выполнения фильтрации и сортировки.

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var searchTerm = ".NET";

var blogs = context.Blogs
    .FromSql($"SELECT * FROM dbo.SearchBlogs({searchTerm})")
    .Where(b => b.Rating > 3)
    .OrderByDescending(b => b.Rating)
    .ToList();
```

### <a name="including-related-data"></a>Включая связанные данные

Составление с операторами LINQ можно использовать для включения связанных данных в запросе.

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var searchTerm = ".NET";

var blogs = context.Blogs
    .FromSql($"SELECT * FROM dbo.SearchBlogs({searchTerm})")
    .Include(b => b.Posts)
    .ToList();
```

> [!WARNING]  
> **Всегда используйте параметризации для необработанных запросов SQL:** интерфейсы API, принимающие необработанные SQL строка, таких как `FromSql` и `ExecuteSqlCommand` позволяют легко передавать в качестве параметров значения. В дополнение к проверке вводимых пользователем данных, для любого значения, используемые в необработанный команда SQL-запроса или всегда используйте параметризации. Если вы используете объединение строк динамически создавать любой части строки запроса, а затем вы несете ответственность за проверку всех входных данных для защиты от атак путем внедрения кода SQL.
