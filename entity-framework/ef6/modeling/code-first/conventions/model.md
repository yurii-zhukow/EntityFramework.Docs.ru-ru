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
# <a name="model-based-conventions"></a><span data-ttu-id="1161c-102">Правила на основе моделей</span><span class="sxs-lookup"><span data-stu-id="1161c-102">Model-Based Conventions</span></span>
> [!NOTE]
> <span data-ttu-id="1161c-103">**Только в EF6 и более поздних версиях**. Функции, API и другие возможности, описанные на этой странице, появились в Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="1161c-103">**EF6 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 6.</span></span> <span data-ttu-id="1161c-104">При использовании более ранней версии могут быть неприменимы некоторые или все сведения.</span><span class="sxs-lookup"><span data-stu-id="1161c-104">If you are using an earlier version, some or all of the information does not apply.</span></span>  

<span data-ttu-id="1161c-105">Соглашения для модели на основе являются расширенный способ настройки модели на основе соглашения.</span><span class="sxs-lookup"><span data-stu-id="1161c-105">Model based conventions are an advanced method of convention based model configuration.</span></span> <span data-ttu-id="1161c-106">В большинстве случаев [Custom соглашение об API для Code First на DbModelBuilder](~/ef6/modeling/code-first/conventions/custom.md) следует использовать.</span><span class="sxs-lookup"><span data-stu-id="1161c-106">For most scenarios the [Custom Code First Convention API on DbModelBuilder](~/ef6/modeling/code-first/conventions/custom.md) should be used.</span></span> <span data-ttu-id="1161c-107">Понимание соглашения об API-интерфейса DbModelBuilder рекомендуется перед использованием соглашений для модели на основе.</span><span class="sxs-lookup"><span data-stu-id="1161c-107">An understanding of the DbModelBuilder API for conventions is recommended before using model based conventions.</span></span>  

<span data-ttu-id="1161c-108">Соглашения для модели на основе разрешение на создание соглашений, которые влияют на свойства и таблиц, которые не могут быть изменены через стандартные соглашения.</span><span class="sxs-lookup"><span data-stu-id="1161c-108">Model based conventions allow the creation of conventions that affect properties and tables which are not configurable through standard conventions.</span></span> <span data-ttu-id="1161c-109">Примеры этих являются столбцами дискриминатора в одна таблица на иерархию модели и столбцами независимом сопоставлении.</span><span class="sxs-lookup"><span data-stu-id="1161c-109">Examples of these are discriminator columns in table per hierarchy models and Independent Association columns.</span></span>  

## <a name="creating-a-convention"></a><span data-ttu-id="1161c-110">Создание соглашения</span><span class="sxs-lookup"><span data-stu-id="1161c-110">Creating a Convention</span></span>   

<span data-ttu-id="1161c-111">Первым шагом создания модели на основе соглашения, выбирая при в конвейере соглашению необходимо применить к модели.</span><span class="sxs-lookup"><span data-stu-id="1161c-111">The first step in creating a model based convention is choosing when in the pipeline the convention needs to be applied to the model.</span></span> <span data-ttu-id="1161c-112">Существует два типа соглашения для модели, концептуальная (C-Space) и Store (S-Space).</span><span class="sxs-lookup"><span data-stu-id="1161c-112">There are two types of model conventions, Conceptual (C-Space) and Store (S-Space).</span></span> <span data-ttu-id="1161c-113">C-Space соглашение применяется к модели, сборка приложения, тогда как S-Space соглашение применяется к версии модели, которая представляет базу данных и элементы управления, такими как автоматически созданные столбцы имеют имена.</span><span class="sxs-lookup"><span data-stu-id="1161c-113">A C-Space convention is applied to the model that the application builds, whereas an S-Space convention is applied to the version of the model that represents the database and controls things such as how automatically-generated columns are named.</span></span>  

