---
title: "Настройка явные значения для создаваемого свойства - EF Core"
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 3f1993c2-cdf5-425b-bac2-a2665a20322b
ms.technology: entity-framework-core
uid: core/saving/explicit-values-generated-properties
ms.openlocfilehash: f34e92d9a3b10b6ff904257ccd047a8acdaad231
ms.sourcegitcommit: 5e2d97e731f975cf3405ff3deab2a3c75ad1b969
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/15/2017
---
# <a name="setting-explicit-values-for-generated-properties"></a>Параметр явные значения для создаваемого свойства.

Созданное свойство — это свойство, значение которого создается (либо EF или базы данных по) при сущности добавлены или обновлены. В разделе [созданные свойства](../modeling/generated-properties.md) для получения дополнительной информации.

Могут возникнуть ситуации, в которой вы хотите задать явное значение для созданного свойства, а не созданные.

> [!TIP]  
> Можно просмотреть в этой статье [пример](https://github.com/aspnet/EntityFramework.Docs/tree/master/samples/core/Saving/Saving/ExplicitValuesGenerateProperties/) на GitHub.

## <a name="the-model"></a>Модели

Модели, используемые в этой статье содержится один `Employee` сущности.

[!code-csharp[Main](../../../samples/core/Saving/Saving/ExplicitValuesGenerateProperties/Employee.cs#Sample)]

## <a name="saving-an-explicit-value-during-add"></a>Сохранение явное значение во время добавления

`Employee.EmploymentStarted` Свойство настроен на значения, сформированные базой данных для новых сущностей (с использованием значения по умолчанию).

[!code-csharp[Main](../../../samples/core/Saving/Saving/ExplicitValuesGenerateProperties/EmployeeContext.cs#EmploymentStarted)]

Следующий код вставляет двух сотрудников в базе данных.
* В первом, присваивается значение не `Employee.EmploymentStarted` , поэтому он остается свойство CLR по умолчанию для `DateTime`.
* Для второго установлен явно заданное значение `1-Jan-2000`.

[!code-csharp[Main](../../../samples/core/Saving/Saving/ExplicitValuesGenerateProperties/Sample.cs#EmploymentStarted)]

Выходные данные показывают, что база данных сгенерировала значение для первого сотрудника и наши явное значение было использовано для второго.

``` Console
1: John Doe, 1/26/2017 12:00:00 AM
2: Jane Doe, 1/1/2000 12:00:00 AM
```

### <a name="explicit-values-into-sql-server-identity-columns"></a>Явные значения в столбцы ИДЕНТИФИКАТОРОВ SQL Server

По соглашению `Employee.EmployeeId` свойство является хранения созданных `IDENTITY` столбца.

В большинстве ситуаций показанный выше подход будет работать для ключевых свойств. Тем не менее чтобы вставить явные значения в SQL Server `IDENTITY` столбец, необходимо вручную включить `IDENTITY_INSERT` перед вызовом `SaveChanges()`.

> [!NOTE]  
> У нас есть [запрос функции](https://github.com/aspnet/EntityFramework/issues/703) на нашем невыполненной работы для этого автоматически в поставщике SQL Server.

[!code-csharp[Main](../../../samples/core/Saving/Saving/ExplicitValuesGenerateProperties/Sample.cs#EmployeeId)]

Выходные данные показывают, что предоставленный идентификаторы были сохранены в базе данных.

``` Console
100: John Doe
101: Jane Doe
```

## <a name="setting-an-explicit-value-during-update"></a>Явное значение во время обновления

`Employee.LastPayRaise` Свойство настроен на значения, сформированные базой данных во время обновления.

[!code-csharp[Main](../../../samples/core/Saving/Saving/ExplicitValuesGenerateProperties/EmployeeContext.cs#LastPayRaise)]

> [!NOTE]  
> По умолчанию основные EF возникает исключение при попытке сохранить явное значение для свойства, которое должно создаваться во время обновления. Чтобы избежать этого, необходимо перейти вниз на нижнем уровне API метаданных и задать `AfterSaveBehavior` (как показано выше).

> [!NOTE]  
> **Изменения в версии 2.0 основные EF:** в предыдущих выпусках было управляются поведение после сохранения `IsReadOnlyAfterSave` флаг. Этот флаг устарел и заменен `AfterSaveBehavior`.

В базе данных для формирования значений для также есть триггер `LastPayRaise` столбца во время `UPDATE` операций.

[!code-sql[Main](../../../samples/core/Saving/Saving/ExplicitValuesGenerateProperties/employee_UPDATE.sql)]

Следующий код увеличивает Зарплата двух сотрудников в базе данных.
* В первом, присваивается значение не `Employee.LastPayRaise` свойство, поэтому он остается установленным в значение null.
* Для второго заданы явное значение одну неделю назад (назад, вышедшие до оплаты raise).

[!code-csharp[Main](../../../samples/core/Saving/Saving/ExplicitValuesGenerateProperties/Sample.cs#LastPayRaise)]

Выходные данные показывают, что база данных сгенерировала значение для первого сотрудника и наши явное значение было использовано для второго.

``` Console
1: John Doe, 1/26/2017 12:00:00 AM
2: Jane Doe, 1/19/2017 12:00:00 AM
```
