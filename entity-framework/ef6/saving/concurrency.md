---
title: Обработка конфликтов параллелизма — EF6
author: divega
ms.date: 2016-10-23
ms.assetid: 2318e4d3-f561-4720-bbc3-921556806476
ms.openlocfilehash: f233af217287dd6bf35e5b7fea8e44974168b312
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/27/2018
ms.locfileid: "42997814"
---
# <a name="handling-concurrency-conflicts"></a><span data-ttu-id="c96b9-102">Обработка конфликтов параллелизма</span><span class="sxs-lookup"><span data-stu-id="c96b9-102">Handling Concurrency Conflicts</span></span>
<span data-ttu-id="c96b9-103">Оптимистичный параллелизм предусматривает оптимистически попытка сохранить сущность в базу данных в надеюсь, что данные оттуда не изменялась после сущность была загружена.</span><span class="sxs-lookup"><span data-stu-id="c96b9-103">Optimistic concurrency involves optimistically attempting to save your entity to the database in the hope that the data there has not changed since the entity was loaded.</span></span> <span data-ttu-id="c96b9-104">Если оказывается, что данные изменились, то возникает исключение, и необходимо разрешить конфликт, прежде чем сохранить еще раз.</span><span class="sxs-lookup"><span data-stu-id="c96b9-104">If it turns out that the data has changed then an exception is thrown and you must resolve the conflict before attempting to save again.</span></span> <span data-ttu-id="c96b9-105">В этом разделе описывается, как для обработки таких исключений в Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="c96b9-105">This topic covers how to handle such exceptions in Entity Framework.</span></span> <span data-ttu-id="c96b9-106">Методы, представленные в этом разделе, также применимы к моделям, созданным с помощью Code First и конструктора EF.</span><span class="sxs-lookup"><span data-stu-id="c96b9-106">The techniques shown in this topic apply equally to models created with Code First and the EF Designer.</span></span>  

<span data-ttu-id="c96b9-107">Это сообщение не подходящим местом для полное описание оптимистичного параллелизма.</span><span class="sxs-lookup"><span data-stu-id="c96b9-107">This post is not the appropriate place for a full discussion of optimistic concurrency.</span></span> <span data-ttu-id="c96b9-108">В следующих разделах, предполагают, что некоторые разрешения параллелизма и отобразите шаблоны для распространенных задач.</span><span class="sxs-lookup"><span data-stu-id="c96b9-108">The sections below assume some knowledge of concurrency resolution and show patterns for common tasks.</span></span>  

<span data-ttu-id="c96b9-109">Многие из этих шаблонов использования темам, обсуждавшимся в [работа со значениями свойств](~/ef6/saving/change-tracking/property-values.md).</span><span class="sxs-lookup"><span data-stu-id="c96b9-109">Many of these patterns make use of the topics discussed in [Working with Property Values](~/ef6/saving/change-tracking/property-values.md).</span></span>  

<span data-ttu-id="c96b9-110">Устранение проблем параллелизма, при использовании независимых сопоставлений (которой внешний ключ не сопоставлен со свойством сущности) гораздо сложнее, чем при использовании внешнего ключа ассоциации.</span><span class="sxs-lookup"><span data-stu-id="c96b9-110">Resolving concurrency issues when you are using independent associations (where the foreign key is not mapped to a property in your entity) is much more difficult than when you are using foreign key associations.</span></span> <span data-ttu-id="c96b9-111">Таким образом Если необходимо выполнить разрешение параллелизма в приложении рекомендуется всегда сопоставляются внешних ключей в сущности.</span><span class="sxs-lookup"><span data-stu-id="c96b9-111">Therefore if you are going to do concurrency resolution in your application it is advised that you always map foreign keys into your entities.</span></span> <span data-ttu-id="c96b9-112">Всех приведенных ниже примерах предполагается, что вы используете ассоциации внешних ключей.</span><span class="sxs-lookup"><span data-stu-id="c96b9-112">All the examples below assume that you are using foreign key associations.</span></span>  

<span data-ttu-id="c96b9-113">DbUpdateConcurrencyException генерируется SaveChanges при обнаружении исключения оптимистичного параллелизма при попытке сохранить сущность, которая использует связи внешнего ключа.</span><span class="sxs-lookup"><span data-stu-id="c96b9-113">A DbUpdateConcurrencyException is thrown by SaveChanges when an optimistic concurrency exception is detected while attempting to save an entity that uses foreign key associations.</span></span>  

