---
title: Настройка таблицы журналов миграции — EF6
description: Настройка таблицы журнала миграции в Entity Framework 6
author: ajcvickers
ms.date: 10/23/2016
uid: ef6/modeling/code-first/migrations/history-customization
ms.openlocfilehash: f0f474507659d5fbe43ecf2ed7216fb5d1a86779
ms.sourcegitcommit: 0a25c03fa65ae6e0e0e3f66bac48d59eceb96a5a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/14/2020
ms.locfileid: "92066399"
---
# <a name="customizing-the-migrations-history-table"></a>Настройка таблицы журнала миграции
> [!NOTE]
> **Только в EF6 и более поздних версиях**. Функции, API и другие возможности, описанные на этой странице, появились в Entity Framework 6. При использовании более ранней версии могут быть неприменимы некоторые или все сведения.

> [!NOTE]
> В этой статье предполагается, что вы умеете использовать Code First Migrations в основных сценариях. В противном случае необходимо прочитать [Code First migrations](xref:ef6/modeling/code-first/migrations/index) , прежде чем продолжить.

## <a name="what-is-migrations-history-table"></a>Что такое таблица журнала миграции?

Таблица журнала миграции — это таблица, используемая Code First Migrations для хранения сведений о миграции, примененных к базе данных. По умолчанию имя таблицы в базе данных — \_ \_ мигратионхистори и создается при применении первой миграции к базе данных. В Entity Framework 5 Эта таблица была системной таблицей, если приложение использовало базу данных Microsoft SQL Server. Это изменилось в Entity Framework 6, но таблица журнала миграции больше не помечена системной таблицей.

## <a name="why-customize-migrations-history-table"></a>Зачем настраивать таблицу журнала миграции?

Таблица журнала миграции должна использоваться исключительно Code First Migrations и изменить ее вручную может привести к нарушению миграции. Однако иногда конфигурация по умолчанию не подходит и таблица должна быть настроена, например:

-   Необходимо изменить имена и (или) аспекты столбцов, чтобы включить 3 поставщика миграции субъектов<sup>удаленных рабочих столов</sup>
-   Необходимо изменить имя таблицы
-   Для \_ таблицы мигратионхистори необходимо использовать схему, не используемую по умолчанию \_
-   Необходимо сохранить дополнительные данные для данной версии контекста, поэтому необходимо добавить дополнительный столбец в таблицу.

## <a name="words-of-precaution"></a>Слова предосторожности

Изменение таблицы журнала миграции является мощным инструментом, но необходимо быть внимательным, чтобы не злоупотреблять его. В настоящее время среда выполнения EF не проверяет, совместима ли таблица журнала настраиваемых миграций со средой выполнения. Если это не так, приложение может прерывать работу во время выполнения или работать непредсказуемым образом. Это еще более важно, если для каждой базы данных используется несколько контекстов. в этом случае несколько контекстов могут использовать одну и ту же таблицу журнала миграции для хранения сведений о миграции.

## <a name="how-to-customize-migrations-history-table"></a>Как настроить таблицу журнала миграции?

Прежде чем начать, необходимо знать, что можно настроить таблицу журнала миграции только до применения первой миграции. Теперь в код.

Во-первых, необходимо создать класс, производный от класса System. Data. Entity. migrations. History. Хисториконтекст. Класс Хисториконтекст является производным от класса DbContext, поэтому настройка таблицы журнала миграции очень похожа на настройку моделей EF с помощью API-интерфейса Fluent. Вам нужно просто переопределить метод OnModelCreating и использовать API Fluent для настройки таблицы.

>[!NOTE]
> Обычно при настройке моделей EF не нужно вызывать base. OnModelCreating () из переопределенного метода OnModelCreating, так как DbContext. OnModelCreating () имеет пустое тело. Это не относится к настройке таблицы журнала миграции. В этом случае первое, что нужно сделать в переопределении OnModelCreating (), — это фактически вызвать base. OnModelCreating (). В этом случае таблица журнала миграции будет настроена по умолчанию, которую вы затем настроите в методе переопределения.

Предположим, что необходимо переименовать таблицу журнала миграции и поместить ее в пользовательскую схему с именем «admin». Кроме того, администратор базы данных хочет изменить имя столбца Мигратионид на идентификатор миграции \_ . Это можно сделать, создав следующий класс, производный от Хисториконтекст:

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

После того, как пользователь Хисториконтекст готов, необходимо сделать EF его осведомленность, зарегистрировав его с помощью [конфигурации на основе кода](https://msdn.com/data/jj680699):

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

Это довольно много. Теперь можно открыть консоль диспетчера пакетов, включить или перенести, Add-Migration и, наконец, обновить базу данных. Это должно привести к добавлению таблицы журнала миграции базы данных, настроенной в соответствии со сведениями, указанными в классе, производном от Хисториконтекст.

![Таблица журнала миграции](~/ef6/media/database.png)
