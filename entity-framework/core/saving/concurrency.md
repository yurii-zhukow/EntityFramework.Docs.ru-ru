---
title: "Обработка параллелизма - EF Core"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: bce0539d-b0cd-457d-be71-f7ca16f3baea
ms.technology: entity-framework-core
uid: core/saving/concurrency
ms.openlocfilehash: bbd3e154c1b27b16c7d8f8fbf9ed51df0849795c
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/27/2017
---
# <a name="handling-concurrency"></a><span data-ttu-id="835c3-102">Обработка параллелизма</span><span class="sxs-lookup"><span data-stu-id="835c3-102">Handling Concurrency</span></span>

<span data-ttu-id="835c3-103">Если свойство настроено как маркер параллелизма затем EF будет проверять, что ни один другой пользователь изменил это значение в базе данных при сохранении изменений в этой записи.</span><span class="sxs-lookup"><span data-stu-id="835c3-103">If a property is configured as a concurrency token then EF will check that no other user has modified that value in the database when saving changes to that record.</span></span>

> [!TIP]  
> <span data-ttu-id="835c3-104">Можно просмотреть в этой статье [пример](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/Concurrency/) на GitHub.</span><span class="sxs-lookup"><span data-stu-id="835c3-104">You can view this article's [sample](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/Concurrency/) on GitHub.</span></span>

## <a name="how-concurrency-handling-works-in-ef-core"></a><span data-ttu-id="835c3-105">Как работает обработки параллелизма в EF Core</span><span class="sxs-lookup"><span data-stu-id="835c3-105">How concurrency handling works in EF Core</span></span>

<span data-ttu-id="835c3-106">Подробное описание того, как работает обработки параллелизма в Entity Framework Core см. в разделе [маркеры параллелизма](../modeling/concurrency.md).</span><span class="sxs-lookup"><span data-stu-id="835c3-106">For a detailed description of how concurrency handling works in Entity Framework Core, see [Concurrency Tokens](../modeling/concurrency.md).</span></span>

## <a name="resolving-concurrency-conflicts"></a><span data-ttu-id="835c3-107">Разрешение конфликтов параллелизма</span><span class="sxs-lookup"><span data-stu-id="835c3-107">Resolving concurrency conflicts</span></span>

<span data-ttu-id="835c3-108">Разрешение конфликтов параллелизма предполагает с помощью алгоритма для слияния ожидающие изменения от имени текущего пользователя с изменениями, внесенными в базе данных.</span><span class="sxs-lookup"><span data-stu-id="835c3-108">Resolving a concurrency conflict involves using an algorithm to merge the pending changes from the current user with the changes made in the database.</span></span> <span data-ttu-id="835c3-109">Точный подход будет отличаться в зависимости от приложения, но наиболее распространенный подход заключается в том, для отображения пользователю значений и их определить правильные значения, хранимые в базе данных.</span><span class="sxs-lookup"><span data-stu-id="835c3-109">The exact approach will vary based on your application, but a common approach is to display the values to the user and have them decide the correct values to be stored in the database.</span></span>

<span data-ttu-id="835c3-110">**Существует три набора значений, который помогает разрешить конфликт параллелизма.**</span><span class="sxs-lookup"><span data-stu-id="835c3-110">**There are three sets of values available to help resolve a concurrency conflict.**</span></span>

* <span data-ttu-id="835c3-111">**Текущие значения** — это значения, при попытке приложения записать в базу данных.</span><span class="sxs-lookup"><span data-stu-id="835c3-111">**Current values** are the values that the application was attempting to write to the database.</span></span>

* <span data-ttu-id="835c3-112">**Исходные значения** — это значения, изначально извлеченные из базы данных, перед были внесены изменения.</span><span class="sxs-lookup"><span data-stu-id="835c3-112">**Original values** are the values that were originally retrieved from the database, before any edits were made.</span></span>

* <span data-ttu-id="835c3-113">**Базы данных значения** являются значения, которые хранятся в базе данных.</span><span class="sxs-lookup"><span data-stu-id="835c3-113">**Database values** are the values currently stored in the database.</span></span>

