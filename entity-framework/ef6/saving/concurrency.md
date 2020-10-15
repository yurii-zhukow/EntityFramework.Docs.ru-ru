---
title: Обработка конфликтов параллелизма — EF6
description: Обработка конфликтов параллелизма в Entity Framework 6
author: ajcvickers
ms.date: 10/23/2016
uid: ef6/saving/concurrency
ms.openlocfilehash: 0cec285ab6071120e162567506d397a23c299177
ms.sourcegitcommit: 0a25c03fa65ae6e0e0e3f66bac48d59eceb96a5a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/14/2020
ms.locfileid: "92064514"
---
# <a name="handling-concurrency-conflicts-ef6"></a>Обработка конфликтов параллелизма (EF6)

Оптимистическая блокировка подразумевает вызывает попытку сохранить сущность в базе данных, что не изменило данные с момента загрузки сущности. Если выяснилось, что данные изменились, возникает исключение и необходимо устранить конфликт перед повторным сохранением. В этом разделе описано, как управлять такими исключениями в Entity Framework. Методы, представленные в этом разделе, также применимы к моделям, созданным с помощью Code First и конструктора EF.  

Эта запись не является необходимым местом для полного обсуждения оптимистичного параллелизма. В следующих разделах предполагается наличие некоторых знаний о разрешении параллелизма и показаны закономерности для распространенных задач.  

Во многих из этих шаблонов используются разделы, обсуждаемые в разделе [Работа со значениями свойств](xref:ef6/saving/change-tracking/property-values).  

Разрешение проблем параллелизма при использовании независимых ассоциаций (когда внешний ключ не сопоставляется со свойством в сущности) гораздо сложнее, чем при использовании связей по внешнему ключу. Поэтому, если вы собираетесь выполнять разрешение параллелизма в приложении, рекомендуется всегда сопоставлять внешние ключи с сущностями. В приведенных ниже примерах предполагается, что вы используете связи по внешнему ключу.  

Метод SaveChanges вызывает исключение Дбупдатеконкурренциексцептион, когда при попытке сохранить сущность, использующую ассоциации внешних ключей, обнаруживается исключение оптимистичного параллелизма.  

## <a name="resolving-optimistic-concurrency-exceptions-with-reload-database-wins"></a>Разрешение исключений оптимистичного параллелизма с помощью перезагрузки (WINS-база данных)  

Метод Reload можно использовать для перезаписи текущих значений сущности значениями, которые теперь находятся в базе данных. Затем сущность, как правило, передается пользователю в некоторой форме и должен попытаться снова внести свои изменения и сохранить их повторно. Например.  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Find(1);
    blog.Name = "The New ADO.NET Blog";

    bool saveFailed;
    do
    {
        saveFailed = false;

        try
        {
            context.SaveChanges();
        }
        catch (DbUpdateConcurrencyException ex)
        {
            saveFailed = true;

            // Update the values of the entity that failed to save from the store
            ex.Entries.Single().Reload();
        }

    } while (saveFailed);
}
```  

Хорошим способом имитации исключения параллелизма является установка точки останова в вызове SaveChanges, а затем Изменение сущности, которая сохраняется в базе данных с помощью другого средства, такого как SQL Server Management Studio. Можно также вставить строку перед параметром SaveChanges, чтобы обновить базу данных непосредственно с помощью SqlCommand. Например.  

``` csharp
context.Database.SqlCommand(
    "UPDATE dbo.Blogs SET Name = 'Another Name' WHERE BlogId = 1");
```  

Метод записей в Дбупдатеконкурренциексцептион возвращает экземпляры Дбентитентри для сущностей, которые не удалось обновить. (В настоящее время это свойство всегда возвращает одно значение для проблем параллелизма. Он может возвращать несколько значений для общих исключений обновления.) Альтернативой в некоторых ситуациях может быть получение записей для всех сущностей, которые, возможно, потребуется перезагрузить из базы данных и вызвать перезагрузку для каждого из них.  

## <a name="resolving-optimistic-concurrency-exceptions-as-client-wins"></a>Разрешение исключений оптимистичного параллелизма в качестве клиента WINS  

Приведенный выше пример, использующий перезагрузку, иногда называется Database WINS или Store WINS, поскольку значения в сущности перезаписываются значениями из базы данных. Иногда может потребоваться выполнить противоположные действия и перезаписать значения в базе данных значениями, находящиеся в сущности. Это иногда называется клиентом WINS и может быть выполнено путем получения текущих значений базы данных и установки их в качестве исходных значений для сущности. (Сведения о текущих и исходных значениях см. в разделе [Работа со значениями свойств](xref:ef6/saving/change-tracking/property-values) .) Например:  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Find(1);
    blog.Name = "The New ADO.NET Blog";

    bool saveFailed;
    do
    {
        saveFailed = false;
        try
        {
            context.SaveChanges();
        }
        catch (DbUpdateConcurrencyException ex)
        {
            saveFailed = true;

            // Update original values from the database
            var entry = ex.Entries.Single();
            entry.OriginalValues.SetValues(entry.GetDatabaseValues());
        }

    } while (saveFailed);
}
```  

