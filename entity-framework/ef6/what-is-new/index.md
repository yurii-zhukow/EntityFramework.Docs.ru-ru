---
title: Новые возможности EF6
author: divega
ms.date: 2016-10-23
ms.prod: entity-framework
ms.author: divega
ms.manager: avickers
ms.technology: entity-framework-6
ms.topic: article
ms.assetid: 41d1f86b-ce66-4bf2-8963-48514406fb4c
caps.latest.revision: 3
ms.openlocfilehash: dba6403fa341e1abfe8da488a19cf8520e3ea574
ms.sourcegitcommit: 390f3a37bc55105ed7cc5b0e0925b7f9c9e80ba6
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/09/2018
ms.locfileid: "37914180"
---
# <a name="whats-new-in-ef6"></a>Новые возможности EF6

Корпорация Майкрософт настоятельно рекомендует использовать последнюю выпущенную версию Entity Framework, чтобы вы могли использовать новые функции с высочайшим уровнем надежности.
Тем не менее мы понимаем, что вам может потребоваться предыдущая версия или вы захотите поэкспериментировать с новыми усовершенствованиями в последнем предварительном выпуске.
Чтобы установить определенные версии EF, см. сведения в разделе о [получении Entity Framework](~/ef6/fundamentals/install.md).

На этой странице приводятся функции, которые включены в каждый новый выпуск.

## <a name="recent-releases"></a>Последние выпуски

### <a name="ef-tools-update-in-visual-studio-2017-157"></a>Обновления инструментов для Entity Framework в Visual Studio 2017 версии 15.7

В мае 2018 г. мы выпустили обновленную версию EF Tools в составе Visual Studio 2017 версии 15.7.
Она содержит ряд улучшений по частым проблемным вопросам.

- Исправлено несколько ошибок в специальных возможностях пользовательского интерфейса.
- Создано обходное решение для проблемы с медленной работой SQL Server при создании моделей из существующих баз данных [#4](https://github.com/aspnet/entityframework6/issues/4).
- Улучшена поддержка обновления моделей большого размера из SQL Server [#185](https://github.com/aspnet/EntityFramework6/issues/185).

Другим улучшением новой версии EF Tools является автоматическая установка среды выполнения EF 6.2 при создании модели в новом проекте. В предыдущих версиях Visual Studio среду выполнения EF 6.2 (а также любую более раннюю версию EF) можно использовать, установив соответствующую версию пакета NuGet.

### <a name="ef-62-runtime"></a>Среда выполнения EF 6.2

Среда выполнения EF 6.2 была выпущена в NuGet в октябре 2017 г.
Благодаря значительным усилиям нашего сообщества участников, вносящих вклад в разработку ресурсов с открытым исходным кодом, EF 6.2 содержит многочисленные [исправления ошибок](https://github.com/aspnet/entityframework6/issues?utf8=%E2%9C%93&q=is%3Aissue%20milestone%3A6.2.0%20is%3Aclosed%20label%3Aclosed-fixed%20-label%3Aarea-tools%20label%3Atype-bug) и [усовершенствования продуктов](https://github.com/aspnet/entityframework6/issues?utf8=%E2%9C%93&q=is%3Aissue%20milestone%3A6.2.0%20is%3Aclosed%20label%3Aclosed-fixed%20-label%3Aarea-tools%20label%3Atype-enhancement%20).

Далее приведен краткий перечень наиболее важных изменений, затрагивающих среду выполнения EF 6.2.

- Сокращено время запуска за счет загрузки готовых моделей Code First из постоянного кэша [#275](https://github.com/aspnet/EntityFramework6/issues/275).
- Доступен текучий API для определения индексов [#274](https://github.com/aspnet/EntityFramework6/issues/274).
- DbFunctions.Like() для написания запросов LINQ, преобразующих LIKE в SQL [#241](https://github.com/aspnet/EntityFramework6/issues/241).
- Migrate.exe теперь поддерживает параметр -script [#240](https://github.com/aspnet/EntityFramework6/issues/240).
- EF6 теперь может работать со значениями ключей, созданными с помощью последовательности в SQL Server [#165](https://github.com/aspnet/EntityFramework6/issues/165).
- Обновленный список временных ошибок для стратегии выполнения SQL Azure [#83](https://github.com/aspnet/EntityFramework6/issues/83).
- Ошибка. Повторное выполнение запросов или команд SQL завершается ошибкой The SqlParameter is already contained by another SqlParameterCollection ("Параметр SqlParameter уже содержится в другом объекте SqlParameterCollection") [#81](https://github.com/aspnet/EntityFramework6/issues/81).
- Ошибка. Часто истекает время ожидания оценки DbQuery.ToString() в отладчике [#73](https://github.com/aspnet/EntityFramework6/issues/73).

## <a name="future-releases"></a>Будущие выпуски

Сведения о следующей версии EF6, см. в нашем [плане выпусков](roadmap.md).

## <a name="past-releases"></a>Прошлые выпуски

На странице [прошлых выпусков](past-releases.md) содержится архив всех предыдущих версий EF и основных функций, которые были представлены в каждом выпуске.
