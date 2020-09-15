---
title: Настройка таблицы журналов миграции — EF6
description: Настройка таблицы журнала миграции в Entity Framework 6
author: divega
ms.date: 10/23/2016
uid: ef6/modeling/code-first/migrations/history-customization
ms.openlocfilehash: b71ac7635d6804af2db9bf00acf7e8f4b2a11033
ms.sourcegitcommit: abda0872f86eefeca191a9a11bfca976bc14468b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/14/2020
ms.locfileid: "90073465"
---
# <a name="customizing-the-migrations-history-table"></a><span data-ttu-id="4c5b4-103">Настройка таблицы журнала миграции</span><span class="sxs-lookup"><span data-stu-id="4c5b4-103">Customizing the migrations history table</span></span>
> [!NOTE]
> <span data-ttu-id="4c5b4-104">**Только в EF6 и более поздних версиях**. Функции, API и другие возможности, описанные на этой странице, появились в Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="4c5b4-104">**EF6 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 6.</span></span> <span data-ttu-id="4c5b4-105">При использовании более ранней версии могут быть неприменимы некоторые или все сведения.</span><span class="sxs-lookup"><span data-stu-id="4c5b4-105">If you are using an earlier version, some or all of the information does not apply.</span></span>

> [!NOTE]
> <span data-ttu-id="4c5b4-106">В этой статье предполагается, что вы умеете использовать Code First Migrations в основных сценариях.</span><span class="sxs-lookup"><span data-stu-id="4c5b4-106">This article assumes you know how to use Code First Migrations in basic scenarios.</span></span> <span data-ttu-id="4c5b4-107">В противном случае необходимо прочитать [Code First migrations](xref:ef6/modeling/code-first/migrations/index) , прежде чем продолжить.</span><span class="sxs-lookup"><span data-stu-id="4c5b4-107">If you don’t, then you’ll need to read [Code First Migrations](xref:ef6/modeling/code-first/migrations/index) before continuing.</span></span>

## <a name="what-is-migrations-history-table"></a><span data-ttu-id="4c5b4-108">Что такое таблица журнала миграции?</span><span class="sxs-lookup"><span data-stu-id="4c5b4-108">What is Migrations History Table?</span></span>

<span data-ttu-id="4c5b4-109">Таблица журнала миграции — это таблица, используемая Code First Migrations для хранения сведений о миграции, примененных к базе данных.</span><span class="sxs-lookup"><span data-stu-id="4c5b4-109">Migrations history table is a table used by Code First Migrations to store details about migrations applied to the database.</span></span> <span data-ttu-id="4c5b4-110">По умолчанию имя таблицы в базе данных — \_ \_ мигратионхистори и создается при применении первой миграции к базе данных.</span><span class="sxs-lookup"><span data-stu-id="4c5b4-110">By default the name of the table in the database is \_\_MigrationHistory and it is created when applying the first migration to the database.</span></span> <span data-ttu-id="4c5b4-111">В Entity Framework 5 Эта таблица была системной таблицей, если приложение использовало базу данных Microsoft SQL Server.</span><span class="sxs-lookup"><span data-stu-id="4c5b4-111">In Entity Framework 5 this table was a system table if the application used Microsoft Sql Server database.</span></span> <span data-ttu-id="4c5b4-112">Это изменилось в Entity Framework 6, но таблица журнала миграции больше не помечена системной таблицей.</span><span class="sxs-lookup"><span data-stu-id="4c5b4-112">This has changed in Entity Framework 6 however and the migrations history table is no longer marked a system table.</span></span>

## <a name="why-customize-migrations-history-table"></a><span data-ttu-id="4c5b4-113">Зачем настраивать таблицу журнала миграции?</span><span class="sxs-lookup"><span data-stu-id="4c5b4-113">Why customize Migrations History Table?</span></span>

<span data-ttu-id="4c5b4-114">Таблица журнала миграции должна использоваться исключительно Code First Migrations и изменить ее вручную может привести к нарушению миграции.</span><span class="sxs-lookup"><span data-stu-id="4c5b4-114">Migrations history table is supposed to be used solely by Code First Migrations and changing it manually can break migrations.</span></span> <span data-ttu-id="4c5b4-115">Однако иногда конфигурация по умолчанию не подходит и таблица должна быть настроена, например:</span><span class="sxs-lookup"><span data-stu-id="4c5b4-115">However sometimes the default configuration is not suitable and the table needs to be customized, for instance:</span></span>

