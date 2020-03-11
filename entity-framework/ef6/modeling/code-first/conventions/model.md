---
title: Соглашения на основе модели — EF6
author: divega
ms.date: 10/23/2016
ms.assetid: 0fc4eef8-29b8-4192-9c77-08fd33d3db3a
ms.openlocfilehash: c873e9a216bd9bd1934f2149ae6af602072f3608
ms.sourcegitcommit: cc0ff36e46e9ed3527638f7208000e8521faef2e
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/06/2020
ms.locfileid: "78415847"
---
# <a name="model-based-conventions"></a><span data-ttu-id="59dbc-102">Соглашения на основе модели</span><span class="sxs-lookup"><span data-stu-id="59dbc-102">Model-Based Conventions</span></span>
> [!NOTE]
> <span data-ttu-id="59dbc-103">**Только в EF6 и более поздних версиях**. Функции, API и другие возможности, описанные на этой странице, появились в Entity Framework 6.</span><span class="sxs-lookup"><span data-stu-id="59dbc-103">**EF6 Onwards Only** - The features, APIs, etc. discussed in this page were introduced in Entity Framework 6.</span></span> <span data-ttu-id="59dbc-104">При использовании более ранней версии могут быть неприменимы некоторые или все сведения.</span><span class="sxs-lookup"><span data-stu-id="59dbc-104">If you are using an earlier version, some or all of the information does not apply.</span></span>  

<span data-ttu-id="59dbc-105">Соглашения на основе модели являются расширенным методом настройки модели на основе соглашений.</span><span class="sxs-lookup"><span data-stu-id="59dbc-105">Model based conventions are an advanced method of convention based model configuration.</span></span> <span data-ttu-id="59dbc-106">В большинстве сценариев следует использовать [Пользовательский API соглашений Code First для дбмоделбуилдер](~/ef6/modeling/code-first/conventions/custom.md) .</span><span class="sxs-lookup"><span data-stu-id="59dbc-106">For most scenarios the [Custom Code First Convention API on DbModelBuilder](~/ef6/modeling/code-first/conventions/custom.md) should be used.</span></span> <span data-ttu-id="59dbc-107">Перед использованием соглашений на основе модели рекомендуется ознакомиться с Дбмоделбуилдер API для соглашений.</span><span class="sxs-lookup"><span data-stu-id="59dbc-107">An understanding of the DbModelBuilder API for conventions is recommended before using model based conventions.</span></span>  

<span data-ttu-id="59dbc-108">Соглашения на основе модели позволяют создавать соглашения, влияющие на свойства и таблицы, которые не настраиваются с помощью стандартных соглашений.</span><span class="sxs-lookup"><span data-stu-id="59dbc-108">Model based conventions allow the creation of conventions that affect properties and tables which are not configurable through standard conventions.</span></span> <span data-ttu-id="59dbc-109">Примерами являются столбцы дискриминатора в таблицах на модели иерархии и независимые столбцы взаимосвязей.</span><span class="sxs-lookup"><span data-stu-id="59dbc-109">Examples of these are discriminator columns in table per hierarchy models and Independent Association columns.</span></span>  

## <a name="creating-a-convention"></a><span data-ttu-id="59dbc-110">Создание соглашения</span><span class="sxs-lookup"><span data-stu-id="59dbc-110">Creating a Convention</span></span>   

<span data-ttu-id="59dbc-111">Первым шагом в создании соглашения на основе модели является выбор того, когда в конвейере необходимо применить соглашение к модели.</span><span class="sxs-lookup"><span data-stu-id="59dbc-111">The first step in creating a model based convention is choosing when in the pipeline the convention needs to be applied to the model.</span></span> <span data-ttu-id="59dbc-112">Существует два типа соглашений о модели: концептуальные (C-Space) и Store (S-Space).</span><span class="sxs-lookup"><span data-stu-id="59dbc-112">There are two types of model conventions, Conceptual (C-Space) and Store (S-Space).</span></span> <span data-ttu-id="59dbc-113">К модели, создаваемой приложением, применяется соглашение о пробелах, а в версии модели, представляющей базу данных, применяется соглашение об использовании пространства, а также элементы управления, например способ именования автоматически созданных столбцов.</span><span class="sxs-lookup"><span data-stu-id="59dbc-113">A C-Space convention is applied to the model that the application builds, whereas an S-Space convention is applied to the version of the model that represents the database and controls things such as how automatically-generated columns are named.</span></span>  

