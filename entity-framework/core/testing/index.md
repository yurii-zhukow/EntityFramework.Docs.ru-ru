---
title: Тестирование кода, использующего EF Core (EF Core)
description: Разные подходы к тестированию приложений, использующих Entity Framework Core
author: ajcvickers
ms.date: 04/22/2020
uid: core/testing/index
ms.openlocfilehash: 5ac7954a87697e43b68845e925e2510c027b7135
ms.sourcegitcommit: 704240349e18b6404e5a809f5b7c9d365b152e2e
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/16/2021
ms.locfileid: "100543579"
---
# <a name="testing-code-that-uses-ef-core"></a>Тестирование кода, использующего EF Core

Тестирование кода, обращающегося к базе данных, требует одного из следующих вариантов:

* Выполнение запросов и обновлений в той же СУБД, что используется в рабочей среде.
* Выполнение запросов и обновлений в другой СУБД, более простой для управления.
* Применение дублеров теста или иного механизма, позволяющего не использовать базу данных вообще.

В этом документе описаны компромиссы, связанные с каждым из этих вариантов, и показано использование EF Core при каждом подходе.

> [!TIP]
> Пример кода, в котором демонстрируются описанные здесь понятия, см. в статье [Пример тестирования EF Core](xref:core/testing/testing-sample).

## <a name="all-database-providers-are-not-equal"></a>Поставщики баз данных неодинаковы

Важно понимать, что платформа EF Core не предназначена для абстрагирования всех аспектов используемой СУБД.
Напротив, она предлагает общий набор шаблонов и понятий, которые можно использовать с любыми системами.
Поставщики баз данных EF Core затем реализуют конкретные действия и функции как следующий уровень, используя эту общую платформу в качестве фундамента.
В результате каждая СУБД может выполнять именно те задачи, с которыми она справляется лучше всего, сохраняя при необходимости общность с другими системами.

По сути это означает, что смена поставщика базы данных приведет к изменению поведения EF Core и приложение не будет работать должным образом, если только оно явным образом не учитывает все различия.
Тем не менее, во многих случаях такая смена возможна из-за высокой степени схожести между различными реляционными базами.
Это имеет и преимущества, и недостатки.
Преимущество состоит в том, что переход между системами баз данных может быть относительно простым.
Недостатком является возможное ложное чувство безопасности, ведь приложение может быть не полностью протестировано в новой СУБД.

## <a name="approach-1-production-database-system"></a>Подход 1. СУБД рабочей среды

Как описано в разделе выше, единственным способом тестирования компонентов, применяемых в рабочей среде, является использование той же системы баз данных.
Например, если развернутое приложение использует SQL в Azure, то тестирование также должно выполняться для SQL в Azure.

Однако если каждый разработчик будет запускать тесты в SQL Azure и одновременно активно работать над кодом, это потребует значительного времени и затрат.
В этом и заключается основной компромисс при таких подходах: когда будет уместно отклониться от рабочей СУБД, чтобы повысить эффективность тестирования?

К счастью, в этом случае ответ довольно прост: для тестирования разработчиками необходимо использовать локальную среду SQL Server.
SQL Server и SQL в Azure практически идентичны, поэтому тестирование в SQL Server обычно является разумным компромиссом.
Вместе с тем все-таки будет целесообразно провести тесты непосредственно в SQL Azure, прежде чем переходить к рабочей среде.

### <a name="localdb"></a>LocalDB

Все популярные СУБД имеют некий вариант "выпуска для разработчиков" (Developer Edition) для локального тестирования.
В SQL Server также есть компонент под названием [LocalDB](/sql/database-engine/configure-windows/sql-server-express-localdb).
Основным преимуществом LocalDB является возможность запуска экземпляра базы данных по запросу.
За счет этого не требуется, чтобы на компьютере постоянно работала служба баз данных, даже когда тесты не выполняются.

Но LocalDB имеет ряд ограничений:

* Этот компонент не поддерживает полный функционал выпуска [SQL Server Developer Edition](/sql/sql-server/editions-and-components-of-sql-server-version-15?view=sql-server-ver15&preserve-view=true).
* Он недоступен для Linux.
* Он может вызывать задержку при первом тестовом запуске из-за развертывания службы.

Автор этой статьи никогда не видел проблем в постоянной работе службы баз данных у себя на компьютере, поэтому в большинстве случаев рекомендует использовать Developer Edition, а не LocalDB.
Но LocalDB может подойти некоторым разработчикам с более слабыми компьютерами.

[Запуск SQL Server](/sql/linux/quickstart-install-connect-docker) (или любой другой системы базы данных) в контейнере Docker (или подобной системе) — это еще один способ избежать запуска системы базы данных непосредственно на компьютере разработки.

## <a name="approach-2-sqlite"></a>Подход 2. SQLite

EF Core тестирует поставщик SQL Server, главным образом запуская его в локальном экземпляре SQL Server.
На быстродействующем компьютере эти тесты выполняют десятки тысяч запросов всего за несколько минут.
Это доказывает, что реальная СУБД может оказаться высокоэффективным решением.
То, что для быстрого тестирования обязательно необходима небольшая база данных, является мифом.

Но что если по какой-то причине вы не можете запускать тесты в среде, близкой к рабочей СУБД?
Следующий по оптимальности вариант заключается в использовании решения с похожими функциями.
Обычно это другая реляционная база данных, и очевидным выбором здесь будет [SQLite](https://sqlite.org/index.html).

Система SQLite имеет следующие преимущества:

* Она выполняется внутри процессов приложения, поэтому создает незначительную нагрузку.
* SQLite использует для баз данных простые, автоматически создаваемые файлы и поэтому не требует управления базами.
* Она имеет режим выполнения в памяти, позволяющий даже не создавать файлы.

Однако следует помнить следующее:

* SQLite безусловно не поддерживает полный функционал вашей рабочей СУБД.
* Для некоторых запросов система SQLite и ваша рабочая СУБД будут работать по-разному.

Поэтому если вы используете SQLite для тестирования, обязательно запускайте тесты и в реальной системе баз данных.

Конкретные рекомендации для EF Core см. в статье [Тестирование с помощью SQLite](xref:core/testing/sqlite).

## <a name="approach-3-the-ef-core-in-memory-database"></a>Подход 3. Выполняющаяся в памяти база данных EF Core

В состав EF Core входит выполняющаяся в памяти база данных, которая используется для внутреннего тестирования самой платформы EF Core.
Эта база в целом **не подходит для тестирования приложений, использующих EF Core**. В частности:

* Она не является реляционной базой данных.
* Она не поддерживает транзакции.
* Она не может выполнять необработанные SQL-запросы.
* Она не оптимизирована для производительности.

Эти факторы не имеют большого значения при тестировании внутренних компонентов EF Core, так как мы используем базу данных именно там, где она не важна для теста.
Однако эти аспекты становятся крайне важными при тестировании использующего EF Core приложения.

## <a name="unit-testing"></a>Модульное тестирование

Предположим, мы тестируем часть бизнес-логики, для которой могут потребоваться какие-то данные из базы, но при этом мы не тестируем работу с базой как таковую.
В этом случае один из вариантов — использовать [дублер теста](https://en.wikipedia.org/wiki/Test_double), например фиктивную реализацию или макет.

Дублеры теста применяются для внутреннего тестирования EF Core.
Однако мы никогда не используем макеты DbContext или IQueryable.
Это сложный, громоздкий и нестабильный процесс.
**Не делайте этого.**

Вместо этого при модульном тестировании решений, в которых применяется DbContext, мы используем выполняющуюся в памяти базу данных EF.
В этом случае ее использование оправданно, так как тест не зависит от работы базы данных.
Только не прибегайте к этому варианту для тестирования реальных запросов и операций обновления.

В статье [Пример тестирования EF Core](xref:core/testing/testing-sample) демонстрируются тесты с использованием выполняющейся в памяти базы данных EF, а также SQL Server и SQLite.
