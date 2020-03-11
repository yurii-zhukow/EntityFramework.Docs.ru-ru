---
title: Обработка конфликтов параллелизма — EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 2318e4d3-f561-4720-bbc3-921556806476
ms.openlocfilehash: 81ae186201fdfac331b1d4e7836b222545fe78b5
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/06/2020
ms.locfileid: "78416249"
---
# <a name="handling-concurrency-conflicts"></a><span data-ttu-id="86aa8-102">Обработка конфликтов параллелизма</span><span class="sxs-lookup"><span data-stu-id="86aa8-102">Handling Concurrency Conflicts</span></span>
<span data-ttu-id="86aa8-103">Оптимистическая блокировка подразумевает вызывает попытку сохранить сущность в базе данных, что не изменило данные с момента загрузки сущности.</span><span class="sxs-lookup"><span data-stu-id="86aa8-103">Optimistic concurrency involves optimistically attempting to save your entity to the database in the hope that the data there has not changed since the entity was loaded.</span></span> <span data-ttu-id="86aa8-104">Если выяснилось, что данные изменились, возникает исключение и необходимо устранить конфликт перед повторным сохранением.</span><span class="sxs-lookup"><span data-stu-id="86aa8-104">If it turns out that the data has changed then an exception is thrown and you must resolve the conflict before attempting to save again.</span></span> <span data-ttu-id="86aa8-105">В этом разделе описано, как управлять такими исключениями в Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="86aa8-105">This topic covers how to handle such exceptions in Entity Framework.</span></span> <span data-ttu-id="86aa8-106">Методы, представленные в этом разделе, также применимы к моделям, созданным с помощью Code First и конструктора EF.</span><span class="sxs-lookup"><span data-stu-id="86aa8-106">The techniques shown in this topic apply equally to models created with Code First and the EF Designer.</span></span>  

<span data-ttu-id="86aa8-107">Эта запись не является необходимым местом для полного обсуждения оптимистичного параллелизма.</span><span class="sxs-lookup"><span data-stu-id="86aa8-107">This post is not the appropriate place for a full discussion of optimistic concurrency.</span></span> <span data-ttu-id="86aa8-108">В следующих разделах предполагается наличие некоторых знаний о разрешении параллелизма и показаны закономерности для распространенных задач.</span><span class="sxs-lookup"><span data-stu-id="86aa8-108">The sections below assume some knowledge of concurrency resolution and show patterns for common tasks.</span></span>  

<span data-ttu-id="86aa8-109">Во многих из этих шаблонов используются разделы, обсуждаемые в разделе [Работа со значениями свойств](~/ef6/saving/change-tracking/property-values.md).</span><span class="sxs-lookup"><span data-stu-id="86aa8-109">Many of these patterns make use of the topics discussed in [Working with Property Values](~/ef6/saving/change-tracking/property-values.md).</span></span>  

<span data-ttu-id="86aa8-110">Разрешение проблем параллелизма при использовании независимых ассоциаций (когда внешний ключ не сопоставляется со свойством в сущности) гораздо сложнее, чем при использовании связей по внешнему ключу.</span><span class="sxs-lookup"><span data-stu-id="86aa8-110">Resolving concurrency issues when you are using independent associations (where the foreign key is not mapped to a property in your entity) is much more difficult than when you are using foreign key associations.</span></span> <span data-ttu-id="86aa8-111">Поэтому, если вы собираетесь выполнять разрешение параллелизма в приложении, рекомендуется всегда сопоставлять внешние ключи с сущностями.</span><span class="sxs-lookup"><span data-stu-id="86aa8-111">Therefore if you are going to do concurrency resolution in your application it is advised that you always map foreign keys into your entities.</span></span> <span data-ttu-id="86aa8-112">В приведенных ниже примерах предполагается, что вы используете связи по внешнему ключу.</span><span class="sxs-lookup"><span data-stu-id="86aa8-112">All the examples below assume that you are using foreign key associations.</span></span>  

<span data-ttu-id="86aa8-113">Метод SaveChanges вызывает исключение Дбупдатеконкурренциексцептион, когда при попытке сохранить сущность, использующую ассоциации внешних ключей, обнаруживается исключение оптимистичного параллелизма.</span><span class="sxs-lookup"><span data-stu-id="86aa8-113">A DbUpdateConcurrencyException is thrown by SaveChanges when an optimistic concurrency exception is detected while attempting to save an entity that uses foreign key associations.</span></span>  

