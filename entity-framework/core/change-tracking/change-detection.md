---
title: Обнаружение изменений и уведомления — EF Core
description: Обнаружение изменений свойств и связей с помощью DetectChanges или уведомлений
author: ajcvickers
ms.date: 12/30/2020
uid: core/change-tracking/change-detection
ms.openlocfilehash: fae8bdb1a89478531535b377f4ba8b02d1c848f4
ms.sourcegitcommit: 4798ab8d04c1fdbe6dd204d94d770fcbf309d09b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/11/2021
ms.locfileid: "103023189"
---
# <a name="change-detection-and-notifications"></a>Обнаружение изменений и уведомления

Каждый экземпляр <xref:Microsoft.EntityFrameworkCore.DbContext> отслеживает изменения, внесенные в сущности. Эти отслеживаемые сущности, в свою очередь, записывают изменения в базу данных при вызове <xref:Microsoft.EntityFrameworkCore.DbContext.SaveChanges%2A>. Это рассматривается в [Отслеживание изменений в EF Core](xref:core/change-tracking/index), и в этом документе предполагается, что состояния сущностей и основы отслеживания изменений Entity Framework Core (EF Core) понятны.

Для отслеживания изменений свойств и связей требуется, чтобы DbContext мог обнаруживать эти изменения. В этом документе описано, как происходит это обнаружение, а также как использовать уведомления о свойствах или прокси-серверы отслеживания изменений для немедленного обнаружения изменений.

