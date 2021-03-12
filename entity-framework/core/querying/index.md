---
title: Запросы к данным — EF Core
description: Общие сведения о запросах в Entity Framework Core
author: smitpatel
ms.date: 10/03/2019
uid: core/querying/index
ms.openlocfilehash: 733b44e2f571bd32689b9d8e5d7507bd90e7848d
ms.sourcegitcommit: 4798ab8d04c1fdbe6dd204d94d770fcbf309d09b
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/11/2021
ms.locfileid: "103023735"
---
# <a name="querying-data"></a>Запросы к данным

Entity Framework Core использует LINQ для запроса данных из базы данных. LINQ позволяет использовать C# (или предпочитаемый вами язык .NET) для написания строго типизированных запросов. Он использует производный контекст и классы сущностей для ссылки на объекты базы данных. EF Core передает поставщику базы данных представление запроса LINQ. Поставщик базы данных преобразует его в язык запроса базы данных (например, SQL для реляционной базы данных). Запросы всегда выполняются к базе данных, даже если возвращаемые в результате сущности уже существуют в контексте.

> [!TIP]
> Для этой статьи вы можете скачать [пример](https://github.com/dotnet/EntityFramework.Docs/tree/main/samples/core/Querying/Overview) из репозитория GitHub.

В следующих фрагментах кода показано несколько примеров выполнения типичных задач с использованием Entity Framework Core.

## <a name="loading-all-data"></a>Загрузка всех данных

[!code-csharp[Main](../../../samples/core/Querying/Overview/Program.cs#LoadingAllData)]

## <a name="loading-a-single-entity"></a>Загрузка отдельной сущности

[!code-csharp[Main](../../../samples/core/Querying/Overview/Program.cs#LoadingSingleEntity)]

## <a name="filtering"></a>Фильтрация

[!code-csharp[Main](../../../samples/core/Querying/Overview/Program.cs#Filtering)]

## <a name="further-readings"></a>Дополнительные материалы

- См. сведения о [выражениях запросов LINQ](/dotnet/csharp/programming-guide/concepts/linq/basic-linq-query-operations).
- Дополнительные сведения по обработке запроса в EF Core см. в статье [Как работают запросы](xref:core/querying/how-query-works).
