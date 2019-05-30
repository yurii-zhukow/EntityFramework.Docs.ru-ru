---
title: Журнал изменений, затрагивающих поставщика — EF Core
author: ajcvickers
ms.author: avickers
ms.date: 08/08/2018
ms.assetid: 7CEF496E-A5B0-4F5F-B68E-529609B23EF9
ms.technology: entity-framework-core
uid: core/providers/provider-log
ms.openlocfilehash: 61a58bd6119763d90731fac62343b983af510cb6
ms.sourcegitcommit: 87fcaba46535aa351db4bdb1231bd14b40e459b9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/22/2019
ms.locfileid: "59929879"
---
# <a name="provider-impacting-changes"></a>Изменения, влияющие на поставщика

Эта страница содержит ссылки на запросы, внесенные в репозитории EF Core, может потребовать авторы других поставщиках баз данных для реагирования на Вытягивание. Планируется представляют собой отправную точку для авторов существующей базы данных сторонних поставщиков, при обновлении до новой версии своего поставщика.

Этот журнал начинается с изменениями из 2.1 и 2.2. Прежде чем 2.1 мы использовали [ `providers-beware` ](https://github.com/aspnet/EntityFrameworkCore/labels/providers-beware) и [ `providers-fyi` ](https://github.com/aspnet/EntityFrameworkCore/labels/providers-fyi) меток на наши проблемы и запросы на включение внесенных изменений.

## <a name="22-----30"></a>2.2 ---> 3.0

Обратите внимание, что многие из [критические изменения на уровне приложений](../what-is-new/ef-core-3.0/breaking-changes.md) также повлияет на поставщиков.

* https://github.com/aspnet/EntityFrameworkCore/pull/14022
  * Удалены устаревшие API и перегрузок свернутого необязательный параметр
  * Removed DatabaseColumn.GetUnderlyingStoreType()
* https://github.com/aspnet/EntityFrameworkCore/pull/14589
  * Удалены устаревшие API
* https://github.com/aspnet/EntityFrameworkCore/pull/15044
  * Подклассы CharTypeMapping была нарушена из-за изменения поведения, необходимые для исправления нескольких ошибок в базовой реализации.
* https://github.com/aspnet/EntityFrameworkCore/pull/15090
  * Добавлен базовый класс для IDatabaseModelFactory и обновили его, чтобы использовать объект значения, указанного для снижения будущих разрывов.
* https://github.com/aspnet/EntityFrameworkCore/pull/15123
  * Использовать объектов параметров в MigrationsSqlGenerator во избежание будущих разрывов.
* https://github.com/aspnet/EntityFrameworkCore/pull/14972
  * Явной настройки журнала уровней, требующий внесения некоторых изменений API-интерфейсы, которые могут использовать поставщики. В частности Если поставщики инфраструктуры ведения журналов используется напрямую, это изменение может нарушить, использующих. Кроме того, поставщики, которые используют инфраструктуру (то есть открытый) в дальнейшем потребуется являются производными от `LoggingDefinitions` или `RelationalLoggingDefinitions`. См. в разделе SQL Server и поставщиков в памяти, примеры.
* https://github.com/aspnet/EntityFrameworkCore/pull/15091
  * Core, реляционные данные и абстракций строки ресурсов теперь являются открытыми.
  * `CoreLoggerExtensions` и `RelationalLoggerExtensions` теперь являются открытыми. Поставщики следует использовать эти API-интерфейсы, при регистрации событий, определенных в core или реляционном уровне. Ведение журнала ресурсы недоступны непосредственно. Это по-прежнему внутренней.
  * `IRawSqlCommandBuilder` в ограниченной службы отличается от это отдельная служба
  * `IMigrationsSqlGenerator` в ограниченной службы отличается от это отдельная служба
* https://github.com/aspnet/EntityFrameworkCore/pull/14706
  * Инфраструктуры для создания реляционных команды сделан общим, его можно безопасно используется поставщиками и немного рефакторинга.
* https://github.com/aspnet/EntityFrameworkCore/pull/14733
  * `ILazyLoader` был изменен с ограниченной службы на временной ошибкой службы
* https://github.com/aspnet/EntityFrameworkCore/pull/14610
  * `IUpdateSqlGenerator` в это отдельная служба отличается от ограниченной службы
  * Кроме того `ISingletonUpdateSqlGenerator` был удален
* https://github.com/aspnet/EntityFrameworkCore/pull/15067
  * Много внутреннего кода, который использовался поставщиками теперь стала общедоступной
  * Больше не должен быть necssary для ссылки на `IndentedStringBuilder` так, как он выделен из мест, которые доступны его
  * Случаи использования `NonCapturingLazyInitializer` следует заменить `LazyInitializer` из библиотеки базовых Классов
* https://github.com/aspnet/EntityFrameworkCore/pull/14608
  * Это изменение полностью описаны в приложении, критические изменения документа. Для поставщиков возможно, дополнительные ущерба, так как тестирование EF core часто может привести к достижению эту проблему, поэтому инфраструктуру тестирования было изменено на убедитесь, что менее вероятно.
* https://github.com/aspnet/EntityFrameworkCore/issues/13961
  * `EntityMaterializerSource` упрощена
* https://github.com/aspnet/EntityFrameworkCore/pull/14895
  * Перевод StartsWith был изменен способом, который поставщики могут want/необходимости реагирования на них
* https://github.com/aspnet/EntityFrameworkCore/pull/15168
  * Соглашение о наборе служб были изменены. Поставщики должны теперь являются производными от «ProviderConventionSet» или «RelationalConventionSet».
  * Настройки можно добавлять с помощью `IConventionSetCustomizer` служб, но это предназначено для использования другими расширениями, не поставщиков.
  * Соглашения, используемые во время выполнения должен быть разрешен из `IConventionSetBuilder`.
* [https://github.com/aspnet/EntityFrameworkCore/pull/15288](https://github.com/aspnet/EntityFrameworkCore/pull/15288) -Заполнение данными были вынесены в открытый API, чтобы избежать необходимости использования внутренних типов. Это следует только повлиять на нереляционных поставщиков, поскольку заполнение обрабатывается базовым классом реляционных для всех поставщиков реляционной.

## <a name="21-----22"></a>2.1 ---> 2.2

### <a name="test-only-changes"></a>Только для тестирования изменений

* [https://github.com/aspnet/EntityFrameworkCore/pull/12057](https://github.com/aspnet/EntityFrameworkCore/pull/12057) — Разрешите разделителей групп разрядов настраиваемый SQL в тестах
  * Проверить изменения, обеспечивающие нестрогом с плавающей запятой сравнения в BuiltInDataTypesTestBase
  * Изменения тестирования, обеспечивающие тестов запросов для многократного использования с помощью различных разделителей групп разрядов SQL
* [https://github.com/aspnet/EntityFrameworkCore/pull/12072](https://github.com/aspnet/EntityFrameworkCore/pull/12072) -Добавьте тесты DbFunction реляционных спецификации тесты
  * Таким образом, эти тесты могут выполняться для всех поставщиков базы данных
* [https://github.com/aspnet/EntityFrameworkCore/pull/12362](https://github.com/aspnet/EntityFrameworkCore/pull/12362) -Очистка тестовой Async
  * Удалить `Wait` вызовы, ненужные async и переименовать некоторые методы теста
* [https://github.com/aspnet/EntityFrameworkCore/pull/12666](https://github.com/aspnet/EntityFrameworkCore/pull/12666) -Унифицировать инфраструктуру ведения журнала тестирования
  * Добавлен `CreateListLoggerFactory` и удалены некоторые ранее инфраструктурой ведения журнала, потребуются поставщиков, использующих эти тесты для реагирования
* [https://github.com/aspnet/EntityFrameworkCore/pull/12500](https://github.com/aspnet/EntityFrameworkCore/pull/12500) — Запустите дополнительные тесты запрос синхронно и асинхронно
  * Имена тестов и факторинга изменилось, что подразумевает работу поставщиков с помощью этих тестов реагирования на них
* [https://github.com/aspnet/EntityFrameworkCore/pull/12766](https://github.com/aspnet/EntityFrameworkCore/pull/12766) — Переименование переходов в модели ComplexNavigations
  * Поставщики, с помощью этих тестов может потребоваться react
* [https://github.com/aspnet/EntityFrameworkCore/pull/12141](https://github.com/aspnet/EntityFrameworkCore/pull/12141) — Изменяет контекст в пул, вместо того чтобы избавляться в функциональных тестов
  * Это изменение включает в себя оптимизация тестирования которого может потребоваться поставщиков реагирования на них

### <a name="test-and-product-code-changes"></a>Изменения кода теста и продукта

* [https://github.com/aspnet/EntityFrameworkCore/pull/12109](https://github.com/aspnet/EntityFrameworkCore/pull/12109) -Консолидировать RelationalTypeMapping.Clone методы
  * Изменения в 2.1 RelationalTypeMapping, разрешенное для упрощения в производных классах. Мы не считают, что это критическим изменением для поставщиков, а поставщики можно воспользоваться преимуществами этого изменения, в их производные типы сопоставление классов.
* [https://github.com/aspnet/EntityFrameworkCore/pull/12069](https://github.com/aspnet/EntityFrameworkCore/pull/12069) -Тегами или именованные запросы
  * Добавляет инфраструктуру для добавления тегов запросов LINQ и необходимости эти теги отображаются в виде комментариев в код SQL. Это может потребовать поставщиков реагировать на создание кода SQL.
* [https://github.com/aspnet/EntityFrameworkCore/pull/13115](https://github.com/aspnet/EntityFrameworkCore/pull/13115) -Поддерживают Пространственные данные с помощью NTS
  * Позволяет преобразователи для регистрации за пределами поставщика сопоставления типов и членов
    * Поставщики должны вызывать base. FindMapping() ITypeMappingSource реализации для работы
  * Следуйте этому шаблону, чтобы добавить поддержку пространственных к поставщику, который остается согласованным на всех поставщиков.
* [https://github.com/aspnet/EntityFrameworkCore/pull/13199](https://github.com/aspnet/EntityFrameworkCore/pull/13199) — Добавление расширенных возможностей отладки для создания поставщика службы
  * Позволяет DbContextOptionsExtensions реализовать новый интерфейс, который может помочь пользователям понять, почему доступ к внутренней службе, повторного построения
* [https://github.com/aspnet/EntityFrameworkCore/pull/13289](https://github.com/aspnet/EntityFrameworkCore/pull/13289) — Добавляет CanConnect API для использования путем проверки работоспособности
  * Этот запрос на Вытягивание добавляет понятие `CanConnect` которого будет использоваться служба работоспособности ASP.NET Core проверяет, чтобы определить, доступна ли база данных. По умолчанию реляционных реализация просто вызывает `Exist`, но поставщики могут реализовывать другое значение при необходимости. Нереляционные поставщиков необходимо реализовать новый интерфейс API в порядке для проверки работоспособности, чтобы можно было использовать.
* [https://github.com/aspnet/EntityFrameworkCore/pull/13306](https://github.com/aspnet/EntityFrameworkCore/pull/13306) -Обновление базового RelationalTypeMapping не задать размер DbParameter
  * Остановите задание размера по умолчанию, так как это может привести к усечению. Поставщики может потребоваться добавить свою собственную логику, если необходимо задать размер.
* (https://github.com/aspnet/EntityFrameworkCore/pull/13372) -RevEng: Всегда указывайте тип столбца для десятичных столбцов
  * Всегда можно настройте тип столбца для десятичных столбцов в шаблонном коде, а не настраивать в соответствии с соглашением.
  * Поставщики должны не требуется вносить изменения с их стороны.
* [https://github.com/aspnet/EntityFrameworkCore/pull/13469](https://github.com/aspnet/EntityFrameworkCore/pull/13469) — Добавляет CaseExpression для создания выражений регистр SQL
* [https://github.com/aspnet/EntityFrameworkCore/pull/13648](https://github.com/aspnet/EntityFrameworkCore/pull/13648) — Добавляет возможность указать сопоставления типов на SqlFunctionExpression для улучшения магазина вывод типа аргументов и результаты.
