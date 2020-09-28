---
title: Критические изменения в EF Core 5.0 — EF Core
description: Полный список критических изменений, появившихся в Entity Framework Core 5.0
author: bricelam
ms.date: 09/09/2020
uid: core/what-is-new/ef-core-5.0/breaking-changes
ms.openlocfilehash: 63fd1d1a01b7a72fd34bb9a0130191131306426c
ms.sourcegitcommit: abda0872f86eefeca191a9a11bfca976bc14468b
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/14/2020
ms.locfileid: "90070800"
---
# <a name="breaking-changes-in-ef-core-50"></a>Критические изменения в EF Core 5.0

Указанные ниже изменения в API и поведении могут нарушить работу существующих приложений при их обновлении до EF Core 5.0.0.

## <a name="summary"></a>Сводка

| **Критическое изменение**                                                                                                                   | **Влияние** |
|:--------------------------------------------------------------------------------------------------------------------------------------|------------|
| [Атрибут обязательного поля имеет разную семантику для навигации к основной сущности и навигации к зависимой сущности](#required-dependent)                                 | Средний     |
| [Определение запроса заменено методами, зависящими от поставщика](#defining-query)                                                          | Средний     |
| [Из расширения NTS для SQLite удален метод HasGeometricDimension.](#geometric-sqlite)                                                   | Низкий        |
| [Cosmos: ключ секции теперь добавляется в первичный ключ](#cosmos-partition-key)                                                        | Низкий        |
| [Cosmos: свойство `id` переименовано в `__id`](#cosmos-id)                                                                                 | Низкий        |
| [Cosmos: byte[] теперь хранится в виде строки base64, а не числового массива](#cosmos-byte)                                             | Низкий        |
| [Cosmos: GetPropertyName и SetPropertyName переименованы](#cosmos-metadata)                                                          | Низкий        |
| [Генераторы значений вызываются, когда состояние сущности меняется с "отсоединено" на "не изменено", "обновлено" или "удалено"](#non-added-generation) | Низкий        |
| [IMigrationsModelDiffer теперь использует IRelationalModel](#relational-model)                                                                 | Низкий        |
| [Дискриминаторы доступны только для чтения](#read-only-discriminators)                                                                             | Низкий        |

<a name="geometric-sqlite"></a>

### <a name="removed-hasgeometricdimension-method-from-sqlite-nts-extension"></a>Из расширения NTS для SQLite удален метод HasGeometricDimension.

[Отслеживание проблемы #14257](https://github.com/aspnet/EntityFrameworkCore/issues/14257)

**Старое поведение**

Метод HasGeometricDimension использовался для включения дополнительных измерений (Z и M) для столбцов геометрии. Однако он влиял только на создание базы данных. Его не требовалось указывать для запроса значений с дополнительными измерениями. Он также не работал корректно при вставке или обновлении значений с дополнительными измерениями ([см. проблему #14257](https://github.com/aspnet/EntityFrameworkCore/issues/14257)).

**Новое поведение**

Чтобы включить вставку и обновление геометрических значений с дополнительными измерениями (Z и M), необходимо указать измерение в составе имени типа столбца. Этот интерфейс API более точно соответствует базовой работе функции AddGeometryColumn расширения SpatiaLite.

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

Благодаря добавленной поддержке обязательных зависимых объектов теперь можно пометить любую навигацию по ссылке как обязательную, а это означает, что в случае, показанном выше, внешний ключ будет определен на другой стороне отношения и свойства не будут помечены как обязательные.

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

<a name="cosmos-partition-key"></a>

### <a name="cosmos-partition-key-is-now-added-to-the-primary-key"></a>Cosmos: ключ секции теперь добавляется в первичный ключ

[Отслеживание вопроса № 15289](https://github.com/aspnet/EntityFrameworkCore/issues/15289)

**Старое поведение**

Свойство ключа секции добавлялось только в альтернативный ключ, включающий `id`.

**Новое поведение**

В соответствии с соглашением свойство ключа секции теперь также добавляется в первичный ключ.

**Причина**

Это изменение делает модель более согласованной с семантикой Azure Cosmos DB и повышает производительность `Find` и некоторых запросов.

**Решение проблемы**

Чтобы предотвратить добавление свойства ключа секции в первичный ключ, настройте его в `OnModelCreating`.

```cs
modelBuilder.Entity<Blog>()
    .HasKey(b => b.Id);
```

<a name="cosmos-id"></a>

### <a name="cosmos-id-property-renamed-to-__id"></a>Cosmos: свойство `id` переименовано в `__id`

[Отслеживание вопроса № 17751](https://github.com/aspnet/EntityFrameworkCore/issues/17751)

**Старое поведение**

Теневое свойство, сопоставленное со свойством JSON `id`, также называлось `id`.

**Новое поведение**

Теневое свойство, создаваемое в соответствии с соглашением, теперь называется `__id`.

**Причина**

Это изменение снижает вероятность того, что свойство `id` будет конфликтовать с существующим свойством типа сущности.

**Решение проблемы**

Чтобы вернуться к поведению, принятому в версии 3.x, настройте свойство `id` в `OnModelCreating`.

```cs
modelBuilder.Entity<Blog>()
    .Property<string>("id")
    .ToJsonProperty("id");
```

<a name="cosmos-byte"></a>

### <a name="cosmos-byte-is-now-stored-as-a-base64-string-instead-of-a-number-array"></a>Cosmos: byte[] теперь хранится в виде строки base64, а не числового массива

[Отслеживание вопроса № 17306](https://github.com/aspnet/EntityFrameworkCore/issues/17306)

**Старое поведение**

Свойства типа byte[] хранились в виде числового массива.

**Новое поведение**

Свойства типа byte[] теперь хранятся в виде строки base64.

**Причина**

Такое представление byte[] ближе к ожидаемому и используется по умолчанию в основных библиотеках сериализации JSON.

**Решение проблемы**

Существующие данные, хранящиеся в виде числовых массивов, будут по-прежнему запрашиваться правильно, но в настоящее время не существует способа восстановить прежнее поведение вставки. Если это ограничение не позволяет реализовать ваш сценарий, оставьте комментарий к [этой проблеме](https://github.com/aspnet/EntityFrameworkCore/issues/17306)

<a name="cosmos-metadata"></a>

### <a name="cosmos-getpropertyname-and-setpropertyname-were-renamed"></a>Cosmos: GetPropertyName и SetPropertyName переименованы

[Отслеживание вопроса № 17874](https://github.com/aspnet/EntityFrameworkCore/issues/17874)

**Старое поведение**

Ранее методы расширения назывались `GetPropertyName` и `SetPropertyName`.

**Новое поведение**

Этот интерфейс API устарел, и были добавлены новые методы: `GetJsonPropertyName` и `SetJsonPropertyName`.

**Причина**

Данное изменение устраняет неоднозначность в отношении того, что настраивают эти методы.

**Решение проблемы**

Используйте новый интерфейс API или временно отключите предупреждения об устаревших возможностях.

<a name="non-added-generation"></a>

### <a name="value-generators-are-called-when-the-entity-state-is-changed-from-detached-to-unchanged-updated-or-deleted"></a>Генераторы значений вызываются, когда состояние сущности меняется с "отсоединено" на "не изменено", "обновлено" или "удалено"

[Отслеживание вопроса № 15289](https://github.com/aspnet/EntityFrameworkCore/issues/15289)

**Старое поведение**

Генераторы значений вызывались только при изменении состояния сущности на «добавлено».

**Новое поведение**

Генераторы значений теперь вызываются, когда состояние сущности меняется с "отсоединено" на "не изменено", "обновлено" или "удалено", а свойство содержит значения по умолчанию.

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

<a name="defining-query"></a>

### <a name="defining-query-is-replaced-with-provider-specific-methods"></a>Определение запроса заменено методами, зависящими от поставщика

[Отслеживание вопроса № 18903](https://github.com/dotnet/efcore/issues/18903)

**Старое поведение**

Типы сущностей сопоставлялись с определяющими запросами на уровне ядра. Каждый раз, когда тип сущности использовался в запросе, корень типа сущности заменялся определяющим запросом для любого поставщика.

**Новое поведение**

Интерфейсы API для определяющего запроса стали нерекомендуемыми. Появились новые интерфейсы API для конкретных поставщиков.

**Причина**

Хотя определяющий запрос был реализован как заменяющий при использовании корня запроса в запросе, существовал ряд проблем.

- Если определяющий запрос проецировал тип сущности с помощью `new { ... }` в методе `Select`, определение его как сущности требовало дополнительной работы и не согласовывалось с тем, как номинальные типы в запросе обрабатываются в EF Core.
- Для реляционных поставщиков `FromSql` по-прежнему требуется для передачи строки SQL в виде выражения LINQ.

Изначально определяющие запросы появились как представления на стороне клиента, предназначенные для использования с поставщиком в памяти для сущностей без ключей (аналогично представлениям базы данных в реляционных базах данных). Такое определение упрощает тестирование приложения с базой данных в памяти. Впоследствии они стали применяться более широко. Это было полезной возможностью, но вносило несогласованность и затрудняло понимание. Поэтому мы решили упростить принцип их работы. Теперь определяющий запрос на основе LINQ предназначен исключительно для поставщика в памяти и обрабатывается по-особому. Дополнительные сведения см. в [этой проблеме](https://github.com/dotnet/efcore/issues/20023).

**Решение проблемы**

Для реляционных поставщиков используйте метод `ToSqlQuery` в `OnModelCreating` и передайте строку SQL, которая должна использоваться для типа сущности.
Для поставщика в памяти используйте метод `ToInMemoryQuery` в `OnModelCreating` и передайте запрос SQL, который должен использоваться для типа сущности.