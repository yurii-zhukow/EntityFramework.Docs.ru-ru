---
title: Настройка таблицы журнала миграции - EF6
author: divega
ms.date: 2016-10-23
ms.assetid: ed5518f0-a9a6-454e-9e98-a4fa7748c8d0
ms.openlocfilehash: ad07b1fe97b3dafe789c0315bd555f979fc412e7
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/27/2018
ms.locfileid: "42996765"
---
# <a name="customizing-the-migrations-history-table"></a><span data-ttu-id="30534-102">Настройка таблицы журнала миграции</span><span class="sxs-lookup"><span data-stu-id="30534-102">Customizing the migrations history table</span></span>
> [!NOTE]
> <span data-ttu-id="30534-103">**Только в EF6 и более поздних версиях**. Функции, API и другие возможности, описанные на этой странице, появились в Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="30534-103">**EF6 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 6.</span></span> <span data-ttu-id="30534-104">При использовании более ранней версии могут быть неприменимы некоторые или все сведения.</span><span class="sxs-lookup"><span data-stu-id="30534-104">If you are using an earlier version, some or all of the information does not apply.</span></span>

> [!NOTE]
> <span data-ttu-id="30534-105">В этой статье предполагается, что вы знаете, как использовать Code First Migrations в основных сценариях.</span><span class="sxs-lookup"><span data-stu-id="30534-105">This article assumes you know how to use Code First Migrations in basic scenarios.</span></span> <span data-ttu-id="30534-106">Если этого не сделать, то нужно будет прочесть [Code First Migrations](~/ef6/modeling/code-first/migrations/index.md) перед продолжением.</span><span class="sxs-lookup"><span data-stu-id="30534-106">If you don’t, then you’ll need to read [Code First Migrations](~/ef6/modeling/code-first/migrations/index.md) before continuing.</span></span>

## <a name="what-is-migrations-history-table"></a><span data-ttu-id="30534-107">Что такое таблица журнала миграции?</span><span class="sxs-lookup"><span data-stu-id="30534-107">What is Migrations History Table?</span></span>

<span data-ttu-id="30534-108">Таблица журнала миграции — это таблица, используемые Code First Migrations для хранения сведений о миграции, применить к базе данных.</span><span class="sxs-lookup"><span data-stu-id="30534-108">Migrations history table is a table used by Code First Migrations to store details about migrations applied to the database.</span></span> <span data-ttu-id="30534-109">По умолчанию является имя таблицы в базе данных \_ \_MigrationHistory и он создается при применении первой do миграции базы данных.</span><span class="sxs-lookup"><span data-stu-id="30534-109">By default the name of the table in the database is \_\_MigrationHistory and it is created when applying the first migration do the database.</span></span> <span data-ttu-id="30534-110">В Entity Framework 5 эта таблица была системную таблицу, если в приложении используется база данных Microsoft Sql Server.</span><span class="sxs-lookup"><span data-stu-id="30534-110">In Entity Framework 5 this table was a system table if the application used Microsoft Sql Server database.</span></span> <span data-ttu-id="30534-111">Это изменилось в Entity Framework 6, но и таблице журнала миграции больше не помечена системную таблицу.</span><span class="sxs-lookup"><span data-stu-id="30534-111">This has changed in Entity Framework 6 however and the migrations history table is no longer marked a system table.</span></span>

## <a name="why-customize-migrations-history-table"></a><span data-ttu-id="30534-112">Зачем настраивать таблицы журнала миграции?</span><span class="sxs-lookup"><span data-stu-id="30534-112">Why customize Migrations History Table?</span></span>

<span data-ttu-id="30534-113">Таблица журнала миграции должен использоваться исключительно с Code First Migrations и менять ее вручную можно нарушить миграций.</span><span class="sxs-lookup"><span data-stu-id="30534-113">Migrations history table is supposed to be used solely by Code First Migrations and changing it manually can break migrations.</span></span> <span data-ttu-id="30534-114">Тем не менее иногда конфигурация по умолчанию не подходит, и таблицы необходимо выполнить настройку, например:</span><span class="sxs-lookup"><span data-stu-id="30534-114">However sometimes the default configuration is not suitable and the table needs to be customized, for instance:</span></span>

