---
title: Инструменты и расширения — EF Core
author: ErikEJ
ms.date: 01/07/2019
ms.assetid: 14fffb6c-a687-4881-a094-af4a1359a296
uid: core/extensions/index
ms.openlocfilehash: d432ca36c166f7baf71253709bf58b1f5428a11a
ms.sourcegitcommit: 8f801993c9b8cd8a8fbfa7134818a8edca79e31a
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/14/2019
ms.locfileid: "59562511"
---
# <a name="ef-core-tools--extensions"></a>Инструменты и расширения EF Core

Эти инструменты и расширения предоставляют дополнительные возможности для Entity Framework Core 2.0 и более поздних версий.

> [!IMPORTANT]  
> Расширения создаются с помощью различных источников и не поддерживаются в рамках проекта Entity Framework Core. Выбирая стороннее расширение, обязательно оцените его качество, лицензирование, совместимость, поддержку и другие показатели на соответствие вашим требованиям.

## <a name="tools"></a>Инструменты

### <a name="llblgen-pro"></a>LLBLGen Pro

LLBLGen Pro — решение для моделирования сущностей с поддержкой Entity Framework и Entity Framework Core. Оно позволяет легко определить модель сущности и сопоставить ее с базой данных с помощью подходов Database-First (сначала база данных) или Model-First (сначала модель), таким образом, вы сможете сразу приступить к написанию запросов.

