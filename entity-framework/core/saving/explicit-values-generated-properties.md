---
title: Установка явных значений для создаваемых свойств — EF Core
description: Сведения о явном задании значений для свойств, настроенных как созданные с помощью Entity Framework Core
author: rowanmiller
ms.date: 10/27/2016
uid: core/saving/explicit-values-generated-properties
ms.openlocfilehash: efaa87356a78c4ec7e11d57c1effad776bd01cba
ms.sourcegitcommit: abda0872f86eefeca191a9a11bfca976bc14468b
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/14/2020
ms.locfileid: "90072490"
---
# <a name="setting-explicit-values-for-generated-properties"></a>Установка явных значений для создаваемых свойств

Создаваемое свойство — это свойство, значение которого создается (либо EF, либо базой данных), когда сущность добавляется или обновляется. Дополнительные сведения о создаваемых свойствах см. в [этой статье](xref:core/modeling/generated-properties).

Могут возникать ситуации, когда вы хотите установить явное значение для создаваемого свойства, вместо того чтобы оно было генерировано.

> [!TIP]  
> Для этой статьи вы можете скачать [пример](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Saving/ExplicitValuesGenerateProperties/) из репозитория GitHub.

## <a name="the-model"></a>Модель

Модель, используемая в этой статье, содержит единственную сущность`Employee`.

[!code-csharp[Main](../../../samples/core/Saving/ExplicitValuesGenerateProperties/Employee.cs#Sample)]

## <a name="saving-an-explicit-value-during-add"></a>Сохранение явного значения во время добавления

Свойство `Employee.EmploymentStarted` настроено принимать значения, сгенерированные базой данных для новых сущностей (с использованием значения по умолчанию).

[!code-csharp[Main](../../../samples/core/Saving/ExplicitValuesGenerateProperties/EmployeeContext.cs#EmploymentStarted)]

Следующий код вставляет записи о двух сотрудниках в базу данных.

* Для первого сотрудника значение свойству `Employee.EmploymentStarted` не присваивается, поэтому для `DateTime` остается установленным значение CLR по умолчанию.
* Для второго сотрудника мы установили явное значение `1-Jan-2000`.

[!code-csharp[Main](../../../samples/core/Saving/ExplicitValuesGenerateProperties/Sample.cs#EmploymentStarted)]

В выходных данных показано, что база данных создала значение для первого сотрудника, а наше явное значение использовалось для второго.

``` Console
1: John Doe, 1/26/2017 12:00:00 AM
2: Jane Doe, 1/1/2000 12:00:00 AM
```

### <a name="explicit-values-into-sql-server-identity-columns"></a>Явные значения в столбцах IDENTITY SQL Server

По соглашению свойство `Employee.EmployeeId` — это столбец `IDENTITY`, сгенерированный хранилищем.

Для большинства ситуаций описанный выше подход будет работать для ключевых свойств. Однако, чтобы вставить явные значения в столбец SQL Server `IDENTITY`, вам необходимо вручную включить `IDENTITY_INSERT` перед вызовом `SaveChanges()`.

> [!NOTE]  
> В нашем журнале заказов есть [запрос на функцию](https://github.com/aspnet/EntityFramework/issues/703), которая позволяет сделать это автоматически в пределах поставщика SQL Server.

[!code-csharp[Main](../../../samples/core/Saving/ExplicitValuesGenerateProperties/Sample.cs#EmployeeId)]

Выходные данные показывают, что предоставленные идентификаторы были сохранены в базе данных.

``` Console
100: John Doe
101: Jane Doe
```

## <a name="setting-an-explicit-value-during-update"></a>Установка явного значения во время обновления

Свойство `Employee.LastPayRaise` настроено принимать значения, сгенерированные базой данных во время обновления.

[!code-csharp[Main](../../../samples/core/Saving/ExplicitValuesGenerateProperties/EmployeeContext.cs#LastPayRaise)]

> [!NOTE]  
> По умолчанию EF Core создаст исключение, если вы попытаетесь сохранить явное значение для свойства, для которого настроено создание во время обновления. Чтобы этого избежать, вам нужно перейти к API метаданных нижнего уровня и установить `AfterSaveBehavior` (как показано выше).

> [!NOTE]  
> **Изменения в EF Core 2.0**. В предыдущих версиях поведение после сохранения контролировалось с помощью флага `IsReadOnlyAfterSave`. Этот флаг устарел и заменен `AfterSaveBehavior`.

Существует также триггер базы данных для генерации значений столбца `LastPayRaise` во время операций `UPDATE`.

[!code-sql[Main](../../../samples/core/Saving/ExplicitValuesGenerateProperties/employee_UPDATE.sql)]

Следующий код увеличивает зарплату двух сотрудников в базе данных.

* Для первого сотрудника значение свойству `Employee.LastPayRaise` не присваивается, поэтому оно остается нулевым.
* Для второго сотрудника мы установили явное значение "неделя назад" (до повышения зарплаты).

[!code-csharp[Main](../../../samples/core/Saving/ExplicitValuesGenerateProperties/Sample.cs#LastPayRaise)]

В выходных данных показано, что база данных создала значение для первого сотрудника, а наше явное значение использовалось для второго.

``` Console
1: John Doe, 1/26/2017 12:00:00 AM
2: Jane Doe, 1/19/2017 12:00:00 AM
```
