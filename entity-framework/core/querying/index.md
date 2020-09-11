---
title: Запросы к данным — EF Core
description: Общие сведения о запросах в Entity Framework Core
author: smitpatel
ms.date: 10/03/2019
ms.assetid: 7c65ec3e-46c8-48f8-8232-9e31f96c277b
uid: core/querying/index
ms.openlocfilehash: 028c640a17c4946158c86bdf1a663a4050f55921
ms.sourcegitcommit: 7c3939504bb9da3f46bea3443638b808c04227c2
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/09/2020
ms.locfileid: "89616330"
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
