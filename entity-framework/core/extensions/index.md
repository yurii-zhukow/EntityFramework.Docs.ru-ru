---
title: Инструменты и расширения — EF Core
description: Внешние средства и расширения для Entity Framework Core
author: ErikEJ
ms.date: 02/21/2021
uid: core/extensions/index
ms.openlocfilehash: 67d078250d8bfdb845cf98949ee1412a8fe842bd
ms.sourcegitcommit: 4798ab8d04c1fdbe6dd204d94d770fcbf309d09b
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/11/2021
ms.locfileid: "103024385"
---
# <a name="ef-core-tools--extensions"></a>Инструменты и расширения EF Core

Эти инструменты и расширения предоставляют дополнительные возможности для Entity Framework Core 2.1 и более поздних версий.

> [!IMPORTANT]
> Расширения создаются с помощью различных источников и не поддерживаются в рамках проекта Entity Framework Core. Выбирая стороннее расширение, обязательно оцените его качество, лицензирование, совместимость, поддержку и другие показатели на соответствие вашим требованиям. В частности, для расширения, созданного для более ранней версии EF Core, может потребоваться обновление, прежде чем это расширение будет работать с последними версиями.

## <a name="tools"></a>Инструменты

### <a name="llblgen-pro"></a>LLBLGen Pro

LLBLGen Pro — решение для моделирования сущностей с поддержкой Entity Framework и Entity Framework Core. Оно позволяет легко определить модель сущности и сопоставить ее с базой данных с помощью подходов Database-First (сначала база данных) или Model-First (сначала модель), таким образом, вы сможете сразу приступить к написанию запросов. Для EF Core: 2, 3.

