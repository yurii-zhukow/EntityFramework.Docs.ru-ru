---
title: Спецификация языка CSDL — EF6
description: Спецификация языка CSDL в Entity Framework 6
author: ajcvickers
ms.date: 10/23/2016
uid: ef6/modeling/designer/advanced/edmx/csdl-spec
ms.openlocfilehash: 9fdd8fc5ed16f7ba7d11e79a9449f120f5d579c2
ms.sourcegitcommit: 0a25c03fa65ae6e0e0e3f66bac48d59eceb96a5a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/14/2020
ms.locfileid: "92066360"
---
# <a name="csdl-specification"></a><span data-ttu-id="5fdd5-103">Спецификация CSDL</span><span class="sxs-lookup"><span data-stu-id="5fdd5-103">CSDL Specification</span></span>
<span data-ttu-id="5fdd5-104">Язык CSDL — это язык на основе XML, описывающий сущности, связи и функции, составляющие концептуальную модель управляемого данными приложения.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-104">Conceptual schema definition language (CSDL) is an XML-based language that describes the entities, relationships, and functions that make up a conceptual model of a data-driven application.</span></span> <span data-ttu-id="5fdd5-105">Эта концептуальная модель может использоваться Entity Framework или WCF Data Services.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-105">This conceptual model can be used by the Entity Framework or WCF Data Services.</span></span> <span data-ttu-id="5fdd5-106">Метаданные, описанные в языке CSDL, используются Entity Framework для соотнесения сущностей и связей, определенных в концептуальной модели, к источнику данных.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-106">The metadata that is described with CSDL is used by the Entity Framework to map entities and relationships that are defined in a conceptual model to a data source.</span></span> <span data-ttu-id="5fdd5-107">Дополнительные сведения см. в статье [спецификации языка SSDL](xref:ef6/modeling/designer/advanced/edmx/ssdl-spec) и [спецификации MSL](xref:ef6/modeling/designer/advanced/edmx/msl-spec).</span><span class="sxs-lookup"><span data-stu-id="5fdd5-107">For more information, see [SSDL Specification](xref:ef6/modeling/designer/advanced/edmx/ssdl-spec) and [MSL Specification](xref:ef6/modeling/designer/advanced/edmx/msl-spec).</span></span>

<span data-ttu-id="5fdd5-108">Язык CSDL — это реализация EDM Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-108">CSDL is the Entity Framework's implementation of the Entity Data Model.</span></span>

<span data-ttu-id="5fdd5-109">В приложении Entity Framework метаданные концептуальной модели загружаются из CSDL-файла (написанного на языке CSDL) в экземпляр класса System. Data. Metadata. EDM. коллекций EdmItemCollection и доступны с помощью методов в классе System. Data. Metadata. EDM. MetadataWorkspace.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-109">In an Entity Framework application, conceptual model metadata is loaded from a .csdl file (written in CSDL) into an instance of the System.Data.Metadata.Edm.EdmItemCollection and is accessible by using methods in the System.Data.Metadata.Edm.MetadataWorkspace class.</span></span> <span data-ttu-id="5fdd5-110">Entity Framework использует метаданные концептуальной модели для преобразования запросов к концептуальной модели в команды, относящиеся к источнику данных.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-110">Entity Framework uses conceptual model metadata to translate queries against the conceptual model to data source-specific commands.</span></span>

<span data-ttu-id="5fdd5-111">Конструктор EF сохраняет сведения о концептуальной модели в EDMX файле во время разработки.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-111">The EF Designer stores conceptual model information in an .edmx file at design time.</span></span> <span data-ttu-id="5fdd5-112">Во время сборки конструктор EF использует сведения в EDMX-файле для создания CSDL-файла, необходимого для Entity Framework во время выполнения.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-112">At build time, the EF Designer uses information in an .edmx file to create the .csdl file that is needed by Entity Framework at runtime.</span></span>

<span data-ttu-id="5fdd5-113">Версии языка CSDL различаются по пространствам имен XML.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-113">Versions of CSDL are differentiated by XML namespaces.</span></span>

| <span data-ttu-id="5fdd5-114">Версия языка CSDL</span><span class="sxs-lookup"><span data-stu-id="5fdd5-114">CSDL Version</span></span> | <span data-ttu-id="5fdd5-115">Пространство имен XML</span><span class="sxs-lookup"><span data-stu-id="5fdd5-115">XML Namespace</span></span>                                |
|:-------------|:---------------------------------------------|
| <span data-ttu-id="5fdd5-116">Язык CSDL v1</span><span class="sxs-lookup"><span data-stu-id="5fdd5-116">CSDL v1</span></span>      | https://schemas.microsoft.com/ado/2006/04/edm |
| <span data-ttu-id="5fdd5-117">Язык CSDL v2</span><span class="sxs-lookup"><span data-stu-id="5fdd5-117">CSDL v2</span></span>      | https://schemas.microsoft.com/ado/2008/09/edm |
| <span data-ttu-id="5fdd5-118">CSDL v3</span><span class="sxs-lookup"><span data-stu-id="5fdd5-118">CSDL v3</span></span>      | https://schemas.microsoft.com/ado/2009/11/edm |

 
## <a name="association-element-csdl"></a><span data-ttu-id="5fdd5-119">Элемент Association (CSDL)</span><span class="sxs-lookup"><span data-stu-id="5fdd5-119">Association Element (CSDL)</span></span>

<span data-ttu-id="5fdd5-120">Элемент **Association** определяет связь между двумя типами сущностей.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-120">An **Association** element defines a relationship between two entity types.</span></span> <span data-ttu-id="5fdd5-121">Ассоциация должна указывать типы сущностей, которые участвуют в связи, и возможное количество типов сущностей на каждом конце связи, которое называется кратностью.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-121">An association must specify the entity types that are involved in the relationship and the possible number of entity types at each end of the relationship, which is known as the multiplicity.</span></span> <span data-ttu-id="5fdd5-122">Кратность элемента ассоциации может иметь значение один (1), ноль или один (0.. 1) или многие ( \* ).</span><span class="sxs-lookup"><span data-stu-id="5fdd5-122">The multiplicity of an association end can have a value of one (1), zero or one (0..1), or many (\*).</span></span> <span data-ttu-id="5fdd5-123">Эти сведения заданы в двух дочерних элементах End.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-123">This information is specified in two child End elements.</span></span>

<span data-ttu-id="5fdd5-124">К экземплярам типов сущностей в одном элементе ассоциации можно обращаться посредством свойств навигации или внешних ключей, если они предоставлены в типе сущности.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-124">Entity type instances at one end of an association can be accessed through navigation properties or foreign keys, if they are exposed on an entity type.</span></span>

<span data-ttu-id="5fdd5-125">В приложении экземпляр ассоциации представляет конкретную ассоциацию между экземплярами типов сущностей.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-125">In an application, an instance of an association represents a specific association between instances of entity types.</span></span> <span data-ttu-id="5fdd5-126">Экземпляры ассоциации логически сгруппированы в набор ассоциаций.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-126">Association instances are logically grouped in an association set.</span></span>

<span data-ttu-id="5fdd5-127">Элемент **Association** может иметь следующие дочерние элементы (в указанном порядке):</span><span class="sxs-lookup"><span data-stu-id="5fdd5-127">An **Association** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="5fdd5-128">Documentation (ноль или один элемент)</span><span class="sxs-lookup"><span data-stu-id="5fdd5-128">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="5fdd5-129">End (ровно 2 элемента)</span><span class="sxs-lookup"><span data-stu-id="5fdd5-129">End (exactly 2 elements)</span></span>
-   <span data-ttu-id="5fdd5-130">ReferentialConstraint (ноль или один элемент)</span><span class="sxs-lookup"><span data-stu-id="5fdd5-130">ReferentialConstraint (zero or one element)</span></span>
-   <span data-ttu-id="5fdd5-131">Элементы Annotation (ноль или более элементов)</span><span class="sxs-lookup"><span data-stu-id="5fdd5-131">Annotation elements (zero or more elements)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="5fdd5-132">Применимые атрибуты</span><span class="sxs-lookup"><span data-stu-id="5fdd5-132">Applicable Attributes</span></span>

<span data-ttu-id="5fdd5-133">В следующей таблице описаны атрибуты, которые можно применить к элементу **Association** .</span><span class="sxs-lookup"><span data-stu-id="5fdd5-133">The table below describes the attributes that can be applied to the **Association** element.</span></span>

| <span data-ttu-id="5fdd5-134">Имя атрибута</span><span class="sxs-lookup"><span data-stu-id="5fdd5-134">Attribute Name</span></span> | <span data-ttu-id="5fdd5-135">Обязательный</span><span class="sxs-lookup"><span data-stu-id="5fdd5-135">Is Required</span></span> | <span data-ttu-id="5fdd5-136">Значение</span><span class="sxs-lookup"><span data-stu-id="5fdd5-136">Value</span></span>                        |
|:---------------|:------------|:-----------------------------|
| <span data-ttu-id="5fdd5-137">**имя**;</span><span class="sxs-lookup"><span data-stu-id="5fdd5-137">**Name**</span></span>       | <span data-ttu-id="5fdd5-138">Да</span><span class="sxs-lookup"><span data-stu-id="5fdd5-138">Yes</span></span>         | <span data-ttu-id="5fdd5-139">Имя ассоциации.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-139">The name of the association.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="5fdd5-140">К элементу **Association** можно применить любое количество атрибутов аннотации (настраиваемые XML-атрибуты).</span><span class="sxs-lookup"><span data-stu-id="5fdd5-140">Any number of annotation attributes (custom XML attributes) may be applied to the **Association** element.</span></span> <span data-ttu-id="5fdd5-141">Однако настраиваемые атрибуты не могут принадлежать к любому пространству имен XML, зарезервированному для CSDL.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-141">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="5fdd5-142">Полные имена любых двух настраиваемых атрибутов не могут совпадать.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-142">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="5fdd5-143">Пример</span><span class="sxs-lookup"><span data-stu-id="5fdd5-143">Example</span></span>

<span data-ttu-id="5fdd5-144">В следующем примере показан элемент **Association** , определяющий ассоциацию **кустомерордерс** , когда внешние ключи не были представлены в типах сущностей **Customer** и **Order** .</span><span class="sxs-lookup"><span data-stu-id="5fdd5-144">The following example shows an **Association** element that defines the **CustomerOrders** association when foreign keys have not been exposed on the **Customer** and **Order** entity types.</span></span> <span data-ttu-id="5fdd5-145">Значения **кратности** для каждого **элемента** ассоциации указывают на то, что многие **заказы** могут быть связаны с **клиентом**, но только один **клиент** может быть связан с **заказом**.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-145">The **Multiplicity** values for each **End** of the association indicate that many **Orders** can be associated with a **Customer**, but only one **Customer** can be associated with an **Order**.</span></span> <span data-ttu-id="5fdd5-146">Кроме того, элемент **OnDelete** указывает, что все **заказы** , связанные с конкретным **клиентом** и загруженные в ObjectContext, будут удалены при удалении **клиента** .</span><span class="sxs-lookup"><span data-stu-id="5fdd5-146">Additionally, the **OnDelete** element indicates that all **Orders** that are related to a particular **Customer** and have been loaded into the ObjectContext will be deleted if the **Customer** is deleted.</span></span>

``` xml
 <Association Name="CustomerOrders">
   <End Type="ExampleModel.Customer" Role="Customer" Multiplicity="1" >
         <OnDelete Action="Cascade" />
   </End>
   <End Type="ExampleModel.Order" Role="Order" Multiplicity="*" />
 </Association>
```
 

<span data-ttu-id="5fdd5-147">В следующем примере показан элемент **Association** , определяющий ассоциацию **кустомерордерс** , когда внешние ключи были представлены в типах сущностей **Customer** и **Order** .</span><span class="sxs-lookup"><span data-stu-id="5fdd5-147">The following example shows an **Association** element that defines the **CustomerOrders** association when foreign keys have been exposed on the **Customer** and **Order** entity types.</span></span> <span data-ttu-id="5fdd5-148">При предоставлении внешних ключей связь между сущностями управляется с помощью элемента **ReferentialConstraint** .</span><span class="sxs-lookup"><span data-stu-id="5fdd5-148">With foreign keys exposed, the relationship between the entities is managed with a **ReferentialConstraint** element.</span></span> <span data-ttu-id="5fdd5-149">Для сопоставления этой ассоциации с источником данных элемент AssociationSetMapping не требуется.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-149">A corresponding AssociationSetMapping element is not necessary to map this association to the data source.</span></span>

``` xml
 <Association Name="CustomerOrders">
   <End Type="ExampleModel.Customer" Role="Customer" Multiplicity="1" >
         <OnDelete Action="Cascade" />
   </End>
   <End Type="ExampleModel.Order" Role="Order" Multiplicity="*" />
   <ReferentialConstraint>
        <Principal Role="Customer">
            <PropertyRef Name="Id" />
        </Principal>
        <Dependent Role="Order">
             <PropertyRef Name="CustomerId" />
         </Dependent>
   </ReferentialConstraint>
 </Association>
```
 

 

## <a name="associationset-element-csdl"></a><span data-ttu-id="5fdd5-150">Элемент AssociationSet (CSDL)</span><span class="sxs-lookup"><span data-stu-id="5fdd5-150">AssociationSet Element (CSDL)</span></span>

<span data-ttu-id="5fdd5-151">Элемент **AssociationSet** в языке CSDL — это логический контейнер для экземпляров ассоциаций того же типа.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-151">The **AssociationSet** element in conceptual schema definition language (CSDL) is a logical container for association instances of the same type.</span></span> <span data-ttu-id="5fdd5-152">Набор ассоциаций предоставляет определение группы экземпляров ассоциаций, чтобы их можно было сопоставить с источником данных.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-152">An association set provides a definition for grouping association instances so that they can be mapped to a data source.</span></span>  

<span data-ttu-id="5fdd5-153">Элемент **AssociationSet** может иметь следующие дочерние элементы (в указанном порядке):</span><span class="sxs-lookup"><span data-stu-id="5fdd5-153">The **AssociationSet** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="5fdd5-154">Documentation (допустимое количество элементов — ноль или один)</span><span class="sxs-lookup"><span data-stu-id="5fdd5-154">Documentation (zero or one elements allowed)</span></span>
-   <span data-ttu-id="5fdd5-155">End (требуется ровно 2 элемента)</span><span class="sxs-lookup"><span data-stu-id="5fdd5-155">End (exactly two elements required)</span></span>
-   <span data-ttu-id="5fdd5-156">Элементы Annotation (допустимое количество элементов — ноль или больше)</span><span class="sxs-lookup"><span data-stu-id="5fdd5-156">Annotation elements (zero or more elements allowed)</span></span>

<span data-ttu-id="5fdd5-157">Атрибут **Association** указывает тип ассоциации, который содержит набор ассоциаций.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-157">The **Association** attribute specifies the type of association that an association set contains.</span></span> <span data-ttu-id="5fdd5-158">Наборы сущностей, которые составляют конечные точки набора ассоциаций, задаются ровно с двумя дочерними **элементами.**</span><span class="sxs-lookup"><span data-stu-id="5fdd5-158">The entity sets that make up the ends of an association set are specified with exactly two child **End** elements.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="5fdd5-159">Применимые атрибуты</span><span class="sxs-lookup"><span data-stu-id="5fdd5-159">Applicable Attributes</span></span>

<span data-ttu-id="5fdd5-160">В следующей таблице описаны атрибуты, которые можно применить к элементу **AssociationSet** .</span><span class="sxs-lookup"><span data-stu-id="5fdd5-160">The table below describes the attributes that can be applied to the **AssociationSet** element.</span></span>

| <span data-ttu-id="5fdd5-161">Имя атрибута</span><span class="sxs-lookup"><span data-stu-id="5fdd5-161">Attribute Name</span></span>  | <span data-ttu-id="5fdd5-162">Обязательный</span><span class="sxs-lookup"><span data-stu-id="5fdd5-162">Is Required</span></span> | <span data-ttu-id="5fdd5-163">Значение</span><span class="sxs-lookup"><span data-stu-id="5fdd5-163">Value</span></span>                                                                                                                                                             |
|:----------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="5fdd5-164">**имя**;</span><span class="sxs-lookup"><span data-stu-id="5fdd5-164">**Name**</span></span>        | <span data-ttu-id="5fdd5-165">Да</span><span class="sxs-lookup"><span data-stu-id="5fdd5-165">Yes</span></span>         | <span data-ttu-id="5fdd5-166">Имя набора сущностей.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-166">The name of the entity set.</span></span> <span data-ttu-id="5fdd5-167">Значение атрибута **Name** не может совпадать со значением атрибута **Association** .</span><span class="sxs-lookup"><span data-stu-id="5fdd5-167">The value of the **Name** attribute cannot be the same as the value of the **Association** attribute.</span></span>                                 |
| <span data-ttu-id="5fdd5-168">**Взаимосвязь**</span><span class="sxs-lookup"><span data-stu-id="5fdd5-168">**Association**</span></span> | <span data-ttu-id="5fdd5-169">Да</span><span class="sxs-lookup"><span data-stu-id="5fdd5-169">Yes</span></span>         | <span data-ttu-id="5fdd5-170">Полное имя ассоциации, экземпляры которой содержатся в наборе ассоциаций.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-170">The fully-qualified name of the association that the association set contains instances of.</span></span> <span data-ttu-id="5fdd5-171">Ассоциация должна находиться в том же пространстве имен, что и набор ассоциаций.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-171">The association must be in the same namespace as the association set.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="5fdd5-172">К элементу **AssociationSet** можно применить любое количество атрибутов аннотации (настраиваемые XML-атрибуты).</span><span class="sxs-lookup"><span data-stu-id="5fdd5-172">Any number of annotation attributes (custom XML attributes) may be applied to the **AssociationSet** element.</span></span> <span data-ttu-id="5fdd5-173">Однако настраиваемые атрибуты не могут принадлежать к любому пространству имен XML, зарезервированному для CSDL.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-173">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="5fdd5-174">Полные имена любых двух настраиваемых атрибутов не могут совпадать.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-174">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="5fdd5-175">Пример</span><span class="sxs-lookup"><span data-stu-id="5fdd5-175">Example</span></span>

<span data-ttu-id="5fdd5-176">В следующем примере показан элемент **EntityContainer** с двумя элементами **AssociationSet** :</span><span class="sxs-lookup"><span data-stu-id="5fdd5-176">The following example shows an **EntityContainer** element with two **AssociationSet** elements:</span></span>

``` xml
 <EntityContainer Name="BooksContainer" >
   <EntitySet Name="Books" EntityType="BooksModel.Book" />
   <EntitySet Name="Publishers" EntityType="BooksModel.Publisher" />
   <EntitySet Name="Authors" EntityType="BooksModel.Author" />
   <AssociationSet Name="PublishedBy" Association="BooksModel.PublishedBy">
     <End Role="Book" EntitySet="Books" />
     <End Role="Publisher" EntitySet="Publishers" />
   </AssociationSet>
   <AssociationSet Name="WrittenBy" Association="BooksModel.WrittenBy">
     <End Role="Book" EntitySet="Books" />
     <End Role="Author" EntitySet="Authors" />
   </AssociationSet>
 </EntityContainer>
```
 

 

## <a name="collectiontype-element-csdl"></a><span data-ttu-id="5fdd5-177">Элемент CollectionType (язык CSDL)</span><span class="sxs-lookup"><span data-stu-id="5fdd5-177">CollectionType Element (CSDL)</span></span>

<span data-ttu-id="5fdd5-178">Элемент **CollectionType** в языке CSDL указывает, что параметр функции или тип возвращаемого значения функции является коллекцией.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-178">The **CollectionType** element in conceptual schema definition language (CSDL) specifies that a function parameter or function return type is a collection.</span></span> <span data-ttu-id="5fdd5-179">Элемент **CollectionType** может быть дочерним по отношению к элементу Parameter или ReturnType (Function).</span><span class="sxs-lookup"><span data-stu-id="5fdd5-179">The **CollectionType** element can be a child of the Parameter element or the ReturnType (Function) element.</span></span> <span data-ttu-id="5fdd5-180">Тип коллекции можно указать с помощью либо атрибута **Type** , либо одного из следующих дочерних элементов:</span><span class="sxs-lookup"><span data-stu-id="5fdd5-180">The type of collection can be specified by using either the **Type** attribute or one of the following child elements:</span></span>

-   <span data-ttu-id="5fdd5-181">**CollectionType**</span><span class="sxs-lookup"><span data-stu-id="5fdd5-181">**CollectionType**</span></span>
-   <span data-ttu-id="5fdd5-182">Тип ссылки</span><span class="sxs-lookup"><span data-stu-id="5fdd5-182">ReferenceType</span></span>
-   <span data-ttu-id="5fdd5-183">RowType</span><span class="sxs-lookup"><span data-stu-id="5fdd5-183">RowType</span></span>
-   <span data-ttu-id="5fdd5-184">TypeRef</span><span class="sxs-lookup"><span data-stu-id="5fdd5-184">TypeRef</span></span>

> [!NOTE]
> <span data-ttu-id="5fdd5-185">Модель не будет проверять, указан ли тип коллекции с атрибутом **Type** и дочерним элементом.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-185">A model will not validate if the type of a collection is specified with both the **Type** attribute and a child element.</span></span>

 

### <a name="applicable-attributes"></a><span data-ttu-id="5fdd5-186">Применимые атрибуты</span><span class="sxs-lookup"><span data-stu-id="5fdd5-186">Applicable Attributes</span></span>

<span data-ttu-id="5fdd5-187">В следующей таблице описаны атрибуты, которые можно применить к элементу **CollectionType** .</span><span class="sxs-lookup"><span data-stu-id="5fdd5-187">The following table describes the attributes that can be applied to the **CollectionType** element.</span></span> <span data-ttu-id="5fdd5-188">Обратите внимание, что атрибуты **DefaultValue**, **MaxLength**, **FixedLength**, **точность**, **масштаб**, **Юникод**и **Параметры сортировки** применимы только к коллекциям **едмсимплетипес**.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-188">Note that the **DefaultValue**, **MaxLength**, **FixedLength**, **Precision**, **Scale**, **Unicode**, and **Collation** attributes are only applicable to collections of **EDMSimpleTypes**.</span></span>

