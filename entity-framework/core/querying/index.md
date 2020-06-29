---
title: Запросы к данным — EF Core
author: smitpatel
ms.date: 10/03/2019
ms.assetid: 7c65ec3e-46c8-48f8-8232-9e31f96c277b
uid: core/querying/index
ms.openlocfilehash: 82f8a2e8e78e9d82f46f2fc6ced6dd9141329a22
ms.sourcegitcommit: ebfd3382fc583bc90f0da58e63d6e3382b30aa22
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/25/2020
ms.locfileid: "85370345"
---
# <a name="querying-data"></a>Запросы к данным

Entity Framework Core использует LINQ для запроса данных из базы данных. LINQ позволяет использовать C# (или предпочитаемый вами язык .NET) для написания строго типизированных запросов. Он использует производный контекст и классы сущностей для ссылки на объекты базы данных. EF Core передает поставщику базы данных представление запроса LINQ. Поставщик базы данных преобразует его в язык запроса базы данных (например, SQL для реляционной базы данных).

> [!TIP]
> Для этой статьи вы можете скачать [пример](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples/core/Querying) из репозитория GitHub.

В следующих фрагментах кода показано несколько примеров выполнения типичных задач с использованием Entity Framework Core.

## <a name="loading-all-data"></a>Загрузка всех данных

[!code-csharp[Main](../../../samples/core/Querying/Basics/Sample.cs#LoadingAllData)]

## <a name="loading-a-single-entity"></a>Загрузка отдельной сущности

[!code-csharp[Main](../../../samples/core/Querying/Basics/Sample.cs#LoadingSingleEntity)]

## <a name="filtering"></a>Фильтрация

[!code-csharp[Main](../../../samples/core/Querying/Basics/Sample.cs#Filtering)]

## <a name="further-readings"></a>Дополнительные материалы

- См. сведения о [выражениях запросов LINQ](/dotnet/csharp/programming-guide/concepts/linq/basic-linq-query-operations).
- Дополнительные сведения по обработке запроса в EF Core см. в статье [Как работают запросы](xref:core/querying/how-query-works).
