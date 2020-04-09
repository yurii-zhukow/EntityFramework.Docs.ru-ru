---
title: Инструменты и расширения — EF Core
author: ErikEJ
ms.date: 12/17/2019
ms.assetid: 14fffb6c-a687-4881-a094-af4a1359a296
uid: core/extensions/index
ms.openlocfilehash: e3806f7161fecfe66450d3e08f97caf3d2c84cf3
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/07/2020
ms.locfileid: "80634242"
---
# <a name="ef-core-tools--extensions"></a>Инструменты и расширения EF Core

Эти инструменты и расширения предоставляют дополнительные возможности для Entity Framework Core 2.1 и более поздних версий.

> [!IMPORTANT]  
> Расширения создаются с помощью различных источников и не поддерживаются в рамках проекта Entity Framework Core. Выбирая стороннее расширение, обязательно оцените его качество, лицензирование, совместимость, поддержку и другие показатели на соответствие вашим требованиям. В частности, для расширения, созданного для более ранней версии EF Core, может потребоваться обновление, прежде чем это расширение будет работать с последними версиями.

## <a name="tools"></a>Инструменты

### <a name="llblgen-pro"></a>LLBLGen Pro

LLBLGen Pro — решение для моделирования сущностей с поддержкой Entity Framework и Entity Framework Core. Оно позволяет легко определить модель сущности и сопоставить ее с базой данных с помощью подходов Database-First (сначала база данных) или Model-First (сначала модель), таким образом, вы сможете сразу приступить к написанию запросов. Для EF Core: 2.