<span data-ttu-id="1161c-114">Соглашение для модели — это класс, расширяющий IConceptualModelConvention или IStoreModelConvention.</span><span class="sxs-lookup"><span data-stu-id="1161c-114">A model convention is a class that extends from either IConceptualModelConvention or IStoreModelConvention.</span></span>  <span data-ttu-id="1161c-115">Эти интерфейсы, принимающие универсальный тип, который может иметь тип MetadataItem, который используется для фильтрации типа данных, к которому применяется соглашение.</span><span class="sxs-lookup"><span data-stu-id="1161c-115">These interfaces both accept a generic type that can be of type MetadataItem which is used to filter the data type that the convention applies to.</span></span>  

## <a name="adding-a-convention"></a><span data-ttu-id="1161c-116">Добавление соглашения</span><span class="sxs-lookup"><span data-stu-id="1161c-116">Adding a Convention</span></span>   

<span data-ttu-id="1161c-117">Соглашения для модели, добавляются в так же, как классы регулярных соглашения.</span><span class="sxs-lookup"><span data-stu-id="1161c-117">Model conventions are added in the same way as regular conventions classes.</span></span> <span data-ttu-id="1161c-118">В **OnModelCreating** метод, добавить соглашение в список соглашений для модели.</span><span class="sxs-lookup"><span data-stu-id="1161c-118">In the **OnModelCreating** method, add the convention to the list of conventions for a model.</span></span>  

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

<span data-ttu-id="1161c-119">Также можно добавить соглашение по отношению к другим соглашением, с помощью Conventions.AddBefore\< \> или Conventions.AddAfter\< \> методы.</span><span class="sxs-lookup"><span data-stu-id="1161c-119">A convention can also be added in relation to another convention using the Conventions.AddBefore\<\> or Conventions.AddAfter\<\> methods.</span></span> <span data-ttu-id="1161c-120">Дополнительные сведения о правилах, к которым применяется Entity Framework см. в разделе "Примечания".</span><span class="sxs-lookup"><span data-stu-id="1161c-120">For more information about the conventions that Entity Framework applies see the notes section.</span></span>  

``` csharp
protected override void OnModelCreating(DbModelBuilder modelBuilder)
{
    modelBuilder.Conventions.AddAfter<IdKeyDiscoveryConvention>(new MyModelBasedConvention());
}
```  

## <a name="example-discriminator-model-convention"></a><span data-ttu-id="1161c-121">Пример: Соглашение для модели дискриминатора</span><span class="sxs-lookup"><span data-stu-id="1161c-121">Example: Discriminator Model Convention</span></span>  

<span data-ttu-id="1161c-122">Переименование столбцов, созданных EF является примером того, что невозможно выполнить с помощью другие соглашения об API-интерфейсы.</span><span class="sxs-lookup"><span data-stu-id="1161c-122">Renaming columns generated by EF is an example of something that you can’t do with the other conventions APIs.</span></span>  <span data-ttu-id="1161c-123">Это ситуация где с помощью соглашения для модели — это единственный вариант.</span><span class="sxs-lookup"><span data-stu-id="1161c-123">This is a situation where using model conventions is your only option.</span></span>  

<span data-ttu-id="1161c-124">Пример того, как использовать соглашение на основе модели для настройки создаваемых столбцов рекомендуется именовать способ названы дискриминатора.</span><span class="sxs-lookup"><span data-stu-id="1161c-124">An example of how to use a model based convention to configure generated columns is customizing the way discriminator columns are named.</span></span>  <span data-ttu-id="1161c-125">Ниже приведен пример простой модели на основе соглашения, которое изменяет имя каждого столбца в модели с именем «Дискриминатора» на «EntityType».</span><span class="sxs-lookup"><span data-stu-id="1161c-125">Below is an example of a simple model based convention that renames every column in the model named “Discriminator” to “EntityType”.</span></span>  <span data-ttu-id="1161c-126">Сюда входят столбцы, что разработчик просто с именем «Дискриминатора».</span><span class="sxs-lookup"><span data-stu-id="1161c-126">This includes columns that the developer simply named “Discriminator”.</span></span> <span data-ttu-id="1161c-127">Так как столбец «Дискриминатора» — это созданный столбец это нужно выполнить в пространстве S.</span><span class="sxs-lookup"><span data-stu-id="1161c-127">Since the “Discriminator” column is a generated column this needs to run in S-Space.</span></span>  

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