<span data-ttu-id="59dbc-114">Соглашение модели — это класс, который расширяется из Иконцептуалмоделконвентион или Исторемоделконвентион.</span><span class="sxs-lookup"><span data-stu-id="59dbc-114">A model convention is a class that extends from either IConceptualModelConvention or IStoreModelConvention.</span></span>  <span data-ttu-id="59dbc-115">Оба этих интерфейса принимают универсальный тип, который может иметь тип Метадатаитем, который используется для фильтрации типа данных, к которому применяется соглашение.</span><span class="sxs-lookup"><span data-stu-id="59dbc-115">These interfaces both accept a generic type that can be of type MetadataItem which is used to filter the data type that the convention applies to.</span></span>  

## <a name="adding-a-convention"></a><span data-ttu-id="59dbc-116">Добавление соглашения</span><span class="sxs-lookup"><span data-stu-id="59dbc-116">Adding a Convention</span></span>   

<span data-ttu-id="59dbc-117">Правила модели добавляются так же, как и обычные классы соглашений.</span><span class="sxs-lookup"><span data-stu-id="59dbc-117">Model conventions are added in the same way as regular conventions classes.</span></span> <span data-ttu-id="59dbc-118">В методе **OnModelCreating** добавьте соглашение в список соглашений для модели.</span><span class="sxs-lookup"><span data-stu-id="59dbc-118">In the **OnModelCreating** method, add the convention to the list of conventions for a model.</span></span>  

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

<span data-ttu-id="59dbc-119">Соглашение также можно добавить по отношению к другому соглашению с помощью методов Аддбефоре\<\> или соглашений. Аддафтер\<\>.</span><span class="sxs-lookup"><span data-stu-id="59dbc-119">A convention can also be added in relation to another convention using the Conventions.AddBefore\<\> or Conventions.AddAfter\<\> methods.</span></span> <span data-ttu-id="59dbc-120">Дополнительные сведения о соглашениях, которые Entity Framework применяются, см. в разделе "Примечания".</span><span class="sxs-lookup"><span data-stu-id="59dbc-120">For more information about the conventions that Entity Framework applies see the notes section.</span></span>  

``` csharp
protected override void OnModelCreating(DbModelBuilder modelBuilder)
{
    modelBuilder.Conventions.AddAfter<IdKeyDiscoveryConvention>(new MyModelBasedConvention());
}
```  

## <a name="example-discriminator-model-convention"></a><span data-ttu-id="59dbc-121">Пример: соглашение о модели дискриминатора</span><span class="sxs-lookup"><span data-stu-id="59dbc-121">Example: Discriminator Model Convention</span></span>  

<span data-ttu-id="59dbc-122">Переименование столбцов, созданных EF, является примером чего-то, что нельзя сделать с помощью других API-интерфейсов соглашений.</span><span class="sxs-lookup"><span data-stu-id="59dbc-122">Renaming columns generated by EF is an example of something that you can’t do with the other conventions APIs.</span></span>  <span data-ttu-id="59dbc-123">Это ситуация, когда единственным вариантом является использование соглашений о модели.</span><span class="sxs-lookup"><span data-stu-id="59dbc-123">This is a situation where using model conventions is your only option.</span></span>  

