---
title: Наследование — EF Core
author: rowanmiller
ms.date: 10/27/2016
ms.assetid: 754be334-dd21-450e-9d22-2591e80012a2
uid: core/modeling/inheritance
ms.openlocfilehash: c5fa9d13dec8cfc3e1cac69e471f509cbbb9e4c5
ms.sourcegitcommit: dadee5905ada9ecdbae28363a682950383ce3e10
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/27/2018
ms.locfileid: "42995900"
---
# <a name="inheritance"></a>Наследование

Наследование в модели EF используется для управления представление наследования в классы сущностей в базе данных.

## <a name="conventions"></a>Соглашения

По соглашению возлагается поставщик базы данных, чтобы определить, как наследование, будет представлено в базе данных. См. в разделе [наследование (реляционная база данных)](relational/inheritance.md) это обработки с помощью поставщика реляционной базы данных.

EF будет Настройка наследования только в том случае, если два или более наследуемых типов явно включены в модель. EF не будет выполнять сканирование для базовых или производных типов, в противном случае не включенные в модели. Можно включить типы в модели, предоставляя *DbSet<TEntity>*  для каждого типа в иерархии наследования.

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Samples/InheritanceDbSets.cs?highlight=3-4&name=Model)]

Если вы не хотите предоставлять *DbSet<TEntity>*  для одной или нескольких сущностей в иерархии, можно использовать Fluent API для обеспечения того, они включаются в модель.
Если вы не полагаться на соглашения можно указать базовый тип явно с помощью `HasBaseType`.

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Samples/InheritanceModelBuilder.cs?highlight=7&name=Context)]

> [!NOTE]
> Можно использовать `.HasBaseType((Type)null)` для удаления типа сущности в иерархии.

## <a name="data-annotations"></a>Заметки к данным

Заметки к данным нельзя использовать для настройки наследования.

## <a name="fluent-api"></a>Текучий API

Fluent API для наследования зависит от поставщика базы данных, которую вы используете. См. в разделе [наследование (реляционная база данных)](relational/inheritance.md) для конфигурации, можно выполнить для поставщика реляционной базы данных.
