---
title: Начало работы с платформой Entity Framework 6 — EF6
description: Начало работы с платформой Entity Framework 6
author: divega
ms.date: 10/23/2016
uid: ef6/get-started
ms.openlocfilehash: 825fe3716ade947f1660c8131100a1c3acba5a1d
ms.sourcegitcommit: abda0872f86eefeca191a9a11bfca976bc14468b
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/14/2020
ms.locfileid: "90072139"
---
# <a name="get-started-with-entity-framework-6"></a>Начало работы с платформой Entity Framework 6

Это руководство содержит набор ссылок на отдельные статьи с документацией, пошаговые руководства и видео, которые помогут быстро начать работу.

## <a name="fundamentals"></a>Основы

* [Установка Entity Framework](xref:ef6/fundamentals/install)

  Здесь вы узнаете, как добавить Entity Framework в свои приложения. Если вы хотите использовать конструктор EF, установите его в Visual Studio.

* [Создание модели: Code First, конструктор EF и рабочие процессы EF](xref:ef6/modeling/index)

  Вы предпочитаете задать модель EF, написав код или нарисовав поля и линии?
Вы собираетесь использовать EF для сопоставления объектов с существующей базой данных или хотите создать в EF базу данных, настроенную для ваших объектов?
Здесь вы узнаете о двух разных подходах к использованию EF6: конструктор EF и Code First.
Обязательно подпишитесь на обсуждение и посмотрите видео о различиях между ними.

* [Работа с DbContext](xref:ef6/fundamentals/working-with-dbcontext)

  DbContext — это первый и самый важный тип EF, который вам нужно научиться использовать. Он служит источником запросов базы данных и отслеживает изменения, внесенные в объекты, чтобы их можно было сохранить в базе данных.

* [Вопросы](xref:ef6/resources/get-help)

  Вы узнаете, как получить справку от экспертов и сможете сами отвечать на вопросы сообщества.

* [Участие](https://github.com/aspnet/EntityFramework6/)

  В Entity Framework 6 используется модель разработки с открытым исходным кодом. Посетите репозиторий GitHub, чтобы узнать, что поможет улучшить EF.

## <a name="code-first-resources"></a>Ресурсы по Code First

  - [Code First для рабочего процесса существующей базы данных](xref:ef6/modeling/code-first/workflows/existing-database)
  - [Code First для рабочего процесса новой базы данных](xref:ef6/modeling/code-first/workflows/new-database)
  - [Сопоставление перечислений с помощью Code First](xref:ef6/modeling/code-first/data-types/enums)
  - [Сопоставление пространственных типов с помощью Code First](xref:ef6/modeling/code-first/data-types/spatial)
  - [Запись пользовательских соглашений Code First](xref:ef6/modeling/code-first/conventions/custom)
  - [Настройка Code First для использования текучего API в Visual Basic](xref:ef6/modeling/code-first/fluent/vb)
  - [Code First Migrations](xref:ef6/modeling/code-first/migrations/index)
  - [Code First Migrations в командных средах](xref:ef6/modeling/code-first/migrations/teams)
  - [Автоматические миграции Code First Migrations](xref:ef6/modeling/code-first/migrations/automatic) (больше не рекомендуется)

## <a name="ef-designer-resources"></a>Ресурсы по конструктору EF
  - [Рабочий процесс Database First](xref:ef6/modeling/designer/workflows/database-first)
  - [Рабочий процесс Model First](xref:ef6/modeling/designer/workflows/model-first)
  - [Сопоставление перечислений](xref:ef6/modeling/designer/data-types/enums)
  - [Сопоставление пространственных типов](xref:ef6/modeling/designer/data-types/spatial)
  - [Сопоставление наследования "одна таблица на иерархию"](xref:ef6/modeling/designer/inheritance/tph)
  - [Сопоставление наследования "одна таблица на тип"](xref:ef6/modeling/designer/inheritance/tpt)
  - [Сопоставление хранимых процедур для обновлений](xref:ef6/modeling/designer/stored-procedures/cud)
  - [Сопоставление хранимых процедур для запроса](xref:ef6/modeling/designer/stored-procedures/query)
  - [Разделение сущности](xref:ef6/modeling/designer/entity-splitting)
  - [Разделение таблицы](xref:ef6/modeling/designer/table-splitting)
  - [Определение запроса](xref:ef6/modeling/designer/advanced/defining-query) (дополнительно)
  - [Функции с табличными значениями](xref:ef6/modeling/designer/advanced/tvfs) (дополнительно)

## <a name="other-resources"></a>Другие ресурсы
  - [Асинхронный запрос и сохранение](xref:ef6/fundamentals/async)
  - [Привязка данных с помощью WinForms](xref:ef6/fundamentals/databinding/winforms)
  - [Привязка данных с помощью WPF](xref:ef6/fundamentals/databinding/wpf)
  - [Отключенные сценарии с самоотслеживающимися сущностями](xref:ef6/fundamentals/disconnected-entities/self-tracking-entities/walkthrough) (больше не рекомендуется)
