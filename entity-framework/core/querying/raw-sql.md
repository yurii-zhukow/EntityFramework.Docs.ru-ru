---
title: Необработанные SQL-запросы — EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 70aae9b5-8743-4557-9c5d-239f688bf418
uid: core/querying/raw-sql
ms.openlocfilehash: 91592ea9f7c73f10446993282c1874c852000871
ms.sourcegitcommit: c9c3e00c2d445b784423469838adc071a946e7c9
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/18/2019
ms.locfileid: "68306547"
---
# <a name="raw-sql-queries"></a>Необработанные SQL-запросы

Entity Framework Core позволяет вам переходить на уровень необработанных SQL-запросов при работе с реляционной базой данных. Это может быть полезно, если запрос, который вы хотите выполнить, не может быть выражен с помощью LINQ или если использование запроса LINQ приводит к отправке неэффективных SQL-запросов. Необработанные SQL-запросы могут возвращать типы сущностей или, начиная с EF Core 2.1, [типы запросов](xref:core/modeling/query-types), которые являются частью модели.

> [!TIP]  
> Для этой статьи вы можете скачать [пример](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Querying) из репозитория GitHub.

## <a name="basic-raw-sql-queries"></a>Основные необработанные SQL-запросы

Вы можете использовать метод расширения *FromSql*, чтобы начать запрос LINQ на основе необработанного SQL-запроса.

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var blogs = context.Blogs
    .FromSql("SELECT * FROM dbo.Blogs")
    .ToList();
```

Необработанные SQL-запросы могут использоваться для выполнения хранимой процедуры.

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var blogs = context.Blogs
    .FromSql("EXECUTE dbo.GetMostPopularBlogs")
    .ToList();
```

## <a name="passing-parameters"></a>Передача параметров

Как и для любого API, который принимает SQL, важно параметризовать любые входные данные пользователя для защиты от атак путем внедрения кода SQL. Вы можете включить заполнители параметров в строку SQL-запроса, а затем указать значения параметров в качестве дополнительных аргументов. Значения всех указанных параметров будут автоматически преобразованы в `DbParameter`.

В следующем примере в хранимую процедуру передается один параметр. Хотя это может выглядеть как синтаксис `String.Format`, предоставленное значение упаковано в параметр и созданное имя параметра вставляется вместо указанного заполнителя `{0}`.

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var user = "johndoe";

var blogs = context.Blogs
    .FromSql("EXECUTE dbo.GetMostPopularBlogsForUser {0}", user)
    .ToList();
```

Это тот же запрос, но с использованием синтаксиса интерполяции строк, который поддерживается в EF Core версии 2.0 и выше:

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var user = "johndoe";

var blogs = context.Blogs
    .FromSql($"EXECUTE dbo.GetMostPopularBlogsForUser {user}")
    .ToList();
```

Вы также можете создать объект DbParameter и указать его как значение параметра.

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var user = new SqlParameter("user", "johndoe");

var blogs = context.Blogs
    .FromSql("EXECUTE dbo.GetMostPopularBlogsForUser @user", user)
    .ToList();
```

Благодаря этому можно использовать именованные параметры в строке SQL-запроса, что полезно, если у хранимой процедуры есть необязательные параметры.

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var user = new SqlParameter("user", "johndoe");

var blogs = context.Blogs
    .FromSql("EXECUTE dbo.GetMostPopularBlogs @filterByUser=@user", user)
    .ToList();
```

## <a name="composing-with-linq"></a>Создание с помощью LINQ

Если SQL-запрос может быть составлен в базе данных, вы можете использовать метод compose поверх исходного необработанного SQL-запроса с помощью операторов LINQ. SQL-запросы, поверх которых можно использовать метод compose, содержат ключевое слово `SELECT`.

В следующем примере используется необработанный SQL-запрос, который выбирает из функции с табличным значением (TVF), а затем выполняет компоновку на ее основе с использованием LINQ для выполнения фильтрации и сортировки.

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var searchTerm = ".NET";

