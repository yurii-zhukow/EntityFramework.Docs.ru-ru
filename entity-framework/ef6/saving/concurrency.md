---
title: Обработка конфликтов параллелизма — EF6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: 2318e4d3-f561-4720-bbc3-921556806476
caps.latest.revision: 3
ms.openlocfilehash: b8608dbb4cadd60ff4ff4984583f8a9d843b3949
ms.sourcegitcommit: f05e7b62584cf228f17390bb086a61d505712e1b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/08/2018
ms.locfileid: "39121833"
---
# <a name="handling-concurrency-conflicts"></a>Обработка конфликтов параллелизма
Оптимистичный параллелизм предусматривает оптимистически попытка сохранить сущность в базу данных в надеюсь, что данные оттуда не изменялась после сущность была загружена. Если оказывается, что данные изменились, то возникает исключение, и необходимо разрешить конфликт, прежде чем сохранить еще раз. В этом разделе описывается, как для обработки таких исключений в Entity Framework. Методы, представленные в этом разделе, также применимы к моделям, созданным с помощью Code First и конструктора EF.  

Это сообщение не подходящим местом для полное описание оптимистичного параллелизма. В следующих разделах, предполагают, что некоторые разрешения параллелизма и отобразите шаблоны для распространенных задач.  

Многие из этих шаблонов использования темам, обсуждавшимся в [работа со значениями свойств](~/ef6/saving/change-tracking/property-values.md).  

Устранение проблем параллелизма, при использовании независимых сопоставлений (которой внешний ключ не сопоставлен со свойством сущности) гораздо сложнее, чем при использовании внешнего ключа ассоциации. Таким образом Если необходимо выполнить разрешение параллелизма в приложении рекомендуется всегда сопоставляются внешних ключей в сущности. Всех приведенных ниже примерах предполагается, что вы используете ассоциации внешних ключей.  

DbUpdateConcurrencyException генерируется SaveChanges при обнаружении исключения оптимистичного параллелизма при попытке сохранить сущность, которая использует связи внешнего ключа.  

## <a name="resolving-optimistic-concurrency-exceptions-with-reload-database-wins"></a>Разрешения исключений оптимистичного параллелизма с перезагрузить базы данных wins)  

Метод перезагрузки может использоваться для перезаписи текущих значений сущности со значениями, теперь в базе данных. Сущность обычно предоставляется обратно пользователю в определенной форме, и они пытаются снова внести свои изменения и повторно сохраните. Пример:  

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

Чтобы установить точку останова в вызове метода SaveChanges, а затем измените сущности, который сохраняется в базе данных с помощью другого средства, например SQL Management Studio является хорошим способом имитации исключения параллельности. Вы также может вставить строку перед SaveChanges для обновления базы данных, непосредственно с помощью SqlCommand. Пример:  

``` csharp
context.Database.SqlCommand(
    "UPDATE dbo.Blogs SET Name = 'Another Name' WHERE BlogId = 1");
```  

Метод записи на DbUpdateConcurrencyException возвращает dbentityentry, который должен экземпляры сущности, которые не удалось обновить. (Это свойство в настоящее время всегда возвращает одно значение для проблем параллелизма. Он может возвращать несколько значений для обновления общего исключения.) Альтернативы в некоторых случаях может быть получение записей для всех сущностей, которые может потребоваться перезагрузить из базы данных и перезагрузить вызова для каждого из них.  

## <a name="resolving-optimistic-concurrency-exceptions-as-client-wins"></a>Разрешения исключений оптимистичного параллелизма, как приоритет клиента  

Приведенном выше примере, использующий перезагрузить иногда называют базы данных wins или победы хранилища, так как значения в сущности, перезаписываются значениями из базы данных. Иногда может потребоваться сделать обратное и перезаписи значений базы данных с текущими значениями в сущности. Это иногда называется приоритет клиента и можно сделать, получив текущие значения базы данных и их настройке в качестве исходных значений для сущности. (См. в разделе [работа со значениями свойств](~/ef6/saving/change-tracking/property-values.md) сведения на текущие и исходные значения.) Пример:  

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

Иногда может потребоваться объединить значения, в настоящее время в базе данных с текущими значениями в сущности. Это обычно требует некоторых пользовательских взаимодействие логики или пользователя. Например формы может представлять пользователю, содержащий текущие значения, значения в базе данных, и значение по умолчанию набора разрешенных значений. Пользователь может затем измените разрешенные значения при необходимости и было бы эти разрешенного значения, которые сохраняются в базу данных. Это можно сделать с помощью DbPropertyValues объекты, возвращаемые CurrentValues и GetDatabaseValues на запись сущности. Пример:  

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

## <a name="custom-resolution-of-optimistic-concurrency-exceptions-using-objects"></a>Пользовательское разрешение исключений оптимистичного параллелизма, с помощью объектов  

Приведенный выше код использует DbPropertyValues экземпляры для передачи текущей базы данных и разрешенные значения. Иногда может быть проще использовать экземпляры типа сущности. Это можно сделать с помощью методов ToObject и SetValues DbPropertyValues. Пример:  

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
