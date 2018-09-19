---
title: Модели с использованием соглашений - EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 0fc4eef8-29b8-4192-9c77-08fd33d3db3a
ms.openlocfilehash: 80b722730b4ca6c9d00a8611b6c9027e8bc9fe61
ms.sourcegitcommit: 269c8a1a457a9ad27b4026c22c4b1a76991fb360
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/18/2018
ms.locfileid: "46283711"
---
# <a name="model-based-conventions"></a>Правила на основе моделей
> [!NOTE]
> **Только в EF6 и более поздних версиях**. Функции, API и другие возможности, описанные на этой странице, появились в Entity Framework 6. При использовании более ранней версии могут быть неприменимы некоторые или все сведения.  

Соглашения для модели на основе являются расширенный способ настройки модели на основе соглашения. В большинстве случаев [Custom соглашение об API для Code First на DbModelBuilder](~/ef6/modeling/code-first/conventions/custom.md) следует использовать. Понимание соглашения об API-интерфейса DbModelBuilder рекомендуется перед использованием соглашений для модели на основе.  

Соглашения для модели на основе разрешение на создание соглашений, которые влияют на свойства и таблиц, которые не могут быть изменены через стандартные соглашения. Примеры этих являются столбцами дискриминатора в одна таблица на иерархию модели и столбцами независимом сопоставлении.  

## <a name="creating-a-convention"></a>Создание соглашения   

Первым шагом создания модели на основе соглашения, выбирая при в конвейере соглашению необходимо применить к модели. Существует два типа соглашения для модели, концептуальная (C-Space) и Store (S-Space). C-Space соглашение применяется к модели, сборка приложения, тогда как S-Space соглашение применяется к версии модели, которая представляет базу данных и элементы управления, такими как автоматически созданные столбцы имеют имена.  

Соглашение для модели — это класс, расширяющий IConceptualModelConvention или IStoreModelConvention.  Эти интерфейсы, принимающие универсальный тип, который может иметь тип MetadataItem, который используется для фильтрации типа данных, к которому применяется соглашение.  

## <a name="adding-a-convention"></a>Добавление соглашения   

Соглашения для модели, добавляются в так же, как классы регулярных соглашения. В **OnModelCreating** метод, добавить соглашение в список соглашений для модели.  

``` csharp
using System.Data.Entity;
using System.Data.Entity.Core.Metadata.Edm;
using System.Data.Entity.Infrastructure;
using System.Data.Entity.ModelConfiguration.Conventions;

public class BlogContext : DbContext  
{  
    public DbSet<Post> Posts { get; set; }  
    public DbSet<Comment> Comments { get; set; }  

    protected override void OnModelCreating(DbModelBuilder modelBuilder)  
    {  
        modelBuilder.Conventions.Add<MyModelBasedConvention>();  
    }  
}
```  

Также можно добавить соглашение по отношению к другим соглашением, с помощью Conventions.AddBefore\< \> или Conventions.AddAfter\< \> методы. Дополнительные сведения о правилах, к которым применяется Entity Framework см. в разделе "Примечания".  

``` csharp
protected override void OnModelCreating(DbModelBuilder modelBuilder)
{
    modelBuilder.Conventions.AddAfter<IdKeyDiscoveryConvention>(new MyModelBasedConvention());
}
```  

## <a name="example-discriminator-model-convention"></a>Пример: Соглашение для модели дискриминатора  

Переименование столбцов, созданных EF является примером того, что невозможно выполнить с помощью другие соглашения об API-интерфейсы.  Это ситуация где с помощью соглашения для модели — это единственный вариант.  

Пример того, как использовать соглашение на основе модели для настройки создаваемых столбцов рекомендуется именовать способ названы дискриминатора.  Ниже приведен пример простой модели на основе соглашения, которое изменяет имя каждого столбца в модели с именем «Дискриминатора» на «EntityType».  Сюда входят столбцы, что разработчик просто с именем «Дискриминатора». Так как столбец «Дискриминатора» — это созданный столбец это нужно выполнить в пространстве S.  

``` csharp
using System.Data.Entity;
using System.Data.Entity.Core.Metadata.Edm;
using System.Data.Entity.Infrastructure;
using System.Data.Entity.ModelConfiguration.Conventions;

class DiscriminatorRenamingConvention : IStoreModelConvention<EdmProperty>  
{  
    public void Apply(EdmProperty property, DbModel model)  
    {            
        if (property.Name == "Discriminator")  
        {  
            property.Name = "EntityType";  
        }  
    }  
}
```  

## <a name="example-general-ia-renaming-convention"></a>Пример: Общие IA переименование соглашение   

Еще один более сложный пример модели на основе соглашений в действии — настроить так, что именуются независимых сопоставлений (IAs).  Это ситуация, где применяются из-за создания IAs в EF соглашения для модели, а не присутствуют в модели, можно получить доступ к API-интерфейса DbModelBuilder.  

Когда EF создает IA, он создает столбец с именем EntityType_KeyName. Например для связи с именем Customer с ключевым столбцом с именем CustomerId, столбец с именем Customer_CustomerId будут сформированы. Следующие ленты соглашение "\_" символа из имени столбца, который создается для IA.  