-   <span data-ttu-id="30534-115">Необходимо изменить имена и/или аспекты столбцов, чтобы включить 3<sup>к удаленному рабочему столу</sup> миграций стороннего поставщика</span><span class="sxs-lookup"><span data-stu-id="30534-115">You need to change names and/or facets of the columns to enable a 3<sup>rd</sup> party Migrations provider</span></span>
-   <span data-ttu-id="30534-116">Чтобы изменить имя таблицы</span><span class="sxs-lookup"><span data-stu-id="30534-116">You want to change the name of the table</span></span>
-   <span data-ttu-id="30534-117">Необходимо использовать схему не по умолчанию для \_ \_MigrationHistory таблицы</span><span class="sxs-lookup"><span data-stu-id="30534-117">You need to use a non-default schema for the \_\_MigrationHistory table</span></span>
-   <span data-ttu-id="30534-118">Вам необходимо сохранить дополнительные данные для данной версии контекста, и поэтому необходимо добавить дополнительный столбец в таблицу</span><span class="sxs-lookup"><span data-stu-id="30534-118">You need to store additional data for a given version of the context and therefore you need to add an additional column to the table</span></span>

## <a name="words-of-precaution"></a><span data-ttu-id="30534-119">Меры предосторожности слов</span><span class="sxs-lookup"><span data-stu-id="30534-119">Words of precaution</span></span>

<span data-ttu-id="30534-120">Изменение таблицы журнала миграции обладает широкими возможностями, но вам нужно соблюдать осторожность, чтобы не злоупотреблять.</span><span class="sxs-lookup"><span data-stu-id="30534-120">Changing the migration history table is powerful but you need to be careful to not overdo it.</span></span> <span data-ttu-id="30534-121">В настоящее время среда выполнения EF не проверяет, совместим ли таблицы журнала настроенный миграции в среде выполнения.</span><span class="sxs-lookup"><span data-stu-id="30534-121">EF runtime currently does not check whether the customized migrations history table is compatible with the runtime.</span></span> <span data-ttu-id="30534-122">Если приложение не может прервать во время выполнения или ведут себя непредсказуемым образом.</span><span class="sxs-lookup"><span data-stu-id="30534-122">If it is not your application may break at runtime or behave in unpredictable ways.</span></span> <span data-ttu-id="30534-123">Это еще более важно, если вы используете несколько контекстов на одну базу данных в этом случае несколько контекстов можно использовать одну прежнюю таблицу миграции для хранения сведений о миграции.</span><span class="sxs-lookup"><span data-stu-id="30534-123">This is even more important if you use multiple contexts per database in which case multiple contexts can use the same migration history table to store information about migrations.</span></span>

## <a name="how-to-customize-migrations-history-table"></a><span data-ttu-id="30534-124">Как настроить таблицу журнала миграции?</span><span class="sxs-lookup"><span data-stu-id="30534-124">How to customize Migrations History Table?</span></span>

<span data-ttu-id="30534-125">Перед началом необходимо знать, в таблицу журнала миграции можно настраивать только в том случае, прежде чем применить первую миграцию.</span><span class="sxs-lookup"><span data-stu-id="30534-125">Before you start you need to know that you can customize the migrations history table only before you apply the first migration.</span></span> <span data-ttu-id="30534-126">Теперь чтобы код.</span><span class="sxs-lookup"><span data-stu-id="30534-126">Now, to the code.</span></span>