## <a name="resolving-optimistic-concurrency-exceptions-with-reload-database-wins"></a><span data-ttu-id="c96b9-114">Разрешения исключений оптимистичного параллелизма с перезагрузить базы данных wins)</span><span class="sxs-lookup"><span data-stu-id="c96b9-114">Resolving optimistic concurrency exceptions with Reload (database wins)</span></span>  

<span data-ttu-id="c96b9-115">Метод перезагрузки может использоваться для перезаписи текущих значений сущности со значениями, теперь в базе данных.</span><span class="sxs-lookup"><span data-stu-id="c96b9-115">The Reload method can be used to overwrite the current values of the entity with the values now in the database.</span></span> <span data-ttu-id="c96b9-116">Сущность обычно предоставляется обратно пользователю в определенной форме, и они пытаются снова внести свои изменения и повторно сохраните.</span><span class="sxs-lookup"><span data-stu-id="c96b9-116">The entity is then typically given back to the user in some form and they must try to make their changes again and re-save.</span></span> <span data-ttu-id="c96b9-117">Пример:</span><span class="sxs-lookup"><span data-stu-id="c96b9-117">For example:</span></span>  

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

<span data-ttu-id="c96b9-118">Чтобы установить точку останова в вызове метода SaveChanges, а затем измените сущности, который сохраняется в базе данных с помощью другого средства, например SQL Management Studio является хорошим способом имитации исключения параллельности.</span><span class="sxs-lookup"><span data-stu-id="c96b9-118">A good way to simulate a concurrency exception is to set a breakpoint on the SaveChanges call and then modify an entity that is being saved in the database using another tool such as SQL Management Studio.</span></span> <span data-ttu-id="c96b9-119">Вы также может вставить строку перед SaveChanges для обновления базы данных, непосредственно с помощью SqlCommand.</span><span class="sxs-lookup"><span data-stu-id="c96b9-119">You could also insert a line before SaveChanges to update the database directly using SqlCommand.</span></span> <span data-ttu-id="c96b9-120">Пример:</span><span class="sxs-lookup"><span data-stu-id="c96b9-120">For example:</span></span>  

``` csharp
context.Database.SqlCommand(
    "UPDATE dbo.Blogs SET Name = 'Another Name' WHERE BlogId = 1");
```  

<span data-ttu-id="c96b9-121">Метод записи на DbUpdateConcurrencyException возвращает dbentityentry, который должен экземпляры сущности, которые не удалось обновить.</span><span class="sxs-lookup"><span data-stu-id="c96b9-121">The Entries method on DbUpdateConcurrencyException returns the DbEntityEntry instances for the entities that failed to update.</span></span> <span data-ttu-id="c96b9-122">(Это свойство в настоящее время всегда возвращает одно значение для проблем параллелизма.</span><span class="sxs-lookup"><span data-stu-id="c96b9-122">(This property currently always returns a single value for concurrency issues.</span></span> <span data-ttu-id="c96b9-123">Он может возвращать несколько значений для обновления общего исключения.) Альтернативы в некоторых случаях может быть получение записей для всех сущностей, которые может потребоваться перезагрузить из базы данных и перезагрузить вызова для каждого из них.</span><span class="sxs-lookup"><span data-stu-id="c96b9-123">It may return multiple values for general update exceptions.) An alternative for some situations might be to get entries for all entities that may need to be reloaded from the database and call reload for each of these.</span></span>  

## <a name="resolving-optimistic-concurrency-exceptions-as-client-wins"></a><span data-ttu-id="c96b9-124">Разрешения исключений оптимистичного параллелизма, как приоритет клиента</span><span class="sxs-lookup"><span data-stu-id="c96b9-124">Resolving optimistic concurrency exceptions as client wins</span></span>  