<span data-ttu-id="835c3-114">Для обработки конфликтов параллелизма, перехватывать `DbUpdateConcurrencyException` во время `SaveChanges()`, используйте `DbUpdateConcurrencyException.Entries` подготовить новый набор изменений для затронутых объектов, а затем повторите `SaveChanges()` операции.</span><span class="sxs-lookup"><span data-stu-id="835c3-114">To handle a concurrency conflict, catch a `DbUpdateConcurrencyException` during `SaveChanges()`, use `DbUpdateConcurrencyException.Entries` to prepare a new set of changes for the affected entities, and then retry the `SaveChanges()` operation.</span></span>

<span data-ttu-id="835c3-115">В следующем примере `Person.FirstName` и `Person.LastName` настраиваются как маркер параллелизма.</span><span class="sxs-lookup"><span data-stu-id="835c3-115">In the following example, `Person.FirstName` and `Person.LastName` are setup as concurrency token.</span></span> <span data-ttu-id="835c3-116">Отсутствует `// TODO:` комментарий в расположение, где следует включить определенную логику приложения выберите значение сохраняется в базе данных.</span><span class="sxs-lookup"><span data-stu-id="835c3-116">There is a `// TODO:` comment in the location where you would include application specific logic to choose the value to be saved to the database.</span></span>

<!-- [!code-csharp[Main](samples/core/Saving/Saving/Concurrency/Sample.cs?highlight=53,54)] -->
``` csharp
using Microsoft.EntityFrameworkCore;
using System;
using System.ComponentModel.DataAnnotations;
using System.Linq;

namespace EFSaving.Concurrency
{
    public class Sample
    {
        public static void Run()
        {
            // Ensure database is created and has a person in it
            using (var context = new PersonContext())
            {
                context.Database.EnsureDeleted();
                context.Database.EnsureCreated();

                context.People.Add(new Person { FirstName = "John", LastName = "Doe" });
                context.SaveChanges();
            }

            using (var context = new PersonContext())
            {
                // Fetch a person from database and change phone number
                var person = context.People.Single(p => p.PersonId == 1);
                person.PhoneNumber = "555-555-5555";

                // Change the persons name in the database (will cause a concurrency conflict)
                context.Database.ExecuteSqlCommand("UPDATE dbo.People SET FirstName = 'Jane' WHERE PersonId = 1");

                try
                {
                    // Attempt to save changes to the database
                    context.SaveChanges();
                }
                catch (DbUpdateConcurrencyException ex)
                {
                    foreach (var entry in ex.Entries)
                    {
                        if (entry.Entity is Person)
                        {
                            // Using a NoTracking query means we get the entity but it is not tracked by the context
                            // and will not be merged with existing entities in the context.
                            var databaseEntity = context.People.AsNoTracking().Single(p => p.PersonId == ((Person)entry.Entity).PersonId);
                            var databaseEntry = context.Entry(databaseEntity);

                            foreach (var property in entry.Metadata.GetProperties())
                            {
                                var proposedValue = entry.Property(property.Name).CurrentValue;
                                var originalValue = entry.Property(property.Name).OriginalValue;
                                var databaseValue = databaseEntry.Property(property.Name).CurrentValue;

                                // TODO: Logic to decide which value should be written to database
                                // entry.Property(property.Name).CurrentValue = <value to be saved>;

                                // Update original values to
                                entry.Property(property.Name).OriginalValue = databaseEntry.Property(property.Name).CurrentValue;
                            }
                        }
                        else
                        {
                            throw new NotSupportedException("Don't know how to handle concurrency conflicts for " + entry.Metadata.Name);
                        }
                    }

                    // Retry the save operation
                    context.SaveChanges();
                }
            }
        }

        public class PersonContext : DbContext
        {
            public DbSet<Person> People { get; set; }

            protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
            {
                optionsBuilder.UseSqlServer(@"Server=(localdb)\mssqllocaldb;Database=EFSaving.Concurrency;Trusted_Connection=True;");
            }
        }

        public class Person
        {
            public int PersonId { get; set; }

            [ConcurrencyCheck]
            public string FirstName { get; set; }

            [ConcurrencyCheck]
            public string LastName { get; set; }

            public string PhoneNumber { get; set; }
        }

    }
}
```