> [!TIP]
> Вы можете запустить и отладить весь код, используемый в этой документации, [скачав пример кода из GitHub](https://github.com/dotnet/EntityFramework.Docs/tree/main/samples/core/ChangeTracking/ChangeDetectionAndNotifications).

## <a name="snapshot-change-tracking"></a>Отслеживание изменений по моментальному снимку

По умолчанию EF Core создает моментальный снимок значений свойств каждой сущности при ее первой отслеживании экземпляром DbContext. Значения, хранящиеся в этом снимке, затем сравниваются с текущими значениями сущности, чтобы определить, какие значения свойств изменились.

Это обнаружение изменений происходит при вызове SaveChanges, чтобы гарантировать, что все измененные значения будут обнаружены перед отправкой обновлений в базу данных. Однако обнаружение изменений также происходит в других случаях, чтобы обеспечить работу приложения с актуальными данными отслеживания. Обнаружение изменений можно принудительно выполнить в любое время, вызвав <xref:Microsoft.EntityFrameworkCore.ChangeTracking.ChangeTracker.DetectChanges?displayProperty=nameWithType> .

### <a name="when-change-detection-is-needed"></a>Когда требуется обнаружение изменений

Обнаружение изменений необходимо при изменении свойства или навигации _без использования EF Core для внесения этого изменения_. Например, можно загрузить блоги и записи, а затем внести изменения в эти сущности:

<!--
        using var context = new BlogsContext();
        var blog = context.Blogs.Include(e => e.Posts).First(e => e.Name == ".NET Blog");

        // Change a property value
        blog.Name = ".NET Blog (Updated!)";

        // Add a new entity to a navigation
        blog.Posts.Add(new Post
        {
            Title = "What’s next for System.Text.Json?",
            Content = ".NET 5.0 was released recently and has come with many..."
        });

        Console.WriteLine(context.ChangeTracker.DebugView.LongView);
        context.ChangeTracker.DetectChanges();
        Console.WriteLine(context.ChangeTracker.DebugView.LongView);
-->
[!code-csharp[Snapshot_change_tracking_1](../../../samples/core/ChangeTracking/ChangeDetectionAndNotifications/SnapshotSamples.cs?name=Snapshot_change_tracking_1)]

[Просмотр представления отладки средства записи изменений](xref:core/change-tracking/debug-views) перед вызовом <xref:Microsoft.EntityFrameworkCore.ChangeTracking.ChangeTracker.DetectChanges?displayProperty=nameWithType> показывает, что внесенные изменения не были обнаружены и, следовательно, не отражаются в состояниях сущностей и измененных данных свойств:

```output
Blog {Id: 1} Unchanged
  Id: 1 PK
  Name: '.NET Blog (Updated!)' Originally '.NET Blog'
  Posts: [{Id: 1}, {Id: 2}, <not found>]
Post {Id: 1} Unchanged
  Id: 1 PK
  BlogId: 1 FK
  Content: 'Announcing the release of EF Core 5.0, a full featured cross...'
  Title: 'Announcing the Release of EF Core 5.0'
  Blog: {Id: 1}
Post {Id: 2} Unchanged
  Id: 2 PK
  BlogId: 1 FK
  Content: 'F# 5 is the latest version of F#, the functional programming...'
  Title: 'Announcing F# 5'
  Blog: {Id: 1}
```

В частности, состояние записи в блоге остается `Unchanged` , а новая запись не отображается в виде отслеживающей сущности. (Внимательный заметит, что свойства сообщают о своих новых значениях, даже если эти изменения еще не обнаружены EF Core. Это происходит потому, что представление отладки считывает текущие значения непосредственно из экземпляра сущности.)

Сравните это с представлением отладки после вызова DetectChanges:

```output
Blog {Id: 1} Modified
  Id: 1 PK
  Name: '.NET Blog (Updated!)' Modified Originally '.NET Blog'
  Posts: [{Id: 1}, {Id: 2}, {Id: -2147482643}]
Post {Id: -2147482643} Added
  Id: -2147482643 PK Temporary
  BlogId: 1 FK
  Content: '.NET 5.0 was released recently and has come with many...'
  Title: 'What's next for System.Text.Json?'
  Blog: {Id: 1}
Post {Id: 1} Unchanged
  Id: 1 PK
  BlogId: 1 FK
  Content: 'Announcing the release of EF Core 5.0, a full featured cross...'
  Title: 'Announcing the Release of EF Core 5.0'
  Blog: {Id: 1}
Post {Id: 2} Unchanged
  Id: 2 PK
  BlogId: 1 FK
  Content: 'F# 5 is the latest version of F#, the functional programming...'
  Title: 'Announcing F# 5'
  Blog: {Id: 1}
```

Теперь блог правильно помечен как `Modified` , и обнаружена новая запись, которая будет записана как `Added` .

В начале этого раздела мы упоминали, что при _использовании EF Core для внесения_ изменений необходимо обнаружить изменения. Это происходит в приведенном выше коде. То есть изменения свойства и навигации вносятся _непосредственно в экземпляры сущности_, а не с помощью каких-либо методов EF Core.

Сравните это с приведенным ниже кодом, который изменяет сущности таким же образом, но на этот раз с помощью EF Core методов:

<!--
        using var context = new BlogsContext();
        var blog = context.Blogs.Include(e => e.Posts).First(e => e.Name == ".NET Blog");

        // Change a property value
        context.Entry(blog).Property(e => e.Name).CurrentValue = ".NET Blog (Updated!)";

        // Add a new entity to the DbContext
        context.Add(
            new Post
            {
                Blog = blog,
                Title = "What’s next for System.Text.Json?",
                Content = ".NET 5.0 was released recently and has come with many..."
            });

        Console.WriteLine(context.ChangeTracker.DebugView.LongView);
-->
[!code-csharp[Snapshot_change_tracking_2](../../../samples/core/ChangeTracking/ChangeDetectionAndNotifications/SnapshotSamples.cs?name=Snapshot_change_tracking_2)]

В этом случае представление отладка изменений Tracker показывает, что все состояния сущностей и изменения свойств известны, даже если обнаружение изменений не произошло. Это обусловлено тем <xref:Microsoft.EntityFrameworkCore.ChangeTracking.PropertyEntry.CurrentValue?displayProperty=nameWithType> , что является методом EF Core, что означает, что EF Core немедленно знает об изменениях, внесенных этим методом. Аналогичным образом, вызов <xref:Microsoft.EntityFrameworkCore.DbContext.Add%2A?displayProperty=nameWithType> позволяет EF Core немедленно узнать о новой сущности и отвести ее соответствующим образом.

> [!TIP]
> Не пытайтесь обнаружить изменения, всегда используя методы EF Core для внесения изменений в сущности. Это часто бывает более громоздким и менее хорошо, чем внесение изменений в сущности обычным способом. Цель этого документа — сообщить о том, когда нужно обнаружить изменения, а когда — нет. Намерение не порекомендую избегать обнаружения изменений.

### <a name="methods-that-automatically-detect-changes"></a>Методы, которые автоматически обнаруживают изменения

<xref:Microsoft.EntityFrameworkCore.ChangeTracking.ChangeTracker.DetectChanges> вызывается автоматически методами, где это может повлиять на результаты. Этими методами являются:

- <xref:Microsoft.EntityFrameworkCore.DbContext.SaveChanges%2A?displayProperty=nameWithType> и <xref:Microsoft.EntityFrameworkCore.DbContext.SaveChangesAsync%2A?displayProperty=nameWithType> , чтобы убедиться, что все изменения обнаружены перед обновлением базы данных.
- <xref:Microsoft.EntityFrameworkCore.ChangeTracking.ChangeTracker.Entries?displayProperty=nameWithType> и <xref:Microsoft.EntityFrameworkCore.ChangeTracking.ChangeTracker.Entries%60%601?displayProperty=nameWithType> , чтобы обеспечить актуальность состояний сущностей и измененных свойств.
- <xref:Microsoft.EntityFrameworkCore.ChangeTracking.ChangeTracker.HasChanges?displayProperty=nameWithType>, чтобы убедиться, что результат является точным.
- <xref:Microsoft.EntityFrameworkCore.ChangeTracking.ChangeTracker.CascadeChanges?displayProperty=nameWithType>, чтобы обеспечить правильное состояние сущности для сущностей Principal/Parent перед каскадным.
- <xref:Microsoft.EntityFrameworkCore.DbSet%601.Local?displayProperty=nameWithType>, чтобы обеспечить актуальность отслеживающего графа.

Кроме того, в некоторых случаях обнаружение изменений происходит только в одном экземпляре сущности, а не на графе отслеживаний сущностей. Это следующие места:

- При использовании <xref:System.Data.Entity.DbContext.Entry%2A?displayProperty=nameWithType> для обеспечения актуальности состояния и измененных свойств сущности.
- При использовании <xref:Microsoft.EntityFrameworkCore.ChangeTracking.EntityEntry> таких методов `Property` , как `Collection` , `Reference` или, `Member` чтобы обеспечить актуальность изменений свойств, текущих значений и т. д.
- Если зависимая или Дочерняя сущность будет удалена из-за того, что необходимая связь была разорвана. Это обнаруживает, что сущность не должна удаляться, так как она была повторно дочерней.

Локальное обнаружение изменений для одной сущности может быть инициировано явным образом путем вызова <xref:Microsoft.EntityFrameworkCore.ChangeTracking.EntityEntry.DetectChanges?displayProperty=nameWithType> .

> [!NOTE]
> Локальное обнаружение изменений может пропускать некоторые изменения, обнаруженные в результате полного обнаружения. Это происходит, когда каскадные действия, являющиеся результатом необнаруженных изменений в других сущностях, оказывают влияние на рассматриваемую сущность. В таких ситуациях приложению может потребоваться принудительно выполнить полную проверку всех сущностей путем явного вызова <xref:Microsoft.EntityFrameworkCore.ChangeTracking.ChangeTracker.DetectChanges?displayProperty=nameWithType> .

### <a name="disabling-automatic-change-detection"></a>Отключение автоматического обнаружения изменений

Производительность обнаружения изменений не является узким местом для большинства приложений. Однако обнаружение изменений может стать проблемой с производительностью для некоторых приложений, которые отслеживают тысячи сущностей. (Точное число зависит от многих вещей, например от количества свойств в сущности.) По этой причине автоматическое обнаружение изменений можно отключить с помощью <xref:Microsoft.EntityFrameworkCore.ChangeTracking.ChangeTracker.AutoDetectChangesEnabled?displayProperty=nameWithType> . Например, рассмотрим обработку сущностей JOIN в связи «многие ко многим» с полезной нагрузкой.

<!--
        public override int SaveChanges()
        {
            foreach (var entityEntry in ChangeTracker.Entries<PostTag>()) // Detects changes automatically
            {
                if (entityEntry.State == EntityState.Added)
                {
                    entityEntry.Entity.TaggedBy = "ajcvickers";
                    entityEntry.Entity.TaggedOn = DateTime.Now;
                }
            }

            try
            {
                ChangeTracker.AutoDetectChangesEnabled = false;
                return base.SaveChanges(); // Avoid automatically detecting changes again here
            }
            finally
            {
                ChangeTracker.AutoDetectChangesEnabled = true;
            }
        }
-->
[!code-csharp[SaveChanges](../../../samples/core/ChangeTracking/ChangeDetectionAndNotifications/SnapshotSamples.cs?name=SaveChanges)]

Как мы узнали из предыдущего раздела, <xref:Microsoft.EntityFrameworkCore.ChangeTracking.ChangeTracker.Entries%60%601?displayProperty=nameWithType> и <xref:Microsoft.EntityFrameworkCore.DbContext.SaveChanges%2A?displayProperty=nameWithType> автоматически обнаруживают изменения. Однако после вызова записей код не вносит изменения в состояние сущности или свойства. (Установка обычных значений свойств для добавленных сущностей не приводит к изменениям состояния.) Таким образом, код отключает ненужное автоматическое обнаружение изменений при вызове базового метода SaveChanges. Код также использует блок try/finally, чтобы гарантировать восстановление значения по умолчанию, даже если команда SaveChanges завершается неудачно.

> [!TIP]
> Не следует рассчитывать на то, что код должен отключать автоматическое обнаружение изменений для выполнения. Это необходимо только в том случае, если профилирование приложения с отслеживанием многих сущностей указывает на то, что производительность обнаружения изменений является проблемой.

### <a name="detecting-changes-and-value-conversions"></a>Обнаружение изменений и преобразований значений

Чтобы использовать отслеживание изменений моментальных снимков с типом сущности, EF Core должен иметь возможность:

- Создание моментального снимка каждого значения свойства при отслеживании сущности
- Сравните это значение с текущим значением свойства
- Создание хэш-кода для значения

Это автоматически обрабатывается EF Core для типов, которые могут быть непосредственно сопоставлены с базой данных. Однако если [для отображения свойства используется преобразователь значений](xref:core/modeling/value-conversions), то этот преобразователь должен указать, как выполнять эти действия. Это достигается с помощью компаратора значений, и подробно описывается в документации по [компараторам значений](xref:core/modeling/value-comparers) .

## <a name="notification-entities"></a>Сущности уведомления

Для большинства приложений рекомендуется отслеживание изменений моментальных снимков. Однако приложения, которые отправляют много сущностей и (или) делают множество изменений в этих сущностях, могут использовать преимущества реализации сущностей, которые автоматически уведомляют EF Core при изменении их свойств и значений навигации. Они называются "сущностями уведомлений".

### <a name="implementing-notification-entities"></a>Реализация сущностей уведомления

В сущностях уведомления используются <xref:System.ComponentModel.INotifyPropertyChanging> интерфейсы и <xref:System.ComponentModel.INotifyPropertyChanged> , которые являются частью библиотеки базовых классов (BCL) .NET. Эти интерфейсы определяют события, которые должны срабатывать до и после изменения значения свойства. Например:

<!--
    public class Blog : INotifyPropertyChanging, INotifyPropertyChanged
    {
        public event PropertyChangingEventHandler PropertyChanging;
        public event PropertyChangedEventHandler PropertyChanged;

        private int _id;
        public int Id
        {
            get => _id;
            set
            {
                PropertyChanging?.Invoke(this, new PropertyChangingEventArgs(nameof(Id)));
                _id = value;
                PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(nameof(Id)));
            }
        }

        private string _name;
        public string Name
        {
            get => _name;
            set
            {
                PropertyChanging?.Invoke(this, new PropertyChangingEventArgs(nameof(Name)));
                _name = value;
                PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(nameof(Name)));
            }
        }

        public IList<Post> Posts { get; } = new ObservableCollection<Post>();
    }