[Веб-сайт](https://www.llblgen.com/)

### <a name="devart-entity-developer"></a>Devart Entity Developer

Entity Developer — мощный конструктор O/RM для ADO.NET Entity Framework, NHibernate, LinqConnect, Telerik Data Access и LINQ to SQL. Он поддерживает визуальное проектирование моделей EF Core с использованием подходов Model First и Database First и создание кода C# или Visual Basic. Для EF Core: 2, 3, 5.

[Веб-сайт](https://www.devart.com/entitydeveloper/)

### <a name="nhydrate-orm-for-entity-framework"></a>Использование nHydrate ORM для Entity Framework

O/RM создает строго типизированные расширяемые классы для Entity Framework. Созданный код представляет собой Entity Framework Core. Различий при этом нет. Это не замена EF или пользовательского O/RM. Это визуальный слой моделирования, который позволяет команде управлять сложными схемами баз данных. Он поддерживает программное обеспечение SCM, например Git, обеспечивая доступ многих пользователей к модели с минимальными конфликтами. Установщик отслеживает изменения модели и создает скрипты обновления. Для EF Core: 3.

[Репозиторий GitHub](https://github.com/nHydrate/nHydrate)

### <a name="ef-core-power-tools"></a>EF Core Power Tools

EF Core Power Tools — это расширение Visual Studio, которое позволяет выполнять различные задачи разработки EF Core в простом пользовательском интерфейсе. Оно включает возможности реконструирования DbContext и классов сущностей из существующих баз данных и [файлов DACPAC SQL Server](/sql/relational-databases/data-tier-applications/data-tier-applications), управления миграцией баз данных и визуализации моделей. Для EF Core: 3, 5.

[Вики-сайт GitHub](https://github.com/ErikEJ/EFCorePowerTools/wiki)

### <a name="entity-framework-visual-editor"></a>Редактор Visual Entity Framework

Редактор Visual Entity Framework — это расширение Visual Studio, которое добавляет конструктор O/RM для визуального проектирования классов EF 6 и EF Core. Код создается с помощью шаблонов T4, поэтому его можно адаптировать к любым потребностям. Поддерживаются наследование, однонаправленные и двунаправленные ассоциации, перечисления и возможность цветового выделения классов и добавления текстовых блоков для объяснения потенциально сложных частей проекта. Для EF Core: 2, 3, 5.

[Marketplace](https://marketplace.visualstudio.com/items?itemName=michaelsawczyn.EFDesigner)

### <a name="catfactory"></a>CatFactory

CatFactory — это механизм формирования шаблонов для .NET Core, который позволяет автоматизировать создание классов DbContext, сущностей, сопоставление конфигураций и создание классов репозиториев из базы данных SQL Server. Для EF Core: 2.

[Репозиторий GitHub](https://github.com/hherzl/CatFactory.EntityFrameworkCore)

### <a name="loresofts-entity-framework-core-generator"></a>Entity Framework Core Generator от LoreSoft

Entity Framework Core Generator (efg) — это средство командной строки .NET Core, которое позволяет создавать модели EF Core на основе существующей базы данных, как и `dotnet ef dbcontext scaffold`, но также поддерживает безопасное [повторное создание кода](https://efg.loresoft.com/en/latest/regeneration/) путем замены регионов или путем синтаксического анализа файлов сопоставления. Средство поддерживает формирование моделей представлений, проверку и код средства сопоставления объектов. Для EF Core: 2.

[Руководство](https://www.loresoft.com/Generate-ASP-NET-Web-API)
[Документация](https://efg.loresoft.com/en/latest/)

## <a name="extensions"></a>Расширения

### <a name="microsoftentityframeworkcoreautohistory"></a>Microsoft.EntityFrameworkCore.AutoHistory

Библиотека подключаемых модулей, которая позволяет автоматически записывать изменения данных, выполняемые EF Core, в таблицу журнала. Для EF Core: 2, 3.

[Репозиторий GitHub](https://github.com/Arch/AutoHistory/)

### <a name="efcoresecondlevelcacheinterceptor"></a>EFCoreSecondLevelCacheInterceptor

Кэширование второго уровня — это кэширование запросов. Результаты команд EF будут храниться в кэше, чтобы такие же команды EF получали данные из кэша, а не выполнялись в базе данных еще раз. Для EF Core: 3, 5.

[Репозиторий GitHub](https://github.com/VahidN/EFCoreSecondLevelCacheInterceptor)

### <a name="geco"></a>Geco

Geco (Generator Console) — простой генератор кода на основе консольного проекта, который работает в .NET Core и использует интерполированные строки C# для создания кода. Geco включает генератор обратной модели для EF Core с поддержкой преобразования во множественную форму, единичную форму и с поддержкой редактируемых шаблонов. Он также предоставляет генератор сценариев начальных данных, средство запуска сценариев и средство очистки баз данных. Для EF Core: 2.

[Репозиторий GitHub](https://github.com/iQuarc/Geco)

### <a name="entityframeworkcorescaffoldinghandlebars"></a>EntityFrameworkCore.Scaffolding.Handlebars

Позволяет настраивать классы, реконструированные из существующей базы данных с помощью цепочки инструментов Entity Framework Core с шаблонами Handlebars. Для EF Core: 2, 3, 5.

[Репозиторий GitHub](https://github.com/TrackableEntities/EntityFrameworkCore.Scaffolding.Handlebars)

### <a name="neinlinqentityframeworkcore"></a>NeinLinq.EntityFrameworkCore

NeinLinq расширяет возможности поставщиков LINQ, таких как Entity Framework, позволяя повторно использовать функции, переписывать запросы и создавать динамические запросы с использованием транслируемых предикатов и селекторов. Для EF Core: 2, 3, 5.

[Репозиторий GitHub](https://github.com/axelheer/nein-linq/)

### <a name="microsoftentityframeworkcoreunitofwork"></a>Microsoft.EntityFrameworkCore.UnitOfWork

Подключаемый модуль Microsoft.EntityFrameworkCore для поддержки репозитория, шаблонов Unit of Work и нескольких баз данных с поддержкой распределенных транзакций. Для EF Core: 2, 3.

[Репозиторий GitHub](https://github.com/Arch/UnitOfWork/)

### <a name="efcorebulkextensions"></a>EFCore.BulkExtensions

Расширения EF Core для массовых операций (Insert, Update, Delete). Для EF Core: 2, 3.

[Репозиторий GitHub](https://github.com/borisdj/EFCore.BulkExtensions)

### <a name="bricelamentityframeworkcorepluralizer"></a>Bricelam.EntityFrameworkCore.Pluralizer

Добавляет преобразование во множественную форму для времени разработки. Для EF Core: 2, 3.

[Репозиторий GitHub](https://github.com/bricelam/EFCore.Pluralizer)

### <a name="toolbeltentityframeworkcoreindexattribute"></a>Toolbelt.EntityFrameworkCore.IndexAttribute

Новая реализация атрибута [Index] (с расширением для создания моделей). Для EF Core: 2, 3, 5.

[Репозиторий GitHub](https://github.com/jsakamoto/EntityFrameworkCore.IndexAttribute)

### <a name="verifyentityframework"></a>Verify.EntityFramework

Расширяет возможности [Verify](https://github.com/VerifyTests/Verify), чтобы разрешить тестирование моментальных снимков с помощью Entity Framework. Для EF Core: 3, 5.

[Репозиторий GitHub](https://github.com/VerifyTests/Verify.EntityFramework)

### <a name="localdb"></a>LocalDB

Предоставляет оболочку для [SQL Server Express LocalDB](https://docs.microsoft.com/sql/database-engine/configure-windows/sql-server-express-localdb), позволяющую упростить выполнение тестов в Entity Framework. Для EF Core: 3, 5.

[Репозиторий GitHub](https://github.com/SimonCropp/LocalDb)

### <a name="effluentvalidation"></a>EfFluentValidation

Добавляет поддержку [FluentValidation](https://fluentvalidation.net/) в Entity Framework. Для EF Core: 3, 5.

[Репозиторий GitHub](https://github.com/SimonCropp/EfFluentValidation)

### <a name="efcoretemporalsupport"></a>EFCore.TemporalSupport

Реализация поддержки темпоральных операций. Для EF Core: 2.

[Репозиторий GitHub](https://github.com/cpoDesign/EFCore.TemporalSupport)

### <a name="efcoretemporaltable"></a>EfCoreTemporalTable

Простое выполнение темпоральных запросов к избранной базе данных с помощью представленных методов расширения: `AsTemporalAll()`, `AsTemporalAsOf(date)`, `AsTemporalFrom(startDate, endDate)`, `AsTemporalBetween(startDate, endDate)`, `AsTemporalContained(startDate, endDate)`. Для EF Core: 3.

[Репозиторий GitHub](https://github.com/glautrou/EfCoreTemporalTable)

### <a name="entityframeworkcoretemporaltables"></a>EntityFrameworkCore.TemporalTables

Библиотека расширений для Entity Framework Core, которая позволяет разработчикам, работающим с SQL Server, легко использовать темпоральные таблицы. Для EF Core: 2, 3.

[Репозиторий GitHub](https://github.com/findulov/EntityFrameworkCore.TemporalTables)

### <a name="entityframeworkcorecacheable"></a>EntityFrameworkCore.Cacheable

Высокопроизводительный кэш запросов второго уровня. Для EF Core: 2.

[Репозиторий GitHub](https://github.com/SteffenMangold/EntityFrameworkCore.Cacheable)

### <a name="entityframeworkcorencache"></a>EntityFrameworkCore.NCache

Поставщик Entity Framework Core NCache является распределенным поставщиком кэша второго уровня для кэширования результатов запросов. Распределенная архитектура NCache повышает уровень его масштабируемости и высокой доступности. Для EF Core 2, 3.

[Веб-сайт](https://www.alachisoft.com/ncache/ef-core-cache.html)

### <a name="entityframeworkcoretriggered"></a>EntityFrameworkCore.Triggered

Триггеры для EF Core. Реагирование на изменения в DbContext до и после фиксации этих изменений в базе данных. Триггеры полностью асинхронны и поддерживают внедрение зависимостей, наследование, каскадирование и другие возможности. Для EF Core: 3, 5.

[Репозиторий GitHub](https://github.com/koenbeuk/EntityFrameworkCore.Triggered)

### <a name="entity-framework-plus"></a>Entity Framework Plus

Расширены возможности DbContext за счет таких функций, как добавление фильтра, аудит, кэширование, пакетная обработка будущих запросов, пакетное удаление, пакетное обновление и многое другое. Для EF Core: 2, 3, 5.

[Веб-сайт](https://entityframework-plus.net/)
[Репозиторий GitHub](https://github.com/zzzprojects/EntityFramework-Plus)

### <a name="entity-framework-extensions"></a>Расширения Entity Framework

Расширены возможности DbContext за счет таких высокопроизводительных пакетных операций, как BulkSaveChanges, BulkInsert, BulkUpdate, BulkDelete, BulkMerge и других. Для EF Core: 2, 3, 5.

[Веб-сайт](https://entityframework-extensions.net/)

### <a name="expressionify"></a>Expressionify

Добавляет поддержку вызова методов расширения в лямбда-выражениях LINQ. Для EF Core: 3.

[Репозиторий GitHub](https://github.com/ClaveConsulting/Expressionify)

### <a name="elinq"></a>ELinq

Технология языка интегрированных запросов (LINQ) для реляционных баз данных. Позволяет писать строго типизированные запросы на C#. Для EF Core: 3.

- Полная поддержка C# для создания запросов: лямбда-выражения с множеством инструкций, переменные, функции и т. д.
- Отсутствие семантического разрыва с SQL. XLinq объявляет инструкции SQL (такие как `SELECT`, `FROM`, `WHERE`) как методы первого класса C#, сочетая привычный синтаксис с IntelliSense, безопасностью типов и рефакторингом.

В результате SQL становится просто еще одной библиотекой классов, предоставляющей свой API локально, буквально являясь *"языком интегрированного SQL"* .

[Веб-сайт](https://entitylinq.com/)

### <a name="ramses"></a>Ramses

Перехватчики жизненного цикла (для SaveChanges). Для EF Core: 2, 3.

[Репозиторий GitHub](https://github.com/JValck/Ramses)

### <a name="efcorenamingconventions"></a>EFCore.NamingConventions

Автоматически преобразует имена всех таблиц и столбцов для использования "змеиного_регистра", ВСЕХ ПРОПИСНЫХ или всех строчных букв. Для EF Core: 3.

[Репозиторий GitHub](https://github.com/efcore/EFCore.NamingConventions)

### <a name="simplersoftwareentityframeworkcoresqlservernodatime"></a>SimplerSoftware.EntityFrameworkCore.SqlServer.NodaTime

Добавляет встроенную поддержку типов NodaTime в EntityFrameworkCore для SQL Server. Для EF Core: 3, 5.

[Репозиторий GitHub](https://github.com/StevenRasmussen/EFCore.SqlServer.NodaTime)

### <a name="dabbleentityframeworkcoretemporalquery"></a>Dabble.EntityFrameworkCore.Temporal.Query

LINQ-расширения Entity Framework Core 3.1 для поддержки запросов во временных таблицах Microsoft SQL Server. Для EF Core: 3.

[Репозиторий GitHub](https://github.com/Adam-Langley/efcore-temporal-query)

### <a name="entityframeworkcoresqlserverhierarchyid"></a>EntityFrameworkCore.SqlServer.HierarchyId

Добавляет поддержку hierarchyid в поставщик SQL Server EF Core. Для EF Core: 3.

[Репозиторий GitHub](https://github.com/efcore/EFCore.SqlServer.HierarchyId)

### <a name="linq2dbentityframeworkcore"></a>linq2db.EntityFrameworkCore

Альтернативный переводчик запросов LINQ к выражениям SQL. Для EF Core: 3, 5.

Включает поддержку расширенных функций SQL, таких как выражения CTE, массовое копирование, табличные указания, оконные функции, временные таблицы и операции создания, обновления и удаления на стороне базы данных.

[Репозиторий GitHub](https://github.com/linq2db/linq2db.EntityFrameworkCore)

### <a name="efcoresoftdelete"></a>EFCore.SoftDelete

Реализация для обратимого удаления сущностей. Для EF Core: 3.

[NuGet](https://www.nuget.org/packages/EFCore.SoftDelete)

### <a name="entityframeworkcoreconfigurationmanager"></a>EntityFrameworkCore.ConfigurationManager

Расширяет возможности EF Core для разрешения строк подключения из файла App.config. Для EF Core: 3.

[Репозиторий GitHub](https://github.com/efcore/EFCore.ConfigurationManager)

### <a name="detached-mapper"></a>Detached Mapper

Средство сопоставления DTO-Entity с обработкой композиции/агрегирования (аналогично GraphDiff). Для EF Core: 3, 5.

[NuGet](https://www.nuget.org/packages/Detached.Mappers.EntityFramework)

### <a name="entityframeworkcoresqlitenodatime"></a>EntityFrameworkCore.Sqlite.NodaTime

Добавляет поддержку типов [NodaTime](https://nodatime.org) при использовании [SQLite](https://sqlite.org). Для EF Core: 5.

[Репозиторий GitHub](https://github.com/khellang/EFCore.Sqlite.NodaTime)

### <a name="erikejentityframeworkcoresqlserverdacpac"></a>ErikEJ.EntityFrameworkCore.SqlServer.Dacpac

Обеспечивает реконструирование модели EF Core из пакета приложения уровня данных SQL Server (.dacpac). Для EF Core: 3, 5.

[Вики-сайт GitHub](https://github.com/ErikEJ/EFCorePowerTools/wiki/ErikEJ.EntityFrameworkCore.SqlServer.Dacpac)

### <a name="erikejentityframeworkcoredgmlbuilder"></a>ErikEJ.EntityFrameworkCore.DgmlBuilder

Создает содержимое DGML (граф), которое визуализирует DbContext. Добавляет метод расширения AsDgml() в класс DbContext. Для EF Core: 3, 5.

[Вики-сайт GitHub](https://github.com/ErikEJ/EFCorePowerTools/wiki/Inspect-your-DbContext-model)

### <a name="entityframeworkexceptions"></a>EntityFramework.Exceptions

В Entity Framework Core все исключения базы данных упаковываются в DbUpdateException. EntityFramework.Exceptions обрабатывает все сведения о базе данных, чтобы определить нарушенное ограничение, и позволяет применять типизированные исключения, например `UniqueConstraintException`, `CannotInsertNullException`, `MaxLengthExceededException`, `NumericOverflowException`, `ReferenceConstraintException`, если запрос нарушает ограничения базы данных.

Поддерживает SQL Server, Postgres, MySql, SQLite и Oracle.

Для EF Core: 3, 5.

[Репозиторий GitHub](https://github.com/Giorgi/EntityFramework.Exceptions)