[Веб-сайт](https://www.llblgen.com/)

### <a name="devart-entity-developer"></a>Devart Entity Developer

Entity Developer — мощный конструктор ORM для ADO.NET Entity Framework, NHibernate, LinqConnect, Telerik Data Access и LINQ to SQL. Он поддерживает визуальное проектирование моделей EF Core с использованием подходов Model First и Database First и создание кода C# или Visual Basic. Для EF Core: 2.

[Веб-сайт](https://www.devart.com/entitydeveloper/)

### <a name="nhydrate-orm-for-entity-framework"></a>Использование nHydrate ORM для Entity Framework

ORM создает строго типизированные расширяемые классы для Entity Framework. Созданный код представляет собой Entity Framework Core. Различий при этом нет. Это не замена EF или пользовательского ORM. Это визуальный слой моделирования, который позволяет команде управлять сложными схемами баз данных. Он поддерживает программное обеспечение SCM, например Git, обеспечивая доступ многих пользователей к модели с минимальными конфликтами. Установщик отслеживает изменения модели и создает скрипты обновления. Для EF Core: 3.

[Сайт GitHub](https://github.com/nHydrate/nHydrate)

### <a name="ef-core-power-tools"></a>EF Core Power Tools

EF Core Power Tools — это расширение Visual Studio, которое позволяет выполнять различные задачи разработки EF Core в простом пользовательском интерфейсе. Оно включает возможности реконструирования DbContext и классов сущностей из существующих баз данных и [файлов DACPAC SQL Server](https://docs.microsoft.com/sql/relational-databases/data-tier-applications/data-tier-applications), управления миграцией баз данных и визуализации моделей. Для EF Core: 2, 3.

[Вики-сайт GitHub](https://github.com/ErikEJ/EFCorePowerTools/wiki)

### <a name="entity-framework-visual-editor"></a>Редактор Visual Entity Framework

Редактор Visual Entity Framework — это расширение Visual Studio, которое добавляет конструктор ORM для визуального проектирования классов EF6 и EF Core. Код создается с помощью шаблонов T4, поэтому его можно адаптировать к любым потребностям. Поддерживаются наследование, однонаправленные и двунаправленные ассоциации, перечисления и возможность цветового выделения классов и добавления текстовых блоков для объяснения потенциально сложных частей проекта. Для EF Core: 2.

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

Библиотека подключаемых модулей, которая позволяет автоматически записывать изменения данных, выполняемые EF Core, в таблицу журнала. Для EF Core: 2.

[Репозиторий GitHub](https://github.com/Arch/AutoHistory/)

### <a name="efsecondlevelcachecore"></a>EFSecondLevelCache.Core

Расширение, которое позволяет хранить результаты запросов EF Core в кэше второго уровня, чтобы при последующем выполнении тех же запросов не обращаться к базе данных и получить данные напрямую из кэша. Для EF Core: 2.

[Репозиторий GitHub](https://github.com/VahidN/EFSecondLevelCache.Core/)

### <a name="geco"></a>Geco

Geco (Generator Console) — простой генератор кода на основе консольного проекта, который работает в .NET Core и использует интерполированные строки C# для создания кода. Geco включает генератор обратной модели для EF Core с поддержкой преобразования во множественную форму, единичную форму и с поддержкой редактируемых шаблонов. Он также предоставляет генератор сценариев начальных данных, средство запуска сценариев и средство очистки баз данных. Для EF Core: 2.

[Репозиторий GitHub](https://github.com/iQuarc/Geco)

### <a name="entityframeworkcorescaffoldinghandlebars"></a>EntityFrameworkCore.Scaffolding.Handlebars 

Позволяет настраивать классы, реконструированные из существующей базы данных с помощью цепочки инструментов Entity Framework Core с шаблонами Handlebars. Для EF Core: 2, 3.

[Репозиторий GitHub](https://github.com/TrackableEntities/EntityFrameworkCore.Scaffolding.Handlebars)

### <a name="neinlinqentityframeworkcore"></a>NeinLinq.EntityFrameworkCore 

NeinLinq расширяет возможности поставщиков LINQ, таких как Entity Framework, позволяя повторно использовать функции, переписывать запросы и создавать динамические запросы с использованием транслируемых предикатов и селекторов. Для EF Core: 2, 3.

[Репозиторий GitHub](https://github.com/axelheer/nein-linq/)

### <a name="microsoftentityframeworkcoreunitofwork"></a>Microsoft.EntityFrameworkCore.UnitOfWork

Подключаемый модуль Microsoft.EntityFrameworkCore для поддержки репозитория, шаблонов Unit of Work и нескольких баз данных с поддержкой распределенных транзакций. Для EF Core: 2.

[Репозиторий GitHub](https://github.com/Arch/UnitOfWork/)

### <a name="efcorebulkextensions"></a>EFCore.BulkExtensions

Расширения EF Core для массовых операций (Insert, Update, Delete). Для EF Core: 2, 3.

[Репозиторий GitHub](https://github.com/borisdj/EFCore.BulkExtensions)

### <a name="bricelamentityframeworkcorepluralizer"></a>Bricelam.EntityFrameworkCore.Pluralizer

Добавляет преобразование во множественную форму для времени разработки. Для EF Core: 2.

[Репозиторий GitHub](https://github.com/bricelam/EFCore.Pluralizer)

### <a name="toolbeltentityframeworkcoreindexattribute"></a>Toolbelt.EntityFrameworkCore.IndexAttribute

Новая реализация атрибута [Index] (с расширением для создания моделей). Для EF Core: 2, 3.

[Репозиторий GitHub](https://github.com/jsakamoto/EntityFrameworkCore.IndexAttribute)

### <a name="efcoreinmemoryhelpers"></a>EfCore.InMemoryHelpers

Предоставляет оболочку для поставщика базы данных в памяти EF Core. Позволяет сделать так, чтобы эта оболочка напоминала поставщик реляционных баз данных. Для EF Core: 2.

[Репозиторий GitHub](https://github.com/SimonCropp/EfCore.InMemoryHelpers)

### <a name="efcoretemporalsupport"></a>EFCore.TemporalSupport

Реализация поддержки темпоральных операций. Для EF Core: 2.

[Репозиторий GitHub](https://github.com/cpoDesign/EFCore.TemporalSupport)

### <a name="efcoretemporaltable"></a>EfCoreTemporalTable

Простое выполнение темпоральных запросов к избранной базе данных с помощью представленных методов расширения: `AsTemporalAll()`, `AsTemporalAsOf(date)`, `AsTemporalFrom(startDate, endDate)`, `AsTemporalBetween(startDate, endDate)`, `AsTemporalContained(startDate, endDate)`. Для EF Core: 3.

[Репозиторий GitHub](https://github.com/glautrou/EfCoreTemporalTable)

### <a name="efcoretimetraveler"></a>EFCore.TimeTraveler

Разрешите полнофункциональные запросы Entity Framework Core к [темпоральной таблице журнала SQL Server](/sql/relational-databases/tables/temporal-table-usage-scenarios#point-in-time-analysis-time-travel) с помощью ранее определенных кода, сущностей и сопоставлений EF Core.  Путешествуйте во времени, заключив код в `using (TemporalQuery.AsOf(targetDateTime)) {...}`. Для EF Core: 3.

[Репозиторий GitHub](https://github.com/VantageSoftware/EFCore.TimeTraveler)


### <a name="entityframeworkcoretemporaltables"></a>EntityFrameworkCore.TemporalTables

Библиотека расширений для Entity Framework Core, которая позволяет разработчикам, работающим с SQL Server, легко использовать темпоральные таблицы. Для EF Core: 2.

[Репозиторий GitHub](https://github.com/findulov/EntityFrameworkCore.TemporalTables)


### <a name="entityframeworkcorecacheable"></a>EntityFrameworkCore.Cacheable

Высокопроизводительный кэш запросов второго уровня. Для EF Core: 2.

[Репозиторий GitHub](https://github.com/SteffenMangold/EntityFrameworkCore.Cacheable)

### <a name="entity-framework-plus"></a>Entity Framework Plus

Расширены возможности DbContext за счет таких функций, как добавление фильтра, аудит, кэширование, пакетная обработка будущих запросов, пакетное удаление, пакетное обновление и многое другое. Для EF Core: 2, 3.

[Веб-сайт](https://entityframework-plus.net/)
[Репозиторий GitHub](https://github.com/zzzprojects/EntityFramework-Plus)

### <a name="entity-framework-extensions"></a>Расширения Entity Framework

Расширены возможности DbContext за счет таких высокопроизводительных пакетных операций, как BulkSaveChanges, BulkInsert, BulkUpdate, BulkDelete, BulkMerge и других. Для EF Core: 2, 3.

[Веб-сайт](https://entityframework-extensions.net/)

### <a name="expressionify"></a>Expressionify

Позволяет вызывать методы расширения в лямбда-выражениях LINQ. Для EF Core: 3.1

[Репозиторий GitHub](https://github.com/ClaveConsulting/Expressionify)

### <a name="xlinq"></a>XLinq

Технология языка интегрированных запросов (LINQ) для реляционных баз данных. Позволяет писать строго типизированные запросы на C#. Для EF Core: 3.1

- Полная поддержка C# для создания запросов: лямбда-выражения с множеством инструкций, переменные, функции и т. д.
- Отсутствие семантического разрыва с SQL. XLinq объявляет инструкции SQL (такие как `SELECT`, `FROM`, `WHERE`) как методы первого класса C#, сочетая привычный синтаксис с IntelliSense, безопасностью типов и рефакторингом.

В результате SQL становится просто еще одной библиотекой классов, предоставляющей свой API локально, буквально являясь *"языком интегрированного SQL"* .

[Веб-сайт](http://xlinq.live/)