-->
[!code-csharp[Model](../../../samples/core/ChangeTracking/ChangeDetectionAndNotifications/NotificationEntitiesSamples.cs?name=Model)]

Кроме того, все переходы по коллекциям должны реализовываться `INotifyCollectionChanged` ; в приведенном выше примере это выполнено с помощью <xref:System.Collections.ObjectModel.ObservableCollection%601> набора записей. EF Core также поставляется с <xref:Microsoft.EntityFrameworkCore.ChangeTracking.ObservableHashSet%601> реализацией, которая имеет более эффективные Просмотры за счет стабильного упорядочения.

Большая часть этого кода уведомления обычно перемещается в Несопоставленный базовый класс. Например:

<!--
    public class Blog : NotifyingEntity
    {
        private int _id;
        public int Id
        {
            get => _id;
            set => SetWithNotify(value, out _id);
        }

        private string _name;
        public string Name
        {
            get => _name;
            set => SetWithNotify(value, out _name);
        }

        public IList<Post> Posts { get; } = new ObservableCollection<Post>();
    }

    public abstract class NotifyingEntity : INotifyPropertyChanging, INotifyPropertyChanged
    {
        protected void SetWithNotify<T>(T value, out T field, [CallerMemberName] string propertyName = "")
        {
            NotifyChanging(propertyName);
            field = value;
            NotifyChanged(propertyName);
        }

        public event PropertyChangingEventHandler PropertyChanging;
        public event PropertyChangedEventHandler PropertyChanged;

        private void NotifyChanged(string propertyName)
            => PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));

        private void NotifyChanging(string propertyName)
            => PropertyChanging?.Invoke(this, new PropertyChangingEventArgs(propertyName));
    }