<span data-ttu-id="59dbc-124">Пример использования соглашения на основе модели для настройки созданных столбцов заключается в настройке способа именования столбцов дискриминатора.</span><span class="sxs-lookup"><span data-stu-id="59dbc-124">An example of how to use a model based convention to configure generated columns is customizing the way discriminator columns are named.</span></span>  <span data-ttu-id="59dbc-125">Ниже приведен пример простого соглашения на основе модели, которое переименовывает каждый столбец в модели с именем "дискриминатор" в "EntityType".</span><span class="sxs-lookup"><span data-stu-id="59dbc-125">Below is an example of a simple model based convention that renames every column in the model named “Discriminator” to “EntityType”.</span></span>  <span data-ttu-id="59dbc-126">Сюда входят столбцы, которые разработчик просто назвал «дискриминатор».</span><span class="sxs-lookup"><span data-stu-id="59dbc-126">This includes columns that the developer simply named “Discriminator”.</span></span> <span data-ttu-id="59dbc-127">Так как столбец "дискриминатор" является сгенерированным столбцом, он должен выполняться в S-Space.</span><span class="sxs-lookup"><span data-stu-id="59dbc-127">Since the “Discriminator” column is a generated column this needs to run in S-Space.</span></span>  

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

## <a name="example-general-ia-renaming-convention"></a><span data-ttu-id="59dbc-128">Пример. Общее соглашение о переименовании IA</span><span class="sxs-lookup"><span data-stu-id="59dbc-128">Example: General IA Renaming Convention</span></span>   

<span data-ttu-id="59dbc-129">Еще один более сложный пример соглашений на основе модели в действии состоит в настройке способа именования независимых ассоциаций (IAs).</span><span class="sxs-lookup"><span data-stu-id="59dbc-129">Another more complicated example of model based conventions in action is to configure the way that Independent Associations (IAs) are named.</span></span>  <span data-ttu-id="59dbc-130">Это ситуация, когда применяются соглашения модели, так как службы IAs создаются EF и отсутствуют в модели, к которой может получить доступ API Дбмоделбуилдер.</span><span class="sxs-lookup"><span data-stu-id="59dbc-130">This is a situation where Model conventions are applicable because IAs are generated by EF and aren’t present in the model that the DbModelBuilder API can access.</span></span>  

<span data-ttu-id="59dbc-131">Когда EF создает IA, он создает столбец с именем EntityType_KeyName.</span><span class="sxs-lookup"><span data-stu-id="59dbc-131">When EF generates an IA, it creates a column named EntityType_KeyName.</span></span> <span data-ttu-id="59dbc-132">Например, для ассоциации с именем Customer с ключевым столбцом CustomerId будет создан столбец с именем Customer_CustomerId.</span><span class="sxs-lookup"><span data-stu-id="59dbc-132">For example, for an association named Customer with a key column named CustomerId it would generate a column named Customer_CustomerId.</span></span> <span data-ttu-id="59dbc-133">В следующем соглашении символ "\_" отделяется от имени столбца, созданного для IA.</span><span class="sxs-lookup"><span data-stu-id="59dbc-133">The following convention strips the ‘\_’ character out of the column name that is generated for the IA.</span></span>  

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

## <a name="extending-existing-conventions"></a><span data-ttu-id="59dbc-134">Расширение существующих соглашений</span><span class="sxs-lookup"><span data-stu-id="59dbc-134">Extending Existing Conventions</span></span>   

