---
title: Начало работы с платформой Entity Framework 6 — EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 66ce9113-81d2-480f-8c16-d00ec405b2f7
uid: ef6/get-started
ms.openlocfilehash: 729dea2c474c35f638ccaf6673550f76e88e2667
ms.sourcegitcommit: 9b562663679854c37c05fca13d93e180213fb4aa
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/07/2020
ms.locfileid: "80136269"
---
# <a name="get-started-with-entity-framework-6"></a>Начало работы с платформой Entity Framework 6

Это руководство содержит набор ссылок на отдельные статьи с документацией, пошаговые руководства и видео, которые помогут быстро начать работу.

## <a name="fundamentals"></a>Основы

* [Установка Entity Framework](~/ef6/fundamentals/install.md)

  Здесь вы узнаете, как добавить Entity Framework в свои приложения. Если вы хотите использовать конструктор EF, установите его в Visual Studio.

* [Создание модели: Code First, конструктор EF и рабочие процессы EF](~/ef6/modeling/index.md)

  Вы предпочитаете задать модель EF, написав код или нарисовав поля и линии?
Вы собираетесь использовать EF для сопоставления объектов с существующей базой данных или хотите создать в EF базу данных, настроенную для ваших объектов?
Здесь вы узнаете о двух разных подходах к использованию EF6: конструктор EF и Code First.
Обязательно подпишитесь на обсуждение и посмотрите видео о различиях между ними.

* [Работа с DbContext](~/ef6/fundamentals/working-with-dbcontext.md)

  DbContext — это первый и самый важный тип EF, который вам нужно научиться использовать. Он служит источником запросов базы данных и отслеживает изменения, внесенные в объекты, чтобы их можно было сохранить в базе данных.

* [Вопросы](~/ef6/resources/get-help.md)

  Вы узнаете, как получить справку от экспертов и сможете сами отвечать на вопросы сообщества.

* [Участие](https://github.com/aspnet/EntityFramework6/)

  В Entity Framework 6 используется модель разработки с открытым исходным кодом. Посетите репозиторий GitHub, чтобы узнать, что поможет улучшить EF.

## <a name="code-first-resources"></a>Ресурсы по Code First

  - [Code First для рабочего процесса существующей базы данных](~/ef6/modeling/code-first/workflows/existing-database.md)
  - [Code First для рабочего процесса новой базы данных](~/ef6/modeling/code-first/workflows/new-database.md)
  - [Сопоставление перечислений с помощью Code First](~/ef6/modeling/code-first/data-types/enums.md)
  - [Сопоставление пространственных типов с помощью Code First](~/ef6/modeling/code-first/data-types/spatial.md)
  - [Запись пользовательских соглашений Code First](~/ef6/modeling/code-first/conventions/custom.md)
  - [Настройка Code First для использования текучего API в Visual Basic](~/ef6/modeling/code-first/fluent/vb.md)
  - [Code First Migrations](~/ef6/modeling/code-first/migrations/index.md)
  - [Code First Migrations в командных средах](~/ef6/modeling/code-first/migrations/teams.md)
  - [Автоматические миграции Code First Migrations](~/ef6/modeling/code-first/migrations/automatic.md) (больше не рекомендуется)

## <a name="ef-designer-resources"></a>Ресурсы по конструктору EF
  - [Рабочий процесс Database First](~/ef6/modeling/designer/workflows/database-first.md)
  - [Рабочий процесс Model First](~/ef6/modeling/designer/workflows/model-first.md)
  - [Сопоставление перечислений](~/ef6/modeling/designer/data-types/enums.md)
  - [Сопоставление пространственных типов](~/ef6/modeling/designer/data-types/spatial.md)
  - [Сопоставление наследования "одна таблица на иерархию"](~/ef6/modeling/designer/inheritance/tph.md)
  - [Сопоставление наследования "одна таблица на тип"](~/ef6/modeling/designer/inheritance/tpt.md)
  - [Сопоставление хранимых процедур для обновлений](~/ef6/modeling/designer/stored-procedures/cud.md)
  - [Сопоставление хранимых процедур для запроса](~/ef6/modeling/designer/stored-procedures/query.md)
  - [Разделение сущности](~/ef6/modeling/designer/entity-splitting.md)
  - [Разделение таблицы](~/ef6/modeling/designer/table-splitting.md)
  - [Определение запроса](~/ef6/modeling/designer/advanced/defining-query.md) (дополнительно)
  - [Функции с табличными значениями](~/ef6/modeling/designer/advanced/tvfs.md) (дополнительно)

## <a name="other-resources"></a>Другие источники
  - [Асинхронный запрос и сохранение](~/ef6/fundamentals/async.md)
  - [Привязка данных с помощью WinForms](~/ef6/fundamentals/databinding/winforms.md)
  - [Привязка данных с помощью WPF](~/ef6/fundamentals/databinding/wpf.md)
  - [Отключенные сценарии с самоотслеживающимися сущностями](~/ef6/fundamentals/disconnected-entities/self-tracking-entities/walkthrough.md) (больше не рекомендуется)
