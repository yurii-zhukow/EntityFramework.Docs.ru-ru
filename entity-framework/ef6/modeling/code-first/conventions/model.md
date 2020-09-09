---
title: Соглашения на основе модели — EF6
description: Соглашения на основе моделей в Entity Framework 6
author: divega
ms.date: 10/23/2016
ms.assetid: 0fc4eef8-29b8-4192-9c77-08fd33d3db3a
uid: ef6/modeling/code-first/conventions/model
ms.openlocfilehash: 30a79f505939220b3d4040778397eab972e6a712
ms.sourcegitcommit: 7c3939504bb9da3f46bea3443638b808c04227c2
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/09/2020
ms.locfileid: "89617189"
---
# <a name="model-based-conventions"></a>Соглашения на основе модели
> [!NOTE]
> **Только в EF6 и более поздних версиях**. Функции, API и другие возможности, описанные на этой странице, появились в Entity Framework 6. При использовании более ранней версии могут быть неприменимы некоторые или все сведения.  

Соглашения на основе модели являются расширенным методом настройки модели на основе соглашений. В большинстве сценариев следует использовать [Пользовательский API соглашений Code First для дбмоделбуилдер](xref:ef6/modeling/code-first/conventions/custom) . Перед использованием соглашений на основе модели рекомендуется ознакомиться с Дбмоделбуилдер API для соглашений.  

Соглашения на основе модели позволяют создавать соглашения, влияющие на свойства и таблицы, которые не настраиваются с помощью стандартных соглашений. Примерами являются столбцы дискриминатора в таблицах на модели иерархии и независимые столбцы взаимосвязей.  

## <a name="creating-a-convention"></a>Создание соглашения   

Первым шагом в создании соглашения на основе модели является выбор того, когда в конвейере необходимо применить соглашение к модели. Существует два типа соглашений о модели: концептуальные (C-Space) и Store (S-Space). К модели, создаваемой приложением, применяется соглашение о пробелах, а в версии модели, представляющей базу данных, применяется соглашение об использовании пространства, а также элементы управления, например способ именования автоматически созданных столбцов.  

Соглашение модели — это класс, который расширяется из Иконцептуалмоделконвентион или Исторемоделконвентион.  Оба этих интерфейса принимают универсальный тип, который может иметь тип Метадатаитем, который используется для фильтрации типа данных, к которому применяется соглашение.  

## <a name="adding-a-convention"></a>Добавление соглашения   

Правила модели добавляются так же, как и обычные классы соглашений. В методе **OnModelCreating** добавьте соглашение в список соглашений для модели.  

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

Соглашение также можно добавить по отношению к другому соглашению с помощью методов Conventions. Аддбефоре \<\> или соглашений. аддафтер \<\> . Дополнительные сведения о соглашениях, которые Entity Framework применяются, см. в разделе "Примечания".  

``` csharp
protected override void OnModelCreating(DbModelBuilder modelBuilder)
{
    modelBuilder.Conventions.AddAfter<IdKeyDiscoveryConvention>(new MyModelBasedConvention());
}
```  

## <a name="example-discriminator-model-convention"></a>Пример: соглашение о модели дискриминатора  

Переименование столбцов, созданных EF, является примером чего-то, что нельзя сделать с помощью других API-интерфейсов соглашений.  Это ситуация, когда единственным вариантом является использование соглашений о модели.  

Пример использования соглашения на основе модели для настройки созданных столбцов заключается в настройке способа именования столбцов дискриминатора.  Ниже приведен пример простого соглашения на основе модели, которое переименовывает каждый столбец в модели с именем "дискриминатор" в "EntityType".  Сюда входят столбцы, которые разработчик просто назвал «дискриминатор». Так как столбец "дискриминатор" является сгенерированным столбцом, он должен выполняться в S-Space.  

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

## <a name="example-general-ia-renaming-convention"></a>Пример. Общее соглашение о переименовании IA   

Еще один более сложный пример соглашений на основе модели в действии состоит в настройке способа именования независимых ассоциаций (IAs).  Это ситуация, когда применяются соглашения модели, так как службы IAs создаются EF и отсутствуют в модели, к которой может получить доступ API Дбмоделбуилдер.  

Когда EF создает IA, он создает столбец с именем EntityType_KeyName. Например, для ассоциации с именем Customer с ключевым столбцом CustomerId будет создан столбец с именем Customer_CustomerId. Следующее соглашение отделяет \_ символ "" от имени столбца, созданного для IA.  

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
        for (int i = 0; i < properties.Count; ++i)
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

## <a name="extending-existing-conventions"></a>Расширение существующих соглашений   

Если необходимо написать соглашение, похожее на одно из соглашений, которые Entity Framework уже применяются к модели, всегда можно расширить это соглашение, чтобы избежать необходимости переписывать его с нуля.  Примером этого является замена существующего соглашения о сопоставлении ID на другое.   Дополнительным преимуществом переопределения соглашения о ключах является то, что переопределенный метод будет вызываться только в том случае, если ключ уже не определен или явно не настроен. Список соглашений, используемых Entity Framework, доступен здесь: [http://msdn.microsoft.com/library/system.data.entity.modelconfiguration.conventions.aspx](https://msdn.microsoft.com/library/system.data.entity.modelconfiguration.conventions.aspx) .  

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

Затем необходимо добавить наше новое соглашение перед существующим соглашением о ключе. После добавления Кустомкэйдисковериконвентион можно удалить Идкэйдисковериконвентион.  Если мы не удалим существующие Идкэйдисковериконвентион, это соглашение будет иметь приоритет над соглашением об обнаружении идентификатора, так как оно выполняется первым, но в случае, когда не найдено свойство "Key", будет выполняться соглашение "ID".  Мы видим такое поведение, поскольку каждое соглашение видит модель в соответствии с предыдущим соглашением (вместо того, чтобы использовать его независимо друг от друга и объединяется вместе), поэтому, если, например, предыдущее соглашение обновило имя столбца в соответствии с интересующим вас соглашением (когда до того, как это имя не является интересным), оно будет применено к этому столбцу.  

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

Список соглашений, которые в настоящее время применяются Entity Framework, доступен в документации MSDN здесь: [http://msdn.microsoft.com/library/system.data.entity.modelconfiguration.conventions.aspx](https://msdn.microsoft.com/library/system.data.entity.modelconfiguration.conventions.aspx) .  Этот список извлекается непосредственно из нашего исходного кода.  Исходный код для Entity Framework 6 доступен в [GitHub](https://github.com/aspnet/entityframework6/) , и многие из соглашений, используемых Entity Framework, являются хорошими отправными точками для настраиваемых соглашений на основе модели.  
