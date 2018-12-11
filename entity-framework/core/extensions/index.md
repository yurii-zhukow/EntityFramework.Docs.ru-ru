---
title: Инструменты и расширения — EF Core
author: ErikEJ
ms.date: 07/03/2018
ms.assetid: 14fffb6c-a687-4881-a094-af4a1359a296
uid: core/extensions/index
ms.openlocfilehash: 67eae6cb943b974cc9cd581b8054836d2e37b1e9
ms.sourcegitcommit: a6082a2caee62029f101eb1000656966195cd6ee
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/10/2018
ms.locfileid: "53181998"
---
# <a name="ef-core-tools--extensions"></a>Инструменты и расширения EF Core

Инструменты и расширения предоставляют дополнительные возможности для Entity Framework Core.

> [!IMPORTANT]  
> Расширения создаются с помощью различных источников и не разрабатываются в рамках проекта Entity Framework Core. Выбирая стороннее расширение, обязательно оцените качество, лицензирование, совместимость, поддержку и другие показатели на соответствие вашим требованиям.

## <a name="tools"></a>Инструменты

### <a name="llblgen-pro"></a>LLBLGen Pro

LLBLGen Pro — решение для моделирования сущностей с поддержкой Entity Framework и Entity Framework Core. Оно позволяет легко определить модель сущности и сопоставить ее с базой данных с помощью подходов Database-First (сначала база данных) или Model-First (сначала модель), таким образом, вы сможете сразу приступить к написанию запросов.