-   <span data-ttu-id="4c5b4-116">Необходимо изменить имена и (или) аспекты столбцов, чтобы включить 3 поставщика миграции субъектов<sup>удаленных рабочих столов</sup></span><span class="sxs-lookup"><span data-stu-id="4c5b4-116">You need to change names and/or facets of the columns to enable a 3<sup>rd</sup> party Migrations provider</span></span>
-   <span data-ttu-id="4c5b4-117">Необходимо изменить имя таблицы</span><span class="sxs-lookup"><span data-stu-id="4c5b4-117">You want to change the name of the table</span></span>
-   <span data-ttu-id="4c5b4-118">Для \_ таблицы мигратионхистори необходимо использовать схему, не используемую по умолчанию \_</span><span class="sxs-lookup"><span data-stu-id="4c5b4-118">You need to use a non-default schema for the \_\_MigrationHistory table</span></span>
-   <span data-ttu-id="4c5b4-119">Необходимо сохранить дополнительные данные для данной версии контекста, поэтому необходимо добавить дополнительный столбец в таблицу.</span><span class="sxs-lookup"><span data-stu-id="4c5b4-119">You need to store additional data for a given version of the context and therefore you need to add an additional column to the table</span></span>

## <a name="words-of-precaution"></a><span data-ttu-id="4c5b4-120">Слова предосторожности</span><span class="sxs-lookup"><span data-stu-id="4c5b4-120">Words of precaution</span></span>

<span data-ttu-id="4c5b4-121">Изменение таблицы журнала миграции является мощным инструментом, но необходимо быть внимательным, чтобы не злоупотреблять его.</span><span class="sxs-lookup"><span data-stu-id="4c5b4-121">Changing the migration history table is powerful but you need to be careful to not overdo it.</span></span> <span data-ttu-id="4c5b4-122">В настоящее время среда выполнения EF не проверяет, совместима ли таблица журнала настраиваемых миграций со средой выполнения.</span><span class="sxs-lookup"><span data-stu-id="4c5b4-122">EF runtime currently does not check whether the customized migrations history table is compatible with the runtime.</span></span> <span data-ttu-id="4c5b4-123">Если это не так, приложение может прерывать работу во время выполнения или работать непредсказуемым образом.</span><span class="sxs-lookup"><span data-stu-id="4c5b4-123">If it is not your application may break at runtime or behave in unpredictable ways.</span></span> <span data-ttu-id="4c5b4-124">Это еще более важно, если для каждой базы данных используется несколько контекстов. в этом случае несколько контекстов могут использовать одну и ту же таблицу журнала миграции для хранения сведений о миграции.</span><span class="sxs-lookup"><span data-stu-id="4c5b4-124">This is even more important if you use multiple contexts per database in which case multiple contexts can use the same migration history table to store information about migrations.</span></span>

## <a name="how-to-customize-migrations-history-table"></a><span data-ttu-id="4c5b4-125">Как настроить таблицу журнала миграции?</span><span class="sxs-lookup"><span data-stu-id="4c5b4-125">How to customize Migrations History Table?</span></span>

<span data-ttu-id="4c5b4-126">Прежде чем начать, необходимо знать, что можно настроить таблицу журнала миграции только до применения первой миграции.</span><span class="sxs-lookup"><span data-stu-id="4c5b4-126">Before you start you need to know that you can customize the migrations history table only before you apply the first migration.</span></span> <span data-ttu-id="4c5b4-127">Теперь в код.</span><span class="sxs-lookup"><span data-stu-id="4c5b4-127">Now, to the code.</span></span>

<span data-ttu-id="4c5b4-128">Во-первых, необходимо создать класс, производный от класса System. Data. Entity. migrations. History. Хисториконтекст.</span><span class="sxs-lookup"><span data-stu-id="4c5b4-128">First, you will need to create a class derived from System.Data.Entity.Migrations.History.HistoryContext class.</span></span> <span data-ttu-id="4c5b4-129">Класс Хисториконтекст является производным от класса DbContext, поэтому настройка таблицы журнала миграции очень похожа на настройку моделей EF с помощью API-интерфейса Fluent.</span><span class="sxs-lookup"><span data-stu-id="4c5b4-129">The HistoryContext class is derived from the DbContext class so configuring the migrations history table is very similar to configuring EF models with fluent API.</span></span> <span data-ttu-id="4c5b4-130">Вам нужно просто переопределить метод OnModelCreating и использовать API Fluent для настройки таблицы.</span><span class="sxs-lookup"><span data-stu-id="4c5b4-130">You just need to override the OnModelCreating method and use fluent API to configure the table.</span></span>