## <a name="example-general-ia-renaming-convention"></a><span data-ttu-id="1161c-128">Пример: Общие IA переименование соглашение</span><span class="sxs-lookup"><span data-stu-id="1161c-128">Example: General IA Renaming Convention</span></span>   

<span data-ttu-id="1161c-129">Еще один более сложный пример модели на основе соглашений в действии — настроить так, что именуются независимых сопоставлений (IAs).</span><span class="sxs-lookup"><span data-stu-id="1161c-129">Another more complicated example of model based conventions in action is to configure the way that Independent Associations (IAs) are named.</span></span>  <span data-ttu-id="1161c-130">Это ситуация, где применяются из-за создания IAs в EF соглашения для модели, а не присутствуют в модели, можно получить доступ к API-интерфейса DbModelBuilder.</span><span class="sxs-lookup"><span data-stu-id="1161c-130">This is a situation where Model conventions are applicable because IAs are generated by EF and aren’t present in the model that the DbModelBuilder API can access.</span></span>  

<span data-ttu-id="1161c-131">Когда EF создает IA, он создает столбец с именем EntityType_KeyName.</span><span class="sxs-lookup"><span data-stu-id="1161c-131">When EF generates an IA, it creates a column named EntityType_KeyName.</span></span> <span data-ttu-id="1161c-132">Например для связи с именем Customer с ключевым столбцом с именем CustomerId, столбец с именем Customer_CustomerId будут сформированы.</span><span class="sxs-lookup"><span data-stu-id="1161c-132">For example, for an association named Customer with a key column named CustomerId it would generate a column named Customer_CustomerId.</span></span> <span data-ttu-id="1161c-133">Следующие ленты соглашение "\_" символа из имени столбца, который создается для IA.</span><span class="sxs-lookup"><span data-stu-id="1161c-133">The following convention strips the ‘\_’ character out of the column name that is generated for the IA.</span></span>  

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

## <a name="extending-existing-conventions"></a><span data-ttu-id="1161c-134">Расширение существующего соглашения</span><span class="sxs-lookup"><span data-stu-id="1161c-134">Extending Existing Conventions</span></span>   