-->
[!code-csharp[Model](../../../samples/core/ChangeTracking/ChangeDetectionAndNotifications/NotificationWithBaseSamples.cs?name=Model)]

### <a name="configuring-notification-entities"></a>Настройка сущностей уведомления

Не существует способа EF Core проверки того, что `INotifyPropertyChanging` или `INotifyPropertyChanged` полностью реализовано для использования с EF Core. В частности, некоторые способы использования этих интерфейсов выполняют с уведомлениями только определенные свойства, а не все свойства (включая навигацию) в соответствии с требованиями EF Core. По этой причине EF Core не выполняет автоматическое подключение к этим событиям.

Вместо этого EF Core должны быть настроены для использования этих сущностей уведомлений. Обычно это делается для всех типов сущностей путем вызова <xref:Microsoft.EntityFrameworkCore.ModelBuilder.HasChangeTrackingStrategy%2A?displayProperty=nameWithType> . Например:

<!--
        protected override void OnModelCreating(ModelBuilder modelBuilder)
        {
            modelBuilder.HasChangeTrackingStrategy(ChangeTrackingStrategy.ChangingAndChangedNotifications);
        }
-->
[!code-csharp[OnModelCreating](../../../samples/core/ChangeTracking/ChangeDetectionAndNotifications/NotificationWithBaseSamples.cs?name=OnModelCreating)]

