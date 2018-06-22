---
title: Наследование - EF Core
author: rowanmiller
ms.author: divega
ms.date: 10/27/2016
ms.assetid: 754be334-dd21-450e-9d22-2591e80012a2
ms.technology: entity-framework-core
uid: core/modeling/inheritance
ms.openlocfilehash: f0394bc55dfbfea8277b1ddf898361165dd1fe51
ms.sourcegitcommit: 01a75cd483c1943ddd6f82af971f07abde20912e
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/27/2017
ms.locfileid: "26052784"
---
# <a name="inheritance"></a>Наследование

Наследование в модели EF используется для управления, как наследование в классах сущностей представляется в базе данных.

## <a name="conventions"></a>Соглашения

По соглашению именно поставщик базы данных, чтобы определить представление наследования в базе данных. В разделе [наследования (реляционной базы данных)](relational/inheritance.md) это обработки с помощью реляционной базы данных поставщика.

EF будет настройте наследования только в том случае, если два или более наследуемых типов явно включены в модель. EF не будут проверять базового или производного типа, в противном случае не включенные в модели. Типы можно включить в модель, предоставляя *DbSet<TEntity>*  для каждого типа в иерархии наследования.

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Samples/InheritanceDbSets.cs?highlight=3-4&name=Model)]

Если вы не хотите предоставлять *DbSet<TEntity>*  для одной или нескольких сущностей в иерархии, чтобы они включаются в модель можно использовать Fluent API.
Если не полагаться на соглашения можно указать базовый тип явно с помощью `HasBaseType`.

[!code-csharp[Main](../../../samples/core/Modeling/Conventions/Samples/InheritanceModelBuilder.cs?highlight=7&name=Context)]

> [!NOTE]
> Можно использовать `.HasBaseType((Type)null)` для удаления типа сущности в иерархии.

## <a name="data-annotations"></a>Заметки к данным

Заметки данных нельзя использовать для настройки наследования.

## <a name="fluent-api"></a>Fluent API

Гибкий интерфейс API для наследования зависит от поставщика базы данных, которую вы используете. В разделе [наследования (реляционной базы данных)](relational/inheritance.md) для конфигурации, можно выполнять для поставщика реляционной базы данных.
