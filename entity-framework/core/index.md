---
title: Обзор Entity Framework Core — EF Core
description: Общие вводные сведения об Entity Framework Core
author: rowanmiller
ms.date: 10/27/2016
uid: core/index
ms.openlocfilehash: 1d320ecae06d9c05fe3a14ec955f7151de6a56e5
ms.sourcegitcommit: abda0872f86eefeca191a9a11bfca976bc14468b
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/14/2020
ms.locfileid: "90071931"
---
# <a name="entity-framework-core"></a>Entity Framework Core

Entity Framework (EF) Core — это простая, кроссплатформенная и расширяемая версия популярной технологии доступа к данным Entity Framework [с открытым исходным кодом](https://github.com/aspnet/EntityFrameworkCore).

EF Core может использоваться как объектно реляционный модуль сопоставления (O/RM), позволяя разработчикам .NET работать с базой данных с помощью объектов .NET и устраняя необходимость в написании большей части кода, требуемого для доступа к данным.

EF Core поддерживает множество систем баз данных. Дополнительные сведения см. в разделе [Поставщики баз данных](xref:core/providers/index).

## <a name="the-model"></a>Модель

В EF Core доступ к данным осуществляется с помощью модели. Модель состоит из классов сущностей и объекта контекста, который представляет сеанс взаимодействия с базой данных, позволяя запрашивать и сохранять данные. Дополнительные сведения см. в разделе [Создание модели](xref:core/modeling/index).

Вы можете создать модель из существующей базы данных, вручную составить код модели, соответствующий базе данных, или использовать [миграции EF](xref:core/managing-schemas/migrations/index) для создания базы данных из модели (и ее модификации по мере изменения модели).

[!code-csharp[Main](../../samples/core/Intro/Model.cs)]

## <a name="querying"></a>Выполнение запроса

Экземпляры классов сущностей извлекаются из базы данных с помощью LINQ. Дополнительные сведения см. в разделе [Запросы к данным](xref:core/querying/index).

[!code-csharp[Main](../../samples/core/Intro/Program.cs#Querying)]

## <a name="saving-data"></a>Сохранение данных

Для создания, удаления и изменения данных в базе данных используются экземпляры классов сущностей. Дополнительные сведения см. в разделе [Сохранение данных](xref:core/saving/index).

[!code-csharp[Main](../../samples/core/Intro/Program.cs#SavingData)]

## <a name="next-steps"></a>Дальнейшие действия

Вводные руководства см. в разделе [Начало работы с Entity Framework Core](xref:core/get-started/index).