[веб-сайт](https://www.llblgen.com/)

### <a name="devart-entity-developer"></a>Devart Entity Developer

Entity Developer — мощный конструктор ORM для ADO.NET Entity Framework, NHibernate, LinqConnect, Telerik Data Access и LINQ to SQL. Используйте подходы Model-First (сначала модель) и Database-First (сначала база данных) для разработки модели ORM и создания для нее кода C# или Visual Basic .NET. В нем используются новые подходы для разработки моделей ORM, повышения производительности и упрощения разработки приложений баз данных.

[веб-сайт](https://www.devart.com/entitydeveloper/)

### <a name="ef-core-power-tools"></a>EF Core Power Tools

Расширение Visual Studio 2017+. Вы можете реконструировать классы DbContext и POCO из существующей базы данных или проекта базы данных SQL Server и визуализировать и проверить DbContext различными способами.

[Вики-сайт GitHub](https://github.com/ErikEJ/SqlCeToolbox/wiki/EF-Core-Power-Tools)

### <a name="entity-framework-visual-editor"></a>Редактор Visual Entity Framework

Расширение Visual Studio 2017, которое добавляет конструктор ORM для проектирования визуальных объектов на платформе Entity Framework 6, Core 2.0 и Core 2.1. Код создается с помощью шаблонов T4, поэтому его можно адаптировать к любым потребностям. Наследование, однонаправленные и двунаправленные ассоциации поддерживаются, как и перечисления и возможность цветового выделения классов и добавления текстовых блоков для объяснения потенциально сложных частей проекта.

[Marketplace](https://marketplace.visualstudio.com/items?itemName=michaelsawczyn.EFDesigner)

### <a name="catfactory"></a>CatFactory

CatFactory — это механизм формирования шаблонов для .NET Core и Entity Framework Core. Концепция, лежащая в основе CatFactory, представляет собой экспорт существующей базы данных из экземпляра SQL Server с ее последующим представлением в моделях для базы данных; формирование шаблонов для сущностей, конфигураций, репозиториев и многое другое.

[Репозиторий GitHub](https://github.com/hherzl/CatFactory.EntityFrameworkCore)

### <a name="loresofts-entity-framework-core-generator"></a>Entity Framework Core Generator от LoreSoft

Entity Framework Core Generator (efg) — это средство командной строки .NET Core, которое создает модели EF Core на основе существующей базы данных почти так же, как `dotnet ef dbcontext scaffold`. Однако отличие Entity Framework Core Generator состоит в том, что оно также поддерживает безопасное [повторное создание](https://efg.loresoft.com/en/latest/regeneration/) кода. Повторное создание кода осуществляется с помощью замены региона или синтаксического анализа файлов сопоставления. Средство также поддерживает формирование моделей представлений, проверку и код средства сопоставления объектов. Дополнительные сведения см. в руководстве и статьях, ссылки на которые приведены в документации продукта.

[Руководство](http://www.loresoft.com/Generate-ASP-NET-Web-API)
[Документация](https://efg.loresoft.com/en/latest/)

## <a name="extensions"></a>Расширения

### <a name="microsoftentityframeworkcoreautohistory"></a>Microsoft.EntityFrameworkCore.AutoHistory

Подключаемый модуль Microsoft.EntityFrameworkCore для поддержки автоматической записи истории изменения данных.

[Репозиторий GitHub](https://github.com/Arch/AutoHistory/)

### <a name="microsoftentityframeworkcoredynamiclinq"></a>Microsoft.EntityFrameworkCore.DynamicLinq

Расширения динамического Linq для Microsoft.EntityFrameworkCore, которые добавляют поддержку асинхронных запросов

 [Репозиторий GitHub](https://github.com/StefH/System.Linq.Dynamic.Core/)

### <a name="efcorepractices"></a>EFCore.Practices

Попытка объединить некоторые хорошие примеры использования в один API, поддерживающий тестирование, включая небольшую платформу для проверки на наличие запросов N+1.

[Репозиторий GitHub](https://github.com/riezebosch/efcore-practices/tree/master/src/EFCore.Practices/)

### <a name="efsecondlevelcachecore"></a>EFSecondLevelCache.Core

Кэширующая библиотека второго уровня. Кэширование второго уровня — это кэширование запросов. Результаты команд EF будут храниться в кэше, чтобы такие же команды EF получали данные из кэша, а не выполнялись в базе данных еще раз.

[Репозиторий GitHub](https://github.com/VahidN/EFSecondLevelCache.Core/)

### <a name="detachedentityframework"></a>Detached.EntityFramework

Загружает и сохраняет полные диаграммы отсоединенных сущностей (сущность со своими дочерними сущностями и списками). Создан под впечатлением от расширения [GraphDiff](https://github.com/refactorthis/GraphDiff/). Идея также заключается в добавлении подключаемых модулей для упрощения некоторых повторяющихся задач, таких как аудит и разбиение на страницы.

[Репозиторий GitHub](https://github.com/leonardoporro/Detached/)

### <a name="entityframeworkcoreprimarykey"></a>EntityFrameworkCore.PrimaryKey

Получает из любой сущности первичный ключ (включая составные ключи) как словарь.

[Репозиторий GitHub](https://github.com/NickStrupat/EntityFramework.PrimaryKey/)

### <a name="entityframeworkcorerx"></a>EntityFrameworkCore.Rx

Оболочки реактивных расширений для критически важных наблюдаемых сущностей Entity Framework.

[Репозиторий GitHub](https://github.com/NickStrupat/EntityFramework.Rx/)

### <a name="entityframeworkcoretriggers"></a>EntityFrameworkCore.Triggers

Добавление триггеров для сущностей с реагированием на события insert, update и delete. Существует три события для каждой операции: до, после и при сбое.

[Репозиторий GitHub](https://github.com/NickStrupat/EntityFramework.Triggers/)

### <a name="entityframeworkcoretypedoriginalvalues"></a>EntityFrameworkCore.TypedOriginalValues

Получите типизированный доступ к OriginalValue свойств сущности. Поддерживаются простые и сложные свойства, навигации и коллекции не поддерживаются.

[Репозиторий GitHub](https://github.com/NickStrupat/EntityFramework.TypedOriginalValues/)

### <a name="geco"></a>Geco

Geco предоставляет генератор реконструированной модели с поддержкой преобразования во множественную или единичную форму и настраиваемых шаблонов на основе интерполированных строк C# 6.0, выполняющихся в .NET Core. Он также предоставляет генератор сценариев начальных значений со сценариями слияния SQL и запускателем сценариев.

[Репозиторий GitHub](https://github.com/iQuarc/Geco)

### <a name="linqkitmicrosoftentityframeworkcore"></a>LinqKit.Microsoft.EntityFrameworkCore

LinqKit.Microsoft.EntityFrameworkCore — бесплатный набор расширений для опытных пользователей LINQ to SQL и EntityFrameworkCore. С поддержкой Include(...) и IDbAsync.

[Репозиторий GitHub](https://github.com/scottksmith95/LINQKit/)

### <a name="neinlinqentityframeworkcore"></a>NeinLinq.EntityFrameworkCore

NeinLinq.EntityFrameworkCore предоставляет полезные расширения для использования поставщиков LINQ, таких как Entity Framework, поддерживающие только небольшое подмножество функций .NET, повторно используемые функции, перезапись запросов (даже переделку их в безопасные для обработки NULL) и построение динамических запросов с помощью транслируемых предикатов и селекторов.

[Репозиторий GitHub](https://github.com/axelheer/nein-linq/)

### <a name="microsoftentityframeworkcoreunitofwork"></a>Microsoft.EntityFrameworkCore.UnitOfWork

Подключаемый модуль Microsoft.EntityFrameworkCore для поддержки репозитория, шаблонов Unit of Work и нескольких баз данных с поддержкой распределенных транзакций.

[Репозиторий GitHub](https://github.com/Arch/UnitOfWork/)

### <a name="entityframeworklazyloading"></a>EntityFramework.LazyLoading

Отложенная загрузка для EF Core 1.1

[Репозиторий GitHub](https://github.com/darxis/EntityFramework.LazyLoading)

### <a name="efcorebulkextensions"></a>EFCore.BulkExtensions

Расширения EntityFrameworkCore для массовых операций (Insert, Update, Delete).

[Репозиторий GitHub](https://github.com/borisdj/EFCore.BulkExtensions)

### <a name="bricelamentityframeworkcorepluralizer"></a>Bricelam.EntityFrameworkCore.Pluralizer

Добавляет в EF Core преобразование во множественную форму для времени разработки.

[Репозиторий GitHub](https://github.com/bricelam/EFCore.Pluralizer)
