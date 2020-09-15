---
title: Критические изменения в EF Core 5.0 — EF Core
description: Полный список критических изменений, появившихся в Entity Framework Core 5.0
author: bricelam
ms.date: 09/08/2020
uid: core/what-is-new/ef-core-5.0/breaking-changes
ms.openlocfilehash: bc6db48edcd7406b31ec2b4369cabf5d55fb4578
ms.sourcegitcommit: 7c3939504bb9da3f46bea3443638b808c04227c2
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/09/2020
ms.locfileid: "89618675"
---
# <a name="breaking-changes-in-ef-core-50"></a>Критические изменения в EF Core 5.0

Указанные ниже изменения в API и поведении могут нарушить работу существующих приложений при их обновлении до EF Core 5.0.0.

## <a name="summary"></a>Сводка

| **Критическое изменение**                                                                                                                   | **Влияние** |
|:--------------------------------------------------------------------------------------------------------------------------------------|------------|
| [Атрибут обязательного поля имеет разную семантику для навигации к основной сущности и навигации к зависимой сущности](#required-dependent)                                 | Средний     |
| [Из расширения NTS для SQLite удален метод HasGeometricDimension.](#geometric-sqlite)                                                   | Низкий        |
| [Генераторы значений вызываются, когда состояние сущности меняется с «отсоединено» на «не изменено», «обновлено» или «удалено»](#non-added-generation)  | Низкий        |
| [IMigrationsModelDiffer теперь использует IRelationalModel](#relational-model)                                                                 | Низкий        |
| [Дискриминаторы доступны только для чтения](#read-only-discriminators)                                                                             | Низкий        |

<a name="geometric-sqlite"></a>
### <a name="removed-hasgeometricdimension-method-from-sqlite-nts-extension"></a>Из расширения NTS для SQLite удален метод HasGeometricDimension.

[Отслеживание проблемы #14257](https://github.com/aspnet/EntityFrameworkCore/issues/14257)

**Старое поведение**

Метод HasGeometricDimension использовался для включения дополнительных измерений (Z и M) для столбцов геометрии. Однако он влиял только на создание базы данных. Его не требовалось указывать для запроса значений с дополнительными измерениями. Он также не работал корректно при вставке или обновлении значений с дополнительными измерениями ([см. проблему #14257](https://github.com/aspnet/EntityFrameworkCore/issues/14257)).

**Новое поведение**

Чтобы включить вставку и обновление геометрических значений с дополнительными измерениями (Z и M), необходимо указать измерение в составе имени типа столбца. Это более точно соответствует базовой работе функции AddGeometryColumn расширения SpatiaLite.

**Причина**

Использование метода HasGeometricDimension после указания измерения в типе столбца является ненужным и избыточным, поэтому этот метод полностью удален.

**Решение проблемы**

Чтобы указать измерение, используйте `HasColumnType`:

```cs
modelBuilder.Entity<GeoEntity>(
    x =>
    {
        // Allow any GEOMETRY value with optional Z and M values
        x.Property(e => e.Geometry).HasColumnType("GEOMETRYZM");

        // Allow only POINT values with an optional Z value
        x.Property(e => e.Point).HasColumnType("POINTZ");
    });
```

<a name="required-dependent"></a>
### <a name="required-on-the-navigation-from-principal-to-dependent-has-different-semantics"></a>Атрибут обязательного поля имеет разную семантику для навигации к основной сущности и навигации к зависимой сущности

[Отслеживание вопроса № 17286](https://github.com/aspnet/EntityFrameworkCore/issues/17286)

**Старое поведение**

При необходимости можно настроить только навигацию к основной сущности. Поэтому при применении `RequiredAttribute` к переходу к зависимой сущности (сущности, содержащей внешний ключ) вместо этого будет создан внешний ключ для определяющего типа сущности.

**Новое поведение**

Благодаря добавленной поддержке обязательных зависимых объектов теперь можно пометить любую навигацию по ссылке как обязательную, а это означает, что в случае, показанном выше, внешний ключ будет определен на другой стороне отношения, и свойства не будут помечены как обязательные.

Вызов `IsRequired` перед указанием зависимого элемента теперь неоднозначен.

```cs
modelBuilder.Entity<Blog>()
    .HasOne(b => b.BlogImage)
    .WithOne(i => i.Blog)
    .IsRequired()
    .HasForeignKey<BlogImage>(b => b.BlogForeignKey);
```

**Причина**

Необходимо новое поведение для обеспечения поддержки требуемых зависимых объектов ([см. #12100](https://github.com/dotnet/efcore/issues/12100)).

**Решение проблемы**

Удалите атрибут `RequiredAttribute` из навигации в зависимый объект и поместите его вместо этого в навигацию к основному объекту или настройте связь в `OnModelCreating`.

```cs
modelBuilder.Entity<Blog>()
    .HasOne(b => b.BlogImage)
    .WithOne(i => i.Blog)
    .HasForeignKey<BlogImage>(b => b.BlogForeignKey)
    .IsRequired();
```

<a name="non-added-generation"></a>
### <a name="value-generators-are-called-when-the-entity-state-is-changed-from-detached-to-unchanged-updated-or-deleted"></a>Генераторы значений вызываются, когда состояние сущности меняется с «отсоединено» на «не изменено», «обновлено» или «удалено»

[Отслеживание вопроса № 15289](https://github.com/aspnet/EntityFrameworkCore/issues/15289)

**Старое поведение**

Генераторы значений вызывались только при изменении состояния сущности на «добавлено».

**Новое поведение**

Генераторы значений теперь вызываются, когда состояние сущности меняется с «отсоединено» на «не изменено», «обновлено» или «удалено», а свойство содержит значения по умолчанию.

**Причина**

Это изменение было необходимо для улучшения работы со свойствами, которые не сохраняются в хранилище данных, а их значения всегда создаются на клиенте.

**Решение проблемы**

Чтобы предотвратить вызов генератора значений, присвойте свойству значение, отличное от значения по умолчанию, до изменения состояния.

<a name="relational-model"></a>
### <a name="imigrationsmodeldiffer-now-uses-irelationalmodel"></a>IMigrationsModelDiffer теперь использует IRelationalModel

[Отслеживание вопроса № 20305](https://github.com/aspnet/EntityFrameworkCore/issues/20305)

**Старое поведение**

API `IMigrationsModelDiffer` определялся с помощью `IModel`.

**Новое поведение**

Теперь API `IMigrationsModelDiffer` использует `IRelationalModel`. Однако моментальный снимок модели по-прежнему содержит только `IModel`, так как этот код является частью приложения, и Entity Framework не может изменить его без внесения значительных изменений.

**Причина**

`IRelationalModel` — это вновь добавленное представление схемы базы данных. Его использование для поиска различий выполняется быстрее и точнее.

**Решение проблемы**

Используйте следующий код для сравнения модели из `snapshot` с моделью из `context`.

```cs
var dependencies = context.GetService<ProviderConventionSetBuilderDependencies>();
var relationalDependencies = context.GetService<RelationalConventionSetBuilderDependencies>();

var typeMappingConvention = new TypeMappingConvention(dependencies);
typeMappingConvention.ProcessModelFinalizing(((IConventionModel)modelSnapshot.Model).Builder, null);

var relationalModelConvention = new RelationalModelConvention(dependencies, relationalDependencies);
var sourceModel = relationalModelConvention.ProcessModelFinalized(snapshot.Model);

var modelDiffer = context.GetService<IMigrationsModelDiffer>();
var hasDifferences = modelDiffer.HasDifferences(
    ((IMutableModel)sourceModel).FinalizeModel().GetRelationalModel(),
    context.Model.GetRelationalModel());
```

Мы планируем улучшить этот процесс в версии 6.0 ([см. #22031](https://github.com/dotnet/efcore/issues/22031)).

<a name="read-only-discriminators"></a>
### <a name="discriminators-are-read-only"></a>Дискриминаторы доступны только для чтения

[Отслеживание вопроса № 21154](https://github.com/aspnet/EntityFrameworkCore/issues/21154)

**Старое поведение**

Можно было изменить значение дискриминатора перед вызовом метода `SaveChanges`

**Новое поведение**

В приведенном выше случае будет вызвано исключение.

**Причина**

EF не ожидает изменения типа сущности, пока он еще отслеживается, поэтому изменение значения дискриминатора оставляет контекст в несогласованном состоянии, что может привести к непредвиденному поведению.

**Решение проблемы**

Если необходимо изменить значение дискриминатора и контекст будет удален сразу после вызова `SaveChanges`, дискриминатор можно сделать изменяемым.

```cs
modelBuilder.Entity<BaseEntity>()
    .Property<string>("Discriminator")
    .Metadata.SetAfterSaveBehavior(PropertySaveBehavior.Save);
```