<span data-ttu-id="30534-127">Во-первых необходимо создать класс, производный от класса System.Data.Entity.Migrations.History.HistoryContext.</span><span class="sxs-lookup"><span data-stu-id="30534-127">First, you will need to create a class derived from System.Data.Entity.Migrations.History.HistoryContext class.</span></span> <span data-ttu-id="30534-128">Класс HistoryContext является производным от класса DbContext, поэтому Настройка таблицы журнала миграции не очень похожа на настройку модели EF с помощью текучего API.</span><span class="sxs-lookup"><span data-stu-id="30534-128">The HistoryContext class is derived from the DbContext class so configuring the migrations history table is very similar to configuring EF models with fluent API.</span></span> <span data-ttu-id="30534-129">Нужно переопределить метод OnModelCreating и используйте fluent API для настройки в таблице.</span><span class="sxs-lookup"><span data-stu-id="30534-129">You just need to override the OnModelCreating method and use fluent API to configure the table.</span></span>

>[!NOTE]
> <span data-ttu-id="30534-130">Обычно при настройке модели EF не нужно вызывать базовый. OnModelCreating() из переопределенного метода OnModelCreating, так как DbContext.OnModelCreating() имеет пустой текст.</span><span class="sxs-lookup"><span data-stu-id="30534-130">Typically when you configure EF models you don’t need to call base.OnModelCreating() from the overridden OnModelCreating method since the DbContext.OnModelCreating() has empty body.</span></span> <span data-ttu-id="30534-131">Это условие не выполняется, при настройке миграции таблицы журнала.</span><span class="sxs-lookup"><span data-stu-id="30534-131">This is not the case when configuring the migrations history table.</span></span> <span data-ttu-id="30534-132">В данном случае первое, что необходимо сделать в переопределении OnModelCreating() является фактически вызов базового. OnModelCreating().</span><span class="sxs-lookup"><span data-stu-id="30534-132">In this case the first thing to do in your OnModelCreating() override is to actually call base.OnModelCreating().</span></span> <span data-ttu-id="30534-133">Это настроит таблице журнала миграции в метод по умолчанию, что вы примените в переопределяющем методе.</span><span class="sxs-lookup"><span data-stu-id="30534-133">This will configure the migrations history table in the default way which you then tweak in the overriding method.</span></span>

<span data-ttu-id="30534-134">Предположим, что вы хотите переименовать таблицу журнала миграции и поместите его на пользовательских схема с именем «admin».</span><span class="sxs-lookup"><span data-stu-id="30534-134">Let’s say you want to rename the migrations history table and put it to a custom schema called “admin”.</span></span> <span data-ttu-id="30534-135">В дополнение к этому Администратору просят переименуйте столбец MigrationId миграции\_идентификатор.</span><span class="sxs-lookup"><span data-stu-id="30534-135">In addition your DBA would like you to rename the MigrationId column to Migration\_ID.</span></span>  <span data-ttu-id="30534-136">Это достигается путем создания следующий класс, производный от HistoryContext:</span><span class="sxs-lookup"><span data-stu-id="30534-136">You could achieve this by creating the following class derived from HistoryContext:</span></span>

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

<span data-ttu-id="30534-137">После подготовки вашего пользовательского HistoryContext необходимо сообщить EF его, зарегистрировав его с помощью [конфигурация на основе кода](http://msdn.com/data/jj680699):</span><span class="sxs-lookup"><span data-stu-id="30534-137">Once your custom HistoryContext is ready you need to make EF aware of it by registering it via [code-based configuration](http://msdn.com/data/jj680699):</span></span>

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

<span data-ttu-id="30534-138">Это равносильно использованию его.</span><span class="sxs-lookup"><span data-stu-id="30534-138">That’s pretty much it.</span></span> <span data-ttu-id="30534-139">Теперь вы можете перейти в консоль диспетчера пакетов, Enable-Migrations, Add-Migration и, наконец, обновления базы данных.</span><span class="sxs-lookup"><span data-stu-id="30534-139">Now you can go to the Package Manager Console, Enable-Migrations, Add-Migration and finally Update-Database.</span></span> <span data-ttu-id="30534-140">Это должно привести к появлению добавления в базу данных в таблицу журнала миграции настраиваются в соответствии с данные, которые вы указали в HistoryContext производного класса.</span><span class="sxs-lookup"><span data-stu-id="30534-140">This should result in adding to the database a migrations history table configured according to the details you specified in your HistoryContext derived class.</span></span>

![База данных](~/ef6/media/database.png)