[Веб-сайт](https://www.llblgen.com/)

### <a name="devart-entity-developer"></a>Devart Entity Developer

Entity Developer — мощный конструктор ORM для ADO.NET Entity Framework, NHibernate, LinqConnect, Telerik Data Access и LINQ to SQL. Он поддерживает визуальное проектирование моделей EF Core с использованием подходов Model First и Database First и создание кода C# или Visual Basic. 

[Веб-сайт](https://www.devart.com/entitydeveloper/)

### <a name="ef-core-power-tools"></a>EF Core Power Tools

EF Core Power Tools — это расширение Visual Studio 2017, которое позволяет выполнять различные задачи разработки EF Core в простом пользовательском интерфейсе. Оно включает возможности реконструирования DbContext и классов сущностей из существующих баз данных и [файлов DACPAC SQL Server](https://docs.microsoft.com/sql/relational-databases/data-tier-applications/data-tier-applications), управления миграцией баз данных и визуализации моделей.

[Вики-сайт GitHub](https://github.com/ErikEJ/EFCorePowerTools/wiki)

### <a name="entity-framework-visual-editor"></a>Редактор Visual Entity Framework

Редактор Visual Entity Framework — это расширение Visual Studio, которое добавляет конструктор ORM для визуального проектирования классов EF6 и EF Core. Код создается с помощью шаблонов T4, поэтому его можно адаптировать к любым потребностям. Поддерживаются наследование, однонаправленные и двунаправленные ассоциации, перечисления и возможность цветового выделения классов и добавления текстовых блоков для объяснения потенциально сложных частей проекта.

[Marketplace](https://marketplace.visualstudio.com/items?itemName=michaelsawczyn.EFDesigner)

### <a name="catfactory"></a>CatFactory

CatFactory — это механизм формирования шаблонов для .NET Core, который позволяет автоматизировать создание классов DbContext, сущностей, сопоставление конфигураций и создание классов репозиториев из базы данных SQL Server.

[Репозиторий GitHub](https://github.com/hherzl/CatFactory.EntityFrameworkCore)

### <a name="loresofts-entity-framework-core-generator"></a>Entity Framework Core Generator от LoreSoft

Entity Framework Core Generator (efg) — это средство командной строки .NET Core, которое позволяет создавать модели EF Core на основе существующей базы данных, как и `dotnet ef dbcontext scaffold`, но также поддерживает безопасное [повторное создание кода](https://efg.loresoft.com/en/latest/regeneration/) путем замены регионов или путем синтаксического анализа файлов сопоставления. Средство поддерживает формирование моделей представлений, проверку и код средства сопоставления объектов. 

[Руководство](http://www.loresoft.com/Generate-ASP-NET-Web-API)
[Документация](https://efg.loresoft.com/en/latest/)

## <a name="extensions"></a>Расширения

### <a name="microsoftentityframeworkcoreautohistory"></a>Microsoft.EntityFrameworkCore.AutoHistory

Библиотека подключаемых модулей, которая позволяет автоматически записывать изменения данных, выполняемые EF Core, в таблицу журнала.

[Репозиторий GitHub](https://github.com/Arch/AutoHistory/)

### <a name="microsoftentityframeworkcoredynamiclinq"></a>Microsoft.EntityFrameworkCore.DynamicLinq

Портированная версия .NET Core/.NET Standard для System.Linq.Dynamic, которая включает поддержку асинхронных операций в EF Core.
Расширение System.Linq.Dynamic изначально представляло собой пример Майкрософт, который демонстрировал динамическое создание запросов LINQ из строковых выражений, а не из кода.

[Репозиторий GitHub](https://github.com/StefH/System.Linq.Dynamic.Core/)

### <a name="efsecondlevelcachecore"></a>EFSecondLevelCache.Core

Расширение, которое позволяет хранить результаты запросов EF Core в кэше второго уровня, чтобы при последующем выполнении тех же запросов не обращаться к базе данных и получить данные напрямую из кэша.

[Репозиторий GitHub](https://github.com/VahidN/EFSecondLevelCache.Core/)

### <a name="entityframeworkcoreprimarykey"></a>EntityFrameworkCore.PrimaryKey

Эта библиотека позволяет получить значения первичного ключа (включая составные ключи) из любой сущности в виде словаря.

[Репозиторий GitHub](https://github.com/NickStrupat/EntityFramework.PrimaryKey/)

### <a name="entityframeworkcoretypedoriginalvalues"></a>EntityFrameworkCore.TypedOriginalValues

Эта библиотека обеспечивает строго типизированный доступ к исходным значениям свойств сущности. 

[Репозиторий GitHub](https://github.com/NickStrupat/EntityFramework.TypedOriginalValues/)

### <a name="geco"></a>Geco

Geco (Generator Console) — простой генератор кода на основе консольного проекта, который работает в .NET Core и использует интерполированные строки C# для создания кода. Geco включает генератор обратной модели для EF Core с поддержкой преобразования во множественную форму, единичную форму и с поддержкой редактируемых шаблонов. Он также предоставляет генератор сценариев начальных данных, средство запуска сценариев и средство очистки баз данных.

[Репозиторий GitHub](https://github.com/iQuarc/Geco)

### <a name="linqkitmicrosoftentityframeworkcore"></a>LinqKit.Microsoft.EntityFrameworkCore

LinqKit.Microsoft.EntityFrameworkCore представляет собой версию библиотеки LINQKit, совместимую с EF Core. LINQKit — это бесплатный набор расширений для опытных пользователей LINQ to SQL и Entity Framework. Он включает дополнительные функциональные возможности, такие как динамическое создание выражений предикатов и использование переменных выражений во вложенных запросах.  

[Репозиторий GitHub](https://github.com/scottksmith95/LINQKit/)

### <a name="neinlinqentityframeworkcore"></a>NeinLinq.EntityFrameworkCore

NeinLinq расширяет возможности поставщиков LINQ, таких как Entity Framework, позволяя повторно использовать функции, переписывать запросы и создавать динамические запросы с использованием транслируемых предикатов и селекторов.

[Репозиторий GitHub](https://github.com/axelheer/nein-linq/)

### <a name="microsoftentityframeworkcoreunitofwork"></a>Microsoft.EntityFrameworkCore.UnitOfWork

Подключаемый модуль Microsoft.EntityFrameworkCore для поддержки репозитория, шаблонов Unit of Work и нескольких баз данных с поддержкой распределенных транзакций.

[Репозиторий GitHub](https://github.com/Arch/UnitOfWork/)

### <a name="efcorebulkextensions"></a>EFCore.BulkExtensions

Расширения EF Core для массовых операций (Insert, Update, Delete).

[Репозиторий GitHub](https://github.com/borisdj/EFCore.BulkExtensions)

### <a name="bricelamentityframeworkcorepluralizer"></a>Bricelam.EntityFrameworkCore.Pluralizer

Добавляет в EF Core преобразование во множественную форму для времени разработки.

[Репозиторий GitHub](https://github.com/bricelam/EFCore.Pluralizer)

### <a name="pomelofoundationpomeloentityframeworkcoreextensionstosql"></a>PomeloFoundation/Pomelo.EntityFrameworkCore.Extensions.ToSql

В простых сценариях для формирования заданного запроса LINQ может использоваться простой метод расширения, который получает инструкцию SQL в EF Core. Использование метода ToSql ограничено простыми сценариями, так как EF Core может создавать несколько инструкций SQL в одном запросе LINQ, а также различные инструкции SQL, которые зависят от значений параметров.

[Репозиторий GitHub](https://github.com/PomeloFoundation/Pomelo.EntityFrameworkCore.Extensions.ToSql)

### <a name="toolbeltentityframeworkcoreindexattribute"></a>Toolbelt.EntityFrameworkCore.IndexAttribute

Новая реализация атрибута [Index] для EF Core (с расширением для создания моделей).

[Репозиторий GitHub](https://github.com/jsakamoto/EntityFrameworkCore.IndexAttribute)

### <a name="efcoreinmemoryhelpers"></a>EfCore.InMemoryHelpers

Предоставляет оболочку для поставщика базы данных в памяти EF Core. Позволяет сделать так, чтобы эта оболочка напоминала поставщик реляционных баз данных.

[Репозиторий GitHub](https://github.com/SimonCropp/EfCore.InMemoryHelpers)

### <a name="efcoretemporalsupport"></a>EFCore.TemporalSupport

Реализация поддержки темпоральных операций для EF Core.

[Репозиторий GitHub](https://github.com/cpoDesign/EFCore.TemporalSupport)

### <a name="entityframeworkcorecacheable"></a>EntityFrameworkCore.Cacheable

Высокопроизводительный кэш запросов второго уровня для EF Core.

[Репозиторий GitHub](https://github.com/SteffenMangold/EntityFrameworkCore.Cacheable)