## <a name="resolving-optimistic-concurrency-exceptions-with-reload-database-wins"></a><span data-ttu-id="86aa8-114">Разрешение исключений оптимистичного параллелизма с помощью перезагрузки (WINS-база данных)</span><span class="sxs-lookup"><span data-stu-id="86aa8-114">Resolving optimistic concurrency exceptions with Reload (database wins)</span></span>  

<span data-ttu-id="86aa8-115">Метод Reload можно использовать для перезаписи текущих значений сущности значениями, которые теперь находятся в базе данных.</span><span class="sxs-lookup"><span data-stu-id="86aa8-115">The Reload method can be used to overwrite the current values of the entity with the values now in the database.</span></span> <span data-ttu-id="86aa8-116">Затем сущность, как правило, передается пользователю в некоторой форме и должен попытаться снова внести свои изменения и сохранить их повторно.</span><span class="sxs-lookup"><span data-stu-id="86aa8-116">The entity is then typically given back to the user in some form and they must try to make their changes again and re-save.</span></span> <span data-ttu-id="86aa8-117">Пример:</span><span class="sxs-lookup"><span data-stu-id="86aa8-117">For example:</span></span>  

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

<span data-ttu-id="86aa8-118">Хорошим способом имитации исключения параллелизма является установка точки останова в вызове SaveChanges, а затем Изменение сущности, которая сохраняется в базе данных с помощью другого средства, такого как SQL Management Studio.</span><span class="sxs-lookup"><span data-stu-id="86aa8-118">A good way to simulate a concurrency exception is to set a breakpoint on the SaveChanges call and then modify an entity that is being saved in the database using another tool such as SQL Management Studio.</span></span> <span data-ttu-id="86aa8-119">Можно также вставить строку перед параметром SaveChanges, чтобы обновить базу данных непосредственно с помощью SqlCommand.</span><span class="sxs-lookup"><span data-stu-id="86aa8-119">You could also insert a line before SaveChanges to update the database directly using SqlCommand.</span></span> <span data-ttu-id="86aa8-120">Пример:</span><span class="sxs-lookup"><span data-stu-id="86aa8-120">For example:</span></span>  

``` csharp
context.Database.SqlCommand(
    "UPDATE dbo.Blogs SET Name = 'Another Name' WHERE BlogId = 1");
```  

<span data-ttu-id="86aa8-121">Метод записей в Дбупдатеконкурренциексцептион возвращает экземпляры Дбентитентри для сущностей, которые не удалось обновить.</span><span class="sxs-lookup"><span data-stu-id="86aa8-121">The Entries method on DbUpdateConcurrencyException returns the DbEntityEntry instances for the entities that failed to update.</span></span> <span data-ttu-id="86aa8-122">(В настоящее время это свойство всегда возвращает одно значение для проблем параллелизма.</span><span class="sxs-lookup"><span data-stu-id="86aa8-122">(This property currently always returns a single value for concurrency issues.</span></span> <span data-ttu-id="86aa8-123">Он может возвращать несколько значений для общих исключений обновления.) Альтернативой в некоторых ситуациях может быть получение записей для всех сущностей, которые, возможно, потребуется перезагрузить из базы данных и вызвать перезагрузку для каждого из них.</span><span class="sxs-lookup"><span data-stu-id="86aa8-123">It may return multiple values for general update exceptions.) An alternative for some situations might be to get entries for all entities that may need to be reloaded from the database and call reload for each of these.</span></span>  

## <a name="resolving-optimistic-concurrency-exceptions-as-client-wins"></a><span data-ttu-id="86aa8-124">Разрешение исключений оптимистичного параллелизма в качестве клиента WINS</span><span class="sxs-lookup"><span data-stu-id="86aa8-124">Resolving optimistic concurrency exceptions as client wins</span></span>  