| <span data-ttu-id="5fdd5-189">Имя атрибута</span><span class="sxs-lookup"><span data-stu-id="5fdd5-189">Attribute Name</span></span>                                                          | <span data-ttu-id="5fdd5-190">Обязательный</span><span class="sxs-lookup"><span data-stu-id="5fdd5-190">Is Required</span></span> | <span data-ttu-id="5fdd5-191">Значение</span><span class="sxs-lookup"><span data-stu-id="5fdd5-191">Value</span></span>                                                                                                                                                                                                                            |
|:------------------------------------------------------------------------|:------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="5fdd5-192">**Тип**</span><span class="sxs-lookup"><span data-stu-id="5fdd5-192">**Type**</span></span>                                                                | <span data-ttu-id="5fdd5-193">Нет</span><span class="sxs-lookup"><span data-stu-id="5fdd5-193">No</span></span>          | <span data-ttu-id="5fdd5-194">Тип коллекции.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-194">The type of the collection.</span></span>                                                                                                                                                                                                      |
| <span data-ttu-id="5fdd5-195">**Допускает значения NULL**</span><span class="sxs-lookup"><span data-stu-id="5fdd5-195">**Nullable**</span></span>                                                            | <span data-ttu-id="5fdd5-196">Нет</span><span class="sxs-lookup"><span data-stu-id="5fdd5-196">No</span></span>          | <span data-ttu-id="5fdd5-197">**True** (значение по умолчанию) или **False** в зависимости от того, может ли свойство иметь значение null.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-197">**True** (the default value) or **False** depending on whether the property can have a null value.</span></span> <br/> [!NOTE]                                                                                                                 |
| <span data-ttu-id="5fdd5-198">> в CSDL v1, свойство сложного типа должно иметь значение `Nullable="False"` .</span><span class="sxs-lookup"><span data-stu-id="5fdd5-198">> In the CSDL v1, a complex type property must have `Nullable="False"`.</span></span> |             |                                                                                                                                                                                                                                  |
| <span data-ttu-id="5fdd5-199">**DefaultValue**</span><span class="sxs-lookup"><span data-stu-id="5fdd5-199">**DefaultValue**</span></span>                                                        | <span data-ttu-id="5fdd5-200">Нет</span><span class="sxs-lookup"><span data-stu-id="5fdd5-200">No</span></span>          | <span data-ttu-id="5fdd5-201">Значение по умолчанию для свойства.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-201">The default value of the property.</span></span>                                                                                                                                                                                               |
| <span data-ttu-id="5fdd5-202">**MaxLength**</span><span class="sxs-lookup"><span data-stu-id="5fdd5-202">**MaxLength**</span></span>                                                           | <span data-ttu-id="5fdd5-203">Нет</span><span class="sxs-lookup"><span data-stu-id="5fdd5-203">No</span></span>          | <span data-ttu-id="5fdd5-204">Максимальная длина значения свойства.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-204">The maximum length of the property value.</span></span>                                                                                                                                                                                        |
| <span data-ttu-id="5fdd5-205">**FixedLength**</span><span class="sxs-lookup"><span data-stu-id="5fdd5-205">**FixedLength**</span></span>                                                         | <span data-ttu-id="5fdd5-206">Нет</span><span class="sxs-lookup"><span data-stu-id="5fdd5-206">No</span></span>          | <span data-ttu-id="5fdd5-207">Значение **true** или **false** в зависимости от того, будет ли значение свойства храниться как строка фиксированной длины.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-207">**True** or **False** depending on whether the property value will be stored as a fixed length string.</span></span>                                                                                                                           |
| <span data-ttu-id="5fdd5-208">**Точность**</span><span class="sxs-lookup"><span data-stu-id="5fdd5-208">**Precision**</span></span>                                                           | <span data-ttu-id="5fdd5-209">Нет</span><span class="sxs-lookup"><span data-stu-id="5fdd5-209">No</span></span>          | <span data-ttu-id="5fdd5-210">Точность значения свойства.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-210">The precision of the property value.</span></span>                                                                                                                                                                                             |
| <span data-ttu-id="5fdd5-211">**Масштабирование**</span><span class="sxs-lookup"><span data-stu-id="5fdd5-211">**Scale**</span></span>                                                               | <span data-ttu-id="5fdd5-212">Нет</span><span class="sxs-lookup"><span data-stu-id="5fdd5-212">No</span></span>          | <span data-ttu-id="5fdd5-213">Масштаб значения свойства.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-213">The scale of the property value.</span></span>                                                                                                                                                                                                 |
| <span data-ttu-id="5fdd5-214">**SRID**</span><span class="sxs-lookup"><span data-stu-id="5fdd5-214">**SRID**</span></span>                                                                | <span data-ttu-id="5fdd5-215">Нет</span><span class="sxs-lookup"><span data-stu-id="5fdd5-215">No</span></span>          | <span data-ttu-id="5fdd5-216">Идентификатор пространственной системной ссылки.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-216">Spatial System Reference Identifier.</span></span> <span data-ttu-id="5fdd5-217">Допустимо только для свойств пространственных типов.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-217">Valid only for properties of spatial types.</span></span><span data-ttu-id="5fdd5-218">Дополнительные сведения см. в разделе [SRID](https://en.wikipedia.org/wiki/SRID) and [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx) .</span><span class="sxs-lookup"><span data-stu-id="5fdd5-218">   For more information, see [SRID](https://en.wikipedia.org/wiki/SRID) and [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx)</span></span> |
| <span data-ttu-id="5fdd5-219">**Юникод**</span><span class="sxs-lookup"><span data-stu-id="5fdd5-219">**Unicode**</span></span>                                                             | <span data-ttu-id="5fdd5-220">Нет</span><span class="sxs-lookup"><span data-stu-id="5fdd5-220">No</span></span>          | <span data-ttu-id="5fdd5-221">Значение **true** или **false** в зависимости от того, будет ли значение свойства храниться как строка в Юникоде.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-221">**True** or **False** depending on whether the property value will be stored as a Unicode string.</span></span>                                                                                                                                |
| <span data-ttu-id="5fdd5-222">**Параметры сортировки**</span><span class="sxs-lookup"><span data-stu-id="5fdd5-222">**Collation**</span></span>                                                           | <span data-ttu-id="5fdd5-223">Нет</span><span class="sxs-lookup"><span data-stu-id="5fdd5-223">No</span></span>          | <span data-ttu-id="5fdd5-224">Строка, указывающая порядок сортировки, используемый в источнике данных.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-224">A string that specifies the collating sequence to be used in the data source.</span></span>                                                                                                                                                    |

 

> [!NOTE]
> <span data-ttu-id="5fdd5-225">К элементу **CollectionType** можно применить любое количество атрибутов аннотации (настраиваемые XML-атрибуты).</span><span class="sxs-lookup"><span data-stu-id="5fdd5-225">Any number of annotation attributes (custom XML attributes) may be applied to the **CollectionType** element.</span></span> <span data-ttu-id="5fdd5-226">Однако настраиваемые атрибуты не могут принадлежать к любому пространству имен XML, зарезервированному для CSDL.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-226">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="5fdd5-227">Полные имена любых двух настраиваемых атрибутов не могут совпадать.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-227">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="5fdd5-228">Пример</span><span class="sxs-lookup"><span data-stu-id="5fdd5-228">Example</span></span>

<span data-ttu-id="5fdd5-229">В следующем примере показана определяемая моделью функция, которая использует элемент **CollectionType** для указания того, что функция возвращает коллекцию типов сущностей **Person** (как указано с атрибутом **ElementType** ).</span><span class="sxs-lookup"><span data-stu-id="5fdd5-229">The following example shows a model-defined function that that uses a **CollectionType** element to specify that the function returns a collection of **Person** entity types (as specified with the **ElementType** attribute).</span></span>

``` xml
 <Function Name="LastNamesAfter">
        <Parameter Name="someString" Type="Edm.String"/>
        <ReturnType>
             <CollectionType  ElementType="SchoolModel.Person"/>
        </ReturnType>
        <DefiningExpression>
             SELECT VALUE p
             FROM SchoolEntities.People AS p
             WHERE p.LastName >= someString
        </DefiningExpression>
 </Function>
```
 

<span data-ttu-id="5fdd5-230">В следующем примере показана определяемая моделью функция, которая использует элемент **CollectionType** для указания того, что функция возвращает коллекцию строк (как указано в элементе **RowType** ).</span><span class="sxs-lookup"><span data-stu-id="5fdd5-230">The following example shows a model-defined function that uses a **CollectionType** element to specify that the function returns a collection of rows (as specified in the **RowType** element).</span></span>

``` xml
 <Function Name="LastNamesAfter">
   <Parameter Name="someString" Type="Edm.String" />
   <ReturnType>
    <CollectionType>
      <RowType>
        <Property Name="FirstName" Type="Edm.String" Nullable="false" />
        <Property Name="LastName" Type="Edm.String" Nullable="false" />
      </RowType>
    </CollectionType>
   </ReturnType>
   <DefiningExpression>
             SELECT VALUE ROW(p.FirstName, p.LastName)
             FROM SchoolEntities.People AS p
             WHERE p.LastName &gt;= somestring
   </DefiningExpression>
 </Function>
```
 

<span data-ttu-id="5fdd5-231">В следующем примере показана определяемая моделью функция, которая использует элемент **CollectionType** для указания того, что функция принимает в качестве параметра коллекцию типов сущностей **отдела** .</span><span class="sxs-lookup"><span data-stu-id="5fdd5-231">The following example shows a model-defined function that uses the **CollectionType** element to specify that the function accepts as a parameter a collection of **Department** entity types.</span></span>

``` xml
 <Function Name="GetAvgBudget">
      <Parameter Name="Departments">
          <CollectionType>
             <TypeRef Type="SchoolModel.Department"/>
          </CollectionType>
           </Parameter>
       <ReturnType Type="Collection(Edm.Decimal)"/>
       <DefiningExpression>
             SELECT VALUE AVG(d.Budget) FROM Departments AS d
       </DefiningExpression>
 </Function>
```
 

 

## <a name="complextype-element-csdl"></a><span data-ttu-id="5fdd5-232">Элемент ComplexType (CSDL)</span><span class="sxs-lookup"><span data-stu-id="5fdd5-232">ComplexType Element (CSDL)</span></span>

<span data-ttu-id="5fdd5-233">Элемент **complexType** определяет структуру данных, состоящую из свойств **EDMSimpleType** или других сложных типов.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-233">A **ComplexType** element defines a data structure composed of **EdmSimpleType** properties or other complex types.</span></span><span data-ttu-id="5fdd5-234">Сложный тип может быть свойством типа сущности или другого сложного типа.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-234">  A complex type can be a property of an entity type or another complex type.</span></span> <span data-ttu-id="5fdd5-235">Сложный тип аналогичен типу сущности, поскольку также определяет данные.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-235">A complex type is similar to an entity type in that a complex type defines data.</span></span> <span data-ttu-id="5fdd5-236">Однако между сложными типами и типами сущности существуют некоторые ключевые различия.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-236">However, there are some key differences between complex types and entity types:</span></span>

-   <span data-ttu-id="5fdd5-237">Сложные типы не имеют идентификаторов (или ключей) и поэтому не могут существовать независимо.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-237">Complex types do not have identities (or keys) and therefore cannot exist independently.</span></span> <span data-ttu-id="5fdd5-238">Сложные типы могут существовать только как свойства типов сущностей или других сложных типов.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-238">Complex types can only exist as properties of entity types or other complex types.</span></span>
-   <span data-ttu-id="5fdd5-239">Сложные типы не могут участвовать в сопоставлениях.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-239">Complex types cannot participate in associations.</span></span> <span data-ttu-id="5fdd5-240">Ни один элемент ассоциации не может быть сложным типом. Поэтому для сложных типов не могут быть определены свойства навигации.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-240">Neither end of an association can be a complex type, and therefore navigation properties cannot be defined for complex types.</span></span>
-   <span data-ttu-id="5fdd5-241">Свойство сложного типа не может иметь значение null, хотя каждое скалярное свойство сложного типа может быть установлено в это значение.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-241">A complex type property cannot have a null value, though the scalar properties of a complex type may each be set to null.</span></span>

<span data-ttu-id="5fdd5-242">Элемент **complexType** может иметь следующие дочерние элементы (в указанном порядке):</span><span class="sxs-lookup"><span data-stu-id="5fdd5-242">A **ComplexType** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="5fdd5-243">Documentation (ноль или один элемент)</span><span class="sxs-lookup"><span data-stu-id="5fdd5-243">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="5fdd5-244">Property (ноль или больше элементов)</span><span class="sxs-lookup"><span data-stu-id="5fdd5-244">Property (zero or more elements)</span></span>
-   <span data-ttu-id="5fdd5-245">Элементы Annotation (ноль или более элементов)</span><span class="sxs-lookup"><span data-stu-id="5fdd5-245">Annotation elements (zero or more elements)</span></span>

<span data-ttu-id="5fdd5-246">В следующей таблице описаны атрибуты, которые можно применить к элементу **complexType** .</span><span class="sxs-lookup"><span data-stu-id="5fdd5-246">The table below describes the attributes that can be applied to the **ComplexType** element.</span></span>

| <span data-ttu-id="5fdd5-247">Имя атрибута</span><span class="sxs-lookup"><span data-stu-id="5fdd5-247">Attribute Name</span></span>                                                                                                 | <span data-ttu-id="5fdd5-248">Обязательный</span><span class="sxs-lookup"><span data-stu-id="5fdd5-248">Is Required</span></span> | <span data-ttu-id="5fdd5-249">Значение</span><span class="sxs-lookup"><span data-stu-id="5fdd5-249">Value</span></span>                                                                                                                                                                               |
|:---------------------------------------------------------------------------------------------------------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="5fdd5-250">Имя</span><span class="sxs-lookup"><span data-stu-id="5fdd5-250">Name</span></span>                                                                                                           | <span data-ttu-id="5fdd5-251">Да</span><span class="sxs-lookup"><span data-stu-id="5fdd5-251">Yes</span></span>         | <span data-ttu-id="5fdd5-252">Имя сложного типа.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-252">The name of the complex type.</span></span> <span data-ttu-id="5fdd5-253">Имя сложного типа не может совпадать с именем другого сложного типа, типа сущности или сопоставления, которые находятся в области модели.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-253">The name of a complex type cannot be the same as the name of another complex type, entity type, or association that is within the scope of the model.</span></span> |
| <span data-ttu-id="5fdd5-254">BaseType</span><span class="sxs-lookup"><span data-stu-id="5fdd5-254">BaseType</span></span>                                                                                                       | <span data-ttu-id="5fdd5-255">Нет</span><span class="sxs-lookup"><span data-stu-id="5fdd5-255">No</span></span>          | <span data-ttu-id="5fdd5-256">Имя другого сложного типа, который является базовым типом определяемого сложного типа.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-256">The name of another complex type that is the base type of the complex type that is being defined.</span></span> <br/> [!NOTE]                                                                     |
| <span data-ttu-id="5fdd5-257">> этот атрибут неприменим в CSDL v1.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-257">> This attribute is not applicable in CSDL v1.</span></span> <span data-ttu-id="5fdd5-258">В этой версии не поддерживается наследование для сложных типов.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-258">Inheritance for complex types is not supported in that version.</span></span> |             |                                                                                                                                                                                     |
| <span data-ttu-id="5fdd5-259">Аннотация</span><span class="sxs-lookup"><span data-stu-id="5fdd5-259">Abstract</span></span>                                                                                                       | <span data-ttu-id="5fdd5-260">Нет</span><span class="sxs-lookup"><span data-stu-id="5fdd5-260">No</span></span>          | <span data-ttu-id="5fdd5-261">**True** или **false** (значение по умолчанию) в зависимости от того, является ли сложный тип абстрактным типом.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-261">**True** or **False** (the default value) depending on whether the complex type is an abstract type.</span></span> <br/> [!NOTE]                                                                  |
| <span data-ttu-id="5fdd5-262">> этот атрибут неприменим в CSDL v1.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-262">> This attribute is not applicable in CSDL v1.</span></span> <span data-ttu-id="5fdd5-263">Сложные типы в этой версии не могут быть абстрактными типами.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-263">Complex types in that version cannot be abstract types.</span></span>         |             |                                                                                                                                                                                     |

 

> [!NOTE]
> <span data-ttu-id="5fdd5-264">К элементу **complexType** можно применить любое количество атрибутов аннотации (настраиваемые XML-атрибуты).</span><span class="sxs-lookup"><span data-stu-id="5fdd5-264">Any number of annotation attributes (custom XML attributes) may be applied to the **ComplexType** element.</span></span> <span data-ttu-id="5fdd5-265">Однако настраиваемые атрибуты не могут принадлежать к любому пространству имен XML, зарезервированному для CSDL.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-265">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="5fdd5-266">Полные имена любых двух настраиваемых атрибутов не могут совпадать.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-266">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="5fdd5-267">Пример</span><span class="sxs-lookup"><span data-stu-id="5fdd5-267">Example</span></span>

<span data-ttu-id="5fdd5-268">В следующем примере показан сложный тип **Address**с **EDMSimpleType** свойствами **streetAddress**, **City**, **StateOrProvince**, **Country**и **PostalCode**.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-268">The following example shows a complex type, **Address**, with the **EdmSimpleType** properties **StreetAddress**, **City**, **StateOrProvince**, **Country**, and **PostalCode**.</span></span>

``` xml
 <ComplexType Name="Address" >
   <Property Type="String" Name="StreetAddress" Nullable="false" />
   <Property Type="String" Name="City" Nullable="false" />
   <Property Type="String" Name="StateOrProvince" Nullable="false" />
   <Property Type="String" Name="Country" Nullable="false" />
   <Property Type="String" Name="PostalCode" Nullable="false" />
 </ComplexType>
```
 

<span data-ttu-id="5fdd5-269">Чтобы определить **адрес** сложного типа (выше) в качестве свойства типа сущности, необходимо объявить тип свойства в определении типа сущности.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-269">To define the complex type **Address** (above) as a property of an entity type, you must declare the property type in the entity type definition.</span></span> <span data-ttu-id="5fdd5-270">В следующем примере показано свойство **Address** в виде сложного типа для типа сущности (**Издатель**):</span><span class="sxs-lookup"><span data-stu-id="5fdd5-270">The following example shows the **Address** property as a complex type on an entity type (**Publisher**):</span></span>

``` xml
 <EntityType Name="Publisher">
       <Key>
         <PropertyRef Name="Id" />
       </Key>
       <Property Type="Int32" Name="Id" Nullable="false" />
       <Property Type="String" Name="Name" Nullable="false" />
       <Property Type="BooksModel.Address" Name="Address" Nullable="false" />
       <NavigationProperty Name="Books" Relationship="BooksModel.PublishedBy"
                           FromRole="Publisher" ToRole="Book" />
     </EntityType>
```
 

 

## <a name="definingexpression-element-csdl"></a><span data-ttu-id="5fdd5-271">Элемент DefiningExpression (CSDL)</span><span class="sxs-lookup"><span data-stu-id="5fdd5-271">DefiningExpression Element (CSDL)</span></span>

<span data-ttu-id="5fdd5-272">Элемент **DefiningExpression** в языке CSDL содержит выражение Entity SQL, определяющее функцию в концептуальной модели.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-272">The **DefiningExpression** element in conceptual schema definition language (CSDL) contains an Entity SQL expression that defines a function in the conceptual model.</span></span>  

> [!NOTE]
> <span data-ttu-id="5fdd5-273">В целях проверки элемент **DefiningExpression** может содержать произвольное содержимое.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-273">For validation purposes, a **DefiningExpression** element can contain arbitrary content.</span></span> <span data-ttu-id="5fdd5-274">Однако Entity Framework выдаст исключение во время выполнения, если элемент **DefiningExpression** не содержит допустимых Entity SQL.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-274">However, Entity Framework will throw an exception at runtime if a **DefiningExpression** element does not contain valid Entity SQL.</span></span>

 

### <a name="applicable-attributes"></a><span data-ttu-id="5fdd5-275">Применимые атрибуты</span><span class="sxs-lookup"><span data-stu-id="5fdd5-275">Applicable Attributes</span></span>

<span data-ttu-id="5fdd5-276">К элементу **DefiningExpression** можно применить любое количество атрибутов аннотации (настраиваемые XML-атрибуты).</span><span class="sxs-lookup"><span data-stu-id="5fdd5-276">Any number of annotation attributes (custom XML attributes) may be applied to the **DefiningExpression** element.</span></span> <span data-ttu-id="5fdd5-277">Однако настраиваемые атрибуты не могут принадлежать к любому пространству имен XML, зарезервированному для CSDL.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-277">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="5fdd5-278">Полные имена любых двух настраиваемых атрибутов не могут совпадать.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-278">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="5fdd5-279">Пример</span><span class="sxs-lookup"><span data-stu-id="5fdd5-279">Example</span></span>

<span data-ttu-id="5fdd5-280">В следующем примере используется элемент **DefiningExpression** для определения функции, возвращающей число лет с момента публикации книги.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-280">The following example uses a **DefiningExpression** element to define a function that returns the number of years since a book was published.</span></span> <span data-ttu-id="5fdd5-281">Содержимое элемента **DefiningExpression** записывается на Entity SQL.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-281">The content of the **DefiningExpression** element is written in Entity SQL.</span></span>

``` xml
 <Function Name="GetYearsInPrint" ReturnType="Edm.Int32" >
       <Parameter Name="book" Type="BooksModel.Book" />
       <DefiningExpression>
         Year(CurrentDateTime()) - Year(cast(book.PublishedDate as DateTime))
       </DefiningExpression>
     </Function>
```
 

 

## <a name="dependent-element-csdl"></a><span data-ttu-id="5fdd5-282">Элемент Dependent (CSDL)</span><span class="sxs-lookup"><span data-stu-id="5fdd5-282">Dependent Element (CSDL)</span></span>

<span data-ttu-id="5fdd5-283">**Зависимый** элемент в языке CSDL является дочерним элементом для элемента ReferentialConstraint и определяет зависимый конец ссылочного ограничения.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-283">The **Dependent** element in conceptual schema definition language (CSDL) is a child element to the ReferentialConstraint element and defines the dependent end of a referential constraint.</span></span> <span data-ttu-id="5fdd5-284">Элемент **ReferentialConstraint** определяет функциональность, похожую на ограничение ссылочной целостности в реляционной базе данных.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-284">A **ReferentialConstraint** element defines functionality that is similar to a referential integrity constraint in a relational database.</span></span> <span data-ttu-id="5fdd5-285">Свойство (или свойства) типа сущности могут ссылаться на ключ сущности в другом типе сущности также, как столбец (или столбцы) в таблице базы данных могут ссылаться на первичный ключ другой таблицы.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-285">In the same way that a column (or columns) from a database table can reference the primary key of another table, a property (or properties) of an entity type can reference the entity key of another entity type.</span></span> <span data-ttu-id="5fdd5-286">Тип сущности, на который указывает ссылка, называется *основным элементом* ограничения.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-286">The entity type that is referenced is called the *principal end* of the constraint.</span></span> <span data-ttu-id="5fdd5-287">Тип сущности, который ссылается на основной элемент, называется *зависимым окончанием* ограничения.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-287">The entity type that references the principal end is called the *dependent end* of the constraint.</span></span> <span data-ttu-id="5fdd5-288">Элементы **PropertyRef** используются для указания ключей, которые ссылаются на основной элемент.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-288">**PropertyRef** elements are used to specify which keys reference the principal end.</span></span>

<span data-ttu-id="5fdd5-289">**Зависимый** элемент может иметь следующие дочерние элементы (в указанном порядке):</span><span class="sxs-lookup"><span data-stu-id="5fdd5-289">The **Dependent** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="5fdd5-290">PropertyRef (один или несколько элементов)</span><span class="sxs-lookup"><span data-stu-id="5fdd5-290">PropertyRef (one or more elements)</span></span>
-   <span data-ttu-id="5fdd5-291">Элементы Annotation (ноль или более элементов)</span><span class="sxs-lookup"><span data-stu-id="5fdd5-291">Annotation elements (zero or more elements)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="5fdd5-292">Применимые атрибуты</span><span class="sxs-lookup"><span data-stu-id="5fdd5-292">Applicable Attributes</span></span>

<span data-ttu-id="5fdd5-293">В следующей таблице описаны атрибуты, которые можно применить к **зависимому** элементу.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-293">The table below describes the attributes that can be applied to the **Dependent** element.</span></span>

| <span data-ttu-id="5fdd5-294">Имя атрибута</span><span class="sxs-lookup"><span data-stu-id="5fdd5-294">Attribute Name</span></span> | <span data-ttu-id="5fdd5-295">Обязательный</span><span class="sxs-lookup"><span data-stu-id="5fdd5-295">Is Required</span></span> | <span data-ttu-id="5fdd5-296">Значение</span><span class="sxs-lookup"><span data-stu-id="5fdd5-296">Value</span></span>                                                                |
|:---------------|:------------|:---------------------------------------------------------------------|
| <span data-ttu-id="5fdd5-297">**Роль**</span><span class="sxs-lookup"><span data-stu-id="5fdd5-297">**Role**</span></span>       | <span data-ttu-id="5fdd5-298">Да</span><span class="sxs-lookup"><span data-stu-id="5fdd5-298">Yes</span></span>         | <span data-ttu-id="5fdd5-299">Имя типа сущности в зависимом элементе ассоциации.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-299">The name of the entity type on the dependent end of the association.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="5fdd5-300">К **зависимому** элементу можно применить любое количество атрибутов аннотации (настраиваемые XML-атрибуты).</span><span class="sxs-lookup"><span data-stu-id="5fdd5-300">Any number of annotation attributes (custom XML attributes) may be applied to the **Dependent** element.</span></span> <span data-ttu-id="5fdd5-301">Однако настраиваемые атрибуты не могут принадлежать к любому пространству имен XML, зарезервированному для CSDL.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-301">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="5fdd5-302">Полные имена любых двух настраиваемых атрибутов не могут совпадать.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-302">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="5fdd5-303">Пример</span><span class="sxs-lookup"><span data-stu-id="5fdd5-303">Example</span></span>

<span data-ttu-id="5fdd5-304">В следующем примере показан элемент **ReferentialConstraint** , используемый как часть определения ассоциации **публишедби** .</span><span class="sxs-lookup"><span data-stu-id="5fdd5-304">The following example shows a **ReferentialConstraint** element being used as part of the definition of the **PublishedBy** association.</span></span> <span data-ttu-id="5fdd5-305">Свойство **PublisherID** типа сущности **Book** образует зависимый конец справочного ограничения.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-305">The **PublisherId** property of the **Book** entity type makes up the dependent end of the referential constraint.</span></span>

``` xml
 <Association Name="PublishedBy">
   <End Type="BooksModel.Book" Role="Book" Multiplicity="*" >
   </End>
   <End Type="BooksModel.Publisher" Role="Publisher" Multiplicity="1" />
   <ReferentialConstraint>
     <Principal Role="Publisher">
       <PropertyRef Name="Id" />
     </Principal>
     <Dependent Role="Book">
       <PropertyRef Name="PublisherId" />
     </Dependent>
   </ReferentialConstraint>
 </Association>
```
 

 

## <a name="documentation-element-csdl"></a><span data-ttu-id="5fdd5-306">Элемент Documentation (CSDL)</span><span class="sxs-lookup"><span data-stu-id="5fdd5-306">Documentation Element (CSDL)</span></span>

<span data-ttu-id="5fdd5-307">Элемент **Documentation** в языке CSDL может использоваться для предоставления сведений об объекте, определенном в родительском элементе.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-307">The **Documentation** element in conceptual schema definition language (CSDL) can be used to provide information about an object that is defined in a parent element.</span></span> <span data-ttu-id="5fdd5-308">В EDMX-файле, если элемент **Documentation** является дочерним элементом элемента, который отображается как объект в области конструктора EF Designer (например, сущность, Ассоциация или свойство), содержимое элемента **Documentation** будет отображаться в окне **свойств** Visual Studio для объекта.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-308">In an .edmx file, when the **Documentation** element is a child of an element that appears as an object on the design surface of the EF Designer  (such as an entity, association, or property), the contents of the **Documentation** element will appear in the Visual Studio **Properties** window for the object.</span></span>

<span data-ttu-id="5fdd5-309">Элемент **Documentation** может иметь следующие дочерние элементы (в указанном порядке):</span><span class="sxs-lookup"><span data-stu-id="5fdd5-309">The **Documentation** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="5fdd5-310">**Сводка**: краткое описание родительского элемента.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-310">**Summary**: A brief description of the parent element.</span></span> <span data-ttu-id="5fdd5-311">(ноль или один элемент)</span><span class="sxs-lookup"><span data-stu-id="5fdd5-311">(zero or one element)</span></span>
-   <span data-ttu-id="5fdd5-312">**Лонгдескриптион**: расширенное описание родительского элемента.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-312">**LongDescription**: An extensive description of the parent element.</span></span> <span data-ttu-id="5fdd5-313">(ноль или один элемент)</span><span class="sxs-lookup"><span data-stu-id="5fdd5-313">(zero or one element)</span></span>
-   <span data-ttu-id="5fdd5-314">Элементы Annotation.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-314">Annotation elements.</span></span> <span data-ttu-id="5fdd5-315">(ноль или больше элементов)</span><span class="sxs-lookup"><span data-stu-id="5fdd5-315">(zero or more elements)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="5fdd5-316">Применимые атрибуты</span><span class="sxs-lookup"><span data-stu-id="5fdd5-316">Applicable Attributes</span></span>

<span data-ttu-id="5fdd5-317">К элементу **Documentation** можно применить любое количество атрибутов аннотации (настраиваемые XML-атрибуты).</span><span class="sxs-lookup"><span data-stu-id="5fdd5-317">Any number of annotation attributes (custom XML attributes) may be applied to the **Documentation** element.</span></span> <span data-ttu-id="5fdd5-318">Однако настраиваемые атрибуты не могут принадлежать к любому пространству имен XML, зарезервированному для CSDL.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-318">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="5fdd5-319">Полные имена любых двух настраиваемых атрибутов не могут совпадать.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-319">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="5fdd5-320">Пример</span><span class="sxs-lookup"><span data-stu-id="5fdd5-320">Example</span></span>

<span data-ttu-id="5fdd5-321">В следующем примере показан элемент **Documentation** в качестве дочернего элемента элемента EntityType.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-321">The following example shows the **Documentation** element as a child element of an EntityType element.</span></span> <span data-ttu-id="5fdd5-322">Если приведенный ниже фрагмент кода был в CSDL-файле EDMX-файла, содержимое элементов **Summary** и **лонгдескриптион** будет отображаться в окне **свойств** Visual Studio при выборе `Customer` типа сущности.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-322">If the snippet below were in the CSDL content of an .edmx file, the contents of the **Summary** and **LongDescription** elements would appear in the Visual Studio **Properties** window when you click on the `Customer` entity type.</span></span>

``` xml
 <EntityType Name="Customer">
    <Documentation>
      <Summary>Summary here.</Summary>
      <LongDescription>Long description here.</LongDescription>
    </Documentation>
    <Key>
      <PropertyRef Name="CustomerId" />
    </Key>
    <Property Type="Int32" Name="CustomerId" Nullable="false" />
    <Property Type="String" Name="Name" Nullable="false" />
 </EntityType>
```
 

 

## <a name="end-element-csdl"></a><span data-ttu-id="5fdd5-323">Элемент End (CSDL)</span><span class="sxs-lookup"><span data-stu-id="5fdd5-323">End Element (CSDL)</span></span>

<span data-ttu-id="5fdd5-324">Элемент **End** в языке CSDL может быть дочерним по отношению к элементу Association или элементу AssociationSet.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-324">The **End** element in conceptual schema definition language (CSDL) can be a child of the Association element or the AssociationSet element.</span></span> <span data-ttu-id="5fdd5-325">В каждом случае роль **конечного** элемента отличается, и применимые атрибуты различаются.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-325">In each case, the role of the **End** element is different and the applicable attributes are different.</span></span>

### <a name="end-element-as-a-child-of-the-association-element"></a><span data-ttu-id="5fdd5-326">Элемент End как дочерний по отношению к элементу Association</span><span class="sxs-lookup"><span data-stu-id="5fdd5-326">End Element as a Child of the Association Element</span></span>

<span data-ttu-id="5fdd5-327">Элемент **End** (в качестве дочернего элемента **ассоциации** ) определяет тип сущности на одном конце ассоциации и число экземпляров типа сущности, которые могут существовать в этом конце ассоциации.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-327">An **End** element (as a child of the **Association** element) identifies the entity type on one end of an association and the number of entity type instances that can exist at that end of an association.</span></span> <span data-ttu-id="5fdd5-328">Элементы ассоциации определяются при определении ассоциации; ассоциация должна иметь два элемента.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-328">Association ends are defined as part of an association; an association must have exactly two association ends.</span></span> <span data-ttu-id="5fdd5-329">К экземплярам типов сущности на одном элементе ассоциации можно осуществлять доступ с помощью свойств навигации или внешних ключей при условии, что они были предоставлены в типах сущности.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-329">Entity type instances at one end of an association can be accessed through navigation properties or foreign keys if they are exposed on an entity type.</span></span>  

<span data-ttu-id="5fdd5-330">Элемент **End** может иметь следующие дочерние элементы (в указанном порядке):</span><span class="sxs-lookup"><span data-stu-id="5fdd5-330">An **End** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="5fdd5-331">Documentation (ноль или один элемент)</span><span class="sxs-lookup"><span data-stu-id="5fdd5-331">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="5fdd5-332">OnDelete (ноль или один элемент)</span><span class="sxs-lookup"><span data-stu-id="5fdd5-332">OnDelete (zero or one element)</span></span>
-   <span data-ttu-id="5fdd5-333">Элементы Annotation (ноль или более элементов)</span><span class="sxs-lookup"><span data-stu-id="5fdd5-333">Annotation elements (zero or more elements)</span></span>

#### <a name="applicable-attributes"></a><span data-ttu-id="5fdd5-334">Применимые атрибуты</span><span class="sxs-lookup"><span data-stu-id="5fdd5-334">Applicable Attributes</span></span>

<span data-ttu-id="5fdd5-335">В следующей таблице описаны атрибуты, которые можно применить к элементу **End** , если он является дочерним для элемента **Association** .</span><span class="sxs-lookup"><span data-stu-id="5fdd5-335">The following table describes the attributes that can be applied to the **End** element when it is the child of an **Association** element.</span></span>

| <span data-ttu-id="5fdd5-336">Имя атрибута</span><span class="sxs-lookup"><span data-stu-id="5fdd5-336">Attribute Name</span></span>   | <span data-ttu-id="5fdd5-337">Обязательный</span><span class="sxs-lookup"><span data-stu-id="5fdd5-337">Is Required</span></span> | <span data-ttu-id="5fdd5-338">Значение</span><span class="sxs-lookup"><span data-stu-id="5fdd5-338">Value</span></span>                                                                                                                                                                                                                                                                                                                                                                                                              |
|:-----------------|:------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="5fdd5-339">**Тип**</span><span class="sxs-lookup"><span data-stu-id="5fdd5-339">**Type**</span></span>         | <span data-ttu-id="5fdd5-340">Да</span><span class="sxs-lookup"><span data-stu-id="5fdd5-340">Yes</span></span>         | <span data-ttu-id="5fdd5-341">Имя типа сущности на одном элементе ассоциации.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-341">The name of the entity type at one end of the association.</span></span>                                                                                                                                                                                                                                                                                                                                                         |
| <span data-ttu-id="5fdd5-342">**Роль**</span><span class="sxs-lookup"><span data-stu-id="5fdd5-342">**Role**</span></span>         | <span data-ttu-id="5fdd5-343">Нет</span><span class="sxs-lookup"><span data-stu-id="5fdd5-343">No</span></span>          | <span data-ttu-id="5fdd5-344">Имя для элемента ассоциации.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-344">A name for the association end.</span></span> <span data-ttu-id="5fdd5-345">Если имя не было предоставлено, будет использовано имя типа сущности на элементе ассоциации.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-345">If no name is provided, the name of the entity type on the association end will be used.</span></span>                                                                                                                                                                                                                                                                                           |
| <span data-ttu-id="5fdd5-346">**Кратность**</span><span class="sxs-lookup"><span data-stu-id="5fdd5-346">**Multiplicity**</span></span> | <span data-ttu-id="5fdd5-347">Да</span><span class="sxs-lookup"><span data-stu-id="5fdd5-347">Yes</span></span>         | <span data-ttu-id="5fdd5-348">**1**, **0.. 1**или в **\*** зависимости от количества экземпляров типа сущности, которые могут находиться в конце ассоциации.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-348">**1**, **0..1**, or **\*** depending on the number of entity type instances that can be at the end of the association.</span></span> <br/> <span data-ttu-id="5fdd5-349">значение **1** указывает, что в конце ассоциации существует ровно один экземпляр типа сущности.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-349">**1** indicates that exactly one entity type instance exists at the association end.</span></span> <br/> <span data-ttu-id="5fdd5-350">**0.. 1** указывает, что в конце ассоциации существует ноль или один экземпляр типа сущности.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-350">**0..1** indicates that zero or one entity type instances exist at the association end.</span></span> <br/> <span data-ttu-id="5fdd5-351">**\*** Указывает, что в конце ассоциации существует ноль, один или несколько экземпляров типа сущности.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-351">**\*** indicates that zero, one, or more entity type instances exist at the association end.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="5fdd5-352">К **конечному** элементу можно применить любое количество атрибутов аннотации (настраиваемые XML-атрибуты).</span><span class="sxs-lookup"><span data-stu-id="5fdd5-352">Any number of annotation attributes (custom XML attributes) may be applied to the **End** element.</span></span> <span data-ttu-id="5fdd5-353">Однако настраиваемые атрибуты не могут принадлежать к любому пространству имен XML, зарезервированному для CSDL.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-353">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="5fdd5-354">Полные имена любых двух настраиваемых атрибутов не могут совпадать.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-354">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

#### <a name="example"></a><span data-ttu-id="5fdd5-355">Пример</span><span class="sxs-lookup"><span data-stu-id="5fdd5-355">Example</span></span>

<span data-ttu-id="5fdd5-356">В следующем примере показан элемент **Association** , определяющий ассоциацию **кустомерордерс** .</span><span class="sxs-lookup"><span data-stu-id="5fdd5-356">The following example shows an **Association** element that defines the **CustomerOrders** association.</span></span> <span data-ttu-id="5fdd5-357">Значения **кратности** для каждого **элемента** ассоциации указывают на то, что многие **заказы** могут быть связаны с **клиентом**, но только один **клиент** может быть связан с **заказом**.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-357">The **Multiplicity** values for each **End** of the association indicate that many **Orders** can be associated with a **Customer**, but only one **Customer** can be associated with an **Order**.</span></span> <span data-ttu-id="5fdd5-358">Кроме того, элемент **OnDelete** указывает, что все **заказы** , связанные с конкретным **клиентом** и которые были загружены в ObjectContext, будут удалены при удалении **клиента** .</span><span class="sxs-lookup"><span data-stu-id="5fdd5-358">Additionally, the **OnDelete** element indicates that all **Orders** that are related to a particular **Customer** and that have been loaded into the ObjectContext will be deleted if the **Customer** is deleted.</span></span>

``` xml
 <Association Name="CustomerOrders">
   <End Type="ExampleModel.Customer" Role="Customer" Multiplicity="1" />
   <End Type="ExampleModel.Order" Role="Order" Multiplicity="*">
         <OnDelete Action="Cascade" />
   </End>
 </Association>
```
 

### <a name="end-element-as-a-child-of-the-associationset-element"></a><span data-ttu-id="5fdd5-359">Элемент End как дочерний по отношению к элементу AssociationSet</span><span class="sxs-lookup"><span data-stu-id="5fdd5-359">End Element as a Child of the AssociationSet Element</span></span>

<span data-ttu-id="5fdd5-360">Элемент **End** указывает один конец набора ассоциаций.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-360">The **End** element specifies one end of an association set.</span></span> <span data-ttu-id="5fdd5-361">Элемент **AssociationSet** должен содержать два элемента **End** .</span><span class="sxs-lookup"><span data-stu-id="5fdd5-361">The **AssociationSet** element must contain two **End** elements.</span></span> <span data-ttu-id="5fdd5-362">Сведения, содержащиеся в элементе **End** , используются при сопоставлении набора ассоциаций с источником данных.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-362">The information contained in an **End** element is used in mapping an association set to a data source.</span></span>

<span data-ttu-id="5fdd5-363">Элемент **End** может иметь следующие дочерние элементы (в указанном порядке):</span><span class="sxs-lookup"><span data-stu-id="5fdd5-363">An **End** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="5fdd5-364">Documentation (ноль или один элемент)</span><span class="sxs-lookup"><span data-stu-id="5fdd5-364">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="5fdd5-365">Элементы Annotation (ноль или более элементов)</span><span class="sxs-lookup"><span data-stu-id="5fdd5-365">Annotation elements (zero or more elements)</span></span>

> [!NOTE]
> <span data-ttu-id="5fdd5-366">Элементы Annotation должны следовать после всех остальных дочерних элементов.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-366">Annotation elements must appear after all other child elements.</span></span> <span data-ttu-id="5fdd5-367">Элементы аннотации разрешены только в CSDL v2 и более поздних версиях.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-367">Annotation elements are only allowed in CSDL v2 and later.</span></span>

 

#### <a name="applicable-attributes"></a><span data-ttu-id="5fdd5-368">Применимые атрибуты</span><span class="sxs-lookup"><span data-stu-id="5fdd5-368">Applicable Attributes</span></span>

<span data-ttu-id="5fdd5-369">В следующей таблице описаны атрибуты, которые можно применить к элементу **End** , если он является дочерним для элемента **AssociationSet** .</span><span class="sxs-lookup"><span data-stu-id="5fdd5-369">The following table describes the attributes that can be applied to the **End** element when it is the child of an **AssociationSet** element.</span></span>

| <span data-ttu-id="5fdd5-370">Имя атрибута</span><span class="sxs-lookup"><span data-stu-id="5fdd5-370">Attribute Name</span></span> | <span data-ttu-id="5fdd5-371">Обязательный</span><span class="sxs-lookup"><span data-stu-id="5fdd5-371">Is Required</span></span> | <span data-ttu-id="5fdd5-372">Значение</span><span class="sxs-lookup"><span data-stu-id="5fdd5-372">Value</span></span>                                                                                                                                                                                                                 |
|:---------------|:------------|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="5fdd5-373">**EntitySet**</span><span class="sxs-lookup"><span data-stu-id="5fdd5-373">**EntitySet**</span></span>  | <span data-ttu-id="5fdd5-374">Да</span><span class="sxs-lookup"><span data-stu-id="5fdd5-374">Yes</span></span>         | <span data-ttu-id="5fdd5-375">Имя элемента **EntitySet** , определяющего один конец родительского элемента **AssociationSet** .</span><span class="sxs-lookup"><span data-stu-id="5fdd5-375">The name of the **EntitySet** element that defines one end of the parent **AssociationSet** element.</span></span> <span data-ttu-id="5fdd5-376">Элемент **EntitySet** должен быть определен в том же контейнере сущностей, что и родительский элемент **AssociationSet** .</span><span class="sxs-lookup"><span data-stu-id="5fdd5-376">The **EntitySet** element must be defined in the same entity container as the parent **AssociationSet** element.</span></span> |
| <span data-ttu-id="5fdd5-377">**Роль**</span><span class="sxs-lookup"><span data-stu-id="5fdd5-377">**Role**</span></span>       | <span data-ttu-id="5fdd5-378">Нет</span><span class="sxs-lookup"><span data-stu-id="5fdd5-378">No</span></span>          | <span data-ttu-id="5fdd5-379">Имя элемента набора ассоциаций.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-379">The name of the association set end.</span></span> <span data-ttu-id="5fdd5-380">Если атрибут **Role** не используется, имя конца набора ассоциаций будет именем набора сущностей.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-380">If the **Role** attribute is not used, the name of the association set end will be the name of the entity set.</span></span>                                                                   |

 

> [!NOTE]
> <span data-ttu-id="5fdd5-381">К **конечному** элементу можно применить любое количество атрибутов аннотации (настраиваемые XML-атрибуты).</span><span class="sxs-lookup"><span data-stu-id="5fdd5-381">Any number of annotation attributes (custom XML attributes) may be applied to the **End** element.</span></span> <span data-ttu-id="5fdd5-382">Однако настраиваемые атрибуты не могут принадлежать к любому пространству имен XML, зарезервированному для CSDL.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-382">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="5fdd5-383">Полные имена любых двух настраиваемых атрибутов не могут совпадать.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-383">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

#### <a name="example"></a><span data-ttu-id="5fdd5-384">Пример</span><span class="sxs-lookup"><span data-stu-id="5fdd5-384">Example</span></span>

<span data-ttu-id="5fdd5-385">В следующем примере показан элемент **EntityContainer** с двумя элементами **AssociationSet** , каждый из которых содержит два элемента **End** :</span><span class="sxs-lookup"><span data-stu-id="5fdd5-385">The following example shows an **EntityContainer** element with two **AssociationSet** elements, each with two **End** elements:</span></span>

``` xml
 <EntityContainer Name="BooksContainer" >
   <EntitySet Name="Books" EntityType="BooksModel.Book" />
   <EntitySet Name="Publishers" EntityType="BooksModel.Publisher" />
   <EntitySet Name="Authors" EntityType="BooksModel.Author" />
   <AssociationSet Name="PublishedBy" Association="BooksModel.PublishedBy">
     <End Role="Book" EntitySet="Books" />
     <End Role="Publisher" EntitySet="Publishers" />
   </AssociationSet>
   <AssociationSet Name="WrittenBy" Association="BooksModel.WrittenBy">
     <End Role="Book" EntitySet="Books" />
     <End Role="Author" EntitySet="Authors" />
   </AssociationSet>
 </EntityContainer>
```
 

 

## <a name="entitycontainer-element-csdl"></a><span data-ttu-id="5fdd5-386">Элемент EntityContainer (CSDL)</span><span class="sxs-lookup"><span data-stu-id="5fdd5-386">EntityContainer Element (CSDL)</span></span>

<span data-ttu-id="5fdd5-387">Элемент **EntityContainer** в языке CSDL — это логический контейнер для наборов сущностей, наборов ассоциаций и импортов функций.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-387">The **EntityContainer** element in conceptual schema definition language (CSDL) is a logical container for entity sets, association sets, and function imports.</span></span> <span data-ttu-id="5fdd5-388">Контейнер сущностей концептуальной модели сопоставляется с контейнером сущностей режима хранения посредством элемента EntityContainerMapping.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-388">A conceptual model entity container maps to a storage model entity container through the EntityContainerMapping element.</span></span> <span data-ttu-id="5fdd5-389">Контейнер сущностей режима хранения описывает структуру базы данных: наборы сущностей описывают таблицы, наборы ассоциаций описывают ограничения внешних ключей, функции импорта описывают хранимые процедуры в базе данных.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-389">A storage model entity container describes the structure of the database: entity sets describe tables, association sets describe foreign key constraints, and function imports describe stored procedures in a database.</span></span>

<span data-ttu-id="5fdd5-390">Элемент **EntityContainer** может иметь один или несколько элементов документации.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-390">An **EntityContainer** element can have zero or one Documentation elements.</span></span> <span data-ttu-id="5fdd5-391">Если элемент **Documentation** присутствует, он должен предшествовать всем элементам **EntitySet**, **AssociationSet**и **FunctionImport** .</span><span class="sxs-lookup"><span data-stu-id="5fdd5-391">If a **Documentation** element is present, it must precede all **EntitySet**, **AssociationSet**, and **FunctionImport** elements.</span></span>

<span data-ttu-id="5fdd5-392">Элемент **EntityContainer** может иметь ноль или более следующих дочерних элементов (в указанном порядке):</span><span class="sxs-lookup"><span data-stu-id="5fdd5-392">An **EntityContainer** element can have zero or more of the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="5fdd5-393">EntitySet</span><span class="sxs-lookup"><span data-stu-id="5fdd5-393">EntitySet</span></span>
-   <span data-ttu-id="5fdd5-394">AssociationSet</span><span class="sxs-lookup"><span data-stu-id="5fdd5-394">AssociationSet</span></span>
-   <span data-ttu-id="5fdd5-395">FunctionImport</span><span class="sxs-lookup"><span data-stu-id="5fdd5-395">FunctionImport</span></span>
-   <span data-ttu-id="5fdd5-396">Элементы Annotation</span><span class="sxs-lookup"><span data-stu-id="5fdd5-396">Annotation elements</span></span>

<span data-ttu-id="5fdd5-397">Можно расширить элемент **EntityContainer** , включив в него содержимое другого **контейнера EntityContainer** , который находится в том же пространстве имен.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-397">You can extend an **EntityContainer** element to include the contents of another **EntityContainer** that is within the same namespace.</span></span> <span data-ttu-id="5fdd5-398">Чтобы включить содержимое другого **контейнера EntityContainer**, в ссылающемся элементе **EntityContainer** установите значение атрибута **extends** в имя элемента **EntityContainer** , который требуется включить.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-398">To include the contents of another **EntityContainer**, in the referencing **EntityContainer** element, set the value of the **Extends** attribute to the name of the **EntityContainer** element that you want to include.</span></span> <span data-ttu-id="5fdd5-399">Все дочерние элементы включаемого элемента **EntityContainer** будут рассматриваться как дочерние элементы ссылающегося элемента **EntityContainer** .</span><span class="sxs-lookup"><span data-stu-id="5fdd5-399">All child elements of the included **EntityContainer** element will be treated as child elements of the referencing **EntityContainer** element.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="5fdd5-400">Применимые атрибуты</span><span class="sxs-lookup"><span data-stu-id="5fdd5-400">Applicable Attributes</span></span>

<span data-ttu-id="5fdd5-401">В следующей таблице описаны атрибуты, которые можно применить к элементу **using** .</span><span class="sxs-lookup"><span data-stu-id="5fdd5-401">The table below describes the attributes that can be applied to the **Using** element.</span></span>

| <span data-ttu-id="5fdd5-402">Имя атрибута</span><span class="sxs-lookup"><span data-stu-id="5fdd5-402">Attribute Name</span></span> | <span data-ttu-id="5fdd5-403">Обязательный</span><span class="sxs-lookup"><span data-stu-id="5fdd5-403">Is Required</span></span> | <span data-ttu-id="5fdd5-404">Значение</span><span class="sxs-lookup"><span data-stu-id="5fdd5-404">Value</span></span>                                                           |
|:---------------|:------------|:----------------------------------------------------------------|
| <span data-ttu-id="5fdd5-405">**имя**;</span><span class="sxs-lookup"><span data-stu-id="5fdd5-405">**Name**</span></span>       | <span data-ttu-id="5fdd5-406">Да</span><span class="sxs-lookup"><span data-stu-id="5fdd5-406">Yes</span></span>         | <span data-ttu-id="5fdd5-407">Имя контейнера сущностей.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-407">The name of the entity container.</span></span>                               |
| <span data-ttu-id="5fdd5-408">**Предоставляет**</span><span class="sxs-lookup"><span data-stu-id="5fdd5-408">**Extends**</span></span>    | <span data-ttu-id="5fdd5-409">Нет</span><span class="sxs-lookup"><span data-stu-id="5fdd5-409">No</span></span>          | <span data-ttu-id="5fdd5-410">Имя другого контейнера сущностей в том же пространстве имен.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-410">The name of another entity container within the same namespace.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="5fdd5-411">К элементу **EntityContainer** можно применить любое количество атрибутов аннотации (настраиваемые XML-атрибуты).</span><span class="sxs-lookup"><span data-stu-id="5fdd5-411">Any number of annotation attributes (custom XML attributes) may be applied to the **EntityContainer** element.</span></span> <span data-ttu-id="5fdd5-412">Однако настраиваемые атрибуты не могут принадлежать к любому пространству имен XML, зарезервированному для CSDL.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-412">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="5fdd5-413">Полные имена любых двух настраиваемых атрибутов не могут совпадать.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-413">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="5fdd5-414">Пример</span><span class="sxs-lookup"><span data-stu-id="5fdd5-414">Example</span></span>

<span data-ttu-id="5fdd5-415">В следующем примере показан элемент **EntityContainer** , определяющий три набора сущностей и два набора ассоциаций.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-415">The following example shows an **EntityContainer** element that defines three entity sets and two association sets.</span></span>

``` xml
 <EntityContainer Name="BooksContainer" >
   <EntitySet Name="Books" EntityType="BooksModel.Book" />
   <EntitySet Name="Publishers" EntityType="BooksModel.Publisher" />
   <EntitySet Name="Authors" EntityType="BooksModel.Author" />
   <AssociationSet Name="PublishedBy" Association="BooksModel.PublishedBy">
     <End Role="Book" EntitySet="Books" />
     <End Role="Publisher" EntitySet="Publishers" />
   </AssociationSet>
   <AssociationSet Name="WrittenBy" Association="BooksModel.WrittenBy">
     <End Role="Book" EntitySet="Books" />
     <End Role="Author" EntitySet="Authors" />
   </AssociationSet>
 </EntityContainer>
```
 

 

## <a name="entityset-element-csdl"></a><span data-ttu-id="5fdd5-416">Элемент EntitySet (CSDL)</span><span class="sxs-lookup"><span data-stu-id="5fdd5-416">EntitySet Element (CSDL)</span></span>

<span data-ttu-id="5fdd5-417">Элемент **EntitySet** в языке определения концептуальной схемы — это логический контейнер для экземпляров типа сущности и экземпляров любого типа, производного от этого типа сущности.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-417">The **EntitySet** element in conceptual schema definition language is a logical container for instances of an entity type and instances of any type that is derived from that entity type.</span></span> <span data-ttu-id="5fdd5-418">Связь между типом сущности и набором сущностей аналогична связи между строкой и таблицей в реляционной базе данных.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-418">The relationship between an entity type and an entity set is analogous to the relationship between a row and a table in a relational database.</span></span> <span data-ttu-id="5fdd5-419">Тип сущности, как и строка, определяет ряд взаимосвязанных данных, а набор сущностей, как и таблица, содержит экземпляры этого определения.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-419">Like a row, an entity type defines a set of related data, and, like a table, an entity set contains instances of that definition.</span></span> <span data-ttu-id="5fdd5-420">Набор сущностей предоставляет конструкцию для группирования экземпляров типа сущности, чтобы их можно было сопоставлять со связанными структурами данных в источнике данных.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-420">An entity set provides a construct for grouping entity type instances so that they can be mapped to related data structures in a data source.</span></span>  

<span data-ttu-id="5fdd5-421">Можно определить больше одного набора сущностей для конкретного типа сущности.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-421">More than one entity set for a particular entity type may be defined.</span></span>

> [!NOTE]
> <span data-ttu-id="5fdd5-422">Конструктор EF не поддерживает концептуальные модели, содержащие несколько наборов сущностей для каждого типа.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-422">The EF Designer does not support conceptual models that contain multiple entity sets per type.</span></span>

 

<span data-ttu-id="5fdd5-423">Элемент **EntitySet** может иметь следующие дочерние элементы (в указанном порядке):</span><span class="sxs-lookup"><span data-stu-id="5fdd5-423">The **EntitySet** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="5fdd5-424">Элемент Documentation (допускается ровно один элемент либо элемент может отсутствовать)</span><span class="sxs-lookup"><span data-stu-id="5fdd5-424">Documentation Element (zero or one elements allowed)</span></span>
-   <span data-ttu-id="5fdd5-425">Элементы Annotation (допустимое количество элементов — ноль или больше)</span><span class="sxs-lookup"><span data-stu-id="5fdd5-425">Annotation elements (zero or more elements allowed)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="5fdd5-426">Применимые атрибуты</span><span class="sxs-lookup"><span data-stu-id="5fdd5-426">Applicable Attributes</span></span>

<span data-ttu-id="5fdd5-427">В следующей таблице описаны атрибуты, которые можно применить к элементу **EntitySet** .</span><span class="sxs-lookup"><span data-stu-id="5fdd5-427">The table below describes the attributes that can be applied to the **EntitySet** element.</span></span>

| <span data-ttu-id="5fdd5-428">Имя атрибута</span><span class="sxs-lookup"><span data-stu-id="5fdd5-428">Attribute Name</span></span> | <span data-ttu-id="5fdd5-429">Обязательный</span><span class="sxs-lookup"><span data-stu-id="5fdd5-429">Is Required</span></span> | <span data-ttu-id="5fdd5-430">Значение</span><span class="sxs-lookup"><span data-stu-id="5fdd5-430">Value</span></span>                                                                                    |
|:---------------|:------------|:-----------------------------------------------------------------------------------------|
| <span data-ttu-id="5fdd5-431">**имя**;</span><span class="sxs-lookup"><span data-stu-id="5fdd5-431">**Name**</span></span>       | <span data-ttu-id="5fdd5-432">Да</span><span class="sxs-lookup"><span data-stu-id="5fdd5-432">Yes</span></span>         | <span data-ttu-id="5fdd5-433">Имя набора сущностей.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-433">The name of the entity set.</span></span>                                                              |
| <span data-ttu-id="5fdd5-434">**EntityType**</span><span class="sxs-lookup"><span data-stu-id="5fdd5-434">**EntityType**</span></span> | <span data-ttu-id="5fdd5-435">Да</span><span class="sxs-lookup"><span data-stu-id="5fdd5-435">Yes</span></span>         | <span data-ttu-id="5fdd5-436">Полное имя типа сущности, для которого набор сущностей содержит экземпляры.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-436">The fully-qualified name of the entity type for which the entity set contains instances.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="5fdd5-437">К элементу **EntitySet** можно применить любое количество атрибутов аннотации (настраиваемые XML-атрибуты).</span><span class="sxs-lookup"><span data-stu-id="5fdd5-437">Any number of annotation attributes (custom XML attributes) may be applied to the **EntitySet** element.</span></span> <span data-ttu-id="5fdd5-438">Однако настраиваемые атрибуты не могут принадлежать к любому пространству имен XML, зарезервированному для CSDL.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-438">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="5fdd5-439">Полные имена любых двух настраиваемых атрибутов не могут совпадать.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-439">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="5fdd5-440">Пример</span><span class="sxs-lookup"><span data-stu-id="5fdd5-440">Example</span></span>

<span data-ttu-id="5fdd5-441">В следующем примере показан элемент **EntityContainer** с тремя элементами **EntitySet** :</span><span class="sxs-lookup"><span data-stu-id="5fdd5-441">The following example shows an **EntityContainer** element with three **EntitySet** elements:</span></span>

``` xml
 <EntityContainer Name="BooksContainer" >
   <EntitySet Name="Books" EntityType="BooksModel.Book" />
   <EntitySet Name="Publishers" EntityType="BooksModel.Publisher" />
   <EntitySet Name="Authors" EntityType="BooksModel.Author" />
   <AssociationSet Name="PublishedBy" Association="BooksModel.PublishedBy">
     <End Role="Book" EntitySet="Books" />
     <End Role="Publisher" EntitySet="Publishers" />
   </AssociationSet>
   <AssociationSet Name="WrittenBy" Association="BooksModel.WrittenBy">
     <End Role="Book" EntitySet="Books" />
     <End Role="Author" EntitySet="Authors" />
   </AssociationSet>
 </EntityContainer>
```
 

<span data-ttu-id="5fdd5-442">Предусмотрена возможность определять несколько наборов сущностей на тип (модель MEST).</span><span class="sxs-lookup"><span data-stu-id="5fdd5-442">It is possible to define multiple entity sets per type (MEST).</span></span> <span data-ttu-id="5fdd5-443">В следующем примере определяется контейнер сущностей с двумя наборами сущностей для типа сущности **Book** :</span><span class="sxs-lookup"><span data-stu-id="5fdd5-443">The following example defines an entity container with two entity sets for the **Book** entity type:</span></span>

``` xml
 <EntityContainer Name="BooksContainer" >
   <EntitySet Name="Books" EntityType="BooksModel.Book" />
   <EntitySet Name="FictionBooks" EntityType="BooksModel.Book" />
   <EntitySet Name="Publishers" EntityType="BooksModel.Publisher" />
   <EntitySet Name="Authors" EntityType="BooksModel.Author" />
   <AssociationSet Name="PublishedBy" Association="BooksModel.PublishedBy">
     <End Role="Book" EntitySet="Books" />
     <End Role="Publisher" EntitySet="Publishers" />
   </AssociationSet>
   <AssociationSet Name="BookAuthor" Association="BooksModel.BookAuthor">
     <End Role="Book" EntitySet="Books" />
     <End Role="Author" EntitySet="Authors" />
   </AssociationSet>
 </EntityContainer>
```
 

 

## <a name="entitytype-element-csdl"></a><span data-ttu-id="5fdd5-444">Элемент EntityType (CSDL)</span><span class="sxs-lookup"><span data-stu-id="5fdd5-444">EntityType Element (CSDL)</span></span>

<span data-ttu-id="5fdd5-445">Элемент **EntityType** представляет структуру концепции верхнего уровня, например Customer или Order, в концептуальной модели.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-445">The **EntityType** element represents the structure of a top-level concept, such as a customer or order, in a conceptual model.</span></span> <span data-ttu-id="5fdd5-446">Тип сущности — это шаблон для экземпляров типов сущностей в приложении.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-446">An entity type is a template for instances of entity types in an application.</span></span> <span data-ttu-id="5fdd5-447">Каждый шаблон содержит следующие сведения.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-447">Each template contains the following information:</span></span>

-   <span data-ttu-id="5fdd5-448">Уникальное имя.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-448">A unique name.</span></span> <span data-ttu-id="5fdd5-449">(Обязательно).</span><span class="sxs-lookup"><span data-stu-id="5fdd5-449">(Required.)</span></span>
-   <span data-ttu-id="5fdd5-450">Ключ сущности, определяемый одним или несколькими свойствами.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-450">An entity key that is defined by one or more properties.</span></span> <span data-ttu-id="5fdd5-451">(Обязательно).</span><span class="sxs-lookup"><span data-stu-id="5fdd5-451">(Required.)</span></span>
-   <span data-ttu-id="5fdd5-452">Свойства содержащихся данных.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-452">Properties for containing data.</span></span> <span data-ttu-id="5fdd5-453">(Необязательно.)</span><span class="sxs-lookup"><span data-stu-id="5fdd5-453">(Optional.)</span></span>
-   <span data-ttu-id="5fdd5-454">Свойства навигации, позволяющие осуществлять переход от одного элемента ассоциации к другому.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-454">Navigation properties that allow for navigation from one end of an association to the other end.</span></span> <span data-ttu-id="5fdd5-455">(Необязательно.)</span><span class="sxs-lookup"><span data-stu-id="5fdd5-455">(Optional.)</span></span>

<span data-ttu-id="5fdd5-456">В приложении экземпляр типа сущности представляет определенный объект (например, определенного клиента или заказ).</span><span class="sxs-lookup"><span data-stu-id="5fdd5-456">In an application, an instance of an entity type represents a specific object (such as a specific customer or order).</span></span> <span data-ttu-id="5fdd5-457">Каждый экземпляр типа сущности в наборе сущностей должен иметь уникальный ключ сущности.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-457">Each instance of an entity type must have a unique entity key within an entity set.</span></span>

<span data-ttu-id="5fdd5-458">Два экземпляра типа сущности считаются равными, только если они являются экземплярами одного типа и значения их ключей сущности равны.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-458">Two entity type instances are considered equal only if they are of the same type and the values of their entity keys are the same.</span></span>

<span data-ttu-id="5fdd5-459">Элемент **EntityType** может иметь следующие дочерние элементы (в указанном порядке):</span><span class="sxs-lookup"><span data-stu-id="5fdd5-459">An **EntityType** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="5fdd5-460">Documentation (ноль или один элемент)</span><span class="sxs-lookup"><span data-stu-id="5fdd5-460">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="5fdd5-461">Key (ноль или один элемент)</span><span class="sxs-lookup"><span data-stu-id="5fdd5-461">Key (zero or one element)</span></span>
-   <span data-ttu-id="5fdd5-462">Property (ноль или больше элементов)</span><span class="sxs-lookup"><span data-stu-id="5fdd5-462">Property (zero or more elements)</span></span>
-   <span data-ttu-id="5fdd5-463">NavigationProperty (допустимое количество элементов — ноль или больше)</span><span class="sxs-lookup"><span data-stu-id="5fdd5-463">NavigationProperty (zero or more elements)</span></span>
-   <span data-ttu-id="5fdd5-464">Элементы Annotation (ноль или более элементов)</span><span class="sxs-lookup"><span data-stu-id="5fdd5-464">Annotation elements (zero or more elements)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="5fdd5-465">Применимые атрибуты</span><span class="sxs-lookup"><span data-stu-id="5fdd5-465">Applicable Attributes</span></span>

<span data-ttu-id="5fdd5-466">В следующей таблице описаны атрибуты, которые можно применить к элементу **EntityType** .</span><span class="sxs-lookup"><span data-stu-id="5fdd5-466">The table below describes the attributes that can be applied to the **EntityType** element.</span></span>

| <span data-ttu-id="5fdd5-467">Имя атрибута</span><span class="sxs-lookup"><span data-stu-id="5fdd5-467">Attribute Name</span></span>                                                                                                                                  | <span data-ttu-id="5fdd5-468">Обязательный</span><span class="sxs-lookup"><span data-stu-id="5fdd5-468">Is Required</span></span> | <span data-ttu-id="5fdd5-469">Значение</span><span class="sxs-lookup"><span data-stu-id="5fdd5-469">Value</span></span>                                                                                            |
|:------------------------------------------------------------------------------------------------------------------------------------------------|:------------|:-------------------------------------------------------------------------------------------------|
| <span data-ttu-id="5fdd5-470">**имя**;</span><span class="sxs-lookup"><span data-stu-id="5fdd5-470">**Name**</span></span>                                                                                                                                        | <span data-ttu-id="5fdd5-471">Да</span><span class="sxs-lookup"><span data-stu-id="5fdd5-471">Yes</span></span>         | <span data-ttu-id="5fdd5-472">Имя типа сущности.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-472">The name of the entity type.</span></span>                                                                     |
| <span data-ttu-id="5fdd5-473">**BaseType**</span><span class="sxs-lookup"><span data-stu-id="5fdd5-473">**BaseType**</span></span>                                                                                                                                    | <span data-ttu-id="5fdd5-474">Нет</span><span class="sxs-lookup"><span data-stu-id="5fdd5-474">No</span></span>          | <span data-ttu-id="5fdd5-475">Имя другого типа сущности, который является базовым типом определяемого типа сущности.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-475">The name of another entity type that is the base type of the entity type that is being defined.</span></span>  |
| <span data-ttu-id="5fdd5-476">**Выделяет**</span><span class="sxs-lookup"><span data-stu-id="5fdd5-476">**Abstract**</span></span>                                                                                                                                    | <span data-ttu-id="5fdd5-477">Нет</span><span class="sxs-lookup"><span data-stu-id="5fdd5-477">No</span></span>          | <span data-ttu-id="5fdd5-478">**Значение true** или **false**в зависимости от того, является ли тип сущности абстрактным типом.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-478">**True** or **False**, depending on whether the entity type is an abstract type.</span></span>                 |
| <span data-ttu-id="5fdd5-479">**OpenType**</span><span class="sxs-lookup"><span data-stu-id="5fdd5-479">**OpenType**</span></span>                                                                                                                                    | <span data-ttu-id="5fdd5-480">Нет</span><span class="sxs-lookup"><span data-stu-id="5fdd5-480">No</span></span>          | <span data-ttu-id="5fdd5-481">**Значение true** или **false** в зависимости от того, является ли тип сущности открытым типом сущности.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-481">**True** or **False** depending on whether the entity type is an open entity type.</span></span> <br/> [!NOTE] |
| <span data-ttu-id="5fdd5-482">> атрибут **OpenType** применим только к типам сущностей, определенным в концептуальных моделях, которые используются со службами ADO.NET Data Services.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-482">> The **OpenType** attribute is only applicable to entity types that are defined in conceptual models that are used with ADO.NET Data Services.</span></span> |             |                                                                                                  |

 

> [!NOTE]
> <span data-ttu-id="5fdd5-483">К элементу **EntityType** можно применить любое количество атрибутов аннотации (настраиваемые XML-атрибуты).</span><span class="sxs-lookup"><span data-stu-id="5fdd5-483">Any number of annotation attributes (custom XML attributes) may be applied to the **EntityType** element.</span></span> <span data-ttu-id="5fdd5-484">Однако настраиваемые атрибуты не могут принадлежать к любому пространству имен XML, зарезервированному для CSDL.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-484">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="5fdd5-485">Полные имена любых двух настраиваемых атрибутов не могут совпадать.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-485">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="5fdd5-486">Пример</span><span class="sxs-lookup"><span data-stu-id="5fdd5-486">Example</span></span>

<span data-ttu-id="5fdd5-487">В следующем примере показан элемент **EntityType** с тремя элементами **свойств** и двумя элементами **NavigationProperty** :</span><span class="sxs-lookup"><span data-stu-id="5fdd5-487">The following example shows an **EntityType** element with three **Property** elements and two **NavigationProperty** elements:</span></span>

``` xml
 <EntityType Name="Book">
   <Key>
     <PropertyRef Name="ISBN" />
   </Key>
   <Property Type="String" Name="ISBN" Nullable="false" />
   <Property Type="String" Name="Title" Nullable="false" />
   <Property Type="Decimal" Name="Revision" Nullable="false" Precision="29" Scale="29" />
   <NavigationProperty Name="Publisher" Relationship="BooksModel.PublishedBy"
                       FromRole="Book" ToRole="Publisher" />
   <NavigationProperty Name="Authors" Relationship="BooksModel.WrittenBy"
                       FromRole="Book" ToRole="Author" />
 </EntityType>
```
 

 

## <a name="enumtype-element-csdl"></a><span data-ttu-id="5fdd5-488">Элемент EnumType (CSDL)</span><span class="sxs-lookup"><span data-stu-id="5fdd5-488">EnumType Element (CSDL)</span></span>

<span data-ttu-id="5fdd5-489">Элемент **enumType** представляет перечисляемый тип.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-489">The **EnumType** element represents an enumerated type.</span></span>

<span data-ttu-id="5fdd5-490">Элемент **enumType** может иметь следующие дочерние элементы (в указанном порядке):</span><span class="sxs-lookup"><span data-stu-id="5fdd5-490">An **EnumType** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="5fdd5-491">Documentation (ноль или один элемент)</span><span class="sxs-lookup"><span data-stu-id="5fdd5-491">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="5fdd5-492">Элемент (ноль или более элементов)</span><span class="sxs-lookup"><span data-stu-id="5fdd5-492">Member (zero or more elements)</span></span>
-   <span data-ttu-id="5fdd5-493">Элементы Annotation (ноль или более элементов)</span><span class="sxs-lookup"><span data-stu-id="5fdd5-493">Annotation elements (zero or more elements)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="5fdd5-494">Применимые атрибуты</span><span class="sxs-lookup"><span data-stu-id="5fdd5-494">Applicable Attributes</span></span>

<span data-ttu-id="5fdd5-495">В следующей таблице описаны атрибуты, которые можно применить к элементу **enumType** .</span><span class="sxs-lookup"><span data-stu-id="5fdd5-495">The table below describes the attributes that can be applied to the **EnumType** element.</span></span>

| <span data-ttu-id="5fdd5-496">Имя атрибута</span><span class="sxs-lookup"><span data-stu-id="5fdd5-496">Attribute Name</span></span>     | <span data-ttu-id="5fdd5-497">Обязательный</span><span class="sxs-lookup"><span data-stu-id="5fdd5-497">Is Required</span></span> | <span data-ttu-id="5fdd5-498">Значение</span><span class="sxs-lookup"><span data-stu-id="5fdd5-498">Value</span></span>                                                                                                                                                                                         |
|:-------------------|:------------|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="5fdd5-499">**имя**;</span><span class="sxs-lookup"><span data-stu-id="5fdd5-499">**Name**</span></span>           | <span data-ttu-id="5fdd5-500">Да</span><span class="sxs-lookup"><span data-stu-id="5fdd5-500">Yes</span></span>         | <span data-ttu-id="5fdd5-501">Имя типа сущности.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-501">The name of the entity type.</span></span>                                                                                                                                                                  |
| <span data-ttu-id="5fdd5-502">**Пометки**</span><span class="sxs-lookup"><span data-stu-id="5fdd5-502">**IsFlags**</span></span>        | <span data-ttu-id="5fdd5-503">Нет</span><span class="sxs-lookup"><span data-stu-id="5fdd5-503">No</span></span>          | <span data-ttu-id="5fdd5-504">**Значение true** или **false**в зависимости от того, можно ли использовать тип перечисления в качестве набора флагов.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-504">**True** or **False**, depending on whether the enum type can be used as a set of flags.</span></span> <span data-ttu-id="5fdd5-505">Значение по умолчанию — **false.**</span><span class="sxs-lookup"><span data-stu-id="5fdd5-505">The default value is **False.**.</span></span>                                                                     |
| <span data-ttu-id="5fdd5-506">**ундерлингтипе**</span><span class="sxs-lookup"><span data-stu-id="5fdd5-506">**UnderlyingType**</span></span> | <span data-ttu-id="5fdd5-507">Нет</span><span class="sxs-lookup"><span data-stu-id="5fdd5-507">No</span></span>          | <span data-ttu-id="5fdd5-508">**EDM. Byte**, **EDM. Int16**, **EDM. Int32**, **EDM. Int64** или **EDM. SByte** , определяющие диапазон значений типа.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-508">**Edm.Byte**, **Edm.Int16**, **Edm.Int32**, **Edm.Int64** or **Edm.SByte** defining the range of values of the type.</span></span> <span data-ttu-id="5fdd5-509">Базовым типом элементов перечисления по умолчанию является **EDM. Int32.**</span><span class="sxs-lookup"><span data-stu-id="5fdd5-509">  The default underlying type of enumeration elements is **Edm.Int32.**.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="5fdd5-510">К элементу **enumType** можно применить любое количество атрибутов аннотации (настраиваемые XML-атрибуты).</span><span class="sxs-lookup"><span data-stu-id="5fdd5-510">Any number of annotation attributes (custom XML attributes) may be applied to the **EnumType** element.</span></span> <span data-ttu-id="5fdd5-511">Однако настраиваемые атрибуты не могут принадлежать к любому пространству имен XML, зарезервированному для CSDL.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-511">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="5fdd5-512">Полные имена любых двух настраиваемых атрибутов не могут совпадать.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-512">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="5fdd5-513">Пример</span><span class="sxs-lookup"><span data-stu-id="5fdd5-513">Example</span></span>

<span data-ttu-id="5fdd5-514">В следующем примере показан элемент **enumType** с тремя элементами **member** :</span><span class="sxs-lookup"><span data-stu-id="5fdd5-514">The following example shows an **EnumType** element with three **Member** elements:</span></span>

``` xml
 <EnumType Name="Color" IsFlags=”false” UnderlyingTyp=”Edm.Byte”>
   <Member Name="Red" />
   <Member Name="Green" />
   <Member Name="Blue" />
 </EntityType>
```
 

 

## <a name="function-element-csdl"></a><span data-ttu-id="5fdd5-515">Элемент Function (CSDL)</span><span class="sxs-lookup"><span data-stu-id="5fdd5-515">Function Element (CSDL)</span></span>

<span data-ttu-id="5fdd5-516">Элемент **Function** в языке CSDL используется для определения или объявления функций в концептуальной модели.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-516">The **Function** element in conceptual schema definition language (CSDL) is used to define or declare functions in the conceptual model.</span></span> <span data-ttu-id="5fdd5-517">Функция определяется с использованием элемента DefiningExpression.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-517">A function is defined by using a DefiningExpression element.</span></span>  

<span data-ttu-id="5fdd5-518">Элемент **функции** может иметь следующие дочерние элементы (в указанном порядке):</span><span class="sxs-lookup"><span data-stu-id="5fdd5-518">A **Function** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="5fdd5-519">Documentation (ноль или один элемент)</span><span class="sxs-lookup"><span data-stu-id="5fdd5-519">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="5fdd5-520">Parameter (ноль или больше элементов)</span><span class="sxs-lookup"><span data-stu-id="5fdd5-520">Parameter (zero or more elements)</span></span>
-   <span data-ttu-id="5fdd5-521">DefiningExpression (ноль или один элемент)</span><span class="sxs-lookup"><span data-stu-id="5fdd5-521">DefiningExpression (zero or one element)</span></span>
-   <span data-ttu-id="5fdd5-522">ReturnType (функция) (ноль или один элемент)</span><span class="sxs-lookup"><span data-stu-id="5fdd5-522">ReturnType (Function) (zero or one element)</span></span>
-   <span data-ttu-id="5fdd5-523">Элементы Annotation (ноль или более элементов)</span><span class="sxs-lookup"><span data-stu-id="5fdd5-523">Annotation elements (zero or more elements)</span></span>

<span data-ttu-id="5fdd5-524">Тип возвращаемого значения для функции должен быть указан либо с помощью элемента **ReturnType** (Function), либо с помощью атрибута **ReturnType** (см. ниже), но не в обоих.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-524">A return type for a function must be specified with either the **ReturnType** (Function) element or the **ReturnType** attribute (see below), but not both.</span></span> <span data-ttu-id="5fdd5-525">Возвращаемым типом может быть EdmSimpleType, тип сущности, сложный тип, строковый тип, ссылочный тип или коллекция, которая включает один из этих типов.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-525">The possible return types are any EdmSimpleType, entity type, complex type, row type, or ref type (or a collection of one of these types).</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="5fdd5-526">Применимые атрибуты</span><span class="sxs-lookup"><span data-stu-id="5fdd5-526">Applicable Attributes</span></span>

<span data-ttu-id="5fdd5-527">В следующей таблице описаны атрибуты, которые можно применить к элементу **Function** .</span><span class="sxs-lookup"><span data-stu-id="5fdd5-527">The table below describes the attributes that can be applied to the **Function** element.</span></span>

| <span data-ttu-id="5fdd5-528">Имя атрибута</span><span class="sxs-lookup"><span data-stu-id="5fdd5-528">Attribute Name</span></span> | <span data-ttu-id="5fdd5-529">Обязательный</span><span class="sxs-lookup"><span data-stu-id="5fdd5-529">Is Required</span></span> | <span data-ttu-id="5fdd5-530">Значение</span><span class="sxs-lookup"><span data-stu-id="5fdd5-530">Value</span></span>                              |
|:---------------|:------------|:-----------------------------------|
| <span data-ttu-id="5fdd5-531">**имя**;</span><span class="sxs-lookup"><span data-stu-id="5fdd5-531">**Name**</span></span>       | <span data-ttu-id="5fdd5-532">Да</span><span class="sxs-lookup"><span data-stu-id="5fdd5-532">Yes</span></span>         | <span data-ttu-id="5fdd5-533">Имя функции.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-533">The name of the function.</span></span>          |
| <span data-ttu-id="5fdd5-534">**ReturnType**</span><span class="sxs-lookup"><span data-stu-id="5fdd5-534">**ReturnType**</span></span> | <span data-ttu-id="5fdd5-535">Нет</span><span class="sxs-lookup"><span data-stu-id="5fdd5-535">No</span></span>          | <span data-ttu-id="5fdd5-536">Тип, возвращаемый функцией.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-536">The type returned by the function.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="5fdd5-537">К элементу **функции** можно применить любое количество атрибутов аннотации (настраиваемые XML-атрибуты).</span><span class="sxs-lookup"><span data-stu-id="5fdd5-537">Any number of annotation attributes (custom XML attributes) may be applied to the **Function** element.</span></span> <span data-ttu-id="5fdd5-538">Однако настраиваемые атрибуты не могут принадлежать к любому пространству имен XML, зарезервированному для CSDL.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-538">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="5fdd5-539">Полные имена любых двух настраиваемых атрибутов не могут совпадать.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-539">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="5fdd5-540">Пример</span><span class="sxs-lookup"><span data-stu-id="5fdd5-540">Example</span></span>

<span data-ttu-id="5fdd5-541">В следующем примере используется элемент **Function** для определения функции, возвращающей количество лет с момента найма инструктора.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-541">The following example uses a **Function** element to define a function that returns the number of years since an instructor was hired.</span></span>

``` xml
 <Function Name="YearsSince" ReturnType="Edm.Int32">
   <Parameter Name="date" Type="Edm.DateTime" />
   <DefiningExpression>
     Year(CurrentDateTime()) - Year(date)
   </DefiningExpression>
 </Function>
```
 

 

## <a name="functionimport-element-csdl"></a><span data-ttu-id="5fdd5-542">Элемент FunctionImport (CSDL)</span><span class="sxs-lookup"><span data-stu-id="5fdd5-542">FunctionImport Element (CSDL)</span></span>

<span data-ttu-id="5fdd5-543">Элемент **FunctionImport** в языке CSDL представляет функцию, которая определена в источнике данных, но доступна объектам через концептуальную модель.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-543">The **FunctionImport** element in conceptual schema definition language (CSDL) represents a function that is defined in the data source but available to objects through the conceptual model.</span></span> <span data-ttu-id="5fdd5-544">Например, элемент Function в модели хранения может быть использован для представления хранимой процедуры в базе данных.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-544">For example, a Function element in the storage model can be used to represent a stored procedure in a database.</span></span> <span data-ttu-id="5fdd5-545">Элемент **FunctionImport** в концептуальной модели представляет соответствующую функцию в Entity Framework приложении и сопоставляется с функцией модели хранения с помощью элемента FunctionImportMapping.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-545">A **FunctionImport** element in the conceptual model represents the corresponding function in an Entity Framework application and is mapped to the storage model function by using the FunctionImportMapping element.</span></span> <span data-ttu-id="5fdd5-546">При вызове функции в приложении соответствующая хранимая процедура выполняется в базе данных.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-546">When the function is called in the application, the corresponding stored procedure is executed in the database.</span></span>

<span data-ttu-id="5fdd5-547">Элемент **FunctionImport** может иметь следующие дочерние элементы (в указанном порядке):</span><span class="sxs-lookup"><span data-stu-id="5fdd5-547">The **FunctionImport** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="5fdd5-548">Documentation (допустимое количество элементов — ноль или один)</span><span class="sxs-lookup"><span data-stu-id="5fdd5-548">Documentation (zero or one elements allowed)</span></span>
-   <span data-ttu-id="5fdd5-549">Parameter (допустимое количество элементов — ноль или больше)</span><span class="sxs-lookup"><span data-stu-id="5fdd5-549">Parameter (zero or more elements allowed)</span></span>
-   <span data-ttu-id="5fdd5-550">Элементы Annotation (допустимое количество элементов — ноль или больше)</span><span class="sxs-lookup"><span data-stu-id="5fdd5-550">Annotation elements (zero or more elements allowed)</span></span>
-   <span data-ttu-id="5fdd5-551">ReturnType (FunctionImport) (разрешено ноль или более элементов)</span><span class="sxs-lookup"><span data-stu-id="5fdd5-551">ReturnType (FunctionImport) (zero or more elements allowed)</span></span>

<span data-ttu-id="5fdd5-552">Для каждого параметра, который принимает функция, должен быть определен один элемент **параметра** .</span><span class="sxs-lookup"><span data-stu-id="5fdd5-552">One **Parameter** element should be defined for each parameter that the function accepts.</span></span>

<span data-ttu-id="5fdd5-553">Тип возвращаемого значения для функции должен быть указан либо с помощью элемента **ReturnType** (FunctionImport), либо с помощью атрибута **ReturnType** (см. ниже), но не в обоих.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-553">A return type for a function must be specified with either the **ReturnType** (FunctionImport) element or the **ReturnType** attribute (see below), but not both.</span></span> <span data-ttu-id="5fdd5-554">Значение возвращаемого типа должно быть коллекцией EdmSimpleType, EntityType или ComplexType.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-554">The return type value must be a  collection of EdmSimpleType, EntityType, or ComplexType.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="5fdd5-555">Применимые атрибуты</span><span class="sxs-lookup"><span data-stu-id="5fdd5-555">Applicable Attributes</span></span>

<span data-ttu-id="5fdd5-556">В следующей таблице описаны атрибуты, которые можно применить к элементу **FunctionImport** .</span><span class="sxs-lookup"><span data-stu-id="5fdd5-556">The table below describes the attributes that can be applied to the **FunctionImport** element.</span></span>

| <span data-ttu-id="5fdd5-557">Имя атрибута</span><span class="sxs-lookup"><span data-stu-id="5fdd5-557">Attribute Name</span></span>   | <span data-ttu-id="5fdd5-558">Обязательный</span><span class="sxs-lookup"><span data-stu-id="5fdd5-558">Is Required</span></span> | <span data-ttu-id="5fdd5-559">Значение</span><span class="sxs-lookup"><span data-stu-id="5fdd5-559">Value</span></span>                                                                                                                                                                                                 |
|:-----------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="5fdd5-560">**имя**;</span><span class="sxs-lookup"><span data-stu-id="5fdd5-560">**Name**</span></span>         | <span data-ttu-id="5fdd5-561">Да</span><span class="sxs-lookup"><span data-stu-id="5fdd5-561">Yes</span></span>         | <span data-ttu-id="5fdd5-562">Имя импортируемой функции.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-562">The name of the imported function.</span></span>                                                                                                                                                                    |
| <span data-ttu-id="5fdd5-563">**ReturnType**</span><span class="sxs-lookup"><span data-stu-id="5fdd5-563">**ReturnType**</span></span>   | <span data-ttu-id="5fdd5-564">Нет</span><span class="sxs-lookup"><span data-stu-id="5fdd5-564">No</span></span>          | <span data-ttu-id="5fdd5-565">Тип, возвращаемый функцией.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-565">The type that the function returns.</span></span> <span data-ttu-id="5fdd5-566">Не используйте этот атрибут, если функция не возвращает значение.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-566">Do not use this attribute if the function does not return a value.</span></span> <span data-ttu-id="5fdd5-567">В противном случае значение должно быть коллекцией типа ComplexType, EntityType или EDMSimpleType.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-567">Otherwise, the value must be a collection of ComplexType, EntityType, or EDMSimpleType.</span></span>        |
| <span data-ttu-id="5fdd5-568">**EntitySet**</span><span class="sxs-lookup"><span data-stu-id="5fdd5-568">**EntitySet**</span></span>    | <span data-ttu-id="5fdd5-569">Нет</span><span class="sxs-lookup"><span data-stu-id="5fdd5-569">No</span></span>          | <span data-ttu-id="5fdd5-570">Если функция возвращает коллекцию типов сущностей, то значение **EntitySet** должно быть набором сущностей, к которому принадлежит коллекция.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-570">If the function returns a collection of entity types, the value of the **EntitySet** must be the entity set to which the collection belongs.</span></span> <span data-ttu-id="5fdd5-571">В противном случае атрибут **EntitySet** не должен использоваться.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-571">Otherwise, the **EntitySet** attribute must not be used.</span></span> |
| <span data-ttu-id="5fdd5-572">**IsComposable**</span><span class="sxs-lookup"><span data-stu-id="5fdd5-572">**IsComposable**</span></span> | <span data-ttu-id="5fdd5-573">Нет</span><span class="sxs-lookup"><span data-stu-id="5fdd5-573">No</span></span>          | <span data-ttu-id="5fdd5-574">Если значение равно true, функция является составной (функция с табличным значением) и может использоваться в запросе LINQ.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-574">If the value is set to true, the function is composable (Table-valued Function) and can be used in a LINQ query.</span></span><span data-ttu-id="5fdd5-575">Значение по умолчанию — **false**.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-575">  The default is **false**.</span></span>                                                           |

 

> [!NOTE]
> <span data-ttu-id="5fdd5-576">К элементу **FunctionImport** можно применить любое количество атрибутов аннотации (настраиваемые XML-атрибуты).</span><span class="sxs-lookup"><span data-stu-id="5fdd5-576">Any number of annotation attributes (custom XML attributes) may be applied to the **FunctionImport** element.</span></span> <span data-ttu-id="5fdd5-577">Однако настраиваемые атрибуты не могут принадлежать к любому пространству имен XML, зарезервированному для CSDL.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-577">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="5fdd5-578">Полные имена любых двух настраиваемых атрибутов не могут совпадать.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-578">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="5fdd5-579">Пример</span><span class="sxs-lookup"><span data-stu-id="5fdd5-579">Example</span></span>

<span data-ttu-id="5fdd5-580">В следующем примере показан элемент **FunctionImport** , принимающий один параметр и возвращающий коллекцию типов сущностей:</span><span class="sxs-lookup"><span data-stu-id="5fdd5-580">The following example shows a **FunctionImport** element that accepts one parameter and returns a collection of entity types:</span></span>

``` xml
 <FunctionImport Name="GetStudentGrades"
                 EntitySet="StudentGrade"
                 ReturnType="Collection(SchoolModel.StudentGrade)">
        <Parameter Name="StudentID" Mode="In" Type="Int32" />
 </FunctionImport>
```
 

 

## <a name="key-element-csdl"></a><span data-ttu-id="5fdd5-581">Элемент Key (CSDL)</span><span class="sxs-lookup"><span data-stu-id="5fdd5-581">Key Element (CSDL)</span></span>

<span data-ttu-id="5fdd5-582">Элемент **Key** является дочерним элементом элемента EntityType и определяет *ключ сущности* (свойство или набор свойств типа сущности, определяющего удостоверение).</span><span class="sxs-lookup"><span data-stu-id="5fdd5-582">The **Key** element is a child element of the EntityType element and defines an *entity key* (a property or a set of properties of an entity type that determine identity).</span></span> <span data-ttu-id="5fdd5-583">Свойства, составляющие ключ сущности, выбираются во время разработки.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-583">The properties that make up an entity key are chosen at design time.</span></span> <span data-ttu-id="5fdd5-584">Значения свойств ключа сущности должны уникально определять экземпляр типа сущности внутри набора сущностей во время выполнения.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-584">The values of entity key properties must uniquely identify an entity type instance within an entity set at run time.</span></span> <span data-ttu-id="5fdd5-585">Свойства, составляющие ключ сущности, должны гарантировать уникальность экземпляра набора сущностей.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-585">The properties that make up an entity key should be chosen to guarantee uniqueness of instances in an entity set.</span></span> <span data-ttu-id="5fdd5-586">Элемент **Key** определяет ключ сущности, ссылаясь на одно или несколько свойств типа сущности.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-586">The **Key** element defines an entity key by referencing one or more of the properties of an entity type.</span></span>

<span data-ttu-id="5fdd5-587">Элемент **Key** может иметь следующие дочерние элементы:</span><span class="sxs-lookup"><span data-stu-id="5fdd5-587">The **Key** element can have the following child elements:</span></span>

-   <span data-ttu-id="5fdd5-588">PropertyRef (один или несколько элементов)</span><span class="sxs-lookup"><span data-stu-id="5fdd5-588">PropertyRef (one or more elements)</span></span>
-   <span data-ttu-id="5fdd5-589">Элементы Annotation (ноль или более элементов)</span><span class="sxs-lookup"><span data-stu-id="5fdd5-589">Annotation elements (zero or more elements)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="5fdd5-590">Применимые атрибуты</span><span class="sxs-lookup"><span data-stu-id="5fdd5-590">Applicable Attributes</span></span>

<span data-ttu-id="5fdd5-591">К элементу **Key** можно применить любое количество атрибутов аннотации (настраиваемые XML-атрибуты).</span><span class="sxs-lookup"><span data-stu-id="5fdd5-591">Any number of annotation attributes (custom XML attributes) may be applied to the **Key** element.</span></span> <span data-ttu-id="5fdd5-592">Однако настраиваемые атрибуты не могут принадлежать к любому пространству имен XML, зарезервированному для CSDL.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-592">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="5fdd5-593">Полные имена любых двух настраиваемых атрибутов не могут совпадать.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-593">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="5fdd5-594">Пример</span><span class="sxs-lookup"><span data-stu-id="5fdd5-594">Example</span></span>

<span data-ttu-id="5fdd5-595">В приведенном ниже примере определяется тип сущности « **Book**».</span><span class="sxs-lookup"><span data-stu-id="5fdd5-595">The example below defines an entity type named **Book**.</span></span> <span data-ttu-id="5fdd5-596">Ключ сущности определяется с помощью ссылки на свойство **ISBN** типа сущности.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-596">The entity key is defined by referencing the **ISBN** property of the entity type.</span></span>

``` xml
 <EntityType Name="Book">
   <Key>
     <PropertyRef Name="ISBN" />
   </Key>
   <Property Type="String" Name="ISBN" Nullable="false" />
   <Property Type="String" Name="Title" Nullable="false" />
   <Property Type="Decimal" Name="Revision" Nullable="false" Precision="29" Scale="29" />
   <NavigationProperty Name="Publisher" Relationship="BooksModel.PublishedBy"
                       FromRole="Book" ToRole="Publisher" />
   <NavigationProperty Name="Authors" Relationship="BooksModel.WrittenBy"
                       FromRole="Book" ToRole="Author" />
 </EntityType>
```
 

<span data-ttu-id="5fdd5-597">Свойство **ISBN** является хорошим выбором для ключа сущности, так как Международный номер книги (ISBN) является уникальным идентификатором книги.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-597">The **ISBN** property is a good choice for the entity key because an International Standard Book Number (ISBN) uniquely identifies a book.</span></span>

<span data-ttu-id="5fdd5-598">В следующем примере показан тип сущности (**Author**), имеющий ключ сущности, состоящий из двух свойств: **Name** и **Address**.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-598">The following example shows an entity type (**Author**) that has an entity key that consists of two properties, **Name** and **Address**.</span></span>

``` xml
 <EntityType Name="Author">
   <Key>
     <PropertyRef Name="Name" />
     <PropertyRef Name="Address" />
   </Key>
   <Property Type="String" Name="Name" Nullable="false" />
   <Property Type="String" Name="Address" Nullable="false" />
   <NavigationProperty Name="Books" Relationship="BooksModel.WrittenBy"
                       FromRole="Author" ToRole="Book" />
 </EntityType>
```
 

<span data-ttu-id="5fdd5-599">Использование **имени** и **адреса** для ключа сущности является разумным выбором, поскольку два автора одного и того же имени вряд ли будут находиться на одном и том же адресе.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-599">Using **Name** and **Address** for the entity key is a reasonable choice, because two authors of the same name are unlikely to live at the same address.</span></span> <span data-ttu-id="5fdd5-600">Однако такой выбор ключа сущности не гарантирует уникальность ключей сущности в наборе сущностей.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-600">However, this choice for an entity key does not absolutely guarantee unique entity keys in an entity set.</span></span> <span data-ttu-id="5fdd5-601">В этом случае рекомендуется добавить свойство, например **аусорид**, которое может быть использовано для уникальной идентификации автора.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-601">Adding a property, such as **AuthorId**, that could be used to uniquely identify an author would be recommended in this case.</span></span>

 

## <a name="member-element-csdl"></a><span data-ttu-id="5fdd5-602">Элемент Member (CSDL)</span><span class="sxs-lookup"><span data-stu-id="5fdd5-602">Member Element (CSDL)</span></span>

<span data-ttu-id="5fdd5-603">Элемент **member** является дочерним элементом элемента enumType и определяет элемент перечисляемого типа.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-603">The **Member** element is a child element of the EnumType element and defines a member of the enumerated type.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="5fdd5-604">Применимые атрибуты</span><span class="sxs-lookup"><span data-stu-id="5fdd5-604">Applicable Attributes</span></span>

<span data-ttu-id="5fdd5-605">В следующей таблице описаны атрибуты, которые можно применить к элементу **FunctionImport** .</span><span class="sxs-lookup"><span data-stu-id="5fdd5-605">The table below describes the attributes that can be applied to the **FunctionImport** element.</span></span>

| <span data-ttu-id="5fdd5-606">Имя атрибута</span><span class="sxs-lookup"><span data-stu-id="5fdd5-606">Attribute Name</span></span> | <span data-ttu-id="5fdd5-607">Обязательный</span><span class="sxs-lookup"><span data-stu-id="5fdd5-607">Is Required</span></span> | <span data-ttu-id="5fdd5-608">Значение</span><span class="sxs-lookup"><span data-stu-id="5fdd5-608">Value</span></span>                                                                                                                                                                                    |
|:---------------|:------------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="5fdd5-609">**имя**;</span><span class="sxs-lookup"><span data-stu-id="5fdd5-609">**Name**</span></span>       | <span data-ttu-id="5fdd5-610">Да</span><span class="sxs-lookup"><span data-stu-id="5fdd5-610">Yes</span></span>         | <span data-ttu-id="5fdd5-611">Имя элемента.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-611">The name of the member.</span></span>                                                                                                                                                                  |
| <span data-ttu-id="5fdd5-612">**Значение**</span><span class="sxs-lookup"><span data-stu-id="5fdd5-612">**Value**</span></span>      | <span data-ttu-id="5fdd5-613">Нет</span><span class="sxs-lookup"><span data-stu-id="5fdd5-613">No</span></span>          | <span data-ttu-id="5fdd5-614">Значение элемента.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-614">The value of the member.</span></span> <span data-ttu-id="5fdd5-615">По умолчанию первый элемент имеет значение 0, а значение каждого последующего перечислителя увеличивается на 1.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-615">By default, the first member has the value 0, and the value of each successive enumerator is incremented by 1.</span></span> <span data-ttu-id="5fdd5-616">Могут существовать несколько членов с одинаковыми значениями.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-616">Multiple members with the same values may exist.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="5fdd5-617">К элементу **FunctionImport** можно применить любое количество атрибутов аннотации (настраиваемые XML-атрибуты).</span><span class="sxs-lookup"><span data-stu-id="5fdd5-617">Any number of annotation attributes (custom XML attributes) may be applied to the **FunctionImport** element.</span></span> <span data-ttu-id="5fdd5-618">Однако настраиваемые атрибуты не могут принадлежать к любому пространству имен XML, зарезервированному для CSDL.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-618">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="5fdd5-619">Полные имена любых двух настраиваемых атрибутов не могут совпадать.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-619">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="5fdd5-620">Пример</span><span class="sxs-lookup"><span data-stu-id="5fdd5-620">Example</span></span>

<span data-ttu-id="5fdd5-621">В следующем примере показан элемент **enumType** с тремя элементами **member** :</span><span class="sxs-lookup"><span data-stu-id="5fdd5-621">The following example shows an **EnumType** element with three **Member** elements:</span></span>

``` xml
 <EnumType Name="Color">
   <Member Name="Red" Value=”1”/>
   <Member Name="Green" Value=”3” />
   <Member Name="Blue" Value=”5”/>
 </EntityType>
```
 

 

## <a name="navigationproperty-element-csdl"></a><span data-ttu-id="5fdd5-622">Элемент NavigationProperty (CSDL)</span><span class="sxs-lookup"><span data-stu-id="5fdd5-622">NavigationProperty Element (CSDL)</span></span>

<span data-ttu-id="5fdd5-623">Элемент **NavigationProperty** определяет свойство навигации, которое предоставляет ссылку на другой конец ассоциации.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-623">A **NavigationProperty** element defines a navigation property, which provides a reference to the other end of an association.</span></span> <span data-ttu-id="5fdd5-624">В отличие от свойств, определенных в элементе Property, свойства навигации не определяют форму и характеристики данных.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-624">Unlike properties defined with the Property element, navigation properties do not define the shape and characteristics of data.</span></span> <span data-ttu-id="5fdd5-625">Они предоставляют возможность навигации по ассоциации между двумя типами сущностей.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-625">They provide a way to navigate an association between two entity types.</span></span>

<span data-ttu-id="5fdd5-626">Обратите внимание, что свойства навигации являются необязательными для обоих типов сущностей, расположенных в конечных элементах ассоциации.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-626">Note that navigation properties are optional on both entity types at the ends of an association.</span></span> <span data-ttu-id="5fdd5-627">Если свойство навигации было определено для типа сущности на одном конечном элементе ассоциации, то определять его для типа сущности на другом конечном элементе необязательно.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-627">If you define a navigation property on one entity type at the end of an association, you do not have to define a navigation property on the entity type at the other end of the association.</span></span>

<span data-ttu-id="5fdd5-628">Тип данных, возвращаемый свойством навигации, определяется кратностью в удаленном элементе ассоциации.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-628">The data type returned by a navigation property is determined by the multiplicity of its remote association end.</span></span> <span data-ttu-id="5fdd5-629">Например, предположим, что свойство навигации **ордерснавпроп**существует в типе сущности **Customer** и выполняет навигацию между **клиентом** и **заказом**.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-629">For example, suppose a navigation property, **OrdersNavProp**, exists on a **Customer** entity type and navigates a one-to-many association between **Customer** and **Order**.</span></span> <span data-ttu-id="5fdd5-630">Так как удаленная ассоциация для свойства навигации имеет кратность many ( \* ), ее тип данных — это коллекция (в **порядке**).</span><span class="sxs-lookup"><span data-stu-id="5fdd5-630">Because the remote association end for the navigation property has multiplicity many (\*), its data type is a collection (of **Order**).</span></span> <span data-ttu-id="5fdd5-631">Аналогично, если свойство навигации **кустомернавпроп**существует в типе сущности **Order** , его тип данных — **Customer** , так как кратность удаленного элемента равна единице (1).</span><span class="sxs-lookup"><span data-stu-id="5fdd5-631">Similarly, if a navigation property, **CustomerNavProp**, exists on the **Order** entity type, its data type would be **Customer** since the multiplicity of the remote end is one (1).</span></span>

<span data-ttu-id="5fdd5-632">Элемент **NavigationProperty** может иметь следующие дочерние элементы (в указанном порядке):</span><span class="sxs-lookup"><span data-stu-id="5fdd5-632">A **NavigationProperty** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="5fdd5-633">Documentation (ноль или один элемент)</span><span class="sxs-lookup"><span data-stu-id="5fdd5-633">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="5fdd5-634">Элементы Annotation (ноль или более элементов)</span><span class="sxs-lookup"><span data-stu-id="5fdd5-634">Annotation elements (zero or more elements)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="5fdd5-635">Применимые атрибуты</span><span class="sxs-lookup"><span data-stu-id="5fdd5-635">Applicable Attributes</span></span>

<span data-ttu-id="5fdd5-636">В следующей таблице описаны атрибуты, которые можно применить к элементу **NavigationProperty** .</span><span class="sxs-lookup"><span data-stu-id="5fdd5-636">The table below describes the attributes that can be applied to the **NavigationProperty** element.</span></span>

| <span data-ttu-id="5fdd5-637">Имя атрибута</span><span class="sxs-lookup"><span data-stu-id="5fdd5-637">Attribute Name</span></span>   | <span data-ttu-id="5fdd5-638">Обязательный</span><span class="sxs-lookup"><span data-stu-id="5fdd5-638">Is Required</span></span> | <span data-ttu-id="5fdd5-639">Значение</span><span class="sxs-lookup"><span data-stu-id="5fdd5-639">Value</span></span>                                                                                                                                                                                                                                            |
|:-----------------|:------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="5fdd5-640">**имя**;</span><span class="sxs-lookup"><span data-stu-id="5fdd5-640">**Name**</span></span>         | <span data-ttu-id="5fdd5-641">Да</span><span class="sxs-lookup"><span data-stu-id="5fdd5-641">Yes</span></span>         | <span data-ttu-id="5fdd5-642">Имя свойства навигации.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-642">The name of the navigation property.</span></span>                                                                                                                                                                                                             |
| <span data-ttu-id="5fdd5-643">**Relationship**</span><span class="sxs-lookup"><span data-stu-id="5fdd5-643">**Relationship**</span></span> | <span data-ttu-id="5fdd5-644">Да</span><span class="sxs-lookup"><span data-stu-id="5fdd5-644">Yes</span></span>         | <span data-ttu-id="5fdd5-645">Имя ассоциации, расположенной в пределах области модели.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-645">The name of an association that is within the scope of the model.</span></span>                                                                                                                                                                                |
| <span data-ttu-id="5fdd5-646">**ToRole**</span><span class="sxs-lookup"><span data-stu-id="5fdd5-646">**ToRole**</span></span>       | <span data-ttu-id="5fdd5-647">Да</span><span class="sxs-lookup"><span data-stu-id="5fdd5-647">Yes</span></span>         | <span data-ttu-id="5fdd5-648">Конечная точка ассоциации на которой заканчивается навигация.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-648">The end of the association at which navigation ends.</span></span> <span data-ttu-id="5fdd5-649">Значение атрибута **ToRole** должно совпадать со значением одного из атрибутов **роли** , определенных на одной из сторон ассоциации (определена в элементе AssociationEnd).</span><span class="sxs-lookup"><span data-stu-id="5fdd5-649">The value of the **ToRole** attribute must be the same as the value of one of the **Role** attributes defined on one of the association ends (defined in the AssociationEnd element).</span></span>       |
| <span data-ttu-id="5fdd5-650">**FromRole**</span><span class="sxs-lookup"><span data-stu-id="5fdd5-650">**FromRole**</span></span>     | <span data-ttu-id="5fdd5-651">Да</span><span class="sxs-lookup"><span data-stu-id="5fdd5-651">Yes</span></span>         | <span data-ttu-id="5fdd5-652">Конечная точка ассоциации с которой начинается навигация.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-652">The end of the association from which navigation begins.</span></span> <span data-ttu-id="5fdd5-653">Значение атрибута **FromRole** должно совпадать со значением одного из атрибутов **роли** , определенных на одной из сторон ассоциации (определена в элементе AssociationEnd).</span><span class="sxs-lookup"><span data-stu-id="5fdd5-653">The value of the **FromRole** attribute must be the same as the value of one of the **Role** attributes defined on one of the association ends (defined in the AssociationEnd element).</span></span> |

 

> [!NOTE]
> <span data-ttu-id="5fdd5-654">К элементу **NavigationProperty** можно применить любое количество атрибутов аннотации (настраиваемые XML-атрибуты).</span><span class="sxs-lookup"><span data-stu-id="5fdd5-654">Any number of annotation attributes (custom XML attributes) may be applied to the **NavigationProperty** element.</span></span> <span data-ttu-id="5fdd5-655">Однако настраиваемые атрибуты не могут принадлежать к любому пространству имен XML, зарезервированному для CSDL.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-655">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="5fdd5-656">Полные имена любых двух настраиваемых атрибутов не могут совпадать.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-656">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="5fdd5-657">Пример</span><span class="sxs-lookup"><span data-stu-id="5fdd5-657">Example</span></span>

<span data-ttu-id="5fdd5-658">В следующем примере определяется тип сущности (**Book**) с двумя свойствами навигации (**публишедби** и **вриттенби**):</span><span class="sxs-lookup"><span data-stu-id="5fdd5-658">The following example defines an entity type (**Book**) with two navigation properties (**PublishedBy** and **WrittenBy**):</span></span>

``` xml
 <EntityType Name="Book">
   <Key>
     <PropertyRef Name="ISBN" />
   </Key>
   <Property Type="String" Name="ISBN" Nullable="false" />
   <Property Type="String" Name="Title" Nullable="false" />
   <Property Type="Decimal" Name="Revision" Nullable="false" Precision="29" Scale="29" />
   <NavigationProperty Name="Publisher" Relationship="BooksModel.PublishedBy"
                       FromRole="Book" ToRole="Publisher" />
   <NavigationProperty Name="Authors" Relationship="BooksModel.WrittenBy"
                       FromRole="Book" ToRole="Author" />
 </EntityType>
```
 

 

## <a name="ondelete-element-csdl"></a><span data-ttu-id="5fdd5-659">Элемент OnDelete (CSDL)</span><span class="sxs-lookup"><span data-stu-id="5fdd5-659">OnDelete Element (CSDL)</span></span>

<span data-ttu-id="5fdd5-660">Элемент **OnDelete** в языке CSDL определяет поведение, связанное с Ассоциацией.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-660">The **OnDelete** element in conceptual schema definition language (CSDL) defines behavior that is connected with an association.</span></span> <span data-ttu-id="5fdd5-661">Если атрибут **Action** имеет значение **CASCADE** на одном конце ассоциации, то связанные типы сущностей на другом конце ассоциации удаляются при удалении типа сущности в первом конце.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-661">If the **Action** attribute is set to **Cascade** on one end of an association, related entity types on the other end of the association are deleted when the entity type on the first end is deleted.</span></span> <span data-ttu-id="5fdd5-662">Если связь между двумя типами сущностей является связью первичного ключа, то загруженный зависимый объект удаляется, когда объект Principal на другом конце ассоциации удаляется независимо от спецификации **OnDelete** .</span><span class="sxs-lookup"><span data-stu-id="5fdd5-662">If the association between two entity types is a primary key-to-primary key relationship, then a loaded dependent object is deleted when the principal object on the other end of the association is deleted regardless of the **OnDelete** specification.</span></span>  

> [!NOTE]
> <span data-ttu-id="5fdd5-663">Элемент **OnDelete** влияет только на поведение среды выполнения приложения. Он не влияет на поведение в источнике данных.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-663">The **OnDelete** element only affects the runtime behavior of an application; it does not affect behavior in the data source.</span></span> <span data-ttu-id="5fdd5-664">Поведение, определенное в источнике данных, должно совпадать с поведением, определенным для приложения.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-664">The behavior defined in the data source should be the same as the behavior defined in the application.</span></span>

 

<span data-ttu-id="5fdd5-665">Элемент **OnDelete** может иметь следующие дочерние элементы (в указанном порядке):</span><span class="sxs-lookup"><span data-stu-id="5fdd5-665">An **OnDelete** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="5fdd5-666">Documentation (ноль или один элемент)</span><span class="sxs-lookup"><span data-stu-id="5fdd5-666">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="5fdd5-667">Элементы Annotation (ноль или более элементов)</span><span class="sxs-lookup"><span data-stu-id="5fdd5-667">Annotation elements (zero or more elements)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="5fdd5-668">Применимые атрибуты</span><span class="sxs-lookup"><span data-stu-id="5fdd5-668">Applicable Attributes</span></span>

<span data-ttu-id="5fdd5-669">В следующей таблице описаны атрибуты, которые можно применить к элементу **OnDelete** .</span><span class="sxs-lookup"><span data-stu-id="5fdd5-669">The table below describes the attributes that can be applied to the **OnDelete** element.</span></span>

| <span data-ttu-id="5fdd5-670">Имя атрибута</span><span class="sxs-lookup"><span data-stu-id="5fdd5-670">Attribute Name</span></span> | <span data-ttu-id="5fdd5-671">Обязательный</span><span class="sxs-lookup"><span data-stu-id="5fdd5-671">Is Required</span></span> | <span data-ttu-id="5fdd5-672">Значение</span><span class="sxs-lookup"><span data-stu-id="5fdd5-672">Value</span></span>                                                                                                                                                                                                                         |
|:---------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="5fdd5-673">**Действие**</span><span class="sxs-lookup"><span data-stu-id="5fdd5-673">**Action**</span></span>     | <span data-ttu-id="5fdd5-674">Да</span><span class="sxs-lookup"><span data-stu-id="5fdd5-674">Yes</span></span>         | <span data-ttu-id="5fdd5-675">**CASCADE** или **None**.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-675">**Cascade** or **None**.</span></span> <span data-ttu-id="5fdd5-676">Если параметр **CASCADE**, зависимые типы сущностей будут удалены при удалении основного типа сущности.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-676">If **Cascade**, dependent entity types will be deleted when the principal entity type is deleted.</span></span> <span data-ttu-id="5fdd5-677">Если **нет**, зависимые типы сущностей не будут удаляться при удалении типа основной сущности.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-677">If **None**, dependent entity types will not be deleted when the principal entity type is deleted.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="5fdd5-678">К элементу **Association** можно применить любое количество атрибутов аннотации (настраиваемые XML-атрибуты).</span><span class="sxs-lookup"><span data-stu-id="5fdd5-678">Any number of annotation attributes (custom XML attributes) may be applied to the **Association** element.</span></span> <span data-ttu-id="5fdd5-679">Однако настраиваемые атрибуты не могут принадлежать к любому пространству имен XML, зарезервированному для CSDL.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-679">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="5fdd5-680">Полные имена любых двух настраиваемых атрибутов не могут совпадать.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-680">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="5fdd5-681">Пример</span><span class="sxs-lookup"><span data-stu-id="5fdd5-681">Example</span></span>

<span data-ttu-id="5fdd5-682">В следующем примере показан элемент **Association** , определяющий ассоциацию **кустомерордерс** .</span><span class="sxs-lookup"><span data-stu-id="5fdd5-682">The following example shows an **Association** element that defines the **CustomerOrders** association.</span></span> <span data-ttu-id="5fdd5-683">Элемент **OnDelete** указывает, что все **заказы** , связанные с конкретным **клиентом** и загруженные в ObjectContext, будут удалены при удалении **клиента** .</span><span class="sxs-lookup"><span data-stu-id="5fdd5-683">The **OnDelete** element indicates that all **Orders** that are related to a particular **Customer** and have been loaded into the ObjectContext will be deleted when the **Customer** is deleted.</span></span>

``` xml
 <Association Name="CustomerOrders">
   <End Type="ExampleModel.Customer" Role="Customer" Multiplicity="1">
         <OnDelete Action="Cascade" />
   </End>
   <End Type="ExampleModel.Order" Role="Order" Multiplicity="*" />
 </Association>
```
 

 

## <a name="parameter-element-csdl"></a><span data-ttu-id="5fdd5-684">Элемент Parameter (CSDL)</span><span class="sxs-lookup"><span data-stu-id="5fdd5-684">Parameter Element (CSDL)</span></span>

<span data-ttu-id="5fdd5-685">Элемент **Parameter** в языке CSDL может быть дочерним по отношению к элементу FunctionImport или Function.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-685">The **Parameter** element in conceptual schema definition language (CSDL) can be a child of the FunctionImport element or the Function element.</span></span>

### <a name="functionimport-element-application"></a><span data-ttu-id="5fdd5-686">Приложение, использующее элемент FunctionImport</span><span class="sxs-lookup"><span data-stu-id="5fdd5-686">FunctionImport Element Application</span></span>

<span data-ttu-id="5fdd5-687">Элемент **Parameter** (как дочерний элемент элемента **FunctionImport** ) используется для определения входных и выходных параметров для импортов функций, объявленных в языке CSDL.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-687">A **Parameter** element (as a child of the **FunctionImport** element) is used to define input and output parameters for function imports that are declared in CSDL.</span></span>

<span data-ttu-id="5fdd5-688">Элемент **Parameter** может иметь следующие дочерние элементы (в указанном порядке):</span><span class="sxs-lookup"><span data-stu-id="5fdd5-688">The **Parameter** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="5fdd5-689">Documentation (допустимое количество элементов — ноль или один)</span><span class="sxs-lookup"><span data-stu-id="5fdd5-689">Documentation (zero or one elements allowed)</span></span>
-   <span data-ttu-id="5fdd5-690">Элементы Annotation (допустимое количество элементов — ноль или больше)</span><span class="sxs-lookup"><span data-stu-id="5fdd5-690">Annotation elements (zero or more elements allowed)</span></span>

#### <a name="applicable-attributes"></a><span data-ttu-id="5fdd5-691">Применимые атрибуты</span><span class="sxs-lookup"><span data-stu-id="5fdd5-691">Applicable Attributes</span></span>

<span data-ttu-id="5fdd5-692">В следующей таблице описаны атрибуты, которые можно применить к элементу **Parameter** .</span><span class="sxs-lookup"><span data-stu-id="5fdd5-692">The following table describes the attributes that can be applied to the **Parameter** element.</span></span>

| <span data-ttu-id="5fdd5-693">Имя атрибута</span><span class="sxs-lookup"><span data-stu-id="5fdd5-693">Attribute Name</span></span> | <span data-ttu-id="5fdd5-694">Обязательный</span><span class="sxs-lookup"><span data-stu-id="5fdd5-694">Is Required</span></span> | <span data-ttu-id="5fdd5-695">Значение</span><span class="sxs-lookup"><span data-stu-id="5fdd5-695">Value</span></span>                                                                                                                                                                                                                           |
|:---------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="5fdd5-696">**имя**;</span><span class="sxs-lookup"><span data-stu-id="5fdd5-696">**Name**</span></span>       | <span data-ttu-id="5fdd5-697">Да</span><span class="sxs-lookup"><span data-stu-id="5fdd5-697">Yes</span></span>         | <span data-ttu-id="5fdd5-698">Имя параметра.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-698">The name of the parameter.</span></span>                                                                                                                                                                                                      |
| <span data-ttu-id="5fdd5-699">**Тип**</span><span class="sxs-lookup"><span data-stu-id="5fdd5-699">**Type**</span></span>       | <span data-ttu-id="5fdd5-700">Да</span><span class="sxs-lookup"><span data-stu-id="5fdd5-700">Yes</span></span>         | <span data-ttu-id="5fdd5-701">Тип параметра.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-701">The parameter type.</span></span> <span data-ttu-id="5fdd5-702">Значение должно представлять собой **EDMSimpleType** или сложный тип в рамках модели.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-702">The value must be an **EDMSimpleType** or a complex type that is within the scope of the model.</span></span>                                                                                                             |
| <span data-ttu-id="5fdd5-703">**Режим**</span><span class="sxs-lookup"><span data-stu-id="5fdd5-703">**Mode**</span></span>       | <span data-ttu-id="5fdd5-704">Нет</span><span class="sxs-lookup"><span data-stu-id="5fdd5-704">No</span></span>          | <span data-ttu-id="5fdd5-705">**В**, **out**или **InOut** в зависимости от того, является ли параметр входным, выходным или входным или выходным.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-705">**In**, **Out**, or **InOut** depending on whether the parameter is an input, output, or input/output parameter.</span></span>                                                                                                                |
| <span data-ttu-id="5fdd5-706">**MaxLength**</span><span class="sxs-lookup"><span data-stu-id="5fdd5-706">**MaxLength**</span></span>  | <span data-ttu-id="5fdd5-707">Нет</span><span class="sxs-lookup"><span data-stu-id="5fdd5-707">No</span></span>          | <span data-ttu-id="5fdd5-708">Максимально допустимая длина параметра.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-708">The maximum allowed length of the parameter.</span></span>                                                                                                                                                                                    |
| <span data-ttu-id="5fdd5-709">**Точность**</span><span class="sxs-lookup"><span data-stu-id="5fdd5-709">**Precision**</span></span>  | <span data-ttu-id="5fdd5-710">Нет</span><span class="sxs-lookup"><span data-stu-id="5fdd5-710">No</span></span>          | <span data-ttu-id="5fdd5-711">Точность параметра.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-711">The precision of the parameter.</span></span>                                                                                                                                                                                                 |
| <span data-ttu-id="5fdd5-712">**Масштабирование**</span><span class="sxs-lookup"><span data-stu-id="5fdd5-712">**Scale**</span></span>      | <span data-ttu-id="5fdd5-713">Нет</span><span class="sxs-lookup"><span data-stu-id="5fdd5-713">No</span></span>          | <span data-ttu-id="5fdd5-714">Масштаб параметра.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-714">The scale of the parameter.</span></span>                                                                                                                                                                                                     |
| <span data-ttu-id="5fdd5-715">**SRID**</span><span class="sxs-lookup"><span data-stu-id="5fdd5-715">**SRID**</span></span>       | <span data-ttu-id="5fdd5-716">Нет</span><span class="sxs-lookup"><span data-stu-id="5fdd5-716">No</span></span>          | <span data-ttu-id="5fdd5-717">Идентификатор пространственной системной ссылки.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-717">Spatial System Reference Identifier.</span></span> <span data-ttu-id="5fdd5-718">Допустимо только для параметров пространственных типов.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-718">Valid only for parameters of spatial types.</span></span> <span data-ttu-id="5fdd5-719">Дополнительные сведения см. в разделе [SRID](https://en.wikipedia.org/wiki/SRID) and [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span><span class="sxs-lookup"><span data-stu-id="5fdd5-719">For more information, see [SRID](https://en.wikipedia.org/wiki/SRID) and [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span></span> |

 

> [!NOTE]
> <span data-ttu-id="5fdd5-720">К элементу **Parameter** можно применить любое количество атрибутов аннотации (настраиваемые XML-атрибуты).</span><span class="sxs-lookup"><span data-stu-id="5fdd5-720">Any number of annotation attributes (custom XML attributes) may be applied to the **Parameter** element.</span></span> <span data-ttu-id="5fdd5-721">Однако настраиваемые атрибуты не могут принадлежать к любому пространству имен XML, зарезервированному для CSDL.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-721">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="5fdd5-722">Полные имена любых двух настраиваемых атрибутов не могут совпадать.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-722">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

#### <a name="example"></a><span data-ttu-id="5fdd5-723">Пример</span><span class="sxs-lookup"><span data-stu-id="5fdd5-723">Example</span></span>

<span data-ttu-id="5fdd5-724">В следующем примере показан элемент **FunctionImport** с одним дочерним элементом **Parameter** .</span><span class="sxs-lookup"><span data-stu-id="5fdd5-724">The following example shows a **FunctionImport** element with one **Parameter** child element.</span></span> <span data-ttu-id="5fdd5-725">Функция принимает один входной параметр и возвращает коллекцию типов сущностей.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-725">The function accepts one input parameter and returns a collection of entity types.</span></span>

``` xml
 <FunctionImport Name="GetStudentGrades"
                 EntitySet="StudentGrade"
                 ReturnType="Collection(SchoolModel.StudentGrade)">
        <Parameter Name="StudentID" Mode="In" Type="Int32" />
 </FunctionImport>
```
 

### <a name="function-element-application"></a><span data-ttu-id="5fdd5-726">Приложение, использующее элемент Function</span><span class="sxs-lookup"><span data-stu-id="5fdd5-726">Function Element Application</span></span>

<span data-ttu-id="5fdd5-727">Элемент **Parameter** (в качестве дочернего элемента **функции** ) определяет параметры для функций, определенных или объявленных в концептуальной модели.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-727">A **Parameter** element (as a child of the **Function** element) defines parameters for functions that are defined or declared in a conceptual model.</span></span>

<span data-ttu-id="5fdd5-728">Элемент **Parameter** может иметь следующие дочерние элементы (в указанном порядке):</span><span class="sxs-lookup"><span data-stu-id="5fdd5-728">The **Parameter** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="5fdd5-729">Documentation (ноль или один элемент)</span><span class="sxs-lookup"><span data-stu-id="5fdd5-729">Documentation (zero or one elements)</span></span>
-   <span data-ttu-id="5fdd5-730">CollectionType (ноль или один элемент)</span><span class="sxs-lookup"><span data-stu-id="5fdd5-730">CollectionType (zero or one elements)</span></span>
-   <span data-ttu-id="5fdd5-731">ReferenceType (ноль или один элемент)</span><span class="sxs-lookup"><span data-stu-id="5fdd5-731">ReferenceType (zero or one elements)</span></span>
-   <span data-ttu-id="5fdd5-732">RowType (ноль или один элемент)</span><span class="sxs-lookup"><span data-stu-id="5fdd5-732">RowType (zero or one elements)</span></span>

> [!NOTE]
> <span data-ttu-id="5fdd5-733">Только один из элементов **CollectionType**, **ReferenceType**или **RowType** может быть дочерним элементом элемента **Property** .</span><span class="sxs-lookup"><span data-stu-id="5fdd5-733">Only one of the **CollectionType**, **ReferenceType**, or **RowType** elements can be a child element of a **Property** element.</span></span>

 

-   <span data-ttu-id="5fdd5-734">Элементы Annotation (допустимое количество элементов — ноль или больше)</span><span class="sxs-lookup"><span data-stu-id="5fdd5-734">Annotation elements (zero or more elements allowed)</span></span>

> [!NOTE]
> <span data-ttu-id="5fdd5-735">Элементы Annotation должны следовать после всех остальных дочерних элементов.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-735">Annotation elements must appear after all other child elements.</span></span> <span data-ttu-id="5fdd5-736">Элементы аннотации разрешены только в CSDL v2 и более поздних версиях.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-736">Annotation elements are only allowed in CSDL v2 and later.</span></span>

 

#### <a name="applicable-attributes"></a><span data-ttu-id="5fdd5-737">Применимые атрибуты</span><span class="sxs-lookup"><span data-stu-id="5fdd5-737">Applicable Attributes</span></span>

<span data-ttu-id="5fdd5-738">В следующей таблице описаны атрибуты, которые можно применить к элементу **Parameter** .</span><span class="sxs-lookup"><span data-stu-id="5fdd5-738">The following table describes the attributes that can be applied to the **Parameter** element.</span></span>

| <span data-ttu-id="5fdd5-739">Имя атрибута</span><span class="sxs-lookup"><span data-stu-id="5fdd5-739">Attribute Name</span></span>   | <span data-ttu-id="5fdd5-740">Обязательный</span><span class="sxs-lookup"><span data-stu-id="5fdd5-740">Is Required</span></span> | <span data-ttu-id="5fdd5-741">Значение</span><span class="sxs-lookup"><span data-stu-id="5fdd5-741">Value</span></span>                                                                                                                                                                                                                           |
|:-----------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="5fdd5-742">**имя**;</span><span class="sxs-lookup"><span data-stu-id="5fdd5-742">**Name**</span></span>         | <span data-ttu-id="5fdd5-743">Да</span><span class="sxs-lookup"><span data-stu-id="5fdd5-743">Yes</span></span>         | <span data-ttu-id="5fdd5-744">Имя параметра.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-744">The name of the parameter.</span></span>                                                                                                                                                                                                      |
| <span data-ttu-id="5fdd5-745">**Тип**</span><span class="sxs-lookup"><span data-stu-id="5fdd5-745">**Type**</span></span>         | <span data-ttu-id="5fdd5-746">Нет</span><span class="sxs-lookup"><span data-stu-id="5fdd5-746">No</span></span>          | <span data-ttu-id="5fdd5-747">Тип параметра.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-747">The parameter type.</span></span> <span data-ttu-id="5fdd5-748">Параметр может иметь любой из следующих типов (или быть коллекцией этих типов):</span><span class="sxs-lookup"><span data-stu-id="5fdd5-748">A parameter can be any of the following types (or collections of these types):</span></span> <br/> <span data-ttu-id="5fdd5-749">**EdmSimpleType**</span><span class="sxs-lookup"><span data-stu-id="5fdd5-749">**EdmSimpleType**</span></span> <br/> <span data-ttu-id="5fdd5-750">тип сущности</span><span class="sxs-lookup"><span data-stu-id="5fdd5-750">entity type</span></span> <br/> <span data-ttu-id="5fdd5-751">сложный тип</span><span class="sxs-lookup"><span data-stu-id="5fdd5-751">complex type</span></span> <br/> <span data-ttu-id="5fdd5-752">тип строки</span><span class="sxs-lookup"><span data-stu-id="5fdd5-752">row type</span></span> <br/> <span data-ttu-id="5fdd5-753">ссылочный тип</span><span class="sxs-lookup"><span data-stu-id="5fdd5-753">reference type</span></span>                             |
| <span data-ttu-id="5fdd5-754">**Допускает значения NULL**</span><span class="sxs-lookup"><span data-stu-id="5fdd5-754">**Nullable**</span></span>     | <span data-ttu-id="5fdd5-755">Нет</span><span class="sxs-lookup"><span data-stu-id="5fdd5-755">No</span></span>          | <span data-ttu-id="5fdd5-756">Значение **true** (значение по умолчанию) или **false** в зависимости от того, может ли свойство иметь значение **null** .</span><span class="sxs-lookup"><span data-stu-id="5fdd5-756">**True** (the default value) or **False** depending on whether the property can have a **null** value.</span></span>                                                                                                                          |
| <span data-ttu-id="5fdd5-757">**DefaultValue**</span><span class="sxs-lookup"><span data-stu-id="5fdd5-757">**DefaultValue**</span></span> | <span data-ttu-id="5fdd5-758">Нет</span><span class="sxs-lookup"><span data-stu-id="5fdd5-758">No</span></span>          | <span data-ttu-id="5fdd5-759">Значение по умолчанию для свойства.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-759">The default value of the property.</span></span>                                                                                                                                                                                              |
| <span data-ttu-id="5fdd5-760">**MaxLength**</span><span class="sxs-lookup"><span data-stu-id="5fdd5-760">**MaxLength**</span></span>    | <span data-ttu-id="5fdd5-761">Нет</span><span class="sxs-lookup"><span data-stu-id="5fdd5-761">No</span></span>          | <span data-ttu-id="5fdd5-762">Максимальная длина значения свойства.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-762">The maximum length of the property value.</span></span>                                                                                                                                                                                       |
| <span data-ttu-id="5fdd5-763">**FixedLength**</span><span class="sxs-lookup"><span data-stu-id="5fdd5-763">**FixedLength**</span></span>  | <span data-ttu-id="5fdd5-764">Нет</span><span class="sxs-lookup"><span data-stu-id="5fdd5-764">No</span></span>          | <span data-ttu-id="5fdd5-765">Значение **true** или **false** в зависимости от того, будет ли значение свойства храниться как строка фиксированной длины.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-765">**True** or **False** depending on whether the property value will be stored as a fixed length string.</span></span>                                                                                                                          |
| <span data-ttu-id="5fdd5-766">**Точность**</span><span class="sxs-lookup"><span data-stu-id="5fdd5-766">**Precision**</span></span>    | <span data-ttu-id="5fdd5-767">Нет</span><span class="sxs-lookup"><span data-stu-id="5fdd5-767">No</span></span>          | <span data-ttu-id="5fdd5-768">Точность значения свойства.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-768">The precision of the property value.</span></span>                                                                                                                                                                                            |
| <span data-ttu-id="5fdd5-769">**Масштабирование**</span><span class="sxs-lookup"><span data-stu-id="5fdd5-769">**Scale**</span></span>        | <span data-ttu-id="5fdd5-770">Нет</span><span class="sxs-lookup"><span data-stu-id="5fdd5-770">No</span></span>          | <span data-ttu-id="5fdd5-771">Масштаб значения свойства.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-771">The scale of the property value.</span></span>                                                                                                                                                                                                |
| <span data-ttu-id="5fdd5-772">**SRID**</span><span class="sxs-lookup"><span data-stu-id="5fdd5-772">**SRID**</span></span>         | <span data-ttu-id="5fdd5-773">Нет</span><span class="sxs-lookup"><span data-stu-id="5fdd5-773">No</span></span>          | <span data-ttu-id="5fdd5-774">Идентификатор пространственной системной ссылки.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-774">Spatial System Reference Identifier.</span></span> <span data-ttu-id="5fdd5-775">Допустимо только для свойств пространственных типов.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-775">Valid only for properties of spatial types.</span></span> <span data-ttu-id="5fdd5-776">Дополнительные сведения см. в разделе [SRID](https://en.wikipedia.org/wiki/SRID) and [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span><span class="sxs-lookup"><span data-stu-id="5fdd5-776">For more information, see [SRID](https://en.wikipedia.org/wiki/SRID) and [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span></span> |
| <span data-ttu-id="5fdd5-777">**Юникод**</span><span class="sxs-lookup"><span data-stu-id="5fdd5-777">**Unicode**</span></span>      | <span data-ttu-id="5fdd5-778">Нет</span><span class="sxs-lookup"><span data-stu-id="5fdd5-778">No</span></span>          | <span data-ttu-id="5fdd5-779">Значение **true** или **false** в зависимости от того, будет ли значение свойства храниться как строка в Юникоде.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-779">**True** or **False** depending on whether the property value will be stored as a Unicode string.</span></span>                                                                                                                               |
| <span data-ttu-id="5fdd5-780">**Параметры сортировки**</span><span class="sxs-lookup"><span data-stu-id="5fdd5-780">**Collation**</span></span>    | <span data-ttu-id="5fdd5-781">Нет</span><span class="sxs-lookup"><span data-stu-id="5fdd5-781">No</span></span>          | <span data-ttu-id="5fdd5-782">Строка, указывающая порядок сортировки, используемый в источнике данных.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-782">A string that specifies the collating sequence to be used in the data source.</span></span>                                                                                                                                                   |

 

> [!NOTE]
> <span data-ttu-id="5fdd5-783">К элементу **Parameter** можно применить любое количество атрибутов аннотации (настраиваемые XML-атрибуты).</span><span class="sxs-lookup"><span data-stu-id="5fdd5-783">Any number of annotation attributes (custom XML attributes) may be applied to the **Parameter** element.</span></span> <span data-ttu-id="5fdd5-784">Однако настраиваемые атрибуты не могут принадлежать к любому пространству имен XML, зарезервированному для CSDL.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-784">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="5fdd5-785">Полные имена любых двух настраиваемых атрибутов не могут совпадать.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-785">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

#### <a name="example"></a><span data-ttu-id="5fdd5-786">Пример</span><span class="sxs-lookup"><span data-stu-id="5fdd5-786">Example</span></span>

<span data-ttu-id="5fdd5-787">В следующем примере показан элемент **Function** , использующий один дочерний элемент **Parameter** для определения параметра функции.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-787">The following example shows a **Function** element that uses one **Parameter** child element to define a function parameter.</span></span>

``` xml
 <Function Name="GetYearsEmployed" ReturnType="Edm.Int32">
 <Parameter Name="Instructor" Type="SchoolModel.Person" />
   <DefiningExpression>
   Year(CurrentDateTime()) - Year(cast(Instructor.HireDate as DateTime))
   </DefiningExpression>
 </Function>
```

 

## <a name="principal-element-csdl"></a><span data-ttu-id="5fdd5-788">Элемент Principal (CSDL)</span><span class="sxs-lookup"><span data-stu-id="5fdd5-788">Principal Element (CSDL)</span></span>

<span data-ttu-id="5fdd5-789">Элемент **Principal** в языке CSDL является дочерним элементом для элемента ReferentialConstraint, который определяет основной элемент справочного ограничения.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-789">The **Principal** element in conceptual schema definition language (CSDL) is a child element to the ReferentialConstraint element that defines the principal end of a referential constraint.</span></span> <span data-ttu-id="5fdd5-790">Элемент **ReferentialConstraint** определяет функциональность, похожую на ограничение ссылочной целостности в реляционной базе данных.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-790">A **ReferentialConstraint** element defines functionality that is similar to a referential integrity constraint in a relational database.</span></span> <span data-ttu-id="5fdd5-791">Свойство (или свойства) типа сущности могут ссылаться на ключ сущности в другом типе сущности также, как столбец (или столбцы) в таблице базы данных могут ссылаться на первичный ключ другой таблицы.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-791">In the same way that a column (or columns) from a database table can reference the primary key of another table, a property (or properties) of an entity type can reference the entity key of another entity type.</span></span> <span data-ttu-id="5fdd5-792">Тип сущности, на который указывает ссылка, называется *основным элементом* ограничения.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-792">The entity type that is referenced is called the *principal end* of the constraint.</span></span> <span data-ttu-id="5fdd5-793">Тип сущности, который ссылается на основной элемент, называется *зависимым окончанием* ограничения.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-793">The entity type that references the principal end is called the *dependent end* of the constraint.</span></span> <span data-ttu-id="5fdd5-794">Элементы **PropertyRef** используются для указания ключей, на которые ссылается зависимый элемент.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-794">**PropertyRef** elements are used to specify which keys are referenced by the dependent end.</span></span>

<span data-ttu-id="5fdd5-795">Элемент **Principal** может иметь следующие дочерние элементы (в указанном порядке):</span><span class="sxs-lookup"><span data-stu-id="5fdd5-795">The **Principal** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="5fdd5-796">PropertyRef (один или несколько элементов)</span><span class="sxs-lookup"><span data-stu-id="5fdd5-796">PropertyRef (one or more elements)</span></span>
-   <span data-ttu-id="5fdd5-797">Элементы Annotation (ноль или более элементов)</span><span class="sxs-lookup"><span data-stu-id="5fdd5-797">Annotation elements (zero or more elements)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="5fdd5-798">Применимые атрибуты</span><span class="sxs-lookup"><span data-stu-id="5fdd5-798">Applicable Attributes</span></span>

<span data-ttu-id="5fdd5-799">В следующей таблице описаны атрибуты, которые можно применить к элементу **Principal** .</span><span class="sxs-lookup"><span data-stu-id="5fdd5-799">The table below describes the attributes that can be applied to the **Principal** element.</span></span>

| <span data-ttu-id="5fdd5-800">Имя атрибута</span><span class="sxs-lookup"><span data-stu-id="5fdd5-800">Attribute Name</span></span> | <span data-ttu-id="5fdd5-801">Обязательный</span><span class="sxs-lookup"><span data-stu-id="5fdd5-801">Is Required</span></span> | <span data-ttu-id="5fdd5-802">Значение</span><span class="sxs-lookup"><span data-stu-id="5fdd5-802">Value</span></span>                                                                |
|:---------------|:------------|:---------------------------------------------------------------------|
| <span data-ttu-id="5fdd5-803">**Роль**</span><span class="sxs-lookup"><span data-stu-id="5fdd5-803">**Role**</span></span>       | <span data-ttu-id="5fdd5-804">Да</span><span class="sxs-lookup"><span data-stu-id="5fdd5-804">Yes</span></span>         | <span data-ttu-id="5fdd5-805">Имя типа сущности в основном конце ассоциации.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-805">The name of the entity type on the principal end of the association.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="5fdd5-806">К **основному** элементу можно применить любое количество атрибутов аннотации (настраиваемые XML-атрибуты).</span><span class="sxs-lookup"><span data-stu-id="5fdd5-806">Any number of annotation attributes (custom XML attributes) may be applied to the **Principal** element.</span></span> <span data-ttu-id="5fdd5-807">Однако настраиваемые атрибуты не могут принадлежать к любому пространству имен XML, зарезервированному для CSDL.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-807">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="5fdd5-808">Полные имена любых двух настраиваемых атрибутов не могут совпадать.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-808">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="5fdd5-809">Пример</span><span class="sxs-lookup"><span data-stu-id="5fdd5-809">Example</span></span>

<span data-ttu-id="5fdd5-810">В следующем примере показан элемент **ReferentialConstraint** , который является частью определения ассоциации **публишедби** .</span><span class="sxs-lookup"><span data-stu-id="5fdd5-810">The following example shows a **ReferentialConstraint** element that is part of the definition of the **PublishedBy** association.</span></span> <span data-ttu-id="5fdd5-811">Свойство **ID** типа сущности **издателя** образует основной конец справочного ограничения.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-811">The **Id** property of the **Publisher** entity type makes up the principal end of the referential constraint.</span></span>

``` xml
 <Association Name="PublishedBy">
   <End Type="BooksModel.Book" Role="Book" Multiplicity="*" >
   </End>
   <End Type="BooksModel.Publisher" Role="Publisher" Multiplicity="1" />
   <ReferentialConstraint>
     <Principal Role="Publisher">
       <PropertyRef Name="Id" />
     </Principal>
     <Dependent Role="Book">
       <PropertyRef Name="PublisherId" />
     </Dependent>
   </ReferentialConstraint>
 </Association>
```
 

 

## <a name="property-element-csdl"></a><span data-ttu-id="5fdd5-812">Элемент Property (CSDL)</span><span class="sxs-lookup"><span data-stu-id="5fdd5-812">Property Element (CSDL)</span></span>

<span data-ttu-id="5fdd5-813">Элемент **Property** в языке CSDL может быть дочерним по отношению к элементу EntityType, элементу complexType или элементу RowType.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-813">The **Property** element in conceptual schema definition language (CSDL) can be a child of the EntityType element, the ComplexType element, or the RowType element.</span></span>

### <a name="entitytype-and-complextype-element-applications"></a><span data-ttu-id="5fdd5-814">Применение элементов EntityType и ComplexType</span><span class="sxs-lookup"><span data-stu-id="5fdd5-814">EntityType and ComplexType Element Applications</span></span>

<span data-ttu-id="5fdd5-815">Элементы **свойств** (как дочерние элементы **EntityType** или **complexType** ) определяют форму и характеристики данных, которые будут содержаться в экземпляре типа сущности или экземпляра сложного типа.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-815">**Property** elements (as children of **EntityType** or **ComplexType** elements) define the shape and characteristics of data that an entity type instance or complex type instance will contain.</span></span> <span data-ttu-id="5fdd5-816">Свойства в концептуальной модели аналогичны свойствам, которые определены в классе.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-816">Properties in a conceptual model are analogous to properties that are defined on a class.</span></span> <span data-ttu-id="5fdd5-817">По такому же принципу, как свойства, относящиеся к классу, определяют форму класса и несут информацию об объектах, свойства в концептуальной модели определяют форму типа сущности и несут информацию об экземплярах типа сущности.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-817">In the same way that properties on a class define the shape of the class and carry information about objects, properties in a conceptual model define the shape of an entity type and carry information about entity type instances.</span></span>

<span data-ttu-id="5fdd5-818">Элемент **Property** может иметь следующие дочерние элементы (в указанном порядке):</span><span class="sxs-lookup"><span data-stu-id="5fdd5-818">The **Property** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="5fdd5-819">Элемент Documentation (допускается ровно один элемент либо элемент может отсутствовать)</span><span class="sxs-lookup"><span data-stu-id="5fdd5-819">Documentation Element (zero or one elements allowed)</span></span>
-   <span data-ttu-id="5fdd5-820">Элементы Annotation (допустимое количество элементов — ноль или больше)</span><span class="sxs-lookup"><span data-stu-id="5fdd5-820">Annotation elements (zero or more elements allowed)</span></span>

<span data-ttu-id="5fdd5-821">Следующие аспекты могут быть применены к элементу **Свойства** : **Nullable**, **DefaultValue**, **MaxLength**, **FixedLength**, **Precision**, **ScaleY**, **Unicode**, **collation**, **ConcurrencyMode**.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-821">The following facets can be applied to a **Property** element: **Nullable**, **DefaultValue**, **MaxLength**, **FixedLength**, **Precision**, **Scale**, **Unicode**, **Collation**, **ConcurrencyMode**.</span></span> <span data-ttu-id="5fdd5-822">Аспекты представляют собой атрибуты XML, которые предоставляют сведения о том, как значения свойств хранятся в хранилище данных.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-822">Facets are XML attributes that provide information about how property values are stored in the data store.</span></span>

> [!NOTE]
> <span data-ttu-id="5fdd5-823">Аспекты можно применять только к свойствам типа **EDMSimpleType**.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-823">Facets can only be applied to properties of type **EDMSimpleType**.</span></span>

 

#### <a name="applicable-attributes"></a><span data-ttu-id="5fdd5-824">Применимые атрибуты</span><span class="sxs-lookup"><span data-stu-id="5fdd5-824">Applicable Attributes</span></span>

<span data-ttu-id="5fdd5-825">В следующей таблице описаны атрибуты, которые можно применить к элементу **Property** .</span><span class="sxs-lookup"><span data-stu-id="5fdd5-825">The following table describes the attributes that can be applied to the **Property** element.</span></span>

| <span data-ttu-id="5fdd5-826">Имя атрибута</span><span class="sxs-lookup"><span data-stu-id="5fdd5-826">Attribute Name</span></span>                                                         | <span data-ttu-id="5fdd5-827">Обязательный</span><span class="sxs-lookup"><span data-stu-id="5fdd5-827">Is Required</span></span> | <span data-ttu-id="5fdd5-828">Значение</span><span class="sxs-lookup"><span data-stu-id="5fdd5-828">Value</span></span>                                                                                                                                                                                                                           |
|:-----------------------------------------------------------------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="5fdd5-829">**имя**;</span><span class="sxs-lookup"><span data-stu-id="5fdd5-829">**Name**</span></span>                                                               | <span data-ttu-id="5fdd5-830">Да</span><span class="sxs-lookup"><span data-stu-id="5fdd5-830">Yes</span></span>         | <span data-ttu-id="5fdd5-831">Имя свойства.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-831">The name of the property.</span></span>                                                                                                                                                                                                       |
| <span data-ttu-id="5fdd5-832">**Тип**</span><span class="sxs-lookup"><span data-stu-id="5fdd5-832">**Type**</span></span>                                                               | <span data-ttu-id="5fdd5-833">Да</span><span class="sxs-lookup"><span data-stu-id="5fdd5-833">Yes</span></span>         | <span data-ttu-id="5fdd5-834">Тип значения свойства.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-834">The type of the property value.</span></span> <span data-ttu-id="5fdd5-835">Тип значения свойства должен представлять собой **EDMSimpleType** или сложный тип (указано в полном имени) в рамках модели.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-835">The property value type must be an **EDMSimpleType** or a complex type (indicated by a fully-qualified name) that is within scope of the model.</span></span>                                                 |
| <span data-ttu-id="5fdd5-836">**Допускает значения NULL**</span><span class="sxs-lookup"><span data-stu-id="5fdd5-836">**Nullable**</span></span>                                                           | <span data-ttu-id="5fdd5-837">Нет</span><span class="sxs-lookup"><span data-stu-id="5fdd5-837">No</span></span>          | <span data-ttu-id="5fdd5-838">**True** (значение по умолчанию) или <strong>False</strong> в зависимости от того, может ли свойство иметь значение null.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-838">**True** (the default value) or <strong>False</strong> depending on whether the property can have a null value.</span></span> <br/> [!NOTE]                                                                                                   |
| <span data-ttu-id="5fdd5-839">> в CSDL v1 свойство сложного типа должно иметь `Nullable="False"` .</span><span class="sxs-lookup"><span data-stu-id="5fdd5-839">> In the CSDL v1 a complex type property must have `Nullable="False"`.</span></span> |             |                                                                                                                                                                                                                                 |
| <span data-ttu-id="5fdd5-840">**DefaultValue**</span><span class="sxs-lookup"><span data-stu-id="5fdd5-840">**DefaultValue**</span></span>                                                       | <span data-ttu-id="5fdd5-841">Нет</span><span class="sxs-lookup"><span data-stu-id="5fdd5-841">No</span></span>          | <span data-ttu-id="5fdd5-842">Значение по умолчанию для свойства.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-842">The default value of the property.</span></span>                                                                                                                                                                                              |
| <span data-ttu-id="5fdd5-843">**MaxLength**</span><span class="sxs-lookup"><span data-stu-id="5fdd5-843">**MaxLength**</span></span>                                                          | <span data-ttu-id="5fdd5-844">Нет</span><span class="sxs-lookup"><span data-stu-id="5fdd5-844">No</span></span>          | <span data-ttu-id="5fdd5-845">Максимальная длина значения свойства.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-845">The maximum length of the property value.</span></span>                                                                                                                                                                                       |
| <span data-ttu-id="5fdd5-846">**FixedLength**</span><span class="sxs-lookup"><span data-stu-id="5fdd5-846">**FixedLength**</span></span>                                                        | <span data-ttu-id="5fdd5-847">Нет</span><span class="sxs-lookup"><span data-stu-id="5fdd5-847">No</span></span>          | <span data-ttu-id="5fdd5-848">Значение **true** или **false** в зависимости от того, будет ли значение свойства храниться как строка фиксированной длины.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-848">**True** or **False** depending on whether the property value will be stored as a fixed length string.</span></span>                                                                                                                          |
| <span data-ttu-id="5fdd5-849">**Точность**</span><span class="sxs-lookup"><span data-stu-id="5fdd5-849">**Precision**</span></span>                                                          | <span data-ttu-id="5fdd5-850">Нет</span><span class="sxs-lookup"><span data-stu-id="5fdd5-850">No</span></span>          | <span data-ttu-id="5fdd5-851">Точность значения свойства.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-851">The precision of the property value.</span></span>                                                                                                                                                                                            |
| <span data-ttu-id="5fdd5-852">**Масштабирование**</span><span class="sxs-lookup"><span data-stu-id="5fdd5-852">**Scale**</span></span>                                                              | <span data-ttu-id="5fdd5-853">Нет</span><span class="sxs-lookup"><span data-stu-id="5fdd5-853">No</span></span>          | <span data-ttu-id="5fdd5-854">Масштаб значения свойства.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-854">The scale of the property value.</span></span>                                                                                                                                                                                                |
| <span data-ttu-id="5fdd5-855">**SRID**</span><span class="sxs-lookup"><span data-stu-id="5fdd5-855">**SRID**</span></span>                                                               | <span data-ttu-id="5fdd5-856">Нет</span><span class="sxs-lookup"><span data-stu-id="5fdd5-856">No</span></span>          | <span data-ttu-id="5fdd5-857">Идентификатор пространственной системной ссылки.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-857">Spatial System Reference Identifier.</span></span> <span data-ttu-id="5fdd5-858">Допустимо только для свойств пространственных типов.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-858">Valid only for properties of spatial types.</span></span> <span data-ttu-id="5fdd5-859">Дополнительные сведения см. в разделе [SRID](https://en.wikipedia.org/wiki/SRID) and [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span><span class="sxs-lookup"><span data-stu-id="5fdd5-859">For more information, see [SRID](https://en.wikipedia.org/wiki/SRID) and [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span></span> |
| <span data-ttu-id="5fdd5-860">**Юникод**</span><span class="sxs-lookup"><span data-stu-id="5fdd5-860">**Unicode**</span></span>                                                            | <span data-ttu-id="5fdd5-861">Нет</span><span class="sxs-lookup"><span data-stu-id="5fdd5-861">No</span></span>          | <span data-ttu-id="5fdd5-862">Значение **true** или **false** в зависимости от того, будет ли значение свойства храниться как строка в Юникоде.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-862">**True** or **False** depending on whether the property value will be stored as a Unicode string.</span></span>                                                                                                                               |
| <span data-ttu-id="5fdd5-863">**Параметры сортировки**</span><span class="sxs-lookup"><span data-stu-id="5fdd5-863">**Collation**</span></span>                                                          | <span data-ttu-id="5fdd5-864">Нет</span><span class="sxs-lookup"><span data-stu-id="5fdd5-864">No</span></span>          | <span data-ttu-id="5fdd5-865">Строка, указывающая порядок сортировки, используемый в источнике данных.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-865">A string that specifies the collating sequence to be used in the data source.</span></span>                                                                                                                                                   |
| <span data-ttu-id="5fdd5-866">**ConcurrencyMode**</span><span class="sxs-lookup"><span data-stu-id="5fdd5-866">**ConcurrencyMode**</span></span>                                                    | <span data-ttu-id="5fdd5-867">Нет</span><span class="sxs-lookup"><span data-stu-id="5fdd5-867">No</span></span>          | <span data-ttu-id="5fdd5-868">**None** (значение по умолчанию) или **Fixed**.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-868">**None** (the default value) or **Fixed**.</span></span> <span data-ttu-id="5fdd5-869">Если задано значение **Fixed**, значение свойства будет использоваться при выполнении проверок оптимистичного параллелизма.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-869">If the value is set to **Fixed**, the property value will be used in optimistic concurrency checks.</span></span>                                                                                  |

 

> [!NOTE]
> <span data-ttu-id="5fdd5-870">К элементу **Свойства** можно применить любое количество атрибутов аннотации (настраиваемые XML-атрибуты).</span><span class="sxs-lookup"><span data-stu-id="5fdd5-870">Any number of annotation attributes (custom XML attributes) may be applied to the **Property** element.</span></span> <span data-ttu-id="5fdd5-871">Однако настраиваемые атрибуты не могут принадлежать к любому пространству имен XML, зарезервированному для CSDL.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-871">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="5fdd5-872">Полные имена любых двух настраиваемых атрибутов не могут совпадать.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-872">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

#### <a name="example"></a><span data-ttu-id="5fdd5-873">Пример</span><span class="sxs-lookup"><span data-stu-id="5fdd5-873">Example</span></span>

<span data-ttu-id="5fdd5-874">В следующем примере показан элемент **EntityType** с тремя элементами **свойств** :</span><span class="sxs-lookup"><span data-stu-id="5fdd5-874">The following example shows an **EntityType** element with three **Property** elements:</span></span>

``` xml
 <EntityType Name="Book">
   <Key>
     <PropertyRef Name="ISBN" />
   </Key>
   <Property Type="String" Name="ISBN" Nullable="false" />
   <Property Type="String" Name="Title" Nullable="false" />
   <Property Type="Decimal" Name="Revision" Nullable="false" Precision="29" Scale="29" />
   <NavigationProperty Name="Publisher" Relationship="BooksModel.PublishedBy"
                       FromRole="Book" ToRole="Publisher" />
   <NavigationProperty Name="Authors" Relationship="BooksModel.WrittenBy"
                       FromRole="Book" ToRole="Author" />
 </EntityType>
```
 

<span data-ttu-id="5fdd5-875">В следующем примере показан элемент **complexType** с пятью элементами **свойств** :</span><span class="sxs-lookup"><span data-stu-id="5fdd5-875">The following example shows a **ComplexType** element with five **Property** elements:</span></span>

``` xml
 <ComplexType Name="Address" >
   <Property Type="String" Name="StreetAddress" Nullable="false" />
   <Property Type="String" Name="City" Nullable="false" />
   <Property Type="String" Name="StateOrProvince" Nullable="false" />
   <Property Type="String" Name="Country" Nullable="false" />
   <Property Type="String" Name="PostalCode" Nullable="false" />
 </ComplexType>
```
 

### <a name="rowtype-element-application"></a><span data-ttu-id="5fdd5-876">Применение элемента RowType</span><span class="sxs-lookup"><span data-stu-id="5fdd5-876">RowType Element Application</span></span>

<span data-ttu-id="5fdd5-877">Элементы **свойств** (как дочерние элементы элемента **RowType** ) определяют форму и характеристики данных, которые могут быть переданы или возвращены из определяемой моделью функции.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-877">**Property** elements (as the children of a **RowType** element) define the shape and characteristics of data that can be passed to or returned from a model-defined function.</span></span>  

<span data-ttu-id="5fdd5-878">Элемент **Property** может иметь только один из следующих дочерних элементов:</span><span class="sxs-lookup"><span data-stu-id="5fdd5-878">The **Property** element can have exactly one of the following child elements:</span></span>

-   <span data-ttu-id="5fdd5-879">CollectionType</span><span class="sxs-lookup"><span data-stu-id="5fdd5-879">CollectionType</span></span>
-   <span data-ttu-id="5fdd5-880">Тип ссылки</span><span class="sxs-lookup"><span data-stu-id="5fdd5-880">ReferenceType</span></span>
-   <span data-ttu-id="5fdd5-881">RowType</span><span class="sxs-lookup"><span data-stu-id="5fdd5-881">RowType</span></span>

<span data-ttu-id="5fdd5-882">Элемент **Property** может иметь любое число дочерних элементов аннотации.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-882">The **Property** element can have any number child annotation elements.</span></span>

> [!NOTE]
> <span data-ttu-id="5fdd5-883">Элементы аннотации разрешены только в CSDL v2 и более поздних версиях.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-883">Annotation elements are only allowed in CSDL v2 and later.</span></span>

 

#### <a name="applicable-attributes"></a><span data-ttu-id="5fdd5-884">Применимые атрибуты</span><span class="sxs-lookup"><span data-stu-id="5fdd5-884">Applicable Attributes</span></span>

<span data-ttu-id="5fdd5-885">В следующей таблице описаны атрибуты, которые можно применить к элементу **Property** .</span><span class="sxs-lookup"><span data-stu-id="5fdd5-885">The following table describes the attributes that can be applied to the **Property** element.</span></span>

| <span data-ttu-id="5fdd5-886">Имя атрибута</span><span class="sxs-lookup"><span data-stu-id="5fdd5-886">Attribute Name</span></span>                                                     | <span data-ttu-id="5fdd5-887">Обязательный</span><span class="sxs-lookup"><span data-stu-id="5fdd5-887">Is Required</span></span> | <span data-ttu-id="5fdd5-888">Значение</span><span class="sxs-lookup"><span data-stu-id="5fdd5-888">Value</span></span>                                                                                                                                                                                                                           |
|:-------------------------------------------------------------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="5fdd5-889">**имя**;</span><span class="sxs-lookup"><span data-stu-id="5fdd5-889">**Name**</span></span>                                                           | <span data-ttu-id="5fdd5-890">Да</span><span class="sxs-lookup"><span data-stu-id="5fdd5-890">Yes</span></span>         | <span data-ttu-id="5fdd5-891">Имя свойства.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-891">The name of the property.</span></span>                                                                                                                                                                                                       |
| <span data-ttu-id="5fdd5-892">**Тип**</span><span class="sxs-lookup"><span data-stu-id="5fdd5-892">**Type**</span></span>                                                           | <span data-ttu-id="5fdd5-893">Да</span><span class="sxs-lookup"><span data-stu-id="5fdd5-893">Yes</span></span>         | <span data-ttu-id="5fdd5-894">Тип значения свойства.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-894">The type of the property value.</span></span>                                                                                                                                                                                                 |
| <span data-ttu-id="5fdd5-895">**Допускает значения NULL**</span><span class="sxs-lookup"><span data-stu-id="5fdd5-895">**Nullable**</span></span>                                                       | <span data-ttu-id="5fdd5-896">Нет</span><span class="sxs-lookup"><span data-stu-id="5fdd5-896">No</span></span>          | <span data-ttu-id="5fdd5-897">**True** (значение по умолчанию) или **False** в зависимости от того, может ли свойство иметь значение null.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-897">**True** (the default value) or **False** depending on whether the property can have a null value.</span></span> <br/> [!NOTE]                                                                                                                |
| <span data-ttu-id="5fdd5-898">> в CSDL v1 свойство сложного типа должно иметь значение `Nullable="False"` .</span><span class="sxs-lookup"><span data-stu-id="5fdd5-898">> In CSDL v1 a complex type property must have `Nullable="False"`.</span></span> |             |                                                                                                                                                                                                                                 |
| <span data-ttu-id="5fdd5-899">**DefaultValue**</span><span class="sxs-lookup"><span data-stu-id="5fdd5-899">**DefaultValue**</span></span>                                                   | <span data-ttu-id="5fdd5-900">Нет</span><span class="sxs-lookup"><span data-stu-id="5fdd5-900">No</span></span>          | <span data-ttu-id="5fdd5-901">Значение по умолчанию для свойства.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-901">The default value of the property.</span></span>                                                                                                                                                                                              |
| <span data-ttu-id="5fdd5-902">**MaxLength**</span><span class="sxs-lookup"><span data-stu-id="5fdd5-902">**MaxLength**</span></span>                                                      | <span data-ttu-id="5fdd5-903">Нет</span><span class="sxs-lookup"><span data-stu-id="5fdd5-903">No</span></span>          | <span data-ttu-id="5fdd5-904">Максимальная длина значения свойства.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-904">The maximum length of the property value.</span></span>                                                                                                                                                                                       |
| <span data-ttu-id="5fdd5-905">**FixedLength**</span><span class="sxs-lookup"><span data-stu-id="5fdd5-905">**FixedLength**</span></span>                                                    | <span data-ttu-id="5fdd5-906">Нет</span><span class="sxs-lookup"><span data-stu-id="5fdd5-906">No</span></span>          | <span data-ttu-id="5fdd5-907">Значение **true** или **false** в зависимости от того, будет ли значение свойства храниться как строка фиксированной длины.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-907">**True** or **False** depending on whether the property value will be stored as a fixed length string.</span></span>                                                                                                                          |
| <span data-ttu-id="5fdd5-908">**Точность**</span><span class="sxs-lookup"><span data-stu-id="5fdd5-908">**Precision**</span></span>                                                      | <span data-ttu-id="5fdd5-909">Нет</span><span class="sxs-lookup"><span data-stu-id="5fdd5-909">No</span></span>          | <span data-ttu-id="5fdd5-910">Точность значения свойства.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-910">The precision of the property value.</span></span>                                                                                                                                                                                            |
| <span data-ttu-id="5fdd5-911">**Масштабирование**</span><span class="sxs-lookup"><span data-stu-id="5fdd5-911">**Scale**</span></span>                                                          | <span data-ttu-id="5fdd5-912">Нет</span><span class="sxs-lookup"><span data-stu-id="5fdd5-912">No</span></span>          | <span data-ttu-id="5fdd5-913">Масштаб значения свойства.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-913">The scale of the property value.</span></span>                                                                                                                                                                                                |
| <span data-ttu-id="5fdd5-914">**SRID**</span><span class="sxs-lookup"><span data-stu-id="5fdd5-914">**SRID**</span></span>                                                           | <span data-ttu-id="5fdd5-915">Нет</span><span class="sxs-lookup"><span data-stu-id="5fdd5-915">No</span></span>          | <span data-ttu-id="5fdd5-916">Идентификатор пространственной системной ссылки.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-916">Spatial System Reference Identifier.</span></span> <span data-ttu-id="5fdd5-917">Допустимо только для свойств пространственных типов.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-917">Valid only for properties of spatial types.</span></span> <span data-ttu-id="5fdd5-918">Дополнительные сведения см. в разделе [SRID](https://en.wikipedia.org/wiki/SRID) and [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span><span class="sxs-lookup"><span data-stu-id="5fdd5-918">For more information, see [SRID](https://en.wikipedia.org/wiki/SRID) and [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span></span> |
| <span data-ttu-id="5fdd5-919">**Юникод**</span><span class="sxs-lookup"><span data-stu-id="5fdd5-919">**Unicode**</span></span>                                                        | <span data-ttu-id="5fdd5-920">Нет</span><span class="sxs-lookup"><span data-stu-id="5fdd5-920">No</span></span>          | <span data-ttu-id="5fdd5-921">Значение **true** или **false** в зависимости от того, будет ли значение свойства храниться как строка в Юникоде.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-921">**True** or **False** depending on whether the property value will be stored as a Unicode string.</span></span>                                                                                                                               |
| <span data-ttu-id="5fdd5-922">**Параметры сортировки**</span><span class="sxs-lookup"><span data-stu-id="5fdd5-922">**Collation**</span></span>                                                      | <span data-ttu-id="5fdd5-923">Нет</span><span class="sxs-lookup"><span data-stu-id="5fdd5-923">No</span></span>          | <span data-ttu-id="5fdd5-924">Строка, указывающая порядок сортировки, используемый в источнике данных.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-924">A string that specifies the collating sequence to be used in the data source.</span></span>                                                                                                                                                   |

 

> [!NOTE]
> <span data-ttu-id="5fdd5-925">К элементу **Свойства** можно применить любое количество атрибутов аннотации (настраиваемые XML-атрибуты).</span><span class="sxs-lookup"><span data-stu-id="5fdd5-925">Any number of annotation attributes (custom XML attributes) may be applied to the **Property** element.</span></span> <span data-ttu-id="5fdd5-926">Однако настраиваемые атрибуты не могут принадлежать к любому пространству имен XML, зарезервированному для CSDL.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-926">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="5fdd5-927">Полные имена любых двух настраиваемых атрибутов не могут совпадать.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-927">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

#### <a name="example"></a><span data-ttu-id="5fdd5-928">Пример</span><span class="sxs-lookup"><span data-stu-id="5fdd5-928">Example</span></span>

<span data-ttu-id="5fdd5-929">В следующем примере показаны элементы **свойств** , используемые для определения формы возвращаемого типа функции, определяемой моделью.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-929">The following example shows **Property** elements used to define the shape of the return type of a model-defined function.</span></span>

``` xml
 <Function Name="LastNamesAfter">
   <Parameter Name="someString" Type="Edm.String" />
   <ReturnType>
    <CollectionType>
      <RowType>
        <Property Name="FirstName" Type="Edm.String" Nullable="false" />
        <Property Name="LastName" Type="Edm.String" Nullable="false" />
      </RowType>
    </CollectionType>
   </ReturnType>
   <DefiningExpression>
             SELECT VALUE ROW(p.FirstName, p.LastName)
             FROM SchoolEntities.People AS p
             WHERE p.LastName &gt;= somestring
   </DefiningExpression>
 </Function>
```
 

 

## <a name="propertyref-element-csdl"></a><span data-ttu-id="5fdd5-930">Элемент PropertyRef (CSDL)</span><span class="sxs-lookup"><span data-stu-id="5fdd5-930">PropertyRef Element (CSDL)</span></span>

<span data-ttu-id="5fdd5-931">Элемент **PropertyRef** в языке CSDL ссылается на свойство типа сущности, чтобы указать, что свойство будет выполнять одну из следующих ролей:</span><span class="sxs-lookup"><span data-stu-id="5fdd5-931">The **PropertyRef** element in conceptual schema definition language (CSDL) references a property of an entity type to indicate that the property will perform one of the following roles:</span></span>

-   <span data-ttu-id="5fdd5-932">Часть ключа сущности (является свойством или набором свойств типа сущности, которые определяют идентификатор).</span><span class="sxs-lookup"><span data-stu-id="5fdd5-932">Part of the entity's key (a property or a set of properties of an entity type that determine identity).</span></span> <span data-ttu-id="5fdd5-933">Для определения ключа сущности можно использовать один или несколько элементов **PropertyRef** .</span><span class="sxs-lookup"><span data-stu-id="5fdd5-933">One or more **PropertyRef** elements can be used to define an entity key.</span></span>
-   <span data-ttu-id="5fdd5-934">Зависимый или основной конец справочного ограничения.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-934">The dependent or principal end of a referential constraint.</span></span>

<span data-ttu-id="5fdd5-935">Элемент **PropertyRef** может иметь только элементы аннотации (ноль или более) в качестве дочерних элементов.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-935">The **PropertyRef** element can only have annotation elements (zero or more) as child elements.</span></span>

> [!NOTE]
> <span data-ttu-id="5fdd5-936">Элементы аннотации разрешены только в CSDL v2 и более поздних версиях.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-936">Annotation elements are only allowed in CSDL v2 and later.</span></span>

 

### <a name="applicable-attributes"></a><span data-ttu-id="5fdd5-937">Применимые атрибуты</span><span class="sxs-lookup"><span data-stu-id="5fdd5-937">Applicable Attributes</span></span>

<span data-ttu-id="5fdd5-938">В следующей таблице описаны атрибуты, которые можно применить к элементу **PropertyRef** .</span><span class="sxs-lookup"><span data-stu-id="5fdd5-938">The table below describes the attributes that can be applied to the **PropertyRef** element.</span></span>

| <span data-ttu-id="5fdd5-939">Имя атрибута</span><span class="sxs-lookup"><span data-stu-id="5fdd5-939">Attribute Name</span></span> | <span data-ttu-id="5fdd5-940">Обязательный</span><span class="sxs-lookup"><span data-stu-id="5fdd5-940">Is Required</span></span> | <span data-ttu-id="5fdd5-941">Значение</span><span class="sxs-lookup"><span data-stu-id="5fdd5-941">Value</span></span>                                |
|:---------------|:------------|:-------------------------------------|
| <span data-ttu-id="5fdd5-942">**имя**;</span><span class="sxs-lookup"><span data-stu-id="5fdd5-942">**Name**</span></span>       | <span data-ttu-id="5fdd5-943">Да</span><span class="sxs-lookup"><span data-stu-id="5fdd5-943">Yes</span></span>         | <span data-ttu-id="5fdd5-944">Имя свойства, на которое дается ссылка.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-944">The name of the referenced property.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="5fdd5-945">К элементу **PropertyRef** можно применить любое количество атрибутов аннотации (настраиваемые XML-атрибуты).</span><span class="sxs-lookup"><span data-stu-id="5fdd5-945">Any number of annotation attributes (custom XML attributes) may be applied to the **PropertyRef** element.</span></span> <span data-ttu-id="5fdd5-946">Однако настраиваемые атрибуты не могут принадлежать к любому пространству имен XML, зарезервированному для CSDL.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-946">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="5fdd5-947">Полные имена любых двух настраиваемых атрибутов не могут совпадать.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-947">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="5fdd5-948">Пример</span><span class="sxs-lookup"><span data-stu-id="5fdd5-948">Example</span></span>

<span data-ttu-id="5fdd5-949">В приведенном ниже примере определяется тип сущности (**Книга**).</span><span class="sxs-lookup"><span data-stu-id="5fdd5-949">The example below defines an entity type (**Book**).</span></span> <span data-ttu-id="5fdd5-950">Ключ сущности определяется с помощью ссылки на свойство **ISBN** типа сущности.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-950">The entity key is defined by referencing the **ISBN** property of the entity type.</span></span>

``` xml
 <EntityType Name="Book">
   <Key>
     <PropertyRef Name="ISBN" />
   </Key>
   <Property Type="String" Name="ISBN" Nullable="false" />
   <Property Type="String" Name="Title" Nullable="false" />
   <Property Type="Decimal" Name="Revision" Nullable="false" Precision="29" Scale="29" />
   <NavigationProperty Name="Publisher" Relationship="BooksModel.PublishedBy"
                       FromRole="Book" ToRole="Publisher" />
   <NavigationProperty Name="Authors" Relationship="BooksModel.WrittenBy"
                       FromRole="Book" ToRole="Author" />
 </EntityType>
```
 

<span data-ttu-id="5fdd5-951">В следующем примере используются два элемента **PropertyRef** для указания того, что два свойства (**ID** и **PublisherID**) являются основными и зависимыми элементами ссылочного ограничения.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-951">In the next example, two **PropertyRef** elements are used to indicate that two properties (**Id** and **PublisherId**) are the principal and dependent ends of a referential constraint.</span></span>

``` xml
 <Association Name="PublishedBy">
   <End Type="BooksModel.Book" Role="Book" Multiplicity="*" >
   </End>
   <End Type="BooksModel.Publisher" Role="Publisher" Multiplicity="1" />
   <ReferentialConstraint>
     <Principal Role="Publisher">
       <PropertyRef Name="Id" />
     </Principal>
     <Dependent Role="Book">
       <PropertyRef Name="PublisherId" />
     </Dependent>
   </ReferentialConstraint>
 </Association>
```
 

 

## <a name="referencetype-element-csdl"></a><span data-ttu-id="5fdd5-952">Элемент ReferenceType (CSDL)</span><span class="sxs-lookup"><span data-stu-id="5fdd5-952">ReferenceType Element (CSDL)</span></span>

<span data-ttu-id="5fdd5-953">Элемент **ReferenceType** в языке CSDL указывает ссылку на тип сущности.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-953">The **ReferenceType** element in conceptual schema definition language (CSDL) specifies a reference to an entity type.</span></span> <span data-ttu-id="5fdd5-954">Элемент **ReferenceType** может быть дочерним по отношению к следующим элементам:</span><span class="sxs-lookup"><span data-stu-id="5fdd5-954">The **ReferenceType** element can be a child of the following elements:</span></span>

-   <span data-ttu-id="5fdd5-955">ReturnType (функция)</span><span class="sxs-lookup"><span data-stu-id="5fdd5-955">ReturnType (Function)</span></span>
-   <span data-ttu-id="5fdd5-956">Параметр</span><span class="sxs-lookup"><span data-stu-id="5fdd5-956">Parameter</span></span>
-   <span data-ttu-id="5fdd5-957">CollectionType</span><span class="sxs-lookup"><span data-stu-id="5fdd5-957">CollectionType</span></span>

<span data-ttu-id="5fdd5-958">Элемент **ReferenceType** используется при определении параметра или возвращаемого типа для функции.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-958">The **ReferenceType** element is used when defining a parameter or return type for a function.</span></span>

<span data-ttu-id="5fdd5-959">Элемент **ReferenceType** может иметь следующие дочерние элементы (в указанном порядке):</span><span class="sxs-lookup"><span data-stu-id="5fdd5-959">A **ReferenceType** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="5fdd5-960">Documentation (ноль или один элемент)</span><span class="sxs-lookup"><span data-stu-id="5fdd5-960">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="5fdd5-961">Элементы Annotation (ноль или более элементов)</span><span class="sxs-lookup"><span data-stu-id="5fdd5-961">Annotation elements (zero or more elements)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="5fdd5-962">Применимые атрибуты</span><span class="sxs-lookup"><span data-stu-id="5fdd5-962">Applicable Attributes</span></span>

<span data-ttu-id="5fdd5-963">В следующей таблице описаны атрибуты, которые можно применить к элементу **ReferenceType** .</span><span class="sxs-lookup"><span data-stu-id="5fdd5-963">The table below describes the attributes that can be applied to the **ReferenceType** element.</span></span>

| <span data-ttu-id="5fdd5-964">Имя атрибута</span><span class="sxs-lookup"><span data-stu-id="5fdd5-964">Attribute Name</span></span> | <span data-ttu-id="5fdd5-965">Обязательный</span><span class="sxs-lookup"><span data-stu-id="5fdd5-965">Is Required</span></span> | <span data-ttu-id="5fdd5-966">Значение</span><span class="sxs-lookup"><span data-stu-id="5fdd5-966">Value</span></span>                                         |
|:---------------|:------------|:----------------------------------------------|
| <span data-ttu-id="5fdd5-967">**Тип**</span><span class="sxs-lookup"><span data-stu-id="5fdd5-967">**Type**</span></span>       | <span data-ttu-id="5fdd5-968">Да</span><span class="sxs-lookup"><span data-stu-id="5fdd5-968">Yes</span></span>         | <span data-ttu-id="5fdd5-969">Имя типа сущности, на который делается ссылка.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-969">The name of the entity type being referenced.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="5fdd5-970">К элементу **ReferenceType** можно применить любое количество атрибутов аннотации (настраиваемые XML-атрибуты).</span><span class="sxs-lookup"><span data-stu-id="5fdd5-970">Any number of annotation attributes (custom XML attributes) may be applied to the **ReferenceType** element.</span></span> <span data-ttu-id="5fdd5-971">Однако настраиваемые атрибуты не могут принадлежать к любому пространству имен XML, зарезервированному для CSDL.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-971">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="5fdd5-972">Полные имена любых двух настраиваемых атрибутов не могут совпадать.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-972">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="5fdd5-973">Пример</span><span class="sxs-lookup"><span data-stu-id="5fdd5-973">Example</span></span>

<span data-ttu-id="5fdd5-974">В следующем примере показан элемент **ReferenceType** , используемый в качестве дочернего элемента **параметра** в определяемой моделью функции, которая принимает ссылку на тип сущности **Person** :</span><span class="sxs-lookup"><span data-stu-id="5fdd5-974">The following example shows the **ReferenceType** element used as a child of a **Parameter** element in a model-defined function that accepts a reference to a **Person** entity type:</span></span>

``` xml
 <Function Name="GetYearsEmployed" ReturnType="Edm.Int32">
   <Parameter Name="instructor">
     <ReferenceType Type="SchoolModel.Person" />
   </Parameter>
   <DefiningExpression>
   Year(CurrentDateTime()) - Year(cast(instructor.HireDate as DateTime))
   </DefiningExpression>
 </Function>
```
 

<span data-ttu-id="5fdd5-975">В следующем примере показан элемент **ReferenceType** , используемый как дочерний элемент для элемента **ReturnType** (Function) в функции, определяемой моделью, которая возвращает ссылку на тип сущности **Person** :</span><span class="sxs-lookup"><span data-stu-id="5fdd5-975">The following example shows the **ReferenceType** element used as a child of a **ReturnType** (Function) element in a model-defined function that returns a reference to a **Person** entity type:</span></span>

``` xml
 <Function Name="GetPersonReference">
     <Parameter Name="p" Type="SchoolModel.Person" />
     <ReturnType>
         <ReferenceType Type="SchoolModel.Person" />
     </ReturnType>
     <DefiningExpression>
           REF(p)
     </DefiningExpression>
 </Function>
```
 

 

## <a name="referentialconstraint-element-csdl"></a><span data-ttu-id="5fdd5-976">Элемент ReferentialConstraint (CSDL)</span><span class="sxs-lookup"><span data-stu-id="5fdd5-976">ReferentialConstraint Element (CSDL)</span></span>

<span data-ttu-id="5fdd5-977">Элемент **ReferentialConstraint** в языке CSDL определяет функциональные возможности, аналогичные ограничению ссылочной целостности в реляционной базе данных.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-977">A **ReferentialConstraint** element in conceptual schema definition language (CSDL) defines functionality that is similar to a referential integrity constraint in a relational database.</span></span> <span data-ttu-id="5fdd5-978">Свойство (или свойства) типа сущности могут ссылаться на ключ сущности в другом типе сущности также, как столбец (или столбцы) в таблице базы данных могут ссылаться на первичный ключ другой таблицы.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-978">In the same way that a column (or columns) from a database table can reference the primary key of another table, a property (or properties) of an entity type can reference the entity key of another entity type.</span></span> <span data-ttu-id="5fdd5-979">Тип сущности, на который указывает ссылка, называется *основным элементом* ограничения.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-979">The entity type that is referenced is called the *principal end* of the constraint.</span></span> <span data-ttu-id="5fdd5-980">Тип сущности, который ссылается на основной элемент, называется *зависимым окончанием* ограничения.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-980">The entity type that references the principal end is called the *dependent end* of the constraint.</span></span>

<span data-ttu-id="5fdd5-981">Если внешний ключ, представленный в одном типе сущности, ссылается на свойство другого типа сущности, элемент **ReferentialConstraint** определяет ассоциацию между двумя типами сущностей.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-981">If a foreign key that is exposed on one entity type references a property on another entity type, the **ReferentialConstraint** element defines an association between the two entity types.</span></span> <span data-ttu-id="5fdd5-982">Поскольку элемент **ReferentialConstraint** предоставляет сведения о том, как два типа сущностей связаны друг с друга, соответствующий элемент AssociationSetMapping не требуется в языке спецификации СОПОСТАВЛЕНИЯ (MSL).</span><span class="sxs-lookup"><span data-stu-id="5fdd5-982">Because the **ReferentialConstraint** element provides information about how two entity types are related, no corresponding AssociationSetMapping element is necessary in the mapping specification language (MSL).</span></span> <span data-ttu-id="5fdd5-983">Ассоциация между двумя типами сущностей, у которых нет внешних ключей, должна иметь соответствующий элемент **AssociationSetMapping** , чтобы сопоставить сведения о сопоставлении с источником данных.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-983">An association between two entity types that do not have foreign keys exposed must have a corresponding **AssociationSetMapping** element in order to map association information to the data source.</span></span>

<span data-ttu-id="5fdd5-984">Если внешний ключ недоступен для типа сущности, элемент **ReferentialConstraint** может определять только ограничение первичного ключа на первичный ключ между типом сущности и другим типом сущности.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-984">If a foreign key is not exposed on an entity type, the **ReferentialConstraint** element can only define a primary key-to-primary key constraint between the entity type and another entity type.</span></span>

<span data-ttu-id="5fdd5-985">Элемент **ReferentialConstraint** может иметь следующие дочерние элементы (в указанном порядке):</span><span class="sxs-lookup"><span data-stu-id="5fdd5-985">A **ReferentialConstraint** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="5fdd5-986">Documentation (ноль или один элемент)</span><span class="sxs-lookup"><span data-stu-id="5fdd5-986">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="5fdd5-987">Principal (ровно один элемент)</span><span class="sxs-lookup"><span data-stu-id="5fdd5-987">Principal (exactly one element)</span></span>
-   <span data-ttu-id="5fdd5-988">Dependent (ровно один элемент)</span><span class="sxs-lookup"><span data-stu-id="5fdd5-988">Dependent (exactly one element)</span></span>
-   <span data-ttu-id="5fdd5-989">Элементы Annotation (ноль или более элементов)</span><span class="sxs-lookup"><span data-stu-id="5fdd5-989">Annotation elements (zero or more elements)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="5fdd5-990">Применимые атрибуты</span><span class="sxs-lookup"><span data-stu-id="5fdd5-990">Applicable Attributes</span></span>

<span data-ttu-id="5fdd5-991">Элемент **ReferentialConstraint** может иметь любое количество атрибутов аннотации (НАСТРАИВАЕМЫЕ атрибуты XML).</span><span class="sxs-lookup"><span data-stu-id="5fdd5-991">The **ReferentialConstraint** element can have any number of annotation attributes (custom XML attributes).</span></span> <span data-ttu-id="5fdd5-992">Однако настраиваемые атрибуты не могут принадлежать к любому пространству имен XML, зарезервированному для CSDL.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-992">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="5fdd5-993">Полные имена любых двух настраиваемых атрибутов не могут совпадать.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-993">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="5fdd5-994">Пример</span><span class="sxs-lookup"><span data-stu-id="5fdd5-994">Example</span></span>

<span data-ttu-id="5fdd5-995">В следующем примере показан элемент **ReferentialConstraint** , используемый как часть определения ассоциации **публишедби** .</span><span class="sxs-lookup"><span data-stu-id="5fdd5-995">The following example shows a **ReferentialConstraint** element being used as part of the definition of the **PublishedBy** association.</span></span>

``` xml
 <Association Name="PublishedBy">
   <End Type="BooksModel.Book" Role="Book" Multiplicity="*" >
   </End>
   <End Type="BooksModel.Publisher" Role="Publisher" Multiplicity="1" />
   <ReferentialConstraint>
     <Principal Role="Publisher">
       <PropertyRef Name="Id" />
     </Principal>
     <Dependent Role="Book">
       <PropertyRef Name="PublisherId" />
     </Dependent>
   </ReferentialConstraint>
 </Association>
```
 

 

## <a name="returntype-function-element-csdl"></a><span data-ttu-id="5fdd5-996">Элемент ReturnType (Function) (CSDL)</span><span class="sxs-lookup"><span data-stu-id="5fdd5-996">ReturnType (Function) Element (CSDL)</span></span>

<span data-ttu-id="5fdd5-997">Элемент **ReturnType** (Function) в языке CSDL указывает тип возвращаемого значения для функции, которая определена в элементе Function.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-997">The **ReturnType** (Function) element in conceptual schema definition language (CSDL) specifies the return type for a function that is defined in a Function element.</span></span> <span data-ttu-id="5fdd5-998">Тип возвращаемого значения функции также можно указать с помощью атрибута **ReturnType** .</span><span class="sxs-lookup"><span data-stu-id="5fdd5-998">A function return type can also be specified with a **ReturnType** attribute.</span></span>

<span data-ttu-id="5fdd5-999">Возвращаемыми типами могут быть любой **EDMSimpleType**, тип сущности, сложный тип, тип строки, ссылочный тип или коллекция одного из этих типов.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-999">Return types can be any **EdmSimpleType**, entity type, complex type, row type, ref type, or a collection of one of these types.</span></span>

<span data-ttu-id="5fdd5-1000">Тип возвращаемого значения функции может быть указан либо с помощью атрибута **Type** элемента **ReturnType** (Function), либо с помощью одного из следующих дочерних элементов:</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1000">The return type of a function can be specified with either the **Type** attribute of the **ReturnType** (Function) element, or with one of the following child elements:</span></span>

-   <span data-ttu-id="5fdd5-1001">CollectionType</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1001">CollectionType</span></span>
-   <span data-ttu-id="5fdd5-1002">Тип ссылки</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1002">ReferenceType</span></span>
-   <span data-ttu-id="5fdd5-1003">RowType</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1003">RowType</span></span>

> [!NOTE]
> <span data-ttu-id="5fdd5-1004">Модель не будет проверяться, если указать возвращаемый тип функции с атрибутом **Type** элемента **ReturnType** (Function) и одного из дочерних элементов.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1004">A model will not validate if you specify a function return type with both the **Type** attribute of the **ReturnType** (Function) element and one of the child elements.</span></span>

 

### <a name="applicable-attributes"></a><span data-ttu-id="5fdd5-1005">Применимые атрибуты</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1005">Applicable Attributes</span></span>

<span data-ttu-id="5fdd5-1006">В следующей таблице описаны атрибуты, которые можно применить к элементу **ReturnType** (Function).</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1006">The following table describes the attributes that can be applied to the **ReturnType** (Function) element.</span></span>

| <span data-ttu-id="5fdd5-1007">Имя атрибута</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1007">Attribute Name</span></span> | <span data-ttu-id="5fdd5-1008">Обязательный</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1008">Is Required</span></span> | <span data-ttu-id="5fdd5-1009">Значение</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1009">Value</span></span>                              |
|:---------------|:------------|:-----------------------------------|
| <span data-ttu-id="5fdd5-1010">**ReturnType**</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1010">**ReturnType**</span></span> | <span data-ttu-id="5fdd5-1011">Нет</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1011">No</span></span>          | <span data-ttu-id="5fdd5-1012">Тип, возвращаемый функцией.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1012">The type returned by the function.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="5fdd5-1013">К элементу **ReturnType** (Function) можно применить любое количество атрибутов аннотации (пользовательские XML-атрибуты).</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1013">Any number of annotation attributes (custom XML attributes) may be applied to the **ReturnType** (Function) element.</span></span> <span data-ttu-id="5fdd5-1014">Однако настраиваемые атрибуты не могут принадлежать к любому пространству имен XML, зарезервированному для CSDL.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1014">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="5fdd5-1015">Полные имена любых двух настраиваемых атрибутов не могут совпадать.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1015">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="5fdd5-1016">Пример</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1016">Example</span></span>

<span data-ttu-id="5fdd5-1017">В следующем примере используется элемент **Function** для определения функции, возвращающей число лет, в течение которых книга была напечатана.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1017">The following example uses a **Function** element to define a function that returns the number of years a book has been in print.</span></span> <span data-ttu-id="5fdd5-1018">Обратите внимание, что тип возвращаемого значения задается атрибутом **Type** элемента **ReturnType** (Function).</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1018">Note that the return type is specified by the **Type** attribute of a **ReturnType** (Function) element.</span></span>

``` xml
 <Function Name="GetYearsInPrint">
   <ReturnType Type=="Edm.Int32">
   <Parameter Name="book" Type="BooksModel.Book" />
   <DefiningExpression>
    Year(CurrentDateTime()) - Year(cast(book.PublishedDate as DateTime))
   </DefiningExpression>
 </Function>
```
 

 

## <a name="returntype-functionimport-element-csdl"></a><span data-ttu-id="5fdd5-1019">Элемент ReturnType (FunctionImport) (CSDL)</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1019">ReturnType (FunctionImport) Element (CSDL)</span></span>

<span data-ttu-id="5fdd5-1020">Элемент **ReturnType** (FunctionImport) в языке CSDL указывает тип возвращаемого значения для функции, которая определена в элементе FunctionImport.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1020">The **ReturnType** (FunctionImport) element in conceptual schema definition language (CSDL) specifies the return type for a function that is defined in a FunctionImport element.</span></span> <span data-ttu-id="5fdd5-1021">Тип возвращаемого значения функции также можно указать с помощью атрибута **ReturnType** .</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1021">A function return type can also be specified with a **ReturnType** attribute.</span></span>

<span data-ttu-id="5fdd5-1022">Возвращаемые типы могут быть любой коллекцией типа сущности, сложного типа или **EDMSimpleType**,</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1022">Return types can be any collection of entity type, complex type,or **EdmSimpleType**,</span></span>

<span data-ttu-id="5fdd5-1023">Тип возвращаемого значения функции указывается с помощью атрибута **Type** элемента **ReturnType** (FunctionImport).</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1023">The return type of a function is specified with the **Type** attribute of the **ReturnType** (FunctionImport) element.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="5fdd5-1024">Применимые атрибуты</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1024">Applicable Attributes</span></span>

<span data-ttu-id="5fdd5-1025">В следующей таблице описаны атрибуты, которые можно применить к элементу **ReturnType** (FunctionImport).</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1025">The following table describes the attributes that can be applied to the **ReturnType** (FunctionImport) element.</span></span>

| <span data-ttu-id="5fdd5-1026">Имя атрибута</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1026">Attribute Name</span></span> | <span data-ttu-id="5fdd5-1027">Обязательный</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1027">Is Required</span></span> | <span data-ttu-id="5fdd5-1028">Значение</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1028">Value</span></span>                                                                                                                                                                                                 |
|:---------------|:------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="5fdd5-1029">**Тип**</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1029">**Type**</span></span>       | <span data-ttu-id="5fdd5-1030">Нет</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1030">No</span></span>          | <span data-ttu-id="5fdd5-1031">Тип, возвращаемый функцией.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1031">The type that the function returns.</span></span> <span data-ttu-id="5fdd5-1032">Значение должно быть коллекцией типа ComplexType, EntityType или EDMSimpleType.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1032">The value must be a collection of ComplexType, EntityType, or EDMSimpleType.</span></span>                                                                                      |
| <span data-ttu-id="5fdd5-1033">**EntitySet**</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1033">**EntitySet**</span></span>  | <span data-ttu-id="5fdd5-1034">Нет</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1034">No</span></span>          | <span data-ttu-id="5fdd5-1035">Если функция возвращает коллекцию типов сущностей, то значение **EntitySet** должно быть набором сущностей, к которому принадлежит коллекция.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1035">If the function returns a collection of entity types, the value of the **EntitySet** must be the entity set to which the collection belongs.</span></span> <span data-ttu-id="5fdd5-1036">В противном случае атрибут **EntitySet** не должен использоваться.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1036">Otherwise, the **EntitySet** attribute must not be used.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="5fdd5-1037">К элементу **ReturnType** (FunctionImport) можно применить любое количество атрибутов аннотации (настраиваемые XML-атрибуты).</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1037">Any number of annotation attributes (custom XML attributes) may be applied to the **ReturnType** (FunctionImport) element.</span></span> <span data-ttu-id="5fdd5-1038">Однако настраиваемые атрибуты не могут принадлежать к любому пространству имен XML, зарезервированному для CSDL.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1038">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="5fdd5-1039">Полные имена любых двух настраиваемых атрибутов не могут совпадать.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1039">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="5fdd5-1040">Пример</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1040">Example</span></span>

<span data-ttu-id="5fdd5-1041">В следующем примере используется функция **FunctionImport** , возвращающая книги и издатели.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1041">The following example uses a **FunctionImport** that returns books and publishers.</span></span> <span data-ttu-id="5fdd5-1042">Обратите внимание, что функция возвращает два результирующих набора и, следовательно, указаны два элемента **ReturnType** (FunctionImport).</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1042">Note that the function returns two result sets and therefore two **ReturnType** (FunctionImport) elements are specified.</span></span>

``` xml
 <FunctionImport Name="GetBooksAndPublishers">
   <ReturnType Type=="Collection(BooksModel.Book )" EntitySet=”Books”>
   <ReturnType Type=="Collection(BooksModel.Publisher)" EntitySet=”Publishers”>
 </FunctionImport>
```
 

 

## <a name="rowtype-element-csdl"></a><span data-ttu-id="5fdd5-1043">Элемент RowType (язык CSDL)</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1043">RowType Element (CSDL)</span></span>

<span data-ttu-id="5fdd5-1044">Элемент **RowType** в языке CSDL определяет неименованную структуру как параметр или возвращаемый тип для функции, определенной в концептуальной модели.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1044">A **RowType** element in conceptual schema definition language (CSDL) defines an unnamed structure as a parameter or return type for a function defined in the conceptual model.</span></span>

<span data-ttu-id="5fdd5-1045">Элемент **RowType** может быть дочерним для следующих элементов:</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1045">A **RowType** element can be the child of the following elements:</span></span>

-   <span data-ttu-id="5fdd5-1046">CollectionType</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1046">CollectionType</span></span>
-   <span data-ttu-id="5fdd5-1047">Параметр</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1047">Parameter</span></span>
-   <span data-ttu-id="5fdd5-1048">ReturnType (функция)</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1048">ReturnType (Function)</span></span>

<span data-ttu-id="5fdd5-1049">Элемент **RowType** может иметь следующие дочерние элементы (в указанном порядке):</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1049">A **RowType** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="5fdd5-1050">Property (один или больше)</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1050">Property (one or more)</span></span>
-   <span data-ttu-id="5fdd5-1051">Элементы annotation (ноль или больше)</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1051">Annotation elements (zero or more)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="5fdd5-1052">Применимые атрибуты</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1052">Applicable Attributes</span></span>

<span data-ttu-id="5fdd5-1053">К элементу **RowType** можно применить любое количество атрибутов аннотации (настраиваемые XML-атрибуты).</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1053">Any number of annotation attributes (custom XML attributes) may be applied to the **RowType** element.</span></span> <span data-ttu-id="5fdd5-1054">Однако настраиваемые атрибуты не могут принадлежать к любому пространству имен XML, зарезервированному для CSDL.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1054">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="5fdd5-1055">Полные имена любых двух настраиваемых атрибутов не могут совпадать.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1055">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

### <a name="example"></a><span data-ttu-id="5fdd5-1056">Пример</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1056">Example</span></span>

<span data-ttu-id="5fdd5-1057">В следующем примере показана определяемая моделью функция, которая использует элемент **CollectionType** для указания того, что функция возвращает коллекцию строк (как указано в элементе **RowType** ).</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1057">The following example shows a model-defined function that uses a **CollectionType** element to specify that the function returns a collection of rows (as specified in the **RowType** element).</span></span>

``` xml
 <Function Name="LastNamesAfter">
   <Parameter Name="someString" Type="Edm.String" />
   <ReturnType>
    <CollectionType>
      <RowType>
        <Property Name="FirstName" Type="Edm.String" Nullable="false" />
        <Property Name="LastName" Type="Edm.String" Nullable="false" />
      </RowType>
    </CollectionType>
   </ReturnType>
   <DefiningExpression>
             SELECT VALUE ROW(p.FirstName, p.LastName)
             FROM SchoolEntities.People AS p
             WHERE p.LastName &gt;= somestring
   </DefiningExpression>
 </Function>
```

## <a name="schema-element-csdl"></a><span data-ttu-id="5fdd5-1058">Элемент Schema (CSDL)</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1058">Schema Element (CSDL)</span></span>

<span data-ttu-id="5fdd5-1059">Элемент **Schema** является корневым элементом определения концептуальной модели.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1059">The **Schema** element is the root element of a conceptual model definition.</span></span> <span data-ttu-id="5fdd5-1060">Он содержит определения объектов, функций и контейнеров, из которых состоит концептуальная модель.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1060">It contains definitions for the objects, functions, and containers that make up a conceptual model.</span></span>

<span data-ttu-id="5fdd5-1061">Элемент **Schema** может содержать ноль или более следующих дочерних элементов:</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1061">The **Schema** element may contain zero or more of the following child elements:</span></span>

-   <span data-ttu-id="5fdd5-1062">Использование</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1062">Using</span></span>
-   <span data-ttu-id="5fdd5-1063">EntityContainer</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1063">EntityContainer</span></span>
-   <span data-ttu-id="5fdd5-1064">EntityType</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1064">EntityType</span></span>
-   <span data-ttu-id="5fdd5-1065">EnumType</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1065">EnumType</span></span>
-   <span data-ttu-id="5fdd5-1066">Взаимосвязь</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1066">Association</span></span>
-   <span data-ttu-id="5fdd5-1067">ComplexType</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1067">ComplexType</span></span>
-   <span data-ttu-id="5fdd5-1068">Функция</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1068">Function</span></span>

<span data-ttu-id="5fdd5-1069">Элемент **схемы** может содержать не более одного элемента annotation.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1069">A **Schema** element may contain zero or one Annotation elements.</span></span>

> [!NOTE]
> <span data-ttu-id="5fdd5-1070">Элементы **Function** и Annotation разрешены только в CSDL v2 и более поздних версиях.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1070">The **Function** element and annotation elements are only allowed in CSDL v2 and later.</span></span>

 

<span data-ttu-id="5fdd5-1071">Элемент **Schema** использует атрибут **Namespace** для определения пространства имен для объектов типа сущности, сложного типа и ассоциаций в концептуальной модели.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1071">The **Schema** element uses the **Namespace** attribute to define the namespace for the entity type, complex type, and association objects in a conceptual model.</span></span> <span data-ttu-id="5fdd5-1072">В пространстве имен не может быть двух объектов с одинаковым именем.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1072">Within a namespace, no two objects can have the same name.</span></span> <span data-ttu-id="5fdd5-1073">Пространства имен могут охватывать несколько элементов **схемы** и несколько CSDL-файлов.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1073">Namespaces can span multiple **Schema** elements and multiple .csdl files.</span></span>

<span data-ttu-id="5fdd5-1074">Пространство имен концептуальной модели отличается от пространства имен XML элемента **Schema** .</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1074">A conceptual model namespace is different from the XML namespace of the **Schema** element.</span></span> <span data-ttu-id="5fdd5-1075">Пространство имен концептуальной модели (как определено атрибутом **пространства имен** ) является логическим контейнером для типов сущностей, сложных типов и типов взаимосвязей.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1075">A conceptual model namespace (as defined by the **Namespace** attribute) is a logical container for entity types, complex types, and association types.</span></span> <span data-ttu-id="5fdd5-1076">Пространство имен XML (обозначенное атрибутом **xmlns** ) элемента **Schema** является пространством имен по умолчанию для дочерних элементов и атрибутов элемента **Schema** .</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1076">The XML namespace (indicated by the **xmlns** attribute) of a **Schema** element is the default namespace for child elements and attributes of the **Schema** element.</span></span> <span data-ttu-id="5fdd5-1077">Пространства имен XML формы https://schemas.microsoft.com/ado/YYYY/MM/edm (где YYYY и mm представляют год и месяц соответственно) зарезервированы для языка CSDL.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1077">XML namespaces of the form https://schemas.microsoft.com/ado/YYYY/MM/edm (where YYYY and MM represent a year and month respectively) are reserved for CSDL.</span></span> <span data-ttu-id="5fdd5-1078">Пользовательские элементы и атрибуты не могут присутствовать в пространствах имен такого вида.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1078">Custom elements and attributes cannot be in namespaces that have this form.</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="5fdd5-1079">Применимые атрибуты</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1079">Applicable Attributes</span></span>

<span data-ttu-id="5fdd5-1080">В приведенной ниже таблице описаны атрибуты, которые можно применить к элементу **схемы** .</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1080">The table below describes the attributes can be applied to the **Schema** element.</span></span>

| <span data-ttu-id="5fdd5-1081">Имя атрибута</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1081">Attribute Name</span></span> | <span data-ttu-id="5fdd5-1082">Обязательный</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1082">Is Required</span></span> | <span data-ttu-id="5fdd5-1083">Значение</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1083">Value</span></span>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
|:---------------|:------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="5fdd5-1084">**Пространство имен**</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1084">**Namespace**</span></span>  | <span data-ttu-id="5fdd5-1085">Да</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1085">Yes</span></span>         | <span data-ttu-id="5fdd5-1086">Пространство имен концептуальной модели.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1086">The namespace of the conceptual model.</span></span> <span data-ttu-id="5fdd5-1087">Значение атрибута **Namespace** используется для формирования полного имени типа.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1087">The value of the **Namespace** attribute is used to form the fully qualified name of a type.</span></span> <span data-ttu-id="5fdd5-1088">Например, если **EntityType** с именем *Customer* находится в пространстве имен Simple. example. Model, то полное имя **EntityType** — симпликсамплемодел. Customer.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1088">For example, if an **EntityType** named *Customer* is in the Simple.Example.Model namespace, then the fully qualified name of the **EntityType** is SimpleExampleModel.Customer.</span></span> <br/> <span data-ttu-id="5fdd5-1089">Следующие строки не могут использоваться в качестве значения для атрибута **пространства имен** : **System**, **временные**или **EDM**.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1089">The following strings cannot be used as the value for the **Namespace** attribute: **System**, **Transient**, or **Edm**.</span></span> <span data-ttu-id="5fdd5-1090">Значение атрибута **пространства имен** не может совпадать со значением атрибута **Namespace** в элементе схемы SSDL.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1090">The value for the **Namespace** attribute cannot be the same as the value for the **Namespace** attribute in the SSDL Schema element.</span></span> |
| <span data-ttu-id="5fdd5-1091">**Псевдоним**</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1091">**Alias**</span></span>      | <span data-ttu-id="5fdd5-1092">Нет</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1092">No</span></span>          | <span data-ttu-id="5fdd5-1093">Идентификатор, используемый в качестве имени пространства имен.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1093">An identifier used in place of the namespace name.</span></span> <span data-ttu-id="5fdd5-1094">Например, если **EntityType** с именем *Customer* находится в пространстве имен Simple. пример. Model, а значение атрибута **Alias** — *model*, то можно использовать модель. Customer как полное имя **EntityType.**</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1094">For example, if an **EntityType** named *Customer* is in the Simple.Example.Model namespace and the value of the **Alias** attribute is *Model*, then you can use Model.Customer as the fully qualified name of the **EntityType.**</span></span>                                                                                                                                                                                                                                                                                                     |

 

> [!NOTE]
> <span data-ttu-id="5fdd5-1095">К элементу **схемы** можно применить любое количество атрибутов аннотации (настраиваемые XML-атрибуты).</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1095">Any number of annotation attributes (custom XML attributes) may be applied to the **Schema** element.</span></span> <span data-ttu-id="5fdd5-1096">Однако настраиваемые атрибуты не могут принадлежать к любому пространству имен XML, зарезервированному для CSDL.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1096">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="5fdd5-1097">Полные имена любых двух настраиваемых атрибутов не могут совпадать.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1097">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="5fdd5-1098">Пример</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1098">Example</span></span>

<span data-ttu-id="5fdd5-1099">В следующем примере показан элемент **схемы** , содержащий элемент **EntityContainer** , два элемента **EntityType** и один элемент **Association** .</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1099">The following example shows a **Schema** element that contains an **EntityContainer** element, two **EntityType** elements, and one **Association** element.</span></span>

``` xml
 <Schema xmlns="https://schemas.microsoft.com/ado/2009/11/edm"
      xmlns:cg="https://schemas.microsoft.com/ado/2009/11/codegeneration"
      xmlns:store="https://schemas.microsoft.com/ado/2009/11/edm/EntityStoreSchemaGenerator"
       Namespace="ExampleModel" Alias="Self">
         <EntityContainer Name="ExampleModelContainer">
           <EntitySet Name="Customers"
                      EntityType="ExampleModel.Customer" />
           <EntitySet Name="Orders" EntityType="ExampleModel.Order" />
           <AssociationSet
                       Name="CustomerOrder"
                       Association="ExampleModel.CustomerOrders">
             <End Role="Customer" EntitySet="Customers" />
             <End Role="Order" EntitySet="Orders" />
           </AssociationSet>
         </EntityContainer>
         <EntityType Name="Customer">
           <Key>
             <PropertyRef Name="CustomerId" />
           </Key>
           <Property Type="Int32" Name="CustomerId" Nullable="false" />
           <Property Type="String" Name="Name" Nullable="false" />
           <NavigationProperty
                    Name="Orders"
                    Relationship="ExampleModel.CustomerOrders"
                    FromRole="Customer" ToRole="Order" />
         </EntityType>
         <EntityType Name="Order">
           <Key>
             <PropertyRef Name="OrderId" />
           </Key>
           <Property Type="Int32" Name="OrderId" Nullable="false" />
           <Property Type="Int32" Name="ProductId" Nullable="false" />
           <Property Type="Int32" Name="Quantity" Nullable="false" />
           <NavigationProperty
                    Name="Customer"
                    Relationship="ExampleModel.CustomerOrders"
                    FromRole="Order" ToRole="Customer" />
           <Property Type="Int32" Name="CustomerId" Nullable="false" />
         </EntityType>
         <Association Name="CustomerOrders">
           <End Type="ExampleModel.Customer"
                Role="Customer" Multiplicity="1" />
           <End Type="ExampleModel.Order"
                Role="Order" Multiplicity="*" />
           <ReferentialConstraint>
             <Principal Role="Customer">
               <PropertyRef Name="CustomerId" />
             </Principal>
             <Dependent Role="Order">
               <PropertyRef Name="CustomerId" />
             </Dependent>
           </ReferentialConstraint>
         </Association>
       </Schema>
```
 

 

## <a name="typeref-element-csdl"></a><span data-ttu-id="5fdd5-1100">Элемент TypeRef (CSDL)</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1100">TypeRef Element (CSDL)</span></span>

<span data-ttu-id="5fdd5-1101">Элемент **TypeRef** в языке CSDL предоставляет ссылку на существующий именованный тип.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1101">The **TypeRef** element in conceptual schema definition language (CSDL) provides a reference to an existing named type.</span></span> <span data-ttu-id="5fdd5-1102">Элемент **TypeRef** может быть дочерним по отношению к элементу CollectionType, который используется для указания того, что функция имеет коллекцию в качестве параметра или возвращаемого типа.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1102">The **TypeRef** element can be a child of the CollectionType element, which is used to specify that a function has a collection as a parameter or return type.</span></span>

<span data-ttu-id="5fdd5-1103">Элемент **TypeRef** может иметь следующие дочерние элементы (в указанном порядке):</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1103">A **TypeRef** element can have the following child elements (in the order listed):</span></span>

-   <span data-ttu-id="5fdd5-1104">Documentation (ноль или один элемент)</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1104">Documentation (zero or one element)</span></span>
-   <span data-ttu-id="5fdd5-1105">Элементы Annotation (ноль или более элементов)</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1105">Annotation elements (zero or more elements)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="5fdd5-1106">Применимые атрибуты</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1106">Applicable Attributes</span></span>

<span data-ttu-id="5fdd5-1107">В следующей таблице описаны атрибуты, которые можно применить к элементу **TypeRef** .</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1107">The following table describes the attributes that can be applied to the **TypeRef** element.</span></span> <span data-ttu-id="5fdd5-1108">Обратите внимание, что атрибуты **DefaultValue**, **MaxLength**, **FixedLength**, **точность**, **масштаб**, **Юникод**и **Параметры сортировки** применимы только к **едмсимплетипес**.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1108">Note that the **DefaultValue**, **MaxLength**, **FixedLength**, **Precision**, **Scale**, **Unicode**, and **Collation** attributes are only applicable to **EDMSimpleTypes**.</span></span>

| <span data-ttu-id="5fdd5-1109">Имя атрибута</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1109">Attribute Name</span></span>                                                     | <span data-ttu-id="5fdd5-1110">Обязательный</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1110">Is Required</span></span> | <span data-ttu-id="5fdd5-1111">Значение</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1111">Value</span></span>                                                                                                                                                                                                                           |
|:-------------------------------------------------------------------|:------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="5fdd5-1112">**Тип**</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1112">**Type**</span></span>                                                           | <span data-ttu-id="5fdd5-1113">Нет</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1113">No</span></span>          | <span data-ttu-id="5fdd5-1114">Имя типа, на который дается ссылка.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1114">The name of the type being referenced.</span></span>                                                                                                                                                                                          |
| <span data-ttu-id="5fdd5-1115">**Допускает значения NULL**</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1115">**Nullable**</span></span>                                                       | <span data-ttu-id="5fdd5-1116">Нет</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1116">No</span></span>          | <span data-ttu-id="5fdd5-1117">**True** (значение по умолчанию) или **False** в зависимости от того, может ли свойство иметь значение null.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1117">**True** (the default value) or **False** depending on whether the property can have a null value.</span></span> <br/> [!NOTE]                                                                                                                |
| <span data-ttu-id="5fdd5-1118">> в CSDL v1 свойство сложного типа должно иметь значение `Nullable="False"` .</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1118">> In CSDL v1 a complex type property must have `Nullable="False"`.</span></span> |             |                                                                                                                                                                                                                                 |
| <span data-ttu-id="5fdd5-1119">**DefaultValue**</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1119">**DefaultValue**</span></span>                                                   | <span data-ttu-id="5fdd5-1120">Нет</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1120">No</span></span>          | <span data-ttu-id="5fdd5-1121">Значение по умолчанию для свойства.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1121">The default value of the property.</span></span>                                                                                                                                                                                              |
| <span data-ttu-id="5fdd5-1122">**MaxLength**</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1122">**MaxLength**</span></span>                                                      | <span data-ttu-id="5fdd5-1123">Нет</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1123">No</span></span>          | <span data-ttu-id="5fdd5-1124">Максимальная длина значения свойства.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1124">The maximum length of the property value.</span></span>                                                                                                                                                                                       |
| <span data-ttu-id="5fdd5-1125">**FixedLength**</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1125">**FixedLength**</span></span>                                                    | <span data-ttu-id="5fdd5-1126">Нет</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1126">No</span></span>          | <span data-ttu-id="5fdd5-1127">Значение **true** или **false** в зависимости от того, будет ли значение свойства храниться как строка фиксированной длины.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1127">**True** or **False** depending on whether the property value will be stored as a fixed length string.</span></span>                                                                                                                          |
| <span data-ttu-id="5fdd5-1128">**Точность**</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1128">**Precision**</span></span>                                                      | <span data-ttu-id="5fdd5-1129">Нет</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1129">No</span></span>          | <span data-ttu-id="5fdd5-1130">Точность значения свойства.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1130">The precision of the property value.</span></span>                                                                                                                                                                                            |
| <span data-ttu-id="5fdd5-1131">**Масштабирование**</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1131">**Scale**</span></span>                                                          | <span data-ttu-id="5fdd5-1132">Нет</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1132">No</span></span>          | <span data-ttu-id="5fdd5-1133">Масштаб значения свойства.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1133">The scale of the property value.</span></span>                                                                                                                                                                                                |
| <span data-ttu-id="5fdd5-1134">**SRID**</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1134">**SRID**</span></span>                                                           | <span data-ttu-id="5fdd5-1135">Нет</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1135">No</span></span>          | <span data-ttu-id="5fdd5-1136">Идентификатор пространственной системной ссылки.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1136">Spatial System Reference Identifier.</span></span> <span data-ttu-id="5fdd5-1137">Допустимо только для свойств пространственных типов.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1137">Valid only for properties of spatial types.</span></span> <span data-ttu-id="5fdd5-1138">Дополнительные сведения см. в разделе [SRID](https://en.wikipedia.org/wiki/SRID) and [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1138">For more information, see [SRID](https://en.wikipedia.org/wiki/SRID) and [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span></span> |
| <span data-ttu-id="5fdd5-1139">**Юникод**</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1139">**Unicode**</span></span>                                                        | <span data-ttu-id="5fdd5-1140">Нет</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1140">No</span></span>          | <span data-ttu-id="5fdd5-1141">Значение **true** или **false** в зависимости от того, будет ли значение свойства храниться как строка в Юникоде.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1141">**True** or **False** depending on whether the property value will be stored as a Unicode string.</span></span>                                                                                                                               |
| <span data-ttu-id="5fdd5-1142">**Параметры сортировки**</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1142">**Collation**</span></span>                                                      | <span data-ttu-id="5fdd5-1143">Нет</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1143">No</span></span>          | <span data-ttu-id="5fdd5-1144">Строка, указывающая порядок сортировки, используемый в источнике данных.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1144">A string that specifies the collating sequence to be used in the data source.</span></span>                                                                                                                                                   |

 

> [!NOTE]
> <span data-ttu-id="5fdd5-1145">К элементу **CollectionType** можно применить любое количество атрибутов аннотации (настраиваемые XML-атрибуты).</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1145">Any number of annotation attributes (custom XML attributes) may be applied to the **CollectionType** element.</span></span> <span data-ttu-id="5fdd5-1146">Однако настраиваемые атрибуты не могут принадлежать к любому пространству имен XML, зарезервированному для CSDL.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1146">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="5fdd5-1147">Полные имена любых двух настраиваемых атрибутов не могут совпадать.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1147">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="5fdd5-1148">Пример</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1148">Example</span></span>

<span data-ttu-id="5fdd5-1149">В следующем примере показана определяемая моделью функция, использующая элемент **TypeRef** (как дочерний элемент элемента **CollectionType** ), чтобы указать, что функция принимает коллекцию типов сущностей **отдела** .</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1149">The following example shows a model-defined function that uses the **TypeRef** element (as a child of a **CollectionType** element) to specify that the function accepts a collection of **Department** entity types.</span></span>

``` xml
 <Function Name="GetAvgBudget">
      <Parameter Name="Departments">
          <CollectionType>
             <TypeRef Type="SchoolModel.Department"/>
          </CollectionType>
           </Parameter>
       <ReturnType Type="Collection(Edm.Decimal)"/>
       <DefiningExpression>
             SELECT VALUE AVG(d.Budget) FROM Departments AS d
       </DefiningExpression>
 </Function>
```
 

 

## <a name="using-element-csdl"></a><span data-ttu-id="5fdd5-1150">Элемент Using (CSDL)</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1150">Using Element (CSDL)</span></span>

<span data-ttu-id="5fdd5-1151">Элемент **using** в языке CSDL импортирует содержимое концептуальной модели, которая существует в другом пространстве имен.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1151">The **Using** element in conceptual schema definition language (CSDL) imports the contents of a conceptual model that exists in a different namespace.</span></span> <span data-ttu-id="5fdd5-1152">Устанавливая значение атрибута **Namespace** , можно ссылаться на типы сущностей, сложные типы и типы взаимосвязей, определенные в другой концептуальной модели.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1152">By setting the value of the **Namespace** attribute, you can refer to entity types, complex types, and association types that are defined in another conceptual model.</span></span> <span data-ttu-id="5fdd5-1153">Более одного элемента **using** может быть потомком элемента **Schema** .</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1153">More than one **Using** element can be a child of a **Schema** element.</span></span>

> [!NOTE]
> <span data-ttu-id="5fdd5-1154">Элемент **using** в языке CSDL не работает точно так же, как оператор **using** в языке программирования.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1154">The **Using** element in CSDL does not function exactly like a **using** statement in a programming language.</span></span> <span data-ttu-id="5fdd5-1155">Импорт пространства имен с помощью оператора **using** в языке программирования не влияет на объекты в исходном пространстве имен.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1155">By importing a namespace with a **using** statement in a programming language, you do not affect objects in the original namespace.</span></span> <span data-ttu-id="5fdd5-1156">В языке CSDL импортированное пространство имен может содержать тип сущности, производный от типа сущности в исходном пространстве имен.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1156">In CSDL, an imported namespace can contain an entity type that is derived from an entity type in the original namespace.</span></span> <span data-ttu-id="5fdd5-1157">Это может влиять на наборы сущностей, объявленные в исходном пространстве имен.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1157">This can affect entity sets declared in the original namespace.</span></span>

 

<span data-ttu-id="5fdd5-1158">Элемент **using** может иметь следующие дочерние элементы:</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1158">The **Using** element can have the following child elements:</span></span>

-   <span data-ttu-id="5fdd5-1159">Documentation (допустимое количество элементов — ноль или один)</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1159">Documentation (zero or one elements allowed)</span></span>
-   <span data-ttu-id="5fdd5-1160">Элементы Annotation (допустимое количество элементов — ноль или больше)</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1160">Annotation elements (zero or more elements allowed)</span></span>

### <a name="applicable-attributes"></a><span data-ttu-id="5fdd5-1161">Применимые атрибуты</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1161">Applicable Attributes</span></span>

<span data-ttu-id="5fdd5-1162">В приведенной ниже таблице описаны атрибуты, которые можно применить к элементу **using** .</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1162">The table below describes the attributes can be applied to the **Using** element.</span></span>

| <span data-ttu-id="5fdd5-1163">Имя атрибута</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1163">Attribute Name</span></span> | <span data-ttu-id="5fdd5-1164">Обязательный</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1164">Is Required</span></span> | <span data-ttu-id="5fdd5-1165">Значение</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1165">Value</span></span>                                                                                                                                                                              |
|:---------------|:------------|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <span data-ttu-id="5fdd5-1166">**Пространство имен**</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1166">**Namespace**</span></span>  | <span data-ttu-id="5fdd5-1167">Да</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1167">Yes</span></span>         | <span data-ttu-id="5fdd5-1168">Имя импортируемого пространства имен.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1168">The name of the imported namespace.</span></span>                                                                                                                                                |
| <span data-ttu-id="5fdd5-1169">**Псевдоним**</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1169">**Alias**</span></span>      | <span data-ttu-id="5fdd5-1170">Да</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1170">Yes</span></span>         | <span data-ttu-id="5fdd5-1171">Идентификатор, используемый в качестве имени пространства имен.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1171">An identifier used in place of the namespace name.</span></span> <span data-ttu-id="5fdd5-1172">Хотя этот атрибут является обязательным, необязательно, чтобы он использовался вместо имени пространства имен при указании имен объектов.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1172">Although this attribute is required, it is not required that it be used in place of the namespace name to qualify object names.</span></span> |

 

> [!NOTE]
> <span data-ttu-id="5fdd5-1173">К элементу **using** можно применить любое количество атрибутов аннотации (настраиваемые XML-атрибуты).</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1173">Any number of annotation attributes (custom XML attributes) may be applied to the **Using** element.</span></span> <span data-ttu-id="5fdd5-1174">Однако настраиваемые атрибуты не могут принадлежать к любому пространству имен XML, зарезервированному для CSDL.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1174">However, custom attributes may not belong to any XML namespace that is reserved for CSDL.</span></span> <span data-ttu-id="5fdd5-1175">Полные имена любых двух настраиваемых атрибутов не могут совпадать.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1175">The fully-qualified names for any two custom attributes cannot be the same.</span></span>

 

### <a name="example"></a><span data-ttu-id="5fdd5-1176">Пример</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1176">Example</span></span>

<span data-ttu-id="5fdd5-1177">В следующем примере показан элемент **using** , используемый для импорта пространства имен, определенного в других местах.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1177">The following example demonstrates the **Using** element being used to import a namespace that is defined elsewhere.</span></span> <span data-ttu-id="5fdd5-1178">Обратите внимание, что пространство имен для элемента **схемы** показано на рисунке `BooksModel` .</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1178">Note that the namespace for the **Schema** element shown is `BooksModel`.</span></span> <span data-ttu-id="5fdd5-1179">`Address`Свойство `Publisher` типа **EntityType** — сложный тип, определенный в `ExtendedBooksModel` пространстве имен (импортированный с **помощью** элемента).</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1179">The `Address` property on the `Publisher`**EntityType** is a complex type that is defined in the `ExtendedBooksModel` namespace (imported with the **Using** element).</span></span>

``` xml
 <Schema xmlns="https://schemas.microsoft.com/ado/2009/11/edm"
           xmlns:cg="https://schemas.microsoft.com/ado/2009/11/codegeneration"
           xmlns:store="https://schemas.microsoft.com/ado/2009/11/edm/EntityStoreSchemaGenerator"
           Namespace="BooksModel" Alias="Self">

     <Using Namespace="BooksModel.Extended" Alias="BMExt" />

 <EntityContainer Name="BooksContainer" >
       <EntitySet Name="Publishers" EntityType="BooksModel.Publisher" />
     </EntityContainer>

 <EntityType Name="Publisher">
       <Key>
         <PropertyRef Name="Id" />
       </Key>
       <Property Type="Int32" Name="Id" Nullable="false" />
       <Property Type="String" Name="Name" Nullable="false" />
       <Property Type="BMExt.Address" Name="Address" Nullable="false" />
     </EntityType>

 </Schema>
```
 

 

## <a name="annotation-attributes-csdl"></a><span data-ttu-id="5fdd5-1180">Атрибуты примечаний (CSDL)</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1180">Annotation Attributes (CSDL)</span></span>

<span data-ttu-id="5fdd5-1181">Атрибуты annotation в языке CSDL представляют собой настраиваемые атрибуты XML в концептуальной модели.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1181">Annotation attributes in conceptual schema definition language (CSDL) are custom XML attributes in the conceptual model.</span></span> <span data-ttu-id="5fdd5-1182">Атрибуты annotation должны не только иметь допустимую структуру XML, но и соответствовать следующим требованиям:</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1182">In addition to having valid XML structure, the following must be true of annotation attributes:</span></span>

-   <span data-ttu-id="5fdd5-1183">Атрибуты annotation не должны находиться ни в одном пространстве имен XML, которое зарезервировано для языка CSDL.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1183">Annotation attributes must not be in any XML namespace that is reserved for CSDL.</span></span>
-   <span data-ttu-id="5fdd5-1184">К данному конкретному элементу языка CSDL может применяться несколько атрибутов annotation.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1184">More than one annotation attribute may be applied to a given CSDL element.</span></span>
-   <span data-ttu-id="5fdd5-1185">Полные имена любых двух атрибутов annotation не должны совпадать.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1185">The fully-qualified names of any two annotation attributes must not be the same.</span></span>

<span data-ttu-id="5fdd5-1186">Атрибуты annotation могут использоваться для предоставления дополнительных метаданных об элементах в концептуальной модели.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1186">Annotation attributes can be used to provide extra metadata about the elements in a conceptual model.</span></span> <span data-ttu-id="5fdd5-1187">Метаданные, содержащиеся в элементах Annotation, могут быть доступны во время выполнения с помощью классов в пространстве имен System. Data. Metadata. EDM.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1187">Metadata contained in annotation elements can be accessed at runtime by using classes in the System.Data.Metadata.Edm namespace.</span></span>

### <a name="example"></a><span data-ttu-id="5fdd5-1188">Пример</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1188">Example</span></span>

<span data-ttu-id="5fdd5-1189">В следующем примере показан элемент **EntityType** с атрибутом заметки (**CustomAttribute**).</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1189">The following example shows an **EntityType** element with an annotation attribute (**CustomAttribute**).</span></span> <span data-ttu-id="5fdd5-1190">В следующем примере показано применение элемента annotation к элементам типа сущности.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1190">The example also shows an annotation element applied to the entity type element.</span></span>

``` xml
 <Schema Namespace="SchoolModel" Alias="Self"
         xmlns:annotation="https://schemas.microsoft.com/ado/2009/02/edm/annotation"
         xmlns="https://schemas.microsoft.com/ado/2009/11/edm">
   <EntityContainer Name="SchoolEntities" annotation:LazyLoadingEnabled="true">
     <EntitySet Name="People" EntityType="SchoolModel.Person" />
   </EntityContainer>
   <EntityType Name="Person" xmlns:p="http://CustomNamespace.com"
               p:CustomAttribute="Data here.">
     <Key>
       <PropertyRef Name="PersonID" />
     </Key>
     <Property Name="PersonID" Type="Int32" Nullable="false"
               annotation:StoreGeneratedPattern="Identity" />
     <Property Name="LastName" Type="String" Nullable="false"
               MaxLength="50" Unicode="true" FixedLength="false" />
     <Property Name="FirstName" Type="String" Nullable="false"
               MaxLength="50" Unicode="true" FixedLength="false" />
     <Property Name="HireDate" Type="DateTime" />
     <Property Name="EnrollmentDate" Type="DateTime" />
     <p:CustomElement>
       Custom metadata.
     </p:CustomElement>
   </EntityType>
 </Schema>
```
 

<span data-ttu-id="5fdd5-1191">Следующий код извлекает метаданные из атрибута annotation и выводит их на консоль:</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1191">The following code retrieves the metadata in the annotation attribute and writes it to the console:</span></span>

``` xml
 EdmItemCollection collection = new EdmItemCollection("School.csdl");
 MetadataWorkspace workspace = new MetadataWorkspace();
 workspace.RegisterItemCollection(collection);
 EdmType contentType;
 workspace.TryGetType("Person", "SchoolModel", DataSpace.CSpace, out contentType);
 if (contentType.MetadataProperties.Contains("http://CustomNamespace.com:CustomAttribute"))
 {
     MetadataProperty annotationProperty =
         contentType.MetadataProperties["http://CustomNamespace.com:CustomAttribute"];
     object annotationValue = annotationProperty.Value;
     Console.WriteLine(annotationValue.ToString());
 }
```
 

<span data-ttu-id="5fdd5-1192">Вышеуказанный код предполагает, что файл `School.csdl` находится в выходном каталоге проекта, а в проект добавлены инструкции `Imports` и `Using`:</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1192">The code above assumes that the `School.csdl` file is in the project's output directory and that you have added the following `Imports` and `Using` statements to your project:</span></span>

``` csharp
 using System.Data.Metadata.Edm;
```
 

 

## <a name="annotation-elements-csdl"></a><span data-ttu-id="5fdd5-1193">Элементы примечаний (CSDL)</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1193">Annotation Elements (CSDL)</span></span>

<span data-ttu-id="5fdd5-1194">Элементы annotation в языке CSDL представляют собой настраиваемые элементы XML в концептуальной модели.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1194">Annotation elements in conceptual schema definition language (CSDL) are custom XML elements in the conceptual model.</span></span> <span data-ttu-id="5fdd5-1195">Элементы annotation должны не только иметь допустимую структуру XML, но и соответствовать следующим требованиям:</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1195">In addition to having valid XML structure, the following must be true of annotation elements:</span></span>

-   <span data-ttu-id="5fdd5-1196">Элементы annotation не должны находиться в каком-либо пространстве имен XML, которое зарезервировано для языка CSDL.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1196">Annotation elements must not be in any XML namespace that is reserved for CSDL.</span></span>
-   <span data-ttu-id="5fdd5-1197">У данного элемента языка CSDL может быть больше одного дочернего элемента annotation.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1197">More than one annotation element may be a child of a given CSDL element.</span></span>
-   <span data-ttu-id="5fdd5-1198">Полные имена любых двух элементов annotation не должны совпадать.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1198">The fully-qualified names of any two annotation elements must not be the same.</span></span>
-   <span data-ttu-id="5fdd5-1199">Элементы annotation должны находиться после всех остальных дочерних элементов в данном элементе CSDL.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1199">Annotation elements must appear after all other child elements of a given CSDL element.</span></span>

<span data-ttu-id="5fdd5-1200">Элементы annotation могут использоваться для предоставления дополнительных метаданных об элементах в концептуальной модели.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1200">Annotation elements can be used to provide extra metadata about the elements in a conceptual model.</span></span> <span data-ttu-id="5fdd5-1201">Начиная с .NET Framework версии 4 метаданные, содержащиеся в элементах Annotation, могут быть доступны во время выполнения с помощью классов в пространстве имен System. Data. Metadata. EDM.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1201">Starting with the .NET Framework version 4, metadata contained in annotation elements can be accessed at runtime by using classes in the System.Data.Metadata.Edm namespace.</span></span>

### <a name="example"></a><span data-ttu-id="5fdd5-1202">Пример</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1202">Example</span></span>

<span data-ttu-id="5fdd5-1203">В следующем примере показан элемент **EntityType** с элементом annotation (**кустомелемент**).</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1203">The following example shows an **EntityType** element with an annotation element (**CustomElement**).</span></span> <span data-ttu-id="5fdd5-1204">В следующем примере показано применение атрибута annotation к элементам типа сущности.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1204">The example also show an annotation attribute applied to the entity type element.</span></span>

``` xml
 <Schema Namespace="SchoolModel" Alias="Self"
         xmlns:annotation="https://schemas.microsoft.com/ado/2009/02/edm/annotation"
         xmlns="https://schemas.microsoft.com/ado/2009/11/edm">
   <EntityContainer Name="SchoolEntities" annotation:LazyLoadingEnabled="true">
     <EntitySet Name="People" EntityType="SchoolModel.Person" />
   </EntityContainer>
   <EntityType Name="Person" xmlns:p="http://CustomNamespace.com"
               p:CustomAttribute="Data here.">
     <Key>
       <PropertyRef Name="PersonID" />
     </Key>
     <Property Name="PersonID" Type="Int32" Nullable="false"
               annotation:StoreGeneratedPattern="Identity" />
     <Property Name="LastName" Type="String" Nullable="false"
               MaxLength="50" Unicode="true" FixedLength="false" />
     <Property Name="FirstName" Type="String" Nullable="false"
               MaxLength="50" Unicode="true" FixedLength="false" />
     <Property Name="HireDate" Type="DateTime" />
     <Property Name="EnrollmentDate" Type="DateTime" />
     <p:CustomElement>
       Custom metadata.
     </p:CustomElement>
   </EntityType>
 </Schema>
```
 

<span data-ttu-id="5fdd5-1205">Следующий код извлекает метаданные из элемента annotation и выводит их на консоль.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1205">The following code retrieves the metadata in the annotation element and writes it to the console:</span></span>

``` csharp
 EdmItemCollection collection = new EdmItemCollection("School.csdl");
 MetadataWorkspace workspace = new MetadataWorkspace();
 workspace.RegisterItemCollection(collection);
 EdmType contentType;
 workspace.TryGetType("Person", "SchoolModel", DataSpace.CSpace, out contentType);
 if (contentType.MetadataProperties.Contains("http://CustomNamespace.com:CustomElement"))
 {
     MetadataProperty annotationProperty =
         contentType.MetadataProperties["http://CustomNamespace.com:CustomElement"];
     object annotationValue = annotationProperty.Value;
     Console.WriteLine(annotationValue.ToString());
 }
```
 

<span data-ttu-id="5fdd5-1206">Вышеприведенный код предполагает, что файл School.csdl находится в выходном каталоге проекта, а в проект добавлены инструкции `Imports` и `Using`.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1206">The code above assumes that the School.csdl file is in the project's output directory and that you have added the following `Imports` and `Using` statements to your project:</span></span>

``` csharp
 using System.Data.Metadata.Edm;
```
 

 

## <a name="conceptual-model-types-csdl"></a><span data-ttu-id="5fdd5-1207">Типы концептуальной модели (CSDL)</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1207">Conceptual Model Types (CSDL)</span></span>

<span data-ttu-id="5fdd5-1208">Язык CSDL поддерживает набор абстрактных примитивных типов данных, именуемых **едмсимплетипес**, которые определяют свойства в концептуальной модели.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1208">Conceptual schema definition language (CSDL) supports a set of abstract primitive data types, called **EDMSimpleTypes**, that define properties in a conceptual model.</span></span> <span data-ttu-id="5fdd5-1209">**Едмсимплетипес** — это прокси-серверы для примитивных типов данных, которые поддерживаются в среде хранения или размещения.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1209">**EDMSimpleTypes** are proxies for primitive data types that are supported in the storage or hosting environment.</span></span>

<span data-ttu-id="5fdd5-1210">В приведенной ниже таблице представлены примитивные типы данных, поддерживаемые CSDL.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1210">The table below lists the primitive data types that are supported by CSDL.</span></span> <span data-ttu-id="5fdd5-1211">В таблице также перечислены аспекты, которые можно применять к каждому **EDMSimpleType**.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1211">The table also lists the facets that can be applied to each **EDMSimpleType**.</span></span>

| <span data-ttu-id="5fdd5-1212">EDMSimpleType</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1212">EDMSimpleType</span></span>                    | <span data-ttu-id="5fdd5-1213">Описание</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1213">Description</span></span>                                                | <span data-ttu-id="5fdd5-1214">Применимые аспекты</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1214">Applicable Facets</span></span>                                                        |
|:---------------------------------|:-----------------------------------------------------------|:-------------------------------------------------------------------------|
| <span data-ttu-id="5fdd5-1215">**Edm.Binary**</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1215">**Edm.Binary**</span></span>                   | <span data-ttu-id="5fdd5-1216">Содержит двоичные данные.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1216">Contains binary data.</span></span>                                      | <span data-ttu-id="5fdd5-1217">MaxLength, FixedLength, Nullable, Default</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1217">MaxLength, FixedLength, Nullable, Default</span></span>                                |
| <span data-ttu-id="5fdd5-1218">**Edm.Boolean**</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1218">**Edm.Boolean**</span></span>                  | <span data-ttu-id="5fdd5-1219">Содержит значение **true** или **false**.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1219">Contains the value **true** or **false**.</span></span>                  | <span data-ttu-id="5fdd5-1220">Nullable, Default</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1220">Nullable, Default</span></span>                                                        |
| <span data-ttu-id="5fdd5-1221">**Edm.Byte**</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1221">**Edm.Byte**</span></span>                     | <span data-ttu-id="5fdd5-1222">Содержит 8-битное целое значение без знака.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1222">Contains an unsigned 8-bit integer value.</span></span>                  | <span data-ttu-id="5fdd5-1223">Precision, Nullable, Default</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1223">Precision, Nullable, Default</span></span>                                             |
| <span data-ttu-id="5fdd5-1224">**Edm.DateTime**</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1224">**Edm.DateTime**</span></span>                 | <span data-ttu-id="5fdd5-1225">Представляет дату и время.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1225">Represents a date and time.</span></span>                                | <span data-ttu-id="5fdd5-1226">Precision, Nullable, Default</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1226">Precision, Nullable, Default</span></span>                                             |
| <span data-ttu-id="5fdd5-1227">**Edm.DateTimeOffset**</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1227">**Edm.DateTimeOffset**</span></span>           | <span data-ttu-id="5fdd5-1228">Возвращает дату и время в виде смещения в минутах от времени GMT.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1228">Contains a date and time as an offset in minutes from GMT.</span></span> | <span data-ttu-id="5fdd5-1229">Precision, Nullable, Default</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1229">Precision, Nullable, Default</span></span>                                             |
| <span data-ttu-id="5fdd5-1230">**Edm.Decimal**</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1230">**Edm.Decimal**</span></span>                  | <span data-ttu-id="5fdd5-1231">Содержит точное числовое значение с заданной точностью и масштабом.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1231">Contains a numeric value with fixed precision and scale.</span></span>   | <span data-ttu-id="5fdd5-1232">Precision, Nullable, Default</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1232">Precision, Nullable, Default</span></span>                                             |
| <span data-ttu-id="5fdd5-1233">**Edm.Double**</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1233">**Edm.Double**</span></span>                   | <span data-ttu-id="5fdd5-1234">Содержит число с плавающей запятой с точностью до 15 цифр</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1234">Contains a floating point number with 15-digit precision</span></span>   | <span data-ttu-id="5fdd5-1235">Precision, Nullable, Default</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1235">Precision, Nullable, Default</span></span>                                             |
| <span data-ttu-id="5fdd5-1236">**EDM. float**</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1236">**Edm.Float**</span></span>                    | <span data-ttu-id="5fdd5-1237">Содержит число с плавающей запятой с точностью до 7 знаков.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1237">Contains a floating point number with 7-digit precision.</span></span>   | <span data-ttu-id="5fdd5-1238">Precision, Nullable, Default</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1238">Precision, Nullable, Default</span></span>                                             |
| <span data-ttu-id="5fdd5-1239">**Edm.Guid**</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1239">**Edm.Guid**</span></span>                     | <span data-ttu-id="5fdd5-1240">Содержит уникальный 16-битный идентификатор.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1240">Contains a 16-byte unique identifier.</span></span>                      | <span data-ttu-id="5fdd5-1241">Precision, Nullable, Default</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1241">Precision, Nullable, Default</span></span>                                             |
| <span data-ttu-id="5fdd5-1242">**Edm.Int16**</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1242">**Edm.Int16**</span></span>                    | <span data-ttu-id="5fdd5-1243">Содержит 16-разрядное целое значение со знаком.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1243">Contains a signed 16-bit integer value.</span></span>                    | <span data-ttu-id="5fdd5-1244">Precision, Nullable, Default</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1244">Precision, Nullable, Default</span></span>                                             |
| <span data-ttu-id="5fdd5-1245">**Edm.Int32**</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1245">**Edm.Int32**</span></span>                    | <span data-ttu-id="5fdd5-1246">Содержит 32-разрядное целое значение со знаком.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1246">Contains a signed 32-bit integer value.</span></span>                    | <span data-ttu-id="5fdd5-1247">Precision, Nullable, Default</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1247">Precision, Nullable, Default</span></span>                                             |
| <span data-ttu-id="5fdd5-1248">**Edm.Int64**</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1248">**Edm.Int64**</span></span>                    | <span data-ttu-id="5fdd5-1249">Содержит 64-разрядное целое значение со знаком.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1249">Contains a signed 64-bit integer value.</span></span>                    | <span data-ttu-id="5fdd5-1250">Precision, Nullable, Default</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1250">Precision, Nullable, Default</span></span>                                             |
| <span data-ttu-id="5fdd5-1251">**Edm.SByte**</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1251">**Edm.SByte**</span></span>                    | <span data-ttu-id="5fdd5-1252">Содержит 8-разрядное целое значение со знаком.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1252">Contains a signed 8-bit integer value.</span></span>                     | <span data-ttu-id="5fdd5-1253">Precision, Nullable, Default</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1253">Precision, Nullable, Default</span></span>                                             |
| <span data-ttu-id="5fdd5-1254">**Edm.String**</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1254">**Edm.String**</span></span>                   | <span data-ttu-id="5fdd5-1255">Содержит символьные данные.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1255">Contains character data.</span></span>                                   | <span data-ttu-id="5fdd5-1256">Unicode, FixedLength, MaxLength, Collation, Precision, Nullable, Default</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1256">Unicode, FixedLength, MaxLength, Collation, Precision, Nullable, Default</span></span> |
| <span data-ttu-id="5fdd5-1257">**Edm.Time**</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1257">**Edm.Time**</span></span>                     | <span data-ttu-id="5fdd5-1258">Содержит время дня.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1258">Contains a time of day.</span></span>                                    | <span data-ttu-id="5fdd5-1259">Precision, Nullable, Default</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1259">Precision, Nullable, Default</span></span>                                             |
| <span data-ttu-id="5fdd5-1260">**Модель EDM. geography**</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1260">**Edm.Geography**</span></span>                |                                                            | <span data-ttu-id="5fdd5-1261">Nullable, по умолчанию, SRID</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1261">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="5fdd5-1262">**Edm.GeographyPoint**</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1262">**Edm.GeographyPoint**</span></span>           |                                                            | <span data-ttu-id="5fdd5-1263">Nullable, по умолчанию, SRID</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1263">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="5fdd5-1264">**EDM. GeographyLineString**</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1264">**Edm.GeographyLineString**</span></span>      |                                                            | <span data-ttu-id="5fdd5-1265">Nullable, по умолчанию, SRID</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1265">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="5fdd5-1266">**EDM. GeographyPolygon**</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1266">**Edm.GeographyPolygon**</span></span>         |                                                            | <span data-ttu-id="5fdd5-1267">Nullable, по умолчанию, SRID</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1267">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="5fdd5-1268">**EDM. GeographyMultiPoint**</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1268">**Edm.GeographyMultiPoint**</span></span>      |                                                            | <span data-ttu-id="5fdd5-1269">Nullable, по умолчанию, SRID</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1269">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="5fdd5-1270">**EDM. GeographyMultiLineString**</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1270">**Edm.GeographyMultiLineString**</span></span> |                                                            | <span data-ttu-id="5fdd5-1271">Nullable, по умолчанию, SRID</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1271">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="5fdd5-1272">**EDM. GeographyMultiPolygon**</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1272">**Edm.GeographyMultiPolygon**</span></span>    |                                                            | <span data-ttu-id="5fdd5-1273">Nullable, по умолчанию, SRID</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1273">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="5fdd5-1274">**EDM. Жеографиколлектион**</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1274">**Edm.GeographyCollection**</span></span>      |                                                            | <span data-ttu-id="5fdd5-1275">Nullable, по умолчанию, SRID</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1275">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="5fdd5-1276">**EDM. Geometry**</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1276">**Edm.Geometry**</span></span>                 |                                                            | <span data-ttu-id="5fdd5-1277">Nullable, по умолчанию, SRID</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1277">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="5fdd5-1278">**EDM. GeometryPoint**</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1278">**Edm.GeometryPoint**</span></span>            |                                                            | <span data-ttu-id="5fdd5-1279">Nullable, по умолчанию, SRID</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1279">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="5fdd5-1280">**EDM. GeometryLineString**</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1280">**Edm.GeometryLineString**</span></span>       |                                                            | <span data-ttu-id="5fdd5-1281">Nullable, по умолчанию, SRID</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1281">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="5fdd5-1282">**EDM. GeometryPolygon**</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1282">**Edm.GeometryPolygon**</span></span>          |                                                            | <span data-ttu-id="5fdd5-1283">Nullable, по умолчанию, SRID</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1283">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="5fdd5-1284">**EDM. GeometryMultiPoint**</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1284">**Edm.GeometryMultiPoint**</span></span>       |                                                            | <span data-ttu-id="5fdd5-1285">Nullable, по умолчанию, SRID</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1285">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="5fdd5-1286">**EDM. GeometryMultiLineString**</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1286">**Edm.GeometryMultiLineString**</span></span>  |                                                            | <span data-ttu-id="5fdd5-1287">Nullable, по умолчанию, SRID</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1287">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="5fdd5-1288">**EDM. GeometryMultiPolygon**</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1288">**Edm.GeometryMultiPolygon**</span></span>     |                                                            | <span data-ttu-id="5fdd5-1289">Nullable, по умолчанию, SRID</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1289">Nullable, Default, SRID</span></span>                                                  |
| <span data-ttu-id="5fdd5-1290">**EDM. GeometryCollection**</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1290">**Edm.GeometryCollection**</span></span>       |                                                            | <span data-ttu-id="5fdd5-1291">Nullable, по умолчанию, SRID</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1291">Nullable, Default, SRID</span></span>                                                  |

## <a name="facets-csdl"></a><span data-ttu-id="5fdd5-1292">Области (CSDL)</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1292">Facets (CSDL)</span></span>

<span data-ttu-id="5fdd5-1293">Аспекты в языке CSDL представляют ограничения для свойств типов сущности и сложных типов.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1293">Facets in conceptual schema definition language (CSDL) represent constraints on properties of entity types and complex types.</span></span> <span data-ttu-id="5fdd5-1294">Аспекты выглядят как XML-атрибуты в следующих элементах CSDL:</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1294">Facets appear as XML attributes on the following CSDL elements:</span></span>

-   <span data-ttu-id="5fdd5-1295">Свойство</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1295">Property</span></span>
-   <span data-ttu-id="5fdd5-1296">TypeRef</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1296">TypeRef</span></span>
-   <span data-ttu-id="5fdd5-1297">Параметр</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1297">Parameter</span></span>

<span data-ttu-id="5fdd5-1298">В следующей таблице описываются аспекты, поддерживаемые в CSDL.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1298">The following table describes the facets that are supported in CSDL.</span></span> <span data-ttu-id="5fdd5-1299">Все аспекты являются необязательными.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1299">All facets are optional.</span></span> <span data-ttu-id="5fdd5-1300">Некоторые аспекты, перечисленные ниже, используются Entity Framework при создании базы данных из концептуальной модели.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1300">Some facets listed below are used by the Entity Framework when generating a database from a conceptual model.</span></span>

> [!NOTE]
> <span data-ttu-id="5fdd5-1301">Сведения о типах данных в концептуальной модели см. в разделе Типы концептуальной модели (CSDL).</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1301">For information about data types in a conceptual model, see Conceptual Model Types (CSDL).</span></span>

| <span data-ttu-id="5fdd5-1302">Аспект</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1302">Facet</span></span>               | <span data-ttu-id="5fdd5-1303">Описание</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1303">Description</span></span>                                                                                                                                                                                                                                                   | <span data-ttu-id="5fdd5-1304">Применяется к</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1304">Applies to</span></span>                                                                                                                                                                                                                                                                                                                                                                           | <span data-ttu-id="5fdd5-1305">Используется для создания базы данных</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1305">Used for the database generation</span></span> | <span data-ttu-id="5fdd5-1306">Используется средой выполнения</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1306">Used by the runtime</span></span> |
|:--------------------|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:---------------------------------|:--------------------|
| <span data-ttu-id="5fdd5-1307">**Параметры сортировки**</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1307">**Collation**</span></span>       | <span data-ttu-id="5fdd5-1308">Задает последовательность сортировки, которая будет использоваться при выполнении операций сравнения и упорядочивания для значений свойств.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1308">Specifies the collating sequence (or sorting sequence) to be used when performing comparison and ordering operations on values of the property.</span></span>                                                                                                               | <span data-ttu-id="5fdd5-1309">**Edm.String**</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1309">**Edm.String**</span></span>                                                                                                                                                                                                                                                                                                                                                                       | <span data-ttu-id="5fdd5-1310">Да</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1310">Yes</span></span>                              | <span data-ttu-id="5fdd5-1311">Нет</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1311">No</span></span>                  |
| <span data-ttu-id="5fdd5-1312">**ConcurrencyMode**</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1312">**ConcurrencyMode**</span></span> | <span data-ttu-id="5fdd5-1313">Указывает, что значение свойства должно использоваться в проверках оптимистического управления параллелизмом.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1313">Indicates that the value of the property should be used for optimistic concurrency checks.</span></span>                                                                                                                                                                    | <span data-ttu-id="5fdd5-1314">Все свойства **EDMSimpleType**</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1314">All **EDMSimpleType** properties</span></span>                                                                                                                                                                                                                                                                                                                                                     | <span data-ttu-id="5fdd5-1315">Нет</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1315">No</span></span>                               | <span data-ttu-id="5fdd5-1316">Да</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1316">Yes</span></span>                 |
| <span data-ttu-id="5fdd5-1317">**Default**</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1317">**Default**</span></span>         | <span data-ttu-id="5fdd5-1318">Задает значение по умолчанию для свойства в случае, если при создании экземпляра не было задано значение.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1318">Specifies the default value of the property if no value is supplied upon instantiation.</span></span>                                                                                                                                                                       | <span data-ttu-id="5fdd5-1319">Все свойства **EDMSimpleType**</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1319">All **EDMSimpleType** properties</span></span>                                                                                                                                                                                                                                                                                                                                                     | <span data-ttu-id="5fdd5-1320">Да</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1320">Yes</span></span>                              | <span data-ttu-id="5fdd5-1321">Да</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1321">Yes</span></span>                 |
| <span data-ttu-id="5fdd5-1322">**FixedLength**</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1322">**FixedLength**</span></span>     | <span data-ttu-id="5fdd5-1323">Указывает, может ли изменяться длина значения свойства.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1323">Specifies whether the length of the property value can vary.</span></span>                                                                                                                                                                                                  | <span data-ttu-id="5fdd5-1324">**EDM. binary**, **EDM. String**</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1324">**Edm.Binary**, **Edm.String**</span></span>                                                                                                                                                                                                                                                                                                                                                       | <span data-ttu-id="5fdd5-1325">Да</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1325">Yes</span></span>                              | <span data-ttu-id="5fdd5-1326">Нет</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1326">No</span></span>                  |
| <span data-ttu-id="5fdd5-1327">**MaxLength**</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1327">**MaxLength**</span></span>       | <span data-ttu-id="5fdd5-1328">Указывает максимальную длину значения свойства.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1328">Specifies the maximum length of the property value.</span></span>                                                                                                                                                                                                           | <span data-ttu-id="5fdd5-1329">**EDM. binary**, **EDM. String**</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1329">**Edm.Binary**, **Edm.String**</span></span>                                                                                                                                                                                                                                                                                                                                                       | <span data-ttu-id="5fdd5-1330">Да</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1330">Yes</span></span>                              | <span data-ttu-id="5fdd5-1331">Нет</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1331">No</span></span>                  |
| <span data-ttu-id="5fdd5-1332">**Допускает значения NULL**</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1332">**Nullable**</span></span>        | <span data-ttu-id="5fdd5-1333">Указывает, может ли свойство иметь значение **null** .</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1333">Specifies whether the property can have a **null** value.</span></span>                                                                                                                                                                                                     | <span data-ttu-id="5fdd5-1334">Все свойства **EDMSimpleType**</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1334">All **EDMSimpleType** properties</span></span>                                                                                                                                                                                                                                                                                                                                                     | <span data-ttu-id="5fdd5-1335">Да</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1335">Yes</span></span>                              | <span data-ttu-id="5fdd5-1336">Да</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1336">Yes</span></span>                 |
| <span data-ttu-id="5fdd5-1337">**Точность**</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1337">**Precision**</span></span>       | <span data-ttu-id="5fdd5-1338">Для свойств типа **Decimal**указывает количество цифр, которое может иметь значение свойства.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1338">For properties of type **Decimal**, specifies the number of digits a property value can have.</span></span> <span data-ttu-id="5fdd5-1339">Для свойств типа **time**, **DateTime**и **DateTimeOffset**указывает количество цифр для дробной части секунды значения свойства.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1339">For properties of type **Time**, **DateTime**, and **DateTimeOffset**, specifies the number of digits for the fractional part of seconds of the property value.</span></span> | <span data-ttu-id="5fdd5-1340">**EDM. DateTime**, **EDM. DateTimeOffset**, **EDM. Decimal**, **модель EDM. Time**</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1340">**Edm.DateTime**, **Edm.DateTimeOffset**, **Edm.Decimal**, **Edm.Time**</span></span>                                                                                                                                                                                                                                                                                                              | <span data-ttu-id="5fdd5-1341">Да</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1341">Yes</span></span>                              | <span data-ttu-id="5fdd5-1342">Нет</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1342">No</span></span>                  |
| <span data-ttu-id="5fdd5-1343">**Масштабирование**</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1343">**Scale**</span></span>           | <span data-ttu-id="5fdd5-1344">Задает число цифр справа от десятичной запятой в значении свойства.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1344">Specifies the number of digits to the right of the decimal point for the property value.</span></span>                                                                                                                                                                      | <span data-ttu-id="5fdd5-1345">**Edm.Decimal**</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1345">**Edm.Decimal**</span></span>                                                                                                                                                                                                                                                                                                                                                                      | <span data-ttu-id="5fdd5-1346">Да</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1346">Yes</span></span>                              | <span data-ttu-id="5fdd5-1347">Нет</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1347">No</span></span>                  |
| <span data-ttu-id="5fdd5-1348">**SRID**</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1348">**SRID**</span></span>            | <span data-ttu-id="5fdd5-1349">Указывает идентификатор системы пространственных ссылок на систему.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1349">Specifies the Spatial System Reference System ID.</span></span> <span data-ttu-id="5fdd5-1350">Дополнительные сведения см. в разделе [SRID](https://en.wikipedia.org/wiki/SRID) and [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1350">For more information, see [SRID](https://en.wikipedia.org/wiki/SRID) and [SRID (SQL Server)](https://msdn.microsoft.com/library/bb964707.aspx).</span></span>                                                              | <span data-ttu-id="5fdd5-1351">**EDM. geography, EDM. GeographyPoint, EDM. GeographyLineString, EDM. GeographyPolygon, EDM. GeographyMultiPoint, EDM. GeographyMultiLineString, EDM. GeographyMultiPolygon, EDM. Жеографиколлектион, EDM. Geometry, EDM. GeometryPoint, EDM. GeometryLineString, EDM. GeometryPolygon, EDM. GeometryMultiPoint, EDM. GeometryMultiLineString, EDM. GeometryMultiPolygon, EDM. GeometryCollection**</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1351">**Edm.Geography, Edm.GeographyPoint, Edm.GeographyLineString, Edm.GeographyPolygon, Edm.GeographyMultiPoint, Edm.GeographyMultiLineString, Edm.GeographyMultiPolygon, Edm.GeographyCollection, Edm.Geometry, Edm.GeometryPoint, Edm.GeometryLineString, Edm.GeometryPolygon, Edm.GeometryMultiPoint, Edm.GeometryMultiLineString, Edm.GeometryMultiPolygon, Edm.GeometryCollection**</span></span> | <span data-ttu-id="5fdd5-1352">Нет</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1352">No</span></span>                               | <span data-ttu-id="5fdd5-1353">Да</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1353">Yes</span></span>                 |
| <span data-ttu-id="5fdd5-1354">**Юникод**</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1354">**Unicode**</span></span>         | <span data-ttu-id="5fdd5-1355">Указывает, будет ли значение свойства храниться в Юникоде.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1355">Indicates whether the property value is stored as Unicode.</span></span>                                                                                                                                                                                                    | <span data-ttu-id="5fdd5-1356">**Edm.String**</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1356">**Edm.String**</span></span>                                                                                                                                                                                                                                                                                                                                                                       | <span data-ttu-id="5fdd5-1357">Да</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1357">Yes</span></span>                              | <span data-ttu-id="5fdd5-1358">Да</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1358">Yes</span></span>                 |

>[!NOTE]
> <span data-ttu-id="5fdd5-1359">При формировании базы данных из концептуальной модели мастер создания базы данных будет распознавать значение атрибута **StoreGeneratedPattern** в элементе **Property** , если оно находится в следующем пространстве имен: https://schemas.microsoft.com/ado/2009/02/edm/annotation .</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1359">When generating a database from a conceptual model, the Generate Database Wizard will recognize the value of the **StoreGeneratedPattern** attribute on a **Property** element if it is in the following namespace: https://schemas.microsoft.com/ado/2009/02/edm/annotation.</span></span> <span data-ttu-id="5fdd5-1360">Для атрибута поддерживаются значения **Identity** и **COMPUTE**.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1360">The supported values for the attribute are **Identity** and **Computed**.</span></span> <span data-ttu-id="5fdd5-1361">Значение **Identity** приведет к созданию столбца базы данных со значением идентификатора, созданным в базе данных.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1361">A value of **Identity** will produce a database column with an identity value that is generated in the database.</span></span> <span data-ttu-id="5fdd5-1362">Значение **вычисляемое** приведет к созданию столбца со значением, вычисляемым в базе данных.</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1362">A value of **Computed** will produce a column with a value that is computed in the database.</span></span>

### <a name="example"></a><span data-ttu-id="5fdd5-1363">Пример</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1363">Example</span></span>

<span data-ttu-id="5fdd5-1364">В следующем примере показывается применение аспектов к свойствам типа сущности:</span><span class="sxs-lookup"><span data-stu-id="5fdd5-1364">The following example shows facets applied to the properties of an entity type:</span></span>

``` xml
 <EntityType Name="Product">
   <Key>
     <PropertyRef Name="ProductId" />
   </Key>
   <Property Type="Int32"
             Name="ProductId" Nullable="false"
             a:StoreGeneratedPattern="Identity"
    xmlns:a="https://schemas.microsoft.com/ado/2009/02/edm/annotation" />
   <Property Type="String"
             Name="ProductName"
             Nullable="false"
             MaxLength="50" />
   <Property Type="String"
             Name="Location"
             Nullable="true"
             MaxLength="25" />
 </EntityType>
```