## <a name="custom-resolution-of-optimistic-concurrency-exceptions"></a>Пользовательское разрешение исключений оптимистичного параллелизма  

Иногда может потребоваться объединить значения, находящиеся в базе данных, с текущими значениями в сущности. Обычно это требует некоторой пользовательской логики или взаимодействия с пользователем. Например, пользователь может предоставить форму пользователю, содержащую текущие значения, значения в базе данных и набор разрешенных значений по умолчанию. При необходимости пользователь будет изменять разрешенные значения, и эти разрешенные значения будут сохранены в базе данных. Это можно сделать с помощью объектов DbPropertyValues, возвращаемых из Куррентвалуес и Жетдатабасевалуес, в записи сущности. Например.  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Find(1);
    blog.Name = "The New ADO.NET Blog";

    bool saveFailed;
    do
    {
        saveFailed = false;
        try
        {
            context.SaveChanges();
        }
        catch (DbUpdateConcurrencyException ex)
        {
            saveFailed = true;

            // Get the current entity values and the values in the database
            var entry = ex.Entries.Single();
            var currentValues = entry.CurrentValues;
            var databaseValues = entry.GetDatabaseValues();

            // Choose an initial set of resolved values. In this case we
            // make the default be the values currently in the database.
            var resolvedValues = databaseValues.Clone();

            // Have the user choose what the resolved values should be
            HaveUserResolveConcurrency(currentValues, databaseValues, resolvedValues);

            // Update the original values with the database values and
            // the current values with whatever the user choose.
            entry.OriginalValues.SetValues(databaseValues);
            entry.CurrentValues.SetValues(resolvedValues);
        }
    } while (saveFailed);
}

public void HaveUserResolveConcurrency(DbPropertyValues currentValues,
                                       DbPropertyValues databaseValues,
                                       DbPropertyValues resolvedValues)
{
    // Show the current, database, and resolved values to the user and have
    // them edit the resolved values to get the correct resolution.
}
```  

## <a name="custom-resolution-of-optimistic-concurrency-exceptions-using-objects"></a>Пользовательское разрешение исключений оптимистичного параллелизма с помощью объектов  

В приведенном выше коде используются экземпляры DbPropertyValues для передачи текущих, баз данных и разрешенных значений. Иногда для этого может оказаться проще использовать экземпляры типа сущности. Это можно сделать с помощью методов Тубжект и SetValue из DbPropertyValues. Пример:  

``` csharp
using (var context = new BloggingContext())
{
    var blog = context.Blogs.Find(1);
    blog.Name = "The New ADO.NET Blog";

    bool saveFailed;
    do
    {
        saveFailed = false;
        try
        {
            context.SaveChanges();
        }
        catch (DbUpdateConcurrencyException ex)
        {
            saveFailed = true;

            // Get the current entity values and the values in the database
            // as instances of the entity type
            var entry = ex.Entries.Single();
            var databaseValues = entry.GetDatabaseValues();
            var databaseValuesAsBlog = (Blog)databaseValues.ToObject();

            // Choose an initial set of resolved values. In this case we
            // make the default be the values currently in the database.
            var resolvedValuesAsBlog = (Blog)databaseValues.ToObject();

            // Have the user choose what the resolved values should be
            HaveUserResolveConcurrency((Blog)entry.Entity,
                                       databaseValuesAsBlog,
                                       resolvedValuesAsBlog);

            // Update the original values with the database values and
            // the current values with whatever the user choose.
            entry.OriginalValues.SetValues(databaseValues);
            entry.CurrentValues.SetValues(resolvedValuesAsBlog);
        }

    } while (saveFailed);
}

public void HaveUserResolveConcurrency(Blog entity,
                                       Blog databaseValues,
                                       Blog resolvedValues)
{
    // Show the current, database, and resolved values to the user and have
    // them update the resolved values to get the correct resolution.
}
```  