<span data-ttu-id="86aa8-125">Приведенный выше пример, использующий перезагрузку, иногда называется Database WINS или Store WINS, поскольку значения в сущности перезаписываются значениями из базы данных.</span><span class="sxs-lookup"><span data-stu-id="86aa8-125">The example above that uses Reload is sometimes called database wins or store wins because the values in the entity are overwritten by values from the database.</span></span> <span data-ttu-id="86aa8-126">Иногда может потребоваться выполнить противоположные действия и перезаписать значения в базе данных значениями, находящиеся в сущности.</span><span class="sxs-lookup"><span data-stu-id="86aa8-126">Sometimes you may wish to do the opposite and overwrite the values in the database with the values currently in the entity.</span></span> <span data-ttu-id="86aa8-127">Это иногда называется клиентом WINS и может быть выполнено путем получения текущих значений базы данных и установки их в качестве исходных значений для сущности.</span><span class="sxs-lookup"><span data-stu-id="86aa8-127">This is sometimes called client wins and can be done by getting the current database values and setting them as the original values for the entity.</span></span> <span data-ttu-id="86aa8-128">(Сведения о текущих и исходных значениях см. в разделе [Работа со значениями свойств](~/ef6/saving/change-tracking/property-values.md) .) Например:</span><span class="sxs-lookup"><span data-stu-id="86aa8-128">(See [Working with Property Values](~/ef6/saving/change-tracking/property-values.md) for information on current and original values.) For example:</span></span>  

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

## <a name="custom-resolution-of-optimistic-concurrency-exceptions"></a><span data-ttu-id="86aa8-129">Пользовательское разрешение исключений оптимистичного параллелизма</span><span class="sxs-lookup"><span data-stu-id="86aa8-129">Custom resolution of optimistic concurrency exceptions</span></span>  

<span data-ttu-id="86aa8-130">Иногда может потребоваться объединить значения, находящиеся в базе данных, с текущими значениями в сущности.</span><span class="sxs-lookup"><span data-stu-id="86aa8-130">Sometimes you may want to combine the values currently in the database with the values currently in the entity.</span></span> <span data-ttu-id="86aa8-131">Обычно это требует некоторой пользовательской логики или взаимодействия с пользователем.</span><span class="sxs-lookup"><span data-stu-id="86aa8-131">This usually requires some custom logic or user interaction.</span></span> <span data-ttu-id="86aa8-132">Например, пользователь может предоставить форму пользователю, содержащую текущие значения, значения в базе данных и набор разрешенных значений по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="86aa8-132">For example, you might present a form to the user containing the current values, the values in the database, and a default set of resolved values.</span></span> <span data-ttu-id="86aa8-133">При необходимости пользователь будет изменять разрешенные значения, и эти разрешенные значения будут сохранены в базе данных.</span><span class="sxs-lookup"><span data-stu-id="86aa8-133">The user would then edit the resolved values as necessary and it would be these resolved values that get saved to the database.</span></span> <span data-ttu-id="86aa8-134">Это можно сделать с помощью объектов DbPropertyValues, возвращаемых из Куррентвалуес и Жетдатабасевалуес, в записи сущности.</span><span class="sxs-lookup"><span data-stu-id="86aa8-134">This can be done using the DbPropertyValues objects returned from CurrentValues and GetDatabaseValues on the entity’s entry.</span></span> <span data-ttu-id="86aa8-135">Пример:</span><span class="sxs-lookup"><span data-stu-id="86aa8-135">For example:</span></span>  

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

## <a name="custom-resolution-of-optimistic-concurrency-exceptions-using-objects"></a><span data-ttu-id="86aa8-136">Пользовательское разрешение исключений оптимистичного параллелизма с помощью объектов</span><span class="sxs-lookup"><span data-stu-id="86aa8-136">Custom resolution of optimistic concurrency exceptions using objects</span></span>  

<span data-ttu-id="86aa8-137">В приведенном выше коде используются экземпляры DbPropertyValues для передачи текущих, баз данных и разрешенных значений.</span><span class="sxs-lookup"><span data-stu-id="86aa8-137">The code above uses DbPropertyValues instances for passing around current, database, and resolved values.</span></span> <span data-ttu-id="86aa8-138">Иногда для этого может оказаться проще использовать экземпляры типа сущности.</span><span class="sxs-lookup"><span data-stu-id="86aa8-138">Sometimes it may be easier to use instances of your entity type for this.</span></span> <span data-ttu-id="86aa8-139">Это можно сделать с помощью методов Тубжект и SetValue из DbPropertyValues.</span><span class="sxs-lookup"><span data-stu-id="86aa8-139">This can be done using the ToObject and SetValues methods of DbPropertyValues.</span></span> <span data-ttu-id="86aa8-140">Пример:</span><span class="sxs-lookup"><span data-stu-id="86aa8-140">For example:</span></span>  

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
