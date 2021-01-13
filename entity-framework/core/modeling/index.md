---
title: Создание и настройка модели — EF Core
description: Общие сведения о создании и настройке модели с помощью Entity Framework Core
author: AndriySvyryd
ms.date: 10/13/2020
uid: core/modeling/index
ms.openlocfilehash: 709e2bde60c8e2c31f0a39390624c5d31a9cfa08
ms.sourcegitcommit: 032a1767d7a6e42052a005f660b80372c6521e7e
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/12/2021
ms.locfileid: "98129204"
---
# <a name="creating-and-configuring-a-model"></a>Создание и настройка модели

Entity Framework использует набор соглашений для создания модели на основе формы классов сущностей. Вы можете указать дополнительную конфигурацию, чтобы дополнить и (или) переопределить данные, обнаруженные соглашением.

Эта статья описывает конфигурацию, которую можно применить к модели, ориентированной на любое хранилище данных, а также при ориентации на любую реляционную базу данных. Поставщики также могут включить конфигурацию, предназначенную для конкретного хранилища данных. Документацию по конфигурации для конкретного поставщика см. в разделе [Поставщики баз данных](xref:core/providers/index).

> [!TIP]
> Для этой статьи вы можете просмотреть [пример](https://github.com/dotnet/EntityFramework.Docs/tree/master/samples) в репозитории GitHub.

## <a name="use-fluent-api-to-configure-a-model"></a>Использование текучего API для настройки модели

Вы можете переопределить метод `OnModelCreating` в производном контексте и использовать `ModelBuilder API` для настройки модели. Это наиболее эффективный метод настройки, который позволяет задать конфигурацию без изменения ваших классов сущностей. Конфигурация текучего API имеет самый высокий приоритет и переопределяет соглашения и заметки к данным.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/Required.cs?highlight=12-14)]

### <a name="grouping-configuration"></a>Группирование конфигурации

Чтобы уменьшить размер метода <xref:System.Data.Entity.DbContext.OnModelCreating%2A>, вы можете извлечь все конфигурации для типа сущности в отдельный класс, реализующий <xref:Microsoft.EntityFrameworkCore.IEntityTypeConfiguration%601>.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/EntityTypeConfiguration.cs?Name=IEntityTypeConfiguration)]

Затем просто вызовите метод `Configure` из `OnModelCreating`.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/EntityTypeConfiguration.cs?Name=ApplyIEntityTypeConfiguration)]

Вы можете применить все конфигурации, заданные в типах, реализующих `IEntityTypeConfiguration` в определенной сборке.

[!code-csharp[Main](../../../samples/core/Modeling/FluentAPI/EntityTypeConfiguration.cs?Name=ApplyConfigurationsFromAssembly)]

> [!NOTE]
> Порядок, в котором будут применяться конфигурации, не определен. Поэтому этот метод следует использовать только в том случае, если порядок не имеет значения.

## <a name="use-data-annotations-to-configure-a-model"></a>Использование заметок к данным для настройки модели

Вы также можете применять для классов и свойств атрибуты (называемые заметками к данным). Заметки к данным переопределяют соглашения, но они будут перезаписаны конфигурацией текучего API.

[!code-csharp[Main](../../../samples/core/Modeling/DataAnnotations/Required.cs?highlight=15)]