>[!NOTE]
> <span data-ttu-id="4c5b4-131">Обычно при настройке моделей EF не нужно вызывать base. OnModelCreating () из переопределенного метода OnModelCreating, так как DbContext. OnModelCreating () имеет пустое тело.</span><span class="sxs-lookup"><span data-stu-id="4c5b4-131">Typically when you configure EF models you don’t need to call base.OnModelCreating() from the overridden OnModelCreating method since the DbContext.OnModelCreating() has empty body.</span></span> <span data-ttu-id="4c5b4-132">Это не относится к настройке таблицы журнала миграции.</span><span class="sxs-lookup"><span data-stu-id="4c5b4-132">This is not the case when configuring the migrations history table.</span></span> <span data-ttu-id="4c5b4-133">В этом случае первое, что нужно сделать в переопределении OnModelCreating (), — это фактически вызвать base. OnModelCreating ().</span><span class="sxs-lookup"><span data-stu-id="4c5b4-133">In this case the first thing to do in your OnModelCreating() override is to actually call base.OnModelCreating().</span></span> <span data-ttu-id="4c5b4-134">В этом случае таблица журнала миграции будет настроена по умолчанию, которую вы затем настроите в методе переопределения.</span><span class="sxs-lookup"><span data-stu-id="4c5b4-134">This will configure the migrations history table in the default way which you then tweak in the overriding method.</span></span>

<span data-ttu-id="4c5b4-135">Предположим, что необходимо переименовать таблицу журнала миграции и поместить ее в пользовательскую схему с именем «admin».</span><span class="sxs-lookup"><span data-stu-id="4c5b4-135">Let’s say you want to rename the migrations history table and put it to a custom schema called “admin”.</span></span> <span data-ttu-id="4c5b4-136">Кроме того, администратор базы данных хочет изменить имя столбца Мигратионид на идентификатор миграции \_ .</span><span class="sxs-lookup"><span data-stu-id="4c5b4-136">In addition your DBA would like you to rename the MigrationId column to Migration\_ID.</span></span> <span data-ttu-id="4c5b4-137">Это можно сделать, создав следующий класс, производный от Хисториконтекст:</span><span class="sxs-lookup"><span data-stu-id="4c5b4-137"> You could achieve this by creating the following class derived from HistoryContext:</span></span>

``` csharp
    using System.Data.Common;
    using System.Data.Entity;
    using System.Data.Entity.Migrations.History;

    namespace CustomizableMigrationsHistoryTableSample
    {
        public class MyHistoryContext : HistoryContext
        {
            public MyHistoryContext(DbConnection dbConnection, string defaultSchema)
                : base(dbConnection, defaultSchema)
            {
            }

            protected override void OnModelCreating(DbModelBuilder modelBuilder)
            {
                base.OnModelCreating(modelBuilder);
                modelBuilder.Entity<HistoryRow>().ToTable(tableName: "MigrationHistory", schemaName: "admin");
                modelBuilder.Entity<HistoryRow>().Property(p => p.MigrationId).HasColumnName("Migration_ID");
            }
        }
    }
```

<span data-ttu-id="4c5b4-138">После того, как пользователь Хисториконтекст готов, необходимо сделать EF его осведомленность, зарегистрировав его с помощью [конфигурации на основе кода](https://msdn.com/data/jj680699):</span><span class="sxs-lookup"><span data-stu-id="4c5b4-138">Once your custom HistoryContext is ready you need to make EF aware of it by registering it via [code-based configuration](https://msdn.com/data/jj680699):</span></span>

``` csharp
    using System.Data.Entity;

    namespace CustomizableMigrationsHistoryTableSample
    {
        public class ModelConfiguration : DbConfiguration
        {
            public ModelConfiguration()
            {
                this.SetHistoryContext("System.Data.SqlClient",
                    (connection, defaultSchema) => new MyHistoryContext(connection, defaultSchema));
            }
        }
    }
```

<span data-ttu-id="4c5b4-139">Это довольно много.</span><span class="sxs-lookup"><span data-stu-id="4c5b4-139">That’s pretty much it.</span></span> <span data-ttu-id="4c5b4-140">Теперь можно переходить к консоли диспетчера пакетов, включить и перенести, добавить-миграцию и, наконец, обновить базу данных.</span><span class="sxs-lookup"><span data-stu-id="4c5b4-140">Now you can go to the Package Manager Console, Enable-Migrations, Add-Migration and finally Update-Database.</span></span> <span data-ttu-id="4c5b4-141">Это должно привести к добавлению таблицы журнала миграции базы данных, настроенной в соответствии со сведениями, указанными в классе, производном от Хисториконтекст.</span><span class="sxs-lookup"><span data-stu-id="4c5b4-141">This should result in adding to the database a migrations history table configured according to the details you specified in your HistoryContext derived class.</span></span>

![Таблица журнала миграции](~/ef6/media/database.png)