var blogs = context.Blogs
    .FromSql($"SELECT * FROM dbo.SearchBlogs({searchTerm})")
    .Where(b => b.Rating > 3)
    .OrderByDescending(b => b.Rating)
    .ToList();
```

## <a name="change-tracking"></a>Отслеживание изменений

Запросы, использующие `FromSql()`, следуют тем же правилам отслеживания изменений, что и любой другой запрос LINQ в EF Core. Например, если запрос проецирует типы сущностей, результаты будут отслеживаться по умолчанию.  

В следующем примере используется необработанный SQL-запрос, который выбирает одну из функций с табличным значением, а затем отключает отслеживание изменений с помощью вызова .AsNoTracking():

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var searchTerm = ".NET";

var blogs = context.Query<SearchBlogsDto>()
    .FromSql($"SELECT * FROM dbo.SearchBlogs({searchTerm})")
    .AsNoTracking()
    .ToList();
```

## <a name="including-related-data"></a>Включение связанных данных

Метод `Include()` можно использовать для включения связанных данных, как с любым другим запросом LINQ:

<!-- [!code-csharp[Main](samples/core/Querying/Querying/RawSQL/Sample.cs)] -->
``` csharp
var searchTerm = ".NET";

var blogs = context.Blogs
    .FromSql($"SELECT * FROM dbo.SearchBlogs({searchTerm})")
    .Include(b => b.Posts)
    .ToList();
```

## <a name="limitations"></a>Ограничения

Есть несколько ограничений, которые следует учитывать при использовании необработанных SQL-запросов:

* SQL-запрос должен возвращать данные для всех свойств сущности или типа запроса.

* Имена столбцов в результирующем наборе должны совпадать с именами столбцов, с которыми сопоставляются свойства. Обратите внимание, что это отличается от EF6, где сопоставление свойств или столбцов игнорировалось для необработанных SQL-запросов, а имена столбцов в результирующем наборе должны были соответствовать именам свойств.

* SQL-запрос не может содержать связанные данные. Однако во многих случаях вы можете использовать метод compose поверх запроса с помощью оператора `Include` для возврата связанных данных (см. раздел [Включение связанных данных](#including-related-data)).

* Инструкции `SELECT`, переданные этому методу, как правило, должны быть составными: если EF Core вычисляет дополнительные операторы запросов на сервере (например, для преобразования операторов LINQ, применяемых после `FromSql`), передаваемый SQL-запрос будет рассматриваться как вложенный запрос. Это означает, что переданный SQL-запрос не должен содержать символы и параметры, которые не допускаются во вложенных запросах, такие как:
  * конечная точка с запятой;
  * на сервере SQL Server конечное указание на уровне запроса (например, `OPTION (HASH JOIN)`);
  * на сервере SQL Server предложение `ORDER BY`, которое не сопровождается `OFFSET 0` или `TOP 100 PERCENT` в предложении `SELECT`.

* Операторы SQL, отличные от `SELECT`, автоматически распознаются как несоставные. Как следствие, полные результаты хранимых процедур всегда возвращаются клиенту, а любые операторы LINQ, применяемые после `FromSql`, вычисляются в памяти.

> [!WARNING]  
> **Всегда используйте параметризацию для необработанных SQL-запросов.** В дополнение к проверке вводимых пользователем данных всегда используйте параметры для любых значений, используемых в необработанном SQL-запросе или команде. API, которые принимают необработанную строку SQL, такую ​​как `FromSql` и `ExecuteSqlCommand`, позволяют легко передавать значения в качестве параметров. Перегрузки `FromSql` и `ExecuteSqlCommand`, принимающие FormattableString, также позволяют использовать синтаксис интерполяции строк, чтобы защититься от атак путем внедрения кода SQL. 
> 
> Если вы используете объединение или интерполяцию строк для динамической сборки любой части строки запроса либо передаете пользовательские входные данные в операторы или хранимые процедуры, способные выполнять эти данные в качестве динамического кода SQL, вам необходимо проверить все входные данные для защиты от атак путем внедрения кода SQL.