(Стратегия также может быть задана по-разному для разных типов сущностей с помощью <xref:Microsoft.EntityFrameworkCore.Metadata.Builders.EntityTypeBuilder.HasChangeTrackingStrategy%2A?displayProperty=nameWithType> , но обычно это понизит производительность, так как DetectChanges все еще требуется для тех типов, которые не являются сущностями уведомлений.)

Для отслеживания изменений полного уведомления необходимо, `INotifyPropertyChanging` чтобы `INotifyPropertyChanged` были реализованы и, и. Это позволяет сохранять исходные значения непосредственно перед изменением значения свойства, избегая необходимости EF Core создавать моментальные снимки при отслеживании сущности. Типы сущностей, реализующие только реализацию, `INotifyPropertyChanged` можно также использовать с EF Core. В этом случае EF по-прежнему создает моментальный снимок при отслеживании сущности для отслеживания исходных значений, а затем использует уведомления для немедленного обнаружения изменений, а не требует вызова DetectChanges.

В <xref:Microsoft.EntityFrameworkCore.ChangeTrackingStrategy> следующей таблице приведены различные значения.

| чанжетраккингстратеги                              | Необходимые интерфейсы                                      | Требуется DetectChanges | Исходные значения моментальных снимков
|:----------------------------------------------------|--------------------------------------------------------|---------------------|--------------------------
| Моментальный снимок                                            | Нет                                                   | Да                 | Да
| чанжеднотификатионс                                | INotifyPropertyChanged                                 | Нет                  | Да
| чангингандчанжеднотификатионс                     | INotifyPropertyChanged и Инотифипропертичангинг     | Нет                  | Нет
| чангингандчанжеднотификатионсвисоригиналвалуес   | INotifyPropertyChanged и Инотифипропертичангинг     | Нет                  | Да

