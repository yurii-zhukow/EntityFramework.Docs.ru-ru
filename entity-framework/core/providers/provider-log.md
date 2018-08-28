---
title: Журнал изменений, затрагивающих поставщика — EF Core
author: ajcvickers
ms.author: avickers
ms.date: 08/08/2018
ms.assetid: 7CEF496E-A5B0-4F5F-B68E-529609B23EF9
ms.technology: entity-framework-core
uid: core/providers/provider-log
ms.openlocfilehash: ee73940e3c0030b76e73438b1852cc29ebeadb45
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/27/2018
ms.locfileid: "42998356"
---
# <a name="provider-impacting-changes"></a>Изменения, влияющие на поставщика

Эта страница содержит ссылки на запросы, внесенные в репозитории EF Core, может потребовать авторы других поставщиках баз данных для реагирования на Вытягивание. Планируется представляют собой отправную точку для авторов существующей базы данных сторонних поставщиков, при обновлении до новой версии своего поставщика.

Этот журнал начинается с изменениями из 2.1 и 2.2. Прежде чем 2.1 мы использовали [ `providers-beware` ](https://github.com/aspnet/EntityFrameworkCore/labels/providers-beware) и [ `providers-fyi` ](https://github.com/aspnet/EntityFrameworkCore/labels/providers-fyi) меток на наши проблемы и запросы на включение внесенных изменений.

### <a name="21-----22"></a>2.1---> 2.2

#### <a name="test-only-changes"></a>Только для тестирования изменений

* https://github.com/aspnet/EntityFrameworkCore/pull/12057 — Разрешите разделителей групп разрядов настраиваемый SQL в тестах
  * Проверить изменения, обеспечивающие нестрогом с плавающей запятой сравнения в BuiltInDataTypesTestBase
  * Изменения тестирования, обеспечивающие тестов запросов для многократного использования с помощью различных разделителей групп разрядов SQL
* https://github.com/aspnet/EntityFrameworkCore/pull/12072 -Добавьте тесты DbFunction реляционных спецификации тесты
  * Таким образом, эти тесты могут выполняться для всех поставщиков базы данных
* https://github.com/aspnet/EntityFrameworkCore/pull/12362 -Очистка тестовой Async
  * Удалить `Wait` вызовы, ненужные async и переименовать некоторые методы теста
* https://github.com/aspnet/EntityFrameworkCore/pull/12666 -Унифицировать инфраструктуру ведения журнала тестирования
  * Добавлен `CreateListLoggerFactory` и удалены некоторые ранее инфраструктурой ведения журнала, потребуются поставщиков, использующих эти тесты для реагирования
* https://github.com/aspnet/EntityFrameworkCore/pull/12500 — Запустите дополнительные тесты запрос синхронно и асинхронно
  * Имена тестов и факторинга изменилось, что подразумевает работу поставщиков с помощью этих тестов реагирования на них
* https://github.com/aspnet/EntityFrameworkCore/pull/12766 — Переименование переходов в модели ComplexNavigations
  * Поставщики, с помощью этих тестов может потребоваться react
* https://github.com/aspnet/EntityFrameworkCore/pull/12141 — Изменяет контекст в пул, вместо того чтобы избавляться в функциональных тестов
  * Это изменение включает в себя оптимизация тестирования которого может потребоваться поставщиков реагирования на них


#### <a name="test-and-product-code-changes"></a>Изменения кода теста и продукта

* https://github.com/aspnet/EntityFrameworkCore/pull/12109 -Консолидировать RelationalTypeMapping.Clone методы
  * Изменения в 2.1 RelationalTypeMapping, разрешенное для упрощения в производных классах. Мы не считают, что это критическим изменением для поставщиков, а поставщики можно воспользоваться преимуществами этого изменения, в их производные типы сопоставление классов.
* https://github.com/aspnet/EntityFrameworkCore/pull/12069 -Тегами или именованные запросы
  * Добавляет инфраструктуру для добавления тегов запросов LINQ и необходимости эти теги отображаются в виде комментариев в код SQL. Это может потребовать поставщиков реагировать на создание кода SQL.