<span data-ttu-id="59dbc-135">Если необходимо написать соглашение, похожее на одно из соглашений, которые Entity Framework уже применяются к модели, всегда можно расширить это соглашение, чтобы избежать необходимости переписывать его с нуля.</span><span class="sxs-lookup"><span data-stu-id="59dbc-135">If you need to write a convention that is similar to one of the conventions that Entity Framework already applies to your model you can always extend that convention to avoid having to rewrite it from scratch.</span></span>  <span data-ttu-id="59dbc-136">Примером этого является замена существующего соглашения о сопоставлении ID на другое.</span><span class="sxs-lookup"><span data-stu-id="59dbc-136">An example of this is to replace the existing Id matching convention with a custom one.</span></span>   <span data-ttu-id="59dbc-137">Дополнительным преимуществом переопределения соглашения о ключах является то, что переопределенный метод будет вызываться только в том случае, если ключ уже не определен или явно не настроен.</span><span class="sxs-lookup"><span data-stu-id="59dbc-137">An added benefit to overriding the key convention is that the overridden method will get called only if there is no key already detected or explicitly configured.</span></span> <span data-ttu-id="59dbc-138">Список соглашений, используемых Entity Framework, доступен здесь: [http://msdn.microsoft.com/library/system.data.entity.modelconfiguration.conventions.aspx](https://msdn.microsoft.com/library/system.data.entity.modelconfiguration.conventions.aspx).</span><span class="sxs-lookup"><span data-stu-id="59dbc-138">A list of conventions that used by Entity Framework is available here: [http://msdn.microsoft.com/library/system.data.entity.modelconfiguration.conventions.aspx](https://msdn.microsoft.com/library/system.data.entity.modelconfiguration.conventions.aspx).</span></span>  

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

<span data-ttu-id="59dbc-139">Затем необходимо добавить наше новое соглашение перед существующим соглашением о ключе.</span><span class="sxs-lookup"><span data-stu-id="59dbc-139">We then need to add our new convention before the existing key convention.</span></span> <span data-ttu-id="59dbc-140">После добавления Кустомкэйдисковериконвентион можно удалить Идкэйдисковериконвентион.</span><span class="sxs-lookup"><span data-stu-id="59dbc-140">After we add the CustomKeyDiscoveryConvention, we can remove the IdKeyDiscoveryConvention.</span></span>  <span data-ttu-id="59dbc-141">Если мы не удалим существующие Идкэйдисковериконвентион, это соглашение будет иметь приоритет над соглашением об обнаружении идентификатора, так как оно выполняется первым, но в случае, когда не найдено свойство "Key", будет выполняться соглашение "ID".</span><span class="sxs-lookup"><span data-stu-id="59dbc-141">If we didn’t remove the existing IdKeyDiscoveryConvention this convention would still take precedence over the Id discovery convention since it is run first, but in the case where no “key” property is found, the “id” convention will run.</span></span>  <span data-ttu-id="59dbc-142">Мы видим такое поведение, так как каждое соглашение видит модель в соответствии с предыдущим соглашением (вместо того, чтобы управлять им независимо друг от друга и объединяется вместе), поэтому, если, например, предыдущее соглашение обновило имя столбца в соответствии с каким-либо из интерес к пользовательскому соглашению (когда до того, как имя не будет представлять интерес), оно будет применено к этому столбцу.</span><span class="sxs-lookup"><span data-stu-id="59dbc-142">We see this behavior because each convention sees the model as updated by the previous convention (rather than operating on it independently and all being combined together) so that if for example, a previous convention updated a column name to match something of interest to your custom convention (when before that the name was not of interest) then it will apply to that column.</span></span>  

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

## <a name="notes"></a><span data-ttu-id="59dbc-143">Примечания</span><span class="sxs-lookup"><span data-stu-id="59dbc-143">Notes</span></span>  

<span data-ttu-id="59dbc-144">Список соглашений, которые в настоящее время применяются Entity Framework, доступен в документации MSDN здесь: [http://msdn.microsoft.com/library/system.data.entity.modelconfiguration.conventions.aspx](https://msdn.microsoft.com/library/system.data.entity.modelconfiguration.conventions.aspx).</span><span class="sxs-lookup"><span data-stu-id="59dbc-144">A list of conventions that are currently applied by Entity Framework is available in the MSDN documentation here: [http://msdn.microsoft.com/library/system.data.entity.modelconfiguration.conventions.aspx](https://msdn.microsoft.com/library/system.data.entity.modelconfiguration.conventions.aspx).</span></span>  <span data-ttu-id="59dbc-145">Этот список извлекается непосредственно из нашего исходного кода.</span><span class="sxs-lookup"><span data-stu-id="59dbc-145">This list is pulled directly from our source code.</span></span>  <span data-ttu-id="59dbc-146">Исходный код для Entity Framework 6 доступен в [GitHub](https://github.com/aspnet/entityframework6/) , и многие из соглашений, используемых Entity Framework, являются хорошими отправными точками для настраиваемых соглашений на основе модели.</span><span class="sxs-lookup"><span data-stu-id="59dbc-146">The source code for Entity Framework 6 is available on [GitHub](https://github.com/aspnet/entityframework6/) and many of the conventions used by Entity Framework are good starting points for custom model based conventions.</span></span>  