### <a name="using-notification-entities"></a>Использование сущностей уведомления

Сущности уведомлений ведут себя так же, как и любые другие сущности, за исключением того, что внесение изменений в экземпляры сущности не требует вызова <xref:Microsoft.EntityFrameworkCore.ChangeTracking.ChangeTracker.DetectChanges?displayProperty=nameWithType> для обнаружения этих изменений. Например:

<!--
            using var context = new BlogsContext();
            var blog = context.Blogs.Include(e => e.Posts).First(e => e.Name == ".NET Blog");

            // Change a property value
            blog.Name = ".NET Blog (Updated!)";

            // Add a new entity to a navigation
            blog.Posts.Add(new Post
            {
                Title = "What’s next for System.Text.Json?",
                Content = ".NET 5.0 was released recently and has come with many..."
            });

            Console.WriteLine(context.ChangeTracker.DebugView.LongView);
-->
[!code-csharp[Notification_entities_2](../../../samples/core/ChangeTracking/ChangeDetectionAndNotifications/NotificationWithBaseSamples.cs?name=Notification_entities_2)]

При работе с обычными сущностями [представление "Отладка" средства записи изменений](xref:core/change-tracking/debug-views) показало, что эти изменения не были обнаружены до вызова DetectChanges. Просмотр представления Отладка при использовании сущностей уведомления показывает, что эти изменения были обнаружены немедленно:

```output
Blog {Id: 1} Modified
  Id: 1 PK
  Name: '.NET Blog (Updated!)' Modified
  Posts: [{Id: 1}, {Id: 2}, {Id: -2147482643}]
Post {Id: -2147482643} Added
  Id: -2147482643 PK Temporary
  BlogId: 1 FK
  Content: '.NET 5.0 was released recently and has come with many...'
  Title: 'What's next for System.Text.Json?'
  Blog: {Id: 1}
Post {Id: 1} Unchanged
  Id: 1 PK
  BlogId: 1 FK
  Content: 'Announcing the release of EF Core 5.0, a full featured cross...'
  Title: 'Announcing the Release of EF Core 5.0'
  Blog: {Id: 1}
Post {Id: 2} Unchanged
  Id: 2 PK
  BlogId: 1 FK
  Content: 'F# 5 is the latest version of F#, the functional programming...'
  Title: 'Announcing F# 5'
  Blog: {Id: 1}
```

## <a name="change-tracking-proxies"></a>Прокси-серверы отслеживания изменений

> [!NOTE]
> Прокси-серверы отслеживания изменений были введены в EF Core 5,0.

EF Core могут динамически создавать типы прокси, реализующие <xref:System.ComponentModel.INotifyPropertyChanging> и <xref:System.ComponentModel.INotifyPropertyChanged> . Для этого необходимо установить пакет NuGet [Microsoft. EntityFrameworkCore. прокси](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Proxies/) и включить учетные записи-посредники с отслеживанием изменений, <xref:Microsoft.EntityFrameworkCore.ProxiesExtensions.UseChangeTrackingProxies%2A> например:

<!--
        protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
            => optionsBuilder.UseChangeTrackingProxies();
-->
[!code-csharp[OnConfiguring](../../../samples/core/ChangeTracking/ChangeDetectionAndNotifications/ChangeTrackingProxiesSamples.cs?name=OnConfiguring)]

Создание динамического прокси-сервера включает создание нового динамического типа .NET (с использованием реализации прокси [Castle. Core](https://www.nuget.org/packages/Castle.Core/) ), который наследует от типа сущности, а затем переопределяет все методы задания свойств. Поэтому типы сущностей для прокси-серверов должны быть типами, которые могут быть унаследованы от и должны иметь свойства, которые могут быть переопределены. Кроме того, структуры навигации по коллекциям, созданные явным образом, должны реализовывать, <xref:System.Collections.Specialized.INotifyCollectionChanged> например:

<!--
    public class Blog
    {
        public virtual int Id { get; set; }
        public virtual string Name { get; set; }

        public virtual IList<Post> Posts { get; } = new ObservableCollection<Post>();
    }

    public class Post
    {
        public virtual int Id { get; set; }
        public virtual string Title { get; set; }
        public virtual string Content { get; set; }

        public virtual int BlogId { get; set; }
        public virtual Blog Blog { get; set; }
    }
-->
[!code-csharp[Model](../../../samples/core/ChangeTracking/ChangeDetectionAndNotifications/ChangeTrackingProxiesSamples.cs?name=Model)]

Одним из значащих недостатков для прокси-серверов отслеживания изменений является то, что EF Core должны всегда отслеживать экземпляры прокси-сервера, никогда не экземпляры базового типа сущности. Это происходит потому, что экземпляры базового типа сущности не будут создавать уведомления, что означает, что изменения, внесенные в эти сущности, будут пропущены.

EF Core создает экземпляры прокси-сервера автоматически при запросе к базе данных, поэтому эта негативная сторона обычно ограничена отслеживанием новых экземпляров сущностей. Эти экземпляры должны быть созданы с помощью <xref:Microsoft.EntityFrameworkCore.ProxiesExtensions.CreateProxy%2A> методов расширения, а **не** обычным способом использования `new` . Это означает, что теперь код из предыдущих примеров должен использовать `CreateProxy` :

<!--
            using var context = new BlogsContext();
            var blog = context.Blogs.Include(e => e.Posts).First(e => e.Name == ".NET Blog");

            // Change a property value
            blog.Name = ".NET Blog (Updated!)";

            // Add a new entity to a navigation
            blog.Posts.Add(
                context.CreateProxy<Post>(
                    p =>
                        {
                            p.Title = "What’s next for System.Text.Json?";
                            p.Content = ".NET 5.0 was released recently and has come with many...";
                        }));

            Console.WriteLine(context.ChangeTracker.DebugView.LongView);
-->
[!code-csharp[Change_tracking_proxies_1](../../../samples/core/ChangeTracking/ChangeDetectionAndNotifications/ChangeTrackingProxiesSamples.cs?name=Change_tracking_proxies_1)]

## <a name="change-tracking-events"></a>События отслеживания изменений

EF Core запускает <xref:Microsoft.EntityFrameworkCore.ChangeTracking.ChangeTracker.Tracked?displayProperty=nameWithType> событие при первой отслеживании сущности. Будущие изменения состояния сущности приводят к <xref:Microsoft.EntityFrameworkCore.ChangeTracking.ChangeTracker.StateChanged?displayProperty=nameWithType> событиям. Дополнительные сведения см. в статье о [событиях .NET в EF Core](xref:core/logging-events-diagnostics/events).

> [!NOTE]
> `StateChanged`Событие не запускается при первой отслеживании сущности, несмотря на то, что состояние изменилось с `Detached` на одно из других состояний. Убедитесь, что прослушиваете `StateChanged` события и, `Tracked` чтобы получить все соответствующие уведомления.
