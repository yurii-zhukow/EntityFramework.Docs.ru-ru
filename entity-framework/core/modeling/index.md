---
title: Создание и настройка модели — EF Core
description: Общие сведения о создании и настройке модели с помощью Entity Framework Core
author: rowanmiller
ms.date: 11/05/2019
ms.assetid: 88253ff3-174e-485c-b3f8-768243d01ee1
uid: core/modeling/index
ms.openlocfilehash: e980f11b08bee7b07156a80c6bead829e7a8b654
ms.sourcegitcommit: 7c3939504bb9da3f46bea3443638b808c04227c2
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/09/2020
ms.locfileid: "89616754"
---
# <a name="creating-and-configuring-a-model"></a>Создание и настройка модели

Entity Framework использует набор соглашений для создания модели на основе формы классов сущностей. Вы можете указать дополнительную конфигурацию, чтобы дополнить и (или) переопределить данные, обнаруженные соглашением.

Эта статья описывает конфигурацию, которую можно применить к модели, ориентированной на любое хранилище данных, а также при ориентации на любую реляционную базу данных. Поставщики также могут включить конфигурацию, предназначенную для конкретного хранилища данных. Документацию по конфигурации для конкретных поставщиков см. в разделе  [Поставщики баз данных](xref:core/providers/index) .

> [!TIP]  
>  [Пример](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples) для этой статьи можно скачать в GitHub.

## <a name="use-fluent-api-to-configure-a-model"></a>Использование текучего API для настройки модели

Вы можете переопределить `OnModelCreating` метод в производном контексте и использовать `ModelBuilder API` для настройки модели. Это наиболее эффективный метод настройки, который позволяет задать конфигурацию без изменения ваших классов сущностей. Конфигурация текучего API имеет самый высокий приоритет и переопределяет соглашения и заметки к данным.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Required.cs?highlight=12-14)]

## <a name="use-data-annotations-to-configure-a-model"></a>Использование заметок к данным для настройки модели

Вы также можете применять для классов и свойств атрибуты (называемые заметками к данным). Заметки к данным переопределяют соглашения, но они будут перезаписаны конфигурацией текучего API.

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Required.cs?highlight=15)]