<span data-ttu-id="c96b9-125">Приведенном выше примере, использующий перезагрузить иногда называют базы данных wins или победы хранилища, так как значения в сущности, перезаписываются значениями из базы данных.</span><span class="sxs-lookup"><span data-stu-id="c96b9-125">The example above that uses Reload is sometimes called database wins or store wins because the values in the entity are overwritten by values from the database.</span></span> <span data-ttu-id="c96b9-126">Иногда может потребоваться сделать обратное и перезаписи значений базы данных с текущими значениями в сущности.</span><span class="sxs-lookup"><span data-stu-id="c96b9-126">Sometimes you may wish to do the opposite and overwrite the values in the database with the values currently in the entity.</span></span> <span data-ttu-id="c96b9-127">Это иногда называется приоритет клиента и можно сделать, получив текущие значения базы данных и их настройке в качестве исходных значений для сущности.</span><span class="sxs-lookup"><span data-stu-id="c96b9-127">This is sometimes called client wins and can be done by getting the current database values and setting them as the original values for the entity.</span></span> <span data-ttu-id="c96b9-128">(См. в разделе [работа со значениями свойств](~/ef6/saving/change-tracking/property-values.md) сведения на текущие и исходные значения.) Пример:</span><span class="sxs-lookup"><span data-stu-id="c96b9-128">(See [Working with Property Values](~/ef6/saving/change-tracking/property-values.md) for information on current and original values.) For example:</span></span>  

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

## <a name="custom-resolution-of-optimistic-concurrency-exceptions"></a><span data-ttu-id="c96b9-129">Пользовательское разрешение исключений оптимистичного параллелизма</span><span class="sxs-lookup"><span data-stu-id="c96b9-129">Custom resolution of optimistic concurrency exceptions</span></span>  

<span data-ttu-id="c96b9-130">Иногда может потребоваться объединить значения, в настоящее время в базе данных с текущими значениями в сущности.</span><span class="sxs-lookup"><span data-stu-id="c96b9-130">Sometimes you may want to combine the values currently in the database with the values currently in the entity.</span></span> <span data-ttu-id="c96b9-131">Это обычно требует некоторых пользовательских взаимодействие логики или пользователя.</span><span class="sxs-lookup"><span data-stu-id="c96b9-131">This usually requires some custom logic or user interaction.</span></span> <span data-ttu-id="c96b9-132">Например формы может представлять пользователю, содержащий текущие значения, значения в базе данных, и значение по умолчанию набора разрешенных значений.</span><span class="sxs-lookup"><span data-stu-id="c96b9-132">For example, you might present a form to the user containing the current values, the values in the database, and a default set of resolved values.</span></span> <span data-ttu-id="c96b9-133">Пользователь может затем измените разрешенные значения при необходимости и было бы эти разрешенного значения, которые сохраняются в базу данных.</span><span class="sxs-lookup"><span data-stu-id="c96b9-133">The user would then edit the resolved values as necessary and it would be these resolved values that get saved to the database.</span></span> <span data-ttu-id="c96b9-134">Это можно сделать с помощью DbPropertyValues объекты, возвращаемые CurrentValues и GetDatabaseValues на запись сущности.</span><span class="sxs-lookup"><span data-stu-id="c96b9-134">This can be done using the DbPropertyValues objects returned from CurrentValues and GetDatabaseValues on the entity’s entry.</span></span> <span data-ttu-id="c96b9-135">Пример:</span><span class="sxs-lookup"><span data-stu-id="c96b9-135">For example:</span></span>  

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

## <a name="custom-resolution-of-optimistic-concurrency-exceptions-using-objects"></a><span data-ttu-id="c96b9-136">Пользовательское разрешение исключений оптимистичного параллелизма, с помощью объектов</span><span class="sxs-lookup"><span data-stu-id="c96b9-136">Custom resolution of optimistic concurrency exceptions using objects</span></span>  

<span data-ttu-id="c96b9-137">Приведенный выше код использует DbPropertyValues экземпляры для передачи текущей базы данных и разрешенные значения.</span><span class="sxs-lookup"><span data-stu-id="c96b9-137">The code above uses DbPropertyValues instances for passing around current, database, and resolved values.</span></span> <span data-ttu-id="c96b9-138">Иногда может быть проще использовать экземпляры типа сущности.</span><span class="sxs-lookup"><span data-stu-id="c96b9-138">Sometimes it may be easier to use instances of your entity type for this.</span></span> <span data-ttu-id="c96b9-139">Это можно сделать с помощью методов ToObject и SetValues DbPropertyValues.</span><span class="sxs-lookup"><span data-stu-id="c96b9-139">This can be done using the ToObject and SetValues methods of DbPropertyValues.</span></span> <span data-ttu-id="c96b9-140">Пример:</span><span class="sxs-lookup"><span data-stu-id="c96b9-140">For example:</span></span>  

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
