---
title: Настройка таблицы журнала миграции - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: ed5518f0-a9a6-454e-9e98-a4fa7748c8d0
ms.openlocfilehash: e3faefc4b812ec4bc440ed2bb48747053d8cb1b3
ms.sourcegitcommit: 269c8a1a457a9ad27b4026c22c4b1a76991fb360
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/18/2018
ms.locfileid: "46283697"
---
# <a name="customizing-the-migrations-history-table"></a>Настройка таблицы журнала миграции
> [!NOTE]
> **Только в EF6 и более поздних версиях**. Функции, API и другие возможности, описанные на этой странице, появились в Entity Framework 6. При использовании более ранней версии могут быть неприменимы некоторые или все сведения.

> [!NOTE]
> В этой статье предполагается, что вы знаете, как использовать Code First Migrations в основных сценариях. Если этого не сделать, то нужно будет прочесть [Code First Migrations](~/ef6/modeling/code-first/migrations/index.md) перед продолжением.

## <a name="what-is-migrations-history-table"></a>Что такое таблица журнала миграции?

Таблица журнала миграции — это таблица, используемые Code First Migrations для хранения сведений о миграции, применить к базе данных. По умолчанию является имя таблицы в базе данных \_ \_MigrationHistory и он создается при применении первой do миграции базы данных. В Entity Framework 5 эта таблица была системную таблицу, если в приложении используется база данных Microsoft Sql Server. Это изменилось в Entity Framework 6, но и таблице журнала миграции больше не помечена системную таблицу.

## <a name="why-customize-migrations-history-table"></a>Зачем настраивать таблицы журнала миграции?

Таблица журнала миграции должен использоваться исключительно с Code First Migrations и менять ее вручную можно нарушить миграций. Тем не менее иногда конфигурация по умолчанию не подходит, и таблицы необходимо выполнить настройку, например:

-   Необходимо изменить имена и/или аспекты столбцов, чтобы включить 3<sup>к удаленному рабочему столу</sup> миграций стороннего поставщика
-   Чтобы изменить имя таблицы
-   Необходимо использовать схему не по умолчанию для \_ \_MigrationHistory таблицы
-   Вам необходимо сохранить дополнительные данные для данной версии контекста, и поэтому необходимо добавить дополнительный столбец в таблицу

## <a name="words-of-precaution"></a>Меры предосторожности слов

Изменение таблицы журнала миграции обладает широкими возможностями, но вам нужно соблюдать осторожность, чтобы не злоупотреблять. В настоящее время среда выполнения EF не проверяет, совместим ли таблицы журнала настроенный миграции в среде выполнения. Если приложение не может прервать во время выполнения или ведут себя непредсказуемым образом. Это еще более важно, если вы используете несколько контекстов на одну базу данных в этом случае несколько контекстов можно использовать одну прежнюю таблицу миграции для хранения сведений о миграции.

## <a name="how-to-customize-migrations-history-table"></a>Как настроить таблицу журнала миграции?

Перед началом необходимо знать, в таблицу журнала миграции можно настраивать только в том случае, прежде чем применить первую миграцию. Теперь чтобы код.

Во-первых необходимо создать класс, производный от класса System.Data.Entity.Migrations.History.HistoryContext. Класс HistoryContext является производным от класса DbContext, поэтому Настройка таблицы журнала миграции не очень похожа на настройку модели EF с помощью текучего API. Нужно переопределить метод OnModelCreating и используйте fluent API для настройки в таблице.

>[!NOTE]
> Обычно при настройке модели EF не нужно вызывать базовый. OnModelCreating() из переопределенного метода OnModelCreating, так как DbContext.OnModelCreating() имеет пустой текст. Это условие не выполняется, при настройке миграции таблицы журнала. В данном случае первое, что необходимо сделать в переопределении OnModelCreating() является фактически вызов базового. OnModelCreating(). Это настроит таблице журнала миграции в метод по умолчанию, что вы примените в переопределяющем методе.

Предположим, что вы хотите переименовать таблицу журнала миграции и поместите его на пользовательских схема с именем «admin». В дополнение к этому Администратору просят переименуйте столбец MigrationId миграции\_идентификатор.  Это достигается путем создания следующий класс, производный от HistoryContext:

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

После подготовки вашего пользовательского HistoryContext необходимо сообщить EF его, зарегистрировав его с помощью [конфигурация на основе кода](https://msdn.com/data/jj680699):

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

Это равносильно использованию его. Теперь вы можете перейти в консоль диспетчера пакетов, Enable-Migrations, Add-Migration и, наконец, обновления базы данных. Это должно привести к появлению добавления в базу данных в таблицу журнала миграции настраиваются в соответствии с данные, которые вы указали в HistoryContext производного класса.

![База данных](~/ef6/media/database.png)