<span data-ttu-id="1161c-135">Если вам нужно написать соглашением, похожее на одно из соглашений, которые Entity Framework уже применяется к модели всегда можно расширить, соглашение, чтобы избежать необходимости переписывать его заново.</span><span class="sxs-lookup"><span data-stu-id="1161c-135">If you need to write a convention that is similar to one of the conventions that Entity Framework already applies to your model you can always extend that convention to avoid having to rewrite it from scratch.</span></span>  <span data-ttu-id="1161c-136">Примером этого является замените существующие сопоставления соглашение о на новый идентификатор.</span><span class="sxs-lookup"><span data-stu-id="1161c-136">An example of this is to replace the existing Id matching convention with a custom one.</span></span>   <span data-ttu-id="1161c-137">Дополнительное преимущество для переопределения в соответствии с соглашением ключа — что переопределенный метод будет вызван только в том случае, если отсутствует ключ уже определено или задано явно.</span><span class="sxs-lookup"><span data-stu-id="1161c-137">An added benefit to overriding the key convention is that the overridden method will get called only if there is no key already detected or explicitly configured.</span></span> <span data-ttu-id="1161c-138">Список соглашений, используемых Entity Framework можно найти здесь: [ http://msdn.microsoft.com/library/system.data.entity.modelconfiguration.conventions.aspx ](https://msdn.microsoft.com/library/system.data.entity.modelconfiguration.conventions.aspx).</span><span class="sxs-lookup"><span data-stu-id="1161c-138">A list of conventions that used by Entity Framework is available here: [http://msdn.microsoft.com/library/system.data.entity.modelconfiguration.conventions.aspx](https://msdn.microsoft.com/library/system.data.entity.modelconfiguration.conventions.aspx).</span></span>  

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

<span data-ttu-id="1161c-139">Затем необходимо добавить наш новое соглашение перед Конвенции существующего ключа.</span><span class="sxs-lookup"><span data-stu-id="1161c-139">We then need to add our new convention before the existing key convention.</span></span> <span data-ttu-id="1161c-140">После того как мы добавлен CustomKeyDiscoveryConvention, мы можем удалить IdKeyDiscoveryConvention.</span><span class="sxs-lookup"><span data-stu-id="1161c-140">After we add the CustomKeyDiscoveryConvention, we can remove the IdKeyDiscoveryConvention.</span></span>  <span data-ttu-id="1161c-141">Если мы не удалим существующий IdKeyDiscoveryConvention, это соглашение будет по-прежнему имеют приоритет над конвенции идентификатор обнаружения, так как он выполняется, во-первых, но в случае, когда свойство «key» не найдено, будет выполняться в соответствии с соглашением «id».</span><span class="sxs-lookup"><span data-stu-id="1161c-141">If we didn’t remove the existing IdKeyDiscoveryConvention this convention would still take precedence over the Id discovery convention since it is run first, but in the case where no “key” property is found, the “id” convention will run.</span></span>  <span data-ttu-id="1161c-142">Мы видим это поведение, поскольку каждый соглашение видит модели, при обновлении с предыдущей соглашением (а не на управлении его независимо друг от друга и все объединяемых друг с другом), чтобы если, например, предыдущий соглашение обновлены так, то имя столбца могут представлять интерес для вашего настраиваемого соглашения (если до этого имени не интерес), то оно будет применяться к этому столбцу.</span><span class="sxs-lookup"><span data-stu-id="1161c-142">We see this behavior because each convention sees the model as updated by the previous convention (rather than operating on it independently and all being combined together) so that if for example, a previous convention updated a column name to match something of interest to your custom convention (when before that the name was not of interest) then it will apply to that column.</span></span>  

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

## <a name="notes"></a><span data-ttu-id="1161c-143">Примечания</span><span class="sxs-lookup"><span data-stu-id="1161c-143">Notes</span></span>  

<span data-ttu-id="1161c-144">Список соглашений, применяемых в настоящее время платформа Entity Framework можно найти в документации MSDN: [ http://msdn.microsoft.com/library/system.data.entity.modelconfiguration.conventions.aspx ](https://msdn.microsoft.com/library/system.data.entity.modelconfiguration.conventions.aspx).</span><span class="sxs-lookup"><span data-stu-id="1161c-144">A list of conventions that are currently applied by Entity Framework is available in the MSDN documentation here: [http://msdn.microsoft.com/library/system.data.entity.modelconfiguration.conventions.aspx](https://msdn.microsoft.com/library/system.data.entity.modelconfiguration.conventions.aspx).</span></span>  <span data-ttu-id="1161c-145">Этот список извлекаются непосредственно из исходного кода.</span><span class="sxs-lookup"><span data-stu-id="1161c-145">This list is pulled directly from our source code.</span></span>  <span data-ttu-id="1161c-146">Исходный код для платформы Entity Framework 6 можно найти в [GitHub](https://github.com/aspnet/entityframework6/) и хорошо подходят многие из соглашения, используемые платформой Entity Framework отправных точек для пользовательской модели на основе соглашений.</span><span class="sxs-lookup"><span data-stu-id="1161c-146">The source code for Entity Framework 6 is available on [GitHub](https://github.com/aspnet/entityframework6/) and many of the conventions used by Entity Framework are good starting points for custom model based conventions.</span></span>  
