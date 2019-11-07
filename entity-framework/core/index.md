---
title: Обзор Entity Framework Core — EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: bc2a2676-bc46-493f-bf49-e3cc97994d57
uid: core/index
ms.openlocfilehash: e6127f775d6bbbdf81debf5519388fe252fe079d
ms.sourcegitcommit: 18ab4c349473d94b15b4ca977df12147db07b77f
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/06/2019
ms.locfileid: "73655624"
---
# <a name="entity-framework-core"></a>Entity Framework Core

Entity Framework (EF) Core — это простая, кроссплатформенная и расширяемая версия популярной технологии доступа к данным Entity Framework [с открытым исходным кодом](https://github.com/aspnet/EntityFrameworkCore).

EF Core может использоваться как объектно реляционный модуль сопоставления (O/RM), позволяя разработчикам .NET работать с базой данных с помощью объектов .NET и устраняя необходимость в написании большей части кода, требуемого для доступа к данным.

EF Core поддерживает множество систем баз данных. Дополнительные сведения см. в разделе [Поставщики баз данных](providers/index.md).

## <a name="the-model"></a>Модель

В EF Core доступ к данным осуществляется с помощью модели. Модель состоит из классов сущностей и объекта контекста, который представляет сеанс взаимодействия с базой данных, позволяя запрашивать и сохранять данные. Дополнительные сведения см. в разделе [Создание модели](modeling/index.md).

Вы можете создать модель из существующей базы данных, вручную составить код модели, соответствующий базе данных, или использовать [миграции EF](managing-schemas/migrations/index.md) для создания базы данных из модели (и ее модификации по мере изменения модели).

[!code-csharp[Main](../../samples/core/Intro/Model.cs)]

## <a name="querying"></a>Выполнение запросов

Экземпляры классов сущностей извлекаются из базы данных с помощью LINQ. Дополнительные сведения см. в разделе [Запросы к данным](querying/index.md).

[!code-csharp[Main](../../samples/core/Intro/Program.cs#Querying)]

## <a name="saving-data"></a>Сохранение данных

Для создания, удаления и изменения данных в базе данных используются экземпляры классов сущностей. Дополнительные сведения см. в разделе [Сохранение данных](saving/index.md).

[!code-csharp[Main](../../samples/core/Intro/Program.cs#SavingData)]

## <a name="next-steps"></a>Следующие шаги

Вводные руководства см. в разделе [Начало работы с Entity Framework Core](get-started/index.md).