``` csharp
using System.Data.Entity;
using System.Data.Entity.Core.Metadata.Edm;
using System.Data.Entity.Infrastructure;
using System.Data.Entity.ModelConfiguration.Conventions;

// Provides a convention for fixing the independent association (IA) foreign key column names.  
public class ForeignKeyNamingConvention : IStoreModelConvention<AssociationType>
{

    public void Apply(AssociationType association, DbModel model)
    {
        // Identify ForeignKey properties (including IAs)  
        if (association.IsForeignKey)
        {
            // rename FK columns  
            var constraint = association.Constraint;
            if (DoPropertiesHaveDefaultNames(constraint.FromProperties, constraint.ToRole.Name, constraint.ToProperties))
            {
                NormalizeForeignKeyProperties(constraint.FromProperties);
            }
            if (DoPropertiesHaveDefaultNames(constraint.ToProperties, constraint.FromRole.Name, constraint.FromProperties))
            {
                NormalizeForeignKeyProperties(constraint.ToProperties);
            }
        }
    }

    private bool DoPropertiesHaveDefaultNames(ReadOnlyMetadataCollection<EdmProperty> properties, string roleName, ReadOnlyMetadataCollection<EdmProperty> otherEndProperties)
    {
        if (properties.Count != otherEndProperties.Count)
        {
            return false;
        }

        for (int i = 0; i < properties.Count; ++i)
        {
            if (!properties[i].Name.EndsWith("_" + otherEndProperties[i].Name))
            {
                return false;
            }
        }
        return true;
    }

    private void NormalizeForeignKeyProperties(ReadOnlyMetadataCollection<EdmProperty> properties)
    {
        for (int i = 0; i \< properties.Count; ++i)
        {
            int underscoreIndex = properties[i].Name.IndexOf('_');
            if (underscoreIndex > 0)
            {
                properties[i].Name = properties[i].Name.Remove(underscoreIndex, 1);
            }                 
        }
    }
}
```  

## <a name="extending-existing-conventions"></a>Расширение существующего соглашения   

Если вам нужно написать соглашением, похожее на одно из соглашений, которые Entity Framework уже применяется к модели всегда можно расширить, соглашение, чтобы избежать необходимости переписывать его заново.  Примером этого является замените существующие сопоставления соглашение о на новый идентификатор.   Дополнительное преимущество для переопределения в соответствии с соглашением ключа — что переопределенный метод будет вызван только в том случае, если отсутствует ключ уже определено или задано явно. Список соглашений, используемых Entity Framework можно найти здесь: [ http://msdn.microsoft.com/library/system.data.entity.modelconfiguration.conventions.aspx ](https://msdn.microsoft.com/library/system.data.entity.modelconfiguration.conventions.aspx).  

``` csharp
using System.Data.Entity;
using System.Data.Entity.Core.Metadata.Edm;
using System.Data.Entity.Infrastructure;
using System.Data.Entity.ModelConfiguration.Conventions;
using System.Linq;  

// Convention to detect primary key properties.
// Recognized naming patterns in order of precedence are:
// 1. 'Key'
// 2. [type name]Key
// Primary key detection is case insensitive.
public class CustomKeyDiscoveryConvention : KeyDiscoveryConvention
{
    private const string Id = "Key";

    protected override IEnumerable<EdmProperty> MatchKeyProperty(
        EntityType entityType, IEnumerable<EdmProperty> primitiveProperties)
    {
        Debug.Assert(entityType != null);
        Debug.Assert(primitiveProperties != null);

        var matches = primitiveProperties
            .Where(p => Id.Equals(p.Name, StringComparison.OrdinalIgnoreCase));

        if (!matches.Any())
       {
            matches = primitiveProperties
                .Where(p => (entityType.Name + Id).Equals(p.Name, StringComparison.OrdinalIgnoreCase));
        }

        // If the number of matches is more than one, then multiple properties matched differing only by
        // case--for example, "Key" and "key".  
        if (matches.Count() > 1)
        {
            throw new InvalidOperationException("Multiple properties match the key convention");
        }

        return matches;
    }
}
```  

Затем необходимо добавить наш новое соглашение перед Конвенции существующего ключа. После того как мы добавлен CustomKeyDiscoveryConvention, мы можем удалить IdKeyDiscoveryConvention.  Если мы не удалим существующий IdKeyDiscoveryConvention, это соглашение будет по-прежнему имеют приоритет над конвенции идентификатор обнаружения, так как он выполняется, во-первых, но в случае, когда свойство «key» не найдено, будет выполняться в соответствии с соглашением «id».  Мы видим это поведение, поскольку каждый соглашение видит модели, при обновлении с предыдущей соглашением (а не на управлении его независимо друг от друга и все объединяемых друг с другом), чтобы если, например, предыдущий соглашение обновлены так, то имя столбца могут представлять интерес для вашего настраиваемого соглашения (если до этого имени не интерес), то оно будет применяться к этому столбцу.  

``` csharp
public class BlogContext : DbContext
{
    public DbSet<Post> Posts { get; set; }
    public DbSet<Comment> Comments { get; set; }

    protected override void OnModelCreating(DbModelBuilder modelBuilder)
    {
        modelBuilder.Conventions.AddBefore<IdKeyDiscoveryConvention>(new CustomKeyDiscoveryConvention());
        modelBuilder.Conventions.Remove<IdKeyDiscoveryConvention>();
    }
}
```  

## <a name="notes"></a>Примечания  

Список соглашений, применяемых в настоящее время платформа Entity Framework можно найти в документации MSDN: [ http://msdn.microsoft.com/library/system.data.entity.modelconfiguration.conventions.aspx ](https://msdn.microsoft.com/library/system.data.entity.modelconfiguration.conventions.aspx).  Этот список извлекаются непосредственно из исходного кода.  Исходный код для платформы Entity Framework 6 можно найти в [GitHub](https://github.com/aspnet/entityframework6/) и хорошо подходят многие из соглашения, используемые платформой Entity Framework отправных точек для пользовательской модели на основе соглашений.  
